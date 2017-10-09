---
title: aaaService Fabric klustret Resource Manager - programgrupper | Microsoft Docs
description: "Översikt över hello programgruppen funktioner i hello resurshanteraren för Service Fabric-kluster"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 4cae2370-77b3-49ce-bf40-030400c4260d
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: b4f068862d962b53a0b3ea813b89bb13ee395681
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooapplication-groups"></a><span data-ttu-id="97a4c-103">Introduktion tooApplication grupper</span><span class="sxs-lookup"><span data-stu-id="97a4c-103">Introduction tooApplication Groups</span></span>
<span data-ttu-id="97a4c-104">Resurshanteraren för Service Fabric-klustret hanterar vanligtvis klusterresurser genom att sprida belastningen hello (representeras [mått](service-fabric-cluster-resource-manager-metrics.md)) jämnt över hello klustret.</span><span class="sxs-lookup"><span data-stu-id="97a4c-104">Service Fabric's Cluster Resource Manager typically manages cluster resources by spreading hello load (represented via [Metrics](service-fabric-cluster-resource-manager-metrics.md)) evenly throughout hello cluster.</span></span> <span data-ttu-id="97a4c-105">Service Fabric hanterar hello kapacitet hello noder i hello och hello klustret som helhet via [kapacitet](service-fabric-cluster-resource-manager-cluster-description.md).</span><span class="sxs-lookup"><span data-stu-id="97a4c-105">Service Fabric manages hello capacity of hello nodes in hello cluster and hello cluster as a whole via [capacity](service-fabric-cluster-resource-manager-cluster-description.md).</span></span> <span data-ttu-id="97a4c-106">Mått och kapacitet fungerar bra för många arbetsbelastningar, men mönster som använder olika Service Fabric-programinstanser ibland hämtas ytterligare krav.</span><span class="sxs-lookup"><span data-stu-id="97a4c-106">Metrics and capacity work great for many workloads, but patterns that make heavy use of different Service Fabric Application Instances sometimes bring in additional requirements.</span></span> <span data-ttu-id="97a4c-107">Till exempel kanske du vill:</span><span class="sxs-lookup"><span data-stu-id="97a4c-107">For example you may want to:</span></span>

- <span data-ttu-id="97a4c-108">Reservera vissa kapacitet på hello noder i klustret hello för hello tjänster inom vissa namngivna programinstansen</span><span class="sxs-lookup"><span data-stu-id="97a4c-108">Reserve some capacity on hello nodes in hello cluster for hello services within some named application instance</span></span>
- <span data-ttu-id="97a4c-109">Begränsa hello Totalt antal noder som kör hello services i en namngiven programinstansen på (i stället för att sprida ut dem över hello hela klustret)</span><span class="sxs-lookup"><span data-stu-id="97a4c-109">Limit hello total number of nodes that hello services within a named application instance run on (instead of spreading them out over hello entire cluster)</span></span>
- <span data-ttu-id="97a4c-110">Definiera kapacitet på hello namngivna programinstansen själva toolimit hello antal tjänster eller total förbrukning av nätverksresurser hello tjänster i den</span><span class="sxs-lookup"><span data-stu-id="97a4c-110">Define capacities on hello named application instance itself toolimit hello number of services or total resource consumption of hello services inside it</span></span>

<span data-ttu-id="97a4c-111">toomeet kraven hello resurshanteraren för Service Fabric-klustret har stöd för en funktion som kallas programgrupper.</span><span class="sxs-lookup"><span data-stu-id="97a4c-111">toomeet these requirements, hello Service Fabric Cluster Resource Manager supports a feature called Application Groups.</span></span>

## <a name="limiting-hello-maximum-number-of-nodes"></a><span data-ttu-id="97a4c-112">Begränsa hello maximalt antal noder</span><span class="sxs-lookup"><span data-stu-id="97a4c-112">Limiting hello maximum number of nodes</span></span>
<span data-ttu-id="97a4c-113">hello enklaste användningsfall för programmet kapacitet är när en instans av programmet måste toobe begränsad tooa vissa maximalt antal noder.</span><span class="sxs-lookup"><span data-stu-id="97a4c-113">hello simplest use case for Application capacity is when an application instance needs toobe limited tooa certain maximum number of nodes.</span></span> <span data-ttu-id="97a4c-114">Detta konsoliderar alla tjänster inom den programinstansen till ett visst antal datorer.</span><span class="sxs-lookup"><span data-stu-id="97a4c-114">This consolidates all services within that application instance onto a set number of machines.</span></span> <span data-ttu-id="97a4c-115">Konsolidering är användbart när du försöker toopredict eller cap fysiska Resursanvändning hello tjänster inom den namngivna programinstansen.</span><span class="sxs-lookup"><span data-stu-id="97a4c-115">Consolidation is useful when you're trying toopredict or cap physical resource use by hello services within that named application instance.</span></span> 

<span data-ttu-id="97a4c-116">hello visar följande bild en programinstans med och utan ett maximalt antal noder som definierats:</span><span class="sxs-lookup"><span data-stu-id="97a4c-116">hello following image shows an application instance with and without a maximum number of nodes defined:</span></span>

<span data-ttu-id="97a4c-117"><center>
![Definiera maximalt antal noder programinstansen][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="97a4c-117"><center>
![Application Instance Defining Maximum Number of Nodes][Image1]
</center></span></span>

<span data-ttu-id="97a4c-118">Hello program har inte ett maximalt antal noder som definierats i hello vänstra exempelvis och den har tre tjänster.</span><span class="sxs-lookup"><span data-stu-id="97a4c-118">In hello left example, hello application doesn’t have a maximum number of nodes defined, and it has three services.</span></span> <span data-ttu-id="97a4c-119">hello klustret Resource Manager har sprids ut alla repliker sex tillgängliga noder tooachieve hello bästa balansen i hello kluster (hello standardbeteendet).</span><span class="sxs-lookup"><span data-stu-id="97a4c-119">hello Cluster Resource Manager has spread out all replicas across six available nodes tooachieve hello best balance in hello cluster (hello default behavior).</span></span> <span data-ttu-id="97a4c-120">I högra hello-exempel finns vi hello samma program begränsad toothree noder.</span><span class="sxs-lookup"><span data-stu-id="97a4c-120">In hello right example, we see hello same application limited toothree nodes.</span></span>

<span data-ttu-id="97a4c-121">hello-parameter som anger det här beteendet kallas MaximumNodes.</span><span class="sxs-lookup"><span data-stu-id="97a4c-121">hello parameter that controls this behavior is called MaximumNodes.</span></span> <span data-ttu-id="97a4c-122">Denna parameter kan anges när programmet skulle skapas eller uppdateras för en programinstans som redan körs.</span><span class="sxs-lookup"><span data-stu-id="97a4c-122">This parameter can be set during application creation, or updated for an application instance that was already running.</span></span>

<span data-ttu-id="97a4c-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="97a4c-123">Powershell</span></span>

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

<span data-ttu-id="97a4c-124">C#</span><span class="sxs-lookup"><span data-stu-id="97a4c-124">C#</span></span>

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
await fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
await fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

```

<span data-ttu-id="97a4c-125">Inom hello uppsättning noder garanterar inte hello klustret Resource Manager vilka tjänstobjekt placeras tillsammans eller vilka noder hämta används.</span><span class="sxs-lookup"><span data-stu-id="97a4c-125">Within hello set of nodes, hello Cluster Resource Manager doesn't guarantee which service objects get placed together or which nodes get used.</span></span>

## <a name="application-metrics-load-and-capacity"></a><span data-ttu-id="97a4c-126">Programmet mått, belastning och kapacitet</span><span class="sxs-lookup"><span data-stu-id="97a4c-126">Application Metrics, Load, and Capacity</span></span>
<span data-ttu-id="97a4c-127">Programgrupper kan du också toodefine mätvärden som är associerade med en viss namngiven programinstansen och den programinstansen kapacitet för dessa mått.</span><span class="sxs-lookup"><span data-stu-id="97a4c-127">Application Groups also allow you toodefine metrics associated with a given named application instance, and that application instance's capacity for those metrics.</span></span> <span data-ttu-id="97a4c-128">Programmet mått kan du tootrack och reservera gränsen hello resursförbrukning hello tjänster i den programinstansen.</span><span class="sxs-lookup"><span data-stu-id="97a4c-128">Application metrics allow you tootrack, reserve, and limit hello resource consumption of hello services inside that application instance.</span></span>

<span data-ttu-id="97a4c-129">Det finns två värden som kan anges för varje mått:</span><span class="sxs-lookup"><span data-stu-id="97a4c-129">For each application metric, there are two values that can be set:</span></span>

- <span data-ttu-id="97a4c-130">**Total kapacitet för programmet** – den här inställningen motsvarar hello total kapacitet för hello program för ett viss mått.</span><span class="sxs-lookup"><span data-stu-id="97a4c-130">**Total Application Capacity** – This setting represents hello total capacity of hello application for a particular metric.</span></span> <span data-ttu-id="97a4c-131">hello klustret Resource Manager tillåts hello skapandet av nya tjänster i den här programinstansen som skulle orsaka totala tooexceed det här värdet.</span><span class="sxs-lookup"><span data-stu-id="97a4c-131">hello Cluster Resource Manager disallows hello creation of any new services within this application instance that would cause total load tooexceed this value.</span></span> <span data-ttu-id="97a4c-132">Anta att till exempel hello programinstansen hade en kapacitet på 10 och redan hade belastningen på fem.</span><span class="sxs-lookup"><span data-stu-id="97a4c-132">For example, let's say hello application instance had a capacity of 10 and already had load of five.</span></span> <span data-ttu-id="97a4c-133">hello skapandet av en tjänst med belastning totala standard 10 skulle inte tillåts.</span><span class="sxs-lookup"><span data-stu-id="97a4c-133">hello creation of a service with a total default load of 10 would be disallowed.</span></span>
- <span data-ttu-id="97a4c-134">**Maximal kapacitet för noden** – den här inställningen anger hello maximala totala belastningen för hello program på en enda nod.</span><span class="sxs-lookup"><span data-stu-id="97a4c-134">**Maximum Node Capacity** – This setting specifies hello maximum total load for hello application on a single node.</span></span> <span data-ttu-id="97a4c-135">Om belastningen över denna kapacitet, flyttar hello klustret Resource Manager repliker tooother noder så att hello belastningen minskar.</span><span class="sxs-lookup"><span data-stu-id="97a4c-135">If load goes over this capacity, hello Cluster Resource Manager moves replicas tooother nodes so that hello load decreases.</span></span>


<span data-ttu-id="97a4c-136">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="97a4c-136">Powershell:</span></span>

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -Metrics @("MetricName:Metric1,MaximumNodeCapacity:100,MaximumApplicationCapacity:1000")
```

<span data-ttu-id="97a4c-137">C#:</span><span class="sxs-lookup"><span data-stu-id="97a4c-137">C#:</span></span>

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;
appMetric.MaximumNodeCapacity = 100;
ad.Metrics.Add(appMetric);
await fc.ApplicationManager.CreateApplicationAsync(ad);
```

## <a name="reserving-capacity"></a><span data-ttu-id="97a4c-138">Reserverar kapacitet</span><span class="sxs-lookup"><span data-stu-id="97a4c-138">Reserving Capacity</span></span>
<span data-ttu-id="97a4c-139">Ett annat vanligt användningsområde för programgrupper är tooensure att resurser inom hello klustret är reserverade för ett visst program-instans.</span><span class="sxs-lookup"><span data-stu-id="97a4c-139">Another common use for application groups is tooensure that resources within hello cluster are reserved for a given application instance.</span></span> <span data-ttu-id="97a4c-140">hello utrymme reserveras alltid när hello programinstansen skapas.</span><span class="sxs-lookup"><span data-stu-id="97a4c-140">hello space is always reserved when hello application instance is created.</span></span>

<span data-ttu-id="97a4c-141">Reservera diskutrymme i hello kluster för programmet hello sker omedelbart även om:</span><span class="sxs-lookup"><span data-stu-id="97a4c-141">Reserving space in hello cluster for hello application happens immediately even when:</span></span>
- <span data-ttu-id="97a4c-142">hello programinstansen har skapats men har inte några tjänster inom det ännu</span><span class="sxs-lookup"><span data-stu-id="97a4c-142">hello application instance is created but doesn't have any services within it yet</span></span>
- <span data-ttu-id="97a4c-143">hello antal tjänster inom hello programinstansen ändras varje gång</span><span class="sxs-lookup"><span data-stu-id="97a4c-143">hello number of services within hello application instance changes every time</span></span> 
- <span data-ttu-id="97a4c-144">hello tjänster finns men förbrukar inte hello resurser</span><span class="sxs-lookup"><span data-stu-id="97a4c-144">hello services exist but aren't consuming hello resources</span></span> 

<span data-ttu-id="97a4c-145">Reservera resurser för en instans av programmet kräver att ange två ytterligare parametrar: *MinimumNodes* och *NodeReservationCapacity*</span><span class="sxs-lookup"><span data-stu-id="97a4c-145">Reserving resources for an application instance requires specifying two additional parameters: *MinimumNodes* and *NodeReservationCapacity*</span></span>

- <span data-ttu-id="97a4c-146">**MinimumNodes** -definierar hello minsta antalet noder som programmet hello instans ska köras på.</span><span class="sxs-lookup"><span data-stu-id="97a4c-146">**MinimumNodes** - Defines hello minimum number of nodes that hello application instance should run on.</span></span>  
- <span data-ttu-id="97a4c-147">**NodeReservationCapacity** -den här inställningen är per mått för hello program.</span><span class="sxs-lookup"><span data-stu-id="97a4c-147">**NodeReservationCapacity** - This setting is per metric for hello application.</span></span> <span data-ttu-id="97a4c-148">hello-värdet är hello som mått som reserverats för programmet hello på varje nod där som hello tjänster i programmet körs.</span><span class="sxs-lookup"><span data-stu-id="97a4c-148">hello value is hello amount of that metric reserved for hello application on any node where that hello services in that application run.</span></span>

<span data-ttu-id="97a4c-149">Kombinera **MinimumNodes** och **NodeReservationCapacity** garanterar en minsta belastningen reservation för hello program inom hello kluster.</span><span class="sxs-lookup"><span data-stu-id="97a4c-149">Combining **MinimumNodes** and **NodeReservationCapacity** guarantees a minimum load reservation for hello application within hello cluster.</span></span> <span data-ttu-id="97a4c-150">Om det finns mindre återstående kapacitet i hello kluster än hello totala reservationen krävs, kan inte skapas av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="97a4c-150">If there's less remaining capacity in hello cluster than hello total reservation required, creation of hello application fails.</span></span> 

<span data-ttu-id="97a4c-151">Nu ska vi titta på ett exempel på kapacitet reservation:</span><span class="sxs-lookup"><span data-stu-id="97a4c-151">Let's look at an example of capacity reservation:</span></span>

<span data-ttu-id="97a4c-152"><center>
![Programinstanser definiera reserverad kapacitet][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="97a4c-152"><center>
![Application Instances Defining Reserved Capacity][Image2]
</center></span></span>

<span data-ttu-id="97a4c-153">I hello vänstra exempelvis har program inte några program kapacitet har definierats.</span><span class="sxs-lookup"><span data-stu-id="97a4c-153">In hello left example, applications do not have any Application Capacity defined.</span></span> <span data-ttu-id="97a4c-154">hello klustret Resource Manager balanserar allt enligt toonormal regler.</span><span class="sxs-lookup"><span data-stu-id="97a4c-154">hello Cluster Resource Manager balances everything according toonormal rules.</span></span>

<span data-ttu-id="97a4c-155">I hello exempel på hello rätt anta att Application1 har skapats med hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="97a4c-155">In hello example on hello right, let's say that Application1 was created with hello following settings:</span></span>

- <span data-ttu-id="97a4c-156">MinimumNodes ange tootwo</span><span class="sxs-lookup"><span data-stu-id="97a4c-156">MinimumNodes set tootwo</span></span>
- <span data-ttu-id="97a4c-157">Ett program mått som definieras med</span><span class="sxs-lookup"><span data-stu-id="97a4c-157">An application Metric defined with</span></span>
  - <span data-ttu-id="97a4c-158">NodeReservationCapacity 20</span><span class="sxs-lookup"><span data-stu-id="97a4c-158">NodeReservationCapacity of 20</span></span>

<span data-ttu-id="97a4c-159">PowerShell</span><span class="sxs-lookup"><span data-stu-id="97a4c-159">Powershell</span></span>

 ``` posh
 New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MinimumNodes 2 -Metrics @("MetricName:Metric1,NodeReservationCapacity:20")
 ```

<span data-ttu-id="97a4c-160">C#</span><span class="sxs-lookup"><span data-stu-id="97a4c-160">C#</span></span>

 ``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MinimumNodes = 2;

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.NodeReservationCapacity = 20;

ad.Metrics.Add(appMetric);

await fc.ApplicationManager.CreateApplicationAsync(ad);
```

<span data-ttu-id="97a4c-161">Service Fabric reserverar kapacitet på två noder för Application1 och tillåter inte tjänster från Application2 tooconsume den kapaciteten även om det finns ingen belastning som förbrukas av hello tjänster i Application1 hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="97a4c-161">Service Fabric reserves capacity on two nodes for Application1, and doesn't allow services from Application2 tooconsume that capacity even if there are no load is being consumed by hello services inside Application1 at hello time.</span></span> <span data-ttu-id="97a4c-162">Denna kapacitet för reserverade programmet betraktas som förbrukats och räknas av mot hello återstående kapacitet på noden och inom hello kluster.</span><span class="sxs-lookup"><span data-stu-id="97a4c-162">This reserved application capacity is considered consumed  and counts against hello remaining capacity on that node and within hello cluster.</span></span>  <span data-ttu-id="97a4c-163">hello reservation dras av från hello återstående klustret kapacitet omedelbart, men hello reserverade förbrukning dras av från hello kapacitet för en viss nod endast när minst en tjänstobjekt placeras i den.</span><span class="sxs-lookup"><span data-stu-id="97a4c-163">hello reservation is deducted from hello remaining cluster capacity immediately, however hello reserved consumption is deducted from hello capacity of a specific node only when at least one service object is placed on it.</span></span> <span data-ttu-id="97a4c-164">Denna senare reservation ger flexibilitet och bättre resursutnyttjande eftersom resurser som endast är reserverad på noder vid behov.</span><span class="sxs-lookup"><span data-stu-id="97a4c-164">This later reservation allows for flexibility and better resource utilization since resources are only reserved on nodes when needed.</span></span>

## <a name="obtaining-hello-application-load-information"></a><span data-ttu-id="97a4c-165">Hämta hello belastningen programinformation</span><span class="sxs-lookup"><span data-stu-id="97a4c-165">Obtaining hello application load information</span></span>
<span data-ttu-id="97a4c-166">Du kan hämta hello information om hello sammanställd belastning som rapporterats av repliker av dess tjänster för varje program som har ett program kapacitet har definierats för ett eller flera mått.</span><span class="sxs-lookup"><span data-stu-id="97a4c-166">For each application that has an Application Capacity defined for one or more metrics you can obtain hello information about hello aggregate load reported by replicas of its services.</span></span>

<span data-ttu-id="97a4c-167">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="97a4c-167">Powershell:</span></span>

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1
```

<span data-ttu-id="97a4c-168">C#</span><span class="sxs-lookup"><span data-stu-id="97a4c-168">C#</span></span>

``` csharp
var v = await fc.QueryManager.GetApplicationLoadInformationAsync("fabric:/MyApplication1");
var metrics = v.ApplicationLoadMetricInformation;
foreach (ApplicationLoadMetricInformation metric in metrics)
{
    Console.WriteLine(metric.ApplicationCapacity);  //total capacity for this metric in this application instance
    Console.WriteLine(metric.ReservationCapacity);  //reserved capacity for this metric in this application instance
    Console.WriteLine(metric.ApplicationLoad);  //current load for this metric in this application instance
}
```

<span data-ttu-id="97a4c-169">Hej ApplicationLoad frågan returnerar hello grundläggande information om kapacitet för program som har angetts för programmet hello.</span><span class="sxs-lookup"><span data-stu-id="97a4c-169">hello ApplicationLoad query returns hello basic information about Application Capacity that was specified for hello application.</span></span> <span data-ttu-id="97a4c-170">Informationen omfattar information om lägsta noder och maximalt antal noder av hello och hello nummer som hello programmet för närvarande använder.</span><span class="sxs-lookup"><span data-stu-id="97a4c-170">This information includes hello Minimum Nodes and Maximum Nodes info, and hello number that hello application is currently occupying.</span></span> <span data-ttu-id="97a4c-171">Det innehåller även information om varje belastningen mått, inklusive:</span><span class="sxs-lookup"><span data-stu-id="97a4c-171">It also includes information about each application load metric, including:</span></span>

* <span data-ttu-id="97a4c-172">Måttnamnet: Namnet på hello mått.</span><span class="sxs-lookup"><span data-stu-id="97a4c-172">Metric Name: Name of hello metric.</span></span>
* <span data-ttu-id="97a4c-173">Reservation Capacity: Kapacitet för kluster som är reserverade i hello kluster för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="97a4c-173">Reservation Capacity: Cluster Capacity that is reserved in hello cluster for this Application.</span></span>
* <span data-ttu-id="97a4c-174">Programinläsning: Totalt antal belastningen på det här programmet underordnade repliker.</span><span class="sxs-lookup"><span data-stu-id="97a4c-174">Application Load: Total Load of this Application’s child replicas.</span></span>
* <span data-ttu-id="97a4c-175">Kapacitet för programmet: Högsta tillåtna värdet för programinläsning.</span><span class="sxs-lookup"><span data-stu-id="97a4c-175">Application Capacity: Maximum permitted value of Application Load.</span></span>

## <a name="removing-application-capacity"></a><span data-ttu-id="97a4c-176">Kapacitet för att ta bort program</span><span class="sxs-lookup"><span data-stu-id="97a4c-176">Removing Application Capacity</span></span>
<span data-ttu-id="97a4c-177">Efter hello programmet kapacitet parametrar har angetts för ett program, kan de tas bort med hjälp av API: er för Update-programmet eller PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="97a4c-177">Once hello Application Capacity parameters are set for an application, they can be removed using Update Application APIs or PowerShell cmdlets.</span></span> <span data-ttu-id="97a4c-178">Exempel:</span><span class="sxs-lookup"><span data-stu-id="97a4c-178">For example:</span></span>

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

<span data-ttu-id="97a4c-179">Det här kommandot tar bort alla parametrar för program-kapacitet från hello programinstansen.</span><span class="sxs-lookup"><span data-stu-id="97a4c-179">This command removes all Application capacity management parameters from hello application instance.</span></span> <span data-ttu-id="97a4c-180">Detta inkluderar MinimumNodes, MaximumNodes och hello programmet statistik, och eventuella.</span><span class="sxs-lookup"><span data-stu-id="97a4c-180">This includes MinimumNodes, MaximumNodes, and hello Application's metrics, if any.</span></span> <span data-ttu-id="97a4c-181">hello effekten av hello kommandot sker omedelbart.</span><span class="sxs-lookup"><span data-stu-id="97a4c-181">hello effect of hello command is immediate.</span></span> <span data-ttu-id="97a4c-182">När det här kommandot har slutförts använder hello klustret Resource Manager hello standardbeteendet för att hantera program.</span><span class="sxs-lookup"><span data-stu-id="97a4c-182">After this command completes, hello Cluster Resource Manager uses hello default behavior for managing applications.</span></span> <span data-ttu-id="97a4c-183">Programmet kapacitet parametrar kan anges igen `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.</span><span class="sxs-lookup"><span data-stu-id="97a4c-183">Application Capacity parameters can be specified again via `Update-ServiceFabricApplication`/`System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.</span></span>

### <a name="restrictions-on-application-capacity"></a><span data-ttu-id="97a4c-184">Begränsningar för programmet kapacitet</span><span class="sxs-lookup"><span data-stu-id="97a4c-184">Restrictions on Application Capacity</span></span>
<span data-ttu-id="97a4c-185">Det finns flera begränsningar för programmet kapacitet parametrar som måste följas.</span><span class="sxs-lookup"><span data-stu-id="97a4c-185">There are several restrictions on Application Capacity parameters that must be respected.</span></span> <span data-ttu-id="97a4c-186">Om det finns valideringsfel görs inga ändringar.</span><span class="sxs-lookup"><span data-stu-id="97a4c-186">If there are validation errors no changes take place.</span></span>

- <span data-ttu-id="97a4c-187">Alla måste heltal vara icke-negativt tal.</span><span class="sxs-lookup"><span data-stu-id="97a4c-187">All integer parameters must be non-negative numbers.</span></span>
- <span data-ttu-id="97a4c-188">MinimumNodes måste aldrig vara större än MaximumNodes.</span><span class="sxs-lookup"><span data-stu-id="97a4c-188">MinimumNodes must never be greater than MaximumNodes.</span></span>
- <span data-ttu-id="97a4c-189">Om kapaciteten för ett mått för belastningen definieras måste de följa reglerna:</span><span class="sxs-lookup"><span data-stu-id="97a4c-189">If capacities for a load metric are defined, then they must follow these rules:</span></span>
  - <span data-ttu-id="97a4c-190">Noden Reservation Capacity får inte vara större än den maximala nod kapaciteten.</span><span class="sxs-lookup"><span data-stu-id="97a4c-190">Node Reservation Capacity must not be greater than Maximum Node Capacity.</span></span> <span data-ttu-id="97a4c-191">Du kan till exempel begränsa hello kapacitet för hello mått ”CPU” på hello nod tootwo enheter och försök tooreserve tre enheter på varje nod.</span><span class="sxs-lookup"><span data-stu-id="97a4c-191">For example, you cannot limit hello capacity for hello metric “CPU” on hello node tootwo units and try tooreserve three units on each node.</span></span>
  - <span data-ttu-id="97a4c-192">Om MaximumNodes anges får sedan hello produkten av MaximumNodes och maximumkapacitet för noden inte vara större än den Totalkapaciteten för programmet.</span><span class="sxs-lookup"><span data-stu-id="97a4c-192">If MaximumNodes is specified, then hello product of MaximumNodes and Maximum Node Capacity must not be greater than Total Application Capacity.</span></span> <span data-ttu-id="97a4c-193">Anta till exempel hello maxkapacitet nod för belastningen mått ”CPU” anges tooeight.</span><span class="sxs-lookup"><span data-stu-id="97a4c-193">For example, let's say hello Maximum Node Capacity for load metric “CPU” is set tooeight.</span></span> <span data-ttu-id="97a4c-194">Anta också att du ställer in hello maximalt antal noder too10.</span><span class="sxs-lookup"><span data-stu-id="97a4c-194">Let's also say you set hello Maximum Nodes too10.</span></span> <span data-ttu-id="97a4c-195">I det här fallet måste totalkapacitet för programmet vara större än 80 för mätvärdet belastningen.</span><span class="sxs-lookup"><span data-stu-id="97a4c-195">In this case, Total Application Capacity must be greater than 80 for this load metric.</span></span>

<span data-ttu-id="97a4c-196">både under skapa program och uppdateringar tillämpas hello begränsningar.</span><span class="sxs-lookup"><span data-stu-id="97a4c-196">hello restrictions are enforced both during application creation and updates.</span></span>

## <a name="how-not-toouse-application-capacity"></a><span data-ttu-id="97a4c-197">Hur inte toouse programmet kapacitet</span><span class="sxs-lookup"><span data-stu-id="97a4c-197">How not toouse Application Capacity</span></span>
- <span data-ttu-id="97a4c-198">Försök inte toouse hello programgruppen funktioner tooconstrain hello programmet tooa _specifika_ del noder.</span><span class="sxs-lookup"><span data-stu-id="97a4c-198">Do not try toouse hello Application Group features tooconstrain hello application tooa _specific_ subset of nodes.</span></span> <span data-ttu-id="97a4c-199">Med andra ord kan du ange att programmet hello körs på högst fem noder, men inte vilka specifika fem noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="97a4c-199">In other words, you can specify that hello application runs on at most five nodes, but not which specific five nodes in hello cluster.</span></span> <span data-ttu-id="97a4c-200">Begränsa ett program toospecific kan noder ske med hjälp av placeringsbegränsningar för tjänster.</span><span class="sxs-lookup"><span data-stu-id="97a4c-200">Constraining an application toospecific nodes can be achieved using placement constraints for services.</span></span>
- <span data-ttu-id="97a4c-201">Försök inte toouse hello programmet kapacitet tooensure att två tjänster från samma program är placerad hello hello samma noder.</span><span class="sxs-lookup"><span data-stu-id="97a4c-201">Do not try toouse hello Application Capacity tooensure that two services from hello same application are placed on hello same nodes.</span></span> <span data-ttu-id="97a4c-202">I stället använda [tillhörighet](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) eller [placeringen](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).</span><span class="sxs-lookup"><span data-stu-id="97a4c-202">Instead use [affinity](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) or [placement constraints](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).</span></span>

## <a name="next-steps"></a><span data-ttu-id="97a4c-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="97a4c-203">Next steps</span></span>
- <span data-ttu-id="97a4c-204">Mer information om hur du konfigurerar tjänster, [Lär dig mer om hur du konfigurerar tjänster](service-fabric-cluster-resource-manager-configure-services.md)</span><span class="sxs-lookup"><span data-stu-id="97a4c-204">For more information on configuring services, [Learn about configuring Services](service-fabric-cluster-resource-manager-configure-services.md)</span></span>
- <span data-ttu-id="97a4c-205">toofind ut om hur hello klustret Resource Manager hanterar och balanserar belastningen i hello kluster kolla hello artikel på [belastningsutjämning](service-fabric-cluster-resource-manager-balancing.md)</span><span class="sxs-lookup"><span data-stu-id="97a4c-205">toofind out about how hello Cluster Resource Manager manages and balances load in hello cluster, check out hello article on [balancing load](service-fabric-cluster-resource-manager-balancing.md)</span></span>
- <span data-ttu-id="97a4c-206">Starta från början hello och [få en introduktion toohello resurshanteraren för Service Fabric-kluster](service-fabric-cluster-resource-manager-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="97a4c-206">Start from hello beginning and [get an Introduction toohello Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)</span></span>
- <span data-ttu-id="97a4c-207">Mer information om hur mått fungerar normalt läsa på [Service Fabric belastningen mått](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="97a4c-207">For more information on how metrics work generally, read up on [Service Fabric Load Metrics](service-fabric-cluster-resource-manager-metrics.md)</span></span>
- <span data-ttu-id="97a4c-208">hello klustret Resource Manager har många alternativ för att beskriva hello klustret.</span><span class="sxs-lookup"><span data-stu-id="97a4c-208">hello Cluster Resource Manager has many options for describing hello cluster.</span></span> <span data-ttu-id="97a4c-209">toofind mer information om dem, Kolla in den här artikeln på [som beskriver ett Service Fabric-kluster](service-fabric-cluster-resource-manager-cluster-description.md)</span><span class="sxs-lookup"><span data-stu-id="97a4c-209">toofind out more about them, check out this article on [describing a Service Fabric cluster](service-fabric-cluster-resource-manager-cluster-description.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
