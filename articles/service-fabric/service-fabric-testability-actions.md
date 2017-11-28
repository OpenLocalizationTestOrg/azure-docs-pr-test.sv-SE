---
title: "aaaSimulate fel i Azure mikrotjänster | Microsoft Docs"
description: "Den här artikeln handlar om hello datatillgång åtgärder finns i Microsoft Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: motanv
manager: timlt
editor: toddabel
ms.assetid: ed53ca5c-4d5e-4b48-93c9-e386f32d8b7a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv;heeldin
ms.openlocfilehash: 5bdda1c0c5a40b243ab956c4791afd52e11c4089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="testability-actions"></a><span data-ttu-id="32a8d-103">Möjlighet att testa åtgärder</span><span class="sxs-lookup"><span data-stu-id="32a8d-103">Testability actions</span></span>
<span data-ttu-id="32a8d-104">I ordning toosimulate en instabilt infrastruktur ger Azure Service Fabric du hello utvecklare med sätt toosimulate olika verkliga fel och tillståndsövergångar.</span><span class="sxs-lookup"><span data-stu-id="32a8d-104">In order toosimulate an unreliable infrastructure, Azure Service Fabric provides you, hello developer, with ways toosimulate various real-world failures and state transitions.</span></span> <span data-ttu-id="32a8d-105">Dessa visas som datatillgång åtgärder.</span><span class="sxs-lookup"><span data-stu-id="32a8d-105">These are exposed as testability actions.</span></span> <span data-ttu-id="32a8d-106">hello åtgärder är hello lågnivå-API: er som orsakar en viss feltolerans injection, tillståndsövergång eller validering.</span><span class="sxs-lookup"><span data-stu-id="32a8d-106">hello actions are hello low-level APIs that cause a specific fault injection, state transition, or validation.</span></span> <span data-ttu-id="32a8d-107">Du kan skriva omfattande testscenarier för dina tjänster genom att kombinera dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="32a8d-107">By combining these actions, you can write comprehensive test scenarios for your services.</span></span>

<span data-ttu-id="32a8d-108">Service Fabric innehåller några vanliga testscenarier består av dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="32a8d-108">Service Fabric provides some common test scenarios composed of these actions.</span></span> <span data-ttu-id="32a8d-109">Vi rekommenderar starkt att du använder dessa inbyggda scenarier som väljs noggrant tootest vanliga tillståndsövergångar och fel fall.</span><span class="sxs-lookup"><span data-stu-id="32a8d-109">We highly recommend that you utilize these built-in scenarios, which are carefully chosen tootest common state transitions and failure cases.</span></span> <span data-ttu-id="32a8d-110">Åtgärder kan dock använda toocreate anpassade testscenarier när du vill tooadd täckning för scenarier som inte omfattas av inbyggda hello-scenarier ännu eller som är anpassade skräddarsydda för ditt program.</span><span class="sxs-lookup"><span data-stu-id="32a8d-110">However, actions can be used toocreate custom test scenarios when you want tooadd coverage for scenarios that are not covered by hello built-in scenarios yet or that are custom tailored for your application.</span></span>

<span data-ttu-id="32a8d-111">C#-implementeringar av hello åtgärder finns i hello System.Fabric.dll sammansättning.</span><span class="sxs-lookup"><span data-stu-id="32a8d-111">C# implementations of hello actions are found in hello System.Fabric.dll assembly.</span></span> <span data-ttu-id="32a8d-112">hello System Fabric PowerShell-modulen finns i hello Microsoft.ServiceFabric.Powershell.dll sammansättning.</span><span class="sxs-lookup"><span data-stu-id="32a8d-112">hello System Fabric PowerShell module is found in hello Microsoft.ServiceFabric.Powershell.dll assembly.</span></span> <span data-ttu-id="32a8d-113">Som en del av runtime-installation är hello ServiceFabric PowerShell-modulen installerad tooallow lätt att använda.</span><span class="sxs-lookup"><span data-stu-id="32a8d-113">As part of runtime installation, hello ServiceFabric PowerShell module is installed tooallow for ease of use.</span></span>

## <a name="graceful-vs-ungraceful-fault-actions"></a><span data-ttu-id="32a8d-114">Korrekt kontra städat fel åtgärder</span><span class="sxs-lookup"><span data-stu-id="32a8d-114">Graceful vs. ungraceful fault actions</span></span>
<span data-ttu-id="32a8d-115">Möjlighet att testa åtgärder indelas i två huvudsakliga buckets.</span><span class="sxs-lookup"><span data-stu-id="32a8d-115">Testability actions are classified into two major buckets:</span></span>

* <span data-ttu-id="32a8d-116">Städat fel: dessa fel simulera fel som omstarter av datorn och krascher.</span><span class="sxs-lookup"><span data-stu-id="32a8d-116">Ungraceful faults: These faults simulate failures like machine restarts and process crashes.</span></span> <span data-ttu-id="32a8d-117">I sådana fall fel hello körningskontexten processens plötsligt.</span><span class="sxs-lookup"><span data-stu-id="32a8d-117">In such cases of failures, hello execution context of process stops abruptly.</span></span> <span data-ttu-id="32a8d-118">Detta innebär att ingen rensning av hello tillstånd kan köra innan programmet hello startas igen.</span><span class="sxs-lookup"><span data-stu-id="32a8d-118">This means no cleanup of hello state can run before hello application starts up again.</span></span>
* <span data-ttu-id="32a8d-119">Korrekt fel: dessa fel simulera korrekt åtgärder som flyttar replik och således som utlösts av belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="32a8d-119">Graceful faults: These faults simulate graceful actions like replica moves and drops triggered by load balancing.</span></span> <span data-ttu-id="32a8d-120">I sådana fall hello tjänsten hämtar ett meddelande om hello Stäng och kan rensa hello tillstånd innan du avslutar.</span><span class="sxs-lookup"><span data-stu-id="32a8d-120">In such cases, hello service gets a notification of hello close and can clean up hello state before exiting.</span></span>

<span data-ttu-id="32a8d-121">För bättre kvalitet verifiering köra hello-tjänsten och företag arbetsbelastning när att olika korrekt och städat fel.</span><span class="sxs-lookup"><span data-stu-id="32a8d-121">For better quality validation, run hello service and business workload while inducing various graceful and ungraceful faults.</span></span> <span data-ttu-id="32a8d-122">Städat fel utöva scenarier där hello tjänstprocessen plötsligt avslutas hello mitten av vissa arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="32a8d-122">Ungraceful faults exercise scenarios where hello service process abruptly exits in hello middle of some workflow.</span></span> <span data-ttu-id="32a8d-123">Detta testar hello Återställningssökväg när hello service replikeringen har återställts av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="32a8d-123">This tests  hello recovery path once hello service replica is restored by Service Fabric.</span></span> <span data-ttu-id="32a8d-124">Detta hjälper testa datakonsekvens och huruvida hello Tjänststatus behålls korrekt efter fel.</span><span class="sxs-lookup"><span data-stu-id="32a8d-124">This will help test data consistency and whether hello service state is maintained correctly after failures.</span></span> <span data-ttu-id="32a8d-125">hello andra uppsättning fel (hello korrekt misslyckanden) testa att hello service korrekt reagerar tooreplicas flyttas runt av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="32a8d-125">hello other set of failures (hello graceful failures) test that hello service correctly reacts tooreplicas being moved around by Service Fabric.</span></span> <span data-ttu-id="32a8d-126">Detta testar hantering av annullering i hello RunAsync metoden.</span><span class="sxs-lookup"><span data-stu-id="32a8d-126">This tests handling of cancellation in hello RunAsync method.</span></span> <span data-ttu-id="32a8d-127">hello-tjänsten måste toocheck för hello annullering token som angetts korrekt spara sitt tillstånd och avsluta hello RunAsync metoden.</span><span class="sxs-lookup"><span data-stu-id="32a8d-127">hello service needs toocheck for hello cancellation token being set, correctly save its state, and exit hello RunAsync method.</span></span>

## <a name="testability-actions-list"></a><span data-ttu-id="32a8d-128">Möjlighet att testa åtgärdslista</span><span class="sxs-lookup"><span data-stu-id="32a8d-128">Testability actions list</span></span>
| <span data-ttu-id="32a8d-129">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="32a8d-129">Action</span></span> | <span data-ttu-id="32a8d-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="32a8d-130">Description</span></span> | <span data-ttu-id="32a8d-131">Hanterade API</span><span class="sxs-lookup"><span data-stu-id="32a8d-131">Managed API</span></span> | <span data-ttu-id="32a8d-132">PowerShell-cmdlet</span><span class="sxs-lookup"><span data-stu-id="32a8d-132">PowerShell cmdlet</span></span> | <span data-ttu-id="32a8d-133">Korrekt/städat fel</span><span class="sxs-lookup"><span data-stu-id="32a8d-133">Graceful/ungraceful faults</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="32a8d-134">CleanTestState</span><span class="sxs-lookup"><span data-stu-id="32a8d-134">CleanTestState</span></span> |<span data-ttu-id="32a8d-135">Tar bort alla hello testtillstånd från hello kluster vid en felaktig avstängning av hello test-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="32a8d-135">Removes all hello test state from hello cluster in case of a bad shutdown of hello test driver.</span></span> |<span data-ttu-id="32a8d-136">CleanTestStateAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-136">CleanTestStateAsync</span></span> |<span data-ttu-id="32a8d-137">Remove-ServiceFabricTestState</span><span class="sxs-lookup"><span data-stu-id="32a8d-137">Remove-ServiceFabricTestState</span></span> |<span data-ttu-id="32a8d-138">Inte tillämpligt</span><span class="sxs-lookup"><span data-stu-id="32a8d-138">Not applicable</span></span> |
| <span data-ttu-id="32a8d-139">InvokeDataLoss</span><span class="sxs-lookup"><span data-stu-id="32a8d-139">InvokeDataLoss</span></span> |<span data-ttu-id="32a8d-140">Startar förlust av data i en partition med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="32a8d-140">Induces data loss into a service partition.</span></span> |<span data-ttu-id="32a8d-141">InvokeDataLossAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-141">InvokeDataLossAsync</span></span> |<span data-ttu-id="32a8d-142">Invoke-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="32a8d-142">Invoke-ServiceFabricPartitionDataLoss</span></span> |<span data-ttu-id="32a8d-143">Korrekt</span><span class="sxs-lookup"><span data-stu-id="32a8d-143">Graceful</span></span> |
| <span data-ttu-id="32a8d-144">InvokeQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="32a8d-144">InvokeQuorumLoss</span></span> |<span data-ttu-id="32a8d-145">Placerar en given tillståndskänslig service partition i förlorar kvorum.</span><span class="sxs-lookup"><span data-stu-id="32a8d-145">Puts a given stateful service partition into quorum loss.</span></span> |<span data-ttu-id="32a8d-146">InvokeQuorumLossAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-146">InvokeQuorumLossAsync</span></span> |<span data-ttu-id="32a8d-147">Anropa ServiceFabricQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="32a8d-147">Invoke-ServiceFabricQuorumLoss</span></span> |<span data-ttu-id="32a8d-148">Korrekt</span><span class="sxs-lookup"><span data-stu-id="32a8d-148">Graceful</span></span> |
| <span data-ttu-id="32a8d-149">Flytta primära</span><span class="sxs-lookup"><span data-stu-id="32a8d-149">Move Primary</span></span> |<span data-ttu-id="32a8d-150">Flyttar hello angetts primära repliken av en tillståndskänslig service toohello angivna klusternoden.</span><span class="sxs-lookup"><span data-stu-id="32a8d-150">Moves hello specified primary replica of a stateful service toohello specified cluster node.</span></span> |<span data-ttu-id="32a8d-151">MovePrimaryAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-151">MovePrimaryAsync</span></span> |<span data-ttu-id="32a8d-152">Flytta ServiceFabricPrimaryReplica</span><span class="sxs-lookup"><span data-stu-id="32a8d-152">Move-ServiceFabricPrimaryReplica</span></span> |<span data-ttu-id="32a8d-153">Korrekt</span><span class="sxs-lookup"><span data-stu-id="32a8d-153">Graceful</span></span> |
| <span data-ttu-id="32a8d-154">Flytta sekundär</span><span class="sxs-lookup"><span data-stu-id="32a8d-154">Move Secondary</span></span> |<span data-ttu-id="32a8d-155">Flyttar hello aktuella sekundär replik av en tillståndskänslig service tooa annan klusternod.</span><span class="sxs-lookup"><span data-stu-id="32a8d-155">Moves hello current secondary replica of a stateful service tooa different cluster node.</span></span> |<span data-ttu-id="32a8d-156">MoveSecondaryAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-156">MoveSecondaryAsync</span></span> |<span data-ttu-id="32a8d-157">Flytta ServiceFabricSecondaryReplica</span><span class="sxs-lookup"><span data-stu-id="32a8d-157">Move-ServiceFabricSecondaryReplica</span></span> |<span data-ttu-id="32a8d-158">Korrekt</span><span class="sxs-lookup"><span data-stu-id="32a8d-158">Graceful</span></span> |
| <span data-ttu-id="32a8d-159">RemoveReplica</span><span class="sxs-lookup"><span data-stu-id="32a8d-159">RemoveReplica</span></span> |<span data-ttu-id="32a8d-160">Simulerar ett replik fel genom att ta bort en replik från ett kluster.</span><span class="sxs-lookup"><span data-stu-id="32a8d-160">Simulates a replica failure by removing a replica from a cluster.</span></span> <span data-ttu-id="32a8d-161">Detta kommer att stängas hello replik och övergår det toorole None, ta bort dess status från hello kluster.</span><span class="sxs-lookup"><span data-stu-id="32a8d-161">This will close hello replica and will transition it toorole 'None', removing all of its state from hello cluster.</span></span> |<span data-ttu-id="32a8d-162">RemoveReplicaAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-162">RemoveReplicaAsync</span></span> |<span data-ttu-id="32a8d-163">Ta bort ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="32a8d-163">Remove-ServiceFabricReplica</span></span> |<span data-ttu-id="32a8d-164">Korrekt</span><span class="sxs-lookup"><span data-stu-id="32a8d-164">Graceful</span></span> |
| <span data-ttu-id="32a8d-165">RestartDeployedCodePackage</span><span class="sxs-lookup"><span data-stu-id="32a8d-165">RestartDeployedCodePackage</span></span> |<span data-ttu-id="32a8d-166">Simulerar ett fel i koden paketet genom att starta om en kodpaketet har distribuerats på en nod i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="32a8d-166">Simulates a code package process failure by restarting a code package deployed on a node in a cluster.</span></span> <span data-ttu-id="32a8d-167">Detta avbryter hello kod paketet processen, vilket startar om alla hello användaren service repliker i den här processen.</span><span class="sxs-lookup"><span data-stu-id="32a8d-167">This aborts hello code package process, which will restart all hello user service replicas hosted in that process.</span></span> |<span data-ttu-id="32a8d-168">RestartDeployedCodePackageAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-168">RestartDeployedCodePackageAsync</span></span> |<span data-ttu-id="32a8d-169">Starta om ServiceFabricDeployedCodePackage</span><span class="sxs-lookup"><span data-stu-id="32a8d-169">Restart-ServiceFabricDeployedCodePackage</span></span> |<span data-ttu-id="32a8d-170">Städat</span><span class="sxs-lookup"><span data-stu-id="32a8d-170">Ungraceful</span></span> |
| <span data-ttu-id="32a8d-171">RestartNode</span><span class="sxs-lookup"><span data-stu-id="32a8d-171">RestartNode</span></span> |<span data-ttu-id="32a8d-172">Simulerar ett nodfel för Service Fabric-kluster genom att starta om en nod.</span><span class="sxs-lookup"><span data-stu-id="32a8d-172">Simulates a Service Fabric cluster node failure by restarting a node.</span></span> |<span data-ttu-id="32a8d-173">RestartNodeAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-173">RestartNodeAsync</span></span> |<span data-ttu-id="32a8d-174">Starta om ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="32a8d-174">Restart-ServiceFabricNode</span></span> |<span data-ttu-id="32a8d-175">Städat</span><span class="sxs-lookup"><span data-stu-id="32a8d-175">Ungraceful</span></span> |
| <span data-ttu-id="32a8d-176">RestartPartition</span><span class="sxs-lookup"><span data-stu-id="32a8d-176">RestartPartition</span></span> |<span data-ttu-id="32a8d-177">Simulerar ett datacenter blackout eller kluster blackout scenario genom att starta om vissa eller alla repliker för en partition.</span><span class="sxs-lookup"><span data-stu-id="32a8d-177">Simulates a datacenter blackout or cluster blackout scenario by restarting some or all replicas of a partition.</span></span> |<span data-ttu-id="32a8d-178">RestartPartitionAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-178">RestartPartitionAsync</span></span> |<span data-ttu-id="32a8d-179">Restart-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="32a8d-179">Restart-ServiceFabricPartition</span></span> |<span data-ttu-id="32a8d-180">Korrekt</span><span class="sxs-lookup"><span data-stu-id="32a8d-180">Graceful</span></span> |
| <span data-ttu-id="32a8d-181">RestartReplica</span><span class="sxs-lookup"><span data-stu-id="32a8d-181">RestartReplica</span></span> |<span data-ttu-id="32a8d-182">Simulerar ett fel för repliken genom att starta om en beständiga replik i ett kluster, stänga hello replik och sedan öppna den igen.</span><span class="sxs-lookup"><span data-stu-id="32a8d-182">Simulates a replica failure by restarting a persisted replica in a cluster, closing hello replica and then reopening it.</span></span> |<span data-ttu-id="32a8d-183">RestartReplicaAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-183">RestartReplicaAsync</span></span> |<span data-ttu-id="32a8d-184">Starta om ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="32a8d-184">Restart-ServiceFabricReplica</span></span> |<span data-ttu-id="32a8d-185">Korrekt</span><span class="sxs-lookup"><span data-stu-id="32a8d-185">Graceful</span></span> |
| <span data-ttu-id="32a8d-186">Startnod</span><span class="sxs-lookup"><span data-stu-id="32a8d-186">StartNode</span></span> |<span data-ttu-id="32a8d-187">Startar en nod i ett kluster som har redan stoppats.</span><span class="sxs-lookup"><span data-stu-id="32a8d-187">Starts a node in a cluster that is already stopped.</span></span> |<span data-ttu-id="32a8d-188">StartNodeAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-188">StartNodeAsync</span></span> |<span data-ttu-id="32a8d-189">Start-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="32a8d-189">Start-ServiceFabricNode</span></span> |<span data-ttu-id="32a8d-190">Inte tillämpligt</span><span class="sxs-lookup"><span data-stu-id="32a8d-190">Not applicable</span></span> |
| <span data-ttu-id="32a8d-191">StopNode</span><span class="sxs-lookup"><span data-stu-id="32a8d-191">StopNode</span></span> |<span data-ttu-id="32a8d-192">Simulerar ett nodfel genom att stoppa en nod i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="32a8d-192">Simulates a node failure by stopping a node in a cluster.</span></span> <span data-ttu-id="32a8d-193">hello nod förblir ned förrän Startnod anropas.</span><span class="sxs-lookup"><span data-stu-id="32a8d-193">hello node will stay down until StartNode is called.</span></span> |<span data-ttu-id="32a8d-194">StopNodeAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-194">StopNodeAsync</span></span> |<span data-ttu-id="32a8d-195">Stop-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="32a8d-195">Stop-ServiceFabricNode</span></span> |<span data-ttu-id="32a8d-196">Städat</span><span class="sxs-lookup"><span data-stu-id="32a8d-196">Ungraceful</span></span> |
| <span data-ttu-id="32a8d-197">ValidateApplication</span><span class="sxs-lookup"><span data-stu-id="32a8d-197">ValidateApplication</span></span> |<span data-ttu-id="32a8d-198">Verifierar hello tillgänglighet och hälsotillståndet för alla Service Fabric-tjänster i ett program, vanligtvis efter att vissa fel till hello system.</span><span class="sxs-lookup"><span data-stu-id="32a8d-198">Validates hello availability and health of all Service Fabric services within an application, usually after inducing some fault into hello system.</span></span> |<span data-ttu-id="32a8d-199">ValidateApplicationAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-199">ValidateApplicationAsync</span></span> |<span data-ttu-id="32a8d-200">Testa ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="32a8d-200">Test-ServiceFabricApplication</span></span> |<span data-ttu-id="32a8d-201">Inte tillämpligt</span><span class="sxs-lookup"><span data-stu-id="32a8d-201">Not applicable</span></span> |
| <span data-ttu-id="32a8d-202">ValidateService</span><span class="sxs-lookup"><span data-stu-id="32a8d-202">ValidateService</span></span> |<span data-ttu-id="32a8d-203">Verifierar hello tillgänglighet och hälsotillståndet för ett Service Fabric-tjänsten, vanligtvis efter att vissa fel till hello system.</span><span class="sxs-lookup"><span data-stu-id="32a8d-203">Validates hello availability and health of a Service Fabric service, usually after inducing some fault into hello system.</span></span> |<span data-ttu-id="32a8d-204">ValidateServiceAsync</span><span class="sxs-lookup"><span data-stu-id="32a8d-204">ValidateServiceAsync</span></span> |<span data-ttu-id="32a8d-205">Testa ServiceFabricService</span><span class="sxs-lookup"><span data-stu-id="32a8d-205">Test-ServiceFabricService</span></span> |<span data-ttu-id="32a8d-206">Inte tillämpligt</span><span class="sxs-lookup"><span data-stu-id="32a8d-206">Not applicable</span></span> |

## <a name="running-a-testability-action-using-powershell"></a><span data-ttu-id="32a8d-207">Köra en datatillgång-åtgärd med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="32a8d-207">Running a testability action using PowerShell</span></span>
<span data-ttu-id="32a8d-208">De här självstudierna visar hur toorun en möjlighet att testa åtgärden med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="32a8d-208">This tutorial shows you how toorun a testability action by using PowerShell.</span></span> <span data-ttu-id="32a8d-209">Du får lära dig hur toorun en datatillgång-åtgärd mot en lokal (en-box)-kluster eller ett Azure-kluster.</span><span class="sxs-lookup"><span data-stu-id="32a8d-209">You will learn how toorun a testability action against a local (one-box) cluster or an Azure cluster.</span></span> <span data-ttu-id="32a8d-210">Microsoft.Fabric.Powershell.dll--hello Service Fabric PowerShell-modulen--installeras automatiskt när du installerar hello Microsoft Service Fabric MSI.</span><span class="sxs-lookup"><span data-stu-id="32a8d-210">Microsoft.Fabric.Powershell.dll--hello Service Fabric PowerShell module--is installed automatically when you install hello Microsoft Service Fabric MSI.</span></span> <span data-ttu-id="32a8d-211">hello modulen laddas automatiskt när du öppnar en PowerShell-kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="32a8d-211">hello module is loaded automatically when you open a PowerShell prompt.</span></span>

<span data-ttu-id="32a8d-212">Självstudiekurs segment:</span><span class="sxs-lookup"><span data-stu-id="32a8d-212">Tutorial segments:</span></span>

* [<span data-ttu-id="32a8d-213">Köra en åtgärd mot ett kluster med en ruta</span><span class="sxs-lookup"><span data-stu-id="32a8d-213">Run an action against a one-box cluster</span></span>](#run-an-action-against-a-one-box-cluster)
* [<span data-ttu-id="32a8d-214">Köra en åtgärd mot ett Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="32a8d-214">Run an action against an Azure cluster</span></span>](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a><span data-ttu-id="32a8d-215">Köra en åtgärd mot ett kluster med en ruta</span><span class="sxs-lookup"><span data-stu-id="32a8d-215">Run an action against a one-box cluster</span></span>
<span data-ttu-id="32a8d-216">toorun en datatillgång-åtgärd mot en lokala klustret först ansluta toohello klustret och öppna hello PowerShell-Kommandotolken i administratörsläge.</span><span class="sxs-lookup"><span data-stu-id="32a8d-216">toorun a testability action against a local cluster, first connect toohello cluster and open hello PowerShell prompt in administrator mode.</span></span> <span data-ttu-id="32a8d-217">Låt oss titta på hello **omstart ServiceFabricNode** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="32a8d-217">Let us look at hello **Restart-ServiceFabricNode** action.</span></span>

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

<span data-ttu-id="32a8d-218">Här hello åtgärd **omstart ServiceFabricNode** körs på en nod med namnet ”Nod1”.</span><span class="sxs-lookup"><span data-stu-id="32a8d-218">Here hello action **Restart-ServiceFabricNode** is being run on a node named "Node1".</span></span> <span data-ttu-id="32a8d-219">hello slutförande läge anger att den inte ska kontrollera om hello starta om noden åtgärden faktiskt har slutförts.</span><span class="sxs-lookup"><span data-stu-id="32a8d-219">hello completion mode specifies that it should not verify whether hello restart-node action actually succeeded.</span></span> <span data-ttu-id="32a8d-220">Att ange hello slutförande-läge som ”verifiera” innebär att den tooverify om hello omstart åtgärden faktiskt har slutförts.</span><span class="sxs-lookup"><span data-stu-id="32a8d-220">Specifying hello completion mode as "Verify" will cause it tooverify whether hello restart action actually succeeded.</span></span> <span data-ttu-id="32a8d-221">Istället för att ange hello noden direkt med namnet kan du ange den via en partition nyckel och hello typ av repliken, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="32a8d-221">Instead of directly specifying hello node by its name, you can specify it via a partition key and hello kind of replica, as follows:</span></span>

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

<span data-ttu-id="32a8d-222">**Starta om ServiceFabricNode** ska använda toorestart ett Service Fabric-nod i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="32a8d-222">**Restart-ServiceFabricNode** should be used toorestart a Service Fabric node in a cluster.</span></span> <span data-ttu-id="32a8d-223">Detta förhindrar hello Fabric.exe-processen, vilket startar om alla hello system-tjänsten och användaren service repliker på noden.</span><span class="sxs-lookup"><span data-stu-id="32a8d-223">This will stop hello Fabric.exe process, which will restart all of hello system service and user service replicas hosted on that node.</span></span> <span data-ttu-id="32a8d-224">Med den här API-tootest kan din tjänst upptäcka buggar längs hello failover återställning sökvägar.</span><span class="sxs-lookup"><span data-stu-id="32a8d-224">Using this API tootest your service helps uncover bugs along hello failover recovery paths.</span></span> <span data-ttu-id="32a8d-225">Det hjälper att simulera nodfel i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="32a8d-225">It helps simulate node failures in hello cluster.</span></span>

<span data-ttu-id="32a8d-226">hello följande skärmbild visar hello **omstart ServiceFabricNode** datatillgång kommandot i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="32a8d-226">hello following screenshot shows hello **Restart-ServiceFabricNode** testability command in action.</span></span>

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

<span data-ttu-id="32a8d-227">hello utdata av hello först **Get-ServiceFabricNode** (en cmdlet från hello Service Fabric PowerShell-modulen) visar den lokala hello-klustret har fem noder: Node.1 tooNode.5.</span><span class="sxs-lookup"><span data-stu-id="32a8d-227">hello output of hello first **Get-ServiceFabricNode** (a cmdlet from hello Service Fabric PowerShell module) shows that hello local cluster has five nodes: Node.1 tooNode.5.</span></span> <span data-ttu-id="32a8d-228">Efter hello datatillgång åtgärd (cmdlet) **omstart ServiceFabricNode** körs på hello nod med namnet Node.4 vi se hello nodens drifttid har återställts.</span><span class="sxs-lookup"><span data-stu-id="32a8d-228">After hello testability action (cmdlet) **Restart-ServiceFabricNode** is executed on hello node, named Node.4, we see that hello node's uptime has been reset.</span></span>

### <a name="run-an-action-against-an-azure-cluster"></a><span data-ttu-id="32a8d-229">Köra en åtgärd mot ett Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="32a8d-229">Run an action against an Azure cluster</span></span>
<span data-ttu-id="32a8d-230">Kör en datatillgång-åtgärd (med hjälp av PowerShell) mot ett Azure-kluster är liknande toorunning hello-åtgärd mot en lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="32a8d-230">Running a testability action (by using PowerShell) against an Azure cluster is similar toorunning hello action against a local cluster.</span></span> <span data-ttu-id="32a8d-231">hello endast skillnaden är att innan du kan köra hello-åtgärden, i stället för anslutande toohello lokala klustret och du behöver tooconnect toohello Azure kluster först.</span><span class="sxs-lookup"><span data-stu-id="32a8d-231">hello only difference is that before you can run hello action, instead of connecting toohello local cluster, you need tooconnect toohello Azure cluster first.</span></span>

## <a name="running-a-testability-action-using-c35"></a><span data-ttu-id="32a8d-232">Köra en datatillgång åtgärd med C & #35.</span><span class="sxs-lookup"><span data-stu-id="32a8d-232">Running a testability action using C&#35;</span></span>
<span data-ttu-id="32a8d-233">toorun en möjlighet att testa åtgärden med hjälp av C#, måste du först tooconnect toohello kluster med hjälp av FabricClient.</span><span class="sxs-lookup"><span data-stu-id="32a8d-233">toorun a testability action by using C#, first you need tooconnect toohello cluster by using FabricClient.</span></span> <span data-ttu-id="32a8d-234">Skaffa hello parametrar som behövs toorun hello åtgärd.</span><span class="sxs-lookup"><span data-stu-id="32a8d-234">Then obtain hello parameters needed toorun hello action.</span></span> <span data-ttu-id="32a8d-235">Olika parametrar som kan användas för toorun hello samma åtgärd.</span><span class="sxs-lookup"><span data-stu-id="32a8d-235">Different parameters can be used toorun hello same action.</span></span>
<span data-ttu-id="32a8d-236">Titta på hello RestartServiceFabricNode åtgärd, enkelriktade toorun är det med hjälp av noden hello information (nodnamnet och nod-instans-ID) i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="32a8d-236">Looking at hello RestartServiceFabricNode action, one way toorun it is by using hello node information (node name and node instance ID) in hello cluster.</span></span>

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

<span data-ttu-id="32a8d-237">Parametern förklaring:</span><span class="sxs-lookup"><span data-stu-id="32a8d-237">Parameter explanation:</span></span>

* <span data-ttu-id="32a8d-238">**CompleteMode** anger att hello-läge inte ska kontrollera om hello omstart åtgärden faktiskt har slutförts.</span><span class="sxs-lookup"><span data-stu-id="32a8d-238">**CompleteMode** specifies that hello mode should not verify whether hello restart action actually succeeded.</span></span> <span data-ttu-id="32a8d-239">Att ange hello slutförande-läge som ”verifiera” innebär att den tooverify om hello omstart åtgärden faktiskt har slutförts.</span><span class="sxs-lookup"><span data-stu-id="32a8d-239">Specifying hello completion mode as "Verify" will cause it tooverify whether hello restart action actually succeeded.</span></span>  
* <span data-ttu-id="32a8d-240">**OperationTimeout** anger hello tiden för hello åtgärden toofinish innan en TimeoutException undantag.</span><span class="sxs-lookup"><span data-stu-id="32a8d-240">**OperationTimeout** sets hello amount of time for hello operation toofinish before a TimeoutException exception is thrown.</span></span>
* <span data-ttu-id="32a8d-241">**CancellationToken** gör en väntande samtal toobe avbröts.</span><span class="sxs-lookup"><span data-stu-id="32a8d-241">**CancellationToken** enables a pending call toobe canceled.</span></span>

<span data-ttu-id="32a8d-242">Du kan ange den via en partition nyckel och hello typ av replik i stället för att ange hello noden direkt med sitt namn.</span><span class="sxs-lookup"><span data-stu-id="32a8d-242">Instead of directly specifying hello node by its name, you can specify it via a partition key and hello kind of replica.</span></span>

<span data-ttu-id="32a8d-243">Mer information finns i [PartitionSelector och ReplicaSelector](#partition_replica_selector).</span><span class="sxs-lookup"><span data-stu-id="32a8d-243">For further information, see [PartitionSelector and ReplicaSelector](#partition_replica_selector).</span></span>

```csharp
// Add a reference tooSystem.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart hello node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way toorestart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a><span data-ttu-id="32a8d-244">PartitionSelector och ReplicaSelector</span><span class="sxs-lookup"><span data-stu-id="32a8d-244">PartitionSelector and ReplicaSelector</span></span>
### <a name="partitionselector"></a><span data-ttu-id="32a8d-245">PartitionSelector</span><span class="sxs-lookup"><span data-stu-id="32a8d-245">PartitionSelector</span></span>
<span data-ttu-id="32a8d-246">PartitionSelector är en hjälp som visas i datatillgång är används tooselect en specifik partition på vilka tooperform hello datatillgång åtgärder.</span><span class="sxs-lookup"><span data-stu-id="32a8d-246">PartitionSelector is a helper exposed in testability and is used tooselect a specific partition on which tooperform any of hello testability actions.</span></span> <span data-ttu-id="32a8d-247">Det kan vara används tooselect en specifik partition om hello partitions-ID är känt i förväg.</span><span class="sxs-lookup"><span data-stu-id="32a8d-247">It can be used tooselect a specific partition if hello partition ID is known beforehand.</span></span> <span data-ttu-id="32a8d-248">Eller, du kan ange hello partitionsnyckel och hello åtgärden kommer att lösa hello partitions-ID internt.</span><span class="sxs-lookup"><span data-stu-id="32a8d-248">Or, you can provide hello partition key and hello operation will resolve hello partition ID internally.</span></span> <span data-ttu-id="32a8d-249">Du kan också ha hello kan välja att en slumpmässig partition.</span><span class="sxs-lookup"><span data-stu-id="32a8d-249">You also have hello option of selecting a random partition.</span></span>

<span data-ttu-id="32a8d-250">toouse helper, skapa hello PartitionSelector objekt och välj hello partition med hjälp av någon av hello Select * metoder.</span><span class="sxs-lookup"><span data-stu-id="32a8d-250">toouse this helper, create hello PartitionSelector object and select hello partition by using one of hello Select* methods.</span></span> <span data-ttu-id="32a8d-251">Ange sedan hello PartitionSelector objektet toohello API som kräver.</span><span class="sxs-lookup"><span data-stu-id="32a8d-251">Then pass in hello PartitionSelector object toohello API that requires it.</span></span> <span data-ttu-id="32a8d-252">Om inget alternativ har valts standard tooa slumpmässiga partition.</span><span class="sxs-lookup"><span data-stu-id="32a8d-252">If no option is selected, it defaults tooa random partition.</span></span>

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a><span data-ttu-id="32a8d-253">ReplicaSelector</span><span class="sxs-lookup"><span data-stu-id="32a8d-253">ReplicaSelector</span></span>
<span data-ttu-id="32a8d-254">ReplicaSelector är en hjälp som visas i datatillgång är används toohelp väljer du en replik på vilka tooperform någon av hello datatillgång åtgärder.</span><span class="sxs-lookup"><span data-stu-id="32a8d-254">ReplicaSelector is a helper exposed in testability and is used toohelp select a replica on which tooperform any of hello testability actions.</span></span> <span data-ttu-id="32a8d-255">Det kan vara används tooselect en specifik replik om hello-replik-ID är känt i förväg.</span><span class="sxs-lookup"><span data-stu-id="32a8d-255">It can be used tooselect a specific replica if hello replica ID is known beforehand.</span></span> <span data-ttu-id="32a8d-256">Dessutom har hello möjlighet att välja en primär replik eller en slumpmässig sekundär.</span><span class="sxs-lookup"><span data-stu-id="32a8d-256">In addition, you have hello option of selecting a primary replica or a random secondary.</span></span> <span data-ttu-id="32a8d-257">ReplicaSelector härleds från PartitionSelector, så du måste tooselect både hello replik och hello partition där du vill tooperform hello datatillgång igen.</span><span class="sxs-lookup"><span data-stu-id="32a8d-257">ReplicaSelector derives from PartitionSelector, so you need tooselect both hello replica and hello partition on which you wish tooperform hello testability operation.</span></span>

<span data-ttu-id="32a8d-258">toouse helper, skapa ett ReplicaSelector objekt och ange hello önskemål tooselect hello replik och hello partition.</span><span class="sxs-lookup"><span data-stu-id="32a8d-258">toouse this helper, create a ReplicaSelector object and set hello way you want tooselect hello replica and hello partition.</span></span> <span data-ttu-id="32a8d-259">Du kan sedan överföra den till hello API som kräver.</span><span class="sxs-lookup"><span data-stu-id="32a8d-259">You can then pass it into hello API that requires it.</span></span> <span data-ttu-id="32a8d-260">Om inget alternativ har valts standard tooa slumpmässiga replik och slumpmässiga partition.</span><span class="sxs-lookup"><span data-stu-id="32a8d-260">If no option is selected, it defaults tooa random replica and random partition.</span></span>

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select hello primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select hello replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a><span data-ttu-id="32a8d-261">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32a8d-261">Next steps</span></span>
* [<span data-ttu-id="32a8d-262">Möjlighet att testa scenarier</span><span class="sxs-lookup"><span data-stu-id="32a8d-262">Testability scenarios</span></span>](service-fabric-testability-scenarios.md)
* <span data-ttu-id="32a8d-263">Hur tootest din tjänst</span><span class="sxs-lookup"><span data-stu-id="32a8d-263">How tootest your service</span></span>
  * [<span data-ttu-id="32a8d-264">Simulera fel under tjänstens arbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="32a8d-264">Simulate failures during service workloads</span></span>](service-fabric-testability-workload-tests.md)
  * [<span data-ttu-id="32a8d-265">Tjänst-till-tjänst kommunikationsfel</span><span class="sxs-lookup"><span data-stu-id="32a8d-265">Service-to-service communication failures</span></span>](service-fabric-testability-scenarios-service-communication.md)

