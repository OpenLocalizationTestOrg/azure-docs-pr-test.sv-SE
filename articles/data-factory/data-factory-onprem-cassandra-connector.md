---
title: "aaaMove data från Cassandra med hjälp av Data Factory | Microsoft Docs"
description: "Läs mer om hur toomove data från en lokal Cassandra databasen med Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 085cc312-42ca-4f43-aa35-535b35a102d5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: 0e265d3a8439d0a2cb2a5c32e5ea8348a1617621
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Flytta data från en lokal Cassandra-databas med Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal Cassandra databas. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.

Du kan kopiera data från ett lokalt Cassandra data store tooany stöds sink dataarkiv. En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell. Data factory stöder för närvarande endast flytta data från en Cassandra lagra tooother datalager, men inte för att flytta data från andra lagrar tooa Cassandra data dataarkiv. 

## <a name="supported-versions"></a>Versioner som stöds
Hej Cassandra connector stöder följande versioner av Cassandra hello: 2.X.

## <a name="prerequisites"></a>Krav
Hello Azure Data Factory-tjänsten toobe kan tooconnect tooyour lokalt Cassandra-databas måste du installera en Data Management Gateway på hello datorn samma värdar hello databasen eller på en separat dator tooavoid konkurrerar om resurser med hello databas. Data Management Gateway är en komponent som ansluter lokala data sources toocloud tjänster i en säker och hanterad sätt. Se [Data Management Gateway](data-factory-data-management-gateway.md) artikeln för information om Data Management Gateway. Se [flytta data från lokala toocloud](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner om hur du konfigurerar hello gateway data pipeline toomove data.

Du måste använda hello gateway tooconnect tooa Cassandra databasen även om hello-databasen finns i hello molnet, till exempel på en Azure IaaS-VM. Y kan innehålla hello gateway hello samma Virtuella värdar hello databasen eller på en separat virtuell dator så länge som hello gateway kan ansluta toohello databas.  

När du installerar hello gateway installeras automatiskt en Microsoft Cassandra ODBC-drivrutinen används tooconnect tooCassandra databas. Därför behöver du inte toomanually installera en drivrutin på hello gateway-datorn när du kopierar data från hello Cassandra databas. 

> [!NOTE]
> Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er. 

- hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data. 
- Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. 
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. 

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett lokalt Cassandra dataarkiv finns [JSON-exempel: kopiera data från Cassandra tooAzure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) i den här artikeln. 

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooa Cassandra datalager:

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello följande tabell innehåller en beskrivning för JSON-element specifika tooCassandra länkad tjänst.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello Typegenskapen måste anges till: **OnPremisesCassandra** |Ja |
| värden |En eller flera IP-adresser eller värdnamn Cassandra servrar.<br/><br/>Ange en kommaavgränsad lista med IP-adresser eller tooall värdservrar namn tooconnect samtidigt. |Ja |
| port |hello TCP-port som hello Cassandra server använder toolisten för klientanslutningar. |Nej, standardvärde: 9042 |
| AuthenticationType |Grundläggande eller anonym |Ja |
| användarnamn |Ange användarnamn för hello användarkonto. |Ja, om authenticationType anges tooBasic. |
| lösenord |Ange lösenordet för användarkontot för hello. |Ja, om authenticationType anges tooBasic. |
| gatewayName |hello namnet på hello-gateway som har använt tooconnect toohello lokalt Cassandra databas. |Ja |
| encryptedCredential |Autentiseringsuppgifter har krypterats av hello gateway. |Nej |

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).

Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej typeProperties avsnittet för dataset av typen **CassandraTable** har hello följande egenskaper

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| keyspace |Namnet på hello keyspace eller schema i Cassandra databasen. |Ja (om **frågan** för **CassandraSource** har inte definierats). |
| tableName |Namnet på hello tabell i Cassandra databas. |Ja (om **frågan** för **CassandraSource** har inte definierats). |

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.

De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

När datakällan är av typen **CassandraSource**, hello följande egenskaper finns i avsnittet typeProperties:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-92 frågan eller CQL frågan. Se [CQL referens](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>När du använder SQL-frågan anger **keyspace name.table namn** toorepresent hello tabell tooquery. |Nej (om tabellnamn och keyspace för datauppsättningen har definierats). |
| consistencyLevel |hello konsekvensnivå anger hur många repliker måste svara tooa läsbegäran innan det returneras data toohello klientprogrammet. Cassandra kontrollerar hello angivet antal repliker för data toosatisfy hello läsa begäran. |EN, TVÅ, TRE, KVORUM, ALL, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Se [konfigurera datakonsekvens](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) mer information. |Nej. Standardvärdet är en. |

## <a name="json-example-copy-data-from-cassandra-tooazure-blob"></a>JSON-exempel: kopiera data från Cassandra tooAzure Blob
Det här exemplet innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Den visar hur toocopy data från en lokal Cassandra databasen tooan Azure Blob Storage. Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.

> [!IMPORTANT]
> Det här exemplet innehåller JSON kodavsnitt. Stegvisa instruktioner för att skapa datafabriken hello inkluderas inte. Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.

hello exemplet har hello följande data factory-enheter:

* En länkad tjänst av typen [OnPremisesCassandra](#linked-service-properties).
* En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Indata [dataset](data-factory-create-datasets.md) av typen [CassandraTable](#dataset-properties).
* Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [CassandraSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

**Cassandra länkade tjänsten:**

Det här exemplet används hello **Cassandra** länkade tjänsten. Se [Cassandra länkade tjänsten](#linked-service-properties) avsnittet hello egenskaper som stöds av den här länkade tjänsten.  

```json
{
    "name": "CassandraLinkedService",
    "properties":
    {
        "type": "OnPremisesCassandra",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "host": "mycassandraserver",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "mygateway"
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

**Cassandra inkommande datauppsättningen:**

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "mykeyspace"
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

Ange **externa** för**SANT** informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

**Azure Blob utdatauppsättningen:**

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/fromcassandra"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Kopiera aktivitet i en pipeline med Cassandra källa och mottagare för Blob:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**CassandraSource** och **sink** typ har angetts för**BlobSink**.

Se [RelationalSource Typegenskaper](#copy-activity-properties) hello lista över egenskaper som stöds av hello RelationalSource.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra tooan Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "CassandraInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
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
        }
        ]    
    }
}
```

### <a name="type-mapping-for-cassandra"></a>Mappning för Cassandra
| Cassandra typ | .NET baserat typ |
| --- | --- |
| ASCII |Sträng |
| BIGINT |Int64 |
| BLOB |byte] |
| BOOLESKT VÄRDE |Booleskt värde |
| DECIMAL |Decimal |
| DUBBEL |dubbla |
| FLYTTAL |Enskild |
| INET |Sträng |
| INT |Int32 |
| TEXT |Sträng |
| TIDSSTÄMPEL |Datum och tid |
| TIMEUUID |GUID |
| UUID |GUID |
| VARCHAR |Sträng |
| VARINT |Decimal |

> [!NOTE]
> Insamling av typer (karta, set, lista, etc.) finns för[arbeta med Cassandra samlingstyper med virtuella tabellen](#work-with-collections-using-virtual-table) avsnitt.
>
> Användardefinierade typer stöds inte.
>
> hello längden av binär kolumn och strängkolumn längd får inte vara större än 4000.
>
>

## <a name="work-with-collections-using-virtual-table"></a>Arbeta med samlingar med virtuella tabellen
Azure Data Factory använder en inbyggd ODBC-drivrutinen tooconnect tooand kopieringsdata från databasen Cassandra. För samlingstyper inklusive karta, uppsättning och lista, renormalizes hello drivrutinen hello data i motsvarande virtuella tabeller. I synnerhet om en tabell innehåller några kolumner i samlingen, hello-drivrutinen genererar hello följande virtuella tabeller:

* En **bastabellen**, som innehåller hello samma data som hello verkliga tabell utom hello samling kolumner. hello bastabellen använder hello samma namn som hello verkliga tabell som representerar.
* En **virtuella tabellen** för varje samling-kolumn som utökar hello kapslade data. hello virtuella tabeller som representerar samlingar namnges med hello namnet på hello verkliga tabell, avgränsare ”*vt*” och hello hello kolumnens namn.

Virtuella tabeller finns toohello data i hello verkliga tabell, aktiverar hello drivrutinen tooaccess hello Avnormaliserade data. Se avsnittet för information. Du kan komma åt hello innehållet i Cassandra samlingar genom att fråga och hello virtuella tabeller.

Du kan använda hello [guiden Kopiera](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively hello listan över tabeller i Cassandra databasen, inklusive hello virtuella tabeller och förhandsgranska hello data i. Du kan också skapa en fråga i hello guiden Kopiera och validera toosee hello resultatet.

### <a name="example"></a>Exempel
Till exempel är hello följande ”ExampleTable” en Cassandra databastabell som innehåller ett heltal primärnyckelkolumnen med namnet ”pk_int”, en textkolumn namngivet värde, en kolumn, en karta kolumn och en set-kolumn (med namnet ”StringSet”).

| pk_int | Värde | Visa lista | karta | StringSet |
| --- | --- | --- | --- | --- |
| 1 |”exempelvärde 1” |["1", "2", "3"] |{”S1”: ”a”, ”S2”: ”b”} |{”A”, ”B”, ”C”} |
| 3 |”exempelvärde 3” |["100", "101", "102", "105"] |{”S1”: ”t”} |{”A”, ”E”} |

hello drivrutinen skulle generera flera virtuella tabeller toorepresent denna tabell. hello sekundärnyckelskolumnerna i hello virtuella tabeller referera hello primärnyckelkolumnerna i hello verkliga tabell och ange vilka verkligt tabellrad raden hello virtuella tabellen motsvarar.

hello första virtuella tabellen är hello bastabellen med namnet ”ExampleTable” visas i följande tabell hello. hello bastabellen innehåller hello samma data som hello ursprungliga databastabell förutom hello samlingar, som utelämnas från den här tabellen och expanderas i andra virtuella tabeller.

| pk_int | Värde |
| --- | --- |
| 1 |”exempelvärde 1” |
| 3 |”exempelvärde 3” |

hello visar följande tabeller hello virtuella register som renormalize hello data från hello lista och mappa StringSet kolumner. hello kolumner med namn som slutar med ”_index” eller ”_nyckelskyddsserverns” ange hello placeringen av hello inom hello ursprungliga listan eller karta. hello kolumnerna med namn som slutar med ”_value” innehåller hello expanderas data från hello samling.

#### <a name="table-exampletablevtlist"></a>Tabell ”ExampleTable_vt_List”:
| pk_int | List_index | List_value |
| --- | --- | --- |
| 1 |0 |1 |
| 1 |1 |2 |
| 1 |2 |3 |
| 3 |0 |100 |
| 3 |1 |101 |
| 3 |2 |102 |
| 3 |3 |103 |

#### <a name="table-exampletablevtmap"></a>Tabell ”ExampleTable_vt_Map”:
| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1 |S1 |A |
| 1 |S2 |B |
| 3 |S1 |T |

#### <a name="table-exampletablevtstringset"></a>Tabell ”ExampleTable_vt_StringSet”:
| pk_int | StringSet_value |
| --- | --- |
| 1 |A |
| 1 |B |
| 1 |C |
| 3 |A |
| 3 |E |

## <a name="map-source-toosink-columns"></a>Mappa källkolumner toosink
toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Upprepbar läsning från relationella källor
När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat. I Azure Data Factory, kan du köra en sektor manuellt. Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår. När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs. Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.
