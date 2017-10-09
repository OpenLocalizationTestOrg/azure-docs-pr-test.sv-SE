---
title: aaaManage dina program i Visual Studio | Microsoft Docs
description: "Använda Visual Studio toocreate, utveckla, paket, distribuera och felsöka din Service Fabric-program och tjänster."
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
ms.openlocfilehash: b2d5803d85e4f9645dcbece33a2208bc0955498d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-visual-studio-toosimplify-writing-and-managing-your-service-fabric-applications"></a><span data-ttu-id="93bd7-103">Använd Visual Studio toosimplify skrivning och hantera dina Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="93bd7-103">Use Visual Studio toosimplify writing and managing your Service Fabric applications</span></span>
<span data-ttu-id="93bd7-104">Du kan hantera dina Azure Service Fabric-program och tjänster via Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93bd7-104">You can manage your Azure Service Fabric applications and services through Visual Studio.</span></span> <span data-ttu-id="93bd7-105">När du har [ställa in din utvecklingsmiljö](service-fabric-get-started.md), du kan använda Visual Studio toocreate Service Fabric-program, Lägg till tjänster eller paket, registrera och distribuerar program i klustret för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="93bd7-105">Once you've [set up your development environment](service-fabric-get-started.md), you can use Visual Studio toocreate Service Fabric applications, add services, or package, register, and deploy applications in your local development cluster.</span></span>

## <a name="deploy-your-service-fabric-application"></a><span data-ttu-id="93bd7-106">Distribuera Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="93bd7-106">Deploy your Service Fabric application</span></span>
<span data-ttu-id="93bd7-107">Som standard kombinerar hello följa stegen i en enkel åtgärd om du distribuerar ett program:</span><span class="sxs-lookup"><span data-stu-id="93bd7-107">By default, deploying an application combines hello following steps into one simple operation:</span></span>

1. <span data-ttu-id="93bd7-108">Skapa hello programpaket</span><span class="sxs-lookup"><span data-stu-id="93bd7-108">Creating hello application package</span></span>
2. <span data-ttu-id="93bd7-109">Överför hello programmet paketet toohello avbildningsarkivet</span><span class="sxs-lookup"><span data-stu-id="93bd7-109">Uploading hello application package toohello image store</span></span>
3. <span data-ttu-id="93bd7-110">Registrera hello programtyp</span><span class="sxs-lookup"><span data-stu-id="93bd7-110">Registering hello application type</span></span>
4. <span data-ttu-id="93bd7-111">Att ta bort alla instanser av programmet körs</span><span class="sxs-lookup"><span data-stu-id="93bd7-111">Removing any running application instances</span></span>
5. <span data-ttu-id="93bd7-112">Skapa en instans av programmet</span><span class="sxs-lookup"><span data-stu-id="93bd7-112">Creating an application instance</span></span>

<span data-ttu-id="93bd7-113">I Visual Studio, trycka på **F5** distribuerar ditt program och bifoga hello felsökare tooall programinstanser.</span><span class="sxs-lookup"><span data-stu-id="93bd7-113">In Visual Studio, pressing **F5** deploys your application and attach hello debugger tooall application instances.</span></span> <span data-ttu-id="93bd7-114">Du kan använda **Ctrl + F5** toodeploy ett program utan felsökning eller du kan publicera lokala tooa eller kluster med hjälp av hello Publicera profil.</span><span class="sxs-lookup"><span data-stu-id="93bd7-114">You can use **Ctrl+F5** toodeploy an application without debugging, or you can publish tooa local or remote cluster by using hello publish profile.</span></span> <span data-ttu-id="93bd7-115">Mer information finns i [publicera ett program tooa kluster med hjälp av Visual Studio](service-fabric-publish-app-remote-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="93bd7-115">For more information, see [Publish an application tooa remote cluster by using Visual Studio](service-fabric-publish-app-remote-cluster.md).</span></span>

### <a name="application-debug-mode"></a><span data-ttu-id="93bd7-116">Programmet felsökningsläge</span><span class="sxs-lookup"><span data-stu-id="93bd7-116">Application Debug Mode</span></span>
<span data-ttu-id="93bd7-117">Visual Studio tillhandahåller en egenskap som kallas **programmet felsökningsläge**, som styr hur du vill att Visual Studios toohandle programdistribution som en del av felsökning.</span><span class="sxs-lookup"><span data-stu-id="93bd7-117">Visual Studio provide a property called **Application Debug Mode**, which controls how you want Visual Studios toohandle Application deployment as part of debugging.</span></span>

#### <a name="tooset-hello-application-debug-mode-property"></a><span data-ttu-id="93bd7-118">tooset hello programmet felsökningsläge egenskapen</span><span class="sxs-lookup"><span data-stu-id="93bd7-118">tooset hello Application Debug Mode property</span></span>
1. <span data-ttu-id="93bd7-119">På hello Service Fabric application projektets (*.sfproj) snabbmenyn väljer **egenskaper** (eller tryck på hello **F4** nyckel).</span><span class="sxs-lookup"><span data-stu-id="93bd7-119">On hello Service Fabric application project's (*.sfproj) shortcut menu, choose **Properties** (or press hello **F4** key).</span></span>
2. <span data-ttu-id="93bd7-120">I hello **egenskaper** fönster, ange hello **programmet felsökningsläge** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="93bd7-120">In hello **Properties** window, set hello **Application Debug Mode** property.</span></span>

![Egenskapen programmet Debug-läge][debugmodeproperty]

#### <a name="application-debug-modes"></a><span data-ttu-id="93bd7-122">Programmet felsökningslägen</span><span class="sxs-lookup"><span data-stu-id="93bd7-122">Application Debug Modes</span></span>

1. <span data-ttu-id="93bd7-123">**Uppdatera program** detta läge kan du ändra tooquickly och felsöka din kod och stöd för redigering av statiska filer när du felsöker.</span><span class="sxs-lookup"><span data-stu-id="93bd7-123">**Refresh Application** This mode enables you tooquickly change and debug your code and supports editing static web files while debugging.</span></span> <span data-ttu-id="93bd7-124">Det här läget fungerar bara om lokal utveckling klustret är i [1 nod läge](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).</span><span class="sxs-lookup"><span data-stu-id="93bd7-124">This mode only works if your local development cluster is in [1-Node mode](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).</span></span>
2. <span data-ttu-id="93bd7-125">**Ta bort programmet** orsaker hello programmet toobe tas bort när hello debug-sessionen avslutas.</span><span class="sxs-lookup"><span data-stu-id="93bd7-125">**Remove Application** causes hello application toobe removed when hello debug session ends.</span></span>
3. <span data-ttu-id="93bd7-126">**Automatisk uppgradering** hello program fortsätter toorun när hello debug-sessionen avslutas.</span><span class="sxs-lookup"><span data-stu-id="93bd7-126">**Auto Upgrade** hello application continues toorun when hello debug session ends.</span></span> <span data-ttu-id="93bd7-127">hello behandlar nästa felsökningssessionen hello distribution som en uppgradering.</span><span class="sxs-lookup"><span data-stu-id="93bd7-127">hello next debug session will treat hello deployment as an upgrade.</span></span> <span data-ttu-id="93bd7-128">hello uppgraderingsprocessen bevarar alla data som du angav i föregående felsökningssessionen.</span><span class="sxs-lookup"><span data-stu-id="93bd7-128">hello upgrade process preserves any data that you entered in a previous debug session.</span></span>
4. <span data-ttu-id="93bd7-129">**Hålla program** hello programmet håller körs i hello klustret när hello debug konsolsessionen avslutas.</span><span class="sxs-lookup"><span data-stu-id="93bd7-129">**Keep Application** hello application keeps running in hello cluster when hello debug session ends.</span></span> <span data-ttu-id="93bd7-130">Hello början av hello nästa felsökningssessionen, tas hello programmet bort.</span><span class="sxs-lookup"><span data-stu-id="93bd7-130">At hello beginning of hello next debug session, hello application will be removed.</span></span>

<span data-ttu-id="93bd7-131">För **automatiskt uppgradera** bevaras data genom att använda hello-programfunktioner uppgradering av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="93bd7-131">For **Auto Upgrade** data is preserved by applying hello application upgrade capabilities of Service Fabric.</span></span> <span data-ttu-id="93bd7-132">Mer information om hur du uppgraderar program och hur du kan utföra en uppgradering i en verklig miljö finns [uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="93bd7-132">For more information about upgrading applications and how you might perform an upgrade in a real environment, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

## <a name="add-a-service-tooyour-service-fabric-application"></a><span data-ttu-id="93bd7-133">Lägg till en service tooyour Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="93bd7-133">Add a service tooyour Service Fabric application</span></span>
<span data-ttu-id="93bd7-134">Du kan lägga till nya tjänster tooyour programmet tooextend dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="93bd7-134">You can add new services tooyour application tooextend its functionality.</span></span>  <span data-ttu-id="93bd7-135">tooensure att hello service ingår i ditt programpaket, lägga till hello tjänsten via hello **nya Fabric-tjänsten...**  menyalternativ.</span><span class="sxs-lookup"><span data-stu-id="93bd7-135">tooensure that hello service is included in your application package, add hello service through hello **New Fabric Service...** menu item.</span></span>

![Lägg till en ny Service Fabric-tjänst][newservice]

<span data-ttu-id="93bd7-137">Välj ett Service Fabric-projekt typen tooadd tooyour program och ange ett namn för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="93bd7-137">Select a Service Fabric project type tooadd tooyour application, and specify a name for hello service.</span></span>  <span data-ttu-id="93bd7-138">Se [att välja ett ramverk för din tjänst](service-fabric-choose-framework.md) toohelp som du bestämmer dig för vilken tjänst skriver toouse.</span><span class="sxs-lookup"><span data-stu-id="93bd7-138">See [Choosing a framework for your service](service-fabric-choose-framework.md) toohelp you decide which service type toouse.</span></span>

![Välj ett Service Fabric-projektet typen tooadd tooyour tjänstprogram][addserviceproject]

<span data-ttu-id="93bd7-140">hello ny tjänst läggs tooyour lösningen och befintliga programpaket.</span><span class="sxs-lookup"><span data-stu-id="93bd7-140">hello new service is added tooyour solution and existing application package.</span></span> <span data-ttu-id="93bd7-141">referenser för hello och en standardinstans för tjänsten kommer att tillagda toohello programmanifestet, orsakar hello service toobe skapas och igång hello nästa gång du distribuerar hello program.</span><span class="sxs-lookup"><span data-stu-id="93bd7-141">hello service references and a default service instance will be added toohello application manifest, causing hello service toobe created and started hello next time you deploy hello application.</span></span>

![hello ny tjänst läggs tooyour programmanifestet][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a><span data-ttu-id="93bd7-143">Paketera Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="93bd7-143">Package your Service Fabric application</span></span>
<span data-ttu-id="93bd7-144">toodeploy hello programmet och dess tjänster tooa kluster, måste toocreate programpaket.</span><span class="sxs-lookup"><span data-stu-id="93bd7-144">toodeploy hello application and its services tooa cluster, you need toocreate an application package.</span></span>  <span data-ttu-id="93bd7-145">hello paketet organiserar hello programmanifestet service manifest och andra nödvändiga filer i en viss layout.</span><span class="sxs-lookup"><span data-stu-id="93bd7-145">hello package organizes hello application manifest, service manifests, and other necessary files in a specific layout.</span></span>  <span data-ttu-id="93bd7-146">Visual Studio konfigurerar och hanterar hello-paket i projektet hello-program-mappen i hello-pkg-katalogen.</span><span class="sxs-lookup"><span data-stu-id="93bd7-146">Visual Studio sets up and manages hello package in hello application project's folder, in hello 'pkg' directory.</span></span>  <span data-ttu-id="93bd7-147">Klicka på **paketet** från hello **programmet** snabbmenyn skapar eller uppdateringar hello programpaket.</span><span class="sxs-lookup"><span data-stu-id="93bd7-147">Clicking **Package** from hello **Application** context menu creates or updates hello application package.</span></span>

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a><span data-ttu-id="93bd7-148">Ta bort program och programtyper med Cloud Explorer</span><span class="sxs-lookup"><span data-stu-id="93bd7-148">Remove applications and application types using Cloud Explorer</span></span>
<span data-ttu-id="93bd7-149">Du kan utföra grundläggande klusterhanteringsåtgärder inifrån Visual Studio med Cloud Explorer som du kan starta från hello **visa** menyn.</span><span class="sxs-lookup"><span data-stu-id="93bd7-149">You can perform basic cluster management operations from within Visual Studio using Cloud Explorer, which you can launch from hello **View** menu.</span></span> <span data-ttu-id="93bd7-150">Du kan till exempel ta bort program och avetablera programtyper på lokala eller fjärranslutna kluster.</span><span class="sxs-lookup"><span data-stu-id="93bd7-150">For instance, you can delete applications and unprovision application types on local or remote clusters.</span></span>

![Ta bort ett program][removeapplication]

> [!TIP]
> <span data-ttu-id="93bd7-152">Rikare hanteringsfunktioner för klustret, se [visualisera ditt kluster med Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="93bd7-152">For richer cluster management functionality, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
>
>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="93bd7-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93bd7-153">Next steps</span></span>
* [<span data-ttu-id="93bd7-154">Service Fabric programmodell</span><span class="sxs-lookup"><span data-stu-id="93bd7-154">Service Fabric application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="93bd7-155">Distribution av Service Fabric</span><span class="sxs-lookup"><span data-stu-id="93bd7-155">Service Fabric application deployment</span></span>](service-fabric-deploy-remove-applications.md)
* [<span data-ttu-id="93bd7-156">Hantera programparametrar för miljöer med flera</span><span class="sxs-lookup"><span data-stu-id="93bd7-156">Managing application parameters for multiple environments</span></span>](service-fabric-manage-multiple-environment-app-configuration.md)
* [<span data-ttu-id="93bd7-157">Felsöka ditt Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="93bd7-157">Debugging your Service Fabric application</span></span>](service-fabric-debugging-your-application.md)
* [<span data-ttu-id="93bd7-158">Visualisera ditt kluster med hjälp av Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="93bd7-158">Visualizing your cluster by using Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png