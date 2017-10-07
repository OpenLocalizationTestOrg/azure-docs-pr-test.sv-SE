---
title: "aaaMove data från SAP HANA med Azure Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från SAP HANA med Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 5cefe4c8ed01ea4e86e02496b2f8a9083d0b949c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sap-hana-using-azure-data-factory"></a>Flytta data från SAP HANA med Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal SAP HANA. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.

Du kan kopiera data från en lokal SAP HANA data store tooany stöds sink dataarkiv. En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell. Data factory stöder för närvarande endast flytta data från ett SAP HANA tooother lagrar, men inte för att flytta data från andra data lagras tooan SAP HANA.

## <a name="supported-versions-and-installation"></a>Versioner som stöds och installation
Den här anslutningen har stöd för någon version av SAP HANA-databas. Det stöder kopiering av data från HANA informationsmodeller (till exempel analys- och vyer) och raden/kolumnen tabeller med SQL-frågor.

tooenable hello anslutningen toohello SAP HANA-instans, installera hello följande komponenter:
- **Data Management Gateway**: Data Factory-tjänsten stöder anslutningar tooon lokala data Arkiv (inklusive SAP HANA) med hjälp av en komponent som kallas Data Management Gateway. toolearn om Data Management Gateway och stegvisa instruktioner för hur du konfigurerar hello gateway finns [flytta data mellan lokala data lagra toocloud datalagret](data-factory-move-data-between-onprem-and-cloud.md) artikel. Gateway krävs även om hello SAP HANA finns i en Azure IaaS-virtuella (VM). Du kan installera hello gateway på hello samma virtuella dator som hello data lagras eller på en annan virtuell dator så länge som hello gateway kan ansluta toohello databas.
- **SAP HANA ODBC-drivrutinen** på hello gateway-datorn. Du kan hämta hello SAP HANA ODBC-drivrutinen från hello [SAP Software Download Center](https://support.sap.com/swdc). Sökning med hello nyckelordet **SAP HANA-klienten för Windows**. 

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er. 

- hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data. 
- Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. 
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. 

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från en lokal SAP HANA finns [JSON-exempel: kopiera data från SAP HANA tooAzure Blob](#json-example-copy-data-from-sap-hana-to-azure-blob) i den här artikeln. 

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooan SAP HANA-datalager:

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello följande tabell innehåller beskrivning för JSON-element specifika tooSAP HANA länkade tjänsten.

Egenskap | Beskrivning | Tillåtna värden | Krävs
-------- | ----------- | -------------- | --------
server | Namnet på hello-server på vilken hello SAP HANA instansen finns. Om servern använder en anpassad port, ange `server:port`. | Sträng | Ja
AuthenticationType | Typ av autentisering. | Sträng. ”Basic” eller ”Windows” | Ja 
användarnamn | Namnet på hello-användare som har åtkomst toohello SAP-server | Sträng | Ja
lösenord | Lösenordet för hello. | Sträng | Ja
gatewayName | Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokal SAP HANA-instans. | Sträng | Ja
encryptedCredential | hello krypterade autentiseringsuppgifter strängen. | Sträng | Nej

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).

Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Det finns inga typspecifika egenskaper som stöds för hello SAP HANA dataset av typen **RelationalTable**. 


## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper, till exempel namn, beskrivning, inkommande och utgående tabeller är principer är tillgängliga för alla typer av aktiviteter.

Medan egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

När datakällan i en Kopieringsaktivitet är av typen **RelationalSource** (som innehåller SAP HANA), hello följande egenskaper finns i avsnittet typeProperties:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB | Anger hello SQL-frågan tooread data från hello SAP HANA-instans. | SQL-frågan. | Ja |

## <a name="json-example-copy-data-from-sap-hana-tooazure-blob"></a>JSON-exempel: kopiera data från SAP HANA tooAzure Blob
hello följande exempel innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Det här exemplet visas hur toocopy data från en lokal SAP HANA tooan Azure Blob Storage. Dock datan kan kopieras **direkt** tooany av hello sänkor visas [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.  

> [!IMPORTANT]
> Det här exemplet innehåller JSON kodavsnitt. Stegvisa instruktioner för att skapa datafabriken hello inkluderas inte. Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.

hello exemplet har hello följande data factory-enheter:

1. En länkad tjänst av typen [SapHana](#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar data från en SAP HANA instans tooan Azure blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

Som ett första steg bör du konfigurera hello data management gateway. hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.

### <a name="sap-hana-linked-service"></a>SAP HANA länkad tjänst
Den här länkade tjänsten länkar din SAP HANA instans toohello data factory. hello typegenskapen har ställts in för**SapHana**. Hej typeProperties avsnittet innehåller anslutningsinformation för hello SAP HANA-instans.

```json
{
    "name": "SapHanaLinkedService",
    "properties":
    {
        "type": "SapHana",
        "typeProperties":
        {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```

### <a name="azure-storage-linked-service"></a>Länkad Azure-lagringstjänst
Det här länkade tjänsten länkar din Azure Storage-konto toohello data factory. hello typegenskapen har ställts in för**AzureStorage**. Hej typeProperties avsnittet innehåller anslutningsinformation för hello Azure Storage-konto.

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

### <a name="sap-hana-input-dataset"></a>SAP HANA inkommande dataset

Den här datauppsättningen definierar hello SAP HANA dataset. Du ställer in hello på hello Data Factory datamängd för**RelationalTable**. För närvarande kan anger du inte några typspecifika egenskaper för en SAP HANA-datamängd. hello frågan i hello kopiera aktivitetsdefinitionen anger vilka data tooread från hello SAP HANA-instans. 

Ange externa egenskapen tootrue informerar hello Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

Frekvensen och intervall egenskaper definierar hello schema. I det här fallet läses hello data från hello SAP HANA instans varje timme. 

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

### <a name="azure-blob-output-dataset"></a>Utdatauppsättning för Azure-blobb
Den här datauppsättningen definierar hello utdata Azure-blobbdatauppsättning. hello typegenskapen anges tooAzureBlob. Hej typeProperties avsnittet innehåller hello data kopieras från hello SAP HANA-instans ska lagras. hello data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/saphana/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="pipeline-with-copy-activity"></a>Pipeline med kopieringsaktiviteten

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** (för SAP HANA-källa) och **sink** typ har angetts för**BlobSink**. hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.

```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
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
                "inputs": [
                    {
                        "name": "SapHanaDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDataSet"
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
                "name": "SapHanaToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```


### <a name="type-mapping-for-sap-hana"></a>Mappning för SAP HANA
Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande två sätt:

1. Konvertera från inbyggda typer too.NET källtypen
2. Konvertera från .NET typen toonative Mottagartypen

När du flyttar data från SAP HANA används hello följande mappningar från SAP HANA typer too.NET typer.

SAP HANA-typ | .NET baserat typ
------------- | ---------------
TINYINT | Mottagna byte
SMALLINT | Int16
INT | Int32
BIGINT | Int64
VERKLIG | Enskild
DUBBEL | Enskild
DECIMAL | Decimal
BOOLESKT VÄRDE | Mottagna byte
VARCHAR | Sträng
NVARCHAR | Sträng
CLOB | byte]
ALPHANUM | Sträng
BLOB | byte]
DATUM | Datum och tid
TID | TimeSpan
TIDSSTÄMPEL | Datum och tid
SECONDDATE | Datum och tid

## <a name="known-limitations"></a>Kända begränsningar
Det finns några kända begränsningar när du kopierar data från SAP HANA:

- NVARCHAR strängar är trunkerat toomaximum längd på 4 000 Unicode-tecken
- SMALLDECIMAL stöds inte
- VARBINARY stöds inte
- Giltiga datum är mellan 1899, 12, 30 och 9999-12-31

## <a name="map-source-toosink-columns"></a>Mappa källkolumner toosink
toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Upprepbar läsning från relationella källor
När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat. I Azure Data Factory, kan du köra en sektor manuellt. Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår. När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs. Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.
