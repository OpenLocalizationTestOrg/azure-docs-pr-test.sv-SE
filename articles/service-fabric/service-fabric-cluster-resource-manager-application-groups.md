---
title: Service Fabric klustret Resource Manager - programgrupper | Microsoft Docs
description: "Översikt över funktionen för programgruppen i Service Fabric klustret Resource Manager"
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
ms.openlocfilehash: 7fc61731c8df2a0c3dc5b77ae718b41c240f9233
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="introduction-to-application-groups"></a><span data-ttu-id="6cea9-103">Introduktion till programgrupper</span><span class="sxs-lookup"><span data-stu-id="6cea9-103">Introduction to Application Groups</span></span>
<span data-ttu-id="6cea9-104">Resurshanteraren för Service Fabric-klustret hanterar vanligtvis klusterresurser genom att sprida belastningen (representeras [mått](service-fabric-cluster-resource-manager-metrics.md)) jämnt i hela klustret.</span><span class="sxs-lookup"><span data-stu-id="6cea9-104">Service Fabric's Cluster Resource Manager typically manages cluster resources by spreading the load (represented via [Metrics](service-fabric-cluster-resource-manager-metrics.md)) evenly throughout the cluster.</span></span> <span data-ttu-id="6cea9-105">Service Fabric hanterar kapacitet av noderna i klustret och klustret som helhet via [kapacitet](service-fabric-cluster-resource-manager-cluster-description.md).</span><span class="sxs-lookup"><span data-stu-id="6cea9-105">Service Fabric manages the capacity of the nodes in the cluster and the cluster as a whole via [capacity](service-fabric-cluster-resource-manager-cluster-description.md).</span></span> <span data-ttu-id="6cea9-106">Mått och kapacitet fungerar bra för många arbetsbelastningar, men mönster som använder olika Service Fabric-programinstanser ibland hämtas ytterligare krav.</span><span class="sxs-lookup"><span data-stu-id="6cea9-106">Metrics and capacity work great for many workloads, but patterns that make heavy use of different Service Fabric Application Instances sometimes bring in additional requirements.</span></span> <span data-ttu-id="6cea9-107">Till exempel kanske du vill:</span><span class="sxs-lookup"><span data-stu-id="6cea9-107">For example you may want to:</span></span>

- <span data-ttu-id="6cea9-108">Reservera vissa kapacitet på noderna i klustret för tjänster i vissa namngivna programinstansen</span><span class="sxs-lookup"><span data-stu-id="6cea9-108">Reserve some capacity on the nodes in the cluster for the services within some named application instance</span></span>
- <span data-ttu-id="6cea9-109">Begränsa det totala antalet noder som kör services i en namngiven programinstansen på (i stället för att sprida ut dem över hela klustret)</span><span class="sxs-lookup"><span data-stu-id="6cea9-109">Limit the total number of nodes that the services within a named application instance run on (instead of spreading them out over the entire cluster)</span></span>
- <span data-ttu-id="6cea9-110">Definiera kapacitet på namngiven programinstansen fristående för att begränsa antalet tjänster eller totalt antal resurser som används för tjänster i den</span><span class="sxs-lookup"><span data-stu-id="6cea9-110">Define capacities on the named application instance itself to limit the number of services or total resource consumption of the services inside it</span></span>

<span data-ttu-id="6cea9-111">För att uppfylla kraven, stöder Service Fabric klustret Resource Manager en funktion som kallas programgrupper.</span><span class="sxs-lookup"><span data-stu-id="6cea9-111">To meet these requirements, the Service Fabric Cluster Resource Manager supports a feature called Application Groups.</span></span>

## <a name="limiting-the-maximum-number-of-nodes"></a><span data-ttu-id="6cea9-112">Begränsar det maximala antalet noder</span><span class="sxs-lookup"><span data-stu-id="6cea9-112">Limiting the maximum number of nodes</span></span>
<span data-ttu-id="6cea9-113">När en instans av programmet ska begränsas till ett maximalt antal noder är det enklaste användningsfallet för programmet kapacitet.</span><span class="sxs-lookup"><span data-stu-id="6cea9-113">The simplest use case for Application capacity is when an application instance needs to be limited to a certain maximum number of nodes.</span></span> <span data-ttu-id="6cea9-114">Detta konsoliderar alla tjänster inom den programinstansen till ett visst antal datorer.</span><span class="sxs-lookup"><span data-stu-id="6cea9-114">This consolidates all services within that application instance onto a set number of machines.</span></span> <span data-ttu-id="6cea9-115">Konsolidering är användbart när du försöker att förutsäga eller cap fysisk Resursanvändning av tjänsterna i den namngivna programinstansen.</span><span class="sxs-lookup"><span data-stu-id="6cea9-115">Consolidation is useful when you're trying to predict or cap physical resource use by the services within that named application instance.</span></span> 

<span data-ttu-id="6cea9-116">Följande bild visar en programinstans med och utan ett maximalt antal noder som definierats:</span><span class="sxs-lookup"><span data-stu-id="6cea9-116">The following image shows an application instance with and without a maximum number of nodes defined:</span></span>

<span data-ttu-id="6cea9-117"><center>
![Definiera maximalt antal noder programinstansen][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="6cea9-117"><center>
![Application Instance Defining Maximum Number of Nodes][Image1]
</center></span></span>

<span data-ttu-id="6cea9-118">Programmet har inte ett maximalt antal noder som definierats i exemplet vänstra och den har tre tjänster.</span><span class="sxs-lookup"><span data-stu-id="6cea9-118">In the left example, the application doesn’t have a maximum number of nodes defined, and it has three services.</span></span> <span data-ttu-id="6cea9-119">Klustret Resource Manager har sprids ut alla repliker sex tillgängliga noder för att uppnå bästa balans i klustret (standardinställning).</span><span class="sxs-lookup"><span data-stu-id="6cea9-119">The Cluster Resource Manager has spread out all replicas across six available nodes to achieve the best balance in the cluster (the default behavior).</span></span> <span data-ttu-id="6cea9-120">I högra exempel visas samma program begränsad till tre noder.</span><span class="sxs-lookup"><span data-stu-id="6cea9-120">In the right example, we see the same application limited to three nodes.</span></span>

<span data-ttu-id="6cea9-121">Parametern som styr det här beteendet kallas MaximumNodes.</span><span class="sxs-lookup"><span data-stu-id="6cea9-121">The parameter that controls this behavior is called MaximumNodes.</span></span> <span data-ttu-id="6cea9-122">Denna parameter kan anges när programmet skulle skapas eller uppdateras för en programinstans som redan körs.</span><span class="sxs-lookup"><span data-stu-id="6cea9-122">This parameter can be set during application creation, or updated for an application instance that was already running.</span></span>

<span data-ttu-id="6cea9-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6cea9-123">Powershell</span></span>

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

<span data-ttu-id="6cea9-124">C#</span><span class="sxs-lookup"><span data-stu-id="6cea9-124">C#</span></span>

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

<span data-ttu-id="6cea9-125">I uppsättningen noder garanterar inte klustret Resource Manager vilka tjänstobjekt placeras tillsammans eller vilka noder hämta används.</span><span class="sxs-lookup"><span data-stu-id="6cea9-125">Within the set of nodes, the Cluster Resource Manager doesn't guarantee which service objects get placed together or which nodes get used.</span></span>

## <a name="application-metrics-load-and-capacity"></a><span data-ttu-id="6cea9-126">Programmet mått, belastning och kapacitet</span><span class="sxs-lookup"><span data-stu-id="6cea9-126">Application Metrics, Load, and Capacity</span></span>
<span data-ttu-id="6cea9-127">Programgrupper kan du definiera mätvärden som är associerade med en viss namngiven programinstansen och den programinstansen kapacitet för dessa mått.</span><span class="sxs-lookup"><span data-stu-id="6cea9-127">Application Groups also allow you to define metrics associated with a given named application instance, and that application instance's capacity for those metrics.</span></span> <span data-ttu-id="6cea9-128">Programmet mått kan du spåra reservera och begränsa resursförbrukning tjänster i den programinstansen.</span><span class="sxs-lookup"><span data-stu-id="6cea9-128">Application metrics allow you to track, reserve, and limit the resource consumption of the services inside that application instance.</span></span>

<span data-ttu-id="6cea9-129">Det finns två värden som kan anges för varje mått:</span><span class="sxs-lookup"><span data-stu-id="6cea9-129">For each application metric, there are two values that can be set:</span></span>

- <span data-ttu-id="6cea9-130">**Total kapacitet för programmet** – den här inställningen som representerar den totala kapaciteten för programmet för ett viss mått.</span><span class="sxs-lookup"><span data-stu-id="6cea9-130">**Total Application Capacity** – This setting represents the total capacity of the application for a particular metric.</span></span> <span data-ttu-id="6cea9-131">Klustret Resource Manager tillåter inte skapandet av nya tjänster i den här programinstansen som skulle orsaka totala belastningen överskrider värdet.</span><span class="sxs-lookup"><span data-stu-id="6cea9-131">The Cluster Resource Manager disallows the creation of any new services within this application instance that would cause total load to exceed this value.</span></span> <span data-ttu-id="6cea9-132">Anta att till exempel programinstansen hade en kapacitet på 10 och redan hade belastningen på fem.</span><span class="sxs-lookup"><span data-stu-id="6cea9-132">For example, let's say the application instance had a capacity of 10 and already had load of five.</span></span> <span data-ttu-id="6cea9-133">Skapandet av en tjänst med belastning totala standard 10 skulle inte tillåts.</span><span class="sxs-lookup"><span data-stu-id="6cea9-133">The creation of a service with a total default load of 10 would be disallowed.</span></span>
- <span data-ttu-id="6cea9-134">**Maximal kapacitet för noden** – den här inställningen anger den maximala totala belastningen för programmet på en enda nod.</span><span class="sxs-lookup"><span data-stu-id="6cea9-134">**Maximum Node Capacity** – This setting specifies the maximum total load for the application on a single node.</span></span> <span data-ttu-id="6cea9-135">Om belastningen över denna kapacitet, flyttar klustret Resource Manager repliker till andra noder så att belastningen minskar.</span><span class="sxs-lookup"><span data-stu-id="6cea9-135">If load goes over this capacity, the Cluster Resource Manager moves replicas to other nodes so that the load decreases.</span></span>


<span data-ttu-id="6cea9-136">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6cea9-136">Powershell:</span></span>

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -Metrics @("MetricName:Metric1,MaximumNodeCapacity:100,MaximumApplicationCapacity:1000")
```

<span data-ttu-id="6cea9-137">C#:</span><span class="sxs-lookup"><span data-stu-id="6cea9-137">C#:</span></span>

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

## <a name="reserving-capacity"></a><span data-ttu-id="6cea9-138">Reserverar kapacitet</span><span class="sxs-lookup"><span data-stu-id="6cea9-138">Reserving Capacity</span></span>
<span data-ttu-id="6cea9-139">Ett annat vanligt användningsområde för programgrupper är att säkerställa att resurser i klustret är reserverade för ett visst program-instans.</span><span class="sxs-lookup"><span data-stu-id="6cea9-139">Another common use for application groups is to ensure that resources within the cluster are reserved for a given application instance.</span></span> <span data-ttu-id="6cea9-140">Utrymmet reserveras alltid när programinstansen skapas.</span><span class="sxs-lookup"><span data-stu-id="6cea9-140">The space is always reserved when the application instance is created.</span></span>

<span data-ttu-id="6cea9-141">Reservera diskutrymme i klustret för programmet sker omedelbart även om:</span><span class="sxs-lookup"><span data-stu-id="6cea9-141">Reserving space in the cluster for the application happens immediately even when:</span></span>
- <span data-ttu-id="6cea9-142">programinstansen har skapats men har inte några tjänster inom det ännu</span><span class="sxs-lookup"><span data-stu-id="6cea9-142">the application instance is created but doesn't have any services within it yet</span></span>
- <span data-ttu-id="6cea9-143">antal tjänster inom programinstansen ändras varje gång</span><span class="sxs-lookup"><span data-stu-id="6cea9-143">the number of services within the application instance changes every time</span></span> 
- <span data-ttu-id="6cea9-144">tjänsterna finns men förbrukar inte resurser</span><span class="sxs-lookup"><span data-stu-id="6cea9-144">the services exist but aren't consuming the resources</span></span> 

<span data-ttu-id="6cea9-145">Reservera resurser för en instans av programmet kräver att ange två ytterligare parametrar: *MinimumNodes* och *NodeReservationCapacity*</span><span class="sxs-lookup"><span data-stu-id="6cea9-145">Reserving resources for an application instance requires specifying two additional parameters: *MinimumNodes* and *NodeReservationCapacity*</span></span>

- <span data-ttu-id="6cea9-146">**MinimumNodes** -definierar det minsta antalet noder som programinstansen ska köras på.</span><span class="sxs-lookup"><span data-stu-id="6cea9-146">**MinimumNodes** - Defines the minimum number of nodes that the application instance should run on.</span></span>  
- <span data-ttu-id="6cea9-147">**NodeReservationCapacity** -den här inställningen är per mått för programmet.</span><span class="sxs-lookup"><span data-stu-id="6cea9-147">**NodeReservationCapacity** - This setting is per metric for the application.</span></span> <span data-ttu-id="6cea9-148">Värdet är mängden det mått som reserverats för programmet på någon nod där som tjänsterna i programmet körs.</span><span class="sxs-lookup"><span data-stu-id="6cea9-148">The value is the amount of that metric reserved for the application on any node where that the services in that application run.</span></span>

<span data-ttu-id="6cea9-149">Kombinera **MinimumNodes** och **NodeReservationCapacity** garanterar en minsta belastningen reservation för program i klustret.</span><span class="sxs-lookup"><span data-stu-id="6cea9-149">Combining **MinimumNodes** and **NodeReservationCapacity** guarantees a minimum load reservation for the application within the cluster.</span></span> <span data-ttu-id="6cea9-150">Om det har mindre återstående kapacitet i klustret än den totala reservationen krävs, inte skapandet av programmet.</span><span class="sxs-lookup"><span data-stu-id="6cea9-150">If there's less remaining capacity in the cluster than the total reservation required, creation of the application fails.</span></span> 

<span data-ttu-id="6cea9-151">Nu ska vi titta på ett exempel på kapacitet reservation:</span><span class="sxs-lookup"><span data-stu-id="6cea9-151">Let's look at an example of capacity reservation:</span></span>

<span data-ttu-id="6cea9-152"><center>
![Programinstanser definiera reserverad kapacitet][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="6cea9-152"><center>
![Application Instances Defining Reserved Capacity][Image2]
</center></span></span>

<span data-ttu-id="6cea9-153">I den vänstra exempelvis har program inte några program kapacitet har definierats.</span><span class="sxs-lookup"><span data-stu-id="6cea9-153">In the left example, applications do not have any Application Capacity defined.</span></span> <span data-ttu-id="6cea9-154">Klustret Resource Manager balanserar allt enligt normala regler.</span><span class="sxs-lookup"><span data-stu-id="6cea9-154">The Cluster Resource Manager balances everything according to normal rules.</span></span>

<span data-ttu-id="6cea9-155">I exemplet på höger anta att Application1 har skapats med följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="6cea9-155">In the example on the right, let's say that Application1 was created with the following settings:</span></span>

- <span data-ttu-id="6cea9-156">MinimumNodes inställd på två</span><span class="sxs-lookup"><span data-stu-id="6cea9-156">MinimumNodes set to two</span></span>
- <span data-ttu-id="6cea9-157">Ett program mått som definieras med</span><span class="sxs-lookup"><span data-stu-id="6cea9-157">An application Metric defined with</span></span>
  - <span data-ttu-id="6cea9-158">NodeReservationCapacity 20</span><span class="sxs-lookup"><span data-stu-id="6cea9-158">NodeReservationCapacity of 20</span></span>

<span data-ttu-id="6cea9-159">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6cea9-159">Powershell</span></span>

 ``` posh
 New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MinimumNodes 2 -Metrics @("MetricName:Metric1,NodeReservationCapacity:20")
 ```

<span data-ttu-id="6cea9-160">C#</span><span class="sxs-lookup"><span data-stu-id="6cea9-160">C#</span></span>

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

<span data-ttu-id="6cea9-161">Service Fabric reserverar kapacitet på två noder för Application1 och tillåter inte tjänster från Application2 att använda den kapaciteten även om det finns ingen belastning som förbrukas av tjänsterna i Application1 samtidigt.</span><span class="sxs-lookup"><span data-stu-id="6cea9-161">Service Fabric reserves capacity on two nodes for Application1, and doesn't allow services from Application2 to consume that capacity even if there are no load is being consumed by the services inside Application1 at the time.</span></span> <span data-ttu-id="6cea9-162">Denna kapacitet för reserverade programmet betraktas som förbrukats och räknas av mot de återstående kapaciteten på noden och inom klustret.</span><span class="sxs-lookup"><span data-stu-id="6cea9-162">This reserved application capacity is considered consumed  and counts against the remaining capacity on that node and within the cluster.</span></span>  <span data-ttu-id="6cea9-163">Reservationen dras från återstående klustret kapacitet omedelbart, men reserverade förbrukningen dras av från kapaciteten för en viss nod endast när minst en tjänstobjekt placeras i den.</span><span class="sxs-lookup"><span data-stu-id="6cea9-163">The reservation is deducted from the remaining cluster capacity immediately, however the reserved consumption is deducted from the capacity of a specific node only when at least one service object is placed on it.</span></span> <span data-ttu-id="6cea9-164">Denna senare reservation ger flexibilitet och bättre resursutnyttjande eftersom resurser som endast är reserverad på noder vid behov.</span><span class="sxs-lookup"><span data-stu-id="6cea9-164">This later reservation allows for flexibility and better resource utilization since resources are only reserved on nodes when needed.</span></span>

## <a name="obtaining-the-application-load-information"></a><span data-ttu-id="6cea9-165">Hämta belastningen programinformation</span><span class="sxs-lookup"><span data-stu-id="6cea9-165">Obtaining the application load information</span></span>
<span data-ttu-id="6cea9-166">För varje program som har ett program kapacitet har definierats för ett eller flera mått som du kan hämta information om sammanställd belastning som rapporterats av repliker av dess tjänster.</span><span class="sxs-lookup"><span data-stu-id="6cea9-166">For each application that has an Application Capacity defined for one or more metrics you can obtain the information about the aggregate load reported by replicas of its services.</span></span>

<span data-ttu-id="6cea9-167">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6cea9-167">Powershell:</span></span>

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1
```

<span data-ttu-id="6cea9-168">C#</span><span class="sxs-lookup"><span data-stu-id="6cea9-168">C#</span></span>

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

<span data-ttu-id="6cea9-169">ApplicationLoad frågan returnerar grundläggande information om kapacitet för program som har angetts för programmet.</span><span class="sxs-lookup"><span data-stu-id="6cea9-169">The ApplicationLoad query returns the basic information about Application Capacity that was specified for the application.</span></span> <span data-ttu-id="6cea9-170">Informationen omfattar information om lägsta noder och maximalt antal noder och hur många som programmet för närvarande använder.</span><span class="sxs-lookup"><span data-stu-id="6cea9-170">This information includes the Minimum Nodes and Maximum Nodes info, and the number that the application is currently occupying.</span></span> <span data-ttu-id="6cea9-171">Det innehåller även information om varje belastningen mått, inklusive:</span><span class="sxs-lookup"><span data-stu-id="6cea9-171">It also includes information about each application load metric, including:</span></span>

* <span data-ttu-id="6cea9-172">Måttnamnet: Namnet på måttet.</span><span class="sxs-lookup"><span data-stu-id="6cea9-172">Metric Name: Name of the metric.</span></span>
* <span data-ttu-id="6cea9-173">Reservation Capacity: Kapacitet för kluster som är reserverade i klustret för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="6cea9-173">Reservation Capacity: Cluster Capacity that is reserved in the cluster for this Application.</span></span>
* <span data-ttu-id="6cea9-174">Programinläsning: Totalt antal belastningen på det här programmet underordnade repliker.</span><span class="sxs-lookup"><span data-stu-id="6cea9-174">Application Load: Total Load of this Application’s child replicas.</span></span>
* <span data-ttu-id="6cea9-175">Kapacitet för programmet: Högsta tillåtna värdet för programinläsning.</span><span class="sxs-lookup"><span data-stu-id="6cea9-175">Application Capacity: Maximum permitted value of Application Load.</span></span>

## <a name="removing-application-capacity"></a><span data-ttu-id="6cea9-176">Kapacitet för att ta bort program</span><span class="sxs-lookup"><span data-stu-id="6cea9-176">Removing Application Capacity</span></span>
<span data-ttu-id="6cea9-177">Efter att programmet kapacitet parametrar har angetts för ett program, kan de tas bort med hjälp av API: er för Update-programmet eller PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="6cea9-177">Once the Application Capacity parameters are set for an application, they can be removed using Update Application APIs or PowerShell cmdlets.</span></span> <span data-ttu-id="6cea9-178">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6cea9-178">For example:</span></span>

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

<span data-ttu-id="6cea9-179">Det här kommandot tar bort alla parametrar för program-kapacitet från programinstansen.</span><span class="sxs-lookup"><span data-stu-id="6cea9-179">This command removes all Application capacity management parameters from the application instance.</span></span> <span data-ttu-id="6cea9-180">Detta inkluderar MinimumNodes, MaximumNodes och programmets statistik, om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="6cea9-180">This includes MinimumNodes, MaximumNodes, and the Application's metrics, if any.</span></span> <span data-ttu-id="6cea9-181">Effekten av kommandot sker omedelbart.</span><span class="sxs-lookup"><span data-stu-id="6cea9-181">The effect of the command is immediate.</span></span> <span data-ttu-id="6cea9-182">När det här kommandot har slutförts använder kluster Resource Manager standardbeteendet för att hantera program.</span><span class="sxs-lookup"><span data-stu-id="6cea9-182">After this command completes, the Cluster Resource Manager uses the default behavior for managing applications.</span></span> <span data-ttu-id="6cea9-183">Programmet kapacitet parametrar kan anges igen `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.</span><span class="sxs-lookup"><span data-stu-id="6cea9-183">Application Capacity parameters can be specified again via `Update-ServiceFabricApplication`/`System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.</span></span>

### <a name="restrictions-on-application-capacity"></a><span data-ttu-id="6cea9-184">Begränsningar för programmet kapacitet</span><span class="sxs-lookup"><span data-stu-id="6cea9-184">Restrictions on Application Capacity</span></span>
<span data-ttu-id="6cea9-185">Det finns flera begränsningar för programmet kapacitet parametrar som måste följas.</span><span class="sxs-lookup"><span data-stu-id="6cea9-185">There are several restrictions on Application Capacity parameters that must be respected.</span></span> <span data-ttu-id="6cea9-186">Om det finns valideringsfel görs inga ändringar.</span><span class="sxs-lookup"><span data-stu-id="6cea9-186">If there are validation errors no changes take place.</span></span>

- <span data-ttu-id="6cea9-187">Alla måste heltal vara icke-negativt tal.</span><span class="sxs-lookup"><span data-stu-id="6cea9-187">All integer parameters must be non-negative numbers.</span></span>
- <span data-ttu-id="6cea9-188">MinimumNodes måste aldrig vara större än MaximumNodes.</span><span class="sxs-lookup"><span data-stu-id="6cea9-188">MinimumNodes must never be greater than MaximumNodes.</span></span>
- <span data-ttu-id="6cea9-189">Om kapaciteten för ett mått för belastningen definieras måste de följa reglerna:</span><span class="sxs-lookup"><span data-stu-id="6cea9-189">If capacities for a load metric are defined, then they must follow these rules:</span></span>
  - <span data-ttu-id="6cea9-190">Noden Reservation Capacity får inte vara större än den maximala nod kapaciteten.</span><span class="sxs-lookup"><span data-stu-id="6cea9-190">Node Reservation Capacity must not be greater than Maximum Node Capacity.</span></span> <span data-ttu-id="6cea9-191">Du kan till exempel begränsa kapacitet för måttet ”CPU” på noden till två enheter och försök att reservera tre enheter på varje nod.</span><span class="sxs-lookup"><span data-stu-id="6cea9-191">For example, you cannot limit the capacity for the metric “CPU” on the node to two units and try to reserve three units on each node.</span></span>
  - <span data-ttu-id="6cea9-192">Om MaximumNodes anges får sedan produkten av MaximumNodes och maximumkapacitet för noden inte vara större än den Totalkapaciteten för programmet.</span><span class="sxs-lookup"><span data-stu-id="6cea9-192">If MaximumNodes is specified, then the product of MaximumNodes and Maximum Node Capacity must not be greater than Total Application Capacity.</span></span> <span data-ttu-id="6cea9-193">Anta till exempel noden maximumkapacitet för belastningen mått ”CPU” anges till åtta.</span><span class="sxs-lookup"><span data-stu-id="6cea9-193">For example, let's say the Maximum Node Capacity for load metric “CPU” is set to eight.</span></span> <span data-ttu-id="6cea9-194">Anta också att du har angett maximalt antal noder till 10.</span><span class="sxs-lookup"><span data-stu-id="6cea9-194">Let's also say you set the Maximum Nodes to 10.</span></span> <span data-ttu-id="6cea9-195">I det här fallet måste totalkapacitet för programmet vara större än 80 för mätvärdet belastningen.</span><span class="sxs-lookup"><span data-stu-id="6cea9-195">In this case, Total Application Capacity must be greater than 80 for this load metric.</span></span>

<span data-ttu-id="6cea9-196">Begränsningarna tillämpas både under skapa program och uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="6cea9-196">The restrictions are enforced both during application creation and updates.</span></span>

## <a name="how-not-to-use-application-capacity"></a><span data-ttu-id="6cea9-197">Hur inte att använda programmet kapacitet</span><span class="sxs-lookup"><span data-stu-id="6cea9-197">How not to use Application Capacity</span></span>
- <span data-ttu-id="6cea9-198">Försök inte att använda funktioner i programgruppen du vill begränsa programmet till en _specifika_ del noder.</span><span class="sxs-lookup"><span data-stu-id="6cea9-198">Do not try to use the Application Group features to constrain the application to a _specific_ subset of nodes.</span></span> <span data-ttu-id="6cea9-199">Med andra ord kan du ange att programmet körs på högst fem noder, men inte vilka specifika fem noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="6cea9-199">In other words, you can specify that the application runs on at most five nodes, but not which specific five nodes in the cluster.</span></span> <span data-ttu-id="6cea9-200">Begränsa ett program till specifika noder kan ske med hjälp av placeringsbegränsningar för tjänster.</span><span class="sxs-lookup"><span data-stu-id="6cea9-200">Constraining an application to specific nodes can be achieved using placement constraints for services.</span></span>
- <span data-ttu-id="6cea9-201">Försök inte att använda programmet kapacitet för att säkerställa att två tjänster från samma program är placerade på samma noder.</span><span class="sxs-lookup"><span data-stu-id="6cea9-201">Do not try to use the Application Capacity to ensure that two services from the same application are placed on the same nodes.</span></span> <span data-ttu-id="6cea9-202">I stället använda [tillhörighet](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) eller [placeringen](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).</span><span class="sxs-lookup"><span data-stu-id="6cea9-202">Instead use [affinity](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) or [placement constraints](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6cea9-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6cea9-203">Next steps</span></span>
- <span data-ttu-id="6cea9-204">Mer information om hur du konfigurerar tjänster, [Lär dig mer om hur du konfigurerar tjänster](service-fabric-cluster-resource-manager-configure-services.md)</span><span class="sxs-lookup"><span data-stu-id="6cea9-204">For more information on configuring services, [Learn about configuring Services](service-fabric-cluster-resource-manager-configure-services.md)</span></span>
- <span data-ttu-id="6cea9-205">Om du vill veta mer om hur klustret Resource Manager hanterar och balanserar belastningen i klustret kan ta en titt i artikeln på [belastningsutjämning](service-fabric-cluster-resource-manager-balancing.md)</span><span class="sxs-lookup"><span data-stu-id="6cea9-205">To find out about how the Cluster Resource Manager manages and balances load in the cluster, check out the article on [balancing load](service-fabric-cluster-resource-manager-balancing.md)</span></span>
- <span data-ttu-id="6cea9-206">Börja från början och [få en introduktion till Service Fabric klustret Resource Manager](service-fabric-cluster-resource-manager-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="6cea9-206">Start from the beginning and [get an Introduction to the Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)</span></span>
- <span data-ttu-id="6cea9-207">Mer information om hur mått fungerar normalt läsa på [Service Fabric belastningen mått](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="6cea9-207">For more information on how metrics work generally, read up on [Service Fabric Load Metrics](service-fabric-cluster-resource-manager-metrics.md)</span></span>
- <span data-ttu-id="6cea9-208">Klustret Resource Manager har många alternativ för att beskriva klustret.</span><span class="sxs-lookup"><span data-stu-id="6cea9-208">The Cluster Resource Manager has many options for describing the cluster.</span></span> <span data-ttu-id="6cea9-209">Kolla in den här artikeln om du vill veta mer om dem på [som beskriver ett Service Fabric-kluster](service-fabric-cluster-resource-manager-cluster-description.md)</span><span class="sxs-lookup"><span data-stu-id="6cea9-209">To find out more about them, check out this article on [describing a Service Fabric cluster](service-fabric-cluster-resource-manager-cluster-description.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
