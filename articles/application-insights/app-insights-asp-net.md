---
title: "使用 Azure Application Insights 設定 ASP.NET 的 Web 應用程式分析 | Microsoft Docs"
description: "針對裝載在內部部署環境或 Azure 的 ASP.NET 網站設定效能、可用性及使用情況分析。"
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/04/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 04bac05363f0f7468e41ccfd96a85e4bd7bf761c
ms.lasthandoff: 04/06/2017


---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>設定 ASP.NET 網站的 Application Insights

此程序會設定 ASP.NET web 應用程式以將遙測傳送至 [Azure Application Insights](app-insights-overview.md) 服務。 這適用於在您擁有的 IIS 伺服器或雲端中託管的 ASP.NET 應用程式。 取得圖表和功能強大的查詢語言可幫助您了解您應用程式的效能以及人員如何使用它，再加上發生失敗或效能問題時的自動警示。 許多開發人員發現這些功能本身就很棒，但是您也可以視需要擴充和自訂遙測。

在 Visual Studio 中只需按幾下滑鼠即可進行安裝。 您可以選擇限制遙測的磁碟區來避免產生費用。 這可讓您實驗和偵錯，或使用不多的使用者監視網站。 當您決定要繼續監視您的生產網站時，很容易在稍後提升限制。

## <a name="before-you-start"></a>開始之前
您需要：

* Visual Studio 2013 Update 3 或更新版本。 越新版越好。
* [Microsoft Azure](http://azure.com)訂用帳戶。 如果您的小組或組織擁有 Azure 訂用帳戶，擁有者就可以使用您的 [Microsoft 帳戶](http://live.com)將您加入。

如果您對下列內容感興趣，請查看其他主題︰

* [在執行階段檢測 Web 應用程式](app-insights-monitor-performance-live-website-now.md)
* [Azure 雲端服務](app-insights-cloudservices.md)

## <a name="ide"></a>步驟 1：新增 Application Insights SDK

在 [方案總管] 中以滑鼠右鍵按一下 Web 應用程式，然後選擇 [新增] > [Application Insights 遙測] 或 [設定 Application Insights]。

![[方案總管] 的螢幕擷取畫面，其中已醒目提示 [新增 Application Insights 遙測]](./media/app-insights-asp-net/appinsights-03-addExisting.png)

(在 Visual Studio 2015 中，[新增專案] 對話方塊中也有選項可新增 Application Insights。)

繼續移至 Application Insights 設定頁面︰

![[向 Application Insights 註冊您的應用程式] 頁面的螢幕擷取畫面](./media/app-insights-asp-net/visual-studio-register-dialog.png)

**a.** 選取您用來存取 Azure 的帳戶和訂用帳戶。

**b.** 在 Azure 中選取資源，以查看來自應用程式的資料。 通常您會為每個應用程式建立個別的資源。 如果您想要設定資源群組，或資料的儲存位置，請按一下 [進行設定]。 資源群組用來控制資料的存取。 例如，如果您有數個應用程式組成相同系統時，您可能會將其 Application Insights 資料放在相同的資源群組中。

**c.** 設定可用資料磁碟區限制的上限，以避免產生費用。 在一定的遙測數量之內，Application Insights 免費。 建立資源之後，您可以在入口網站中開啟 [功能 + 定價] > [資料量管理] > [每日用量上限]，以變更您的選擇。

**d.** 按一下 [註冊]，繼續設定 Web 應用程式的 Application Insights。 在偵錯期間和您發佈應用程式之後，遙測會傳送至 [Azure 入口網站](https://portal.azure.com)。

**e.** 如果您不想在您偵錯時將遙測傳送到入口網站，而只將 Application Insights SDK 新增至您的應用程式，但不在入口網站中設定資源。 您在 Visual Studio 中偵錯時可以看到遙測。 稍後，您可以回到此設定頁面，或等到部署您的應用程式之後，[在執行階段開啟遙測](app-insights-monitor-performance-live-website-now.md)。


## <a name="run"></a> 步驟 2︰執行您的應用程式
按 F5 執行您的應用程式。 開啟不同的頁面來產生一些遙測。

在 Visual Studio 中，您可以看見已記錄的事件計數。

![Visual Studio 的螢幕擷取畫面。 偵錯期間會顯示 Application Insights 按鈕。](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry"></a>步驟 3：查看遙測
您可以在 Visual Studio 或 Application Insights Web 入口網站中看到遙測。 在 Visual Studio 中搜尋遙測可協助您偵錯應用程式。 當系統作用中時監視效能及 web 入口網站的使用量。 

### <a name="see-your-telemetry-in-visual-studio"></a>在 Visual Studio 中查看遙測

在 Visual Studio 中，開啟 Application Insights 視窗。 按一下 [Application Insights] 按鈕，或以滑鼠右鍵按一下 [方案總管] 中的專案﹐選取 [Application Insights]﹐然後按一下 [搜尋即時遙測]。

在 Visual Studio 的 [Application Insights 搜尋] 視窗中，利用 [偵錯工作階段中的資料] 檢視，查看應用程式的伺服器端所產生的遙測。 試驗篩選器，然後按一下任何事件以查看更多詳細資料。

![[Application Insights] 視窗中 [偵錯工作階段中的資料] 檢視的螢幕擷取畫面。](./media/app-insights-asp-net/55.png)

> [!NOTE]
> 如果您沒有看到任何資料，請確定時間範圍正確，然後按一下 [搜尋] 圖示。

[深入了解 Visual Studio 中的 Application Insights 工具](app-insights-visual-studio.md)。

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a>請參閱 web 入口網站中的遙測

您也可以在 Application Insights Web 入口網站中看到遙測 (除非您選擇只安裝 SDK)。 此入口網站中的圖表、分析工具和跨元件檢視比 Visual Studio 還多。 此入口網站也提供警示。

開啟 Application Insights 資源。 登入 [Azure 入口網站](https://portal.azure.com/)並找到它，或以滑鼠右鍵按一下 Visual Studio 中的專案並讓它帶您前往。

![Visual Studio 的螢幕擷取畫面，顯示如何開啟 Application Insights 入口網站](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> 如果您收到存取錯誤：您是否有一組以上的 Microsoft 認證，且是否以錯誤的組合登入？ 在入口網站中，登出後重新登入。

入口網站會從您的應用程式開啟遙測檢視。

![Application Insights 概觀頁面的螢幕擷取畫面](./media/app-insights-asp-net/66.png)

在入口網站中，按一下任何圖格或圖表以查看詳細資料。

[深入了解在 Azure 入口網站中使用 Application Insights](app-insights-dashboards.md)。

## <a name="step-4-publish-your-app"></a>步驟 4：發佈您的應用程式
將您的應用程式發佈至 IIS 伺服器或 Azure。 監看 [即時計量串流](app-insights-metrics-explorer.md#live-metrics-stream) 以確定一切順利執行。

您的遙測會累積在 Application Insights 入口網站，您可以在此監視計量，搜尋您的遙測，以及設定[儀表板](app-insights-dashboards.md)。 您也可以使用功能強大的[分析查詢語言](app-insights-analytics.md)，分析使用狀況和效能或尋找特定事件。

您也可以繼續在 [Visual Studio](app-insights-visual-studio.md) 中，以診斷搜尋和[趨勢](app-insights-visual-studio-trends.md)等工具來分析您的遙測。

> [!NOTE]
> 如果應用程式傳送足夠的遙測資料達到[節流限制](app-insights-pricing.md#limits-summary)，則會切換為自動[取樣](app-insights-sampling.md)。 取樣可減少從應用程式傳送的遙測數量，同時為供診斷之用保留相互關聯的資料。
>
>

## <a name="land"></a> 您全都準備好了

恭喜！ 您在應用程式中安裝了 Application Insights 套件，並將其設定為將遙測傳送至 Azure 上的 Application Insights 服務。

![遙測移動的圖表](./media/app-insights-asp-net/01-scheme.png)

接收您應用程式遙測的 Azure 資源會由檢測金鑰識別。 您可以在 ApplicationInsights.config 檔案中找到此金鑰。


## <a name="upgrade-to-future-sdk-versions"></a>升級至未來的 SDK 版本
若要升級至[新版的 SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases)，請再次開啟 **NuGet 套件管理員**，並篩選出已安裝的套件。 選取 **Microsoft.ApplicationInsights.Web**，然後選擇 [升級]。

如果您已對 ApplicationInsights.config 進行任何的自訂，請在升級前儲存複本。 然後，將您的變更合併至新版本中。

## <a name="video"></a>影片

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>後續步驟

### <a name="more-telemetry"></a>更多遙測

* **[瀏覽器和頁面載入資料](app-insights-javascript.md)** - 在您的網頁中插入程式碼片段。
* **[取得更詳細的相依性和例外狀況監視](app-insights-monitor-performance-live-website-now.md)** - 在您的伺服器上安裝狀態監視器。
* **[撰寫自訂事件](app-insights-api-custom-events-metrics.md)**以計數、計時或測量使用者動作。
* **[取得記錄資料](app-insights-asp-net-trace-logs.md)** - 將記錄資料與您的遙測相互關聯。

### <a name="analysis"></a>分析

* **[在 Visual Studio 中使用 Application Insights](app-insights-visual-studio.md)**<br/>包括使用遙測來偵錯、診斷搜尋及鑽研程式碼的相關資訊。
* **[使用 Application Insights 入口網站](app-insights-dashboards.md)**<br/> 包括儀表板、功能強大的診斷和分析工具、警示、即時的應用程式相依性對應，以及遙測匯出的相關資訊。
* **[分析 - ](app-insights-analytics-tour.md)** - 功能強大的查詢語言。

### <a name="alerts"></a>Alerts

* [可用性測試](app-insights-monitor-web-app-availability.md)：建立測試，以確保網路上看得見您的網站。
* [智慧型診斷](app-insights-proactive-diagnostics.md)︰這些測試會自動執行，您不需要採取任何動作來設定它們。 它們會讓您知道應用程式是否有不尋常的失敗要求率。
* [計量警示](app-insights-alerts.md)：設定這些警示，當計量超出臨界值時警告您。 您可以在撰寫於程式碼中的自訂度量上設定它們。

### <a name="automation"></a>自動化

* [自動建立 Application Insights 資源](app-insights-powershell.md)

