---
title: "aaaAzure Behållarinstanser och Orchestration-behållare"
description: "Förstå hur Azure Behållarinstanser interagera med behållaren orchestrators"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 69a39edc6f14d885c1ac300990ed1399002ccfee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-instances-and-container-orchestrators"></a>Azure Container instanser och behållare orchestrators

Behållare är väl lämpade för flexibel leverans miljöer och mikrotjänster-baserade arkitekturer på grund av deras liten storlek och programmet orientering. hello uppgiften att automatisera och hantera ett stort antal behållare och hur de samverkar kallas *orchestration*. Populära behållaren orchestrators inkluderar Kubernetes DC/OS och Docker Swarm som är tillgängliga i hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).

Behållarinstanser som Azure tillhandahåller en del av hello grundläggande funktionerna i orchestration-plattformar, men de täcker inte hello högre värde tjänster som dessa plattformar ger och kan i praktiken vara kompletterande med dem. Den här artikeln beskriver hello omfattning Azure Behållarinstanser hanterar och hur fullständig behållaren orchestrators kan interagera med den.

## <a name="traditional-orchestration"></a>Traditionella orchestration

hello standard definition av orchestration omfattar hello följande uppgifter:

- **Schemalägga**: baserat på en avbildning av behållare och en resursbegäran om, hitta en lämplig dator på vilken toorun hello-behållare.
- **Tillhörighet/mot-affinity**: ange att en uppsättning behållare ska köras i närheten varandra (för prestanda) eller tillräckligt långt ifrån varandra (för tillgänglighet).
- **Hälsoövervakning**: Titta på för behållaren och automatiskt schemalägga dem.
- **Redundans**: hålla reda på vad som körs på varje dator och schemalägga behållare från datorer som inte fungerar toohealthy noder.
- **Skalning**: Lägg till eller ta bort behållaren instanser toomatch begäran, antingen manuellt eller automatiskt.
- **Nätverk**: Ange ett överlägget nätverk för att koordinera behållare toocommunicate över flera datorer på värden.
- **Identifiering av tjänst**: aktivera behållare toolocate varandra automatiskt även när de flyttar mellan värddatorerna och ändra IP-adresser.
- **Koordineras programuppgraderingar**: hantera uppgraderingar tooavoid programmet stillestånd och aktivera återställning om något går fel.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Orchestration med Azure Container instanser: överlappande tillvägagångssättet

Azure Behållarinstanser som aktiverar en överlappande tillvägagångssättet tooorchestration eftersom alla hello schemaläggning och hanteringsfunktioner krävs toorun en enskild behållare samtidigt orchestrator plattformar toomanage flera behållare uppgifter ovanpå den.

Eftersom alla hello underliggande infrastruktur för Behållarinstanser som hanteras av Azure, behöver inte tooconcern med att hitta en lämplig värddator på vilken toorun en enskild behållare i en orchestrator-plattformen. hello elasticitet för hello moln garanterar att en alltid är tillgänglig. Hello orchestrator kan istället fokusera på hello uppgifter som förenklar hello utvecklingen av flera behållare arkitekturer, inklusive skalning och samordnade uppgraderingar.



## <a name="potential-scenarios"></a>Möjliga scenarier

Orchestrator-integrering med Azure Container instanser är fortfarande framväxande, förutse att några olika miljöer kan du se ett mönster:

### <a name="orchestration-of-container-instances-exclusively"></a>Dirigering av Behållarinstanser exklusivt

Eftersom de komma igång snabbt och debiterar av hello andra, en miljö baserat på enbart Behållarinstanser som Azure erbjuder hello snabbaste sättet tooget igång och toodeal med hög variabel arbetsbelastningar.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Kombinationen av Behållarinstanser och behållare i virtuella datorer

För tidskrävande blir stabil arbetsbelastningar, samordna behållare i ett kluster på dedikerade virtuella datorer vanligtvis billigare än att köra hello samma behållare med Behållarinstanser. Behållarinstanser erbjuder dock en bra lösning för snabbt expanderande och tilldelning av kontrakt din totala kapaciteten toodeal med oväntade eller tillfällig toppar i användning. I stället för att skala ut hello antalet virtuella datorer i klustret och sedan distribuera ytterligare behållare på de datorerna hello orchestrator kan bara schemalägga hello ytterligare behållare med Behållarinstanser och ta bort dem när de är ingen längre behövs.

## <a name="sample-implementation-azure-container-instances-connector-for-kubernetes"></a>Exempel på implementering: Azure Container instanser Connector för Kubernetes

toodemonstrate hur behållaren orchestration plattformar kan integrera med Azure Container instanser, vi har börjat skapa en [exempel connector för Kubernetes][aci-connector-k8s]. 

Hej connector för Kubernetes efterliknar hello [kubelet] [ kubelet-doc] genom att registrera som en nod med obegränsad kapacitet och sändning av hello skapandet av [skida] [ pod-doc] som behållargrupper i Azure Container instanser. 

<!-- ![ACI Connector for Kubernetes][aci-connector-k8s-gif] -->

Kopplingar för andra orchestrators kan byggas som på samma sätt är integrerad med plattformen primitiver toocombine hello power hello orchestrator API med hello hastighet och enkelhet för att hantera behållare i Azure Container instanser.

> [!WARNING]
> Hej ACI connector för Kubernetes är *experiment* och ska inte användas i produktionen.

## <a name="next-steps"></a>Nästa steg

Skapa din första behållare med Azure Container instanser med hello [snabbstartsguiden](container-instances-quickstart.md).

<!-- IMAGES -->
[aci-connector-k8s-gif]: ./media/container-instances-orchestrator-relationship/aci-connector-k8s.gif

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/azure/aci-connector-k8s
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/