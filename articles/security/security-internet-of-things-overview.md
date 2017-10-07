---
title: aaaSecure din Sakernas Internet (IoT) i Azure | Microsoft Docs
description: " Azure internet av saker (IoT) services erbjuder en mängd funktioner. Den här artikeln hjälper dig att förstå hur toosecure IoT-lösningar i Azure. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 1473c8dd-8669-48fb-86db-b3c50e2eaf59
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: b6cb2ea1c1facada854fb52c55066f34a8289e47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-overview"></a>Översikt över säkerheten i Sakernas Internet
Azure internet av saker (IoT) services erbjuder en mängd funktioner. Med dessa tjänster i företagsklass kan du:

* Samla in data från enheter
* Analysera dataströmmar i rörelse
* Lagra och skicka frågor mot stora datamängder
* Visualisera både realtidsdata och historiska data
* Integrera med back office-system

Dessa funktioner, Azure IoT Suite paket tillsammans toodeliver flera Azure-tjänster med anpassade tillägg som förkonfigurerade lösningar. Dessa förkonfigurerade lösningar är grundläggande implementeringar av vanliga IoT-lösningen mönster som hjälper tooreduce hello tid som går åt toodeliver IoT-lösningar. Med hello IoT software development Kit kan du anpassa och utöka dessa lösningar toomeet dina egna behov. Du kan också använda dessa lösningar som exempel eller mallar när du utvecklar nya IoT-lösningar.

hello Azure IoT suite är en kraftfull lösning för din IoT-behov. Det är dock upmost viktigt att IoT-lösningar är utformade med säkerhet i åtanke från hello start. På grund av hello finns så många IoT-enheter, kan en säkerhetsincident snabbt bli en omfattande händelse med betydande konsekvenser.

toohelp du förstår hur toosecure IoT-lösningar har vi hello följande information.

## <a name="security-architecture"></a>Säkerhetsarkitektur
När du designar ett system, är viktiga toounderstand hello potentiella hot toothat system och Lägg till lämpliga försvar därför hello system är utformad och konstruerad. Det är viktigt toodesign hello produkten från hello start med säkerhet i åtanke eftersom förstå hur en angripare kan vara kan toocompromise ett system som hjälper dig att se till att lämpliga åtgärder är på plats från hello början.

Du kan lära dig om IoT-säkerhetsarkitekturen genom att läsa [Internet av saker säkerhetsarkitekturen](../iot-suite/iot-security-architecture.md).

Den här artikeln beskrivs hello följande avsnitt:

* [Säkerhet börjar med en Hotmodell](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
* [Säkerhet i IoT](../iot-suite/iot-security-architecture.md#security-in-iot)
* [Hot modellering hello Azure IoT-Referensarkitektur](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-hello-ground-up"></a>Säkerhet från hello bakgrund
Hej IoT utgör unika säkerhet, sekretess och kompatibilitet utmaningar toobusinesses över hela världen. Till skillnad från traditionella cyber teknik där problemen omfångsfasen handlar om programvara och hur den har implementerats gäller IoT vad som händer när hello cyber och hello fysiska världar Konvergera. Skydda IoT-lösningar kräver att säkerställa säker etablering av enheter, säker anslutning mellan dessa enheter och hello molnet och säkert dataskydd i hello molnet under bearbetning och lagring. Arbeta mot dessa funktioner är dock begränsad resurs enheter, geografisk fördelning av distributioner och många enheter i en lösning.

Du kan lära dig hur toohandle säkerhet i dessa områden genom att läsa [Sakernas Internet security från hello bakgrund](../iot-suite/securing-iot-ground-up.md).

hello artikeln diskuteras hello följande avsnitt:

* [Säker infrastruktur från hello bakgrund](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
* [Microsoft Azure – säker IoT-infrastruktur för ditt företag](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>Metodtips
Skydda en IoT-infrastruktur kräver en rigorösa security-strategi. Från att skydda data i hello moln, skydda dataintegriteten medan under överföring via hello bygger offentliga internet toosecurely etablering enheter, varje lager större säkerhet säkerhet hello hela infrastrukturen.

Lär du dig i Sakernas Internet security bästa praxis genom att läsa [Sakernas Internet säkerhetsmetoder](../iot-suite/iot-security-best-practices.md).

hello artikeln diskuteras hello följande avsnitt:

* [IoT maskinvara tillverkare/integrator](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
* [IoT-lösningen utvecklare](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
* [Distribueraren för IoT-lösning](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
* [IoT-lösningen operator](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
