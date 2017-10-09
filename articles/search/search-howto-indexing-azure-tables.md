---
title: aaaIndexing Azure Table storage med Azure Search | Microsoft Docs
description: "Lär dig hur tooindex data lagras i Azure Table storage med Azure Search"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 1cc27411-d0cc-40ed-8aed-c7cb9ab402b9
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/10/2017
ms.author: eugenesh
ms.openlocfilehash: abb01a4d807cede310246b34775d8059fceb4456
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="index-azure-table-storage-with-azure-search"></a>Index Azure Table storage med Azure Search
Den här artikeln visar hur toouse Azure Search tooindex data lagras i Azure Table storage.

## <a name="set-up-azure-table-storage-indexing"></a>Konfigurera Azure Table storage indexering

Du kan konfigurera en indexerare för Azure Table storage med hjälp av följande resurser:

* [Azure Portal](https://ms.portal.azure.com)
* Azure Search [REST-API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure Search [.NET SDK](https://aka.ms/search-sdk)

Här ser hello flödet med hjälp av hello REST API. 

### <a name="step-1-create-a-datasource"></a>Steg 1: Skapa en datakälla

En datakälla anger vilka data tooindex hello autentiseringsuppgifter behövs tooaccess hello data och hello principer som gör att Azure Search tooefficiently identifiera ändringar i hello data.

För tabellen indexering måste hello datasource ha hello följande egenskaper:

- **namnet** är hello unika namnet på hello datasource inom din söktjänst.
- **typen** måste vara `azuretable`.
- **autentiseringsuppgifter** parametern innehåller hello lagringsanslutningssträng för kontot. Se hello [ange autentiseringsuppgifter](#Credentials) information.
- **behållaren** anger hello tabellnamn, och en valfri fråga.
    - Ange hello tabellnamn med hello `name` parameter.
    - Du kan också ange en fråga med hjälp av hello `query` parameter. 

> [!IMPORTANT] 
> När det är möjligt ska du använda ett filter på PartitionKey för bättre prestanda. Andra frågan har en fullständig tabellgenomsökning, vilket resulterar i sämre prestanda för stora tabeller. Se hello [prestandaöverväganden](#Performance) avsnitt.


toocreate en datakälla:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Mer information om hello skapa Datasource API finns [skapa Datasource](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="ways-toospecify-credentials"></a>Sätt toospecify autentiseringsuppgifter ####

Du kan ange hello autentiseringsuppgifter för hello tabellen i något av följande sätt: 

- **Fullständig åtkomst lagringsanslutningssträng för kontot**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>` kan du få hello anslutningssträngen från hello Azure-portalen genom att gå toohello **lagring kontoblad** > **inställningar**   >  **Nycklar** (för klassiska lagringskonton) eller **inställningar** > **åtkomstnycklar** (för Azure-resurs Manager storage-konton).
- **Storage-konto delad åtkomst signatur anslutningssträngen**: `TableEndpoint=https://<your account>.table.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<hello signature>&spr=https&se=<hello validity end time>&srt=co&ss=t&sp=rl` hello signatur för delad åtkomst ska ha hello listan och läsbehörighet på behållare (tabeller i det här fallet) och objekt (rader).
-  **Signatur för delad åtkomst**: `ContainerSharedAccessUri=https://<your storage account>.table.core.windows.net/<table name>?tn=<table name>&sv=2016-05-31&sig=<hello signature>&se=<hello validity end time>&sp=r` hello signatur för delad åtkomst måste ha behörigheter för frågan (läsa) för hello tabellen.

Mer information om delad lagring komma åt signaturer, se [använder signaturer för delad åtkomst](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Om du använder autentiseringsuppgifter för signatur för delad åtkomst måste tooupdate hello datakällsautentiseringsuppgifter regelbundet med förnyat signaturer tooprevent de gått ut. Om autentiseringsuppgifter för signatur för delad åtkomst ut misslyckas hello indexeraren med ett felmeddelande för ”autentiseringsuppgifter som angetts i anslutningssträngen för hello är ogiltig eller har upphört att gälla”.  

### <a name="step-2-create-an-index"></a>Steg 2: Skapa ett index
hello index anger hello fält i ett dokument, hello attribut och andra konstruktioner som formen hello sökinställningar.

toocreate ett index:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
          ]
    }

Mer information om hur du skapar index finns [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index).

### <a name="step-3-create-an-indexer"></a>Steg 3: Skapa en indexerare
En indexerare ansluter en datakälla med ett mål sökindex och ger en schemalagd tooautomate hello datauppdatering. 

När hello index och datakälla har skapats kan är du klar toocreate hello indexeraren:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Indexeraren körs varannan timme. (hello schemaintervallet anges för ”PT2H”.) toorun en indexerare var 30: e minut, ange hello-intervall för ”PT30M”. hello är kortaste stöds fem minuter. hello-schemat är valfritt. Om det utelämnas används körs en indexeraren bara en gång när den skapas. Du kan dock köra en indexerare på begäran när som helst.   

Mer information om hello skapa indexeraren API finns [skapa indexeraren](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="deal-with-different-field-names"></a>Hantera olika fältnamn
Ibland skiljer hello fältnamnen i ditt befintliga index sig från hello egenskapsnamn i tabellen. Du kan använda mappningar toomap hello egenskapen fältnamn från hello tabellnamn toohello fält i search-index. toolearn mer om fältmappningar finns [Azure Search indexeraren fältmappningar överbrygga hello skillnaderna mellan datakällor och Sök index](search-indexer-field-mappings.md).

## <a name="handle-document-keys"></a>Hantera dokument nycklar
I Azure Search identifierar hello Dokumentnyckel ett dokument. Varje sökindex måste ha exakt ett fält av typen `Edm.String`. hello nyckelfältet krävs för varje dokument som har lagts till toohello index. (I själva verket dess hello endast obligatoriskt fält.)

Eftersom rader har en sammansatt nyckel, Azure Search genererar ett syntetiskt fält med namnet `Key` som är en sammansättning av partition nyckel- och nyckelvärden. Till exempel om en rad PartitionKey är `PK1` och RowKey `RK1`, sedan hello `Key` fältvärde är `PK1RK1`.

> [!NOTE]
> Hej `Key` värde får bestå av tecken som är ogiltiga i dokumentet nycklar, till exempel bindestreck. Du kan hantera ogiltiga tecken med hjälp av hello `base64Encode` [fältet mappning funktionen](search-indexer-field-mappings.md#base64EncodeFunction). Kom ihåg tooalso använder URL-säkert Base64-kodning när skicka dokumentet nycklar i API-anrop, till exempel sökning om du gör detta.
>
>

## <a name="incremental-indexing-and-deletion-detection"></a>Identifiering av inkrementell indexering och borttagning
När du skapar en tabell indexeraren toorun enligt ett schema, den reindexes bara nya eller uppdaterade rader, som bestäms av en rad `Timestamp` värde. Du har inte toospecify en princip för identifiering av ändring. Inkrementell indexering aktiveras åt dig automatiskt.

tooindicate att vissa dokument måste tas bort från hello index, kan du använda en strategi för mjuk borttagning. Lägg till en egenskap tooindicate som den har tagits bort och konfigurera en princip för mjuk borttagning identifiering på hello datasource i stället för att ta bort en rad. Till exempel följa principen hello anser att en rad har tagits bort om hello rad har en egenskap `IsDeleted` med hello värdet `"true"`:

    PUT https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "<query>" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   

<a name="Performance"></a>
## <a name="performance-considerations"></a>Saker att tänka på gällande prestanda

Som standard använder Azure Search hello följande frågefilter: `Timestamp >= HighWaterMarkValue`. Eftersom Azure-tabeller som inte har ett sekundärt index på hello `Timestamp` denna typ av fråga kräver en fullständig tabellgenomsökning och därför går långsamt för stora tabeller.


Här är två möjliga sätt för att förbättra prestanda för tabellen indexering. Bägge förlitar sig på tabellpartitioner: 

- Om dina data kan naturligt partitioneras i flera partition intervall, skapa en datakälla och en motsvarande indexerare för varje partition. Varje indexerare har nu tooprocess endast en specifik partitionsintervall, vilket resulterar i bättre prestanda för frågor. Hello-data som behöver toobe indexerade har ett litet antal fasta partitioner, ännu bättre: varje indexerare har endast en partition sökning. Till exempel toocreate en datakälla för bearbetning av en partitionsintervall med nycklar från `000` för`100`, använda en fråga så här: 
    ```
    "container" : { "name" : "my-table", "query" : "PartitionKey ge '000' and PartitionKey lt '100' " }
    ```

- Om dina data är partitionerad med tid (exempelvis kan du skapa en ny partition varje dag eller en vecka), Överväg hello följande metod: 
    - Använd en fråga hello formatet: `(PartitionKey ge <TimeStamp>) and (other filters)`. 
    - Övervaka indexeraren förlopp med hjälp av [hämta indexeraren Status API](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status), och regelbundet uppdatera hello `<TimeStamp>` villkoret för hello fråga baserat på hello senaste lyckade vattenmärke-värde. 
    - Med den här metoden om du behöver en fullständig indexeringen tootrigger måste tooreset hello datasource frågan i tillägg tooresetting hello indexeraren. 


## <a name="help-us-make-azure-search-better"></a>Hjälp oss att förbättra Azure Search
Om du har funktionsförfrågningar om eller förslag på förbättringar kan du skicka dem på vår [UserVoice-webbplatsen](https://feedback.azure.com/forums/263029-azure-search/).
