---
title: "aaaMove data från DB2 med hjälp av Azure Data Factory | Microsoft Docs"
description: "Lär dig hur toomove data från en lokal DB2-databas med hjälp av Azure Data Factory-Kopieringsaktiviteten"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c1644e17-4560-46bb-bf3c-b923126671f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: 696ac059be644cb3901c37d2fc746e0682c65a1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-db2-by-using-azure-data-factory-copy-activity"></a>Flytta data från DB2 med hjälp av Azure Data Factory-Kopieringsaktiviteten
Den här artikeln beskriver hur du kan använda Kopieringsaktiviteten i Azure Data Factory toocopy data från ett dataarkiv för lokala DB2-databas tooa. Du kan kopiera tooany datalager som har listats som en mottagare som stöds i hello [Data Factory data movement aktiviteter](data-factory-data-movement-activities.md#supported-data-stores-and-formats) artikel. Det här avsnittet bygger på hello Data Factory artikel som visar en översikt över data flyttas med hjälp av Kopieringsaktiviteten och visar hello stöds data store kombinationer. 

Data Factory stöder för närvarande endast flytta data från en DB2-databas tooa [stöds sink datalagret](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Flytta data från andra data lagras tooa DB2-databas inte stöds.

## <a name="prerequisites"></a>Krav
Data Factory stöder anslutande tooan lokala DB2-databas med hjälp av hello [data management gateway](data-factory-data-management-gateway.md). Stegvisa instruktioner tooset hello gateway data i pipeline toomove dina data finns i avsnittet hello [flytta data från lokala toocloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

En gateway krävs även om hello DB2 finns på Azure IaaS-VM. Du kan installera hello gateway på hello samma IaaS VM som hello datalager. Om hello gateway kan ansluta toohello databasen kan installera du hello gateway på en annan virtuell dator.

hello data management gateway innehåller en inbyggd DB2-drivrutin, så att du inte behöver toomanually installera en drivrutin toocopy data från DB2.

> [!NOTE]
> Tips om hur du felsöker anslutning och gateway-problem finns i hello [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) artikel.


## <a name="supported-versions"></a>Versioner som stöds
hello Data Factory DB2-koppling stöder hello följande IBM DB2-plattformar och versioner med distribuerade relationella Database Architecture (DRDA) SQL åtkomst Manager version 9, 10 och 11:

* IBM DB2 för z/OS-versionen 11,1
* IBM DB2 för z/OS-version 10.1
* IBM DB2 för i (AS400) version 7.2
* IBM DB2 för i (AS400) version 7.1
* IBM DB2 för Linux, UNIX- och Windows (LUW) version 11
* IBM DB2 för LUW version 10.5
* IBM DB2 för LUW version 10.1

> [!TIP]
> Om felmeddelande hello ”hello paketet motsvarande tooan SQL-instruktionen Körningsbegäran hittades inte. SQLSTATE = 51002 SQLCODE =-805 ”, Hej orsaken är ett nödvändigt paket inte skapas för hello normal användare på hello OS. tooresolve i detta fall, följ instruktionerna för din servertyp DB2:
> - DB2 för i (AS400): låta en användare skapar hello samling för hello normal användare innan du kör Kopieringsaktiviteten. toocreate hello insamling, användning hello kommando:`create collection <username>`
> - DB2 för z/OS eller LUW: Använd ett konto med hög behörighet--privilegierad användare eller administratör som har paketet myndigheter och BIND BINDADD, BEVILJA behörighet för EXECUTE tooPUBLIC--toorun hello kopiera en gång. hello nödvändiga paketet skapas automatiskt under hello kopia. Därefter kan du växla tillbaka toohello normal användare för efterföljande kopia-körs.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en aktivitet toomove data från en lokal DB2-datalagret med hjälp av olika verktyg och API: er: 

- hello enklaste sättet toocreate en pipeline är toouse hello Azure Data Factory kopiera guiden. En snabb genomgång om hur du skapar en pipeline med hjälp av hello guiden Kopiera finns hello [Självstudier: skapa en pipeline med hello guiden Kopiera](data-factory-copy-data-wizard-tutorial.md). 
- Du kan också använda verktygen toocreate en pipeline, inklusive hello Azure-portalen, Visual Studio, Azure PowerShell, en Azure Resource Manager-mall, hello .NET-API och hello REST API. Stegvisa instruktioner toocreate en pipeline med en kopieringsaktiviteten finns hello [Kopieringsaktiviteten kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:

1. Skapa länkade tjänster toolink indata och utdata lagrar tooyour data factory.
2. Skapa datauppsättningar toorepresent indata och utdata för hello kopieringen. 
3. Skapa en pipeline med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. 

När du använder hello guiden Kopiera, JSON definitioner för hello Data Factory länkade tjänster, skapas automatiskt datauppsättningar och pipeline entiteter för dig. När du använder verktyg eller API: er (utom hello .NET-API) kan du definiera hello Data Factory-enheter med hjälp av hello JSON-format. Hej [JSON-exempel: kopiera data från DB2 tooAzure Blob storage](#json-example-copy-data-from-db2-to-azure-blob) visar hello JSON definitioner för hello Data Factory-enheter som ska använda toocopy data från ett lokalt DB2-dataarkiv.

hello följande avsnitt innehåller information om hello JSON-egenskaper används toodefine hello Data Factory enheter som är specifika tooa DB2-datalagret.

## <a name="db2-linked-service-properties"></a>DB2 länkade tjänstens egenskaper
hello i den följande tabellen listas hello JSON egenskaper som är specifika tooa DB2 länkade tjänsten.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| **typ** |Den här egenskapen måste anges för**OnPremisesDB2**. |Ja |
| **Server** |hello namnet på hello DB2-server. |Ja |
| **databasen** |hello namnet på hello DB2-databas. |Ja |
| **schemat** |hello namnet på hello schema i hello DB2-databasen. Den här egenskapen är skiftlägeskänsliga. |Nej |
| **authenticationType** |hello typ av autentisering som används tooconnect toohello DB2-databasen. hello möjliga värden är: anonym, grundläggande och Windows. |Ja |
| **användarnamn** |hello namnet för hello användarkonto om du använder grundläggande eller Windows-autentisering. |Nej |
| **lösenord** |hello lösenordet för användarkontot för hello. |Nej |
| **gatewayName** |hello namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala DB2-databas. |Ja |

## <a name="dataset-properties"></a>Egenskaper för datamängd
En lista över egenskaper som är tillgängliga för att definiera datauppsättningar och hello avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitten som **struktur**, **tillgänglighet**, och hello **princip** för en dataset JSON är liknande för alla typer av dataset Azure SQL (Azure Blob storage, Azure Table lagring och så vidare).

Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej **typeProperties** avsnittet för en dataset av typen **RelationalTable**, vilket inkluderar hello DB2 dataset, har hello följande egenskap:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| **tableName** |hello namn på hello tabell i hello DB2-databasinstansen som hello länkade tjänsten refererar till. Den här egenskapen är skiftlägeskänsliga. |Nej (om hello **frågan** -egenskapen för en kopia aktivitet av typen **RelationalSource** har angetts) |

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En lista över egenskaper som är tillgängliga för att definiera kopiera aktiviteter och hello avsnitt finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Kopiera egenskaper för aktivitet som **namn**, **beskrivning**, **indata** tabellen **matar ut** tabellen och **princip**, är tillgängliga för alla typer av aktiviteter. Hej egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet för varje aktivitetstyp. För Kopieringsaktiviteten varierar hello egenskaper beroende på hello typer av datakällor och sänkor.

För Kopieringsaktiviteten när hello källa är av typen **RelationalSource** (som omfattar DB2), hello följande egenskaper är tillgängliga i hello **typeProperties** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| **frågan** |Använda hello anpassad fråga tooread hello data. |SQL-sträng. Exempel: `"query": "select * from "MySchema"."MyTable""` |Nej (om hello **tableName** -egenskapen för en dataset har angetts) |

> [!NOTE]
> Schema och tabellnamn är skiftlägeskänsliga. I hello frågeuttrycket omge egenskapsnamn med ”” (dubbla citattecken). Exempel:
>
> ```sql
> "query": "select * from "DB2ADMIN"."Customers""
> ```

## <a name="json-example-copy-data-from-db2-tooazure-blob-storage"></a>JSON-exempel: kopiera data från DB2 tooAzure Blob storage
Det här exemplet innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av hello [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). hello exempel visas hur toocopy data från en DB2-databas tooBlob lagring. Dock datan kan kopieras för[stöds data lagra Mottagartypen](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av Azure Data Factory-Kopieringsaktiviteten.

hello exemplet har hello följande Data Factory-enheter:

- En DB2 länkade tjänsten av typen [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)
- Ett Azure Blob storage länkade tjänsten av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
- Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)
- Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
- En [pipeline](data-factory-create-pipelines.md) med en kopia-aktivitet som använder hello [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) egenskaper

hello exemplet kopierar data från ett frågeresultat i Azure blob för tooan en DB2-databasen varje timme. hello JSON egenskaper som används i hello exemplet beskrivs i hello avsnitten som följer hello entitet definitioner.

Som ett första steg bör du installera och konfigurera en datagateway. Anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**DB2 länkad tjänst**

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

**Azure Blob länkade lagringstjänsten**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorageLinkedService",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```

**DB2 inkommande dataset**

hello exemplet förutsätter att du har skapat en tabell i DB2 med namnet ”mytable” som prefix som har en kolumn med rubriken ”tidsstämpel” för hello tid series-data.

Hej **externa** -egenskapen anges för ”true”. Den här inställningen informerar hello Data Factory-tjänsten att den här datauppsättningen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory. Observera att hello **typen** egenskapen för**RelationalTable**.


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

**Azure Blob utdatauppsättningen**

Data skrivs tooa nya blob varje timme genom att ange hello **frekvens** egenskapen för ”timme” och hello **intervall** egenskapen too1. Hej **folderPath** egenskapen för blob hello utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder hello år, månad, dag och timme delar av hello starttid.

```json
{
    "name": "AzureBlobDb2DataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Pipeline för hello kopieringsaktiviteten**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerad toouse angivna indata och utdata-datauppsättningar och som är schemalagda toorun varje timme. I hello JSON-definitionen för hello pipelinen hello **källa** typ har angetts för**RelationalSource** och hello **sink** typ har angetts för**BlobSink**. hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data från hello ”order”-tabellen.

```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for hello copy activity",
        "activities": [
            {
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
                "inputs": [
                    {
                        "name": "Db2DataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDb2DataSet"
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
                "name": "Db2ToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-db2"></a>Mappning för DB2
Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln Kopieringsaktiviteten utför automatisk konverteringar från källtyp typen toosink med hello följande två sätt:

1. Konvertera från en inbyggd typ tooa .NET källtypen
2. Konvertera från en .NET typen tooa interna sink-typ

hello används följande mappningar när Kopieringsaktiviteten konverterar hello data från en DB2 typen tooa .NET-typ:

| Typen för DB2-databas | .NET framework-typ |
| --- | --- |
| SmallInt |Int16 |
| Integer |Int32 |
| BigInt |Int64 |
| Real |Enskild |
| dubbla |dubbla |
| flyttal |dubbla |
| Decimal |Decimal |
| DecimalFloat |Decimal |
| numeriskt |Decimal |
| Date |Datum och tid |
| Tid |TimeSpan |
| tidsstämpel |Datum och tid |
| XML |byte] |
| Char |Sträng |
| VarChar |Sträng |
| LongVarChar |Sträng |
| DB2DynArray |Sträng |
| Binär |byte] |
| VarBinary |byte] |
| LongVarBinary |byte] |
| Bild |Sträng |
| VarGraphic |Sträng |
| LongVarGraphic |Sträng |
| CLOB |Sträng |
| Blob |byte] |
| DbClob |Sträng |
| SmallInt |Int16 |
| Integer |Int32 |
| BigInt |Int64 |
| Real |Enskild |
| dubbla |dubbla |
| flyttal |dubbla |
| Decimal |Decimal |
| DecimalFloat |Decimal |
| numeriskt |Decimal |
| Date |Datum och tid |
| Tid |TimeSpan |
| tidsstämpel |Datum och tid |
| XML |byte] |
| Char |Sträng |

## <a name="map-source-toosink-columns"></a>Mappa källkolumner toosink
hur toomap kolumner i hello källa dataset toocolumns i hello sink dataset Se toolearn [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-reads-from-relational-sources"></a>Repeterbara läsningar från relationella källor
När du kopierar data från en relationsdatabas, Kom ihåg tooavoid repeterbarhet oönskade resultat. I Azure Data Factory, kan du köra en sektor manuellt. Du kan också konfigurera hello försök **princip** -egenskapen för en dataset toorerun ett segment när ett fel uppstår. Kontrollera som hello samma data läses oavsett hur många gånger hello sektorn är kör och oavsett hur du kör hello sektorn. Mer information finns i [Repeatable läser från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestanda- och justering
Lär dig mer om viktiga faktorer som påverkar prestanda hello Kopieringsaktiviteten och sätt toooptimize prestanda i hello [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md).
