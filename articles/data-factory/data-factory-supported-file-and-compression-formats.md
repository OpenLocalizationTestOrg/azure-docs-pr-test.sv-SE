---
title: komprimering och aaaFile format i Azure Data Factory | Microsoft Docs
description: "Läs mer om hello-filformat som stöds av Azure Data Factory."
keywords: BLOB-data, azure blob-kopia
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 9d40517b059fc533776bcc088db8c531ee5b003d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="file-and-compression-formats-supported-by-azure-data-factory"></a>Fil- och komprimering format som stöds av Azure Data Factory
*Det här avsnittet gäller toohello följande kopplingar: [Amazon S3](data-factory-amazon-simple-storage-service-connector.md), [Azure Blob](data-factory-azure-blob-connector.md), [Azure Data Lake Store](data-factory-azure-datalake-connector.md), [filsystemet](data-factory-onprem-file-system-connector.md), [ FTP](data-factory-ftp-connector.md), [HDFS](data-factory-hdfs-connector.md), [HTTP](data-factory-http-connector.md), och [SFTP](data-factory-sftp-connector.md).*

Azure Data Factory stöder hello följande filtyper format:

* [Textformat](#text-format)
* [JSON-format](#json-format)
* [Avro-formatet](#avro-format)
* [ORC-format](#orc-format)
* [Parkettgolv format](#parquet-format)

## <a name="text-format"></a>Textformat
Om du vill tooread från en textfil eller skriva tooa textfil, ange hello `type` egenskap i hello `format` avsnitt i hello datamängden för**TextFormat**. Du kan också ange hello följande **valfria** egenskaper i hello `format` avsnitt. Se [TextFormat exempel](#textformat-example) avsnittet om hur tooconfigure.

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| columnDelimiter |hello tecknet används tooseparate kolumner i en fil. Kan du toouse en sällsynt icke utskrivbara tecken som kanske inte är troligt finns i dina data. Till exempel ange ”\u0001” som representerar Start av rubriken (SOH). |Endast ett tecken är tillåtet. Hej **standard** värdet är **kommatecken (',')**. <br/><br/>toouse Unicode-tecken finns för[Unicode-tecken](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget hello motsvarande kod för den. |Nej |
| rowDelimiter |hello tecknet används tooseparate rader i en fil. |Endast ett tecken är tillåtet. Hej **standard** värdet är något av följande värden på Läs hello: **[”\r\n”, ”\r”, ”\n”]** och **”\r\n”** vid skrivning. |Nej |
| escapeChar |hello specialtecken används tooescape avgränsare för en kolumn i hello innehållet i indatafilen. <br/><br/>Du kan inte ange både escapeChar och quoteChar för en tabell. |Endast ett tecken är tillåtet. Inget standardvärde. <br/><br/>Exempel: Om du har kommatecken (', ') som hello kolumnen avgränsare, men du vill att toohave hello kommatecken tecknet i texten hello (exempel: ”Hello, world”), kan du definiera '$' som hello escape-tecken och använda strängen ”$Hello, world” i hello källan. |Nej |
| quoteChar |hello tecknet används tooquote ett strängvärde. hello kolumn- och avgränsare i hello citattecken skulle behandlas som en del av hello strängvärde. Den här egenskapen är tillämpliga tooboth indata och utdata datauppsättningar.<br/><br/>Du kan inte ange både escapeChar och quoteChar för en tabell. |Endast ett tecken är tillåtet. Inget standardvärde. <br/><br/>Till exempel om du har kommatecken (', ') hello kolumnen avgränsare, men du vill toohave kommatecken tecknet i texten hello (exempel: < Hello, world >), kan du definiera ”(dubbla citattecken) som hello citattecken och Använd hello sträng” Hello, world ”i hello källan. |Nej |
| nullValue |Ett eller flera tecken används toorepresent ett null-värde. |Ett eller flera tecken. Hej **standard** värden är **”\N” och ”NULL”** vid läsning och **”\N”** vid skrivning. |Nej |
| encodingName |Ange hello kodningsnamn. |Ett giltigt kodningsnamn. Se [Egenskapen Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx). Exempel: windows-1250 or shift_jis. Hej **standard** värdet är **UTF-8**. |Nej |
| firstRowAsHeader |Anger om tooconsider hello första raden som rubrik. För en indatauppsättning läser Data Factory den första raden som en rubrik. För en utdatauppsättning skriver Data Factory den första raden som en rubrik. <br/><br/>Exempelscenarier finns i avsnittet med [användningsscenarier för `firstRowAsHeader` och `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount). |True<br/><b>False (standard)</b> |Nej |
| skipLineCount |Anger hello antal rader tooskip när data lästes från indatafiler. Om både skipLineCount och firstRowAsHeader anges, hello rader hoppas över först och sedan hello rubrikinformation läses från hello indatafilen. <br/><br/>Exempelscenarier finns i avsnittet med [användningsscenarier för `firstRowAsHeader` och `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount). |Integer |Nej |
| treatEmptyAsNull |Anger om tootreat null eller en tom sträng som en null-värde när data lästes från en indatafil. |**True (standard)**<br/>False |Nej |

### <a name="textformat-example"></a>TextFormat-exempel
I följande JSON-definitionen för en dataset hello, har vissa hello valfria egenskaper angetts.

```json
"typeProperties":
{
    "folderPath": "mycontainer/myfolder",
    "fileName": "myblobname",
    "format":
    {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";",
        "quoteChar": "\"",
        "NullValue": "NaN",
        "firstRowAsHeader": true,
        "skipLineCount": 0,
        "treatEmptyAsNull": true
    }
},
```

toouse en `escapeChar` i stället för `quoteChar`, Ersätt hello raden med `quoteChar` med hello följande escapeChar:

```json
"escapeChar": "$",
```

### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Användningsscenarier för firstRowAsHeader och skipLineCount
* Du kopierar från en icke-källa tooa textfil och vill tooadd en huvudrad som innehåller hello schemat metadata (till exempel: SQL-schemat). Ange `firstRowAsHeader` som true i hello utdatauppsättningen för det här scenariot.
* Du kopierar från en textfil som innehåller ett huvud rad tooa-fil som tar emot och vill toodrop rad. Ange `firstRowAsHeader` som true i hello inkommande dataset.
* Du kopierar från en textfil och vill tooskip några rader hello början som innehåller ingen information för data eller huvudet. Ange `skipLineCount` tooindicate hello antalet rader toobe hoppas över. Om hello resten av hello-filen innehåller en rubrikrad, du kan också ange `firstRowAsHeader`. Om båda `skipLineCount` och `firstRowAsHeader` anges hello rader hoppas över först och sedan hello rubrikinformation läses från hello indatafilen

## <a name="json-format"></a>JSON-format
för**importera och exportera en JSON-fil som-är i/från Azure Cosmos DB**, hello Se [Import/export JSON-dokument](data-factory-azure-documentdb-connector.md#importexport-json-documents) i avsnittet [flytta data till/från Azure Cosmos DB](data-factory-azure-documentdb-connector.md) artikel.

Om du vill tooparse hello JSON-filer eller skriva hello data i JSON-format, ange hello `type` egenskap i hello `format` avsnittet för**JsonFormat**. Du kan också ange hello följande **valfria** egenskaper i hello `format` avsnitt. Se [JsonFormat exempel](#jsonformat-example) avsnittet om hur tooconfigure.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| filePattern |Ange hello mönstret för data som lagras i varje JSON-fil. Tillåtna värden är: **setOfObjects** och **arrayOfObjects**. Hej **standard** värdet är **setOfObjects**. Detaljerad information om dessa mönster finns i avsnittet om [JSON-filmönster](#json-file-patterns). |Nej |
| jsonNodeReference | Om du vill tooiterate och extrahera data från hello-objekt i en matris fältet med hello samma mönster, ange hello JSON-sökvägen i matrisen. Den här egenskapen stöds endast när du kopierar data från JSON-filer. | Nej |
| jsonPathDefinition | Ange hello JSON sökvägsuttryck för varje kolumnmappning med ett anpassat kolumnnamn (start med gemener). Den här egenskapen stöds endast när du kopierar data från JSON-filer, och du kan extrahera data från objekt eller matriser. <br/><br/> Börja med $ root; för fält under rotobjektet för fält i hello matris som valts av `jsonNodeReference` start från hello matriselement-egenskapen. Se [JsonFormat exempel](#jsonformat-example) avsnittet om hur tooconfigure. | Nej |
| encodingName |Ange hello kodningsnamn. Hello lista över giltiga kodning namn, se: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) egenskapen. Exempel: windows-1250 or shift_jis. Hej **standard** värde är: **UTF-8**. |Nej |
| nestingSeparator |Tecken som används tooseparate kapslingsnivåer. hello standardvärdet är '.' (punkt). |Nej |

### <a name="json-file-patterns"></a>JSON-filmönster

Kopieringsaktiviteten kan parsa hello efter mönster av JSON-filer:

- **Typ I: setOfObjects**

    Varje fil som innehåller ett enskilt objekt eller flera radavgränsade/sammanfogade objekt. När det här alternativet väljs i en utdatauppsättning genererar kopieringsaktiviteten en enskild JSON-fil med ett objekt per rad (radavgränsade).

    * **Exempel på JSON med enskilda objekt**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * **Exempel med radavgränsad JSON**

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * **Exempel med sammanfogad JSON**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- **Typ II: arrayOfObjects**

    Varje fil innehåller en matris med objekt.

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

### <a name="jsonformat-example"></a>JsonFormat-exempel

**Fall 1: Kopiera data från JSON-filer**

Se hello följande två samplingarna när du kopierar data från JSON-filer. hello generiska pekar toonote:

**Exempel 1: hämta data från objektet och matrisen**

I det här exemplet du förväntar dig en JSON-rotobjektet mappar toosingle post i tabellform resultat. Om du har en JSON-fil med hello följande innehåll:  

```json
{
    "id": "ed0e4960-d9c5-11e6-85dc-d7996816aad3",
    "context": {
        "device": {
            "type": "PC"
        },
        "custom": {
            "dimensions": [
                {
                    "TargetResourceType": "Microsoft.Compute/virtualMachines"
                },
                {
                    "ResourceManagmentProcessRunId": "827f8aaa-ab72-437c-ba48-d8917a7336a3"
                },
                {
                    "OccurrenceTime": "1/13/2017 11:24:37 AM"
                }
            ]
        }
    }
}
```
och du vill toocopy den till en Azure SQL-tabellen i hello följande format genom att extrahera data från både objekt och matrisen:

| id | deviceType | targetResourceType | resourceManagmentProcessRunId | occurrenceTime |
| --- | --- | --- | --- | --- |
| ed0e4960-d9c5-11e6-85dc-d7996816aad3 | PC | Microsoft.Compute/virtualMachines | 827f8aaa-ab72-437c-ba48-d8917a7336a3 | 1/13/2017 11:24:37 AM |

hello inkommande datamängd med **JsonFormat** typ definieras enligt följande: (partiell definition med endast hello relevanta delar). Mer specifikt:

- `structure`avsnittet definierar hello anpassat kolumnnamn och hello motsvarande datatyp vid konvertering av tootabular data. Det här avsnittet är **valfria** om du inte behöver toodo kolumnmappningen. Se [mappa dataset kolumner toodestination dataset källkolumner](data-factory-map-columns.md) mer information.
- `jsonPathDefinition`Anger hello JSON-sökvägen för varje kolumn som anger om tooextract hello data från. toocopy data från en matris, som du kan använda **matris [x] .property** tooextract värdet för hello anges egenskapen från hello x objekt eller du kan använda  **matris [*] .property** toofind hello-värde från vilket objekt som innehåller sådan egenskap.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "deviceType",
            "type": "String"
        },
        {
            "name": "targetResourceType",
            "type": "String"
        },
        {
            "name": "resourceManagmentProcessRunId",
            "type": "String"
        },
        {
            "name": "occurrenceTime",
            "type": "DateTime"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonPathDefinition": {"id": "$.id", "deviceType": "$.context.device.type", "targetResourceType": "$.context.custom.dimensions[0].TargetResourceType", "resourceManagmentProcessRunId": "$.context.custom.dimensions[1].ResourceManagmentProcessRunId", "occurrenceTime": " $.context.custom.dimensions[2].OccurrenceTime"}      
        }
    }
}
```

**Exempel 2: mellan tillämpa flera objekt med samma mönster från en matris hello**

I det här exemplet du förväntar dig tootransform en JSON-objekt i roten till flera poster i tabellform resultat. Om du har en JSON-fil med hello följande innehåll:  

```json
{
    "ordernumber": "01",
    "orderdate": "20170122",
    "orderlines": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "sanmateo": "No 1" } ]
}
```
och du vill toocopy den till en Azure SQL-tabellen i hello följande format genom att förenkla hello data i hello matris och korskoppling med hello vanliga rot-information:

| ordernumber | orderdate | order_pd | order_price | city |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | [{"sanmateo":"No 1"}] |
| 01 | 20170122 | P2 | 13 | [{"sanmateo":"No 1"}] |
| 01 | 20170122 | P3 | 231 | [{"sanmateo":"No 1"}] |

hello inkommande datamängd med **JsonFormat** typ definieras enligt följande: (partiell definition med endast hello relevanta delar). Mer specifikt:

- `structure`avsnittet definierar hello anpassat kolumnnamn och hello motsvarande datatyp vid konvertering av tootabular data. Det här avsnittet är **valfria** om du inte behöver toodo kolumnmappningen. Se [mappa dataset kolumner toodestination dataset källkolumner](data-factory-map-columns.md) mer information.
- `jsonNodeReference`Anger tooiterate och extrahera data från hello-objekt med samma mönster under hello **matris** orderlines.
- `jsonPathDefinition`Anger hello JSON-sökvägen för varje kolumn som anger om tooextract hello data från. I det här exemplet är ”ordernumber”, ”orderdate” och ”ort” under rotobjektet med JSON-sökvägen som börjar med ”$.”, medan ”order_pd” och ”order_price” definieras med sökvägen som härletts från hello matriselement utan ”$”.

```json
"properties": {
    "structure": [
        {
            "name": "ordernumber",
            "type": "String"
        },
        {
            "name": "orderdate",
            "type": "String"
        },
        {
            "name": "order_pd",
            "type": "String"
        },
        {
            "name": "order_price",
            "type": "Int64"
        },
        {
            "name": "city",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonNodeReference": "$.orderlines",
            "jsonPathDefinition": {"ordernumber": "$.ordernumber", "orderdate": "$.orderdate", "order_pd": "prod", "order_price": "price", "city": " $.city"}         
        }
    }
}
```

**Observera följande punkter hello:**

* Om hello `structure` och `jsonPathDefinition` har inte definierats i hello Data Factory dataset hello Kopieringsaktiviteten identifierar hello schema från första hello-objektet och förenkla hello hela objektet.
* Om hello JSON-indata har en matris, som standard konverterar hello Kopieringsaktiviteten hello hela Matrisvärde till en sträng. Du kan välja tooextract data från den med hjälp av `jsonNodeReference` och/eller `jsonPathDefinition`, eller hoppa över det genom att inte ange det i `jsonPathDefinition`.
* Om det finns duplicerade namn på Hej samma nivå, hello Kopieringsaktiviteten hämtar hello senast.
* Egenskapsnamn är skiftlägeskänsliga. Två egenskaper med samma namn men med olika skiftlägen behandlas som två olika egenskaper.

**Fall 2: Skriver data tooJSON-fil**

Om du har hello följande tabell i SQL-databas:

| id | order_date | order_price | order_by |
| --- | --- | --- | --- |
| 1 | 20170119 | 2000 | David |
| 2 | 20170120 | 3500 | Patrick |
| 3 | 20170121 | 4000 | Jason |

och för varje post du förväntar dig toowrite tooa JSON-objekt i hello följande format:
```json
{
    "id": "1",
    "order": {
        "date": "20170119",
        "price": 2000,
        "customer": "David"
    }
}
```

Hej utdatauppsättningen med **JsonFormat** typ definieras enligt följande: (partiell definition med endast hello relevanta delar). Mer specifikt `structure` avsnittet definierar hello anpassade egenskapsnamn i målfilen och `nestingSeparator` (standardvärdet är ””.) är används tooidentify hello kapsla lagret från hello namn. Det här avsnittet är **valfria** om du vill jämföra med källkolumnsnamnet toochange hello egenskapens namn eller kapsla några av hello egenskaper.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "order.date",
            "type": "String"
        },
        {
            "name": "order.price",
            "type": "Int64"
        },
        {
            "name": "order.customer",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat"
        }
    }
}
```

## <a name="avro-format"></a>AVRO-formatet
Om du vill tooparse hello Avro-filernas eller skriva hello data i Avro-formatet, ange hello `format` `type` egenskapen för**AvroFormat**. Du behöver inte toospecify alla egenskaper i avsnittet för hello-Format i hello typeProperties avsnitt. Exempel:

```json
"format":
{
    "type": "AvroFormat",
}
```

toouse Avro-formatet i en Hive-tabell, kan du läsa för[Apache Hive kursen](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Observera följande punkter hello:  

* [Komplexa datatyper](http://avro.apache.org/docs/current/spec.html#schema_complex) stöds inte (registrerar uppräkningar, matriser, kartor, unioner, och fast).

## <a name="orc-format"></a>ORC-format
Om du vill tooparse hello ORC-filer eller skriva hello data i ORC-format, ange hello `format` `type` egenskapen för**OrcFormat**. Du behöver inte toospecify alla egenskaper i avsnittet för hello-Format i hello typeProperties avsnitt. Exempel:

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> Om du inte kopierar ORC-filer **som-är** mellan lokala och moln datalager, du behöver tooinstall hello 8 JRE (Java Runtime Environment) på din gateway-datorn. En 64-bitars gateway kräver 64-bitars JRE och en 32-bitars gateway kräver 32-bitars JRE. Du hittar båda versionerna [här](http://go.microsoft.com/fwlink/?LinkId=808605). Välja hello lämplig.
>
>

Observera följande punkter hello:

* Komplexa datatyper stöds inte (STRUCT, MAP, LIST, UNION)
* ORC-filen har tre [komprimeringsrelaterade alternativ](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB och SNAPPY. Data Factory stöder läsning av data från ORC-filer i alla dessa komprimerade format. Den använder hello komprimering codec finns i hello metadata tooread hello data. Men när du skriver tooan ORC filen väljer Data Factory ZLIB, som är hello standard för ORC. För närvarande finns ingen alternativet toooverride detta beteende.

## <a name="parquet-format"></a>Parkettgolv format
Om du vill tooparse hello parkettgolv filer eller skriva hello data i parkettgolv format, ange hello `format` `type` egenskapen för**ParquetFormat**. Du behöver inte toospecify alla egenskaper i avsnittet för hello-Format i hello typeProperties avsnitt. Exempel:

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> Om du inte kopierar filer parkettgolv **som-är** mellan lokala och moln datalager, du behöver tooinstall hello 8 JRE (Java Runtime Environment) på din gateway-datorn. En 64-bitars gateway kräver 64-bitars JRE och en 32-bitars gateway kräver 32-bitars JRE. Du hittar båda versionerna [här](http://go.microsoft.com/fwlink/?LinkId=808605). Välja hello lämplig.
>
>

Observera följande punkter hello:

* Komplexa datatyper stöds inte (MAP, LIST)
* Parkettgolv filen har följande alternativ för komprimering-relaterade hello: NONE, SNAPPY GZIP och LZO. Data Factory stöder läsning av data från ORC-filer i alla dessa komprimerade format. Hello komprimerings-codec används i hello metadata tooread hello data. Men när du skriver tooa parkettgolv filen väljer Data Factory SNAPPY, vilket är standard hello parkettgolv format. För närvarande finns ingen alternativet toooverride detta beteende.

## <a name="compression-support"></a>Komprimeringsstöd för
Bearbetning av stora datamängder kan det orsaka flaskhalsar i i/o och nätverk. Därför kan komprimerade data i appbutiker inte bara snabbare dataöverföring hello nätverket och sparar diskutrymme, men också sätta betydande prestandaförbättringar vid bearbetning av stordata. Komprimering stöds för närvarande för filbaserade datalager, till exempel Azure Blob- eller lokala filsystem.  

toospecify komprimering för en dataset används hello **komprimering** egenskap i hello dataset JSON som i följande exempel hello:   

```json
{  
    "name": "AzureBlobDataSet",  
    "properties": {  
        "availability": {  
            "frequency": "Day",  
              "interval": 1  
        },  
        "type": "AzureBlob",  
        "linkedServiceName": "StorageLinkedService",  
        "typeProperties": {  
            "fileName": "pagecounts.csv.gz",  
            "folderPath": "compression/file/",  
            "compression": {  
                "type": "GZip",  
                "level": "Optimal"  
            }  
        }  
    }  
}  
```

Anta att hello exempeldatamängd används som hello utdata från en kopieringsaktiviteten utdata med GZIP codec med optimala hello kopiera aktivitet komprimerar hello och sedan skriva hello komprimerade data i en fil med namnet pagecounts.csv.gz i hello Azure Blob Storage.

> [!NOTE]
> Inställningarna för komprimering stöds inte för data i hello **AvroFormat**, **OrcFormat**, eller **ParquetFormat**. Vid läsning av filer i dessa format, Data Factory identifierar och använder hello komprimerings-codec i hello metadata. När du skriver toofiles i dessa format väljer Data Factory hello standard komprimerings-codec för detta format. Till exempel ZLIB för OrcFormat och SNAPPY för ParquetFormat.   

Hej **komprimering** avsnitt har två egenskaper:  

* **Typ:** hello komprimerings-codec, vilket kan vara **GZIP**, **Deflate**, **BZIP2**, eller **ZipDeflate**.  
* **Nivå:** hello komprimeringsförhållandet, vilket kan vara **Optimal** eller **snabbast**.

  * **Snabbaste:** hello komprimeringsåtgärden ska slutföras så snabbt som möjligt, även om hello resulterande filen inte komprimeras optimalt.
  * **Optimal**: hello komprimeringsåtgärden ska optimalt komprimeras, även om hello åtgärden tar en längre tid toocomplete.

    Mer information finns i [komprimeringsnivå](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) avsnittet.

När du anger `compression` egenskap i en inkommande datauppsättning JSON hello-pipeline kan läsa komprimerade data från hello källa, och när du anger hello-egenskapen i en datamängd för utdata JSON hello Kopieringsaktivitet kan skriva komprimerade data toohello mål. Här följer några exempelscenarier:

* Läsa GZIP komprimerade data från en Azure blob, expandera den och skriva resultatet data tooan Azure SQL-databas. Du definierar hello inkommande Azure-blobbdatauppsättning med hello `compression` `type` JSON-egenskap som GZIP.
* Läsa data från en fil med oformaterad text från lokalt filsystem, komprimera formatet GZip och skriva hello komprimerade data tooan Azure blob. Du definierar ett Azure Blob-datamängd för utdata med hello `compression` `type` JSON-egenskap som GZip.
* Läs ZIP-filen från FTP-server, expandera den tooget hello filer i och mark filerna i Azure Data Lake Store. Du definierar en inkommande FTP-datamängd med hello `compression` `type` JSON-egenskap som ZipDeflate.
* Läsa in en GZIP-komprimerade data från en Azure blob, expandera den, komprimera den med hjälp av BZIP2 och skriva resultatet data tooan Azure blob. Du definierar hello inkommande Azure-blobbdatauppsättning med `compression` `type` ange tooGZIP och hello utdatauppsättningen med `compression` `type` ange tooBZIP2 i det här fallet.   


## <a name="next-steps"></a>Nästa steg
Se följande artiklar för filbaserade datakällor som stöds av Azure Data Factory hello:

- [Azure Blob Storage](data-factory-azure-blob-connector.md)
- [Azure Data Lake Store](data-factory-azure-datalake-connector.md)
- [FTP](data-factory-ftp-connector.md)
- [HDFS](data-factory-hdfs-connector.md)
- [Filsystem](data-factory-onprem-file-system-connector.md)
- [Amazon S3](data-factory-amazon-simple-storage-service-connector.md)
