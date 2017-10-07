---
title: "aaaMove data från SAP Business Warehouse med hjälp av Azure Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från SAP Business Warehouse med hjälp av Azure Data Factory."
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
ms.date: 05/16/2017
ms.author: jingwang
ms.openlocfilehash: 85df16f4759a846f578cad301e3cf918179143d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a>Flytta data från SAP Business Warehouse med hjälp av Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal SAP Business Warehouse (BW). Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.

Du kan kopiera data från en lokal SAP Business Warehouse data store tooany stöds sink dataarkiv. En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell. Data factory stöder för närvarande endast flytta data från en SAP Business Warehouse tooother data lagras, men inte för att flytta data från andra data lagras tooan SAP Business Warehouse. 

## <a name="supported-versions-and-installation"></a>Versioner som stöds och installation
Den här anslutningen har stöd för SAP Business Warehouse version 7.x. Det stöder kopiering av data från InfoCubes och QueryCubes (inklusive BEx frågor) med hjälp av MDX-frågor.

tooenable hello anslutningen toohello SAP BW-instans, installera hello följande komponenter:
- **Data Management Gateway**: Data Factory-tjänsten stöder anslutningar tooon lokala data Arkiv (inklusive SAP Business Warehouse) med hjälp av en komponent som kallas Data Management Gateway. toolearn om Data Management Gateway och stegvisa instruktioner för hur du konfigurerar hello gateway finns [flytta data mellan lokala data lagra toocloud datalagret](data-factory-move-data-between-onprem-and-cloud.md) artikel. Gateway krävs även om hello SAP Business Warehouse finns i en Azure IaaS-virtuella (VM). Du kan installera hello gateway på hello samma virtuella dator som hello data lagras eller på en annan virtuell dator så länge som hello gateway kan ansluta toohello databas.
- **SAP NetWeaver biblioteket** på hello gateway-datorn. Du kan hämta hello SAP Netweaver bibliotek från SAP-administratören eller direkt från hello [SAP Software Download Center](https://support.sap.com/swdc). Sök efter hello **SAP Obs #1025361** tooget hello hämtningsplats för hello senaste versionen. Kontrollera att hello arkitektur för hello SAP NetWeaver biblioteket (32-bitars eller 64-bitars) matchar din gateway-installation. Installera alla filer som ingår i hello SAP NetWeaver RFC SDK bl.a toohello SAP-kommentar. hello SAP NetWeaver biblioteket ingår också i hello SAP-klientverktyg installation.

> [!TIP]
> Placera hello DLL-filer extraheras från hello NetWeaver RFC SDK i mappen system32.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er. 

- hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data. 
- Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. 
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. 

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från en lokal SAP Business Warehouse finns [JSON-exempel: kopiera data från SAP Business Warehouse tooAzure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) i den här artikeln. 

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooan SAP BW datalager:

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello följande tabell ger en beskrivning för JSON-element specifika tooSAP Business Warehouse (BW) länkade tjänsten.

Egenskap | Beskrivning | Tillåtna värden | Krävs
-------- | ----------- | -------------- | --------
server | Namnet på hello-server på vilken hello SAP BW instansen finns. | Sträng | Ja
systemNumber | System antal hello SAP BW system. | Två siffror decimaltal representeras som en sträng. | Ja
clientId | Klient-ID för hello klienten i hello SAP W system. | Tre siffror decimaltal representeras som en sträng. | Ja
användarnamn | Namnet på hello-användare som har åtkomst toohello SAP-server | Sträng | Ja
lösenord | Lösenordet för hello. | Sträng | Ja
gatewayName | Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokal SAP BW instans. | Sträng | Ja
encryptedCredential | hello krypterade autentiseringsuppgifter strängen. | Sträng | Nej

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).

Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Det finns inga typspecifika egenskaper som stöds för hello SAP BW dataset av typen **RelationalTable**. 


## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper, till exempel namn, beskrivning, inkommande och utgående tabeller är principer är tillgängliga för alla typer av aktiviteter.

Medan egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

När datakällan i en Kopieringsaktivitet är av typen **RelationalSource** (som innehåller SAP BW), hello följande egenskaper finns i avsnittet typeProperties:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB | Anger hello MDX-fråga tooread data från hello SAP BW-instans. | MDX-fråga. | Ja |


## <a name="json-example-copy-data-from-sap-business-warehouse-tooazure-blob"></a>JSON-exempel: kopiera data från SAP Business Warehouse tooAzure Blob
hello följande exempel innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Det här exemplet visas hur toocopy data från en lokal SAP Business Warehouse tooan Azure Blob Storage. Dock datan kan kopieras **direkt** tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.  

> [!IMPORTANT]
> Det här exemplet innehåller JSON kodavsnitt. Stegvisa instruktioner för att skapa datafabriken hello inkluderas inte. Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.

hello exemplet har hello följande data factory-enheter:

1. En länkad tjänst av typen [SapBw](#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar data från en SAP Business Warehouse-instans tooan Azure blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

Som ett första steg bör du konfigurera hello data management gateway. hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.

### <a name="sap-business-warehouse-linked-service"></a>SAP Business Warehouse länkade tjänsten
Den här länkade tjänsten länkar din SAP BW instans toohello data factory. hello typegenskapen har ställts in för**SapBw**. Hej typeProperties avsnittet innehåller anslutningsinformation för hello SAP BW-instans. 

```json
{
    "name": "SapBwLinkedService",
    "properties":
    {
        "type": "SapBw",
        "typeProperties":
        {
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

### <a name="sap-bw-input-dataset"></a>SAP BW inkommande dataset
Den här datauppsättningen definierar hello SAP Business Warehouse dataset. Du ställer in hello på hello Data Factory datamängd för**RelationalTable**. För närvarande kan anger du inte några typspecifika egenskaper för en SAP BW datauppsättning. hello frågan i hello kopiera aktivitetsdefinitionen anger vilka data tooread från hello SAP BW-instans. 

Ange externa egenskapen tootrue informerar hello Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

Frekvensen och intervall egenskaper definierar hello schema. I det här fallet läses hello data från hello SAP BW instans varje timme. 

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



### <a name="azure-blob-output-dataset"></a>Utdatauppsättning för Azure-blobb
Den här datauppsättningen definierar hello utdata Azure-blobbdatauppsättning. hello typegenskapen anges tooAzureBlob. Hej typeProperties avsnittet innehåller hello data kopieras från hello SAP BW instans ska lagras. hello data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sapbw/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** (för SAP BW källa) och **sink** typ har angetts för**BlobSink**. hello-fråga som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
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
                "inputs": [
                    {
                        "name": "SapBwDataset"
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
                "name": "SapBwToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```



### <a name="type-mapping-for-sap-bw"></a>Mappning för SAP BW
Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande två sätt:

1. Konvertera från inbyggda typer too.NET källtypen
2. Konvertera från .NET typen toonative Mottagartypen

När du flyttar data från SAP BW används hello följande mappningar från SAP BW typer too.NET typer.

Datatypen i hello ABAP ordlista | .NET-datatyp
-------------------------------- | --------------
ACCP |  int
CHAR | Sträng
CLNT | Sträng
AKTUELLT DATUM | Decimal
CUKY | Sträng
DEC | Decimal
FLTP | dubbla
INT1 | Mottagna byte
INT2 | Int16
INT4 | int
LANG | Sträng
LCHR | Sträng
LRAW | byte]
PREC | Int16
QUAN | Decimal
RÅDATA | byte]
RAWSTRING | byte]
STRÄNG | Sträng
ENHET | Sträng
DATS | Sträng
NUMC | Sträng
TIMS | Sträng

> [!NOTE]
> toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).


## <a name="map-source-toosink-columns"></a>Mappa källkolumner toosink
toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Upprepbar läsning från relationella källor
När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat. I Azure Data Factory, kan du köra en sektor manuellt. Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår. När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs. Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.
