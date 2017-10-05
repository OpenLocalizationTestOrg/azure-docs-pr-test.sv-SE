---
title: "Nästa steg för Service Fabric-projekt skapa | Microsoft Docs"
description: "Den här artikeln innehåller länkar till en uppsättning core development uppgifter för Service Fabric"
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
ms.openlocfilehash: 74019850c507902d9ef7ec47a364fff234aaf32b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="your-service-fabric-application-and-next-steps"></a><span data-ttu-id="66e69-103">Service Fabric-programmet och nästa steg</span><span class="sxs-lookup"><span data-stu-id="66e69-103">Your Service Fabric application and next steps</span></span>
<span data-ttu-id="66e69-104">Azure Service Fabric-programmet har skapats.</span><span class="sxs-lookup"><span data-stu-id="66e69-104">Your Azure Service Fabric application has been created.</span></span> <span data-ttu-id="66e69-105">Den här artikeln beskriver makeup för ditt projekt och vissa potentiella nästa steg.</span><span class="sxs-lookup"><span data-stu-id="66e69-105">This article describes the makeup of your project and some potential next steps.</span></span>

## <a name="your-application"></a><span data-ttu-id="66e69-106">Ditt program</span><span class="sxs-lookup"><span data-stu-id="66e69-106">Your application</span></span>
<span data-ttu-id="66e69-107">Alla nya program innehåller ett projekt för programmet.</span><span class="sxs-lookup"><span data-stu-id="66e69-107">Every new application includes an application project.</span></span> <span data-ttu-id="66e69-108">Det kan finnas en eller två ytterligare projekt, beroende på vilken sorts tjänst som är valt.</span><span class="sxs-lookup"><span data-stu-id="66e69-108">There may be one or two additional projects, depending on the type of service chosen.</span></span>

### <a name="the-application-project"></a><span data-ttu-id="66e69-109">Application-projekt</span><span class="sxs-lookup"><span data-stu-id="66e69-109">The application project</span></span>
<span data-ttu-id="66e69-110">Projektet för konsolprogrammet består av:</span><span class="sxs-lookup"><span data-stu-id="66e69-110">The application project consists of:</span></span>

* <span data-ttu-id="66e69-111">En uppsättning referenser till de tjänster som utgör ditt program.</span><span class="sxs-lookup"><span data-stu-id="66e69-111">A set of references to the services that make up your application.</span></span>
* <span data-ttu-id="66e69-112">Tre publicera profiler (1-nod lokala 5-nod lokala och moln) som du kan använda för att underhålla inställningar för att arbeta med olika miljöer, till exempel göra inställningar för en kluster-slutpunkt och om du vill utföra uppgraderingen distributioner som standard.</span><span class="sxs-lookup"><span data-stu-id="66e69-112">Three publish profiles (1-Node Local, 5-Node Local, and Cloud) that you can use to maintain preferences for working with different environments--such as preferences related to a cluster endpoint and whether to perform upgrade deployments by default.</span></span>
* <span data-ttu-id="66e69-113">Tre program parametern filer (samma som ovan) som du kan använda för att underhålla miljö-specifika programkonfigurationer, till exempel antalet partitioner för att skapa för en tjänst.</span><span class="sxs-lookup"><span data-stu-id="66e69-113">Three application parameter files (same as above) that you can use to maintain environment-specific application configurations, such as the number of partitions to create for a service.</span></span>
* <span data-ttu-id="66e69-114">Ett skript för distribution som du kan använda för att distribuera ditt program från kommandoraden eller som en del av en automatisk kontinuerlig integrering och distribution pipeline.</span><span class="sxs-lookup"><span data-stu-id="66e69-114">A deployment script that you can use to deploy your application from the command line or as part of an automated continuous integration and deployment pipeline.</span></span>
* <span data-ttu-id="66e69-115">Applikationsmanifestet som beskriver programmet.</span><span class="sxs-lookup"><span data-stu-id="66e69-115">The application manifest, which describes the application.</span></span> <span data-ttu-id="66e69-116">Manifestet hittar du under mappen ApplicationPackageRoot.</span><span class="sxs-lookup"><span data-stu-id="66e69-116">You can find the manifest under the ApplicationPackageRoot folder.</span></span>

### <a name="stateless-service"></a><span data-ttu-id="66e69-117">Tillståndslösa tjänsten</span><span class="sxs-lookup"><span data-stu-id="66e69-117">Stateless service</span></span>
<span data-ttu-id="66e69-118">När du lägger till en ny tjänst för tillståndslösa Visual Studio lägger till en service-projekt i lösningen som innehåller en typ som underordnade `StatelessService`.</span><span class="sxs-lookup"><span data-stu-id="66e69-118">When you add a new stateless service, Visual Studio adds a service project to your solution that includes a type descended from `StatelessService`.</span></span> <span data-ttu-id="66e69-119">Tjänsten ökar en lokal variabel i en räknare.</span><span class="sxs-lookup"><span data-stu-id="66e69-119">The service increments a local variable in a counter.</span></span>

### <a name="stateful-service"></a><span data-ttu-id="66e69-120">Tillståndskänslig service</span><span class="sxs-lookup"><span data-stu-id="66e69-120">Stateful service</span></span>
<span data-ttu-id="66e69-121">När du lägger till en ny tjänst för tillståndskänsliga Visual Studio lägger till en service-projekt i lösningen som innehåller en typ som underordnade `StatefulService`.</span><span class="sxs-lookup"><span data-stu-id="66e69-121">When you add a new stateful service, Visual Studio adds a service project to your solution that includes a type descended from `StatefulService`.</span></span> <span data-ttu-id="66e69-122">Tjänsten ökar en räknare i dess `RunAsync` metod och lagrar resultatet i ett `ReliableDictionary`.</span><span class="sxs-lookup"><span data-stu-id="66e69-122">The service increments a counter in its `RunAsync` method and stores the result in a `ReliableDictionary`.</span></span>

### <a name="actor-service"></a><span data-ttu-id="66e69-123">Aktören service</span><span class="sxs-lookup"><span data-stu-id="66e69-123">Actor service</span></span>
<span data-ttu-id="66e69-124">När du lägger till en ny tillförlitliga aktören Visual Studio lägger till två projekten i din lösning: en aktören projektet och ett gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="66e69-124">When you add a new reliable actor, Visual Studio adds two projects to your solution: an actor project and an interface project.</span></span>

<span data-ttu-id="66e69-125">Aktören projektet innehåller metoder för inställningen och på ett tillförlitligt sätt att hämta värdet på en räknare som sparas i den aktörstillstånd.</span><span class="sxs-lookup"><span data-stu-id="66e69-125">The actor project provides methods for setting and getting the value of a counter that is reliably persisted within the actor's state.</span></span> <span data-ttu-id="66e69-126">Gränssnittet projektet tillhandahåller ett gränssnitt som andra tjänster kan använda för att anropa aktören.</span><span class="sxs-lookup"><span data-stu-id="66e69-126">The interface project provides an interface that other services can use to invoke the actor.</span></span>

### <a name="stateless-web-api"></a><span data-ttu-id="66e69-127">Tillståndslösa webb-API</span><span class="sxs-lookup"><span data-stu-id="66e69-127">Stateless Web API</span></span>
<span data-ttu-id="66e69-128">Tillståndslösa Web API-projektet innehåller en grundläggande webbtjänst som du kan använda för att öppna programmet till externa klienter.</span><span class="sxs-lookup"><span data-stu-id="66e69-128">The stateless Web API project provides a basic web service that you can use to open your application to external clients.</span></span> <span data-ttu-id="66e69-129">Mer information om hur projektet strukturerad, finns [Service Fabric Web API-tjänster med OWIN värd själv](service-fabric-reliable-services-communication-webapi.md).</span><span class="sxs-lookup"><span data-stu-id="66e69-129">For more information about how the project structured, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md).</span></span>


### <a name="aspnet-core"></a><span data-ttu-id="66e69-130">ASP.NET core</span><span class="sxs-lookup"><span data-stu-id="66e69-130">ASP.NET core</span></span>
<span data-ttu-id="66e69-131">Service Fabric-SDK innehåller samma uppsättning ASP.NET Core mallar som är tillgängliga för fristående ASP.NET Core projekt: tom [Web API][aspnet-webapi], och [webbprogram] [aspnet-webapp].</span><span class="sxs-lookup"><span data-stu-id="66e69-131">The Service Fabric SDK provides the same set of ASP.NET Core templates that are available for standalone ASP.NET Core projects: empty, [Web API][aspnet-webapi], and [Web Application][aspnet-webapp].</span></span>

### <a name="guest-executables-and-guest-containers"></a><span data-ttu-id="66e69-132">Gästen körbara filer och Gäst-behållare</span><span class="sxs-lookup"><span data-stu-id="66e69-132">Guest executables and guest containers</span></span>

<span data-ttu-id="66e69-133">Ett Service Fabric ”gäst” är en tjänst som inte är inbyggd med plattformens programmeringsmodeller.</span><span class="sxs-lookup"><span data-stu-id="66e69-133">A Service Fabric 'guest' is a service that is not built with the platform's programming models.</span></span> <span data-ttu-id="66e69-134">Du kan även Paketera om binärfilerna för gäst antingen [direkt i programpaketet](service-fabric-deploy-existing-app.md) eller [via en behållare avbildning](service-fabric-deploy-container.md).</span><span class="sxs-lookup"><span data-stu-id="66e69-134">You can package the binaries for a guest either [directly in the application package](service-fabric-deploy-existing-app.md) or [through a container image](service-fabric-deploy-container.md).</span></span> <span data-ttu-id="66e69-135">I båda fallen Visual Studio skapar nödvändiga artefakter i den **ApplicationPackageRoot** mappen i projektet för konsolprogrammet.</span><span class="sxs-lookup"><span data-stu-id="66e69-135">In both cases, Visual Studio creates the necessary artifacts in the **ApplicationPackageRoot** folder of the application project.</span></span> <span data-ttu-id="66e69-136">Visual Studio kommer inte att skapa ett nytt service-projekt eftersom koden redan finns någon annanstans.</span><span class="sxs-lookup"><span data-stu-id="66e69-136">Visual Studio will not create a new service project because the code already exists elsewhere.</span></span> <span data-ttu-id="66e69-137">Om du vill hantera projektet gäst tillsammans med Service Fabric application-projekt kan du lägga till dem i samma Visual Studio-lösning.</span><span class="sxs-lookup"><span data-stu-id="66e69-137">If you would like to manage your guest projects alongside the Service Fabric application project, you can add them to the same Visual Studio solution.</span></span>

## <a name="next-steps"></a><span data-ttu-id="66e69-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66e69-138">Next steps</span></span>
### <a name="create-an-azure-cluster"></a><span data-ttu-id="66e69-139">Skapa ett Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="66e69-139">Create an Azure cluster</span></span>
<span data-ttu-id="66e69-140">Service Fabric-SDK tillhandahåller ett lokala kluster för utveckling och testning.</span><span class="sxs-lookup"><span data-stu-id="66e69-140">The Service Fabric SDK provides a local cluster for development and testing.</span></span> <span data-ttu-id="66e69-141">Om du vill skapa ett kluster i Azure, se [ställa in ett Service Fabric-kluster från Azure portal][create-cluster-in-portal].</span><span class="sxs-lookup"><span data-stu-id="66e69-141">To create a cluster in Azure, see [Setting up a Service Fabric cluster from the Azure portal][create-cluster-in-portal].</span></span>

### <a name="publish-your-application-to-azure"></a><span data-ttu-id="66e69-142">Publicera programmet till Azure</span><span class="sxs-lookup"><span data-stu-id="66e69-142">Publish your application to Azure</span></span>
<span data-ttu-id="66e69-143">Du kan publicera programmet direkt från Visual Studio till ett Azure-kluster.</span><span class="sxs-lookup"><span data-stu-id="66e69-143">You can publish your application directly from Visual Studio to an Azure cluster.</span></span> <span data-ttu-id="66e69-144">Mer information finns i avsnittet [publicering av programmet till Azure][publish-app-to-azure].</span><span class="sxs-lookup"><span data-stu-id="66e69-144">To learn how, see [Publishing your application to Azure][publish-app-to-azure].</span></span>

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a><span data-ttu-id="66e69-145">Använda Service Fabric Explorer för att visualisera ditt kluster</span><span class="sxs-lookup"><span data-stu-id="66e69-145">Use Service Fabric Explorer to visualize your cluster</span></span>
<span data-ttu-id="66e69-146">Service Fabric Explorer erbjuder ett enkelt sätt att visualisera ditt kluster, inklusive distribuerade program och fysiska struktur.</span><span class="sxs-lookup"><span data-stu-id="66e69-146">Service Fabric Explorer offers an easy way to visualize your cluster, including deployed applications and physical layout.</span></span> <span data-ttu-id="66e69-147">Läs mer i [visualisera ditt kluster med hjälp av Service Fabric Explorer][visualize-with-sfx].</span><span class="sxs-lookup"><span data-stu-id="66e69-147">To learn more, see [Visualizing your cluster by using Service Fabric Explorer][visualize-with-sfx].</span></span>

### <a name="version-and-upgrade-your-services"></a><span data-ttu-id="66e69-148">Version och uppgradera dina tjänster</span><span class="sxs-lookup"><span data-stu-id="66e69-148">Version and upgrade your services</span></span>
<span data-ttu-id="66e69-149">Service Fabric kan oberoende versionshantering och uppgradering av oberoende tjänster i ett program.</span><span class="sxs-lookup"><span data-stu-id="66e69-149">Service Fabric enables independent versioning and upgrading of independent services in an application.</span></span> <span data-ttu-id="66e69-150">Läs mer i [versionshantering och uppgradera dina tjänster][app-upgrade-tutorial].</span><span class="sxs-lookup"><span data-stu-id="66e69-150">To learn more, see [Versioning and upgrading your services][app-upgrade-tutorial].</span></span>

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a><span data-ttu-id="66e69-151">Konfigurera kontinuerlig integrering med Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="66e69-151">Configure continuous integration with Visual Studio Team Services</span></span>
<span data-ttu-id="66e69-152">Information om hur du ställer in en kontinuerlig integrationsprocess för Service Fabric-program finns [konfigurera kontinuerlig integrering med Visual Studio Team Services][ci-with-vso].</span><span class="sxs-lookup"><span data-stu-id="66e69-152">To learn how you can set up a continuous integration process for your Service Fabric application, see [Configure continuous integration with Visual Studio Team Services][ci-with-vso].</span></span>

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
