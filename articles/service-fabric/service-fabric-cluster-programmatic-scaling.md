---
title: "aaaAzure Service Fabric programmässiga skalning | Microsoft Docs"
description: "Skala ett Azure Service Fabric-kluster in eller ut programmässigt, enligt toocustom utlösare"
services: service-fabric
documentationcenter: .net
author: mjrousos
manager: jonjung
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: mikerou
ms.openlocfilehash: a0c6499b1a143a173006248cf8a15380632637e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a><span data-ttu-id="262cc-103">Skala Service Fabric-klustret via programmering</span><span class="sxs-lookup"><span data-stu-id="262cc-103">Scale a Service Fabric cluster programmatically</span></span> 

<span data-ttu-id="262cc-104">Grunderna i skala ett Service Fabric-kluster i Azure beskrivs i dokumentationen på [skalning av klustret](./service-fabric-cluster-scale-up-down.md).</span><span class="sxs-lookup"><span data-stu-id="262cc-104">Fundamentals of scaling a Service Fabric cluster in Azure are covered in documentation on [cluster scaling](./service-fabric-cluster-scale-up-down.md).</span></span> <span data-ttu-id="262cc-105">Artikeln beskriver hur Service Fabric-kluster är byggda på virtuella datorer och kan skalas manuellt eller med Autoskala regler.</span><span class="sxs-lookup"><span data-stu-id="262cc-105">That article covers how Service Fabric clusters are built on top of virtual machine scale sets and can be scaled either manually or with auto-scale rules.</span></span> <span data-ttu-id="262cc-106">Det här dokumentet tittar på programmering koordinerande Azure skalning åtgärder för mer avancerade scenarier.</span><span class="sxs-lookup"><span data-stu-id="262cc-106">This document looks at programmatic methods of coordinating Azure scaling operations for more advanced scenarios.</span></span> 

## <a name="reasons-for-programmatic-scaling"></a><span data-ttu-id="262cc-107">Orsaker till programmässiga skalning</span><span class="sxs-lookup"><span data-stu-id="262cc-107">Reasons for programmatic scaling</span></span>
<span data-ttu-id="262cc-108">I många fall är är skalning manuellt eller via Autoskala regler bra lösningar.</span><span class="sxs-lookup"><span data-stu-id="262cc-108">In many scenarios, scaling manually or via auto-scale rules are good solutions.</span></span> <span data-ttu-id="262cc-109">I andra scenarier, men får de inte vara hello rätt plats.</span><span class="sxs-lookup"><span data-stu-id="262cc-109">In other scenarios, though, they may not be hello right fit.</span></span> <span data-ttu-id="262cc-110">Potentiella nackdelar toothese metoder är:</span><span class="sxs-lookup"><span data-stu-id="262cc-110">Potential drawbacks toothese approaches include:</span></span>

- <span data-ttu-id="262cc-111">Skalning manuellt måste du toolog i och uttryckligen begära skalning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="262cc-111">Manually scaling requires you toolog in and explicitly request scaling operations.</span></span> <span data-ttu-id="262cc-112">Om skalning åtgärder krävs ofta eller vid oväntade tidpunkter, kanske inte en bra lösning i den här metoden.</span><span class="sxs-lookup"><span data-stu-id="262cc-112">If scaling operations are required frequently or at unpredictable times, this approach may not be a good solution.</span></span>
- <span data-ttu-id="262cc-113">När Autoskala regler för att ta bort en instans från en skaluppsättning för virtuell dator, de inte bort automatiskt kunskap om noden från hello associerade Service Fabric-klustret såvida inte hello nodtyp har en hållbarhet Silver eller guld.</span><span class="sxs-lookup"><span data-stu-id="262cc-113">When auto-scale rules remove an instance from a virtual machine scale set, they do not automatically remove knowledge of that node from hello associated Service Fabric cluster unless hello node type has a durability level of Silver or Gold.</span></span> <span data-ttu-id="262cc-114">Eftersom Autoskala regler fungerar i hello skala ange nivån (i stället för vid hello Service Fabric-nivå), Autoskala regler kan ta bort Service Fabric-noder utan att dem avslutas.</span><span class="sxs-lookup"><span data-stu-id="262cc-114">Because auto-scale rules work at hello scale set level (rather than at hello Service Fabric level), auto-scale rules can remove Service Fabric nodes without shutting them down gracefully.</span></span> <span data-ttu-id="262cc-115">Borttagningen oartigt nod lämnar 'ghost-tillstånd för Service Fabric-noden efter efter skala i operations.</span><span class="sxs-lookup"><span data-stu-id="262cc-115">This rude node removal will leave 'ghost' Service Fabric node state behind after scale-in operations.</span></span> <span data-ttu-id="262cc-116">En person (eller en tjänst) måste tooperiodically Rensa borttagna noden tillstånd i hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="262cc-116">An individual (or a service) would need tooperiodically clean up removed node state in hello Service Fabric cluster.</span></span>
  - <span data-ttu-id="262cc-117">Observera att en nodtyp med hållbarhet guld eller Silver automatiskt rensar bort noder.</span><span class="sxs-lookup"><span data-stu-id="262cc-117">Note that a node type with a durability level of Gold or Silver will automatically clean up removed nodes.</span></span>  
- <span data-ttu-id="262cc-118">Även om det finns [många mått](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) stöds av Autoskala regler, är det fortfarande en begränsad uppsättning.</span><span class="sxs-lookup"><span data-stu-id="262cc-118">Although there are [many metrics](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) supported by auto-scale rules, it is still a limited set.</span></span> <span data-ttu-id="262cc-119">Om ditt scenario anrop för att skala baserat på vissa mått som inte omfattas i uppsättningen kan sedan kanske Autoskala regler inte ett bra alternativ.</span><span class="sxs-lookup"><span data-stu-id="262cc-119">If your scenario calls for scaling based on some metric not covered in that set, then auto-scale rules may not be a good option.</span></span>

<span data-ttu-id="262cc-120">Baserat på dessa begränsningar du tooimplement mer anpassade automatisk skalning modeller.</span><span class="sxs-lookup"><span data-stu-id="262cc-120">Based on these limitations, you may wish tooimplement more customized automatic scaling models.</span></span> 

## <a name="scaling-apis"></a><span data-ttu-id="262cc-121">Skalning API: er</span><span class="sxs-lookup"><span data-stu-id="262cc-121">Scaling APIs</span></span>
<span data-ttu-id="262cc-122">Azure API: er finns som tillåter program tooprogrammatically fungerar med skalningsuppsättningar i virtuella datorer och Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="262cc-122">Azure APIs exist which allow applications tooprogrammatically work with virtual machine scale sets and Service Fabric clusters.</span></span> <span data-ttu-id="262cc-123">Om den befintliga Autoskala alternativ inte fungerar för ditt scenario, dessa API: er som gör att den möjliga tooimplement anpassade skalning logik.</span><span class="sxs-lookup"><span data-stu-id="262cc-123">If existing auto-scale options don't work for your scenario, these APIs make it possible tooimplement custom scaling logic.</span></span> 

<span data-ttu-id="262cc-124">En metod som tooimplementing detta ”home-tillverkade” automatisk skalning funktioner är tooadd en ny tillståndslösa tjänsten toohello Service Fabric application toomanage skalning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="262cc-124">One approach tooimplementing this 'home-made' auto-scaling functionality is tooadd a new stateless service toohello Service Fabric application toomanage scaling operations.</span></span> <span data-ttu-id="262cc-125">I hello tjänsten `RunAsync` metod, en uppsättning utlösare kan avgöra om skalning krävs (inklusive kontrollerar parametrar, till exempel klustrets maximala storlek och skala cooldowns).</span><span class="sxs-lookup"><span data-stu-id="262cc-125">Within hello service's `RunAsync` method, a set of triggers can determine if scaling is required (including checking parameters such as maximum cluster size and scaling cooldowns).</span></span>   

<span data-ttu-id="262cc-126">hello API som används för virtual machine scale set interaktioner (både toocheck hello aktuellt antal instanser för virtuella datorer och toomodify den) är hello [flytande Azure Management Compute biblioteket](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/).</span><span class="sxs-lookup"><span data-stu-id="262cc-126">hello API used for virtual machine scale set interactions (both toocheck hello current number of virtual machine instances and toomodify it) is hello [fluent Azure Management Compute library](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/).</span></span> <span data-ttu-id="262cc-127">hello flytande beräkning biblioteket tillhandahåller ett enkelt att använda API för att interagera med virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="262cc-127">hello fluent compute library provides an easy-to-use API for interacting with virtual machine scale sets.</span></span>

<span data-ttu-id="262cc-128">toointeract med hello Service Fabric-kluster, använda [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).</span><span class="sxs-lookup"><span data-stu-id="262cc-128">toointeract with hello Service Fabric cluster itself, use [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).</span></span>

<span data-ttu-id="262cc-129">Naturligtvis hello skalning kod behöver inte toorun som en tjänst i hello klustret toobe skalas.</span><span class="sxs-lookup"><span data-stu-id="262cc-129">Of course, hello scaling code doesn't need toorun as a service in hello cluster toobe scaled.</span></span> <span data-ttu-id="262cc-130">Båda `IAzure` och `FabricClient` kan fjärransluta tootheir associerade Azure-resurser, så hello skalning service kan enkelt ett konsolprogram eller Windows-tjänsten körs från utanför hello Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="262cc-130">Both `IAzure` and `FabricClient` can connect tootheir associated Azure resources remotely, so hello scaling service could easily be a console application or Windows service running from outside hello Service Fabric application.</span></span> 

## <a name="credential-management"></a><span data-ttu-id="262cc-131">Hantering av autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="262cc-131">Credential management</span></span>
<span data-ttu-id="262cc-132">En utmaning för att skriva en service toohandle skalning är att hello tjänsten måste vara kan tooaccess scale set datorresurser utan interaktiv inloggning.</span><span class="sxs-lookup"><span data-stu-id="262cc-132">One challenge of writing a service toohandle scaling is that hello service must be able tooaccess virtual machine scale set resources without an interactive login.</span></span> <span data-ttu-id="262cc-133">Åtkomst till hello Service Fabric klustret är enkelt om hello skalning tjänsten ändrar sin egen Service Fabric-program, men autentiseringsuppgifterna är nödvändiga tooaccess hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="262cc-133">Accessing hello Service Fabric cluster is easy if hello scaling service is modifying its own Service Fabric application, but credentials are needed tooaccess hello scale set.</span></span> <span data-ttu-id="262cc-134">toolog i som du kan använda en [tjänstens huvudnamn](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) skapats med hello [Azure CLI 2.0](https://github.com/azure/azure-cli).</span><span class="sxs-lookup"><span data-stu-id="262cc-134">toolog in, you can use a [service principal](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) created with hello [Azure CLI 2.0](https://github.com/azure/azure-cli).</span></span>

<span data-ttu-id="262cc-135">Du kan skapa ett huvudnamn för tjänsten med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="262cc-135">A service principal can be created with hello following steps:</span></span>

1. <span data-ttu-id="262cc-136">Logga in toohello Azure CLI (`az login`) som en användare med åtkomst toohello virtuella skala anger</span><span class="sxs-lookup"><span data-stu-id="262cc-136">Log in toohello Azure CLI (`az login`) as a user with access toohello virtual machine scale set</span></span>
2. <span data-ttu-id="262cc-137">Skapa hello tjänstens huvudnamn med`az ad sp create-for-rbac`</span><span class="sxs-lookup"><span data-stu-id="262cc-137">Create hello service principal with `az ad sp create-for-rbac`</span></span>
    1. <span data-ttu-id="262cc-138">Anteckna hello appId (kallas klient-ID någon annanstans), namn, lösenord och klientorganisations för senare användning.</span><span class="sxs-lookup"><span data-stu-id="262cc-138">Make note of hello appId (called 'client ID' elsewhere), name, password, and tenant for later use.</span></span>
    2. <span data-ttu-id="262cc-139">Du måste också ditt prenumerations-ID som kan visas med`az account list`</span><span class="sxs-lookup"><span data-stu-id="262cc-139">You will also need your subscription ID, which can be viewed with `az account list`</span></span>

<span data-ttu-id="262cc-140">hello flytande beräknings-biblioteket kan logga in med autentiseringsuppgifterna enligt följande (Observera att core flytande Azure typer som `IAzure` i hello [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) paketet):</span><span class="sxs-lookup"><span data-stu-id="262cc-140">hello fluent compute library can log in using these credentials as follows (note that core fluent Azure types like `IAzure` are in hello [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) package):</span></span>

```C#
var credentials = new AzureCredentials(new ServicePrincipalLoginInformation {
                ClientId = AzureClientId,
                ClientSecret = 
                AzureClientKey }, AzureTenantId, AzureEnvironment.AzureGlobalCloud);
IAzure AzureClient = Azure.Authenticate(credentials).WithSubscription(AzureSubscriptionId);

if (AzureClient?.SubscriptionId == AzureSubscriptionId)
{
    ServiceEventSource.Current.ServiceMessage(Context, "Successfully logged into Azure");
}
else
{
    ServiceEventSource.Current.ServiceMessage(Context, "ERROR: Failed toologin tooAzure");
}
```

<span data-ttu-id="262cc-141">När du loggade in scale set-instanser kan efterfrågas `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.</span><span class="sxs-lookup"><span data-stu-id="262cc-141">Once logged in, scale set instance count can be queried via `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.</span></span>

## <a name="scaling-out"></a><span data-ttu-id="262cc-142">Skala ut</span><span class="sxs-lookup"><span data-stu-id="262cc-142">Scaling out</span></span>
<span data-ttu-id="262cc-143">Med hjälp av flytande hello Azure compute SDK, instanser kan läggas till toohello virtuella skaluppsättningen med bara några få anrop-</span><span class="sxs-lookup"><span data-stu-id="262cc-143">Using hello fluent Azure compute SDK, instances can be added toohello virtual machine scale set with just a few calls -</span></span>

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

<span data-ttu-id="262cc-144">Du kan också kan virtuella skala storlek också hanteras med PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="262cc-144">Alternatively, virtual machine scale set size can also be managed with PowerShell cmdlets.</span></span> <span data-ttu-id="262cc-145">[`Get-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmss)Hämta hello virtuella Skala uppsättningsobjekt.</span><span class="sxs-lookup"><span data-stu-id="262cc-145">[`Get-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmss) can retrieve hello virtual machine scale set object.</span></span> <span data-ttu-id="262cc-146">hello aktuella kapaciteten kommer att lagras i hello `.sku.capacity` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="262cc-146">hello current capacity will be stored in hello `.sku.capacity` property.</span></span> <span data-ttu-id="262cc-147">När du ändrar hello kapacitet toohello önskas värdet hello virtuella skaluppsättningen i Azure kan uppdateras med hello [ `Update-AzureRmVmss` ](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/update-azurermvmss) kommando.</span><span class="sxs-lookup"><span data-stu-id="262cc-147">After changing hello capacity toohello desired value, hello virtual machine scale set in Azure can be updated with hello [`Update-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/update-azurermvmss) command.</span></span>

<span data-ttu-id="262cc-148">Som när du lägger till en nod manuellt lägga till en skaluppsättning för instansen bör vara alla som behövs toostart som innehåller en ny Service Fabric-nod eftersom hello skaluppsättning mall Anslut tillägg tooautomatically nya instanser toohello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="262cc-148">As when adding a node manually, adding a scale set instance should be all that's needed toostart a new Service Fabric node since hello scale set template includes extensions tooautomatically join new instances toohello Service Fabric cluster.</span></span> 

## <a name="scaling-in"></a><span data-ttu-id="262cc-149">Skalning i</span><span class="sxs-lookup"><span data-stu-id="262cc-149">Scaling in</span></span>

<span data-ttu-id="262cc-150">Skalning i är liknande tooscaling ut. hello den faktiska virtuella datorns skaluppsättning ändringar är praktiskt taget hello samma.</span><span class="sxs-lookup"><span data-stu-id="262cc-150">Scaling in is similar tooscaling out. hello actual virtual machine scale set changes are practically hello same.</span></span> <span data-ttu-id="262cc-151">Men som har diskuterats tidigare, Service Fabric endast rensas automatiskt bort noder med en hållbarhet guld eller Silver.</span><span class="sxs-lookup"><span data-stu-id="262cc-151">But, as was discussed previously, Service Fabric only automatically cleans up removed nodes with a durability of Gold or Silver.</span></span> <span data-ttu-id="262cc-152">Så hello Brons hållbarhet skala i om det är nödvändigt toointeract med hello Service Fabric-kluster tooshut ned hello nod toobe tas bort och tooremove dess tillstånd.</span><span class="sxs-lookup"><span data-stu-id="262cc-152">So, in hello Bronze-durability scale-in case, it's necessary toointeract with hello Service Fabric cluster tooshut down hello node toobe removed and then tooremove its state.</span></span>

<span data-ttu-id="262cc-153">Förbereda hello nod för avstängning gäller att hitta hello nod toobe bort (hello nyligen tillagda noden) och inaktivera den.</span><span class="sxs-lookup"><span data-stu-id="262cc-153">Preparing hello node for shutdown involves finding hello node toobe removed (hello most recently added node) and deactivating it.</span></span> <span data-ttu-id="262cc-154">För icke-seed noder nyare noder kan hittas genom att jämföra `NodeInstanceId`.</span><span class="sxs-lookup"><span data-stu-id="262cc-154">For non-seed nodes, newer nodes can be found by comparing `NodeInstanceId`.</span></span> 

```C#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n => n.NodeInstanceId)
        .FirstOrDefault();
```

<span data-ttu-id="262cc-155">Tänk som *seed* noder inte verkar vara tooalways följer hello konventionen större instans-ID: N tas bort först.</span><span class="sxs-lookup"><span data-stu-id="262cc-155">Be aware that *seed* nodes don't seem tooalways follow hello convention that greater instance IDs are removed first.</span></span>

<span data-ttu-id="262cc-156">När hello nod toobe bort hittas kan inaktiveras och tas bort med hjälp av hello samma `FabricClient` instansen och hello `IAzure` instansen från tidigare.</span><span class="sxs-lookup"><span data-stu-id="262cc-156">Once hello node toobe removed is found, it can be deactivated and removed using hello same `FabricClient` instance and hello `IAzure` instance from earlier.</span></span>

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove hello node from hello Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up tooa timeout) for hello node toogracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

<span data-ttu-id="262cc-157">Som med skala ut PowerShell-cmdlets för att ändra virtuella datorn kan set kapacitet även användas här om en scripting metod är att föredra.</span><span class="sxs-lookup"><span data-stu-id="262cc-157">As with scaling out, PowerShell cmdlets for modifying virtual machine scale set capacity can also be used here if a scripting approach is preferable.</span></span> <span data-ttu-id="262cc-158">När hello virtuella instansen har tagits bort, kan tas bort Service Fabric nodens tillstånd.</span><span class="sxs-lookup"><span data-stu-id="262cc-158">Once hello virtual machine instance is removed, Service Fabric node state can be removed.</span></span>

```C#
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="potential-drawbacks"></a><span data-ttu-id="262cc-159">Nackdelarna</span><span class="sxs-lookup"><span data-stu-id="262cc-159">Potential drawbacks</span></span>

<span data-ttu-id="262cc-160">Som visas i föregående kodfragment hello ger skapa egna skalning tjänst hello största möjliga kontroll och anpassningsbarheten över ditt program skalning beteende.</span><span class="sxs-lookup"><span data-stu-id="262cc-160">As demonstrated in hello preceding code snippets, creating your own scaling service provides hello highest degree of control and customizability over your application's scaling behavior.</span></span> <span data-ttu-id="262cc-161">Detta kan vara användbart för scenarier som kräver exakt kontroll över när eller hur ett program skalas in eller ut. Den här kontrollen har dock en kompromiss förenklar koden.</span><span class="sxs-lookup"><span data-stu-id="262cc-161">This can be useful for scenarios requiring precise control over when or how an application scales in or out. However, this control comes with a tradeoff of code complexity.</span></span> <span data-ttu-id="262cc-162">Med den här metoden innebär att du måste tooown skalning kod, som är icke-trivial.</span><span class="sxs-lookup"><span data-stu-id="262cc-162">Using this approach means that you need tooown scaling code, which is non-trivial.</span></span>

<span data-ttu-id="262cc-163">Hur ska du bör Service Fabric skalning beror på ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="262cc-163">How you should approach Service Fabric scaling depends on your scenario.</span></span> <span data-ttu-id="262cc-164">Om det är ovanligt att skalning, hello möjlighet tooadd eller ta bort noder manuellt räcker förmodligen.</span><span class="sxs-lookup"><span data-stu-id="262cc-164">If scaling is uncommon, hello ability tooadd or remove nodes manually is probably sufficient.</span></span> <span data-ttu-id="262cc-165">För mer komplicerade scenarier erbjuder Autoskala regler och SDK exponera hello möjlighet tooscale programmässigt kraftfulla alternativ.</span><span class="sxs-lookup"><span data-stu-id="262cc-165">For more complex scenarios, auto-scale rules and SDKs exposing hello ability tooscale programmatically offer powerful alternatives.</span></span>

## <a name="next-steps"></a><span data-ttu-id="262cc-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="262cc-166">Next steps</span></span>

<span data-ttu-id="262cc-167">tooget igång implementera egna automatisk skalning logik bekanta dig med hello följande begrepp och användbara API: er:</span><span class="sxs-lookup"><span data-stu-id="262cc-167">tooget started implementing your own auto-scaling logic, familiarize yourself with hello following concepts and useful APIs:</span></span>

- [<span data-ttu-id="262cc-168">Skalning manuellt eller med Autoskala regler</span><span class="sxs-lookup"><span data-stu-id="262cc-168">Scaling manually or with auto-scale rules</span></span>](./service-fabric-cluster-scale-up-down.md)
- <span data-ttu-id="262cc-169">[Flytande Azure-bibliotek för .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (användbart för att interagera med Service Fabric-klustret underliggande skalningsuppsättningar i virtuella datorer)</span><span class="sxs-lookup"><span data-stu-id="262cc-169">[Fluent Azure Management Libraries for .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (useful for interacting with a Service Fabric cluster's underlying virtual machine scale sets)</span></span>
- <span data-ttu-id="262cc-170">[System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (användbart för att interagera med ett Service Fabric-kluster och noder)</span><span class="sxs-lookup"><span data-stu-id="262cc-170">[System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (useful for interacting with a Service Fabric cluster and its nodes)</span></span>
