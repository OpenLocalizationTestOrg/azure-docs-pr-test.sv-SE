---
title: "aaaAzure Data Factory - vanliga frågor och svar"
description: "Vanliga frågor om Azure Data Factory."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 78289fb4b6e15d74772af6c71ec25c7d2ca1a0bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---frequently-asked-questions"></a>Azure Data Factory - vanliga frågor och svar
## <a name="general-questions"></a>Allmänna frågor
### <a name="what-is-azure-data-factory"></a>Vad är Azure Data Factory?
Data Factory är en molnbaserad dataintegrering tjänsten som **automatiserar hello rörelser och transformering av data**. Precis som en fabrik som kör utrustning tootake raw material och omvandla dem till färdiga varor samordnar Data Factory befintliga tjänster som samlar in rådata och omvandla det till färdiga att använda information.

Data Factory kan toocreate datadrivna arbetsflöden toomove data mellan både lokalt och molnet datalager samt processen/Transformera data med hjälp av beräknings-tjänster som Azure HDInsight och Azure Data Lake Analytics. När du har skapat en pipeline som utför hello-åtgärd som du behöver schemalägga du den toorun regelbundet (varje timme, varje dag, varje vecka osv.).   

Mer information finns i [översikt & nyckelkoncept](data-factory-introduction.md).

### <a name="where-can-i-find-pricing-details-for-azure-data-factory"></a>Var hittar jag prisinformation för Azure Data Factory?
Se [Data Factory-prisinformation sidan] [ adf-pricing-details] för hello prisinformation för hello Azure Data Factory.  

### <a name="how-do-i-get-started-with-azure-data-factory"></a>Hur kommer jag igång med Azure Data Factory?
* En översikt över Azure Data Factory finns [introduktion tooAzure Data Factory](data-factory-introduction.md).
* En självstudiekurs om hur för**kopiera/flytta data** med Kopieringsaktivitet kan visa [kopiera data från Azure Blob Storage tooAzure SQL-databas](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* En självstudiekurs om hur för**Transformera data** använder HDInsight Hive-aktivitet. Se [bearbeta data genom att köra Hive-skript på Hadoop-kluster](data-factory-build-your-first-pipeline.md)

### <a name="what-is-hello-data-factorys-region-availability"></a>Vad är hello Data Factory regional tillgänglighet?
Data Factory finns i **oss West** och **Nordeuropa**. Hej beräkning och lagringstjänster som används av datafabriker kan vara i andra regioner. Se [regioner som stöds](data-factory-introduction.md#supported-regions).

### <a name="what-are-hello-limits-on-number-of-data-factoriespipelinesactivitiesdatasets"></a>Vad är hello gränsen för antalet data fabriker/pipelines/aktiviteter/datauppsättningar?
Se **Azure Data Factory gränser** avsnitt i hello [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md#data-factory-limits) artikel.

### <a name="what-is-hello-authoringdeveloper-experience-with-azure-data-factory-service"></a>Vad är hello redigering/utvecklare erfarenhet av Azure Data Factory-tjänsten?
Du kan redigera/skapa datafabriker med någon av följande verktyg/SDK hello:

* **Azure-portalen** hello Data Factory-blad i hello Azure-portalen ger omfattande användargränssnitt du toocreate data fabriker ad länkade tjänster. Hej **Data Factory-redigeraren**, som också är en del av hello-portalen kan du tooeasily Skapa länkade tjänster, tabeller, datauppsättningar och pipelines genom att ange JSON definitioner för dessa artefakter. Se [skapa din första pipeline för data med hjälp av Azure portal](data-factory-build-your-first-pipeline-using-editor.md) för ett exempel på hur hello portal och redigerare toocreate och distribuera en datafabrik.
* **Visual Studio** du kan använda Visual Studio toocreate ett Azure data factory. Se [skapa din första pipeline för data med hjälp av Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) mer information.
* **Azure PowerShell** finns [skapa och övervaka Azure Data Factory med Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md) stegvisa självstudier/för att skapa en datafabrik med hjälp av PowerShell. Se [Data Factory-Cmdlet-referens] [ adf-powershell-reference] innehållet i MSDN Library för en omfattande dokumentation av Data Factory-cmdlets.
* **.NET-klassbibliotek** programmässigt kan du skapa datafabriker med Data Factory .NET SDK. Se [skapa, övervaka och hantera datafabriker med .NET SDK](data-factory-create-data-factories-programmatically.md) för en genomgång av hur du skapar en datafabrik med .NET SDK. Se [Data Factory klassen biblioteksreferens] [ msdn-class-library-reference] för en omfattande Data Factory .NET SDK-dokumentationen.
* **REST API** du kan också använda hello REST-API som exponeras av hello Azure Data Factory-tjänsten toocreate och distribuera datafabriker. Se [Data Factory REST API-referens] [ msdn-rest-api-reference] för en omfattande Data Factory REST API-dokumentationen.
* **Azure Resource Manager-mall** finns [Självstudier: skapa din första Azure data factory med Azure Resource Manager-mall](data-factory-build-your-first-pipeline-using-arm.md) för information.

### <a name="can-i-rename-a-data-factory"></a>Kan jag byta namn på en datafabrik?
Nej. Precis som andra Azure-resurser kan inte hello namn i ett Azure data factory ändras.

### <a name="can-i-move-a-data-factory-from-one-azure-subscription-tooanother"></a>Kan jag flytta en datafabrik från en Azure-prenumeration tooanother?
Ja. Använd hello **flytta** knappen på din data factory-bladet som visas i följande diagram hello:

![Flytta data factory](media/data-factory-faq/move-data-factory.png)

### <a name="what-are-hello-compute-environments-supported-by-data-factory"></a>Vad är hello beräkning miljöer som stöds av Data Factory?
hello innehåller följande tabell en lista över compute-miljöer som stöds av Data Factory och hello aktiviteter som kan köras på dem.

| Compute-miljö | activities |
| --- | --- |
| [HDInsight-kluster på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) eller [egna HDInsight-kluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) |[DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop-strömning](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](data-factory-compute-linked-services.md#azure-batch-linked-service) |[DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](data-factory-compute-linked-services.md#azure-machine-learning-linked-service) |[Machine Learning-aktiviteter: batchkörning och resursuppdatering](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](data-factory-compute-linked-services.md#azure-data-lake-analytics-linked-service) |[Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](data-factory-compute-linked-services.md#azure-sql-linked-service), [Azure SQL Data Warehouse](data-factory-compute-linked-services.md#azure-sql-data-warehouse-linked-service), [SQLServer](data-factory-compute-linked-services.md#sql-server-linked-service) |[Lagrad procedur](data-factory-stored-proc-activity.md) |

### <a name="how-does-azure-data-factory-compare-with-sql-server-integration-services-ssis"></a>Hur jämfört med SQL Server Integration Services (SSIS) i Azure Data Factory? 
Se hello [jämfört med Azure Data Factory. SSIS](http://www.sqlbits.com/Sessions/Event15/Azure_Data_Factory_vs_SSIS) presentation från en av våra MVP (mest värdefulla personal): Reza Rad. Vissa av hello ändringar i Data Factory får inte anges i hello-bildspel. Vi kontinuerligt lägger till flera funktioner tooAzure Data Factory. Vi kontinuerligt lägger till flera funktioner tooAzure Data Factory. Vi kommer införliva de här uppdateringarna i hello jämförelse av data integration teknik från Microsoft stund senare i år.   

## <a name="activities---faq"></a>Aktiviteter - vanliga frågor och svar
### <a name="what-are-hello-different-types-of-activities-you-can-use-in-a-data-factory-pipeline"></a>Vad är hello olika typer av aktiviteter som du kan använda i en Data Factory-pipelinen?
* [Data Movement aktiviteter](data-factory-data-movement-activities.md) toomove data.
* [Data Transformation aktiviteter](data-factory-data-transformation-activities.md) tooprocess/Transformera data.

### <a name="when-does-an-activity-run"></a>När körs en aktivitet
Hej **tillgänglighet** konfigurationsinställning i hello utdata datatabell avgör när hello aktiviteten körs. Om indatauppsättningar anges hello aktiviteten kontrollerar om alla hello indata beroenden är uppfyllda (det vill säga **klar** tillstånd) innan den börjar köras.

## <a name="copy-activity---faq"></a>Kopieringsaktiviteten – vanliga frågor och svar
### <a name="is-it-better-toohave-a-pipeline-with-multiple-activities-or-a-separate-pipeline-for-each-activity"></a>Är det bättre toohave en pipeline med flera aktiviteter eller en separat pipeline för varje aktivitet?
Pipelines som är avsedda toobundle relaterade aktiviteter. Om hello datauppsättningar som ansluter dem inte används av andra aktiviteter utanför hello pipeline, kan du behålla hello aktiviteter i en pipeline. På så sätt kan du inte behöver toochain pipeline aktiva perioder så att de överensstämmer med varandra. Dessutom bevaras bättre hello dataintegriteten i hello tabeller interna toohello pipeline vid uppdatering av hello pipelinen. Pipeline-uppdatering i stort sett stoppar alla hello aktiviteter inom hello pipeline, tas de bort och skapar dem igen. Från redigering perspektiv, det kan också vara enklare toosee hello flödet av data i hello relaterade aktiviteter i en JSON-filen för hello pipeline.

### <a name="what-are-hello-supported-data-stores"></a>Vad hello stöds datalager?
Kopieringsaktiviteten i Data Factory kopierar data från en källa data store tooa sink datalagret. Data Factory stöder hello följande datalager. Data från en källa kan skrivas tooany mottagare. Klicka på en data store toolearn hur toocopy data tooand från detta Arkiv.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Data som lagras med * kan vara lokalt eller på Azure IaaS och kräver tooinstall [Data Management Gateway](data-factory-data-management-gateway.md) på en på-lokal-/ Azure IaaS-dator.

### <a name="what-are-hello-supported-file-formats"></a>Vad är hello filformat som stöds?
[!INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]

### <a name="where-is-hello-copy-operation-performed"></a>Där utförs hello kopieringsåtgärden?
Se [globalt tillgänglig dataflyttning](data-factory-data-movement-activities.md#global) information. När ett lokalt datalager ingår, utförs i korthet hello kopieringen av hello Data Management Gateway i din lokala miljö. Och när hello dataförflyttning mellan två molnet butiker hello kopieringsåtgärden utförs på hello region närmaste toohello sink plats i hello samma geografisk plats.

## <a name="hdinsight-activity---faq"></a>HDInsight-aktivitet – vanliga frågor och svar
### <a name="what-regions-are-supported-by-hdinsight"></a>Vilka regioner som stöds av HDInsight?
Se hello geografiska tillgänglighetssektion i följande artikel hello: eller [HDInsight prisinformation][hdinsight-supported-regions].

### <a name="what-region-is-used-by-an-on-demand-hdinsight-cluster"></a>Vilken region som används av ett HDInsight-kluster på begäran?
hello på begäran HDInsight-kluster skapas i hello samma region där det finns hello lagringsutrymme som du angett toobe används med hello-kluster.    

### <a name="how-tooassociate-additional-storage-accounts-tooyour-hdinsight-cluster"></a>Hur tooassociate ytterligare lagringskonton tooyour HDInsight-kluster?
Om du använder egna HDInsight-kluster (BYOC - sätta ditt eget kluster) finns i följande avsnitt hello:

* [Med hjälp av ett HDInsight-kluster med alternativa Lagringskonton och Metastores][hdinsight-alternate-storage]
* [Använda ytterligare Lagringskonton med HDInsight Hive][hdinsight-alternate-storage-2]

Om du använder ett på begäran-kluster som skapas av hello Data Factory-tjänsten kan du ange ytterligare lagringskonton för hello HDInsight länkade tjänsten så att hello Data Factory-tjänsten kan registrera dem å dina vägnar. I hello JSON-definitionen för den länkade tjänsten för hello på begäran, använda **additionalLinkedServiceNames** egenskapen toospecify alternativa lagringskonton som visas i följande JSON-fragment hello:

```JSON
{
    "name": "MyHDInsightOnDemandLinkedService",
    "properties":
    {
        "type": "HDInsightOnDemandLinkedService",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "LinkedService-SampleData",
            "additionalLinkedServiceNames": [ "otherLinkedServiceName1", "otherLinkedServiceName2" ]
        }
    }
}
```
Representerar länkade tjänster vars definitioner innehåller autentiseringsuppgifter som hello HDInsight-kluster måste tooaccess alternativa storage-konton i hello-exemplet ovan, otherLinkedServiceName1 och otherLinkedServiceName2.

## <a name="slices---faq"></a>Segment - vanliga frågor och svar
### <a name="why-are-my-input-slices-not-in-ready-state"></a>Varför är min inkommande segment inte i tillståndet Ready?
Vanliga fel inte inställningen **externa** egenskapen för**SANT** på hello inkommande dataset när hello indata är externa toohello data factory (som inte tillverkas av hello data factory).

I följande exempel hello, behöver du bara tooset **externa** tootrue på **dataset1**.  

**DataFactory1** Pipeline 1: dataset1 -> activity1 -> dataset2 -> activity2 -> dataset3 Pipeline 2: dataset3 -> activity3 -> dataset4

Om du har en annan data factory med en rörledning som tar dataset4 (tillverkas av pipeline 2 i data factory 1), måste du markera dataset4 som en extern datauppsättning eftersom hello dataset produceras av en annan datafabrik (DataFactory1, inte DataFactory2).  

**DataFactory2**    
Pipeline 1: dataset4 -> activity4 -> dataset5

Om externa hello-egenskapen anges korrekt, kontrollerar du om hello indata finns i hello-plats som anges i hello inkommande datauppsättningsdefinitionen.

### <a name="how-toorun-a-slice-at-another-time-than-midnight-when-hello-slice-is-being-produced-daily"></a>Hur toorun ett segment vid ett senare tillfälle än midnatt när hello sektorn tillverkas dagligen?
Använd hello **offset** egenskapen toospecify hello gång som du vill hello sektorn toobe. Se [Dataset tillgänglighet](data-factory-create-datasets.md#dataset-availability) finns mer information om den här egenskapen. Här är ett enkelt exempel:

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
Dagliga segment som börjar vid **06: 00** i stället för hello standard midnatt.     

### <a name="how-can-i-rerun-a-slice"></a>Hur kan jag köra ett segment?
Du kan köra en sektor i ett av följande sätt hello:

* Använd övervaka och hantera appen toorerun en aktivitetsfönstret eller segment. Se [kör den markerade aktiviteten windows](data-factory-monitor-manage-app.md#perform-batch-actions) anvisningar.   
* Klicka på **kör** i hello kommandofältet hello **DATASEKTORN** bladet för hello sektor i hello Azure-portalen.
* Kör **Set AzureRmDataFactorySliceStatus** med Status för ange**väntar på** för hello segment.   

    ```PowerShell
    Set-AzureRmDataFactorySliceStatus -Status Waiting -ResourceGroupName $ResourceGroup -DataFactoryName $df -TableName $table -StartDateTime "02/26/2015 19:00:00" -EndDateTime "02/26/2015 20:00:00"
    ```
Se [Set AzureRmDataFactorySliceStatus] [ set-azure-datafactory-slice-status] för ytterligare information om hello cmdlet.

### <a name="how-long-did-it-take-tooprocess-a-slice"></a>Hur lång tid det tog tooprocess ett segment?
Använd aktiviteten fönstret Explorer i övervaka och hantera appen tooknow hur lång tid det tog tooprocess en datasektorn. Se [aktivitet fönstret Explorer](data-factory-monitor-manage-app.md#activity-window-explorer) mer information.

Du kan också göra hello efter i hello Azure-portalen:  

1. Klicka på **datauppsättningar** panelen på hello **DATA FACTORY** bladet för din data factory.
2. Klicka på hello specifika dataset i hello **datauppsättningar** bladet.
3. Välj hello sektor som du är intresserad från hello **senaste segment** listan på hello **tabell** bladet.
4. Klicka på hello-aktivitet som körs från hello **Aktivitetskörningar** listan på hello **DATASEKTORN** bladet.
5. Klicka på **egenskaper** panelen på hello **kör AKTIVITETSINFORMATION** bladet.
6. Du bör se hello **varaktighet** fält med ett värde. Det här värdet är hello tidsåtgång tooprocess hello sektorn.   

### <a name="how-toostop-a-running-slice"></a>Hur toostop körs segmentet?
Om du behöver toostop hello pipelinen körs, kan du använda [pausa AzureRmDataFactoryPipeline](/powershell/module/azurerm.datafactories/suspend-azurermdatafactorypipeline) cmdlet. För närvarande slutar pausa hello pipeline inte hello sektorn körningar som pågår. När hello pågående körningar har hämtas inga extra sektorn.

Om du verkligen toostop alla hello körningar omedelbart, skulle hello endast sätt toodelete hello pipeline och skapa den igen. Om du väljer toodelete hello pipeline, behöver du inte toodelete tabeller och länkade tjänster som används av hello pipeline.

[create-factory-using-dotnet-sdk]: data-factory-create-data-factories-programmatically.md
[msdn-class-library-reference]: /dotnet/api/microsoft.azure.management.datafactories.models
[msdn-rest-api-reference]: /rest/api/datafactory/

[adf-powershell-reference]: /powershell/resourcemanager/azurerm.datafactories/v2.3.0/azurerm.datafactories
[azure-portal]: http://portal.azure.com
[set-azure-datafactory-slice-status]: /powershell/resourcemanager/azurerm.datafactories/v2.3.0/set-azurermdatafactoryslicestatus

[adf-pricing-details]: http://go.microsoft.com/fwlink/?LinkId=517777
[hdinsight-supported-regions]: http://azure.microsoft.com/pricing/details/hdinsight/
[hdinsight-alternate-storage]: http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx
[hdinsight-alternate-storage-2]: http://blogs.msdn.com/b/cindygross/archive/2014/05/05/use-additional-storage-accounts-with-hdinsight-hive.aspx
