---
title: aaaDebug ditt program i Visual Studio | Microsoft Docs
description: "Förbättra hello pålitligheten och prestandan för dina tjänster genom att utveckla och felsökning i Visual Studio dem på ett kluster för lokal utveckling."
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
ms.openlocfilehash: 8d08689e82195d09f57b462631ad04fd237bc4fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a><span data-ttu-id="045ae-103">Felsöka Service Fabric-program med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045ae-103">Debug your Service Fabric application by using Visual Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="045ae-104">Visual Studio/CSharp</span><span class="sxs-lookup"><span data-stu-id="045ae-104">Visual Studio/CSharp</span></span>](service-fabric-debugging-your-application.md) 
> * [<span data-ttu-id="045ae-105">Eclipse/Java</span><span class="sxs-lookup"><span data-stu-id="045ae-105">Eclipse/Java</span></span>](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a><span data-ttu-id="045ae-106">Felsöka ett lokala Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="045ae-106">Debug a local Service Fabric application</span></span>
<span data-ttu-id="045ae-107">Du kan spara tid och pengar genom att distribuera och felsöka ditt Azure Service Fabric-program i ett kluster för utveckling av lokal dator.</span><span class="sxs-lookup"><span data-stu-id="045ae-107">You can save time and money by deploying and debugging your Azure Service Fabric application in a local computer development cluster.</span></span> <span data-ttu-id="045ae-108">Visual Studio 2017 eller Visual Studio 2015 kan distribuera hello programmet toohello lokala klustret och ansluta automatiskt hello felsökare tooall instanser av programmet.</span><span class="sxs-lookup"><span data-stu-id="045ae-108">Visual Studio 2017 or Visual Studio 2015 can deploy hello application toohello local cluster and automatically connect hello debugger tooall instances of your application.</span></span>

1. <span data-ttu-id="045ae-109">Starta en lokal utveckling klustret genom att följa stegen hello i [ställa in din utvecklingsmiljö för Service Fabric](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="045ae-109">Start a local development cluster by following hello steps in [Setting up your Service Fabric development environment](service-fabric-get-started.md).</span></span>
2. <span data-ttu-id="045ae-110">Tryck på **F5** eller klicka på **felsöka** > **Start Debugging**.</span><span class="sxs-lookup"><span data-stu-id="045ae-110">Press **F5** or click **Debug** > **Start Debugging**.</span></span>
   
    ![Starta felsökning av ett program][startdebugging]
3. <span data-ttu-id="045ae-112">Ange brytpunkter i koden och stega igenom hello program genom att klicka på kommandon i hello **felsöka** menyn.</span><span class="sxs-lookup"><span data-stu-id="045ae-112">Set breakpoints in your code and step through hello application by clicking commands in hello **Debug** menu.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="045ae-113">Visual Studio bifogar tooall instanser av programmet.</span><span class="sxs-lookup"><span data-stu-id="045ae-113">Visual Studio attaches tooall instances of your application.</span></span> <span data-ttu-id="045ae-114">När du stega igenom koden hämta brytpunkter nådde av flera processer som resulterar i samtidiga sessioner.</span><span class="sxs-lookup"><span data-stu-id="045ae-114">While you're stepping through code, breakpoints may get hit by multiple processes resulting in concurrent sessions.</span></span> <span data-ttu-id="045ae-115">Prova att inaktivera hello brytpunkter när de träffar, genom att göra varje brytpunkt villkorlig på hello tråd-ID eller med hjälp av diagnostiska händelser.</span><span class="sxs-lookup"><span data-stu-id="045ae-115">Try disabling hello breakpoints after they're hit, by making each breakpoint conditional on hello thread ID or by using diagnostic events.</span></span>
   > 
   > 
4. <span data-ttu-id="045ae-116">Hej **diagnostiska händelser** öppnas automatiskt så att du kan visa diagnostik händelser i realtid.</span><span class="sxs-lookup"><span data-stu-id="045ae-116">hello **Diagnostic Events** window will automatically open so you can view diagnostic events in real time.</span></span>
   
    ![Visa diagnostik händelser i realtid][diagnosticevents]
5. <span data-ttu-id="045ae-118">Du kan också öppna hello **diagnostiska händelser** fönster i Cloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="045ae-118">You can also open hello **Diagnostic Events** window in Cloud Explorer.</span></span>  <span data-ttu-id="045ae-119">Under **Service Fabric**, högerklicka på en nod och välj **visa strömning spårningar**.</span><span class="sxs-lookup"><span data-stu-id="045ae-119">Under **Service Fabric**, right-click any node and choose **View Streaming Traces**.</span></span>
   
    ![Öppna hello diagnostiska händelser fönster][viewdiagnosticevents]
   
    <span data-ttu-id="045ae-121">Om du vill toofilter spårningar tooa specifika tjänsten eller programmet kan bara aktivera strömmande spårningar på den specifika tjänsten eller programmet.</span><span class="sxs-lookup"><span data-stu-id="045ae-121">If you want toofilter your traces tooa specific service or application, simply enable streaming traces on that specific service or application.</span></span>
6. <span data-ttu-id="045ae-122">hello diagnostiska händelser kan ses i hello genereras automatiskt **ServiceEventSource.cs** filen och anropas från programkod.</span><span class="sxs-lookup"><span data-stu-id="045ae-122">hello diagnostic events can be seen in hello automatically generated **ServiceEventSource.cs** file and are called from application code.</span></span>
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. <span data-ttu-id="045ae-123">Hej **diagnostiska händelser** fönstret stöder filtrering, pausa och undersökning av händelser i realtid.</span><span class="sxs-lookup"><span data-stu-id="045ae-123">hello **Diagnostic Events** window supports filtering, pausing, and inspecting events in real time.</span></span>  <span data-ttu-id="045ae-124">hello-filter är en sträng sökning i hello händelsemeddelande, inklusive dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="045ae-124">hello filter is a simple string search of hello event message, including its contents.</span></span>
   
    ![Filtrera, pausa och återuppta eller granska händelser i realtid][diagnosticeventsactions]
8. <span data-ttu-id="045ae-126">Tjänster-felsökning är precis som du felsöker andra program.</span><span class="sxs-lookup"><span data-stu-id="045ae-126">Debugging services is like debugging any other application.</span></span> <span data-ttu-id="045ae-127">Du kommer normalt ange brytpunkter via Visual Studio för enkelt felsökning.</span><span class="sxs-lookup"><span data-stu-id="045ae-127">You will normally set Breakpoints through Visual Studio for easy debugging.</span></span> <span data-ttu-id="045ae-128">Även om tillförlitliga samlingar replikeras över flera noder, implementerar de fortfarande IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="045ae-128">Even though Reliable Collections replicate across multiple nodes, they still implement IEnumerable.</span></span> <span data-ttu-id="045ae-129">Detta innebär att du kan använda hello resultat i Visual Studio när du felsöker toosee vad du har lagrat i.</span><span class="sxs-lookup"><span data-stu-id="045ae-129">This means that you can use hello Results View in Visual Studio while debugging toosee what you've stored inside.</span></span> <span data-ttu-id="045ae-130">Ange en brytpunkt bara, var som helst i koden.</span><span class="sxs-lookup"><span data-stu-id="045ae-130">Simply set a breakpoint anywhere in your code.</span></span>
   
    ![Starta felsökning av ett program][breakpoint]

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a><span data-ttu-id="045ae-132">Felsöka fjärrprogram Service Fabric</span><span class="sxs-lookup"><span data-stu-id="045ae-132">Debug a remote Service Fabric application</span></span>
<span data-ttu-id="045ae-133">Om Service Fabric-program körs på ett Service Fabric-kluster i Azure, du kan tooremotely felsöka dessa direkt från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="045ae-133">If your Service Fabric applications are running on a Service Fabric cluster in Azure, you are able tooremotely debug these, directly from Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="045ae-134">hello-funktionen kräver [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) och [Azure SDK för .NET 2.9](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="045ae-134">hello feature requires [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) and [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span></span>    
> 
> 

<!-- -->
> [!WARNING]
> <span data-ttu-id="045ae-135">Fjärrfelsökning är avsett för scenarier för utveckling och testning och inte toobe användas i produktionsmiljöer, på grund av hello inverkan på hello program som körs.</span><span class="sxs-lookup"><span data-stu-id="045ae-135">Remote debugging is meant for dev/test scenarios and not toobe used in production environments, because of hello impact on hello running applications.</span></span>
> 
> 

1. <span data-ttu-id="045ae-136">Navigera tooyour klustret i **Cloud Explorer**, högerklicka och välj **aktivera felsökning**</span><span class="sxs-lookup"><span data-stu-id="045ae-136">Navigate tooyour cluster in **Cloud Explorer**, right-click and choose **Enable Debugging**</span></span>
   
    ![Aktivera fjärrfelsökning][enableremotedebugging]
   
    <span data-ttu-id="045ae-138">Detta kommer startar hello innebär att aktivera hello fjärrfelsökning tillägg på klusternoderna, samt krävs nätverkskonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="045ae-138">This will kick off hello process of enabling hello remote debugging extension on your cluster nodes, as well as required network configurations.</span></span>
2. <span data-ttu-id="045ae-139">Högerklicka på hello klusternod i **Cloud Explorer**, och välj **koppla felsökare**</span><span class="sxs-lookup"><span data-stu-id="045ae-139">Right-click hello cluster node in **Cloud Explorer**, and choose **Attach Debugger**</span></span>
   
    ![Koppla felsökare][attachdebugger]
3. <span data-ttu-id="045ae-141">I hello **bifoga tooprocess** dialogrutan Välj hello process du toodebug och på **bifoga**</span><span class="sxs-lookup"><span data-stu-id="045ae-141">In hello **Attach tooprocess** dialog, choose hello process you want toodebug, and click **Attach**</span></span>
   
    ![Välj process][chooseprocess]
   
    <span data-ttu-id="045ae-143">hello namn hello process som du vill tooattach till, som är lika med hello namnet på sammansättningen för tjänsten projektnamn.</span><span class="sxs-lookup"><span data-stu-id="045ae-143">hello name of hello process you want tooattach to, equals hello name of your service project assembly name.</span></span>
   
    <span data-ttu-id="045ae-144">hello-felsökaren kan koppla tooall noder som kör hello-processen.</span><span class="sxs-lookup"><span data-stu-id="045ae-144">hello debugger will attach tooall nodes running hello process.</span></span>
   
   * <span data-ttu-id="045ae-145">I hello fall där du felsöker tillståndslösa tjänsten måste tillhör alla instanser av hello-tjänsten på alla noder hello felsökningssessionen.</span><span class="sxs-lookup"><span data-stu-id="045ae-145">In hello case where you are debugging a stateless service, all instances of hello service on all nodes are part of hello debug session.</span></span>
   * <span data-ttu-id="045ae-146">Om du felsöker en tillståndskänslig service blir bara hello primära repliken av någon partition active och därför fångats med hello felsökningsprogram.</span><span class="sxs-lookup"><span data-stu-id="045ae-146">If you are debugging a stateful service, only hello primary replica of any partition will be active and therefore caught by hello debugger.</span></span> <span data-ttu-id="045ae-147">Om hello primära repliken flyttas under felsökningssessionen hello, kommer hello bearbetningen av den repliken fortfarande att en del av hello felsökningssessionen.</span><span class="sxs-lookup"><span data-stu-id="045ae-147">If hello primary replica moves during hello debug session, hello processing of that replica will still be part of hello debug session.</span></span>
   * <span data-ttu-id="045ae-148">Du kan använda villkorlig brytpunkter tooonly break en specifik partition eller instans i ordning tooonly catch relevanta partitioner eller instanser av en viss tjänst.</span><span class="sxs-lookup"><span data-stu-id="045ae-148">In order tooonly catch relevant partitions or instances of a given service, you can use conditional breakpoints tooonly break a specific partition or instance.</span></span>
     
     ![Villkorlig brytpunkt][conditionalbreakpoint]
     
     > [!NOTE]
     > <span data-ttu-id="045ae-150">Det finns inte stöd för felsökning av ett Service Fabric-kluster med flera instanser av hello samma körbara namn.</span><span class="sxs-lookup"><span data-stu-id="045ae-150">Currently we do not support debugging a Service Fabric cluster with multiple instances of hello same service executable name.</span></span>
     > 
     > 
4. <span data-ttu-id="045ae-151">När du är klar med att felsöka ditt program kan du inaktivera hello remote felsökning tillägg genom att högerklicka på hello-kluster i **Cloud Explorer** och välj **inaktivera felsökning**</span><span class="sxs-lookup"><span data-stu-id="045ae-151">Once you finish debugging your application, you can disable hello remote debugging extension by right-clicking hello cluster in **Cloud Explorer** and choose **Disable Debugging**</span></span>
   
    ![Inaktivera fjärrfelsökning][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a><span data-ttu-id="045ae-153">Strömning spår från en fjärransluten klusternod</span><span class="sxs-lookup"><span data-stu-id="045ae-153">Streaming traces from a remote cluster node</span></span>
<span data-ttu-id="045ae-154">Du kan också kan toostream spårningssessioner direkt från ett kluster nod tooVisual Studio.</span><span class="sxs-lookup"><span data-stu-id="045ae-154">You are also able toostream traces directly from a remote cluster node tooVisual Studio.</span></span> <span data-ttu-id="045ae-155">Den här funktionen kan du toostream ETW spåra händelser som genereras på en klusternod för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="045ae-155">This feature allows you toostream ETW trace events, produced on a Service Fabric cluster node.</span></span>

> [!NOTE]
> <span data-ttu-id="045ae-156">Den här funktionen kräver [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) och [Azure SDK för .NET 2.9](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="045ae-156">This feature requires [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) and [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span></span>
> <span data-ttu-id="045ae-157">Den här funktionen har endast stöd för kluster som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="045ae-157">This feature only supports clusters running in Azure.</span></span>
> 
> 

<!-- -->
> [!WARNING]
> <span data-ttu-id="045ae-158">Strömning spårningar är avsett för scenarier för utveckling och testning och inte toobe användas i produktionsmiljöer, på grund av hello inverkan på hello program som körs.</span><span class="sxs-lookup"><span data-stu-id="045ae-158">Streaming traces is meant for dev/test scenarios and not toobe used in production environments, because of hello impact on hello running applications.</span></span>
> <span data-ttu-id="045ae-159">I ett produktionsscenario ska lita på vidarebefordran av händelser med hjälp av Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="045ae-159">In a production scenario, you should rely on forwarding events using Azure Diagnostics.</span></span>
> 
> 

1. <span data-ttu-id="045ae-160">Navigera tooyour klustret i **Cloud Explorer**, högerklicka och välj **aktivera strömning spårningar**</span><span class="sxs-lookup"><span data-stu-id="045ae-160">Navigate tooyour cluster in **Cloud Explorer**, right-click and choose **Enable Streaming Traces**</span></span>
   
    ![Aktivera remote strömmande spårningar][enablestreamingtraces]
   
    <span data-ttu-id="045ae-162">Detta kommer startar hello innebär att aktivera hello strömning spårningar tillägg på klusternoderna, samt krävs nätverkskonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="045ae-162">This will kick off hello process of enabling hello streaming traces extension on your cluster nodes, as well as required network configurations.</span></span>
2. <span data-ttu-id="045ae-163">Expandera hello **noder** element i **Cloud Explorer**, högerklicka på hello nod toostream spår från och välj **visa strömning spårningar**</span><span class="sxs-lookup"><span data-stu-id="045ae-163">Expand hello **Nodes** element in **Cloud Explorer**, right-click hello node you want toostream traces from and choose **View Streaming Traces**</span></span>
   
    ![Visa remote strömning spårningar][viewremotestreamingtraces]
   
    <span data-ttu-id="045ae-165">Upprepa steg 2 för så många noder som du vill toosee spår från.</span><span class="sxs-lookup"><span data-stu-id="045ae-165">Repeat step 2 for as many nodes as you want toosee traces from.</span></span> <span data-ttu-id="045ae-166">Varje noder dataström kommer att visas i ett dedikerat fönster.</span><span class="sxs-lookup"><span data-stu-id="045ae-166">Each nodes stream will show up in a dedicated window.</span></span>
   
    <span data-ttu-id="045ae-167">Du är nu kan toosee hello spårningar sänds av Service Fabric och dina tjänster.</span><span class="sxs-lookup"><span data-stu-id="045ae-167">You are now able toosee hello traces emitted by Service Fabric, and your services.</span></span> <span data-ttu-id="045ae-168">Om du vill visa toofilter hello händelser tooonly ett visst program bara ange hello namn för hello programmet hello filter.</span><span class="sxs-lookup"><span data-stu-id="045ae-168">If you want toofilter hello events tooonly show a specific application, simply type in hello name of hello application in hello filter.</span></span>
   
    ![Visa strömning spårar][viewingstreamingtraces]
3. <span data-ttu-id="045ae-170">När du är klar strömmande spår från ditt kluster, du kan inaktivera fjärråtkomst strömmande spårning, genom att högerklicka på klustret hello i **Cloud Explorer** och välj **inaktivera strömning spårningar**</span><span class="sxs-lookup"><span data-stu-id="045ae-170">Once you are done streaming traces from your cluster, you can disable remote streaming traces, by right-clicking hello cluster in **Cloud Explorer** and choose **Disable Streaming Traces**</span></span>
   
    ![Inaktivera fjärråtkomst strömmande spårningar][disablestreamingtraces]

## <a name="next-steps"></a><span data-ttu-id="045ae-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="045ae-172">Next steps</span></span>
* <span data-ttu-id="045ae-173">[Testa en tjänst för Service Fabric](service-fabric-testability-overview.md).</span><span class="sxs-lookup"><span data-stu-id="045ae-173">[Test a Service Fabric service](service-fabric-testability-overview.md).</span></span>
* <span data-ttu-id="045ae-174">[Hantera Service Fabric-program i Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="045ae-174">[Manage your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span></span>

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
