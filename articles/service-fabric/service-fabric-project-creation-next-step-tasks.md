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
# <a name="your-service-fabric-application-and-next-steps"></a><span data-ttu-id="2751c-103">Service Fabric-programmet och nästa steg</span><span class="sxs-lookup"><span data-stu-id="2751c-103">Your Service Fabric application and next steps</span></span>
<span data-ttu-id="2751c-104">Azure Service Fabric-programmet har skapats.</span><span class="sxs-lookup"><span data-stu-id="2751c-104">Your Azure Service Fabric application has been created.</span></span> <span data-ttu-id="2751c-105">Den här artikeln beskriver hello makeup för ditt projekt och vissa potentiella nästa steg.</span><span class="sxs-lookup"><span data-stu-id="2751c-105">This article describes hello makeup of your project and some potential next steps.</span></span>

## <a name="your-application"></a><span data-ttu-id="2751c-106">Ditt program</span><span class="sxs-lookup"><span data-stu-id="2751c-106">Your application</span></span>
<span data-ttu-id="2751c-107">Alla nya program innehåller ett projekt för programmet.</span><span class="sxs-lookup"><span data-stu-id="2751c-107">Every new application includes an application project.</span></span> <span data-ttu-id="2751c-108">Det kan finnas en eller två ytterligare projekt, beroende på hello typ av tjänst som är valt.</span><span class="sxs-lookup"><span data-stu-id="2751c-108">There may be one or two additional projects, depending on hello type of service chosen.</span></span>

### <a name="hello-application-project"></a><span data-ttu-id="2751c-109">hello application-projekt</span><span class="sxs-lookup"><span data-stu-id="2751c-109">hello application project</span></span>
<span data-ttu-id="2751c-110">hello-programprojekt består av:</span><span class="sxs-lookup"><span data-stu-id="2751c-110">hello application project consists of:</span></span>

* <span data-ttu-id="2751c-111">En uppsättning referenser toohello tjänster som utgör ditt program.</span><span class="sxs-lookup"><span data-stu-id="2751c-111">A set of references toohello services that make up your application.</span></span>
* <span data-ttu-id="2751c-112">Tre publicera profiler (1-nod lokala 5-nod lokala och moln) som du kan använda toomaintain inställningar för att arbeta med olika miljöer, till exempel inställningar relaterade tooa klusterslutpunkten och om tooperform uppgradera distributioner som standard.</span><span class="sxs-lookup"><span data-stu-id="2751c-112">Three publish profiles (1-Node Local, 5-Node Local, and Cloud) that you can use toomaintain preferences for working with different environments--such as preferences related tooa cluster endpoint and whether tooperform upgrade deployments by default.</span></span>
* <span data-ttu-id="2751c-113">Tre parametern programfiler (samma som ovan) som du kan använda toomaintain miljö-specifika programkonfigurationer, till exempel hello antal partitioner toocreate för en tjänst.</span><span class="sxs-lookup"><span data-stu-id="2751c-113">Three application parameter files (same as above) that you can use toomaintain environment-specific application configurations, such as hello number of partitions toocreate for a service.</span></span>
* <span data-ttu-id="2751c-114">Ett skript för distribution som du kan använda toodeploy programmet hello kommandoraden eller som en del av en automatisk kontinuerlig integrering och distribution pipeline.</span><span class="sxs-lookup"><span data-stu-id="2751c-114">A deployment script that you can use toodeploy your application from hello command line or as part of an automated continuous integration and deployment pipeline.</span></span>
* <span data-ttu-id="2751c-115">hello programmanifestet som beskriver hello program.</span><span class="sxs-lookup"><span data-stu-id="2751c-115">hello application manifest, which describes hello application.</span></span> <span data-ttu-id="2751c-116">Du hittar hello manifestet hello ApplicationPackageRoot i mappen.</span><span class="sxs-lookup"><span data-stu-id="2751c-116">You can find hello manifest under hello ApplicationPackageRoot folder.</span></span>

### <a name="stateless-service"></a><span data-ttu-id="2751c-117">Tillståndslösa tjänsten</span><span class="sxs-lookup"><span data-stu-id="2751c-117">Stateless service</span></span>
<span data-ttu-id="2751c-118">När du lägger till en ny tjänst för tillståndslösa Visual Studio lägger till ett projekt tooyour lösning som innehåller en typ som underordnade `StatelessService`.</span><span class="sxs-lookup"><span data-stu-id="2751c-118">When you add a new stateless service, Visual Studio adds a service project tooyour solution that includes a type descended from `StatelessService`.</span></span> <span data-ttu-id="2751c-119">hello service delar en lokal variabel i en räknare.</span><span class="sxs-lookup"><span data-stu-id="2751c-119">hello service increments a local variable in a counter.</span></span>

### <a name="stateful-service"></a><span data-ttu-id="2751c-120">Tillståndskänslig service</span><span class="sxs-lookup"><span data-stu-id="2751c-120">Stateful service</span></span>
<span data-ttu-id="2751c-121">När du lägger till en ny tjänst för tillståndskänsliga Visual Studio lägger till ett projekt tooyour lösning som innehåller en typ som underordnade `StatefulService`.</span><span class="sxs-lookup"><span data-stu-id="2751c-121">When you add a new stateful service, Visual Studio adds a service project tooyour solution that includes a type descended from `StatefulService`.</span></span> <span data-ttu-id="2751c-122">hello service ökar en räknare i dess `RunAsync` metoden och lagrar hello resultera i en `ReliableDictionary`.</span><span class="sxs-lookup"><span data-stu-id="2751c-122">hello service increments a counter in its `RunAsync` method and stores hello result in a `ReliableDictionary`.</span></span>

### <a name="actor-service"></a><span data-ttu-id="2751c-123">Aktören service</span><span class="sxs-lookup"><span data-stu-id="2751c-123">Actor service</span></span>
<span data-ttu-id="2751c-124">När du lägger till en ny tillförlitliga aktören Visual Studio lägger till två projekt tooyour lösning: en aktören projektet och ett gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="2751c-124">When you add a new reliable actor, Visual Studio adds two projects tooyour solution: an actor project and an interface project.</span></span>

<span data-ttu-id="2751c-125">hello aktören projektet tillhandahåller metoder för inställningen och hämta hello värdet på en räknare som sparas på ett tillförlitligt sätt inom hello aktörstillstånd.</span><span class="sxs-lookup"><span data-stu-id="2751c-125">hello actor project provides methods for setting and getting hello value of a counter that is reliably persisted within hello actor's state.</span></span> <span data-ttu-id="2751c-126">hello gränssnittet projektet tillhandahåller ett gränssnitt som andra tjänster kan använda tooinvoke hello aktören.</span><span class="sxs-lookup"><span data-stu-id="2751c-126">hello interface project provides an interface that other services can use tooinvoke hello actor.</span></span>

### <a name="stateless-web-api"></a><span data-ttu-id="2751c-127">Tillståndslösa webb-API</span><span class="sxs-lookup"><span data-stu-id="2751c-127">Stateless Web API</span></span>
<span data-ttu-id="2751c-128">hello tillståndslös Web API-projekt innehåller en grundläggande webbtjänst som du kan använda tooopen-tooexternal-klienter.</span><span class="sxs-lookup"><span data-stu-id="2751c-128">hello stateless Web API project provides a basic web service that you can use tooopen your application tooexternal clients.</span></span> <span data-ttu-id="2751c-129">Mer information om hur hello projektet strukturerad finns [Service Fabric Web API-tjänster med OWIN värd själv](service-fabric-reliable-services-communication-webapi.md).</span><span class="sxs-lookup"><span data-stu-id="2751c-129">For more information about how hello project structured, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md).</span></span>


### <a name="aspnet-core"></a><span data-ttu-id="2751c-130">ASP.NET core</span><span class="sxs-lookup"><span data-stu-id="2751c-130">ASP.NET core</span></span>
<span data-ttu-id="2751c-131">hello Service Fabric SDK innehåller hello samma uppsättning ASP.NET Core mallar som är tillgängliga för fristående ASP.NET Core projekt: tom [Web API][aspnet-webapi], och [webbprogram][aspnet-webapp].</span><span class="sxs-lookup"><span data-stu-id="2751c-131">hello Service Fabric SDK provides hello same set of ASP.NET Core templates that are available for standalone ASP.NET Core projects: empty, [Web API][aspnet-webapi], and [Web Application][aspnet-webapp].</span></span>

### <a name="guest-executables-and-guest-containers"></a><span data-ttu-id="2751c-132">Gästen körbara filer och Gäst-behållare</span><span class="sxs-lookup"><span data-stu-id="2751c-132">Guest executables and guest containers</span></span>

<span data-ttu-id="2751c-133">Ett Service Fabric ”gäst” är en tjänst som inte är inbyggd med programmeringsmodeller hello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="2751c-133">A Service Fabric 'guest' is a service that is not built with hello platform's programming models.</span></span> <span data-ttu-id="2751c-134">Du kan även paketera hello binärfilerna för gäst antingen [direkt i hello programpaketet](service-fabric-deploy-existing-app.md) eller [via en behållare avbildning](service-fabric-deploy-container.md).</span><span class="sxs-lookup"><span data-stu-id="2751c-134">You can package hello binaries for a guest either [directly in hello application package](service-fabric-deploy-existing-app.md) or [through a container image](service-fabric-deploy-container.md).</span></span> <span data-ttu-id="2751c-135">I båda fallen Visual Studio skapar hello nödvändiga artefakter i hello **ApplicationPackageRoot** för hello projektet.</span><span class="sxs-lookup"><span data-stu-id="2751c-135">In both cases, Visual Studio creates hello necessary artifacts in hello **ApplicationPackageRoot** folder of hello application project.</span></span> <span data-ttu-id="2751c-136">Visual Studio kommer inte att skapa ett nytt service-projekt eftersom hello koden finns redan på en annan plats.</span><span class="sxs-lookup"><span data-stu-id="2751c-136">Visual Studio will not create a new service project because hello code already exists elsewhere.</span></span> <span data-ttu-id="2751c-137">Om du vill att toomanage gästerna projekt tillsammans med hello Service Fabric application-projekt, du kan lägga till dem toohello samma Visual Studio-lösning.</span><span class="sxs-lookup"><span data-stu-id="2751c-137">If you would like toomanage your guest projects alongside hello Service Fabric application project, you can add them toohello same Visual Studio solution.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2751c-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2751c-138">Next steps</span></span>
### <a name="create-an-azure-cluster"></a><span data-ttu-id="2751c-139">Skapa ett Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="2751c-139">Create an Azure cluster</span></span>
<span data-ttu-id="2751c-140">hello Service Fabric SDK innehåller ett lokala kluster för utveckling och testning.</span><span class="sxs-lookup"><span data-stu-id="2751c-140">hello Service Fabric SDK provides a local cluster for development and testing.</span></span> <span data-ttu-id="2751c-141">toocreate ett kluster i Azure, se [installera ett Service Fabric-kluster från hello Azure-portalen][create-cluster-in-portal].</span><span class="sxs-lookup"><span data-stu-id="2751c-141">toocreate a cluster in Azure, see [Setting up a Service Fabric cluster from hello Azure portal][create-cluster-in-portal].</span></span>

### <a name="publish-your-application-tooazure"></a><span data-ttu-id="2751c-142">Publicera program-tooAzure</span><span class="sxs-lookup"><span data-stu-id="2751c-142">Publish your application tooAzure</span></span>
<span data-ttu-id="2751c-143">Du kan publicera programmet direkt från Visual Studio tooan Azure klustret.</span><span class="sxs-lookup"><span data-stu-id="2751c-143">You can publish your application directly from Visual Studio tooan Azure cluster.</span></span> <span data-ttu-id="2751c-144">toolearn finns i avsnittet [publicering av program-tooAzure][publish-app-to-azure].</span><span class="sxs-lookup"><span data-stu-id="2751c-144">toolearn how, see [Publishing your application tooAzure][publish-app-to-azure].</span></span>

### <a name="use-service-fabric-explorer-toovisualize-your-cluster"></a><span data-ttu-id="2751c-145">Använda Service Fabric Explorer toovisualize klustret</span><span class="sxs-lookup"><span data-stu-id="2751c-145">Use Service Fabric Explorer toovisualize your cluster</span></span>
<span data-ttu-id="2751c-146">Service Fabric Explorer erbjuder ett enkelt sätt toovisualize klustret, inklusive distribuerade program och fysiska struktur.</span><span class="sxs-lookup"><span data-stu-id="2751c-146">Service Fabric Explorer offers an easy way toovisualize your cluster, including deployed applications and physical layout.</span></span> <span data-ttu-id="2751c-147">Det finns fler toolearn [visualisera ditt kluster med hjälp av Service Fabric Explorer][visualize-with-sfx].</span><span class="sxs-lookup"><span data-stu-id="2751c-147">toolearn more, see [Visualizing your cluster by using Service Fabric Explorer][visualize-with-sfx].</span></span>

### <a name="version-and-upgrade-your-services"></a><span data-ttu-id="2751c-148">Version och uppgradera dina tjänster</span><span class="sxs-lookup"><span data-stu-id="2751c-148">Version and upgrade your services</span></span>
<span data-ttu-id="2751c-149">Service Fabric kan oberoende versionshantering och uppgradering av oberoende tjänster i ett program.</span><span class="sxs-lookup"><span data-stu-id="2751c-149">Service Fabric enables independent versioning and upgrading of independent services in an application.</span></span> <span data-ttu-id="2751c-150">Det finns fler toolearn [versionshantering och uppgradera dina tjänster][app-upgrade-tutorial].</span><span class="sxs-lookup"><span data-stu-id="2751c-150">toolearn more, see [Versioning and upgrading your services][app-upgrade-tutorial].</span></span>

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a><span data-ttu-id="2751c-151">Konfigurera kontinuerlig integrering med Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="2751c-151">Configure continuous integration with Visual Studio Team Services</span></span>
<span data-ttu-id="2751c-152">toolearn hur du kan ställa in en kontinuerlig integrationsprocess för ditt Service Fabric-program finns i [konfigurera kontinuerlig integrering med Visual Studio Team Services][ci-with-vso].</span><span class="sxs-lookup"><span data-stu-id="2751c-152">toolearn how you can set up a continuous integration process for your Service Fabric application, see [Configure continuous integration with Visual Studio Team Services][ci-with-vso].</span></span>

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
