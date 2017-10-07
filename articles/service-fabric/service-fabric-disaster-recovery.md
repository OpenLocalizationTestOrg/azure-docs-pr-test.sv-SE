---
title: "aaaAzure Service Fabric-katastrofåterställning | Microsoft Docs"
description: "Azure Service Fabric erbjuder hello funktioner nödvändiga toodeal med alla typer av katastrofer. Den här artikeln beskriver hello typer av katastrofer som kan uppstå och hur toodeal med dem.."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 04b8348fb63e8a1c76a8f722c4c8255b339908e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-in-azure-service-fabric"></a>Katastrofåterställning i Azure Service Fabric
En viktig del av ger hög tillgänglighet är att säkerställa att tjänster kan överleva alla olika typer av fel. Detta är särskilt viktigt för fel som är oplanerade och utanför din kontroll. Den här artikeln beskriver vissa vanliga feltillstånd som kan vara katastrofer om inte modelleras och hanteras korrekt. Åtgärder och åtgärder tootake beskrivs även om en katastrof hände ändå. hello målet är toolimit eller eliminera hello risken för avbrott eller dataförlust när de uppstår fel, planerade eller annars kan uppstå.

## <a name="avoiding-disaster"></a>Undvika katastrofåterställning
Service Fabric primära målet är toohelp du modellen både din miljö och dina tjänster så att den vanliga fel typer inte är katastrofer. 

I allmänhet finns två typer av disaster/scenarier:

1. Maskinvaru- eller fel
2. Använd fel

### <a name="hardware-and-software-faults"></a>Fel för maskinvara och programvara
Fel för maskinvara och programvara är oförutsägbart. hello enklaste sättet toosurvive fel körs fler kopior av hello service omfattas över maskinvaru- eller fault-gränser. Till exempel om din tjänst körs bara på en viss dator, är sedan hello felet på att en dator en katastrofåterställning för tjänsten. hello enkelt tooavoid denna katastrofåterställning är tooensure hello faktiskt körs på flera datorer. Testning är också nödvändigt tooensure hello fel på en dator inte störa hello tjänsten körs. Kapacitetsplanering säkerställer en ersättning instans kan skapas på en annan plats och att minskad kapacitet inte överlagra hello återstående tjänster. hello samma mönster fungerar oavsett vad du skulle tooavoid hello fel i. Till exempel. Om du är orolig hello fel i ett SAN-nätverk kan köra du över flera SAN-nätverk. Om du vill göra hello förlust av en samling servrar, kör över flera rack. Om du oroar hello förlust av datacenter, ska tjänsten köras i flera Azure-regioner eller Datacenter. 

När den körs i den här typen av disklänkande läge du fortfarande är toosome ämnestyper för samtidiga fel, men enskilt och även flera fel i en viss typ (ex: en enskild virtuell dator eller nätverket länken misslyckas) hanteras automatiskt (och därför inte längre en ”katastrof”). Service Fabric innehåller många metoder för att expandera hello klustret och hanterar tillbaka när felande noder och tjänster. Service Fabric kan också kör flera instanser av tjänsterna i ordning tooavoid dessa typer av oplanerade fel från aktivera i riktiga katastrofer.

Det kan finnas skäl varför kör en tillräckligt stor toospan distribution över fel inte är möjligt. Det kan till exempel ta mer maskinvaruresurser än du inte vill toopay för relativa toohello risken för fel. När du hanterar distribuerade program, kan det bero på att ytterligare kommunikation hopp eller tillstånd replikering kostnader över geografiska avstånd orsaker acceptabel svarstid. Om den här raden ritas skiljer sig för varje program. För programvarufel specifikt kan hello fel vara i hello-tjänsten som du försöker tooscale. I det här fallet förhindra inte fler kopior hello katastrofåterställning, eftersom hello feltillståndet korreleras över alla hello-instanser.

### <a name="operational-faults"></a>Använd fel
Även om tjänsten omfattas över hello jordglob med många uppsägningar, kan det fortfarande har katastrofala händelser. Till exempel om någon av misstag konfigurerar hello dns-namn för tjänsten hello, eller tar bort den direkt. Exempel anta att du har en tillståndskänslig Service Fabric-tjänst och någon av misstag tas bort tjänsten. Om det inte finns några andra minskning, tjänsten och alla hello tillstånd den hade är nu rest. Dessa typer av operativa katastrofer kräver (”hoppsan”) olika åtgärder och steg för återställning än vanliga oplanerade fel. 

hello bästa sätt tooavoid dessa typer av operativa fel är att
1. begränsa operativa åtkomst toohello miljö
2. strikt granskningsåtgärder farliga
3. införa automation, förhindra manuell eller out-of-band-ändringar och verifiera specifika ändringar mot hello faktiska miljö innan anta dem
4. Se till att skadliga åtgärder ”soft”. Mjuka operations börjar inte gälla omedelbart eller kan ångras i vissa tidsfönstret

Service Fabric innehåller vissa mekanismer tooprevent operativa fel, till exempel att tillhandahålla [rollbaserad](service-fabric-cluster-security-roles.md) åtkomstkontroll för klusteråtgärder för. De flesta av dessa operativa fel kräver dock arbete och andra system. Service Fabric tillhandahåller en mekanism för kvarvarande operativa fel, framför allt säkerhetskopiering och återställning för tillståndskänsliga tjänster.

## <a name="managing-failures"></a>Hantera fel
hello målet för Service Fabric är nästan alltid automatisk hantering av fel. Men i order toohandle vissa typer av fel, tjänster måste ha ytterligare kod. Andra typer av fel bör _inte_ åtgärdas automatiskt av säkerhet och business continuity skäl. 

### <a name="handling-single-failures"></a>Enkel hantering-fel
Enskild datorerna kan växlas till en mängd olika skäl. Vissa av dessa är maskinvara orsaker som strömkällor och nätverk maskinvarufel. Andra fel finns i programvaran. Dessa inkluderar fel hello faktiska operativsystem och själva hello-tjänsten. Service Fabric identifierar automatiskt dessa typer av fel, inklusive fall där hello datorn blir isolerad från andra datorer på grund av problem med toonetwork.

Oavsett hello typ av tjänst, kör en enda instans resultat driftstopp för tjänsten om den enda kopian av hello kod misslyckas av någon anledning. 

I ordning toohandle varje enskilt fel hello enklaste som du kan göra är tooensure som dina tjänster som körs på fler än en nod som standard. För tillståndslösa tjänster detta kan åstadkommas genom att låta en `InstanceCount` större än 1. För tillståndskänsliga tjänster hello minsta rekommendation är alltid en `TargetReplicaSetSize` och `MinReplicaSetSize` på minst 3. Kör fler kopior av koden för tjänsten säkerställer att din tjänst kan hantera varje enskilt fel automatiskt. 

### <a name="handling-coordinated-failures"></a>Hantering av koordineras fel
Samordnade fel kan inträffa i ett kluster på grund av tooeither planerad eller oplanerad infrastruktur fel och ändringar eller planerade programändringar. Service Fabric modeller infrastruktur zoner som samordnade fel som Feldomäner uppstår. Områden som får samordnade programvaruändringar modelleras som uppgradera domäner. Mer information om fel- och domäner finns i [dokumentet](service-fabric-cluster-resource-manager-cluster-description.md) som beskriver klustertopologi och definition.

Som standard anser Service Fabric-fel och uppgradera domäner när du planerar där dina tjänster ska köras. Som standard försöker Service Fabric tooensure som dina tjänster körs över flera fel- och domäner så om planerad eller oplanerad ändringar sker tjänsterna är tillgängliga. 

Anta exempelvis att fel i ett eluttag orsakar en samling datorer toofail samtidigt. Blir ett exempel på en enda misslyckades för en viss tjänst med flera kopior av hello-tjänsten körs hello förlust av många datorer i fel domän fel. Det är därför hantera feldomäner är kritiska tooensuring hög tillgänglighet för dina tjänster. När du kör Service Fabric i Azure, hanteras automatiskt feldomäner. I andra miljöer kanske de inte. Om du utvecklar dina egna kluster lokalt vara säker på att toomap och planera fel domän layouten på rätt sätt.

Uppgraderingsdomäner är användbara för att modellera områden där programvaran kommer toobe uppgraderas vid hello samma tid. Därför definiera uppgradera domäner också ofta hello gränser där programvara tas under planerade uppgraderingar. Uppgraderingar av både Service Fabric och dina tjänster följer hello samma modell. Mer information om rullande uppgraderingar och uppgraderingsdomäner hello Service Fabric-hälsomodell som förhindrar oväntade ändringar påverkar hello klustret och din tjänst finns i dessa dokument:

 - [Uppgradering av programmet](service-fabric-application-upgrade.md)
 - [Uppgradera självstudien](service-fabric-application-upgrade-tutorial.md)
 - [Service Fabric-Hälsomodell](service-fabric-health-introduction.md)

Du kan visualisera hello layout på klustret med hjälp av hello över lediga kluster i [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md):

<center>
![Noder som är fördelade på feldomäner i Service Fabric Explorer][sfx-cluster-map]
</center>

> [!NOTE]
> Modeling områden för fel, rullande uppgraderingar, kör flera instanser av Tjänstkod och tillstånd, placering regler tooensure dina tjänster köra fel- och domäner och inbyggda hälsoövervakning är bara **vissa** av hello funktioner som Service Fabric ger i ordning tookeep normala operativa problem och fel från att omvandla till katastrofer. 
>

### <a name="handling-simultaneous-hardware-or-software-failures"></a>Hantering av samtidiga maskinvaru- eller fel
Högre diskuterats vi enstaka fel. Som du ser är enkelt toohandle för både tillståndslösa och tillståndskänsliga tjänster genom att hålla fler kopior av hello kod (och tillstånd) körs i fel- och uppgraderingsdomäner. Flera samtidiga slumpmässiga fel kan också inträffa. Det här är mer troligt toolead tooan faktiska katastrofåterställning.


### <a name="random-failures-leading-tooservice-failures"></a>Slumpmässiga fel inledande tooservice fel
Anta att hello-tjänsten hade en `InstanceCount` 5 och flera noder som kör dessa instanser alla misslyckades vid hello samma tid. Service Fabric svarar genom att automatiskt skapa ersättning instanser på andra noder. Kommer att fortsätta skapa ersättningar tills hello-tjänsten är tillbaka tooits önskat instansantal. Ett annat exempel anta att det fanns en tillståndslös tjänst med en `InstanceCount`1, vilket innebär att den körs på alla noder i klustret hello som giltig. Anta att några av dessa instanser kunde toofail. I det här fallet meddelanden Service Fabric att hello-tjänsten inte är dess status som önskas och försöker toocreate hello instanser på hello noder där de saknas. 

För tillståndskänsliga tjänster beror hello situationen på om hello-tjänsten har sparat tillstånd eller inte. Det beror också på hur många repliker hello-tjänsten har och hur många misslyckades. Avgöra om en katastrof uppstod för en tillståndskänslig service och hanteringen av den följer tre steg:

1. Bestämma om det har förekommit förlorar kvorum eller inte
 - En förlorar kvorum är när en majoritet av hello repliker av en tillståndskänslig service löper hello samtidigt, inklusive hello primär.
2. Bestämma om hello kvorumförlust är permanent
 - De flesta hello tid är fel tillfälligt. Processer har startats om, noder har startats om, virtuella datorer är relaunched, läka nätverkspartitioner. Ibland om fel är permanent. 
    - För tjänster utan beständiga tillståndet kan ett fel i minst ett kvorum av repliker resultat _omedelbart_ förlorar kvorum permanent. När Service Fabric upptäcker förlorar kvorum i ett icke-beständig tillståndskänslig service, fortsätter omedelbart dataloss toostep 3 genom att deklarera (potentiella). Om du fortsätter toodataloss meningsfullt eftersom Service Fabric vet att det finns ingen anledning att vänta på hello repliker toocome tillbaka eftersom även om de har återställts de skulle vara tom.
    - För tillståndskänsliga beständiga tjänster orsakar ett fel i minst ett kvorum av repliker Service Fabric-toostart väntar hello repliker tillbaka toocome och Återställ kvorum. Detta resulterar i ett avbrott för alla _skriver_ toohello påverkas partition (eller ”replikuppsättningen”) för hello-tjänsten. Läser kan dock fortfarande vara möjligt med nedsatt konsekvens garanterar. hello standardmängden tid som Service Fabric väntar kvorum toobe återställts är oändligt, eftersom du fortsätter är en (eventuella) dataloss händelse och har andra risker. Åsidosätta hello standard `QuorumLossWaitDuration` värdet går men rekommenderas inte. I stället för tillfället alla ansträngningar bör göras toorestore hello ned repliker. Detta kräver att hello-noder som är nere tillbaka in och säkerställa att de kan återansluta hello enheter där de lagras hello lokala beständiga tillstånd. Om hello kvorumförlust orsakas av misslyckade försök toorecreate hello processer Service Fabric automatiskt och starta om hello repliker i dem. Om detta misslyckas rapporterar Service Fabric hälsa fel. Om de kan lösas kommer hello repliker vanligtvis tillbaka. Ibland kan dock kan hello repliker återtas. Till exempel hello enheter alla har misslyckats eller hello datorer förstörs fysiskt på något sätt. I dessa fall har vi nu en permanent kvorum dataförlust inträffat. tootell Service Fabric toostop väntar hello ned repliker toocome tillbaka en Klusteradministratör måste avgöra vilka partitioner av vilka tjänster som påverkas och anropa hello `Repair-ServiceFabricPartition -PartitionId` eller ` System.Fabric.FabricClient.ClusterManagementClient.RecoverPartitionAsync(Guid partitionId)` API.  Detta API kan du ange hello-ID för hello partition toomove utanför QuorumLoss och till eventuella dataloss.

> [!NOTE]
> Det är _aldrig_ säker toouse detta API än på ett sätt som är riktad mot specifika partitioner. 
>

3. Avgöra om det har förekommit faktiska data går förlorade och återställning från säkerhetskopior
  - När Service Fabric anropar hello `OnDataLossAsync` metod är det alltid eftersom _misstänkt_ dataloss. Service Fabric garanterar att anropet levereras toohello _bästa_ återstående replik. Det här är repliken har gjort hello de flesta pågår. Hej orsak alltid innebär _misstänkt_ dataloss är att det är möjligt hello återstående repliken faktiskt har alla samma tillstånd som hello primära när den avslutades. Men utan att tillståndet toocompare den till, det går inte bra för Service Fabric eller operatorer tooknow verkligen. Nu Service Fabric också vet hello repliker inte kommer tillbaka. Som var hello beslut om när vi stoppats och väntar hello kvorum förlust tooresolve sig själv. hello bästa erhåller för hello-tjänsten är vanligtvis toofreeze och vänta tills särskilda administrativa åtgärder. Därför det gör att en typisk implementering av hello `OnDataLossAsync` metoden göra?
  - Logga först som `OnDataLossAsync` har utlösts och utlösa alla nödvändiga administrativa varningar.
   - Vanligtvis nu toopause och vänta tills ytterligare beslut och manuella åtgärder toobe vidtas. Det beror på att även om säkerhetskopior är tillgängliga kan de behöver toobe förberedd. Om två olika tjänster samordna information, till exempel måste säkerhetskopieringarna toobe ändras i ordning tooensure som när hello återställning händer att hello information dessa två tjänster är intresserad är konsekvent. 
  - Ofta finns även vissa andra telemetri eller avgaser från hello-tjänsten. Dessa metadata kan finnas i loggarna eller i andra tjänster. Den här informationen kan vara används behövs toodetermine om det inte fanns några anrop emot och bearbetas på hello primära som inte fanns i hello säkerhetskopiering eller replikerade toothis viss replik. Dessa kräver toobe upprepat eller har lagts till toohello säkerhetskopiering innan det är möjligt att återställningen.  
   - Jämförelser av hello återstående replikens tillstånd toothat som ingår i alla säkerhetskopior som är tillgängliga. Om du använder hello Service Fabric tillförlitliga samlingar finns det verktyg och bearbetar tillgängliga för att göra det, beskrivs i [i den här artikeln](service-fabric-reliable-services-backup-restore.md). hello målet är toosee om hello tillstånd i hello replik räcker, eller också vilka hello-säkerhetskopiering kanske saknas.
  - En gång hello jämförelsen görs och om nödvändigt hello återställningen har slutförts, hello service-kod ska returnera true om alla tillstånd har ändrats. Om hello replik har fastställt att det var hello bäst tillgängliga kopia av hello tillstånd och gjort några ändringar, returnera false. SANT anger att de _andra_ återstående repliker kan nu inte stämmer överens med den här. De tas bort och återskapas från den här repliken. Falskt anger att gjordes inga tillståndsändringar, så hello repliker kan behålla de har. 

Det är ytterst viktigt att tjänsten författare öva potentiella dataloss och scenarier innan tjänster aldrig har distribuerats i produktionsmiljön. tooprotect mot hello möjligheten att dataloss är det viktigt tooperiodically [säkerhetskopiera hello tillstånd](service-fabric-reliable-services-backup-restore.md) för någon av dina tillståndskänsliga tjänster tooa geo-redundant lagring. Du måste också kontrollera att du har hello möjlighet toorestore den. Eftersom säkerhetskopior av många olika tjänster utförs vid olika tidpunkter, måste tooensure att tjänsterna efter en återställning har en konsekvent överblick över varandra. Anta till exempel att en situation där en tjänst genererar ett tal och lagrar det och skickar den tooanother tjänst som lagrar även det. Efter en återställning kan du identifiera att andra hello-tjänsten har hello nummer men hello först inte, eftersom dess säkerhetskopieringen inte innehåller denna åtgärd.

Om du upptäcker att hello återstående repliker är otillräcklig toocontinue från i ett scenario med dataloss och du kan inte återskapa Tjänststatus från telemetri eller avgaser, avgör hello frekvens för dina säkerhetskopieringar det bästa möjliga återställningspunktmålet (RPO) . Service Fabric innehåller många verktyg för att testa olika scenarier, inklusive permanent kvorum och dataloss som kräver återställning från en säkerhetskopia. Dessa scenarier ingår som en del av Service Fabric datatillgång verktyg, hanteras av hello fel Analysis Services. Mer information om dessa verktyg och mönster finns [här](service-fabric-testability-overview.md). 

> [!NOTE]
> Systemtjänster kan också påverkas kvorum försvinner, hello påverkas som specifika toohello tjänsten i fråga. Till exempel påverkar kvorumförlust i hello namngivningstjänst namnmatchning, medan förlorar kvorum i hello failover manager service blockerar skapandet av ny tjänst och växling vid fel. Medan hello Service Fabric-systemtjänster följer samma mönster som dina tjänster för tillståndshantering av hello, rekommenderas inte att du ska försöka toomove dem utanför förlorar kvorum och till eventuella dataloss. hello rekommendation är i stället för[söka stöd](service-fabric-support.md) toodetermine en lösning som är riktade tooyour specifika situation.  Vanligtvis är det bättre toosimply vänta tills hello ned repliker returtyp.
>

## <a name="availability-of-hello-service-fabric-cluster"></a>Tillgängligheten för hello Service Fabric-kluster
Generellt sett är hello Service Fabric själva klustret en miljö med några enskilda felpunkter. Ett fel på någon en nod inte ska orsaka tillgänglighet eller tillförlitlighetsproblem för hello-kluster i första hand eftersom hello Service Fabric-systemtjänster följa samma riktlinjer som tidigare hello: de alltid körs med tre eller flera repliker som standard och de systemtjänster som är tillståndslös köras på alla noder. hello underliggande Service Fabric-nätverk och lager för identifiering av fel är helt distribuerade. De flesta systemtjänster kan byggas från metadata i hello kluster eller vet hur tooresynchronize deras tillstånd från andra platser. hello tillgängligheten för hello klustret kan bli komprometterade om systemtjänster komma in kvorum förlust situationer som de som beskrivs ovan. I dessa fall kanske inte kan tooperform vissa åtgärder på hello klustret som påbörjar en uppgradering eller distribuera nya tjänster, men själva hello klustret är fortfarande in. Tjänster på redan körs fortsätter att köras i dessa villkor om de inte behöver skrivningar toohello system services toocontinue fungerar. Till exempel om hello Failover Manager är förlorar kvorum alla tjänster fortsätter toorun, men alla tjänster som inte kommer inte att kunna tooautomatically omstart, eftersom det kräver hello involvering av hello Failover Manager. 

### <a name="failures-of-a-datacenter-or-azure-region"></a>Fel i en datacenter- eller Azure-region
I sällsynta fall kan ett fysiska Datacenter blir tillfälligt otillgänglig på grund av tooloss av power eller nätverksanslutning. I dessa fall måste blir din Service Fabric-kluster och tjänster i datacenter eller Azure-region otillgänglig. Dock _dina data bevaras_. För kluster som körs i Azure, kan du visa uppdateringar på avbrott på hello [status för Azure][azure-status-dashboard]. Hög osannolika fysiska Datacenter är helt eller delvis förstörs alla Service Fabric-kluster finns det i hello eller hello tjänster i dem kan gå förlorade. Detta inkluderar några tillstånd som inte säkerhetskopieras utanför det datacenter eller regionen.

Det finns två olika strategier för kvarvarande hello permanent eller varaktigt fel i ett enda datacenter eller en region. 

1. Kör separata Service Fabric-kluster i flera områden, och använda någon mekanism för växling vid fel och redundansväxla mellan dessa miljöer. Den här typen av flera klustermodellen för aktiv-aktiv eller aktivt-passivt kräver ytterligare kod för hantering och åtgärder. Detta kräver samordning av säkerhetskopior från hello tjänster i ett datacenter eller region så att de är tillgängliga i andra Datacenter när en misslyckas. 
2. Kör ett enda Service Fabric-kluster som omfattar flera Datacenter eller regioner. hello minimikraven på konfigurationen för det här är tre Datacenter eller regioner. hello rekommenderade antalet regioner eller Datacenter är fem. Detta kräver en mer komplex klustertopologi. Hello är fördelen med den här modellen dock att fel i ett datacenter eller region konverteras till ett vanligt fel från en katastrof. Dessa fel kan hanteras av hello mekanismer som fungerar för kluster inom en enskild region. Se till arbetsbelastningar distribueras så att de tolerera normal fel feldomäner, uppgraderingsdomäner och regler för Service Fabric-placering. Mer information om principer som kan hjälpa dig att använda tjänster i den här typen av klustret läsa på [placeringsprinciper](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)

### <a name="random-failures-leading-toocluster-failures"></a>Slumpmässiga fel inledande toocluster fel
Service Fabric har hello begreppet Seed-noder. Dessa är noder som underhåller hello tillgängligheten för underliggande hello-klustret. Dessa noder hjälpa tooensure hello klustret fortfarande in genom att upprätta lån med andra noder och fungerar som tiebreakers under vissa typer av nätverksfel. Om de inte återförts, slumpmässiga fel ta bort en majoritet av hello seed noder i klustret hello stängs hello klustret automatiskt. I Azure, hanteras automatiskt Seed noder: de distribueras över hello tillgängliga fel- och uppgraderingsdomäner och om en enda seed-nod har tagits bort från hello klustret skapas en annan i dess ställe. 

I både fristående Service Fabric-kluster och Azure är hello ”primära nodtypen” hello en som kör hello frö. När du definierar en primära nodtypen Service Fabric automatiskt att dra nytta av hello antalet noder som tillhandahålls av skapar too9 seed noder och 9 repliker för varje hello systemtjänster. Om en uppsättning slumpmässiga fel tar ut en majoritet av dessa system service repliker samtidigt ange hello systemtjänster kvorum försvinner, som det beskrivs ovan. Om en majoritet av hello seed noder går förlorade, stängs hello klustret strax efter.

## <a name="next-steps"></a>Nästa steg
- Lär dig hur toosimulate olika fel med hello [datatillgång framework](service-fabric-testability-overview.md)
- Läs andra resurser för katastrofåterställning och hög tillgänglighet. Microsoft har publicerat en stor mängd information i dessa avsnitt. Även om vissa av de här dokumenten finns toospecific tekniker för användning i andra produkter innehåller många allmänna metodtips som du kan använda i hello Service Fabric-kontexten också:
  - [Tillgänglighetschecklista](../best-practices-availability-checklist.md)
  - [Utför en katastrofåterställning återställningsgranskning](../sql-database/sql-database-disaster-recovery-drills.md)
  - [Katastrofåterställning och hög tillgänglighet för Azure-program][dr-ha-guide]
- Lär dig mer om [Service Fabric-supportalternativen](service-fabric-support.md)

<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
