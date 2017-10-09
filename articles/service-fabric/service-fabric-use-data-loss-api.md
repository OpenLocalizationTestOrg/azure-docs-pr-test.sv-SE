---
title: "aaaHow tooInvoke förlust av Data på Service Fabric-tjänster | Microsoft Docs"
description: "Beskriver hur toouse hello dataförlust api"
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
ms.openlocfilehash: 014c7ebfd2c42d79a5fe1802ecc3fa0c1f26f9d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinvoke-data-loss-on-services"></a><span data-ttu-id="5e4d5-103">Hur tooInvoke dataförlust för tjänster</span><span class="sxs-lookup"><span data-stu-id="5e4d5-103">How tooInvoke Data Loss on Services</span></span>
> [!WARNING]
> <span data-ttu-id="5e4d5-104">Det här dokumentet beskrivs hur toocause dataförlust i dina tjänster och bör användas med försiktighet.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-104">This document describe how toocause data loss in your services, and should be used with care.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="5e4d5-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="5e4d5-105">Introduction</span></span>
<span data-ttu-id="5e4d5-106">Du kan anropa förlust av data på en partition av Service Fabric-tjänsten genom att anropa StartPartitionDataLossAsync().</span><span class="sxs-lookup"><span data-stu-id="5e4d5-106">You can invoke data loss on a partition of your Service Fabric Service by calling StartPartitionDataLossAsync().</span></span>  <span data-ttu-id="5e4d5-107">Detta api använder hello fel Injection och Analysis Service tooperform hello arbete toocause data går förlorade villkor.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-107">This api uses hello Fault Injection and Analysis Service tooperform hello work toocause data loss conditions.</span></span>

## <a name="using-hello-fault-injection-and-analysis-service"></a><span data-ttu-id="5e4d5-108">Med hjälp av hello fel Injection och Analysis Services</span><span class="sxs-lookup"><span data-stu-id="5e4d5-108">Using hello Fault Injection and Analysis Service</span></span>
<span data-ttu-id="5e4d5-109">hello fel Injection och Analysis Services stöder för närvarande hello följande API: er i hello diagrammet nedan.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-109">hello Fault Injection and Analysis Service currently supports hello following APIs in hello chart below.</span></span>  <span data-ttu-id="5e4d5-110">hello höger sida av hello diagram visar hello motsvarande PowerShell-cmdleten.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-110">hello right side of hello chart shows hello corresponding PowerShell cmdlet.</span></span>  <span data-ttu-id="5e4d5-111">Mer information om var och en finns toohello msdn-dokumentationen på varje API.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-111">Please refer toohello msdn documentation on each API for more information on each one.</span></span>

| <span data-ttu-id="5e4d5-112">C#-API</span><span class="sxs-lookup"><span data-stu-id="5e4d5-112">C# API</span></span> | <span data-ttu-id="5e4d5-113">PowerShell-Cmdlet</span><span class="sxs-lookup"><span data-stu-id="5e4d5-113">PowerShell Cmdlet</span></span> |
| --- | ---:|
| <span data-ttu-id="5e4d5-114">[StartPartitionDataLossAsync][dl]</span><span class="sxs-lookup"><span data-stu-id="5e4d5-114">[StartPartitionDataLossAsync][dl]</span></span> |<span data-ttu-id="5e4d5-115">[Start-ServiceFabricPartitionDataLoss][psdl]</span><span class="sxs-lookup"><span data-stu-id="5e4d5-115">[Start-ServiceFabricPartitionDataLoss][psdl]</span></span> |
| <span data-ttu-id="5e4d5-116">[StartPartitionQuorumLossAsync][ql]</span><span class="sxs-lookup"><span data-stu-id="5e4d5-116">[StartPartitionQuorumLossAsync][ql]</span></span> |<span data-ttu-id="5e4d5-117">[Start-ServiceFabricPartitionQuorumLoss][psql]</span><span class="sxs-lookup"><span data-stu-id="5e4d5-117">[Start-ServiceFabricPartitionQuorumLoss][psql]</span></span> |
| <span data-ttu-id="5e4d5-118">[StartPartitionRestartAsync][rp]</span><span class="sxs-lookup"><span data-stu-id="5e4d5-118">[StartPartitionRestartAsync][rp]</span></span> |<span data-ttu-id="5e4d5-119">[Start-ServiceFabricPartitionRestart][psrp]</span><span class="sxs-lookup"><span data-stu-id="5e4d5-119">[Start-ServiceFabricPartitionRestart][psrp]</span></span> |

## <a name="conceptual-overview-of-running-a-command"></a><span data-ttu-id="5e4d5-120">Översikt över ett kommando körs</span><span class="sxs-lookup"><span data-stu-id="5e4d5-120">Conceptual Overview of Running a Command</span></span>
<span data-ttu-id="5e4d5-121">Hej fel Injection och Analysis Services använder en asynkron modell där du startar hello kommandot med ett API som anges tooas hello ”Start” API i detta dokument, sedan kontrollerar hello förloppet för det här kommandot med ett ”GetProgress” API tills en terminal tillstånd, eller tills du avbryta den.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-121">hello Fault Injection and Analysis Service uses an asynchronous model where you start hello command with one API, referred tooas hello “Start” API in this document, then checks hello progress of this command using a “GetProgress” API until it has reached a terminal state, or until you cancel it.</span></span>
<span data-ttu-id="5e4d5-122">toostart ett kommando hello ”Start” API-anropet efter hello motsvarande API.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-122">toostart a command, call hello “Start” API for hello corresponding API.</span></span>  <span data-ttu-id="5e4d5-123">Detta API returnerar när hello fel Injection och Analysis Services har accepterat hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-123">This API returns when hello Fault Injection and Analysis Service has accepted hello request.</span></span>  <span data-ttu-id="5e4d5-124">Men det visar inte hur långt kommandot har körts, eller om den har startat ännu.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-124">However, it does not indicate how far a command has run, or even if it has started yet.</span></span>  <span data-ttu-id="5e4d5-125">Anropa hello ”GetProgress” API som motsvarar toohello ”Start” API tidigare kallade pågående ordning toocheck av ett kommando.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-125">In order toocheck progress of a command, call hello “GetProgress” API that corresponds toohello “Start” API previously called.</span></span>  <span data-ttu-id="5e4d5-126">Hej ”GetProgress” API returnerar ett objekt som visar hello aktuell status för hello-kommando i egenskapen tillstånd.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-126">hello “GetProgress” API will return an object indicating hello current status of hello command inside its State property.</span></span>  <span data-ttu-id="5e4d5-127">Ett kommando körs under obestämd tid tills:</span><span class="sxs-lookup"><span data-stu-id="5e4d5-127">A command runs indefinitely until:</span></span>

1. <span data-ttu-id="5e4d5-128">Den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-128">It completes successfully.</span></span>  <span data-ttu-id="5e4d5-129">Om du anropar ”GetProgress” på den i det här fallet kommer hello förlopp objektets tillstånd att slutföras.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-129">If you call “GetProgress” on it in this case, hello progress object’s State will be Completed.</span></span>
2. <span data-ttu-id="5e4d5-130">Ett oåterkalleligt fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-130">It encounters a fatal error.</span></span>  <span data-ttu-id="5e4d5-131">Om du anropar ”GetProgress” på den i det här fallet blir hello förlopp objektets tillstånd felaktig</span><span class="sxs-lookup"><span data-stu-id="5e4d5-131">If you call “GetProgress” on it in this case, hello progress object’s State will be Faulted</span></span>
3. <span data-ttu-id="5e4d5-132">Avbryta via hello [CancelTestCommandAsync] [ cancel] API, eller [stoppa ServiceFabricTestCommand] [ cancelps] PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-132">You cancel it through hello [CancelTestCommandAsync][cancel] API, or [Stop-ServiceFabricTestCommand][cancelps] PowerShell cmdlet.</span></span>  <span data-ttu-id="5e4d5-133">Om du anropa ”GetProgress” på den i det här fallet att hello förlopp objektets tillstånd avbrott eller ForceCancelled, beroende på ett argumentet toothat API.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-133">If you call “GetProgress” on it in this case, hello progress object’s State will be either Cancelled or ForceCancelled, depending on an argument toothat API.</span></span>  <span data-ttu-id="5e4d5-134">Hello i dokumentationen för [CancelTestCommandAsync] [ cancel] för mer information.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-134">See hello documentation for [CancelTestCommandAsync][cancel] for more details.</span></span>

## <a name="details-of-running-a-command"></a><span data-ttu-id="5e4d5-135">Information om ett kommando körs</span><span class="sxs-lookup"><span data-stu-id="5e4d5-135">Details of Running a Command</span></span>
<span data-ttu-id="5e4d5-136">Anropa hello starta API med hello förväntade argument i ordning toostart ett kommando.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-136">In order toostart a command, call hello Start API with hello expected arguments.</span></span>  <span data-ttu-id="5e4d5-137">Alla Start-API: er har en Guid-argument med namnet åtgärds-ID.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-137">All Start APIs have a Guid argument named operationId.</span></span>  <span data-ttu-id="5e4d5-138">Du bör hålla reda på alla hello åtgärds-ID argumentet, eftersom den används tootrack förloppet för det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-138">You should keep track of hello operationId argument, since it is used tootrack progress of this command.</span></span>  <span data-ttu-id="5e4d5-139">Detta måste överföras till hello ”GetProgress” API pågår ordning tootrack hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-139">This must be passed into hello “GetProgress” API in order tootrack progress of hello command.</span></span>  <span data-ttu-id="5e4d5-140">hello åtgärds-ID måste vara unikt.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-140">hello operationId must be unique.</span></span>

<span data-ttu-id="5e4d5-141">När du anropar hello starta API hello GetProgress API ska anropas i en slinga tills hello tillbaka förlopp har objektets tillstånd egenskap slutförts.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-141">After successfully calling hello Start API, hello GetProgress API should be called in a loop until hello returned progress object’s State property is Completed.</span></span>  <span data-ttu-id="5e4d5-142">Alla [Fabrictransientexception's] [ fte] och Operationcanceledexceptions bör provas igen.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-142">All [FabricTransientException’s][fte] and OperationCanceledException’s should be retried.</span></span>
<span data-ttu-id="5e4d5-143">När hello-kommandot har uppnått ett avslutat tillstånd (slutförd, Faulted eller avbrott), returnerade hello förlopp objektets resultatet egenskapen har ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-143">When hello command has reached a terminal state (Completed, Faulted, or Cancelled), hello returned progress object’s Result property will have additional information.</span></span>  <span data-ttu-id="5e4d5-144">Om hello tillstånd är klar innehåller Result.SelectedPartition.PartitionId hello partitions-id som har valts.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-144">If hello state is Completed, Result.SelectedPartition.PartitionId will contain hello partition id that was selected.</span></span>  <span data-ttu-id="5e4d5-145">Result.Exception ska vara null.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-145">Result.Exception will be null.</span></span>  <span data-ttu-id="5e4d5-146">Om hello tillstånd är fel har Result.Exception hello orsak hello fel Injection och analys fel hello tjänstkommandot.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-146">If hello state is Faulted, Result.Exception will have hello reason hello Fault Injection and Analysis Service faulted hello command.</span></span>  <span data-ttu-id="5e4d5-147">Result.SelectedPartition.PartitionId har hello partitions-id som har valts.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-147">Result.SelectedPartition.PartitionId will have hello partition id that was selected.</span></span>  <span data-ttu-id="5e4d5-148">I vissa situationer kan hello-kommandot inte har gått tillräckligt långt toochoose en partition.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-148">In some situations, hello command may not have proceeded far enough toochoose a partition.</span></span>  <span data-ttu-id="5e4d5-149">Hello PartitionId kommer i så fall vara 0.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-149">In that case, hello PartitionId will be 0.</span></span>  <span data-ttu-id="5e4d5-150">Om hello tillstånd avbryts vara Result.Exception null.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-150">If hello state is Cancelled, Result.Exception will be null.</span></span>  <span data-ttu-id="5e4d5-151">Result.SelectedPartition.PartitionId ha hello partitions-id som har valts som hello Faulted skiftläge, men om hello kommando inte gått tillräckligt långt toodo så, ska den vara 0.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-151">Like hello Faulted case, Result.SelectedPartition.PartitionId will have hello partition id that was chosen, but if hello command has not proceeded far enough toodo so, it will be 0.</span></span>  <span data-ttu-id="5e4d5-152">Du läsa toohello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-152">Please also refer toohello sample below.</span></span>

<span data-ttu-id="5e4d5-153">hello-exempelkod nedan visar hur toostart sedan följa förloppet på ett kommando toocause förlust av data på en specifik partition.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-153">hello sample code below shows how toostart then check progress on a command toocause data loss on a specific partition.</span></span>

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for hello command below
        Guid operationId = Guid.NewGuid();

        // Note: Use hello appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // hello name of hello target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // hello id of hello target partition inside hello target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start hello command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means hello Fault Injection and Analysis Service has saved hello intent tooperform this work.  It does not say anything about hello progress
        // of hello command.
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

        // Poll hello progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // hello command won't be cancelled.        

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

                // In a terminal state .Result.SelectedPartition.PartitionId will have hello chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, hello progress object's Result property's Exception property will have hello reason why.
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

<span data-ttu-id="5e4d5-154">hello exemplet nedan visar hur toouse hello PartitionSelector toochoose en slumpmässig partition för en angiven tjänst:</span><span class="sxs-lookup"><span data-stu-id="5e4d5-154">hello sample below shows how toouse hello PartitionSelector toochoose a random partition of a specified service:</span></span>

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for hello command below
        Guid operationId = Guid.NewGuid();

        // Note: Use hello appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // hello name of hello target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have hello Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start hello command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means hello Fault Injection and Analysis Service has saved hello intent tooperform this work.  It does not say anything about hello progress
        // of hello command.
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

        // Poll hello progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // hello command won't be cancelled.

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
                // If State is Faulted, hello progress object's Result property's Exception property will have hello reason why.
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

## <a name="history-and-truncation"></a><span data-ttu-id="5e4d5-155">Historik över och trunkering</span><span class="sxs-lookup"><span data-stu-id="5e4d5-155">History and Truncation</span></span>
<span data-ttu-id="5e4d5-156">När ett kommando har uppnått ett avslutat tillstånd, dess metadata finns kvar i hello fel Injection och Analysis Services för en viss tid innan den blir bort toosave utrymme.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-156">After a command has reached a terminal state, its metadata will remain in hello Fault Injection and Analysis Service for a certain time, before it will be removed toosave space.</span></span>  <span data-ttu-id="5e4d5-157">Om ”GetProgress” anropas med hello åtgärds-ID för ett kommando efter att den har tagits bort, returneras en FabricException med en ErrorCode KeyNotFound.</span><span class="sxs-lookup"><span data-stu-id="5e4d5-157">If “GetProgress” is called using hello operationId of a command after it has been removed, it will return a FabricException with an ErrorCode of KeyNotFound.</span></span>

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
