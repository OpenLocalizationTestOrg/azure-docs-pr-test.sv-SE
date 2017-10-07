---
title: "aaaCompute miljöer som stöds av Azure Data Factory | Microsoft Docs"
description: "Mer information om beräkning miljöer där du kan använda i Azure Data Factory pipelines (till exempel Azure HDInsight) tootransform eller process."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6877a7e8-1a58-4cfb-bbd3-252ac72e4145
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/25/2017
ms.author: shlo
ms.openlocfilehash: aba7d7de695bc1c7d475f1e741ee3b3e884151c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Compute-miljöer som stöds av Azure Data Factory
Den här artikeln beskrivs olika beräknings-miljöer som du kan använda tooprocess eller Transformera data. Det ger också information om olika konfigurationer (på begäran eller ta med din egen) som stöds av Data Factory när du konfigurerar länkade tjänster länka dessa beräkningar miljöer tooan Azure data factory.

hello innehåller följande tabell en lista över compute-miljöer som stöds av Data Factory och hello aktiviteter som kan köras på dem. 

| Compute-miljö | activities |
| --- | --- |
| [HDInsight-kluster på begäran](#azure-hdinsight-on-demand-linked-service) eller [egna HDInsight-kluster](#azure-hdinsight-linked-service) |[DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop-strömning](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](#azure-batch-linked-service) |[DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) |[Machine Learning-aktiviteter: batchkörning och resursuppdatering](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) |[Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](#azure-sql-linked-service), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-linked-service), [SQLServer](#sql-server-linked-service) |[Lagrad procedur](data-factory-stored-proc-activity.md) |

## <a name="supported-hdinsight-versions-in-azure-data-factory"></a>HDInsight-versioner som stöds i Azure Data Factory
Azure HDInsight har stöd för flera Hadoop-klusterversioner som kan distribueras när som helst. Varje version alternativ skapas en viss version av hello Hortonworks Data Platform (HDP) distribution och en uppsättning komponenter som ingår i distributionen. Microsoft håller uppdaterar hello lista över versioner som stöds för HDInsight tooprovide senaste Hadoop-ekosystemet komponenter och korrigeringar. Hej HDInsight 3.2 är föråldrad på 1 April 2017. Detaljerad information finns i [HDInsight-versioner som stöds](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

Detta påverkar befintliga Azure-Datafabriker som har aktiviteter som körs mot 3.2 HDInsight-kluster. Vi rekommenderar att användare toofollow hello riktlinjerna i följande avsnitt tooupdate hello hello påverkas Datafabriker:

### <a name="for-linked-services-pointing-tooyour-own-hdinsight-clusters"></a>För länkade tjänster pekar tooyour egna HDInsight-kluster
* **HDInsight länkade tjänster pekar tooyour äger HDInsight 3.2 eller nedan kluster:**

  Azure Data Factory stöder skickar jobb tooyour egna HDInsight-kluster från HDI 3.1 för[hello senaste stöds HDInsight version](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions). Du kan dock inte längre skapa HDInsight 3.2 klustret när 1 April 2017 baserat på hello utfasningspolicy dokumenterade i [HDInsight-versioner som stöds](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).  

  **Rekommendationer:** 
  * Utföra tester tooensure hello kompatibilitet för hello aktiviteter som refererar till den här länkade tjänster för[hello senaste stöds HDInsight version](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) med information som beskrivs i [Hadoop-komponenter som är tillgängliga med olika versioner av HDInsight](../hdinsight/hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions) och [Hortonworks viktig information som är associerade med HDInsight-versioner](../hdinsight/hdinsight-component-versioning.md#hortonworks-release-notes-associated-with-hdinsight-versions).
  * Uppgradera 3.2 HDInsight-kluster för[hello senaste stöds HDInsight version](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) tooget hello senaste komponenterna i Hadoop-ekosystemet och korrigeringar. 

* **HDInsight länkade tjänster pekar tooyour äger HDInsight 3.3 eller över kluster:**

  Azure Data Factory stöder skickar jobb tooyour egna HDInsight-kluster från HDI 3.1 för[hello senaste stöds HDInsight version](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions). 
  
  **Rekommendationer:** 
  * Ingen åtgärd krävs från Data Factory perspektiv. Om du har en tidigare version av HDInsight, rekommenderar vi dock fortfarande uppgradering för[hello senaste stöds HDInsight version](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) tooget hello senaste komponenterna i Hadoop-ekosystemet och korrigeringar.

### <a name="for-hdinsight-on-demand-linked-services"></a>För HDInsight på begäran länkade tjänster
* **Version 3.2 eller nedan anges i HDInsight på begäran länkade tjänster JSON-definitionen:**
  
  Azure Data Factory stöder skapandet av på begäran HDInsight-kluster av version 3.3 eller flera från **2017-05/15** och senare. Och hello slutet av stödet för befintliga på begäran HDInsight 3.2 länkade tjänster är utökat för**2017-07/15**.  

  **Rekommendationer:** 
  * Utföra tester tooensure hello kompatibilitet för hello aktiviteter som refererar till den här länkade tjänster för [hello senaste stöds HDInsight version](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) med information som beskrivs i [Hadoop-komponenter som är tillgängliga med olika versioner av HDInsight](../hdinsight/hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions) och [Hortonworks viktig information som är associerade med HDInsight-versioner](../hdinsight/hdinsight-component-versioning.md#hortonworks-release-notes-associated-with-hdinsight-versions).
  * Innan du **2017-07/15**, uppdatera hello versionsegenskapen på begäran HDI länkade tjänsten JSON-definitionen för[hello senaste stöds HDInsight version](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) tooget hello senaste Hadoop-ekosystemet komponenterna och korrigeringar. Detaljerad JSON-definitionen finns toohello [Azure på begäran länkad HDInsight-tjänst exempel](#azure-hdinsight-on-demand-linked-service). 

* **Version som inte har angetts i på begäran HDInsight länkade tjänster:**
  
  Azure Data Factory stöder skapandet av på begäran HDInsight-kluster av version 3.3 eller flera från **2017-05/15** och senare. Och hello slutet av HDInsight 3.2 länkade supporttjänster tooexisting på begäran har utökats för**2017-07/15**. 

  Innan du **2017-07/15**, om tomt hello standardvärdena för version och osType egenskaper är: 

  | Egenskap | Standardvärde | Krävs |
  | --- | --- | --- |
  Version   | HDI 3.1 för Windows-kluster och HDI 3.2 för Linux-kluster.| Nej
  osType | hello standardvärdet är Windows | Nej

  Efter **2017-07/15**, om tomt hello standardvärdena för version och osType egenskaper är:

  | Egenskap | Standardvärde | Krävs |
  | --- | --- | --- |
  Version   | HDI 3.3 för Windows-kluster och 3.5 för Linux-kluster.    | Nej
  osType | hello standard är Linux   | Nej

  **Rekommendationer:** 
  * Innan du **2017-07/15**, utföra tester tooensure hello kompatibilitet för hello aktiviteter som refererar till den här länkade tjänster för[hello senaste stöds HDInsight version](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) med information som dokumenteras i [Hadoop-komponenter som är tillgängliga med olika versioner av HDInsight](../hdinsight/hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions) och [Hortonworks viktig information som är associerade med HDInsight-versioner](../hdinsight/hdinsight-component-versioning.md#hortonworks-release-notes-associated-with-hdinsight-versions).  
  * Efter **2017-07/15**, kontrollera att du anger explicit osType och version värden om du vill att toooverride hello standardinställningarna. 

>[!Note]
>Azure Data Factory stöder för närvarande inte HDInsight-kluster med Azure Data Lake Store som primär store. Använd Azure Storage som primär lagringsplats för HDInsight-kluster. 
>  
>  

## <a name="on-demand-compute-environment"></a>På begäran beräknings-miljö
I den här typen av konfiguration av hello datormiljö fullständigt hello Azure Data Factory-tjänsten. Det automatiskt skapas av hello Data Factory-tjänsten innan ett jobb har skickats tooprocess data och tas bort när hello jobbet har slutförts. Du kan skapa en länkad tjänst för hello på begäran beräkning miljö, konfigurera den och styra detaljerade inställningar för jobbkörningen klusterhantering och startprogram åtgärder.

> [!NOTE]
> hello på begäran-konfiguration stöds för närvarande endast för Azure HDInsight-kluster.
> 
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure HDInsight på begäran länkade tjänsten
hello Azure Data Factory-tjänsten kan automatiskt skapa ett Windows/Linux-baserade på begäran HDInsight-kluster tooprocess data. hello klustret skapas i samma region som lagringskontot för hello (linkedServiceName-egenskapen i hello JSON) som är associerade med klustret hello hello. hello storage-konto måste vara en generell Azure storage-standardkonto. 

Observera följande hello **viktiga** punkter om på begäran HDInsight länkade tjänsten:

* Du ser inte hello på begäran HDInsight-kluster skapas i din Azure-prenumeration. hello Azure Data Factory-tjänsten hanterar hello på begäran HDInsight-kluster för din räkning.
* hello kopieras loggar för jobb som körs på en på-begäran HDInsight kluster toohello storage-konto som är associerade med hello HDInsight-kluster. Du kan komma åt dessa loggar från hello Azure-portalen i hello **kör aktivitetsinformation** bladet. Se [övervaka och hantera Pipelines](data-factory-monitor-manage-pipelines.md) artikeln för information.
* Du debiteras endast för hello tid när hello HDInsight-kluster är igång och jobb som körs.

> [!IMPORTANT]
> Det tar vanligtvis **20 minuter** eller mer tooprovision ett Azure HDInsight-kluster på begäran.
> 
> 

### <a name="example"></a>Exempel
hello följande JSON definierar en Linux-baserade på begäran HDInsight länkad tjänst. hello Data Factory-tjänsten skapar automatiskt en **Linux-baserade** vid bearbetning av en datasektorn HDInsight-kluster. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

Ange toouse ett Windows-baserade HDInsight-kluster **osType** för**windows** eller Använd inte hello-egenskap som är standardvärdet för hello: windows.  

> [!IMPORTANT]
> Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (**linkedServiceName**). HDInsight tar inte bort den här behållaren när hello kluster har tagits bort. Det här beteendet är avsiktligt. Med länkad HDInsight-tjänsten på begäran, ett HDInsight-kluster skapas varje gång ett segment måste toobe bearbetas såvida det inte finns ett befintligt live kluster (**timeToLive**) och tas bort när hello bearbetningen är klar. 
> 
> Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage. Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad. hello namnen på de här behållarna följer ett mönster: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.
> 
> 

### <a name="properties"></a>Egenskaper
| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello typegenskapen ska anges för**HDInsightOnDemand**. |Ja |
| ClusterSize |Antalet worker/data noder i klustret hello. Hej HDInsight-kluster skapas med 2 huvudnoderna tillsammans med hello antalet arbetarnoder som du anger för den här egenskapen. hello noder har storlek Standard_D3 med 4 kärnor, så ett kluster med noder 4 worker tar 24 kärnor (4\*4 = 16 kärnor för arbetarnoder plus 2\*4 = 8 kärnor för huvudnoderna). Se [skapa Linux-baserade Hadoop-kluster i HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) för ytterligare information om hello Standard_D3 nivå. |Ja |
| TimeToLive |hello tillåten inaktivitetstid för hello på begäran HDInsight-kluster. Anger hur länge hello på begäran HDInsight-kluster förblir aktiv efter slutförande av en aktivitet som kör om det finns inga aktiva jobb i hello klustret.<br/><br/>Om en aktivitet som kör tar 6 minuter och timetolive är exempelvis ange too5 minuter hello kluster förblir alive för 5 minuter efter hello 6 minuter hello aktiviteten körs. Om en annan aktivitet kör körs med hello 6 minuter fönster, men det bearbetas av hello samma kluster.<br/><br/>Skapar ett HDInsight-kluster på begäran är en kostsam åtgärd (kan ta en stund), så Använd den här inställningen som behövs tooimprove prestanda för en datafabrik genom att återanvända ett HDInsight-kluster på begäran.<br/><br/>Om du ställer in timetolive värdet too0 tas hello klustret bort när hello aktiviteten kör har slutförts. Men om du anger ett högt värde kan hello kluster förblir inaktiva i onödan ledde höga kostnader. Det är därför viktigt att du ställer in hello lämpligt värde baserat på dina behov.<br/><br/>Om hello timetolive egenskapens värde är korrekt, kan flera pipelines dela hello instans av hello på begäran HDInsight-kluster.  |Ja |
| Version |Version av hello HDInsight-kluster. hello standardvärdet är 3.1 för Windows-kluster och 3.2 för Linux-kluster. |Nej |
| linkedServiceName | Azure Storage länkade tjänsten toobe som används av hello på begäran klustret för lagring och bearbetning av data. Hej HDInsight-kluster skapas i hello samma region som Azure Storage-konto.<p>För närvarande kan du skapa ett HDInsight-kluster med på begäran som använder ett Azure Data Lake Store som hello lagring. Om du vill toostore hello Resultatdata från HDInsight som bearbetas i en Azure Data Lake Store kan använda en Kopieringsaktiviteten toocopy hello data från hello Azure Blob Storage toohello Azure Data Lake Store. </p>  | Ja |
| additionalLinkedServiceNames |Anger ytterligare lagringskonton för hello HDInsight länkade tjänsten så att hello Data Factory-tjänsten kan registrera dem å dina vägnar. Dessa storage-konton måste vara i hello samma region som hello HDInsight-kluster som skapas i hello samma region som anges av linkedServiceName hello storage-konto. |Nej |
| osType |Typ av operativsystem. Tillåtna värden är: (standard) för Windows och Linux |Nej |
| hcatalogLinkedServiceName |hello namnet på Azure SQL-länkade tjänsten punkt toohello HCatalog databasen. hello på begäran HDInsight-kluster skapas med hjälp av hello Azure SQL-databas som hello metastore. |Nej |

#### <a name="additionallinkedservicenames-json-example"></a>additionalLinkedServiceNames JSON-exempel

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>Avancerade egenskaper
Du kan också ange hello följande egenskaper för hello detaljerade konfigurationen av hello på begäran HDInsight-kluster.

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| coreConfiguration |Anger hello core konfigurationsparametrar (som core-site.xml) för hello HDInsight-kluster toobe skapas. |Nej |
| hBaseConfiguration |Anger hello HBase konfigurationsparametrar (hbase-site.xml) för hello HDInsight-kluster. |Nej |
| hdfsConfiguration |Anger hello HDFS konfigurationsparametrar (hdfs-site.xml) för hello HDInsight-kluster. |Nej |
| hiveConfiguration |Anger hello hive konfigurationsparametrar (hive-site.xml) för hello HDInsight-kluster. |Nej |
| mapReduceConfiguration |Anger hello MapReduce konfigurationsparametrar (mapred site.xml) för hello HDInsight-kluster. |Nej |
| oozieConfiguration |Anger hello Oozie konfigurationsparametrar (oozie-site.xml) för hello HDInsight-kluster. |Nej |
| stormConfiguration |Anger hello Storm konfigurationsparametrar (storm-site.xml) för hello HDInsight-kluster. |Nej |
| yarnConfiguration |Anger hello Yarn konfigurationsparametrar (yarn-site.xml) för hello HDInsight-kluster. |Nej |

#### <a name="example--on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Exempel – på begäran HDInsight klusterkonfigurationen med avancerade egenskaper

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

### <a name="node-sizes"></a>Noden storlekar
Du kan ange hello storlekar på head, data och zookeeper-noder som använder hello följande egenskaper: 

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| headNodeSize |Anger hello storleken på hello huvudnod. hello standardvärdet är: Standard_D3. Se hello **ange nod storlekar** information. |Nej |
| dataNodeSize |Anger hello storleken på hello datanod. hello standardvärdet är: Standard_D3. |Nej |
| zookeeperNodeSize |Anger hello storleken på hello djurskötare nod. hello standardvärdet är: Standard_D3. |Nej |

#### <a name="specifying-node-sizes"></a>Anger att noden storlekar
Se hello [storlekar för virtuella datorer](../virtual-machines/linux/sizes.md) strängvärden som du behöver toospecify för hello egenskaper som beskrivs i föregående avsnitt i hello-artikel. hello-värden måste tooconform toohello **CMDLETs & API: er** refereras i hello artikel. Som du ser i hello artikeln har hello datanoden i stora (standard) storlek 7 GB minne, vilket inte kanske är bra för ditt scenario. 

Om du vill toocreate D4 storlek huvudnoderna och arbetsnoder anger **Standard_D4** som hello värdet för egenskaperna headNodeSize och dataNodeSize. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Om du anger ett felaktigt värde för dessa egenskaper, kan du få hello följande **fel:** misslyckades toocreate klustret. Undantag: Det går inte toocomplete hello kluster Skapa-åtgärd. Operation failed with code '400'. (Åtgärden misslyckades med koden 400). Cluster left behind state: 'Error'. (Klustret efterlämnade status: Fel.) Meddelande: 'PreClusterCreationValidationFailure'. När du får det här felet kan du se till att du använder hello **CMDLET & API: er** namn från hello tabell i hello [storlekar för virtuella datorer](../virtual-machines/linux/sizes.md) artikel.  

## <a name="bring-your-own-compute-environment"></a>Ta med din egen beräknings-miljö
I den här typen av konfiguration kan användare registrera en redan befintlig datormiljö som en länkad tjänst i Data Factory. hello datormiljö hanteras av hello användare och hello Data Factory-tjänsten använder den tooexecute hello aktiviteter.

Den här typen av konfiguration stöds för följande hello compute miljöer:

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL DB, Azure SQL DW, SQLServer

## <a name="azure-hdinsight-linked-service"></a>Azure HDInsight länkade tjänsten
Du kan skapa ett Azure HDInsight länkade tjänsten tooregister ditt eget kluster i HDInsight med Data Factory.

### <a name="example"></a>Exempel

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

### <a name="properties"></a>Egenskaper
| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello typegenskapen ska anges för**HDInsight**. |Ja |
| clusterUri |hello hello HDInsight-kluster-URI. |Ja |
| användarnamn |Ange hello namnet hello användaren toobe används tooconnect tooan befintligt HDInsight-kluster. |Ja |
| lösenord |Ange lösenordet för användarkontot för hello. |Ja |
| linkedServiceName | Namnet på hello länkad Azure Storage-tjänst som refererar toohello Azure blob-lagring som används av hello HDInsight-kluster. <p>För närvarande kan du ange ett Azure Data Lake Store länkade tjänsten för den här egenskapen. Om hello HDInsight-kluster har åtkomst toohello Data Lake Store, kan du komma åt data i hello Azure Data Lake Store från Hive/Pig-skript. </p>  |Ja |

## <a name="azure-batch-linked-service"></a>Azure Batch länkade tjänsten
Du kan skapa ett Azure Batch länkade tjänsten tooregister Batch-pool för virtuella datorer (VM) tooa data factory. Du kan köra .NET anpassade aktiviteter med hjälp av Azure Batch eller Azure HDInsight.

Se följande avsnitt om du är ny tooAzure Batch-tjänsten:

* [Grunderna i Azure Batch](../batch/batch-technical-overview.md) en översikt över hello Azure Batch-tjänsten.
* [Nya AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) cmdlet toocreate Azure Batch-kontot (eller) [Azure-portalen](../batch/batch-account-create-portal.md) toocreate hello Azure Batch-kontot med hjälp av Azure portal. Se [med PowerShell toomanage Azure Batch-kontot](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) avsnittet detaljerade anvisningar om hur du använder hello cmdlet.
* [Nya AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) cmdlet toocreate Azure Batch-pool.

### <a name="example"></a>Exempel

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

Lägg till ”**.\< regionsnamnet\>**”toohello namnet på batch-kontot för hello **accountName** egenskapen. Exempel:

```json
"accountName": "mybatchaccount.eastus"
```

Ett annat alternativ är tooprovide hello batchUri slutpunkt som visas i följande exempel hello:

```json
"accountName": "adfteam",
"batchUri": "https://eastus.batch.azure.com",
```

### <a name="properties"></a>Egenskaper
| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello typegenskapen ska anges för**AzureBatch**. |Ja |
| Kontonamn |Namnet på hello Azure Batch-kontot. |Ja |
| accessKey |Åtkomstnyckeln för hello Azure Batch-kontot. |Ja |
| Poolnamn |Namnet på hello pool för virtuella datorer. |Ja |
| linkedServiceName |Namnet på hello länkad Azure Storage-tjänst som är associerad med den här Azure Batch länkade tjänsten. Den här länkade tjänsten används för mellanlagring av filer krävs toorun hello aktivitet och lagra hello aktivitetsloggar för körning. |Ja |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning länkade tjänsten
Skapa en Azure Machine Learning länkade tjänsten tooregister en slutpunkt tooa data factory för Machine Learning av batchbedömningsjobbet.

### <a name="example"></a>Exempel

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

### <a name="properties"></a>Egenskaper
| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| Typ |hello typegenskapen ska anges till: **AzureML**. |Ja |
| mlEndpoint |Hej batchbedömningsjobbet URL. |Ja |
| apiKey |hello publicerade arbetsytemodellens API. |Ja |

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics länkade tjänsten
Du skapar en **Azure Data Lake Analytics** länkade tjänsten toolink ett Azure Data Lake Analytics beräkning service tooan Azure data factory. hello Data Lake Analytics U-SQL-aktivitet i pipelinen hello refererar toothis länkade tjänsten. 

hello innehåller följande tabell beskrivningar hello allmänna egenskaper som används i hello JSON-definitionen. Du kan ytterligare välja mellan tjänstens huvudnamn och autentiseringsuppgifter för användarautentisering.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| **typ** |hello typegenskapen ska anges till: **AzureDataLakeAnalytics**. |Ja |
| **Kontonamn** |Azure Data Lake Analytics-kontonamn. |Ja |
| **dataLakeAnalyticsUri** |Azure Data Lake Analytics-URI. |Nej |
| **prenumerations-ID** |Azure prenumerations-id |Nej (om inte anges prenumeration hello datafabriken används). |
| **resourceGroupName** |Azure resursgruppens namn |Nej (om inte anges resursgruppen av hello datafabriken används). |

### <a name="service-principal-authentication-recommended"></a>Tjänstens huvudnamn autentisering (rekommenderas)
toouse service principal autentisering, registrera en Programenhet i Azure Active Directory (Azure AD) och bevilja den hello komma åt tooData Lake Store. Detaljerade anvisningar finns i [tjänst-till-tjänst autentisering](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Anteckna hello följande värden som du använder toodefine hello länkade tjänsten:
* Program-ID:t
* Nyckeln för programmet 
* Klient-ID:t

Använd service principal autentisering genom att ange hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| **servicePrincipalId** | Ange hello programmets klient-ID. | Ja |
| **servicePrincipalKey** | Ange hello programnyckel. | Ja |
| **klient** | Ange hello klient information (domain name eller klient ID) under där programmet finns. Du kan hämta den med hovra hello musen i hello övre högra hörnet av hello Azure-portalen. | Ja |

**Exempel: Service principal autentisering**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Användarautentisering för autentiseringsuppgifter
Alternativt kan du använda användarautentisering för autentiseringsuppgifter för Data Lake Analytics genom att ange hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| **auktorisering** | Klicka på hello **auktorisera** i hello Data Factory-redigeraren och ange dina autentiseringsuppgifter som tilldelar hello automatiskt genererade auktorisering URL toothis-egenskapen. | Ja |
| **sessions-ID** | OAuth sessions-ID från hello OAuth-auktorisering session. Varje sessions-ID är unikt och kan bara användas en gång. Den här inställningen genereras automatiskt när du använder hello Data Factory-redigeraren. | Ja |

**Exempel: Användarautentisering för autentiseringsuppgifter**
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
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Token upphör att gälla
Hej auktoriseringskod som du genererade med hjälp av hello **auktorisera** knappen upphör att gälla efter en stund. Se hello i den följande tabellen för hello upphör att gälla tidpunkter för olika typer av användarkonton. Kan du se hello följande felmeddelande när hello autentisering **token upphör att gälla**: autentiseringsuppgifter fel: invalid_grant - AADSTS70002: fel vid verifiering av autentiseringsuppgifter. AADSTS70008: hello angetts åtkomsten har upphört att gälla eller återkallats. Trace-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelations-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tidsstämpel: 2015-12-15 21:09:31Z

| Användartyp | Upphör att gälla efter |
|:--- |:--- |
| Användarkonton som inte hanteras av Azure Active Directory (@hotmail.com, @live.comosv.) |12 timmar |
| Användarkonton som hanteras av Azure Active Directory (AAD) |14 dagar efter hello sista segmentet kör. <br/><br/>90 dagar, om ett segment baserat på OAuth-baserad länkade tjänst körs på minst en gång var fjortonde dag. |

tooavoid/Lös det här felet, omauktorisera med hello **auktorisera** knappen när hello **token upphör att gälla** och distribuera hello länkade tjänsten. Du kan också generera värdena för **sessionId** och **auktorisering** egenskaper programmässigt med hjälp av koden på följande sätt:

```csharp
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

Se [AzureDataLakeStoreLinkedService klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), och [AuthorizationSessionGetResponse klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) avsnitt hittar du information Om hello Data Factory-klasser används i hello-koden. Lägg till en referens till: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll för hello WindowsFormsWebAuthenticationDialog klass. 

## <a name="azure-sql-linked-service"></a>Azure SQL länkade tjänsten
Du skapar en Azure SQL-länkade tjänst och använda den med hello [lagrade Proceduraktiviteten](data-factory-stored-proc-activity.md) tooinvoke en lagrad procedur från Data Factory-pipelinen. Se [Azure SQL Connector](data-factory-azure-sql-connector.md#linked-service-properties) artikeln för information om den här länkade tjänsten.

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL Data Warehouse länkad tjänst
Du skapar en länkad Azure SQL Data Warehouse-tjänst och använda den med hello [lagrade Proceduraktiviteten](data-factory-stored-proc-activity.md) tooinvoke en lagrad procedur från Data Factory-pipelinen. Se [Azure SQL Data Warehouse-Anslutningsapp](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) artikeln för information om den här länkade tjänsten.

## <a name="sql-server-linked-service"></a>SQLServer länkade tjänsten
Du skapar en SQL Server som är länkad tjänst och använda den med hello [lagrade Proceduraktiviteten](data-factory-stored-proc-activity.md) tooinvoke en lagrad procedur från Data Factory-pipelinen. Se [SQL Server-anslutningen](data-factory-sqlserver-connector.md#linked-service-properties) artikeln för information om den här länkade tjänsten.

