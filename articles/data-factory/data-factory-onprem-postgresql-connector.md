---
title: "aaaMove data från PostgreSQL med hjälp av Azure Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från PostgreSQL-databas med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 888d9ebc-2500-4071-b6d1-0f6bd1b5997c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: ea384f4e06f7d7bedae2949e4ea727c8f8806614
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a>Flytta data från PostgreSQL med hjälp av Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal PostgreSQL-databas. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.

Du kan kopiera data från ett lokalt PostgreSQL data store tooany stöds sink dataarkiv. En lista över datakällor som stöds som sänkor av hello kopieringsaktiviteten finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Data factory för närvarande stöder flyttning av data från en PostgreSQL-databas tooother datalager, men inte för att flytta data från andra data lagras tooan PostgreSQL-databas. 

## <a name="prerequisites"></a>Nödvändiga komponenter

Data Factory-tjänsten stöder anslutande tooon lokala PostgreSQL källor med hello Data Management Gateway. Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway.

Gateway krävs även om hello PostgreSQL-databasen finns i en Azure IaaS-VM. Du kan installera gatewayen på hello samma IaaS VM som hello data lagras eller på en annan virtuell dator så länge som hello gateway kan ansluta toohello databas.

> [!NOTE]
> Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.

## <a name="supported-versions-and-installation"></a>Versioner som stöds och installation
För Data Management Gateway tooconnect toohello PostgreSQL-databas, installera hello [Ngpsql dataprovider för PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 eller ovan på hello samma system som hello Data Management Gateway. PostgreSQL version 7.4 och senare stöds.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från ett dataarkiv för lokala PostgreSQL med hjälp av olika verktyg/API: er. 

- hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data. 
- Du kan också använda följande verktyg toocreate en pipeline hello: 
    - Azure Portal
    - Visual Studio
    - Azure PowerShell
    - Azure Resource Manager-mall
    - .NET-API
    - REST API

     Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. 
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. 

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett dataarkiv för lokala PostgreSQL finns [JSON-exempel: kopiera data från PostgreSQL tooAzure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) i den här artikeln. 

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooa PostgreSQL-datalager:

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello följande tabell innehåller en beskrivning för JSON-element specifika tooPostgreSQL länkad tjänst.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello Typegenskapen måste anges till: **OnPremisesPostgreSql** |Ja |
| server |Namnet på hello PostgreSQL-server. |Ja |
| Databasen |Namnet på hello PostgreSQL-databas. |Ja |
| Schemat |Namnet på hello schema i hello-databasen. hello schemanamnet är skiftlägeskänslig. |Nej |
| AuthenticationType |Typ av autentisering används tooconnect toohello PostgreSQL-databas. Möjliga värden är: anonym, grundläggande och Windows. |Ja |
| användarnamn |Ange användarnamnet om du använder grundläggande eller Windows-autentisering. |Nej |
| lösenord |Ange lösenord för hello-användarkonto som du angav för hello användarnamn. |Nej |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala PostgreSQL-databas. |Ja |

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen.

hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej typeProperties avsnittet för dataset av typen **RelationalTable** (som omfattar PostgreSQL dataset) har hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i hello PostgreSQL-databasinstansen som den länkade tjänsten refererar till. hello tableName är skiftlägeskänslig. |Nej (om **frågan** av **RelationalSource** har angetts) |

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.

De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

När datakällan är av typen **RelationalSource** (som omfattar PostgreSQL), hello följande egenskaper finns i avsnittet typeProperties:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: ”frågan” ”: Välj * från \"MySchema\".\" Mytable prefix\"”. |Nej (om **tableName** av **dataset** har angetts) |

> [!NOTE]
> Schema och tabellnamn är skiftlägeskänsliga. Innesluts i `""` (dubbla citattecken) i hello-frågan.  

**Exempel:**

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-tooazure-blob"></a>JSON-exempel: kopiera data från PostgreSQL tooAzure Blob
Det här exemplet innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De visar hur toocopy data från PostgreSQL-databas tooAzure Blob Storage. Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.   

> [!IMPORTANT]
> Det här exemplet innehåller JSON kodavsnitt. Stegvisa instruktioner för att skapa datafabriken hello inkluderas inte. Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.

hello exemplet har hello följande data factory-enheter:

1. En länkad tjänst av typen [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Hej [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar data från ett frågeresultat i PostgreSQL-databas tooa blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

Som ett första steg bör du konfigurera hello data management gateway. hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**PostgreSQL länkade tjänsten:**

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
**Azure Blob storage länkade tjänsten:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
    }
    }
}
```
**PostgreSQL inkommande datauppsättningen:**

hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i PostgreSQL och innehåller en kolumn med namnet ”tidsstämpel” för tid series-data.

Ange `"external": true` informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

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

**Azure Blob utdatauppsättningen:**

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.

```json
{
    "name": "AzureBlobPostgreSqlDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Pipeline med kopieringsaktiviteten:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** och **sink** typ har angetts för**BlobSink**. hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data från hello public.usstates tabell i hello PostgreSQL-databas.

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
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
                "inputs": [
                    {
                        "name": "PostgreSqlDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobPostgreSqlDataSet"
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
                "name": "PostgreSqlToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
## <a name="type-mapping-for-postgresql"></a>Mappning för PostgreSQL
Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:

1. Konvertera från inbyggda typer too.NET källtypen
2. Konvertera från .NET typen toonative Mottagartypen

När du flyttar data tooPostgreSQL som hello följande mappningar används från PostgreSQL typ too.NET.

| Typ av PostgreSQL-databas | PostgresSQL alias | .NET framework-typ |
| --- | --- | --- |
| abstime | |Datum och tid | &nbsp;
| bigint |int8 |Int64 |
| bigserial |serial8 |Int64 |
| bitars [(n)] | |Byte [], sträng | &nbsp;
| bit varierande [(n)] |varbit |Byte [], sträng |
| Booleskt värde |bool |Booleskt värde |
| Rutan | |Byte [], sträng |&nbsp;
| bytea | |Byte [], sträng |&nbsp;
| tecknet [(n)] |char [(n)] |Sträng |
| tecknet varierande [(n)] |varchar [(n)] |Sträng |
| CID | |Sträng |&nbsp;
| CIDR | |Sträng |&nbsp;
| cirkel | |Byte [], sträng |&nbsp;
| Datum | |Datum och tid |&nbsp;
| DateRange | |Sträng |&nbsp;
| dubbel precision |FLOAT8 |dubbla |
| inet | |Byte [], sträng |&nbsp;
| intarry | |Sträng |&nbsp;
| int4range | |Sträng |&nbsp;
| int8range | |Sträng |&nbsp;
| heltal |int, int4 |Int32 |
| intervallet [fält] [(p)] | |Tidsintervall |&nbsp;
| JSON | |Sträng |&nbsp;
| jsonb | |byte] |&nbsp;
| raden | |Byte [], sträng |&nbsp;
| lseg | |Byte [], sträng |&nbsp;
| macaddr | |Byte [], sträng |&nbsp;
| Money | |Decimal |&nbsp;
| numeriska [(p, s)] |decimal [(p, s)] |Decimal |
| numrange | |Sträng |&nbsp;
| OID | |Int32 |&nbsp;
| Sökväg | |Byte [], sträng |&nbsp;
| pg_lsn | |Int64 |&nbsp;
| punkt | |Byte [], sträng |&nbsp;
| polygon | |Byte [], sträng |&nbsp;
| Verklig |FLOAT4 |Enskild |
| smallint |int2 |Int16 |
| smallserial |serial2 |Int16 |
| Seriell |serial4 |Int32 |
| Text | |Sträng |&nbsp;

## <a name="map-source-toosink-columns"></a>Mappa källkolumner toosink
toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Upprepbar läsning från relationella källor
När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat. I Azure Data Factory, kan du köra en sektor manuellt. Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår. När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs. Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.
