---
title: "aaaUse prestandaräknare i Azure-diagnostik | Microsoft Docs"
description: "Använd prestandaräknare i Azure-molntjänster eller virtuella toofind flaskhalsar och justera prestanda."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: fc4c61e9-d49d-4ed9-a32c-b91cdf851909
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/29/2016
ms.author: robb
ms.openlocfilehash: f3250816c01fc6e164a6aae48da5035845e6d2b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-use-performance-counters-in-an-azure-application"></a><span data-ttu-id="cbcf2-103">Skapa och använda prestandaräknare i ett Azure-program</span><span class="sxs-lookup"><span data-stu-id="cbcf2-103">Create and use performance counters in an Azure application</span></span>
<span data-ttu-id="cbcf2-104">Den här artikeln beskriver hello fördelarna och hur tooput prestandaräknare i Azure-program.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-104">This article describes hello benefits of and how tooput performance counters into your Azure application.</span></span> <span data-ttu-id="cbcf2-105">Du kan använda dem toocollect data, hitta flaskhalsar och finjustera system- och programprestanda.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-105">You can use them toocollect data, find bottlenecks, and tune system and application performance.</span></span>

<span data-ttu-id="cbcf2-106">Prestandaräknare som är tillgängliga för Windows Server, IIS och ASP.NET kan också samlas in och användas toodetermine hello hälsan hos ditt Azure webbroller, arbetsroller och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-106">Performance counters available for Windows Server, IIS, and ASP.NET can also be collected and used toodetermine hello health of your Azure web roles, worker roles, and Virtual Machines.</span></span> <span data-ttu-id="cbcf2-107">Du kan också skapa och använda anpassade prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-107">You can also create and use custom performance counters.</span></span>  

<span data-ttu-id="cbcf2-108">Du kan undersöka prestandaräknardata</span><span class="sxs-lookup"><span data-stu-id="cbcf2-108">You can examine performance counter data</span></span>

1. <span data-ttu-id="cbcf2-109">Direkt på hello programvärd med hello Performance Monitor-verktyget som nås med hjälp av fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="cbcf2-109">Directly on hello application host with hello Performance Monitor tool accessed using Remote Desktop</span></span>
2. <span data-ttu-id="cbcf2-110">Med System Center Operations Manager med hjälp av hello Azure Management Pack</span><span class="sxs-lookup"><span data-stu-id="cbcf2-110">With System Center Operations Manager using hello Azure Management Pack</span></span>
3. <span data-ttu-id="cbcf2-111">Med andra övervakningsverktyg som har åtkomst till hello överförs diagnostikdata tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-111">With other monitoring tools that access hello diagnostic data transferred tooAzure storage.</span></span> <span data-ttu-id="cbcf2-112">Se [Store och visa diagnostiska Data i Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-112">See [Store and View Diagnostic Data in Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) for more information.</span></span>  

<span data-ttu-id="cbcf2-113">Mer information om hur du övervakar hello prestanda i hello [Azure-portalen](http://portal.azure.com/), se [hur tooMonitor Cloud Services](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).</span><span class="sxs-lookup"><span data-stu-id="cbcf2-113">For more information on monitoring hello performance of your application in hello [Azure portal](http://portal.azure.com/), see [How tooMonitor Cloud Services](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).</span></span>

<span data-ttu-id="cbcf2-114">Mer detaljerad information om att skapa en loggning och spårning strategi och använder diagnostik- och andra tekniker tootroubleshoot problem och optimera Azure-program, se [felsökning bästa praxis för att utveckla Azure Program](https://msdn.microsoft.com/library/azure/hh771389.aspx).</span><span class="sxs-lookup"><span data-stu-id="cbcf2-114">For additional in-depth guidance on creating a logging and tracing strategy and using diagnostics and other techniques tootroubleshoot problems and optimize Azure applications, see [Troubleshooting Best Practices for Developing Azure Applications](https://msdn.microsoft.com/library/azure/hh771389.aspx).</span></span>

## <a name="enable-performance-counter-monitoring"></a><span data-ttu-id="cbcf2-115">Aktivera övervakning av räknare</span><span class="sxs-lookup"><span data-stu-id="cbcf2-115">Enable performance counter monitoring</span></span>
<span data-ttu-id="cbcf2-116">Prestandaräknare är inte aktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-116">Performance counters are not enabled by default.</span></span> <span data-ttu-id="cbcf2-117">Ditt program eller en startåtgärd måste ändra hello standard diagnostik agent configuration tooinclude hello specifika prestandaräknare som du vill toomonitor för varje rollinstans.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-117">Your application or a startup task must modify hello default diagnostics agent configuration tooinclude hello specific performance counters that you wish toomonitor for each role instance.</span></span>

### <a name="performance-counters-available-for-microsoft-azure"></a><span data-ttu-id="cbcf2-118">Prestandaräknare för Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="cbcf2-118">Performance counters available for Microsoft Azure</span></span>
<span data-ttu-id="cbcf2-119">Azure tillhandahåller en delmängd av hello prestandaräknare som är tillgängliga för Windows Server, IIS och hello ASP.NET-stacken.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-119">Azure provides a subset of hello performance counters available for Windows Server, IIS and hello ASP.NET stack.</span></span> <span data-ttu-id="cbcf2-120">hello följande tabell visas några av hello prestandaräknarna särskilt intressanta för Azure-program.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-120">hello following table lists some of hello performance counters of particular interest for Azure applications.</span></span>

| <span data-ttu-id="cbcf2-121">: Räknaren Kategoriobjektet (instans)</span><span class="sxs-lookup"><span data-stu-id="cbcf2-121">Counter Category: Object (Instance)</span></span> | <span data-ttu-id="cbcf2-122">Räknarens namn</span><span class="sxs-lookup"><span data-stu-id="cbcf2-122">Counter Name</span></span> | <span data-ttu-id="cbcf2-123">Referens</span><span class="sxs-lookup"><span data-stu-id="cbcf2-123">Reference</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cbcf2-124">.NET CLR-undantag (*globala*)</span><span class="sxs-lookup"><span data-stu-id="cbcf2-124">.NET CLR Exceptions(*Global*)</span></span> |<span data-ttu-id="cbcf2-125"># Undantag som utlöses / sek</span><span class="sxs-lookup"><span data-stu-id="cbcf2-125"># Exceps Thrown / sec</span></span> |<span data-ttu-id="cbcf2-126">Prestandaräknare för undantag</span><span class="sxs-lookup"><span data-stu-id="cbcf2-126">Exception Performance Counters</span></span> |
| <span data-ttu-id="cbcf2-127">.NET CLR-minne (*globala*)</span><span class="sxs-lookup"><span data-stu-id="cbcf2-127">.NET CLR Memory(*Global*)</span></span> |<span data-ttu-id="cbcf2-128">Tid i GC i procent</span><span class="sxs-lookup"><span data-stu-id="cbcf2-128">% Time in GC</span></span> |<span data-ttu-id="cbcf2-129">Prestandaräknare för minnet</span><span class="sxs-lookup"><span data-stu-id="cbcf2-129">Memory Performance Counters</span></span> |
| <span data-ttu-id="cbcf2-130">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-130">ASP.NET</span></span> |<span data-ttu-id="cbcf2-131">Programmet startas om</span><span class="sxs-lookup"><span data-stu-id="cbcf2-131">Application Restarts</span></span> |<span data-ttu-id="cbcf2-132">Prestandaräknare för ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-132">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="cbcf2-133">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-133">ASP.NET</span></span> |<span data-ttu-id="cbcf2-134">Körningstid för begäran</span><span class="sxs-lookup"><span data-stu-id="cbcf2-134">Request Execution Time</span></span> |<span data-ttu-id="cbcf2-135">Prestandaräknare för ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-135">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="cbcf2-136">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-136">ASP.NET</span></span> |<span data-ttu-id="cbcf2-137">Begäranden som kopplats från</span><span class="sxs-lookup"><span data-stu-id="cbcf2-137">Requests Disconnected</span></span> |<span data-ttu-id="cbcf2-138">Prestandaräknare för ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-138">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="cbcf2-139">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-139">ASP.NET</span></span> |<span data-ttu-id="cbcf2-140">Omstart av arbetsprocess</span><span class="sxs-lookup"><span data-stu-id="cbcf2-140">Worker Process Restarts</span></span> |<span data-ttu-id="cbcf2-141">Prestandaräknare för ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-141">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="cbcf2-142">ASP.NET-program (**totala**)</span><span class="sxs-lookup"><span data-stu-id="cbcf2-142">ASP.NET Applications(**Total**)</span></span> |<span data-ttu-id="cbcf2-143">Totalt antal begäranden</span><span class="sxs-lookup"><span data-stu-id="cbcf2-143">Requests Total</span></span> |<span data-ttu-id="cbcf2-144">Prestandaräknare för ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-144">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="cbcf2-145">ASP.NET-program (**totala**)</span><span class="sxs-lookup"><span data-stu-id="cbcf2-145">ASP.NET Applications(**Total**)</span></span> |<span data-ttu-id="cbcf2-146">Begäranden per sekund</span><span class="sxs-lookup"><span data-stu-id="cbcf2-146">Requests/Sec</span></span> |<span data-ttu-id="cbcf2-147">Prestandaräknare för ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-147">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="cbcf2-148">ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="cbcf2-148">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="cbcf2-149">Körningstid för begäran</span><span class="sxs-lookup"><span data-stu-id="cbcf2-149">Request Execution Time</span></span> |<span data-ttu-id="cbcf2-150">Prestandaräknare för ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-150">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="cbcf2-151">ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="cbcf2-151">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="cbcf2-152">Väntetid för begäran</span><span class="sxs-lookup"><span data-stu-id="cbcf2-152">Request Wait Time</span></span> |<span data-ttu-id="cbcf2-153">Prestandaräknare för ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-153">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="cbcf2-154">ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="cbcf2-154">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="cbcf2-155">Aktuella begäranden</span><span class="sxs-lookup"><span data-stu-id="cbcf2-155">Requests Current</span></span> |<span data-ttu-id="cbcf2-156">Prestandaräknare för ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-156">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="cbcf2-157">ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="cbcf2-157">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="cbcf2-158">Begäranden i kö</span><span class="sxs-lookup"><span data-stu-id="cbcf2-158">Requests Queued</span></span> |<span data-ttu-id="cbcf2-159">Prestandaräknare för ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-159">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="cbcf2-160">ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="cbcf2-160">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="cbcf2-161">Begäranden som nekats</span><span class="sxs-lookup"><span data-stu-id="cbcf2-161">Requests Rejected</span></span> |<span data-ttu-id="cbcf2-162">Prestandaräknare för ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-162">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="cbcf2-163">Minne</span><span class="sxs-lookup"><span data-stu-id="cbcf2-163">Memory</span></span> |<span data-ttu-id="cbcf2-164">Tillgängliga megabyte</span><span class="sxs-lookup"><span data-stu-id="cbcf2-164">Available MBytes</span></span> |<span data-ttu-id="cbcf2-165">Prestandaräknare för minnet</span><span class="sxs-lookup"><span data-stu-id="cbcf2-165">Memory Performance Counters</span></span> |
| <span data-ttu-id="cbcf2-166">Minne</span><span class="sxs-lookup"><span data-stu-id="cbcf2-166">Memory</span></span> |<span data-ttu-id="cbcf2-167">Dedikerade byte</span><span class="sxs-lookup"><span data-stu-id="cbcf2-167">Committed Bytes</span></span> |<span data-ttu-id="cbcf2-168">Prestandaräknare för minnet</span><span class="sxs-lookup"><span data-stu-id="cbcf2-168">Memory Performance Counters</span></span> |
| <span data-ttu-id="cbcf2-169">Processor(_Total)</span><span class="sxs-lookup"><span data-stu-id="cbcf2-169">Processor(_Total)</span></span> |<span data-ttu-id="cbcf2-170">% Processortid</span><span class="sxs-lookup"><span data-stu-id="cbcf2-170">% Processor Time</span></span> |<span data-ttu-id="cbcf2-171">Prestandaräknare för ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cbcf2-171">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="cbcf2-172">TCPv4</span><span class="sxs-lookup"><span data-stu-id="cbcf2-172">TCPv4</span></span> |<span data-ttu-id="cbcf2-173">Anslutningsfel</span><span class="sxs-lookup"><span data-stu-id="cbcf2-173">Connection Failures</span></span> |<span data-ttu-id="cbcf2-174">TCP-objekt</span><span class="sxs-lookup"><span data-stu-id="cbcf2-174">TCP Object</span></span> |
| <span data-ttu-id="cbcf2-175">TCPv4</span><span class="sxs-lookup"><span data-stu-id="cbcf2-175">TCPv4</span></span> |<span data-ttu-id="cbcf2-176">Anslutningar</span><span class="sxs-lookup"><span data-stu-id="cbcf2-176">Connections Established</span></span> |<span data-ttu-id="cbcf2-177">TCP-objekt</span><span class="sxs-lookup"><span data-stu-id="cbcf2-177">TCP Object</span></span> |
| <span data-ttu-id="cbcf2-178">TCPv4</span><span class="sxs-lookup"><span data-stu-id="cbcf2-178">TCPv4</span></span> |<span data-ttu-id="cbcf2-179">Återställda anslutningar</span><span class="sxs-lookup"><span data-stu-id="cbcf2-179">Connections Reset</span></span> |<span data-ttu-id="cbcf2-180">TCP-objekt</span><span class="sxs-lookup"><span data-stu-id="cbcf2-180">TCP Object</span></span> |
| <span data-ttu-id="cbcf2-181">TCPv4</span><span class="sxs-lookup"><span data-stu-id="cbcf2-181">TCPv4</span></span> |<span data-ttu-id="cbcf2-182">Segment/s</span><span class="sxs-lookup"><span data-stu-id="cbcf2-182">Segments Sent/sec</span></span> |<span data-ttu-id="cbcf2-183">TCP-objekt</span><span class="sxs-lookup"><span data-stu-id="cbcf2-183">TCP Object</span></span> |
| <span data-ttu-id="cbcf2-184">Nätverk Interface(*)</span><span class="sxs-lookup"><span data-stu-id="cbcf2-184">Network Interface(*)</span></span> |<span data-ttu-id="cbcf2-185">Mottagna byte/sek</span><span class="sxs-lookup"><span data-stu-id="cbcf2-185">Bytes Received/sec</span></span> |<span data-ttu-id="cbcf2-186">Objektet för nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="cbcf2-186">Network Interface Object</span></span> |
| <span data-ttu-id="cbcf2-187">Nätverk Interface(*)</span><span class="sxs-lookup"><span data-stu-id="cbcf2-187">Network Interface(*)</span></span> |<span data-ttu-id="cbcf2-188">Skickade byte/sek</span><span class="sxs-lookup"><span data-stu-id="cbcf2-188">Bytes Sent/sec</span></span> |<span data-ttu-id="cbcf2-189">Objektet för nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="cbcf2-189">Network Interface Object</span></span> |
| <span data-ttu-id="cbcf2-190">Nätverksgränssnittet (Microsoft Virtual Machine Bus nätverk kort _2)</span><span class="sxs-lookup"><span data-stu-id="cbcf2-190">Network Interface(Microsoft Virtual Machine Bus Network Adapter _2)</span></span> |<span data-ttu-id="cbcf2-191">Mottagna byte/sek</span><span class="sxs-lookup"><span data-stu-id="cbcf2-191">Bytes Received/sec</span></span> |<span data-ttu-id="cbcf2-192">Objektet för nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="cbcf2-192">Network Interface Object</span></span> |
| <span data-ttu-id="cbcf2-193">Nätverksgränssnittet (Microsoft Virtual Machine Bus nätverk kort _2)</span><span class="sxs-lookup"><span data-stu-id="cbcf2-193">Network Interface(Microsoft Virtual Machine Bus Network Adapter _2)</span></span> |<span data-ttu-id="cbcf2-194">Skickade byte/sek</span><span class="sxs-lookup"><span data-stu-id="cbcf2-194">Bytes Sent/sec</span></span> |<span data-ttu-id="cbcf2-195">Objektet för nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="cbcf2-195">Network Interface Object</span></span> |
| <span data-ttu-id="cbcf2-196">Nätverksgränssnittet (Microsoft Virtual Machine Bus nätverk kort _2)</span><span class="sxs-lookup"><span data-stu-id="cbcf2-196">Network Interface(Microsoft Virtual Machine Bus Network Adapter _2)</span></span> |<span data-ttu-id="cbcf2-197">Byte totalt/sek</span><span class="sxs-lookup"><span data-stu-id="cbcf2-197">Bytes Total/sec</span></span> |<span data-ttu-id="cbcf2-198">Objektet för nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="cbcf2-198">Network Interface Object</span></span> |

## <a name="create-and-add-custom-performance-counters-tooyour-application"></a><span data-ttu-id="cbcf2-199">Skapa och lägga till anpassade prestandaräknare tooyour program</span><span class="sxs-lookup"><span data-stu-id="cbcf2-199">Create and add custom performance counters tooyour application</span></span>
<span data-ttu-id="cbcf2-200">Azure har stöd för toocreate och ändra anpassade prestandaräknare för webbprogram och arbetsroller.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-200">Azure has support toocreate and modify custom performance counters for web roles and worker roles.</span></span> <span data-ttu-id="cbcf2-201">hello räknare kan vara används tootrack och övervaka programspecifika beteende.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-201">hello counters may be used tootrack and monitor application-specific behavior.</span></span> <span data-ttu-id="cbcf2-202">Du kan skapa och ta bort anpassade prestandaräknarkategorier och specificerare från en startaktivitet, webbroll eller arbetsroll med förhöjd behörighet.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-202">You can create and delete custom performance counter categories and specifiers from a startup task, web role, or worker role with elevated permissions.</span></span>

> [!NOTE]
> <span data-ttu-id="cbcf2-203">Kod som ändrar toocustom prestandaräknare har utökade behörigheter toorun.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-203">Code that makes changes toocustom performance counters must have elevated permissions toorun.</span></span> <span data-ttu-id="cbcf2-204">Om hello kod i en webbroll eller arbetsrollen hello rollen måste innehålla hello taggen <Runtime executionContext="elevated" /> i hello ServiceDefinition.csdef fil för hello rollen tooinitialize korrekt.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-204">If hello code is in a web role or worker role, hello role must include hello tag <Runtime executionContext="elevated" /> in hello ServiceDefinition.csdef file for hello role tooinitialize properly.</span></span>
>
>

<span data-ttu-id="cbcf2-205">Du kan skicka anpassade räknare tooAzure lagringsprestanda med hello diagnostik-agenten.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-205">You can send custom performance counter data tooAzure storage using hello diagnostics agent.</span></span>

<span data-ttu-id="cbcf2-206">hello standard prestandaräknardata genereras av hello Azure processer.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-206">hello standard performance counter data is generated by hello Azure processes.</span></span> <span data-ttu-id="cbcf2-207">Anpassade prestandaräknardata måste ha skapats av rollen eller worker-rollen webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-207">Custom performance counter data must be created by your web role or worker role application.</span></span> <span data-ttu-id="cbcf2-208">Se [prestandaräknaren typer](https://msdn.microsoft.com/library/z573042h.aspx) information om hello datatyper som kan lagras i anpassade prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-208">See [Performance Counter Types](https://msdn.microsoft.com/library/z573042h.aspx) for information on hello types of data that can be stored in custom performance counters.</span></span> <span data-ttu-id="cbcf2-209">Se [PerformanceCounters exempel](http://code.msdn.microsoft.com/azure/) ett exempel som skapar och anger anpassade prestandaräknardata i en webbroll.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-209">See [PerformanceCounters Sample](http://code.msdn.microsoft.com/azure/) for an example that creates and sets custom performance counter data in a web role.</span></span>

## <a name="store-and-view-performance-counter-data"></a><span data-ttu-id="cbcf2-210">Lagra och visa information om prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="cbcf2-210">Store and view performance counter data</span></span>
<span data-ttu-id="cbcf2-211">Azure cachelagrar prestandaräknardata med annan diagnostisk information.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-211">Azure caches performance counter data with other diagnostic information.</span></span> <span data-ttu-id="cbcf2-212">Informationen är tillgänglig för fjärranslutna övervakning medan hälsningspaket rollinstansen körs med hjälp av fjärråtkomst till skrivbordet tooview verktyg, till exempel Prestandaövervakaren.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-212">This data is available for remote monitoring while hello role instance is running using remote desktop access tooview tools such as Performance Monitor.</span></span> <span data-ttu-id="cbcf2-213">toopersist hello utanför hälsningspaket rollinstansen, hello diagnostik agent måste dataöverföring hello tooAzure datalagring.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-213">toopersist hello data outside of hello role instance, hello diagnostics agent must transfer hello data tooAzure storage.</span></span> <span data-ttu-id="cbcf2-214">hello storleksgränsen på hello cachelagras prestandaräknardata kan konfigureras i hello diagnostik agent eller också är det konfigurerade toobe en del av en delad gräns för alla hello diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-214">hello size limit of hello cached performance counter data can be configured in hello diagnostics agent, or it may be configured toobe part of a shared limit for all hello diagnostic data.</span></span> <span data-ttu-id="cbcf2-215">Mer information om hur du anger hello buffertstorlek finns [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) och [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx).</span><span class="sxs-lookup"><span data-stu-id="cbcf2-215">For more information about setting hello buffer size, see [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) and [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx).</span></span> <span data-ttu-id="cbcf2-216">Se [Store och visa diagnostiska Data i Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) en översikt över hur du konfigurerar hello agent tootransfer data tooa diagnostiklagringskonto.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-216">See [Store and View Diagnostic Data in Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) for an overview of setting up hello diagnostics agent tootransfer data tooa storage account.</span></span>

<span data-ttu-id="cbcf2-217">Varje konfigurerade prestandaräknarinstans registreras på en angiven samplingsfrekvensen och hello provtagning data överförs toohello storage-konto genom att en schemalagd begäran eller ett för uppringning på begäran.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-217">Each configured performance counter instance is recorded at a specified sampling rate, and hello sampled data is transferred toohello storage account either by a scheduled transfer request or an on-demand transfer request.</span></span> <span data-ttu-id="cbcf2-218">Automatiska överföringar kan schemaläggas så ofta som en gång per minut.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-218">Automatic transfers may be scheduled as often as once per minute.</span></span> <span data-ttu-id="cbcf2-219">Prestandaräknardata som överförs av hello diagnostik agent lagras i en tabell, WADPerformanceCountersTable, i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-219">Performance counter data transferred by hello diagnostics agent is stored in a table, WADPerformanceCountersTable, in hello storage account.</span></span> <span data-ttu-id="cbcf2-220">Den här tabellen kan nås och efterfrågas med Azure standardlagring API-metoder.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-220">This table may be accessed and queried with standard Azure storage API methods.</span></span> <span data-ttu-id="cbcf2-221">Se [Microsoft Azure prestandaräknarna Sample](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) ett exempel på frågor och visa prestandaräknardata från hello WADPerformanceCountersTable tabell.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-221">See [Microsoft Azure PerformanceCounters Sample](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) for an example of querying and displaying performance counter data from hello WADPerformanceCountersTable table.</span></span>

> [!NOTE]
> <span data-ttu-id="cbcf2-222">Beroende på hello diagnostik agent överföring frekvens och kön latens kan hello senaste prestandaräknardata i hello storage-konto vara flera minuter för gammal.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-222">Depending on hello diagnostics agent transfer frequency and queue latency, hello most recent performance counter data in hello storage account may be several minutes out of date.</span></span>
>
>

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a><span data-ttu-id="cbcf2-223">Aktivera prestandaräknare med diagnostik konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="cbcf2-223">Enable performance counters using diagnostics configuration file</span></span>
<span data-ttu-id="cbcf2-224">Använd följande procedur tooenable prestandaräknare i Azure-programmet hello.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-224">Use hello following procedure tooenable performance counters in your Azure application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cbcf2-225">Krav</span><span class="sxs-lookup"><span data-stu-id="cbcf2-225">Prerequisites</span></span>
<span data-ttu-id="cbcf2-226">Det här avsnittet förutsätter att du har importerat hello diagnostik övervakaren till ditt program och lagt till hello diagnostik configuration file tooyour Visual Studio-lösning (diagnostics.wadcfg i SDK 2.4 och nedan eller diagnostics.wadcfgx i SDK 2.5 och senare).</span><span class="sxs-lookup"><span data-stu-id="cbcf2-226">This section assumes that you have imported hello Diagnostics monitor into your application and added hello diagnostics configuration file tooyour Visual Studio solution (diagnostics.wadcfg in SDK 2.4 and below or diagnostics.wadcfgx in SDK 2.5 and above).</span></span> <span data-ttu-id="cbcf2-227">Se steg 1 och 2 i [aktiverar diagnostik i Azure-molntjänster och virtuella datorer](cloud-services-dotnet-diagnostics.md)) mer information.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-227">See steps 1 and 2 in [Enabling Diagnostics in Azure Cloud Services and Virtual Machines](cloud-services-dotnet-diagnostics.md)) for more information.</span></span>

## <a name="step-1-collect-and-store-data-from-performance-counters"></a><span data-ttu-id="cbcf2-228">Steg 1: Samla in och lagra data från prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="cbcf2-228">Step 1: Collect and store data from performance counters</span></span>
<span data-ttu-id="cbcf2-229">När du har lagt till hello diagnostik filen tooyour Visual Studio-lösning kan konfigurera du hello insamling och lagring av prestandaräknardata i ett Azure-program.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-229">After you have added hello diagnostics file tooyour Visual Studio solution, you can configure hello collection and storage of performance counter data in an Azure application.</span></span> <span data-ttu-id="cbcf2-230">Detta görs genom att lägga till prestandaräknare toohello diagnostik filen.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-230">This is done by adding performance counters toohello diagnostics file.</span></span> <span data-ttu-id="cbcf2-231">Diagnostikdata, inklusive prestandaräknare, samlas först på hello-instansen.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-231">Diagnostics data, including performance counters, is first collected on hello instance.</span></span> <span data-ttu-id="cbcf2-232">hello data är beständiga toohello WADPerformanceCountersTable tabell i hello Azure tabelltjänsten, så kommer du att också behöver toospecify hello storage-konto i ditt program.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-232">hello data is then persisted toohello WADPerformanceCountersTable table in hello Azure Table service, so you will also need toospecify hello storage account in your application.</span></span> <span data-ttu-id="cbcf2-233">Om du testar programmet lokalt i hello Compute Emulator, kan du också lagra diagnostikdata lokalt i hello Storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-233">If you're testing your application locally in hello Compute Emulator, you can also store diagnostics data locally in hello Storage Emulator.</span></span> <span data-ttu-id="cbcf2-234">Innan du sparar diagnostikdata, måste du först gå toohello [Azure-portalen](http://portal.azure.com/) och skapa ett klassiska storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-234">Before you store diagnostics data, you must first go toohello [Azure portal](http://portal.azure.com/) and create a classic storage account.</span></span> <span data-ttu-id="cbcf2-235">Ett bra tips är toolocate storage-konto i hello samma geografiska plats som din Azure-program.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-235">A best practice is toolocate your storage account in hello same geo-location as your Azure application.</span></span> <span data-ttu-id="cbcf2-236">Genom att hålla hello Azure program- och storage-konto finns i hello samma geografiska plats kan du undvika betalar externa bandbreddskostnader och minska svarstiden.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-236">By keeping hello Azure application and storage account are in hello same geo-location, you avoid paying external bandwidth costs and reduce latency.</span></span>

### <a name="add-performance-counters-toohello-diagnostics-file"></a><span data-ttu-id="cbcf2-237">Lägg till prestandaräknare toohello diagnostik-fil</span><span class="sxs-lookup"><span data-stu-id="cbcf2-237">Add performance counters toohello diagnostics file</span></span>
<span data-ttu-id="cbcf2-238">Det finns många räknare som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-238">There are many counters you can use.</span></span> <span data-ttu-id="cbcf2-239">hello visar följande exempel flera prestandaräknare som rekommenderas för webb- och arbetsprocesser för övervakning av rollen.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-239">hello following example shows several performance counters that are recommended for web and worker role monitoring.</span></span>

<span data-ttu-id="cbcf2-240">Öppna hello diagnostik filen (diagnostics.wadcfg SDK 2.4 och lägre eller diagnostics.wadcfgx i SDK 2.5 och senare) och Lägg till följande toohello DiagnosticMonitorConfiguration element hello:</span><span class="sxs-lookup"><span data-stu-id="cbcf2-240">Open hello diagnostics file (diagnostics.wadcfg in SDK 2.4 and below or diagnostics.wadcfgx in SDK 2.5 and above) and add hello following toohello DiagnosticMonitorConfiguration element:</span></span>

```xml
<PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
    <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use hello Process(w3wp) category counters in a web role -->

    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use hello Process(WaWorkerHost) category counters in a worker role.
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
</PerformanceCounters>
```

<span data-ttu-id="cbcf2-241">Hej bufferQuotaInMB attribut, som anger hello maximal mängd fillagring för system som är tillgängligt för hello samling datatyp (Azure loggar, IIS-loggar osv.).</span><span class="sxs-lookup"><span data-stu-id="cbcf2-241">hello bufferQuotaInMB attribute, which specifies hello maximum amount of file system storage that is available for hello data collection type (Azure logs, IIS logs, etc.).</span></span> <span data-ttu-id="cbcf2-242">hello standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-242">hello default is 0.</span></span> <span data-ttu-id="cbcf2-243">När hello kvoten har uppnåtts tas hello äldsta data bort när nya data läggs.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-243">When hello quota is reached, hello oldest data is deleted as new data is added.</span></span> <span data-ttu-id="cbcf2-244">hello summan av alla hello bufferQuotaInMB egenskaper måste vara större än hello värdet för hello OverallQuotaInMB attribut.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-244">hello sum of all hello bufferQuotaInMB properties must be greater than hello value of hello OverallQuotaInMB attribute.</span></span> <span data-ttu-id="cbcf2-245">En mer detaljerad beskrivning för att avgöra hur mycket lagringsutrymme måste finnas för hello samling diagnostikdata finns hello installationsprogrammet BOMULLSTUSS avsnitt i [felsökning bästa praxis för att utveckla program i Azure](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).</span><span class="sxs-lookup"><span data-stu-id="cbcf2-245">For a more detailed discussion of determining how much storage will be required for hello collection of diagnostics data, see hello Setup WAD section of [Troubleshooting Best Practices for Developing Azure Applications](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).</span></span>

<span data-ttu-id="cbcf2-246">Hej scheduledTransferPeriod attribut, som anger hello intervallet mellan schemalagda dataöverföringar avrundat toohello närmast minuter.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-246">hello scheduledTransferPeriod attribute, which specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span> <span data-ttu-id="cbcf2-247">I följande exempel hello, är den inställd tooPT30M (30 minuter).</span><span class="sxs-lookup"><span data-stu-id="cbcf2-247">In hello following examples, it is set tooPT30M (30 minutes).</span></span> <span data-ttu-id="cbcf2-248">Hello överföring period tooa små Inställningsvärdet, till exempel 1 minut, påverkar negativt programmets prestanda i produktion, men kan vara användbart för att visa diagnostiken arbetar snabbt när du testar.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-248">Setting hello transfer period tooa small value, such as 1 minute, will adversely impact your application's performance in production but can be useful for seeing diagnostics working quickly when you are testing.</span></span> <span data-ttu-id="cbcf2-249">hello schemalagda överföring period måste vara tillräckligt litet tooensure diagnostikdata inte är över på hello-instans, men tillräckligt stor för att det inte påverkar hello prestanda för ditt program.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-249">hello scheduled transfer period should be small enough tooensure that diagnostic data is not overwritten on hello instance, but large enough that it will not impact hello performance of your application.</span></span>

<span data-ttu-id="cbcf2-250">Hej counterSpecifier attributet anger hello prestandaräknaren toocollect.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-250">hello counterSpecifier attribute specifies hello performance counter toocollect.</span></span> <span data-ttu-id="cbcf2-251">hello sampleRate attribut anger hello hastighet med vilken hello prestandaräknaren ska samlas in, i det här fallet 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-251">hello sampleRate attribute specifies hello rate at which hello performance counter should be sampled, in this case 30 seconds.</span></span>

<span data-ttu-id="cbcf2-252">När du har lagt till hello prestandaräknare som du vill toocollect, sparar du ändringarna toohello diagnostik-filen.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-252">Once you've added hello performance counters that you want toocollect, save your changes toohello diagnostics file.</span></span> <span data-ttu-id="cbcf2-253">Sedan måste toospecify hello storage-konto som ska vara beständig hello diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-253">Next, you need toospecify hello storage account that hello diagnostics data will be persisted to.</span></span>

### <a name="specify-hello-storage-account"></a><span data-ttu-id="cbcf2-254">Ange hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="cbcf2-254">Specify hello storage account</span></span>
<span data-ttu-id="cbcf2-255">toopersist din diagnostik information tooyour Azure Storage-konto, måste du ange en anslutningssträng i din tjänstekonfigurationsfil (ServiceConfiguration.cscfg).</span><span class="sxs-lookup"><span data-stu-id="cbcf2-255">toopersist your diagnostics information tooyour Azure Storage account, you must specify a connection string in your service configuration (ServiceConfiguration.cscfg) file.</span></span>

<span data-ttu-id="cbcf2-256">För Azure SDK 2.5 hello Storage-konto anges i hello diagnostics.wadcfgx-filen.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-256">For Azure SDK 2.5, hello Storage Account can be specified in hello diagnostics.wadcfgx file.</span></span>

> [!NOTE]
> <span data-ttu-id="cbcf2-257">Dessa anvisningar gäller endast tooAzure SDK 2.4 och nedan.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-257">These instructions only apply tooAzure SDK 2.4 and below.</span></span> <span data-ttu-id="cbcf2-258">För Azure SDK 2.5 hello Storage-konto anges i hello diagnostics.wadcfgx-filen.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-258">For Azure SDK 2.5, hello Storage Account can be specified in hello diagnostics.wadcfgx file.</span></span>
>
>

<span data-ttu-id="cbcf2-259">anslutningssträngar för tooset hello:</span><span class="sxs-lookup"><span data-stu-id="cbcf2-259">tooset hello connection strings:</span></span>

1. <span data-ttu-id="cbcf2-260">Öppna hello ServiceConfiguration.Cloud.cscfg filen med ditt favoritprogram för textredigering och ange hello anslutningssträngen för lagringen.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-260">Open hello ServiceConfiguration.Cloud.cscfg file using your favorite text editor and set hello connection string for your storage.</span></span> <span data-ttu-id="cbcf2-261">Hej *AccountName* och *AccountKey* värden finns i hello Azure-portalen i hello storage-konto instrumentpanelen under åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-261">hello *AccountName* and *AccountKey* values are found in hello Azure portal in hello storage account dashboard, under Access keys.</span></span>

    ```xml
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. <span data-ttu-id="cbcf2-262">Spara hello ServiceConfiguration.Cloud.cscfg filen.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-262">Save hello ServiceConfiguration.Cloud.cscfg file.</span></span>
3. <span data-ttu-id="cbcf2-263">Öppna hello ServiceConfiguration.Local.cscfg filen och kontrollera att UseDevelopmentStorage är tootrue.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-263">Open hello ServiceConfiguration.Local.cscfg file and verify that UseDevelopmentStorage is set tootrue.</span></span>

    ```xml
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
   <span data-ttu-id="cbcf2-264">Nu när hello anslutningssträngar ställs finns programmet kvar diagnostiklagringskonto för data tooyour när programmet distribueras.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-264">Now that hello connection strings are set, your application will persist diagnostics data tooyour storage account when your application is deployed.</span></span>
4. <span data-ttu-id="cbcf2-265">Spara och bygga projektet och sedan distribuera ditt program.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-265">Save and build your project, then deploy your application.</span></span>

## <a name="step-2-optional-create-custom-performance-counters"></a><span data-ttu-id="cbcf2-266">Steg 2: (Valfritt) skapa anpassade prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="cbcf2-266">Step 2: (Optional) Create custom performance counters</span></span>
<span data-ttu-id="cbcf2-267">Dessutom toohello fördefinierad prestandaräknare, du kan lägga till egna anpassade prestandaräknare toomonitor webb eller arbetare roller.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-267">In addition toohello pre-defined performance counters, you can add your own custom performance counters toomonitor web or worker roles.</span></span> <span data-ttu-id="cbcf2-268">Anpassade prestandaräknare kan vara används tootrack och övervaka programspecifika beteende och kan skapas eller tas bort i en startaktivitet, webbroll eller arbetsroll med förhöjd behörighet.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-268">Custom performance counters may be used tootrack and monitor application-specific behavior and can be created or deleted in a startup task, web role, or worker role with elevated permissions.</span></span>

<span data-ttu-id="cbcf2-269">hello Azure diagnostics agenten uppdaterar hello prestandaräknaren konfiguration från hello .wadcfg fil minuts när du har startat.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-269">hello Azure diagnostics agent refreshes hello performance counter configuration from hello .wadcfg file one minute after starting.</span></span>  <span data-ttu-id="cbcf2-270">Om du skapar anpassade prestandaräknare i hello OnStart-metoden och Start aktiviteterna ta längre tid än en minut tooexecute, dina anpassade prestandaräknare kommer inte har skapats när hello Azure Diagnostics agent försöker tooload dem.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-270">If you create custom performance counters in hello OnStart method and your startup tasks take longer than one minute tooexecute, your custom performance counters will not have been created when hello Azure Diagnostics agent tries tooload them.</span></span>  <span data-ttu-id="cbcf2-271">I det här scenariot ser du att Azure-diagnostik korrekt fångar upp alla diagnostikdata utom dina anpassade prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-271">In this scenario, you will see that Azure Diagnostics correctly captures all diagnostics data except your custom performance counters.</span></span>  <span data-ttu-id="cbcf2-272">tooresolve problemet, skapa hello prestandaräknare i en startåtgärd eller flytta vissa av dina startaktivitet fungerar toohello OnStart-metoden när du har skapat hello prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-272">tooresolve this issue, create hello performance counters in a startup task or move some of your startup task work toohello OnStart method after creating hello performance counters.</span></span>

<span data-ttu-id="cbcf2-273">Utför följande steg toocreate en enkel anpassade prestandaräknaren ”\MyCustomCounterCategory\MyButton1Counter” hello:</span><span class="sxs-lookup"><span data-stu-id="cbcf2-273">Perform hello following steps toocreate a simple custom performance counter named "\MyCustomCounterCategory\MyButton1Counter":</span></span>

1. <span data-ttu-id="cbcf2-274">Öppna hello tjänstdefinitionsfilen (CSDEF) för ditt program.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-274">Open hello service definition file (CSDEF) for your application.</span></span>
2. <span data-ttu-id="cbcf2-275">Lägg till hello Runtime elementet toohello WebRole eller WorkerRole element tooallow körning med förhöjd behörighet:</span><span class="sxs-lookup"><span data-stu-id="cbcf2-275">Add hello Runtime element toohello WebRole or WorkerRole element tooallow execution with elevated privileges:</span></span>

    ```xml
    <runtime executioncontext="elevated"/>
    ```
3. <span data-ttu-id="cbcf2-276">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-276">Save hello file.</span></span>
4. <span data-ttu-id="cbcf2-277">Öppna hello diagnostik filen (diagnostics.wadcfg SDK 2.4 och lägre eller diagnostics.wadcfgx i SDK 2.5 och senare) och Lägg till följande toohello DiagnosticMonitorConfiguration hello</span><span class="sxs-lookup"><span data-stu-id="cbcf2-277">Open hello diagnostics file (diagnostics.wadcfg in SDK 2.4 and below or diagnostics.wadcfgx in SDK 2.5 and above) and add hello following toohello DiagnosticMonitorConfiguration</span></span>

    ```xml
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
      <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. <span data-ttu-id="cbcf2-278">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-278">Save hello file.</span></span>
6. <span data-ttu-id="cbcf2-279">Skapa hello anpassade prestandaräknarkategorin i hello OnStart-metoden i din roll innan du anropar base. OnStart.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-279">Create hello custom performance counter category in hello OnStart method of your role, before invoking base.OnStart.</span></span> <span data-ttu-id="cbcf2-280">hello skapar följande C#-exempel en anpassad kategori, om den inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="cbcf2-280">hello following C# example creates a custom category, if it does not already exist:</span></span>

    ```csharp
    public override bool OnStart()
    {
      if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
      {
         CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

         // add a counter tracking user button1 clicks
         CounterCreationData operationTotal1 = new CounterCreationData();
         operationTotal1.CounterName = "MyButton1Counter";
         operationTotal1.CounterHelp = "My Custom Counter for Button1";
         operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
         counterCollection.Add(operationTotal1);

         PerformanceCounterCategory.Create(
           "MyCustomCounterCategory",
           "My Custom Counter Category",
           PerformanceCounterCategoryType.SingleInstance, counterCollection);

         Trace.WriteLine("Custom counter category created.");
      }
      else {
        Trace.WriteLine("Custom counter category already exists.");
      }

    return base.OnStart();
    }
    ```
7. <span data-ttu-id="cbcf2-281">Uppdatera hello räknare i ditt program.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-281">Update hello counters within your application.</span></span> <span data-ttu-id="cbcf2-282">följande exempel hello uppdateringar för en anpassad prestandaräknare på Button1_Click händelser:</span><span class="sxs-lookup"><span data-stu-id="cbcf2-282">hello following example updates a custom performance counter on Button1_Click events:</span></span>

    ```csharp
    protected void Button1_Click(object sender, EventArgs e)
    {
      PerformanceCounter button1Counter = new PerformanceCounter(
        "MyCustomCounterCategory",
        "MyButton1Counter",
        string.Empty,
        false);
      button1Counter.Increment();
      this.Button1.Text = "Button 1 count: " +
        button1Counter.RawValue.ToString();
    }
    ```
8. <span data-ttu-id="cbcf2-283">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-283">Save hello file.</span></span>  

<span data-ttu-id="cbcf2-284">Anpassade prestandaräknardata samlas nu av hello Azure diagnostics Övervakare.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-284">Custom performance counter data will now be collected by hello Azure diagnostics monitor.</span></span>

## <a name="step-3-query-performance-counter-data"></a><span data-ttu-id="cbcf2-285">Steg 3: Fråga prestandaräknardata</span><span class="sxs-lookup"><span data-stu-id="cbcf2-285">Step 3: Query performance counter data</span></span>
<span data-ttu-id="cbcf2-286">När programmet har distribuerats och kör, hello diagnostik börjar samla in prestandaräknare och spara den tooAzure datalagringen.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-286">Once your application is deployed and running, hello Diagnostics monitor will begin collecting performance counters and persisting that data tooAzure storage.</span></span> <span data-ttu-id="cbcf2-287">Du kan använda verktyg som till exempel Server Explorer i Visual Studio [Azure Lagringsutforskaren](http://azurestorageexplorer.codeplex.com/), eller [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) av Cerebrata tooview hello prestandaräknare för data i hello WADPerformanceCountersTable tabell.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-287">You use tools such as Server Explorer in Visual Studio,  [Azure Storage Explorer](http://azurestorageexplorer.codeplex.com/), or [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) by Cerebrata tooview hello performance counters data in hello WADPerformanceCountersTable table.</span></span> <span data-ttu-id="cbcf2-288">Du kan också programmässigt fråga hello tabell tjänsten använder [C#](../cosmos-db/table-storage-how-to-use-dotnet.md), [Java](../cosmos-db/table-storage-how-to-use-java.md), [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), eller [PHP](../cosmos-db/table-storage-how-to-use-php.md).</span><span class="sxs-lookup"><span data-stu-id="cbcf2-288">You can also programmatically query hello Table service using [C#](../cosmos-db/table-storage-how-to-use-dotnet.md),  [Java](../cosmos-db/table-storage-how-to-use-java.md),  [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), or [PHP](../cosmos-db/table-storage-how-to-use-php.md).</span></span>

<span data-ttu-id="cbcf2-289">hello C# exemplet nedan visar en enkel fråga mot hello WADPerformanceCountersTable tabell och sparar hello diagnostik data tooa CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-289">hello following C# example shows a basic query against hello WADPerformanceCountersTable table and saves hello diagnostics data tooa CSV file.</span></span> <span data-ttu-id="cbcf2-290">När hello prestandaräknare sparas tooa CSV-fil, kan du använda hello rita in funktioner i Microsoft Excel eller andra verktyg toovisualize hello data.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-290">Once hello performance counters are saved tooa CSV file, you can use hello graphing capabilities in Microsoft Excel or some other tool toovisualize hello data.</span></span> <span data-ttu-id="cbcf2-291">Vara säker på att tooadd en referens tooMicrosoft.WindowsAzure.Storage.dll som ingår i hello Azure SDK för .NET oktober 2012 och senare.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-291">Be sure tooadd a reference tooMicrosoft.WindowsAzure.Storage.dll, which is included in hello Azure SDK for .NET October 2012 and later.</span></span> <span data-ttu-id="cbcf2-292">hello-sammansättningen är installerade toohello % Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ katalog.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-292">hello assembly is installed toohello %Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ directory.</span></span>

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Table;
...

// Get hello connection string. When using Microsoft Azure Cloud Services, it is recommended
// you store your connection string using hello Microsoft Azure service configuration
// system (*.csdef and *.cscfg files). You can you use hello CloudConfigurationManager type
// tooretrieve your storage connection string.  If you're not using Cloud Services, it's
// recommended that you store hello connection string in your web.config or app.config file.
// Use hello ConfigurationManager type tooretrieve your storage connection string.

string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
//string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

// Get a reference toohello storage account using hello connection string.  You can also use hello development
// storage account (Storage Emulator) for local debugging.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
//CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "WADPerformanceCountersTable" table.
CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

// Create hello table query, filter on a specific CounterName, DeploymentId and RoleInstance.
TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
  .Where(
    TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
      TableOperators.And,
      TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
      TableOperators.And,
      TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
    )
  )
);

// Execute hello table query.
IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

// Process hello query results and build a CSV file.
StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

foreach (PerformanceCountersEntity entity in result)
{
  sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
    + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
}

StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
sw.Write(sb.ToString());
sw.Close();
```

<span data-ttu-id="cbcf2-293">Entiteter mappar tooC # objekt med hjälp av en anpassad klass som härleds från **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-293">Entities map tooC# objects using a custom class derived from **TableEntity**.</span></span> <span data-ttu-id="cbcf2-294">hello följande kod definierar en entitetsklass som representerar en prestandaräknare i hello **WADPerformanceCountersTable** tabell.</span><span class="sxs-lookup"><span data-stu-id="cbcf2-294">hello following code defines an entity class that represents a performance counter in hello **WADPerformanceCountersTable** table.</span></span>

```csharp
public class PerformanceCountersEntity : TableEntity
{
  public long EventTickCount { get; set; }
  public string DeploymentId { get; set; }
  public string Role { get; set; }
  public string RoleInstance { get; set; }
  public string CounterName { get; set; }
  public double CounterValue { get; set; }
}
```

## <a name="next-steps"></a><span data-ttu-id="cbcf2-295">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cbcf2-295">Next Steps</span></span>
[<span data-ttu-id="cbcf2-296">Visa ytterligare artiklar på Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="cbcf2-296">View additional articles on Azure Diagnostics</span></span>](../azure-diagnostics.md)
