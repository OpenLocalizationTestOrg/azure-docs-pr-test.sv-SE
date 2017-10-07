---
title: "aaaPrivate Docker behållare register i Azure | Microsoft Docs"
description: "Introduktion toohello Azure Container Registry och tillhandahåller molnbaserade, hanterade privata Docker-register."
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
ms.openlocfilehash: f6edcf0bf947b7770ee0a4e4a5cfbf4ef8b7392a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooprivate-docker-container-registries"></a>Introduktion tooprivate Docker behållare register


Azure-behållare är en hanterad [Docker registret](https://docs.docker.com/registry/) tjänsten baserat på hello öppen källkod Docker registret 2.0. Skapa och underhålla Azure-behållaren register toostore och hantera dina privata [dockerbehållare](https://www.docker.com/what-docker) bilder. Använda behållaren register i Azure med ditt befintliga behållaren utveckling och distribution pipelines och rita på hello brödtext Docker community kunskaper.

Bakgrundsinformation om Docker och behållare finns här:

* [Användarhandbok för Docker](https://docs.docker.com/engine/userguide/)




## <a name="use-cases"></a>Användningsfall
Hämta bilder från en Azure-behållare registret toovarious distribution mål:

* **Skalbart dirigeringssystem** som hanterar program i behållare över kluster med värdar, inklusive [DC/OS](https://docs.mesosphere.com/), [Docker Swarm](https://docs.docker.com/swarm/) och [Kubernetes](http://kubernetes.io/docs/).
* **Azure-tjänster** som stöder utveckling och körning av program i stor skala, bland annat [Container Service](../container-service/index.yml), [App Service](/app-service/index.md), [Batch](../batch/index.md) och [Service Fabric](/azure/service-fabric/).

Utvecklare kan också skicka tooa behållare registret som en del av ett arbetsflöde för utveckling av behållare. Du kan till exempel arbeta mot ett behållarregister från ett verktyg för löpande integrering och distribution som [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) eller [Jenkins](https://jenkins.io/).





## <a name="key-concepts"></a>Viktiga begrepp
* **Register** – Skapa en eller flera behållarregister i din Azure-prenumeration. Varje registret backas upp av en standard Azure [lagringskonto](../storage/common/storage-introduction.md) i hello samma plats. Dra nytta av lokal, network-sista lagring på bilderna behållare genom att skapa ett register i hello samma Azure-plats som din distribution. Ett fullständigt kvalificerat registret namn har hello formuläret `myregistry.azurecr.io`.

  Du [åtkomstkontroll](container-registry-authentication.md) tooa behållare registret med hjälp av en Azure Active Directory-uppbackade [tjänstens huvudnamn](../active-directory/active-directory-application-objects.md) eller angivna administratörskonto. Kör hello standard `docker login` kommandot tooauthenticate med ett register.

* **Hanterat register** – En nivå som erbjuder ytterligare funktioner för register i tre SKU:er: Basic, Standard och Premium. hello-avbildningar i dessa SKU: er lagras i Storage-konton som hanteras av hello Azure Container register-tjänsten, som förbättrar tillförlitligheten och aktiverar nya funktioner. Nya funktioner innefattar webhook-integrering, databasautentisering med Azure Active Directory och stöd för borttagning. Användare har hello alternativet toochoose mellan hanterade register eller toocreate ett register som backas upp av sina egna Storage-konton när du skapar register.

* **Lagringsplats** – Ett register innehåller en eller flera databaser, som är grupper med behållaravbildningar. Azure Container Registry har stöd för namnområden för lagringsplatser på flera nivåer. Den här funktionen kan du toogroup samlingar med bilder relaterade tooa viss app eller en uppsättning appar toospecific utveckling eller operativa team. Exempel:

  * `myregistry.azurecr.io/aspnetcore:1.0.1` representerar en företagsomfattande avbildning
  * `myregistry.azurecr.io/warrantydept/dotnet-build`representerar en bild används toobuild .NET apps, delas mellan hello garanti avdelning
  * `myregistry.azrecr.io/warrantydept/customersubmissions/web`representerar en avbildning av webbserver, grupperade i hello kunden bidrag app som ägs av hello garanti avdelning

* **Avbildning** – Lagras på en lagringsplats. Varje avbildning är en skrivskyddad ögonblicksbild av en Docker-behållare. Azure-behållarregister kan innehålla både Windows- och Linux-avbildningar. Du styr avbildningsnamnen för alla behållardistributioner. Använd standard [Docker kommandon](https://docs.docker.com/engine/reference/commandline/) toopush bilder i databasen eller dra en bild från en databas.

* **Behållare** – Behållare definierar programvara och dess beroenden och är inneslutna i ett komplett filsystem, inklusive kod, runtime, systemverktyg och bibliotek. Kör Docker-behållare baserat på Windows- eller Linux-avbildningar som du hämtar från ett behållarregister. Behållare som körs på en dator delar hello operativsystemets kärna. Docker-behållare är fullständigt anpassade tooall större distributioner för Linux, Mac och Windows.




## <a name="next-steps"></a>Nästa steg
* [Skapa en behållare registret med hjälp av hello Azure-portalen](container-registry-get-started-portal.md)
* [Skapa en behållare registret med hjälp av hello Azure CLI](container-registry-get-started-azure-cli.md)
* [Push-en avbildning med hjälp av hello Docker CLI](container-registry-get-started-docker-cli.md)
* toobuild en kontinuerlig integration och arbetsflöde för distribution med hjälp av Visual Studio Team Services, Azure Container Service och Azure Container registret finns [självstudierna](../container-service/dcos-swarm/container-service-docker-swarm-setup-ci-cd.md).
* Om du vill tooset egna Docker privata registret i Azure (utan en offentlig slutpunkt), se [distribuera din egen privata Docker registret på Azure](../virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md).
