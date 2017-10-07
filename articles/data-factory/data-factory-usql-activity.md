---
title: "aaaTransform data med hjälp av U-SQL - skript i Azure | Microsoft Docs"
description: "Lär dig hur compute tooprocess eller Transformera data genom att köra U-SQL-skript på Azure Data Lake Analytics-tjänsten."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e17c1255-62c2-4e2e-bb60-d25274903e80
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: 51fdb40334d0c131720f65c3a96b4c5045a98b24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Transformera data genom att köra U-SQL-skript på Azure Data Lake Analytics 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-aktivitet](data-factory-hive-activity.md) 
> * [Pig-aktivitet](data-factory-pig-activity.md)
> * [MapReduce Activity](data-factory-map-reduce.md)
> * [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md)
> * [Spark-aktivitet](data-factory-spark.md)
> * [Machine Learning Batch-körningsaktivitet](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning-uppdateringsresursaktivitet](data-factory-azure-ml-update-resource-activity.md)
> * [Lagrad proceduraktivitet](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-aktivitet](data-factory-usql-activity.md)
> * [Anpassad aktivitet för .NET](data-factory-use-custom-activities.md)

En pipeline i ett Azure data factory bearbetar data i länkade storage-tjänster med hjälp av länkade beräknings-tjänster. Den innehåller en serie aktiviteter där varje aktivitet utför en specifik bearbetning. Den här artikeln beskriver hello **Data Lake Analytics U-SQL-aktivitet** som kör en **U-SQL** skript på en **Azure Data Lake Analytics** compute länkade tjänsten. 

> [!NOTE]
> Skapa ett Azure Data Lake Analytics-konto innan du skapar en pipeline med en Data Lake Analytics U-SQL-aktivitet. toolearn om Azure Data Lake Analytics finns [Kom igång med Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
> 
> Granska hello [skapa din första pipeline-självstudierna](data-factory-build-your-first-pipeline.md) för detaljerade anvisningar toocreate en datafabrik länkade tjänster, datauppsättningar och en pipeline. Använd JSON kodavsnitt hos Data Factory-redigeraren eller Visual Studio eller Azure PowerShell toocreate Data Factory-enheter.

## <a name="supported-authentication-types"></a>Typer av autentiseringsmetoder som stöds
U-SQL-aktiviteten stöder nedan autentiseringstyper mot Data Lake Analytics:
* Autentisering av tjänstens huvudnamn
* Användarautentisering autentiseringsuppgifter (OAuth) 

Vi rekommenderar att du använder service principal autentisering, särskilt för en schemalagd U-SQL-körningen. Giltighetstid för token kan inträffa med autentiseringsuppgifter för användarautentisering. Konfigurationsinformation finns hello [länkade tjänstegenskaper](#azure-data-lake-analytics-linked-service) avsnitt.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics länkade tjänsten
Du skapar en **Azure Data Lake Analytics** länkade tjänsten toolink ett Azure Data Lake Analytics beräkning service tooan Azure data factory. hello Data Lake Analytics U-SQL-aktivitet i pipelinen hello refererar toothis länkade tjänsten. 

hello innehåller följande tabell beskrivningar hello allmänna egenskaper som används i hello JSON-definitionen. Du kan ytterligare välja mellan tjänstens huvudnamn och autentiseringsuppgifter för användarautentisering.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| **typ** |hello typegenskapen ska anges till: **AzureDataLakeAnalytics**. |Ja |
| **Kontonamn** |Azure Data Lake Analytics-kontonamn. |Ja |
| **dataLakeAnalyticsUri** |Azure Data Lake Analytics-URI. |Nej |
| **prenumerations-ID** |Azure prenumerations-id |Nej (om inte anges prenumeration hello datafabriken används). |
| **resourceGroupName** |Azure resursgruppens namn |Nej (om inte anges resursgruppen av hello datafabriken används). |

### <a name="service-principal-authentication-recommended"></a>Tjänstens huvudnamn autentisering (rekommenderas)
toouse service principal autentisering, registrera en Programenhet i Azure Active Directory (Azure AD) och bevilja den hello komma åt tooData Lake Store. Detaljerade anvisningar finns i [tjänst-till-tjänst autentisering](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Anteckna hello följande värden som du använder toodefine hello länkade tjänsten:
* Program-ID:t
* Nyckeln för programmet 
* Klient-ID:t

Använd service principal autentisering genom att ange hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| **servicePrincipalId** | Ange hello programmets klient-ID. | Ja |
| **servicePrincipalKey** | Ange hello programnyckel. | Ja |
| **klient** | Ange hello klient information (domain name eller klient ID) under där programmet finns. Du kan hämta den med hovra hello musen i hello övre högra hörnet av hello Azure-portalen. | Ja |

**Exempel: Service principal autentisering**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Användarautentisering för autentiseringsuppgifter
Alternativt kan du använda användarautentisering för autentiseringsuppgifter för Data Lake Analytics genom att ange hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| **auktorisering** | Klicka på hello **auktorisera** i hello Data Factory-redigeraren och ange dina autentiseringsuppgifter som tilldelar hello automatiskt genererade auktorisering URL toothis-egenskapen. | Ja |
| **sessions-ID** | OAuth sessions-ID från hello OAuth-auktorisering session. Varje sessions-ID är unikt och kan bara användas en gång. Den här inställningen genereras automatiskt när du använder hello Data Factory-redigeraren. | Ja |

**Exempel: Användarautentisering för autentiseringsuppgifter**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Token upphör att gälla
Hej auktoriseringskod som du genererade med hjälp av hello **auktorisera** knappen upphör att gälla efter en stund. Se hello i den följande tabellen för hello upphör att gälla tidpunkter för olika typer av användarkonton. Kan du se hello följande felmeddelande när hello autentisering **token upphör att gälla**: autentiseringsuppgifter fel: invalid_grant - AADSTS70002: fel vid verifiering av autentiseringsuppgifter. AADSTS70008: hello angetts åtkomsten har upphört att gälla eller återkallats. Trace-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelations-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tidsstämpel: 2015-12-15 21:09:31Z

| Användartyp | Upphör att gälla efter |
|:--- |:--- |
| Användarkonton som inte hanteras av Azure Active Directory (@hotmail.com, @live.comosv.) |12 timmar |
| Användarkonton som hanteras av Azure Active Directory (AAD) |14 dagar efter hello sista segmentet kör. <br/><br/>90 dagar, om ett segment baserat på OAuth-baserad länkade tjänst körs på minst en gång var fjortonde dag. |

tooavoid/Lös det här felet, omauktorisera med hello **auktorisera** knappen när hello **token upphör att gälla** och distribuera hello länkade tjänsten. Du kan också generera värdena för **sessionId** och **auktorisering** egenskaper programmässigt med hjälp av koden på följande sätt:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

Se [AzureDataLakeStoreLinkedService klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), och [AuthorizationSessionGetResponse klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) avsnitt hittar du information Om hello Data Factory-klasser används i hello-koden. Lägg till en referens till: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll för hello WindowsFormsWebAuthenticationDialog klass. 

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL-aktivitet
hello följande JSON-fragment definierar en pipeline med en Data Lake Analytics U-SQL-aktivitet. hello aktivitetsdefinitionen har en referens toohello länkade Azure Data Lake Analytics-tjänsten som du skapade tidigare.   

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This is a pipeline toocompute events for en-gb locale and date less than 2012/02/19.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00Z",
        "end": "2015-08-08T01:00:00Z",
        "isPaused": false
    }
}
```

hello följande tabell beskrivs namn och beskrivningar av egenskaper som är specifika toothis aktivitet. 

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ |hello Typegenskapen måste anges för**DataLakeAnalyticsU SQL**. |Ja |
| ScriptPath |Sökvägen toofolder som innehåller hello U-SQL-skriptet. Namnet på hello-filen är skiftlägeskänslig. |Nej (om du använder skriptet) |
| scriptLinkedService |Länkade tjänst som länkar hello lagring som innehåller hello skriptet toohello data factory |Nej (om du använder skriptet) |
| Skriptet |Ange infogat skript i stället för att ange scriptPath och scriptLinkedService. Till exempel: `"script": "CREATE DATABASE test"`. |Nej (om du använder scriptPath och scriptLinkedService) |
| degreeOfParallelism |hello maximalt antal noder används samtidigt toorun hello jobb. |Nej |
| Prioritet |Anger vilka jobb av alla köas ska vara markerade toorun först. hello lägre hello nummer, hello högre hello prioritet. |Nej |
| parameters |Parametrar för hello U-SQL-skript |Nej |
| runtimeVersion | Runtime-versionen av hello U-SQL-motorn toouse | Nej | 
| compilationMode | <p>Kompileringsläge för U-SQL. Måste vara ett av följande värden:</p> <ul><li>**Semantiska:** endast utföra semantiska kontroller och nödvändiga hälsokontroller.</li><li>**Fullständig:** utföra hello fullständig kompilering, inklusive syntaxkontrollen, optimering, kodgenerering osv.</li><li>**SingleBox:** utföra hello fullständig kompilering med TargetType inställningen tooSingleBox.</li></ul><p>Om du inte anger ett värde för den här egenskapen anger hello server hello optimala kompilering. </p>| Nej | 

Se [SearchLogProcessing.txt skript Definition](#sample-u-sql-script) hello skript definition. 

## <a name="sample-input-and-output-datasets"></a>Exempel på indata och utdata datauppsättningar
### <a name="input-dataset"></a>Indatauppsättning
I det här exemplet hello inkommande data som finns i ett Azure Data Lake Store (SearchLog.tsv-fil i mappen för hello datalake-indata). 

```json
{
    "name": "DataLakeTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}    
```

### <a name="output-dataset"></a>Datamängd för utdata
I det här exemplet lagras hello-utdata som genereras av hello U-SQL-skript i ett Azure Data Lake Store (datalake-/ utdata mapp). 

```json
{
    "name": "EventsByRegionTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="sample-data-lake-store-linked-service"></a>Exempel Data Lake Store länkade tjänsten
Här är hello definition av hello exempel Azure Data Lake Store länkade tjänst som används av hello in-/ utdata-datauppsättningar. 

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
        }
    }
}
```

Se [flytta data tooand från Azure Data Lake Store](data-factory-azure-datalake-connector.md) artikel beskrivningar av JSON-egenskaper. 

## <a name="sample-u-sql-script"></a>Exempel U-SQL-skript

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    too@out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

Hej värden för  **@in**  och  **@out**  parametrar i hello U-SQL-skriptet skickas dynamiskt av ADF med hello-parametrar'. Avsnittet hello-parametrar' i hello pipeline-definition.

Du kan ange andra egenskaper, till exempel degreeOfParallelism och prioritet samt i pipeline-definitionen för hello jobb som körs på hello Azure Data Lake Analytics-tjänsten.

## <a name="dynamic-parameters"></a>Dynamiska parametrar
I hello exempel pipeline definition tilldelas in och ut parametrar med hårdkodade värden. 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

Det är möjligt toouse dynamiska parametrar i stället. Exempel: 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

I det här fallet indatafiler fortfarande tas upp från hello /datalake/input mapp och utdatafiler skapas i hello /datalake/output mapp. hello filnamn är dynamiska baserat på hello sektorn starttid.  

