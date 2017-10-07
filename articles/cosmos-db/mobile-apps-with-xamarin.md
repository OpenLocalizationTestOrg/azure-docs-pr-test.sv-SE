---
title: aaaBuild mobila program med Xamarin och Azure Cosmos DB | Microsoft Docs
description: "En självstudiekurs som skapar en Xamarin-iOS, Android eller formulär program med hjälp av Azure Cosmos DB. Azure Cosmos DB är en snabb, planeten skala, molnet databas för mobila appar."
services: cosmos-db
documentationcenter: .net
author: arramac
manager: monicar
editor: 
ms.assetid: ff97881a-b41a-499d-b7ab-4f394df0e153
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: arramac
ms.openlocfilehash: 362a0e239a61e1309e1cf720c23151760952cf69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-mobile-applications-with-xamarin-and-azure-cosmos-db"></a>Skapa mobila program med Xamarin och Azure Cosmos DB
De flesta mobila appar måste toostore data i molnet hello och Azure Cosmos DB är en moln-databas för mobila appar. Den innehåller allt mobila utvecklare behöver. Det är en helt hanterad databas som en tjänst som kan skalas på begäran. Det kan ta tillämpningsprogrammet tooyour data transparent, oavsett var användarna befinner sig runt hello världen. Med hjälp av hello [Azure Cosmos DB .NET Core SDK](documentdb-sdk-dotnet-core.md), kan du aktivera Xamarin-appar toointeract direkt med Azure Cosmos DB, utan en mellannivå.

Den här artikeln innehåller en vägledning för att skapa mobila appar med Xamarin och Azure Cosmos DB. Du kan hitta hello fullständig källkoden hello genomgång på [Xamarin och Azure Cosmos DB på GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin), inklusive hur toomanage användare och behörigheter.

## <a name="azure-cosmos-db-capabilities-for-mobile-apps"></a>Azure DB Cosmos-funktioner för mobila appar
Azure Cosmos-DB ger följande viktiga funktioner för mobila appar utvecklare hello:

![Azure DB Cosmos-funktioner för mobila appar](media/mobile-apps-with-xamarin/documentdb-for-mobile.png)

* Omfattande frågor över schemalös data. Azure Cosmos-DB lagrar data som schemalös JSON-dokument i heterogena samlingar. Den erbjuder [omfattande och snabb frågor](documentdb-sql-query.md) utan hello behöver tooworry om scheman eller index.
* Snabb genomflöde. Det tar bara några millisekunder tooread och skriva dokument med Azure Cosmos DB. Utvecklare kan ange hello genomströmning som de behöver och Azure Cosmos DB godkänner den med 99,99 procent serviceavtal.
* Obegränsad skala. Azure DB som Cosmos-samlingar [växer när din app växer](partition-data.md). Du kan starta med liten storlek och dataflöde hundratals begäranden per sekund. Samlingar kan växa toopetabytes av data och godtyckligt högt dataflöde med hundratals miljoner förfrågningar per sekund.
* Globalt distribuerad. Mobila appar som användare använder hello gå ofta över hälsningsmeddelande. Azure Cosmos-databasen är en [globalt distribuerad databas](distribute-data-globally.md). Klicka på hello kartan toomake data tillgänglig tooyour användarna.
* Inbyggda omfattande auktorisering. Med Azure Cosmos DB, kan du enkelt implementera populära mönster som [användardata](https://aka.ms/documentdb-xamarin-todouser) eller fleranvändar delade data utan kod för komplexa anpassad auktorisering.
* Geospatiala frågor. Geo-kontextuella upplevelser erbjuder idag för flera mobila appar. Med förstklassigt stöd för [geospatiala typer](geospatial.md), Azure Cosmos DB gör att skapa dessa upplevelser enkelt tooaccomplish.
* Binära bifogade filer. Din AppData innehåller ofta binär blobbar. Inbyggt stöd för bifogade filer gör det enklare toouse Azure Cosmos DB som affär för dina data i appen.

## <a name="azure-cosmos-db-and-xamarin-tutorial"></a>Azure Cosmos DB och Xamarin självstudiekursen
Hej följande självstudiekurser visar hur toobuild ett mobilt program med Xamarin och Azure Cosmos DB. Du kan hitta hello fullständig källkoden hello genomgång på [Xamarin och Azure Cosmos DB på GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).

### <a name="get-started"></a>Kom igång
Det är enkelt tooget igång med Azure Cosmos DB. Gå toohello Azure-portalen och skapa ett nytt konto i Azure Cosmos DB. Klicka på hello **Snabbstart** fliken. Hämta hello Xamarin Forms uppgiften listan exempel är redan ansluten tooyour Azure DB som Cosmos-konto. 

![Azure Cosmos DB Snabbstart för mobila appar](media/mobile-apps-with-xamarin/cosmos-db-quickstart.png)

Om du har en befintlig Xamarin-app kan du lägga till hello [Azure Cosmos DB NuGet-paketet](documentdb-sdk-dotnet-core.md). Azure Cosmos-DB stöder Xamarin.IOS Xamarin.Android, och Xamarin Forms delade bibliotek.

### <a name="work-with-data"></a>Arbeta med data
Spara data lagras i Azure Cosmos DB som schemalös JSON-dokument i heterogena samlingar. Du kan lagra dokument med olika strukturer i hello samma samling:

```cs
    var result = await client.CreateDocumentAsync(collectionLink, todoItem);
```

Du kan använda språkintegrerade frågor över schemalös data i dina Xamarin-projekt:

```cs
    var query = await client.CreateDocumentQuery<ToDoItem>(collectionLink)
                    .Where(todoItem => todoItem.Complete == false)
                    .AsDocumentQuery();

    Items = new List<TodoItem>();
    while (query.HasMoreResults) {
        Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
    }
```
### <a name="add-users"></a>Lägga till användare
Som många Kom igång exempel autentiserar hello Azure Cosmos DB exempel som du hämtade toohello tjänsten med hjälp av en huvudnyckel hårdkodad i hello app kod. Den här standardinställningen är inte bra för en app som du avser toorun var som helst utom på din lokala emulatorn. Om en obehörig användare som fick hello huvudnyckel, äventyras alla hello data över ditt konto i Azure Cosmos DB. I stället vill du din app tooaccess hello bara poster för hello inloggad användare. Azure Cosmos-DB kan utvecklare toogrant program läsa eller läsning och skrivning behörighet tooa samling, en uppsättning grupperade efter en partitionsnyckel dokument eller ett visst dokument. 

Följ dessa steg toomodify hello uppgiften listan app tooa flera användare att göra listan app: 

  1. Lägg till inloggningen tooyour app med hjälp av Facebook, Active Directory eller någon annan provider.

  2. Skapa en samling Azure Cosmos DB UserItems med **/userId** som hello partitionsnyckel. Att ange hello partitionsnyckel för samlingen kan Azure Cosmos DB tooscale oändligt som hello antal användare av ditt program växer, medan toooffer snabb frågor.

  3. Lägg till Azure Cosmos DB resurs Token Broker. Den här enkla Web API autentiserar användare och utfärdar tillfällig token toosigned användare med åtkomst endast toohello dokument inom deras partition. I det här exemplet finns resurs Token Broker i App Service.

  4. Ändra hello app tooauthenticate tooResource Token Broker med Facebook och begära hello resurs token för hello inloggade Facebook-användare. Du kan sedan komma åt sina data i hello UserItems samling.  

Du hittar en fullständig kodexemplet i det här mönstret på [resurs Token Broker på GitHub](http://aka.ms/documentdb-xamarin-todouser). Det här diagrammet visar hello lösningen:

![Azure DB Cosmos-användare och behörigheter broker](media/mobile-apps-with-xamarin/documentdb-resource-token-broker.png)

Om du vill att två användare toohave åtkomst toohello samma att göra-lista, du kan lägga till ytterligare behörigheter toohello åtkomst-token i resursen Token Broker.

### <a name="scale-on-demand"></a>Skala på begäran
Azure Cosmos-DB är en hanterad databas som en tjänst. När användarbasen växer, behöver du inte tooworry om etablering av virtuella datorer eller att öka kärnor. Allt du behöver tootell Azure Cosmos DB är hur många åtgärder per sekund (genomströmning) din app behöver. Du kan ange hello genomströmning via hello **skala** fliken med hjälp av ett mått på dataflödet som kallas begära enheter (RUs) per sekund. Till exempel en Läsåtgärd på ett dokument på 1 KB kräver 1 RU. Du kan också lägga till aviseringar toohello **genomströmning** mått toomonitor hello trafik tillväxt och programmässigt Ändra hello genomströmning som aviseringar brand.

![Azure DB Cosmos skala genomströmning på begäran](media/mobile-apps-with-xamarin/cosmos-db-xamarin-scale.png)

### <a name="go-planet-scale"></a>Gå planeten skala
Du kan få användare i hello globen som din app får popularitet. Eller kanske toobe förberedd för oförutsedda händelser. Gå toohello Azure-portalen och öppna Azure DB som Cosmos-konto. Klicka på hello kartan toomake data kontinuerligt replikera tooany-antal regioner över hälsningsmeddelande. Den här funktionen gör data tillgängliga var användarna är. Du kan också lägga till redundans principer toobe förberedd för risker.

![Azure DB Cosmos skala över geografiska områden](media/mobile-apps-with-xamarin/cosmos-db-xamarin-replicate.png)

Grattis! Du har slutfört hello lösning och har en mobil app med Xamarin och Azure Cosmos DB. Följa liknande steg toobuild Cordova-appar med hjälp av hello Azure Cosmos DB JavaScript SDK och inbyggda iOS/Android-appar med hjälp av Azure Cosmos DB REST API: er.

## <a name="next-steps"></a>Nästa steg
* Visa hello källkoden för [Xamarin och Azure Cosmos DB på GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).
* Hämta hello [Azure Cosmos DB .NET Core SDK](documentdb-sdk-dotnet-core.md).
* Hitta fler kodexempel för [.NET-program](documentdb-dotnet-samples.md).
* Lär dig mer om [Azure Cosmos DB omfattande fråga funktioner](documentdb-sql-query.md).
* Lär dig mer om [geospatiala stöd i Azure Cosmos DB](geospatial.md).



