---
title: "Simulera fel i Azure mikrotjänster | Microsoft Docs"
description: "Den här artikeln handlar om datatillgång åtgärder finns i Microsoft Azure Service Fabric."
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
ms.openlocfilehash: c8ddc7732999ae555323bebaef60aa34c8f2ec17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="testability-actions"></a><span data-ttu-id="78978-103">Möjlighet att testa åtgärder</span><span class="sxs-lookup"><span data-stu-id="78978-103">Testability actions</span></span>
<span data-ttu-id="78978-104">För att simulera en instabilt infrastruktur tillhandahåller Azure Service Fabric utvecklaren med olika sätt att simulera olika verkliga fel och tillståndsövergångar.</span><span class="sxs-lookup"><span data-stu-id="78978-104">In order to simulate an unreliable infrastructure, Azure Service Fabric provides you, the developer, with ways to simulate various real-world failures and state transitions.</span></span> <span data-ttu-id="78978-105">Dessa visas som datatillgång åtgärder.</span><span class="sxs-lookup"><span data-stu-id="78978-105">These are exposed as testability actions.</span></span> <span data-ttu-id="78978-106">Åtgärderna är för låg nivå API: er som orsakar en viss feltolerans injection, tillståndsövergång eller validering.</span><span class="sxs-lookup"><span data-stu-id="78978-106">The actions are the low-level APIs that cause a specific fault injection, state transition, or validation.</span></span> <span data-ttu-id="78978-107">Du kan skriva omfattande testscenarier för dina tjänster genom att kombinera dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="78978-107">By combining these actions, you can write comprehensive test scenarios for your services.</span></span>

<span data-ttu-id="78978-108">Service Fabric innehåller några vanliga testscenarier består av dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="78978-108">Service Fabric provides some common test scenarios composed of these actions.</span></span> <span data-ttu-id="78978-109">Vi rekommenderar starkt att du använder dessa inbyggda scenarier som väljs noggrant för att testa vanliga tillståndsövergångar och fel fall.</span><span class="sxs-lookup"><span data-stu-id="78978-109">We highly recommend that you utilize these built-in scenarios, which are carefully chosen to test common state transitions and failure cases.</span></span> <span data-ttu-id="78978-110">Åtgärder kan dock användas för att skapa anpassade testscenarier när du vill lägga till täckning för scenarier som inte omfattas av inbyggda scenarier ännu eller som är anpassade skräddarsydda för ditt program.</span><span class="sxs-lookup"><span data-stu-id="78978-110">However, actions can be used to create custom test scenarios when you want to add coverage for scenarios that are not covered by the built-in scenarios yet or that are custom tailored for your application.</span></span>

<span data-ttu-id="78978-111">C#-implementeringar av åtgärder finns i System.Fabric.dll-sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="78978-111">C# implementations of the actions are found in the System.Fabric.dll assembly.</span></span> <span data-ttu-id="78978-112">System Fabric PowerShell-modulen finns i Microsoft.ServiceFabric.Powershell.dll-sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="78978-112">The System Fabric PowerShell module is found in the Microsoft.ServiceFabric.Powershell.dll assembly.</span></span> <span data-ttu-id="78978-113">Modulen ServiceFabric PowerShell installeras som en del av runtime-installation, så att lätt att använda.</span><span class="sxs-lookup"><span data-stu-id="78978-113">As part of runtime installation, the ServiceFabric PowerShell module is installed to allow for ease of use.</span></span>

## <a name="graceful-vs-ungraceful-fault-actions"></a><span data-ttu-id="78978-114">Korrekt kontra städat fel åtgärder</span><span class="sxs-lookup"><span data-stu-id="78978-114">Graceful vs. ungraceful fault actions</span></span>
<span data-ttu-id="78978-115">Möjlighet att testa åtgärder indelas i två huvudsakliga buckets.</span><span class="sxs-lookup"><span data-stu-id="78978-115">Testability actions are classified into two major buckets:</span></span>

* <span data-ttu-id="78978-116">Städat fel: dessa fel simulera fel som omstarter av datorn och krascher.</span><span class="sxs-lookup"><span data-stu-id="78978-116">Ungraceful faults: These faults simulate failures like machine restarts and process crashes.</span></span> <span data-ttu-id="78978-117">I sådana fall fel körningskontexten processens plötsligt.</span><span class="sxs-lookup"><span data-stu-id="78978-117">In such cases of failures, the execution context of process stops abruptly.</span></span> <span data-ttu-id="78978-118">Detta innebär att ingen rensning av tillståndet kan köra innan programmet startas igen.</span><span class="sxs-lookup"><span data-stu-id="78978-118">This means no cleanup of the state can run before the application starts up again.</span></span>
* <span data-ttu-id="78978-119">Korrekt fel: dessa fel simulera korrekt åtgärder som flyttar replik och således som utlösts av belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="78978-119">Graceful faults: These faults simulate graceful actions like replica moves and drops triggered by load balancing.</span></span> <span data-ttu-id="78978-120">I sådana fall kan tjänsten hämtar ett meddelande om stängning och kan rensa tillståndet innan du avslutar.</span><span class="sxs-lookup"><span data-stu-id="78978-120">In such cases, the service gets a notification of the close and can clean up the state before exiting.</span></span>

<span data-ttu-id="78978-121">För bättre kvalitet verifiering, kör du tjänsten och affärskrav arbetsbelastningen när att olika korrekt och städat fel.</span><span class="sxs-lookup"><span data-stu-id="78978-121">For better quality validation, run the service and business workload while inducing various graceful and ungraceful faults.</span></span> <span data-ttu-id="78978-122">Städat fel utöva scenarier där tjänstprocessen plötsligt avslutas mitt i vissa arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="78978-122">Ungraceful faults exercise scenarios where the service process abruptly exits in the middle of some workflow.</span></span> <span data-ttu-id="78978-123">Detta testar återställningssökvägen när tjänsten replikeringen har återställts av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="78978-123">This tests  the recovery path once the service replica is restored by Service Fabric.</span></span> <span data-ttu-id="78978-124">Detta hjälper testa datakonsekvens och om tjänstens tillstånd hanteras korrekt efter fel.</span><span class="sxs-lookup"><span data-stu-id="78978-124">This will help test data consistency and whether the service state is maintained correctly after failures.</span></span> <span data-ttu-id="78978-125">Den andra uppsättningen fel (korrekt misslyckanden) testa att tjänsten korrekt reagerar på repliker flyttas runt av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="78978-125">The other set of failures (the graceful failures) test that the service correctly reacts to replicas being moved around by Service Fabric.</span></span> <span data-ttu-id="78978-126">Detta testar hantering av annullering i metoden RunAsync.</span><span class="sxs-lookup"><span data-stu-id="78978-126">This tests handling of cancellation in the RunAsync method.</span></span> <span data-ttu-id="78978-127">Tjänsten måste kontrollera för att ange, spara tillståndet annullering token och avsluta RunAsync-metoden.</span><span class="sxs-lookup"><span data-stu-id="78978-127">The service needs to check for the cancellation token being set, correctly save its state, and exit the RunAsync method.</span></span>

## <a name="testability-actions-list"></a><span data-ttu-id="78978-128">Möjlighet att testa åtgärdslista</span><span class="sxs-lookup"><span data-stu-id="78978-128">Testability actions list</span></span>
| <span data-ttu-id="78978-129">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="78978-129">Action</span></span> | <span data-ttu-id="78978-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="78978-130">Description</span></span> | <span data-ttu-id="78978-131">Hanterade API</span><span class="sxs-lookup"><span data-stu-id="78978-131">Managed API</span></span> | <span data-ttu-id="78978-132">PowerShell-cmdlet</span><span class="sxs-lookup"><span data-stu-id="78978-132">PowerShell cmdlet</span></span> | <span data-ttu-id="78978-133">Korrekt/städat fel</span><span class="sxs-lookup"><span data-stu-id="78978-133">Graceful/ungraceful faults</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="78978-134">CleanTestState</span><span class="sxs-lookup"><span data-stu-id="78978-134">CleanTestState</span></span> |<span data-ttu-id="78978-135">Tar bort alla testtillstånd från klustret vid en felaktig avstängning av test-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="78978-135">Removes all the test state from the cluster in case of a bad shutdown of the test driver.</span></span> |<span data-ttu-id="78978-136">CleanTestStateAsync</span><span class="sxs-lookup"><span data-stu-id="78978-136">CleanTestStateAsync</span></span> |<span data-ttu-id="78978-137">Remove-ServiceFabricTestState</span><span class="sxs-lookup"><span data-stu-id="78978-137">Remove-ServiceFabricTestState</span></span> |<span data-ttu-id="78978-138">Inte tillämpligt</span><span class="sxs-lookup"><span data-stu-id="78978-138">Not applicable</span></span> |
| <span data-ttu-id="78978-139">InvokeDataLoss</span><span class="sxs-lookup"><span data-stu-id="78978-139">InvokeDataLoss</span></span> |<span data-ttu-id="78978-140">Startar förlust av data i en partition med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="78978-140">Induces data loss into a service partition.</span></span> |<span data-ttu-id="78978-141">InvokeDataLossAsync</span><span class="sxs-lookup"><span data-stu-id="78978-141">InvokeDataLossAsync</span></span> |<span data-ttu-id="78978-142">Invoke-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="78978-142">Invoke-ServiceFabricPartitionDataLoss</span></span> |<span data-ttu-id="78978-143">Korrekt</span><span class="sxs-lookup"><span data-stu-id="78978-143">Graceful</span></span> |
| <span data-ttu-id="78978-144">InvokeQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="78978-144">InvokeQuorumLoss</span></span> |<span data-ttu-id="78978-145">Placerar en given tillståndskänslig service partition i förlorar kvorum.</span><span class="sxs-lookup"><span data-stu-id="78978-145">Puts a given stateful service partition into quorum loss.</span></span> |<span data-ttu-id="78978-146">InvokeQuorumLossAsync</span><span class="sxs-lookup"><span data-stu-id="78978-146">InvokeQuorumLossAsync</span></span> |<span data-ttu-id="78978-147">Anropa ServiceFabricQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="78978-147">Invoke-ServiceFabricQuorumLoss</span></span> |<span data-ttu-id="78978-148">Korrekt</span><span class="sxs-lookup"><span data-stu-id="78978-148">Graceful</span></span> |
| <span data-ttu-id="78978-149">Flytta primära</span><span class="sxs-lookup"><span data-stu-id="78978-149">Move Primary</span></span> |<span data-ttu-id="78978-150">Flyttar den angivna primära repliken av en tillståndskänslig tjänst till den angivna klusternoden.</span><span class="sxs-lookup"><span data-stu-id="78978-150">Moves the specified primary replica of a stateful service to the specified cluster node.</span></span> |<span data-ttu-id="78978-151">MovePrimaryAsync</span><span class="sxs-lookup"><span data-stu-id="78978-151">MovePrimaryAsync</span></span> |<span data-ttu-id="78978-152">Flytta ServiceFabricPrimaryReplica</span><span class="sxs-lookup"><span data-stu-id="78978-152">Move-ServiceFabricPrimaryReplica</span></span> |<span data-ttu-id="78978-153">Korrekt</span><span class="sxs-lookup"><span data-stu-id="78978-153">Graceful</span></span> |
| <span data-ttu-id="78978-154">Flytta sekundär</span><span class="sxs-lookup"><span data-stu-id="78978-154">Move Secondary</span></span> |<span data-ttu-id="78978-155">Flyttar den aktuella sekundär repliken av en tillståndskänslig tjänst till en annan klusternod.</span><span class="sxs-lookup"><span data-stu-id="78978-155">Moves the current secondary replica of a stateful service to a different cluster node.</span></span> |<span data-ttu-id="78978-156">MoveSecondaryAsync</span><span class="sxs-lookup"><span data-stu-id="78978-156">MoveSecondaryAsync</span></span> |<span data-ttu-id="78978-157">Flytta ServiceFabricSecondaryReplica</span><span class="sxs-lookup"><span data-stu-id="78978-157">Move-ServiceFabricSecondaryReplica</span></span> |<span data-ttu-id="78978-158">Korrekt</span><span class="sxs-lookup"><span data-stu-id="78978-158">Graceful</span></span> |
| <span data-ttu-id="78978-159">RemoveReplica</span><span class="sxs-lookup"><span data-stu-id="78978-159">RemoveReplica</span></span> |<span data-ttu-id="78978-160">Simulerar ett replik fel genom att ta bort en replik från ett kluster.</span><span class="sxs-lookup"><span data-stu-id="78978-160">Simulates a replica failure by removing a replica from a cluster.</span></span> <span data-ttu-id="78978-161">Detta kommer att stängas repliken och kommer övergång till rollen None, ta bort dess status från klustret.</span><span class="sxs-lookup"><span data-stu-id="78978-161">This will close the replica and will transition it to role 'None', removing all of its state from the cluster.</span></span> |<span data-ttu-id="78978-162">RemoveReplicaAsync</span><span class="sxs-lookup"><span data-stu-id="78978-162">RemoveReplicaAsync</span></span> |<span data-ttu-id="78978-163">Ta bort ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="78978-163">Remove-ServiceFabricReplica</span></span> |<span data-ttu-id="78978-164">Korrekt</span><span class="sxs-lookup"><span data-stu-id="78978-164">Graceful</span></span> |
| <span data-ttu-id="78978-165">RestartDeployedCodePackage</span><span class="sxs-lookup"><span data-stu-id="78978-165">RestartDeployedCodePackage</span></span> |<span data-ttu-id="78978-166">Simulerar ett fel i koden paketet genom att starta om en kodpaketet har distribuerats på en nod i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="78978-166">Simulates a code package process failure by restarting a code package deployed on a node in a cluster.</span></span> <span data-ttu-id="78978-167">Detta avbryter kod paketet processen, vilket startar om alla användare service repliker finns i den här processen.</span><span class="sxs-lookup"><span data-stu-id="78978-167">This aborts the code package process, which will restart all the user service replicas hosted in that process.</span></span> |<span data-ttu-id="78978-168">RestartDeployedCodePackageAsync</span><span class="sxs-lookup"><span data-stu-id="78978-168">RestartDeployedCodePackageAsync</span></span> |<span data-ttu-id="78978-169">Starta om ServiceFabricDeployedCodePackage</span><span class="sxs-lookup"><span data-stu-id="78978-169">Restart-ServiceFabricDeployedCodePackage</span></span> |<span data-ttu-id="78978-170">Städat</span><span class="sxs-lookup"><span data-stu-id="78978-170">Ungraceful</span></span> |
| <span data-ttu-id="78978-171">RestartNode</span><span class="sxs-lookup"><span data-stu-id="78978-171">RestartNode</span></span> |<span data-ttu-id="78978-172">Simulerar ett nodfel för Service Fabric-kluster genom att starta om en nod.</span><span class="sxs-lookup"><span data-stu-id="78978-172">Simulates a Service Fabric cluster node failure by restarting a node.</span></span> |<span data-ttu-id="78978-173">RestartNodeAsync</span><span class="sxs-lookup"><span data-stu-id="78978-173">RestartNodeAsync</span></span> |<span data-ttu-id="78978-174">Starta om ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="78978-174">Restart-ServiceFabricNode</span></span> |<span data-ttu-id="78978-175">Städat</span><span class="sxs-lookup"><span data-stu-id="78978-175">Ungraceful</span></span> |
| <span data-ttu-id="78978-176">RestartPartition</span><span class="sxs-lookup"><span data-stu-id="78978-176">RestartPartition</span></span> |<span data-ttu-id="78978-177">Simulerar ett datacenter blackout eller kluster blackout scenario genom att starta om vissa eller alla repliker för en partition.</span><span class="sxs-lookup"><span data-stu-id="78978-177">Simulates a datacenter blackout or cluster blackout scenario by restarting some or all replicas of a partition.</span></span> |<span data-ttu-id="78978-178">RestartPartitionAsync</span><span class="sxs-lookup"><span data-stu-id="78978-178">RestartPartitionAsync</span></span> |<span data-ttu-id="78978-179">Restart-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="78978-179">Restart-ServiceFabricPartition</span></span> |<span data-ttu-id="78978-180">Korrekt</span><span class="sxs-lookup"><span data-stu-id="78978-180">Graceful</span></span> |
| <span data-ttu-id="78978-181">RestartReplica</span><span class="sxs-lookup"><span data-stu-id="78978-181">RestartReplica</span></span> |<span data-ttu-id="78978-182">Simulerar ett fel för repliken genom att starta om en beständiga replik i ett kluster, stänga repliken och sedan öppna den igen.</span><span class="sxs-lookup"><span data-stu-id="78978-182">Simulates a replica failure by restarting a persisted replica in a cluster, closing the replica and then reopening it.</span></span> |<span data-ttu-id="78978-183">RestartReplicaAsync</span><span class="sxs-lookup"><span data-stu-id="78978-183">RestartReplicaAsync</span></span> |<span data-ttu-id="78978-184">Starta om ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="78978-184">Restart-ServiceFabricReplica</span></span> |<span data-ttu-id="78978-185">Korrekt</span><span class="sxs-lookup"><span data-stu-id="78978-185">Graceful</span></span> |
| <span data-ttu-id="78978-186">Startnod</span><span class="sxs-lookup"><span data-stu-id="78978-186">StartNode</span></span> |<span data-ttu-id="78978-187">Startar en nod i ett kluster som har redan stoppats.</span><span class="sxs-lookup"><span data-stu-id="78978-187">Starts a node in a cluster that is already stopped.</span></span> |<span data-ttu-id="78978-188">StartNodeAsync</span><span class="sxs-lookup"><span data-stu-id="78978-188">StartNodeAsync</span></span> |<span data-ttu-id="78978-189">Start-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="78978-189">Start-ServiceFabricNode</span></span> |<span data-ttu-id="78978-190">Inte tillämpligt</span><span class="sxs-lookup"><span data-stu-id="78978-190">Not applicable</span></span> |
| <span data-ttu-id="78978-191">StopNode</span><span class="sxs-lookup"><span data-stu-id="78978-191">StopNode</span></span> |<span data-ttu-id="78978-192">Simulerar ett nodfel genom att stoppa en nod i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="78978-192">Simulates a node failure by stopping a node in a cluster.</span></span> <span data-ttu-id="78978-193">Noden förblir ned förrän Startnod anropas.</span><span class="sxs-lookup"><span data-stu-id="78978-193">The node will stay down until StartNode is called.</span></span> |<span data-ttu-id="78978-194">StopNodeAsync</span><span class="sxs-lookup"><span data-stu-id="78978-194">StopNodeAsync</span></span> |<span data-ttu-id="78978-195">Stop-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="78978-195">Stop-ServiceFabricNode</span></span> |<span data-ttu-id="78978-196">Städat</span><span class="sxs-lookup"><span data-stu-id="78978-196">Ungraceful</span></span> |
| <span data-ttu-id="78978-197">ValidateApplication</span><span class="sxs-lookup"><span data-stu-id="78978-197">ValidateApplication</span></span> |<span data-ttu-id="78978-198">Verifierar tillgänglighet och hälsotillståndet för alla Service Fabric-tjänster i ett program, vanligtvis efter att vissa fel i systemet.</span><span class="sxs-lookup"><span data-stu-id="78978-198">Validates the availability and health of all Service Fabric services within an application, usually after inducing some fault into the system.</span></span> |<span data-ttu-id="78978-199">ValidateApplicationAsync</span><span class="sxs-lookup"><span data-stu-id="78978-199">ValidateApplicationAsync</span></span> |<span data-ttu-id="78978-200">Testa ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="78978-200">Test-ServiceFabricApplication</span></span> |<span data-ttu-id="78978-201">Inte tillämpligt</span><span class="sxs-lookup"><span data-stu-id="78978-201">Not applicable</span></span> |
| <span data-ttu-id="78978-202">ValidateService</span><span class="sxs-lookup"><span data-stu-id="78978-202">ValidateService</span></span> |<span data-ttu-id="78978-203">Verifierar tillgänglighet och hälsotillståndet för ett Service Fabric-tjänsten, vanligtvis efter att vissa fel i systemet.</span><span class="sxs-lookup"><span data-stu-id="78978-203">Validates the availability and health of a Service Fabric service, usually after inducing some fault into the system.</span></span> |<span data-ttu-id="78978-204">ValidateServiceAsync</span><span class="sxs-lookup"><span data-stu-id="78978-204">ValidateServiceAsync</span></span> |<span data-ttu-id="78978-205">Testa ServiceFabricService</span><span class="sxs-lookup"><span data-stu-id="78978-205">Test-ServiceFabricService</span></span> |<span data-ttu-id="78978-206">Inte tillämpligt</span><span class="sxs-lookup"><span data-stu-id="78978-206">Not applicable</span></span> |

## <a name="running-a-testability-action-using-powershell"></a><span data-ttu-id="78978-207">Köra en datatillgång-åtgärd med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="78978-207">Running a testability action using PowerShell</span></span>
<span data-ttu-id="78978-208">Den här kursen visar hur du kör en möjlighet att testa åtgärden med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="78978-208">This tutorial shows you how to run a testability action by using PowerShell.</span></span> <span data-ttu-id="78978-209">Du får lära dig att köra en datatillgång-åtgärd mot en lokal (en-box)-kluster eller ett Azure-kluster.</span><span class="sxs-lookup"><span data-stu-id="78978-209">You will learn how to run a testability action against a local (one-box) cluster or an Azure cluster.</span></span> <span data-ttu-id="78978-210">Microsoft.Fabric.Powershell.dll--Service Fabric PowerShell-modulen--installeras automatiskt när du installerar Microsoft Service Fabric MSI.</span><span class="sxs-lookup"><span data-stu-id="78978-210">Microsoft.Fabric.Powershell.dll--the Service Fabric PowerShell module--is installed automatically when you install the Microsoft Service Fabric MSI.</span></span> <span data-ttu-id="78978-211">Modulen har lästs in automatiskt när du öppnar en PowerShell-kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="78978-211">The module is loaded automatically when you open a PowerShell prompt.</span></span>

<span data-ttu-id="78978-212">Självstudiekurs segment:</span><span class="sxs-lookup"><span data-stu-id="78978-212">Tutorial segments:</span></span>

* [<span data-ttu-id="78978-213">Köra en åtgärd mot ett kluster med en ruta</span><span class="sxs-lookup"><span data-stu-id="78978-213">Run an action against a one-box cluster</span></span>](#run-an-action-against-a-one-box-cluster)
* [<span data-ttu-id="78978-214">Köra en åtgärd mot ett Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="78978-214">Run an action against an Azure cluster</span></span>](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a><span data-ttu-id="78978-215">Köra en åtgärd mot ett kluster med en ruta</span><span class="sxs-lookup"><span data-stu-id="78978-215">Run an action against a one-box cluster</span></span>
<span data-ttu-id="78978-216">Om du vill köra en datatillgång-åtgärd mot en lokala klustret, Anslut till klustret och öppna PowerShell-Kommandotolken i administratörsläge.</span><span class="sxs-lookup"><span data-stu-id="78978-216">To run a testability action against a local cluster, first connect to the cluster and open the PowerShell prompt in administrator mode.</span></span> <span data-ttu-id="78978-217">Låt oss titta på den **omstart ServiceFabricNode** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="78978-217">Let us look at the **Restart-ServiceFabricNode** action.</span></span>

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

<span data-ttu-id="78978-218">Här åtgärden **omstart ServiceFabricNode** körs på en nod med namnet ”Nod1”.</span><span class="sxs-lookup"><span data-stu-id="78978-218">Here the action **Restart-ServiceFabricNode** is being run on a node named "Node1".</span></span> <span data-ttu-id="78978-219">Slutförande-läge anger att den inte ska kontrollera om omstart av nod åtgärden faktiskt har slutförts.</span><span class="sxs-lookup"><span data-stu-id="78978-219">The completion mode specifies that it should not verify whether the restart-node action actually succeeded.</span></span> <span data-ttu-id="78978-220">Ange slutförande-läge som ”verifiera” gör att den kan kontrollera om omstart åtgärden faktiskt har slutförts.</span><span class="sxs-lookup"><span data-stu-id="78978-220">Specifying the completion mode as "Verify" will cause it to verify whether the restart action actually succeeded.</span></span> <span data-ttu-id="78978-221">I stället för att direkt ange noden med namnet, kan du ange den via en partitionsnyckel och vilken typ av repliken, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="78978-221">Instead of directly specifying the node by its name, you can specify it via a partition key and the kind of replica, as follows:</span></span>

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

<span data-ttu-id="78978-222">**Starta om ServiceFabricNode** ska användas för att starta om en Service Fabric-nod i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="78978-222">**Restart-ServiceFabricNode** should be used to restart a Service Fabric node in a cluster.</span></span> <span data-ttu-id="78978-223">Detta förhindrar att Fabric.exe-processen, vilket startar om alla system-tjänsten och användaren service repliker finns på noden.</span><span class="sxs-lookup"><span data-stu-id="78978-223">This will stop the Fabric.exe process, which will restart all of the system service and user service replicas hosted on that node.</span></span> <span data-ttu-id="78978-224">Använd detta API för att testa din tjänst kan upptäcka buggar längs failover återställning sökvägar.</span><span class="sxs-lookup"><span data-stu-id="78978-224">Using this API to test your service helps uncover bugs along the failover recovery paths.</span></span> <span data-ttu-id="78978-225">Det hjälper att simulera nodfel i klustret.</span><span class="sxs-lookup"><span data-stu-id="78978-225">It helps simulate node failures in the cluster.</span></span>

<span data-ttu-id="78978-226">I följande skärmbild visas den **omstart ServiceFabricNode** datatillgång kommandot i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="78978-226">The following screenshot shows the **Restart-ServiceFabricNode** testability command in action.</span></span>

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

<span data-ttu-id="78978-227">Utdata från först **Get-ServiceFabricNode** (en cmdlet från modulen Service Fabric PowerShell) visar att det lokala klustret har fem noder: Node.1 till Node.5.</span><span class="sxs-lookup"><span data-stu-id="78978-227">The output of the first **Get-ServiceFabricNode** (a cmdlet from the Service Fabric PowerShell module) shows that the local cluster has five nodes: Node.1 to Node.5.</span></span> <span data-ttu-id="78978-228">När du har möjlighet att testa åtgärden (cmdlet) **omstart ServiceFabricNode** körs på noden med namnet Node.4 vi se att nodens drifttid har återställts.</span><span class="sxs-lookup"><span data-stu-id="78978-228">After the testability action (cmdlet) **Restart-ServiceFabricNode** is executed on the node, named Node.4, we see that the node's uptime has been reset.</span></span>

### <a name="run-an-action-against-an-azure-cluster"></a><span data-ttu-id="78978-229">Köra en åtgärd mot ett Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="78978-229">Run an action against an Azure cluster</span></span>
<span data-ttu-id="78978-230">Kör en datatillgång-åtgärd (med hjälp av PowerShell) mot ett Azure-kluster liknar Kör åtgärden mot lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="78978-230">Running a testability action (by using PowerShell) against an Azure cluster is similar to running the action against a local cluster.</span></span> <span data-ttu-id="78978-231">Den enda skillnaden är att du måste först ansluta till Azure-klustret innan du kan köra åtgärden i stället för att ansluta till det lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="78978-231">The only difference is that before you can run the action, instead of connecting to the local cluster, you need to connect to the Azure cluster first.</span></span>

## <a name="running-a-testability-action-using-c35"></a><span data-ttu-id="78978-232">Köra en datatillgång åtgärd med C & #35.</span><span class="sxs-lookup"><span data-stu-id="78978-232">Running a testability action using C&#35;</span></span>
<span data-ttu-id="78978-233">Om du vill köra en möjlighet att testa åtgärden med hjälp av C#, måste du först ansluta till klustret med hjälp av FabricClient.</span><span class="sxs-lookup"><span data-stu-id="78978-233">To run a testability action by using C#, first you need to connect to the cluster by using FabricClient.</span></span> <span data-ttu-id="78978-234">Skaffa de parametrar som behövs för att köra instruktionen.</span><span class="sxs-lookup"><span data-stu-id="78978-234">Then obtain the parameters needed to run the action.</span></span> <span data-ttu-id="78978-235">Olika parametrar som kan användas för att köra samma åtgärd.</span><span class="sxs-lookup"><span data-stu-id="78978-235">Different parameters can be used to run the same action.</span></span>
<span data-ttu-id="78978-236">Titta på åtgärden RestartServiceFabricNode är ett sätt att köra den med hjälp av noden informationen (nodnamnet och nod-instans-ID) i klustret.</span><span class="sxs-lookup"><span data-stu-id="78978-236">Looking at the RestartServiceFabricNode action, one way to run it is by using the node information (node name and node instance ID) in the cluster.</span></span>

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

<span data-ttu-id="78978-237">Parametern förklaring:</span><span class="sxs-lookup"><span data-stu-id="78978-237">Parameter explanation:</span></span>

* <span data-ttu-id="78978-238">**CompleteMode** anger att läget som inte ska kontrollera om omstart åtgärden faktiskt har slutförts.</span><span class="sxs-lookup"><span data-stu-id="78978-238">**CompleteMode** specifies that the mode should not verify whether the restart action actually succeeded.</span></span> <span data-ttu-id="78978-239">Ange slutförande-läge som ”verifiera” gör att den kan kontrollera om omstart åtgärden faktiskt har slutförts.</span><span class="sxs-lookup"><span data-stu-id="78978-239">Specifying the completion mode as "Verify" will cause it to verify whether the restart action actually succeeded.</span></span>  
* <span data-ttu-id="78978-240">**OperationTimeout** anger hur lång tid för att åtgärden ska slutföras innan en TimeoutException undantag.</span><span class="sxs-lookup"><span data-stu-id="78978-240">**OperationTimeout** sets the amount of time for the operation to finish before a TimeoutException exception is thrown.</span></span>
* <span data-ttu-id="78978-241">**CancellationToken** aktiverar ett väntande samtal avbrytas.</span><span class="sxs-lookup"><span data-stu-id="78978-241">**CancellationToken** enables a pending call to be canceled.</span></span>

<span data-ttu-id="78978-242">I stället för att direkt ange noden med namnet, kan du ange det via en partitionsnyckel och vilken typ av replikering.</span><span class="sxs-lookup"><span data-stu-id="78978-242">Instead of directly specifying the node by its name, you can specify it via a partition key and the kind of replica.</span></span>

<span data-ttu-id="78978-243">Mer information finns i [PartitionSelector och ReplicaSelector](#partition_replica_selector).</span><span class="sxs-lookup"><span data-stu-id="78978-243">For further information, see [PartitionSelector and ReplicaSelector](#partition_replica_selector).</span></span>

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
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
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
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

## <a name="partitionselector-and-replicaselector"></a><span data-ttu-id="78978-244">PartitionSelector och ReplicaSelector</span><span class="sxs-lookup"><span data-stu-id="78978-244">PartitionSelector and ReplicaSelector</span></span>
### <a name="partitionselector"></a><span data-ttu-id="78978-245">PartitionSelector</span><span class="sxs-lookup"><span data-stu-id="78978-245">PartitionSelector</span></span>
<span data-ttu-id="78978-246">PartitionSelector är en hjälp som visas i datatillgång och används för att välja en specifik partition som du vill utföra några åtgärder för datatillgång.</span><span class="sxs-lookup"><span data-stu-id="78978-246">PartitionSelector is a helper exposed in testability and is used to select a specific partition on which to perform any of the testability actions.</span></span> <span data-ttu-id="78978-247">Det kan användas för att välja en specifik partition om partitions-ID är känt i förväg.</span><span class="sxs-lookup"><span data-stu-id="78978-247">It can be used to select a specific partition if the partition ID is known beforehand.</span></span> <span data-ttu-id="78978-248">Eller, du kan ange Partitionsnyckeln och åtgärden kommer att lösa partitions-ID internt.</span><span class="sxs-lookup"><span data-stu-id="78978-248">Or, you can provide the partition key and the operation will resolve the partition ID internally.</span></span> <span data-ttu-id="78978-249">Du har också välja en slumpmässig partition.</span><span class="sxs-lookup"><span data-stu-id="78978-249">You also have the option of selecting a random partition.</span></span>

<span data-ttu-id="78978-250">Skapa PartitionSelector-objekt och markerar du partitionen med någon av metoderna väljer * om du vill använda den här hjälpfilen.</span><span class="sxs-lookup"><span data-stu-id="78978-250">To use this helper, create the PartitionSelector object and select the partition by using one of the Select* methods.</span></span> <span data-ttu-id="78978-251">Ange sedan objektet PartitionSelector-API: et som kräver.</span><span class="sxs-lookup"><span data-stu-id="78978-251">Then pass in the PartitionSelector object to the API that requires it.</span></span> <span data-ttu-id="78978-252">Om inget alternativ har valts standard till en slumpmässig partition.</span><span class="sxs-lookup"><span data-stu-id="78978-252">If no option is selected, it defaults to a random partition.</span></span>

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

### <a name="replicaselector"></a><span data-ttu-id="78978-253">ReplicaSelector</span><span class="sxs-lookup"><span data-stu-id="78978-253">ReplicaSelector</span></span>
<span data-ttu-id="78978-254">ReplicaSelector är en hjälp som visas i datatillgång och används för att markera en replik som du vill utföra några åtgärder för datatillgång.</span><span class="sxs-lookup"><span data-stu-id="78978-254">ReplicaSelector is a helper exposed in testability and is used to help select a replica on which to perform any of the testability actions.</span></span> <span data-ttu-id="78978-255">Det kan användas för att välja en specifik replik om replik-ID är känt i förväg.</span><span class="sxs-lookup"><span data-stu-id="78978-255">It can be used to select a specific replica if the replica ID is known beforehand.</span></span> <span data-ttu-id="78978-256">Dessutom har möjlighet att välja en primär replik eller en slumpmässig sekundär.</span><span class="sxs-lookup"><span data-stu-id="78978-256">In addition, you have the option of selecting a primary replica or a random secondary.</span></span> <span data-ttu-id="78978-257">ReplicaSelector härleds från PartitionSelector, så du måste välja både repliken och den partition som du vill utföra åtgärden datatillgång.</span><span class="sxs-lookup"><span data-stu-id="78978-257">ReplicaSelector derives from PartitionSelector, so you need to select both the replica and the partition on which you wish to perform the testability operation.</span></span>

<span data-ttu-id="78978-258">Skapa ett ReplicaSelector-objekt om du vill använda den här hjälpfilen och ange hur du vill välja repliken och partitionen.</span><span class="sxs-lookup"><span data-stu-id="78978-258">To use this helper, create a ReplicaSelector object and set the way you want to select the replica and the partition.</span></span> <span data-ttu-id="78978-259">Du kan sedan överföra den till API-gränssnitt som kräver.</span><span class="sxs-lookup"><span data-stu-id="78978-259">You can then pass it into the API that requires it.</span></span> <span data-ttu-id="78978-260">Om inget alternativ har valts används som standard slumpmässig replik- och slumpmässiga partition.</span><span class="sxs-lookup"><span data-stu-id="78978-260">If no option is selected, it defaults to a random replica and random partition.</span></span>

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a><span data-ttu-id="78978-261">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="78978-261">Next steps</span></span>
* [<span data-ttu-id="78978-262">Möjlighet att testa scenarier</span><span class="sxs-lookup"><span data-stu-id="78978-262">Testability scenarios</span></span>](service-fabric-testability-scenarios.md)
* <span data-ttu-id="78978-263">Hur du testar din tjänst</span><span class="sxs-lookup"><span data-stu-id="78978-263">How to test your service</span></span>
  * [<span data-ttu-id="78978-264">Simulera fel under tjänstens arbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="78978-264">Simulate failures during service workloads</span></span>](service-fabric-testability-workload-tests.md)
  * [<span data-ttu-id="78978-265">Tjänst-till-tjänst kommunikationsfel</span><span class="sxs-lookup"><span data-stu-id="78978-265">Service-to-service communication failures</span></span>](service-fabric-testability-scenarios-service-communication.md)

