---
title: "Azure-behållaren instanser Behållargrupper"
description: "Förstå hur Behållargrupper fungerar i Azure Container instanser"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 12/19/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: a42c01917926a4297c97cf9c5dfd1333dbef6793
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/05/2018
---
# <a name="container-groups-in-azure-container-instances"></a>Behållargrupper i Azure Container instanser

Resursen på den översta nivån i Azure Container instanser är den *behållargruppen*. Den här artikeln beskriver behållargrupper är och vilka typer av scenarier som de aktiverar.

## <a name="how-a-container-group-works"></a>Så här fungerar en behållare grupp

En behållare grupp är en samling av behållare som få schemalagda på samma värddator. Behållare i en behållare grupp dela en livscykel, lokala nätverk och lagringsvolymer. Den liknar konceptet för en *baljor* i [Kubernetes] [ kubernetes-pod] och [DC/OS][dcos-pod].

Följande diagram visar ett exempel på en behållare grupp som innehåller flera behållare.

![Behållaren grupper diagram][container-groups-example]

Det här exemplet behållaren grupp:

* Schemaläggs på en enda värddator.
* Visar en offentlig IP-adress, med en exponerade port.
* Består av två behållare. En behållare lyssnar på port 80, medan andra lyssnar på port 5000.
* Innehåller två Azure filresurser som volym monteringar och varje behållare monterar en resurs lokalt.

> [!NOTE]
> Flera behållare grupper är för närvarande begränsad till Linux-behållare. När vi arbetar för att göra alla funktioner till Windows-behållare, hittar du den aktuella plattformen skillnader i [kvoter och regional tillgänglighet för Azure-Behållarinstanser](container-instances-quotas.md).

### <a name="networking"></a>Nätverk

Behållargrupper delar en IP-adress och ett namnområde för port på att IP-adress. Om du vill aktivera externa klienter ska nå en behållare i gruppen, måste du exponera port på IP-adressen och från behållaren. Eftersom behållare i gruppen delar port namnområde stöds portmappning inte. Behållare i en grupp kan du nå en varandra via localhost på de portar som de har exponerade, även om dessa portar inte är tillgängliga externt på gruppens IP-adress.

### <a name="storage"></a>Lagring

Du kan ange externa volymer att montera i en grupp i behållaren. Du kan mappa volymerna till specifika sökvägar i enskilda behållare i en grupp.

## <a name="common-scenarios"></a>Vanliga scenarier

Flera behållare grupper är användbart i fall där du vill dela upp en funktionell aktivitet i ett litet antal behållare bilder, som levereras med olika team och ha separata resurskraven.

Exempel på användning kan vara:

* En behållare för program och en behållare för loggning. Behållaren loggning samlar in loggar och mått utdata av de huvudsakliga programmet och skriver dem till långsiktig lagring.
* Ett program och en behållare för övervakning. Behållaren övervakning skickar med jämna mellanrum en begäran till programmet så att den körs och svara på rätt sätt och aktiverar en avisering om det inte.
* En behållare som betjänar ett webbprogram och en behållare dra det senaste innehållet från källkontroll.

## <a name="next-steps"></a>Nästa steg

Lär dig hur du [distribuera flera behållargruppen](container-instances-multi-container-group.md) med en Azure Resource Manager-mall.

<!-- IMAGES -->
[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png

<!-- LINKS - External -->
[dcos-pod]: https://dcos.io/docs/1.10/deploying-services/pods/
[kubernetes-pod]: https://kubernetes.io/docs/concepts/workloads/pods/pod/
