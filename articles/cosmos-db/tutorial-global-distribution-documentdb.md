---
title: "Azure DB Cosmos global distributionsplatsen självstudier för DocumentDB API | Microsoft Docs"
description: "Lär dig hur du ställer in Azure Cosmos DB global distributionsplatsen med hjälp av DocumentDB-API."
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
ms.openlocfilehash: f4d8efe9814bd28bb902567a23b541bc9b5414a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-documentdb-api"></a>Hur du konfigurerar Azure Cosmos DB global distributionsplatsen med hjälp av DocumentDB-API

I den här artikeln visar vi hur du använder Azure-portalen för att konfigurera Azure Cosmos DB global distributionsplatsen och ansluter sedan med hjälp av DocumentDB-API.

Den här artikeln omfattar följande aktiviteter: 

> [!div class="checklist"]
> * Konfigurera distributionslistor med Azure-portalen
> * Konfigurera distributionslistor med hjälp av den [DocumentDB APIs](documentdb-introduction.md)

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-documentdb-api"></a>Ansluter till en önskad region med DocumentDB-API

För att kunna dra nytta av [global distributionsplatsen](distribute-data-globally.md), klientprogram kan ange beställda inställningar listan över regioner som används för att utföra åtgärder för dokumentet. Detta kan göras genom att ställa in principen. Baserat på konfigurationen av Azure DB som Cosmos-kontot, aktuella regional tillgänglighet och inställningar listan som anges, kommer den mest optimala slutpunkten väljas av DocumentDB SDK, för att utföra skrivåtgärder och läsåtgärder.

Den här inställningen listan har angetts när du initierar en anslutning med DocumentDB-SDK. SDK: erna acceptera en valfri parameter ”PreferredLocations” som en sorterad lista över Azure-regioner.

SDK skickar automatiskt alla skrivningar till aktuellt skriva region.

Alla Läs kommer att skickas till den första tillgängliga regionen i listan över PreferredLocations. Om begäran inte klienten misslyckas nedåt i listan till nästa region, och så vidare.

SDK: erna görs endast försök att läsa från de regioner som anges i PreferredLocations. Så, till exempel om det konto som är tillgänglig i tre regioner, men klienten endast anger två av regionerna som icke-och skrivbehörighet för PreferredLocations, sedan utan läsning hanteras utanför området skrivåtgärder även vid redundans.

Programmet kan verifiera den aktuella write-slutpunkten och läsa slutpunkt som valts av SDK genom att kontrollera två egenskaper, WriteEndpoint och ReadEndpoint, finns i SDK-version 1.8 och senare.

Om egenskapen PreferredLocations inte har angetts hanteras alla förfrågningar från det aktuella området för skrivning.

## <a name="net-sdk"></a>.NET SDK
SDK kan användas utan någon kodändringar. I det här fallet SDK automatiskt styr både läs och skriver till det aktuella området för skrivning.

Parametern ConnectionPolicy för konstruktorn DocumentClient har en egenskap som kallas Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations i version 1,8 och senare av .NET SDK. Den här egenskapen är av typen samling `<string>` och bör innehålla en lista över regionnamn. Strängvärden formateras per Region namnkolumnen på den [Azure-regioner] [ regions] sida, utan blanksteg före eller efter först och sista tecknet respektive.

Aktuella Skriv- och Läs slutpunkter är tillgängliga i DocumentClient.WriteEndpoint och DocumentClient.ReadEndpoint respektive.

> [!NOTE]
> URL: er för slutpunkterna ska inte betraktas som långlivade konstanter. Tjänsten kan uppdatera dem när som helst. SDK hanterar automatiskt den här ändringen.
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

// connect to DocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript och Python SDK
SDK kan användas utan någon kodändringar. I det här fallet SDK kommer automatiskt att dirigera både läsningar och skrivningar till aktuellt skriva region.

I version 1,8 och senare av varje SDK parametern ConnectionPolicy för konstruktorn DocumentClient en ny egenskap kallas DocumentClient.ConnectionPolicy.PreferredLocations. Detta är parametern är en matris med strängar som tar en lista över regionnamn. Namnen är formaterade per Region namnkolumnen i den [Azure-regioner] [ regions] sidan. Du kan också använda fördefinierade konstanter i informationssyfte objektet AzureDocuments.Regions

Aktuella Skriv- och Läs slutpunkter är tillgängliga i DocumentClient.getWriteEndpoint och DocumentClient.getReadEndpoint respektive.

> [!NOTE]
> URL: er för slutpunkterna ska inte betraktas som långlivade konstanter. Tjänsten kan uppdatera dem när som helst. Den här ändringen ska hanteras automatiskt av SDK.
>
>

Nedan visas ett kodexempel för NodeJS eller Javascript. Python och Java följer samma mönster.

```java
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in the following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize the connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a>REST
När ett databaskonto har gjorts i flera områden, kan klienterna fråga dess tillgänglighet genom att utföra en GET-begäran för följande URI.

    https://{databaseaccount}.documents.azure.com/

Tjänsten returneras en lista över regioner och deras motsvarande Azure DB som Cosmos-slutpunkt URI: er för replikerna. Det aktuella området skrivåtgärder kommer att visas i svaret. Klienten kan sedan välja lämpliga slutpunkten för alla ytterligare REST API-begäranden på följande sätt.

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


* PUT, POST och DELETE-begäranden måste gå till den angivna skrivningen URI
* Alla hämtar och andra skrivskyddade förfrågningar (till exempel frågor) kan gå till en slutpunkt av klientens val

Skriva begäranden till skrivskyddad regioner misslyckas med felkoden för HTTP 403 (”förbjuden”).

Om skrivning region ändras efter klientens inledande identifieringen fas, misslyckas efterföljande skrivningar till den föregående skrivåtgärder regionen med HTTP-felkod 403 (”förbjuden”). Klienten bör sedan få listan över regioner igen för att hämta den uppdaterade write-regionen.

Det är den som Slutför den här kursen. Du kan lära dig hur du hanterar konsekvensen för globalt replikerade kontot genom att läsa [konsekvensnivåer i Azure Cosmos DB](consistency-levels.md). Och för mer information om hur globala databasreplikering fungerar i Azure Cosmos DB, se [distribuera data globalt med Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen kommer du har gjort följande:

> [!div class="checklist"]
> * Konfigurera distributionslistor med Azure-portalen
> * Konfigurera globala distribution via DocumentDB APIs

Du kan nu fortsätta till nästa kurs att lära dig hur du utvecklar lokalt med hjälp av lokala Azure DB som Cosmos-emulatorn.

> [!div class="nextstepaction"]
> [Utveckla lokalt med emulatorn](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

