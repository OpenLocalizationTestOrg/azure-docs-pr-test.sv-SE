---
title: "Azure IoT-hubb hög tillgänglighet och disaster recovery | Microsoft Docs"
description: "Beskriver funktionerna i Azure och IoT-hubb som hjälper dig att skapa hög tillgänglighet Azure IoT-lösningar med katastrofåterställning återställningsfunktioner."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: ae320e58-aa20-45b9-abdc-fa4faae8e6dd
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: elioda
ms.openlocfilehash: 76c3187549e1821908263c30e394db26ee6f75e6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT-hubb hög tillgänglighet och disaster recovery
IoT-hubb ger hög tillgänglighet (HA) med hjälp av uppsägningar på nivån Azure-region utan ytterligare krävs för lösningen som en Azure-tjänst. Microsoft Azure-plattformen innehåller också funktioner för att skapa lösningar med funktioner för katastrofåterställning (DR) eller mellan regional tillgänglighet. Om du vill tillhandahålla hög tillgänglighet för global, cross-region för enheter eller användare, utforma och förbereda dina lösningar kan dra nytta av dessa Azure DR-funktioner. Artikeln [Azure Business Continuity teknisk vägledning](../resiliency/resiliency-technical-guidance.md) beskrivs de inbyggda funktionerna i Azure för kontinuitet för företag och Katastrofåterställning. Den [katastrofåterställning och hög tillgänglighet för Azure-program] [ Disaster recovery and high availability for Azure applications] dokumentet innehåller arkitekturvägledning om strategier för Azure-program att uppnå hög tillgänglighet och Katastrofåterställning.

## <a name="azure-iot-hub-dr"></a>Azure IoT-hubb-Katastrofåterställning
Förutom intra-region hög tillgänglighet implementerar IoT-hubb redundans mekanismer för katastrofåterställning som kräver ingen åtgärd från användaren. IoT Hub DR själva initieras och har en återställning tid mål för Återställningstid 2-26 timmar och följande återställningspunktmål (återställningspunkter).

| Funktioner | ÅTERSTÄLLNINGSPUNKTMÅL |
| --- | --- |
| Tjänsttillgänglighet för registret och kommunikation |Förlust av CName |
| Identitetsuppgifter i identitetsregistret |0-5 minuter dataförlust |
| Meddelanden från enheten till molnet |Alla oläst meddelanden försvinner |
| Åtgärder som övervakning av meddelanden |Alla oläst meddelanden försvinner |
| Meddelanden moln till enhet |0-5 minuter dataförlust |
| Feedbackkö moln till enhet |Alla oläst meddelanden försvinner |

## <a name="regional-failover-with-iot-hub"></a>Regional växling vid fel med IoT-hubb
En fullständig behandling av distributionstopologier i IoT-lösningar som ligger utanför omfånget för den här artikeln. Här beskrivs de *regional växling vid fel* distributionsmodell för hög tillgänglighet och katastrofåterställning återställning.

I en modell för regional växling vid fel distribueras lösning tillbaka slutet körs främst i en datacenter-plats och en sekundär IoT-hubb och tillbaka slutet på en annan plats för datacenter. Om IoT-hubb i det primära datacentret drabbas av ett avbrott eller nätverksanslutningen från enheten till det primära datacentret avbryts. Enheterna använder en sekundär tjänstslutpunkt när den primära gatewayen inte kan nås. Med en mellan region redundanskapacitet kan lösning tillgängligheten förbättras utöver den höga tillgängligheten för en enskild region.

På en hög nivå om du vill implementera en modell för regional växling vid fel med IoT-hubben, behöver du följande:

* **En sekundär IoT-hubb och enheten routning logik**: vid ett avbrott i tjänsten i din primära region måste enheter måste börja ansluter till din sekundära regionen. Eftersom tillståndsmedveten de flesta tjänster ingår, är det vanligt att lösningen administratörer som kan utlösa mellan region failover-processen. Det bästa sättet att kommunicera ny slutpunkt till enheter, samtidigt som kontrollen över processen är att be dem regelbundet kontrollera en *concierge* tjänsten för den aktuella aktiva slutpunkten. Tjänsten concierge kan vara ett webbprogram som ska replikeras och sparas kan nås med hjälp av DNS-omdirigering tekniker (till exempel med hjälp av [Azure Traffic Manager][Azure Traffic Manager]).
* **Identitet Registerreplikering** – du kan användas, sekundära IoT-hubben måste innehålla alla enheten identiteter som kan ansluta till lösningen. Lösningen ska hålla georeplikerad säkerhetskopior av enheten identiteter och överför dem till sekundära IoT-hubben innan du byter aktiv slutpunkt för enheter. Exportfunktionen enheten identitet för IoT-hubben är användbart i detta sammanhang. Mer information finns i [IoT-hubb Utvecklarhandbok - identitetsregistret][IoT Hub developer guide - identity registry].
* **Sammanslagning av logik** – när den primära regionen blir tillgänglig igen alla tillstånd och data som har skapats på den sekundära platsen måste flyttas tillbaka till den primära regionen. Detta tillstånd och data avser främst enheten identiteter och programmetadata måste slås samman med den primära IoT-hubben och andra programspecifika data lagras i den primära regionen. Du bör använda idempotent åtgärder för att förenkla det här steget. Idempotent operations minimera sidoeffekter eventuell konsekvent distribution av händelser och från dubbletter eller out-ordning leverans av händelser. Dessutom designas programlogiken klarar potentiella inkonsekvenser eller ”något” utanför datum-tillstånd. Den här situationen kan inträffa på grund av den extra tid det tar för systemet kan ”reparera” baserat på återställningspunktmål (RPO).

## <a name="next-steps"></a>Nästa steg
Du kan följa dessa länkar om du vill veta mer om Azure IoT-hubb:

* [Kom igång med IoT-hubbar (självstudier)][lnk-get-started]
* [Vad är Azure IoT Hub?][What is Azure IoT Hub?]

[Disaster recovery and high availability for Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub developer guide - identity registry]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[What is Azure IoT Hub?]: iot-hub-what-is-iot-hub.md
