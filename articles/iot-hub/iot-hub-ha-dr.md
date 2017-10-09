---
title: "aaaAzure IoT-hubb hög tillgänglighet och disaster recovery | Microsoft Docs"
description: "Beskriver hello Azure- och IoT-hubb funktioner som hjälper dig toobuild högtillgänglig Azure IoT-lösningar med funktioner för katastrofåterställning."
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
ms.openlocfilehash: 74e1fa1682258819ffb3fd84eb79e42fc6e4120f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT-hubb hög tillgänglighet och disaster recovery
IoT-hubb ger hög tillgänglighet (HA) med hjälp av uppsägningar nivån hello Azure-region, utan ytterligare krävs av hello-lösning som en Azure-tjänst. hello Microsoft Azure-plattformen innehåller också funktioner toohelp du skapa lösningar med funktioner för katastrofåterställning (DR) eller mellan regional tillgänglighet. Om du vill tooprovide globala mellan region hög tillgänglighet för enheter eller användare, utforma och förbereda dina lösningar tootake använda dessa Azure DR-funktioner. hello artikel [Azure Business Continuity teknisk vägledning](../resiliency/resiliency-technical-guidance.md) beskriver hello inbyggda funktioner i Azure för kontinuitet för företag och Katastrofåterställning. Hej [katastrofåterställning och hög tillgänglighet för Azure-program] [ Disaster recovery and high availability for Azure applications] rapporten innehåller arkitekturvägledning på strategier för Azure-program tooachieve hög tillgänglighet och Katastrofåterställning.

## <a name="azure-iot-hub-dr"></a>Azure IoT-hubb-Katastrofåterställning
Dessutom toointra region HA IoT-hubb implementerar redundans mekanismer för katastrofåterställning som kräver ingen åtgärd från användaren hello. IoT Hub DR själva initieras och har en återställning tid mål för Återställningstid 2-26 timmar och hello efter återställningspunktmål (återställningspunkter).

| Funktioner | ÅTERSTÄLLNINGSPUNKTMÅL |
| --- | --- |
| Tjänsttillgänglighet för registret och kommunikation |Förlust av CName |
| Identitetsuppgifter i identitetsregistret |0-5 minuter dataförlust |
| Meddelanden från enheten till molnet |Alla oläst meddelanden försvinner |
| Åtgärder som övervakning av meddelanden |Alla oläst meddelanden försvinner |
| Meddelanden moln till enhet |0-5 minuter dataförlust |
| Feedbackkö moln till enhet |Alla oläst meddelanden försvinner |

## <a name="regional-failover-with-iot-hub"></a>Regional växling vid fel med IoT-hubb
En fullständig behandling av distributionstopologier i IoT-lösningar är utanför hello omfånget för den här artikeln. hello artikeln hello *regional växling vid fel* distributionsmodell för hello syftet med hög tillgänglighet och katastrofåterställning återställning.

Hello lösning tillbaka slutet körs i första hand i en datacenter-plats i en modell för regional växling vid fel, och en sekundär IoT-hubb och serverdel distribueras på en annan plats för datacenter. Om hello IoT-hubb i hello primära datacenter drabbas av ett avbrott eller hello nätverksanslutningen från hello enheten toohello primära datacenter avbryts. Enheterna använder en sekundär tjänstslutpunkt när hello primära gateway inte kan nås. Med en mellan region redundanskapacitet kan hello lösning tillgänglighet förbättras utöver hello hög tillgänglighet för en enskild region.

På en hög nivå tooimplement en modell för regional växling vid fel med IoT-hubb måste hello följande:

* **En sekundär IoT-hubb och enheten routning logik**: hello gäller störningar i din primära region, enheter måste börja anslutande tooyour sekundär region. Eftersom hello tillståndsmedveten de flesta tjänster ingår, är det vanligt att administratörer tootrigger hello mellan region redundans lösningsprocessen. hello bästa sätt toocommunicate hello ny slutpunkt toodevices, men behålla kontrollen över hello processen är toohave dem regelbundet kontrollera en *concierge* tjänsten för hello aktuella aktiv slutpunkt. hello concierge-tjänsten kan vara ett webbprogram som ska replikeras och sparas kan nås med hjälp av DNS-omdirigering tekniker (till exempel med hjälp av [Azure Traffic Manager][Azure Traffic Manager]).
* **Identitet Registerreplikering** -toobe som är användbara hello sekundära IoT-hubb måste innehålla alla enheten identiteter som kan ansluta toohello lösning. hello lösningen behåller georeplikerad säkerhetskopior av enheten identiteter, och överföra toohello sekundära IoT-hubb innan du byter hello aktiv slutpunkt för hello-enheter. hello enheten identitet exportfunktionen för IoT-hubben är användbart i detta sammanhang. Mer information finns i [IoT-hubb Utvecklarhandbok - identitetsregistret][IoT Hub developer guide - identity registry].
* **Sammanslagning av logik** – när hello primär region blir tillgänglig igen, alla hello tillstånd och data som har skapats i hello sekundär plats måste vara migrerade tillbaka toohello primär region. Detta tillstånd och data handlar om främst toodevice identiteter och programmetadata måste slås samman med hello primära IoT-hubb och andra programspecifika butiker i hello primär region. toosimplify det här steget ska du använda idempotent åtgärder. Idempotent operations minimera hello sidoeffekter hello eventuell konsekvent distribution av händelser och från dubbletter eller out-ordning leverans av händelser. Hello programlogik ska dessutom vara utformad tootolerate eventuella inkonsekvenser eller ”något” utanför datum-tillstånd. Den här situationen kan inträffa på grund av toohello extra tid det tar för hello system för ”läka” baserat på återställningspunktmål (RPO).

## <a name="next-steps"></a>Nästa steg
Följ dessa länkar toolearn mer om Azure IoT-hubb:

* [Kom igång med IoT-hubbar (självstudier)][lnk-get-started]
* [Vad är Azure IoT Hub?][What is Azure IoT Hub?]

[Disaster recovery and high availability for Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub developer guide - identity registry]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[What is Azure IoT Hub?]: iot-hub-what-is-iot-hub.md
