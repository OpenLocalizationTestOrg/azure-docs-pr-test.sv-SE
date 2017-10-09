---
title: "aaaCreate datauppsättningar i Azure Data Factory | Microsoft Docs"
description: "Lär dig hur toocreate datauppsättningar i Azure Data Factory med exempel på egenskaper som förskjutning och anchorDateTime."
keywords: Skapa dataset, dataset exempel, offset exempel
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 0614cd24-2ff0-49d3-9301-06052fd4f92a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: shlo
ms.openlocfilehash: 181859ed250595d756df73e9ebcac08d9e7184c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="datasets-in-azure-data-factory"></a>Datauppsättningar i Azure Data Factory
Den här artikeln beskriver vilka datauppsättningar är hur de har definierats i JSON-format och hur de används i Azure Data Factory rörledningar. Den innehåller information om varje avsnitt (till exempel struktur, tillgänglighet och princip) i hello dataset JSON-definitionen. hello artikeln innehåller även exempel för att använda hello **offset**, **anchorDateTime**, och **style** egenskaper i en dataset JSON-definition.

> [!NOTE]
> Om du är ny tooData Factory finns [introduktion tooAzure Data Factory](data-factory-introduction.md) en översikt. Om du inte har praktisk erfarenhet av att skapa datafabriker kan du få en bättre förståelse av läsning hello [data transformation kursen](data-factory-build-your-first-pipeline.md) och hello [data movement kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="overview"></a>Översikt
En datafabrik kan ha en eller flera pipelines. En **pipeline** är en logisk gruppering av **aktiviteter** som tillsammans utföra en uppgift. hello aktiviteter i en pipeline definiera åtgärder tooperform på dina data. Du kan till exempel använda en aktivitet toocopy data från en lokal SQL Server-tooAzure Blob storage. Du kan sedan använda en Hive-aktivitet som kör en Hive-skript på en Azure HDInsight-kluster tooprocess data från Blob storage tooproduce utdata. Slutligen kan du använda en andra kopia aktiviteten toocopy hello utgående data tooAzure SQL Data Warehouse ovanpå vilka business intelligence (BI) reporting lösningar har skapats. Mer information om pipelines och aktiviteter finns [Pipelines och aktiviteter i Azure Data Factory](data-factory-create-pipelines.md).

En aktivitet kan ta noll eller fler indata **datauppsättningar**, och skapa en eller flera utdata-datauppsättningar. En inkommande datauppsättning representerar hello indata för en aktivitet i pipelinen hello och en datamängd för utdata representerar hello utdata för hello-aktivitet. Datauppsättningar identifierar data inom olika datalager, till exempel tabeller, filer, mappar och dokument. Azure-blobbdatauppsättning anger hello blob-behållaren och mappen i Blob storage från vilka hello pipeline bör du läsa hello data. 

Innan du skapar en datamängd, skapa en **länkade tjänsten** toolink dina data lagras toohello data factory. Länkade tjänster liknar anslutningssträngar som definierar hello anslutningsinformation som behövs för Data Factory tooconnect tooexternal resurser. Datauppsättningar identifiera data inom hello länkade datalager, till exempel SQL-tabeller, filer, mappar och dokument. Till exempel länkade ett Azure Storage tjänsten länkar en storage-konto toohello data factory. Azure-blobbdatauppsättning representerar hello blob-behållaren och hello-mapp som innehåller hello inkommande blobbar toobe bearbetas. 

Här är ett exempelscenario. toocopy data från Blob storage tooa SQL-databas, som du skapar två länkade tjänster: Azure Storage- och Azure SQL Database. Skapa sedan två datamängder: Azure Blob-datauppsättningen (varvid refererar toohello länkad Azure Storage service) och Azure SQL-tabell datauppsättningen (varvid refererar toohello Azure SQL Database länkad tjänst). hello Azure Storage- och Azure SQL Database länkade tjänster innehålla anslutningssträngar som Data Factory använder respektive vid körning tooconnect tooyour Azure Storage och Azure SQL Database. hello Azure-blobbdatauppsättning anger hello blob-behållaren och blob-mapp som innehåller hello inkommande blobbar i Blob storage. hello Azure SQL-tabell dataset anger hello SQL-tabell i SQL-databasen toowhich hello data toobe kopieras.

hello följande diagram visar hello relationerna mellan pipeline, aktivitet, datamängd och länkade tjänsten i Data Factory: 

![Förhållandet mellan pipeline, aktivitet, dataset, länkade tjänster](media/data-factory-create-datasets/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a>Datauppsättnings-JSON
En datamängd i Data Factory har definierats i JSON-format på följande sätt:

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
| namn |Namnet på hello dataset. Se [Azure Data Factory - namngivningsregler](data-factory-naming-rules.md) för namngivningsregler. |Ja |Ej tillämpligt |
| typ |typ av hello dataset. Ange en hello-typer som stöds av Data Factory (till exempel: AzureBlob, AzureSqlTable). <br/><br/>Mer information finns i [datauppsättningstypen](#Type). |Ja |Ej tillämpligt |
| struktur |Schemat för hello dataset.<br/><br/>Mer information finns i [datauppsättningsstrukturen](#Structure). |Nej |Ej tillämpligt |
| typeProperties | hello Typegenskaper är olika för varje typ (till exempel: Azure Blob, Azure SQL-tabell). Mer information om hello stöds typer och egenskaper finns [datauppsättningstypen](#Type). |Ja |Ej tillämpligt |
| extern | Boolesk flagga toospecify om en datamängd uttryckligen produceras av en data factory-pipelinen eller inte. Ange den här flaggan tootrue om hello inkommande datamängden för en aktivitet inte tillverkas av hello aktuella pipeline. Ange den här flaggan tootrue för hello inkommande dataset hello första aktivitet i hello pipeline.  |Nej |FALSKT |
| availability | Definierar hello bearbetning fönstret (till exempel varje timme eller varje dag) eller hello segmentering modellen för hello dataset produktion. Varje dataenhet förbrukats och produceras av en aktivitet körs kallas en datasektorn. Om hello tillgängligheten för en datamängd för utdata är uppsättningen toodaily (frekvens - dag, intervallet - 1), skapas en sektor dagligen. <br/><br/>Mer information finns i [Dataset tillgänglighet](#Availability). <br/><br/>Mer information om hello dataset segmentering modellen finns hello [schemaläggning och körning](data-factory-scheduling-and-execution.md) artikel. |Ja |Ej tillämpligt |
| policy |Definierar hello kriterier eller hello villkor som hello dataset segment måste vara uppfyllda. <br/><br/>Mer information finns i hello [Dataset princip](#Policy) avsnitt. |Nej |Ej tillämpligt |

## <a name="dataset-example"></a>DataSet-exempel
I följande exempel hello, hello datauppsättning representerar en tabell med namnet **mytable prefix** i en SQL-databas.

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties":
        {
            "tableName": "MyTable"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Observera följande punkter hello:

* **typen** tooAzureSqlTable anges.
* **tableName** typegenskapen (specifika tooAzureSqlTable typ) anges tooMyTable.
* **linkedServiceName** refererar tooa länkad tjänst av typen AzureSqlDatabase som definieras i hello nästa JSON-fragment. 
* **tillgänglighet frekvens** anges tooDay, och **intervall** too1 anges. Det innebär att hello datamängdssektor skapas varje dag.  

**AzureSqlLinkedService** definieras enligt följande:

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

I föregående JSON fragment hello:

* **typen** tooAzureSqlDatabase anges.
* **connectionString** typegenskapen anger information tooconnect tooa SQL-databas.  

Som du ser hello länkad tjänst definierar hur tooconnect tooa SQL-databas. hello dataset definierar vilka tabellen används som indata och utdata för hello aktivitet i en pipeline.   

> [!IMPORTANT]
> Om ett DataSet-objekt produceras av hello pipeline, bör det markeras som **externa**. Den här inställningen gäller vanligtvis tooinputs första aktivitet i en pipeline.   


## <a name="Type"></a>Typen av datauppsättning
hello beror hello dataset på hello dataarkiv som du använder. Se följande tabell för en lista över datakällor som stöds av Data Factory hello. Klicka på en data store toolearn hur toocreate en länkad tjänst och en datauppsättning för dessa data lagras.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Data som lagras med * kan vara lokalt eller på Azure-infrastrukturen som en tjänst (IaaS). Dessa datalager kräver tooinstall [Data Management Gateway](data-factory-data-management-gateway.md).

I exemplet hello hello föregående avsnitt hello typ hello dataset är inställd för**AzureSqlTable**. På samma sätt för ett Azure-blobbdatauppsättning hello typ hello dataset är inställd för**AzureBlob**, enligt följande JSON hello:

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

## <a name="Structure"></a>DataSet-struktur
Hej **struktur** avsnittet är valfritt. Den definierar hello schemat för hello dataset av som innehåller en samling med namn och datatyperna för kolumnerna. Du använder hello struktur avsnittet tooprovide typinformation används tooconvert typer och mappa kolumner från hello källa toohello mål. I följande exempel hello, hello datamängden har tre kolumner: `slicetimestamp`, `projectname`, och `pageviews`. De är av typen String, String och Decimal, respektive.

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Varje kolumn i hello strukturen innehåller hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| namn |Namnet på hello-kolumn. |Ja |
| typ |Datatypen för kolumnen hello.  |Nej |
| Kultur |. NET-baserade kultur toobe används när hello typen är en .NET-typ: `Datetime` eller `Datetimeoffset`. hello standardvärdet är `en-us`. |Nej |
| Format |Formatera strängen toobe används när hello typen är en .NET-typ: `Datetime` eller `Datetimeoffset`. |Nej |

hello följande riktlinjer hjälper dig att avgöra när tooinclude struktur information och vilka tooinclude i hello **struktur** avsnitt.

* **För strukturerade datakällor**, ange hello struktur avsnittet om du vill mappa källkolumner kolumner toosink och deras namn är inte hello samma. Den här typen av datakälla för strukturerade lagrar data schema och ange information tillsammans med själva hello informationen. Exempel på strukturerade datakällor är SQL Server, Oracle och Azure-tabellen. 
  
    Som typen finns redan för strukturerade datakällor, kan inkludera du inte typinformation när du inkluderar hello struktur avsnittet.
* **För schemat för skrivskyddade datakällor (särskilt Blob storage)**, du kan välja toostore data utan att spara schemat eller typ information med hello data. Inkludera struktur för dessa typer av datakällor om du vill toomap källkolumner kolumner toosink. Inkludera även struktur när hello datauppsättningen utgör indata för en kopieringsaktiviteten och datatyper i källan dataset ska vara konverterade toonative typer för hello sink. 
    
    Data Factory stöder följande värden för att ange information i strukturen hello: **Int16, Int32, Int64, Single, Double, Decimal, Byte [], Boolean, String, Guid, Datetime, Datetimeoffset och Timespan**. Dessa värden är Common Language Specification (CLS)-kompatibel,. NET-baserade typen värden.

Data Factory utför konverteringar automatiskt när du flyttar från en källdata datalager tooa sink-datalagret. 
  

## <a name="dataset-availability"></a>DataSet-tillgänglighet
Hej **tillgänglighet** avsnitt i en dataset definierar hello bearbetning fönstret (till exempel varje timme, varje dag eller varje vecka) för hello dataset. Läs mer om aktiviteten windows [schemaläggning och körning](data-factory-scheduling-and-execution.md).

hello efter tillgänglighet avsnittet anger att hello datamängd för utdata är antingen produceras per timma eller hello inkommande dataset finns varje timme:

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

Om hello pipeline har följande start- och sluttider hello:  

```json
    "start": "2016-08-25T00:00:00Z",
    "end": "2016-08-25T05:00:00Z",
```

hello datamängd för utdata skapas varje timme inom hello pipeline start- och sluttider. Det finns därför fem dataset-segment som producerats av denna pipeline, en för varje aktivitetsfönstret (12: 00 - 1 AM, 1 AM - 2 AM, 02: 00 - 3 AM, 3 AM - 4 AM, 4 AM - 5 AM). 

hello beskriver följande tabell egenskaper som kan användas i hello tillgänglighet avsnittet:

| Egenskap | Beskrivning | Krävs | Standard |
| --- | --- | --- | --- |
| frequency |Anger hello tidsenhet för dataset sektorn produktion.<br/><br/><b>Stöd för frekvens</b>: minut, timma, dag, vecka, månad |Ja |Ej tillämpligt |
| interval |Anger en multiplikator för frekvens.<br/><br/>”X frekvensintervall” avgör hur ofta hello segment skapas. Till exempel om du behöver hello dataset toobe segmenterat timme du ange <b>frekvens</b> för<b>timme</b>, och <b>intervall</b> för<b>1</b>.<br/><br/>Observera att om du anger **frekvens** som **minut**, bör du ange hello intervall toono som är mindre än 15. |Ja |Ej tillämpligt |
| format |Anger om hello segment ska produceras i hello början eller slutet av hello intervall.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul>Om **frekvens** har angetts för**månad**, och **style** har angetts för**EndOfInterval**, hello segment skapas på hello sista dagen i månaden. Om **style** har angetts för**StartOfInterval**, hello segment skapas på hello första dagen i månaden.<br/><br/>Om **frekvens** har angetts för**dag**, och **style** har angetts för**EndOfInterval**, hello segment skapas i hello senaste timmen på dagen hello.<br/><br/>Om **frekvens** har angetts för**timme**, och **style** har angetts för**EndOfInterval**, hello sektorn produceras hello slutet av hello timme. För ett segment för hello 1 PM - 14: 00 period, till exempel produceras hello sektorn klockan 2. |Nej |EndOfInterval |
| anchorDateTime |Definierar hello absolut placering i tid som används av hello scheduler toocompute dataset sektorn gränser. <br/><br/>Observera att om den här propoerty har datumdelar som är mer detaljerad än hello angivna frekvensen hello mer detaljerade delar ignoreras. Till exempel, om hello **intervall** är **varje timme** (frekvens: timme och intervall: 1), och hello **anchorDateTime** innehåller **minuter och sekunder**, hello minuter och sekunder delar av **anchorDateTime** ignoreras. |Nej |01/01/0001 |
| förskjutning |TimeSpan genom vilka hello början och slutet av alla dataset segment flyttat. <br/><br/>Observera att om båda **anchorDateTime** och **offset** anges, hello resultatet är hello kombineras SKIFT. |Nej |Ej tillämpligt |

### <a name="offset-example"></a>Offset exempel
Som standard varje dag (`"frequency": "Day", "interval": 1`) segment som börjar vid 12: 00 (midnatt) Coordinated Universal Time (UTC). Om du vill hello start tid toobe 06: 00 UTC-tid i stället anger du hello förskjutning som visas i följande fragment hello: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>anchorDateTime-exempel
I följande exempel hello, produceras hello dataset var 23: e timme. hello första sektorn startar vid hello tid som anges av **anchorDateTime**, som har angetts för`2017-04-19T08:00:00` (UTC).

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a>förskjutning/style-exempel
hello följande dataset är månadsvis, och skapas på hello 3: e varje månad kl 8:00 (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

## <a name="Policy"></a>DataSet-princip
Hej **princip** avsnitt i hello datauppsättningsdefinitionen definierar hello kriterier eller hello villkor som hello dataset segment måste vara uppfyllda.

### <a name="validation-policies"></a>Verifieringsprinciper
| Principnamn | Beskrivning | Används för| Krävs | Standard |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Verifierar att hello-data i **Azure Blob storage** uppfyller hello krav på minsta storlek (i megabyte). |Azure Blob Storage |Nej |Ej tillämpligt |
| minimumRows |Verifierar att hello-data i en **Azure SQL database** eller en **Azure-tabellen** innehåller hello minsta antal rader. |<ul><li>Azure SQL-databas</li><li>Azure-tabellen</li></ul> |Nej |Ej tillämpligt |

#### <a name="examples"></a>Exempel
**minimumSizeMB:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

**minimumRows:**

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

### <a name="external-datasets"></a>Externa datauppsättningar
Externa datauppsättningar är hello de som inte kommer från en pipeline som körs i hello data factory. Om hello datamängden har markerats som **externa**, hello **ExternalData** princip kan vara definierade tooinfluence hello funktionssätt hello dataset sektorn tillgänglighet.

Om ett DataSet-objekt produceras av Data Factory, bör det markeras som **externa**. Den här inställningen gäller vanligtvis toohello indata för den första aktiviteten i en pipeline om aktiviteten eller pipeline länkning används.

| Namn | Beskrivning | Krävs | Standardvärde |
| --- | --- | --- | --- |
| dataDelay |hello gång toodelay hello på hello tillgängligheten för hello externa data för hello får sektorn. Du kan till exempel fördröja en kontroll av varje timme med den här inställningen.<br/><br/>hello inställningen gäller endast toohello aktuell tid.  Om det är 1:00 PM just nu och det här värdet är 10 minuter, till exempel startas hello validering klockan 13:10.<br/><br/>Observera att den här inställningen inte påverkar segment i hello tidigare. Sektorer med **sektorn sluttid** + **dataDelay** < **nu** bearbetas utan fördröjning.<br/><br/>Gånger större än 23:59 timmar måste anges med hjälp av hello `day.hours:minutes:seconds` format. Till exempel toospecify 24 timmar, Använd inte 24:00:00. Använd i stället 1.00:00:00. Om du använder 24:00:00, behandlas den som 24 dagar (24.00:00:00). Ange 1:04:00:00 för 1 dag och 4 timmar. |Nej |0 |
| RetryInterval |hello väntetiden mellan felet och hello nästa försök. Den här inställningen gäller toopresent tid. Om hello föregående försök misslyckades hello nästa försök är efter hello **retryInterval** period. <br/><br/>Om den är 1:00 PM just nu kan börja vi hello första försöket. Om hello varaktighet toocomplete hello första verifieringen är 1 minut och hello-åtgärden misslyckades, hello nästa försök görs 1:00 + 1 min. (varaktighet) + 1 min. (Återförsöksintervall) = 1:02 PM. <br/><br/>För segment i hello senaste finns ingen fördröjning. hello försök sker omedelbart. |Nej |00:01:00 (1 minut) |
| retryTimeout |hello tidsgräns för varje nytt försök.<br/><br/>Om den här egenskapen anges too10 minuter hello validering ska slutföras inom 10 minuter. Om det tar längre tid än 10 minuter tooperform hello validering försök hello gånger ut.<br/><br/>Om alla försök för timeout för hello validering hello segment har markerats som **orsakade**. |Nej |00:10:00 (10 minuter) |
| maximumRetry |hello gånger toocheck hello tillgänglighet för hello externa data. hello högsta tillåtna värdet är 10. |Nej |3 |


## <a name="create-datasets"></a>Skapa datauppsättningar
Du kan skapa datauppsättningar på något av dessa verktyg och SDK: 

- Guiden Kopiera 
- Azure Portal
- Visual Studio
- PowerShell
- Azure Resource Manager-mall
- REST API
- .NET-API

Se hello följande självstudier för stegvisa instruktioner för att skapa pipelines och datauppsättningar på något av dessa verktyg och SDK:
 
- [Skapa en pipeline med en datatransformeringsaktivitet](data-factory-build-your-first-pipeline.md)
- [Skapa en pipeline med en aktivitet för flytt av data](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

När en pipeline skapas och distribueras, kan du hantera och övervaka din pipelines med hjälp av hello Azure portal blad eller hello övervakning och hantering av app. Se hello följande avsnitt instruktioner steg för steg: 

- [Övervaka och hantera pipelines med hjälp av Azure portal blad](data-factory-monitor-manage-pipelines.md)
- [Övervaka och hantera pipelines med hello övervakning och hantering av appen](data-factory-monitor-manage-app.md)


## <a name="scoped-datasets"></a>Begränsade datauppsättningar
Du kan skapa datauppsättningar som är begränsade tooa pipeline med hjälp av hello **datauppsättningar** egenskapen. Dessa data kan endast användas av aktiviteter i denna pipeline, inte av aktiviteter i andra pipelines. hello följande exempel definierar en pipeline med två datamängder (InputDataset rdc och OutputDataset rdc) toobe som används i hello pipeline.  

> [!IMPORTANT]
> Begränsade datauppsättningar kan bara användas med enstaka pipelines (där **pipelineMode** har angetts för**OneTime**). Se [Onetime pipeline](data-factory-create-pipelines.md#onetime-pipeline) mer information.
>
>

```json
{
    "name": "CopyPipeline-rdc",
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
                        "name": "InputDataset-rdc"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-rdc"
                    }
                ],
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1,
                    "style": "StartOfInterval"
                },
                "name": "CopyActivity-0"
            }
        ],
        "start": "2016-02-28T00:00:00Z",
        "end": "2016-02-28T00:00:00Z",
        "isPaused": false,
        "pipelineMode": "OneTime",
        "expirationTime": "15.00:00:00",
        "datasets": [
            {
                "name": "InputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "InputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/input",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": true,
                    "policy": {}
                }
            },
            {
                "name": "OutputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "OutputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/output",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": false,
                    "policy": {}
                }
            }
        ]
    }
}
```

## <a name="next-steps"></a>Nästa steg
- Läs mer om pipelines [skapa pipelines](data-factory-create-pipelines.md). 
- Mer information om hur pipelines schemaläggs och körs finns [schemaläggning och körning i Azure Data Factory](data-factory-scheduling-and-execution.md). 
