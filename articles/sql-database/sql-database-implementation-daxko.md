---
title: aaaAzure Fallstudie SQL Database Azure - Daxko/CSI | Microsoft Docs
description: "Lär dig mer om hur Daxko/CSI använder SQL-databas tooaccelerate dess utvecklingscykeln och tooenhance dess kundtjänst och prestanda"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 00c8a713-f20c-4d6b-b8b7-0c1b9ba5f05b
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 3e3d58a1d9c3c919fc0e4cdb2765f680719c19d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="daxkocsi-used-azure-tooaccelerate-its-development-cycle-and-tooenhance-its-customer-services-and-performance"></a>Daxko/CSI används Azure tooaccelerate dess utvecklingscykeln och tooenhance dess kundtjänst och prestanda
![Daxko/CSI-logotyp](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Daxko/CSI programvara inför en utmaning: dess kundbas för lämplighet och fritid växer snabbt, tack toohello genomförandet av dess omfattande enterprise-lösning, men behåller med hello IT-infrastruktur som behövs för den växande kundbas testning hello företagets IT-personal. hello företag har allt mer begränsad av ökande operations kostnader, särskilt för att hantera växande databaserna. Arbetet som operations värre, klippa ut till Utvecklingsresurser för nya initiativ som nya mobility-funktioner för hello företagets programvara.

Enligt tooDavid Molina Director av produktutveckling på Daxko/CSI Azure angivna CSI programvara med hello plattform som en tjänst (PaaS) modellen toosimplify databashantering krävdes bättre skalbarhet och frigör resurser toofocus på programvara i stället för ops. ”Azure SQL Database har ett bra alternativ för oss. Slipper tooworry om att underhålla en SQL Server, ett failover-kluster och alla hello andra infrastrukturbehov var perfekt för oss ”.

Sedan migrerar tooAzure CSI programvara måste en driftspersonal av bara två toomanage över 600 kunddatabaser. hello företaget använder Azure SQL Database elastiska pooler toomove kunddatabaser baserat på storleken och behöver.

Molina fortsätter ”våra kunder tyckte hello ändra omedelbart. Innan du elastiska pooler hade de ibland tidsgränser och andra problem under burst perioder. Med Azure elastiska pooler de burst efter behov och använda hello programvaran utan problem ”.

Dessutom tooimproving prestanda för kunder, Azure elastiska pooler frigjort CSI programvaruresurser toofocus på utveckling av nya tjänster och funktioner, i stället för att hantera åtgärder och hantering. Resurserna IT hjälpt CSI programvara förbättra enterprise programvaran erbjuder, SpectrumNG, toohelp engagera gymmet medlemmar, förbättra personaleffektiviteten och ge personal och medlemmar mobil åtkomst för interaktiva aktiviteter och meddelanden i realtid.

Azure har också hjälpt CSI programvara påskynda och förbättra hello utveckling och kvalitet säkerhet (QA) cykeln genom att aktivera automatiseringsalternativ. Med Azure hello företags-implementering kan bygga chefer paketera komponenter med hello klickar på en knapp. Eftersom Molina beskriver ”som en del av hello utgivningscykeln, QA är nu kan toodeploy tooa testmiljö i Azure, vilket nära efterliknar våra produktions-stacken. Vi kan distribuera versioner omedelbart tooour dev miljö toovet ändras. Som är en stor win för oss, eftersom vi inte hade paritet för testning innan ”.

## <a name="offloading-toohello-cloud"></a>Avlastning toohello moln
Innan du flyttar toohello moln hade CSI programvara har skapat in sin egen multitenant infrastruktur i ett lokalt datacenter i Houston. Som hello företagets utökad det står inför ökande growing framgång från inköp, etablering och underhåll av all hello maskinvara och programvara som behövs toosupport sina kunder. IT-avdelningen för personal toohandle blev en annan flaskhals som ledde tooa nedgång i etablera nya resurser och lansera nya tjänster toocustomers.

CSI programvara undersökt molnalternativ för att ta bort den kostnader, så att den kan fokusera på koden, i stället för åtgärderna. hello företagets identifierade att många av hello översta molntjänstleverantörer erbjuder infrastruktur som en tjänst (IaaS) lösningar som behöver fortfarande en stora IT-personal toomanage hello IaaS-stacken. I hello slutet fastställs CSI programvara att hello Azure PaaS lösning var hello bäst lämpade för dess behov. Molina förklarar ”Azure hämtar hello maskin- och programvaran utanför hello sätt, så vi kan fokusera på vår programvara erbjudanden samtidigt minska vår IT-omkostnaderna”.

## <a name="making-hello-transition-tooazure"></a>Gör hello övergången tooAzure
När du har valt Azure för dess PaaS-lösning, började CSI programvara migrera dess backend-infrastruktur och databaser toohello moln. Tidigare tooAzure SpectrumNG kunder behövs tooinstall ett klientprogram som kommunicerat med en Windows Communication Foundation (WCF)-tjänst på hello serverdel. Enligt tooMolina, ”även om vissa kunder finns allt innehåll i sina egna datacenter, vi byggt ut hello produkten toobe multitenant. Vi finns allt innehåll i ett datacenter i Houston, använder SQL Server som hello datalager.

”Vår produkt erbjudande också finns en medlem riktade webbportal (skrivs i ASP.net), som var utformad toobe vit etikett toomatch hello kundens webben och ett SOAP API toosupport hello online-sidor och någon tredje parts-integrering”.

hello migrering toohello molnet inte ta lång tid för hello arkitektur. Enligt tooMolina ”hello merparten av hello arbete behandlas ändra hello sätt som vi läsa config filinformation, en centraliserad anslutningssträngen ändring och automatisera hello paketering, överföring och distribution av våra versioner”.

toodevelop hello skapa automatisering, CSI program används Azure PowerShell och REST API: er toocreate paket och överför dem tooa mellanlagringsmiljön för versionen varje natt.
hello gick övergripande övergången tooan Azure molnbaserad distribution smidigt och snabbt för hello CSI programvara IT-teamet. Molina förklarar ”i alla, vi hade en beta-miljö i hello molnet inom tre toofour veckor tar med hello-projekt. Som har en konstigt win för oss ”.

När du konfigurerar och testar hello miljö, CSI programvara började migrera kunder. För kunder som redan använder CSI programvara värd var hello övergången nästan sömlös. För kunder som migrerar från en lokal distribution hello migrering toohello moln tog vissa extra tid, men var fortfarande främst smärta kostnadsfri för kunder och CSI programvara.

För nya kunder, CSI programvaras IT-personal använda hello följande process tooon-kort dem tooAzure:

1. Azure PowerShell-skript har använt toospin upp en ny databas för hello kund. alla kunder starta på en premium-nivån tooensure tillräckligt med inledande genomströmning för hello övergång.
2. När det är möjligt använder CSI programvaran hello Azure SQL-migreringsguiden toomove befintlig data tooan Azure SQL Database-instans.
3. Slutligen, Microsoft SQL Server Integration Services (SSIS) har använt tooreconcile avvikelser i hello data eller tooperform Rensa alla data som krävs.

Idag finns om 99 procent av CSI Software kunder i Azure, i fyra regionala datacenter (norra centrala, södra centrala, Öst och Väst). Genom att datacenter i geografisk region för varje kund, sparas svarstid tooa minsta.

## <a name="azure-elastic-pools-free-up-it-resources"></a>Azure elastiska pooler Frigör IT-resurser
Flera funktioner i Azure har hjälpt CSI programvara SKIFT från att infrastrukturen och åtgärder fokuserad toobeing funktion och utveckling fokuserar. Kanske har hello största fördelen från elastiska pooler.

CSI programvara innehåller för närvarande cirka 550 databaser för kunder. Före elastiska pooler det var svårt toomanage så många databaser i en nivå-struktur. OPS chefer hade tooassign prestandanivåer baserat på hello burst behoven hos kunder som krävde betydande IT-resurs omkostnader. Med elastiska pooler kan chefer tilldela klienterna en premium eller Standardpool efter behov, och sedan flytta kunder baserat på storleken och behöver. Kunder tyckte hello effekterna av hello elastiska pooler nästan omedelbart. kunder har tidsgränser och andra problem under burst-användning perioder innan elastiska pooler, men med elastiska pooler kunder kan drabbas av aktiviteten belastning vid behov och de kan fortsätta toouse SpectrumNG utan problem.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Azure active geo-replikering accelererar reporting
Flera CSI programvara kunder också dra nytta av Azure active geo-replikering. Med aktiv geo-replikering, in toofour läsbara sekundära databaser kan konfigureras i hello samma eller olika datacenterregioner. CSI programvara använder aktiv geo-replikering på två sätt: först hello sekundära databaser är tillgängliga i hello skiftläget för ett datacenter nätverksavbrott eller hello oförmåga tooconnect toohello primära databasen. och andra hello sekundära databaser kan läsas och kan vara används toooffload skrivskyddade arbetsbelastningar som rapportering jobb. Vissa CSI programvara kunder att använda den här förmånen tooaccelerate reporting arbetsflöden.

## <a name="csi-software-application-logic-and-architecture"></a>CSI programvara programlogiken och arkitektur
SpectrumNG använder webbroller. Eftersom programmet hello är flera innehavare, är en WCF-tjänst används toohandle hello första anslutningsbegäran från kunder. Som Molina tillstånd ”hello förfrågan identifierar varje kund, som sedan kan vi skapa en anslutningssträng ut tootheir databaser toodo vad vi behöver toodo”.

För hello webbnivå av sin tjänst utnyttjar CSI program Azure automatisk skalning, baserat på dag och tid. Tillgängliga resurser är automatiskt ökad tooaccommodate högre användning under kontorstid, enligt toohello tidszonen för varje regionala datacenter. Resurser även är inställda tooscale helger, när kundens behov är lägre.

![Daxko/CSI-arkitektur](./media/sql-database-implementation-daxko/figure1.png)

Bild 1. En arbetsroll för cloud services ritar strukturerade data från Azure SQL Database och halvstrukturerade data från tabellagring. SpectrumNG användarna samverkar med att webbroll services-data via ett moln.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Med hjälp av webbprogram och en nivå med web-plan för mobila appar
Med Azure SQL Database frigjort resurser för CSI programvara tooenable nya initiativ, inklusive en komplett mobil plattform baserat på en anpassad API finns i Azure-webbappar. hello-plattformen gör det möjligt för gymmet medlemmar och personal toouse mobilenheter toocheck scheman bok klasser och ta emot meddelanden.

hello-plattformen använder tjänstorienterad arkitektur (SOA) tootake en enda komponent, som ett kassan system (POS) eller ett system för försäljning – flytta på hello Lägg tooanother web plan och sedan få igång en service toosupport komponent, och lämna allt annat på hello ursprungliga web-plan. Den här möjligheten ger CSI programvara enorm flexibilitet och det hjälper håll nere kostnaderna.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure låter CSI programvara utvecklare att fokusera på appar och tjänster
Azure SQL-databas är inte bara en boon tooSpectrumNG kunder som få hello snabb och tillförlitlig tjänst är det också en stor win för CSI programvara IT-personal och utvecklare. Genom att avlasta ops tooAzure i hello molnet CSI programvara minskar dess kostnader för resurser och infrastruktur, snabbare betydligt dess utvecklingscykler och inte längre behöver toomicromanage databaser toooptimize prestanda för sina klienter.

## <a name="more-information"></a>Mer information
* toolearn mer om Azure elastiska pooler finns [elastiska pooler](sql-database-elastic-pool.md).
* toolearn mer om Databasverktyg och elastiska skalning, se [elastisk Databasverktyg och elastiska skalning](sql-database-elastic-scale-get-started.md).
* toolearn mer information om hur du migrerar en SQL Server-databas finns i avsnittet [migrera en SQL Server-databasen tooAzure](sql-database-cloud-migrate.md).
* toolearn mer information om aktiv geo-replikering, se [aktiv geo-replikering](sql-database-geo-replication-overview.md).
* toolearn mer om Web och arbetsroller, se [arbetsroller](../fundamentals-introduction-to-azure.md#compute).    
* toolearn mer om Azure Service Bus finns [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* toolearn mer om automatisk skalning, se [skalning molntjänster](../cloud-services/cloud-services-how-to-scale.md).

