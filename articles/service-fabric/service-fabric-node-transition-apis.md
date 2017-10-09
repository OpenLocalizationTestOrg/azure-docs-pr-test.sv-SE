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
# <a name="replacing-hello-start-node-and-stop-node-apis-with-hello-node-transition-api"></a><span data-ttu-id="fa643-103">Ersätta hello noden starta och stoppa nod API: er med hello nod övergången API</span><span class="sxs-lookup"><span data-stu-id="fa643-103">Replacing hello Start Node and Stop node APIs with hello Node Transition API</span></span>

## <a name="what-do-hello-stop-node-and-start-node-apis-do"></a><span data-ttu-id="fa643-104">Vad hello stoppa nod och starta nod API: er använda?</span><span class="sxs-lookup"><span data-stu-id="fa643-104">What do hello Stop Node and Start Node APIs do?</span></span>

<span data-ttu-id="fa643-105">hello stoppa noden API (hanterade: [StopNodeAsync()][stopnode], PowerShell: [stoppa ServiceFabricNode][stopnodeps]) stoppar en Service Fabric-nod.</span><span class="sxs-lookup"><span data-stu-id="fa643-105">hello Stop Node API (managed: [StopNodeAsync()][stopnode], PowerShell: [Stop-ServiceFabricNode][stopnodeps]) stops a Service Fabric node.</span></span>  <span data-ttu-id="fa643-106">En Service Fabric-nod är inte en virtuell dator eller datorn – hello VM eller datorn fortfarande körs.</span><span class="sxs-lookup"><span data-stu-id="fa643-106">A Service Fabric node is process, not a VM or machine – hello VM or machine will still be running.</span></span>  <span data-ttu-id="fa643-107">Hello resten av dokumentet hello betyder ”nod” Service Fabric-noden.</span><span class="sxs-lookup"><span data-stu-id="fa643-107">For hello rest of hello document "node" will mean Service Fabric node.</span></span>  <span data-ttu-id="fa643-108">Stoppa en nod placeras i en *stoppats* tillstånd där den inte är medlem i hello kluster och kan inte vara värd för tjänster, vilket simulera en *ned* nod.</span><span class="sxs-lookup"><span data-stu-id="fa643-108">Stopping a node puts it into a *stopped* state where it is not a member of hello cluster and cannot host services, thus simulating a *down* node.</span></span>  <span data-ttu-id="fa643-109">Detta är användbart för fel i hello system tootest ditt program.</span><span class="sxs-lookup"><span data-stu-id="fa643-109">This is useful for injecting faults into hello system tootest your application.</span></span>  <span data-ttu-id="fa643-110">hello starta nod API (hanterade: [StartNodeAsync()][startnode], PowerShell: [Start ServiceFabricNode][startnodeps]]) omvänd hello stoppa noden API  som ger hello nod tillbaka tooa normala tillståndet.</span><span class="sxs-lookup"><span data-stu-id="fa643-110">hello Start Node API (managed: [StartNodeAsync()][startnode], PowerShell: [Start-ServiceFabricNode][startnodeps]]) reverses hello Stop Node API,  which brings hello node back tooa normal state.</span></span>

## <a name="why-are-we-replacing-these"></a><span data-ttu-id="fa643-111">Varför Vi ersätter dessa?</span><span class="sxs-lookup"><span data-stu-id="fa643-111">Why are we replacing these?</span></span>

<span data-ttu-id="fa643-112">Enligt beskrivningen tidigare, en *stoppats* Service Fabric-noden är en nod som avsiktligt mål med hello stoppa noden API.</span><span class="sxs-lookup"><span data-stu-id="fa643-112">As described earlier, a *stopped* Service Fabric node is a node intentionally targeted using hello Stop Node API.</span></span>  <span data-ttu-id="fa643-113">En *ned* nod är en nod som anges av någon annan anledning (t.ex. hello VM eller datorn är inaktiverat).</span><span class="sxs-lookup"><span data-stu-id="fa643-113">A *down* node is a node that is down for any other reason (e.g. hello VM or machine is off).</span></span>  <span data-ttu-id="fa643-114">Med hello stoppa noden API, visar inte hello system information toodifferentiate mellan *stoppats* noder och *ned* noder.</span><span class="sxs-lookup"><span data-stu-id="fa643-114">With hello Stop Node API, hello system does not expose information toodifferentiate between *stopped* nodes and *down* nodes.</span></span>

<span data-ttu-id="fa643-115">Dessutom kan är vissa fel som returneras av API: erna inte så beskrivande de kunde vara.</span><span class="sxs-lookup"><span data-stu-id="fa643-115">In addition, some errors returned by these APIs are not as descriptive as they could be.</span></span>  <span data-ttu-id="fa643-116">Till exempel anropar hello stoppa noden API på en redan *stoppats* hello fel returneras nod *InvalidAddress*.</span><span class="sxs-lookup"><span data-stu-id="fa643-116">For example, invoking hello Stop Node API on an already *stopped* node will return hello error *InvalidAddress*.</span></span>  <span data-ttu-id="fa643-117">Det här upplevelsen kan förbättras.</span><span class="sxs-lookup"><span data-stu-id="fa643-117">This experience could be improved.</span></span>

<span data-ttu-id="fa643-118">Hello varaktighet som en nod har stoppats för är också ”oändlig” tills hello starta nod API anropas.</span><span class="sxs-lookup"><span data-stu-id="fa643-118">Also, hello duration a node is stopped for is “infinite” until hello Start Node API is invoked.</span></span>  <span data-ttu-id="fa643-119">Vi har hittat detta kan orsaka problem och kan vara problematiskt.</span><span class="sxs-lookup"><span data-stu-id="fa643-119">We’ve found this can cause problems and may be error-prone.</span></span>  <span data-ttu-id="fa643-120">Till exempel har vi sett problem där en användare anropas hello stoppa noden API på en nod och sedan glömt om den.</span><span class="sxs-lookup"><span data-stu-id="fa643-120">For example, we’ve seen problems where a user invoked hello Stop Node API on a node and then forgot about it.</span></span>  <span data-ttu-id="fa643-121">Var senare oklart om hello nod *ned* eller *stoppats*.</span><span class="sxs-lookup"><span data-stu-id="fa643-121">Later, it was unclear if hello node was *down* or *stopped*.</span></span>


## <a name="introducing-hello-node-transition-apis"></a><span data-ttu-id="fa643-122">Introduktion till hello nod övergången API: er</span><span class="sxs-lookup"><span data-stu-id="fa643-122">Introducing hello Node Transition APIs</span></span>

<span data-ttu-id="fa643-123">Vi har åtgärdas problemen ovan i en ny uppsättning API: er.</span><span class="sxs-lookup"><span data-stu-id="fa643-123">We’ve addressed these issues above in a new set of APIs.</span></span>  <span data-ttu-id="fa643-124">hello nya nod övergången API (hanterade: [StartNodeTransitionAsync()][snt]) kanske används tootransition tooa ett Service Fabric-noden *stoppats* tillstånd eller tootransition den från en *stoppats* tillstånd tooa normala tillstånd.</span><span class="sxs-lookup"><span data-stu-id="fa643-124">hello new Node Transition API (managed: [StartNodeTransitionAsync()][snt]) may be used tootransition a Service Fabric node tooa *stopped* state, or tootransition it from a *stopped* state tooa normal up state.</span></span>  <span data-ttu-id="fa643-125">Observera att hello ”Start” i hello namnet på hello API inte refererar toostarting en nod.</span><span class="sxs-lookup"><span data-stu-id="fa643-125">Please note that hello “Start” in hello name of hello API does not refer toostarting a node.</span></span>  <span data-ttu-id="fa643-126">Den hänvisar toobeginning en asynkron åtgärd hello system körs tootransition hello nod tooeither *stoppats* eller startat tillstånd.</span><span class="sxs-lookup"><span data-stu-id="fa643-126">It refers toobeginning an asynchronous operation that hello system will execute tootransition hello node tooeither *stopped* or started state.</span></span>

<span data-ttu-id="fa643-127">**Användning**</span><span class="sxs-lookup"><span data-stu-id="fa643-127">**Usage**</span></span>

<span data-ttu-id="fa643-128">Om hello nod övergången API inget genereras ett undantag vid aktivering hello system har accepterat hello asynkron åtgärd och sedan körs den.</span><span class="sxs-lookup"><span data-stu-id="fa643-128">If hello Node Transition API does not throw an exception when invoked, then hello system has accepted hello asynchronous operation, and will execute it.</span></span>  <span data-ttu-id="fa643-129">Lyckade anrop innebär inte hello-åtgärden har slutförts ännu.</span><span class="sxs-lookup"><span data-stu-id="fa643-129">A successful call does not imply hello operation is finished yet.</span></span>  <span data-ttu-id="fa643-130">tooget information om hello hello åtgärdens, anrop hello nod övergången förlopp API aktuella status (hanterade: [GetNodeTransitionProgressAsync()][gntp]) med hello guid som används när du anropar nod Övergången API för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="fa643-130">tooget information about hello current state of hello operation, call hello Node Transition Progress API (managed: [GetNodeTransitionProgressAsync()][gntp]) with hello guid used when invoking Node Transition API for this operation.</span></span>  <span data-ttu-id="fa643-131">hello nod övergången förlopp API returnerar ett NodeTransitionProgress-objekt.</span><span class="sxs-lookup"><span data-stu-id="fa643-131">hello Node Transition Progress API returns an NodeTransitionProgress object.</span></span>  <span data-ttu-id="fa643-132">Det här objektet tillstånd egenskapen anger hello hello åtgärdens aktuella status.</span><span class="sxs-lookup"><span data-stu-id="fa643-132">This object’s State property specifies hello current state of hello operation.</span></span>  <span data-ttu-id="fa643-133">Om hello tillståndet ”körs” körs hello-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="fa643-133">If hello state is “Running” then hello operation is executing.</span></span>  <span data-ttu-id="fa643-134">Om den är klar hello åtgärden slutförts utan fel.</span><span class="sxs-lookup"><span data-stu-id="fa643-134">If it is Completed, hello operation finished without error.</span></span>  <span data-ttu-id="fa643-135">Om det är fel, ett problem uppstod hello åtgärden.</span><span class="sxs-lookup"><span data-stu-id="fa643-135">If it is Faulted, there was a problem executing hello operation.</span></span>  <span data-ttu-id="fa643-136">hello resultatet egenskapen undantaget egenskapen visar vilka hello utfärda var.</span><span class="sxs-lookup"><span data-stu-id="fa643-136">hello Result property’s Exception property will indicate what hello issue was.</span></span>  <span data-ttu-id="fa643-137">Se https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate för mer information om hello tillstånd egenskapen och hello ”exempel” nedan för kodexempel.</span><span class="sxs-lookup"><span data-stu-id="fa643-137">See https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate for more information about hello State property, and hello “Sample Usage” section below for code examples.</span></span>


<span data-ttu-id="fa643-138">**Skilja mellan en stoppad nod och en inaktiv nod** om en nod *stoppats* med hello nod övergången API, hello utdata från en nod-fråga (hanterade: [GetNodeListAsync()] [ nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) visar att den här noden har en *IsStopped* egenskapsvärdet true.</span><span class="sxs-lookup"><span data-stu-id="fa643-138">**Differentiating between a stopped node and a down node** If a node is *stopped* using hello Node Transition API, hello output of a node query (managed: [GetNodeListAsync()][nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) will show that this node has an *IsStopped* property value of true.</span></span>  <span data-ttu-id="fa643-139">Observera att detta skiljer sig från värdet hello hello *NodeStatus* -egenskap som talar om *ned*.</span><span class="sxs-lookup"><span data-stu-id="fa643-139">Note this is different from hello value of hello *NodeStatus* property, which will say *Down*.</span></span>  <span data-ttu-id="fa643-140">Om hello *NodeStatus* egenskap har ett värde av *ned*, men *IsStopped* är FALSKT hello nod inte har stoppats med hello nod övergången API och är  *Ned* på grund av någon anledning.</span><span class="sxs-lookup"><span data-stu-id="fa643-140">If hello *NodeStatus* property has a value of *Down*, but *IsStopped* is false, then hello node was not stopped using hello Node Transition API, and is *Down* due some other reason.</span></span>  <span data-ttu-id="fa643-141">Om hello *IsStopped* egenskapen är true och hello *NodeStatus* egenskapen är *ned*, sedan den stoppades med hello nod övergången API.</span><span class="sxs-lookup"><span data-stu-id="fa643-141">If hello *IsStopped* property is true, and hello *NodeStatus* property is *Down*, then it was stopped using hello Node Transition API.</span></span>

<span data-ttu-id="fa643-142">Starta en *stoppats* noden med hello nod övergången API returnerar den toofunction som en normal medlem hello klustret igen.</span><span class="sxs-lookup"><span data-stu-id="fa643-142">Starting a *stopped* node using hello Node Transition API will return it toofunction as a normal member of hello cluster again.</span></span>  <span data-ttu-id="fa643-143">hello utdata av hello noden frågan API visar *IsStopped* som FALSKT och *NodeStatus* som något som inte är nere (t.ex. upp).</span><span class="sxs-lookup"><span data-stu-id="fa643-143">hello output of hello node query API will show *IsStopped* as false, and *NodeStatus* as something that is not Down (e.g. Up).</span></span>


<span data-ttu-id="fa643-144">**Begränsat varaktighet** när du använder hello nod övergången API toostop en nod, en hello obligatoriska parametrar, *stopNodeDurationInSeconds*, representerar hello tid i sekunder tookeep hello noden  *stoppats*.</span><span class="sxs-lookup"><span data-stu-id="fa643-144">**Limited Duration** When using hello Node Transition API toostop a node, one of hello required parameters, *stopNodeDurationInSeconds*, represents hello amount of time in seconds tookeep hello node *stopped*.</span></span>  <span data-ttu-id="fa643-145">Det här värdet måste vara i hello tillåtet intervall som har minst 600 och högst 14400.</span><span class="sxs-lookup"><span data-stu-id="fa643-145">This value must be in hello allowed range, which has a minimum of 600, and a maximum of 14400.</span></span>  <span data-ttu-id="fa643-146">När denna tid har löpt ut startas hello nod om sig själv i tillstånd automatiskt.</span><span class="sxs-lookup"><span data-stu-id="fa643-146">After this time expires, hello node will restart itself into Up state automatically.</span></span>  <span data-ttu-id="fa643-147">Läs tooSample 1 nedan ett exempel på användning.</span><span class="sxs-lookup"><span data-stu-id="fa643-147">Refer tooSample 1 below for an example of usage.</span></span>

> [!WARNING]
> <span data-ttu-id="fa643-148">Undvika nod övergången API: er och hello stoppa nod och starta nod API: er.</span><span class="sxs-lookup"><span data-stu-id="fa643-148">Avoid mixing Node Transition APIs and hello Stop Node and Start Node APIs.</span></span>  <span data-ttu-id="fa643-149">hello rekommendation är att använda hello nod övergången API för.</span><span class="sxs-lookup"><span data-stu-id="fa643-149">hello recommendation is too use hello Node Transition API only.</span></span>  <span data-ttu-id="fa643-150">> Om en nod har redan har slutat använda hello stoppa noden API, den ska startas först med hello starta nod API innan du använder hello > noden övergången API: er.</span><span class="sxs-lookup"><span data-stu-id="fa643-150">> If a node has been already been stopped using hello Stop Node API, it should be started using hello Start Node API first before using hello > Node Transition APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="fa643-151">Flera nod övergången API-anrop kan göras på hello samma nod parallellt.</span><span class="sxs-lookup"><span data-stu-id="fa643-151">Multiple Node Transition APIs calls cannot be made on hello same node in parallel.</span></span>  <span data-ttu-id="fa643-152">I en sådan situation hello nod övergången API kommer > utlösa en FabricException med ett ErrorCode egenskapsvärde för NodeTransitionInProgress.</span><span class="sxs-lookup"><span data-stu-id="fa643-152">In such a situation, hello Node Transition API will    > throw a FabricException with an ErrorCode property value of NodeTransitionInProgress.</span></span>  <span data-ttu-id="fa643-153">När en nod övergång på en viss nod har > är igång kan du vänta tills hello åtgärden når ett avslutat tillstånd (slutförd, Faulted eller ForceCancelled) innan du startar en > Ny övergång på hello samma nod.</span><span class="sxs-lookup"><span data-stu-id="fa643-153">Once a node transition on a specific node has  > been started, you should wait until hello operation reaches a terminal state (Completed, Faulted, or ForceCancelled) before starting a  > new transition on hello same node.</span></span>  <span data-ttu-id="fa643-154">Parallel-nod övergången anrop på olika noder är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="fa643-154">Parallel node transition calls on different nodes are allowed.</span></span>


#### <a name="sample-usage"></a><span data-ttu-id="fa643-155">Exempel på användning</span><span class="sxs-lookup"><span data-stu-id="fa643-155">Sample Usage</span></span>


<span data-ttu-id="fa643-156">**Exempel 1** -hello följande exempel använder hello nod övergången API toostop en nod.</span><span class="sxs-lookup"><span data-stu-id="fa643-156">**Sample 1** - hello following sample uses hello Node Transition API toostop a node.</span></span>

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

<span data-ttu-id="fa643-157">**Exempel 2** - hello följande exempel startar en *stoppats* nod.</span><span class="sxs-lookup"><span data-stu-id="fa643-157">**Sample 2** - hello following sample starts a *stopped* node.</span></span>  <span data-ttu-id="fa643-158">Vissa hjälpmetoder från hello första exemplet används.</span><span class="sxs-lookup"><span data-stu-id="fa643-158">It uses some helper methods from hello first sample.</span></span>

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

<span data-ttu-id="fa643-159">**Exempel 3** - hello följande exempel visar felaktig användning.</span><span class="sxs-lookup"><span data-stu-id="fa643-159">**Sample 3** - hello following sample shows incorrect usage.</span></span>  <span data-ttu-id="fa643-160">Denna användning är felaktigt eftersom hello *stopDurationInSeconds* ger är större än hello tillåtet intervall.</span><span class="sxs-lookup"><span data-stu-id="fa643-160">This usage is incorrect because hello *stopDurationInSeconds* it provides is greater than hello allowed range.</span></span>  <span data-ttu-id="fa643-161">Eftersom StartNodeTransitionAsync() misslyckas med ett allvarligt fel hello åtgärden accepteras inte och hello förlopp API ska inte anropas.</span><span class="sxs-lookup"><span data-stu-id="fa643-161">Since StartNodeTransitionAsync() will fail with a fatal error, hello operation was not accepted, and hello progress API should not be called.</span></span>  <span data-ttu-id="fa643-162">Det här exemplet använder vissa hjälpmetoder från hello första exemplet.</span><span class="sxs-lookup"><span data-stu-id="fa643-162">This sample uses some helper methods from hello first sample.</span></span>

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

<span data-ttu-id="fa643-163">**Exempel 4** - hello följande exempel visas information om hello fel som returneras från hello nod övergången förlopp API när hello-åtgärden initierades av hello nod övergången API har godkänts, men inte senare vid körning.</span><span class="sxs-lookup"><span data-stu-id="fa643-163">**Sample 4** - hello following sample shows hello error information that will be returned from hello Node Transition Progress API when hello operation initiated by hello Node Transition API is accepted, but fails later while executing.</span></span>  <span data-ttu-id="fa643-164">I fallet hello misslyckas eftersom hello nod övergången API försöker toostart en nod som inte finns.</span><span class="sxs-lookup"><span data-stu-id="fa643-164">In hello case, it fails because hello Node Transition API attempts toostart a node that does not exist.</span></span>  <span data-ttu-id="fa643-165">Det här exemplet använder vissa hjälpmetoder från hello första exemplet.</span><span class="sxs-lookup"><span data-stu-id="fa643-165">This sample uses some helper methods from hello first sample.</span></span>

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
