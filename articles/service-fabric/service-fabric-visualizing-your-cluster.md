---
title: "Visualisera ditt kluster med hjälp av Service Fabric Explorer | Microsoft Docs"
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
ms.openlocfilehash: 789793a7f50170188d688881a9178546c3074018
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a><span data-ttu-id="e6842-103">Visualisera ditt kluster med Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="e6842-103">Visualize your cluster with Service Fabric Explorer</span></span>
<span data-ttu-id="e6842-104">Service Fabric Explorer är ett webbaserat verktyg för att kontrollera och hantera program och noderna i Azure Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="e6842-104">Service Fabric Explorer is a web-based tool for inspecting and managing applications and nodes in an Azure Service Fabric cluster.</span></span> <span data-ttu-id="e6842-105">Service Fabric Explorer ligger direkt på klustret, så att det alltid är tillgängliga, oavsett om klustret körs.</span><span class="sxs-lookup"><span data-stu-id="e6842-105">Service Fabric Explorer is hosted directly within the cluster, so it is always available, regardless of where your cluster is running.</span></span>

## <a name="video-tutorial"></a><span data-ttu-id="e6842-106">Videosjälvstudie</span><span class="sxs-lookup"><span data-stu-id="e6842-106">Video tutorial</span></span>

<span data-ttu-id="e6842-107">Information om hur du använder Service Fabric Explorer, se följande Microsoft Virtual Academy video:</span><span class="sxs-lookup"><span data-stu-id="e6842-107">To learn how to use Service Fabric Explorer, watch the following Microsoft Virtual Academy video:</span></span>

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-to-service-fabric-explorer"></a><span data-ttu-id="e6842-108">Ansluta till Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="e6842-108">Connect to Service Fabric Explorer</span></span>
<span data-ttu-id="e6842-109">Om du har följt anvisningarna för att [förbereda din utvecklingsmiljö](service-fabric-get-started.md), kan du starta Service Fabric-Utforskaren på din lokala klustret genom att gå till http://localhost:19080/Explorer.</span><span class="sxs-lookup"><span data-stu-id="e6842-109">If you have followed the instructions to [prepare your development environment](service-fabric-get-started.md), you can launch Service Fabric Explorer on your local cluster by navigating to http://localhost:19080/Explorer.</span></span>

## <a name="understand-the-service-fabric-explorer-layout"></a><span data-ttu-id="e6842-110">Förstå Service Fabric Explorer-layout</span><span class="sxs-lookup"><span data-stu-id="e6842-110">Understand the Service Fabric Explorer layout</span></span>
<span data-ttu-id="e6842-111">Du kan bläddra igenom Service Fabric Explorer med hjälp av trädet till vänster.</span><span class="sxs-lookup"><span data-stu-id="e6842-111">You can navigate through Service Fabric Explorer by using the tree on the left.</span></span> <span data-ttu-id="e6842-112">Roten i trädet innehåller en översikt över klustret, inklusive en sammanfattning av program och noden hälsa i instrumentpanelen för klustret.</span><span class="sxs-lookup"><span data-stu-id="e6842-112">At the root of the tree, the cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span>

![Instrumentpanelen för Service Fabric Explorer-klustret][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a><span data-ttu-id="e6842-114">Visa klustrets layout</span><span class="sxs-lookup"><span data-stu-id="e6842-114">View the cluster's layout</span></span>
<span data-ttu-id="e6842-115">Noder i ett Service Fabric-kluster är placerade över en tvådimensionell rutnät för feldomäner och uppgradera domäner.</span><span class="sxs-lookup"><span data-stu-id="e6842-115">Nodes in a Service Fabric cluster are placed across a two-dimensional grid of fault domains and upgrade domains.</span></span> <span data-ttu-id="e6842-116">Den här placering garanterar att dina program finns tillgängliga med maskinvarufel och programuppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="e6842-116">This placement ensures that your applications remain available in the presence of hardware failures and application upgrades.</span></span> <span data-ttu-id="e6842-117">Du kan se hur det aktuella klustret är placerade med hjälp av listan över lediga kluster.</span><span class="sxs-lookup"><span data-stu-id="e6842-117">You can view how the current cluster is laid out by using the cluster map.</span></span>

![Service Fabric Explorer över lediga kluster][sfx-cluster-map]

### <a name="view-applications-and-services"></a><span data-ttu-id="e6842-119">Visa program och tjänster</span><span class="sxs-lookup"><span data-stu-id="e6842-119">View applications and services</span></span>
<span data-ttu-id="e6842-120">Klustret innehåller två underträd: en för program och en annan för noder.</span><span class="sxs-lookup"><span data-stu-id="e6842-120">The cluster contains two subtrees: one for applications and another for nodes.</span></span>

<span data-ttu-id="e6842-121">Du kan använda vyn program kan navigera i Service Fabric logiska hierarkin: program, tjänster, partitioner och repliker.</span><span class="sxs-lookup"><span data-stu-id="e6842-121">You can use the application view to navigate through Service Fabric's logical hierarchy: applications, services, partitions, and replicas.</span></span>

<span data-ttu-id="e6842-122">I exemplet nedan programmet **MyApp** består av två tjänster **MyStatefulService** och **WebService**.</span><span class="sxs-lookup"><span data-stu-id="e6842-122">In the example below, the application **MyApp** consists of two services, **MyStatefulService** and **WebService**.</span></span> <span data-ttu-id="e6842-123">Eftersom **MyStatefulService** är tillståndskänslig så innehåller en partition med en primär och två sekundära repliker.</span><span class="sxs-lookup"><span data-stu-id="e6842-123">Since **MyStatefulService** is stateful, it includes a partition with one primary and two secondary replicas.</span></span> <span data-ttu-id="e6842-124">Däremot WebSvcService är tillståndslösa och innehåller en enda instans.</span><span class="sxs-lookup"><span data-stu-id="e6842-124">By contrast, WebSvcService is stateless and contains a single instance.</span></span>

![Service Fabric Explorer programvy][sfx-application-tree]

<span data-ttu-id="e6842-126">På varje nivå i trädet visar i huvudfönstret viktig information om objektet.</span><span class="sxs-lookup"><span data-stu-id="e6842-126">At each level of the tree, the main pane shows pertinent information about the item.</span></span> <span data-ttu-id="e6842-127">Du kan till exempel se hälsostatus och version för en viss tjänst.</span><span class="sxs-lookup"><span data-stu-id="e6842-127">For example, you can see the health status and version for a particular service.</span></span>

![Service Fabric Explorer essentials fönstret][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a><span data-ttu-id="e6842-129">Visa noder i klustret</span><span class="sxs-lookup"><span data-stu-id="e6842-129">View the cluster's nodes</span></span>
<span data-ttu-id="e6842-130">Nodvyn visar klustrets fysiska layout.</span><span class="sxs-lookup"><span data-stu-id="e6842-130">The node view shows the physical layout of the cluster.</span></span> <span data-ttu-id="e6842-131">För en viss nod kan du inspektera vilka program som har kod distribuerad på noden.</span><span class="sxs-lookup"><span data-stu-id="e6842-131">For a given node, you can inspect which applications have code deployed on that node.</span></span> <span data-ttu-id="e6842-132">Mer specifikt kan du se vilka repliker körs det.</span><span class="sxs-lookup"><span data-stu-id="e6842-132">More specifically, you can see which replicas are currently running there.</span></span>

## <a name="actions"></a><span data-ttu-id="e6842-133">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="e6842-133">Actions</span></span>
<span data-ttu-id="e6842-134">Service Fabric Explorer erbjuder ett snabbt sätt att anropa åtgärder på noder, program och tjänster inom klustret.</span><span class="sxs-lookup"><span data-stu-id="e6842-134">Service Fabric Explorer offers a quick way to invoke actions on nodes, applications, and services within your cluster.</span></span>

<span data-ttu-id="e6842-135">Exempelvis för att ta bort en instans av programmet, Välj programmet från trädet till vänster och välj sedan **åtgärder** > **ta bort programmet**.</span><span class="sxs-lookup"><span data-stu-id="e6842-135">For example, to delete an application instance, choose the application from the tree on the left, and then choose **Actions** > **Delete Application**.</span></span>

![Om du tar bort ett program i Service Fabric Explorer][sfx-delete-application]

> [!TIP]
> <span data-ttu-id="e6842-137">Du kan utföra samma åtgärder genom att klicka på knappen bredvid varje element.</span><span class="sxs-lookup"><span data-stu-id="e6842-137">You can perform the same actions by clicking the ellipsis next to each element.</span></span>
>
>

<span data-ttu-id="e6842-138">I följande tabell visas åtgärderna som är tillgängliga för varje entitet:</span><span class="sxs-lookup"><span data-stu-id="e6842-138">The following table lists the actions available for each entity:</span></span>

| <span data-ttu-id="e6842-139">**Entitet**</span><span class="sxs-lookup"><span data-stu-id="e6842-139">**Entity**</span></span> | <span data-ttu-id="e6842-140">**Åtgärd**</span><span class="sxs-lookup"><span data-stu-id="e6842-140">**Action**</span></span> | <span data-ttu-id="e6842-141">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="e6842-141">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e6842-142">Programtyp</span><span class="sxs-lookup"><span data-stu-id="e6842-142">Application type</span></span> |<span data-ttu-id="e6842-143">Avetablera typ</span><span class="sxs-lookup"><span data-stu-id="e6842-143">Unprovision type</span></span> |<span data-ttu-id="e6842-144">Tar bort programpaketet från avbildningsarkivet i klustret.</span><span class="sxs-lookup"><span data-stu-id="e6842-144">Removes the application package from the cluster's image store.</span></span> <span data-ttu-id="e6842-145">Kräver att alla program av den typen som ska tas bort först.</span><span class="sxs-lookup"><span data-stu-id="e6842-145">Requires all applications of that type to be removed first.</span></span> |
| <span data-ttu-id="e6842-146">Program</span><span class="sxs-lookup"><span data-stu-id="e6842-146">Application</span></span> |<span data-ttu-id="e6842-147">Ta bort program</span><span class="sxs-lookup"><span data-stu-id="e6842-147">Delete Application</span></span> |<span data-ttu-id="e6842-148">Ta bort program, inklusive alla tjänster och deras tillstånd (eventuella).</span><span class="sxs-lookup"><span data-stu-id="e6842-148">Delete the application, including all its services and their state (if any).</span></span> |
| <span data-ttu-id="e6842-149">Tjänst</span><span class="sxs-lookup"><span data-stu-id="e6842-149">Service</span></span> |<span data-ttu-id="e6842-150">Ta bort tjänsten</span><span class="sxs-lookup"><span data-stu-id="e6842-150">Delete Service</span></span> |<span data-ttu-id="e6842-151">Ta bort tjänsten och dess tillstånd (eventuella).</span><span class="sxs-lookup"><span data-stu-id="e6842-151">Delete the service and its state (if any).</span></span> |
| <span data-ttu-id="e6842-152">Node</span><span class="sxs-lookup"><span data-stu-id="e6842-152">Node</span></span> |<span data-ttu-id="e6842-153">Aktivera</span><span class="sxs-lookup"><span data-stu-id="e6842-153">Activate</span></span> |<span data-ttu-id="e6842-154">Aktivera noden.</span><span class="sxs-lookup"><span data-stu-id="e6842-154">Activate the node.</span></span> |
| <span data-ttu-id="e6842-155">Node</span><span class="sxs-lookup"><span data-stu-id="e6842-155">Node</span></span> | <span data-ttu-id="e6842-156">Inaktivera (paus)</span><span class="sxs-lookup"><span data-stu-id="e6842-156">Deactivate (pause)</span></span> | <span data-ttu-id="e6842-157">Pausa noden i det aktuella tillståndet.</span><span class="sxs-lookup"><span data-stu-id="e6842-157">Pause the node in its current state.</span></span> <span data-ttu-id="e6842-158">Tjänster fortsätter att köras men Service Fabric flyttas proaktivt inte något på eller inaktivera den om det är nödvändigt för att förhindra att en inkonsekvens nätverksavbrott eller om data.</span><span class="sxs-lookup"><span data-stu-id="e6842-158">Services continue to run but Service Fabric does not proactively move anything onto or off it unless it is required to prevent an outage or data inconsistency.</span></span> <span data-ttu-id="e6842-159">Den här åtgärden används vanligtvis för att aktivera felsökning services på en viss nod så att de inte flytta under kontroll.</span><span class="sxs-lookup"><span data-stu-id="e6842-159">This action is typically used to enable debugging services on a specific node to ensure that they do not move during inspection.</span></span> | |
| <span data-ttu-id="e6842-160">Node</span><span class="sxs-lookup"><span data-stu-id="e6842-160">Node</span></span> | <span data-ttu-id="e6842-161">Inaktivera (omstart)</span><span class="sxs-lookup"><span data-stu-id="e6842-161">Deactivate (restart)</span></span> | <span data-ttu-id="e6842-162">Flytta alla InMemory-tjänster av en nod och Stäng beständiga tjänster på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="e6842-162">Safely move all in-memory services off a node and close persistent services.</span></span> <span data-ttu-id="e6842-163">Används vanligtvis när värdprocesser eller datorn måste startas om.</span><span class="sxs-lookup"><span data-stu-id="e6842-163">Typically used when the host processes or machine need to be restarted.</span></span> | |
| <span data-ttu-id="e6842-164">Node</span><span class="sxs-lookup"><span data-stu-id="e6842-164">Node</span></span> | <span data-ttu-id="e6842-165">Inaktivera (ta bort data)</span><span class="sxs-lookup"><span data-stu-id="e6842-165">Deactivate (remove data)</span></span> | <span data-ttu-id="e6842-166">Stäng alla tjänster som körs på noden när du har skapat tillräckligt ledig repliker på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="e6842-166">Safely close all services running on the node after building sufficient spare replicas.</span></span> <span data-ttu-id="e6842-167">Används vanligtvis när en nod (eller åtminstone dess lagring) som permanent tas utanför kommissionen.</span><span class="sxs-lookup"><span data-stu-id="e6842-167">Typically used when a node (or at least its storage) is being permanently taken out of commission.</span></span> | |
| <span data-ttu-id="e6842-168">Node</span><span class="sxs-lookup"><span data-stu-id="e6842-168">Node</span></span> | <span data-ttu-id="e6842-169">Ta bort nodens tillstånd</span><span class="sxs-lookup"><span data-stu-id="e6842-169">Remove node state</span></span> | <span data-ttu-id="e6842-170">Ta bort kunskap om repliker för en nod från klustret.</span><span class="sxs-lookup"><span data-stu-id="e6842-170">Remove knowledge of a node's replicas from the cluster.</span></span> <span data-ttu-id="e6842-171">Används vanligtvis när en redan felaktiga noden bedöms oåterkalleligt.</span><span class="sxs-lookup"><span data-stu-id="e6842-171">Typically used when an already failed node is deemed unrecoverable.</span></span> | |
| <span data-ttu-id="e6842-172">Node</span><span class="sxs-lookup"><span data-stu-id="e6842-172">Node</span></span> | <span data-ttu-id="e6842-173">Starta om</span><span class="sxs-lookup"><span data-stu-id="e6842-173">Restart</span></span> | <span data-ttu-id="e6842-174">Simulera ett nodfel genom att starta om noden.</span><span class="sxs-lookup"><span data-stu-id="e6842-174">Simulate a node failure by restarting the node.</span></span> <span data-ttu-id="e6842-175">Mer information [här](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="e6842-175">More information [here](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps)</span></span> | |

<span data-ttu-id="e6842-176">Eftersom många åtgärder är skadliga, kan du bli ombedd att bekräfta din avsikt innan åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="e6842-176">Since many actions are destructive, you may be asked to confirm your intent before the action is completed.</span></span>

> [!TIP]
> <span data-ttu-id="e6842-177">Varje åtgärd som kan utföras via Service Fabric Explorer kan också utföras via PowerShell eller REST-API för att aktivera automatisering.</span><span class="sxs-lookup"><span data-stu-id="e6842-177">Every action that can be performed through Service Fabric Explorer can also be performed through PowerShell or a REST API, to enable automation.</span></span>
>
>

<span data-ttu-id="e6842-178">Du kan också använda Service Fabric Explorer för att skapa instanser av programmet för en viss programtypen och versionen.</span><span class="sxs-lookup"><span data-stu-id="e6842-178">You can also use Service Fabric Explorer to create application instances for a given application type and version.</span></span> <span data-ttu-id="e6842-179">Välj programtyp av i trädvyn och klicka sedan på den **skapa app-instansen** länken bredvid den version som du vill ha i den högra rutan.</span><span class="sxs-lookup"><span data-stu-id="e6842-179">Choose the application type in the tree view, then click the **Create app instance** link next to the version you'd like in the right pane.</span></span>

![Skapa en programinstans i Service Fabric Explorer][sfx-create-app-instance]

> [!NOTE]
> <span data-ttu-id="e6842-181">Programinstanser som skapats via Service Fabric Explorer kan för närvarande parameteriseras.</span><span class="sxs-lookup"><span data-stu-id="e6842-181">Application instances created through Service Fabric Explorer cannot currently be parameterized.</span></span> <span data-ttu-id="e6842-182">De skapas med hjälp av standardparametervärden.</span><span class="sxs-lookup"><span data-stu-id="e6842-182">They are created using default parameter values.</span></span>
>
>

## <a name="connect-to-a-remote-service-fabric-cluster"></a><span data-ttu-id="e6842-183">Ansluta till en fjärransluten Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="e6842-183">Connect to a remote Service Fabric cluster</span></span>
<span data-ttu-id="e6842-184">Om du känner klustrets slutpunkt och den behörighet som du har åtkomst till Service Fabric Explorer från en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="e6842-184">If you know the cluster's endpoint and have sufficient permissions you can access Service Fabric Explorer from any browser.</span></span> <span data-ttu-id="e6842-185">Detta beror på att Service Fabric Explorer är en tjänst som körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="e6842-185">This is because Service Fabric Explorer is just another service that runs in the cluster.</span></span>

### <a name="discover-the-service-fabric-explorer-endpoint-for-a-remote-cluster"></a><span data-ttu-id="e6842-186">Identifiera Service Fabric Explorer-slutpunkt för ett kluster</span><span class="sxs-lookup"><span data-stu-id="e6842-186">Discover the Service Fabric Explorer endpoint for a remote cluster</span></span>
<span data-ttu-id="e6842-187">Peka webbläsaren för att nå Service Fabric Explorer för ett kluster:</span><span class="sxs-lookup"><span data-stu-id="e6842-187">To reach Service Fabric Explorer for a given cluster, point your browser to:</span></span>

<span data-ttu-id="e6842-188">http://&lt;din klusterslutpunkten&gt;: 19080/Explorer</span><span class="sxs-lookup"><span data-stu-id="e6842-188">http://&lt;your-cluster-endpoint&gt;:19080/Explorer</span></span>

<span data-ttu-id="e6842-189">För Azure-kluster finns också hela Webbadressen i rutan klustret essentials i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e6842-189">For Azure clusters, the full URL is also available in the cluster essentials pane of the Azure portal.</span></span>

### <a name="connect-to-a-secure-cluster"></a><span data-ttu-id="e6842-190">Ansluta till ett säkert kluster</span><span class="sxs-lookup"><span data-stu-id="e6842-190">Connect to a secure cluster</span></span>
<span data-ttu-id="e6842-191">Du kan kontrollera klientåtkomst till Service Fabric-kluster med certifikat eller med hjälp av Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="e6842-191">You can control client access to your Service Fabric cluster either with certificates or using Azure Active Directory (AAD).</span></span>

<span data-ttu-id="e6842-192">Om du försöker ansluta till Service Fabric-Utforskaren på ett säkert kluster ska sedan beroende på klustrets konfiguration du behöva ange ett klientcertifikat eller logga in med AAD.</span><span class="sxs-lookup"><span data-stu-id="e6842-192">If you attempt to connect to Service Fabric Explorer on a secure cluster, then depending on the cluster's configuration you'll be required to present a client certificate or log in using AAD.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6842-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6842-193">Next steps</span></span>
* [<span data-ttu-id="e6842-194">Möjlighet att testa översikt</span><span class="sxs-lookup"><span data-stu-id="e6842-194">Testability overview</span></span>](service-fabric-testability-overview.md)
* [<span data-ttu-id="e6842-195">Hantera dina Service Fabric-program i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6842-195">Managing your Service Fabric applications in Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)
* [<span data-ttu-id="e6842-196">Service Fabric-programdistribution med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6842-196">Service Fabric application deployment using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
