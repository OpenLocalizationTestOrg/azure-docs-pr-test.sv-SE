---
title: "aaaIntroducing hello resurshanteraren för Service Fabric-kluster | Microsoft Docs"
description: "En introduktion toohello resurshanteraren för Service Fabric-klustret."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: cfab735b-923d-4246-a2a8-220d4f4e0c64
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: e815925880e2f3a755294de1dcfb9b88fbdde08a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introducing-hello-service-fabric-cluster-resource-manager"></a>Introduktion till resurshanteraren för hello Service Fabric-kluster
Traditionellt hantera IT-system eller onlinetjänster avsedda trafikklass särskilda fysiska eller virtuella datorer toothose vissa tjänster eller system. Tjänster har konstruerad som nivåer. Det vore en nivå med ”web” och ”data” eller ”lagring”-nivån. Program skulle ha en meddelanden nivå där förfrågningar gått in och ut, samt en uppsättning datorer dedikerade toocaching. Varje nivå eller en typ av arbetsbelastning hade specifika datorer dedikerade tooit: hello databasen har fått några datorer dedikerade tooit, hello webbservrar några. Om en viss typ av arbetsbelastning orsakade hello-datorer som den var på toorun för hot, till flera datorer med samma konfiguration toothat nivån. Dock inte alla arbetsbelastningar kan inte skalas ut så enkelt - särskilt med hello datanivå du vanligtvis ersätta datorer med större datorer. Enkelt. Om en dator inte kördes som en del av den totala programmet hello på lägre kapacitet tills hello datorn kunde återställas. Fortfarande ganska enkelt (om det är inte nödvändigtvis roligt).

Nu men hello world för tjänsten och programvaruarkitekturen har ändrats. Nu är det numera vanligast att program har antagit en skalbar design. Det är vanligt att bygga program med behållare eller mikrotjänster (eller båda). Nu när du kanske endast några datorer, kör de inte en enda instans av en arbetsbelastning. De kan även köra flera olika arbetsbelastningar på hello samtidigt. Nu har du mängder av olika typer av tjänster (ingen förbrukar en fullständig dator kan du se resurser) kanske hundratals olika instanser av tjänsterna. Varje namngiven instans har en eller flera instanser repliker för hög tillgänglighet. Beroende på hello storlekarna för dessa arbetsbelastningar och hur upptagen som de är, kan du själv med hundratals eller tusentals datorer. 

Plötsligt hantera din miljö är inte så enkelt som att hantera några datorer dedikerade toosingle typer av arbetsbelastningar. Dina servrar är virtuell och inte längre har namn (du har växlat mindsets från [husdjur toocattle](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) när alla). Konfigurationen är mindre om hello datorer och mer information om hello tjänster sig själva. Maskinvara som är dedikerad tooa instans av en arbetsbelastning är i stort sett något hello tidigare. Tjänsterna har blivit små distribuerade system som sträcker sig över flera mindre delar av vanlig hårdvara.

Eftersom din app inte längre är en serie monoliths fördelade på flera nivåer, nu har du många flera kombinationer toodeal med. Som bestämmer vilka typer av arbetsbelastningar kan köras på vilken maskinvara, eller hur många? Vilka arbetsbelastningar fungerar bra på hello samma maskinvara och som orsakar en konflikt? Om en dator går ned gör du vet vad det har körts på den datorn? Vem är ansvarig för att se till att arbetsbelastningar börjar köras igen? Vänta för hello (virtuellt)? datorn toocome tillbaka eller dina arbetsbelastningar automatiskt redundansväxla tooother datorer och hålla kör? Är mänsklig inblandning krävs? Nyheter om uppgraderingar i den här miljön?

Som utvecklare och operatorer som rör i den här miljön måste vi toowant hur du hanterar den här komplexitet. En uthyrning binge och försök toohide hello komplexiteten med personer är förmodligen inte hello fel, så vad vi gör?

## <a name="introducing-orchestrators"></a>Introduktion till orchestrators
”Orchestrator” är hello allmänna termen för programvara som hjälper administratörer att hantera dessa typer av miljöer. Orchestrators är hello-komponenter som ta begäranden som ”jag vill fem kopior av den här tjänsten som körs i Min miljö”. De försöker toomake hello miljö matchar hello önskad tillstånd, oavsett vad som händer.

Orchestrators (inte människor) är vad vidta åtgärder när en dator misslyckas eller en arbetsbelastning avslutas oväntat önskemål. De flesta orchestrators mer än bara hantera fel. Andra funktioner som de har hanterar nya distributioner, hanterar uppgraderingar och hantera resursförbrukning och styrning. Alla orchestrators är mycket om hur du underhåller vissa tillstånd i konfigurationen i hello-miljö. Du vill toobe kan tootell en orchestrator vad du vill använda och det hello frekventa lyft. Aurora ovanpå Mesos Docker Datacenter/Docker Swarm, Kubernetes och Service Fabric är alla exempel på orchestrators. Dessa orchestrators som aktivt utvecklade toomeet hello behov av verkliga arbetsbelastningar i produktionsmiljöer. 

## <a name="orchestration-as-a-service"></a>Orchestration som en tjänst
hello klustret Resource Manager är hello systemkomponenter som hanterar orchestration i Service Fabric. hello klustret Resource Manager-jobb är uppdelad i tre delar:

1. Tillämpa regler
2. Optimera din miljö
3. Hjälper till med andra processer

toosee hur hello klustret Resource Manager fungerar, titta på hello följande Microsoft Virtual Academy video:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=d4tka66yC_5706218965">
<img src="./media/service-fabric-cluster-resource-manager-introduction/ConceptsAndDemoVid.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="what-it-isnt"></a>Det finns inte
I traditionella N nivåer program finns alltid en [belastningsutjämnaren](https://en.wikipedia.org/wiki/Load_balancing_(computing)). Detta är vanligtvis en belastningsutjämnare (Utjämning av nätverksbelastning) eller ett program belastningen belastningsutjämnare (ALB) beroende på där den lör i hello nätverksstacken. Vissa belastningsutjämnare är maskinvarubaserad som F5 BigIP erbjudande, andra är baserade på programvara, till exempel Microsoft har NLB. I andra miljöer, kan du se något som HAProxy, nginx, Istio eller Envoy i den här rollen. I dessa arkitekturer hello jobb av belastningsutjämning är tooensure tillståndslösa arbetsbelastningar (ungefär) får hello samma mängd arbete. Strategier för att balansera läsa in olika. Vissa belastningsutjämnare skulle skicka varje annat tooa annan server. Andra angetts fästning session/varaktighet. Mer avancerade belastningsutjämnare använder faktiska belastningen uppskattning eller reporting tooroute ett anrop baserat på dess förväntade kostnader och aktuella belastningen på datorn.

Belastningsutjämning för nätverk eller meddelandet routrar försökte tooensure som hello web/worker nivån låg ungefär belastningsutjämnade. Strategier för att balansera hello datanivå har olika och beroende på hello mekanism för lagring av data. NLB hello datanivå förlita sig på data horisontell partitionering cachelagring hanterade vyer, lagrade procedurer och andra store-specifika metoder.

Även om vissa av dessa strategier är intressanta är hello resurshanteraren för Service Fabric-klustret inte något som en Utjämning av nätverksbelastning eller en cache. En Utjämning av nätverksbelastning balanserar frontends genom att sprida trafik över frontends. hello resurshanteraren för Service Fabric-klustret har en annan strategi. Grunden, Service Fabric flyttar *services* toowhere som de gör hello bäst, förväntas trafik eller läsa in toofollow. Det kan till exempel flytta services toonodes som för närvarande cold eftersom hello-tjänster som är det inte gör mycket arbete. hello noder kanske cold eftersom hello-tjänsterna som fanns har tagits bort eller flyttats någon annanstans. Ett annat exempel är kan hello klustret Resource Manager också flytta en tjänst från en dator. Kanske hello datorn är på väg toobe uppgraderas eller är överbelastad på grund av tooa topp förbrukning av hello-tjänster som körs på den. Alernatively, hello service resurskrav kan har ökat. Därför det inte finns tillräckligt med resurser på den här datorn toocontinue körs. 

Eftersom hello klustret Resource Manager ansvarar för att flytta tjänster, innehåller en annan funktion set jämfört med toowhat som finns i en Utjämning av nätverksbelastning. Det beror på att nätverksbelastningsutjämnare leverera nätverket trafik toowhere tjänster är redan, även om platsen inte är idealiskt för att köra själva hello-tjänsten. hello resurshanteraren för Service Fabric-kluster använder helt olika strategier för att säkerställa att hello resurserna i hello kluster är effektiv.

## <a name="next-steps"></a>Nästa steg
- Information om hello arkitektur och information om flödet i hello klustret Resource Manager kolla [den här artikeln](service-fabric-cluster-resource-manager-architecture.md)
- hello klustret Resource Manager har många alternativ för att beskriva hello klustret. toofind mer information om mått checka ut den här artikeln på [som beskriver ett Service Fabric-kluster](service-fabric-cluster-resource-manager-cluster-description.md)
- Mer information om hur du konfigurerar tjänster, [Lär dig mer om hur du konfigurerar tjänster](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)
- Mått är hur hello Service Fabric-kluster Resource Manager hanterar förbrukning och kapacitet i hello klustret. Mer om mått och hur tooconfigure dem kolla toolearn [i den här artikeln](service-fabric-cluster-resource-manager-metrics.md)
- hello klustret Resource Manager fungerar med funktioner för hantering av Service Fabric. toofind mer information om att integration läsa [i den här artikeln](service-fabric-cluster-resource-manager-management-integration.md)
- toofind ut om hur hello klustret Resource Manager hanterar och balanserar belastningen i hello kluster kolla hello artikel på [belastningsutjämning](service-fabric-cluster-resource-manager-balancing.md)
