---
title: "aaaStart och stoppa klustret noder tootest Azure mikrotjänster | Microsoft Docs"
description: "Lär dig hur toouse fault injection tootest ett Service Fabric-program genom att starta och stoppa klusternoder."
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
ms.date: 6/12/2017
ms.author: lemai
ms.openlocfilehash: 7d3f5147328e6233a67533fbfb2a525aa5fc060e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replacing-hello-start-node-and-stop-node-apis-with-hello-node-transition-api"></a>Ersätta hello noden starta och stoppa nod API: er med hello nod övergången API

## <a name="what-do-hello-stop-node-and-start-node-apis-do"></a>Vad hello stoppa nod och starta nod API: er använda?

hello stoppa noden API (hanterade: [StopNodeAsync()][stopnode], PowerShell: [stoppa ServiceFabricNode][stopnodeps]) stoppar en Service Fabric-nod.  En Service Fabric-nod är inte en virtuell dator eller datorn – hello VM eller datorn fortfarande körs.  Hello resten av dokumentet hello betyder ”nod” Service Fabric-noden.  Stoppa en nod placeras i en *stoppats* tillstånd där den inte är medlem i hello kluster och kan inte vara värd för tjänster, vilket simulera en *ned* nod.  Detta är användbart för fel i hello system tootest ditt program.  hello starta nod API (hanterade: [StartNodeAsync()][startnode], PowerShell: [Start ServiceFabricNode][startnodeps]]) omvänd hello stoppa noden API  som ger hello nod tillbaka tooa normala tillståndet.

## <a name="why-are-we-replacing-these"></a>Varför Vi ersätter dessa?

Enligt beskrivningen tidigare, en *stoppats* Service Fabric-noden är en nod som avsiktligt mål med hello stoppa noden API.  En *ned* nod är en nod som anges av någon annan anledning (t.ex. hello VM eller datorn är inaktiverat).  Med hello stoppa noden API, visar inte hello system information toodifferentiate mellan *stoppats* noder och *ned* noder.

Dessutom kan är vissa fel som returneras av API: erna inte så beskrivande de kunde vara.  Till exempel anropar hello stoppa noden API på en redan *stoppats* hello fel returneras nod *InvalidAddress*.  Det här upplevelsen kan förbättras.

Hello varaktighet som en nod har stoppats för är också ”oändlig” tills hello starta nod API anropas.  Vi har hittat detta kan orsaka problem och kan vara problematiskt.  Till exempel har vi sett problem där en användare anropas hello stoppa noden API på en nod och sedan glömt om den.  Var senare oklart om hello nod *ned* eller *stoppats*.


## <a name="introducing-hello-node-transition-apis"></a>Introduktion till hello nod övergången API: er

Vi har åtgärdas problemen ovan i en ny uppsättning API: er.  hello nya nod övergången API (hanterade: [StartNodeTransitionAsync()][snt]) kanske används tootransition tooa ett Service Fabric-noden *stoppats* tillstånd eller tootransition den från en *stoppats* tillstånd tooa normala tillstånd.  Observera att hello ”Start” i hello namnet på hello API inte refererar toostarting en nod.  Den hänvisar toobeginning en asynkron åtgärd hello system körs tootransition hello nod tooeither *stoppats* eller startat tillstånd.

**Användning**

Om hello nod övergången API inget genereras ett undantag vid aktivering hello system har accepterat hello asynkron åtgärd och sedan körs den.  Lyckade anrop innebär inte hello-åtgärden har slutförts ännu.  tooget information om hello hello åtgärdens, anrop hello nod övergången förlopp API aktuella status (hanterade: [GetNodeTransitionProgressAsync()][gntp]) med hello guid som används när du anropar nod Övergången API för den här åtgärden.  hello nod övergången förlopp API returnerar ett NodeTransitionProgress-objekt.  Det här objektet tillstånd egenskapen anger hello hello åtgärdens aktuella status.  Om hello tillståndet ”körs” körs hello-åtgärden.  Om den är klar hello åtgärden slutförts utan fel.  Om det är fel, ett problem uppstod hello åtgärden.  hello resultatet egenskapen undantaget egenskapen visar vilka hello utfärda var.  Se https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate för mer information om hello tillstånd egenskapen och hello ”exempel” nedan för kodexempel.


**Skilja mellan en stoppad nod och en inaktiv nod** om en nod *stoppats* med hello nod övergången API, hello utdata från en nod-fråga (hanterade: [GetNodeListAsync()] [ nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) visar att den här noden har en *IsStopped* egenskapsvärdet true.  Observera att detta skiljer sig från värdet hello hello *NodeStatus* -egenskap som talar om *ned*.  Om hello *NodeStatus* egenskap har ett värde av *ned*, men *IsStopped* är FALSKT hello nod inte har stoppats med hello nod övergången API och är  *Ned* på grund av någon anledning.  Om hello *IsStopped* egenskapen är true och hello *NodeStatus* egenskapen är *ned*, sedan den stoppades med hello nod övergången API.

Starta en *stoppats* noden med hello nod övergången API returnerar den toofunction som en normal medlem hello klustret igen.  hello utdata av hello noden frågan API visar *IsStopped* som FALSKT och *NodeStatus* som något som inte är nere (t.ex. upp).


**Begränsat varaktighet** när du använder hello nod övergången API toostop en nod, en hello obligatoriska parametrar, *stopNodeDurationInSeconds*, representerar hello tid i sekunder tookeep hello noden  *stoppats*.  Det här värdet måste vara i hello tillåtet intervall som har minst 600 och högst 14400.  När denna tid har löpt ut startas hello nod om sig själv i tillstånd automatiskt.  Läs tooSample 1 nedan ett exempel på användning.

> [!WARNING]
> Undvika nod övergången API: er och hello stoppa nod och starta nod API: er.  hello rekommendation är att använda hello nod övergången API för.  > Om en nod har redan har slutat använda hello stoppa noden API, den ska startas först med hello starta nod API innan du använder hello > noden övergången API: er.

> [!WARNING]
> Flera nod övergången API-anrop kan göras på hello samma nod parallellt.  I en sådan situation hello nod övergången API kommer > utlösa en FabricException med ett ErrorCode egenskapsvärde för NodeTransitionInProgress.  När en nod övergång på en viss nod har > är igång kan du vänta tills hello åtgärden når ett avslutat tillstånd (slutförd, Faulted eller ForceCancelled) innan du startar en > Ny övergång på hello samma nod.  Parallel-nod övergången anrop på olika noder är tillåtna.


#### <a name="sample-usage"></a>Exempel på användning


**Exempel 1** -hello följande exempel använder hello nod övergången API toostop en nod.

```csharp
        // Helper function tooget information about a node
        static Node GetNodeInfo(FabricClient fc, string node)
        {
            NodeList n = null;
            while (n == null)
            {
                n = fc.QueryManager.GetNodeListAsync(node).GetAwaiter().GetResult();
                Task.Delay(TimeSpan.FromSeconds(1)).GetAwaiter();
            };

            return n.FirstOrDefault();
        }

        static async Task WaitForStateAsync(FabricClient fc, Guid operationId, TestCommandProgressState targetState)
        {
            NodeTransitionProgress progress = null;

            do
            {
                bool exceptionObserved = false;
                try
                {
                    progress = await fc.TestManager.GetNodeTransitionProgressAsync(operationId, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                    exceptionObserved = true;
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                    exceptionObserved = true;
                }

                if (!exceptionObserved)
                {
                    Console.WriteLine("Current state of operation '{0}': {1}", operationId, progress.State);

                    if (progress.State == TestCommandProgressState.Faulted)
                    {
                        // Inspect hello progress object's Result.Exception.HResult tooget hello error code.
                        Console.WriteLine("'{0}' failed with: {1}, HResult: {2}", operationId, progress.Result.Exception, progress.Result.Exception.HResult);

                        // ...additional logic as required
                    }

                    if (progress.State == targetState)
                    {
                        Console.WriteLine("Target state '{0}' has been reached", targetState);
                        break;
                    }
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (true);
        }

        static async Task StopNodeAsync(FabricClient fc, string nodeName, int durationInSeconds)
        {
            // Uses hello GetNodeListAsync() API tooget information about hello target node
            Node n = GetNodeInfo(fc, nodeName);

            // Create a Guid
            Guid guid = Guid.NewGuid();

            // Create a NodeStopDescription object, which will be used as a parameter into StartNodeTransition
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, durationInSeconds);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with hello NodeStopDescription from above, which will stop hello target node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    // This is retryable
                }
                catch (FabricTransientException fte)
                {
                    // This is retryable
                }

                // Backoff
                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hte desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Exempel 2** - hello följande exempel startar en *stoppats* nod.  Vissa hjälpmetoder från hello första exemplet används.

```csharp
        static async Task StartNodeAsync(FabricClient fc, string nodeName)
        {
            // Uses hello GetNodeListAsync() API tooget information about hello target node
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = n.NodeInstanceId;

            // Create a NodeStartDescription object, which will be used as a parameter into StartNodeTransition
            NodeStartDescription description = new NodeStartDescription(guid, n.NodeName, nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with hello NodeStartDescription from above, which will start hello target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hte desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Exempel 3** - hello följande exempel visar felaktig användning.  Denna användning är felaktigt eftersom hello *stopDurationInSeconds* ger är större än hello tillåtet intervall.  Eftersom StartNodeTransitionAsync() misslyckas med ett allvarligt fel hello åtgärden accepteras inte och hello förlopp API ska inte anropas.  Det här exemplet använder vissa hjälpmetoder från hello första exemplet.

```csharp
        static async Task StopNodeWithOutOfRangeDurationAsync(FabricClient fc, string nodeName)
        {
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();

            // Use an out of range value for stopDurationInSeconds toodemonstrate error
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, 99999);

            try
            {
                await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
            }

            catch (FabricException e)
            {
                Console.WriteLine("Caught {0}", e);
                Console.WriteLine("ErrorCode {0}", e.ErrorCode);
                // Output:
                // Caught System.Fabric.FabricException: System.Runtime.InteropServices.COMException (-2147017629)
                // StopDurationInSeconds is out of range ---> System.Runtime.InteropServices.COMException: Exception from HRESULT: 0x80071C63
                // << Parts of exception omitted>>
                //
                // ErrorCode InvalidDuration
            }
        }
```

**Exempel 4** - hello följande exempel visas information om hello fel som returneras från hello nod övergången förlopp API när hello-åtgärden initierades av hello nod övergången API har godkänts, men inte senare vid körning.  I fallet hello misslyckas eftersom hello nod övergången API försöker toostart en nod som inte finns.  Det här exemplet använder vissa hjälpmetoder från hello första exemplet.

```csharp
        static async Task StartNodeWithNonexistentNodeAsync(FabricClient fc)
        {
            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = 12345;

            // Intentionally use a nonexistent node
            NodeStartDescription description = new NodeStartDescription(guid, "NonexistentNode", nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with hello NodeStartDescription from above, which will start hello target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hello desired state is reached.  In this case, it will end up in hello Faulted state since hello node does not exist.
            // When StartNodeTransitionProgressAsync()'s returned progress object has a State if Faulted, inspect hello progress object's Result.Exception.HResult tooget hello error code.
            // In this case, it will be NodeNotFound.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Faulted).ConfigureAwait(false);
        }
```

[stopnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN#System_Fabric_FabricClient_FaultManagementClient_StopNodeAsync_System_String_System_Numerics_BigInteger_System_Fabric_CompletionMode_
[stopnodeps]: https://msdn.microsoft.com/library/mt125982.aspx
[startnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN#System_Fabric_FabricClient_FaultManagementClient_StartNodeAsync_System_String_System_Numerics_BigInteger_System_String_System_Int32_System_Fabric_CompletionMode_System_Threading_CancellationToken_
[startnodeps]: https://msdn.microsoft.com/library/mt163520.aspx
[nodequery]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient#System_Fabric_FabricClient_QueryClient_GetNodeListAsync_System_String_
[nodequeryps]: https://docs.microsoft.com/powershell/servicefabric/vlatest/Get-ServiceFabricNode?redirectedfrom=msdn
[snt]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_StartNodeTransitionAsync_System_Fabric_Description_NodeTransitionDescription_System_TimeSpan_System_Threading_CancellationToken_
[gntp]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_GetNodeTransitionProgressAsync_System_Guid_System_TimeSpan_System_Threading_CancellationToken_
