---
title: "Schemalagda händelser för Linux virtuella datorer i Azure | Microsoft Docs"
description: "Schemalagda händelser som använder tjänsten Azure Metadata för på din virtuella Linux-datorer."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: zivr
ms.openlocfilehash: 75e509a7e39f5b268ce550d0c4dea2261d4fe56f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-metadata-service-scheduled-events-preview-for-linux-vms"></a><span data-ttu-id="fda39-103">Metadata Azure: Schemalagda händelser (förhandsversion) för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="fda39-103">Azure Metadata Service: Scheduled Events (Preview) for Linux VMs</span></span>

> [!NOTE] 
> <span data-ttu-id="fda39-104">Förhandsgranskningar görs tillgängliga för dig under förutsättning att du godkänner användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="fda39-104">Previews are made available to you on the condition that you agree to the terms of use.</span></span> <span data-ttu-id="fda39-105">Mer information finns i [de kompletterande villkoren för användning av Microsoft Azure-förhandsversioner](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).</span><span class="sxs-lookup"><span data-stu-id="fda39-105">For more information, see [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).</span></span>
>

<span data-ttu-id="fda39-106">Schemalagda händelser är en av subservices under tjänsten Azure Metadata.</span><span class="sxs-lookup"><span data-stu-id="fda39-106">Scheduled Events is one of the subservices under the Azure Metadata Service.</span></span> <span data-ttu-id="fda39-107">Ansvarar för att visa information om kommande händelser (till exempel omstart) så att programmet kan förbereda för dem och begränsa avbrott.</span><span class="sxs-lookup"><span data-stu-id="fda39-107">It is responsible for surfacing information regarding upcoming events (for example, reboot) so your application can prepare for them and limit disruption.</span></span> <span data-ttu-id="fda39-108">Den är tillgänglig för alla typer av Azure virtuella datorer inklusive PaaS och IaaS.</span><span class="sxs-lookup"><span data-stu-id="fda39-108">It is available for all Azure Virtual Machine types including PaaS and IaaS.</span></span> <span data-ttu-id="fda39-109">Schemalagda händelser ger din virtuella tid att utföra förebyggande åtgärder för att minimera effekten av en händelse.</span><span class="sxs-lookup"><span data-stu-id="fda39-109">Scheduled Events gives your Virtual Machine time to perform preventive tasks to minimize the effect of an event.</span></span> 

<span data-ttu-id="fda39-110">Schemalagda händelser är tillgänglig för både Windows- och Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="fda39-110">Scheduled Events is available for both Windows and Linux VMs.</span></span> <span data-ttu-id="fda39-111">Information om schemalagda händelser i Windows finns i [schemalagda händelser för virtuella Windows-datorer](../windows/scheduled-events.md).</span><span class="sxs-lookup"><span data-stu-id="fda39-111">For information about Scheduled Events on Windows, see [Scheduled Events for Windows VMs](../windows/scheduled-events.md).</span></span>

## <a name="why-scheduled-events"></a><span data-ttu-id="fda39-112">Varför schemalagda händelser?</span><span class="sxs-lookup"><span data-stu-id="fda39-112">Why Scheduled Events?</span></span>

<span data-ttu-id="fda39-113">Du kan vidta åtgärder för att begränsa effekten av plattform intiated underhåll eller användare utför åtgärder på din tjänst med schemalagda händelser.</span><span class="sxs-lookup"><span data-stu-id="fda39-113">With Scheduled Events, you can take steps to limit the impact of platform-intiated maintenance or user-initiated actions on your service.</span></span> 

<span data-ttu-id="fda39-114">Flera instanser arbetsbelastningar, som använder replikeringstekniker för för att upprätthålla tillstånd, kan vara utsatt för avbrott som händer i flera instanser.</span><span class="sxs-lookup"><span data-stu-id="fda39-114">Multi-instance workloads, which use replication techniques to maintain state, may be vulnerable to outages happening across multiple instances.</span></span> <span data-ttu-id="fda39-115">Exempel driftsavbrott kan resultera i dyra aktiviteter (till exempel återskapande index) eller även en replik går förlorade.</span><span class="sxs-lookup"><span data-stu-id="fda39-115">Such outages may result in expensive tasks (for example, rebuilding indexes) or even a replica loss.</span></span> 

<span data-ttu-id="fda39-116">I många andra fall totala tjänsttillgängligheten kan förbättras genom att utföra en korrekt avslutningssekvens som Slutför (eller annullerar) pågående transaktioner, tilldela uppgifter till andra virtuella datorer i klustret (manuell redundans) eller ta bort virtuellt Datorn från poolen network load belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="fda39-116">In many other cases, the overall service availability may be improved by performing a graceful shutdown sequence such as completing (or canceling) in-flight transactions, reassigning tasks to other VMs in the cluster (manual failover), or removing the Virtual Machine from a network load balancer pool.</span></span> 

<span data-ttu-id="fda39-117">Finns det fall där tala med administratören om en kommande händelse eller logga sådan händelse att förbättra brukbarheten av program finns i molnet.</span><span class="sxs-lookup"><span data-stu-id="fda39-117">There are cases where notifying an administrator about an upcoming event or logging such an event help improving the serviceability of applications hosted in the cloud.</span></span>

<span data-ttu-id="fda39-118">Azure Metadata Service hämtar schemalagda händelser i följande användningsområden:</span><span class="sxs-lookup"><span data-stu-id="fda39-118">Azure Metadata Service surfaces Scheduled Events in the following use cases:</span></span>
-   <span data-ttu-id="fda39-119">Plattform som initierade Underhåll (till exempel värd-OS-installationen)</span><span class="sxs-lookup"><span data-stu-id="fda39-119">Platform initiated maintenance (for example, Host OS rollout)</span></span>
-   <span data-ttu-id="fda39-120">Användarinitierad anrop (till exempel användaren startar om eller återdistribuerar en virtuell dator)</span><span class="sxs-lookup"><span data-stu-id="fda39-120">User-initiated calls (for example, user restarts or redeploys a VM)</span></span>


## <a name="the-basics"></a><span data-ttu-id="fda39-121">Grunderna</span><span class="sxs-lookup"><span data-stu-id="fda39-121">The basics</span></span>  

<span data-ttu-id="fda39-122">Metadata i Azure-tjänsten visar information om att köra virtuella datorer med en REST-slutpunkt som är tillgänglig från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="fda39-122">Azure Metadata service exposes information about running Virtual Machines using a REST Endpoint accessible from within the VM.</span></span> <span data-ttu-id="fda39-123">Informationen är tillgänglig via en icke-dirigerbara IP-adress så att inte exponeras utanför den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="fda39-123">The information is available via a non-routable IP so that it is not exposed outside the VM.</span></span>

### <a name="scope"></a><span data-ttu-id="fda39-124">Omfång</span><span class="sxs-lookup"><span data-stu-id="fda39-124">Scope</span></span>
<span data-ttu-id="fda39-125">Schemalagda händelser är anslutna till alla virtuella datorer i en molnbaserad tjänst eller till alla virtuella datorer i en Tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="fda39-125">Scheduled events are surfaced to all Virtual Machines in a cloud service or to all Virtual Machines in an Availability Set.</span></span> <span data-ttu-id="fda39-126">Därför bör du kontrollera den `Resources` i händelsen för att identifiera vilka virtuella datorer kommer att påverkas.</span><span class="sxs-lookup"><span data-stu-id="fda39-126">As a result, you should check the `Resources` field in the event to identify which VMs are going to be impacted.</span></span> 

### <a name="discovering-the-endpoint"></a><span data-ttu-id="fda39-127">Identifiering av slutpunkten</span><span class="sxs-lookup"><span data-stu-id="fda39-127">Discovering the endpoint</span></span>
<span data-ttu-id="fda39-128">I de fall där en virtuell dator skapas ett virtuellt nätverk (VNet), metadatatjänsten är tillgänglig från en statisk icke-dirigerbara IP `169.254.169.254`.</span><span class="sxs-lookup"><span data-stu-id="fda39-128">In the case where a Virtual Machine is created within a Virtual Network (VNet), the metadata service is available from a static non-routable IP, `169.254.169.254`.</span></span>
<span data-ttu-id="fda39-129">Om den virtuella datorn inte har skapat ett virtuellt nätverk, standard-fall för molntjänster och klassiska virtuella datorer, krävs ytterligare logik för att identifiera slutpunkt för att använda.</span><span class="sxs-lookup"><span data-stu-id="fda39-129">If the Virtual Machine is not created within a Virtual Network, the default cases for cloud services and classic VMs, additional logic is required to discover the endpoint to use.</span></span> <span data-ttu-id="fda39-130">Referera till det här exemplet att lära dig hur du [identifiera värden slutpunkt](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span><span class="sxs-lookup"><span data-stu-id="fda39-130">Refer to this sample to learn how to [discover the host endpoint](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span></span>

### <a name="versioning"></a><span data-ttu-id="fda39-131">Versionshantering</span><span class="sxs-lookup"><span data-stu-id="fda39-131">Versioning</span></span> 
<span data-ttu-id="fda39-132">Tjänsten instans Metadata är en ny version.</span><span class="sxs-lookup"><span data-stu-id="fda39-132">The Instance Metadata Service is versioned.</span></span> <span data-ttu-id="fda39-133">Versioner är obligatoriska och den aktuella versionen är `2017-03-01`.</span><span class="sxs-lookup"><span data-stu-id="fda39-133">Versions are mandatory and the current version is `2017-03-01`.</span></span>

> [!NOTE] 
> <span data-ttu-id="fda39-134">Tidigare förhandsvisningarna av schemalagda händelser stöds {senaste} som den api-versionen.</span><span class="sxs-lookup"><span data-stu-id="fda39-134">Previous preview releases of scheduled events supported {latest} as the api-version.</span></span> <span data-ttu-id="fda39-135">Det här formatet stöds inte längre och kommer att inaktualiseras i framtiden.</span><span class="sxs-lookup"><span data-stu-id="fda39-135">This format is no longer supported and will be deprecated in the future.</span></span>

### <a name="using-headers"></a><span data-ttu-id="fda39-136">Med hjälp av rubriker</span><span class="sxs-lookup"><span data-stu-id="fda39-136">Using headers</span></span>
<span data-ttu-id="fda39-137">När du frågar Metadata Service måste du ange rubriken `Metadata: true` så begäran inte omdirigerades oavsiktligt.</span><span class="sxs-lookup"><span data-stu-id="fda39-137">When you query the Metadata Service, you must provide the header `Metadata: true` to ensure the request was not unintentionally redirected.</span></span>

### <a name="enabling-scheduled-events"></a><span data-ttu-id="fda39-138">Aktivera schemalagd händelser</span><span class="sxs-lookup"><span data-stu-id="fda39-138">Enabling Scheduled Events</span></span>
<span data-ttu-id="fda39-139">Första gången du skapar en begäran om schemalagda händelser aktiverar Azure implicit funktionen på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="fda39-139">The first time you make a request for scheduled events, Azure implicitly enables the feature on your Virtual Machine.</span></span> <span data-ttu-id="fda39-140">Därför bör du förväntar dig en fördröjd svar i din första anropet av upp till två minuter.</span><span class="sxs-lookup"><span data-stu-id="fda39-140">As a result, you should expect a delayed response in your first call of up to two minutes.</span></span>

### <a name="user-initiated-maintenance"></a><span data-ttu-id="fda39-141">Användarinitierad Underhåll</span><span class="sxs-lookup"><span data-stu-id="fda39-141">User initiated maintenance</span></span>
<span data-ttu-id="fda39-142">Användaren initierade underhålla virtuella datorer via Azure portal, API, CLI eller PowerShell resulterar i en schemalagd händelse.</span><span class="sxs-lookup"><span data-stu-id="fda39-142">User initiated virtual machine maintenance via the Azure portal, API, CLI, or PowerShell results in a scheduled event.</span></span> <span data-ttu-id="fda39-143">Det kan du testa Underhåll förberedelse av logiken i ditt program och att ditt program att förbereda för användarinitierad underhåll.</span><span class="sxs-lookup"><span data-stu-id="fda39-143">This allows you to test the maintenance preparation logic in your application and allows your application to prepare for user initiated maintenance.</span></span>

<span data-ttu-id="fda39-144">Starta om en virtuell dator schemalägger en händelse med typen `Reboot`.</span><span class="sxs-lookup"><span data-stu-id="fda39-144">Restarting a virtual machine schedules an event with type `Reboot`.</span></span> <span data-ttu-id="fda39-145">Omdistribuera en virtuell dator schemalägger en händelse med typen `Redeploy`.</span><span class="sxs-lookup"><span data-stu-id="fda39-145">Redeploying a virtual machine schedules an event with type `Redeploy`.</span></span>

> [!NOTE] 
> <span data-ttu-id="fda39-146">För närvarande kan högst 10 användarinitierad underhållsåtgärder samtidigt schemaläggas.</span><span class="sxs-lookup"><span data-stu-id="fda39-146">Currently a maximum of 10 user initiated maintenance operations can be simultaneously scheduled.</span></span> <span data-ttu-id="fda39-147">Den här gränsen mjukas upp före schemalagda händelser allmän tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="fda39-147">This limit will be relaxed before Scheduled Events general availability.</span></span>

> [!NOTE] 
> <span data-ttu-id="fda39-148">Användarinitierad Underhåll ledde schemalagda händelser kan för närvarande inte konfigureras.</span><span class="sxs-lookup"><span data-stu-id="fda39-148">Currently user initiated maintenance resulting in Scheduled Events is not configurable.</span></span> <span data-ttu-id="fda39-149">Konfigurationsmöjligheter är planerad för framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="fda39-149">Configurability is planned for a future release.</span></span>

## <a name="using-the-api"></a><span data-ttu-id="fda39-150">Med hjälp av API</span><span class="sxs-lookup"><span data-stu-id="fda39-150">Using the API</span></span>

### <a name="query-for-events"></a><span data-ttu-id="fda39-151">Fråga efter händelser</span><span class="sxs-lookup"><span data-stu-id="fda39-151">Query for events</span></span>
<span data-ttu-id="fda39-152">Du kan fråga efter schemalagda händelser genom att göra följande anrop:</span><span class="sxs-lookup"><span data-stu-id="fda39-152">You can query for Scheduled Events simply by making the following call:</span></span>

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

<span data-ttu-id="fda39-153">Svaret innehåller en matris med schemalagda händelser.</span><span class="sxs-lookup"><span data-stu-id="fda39-153">A response contains an array of scheduled events.</span></span> <span data-ttu-id="fda39-154">En tom matris innebär att det inte finns för närvarande inga händelser som är schemalagda.</span><span class="sxs-lookup"><span data-stu-id="fda39-154">An empty array means that there are currently no events scheduled.</span></span>
<span data-ttu-id="fda39-155">I fall där det finns schemalagda händelser, svaret innehåller en matris med händelser:</span><span class="sxs-lookup"><span data-stu-id="fda39-155">In the case where there are scheduled events, the response contains an array of events:</span></span> 
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

### <a name="event-properties"></a><span data-ttu-id="fda39-156">Egenskaper för händelse</span><span class="sxs-lookup"><span data-stu-id="fda39-156">Event properties</span></span>
|<span data-ttu-id="fda39-157">Egenskap</span><span class="sxs-lookup"><span data-stu-id="fda39-157">Property</span></span>  |  <span data-ttu-id="fda39-158">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fda39-158">Description</span></span> |
| - | - |
| <span data-ttu-id="fda39-159">Händelse-ID</span><span class="sxs-lookup"><span data-stu-id="fda39-159">EventId</span></span> | <span data-ttu-id="fda39-160">Globalt unik identifierare för den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="fda39-160">Globally unique identifier for this event.</span></span> <br><br> <span data-ttu-id="fda39-161">Exempel:</span><span class="sxs-lookup"><span data-stu-id="fda39-161">Example:</span></span> <br><ul><li><span data-ttu-id="fda39-162">602d9444-d2cd-49c7-8624-8643e7171297</span><span class="sxs-lookup"><span data-stu-id="fda39-162">602d9444-d2cd-49c7-8624-8643e7171297</span></span>  |
| <span data-ttu-id="fda39-163">Händelsetyp</span><span class="sxs-lookup"><span data-stu-id="fda39-163">EventType</span></span> | <span data-ttu-id="fda39-164">Inverkan som gör att den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="fda39-164">Impact this event causes.</span></span> <br><br> <span data-ttu-id="fda39-165">Värden:</span><span class="sxs-lookup"><span data-stu-id="fda39-165">Values:</span></span> <br><ul><li> <span data-ttu-id="fda39-166">`Freeze`: Den virtuella datorn kommer att pausa under några sekunder.</span><span class="sxs-lookup"><span data-stu-id="fda39-166">`Freeze`: The Virtual Machine is scheduled to pause for few seconds.</span></span> <span data-ttu-id="fda39-167">Processorn har pausats, men det finns ingen inverkan på minnet, öppna filer eller nätverksanslutningar.</span><span class="sxs-lookup"><span data-stu-id="fda39-167">The CPU is suspended, but there is no impact on memory, open files, or network connections.</span></span> <li><span data-ttu-id="fda39-168">`Reboot`: Den virtuella datorn är schemalagt för omstart (icke-beständig minne är förlorade).</span><span class="sxs-lookup"><span data-stu-id="fda39-168">`Reboot`: The Virtual Machine is scheduled for reboot (non-persistent memory is lost).</span></span> <li><span data-ttu-id="fda39-169">`Redeploy`: Den virtuella datorn kommer att flytta till en annan nod (tillfälliga diskar går förlorade).</span><span class="sxs-lookup"><span data-stu-id="fda39-169">`Redeploy`: The Virtual Machine is scheduled to move to another node (ephemeral disks are lost).</span></span> |
| <span data-ttu-id="fda39-170">ResourceType</span><span class="sxs-lookup"><span data-stu-id="fda39-170">ResourceType</span></span> | <span data-ttu-id="fda39-171">Typ av resurs som påverkar den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="fda39-171">Type of resource this event impacts.</span></span> <br><br> <span data-ttu-id="fda39-172">Värden:</span><span class="sxs-lookup"><span data-stu-id="fda39-172">Values:</span></span> <ul><li>`VirtualMachine`|
| <span data-ttu-id="fda39-173">Resurser</span><span class="sxs-lookup"><span data-stu-id="fda39-173">Resources</span></span>| <span data-ttu-id="fda39-174">Lista över resurser som påverkar den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="fda39-174">List of resources this event impacts.</span></span> <span data-ttu-id="fda39-175">Detta är säkert att innehålla datorer från högst ett [Uppdateringsdomän](manage-availability.md), men får inte innehålla alla datorer i UD.</span><span class="sxs-lookup"><span data-stu-id="fda39-175">This is guaranteed to contain machines from at most one [Update Domain](manage-availability.md), but may not contain all machines in the UD.</span></span> <br><br> <span data-ttu-id="fda39-176">Exempel:</span><span class="sxs-lookup"><span data-stu-id="fda39-176">Example:</span></span> <br><ul><li> <span data-ttu-id="fda39-177">[”FrontEnd_IN_0”, ”BackEnd_IN_0”]</span><span class="sxs-lookup"><span data-stu-id="fda39-177">["FrontEnd_IN_0", "BackEnd_IN_0"]</span></span> |
| <span data-ttu-id="fda39-178">Händelsestatus</span><span class="sxs-lookup"><span data-stu-id="fda39-178">Event Status</span></span> | <span data-ttu-id="fda39-179">Status för den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="fda39-179">Status of this event.</span></span> <br><br> <span data-ttu-id="fda39-180">Värden:</span><span class="sxs-lookup"><span data-stu-id="fda39-180">Values:</span></span> <ul><li><span data-ttu-id="fda39-181">`Scheduled`: Den här händelsen har schemalagts att starta efter den tid som anges i den `NotBefore` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="fda39-181">`Scheduled`: This event is scheduled to start after the time specified in the `NotBefore` property.</span></span><li><span data-ttu-id="fda39-182">`Started`: Den här händelsen har startats.</span><span class="sxs-lookup"><span data-stu-id="fda39-182">`Started`: This event has started.</span></span></ul> <span data-ttu-id="fda39-183">Inte `Completed` eller liknande status tillhandahålls någonsin; händelsen inte längre kommer att returneras när händelsen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="fda39-183">No `Completed` or similar status is ever provided; the event will no longer be returned when the event is completed.</span></span>
| <span data-ttu-id="fda39-184">Inte före</span><span class="sxs-lookup"><span data-stu-id="fda39-184">NotBefore</span></span>| <span data-ttu-id="fda39-185">Tid som den här händelsen kan starta.</span><span class="sxs-lookup"><span data-stu-id="fda39-185">Time after which this event may start.</span></span> <br><br> <span data-ttu-id="fda39-186">Exempel:</span><span class="sxs-lookup"><span data-stu-id="fda39-186">Example:</span></span> <br><ul><li> <span data-ttu-id="fda39-187">2016-09-19T18:29:47Z</span><span class="sxs-lookup"><span data-stu-id="fda39-187">2016-09-19T18:29:47Z</span></span>  |

### <a name="event-scheduling"></a><span data-ttu-id="fda39-188">Schemaläggning av händelse</span><span class="sxs-lookup"><span data-stu-id="fda39-188">Event scheduling</span></span>
<span data-ttu-id="fda39-189">Varje händelse schemaläggs en minimal mängd tidpunkt i framtiden baserat på händelsetyp.</span><span class="sxs-lookup"><span data-stu-id="fda39-189">Each event is scheduled a minimum amount of time in the future based on event type.</span></span> <span data-ttu-id="fda39-190">Nu visas i en händelse `NotBefore` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="fda39-190">This time is reflected in an event's `NotBefore` property.</span></span> 

|<span data-ttu-id="fda39-191">Händelsetyp</span><span class="sxs-lookup"><span data-stu-id="fda39-191">EventType</span></span>  | <span data-ttu-id="fda39-192">Minsta meddelande</span><span class="sxs-lookup"><span data-stu-id="fda39-192">Minimum Notice</span></span> |
| - | - |
| <span data-ttu-id="fda39-193">Lås</span><span class="sxs-lookup"><span data-stu-id="fda39-193">Freeze</span></span>| <span data-ttu-id="fda39-194">15 minuter</span><span class="sxs-lookup"><span data-stu-id="fda39-194">15 minutes</span></span> |
| <span data-ttu-id="fda39-195">Starta om</span><span class="sxs-lookup"><span data-stu-id="fda39-195">Reboot</span></span> | <span data-ttu-id="fda39-196">15 minuter</span><span class="sxs-lookup"><span data-stu-id="fda39-196">15 minutes</span></span> |
| <span data-ttu-id="fda39-197">Omdistribuera</span><span class="sxs-lookup"><span data-stu-id="fda39-197">Redeploy</span></span> | <span data-ttu-id="fda39-198">10 minuter</span><span class="sxs-lookup"><span data-stu-id="fda39-198">10 minutes</span></span> |

### <a name="starting-an-event"></a><span data-ttu-id="fda39-199">Starta en händelse</span><span class="sxs-lookup"><span data-stu-id="fda39-199">Starting an event</span></span> 

<span data-ttu-id="fda39-200">När du har lärt dig i en kommande händelse och slutföra din logik för att korrekt avslutning, kan du godkänna utestående händelsen genom att göra en `POST` anrop till metadatatjänsten med i `EventId`.</span><span class="sxs-lookup"><span data-stu-id="fda39-200">Once you have learned of an upcoming event and completed your logic for graceful shutdown, you can approve the outstanding event by making a `POST` call to the metadata service with the `EventId`.</span></span> <span data-ttu-id="fda39-201">Detta anger att Azure att det korta meddelandet minsta (när det är möjligt).</span><span class="sxs-lookup"><span data-stu-id="fda39-201">This indicates to Azure that it can shorten the minimum notification time (when possible).</span></span> 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> <span data-ttu-id="fda39-202">Bekräfta en händelse kan händelsen för att fortsätta för alla `Resources` i händelseloggen, inte bara den virtuella datorn som om händelsen.</span><span class="sxs-lookup"><span data-stu-id="fda39-202">Acknowledging an event allows the event to proceed for all `Resources` in the event, not just the virtual machine that acknowledges the event.</span></span> <span data-ttu-id="fda39-203">Därför kan du välja att välja ledande koordinera bekräftelse, vilket kan vara så enkelt som den första datorn i den `Resources` fältet.</span><span class="sxs-lookup"><span data-stu-id="fda39-203">You may therefore choose to elect a leader to coordinate the acknowledgement, which may be as simple as the first machine in the `Resources` field.</span></span>




## <a name="python-sample"></a><span data-ttu-id="fda39-204">Python-exempel</span><span class="sxs-lookup"><span data-stu-id="fda39-204">Python sample</span></span> 

<span data-ttu-id="fda39-205">I följande exempel frågar metadatatjänsten för schemalagda händelser och godkänner alla utestående händelser.</span><span class="sxs-lookup"><span data-stu-id="fda39-205">The following sample queries the metadata service for scheduled events and approves each outstanding event.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="fda39-206">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fda39-206">Next steps</span></span> 

- <span data-ttu-id="fda39-207">Läs mer om API: er finns i den [instans Metadata tjänsten](instance-metadata-service.md).</span><span class="sxs-lookup"><span data-stu-id="fda39-207">Read more about the APIs available in the [Instance Metadata service](instance-metadata-service.md).</span></span>
- <span data-ttu-id="fda39-208">Lär dig mer om [planerat underhåll för Linux virtuella datorer i Azure](planned-maintenance.md).</span><span class="sxs-lookup"><span data-stu-id="fda39-208">Learn about [planned maintenance for Linux virtual machines in Azure](planned-maintenance.md).</span></span>
