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
# <a name="how-tooinvoke-data-loss-on-services"></a>Hur tooInvoke dataförlust för tjänster
> [!WARNING]
> Det här dokumentet beskrivs hur toocause dataförlust i dina tjänster och bör användas med försiktighet.
> 
> 

## <a name="introduction"></a>Introduktion
Du kan anropa förlust av data på en partition av Service Fabric-tjänsten genom att anropa StartPartitionDataLossAsync().  Detta api använder hello fel Injection och Analysis Service tooperform hello arbete toocause data går förlorade villkor.

## <a name="using-hello-fault-injection-and-analysis-service"></a>Med hjälp av hello fel Injection och Analysis Services
hello fel Injection och Analysis Services stöder för närvarande hello följande API: er i hello diagrammet nedan.  hello höger sida av hello diagram visar hello motsvarande PowerShell-cmdleten.  Mer information om var och en finns toohello msdn-dokumentationen på varje API.

| C#-API | PowerShell-Cmdlet |
| --- | ---:|
| [StartPartitionDataLossAsync][dl] |[Start-ServiceFabricPartitionDataLoss][psdl] |
| [StartPartitionQuorumLossAsync][ql] |[Start-ServiceFabricPartitionQuorumLoss][psql] |
| [StartPartitionRestartAsync][rp] |[Start-ServiceFabricPartitionRestart][psrp] |

## <a name="conceptual-overview-of-running-a-command"></a>Översikt över ett kommando körs
Hej fel Injection och Analysis Services använder en asynkron modell där du startar hello kommandot med ett API som anges tooas hello ”Start” API i detta dokument, sedan kontrollerar hello förloppet för det här kommandot med ett ”GetProgress” API tills en terminal tillstånd, eller tills du avbryta den.
toostart ett kommando hello ”Start” API-anropet efter hello motsvarande API.  Detta API returnerar när hello fel Injection och Analysis Services har accepterat hello-begäran.  Men det visar inte hur långt kommandot har körts, eller om den har startat ännu.  Anropa hello ”GetProgress” API som motsvarar toohello ”Start” API tidigare kallade pågående ordning toocheck av ett kommando.  Hej ”GetProgress” API returnerar ett objekt som visar hello aktuell status för hello-kommando i egenskapen tillstånd.  Ett kommando körs under obestämd tid tills:

1. Den har slutförts.  Om du anropar ”GetProgress” på den i det här fallet kommer hello förlopp objektets tillstånd att slutföras.
2. Ett oåterkalleligt fel uppstår.  Om du anropar ”GetProgress” på den i det här fallet blir hello förlopp objektets tillstånd felaktig
3. Avbryta via hello [CancelTestCommandAsync] [ cancel] API, eller [stoppa ServiceFabricTestCommand] [ cancelps] PowerShell-cmdlet.  Om du anropa ”GetProgress” på den i det här fallet att hello förlopp objektets tillstånd avbrott eller ForceCancelled, beroende på ett argumentet toothat API.  Hello i dokumentationen för [CancelTestCommandAsync] [ cancel] för mer information.

## <a name="details-of-running-a-command"></a>Information om ett kommando körs
Anropa hello starta API med hello förväntade argument i ordning toostart ett kommando.  Alla Start-API: er har en Guid-argument med namnet åtgärds-ID.  Du bör hålla reda på alla hello åtgärds-ID argumentet, eftersom den används tootrack förloppet för det här kommandot.  Detta måste överföras till hello ”GetProgress” API pågår ordning tootrack hello-kommando.  hello åtgärds-ID måste vara unikt.

När du anropar hello starta API hello GetProgress API ska anropas i en slinga tills hello tillbaka förlopp har objektets tillstånd egenskap slutförts.  Alla [Fabrictransientexception's] [ fte] och Operationcanceledexceptions bör provas igen.
När hello-kommandot har uppnått ett avslutat tillstånd (slutförd, Faulted eller avbrott), returnerade hello förlopp objektets resultatet egenskapen har ytterligare information.  Om hello tillstånd är klar innehåller Result.SelectedPartition.PartitionId hello partitions-id som har valts.  Result.Exception ska vara null.  Om hello tillstånd är fel har Result.Exception hello orsak hello fel Injection och analys fel hello tjänstkommandot.  Result.SelectedPartition.PartitionId har hello partitions-id som har valts.  I vissa situationer kan hello-kommandot inte har gått tillräckligt långt toochoose en partition.  Hello PartitionId kommer i så fall vara 0.  Om hello tillstånd avbryts vara Result.Exception null.  Result.SelectedPartition.PartitionId ha hello partitions-id som har valts som hello Faulted skiftläge, men om hello kommando inte gått tillräckligt långt toodo så, ska den vara 0.  Du läsa toohello exemplet nedan.

hello-exempelkod nedan visar hur toostart sedan följa förloppet på ett kommando toocause förlust av data på en specifik partition.

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

hello exemplet nedan visar hur toouse hello PartitionSelector toochoose en slumpmässig partition för en angiven tjänst:

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

## <a name="history-and-truncation"></a>Historik över och trunkering
När ett kommando har uppnått ett avslutat tillstånd, dess metadata finns kvar i hello fel Injection och Analysis Services för en viss tid innan den blir bort toosave utrymme.  Om ”GetProgress” anropas med hello åtgärds-ID för ett kommando efter att den har tagits bort, returneras en FabricException med en ErrorCode KeyNotFound.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
