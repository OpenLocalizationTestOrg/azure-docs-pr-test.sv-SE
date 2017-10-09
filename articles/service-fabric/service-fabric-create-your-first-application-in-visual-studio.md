---
title: "aaaCreate en tillförlitlig Azure Service Fabric-tjänsten med C#"
description: "Skapa, distribuera och felsöka ett tillförlitligt tjänstprogram som bygger på Azure Service Fabric med Visual Studio."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2017
ms.author: ryanwi
ms.openlocfilehash: 740c866da6e639219b529fe92ed63cbeaa702a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a><span data-ttu-id="b1d80-103">Skapa ditt första tillståndskänsliga tillförlitliga C# Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="b1d80-103">Create your first C# Service Fabric stateful reliable services application</span></span>

<span data-ttu-id="b1d80-104">Lär dig hur toodeploy ditt första Service Fabric-program för .NET i Windows på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="b1d80-104">Learn how toodeploy your first Service Fabric application for .NET on Windows in just a few minutes.</span></span> <span data-ttu-id="b1d80-105">När du är klar har du ett lokala kluster som körs med ett tillförlitligt tjänstprogram.</span><span class="sxs-lookup"><span data-stu-id="b1d80-105">When you're finished, you'll have a local cluster running with a reliable service application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1d80-106">Krav</span><span class="sxs-lookup"><span data-stu-id="b1d80-106">Prerequisites</span></span>

<span data-ttu-id="b1d80-107">Du måste [konfigurera utvecklingsmiljön](service-fabric-get-started.md) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="b1d80-107">Before you get started, make sure that you have [set up your development environment](service-fabric-get-started.md).</span></span> <span data-ttu-id="b1d80-108">Detta omfattar att installera hello Service Fabric-SDK och Visual Studio 2017 eller 2015.</span><span class="sxs-lookup"><span data-stu-id="b1d80-108">This includes installing hello Service Fabric SDK and Visual Studio 2017 or 2015.</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="b1d80-109">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="b1d80-109">Create hello application</span></span>

<span data-ttu-id="b1d80-110">Starta Visual Studio som **administratör**.</span><span class="sxs-lookup"><span data-stu-id="b1d80-110">Launch Visual Studio as an **administrator**.</span></span>

<span data-ttu-id="b1d80-111">Skapa ett projekt med `CTRL`+`SHIFT`+`N`</span><span class="sxs-lookup"><span data-stu-id="b1d80-111">Create a project with `CTRL`+`SHIFT`+`N`</span></span>

<span data-ttu-id="b1d80-112">I hello **nytt projekt** dialogrutan Välj **molntjänster > Fabric tjänstprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="b1d80-112">In hello **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

<span data-ttu-id="b1d80-113">Namnge programmet hello **MyApplication** och tryck på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1d80-113">Name hello application **MyApplication** and press **OK**.</span></span>

   
![Dialogrutan Nytt projekt i Visual Studio][1]

<span data-ttu-id="b1d80-115">Du kan skapa Service Fabric-program från hello nästa dialogruta.</span><span class="sxs-lookup"><span data-stu-id="b1d80-115">You can create any type of Service Fabric application from hello next dialog.</span></span> <span data-ttu-id="b1d80-116">För den här snabbstarten, väljer du **Tillståndskänslig tjänst**.</span><span class="sxs-lookup"><span data-stu-id="b1d80-116">For this Quickstart, choose **Stateful Service**.</span></span>

<span data-ttu-id="b1d80-117">Namnge hello service **MyStatefulService** och tryck på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1d80-117">Name hello service **MyStatefulService** and press **OK**.</span></span>

![Dialogrutan Ny tjänst i Visual Studio][2]


<span data-ttu-id="b1d80-119">Visual Studio skapar hello programmet projektet och hello tillståndskänslig service och visar dem i Solution Explorer.</span><span class="sxs-lookup"><span data-stu-id="b1d80-119">Visual Studio creates hello application project and hello stateful service project and displays them in Solution Explorer.</span></span>

![Solution Explorer när ett program med en tillståndskänslig tjänst har skapats][3]

<span data-ttu-id="b1d80-121">hello projektet (**MyApplication**) innehåller inte någon kod direkt.</span><span class="sxs-lookup"><span data-stu-id="b1d80-121">hello application project (**MyApplication**) does not contain any code directly.</span></span> <span data-ttu-id="b1d80-122">I stället refererar det till en uppsättning tjänstprojekt.</span><span class="sxs-lookup"><span data-stu-id="b1d80-122">Instead, it references a set of service projects.</span></span> <span data-ttu-id="b1d80-123">Dessutom innehåller det tre andra typer av innehåll:</span><span class="sxs-lookup"><span data-stu-id="b1d80-123">In addition, it contains three other types of content:</span></span>

* <span data-ttu-id="b1d80-124">**Publicera profiler**</span><span class="sxs-lookup"><span data-stu-id="b1d80-124">**Publish profiles**</span></span>  
<span data-ttu-id="b1d80-125">Profiler för att distribuera toodifferent miljöer.</span><span class="sxs-lookup"><span data-stu-id="b1d80-125">Profiles for deploying toodifferent environments.</span></span>

* <span data-ttu-id="b1d80-126">**Skript**</span><span class="sxs-lookup"><span data-stu-id="b1d80-126">**Scripts**</span></span>  
<span data-ttu-id="b1d80-127">PowerShell-skript för distribution/uppgradering av program.</span><span class="sxs-lookup"><span data-stu-id="b1d80-127">PowerShell script for deploying/upgrading your application.</span></span>

* <span data-ttu-id="b1d80-128">**Programdefinition**</span><span class="sxs-lookup"><span data-stu-id="b1d80-128">**Application definition**</span></span>  
<span data-ttu-id="b1d80-129">Innehåller hello ApplicationManifest.xml fil under *ApplicationPackageRoot* som beskriver ditt programs sammansättning.</span><span class="sxs-lookup"><span data-stu-id="b1d80-129">Includes hello ApplicationManifest.xml file under *ApplicationPackageRoot* which describes your application's composition.</span></span> <span data-ttu-id="b1d80-130">Associerade programfiler för parametern är *ApplicationParameters*, vilket kan vara används toospecify miljö-specifika parametrar.</span><span class="sxs-lookup"><span data-stu-id="b1d80-130">Associated application parameter files are under *ApplicationParameters*, which can be used toospecify environment-specific parameters.</span></span> <span data-ttu-id="b1d80-131">Visual Studio väljer en programfil parameter som anges i hello associerade publiceringsprofil under distributionen tooa specifika miljö.</span><span class="sxs-lookup"><span data-stu-id="b1d80-131">Visual Studio selects an application parameter file that's specified in hello associated publish profile during deployment tooa specific environment.</span></span>
    
<span data-ttu-id="b1d80-132">En översikt över hello innehållet i hello service-projekt, se [komma igång med Reliable Services](service-fabric-reliable-services-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="b1d80-132">For an overview of hello contents of hello service project, see [Getting started with Reliable Services](service-fabric-reliable-services-quick-start.md).</span></span>

## <a name="deploy-and-debug-hello-application"></a><span data-ttu-id="b1d80-133">Distribuera och felsöka hello program</span><span class="sxs-lookup"><span data-stu-id="b1d80-133">Deploy and debug hello application</span></span>

<span data-ttu-id="b1d80-134">Nu när du har ett program kan du prova att köra det.</span><span class="sxs-lookup"><span data-stu-id="b1d80-134">Now that you have an application, run it.</span></span>

<span data-ttu-id="b1d80-135">I Visual Studio trycker du på `F5` toodeploy hello program för felsökning.</span><span class="sxs-lookup"><span data-stu-id="b1d80-135">In Visual Studio, press `F5` toodeploy hello application for debugging.</span></span>

>[!NOTE]
><span data-ttu-id="b1d80-136">hello skapar första gången du kör och distribuera hello programmet lokalt, Visual Studio ett lokala kluster för felsökning.</span><span class="sxs-lookup"><span data-stu-id="b1d80-136">hello first time you run and deploy hello application locally, Visual Studio creates a local cluster for debugging.</span></span> <span data-ttu-id="b1d80-137">Det kan ta lite tid.</span><span class="sxs-lookup"><span data-stu-id="b1d80-137">This may take some time.</span></span> <span data-ttu-id="b1d80-138">hello klustret Skapandestatus visas i utdatafönstret för hello Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b1d80-138">hello cluster creation status is displayed in hello Visual Studio output window.</span></span>

<span data-ttu-id="b1d80-139">När hello klustret är klar, kan du få ett meddelande från hello lokala klustret fack manager systemprogram medföljer hello SDK.</span><span class="sxs-lookup"><span data-stu-id="b1d80-139">When hello cluster is ready, you get a notification from hello local cluster system tray manager application included with hello SDK.</span></span>
   
![Meddelande från Local Cluster Manager i systemfältet][4]

<span data-ttu-id="b1d80-141">En gång hello programmet startar Visual Studio automatiskt öppnar hello **diagnostik Loggboken**, där du kan se spårningsutdata från dina tjänster.</span><span class="sxs-lookup"><span data-stu-id="b1d80-141">Once hello application starts, Visual Studio automatically brings up hello **Diagnostics Event Viewer**, where you can see trace output from your services.</span></span>
   
![Loggboken Diagnostik][5]

<span data-ttu-id="b1d80-143">hello tillståndskänslig tjänstmall som vi använde visas endast en räknare värde ökar i hello `RunAsync` metod för **MyStatefulService.cs**.</span><span class="sxs-lookup"><span data-stu-id="b1d80-143">hello stateful service template we used simply shows a counter value incrementing in hello `RunAsync` method of **MyStatefulService.cs**.</span></span>

<span data-ttu-id="b1d80-144">Expandera en hello händelser toosee mer information, inklusive hello-nod där hello kod körs.</span><span class="sxs-lookup"><span data-stu-id="b1d80-144">Expand one of hello events toosee more details, including hello node where hello code is running.</span></span> <span data-ttu-id="b1d80-145">I det här fallet är det \_Node\_2, men det kan skilja sig på din dator.</span><span class="sxs-lookup"><span data-stu-id="b1d80-145">In this case, it is \_Node\_2, though it may differ on your machine.</span></span>
   
![Detaljer från loggboken Diagnostik][6]

<span data-ttu-id="b1d80-147">hello lokala kluster innehåller fem noder som finns på en enskild dator.</span><span class="sxs-lookup"><span data-stu-id="b1d80-147">hello local cluster contains five nodes hosted on a single machine.</span></span> <span data-ttu-id="b1d80-148">Varje nod finns på en distinkt fysisk eller virtuell dator i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b1d80-148">In a production environment, each node is hosted on a distinct physical or virtual machine.</span></span> <span data-ttu-id="b1d80-149">toosimulate hello förlust av en dator när utöva hello Visual Studio för felsökning på hello samma tid, ska vi ta bort en av noderna hello på hello lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="b1d80-149">toosimulate hello loss of a machine while exercising hello Visual Studio debugger at hello same time, let's take down one of hello nodes on hello local cluster.</span></span>

<span data-ttu-id="b1d80-150">I hello **Solution Explorer** fönster, **MyStatefulService.cs**.</span><span class="sxs-lookup"><span data-stu-id="b1d80-150">In hello **Solution Explorer** window, open **MyStatefulService.cs**.</span></span> 

<span data-ttu-id="b1d80-151">Hitta hello `RunAsync` metod och ange en brytpunkt på hello första raden i hello-metoden.</span><span class="sxs-lookup"><span data-stu-id="b1d80-151">Find hello `RunAsync` method and set a breakpoint on hello first line of hello method.</span></span>

![Brytpunkt i den tillståndskänsliga tjänstens RunAsync-metod][7]

<span data-ttu-id="b1d80-153">Starta hello **Service Fabric Explorer** verktyget genom att högerklicka på hello **lokala Klusterhanterare** fack systemprogram och välj **hantera lokala klustret**.</span><span class="sxs-lookup"><span data-stu-id="b1d80-153">Launch hello **Service Fabric Explorer** tool by right-clicking on hello **Local Cluster Manager** system tray application and choose **Manage Local Cluster**.</span></span>

![Starta Service Fabric Explorer från hello Klusterhanterare för lokala][systray-launch-sfx]

<span data-ttu-id="b1d80-155">[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) visar en bild av ett kluster.</span><span class="sxs-lookup"><span data-stu-id="b1d80-155">[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) offers a visual representation of a cluster.</span></span> <span data-ttu-id="b1d80-156">Det innehåller hello uppsättning program som distribueras tooit och hello uppsättning fysiska noder som utgör den.</span><span class="sxs-lookup"><span data-stu-id="b1d80-156">It includes hello set of applications deployed tooit and hello set of physical nodes that make it up.</span></span>

<span data-ttu-id="b1d80-157">I hello till vänster och expanderar **kluster > noder** och hitta hello nod där din kod körs.</span><span class="sxs-lookup"><span data-stu-id="b1d80-157">In hello left pane, expand **Cluster > Nodes** and find hello node where your code is running.</span></span>

<span data-ttu-id="b1d80-158">Klicka på **åtgärder > Inaktivera (omstart)** toosimulate en datorn startas om.</span><span class="sxs-lookup"><span data-stu-id="b1d80-158">Click **Actions > Deactivate (Restart)** toosimulate a machine restarting.</span></span>

![Stoppa en nod i Service Fabric Explorer][sfx-stop-node]

<span data-ttu-id="b1d80-160">Tillfälligt, bör du se din brytpunkt träffar i Visual Studio som hello beräkning som du gjorde på en nod sömlöst växlar tooanother.</span><span class="sxs-lookup"><span data-stu-id="b1d80-160">Momentarily, you should see your breakpoint hit in Visual Studio as hello computation you were doing on one node seamlessly fails over tooanother.</span></span>


<span data-ttu-id="b1d80-161">Sedan returnera toohello diagnostiska Loggboken och se hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="b1d80-161">Next, return toohello Diagnostic Events Viewer and observe hello messages.</span></span> <span data-ttu-id="b1d80-162">hello räknare har blivit ökar, även om hello händelser faktiskt kommer från en annan nod.</span><span class="sxs-lookup"><span data-stu-id="b1d80-162">hello counter has continued incrementing, even though hello events are actually coming from a different node.</span></span>

![Loggboken Diagnostik efter en redundansväxling][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-hello-local-cluster-optional"></a><span data-ttu-id="b1d80-164">Rensa hello lokala klustret (valfritt)</span><span class="sxs-lookup"><span data-stu-id="b1d80-164">Cleaning up hello local cluster (optional)</span></span>

<span data-ttu-id="b1d80-165">Kom ihåg att det här lokala klustret är verkligt.</span><span class="sxs-lookup"><span data-stu-id="b1d80-165">Remember, this local cluster is real.</span></span> <span data-ttu-id="b1d80-166">Stoppa hello felsökare tar bort din programinstansen och Avregistrerar hello programtyp.</span><span class="sxs-lookup"><span data-stu-id="b1d80-166">Stopping hello debugger removes your application instance and unregisters hello application type.</span></span> <span data-ttu-id="b1d80-167">Hello klustret fortsätter dock toorun i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="b1d80-167">However, hello cluster continues toorun in hello background.</span></span> <span data-ttu-id="b1d80-168">När du är klar toostop hello lokala klustret, finns det några alternativ.</span><span class="sxs-lookup"><span data-stu-id="b1d80-168">When you're ready toostop hello local cluster, there are a couple options.</span></span>

### <a name="keep-application-and-trace-data"></a><span data-ttu-id="b1d80-169">Bibehåll program- och spårningsdata</span><span class="sxs-lookup"><span data-stu-id="b1d80-169">Keep application and trace data</span></span>

<span data-ttu-id="b1d80-170">Stäng hello klustret genom att högerklicka på hello **lokala Klusterhanterare** fack systemprogram och välj sedan **stoppa lokala klustret**.</span><span class="sxs-lookup"><span data-stu-id="b1d80-170">Shut down hello cluster by right-clicking on hello **Local Cluster Manager** system tray application and then choose **Stop Local Cluster**.</span></span>

### <a name="delete-hello-cluster-and-all-data"></a><span data-ttu-id="b1d80-171">Ta bort hello kluster och alla data</span><span class="sxs-lookup"><span data-stu-id="b1d80-171">Delete hello cluster and all data</span></span>

<span data-ttu-id="b1d80-172">Ta bort hello klustret genom att högerklicka på hello **lokala Klusterhanterare** fack systemprogram och välj sedan **ta bort lokala klustret**.</span><span class="sxs-lookup"><span data-stu-id="b1d80-172">Remove hello cluster by right-clicking on hello **Local Cluster Manager** system tray application and then choose **Remove Local Cluster**.</span></span> 

<span data-ttu-id="b1d80-173">Om du väljer det här alternativet, kommer Visual Studio omdistribuera hello klustret hello nästa gång som din kör hello program.</span><span class="sxs-lookup"><span data-stu-id="b1d80-173">If you choose this option, Visual Studio will redeploy hello cluster hello next time your run hello application.</span></span> <span data-ttu-id="b1d80-174">Välj det här alternativet om du inte avser toouse hello lokala kluster under en viss tid eller om du behöver tooreclaim resurser.</span><span class="sxs-lookup"><span data-stu-id="b1d80-174">Choose this option if you don't intend toouse hello local cluster for some time or if you need tooreclaim resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b1d80-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b1d80-175">Next steps</span></span>
<span data-ttu-id="b1d80-176">Mer information om [Reliable Services](service-fabric-reliable-services-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b1d80-176">Read more about [reliable services](service-fabric-reliable-services-introduction.md).</span></span>
<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
