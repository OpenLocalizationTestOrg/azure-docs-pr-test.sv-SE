---
title: Hantera dina program i Visual Studio | Microsoft Docs
description: "Du kan använda Visual Studio för att skapa, utveckla, paketera, distribuera och felsöka din Service Fabric-program och tjänster."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: c317cb7e-7eae-466e-ba41-6aa2518be5cf
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.openlocfilehash: 3f6a47a15b74a7ceb6504b2834be62e76ab70bcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a><span data-ttu-id="7cf99-103">Använd Visual Studio för att förenkla skriva och hantera dina Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="7cf99-103">Use Visual Studio to simplify writing and managing your Service Fabric applications</span></span>
<span data-ttu-id="7cf99-104">Du kan hantera dina Azure Service Fabric-program och tjänster via Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7cf99-104">You can manage your Azure Service Fabric applications and services through Visual Studio.</span></span> <span data-ttu-id="7cf99-105">När du har [ställa in din utvecklingsmiljö](service-fabric-get-started.md), du kan använda Visual Studio skapar Service Fabric-program, lägga till tjänster eller paket, registrera och distribuera program i klustret för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="7cf99-105">Once you've [set up your development environment](service-fabric-get-started.md), you can use Visual Studio to create Service Fabric applications, add services, or package, register, and deploy applications in your local development cluster.</span></span>

## <a name="deploy-your-service-fabric-application"></a><span data-ttu-id="7cf99-106">Distribuera Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="7cf99-106">Deploy your Service Fabric application</span></span>
<span data-ttu-id="7cf99-107">Som standard kombinerar följande steg i en enkel åtgärd när du distribuerar ett program:</span><span class="sxs-lookup"><span data-stu-id="7cf99-107">By default, deploying an application combines the following steps into one simple operation:</span></span>

1. <span data-ttu-id="7cf99-108">Skapa programpaketet</span><span class="sxs-lookup"><span data-stu-id="7cf99-108">Creating the application package</span></span>
2. <span data-ttu-id="7cf99-109">Ladda upp programpaketet image store</span><span class="sxs-lookup"><span data-stu-id="7cf99-109">Uploading the application package to the image store</span></span>
3. <span data-ttu-id="7cf99-110">Registrerar programtyp</span><span class="sxs-lookup"><span data-stu-id="7cf99-110">Registering the application type</span></span>
4. <span data-ttu-id="7cf99-111">Att ta bort alla instanser av programmet körs</span><span class="sxs-lookup"><span data-stu-id="7cf99-111">Removing any running application instances</span></span>
5. <span data-ttu-id="7cf99-112">Skapa en instans av programmet</span><span class="sxs-lookup"><span data-stu-id="7cf99-112">Creating an application instance</span></span>

<span data-ttu-id="7cf99-113">I Visual Studio, trycka på **F5** distribuerar ditt program och koppla felsökaren till alla instanser av programmet.</span><span class="sxs-lookup"><span data-stu-id="7cf99-113">In Visual Studio, pressing **F5** deploys your application and attach the debugger to all application instances.</span></span> <span data-ttu-id="7cf99-114">Du kan använda **Ctrl + F5** du distribuerar ett program utan felsökning eller du kan publicera till en lokal eller fjärransluten kluster med hjälp av profilen.</span><span class="sxs-lookup"><span data-stu-id="7cf99-114">You can use **Ctrl+F5** to deploy an application without debugging, or you can publish to a local or remote cluster by using the publish profile.</span></span> <span data-ttu-id="7cf99-115">Mer information finns i [publicera ett program till ett kluster med hjälp av Visual Studio](service-fabric-publish-app-remote-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="7cf99-115">For more information, see [Publish an application to a remote cluster by using Visual Studio](service-fabric-publish-app-remote-cluster.md).</span></span>

### <a name="application-debug-mode"></a><span data-ttu-id="7cf99-116">Programmet felsökningsläge</span><span class="sxs-lookup"><span data-stu-id="7cf99-116">Application Debug Mode</span></span>
<span data-ttu-id="7cf99-117">Visual Studio tillhandahåller en egenskap som kallas **programmet felsökningsläge**, som styr hur du vill att Visual Studios att hantera programdistribution som en del av felsökning.</span><span class="sxs-lookup"><span data-stu-id="7cf99-117">Visual Studio provide a property called **Application Debug Mode**, which controls how you want Visual Studios to handle Application deployment as part of debugging.</span></span>

#### <a name="to-set-the-application-debug-mode-property"></a><span data-ttu-id="7cf99-118">Att ange egenskapen Debug programläge</span><span class="sxs-lookup"><span data-stu-id="7cf99-118">To set the Application Debug Mode property</span></span>
1. <span data-ttu-id="7cf99-119">På Service Fabric application projektets (*.sfproj) snabbmenyn väljer **egenskaper** (eller trycker på den **F4** nyckel).</span><span class="sxs-lookup"><span data-stu-id="7cf99-119">On the Service Fabric application project's (*.sfproj) shortcut menu, choose **Properties** (or press the **F4** key).</span></span>
2. <span data-ttu-id="7cf99-120">I den **egenskaper** fönstret kan du ange den **programmet felsökningsläge** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7cf99-120">In the **Properties** window, set the **Application Debug Mode** property.</span></span>

![Egenskapen programmet Debug-läge][debugmodeproperty]

#### <a name="application-debug-modes"></a><span data-ttu-id="7cf99-122">Programmet felsökningslägen</span><span class="sxs-lookup"><span data-stu-id="7cf99-122">Application Debug Modes</span></span>

1. <span data-ttu-id="7cf99-123">**Uppdatera program** detta läge kan du snabbt ändra och felsöka din kod och stöd för redigering av statiska filer vid felsökning.</span><span class="sxs-lookup"><span data-stu-id="7cf99-123">**Refresh Application** This mode enables you to quickly change and debug your code and supports editing static web files while debugging.</span></span> <span data-ttu-id="7cf99-124">Det här läget fungerar bara om lokal utveckling klustret är i [1 nod läge](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).</span><span class="sxs-lookup"><span data-stu-id="7cf99-124">This mode only works if your local development cluster is in [1-Node mode](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).</span></span>
2. <span data-ttu-id="7cf99-125">**Ta bort programmet** gör att programmet ska tas bort när debug-sessionen avslutas.</span><span class="sxs-lookup"><span data-stu-id="7cf99-125">**Remove Application** causes the application to be removed when the debug session ends.</span></span>
3. <span data-ttu-id="7cf99-126">**Automatisk uppgradering** programmet fortsätter att köras när debug-sessionen avslutas.</span><span class="sxs-lookup"><span data-stu-id="7cf99-126">**Auto Upgrade** The application continues to run when the debug session ends.</span></span> <span data-ttu-id="7cf99-127">Nästa felsökningssessionen behandlar distributionen som en uppgradering.</span><span class="sxs-lookup"><span data-stu-id="7cf99-127">The next debug session will treat the deployment as an upgrade.</span></span> <span data-ttu-id="7cf99-128">Uppgraderingsprocessen behåller alla data som du angav i föregående felsökningssessionen.</span><span class="sxs-lookup"><span data-stu-id="7cf99-128">The upgrade process preserves any data that you entered in a previous debug session.</span></span>
4. <span data-ttu-id="7cf99-129">**Hålla program** programmet körs i klustret när debug-sessionen avslutas.</span><span class="sxs-lookup"><span data-stu-id="7cf99-129">**Keep Application** The application keeps running in the cluster when the debug session ends.</span></span> <span data-ttu-id="7cf99-130">I början av nästa felsökningssessionen tas programmet bort.</span><span class="sxs-lookup"><span data-stu-id="7cf99-130">At the beginning of the next debug session, the application will be removed.</span></span>

<span data-ttu-id="7cf99-131">För **automatiskt uppgradera** bevaras data genom att använda programfunktioner för uppgradering av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="7cf99-131">For **Auto Upgrade** data is preserved by applying the application upgrade capabilities of Service Fabric.</span></span> <span data-ttu-id="7cf99-132">Mer information om hur du uppgraderar program och hur du kan utföra en uppgradering i en verklig miljö finns [uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="7cf99-132">For more information about upgrading applications and how you might perform an upgrade in a real environment, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

## <a name="add-a-service-to-your-service-fabric-application"></a><span data-ttu-id="7cf99-133">Lägga till en tjänst till Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="7cf99-133">Add a service to your Service Fabric application</span></span>
<span data-ttu-id="7cf99-134">Du kan lägga till nya tjänster i programmet för att utöka dess funktionalitet.</span><span class="sxs-lookup"><span data-stu-id="7cf99-134">You can add new services to your application to extend its functionality.</span></span>  <span data-ttu-id="7cf99-135">För att säkerställa att tjänsten ingår i ditt programpaket, lägger du till tjänsten via den **nya Fabric-tjänsten...**  menyalternativ.</span><span class="sxs-lookup"><span data-stu-id="7cf99-135">To ensure that the service is included in your application package, add the service through the **New Fabric Service...** menu item.</span></span>

![Lägg till en ny Service Fabric-tjänst][newservice]

<span data-ttu-id="7cf99-137">Välj en typ av Service Fabric-projekt att lägga till i ditt program och ange ett namn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7cf99-137">Select a Service Fabric project type to add to your application, and specify a name for the service.</span></span>  <span data-ttu-id="7cf99-138">Se [att välja ett ramverk för din tjänst](service-fabric-choose-framework.md) för att avgöra vilken tjänsttyp som ska använda.</span><span class="sxs-lookup"><span data-stu-id="7cf99-138">See [Choosing a framework for your service](service-fabric-choose-framework.md) to help you decide which service type to use.</span></span>

![Välj en projekttyp för Service Fabric-tjänsten att lägga till i ditt program][addserviceproject]

<span data-ttu-id="7cf99-140">Den nya tjänsten har lagts till i din lösning och befintliga programpaket.</span><span class="sxs-lookup"><span data-stu-id="7cf99-140">The new service is added to your solution and existing application package.</span></span> <span data-ttu-id="7cf99-141">Referenser för och en standard tjänstinstans läggs till applikationsmanifestet orsakar tjänsten kan skapas och startas nästa gång du distribuerar programmet.</span><span class="sxs-lookup"><span data-stu-id="7cf99-141">The service references and a default service instance will be added to the application manifest, causing the service to be created and started the next time you deploy the application.</span></span>

![Den nya tjänsten har lagts till i programmanifestet][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a><span data-ttu-id="7cf99-143">Paketera Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="7cf99-143">Package your Service Fabric application</span></span>
<span data-ttu-id="7cf99-144">Om du vill distribuera programmet och dess tjänster till ett kluster, måste du skapa ett programpaket.</span><span class="sxs-lookup"><span data-stu-id="7cf99-144">To deploy the application and its services to a cluster, you need to create an application package.</span></span>  <span data-ttu-id="7cf99-145">Paketet organiserar programmanifestet service manifest och andra nödvändiga filer i en viss layout.</span><span class="sxs-lookup"><span data-stu-id="7cf99-145">The package organizes the application manifest, service manifests, and other necessary files in a specific layout.</span></span>  <span data-ttu-id="7cf99-146">Visual Studio konfigurerar och hanterar paket i programprojektet-mappen i katalogen 'pkg'.</span><span class="sxs-lookup"><span data-stu-id="7cf99-146">Visual Studio sets up and manages the package in the application project's folder, in the 'pkg' directory.</span></span>  <span data-ttu-id="7cf99-147">Klicka på **paketet** från den **programmet** snabbmenyn skapar eller uppdaterar programpaketet.</span><span class="sxs-lookup"><span data-stu-id="7cf99-147">Clicking **Package** from the **Application** context menu creates or updates the application package.</span></span>

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a><span data-ttu-id="7cf99-148">Ta bort program och programtyper med Cloud Explorer</span><span class="sxs-lookup"><span data-stu-id="7cf99-148">Remove applications and application types using Cloud Explorer</span></span>
<span data-ttu-id="7cf99-149">Du kan utföra grundläggande klusterhanteringsåtgärder inifrån Visual Studio med Cloud Explorer som du kan starta från den **visa** menyn.</span><span class="sxs-lookup"><span data-stu-id="7cf99-149">You can perform basic cluster management operations from within Visual Studio using Cloud Explorer, which you can launch from the **View** menu.</span></span> <span data-ttu-id="7cf99-150">Du kan till exempel ta bort program och avetablera programtyper på lokala eller fjärranslutna kluster.</span><span class="sxs-lookup"><span data-stu-id="7cf99-150">For instance, you can delete applications and unprovision application types on local or remote clusters.</span></span>

![Ta bort ett program][removeapplication]

> [!TIP]
> <span data-ttu-id="7cf99-152">Rikare hanteringsfunktioner för klustret, se [visualisera ditt kluster med Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="7cf99-152">For richer cluster management functionality, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
>
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="7cf99-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7cf99-153">Next steps</span></span>
* [<span data-ttu-id="7cf99-154">Service Fabric programmodell</span><span class="sxs-lookup"><span data-stu-id="7cf99-154">Service Fabric application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="7cf99-155">Distribution av Service Fabric</span><span class="sxs-lookup"><span data-stu-id="7cf99-155">Service Fabric application deployment</span></span>](service-fabric-deploy-remove-applications.md)
* [<span data-ttu-id="7cf99-156">Hantera programparametrar för miljöer med flera</span><span class="sxs-lookup"><span data-stu-id="7cf99-156">Managing application parameters for multiple environments</span></span>](service-fabric-manage-multiple-environment-app-configuration.md)
* [<span data-ttu-id="7cf99-157">Felsöka ditt Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="7cf99-157">Debugging your Service Fabric application</span></span>](service-fabric-debugging-your-application.md)
* [<span data-ttu-id="7cf99-158">Visualisera ditt kluster med hjälp av Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="7cf99-158">Visualizing your cluster by using Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png