---
title: aaaAzure Fallstudie SQL Database Azure - Umbraco | Microsoft Docs
description: "Lär dig mer om hur Umbraco använder SQL-databas tooquickly etablera och skala tjänster för tusentals hyresgäster i hello moln"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5243d31e-3241-4cb0-9470-ad488ff28572
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 93e39e509831a5ff90f129d9537ece0b0dafef0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="umbraco-uses-azure-sql-database-tooquickly-provision-and-scale-services-for-thousands-of-tenants-in-hello-cloud"></a>Umbraco använder Azure SQL Database tooquickly etablera och skala tjänster för tusentals hyresgäster i hello moln
![Umbraco-logotyp](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco är ett populärt öppen källkod innehållshantering system (CMS) som kan köra något från små kampanj eller broschyr platser toocomplex program för Fortune 500 företag och globala media webbplatser. 

> ”Vi har en stor community med utvecklare som använder hello system med mer än 100 000 utvecklare på våra forum och mer än 350 000 platser som är aktiv, kör Umbraco”.
> 
> – Morten Christensen, tekniska Lead Umbraco
> 
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-Umbraco/player]
> 
> 

toosimplify kunddistributioner Umbraco läggs Umbraco-as-a-Service (UaaS): ett erbjudande för programvara som en-tjänst (SaaS) som eliminerar hello behovet av lokala distributioner innehåller inbyggda skalning och tar bort management omkostnader genom att aktivera utvecklare toofocus på produkten innovation i stället för lösning för hantering. Umbraco är kan tooprovide dessa fördelar med hjälp av hello flexibel plattform som en tjänst (PaaS) modellen som erbjuds av Microsoft Azure.

UaaS aktiverar SaaS kunder toouse Umbraco CMS funktioner som tidigare fanns utanför deras räckvidden. Dessa kunder etableras med en fungerande CMS-miljö som innehåller en produktionsdatabas. Kunder lägga tootwo ytterligare databaser för utveckling och mellanlagringsmiljöer, beroende på deras behov. När en ny miljö krävs tilldelas en automatiserad process kunden en databas automatiskt. hello ny databas är klar i sekunder, eftersom hello databasen har redan etablerats före av Umbraco från ett Azure elastisk pool med tillgängliga databaser (se bild 1).

![Umbraco etablering livscykel](./media/sql-database-implementation-umbraco/figure1.png)

Bild 1. Etablering livscykel för Umbraco som en tjänst (UaaS)

## <a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Azure elastiska pooler och automatisering förenkla distributioner
Med Azure SQL Database och andra Azure-tjänster, Umbraco kunder kan själv etablera sina miljöer och Umbraco enkelt kan övervaka och hantera databaser som en del av ett intuitivt arbetsflöde:

1. Etablera
   
   Umbraco upprätthåller en kapacitet på 200 företablerad tillgängliga databaser från elastiska pooler. När en ny kund registrerar sig för UaaS, ger Umbraco hello kund med en ny CMS-miljö i nära realtid genom att tilldela dem en databas från hello tillgänglighet poolen.
   
   När en pool med tillgänglighet når sin tröskeln, skapas en ny elastisk pool och nya databaser är företablerad toobe tilldelade toocustomers efter behov.
   
   Implementering automatiserad helt med C# bibliotek och Azure Service Bus-köer.
2. Använda
   
   Kunder med någon toothree miljöer (för produktion, mellanlagring och/eller utveckling), varje med en egen databas. Kunddatabaserna är i elastiska pooler, vilket gör att Umbraco tooprovide effektiv skalning utan att behöva etablera tooover.
   
   ![Översikt över Umbraco-projekt](./media/sql-database-implementation-umbraco/figure2.png)
   
   ![Umbraco projektinformation](./media/sql-database-implementation-umbraco/figure3.png)
   
   Figur 2. Umbraco-as-a-Service (UaaS) kunden webbplats, visar en översikt över projekt och information
   
   Azure SQL Database använder Database Transaction Units (Dtu) toorepresent hello relativa styrka krävs för verkliga databastransaktioner. Databaser fungerar normalt med cirka 10 dtu: er för UaaS kunder, men varje har hello elasticitet tooscale på begäran. Det innebär att UaaS kan se till att kunderna alltid har nödvändiga resurser, även under tider med låg belastning. Exempelvis under en senaste söndag kväll sport händelse uppstod en UaaS kund databasen toppar in too100 dtu: er hello spelet hello varaktighet. Azure elastiska pooler möjliggjorde för Umbraco toosupport som hög kräver utan prestandaförsämring.
3. Övervaka
   
   Övervakare för Umbraco databas aktiviteten med kontrollpaneler i hello Azure-portalen, tillsammans med anpassade e-postaviseringar.
4. Haveriberedskap
   
   Azure tillhandahåller två alternativ för katastrofåterställning (DR): aktiv geo-replikering och geo-återställning. hello DR-alternativ som företaget bör välja beror på dess [kontinuitet för företag mål](sql-database-business-continuity.md).
   
   aktiv geo-replikering ger hello snabbaste svar i hello händelse driftstopp. Använder aktiv geo-replikering kan du skapa toofour läsbara sekundärservrar på servrar i olika regioner, och du kan sedan initiera redundans tooany av hello sekundärservrar i hello händelse av ett fel.
   
   Umbraco kräver inte geo-replikering, men den dra nytta av Azure geo-återställning toohelp Kontrollera minsta avbrottstid hello händelse av ett avbrott. GEO-återställning är beroende av säkerhetskopiorna av databasen i geo-redundant Azure-lagring. Som gör att användare toorestore från en säkerhetskopia om det finns ett avbrott i hello primär region.
5. Avinstallation etablera
   
   När en miljö med projektet tas bort tas alla associerade databaser (utveckling, mellanlagring och live) bort under rensning av Azure Service Bus-kö. Detta automatiserad process återställer hello oanvända databaser tooUmbraco elastisk databas tillgänglighet poolen, gör dem tillgängliga för framtida etablering samtidigt maximal användning.

## <a name="elastic-pools-allow-uaas-tooscale-with-ease"></a>Elastiska pooler Tillåt UaaS tooscale utan problem
Genom att utnyttja Azure elastiska pooler, optimera Umbraco prestanda för sina kunder utan tooover eller under etablera. Umbraco har för närvarande nästan 3 000 databaser över 19 elastiska pooler, med hello möjlighet tooeasily skala som behövs tooaccommodate några av sina befintliga 325,000 kunder eller nya kunder som är redo toodeploy en CMS i hello molnet.

I själva verket har enligt tooMorten Christensen, tekniska leda till Umbraco, ”UaaS nu tillväxt ca 30 nya kunder per dag. Är mycket nöjd med hello praktiska funktioner som kan tooprovision nya projekt i sekunder för våra kunder, direkt publicera uppdateringar tootheir live platser från en utvecklingsmiljö med ett klick-distributionen och gör ändringar precis som snabbt om de hitta fel ”.

Om en kund inte kräver en andra och tredje miljö längre, den helt enkelt ta bort dessa miljöer. Som frigör resurser som kan användas för andra kunder som en del av hello Umbraco elastisk databas tillgänglighet poolen.

![Arkitektur för Umbraco-distribution](./media/sql-database-implementation-umbraco/figure4.png)

Bild 3. Arkitektur för distribution av UaaS i Microsoft Azure

## <a name="hello-path-from-datacenter-toocloud"></a>hello sökvägen från datacenter toocloud
När hello Umbraco utvecklare görs först hello beslut toomove tooa SaaS-modellen, känner de att de behöver en kostnadseffektiv och skalbar sätt toobuild ut hello-tjänst.

> ”elastiska pooler är ett perfekt val för våra SaaS-erbjudande eftersom vi kan reglerar kapacitet upp eller ner efter behov. Det är enkelt att etablera, och med vår installationen vi hålla användning maximala ”.
> 
> – Morten Christensen, tekniska Lead Umbraco
> 
> 

”Vill vi toospend tid på att lösa kundernas problem, inte hanterar infrastrukturen. Vi vill toomake det enklare för våra kunder tooget hello största möjliga värde ”står Niels Hartvig, grundare av Umbraco. ”Det anses vara ursprungligen värd för hello servrar oss själva, men kapacitetsplanering skulle ha varit utmaning”. Tillfälligtvis, använder Umbraco inte av alla databasadministratörer som understreck en nyckel/värde-lösning för att använda UaaS.

Ett viktigt mål för hello Umbraco utvecklare var tooprovide UaaS kunder tooprovision miljöer kan snabbt och utan kapacitetsbegränsningar. Men att tillhandahålla en dedikerad värdbaserad tjänst i Umbraco Datacenter skulle ha nödvändiga mängd outnyttjad kapacitet toohandle belastning i bearbetningen. Som skulle ha tänkt att lägga till betydande beräkningsinfrastrukturen som skulle ha varit regelbundet underutnyttjade.

Dessutom hello Umbraco Utvecklingsteamet ville ha en lösning som skulle låta dem tooreuse så mycket av den befintliga koden som möjligt. Som Umbraco developer tillstånd Mikkel Madsen, ”vi är glada med hello Microsoft-utvecklingsverktyg som vi har redan känner till, t.ex. Microsoft SQL Server, Microsoft Azure SQL Database, ASP.net och Internet Information Services (IIS). Innan du investerar i en IaaS eller en PaaS molnet lösning, vill vi toomake till att det skulle stöder våra Microsoft-verktyg och plattformar, så att det skulle toomake omfattande ändringar tooour kodbas ”.

toomeet alla dess villkor, Umbraco sökt efter en partner för molnet med hello följande kvalifikationer:

* Tillräcklig kapacitet och tillförlitlighet
* Stöd för Microsoft-utvecklingsverktyg, så att Umbraco engineers skulle inte tvingas toocompletely tanken utvecklarmiljön
* Förekomst i alla hello geografiska marknader där UaaS konkurrerar (företag behöver tooensure att de kan komma åt sina data snabbt och att deras data lagras på en plats som uppfyller de regionala regelkrav)

## <a name="why-umbraco-chose-azure-for-uaas"></a>Varför Umbraco valde Azure för UaaS
Enligt tooMorten Christensen ”efter att alla våra alternativ vi valt Azure eftersom den har uppfyllt alla våra kriterier, hanterbarhet, skalbarhet toofamiliarity och kostnadseffektivitet. Vi konfigurerar hello miljöer på virtuella Azure-datorer och alla miljöer har sin egen Azure SQL Database-instans med alla hello-instanser i elastiska pooler. Genom att skilja databaser mellan utveckling, mellanlagring och live miljöer vi kan erbjuda kunderna robust prestanda isolering matchade tooscale – enorma win ”.

Morten fortsätter ”innan vi hade tooprovision servrar för webbdatabaser manuellt. Nu har vi inte toothink om den. Allt är automatisk, från etablering toocleanup ”.

Morten är också Grattis med hello skalning funktionerna i Azure. ”elastiska pooler är ett perfekt val för våra SaaS-erbjudande eftersom vi kan reglerar kapacitet upp eller ner efter behov. Det är enkelt att etablera, och med vår installationen vi hålla användning maximala ”. Morten tillstånd, ”Hej enkelhet för elastiska pooler, tillsammans med hello försäkran för service-nivå-baserade dtu: er ger oss hello power tooprovision nya resurspooler på begäran. En av våra större kunder topp nyligen too100 dtu: er i live-miljö. Vår elastiska pooler som med Azure hello kundens databaser med hello resurser som de behövs i realtid utan att behöva toopredict DTU krav. Kort sagt, få våra kunder hello Stäng runt tid att de förväntar sig och vi kan uppfylla våra prestanda servicenivåavtal ”.

Mikkel Madsen summeras i den: ”Vi har tagit hello kraftfulla Azure-algoritm som ansluter en vanliga SaaS scenario (onboarding nya kunder i realtid i skala) tooour programmönster (före etablering av databaser, både utveckling och live) ovanpå hello underliggande tekniken (med Azure Service Bus-köer tillsammans med Azure SQL Database) ”.

## <a name="with-azure-uaas-is-exceeding-customer-expectations"></a>Med Azure överstiger UaaS kundens förväntningar
Sedan väljer Azure som sin partner i molnet, har Umbraco kan tooprovide UaaS kunder med optimal prestanda för innehållshantering, utan investeringar hello IT-resurs från en egen värdbaserade lösning. När Morten som säger ”vi älskar hello developer bekvämlighet och skalbarhet som Azure ger oss och våra kunder är mycket förtjusta i hello funktioner och tillförlitlighet. Generellt sett har en vinner på oss ”!

## <a name="more-information"></a>Mer information
* toolearn mer om Azure elastiska pooler finns [elastiska pooler](sql-database-elastic-pool.md).
* toolearn mer om Azure Service Bus finns [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* toolearn mer om Web och arbetsroller, se [arbetsroller](../fundamentals-introduction-to-azure.md#compute).    
* toolearn mer om virtuella nätverk finns [virtuella nätverk](https://azure.microsoft.com/documentation/services/virtual-network/).    
* toolearn mer information om säkerhetskopiering och återställning, se [företagskontinuitet](sql-database-business-continuity.md).    
* toolearn mer information om hur du övervakar ppols, se [övervakning pooler](sql-database-elastic-pool-manage-portal.md).    
* toolearn mer om Umbraco som en tjänst finns [Umbraco](https://umbraco.com/cloud).

