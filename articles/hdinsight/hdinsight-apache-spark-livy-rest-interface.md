---
title: "使用 Livy 從遠端將作業提交至 Azure HDInsight 上的 Spark | Microsoft Docs"
description: "了解如何使用 Livy 和 HDInsight 叢集從遠端提交 Spark 作業。"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2817b779-1594-486b-8759-489379ca907d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 6cb0da6d7b3aafeb9a8079b427e31c66811a6281
ms.lasthandoff: 03/25/2017


---
# <a name="submit-spark-jobs-remotely-to-an-apache-spark-cluster-on-hdinsight-using-livy"></a>使用 Livy 將 Spark 作業遠端提交至 HDInsight 上的 Apache Spark 叢集

Azure HDInsight 上的 Apache Spark 叢集包含 Livy，這是一個 REST 介面，可讓您從遠端將作業提交給 Spark 叢集。 如需詳細文件，請參閱 [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)。

您可以使用 Livy 執行互動式 Spark 殼層，或提交要在 Spark 上執行的批次作業。 本文將討論如何使用 Livy 提交批次作業。 下列語法會使用 Curl 對 Livy 端點發出 REST 呼叫。

**必要條件：**

您必須滿足以下條件：

* Azure 訂用帳戶。 請參閱 [取得 Azure 免費試用](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
* HDInsight 上的 Apache Spark 叢集。 如需指示，請參閱 [在 Azure HDInsight 中建立 Apache Spark 叢集](hdinsight-apache-spark-jupyter-spark-sql.md)。

## <a name="submit-a-batch-job"></a>提交批次工作
在提交批次作業之前，您必須將應用程式 jar 上傳至與叢集相關聯的叢集儲存體。 您可以使用命令列公用程式 [**AzCopy**](../storage/storage-use-azcopy.md) 來執行此動作。 此外也有很多用戶端可用來上傳資料。 您可以在 [在 HDInsight 上將 Hadoop 作業的資料上傳](hdinsight-upload-data.md)中找到其詳細資訊。

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**範例**：

* 如果 jar 檔案位於叢集儲存體 (WASB) 上
  
        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"
* 如果您想要在輸入檔案 (在此範例中為 input.txt) 中傳遞 jar 檔案名稱和類別名稱
  
        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-batches-running-on-the-cluster"></a>取得在叢集上執行之批次的相關資訊
    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**範例**：

* 如果您想要擷取在叢集上執行的所有批次：
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
* 如果您想要擷取具有指定批次識別碼的特定批次
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="delete-a-batch-job"></a>刪除批次作業
    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**範例**：

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-and-high-availability"></a>Livy 與高可用性
Livy 可為在叢集上執行的 Spark 作業提供高可用性。 以下是一些範例。

* 如果在您從遠端將作業提交給 Spark 叢集之後，Livy 服務當機，作業將會繼續在背景執行。 當 Livy 恢復運作時，它會還原作業的狀態並回報。
* 適用於 HDInsight 的 Jupyter Notebook 是由 Livy 在後端提供技術支援。 如果在 Notebook 執行 Spark 作業時，Livy 服務重新啟動，Notebook 將會繼續執行程式碼單元。 

## <a name="show-me-an-example"></a>請舉例說明
在本節中，我們將透過範例了解如何使用 Livy 提交 Spark 應用程式、監視應用程式的進度，然後刪除作業。 我們在此範例中使用的應用程式，就是 [建立獨立 Scala 應用程式，並在 HDInsight Spark 叢集上執行](hdinsight-apache-spark-create-standalone-application.md)一文中所開發的應用程式。 下列步驟假設：

* 您已將應用程式 jar 複製到與叢集相關聯的儲存體帳戶。
* 您已將 CuRL 安裝在要嘗試這些步驟的電腦上。

請執行下列步驟：

1. 我們先確認 Livy 正在叢集上執行。 我們可以取得執行中的批次清單，加以確認。 如果這是您第一次使用 Livy 執行作業，則應會傳回零。
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    您應該會看到如下的輸出：
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    請留意到輸出的最後一行顯示為 **total:0**，這表示沒有執行中的批次。
2. 現在，我們要提交批次作業。 下列程式碼片段會使用輸入檔案 (input.txt) 傳遞 jar 名稱和類別名稱，做為參數。 如果您要從 Windows 電腦執行這些步驟，建議您採用此方法。
   
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    檔案 **input.txt** 中的參數定義如下：
   
        { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
   
    您應該會看到如下所示的輸出：
   
        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    請留意到輸出的最後一行顯示為 **state:starting**。 此外也顯示 **id:0**。 這是批次識別碼。
3. 現在，您可以使用批次識別碼來擷取此批次的狀態。
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    您應該會看到如下所示的輸出：
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    輸出此時顯示 **state:success**，這表示作業已順利完成。
4. 現在，您可以視需要刪除批次。
   
        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    您應該會看到如下所示的輸出：
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    輸出的最後一行顯示批次已成功刪除。 如果您刪除執行中的作業，該作業實際上就會被刪除。 如果您刪除已完成的作業，無論成功與否，這將會完全刪除作業資訊。

## <a name="using-livy-on-hdinsight-35-spark-clusters"></a>在 HDInsight 3.5 Spark 叢集上使用 Livy

根據預設，HDInsight 3.5 叢集會停用使用本機檔案路徑，以存取範本資料檔案或 jar。 建議您使用 `wasb://` 路徑，而不是從叢集存取 jar 或範本資料檔案。 如果您確定要使用本機路徑，您就必須同時更新 Ambari 組態。 若要這樣做：

1. 移至叢集的 Ambari 入口網站。 Ambari Web UI 位在您的 HDInsight 叢集的 https://**CLUSTERNAME**.azurehdidnsight.net，其中 CLUSTERNAME 是您的叢集的名稱。

2. 在左側導覽中，按一下 [Livy]，然後按一下 [設定]。

3. 在 [livy-default] 底下新增屬性名稱 `livy.file.local-dir-whitelist`，如果您想要允許存取整個檔案系統，可將其值設為 **"/"**。 如果您只想要允許存取特定目錄，請將值設為該目錄的路徑。

## <a name="troubleshooting"></a>疑難排解

以下是一些使用 Livy 進行對 Spark 叢集的遠端作業提交時可能遇到的問題。

### <a name="using-an-external-jar-from-the-additional-storage-is-not-supported"></a>不支援從其他儲存體使用外部 jar

**問題︰**如果您正在使用參考與叢集相關聯之其他儲存體中外部 jar 的 Livy 來執行 Spark 作業，則作業將會失敗。

**解決方式︰**請確定您想要使用的 jar 位於與 HDInsight 叢集相關聯的預設儲存體中。


## <a name="seealso"></a>另請參閱
* [概觀：Azure HDInsight 上的 Apache Spark](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>案例
* [Spark 和 BI：在 HDInsight 中搭配使用 Spark 和 BI 工具執行互動式資料分析](hdinsight-apache-spark-use-bi-tools.md)
* [Spark 和機器學習服務：使用 HDInsight 中的 Spark，利用 HVAC 資料來分析建築物溫度](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark 和機器學習服務：使用 HDInsight 中的 Spark 來預測食品檢查結果](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark 串流：使用 HDInsight 中的 Spark 來建置即時串流應用程式](hdinsight-apache-spark-eventhub-streaming.md)
* [使用 HDInsight 中的 Spark 進行網站記錄分析](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>建立及執行應用程式
* [使用 Scala 建立獨立應用程式](hdinsight-apache-spark-create-standalone-application.md)

### <a name="tools-and-extensions"></a>工具和擴充功能
* [Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applicatons (使用 IntelliJ IDEA 的 HDInsight Tools 外掛程式來建立和提交 Spark Scala 應用程式)](hdinsight-apache-spark-intellij-tool-plugin.md)
* [使用 IntelliJ IDEA 的 HDInsight Tools 外掛程式遠端偵錯 Spark 應用程式](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [利用 HDInsight 上的 Spark 叢集來使用 Zeppelin Notebook](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [HDInsight 的 Spark 叢集中 Jupyter Notebook 可用的核心](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [搭配 Jupyter Notebook 使用外部套件](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [在電腦上安裝 Jupyter 並連接到 HDInsight Spark 叢集](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>管理資源
* [在 Azure HDInsight 中管理 Apache Spark 叢集的資源](hdinsight-apache-spark-resource-manager.md)
* [追蹤和偵錯在 HDInsight 中的 Apache Spark 叢集上執行的作業](hdinsight-apache-spark-job-debugging.md)


