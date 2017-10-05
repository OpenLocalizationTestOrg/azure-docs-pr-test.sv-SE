---
title: "Privata Docker-behållarregister i Azure | Microsoft Docs"
description: "Introduktion till Azure Container Registry-tjänsten, som tillhandahåller molnbaserade, hanterade, privata Docker-register."
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: ee2b652b-fb7c-455b-8275-b8d4d08ffeb3
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fd0356286be46f99fd9ab8eabc53256103038407
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-private-docker-container-registries"></a>Introduktion till privata Docker-behållarregister


Azure Container Registry är en hanterad [Docker-registertjänst](https://docs.docker.com/registry/) som baseras på Docker Registry 2.0 (öppen källkod). Skapa och underhåll Azure-behållarregister för att lagra och hantera dina privata avbildningar av [Docker-behållare](https://www.docker.com/what-docker). Använd behållarregister i Azure med dina befintliga behållarutvecklings- och distributionspipelines och dra nytta av andras kunskaper i Docker-communityn.

Bakgrundsinformation om Docker och behållare finns här:

* [Användarhandbok för Docker](https://docs.docker.com/engine/userguide/)




## <a name="use-cases"></a>Användningsfall
Hämta avbildningar från ett Azure-behållarregister till olika distributionsmål:

* **Skalbart dirigeringssystem** som hanterar program i behållare över kluster med värdar, inklusive [DC/OS](https://docs.mesosphere.com/), [Docker Swarm](https://docs.docker.com/swarm/) och [Kubernetes](http://kubernetes.io/docs/).
* **Azure-tjänster** som stöder utveckling och körning av program i stor skala, bland annat [Container Service](../container-service/index.yml), [App Service](/app-service/index.md), [Batch](../batch/index.md) och [Service Fabric](/azure/service-fabric/).

Utvecklare kan även skicka till ett behållarregister som en del av ett arbetsflöde för utveckling av behållare. Du kan till exempel arbeta mot ett behållarregister från ett verktyg för löpande integrering och distribution som [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) eller [Jenkins](https://jenkins.io/).





## <a name="key-concepts"></a>Viktiga begrepp
* **Register** – Skapa en eller flera behållarregister i din Azure-prenumeration. Varje register backas upp av ett [Azure-standardlagringskonto](../storage/common/storage-introduction.md) på samma plats. Dra nytta av lokal, nätverksnära lagring av dina behållaravbildningar genom att skapa ett register på samma Azure-plats som dina distributioner. Ett fullständigt kvalificerat registernamn har formatet `myregistry.azurecr.io`.

  Du [styr åtkomsten](container-registry-authentication.md) till en behållare med hjälp av ett Azure Active Directory-kopplat [tjänstobjekt](../active-directory/active-directory-application-objects.md) eller ett angivet administratörskonto. Kör `docker login`-standardkommandot för att autentisera med ett register.

* **Hanterat register** – En nivå som erbjuder ytterligare funktioner för register i tre SKU:er: Basic, Standard och Premium. Avbildningarna i dessa SKU: er lagras i lagringskonton som hanteras av tjänsten Azure Container Registry, vilket förbättrar tillförlitligheten och aktiverar nya funktioner. Nya funktioner innefattar webhook-integrering, databasautentisering med Azure Active Directory och stöd för borttagning. Användarna kan välja mellan hanterade register eller skapa ett register som backas upp av egna lagringskonton när de skapar register.

* **Lagringsplats** – Ett register innehåller en eller flera databaser, som är grupper med behållaravbildningar. Azure Container Registry har stöd för namnområden för lagringsplatser på flera nivåer. Den här funktionen gör att du kan gruppera samlingar med avbildningar relaterade till en viss app, eller en samling appar för specifika utvecklingsgrupper eller operativa team. Exempel:

  * `myregistry.azurecr.io/aspnetcore:1.0.1` representerar en företagsomfattande avbildning
  * `myregistry.azurecr.io/warrantydept/dotnet-build` representerar en avbildning som används för att skapa .NET-appar, som delas på garantiavdelningen.
  * `myregistry.azrecr.io/warrantydept/customersubmissions/web` representerar en webbavbildning, grupperad i appen för kundöverföringar, som ägs av garantiavdelningen.

* **Avbildning** – Lagras på en lagringsplats. Varje avbildning är en skrivskyddad ögonblicksbild av en Docker-behållare. Azure-behållarregister kan innehålla både Windows- och Linux-avbildningar. Du styr avbildningsnamnen för alla behållardistributioner. Använd [Docker-standardkommandon](https://docs.docker.com/engine/reference/commandline/) för att skicka avbildningar till en lagringsplats, eller för att hämta en avbildning från en lagringsplats.

* **Behållare** – Behållare definierar programvara och dess beroenden och är inneslutna i ett komplett filsystem, inklusive kod, runtime, systemverktyg och bibliotek. Kör Docker-behållare baserat på Windows- eller Linux-avbildningar som du hämtar från ett behållarregister. Behållare som körs på en enskild dator delar operativsystemets kernel. Docker-behållare är helt portabla till alla större Linux-distributioner, Mac och Windows.




## <a name="next-steps"></a>Nästa steg
* [Skapa ett behållarregister med hjälp av Azure Portal](container-registry-get-started-portal.md)
* [Skapa ett behållarregister med hjälp av Azure CLI](container-registry-get-started-azure-cli.md)
* [Skicka din första avbildning med hjälp av Docker CLI](container-registry-get-started-docker-cli.md)
* Om du vill skapa ett kontinuerligt arbetsflöde för integrering och distribution med Visual Studio Team Services, Azure Container Service och Azure Container Registry kan du kika på den här [självstudiekursen](../container-service/dcos-swarm/container-service-docker-swarm-setup-ci-cd.md).
* Information om hur du ställer in ett privat Docker-register i Azure (utan en offentlig slutpunkt) finns i [Distribuera ditt egna privata Docker-register på Azure](../virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md).
