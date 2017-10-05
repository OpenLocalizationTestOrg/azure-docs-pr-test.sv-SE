---
title: "Starta och stoppa klusternoder för att testa Azure mikrotjänster | Microsoft Docs"
description: "Lär dig hur du använder fel injection för att testa ett Service Fabric-program genom att starta och stoppa klusternoder."
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
ms.openlocfilehash: 850fbc0c74811ec942292da64064dec867cd1b9e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="replacing-the-start-node-and-stop-node-apis-with-the-node-transition-api"></a><span data-ttu-id="0adc4-103">Ersätta noden starta och stoppa noden API: er med API: T för noden övergång</span><span class="sxs-lookup"><span data-stu-id="0adc4-103">Replacing the Start Node and Stop node APIs with the Node Transition API</span></span>

## <a name="what-do-the-stop-node-and-start-node-apis-do"></a><span data-ttu-id="0adc4-104">Vad stoppa nod och starta nod API: er?</span><span class="sxs-lookup"><span data-stu-id="0adc4-104">What do the Stop Node and Start Node APIs do?</span></span>

<span data-ttu-id="0adc4-105">Stoppa nod-API (hanterade: [StopNodeAsync()][stopnode], PowerShell: [stoppa ServiceFabricNode][stopnodeps]) stoppar en Service Fabric-nod.</span><span class="sxs-lookup"><span data-stu-id="0adc4-105">The Stop Node API (managed: [StopNodeAsync()][stopnode], PowerShell: [Stop-ServiceFabricNode][stopnodeps]) stops a Service Fabric node.</span></span>  <span data-ttu-id="0adc4-106">En Service Fabric-nod är process, inte en virtuell dator eller en dator – den virtuella datorn eller datorn fortfarande körs.</span><span class="sxs-lookup"><span data-stu-id="0adc4-106">A Service Fabric node is process, not a VM or machine – the VM or machine will still be running.</span></span>  <span data-ttu-id="0adc4-107">För resten av dokumentet innebär ”nod” Service Fabric-noden.</span><span class="sxs-lookup"><span data-stu-id="0adc4-107">For the rest of the document "node" will mean Service Fabric node.</span></span>  <span data-ttu-id="0adc4-108">Stoppa en nod placeras i en *stoppats* tillstånd där den inte är medlem i klustret och kan inte vara värd för tjänster, vilket simulera en *ned* nod.</span><span class="sxs-lookup"><span data-stu-id="0adc4-108">Stopping a node puts it into a *stopped* state where it is not a member of the cluster and cannot host services, thus simulating a *down* node.</span></span>  <span data-ttu-id="0adc4-109">Detta är användbart för fel i systemet för att testa ditt program.</span><span class="sxs-lookup"><span data-stu-id="0adc4-109">This is useful for injecting faults into the system to test your application.</span></span>  <span data-ttu-id="0adc4-110">Starta nod-API (hanterade: [StartNodeAsync()][startnode], PowerShell: [Start ServiceFabricNode][startnodeps]]) ångrar stoppa noden API, som ger noden till normalt läge.</span><span class="sxs-lookup"><span data-stu-id="0adc4-110">The Start Node API (managed: [StartNodeAsync()][startnode], PowerShell: [Start-ServiceFabricNode][startnodeps]]) reverses the Stop Node API,  which brings the node back to a normal state.</span></span>

## <a name="why-are-we-replacing-these"></a><span data-ttu-id="0adc4-111">Varför Vi ersätter dessa?</span><span class="sxs-lookup"><span data-stu-id="0adc4-111">Why are we replacing these?</span></span>

<span data-ttu-id="0adc4-112">Enligt beskrivningen tidigare, en *stoppats* Service Fabric-noden är en nod som avsiktligt mål med stoppa noden API.</span><span class="sxs-lookup"><span data-stu-id="0adc4-112">As described earlier, a *stopped* Service Fabric node is a node intentionally targeted using the Stop Node API.</span></span>  <span data-ttu-id="0adc4-113">En *ned* nod är en nod som anges av någon annan anledning (t.ex. den virtuella datorn eller datorn är inaktiverat).</span><span class="sxs-lookup"><span data-stu-id="0adc4-113">A *down* node is a node that is down for any other reason (e.g. the VM or machine is off).</span></span>  <span data-ttu-id="0adc4-114">Med API stoppa noden systemet inte avslöja information att skilja mellan *stoppats* noder och *ned* noder.</span><span class="sxs-lookup"><span data-stu-id="0adc4-114">With the Stop Node API, the system does not expose information to differentiate between *stopped* nodes and *down* nodes.</span></span>

<span data-ttu-id="0adc4-115">Dessutom kan är vissa fel som returneras av API: erna inte så beskrivande de kunde vara.</span><span class="sxs-lookup"><span data-stu-id="0adc4-115">In addition, some errors returned by these APIs are not as descriptive as they could be.</span></span>  <span data-ttu-id="0adc4-116">Till exempel anropar API: et för stoppa nod på en redan *stoppats* nod returneras felet *InvalidAddress*.</span><span class="sxs-lookup"><span data-stu-id="0adc4-116">For example, invoking the Stop Node API on an already *stopped* node will return the error *InvalidAddress*.</span></span>  <span data-ttu-id="0adc4-117">Det här upplevelsen kan förbättras.</span><span class="sxs-lookup"><span data-stu-id="0adc4-117">This experience could be improved.</span></span>

<span data-ttu-id="0adc4-118">En nod har stoppats för varaktighet är också ”oändlig” tills starta nod-API anropas.</span><span class="sxs-lookup"><span data-stu-id="0adc4-118">Also, the duration a node is stopped for is “infinite” until the Start Node API is invoked.</span></span>  <span data-ttu-id="0adc4-119">Vi har hittat detta kan orsaka problem och kan vara problematiskt.</span><span class="sxs-lookup"><span data-stu-id="0adc4-119">We’ve found this can cause problems and may be error-prone.</span></span>  <span data-ttu-id="0adc4-120">Till exempel har vi sett problem där en användare anropas stoppa noden API: N på en nod och sedan glömt om den.</span><span class="sxs-lookup"><span data-stu-id="0adc4-120">For example, we’ve seen problems where a user invoked the Stop Node API on a node and then forgot about it.</span></span>  <span data-ttu-id="0adc4-121">Var senare oklart om noden var *ned* eller *stoppats*.</span><span class="sxs-lookup"><span data-stu-id="0adc4-121">Later, it was unclear if the node was *down* or *stopped*.</span></span>


## <a name="introducing-the-node-transition-apis"></a><span data-ttu-id="0adc4-122">Introduktion till API: er för noden övergång</span><span class="sxs-lookup"><span data-stu-id="0adc4-122">Introducing the Node Transition APIs</span></span>

<span data-ttu-id="0adc4-123">Vi har åtgärdas problemen ovan i en ny uppsättning API: er.</span><span class="sxs-lookup"><span data-stu-id="0adc4-123">We’ve addressed these issues above in a new set of APIs.</span></span>  <span data-ttu-id="0adc4-124">Den nya noden övergången API (hanterade: [StartNodeTransitionAsync()][snt]) kan användas för att överföra en Service Fabric-nod en *stoppats* tillstånd, eller att överföra den från en *stoppats* till en normal tillstånd.</span><span class="sxs-lookup"><span data-stu-id="0adc4-124">The new Node Transition API (managed: [StartNodeTransitionAsync()][snt]) may be used to transition a Service Fabric node to a *stopped* state, or to transition it from a *stopped* state to a normal up state.</span></span>  <span data-ttu-id="0adc4-125">Observera att ”Start” i namnet API: et inte refererar till början av en nod.</span><span class="sxs-lookup"><span data-stu-id="0adc4-125">Please note that the “Start” in the name of the API does not refer to starting a node.</span></span>  <span data-ttu-id="0adc4-126">Den hänvisar till början av en asynkron åtgärd som systemet ska köras för att övergå till antingen noden *stoppats* eller startat tillstånd.</span><span class="sxs-lookup"><span data-stu-id="0adc4-126">It refers to beginning an asynchronous operation that the system will execute to transition the node to either *stopped* or started state.</span></span>

<span data-ttu-id="0adc4-127">**Användning**</span><span class="sxs-lookup"><span data-stu-id="0adc4-127">**Usage**</span></span>

<span data-ttu-id="0adc4-128">Om noden övergången API inget genereras ett undantag när den anropas sedan systemet har godkänt den asynkrona åtgärden och körs den.</span><span class="sxs-lookup"><span data-stu-id="0adc4-128">If the Node Transition API does not throw an exception when invoked, then the system has accepted the asynchronous operation, and will execute it.</span></span>  <span data-ttu-id="0adc4-129">Lyckade anrop innebär inte åtgärden har slutförts ännu.</span><span class="sxs-lookup"><span data-stu-id="0adc4-129">A successful call does not imply the operation is finished yet.</span></span>  <span data-ttu-id="0adc4-130">Anropa API: T för noden övergången förlopp för att få information om det aktuella tillståndet för åtgärden (hanterade: [GetNodeTransitionProgressAsync()][gntp]) med guid som används när du anropar API: T för noden övergången för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="0adc4-130">To get information about the current state of the operation, call the Node Transition Progress API (managed: [GetNodeTransitionProgressAsync()][gntp]) with the guid used when invoking Node Transition API for this operation.</span></span>  <span data-ttu-id="0adc4-131">API: T för noden övergången förlopp returnerar ett NodeTransitionProgress-objekt.</span><span class="sxs-lookup"><span data-stu-id="0adc4-131">The Node Transition Progress API returns an NodeTransitionProgress object.</span></span>  <span data-ttu-id="0adc4-132">Det här objektet tillstånd egenskapen anger det aktuella tillståndet för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="0adc4-132">This object’s State property specifies the current state of the operation.</span></span>  <span data-ttu-id="0adc4-133">Om tillståndet ”körs” körs igen.</span><span class="sxs-lookup"><span data-stu-id="0adc4-133">If the state is “Running” then the operation is executing.</span></span>  <span data-ttu-id="0adc4-134">Om den är slutförd, avslutad igen utan fel.</span><span class="sxs-lookup"><span data-stu-id="0adc4-134">If it is Completed, the operation finished without error.</span></span>  <span data-ttu-id="0adc4-135">Om det är fel, ett problem uppstod när åtgärden.</span><span class="sxs-lookup"><span data-stu-id="0adc4-135">If it is Faulted, there was a problem executing the operation.</span></span>  <span data-ttu-id="0adc4-136">Egenskapen resultatet Undantagsegenskapen visar vad problemet är.</span><span class="sxs-lookup"><span data-stu-id="0adc4-136">The Result property’s Exception property will indicate what the issue was.</span></span>  <span data-ttu-id="0adc4-137">Se https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate för mer information om egenskapen State och avsnittet ”exempel” nedan kodexempel.</span><span class="sxs-lookup"><span data-stu-id="0adc4-137">See https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate for more information about the State property, and the “Sample Usage” section below for code examples.</span></span>


<span data-ttu-id="0adc4-138">**Skilja mellan en stoppad nod och en inaktiv nod** om en nod *stoppats* med hjälp av noden övergången API, resultatet av en nod-fråga (hanterade: [GetNodeListAsync()][nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) visar att den här noden har en *IsStopped* egenskapsvärdet true.</span><span class="sxs-lookup"><span data-stu-id="0adc4-138">**Differentiating between a stopped node and a down node** If a node is *stopped* using the Node Transition API, the output of a node query (managed: [GetNodeListAsync()][nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) will show that this node has an *IsStopped* property value of true.</span></span>  <span data-ttu-id="0adc4-139">Observera att detta skiljer sig från värdet för den *NodeStatus* -egenskap som talar om *ned*.</span><span class="sxs-lookup"><span data-stu-id="0adc4-139">Note this is different from the value of the *NodeStatus* property, which will say *Down*.</span></span>  <span data-ttu-id="0adc4-140">Om den *NodeStatus* egenskap har ett värde av *ned*, men *IsStopped* är false, och sedan noden inte har stoppats med hjälp av noden övergången API och *ned* på grund av någon anledning.</span><span class="sxs-lookup"><span data-stu-id="0adc4-140">If the *NodeStatus* property has a value of *Down*, but *IsStopped* is false, then the node was not stopped using the Node Transition API, and is *Down* due some other reason.</span></span>  <span data-ttu-id="0adc4-141">Om den *IsStopped* egenskapen är true, och *NodeStatus* egenskapen är *ned*, sedan den stoppades med hjälp av noden övergången API.</span><span class="sxs-lookup"><span data-stu-id="0adc4-141">If the *IsStopped* property is true, and the *NodeStatus* property is *Down*, then it was stopped using the Node Transition API.</span></span>

<span data-ttu-id="0adc4-142">Starta en *stoppats* nod med hjälp av noden övergången API returneras den så att den fungerar som en normal medlem i klustret igen.</span><span class="sxs-lookup"><span data-stu-id="0adc4-142">Starting a *stopped* node using the Node Transition API will return it to function as a normal member of the cluster again.</span></span>  <span data-ttu-id="0adc4-143">Visar resultatet av frågan nod API *IsStopped* som FALSKT och *NodeStatus* som något som inte är nere (t.ex. upp).</span><span class="sxs-lookup"><span data-stu-id="0adc4-143">The output of the node query API will show *IsStopped* as false, and *NodeStatus* as something that is not Down (e.g. Up).</span></span>


<span data-ttu-id="0adc4-144">**Begränsat varaktighet** när noden övergången API för att stoppa en nod, en av de obligatoriska parametrarna *stopNodeDurationInSeconds*, representerar hur lång tid i sekunder som noden *stoppats*.</span><span class="sxs-lookup"><span data-stu-id="0adc4-144">**Limited Duration** When using the Node Transition API to stop a node, one of the required parameters, *stopNodeDurationInSeconds*, represents the amount of time in seconds to keep the node *stopped*.</span></span>  <span data-ttu-id="0adc4-145">Det här värdet måste vara i det tillåtna intervallet som har minst 600 och högst 14400.</span><span class="sxs-lookup"><span data-stu-id="0adc4-145">This value must be in the allowed range, which has a minimum of 600, and a maximum of 14400.</span></span>  <span data-ttu-id="0adc4-146">När denna tid har löpt ut startas noden om sig själv i tillstånd automatiskt.</span><span class="sxs-lookup"><span data-stu-id="0adc4-146">After this time expires, the node will restart itself into Up state automatically.</span></span>  <span data-ttu-id="0adc4-147">Finns i exempel 1 nedan ett exempel på användning.</span><span class="sxs-lookup"><span data-stu-id="0adc4-147">Refer to Sample 1 below for an example of usage.</span></span>

> [!WARNING]
> <span data-ttu-id="0adc4-148">Undvik att noden övergången API: er och stoppa nod och starta nod API: er.</span><span class="sxs-lookup"><span data-stu-id="0adc4-148">Avoid mixing Node Transition APIs and the Stop Node and Start Node APIs.</span></span>  <span data-ttu-id="0adc4-149">Rekommendationen är att använda den nod övergång endast API.</span><span class="sxs-lookup"><span data-stu-id="0adc4-149">The recommendation is to  use the Node Transition API only.</span></span>  <span data-ttu-id="0adc4-150">> Om en nod har redan har stoppats med hjälp av noden API stoppa den ska startas med starta nod API innan du börjar använda den > noden övergången API: er.</span><span class="sxs-lookup"><span data-stu-id="0adc4-150">> If a node has been already been stopped using the Stop Node API, it should be started using the Start Node API first before using the > Node Transition APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="0adc4-151">Flera nod övergången API-anrop kan göras på samma nod parallellt.</span><span class="sxs-lookup"><span data-stu-id="0adc4-151">Multiple Node Transition APIs calls cannot be made on the same node in parallel.</span></span>  <span data-ttu-id="0adc4-152">I en sådan situation nod övergången API kommer > utlösa en FabricException med ett ErrorCode egenskapsvärde för NodeTransitionInProgress.</span><span class="sxs-lookup"><span data-stu-id="0adc4-152">In such a situation, the Node Transition API will    > throw a FabricException with an ErrorCode property value of NodeTransitionInProgress.</span></span>  <span data-ttu-id="0adc4-153">När en nod övergång på en viss nod har > är igång kan du vänta tills åtgärden når ett avslutat tillstånd (slutförd, Faulted eller ForceCancelled) innan du startar en > Ny övergång på samma nod.</span><span class="sxs-lookup"><span data-stu-id="0adc4-153">Once a node transition on a specific node has  > been started, you should wait until the operation reaches a terminal state (Completed, Faulted, or ForceCancelled) before starting a  > new transition on the same node.</span></span>  <span data-ttu-id="0adc4-154">Parallel-nod övergången anrop på olika noder är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="0adc4-154">Parallel node transition calls on different nodes are allowed.</span></span>


#### <a name="sample-usage"></a><span data-ttu-id="0adc4-155">Exempel på användning</span><span class="sxs-lookup"><span data-stu-id="0adc4-155">Sample Usage</span></span>


<span data-ttu-id="0adc4-156">**Exempel 1** -i följande exempel används nod övergången API för att stoppa en nod.</span><span class="sxs-lookup"><span data-stu-id="0adc4-156">**Sample 1** - The following sample uses the Node Transition API to stop a node.</span></span>

```csharp
        // Helper function to get information about a node
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
                        // Inspect the progress object's Result.Exception.HResult to get the error code.
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
            // Uses the GetNodeListAsync() API to get information about the target node
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
                    // Invoke StartNodeTransitionAsync with the NodeStopDescription from above, which will stop the target node.  Retry transient errors.
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

<span data-ttu-id="0adc4-157">**Exempel 2** -följande exempel startar en *stoppats* nod.</span><span class="sxs-lookup"><span data-stu-id="0adc4-157">**Sample 2** - The following sample starts a *stopped* node.</span></span>  <span data-ttu-id="0adc4-158">Vissa hjälpmetoder från det första exemplet används.</span><span class="sxs-lookup"><span data-stu-id="0adc4-158">It uses some helper methods from the first sample.</span></span>

```csharp
        static async Task StartNodeAsync(FabricClient fc, string nodeName)
        {
            // Uses the GetNodeListAsync() API to get information about the target node
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
                    // Invoke StartNodeTransitionAsync with the NodeStartDescription from above, which will start the target stopped node.  Retry transient errors.
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

<span data-ttu-id="0adc4-159">**Exempel 3** -följande exempel visar felaktig användning.</span><span class="sxs-lookup"><span data-stu-id="0adc4-159">**Sample 3** - The following sample shows incorrect usage.</span></span>  <span data-ttu-id="0adc4-160">Denna användning är felaktig eftersom den *stopDurationInSeconds* ger är större än det tillåtna intervallet.</span><span class="sxs-lookup"><span data-stu-id="0adc4-160">This usage is incorrect because the *stopDurationInSeconds* it provides is greater than the allowed range.</span></span>  <span data-ttu-id="0adc4-161">Eftersom StartNodeTransitionAsync() misslyckas med ett allvarligt fel accepterades inte igen och förloppet API ska inte anropas.</span><span class="sxs-lookup"><span data-stu-id="0adc4-161">Since StartNodeTransitionAsync() will fail with a fatal error, the operation was not accepted, and the progress API should not be called.</span></span>  <span data-ttu-id="0adc4-162">Det här exemplet använder vissa hjälpmetoder från det första exemplet.</span><span class="sxs-lookup"><span data-stu-id="0adc4-162">This sample uses some helper methods from the first sample.</span></span>

```csharp
        static async Task StopNodeWithOutOfRangeDurationAsync(FabricClient fc, string nodeName)
        {
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();

            // Use an out of range value for stopDurationInSeconds to demonstrate error
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

<span data-ttu-id="0adc4-163">**Exempel 4** -i följande exempel visas information om felet som returneras från API: et för noden övergången pågår när åtgärden som initierats av API: T för noden övergången har godkänts, men inte senare vid körning.</span><span class="sxs-lookup"><span data-stu-id="0adc4-163">**Sample 4** - The following sample shows the error information that will be returned from the Node Transition Progress API when the operation initiated by the Node Transition API is accepted, but fails later while executing.</span></span>  <span data-ttu-id="0adc4-164">I fallet kan misslyckas eftersom API: T för noden övergången försöker starta en nod som inte finns.</span><span class="sxs-lookup"><span data-stu-id="0adc4-164">In the case, it fails because the Node Transition API attempts to start a node that does not exist.</span></span>  <span data-ttu-id="0adc4-165">Det här exemplet använder vissa hjälpmetoder från det första exemplet.</span><span class="sxs-lookup"><span data-stu-id="0adc4-165">This sample uses some helper methods from the first sample.</span></span>

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
                    // Invoke StartNodeTransitionAsync with the NodeStartDescription from above, which will start the target stopped node.  Retry transient errors.
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

            // Now call StartNodeTransitionProgressAsync() until the desired state is reached.  In this case, it will end up in the Faulted state since the node does not exist.
            // When StartNodeTransitionProgressAsync()'s returned progress object has a State if Faulted, inspect the progress object's Result.Exception.HResult to get the error code.
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
