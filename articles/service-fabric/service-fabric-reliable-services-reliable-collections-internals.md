---
title: "aaaAzure Service Fabric tillförlitliga tillstånd Manager och tillförlitlig samling internals | Microsoft Docs"
description: "Ingående om tillförlitliga samling begrepp och design i Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: 651bfb52785a2475e4840cd471e87220d1936391
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-reliable-state-manager-and-reliable-collection-internals"></a>Azure Service Fabric tillförlitlig Tillståndshanterare och tillförlitlig samling internals
Det här dokumentet går på djupet i tillförlitliga Tillståndshanterare och tillförlitlig samlingar toosee hur kärnkomponenter fungerar hello bakgrunden.

> [!NOTE]
> Det här dokumentet är arbetet pågår. Lägga till kommentarer toothis artikel tootell oss vilka avsnitt som du vill att toolearn mer om.
>

##  <a name="local-persistence-model-log-and-checkpoint"></a>Lokala beständiga modell: loggen och kontrollpunkt
hello tillförlitliga Tillståndshanterare och tillförlitlig samlingar följer en beständiga modell som kallas loggen och kontrollpunkt.
I den här modellen är varje tillståndsändring inloggad disk först och sedan tillämpas i minnet.
hello fullständig tillstånd själva sparas bara ibland (kallas även Kontrollpunkten).
hello fördelen är att går blev sekventiella Lägg endast skrivåtgärder på disken för bättre prestanda.

toobetter förstå hello loggen och kontrollpunkt modell, låt oss först titta på hello oändlig disk scenario.
hello tillförlitliga Tillståndshanterare loggar varje åtgärd innan de replikeras.
Loggning kan hello tillförlitliga samlingar tooapply hello åtgärden endast i minnet.
Eftersom loggar sparas även när hello replik misslyckas och måste startas om toobe har hello tillförlitliga Tillståndshanterare tillräckligt med information i dess loggen tooreplay alla hello åtgärder hello replik har förlorat.
Hello disk är oändlig loggposter behöver aldrig toobe tas bort och hello tillförlitliga samlingen måste toomanage endast hello InMemory-läge.

Nu ska vi titta på hello ändligt disk scenario.
Eftersom loggposter ackumuleras körs hello tillförlitliga Tillståndshanterare slut på diskutrymme.
Innan i så fall måste hello tillförlitliga Tillståndshanterare tootruncate dess toomake utrymme i loggen för hello senare poster.
Tillstånd för tillförlitlig Manager begäranden hello tillförlitliga samlingar toocheckpoint toodisk sina InMemory-läge.
Nu skulle hello tillförlitliga samlingar' sparas tillståndet i minnet.
När hello tillförlitliga samlingar har slutfört sin kontrollpunkter trunkera hello tillförlitliga Tillståndshanterare hello loggen toofree utrymme på disken.
När hello replik måste toobe startas om, tillförlitlig samlingar återställa sina kontrollpunkt tillstånd och hello tillförlitliga Tillståndshanterare återställer och spelar upp alla hello tillståndsändringar som gjorts sedan hello senaste kontrollpunkten.

Ett annat värde lägga till kontrollpunkter är det förbättrar återställningstiden i vanliga scenarier. Loggen innehåller alla åtgärder som inträffat sedan hello senaste kontrollpunkten.
Så kan den innehålla flera versioner av ett objekt som flera värden för en viss rad i tillförlitliga ordlistan.
En tillförlitlig samling kontrollpunkter hello däremot bara senaste versionen av varje värde för en nyckel.

## <a name="next-steps"></a>Nästa steg
* [Transaktioner och lås](service-fabric-reliable-services-reliable-collections-transactions-locks.md)

