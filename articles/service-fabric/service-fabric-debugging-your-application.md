---
title: "Felsöka ditt program i Visual Studio | Microsoft Docs"
description: "Förbättra tillförlitligheten och prestandan hos dina tjänster genom att utveckla och felsökning i Visual Studio dem på ett kluster för lokal utveckling."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: vturecek;mikhegn
ms.openlocfilehash: 2459025899a7f5ffebf44fa104ed112c0eb99dfa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a><span data-ttu-id="469f7-103">Felsöka Service Fabric-program med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="469f7-103">Debug your Service Fabric application by using Visual Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="469f7-104">Visual Studio/CSharp</span><span class="sxs-lookup"><span data-stu-id="469f7-104">Visual Studio/CSharp</span></span>](service-fabric-debugging-your-application.md) 
> * [<span data-ttu-id="469f7-105">Eclipse/Java</span><span class="sxs-lookup"><span data-stu-id="469f7-105">Eclipse/Java</span></span>](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a><span data-ttu-id="469f7-106">Felsöka ett lokala Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="469f7-106">Debug a local Service Fabric application</span></span>
<span data-ttu-id="469f7-107">Du kan spara tid och pengar genom att distribuera och felsöka ditt Azure Service Fabric-program i ett kluster för utveckling av lokal dator.</span><span class="sxs-lookup"><span data-stu-id="469f7-107">You can save time and money by deploying and debugging your Azure Service Fabric application in a local computer development cluster.</span></span> <span data-ttu-id="469f7-108">Visual Studio 2017 eller Visual Studio 2015 kan distribuera programmet till det lokala klustret och felsökningsprogrammet ansluta automatiskt till alla instanser av programmet.</span><span class="sxs-lookup"><span data-stu-id="469f7-108">Visual Studio 2017 or Visual Studio 2015 can deploy the application to the local cluster and automatically connect the debugger to all instances of your application.</span></span>

1. <span data-ttu-id="469f7-109">Starta en lokal utveckling klustret genom att följa stegen i [ställa in din utvecklingsmiljö för Service Fabric](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="469f7-109">Start a local development cluster by following the steps in [Setting up your Service Fabric development environment](service-fabric-get-started.md).</span></span>
2. <span data-ttu-id="469f7-110">Tryck på **F5** eller klicka på **felsöka** > **Start Debugging**.</span><span class="sxs-lookup"><span data-stu-id="469f7-110">Press **F5** or click **Debug** > **Start Debugging**.</span></span>
   
    ![Starta felsökning av ett program][startdebugging]
3. <span data-ttu-id="469f7-112">Ange brytpunkter i koden och stega igenom programmet genom att klicka på kommandon i den **felsöka** menyn.</span><span class="sxs-lookup"><span data-stu-id="469f7-112">Set breakpoints in your code and step through the application by clicking commands in the **Debug** menu.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="469f7-113">Visual Studio bifogar till alla instanser av programmet.</span><span class="sxs-lookup"><span data-stu-id="469f7-113">Visual Studio attaches to all instances of your application.</span></span> <span data-ttu-id="469f7-114">När du stega igenom koden hämta brytpunkter nådde av flera processer som resulterar i samtidiga sessioner.</span><span class="sxs-lookup"><span data-stu-id="469f7-114">While you're stepping through code, breakpoints may get hit by multiple processes resulting in concurrent sessions.</span></span> <span data-ttu-id="469f7-115">Prova att inaktivera brytpunkter när de träffar, genom att göra varje brytpunkt villkorlig på tråd-ID eller med hjälp av diagnostiska händelser.</span><span class="sxs-lookup"><span data-stu-id="469f7-115">Try disabling the breakpoints after they're hit, by making each breakpoint conditional on the thread ID or by using diagnostic events.</span></span>
   > 
   > 
4. <span data-ttu-id="469f7-116">Den **diagnostiska händelser** öppnas automatiskt så att du kan visa diagnostik händelser i realtid.</span><span class="sxs-lookup"><span data-stu-id="469f7-116">The **Diagnostic Events** window will automatically open so you can view diagnostic events in real time.</span></span>
   
    ![Visa diagnostik händelser i realtid][diagnosticevents]
5. <span data-ttu-id="469f7-118">Du kan också öppna den **diagnostiska händelser** fönster i Cloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="469f7-118">You can also open the **Diagnostic Events** window in Cloud Explorer.</span></span>  <span data-ttu-id="469f7-119">Under **Service Fabric**, högerklicka på en nod och välj **visa strömning spårningar**.</span><span class="sxs-lookup"><span data-stu-id="469f7-119">Under **Service Fabric**, right-click any node and choose **View Streaming Traces**.</span></span>
   
    ![Öppna fönstret diagnostiska händelser][viewdiagnosticevents]
   
    <span data-ttu-id="469f7-121">Om du vill filtrera dina spår till en specifik tjänst eller ett program kan bara aktivera strömmande spårningar på den specifika tjänsten eller programmet.</span><span class="sxs-lookup"><span data-stu-id="469f7-121">If you want to filter your traces to a specific service or application, simply enable streaming traces on that specific service or application.</span></span>
6. <span data-ttu-id="469f7-122">Diagnostiska händelser kan ses i den automatiskt genererade **ServiceEventSource.cs** filen och anropas från programkod.</span><span class="sxs-lookup"><span data-stu-id="469f7-122">The diagnostic events can be seen in the automatically generated **ServiceEventSource.cs** file and are called from application code.</span></span>
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. <span data-ttu-id="469f7-123">Den **diagnostiska händelser** fönstret stöder filtrering, pausa och undersökning av händelser i realtid.</span><span class="sxs-lookup"><span data-stu-id="469f7-123">The **Diagnostic Events** window supports filtering, pausing, and inspecting events in real time.</span></span>  <span data-ttu-id="469f7-124">Filtret är en sträng sökning i händelsemeddelandet, inklusive dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="469f7-124">The filter is a simple string search of the event message, including its contents.</span></span>
   
    ![Filtrera, pausa och återuppta eller granska händelser i realtid][diagnosticeventsactions]
8. <span data-ttu-id="469f7-126">Tjänster-felsökning är precis som du felsöker andra program.</span><span class="sxs-lookup"><span data-stu-id="469f7-126">Debugging services is like debugging any other application.</span></span> <span data-ttu-id="469f7-127">Du kommer normalt ange brytpunkter via Visual Studio för enkelt felsökning.</span><span class="sxs-lookup"><span data-stu-id="469f7-127">You will normally set Breakpoints through Visual Studio for easy debugging.</span></span> <span data-ttu-id="469f7-128">Även om tillförlitliga samlingar replikeras över flera noder, implementerar de fortfarande IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="469f7-128">Even though Reliable Collections replicate across multiple nodes, they still implement IEnumerable.</span></span> <span data-ttu-id="469f7-129">Detta innebär att du kan använda vyn resultat i Visual Studio när du felsöker för att se vad du har lagrat i.</span><span class="sxs-lookup"><span data-stu-id="469f7-129">This means that you can use the Results View in Visual Studio while debugging to see what you've stored inside.</span></span> <span data-ttu-id="469f7-130">Ange en brytpunkt bara, var som helst i koden.</span><span class="sxs-lookup"><span data-stu-id="469f7-130">Simply set a breakpoint anywhere in your code.</span></span>
   
    ![Starta felsökning av ett program][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a><span data-ttu-id="469f7-132">Felsöka fjärrprogram Service Fabric</span><span class="sxs-lookup"><span data-stu-id="469f7-132">Debug a remote Service Fabric application</span></span>
<span data-ttu-id="469f7-133">Om Service Fabric-program körs på ett Service Fabric-kluster i Azure, kan du felsöka de direkt från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="469f7-133">If your Service Fabric applications are running on a Service Fabric cluster in Azure, you are able to remotely debug these, directly from Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="469f7-134">Funktionen kräver [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) och [Azure SDK för .NET 2.9](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="469f7-134">The feature requires [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) and [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span></span>    
> 
> 

<!-- -->
> [!WARNING]
> <span data-ttu-id="469f7-135">Fjärrfelsökning är avsedd för utveckling och testning scenarier och inte ska användas i produktionsmiljöer, på grund av påverkan på program som körs.</span><span class="sxs-lookup"><span data-stu-id="469f7-135">Remote debugging is meant for dev/test scenarios and not to be used in production environments, because of the impact on the running applications.</span></span>
> 
> 

1. <span data-ttu-id="469f7-136">Navigera till klustret i **Cloud Explorer**, högerklicka och välj **aktivera felsökning**</span><span class="sxs-lookup"><span data-stu-id="469f7-136">Navigate to your cluster in **Cloud Explorer**, right-click and choose **Enable Debugging**</span></span>
   
    ![Aktivera fjärrfelsökning][enableremotedebugging]
   
    <span data-ttu-id="469f7-138">Detta kommer startar processen för att aktivera tillägget för fjärranslutna felsökning på klusternoder, samt nödvändiga nätverkskonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="469f7-138">This will kick off the process of enabling the remote debugging extension on your cluster nodes, as well as required network configurations.</span></span>
2. <span data-ttu-id="469f7-139">Högerklicka på noden i klustret i **Cloud Explorer**, och välj **koppla felsökare**</span><span class="sxs-lookup"><span data-stu-id="469f7-139">Right-click the cluster node in **Cloud Explorer**, and choose **Attach Debugger**</span></span>
   
    ![Koppla felsökare][attachdebugger]
3. <span data-ttu-id="469f7-141">I den **koppla till process** dialogrutan, Välj den process som du vill felsöka och klicka på **bifoga**</span><span class="sxs-lookup"><span data-stu-id="469f7-141">In the **Attach to process** dialog, choose the process you want to debug, and click **Attach**</span></span>
   
    ![Välj process][chooseprocess]
   
    <span data-ttu-id="469f7-143">Namnet på processen du vill koppla till, lika med namnet på sammansättningen för tjänsten projektnamn.</span><span class="sxs-lookup"><span data-stu-id="469f7-143">The name of the process you want to attach to, equals the name of your service project assembly name.</span></span>
   
    <span data-ttu-id="469f7-144">Felsökningsprogrammet ska kopplas till alla noder som kör processen.</span><span class="sxs-lookup"><span data-stu-id="469f7-144">The debugger will attach to all nodes running the process.</span></span>
   
   * <span data-ttu-id="469f7-145">I de fall där du felsöker en tillståndslös service, tillhör alla instanser av tjänsten på alla noder felsökningssessionen.</span><span class="sxs-lookup"><span data-stu-id="469f7-145">In the case where you are debugging a stateless service, all instances of the service on all nodes are part of the debug session.</span></span>
   * <span data-ttu-id="469f7-146">Om du felsöker en tillståndskänslig service blir den primära repliken av någon partition active och därför fångats av felsökningsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="469f7-146">If you are debugging a stateful service, only the primary replica of any partition will be active and therefore caught by the debugger.</span></span> <span data-ttu-id="469f7-147">Om den primära repliken flyttas under felsökningssessionen, bearbetning av repliken fortfarande att vara en del av felsökningssessionen.</span><span class="sxs-lookup"><span data-stu-id="469f7-147">If the primary replica moves during the debug session, the processing of that replica will still be part of the debug session.</span></span>
   * <span data-ttu-id="469f7-148">För att fånga endast relevanta partitioner eller instanser av en viss tjänst kan använda du villkorlig brytpunkter bryta endast en specifik partition eller instansen.</span><span class="sxs-lookup"><span data-stu-id="469f7-148">In order to only catch relevant partitions or instances of a given service, you can use conditional breakpoints to only break a specific partition or instance.</span></span>
     
     ![Villkorlig brytpunkt][conditionalbreakpoint]
     
     > [!NOTE]
     > <span data-ttu-id="469f7-150">Vi stöder för närvarande inte felsökning ett Service Fabric-kluster med flera instanser av samma körbara namn.</span><span class="sxs-lookup"><span data-stu-id="469f7-150">Currently we do not support debugging a Service Fabric cluster with multiple instances of the same service executable name.</span></span>
     > 
     > 
4. <span data-ttu-id="469f7-151">När du är klar med att felsöka ditt program kan du inaktivera fjärråtkomst felsökning tillägget genom att högerklicka på klustret i **Cloud Explorer** och välj **inaktivera felsökning**</span><span class="sxs-lookup"><span data-stu-id="469f7-151">Once you finish debugging your application, you can disable the remote debugging extension by right-clicking the cluster in **Cloud Explorer** and choose **Disable Debugging**</span></span>
   
    ![Inaktivera fjärrfelsökning][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a><span data-ttu-id="469f7-153">Strömning spår från en fjärransluten klusternod</span><span class="sxs-lookup"><span data-stu-id="469f7-153">Streaming traces from a remote cluster node</span></span>
<span data-ttu-id="469f7-154">Du kan också att dataströmmen spårningar direkt från en fjärransluten klusternod till Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="469f7-154">You are also able to stream traces directly from a remote cluster node to Visual Studio.</span></span> <span data-ttu-id="469f7-155">Den här funktionen kan du dataströmmen ETW spåra händelser som genereras på en klusternod för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="469f7-155">This feature allows you to stream ETW trace events, produced on a Service Fabric cluster node.</span></span>

> [!NOTE]
> <span data-ttu-id="469f7-156">Den här funktionen kräver [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) och [Azure SDK för .NET 2.9](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="469f7-156">This feature requires [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) and [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span></span>
> <span data-ttu-id="469f7-157">Den här funktionen har endast stöd för kluster som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="469f7-157">This feature only supports clusters running in Azure.</span></span>
> 
> 

<!-- -->
> [!WARNING]
> <span data-ttu-id="469f7-158">Strömning spårningar är avsedd för utveckling och testning scenarier och inte ska användas i produktionsmiljöer, på grund av påverkan på program som körs.</span><span class="sxs-lookup"><span data-stu-id="469f7-158">Streaming traces is meant for dev/test scenarios and not to be used in production environments, because of the impact on the running applications.</span></span>
> <span data-ttu-id="469f7-159">I ett produktionsscenario ska lita på vidarebefordran av händelser med hjälp av Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="469f7-159">In a production scenario, you should rely on forwarding events using Azure Diagnostics.</span></span>
> 
> 

1. <span data-ttu-id="469f7-160">Navigera till klustret i **Cloud Explorer**, högerklicka och välj **aktivera strömning spårningar**</span><span class="sxs-lookup"><span data-stu-id="469f7-160">Navigate to your cluster in **Cloud Explorer**, right-click and choose **Enable Streaming Traces**</span></span>
   
    ![Aktivera remote strömmande spårningar][enablestreamingtraces]
   
    <span data-ttu-id="469f7-162">Detta kommer startar processen för att aktivera tillägget strömmande spårningar på klusternoder, samt nödvändiga nätverkskonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="469f7-162">This will kick off the process of enabling the streaming traces extension on your cluster nodes, as well as required network configurations.</span></span>
2. <span data-ttu-id="469f7-163">Expandera den **noder** element i **Cloud Explorer**, högerklicka på den nod som du vill strömma spår från och välj **visa strömning spårningar**</span><span class="sxs-lookup"><span data-stu-id="469f7-163">Expand the **Nodes** element in **Cloud Explorer**, right-click the node you want to stream traces from and choose **View Streaming Traces**</span></span>
   
    ![Visa remote strömning spårningar][viewremotestreamingtraces]
   
    <span data-ttu-id="469f7-165">Upprepa steg 2 för så många noder som du vill visa spårningsinformation från.</span><span class="sxs-lookup"><span data-stu-id="469f7-165">Repeat step 2 for as many nodes as you want to see traces from.</span></span> <span data-ttu-id="469f7-166">Varje noder dataström kommer att visas i ett dedikerat fönster.</span><span class="sxs-lookup"><span data-stu-id="469f7-166">Each nodes stream will show up in a dedicated window.</span></span>
   
    <span data-ttu-id="469f7-167">Du är nu kunna se spårningen sänds av Service Fabric och dina tjänster.</span><span class="sxs-lookup"><span data-stu-id="469f7-167">You are now able to see the traces emitted by Service Fabric, and your services.</span></span> <span data-ttu-id="469f7-168">Om du vill filtrera händelser så att endast ett visst program bara ange namnet på programmet i filtret.</span><span class="sxs-lookup"><span data-stu-id="469f7-168">If you want to filter the events to only show a specific application, simply type in the name of the application in the filter.</span></span>
   
    ![Visa strömning spårar][viewingstreamingtraces]
3. <span data-ttu-id="469f7-170">När du är klar strömmande spår från klustret, du kan inaktivera fjärråtkomst strömmande spårning, genom att högerklicka på klustret i **Cloud Explorer** och välj **inaktivera strömning spårningar**</span><span class="sxs-lookup"><span data-stu-id="469f7-170">Once you are done streaming traces from your cluster, you can disable remote streaming traces, by right-clicking the cluster in **Cloud Explorer** and choose **Disable Streaming Traces**</span></span>
   
    ![Inaktivera fjärråtkomst strömmande spårningar][disablestreamingtraces]

## <a name="next-steps"></a><span data-ttu-id="469f7-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="469f7-172">Next steps</span></span>
* <span data-ttu-id="469f7-173">[Testa en tjänst för Service Fabric](service-fabric-testability-overview.md).</span><span class="sxs-lookup"><span data-stu-id="469f7-173">[Test a Service Fabric service](service-fabric-testability-overview.md).</span></span>
* <span data-ttu-id="469f7-174">[Hantera Service Fabric-program i Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="469f7-174">[Manage your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span></span>

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
