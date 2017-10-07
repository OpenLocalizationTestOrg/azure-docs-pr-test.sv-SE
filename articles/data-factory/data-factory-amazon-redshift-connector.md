---
title: "aaaMove data från Amazon Redshift med hjälp av Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från Amazon Redshift med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 01d15078-58dc-455c-9d9d-98fbdf4ea51e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: 2a097320734ebdd57282d250f7fdba35741777f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Flytta data från Amazon Redshift med hjälp av Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från Amazon Redshift. hello artikel bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten. 

Du kan kopiera data från Amazon Redshift tooany stöds sink-datalagret. En lista över datakällor som stöds som sänkor av hello kopieringsaktiviteten finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Data factory stöder för närvarande flytta data från Amazon Redshift tooother datalager, men inte för att flytta data från andra data lagras tooAmazon Redshift.

## <a name="prerequisites"></a>Krav
* Om du flyttar data tooan lokala datalagret, installera [Data Management Gateway](data-factory-data-management-gateway.md) på en lokal dator. Sedan bevilja Data Management Gateway (Använd IP-adressen för datorn hello) hello åtkomst tooAmazon Redshift kluster. Se [auktorisera åtkomst toohello klustret](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) anvisningar.
* Om du flyttar data tooan Azure datalagret finns [Azure Data Center IP-adressintervall](https://www.microsoft.com/download/details.aspx?id=41653) för hello Compute IP-adress och SQL-intervall som används av hello Azure-datacenter.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en källa för Amazon Redshift med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret: 

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. 
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. 

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett dataarkiv för Amazon Redshift finns [JSON-exempel: kopiera data från Amazon Redshift tooAzure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) i den här artikeln. 

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAmazon Redshift: 

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello följande tabell ger en beskrivning för JSON-element specifika tooAmazon Redshift länkade tjänsten.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello Typegenskapen måste anges till: **AmazonRedshift**. |Ja |
| server |IP-adressen eller värdnamnet namnet på hello Amazon Redshift server. |Ja |
| port |hello antal hello TCP-port som hello Amazon Redshift server använder toolisten för klientanslutningar. |Nej, standardvärde: 5439 |
| Databasen |Namnet på hello Amazon Redshift databas. |Ja |
| användarnamn |Namnet på användaren som har åtkomst till toohello databas. |Ja |
| lösenord |Lösenordet för användarkontot för hello. |Ja |

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och principen är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).

Hej **typeProperties** avsnittet är olika för varje typ av datauppsättningen. Den ger information om hello platsen för hello data i datalagret hello. Hej typeProperties avsnittet för dataset av typen **RelationalTable** (som omfattar Amazon Redshift dataset) har hello följande egenskaper

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i hello Amazon Redshift databas som den länkade tjänsten refererar till. |Nej (om **frågan** av **RelationalSource** har angetts) |

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.

Medan egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

Om källan för kopieringsaktiviteten är av typen **RelationalSource** (som omfattar Amazon Redshift), hello följande egenskaper finns i avsnittet typeProperties:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: Välj * från mytable prefix. |Nej (om **tableName** av **dataset** har angetts) |

## <a name="json-example-copy-data-from-amazon-redshift-tooazure-blob"></a>JSON-exempel: kopiera data från Amazon Redshift tooAzure Blob
Det här exemplet visas hur toocopy data från en Amazon Redshift databasen tooan Azure Blob Storage. Dock datan kan kopieras **direkt** tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.  

hello exemplet har hello följande data factory-enheter:

* En länkad tjänst av typen [AmazonRedshift](#linked-service-properties).
* En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](#dataset-properties).
* Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).

hello exemplet kopierar data från ett frågeresultat i Amazon Redshift tooa blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

**Amazon Redshift länkade tjänsten:**

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< hello IP address or host name of hello Amazon Redshift server >",
            "port": <hello number of hello TCP port that hello Amazon Redshift server uses toolisten for client connections.>,
            "database": "<hello database name of hello Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

**Azure Storage länkade tjänsten:**

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Amazon Redshift inkommande datauppsättningen:**

Ange `"external": true` informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory. Ange den här egenskapen tootrue på ett inkommande datamängd som inte tillverkas av en aktivitet i hello pipeline.

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

**Azure Blob utdatauppsättningen:**

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Kopiera aktivitet i en pipeline med Azure Redshift källa (RelationalSource) och Blob mottagare:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** och **sink** typ har angetts för**BlobSink**. hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
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
                "inputs": [
                    {
                        "name": "AmazonRedshiftInputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AmazonRedshiftToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-amazon-redshift"></a>Mappning för Amazon Redshift
Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande två sätt:

1. Konvertera från inbyggda typer too.NET källtypen
2. Konvertera från .NET typen toonative Mottagartypen

När du flyttar data tooAmazon Redshift används hello följande mappningar från Amazon Redshift typer too.NET typer.

| Amazon Redshift typ | .NET baserat typ |
| --- | --- |
| SMALLINT |Int16 |
| HELTAL |Int32 |
| BIGINT |Int64 |
| DECIMAL |Decimal |
| VERKLIG |Enskild |
| DUBBEL PRECISION |dubbla |
| BOOLESKT VÄRDE |Sträng |
| CHAR |Sträng |
| VARCHAR |Sträng |
| DATUM |Datum och tid |
| TIDSSTÄMPEL |Datum och tid |
| TEXT |Sträng |

## <a name="map-source-toosink-columns"></a>Mappa källkolumner toosink
toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Upprepbar läsning från relationella källor
När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat. I Azure Data Factory, kan du köra en sektor manuellt. Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår. När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs. Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.

## <a name="next-steps"></a>Nästa steg
Se hello följande artiklar:

* [Kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) stegvisa instruktioner för att skapa en pipeline med en kopia-aktivitet.
