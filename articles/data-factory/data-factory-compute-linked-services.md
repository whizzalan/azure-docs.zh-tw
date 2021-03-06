---
title: "Azure Data Factory 支援的計算環境 | Microsoft Docs"
description: "了解您可以在 Azure Data Factory 管線中用來轉換/處理資料的計算環境。"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6877a7e8-1a58-4cfb-bbd3-252ac72e4145
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: f9f29cd20020ec5e6538bf1dd31e89c2f7adcc92
ms.lasthandoff: 04/03/2017


---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Azure Data Factory 支援的計算環境
本文說明您可用來處理或轉換資料的各種計算環境。 其中還提供在設定將這些計算環境連結至 Azure Data Factory 的連結服務時，Data Factory 所支援的不同組態 (隨選與自備) 的詳細資料。

下表列出 Data Factory 支援的計算環境以及可在環境上執行的活動。 

| 計算環境 | 活動 |
| --- | --- |
| [隨選 HDInsight 叢集](#azure-hdinsight-on-demand-linked-service)或[您自己的 HDInsight 叢集](#azure-hdinsight-linked-service) |[DotNet](data-factory-use-custom-activities.md)、[Hive](data-factory-hive-activity.md)、[Pig](data-factory-pig-activity.md)、[MapReduce](data-factory-map-reduce.md)、[Hadoop 串流](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](#azure-batch-linked-service) |[DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) |[Machine Learning 活動︰批次執行和更新資源](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) |[Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](#azure-sql-linked-service)、[Azure SQL 資料倉儲](#azure-sql-data-warehouse-linked-service)、[SQL Server](#sql-server-linked-service) |[預存程序](data-factory-stored-proc-activity.md) |

## <a name="supported-hdinsight-versions-in-azure-data-factory"></a>Azure Data Factory 中支援的 HDInsight 版本
Azure HDInsight 支援多個可隨時部署的 Hadoop 叢集版本。 每一個版本選擇都會建立特定版本的 Hortonworks Data Platform (HDP) 散發，以及該散發內包含的一組元件。 Microsoft 會持續更新所支援 HDInsight 版本的清單，以提供最新的 Hadoop 生態系統元件和修正程式。 HDInsight 3.2 已於 04/01/2017 淘汰，如需詳細資訊，請參閱[支援的 HDInsight 版本](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions)。

這會影響具有對 HDInsight 3.2 叢集執行活動的現有 Azure Data Factory。 我們建議使用者遵循下列指導方針來更新受影響的 Data Factory。

### <a name="for-linked-services-pointing-to-your-own-hdinsight-clusters"></a>針對指向您自己的 HDInsight 叢集的連結服務
* **指向您自己的 HDInsight 3.2 或舊版叢集的 HDInsight 連結服務：**

  Azure Data Factory 支援將作業提交至您自己的 HDInsight 叢集，從 HDI 3.1 至[最新支援的 HDInsight 版本](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions)。 不過，根據[支援的 HDInsight 版本](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions)中記載的淘汰原則，在 04/01/2017 之後，您無法再建立 HDInsight 3.2 叢集。  

  **建議：** 
  * 利用[可以搭配不同 HDInsight 版本使用的 Hadoop 元件](../hdinsight/hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions)和[與 HDInsight 版本相關聯的 Hortonworks 版本資訊](../hdinsight/hdinsight-component-versioning.md#hortonworks-release-notes-associated-with-hdinsight-versions)中記載的資訊，執行測試以確保參考此連結服務之活動的相容性為[最新支援的 HDInsight 版本](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions)。
  * 將您的 HDInsight 3.2 叢集升級為[最新支援的 HDInsight 版本](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions)，以取得最新的 Hadoop 生態系統元件和修正程式。 

* **指向您自己的 HDInsight 3.3 或以上叢集的 HDInsight 連結服務：**

  Azure Data Factory 支援將作業提交至您自己的 HDInsight 叢集，從 HDI 3.1 至[最新支援的 HDInsight 版本](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions)。 
  
  **建議：** 
  * 從 Data Factory 的觀點來看，不需要採取任何動作。 不過，如果您是在較低版本的 HDInsight，仍建議您升級至[最新支援的 HDInsight 版本](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions)，以取得最新的 Hadoop 生態系統元件和修正程式。

### <a name="for-hdinsight-on-demand-linked-services"></a>針對 HDInsight 隨選連結服務
* **3.2 版或以下版本是在 HDInsight 隨選連結服務 JSON 定義中指定：**
  
  自 **05/05/2017** 起，Azure Data Factory 可支援建立隨選 HDInsight 3.3 和更新版本的叢集。 並且對現有隨選 HDInsight 3.2 連結服務的結束支援會延長至 **07/05/2017**。  

  **建議：** 
  * 利用[可以搭配不同 HDInsight 版本使用的 Hadoop 元件](../hdinsight/hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions)和[與 HDInsight 版本相關聯的 Hortonworks 版本資訊](../hdinsight/hdinsight-component-versioning.md#hortonworks-release-notes-associated-with-hdinsight-versions)中記載的資訊，執行測試以確保參考此連結服務之活動的相容性為[最新支援的 HDInsight 版本](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions)。
  * 在 **07/05/2017** 之前，將隨選 HDI 連結服務 JSON 定義中的 Version 屬性更新為[最新支援的 HDInsight 版本](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions)，以取得最新的 Hadoop 生態系統元件和修正程式。 如需詳細的 JSON 定義，請參閱 [Azure HDInsight 隨選連結服務範例](#azure-hdinsight-on-demand-linked-service)。 

* **隨選 HDInsight 連結服務中未指定的版本：**
  
  自 **05/05/2017** 起，Azure Data Factory 可支援建立隨選 HDInsight 3.3 和更新版本的叢集。 並且對現有隨選 HDInsight 3.2 連結服務的結束支援會延長至 **07/05/2017**。 

  在 **05/05/2017** 之前，如果保留空白，version 和 osType 屬性的預設值為： 

  | 屬性 | 預設值 | 必要 |
  | --- | --- | --- |
  版本    | Windows 叢集為 HDI 3.1，Linux 叢集為 HDI 3.2。| 否
  osType | 預設值為 Windows | 否

  在 **05/05/2017** 之後，如果保留空白，version 和 osType 屬性的預設值為：

  | 屬性 | 預設值 | 必要 |
  | --- | --- | --- |
  版本    | Windows 叢集為 HDI 3.3，Linux 叢集為 3.5。    | 否
  osType | 預設值為 Linux    | 否

  **建議：** 
  * 在 **05/05/2017** 之前，更新連結服務，以明確地在隨選 HDInsight 連結服務 JSON 定義中定義預期的 Version 和 osType 組合。 您可以將 Version 設定為 3.2 來確保回溯相容性。 
  * 在 **05/05/2017** 與 **07/05/2017** 之間，利用[可以搭配不同 HDInsight 版本使用的 Hadoop 元件](../hdinsight/hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions)和[與 HDInsight 版本相關聯的 Hortonworks 版本資訊](../hdinsight/hdinsight-component-versioning.md#hortonworks-release-notes-associated-with-hdinsight-versions)中記載的資訊，執行測試以確保參考此連結服務之活動的相容性為[最新支援的 HDInsight 版本](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions)。  
  * 在 **07/05/2017** 之前，將隨選 HDInsight 連結服務 JSON 定義中的 Version 屬性更新為[最新支援的 HDInsight 版本](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions)，或利用預設值 (即 HDInsight 3.5) 以取得最新的 Hadoop 生態系統元件和修正程式。 如需詳細的 JSON 定義，請參閱 [Azure HDInsight 隨選連結服務範例](#azure-hdinsight-on-demand-linked-service)。

>[!Note]
>目前 Azure Data Factory 不支援使用 Azure Data Lake Store 作為主要存放區的 HDInsight 叢集。 您將需要使用 Azure 儲存體作為 HDInsight 叢集的主要存放區。 
>  
>  

## <a name="on-demand-compute-environment"></a>隨選計算環境
在這種組態中，運算環境完全是由 Azure Data Factory 服務管理。 Data Factory 服務會在工作提交前自動建立運算環境以處理資料，而在工作完成時予以移除。 您可以建立隨選計算環境的連結服務、加以設定，以及控制工作執行、叢集管理和啟動動作的細微設定。

> [!NOTE]
> 目前僅支援 Azure HDInsight 叢集的隨選組態。
> 
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure HDInsight 隨選連結服務
Azure Data Factory 服務可自動建立以 Windows/Linux 為基礎的隨選 HDInsight 叢集來處理資料。 此叢集會建立在與叢集相關聯的儲存體帳戶 (JSON 中的 linkedServiceName 屬性) 相同的區域中。

請注意下列有關隨選 HDInsight 連結服務的 **重點** ：

* 您不會看到 Azure 訂用帳戶中建立的隨選 HDInsight 叢集。 Azure Data Factory 服務會代您管理隨選 HDInsight 叢集。
* 在隨選 HDInsight 叢集上執行之工作的記錄檔會被複製到與 HDInsight 叢集相關聯的儲存體帳戶。 您可以從 Azure 入口網站的 [活動執行詳細資料]  刀鋒視窗存取這些記錄檔。 如需詳細資訊，請參閱 [監視及管理管線](data-factory-monitor-manage-pipelines.md) 一文。
* 只會針對 HDInsight 叢集啟動並執行工作的時間來向您收取費用。

> [!IMPORTANT]
> 通常會花費 **20 分鐘**或更久的時間來佈建隨選 Azure HDInsight 叢集。
> 
> 

### <a name="example"></a>範例
下列 JSON 會定義以 Linux 為基礎的隨選 HDInsight 連結服務。 Data Factory 服務會在處理資料配量時自動建立 **以 Linux 為基礎的** HDInsight 叢集。 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "clusterSize": 4,
            "timeToLive": "00:05:00",
            "osType": "linux",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

若要使用以 Windows 為基礎的 HDInsight 叢集，請將 **osType** 設為 **windows**，或者不要使用此屬性，因為預設值是︰windows。  

> [!IMPORTANT]
> HDInsight 叢集會在您於 JSON 中指定的 Blob 儲存體 (**linkedServiceName**) 建立**預設容器**。 HDInsight 不會在刪除叢集時刪除此容器。 這是設計的行為。 在使用 HDInsight 隨選連結服務時，除非有現有的即時叢集 (**timeToLive**)，否則每當需要處理配量時，就會建立 HDInsight 叢集，並在處理完成時予以刪除。 
> 
> 隨著處理的配量越來越多，您會在 Azure Blob 儲存體中看到許多容器。 如果在疑難排解作業時不需要這些容器，建議您加以刪除以降低儲存成本。 這些容器的名稱遵循下列模式："adf**yourdatafactoryname**-**linkedservicename**-datetimestamp"。 請使用 [Microsoft 儲存體總管](http://storageexplorer.com/) 之類的工具刪除 Azure Blob 儲存體中的容器。
> 
> 

### <a name="properties"></a>屬性
| 屬性 | 說明 | 必要 |
| --- | --- | --- |
| 類型 |type 屬性應設為 **HDInsightOnDemand**。 |是 |
| clusterSize |叢集中的背景工作/資料節點數。 HDInsight 叢集會利用您為此屬性指定的 2 個前端節點以及背景工作節點數目來建立。 節點大小為具有 4 個核心的 Standard_D3，因此 4 個背景工作節點的叢集需要 24 個核心 (4\*4 = 16 個核心用於背景工作節點，加上 2\*4 = 8 個核心用於前端節點)。 如需 Standard_D3 層的詳細資料，請參閱[在 HDInsight 中建立 Linux 型 Hadoop 叢集](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md)。 |是 |
| timetolive |隨選 HDInsight 叢集允許的閒置時間。 指定在活動執行完成後，如果叢集中沒有其他作用中的作業，隨選 HDInsight 叢集要保持運作多久。<br/><br/>例如，如果活動執行花費 6 分鐘，而 timetolive 設為 5 分鐘，叢集會在處理活動執行的 6 分鐘期間之後保持運作 5 分鐘。 如果 6 分鐘期間內執行了另一個活動執行，它便會由相同叢集來處理。<br/><br/>建立隨選 HDInsight 叢集是昂貴的作業 (可能需要一段時間)，因此請視需要使用這項設定，重複使用隨選 HDInsight 叢集以改善 Data Factory 的效能。<br/><br/>如果您將 timetolive 值設為 0，叢集會在處理活動執行後立即刪除。 另一方面，如果您設定較高的值，叢集可能會有不必要的閒置而導致高成本。 因此，請務必根據您的需求設定適當的值。<br/><br/>如果適當地設定 timetolive 屬性值，則多個管線可以共用相同的隨選 HDInsight 叢集執行個體 |是 |
| 版本 |HDInsight 叢集的版本。 針對 Windows 叢集的預設值為 3.1，針對 Linux 叢集的預設值為 3.2。 |否 |
| 預設容器 |隨選叢集用於儲存及處理資料的 Azure 儲存體連結服務。 <p>目前，您無法建立使用 Azure Data Lake Store 做為儲存體的隨選 HDInsight 叢集。 如果您想要在 Azure Data Lake Store 中儲存 HDInsight 處理的結果資料，可使用複製活動將 Azure Blob 儲存體的資料複製到 Azure Data Lake Store。</p>  | 是 |
| additionalLinkedServiceNames |指定 HDInsight 連結服務的其他儲存體帳戶，讓 Data Factory 服務代表您註冊它們。 |否 |
| osType |作業系統的類型。 允許的值為：Windows (預設值) 和 linux |否 |
| hcatalogLinkedServiceName |指向 HCatalog 資料庫的 Azure SQL 連結服務名稱。 會使用 Azure SQL 資料庫作為中繼存放區，建立隨選 HDInsight 叢集。 |否 |

#### <a name="additionallinkedservicenames-json-example"></a>additionalLinkedServiceNames JSON 範例

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>進階屬性
您也可以針對隨選 HDInsight 叢集的細微組態指定下列屬性。

| 屬性 | 說明 | 必要 |
|:--- |:--- |:--- |
| coreConfiguration |指定要建立之 HDInsight 叢集的核心組態參數 (如 core-site.xml 所示)。 |否 |
| hBaseConfiguration |指定 HDInsight 叢集的 HBase 組態參數 (hbase-site.xml)。 |否 |
| hdfsConfiguration |指定 HDInsight 叢集的 HDFS 組態參數 (hdfs-site.xml)。 |否 |
| hiveConfiguration |指定 HDInsight 叢集的 hive 組態參數 (hive-site.xml)。 |否 |
| mapReduceConfiguration |指定 HDInsight 叢集的 MapReduce 組態參數 (mapred-site.xml)。 |否 |
| oozieConfiguration |指定 HDInsight 叢集的 Oozie 組態參數 (oozie-site.xml)。 |否 |
| stormConfiguration |指定 HDInsight 叢集的 Storm 組態參數 (storm-site.xml)。 |否 |
| yarnConfiguration |指定 HDInsight 叢集的 Yarn 組態參數 (yarn-site.xml)。 |否 |

#### <a name="example--on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>範例 – 包含進階屬性的隨選 HDInsight 叢集組態

```json
{
  "name": " HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "clusterSize": 16,
      "timeToLive": "01:30:00",
      "linkedServiceName": "adfods1",
      "coreConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "hiveConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "mapReduceConfiguration": {
        "mapreduce.reduce.java.opts": "-Xmx4000m",
        "mapreduce.map.java.opts": "-Xmx4000m",
        "mapreduce.map.memory.mb": "5000",
        "mapreduce.reduce.memory.mb": "5000",
        "mapreduce.job.reduce.slowstart.completedmaps": "0.8"
      },
      "yarnConfiguration": {
        "yarn.app.mapreduce.am.resource.mb": "5000",
        "mapreduce.map.memory.mb": "5000"
      },
      "additionalLinkedServiceNames": [
        "datafeeds",
        "adobedatafeed"
      ]
    }
  }
}
```

### <a name="node-sizes"></a>節點大小
您可使用下列屬性指定前端、資料和的 zookeeper 節點的大小： 

| 屬性 | 說明 | 必要 |
|:--- |:--- |:--- |
| headNodeSize |指定前端節點的大小。 預設值為：Standard_D3。 如需詳細資料，請參閱以下 **指定節點大小** 一節。 |否 |
| dataNodeSize |指定資料節點的大小。 預設值為：Standard_D3。 |否 |
| zookeeperNodeSize |指定 Zoo Keeper 節點的大小。 預設值為：Standard_D3。 |否 |

#### <a name="specifying-node-sizes"></a>指定節點大小
如需了解需為上方屬性指定的字串值，請參閱[虛擬機器的大小](../virtual-machines/linux/sizes.md)一文。 值必須符合本文件中所參考的 **CMDLET 與 APIS**。 如您在文中所見，若資料節點的大小設定為大 (預設值)，則記憶體大小為 7 GB，其可能不適用於您的案例。 

若想要建立 D4 大小的前端節點與背景工作節點，必須指定 **Standard_D4** 作為 headNodeSize 與 dataNodeSize 屬性的值。 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

若您為這些屬性指定錯誤的值，可能會顯示下列 **錯誤：** 無法建立叢集。 例外狀況：無法完成叢集建立作業。 作業失敗，錯誤碼為 '400'。 叢集剩餘狀態：「錯誤」。 訊息：「PreClusterCreationValidationFailure」。 若顯示此錯誤，請確認您使用來自前述文章中資料表的 **CMDLET 與 APIS** 名稱。  

## <a name="bring-your-own-compute-environment"></a>自備計算環境
在這種組態中，使用者可以將現有的運算環境註冊為 Data Factory 中的連結服務。 此運算環境是由使用者管理並由 Data Factory 服務用來執行活動。

下列計算環境可支援這類型的組態：

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL DB、Azure SQL DW、SQL Server

## <a name="azure-hdinsight-linked-service"></a>Azure HDInsight 連結服務
您可以建立 Azure HDInsight 連結服務，以向 Data Factory 註冊自己的 HDInsight 叢集。

### <a name="example"></a>範例

```json
{
  "name": "HDInsightLinkedService",
  "properties": {
    "type": "HDInsight",
    "typeProperties": {
      "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
      "userName": "admin",
      "password": "<password>",
      "linkedServiceName": "MyHDInsightStoragelinkedService"
    }
  }
}
```

### <a name="properties"></a>屬性
| 屬性 | 說明 | 必要 |
| --- | --- | --- |
| 類型 |type 屬性應設為 **HDInsight**。 |是 |
| clusterUri |HDInsight 叢集的 URI。 |是 |
| username |指定要用來連接到現有 HDInsight 叢集的使用者名稱。 |是 |
| password |指定使用者帳戶的密碼。 |是 |
| linkedServiceName | 參照 HDInsight 叢集所使用 Azure Blob 儲存體的 Azure 儲存體連結服務名稱。 <p>目前，您無法針對此屬性指定 Azure Data Lake Store 連結服務。 如果 HDInsight 叢集可存取 Data Lake Store，您可以透過 Hive/Pig 指令碼存取 Azure Data Lake Store 中的資料。 </p>  |是 |

## <a name="azure-batch-linked-service"></a>Azure Batch 連結服務
您可以建立 Azure Batch 連結服務，以向 Data Factory 註冊虛擬機器 (VM) 的 Batch 集區。 您可以使用 Azure Batch 或 Azure HDInsight 執行 .NET 自訂活動。

如果您不熟悉 Azure Batch 服務，請參閱下列主題：

* [Azure Batch 基本知識](../batch/batch-technical-overview.md) ，以取得 Azure Batch 服務的概觀。
* [New-AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) Cmdlet 可建立 Azure Batch 帳戶 (或) [Azure 入口網站](../batch/batch-account-create-portal.md)，以使用 Azure 入口網站建立 Azure Batch 帳戶。 如需使用此 Cmdlet 的詳細指示，請參閱 [使用 PowerShell 管理 Azure Batch 帳戶](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) 主題。
* [New-AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) Cmdlet 可建立 Azure Batch 集區。

### <a name="example"></a>範例

```json
{
  "name": "AzureBatchLinkedService",
  "properties": {
    "type": "AzureBatch",
    "typeProperties": {
      "accountName": "<Azure Batch account name>",
      "accessKey": "<Azure Batch account key>",
      "poolName": "<Azure Batch pool name>",
      "linkedServiceName": "<Specify associated storage linked service reference here>"
    }
  }
}
```

將 "**.\<區域名稱\>**" 附加至為 **accountName** 屬性所用的 Batch 帳戶名稱。 範例：

```json
"accountName": "mybatchaccount.eastus"
```

另一個選項是提供 batchUri 端點，如下所示。  

```json
"accountName": "adfteam",
"batchUri": "https://eastus.batch.azure.com",
```

### <a name="properties"></a>屬性
| 屬性 | 說明 | 必要 |
| --- | --- | --- |
| 類型 |type 屬性應設為 **AzureBatch**。 |是 |
| accountName |建立 Azure Batch 帳戶。 |是 |
| accessKey |Azure Batch 帳戶的存取金鑰。 |是 |
| poolName |虛擬機器的集區名稱。 |是 |
| 預設容器 |與此 Azure Batch 連結服務相關聯的 Azure 儲存體服務連結名稱。 此連結服務用於執行活動及儲存活動執行記錄檔所需的暫存檔案。 |是 |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning 連結服務
您可建立 Azure Machine Learning 連結服務，以向 Data Factory 註冊 Machine Learning 批次評分端點。

### <a name="example"></a>範例

```json
{
  "name": "AzureMLLinkedService",
  "properties": {
    "type": "AzureML",
    "typeProperties": {
      "mlEndpoint": "https://[batch scoring endpoint]/jobs",
      "apiKey": "<apikey>"
    }
  }
}
```

### <a name="properties"></a>屬性
| 屬性 | 說明 | 必要 |
| --- | --- | --- |
| 類型 |type 屬性應設為： **AzureML**。 |是 |
| mlEndpoint |批次評分 URL。 |是 |
| apiKey |已發佈的工作區模型的 API。 |是 |

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics 連結服務
您應建立 **Azure Data Lake Analytics** 連結服務，將 Azure Data Lake Analytics 計算服務連結至 Azure Data Factory，然後再使用管線中的 [Data Lake Analytics U-SQL 活動](data-factory-usql-activity.md) 。

下列範例提供 Azure Data Lake Analytics 連結服務的 JSON 定義。

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>",
            "subscriptionId": "<subscription id>",
            "resourceGroupName": "<resource group name>"
        }
    }
}
```

下表提供 JSON 定義中所使用之屬性的描述：

| 屬性 | 說明 | 必要 |
| --- | --- | --- |
| 類型 |type 屬性應設為： **AzureDataLakeAnalytics**。 |是 |
| accountName |Azure Data Lake Analytics 帳戶名稱。 |是 |
| dataLakeAnalyticsUri |Azure Data Lake Analytics URI。 |否 |
| 授權 |按一下 Data Factory 編輯器中的 [授權]  按鈕並完成 OAuth 登入後，即會自動擷取授權碼。 |是 |
| subscriptionId |Azure 訂用帳戶識別碼 |否 (如果未指定，便會使用 Data Factory 的訂用帳戶)。 |
| resourceGroupName |Azure 資源群組名稱 |否 (若未指定，便會使用 Data Factory 的資源群組)。 |
| sessionId |OAuth 授權工作階段的工作階段識別碼。 每個工作階段識別碼都是唯一的，只能使用一次。 這是在 Data Factory 編輯器中自動產生。 |是 |

您使用 [授權] 按鈕所產生的授權碼在一段時間後會到期。 請參閱下表以了解不同類型的使用者帳戶的到期時間。 當驗證**權杖到期**時，您可能會看到下列錯誤訊息：認證作業發生錯誤：invalid_grant - AADSTS70002：驗證認證時發生錯誤。 AADSTS70008：提供的存取授權已過期或撤銷。 追蹤識別碼：d18629e8-af88-43c5-88e3-d8419eb1fca1 相互關連識別碼：fac30a0c-6be6-4e02-8d69-a776d2ffefd7 時間戳記：2015-12-15 21:09:31Z

| 使用者類型 | 到期時間 |
|:--- |:--- |
| 不受 Azure Active Directory 管理的使用者帳戶 (@hotmail.com、@live.com、@outlook.com 等) |12 小時 |
| 受 Azure Active Directory (AAD) 管理的使用者帳戶 |最後一次執行配量後的 14 天。 <br/><br/>如果以 OAuth 式連結服務為基礎的配量至少每 14 天執行一次，則為 90 天。 |

如果要避免/解決此錯誤，您必須在**權杖到期**時使用 [授權] 按鈕重新授權，然後重新部署連結服務。 您也可以使用下一節中的程式碼以程式設計方式產生 sessionId 和 authorization 屬性的值。 

### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>若要以程式設計方式產生 sessionId 與 authorization 的值
下列程式碼會產生 **sessionId** 與 **authorization** 值。  

```CSharp

if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

請參閱 [AzureDataLakeStoreLinkedService 類別](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)、[AzureDataLakeAnalyticsLinkedService 類別](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)和 [AuthorizationSessionGetResponse 類別](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx)主題，以取得在程式碼中使用的 Data Factory 類別的詳細資訊。 您必須針對 WindowsFormsWebAuthenticationDialog 類別將參考新增至：Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll。 

## <a name="azure-sql-linked-service"></a>Azure SQL 連結服務
您可建立 Azure SQL 連結服務，並將其與 [預存程序活動](data-factory-stored-proc-activity.md) 搭配使用，以叫用 Data Factory 管線中的預存程序。 如需此連結服務的詳細資料，請參閱 [Azure SQL 連接器](data-factory-azure-sql-connector.md#linked-service-properties) 一文。

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL 資料倉儲連結服務
您可以建立 Azure SQL 資料倉儲連結服務，並將其與 [預存程序活動](data-factory-stored-proc-activity.md) 搭配使用，以叫用 Data Factory 管線中的預存程序。 如需此連結服務的詳細資料，請參閱 [Azure SQL 資料倉儲連接器](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) 一文。

## <a name="sql-server-linked-service"></a>SQL Server 連結服務
您可以建立 SQL Server 連結服務，並將其與 [預存程序活動](data-factory-stored-proc-activity.md) 搭配使用，以叫用 Data Factory 管線中的預存程序。 如需此連結服務的詳細資料，請參閱 [SQL Server 連接器](data-factory-sqlserver-connector.md#linked-service-properties) 一文。


