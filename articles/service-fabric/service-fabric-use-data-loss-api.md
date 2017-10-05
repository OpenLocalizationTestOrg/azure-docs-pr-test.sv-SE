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
# <a name="how-to-invoke-data-loss-on-services"></a>Hur du anropa dataförlust för tjänster
> [!WARNING]
> Det här dokumentet beskrivs hur du kan data gå förlorade i dina tjänster och ska användas med försiktighet.
> 
> 

## <a name="introduction"></a>Introduktion
Du kan anropa förlust av data på en partition av Service Fabric-tjänsten genom att anropa StartPartitionDataLossAsync().  Detta api använder fel Injection och Analysis Service för att utföra arbetet om du vill att data går förlorade villkor.

## <a name="using-the-fault-injection-and-analysis-service"></a>Med fel Injection och Analysis Services
Fel Injection och Analysis Services stöder för närvarande följande API: er i diagrammet nedan.  Till höger om diagrammet visas motsvarande PowerShell-cmdleten.  Information finns i msdn-dokumentationen på varje API för mer information om var och en.

| C#-API | PowerShell-Cmdlet |
| --- | ---:|
| [StartPartitionDataLossAsync][dl] |[Start-ServiceFabricPartitionDataLoss][psdl] |
| [StartPartitionQuorumLossAsync][ql] |[Start-ServiceFabricPartitionQuorumLoss][psql] |
| [StartPartitionRestartAsync][rp] |[Start-ServiceFabricPartitionRestart][psrp] |

## <a name="conceptual-overview-of-running-a-command"></a>Översikt över ett kommando körs
Fel Injection och Analysis Services använder en asynkron modell där du startar kommandot med ett API som kallas ”Start” API: et i detta dokument, söker förloppet för det här kommandot med en ”GetProgress” API tills ett avslutat tillstånd, eller tills du avbryta den.
Anropa ”Start”-API: et för motsvarande API för att starta ett kommando.  Detta API returnerar när fel Injection och Analysis Services har accepterat begäran.  Men det visar inte hur långt kommandot har körts, eller om den har startat ännu.  Anropa API som motsvarar ”Start”-API: et tidigare kallade för ”GetProgress” för att kunna kontrollera status för ett kommando.  API för ”GetProgress” returnerar ett objekt som indikerar aktuell status för kommandot i egenskapen tillstånd.  Ett kommando körs under obestämd tid tills:

1. Den har slutförts.  Om du anropar ”GetProgress” på den i det här fallet kommer förlopp objektets tillstånd att slutföras.
2. Ett oåterkalleligt fel uppstår.  Om du anropar ”GetProgress” på den i det här fallet blir förlopp objektets tillstånd felaktig
3. Avbryta via den [CancelTestCommandAsync] [ cancel] API, eller [stoppa ServiceFabricTestCommand] [ cancelps] PowerShell-cmdlet.  Om du anropar ”GetProgress” på den i det här fallet är förlopp objektets tillstånd avbrott eller ForceCancelled, beroende på ett argument för detta API.  Finns i dokumentationen för [CancelTestCommandAsync] [ cancel] för mer information.

## <a name="details-of-running-a-command"></a>Information om ett kommando körs
Anropa Start-API med förväntade argument för att starta ett kommando.  Alla Start-API: er har en Guid-argument med namnet åtgärds-ID.  Du bör hålla koll på argumentet åtgärds-ID, eftersom den används för att spåra förloppet för det här kommandot.  Detta måste överföras till ”GetProgress” API för att kunna spåra förloppet för kommandot.  Åtgärds-ID måste vara unikt.

När du anropar API: et startar, bör GetProgress API: et anropas i en slinga tills objektet returnerade förlopp tillstånd egenskapen har slutförts.  Alla [Fabrictransientexception's] [ fte] och Operationcanceledexceptions bör provas igen.
När kommandot har uppnått ett avslutat tillstånd (slutförd, Faulted eller avbrott), har returnerade förlopp objektets resultatet egenskapen ytterligare information.  Om tillståndet är klar innehåller Result.SelectedPartition.PartitionId partitions-id som har valts.  Result.Exception ska vara null.  Om tillståndet är fel Result.Exception har orsaken fel Injection och Analysis Service fel inträffade i kommandot.  Result.SelectedPartition.PartitionId har partitions-id som har valts.  I vissa situationer kan kan kommandot inte ha gått tillräckligt långt att välja en partition.  I så fall kommer PartitionId vara 0.  Om tillståndet avbryts vara Result.Exception null.  Result.SelectedPartition.PartitionId ha partitions-id som har valts som Faulted fallet, men om kommandot inte gått tillräckligt långt att göra det, kommer det vara 0.  Dessutom finns i exemplet nedan.

Exempelkoden nedan visar hur du startar och sedan följa förloppet på ett kommando för att orsaka dataförlust av på en specifik partition.

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

Exemplet nedan visar hur du använder PartitionSelector för att välja en slumpmässig partition för en angiven tjänst:

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

## <a name="history-and-truncation"></a>Historik över och trunkering
När ett kommando har uppnått ett avslutat tillstånd, förblir dess metadata i fel Injection och Analysis Service för en viss tid innan den tas bort om du vill spara utrymme.  Om ”GetProgress” anropas med ett kommando åtgärds-ID när den har tagits bort, returneras en FabricException med en ErrorCode KeyNotFound.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
