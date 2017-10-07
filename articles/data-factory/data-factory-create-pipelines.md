---
title: aaaCreate/schema Pipelines, kedjan aktiviteter i Data Factory | Microsoft Docs
description: "Lär dig toocreate en data-pipeline i Azure Data Factory toomove och transformera data. Skapa en datadrivna arbetsflödesinformation tooproduce klar toouse."
keywords: "data pipeline, datadrivna arbetsflöde"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 13b137c7-1033-406f-aea7-b66f25b313c0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2017
ms.author: shlo
ms.openlocfilehash: 4a0fc20f98ce6453c16955e97fddb891926c173a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pipelines-and-activities-in-azure-data-factory"></a>Pipelines och aktiviteter i Azure Data Factory
Den här artikeln hjälper dig att förstå pipelines och aktiviteter i Azure Data Factory och använda dem tooconstruct slutpunkt till slutpunkt datadrivna arbetsflöden för dataförflyttning och databehandlingsscenarier.  

> [!NOTE]
> Den här artikeln förutsätter att du har gått igenom [introduktion tooAzure Data Factory](data-factory-introduction.md). Om du inte har praktiska-på-upplevelse med att skapa datafabriker gå igenom [data transformation kursen](data-factory-build-your-first-pipeline.md) och/eller [data movement kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hjälper dig att bättre förstå den här artikeln.  

## <a name="overview"></a>Översikt
En datafabrik kan ha en eller flera pipelines. En pipeline är en logisk gruppering aktiviteter som tillsammans utför en uppgift. hello aktiviteter i en pipeline definiera åtgärder tooperform på dina data. Du kan till exempel använda en aktivitet toocopy data från en lokal SQL Server-tooan Azure Blob Storage. Använd sedan en Hive-aktivitet som kör en Hive-skript på en Azure HDInsight-kluster tooprocess/Transformera data från hello blob storage tooproduce utdata. Slutligen ska du använda en andra kopia aktiviteten toocopy hello utgående data tooan Azure SQL Data Warehouse rapportlösningar bygger på vilka business intelligence (BI). 

En aktivitet kan ha noll eller flera [indatauppsättningar](data-factory-create-datasets.md) och kan producera en eller flera [utdatauppsättningar](data-factory-create-datasets.md). hello följande diagram visar hello förhållandet mellan pipeline, aktiviteten och dataset i Data Factory: 

![Förhållandet mellan pipeline, aktiviteten och dataset](media/data-factory-create-pipelines/relationship-pipeline-activity-dataset.png)

En pipeline kan du toomanage aktiviteter som en uppsättning i stället för var och en individuellt. Du kan till exempel distribuera, schemalägga, pausa och återuppta en pipeline, i stället för att hantera aktiviteter i hello pipeline oberoende av varandra.

Data Factory stöder två typer av aktiviteter: aktiviteter för dataförflyttning och datatransformering. Varje aktivitet kan ha noll eller fler indata [datauppsättningar](data-factory-create-datasets.md) och skapa en eller flera utdata-datauppsättningar.

En inkommande datauppsättning representerar hello indata för en aktivitet i pipelinen hello och en datamängd för utdata representerar hello utdata för hello-aktivitet. Datauppsättningar identifierar data inom olika datalager, till exempel tabeller, filer, mappar och dokument. När du har skapat en datauppsättning kan du använda den med aktiviteter i en pipeline. Till exempel kan en datauppsättning vara en in-/utdatauppsättning för en kopieringsaktivitet eller en HDInsightHive-aktivitet. Mer information om datauppsättning finns i artikeln [Datauppsättningar i Azure Data Factory](data-factory-create-datasets.md).

### <a name="data-movement-activities"></a>Dataförflyttningsaktiviteter
Kopieringsaktiviteten i Data Factory kopierar data från en källa data store tooa sink datalagret. Data Factory stöder hello följande datalager. Data från en källa kan skrivas tooany mottagare. Klicka på en data store toolearn hur toocopy data tooand från detta Arkiv.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Data som lagras med * kan vara lokalt eller på Azure IaaS och kräver tooinstall [Data Management Gateway](data-factory-data-management-gateway.md) på en på-lokal-/ Azure IaaS-dator.

Mer information finns i artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md).

### <a name="data-transformation-activities"></a>Datatransformeringsaktiviteter
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Mer information finns i artikeln [Datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).

### <a name="custom-net-activities"></a>Anpassa .NET-aktiviteter 
Om du behöver toomove data till/från en butik som hello Kopieringsaktiviteten inte stöder, eller Transformera data med egna logik, skapa en **anpassad .NET-aktivitet**. Mer information om att skapa och använda en anpassad aktivitet finns i [Use custom activities in an Azure Data Factory pipeline (Använda anpassade aktiviteter i en Azure Data Factory-pipeline)](data-factory-use-custom-activities.md).

## <a name="schedule-pipelines"></a>Schemat pipelines
En pipeline är aktiv endast mellan dess **starta** tid och **end** tid. Det utförs inte före starttiden för hello eller efter hello sluttid. Om hello pipeline pausas körs den inte oavsett dess start- och tid. För en pipeline-toorun bör det inte pausas. Se [schemaläggning och körning](data-factory-scheduling-and-execution.md) toounderstand hur schemaläggning och körning fungerar i Azure Data Factory.

## <a name="pipeline-json"></a>Pipeline JSON
Nu tar vi en närmare titt på hur en pipeline definieras i JSON-format. hello allmän struktur för en pipeline ser ut som följer:

```json
{
    "name": "PipelineName",
    "properties": 
    {
        "description" : "pipeline description",
        "activities":
        [

        ],
        "start": "<start date-time>",
        "end": "<end date-time>",
        "isPaused": true/false,
        "pipelineMode": "scheduled/onetime",
        "expirationTime": "15.00:00:00",
        "datasets": 
        [
        ]
    }
}
```

| Tagga | Beskrivning | Krävs |
| --- | --- | --- |
| namn |Namnet på hello pipeline. Ange ett namn som representerar hello-åtgärd som hello pipeline utför. <br/><ul><li>Max. antal tecken: 260</li><li>Måste börja med en bokstav, en siffra eller ett understreck (_)</li><li>Följande tecken är inte tillåtna ”:”., ”+” ”,”?, ”/”, ”<” ”, >” ”, *”, ”%”, ”&” ”,:” ”,\\”</li></ul> |Ja |
| description | Ange hello text som beskriver vilka hello pipeline används för. |Ja |
| activities | Hej **aktiviteter** avsnitt kan ha en eller flera aktiviteter som definierats i den. Finns hello nästa avsnitt för mer information om hello aktiviteter JSON-element. | Ja |  
| start | Starttid för hello pipelinen. Måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601). Till exempel: `2016-10-14T16:32:41Z`. <br/><br/>Det är möjligt toospecify lokal tid, till exempel en EST tid. Här är ett exempel: `2016-02-27T06:00:00-05:00`”, vilket är 6 AM beräknat<br/><br/>hello start- och slutdatum egenskaper anger tillsammans aktiva perioden för hello pipelinen. Utdata segment som endast produceras med i den här aktiva period. |Nej<br/><br/>Om du anger ett värde för hello end-egenskapen måste du ange värdet för egenskapen för hello start.<br/><br/>hello kan start- och sluttider vara tom toocreate en pipeline. Du måste ange båda värdena tooset en aktiva perioden för hello pipeline toorun. Om du inte anger start- och sluttider när du skapar en pipeline, kan du ange dem med hjälp av cmdlet hello Set AzureRmDataFactoryPipelineActivePeriod senare. |
| End | Datum sluttiden för hello pipeline. Om du måste vara i ISO-format. Exempel: `2016-10-14T17:32:41Z` <br/><br/>Det är möjligt toospecify lokal tid, till exempel en EST tid. Här är ett exempel: `2016-02-27T06:00:00-05:00`, vilket är 6 AM EST.<br/><br/>toorun hello pipeline ange på obestämd tid 09-09-9999 som hello värde för hello end-egenskapen. <br/><br/> En pipeline är aktiv endast mellan dess starttid och sluttid. Det utförs inte före starttiden för hello eller efter hello sluttid. Om hello pipeline pausas körs den inte oavsett dess start- och tid. För en pipeline-toorun bör det inte pausas. Se [schemaläggning och körning](data-factory-scheduling-and-execution.md) toounderstand hur schemaläggning och körning fungerar i Azure Data Factory. |Nej <br/><br/>Om du anger ett värde för egenskapen för hello start, måste du ange värde för hello end-egenskapen.<br/><br/>Du hittar information för hello **starta** egenskapen. |
| isPaused | Om set tootrue, hello pipelinen inte körs. Det har i hello pausades tillstånd. Standardvärde = false. Du kan använda den här egenskapen tooenable eller inaktivera en pipeline. |Nej |
| pipelineMode | hello-metoden för schemaläggning körs för hello pipeline. Tillåtna värden är: schemalagda (standard), görs.<br/><br/>”Schemalagd' anger att hello pipeline körs vid ett angivet tidsintervall enligt tooits aktiva period (start- och -tid). 'Görs ”anger att hello pipelinen körs bara en gång. Görs pipelines när skapat går inte att ändra/uppdatera för närvarande. Se [Onetime pipeline](#onetime-pipeline) mer information om hur du görs. |Nej |
| ExpirationTime | Tid när den har skapats för vilka hello [enstaka pipeline](#onetime-pipeline) är giltig och vara etablerade. Om den inte har någon aktiv, misslyckades, eller väntande körs hello pipeline är automatiskt når bort när den hello förfallotid. hello Standardvärde:`"expirationTime": "3.00:00:00"`|Nej |
| Datauppsättningar |Lista över datauppsättningar toobe som används av aktiviteter som definierats i hello pipeline. Den här egenskapen kan vara används toodefine datauppsättningar som är specifika toothis pipeline och inte definierats inom hello data factory. Datauppsättningar som definierats i denna pipeline kan endast användas av denna pipeline och kan inte delas. Se [omfång datauppsättningar](data-factory-create-datasets.md#scoped-datasets) mer information. |Nej |

## <a name="activity-json"></a>Aktivitets-JSON
Hej **aktiviteter** avsnitt kan ha en eller flera aktiviteter som definierats i den. Varje aktivitet har hello efter strukturen på översta nivån:

```json
{
    "name": "ActivityName",
    "description": "description", 
    "type": "<ActivityType>",
    "inputs":  "[]",
    "outputs":  "[]",
    "linkedServiceName": "MyLinkedService",
    "typeProperties":
    {

    },
    "policy":
    {
    },
    "scheduler":
    {
    }
}
```

Följande tabell beskriver egenskaper i hello aktivitet JSON-definitionen:

| Tagga | Beskrivning | Krävs |
| --- | --- | --- |
| namn | Namn på hello-aktivitet. Ange ett namn som representerar hello-åtgärd som hello aktiviteten utför. <br/><ul><li>Max. antal tecken: 260</li><li>Måste börja med en bokstav, en siffra eller ett understreck (_)</li><li>Följande tecken är inte tillåtna ”:”., ”+” ”,”?, ”/”, ”<” ”, >” ”, *”, ”%”, ”&” ”,:” ”,\\”</li></ul> |Ja |
| description | Text som beskriver vad hello aktivitet eller används för |Ja |
| typ | Typ av hello-aktivitet. Se hello [Data Movement aktiviteter](#data-movement-activities) och [Data Transformation aktiviteter](#data-transformation-activities) avsnitt för olika typer av aktiviteter. |Ja |
| Indata |Indatatabeller som används av hello-aktivitet<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Ja |
| utdata |Utdata tabeller som används av hello-aktivitet.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": "outputtable1" } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": "outputtable1" }, { "name": "outputtable2" }  ],` |Ja |
| linkedServiceName |Namnet på hello länkade tjänst som används av hello-aktivitet. <br/><br/>En aktivitet kan kräva att du anger hello länkade tjänst som länkar toohello krävs beräknings-miljö. |Ja för HDInsight-aktivitet och Azure Machine Learning-Batchbedömningsaktivitet <br/><br/>Nej för alla andra |
| typeProperties |Egenskaper i hello **typeProperties** avsnittet beror på typen av hello aktivitet. toosee egenskaper för en aktivitet klickar du på länkarna toohello aktivitet i hello föregående avsnitt. | Nej |
| policy |Principer som påverkar hello körning funktionssätt hello-aktivitet. Om det inte anges används standardprinciper. |Nej |
| Schemaläggaren | Egenskapen ”scheduler” är används toodefine önskad schemaläggning för hello aktiviteten. Dess subegenskaper är hello samma som hello i hello [tillgänglighet egenskap i en dataset](data-factory-create-datasets.md#dataset-availability). |Nej |


### <a name="policies"></a>Principer
Principer påverkar hello körning beteendet för en aktivitet, särskilt när hello segment i en tabell som har bearbetats. hello följande tabell innehåller hello information.

| Egenskap | Tillåtna värden | Standardvärde | Beskrivning |
| --- | --- | --- | --- |
| Concurrency |Integer <br/><br/>Värdet för maximalt antal: 10 |1 |Antal samtidiga körningar av hello-aktivitet.<br/><br/>Den avgör hello antalet ParallellAktivitet körningar som kan inträffa på olika segment. Till exempel om en aktivitet måste toogo via snabbare en stor mängd tillgänglig data, med ett större värde för samtidighet hello databearbetning. |
| executionPriorityOrder |NewestFirst<br/><br/>OldestFirst |OldestFirst |Anger hello sorteringen av datasektorer som bearbetas.<br/><br/>Till exempel om du har 2 sektorer (en inträffar klockan 4, och en annan kl) och båda finns väntande körning. Om du ställer in hello executionPriorityOrder toobe NewestFirst bearbetas hello sektorn kl först. På liknande sätt om du ställer in hello executionPriorityORder toobe OldestFIrst bearbetas sedan hello sektorn klockan 4. |
| retry |Integer<br/><br/>Värdet för maximalt antal kan vara 10 |0 |Antalet försök innan hello databearbetning för hello segment har markerats som ett fel. Aktivitetskörningen för en datasektorn försöks in toohello angetts antal nya försök. hello försök görs så snart som möjligt efter hello-fel. |
| timeout |TimeSpan |00:00:00 |Tidsgräns för hello-aktivitet. Exempel: 00:10:00 (inbegriper timeout 10 minuter)<br/><br/>Om ett värde har angetts eller är 0, är hello timeout oändligt.<br/><br/>Om hello bearbetningstid på ett segment överskrider hello timeout-värde, det avbryts och hello system försöker tooretry hello bearbetning. hello antal återförsök beror på hello försök egenskapen. När timeout uppstår status hello tooTimedOut. |
| Fördröjning |TimeSpan |00:00:00 |Ange hello fördröjning innan data bearbetning av hello segment startas.<br/><br/>hello körningen av aktiviteten för en datasektorn startas efter att hello fördröjning har förfallit hello förväntades körningstid.<br/><br/>Exempel: 00:10:00 (inbegriper fördröjning på 10 minuter) |
| longRetry |Integer<br/><br/>Värdet för maximalt antal: 10 |1 |hello antal lång försök försök innan hello sektorn körningen misslyckades.<br/><br/>longRetry försök fördelade av longRetryInterval. Så om du behöver toospecify taget mellan nya försök använda longRetry. Om både försök och longRetry anges varje longRetry försök omförsök Hej max antal försök används och försök igen * longRetry.<br/><br/>Om till exempel har vi hello följande inställningar i hello aktivitetsprincip:<br/>Försök: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Anta att det finns bara ett segment tooexecute (status väntar) och hello aktivitetskörningen misslyckas varje gång. Det skulle inledningsvis 3 körning på varandra följande försök. Efter varje försök skulle hello sektorn status vara försök igen. Efter första 3 försök är över, är hello sektorn status LongRetry.<br/><br/>Efter en timme (det vill säga Longretryinteval's värde), skulle det finnas en annan uppsättning 3 körning på varandra följande försök. Efter detta hello sektorn status skulle misslyckades och inga fler försök kan inte genomföras. Därför har övergripande 6 försök gjorts.<br/><br/>Om alla körning lyckas hello sektorn status är klar och inga fler försök görs.<br/><br/>longRetry får användas i situationer där beroende data kommer till icke-deterministiska gånger eller hello hela miljön är flaky under vilken databearbetningen sker. I sådana fall kan inte göra återförsök efter varandra kan hjälpa och gör det med tiden resulterar i hello tidsintervall önskad utdata.<br/><br/>Liten varning: Ange inte höga värden för longRetry eller longRetryInterval. Normalt en högre värden andra systemfel problem. |
| longRetryInterval |TimeSpan |00:00:00 |hello fördröjning mellan försök har lång |

## <a name="sample-copy-pipeline"></a>Exempel på kopieringspipeline
I följande exempel pipeline hello, är en aktivitet av typen **kopiera** i hello **aktiviteter** avsnitt. I det här exemplet hello [kopieringsaktiviteten](data-factory-data-movement-activities.md) kopierar data från Azure Blob storage tooan Azure SQL-databas. 

```json
{
  "name": "CopyPipeline",
  "properties": {
    "description": "Copy data from a blob tooAzure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputDataset"
          }
        ],
        "outputs": [
          {
            "name": "OutputDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
} 
```

Observera följande punkter hello:

* Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**kopiera**.
* Indata för hello aktiviteten är inställd för**InputDataset** och utdata för hello aktiviteten är inställd för**OutputDataset**. I artikeln [Datauppsättningar](data-factory-create-datasets.md) finns information om hur du definierar datauppsättningar i JSON. 
* I hello **typeProperties** avsnittet **BlobSource** har angetts som hello källtypen och **SqlSink** har angetts som hello Mottagartypen. I hello [Data movement aktiviteter](#data-movement-activities) klickar du på hello datalager som du vill toouse som en källa eller en mottagare toolearn mer om att flytta data till och från den datalagringen. 

En fullständig genomgång av hur du skapar den här pipelinen finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Exempel på transfomeringspipeline
I följande exempel pipeline hello, är en aktivitet av typen **HDInsightHive** i hello **aktiviteter** avsnitt. I det här exemplet hello [HDInsight Hive aktiviteten](data-factory-hive-activity.md) omvandlar data från Azure Blob-lagring genom att köra en Hive-skriptfil på ett Azure HDInsight Hadoop-kluster. 

```json
{
    "name": "TransformPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-04-01T00:00:00Z",
        "end": "2016-04-02T00:00:00Z",
        "isPaused": false
    }
}
```

Observera följande punkter hello: 

* Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**HDInsightHive**.
* hello Hive-skriptfil, **partitionweblogs.hql**, lagras i hello Azure storage-konto (anges av hello scriptLinkedService, kallas **AzureStorageLinkedService**), och i  **skriptet** mapp i hello behållaren **adfgetstarted**.
* Hej `defines` avsnitt är används toospecify hello runtime inställningarna som överförs toohello hive-skript som Hive konfigurationsvärden (t.ex `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).

Hej **typeProperties** avsnittet är olika för varje aktivitet för omvandling. toolearn om typen egenskaper som stöds för en omvandling av aktivitet, klickar du på hello omvandling aktivitet i hello [Data transformation aktiviteter](#data-transformation-activities) tabell. 

En fullständig genomgång av hur du skapar den här pipelinen finns [Självstudier: skapa din första pipeline tooprocess data med Hadoop-kluster](data-factory-build-your-first-pipeline.md). 

## <a name="multiple-activities-in-a-pipeline"></a>Flera aktiviteter i en pipeline
hello föregående två exempel pipelines har endast en aktivitet. Du kan fler än en aktivitet i en pipeline.  

Om du har flera aktiviteter i en pipeline och utdata för en aktivitet inte är indata för en annan aktivitet, kan hello aktiviteter köras parallellt om indata segment för hello aktiviteter är klar. 

Du kan länka två aktiviteter genom att låta hello datamängd för utdata för en aktivitet som hello inkommande dataset av hello andra aktiviteter. hello andra aktiviteten körs bara när hello först en har slutförts.

![Länkning aktiviteter i hello pipeline samma](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

I det här exemplet hello pipeline har två aktiviteter: Activity1 och Activity2. Hej Activity1 tar Dataset1 som indata och skapar ett utgående Dataset2. hello aktiviteten tar Dataset2 som indata och skapar ett utgående Dataset3. Eftersom hello utdata från Activity1 (Dataset2) är hello inmatning av Activity2, hello Activity2 körs bara efter hello aktiviteten har slutförts och ger hello Dataset2 sektorn. Om hello Activity1 misslyckas av någon anledning och skapar inte hello Dataset2 segment, hello aktivitet 2 körs inte för den sektorn (till exempel: 9 AM too10 är). 

Du kan också kedja aktiviteter som finns i olika pipelines.

![Länkning aktiviteter i två rörledningar](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

I det här exemplet har Pipeline1 bara en aktivitet som tar Dataset1 som indata och producerar Dataset2 som utdata. Hej Pipeline2 har också bara en aktivitet som tar Dataset2 som indata och Dataset3 som utdata. 

Mer information finns i [schemaläggning och körning av](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

## <a name="create-and-monitor-pipelines"></a>Skapa och övervaka pipelines
Du kan skapa pipelines med någon av dessa verktyg och SDK: er. 

- Guiden Kopiera. 
- Azure Portal
- Visual Studio
- Azure PowerShell
- Azure Resource Manager-mall
- REST API
- .NET-API

Se hello följande självstudier för stegvisa instruktioner för att skapa pipelines med någon av dessa verktyg och SDK: er.
 
- [Skapa en pipeline med en datatransformeringsaktivitet](data-factory-build-your-first-pipeline.md)
- [Skapa en pipeline med en aktivitet för flytt av data](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

När en pipeline skapas/distribueras kan du hantera och övervaka din pipelines med hjälp av hello Azure portal blad eller övervaka och hantera appen. Se hello följande avsnitt för stegvisa instruktioner. 

- [Övervaka och hantera pipelines med hjälp av Azure portal blad](data-factory-monitor-manage-pipelines.md).
- [Övervaka och hantera pipelines med övervaka och hantera appen](data-factory-monitor-manage-app.md)


## <a name="onetime-pipeline"></a>Görs pipeline
Du kan skapa och schemalägga en pipeline-toorun regelbundet (till exempel: varje timme eller varje dag) inom hello start- och sluttider du anger i hello pipeline-definition. Se [schemalägga aktiviteter](#scheduling-and-execution) mer information. Du kan också skapa en pipeline som körs bara en gång. toodo så är fallet bör du ställa in hello **pipelineMode** egenskap i hello pipeline definition för**görs** enligt hello följande JSON-exemplet. hello standardvärdet för den här egenskapen är **schemalagda**.

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ]
                "name": "CopyActivity-0"
            }
        ]
        "pipelineMode": "OneTime"
    }
}
```

Observera följande hello:

* **Starta** och **end** tider för hello pipelinen inte har angetts.
* **Tillgänglighet** datauppsättningar som anges av inkommande och utgående (**frekvens** och **intervall**), även om Data Factory inte använder hello värden.  
* Diagramvyn visas inte enstaka pipelines. Det här beteendet är avsiktligt.
* Enstaka pipelines kan inte uppdateras. Du klona en enstaka pipeline, byta namn på den, uppdatera egenskaper och distribuera den toocreate en annan.


## <a name="next-steps"></a>Nästa steg
- Läs mer om datauppsättningar [skapa datauppsättningar](data-factory-create-datasets.md) artikel. 
- Mer information om hur pipelines schemaläggs och körs finns [schemaläggning och körning i Azure Data Factory](data-factory-scheduling-and-execution.md) artikel. 
  

