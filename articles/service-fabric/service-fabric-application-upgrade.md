---
title: uppgradering av aaaService Fabric programmet | Microsoft Docs
description: "Den här artikeln innehåller en introduktion tooupgrading ett Service Fabric-program, inklusive att välja uppgradera lägen och utför hälsokontroller."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 803c9c63-373a-4d6a-8ef2-ea97e16e88dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 6f649ef4a5c0afab682522bcba7d2d66a4268ead
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade"></a>Uppgradera Service Fabric-programmet
Ett program med Azure Service Fabric är en samling tjänster. Under en uppgradering, Service Fabric jämför hello nya [programmanifestet](service-fabric-application-model.md#describe-an-application) med hello tidigare version och avgör vilka tjänster i hello program kräver uppdateringar. Service Fabric jämför hello version siffror i hello-tjänsten visar hello versionsnumren i hello tidigare version. Om en tjänst inte har ändrats, uppgraderas att tjänsten inte.

## <a name="rolling-upgrades-overview"></a>Rullande uppgraderingar översikt
I en löpande uppgradering av programmet utförs hello uppgraderingen i etapper. I varje fas är hello uppgradering tillämpade tooa delmängd av noderna i klustret hello, kallas en uppdateringsdomän. Därför kvar hello programmet hello uppgraderingens. Under uppgraderingen hello kan hello kluster innehåller en blandning av hello gamla och nya versioner.

Hello två versioner måste därför vara framåt och bakåt kompatibel. Om de inte är kompatibla ansvarar hello programadministratör för mellanlagring av en flera-fas uppgradera toomaintain tillgänglighet. Uppgraderar en flera-fas uppgraderas hello första steget tooan mellanliggande version av hello-programmet som är kompatibel med hello tidigare version. hello andra steget är tooupgrade hello slutliga versionen som bryter kompatibilitet med hello före uppdateringen version, men som är kompatibel med hello mellanliggande versionen.

Uppdatera domäner har angetts i klustermanifestet hello när du konfigurerar hello kluster. Uppdatera domäner får inte uppdateringar i en viss ordning. En uppdateringsdomän är en logisk enhet för distribution för ett program. Uppdatera domäner tillåta hello tjänster tooremain vid hög tillgänglighet under en uppgradering.

Icke-rullande uppgraderingar kan utföras om hello uppgraderingen är tillämpade tooall noder i hello-kluster, vilket är fallet för hello när hello programmet har bara en uppdateringsdomän. Den här metoden rekommenderas inte eftersom tjänsten hello kraschar och inte är tillgänglig vid hello tiden för uppgraderingen. Dessutom tillhandahåller inte Azure garantier när ett kluster har konfigurerats med endast en uppdateringsdomän.

## <a name="health-checks-during-upgrades"></a>Hälsokontroller under uppgradering
Hälsoprinciper har toobe som angetts för en uppgradering (eller standardvärden kan användas). En uppgradering kallas lyckas när alla update domäner uppgraderas inom hello angivna timeout och när alla uppdaterar domäner bedöms felfritt.  En felfri uppdateringsdomän innebär hello update domänen skickas alla hello hälsokontroller som anges i hello hälsoprincip. Till exempel en hälsoprincip kan behovet av att alla tjänster i en programinstans måste vara *felfri*, som health definieras av Service Fabric.

Hälsoprinciper och kontrollerar under uppgradering av Service Fabric är oberoende av tjänster och program. Det vill säga är inga tjänstspecifika tester klar.  Exempelvis din tjänst kan ha ett genomflöde krav, men Service Fabric saknar hello information toocheck genomflöde. Se toohello [hälsa artiklar](service-fabric-health-introduction.md) för hello-kontroller som utförs. hello kontrollerar att inträffar under en uppgradering inkludera tester för om hello programpaketet har kopierats korrekt om hello-instansen har startats och så vidare.

Hej programhälsan är en sammanställning av hello underordnade entiteter av programmet hello. Kort sagt, utvärderar Service Fabric hello program via hello hälsa som rapporteras till hello programmet hello hälsa. Även utvärderas hello hälsotillståndet för alla hello-tjänster för programmet hello det här sättet. Service Fabric ytterligare utvärderar hello hälsotillstånd hello programtjänster genom att sammanställa hello hälsotillståndet för deras underordnade, till exempel hello service replik. När hello programmets hälsoprincip är uppfyllt, kan hello uppgraderingen fortsätta. Uppgradering av programmet hello misslyckas om hello hälsoprincip har överskridits.

## <a name="upgrade-modes"></a>Uppgradera lägen
hello-läge som vi rekommenderar för uppgradering av programmet är hello övervakas läge, som är vanliga hello läge. Övervakat läge utför hello uppgradering på en uppdateringsdomän och om alla hälsa kontrollerar pass (per hello principen har angetts), flyttar toohello nästa uppdateringsdomän automatiskt.  Om hälsokontroller misslyckas och/eller timeout har uppnåtts, hello uppgraderingen återställs antingen för hello uppdateringsdomän eller hello läge är ändrade toounmonitored manuell. Du kan konfigurera hello uppgradera toochoose dessa två lägen för misslyckad uppgradering. 

Oövervakade manuellt läge måste manuell åtgärd efter varje uppgradering på en uppdateringsdomän, tookick av hello uppgraderingen på hello nästa uppdateringsdomän. Ingen Service Fabric-hälsokontroller utförs. Hej administratör utför hello hälsa och status kontrollerna innan du påbörjar uppgraderingen hello i hello nästa uppdateringsdomän.

## <a name="upgrade-default-services"></a>Uppgradera standardtjänster
Standardtjänster i Service Fabric-programmet kan uppgraderas under hello uppgraderingen av ett program. Standardtjänster definieras i hello [programmanifestet](service-fabric-application-model.md#describe-an-application). hello standardregler för att uppgradera standardtjänster är:

1. Standard tjänster i hello nya [programmanifestet](service-fabric-application-model.md#describe-an-application) som inte finns i hello klustret skapas.
> [!TIP]
> [EnableDefaultServicesUpgrade](service-fabric-cluster-fabric-settings.md#fabric-settings-that-you-can-customize) behov toobe ange tootrue tooenable hello följande regler. Den här funktionen stöds från version 5.5.

2. Standard tjänster finns i båda tidigare [programmanifestet](service-fabric-application-model.md#describe-an-application) och ny version har uppdaterats. Beskrivningar av rolltjänster i den nya versionen av hello skulle skriva över de redan i hello klustret. Uppgradering av programmet skulle återställning automatiskt vid uppdatering standard tjänstfel.
3. Standard tjänster i hello tidigare [programmanifestet](service-fabric-application-model.md#describe-an-application) men inte i den nya versionen av hello tas bort. **Observera att detta tar bort standardtjänster inte kan återställas.**

Om ett program uppgraderingen återställs, är standardtjänster återställts toohello status innan uppgraderingen startas. Men aldrig borttagna tjänster kan skapas.

## <a name="application-upgrade-flowchart"></a>Uppgradera flödesschema för programmet
hello flödesschema följande stycke kan hjälpa dig att förstå hello uppgraderingen av ett Service Fabric-program. I synnerhet hello flödet beskrivs hur hello timeout, inklusive *HealthCheckStableDuration*, *HealthCheckRetryTimeout*, och *UpgradeHealthCheckInterval*, hjälper dig att kontrollera när hello uppgradering i en uppdateringsdomän betraktas som en lyckats eller misslyckats.

![hello uppgraderingsprocessen för ett Service Fabric-program][image]

## <a name="next-steps"></a>Nästa steg
[Uppgradera ditt program med hjälp av Visual Studio](service-fabric-application-upgrade-tutorial.md) vägleder dig genom en uppgradering av programmet med hjälp av Visual Studio.

[Uppgradera ditt program med hjälp av Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vägleder dig genom en uppgradering av programmet med hjälp av PowerShell.

Styra hur programmet ska uppgraderas med hjälp av [uppgradera parametrar](service-fabric-application-upgrade-parameters.md).

Gör din programuppgraderingar kompatibla genom att lära dig hur toouse [dataserialisering](service-fabric-application-upgrade-data-serialization.md).

Lär dig hur toouse avancerade funktioner när du uppgraderar ditt program genom att referera för[avancerade ämnen](service-fabric-application-upgrade-advanced.md).

Lösa vanliga problem i programuppgraderingar genom att referera toohello stegen i [felsökning programuppgraderingar](service-fabric-application-upgrade-troubleshooting.md).

[image]: media/service-fabric-application-upgrade/service-fabric-application-upgrade-flowchart.png
