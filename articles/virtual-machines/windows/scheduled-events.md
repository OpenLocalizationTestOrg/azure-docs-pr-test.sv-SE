---
title: "aaaScheduled händelser för virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Schemalagda händelser med hjälp av hello Azure Metadata-tjänsten för på Windows-datorer."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: zivr
ms.openlocfilehash: c9f5f332a5d77e8d54d1ae8bdaadafc1a14f3b77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-metadata-service-scheduled-events-preview-for-windows-vms"></a><span data-ttu-id="da5bb-103">Metadata Azure: Schemalagda händelser (förhandsversion) för virtuella Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="da5bb-103">Azure Metadata Service: Scheduled Events (Preview) for Windows VMs</span></span>

> [!NOTE] 
> <span data-ttu-id="da5bb-104">Förhandsgranskningar görs tillgängliga tooyou hello förutsatt att du godkänner toohello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="da5bb-104">Previews are made available tooyou on hello condition that you agree toohello terms of use.</span></span> <span data-ttu-id="da5bb-105">Mer information finns i [de kompletterande villkoren för användning av Microsoft Azure-förhandsversioner](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).</span><span class="sxs-lookup"><span data-stu-id="da5bb-105">For more information, see [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).</span></span>
>

<span data-ttu-id="da5bb-106">Schemalagda händelser är en av hello subservices under hello Azure Metadata Service.</span><span class="sxs-lookup"><span data-stu-id="da5bb-106">Scheduled Events is one of hello subservices under hello Azure Metadata Service.</span></span> <span data-ttu-id="da5bb-107">Ansvarar för att visa information om kommande händelser (till exempel omstart) så att programmet kan förbereda för dem och begränsa avbrott.</span><span class="sxs-lookup"><span data-stu-id="da5bb-107">It is responsible for surfacing information regarding upcoming events (for example, reboot) so your application can prepare for them and limit disruption.</span></span> <span data-ttu-id="da5bb-108">Den är tillgänglig för alla typer av Azure virtuella datorer inklusive PaaS och IaaS.</span><span class="sxs-lookup"><span data-stu-id="da5bb-108">It is available for all Azure Virtual Machine types including PaaS and IaaS.</span></span> <span data-ttu-id="da5bb-109">Schemalagda händelser ger virtuella tid tooperform förebyggande aktiviteterna toominimize hello effekten av en händelse.</span><span class="sxs-lookup"><span data-stu-id="da5bb-109">Scheduled Events gives your Virtual Machine time tooperform preventive tasks toominimize hello effect of an event.</span></span> 

<span data-ttu-id="da5bb-110">Schemalagda händelser är tillgänglig för både Linux och Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="da5bb-110">Scheduled Events is available for both Linux and Windows VMs.</span></span> <span data-ttu-id="da5bb-111">Information om schemalagda händelser på Linux finns [schemalagda händelser för virtuella Linux-datorer](../windows/scheduled-events.md).</span><span class="sxs-lookup"><span data-stu-id="da5bb-111">For information about Scheduled Events on Linux, see [Scheduled Events for Linux VMs](../windows/scheduled-events.md).</span></span>

## <a name="why-scheduled-events"></a><span data-ttu-id="da5bb-112">Varför schemalagda händelser?</span><span class="sxs-lookup"><span data-stu-id="da5bb-112">Why Scheduled Events?</span></span>

<span data-ttu-id="da5bb-113">Schemalagda händelser, kan du vidta åtgärder för toolimit hello effekten av plattform intiated underhåll eller användare utför åtgärder på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="da5bb-113">With Scheduled Events, you can take steps toolimit hello impact of platform-intiated maintenance or user-initiated actions on your service.</span></span> 

<span data-ttu-id="da5bb-114">Flera instanser arbetsbelastningar som använder replikeringstillståndet tekniker toomaintain kanske sårbara toooutages händer i flera instanser.</span><span class="sxs-lookup"><span data-stu-id="da5bb-114">Multi-instance workloads, which use replication techniques toomaintain state, may be vulnerable toooutages happening across multiple instances.</span></span> <span data-ttu-id="da5bb-115">Exempel driftsavbrott kan resultera i dyra aktiviteter (till exempel återskapande index) eller även en replik går förlorade.</span><span class="sxs-lookup"><span data-stu-id="da5bb-115">Such outages may result in expensive tasks (for example, rebuilding indexes) or even a replica loss.</span></span> 

<span data-ttu-id="da5bb-116">I många andra fall hello övergripande tjänsttillgängligheten kan förbättras genom att utföra en korrekt avslutningssekvens som Slutför (eller annullerar) pågående transaktioner, tilldela uppgifter tooother virtuella datorer i hello kluster (manuell redundans) eller ta bort hello Den virtuella datorn från poolen network load belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="da5bb-116">In many other cases, hello overall service availability may be improved by performing a graceful shutdown sequence such as completing (or canceling) in-flight transactions, reassigning tasks tooother VMs in hello cluster (manual failover), or removing hello Virtual Machine from a network load balancer pool.</span></span> 

<span data-ttu-id="da5bb-117">Finns det fall där tala med administratören om en kommande händelse eller logga sådan händelse att förbättra hello brukbarheten av program som finns i molnet hello.</span><span class="sxs-lookup"><span data-stu-id="da5bb-117">There are cases where notifying an administrator about an upcoming event or logging such an event help improving hello serviceability of applications hosted in hello cloud.</span></span>

<span data-ttu-id="da5bb-118">Azure Metadata Service hämtar schemalagda händelser i hello följande användningsfall:</span><span class="sxs-lookup"><span data-stu-id="da5bb-118">Azure Metadata Service surfaces Scheduled Events in hello following use cases:</span></span>
-   <span data-ttu-id="da5bb-119">Plattform som initierade Underhåll (till exempel värd-OS-installationen)</span><span class="sxs-lookup"><span data-stu-id="da5bb-119">Platform initiated maintenance (for example, Host OS rollout)</span></span>
-   <span data-ttu-id="da5bb-120">Användarinitierad anrop (till exempel användaren startar om eller återdistribuerar en virtuell dator)</span><span class="sxs-lookup"><span data-stu-id="da5bb-120">User-initiated calls (for example, user restarts or redeploys a VM)</span></span>


## <a name="hello-basics"></a><span data-ttu-id="da5bb-121">hello-grunderna</span><span class="sxs-lookup"><span data-stu-id="da5bb-121">hello basics</span></span>  

<span data-ttu-id="da5bb-122">Metadata i Azure-tjänsten visar information om att köra virtuella datorer med en REST-slutpunkt som är tillgänglig från hello VM.</span><span class="sxs-lookup"><span data-stu-id="da5bb-122">Azure Metadata service exposes information about running Virtual Machines using a REST Endpoint accessible from within hello VM.</span></span> <span data-ttu-id="da5bb-123">hello-information är tillgänglig via en icke-dirigerbara IP-adress så att inte exponeras utanför hello VM.</span><span class="sxs-lookup"><span data-stu-id="da5bb-123">hello information is available via a non-routable IP so that it is not exposed outside hello VM.</span></span>

### <a name="scope"></a><span data-ttu-id="da5bb-124">Omfång</span><span class="sxs-lookup"><span data-stu-id="da5bb-124">Scope</span></span>
<span data-ttu-id="da5bb-125">Schemalagda händelser är på ytan tooall virtuella datorer i en molnbaserad tjänst eller tooall virtuella datorer i en Tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="da5bb-125">Scheduled events are surfaced tooall Virtual Machines in a cloud service or tooall Virtual Machines in an Availability Set.</span></span> <span data-ttu-id="da5bb-126">Därför bör du kontrollera hello `Resources` i hello händelse tooidentify som virtuella datorer ska toobe påverkas.</span><span class="sxs-lookup"><span data-stu-id="da5bb-126">As a result, you should check hello `Resources` field in hello event tooidentify which VMs are going toobe impacted.</span></span> 

### <a name="discovering-hello-endpoint"></a><span data-ttu-id="da5bb-127">Identifiera hello slutpunkt</span><span class="sxs-lookup"><span data-stu-id="da5bb-127">Discovering hello endpoint</span></span>
<span data-ttu-id="da5bb-128">I hello fall där en virtuell dator skapas ett virtuellt nätverk (VNet) hello metadata-tjänsten är tillgänglig från en statisk icke-dirigerbara IP `169.254.169.254`.</span><span class="sxs-lookup"><span data-stu-id="da5bb-128">In hello case where a Virtual Machine is created within a Virtual Network (VNet), hello metadata service is available from a static non-routable IP, `169.254.169.254`.</span></span>
<span data-ttu-id="da5bb-129">Om hello virtuell dator inte skapas inom ett virtuellt nätverk hello fall för molntjänster och klassiska virtuella datorer, är obligatoriska toodiscover hello endpoint toouse ytterligare logik.</span><span class="sxs-lookup"><span data-stu-id="da5bb-129">If hello Virtual Machine is not created within a Virtual Network, hello default cases for cloud services and classic VMs, additional logic is required toodiscover hello endpoint toouse.</span></span> <span data-ttu-id="da5bb-130">Se hur för toothis exempel toolearn[identifiera hello värden slutpunkt](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span><span class="sxs-lookup"><span data-stu-id="da5bb-130">Refer toothis sample toolearn how too[discover hello host endpoint](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span></span>

### <a name="versioning"></a><span data-ttu-id="da5bb-131">Versionshantering</span><span class="sxs-lookup"><span data-stu-id="da5bb-131">Versioning</span></span> 
<span data-ttu-id="da5bb-132">hello instans Metadata Service är en ny version.</span><span class="sxs-lookup"><span data-stu-id="da5bb-132">hello Instance Metadata Service is versioned.</span></span> <span data-ttu-id="da5bb-133">Versioner är obligatoriska och hello aktuella versionen är `2017-03-01`.</span><span class="sxs-lookup"><span data-stu-id="da5bb-133">Versions are mandatory and hello current version is `2017-03-01`.</span></span>

> [!NOTE] 
> <span data-ttu-id="da5bb-134">Tidigare förhandsvisningarna av schemalagda händelser stöds {senaste} som hello api-versionen.</span><span class="sxs-lookup"><span data-stu-id="da5bb-134">Previous preview releases of scheduled events supported {latest} as hello api-version.</span></span> <span data-ttu-id="da5bb-135">Det här formatet stöds inte längre och kommer att inaktualiseras i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="da5bb-135">This format is no longer supported and will be deprecated in hello future.</span></span>

### <a name="using-headers"></a><span data-ttu-id="da5bb-136">Med hjälp av rubriker</span><span class="sxs-lookup"><span data-stu-id="da5bb-136">Using headers</span></span>
<span data-ttu-id="da5bb-137">När du frågar hello Metadata Service måste du ange hello huvud `Metadata: true` tooensure hello begäran omdirigerades inte oavsiktligt.</span><span class="sxs-lookup"><span data-stu-id="da5bb-137">When you query hello Metadata Service, you must provide hello header `Metadata: true` tooensure hello request was not unintentionally redirected.</span></span>

### <a name="enabling-scheduled-events"></a><span data-ttu-id="da5bb-138">Aktivera schemalagd händelser</span><span class="sxs-lookup"><span data-stu-id="da5bb-138">Enabling Scheduled Events</span></span>
<span data-ttu-id="da5bb-139">hello aktiverar första gången du skapar en begäran om schemalagda händelser Azure implicit hello funktion på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="da5bb-139">hello first time you make a request for scheduled events, Azure implicitly enables hello feature on your Virtual Machine.</span></span> <span data-ttu-id="da5bb-140">Därför bör du förväntar dig fördröjd svar i din första anropet av in tootwo i minuter.</span><span class="sxs-lookup"><span data-stu-id="da5bb-140">As a result, you should expect a delayed response in your first call of up tootwo minutes.</span></span>

### <a name="user-initiated-maintenance"></a><span data-ttu-id="da5bb-141">Användarinitierad Underhåll</span><span class="sxs-lookup"><span data-stu-id="da5bb-141">User initiated maintenance</span></span>
<span data-ttu-id="da5bb-142">Användaren initierade underhålla virtuella datorer via hello Azure-portalen API, CLI eller PowerShell resulterar i en schemalagd händelse.</span><span class="sxs-lookup"><span data-stu-id="da5bb-142">User initiated virtual machine maintenance via hello Azure portal, API, CLI, or PowerShell results in a scheduled event.</span></span> <span data-ttu-id="da5bb-143">Detta kan tootest hello Underhåll förberedelse logik i ditt program och dina program tooprepare för användarinitierad underhåll.</span><span class="sxs-lookup"><span data-stu-id="da5bb-143">This allows you tootest hello maintenance preparation logic in your application and allows your application tooprepare for user initiated maintenance.</span></span>

<span data-ttu-id="da5bb-144">Starta om en virtuell dator schemalägger en händelse med typen `Reboot`.</span><span class="sxs-lookup"><span data-stu-id="da5bb-144">Restarting a virtual machine schedules an event with type `Reboot`.</span></span> <span data-ttu-id="da5bb-145">Omdistribuera en virtuell dator schemalägger en händelse med typen `Redeploy`.</span><span class="sxs-lookup"><span data-stu-id="da5bb-145">Redeploying a virtual machine schedules an event with type `Redeploy`.</span></span>

> [!NOTE] 
> <span data-ttu-id="da5bb-146">För närvarande kan högst 10 användarinitierad underhållsåtgärder samtidigt schemaläggas.</span><span class="sxs-lookup"><span data-stu-id="da5bb-146">Currently a maximum of 10 user initiated maintenance operations can be simultaneously scheduled.</span></span> <span data-ttu-id="da5bb-147">Den här gränsen mjukas upp före schemalagda händelser allmän tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="da5bb-147">This limit will be relaxed before Scheduled Events general availability.</span></span>

> [!NOTE] 
> <span data-ttu-id="da5bb-148">Användarinitierad Underhåll ledde schemalagda händelser kan för närvarande inte konfigureras.</span><span class="sxs-lookup"><span data-stu-id="da5bb-148">Currently user initiated maintenance resulting in Scheduled Events is not configurable.</span></span> <span data-ttu-id="da5bb-149">Konfigurationsmöjligheter är planerad för framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="da5bb-149">Configurability is planned for a future release.</span></span>

## <a name="using-hello-api"></a><span data-ttu-id="da5bb-150">Med hello-API</span><span class="sxs-lookup"><span data-stu-id="da5bb-150">Using hello API</span></span>

### <a name="query-for-events"></a><span data-ttu-id="da5bb-151">Fråga efter händelser</span><span class="sxs-lookup"><span data-stu-id="da5bb-151">Query for events</span></span>
<span data-ttu-id="da5bb-152">Du kan fråga efter schemalagda händelser genom att göra hello följande anropa:</span><span class="sxs-lookup"><span data-stu-id="da5bb-152">You can query for Scheduled Events simply by making hello following call:</span></span>

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

<span data-ttu-id="da5bb-153">Svaret innehåller en matris med schemalagda händelser.</span><span class="sxs-lookup"><span data-stu-id="da5bb-153">A response contains an array of scheduled events.</span></span> <span data-ttu-id="da5bb-154">En tom matris innebär att det inte finns för närvarande inga händelser som är schemalagda.</span><span class="sxs-lookup"><span data-stu-id="da5bb-154">An empty array means that there are currently no events scheduled.</span></span>
<span data-ttu-id="da5bb-155">I hello fall där det finns schemalagda händelser, hello svaret innehåller en matris med händelser:</span><span class="sxs-lookup"><span data-stu-id="da5bb-155">In hello case where there are scheduled events, hello response contains an array of events:</span></span> 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},              
        }
    ]
}
```

### <a name="event-properties"></a><span data-ttu-id="da5bb-156">Egenskaper för händelse</span><span class="sxs-lookup"><span data-stu-id="da5bb-156">Event properties</span></span>
|<span data-ttu-id="da5bb-157">Egenskap</span><span class="sxs-lookup"><span data-stu-id="da5bb-157">Property</span></span>  |  <span data-ttu-id="da5bb-158">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="da5bb-158">Description</span></span> |
| - | - |
| <span data-ttu-id="da5bb-159">Händelse-ID</span><span class="sxs-lookup"><span data-stu-id="da5bb-159">EventId</span></span> | <span data-ttu-id="da5bb-160">Globalt unik identifierare för den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="da5bb-160">Globally unique identifier for this event.</span></span> <br><br> <span data-ttu-id="da5bb-161">Exempel:</span><span class="sxs-lookup"><span data-stu-id="da5bb-161">Example:</span></span> <br><ul><li><span data-ttu-id="da5bb-162">602d9444-d2cd-49c7-8624-8643e7171297</span><span class="sxs-lookup"><span data-stu-id="da5bb-162">602d9444-d2cd-49c7-8624-8643e7171297</span></span>  |
| <span data-ttu-id="da5bb-163">Händelsetyp</span><span class="sxs-lookup"><span data-stu-id="da5bb-163">EventType</span></span> | <span data-ttu-id="da5bb-164">Inverkan som gör att den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="da5bb-164">Impact this event causes.</span></span> <br><br> <span data-ttu-id="da5bb-165">Värden:</span><span class="sxs-lookup"><span data-stu-id="da5bb-165">Values:</span></span> <br><ul><li> <span data-ttu-id="da5bb-166">`Freeze`: hello virtuella datorn är schemalagt toopause för några sekunder.</span><span class="sxs-lookup"><span data-stu-id="da5bb-166">`Freeze`: hello Virtual Machine is scheduled toopause for few seconds.</span></span> <span data-ttu-id="da5bb-167">hello CPU har pausats, men det finns ingen inverkan på minnet, öppna filer eller nätverksanslutningar.</span><span class="sxs-lookup"><span data-stu-id="da5bb-167">hello CPU is suspended, but there is no impact on memory, open files, or network connections.</span></span> <li><span data-ttu-id="da5bb-168">`Reboot`: hello virtuella datorn är schemalagt för omstart (icke-beständig minne är förlorade).</span><span class="sxs-lookup"><span data-stu-id="da5bb-168">`Reboot`: hello Virtual Machine is scheduled for reboot (non-persistent memory is lost).</span></span> <li><span data-ttu-id="da5bb-169">`Redeploy`: hello virtuella datorn är schemalagt toomove tooanother nod (tillfälliga diskar går förlorade).</span><span class="sxs-lookup"><span data-stu-id="da5bb-169">`Redeploy`: hello Virtual Machine is scheduled toomove tooanother node (ephemeral disks are lost).</span></span> |
| <span data-ttu-id="da5bb-170">ResourceType</span><span class="sxs-lookup"><span data-stu-id="da5bb-170">ResourceType</span></span> | <span data-ttu-id="da5bb-171">Typ av resurs som påverkar den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="da5bb-171">Type of resource this event impacts.</span></span> <br><br> <span data-ttu-id="da5bb-172">Värden:</span><span class="sxs-lookup"><span data-stu-id="da5bb-172">Values:</span></span> <ul><li>`VirtualMachine`|
| <span data-ttu-id="da5bb-173">Resurser</span><span class="sxs-lookup"><span data-stu-id="da5bb-173">Resources</span></span>| <span data-ttu-id="da5bb-174">Lista över resurser som påverkar den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="da5bb-174">List of resources this event impacts.</span></span> <span data-ttu-id="da5bb-175">Detta är garanterat toocontain datorer från högst ett [Uppdateringsdomän](manage-availability.md), men får inte innehålla alla datorer i hello UD.</span><span class="sxs-lookup"><span data-stu-id="da5bb-175">This is guaranteed toocontain machines from at most one [Update Domain](manage-availability.md), but may not contain all machines in hello UD.</span></span> <br><br> <span data-ttu-id="da5bb-176">Exempel:</span><span class="sxs-lookup"><span data-stu-id="da5bb-176">Example:</span></span> <br><ul><li> <span data-ttu-id="da5bb-177">[”FrontEnd_IN_0”, ”BackEnd_IN_0”]</span><span class="sxs-lookup"><span data-stu-id="da5bb-177">["FrontEnd_IN_0", "BackEnd_IN_0"]</span></span> |
| <span data-ttu-id="da5bb-178">Händelsestatus</span><span class="sxs-lookup"><span data-stu-id="da5bb-178">Event Status</span></span> | <span data-ttu-id="da5bb-179">Status för den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="da5bb-179">Status of this event.</span></span> <br><br> <span data-ttu-id="da5bb-180">Värden:</span><span class="sxs-lookup"><span data-stu-id="da5bb-180">Values:</span></span> <ul><li><span data-ttu-id="da5bb-181">`Scheduled`: Den här händelsen är schemalagda toostart efter hello tid som anges i hello `NotBefore` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="da5bb-181">`Scheduled`: This event is scheduled toostart after hello time specified in hello `NotBefore` property.</span></span><li><span data-ttu-id="da5bb-182">`Started`: Den här händelsen har startats.</span><span class="sxs-lookup"><span data-stu-id="da5bb-182">`Started`: This event has started.</span></span></ul> <span data-ttu-id="da5bb-183">Inte `Completed` eller liknande status tillhandahålls någonsin; hello händelsen inte längre kommer att returneras när hello händelse har slutförts.</span><span class="sxs-lookup"><span data-stu-id="da5bb-183">No `Completed` or similar status is ever provided; hello event will no longer be returned when hello event is completed.</span></span>
| <span data-ttu-id="da5bb-184">Inte före</span><span class="sxs-lookup"><span data-stu-id="da5bb-184">NotBefore</span></span>| <span data-ttu-id="da5bb-185">Tid som den här händelsen kan starta.</span><span class="sxs-lookup"><span data-stu-id="da5bb-185">Time after which this event may start.</span></span> <br><br> <span data-ttu-id="da5bb-186">Exempel:</span><span class="sxs-lookup"><span data-stu-id="da5bb-186">Example:</span></span> <br><ul><li> <span data-ttu-id="da5bb-187">2016-09-19T18:29:47Z</span><span class="sxs-lookup"><span data-stu-id="da5bb-187">2016-09-19T18:29:47Z</span></span>  |

### <a name="event-scheduling"></a><span data-ttu-id="da5bb-188">Schemaläggning av händelse</span><span class="sxs-lookup"><span data-stu-id="da5bb-188">Event scheduling</span></span>
<span data-ttu-id="da5bb-189">Varje händelse är schemalagd kortaste tid i hello framtida baserat på händelsetyp.</span><span class="sxs-lookup"><span data-stu-id="da5bb-189">Each event is scheduled a minimum amount of time in hello future based on event type.</span></span> <span data-ttu-id="da5bb-190">Nu visas i en händelse `NotBefore` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="da5bb-190">This time is reflected in an event's `NotBefore` property.</span></span> 

|<span data-ttu-id="da5bb-191">Händelsetyp</span><span class="sxs-lookup"><span data-stu-id="da5bb-191">EventType</span></span>  | <span data-ttu-id="da5bb-192">Minsta meddelande</span><span class="sxs-lookup"><span data-stu-id="da5bb-192">Minimum Notice</span></span> |
| - | - |
| <span data-ttu-id="da5bb-193">Lås</span><span class="sxs-lookup"><span data-stu-id="da5bb-193">Freeze</span></span>| <span data-ttu-id="da5bb-194">15 minuter</span><span class="sxs-lookup"><span data-stu-id="da5bb-194">15 minutes</span></span> |
| <span data-ttu-id="da5bb-195">Starta om</span><span class="sxs-lookup"><span data-stu-id="da5bb-195">Reboot</span></span> | <span data-ttu-id="da5bb-196">15 minuter</span><span class="sxs-lookup"><span data-stu-id="da5bb-196">15 minutes</span></span> |
| <span data-ttu-id="da5bb-197">Omdistribuera</span><span class="sxs-lookup"><span data-stu-id="da5bb-197">Redeploy</span></span> | <span data-ttu-id="da5bb-198">10 minuter</span><span class="sxs-lookup"><span data-stu-id="da5bb-198">10 minutes</span></span> |

### <a name="starting-an-event"></a><span data-ttu-id="da5bb-199">Starta en händelse</span><span class="sxs-lookup"><span data-stu-id="da5bb-199">Starting an event</span></span> 

<span data-ttu-id="da5bb-200">När du har lärt dig i en kommande händelse och slutföra din logik för att korrekt avslutning, kan du godkänna hello utestående händelsen genom att göra en `POST` anropa toohello metadatatjänsten med hello `EventId`.</span><span class="sxs-lookup"><span data-stu-id="da5bb-200">Once you have learned of an upcoming event and completed your logic for graceful shutdown, you can approve hello outstanding event by making a `POST` call toohello metadata service with hello `EventId`.</span></span> <span data-ttu-id="da5bb-201">Detta anger att den korta minsta hello-meddelande tooAzure (när det är möjligt).</span><span class="sxs-lookup"><span data-stu-id="da5bb-201">This indicates tooAzure that it can shorten hello minimum notification time (when possible).</span></span> 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> <span data-ttu-id="da5bb-202">Bekräfta en händelse kan hello händelse tooproceed för alla `Resources` i hello-händelse, inte bara hello virtuella om hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="da5bb-202">Acknowledging an event allows hello event tooproceed for all `Resources` in hello event, not just hello virtual machine that acknowledges hello event.</span></span> <span data-ttu-id="da5bb-203">Därför du tooelect en ledande toocoordinate hello bekräftelse, vilket kan vara så enkelt som hello första dator i hello `Resources` fältet.</span><span class="sxs-lookup"><span data-stu-id="da5bb-203">You may therefore choose tooelect a leader toocoordinate hello acknowledgement, which may be as simple as hello first machine in hello `Resources` field.</span></span>


## <a name="powershell-sample"></a><span data-ttu-id="da5bb-204">PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="da5bb-204">PowerShell sample</span></span> 

<span data-ttu-id="da5bb-205">hello följande exempelfrågor hello metadatatjänsten för schemalagda händelser och godkänner alla utestående händelser.</span><span class="sxs-lookup"><span data-stu-id="da5bb-205">hello following sample queries hello metadata service for scheduled events and approves each outstanding event.</span></span>

```PowerShell
# How tooget scheduled events 
function GetScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How tooapprove a scheduled event
function ApproveScheduledEvent($eventId, $docIncarnation, $uri)
{    
    # Create hello Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests; "DocumentIncarnation" = $docIncarnation} 
    
    # Convert tooJSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with hello following: `n" $approvalString

    # Post approval string tooscheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function HandleScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up hello scheduled events URI for a VNET-enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-03-01' -f $localHostIP 

# Get events
$scheduledEvents = GetScheduledEvents $scheduledEventURI

# Handle events however is best for your service
HandleScheduledEvents $scheduledEvents

# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
        ApproveScheduledEvent $event.EventId $scheduledEvents.DocumentIncarnation $scheduledEventURI 
    }
}
``` 


## <a name="c-sample"></a><span data-ttu-id="da5bb-206">C\# exempel</span><span class="sxs-lookup"><span data-stu-id="da5bb-206">C\# sample</span></span> 

<span data-ttu-id="da5bb-207">hello är följande exempel en enkel klient som kommunicerar med hello metadata-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="da5bb-207">hello following sample is of a simple client that communicates with hello metadata service.</span></span>

```csharp
public class ScheduledEventsClient
{
    private readonly string scheduledEventsEndpoint;
    private readonly string defaultIpAddress = "169.254.169.254"; 

    // Set up hello scheduled events URI for a VNET-enabled VM
    public ScheduledEventsClient()
    {
        scheduledEventsEndpoint = string.Format("http://{0}/metadata/scheduledevents?api-version=2017-03-01", defaultIpAddress);
    }

    // Get events
    public string GetScheduledEvents()
    {
        Uri cloudControlUri = new Uri(scheduledEventsEndpoint);
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Metadata", "true");
            return webClient.DownloadString(cloudControlUri);
        }   
    }

    // Approve events
    public void ApproveScheduledEvents(string jsonPost)
    {
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Content-Type", "application/json");
            webClient.UploadString(scheduledEventsEndpoint, jsonPost);
        }
    }
}
```

<span data-ttu-id="da5bb-208">Schemalagda händelser kan representeras med hjälp av följande datastrukturer hello:</span><span class="sxs-lookup"><span data-stu-id="da5bb-208">Scheduled Events can be represented using hello following data structures:</span></span>

```csharp
public class ScheduledEventsDocument
{
    public string DocumentIncarnation;
    public List<CloudControlEvent> Events { get; set; }
}

public class CloudControlEvent
{
    public string EventId { get; set; }
    public string EventStatus { get; set; }
    public string EventType { get; set; }
    public string ResourceType { get; set; }
    public List<string> Resources { get; set; }
    public DateTime? NotBefore { get; set; }
}

public class ScheduledEventsApproval
{
    public string DocumentIncarnation;
    public List<StartRequest> StartRequests = new List<StartRequest>();
}

public class StartRequest
{
    [JsonProperty("EventId")]
    private string eventId;

    public StartRequest(string eventId)
    {
        this.eventId = eventId;
    }
}
```

<span data-ttu-id="da5bb-209">hello följande exempelfrågor hello metadatatjänsten för schemalagda händelser och godkänner alla utestående händelser.</span><span class="sxs-lookup"><span data-stu-id="da5bb-209">hello following sample queries hello metadata service for scheduled events and approves each outstanding event.</span></span>

```csharp
public class Program
{
    static ScheduledEventsClient client;

    static void Main(string[] args)
    {
        client = new ScheduledEventsClient();

        while (true)
        {
            string json = client.GetDocument();
            ScheduledEventsDocument scheduledEventsDocument = JsonConvert.DeserializeObject<ScheduledEventsDocument>(json);

            HandleEvents(scheduledEventsDocument.Events);

            // Wait for user response
            Console.WriteLine("Press Enter tooapprove executing events\n");
            Console.ReadLine();

            // Approve events
            ScheduledEventsApproval scheduledEventsApprovalDocument = new ScheduledEventsApproval()
            {
                DocumentIncarnation = scheduledEventsDocument.DocumentIncarnation
            };
        
            foreach (CloudControlEvent event in scheduledEventsDocument.Events)
            {
                scheduledEventsApprovalDocument.StartRequests.Add(new StartRequest(event.EventId));
            }

            if (scheduledEventsApprovalDocument.StartRequests.Count > 0)
            {
                // Serialize using Newtonsoft.Json
                string approveEventsJsonDocument =
                    JsonConvert.SerializeObject(scheduledEventsApprovalDocument);

                Console.WriteLine($"Approving events with json: {approveEventsJsonDocument}\n");
                client.ApproveScheduledEvents(approveEventsJsonDocument);
            }

            Console.WriteLine("Complete. Press enter toorepeat\n\n");
            Console.ReadLine();
            Console.Clear();
        }
    }

    private static void HandleEvents(List<CloudControlEvent> events)
    {
        // Add logic for handling events here
    }
}
```

## <a name="python-sample"></a><span data-ttu-id="da5bb-210">Python-exempel</span><span class="sxs-lookup"><span data-stu-id="da5bb-210">Python sample</span></span> 

<span data-ttu-id="da5bb-211">hello följande exempelfrågor hello metadatatjänsten för schemalagda händelser och godkänner alla utestående händelser.</span><span class="sxs-lookup"><span data-stu-id="da5bb-211">hello following sample queries hello metadata service for scheduled events and approves each outstanding event.</span></span>

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01"
headers = "{Metadata:true}"
this_host = socket.gethostname()

def get_scheduled_events():
   req = urllib2.Request(metadata_url)
   req.add_header('Metadata', 'true')
   resp = urllib2.urlopen(req)
   data = json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ","_")
        if this_host in resources:
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            # Add logic for handling events here

def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)
   
if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a><span data-ttu-id="da5bb-212">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da5bb-212">Next steps</span></span> 

- <span data-ttu-id="da5bb-213">Läs mer om hello API: er som är tillgängliga i hello [instans Metadata tjänsten](instance-metadata-service.md).</span><span class="sxs-lookup"><span data-stu-id="da5bb-213">Read more about hello APIs available in hello [Instance Metadata service](instance-metadata-service.md).</span></span>
- <span data-ttu-id="da5bb-214">Lär dig mer om [planerat underhåll för Windows-datorer i Azure](planned-maintenance.md).</span><span class="sxs-lookup"><span data-stu-id="da5bb-214">Learn about [planned maintenance for Windows virtual machines in Azure](planned-maintenance.md).</span></span>

