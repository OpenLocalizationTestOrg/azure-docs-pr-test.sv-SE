---
title: "aaaAzure Behållargrupper instanser behållare"
description: "Förstå hur Behållargrupper fungerar i Azure Container instanser"
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
ms.date: 08/08/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2b0e784609979045c8f77d7b6d0987ec3fee714c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="container-groups-in-azure-container-instances"></a>Behållargrupper i Azure-Behållarinstanser

hello översta resurs i Azure Container instanser är en grupp i behållaren. Den här artikeln beskriver vad behållargrupper är och vilka typer av scenarier som de aktiverar.

## <a name="how-a-container-group-works"></a>Så här fungerar en behållare grupp

En behållare är en samling av behållare som få schemalagda på hello samma värd för dator och dela en livscykel, lokala nätverk och lagringsvolymer. Det är liknande toohello begreppet en *baljor* i [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) och [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).

hello visar följande diagram ett exempel på en behållare grupp som innehåller flera behållare.

![Behållaren grupper exempel][container-groups-example]

Tänk på följande:

- hello grupp schemaläggs på en enda värddator.
- hello grupp Exponerar en offentlig IP-adress, med en exponerade port.
- hello-grupp består av två behållare. En behållare lyssnar på port 80, medan andra hello lyssnar på port 5000.
- hello grupp innehåller två Azure-filresurser som volym monteringar, och varje behållare monterar en hello resurser lokalt.

### <a name="networking"></a>Nätverk

Behållargrupper delar en IP-adress och ett namnområde för port på att IP-adress. tooenable externa klienter tooreach en behållare i hello grupp, måste du exponera hello port på hello IP-adress och från hello behållare. Eftersom behållare i hello grupp delar port namnområde stöds portmappning inte. Behållare i en grupp kan nå varandra via localhost på hello portar som de har exponerade, även om dessa portar inte är tillgängliga externt på hello gruppens IP-adress.

### <a name="storage"></a>Lagring

Du kan ange externa volymer toomount i en grupp i behållaren. Du kan mappa volymerna till specifika sökvägar i hello enskilda behållare i en grupp.

## <a name="common-scenarios"></a>Vanliga scenarier

Flera behållare grupper är användbart i fall där du vill toodivide en enda funktionella uppgift i ett litet antal behållare bilder, som levereras med olika team och ha separata resurskraven. Exempel på användning kan vara:

- En behållare för program och en behållare för loggning. hello loggning behållaren samlar in hello loggar och mått utdata med hello programmets och skriver dem toolong-lagring.
- Ett program och en behållare för övervakning. hello övervakning behållaren regelbundet gör en begäran toohello programmet tooensure att den körs och svara på rätt sätt och aktiverar en avisering om det inte.
- En behållare som betjänar ett webbprogram och en behållare dra hello senaste innehållet från källkontroll.

## <a name="next-steps"></a>Nästa steg

Lär dig hur för[distribuera flera behållargruppen](container-instances-multi-container-group.md) med en Azure Resource Manager-mall.

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png