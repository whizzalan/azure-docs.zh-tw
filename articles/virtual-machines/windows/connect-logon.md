---
title: "連接到 Windows Server VM | Microsoft Docs"
description: "了解如何使用 Azure 入口網站和 Resource Manager 部署模型來連接和登入 Windows VM。"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/01/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 88431377a36d5bc36220c630f0c8d4a46ab4a434
ms.lasthandoff: 03/31/2017


---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a>如何連接和登入執行 Windows 的 Azure 虛擬機器
您會使用 Azure 入口網站中的 [連線] 按鈕，從 Windows 桌面啟動遠端桌面 (RDP) 工作階段。 首先您要連接至虛擬機器，然後登入。

如果您嘗試從 Mac 連線至 Windows VM，您需要安裝適用於 Mac 的 RDP 用戶端，例如 [Microsoft 遠端桌面](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417)。

## <a name="connect-to-the-virtual-machine"></a>連接至虛擬機器
1. 如果您尚未登入 [Azure 入口網站](https://portal.azure.com/)，請先登入。
2. 在 [中樞] 功能表上，按一下 [虛擬機器] 。
3. 然後從清單中選取虛擬機器。
4. 在虛擬機器的刀鋒視窗中，按一下 [ **連線**]。
   
    ![顯示如何連接至 VM 的 Azure 入口網站螢幕擷取畫面。](./media/connect-logon/connect.png)
   
   > [!TIP]
   > 如果入口網站中的 [連接] 按鈕呈現灰色，而且您未透過 [Express Route](../../expressroute/expressroute-introduction.md) 或[網站間 VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) 連接來連接到 Azure，您就必須先建立 VM 並為其指派公用 IP 位址，才能使用 RDP。 您可以深入了解 [Azure 中的公用 IP 位址](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)。
   > 
   > 

## <a name="log-on-to-the-virtual-machine"></a>登入虛擬機器
[!INCLUDE [virtual-machines-log-on-win-server](../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>後續步驟
如果嘗試連線時遇到問題，請參閱 [遠端桌面連線進行疑難排解](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。 本文會逐步帶領您診斷及解決常見的問題。


