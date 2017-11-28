---
title: "Hur du anropa dataförlust på Service Fabric-tjänster | Microsoft Docs"
description: "Beskriver hur du använder data går förlorade api"
services: service-fabric
documentationcenter: .net
author: LMWF
manager: rsinha
editor: 
ms.assetid: f4e70f6f-cad9-4a3e-9655-009b4db09c6d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/19/2016
ms.author: lemai
redirect_url: /azure/service-fabric/service-fabric-testability-overview
ms.openlocfilehash: 0c4791e56f84d0df38783a13c8d8c564fd25f55f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-invoke-data-loss-on-services"></a><span data-ttu-id="11719-103">Hur du anropa dataförlust för tjänster</span><span class="sxs-lookup"><span data-stu-id="11719-103">How to Invoke Data Loss on Services</span></span>
> [!WARNING]
> <span data-ttu-id="11719-104">Det här dokumentet beskrivs hur du kan data gå förlorade i dina tjänster och ska användas med försiktighet.</span><span class="sxs-lookup"><span data-stu-id="11719-104">This document describe how to cause data loss in your services, and should be used with care.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="11719-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="11719-105">Introduction</span></span>
<span data-ttu-id="11719-106">Du kan anropa förlust av data på en partition av Service Fabric-tjänsten genom att anropa StartPartitionDataLossAsync().</span><span class="sxs-lookup"><span data-stu-id="11719-106">You can invoke data loss on a partition of your Service Fabric Service by calling StartPartitionDataLossAsync().</span></span>  <span data-ttu-id="11719-107">Detta api använder fel Injection och Analysis Service för att utföra arbetet om du vill att data går förlorade villkor.</span><span class="sxs-lookup"><span data-stu-id="11719-107">This api uses the Fault Injection and Analysis Service to perform the work to cause data loss conditions.</span></span>

## <a name="using-the-fault-injection-and-analysis-service"></a><span data-ttu-id="11719-108">Med fel Injection och Analysis Services</span><span class="sxs-lookup"><span data-stu-id="11719-108">Using the Fault Injection and Analysis Service</span></span>
<span data-ttu-id="11719-109">Fel Injection och Analysis Services stöder för närvarande följande API: er i diagrammet nedan.</span><span class="sxs-lookup"><span data-stu-id="11719-109">The Fault Injection and Analysis Service currently supports the following APIs in the chart below.</span></span>  <span data-ttu-id="11719-110">Till höger om diagrammet visas motsvarande PowerShell-cmdleten.</span><span class="sxs-lookup"><span data-stu-id="11719-110">The right side of the chart shows the corresponding PowerShell cmdlet.</span></span>  <span data-ttu-id="11719-111">Information finns i msdn-dokumentationen på varje API för mer information om var och en.</span><span class="sxs-lookup"><span data-stu-id="11719-111">Please refer to the msdn documentation on each API for more information on each one.</span></span>

| <span data-ttu-id="11719-112">C#-API</span><span class="sxs-lookup"><span data-stu-id="11719-112">C# API</span></span> | <span data-ttu-id="11719-113">PowerShell-Cmdlet</span><span class="sxs-lookup"><span data-stu-id="11719-113">PowerShell Cmdlet</span></span> |
| --- | ---:|
| <span data-ttu-id="11719-114">[StartPartitionDataLossAsync][dl]</span><span class="sxs-lookup"><span data-stu-id="11719-114">[StartPartitionDataLossAsync][dl]</span></span> |<span data-ttu-id="11719-115">[Start-ServiceFabricPartitionDataLoss][psdl]</span><span class="sxs-lookup"><span data-stu-id="11719-115">[Start-ServiceFabricPartitionDataLoss][psdl]</span></span> |
| <span data-ttu-id="11719-116">[StartPartitionQuorumLossAsync][ql]</span><span class="sxs-lookup"><span data-stu-id="11719-116">[StartPartitionQuorumLossAsync][ql]</span></span> |<span data-ttu-id="11719-117">[Start-ServiceFabricPartitionQuorumLoss][psql]</span><span class="sxs-lookup"><span data-stu-id="11719-117">[Start-ServiceFabricPartitionQuorumLoss][psql]</span></span> |
| <span data-ttu-id="11719-118">[StartPartitionRestartAsync][rp]</span><span class="sxs-lookup"><span data-stu-id="11719-118">[StartPartitionRestartAsync][rp]</span></span> |<span data-ttu-id="11719-119">[Start-ServiceFabricPartitionRestart][psrp]</span><span class="sxs-lookup"><span data-stu-id="11719-119">[Start-ServiceFabricPartitionRestart][psrp]</span></span> |

## <a name="conceptual-overview-of-running-a-command"></a><span data-ttu-id="11719-120">Översikt över ett kommando körs</span><span class="sxs-lookup"><span data-stu-id="11719-120">Conceptual Overview of Running a Command</span></span>
<span data-ttu-id="11719-121">Fel Injection och Analysis Services använder en asynkron modell där du startar kommandot med ett API som kallas ”Start” API: et i detta dokument, söker förloppet för det här kommandot med en ”GetProgress” API tills ett avslutat tillstånd, eller tills du avbryta den.</span><span class="sxs-lookup"><span data-stu-id="11719-121">The Fault Injection and Analysis Service uses an asynchronous model where you start the command with one API, referred to as the “Start” API in this document, then checks the progress of this command using a “GetProgress” API until it has reached a terminal state, or until you cancel it.</span></span>
<span data-ttu-id="11719-122">Anropa ”Start”-API: et för motsvarande API för att starta ett kommando.</span><span class="sxs-lookup"><span data-stu-id="11719-122">To start a command, call the “Start” API for the corresponding API.</span></span>  <span data-ttu-id="11719-123">Detta API returnerar när fel Injection och Analysis Services har accepterat begäran.</span><span class="sxs-lookup"><span data-stu-id="11719-123">This API returns when the Fault Injection and Analysis Service has accepted the request.</span></span>  <span data-ttu-id="11719-124">Men det visar inte hur långt kommandot har körts, eller om den har startat ännu.</span><span class="sxs-lookup"><span data-stu-id="11719-124">However, it does not indicate how far a command has run, or even if it has started yet.</span></span>  <span data-ttu-id="11719-125">Anropa API som motsvarar ”Start”-API: et tidigare kallade för ”GetProgress” för att kunna kontrollera status för ett kommando.</span><span class="sxs-lookup"><span data-stu-id="11719-125">In order to check progress of a command, call the “GetProgress” API that corresponds to the “Start” API previously called.</span></span>  <span data-ttu-id="11719-126">API för ”GetProgress” returnerar ett objekt som indikerar aktuell status för kommandot i egenskapen tillstånd.</span><span class="sxs-lookup"><span data-stu-id="11719-126">The “GetProgress” API will return an object indicating the current status of the command inside its State property.</span></span>  <span data-ttu-id="11719-127">Ett kommando körs under obestämd tid tills:</span><span class="sxs-lookup"><span data-stu-id="11719-127">A command runs indefinitely until:</span></span>

1. <span data-ttu-id="11719-128">Den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="11719-128">It completes successfully.</span></span>  <span data-ttu-id="11719-129">Om du anropar ”GetProgress” på den i det här fallet kommer förlopp objektets tillstånd att slutföras.</span><span class="sxs-lookup"><span data-stu-id="11719-129">If you call “GetProgress” on it in this case, the progress object’s State will be Completed.</span></span>
2. <span data-ttu-id="11719-130">Ett oåterkalleligt fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="11719-130">It encounters a fatal error.</span></span>  <span data-ttu-id="11719-131">Om du anropar ”GetProgress” på den i det här fallet blir förlopp objektets tillstånd felaktig</span><span class="sxs-lookup"><span data-stu-id="11719-131">If you call “GetProgress” on it in this case, the progress object’s State will be Faulted</span></span>
3. <span data-ttu-id="11719-132">Avbryta via den [CancelTestCommandAsync] [ cancel] API, eller [stoppa ServiceFabricTestCommand] [ cancelps] PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="11719-132">You cancel it through the [CancelTestCommandAsync][cancel] API, or [Stop-ServiceFabricTestCommand][cancelps] PowerShell cmdlet.</span></span>  <span data-ttu-id="11719-133">Om du anropar ”GetProgress” på den i det här fallet är förlopp objektets tillstånd avbrott eller ForceCancelled, beroende på ett argument för detta API.</span><span class="sxs-lookup"><span data-stu-id="11719-133">If you call “GetProgress” on it in this case, the progress object’s State will be either Cancelled or ForceCancelled, depending on an argument to that API.</span></span>  <span data-ttu-id="11719-134">Finns i dokumentationen för [CancelTestCommandAsync] [ cancel] för mer information.</span><span class="sxs-lookup"><span data-stu-id="11719-134">See the documentation for [CancelTestCommandAsync][cancel] for more details.</span></span>

## <a name="details-of-running-a-command"></a><span data-ttu-id="11719-135">Information om ett kommando körs</span><span class="sxs-lookup"><span data-stu-id="11719-135">Details of Running a Command</span></span>
<span data-ttu-id="11719-136">Anropa Start-API med förväntade argument för att starta ett kommando.</span><span class="sxs-lookup"><span data-stu-id="11719-136">In order to start a command, call the Start API with the expected arguments.</span></span>  <span data-ttu-id="11719-137">Alla Start-API: er har en Guid-argument med namnet åtgärds-ID.</span><span class="sxs-lookup"><span data-stu-id="11719-137">All Start APIs have a Guid argument named operationId.</span></span>  <span data-ttu-id="11719-138">Du bör hålla koll på argumentet åtgärds-ID, eftersom den används för att spåra förloppet för det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="11719-138">You should keep track of the operationId argument, since it is used to track progress of this command.</span></span>  <span data-ttu-id="11719-139">Detta måste överföras till ”GetProgress” API för att kunna spåra förloppet för kommandot.</span><span class="sxs-lookup"><span data-stu-id="11719-139">This must be passed into the “GetProgress” API in order to track progress of the command.</span></span>  <span data-ttu-id="11719-140">Åtgärds-ID måste vara unikt.</span><span class="sxs-lookup"><span data-stu-id="11719-140">The operationId must be unique.</span></span>

<span data-ttu-id="11719-141">När du anropar API: et startar, bör GetProgress API: et anropas i en slinga tills objektet returnerade förlopp tillstånd egenskapen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="11719-141">After successfully calling the Start API, the GetProgress API should be called in a loop until the returned progress object’s State property is Completed.</span></span>  <span data-ttu-id="11719-142">Alla [Fabrictransientexception's] [ fte] och Operationcanceledexceptions bör provas igen.</span><span class="sxs-lookup"><span data-stu-id="11719-142">All [FabricTransientException’s][fte] and OperationCanceledException’s should be retried.</span></span>
<span data-ttu-id="11719-143">När kommandot har uppnått ett avslutat tillstånd (slutförd, Faulted eller avbrott), har returnerade förlopp objektets resultatet egenskapen ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="11719-143">When the command has reached a terminal state (Completed, Faulted, or Cancelled), the returned progress object’s Result property will have additional information.</span></span>  <span data-ttu-id="11719-144">Om tillståndet är klar innehåller Result.SelectedPartition.PartitionId partitions-id som har valts.</span><span class="sxs-lookup"><span data-stu-id="11719-144">If the state is Completed, Result.SelectedPartition.PartitionId will contain the partition id that was selected.</span></span>  <span data-ttu-id="11719-145">Result.Exception ska vara null.</span><span class="sxs-lookup"><span data-stu-id="11719-145">Result.Exception will be null.</span></span>  <span data-ttu-id="11719-146">Om tillståndet är fel Result.Exception har orsaken fel Injection och Analysis Service fel inträffade i kommandot.</span><span class="sxs-lookup"><span data-stu-id="11719-146">If the state is Faulted, Result.Exception will have the reason the Fault Injection and Analysis Service faulted the command.</span></span>  <span data-ttu-id="11719-147">Result.SelectedPartition.PartitionId har partitions-id som har valts.</span><span class="sxs-lookup"><span data-stu-id="11719-147">Result.SelectedPartition.PartitionId will have the partition id that was selected.</span></span>  <span data-ttu-id="11719-148">I vissa situationer kan kan kommandot inte ha gått tillräckligt långt att välja en partition.</span><span class="sxs-lookup"><span data-stu-id="11719-148">In some situations, the command may not have proceeded far enough to choose a partition.</span></span>  <span data-ttu-id="11719-149">I så fall kommer PartitionId vara 0.</span><span class="sxs-lookup"><span data-stu-id="11719-149">In that case, the PartitionId will be 0.</span></span>  <span data-ttu-id="11719-150">Om tillståndet avbryts vara Result.Exception null.</span><span class="sxs-lookup"><span data-stu-id="11719-150">If the state is Cancelled, Result.Exception will be null.</span></span>  <span data-ttu-id="11719-151">Result.SelectedPartition.PartitionId ha partitions-id som har valts som Faulted fallet, men om kommandot inte gått tillräckligt långt att göra det, kommer det vara 0.</span><span class="sxs-lookup"><span data-stu-id="11719-151">Like the Faulted case, Result.SelectedPartition.PartitionId will have the partition id that was chosen, but if the command has not proceeded far enough to do so, it will be 0.</span></span>  <span data-ttu-id="11719-152">Dessutom finns i exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="11719-152">Please also refer to the sample below.</span></span>

<span data-ttu-id="11719-153">Exempelkoden nedan visar hur du startar och sedan följa förloppet på ett kommando för att orsaka dataförlust av på en specifik partition.</span><span class="sxs-lookup"><span data-stu-id="11719-153">The sample code below shows how to start then check progress on a command to cause data loss on a specific partition.</span></span>

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

<span data-ttu-id="11719-154">Exemplet nedan visar hur du använder PartitionSelector för att välja en slumpmässig partition för en angiven tjänst:</span><span class="sxs-lookup"><span data-stu-id="11719-154">The sample below shows how to use the PartitionSelector to choose a random partition of a specified service:</span></span>

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a><span data-ttu-id="11719-155">Historik över och trunkering</span><span class="sxs-lookup"><span data-stu-id="11719-155">History and Truncation</span></span>
<span data-ttu-id="11719-156">När ett kommando har uppnått ett avslutat tillstånd, förblir dess metadata i fel Injection och Analysis Service för en viss tid innan den tas bort om du vill spara utrymme.</span><span class="sxs-lookup"><span data-stu-id="11719-156">After a command has reached a terminal state, its metadata will remain in the Fault Injection and Analysis Service for a certain time, before it will be removed to save space.</span></span>  <span data-ttu-id="11719-157">Om ”GetProgress” anropas med ett kommando åtgärds-ID när den har tagits bort, returneras en FabricException med en ErrorCode KeyNotFound.</span><span class="sxs-lookup"><span data-stu-id="11719-157">If “GetProgress” is called using the operationId of a command after it has been removed, it will return a FabricException with an ErrorCode of KeyNotFound.</span></span>

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
