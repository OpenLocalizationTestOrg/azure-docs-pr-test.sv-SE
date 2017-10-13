---
title: Transformera data med Spark i Azure Data Factory | Microsoft Docs
description: "Den här självstudiekursen innehåller stegvisa instruktioner för hur du transformerar data genom att använda en Spark-aktivitet i Azure Data Factory."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/10/2017
ms.author: shengc
ms.openlocfilehash: 93031615b271e542d8832b980a40ca25d1cd6d5c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="transform-data-in-the-cloud-by-using-spark-activity-in-azure-data-factory"></a>Transformera data i molnet genom att använda Spark-aktivitet i Azure Data Factory
Azure Data Factory är en molnbaserad dataintegreringstjänst som gör att du kan skapa datadrivna arbetsflöden i molnet för att samordna och automatisera dataförflyttning och dataomvandling. Med Azure Data Factory kan du skapa och schemalägga datadrivna arbetsflöden (kallas pipelines) som kan föra in data från olika datalager, bearbeta/omvandla data med beräkningstjänster som Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics och Azure Machine Learning och publicera utgående data till datalager som Azure SQL Data Warehouse för BI-program (business intelligence) kan använda. 

I den här självstudien använder du Azure PowerShell för att skapa en Data Factory-pipeline som transformerar data med Spark-aktivitet och en länkad HDInsight-tjänst på begäran. I de här självstudierna går du igenom följande steg:

> [!div class="checklist"]
> * Skapa en datafabrik. 
> * Skapa och distribuera länkade tjänster.
> * Skapa och distribuera en pipeline. 
> * Starta en pipelinekörning.
> * Övervaka pipelinekörningen.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.

## <a name="prerequisites"></a>Krav
* **Azure Storage-konto**. Du skapar ett Python-skript och en indatafil och överför dem till Azure Storage. Spark-programmets utdata lagras på det här lagringskontot. Spark-klustret på begäran använder samma lagringskonto som den primära lagringen.  
* **Azure PowerShell**. Följ instruktionerna i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/install-azurerm-ps).


### <a name="upload-python-script-to-your-blob-storage-account"></a>Överföra Python-skriptet till ditt Blob Storage-konto
1. Skapa en Python-fil med namnet **WordCount_Spark.py** med följande innehåll: 

    ```python
    import sys
    from operator import add
    
    from pyspark.sql import SparkSession
    
    def main():
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
            
        lines = spark.read.text("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/inputfiles/minecraftstory.txt").rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' ')) \
            .map(lambda x: (x, 1)) \
            .reduceByKey(add)
        counts.saveAsTextFile("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/outputfiles/wordcount")
        
        spark.stop()
    
    if __name__ == "__main__":
        main()
    ```
2. Ersätt **&lt;storageAccountName&gt;** med namnet på ditt Azure-konto. Spara sedan filen. 
3. Skapa en behållare med namnet **adftutorial** i Azure Blob Storage om den inte finns. 
4. Skapa en mapp med namnet **spark**.
5. Skapa en undermapp med namnet **script** under mappen **spark**. 
6. Överför filen **WordCount_Spark.py** till undermappen **script**. 


### <a name="upload-the-input-file"></a>Överför indatafilen
1. Skapa en fil med namnet **minecraftstory.txt** med lite text. Spark-programmet räknar antalet ord i texten. 
2. Skapa en undermapp med namnet `inputfiles` i mappen `spark`. 
3. Överför `minecraftstory.txt` till mappen `inputfiles`. 

## <a name="author-linked-services"></a>Skapa länkade tjänster
Du har skapat två länkade tjänster i det här avsnittet: 
    
- Den länkade Azure Storage-tjänsten som länkar ditt Azure Storage-konto till datafabriken. Den här lagringen används av HDInsight-kluster på begäran. Den innehåller också Spark-skriptet som ska köras. 
- En på begäran länkad HDInsight-tjänst. Azure Data Factory skapar automatiskt ett HDInsight-kluster, kör Spark-programmet och tar sedan bort HDInsight-klustret när det har varit inaktivt under en förkonfigurerad tid. 

### <a name="azure-storage-linked-service"></a>Länkad Azure-lagringstjänst
Skapa en JSON-fil med önskat redigeringsprogram, kopiera följande JSON-definition för en länkad Azure Storage-tjänst och spara filen som **MyStorageLinkedService.json**.  

```json
{
    "name": "MyStorageLinkedService",
    "properties": {
      "type": "AzureStorage",
      "typeProperties": {
        "connectionString": {
          "value": "DefaultEndpointsProtocol=https;AccountName=<storageAccountName>;AccountKey=<storageAccountKey>",
          "type": "SecureString"
        }
      }
    }
}
```
Uppdatera &lt;storageAccountName&gt; och &lt;storageAccountKey&gt; med namnet på och nyckeln för ditt Azure Storage-konto. 


### <a name="on-demand-hdinsight-linked-service"></a>På begäran länkad HDInsight-tjänst
Skapa en JSON-fil med önskat redigeringsprogram, kopiera följande JSON-definition för en länkad Azure HDInsight-tjänst och spara filen som **MyOnDemandSparkLinkedService.json**.  

```json
{
    "name": "MyOnDemandSparkLinkedService",
    "properties": {
      "type": "HDInsightOnDemand",
      "typeProperties": {
        "clusterSize": 2,
        "clusterType": "spark",
        "timeToLive": "00:15:00",
        "hostSubscriptionId": "<subscriptionID> ",
        "servicePrincipalId": "<servicePrincipalID>",
        "servicePrincipalKey": {
          "value": "<servicePrincipalKey>",
          "type": "SecureString"
        },
        "tenant": "<tenant ID>",
        "clusterResourceGroup": "<resourceGroupofHDICluster>",
        "version": "3.6",
        "osType": "Linux",
        "clusterNamePrefix":"ADFSparkSample",
        "linkedServiceName": {
          "referenceName": "MyStorageLinkedService",
          "type": "LinkedServiceReference"
        }
      }
    }
}
```
Uppdatera värden för följande egenskaper i definitionen för den länkade tjänsten: 

- **hostSubscriptionId**. Ersätt &lt;subscriptionId&gt; med ID:t för din Azure-prenumeration. Klustret HDInsight på begäran skapas i den här prenumerationen. 
- **klient**. Ersätt &lt;tenantID&gt; med Azure-klientens ID. 
- **servicePrincipalId**, **servicePrincipalKey**. Ersätt &lt;servicePrincipalID&gt; och &lt;servicePrincipalKey&gt; med ID och nyckel för tjänstens huvudnamn i Azure Active Directory. Tjänstens huvudnamn måste vara medlem av prenumerationens deltagarrollen eller resursgruppen som klustret har skapats i. Mer information finns i [create Azure Active Directory application and service principal](../azure-resource-manager/resource-group-create-service-principal-portal.md) (skapa Azure Active Directory-program och ett huvudnamn för tjänsten). 
- **clusterResourceGroup**. Ersätt &lt;resourceGroupOfHDICluster&gt; med namnet på resursgruppen som HDInsight-klustret ska skapas i. 

> [!NOTE]
> Azure HDInsight har en begränsning för hur många kärnor du kan använda i varje Azure-region som stöds. För den HDInsight-länkade tjänsten på begäran skapas HDInsight-klustret på samma plats som Azure Storage använde som primär lagring. Se till att du har tillräckligt med kärnkvoter så att klustret kan skapats på rätt sätt. Mer information finns i [Set up clusters in HDInsight with Hadoop, Spark, Kafka, and more](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) (Konfigurera kluster i HDInsight med Hadoop, Spark, Kafka med mera). 


## <a name="author-a-pipeline"></a>Skapa en pipeline 
I det här steget kan du skapa en ny pipeline med en Spark-aktivitet. Aktiviteten använder exemplet **ordräkning**. Hämta innehållet från den här platsen om du inte redan gjort det.

Skapa en JSON-fil med önskat redigeringsprogram, kopiera följande JSON-definition för en pipelinedefinition och spara filen som **MySparkOnDemandPipeline.json**. 

```json
{
  "name": "MySparkOnDemandPipeline",
  "properties": {
    "activities": [
      {
        "name": "MySparkActivity",
        "type": "HDInsightSpark",
        "linkedServiceName": {
            "referenceName": "MyOnDemandSparkLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
          "rootPath": "adftutorial/spark",
          "entryFilePath": "script/WordCount_Spark.py",
          "getDebugInfo": "Failure",
          "sparkJobLinkedService": {
            "referenceName": "MyStorageLinkedService",
            "type": "LinkedServiceReference"
          }
        }
      }
    ]
  }
}
```

Observera följande punkter: 

- rootPath pekar på Spark-mappen i behållaren adftutorial. 
- entryFilePath pekar på filen WordCount_Spark.py i skriptets undermapp i Spark-mappen. 


## <a name="create-a-data-factory"></a>Skapa en datafabrik 
Du har skapat definitioner för länkad tjänst och pipeline i JSON-filer. Nu ska vi skapa en datafabrik och distribuera den länkade tjänsten och pipeline-JSON-filerna med hjälp av PowerShell-cmdlets. Kör följande PowerShell-kommandon ett i taget: 

1. Ange variabler en i taget.

    ```powershell
    $subscriptionID = "<subscription ID>" # Your Azure subscription ID
    $resourceGroupName = "ADFTutorialResourceGroup" # Name of the resource group
    $dataFactoryName = "MyDataFactory09102017" # Globally unique name of the data factory
    $pipelineName = "MySparkOnDemandPipeline" # Name of the pipeline
    ```
2. Starta **PowerShell**. Låt Azure PowerShell vara öppet tills du är klar med snabbstarten. Om du stänger och öppnar det igen måste du köra kommandona en gång till.

    Kör följande kommando och ange det användarnamn och lösenord som du använder för att logga in i Azure Portal:
        
    ```powershell
    Login-AzureRmAccount
    ```        
    Kör följande kommando för att visa alla prenumerationer för det här kontot:

    ```powershell
    Get-AzureRmSubscription
    ```
    Kör följande kommando för att välja den prenumeration som du vill arbeta med. Ersätt **SubscriptionId** med ID:t för din Azure-prenumeration:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"    
    ```  
3. Skapa resursgruppen: ADFTutorialResourceGroup. 

    ```powershell
    New-AzureRmResourceGroup -Name $resourceGroupName -Location "East Us" 
    ```
4. Skapa datafabriken. 

    ```powershell
     $df = Set-AzureRmDataFactoryV2 -Location EastUS -Name $dataFactoryName -ResourceGroupName $resourceGroupName
    ```

    Kör följande kommando för att se utdata: 

    ```powershell
    $df
    ```
5. Växla till den mapp där du skapade JSON-filerna och kör följande kommando för att distribuera en länkad Azure Storage-tjänst: 
       
    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "MyStorageLinkedService" -File "MyStorageLinkedService.json"
    ```
6. Kör följande kommando för att distribuera en länkad Spark-tjänst på begäran: 
       
    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "MyOnDemandSparkLinkedService" -File "MyOnDemandSparkLinkedService.json"
    ```
7. Kör följande kommando för att distribuera en pipeline: 
       
    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name $pipelineName -File "MySparkOnDemandPipeline.json"
    ```
    
## <a name="start-and-monitor-a-pipeline-run"></a>Starta och övervaka en pipelinekörning  

1. Starta en pipelinekörning. Den samlar även in pipelinekörningens ID för kommande övervakning.

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName $pipelineName  
    ```
2. Kör följande skript för att kontinuerligt kontrollera pipelinekörningens status tills den är klar.

    ```powershell
    while ($True) {
        $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    
        if(!$result) {
            Write-Host "Waiting for pipeline to start..." -foregroundcolor "Yellow"
        }
        elseif (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
            Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
        }
        else {
            Write-Host "Pipeline '"$pipelineName"' run finished. Result:" -foregroundcolor "Yellow"
            $result
            break
        }
        ($result | Format-List | Out-String)
        Start-Sleep -Seconds 15
    }

    Write-Host "Activity `Output` section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"

    Write-Host "Activity `Error` section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n" 
    ```  
3. Här är utdata för exempelkörningen: 

    ```
    Pipeline run status: In Progress
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : 
    ActivityName      : MySparkActivity
    PipelineRunId     : 94e71d08-a6fa-4191-b7d1-cf8c71cb4794
    PipelineName      : MySparkOnDemandPipeline
    Input             : {rootPath, entryFilePath, getDebugInfo, sparkJobLinkedService}
    Output            : 
    LinkedServiceName : 
    ActivityRunStart  : 9/20/2017 6:33:47 AM
    ActivityRunEnd    : 
    DurationInMs      : 
    Status            : InProgress
    Error             :
    …
    
    Pipeline ' MySparkOnDemandPipeline' run finished. Result:
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : MyDataFactory09102017
    ActivityName      : MySparkActivity
    PipelineRunId     : 94e71d08-a6fa-4191-b7d1-cf8c71cb4794
    PipelineName      : MySparkOnDemandPipeline
    Input             : {rootPath, entryFilePath, getDebugInfo, sparkJobLinkedService}
    Output            : {clusterInUse, jobId, ExecutionProgress, effectiveIntegrationRuntime}
    LinkedServiceName : 
    ActivityRunStart  : 9/20/2017 6:33:47 AM
    ActivityRunEnd    : 9/20/2017 6:46:30 AM
    DurationInMs      : 763466
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    Activity Output section:
    "clusterInUse": "https://ADFSparkSamplexxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.azurehdinsight.net/"
    "jobId": "0"
    "ExecutionProgress": "Succeeded"
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)"
    Activity Error section:
    "errorCode": ""
    "message": ""
    "failureType": ""
    "target": "MySparkActivity"
    ```
4. Bekräfta att en mapp med namnet `outputfiles` har skapats i mappen `spark` i adftutorial-behållaren med utdata från Spark-programmet. 


## <a name="next-steps"></a>Nästa steg
Pipeline i det här exemplet kopierar data från en plats till en annan i Azure Blob Storage. Du har lärt dig att: 

> [!div class="checklist"]
> * Skapa en datafabrik. 
> * Skapa och distribuera länkade tjänster.
> * Skapa och distribuera en pipeline. 
> * Starta en pipelinekörning.
> * Övervaka pipelinekörningen.

Gå vidare till nästa självstudiekurs för att lära dig hur du transformerar data genom att köra Hive-skript på ett Azure HDInsight-kluster som är i ett virtuellt nätverk. 

> [!div class="nextstepaction"]
> [Självstudie: transformera data med Hive i Azure Virtual Network](tutorial-transform-data-hive-virtual-network.md).





