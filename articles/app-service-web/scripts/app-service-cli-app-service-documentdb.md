---
title: "Azure CLI 指令碼範例 - 將 Web 應用程式連線至 documentdb | Microsoft Docs"
description: "Azure CLI 指令碼範例 - 將 Web 應用程式連線至 documentdb"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bbbdbc42-efb5-4b4f-8ba6-c03c9d16a7ea
ms.service: app-service
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 26d460a699e31f6c19e3b282fa589ed07ce4a068
ms.openlocfilehash: 6335fae28c1198b9276ece651b8d90c82a209905
ms.lasthandoff: 04/04/2017

---

# <a name="connect-a-web-app-to-a-documentdb"></a>將 Web 應用程式連線至 documentdb

在此案例中，您將了解如何建立 Azure documentdb 和 Azure Web 應用程式。 然後，您會使用應用程式設定將 documentdb 連結至 Web 應用程式。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>範例指令碼

[!code-azurecli[主要](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure DocumentDB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>指令碼說明

此指令碼使用下列命令來建立資源群組、Web 應用程式、documentdb 和所有相關資源。 下表中的每個命令都會連結至命令特定的文件。

| 命令 | 注意事項 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | 建立用來存放所有資源的資源群組。 |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | 建立 App Service 方案。 這就像是 Azure Web 應用程式的伺服器陣列。 |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#create) | 在 App Service 方案內建立 Azure Web 應用程式。 |
| [az documentdb create](https://docs.microsoft.com/en-us/cli/azure/documentdb#create) | 建立 documentdb。 這是儲存資料的位置。 |
| [az documentdb list-keys](https://docs.microsoft.com/en-us/cli/azure/documentdb#list-keys) | 列出指定的 Azure DocumentDB 資料庫帳戶存取金鑰。 |
| [az appservice web config appsetings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) | 建立或更新 Azure Web 應用程式的應用程式設定。 應用程式設定會顯示為應用程式的環境變數。 |

## <a name="next-steps"></a>後續步驟

如需 Azure CLI 的詳細資訊，請參閱 [Azure CLI 文件](https://docs.microsoft.com/cli/azure/overview)。

您可以在 [Azure App Service 文件](../app-service-cli-samples.md)中找到其他的 App Service CLI 指令碼範例。
