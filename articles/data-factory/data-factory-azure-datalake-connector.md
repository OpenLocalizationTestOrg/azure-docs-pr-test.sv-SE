---
title: "aaaCopy data tooand från Azure Data Lake Store | Microsoft Docs"
description: "Lär dig hur toocopy data tooand från Data Lake Store med hjälp av Azure Data Factory"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 25b1ff3c-b2fd-48e5-b759-bb2112122e30
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jingwang
ms.openlocfilehash: 2e78f75f3821738332dacf70f6bf2c16f0136408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-data-lake-store-by-using-data-factory"></a>Kopiera data tooand från Data Lake Store med hjälp av Data Factory
Den här artikeln förklarar hur toouse Kopieringsaktiviteten i Azure Data Factory toomove data tooand från Azure Data Lake Store. Den bygger på hello [Data movement aktiviteter](data-factory-data-movement-activities.md) artikel, en översikt över dataflyttning med Kopieringsaktiviteten.

## <a name="supported-scenarios"></a>Scenarier som stöds
Du kan kopiera data **från Azure Data Lake Store** toohello följande datakällor:

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

Du kan kopiera data från hello följande datalager **tooAzure Datasjölager**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Skapa ett Data Lake Store-konto innan du skapar en pipeline med Kopieringsaktiviteten. Mer information finns i [Kom igång med Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="supported-authentication-types"></a>Typer av autentiseringsmetoder som stöds
hello Data Lake Store-anslutningen har stöd för dessa typer av autentisering:
* Autentisering av tjänstens huvudnamn
* Användarautentisering autentiseringsuppgifter (OAuth) 

Vi rekommenderar att du använder service principal autentisering, särskilt för en schemalagd datauppsättning kopia. Giltighetstid för token kan inträffa med autentiseringsuppgifter för användarautentisering. Konfigurationsinformation finns hello [länkade tjänstegenskaper](#linked-service-properties) avsnitt.

## <a name="get-started"></a>Kom igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till och från ett Azure Data Lake Store med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate pipeline toocopy data är toouse hello **guiden Kopiera**. En självstudiekurs om hur du skapar en pipeline med hjälp av hello guiden Kopiera finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md).

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:

1. Skapa en **datafabriken**. En datafabrik kan innehålla en eller flera pipelines. 
2. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory. Till exempel om du kopierar data från ett Azure Data Lake Store med Azure blob storage tooan skapa du två länkade tjänster toolink dina Azure storage-konto och Azure Data Lake store tooyour data factory. Länkad tjänstegenskaper som är specifika tooAzure Data Lake Store, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt. 
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello blob-behållaren och mappen som innehåller hello indata. Och du skapar en annan dataset toospecify hello mapp och filsökvägen i hello Data Lake store som innehåller hello data som kopieras från hello blob storage. Egenskaper för datamängd som är specifika tooAzure Data Lake Store, se [egenskaper för datamängd](#dataset-properties) avsnitt.
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. I hello-exemplet ovan, använder du BlobSource som en källa och AzureDataLakeStoreSink som en mottagare för hello kopieringsaktiviteten. På samma sätt om du vill kopiera från Azure Data Lake Store tooAzure Blob Storage, använder du AzureDataLakeStoreSource och BlobSink i hello kopieringsaktiviteten. Kopiera Aktivitetsegenskaper som är specifika tooAzure Data Lake Store, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt. Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager.  

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från ett Azure Data Lake Store finns [JSON-exempel](#json-examples-for-copying-data-to-and-from-data-lake-store) i den här artikeln.

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooData Lake Store.

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
En länkad tjänst länkar en data store tooa data factory. Du skapar en länkad tjänst av typen **AzureDataLakeStore** toolink din Data Lake Store data tooyour data factory. hello i den följande tabellen beskrivs JSON-element specifika tooData Datasjölager länkade tjänster. Du kan välja mellan tjänstens huvudnamn och autentiseringsuppgifter för användarautentisering.

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| **typ** | hello Typegenskapen måste anges för**AzureDataLakeStore**. | Ja |
| **dataLakeStoreUri** | Information om hello Azure Data Lake Store-konto. Den här informationen tar en hello följande format: `https://[accountname].azuredatalakestore.net/webhdfs/v1` eller `adl://[accountname].azuredatalakestore.net/`. | Ja |
| **prenumerations-ID** | Azure-prenumeration ID toowhich hello Data Lake Store-konto tillhör. | Krävs för sink |
| **resourceGroupName** | Azure-resurs grupp namnet toowhich hello Data Lake Store-konto tillhör. | Krävs för sink |

### <a name="service-principal-authentication-recommended"></a>Tjänstens huvudnamn autentisering (rekommenderas)
toouse service principal autentisering, registrera en Programenhet i Azure Active Directory (Azure AD) och bevilja den hello komma åt tooData Lake Store. Detaljerade anvisningar finns i [tjänst-till-tjänst autentisering](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Anteckna hello följande värden som du använder toodefine hello länkade tjänsten:
* Program-ID:t
* Nyckeln för programmet 
* Klient-ID:t

> [!IMPORTANT]
> Om du använder hello guiden Kopiera tooauthor data pipelines, se till att du ger hello tjänstens huvudnamn på minst **Reader** roll i åtkomstkontroll (identitets- och åtkomsthantering) för hello Data Lake Store-konto. Dessutom ge hello tjänstens huvudnamn minst **Läs + Execute** behörighet tooyour Data Lake Store rot (”/”) och dess underordnade. Annars kan du se hello-meddelande ”hello angivna autentiseringsuppgifterna är ogiltiga”.<br/><br/>
När du skapar eller uppdaterar ett huvudnamn för tjänsten i Azure AD, kan det ta några minuter för hello ändringar tootake effekt. Kontrollera hello tjänstens huvudnamn och Data Lake Store access control åtkomstkontrollistan (ACL) konfigurationer. Om du ser fortfarande hello-meddelande ”hello angivna autentiseringsuppgifterna är ogiltiga”, vänta en stund och försök igen.

Använd service principal autentisering genom att ange hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| **servicePrincipalId** | Ange hello programmets klient-ID. | Ja |
| **servicePrincipalKey** | Ange hello programnyckel. | Ja |
| **klient** | Ange hello klient information (domain name eller klient ID) under där programmet finns. Du kan hämta den med hovra hello musen i hello övre högra hörnet av hello Azure-portalen. | Ja |

**Exempel: Service principal autentisering**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Användarautentisering för autentiseringsuppgifter
Du kan också använda användarens autentiseringsuppgifter autentisering toocopy från eller tooData Datasjölager genom att ange hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| **auktorisering** | Klicka på hello **auktorisera** i hello Data Factory-redigeraren och ange dina autentiseringsuppgifter som tilldelar hello automatiskt genererade auktorisering URL toothis-egenskapen. | Ja |
| **sessions-ID** | OAuth sessions-ID från hello OAuth-auktorisering session. Varje sessions-ID är unikt och kan bara användas en gång. Den här inställningen genereras automatiskt när du använder hello Data Factory-redigeraren. | Ja |

**Exempel: Användarautentisering för autentiseringsuppgifter**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

#### <a name="token-expiration"></a>Token upphör att gälla
Hej auktoriseringskod som du skapar med hjälp av hello **auktorisera** knappen upphör att gälla efter en viss tid. hello efter meddelande innebär att hello-autentiseringstoken har upphört att gälla:

Autentiseringsuppgifter fel: invalid_grant - AADSTS70002: fel vid verifiering av autentiseringsuppgifter. AADSTS70008: hello angetts åtkomsten har upphört att gälla eller återkallats. Trace-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelations-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tidsstämpel: 2015-12-15 21-09-31Z.

hello visar följande tabell hello giltighetstid gånger med olika typer av användarkonton:


| Användartyp | Upphör att gälla efter |
|:--- |:--- |
| Användarkonton *inte* hanteras av Azure Active Directory (till exempel @hotmail.com eller @live.com) |12 timmar |
| Användarkonton som hanteras av Azure Active Directory |Kör 14 dagar efter hello sista segmentet <br/><br/>90 dagar, om ett segment baserat på en länkad OAuth-tjänst körs på minst en gång var fjortonde dag |

Om du ändrar ditt lösenord innan hello token förfallotid hello token upphör att gälla omedelbart. Hello-meddelande som nämnts tidigare i det här avsnittet visas.

Du kan omauktorisera hello-konto med hjälp av hello **auktorisera** knappen när hello-token upphör att gälla tooredeploy hello länkade tjänsten. Du kan också generera värdena för hello **sessionId** och **auktorisering** egenskaper programmatiskt genom att använda hello följande kod:


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
Mer information om hello Data Factory-klasser som används i koden hello finns hello [AzureDataLakeStoreLinkedService klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), och [ AuthorizationSessionGetResponse klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) avsnitt. Lägg till en referens tooversion `2.9.10826.1824` av `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` för hello `WindowsFormsWebAuthenticationDialog` klass som används i hello kod.

## <a name="dataset-properties"></a>Egenskaper för datamängd
toospecify en dataset toorepresent indata i ett Data Lake Store som du ställer in hello **typen** -egenskapen för hello datamängden för**AzureDataLakeStore**. Ange hello **linkedServiceName** länkad egenskap hello dataset toohello namnet på hello Data Lake Store-tjänsten. En fullständig lista över JSON-avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt i en dataset i JSON, t.ex **struktur**, **tillgänglighet**, och **princip**, är liknande för alla typer av datauppsättningen (Azure SQL-databasen, Azure blob och Azure-tabellen, till exempel). Hej **typeProperties** avsnittet skiljer sig för varje typ av datauppsättningen och innehåller information som plats och dataformat hello i hello datalagret. 

Hej **typeProperties** avsnittet för en dataset av typen **AzureDataLakeStore** innehåller hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| **folderPath** |Sökvägen toohello behållaren och mappen i Data Lake Store. |Ja |
| **Filnamn** |Namnet på hello-fil i Azure Data Lake Store. Hej **fileName** egenskapen är valfri och är skiftlägeskänsliga. <br/><br/>Om du anger **fileName**, hello aktivitet (inklusive kopia) fungerar på hello specifik fil.<br/><br/>När **fileName** anges kopia innehåller alla filer i **folderPath** i hello inkommande dataset.<br/><br/>När **fileName** har inte angetts för en datamängd för utdata och **preserveHierarchy** har inte angetts i aktiviteten sink hello hello genereras filen heter hello format Data. _GUID_.txt'. Till exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt. |Nej |
| **partitionedBy** |Hej **partitionedBy** egenskapen är valfri. Du kan använda den toospecify en dynamisk sökväg och filnamn för time series-data. Till exempel **folderPath** kan parameteriseras för varje timme av data. Mer information och exempel finns [hello partitionedBy egenskap](#using-partitionedby-property). |Nej |
| **format** | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, och  **ParquetFormat**. Ange hello **typen** egenskap under **format** tooone av dessa värden. Mer information finns i hello [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [JSON-format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format ](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt i hello [format och komprimering stöds av Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel. <br><br> Om du vill toocopy filer ”som-är” mellan filbaserade butiker (binär kopia), hoppar du över hello `format` avsnitt i både inkommande och utgående dataset-definitioner. |Nej |
| **komprimering** | Ange hello typ och kompression för hello data. Typer som stöds är **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**. Stöds nivåerna **Optimal** och **snabbast**. Mer information finns i [format och komprimering stöds av Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |

### <a name="hello-partitionedby-property"></a>Hej partitionedBy egenskap
Du kan ange dynamiska **folderPath** och **fileName** egenskaper för time series-data med hello **partitionedBy** egenskapen, Data Factory-funktioner och system variabler. Mer information finns i hello [Azure Data Factory - funktioner och systemvariabler](data-factory-functions-variables.md) artikel.


I följande exempel hello `{Slice}` ersätts med hello värdet för variabeln hello Data Factory `SliceStart` i hello format (`yyyyMMddHH`). hello namn `SliceStart` refererar toohello starttiden för hello sektorn. Hej `folderPath` egenskapen är olika för varje segment som i `wikidatagateway/wikisampledataout/2014100103` eller `wikidatagateway/wikisampledataout/2014100104`.

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

I följande exempel, hello år, månad, dag och tid för hello `SliceStart` extraheras till olika variabler som används av hello `folderPath` och `fileName` egenskaper:
```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
Mer information om tidsserier datauppsättningar schemaläggning och segment, se hello [datauppsättningar i Azure Data Factory](data-factory-create-datasets.md) och [Data Factory schemaläggning och körning av](data-factory-scheduling-and-execution.md) artiklar. 


## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.

Hej egenskaper som är tillgängliga i hello **typeProperties** avsnitt i en aktivitet varierar med varje aktivitetstyp. För en kopieringsaktiviteten varierar de beroende på hello typer av datakällor och sänkor.

**AzureDataLakeStoreSource** stöder hello efter egenskap i hello **typeProperties** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| **rekursiva** |Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen. |SANT (standardvärdet), FALSKT |Nej |


**AzureDataLakeStoreSink** stöder följande egenskaper i hello hello **typeProperties** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| **copyBehavior** |Anger hello kopiera beteende. |<b>PreserveHierarchy</b>: bevarar hello filen hierarki i hello målmappen. hello relativa sökvägen till källmappen för filen toosource är identiska toohello relativa sökvägen till filen tootarget mapp.<br/><br/><b>FlattenHierarchy</b>: alla filer från källmappen hello skapas i hello första nivån i hello målmappen. hello med filer skapas med automatiskt genererade namn.<br/><br/><b>MergeFiles</b>: sammanfogar alla filer från hello källfil mappen tooone. Om hello fil-eller blob anges är hello kopplade filnamnet hello angivet namn. Annars är hello filnamn automatiskt genererade. |Nej |

### <a name="recursive-and-copybehavior-examples"></a>rekursiva och copyBehavior exempel
Det här avsnittet beskrivs hello resultatet av hello kopia för olika kombinationer av värden rekursiv och copyBehavior.

| Rekursiva | copyBehavior | Resultatet |
| --- | --- | --- |
| SANT |preserveHierarchy |För en källmapp Mapp1 med hello följande struktur: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello målmappen Mapp1 skapas med hello samma struktur som hello källa<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| SANT |flattenHierarchy |För en källmapp Mapp1 med hello följande struktur: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello mål Mapp1 skapas med följande struktur hello: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File5 |
| SANT |mergeFiles |För en källmapp Mapp1 med hello följande struktur: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello mål Mapp1 skapas med följande struktur hello: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 + fil3 + File4 + filen 5 innehållet slås samman till en fil med automatiskt genererade namnet |
| FALSKT |preserveHierarchy |För en källmapp Mapp1 med hello följande struktur: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello målmappen Mapp1 skapas med följande struktur hello<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/><br/><br/>Subfolder1 med fil3, File4 och File5 har inte plockats. |
| FALSKT |flattenHierarchy |För en källmapp Mapp1 med hello följande struktur:<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello målmappen Mapp1 skapas med följande struktur hello<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2<br/><br/><br/>Subfolder1 med fil3, File4 och File5 har inte plockats. |
| FALSKT |mergeFiles |För en källmapp Mapp1 med hello följande struktur:<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello målmappen Mapp1 skapas med följande struktur hello<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 innehållet slås samman till en fil med automatiskt genererade namnet. automatiskt genererade namnet på File1<br/><br/>Subfolder1 med fil3, File4 och File5 har inte plockats. |

## <a name="supported-file-and-compression-formats"></a>Fil- och komprimering format som stöds
Mer information finns i hello [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel.

## <a name="json-examples-for-copying-data-tooand-from-data-lake-store"></a>JSON-exempel för att kopiera data tooand från Data Lake Store
följande exempel hello ger exempel JSON-definitioner. Du kan använda dessa exempel definitioner toocreate en pipeline med hjälp av hello [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Hej exempel visas hur toocopy data tooand från Data Lake Store och Azure Blob storage. Dock datan kan kopieras _direkt_ från någon av hello källor tooany av sänkor hello stöds. Mer information finns i avsnittet hello ”stöds datalager och format” i hello [flytta data med hjälp av Kopieringsaktiviteten](data-factory-data-movement-activities.md) artikel.  

### <a name="example-copy-data-from-azure-blob-storage-tooazure-data-lake-store"></a>Exempel: Kopiera data från Azure Blob Storage tooAzure Data Lake Store
hello exempelkod i det här avsnittet visas:

* En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* En länkad tjänst av typen [AzureDataLakeStore](#linked-service-properties).
* Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Utdata [dataset](data-factory-create-datasets.md) av typen [AzureDataLakeStore](#dataset-properties).
* En [pipeline](data-factory-create-pipelines.md) med en kopia-aktivitet som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [AzureDataLakeStoreSink](#copy-activity-properties).

hello exemplen visar hur tidsserier data från Azure Blob Storage är kopieras tooData Datasjölager varje timme. 

**Länkad Azure Storage-tjänst**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**Azure Data Lake Store länkade tjänsten**

```JSON
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

> [!NOTE]
> Konfigurationsinformation finns hello [länkade tjänstegenskaper](#linked-service-properties) avsnitt.
>

**Indatauppsättning för Azure-blobb**

I följande exempel hello, data hämtas från en ny blob varje timme (`"frequency": "Hour", "interval": 1`). hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder hello år, månad och dagsdelen av hello starttid. hello filnamn använder hello timme som del av hello starttid. Hej `"external": true` inställningen informerar hello Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
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

**Azure Data Lake Store utdatauppsättningen**

följande exempel kopierar tooData Datasjölager hello. Nya data kopieras tooData Datasjölager varje timme.

```JSON
{
    "name": "AzureDataLakeStoreOutput",
      "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
              "frequency": "Hour",
              "interval": 1
        }
      }
}
```


**Kopiera aktivitet i en pipeline med en blob-källa och mottagare ett Data Lake Store**

I följande exempel hello, hello pipelinen innehåller en kopia-aktivitet som är konfigurerad toouse hello inkommande och utgående datauppsättningar. Hej kopieringsaktiviteten är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello `source` typ har angetts för`BlobSource`, och hello `sink` typ har angetts för`AzureDataLakeStoreSink`.

```json
{  
    "name":"SamplePipeline",
    "properties":
    {  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":
        [  
              {
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureBlobInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureDataLakeStoreOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                      },
                      "sink": {
                        "type": "AzureDataLakeStoreSink"
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

### <a name="example-copy-data-from-azure-data-lake-store-tooan-azure-blob"></a>Exempel: Kopiera data från Azure Data Lake Store tooan Azure blob
hello exempelkod i det här avsnittet visas:

* En länkad tjänst av typen [AzureDataLakeStore](#linked-service-properties).
* En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Indata [dataset](data-factory-create-datasets.md) av typen [AzureDataLakeStore](#dataset-properties).
* Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* En [pipeline](data-factory-create-pipelines.md) med en kopia-aktivitet som använder [AzureDataLakeStoreSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello kod kopierar time series-data från Data Lake Store tooan Azure blob varje timme. 

**Azure Data Lake Store länkade tjänsten**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        }
    }
}
```

> [!NOTE]
> Konfigurationsinformation finns hello [länkade tjänstegenskaper](#linked-service-properties) avsnitt.
>

**Länkad Azure Storage-tjänst**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure Data Lake-indatauppsättningen**

I det här exemplet anger `"external"` för`true` informerar hello Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```json
{
    "name": "AzureDataLakeStoreInput",
      "properties":
    {
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
        "external": true,
        "availability": {
            "frequency": "Hour",
              "interval": 1
        },
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
**Utdatauppsättning för Azure-blobb**

I följande exempel hello, skrivs data tooa nya blob varje timme (`"frequency": "Hour", "interval": 1`). hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder hello år, månad, dag och timmar delen av hello starttid.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**En kopia aktivitet i en pipeline med ett Azure Data Lake Store-källa och en blob-mottagare**

I följande exempel hello, hello pipelinen innehåller en kopia-aktivitet som är konfigurerad toouse hello inkommande och utgående datauppsättningar. Hej kopieringsaktiviteten är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello `source` typ har angetts för`AzureDataLakeStoreSource`, och hello `sink` typ har angetts för`BlobSink`.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureDakeLaketoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureDataLakeStoreInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "AzureDataLakeStoreSource",
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

Du kan mappa kolumner från hello källa dataset toocolumns i hello sink dataset i aktivitetsdefinitionen för hello kopia. Mer information finns i [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestanda- och justering
toolearn om hello faktorer som påverkar Kopieringsaktiviteten prestanda och hur toooptimize, se hello [prestandajustering guide och Kopieringsaktivitet prestanda](data-factory-copy-activity-performance.md) artikel.
