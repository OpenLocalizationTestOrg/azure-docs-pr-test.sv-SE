---
title: "aaaAzure Cosmos DB global distributionsplatsen självstudier för DocumentDB API | Microsoft Docs"
description: "Lär dig hur toosetup Azure Cosmos DB global distributionsplatsen med hello DocumentDB-API."
services: cosmos-db
keywords: Global distributionsplatsen, documentdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: a1d5f01faa62407fbbc9c078ef4a9589a1a29219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-documentdb-api"></a>Hur toosetup Azure Cosmos DB global distributionsplatsen med hello DocumentDB-API

I den här artikeln visar vi hur toouse hello Azure portal toosetup Azure Cosmos DB global distributionsplatsen och sedan ansluta med hello DocumentDB-API.

Den här artikeln beskriver hello följande uppgifter: 

> [!div class="checklist"]
> * Konfigurera distributionslistor med hello Azure-portalen
> * Konfigurera distributionslistor med hello [DocumentDB APIs](documentdb-introduction.md)

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-documentdb-api"></a>Ansluta tooa önskad region med hello DocumentDB-API

I ordning tootake nytta av [global distributionsplatsen](distribute-data-globally.md), klientprogram kan ange hello sorterade inställningar lista över regioner toobe används tooperform dokumentet åtgärder. Detta kan göras genom att ange hello anslutningsprincip. Baserat på hello Azure Cosmos DB kontokonfigurationen anges regional tillgänglighet och hello inställningar lista hello de flesta optimala endpoint väljas av hello DocumentDB SDK tooperform skriva och läsåtgärder.

Den här inställningen listan anges när initierar en anslutning med hello DocumentDB-SDK. hello SDK acceptera en valfri parameter ”PreferredLocations” som en sorterad lista över Azure-regioner.

hello SDK skickar automatiskt alla skrivningar toohello aktuella skrivåtgärder region.

Alla Läs skickas toohello första tillgängliga region i hello PreferredLocations lista. Om hello begäran misslyckas hello klienten misslyckas ned hello listan toohello nästa område, och så vidare.

hello SDK försöker tooread från hello regioner som anges i PreferredLocations. Så, till exempel om hello databaskonto finns tillgänglig i tre regioner, men hello klienten endast anger två hello-write regioner för PreferredLocations, sedan utan läsning hanteras utanför hello skrivåtgärder region, även i hello fall av redundans.

hello-programmet kan verifiera hello aktuella skrivåtgärder slutpunkt och läsa slutpunkt som valts av hello SDK av kontrollerar två egenskaper, WriteEndpoint och ReadEndpoint, finns i SDK-version 1.8 och senare.

Om hello PreferredLocations egenskapen inte anges kommer alla begäranden hanteras från hello aktuella skrivåtgärder region.

## <a name="net-sdk"></a>.NET SDK
hello SDK kan användas utan någon kodändringar. I det här fallet hello SDK automatiskt styr både läs och skriver toohello aktuella skrivåtgärder region.

I version 1,8 och senare av hello .NET SDK har hello ConnectionPolicy parametern för hello DocumentClient konstruktorn en egenskap som kallas Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Den här egenskapen är av typen samling `<string>` och bör innehålla en lista över regionnamn. hello strängvärden formateras per hello regionnamn kolumn på hello [Azure-regioner] [ regions] sida, utan blanksteg före eller efter hello första och sista tecknet respektive.

hello aktuella Skriv- och Läs slutpunkter är tillgängliga i DocumentClient.WriteEndpoint och DocumentClient.ReadEndpoint respektive.

> [!NOTE]
> hello URL: er för hello slutpunkter ska inte betraktas som långlivade konstanter. hello-tjänsten kan uppdatera dem när som helst. hello SDK hanterar automatiskt den här ändringen.
>
>

```csharp
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;
  
ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect tooDocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript och Python SDK
hello SDK kan användas utan någon kodändringar. I det här fallet kommer automatiskt att dirigera SDK hello både läser och skriver toohello aktuella skrivåtgärder region.

I version 1,8 och senare av varje SDK hello ConnectionPolicy parameter för hello DocumentClient konstruktorn den nya egenskapen DocumentClient.ConnectionPolicy.PreferredLocations. Detta är parametern är en matris med strängar som tar en lista över regionnamn. hello namn är formaterade per hello regionnamn kolumn i hello [Azure-regioner] [ regions] sidan. Du kan också använda hello fördefinierade konstanter i hello bekvämlighet objektet AzureDocuments.Regions

hello aktuella Skriv- och Läs slutpunkter är tillgängliga i DocumentClient.getWriteEndpoint och DocumentClient.getReadEndpoint respektive.

> [!NOTE]
> hello URL: er för hello slutpunkter ska inte betraktas som långlivade konstanter. hello-tjänsten kan uppdatera dem när som helst. hello SDK kommer att hantera den här ändringen automatiskt.
>
>

Nedan visas ett kodexempel för NodeJS eller Javascript. Python och Java följer hello samma mönster.

```java
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in hello following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize hello connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a>REST
När ett databaskonto har gjorts i flera områden, kan klienterna fråga dess tillgänglighet genom att utföra en GET-begäran på hello följande URI.

    https://{databaseaccount}.documents.azure.com/

hello-tjänsten returnerar en lista över regioner och deras motsvarande Azure DB som Cosmos-slutpunkt URI: er för hello repliker. hello aktuella skrivåtgärder region visas hello svar. hello-klienten kan sedan välja lämpliga hello-slutpunkt för alla ytterligare REST API-begäranden på följande sätt.

Exempelsvar

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


* PUT, POST och DELETE-begäranden måste gå toohello anges skriva URI
* Alla hämtar och andra skrivskyddade förfrågningar (till exempel frågor) kan gå tooany endpoint hello klienten väljer

Skriva begäranden endast tooread regioner misslyckas med felkoden för HTTP 403 (”förbjuden”).

Om hello skrivåtgärder region ändras efter skriver hello klienten den inledande identifieringen-fasen efterföljande toohello tidigare skrivåtgärder region misslyckas med felkoden för HTTP 403 (”förbjuden”). hello klienten bör sedan få hello listan över regioner igen tooget hello uppdaterade skrivåtgärder region.

Det är den som Slutför den här kursen. Du kan lära dig hur toomanage hello konsekvenskontroll av globalt replikerade kontot genom att läsa [konsekvensnivåer i Azure Cosmos DB](consistency-levels.md). Och för mer information om hur globala databasreplikering fungerar i Azure Cosmos DB, se [distribuera data globalt med Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Nästa steg

I den här kursen har du gjort hello följande:

> [!div class="checklist"]
> * Konfigurera distributionslistor med hello Azure-portalen
> * Konfigurera distributionslistor med hello DocumentDB APIs

Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodevelop lokalt med hjälp av hello Azure Cosmos DB lokala emulatorn.

> [!div class="nextstepaction"]
> [Utveckla lokalt med hello-emulatorn](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

