---
title: "Nästa steg för att skapa aaaService Fabric projektet | Microsoft Docs"
description: "Den här artikeln innehåller länkar tooa development uppgifter för Service Fabric"
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: rwike77
ms.openlocfilehash: 45598bfabedf280fba8af449ef920f40b409a609
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="your-service-fabric-application-and-next-steps"></a>Service Fabric-programmet och nästa steg
Azure Service Fabric-programmet har skapats. Den här artikeln beskriver hello makeup för ditt projekt och vissa potentiella nästa steg.

## <a name="your-application"></a>Ditt program
Alla nya program innehåller ett projekt för programmet. Det kan finnas en eller två ytterligare projekt, beroende på hello typ av tjänst som är valt.

### <a name="hello-application-project"></a>hello application-projekt
hello-programprojekt består av:

* En uppsättning referenser toohello tjänster som utgör ditt program.
* Tre publicera profiler (1-nod lokala 5-nod lokala och moln) som du kan använda toomaintain inställningar för att arbeta med olika miljöer, till exempel inställningar relaterade tooa klusterslutpunkten och om tooperform uppgradera distributioner som standard.
* Tre parametern programfiler (samma som ovan) som du kan använda toomaintain miljö-specifika programkonfigurationer, till exempel hello antal partitioner toocreate för en tjänst.
* Ett skript för distribution som du kan använda toodeploy programmet hello kommandoraden eller som en del av en automatisk kontinuerlig integrering och distribution pipeline.
* hello programmanifestet som beskriver hello program. Du hittar hello manifestet hello ApplicationPackageRoot i mappen.

### <a name="stateless-service"></a>Tillståndslösa tjänsten
När du lägger till en ny tjänst för tillståndslösa Visual Studio lägger till ett projekt tooyour lösning som innehåller en typ som underordnade `StatelessService`. hello service delar en lokal variabel i en räknare.

### <a name="stateful-service"></a>Tillståndskänslig service
När du lägger till en ny tjänst för tillståndskänsliga Visual Studio lägger till ett projekt tooyour lösning som innehåller en typ som underordnade `StatefulService`. hello service ökar en räknare i dess `RunAsync` metoden och lagrar hello resultera i en `ReliableDictionary`.

### <a name="actor-service"></a>Aktören service
När du lägger till en ny tillförlitliga aktören Visual Studio lägger till två projekt tooyour lösning: en aktören projektet och ett gränssnitt.

hello aktören projektet tillhandahåller metoder för inställningen och hämta hello värdet på en räknare som sparas på ett tillförlitligt sätt inom hello aktörstillstånd. hello gränssnittet projektet tillhandahåller ett gränssnitt som andra tjänster kan använda tooinvoke hello aktören.

### <a name="stateless-web-api"></a>Tillståndslösa webb-API
hello tillståndslös Web API-projekt innehåller en grundläggande webbtjänst som du kan använda tooopen-tooexternal-klienter. Mer information om hur hello projektet strukturerad finns [Service Fabric Web API-tjänster med OWIN värd själv](service-fabric-reliable-services-communication-webapi.md).


### <a name="aspnet-core"></a>ASP.NET core
hello Service Fabric SDK innehåller hello samma uppsättning ASP.NET Core mallar som är tillgängliga för fristående ASP.NET Core projekt: tom [Web API][aspnet-webapi], och [webbprogram][aspnet-webapp].

### <a name="guest-executables-and-guest-containers"></a>Gästen körbara filer och Gäst-behållare

Ett Service Fabric ”gäst” är en tjänst som inte är inbyggd med programmeringsmodeller hello-plattformen. Du kan även paketera hello binärfilerna för gäst antingen [direkt i hello programpaketet](service-fabric-deploy-existing-app.md) eller [via en behållare avbildning](service-fabric-deploy-container.md). I båda fallen Visual Studio skapar hello nödvändiga artefakter i hello **ApplicationPackageRoot** för hello projektet. Visual Studio kommer inte att skapa ett nytt service-projekt eftersom hello koden finns redan på en annan plats. Om du vill att toomanage gästerna projekt tillsammans med hello Service Fabric application-projekt, du kan lägga till dem toohello samma Visual Studio-lösning.

## <a name="next-steps"></a>Nästa steg
### <a name="create-an-azure-cluster"></a>Skapa ett Azure-kluster
hello Service Fabric SDK innehåller ett lokala kluster för utveckling och testning. toocreate ett kluster i Azure, se [installera ett Service Fabric-kluster från hello Azure-portalen][create-cluster-in-portal].

### <a name="publish-your-application-tooazure"></a>Publicera program-tooAzure
Du kan publicera programmet direkt från Visual Studio tooan Azure klustret. toolearn finns i avsnittet [publicering av program-tooAzure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-toovisualize-your-cluster"></a>Använda Service Fabric Explorer toovisualize klustret
Service Fabric Explorer erbjuder ett enkelt sätt toovisualize klustret, inklusive distribuerade program och fysiska struktur. Det finns fler toolearn [visualisera ditt kluster med hjälp av Service Fabric Explorer][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Version och uppgradera dina tjänster
Service Fabric kan oberoende versionshantering och uppgradering av oberoende tjänster i ett program. Det finns fler toolearn [versionshantering och uppgradera dina tjänster][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Konfigurera kontinuerlig integrering med Visual Studio Team Services
toolearn hur du kan ställa in en kontinuerlig integrationsprocess för ditt Service Fabric-program finns i [konfigurera kontinuerlig integrering med Visual Studio Team Services][ci-with-vso].

<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
