---
title: "Översikt över Azure Container Instances"
description: "Förstå Azure Container Instances"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 01e539856adbdcf02dc4e49087a3ab71b328db5a
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/05/2018
---
# <a name="azure-container-instances"></a>Azure Container Instances

Behållare är på väg att bli det bästa sättet att paketera, distribuera och hantera molnprogram. Azure Container Instances erbjuder det snabbaste och enklaste sättet att köra en behållare i Azure utan att man behöver etablera några virtuella datorer och utan att använda en tjänst på högre nivå.

Azure Container Instances är en bra lösning för alla scenarier som kan fungera i isolerade behållare, däribland enkla program, automatisering av uppgifter och att skapa jobb. För scenarier där du behöver fullständig behållarsamordning, inklusive tjänstidentifiering för flera behållare, automatisk skalning och koordinerade programuppgraderingar rekommenderar vi [Azure Container Service (AKS)](../aks/index.yml).

## <a name="fast-startup-times"></a>Snabba starttider

Med behållare får du betydande startfördelar jämfört med virtuella datorer. Med Azure Container Instances kan du starta en behållare i Azure på några sekunder utan att behöva etablera och hantera virtuella datorer.

## <a name="hypervisor-level-security"></a>Säkerhet på hypervisornivå

Tidigare har behållare erbjudit isolering av programberoenden och resursstyrning men har inte ansetts vara tillräckligt strikta för fientlig användning med flera innehavare. Med Azure Container Instances är ditt program lika isolerat i en behållare som i en virtuell dator.

## <a name="custom-sizes"></a>Anpassade storlekar

Behållare är vanligtvis optimerade för att endast köra ett program, men de specifika behoven för dessa program kan skilja sig åt avsevärt. Med Azure Container Instances kan du begära exakt det du behöver när det gäller processorkärnor och minne. Du betalar baserat på vad du begär och faktureras per sekund, så att du kan optimera dina utgifter noggrant utifrån dina behov.

## <a name="public-ip-connectivity"></a>Offentlig IP-anslutning

Med Azure Container Instances kan du exponera dina behållare direkt för internet med en offentlig IP-adress. I framtiden kommer vi att utöka våra nätverksfunktioner för att inkludera integration med virtuella nätverk, belastningsutjämnare och andra viktiga delar av nätverksinfrastrukturen i Azure.

## <a name="persistent-storage"></a>Beständig lagring

Vi erbjuder direkt [montering av Azure Files-resurser](container-instances-mounting-azure-files-volume.md) för att hämta och bevara tillstånd med Azure Container Instances.

## <a name="linux-and-windows-containers"></a>Linux- och Windows-behållare

Med Azure Container Instances kan du schemalägga både Windows- och Linux-behållare med samma API. Ange typ av operativsystem när du skapar dina [behållargrupper](container-instances-container-groups.md).

Vissa funktioner är för närvarande begränsade till Linux-behållare. Under tiden som vi arbetar för att göra alla funktioner tillgängliga för Windows-behållare kan du se de nuvarande skillnaderna mellan plattformarna i informationen om [kvoter och regional tillgänglighet för Azure Container Instances](container-instances-quotas.md).

## <a name="co-scheduled-groups"></a>Samordna schemalagda grupper

Azure Container Instances stöder schemaläggning av [grupper med flera behållare](container-instances-container-groups.md) som delar en värddator, lokalt nätverk, lagring och livscykel. Det gör att du kan kombinera ditt huvudprogram med andra som fungerar som en stödjande roll som loggning.

## <a name="next-steps"></a>Nästa steg

Försök att distribuera en behållare till Azure med ett enda kommando med hjälp av vår [snabbstartsguide](container-instances-quickstart.md).