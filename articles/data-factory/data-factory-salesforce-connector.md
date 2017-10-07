---
title: "aaaMove data från Salesforce med hjälp av Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från Salesforce med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: dbe3bfd6-fa6a-491a-9638-3a9a10d396d1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: c1bde2a333f5a3c0a995eb8c13ecf585132888b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a>Flytta data från Salesforce med hjälp av Azure Data Factory
Den här artikeln beskrivs hur du kan använda Kopieringsaktiviteten i ett Azure data factory toocopy data från datalagret för Salesforce tooany som visas under hello Sink-kolumn i hello [källor och sänkor stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell. Den här artikeln bygger på hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning stöds data store kombinationer och Kopieringsaktivitet.

Azure Data Factory stöder för närvarande endast flytta data från Salesforce för[stöds sink datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats), men stöder inte flytta data från andra data lagras tooSalesforce.

## <a name="supported-versions"></a>Versioner som stöds
Den här anslutningen har stöd för följande versioner av Salesforce hello: Developer Edition, Professional Edition, Enterprise Edition eller obegränsade Edition. Och kopiera från Salesforce produktion, sandbox och anpassade domäner.

## <a name="prerequisites"></a>Krav
* API-behörighet måste aktiveras. Se [hur aktivera API-åtkomst i Salesforce av behörighetsgrupp?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)
* toocopy data från Salesforce tooon lokala datalager, du måste ha minst Data Management Gateway 2.0 är installerat i din lokala miljö.

## <a name="salesforce-request-limits"></a>Salesforce begärandebegränsningar
Salesforce har begränsningar för både förfrågningarna API och samtidiga API-begäranden. Observera följande punkter hello:

- Om hello antalet samtidiga begäranden överskrider gränsen hello, begränsning inträffar och slumpmässiga fel visas.
- Om hello Totalt antal begäranden överskrider gränsen hello, blockeras hello Salesforce-konto i 24 timmar.

Du kan också få hello ”REQUEST_LIMIT_EXCEEDED” fel i båda scenarierna. I avsnittet hello ”API Förfrågningsgränser” i hello [Salesforce Developer gränser](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) artikeln för information.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från Salesforce med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret: 

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. 
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. 

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från Salesforce finns [JSON-exempel: kopiera data från Salesforce tooAzure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) i den här artikeln. 

hello följande avsnitt innehåller information om JSON-egenskaper som är specifika tooSalesforce för används toodefine Data Factory-enheter: 

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello i den följande tabellen finns beskrivningar JSON-element som är specifika toohello Salesforce länkade tjänsten.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello Typegenskapen måste anges till: **Salesforce**. |Ja |
| environmentUrl | Ange hello URL Salesforce-instans. <br><br> – Standardvärdet är ”https://login.salesforce.com”. <br> -toocopy data från sandbox, ange ”https://test.salesforce.com”. <br> -toocopy data från domän, till exempel ange ”https://[domain].my.salesforce.com”. |Nej |
| användarnamn |Ange ett användarnamn för hello användarkonto. |Ja |
| lösenord |Ange ett lösenord för hello användarkonto. |Ja |
| securityToken |Ange en säkerhetstoken för hello användarkonto. Se [hämta säkerhetstoken](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) anvisningar för hur tooreset/hämta en säkerhetstoken. i allmänhet finns i toolearn om säkerhetstoken [säkerhet och hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Ja |

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen (Azure SQL Azure blob, Azure-tabellen och så vidare).

Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej typeProperties avsnittet för en dataset av hello typen **RelationalTable** har hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i Salesforce. |Nej (om en **frågan** av **RelationalSource** har angetts) |

> [!IMPORTANT]
> Hej ”__c” tillhör hello API namn krävs för alla anpassade objekt.

![Namnet på data Factory - anslutning Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, indata och utdata tabeller och olika principer är tillgängliga för alla typer av aktiviteter.

hello-egenskaper som är tillgängliga i hello typeProperties avsnittet hello aktivitet på hello andra sidan, varierar med varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

I en Kopieringsaktivitet när hello källan är av typen hello **RelationalSource** (som omfattar Salesforce), hello följande egenskaper finns i avsnittet typeProperties:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |En SQL-92-fråga eller [Salesforce objektet Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) frågan. Till exempel: `select * from MyTable__c`. |Nej (om hello **tableName** av hello **dataset** har angetts) |

> [!IMPORTANT]
> Hej ”__c” tillhör hello API namn krävs för alla anpassade objekt.

![Namnet på data Factory - anslutning Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a>Frågan tips
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>Hämtning av data med hjälp av where-sats för kolumnen datum/tid
Ange när hello SOQL eller SQL fråga, pay kontrolleras toohello skillnaden för DateTime-format. Exempel:

* **SOQL exempel**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`
* **SQL-exempel**:
    * **Med hjälp av kopiera guiden toospecify hello fråga:**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`
    * **Med JSON redigerar toospecify hello fråga (escape-tecken korrekt):**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`

### <a name="retrieving-data-from-salesforce-report"></a>Hämtar data från Salesforce-rapport
Du kan hämta data från Salesforce-rapporter genom att ange frågan som `{call "<report name>"}`, till exempel. `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Hämta borttagna poster från Salesforce-Papperskorgen
tooquery hello ej permanent borttagna poster från Salesforce-Papperskorgen, kan du ange **”IsDeleted = 1”** i frågan. Exempel:

* Ange tooquery endast hello bort poster ”Välj * från MyTable__c **där IsDeleted = 1**”
* tooquery alla hello poster som hello befintliga och hello bort, ange ”Välj * från MyTable__c **där IsDeleted = 0 eller IsDeleted = 1**”

## <a name="json-example-copy-data-from-salesforce-tooazure-blob"></a>JSON-exempel: kopiera data från Salesforce tooAzure Blob
hello följande exempel innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av hello [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De visar hur toocopy data från Salesforce tooAzure Blob Storage. Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.   

Här följer hello Data Factory-artefakter behöver toocreate tooimplement hello scenario. hello avsnitten som följer hello lista innehåller information om de här stegen.

* En länkad tjänst av typen hello [Salesforce](#linked-service-properties)
* En länkad tjänst av typen hello [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Indata [dataset](data-factory-create-datasets.md) av hello typen [RelationalTable](#dataset-properties)
* Utdata [dataset](data-factory-create-datasets.md) av hello typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

**Salesforce länkad tjänst**

Det här exemplet används hello **Salesforce** länkade tjänsten. Se hello [Salesforce länkade tjänsten](#linked-service-properties) avsnittet för hello egenskaper som stöds av den här länkade tjänsten.  Se [hämta säkerhetstoken](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) anvisningar för hur tooreset/hämta hello säkerhetstoken.

```json
{
    "name": "SalesforceLinkedService",
    "properties":
    {
        "type": "Salesforce",
        "typeProperties":
        {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```
**Länkad Azure Storage-tjänst**

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
**Inkommande Salesforce-dataset**

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

Ange **externa** för**SANT** informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

> [!IMPORTANT]
> Hej ”__c” tillhör hello API namn krävs för alla anpassade objekt.

![Namnet på data Factory - anslutning Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

**Utdatauppsättning för Azure-blobb**

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
            "folderPath": "adfgetstarted/alltypes_c"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Rörledningen med Kopieringsaktiviteten**

hello pipeline innehåller Kopieringsaktiviteten som är konfigurerad för toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource**, och hello **sink** typ har angetts för**BlobSink**.

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
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce tooan Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "SalesforceInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
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
        }
        ]
    }
}
```
> [!IMPORTANT]
> Hej ”__c” tillhör hello API namn krävs för alla anpassade objekt.

![Namnet på data Factory - anslutning Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a>Mappning för Salesforce
| Salesforce-typ | . NET-baserade typ |
| --- | --- |
| Automatisk tal |Sträng |
| Kryssruta |Booleskt värde |
| Valuta |dubbla |
| Date |Datum och tid |
| Datum/tid |Datum och tid |
| E-post |Sträng |
| Id |Sträng |
| Uppslagsrelation |Sträng |
| Flerval listruta |Sträng |
| Tal |dubbla |
| Procent |dubbla |
| Telefon |Sträng |
| Listruta |Sträng |
| Text |Sträng |
| Textområde |Sträng |
| Textområde (Long) |Sträng |
| Textområde (omfattande) |Sträng |
| Text (krypterade) |Sträng |
| URL: EN |Sträng |

> [!NOTE]
> toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Prestanda- och justering
Se hello [prestandajustering guide och Kopieringsaktivitet prestanda](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.
