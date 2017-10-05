---
title: "Vanliga frågor om Microsoft Azure Service Fabric | Microsoft Docs"
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
ms.openlocfilehash: ee1fe4e83ce796fd50b779c0880701b9dfcefff7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="commonly-asked-service-fabric-questions"></a>Vanliga frågor för Service Fabric

Det finns många vanliga frågor om vad Service Fabric kan göra och hur den ska användas. Det här dokumentet beskrivs många av dessa vanliga frågor och deras svar.

## <a name="cluster-setup-and-management"></a>Konfigurera och hantera kluster

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions-or-my-own-datacenters"></a>Kan jag skapa ett kluster som sträcker sig över flera Azure-regioner eller egna Datacenter?

Ja. 

Core Service Fabric-kluster teknik kan användas för att kombinera datorer var som helst i världen, så länge som de har nätverksanslutning till varandra. Skapa och köra sådant kluster kan dock vara komplicerat.

Om du är intresserad av att det här scenariot kan du gärna komma i kontakt antingen via den [Service Fabric Github problem listan](https://github.com/azure/service-fabric-issues) eller via din supportrepresentant för att få ytterligare hjälp. Service Fabric-teamet arbetar för att ge ytterligare tydlighetens skull, vägledning och rekommendationer för det här scenariot. 

Några saker som du bör tänka på: 

1. Service Fabric-klusterresursen i Azure är regionala idag, eftersom virtuella datorns skaluppsättning anger att klustret bygger på. Det innebär att vid regionala fel kan förlorar du möjligheten att hantera kluster via Azure Resource Manager eller i Azure-portalen. Detta kan inträffa även om klustret är igång och du skulle kunna interagera med den direkt. Dessutom erbjuder Azure idag inte möjlighet att ha ett virtuellt nätverk som kan användas över regioner. Detta innebär att ett kluster med flera region i Azure kräver antingen [offentliga IP-adresser för varje virtuell dator i den Skalningsuppsättningar](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine) eller [Azure VPN-gatewayer](../vpn-gateway/vpn-gateway-about-vpngateways.md). Valen nätverk har olika påverkan på kostnader, prestanda, och att vissa programdesign grad så noggrann analys och planering krävs innan stående in sådan miljö.
2. Underhåll, hantering, och övervakning av dessa datorer kan bli komplicerad, särskilt när omfattas över _typer_ av miljöer mellan olika molntjänstleverantörer eller mellan lokala resurser och Azure. Vara måste försiktig så att uppgraderingar, övervakning, hantering och diagnostik förstås för både klustret och program innan du kör produktionsarbetsbelastningar i en sådan miljö. Om du redan har stor erfarenhet lösa dessa problem i Azure eller i ditt eget datacenter, är det troligt att dessa samma lösningar kan användas när bygga ut eller kör Service Fabric-klustret. 

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Får Service Fabric-noder automatiskt operativsystemuppdateringar?

Inte idag, men det är också en gemensam begäran som Azure har för avsikt att leverera.

Under tiden kan vi har [som ett program](service-fabric-patch-orchestration-application.md) att operativsystemen under Service Fabric-noder hålls uppdaterad och uppdaterad.

Utmaningen med operativsystemuppdateringar är de vanligtvis kräver en omstart av datorn, vilket resulterar i tillfälliga tillgänglighetsproblem. Som är inte ett problem Eftersom Service Fabric automatiskt omdirigera trafik för dessa tjänster till andra noder. Men om operativsystemuppdateringar inte koordineras hela klustret, finns det risk att många noder gå ned på samma gång. Sådana samtidiga omstarter kan gå förlorade fullständig tillgängligheten för en tjänst eller på minst för en specifik partition (för en tillståndskänslig service).

I framtiden, planerar vi att stödja en princip för OS som helt automatiserad och koordineras update domäner säkerställer att tillgängligheten bibehålls trots omstarter och andra oväntade fel.

### <a name="can-i-use-large-virtual-machine-scale-sets-in-my-sf-cluster"></a>Kan jag använda stora Skalningsuppsättningar i virtuella datorer i min SA kluster? 

**Kort svar** : Nej 

**Svara på länge** – även om stora Skaluppsättningar för virtuell dator kan du skala en virtuell dator skala ange upp till 1000 VM-instanser, sker detta med hjälp av placering grupper (PGs). Feldomäner (FDs) och uppgraderingsdomäner (UDs) är bara konsekvent inom en placering grupp Service fabric använder-FDs och UDs för att fatta beslut om placeringen av repliker/tjänstinstanser din tjänst. Eftersom FDs och UDs är jämförbar endast inom en grupp för placering SA kan inte använda den. Till exempel om VM1 i SG1 har en topologi för FD = 0 och VM9 i SG2 har en topologi för FD = 4, innebär inte att VM1 och VM2 finns på två olika maskinvara rack, därför SA kan inte använda FD värdena i det här fallet att fatta beslut om placeringen.

Det finns andra problem med stora virtuella datorer, som bristen på nivå 4 läsa in stöd för belastningsutjämning. Det finns [information i stor skala uppsättningar](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md)



### <a name="what-is-the-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>Vad är den minimala storleken för ett Service Fabric-kluster? Varför kan det vara mindre?

Den minsta storleken som stöds för ett Service Fabric-kluster som kör produktionsarbetsbelastningar är fem noder. Vi stöder tre kluster med noder för utveckling och testning scenarier.

Dessa minimivärdena finns eftersom Service Fabric-kluster körs en uppsättning tillståndskänslig systemtjänster, inklusive naming service och hanteraren för redundanskluster. Dessa tjänster, vilka spåra vilka tjänster som har distribuerats till klustret och där de är för närvarande finns, är beroende av stark konsekvens. Det stark konsekvent i sin tur är beroende av möjlighet att hämta en *kvorum* för angivna uppdateringar i läget för dessa tjänster, där ett kvorum representerar en strikt majoritet av replikerna (N/2 + 1) för en viss tjänst.

Med den bakgrunden, låt oss nu undersöka vissa klusterkonfigurationer som möjligt:

**En nod**: det här alternativet erbjuder inte hög tillgänglighet eftersom förlust av enskild nod för någon anledning innebär förlust av hela klustret.

**Två noder**: ett kvorum för en tjänst som distribuerats på två noder (N = 2) är 2 (2/2 + 1 = 2). Det går inte att skapa ett kvorum när en enskild replik har gått förlorade. Eftersom en tjänsteuppgradering pågår kräver att tillfälligt stoppa en replik, men detta är inte en användbar konfiguration.

**Tre noder**: med tre noder (N = 3), kravet på att skapa ett kvorum är fortfarande två noder (3/2 + 1 = 2). Det innebär att du förlorar en enskild nod och ändå upprätthålla kvorum.

Klusterkonfigurationen tre noder stöds för utveckling och testning eftersom du på ett säkert sätt kan utföra uppgraderingar och överleva fel på enskilda noder, förutsatt att de inte råkar samtidigt. För produktionsarbetsbelastningar, måste du vara motståndskraftiga mot sådana ett samtidigt fel, så att fem noder krävs.

### <a name="can-i-turn-off-my-cluster-at-nightweekends-to-save-costs"></a>Kan jag inaktivera min kluster på natten/helger för att minska kostnaderna?

I allmänhet inte. Service Fabric lagrar tillstånd på lokala tillfälliga diskar, vilket innebär att om den virtuella datorn flyttas till en annan värd, inte flyttar data med den. Under normal drift som är inte ett problem som den nya noden tas uppdaterade av andra noder. Om du stoppar alla noder och starta om dem senare, finns det dock en betydande risk som de flesta av noderna som börjar på nya värdar och gör det går inte att återställa systemet.

Om du vill skapa kluster för att testa programmet innan det distribueras rekommenderar vi att du skapar dessa kluster dynamiskt som en del av din [kontinuerlig integration/kontinuerlig distribution pipeline](service-fabric-set-up-continuous-integration.md).


### <a name="how-do-i-upgrade-my-operating-system-for-example-from-windows-server-2012-to-windows-server-2016"></a>Hur uppgraderar jag mitt operativsystem (till exempel från Windows Server 2012 till Windows Server 2016)?

Du är ansvarig för att uppgradera medan vi arbetar på en bättre upplevelse idag. Du måste uppgradera den OS-avbildningen på de virtuella datorerna i klustret en virtuell dator i taget. 

## <a name="container-support"></a>Stöd för behållaren

### <a name="why-are-my-containers-that-are-deployed-to-sf-unable-to-resolve-dns-addresses"></a>Varför är min behållare som har distribuerats till SA inte matcha DNS-adresser?

Det här problemet har rapporterats på kluster som finns på 5.6.204.9494 version 

**Minskning** : Följ [dokumentet](service-fabric-dnsservice.md) att aktivera DNS-service fabric-tjänsten i klustret.

**Åtgärda** : uppgradering till en version som stöds kluster som är högre än 5.6.204.9494, när den är tillgänglig. Om klustret är automatiska uppgraderingar uppgraderar automatiskt klustret till den version som har problemet lösts.

  
## <a name="application-design"></a>Programmet Design

### <a name="whats-the-best-way-to-query-data-across-partitions-of-a-reliable-collection"></a>Vad är det bästa sättet att fråga efter data mellan partitioner i en tillförlitlig samling?

Tillförlitliga samlingar är vanligtvis [partitionerade](service-fabric-concepts-partitioning.md) att skala ut för bättre prestanda och genomflöde. Det innebär att tillståndet för en viss tjänst kan spridas över 10-tal eller 100-tal datorer. Om du vill utföra åtgärder via den fullständiga datauppsättningen, har du några alternativ:

- Skapa en tjänst som frågar alla partitioner i en annan tjänst att dra in nödvändiga data.
- Skapa en tjänst som kan ta emot data från alla partitioner i en annan tjänst.
- Skicka data regelbundet från varje tjänst till en extern butik. Den här metoden är endast korrekt om de frågor som du utför inte är en del av affärslogiken kärnor.


### <a name="whats-the-best-way-to-query-data-across-my-actors"></a>Vad är det bästa sättet att fråga efter data över min aktörer?

Aktörer är avsedda att vara oberoende enheter av tillstånd och beräkning, så inte rekommenderas att utföra bred frågor för aktörstillstånd vid körning. Om du behöver fråga i den fullständiga uppsättningen aktörstillstånd, bör du antingen:

- Ersätt din aktörstjänster med tillståndskänsliga reliable services så att antalet nätverket begäranden att samla in alla data från antalet aktörer med antalet partitioner i din tjänst.
- Designa din aktörer att regelbundet skicka deras tillstånd till en extern butik för enklare frågor. Som är ovan, denna metod bara användbara om frågor som du utför inte krävs för din runtime-beteende.

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>Hur mycket data som kan lagra i en tillförlitlig samling?

Reliable services är vanligtvis partitionerade så mycket du kan lagra begränsas bara av antalet datorer som du har i klustret och mängden tillgängligt minne på dessa datorer.

Ett exempel anta att du har en tillförlitlig samling i en tjänst med 100 partitioner och 3 repliker, lagra objekt som genomsnittlig storlek på 1kb. Anta att du har ett 10 datorn kluster med 16gb minne per dator. För enkelhetens skull och för att vara mycket försiktig, förutsätts att operativsystemet och systemtjänster, Service Fabric-runtime och dina tjänster kan du använda 6gb, lämnar 10gb tillgängligt per dator eller 100gb för klustret.

Med tanke på att varje objekt måste vara lagrade tre gånger (en primär och två repliker), har du tillräckligt med minne för ungefär 35 miljoner objekt i samlingen när du arbetar med full kapacitet. Vi rekommenderar dock att motståndskraftiga mot samtidiga förlust av en fel-domän och en uppgraderingsdomän som representerar ungefär 1/3 av kapacitet och kan minska antalet till ungefär 23 miljoner.

Observera att den här beräkningen förutsätter också:

- Att fördelning av data över partitioner är ungefär uniform eller att du rapporterar belastningen mått till klustret Resource Manager. Som standard kommer Service Fabric belastningsutjämna baserat på replikantalet. I vårt exempel som placerar 10 primära repliker och 20 sekundära repliker på varje nod i klustret. Som fungerar bra för belastningen jämnt fördelad över partitioner. Om belastningen inte är ENS, måste du rapportera belastning så att resurshanteraren kan packa ihop mindre repliker och Tillåt större repliker förbruka mer minne på en enskild nod.

- Att är tillförlitlig tjänsten i fråga tillståndet för en enda lagring i klustret. Eftersom du kan distribuera flera tjänster till ett kluster, behöver du vara uppmärksam på resurser att varje måste köra och hantera dess tillstånd.

- Att själva klustret inte växer eller krymper. Om du lägger till flera datorer, kommer Service Fabric balansera repliker för att kunna utnyttja ytterligare kapacitet tills antalet datorer överskrider antalet partitioner i din tjänst eftersom datorer inte kan finnas på en enskild replik. Däremot om du minskar storleken på klustret genom att ta bort datorer repliker packade tätare och har mindre totala kapaciteten.

### <a name="how-much-data-can-i-store-in-an-actor"></a>Hur mycket data som kan lagra i en aktör?

Precis som med tillförlitlig services begränsas mängden data som du kan lagra i en aktören tjänst bara av totalt diskutrymme och tillgängligt minne över noderna i klustret. Enskilda aktörer är mest effektiva när de används för att kapsla in en liten mängd tillstånd och associerade affärslogik. Som regel bör en enskild aktör har tillstånd som mäts i kilobyte.

## <a name="other-questions"></a>Andra frågor

### <a name="how-does-service-fabric-relate-to-containers"></a>Hur relaterar Service Fabric till behållare?

Behållare erbjuder ett enkelt sätt att paketet tjänster och deras beroenden så att de kör konsekvent i alla miljöer och kan fungera i isolerat läge på en enskild dator. Service Fabric är ett sätt att distribuera och hantera tjänster, inklusive [tjänster som har paketerats i en behållare](service-fabric-containers-overview.md).

### <a name="are-you-planning-to-open-source-service-fabric"></a>Planerar du att öppna datakällan Service Fabric?

Vi planerar att öppna datakällan tillförlitliga tjänster och tillförlitlig aktörer ramverk på GitHub och accepterar communitybidrag till dessa projekt. Följ den [Service Fabric-blogg](https://blogs.msdn.microsoft.com/azureservicefabric/) för mer information som de är tillkännages.

Är för närvarande inga planer på att öppna datakällan Service Fabric-körningsmiljön.

## <a name="next-steps"></a>Nästa steg

- [Lär dig mer om grundläggande begrepp som Service Fabric och bästa praxis](https://mva.microsoft.com/en-us/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965)
