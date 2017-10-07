---
title: aaaRegional redundans i Azure Cosmos DB | Microsoft Docs
description: "Läs mer om hur manuell och automatisk redundans fungerar med Azure Cosmos DB."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 446e2580-ff49-4485-8e53-ae34e08d997f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2fdc7b0e8958d129ab027e4b11193b12961ddae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automatic-regional-failover-for-business-continuity-in-azure-cosmos-db"></a>Automatisk regional växling vid fel för kontinuitet i Azure Cosmos DB
Azure Cosmos-DB förenklar hello globala fördelning av data genom att erbjuda fullständigt hanterade [flera regioner databasen konton](distribute-data-globally.md) som ger tydliga kompromisser mellan konsekvens, tillgänglighet och prestanda, alla med motsvarande garanterar. Cosmos DB konton ger hög tillgänglighet, siffra ms latens [väldefinierade konsekvensnivåer](consistency-levels.md), transparent regional växling vid fel med flera API: er och hello möjlighet tooelastically skalbara dataflöden och lagringsutrymmen över hello jordglob. 

Cosmos DB stöder både explicit och principen drivs växling vid fel som gör det möjligt toocontrol hello slutpunkt till slutpunkt systembeteendet i hello händelse av fel. I den här artikeln titta på:

* Hur fungerar manuell växling vid fel i Cosmos-databasen?
* Hur automatisk redundans arbete i Cosmos-DB och vad som händer när data center går ned?
* Hur kan du använda manuell växling vid fel i programarkitekturer?

Du kan också information om regional växling vid fel i den här Azure fredag video med Scott Hanselman och huvudnamn tekniker Manager Karthik Raman.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

## <a id="ConfigureMultiRegionApplications"></a>Konfigurera flera regioner program
Innan vi fördjupa dig i redundans lägen titta vi på hur du konfigurerar ett program tootake nytta av flera regional tillgänglighet och bli motståndskraftiga i hello yta regional växling vid fel.

* Först distribuera ditt program i flera områden
* tooensure låg latens åtkomst från varje region som ditt program är distribuerat konfigurera hello motsvarande [över önskade regioner](https://msdn.microsoft.com/library/microsoft.azure.documents.client.connectionpolicy.preferredlocations.aspx#P:Microsoft.Azure.Documents.Client.ConnectionPolicy.PreferredLocations) för varje region via en hello stöds SDK: er.

hello följande fragment visas hur tooinitialize ett program i flera regioner. Här hello Azure Cosmos DB konto `contoso.documents.azure.com` har konfigurerats med två områden - västra USA och Norra Europa. 

* hello program är distribuerat i hello västra USA region (med Azure App Service till exempel) 
* Konfigurerad med `West US` som hello första önskad region för låg fördröjning läser
* Konfigurerad med `North Europe` som hello andra önskad region (för hög tillgänglighet under regionala fel)

I hello DocumentDB API, den här konfigurationen ser ut så hello följande utdrag:

```cs
ConnectionPolicy usConnectionPolicy = new ConnectionPolicy 
{ 
    ConnectionMode = ConnectionMode.Direct,
    ConnectionProtocol = Protocol.Tcp
};

usConnectionPolicy.PreferredLocations.Add(LocationNames.WestUS);
usConnectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

DocumentClient usClient = new DocumentClient(
    new Uri("https://contosodb.documents.azure.com"),
    "memf7qfF89n6KL9vcb7rIQl6tfgZsRt5gY5dh3BIjesarJanYIcg2Edn9uPOUIVwgkAugOb2zUdCR2h0PTtMrA==",
    usConnectionPolicy);
```

hello programmet distribueras också i hello Nordeuropa region hello prioriterade regioner omvänd ordning. Det vill säga har hello Nordeuropa region angetts först för låg latens läsningar. Sedan har hello västra USA region angetts som hello andra önskad region för hög tillgänglighet under regionala fel.

hello arkitekturdiagrammet nedan visas en distribution av flera regioner där Cosmos-DB och hello program är konfigurerade toobe som är tillgängliga i fyra Azure geografiska regioner.  

![Globalt distribuerade programdistribution med Azure Cosmos DB](./media/regional-failover/app-deployment.png)

Nu ska vi titta på hur hello Cosmos DB tjänsten hanterar regionala fel via automatisk redundans. 

## <a id="AutomaticFailovers"></a>Automatisk redundans
I hello sällsynt händelse av en Azure regionala nätverksavbrott eller om data center avbrott utlöser Cosmos DB automatiskt redundans av alla Cosmos-DB-konton med i hello påverkas region. 

**Vad händer om en skrivskyddad region har ett avbrott?**

Cosmos DB konton med en skrivskyddad region i någon av hello påverkas regioner frånkopplad från deras skrivåtgärder region och markerats offline automatiskt. Hej Cosmos DB SDK implementera ett regionalt discovery-protokollet som gör att de tooautomatically identifiera när en region är tillgänglig och omdirigering läsa anrop toohello nästa tillgängliga område i hello önskad regionlista. Om ingen av hello regioner i hello önskad region är tillgänglig växla samtal automatiskt över toohello aktuella skrivåtgärder region. Inga ändringar som krävs i ditt program kod toohandle regional växling vid fel. Under hela processen fortsätter konsekvens garanterar toobe hanteras med Cosmos DB.

![Läs region fel i Azure Cosmos DB](./media/regional-failover/read-region-failures.png)

När hello påverkas region återställer från hello avbrott, återställs alla hello påverkas Cosmos-DB-konton i hello region automatiskt av hello-tjänsten. Cosmos DB-konton som hade en skrivskyddad region i hello påverkas region och sedan automatiskt synkroniseras med den aktuella skrivåtgärder region och aktivera online. Hej Cosmos DB SDK identifiera hello tillgängligheten för hello ny region och utvärdera om hello region måste väljas som hello aktuella skrivskyddade region baserat på hello önskad regionlista som konfigurerats av hello program. Efterföljande läsningar är omdirigerade toohello återställda region utan några ändringar tooyour programkod.

**Vad händer om en skrivning region har ett avbrott?**

Om hello berörda region är hello aktuella skrivåtgärder region för en viss Cosmos-DB-konto, sedan markeras hello region automatiskt som offline. En annan region befordras sedan som hello skriva region varje berörda Cosmos-DB-konto. Du kan helt styra hello region val för Cosmos-DB-konton via hello Azure-portalen eller [programmässigt](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_FailoverPriorityChange). 

![Redundans prioriteringar för Azure Cosmos DB](./media/regional-failover/failover-priorities.png)

Under automatisk redundans Cosmos DB väljer automatiskt hello nästa skrivning område för en given Cosmos-DB konto baserat på hello angetts prioritetsordning. 

![Skriva fel region i Azure Cosmos DB](./media/regional-failover/write-region-failures.png)

När hello påverkas region återställer från hello avbrott, återställs alla hello påverkas Cosmos-DB-konton i hello region automatiskt av hello-tjänsten. 

* Cosmos DB konton med sina tidigare skrivåtgärder region i hello påverkas region kvar i offlineläge med skrivskyddade tillgänglighet även efter hello återställning av hello region. 
* Du kan fråga den här regionen toocompute alla unreplicated skrivningar under hello avbrott genom att jämföra med hello data är tillgängliga i hello aktuella skrivåtgärder region. Baserat på hello behoven för ditt program, kan du utföra merge och/eller konflikt upplösning och skriva hello slutgiltiga uppsättningen med ändringarna tillbaka toohello aktuella skrivåtgärder region. 
* När du har slutfört importerar ändringar kan du ta hello påverkas region tillbaka online genom att ta bort och readding hello region tooyour Cosmos-DB-konto. När hello region har lagts till igen kan du konfigurera det tillbaka som hello skrivåtgärder region genom att utföra en manuell växling via hello Azure-portalen eller [programmässigt](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate).

## <a id="ManualFailovers"></a>Manuell växling vid fel

Dessutom skriva tooautomatic växling vid fel, hello aktuella region för en viss Cosmos-DB-konto kan ändras dynamiskt tooone hello befintliga skrivskyddade regioner. Manuell redundans kan inledas via hello Azure-portalen eller [programmässigt](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate). 

Kontrollera manuell redundans **noll dataförlust** och **noll tillgänglighet** förlust och att skrivåtgärder Överföringsstatus från hello gamla skriver region toohello ny för hello angivna Cosmos-DB-konto. Som i automatisk redundans hello Cosmos DB SDK automatiskt skriva handtag region ändras under manuell redundans och ser till att anrop är automatiskt omdirigerade toohello ny skrivning region. Ingen kod eller ändringar av konfigurationen som krävs i ditt program toomanage växling vid fel. 

![Manuell växling vid fel i Azure Cosmos DB](./media/regional-failover/manual-failovers.png)

Några av hello vanliga scenarier där manuell växling kan användas är:

**Följ hello klockan modellen**: om ditt program har förutsägbar trafikmönster baserat på hello tidpunkten hello, kan du regelbundet ändra hello skrivåtgärder status toohello mest aktiva geografiskt område utifrån tid på dagen hello.

**Tjänstuppdatering**: vissa globalt distribuerade programdistribution kan omfatta omdirigering trafik toodifferent region via traffic manager under deras planerade tjänstuppdatering. Sådana programdistribution nu kan använda manuell växling tookeep hello skrivåtgärder status toohello region där det händer toobe active trafik under hello-uppdatering fönster.

**Kontinuitet för företag och Disaster Recovery BCDR- och hög tillgänglighet och Disaster Recovery (HADR) flyttar**: de flesta företagsprogram inkluderar business continuity tester som en del av deras utveckling och versionen. BCDR och HADR testning är ofta ett viktigt steg i efterlevnad certifieringar och garanterande tjänsttillgänglighet hello gäller regionala avbrott. Du kan testa hello BCDR beredskap för dina program som använder Cosmos-databas för lagring av utlösa en manuell växling för Cosmos-DB-konto och/eller att lägga till och ta bort en region dynamiskt.

Den här artikeln har granskat hur manuell och automatisk redundans arbete i Cosmos-databasen och hur du kan konfigurera din Cosmos DB konton och program toobe globalt tillgänglig. Genom att använda Cosmos DB globala replication stöd kan du förbättra svarstiden för slutpunkt till slutpunkt och så att de är hög tillgänglighet även i händelse av hello fel region. 

## <a id="NextSteps"></a>Nästa steg
* Lär dig mer om hur Cosmos DB stöder [global distributionsplatsen](distribute-data-globally.md)
* Lär dig mer om [global konsekvens med Azure Cosmos DB](consistency-levels.md)
* Utveckla med flera områden med Azure Cosmos DB [DocumentDB-API](../cosmos-db/tutorial-global-distribution-documentdb.md)
* Lär dig hur toobuild [flera regioner writer arkitekturer](multi-region-writers.md) med Azure DocumentDB

