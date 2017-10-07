---
title: "aaaConsistency nivåer i Azure Cosmos DB | Microsoft Docs"
description: "Azure Cosmos-DB har fem konsekvenskontroll nivåer toohelp saldo eventuell konsekvens, tillgänglighet och svarstid kompromisser."
keywords: slutliga konsekvensen azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: mimig1
manager: jhubbard
editor: cgronlun
documentationcenter: 
ms.assetid: 3fe51cfa-a889-4a4a-b320-16bf871fe74c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ac399c229d0856cd811bc81568536e519af3300f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tunable-data-consistency-levels-in-azure-cosmos-db"></a>Data justerbara konsekvensnivåer i Azure Cosmos DB
Azure Cosmos-DB är utformad från hello bakgrund med global distributionsplatsen i åtanke för varje datamodell. Det är utformad toooffer förutsägbar låg latens garantier, ett 99,99% tillgänglighet SERVICENIVÅAVTAL, och flera väldefinierade mjukas upp konsekvenskontroll modeller. För närvarande Azure Cosmos DB innehåller fem konsekvensnivåer: stark, begränsat föråldrad, session, konsekvent prefix och eventuell. 

Förutom **starkt** och **slutliga konsekvensen** modeller ofta erbjuds av distribuerade databaser Azure Cosmos DB erbjuder tre mer noggrant kodade och operationalized konsekvenskontroll modeller och har verifierat sina användbarhet mot verkligheten användningsfall. Dessa är hello **avgränsas föråldrad**, **session**, och **konsekvent prefixet** konsekvensnivåer. Dessa fem konsekvensnivåer aktivera gemensamt toomake välmotiverat avvägningarna mellan konsekvens, tillgänglighet och svarstid. 

## <a name="distributed-databases-and-consistency"></a>Distribuerade databaser och konsekvenskontroll
Kommersiellt distribuerade databaser är indelade i två kategorier: databaser som inte alls erbjuder väldefinierade, beprövade konsekvensval och databaser som erbjuder två extremt programmerbara val (stark kontra eventuell konsekvens). 

Hej tidigare skada programvaruutvecklare med minutia av deras Replikeringsprotokoll och förväntar sig dem. toomake svårt kompromisser mellan konsekvens, tillgänglighet, svarstid och genomströmning. hello senare placerar en trycket toochoose en hello två yttersta. Trots hello förekomst av forskning och förslag till mer än 50 konsekvenskontroll modeller har hello distribuerad databas community inte kan toocommercialize konsekvensnivåer utöver starkt och eventuell konsekvenskontroll. Cosmos DB kan utvecklare toochoose mellan fem väldefinierade konsekvenskontroll längs hello konsekvenskontroll spektrumet – stark, begränsat föråldrad, [session](http://dl.acm.org/citation.cfm?id=383631), konsekvent prefix och eventuell. 

![Azure Cosmos-DB erbjuder flera väldefinierade (Avslappnad) konsekvenskontroll modeller toochoose från](./media/consistency-levels/five-consistency-levels.png)

hello följande tabell visar hello särskilda garantier varje konsekvensnivå tillhandahåller.
 
**Konsekvensnivåer och garantier**

| Konsekvensnivå | Garantier |
| --- | --- |
| Stark | Lineariserbarhet |
| Begränsad föråldring | Konsekvent Prefix. Läsningar släpar efter skrivningar med k-prefix eller t-intervall |
| Session   | Konsekvent Prefix. Monotoniska läsningar, monotoniska skrivningar, läs-dina-skrivningar, skrivning-följer-läsning |
| Konsekvent prefix | Uppdateringar som returnerades är vissa prefix för alla hello uppdateringar med några mellanrum |
| Eventuell  | Oordnade läsningar |

Du kan konfigurera hello standardnivå för konsekvens för Cosmos-DB-konto (och senare åsidosätta hello konsekvenskontroll på en specifik läsbegäran). Internt, gäller hello konsekvenskontroll Standardnivå toodata inom hello partitionsuppsättningar som kan sträcka sig över regioner. Om 73% av våra klientorganisationer använder sessionskonsekvens 20% föredrar avgränsas föråldrad. Vi se att cirka 3 procent av våra kunder experimentera med olika konsekvensnivåer ursprungligen innan reglera på en specifik konsekvenskontroll val för sina program. Vi kan också se att endast 2% av våra klientorganisationer åsidosätter konsekvensnivåer på grundval av per begäran. 

Läser hanteras i sessionen, konsekvent prefix i Cosmos-DB och slutliga konsekvensen är två gånger så låg som läser med starka eller begränsad föråldringskonsekvens. Cosmos DB har branschledande serviceavtal för omfattande 99,99% inklusive konsekvens garanterar tillsammans med tillgänglighet, genomflöde och svarstid. Vi använder en [linearizability layout](http://dl.acm.org/citation.cfm?id=1806634), som körs kontinuerligt i vår tjänst telemetri och öppet rapporterar någon konsekvenskontroll överträdelser tooyou. Begränsat föråldrad, vi övervaka och rapportera eventuella överträdelser tog och t gränser för. För alla fem Avslappnad konsekvensnivåer, vi även rapportera hello [probabilistic avgränsas föråldrad mått](http://dl.acm.org/citation.cfm?id=2212359) direkt tooyou.  

## <a name="scope-of-consistency"></a>Omfånget för konsekvenskontroll
hello Granulariteten för konsekvenskontroll är begränsade tooa enanvändarläge begäran. Write-förfrågan kan motsvarar tooan infoga, Ersätt, upsert eller ta bort transaktionen. En Läs/fråga transaktion är också begränsade tooa enanvändarläge begäran som skrivningar. hello-användare kan vara nödvändiga toopaginate via en stor resultatmängd, sträcker sig över flera partitioner, men varje läsa transaktion är begränsade tooa sida och hanteras från inom en partition.

## <a name="consistency-levels"></a>Konsekvensnivåer
Du kan konfigurera en konsekvenskontroll Standardnivå på ditt konto som gäller tooall samlingar (och databaser) under Cosmos-DB-kontot. Som standard kan alla läser och frågor som utfärdats för hello använder användardefinierade resurser hello konsekvenskontroll Standardnivå anges på hello konto. Kan du slappna av hello konsekvensnivå för en specifik Läs/fråga begäran med i varje hello stöds API: er. Det finns fem typer av konsekvensnivåer som stöds av hello Azure Cosmos DB replikering protokoll som tillhandahåller en tydlig kompromiss mellan specifika konsekvens garanterar och prestanda, enligt beskrivningen i det här avsnittet.

**Starka**: 

* Stark konsekvens erbjuder en [linearizability](https://aphyr.com/posts/313-strong-consistency-models) garanti med hello läser garanterad tooreturn hello senaste versionen av ett objekt. 
* Stark konsekvens garanterar att en skrivning enbart är synliga när den har bevarats varaktigt av kvorum för hello majoritet av replikerna. Skrivning har antingen synkront bevarats varaktigt av både hello primära och hello kvorum av sekundärservrar eller den avbröts. Läs bekräftas alltid majoritet hello läsa kvorum, en klient kan aldrig finns i en icke allokerad eller partiell skrivning och alltid garanteras tooread hello senaste godkänt skrivåtgärder. 
* Azure DB Cosmos-konton som är konfigurerade toouse stark konsekvens kan inte associera mer än en Azure-region med ett konto i Azure Cosmos DB. 
* Hej kostnaden för en läsning (i [programbegäran](request-units.md) förbrukas) med stark konsekvens är högre än session och slutlig men hello samma som avgränsas föråldrad.

**Begränsat föråldrad**: 

* Begränsad föråldringskonsekvens garanterar att hello läsningar kan ligga efter skrivning av högst *K* versioner eller prefix för ett objekt eller *t* tidsintervallet. 
* Därför när du väljer begränsat föråldrad, hello ”föråldrad” kan konfigureras på två sätt: antal versioner *K* hello-objektet som hello läsningar lag bakom hello skrivningar och hello tidsintervall *t* 
* Begränsat föråldrad erbjudanden totala globala ordning utom i hello ”föråldrad fönster”. hello monotonisk läsa garantier finns inom en region både inom och utanför hello ”föråldrad fönster”. 
* Begränsad föråldrad ger konsekvens bättre säkerhet än session eller slutliga konsekvensen. För globalt distribuerade program rekommenderar vi att du använder avgränsas föråldrad för scenarier där du vill toohave stark konsekvens utan också 99,99% tillgänglighet och låg latens. 
* Azure DB Cosmos-konton som har konfigurerats med begränsad föråldringskonsekvens kan associera valfritt antal Azure-regioner med sina Azure DB som Cosmos-konto. 
* Hej kostnaden för en läsning (vad gäller RUs förbrukas) med begränsad föråldrad är högre än sessionen och slutliga konsekvensen men hello samma som stark konsekvens.

**Sessionen**: 

* Till skillnad från hello globalt konsekvensfel modeller som erbjuds av stark och avgränsas föråldrad konsekvensnivåer, är sessionskonsekvens begränsade tooa klientsessionen. 
* Sessionskonsekvens är idealiskt för alla scenarier där en enhet eller användare sessionen ingår eftersom det garanterar monotonisk läsningar, monotonisk skrivningar och läsa garanterar att din egen skrivningar (RYW). 
* Sessionskonsekvens tillhandahåller förutsägbar konsekvenskontroll för en session och maximalt läsa genomströmning samtidigt som den erbjuder hello lägsta svarstid skrivningar och läsningar. 
* Azure DB Cosmos-konton som har konfigurerats med sessionskonsekvens kan associera valfritt antal Azure-regioner med sina Azure DB som Cosmos-konto. 
* Hej kostnaden för en läsning (vad gäller RUs förbrukas) med session konsekvensnivå är mindre än starkt som avgränsas föråldrad, men mer än slutliga konsekvensen

<a id="consistent-prefix"></a>
**Konsekvent prefixet**: 

* Konsekvent prefix garanterar att i frånvaron av ytterligare skrivningar hello replikerna i gruppen hello slutligen Konvergera. 
* Konsekvent prefix garanterar att läsningar aldrig finns i oordning skrivningar. Om skrivningar utfördes i hello ordning `A, B, C`, och sedan en klient ser antingen `A`, `A,B`, eller `A,B,C`, men aldrig i oordning som `A,C` eller `B,A,C`.
* Azure DB Cosmos-konton som har konfigurerats med konsekvent prefix konsekvenskontroll kan associera valfritt antal Azure-regioner med sina Azure Cosmos DB-konto. 

**Eventuell**: 

* Slutliga konsekvensen garanterar att i frånvaron av ytterligare skrivningar hello replikerna i gruppen hello slutligen Konvergera. 
* Slutliga konsekvensen är hello svagaste konsekvenskontroll om en klient kan få hello-värden som är äldre än hello som har läst.
* Slutliga konsekvensen ger hello svagaste läsningskontinuitet men erbjudanden hello lägsta fördröjningen för både läsningar och skrivningar.
* Azure DB Cosmos-konton som har konfigurerats med slutliga konsekvensen kan associera valfritt antal Azure-regioner med sina Azure DB som Cosmos-konto. 
* Hej kostnaden för en läsning (vad gäller RUs förbrukas) med hello slutliga konsekvensen är hello lägsta av konsekvensnivåer för alla hello Azure Cosmos DB.

## <a name="configuring-hello-default-consistency-level"></a>Konfigurera hello standardnivå för konsekvenskontroll
1. I hello [Azure-portalen](https://portal.azure.com/), i hello Jumpbar klickar du på **Azure Cosmos DB**.
2. I hello **Azure Cosmos DB** bladet, Välj hello databasen konto toomodify.
3. I hello-kontoblad klickar du på **standard konsekvenskontroll**.
4. I hello **standard konsekvenskontroll** bladet, Välj hello nya konsekvensnivå och klickar på **spara**.
   
    ![Skärmbild som visar markering hello inställningsikonen och standard konsekvenskontroll post](./media/consistency-levels/database-consistency-level-1.png)

## <a name="consistency-levels-for-queries"></a>Konsekvensnivåer för frågor
Som standard för användardefinierade resurser hello konsekvensnivå för frågor är hello samma hello konsekvensnivå för läser. Hello indexet uppdateras synkront som standard på varje infoga, Ersätt eller borttagning av en objektbehållaren toohello Cosmos DB. Den här möjliggör hello frågor toohonor hello samma konsekvensnivå som pekar läsningar. När Azure Cosmos DB är skrivåtgärder optimerad och stöder varaktigt mängder skrivningar, synkron index underhåll och betjänar konsekvent frågor, kan du konfigurera vissa samlingar tooupdate deras index lazy. Lazy indexering ytterligare förstärker hello skrivprestanda och är idealisk för bulk införandet scenarier när en arbetsbelastning är främst Läs aktiverat.  

| Indexering läge | Läser | Frågor |
| --- | --- | --- |
| Konsekvent (standard) |Välj från stark, begränsat föråldrad, session, konsekvent prefix eller senare |Välj från stark, begränsat föråldrad, session eller senare |
| Lazy |Välj från stark, begränsat föråldrad, session, konsekvent prefix eller senare |Eventuell |
| Ingen |Välj från stark, begränsat föråldrad, session, konsekvent prefix eller senare |Inte tillämpligt |

Med skrivskyddade förfrågningar lägre som hello konsekvensnivå för en specifik fråga begäran i varje API.

## <a name="next-steps"></a>Nästa steg
Om du vill att toodo mer läsning om konsekvensnivåer och kompromisser, rekommenderar vi hello följande resurser:

* Doug Terry. Replikerade datakonsekvens beskrivs via baseball (video).   
  [https://www.YouTube.com/Watch?v=gluIh8zd26I](https://www.youtube.com/watch?v=gluIh8zd26I)
* Doug Terry. Replikerade datakonsekvens beskrivs via baseball.   
  [http://Research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.PDF](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
* Doug Terry. Sessionen garantier för svagt konsekvent replikerade Data.   
  [http://DL.acm.org/citation.cfm?ID=383631](http://dl.acm.org/citation.cfm?id=383631)
* Mikael Abadi. Konsekvenskontroll kompromisser i moderna distribuerade system databasstruktur: Fästpunkten är endast en del av en artikel hello ”.   
  [http://Computer.org/CSDL/mags/CO/2012/02/mco2012020037-ABS.HTML](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html)
* Peter Bailis, Shivaram Venkataraman, Michael J. Franklin, Joseph m Hellerstein, med Stoica. Probabilistic innanför föråldrad (PBS) för praktiska partiella beslutsförhet.   
  [http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.PDF](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
* Werner Vogels. Eventuell konsekvent - Revisited.    
  [http://allthingsdistributed.com/2008/12/eventually_consistent.HTML](http://allthingsdistributed.com/2008/12/eventually_consistent.html)
* Moni Naor, Avishai ull, hello belastning, kapacitet och tillgänglighet av kvorum system, SIAM journalen på Computing, v.27 n.2, p.423-447, April 1998.
  [http://epubs.siam.org/DOI/ABS/10.1137/S0097539795281232](http://epubs.siam.org/doi/abs/10.1137/S0097539795281232)
* Sebastian Burckhardt, Chris Dern, Macanal Musuvathi, Roy Tan, rad: en fullständig och automatisk linearizability bricka, förfaranden för hello 2010 ACM SIGPLAN konferens om programmering språk design och implementeringslösning juni 05 10 2010, Stockholmsderbyt, Ontario, Kanada [doi > 10.1145/1806596.1806634] [http://dl.acm.org/citation.cfm?id=1806634](http://dl.acm.org/citation.cfm?id=1806634)
* Peter Bailis, Shivaram Venkataraman, Michael J. Franklin, Joseph M. Hellerstein, med Stoica Probabilistically avgränsas föråldrad för praktiska partiella beslutsförhet förfaranden hello VLDB särskilt tillskjutet, v.5 n.8, p.776-787, April 2012 [http:// DL.acm.org/citation.cfm?ID=2212359](http://dl.acm.org/citation.cfm?id=2212359)
