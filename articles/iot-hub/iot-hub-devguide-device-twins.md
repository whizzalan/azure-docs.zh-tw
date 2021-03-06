---
title: "了解 Azure IoT 中樞裝置對應項 | Microsoft Docs"
description: "開發人員指南 - 使用裝置對應項同步處理 IoT 中樞與裝置之間的狀態和組態資料"
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 8a3da072-a5bf-46e5-8de4-24cdbb2a03fa
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: e72fcd696a4f21aa4b2cff7ae7178dbc372f1929
ms.lasthandoff: 03/10/2017


---
# <a name="understand-and-use-device-twins-in-iot-hub"></a>了解和使用 Azure IoT 中樞的裝置對應項
## <a name="overview"></a>概觀
「裝置對應項」是存放裝置狀態資訊 (中繼資料、組態和條件) 的 JSON 文件。 IoT 中樞會為連線到 IoT 中樞的每個裝置保存裝置對應項。 本文章說明：

* 裝置對應項的結構︰標籤、所需屬性和報告屬性，以及
* 裝置應用程式和後端可以對裝置對應項執行的作業。

> [!NOTE]
> 目前，裝置對應項只能從使用 MQTT 通訊協定連線至 IoT 中樞的裝置存取。 請參閱 [MQTT 支援][lnk-devguide-mqtt]文章，以取得如何將現有裝置應用程式轉換為使用 MQTT 的指示。
> 
> 

### <a name="when-to-use"></a>使用時機
使用裝置對應項可以：

* 在雲端儲存裝置特定的中繼資料。 例如，販賣機的部署位置。
* 從您的裝置應用程式報告目前狀態資訊，例如可用功能和狀況。 例如，裝置會透過行動電話或 WiFi 連線到您的 IoT 中樞。
* 同步處理裝置與後端應用程式之間長時間執行之工作流程的狀態。 例如，當解決方案後端指定要安裝的新韌體版本時，以及裝置應用程式報告更新處理程序的各個階段時。
* 查詢裝置中繼資料、組態或狀態。

請參閱[裝置到雲端通訊指引][lnk-d2c-guidance]，以取得有關使用報告屬性、裝置到雲端訊息或檔案上傳的指引。
請參閱[雲端對裝置通訊指引][lnk-c2d-guidance]，以取得有關使用所需屬性、直接方法或雲端到裝置訊息的指引。

## <a name="device-twins"></a>裝置對應項
裝置對應項會儲存裝置相關資訊，以供︰

* 裝置和後端用來同步處理裝置的狀況和組態。
* 解決方案後端用來查詢和鎖定長時間執行的作業。

裝置對應項的生命週期會連結至對應的[裝置身分識別][lnk-identity]。 在 IoT 中樞建立或刪除新的裝置身分識別時，會隱含地建立和刪除裝置對應項。

裝置的兩個是 JSON 文件，其中含有︰

* **標籤**。 解決方案後端可以讀取及寫入的 JSON 文件區段。 裝置應用程式看不到標籤。
* **所需屬性**。 搭配報告屬性使用，以便同步處理裝置的組態或狀況。 所需屬性只能由解決方案後端設定，且可供裝置應用程式讀取。 所需屬性有所變更時，裝置應用程式也可以即時收到通知。
* **報告屬性**。 搭配所需屬性使用，以便同步處理裝置的組態或狀況。 報告屬性只能由裝置應用程式設定，且可供解決方案後端讀取和查詢。

此外，裝置對應項 JSON 文件的根目錄包含來自[身分識別登錄][lnk-identity]中儲存之對應身分識別的唯讀屬性。

![][img-twin]

以下範例顯示裝置對應項 JSON 文件︰

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

在根物件中，都是系統屬性，並且是 `tags` 以及 `reported` 和 `desired` 屬性的容器物件。 `properties` 容器包含一些唯讀元素 (`$metadata`、`$etag` 和 `$version`)，其說明位於[裝置對應項中繼資料][lnk-twin-metadata]和[開放式並行存取][lnk-concurrency]章節。

### <a name="reported-property-example"></a>報告屬性範例
在上述範例中，裝置對應項包含由裝置應用程式所報告的 `batteryLevel` 屬性。 此屬性可讓您根據最後一次報告的電池電量對裝置進行查詢和操作。 其他範例包含裝置應用程式報告功能或連線能力選項。

> [!NOTE]
> 當解決方案後端想要取得屬性的最後已知值時，報告屬性如何簡化這種情況。 如果解決方案後端需要以一連串時間戳記事件 (例如時間序列) 的形式處理裝置遙測，請使用[裝置到雲端訊息][lnk-d2c]。

### <a name="desired-property-example"></a>所需屬性範例
在上述範例中，解決方案後端和裝置應用程式會使用 `telemetryConfig` 裝置對應項的所需屬性和報告屬性，以同步處理此裝置的遙測組態。 例如：

1. 解決方案後端會以所需組態值來設定所需屬性。 以下是含有所需屬性集的文件部分︰
   
        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
2. 裝置應用程式會在已連線或第一次重新連線時，立即收到變更通知。 裝置應用程式接著會報告更新的組態 (或使用 `status` 屬性的錯誤狀況)。 以下是報告屬性的部分︰
   
        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...
3. 解決方案後端可以[查詢][lnk-query]裝置對應項，以追蹤組態作業在許多裝置上的結果。

> [!NOTE]
> 上述程式碼片段舉例說明了用來編碼裝置組態和其狀態的方式，為了方便閱讀，其內容已經過最佳化。 IoT 中樞不會對裝置對應項中的裝置對應項所需屬性和報告屬性強制實行特定結構描述。
> 
> 

您可以使用裝置對應項來同步處理長時間執行的作業，例如韌體更新。 如需如何使用屬性來跨裝置同步處理及追蹤長時間執行的作業，請參閱[使用所需屬性來設定裝置][lnk-twin-properties]。

## <a name="back-end-operations"></a>後端作業
解決方案後端會使用下列不可部分完成的作業 (透過 HTTP 公開) 來操作裝置對應項︰

1. **依識別碼擷取裝置對應項**。 此作業會傳回裝置對應項文件，包括標籤以及所需屬性、報告屬性和系統屬性。
2. **部份更新裝置對應項**。 此作業可讓解決方案後端局部地更新裝置對應項中的標籤或所需屬性。 部分更新會以 JSON 文件的形式來表示，以新增或更新任何屬性。 設定為 `null` 的屬性會遭到移除。 下列範例會以 `{"newProperty": "newValue"}` 值建立新的所需屬性、以 `"otherNewValue"` 覆寫 `existingProperty` 的現有值，並移除 `otherOldProperty`。 不會對現有的所需屬性或標籤進行任何變更︰
   
        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }
3. **取代所需屬性**。 此作業可讓解決方案後端完全覆寫所有現有的所需屬性，並以新的 JSON 文件取代 `properties/desired`。
4. **取代標籤**。 此作業可讓解決方案後端完全覆寫所有現有的標籤，並以新的 JSON 文件取代 `tags`。

上述所有作業皆支援[開放式並行存取][lnk-concurrency]，而且需要 **ServiceConnect** 權限，如[安全性][lnk-security]一文所定義。

除了這些作業，解決方案後端還可以︰

* 使用類似 SQL 的 [IoT 中樞查詢語言][lnk-query]來查詢裝置對應項。
* 使用[作業][lnk-jobs]在大型裝置對應項集合上執行作業。

## <a name="device-operations"></a>裝置作業
裝置應用程式會使用下列不可部分完成的作業來操作裝置對應項︰

1. **擷取裝置對應項**。 此作業會針對目前連線的裝置傳回裝置對應項文件 (包括標籤以及所需屬性、報告屬性和系統屬性)。
2. **部分更新的報告屬性**。 此作業可針對目前連線裝置的報告屬性進行部分更新。 此作業使用的 JSON 更新格式，與解決方案後端用於局部更新所需屬性的格式相同。
3. **觀察所需屬性**。 目前連線的裝置可以選擇在所需屬性進行更新時收到通知。 裝置會收到由解決方案後端執行的相同更新形式 (部分或完整取代)。

上述所有作業都需要 **DeviceConnect** 權限，如[安全性][lnk-security]一文所定義。

[Azure IoT 裝置 SDK][lnk-sdks] 可讓您透過許多語言和平台輕鬆使用上述作業。 如需 IoT 中樞之所需屬性同步處理基元的詳細資訊，請參閱[裝置重新連線流程][lnk-reconnection]。

> [!NOTE]
> 目前，裝置對應項只能從使用 MQTT 通訊協定連線至 IoT 中樞的裝置存取。
> 
> 

## <a name="reference-topics"></a>參考主題：
下列參考主題會提供您關於控制 IoT 中樞存取權的詳細資訊。

## <a name="tags-and-properties-format"></a>標籤和屬性格式
標籤、所需屬性和報告屬性是具有下列限制的 JSON 物件︰

* JSON 物件中的所有索引鍵是區分大小寫的 64 個位元組的 UTF-8 UNICODE 字串。 允許的字元會排除 UNICODE 控制字元 (區段 C0 和 C1)，以及 `'.'`、`' '` 和 `'$'`。
* JSON 物件中的所有值可以屬於下列 JSON 類型︰布林值、數字、字串、物件。 不允許使用陣列。
* 標籤、所需屬性和報告屬性中所有 JSON 物件的深度上限為 5。 例如，下列物件有效：

        {
            ...
            "tags": {
                "one": {
                    "two": {
                        "three": {
                            "four": {
                                "five": {
                                    "property": "value"
                                }
                            }
                        }
                    }
                }
            },
            ...
        }

* 所有字串值的最大長度為 512 個位元組。

## <a name="device-twin-size"></a>裝置對應項大小
IoT 中樞會對 `tags`、`properties/desired` 和 `properties/reported` 的值 (排除唯讀元素) 強制執行 8 KB 大小的限制。
大小的計算方式是計算所有字元的數量，並排除出現在字串常數之外的 UNICODE 控制字元 (區段 C0 和 C1) 和空格 `' '`。
IoT 中樞會拒絕 (並出現錯誤) 將會讓這些文件的大小增加到超過限制的所有作業。

## <a name="device-twin-metadata"></a>裝置對應項中繼資料
IoT 中樞會為裝置對應項所需屬性和報告屬性的每個 JSON 物件保有上次更新的時間戳記。 時間戳記採用 UTC 格式，並以 [ISO8601] 格式 `YYYY-MM-DDTHH:MM:SS.mmmZ` 進行編碼。
例如：

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

此資訊會保留在每個層級 (而不只是 JSON 結構的分葉)，以保留可移除物件索引鍵的更新。

## <a name="optimistic-concurrency"></a>開放式並行存取
標籤、所需屬性和報告屬性全都支援開放式並行存取。
依據 [RFC7232]，標籤會有一個 ETag 來表示該標籤的 JSON 表示法。 您可以從解決方案後端在條件式更新作業中使用 ETag，以確保一致性。

裝置對應項所需屬性和報告屬性沒有 ETag，但是有一定會遞增的 `$version` 值。 類似於 ETag，更新端可以使用版本強制達到更新的一致性。 例如，報告屬性的裝置應用程式或所需屬性的解決方案後端。

當觀察端代理程式 (例如，觀察所需屬性的裝置應用程式) 必須協調擷取作業結果與更新通知之間的競爭情況時，版本也相當有用。 [裝置重新連線流程][lnk-reconnection]一節會提供詳細資訊。

## <a name="device-reconnection-flow"></a>裝置重新連線流程
IoT 中樞不會保留已中斷連線之裝置的所需屬性更新通知。 因此，連線的裝置必須擷取完整的所需屬性文件，並訂閱更新通知。 由於更新通知和完整擷取之間可能會有競爭情況，因此必須確保下列流程︰

1. 裝置應用程式連線到 IoT 中樞。
2. 裝置應用程式訂閱所需屬性更新通知。
3. 裝置應用程式擷取所需屬性的完整文件。

裝置應用程式可以忽略 `$version` 小於或等於完整擷取文件版本的所有通知。 IoT 中樞保證版本一定會遞增，因此這是可行的方法。

> [!NOTE]
> [Azure IoT 裝置 SDK][lnk-sdks] 中已實作此邏輯。 只有當裝置應用程式無法使用任何 Azure IoT 裝置 SDK，因而必須直接為 MQTT 介面編寫程式時，此說明才有用。
> 
> 

## <a name="additional-reference-material"></a>其他參考資料
IoT 中樞開發人員指南中的其他參考主題包括︰

* [IoT 中樞端點][lnk-endpoints]一文說明每個 IoT 中樞針對執行階段和管理作業所公開的各種端點。
* [節流和配額][lnk-quotas]一文說明適用於 IoT 中樞服務的配額，和使用服務時所預期的節流行為。
* [Azure IoT 裝置和服務 SDK][lnk-sdks] 一文列出各種語言 SDK，可供您在開發與「IoT 中樞」互動的裝置和服務應用程式時使用。
* [裝置對應項和作業的 IoT 中樞查詢語言][lnk-query]一文說明的 IoT 中樞查詢語言可用來從 IoT 中樞擷取有關裝置對應項和作業的資訊。
* [IoT 中樞 MQTT 支援][lnk-devguide-mqtt]一文針對 MQTT 通訊協定提供 IoT 中樞支援的詳細資訊。

## <a name="next-steps"></a>後續步驟
現在您已了解裝置對應項，接下來您可能會對下列 IoT 中樞開發人員指南主題感興趣︰

* [在裝置上叫用直接方法][lnk-methods]
* [排程多個裝置上的作業][lnk-jobs]

如果您想要嘗試本文章所述的概念，您可能會對下列 IoT 中樞教學課程感興趣：

* [如何使用裝置對應項][lnk-twin-tutorial]
* [如何使用裝置對應項屬性][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#device-twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png

