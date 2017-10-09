---
title: "aaaVisualizing med hjälp av Service Fabric Explorer | Microsoft Docs"
description: "Service Fabric Explorer är ett webbaserat verktyg för att kontrollera och hantera molnprogram och noder i ett Microsoft Azure Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: ryanwi
ms.openlocfilehash: 73adc4fc254cf6b949b4419b02a046cee3f6a83d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a><span data-ttu-id="eb356-103">Visualisera ditt kluster med Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="eb356-103">Visualize your cluster with Service Fabric Explorer</span></span>
<span data-ttu-id="eb356-104">Service Fabric Explorer är ett webbaserat verktyg för att kontrollera och hantera program och noderna i Azure Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="eb356-104">Service Fabric Explorer is a web-based tool for inspecting and managing applications and nodes in an Azure Service Fabric cluster.</span></span> <span data-ttu-id="eb356-105">Service Fabric Explorer ligger direkt på hello klustret så att det alltid är tillgängliga, oavsett om klustret körs.</span><span class="sxs-lookup"><span data-stu-id="eb356-105">Service Fabric Explorer is hosted directly within hello cluster, so it is always available, regardless of where your cluster is running.</span></span>

## <a name="video-tutorial"></a><span data-ttu-id="eb356-106">Videosjälvstudie</span><span class="sxs-lookup"><span data-stu-id="eb356-106">Video tutorial</span></span>

<span data-ttu-id="eb356-107">toolearn hur toouse Service Fabric Explorer titta på hello följande Microsoft Virtual Academy video:</span><span class="sxs-lookup"><span data-stu-id="eb356-107">toolearn how toouse Service Fabric Explorer, watch hello following Microsoft Virtual Academy video:</span></span>

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-tooservice-fabric-explorer"></a><span data-ttu-id="eb356-108">Ansluta tooService Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="eb356-108">Connect tooService Fabric Explorer</span></span>
<span data-ttu-id="eb356-109">Om du har följt anvisningarna hello för[förbereda din utvecklingsmiljö](service-fabric-get-started.md), kan du starta Service Fabric-Utforskaren på din lokala klustret genom att gå toohttp://localhost:19080 / Explorer.</span><span class="sxs-lookup"><span data-stu-id="eb356-109">If you have followed hello instructions too[prepare your development environment](service-fabric-get-started.md), you can launch Service Fabric Explorer on your local cluster by navigating toohttp://localhost:19080/Explorer.</span></span>

## <a name="understand-hello-service-fabric-explorer-layout"></a><span data-ttu-id="eb356-110">Förstå hello Service Fabric Explorer layout</span><span class="sxs-lookup"><span data-stu-id="eb356-110">Understand hello Service Fabric Explorer layout</span></span>
<span data-ttu-id="eb356-111">Du kan bläddra igenom Service Fabric Explorer med hjälp av hello trädet hello vänster.</span><span class="sxs-lookup"><span data-stu-id="eb356-111">You can navigate through Service Fabric Explorer by using hello tree on hello left.</span></span> <span data-ttu-id="eb356-112">Hello roten i trädet hello innehåller hello klusterinstrumentpanel en översikt över klustret, inklusive en sammanfattning av program och noden hälsa.</span><span class="sxs-lookup"><span data-stu-id="eb356-112">At hello root of hello tree, hello cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span>

![Instrumentpanelen för Service Fabric Explorer-klustret][sfx-cluster-dashboard]

### <a name="view-hello-clusters-layout"></a><span data-ttu-id="eb356-114">Visa hello klustrets layout</span><span class="sxs-lookup"><span data-stu-id="eb356-114">View hello cluster's layout</span></span>
<span data-ttu-id="eb356-115">Noder i ett Service Fabric-kluster är placerade över en tvådimensionell rutnät för feldomäner och uppgradera domäner.</span><span class="sxs-lookup"><span data-stu-id="eb356-115">Nodes in a Service Fabric cluster are placed across a two-dimensional grid of fault domains and upgrade domains.</span></span> <span data-ttu-id="eb356-116">Den här placering garanterar att dina program ska vara tillgängliga i hello förekomsten av maskinvarufel och programuppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="eb356-116">This placement ensures that your applications remain available in hello presence of hardware failures and application upgrades.</span></span> <span data-ttu-id="eb356-117">Du kan visa hur hello aktuella klustret är placerade med hjälp av hello över lediga kluster.</span><span class="sxs-lookup"><span data-stu-id="eb356-117">You can view how hello current cluster is laid out by using hello cluster map.</span></span>

![Service Fabric Explorer över lediga kluster][sfx-cluster-map]

### <a name="view-applications-and-services"></a><span data-ttu-id="eb356-119">Visa program och tjänster</span><span class="sxs-lookup"><span data-stu-id="eb356-119">View applications and services</span></span>
<span data-ttu-id="eb356-120">hello-kluster som innehåller två underträd: en för program och en annan för noder.</span><span class="sxs-lookup"><span data-stu-id="eb356-120">hello cluster contains two subtrees: one for applications and another for nodes.</span></span>

<span data-ttu-id="eb356-121">Du kan använda hello program visa toonavigate via Service Fabric logiska hierarkin: program, tjänster, partitioner och repliker.</span><span class="sxs-lookup"><span data-stu-id="eb356-121">You can use hello application view toonavigate through Service Fabric's logical hierarchy: applications, services, partitions, and replicas.</span></span>

<span data-ttu-id="eb356-122">Hej program i hello exemplet nedan **MyApp** består av två tjänster **MyStatefulService** och **WebService**.</span><span class="sxs-lookup"><span data-stu-id="eb356-122">In hello example below, hello application **MyApp** consists of two services, **MyStatefulService** and **WebService**.</span></span> <span data-ttu-id="eb356-123">Eftersom **MyStatefulService** är tillståndskänslig så innehåller en partition med en primär och två sekundära repliker.</span><span class="sxs-lookup"><span data-stu-id="eb356-123">Since **MyStatefulService** is stateful, it includes a partition with one primary and two secondary replicas.</span></span> <span data-ttu-id="eb356-124">Däremot WebSvcService är tillståndslösa och innehåller en enda instans.</span><span class="sxs-lookup"><span data-stu-id="eb356-124">By contrast, WebSvcService is stateless and contains a single instance.</span></span>

![Service Fabric Explorer programvy][sfx-application-tree]

<span data-ttu-id="eb356-126">På varje nivå i trädet hello visar hello huvudfönstret relevant information om hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="eb356-126">At each level of hello tree, hello main pane shows pertinent information about hello item.</span></span> <span data-ttu-id="eb356-127">Du kan till exempel se hello hälsostatus och version för en viss tjänst.</span><span class="sxs-lookup"><span data-stu-id="eb356-127">For example, you can see hello health status and version for a particular service.</span></span>

![Service Fabric Explorer essentials fönstret][sfx-service-essentials]

### <a name="view-hello-clusters-nodes"></a><span data-ttu-id="eb356-129">Visa hello klusternoder</span><span class="sxs-lookup"><span data-stu-id="eb356-129">View hello cluster's nodes</span></span>
<span data-ttu-id="eb356-130">hello nod vyn visar hello fysiska struktur hello klustret.</span><span class="sxs-lookup"><span data-stu-id="eb356-130">hello node view shows hello physical layout of hello cluster.</span></span> <span data-ttu-id="eb356-131">För en viss nod kan du inspektera vilka program som har kod distribuerad på noden.</span><span class="sxs-lookup"><span data-stu-id="eb356-131">For a given node, you can inspect which applications have code deployed on that node.</span></span> <span data-ttu-id="eb356-132">Mer specifikt kan du se vilka repliker körs det.</span><span class="sxs-lookup"><span data-stu-id="eb356-132">More specifically, you can see which replicas are currently running there.</span></span>

## <a name="actions"></a><span data-ttu-id="eb356-133">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="eb356-133">Actions</span></span>
<span data-ttu-id="eb356-134">Service Fabric Explorer erbjuder ett snabbt sätt tooinvoke åtgärder på noder, program och tjänster inom klustret.</span><span class="sxs-lookup"><span data-stu-id="eb356-134">Service Fabric Explorer offers a quick way tooinvoke actions on nodes, applications, and services within your cluster.</span></span>

<span data-ttu-id="eb356-135">Till exempel toodelete en programinstans Välj hello programmet hello trädet hello vänster och välj **åtgärder** > **ta bort programmet**.</span><span class="sxs-lookup"><span data-stu-id="eb356-135">For example, toodelete an application instance, choose hello application from hello tree on hello left, and then choose **Actions** > **Delete Application**.</span></span>

![Om du tar bort ett program i Service Fabric Explorer][sfx-delete-application]

> [!TIP]
> <span data-ttu-id="eb356-137">Du kan utföra samma åtgärder hello genom att klicka på hello knappen Nästa tooeach element.</span><span class="sxs-lookup"><span data-stu-id="eb356-137">You can perform hello same actions by clicking hello ellipsis next tooeach element.</span></span>
>
>

<span data-ttu-id="eb356-138">hello visas följande tabell hello-åtgärder som är tillgängliga för varje entitet:</span><span class="sxs-lookup"><span data-stu-id="eb356-138">hello following table lists hello actions available for each entity:</span></span>

| <span data-ttu-id="eb356-139">**Entitet**</span><span class="sxs-lookup"><span data-stu-id="eb356-139">**Entity**</span></span> | <span data-ttu-id="eb356-140">**Åtgärd**</span><span class="sxs-lookup"><span data-stu-id="eb356-140">**Action**</span></span> | <span data-ttu-id="eb356-141">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="eb356-141">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="eb356-142">Programtyp</span><span class="sxs-lookup"><span data-stu-id="eb356-142">Application type</span></span> |<span data-ttu-id="eb356-143">Avetablera typ</span><span class="sxs-lookup"><span data-stu-id="eb356-143">Unprovision type</span></span> |<span data-ttu-id="eb356-144">Tar bort hello programpaketet från avbildningsarkivet hello klustret.</span><span class="sxs-lookup"><span data-stu-id="eb356-144">Removes hello application package from hello cluster's image store.</span></span> <span data-ttu-id="eb356-145">Kräver att typen toobe först bort alla applikationer.</span><span class="sxs-lookup"><span data-stu-id="eb356-145">Requires all applications of that type toobe removed first.</span></span> |
| <span data-ttu-id="eb356-146">Program</span><span class="sxs-lookup"><span data-stu-id="eb356-146">Application</span></span> |<span data-ttu-id="eb356-147">Ta bort program</span><span class="sxs-lookup"><span data-stu-id="eb356-147">Delete Application</span></span> |<span data-ttu-id="eb356-148">Ta bort hello program, inklusive alla tjänster och deras tillstånd (eventuella).</span><span class="sxs-lookup"><span data-stu-id="eb356-148">Delete hello application, including all its services and their state (if any).</span></span> |
| <span data-ttu-id="eb356-149">Tjänst</span><span class="sxs-lookup"><span data-stu-id="eb356-149">Service</span></span> |<span data-ttu-id="eb356-150">Ta bort tjänsten</span><span class="sxs-lookup"><span data-stu-id="eb356-150">Delete Service</span></span> |<span data-ttu-id="eb356-151">Ta bort hello service och dess tillstånd (eventuella).</span><span class="sxs-lookup"><span data-stu-id="eb356-151">Delete hello service and its state (if any).</span></span> |
| <span data-ttu-id="eb356-152">Node</span><span class="sxs-lookup"><span data-stu-id="eb356-152">Node</span></span> |<span data-ttu-id="eb356-153">Aktivera</span><span class="sxs-lookup"><span data-stu-id="eb356-153">Activate</span></span> |<span data-ttu-id="eb356-154">Aktivera hello-nod.</span><span class="sxs-lookup"><span data-stu-id="eb356-154">Activate hello node.</span></span> |
| <span data-ttu-id="eb356-155">Node</span><span class="sxs-lookup"><span data-stu-id="eb356-155">Node</span></span> | <span data-ttu-id="eb356-156">Inaktivera (paus)</span><span class="sxs-lookup"><span data-stu-id="eb356-156">Deactivate (pause)</span></span> | <span data-ttu-id="eb356-157">Pausa hello nod i det aktuella tillståndet.</span><span class="sxs-lookup"><span data-stu-id="eb356-157">Pause hello node in its current state.</span></span> <span data-ttu-id="eb356-158">Tjänster fortsätter toorun men Service Fabric flyttas proaktivt inte något på eller inaktivera den om den inte är nödvändiga tooprevent ett avbrott eller inkonsekventa data.</span><span class="sxs-lookup"><span data-stu-id="eb356-158">Services continue toorun but Service Fabric does not proactively move anything onto or off it unless it is required tooprevent an outage or data inconsistency.</span></span> <span data-ttu-id="eb356-159">Den här åtgärden är brukar användas tooenable felsökning tjänster på en viss nod-tooensure inte flytta under kontroll.</span><span class="sxs-lookup"><span data-stu-id="eb356-159">This action is typically used tooenable debugging services on a specific node tooensure that they do not move during inspection.</span></span> | |
| <span data-ttu-id="eb356-160">Node</span><span class="sxs-lookup"><span data-stu-id="eb356-160">Node</span></span> | <span data-ttu-id="eb356-161">Inaktivera (omstart)</span><span class="sxs-lookup"><span data-stu-id="eb356-161">Deactivate (restart)</span></span> | <span data-ttu-id="eb356-162">Flytta alla InMemory-tjänster av en nod och Stäng beständiga tjänster på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="eb356-162">Safely move all in-memory services off a node and close persistent services.</span></span> <span data-ttu-id="eb356-163">Används vanligtvis när hello värdprocesser eller toobe för datorn måste startas om.</span><span class="sxs-lookup"><span data-stu-id="eb356-163">Typically used when hello host processes or machine need toobe restarted.</span></span> | |
| <span data-ttu-id="eb356-164">Node</span><span class="sxs-lookup"><span data-stu-id="eb356-164">Node</span></span> | <span data-ttu-id="eb356-165">Inaktivera (ta bort data)</span><span class="sxs-lookup"><span data-stu-id="eb356-165">Deactivate (remove data)</span></span> | <span data-ttu-id="eb356-166">Stäng alla tjänster som körs på hello nod när du har skapat tillräckligt ledig repliker på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="eb356-166">Safely close all services running on hello node after building sufficient spare replicas.</span></span> <span data-ttu-id="eb356-167">Används vanligtvis när en nod (eller åtminstone dess lagring) som permanent tas utanför kommissionen.</span><span class="sxs-lookup"><span data-stu-id="eb356-167">Typically used when a node (or at least its storage) is being permanently taken out of commission.</span></span> | |
| <span data-ttu-id="eb356-168">Node</span><span class="sxs-lookup"><span data-stu-id="eb356-168">Node</span></span> | <span data-ttu-id="eb356-169">Ta bort nodens tillstånd</span><span class="sxs-lookup"><span data-stu-id="eb356-169">Remove node state</span></span> | <span data-ttu-id="eb356-170">Ta bort kunskap om en nod repliker från hello kluster.</span><span class="sxs-lookup"><span data-stu-id="eb356-170">Remove knowledge of a node's replicas from hello cluster.</span></span> <span data-ttu-id="eb356-171">Används vanligtvis när en redan felaktiga noden bedöms oåterkalleligt.</span><span class="sxs-lookup"><span data-stu-id="eb356-171">Typically used when an already failed node is deemed unrecoverable.</span></span> | |
| <span data-ttu-id="eb356-172">Node</span><span class="sxs-lookup"><span data-stu-id="eb356-172">Node</span></span> | <span data-ttu-id="eb356-173">Starta om</span><span class="sxs-lookup"><span data-stu-id="eb356-173">Restart</span></span> | <span data-ttu-id="eb356-174">Simulera ett nodfel genom att starta om hello-nod.</span><span class="sxs-lookup"><span data-stu-id="eb356-174">Simulate a node failure by restarting hello node.</span></span> <span data-ttu-id="eb356-175">Mer information [här](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="eb356-175">More information [here](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps)</span></span> | |

<span data-ttu-id="eb356-176">Eftersom många åtgärder är skadliga du tillfrågas tooconfirm din avsikt innan hello åtgärd har slutförts.</span><span class="sxs-lookup"><span data-stu-id="eb356-176">Since many actions are destructive, you may be asked tooconfirm your intent before hello action is completed.</span></span>

> [!TIP]
> <span data-ttu-id="eb356-177">Varje åtgärd som kan utföras via Service Fabric Explorer kan också utföras via PowerShell eller REST-API, tooenable automation.</span><span class="sxs-lookup"><span data-stu-id="eb356-177">Every action that can be performed through Service Fabric Explorer can also be performed through PowerShell or a REST API, tooenable automation.</span></span>
>
>

<span data-ttu-id="eb356-178">Du kan också använda Service Fabric Explorer toocreate programinstanser för en given programtypen och versionen.</span><span class="sxs-lookup"><span data-stu-id="eb356-178">You can also use Service Fabric Explorer toocreate application instances for a given application type and version.</span></span> <span data-ttu-id="eb356-179">Välj hello programtyp i hello trädvyn och klicka sedan på hello **skapa app-instansen** länken nästa toohello version som du vill i hello till höger.</span><span class="sxs-lookup"><span data-stu-id="eb356-179">Choose hello application type in hello tree view, then click hello **Create app instance** link next toohello version you'd like in hello right pane.</span></span>

![Skapa en programinstans i Service Fabric Explorer][sfx-create-app-instance]

> [!NOTE]
> <span data-ttu-id="eb356-181">Programinstanser som skapats via Service Fabric Explorer kan för närvarande parameteriseras.</span><span class="sxs-lookup"><span data-stu-id="eb356-181">Application instances created through Service Fabric Explorer cannot currently be parameterized.</span></span> <span data-ttu-id="eb356-182">De skapas med hjälp av standardparametervärden.</span><span class="sxs-lookup"><span data-stu-id="eb356-182">They are created using default parameter values.</span></span>
>
>

## <a name="connect-tooa-remote-service-fabric-cluster"></a><span data-ttu-id="eb356-183">Ansluta tooa remote Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="eb356-183">Connect tooa remote Service Fabric cluster</span></span>
<span data-ttu-id="eb356-184">Om du känner hello klusterslutpunkten och har behörighet kan du komma åt Service Fabric Explorer från en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="eb356-184">If you know hello cluster's endpoint and have sufficient permissions you can access Service Fabric Explorer from any browser.</span></span> <span data-ttu-id="eb356-185">Detta beror på att Service Fabric Explorer är en tjänst som körs i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="eb356-185">This is because Service Fabric Explorer is just another service that runs in hello cluster.</span></span>

### <a name="discover-hello-service-fabric-explorer-endpoint-for-a-remote-cluster"></a><span data-ttu-id="eb356-186">Identifiera hello Service Fabric Explorer-slutpunkt för ett kluster</span><span class="sxs-lookup"><span data-stu-id="eb356-186">Discover hello Service Fabric Explorer endpoint for a remote cluster</span></span>
<span data-ttu-id="eb356-187">tooreach Service Fabric Explorer för ett kluster, peka webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="eb356-187">tooreach Service Fabric Explorer for a given cluster, point your browser to:</span></span>

<span data-ttu-id="eb356-188">http://&lt;din klusterslutpunkten&gt;: 19080/Explorer</span><span class="sxs-lookup"><span data-stu-id="eb356-188">http://&lt;your-cluster-endpoint&gt;:19080/Explorer</span></span>

<span data-ttu-id="eb356-189">För Azure-kluster finns också hello fullständiga URL: en i hello klustret essentials rutan hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="eb356-189">For Azure clusters, hello full URL is also available in hello cluster essentials pane of hello Azure portal.</span></span>

### <a name="connect-tooa-secure-cluster"></a><span data-ttu-id="eb356-190">Ansluta tooa säker kluster</span><span class="sxs-lookup"><span data-stu-id="eb356-190">Connect tooa secure cluster</span></span>
<span data-ttu-id="eb356-191">Du kan styra klienten åtkomst tooyour Service Fabric-kluster med certifikat eller med hjälp av Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="eb356-191">You can control client access tooyour Service Fabric cluster either with certificates or using Azure Active Directory (AAD).</span></span>

<span data-ttu-id="eb356-192">Om du försöker tooconnect tooService Fabric-Utforskaren på en säker klustret sedan beroende på hello klusterkonfigurationen ska du vara nödvändiga toopresent ett klientcertifikat eller logga in med AAD.</span><span class="sxs-lookup"><span data-stu-id="eb356-192">If you attempt tooconnect tooService Fabric Explorer on a secure cluster, then depending on hello cluster's configuration you'll be required toopresent a client certificate or log in using AAD.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eb356-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eb356-193">Next steps</span></span>
* [<span data-ttu-id="eb356-194">Möjlighet att testa översikt</span><span class="sxs-lookup"><span data-stu-id="eb356-194">Testability overview</span></span>](service-fabric-testability-overview.md)
* [<span data-ttu-id="eb356-195">Hantera dina Service Fabric-program i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eb356-195">Managing your Service Fabric applications in Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)
* [<span data-ttu-id="eb356-196">Service Fabric-programdistribution med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="eb356-196">Service Fabric application deployment using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
