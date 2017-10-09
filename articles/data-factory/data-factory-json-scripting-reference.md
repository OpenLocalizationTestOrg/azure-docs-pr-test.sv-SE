---
title: "aaaAzure Data Factory - referens för JSON-skript | Microsoft Docs"
description: "Ger JSON-scheman för Data Factory entiteter."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 813fd752bb0ecb1b513d022b9f302325105dac31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---json-scripting-reference"></a>Azure Data Factory - JSON-skript referens
Den här artikeln innehåller exempel och JSON-scheman för att definiera Azure Data Factory-enheter (pipeline, aktivitet, datamängd och länkade tjänsten).  

## <a name="pipeline"></a>Pipeline 
hello övergripande struktur för en pipeline-definition är följande: 

```json
{
  "name": "SamplePipeline",
  "properties": {
    "description": "Describe what pipeline does",
    "activities": [
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

Följande tabell beskriver hello egenskaper i hello pipeline JSON-definitionen:

| Egenskap | Beskrivning | Krävs
-------- | ----------- | --------
| namn | Namnet på hello pipeline. Ange ett namn som representerar hello-åtgärd som hello aktivitet eller pipeline är konfigurerade toodo<br/><ul><li>Max. antal tecken: 260</li><li>Måste börja med en bokstav, en siffra eller ett understreck (_)</li><li>Följande tecken är inte tillåtna ”:”., ”+” ”,”?, ”/”, ”<” ”, >” ”, *”, ”%”, ”&” ”,:” ”,\\”</li></ul> |Ja |
| description |Text som beskriver vilka hello aktivitet eller den pipeline som används för | Nej |
| activities | Innehåller en lista med aktiviteter. | Ja |
| start |Starttid för hello pipelinen. Måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601). Till exempel: 2014-10-14T16:32:41. <br/><br/>Det är möjligt toospecify lokal tid, till exempel en EST tid. Här är ett exempel: `2016-02-27T06:00:00**-05:00`, vilket är 6 AM EST.<br/><br/>hello start- och slutdatum egenskaper anger tillsammans aktiva perioden för hello pipelinen. Utdata segment som endast produceras med i den här aktiva period. |Nej<br/><br/>Om du anger ett värde för hello end-egenskapen måste du ange värdet för egenskapen för hello start.<br/><br/>hello kan start- och sluttider vara tom toocreate en pipeline. Du måste ange båda värdena tooset en aktiva perioden för hello pipeline toorun. Om du inte anger start- och sluttider när du skapar en pipeline, kan du ange dem med hjälp av cmdlet hello Set AzureRmDataFactoryPipelineActivePeriod senare. |
| End |Datum sluttiden för hello pipeline. Om du måste vara i ISO-format. Till exempel: 2014-10-14T17:32:41 <br/><br/>Det är möjligt toospecify lokal tid, till exempel en EST tid. Här är ett exempel: `2016-02-27T06:00:00**-05:00`, vilket är 6 AM EST.<br/><br/>toorun hello pipeline ange på obestämd tid 09-09-9999 som hello värde för hello end-egenskapen. |Nej <br/><br/>Om du anger ett värde för egenskapen för hello start, måste du ange värde för hello end-egenskapen.<br/><br/>Du hittar information för hello **starta** egenskapen. |
| isPaused |Om set tootrue hello pipelinen inte körs. Standardvärde = false. Du kan använda den här egenskapen tooenable eller inaktivera. |Nej |
| pipelineMode |hello-metoden för schemaläggning körs för hello pipeline. Tillåtna värden är: schemalagda (standard), görs.<br/><br/>”Schemalagd' anger att hello pipeline körs vid ett angivet tidsintervall enligt tooits aktiva period (start- och -tid). 'Görs ”anger att hello pipelinen körs bara en gång. Görs pipelines när skapat går inte att ändra/uppdatera för närvarande. Se [Onetime pipeline](data-factory-create-pipelines.md#onetime-pipeline) mer information om hur du görs. |Nej |
| ExpirationTime |Tidsperiod när den har skapats för vilka hello pipeline är giltig och vara etablerade. Om den inte har någon aktiv, misslyckades, eller väntande körs hello pipeline tas bort automatiskt när når den hello förfallotid. |Nej |


## <a name="activity"></a>Aktivitet 
hello övergripande struktur för en aktivitet i en pipeline-definition (aktiviteter element) är följande:

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
    }
    "scheduler":
    {
    }
}
```

Följande tabell Beskriv hello egenskaper i hello aktivitet JSON-definitionen:

| Tagga | Beskrivning | Krävs |
| --- | --- | --- |
| namn |Namn på hello-aktivitet. Ange ett namn som representerar hello-åtgärd som hello aktivitet är konfigurerade toodo<br/><ul><li>Max. antal tecken: 260</li><li>Måste börja med en bokstav, en siffra eller ett understreck (_)</li><li>Följande tecken är inte tillåtna ”:”., ”+” ”,”?, ”/”, ”<” ”, >” ”, *”, ”%”, ”&” ”,:” ”,\\”</li></ul> |Ja |
| description |Text som beskriver vilka hello-aktivitet används för. |Ja |
| typ |Anger hello hello-aktivitet. Se hello [DATALAGER](#data-stores) och [DATA TRANSFORMATION aktiviteter](#data-transformation-activities) avsnitt för olika typer av aktiviteter. |Ja |
| Indata |Indatatabeller som används av hello-aktivitet<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Ja |
| utdata |Utdata tabeller som används av hello-aktivitet.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": “outputtable1” } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": “outputtable1” }, { "name": “outputtable2” }  ],` |Ja |
| linkedServiceName |Namnet på hello länkade tjänst som används av hello-aktivitet. <br/><br/>En aktivitet kan kräva att du anger hello länkade tjänst som länkar toohello krävs beräknings-miljö. |Ja för HDInsight aktiviteter, Azure Machine Learning aktiviteter och lagrade Proceduraktiviteten. <br/><br/>Nej för alla andra |
| typeProperties |Egenskaper i hello typeProperties avsnittet beror på typ av hello-aktivitet. |Nej |
| policy |Principer som påverkar hello körning funktionssätt hello-aktivitet. Om det inte anges används standardprinciper. |Nej |
| Schemaläggaren |Egenskapen ”scheduler” är används toodefine önskad schemaläggning för hello aktiviteten. Dess subegenskaper är hello samma som hello i hello [tillgänglighet egenskap i en dataset](data-factory-create-datasets.md#dataset-availability). |Nej |

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

### <a name="typeproperties-section"></a>typeProperties avsnitt
Hej typeProperties avsnitt är olika för varje aktivitet. Omvandling aktiviteter har precis hello Typegenskaper. Se [DATA TRANSFORMATION aktiviteter](#data-transformation-activities) i den här artikeln för JSON-exempel som definierar omvandling av aktiviteter i en pipeline. 

**Kopieringsaktiviteten** har två avsnitt i hello typeProperties avsnittet: **källa** och **sink**. Se [DATALAGER](#data-stores) avsnitt i den här artikeln för JSON-exempel som visar hur toouse data lagras som en källa och/eller mottagare. 

### <a name="sample-copy-pipeline"></a>Exempel på kopieringspipeline
I följande exempel pipeline hello, är en aktivitet av typen **kopiera** i hello **aktiviteter** avsnitt. I det här exemplet hello [Kopieringsaktiviteten](data-factory-data-movement-activities.md) kopierar data från Azure Blob storage tooan Azure SQL-databas. 

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
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

Observera följande punkter hello:

* Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**kopiera**.
* Indata för hello aktiviteten är inställd för**InputDataset** och utdata för hello aktiviteten är inställd för**OutputDataset**.
* I hello **typeProperties** avsnittet **BlobSource** har angetts som hello källtypen och **SqlSink** har angetts som hello Mottagartypen.

Se [DATALAGER](#data-stores) avsnitt i den här artikeln för JSON-exempel som visar hur toouse data lagras som en källa och/eller mottagare.    

En fullständig genomgång av hur du skapar den här pipelinen finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

### <a name="sample-transformation-pipeline"></a>Exempel på transfomeringspipeline
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
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00",
        "isPaused": false
    }
}
```

Observera följande punkter hello: 

* Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**HDInsightHive**.
* hello Hive-skriptfil, **partitionweblogs.hql**, lagras i hello Azure storage-konto (anges av hello scriptLinkedService, kallas **AzureStorageLinkedService**), och i  **skriptet** mapp i hello behållaren **adfgetstarted**.
* Hej **definierar** avsnitt är används toospecify hello runtime inställningarna som överförs toohello hive-skript som Hive konfigurationsvärden (t.ex `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).

Se [DATA TRANSFORMATION aktiviteter](#data-transformation-activities) i den här artikeln för JSON-exempel som definierar omvandling av aktiviteter i en pipeline.

En fullständig genomgång av hur du skapar den här pipelinen finns [Självstudier: skapa din första pipeline tooprocess data med Hadoop-kluster](data-factory-build-your-first-pipeline.md). 

## <a name="linked-service"></a>Länkad tjänst
hello övergripande struktur för en länkad tjänst-definition är följande:

```json
{
    "name": "<name of hello linked service>",
    "properties": {
        "type": "<type of hello linked service>",
        "typeProperties": {
        }
    }
}
```

Följande tabell Beskriv hello egenskaper i hello aktivitet JSON-definitionen:

| Egenskap | Beskrivning | Krävs |
| -------- | ----------- | -------- | 
| namn | Namnet på hello länkade tjänsten. | Ja | 
| Egenskaper - typ | Typ av hello länkad tjänst. Till exempel: Azure Storage, Azure SQL Database. |
| typeProperties | Hej typeProperties avsnittet innehåller element som är olika för varje dataarkiv eller compute-miljö. Se [datalager](#datastores) avsnittet för alla hello datalager länkade tjänster och [compute miljöer](#compute-environments) för alla hello compute länkade tjänster |   

## <a name="dataset"></a>Datauppsättning 
En datamängd i Azure Data Factory definieras enligt följande:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag tooindicate external data. only for input datasets>,
        "linkedServiceName": "<Name of hello linked service that refers tooa data store.>",
        "structure": [
            {
                "name": "<Name of hello column>",
                "type": "<Name of hello type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies hello time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies hello interval within hello defined frequency. For example, frequency set too'Hour' and interval set too1 indicates that new data slices should be produced hourly>"
        },
       "policy":
        {      
        }
    }
}
```

hello följande tabell beskriver egenskaper i hello ovan JSON:   

| Egenskap | Beskrivning | Krävs | Standard |
| --- | --- | --- | --- |
| namn | Namnet på hello dataset. Se [Azure Data Factory - namngivningsregler](data-factory-naming-rules.md) för namngivningsregler. |Ja |Ej tillämpligt |
| typ | typ av hello dataset. Ange en hello-typer som stöds av Azure Data Factory (till exempel: AzureBlob, AzureSqlTable). Se [DATALAGER](#data-stores) avsnittet för alla hello datalager och dataset-typer som stöds av Data Factory. | 
| struktur | Schemat för hello dataset. Den innehåller kolumner, deras typer och så vidare. | Nej |Ej tillämpligt |
| typeProperties | Egenskaper för motsvarande toohello valda typen. Se [DATALAGER](#data-stores) för typer som stöds och deras egenskaper. |Ja |Ej tillämpligt |
| extern | Boolesk flagga toospecify om en datamängd uttryckligen produceras av en data factory-pipelinen eller inte. |Nej |FALSKT |
| availability | Definierar hello bearbetning fönster eller hello segmentering modellen för hello dataset produktion. Mer information om hello dataset segmentering modellen finns [schemaläggning och körning](data-factory-scheduling-and-execution.md) artikel. |Ja |Ej tillämpligt |
| policy |Definierar hello kriterier eller hello villkor som hello dataset segment måste vara uppfyllda. <br/><br/>Mer information finns i [Dataset princip](#Policy) avsnitt. |Nej |Ej tillämpligt |

Varje kolumn i hello **struktur** avsnitt innehåller hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| namn |Namnet på hello-kolumn. |Ja |
| typ |Datatypen för kolumnen hello.  |Nej |
| Kultur |.NET baserat kultur toobe används när typ har angetts och .NET-typ `Datetime` eller `Datetimeoffset`. Standardvärdet är `en-us`. |Nej |
| Format |Formatera strängen toobe används när typ har angetts och .NET-typ `Datetime` eller `Datetimeoffset`. |Nej |

I följande exempel hello, hello datamängden har tre kolumner `slicetimestamp`, `projectname`, och `pageviews` och de är av typen: String, String och Decimal respektive.

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

hello följande tabell beskriver egenskaper som kan användas i hello **tillgänglighet** avsnitt:

| Egenskap | Beskrivning | Krävs | Standard |
| --- | --- | --- | --- |
| frequency |Anger hello tidsenhet för dataset sektorn produktion.<br/><br/><b>Stöd för frekvens</b>: minut, timma, dag, vecka, månad |Ja |Ej tillämpligt |
| interval |Anger en multiplikator för frekvens<br/><br/>”X frekvensintervall” avgör hur ofta hello segment skapas.<br/><br/>Om du behöver hello dataset toobe segmenterat timme, ange <b>frekvens</b> för<b>timme</b>, och <b>intervall</b> för<b>1</b>.<br/><br/><b>Obs</b>: Om du anger frekvens som minut, rekommenderar vi att du ställer in hello intervall toono som är mindre än 15 |Ja |Ej tillämpligt |
| format |Anger om hello segment ska produceras hello start/slutet av hello intervall.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Om frekvensen anges tooMonth och format anges tooEndOfInterval, tillverkas hello segment på hello sista dagen i månaden. Om hello format anges tooStartOfInterval, produceras hello segment på hello första dagen i månaden.<br/><br/>Om frekvensen anges tooDay och format anges tooEndOfInterval, tillverkas hello segment i hello senaste timmen på dagen hello.<br/><br/>Om frekvensen anges tooHour och format anges tooEndOfInterval, tillverkas hello sektorn hello slutet av hello timme. För ett segment för PM 1 – 2 PM period, till exempel produceras hello sektorn klockan 2. |Nej |EndOfInterval |
| anchorDateTime |Definierar hello absolut placering i tid som används av Schemaläggaren toocompute dataset sektorn gränser. <br/><br/><b>Obs</b>: om hello AnchorDateTime har datumdelar som är mer detaljerad än hello frekvens sedan hello mer detaljerade delar ignoreras. <br/><br/>Till exempel, om hello <b>intervall</b> är <b>varje timme</b> (frekvens: timme och intervall: 1) och hello <b>AnchorDateTime</b> innehåller <b>minuter och sekunder</b>sedan hello <b>minuter och sekunder</b> delar av hello AnchorDateTime ignoreras. |Nej |01/01/0001 |
| förskjutning |TimeSpan genom vilka hello början och slutet av alla dataset segment flyttat. <br/><br/><b>Obs</b>: om både anchorDateTime och offset anges hello resultatet är hello kombineras SKIFT. |Nej |Ej tillämpligt |

hello efter tillgänglighet avsnittet anger att hello utdatauppsättningen är producerade varje timme (eller) indata dataset finns varje timme:

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

Hej **princip** avsnitt i datauppsättningsdefinitionen definierar hello kriterier eller hello villkor som hello dataset segment måste vara uppfyllda.

| Principnamn | Beskrivning | Används för| Krävs | Standard |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Verifierar att hello-data i en **Azure blob** uppfyller hello krav på minsta storlek (i megabyte). |Azure-blobb |Nej |Ej tillämpligt |
| minimumRows |Verifierar att hello-data i en **Azure SQL database** eller en **Azure-tabellen** innehåller hello minsta antal rader. |<ul><li>Azure SQL Database</li><li>Azure-tabellen</li></ul> |Nej |Ej tillämpligt |

**Exempel:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

Om ett DataSet-objekt produceras av Azure Data Factory, bör det markeras som **externa**. Den här inställningen gäller vanligtvis toohello indata för den första aktiviteten i en pipeline om aktiviteten eller pipeline länkning används.

| Namn | Beskrivning | Krävs | Standardvärde |
| --- | --- | --- | --- |
| dataDelay |Tid toodelay hello kontroll av hello tillgängligheten för hello externa data för hello angivna sektorn. Om hello data finns varje timme, hello Kontrollera toosee hello externa data är tillgängliga och hello motsvarande segment kan klar försenas med hjälp av dataDelay.<br/><br/>Gäller endast toohello aktuell tid.  Om det är 1:00 PM just nu och det här värdet är 10 minuter, till exempel startas hello validering klockan 13:10.<br/><br/>Den här inställningen påverkar inte segment i hello tidigare (segment med segment sluttid + dataDelay < nu) bearbetas utan fördröjning.<br/><br/>Tid som är större än 23:59 timmar måste toospecified med hello `day.hours:minutes:seconds` format. Till exempel toospecify 24 timmar ska inte använda 24:00:00. Använd i stället 1.00:00:00. Om du använder 24:00:00, behandlas den som 24 dagar (24.00:00:00). Ange 1:04:00:00 för 1 dag och 4 timmar. |Nej |0 |
| RetryInterval |hello väntetiden mellan en felet och hello nästa försök. Om ett försök misslyckas är hello nästa försök efter retryInterval. <br/><br/>Om den är 1:00 PM just nu kan börja vi hello första försöket. Om hello varaktighet toocomplete hello första verifieringen är 1 minut och hello-åtgärden misslyckades, hello nästa försök görs 1:00 + 1 min. (varaktighet) + 1 min. (Återförsöksintervall) = 1:02 PM. <br/><br/>För segment i hello senaste finns ingen fördröjning. hello försök sker omedelbart. |Nej |00:01:00 (1 minut) |
| retryTimeout |hello tidsgräns för varje nytt försök.<br/><br/>Om den här egenskapen anges too10 minuter hello verifiering måste toobe slutföras inom 10 minuter. Om det tar längre tid än 10 minuter tooperform hello validering försök hello gånger ut.<br/><br/>Om alla försök för hello verifiering timeout har hello segment markerats som för lång tid. |Nej |00:10:00 (10 minuter) |
| maximumRetry |Antal gånger toocheck hello tillgänglighet för hello externa data. hello tillåtna maxvärdet är 10. |Nej |3 |


## <a name="data-stores"></a>DATALAGER
Hej [länkade tjänsten](#linked-service) avsnitt som finns beskrivningar för JSON-element som är vanliga tooall typer av länkade tjänster. Det här avsnittet innehåller information om JSON-element som är specifika tooeach datalagret.

Hej [Dataset](#dataset) avsnitt som finns beskrivningar för JSON-element som är vanliga tooall typer av datauppsättningar. Det här avsnittet innehåller information om JSON-element som är specifika tooeach datalagret.

Hej [aktiviteten](#activity) avsnitt som finns beskrivningar för JSON-element som är vanliga tooall typer av aktiviteter. Det här avsnittet innehåller information om JSON-element som är specifika tooeach datalagret när den används som en källor/mottagare i en Kopieringsaktivitet.  

Klicka på länken hello för hello store du är intresserad av toosee hello JSON-scheman för den länkade tjänsten, dataset, och hello källor/mottagare för hello kopieringsaktiviteten.

| Kategori | Datalager 
|:--- |:--- |
| **Azure** |[Azure Blob Storage](#azure-blob-storage) |
| &nbsp; |[Azure Data Lake Store](#azure-datalake-store) |
| &nbsp; |[Azure Cosmos DB](#azure-cosmos-db) |
| &nbsp; |[Azure SQL Database](#azure-sql-database) |
| &nbsp; |[Azure SQL Data Warehouse](#azure-sql-data-warehouse) |
| &nbsp; |[Azure Search](#azure-search) |
| &nbsp; |[Azure Table Storage](#azure-table-storage) |
| **Databaser** |[Amazon Redshift](#amazon-redshift) |
| &nbsp; |[IBM DB2](#ibm-db2) |
| &nbsp; |[MySQL](#mysql) |
| &nbsp; |[Oracle](#oracle) |
| &nbsp; |[PostgreSQL](#postgresql) |
| &nbsp; |[SAP Business Warehouse](#sap-business-warehouse) |
| &nbsp; |[SAP HANA](#sap-hana) |
| &nbsp; |[SQL Server](#sql-server) |
| &nbsp; |[Sybase](#sybase) |
| &nbsp; |[Teradata](#teradata) |
| **NoSQL** |[Cassandra](#cassandra) |
| &nbsp; |[MongoDB](#mongodb) |
| **Fil** |[Amazon S3](#amazon-s3) |
| &nbsp; |[Filsystem](#file-system) |
| &nbsp; |[FTP](#ftp) |
| &nbsp; |[HDFS](#hdfs) |
| &nbsp; |[SFTP](#sftp) |
| **Andra** |[HTTP](#http) |
| &nbsp; |[OData](#odata) |
| &nbsp; |[ODBC](#odbc) |
| &nbsp; |[Salesforce](#salesforce) |
| &nbsp; |[Webbtabell](#web-table) |

## <a name="azure-blob-storage"></a>Azure Blob Storage

### <a name="linked-service"></a>Länkad tjänst
Det finns två typer av länkade tjänster: Azure länkade lagringstjänsten och Azure Storage SAS länkade tjänsten.

#### <a name="azure-storage-linked-service"></a>Länkad Azure Storage-tjänst
toolink din Azure storage-konto tooa data factory med hjälp av hello **kontonyckel**, skapa en länkad Azure Storage-tjänst. toodefine ett Azure Storage länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureStorage**. Du kan sedan ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| connectionString |Ange nödvändig information tooconnect tooAzure lagring för hello-egenskapen connectionString. |Ja |

##### <a name="example"></a>Exempel  

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

#### <a name="azure-storage-sas-linked-service"></a>Azure Storage SAS länkade tjänsten
hello Azure Storage SAS länkad tjänst kan du toolink ett Azure Storage-konto tooan Azure data factory med hjälp av en delad signatur åtkomst (SAS). Det ger hello data factory med begränsad/Tidsbundna åtkomst tooall utvalda resurser (blobbehållare) i hello lagring. toolink din Azure storage-konto tooa data factory med signatur för delad åtkomst, skapa en Azure Storage SAS länkad tjänst. toodefine ett Azure Storage SAS länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureStorageSas**. Du kan sedan ange följande egenskaper i hello **typeProperties** avsnitt:   

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| sasUri |Ange URI för delad åtkomst-signatur toohello Azure Storage-resurser, till exempel blob, behållare eller tabellen. |Ja |

##### <a name="example"></a>Exempel

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

Mer information om dessa länkade tjänster finns [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine en Azure-blobbdatauppsättning set hello **typen** på hello datamängd för**AzureBlob**. Ange sedan följande specifika Azure Blob-egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| folderPath |Sökvägen toohello behållaren och mappen i hello blob storage. Exempel: myblobcontainer\myblobfolder\ |Ja |
| fileName |Namnet på hello-blob. Filnamnet är valfria och skiftlägeskänsligt.<br/><br/>Om du anger ett filnamn, hello aktivitet (inklusive kopia) fungerar på hello specifika Blob.<br/><br/>Om filnamnet har angetts innehåller kopiera alla BLOB i hello folderPath för inkommande dataset.<br/><br/>Om filnamnet inte anges för en utdatauppsättningen hello namnet på hello genereras skulle vara i hello efter det här formatet: Data. <Guid>.txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nej |
| partitionedBy |partitionedBy är en valfri egenskap. Du kan använda det toospecify dynamiska folderPath och filnamnet för tid series-data. Exempelvis kan folderPath parameteriseras för varje timme av data. |Nej |
| Format | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Ange hello **typen** egenskap under format tooone av dessa värden. Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. <br><br> Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner. |Nej |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**. Nivåer som stöds är: **Optimal** och **snabbast**. Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
 ```


Mer information finns i [Azure Blob-koppling](data-factory-azure-blob-connector.md#dataset-properties) artikel.

### <a name="blobsource-in-copy-activity"></a>BlobSource i en Kopieringsaktivitet
Om du vill kopiera data från ett Azure Blob Storage, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**BlobSource**, och ange följande egenskaper i hello ** källa ** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| Rekursiva |Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen. |SANT (standardvärdet), FALSKT |Nej |

#### <a name="example-blobsource"></a>Exempel: BlobSource **
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
### <a name="blobsink-in-copy-activity"></a>BlobSink i en Kopieringsaktivitet
Om du kopierar data tooan Azure Blob Storage, ange hello **sink typen** av hello kopieringsaktiviteten för**BlobSink**, och ange följande egenskaper i hello **sink** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| copyBehavior |Definierar hello kopiera beteende när hello källa är BlobSource eller filsystem. |<b>PreserveHierarchy</b>: bevarar hello filstruktur i hello målmapp. hello relativa sökvägen till källmappen för filen toosource är identiska toohello relativa sökvägen till filen tootarget mapp.<br/><br/><b>FlattenHierarchy</b>: alla filer från källmappen hello finns i hello först för målmappen. hello målfilerna har genereras automatiskt namn. <br/><br/><b>MergeFiles (standard):</b> sammanfogar alla filer från hello källfil mappen tooone. Om hello fil/Blobbnamnet anges skulle hello kopplade filnamnet vara hello angivna name; annars skulle vara automatiskt genererade filnamn. |Nej |

#### <a name="example-blobsink"></a>Exempel: BlobSink

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information finns i [Azure Blob-koppling](data-factory-azure-blob-connector.md#copy-activity-properties) artikel. 

## <a name="azure-data-lake-store"></a>Azure Data Lake Store

### <a name="linked-service"></a>Länkad tjänst
toodefine en länkad Azure Data Lake Store-tjänsten, ange hello typ av hello länkade tjänsten för**AzureDataLakeStore**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ | hello Typegenskapen måste anges till: **AzureDataLakeStore** | Ja |
| dataLakeStoreUri | Ange information om hello Azure Data Lake Store-konto. Det är i hello följande format: `https://[accountname].azuredatalakestore.net/webhdfs/v1` eller `adl://[accountname].azuredatalakestore.net/`. | Ja |
| subscriptionId | Azure-prenumeration Id toowhich Data Lake Store tillhör. | Krävs för sink |
| resourceGroupName | Azure-resurs grupp namnet toowhich Data Lake Store tillhör. | Krävs för sink |
| servicePrincipalId | Ange hello programmets klient-ID. | Ja (för tjänstens huvudnamn autentisering) |
| servicePrincipalKey | Ange hello programnyckel. | Ja (för tjänstens huvudnamn autentisering) |
| Klient | Ange hello klient information (domain name eller klient ID) under där programmet finns. Du kan hämta den med hovra hello musen i hello övre högra hörnet av hello Azure-portalen. | Ja (för tjänstens huvudnamn autentisering) |
| Auktorisering | Klicka på **auktorisera** knapp i hello **Data Factory-redigeraren** och ange dina autentiseringsuppgifter som tilldelar hello autogenererade auktorisering URL toothis-egenskapen. | Ja (för autentisering av autentiseringsuppgifter för användare)|
| Sessions-ID | OAuth sessions-id från hello OAuth-auktorisering session. Varje sessions-id är unikt och får endast användas en gång. Den här inställningen genereras automatiskt när du använder Data Factory-redigeraren. | Ja (för autentisering av autentiseringsuppgifter för användare) |

#### <a name="example-using-service-principal-authentication"></a>Exempel: med service principal autentisering
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info. Example: microsoft.onmicrosoft.com>"
        }
    }
}
```

#### <a name="example-using-user-credential-authentication"></a>Exempel: med hjälp av användarautentisering för autentiseringsuppgifter
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

Mer information finns i [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine ett Azure Data Lake Store-dataset set hello **typen** på hello datamängd för**AzureDataLakeStore**, och ange följande egenskaper i hello hello **typeProperties**avsnitt: 

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| folderPath |Lagra sökvägen toohello behållaren och mappen i hello Azure Data Lake. |Ja |
| fileName |Namnet på hello fil hello Azure Data Lake store. Filnamnet är valfria och skiftlägeskänsligt. <br/><br/>Om du anger ett filnamn, fungerar hello aktivitet (inklusive kopia) för specifik hello-fil.<br/><br/>Om filnamnet har angetts innehåller kopiera alla filer i hello folderPath för inkommande dataset.<br/><br/>Om filnamnet inte anges för en utdatauppsättningen hello namnet på hello genereras skulle vara i hello efter det här formatet: Data. <Guid>.txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nej |
| partitionedBy |partitionedBy är en valfri egenskap. Du kan använda det toospecify dynamiska folderPath och filnamnet för tid series-data. Exempelvis kan folderPath parameteriseras för varje timme av data. |Nej |
| Format | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Ange hello **typen** egenskap under format tooone av dessa värden. Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. <br><br> Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner. |Nej |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**. Nivåer som stöds är: **Optimal** och **snabbast**. Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |

#### <a name="example"></a>Exempel
```json
{
    "name": "AzureDataLakeStoreInput",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Mer information finns i [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#dataset-properties) artikel. 

### <a name="azure-data-lake-store-source-in-copy-activity"></a>Azure Data Lake Store-källan i en Kopieringsaktivitet
Om du vill kopiera data från ett Azure Data Lake Store, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**AzureDataLakeStoreSource**, och ange följande egenskaper i hello **källa**  avsnitt:

**AzureDataLakeStoreSource** stöder följande egenskaper hello **typeProperties** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| Rekursiva |Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen. |SANT (standardvärdet), FALSKT |Nej |

#### <a name="example-azuredatalakestoresource"></a>Exempel: AzureDataLakeStoreSource

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureDakeLaketoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureDataLakeStoreInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureDataLakeStoreSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information finns i [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) artikel.

### <a name="azure-data-lake-store-sink-in-copy-activity"></a>Azure Data Lake Store mottagare i en Kopieringsaktivitet
Om du kopierar data tooan Azure Data Lake Store, ange hello **sink typen** av hello kopieringsaktiviteten för**AzureDataLakeStoreSink**, och ange följande egenskaper i hello **sink**avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| copyBehavior |Anger hello kopiera beteende. |<b>PreserveHierarchy</b>: bevarar hello filstruktur i hello målmapp. hello relativa sökvägen till källmappen för filen toosource är identiska toohello relativa sökvägen till filen tootarget mapp.<br/><br/><b>FlattenHierarchy</b>: alla filer från källmappen hello skapas i hello första nivån i målmappen. hello mål filer skapas med namnet genereras automatiskt.<br/><br/><b>MergeFiles</b>: sammanfogar alla filer från hello källfil mappen tooone. Om hello fil/Blobbnamnet anges skulle hello kopplade filnamnet vara hello angivna name; annars skulle vara automatiskt genererade filnamn. |Nej |

#### <a name="example-azuredatalakestoresink"></a>Exempel: AzureDataLakeStoreSink
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoDataLake",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureDataLakeStoreOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information finns i [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) artikel. 

## <a name="azure-cosmos-db"></a>Azure Cosmos DB  

### <a name="linked-service"></a>Länkad tjänst
toodefine en Azure-Cosmos-DB länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**DocumentDb**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| **Egenskap** | **Beskrivning** | **Krävs** |
| --- | --- | --- |
| connectionString |Ange information som behövs för tooconnect tooAzure Cosmos-DB-databas. |Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "CosmosDBLinkedService",
    "properties": {
        "type": "DocumentDb",
        "typeProperties": {
            "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
    }
}
```
Mer information finns i [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Datauppsättning
toodefine en Azure Cosmos DB datauppsättning set hello **typen** på hello datamängd för**DocumentDbCollection**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| **Egenskap** | **Beskrivning** | **Krävs** |
| --- | --- | --- |
| Samlingsnamn |Namnet på hello Azure DB som Cosmos-samling. |Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "PersonCosmosDBTable",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "CosmosDBLinkedService",
        "typeProperties": {
            "collectionName": "Person"
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```
Mer information finns i [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#dataset-properties) artikel.

### <a name="azure-cosmos-db-collection-source-in-copy-activity"></a>Azure Cosmos DB samling källa i en Kopieringsaktivitet
Om du vill kopiera data från en Azure-Cosmos-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**DocumentDbCollectionSource**, och ange följande egenskaper i hello **källa** avsnitt:


| **Egenskap** | **Beskrivning** | **Tillåtna värden** | **Krävs** |
| --- | --- | --- | --- |
| DocumentDB |Ange hello frågan tooread data. |Frågesträng stöds av Azure Cosmos DB. <br/><br/>Exempel:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Nej <br/><br/>Om inget anges hello SQL-instruktionen som körs:`select <columns defined in structure> from mycollection` |
| nestingSeparator |Specialtecken tooindicate som hello dokumentet är kapslad |Valfritt tecken. <br/><br/>Azure Cosmos-DB är en NoSQL store för JSON-dokument, där kapslade strukturer är tillåtna. Azure Data Factory aktiverar toodenote användarhierarkin via nestingSeparator, vilket är ””. i hello exemplen ovan. Med hello avgränsare hello kopieringsaktiviteten genererar hello ”Name”-objekt med tre underordnade element första mellersta och sista, bl.a too"Name.First”, ”Name.Middle” och ”Name.Last” i hello tabell definition. |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "DocDbToBlobPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "DocumentDbCollectionSource",
                    "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                    "nestingSeparator": "."
                },
                "sink": {
                    "type": "BlobSink",
                    "blobWriterAddHeader": true,
                    "writeBatchSize": 1000,
                    "writeBatchTimeout": "00:00:59"
                }
            },
            "inputs": [{
                "name": "PersonCosmosDBTable"
            }],
            "outputs": [{
                "name": "PersonBlobTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromCosmosDbToBlob"
        }],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00"
    }
}
```

### <a name="azure-cosmos-db-collection-sink-in-copy-activity"></a>Azure Cosmos DB samling mottagare i en Kopieringsaktivitet
Om du kopierar data tooAzure Cosmos DB ange hello **sink typen** av hello kopieringsaktiviteten för**DocumentDbCollectionSink**, och ange följande egenskaper i hello **sink**avsnitt:

| **Egenskap** | **Beskrivning** | **Tillåtna värden** | **Krävs** |
| --- | --- | --- | --- |
| nestingSeparator |Det krävs ett specialtecken i hello källa kolumnen namn tooindicate som kapslade dokument. <br/><br/>Till exempel ovan: `Name.First` i hello utdata producerar tabellen hello följande JSON-strukturen i hello Cosmos DB dokument:<br/><br/>”Name”: {<br/>    ”Första”: ”John”<br/>}, |Tecken som används tooseparate kapslingsnivåer.<br/><br/>Standardvärdet är `.` (punkt). |Tecken som används tooseparate kapslingsnivåer. <br/><br/>Standardvärdet är `.` (punkt). |
| writeBatchSize |Antalet parallella begäranden tooAzure Cosmos DB toocreate servicedokument.<br/><br/>Du kan finjustera hello prestanda vid kopiering av data till och från Azure Cosmos DB genom att använda den här egenskapen. Du kan förvänta dig bättre prestanda om du ökar writeBatchSize eftersom flera parallella begäranden tooAzure Cosmos DB skickas. Men du behöver tooavoid begränsning som kan utlösa hello felmeddelande: ”begär frekvensen är stor”.<br/><br/>Begränsning bestäms av ett antal faktorer, bland annat storlek dokument, antalet villkoren i dokument, indexering princip målsamling osv. Kopieringen, kan du använda en bättre samling (till exempel S3) toohave hello de flesta genomströmning tillgänglig (2 500 begärande enheter per sekund). |Integer |Nej (standard: 5) |
| writeBatchTimeout |Vänta tills hello åtgärden toocomplete innan tidsgränsen uppnås. |TimeSpan<br/><br/> Exempel ”: 00: 30:00” (30 minuter). |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "BlobToDocDbPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "DocumentDbCollectionSink",
                    "nestingSeparator": ".",
                    "writeBatchSize": 2,
                    "writeBatchTimeout": "00:00:00"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, title: aaaTitle, Suffix: Suffix"
                }
            },
            "inputs": [{
                "name": "PersonBlobTableIn"
            }],
            "outputs": [{
                "name": "PersonCosmosDbTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromBlobToCosmosDb"
        }],
        "start": "2016-04-14T00:00:00",
        "end": "2016-04-15T00:00:00"
    }
}
```

Mer information finns i [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#copy-activity-properties) artikel.

## <a name="azure-sql-database"></a>Azure SQL Database

### <a name="linked-service"></a>Länkad tjänst
toodefine en Azure SQL Database länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureSqlDatabase**, och ange följande egenskaper i hello **typeProperties**avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| connectionString |Ange nödvändig information tooconnect toohello Azure SQL Database-instans för hello-egenskapen connectionString. |Ja |

#### <a name="example"></a>Exempel
```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Mer information finns i [Azure SQL-anslutningen](data-factory-azure-sql-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine en Azure SQL Database-datauppsättning set hello **typen** på hello datamängd för**AzureSqlTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabellen eller vyn i hello Azure SQL Database-instans som den länkade tjänsten refererar till. |Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
Mer information finns i [Azure SQL-anslutningen](data-factory-azure-sql-connector.md#dataset-properties) artikel. 

### <a name="sql-source-in-copy-activity"></a>SQL-datakälla i en Kopieringsaktivitet
Om du vill kopiera data från en Azure SQL Database, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**SqlSource**, och ange följande egenskaper i hello **källa** avsnitt:


| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| sqlReaderQuery |Använda hello anpassad fråga tooread data. |SQL-sträng. Exempel: `select * from MyTable`. |Nej |
| sqlReaderStoredProcedureName |Namnet på hello lagrad procedur som läser data från hello källtabellen. |Namnet på hello lagrad procedur. |Nej |
| storedProcedureParameters |Parametrar för hello lagrad procedur. |Namn/värde-par. Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar. |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Mer information finns i [Azure SQL-anslutningen](data-factory-azure-sql-connector.md#copy-activity-properties) artikel. 

### <a name="sql-sink-in-copy-activity"></a>SQL-mottagare i en Kopieringsaktivitet
Om du kopierar data tooAzure SQL-databas, ange hello **sink typen** av hello kopieringsaktiviteten för**SqlSink**, och ange följande egenskaper i hello **sink** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| writeBatchTimeout |Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås. |TimeSpan<br/><br/> Exempel ”: 00: 30:00” (30 minuter). |Nej |
| writeBatchSize |Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize. |Heltal (antalet rader) |Nej (standard: 10000) |
| sqlWriterCleanupScript |Ange en fråga för aktiviteten kopiera tooexecute så att data för ett visst segment har rensats bort. |En frågesats. |Nej |
| sliceIdentifierColumnName |Ange ett kolumnnamn för Kopieringsaktiviteten toofill med genereras automatiskt segment-ID som har använt tooclean in data för ett visst segment när köras på nytt. |Kolumnnamnet för en kolumn med datatypen för binary(32). |Nej |
| sqlWriterStoredProcedureName |Namnet på hello lagrad procedur upserts (uppdateringar/infogar) data i hello måltabellen. |Namnet på hello lagrad procedur. |Nej |
| storedProcedureParameters |Parametrar för hello lagrad procedur. |Namn/värde-par. Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar. |Nej |
| sqlWriterTableType |Ange tabellen typen namnet toobe används i hello lagrade proceduren. Kopieringsaktiviteten tillgängliggör hello data flyttas i en temporär tabell med den här tabellen. Lagrade procedurer kan sedan koppla hello data kopieras med befintliga data. |Ett namn för tabellen. |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information finns i [Azure SQL-anslutningen](data-factory-azure-sql-connector.md#copy-activity-properties) artikel. 

## <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

### <a name="linked-service"></a>Länkad tjänst
toodefine ett Azure SQL Data Warehouse länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureSqlDW**, och ange följande egenskaper i hello **typeProperties**avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| connectionString |Ange nödvändig information tooconnect toohello Azure SQL Data Warehouse-instans för hello-egenskapen connectionString. |Ja |



#### <a name="example"></a>Exempel

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Mer information finns i [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine en Azure SQL Data Warehouse-datauppsättning set hello **typen** på hello datamängd för**AzureSqlDWTable**, och ange följande egenskaper i hello hello **typeProperties**avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabellen eller vyn i hello Azure SQL Data Warehouse-databas som hello länkade tjänsten refererar till. |Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "AzureSqlDWInput",
    "properties": {
    "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Mer information finns i [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) artikel. 

### <a name="sql-dw-source-in-copy-activity"></a>SQL DW-datakälla i en Kopieringsaktivitet
Om du vill kopiera data från Azure SQL Data Warehouse, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**SqlDWSource**, och ange följande egenskaper i hello **källa**avsnitt:


| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| sqlReaderQuery |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: `select * from MyTable`. |Nej |
| sqlReaderStoredProcedureName |Namnet på hello lagrad procedur som läser data från hello källtabellen. |Namnet på hello lagrad procedur. |Nej |
| storedProcedureParameters |Parametrar för hello lagrad procedur. |Namn/värde-par. Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar. |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLDWtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSqlDWInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlDWSource",
                    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information finns i [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) artikel. 

### <a name="sql-dw-sink-in-copy-activity"></a>SQL DW mottagare i en Kopieringsaktivitet
Om du kopierar data tooAzure SQL Data Warehouse, ange hello **sink typen** av hello kopieringsaktiviteten för**SqlDWSink**, och ange följande egenskaper i hello **sink** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Ange en fråga för aktiviteten kopiera tooexecute så att data för ett visst segment har rensats bort. |En frågesats. |Nej |
| allowPolyBase |Anger om toouse PolyBase (i förekommande fall) i stället för BULKINSERT mekanism. <br/><br/> **Hello rekommenderat sätt tooload data till SQL Data Warehouse är med PolyBase.** |True <br/>FALSKT (standard) |Nej |
| polyBaseSettings |En grupp egenskaper som kan anges när hello **allowPolybase** egenskapen för**SANT**. |&nbsp; |Nej |
| rejectValue |Anger antalet hello eller procentandelen rader som kan avvisas innan hello frågan inte kunde köras. <br/><br/>Lär dig mer om hello PolyBase avvisa alternativ i hello **argument** avsnitt i [Skapa extern tabell (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) avsnittet. |0 (standard), 1, 2... |Nej |
| rejectType |Anger om hello rejectValue alternativet har angetts som ett litteralvärde eller ett procentvärde. |Värdet (standard), procent |Nej |
| rejectSampleValue |Anger hello antalet rader tooretrieve innan hello PolyBase beräknar hello procentandelen Avvisade rader. |1, 2, … |Ja, om **rejectType** är **procent** |
| useTypeDefault |Anger hur toohandle saknar värden i avgränsade textfiler när PolyBase hämtar data från hello textfil.<br/><br/>Mer information om den här egenskapen från hello argument-avsnittet i [skapa externt FILFORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). |SANT, FALSKT (standard) |Nej |
| writeBatchSize |Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize |Heltal (antalet rader) |Nej (standard: 10000) |
| writeBatchTimeout |Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås. |TimeSpan<br/><br/> Exempel ”: 00: 30:00” (30 minuter). |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQLDW",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlDWOutput"
            }],
            "typeProperties": {
                "source": {
                "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlDWSink",
                    "allowPolyBase": true
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information finns i [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) artikel. 

## <a name="azure-search"></a>Azure Search

### <a name="linked-service"></a>Länkad tjänst
toodefine ett Azure Search länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureSearch**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| -------- | ----------- | -------- |
| URL: en | URL för hello Azure Search-tjänsten. | Ja |
| key | Admin-nyckel för hello Azure Search-tjänsten. | Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

Mer information finns i [Azure Search connector](data-factory-azure-search-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Datauppsättning
toodefine en Azure Search-datauppsättning set hello **typen** på hello datamängd för**AzureSearchIndex**, och ange följande egenskaper i hello hello **typeProperties** avsnitt : 

| Egenskap | Beskrivning | Krävs |
| -------- | ----------- | -------- |
| typ | hello Typegenskapen måste anges för**AzureSearchIndex**.| Ja |
| Indexnamn | Namnet på hello Azure Search index. Data Factory skapar inte hello index. hello index måste finnas i Azure Search. | Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties": {
            "indexName": "products"
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
    }
}
```

Mer information finns i [Azure Search connector](data-factory-azure-search-connector.md#dataset-properties) artikel.

### <a name="azure-search-index-sink-in-copy-activity"></a>Azure Search Index mottagare i en Kopieringsaktivitet
Om du kopierar data tooan Azure Search index, ange hello **sink typen** av hello kopieringsaktiviteten för**AzureSearchIndexSink**, och ange följande egenskaper i hello **sink**avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Anger om toomerge eller ersätta när ett dokument det redan finns i hello index. | sammanfoga (standard)<br/>Ladda upp| Nej |
| writeBatchSize | Överför data till hello Azure Search index när hello buffertstorlek når writeBatchSize. | 1 too1 000. Standardvärdet är 1000. | Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoAzureSearchIndex",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureSearchIndexDataset"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "AzureSearchIndexSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information finns i [Azure Search connector](data-factory-azure-search-connector.md#copy-activity-properties) artikel.

## <a name="azure-table-storage"></a>Azure Table Storage

### <a name="linked-service"></a>Länkad tjänst
Det finns två typer av länkade tjänster: Azure länkade lagringstjänsten och Azure Storage SAS länkade tjänsten.

#### <a name="azure-storage-linked-service"></a>Länkad Azure Storage-tjänst
toolink din Azure storage-konto tooa data factory med hjälp av hello **kontonyckel**, skapa en länkad Azure Storage-tjänst. toodefine ett Azure Storage länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureStorage**. Du kan sedan ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ |hello Typegenskapen måste anges till: **AzureStorage** |Ja |
| connectionString |Ange nödvändig information tooconnect tooAzure lagring för hello-egenskapen connectionString. |Ja |

**Exempel:**  

```json
{  
    "name": "StorageLinkedService",  
    "properties": {  
        "type": "AzureStorage",  
        "typeProperties": {  
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
        }  
    }  
}  
```

#### <a name="azure-storage-sas-linked-service"></a>Azure Storage SAS länkade tjänsten
hello Azure Storage SAS länkad tjänst kan du toolink ett Azure Storage-konto tooan Azure data factory med hjälp av en delad signatur åtkomst (SAS). Det ger hello data factory med begränsad/Tidsbundna åtkomst tooall utvalda resurser (blobbehållare) i hello lagring. toolink din Azure storage-konto tooa data factory med signatur för delad åtkomst, skapa en Azure Storage SAS länkad tjänst. toodefine ett Azure Storage SAS länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureStorageSas**. Du kan sedan ange följande egenskaper i hello **typeProperties** avsnitt:   

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ |hello Typegenskapen måste anges till: **AzureStorageSas** |Ja |
| sasUri |Ange URI för delad åtkomst-signatur toohello Azure Storage-resurser, till exempel blob, behållare eller tabellen. |Ja |

**Exempel:**

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

Mer information om dessa länkade tjänster finns [Azure Table Storage connector](data-factory-azure-table-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine en Azure Table-datauppsättning set hello **typen** på hello datamängd för**AzureTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i hello Azure Table-databasinstansen som den länkade tjänsten refererar till. |Ja. När ett tabellnamn har angetts utan ett azureTableSourceQuery, är alla poster från hello tabell kopierade toohello mål. Om en azureTableSourceQuery också anges är poster från hello-tabell som uppfyller frågan hello kopierade toohello mål. |

#### <a name="example"></a>Exempel

```json
{
    "name": "AzureTableInput",
    "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Mer information om dessa länkade tjänster finns [Azure Table Storage connector](data-factory-azure-table-connector.md#dataset-properties) artikel. 

### <a name="azure-table-source-in-copy-activity"></a>Azure Tabellkälla i en Kopieringsaktivitet
Om du vill kopiera data från Azure Table Storage, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**AzureTableSource**, och ange följande egenskaper i hello **källa**avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| azureTableSourceQuery |Använda hello anpassad fråga tooread data. |Azure-tabellen frågesträngen. Se exemplen i hello nästa avsnitt. |Nej. När ett tabellnamn har angetts utan ett azureTableSourceQuery, är alla poster från hello tabell kopierade toohello mål. Om en azureTableSourceQuery också anges är poster från hello-tabell som uppfyller frågan hello kopierade toohello mål. |
| azureTableSourceIgnoreTableNotFound |Ange om swallow hello undantag av tabellen inte finns. |SANT<br/>FALSKT |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureTabletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureTableSource",
                    "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information om dessa länkade tjänster finns [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) artikel. 

### <a name="azure-table-sink-in-copy-activity"></a>Azure-tabellen mottagare i en Kopieringsaktivitet
Om du kopierar data tooAzure Table Storage, ange hello **sink typen** av hello kopieringsaktiviteten för**AzureTableSink**, och ange följande egenskaper i hello **sink** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Standard partitionsnyckelvärde som kan användas av hello mottagare. |Ett strängvärde. |Nej |
| azureTablePartitionKeyName |Ange namnet på hello kolumn vars värden används som partitionsnycklar. Om inget anges används AzureTableDefaultPartitionKeyValue som hello partitionsnyckel. |Ett kolumnnamn. |Nej |
| azureTableRowKeyName |Ange namnet på hello kolumn vars kolumnvärdena används som radnyckel. Om inget annat anges, kan du använda ett GUID för varje rad. |Ett kolumnnamn. |Nej |
| azureTableInsertType |hello läge tooinsert data till Azure-tabellen.<br/><br/>Den här egenskapen anger om befintliga rader i hello utdatatabell med matchande partition och radnycklar få sina värden bytas ut eller samman. <br/><br/>toolearn om hur dessa inställningar (dokument och Ersätt) fungerar, se [Insert- eller Merge-entiteten](https://msdn.microsoft.com/library/azure/hh452241.aspx) och [infoga eller ersätta entiteten](https://msdn.microsoft.com/library/azure/hh452242.aspx) avsnitt. <br/><br> Den här inställningen gäller vid hello raden nivån, inte hello tabell nivån, varken alternativet tar bort rader i hello utdatatabell som inte finns i hello indata. |sammanfoga (standard)<br/>Ersätt |Nej |
| writeBatchSize |Infogar data i hello Azure-tabellen när hello writeBatchSize eller writeBatchTimeout namn. |Heltal (antalet rader) |Nej (standard: 10000) |
| writeBatchTimeout |Infogar data i hello Azure-tabellen när hello writeBatchSize eller writeBatchTimeout namn |TimeSpan<br/><br/>Exempel ”: 00: 20:00” (20 minuter) |Nej (standard toostorage klienten standardtimeout-värdet 90 sek) |

#### <a name="example"></a>Exempel

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureTableOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureTableSink",
                    "writeBatchSize": 100,
                    "writeBatchTimeout": "01:00:00"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Mer information om dessa länkade tjänster finns [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) artikel. 

## <a name="amazon-redshift"></a>Amazon RedShift

### <a name="linked-service"></a>Länkad tjänst
toodefine en Amazon Redshift länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AmazonRedshift**, och ange följande egenskaper i hello **typeProperties**avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| server |IP-adressen eller värdnamnet namnet på hello Amazon Redshift server. |Ja |
| port |hello antal hello TCP-port som hello Amazon Redshift server använder toolisten för klientanslutningar. |Nej, standardvärde: 5439 |
| Databasen |Namnet på hello Amazon Redshift databas. |Ja |
| användarnamn |Namnet på användaren som har åtkomst till toohello databas. |Ja |
| lösenord |Lösenordet för användarkontot för hello. |Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties": {
        "type": "AmazonRedshift",
        "typeProperties": {
            "server": "<Amazon Redshift host name or IP address>",
            "port": 5439,
            "database": "<database name>",
            "username": "user",
            "password": "password"
        }
    }
}
```

Mer information finns i [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine en Amazon Redshift datauppsättning set hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i hello Amazon Redshift databas som den länkade tjänsten refererar till. |Nej (om **frågan** av **RelationalSource** har angetts) |


#### <a name="example"></a>Exempel

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
Mer information finns i [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationella källa i en Kopieringsaktivitet 
Om du vill kopiera data från Amazon Redshift, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: `select * from MyTable`. |Nej (om **tableName** av **dataset** har angetts) |

#### <a name="example"></a>Exempel

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonRedshiftInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonRedshiftToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
Mer information finns i [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#copy-activity-properties) artikel.

## <a name="ibm-db2"></a>IBM DB2

### <a name="linked-service"></a>Länkad tjänst
toodefine en IBM DB2 länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesDB2**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| server |Namnet på hello DB2-server. |Ja |
| Databasen |Namnet på hello DB2-databas. |Ja |
| Schemat |Namnet på hello schema i hello-databasen. hello schemanamnet är skiftlägeskänslig. |Nej |
| AuthenticationType |Typ av autentisering används tooconnect toohello DB2-databas. Möjliga värden är: anonym, grundläggande och Windows. |Ja |
| användarnamn |Ange användarnamnet om du använder grundläggande eller Windows-autentisering. |Nej |
| lösenord |Ange lösenord för hello-användarkonto som du angav för hello användarnamn. |Nej |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala DB2-databas. |Ja |

#### <a name="example"></a>Exempel
```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
Mer information finns i [IBM DB2-koppling](#data-factory-onprem-db2-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Datauppsättning
toodefine en DB2 dataset, ange hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i hello DB2-databasinstansen som den länkade tjänsten refererar till. hello tableName är skiftlägeskänslig. |Nej (om **frågan** av **RelationalSource** har angetts) 

#### <a name="example"></a>Exempel
```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Mer information finns i [IBM DB2-koppling](#data-factory-onprem-db2-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationella källa i en Kopieringsaktivitet
Om du vill kopiera data från IBM DB2, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:


| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: `"query": "select * from "MySchema"."MyTable""`. |Nej (om **tableName** av **dataset** har angetts) |

#### <a name="example"></a>Exempel
```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"Orders\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "Db2DataSet"
            }],
            "outputs": [{
                "name": "AzureBlobDb2DataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Db2ToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
Mer information finns i [IBM DB2-koppling](#data-factory-onprem-db2-connector.md#copy-activity-properties) artikel.

## <a name="mysql"></a>MySQL

### <a name="linked-service"></a>Länkad tjänst
toodefine en MySQL länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesMySql**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| server |Namnet på hello MySQL-server. |Ja |
| Databasen |Namnet på hello MySQL-databas. |Ja |
| Schemat |Namnet på hello schema i hello-databasen. |Nej |
| AuthenticationType |Typ av autentisering används tooconnect toohello MySQL-databas. Möjliga värden är: `Basic`. |Ja |
| användarnamn |Ange användarens namn tooconnect toohello MySQL-databas. |Ja |
| lösenord |Ange lösenord för hello-användarkonto som du angav. |Ja |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala MySQL-databas. |Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "OnPremMySqlLinkedService",
    "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
            "server": "<server name>",
            "database": "<database name>",
            "schema": "<schema name>",
            "authenticationType": "<authentication type>",
            "userName": "<user name>",
            "password": "<password>",
            "gatewayName": "<gateway>"
        }
    }
}
```

Mer information finns i [MySQL connector](data-factory-onprem-mysql-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine en MySQL-dataset set hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i hello MySQL-databasinstans som den länkade tjänsten refererar till. |Nej (om **frågan** av **RelationalSource** har angetts) |

#### <a name="example"></a>Exempel

```json
{
    "name": "MySqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremMySqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
Mer information finns i [MySQL connector](data-factory-onprem-mysql-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationella källa i en Kopieringsaktivitet
Om du vill kopiera data från en MySQL-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:


| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: `select * from MyTable`. |Nej (om **tableName** av **dataset** har angetts) |


#### <a name="example"></a>Exempel
```json
{
    "name": "CopyMySqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MySqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobMySqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MySqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Mer information finns i [MySQL connector](data-factory-onprem-mysql-connector.md#copy-activity-properties) artikel. 

## <a name="oracle"></a>Oracle 

### <a name="linked-service"></a>Länkad tjänst
toodefine en Oracle länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesOracle**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| driverType | Ange vilka drivrutinen toouse toocopy data från / tooOracle databasen. Tillåtna värden är **Microsoft** eller **ODP** (standard). Se [stöds version och vilka installationsalternativ](#supported-versions-and-installation) avsnitt för mer information. | Nej |
| connectionString | Ange nödvändig information tooconnect toohello Oracle-databasinstans för hello-egenskapen connectionString. | Ja |
| gatewayName | Namnet på hello gateway som som används tooconnect toohello lokal Oracle-server |Ja |

#### <a name="example"></a>Exempel
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Mer information finns i [Oracle connector](data-factory-onprem-oracle-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Datauppsättning
toodefine en Oracle-datauppsättning set hello **typen** på hello datamängd för**OracleTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i hello Oracle-databas som hello länkade tjänsten refererar till. |Nej (om **oracleReaderQuery** av **OracleSource** har angetts) |

#### <a name="example"></a>Exempel

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2016-02-27T12:00:00",
            "frequency": "Hour"
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
Mer information finns i [Oracle connector](data-factory-onprem-oracle-connector.md#dataset-properties) artikel.

### <a name="oracle-source-in-copy-activity"></a>Oracle-källan i en Kopieringsaktivitet
Om du vill kopiera data från en Oracle-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**OracleSource**, och ange följande egenskaper i hello **källa** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| oracleReaderQuery |Använda hello anpassad fråga tooread data. |SQL-sträng. Exempel: `select * from MyTable` <br/><br/>Om inget anges hello SQL-instruktionen som körs:`select * from MyTable` |Nej (om **tableName** av **dataset** har angetts) |

#### <a name="example"></a>Exempel

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " OracleInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "OracleSource",
                    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information finns i [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) artikel.

### <a name="oracle-sink-in-copy-activity"></a>Oracle mottagare i en Kopieringsaktivitet
Om du kopierar data tooam Oracle-databas, ange hello **sink typen** av hello kopieringsaktiviteten för**OracleSink**, och ange följande egenskaper i hello **sink** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| writeBatchTimeout |Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås. |TimeSpan<br/><br/> Exempel: 00:30:00 (30 minuter). |Nej |
| writeBatchSize |Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize. |Heltal (antalet rader) |Nej (standard: 100) |
| sqlWriterCleanupScript |Ange en fråga för aktiviteten kopiera tooexecute så att data för ett visst segment har rensats bort. |En frågesats. |Nej |
| sliceIdentifierColumnName |Ange kolumnnamn för Kopieringsaktiviteten toofill med genereras automatiskt segment-ID som har använt tooclean in data för ett visst segment när köras på nytt. |Kolumnnamnet för en kolumn med datatypen för binary(32). |Nej |

#### <a name="example"></a>Exempel
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-05T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "OracleOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "OracleSink"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Mer information finns i [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) artikel.

## <a name="postgresql"></a>PostgreSQL

### <a name="linked-service"></a>Länkad tjänst
toodefine en PostgreSQL länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesPostgreSql**, och ange följande egenskaper i hello **typeProperties**avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| server |Namnet på hello PostgreSQL-server. |Ja |
| Databasen |Namnet på hello PostgreSQL-databas. |Ja |
| Schemat |Namnet på hello schema i hello-databasen. hello schemanamnet är skiftlägeskänslig. |Nej |
| AuthenticationType |Typ av autentisering används tooconnect toohello PostgreSQL-databas. Möjliga värden är: anonym, grundläggande och Windows. |Ja |
| användarnamn |Ange användarnamnet om du använder grundläggande eller Windows-autentisering. |Nej |
| lösenord |Ange lösenord för hello-användarkonto som du angav för hello användarnamn. |Nej |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala PostgreSQL-databas. |Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
Mer information finns i [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Datauppsättning
toodefine en PostgreSQL-dataset set hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i hello PostgreSQL-databasinstansen som den länkade tjänsten refererar till. hello tableName är skiftlägeskänslig. |Nej (om **frågan** av **RelationalSource** har angetts) |

#### <a name="example"></a>Exempel
```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
Mer information finns i [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationella källa i en Kopieringsaktivitet
Om du vill kopiera data från en PostgreSQL-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa**avsnitt:


| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: ”frågan” ”: Välj * från \"MySchema\".\" Mytable prefix\"”. |Nej (om **tableName** av **dataset** har angetts) |

#### <a name="example"></a>Exempel

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"public\".\"usstates\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "PostgreSqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobPostgreSqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "PostgreSqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Mer information finns i [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#copy-activity-properties) artikel.

## <a name="sap-business-warehouse"></a>SAP Business Warehouse


### <a name="linked-service"></a>Länkad tjänst
toodefine en SAP Business Warehouse (BW) länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**SapBw**, och ange följande egenskaper i hello **typeProperties**avsnitt:  

Egenskap | Beskrivning | Tillåtna värden | Krävs
-------- | ----------- | -------------- | --------
server | Namnet på hello-server på vilken hello SAP BW instansen finns. | Sträng | Ja
systemNumber | System antal hello SAP BW system. | Två siffror decimaltal representeras som en sträng. | Ja
clientId | Klient-ID för hello klienten i hello SAP W system. | Tre siffror decimaltal representeras som en sträng. | Ja
användarnamn | Namnet på hello-användare som har åtkomst toohello SAP-server | Sträng | Ja
lösenord | Lösenordet för hello. | Sträng | Ja
gatewayName | Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokal SAP BW instans. | Sträng | Ja
encryptedCredential | hello krypterade autentiseringsuppgifter strängen. | Sträng | Nej

#### <a name="example"></a>Exempel

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Mer information finns i [SAP Business Warehouse-anslutningsapp](data-factory-sap-business-warehouse-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine en SAP BW dataset set hello **typen** på hello datamängd för**RelationalTable**. Det finns inga typspecifika egenskaper som stöds för hello SAP BW dataset av typen **RelationalTable**.  

#### <a name="example"></a>Exempel

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
Mer information finns i [SAP Business Warehouse-anslutningsapp](data-factory-sap-business-warehouse-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationella källa i en Kopieringsaktivitet
Om du vill kopiera data från SAP Business Warehouse, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa**avsnitt:


| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB | Anger hello MDX-fråga tooread data från hello SAP BW-instans. | MDX-fråga. | Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<MDX query for SAP BW>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapBwDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapBwToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

Mer information finns i [SAP Business Warehouse-anslutningsapp](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) artikel. 

## <a name="sap-hana"></a>SAP HANA

### <a name="linked-service"></a>Länkad tjänst
toodefine en SAP HANA länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**SapHana**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

Egenskap | Beskrivning | Tillåtna värden | Krävs
-------- | ----------- | -------------- | --------
server | Namnet på hello-server på vilken hello SAP HANA instansen finns. Om servern använder en anpassad port, ange `server:port`. | Sträng | Ja
AuthenticationType | Typ av autentisering. | Sträng. ”Basic” eller ”Windows” | Ja 
användarnamn | Namnet på hello-användare som har åtkomst toohello SAP-server | Sträng | Ja
lösenord | Lösenordet för hello. | Sträng | Ja
gatewayName | Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokal SAP HANA-instans. | Sträng | Ja
encryptedCredential | hello krypterade autentiseringsuppgifter strängen. | Sträng | Nej

#### <a name="example"></a>Exempel

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```
Mer information finns i [SAP HANA connector](data-factory-sap-hana-connector.md#linked-service-properties) artikel.
 
### <a name="dataset"></a>Datauppsättning
toodefine en SAP HANA-dataset set hello **typen** på hello datamängd för**RelationalTable**. Det finns inga typspecifika egenskaper som stöds för hello SAP HANA dataset av typen **RelationalTable**. 

#### <a name="example"></a>Exempel

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
Mer information finns i [SAP HANA connector](data-factory-sap-hana-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationella källa i en Kopieringsaktivitet
Om du vill kopiera data från en SAP HANA-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa**avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB | Anger hello SQL-frågan tooread data från hello SAP HANA-instans. | SQL-frågan. | Ja |


#### <a name="example"></a>Exempel


```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<SQL Query for HANA>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapHanaDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapHanaToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

Mer information finns i [SAP HANA connector](data-factory-sap-hana-connector.md#copy-activity-properties) artikel.


## <a name="sql-server"></a>SQL Server

### <a name="linked-service"></a>Länkad tjänst
Du skapar en länkad tjänst av typen **OnPremisesSqlServer** toolink fabriken för tooa en lokal SQL Server-databasen. hello följande tabell ger en beskrivning för JSON-element specifika tooon lokala SQL Server som är länkade tjänsten.

hello följande tabell ger en beskrivning för JSON-element specifika tooSQL länkad Server-tjänsten.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello typegenskapen ska anges till: **OnPremisesSqlServer**. |Ja |
| connectionString |Ange connectionString information som behövs för tooconnect toohello lokala SQL Server-databas med SQL-autentisering eller Windows-autentisering. |Ja |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala SQL Server-databas. |Ja |
| användarnamn |Ange användarnamnet om du använder Windows-autentisering. Exempel: **domainname\\användarnamn**. |Nej |
| lösenord |Ange lösenord för hello-användarkonto som du angav för hello användarnamn. |Nej |

Du kan kryptera autentiseringsuppgifterna med hjälp av hello **ny AzureRmDataFactoryEncryptValue** cmdlet och använda dem i hello anslutningssträngen som visas i följande exempel hello (**EncryptedCredential** egenskapen):  

```json
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a>Exempel: JSON för att använda SQL-autentisering

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a>Exempel: JSON för att använda Windows-autentisering

Om användarnamn och lösenord anges gateway använder dem tooimpersonate hello användaren konto tooconnect toohello lokala SQL Server-databas. Gatewayen ansluter annars toohello SQL Server direkt med hello säkerhetskontexten för Gateway (dess start-konto).

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Mer information finns i [SQL Server-anslutningen](data-factory-sqlserver-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine en SQL Server-dataset set hello **typen** på hello datamängd för**SqlServerTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabellen eller vyn i hello SQL Server-databasinstansen som den länkade tjänsten refererar till. |Ja |

#### <a name="example"></a>Exempel
```json
{
    "name": "SqlServerInput",
    "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlServerLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Mer information finns i [SQL Server-anslutningen](data-factory-sqlserver-connector.md#dataset-properties) artikel. 

### <a name="sql-source-in-copy-activity"></a>SQL-datakälla i en Kopieringsaktivitet
Om du vill kopiera data från en SQL Server-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**SqlSource**, och ange följande egenskaper i hello **källa** avsnitt:


| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| sqlReaderQuery |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: `select * from MyTable`. Kan referera till flera tabeller från hello-databas som refereras av hello inkommande dataset. Om inget anges hello SQL-instruktionen som körs: Välj från mytable prefix. |Nej |
| sqlReaderStoredProcedureName |Namnet på hello lagrad procedur som läser data från hello källtabellen. |Namnet på hello lagrad procedur. |Nej |
| storedProcedureParameters |Parametrar för hello lagrad procedur. |Namn/värde-par. Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar. |Nej |

Om hello **sqlReaderQuery** har angetts för hello SqlSource, hello Kopieringsaktiviteten kör den här frågan mot hello SQL Server-databas källa tooget hello data.

Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).

Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName är hello kolumner har definierats i hello struktur avsnitt används toobuild en select-frågan toorun mot hello SQL Server-databas. Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.

> [!NOTE]
> När du använder **sqlReaderStoredProcedureName**, du behöver fortfarande toospecify ett värde för hello **tableName** egenskap i hello dataset JSON. Det finns inga verifieringar utföras om mot den här tabellen.


#### <a name="example"></a>Exempel
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

I det här exemplet **sqlReaderQuery** har angetts för hello SqlSource. Hej Kopieringsaktiviteten kör den här frågan mot hello SQL Server-databas källdata tooget hello. Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar). Hej sqlReaderQuery kan referera till flera tabeller i hello-databas som refereras av hello inkommande dataset. Det är inte begränsad tooonly hello tabellen som hello datauppsättnings tableName typeProperty.

Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName är hello kolumner har definierats i hello struktur avsnitt används toobuild en select-frågan toorun mot hello SQL Server-databas. Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.

Mer information finns i [SQL Server-anslutningen](data-factory-sqlserver-connector.md#copy-activity-properties) artikel. 

### <a name="sql-sink-in-copy-activity"></a>SQL-mottagare i en Kopieringsaktivitet
Om du kopierar data tooa SQL Server-databas, ange hello **sink typen** av hello kopieringsaktiviteten för**SqlSink**, och ange följande egenskaper i hello **sink** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| writeBatchTimeout |Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås. |TimeSpan<br/><br/> Exempel ”: 00: 30:00” (30 minuter). |Nej |
| writeBatchSize |Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize. |Heltal (antalet rader) |Nej (standard: 10000) |
| sqlWriterCleanupScript |Ange frågan för Kopieringsaktiviteten tooexecute så att data för ett visst segment har rensats bort. Mer information finns i [repeterbarhet](#repeatability-during-copy) avsnitt. |En frågesats. |Nej |
| sliceIdentifierColumnName |Ange kolumnnamn för Kopieringsaktiviteten toofill med genereras automatiskt segment-ID som har använt tooclean in data för ett visst segment när köras på nytt. Mer information finns i [repeterbarhet](#repeatability-during-copy) avsnitt. |Kolumnnamnet för en kolumn med datatypen för binary(32). |Nej |
| sqlWriterStoredProcedureName |Namnet på hello lagrad procedur upserts (uppdateringar/infogar) data i hello måltabellen. |Namnet på hello lagrad procedur. |Nej |
| storedProcedureParameters |Parametrar för hello lagrad procedur. |Namn/värde-par. Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar. |Nej |
| sqlWriterTableType |Ange tabellen typen namnet toobe används i hello lagrade proceduren. Kopieringsaktiviteten tillgängliggör hello data flyttas i en temporär tabell med den här tabellen. Lagrade procedurer kan sedan koppla hello data kopieras med befintliga data. |Ett namn för tabellen. |Nej |

#### <a name="example"></a>Exempel
hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse dessa indata och utdata-datauppsättningar och schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och **sink** typ har angetts för**SqlSink**.

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": " SqlServerOutput "
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information finns i [SQL Server-anslutningen](data-factory-sqlserver-connector.md#copy-activity-properties) artikel. 

## <a name="sybase"></a>Sybase

### <a name="linked-service"></a>Länkad tjänst
toodefine en Sybase länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesSybase**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| server |Namnet på hello Sybase-servern. |Ja |
| Databasen |Namnet på hello Sybase-databas. |Ja |
| Schemat |Namnet på hello schema i hello-databasen. |Nej |
| AuthenticationType |Typ av autentisering används tooconnect toohello Sybase-databas. Möjliga värden är: anonym, grundläggande och Windows. |Ja |
| användarnamn |Ange användarnamnet om du använder grundläggande eller Windows-autentisering. |Nej |
| lösenord |Ange lösenord för hello-användarkonto som du angav för hello användarnamn. |Nej |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala Sybase-databas. |Ja |

#### <a name="example"></a>Exempel
```json
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

Mer information finns i [Sybase connector](data-factory-onprem-sybase-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine en Sybase-dataset set hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i hello Sybase-databasinstansen som den länkade tjänsten refererar till. |Nej (om **frågan** av **RelationalSource** har angetts) |

#### <a name="example"></a>Exempel

```json
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Mer information finns i [Sybase connector](data-factory-onprem-sybase-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationella källa i en Kopieringsaktivitet
Om du vill kopiera data från en Sybase-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:


| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: `select * from MyTable`. |Nej (om **tableName** av **dataset** har angetts) |

#### <a name="example"></a>Exempel

```json
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from DBA.Orders"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "SybaseDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobSybaseDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SybaseToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Mer information finns i [Sybase connector](data-factory-onprem-sybase-connector.md#copy-activity-properties) artikel.

## <a name="teradata"></a>Teradata

### <a name="linked-service"></a>Länkad tjänst
toodefine en Teradata länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesTeradata**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| server |Namnet på hello Teradata-server. |Ja |
| AuthenticationType |Typ av autentisering används tooconnect toohello Teradata-databasen. Möjliga värden är: anonym, grundläggande och Windows. |Ja |
| användarnamn |Ange användarnamnet om du använder grundläggande eller Windows-autentisering. |Nej |
| lösenord |Ange lösenord för hello-användarkonto som du angav för hello användarnamn. |Nej |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala Teradata-databasen. |Ja |

#### <a name="example"></a>Exempel
```json
{
    "name": "OnPremTeradataLinkedService",
    "properties": {
        "type": "OnPremisesTeradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

Mer information finns i [Teradata connector](data-factory-onprem-teradata-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Datauppsättning
toodefine en Teradata-blobbdatauppsättning set hello **typen** på hello datamängd för**RelationalTable**. Det finns för närvarande inga egenskaper som stöds för hello Teradata dataset. 

#### <a name="example"></a>Exempel
```json
{
    "name": "TeradataDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Mer information finns i [Teradata connector](data-factory-onprem-teradata-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationella källa i en Kopieringsaktivitet
Om du vill kopiera data från en Teradata-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa**avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: `select * from MyTable`. |Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "TeradataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobTeradataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "TeradataToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "isPaused": false
    }
}
```

Mer information finns i [Teradata connector](data-factory-onprem-teradata-connector.md#copy-activity-properties) artikel.

## <a name="cassandra"></a>Cassandra


### <a name="linked-service"></a>Länkad tjänst
toodefine en Cassandra länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesCassandra**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| värden |En eller flera IP-adresser eller värdnamn Cassandra servrar.<br/><br/>Ange en kommaavgränsad lista med IP-adresser eller tooall värdservrar namn tooconnect samtidigt. |Ja |
| port |hello TCP-port som hello Cassandra server använder toolisten för klientanslutningar. |Nej, standardvärde: 9042 |
| AuthenticationType |Grundläggande eller anonym |Ja |
| användarnamn |Ange användarnamn för hello användarkonto. |Ja, om authenticationType anges tooBasic. |
| lösenord |Ange lösenordet för användarkontot för hello. |Ja, om authenticationType anges tooBasic. |
| gatewayName |hello namnet på hello-gateway som har använt tooconnect toohello lokalt Cassandra databas. |Ja |
| encryptedCredential |Autentiseringsuppgifter har krypterats av hello gateway. |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "CassandraLinkedService",
    "properties": {
        "type": "OnPremisesCassandra",
        "typeProperties": {
            "authenticationType": "Basic",
            "host": "<cassandra server name or IP address>",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Mer information finns i [Cassandra connector](data-factory-onprem-cassandra-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine datauppsättningars Cassandra set hello **typen** på hello datamängd för**CassandraTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| keyspace |Namnet på hello keyspace eller schema i Cassandra databasen. |Ja (om **frågan** för **CassandraSource** har inte definierats). |
| tableName |Namnet på hello tabell i Cassandra databas. |Ja (om **frågan** för **CassandraSource** har inte definierats). |

#### <a name="example"></a>Exempel

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "<key space>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Mer information finns i [Cassandra connector](data-factory-onprem-cassandra-connector.md#dataset-properties) artikel. 

### <a name="cassandra-source-in-copy-activity"></a>Cassandra källa i en Kopieringsaktivitet
Om du vill kopiera data från Cassandra, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**CassandraSource**, och ange följande egenskaper i hello **källa** avsnitt :

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-92 frågan eller CQL frågan. Se [CQL referens](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>När du använder SQL-frågan anger **keyspace name.table namn** toorepresent hello tabell tooquery. |Nej (om tabellnamn och keyspace för datauppsättningen har definierats). |
| consistencyLevel |hello konsekvensnivå anger hur många repliker måste svara tooa läsbegäran innan det returneras data toohello klientprogrammet. Cassandra kontrollerar hello angivet antal repliker för data toosatisfy hello läsa begäran. |EN, TVÅ, TRE, KVORUM, ALL, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Se [konfigurera datakonsekvens](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) mer information. |Nej. Standardvärdet är en. |

#### <a name="example"></a>Exempel
  
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "CassandraInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information finns i [Cassandra connector](data-factory-onprem-cassandra-connector.md#copy-activity-properties) artikel.

## <a name="mongodb"></a>MongoDB

### <a name="linked-service"></a>Länkad tjänst
toodefine en MongoDB länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesMongoDB**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| server |IP-adressen eller värdnamnet namnet på hello MongoDB-servern. |Ja |
| port |TCP-port som hello MongoDB-servern använder toolisten för klientanslutningar. |Valfritt, standardvärdet: 27017 |
| AuthenticationType |Grundläggande eller anonym. |Ja |
| användarnamn |Användarens konto tooaccess MongoDB. |Ja (om grundläggande autentisering används). |
| lösenord |Lösenordet för hello. |Ja (om grundläggande autentisering används). |
| authSource |Namnet på hello MongoDB-databas som du vill toouse toocheck dina autentiseringsuppgifter för autentisering. |Valfritt (om grundläggande autentisering används). standard: använder hello administratörskonto och hello databas som har angetts med egenskapen databaseName. |
| DatabaseName |Namnet på hello MongoDB-databas som du vill tooaccess. |Ja |
| gatewayName |Namnet på hello-gateway som har åtkomst till hello-datalagret. |Ja |
| encryptedCredential |Autentiseringsuppgifter har krypterats av gateway. |Valfri |

#### <a name="example"></a>Exempel

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties": {
        "type": "OnPremisesMongoDb",
        "typeProperties": {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< hello IP address or host name of hello MongoDB server >",
            "port": "<hello number of hello TCP port that hello MongoDB server uses toolisten for client connections.>",
            "username": "<username>",
            "password": "<password>",
            "authSource": "< hello database that you want toouse toocheck your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Mer information finns i [MongoDB connector artikel](data-factory-on-premises-mongodb-connector.md#linked-service-properties)

### <a name="dataset"></a>Datauppsättning
toodefine en MongoDB-dataset set hello **typen** på hello datamängd för**MongoDbCollection**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| Samlingsnamn |Namnet på hello-samlingen i MongoDB-databas. |Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

Mer information finns i [MongoDB connector artikel](data-factory-on-premises-mongodb-connector.md#dataset-properties)

#### <a name="mongodb-source-in-copy-activity"></a>MongoDB-källan i en Kopieringsaktivitet
Om du vill kopiera data från MongoDB, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**MongoDbSource**, och ange följande egenskaper i hello **källa** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-92 frågesträngen. Till exempel: `select * from MyTable`. |Nej (om **samlingsnamn** av **dataset** har angetts) |

#### <a name="example"></a>Exempel

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "MongoDbSource",
                    "query": "select * from MyTable"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MongoDbInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MongoDBToAzureBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Mer information finns i [MongoDB connector artikel](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)

## <a name="amazon-s3"></a>Amazon S3


### <a name="linked-service"></a>Länkad tjänst
toodefine en Amazon S3 länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AwsAccessKey**, och ange följande egenskaper i hello **typeProperties** avsnitt :  

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| accessKeyID |ID för hello hemliga snabbtangent. |Sträng |Ja |
| secretAccessKey |hello hemliga åtkomst själva nyckeln. |Krypterad hemliga sträng |Ja |

#### <a name="example"></a>Exempel
```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

Mer information finns i [Amazon S3 connector artikel](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).

### <a name="dataset"></a>Datauppsättning
toodefine en Amazon S3 dataset, ange hello **typen** på hello datamängd för**AmazonS3**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| bucketName |hello S3 bucket-namn. |Sträng |Ja |
| key |hello S3 Objektnyckel. |Sträng |Nej |
| prefix |Prefix för hello S3 objekt nyckeln. Objekt vars nycklar som börjar med prefixet är markerade. Gäller endast när nyckeln är tom. |Sträng |Nej |
| Version |hello version av S3 objekt om S3 versionshantering är aktiverat. |Sträng |Nej |
| Format | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Ange hello **typen** egenskap under format tooone av dessa värden. Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. <br><br> Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner. |Nej | |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**. hello som stöds är: **Optimal** och **snabbast**. Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej | |


> [!NOTE]
> bucketName + nyckeln anger hello platsen för hello S3 objekt där bucket är hello Rotbehållare för S3 objekt och nyckeln är hello fullständig sökväg tooS3 objekt.

#### <a name="example-sample-dataset-with-prefix"></a>Exempel: Exempeldatamängd med prefix

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "<S3 bucket name>",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
#### <a name="example-sample-data-set-with-version"></a>Exempel: Exempel datauppsättning (med version)

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "<S3 bucket name>",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

#### <a name="example-dynamic-paths-for-s3"></a>Exempel: Dynamiska sökvägar för S3
I exemplet hello använder vi fasta värden för nyckeln och bucketName egenskaper i hello Amazon S3 dataset.

```json
"key": "testFolder/test.orc",
"bucketName": "<S3 bucket name>",
```

Du kan ha Data Factory beräkna hello nyckel och bucketName dynamiskt vid körning med systemvariabler som SliceStart.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Du kan göra hello samma för hello prefix-egenskapen för en Amazon S3 datauppsättning. Se [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md) en lista över funktioner som stöds och variabler.

Mer information finns i [Amazon S3 connector artikel](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>Källa för System i en Kopieringsaktivitet
Om du vill kopiera data från Amazon S3, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**FileSystemSource**, och ange följande egenskaper i hello **källa** avsnitt :


| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| Rekursiva |Anger om toorecursively lista S3 objekt under hello katalog. |SANT/FALSKT |Nej |


#### <a name="example"></a>Exempel


```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource",
                    "recursive": true
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonS3InputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonS3ToBlob"
        }],
        "start": "2016-08-08T18:00:00",
        "end": "2016-08-08T19:00:00"
    }
}
```

Mer information finns i [Amazon S3 connector artikel](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).

## <a name="file-system"></a>Filsystem


### <a name="linked-service"></a>Länkad tjänst
Du kan länka en lokal fil system tooan Azure data factory med hello **lokala filserver** länkade tjänsten. hello i den följande tabellen finns beskrivningar JSON-element som är specifika toohello lokala filserver länkade tjänsten.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |Kontrollera att hello type-egenskap är inställd för**OnPremisesFileServer**. |Ja |
| värden |Anger hello rotsökvägen för hello mappen som du vill toocopy. Använd hello escape-tecknet ' \ ' för specialtecken i hello-sträng. Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel. |Ja |
| användar-ID |Ange hello-ID för hello-användare som har åtkomst till toohello servern. |Nej (om du väljer encryptedCredential) |
| lösenord |Ange hello användarlösenord hello (användar-ID). |Nej (om du väljer encryptedCredential |
| encryptedCredential |Ange hello krypterade autentiseringsuppgifter som du kan få genom att köra hello ny AzureRmDataFactoryEncryptValue cmdlet. |Nej (om du väljer toospecify användar-ID och lösenord i klartext) |
| gatewayName |Anger hello namnet på hello gateway som Data Factory ska använda tooconnect toohello lokal server. |Ja |

#### <a name="sample-folder-path-definitions"></a>Exempel mappen sökväg definitioner 
| Scenario | Värden i länkade tjänstdefinitionen | folderPath i datauppsättningsdefinitionen |
| --- | --- | --- |
| Lokal mapp på Data Management Gateway-datorn: <br/><br/>Exempel: D:\\ \* eller D:\folder\subfolder\\* |D:\\ \\ (för Data Management Gateway 2.0 och senare) <br/><br/> localhost (för tidigare versioner än Data Management Gateway 2.0) |. \\ \\ eller mappen\\\\undermapp (för Data Management Gateway 2.0 och senare) <br/><br/>D:\\ \\ eller D:\\\\mappen\\\\undermapp (för gateway-versionen nedan 2.0) |
| Delad fjärrmapp: <br/><br/>Exempel: \\ \\minserver\\dela\\ \* eller \\ \\minserver\\dela\\mappen\\undermapp\\* |\\\\\\\\minserver\\\\dela |. \\ \\ eller mappen\\\\undermapp |


#### <a name="example-using-username-and-password-in-plain-text"></a>Exempel: Med användarnamn och lösenord i klartext

```json
{
    "Name": "OnPremisesFileServerLinkedService",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "\\\\Contosogame-Asia",
            "userid": "Admin",
            "password": "123456",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-encryptedcredential"></a>Exempel: Använda encryptedcredential

```json
{
    "Name": " OnPremisesFileServerLinkedService ",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "D:\\",
            "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Mer information finns i [filsystemet connector artikel](data-factory-onprem-file-system-connector.md#linked-service-properties).

### <a name="dataset"></a>Datauppsättning
toodefine datauppsättningars filsystemet set hello **typen** på hello datamängd för**filresursen**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| folderPath |Anger hello undersökvägen toohello mapp. Använd hello escape-tecknet ' \' för specialtecken i hello-sträng. Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.<br/><br/>Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger. |Ja |
| fileName |Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello. Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.<br/><br/>Om filnamnet inte anges för en datamängd för utdata heter hello hello skapas filen i hello följande format: <br/><br/>`Data.<Guid>.txt`(Exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Nej |
| fileFilter |Ange ett filter toobe används tooselect en delmängd av filer i hello folderPath i stället för alla filer. <br/><br/>Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).<br/><br/>Exempel 1: ”fileFilter” ”: * .log”<br/>Exempel 2: ”fileFilter”: 2016 - 1-?. txt ”<br/><br/>Observera att fileFilter gäller för en inkommande filresursen datauppsättning. |Nej |
| partitionedBy |Du kan använda partitionedBy toospecify dynamiska folderPath/filnamn för time series-data. Ett exempel är folderPath parametriserade varje timme av data. |Nej |
| Format | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Ange hello **typen** egenskap under format tooone av dessa värden. Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. <br><br> Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner. |Nej |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**; och nivåer som stöds är: **Optimal** och **snabbast**. Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |

> [!NOTE]
> Du kan inte använda filnamn och fileFilter samtidigt.

#### <a name="example"></a>Exempel

```json
{
    "name": "OnpremisesFileSystemInput",
    "properties": {
        "type": " FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
            "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": [{
                "name": "Year",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                        "format": "yyyy"
                }
            }, {
                "name": "Month",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "MM"
                }
            }, {
                "name": "Day",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "dd"
                }
            }, {
                "name": "Hour",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HH"
                }
            }]
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Mer information finns i [filsystemet connector artikel](data-factory-onprem-file-system-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>Källa för System i en Kopieringsaktivitet
Om du kopierar data från filsystemet, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**FileSystemSource**, och ange följande egenskaper i hello **källa** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| Rekursiva |Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen. |SANT, FALSKT (standard) |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T19:00:00",
        "description": "Pipeline for copy activity",
        "activities": [{
            "name": "OnpremisesFileSystemtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "OnpremisesFileSystemInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Mer information finns i [filsystemet connector artikel](data-factory-onprem-file-system-connector.md#copy-activity-properties).

### <a name="file-system-sink-in-copy-activity"></a>Filsystem mottagare i en Kopieringsaktivitet
Om du kopierar data tooFile System, ange hello **sink typen** av hello kopieringsaktiviteten för**FileSystemSink**, och ange följande egenskaper i hello **sink** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| copyBehavior |Definierar hello kopiera beteende när hello källa är BlobSource eller filsystem. |**PreserveHierarchy:** bevarar hello filen hierarki i hello målmappen. Hello relativ sökväg hello filen toohello källa källmappen är det vill säga hello samma som hello relativa sökväg hello filen toohello mål målmappen.<br/><br/>**FlattenHierarchy:** alla filer från källmappen hello skapas i hello första nivån i målmappen. hello mål filer skapas med ett namn som genererats automatiskt.<br/><br/>**MergeFiles:** sammanfogar alla filer från hello källfil mappen tooone. Om hello namn/blob filnamn har angetts är hello kopplade filnamnet hello angivet namn. Annars är ett automatiskt genererat namn. |Nej |
Auto-

#### <a name="example"></a>Exempel

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T20:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoOnPremisesFile",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "OnpremisesFileSystemOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "FileSystemSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information finns i [filsystemet connector artikel](data-factory-onprem-file-system-connector.md#copy-activity-properties).

## <a name="ftp"></a>FTP

### <a name="linked-service"></a>Länkad tjänst
toodefine en FTP länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**FtpServer**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs | Standard |
| --- | --- | --- | --- |
| värden |Namn eller IP-adress för hello FTP-servern |Ja |&nbsp; |
| AuthenticationType |Ange autentiseringstyp |Ja |Grundläggande, anonyma |
| användarnamn |Användare som har åtkomst till toohello FTP-servern |Nej |&nbsp; |
| lösenord |Lösenordet för hello (användarnamn) |Nej |&nbsp; |
| encryptedCredential |Krypterade autentiseringsuppgifter tooaccess hello FTP-server |Nej |&nbsp; |
| gatewayName |Namnet på hello Data Management Gateway gateway tooconnect tooan lokalt FTP-servern |Nej |&nbsp; |
| port |Port servern lyssnar på vilka hello FTP |Nej |21 |
| enableSsl |Ange om toouse FTP över SSL/TLS-kanalen |Nej |SANT |
| enableServerCertificateValidation |Ange om tooenable server SSL-certifikatet verifieringen när du använder FTP över SSL/TLS-kanalen |Nej |SANT |

#### <a name="example-using-anonymous-authentication"></a>Exempel: Använder anonym autentisering

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
            "typeProperties": {
            "authenticationType": "Anonymous",
            "host": "myftpserver.com"
        }
    }
}
```

#### <a name="example-using-username-and-password-in-plain-text-for-basic-authentication"></a>Exempel: Med användarnamn och lösenord i klartext för grundläggande autentisering

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
    }
}
```

#### <a name="example-using-port-enablessl-enableservercertificatevalidation"></a>Exempel: Med port, enableSsl enableServerCertificateValidation

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

#### <a name="example-using-encryptedcredential-for-authentication-and-gateway"></a>Exempel: Använder encryptedCredential för autentisering och gateway

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
      }
}
```

Mer information finns i [FTP-anslutningen](data-factory-ftp-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Datauppsättning
toodefine en FTP-dataset, ange hello **typ** på hello datamängd för**FileShare**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| folderPath |Sökvägen toohello undermapp. Använda escape-tecknet ' \ ' för specialtecken i hello-sträng. Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.<br/><br/>Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger. |Ja 
| fileName |Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello. Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.<br/><br/>Om filnamnet inte anges för en utdatauppsättningen skulle hello namnet på hello genereras vara i hello efter det här formatet: <br/><br/>Data. <Guid>.txt (exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nej |
| fileFilter |Ange ett filter toobe används tooselect en delmängd av filer i hello folderPath i stället för alla filer.<br/><br/>Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).<br/><br/>Exempel 1:`"fileFilter": "*.log"`<br/>Exempel 2:`"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter gäller för en inkommande filresursen datauppsättning. Den här egenskapen stöds inte med HDFS. |Nej |
| partitionedBy |partitionedBy kan vara används toospecify en dynamisk folderPath filnamn för tid series-data. Till exempel folderPath som innehåller parametrar för varje timme av data. |Nej |
| Format | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Ange hello **typen** egenskap under format tooone av dessa värden. Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. <br><br> Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner. |Nej |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**; och nivåer som stöds är: **Optimal** och **snabbast**. Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |
| useBinaryTransfer |Ange om använder binära överföringsläge. True för en binär och FALSKT ASCII. Standardvärde: True. Den här egenskapen kan endast användas när associerade linked service-typen är av typen: FtpServer. |Nej |

> [!NOTE]
> filnamnet och fileFilter kan inte användas samtidigt.

#### <a name="example"></a>Exempel

```json
{
    "name": "FTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
            "folderPath": "<path tooshared folder>",
            "fileName": "test.csv",
            "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Mer information finns i [FTP-anslutningen](data-factory-ftp-connector.md#dataset-properties) artikel.

### <a name="file-system-source-in-copy-activity"></a>Källa för System i en Kopieringsaktivitet
Om du vill kopiera data från en FTP-server, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**FileSystemSource**, och ange följande egenskaper i hello **källa** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| Rekursiva |Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen. |SANT, FALSKT (standard) |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00",
        "end": "2016-08-24T19:00:00"
    }
}
```

Mer information finns i [FTP-anslutningen](data-factory-ftp-connector.md#copy-activity-properties) artikel.


## <a name="hdfs"></a>HDFS

### <a name="linked-service"></a>Länkad tjänst
toodefine en HDFS länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**Hdfs**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello Typegenskapen måste anges till: **Hdfs** |Ja |
| URL |URL: en toohello HDFS |Ja |
| AuthenticationType |Anonym, eller Windows. <br><br> toouse **Kerberos-autentisering** HDFS-anslutningen finns för[i det här avsnittet](#use-kerberos-authentication-for-hdfs-connector) tooset upp din lokala miljö därefter. |Ja |
| Användarnamn |Användarnamn för Windows-autentisering. |Ja (för Windows-autentisering) |
| lösenord |Lösenordet för Windows-autentisering. |Ja (för Windows-autentisering) |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello HDFS. |Ja |
| encryptedCredential |[Nya AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) utdata från hello autentiseringsuppgifter. |Nej |

#### <a name="example-using-anonymous-authentication"></a>Exempel: Använder anonym autentisering

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-windows-authentication"></a>Exempel: Med Windows-autentisering

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Mer information finns i [HDFS connector](#data-factory-hdfs-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine datauppsättningars HDFS set hello **typen** på hello datamängd för**filresursen**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| folderPath |Sökvägen toohello mapp. Exempel:`myfolder`<br/><br/>Använda escape-tecknet ' \ ' för specialtecken i hello-sträng. Till exempel: Ange mapp för folder\subfolder,\\\\undermapp och ange d: för d:\samplefolder,\\\\Exempelmapp.<br/><br/>Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger. |Ja |
| fileName |Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello. Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.<br/><br/>Om filnamnet inte anges för en utdatauppsättningen skulle hello namnet på hello genereras vara i hello efter det här formatet: <br/><br/>Data. <Guid>.txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nej |
| partitionedBy |partitionedBy kan vara används toospecify en dynamisk folderPath filnamn för tid series-data. Exempel: folderPath parametriserade varje timme av data. |Nej |
| Format | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Ange hello **typen** egenskap under format tooone av dessa värden. Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. <br><br> Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner. |Nej |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**. Nivåer som stöds är: **Optimal** och **snabbast**. Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |

> [!NOTE]
> filnamnet och fileFilter kan inte användas samtidigt.

#### <a name="example"></a>Exempel

```json
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Mer information finns i [HDFS connector](#data-factory-hdfs-connector.md#dataset-properties) artikel. 

### <a name="file-system-source-in-copy-activity"></a>Källa för System i en Kopieringsaktivitet
Om du vill kopiera data från HDFS, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**FileSystemSource**, och ange följande egenskaper i hello **källa** avsnitt:

**FileSystemSource** stöder hello följande egenskaper:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| Rekursiva |Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen. |SANT, FALSKT (standard) |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "HdfsToBlobCopy",
            "inputs": [{
                "name": "InputDataset"
            }],
            "outputs": [{
                "name": "OutputDataset"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Mer information finns i [HDFS connector](#data-factory-hdfs-connector.md#copy-activity-properties) artikel.

## <a name="sftp"></a>SFTP


### <a name="linked-service"></a>Länkad tjänst
toodefine en SFTP länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**Sftp**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- | --- |
| värden | Namn eller IP-adressen för hello SFTP-server. |Ja |
| port |Port på vilken hello SFTP servern lyssnar. hello standardvärdet är: 21 |Nej |
| AuthenticationType |Ange autentiseringstypen. Tillåtna värden: **grundläggande**, **SshPublicKey**. <br><br> Se för[använder grundläggande autentisering](#using-basic-authentication) och [med hjälp av SSH autentisering med offentlig nyckel](#using-ssh-public-key-authentication) respektive avsnitt på fler egenskaper och JSON-exempel. |Ja |
| skipHostKeyValidation | Ange om tooskip värd viktiga validering. | Nej. Hej standardvärdet: false |
| hostKeyFingerprint | Ange hello fingeravtryck hello värden nyckel. | Ja om hello `skipHostKeyValidation` toofalse anges.  |
| gatewayName |Namnet på hello Data Management Gateway tooconnect tooan lokalt SFTP-server. | Ja om du kopierar data från en lokal SFTP-server. |
| encryptedCredential | Krypterade autentiseringsuppgifter tooaccess hello SFTP-server. Genereras automatiskt när du anger grundläggande autentisering (användarnamn och lösenord) eller SshPublicKey autentisering (användarnamn + privat sökväg eller innehåll) i Kopiera guiden eller hello ClickOnce popup-dialogruta. | Nej. Gäller bara när du kopierar data från en lokal SFTP-server. |

#### <a name="example-using-basic-authentication"></a>Exempel: Använder grundläggande autentisering

toouse grundläggande autentisering, ange `authenticationType` som `Basic`, och ange följande egenskaper utöver hello SFTP connector generiska som introducerades i hello sista avsnittet hello:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- | --- |
| användarnamn | Användare som har åtkomst till toohello SFTP-servern. |Ja |
| lösenord | Lösenordet för hello (användarnamn). | Ja |

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<SFTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Exempel: Grundläggande autentisering med krypterade autentiseringsuppgifter **

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="using-ssh-public-key-authentication"></a>Med hjälp av autentisering med SSH offentlig nyckel: **

toouse grundläggande autentisering, ange `authenticationType` som `SshPublicKey`, och ange följande egenskaper utöver hello SFTP connector generiska som introducerades i hello sista avsnittet hello:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- | --- |
| användarnamn |Användare som har åtkomst till toohello SFTP-servern |Ja |
| privateKeyPath | Ange absolut sökväg toohello fil för privat nyckel som gateway kan komma åt. | Ange antingen hello `privateKeyPath` eller `privateKeyContent`. <br><br> Gäller bara när du kopierar data från en lokal SFTP-server. |
| privateKeyContent | En serialiserad textsträng hello privata nyckel innehåll. hello guiden Kopiera kan läsa hello-fil för privat nyckel och extrahera hello privata nyckel innehållet automatiskt. Om du använder någon annan verktyget/SDK, Använd hello privateKeyPath egenskapen i stället. | Ange antingen hello `privateKeyPath` eller `privateKeyContent`. |
| Lösenfrasen | Ange hello pass frasen/lösenord toodecrypt hello privata nyckel om hello nyckelfilen skyddas av ett lösenord. | Ja om hello privata nyckeln skyddas av ett lösenord. |

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Exempel: SshPublicKey autentisering med hjälp av privat nyckel innehåll **

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of hello private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

Mer information finns i [SFTP connector](data-factory-sftp-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine en SFTP-datauppsättning set hello **typen** på hello datamängd för**filresursen**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| folderPath |Sökvägen toohello undermapp. Använda escape-tecknet ' \ ' för specialtecken i hello-sträng. Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.<br/><br/>Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger. |Ja |
| fileName |Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello. Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.<br/><br/>Om filnamnet inte anges för en utdatauppsättningen skulle hello namnet på hello genereras vara i hello efter det här formatet: <br/><br/>Data. <Guid>.txt (exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nej |
| fileFilter |Ange ett filter toobe används tooselect en delmängd av filer i hello folderPath i stället för alla filer.<br/><br/>Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).<br/><br/>Exempel 1:`"fileFilter": "*.log"`<br/>Exempel 2:`"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter gäller för en inkommande filresursen datauppsättning. Den här egenskapen stöds inte med HDFS. |Nej |
| partitionedBy |partitionedBy kan vara används toospecify en dynamisk folderPath filnamn för tid series-data. Till exempel folderPath som innehåller parametrar för varje timme av data. |Nej |
| Format | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Ange hello **typen** egenskap under format tooone av dessa värden. Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. <br><br> Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner. |Nej |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**. Nivåer som stöds är: **Optimal** och **snabbast**. Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |
| useBinaryTransfer |Ange om använder binära överföringsläge. True för en binär och FALSKT ASCII. Standardvärde: True. Den här egenskapen kan endast användas när associerade linked service-typen är av typen: FtpServer. |Nej |

> [!NOTE]
> filnamnet och fileFilter kan inte användas samtidigt.

#### <a name="example"></a>Exempel

```json
{
    "name": "SFTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "SftpLinkedService",
        "typeProperties": {
            "folderPath": "<path tooshared folder>",
            "fileName": "test.csv"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Mer information finns i [SFTP connector](data-factory-sftp-connector.md#dataset-properties) artikel. 

### <a name="file-system-source-in-copy-activity"></a>Källa för System i en Kopieringsaktivitet
Om du kopierar data från en källa för SFTP ange hello **typ av datakälla** av hello kopieringsaktiviteten för**FileSystemSource**, och ange följande egenskaper i hello **källa** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| Rekursiva |Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen. |SANT, FALSKT (standard) |Nej |



#### <a name="example"></a>Exempel

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2017-02-20T18:00:00",
        "end": "2017-02-20T19:00:00"
    }
}
```

Mer information finns i [SFTP connector](data-factory-sftp-connector.md#copy-activity-properties) artikel.


## <a name="http"></a>HTTP

### <a name="linked-service"></a>Länkad tjänst
toodefine en HTTP länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**Http**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| URL: en | Bas-URL: en toohello webbserver | Ja |
| AuthenticationType | Anger hello autentiseringstyp. Tillåtna värden är: **anonym**, **grundläggande**, **sammanfattad**, **Windows**, **ClientCertificate**. <br><br> Läs respektive toosections under den här tabellen på fler egenskaper och JSON-exempel för dessa typer av autentisering. | Ja |
| enableServerCertificateValidation | Ange om tooenable server SSL-certifikatverifieringen om datakällan är HTTPS-webbserver | Nej, standard är SANT |
| gatewayName | Namnet på hello Data Management Gateway tooconnect tooan lokalt http-källa. | Ja om du kopierar data från en lokal http-källa. |
| encryptedCredential | Krypterade autentiseringsuppgifter tooaccess hello HTTP-slutpunkten. Genereras automatiskt när du konfigurerar hello autentiseringsinformation i Kopiera guiden eller hello ClickOnce popup-dialogrutan. | Nej. Gäller bara när du kopierar data från en lokal HTTP-server. |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Exempel: Med grundläggande, sammanfattad eller Windows-autentisering
Ange `authenticationType` som `Basic`, `Digest`, eller `Windows`, och ange följande egenskaper utöver hello HTTP-anslutningen generiska de som introducerats ovan hello:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| användarnamn | Användarnamnet tooaccess hello HTTP-slutpunkten. | Ja |
| lösenord | Lösenordet för hello (användarnamn). | Ja |

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "basic",
            "url": "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

#### <a name="example-using-clientcertificate-authentication"></a>Exempel: Med ClientCertificate autentisering

toouse grundläggande autentisering, ange `authenticationType` som `ClientCertificate`, och ange följande egenskaper utöver hello HTTP-anslutningen generiska de som introducerats ovan hello:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| embeddedCertData | hello Base64-kodade innehåll för binära data i hello Personal Information Exchange (PFX)-fil. | Ange antingen hello `embeddedCertData` eller `certThumbprint`. |
| certThumbprint | Hej tumavtrycket för certifikatet för hello som har installerats på gateway-datorns certifikatarkiv. Gäller bara när du kopierar data från en lokal http-källa. | Ange antingen hello `embeddedCertData` eller `certThumbprint`. |
| lösenord | Lösenordet som är associerat med hello certifikat. | Nej |

Om du använder `certThumbprint` för autentisering och hello certifikat installeras i hello personliga arkivet i hello lokala datorn, behöver du toogrant hello läsbehörighet toohello gateway-tjänsten:

1. Starta Microsoft Management Console (MMC). Lägg till hello **certifikat** snapin-modulen som mål hello **lokal dator**.
2. Expandera **certifikat**, **personliga**, och klicka på **certifikat**.
3. Högerklicka på hello certifikatet från datorarkivet hello och välj **alla aktiviteter**->**hantera privata nycklar...**
3. På hello **säkerhet** lägger du till hello-användarkonto som Data Management Gateway-värdtjänsten körs under med hello läsbehörighet toohello certifikat.  

**Exempel: använder klientcertifikat:** detta länkade tjänsten länkar din data factory tooan lokalt HTTP-server. Den använder ett klientcertifikat som är installerad på datorn hello med Data Management Gateway är installerad.

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"
        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a>Exempel: använder klientcertifikat i en fil
Det här länkade tjänsten länkar din data factory tooan lokalt HTTP-server. Det använder en klient-certifikatfil på hello datorn med Data Management Gateway är installerad.

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

Mer information finns i [HTTP-anslutningen](data-factory-http-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Datauppsättning
toodefine en HTTP-dataset, ange hello **typen** på hello datamängd för**Http**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| relativeUrl | En relativ URL toohello resurs som innehåller hello data. Om sökvägen inte anges används endast hello-URL som anges i tjänstdefinitionen hello länkad. <br><br> dynamisk tooconstruct-URL som du kan använda [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md), exempel: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`. | Nej |
| requestMethod | HTTP-metod. Tillåtna värden är **hämta** eller **efter**. | Nej. Standardvärdet är `GET`. |
| additionalHeaders | Ytterligare HTTP-begärans sidhuvud. | Nej |
| requestBody | Brödtext för HTTP-begäran. | Nej |
| Format | Om du vill toosimply **hämta hello data från HTTP-slutpunkt som-är** hoppa över den här formatinställningar utan parsning den. <br><br> Om du vill tooparse hello HTTP-svar innehåll vid kopiering, hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. |Nej |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**. Nivåer som stöds är: **Optimal** och **snabbast**. Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |

#### <a name="example-using-hello-get-default-method"></a>Exempel: genom att använda metoden för hello GET (standard)

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

#### <a name="example-using-hello-post-method"></a>Exempel: använda hello POST-metoden

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
            "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```
Mer information finns i [HTTP-anslutningen](data-factory-http-connector.md#dataset-properties) artikel.

### <a name="http-source-in-copy-activity"></a>HTTP-källan i en Kopieringsaktivitet
Om du kopierar data från en HTTP-källa, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**HttpSource**, och ange följande egenskaper i hello **källa** avsnitt:

| Egenskap | Beskrivning | Krävs |
| -------- | ----------- | -------- |
| httpRequestTimeout | Hej timeout (TimeSpan) för hello HTTP-begäran tooget ett svar. Det är hello timeout tooget ett svar inte hello timeout tooread svarsdata. | Nej. Standardvärde: 00:01:40 |


#### <a name="example"></a>Exempel

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "HttpSourceToAzureBlob",
            "description": "Copy from an HTTP source tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "HttpSourceDataInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "HttpSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information finns i [HTTP-anslutningen](data-factory-http-connector.md#copy-activity-properties) artikel.

## <a name="odata"></a>OData

### <a name="linked-service"></a>Länkad tjänst
toodefine en OData länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OData**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| URL: en |URL till hello OData-tjänst. |Ja |
| AuthenticationType |Typ av autentisering används tooconnect toohello OData-källan. <br/><br/> För molnet OData är möjliga värden anonym, grundläggande och OAuth (Observera Azure Data Factory för närvarande endast stöder Azure Active Directory-baserad OAuth). <br/><br/> För lokala OData är möjliga värden anonym, grundläggande och Windows. |Ja |
| användarnamn |Ange användarnamnet om du använder grundläggande autentisering. |Ja (endast om du använder grundläggande autentisering) |
| lösenord |Ange lösenord för hello-användarkonto som du angav för hello användarnamn. |Ja (endast om du använder grundläggande autentisering) |
| authorizedCredential |Om du använder OAuth, klickar du på **auktorisera** i hello guiden för Data Factory kopiera eller redigerare och ange dina autentiseringsuppgifter och sedan hello värdet på egenskapen kommer att genereras automatiskt. |Ja (endast om du använder OAuth-autentisering) |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala OData-tjänst. Ange endast om du vill kopiera data från lokala OData-källan. |Nej |

#### <a name="example---using-basic-authentication"></a>Exempel: använder grundläggande autentisering
```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

#### <a name="example---using-anonymous-authentication"></a>Exempel: använder anonym autentisering

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

#### <a name="example---using-windows-authentication-accessing-on-premises-odata-source"></a>Exempel – med hjälp av Windows-autentisering åtkomst till lokala OData-källan

```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of on-premises OData source, for example, Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example---using-oauth-authentication-accessing-cloud-odata-source"></a>Exempel – med åtkomst till molnet OData-källan OAuth-autentisering
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source, for example, https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking hello Authorize button on UI>"
        }
    }
}
```

Mer information finns i [OData connector](data-factory-odata-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Datauppsättning
toodefine en OData-datauppsättning set hello **typen** på hello datamängd för**ODataResource**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| Sökväg |Sökvägen toohello OData-resurs |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "ODataDataset",
    "properties": {
        "type": "ODataResource",
        "typeProperties": {
            "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
        }
    }
}
```

Mer information finns i [OData connector](data-factory-odata-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationella källa i en Kopieringsaktivitet
Om du kopierar data från en OData-källa, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:

| Egenskap | Beskrivning | Exempel | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |”? $select = namn, beskrivning och $top = 5” |Nej |

#### <a name="example"></a>Exempel

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "?$select=Name, Description&$top=5"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "ODataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobODataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "ODataToBlob"
        }],
        "start": "2017-02-01T18:00:00",
        "end": "2017-02-03T19:00:00"
    }
}
```

Mer information finns i [OData connector](data-factory-odata-connector.md#copy-activity-properties) artikel.


## <a name="odbc"></a>ODBC


### <a name="linked-service"></a>Länkad tjänst
toodefine ODBC länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesOdbc**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| connectionString |hello-access credential del av hello anslutningssträngen och en valfri krypteras autentiseringsuppgifter. Se exemplen i följande avsnitt hello. |Ja |
| autentiseringsuppgifter |hello access credential delen av hello anslutningssträngen som angetts i drivrutinsspecifika egenskapsvärdet format. Exempel ”: Uid =<user ID>; Pwd =<password>; RefreshToken =<secret refresh token>”;. |Nej |
| AuthenticationType |Typ av autentisering används tooconnect toohello ODBC-datalagret. Möjliga värden är: anonyma och grundläggande. |Ja |
| användarnamn |Ange användarnamnet om du använder grundläggande autentisering. |Nej |
| lösenord |Ange lösenord för hello-användarkonto som du angav för hello användarnamn. |Nej |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello ODBC-datalagret. |Ja |

#### <a name="example---using-basic-authentication"></a>Exempel: använder grundläggande autentisering

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```
#### <a name="example---using-basic-authentication-with-encrypted-credentials"></a>Exempel – med grundläggande autentisering och krypterade autentiseringsuppgifter
Du kan kryptera hello autentiseringsuppgifterna med hjälp av hello [ny AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (version 1.0 av Azure PowerShell) cmdlet eller [ny AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0,9 eller tidigare version av hello Azure PowerShell).  

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-anonymous-authentication"></a>Exempel: Använder anonym autentisering

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Mer information finns i [ODBC connector](data-factory-odbc-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine en ODBC-datauppsättning set hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i hello ODBC-datalagret. |Ja |


#### <a name="example"></a>Exempel

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "ODBCLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Mer information finns i [ODBC connector](data-factory-odbc-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationella källa i en Kopieringsaktivitet
Om du vill kopiera data från en ODBC-datalagret, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: `select * from MyTable`. |Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "OdbcDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobOdbcDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "OdbcToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
``` 

Mer information finns i [ODBC connector](data-factory-odbc-connector.md#copy-activity-properties) artikel.

## <a name="salesforce"></a>Salesforce


### <a name="linked-service"></a>Länkad tjänst
toodefine en Salesforce länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**Salesforce**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| environmentUrl | Ange hello URL Salesforce-instans. <br><br> – Standardvärdet är ”https://login.salesforce.com”. <br> -toocopy data från sandbox, ange ”https://test.salesforce.com”. <br> -toocopy data från domän, till exempel ange ”https://[domain].my.salesforce.com”. |Nej |
| användarnamn |Ange ett användarnamn för hello användarkonto. |Ja |
| lösenord |Ange ett lösenord för hello användarkonto. |Ja |
| securityToken |Ange en säkerhetstoken för hello användarkonto. Se [hämta säkerhetstoken](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) anvisningar för hur tooreset/hämta en säkerhetstoken. i allmänhet finns i toolearn om säkerhetstoken [säkerhet och hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```

Mer information finns i [Salesforce-anslutningsprogrammet](data-factory-salesforce-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine en Salesforce-dataset set hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i Salesforce. |Nej (om en **frågan** av **RelationalSource** har angetts) |

#### <a name="example"></a>Exempel

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Mer information finns i [Salesforce-anslutningsprogrammet](data-factory-salesforce-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationella källa i en Kopieringsaktivitet
Om du vill kopiera data från Salesforce, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |En SQL-92-fråga eller [Salesforce objektet Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) frågan. Till exempel: `select * from MyTable__c`. |Nej (om hello **tableName** av hello **dataset** har angetts) |

#### <a name="example"></a>Exempel  



```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "SalesforceInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

> [!IMPORTANT]
> Hej ”__c” tillhör hello API namn krävs för alla anpassade objekt.

Mer information finns i [Salesforce-anslutningsprogrammet](data-factory-salesforce-connector.md#copy-activity-properties) artikel. 

## <a name="web-data"></a>Web-Data 

### <a name="linked-service"></a>Länkad tjänst
toodefine en webbplats länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**Web**, och ange följande egenskaper i hello **typeProperties** avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| URL |URL: en toohello webbadress |Ja |
| AuthenticationType |Anonym. |Ja |
 

#### <a name="example"></a>Exempel


```json
{
    "name": "web",
    "properties": {
        "type": "Web",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "url": "https://en.wikipedia.org/wiki/"
        }
    }
}
```

Mer information finns i [Webbtabell connector](data-factory-web-table-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Datauppsättning
toodefine datauppsättningars Web set hello **typen** på hello datamängd för**WebTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt: 

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ |typ av hello dataset. måste anges för**WebTable** |Ja |
| Sökväg |En relativ URL toohello resurs som innehåller hello tabell. |Nej. Om sökvägen inte anges används endast hello-URL som anges i tjänstdefinitionen hello länkad. |
| Index |hello index för hello tabellen i hello resurs. Se [Get-index för en tabell i en HTML-sida](#get-index-of-a-table-in-an-html-page) avsnittet steg toogetting index för en tabell i en HTML-sida. |Ja |

#### <a name="example"></a>Exempel

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Mer information finns i [Webbtabell connector](data-factory-web-table-connector.md#dataset-properties) artikel. 

### <a name="web-source-in-copy-activity"></a>Webbadress i en Kopieringsaktivitet
Om du kopierar data från en webbserver-tabell, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**WebSource**. För närvarande när hello-källan i en Kopieringsaktivitet är av typen **WebSource**, inga ytterligare egenskaper som stöds.

#### <a name="example"></a>Exempel

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "WebTableToAzureBlob",
            "description": "Copy from a Web table tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "WebTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "WebSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Mer information finns i [Webbtabell connector](data-factory-web-table-connector.md#copy-activity-properties) artikel. 

## <a name="compute-environments"></a>COMPUTE-MILJÖER
hello visar följande tabell hello beräkning miljöer som stöds av Data Factory och hello omvandling av aktiviteter som kan köras på dem. Klicka på hello länk hello beräkning som du är intresserad av toosee hello JSON-scheman för länkad tjänst toolink den tooa data factory. 

| Compute-miljö | Aktiviteter |
| --- | --- |
| [HDInsight-kluster på begäran](#on-demand-azure-hdinsight-cluster) eller [egna HDInsight-kluster](#existing-azure-hdinsight-cluster) |[.NET anpassad aktivitet](#net-custom-activity), [Hive aktiviteten](#hdinsight-hive-activity), [svin aktivitet] (#hdinsight-pig-aktivitet, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop-strömning aktiviteten](#hdinsight-streaming-activityd), [Väck aktivitet](#hdinsight-spark-activity) |
| [Azure Batch](#azure-batch) |[.NET-anpassad aktivitet](#net-custom-activity) |
| [Azure Machine Learning](#azure-machine-learning) | [Maskininlärning Batchkörningsaktivitet](#machine-learning-batch-execution-activity), [Maskininlärning Uppdateringsresursaktivitet](#machine-learning-update-resource-activity) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics) |[Data Lake Analytics U-SQL](#data-lake-analytics-u-sql-activity) |
| [Azure SQL Database](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [SQLServer](#sql-server-1) |[Lagrad procedur](#stored-procedure-activity) |

## <a name="on-demand-azure-hdinsight-cluster"></a>Azure HDInsight-kluster på begäran
hello Azure Data Factory-tjänsten kan automatiskt skapa ett Windows/Linux-baserade på begäran HDInsight-kluster tooprocess data. hello klustret skapas i samma region som lagringskontot för hello (linkedServiceName-egenskapen i hello JSON) som är associerade med klustret hello hello. Du kan köra hello efter omvandling aktiviteter på den här länkade tjänsten: [.NET anpassad aktivitet](#net-custom-activity), [Hive aktiviteten](#hdinsight-hive-activity), [svin aktivitet] (#hdinsight-pig-aktivitet, [MapReduce activity ](#hdinsight-mapreduce-activity), [Hadoop-strömning aktiviteten](#hdinsight-streaming-activityd), [Väck aktiviteten](#hdinsight-spark-activity). 

### <a name="linked-service"></a>Länkad tjänst 
hello i den följande tabellen finns beskrivningar hello egenskaper som används i hello Azure JSON-definitionen för en länkad HDInsight på begäran-tjänst.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello typegenskapen ska anges för**HDInsightOnDemand**. |Ja |
| ClusterSize |Antalet worker/data noder i klustret hello. Hej HDInsight-kluster skapas med 2 huvudnoderna tillsammans med hello antalet arbetarnoder som du anger för den här egenskapen. hello noder har storlek Standard_D3 med 4 kärnor, så ett kluster med noder 4 worker tar 24 kärnor (4\*4 = 16 kärnor för arbetarnoder plus 2\*4 = 8 kärnor för huvudnoderna). Se [skapa Linux-baserade Hadoop-kluster i HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) för ytterligare information om hello Standard_D3 nivå. |Ja |
| TimeToLive |hello tillåten inaktivitetstid för hello på begäran HDInsight-kluster. Anger hur länge hello på begäran HDInsight-kluster förblir aktiv efter slutförande av en aktivitet som kör om det finns inga aktiva jobb i hello klustret.<br/><br/>Om en aktivitet som kör tar 6 minuter och timetolive är exempelvis ange too5 minuter hello kluster förblir alive för 5 minuter efter hello 6 minuter hello aktiviteten körs. Om en annan aktivitet kör körs med hello 6 minuter fönster, men det bearbetas av hello samma kluster.<br/><br/>Skapar ett HDInsight-kluster på begäran är en kostsam åtgärd (kan ta en stund), så Använd den här inställningen som behövs tooimprove prestanda för en datafabrik genom att återanvända ett HDInsight-kluster på begäran.<br/><br/>Om du ställer in timetolive värdet too0 tas hello klustret bort när hello aktivitet köras i bearbetade. På hello däremot om du anger ett högt värde hello klustret kan förblir inaktiva i onödan ledde höga kostnader. Det är därför viktigt att du ställer in hello lämpligt värde baserat på dina behov.<br/><br/>Flera pipelines kan dela hello samma instans av HDInsight-kluster för hello på begäran om hello timetolive egenskapens värde är korrekt |Ja |
| Version |Version av hello HDInsight-kluster. Mer information finns i [HDInsight-versioner som stöds i Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory). |Nej |
| linkedServiceName |Azure Storage länkade tjänsten toobe som används av hello på begäran klustret för lagring och bearbetning av data. <p>För närvarande kan du skapa ett HDInsight-kluster med på begäran som använder ett Azure Data Lake Store som hello lagring. Om du vill toostore hello Resultatdata från HDInsight som bearbetas i en Azure Data Lake Store kan använda en Kopieringsaktiviteten toocopy hello data från hello Azure Blob Storage toohello Azure Data Lake Store.</p>  | Ja |
| additionalLinkedServiceNames |Anger ytterligare lagringskonton för hello HDInsight länkade tjänsten så att hello Data Factory-tjänsten kan registrera dem å dina vägnar. |Nej |
| osType |Typ av operativsystem. Tillåtna värden är: (standard) för Windows och Linux |Nej |
| hcatalogLinkedServiceName |hello namnet på Azure SQL-länkade tjänsten punkt toohello HCatalog databasen. hello på begäran HDInsight-kluster skapas med hjälp av hello Azure SQL-databas som hello metastore. |Nej |

### <a name="json-example"></a>JSON-exempel
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
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

Mer information finns i [Compute länkade tjänster](data-factory-compute-linked-services.md) artikel. 

## <a name="existing-azure-hdinsight-cluster"></a>Befintligt Azure HDInsight-kluster
Du kan skapa ett Azure HDInsight länkade tjänsten tooregister ditt eget kluster i HDInsight med Data Factory. Du kan köra hello följa data transformation aktiviteter på den här länkade tjänsten: [.NET anpassad aktivitet](#net-custom-activity), [Hive aktiviteten](#hdinsight-hive-activity), [svin aktivitet] (#hdinsight-pig-aktivitet, [MapReduce aktiviteten](#hdinsight-mapreduce-activity), [Hadoop-strömning aktiviteten](#hdinsight-streaming-activityd), [Väck aktiviteten](#hdinsight-spark-activity). 

### <a name="linked-service"></a>Länkad tjänst
hello i den följande tabellen finns beskrivningar hello egenskaper som används i hello Azure JSON-definitionen för en Azure HDInsight länkad tjänst.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello typegenskapen ska anges för**HDInsight**. |Ja |
| clusterUri |hello hello HDInsight-kluster-URI. |Ja |
| användarnamn |Ange hello namnet hello användaren toobe används tooconnect tooan befintligt HDInsight-kluster. |Ja |
| lösenord |Ange lösenordet för användarkontot för hello. |Ja |
| linkedServiceName | Namnet på hello länkad Azure Storage-tjänst som refererar toohello Azure blob-lagring som används av hello HDInsight-kluster. <p>För närvarande kan du ange ett Azure Data Lake Store länkade tjänsten för den här egenskapen. Du kan komma åt data i hello Azure Data Lake Store från Hive/Pig-skript om hello HDInsight-kluster har åtkomst toohello Data Lake Store. </p>  |Ja |

Versioner av HDInsight-kluster som stöds finns [HDInsight-versioner som stöds](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory). 

#### <a name="json-example"></a>JSON-exempel

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

## <a name="azure-batch"></a>Azure Batch
Du kan skapa ett Azure Batch länkade tjänsten tooregister Batch-pool för virtuella datorer (VM) med en data factory. Du kan köra .NET anpassade aktiviteter med hjälp av Azure Batch eller Azure HDInsight. Du kan köra en [.NET anpassad aktivitet](#net-custom-activity) på den här länkade tjänsten. 

### <a name="linked-service"></a>Länkad tjänst
hello i den följande tabellen finns beskrivningar hello egenskaper som används i hello Azure JSON-definitionen för en Azure Batch länkad tjänst.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello typegenskapen ska anges för**AzureBatch**. |Ja |
| Kontonamn |Namnet på hello Azure Batch-kontot. |Ja |
| accessKey |Åtkomstnyckeln för hello Azure Batch-kontot. |Ja |
| Poolnamn |Namnet på hello pool för virtuella datorer. |Ja |
| linkedServiceName |Namnet på hello länkad Azure Storage-tjänst som är associerad med den här Azure Batch länkade tjänsten. Den här länkade tjänsten används för mellanlagring av filer krävs toorun hello aktivitet och lagra hello aktivitetsloggar för körning. |Ja |


#### <a name="json-example"></a>JSON-exempel

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

## <a name="azure-machine-learning"></a>Azure Machine Learning
Skapa en Azure Machine Learning länkade tjänsten tooregister en Machine Learning-batch bedömningsslutpunkten med en data factory. Två data transformation aktiviteter som kan köras på den här länkade tjänsten: [Machine Learning-Batchkörningsaktivitet](#machine-learning-batch-execution-activity), [Machine Learning-Uppdateringsresursaktivitet](#machine-learning-update-resource-activity). 

### <a name="linked-service"></a>Länkad tjänst
hello i den följande tabellen finns beskrivningar hello egenskaper som används i hello Azure JSON-definitionen för en Azure Machine Learning länkad tjänst.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| Typ |hello typegenskapen ska anges till: **AzureML**. |Ja |
| mlEndpoint |Hej batchbedömningsjobbet URL. |Ja |
| apiKey |hello publicerade arbetsytemodellens API. |Ja |

#### <a name="json-example"></a>JSON-exempel

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

## <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics
Du skapar en **Azure Data Lake Analytics** länkade tjänsten toolink ett Azure Data Lake Analytics beräkning service tooan Azure data factory innan du använder hello [Data Lake Analytics U-SQL-aktivitet](data-factory-usql-activity.md) i en pipeline .

### <a name="linked-service"></a>Länkad tjänst

hello i den följande tabellen finns beskrivningar hello egenskaper som används i hello JSON-definitionen för en länkad Azure Data Lake Analytics-tjänst. 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| Typ |hello typegenskapen ska anges till: **AzureDataLakeAnalytics**. |Ja |
| Kontonamn |Azure Data Lake Analytics-kontonamn. |Ja |
| dataLakeAnalyticsUri |Azure Data Lake Analytics-URI. |Nej |
| Auktorisering |Auktoriseringskoden hämtas automatiskt när du klickar på **auktorisera** knappen i hello Data Factory-redigeraren och slutför hello OAuth-inloggningen. |Ja |
| subscriptionId |Azure prenumerations-id |Nej (om inte anges prenumeration hello datafabriken används). |
| resourceGroupName |Azure resursgruppens namn |Nej (om inte anges resursgruppen av hello datafabriken används). |
| Sessions-ID |sessions-id från hello OAuth-auktorisering session. Varje sessions-id är unikt och får endast användas en gång. När du använder hello Data Factory-redigeraren genereras detta ID automatiskt. |Ja |


#### <a name="json-example"></a>JSON-exempel
hello följande exempel innehåller JSON-definitionen för en länkad Azure Data Lake Analytics-tjänst.

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "<account name>",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>",
            "subscriptionId": "<subscription id>",
            "resourceGroupName": "<resource group name>"
        }
    }
}
```

## <a name="azure-sql-database"></a>Azure SQL Database
Du skapar en Azure SQL-länkade tjänst och använda den med hello [lagrade Proceduraktiviteten](#stored-procedure-activity) tooinvoke en lagrad procedur från Data Factory-pipelinen. 

### <a name="linked-service"></a>Länkad tjänst
toodefine en Azure SQL Database länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureSqlDatabase**, och ange följande egenskaper i hello **typeProperties**avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| connectionString |Ange nödvändig information tooconnect toohello Azure SQL Database-instans för hello-egenskapen connectionString. |Ja |

#### <a name="json-example"></a>JSON-exempel

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Se [Azure SQL Connector](data-factory-azure-sql-connector.md#linked-service-properties) artikeln för information om den här länkade tjänsten.

## <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Du skapar en länkad Azure SQL Data Warehouse-tjänst och använda den med hello [lagrade Proceduraktiviteten](data-factory-stored-proc-activity.md) tooinvoke en lagrad procedur från Data Factory-pipelinen. 

### <a name="linked-service"></a>Länkad tjänst
toodefine ett Azure SQL Data Warehouse länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureSqlDW**, och ange följande egenskaper i hello **typeProperties**avsnitt:  

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| connectionString |Ange nödvändig information tooconnect toohello Azure SQL Data Warehouse-instans för hello-egenskapen connectionString. |Ja |

#### <a name="json-example"></a>JSON-exempel

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Mer information finns i [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) artikel. 

## <a name="sql-server"></a>SQL Server 
Du skapar en SQL Server som är länkad tjänst och använda den med hello [lagrade Proceduraktiviteten](data-factory-stored-proc-activity.md) tooinvoke en lagrad procedur från Data Factory-pipelinen. 

### <a name="linked-service"></a>Länkad tjänst
Du skapar en länkad tjänst av typen **OnPremisesSqlServer** toolink fabriken för tooa en lokal SQL Server-databasen. hello följande tabell ger en beskrivning för JSON-element specifika tooon lokala SQL Server som är länkade tjänsten.

hello följande tabell ger en beskrivning för JSON-element specifika tooSQL länkad Server-tjänsten.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello typegenskapen ska anges till: **OnPremisesSqlServer**. |Ja |
| connectionString |Ange connectionString information som behövs för tooconnect toohello lokala SQL Server-databas med SQL-autentisering eller Windows-autentisering. |Ja |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala SQL Server-databas. |Ja |
| användarnamn |Ange användarnamnet om du använder Windows-autentisering. Exempel: **domainname\\användarnamn**. |Nej |
| lösenord |Ange lösenord för hello-användarkonto som du angav för hello användarnamn. |Nej |

Du kan kryptera autentiseringsuppgifterna med hjälp av hello **ny AzureRmDataFactoryEncryptValue** cmdlet och använda dem i hello anslutningssträngen som visas i följande exempel hello (**EncryptedCredential** egenskapen):  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a>Exempel: JSON för att använda SQL-autentisering

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a>Exempel: JSON för att använda Windows-autentisering

Om användarnamn och lösenord anges gateway använder dem tooimpersonate hello användaren konto tooconnect toohello lokala SQL Server-databas. Gatewayen ansluter annars toohello SQL Server direkt med hello säkerhetskontexten för Gateway (dess start-konto).

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Mer information finns i [SQL Server-anslutningen](data-factory-sqlserver-connector.md#linked-service-properties) artikel.

## <a name="data-transformation-activities"></a>DATA TRANSFORMATION AKTIVITETER

Aktivitet | Beskrivning
-------- | -----------
[HDInsight Hive-aktivitet](#hdinsight-hive-activity) | Hej HDInsight Hive aktivitet i en Data Factory-pipelinen kör Hive-frågor på egen hand eller på begäran Windows/Linux-baserade HDInsight-kluster. 
[HDInsight Pig-aktivitet](#hdinsight-pig-activity) | Hej HDInsight Pig aktivitet i en Data Factory-pipelinen kör Pig frågor på egen hand eller på begäran Windows/Linux-baserade HDInsight-kluster.
[HDInsight MapReduce-aktivitet](#hdinsight-mapreduce-activity) | Hej HDInsight MapReduce aktivitet i en Data Factory-pipelinen kör MapReduce program på egen hand eller på begäran Windows/Linux-baserade HDInsight-kluster.
[HDInsight-strömningsaktivitet](#hdinsight-streaming-activity) | Hej HDInsight Streaming Activity i Data Factory-pipelinen kör Hadoop Streaming program på egen hand eller på begäran Windows/Linux-baserade HDInsight-kluster.
[HDInsight Spark-aktivitet](#hdinsight-spark-activity) | hello HDInsight Spark-aktivitet i en Data Factory-pipelinen körs Spark-program på din egen HDInsight-kluster. 
[Machine Learning Batch-körningsaktivitet](#machine-learning-batch-execution-activity) | Azure Data Factory aktiverar du tooeasily skapa pipelines som använder en publicerad Azure Machine Learning-webbtjänsten för förutsägelseanalys. Du kan anropa en Machine Learning web service toomake förutsägelser på hello data i batch med hello-Batchkörningsaktivitet i ett Azure Data Factory-pipelinen. 
[Machine Learning-uppdateringsresursaktivitet](#machine-learning-update-resource-activity) | Över tiden ange hello förutsägelsemodeller i hello Machine Learning bedömningsprofil experiment måste toobe retrained med nya datauppsättningar. När du är klar med omtränings vill du tooupdate hello bedömningen webbtjänst med hello retrained Machine Learning-modellen. Du kan använda hello Uppdateringsresursaktivitet tooupdate hello-webbtjänsten med hello nyligen tränats modell.
[Lagrad proceduraktivitet](#stored-procedure-activity) | Du kan använda hello lagrade proceduren aktivitet i en Data Factory-pipelinen tooinvoke en lagrad procedur i någon av följande datalager hello: Azure SQL Database, Azure SQL Data Warehouse, SQL Server-databas i ditt företag eller en Azure VM. 
[Data Lake Analytics U-SQL-aktivitet](#data-lake-analytics-u-sql-activity) | Data Lake Analytics U-SQL-aktivitet körs ett U-SQL-skript i ett Azure Data Lake Analytics-kluster.  
[.NET-anpassad aktivitet](#net-custom-activity) | Om du behöver tootransform data på ett sätt som inte stöds av Data Factory kan du skapa en anpassad aktivitet med din egen databearbetning logik och använder hello aktivitet i hello pipeline. Du kan konfigurera hello anpassade .NET aktiviteten toorun med hjälp av Azure Batch-tjänsten eller ett Azure HDInsight-kluster. 

     
## <a name="hdinsight-hive-activity"></a>HDInsight Hive-aktivitet
Du kan ange hello följande egenskaper i en definition av Hive aktivitets-JSON. hello typegenskapen för hello aktiviteten måste vara: **HDInsightHive**. Du måste först skapa en länkad HDInsight-tjänst och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen. hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooHDInsightHive:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| Skriptet |Ange hello Hive-skript infogade |Nej |
| sökvägen för skriptet |Store hello registreringsdatafilen i ett Azure blob storage och ange hello sökväg toohello fil. Använd egenskapen 'script' eller 'scriptPath'. Båda kan inte användas tillsammans. hello-filnamnet är skiftlägeskänsliga. |Nej |
| definierar |Ange parametrar som nyckel/värde-par för refererar till i hello Hive-skript med hjälp av 'hiveconf' |Nej |

Dessa egenskaper finns specifika toohello Hive aktivitet. Andra egenskaper (utanför hello typeProperties avsnittet) stöds för alla aktiviteter.   

### <a name="json-example"></a>JSON-exempel
hello följande JSON definierar en HDInsight Hive-aktivitet i en pipeline.  

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "inputs": [
      {
        "name": "input tables"
      }
    ],
    "outputs": [
      {
        "name": "output tables"
      }
    ],
    "linkedServiceName": "MyHDInsightLinkedService",
    "typeProperties": {
      "script": "Hive script",
      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
      "defines": {
        "param1": "param1Value"
      }
    },
   "scheduler": {
      "frequency": "Day",
      "interval": 1
    }
}
```

Mer information finns i [Hive aktiviteten](data-factory-hive-activity.md) artikel. 

## <a name="hdinsight-pig-activity"></a>HDInsight-piggningsåtgärd
Du kan ange följande egenskaper i en definition av Pig aktivitet JSON hello. hello typegenskapen för hello aktiviteten måste vara: **HDInsightPig**. Du måste först skapa en länkad HDInsight-tjänst och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen. hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooHDInsightPig: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| Skriptet |Ange hello Pig-skriptet infogade |Nej |
| sökvägen för skriptet |Lagra hello Pig-skriptet i en Azure blob storage och ange hello sökväg toohello fil. Använd egenskapen 'script' eller 'scriptPath'. Båda kan inte användas tillsammans. hello-filnamnet är skiftlägeskänsliga. |Nej |
| definierar |Ange parametrar som nyckel/värde-par för refererar till i hello Pig-skriptet |Nej |

Dessa egenskaper finns specifika toohello Pig aktivitet. Andra egenskaper (utanför hello typeProperties avsnittet) stöds för alla aktiviteter.   

### <a name="json-example"></a>JSON-exempel

```json
{
    "name": "HiveActivitySamplePipeline",
      "properties": {
    "activities": [
        {
            "name": "Pig Activity",
            "description": "description",
            "type": "HDInsightPig",
            "inputs": [
                  {
                    "name": "input tables"
                  }
            ],
            "outputs": [
                  {
                    "name": "output tables"
                  }
            ],
            "linkedServiceName": "MyHDInsightLinkedService",
            "typeProperties": {
                  "script": "Pig script",
                  "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                  "defines": {
                    "param1": "param1Value"
                  }
            },
               "scheduler": {
                  "frequency": "Day",
                  "interval": 1
            }
          }
    ]
  }
}
```

Mer information finns i [Pig aktiviteten](#data-factory-pig-activity.md) artikel. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce-aktivitet
Du kan ange följande egenskaper i en definition av MapReduce aktivitet JSON hello. hello typegenskapen för hello aktiviteten måste vara: **HDInsightMapReduce**. Du måste först skapa en länkad HDInsight-tjänst och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen. hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooHDInsightMapReduce: 

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| jarLinkedService | Namnet på hello länkad tjänst för hello Azure Storage som innehåller hello JAR-filen. | Ja |
| jarFilePath | Sökvägen toohello JAR-filen i hello Azure Storage. | Ja | 
| Klassnamn | Namnet på hello huvudsakliga klassen i hello JAR-filen. | Ja | 
| Argument | En lista över kommaavgränsade argument för hello MapReduce program. Vid körning kan du se några extra argument (till exempel: mapreduce.job.tags) från hello MapReduce-ramverket. toodifferentiate din argument med hello MapReduce argument, Överväg att använda både alternativet och värde som argument som visas i följande exempel hello (- s,--indata--utdata osv., är en alternativ direkt följt av deras värden) | Nej | 

### <a name="json-example"></a>JSON-exempel

```json
{
    "name": "MahoutMapReduceSamplePipeline",
    "properties": {
        "description": "Sample Pipeline tooRun a Mahout Custom Map Reduce Jar. This job calculates an Item Similarity Matrix toodetermine hello similarity between two items",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                    "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": ["-s", "SIMILARITY_LOGLIKELIHOOD", "--input", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input", "--output", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/", "--maxSimilaritiesPerItem", "500", "--tempDir", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"]
                },
                "inputs": [
                    {
                        "name": "MahoutInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "MahoutOutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "MahoutActivity",
                "description": "Custom Map Reduce toogenerate Mahout result",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00",
        "end": "2017-01-04T00:00:00"
    }
}
```

Mer information finns i [MapReduce Activity](data-factory-map-reduce.md) artikel. 

## <a name="hdinsight-streaming-activity"></a>HDInsight-strömningsaktivitet
Du kan ange följande egenskaper i en definition av Hadoop Streaming aktivitet JSON hello. hello typegenskapen för hello aktiviteten måste vara: **HDInsightStreaming**. Du måste först skapa en länkad HDInsight-tjänst och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen. hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooHDInsightStreaming: 

| Egenskap | Beskrivning | 
| --- | --- |
| Mapper | Namnet på hello mapper körbara. I exemplet hello är cat.exe hello mapper körbara.| 
| Reducer | Namnet på hello reducer körbara. I exemplet hello är wc.exe hello reducer körbara. | 
| Indata | Indatafilen (inklusive platsen) för hello mapper. I exemplet hello ”: wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt”: adfsample är hello blob-behållare, exempel/data/Gutenberg är hello mapp och davinci.txt är hello-blob. |
| Utdata | Utdatafilen (inklusive platsen) för hello reducer. hello utdata från hello Hadoop Streaming job skrivs toohello plats som anges för den här egenskapen. |
| filePaths | Sökvägar för hello mapper och reducer körbara filer. I exemplet hello: ”adfsample/example/apps/wc.exe” adfsample är hello blob-behållare, exempel/appar är hello mapp och wc.exe är hello körbara. | 
| fileLinkedService | Azure Storage länkade tjänst som representerar hello Azure-lagring som innehåller hello-filer som anges i hello filePaths avsnitt. | 
| Argument | En lista över kommaavgränsade argument för hello MapReduce program. Vid körning kan du se några extra argument (till exempel: mapreduce.job.tags) från hello MapReduce-ramverket. toodifferentiate din argument med hello MapReduce argument, Överväg att använda både alternativet och värde som argument som visas i följande exempel hello (- s,--indata--utdata osv., är en alternativ direkt följt av deras värden) | 
| getDebugInfo | Ett valfritt element. När den är inställd tooFailure laddas hello loggar endast vid fel. När den är inställd tooAll laddas alltid loggar oavsett hello Körstatus. | 

> [!NOTE]
> Du måste ange en utdatauppsättningen för hello Hadoop Streaming Activity för hello **matar ut** egenskapen. Den här datauppsättningen kan bara en dummy datamängd som är nödvändiga toodrive hello pipeline schema (varje timme, varje dag, osv.). Om hello aktiviteten inte tar indata, du kan hoppa över att ange en inkommande datauppsättning för hello aktivitet för hello **indata** egenskapen.  

## <a name="json-example"></a>JSON-exempel

```json
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": ["<nameofthecluster>/example/apps/wc.exe","<nameofthecluster>/example/apps/cat.exe"],
                    "fileLinkedService": "StorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00",
        "end": "2014-01-05T00:00:00"
    }
}
```

Mer information finns i [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md) artikel. 

## <a name="hdinsight-spark-activity"></a>HDInsight Apache Spark-aktivitet
Du kan ange följande egenskaper i en definition av Spark aktivitet JSON hello. hello typegenskapen för hello aktiviteten måste vara: **HDInsightSpark**. Du måste först skapa en länkad HDInsight-tjänst och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen. hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooHDInsightSpark: 

| Egenskap | Beskrivning | Krävs |
| -------- | ----------- | -------- |
| rootPath | hello Azure Blob-behållaren och mappen som innehåller hello Spark-fil. hello-filnamnet är skiftlägeskänsliga. | Ja |
| entryFilePath | Relativ sökväg toohello rotmapp hello Spark kodpaketet. | Ja |
| Klassnamn | Programmets Java/Spark huvudsakliga klass | Nej | 
| Argument | En lista med kommandoradsargument toohello Spark-program. | Nej | 
| proxyUser | hello konto tooimpersonate tooexecute hello Spark användarprogram | Nej | 
| sparkConfig | Spark konfigurationsegenskaper. | Nej | 
| getDebugInfo | Anger när hello Spark loggfilerna kopierade toohello Azure storage som används av HDInsight-kluster (eller) anges av sparkJobLinkedService. Tillåtna värden: None, alltid eller fel. Standardvärde: Ingen. | Nej | 
| sparkJobLinkedService | hello länkad Azure Storage-tjänst som äger hello Spark fil, beroenden och loggar.  Om du inte anger ett värde för den här egenskapen används hello lagring som är associerade med HDInsight-kluster. | Nej |

### <a name="json-example"></a>JSON-exempel

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-05T00:00:00",
        "end": "2017-02-06T00:00:00"
    }
}
```
Observera följande punkter hello: 

- Hej **typen** egenskapen för**HDInsightSpark**.
- Hej **rootPath** har angetts för**adfspark\\pyFiles** där adfspark är hello Azure Blob-behållare och pyFiles är bra mapp i behållaren. I det här exemplet är hello Azure Blob Storage hello en som är associerad med hello Spark-kluster. Du kan ladda upp hello filen tooa olika Azure Storage. Om du gör det, skapa en länkad Azure Storage service toolink storage-konto toohello data factory. Ange hello namnet på hello länkade tjänst som ett värde för hello **sparkJobLinkedService** egenskapen. Se [Spark Aktivitetsegenskaper](#spark-activity-properties) för ytterligare information om den här egenskapen och andra egenskaper som stöds av hello Spark-aktivitet.
- Hej **entryFilePath** anges toohello **test.py**, vilket är hello python-fil. 
- Hej **getDebugInfo** egenskapen för**alltid**, vilket innebär att hello loggfiler är alltid genereras (lyckade eller misslyckade).  

    > [!IMPORTANT]
    > Vi rekommenderar att du inte anger den här egenskapen tooAlways i en produktionsmiljö om du felsöker ett problem. 
- Hej **matar ut** avsnittet innehåller en datamängd för utdata. Du måste ange en datamängd för utdata, även om hello spark-program inte producerar några utdata. hello utdata dataset enheter hello schema för hello pipelinen (varje timme, varje dag, osv.).

Mer information om hello aktivitet finns [Spark aktiviteten](data-factory-spark.md) artikel.  

## <a name="machine-learning-batch-execution-activity"></a>Machine Learning Batch-körningsaktivitet
Du kan ange följande egenskaper i en definition av Azure ML Batch Execution aktivitet JSON hello. hello typegenskapen för hello aktiviteten måste vara: **AzureMLBatchExecution**. Du måste skapa en Azure Machine Learning länkade tjänsten först och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen. hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooAzureMLBatchExecution:

Egenskap | Beskrivning | Krävs 
-------- | ----------- | --------
webServiceInput | hello dataset toobe angavs som indata för hello Azure ML-webbtjänsten. Den här datauppsättningen måste också tas med i hello indata för hello aktiviteten. |Använda webServiceInput eller webServiceInputs. | 
webServiceInputs | Ange datauppsättningar toobe skickas som indata för hello Azure ML-webbtjänsten. Om hello webbtjänst tar flera indata kan använda hello webServiceInputs egenskapen i stället för att använda hello webServiceInput-egenskapen. Datauppsättningar som refereras av hello **webServiceInputs** måste också tas med i hello aktiviteten **indata**. | Använda webServiceInput eller webServiceInputs. | 
webServiceOutputs | hello datauppsättningar som är tilldelad som utdata för hello Azure ML web service. hello webbtjänst returnerar utdata i denna dataset. | Ja | 
globalParameters | Ange värden för hello webbtjänstparametrar i det här avsnittet. | Nej | 

### <a name="json-example"></a>JSON-exempel
I det här exemplet hello aktiviteten har hello dataset **MLSqlInput** som indata och **MLSqlOutput** som hello utdata. Hej **MLSqlInput** skickas som ett inkommande toohello webbtjänsten genom att använda hello **webServiceInput** JSON-egenskapen. Hej **MLSqlOutput** skickas som en webbtjänst för utdata toohello genom att använda hello **webServiceOutputs** JSON-egenskapen. 

```json
{
   "name": "MLWithSqlReaderSqlWriter",
   "properties": {
      "description": "Azure ML model with sql azure reader/writer",
      "activities": [{
         "name": "MLSqlReaderSqlWriterActivity",
         "type": "AzureMLBatchExecution",
         "description": "test",
         "inputs": [ { "name": "MLSqlInput" }],
         "outputs": [ { "name": "MLSqlOutput" } ],
         "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
         "typeProperties":
         {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
               "output1": "MLSqlOutput"
            },
            "globalParameters": {
               "Database server name": "<myserver>.database.windows.net",
               "Database name": "<database>",
               "Server user account name": "<user name>",
               "Server user account password": "<password>"
            }              
         },
         "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
         }
      }],
      "start": "2016-02-13T00:00:00",
       "end": "2016-02-14T00:00:00"
   }
}
```

Hello JSON-exempelvis hello distribuerade tjänsten använder en läsare och skrivare modulen tooread/skriva data från Azure Machine Learning Web / tooan Azure SQL Database. Den här webbtjänsten visar hello följande fyra parametrar: databasen servernamnet, databasnamnet, Server-användarkonto och serverlösenord.

> [!NOTE]
> Endast indata och utdata för hello AzureMLBatchExecution aktivitet kan skickas som parametrar toohello webbtjänsten. Hello ovan JSON fragment är exempelvis MLSqlInput en inkommande toohello AzureMLBatchExecution aktivitet som skickas som ett inkommande toohello webbtjänsten via webServiceInput-parametern.

## <a name="machine-learning-update-resource-activity"></a>Machine Learning-uppdateringsresursaktivitet
Du kan ange följande egenskaper i en definition av Azure ML Update resurs aktiviteten JSON hello. hello typegenskapen för hello aktiviteten måste vara: **AzureMLUpdateResource**. Du måste skapa en Azure Machine Learning länkade tjänsten först och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen. hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooAzureMLUpdateResource:

Egenskap | Beskrivning | Krävs 
-------- | ----------- | --------
trainedModelName | Namnet på hello retrained modell. | Ja |  
trainedModelDatasetName | DataSet pekar toohello iLearner-fil som returneras av hello omtränings igen. | Ja | 

### <a name="json-example"></a>JSON-exempel
hello pipeline har två aktiviteter: **AzureMLBatchExecution** och **AzureMLUpdateResource**. hello Azure ML-batchkörning aktiviteten tar hello utbildningsdata som indata och genererar en iLearner-fil som utdata. hello-aktivitet anropar hello utbildning webbtjänst (träningsexperiment visas som en webbtjänst) med hello indata utbildning data och tar emot hello ilearner-fil från hello-webbtjänsten. Hej placeholderBlob är bara en dummy datamängd för utdata som krävs av hello Azure Data Factory-tjänsten toorun hello pipeline.


```json
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "trained model",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [{ "name": "trainedModelBlob" }],
                "outputs": [{ "name": "placeholderBlob" }],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00",
        "end": "2016-02-14T00:00:00"
    }
}
```

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL-aktivitet
Du kan ange följande egenskaper i en definition av U-SQL-aktivitet JSON hello. hello typegenskapen för hello aktiviteten måste vara: **DataLakeAnalyticsU SQL**. Du måste skapa en länkad Azure Data Lake Analytics-tjänst och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen. hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooDataLakeAnalyticsU-SQL: 

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| ScriptPath |Sökvägen toofolder som innehåller hello U-SQL-skriptet. Namnet på hello-filen är skiftlägeskänslig. |Nej (om du använder skriptet) |
| scriptLinkedService |Länkade tjänst som länkar hello lagring som innehåller hello skriptet toohello data factory |Nej (om du använder skriptet) |
| Skriptet |Ange infogat skript i stället för att ange scriptPath och scriptLinkedService. Till exempel: ”skript”: ”skapa databastest”. |Nej (om du använder scriptPath och scriptLinkedService) |
| degreeOfParallelism |hello maximalt antal noder används samtidigt toorun hello jobb. |Nej |
| Prioritet |Anger vilka jobb av alla köas ska vara markerade toorun först. hello lägre hello nummer, hello högre hello prioritet. |Nej |
| parameters |Parametrar för hello U-SQL-skript |Nej |

### <a name="json-example"></a>JSON-exempel

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This pipeline computes events for en-gb locale and date less than Feb 19, 2012.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00",
        "end": "2015-08-08T01:00:00",
        "isPaused": false
    }
}
```

Mer information finns i [Data Lake Analytics U-SQL-aktivitet](data-factory-usql-activity.md). 

## <a name="stored-procedure-activity"></a>Lagrad proceduraktivitet
Du kan ange hello följande egenskaper i en lagrad procedur aktivitet JSON-definition. hello typegenskapen för hello aktiviteten måste vara: **SqlServerStoredProcedure**. Du måste skapa en något av följande länkade tjänster hello och ange hello namnet på hello länkade tjänst som värde för hello **linkedServiceName** egenskapen:

- SQL Server 
- Azure SQL Database
- Azure SQL Data Warehouse

hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooSqlServerStoredProcedure:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| storedProcedureName |Ange hello namnet på hello lagrade procedur i hello Azure SQL database eller Azure SQL Data Warehouse som representeras av hello länkade tjänst som hello utdata tabellen använder. |Ja |
| storedProcedureParameters |Ange värden för parametrarna för lagrade procedurer. Om du behöver toopass null för en parameter, Använd hello syntax: ”param1”: null (alla gemen). Se följande exempel toolearn om hur du använder den här egenskapen hello. |Nej |

Om du anger en inkommande datauppsättning, måste det vara tillgänglig (statusen ”klar”) för hello lagrade proceduren aktiviteten toorun. hello inkommande dataset kan inte användas i hello lagrade procedur som en parameter. Det är bara använda toocheck hello beroende innan start hello lagrade proceduraktiviteten. Du måste ange en datamängd för utdata för en lagrad procedur-aktivitet. 

Utdatauppsättningen anger hello **schema** för hello lagrade proceduraktiviteten (varje timme, varje vecka, månad, etc.). Hej utdatauppsättningen måste använda en **länkade tjänsten** som refererar tooan Azure SQL Database eller ett Azure SQL Data Warehouse eller en SQL Server-databas som du vill hello lagrade proceduren toorun. Hej utdatauppsättningen kan fungera som ett sätt toopass hello resultat hello lagrade proceduren för senare bearbetning av en annan aktivitet ([länkning aktiviteter](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) i hello pipeline. Data Factory kan dock inte automatiskt skriva hello utdata från en lagrad procedur toothis dataset. Det är hello lagrad procedur som skrivningar tooa SQL-tabell som hello utdata dataset pekar på. I vissa fall hello utdatauppsättningen kan vara en **dummy dataset**, som används för endast toospecify hello schema för att köra hello lagrade proceduraktiviteten.  

### <a name="json-example"></a>JSON-exempel

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [{ "name": "sprocsampleout" }],
                "name": "SprocActivitySample"
            }
        ],
         "start": "2016-08-02T00:00:00",
         "end": "2016-08-02T05:00:00",
        "isPaused": false
    }
}
```

Mer information finns i [lagrade Proceduraktiviteten](data-factory-stored-proc-activity.md) artikel. 

## <a name="net-custom-activity"></a>.NET-anpassad aktivitet
Du kan ange hello följande egenskaper i en anpassad aktivitet för .NET JSON-definitionen. hello typegenskapen för hello aktiviteten måste vara: **DotNetActivity**. Du måste skapa en Azure HDInsight länkad tjänst eller ett Azure Batch länkade tjänsten och ange hello namnet på hello länkade tjänst som värde för hello **linkedServiceName** egenskapen. hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooDotNetActivity:
 
| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| AssemblyName | Namnet på hello sammansättning. I hello exempelvis är: **MyDotnetActivity.dll**. | Ja |
| EntryPoint |Namnet på hello-klass som implementerar hello IDotNetActivity gränssnitt. I hello exempelvis är: **MyDotNetActivityNS.MyDotNetActivity** där MyDotNetActivityNS är hello namnområde och MyDotNetActivity är hello-klass.  | Ja | 
| PackageLinkedService | Namnet på hello länkad Azure Storage-tjänst som pekar toohello blob-lagring som innehåller hello anpassad aktivitet zip-filen. I hello exempelvis är: **AzureStorageLinkedService**.| Ja |
| PackageFile | Namnet på hello zip-filen. I hello exempelvis är: **customactivitycontainer/MyDotNetActivity.zip**. | Ja |
| extendedProperties | Utökade egenskaper som du kan definiera och vidarebefordra toohello .NET-kod. I det här exemplet hello **SliceStart** variabeln anges tooa värde baserat på hello SliceStart systemvariabeln. | Nej | 

### <a name="json-example"></a>JSON-exempel

```json
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "AzureBatchLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00",
    "end": "2016-11-16T05:00:00",
    "isPaused": false
  }
}
```

Detaljerad information finns i [använda anpassade aktiviteter i Data Factory](data-factory-use-custom-activities.md) artikel. 

## <a name="next-steps"></a>Nästa steg
Se hello följande kurser: 

- [Självstudier: skapa en pipeline med en kopia-aktivitet](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Självstudier: skapa en pipeline med en hive-aktivitet](data-factory-build-your-first-pipeline-using-editor.md)