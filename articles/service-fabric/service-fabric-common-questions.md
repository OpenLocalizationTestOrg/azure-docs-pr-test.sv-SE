---
title: "aaaCommon frågor om Microsoft Azure Service Fabric | Microsoft Docs"
description: "Vanliga frågor och svar om Service Fabric och deras svar"
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: 
ms.assetid: 5a179703-ff0c-4b8e-98cd-377253295d12
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: chackdan
ms.openlocfilehash: 4cbe92d2a03f7a1ea5d077807fdc982288220a7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="commonly-asked-service-fabric-questions"></a>Vanliga frågor för Service Fabric

Det finns många vanliga frågor om vad Service Fabric kan göra och hur den ska användas. Det här dokumentet beskrivs många av dessa vanliga frågor och deras svar.

## <a name="cluster-setup-and-management"></a>Konfigurera och hantera kluster

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions-or-my-own-datacenters"></a>Kan jag skapa ett kluster som sträcker sig över flera Azure-regioner eller egna Datacenter?

Ja. 

hello core Service Fabric-kluster teknik kan vara används toocombine datorer var som helst i hello world så länge som de har network connectivity tooeach andra. Skapa och köra sådant kluster kan dock vara komplicerat.

Om du är intresserad av att det här scenariot gärna tooget i Kontakta antingen via hello [Service Fabric Github problem listan](https://github.com/azure/service-fabric-issues) eller via din supportrepresentant i ordning tooobtain ytterligare hjälp. hello Service Fabric-teamet arbetar tooprovide ytterligare tydlighetens skull, vägledning och rekommendationer för det här scenariot. 

Vissa saker tooconsider: 

1. hello resurs i Azure Service Fabric är regional idag, som är hello virtuella skalningsuppsättningarna hello klustret bygger på. Detta innebär att hello regionala fel för händelsen du kan förlora möjligheten hello toomanage hello klustret via hello Azure Resource Manager eller hello Azure-portalen. Detta kan inträffa även om hello klustret är igång och du kommer att kunna toointeract med det direkt. Dessutom erbjuder Azure idag inte hello möjlighet toohave ett virtuellt nätverk som kan användas över regioner. Detta innebär att ett kluster med flera region i Azure kräver antingen [offentliga IP-adresser för varje virtuell dator i hello Skalningsuppsättningar](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine) eller [Azure VPN-gatewayer](../vpn-gateway/vpn-gateway-about-vpngateways.md). Valen nätverk har olika påverkan på kostnader, prestanda och toosome grad programdesign, så noggrann analys och planering krävs före stående in sådan miljö.
2. hello underhåll, hantering och övervakning av dessa datorer kan bli komplicerad, särskilt när omfattas över _typer_ av miljöer mellan olika molntjänstleverantörer eller mellan lokala resurser och Azure . Var försiktig tooensure som uppgraderar, övervakning, hantering och diagnostik förstås för både hello och hello program innan du kör produktionsarbetsbelastningar i en sådan miljö. Om du redan har stor erfarenhet lösa dessa problem i Azure eller i ditt eget datacenter, är det troligt att dessa samma lösningar kan användas när bygga ut eller kör Service Fabric-klustret. 

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Får Service Fabric-noder automatiskt operativsystemuppdateringar?

Inte idag, men det är också en gemensam begäran om att Azure avser toodeliver.

I hello tillfälligt, har vi [som ett program](service-fabric-patch-orchestration-application.md) att hello operativsystem under Service Fabric-noder hålls uppdaterad och uppåt toodate.

hello utmaningen med operativsystemuppdateringar är de vanligtvis kräver en omstart av hello datorn, vilket resulterar i tillfälliga tillgänglighetsproblem. Som är inte ett problem Eftersom Service Fabric att automatiskt omdirigera trafik för de tjänster tooother noderna. Om operativsystemuppdateringar inte koordineras över hello kluster, är det dock hello risk att många noder gå ned på samma gång. Sådana samtidiga omstarter kan gå förlorade fullständig tillgängligheten för en tjänst eller på minst för en specifik partition (för en tillståndskänslig service).

I framtida hello kommer principen för toosupport ett operativsystem som är helt automatiserad och koordineras update domäner säkerställer att tillgängligheten bibehålls trots omstarter och andra oväntade fel.

### <a name="can-i-use-large-virtual-machine-scale-sets-in-my-sf-cluster"></a>Kan jag använda stora Skalningsuppsättningar i virtuella datorer i min SA kluster? 

**Kort svar** : Nej 

**Svara på länge** – även om hello stora Skalningsuppsättningar i virtuella datorer kan tooscale en virtuell dator skala ange upp till 1000 VM-instanser, sker detta med hello placering grupper (PGs). Feldomäner (FDs) och uppgraderingsdomäner (UDs) är bara konsekvent inom en placering grupp Service fabric använder FDs och UDs toomake placering beslut för din tjänstinstanser repliker-tjänsten. Eftersom hello FDs och UDs är jämförbar endast inom en grupp för placering SA kan inte använda den. Till exempel om VM1 i SG1 har en topologi för FD = 0 och VM9 i SG2 har en topologi för FD = 4, innebär inte att VM1 och VM2 finns på två olika maskinvara rack, därför SA kan inte använda hello FD värdena i det här fallet toomake placering beslut.

Det finns andra problem med stora virtuella datorer, som hello brist på nivå 4 läsa in stöd för belastningsutjämning. Se toofor [information i stor skala uppsättningar](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md)



### <a name="what-is-hello-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>Vad är hello minimala storleken för ett Service Fabric-kluster? Varför kan det vara mindre?

hello stöds minimistorleken för ett Service Fabric-kluster som kör produktionsarbetsbelastningar är fem noder. Vi stöder tre kluster med noder för utveckling och testning scenarier.

Dessa minimivärdena finns eftersom hello Service Fabric-kluster körs en uppsättning med tillståndskänsliga tjänster, inklusive hello namngivningstjänst och hello failover manager. Dessa tjänster, som spårar vad tjänster har distribuerats toohello klustret och där de för närvarande är värd, är beroende av stark konsekvens. Det stark konsekvent i sin tur är beroende av hello möjlighet tooacquire en *kvorum* för en viss uppdatering toohello läget för dessa tjänster, där ett kvorum representerar en strikt majoritet av replikerna hello (N/2 + 1) för en viss tjänst.

Med den bakgrunden, låt oss nu undersöka vissa klusterkonfigurationer som möjligt:

**En nod**: det här alternativet inte ge hög tillgänglighet eftersom hello förlust av hello nod av någon anledning innebär hello förlust av hello hela klustret.

**Två noder**: ett kvorum för en tjänst som distribuerats på två noder (N = 2) är 2 (2/2 + 1 = 2). När en enskild replik tappas bort, är det omöjligt toocreate ett kvorum. Eftersom en tjänsteuppgradering pågår kräver att tillfälligt stoppa en replik, men detta är inte en användbar konfiguration.

**Tre noder**: med tre noder (N = 3), hello krav toocreate ett kvorum är fortfarande två noder (3/2 + 1 = 2). Det innebär att du förlorar en enskild nod och ändå upprätthålla kvorum.

hello tre nod klusterkonfigurationen stöds för utveckling och testning eftersom du på ett säkert sätt kan utföra uppgraderingar och överleva fel på enskilda noder, så länge som de inte råkar samtidigt. Du måste vara flexibel toosuch ett samtidigt fel så fem noder krävs för produktionsarbetsbelastningar.

### <a name="can-i-turn-off-my-cluster-at-nightweekends-toosave-costs"></a>Kan jag inaktivera min klustret på natten/helger toosave kostnader?

I allmänhet inte. Service Fabric lagrar tillstånd på lokala tillfälliga diskar, vilket innebär att om hello virtuell dator har flyttats tooa olika värden, inte flyttar hello data med den. Under normal drift som är inte ett problem som hello nya noden tas upp toodate av andra noder. Om du stoppar alla noder och starta om dem senare, finns det dock en betydande risk för att de flesta av hello noder starta på nya värdar och göra hello system toorecover.

Om du vill att toocreate kluster för att testa programmet innan det distribueras, rekommenderar vi att du skapar dessa kluster dynamiskt som en del av din [kontinuerlig integration/kontinuerlig distribution pipeline](service-fabric-set-up-continuous-integration.md).


### <a name="how-do-i-upgrade-my-operating-system-for-example-from-windows-server-2012-toowindows-server-2016"></a>Hur uppgraderar jag mitt operativsystem (till exempel från Windows Server 2012 tooWindows Server 2016)?

Du är ansvarig för hello uppgraderingen medan vi arbetar på en bättre upplevelse idag. Du måste uppgradera hello OS-avbildningen på hello virtuella datorer på hello kluster en virtuell dator i taget. 

## <a name="container-support"></a>Stöd för behållaren

### <a name="why-are-my-containers-that-are-deployed-toosf-unable-tooresolve-dns-addresses"></a>Varför är min behållare som är distribuerade tooSF tooresolve DNS-adresser?

Det här problemet har rapporterats på kluster som finns på 5.6.204.9494 version 

**Minskning** : Följ [dokumentet](service-fabric-dnsservice.md) tooenable hello DNS service fabrikstjänsten i klustret.

**Åtgärda** : uppgradera tooa stöds kluster av version som är högre än 5.6.204.9494, när den är tillgänglig. Om klustret är tooautomatic uppgraderingar, uppgraderar toohello-version som har problemet lösts automatiskt i hello klustret.

  
## <a name="application-design"></a>Programmet Design

### <a name="whats-hello-best-way-tooquery-data-across-partitions-of-a-reliable-collection"></a>Vad är hello bästa sätt tooquery data mellan partitioner i en tillförlitlig samling?

Tillförlitliga samlingar är vanligtvis [partitionerade](service-fabric-concepts-partitioning.md) tooenable skala ut för bättre prestanda och genomflöde. Det innebär att hello tillstånd för en viss tjänst kan spridas över 10-tal eller 100-tal datorer. tooperform åtgärder via den fullständiga datauppsättningen, har du några alternativ:

- Skapa en tjänst som frågar alla partitioner i en annan tjänst toopull i hello nödvändiga data.
- Skapa en tjänst som kan ta emot data från alla partitioner i en annan tjänst.
- Med jämna mellanrum skicka data från varje tjänst tooan extern butik. Den här metoden är endast korrekt om hello frågor som du utför inte är en del av affärslogiken kärnor.


### <a name="whats-hello-best-way-tooquery-data-across-my-actors"></a>Vad är hello bästa sätt tooquery data över min aktörer?

Aktörer är utformad toobe oberoende enheter av tillstånd och beräkning, så det inte är rekommenderade tooperform bred frågor för aktörstillstånd vid körning. Om du har en behovet tooquery över hello fullständig uppsättning aktörstillstånd bör du antingen:

- Ersätt din aktörstjänster med tillståndskänsliga reliable services så att hello antal nätverk begäranden toogather alla data från hello antal aktörer toohello antalet partitioner i din tjänst.
- Designa din aktörer tooperiodically push sina tooan externa tillståndslager för enklare frågor. Som är ovan, denna metod bara användbara om hello frågor som du utför inte krävs för din runtime-beteende.

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>Hur mycket data som kan lagra i en tillförlitlig samling?

Reliable services partitioneras vanligtvis, så hello belopp som du kan lagra begränsas bara av hello antalet datorer som du har i hello kluster och hello mängden tillgängligt minne på dessa datorer.

Ett exempel anta att du har en tillförlitlig samling i en tjänst med 100 partitioner och 3 repliker, lagra objekt som genomsnittlig storlek på 1kb. Anta att du har ett 10 datorn kluster med 16gb minne per dator. För enkelhetens skull toobe mycket försiktig förutsätter att hello operativsystem och systemtjänster hello Service Fabric runtime och dina tjänster använder 6gb, lämnar 10gb tillgängligt per dator eller 100gb för hello-kluster.

Med tanke på att varje objekt måste vara lagrade tre gånger (en primär och två repliker), har du tillräckligt med minne för ungefär 35 miljoner objekt i samlingen när du arbetar med full kapacitet. Vi rekommenderar dock att flexibel toohello samtidiga förlust av en fel-domän och en uppgraderingsdomän som representerar ungefär 1/3 av kapacitet och skulle minska hello nummer tooroughly 23 miljoner.

Observera att den här beräkningen förutsätter också:

- Att hello fördelning av data mellan hello partitioner är ungefär uniform eller att du rapporterar belastningen mått toohello klustret Resource Manager. Som standard kommer Service Fabric belastningsutjämna baserat på replikantalet. I vårt exempel som placerar 10 primära repliker och 20 sekundära repliker på varje nod i klustret hello. Som fungerar bra för belastningen jämnt fördelad över hello partitioner. Om belastningen inte är ENS, måste du rapportera belastning så att hello Resource Manager kan packa ihop mindre repliker och Tillåt större repliker tooconsume mer minne på en enskild nod.

- Hello tillförlitliga tjänsten i fråga är hello status för en enda lagring i hello kluster. Eftersom du kan distribuera flera tjänster tooa klustret måste toobe uppmärksam på hello resurser att varje måste toorun och hantera dess tillstånd.

- Hello klustret själva är inte växer eller krymper. Om du lägger till flera datorer balansera Service Fabric din ytterligare kapacitet för repliker tooleverage hello tills hello antalet datorer överskrider hello antalet partitioner i tjänsten, eftersom datorer inte kan finnas på en enskild replik. Däremot om du minskar hello storleken på hello kluster genom att ta bort datorer repliker packade tätare och har mindre totala kapaciteten.

### <a name="how-much-data-can-i-store-in-an-actor"></a>Hur mycket data som kan lagra i en aktör?

Precis som med tillförlitlig services är hello mängden data som du kan lagra i en aktören tjänst begränsas bara av hello totalt diskutrymme och tillgängligt minne över hello noder i klustret. Enskilda aktörer är mest effektiva när de används tooencapsulate en liten mängd tillstånd och associerade affärslogik. Som regel bör en enskild aktör har tillstånd som mäts i kilobyte.

## <a name="other-questions"></a>Andra frågor

### <a name="how-does-service-fabric-relate-toocontainers"></a>Hur relaterar Service Fabric toocontainers?

Behållare erbjuder ett enkelt sätt toopackage tjänster och deras beroenden så att de kör konsekvent i alla miljöer och kan fungera i isolerat läge på en enskild dator. Service Fabric erbjuder ett sätt toodeploy och hantera tjänster, inklusive [tjänster som har paketerats i en behållare](service-fabric-containers-overview.md).

### <a name="are-you-planning-tooopen-source-service-fabric"></a>Planerar du tooopen källa Service Fabric?

Vi planerar tooopen källa hello tillförlitliga tjänster och tillförlitlig aktörer ramverk på GitHub och accepterar community bidrag toothose projekt. Följ hello [Service Fabric-blogg](https://blogs.msdn.microsoft.com/azureservicefabric/) för mer information som de är tillkännages.

hello finns för närvarande inga planer tooopen källa hello Service Fabric runtime.

## <a name="next-steps"></a>Nästa steg

- [Lär dig mer om grundläggande begrepp som Service Fabric och bästa praxis](https://mva.microsoft.com/en-us/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965)
