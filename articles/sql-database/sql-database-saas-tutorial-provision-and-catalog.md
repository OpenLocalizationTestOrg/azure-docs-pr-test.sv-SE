---
title: "aaaProvision nya klienter i en app för flera innehavare som använder Azure SQL Database | Microsoft Docs"
description: "Lär dig hur tooprovision och katalogen nya hyresgäster i hello Wingtip SaaS-app"
keywords: sql database tutorial
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: sstein
ms.openlocfilehash: eb26f523305650c2124e36707d187dfcdad06fcc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="provision-new-tenants-and-register-them-in-hello-catalog"></a>Etablera nya klienter och registrerar dem i hello-katalogen

I kursen får du lära dig om hello etablera och katalogen SaaS mönster och hur de implementeras i hello Wingtip SaaS-program. Skapa och initiera den nya innehavaren databaser och registrerar dem i hello programmets klient-katalogen. hello katalog är en databas som underhåller hello mappning mellan hello SaaS programmet många klienter och deras data. hello katalog spelar en viktig roll dirigera begäranden toohello rätt databas.  

I den här guiden får du lära dig hur man:

> [!div class="checklist"]

> * Etablera en enda ny klient, inklusive stega igenom hur detta implementeras
> * Etablera en batch med ytterligare klienter


den här självstudiekursen, se till att hello följande krav är slutförda toocomplete:

* hello Wingtip SaaS appen har distribuerats. toodeploy på mindre än fem minuter finns [distribuera och utforska hello Wingtip SaaS-program](sql-database-saas-tutorial.md)
* Azure PowerShell ska ha installerats. Mer information finns i [Komma igång med Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-toohello-saas-catalog-pattern"></a>Introduktion toohello SaaS katalog mönster

Det är viktigt tooknow där information för varje klient lagras i ett databasen har säkerhetskopierats flera innehavare SaaS-program. En katalog-databas är används toohold hello mappning mellan varje klient och där deras data lagras i hello SaaS mönster för katalogen. hello Wingtip SaaS appen använder en enskild klient per databas arkitektur, men hello grundläggande mönster för lagring av mappning för klient-till-databasen i en katalog som gäller om en eller flera single-klient databas används.

Varje klient tilldelas en nyckel som identifierar dem i hello-katalogen och som är mappad toohello platsen för hello lämplig databas. I hello Wingtip SaaS app format hello nyckel från en hash av hello innehavarens namn. Detta gör att hello klient del av hello programmet URL toobe används tooconstruct hello nyckel. Andra viktiga klient-system kan användas.  

hello katalog kan hello namnet eller platsen för hello databasen toobe ändras med minimal påverkan på hello program.  I en databasmodell för flera innehavare, detta kan även hantera, flytta' en klient mellan databaser.  hello katalogen kan också vara används tooindicate om en klient eller databasen är offline för underhåll eller andra åtgärder. Detta är utforskade i hello [Återställ enstaka klient kursen](sql-database-saas-tutorial-restore-single-tenant.md).

Dessutom hello katalog, som är i praktiken en management-databas för ett SaaS-program, kan lagra ytterligare klient eller databasen metadata, till exempel hello nivå eller utgåva av en databas, schemaversionen, service-plan eller serviceavtal erbjuds tootenants och annan information som aktiverar programhantering, kundsupport eller devops-processer.  

Hello katalog kan aktivera Databasverktyg utöver hello SaaS-program.  I exemplet Wingtip SaaS hello hello katalog är används tooenable mellan klient-fråga som utforskade i hello [ad hoc-analytics kursen](sql-database-saas-tutorial-adhoc-analytics.md). Hantering av flera databaser utskriftsjobb utforskade i hello [schemat management](sql-database-saas-tutorial-schema-management.md) och [klient analytics](sql-database-saas-tutorial-tenant-analytics.md) självstudier. 

I hello Wingtip SaaS app hello katalog implementeras med hjälp av hello Fragmentera hanteringsfunktionerna i hello [elastisk databas klienten bibliotek (EDCL)](sql-database-elastic-database-client-library.md). Hej EDCL gör det möjligt för ett program toocreate, hantera och använda en databasen har säkerhetskopierats Fragmentera karta. En Fragmentera-mappning innehåller en lista över delar (databaser) och hello mappning mellan nycklar (klienter) och databaser.  EDCL funktioner kan användas under klientorganisationens Etableringsläge toocreate hello poster i hello Fragmentera mappning från program eller PowerShell-skript och program tooefficiently ansluter toohello rätt databas. EDCL cachelagras information toominimize hello trafik toohello katalog databas och snabba upp hello program.  

> [!IMPORTANT]
> hello mappningsdata är tillgängliga i hello katalogdatabasen men *inte redigera den*! Redigera endast mappningsdata med API:er för klientbiblioteket för den elastiska databasen. Direkt manipulering hello mappade data risker skadas hello-katalogen och stöds inte.


## <a name="introduction-toohello-saas-provisioning-pattern"></a>Introduktion toohello SaaS etablering mönster

När onboarding en ny klient i ett SaaS-program som använder måste en enskild klient databasmodell en ny klient-databas etableras.  Det måste skapas i hello lämplig plats och tjänstnivån initieras med lämpligt schema- och data och registreras sedan på hello-katalogen under hello lämpliga klientnyckel.  

Olika strategier kan vara används toodatabase etablering, vilket kan omfatta SQL-skript körs, distribuera en bacpac, eller kopiera en 'gyllene' mall-databas.  

hello etablerar du anger måste vara comprehended i din övergripande schemat hanteringsstrategi som skall se till att nya databaser är etablerad med hello senaste schema.  Detta är utforskade i hello [schemat management kursen](sql-database-saas-tutorial-schema-management.md).  

hello Wingtip SaaS app tillhandahåller nya klienter genom att kopiera en gyllene databas med namnet basetenantdb som distribuerats på hello katalogserver.  Etablering kan vara integrerade i hello programmet som en del av en registrering och/eller stöds offline med hjälp av skript. Den här självstudiekursen kommer utforska etablering med hjälp av PowerShell. hello etablering skript kopiera hello basetenantdb toocreate en ny klient-databas i en elastisk pool och sedan initiera med klient-specifik information och registrera det i hello katalog Fragmentera kartan.  I hello exempelapp databaser ges namn baserat på hello-klient, men detta är inte en viktig del av hello mönster – hello med hello katalog kan någon databas med namnet toobe som tilldelats toohello. + 


## <a name="get-hello-wingtip-application-scripts"></a>Hämta hello Wingtip programskript

hello Wingtip SaaS-skript och programmets källkod är tillgängliga i hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-lagringsplatsen. [Steg toodownload hello Wingtip SaaS skript](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).


## <a name="provision-and-catalog-detailed-walkthrough"></a>Etablera och katalogisera detaljerad genomgång

toounderstand hur hello Wingtip programmet innehåller ny klient etablering, lägga till en brytpunkt och gå igenom hello arbetsflöde vid etablering av en klient:

1. Öppna... \\Learning moduler\\ProvisionAndCatalog\\_Demo-ProvisionAndCatalog.ps1_ och ange hello följande parametrar:
   * **$TenantName** = hello namnet på hello ny plats (till exempel *Bushwillow blått*).
   * **$VenueType** = hello fördefinierade platsen typer: *blått*, classicalmusic, webbsidor, jazz, judo, motorracing, multipurpose, opera, rockmusic, fotboll.
   * **$DemoScenario** = **1**, Ställ in för**1** för*etablera en enskild klient*.

1. Lägga till en brytpunkt genom att placera markören på raden 48, hello raden: *ny klient '*, och tryck på **F9**.

   ![brytpunkt](media/sql-database-saas-tutorial-provision-and-catalog/breakpoint.png)

1. Tryck på toorun hello-skript **F5**.

1. När hello skriptkörningen slutar vid hello brytpunkt, trycker du på **F11** toostep till hello kod.

   ![brytpunkt](media/sql-database-saas-tutorial-provision-and-catalog/debug.png)



Spåra körning av hello skript med hjälp av hello **felsöka** menyalternativen - **F10** och **F11** toostep via eller i hello kallas för funktioner. Mer information om felsökning av PowerShell-skript finns [Tips om att arbeta med och felsöka PowerShell-skript](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


hello följande är inte tooexplicitly följer stegen, men en förklaring av hello arbetsflöde du steg för steg när du felsöker hello skript:

1. **Importera hello SubscriptionManagement.psm1** modul som innehåller funktioner för att logga in tooAzure och markera hello Azure-prenumeration du arbetar med.
1. **Importera hello CatalogAndDatabaseManagement.psm1** modul som tillhandahåller en katalog och klient-nivå abstraction över hello [Fragmentera Management](sql-database-elastic-scale-shard-map-management.md) funktioner. Detta är ett viktigt modul som kapslar in mycket hello katalog mönster och är värt att utforska.
1. **Hämta konfigurationsinformationen**. Gå till Get-konfiguration (med F11) och se hur hello programmets konfiguration har angetts. Resursnamn och andra app-specifik värden definieras här, men ändra inte dessa värden tills du är bekant med hello skript.
1. **Hämta hello katalogobjektet**. Gå till Get-katalog composes och returnerar ett objekt i katalogen som används i hello på högre nivå skript.  Den här funktionen används Fragmentera hanteringsfunktioner som importeras från **AzureShardManagement.psm1**. hello katalogobjektet består av hello följande:
   * $catalogServerFullyQualifiedName konstrueras hello standard stam plus ditt användarnamn: _katalog -\<användare\>. database.windows.net_.
   * $catalogDatabaseName hämtas från hello config: *tenantcatalog*.
   * $shardMapManager objektet har initierats från hello katalog-databasen.
   * $shardMap-objektet har initierats från hello *tenantcatalog* Fragmentera kartan i hello katalogdatabasen.
   Ett objekt i katalogen är består returnerade och i hello på högre nivå skript.
1. **Beräkna hello nya klientnyckel**. En hash-funktionen är används toocreate hello klientnyckel från hello innehavarens namn.
1. **Kontrollera om det redan finns hello klientnyckel**. hello katalogen kontrolleras tooensure hello nyckeln är tillgänglig.
1. **hello klient databasen har etablerats med ny TenantDatabase.** Använd **F11** toostep i och se hur hello databasen är etablerats med hjälp av en [Azure Resource Manager-mall](../azure-resource-manager/resource-manager-template-walkthrough.md).

hello databasnamnet konstrueras utifrån hello klient namnet toomake den avmarkera vilka Fragmentera tillhör toowhich klient. (Andra strategier för namngivning av databasen enkelt kunde användas.) + En Resource Manager-mall är används toocreate en klient-databas genom att kopiera en gyllene databas (baseTenantDB) på hello katalogserver. En annan metod kan vara en tom databas för toocreate och sedan starta den genom att importera en bacpac eller tooexecute en Initieringsskript från en känd plats.  

hello Resource Manager-mall finns i hello ...\Learning Modules\Common\ mapp: *tenantdatabasecopytemplate.json*

När hello klient-databasen har skapats är det sedan ytterligare **initieras med hello plats (klient) namn och typ för hello platsen**. Övrig initialisering kan också göras här.

Hej **klient databas har registrerats i katalogen hello** med *Lägg till TenantDatabaseToCatalog* använder hello klientnyckel. Använd **F11** toostep hello detaljer:

* hello katalog databas läggs toohello Fragmentera karta (hello lista över kända databaser).
* hello mappning som länkar hello nyckelvärdet toohello Fragmentera skapas.
* Ytterligare metadata (namn hello platsen) om hello klienten läggs toohello hyresgäster tabellen i hello-katalogen.  hello hyresgäster tabellen ingår inte i hello ShardManagement schema och installeras inte som hello EDCL.  I följande tabell visas hur hello katalogdatabasen kan utökas toosupport ytterligare programspecifika data.   


När etableringen är klar körning returnerar toohello ursprungliga *Demo-ProvisionAndCatalog* skript, vilket öppnar hello **händelser** sidan för hello ny klient i hello webbläsare:

   ![evenemang](media/sql-database-saas-tutorial-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Etablera en grupp med klienter

Den här övningen etablerar en batch med 17 innehavare. Det rekommenderas att du etablerar den här gruppen med klienter innan du startar andra Wingtip SaaS-självstudier, så det finns fler än några databaser toowork med.

1. Öppna... \\Learning moduler\\ProvisionAndCatalog\\*Demo-ProvisionAndCatalog.ps1* i hello *PowerShell ISE* och ändra hello *$ DemoScenario* parametern too3:
   * **$DemoScenario** = **3**, ändra också**3** för*etablera en grupp med klienter*.
1. Tryck på **F5** och kör hello-skript.

hello skript distribuerar en batch med ytterligare klienter. Den använder en [Azure Resource Manager-mall](../azure-resource-manager/resource-manager-template-walkthrough.md) som styr hello batch och sedan delegerar etablering av varje databas tooa länkade mall. Med hjälp av mallar på så sätt kan Azure Resource Manager toobroker hello etableringsprocessen för skriptet. Mallar etablera databaser parallellt där den kan och hanterar försök om det behövs, optimera hello övergripande processer. Hej skriptet idempotent så om den misslyckas eller avbryts av någon anledning, körs det igen.

### <a name="verify-hello-batch-of-tenants-successfully-deployed"></a>Kontrollera hello grupp med klienter som har distribuerats

* Öppna hello *tenants1* servern genom att bläddra tooyour listan över servrar i hello [Azure-portalen](https://portal.azure.com), klickar du på **SQL-databaser**, och kontrollera hello batch 17 nya databaser är nu i listan hello:

   ![databaslista](media/sql-database-saas-tutorial-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Andra etableringsmönster

Andra etableringsmönster som inte ingår i den här guiden inkluderar:

**Företablering av databaser.** hello före etablering mönster utnyttjar hello faktum att databaser i en elastisk pool inte Lägg till extra kostnad. Faktureringen är för hello elastiska poolen, hello inte databaser och inaktiv databaser förbrukar några resurser. Genom före etablering databaser i poolen och tilldela dem vid behov, minskas tid för klient onboarding avsevärt. hello antalet databaser som etableras i förväg kan justeras som behövs tookeep en buffert som är lämplig för hello förväntat etablering hastighet.

**Automatisk etablering.** I hello Automatisk etablering mönster en dedikerad etablering tjänst är används tooprovision servrar, pooler och databaser automatiskt efter behov – inklusive före etablering databaser i elastiska pooler om så önskas. Och om databaser Frigör uppdrag och tas bort, luckor i elastiska pooler kan fyllas av hello Etablerar tjänsten efter behov. Dessa tjänster kan vara enkla eller avancerade – till exempel hantera etablering över flera områden, och kan ställa in geo-replikering automatiskt om den strategin som används för katastrofåterställning. Ett klientprogram eller skript med hello Automatisk etablering mönster skulle skicka en allokering begäran tooa kön toobe bearbetas av hello etableras och skulle sedan avsöka hello tjänsten toodetermine avslutas. Om före etablering används, skulle begäranden hanteras snabbt med hello-tjänst som hanterar etablering av en ersättning-databas som körs i bakgrunden hello.



## <a name="next-steps"></a>Nästa steg

I den här guiden lärde du dig hur man:

> [!div class="checklist"]

> * Etablera en enskild ny klient
> * Etablera en batch med ytterligare klienter
> * Steg hello detaljer om etablering klienter och registrera dem till hello katalog

Försök hello [prestanda övervakning kursen](sql-database-saas-tutorial-performance-monitoring.md).

## <a name="additional-resources"></a>Ytterligare resurser

* Ytterligare [självstudier som bygger på hello Wingtip SaaS-program](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Klientbibliotek för elastiska databaser](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library)
* [Hur tooDebug skript i Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
