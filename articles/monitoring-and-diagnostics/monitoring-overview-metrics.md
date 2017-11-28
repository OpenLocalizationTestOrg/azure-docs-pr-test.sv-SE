---
title: "aaaOverview av mått i Microsoft Azure | Microsoft Docs"
description: "Översikt över mått och deras användning i Microsoft Azure"
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 405ec51c-0946-4ec9-b535-60f65c4a5bd1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: johnkem
ms.openlocfilehash: 2b97f51e0554dae95a929241ae1f0e25e5c215ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-metrics-in-microsoft-azure"></a><span data-ttu-id="084b7-103">Översikt över mått i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="084b7-103">Overview of metrics in Microsoft Azure</span></span>
<span data-ttu-id="084b7-104">Den här artikeln beskriver vad är i Microsoft Azure har sina fördelar, och hur toostart som använder dem.</span><span class="sxs-lookup"><span data-stu-id="084b7-104">This article describes what metrics are in Microsoft Azure, their benefits, and how toostart using them.</span></span>  

## <a name="what-are-metrics"></a><span data-ttu-id="084b7-105">Vad är mått?</span><span class="sxs-lookup"><span data-stu-id="084b7-105">What are metrics?</span></span>
<span data-ttu-id="084b7-106">Azure-Monitor kan du tooconsume telemetri toogain insyn i hello prestanda och hälsotillståndet för dina arbetsbelastningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="084b7-106">Azure Monitor enables you tooconsume telemetry toogain visibility into hello performance and health of your workloads on Azure.</span></span> <span data-ttu-id="084b7-107">hello är viktigaste Azure telemetridata hello mått (kallas även prestandaräknare) sänds av mest Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="084b7-107">hello most important type of Azure telemetry data is hello metrics (also called performance counters) emitted by most Azure resources.</span></span> <span data-ttu-id="084b7-108">Övervakare för Azure tillhandahåller flera olika sätt tooconfigure och använda de här måtten för övervakning och felsökning.</span><span class="sxs-lookup"><span data-stu-id="084b7-108">Azure Monitor provides several ways tooconfigure and consume these metrics for monitoring and troubleshooting.</span></span>

## <a name="what-can-you-do-with-metrics"></a><span data-ttu-id="084b7-109">Vad kan du göra med mått?</span><span class="sxs-lookup"><span data-stu-id="084b7-109">What can you do with metrics?</span></span>
<span data-ttu-id="084b7-110">Mått är en del av telemetri och aktivera toodo hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="084b7-110">Metrics are a valuable source of telemetry and enable you toodo hello following tasks:</span></span>

* <span data-ttu-id="084b7-111">**Spåra hello prestanda** för din resurs (till exempel en VM, webbplats eller logik app) genom att rita upp dess mått i en portal diagram och fästa diagrammet tooa instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="084b7-111">**Track hello performance** of your resource (such as a VM, website, or logic app) by plotting its metrics on a portal chart and pinning that chart tooa dashboard.</span></span>
* <span data-ttu-id="084b7-112">**Få ett meddelande om ett problem** att hello påverkan på prestandan för din resurs när en måttet överskrider ett visst tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="084b7-112">**Get notified of an issue** that impacts hello performance of your resource when a metric crosses a certain threshold.</span></span>
* <span data-ttu-id="084b7-113">**Konfigurera automatiska åtgärder**, till exempel autoskalning en resurs eller startar en runbook när en måttet överskrider ett visst tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="084b7-113">**Configure automated actions**, such as autoscaling a resource or firing a runbook when a metric crosses a certain threshold.</span></span>
* <span data-ttu-id="084b7-114">**Utföra avancerade analyser** eller rapportering på prestanda eller användning trender för din resurs.</span><span class="sxs-lookup"><span data-stu-id="084b7-114">**Perform advanced analytics** or reporting on performance or usage trends of your resource.</span></span>
* <span data-ttu-id="084b7-115">**Arkivera** hello prestanda eller hälsa historiken för din resurs **för efterlevnad och granskning** syften.</span><span class="sxs-lookup"><span data-stu-id="084b7-115">**Archive** hello performance or health history of your resource **for compliance or auditing** purposes.</span></span>

## <a name="what-are-hello-characteristics-of-metrics"></a><span data-ttu-id="084b7-116">Vad är hello egenskaper mått?</span><span class="sxs-lookup"><span data-stu-id="084b7-116">What are hello characteristics of metrics?</span></span>
<span data-ttu-id="084b7-117">Mått har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="084b7-117">Metrics have hello following characteristics:</span></span>

* <span data-ttu-id="084b7-118">Alla mätvärden har **minut frekvens**.</span><span class="sxs-lookup"><span data-stu-id="084b7-118">All metrics have **one-minute frequency**.</span></span> <span data-ttu-id="084b7-119">Du får ett värde varje minut från din resurs så att du nära realtid insyn i hello tillstånd och hälsotillståndet för din resurs.</span><span class="sxs-lookup"><span data-stu-id="084b7-119">You receive a metric value every minute from your resource, giving you near real-time visibility into hello state and health of your resource.</span></span>
* <span data-ttu-id="084b7-120">Mått är **tillgängliga omedelbart**.</span><span class="sxs-lookup"><span data-stu-id="084b7-120">Metrics are **available immediately**.</span></span> <span data-ttu-id="084b7-121">Du inte behöver tooopt i eller konfigurera ytterligare diagnostik.</span><span class="sxs-lookup"><span data-stu-id="084b7-121">You don't need tooopt in or set up additional diagnostics.</span></span>
* <span data-ttu-id="084b7-122">Du kan komma åt **30 dagar från historiken** för varje mått.</span><span class="sxs-lookup"><span data-stu-id="084b7-122">You can access **30 days of history** for each metric.</span></span> <span data-ttu-id="084b7-123">Du kan snabbt se hello senare och månatliga trender i hello prestanda eller hälsotillståndet för din resurs.</span><span class="sxs-lookup"><span data-stu-id="084b7-123">You can quickly look at hello recent and monthly trends in hello performance or health of your resource.</span></span>

<span data-ttu-id="084b7-124">Du kan också:</span><span class="sxs-lookup"><span data-stu-id="084b7-124">You can also:</span></span>

* <span data-ttu-id="084b7-125">Konfigurera ett mått **avisering regel som skickar ett meddelande eller tar automatisk åtgärd** när hello mått korsar hello tröskelvärde som du har angett.</span><span class="sxs-lookup"><span data-stu-id="084b7-125">Configure a metric **alert rule that sends a notification or takes automated action** when hello metric crosses hello threshold that you have set.</span></span> <span data-ttu-id="084b7-126">Autoskala är en automatisk specialåtgärd som aktiverar tooscale ut din resurs toomeet inkommande begäranden eller läses in på webbplatsen eller datorresurser.</span><span class="sxs-lookup"><span data-stu-id="084b7-126">Autoscale is a special automated action that enables you tooscale out your resource toomeet incoming requests or loads on your website or computing resources.</span></span> <span data-ttu-id="084b7-127">Du kan konfigurera en Autoskalningsinställning regel tooscale in eller ut baserat på ett mått som passerar ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="084b7-127">You can configure an Autoscale setting rule tooscale in or out based on a metric crossing a threshold.</span></span>

* <span data-ttu-id="084b7-128">**Väg** alla mått Application Insights eller logganalys (OMS) tooenable instant analytics Sök och anpassade aviseringar på mått data från dina resurser.</span><span class="sxs-lookup"><span data-stu-id="084b7-128">**Route** all metrics Application Insights or Log Analytics (OMS) tooenable instant analytics, search, and custom alerting on metrics data from your resources.</span></span> <span data-ttu-id="084b7-129">Du kan också strömma mått tooan Event Hub, aktiverar du toothen dirigera dem tooAzure Stream Analytics eller toocustom appar för analys i nära realtid.</span><span class="sxs-lookup"><span data-stu-id="084b7-129">You can also stream metrics tooan Event Hub, enabling you toothen route them tooAzure Stream Analytics or toocustom apps for near-real time analysis.</span></span> <span data-ttu-id="084b7-130">Du ställa in Event Hub strömning med diagnostikinställningar.</span><span class="sxs-lookup"><span data-stu-id="084b7-130">You set up Event Hub streaming using diagnostic settings.</span></span>

* <span data-ttu-id="084b7-131">**Arkivera mått toostorage** längre bevarande eller använda dem för offline-rapportering.</span><span class="sxs-lookup"><span data-stu-id="084b7-131">**Archive metrics toostorage** for longer retention or use them for offline reporting.</span></span> <span data-ttu-id="084b7-132">Du kan vidarebefordra dina mått tooAzure Blob storage när du konfigurerar inställningar för diagnostik för din resurs.</span><span class="sxs-lookup"><span data-stu-id="084b7-132">You can route your metrics tooAzure Blob storage when you configure diagnostic settings for your resource.</span></span>

* <span data-ttu-id="084b7-133">Enkelt identifiera, komma åt, och **visa alla** via hello Azure-portalen när du väljer en resurs och rita hello mått i ett diagram.</span><span class="sxs-lookup"><span data-stu-id="084b7-133">Easily discover, access, and **view all metrics** via hello Azure portal when you select a resource and plot hello metrics on a chart.</span></span>

* <span data-ttu-id="084b7-134">**Använda** hello mätvärden via hello nya Azure övervakaren REST API: er.</span><span class="sxs-lookup"><span data-stu-id="084b7-134">**Consume** hello metrics via hello new Azure Monitor REST APIs.</span></span>

* <span data-ttu-id="084b7-135">**Frågan** mått med hjälp av hello PowerShell-cmdlets eller hello plattformsoberoende REST API.</span><span class="sxs-lookup"><span data-stu-id="084b7-135">**Query** metrics by using hello PowerShell cmdlets or hello Cross-Platform REST API.</span></span>

  ![Routning av mätvärden i Azure-Monitor](./media/monitoring-overview-metrics/Metrics_Overview_v4.png)

## <a name="access-metrics-via-hello-portal"></a><span data-ttu-id="084b7-137">Åtkomst till mätvärden via hello-portalen</span><span class="sxs-lookup"><span data-stu-id="084b7-137">Access metrics via hello portal</span></span>
<span data-ttu-id="084b7-138">Följande är en snabb genomgång av hur toocreate ett mått diagram med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="084b7-138">Following is a quick walkthrough of how toocreate a metric chart by using hello Azure portal.</span></span>

### <a name="tooview-metrics-after-creating-a-resource"></a><span data-ttu-id="084b7-139">tooview mått när du har skapat en resurs</span><span class="sxs-lookup"><span data-stu-id="084b7-139">tooview metrics after creating a resource</span></span>
1. <span data-ttu-id="084b7-140">Öppna hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="084b7-140">Open hello Azure portal.</span></span>
2. <span data-ttu-id="084b7-141">Skapa en Azure App Service-webbplats.</span><span class="sxs-lookup"><span data-stu-id="084b7-141">Create an Azure App Service website.</span></span>
3. <span data-ttu-id="084b7-142">När du skapar en webbplats går toohello **översikt** bladet för hello webbplats.</span><span class="sxs-lookup"><span data-stu-id="084b7-142">After you create a website, go toohello **Overview** blade of hello website.</span></span>
4. <span data-ttu-id="084b7-143">Du kan visa nya mått som en **övervakning** panelen.</span><span class="sxs-lookup"><span data-stu-id="084b7-143">You can view new metrics as a **Monitoring** tile.</span></span> <span data-ttu-id="084b7-144">Du kan redigera hello sida vid sida och välja fler mått.</span><span class="sxs-lookup"><span data-stu-id="084b7-144">You can then edit hello tile and select more metrics.</span></span>

   ![Mått på en resurs i Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview1.png)

### <a name="tooaccess-all-metrics-in-a-single-place"></a><span data-ttu-id="084b7-146">tooaccess alla mått i en enda plats</span><span class="sxs-lookup"><span data-stu-id="084b7-146">tooaccess all metrics in a single place</span></span>
1. <span data-ttu-id="084b7-147">Öppna hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="084b7-147">Open hello Azure portal.</span></span>
2. <span data-ttu-id="084b7-148">Navigera toohello nya **övervakaren** fliken och sedan och välj hello **mått** alternativet under.</span><span class="sxs-lookup"><span data-stu-id="084b7-148">Navigate toohello new **Monitor** tab, and then and select hello **Metrics** option underneath it.</span></span>
3. <span data-ttu-id="084b7-149">Välj din prenumeration, resursgrupp och hello namnet på hello resursen hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="084b7-149">Select your subscription, resource group, and hello name of hello resource from hello drop-down list.</span></span>
4. <span data-ttu-id="084b7-150">Visa hello tillgängliga mått lista.</span><span class="sxs-lookup"><span data-stu-id="084b7-150">View hello available metrics list.</span></span> <span data-ttu-id="084b7-151">Välj sedan hello mått du är intresserad av och rita den.</span><span class="sxs-lookup"><span data-stu-id="084b7-151">Then select hello metric you are interested in and plot it.</span></span>
5. <span data-ttu-id="084b7-152">Du kan fästa den toohello instrumentpanelen genom att klicka på hello PIN-kod på hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="084b7-152">You can pin it toohello dashboard by clicking hello pin on hello upper-right corner.</span></span>

   ![Åtkomst till alla mått i en enda plats i Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview2.png)

> [!NOTE]
> <span data-ttu-id="084b7-154">Du kan komma åt värdnivå mått från virtuella datorer (Azure Resource Manager-baserat) och virtuella datorn anger utan några ytterligare inställningar för diagnostik.</span><span class="sxs-lookup"><span data-stu-id="084b7-154">You can access host-level metrics from VMs (Azure Resource Manager-based) and virtual machine scale sets without any additional diagnostic setup.</span></span> <span data-ttu-id="084b7-155">Dessa nya värdnivå mått är tillgängliga för Windows och Linux-instanser.</span><span class="sxs-lookup"><span data-stu-id="084b7-155">These new host-level metrics are available for Windows and Linux instances.</span></span> <span data-ttu-id="084b7-156">De här måtten är inte toobe förväxlas med hello gäst-OS-nivå mått som du har åtkomst toowhen du aktiverar Azure-diagnostik på virtuella datorer eller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="084b7-156">These metrics are not toobe confused with hello Guest-OS-level metrics that you have access toowhen you turn on Azure Diagnostics on your VMs or virtual machine scale sets.</span></span> <span data-ttu-id="084b7-157">toolearn mer om hur du konfigurerar Diagnostics, se [vad är Microsoft Azure-diagnostik](../azure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="084b7-157">toolearn more about configuring Diagnostics, see [What is Microsoft Azure Diagnostics](../azure-diagnostics.md).</span></span>
>
>

## <a name="access-metrics-via-hello-rest-api"></a><span data-ttu-id="084b7-158">Åtkomst till mätvärden via hello REST API</span><span class="sxs-lookup"><span data-stu-id="084b7-158">Access metrics via hello REST API</span></span>
<span data-ttu-id="084b7-159">Azure mått kan nås via hello Azure-Monitor API: er.</span><span class="sxs-lookup"><span data-stu-id="084b7-159">Azure Metrics can be accessed via hello Azure Monitor APIs.</span></span> <span data-ttu-id="084b7-160">Det finns två API: er som hjälper dig identifiera och komma åt mått:</span><span class="sxs-lookup"><span data-stu-id="084b7-160">There are two APIs that help you discover and access metrics:</span></span>

* <span data-ttu-id="084b7-161">Använd hello [mått för Azure-Monitor definitioner REST API](https://msdn.microsoft.com/library/mt743621.aspx) tooaccess hello listan över mått som är tillgängliga för en tjänst.</span><span class="sxs-lookup"><span data-stu-id="084b7-161">Use hello [Azure Monitor Metric definitions REST API](https://msdn.microsoft.com/library/mt743621.aspx) tooaccess hello list of metrics that are available for a service.</span></span>
* <span data-ttu-id="084b7-162">Använd hello [Azure övervakaren mått REST API](https://msdn.microsoft.com/library/mt743622.aspx) tooaccess hello faktiska mätvärdesdata.</span><span class="sxs-lookup"><span data-stu-id="084b7-162">Use hello [Azure Monitor Metrics REST API](https://msdn.microsoft.com/library/mt743622.aspx) tooaccess hello actual metrics data.</span></span>

> [!NOTE]
> <span data-ttu-id="084b7-163">Den här artikeln beskriver hello mätvärden via hello [nya API: et för mått](https://msdn.microsoft.com/library/dn931930.aspx) för Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="084b7-163">This article covers hello metrics via hello [new API for metrics](https://msdn.microsoft.com/library/dn931930.aspx) for Azure resources.</span></span> <span data-ttu-id="084b7-164">hello API-version för hello nya måttdefinitioner API är 2016-03-01 och hello version för mått API är 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="084b7-164">hello API version for hello new metric definitions API is 2016-03-01 and hello version for metrics API is 2016-09-01.</span></span> <span data-ttu-id="084b7-165">hello äldre måttdefinitioner och mått som kan nås med hello API version 2014-04-01.</span><span class="sxs-lookup"><span data-stu-id="084b7-165">hello legacy metric definitions and metrics can be accessed with hello API version 2014-04-01.</span></span>
>
>

<span data-ttu-id="084b7-166">En mer detaljerad genomgång med hello Azure övervakaren REST API: er finns [REST-API för Azure-Monitor genomgången](monitoring-rest-api-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="084b7-166">For a more detailed walkthrough using hello Azure Monitor REST APIs, see [Azure Monitor REST API walkthrough](monitoring-rest-api-walkthrough.md).</span></span>

## <a name="export-metrics"></a><span data-ttu-id="084b7-167">Exportera mått</span><span class="sxs-lookup"><span data-stu-id="084b7-167">Export metrics</span></span>
<span data-ttu-id="084b7-168">Du kan gå toohello **diagnostikinställningarna** bladet under hello **övervakaren** fliken och visa hello exportalternativ för mått.</span><span class="sxs-lookup"><span data-stu-id="084b7-168">You can go toohello **Diagnostics settings** blade under hello **Monitor** tab and view hello export options for metrics.</span></span> <span data-ttu-id="084b7-169">Du kan välja mått (och diagnostikloggar) toobe dirigeras tooBlob lagring, tooAzure Händelsehubbar eller tooOMS för användningsfall som nämndes tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="084b7-169">You can select metrics (and diagnostic logs) toobe routed tooBlob storage, tooAzure Event Hubs, or tooOMS for use-cases that were mentioned previously in this article.</span></span>

 ![Exportalternativ för mätvärden i Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview3.png)

<span data-ttu-id="084b7-171">Du kan konfigurera detta via Resource Manager-mallar [PowerShell](insights-powershell-samples.md), [Azure CLI](insights-cli-samples.md), eller [REST API: er](https://msdn.microsoft.com/library/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="084b7-171">You can configure this via Resource Manager templates, [PowerShell](insights-powershell-samples.md), [Azure CLI](insights-cli-samples.md), or [REST APIs](https://msdn.microsoft.com/library/dn931943.aspx).</span></span>

## <a name="take-action-on-metrics"></a><span data-ttu-id="084b7-172">Utför en åtgärd på mått</span><span class="sxs-lookup"><span data-stu-id="084b7-172">Take action on metrics</span></span>
<span data-ttu-id="084b7-173">Du kan konfigurera Varningsregler eller Autoskala inställningar tooreceive meddelanden eller vidta automatiserade åtgärder på måttdata.</span><span class="sxs-lookup"><span data-stu-id="084b7-173">tooreceive notifications or take automated actions on metric data, you can configure alert rules or Autoscale settings.</span></span>

### <a name="configure-alert-rules"></a><span data-ttu-id="084b7-174">Konfigurera Varningsregler</span><span class="sxs-lookup"><span data-stu-id="084b7-174">Configure alert rules</span></span>
<span data-ttu-id="084b7-175">Du kan konfigurera Varningsregler på mått.</span><span class="sxs-lookup"><span data-stu-id="084b7-175">You can configure alert rules on metrics.</span></span> <span data-ttu-id="084b7-176">Dessa Varningsregler kan kontrollera om ett mått har passerat ett visst tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="084b7-176">These alert rules can check if a metric has crossed a certain threshold.</span></span> <span data-ttu-id="084b7-177">De kan sedan meddela dig via e-post eller starta en webhook som kan använda toorun alla anpassade skript.</span><span class="sxs-lookup"><span data-stu-id="084b7-177">They can then notify you via email or fire a webhook that can be used toorun any custom script.</span></span> <span data-ttu-id="084b7-178">Du kan också använda hello webhook tooconfigure tredjepartsprodukt integreringar.</span><span class="sxs-lookup"><span data-stu-id="084b7-178">You can also use hello webhook tooconfigure third-party product integrations.</span></span>

 ![Mått och Varningsregler i Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale-your-azure-resources"></a><span data-ttu-id="084b7-180">Autoskala din Azure resurser</span><span class="sxs-lookup"><span data-stu-id="084b7-180">Autoscale your Azure resources</span></span>
<span data-ttu-id="084b7-181">Vissa Azure-resurser stöd hello skalning in eller ut av flera instanser toohandle dina arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="084b7-181">Some Azure resources support hello scaling out or in of multiple instances toohandle your workloads.</span></span> <span data-ttu-id="084b7-182">Autoskala gäller tooApp Service (Web Apps), skalningsuppsättningar i virtuella datorer och klassiska Azure-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="084b7-182">Autoscale applies tooApp Service (Web Apps), virtual machine scale sets, and classic Azure Cloud Services.</span></span> <span data-ttu-id="084b7-183">Du kan konfigurera automatiska regler tooscale in eller ut när ett mått som påverkar din arbetsbelastning överskrider ett tröskelvärde som du anger.</span><span class="sxs-lookup"><span data-stu-id="084b7-183">You can configure Autoscale rules tooscale out or in when a certain metric that impacts your workload crosses a threshold that you specify.</span></span> <span data-ttu-id="084b7-184">Mer information finns i [översikt över autoskalning](monitoring-overview-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="084b7-184">For more information, see [Overview of autoscaling](monitoring-overview-autoscale.md).</span></span>

 ![Mått och Autoskala i Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="learn-about-supported-services-and-metrics"></a><span data-ttu-id="084b7-186">Lär dig mer om tjänster som stöds och mått</span><span class="sxs-lookup"><span data-stu-id="084b7-186">Learn about supported services and metrics</span></span>
<span data-ttu-id="084b7-187">Azure övervakaren är en ny infrastruktur för mått.</span><span class="sxs-lookup"><span data-stu-id="084b7-187">Azure Monitor is a new metrics infrastructure.</span></span> <span data-ttu-id="084b7-188">Den stöder hello följande Azure-tjänster i hello Azure-portalen och hello ny version av hello Azure övervakaren API:</span><span class="sxs-lookup"><span data-stu-id="084b7-188">It supports hello following Azure services in hello Azure portal and hello new version of hello Azure Monitor API:</span></span>

* <span data-ttu-id="084b7-189">Virtuella datorer (Azure Resource Manager-baserade)</span><span class="sxs-lookup"><span data-stu-id="084b7-189">VMs (Azure Resource Manager-based)</span></span>
* <span data-ttu-id="084b7-190">Skalningsuppsättningar för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="084b7-190">Virtual machine scale sets</span></span>
* <span data-ttu-id="084b7-191">Batch</span><span class="sxs-lookup"><span data-stu-id="084b7-191">Batch</span></span>
* <span data-ttu-id="084b7-192">Event Hubs namnområde</span><span class="sxs-lookup"><span data-stu-id="084b7-192">Event Hubs namespace</span></span>
* <span data-ttu-id="084b7-193">Service Bus-namnrymd (endast premium SKU)</span><span class="sxs-lookup"><span data-stu-id="084b7-193">Service Bus namespace (premium SKU only)</span></span>
* <span data-ttu-id="084b7-194">SQL-databas (version 12)</span><span class="sxs-lookup"><span data-stu-id="084b7-194">SQL Database (version 12)</span></span>
* <span data-ttu-id="084b7-195">Elastisk SQL-poolen</span><span class="sxs-lookup"><span data-stu-id="084b7-195">Elastic SQL Pool</span></span>
* <span data-ttu-id="084b7-196">Webbplatser</span><span class="sxs-lookup"><span data-stu-id="084b7-196">Websites</span></span>
* <span data-ttu-id="084b7-197">Webbservergrupper</span><span class="sxs-lookup"><span data-stu-id="084b7-197">Web server farms</span></span>
* <span data-ttu-id="084b7-198">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="084b7-198">Logic Apps</span></span>
* <span data-ttu-id="084b7-199">IoT-hubbar</span><span class="sxs-lookup"><span data-stu-id="084b7-199">IoT hubs</span></span>
* <span data-ttu-id="084b7-200">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="084b7-200">Redis Cache</span></span>
* <span data-ttu-id="084b7-201">Nätverk: Programgatewayer</span><span class="sxs-lookup"><span data-stu-id="084b7-201">Networking: Application gateways</span></span>
* <span data-ttu-id="084b7-202">Söka</span><span class="sxs-lookup"><span data-stu-id="084b7-202">Search</span></span>

<span data-ttu-id="084b7-203">Du kan visa en detaljerad lista över alla hello stöds tjänster och deras mått på [Azure-Monitor mått--stöds mått per resurstypen](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="084b7-203">You can view a detailed list of all hello supported services and their metrics at [Azure Monitor metrics--supported metrics per resource type](monitoring-supported-metrics.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="084b7-204">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="084b7-204">Next steps</span></span>
<span data-ttu-id="084b7-205">Läs toohello länkar i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="084b7-205">Refer toohello links throughout this article.</span></span> <span data-ttu-id="084b7-206">Dessutom lär dig mer om:</span><span class="sxs-lookup"><span data-stu-id="084b7-206">Additionally, learn about:</span></span>  

* [<span data-ttu-id="084b7-207">Vanliga mätvärden för autoskalning</span><span class="sxs-lookup"><span data-stu-id="084b7-207">Common metrics for autoscaling</span></span>](insights-autoscale-common-metrics.md)
* [<span data-ttu-id="084b7-208">Hur toocreate Varningsregler</span><span class="sxs-lookup"><span data-stu-id="084b7-208">How toocreate alert rules</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="084b7-209">Analysera loggar från Azure storage med logganalys</span><span class="sxs-lookup"><span data-stu-id="084b7-209">Analyze logs from Azure storage with Log Analytics</span></span>](../log-analytics/log-analytics-azure-storage.md)
