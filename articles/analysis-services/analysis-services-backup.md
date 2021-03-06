---
title: "Azure Analysis Services 備份與還原 | Microsoft Docs"
description: "說明如何備份和還原 Azure Analysis Services 資料庫。"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: fb660384f2f9f569bcfbe7fa7d5c1f7ce772cacd
ms.lasthandoff: 04/20/2017


---

# <a name="backup-and-restore"></a>備份與還原

在 Azure Analysis Services 中備份表格式模型資料庫與內部部署的 Analysis Services 情況非常類似。 主要的差異在於備份檔案的儲存位置。 備份檔案必須儲存至 [Azure 儲存體帳戶](../storage/storage-create-storage-account.md)中的容器。 您可以使用您已經有的儲存體帳戶和容器，或是在為您的伺服器設定儲存體設定時再建立帳戶和容器。

> [!NOTE]
> 建立儲存體帳戶會導致產生新的可計費服務。 若要深入了解，請參閱 [Azure 儲存體定價](https://azure.microsoft.com/pricing/details/storage/blobs/)。
> 
> 

備份會以 .abf 副檔名儲存。 針對記憶體內表格式模型，會同時儲存模型資料和中繼資料。 針對 Direct Query 表格式模型，則只會儲存模型中繼資料。 視您選擇的選項而定，可以將備份壓縮和加密。 



## <a name="configure-storage-settings"></a>設定儲存體設定
進行備份之前，您必須為伺服器設定儲存體設定。


### <a name="to-configure-storage-settings"></a>設定儲存體設定
1.  在 Azure 入口網站 > [設定] 中，按一下 [備份]。

    ![[設定] 中的 [備份]](./media/analysis-services-backup/aas-backup-backups.png)

2.  按一下 [已啟用]，然後按一下 [儲存體設定]。

    ![啟用](./media/analysis-services-backup/aas-backup-enable.png)

3. 選取您的儲存體帳戶，或建立新帳戶。

4. 選取容器，或建立新容器。

    ![選取容器](./media/analysis-services-backup/aas-backup-container.png)

5. 儲存您的備份設定。 每當您變更儲存體設定，或是啟用或停用備份，都必須儲存變更。

    ![儲存備份設定](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a>備份

### <a name="to-backup-by-using-ssms"></a>使用 SSMS 來進行備份

1. 在 SSMS 中，於資料庫上按一下滑鼠右鍵 > [備份]。

2. 在 [備份資料庫] > [備份檔案] 中，按一下 [瀏覽]。

3. 在 [另存新檔] 對話方塊中，確認資料夾路徑，然後輸入備份檔案的名稱。 檔案名稱的副檔名預設為 .abf。 

4. 在 [備份資料庫] 對話方塊中，選取選項。

    **允許檔案覆寫** - 若要覆寫同名的備份檔案，請選取此選項。 如果未選取此選項，您要儲存的檔案就不能與相同位置中已經存在的檔案同名。

    **套用壓縮** - 若要壓縮備份檔案，請選取此選項。 壓縮過的備份檔案可節省磁碟空間，但需要使用稍微多一點的 CPU 資源。 

    **加密備份檔案** - 若要將備份檔案加密，請選取此選項。 此選項需要有使用者提供的密碼來保護備份檔案。 此密碼可防止以還原作業以外的任何其他方式讀取備份資料。 如果您選擇將備份加密，請將密碼儲存在安全的位置。

5. 按一下 [確定] 以建立並儲存備份檔案。


### <a name="powershell"></a>PowerShell
使用 [Backup-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) Cmdlet。

## <a name="restore"></a>還原
在還原時，您的備份檔案必須位於您為伺服器所設定的儲存體帳戶中。 如果您需要將備份檔案從內部部署位置移至儲存體帳戶，請使用 [Microsoft Azure 儲存體總管](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)或 [AzCopy](../storage/storage-use-azcopy.md) 命令列公用程式。 

如果您要從內部部署 SQL Server Analysis Services 伺服器來還原表格式 1200 模型資料庫，您必須先從該模型的角色中移除所有網域使用者，再將他們新增回角色中以成為 Azure Active Directory 使用者。 這些角色將會相同。

### <a name="to-restore-by-using-ssms"></a>使用 SSMS 來進行還原

1. 在 SSMS 中，於資料庫上按一下滑鼠右鍵 > [還原]。

2. 在 [備份資料庫] 對話方塊的 [備份檔案] 中，按一下 [瀏覽]。

3. 在 [尋找資料庫檔案] 對話方塊中，選取您想要還原的檔案。

4. 在 [還原資料庫] 中，選取資料庫。

5. 指定選項。 安全性選項必須與您備份時所使用的備份選項相符。


### <a name="powershell"></a>PowerShell

使用 [Restore-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) Cmdlet。


## <a name="related-information"></a>相關資訊

[Azure 儲存體帳戶](../storage/storage-create-storage-account.md)  
[高可用性](analysis-services-bcdr.md)     
[管理 Azure Analysis Services](analysis-services-manage.md)

