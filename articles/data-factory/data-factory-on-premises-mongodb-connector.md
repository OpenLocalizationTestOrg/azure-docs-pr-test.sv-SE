---
title: "aaaMove data från MongoDB med hjälp av Data Factory | Microsoft Docs"
description: "Läs mer om hur toomove data från MongoDB-databas med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 10ca7d9a-7715-4446-bf59-2d2876584550
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: 154e85712f27b978976c7499c43dde9429f124c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Flytta data från MongoDB med hjälp av Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal MongoDB-databas. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.

Du kan kopiera data från ett lokalt MongoDB data store tooany stöds sink dataarkiv. En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell. Data factory stöder för närvarande endast flytta data från en MongoDB lagra tooother datalager, men inte för att flytta data från andra data lagras tooan MongoDB-datalagret. 

## <a name="prerequisites"></a>Krav
Hello Azure Data Factory-tjänsten toobe kan tooconnect tooyour lokalt MongoDB-databas måste du installera hello följande komponenter:

- MongoDB-versioner som stöds är: 2.4, 2.6, 3.0 och 3.2.
- Data Management Gateway på hello samma dator värdar hello databasen eller på en separat dator tooavoid konkurrerar om resurser med hello-databasen. Data Management Gateway är en programvara som ansluter lokala data sources toocloud tjänster på en säker och hanterad sätt. Se [Data Management Gateway](data-factory-data-management-gateway.md) artikeln för information om Data Management Gateway. Se [flytta data från lokala toocloud](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner om hur du konfigurerar hello gateway data pipeline toomove data.

    När du installerar hello gateway installeras automatiskt en Microsoft MongoDB ODBC-drivrutinen används tooconnect tooMongoDB.

    > [!NOTE]
    > Du måste toouse hello gateway tooconnect tooMongoDB även om den finns i Azure IaaS-VM. Du kan också installera hello gateway-instans i hello IaaS VM om du försöker tooconnect tooan instans av MongoDB som finns i molnet.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal MongoDB-datalagret med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret: 

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. 
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. 

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett lokalt MongoDB-dataarkiv finns [JSON-exempel: kopiera data från MongoDB tooAzure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) i den här artikeln. 

hello följande avsnitt innehåller information om JSON-egenskaper som används toodefine Data Factory entiteter specifika tooMongoDB källan:

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello följande tabell innehåller beskrivning för JSON-element som är specifika för**OnPremisesMongoDB** länkade tjänsten.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello Typegenskapen måste anges till: **OnPremisesMongoDb** |Ja |
| server |IP-adressen eller värdnamnet namnet på hello MongoDB-servern. |Ja |
| port |TCP-port som hello MongoDB-servern använder toolisten för klientanslutningar. |Valfritt, standardvärdet: 27017 |
| AuthenticationType |Grundläggande eller anonym. |Ja |
| användarnamn |Användarens konto tooaccess MongoDB. |Ja (om grundläggande autentisering används). |
| lösenord |Lösenordet för hello. |Ja (om grundläggande autentisering används). |
| authSource |Namnet på hello MongoDB-databas som du vill toouse toocheck dina autentiseringsuppgifter för autentisering. |Valfritt (om grundläggande autentisering används). standard: använder hello administratörskonto och hello databas som har angetts med egenskapen databaseName. |
| DatabaseName |Namnet på hello MongoDB-databas som du vill tooaccess. |Ja |
| gatewayName |Namnet på hello-gateway som har åtkomst till hello-datalagret. |Ja |
| encryptedCredential |Autentiseringsuppgifter har krypterats av gateway. |Valfri |

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).

Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej typeProperties avsnittet för dataset av typen **MongoDbCollection** har hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| Samlingsnamn |Namnet på hello-samlingen i MongoDB-databas. |Ja |

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.

Egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet på hello andra sidan varierar med varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

När hello källan är av typen **MongoDbSource** hello följande egenskaper finns i avsnittet typeProperties:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-92 frågesträngen. Till exempel: Välj * från mytable prefix. |Nej (om **samlingsnamn** av **dataset** har angetts) |



## <a name="json-example-copy-data-from-mongodb-tooazure-blob"></a>JSON-exempel: kopiera data från MongoDB tooAzure Blob
Det här exemplet innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Den visar hur toocopy data från en lokal MongoDB tooan Azure Blob Storage. Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.

hello exemplet har hello följande data factory-enheter:

1. En länkad tjänst av typen [OnPremisesMongoDb](#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [MongoDbCollection](#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [MongoDbSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar data från ett frågeresultat i MongoDB databasen tooa blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

Konfigurera som ett första steg hello data management gateway enligt hello instruktionerna i hello [Data Management Gateway](data-factory-data-management-gateway.md) artikel.

**MongoDB länkade tjänsten:**

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties":
    {
        "type": "OnPremisesMongoDb",
        "typeProperties":
        {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< hello IP address or host name of hello MongoDB server >",  
            "port": "<hello number of hello TCP port that hello MongoDB server uses toolisten for client connections.>",
            "username": "<username>",
            "password": "<password>",
           "authSource": "< hello database that you want toouse toocheck your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<mygateway>"
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

**MongoDB inkommande dataset:** inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```json
{
     "name":  "MongoDbInputDataset",
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

**Azure Blob utdatauppsättningen:**

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kopiera aktivitet i en pipeline med MongoDB källa och mottagare för Blob:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello ovan inkommande och utgående datauppsättningar och schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**MongoDbSource** och **sink** typ har angetts för**BlobSink**. hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "MongoDbSource",
                        "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "MongoDbInputDataset"
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
                "name": "MongoDBToAzureBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```


## <a name="schema-by-data-factory"></a>Schema som Data Factory
Azure Data Factory-tjänsten skapar schema från en MongoDB-samling med hjälp av hello senaste 100 dokument i hello samling. Om dokumenten 100 inte innehåller fullständig schemat, att vissa kolumner ignoreras under hello kopieringen.

## <a name="type-mapping-for-mongodb"></a>Mappning för MongoDB
Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:

1. Konvertera från inbyggda typer too.NET källtypen
2. Konvertera från .NET typen toonative Mottagartypen

När du flyttar data tooMongoDB hello efter används mappningar från MongoDB typer too.NET typer.

| MongoDB-typ | .NET framework-typ |
| --- | --- |
| Binär |byte] |
| Booleskt värde |Booleskt värde |
| Date |Datum och tid |
| NumberDouble |dubbla |
| NumberInt |Int32 |
| NumberLong |Int64 |
| Objekt-ID |Sträng |
| Sträng |Sträng |
| UUID |GUID |
| Objekt |Renormalized förenkla i kolumner med ”_” som kapslad avgränsare |

> [!NOTE]
> toolearn om stöd för matriser med virtuella tabeller finns för[stöd för komplexa typer som använder virtuella tabeller](#support-for-complex-types-using-virtual-tables) nedan.

För närvarande hello följande MongoDB-datatyper stöds inte: DBPointer, JavaScript, Max per minut nyckel, reguljärt uttryck, symboler, Timestamp, Undefined

## <a name="support-for-complex-types-using-virtual-tables"></a>Stöd för komplexa typer som använder virtuella tabeller
Azure Data Factory använder en inbyggd ODBC-drivrutinen tooconnect tooand kopieringsdata från MongoDB-databas. För komplexa typer som matriser eller objekt med olika typer i hello dokument normaliserar hello drivrutinen igen data i motsvarande virtuella tabeller. I synnerhet om en tabell innehåller sådana kolumner, hello-drivrutinen genererar hello följande virtuella tabeller:

* En **bastabellen**, som innehåller hello samma data som hello verkliga tabell utom hello komplex typkolumner. hello bastabellen använder hello samma namn som hello verkliga tabell som representerar.
* En **virtuella tabellen** för varje kolumn för komplex typ, som utökar hello kapslade data. hello virtuella tabeller namnges med hello namnet på hello verkliga tabell, avgränsare ”_” och hello namnet på hello matris eller ett objekt.

Virtuella tabeller finns toohello data i hello verkliga tabell, aktiverar hello drivrutinen tooaccess hello Avnormaliserade data. Se avsnittet nedan information. Du kan komma åt hello innehållet i MongoDB matriser genom att fråga och hello virtuella tabeller.

Du kan använda hello [guiden Kopiera](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively hello listan över tabeller i databasen MongoDB, inklusive hello virtuella tabeller och förhandsgranska hello data i. Du kan också skapa en fråga i hello guiden Kopiera och validera toosee hello resultatet.

### <a name="example"></a>Exempel
Till exempel är ”ExampleTable” nedan en MongoDB-tabell som har en kolumn med en matris med objekt i varje cell – fakturor och en kolumn med en matris med skalära typer – klassificeringar.

| _id | Kundens namn | Fakturor | Servicenivå | Klassificering |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{invoice_id: ”123” objektet: ”toaster”, pris: ”456” rabatt: ”0,2”}, {invoice_id: ”124” objektet: ”vara”, pris: rabatt ”1235”: ”0,2”}] |Silver |[5,6] |
| 2222 |XYZ |[{invoice_id: objektet ”135”: ”kombinerad kyl”, pris: ”12543” rabatt: ”0,0”}] |Guld |[1,2] |

hello drivrutinen skulle generera flera virtuella tabeller toorepresent denna tabell. hello första virtuella tabellen är hello bastabellen med namnet ”ExampleTable” nedan. hello bastabellen innehåller alla hello data för hello ursprungliga tabellen, men hello data från hello matriser har utelämnats och utökas i hello virtuella register.

| _id | Kundens namn | Servicenivå |
| --- | --- | --- |
| 1111 |ABC |Silver |
| 2222 |XYZ |Guld |

hello följande tabeller visar hello virtuella tabeller som representerar hello ursprungliga matriser i hello exempel. Dessa tabeller innehåller hello följande:

* En referens tillbaka toohello ursprungliga primärnyckelkolumnen motsvarande toohello rad hello ursprungliga matrisen (via hello _id kolumn)
* Uppgift om hello placeringen av hello inom hello ursprungliga matris
* hello expanderas data för varje element i matrisen hello

Tabell ”ExampleTable_Invoices”:

| _id | ExampleTable_Invoices_dim1_idx | invoice_id | Objektet | price | Rabatt |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |Toaster |456 |0.2 |
| 1111 |1 |124 |vara |1235 |0.2 |
| 2222 |0 |135 |kombinerad kyl |12543 |0.0 |

Tabell ”ExampleTable_Ratings”:

| _id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1 |6 |
| 2222 |0 |1 |
| 2222 |1 |2 |

## <a name="map-source-toosink-columns"></a>Mappa källkolumner toosink
toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Upprepbar läsning från relationella källor
När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat. I Azure Data Factory, kan du köra en sektor manuellt. Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår. När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs. Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.

## <a name="next-steps"></a>Nästa steg
Se [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner för att skapa en pipeline för data som flyttas från ett lokalt datalager tooan Azure datalagret.
