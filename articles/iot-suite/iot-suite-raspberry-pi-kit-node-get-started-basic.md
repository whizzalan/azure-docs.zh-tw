---
title: "使用 Node.js 搭配真實感應器將 Raspberry Pi 連線到 Azure IoT 套件來支援韌體更新 | Microsoft Docs"
description: "使用 Raspberry Pi 3 和 Azure IoT 套件的 Microsoft Azure IoT 入門套件。 使用 Node.js 將 Raspberry Pi 連線到遠端監視解決方案、將遙測從感應器傳送到雲端，並回應解決方案儀表板所叫用的方法。"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/24/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 6d8e6ef070c3b9d9623f4b29ab97a5d20f69d499
ms.contentlocale: zh-tw
ms.lasthandoff: 05/03/2017


---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-nodejs"></a>將 Raspberry Pi 3 連線到遠端監視解決方案，並且使用 Node.js 從真實感應器傳送遙測

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

本教學課程會示範如何使用 Raspberry Pi 3 的 Microsoft Azure IoT 入門套件來開發可與雲端通訊的溫度與溼度讀取器。 教學課程會使用：

- Raspbian OS、Node.js 程式設計語言和 Microsoft Azure IoT SDK for Node.js 來實作範例裝置。
- IoT 套件遠端監視預先設定解決方案作為以雲端為基礎的後端。

## <a name="overview"></a>概觀

在本教學課程中，您會完成下列步驟：

- 將遠端監視預先設定解決方案的執行個體部署至您的 Azure 訂用帳戶。 這個步驟會自動部署並設定多個 Azure 服務。
- 設定您的裝置和感應器，以便與您的電腦和遠端監視解決方案進行通訊。
- 更新範例裝置程式碼以連線到遠端監視解決方案，並傳送您可以在解決方案儀表板上檢視的遙測。

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> 遠端監視解決方案會佈建一組 Azure 訂用帳戶中的 Azure 服務。 部署會反映實際的企業架構。 若要避免不必要的 Azure 耗用量費用，當您使用完 azureiotsuite.com 中預先設定解決方案的執行個體時，請將它刪除。 如果您還需要預先設定的解決方案，可以輕鬆地將它重新建立。 如需將遠端監視解決方案執行時的耗用量降低的詳細資訊，請參閱[設定 Azure IoT 套件預先設定的解決方案以供示範][lnk-demo-config]。

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a>下載並設定範例

您現在可以在 Raspberry Pi 上將遠端監視用戶端應用程式進行下載及設定。

### <a name="install-nodejs"></a>安裝 Node.js

在 Raspberry Pi 上安裝 Node.js。 適用於 Node.js 的 IoT SDK 需要 Node.js 0.11.5 版或更新版本。 下列步驟說明如何在 Raspberry Pi 上安裝 Node.js 6.10.2 版：

1. 若要更新 Raspberry Pi，請使用下列命令︰

    `sudo apt-get update`

1. 若要將 Node.js 二進位檔下載至 Raspberry Pi，請使用下列命令︰

    `wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz`

1. 使用下列命令來安裝二進位檔：

    `sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz`

1. 若要確認您已成功安裝 Node.js 6.10.2 版，請使用下列命令︰

    `node --version`

### <a name="clone-the-repositories"></a>複製存放庫

如果您尚未這麼做，請在 Pi 上執行下列命令來複製所需的存放庫︰

`cd ~`

`git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git`

### <a name="update-the-device-connection-string"></a>更新裝置連接字串

使用下列命令，將 **nano** 編輯器中的範例來源檔案開啟︰

`nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js`

請尋找以下這行︰

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

將預留位置值取代為本教學課程開頭所建立及儲存的裝置和 IoT 中樞。 儲存您的變更 (**Ctrl-O**、**Enter**) 並結束編輯器 (**Ctrl-X**)。

## <a name="run-the-sample"></a>執行範例

執行下列命令來安裝範例的必要條件套件︰

`cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic`

`npm install`

您現在可以在 Raspberry Pi 上執行範例程式。 輸入命令：

`sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js`

下列範例輸出是您在 Raspberry Pi 的命令提示字元所看到的輸出範例︰

![Raspberry Pi 應用程式的輸出][img-raspberry-output]

按下 **CTRL-C** 可隨時結束程式。

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a>後續步驟

請瀏覽 [Azure IoT 開發人員中心](https://azure.microsoft.com/develop/iot/)，取得更多 Azure IoT 的相關範例和文件。


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-basic/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

