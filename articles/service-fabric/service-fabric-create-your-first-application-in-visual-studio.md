---
title: "Skapa en tillförlitlig tjänst för Azure Service Fabric med C#"
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
ms.openlocfilehash: f93298e6483fd8c9dfda835964aeebd1a430af69
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a><span data-ttu-id="b261a-103">Skapa ditt första tillståndskänsliga tillförlitliga C# Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="b261a-103">Create your first C# Service Fabric stateful reliable services application</span></span>

<span data-ttu-id="b261a-104">Lär dig hur du distribuerar ditt första Service Fabric-program för .NET i Windows på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="b261a-104">Learn how to deploy your first Service Fabric application for .NET on Windows in just a few minutes.</span></span> <span data-ttu-id="b261a-105">När du är klar har du ett lokala kluster som körs med ett tillförlitligt tjänstprogram.</span><span class="sxs-lookup"><span data-stu-id="b261a-105">When you're finished, you'll have a local cluster running with a reliable service application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b261a-106">Krav</span><span class="sxs-lookup"><span data-stu-id="b261a-106">Prerequisites</span></span>

<span data-ttu-id="b261a-107">Du måste [konfigurera utvecklingsmiljön](service-fabric-get-started.md) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="b261a-107">Before you get started, make sure that you have [set up your development environment](service-fabric-get-started.md).</span></span> <span data-ttu-id="b261a-108">Detta innefattar hur du installerar Service Fabric-SDK och Visual Studio 2017 eller 2015.</span><span class="sxs-lookup"><span data-stu-id="b261a-108">This includes installing the Service Fabric SDK and Visual Studio 2017 or 2015.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="b261a-109">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="b261a-109">Create the application</span></span>

<span data-ttu-id="b261a-110">Starta Visual Studio som **administratör**.</span><span class="sxs-lookup"><span data-stu-id="b261a-110">Launch Visual Studio as an **administrator**.</span></span>

<span data-ttu-id="b261a-111">Skapa ett projekt med `CTRL`+`SHIFT`+`N`</span><span class="sxs-lookup"><span data-stu-id="b261a-111">Create a project with `CTRL`+`SHIFT`+`N`</span></span>

<span data-ttu-id="b261a-112">Klicka på **Moln > Service Fabric-program** i dialogrutan **Nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="b261a-112">In the **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

<span data-ttu-id="b261a-113">Ge programmet namnet **MyApplication** och tryck på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b261a-113">Name the application **MyApplication** and press **OK**.</span></span>

   
![Dialogrutan Nytt projekt i Visual Studio][1]

<span data-ttu-id="b261a-115">Du kan skapa alla typer av Service Fabric-program från nästa dialogruta.</span><span class="sxs-lookup"><span data-stu-id="b261a-115">You can create any type of Service Fabric application from the next dialog.</span></span> <span data-ttu-id="b261a-116">För den här snabbstarten, väljer du **Tillståndskänslig tjänst**.</span><span class="sxs-lookup"><span data-stu-id="b261a-116">For this Quickstart, choose **Stateful Service**.</span></span>

<span data-ttu-id="b261a-117">Namnge tjänsten **MyStatefulService** och tryck på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b261a-117">Name the service **MyStatefulService** and press **OK**.</span></span>

![Dialogrutan Ny tjänst i Visual Studio][2]


<span data-ttu-id="b261a-119">Visual Studio skapar programprojektet och det tillståndskänsliga tjänstprojektet och visar dem i Solution Explorer.</span><span class="sxs-lookup"><span data-stu-id="b261a-119">Visual Studio creates the application project and the stateful service project and displays them in Solution Explorer.</span></span>

![Solution Explorer när ett program med en tillståndskänslig tjänst har skapats][3]

<span data-ttu-id="b261a-121">Programprojektet (**MyApplication**) innehåller ingen direkt kod.</span><span class="sxs-lookup"><span data-stu-id="b261a-121">The application project (**MyApplication**) does not contain any code directly.</span></span> <span data-ttu-id="b261a-122">I stället refererar det till en uppsättning tjänstprojekt.</span><span class="sxs-lookup"><span data-stu-id="b261a-122">Instead, it references a set of service projects.</span></span> <span data-ttu-id="b261a-123">Dessutom innehåller det tre andra typer av innehåll:</span><span class="sxs-lookup"><span data-stu-id="b261a-123">In addition, it contains three other types of content:</span></span>

* <span data-ttu-id="b261a-124">**Publicera profiler**</span><span class="sxs-lookup"><span data-stu-id="b261a-124">**Publish profiles**</span></span>  
<span data-ttu-id="b261a-125">Profiler för att distribuera till olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="b261a-125">Profiles for deploying to different environments.</span></span>

* <span data-ttu-id="b261a-126">**Skript**</span><span class="sxs-lookup"><span data-stu-id="b261a-126">**Scripts**</span></span>  
<span data-ttu-id="b261a-127">PowerShell-skript för distribution/uppgradering av program.</span><span class="sxs-lookup"><span data-stu-id="b261a-127">PowerShell script for deploying/upgrading your application.</span></span>

* <span data-ttu-id="b261a-128">**Programdefinition**</span><span class="sxs-lookup"><span data-stu-id="b261a-128">**Application definition**</span></span>  
<span data-ttu-id="b261a-129">Innehåller filen ApplicationManifest.xml under *ApplicationPackageRoot* som beskriver ditt programs sammansättning.</span><span class="sxs-lookup"><span data-stu-id="b261a-129">Includes the ApplicationManifest.xml file under *ApplicationPackageRoot* which describes your application's composition.</span></span> <span data-ttu-id="b261a-130">Associerade programfiler för parametern är *ApplicationParameters*, som kan användas för att ange miljöspecifikt parametrar.</span><span class="sxs-lookup"><span data-stu-id="b261a-130">Associated application parameter files are under *ApplicationParameters*, which can be used to specify environment-specific parameters.</span></span> <span data-ttu-id="b261a-131">Visual Studio väljer en programparameterfil som anges i den tillhörande publikationsprofilen vid distribution till en specifik miljö.</span><span class="sxs-lookup"><span data-stu-id="b261a-131">Visual Studio selects an application parameter file that's specified in the associated publish profile during deployment to a specific environment.</span></span>
    
<span data-ttu-id="b261a-132">En översikt över innehållet i tjänstprojektet finns i [Komma igång med Reliable Services](service-fabric-reliable-services-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="b261a-132">For an overview of the contents of the service project, see [Getting started with Reliable Services](service-fabric-reliable-services-quick-start.md).</span></span>

## <a name="deploy-and-debug-the-application"></a><span data-ttu-id="b261a-133">Distribuera och felsöka programmet</span><span class="sxs-lookup"><span data-stu-id="b261a-133">Deploy and debug the application</span></span>

<span data-ttu-id="b261a-134">Nu när du har ett program kan du prova att köra det.</span><span class="sxs-lookup"><span data-stu-id="b261a-134">Now that you have an application, run it.</span></span>

<span data-ttu-id="b261a-135">Tryck på `F5` i Visual Studio för att distribuera programmet för felsökning.</span><span class="sxs-lookup"><span data-stu-id="b261a-135">In Visual Studio, press `F5` to deploy the application for debugging.</span></span>

>[!NOTE]
><span data-ttu-id="b261a-136">Första gången du kör och distribuerar programmet lokalt, skapar Visual Studio ett lokalt kluster för felsökning.</span><span class="sxs-lookup"><span data-stu-id="b261a-136">The first time you run and deploy the application locally, Visual Studio creates a local cluster for debugging.</span></span> <span data-ttu-id="b261a-137">Det kan ta lite tid.</span><span class="sxs-lookup"><span data-stu-id="b261a-137">This may take some time.</span></span> <span data-ttu-id="b261a-138">Statusen för klustergenereringen visas i utdatafönstret i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b261a-138">The cluster creation status is displayed in the Visual Studio output window.</span></span>

<span data-ttu-id="b261a-139">När klustret är klart får du ett meddelande från programmet Local Cluster Manager i systemfältet som ingår i SDK.</span><span class="sxs-lookup"><span data-stu-id="b261a-139">When the cluster is ready, you get a notification from the local cluster system tray manager application included with the SDK.</span></span>
   
![Meddelande från Local Cluster Manager i systemfältet][4]

<span data-ttu-id="b261a-141">När programmet startar visas loggboken **Diagnostik automatiskt**, där du kan se spårningsinformation från tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b261a-141">Once the application starts, Visual Studio automatically brings up the **Diagnostics Event Viewer**, where you can see trace output from your services.</span></span>
   
![Loggboken Diagnostik][5]

<span data-ttu-id="b261a-143">Om du använder mallen för tillståndskänsliga tjänster visar meddelandena bara antalet som ökar i metoden `RunAsync` i **MyStatefulService.cs**.</span><span class="sxs-lookup"><span data-stu-id="b261a-143">The stateful service template we used simply shows a counter value incrementing in the `RunAsync` method of **MyStatefulService.cs**.</span></span>

<span data-ttu-id="b261a-144">Expandera någon av händelserna om du vill visa mer information, inklusive noden där koden körs.</span><span class="sxs-lookup"><span data-stu-id="b261a-144">Expand one of the events to see more details, including the node where the code is running.</span></span> <span data-ttu-id="b261a-145">I det här fallet är det \_Node\_2, men det kan skilja sig på din dator.</span><span class="sxs-lookup"><span data-stu-id="b261a-145">In this case, it is \_Node\_2, though it may differ on your machine.</span></span>
   
![Detaljer från loggboken Diagnostik][6]

<span data-ttu-id="b261a-147">Det lokala klustret innehåller fem noder som finns på en enda dator.</span><span class="sxs-lookup"><span data-stu-id="b261a-147">The local cluster contains five nodes hosted on a single machine.</span></span> <span data-ttu-id="b261a-148">Varje nod finns på en distinkt fysisk eller virtuell dator i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b261a-148">In a production environment, each node is hosted on a distinct physical or virtual machine.</span></span> <span data-ttu-id="b261a-149">Nu ska vi ta bort en av noderna i det lokala klustret för att simulera förlusten av en dator och passa på att använda Visual Studio-felsökaren.</span><span class="sxs-lookup"><span data-stu-id="b261a-149">To simulate the loss of a machine while exercising the Visual Studio debugger at the same time, let's take down one of the nodes on the local cluster.</span></span>

<span data-ttu-id="b261a-150">I fönster **Solution Explorer** öppnar du **MyStatefulService.cs**.</span><span class="sxs-lookup"><span data-stu-id="b261a-150">In the **Solution Explorer** window, open **MyStatefulService.cs**.</span></span> 

<span data-ttu-id="b261a-151">Hitta `RunAsync`-metoden och ange en brytpunkt på den första raden i metoden.</span><span class="sxs-lookup"><span data-stu-id="b261a-151">Find the `RunAsync` method and set a breakpoint on the first line of the method.</span></span>

![Brytpunkt i den tillståndskänsliga tjänstens RunAsync-metod][7]

<span data-ttu-id="b261a-153">Högerklicka på **Local Cluster Manager** i systemfältet och välj **Hantera lokalt kluster** för att starta verktyget **Service Fabric Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b261a-153">Launch the **Service Fabric Explorer** tool by right-clicking on the **Local Cluster Manager** system tray application and choose **Manage Local Cluster**.</span></span>

![Starta Service Fabric Explorer från Local Cluster Manager][systray-launch-sfx]

<span data-ttu-id="b261a-155">[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) visar en bild av ett kluster.</span><span class="sxs-lookup"><span data-stu-id="b261a-155">[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) offers a visual representation of a cluster.</span></span> <span data-ttu-id="b261a-156">Den innehåller en uppsättning program som distribueras till den och en uppsättning fysiska noder den består av.</span><span class="sxs-lookup"><span data-stu-id="b261a-156">It includes the set of applications deployed to it and the set of physical nodes that make it up.</span></span>

<span data-ttu-id="b261a-157">I den vänstra rutan expanderar du **Kluster > Noder** och letar upp noden där koden körs.</span><span class="sxs-lookup"><span data-stu-id="b261a-157">In the left pane, expand **Cluster > Nodes** and find the node where your code is running.</span></span>

<span data-ttu-id="b261a-158">Klicka på **Åtgärder > Inaktivera (omstart)** för att simulera omstarten av en dator.</span><span class="sxs-lookup"><span data-stu-id="b261a-158">Click **Actions > Deactivate (Restart)** to simulate a machine restarting.</span></span>

![Stoppa en nod i Service Fabric Explorer][sfx-stop-node]

<span data-ttu-id="b261a-160">Under ett ögonblick kan du se din brytpunkt i Visual Studio när beräkningen som du gjorde på en nod smidigt växlar över till en annan.</span><span class="sxs-lookup"><span data-stu-id="b261a-160">Momentarily, you should see your breakpoint hit in Visual Studio as the computation you were doing on one node seamlessly fails over to another.</span></span>


<span data-ttu-id="b261a-161">Gå sedan tillbaka till loggboken Diagnostik och notera meddelandena.</span><span class="sxs-lookup"><span data-stu-id="b261a-161">Next, return to the Diagnostic Events Viewer and observe the messages.</span></span> <span data-ttu-id="b261a-162">Räknaren har fortsatt att öka, även om händelserna egentligen kommer från en annan nod.</span><span class="sxs-lookup"><span data-stu-id="b261a-162">The counter has continued incrementing, even though the events are actually coming from a different node.</span></span>

![Loggboken Diagnostik efter en redundansväxling][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-the-local-cluster-optional"></a><span data-ttu-id="b261a-164">Rensa det lokala klustret (valfritt)</span><span class="sxs-lookup"><span data-stu-id="b261a-164">Cleaning up the local cluster (optional)</span></span>

<span data-ttu-id="b261a-165">Kom ihåg att det här lokala klustret är verkligt.</span><span class="sxs-lookup"><span data-stu-id="b261a-165">Remember, this local cluster is real.</span></span> <span data-ttu-id="b261a-166">Om du stoppar felsökningen tar du bort din instans av programmet och avregistrerar programtypen.</span><span class="sxs-lookup"><span data-stu-id="b261a-166">Stopping the debugger removes your application instance and unregisters the application type.</span></span> <span data-ttu-id="b261a-167">Klustret fortsätter dock att köras i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="b261a-167">However, the cluster continues to run in the background.</span></span> <span data-ttu-id="b261a-168">När du är redo att stoppa det lokala klustret finns det några alternativ.</span><span class="sxs-lookup"><span data-stu-id="b261a-168">When you're ready to stop the local cluster, there are a couple options.</span></span>

### <a name="keep-application-and-trace-data"></a><span data-ttu-id="b261a-169">Bibehåll program- och spårningsdata</span><span class="sxs-lookup"><span data-stu-id="b261a-169">Keep application and trace data</span></span>

<span data-ttu-id="b261a-170">Högerklicka på **Local Cluster Manager** i systemfältet och välj **Stäng lokalt kluster** för att stänga det lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="b261a-170">Shut down the cluster by right-clicking on the **Local Cluster Manager** system tray application and then choose **Stop Local Cluster**.</span></span>

### <a name="delete-the-cluster-and-all-data"></a><span data-ttu-id="b261a-171">Ta bort klustret och alla data</span><span class="sxs-lookup"><span data-stu-id="b261a-171">Delete the cluster and all data</span></span>

<span data-ttu-id="b261a-172">Högerklicka på **Local Cluster Manager** i systemfältet och välj **Ta bort lokalt kluster** för att ta bort det lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="b261a-172">Remove the cluster by right-clicking on the **Local Cluster Manager** system tray application and then choose **Remove Local Cluster**.</span></span> 

<span data-ttu-id="b261a-173">Om du väljer det här alternativet, distribuerar Visual Studio klustret nästa gång du kör programmet.</span><span class="sxs-lookup"><span data-stu-id="b261a-173">If you choose this option, Visual Studio will redeploy the cluster the next time your run the application.</span></span> <span data-ttu-id="b261a-174">Använd bara det här alternativet om du inte planerar att använda det lokala klustret under en tid eller om du behöver frigöra resurser.</span><span class="sxs-lookup"><span data-stu-id="b261a-174">Choose this option if you don't intend to use the local cluster for some time or if you need to reclaim resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b261a-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b261a-175">Next steps</span></span>
<span data-ttu-id="b261a-176">Mer information om [Reliable Services](service-fabric-reliable-services-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b261a-176">Read more about [reliable services](service-fabric-reliable-services-introduction.md).</span></span>
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
