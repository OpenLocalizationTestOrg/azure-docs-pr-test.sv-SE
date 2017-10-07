---
title: "aaaDesign mönster för flera innehavare SaaS-program och Azure SQL Database | Microsoft Docs"
description: "Den här artikeln innehåller information om hello och vanliga arkitektur datamönster för flera innehavare databasprogram som körs i en molnmiljö behöver tooconsider och hello olika kompromisser som associeras med dessa mönster. Här förklaras också hur Azure SQL Database med elastiska pooler och elastiska verktyg hjälpa uppfylla dessa krav på ett sätt utan kompromisser."
keywords: 
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 1dd20c6b-ddbb-40ef-ad34-609d398d008a
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-design
ms.date: 02/01/2017
ms.author: srinia
ms.openlocfilehash: a4b58935b08cb78792e65a675d680de708b709fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-patterns-for-multi-tenant-saas-applications-and-azure-sql-database"></a>Designmönster för SaaS-program med flera klienter och Azure SQL Database
I den här artikeln får information du om hello krav och vanliga arkitektur datamönster för flera innehavare programvara som en tjänst (SaaS) databasprogram som körs i en molnmiljö. Det förklarar också hello faktorer du behöver tooconsider och hello Eftergifter eller av annan designmönster. Elastiska pooler och elastiska verktyg i Azure SQL Database kan hjälpa dig enligt dina särskilda behov utan att kompromissa med andra mål.

Utvecklare kan ibland göra alternativ som fungerar mot sina långsiktiga intressen när de skapar innehavare modeller för hello data nivåerna för program med flera klienter. Inledningsvis på minst kan en utvecklare uppfattar enkel utveckling och lägre cloud service provider kostnader som viktigare än klient isolering eller hello skalbarhet för ett program. Det här alternativet kan leda toocustomer tillfredsställande frågor och kostsamma kursen-korrigering senare.

Ett program med flera innehavare är ett program som finns i en molnmiljö och som ger hello samma uppsättning tjänster toohundreds eller tusentals klienter som inte delar eller se varandras data. Ett exempel är en SaaS-program som tillhandahåller tjänster tootenants i en miljö med värd i molnet.

## <a name="multi-tenant-applications"></a>Program med flera klienter
I program med flera klienter, kan data och arbetsbelastningen enkelt partitioneras. Du kan partitionera data och arbetsbelastning, till exempel längs klient gränser, eftersom de flesta förfrågningar sker inom hello gränserna för en klient. Denna egenskap är inbyggd i hello data och hello arbetsbelastning och den prioriterar hello programmet mönster i den här artikeln.

Utvecklare kan du använda den här typen av program över hello hela spektrum av molnbaserade program, inklusive:

* Partner databasprogram som har gått över toohello molnet som SaaS-program
* SaaS-program som skapats för hello moln från hello bakgrund
* Direkt, kund-riktade program
* Medarbetare enterprise program

SaaS-program som är utformade för hello moln och de med rötter som partner databasprogram normalt program med flera klienter. Dessa SaaS-program ger ett särskilda program som en tjänst tootheir klienter. Klienter kan komma åt hello programtjänsten och ägas associerade data som lagras som en del av programmet hello. Men tootake nytta av hello fördelarna med SaaS klienter måste överlämnande viss kontroll över sina egna data. De litar hello SaaS service provider tookeep sina data säkert och isolerade från andra klienter data. Exempel på den här typen av flera innehavare SaaS-program är MYOB, SnelStart och Salesforce.com. Var och en av dessa program kan partitioneras längs klient gränser och support hello programmet designmönster diskuterar vi i den här artikeln.

Program som ger direkt service toocustomers och tooemployees i en organisation (som ofta anges tooas användare, i stället klienter) är en annan kategori på hello flera innehavare programmet spektrumet. Kunder som prenumererar toohello service och äger inte hello data som hello-leverantör samlar in och lagrar. Leverantörer har mindre strikta krav tookeep sina kunders data isoleras från varandra utöver government obligatorisk sekretessbestämmelser. Exempel på den här typen av program som ansluter till en kund med flera innehavare är media innehållsleverantörer som Netflix, Spotify och Xbox LIVE. Andra exempel på enkelt partitionable program är kund-riktade, Internet-skala program eller Sakernas Internet (IoT)-program i vilken varje kund eller enheten kan fungera som en partition. Partitionsgränserna avgränsa användare och enheter.

Inte alla program partition enkelt längs en egenskap som innehavare, kunder, användare eller enhet. En komplex företagsresurser (ERP) programmet har till exempel produkter, order och kunder. Det har vanligtvis ett sammansatt schema med tusentals hög sammankopplade tabeller.

Ingen enskild partition strategi kan tillämpa tooall tabeller och fungerar över arbetsbelastningen för ett program. Den här artikeln fokuserar på program med flera klienter som har enkelt partitionable data och arbetsbelastningar.

## <a name="multi-tenant-application-design-trade-offs"></a>Flera innehavare program design avvägningarna
hello designmönstret som en flera innehavare programutvecklaren väljer vanligtvis baserat på ersättning av hello följande faktorer:

* **Klientisolering**. hello utvecklaren tooensure att ingen klient har obehöriga tooother klienternas data. krav för isolering av hello utökar tooother egenskaper, som ger skydd från störningar grannar, som kan toorestore en klients data och implementera klient-specifika anpassningar.
* **Molnet resurskostnader**. Ett SaaS-program måste toobe kostnaden konkurrenskraftig. En programutvecklare för flera innehavare kan välja toooptimize för lägre kostnad hello användningen av molnresurser, till exempel lagring och datorkostnader.
* **Enkel DevOps**. En flera innehavare programutvecklaren måste tooincorporate isolering skydd, underhålla, övervakning av sina program och databasschemat hello och felsökning av problem med klient. Komplexitet för utveckling och drift översätter direkt tooincreased kostnad och utmaningar med klienten tillfredsställande sätt.
* **Skalbarhet**. hello möjlighet tooincrementally lägga till fler klienter och kapacitet för klienter som behöver det är absolut nödvändigt tooa lyckade SaaS-åtgärd.

Dessa faktorer har avvägningarna jämfört med tooanother. hello billigaste moln erbjudande inte kanske kan erbjuda hello enklaste utvecklingsarbetet. Det är viktigt för utvecklare toomake informeras val om dessa alternativ och deras avvägningarna hello designprocessen för programmet.

Ett populärt development mönster är toopack flera klienter i en eller några databaser. hello fördelarna med den här metoden är en lägre kostnad eftersom du betalar för några databaser och hello enkelhet arbetar med ett begränsat antal databaser. Men över tiden, kommer en SaaS flera innehavare programutvecklaren inser att detta val har betydande downsides i klientisolering och skalbarhet. Om klientisolering blir viktigt, är extra arbete krävs tooprotect klientdata i den delade lagringen från obehörig åtkomst och störningar grannar. Den här extra arbete kan avsevärt öka utvecklingsarbete och isolering underhållskostnaderna. På samma sätt, om det inte krävs för att lägga till klienter, det här designmönstret kräver normalt expertis tooredistribute klientdata via datanivå för databaser tooproperly skala hello av ett program.  

Klientisolering ofta är ett grundläggande krav i SaaS flera innehavare program som tillgodoser toobusinesses och organisationer. En utvecklare kan frestas av upplevd fördelar i enkelhet och kostnaden jämfört med klientisolering och skalbarhet. Kompromissen kan vara komplicerat och dyrt när hello service växer och krav för isolering av innehavare blir mer viktiga och hanterade på programnivå hello. I program med flera klienter som tillhandahåller en tjänst direkt, konsumentinriktade toocustomers, men vara klientisolering lägre prioritet än Optimera för resurskostnader för molnet.

## <a name="multi-tenant-data-models"></a>Flera innehavare datamodeller
Vanliga design praxis för att placera klientdata följer tre olika modeller som illustreras i bild 1.

![flera innehavare appens datamodeller](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)

Bild 1: Vanliga design praxis för flera innehavare datamodeller

* **Databasen per klient**. Varje innehavare har en egen databas. Alla klient-specifika data är begränsat toohello klientens databas och isoleras från andra klienter och deras data.
* **Delade databasen delat**. Flera innehavare dela någon av flera databaser. En specifik uppsättning klienter tilldelas tooeach databasen med hjälp av en partitioneringsstrategi som hash, intervall eller lista partitionering. Den här strategin för distribution av data är ofta hänvisas tooas horisontell partitionering.
* **Delad enskild databas**. En enda ibland stora databas innehåller data för alla klienter som är skiljas åt i en klient-ID-kolumn.

> [!NOTE]
> En programutvecklare kan välja tooplace olika klienter i en annan databas scheman och sedan använda hello schemat namn toodisambiguate hello olika klienter. Vi rekommenderar inte den här metoden eftersom det kräver normalt hello användning av dynamisk SQL och den kan inte vara effektiva i planen cachelagring. I hello resten av den här artikeln fokusera vi på hello delade tabell tillvägagångssätt för den här kategorin för flera innehavare program.
> 
> 

## <a name="popular-multi-tenant-data-models"></a>Populära datamodeller flera innehavare
Det är viktigt tooevaluate hello olika typer av flera innehavare datamodeller vad gäller hello program design kompromissa vad vi redan har identifierats. Dessa faktorer hjälpa karaktäriserar hello tre vanligaste flera innehavare datamodeller ovan och deras databasanvändningen som visas i bild 2.

* **Isolering**. hello grad av isolering mellan klienter kan vara ett mått på hur mycket klientisolering uppnår en datamodell.
* **Molnet resurskostnader**. hello mängden resursdelning mellan klienter kan optimera resurskostnader för molnet. En resurs kan definieras som hello beräkning och lagring kostnad.
* **DevOps kostnaden**. hello enkel programutveckling, distribution och hantering minskar totalkostnaden för SaaS-åtgärden.  

I bild 2 visar hello Y-axeln hello nivå av klientisolering. hello X-axeln visar hello nivå av resursdelning. hello grå diagonal pil hello mitten anger DevOps kostnader, enhetsfack tooincrease eller minska hello riktning.

![Designmönster för populära program för flera innehavare](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png)

Bild 2: Populära flera innehavare datamodeller

hello nedre högra kvadrant i bild 2 visar ett programmönster som använder en potentiellt stora, delad databas och hello delade tabellen (eller separat schema) metod. Det är bra för resursdelning eftersom alla klienter använder hello samma databas resurser (CPU, minne, indata/utdata) i en enskild databas. Men klientisolering är begränsad. Du kan behöva tootake ytterligare steg tooprotect klienter från varandra i hello programnivå. Dessa ytterligare steg kan avsevärt öka hello DevOps kostnaden för utveckling och hantering av programmet hello. Skalbarhet begränsas av hello skala hello maskinvara som värd hello-databasen.

hello nedre vänstra kvadrant i bild 2 visar flera innehavare delat över flera databaser (vanligtvis olika typer av maskinvara skalningsenheter). Varje databas är värd för en delmängd av klienter, vilket löser hello skalbarhet problem för andra. Om det krävs mer kapacitet för flera klienter, kan du lätt lägga hello klienter på nya databaser som har allokerats toonew maskinvara skalenheter. Dock minskas hello mängden resursdelning. Endast klienter placerad hello samma skalenheter dela resurser. Den här metoden ger liten förbättring tootenant isolering eftersom många hyresgäster fortfarande är samordnad utan skyddas automatiskt från varandras åtgärder. Programmet komplexitet förblir hög.

hello övre vänstra kvadrant i bild 2 är hello tredje metod. Varje klient data placeras i en egen databas. Den här metoden har bra klientisolering egenskaper men begränsar resursdelning när varje databas har sin egen dedicerade resurser. Den här metoden är bra om alla klienter har förutsägbar arbetsbelastningar. Om klienternas arbetsbelastningar blir mindre förutsägbar inte kan hello-providern optimera resursdelning. Unpredictability är vanligt för SaaS-program. hello-providern måste antingen etablera över toomeet krav eller lägre resurser. Antingen åtgärden resulterar i högre kostnader eller lägre klient tillfredsställande sätt. En högre grad av resursdelning över klienter blir önskvärt toomake hello lösning är kostnadseffektiv. Ökande hello antal databaser också ökar DevOps kostnaden toodeploy och underhålla hello program. Trots dessa problem ger den här metoden hello bästa och enklaste isolering för klienter.

Dessa faktorer påverkar också hello designmönstret en kund väljer:

* **Ägare till klientdata**. Ett program där klienter Behåll ägarskapet av sina egna data ökar hello mönster för en enskild databas per klient.
* **Skalning**. Ett program som riktar sig till hundratals tusentals eller miljontals hyresgäster prioriterar Databasdelning metoder, till exempel horisontell partitionering. Isoleringskrav för kan fortfarande medföra utmaningar.
* **Värde och företag modellen**. Om ett program per klientorganisation intäkter om klokt att små (mindre än dollar), isoleringskrav blir mindre viktigt och en delad databas. Om intäkter per klient är några kronor eller mer, är en databas per klient modell mer möjligt. Det kan minska utvecklingskostnaderna för.

Hello design avvägningarna visas i figur 2 får måste en perfekt modell för flera klienter tooincorporate bra klient isolering egenskaper med optimala resursdelning mellan klienter. Den här modellen passar i hello kategori som beskrivs i hello övre högra kvadranten på bild 2.

## <a name="multi-tenancy-support-in-azure-sql-database"></a>Stöd för flera innehavare i Azure SQL Database
Azure SQL Database stöder alla program med flera innehavare mönster som beskrivs i bild 2. Det stöder också ett programmönster som kombinerar bra resursdelning med elastiska pooler och hello isolering fördelarna med hello databas per klient närmar sig (se hello övre högra kvadrant i bild 3). Elastisk Databasverktyg och funktioner i SQL-databas kan du minska hello kostnaden toodevelop och använda ett program som har många databaser (visas i hello skuggade området i bild 3). Dessa verktyg kan hjälpa dig att skapa och hantera program som använder någon av hello flera databasen mönster.

![Mönster i Azure SQL-databas](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png)

Bild 3: Flera innehavare programmet mönster i Azure SQL Database

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Databasen per klient modellen med elastiska pooler och verktyg
Elastiska pooler i SQL-databas kombinera klientisolering med resursdelning mellan klient databaser toobetter stöd hello databas per klient metod. SQL-databasen är en lösning för nivån för SaaS-providers som skapar program med flera klienter. hello belastningen av resursdelning mellan klienter flyttas från hello layer toohello databasen service programnivå. hello komplexitet för att hantera och skicka en fråga i större skala över databaser förenklas med elastiska jobb, elastisk frågan, elastisk transaktioner och hello klientbibliotek för elastisk databas.

| Programvarukrav | Funktioner för SQL-databas |
| --- | --- |
| Klientisolering och resursdelning |[Elastiska pooler](sql-database-elastic-pool.md): allokerar en pool av resurser för SQL-databas och dela hello resurser mellan olika databaser. Enskilda databaser kan också hämta så mycket resurser från hello pool som behövs tooaccommodate kapacitet efterfrågan ökar kraftigt och tillfälligt på grund av toochanges i klienternas arbetsbelastningar. hello elastisk pool själva kan skalas upp eller ned efter behov. Elastiska pooler ger även enkel hantering och övervakning och felsökning på hello poolen nivå. |
| Enkel DevOps över databaser |[Elastiska pooler](sql-database-elastic-pool.md): som tidigare nämnts. |
| | [Elastisk frågan](sql-database-elastic-query-horizontal-partitioning.md): frågan över databaser för rapportering eller mellan klient analys. |
| | [Elastiska jobb](sql-database-elastic-jobs-overview.md): Paketera och distribuera på ett tillförlitligt sätt databasunderhåll eller databasschemat ändras toomultiple databaser. |
| | [Elastisk transaktioner](sql-database-elastic-transactions-overview.md): tooseveral databaser ändras i ett atomiskt och isolerade sätt. Elastisk transaktioner behövs när program behöver ”allt eller inget” garantier över flera databasåtgärder. |
| | [Klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md): hantera distributioner av data och mappa hyresgäster toodatabases. |

## <a name="shared-models"></a>Delade modeller
Enligt beskrivningen tidigare, för de flesta SaaS-providers, kan en delad modell-metod medföra problem med klient isolering problem och svårigheter med programutveckling och underhåll. Dock för flera innehavare program som innehåller en tjänst direkt tooconsumers, klient kanske-isoleringskrav inte med hög prioritet som minimerar kostnaden. De kanske kan toopack klienter i en eller flera databaser i en hög densitet tooreduce kostnader. Delade databasen modeller med hjälp av en enskild databas eller flera delat databaser kan resultera i ytterligare effektivitet i resurskostnader för fildelning och övergripande. Azure SQL Database tillhandahåller vissa funktioner som hjälper kunder att bygga isolering för ökad säkerhet och hantering i större skala i hello datanivå.

| Programvarukrav | Funktioner för SQL-databas |
| --- | --- |
| Säkerhetsfunktioner för isolering |[Säkerhet på radnivå](https://msdn.microsoft.com/library/dn765131.aspx) |
| [Databasschemat](https://msdn.microsoft.com/library/dd207005.aspx) | |
| Enkel DevOps över databaser |[Elastisk fråga](sql-database-elastic-query-horizontal-partitioning.md) |
| | [Elastiska jobb](sql-database-elastic-jobs-overview.md) |
| | [Elastisk transaktioner](sql-database-elastic-transactions-overview.md) |
| | [Klientbibliotek för elastiska databaser](sql-database-elastic-database-client-library.md) |
| | [Elastisk databas dela och slå samman](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Sammanfattning
Krav för isolering av innehavare är viktiga för de flesta SaaS flera innehavare program. hello bästa alternativet tooprovide isolering leans kraftigt mot hello databas per klient metod. hello kräver andra två metoder investeringar i lager för komplexa program som kräver skicklig development personal tooprovide isolering, vilket ökar avsevärt kostnader och risker. Om isoleringskrav inte är klara för tidigt i hello service utveckling, kan vara onlineåterställningspunkter dem ännu mer kostsamma i hello första två modeller. hello huvudsakliga nackdelarna som är associerade med hello databas per klient modellen är relaterade tooincreased moln resurskostnader på grund av tooreduced delning och underhåll och hantering av flera databaser. SaaS-programutvecklare har ofta svårt när de gör dessa avvägningarna.

Även om avvägningarna kan vara större hinder med de flesta molntjänstleverantörer för databasen, eliminerar Azure SQL Database hello barriärer med elastisk pool och funktioner för elastisk databas. SaaS-utvecklare kan kombinera hello isolering egenskaperna för en databas per klient-modell och optimera resurs delning och hello förbättringar av många databaser med elastiska pooler och associerade verktyg.

Flera innehavare programleverantörer som har inga krav för isolering av innehavare och som kan packa klienter i en databas på en hög densitet kanske upptäcker att delade data modeller resultatet i ytterligare effektivitet för resursdelning och minska kostnaderna. Azure SQL Database elastisk Databasverktyg, horisontell partitionering bibliotek och säkerhetsfunktioner hjälpa SaaS-providers bygga och hantera program med flera klienter.

## <a name="next-steps"></a>Nästa steg
[Kom igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md) med en exempelapp som visar hello klientbiblioteket.

Skapa en [elastisk pool anpassade instrumentpanel för SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) med en exempelapp som använder elastiska pooler för en kostnadseffektiv och skalbar databaslösning.

Använda hello Azure SQL Database-verktyg för[migrera befintliga databaser tooscale ut](sql-database-elastic-convert-to-use-elastic-tools.md).

en elastisk pool med toocreate hello Azure portal, se [skapa en elastisk pool](sql-database-elastic-pool-manage-portal.md).  

Lär dig hur för[övervaka och hantera en elastisk pool](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Distribuera och utforska ett program med flera innehavare som använder Azure SQL Database - Wingtip SaaS](sql-database-saas-tutorial.md)
* [Vad är en Azure elastisk pool?](sql-database-elastic-pool.md)
* [Skala ut med Azure SQL Database](sql-database-elastic-scale-introduction.md)
* [program med flera klienter med elastiska Databasverktyg och säkerhet på radnivå](sql-database-elastic-tools-multi-tenant-row-level-security.md)
* [Autentisering i flera innehavare appar med hjälp av Azure Active Directory och OpenID Connect](../guidance/guidance-multitenant-identity-authenticate.md)
* [Tailspin Surveys-programmet](../guidance/guidance-multitenant-identity-tailspin.md)


## <a name="questions-and-feature-requests"></a>Frågor och funktionsbegäranden

Frågor, visa oss i hello [SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Lägg till en funktionsbegäran i hello [SQL-databas Feedbackforum](https://feedback.azure.com/forums/217321-sql-database/).

