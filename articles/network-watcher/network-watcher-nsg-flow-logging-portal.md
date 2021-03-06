---
title: "使用 Azure 網路監看員管理網路安全性群組流量記錄 | Microsoft Docs"
description: "此頁面說明如何在 Azure 網路監看員中管理網路安全性群組流量記錄"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: d716f11818bc0ad6dd9e5f93951b011dd6774c7b
ms.lasthandoff: 03/22/2017


---

# <a name="manage-network-security-group-flow-logs-in-the-azure-portal"></a>在 Azure 入口網站管理網路安全性群組流程

> [!div class="op_single_selector"]
> - [Azure 入口網站](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

網路安全性群組流量記錄是網路監看員的一項功能，可讓您檢視透過網路安全性群組傳輸之輸入和輸出 IP 流量的相關資訊。 這些流量記錄是以 json 格式撰寫，會顯示每一規則的輸出和輸入流量、流量套用至的 NIC、有關流量的 5 個 Tuple 資訊 (來源/目的地 IP、來源/目的地連接埠、通訊協定)，以及流量是被允許或拒絕。

## <a name="before-you-begin"></a>開始之前

此案例假設您已依照[建立網路監看員](network-watcher-create.md)中的步驟建立網路監看員。 此案例也假設已有具有有效虛擬機器的資源群組可供使用。

## <a name="enable-flow-logs"></a>啟用流程記錄

這些步驟會引導您啟用網路安全性群組上的流量記錄檔。

### <a name="step-1"></a>步驟 1

瀏覽至網路監看員執行個體，然後選取 [傳送記錄檔]

![流量記錄概觀][1]

### <a name="step-2"></a>步驟 2

按一下從清單中選取網路安全性群組。

![流量記錄概觀][2]

### <a name="step-3"></a>步驟 3 

在 [流程記錄設定] 刀鋒視窗中，將狀態設為 [開啟] 並設定儲存體帳戶。  完成時，按一下 [確定] 和 [儲存]

![流量記錄概觀][3]

## <a name="download-flow-logs"></a>下載流量記錄

流程記錄會儲存在儲存體帳戶中。 若要檢視流程記錄，您需要下載它們。

### <a name="step-1"></a>步驟 1

若要下載流程記錄，請按一下 [您可從設定的儲存體帳戶下載流程記錄檔]。  這樣會引導您至儲存體帳戶檢視，您可以在其中瀏覽至您要下載的記錄檔。

![流程記錄設定][4]

### <a name="step-2"></a>步驟 2

依序瀏覽至正確的儲存體帳戶、[容器] > **insights-log-networksecuritygroupflowevent**

![流程記錄設定][5]

### <a name="step-3"></a>步驟 3

向下切入到流程記錄的位置、選取流程記錄，然後按一下 [下載]

![流程記錄設定][6]

如需記錄結構的相關資訊，請造訪[網路安全性群組流量記錄概觀](network-watcher-nsg-flow-logging-overview.md)

## <a name="next-steps"></a>後續步驟

了解如何[使用 PowerBI 視覺化 NSG 流量記錄](network-watcher-visualize-nsg-flow-logs-power-bi.md)

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png

