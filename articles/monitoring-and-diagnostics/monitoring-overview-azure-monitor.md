---
title: "aaaAzure Monitor: översikt | Microsoft Docs"
description: "Azure övervakaren samlar in statistik för aviseringar, webhooks, Autoskala och automatisering. Artikel också en lista över andra Microsoft-övervakningsalternativ."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: robb
ms.openlocfilehash: ffa304e7b158f0fceb7f60ab88fab291976aa0e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-monitor"></a><span data-ttu-id="5b61f-104">Översikt över Azure Övervakare</span><span class="sxs-lookup"><span data-stu-id="5b61f-104">Overview of Azure Monitor</span></span>
<span data-ttu-id="5b61f-105">Den här artikeln innehåller en översikt över hello Azure-Monitor-tjänsten i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="5b61f-105">This article provides an overview of hello Azure Monitor service in Microsoft Azure.</span></span> <span data-ttu-id="5b61f-106">Det beskriver vad Azure-Monitor har och pekare tooadditional information om hur toouse Azure-Monitor.</span><span class="sxs-lookup"><span data-stu-id="5b61f-106">It discusses what Azure Monitor does and provides pointers tooadditional information on how toouse Azure Monitor.</span></span>  <span data-ttu-id="5b61f-107">Om du föredrar en videointroduktion finns i nästa steg länkarna längst ned hello i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5b61f-107">If you prefer a video introduction, see Next steps links at hello bottom of this article.</span></span> 

## <a name="why-monitor-your-application-or-system"></a><span data-ttu-id="5b61f-108">Varför övervaka programmet eller system</span><span class="sxs-lookup"><span data-stu-id="5b61f-108">Why monitor your application or system</span></span>
<span data-ttu-id="5b61f-109">Molnprogram är komplicerade med många rörliga delar.</span><span class="sxs-lookup"><span data-stu-id="5b61f-109">Cloud applications are complex with many moving parts.</span></span> <span data-ttu-id="5b61f-110">Övervakning innehåller data tooensure tillämpningsprogrammet stanna upp och körs i ett felfritt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="5b61f-110">Monitoring provides data tooensure that your application stays up and running in a healthy state.</span></span> <span data-ttu-id="5b61f-111">Toostave av eventuella problem eller felsöka efter de som hjälper också till.</span><span class="sxs-lookup"><span data-stu-id="5b61f-111">It also helps you toostave off potential problems or troubleshoot past ones.</span></span> <span data-ttu-id="5b61f-112">Du kan dessutom använda övervakning data toogain djupa insikter om ditt program.</span><span class="sxs-lookup"><span data-stu-id="5b61f-112">In addition, you can use monitoring data toogain deep insights about your application.</span></span> <span data-ttu-id="5b61f-113">Denna kunskap kan hjälpa dig att tooimprove programprestanda eller underhålla eller automatisera åtgärder som annars skulle kräva manuella åtgärder.</span><span class="sxs-lookup"><span data-stu-id="5b61f-113">That knowledge can help you tooimprove application performance or maintainability, or automate actions that would otherwise require manual intervention.</span></span>


## <a name="azure-monitor-and-microsofts-other-monitoring-products"></a><span data-ttu-id="5b61f-114">Övervakare för Azure och Microsoft har andra övervakning produkter</span><span class="sxs-lookup"><span data-stu-id="5b61f-114">Azure Monitor and Microsoft's other monitoring products</span></span>
<span data-ttu-id="5b61f-115">Azure-Monitor innehåller basnivån infrastruktur mått och loggar för de flesta tjänster i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="5b61f-115">Azure Monitor provides base level infrastructure metrics and logs for most services in Microsoft Azure.</span></span> <span data-ttu-id="5b61f-116">Azure-tjänster som inte ännu placerar sina data till Azure-Monitor kommer lägga till det i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="5b61f-116">Azure services that do not yet put their data into Azure Monitor will put it there in hello future.</span></span>

<span data-ttu-id="5b61f-117">Microsoft levererar ytterligare produkter och tjänster som erbjuder ytterligare övervakningsfunktioner för utvecklare, DevOps eller IT-Ops som också har lokala installationer.</span><span class="sxs-lookup"><span data-stu-id="5b61f-117">Microsoft ships additional products and services that provide additional monitoring capabilities for developers, DevOps, or IT Ops that also have on-premises installations.</span></span> <span data-ttu-id="5b61f-118">En översikt och förståelse av hur dessa olika produkter och tjänster tillsammans finns [övervakning i Microsoft Azure](monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5b61f-118">For an overview and understanding of how these different products and services work together, see [Monitoring in Microsoft Azure](monitoring-overview.md).</span></span>

## <a name="monitoring-sources---compute"></a><span data-ttu-id="5b61f-119">Övervaka källor - beräkning</span><span class="sxs-lookup"><span data-stu-id="5b61f-119">Monitoring Sources - Compute</span></span>

![Modellen för övervakning och diagnostik för icke-beräkningsresurser](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-compute_v6.png)

<span data-ttu-id="5b61f-121">hello beräknings-tjänster omfattar</span><span class="sxs-lookup"><span data-stu-id="5b61f-121">hello Compute services include</span></span> 
- <span data-ttu-id="5b61f-122">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="5b61f-122">Cloud Services</span></span> 
- <span data-ttu-id="5b61f-123">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="5b61f-123">Virtual Machines</span></span> 
- <span data-ttu-id="5b61f-124">Skaluppsättningar för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="5b61f-124">Virtual Machine scale sets</span></span> 
- <span data-ttu-id="5b61f-125">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="5b61f-125">Service Fabric</span></span>

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a><span data-ttu-id="5b61f-126">Program - diagnostik loggar och programloggarna mått</span><span class="sxs-lookup"><span data-stu-id="5b61f-126">Application - Diagnostics Logs, Application Logs, and Metrics</span></span>
<span data-ttu-id="5b61f-127">Program kan köras ovanpå hello Gästoperativsystem i hello beräknings-modellen.</span><span class="sxs-lookup"><span data-stu-id="5b61f-127">Applications can run on top of hello Guest OS in hello compute model.</span></span> <span data-ttu-id="5b61f-128">De genererar en egen uppsättning loggar och mått.</span><span class="sxs-lookup"><span data-stu-id="5b61f-128">They emit their own set of logs and metrics.</span></span> <span data-ttu-id="5b61f-129">Övervakare för Azure förlitar sig på hello Azure diagnostics tillägg (Windows eller Linux) toocollect de flesta program nivån mått och loggar.</span><span class="sxs-lookup"><span data-stu-id="5b61f-129">Azure Monitor relies on hello Azure diagnostics extension (Windows or Linux) toocollect most application level metrics and logs.</span></span> <span data-ttu-id="5b61f-130">typer av hello</span><span class="sxs-lookup"><span data-stu-id="5b61f-130">hello types include</span></span>

* <span data-ttu-id="5b61f-131">Prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="5b61f-131">Performance counters</span></span>
* <span data-ttu-id="5b61f-132">Programloggar</span><span class="sxs-lookup"><span data-stu-id="5b61f-132">Application Logs</span></span>
* <span data-ttu-id="5b61f-133">Windows-händelseloggar</span><span class="sxs-lookup"><span data-stu-id="5b61f-133">Windows Event Logs</span></span>
* <span data-ttu-id="5b61f-134">Händelsekällan för .NET</span><span class="sxs-lookup"><span data-stu-id="5b61f-134">.NET Event Source</span></span>
* <span data-ttu-id="5b61f-135">IIS-loggar</span><span class="sxs-lookup"><span data-stu-id="5b61f-135">IIS Logs</span></span>
* <span data-ttu-id="5b61f-136">Manifestet baserat ETW</span><span class="sxs-lookup"><span data-stu-id="5b61f-136">Manifest based ETW</span></span>
* <span data-ttu-id="5b61f-137">Krasch minnesdumpar</span><span class="sxs-lookup"><span data-stu-id="5b61f-137">Crash Dumps</span></span>
* <span data-ttu-id="5b61f-138">Kunden felloggar</span><span class="sxs-lookup"><span data-stu-id="5b61f-138">Customer Error Logs</span></span>

<span data-ttu-id="5b61f-139">Utan hello diagnostik tillägg finns bara några mått som CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="5b61f-139">Without hello diagnostics extension, only a few metrics like CPU usage are available.</span></span> 

### <a name="host-and-guest-vm-metrics"></a><span data-ttu-id="5b61f-140">Värd och Gäst-VM mått</span><span class="sxs-lookup"><span data-stu-id="5b61f-140">Host and Guest VM metrics</span></span>
<span data-ttu-id="5b61f-141">hello har ovanstående beräkningsresurser en dedikerad värd virtuell dator och operativsystem som de nyttjar.</span><span class="sxs-lookup"><span data-stu-id="5b61f-141">hello previously listed compute resources have a dedicated host VM and guest OS they interact with.</span></span> <span data-ttu-id="5b61f-142">hello värd virtuell dator och operativsystem är hello motsvarigheten till roten VM och Gäst-VM i hello Hyper-V hypervisor-modellen.</span><span class="sxs-lookup"><span data-stu-id="5b61f-142">hello host VM and guest OS are hello equivalent of root VM and guest VM in hello Hyper-V hypervisor model.</span></span> <span data-ttu-id="5b61f-143">Du kan samla in mått på båda.</span><span class="sxs-lookup"><span data-stu-id="5b61f-143">You can collect metrics on both.</span></span> <span data-ttu-id="5b61f-144">Du kan också samla in diagnostik loggar på hello gästoperativsystemet.</span><span class="sxs-lookup"><span data-stu-id="5b61f-144">You can also collect diagnostics logs on hello guest OS.</span></span>   

### <a name="activity-log"></a><span data-ttu-id="5b61f-145">Aktivitetslogg</span><span class="sxs-lookup"><span data-stu-id="5b61f-145">Activity Log</span></span>
<span data-ttu-id="5b61f-146">Du kan söka hello aktivitetsloggen (tidigare kallade drift- eller granskningsloggar) för information om resurs enligt hello Azure-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="5b61f-146">You can search hello Activity Log (previously called Operational or Audit Logs) for information about your resource as seen by hello Azure infrastructure.</span></span> <span data-ttu-id="5b61f-147">hello-loggen innehåller information såsom tider när resurserna skapas eller förstörs.</span><span class="sxs-lookup"><span data-stu-id="5b61f-147">hello log contains information such as times when resources are created or destroyed.</span></span>  <span data-ttu-id="5b61f-148">Mer information finns i [översikt aktivitetsloggen](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="5b61f-148">For more information, see [Overview of Activity Log](monitoring-overview-activity-logs.md).</span></span> 

## <a name="monitoring-sources---everything-else"></a><span data-ttu-id="5b61f-149">Övervaka källor - allt annat</span><span class="sxs-lookup"><span data-stu-id="5b61f-149">Monitoring Sources - everything else</span></span>

![Modellen för övervakning och diagnostik för beräkningsresurser](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-non-compute_v6.png)


### <a name="resource---metrics-and-diagnostics-logs"></a><span data-ttu-id="5b61f-151">Resurs - mätvärden och diagnostik loggar</span><span class="sxs-lookup"><span data-stu-id="5b61f-151">Resource - Metrics and Diagnostics Logs</span></span>
<span data-ttu-id="5b61f-152">Filer mätvärden och diagnostikfunktionerna loggar variera beroende på hello resurstypen.</span><span class="sxs-lookup"><span data-stu-id="5b61f-152">Collectable metrics and diagnostics logs vary based on hello resource type.</span></span> <span data-ttu-id="5b61f-153">Web Apps innehåller till exempel statistik på hello Disk-i/o och procent CPU.</span><span class="sxs-lookup"><span data-stu-id="5b61f-153">For example, Web Apps provides statistics on hello Disk IO and Percent CPU.</span></span> <span data-ttu-id="5b61f-154">Dessa mått finns inte för en Service Bus-kö som ger mått som kön storlek och meddelandet genomflöde i stället.</span><span class="sxs-lookup"><span data-stu-id="5b61f-154">Those metrics don't exist for a Service Bus queue, which instead provides metrics like queue size and message throughput.</span></span> <span data-ttu-id="5b61f-155">En lista över filer mätvärden för varje resurs som är tillgänglig på [stöds mått](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="5b61f-155">A list of collectable metrics for each resource is available at [supported metrics](monitoring-supported-metrics.md).</span></span> 

### <a name="host-and-guest-vm-metrics"></a><span data-ttu-id="5b61f-156">Värd och Gäst-VM mått</span><span class="sxs-lookup"><span data-stu-id="5b61f-156">Host and Guest VM metrics</span></span>
<span data-ttu-id="5b61f-157">Det är inte nödvändigtvis en 1:1-mappning mellan din resurs och en viss värd och Gäst-VM så mått inte är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="5b61f-157">There is not necessarily a 1:1 mapping between your resource and a particular Host or Guest VM so metrics are not available.</span></span>

### <a name="activity-log"></a><span data-ttu-id="5b61f-158">Aktivitetslogg</span><span class="sxs-lookup"><span data-stu-id="5b61f-158">Activity Log</span></span>
<span data-ttu-id="5b61f-159">hello aktivitetsloggen är hello samma som för beräkningsresurser.</span><span class="sxs-lookup"><span data-stu-id="5b61f-159">hello activity log is hello same as for compute resources.</span></span>  

## <a name="uses-for-monitoring-data"></a><span data-ttu-id="5b61f-160">Används för övervakning av Data</span><span class="sxs-lookup"><span data-stu-id="5b61f-160">Uses for Monitoring Data</span></span>
<span data-ttu-id="5b61f-161">När du samlar in data kan du göra hello följa med i Azure-Monitor</span><span class="sxs-lookup"><span data-stu-id="5b61f-161">Once you collect your data, you can do hello following with it in Azure Monitor</span></span>

### <a name="route"></a><span data-ttu-id="5b61f-162">Routa</span><span class="sxs-lookup"><span data-stu-id="5b61f-162">Route</span></span>
<span data-ttu-id="5b61f-163">Du kan strömma övervakning tooother dataplatserna i realtid.</span><span class="sxs-lookup"><span data-stu-id="5b61f-163">You can stream monitoring data tooother locations in real time.</span></span>

<span data-ttu-id="5b61f-164">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5b61f-164">Examples include:</span></span>

- <span data-ttu-id="5b61f-165">Skicka tooApplication insikter så att du kan använda dess bättre verktyg för visualisering och analys.</span><span class="sxs-lookup"><span data-stu-id="5b61f-165">Send tooApplication Insights so you can use its richer visualization and analysis tools.</span></span>
- <span data-ttu-id="5b61f-166">Skicka tooEvent Hubs så att du kan vidarebefordra toothird parts verktyg.</span><span class="sxs-lookup"><span data-stu-id="5b61f-166">Send tooEvent Hubs so you can route toothird-party tools.</span></span> 

### <a name="store-and-archive"></a><span data-ttu-id="5b61f-167">Store och arkivera</span><span class="sxs-lookup"><span data-stu-id="5b61f-167">Store and Archive</span></span>
<span data-ttu-id="5b61f-168">Vissa övervakningsdata är redan lagrade och är tillgängliga i Azure-Monitor under en tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="5b61f-168">Some monitoring data is already stored and available in Azure Monitor for a set amount of time.</span></span> 
- <span data-ttu-id="5b61f-169">Mått som lagras i 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="5b61f-169">Metrics are stored for 30 days.</span></span> 
- <span data-ttu-id="5b61f-170">Aktiviteten loggposter som lagras i 90 dagar.</span><span class="sxs-lookup"><span data-stu-id="5b61f-170">Activity log entries are stored for 90 days.</span></span> 
- <span data-ttu-id="5b61f-171">Diagnostik loggar lagras inte alls.</span><span class="sxs-lookup"><span data-stu-id="5b61f-171">Diagnostics logs are not stored at all.</span></span> 

<span data-ttu-id="5b61f-172">Om du vill toostore data längre än hello tidsperioder som anges ovan, kan du använda någon Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="5b61f-172">If you want toostore data longer than hello time periods listed above, you can use an Azure storage.</span></span> <span data-ttu-id="5b61f-173">Övervakningsdata sparas i din lagring för baserat på en bevarandeprincip som du anger.</span><span class="sxs-lookup"><span data-stu-id="5b61f-173">Monitoring data is kept in your storage acccount based on a retention policy you set.</span></span> <span data-ttu-id="5b61f-174">Du har toopay för hello utrymme hello data tar upp i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="5b61f-174">You do have toopay for hello space hello data takes up in Azure storage.</span></span> 

<span data-ttu-id="5b61f-175">Några sätt toouse informationen:</span><span class="sxs-lookup"><span data-stu-id="5b61f-175">A few ways toouse this data:</span></span>

- <span data-ttu-id="5b61f-176">Du kan ha andra verktyg inom eller utanför Azure läsa den och bearbetar den väl har skrivits.</span><span class="sxs-lookup"><span data-stu-id="5b61f-176">Once written, you can have other tools within or outside of Azure read it and process it.</span></span>
- <span data-ttu-id="5b61f-177">Du hämtar hello data lokalt för ett lokalt arkiv eller ändra din bevarandeprincip i hello tookeep molndata för längre tid.</span><span class="sxs-lookup"><span data-stu-id="5b61f-177">You download hello data locally for a local archive or change your retention policy in hello cloud tookeep data for extended periods of time.</span></span>  
- <span data-ttu-id="5b61f-178">Du kan lämna hello data i Azure-lagring på obestämd tid för.</span><span class="sxs-lookup"><span data-stu-id="5b61f-178">You leave hello data in Azure storage indefinitely for archive purposes.</span></span> 

### <a name="query"></a><span data-ttu-id="5b61f-179">Fråga</span><span class="sxs-lookup"><span data-stu-id="5b61f-179">Query</span></span>
<span data-ttu-id="5b61f-180">Du kan använda hello Azure övervakaren REST API, plattformsoberoende kommandoradsgränssnittet (CLI)-kommandon, PowerShell-cmdlets eller hello .NET SDK tooaccess hello data i hello system eller Azure storage</span><span class="sxs-lookup"><span data-stu-id="5b61f-180">You can use hello Azure Monitor REST API, cross platform Command-Line Interface (CLI) commands, PowerShell cmdlets, or hello .NET SDK tooaccess hello data in hello system or Azure storage</span></span>

<span data-ttu-id="5b61f-181">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5b61f-181">Examples include:</span></span>

* <span data-ttu-id="5b61f-182">Hämta data för ett anpassat övervakning program som du har skrivit</span><span class="sxs-lookup"><span data-stu-id="5b61f-182">Getting data for a custom monitoring application you have written</span></span>
* <span data-ttu-id="5b61f-183">Skapa egna frågor och skicka data tooa tredjeparts-programmet.</span><span class="sxs-lookup"><span data-stu-id="5b61f-183">Creating custom queries and sending that data tooa third-party application.</span></span>

### <a name="visualize"></a><span data-ttu-id="5b61f-184">Visualisera</span><span class="sxs-lookup"><span data-stu-id="5b61f-184">Visualize</span></span>
<span data-ttu-id="5b61f-185">Visualisera dina övervakningsdata i grafik och diagram hjälper dig att hitta trender snabbare än att slå via hello data.</span><span class="sxs-lookup"><span data-stu-id="5b61f-185">Visualizing your monitoring data in graphics and charts helps you find trends quicker than looking through hello data itself.</span></span>  

<span data-ttu-id="5b61f-186">Några visualiseringen metoderna är:</span><span class="sxs-lookup"><span data-stu-id="5b61f-186">A few visualization methods include:</span></span>

* <span data-ttu-id="5b61f-187">Använd hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5b61f-187">Use hello Azure portal</span></span>
* <span data-ttu-id="5b61f-188">Vidarebefordra data tooAzure Application Insights</span><span class="sxs-lookup"><span data-stu-id="5b61f-188">Route data tooAzure Application Insights</span></span>
* <span data-ttu-id="5b61f-189">Vidarebefordra data tooMicrosoft PowerBI</span><span class="sxs-lookup"><span data-stu-id="5b61f-189">Route data tooMicrosoft PowerBI</span></span>
* <span data-ttu-id="5b61f-190">Väg hello data tooa från tredje part visualiseringen verktyget med hjälp av antingen live streaming eller genom att läsa hello verktyget från ett Arkiv i Azure-lagring</span><span class="sxs-lookup"><span data-stu-id="5b61f-190">Route hello data tooa third-party visualization tool using either live streaming or by having hello tool read from an archive in Azure storage</span></span>


### <a name="automate"></a><span data-ttu-id="5b61f-191">Automatisera</span><span class="sxs-lookup"><span data-stu-id="5b61f-191">Automate</span></span>
<span data-ttu-id="5b61f-192">Du kan använda data tootrigger övervakningsaviseringar eller hela processer.</span><span class="sxs-lookup"><span data-stu-id="5b61f-192">You can use monitoring data tootrigger alerts or even whole processes.</span></span> <span data-ttu-id="5b61f-193">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5b61f-193">Examples include:</span></span>

* <span data-ttu-id="5b61f-194">Använd data tooautoscale compute-instanser uppåt eller nedåt utifrån belastningen för programmet.</span><span class="sxs-lookup"><span data-stu-id="5b61f-194">Use data tooautoscale compute instances up or down based on application load.</span></span>
* <span data-ttu-id="5b61f-195">Skicka e-postmeddelanden när ett mått korsar ett förutbestämt tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="5b61f-195">Send emails when a metric crosses a predetermined threshold.</span></span>
* <span data-ttu-id="5b61f-196">Anropa en webbtjänst-URL (webhook) tooexecute en åtgärd i ett system utanför Azure</span><span class="sxs-lookup"><span data-stu-id="5b61f-196">Call a web URL (webhook) tooexecute an action in a system outside of Azure</span></span>
* <span data-ttu-id="5b61f-197">Starta en runbook i Azure automation tooperform en rad olika uppgifter</span><span class="sxs-lookup"><span data-stu-id="5b61f-197">Start a runbook in Azure automation tooperform any variety of tasks</span></span>

## <a name="methods-of-accessing-azure-monitor"></a><span data-ttu-id="5b61f-198">Metoder för att komma åt Azure-Monitor</span><span class="sxs-lookup"><span data-stu-id="5b61f-198">Methods of accessing Azure Monitor</span></span>
<span data-ttu-id="5b61f-199">I allmänhet kan du ändra dataspårning, Routning och hämtning med någon av följande metoder hello.</span><span class="sxs-lookup"><span data-stu-id="5b61f-199">In general, you can manipulate data tracking, routing, and retrieval using one of hello following methods.</span></span> <span data-ttu-id="5b61f-200">Inte alla metoder är tillgängliga för alla åtgärder eller -datatyper.</span><span class="sxs-lookup"><span data-stu-id="5b61f-200">Not all methods are available for all actions or data types.</span></span>

* [<span data-ttu-id="5b61f-201">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5b61f-201">Azure portal</span></span>](https://portal.azure.com)
* [<span data-ttu-id="5b61f-202">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5b61f-202">PowerShell</span></span>](insights-powershell-samples.md)  
* [<span data-ttu-id="5b61f-203">Plattformsoberoende kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="5b61f-203">Cross-platform Command Line Interface (CLI)</span></span>](insights-cli-samples.md)
* [<span data-ttu-id="5b61f-204">REST-API</span><span class="sxs-lookup"><span data-stu-id="5b61f-204">REST API</span></span>](https://docs.microsoft.com/rest/api/monitor/)
* [<span data-ttu-id="5b61f-205">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="5b61f-205">.NET SDK</span></span>](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a><span data-ttu-id="5b61f-206">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5b61f-206">Next steps</span></span>
<span data-ttu-id="5b61f-207">Lär dig mer om</span><span class="sxs-lookup"><span data-stu-id="5b61f-207">Learn more about</span></span>
- <span data-ttu-id="5b61f-208">En videogenomgång av bara Azure-Monitor finns på</span><span class="sxs-lookup"><span data-stu-id="5b61f-208">A video walkthrough of just Azure Monitor is available at</span></span>  
<span data-ttu-id="5b61f-209">[Kom igång med Azure-Monitor](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor).</span><span class="sxs-lookup"><span data-stu-id="5b61f-209">[Get Started with Azure Monitor](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor).</span></span> <span data-ttu-id="5b61f-210">En ytterligare video som förklarar ett scenario där du kan använda Azure-Monitor finns på [utforska Microsoft Azure-övervakning och diagnostik](https://channel9.msdn.com/events/Ignite/2016/BRK2234) och [Azure Övervakare i ett videoklipp från Ignite 2016](https://myignite.microsoft.com/videos/4977)</span><span class="sxs-lookup"><span data-stu-id="5b61f-210">An additional video explaining a scenario where you can use Azure Monitor is available at [Explore Microsoft Azure monitoring and diagnostics](https://channel9.msdn.com/events/Ignite/2016/BRK2234) and [Azure Monitor in a video from Ignite 2016](https://myignite.microsoft.com/videos/4977)</span></span>
- <span data-ttu-id="5b61f-211">Kör genom hello Azure-Monitor gränssnitt i [komma igång med Azure-Monitor](monitoring-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="5b61f-211">Run through hello Azure Monitor interface in [Getting Started with Azure Monitor](monitoring-get-started.md)</span></span>
- <span data-ttu-id="5b61f-212">Ställ in hello [Azure Diagnostics tillägg](../azure-diagnostics.md) om du försöker toodiagnose problem i din molntjänst, virtuell dator, virtuella skala anger eller Service Fabric-programmet.</span><span class="sxs-lookup"><span data-stu-id="5b61f-212">Set up hello [Azure Diagnostics Extensions](../azure-diagnostics.md) if you are attempting toodiagnose problems in your Cloud Service, Virtual Machine, Virtual machine scale sets, or Service Fabric application.</span></span>
- <span data-ttu-id="5b61f-213">[Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) om du försöker toodiagnostic problem i din App Service Web app.</span><span class="sxs-lookup"><span data-stu-id="5b61f-213">[Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) if you are trying toodiagnostic problems in your App Service Web app.</span></span>
- <span data-ttu-id="5b61f-214">[Felsöka Azure Storage](../storage/common/storage-e2e-troubleshooting.md) när du använder Storage-Blobbar, tabeller eller köer</span><span class="sxs-lookup"><span data-stu-id="5b61f-214">[Troubleshooting Azure Storage](../storage/common/storage-e2e-troubleshooting.md) when using Storage Blobs, Tables, or Queues</span></span>
- <span data-ttu-id="5b61f-215">[Logga Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) och hello [Operations Management Suite](https://www.microsoft.com/oms/)</span><span class="sxs-lookup"><span data-stu-id="5b61f-215">[Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) and hello [Operations Management Suite](https://www.microsoft.com/oms/)</span></span>
