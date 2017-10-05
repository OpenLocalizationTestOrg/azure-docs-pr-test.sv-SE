---
title: "Så här övervakar du ett Azure Storage-konto | Microsoft Docs"
description: "Lär dig hur du övervakar ett lagringskonto i Azure med hjälp av Azure portal."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: b83cba7b-4627-4ba7-b5d0-f1039fe30e78
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: marsma
ms.openlocfilehash: e8fbc4ecdffe62806019f494e1412cfedbccf71f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="monitor-a-storage-account-in-the-azure-portal"></a><span data-ttu-id="5103c-103">Övervaka ett lagringskonto i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5103c-103">Monitor a storage account in the Azure portal</span></span>

<span data-ttu-id="5103c-104">[Azure Storage Analytics](../storage-analytics.md) ger mått för alla lagringstjänster och loggar för BLOB, köer och tabeller.</span><span class="sxs-lookup"><span data-stu-id="5103c-104">[Azure Storage Analytics](../storage-analytics.md) provides metrics for all storage services, and logs for blobs, queues, and tables.</span></span> <span data-ttu-id="5103c-105">Du kan använda den [Azure-portalen](https://portal.azure.com) konfigurera vilka mått och loggar in för ditt konto och konfigurera diagram som ger visuella representationer av mått-data.</span><span class="sxs-lookup"><span data-stu-id="5103c-105">You can use the [Azure portal](https://portal.azure.com) to configure which metrics and logs are recorded for your account, and configure charts that provide visual representations of your metrics data.</span></span>

> [!NOTE]
> <span data-ttu-id="5103c-106">Det finns kostnader i samband med att undersöka övervakningsdata i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5103c-106">There are costs associated with examining monitoring data in the Azure portal.</span></span> <span data-ttu-id="5103c-107">Mer information finns i [Storage Analytics och fakturering för](/rest/api/storageservices/Storage-Analytics-and-Billing).</span><span class="sxs-lookup"><span data-stu-id="5103c-107">For more information, see [Storage Analytics and Billing](/rest/api/storageservices/Storage-Analytics-and-Billing).</span></span>
>
> <span data-ttu-id="5103c-108">Azure File storage kan du för närvarande stöder Storage Analytics mätvärden, men ännu stöder inte loggning.</span><span class="sxs-lookup"><span data-stu-id="5103c-108">Azure File storage currently supports Storage Analytics metrics, but does not yet support logging.</span></span>
>
> <span data-ttu-id="5103c-109">Storage-konton med en typ av replikering av Zonredundant lagring (ZRS) för närvarande har inte mått eller loggningsfunktioner aktiverad.</span><span class="sxs-lookup"><span data-stu-id="5103c-109">Storage accounts with a replication type of Zone-Redundant Storage (ZRS) currently do not have the metrics or logging capability enabled.</span></span>
> 
> <span data-ttu-id="5103c-110">En detaljerad vägledning om använder Storage Analytics och andra verktyg för att identifiera, diagnostisera och felsöka problem med Azure Storage finns [övervaka, diagnostisera och felsöka Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="5103c-110">For an in-depth guide on using Storage Analytics and other tools to identify, diagnose, and troubleshoot Azure Storage-related issues, see [Monitor, diagnose, and troubleshoot Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).</span></span>
>

## <a name="configure-monitoring-for-a-storage-account"></a><span data-ttu-id="5103c-111">Konfigurera övervakning för ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="5103c-111">Configure monitoring for a storage account</span></span>

1. <span data-ttu-id="5103c-112">I den [Azure-portalen](https://portal.azure.com)väljer **lagringskonton**, sedan lagringskontonamnet att öppna instrumentpanelen konto.</span><span class="sxs-lookup"><span data-stu-id="5103c-112">In the [Azure portal](https://portal.azure.com), select **Storage accounts**, then the storage account name to open the account dashboard.</span></span>
1. <span data-ttu-id="5103c-113">Välj **diagnostik** i den **övervakning** på menyn bladet.</span><span class="sxs-lookup"><span data-stu-id="5103c-113">Select **Diagnostics** in the **MONITORING** section of the menu blade.</span></span>

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)

1. <span data-ttu-id="5103c-115">Välj den **typen** av mätvärdesdata för varje **service** du vill övervaka, och **bevarandeprincip** för data.</span><span class="sxs-lookup"><span data-stu-id="5103c-115">Select the **type** of metrics data for each **service** you wish to monitor, and the **retention policy** for the data.</span></span> <span data-ttu-id="5103c-116">Du kan också inaktivera övervakning genom att ange **Status** till **av**.</span><span class="sxs-lookup"><span data-stu-id="5103c-116">You can also disable monitoring by setting **Status** to **Off**.</span></span>

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-01.png)

   <span data-ttu-id="5103c-118">Det finns två typer av mått som du kan aktivera för varje tjänst som är aktiverad som standard för nya storage-konton:</span><span class="sxs-lookup"><span data-stu-id="5103c-118">There are two types of metrics you can enable for each service, both of which are enabled by default for new storage accounts:</span></span>

   * <span data-ttu-id="5103c-119">**Sammanställd**: samlar in mätvärden, till exempel ingång-/ utgång, tillgänglighet, svarstid och lyckade procenttal.</span><span class="sxs-lookup"><span data-stu-id="5103c-119">**Aggregate**: Collects metrics such as ingress/egress, availability, latency, and success percentages.</span></span> <span data-ttu-id="5103c-120">De här måtten samman för blob, kön, tabell och Filtjänster.</span><span class="sxs-lookup"><span data-stu-id="5103c-120">These metrics are aggregated for the blob, queue, table, and file services.</span></span>
   * <span data-ttu-id="5103c-121">**Per API**: förutom sammanställd statistik, samlar in samma uppsättning mätvärden för varje Lagringsåtgärden i Azure Storage service API.</span><span class="sxs-lookup"><span data-stu-id="5103c-121">**Per API**: In addition to the aggregate metrics, collects the same set of metrics for each storage operation in the Azure Storage service API.</span></span>

   <span data-ttu-id="5103c-122">Ange databevarandeprincip genom att flytta den **bevarande (dagar)** skjutreglaget eller ange antal dagar som data ska bevaras från 1 till 365.</span><span class="sxs-lookup"><span data-stu-id="5103c-122">To set the data retention policy, move the **Retention (days)** slider or enter the number of days of data to retain, from 1 to 365.</span></span> <span data-ttu-id="5103c-123">Standard för nya storage-konton är sju dagar.</span><span class="sxs-lookup"><span data-stu-id="5103c-123">The default for new storage accounts is seven days.</span></span> <span data-ttu-id="5103c-124">Om du inte vill ange en bevarandeprincip ange noll.</span><span class="sxs-lookup"><span data-stu-id="5103c-124">If you do not want to set a retention policy, enter zero.</span></span> <span data-ttu-id="5103c-125">Om det finns inga bevarandeprincip, är det att ta bort övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="5103c-125">If there is no retention policy, it is up to you to delete the monitoring data.</span></span>

   > [!WARNING]
   > <span data-ttu-id="5103c-126">Du debiteras när du manuellt ta bort mått data.</span><span class="sxs-lookup"><span data-stu-id="5103c-126">You are charged when you manually delete metrics data.</span></span> <span data-ttu-id="5103c-127">Inaktuella analysdata (data som är äldre än din bevarandeprincip) tas bort av systemet utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="5103c-127">Stale analytics data (data older than your retention policy) is deleted by the system at no cost.</span></span> <span data-ttu-id="5103c-128">Vi rekommenderar en bevarandeprincip baserat på hur länge du vill behålla storage analytics-data för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="5103c-128">We recommend setting a retention policy based on how long you want to retain storage analytics data for your account.</span></span> <span data-ttu-id="5103c-129">Se [vilka avgifter gör uppkommer när du aktiverar storage-mätvärden?](../common/storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) för mer information.</span><span class="sxs-lookup"><span data-stu-id="5103c-129">See [What charges do you incur when you enable storage metrics?](../common/storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) for more information.</span></span>
   >

1. <span data-ttu-id="5103c-130">När du är klar med konfigurationen av övervakningen väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="5103c-130">When you finish the monitoring configuration, select **Save**.</span></span>

<span data-ttu-id="5103c-131">En standarduppsättning mått visas i diagram på bladet storage-konto som enskild tjänst blad (blob, kön, tabell och filen).</span><span class="sxs-lookup"><span data-stu-id="5103c-131">A default set of metrics is displayed in charts on the storage account blade, as well as the individual service blades (blob, queue, table, and file).</span></span> <span data-ttu-id="5103c-132">När du har aktiverat mått för en tjänst, kan det ta upp till en timme innan data ska visas i dess diagram.</span><span class="sxs-lookup"><span data-stu-id="5103c-132">Once you've enabled metrics for a service, it may take up to an hour for data to appear in its charts.</span></span> <span data-ttu-id="5103c-133">Du kan välja **redigera** på mått diagram till [konfigurera vilka mått](#how-to-customize-metrics-charts) visas i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="5103c-133">You can select **Edit** on any metric chart to [configure which metrics](#how-to-customize-metrics-charts) are displayed in the chart.</span></span>

<span data-ttu-id="5103c-134">Du kan inaktivera insamling av mätvärden och loggning genom att ange **Status** till **av**.</span><span class="sxs-lookup"><span data-stu-id="5103c-134">You can disable metrics collection and logging by setting **Status** to **Off**.</span></span>

> [!NOTE]
> <span data-ttu-id="5103c-135">Azure Storage använder [tabell lagring](../common/storage-introduction.md#table-storage) att lagra mätvärden för ditt lagringskonto och lagrar mätvärdena i tabeller i ditt konto.</span><span class="sxs-lookup"><span data-stu-id="5103c-135">Azure Storage uses [table storage](../common/storage-introduction.md#table-storage) to store the metrics for your storage account, and stores the metrics in tables in your account.</span></span> <span data-ttu-id="5103c-136">Mer information finns i.</span><span class="sxs-lookup"><span data-stu-id="5103c-136">For more information, see.</span></span> <span data-ttu-id="5103c-137">[Hur mått lagras](../common/storage-analytics.md#how-metrics-are-stored).</span><span class="sxs-lookup"><span data-stu-id="5103c-137">[How metrics are stored](../common/storage-analytics.md#how-metrics-are-stored).</span></span>
>

## <a name="customize-metrics-charts"></a><span data-ttu-id="5103c-138">Anpassa mått diagram</span><span class="sxs-lookup"><span data-stu-id="5103c-138">Customize metrics charts</span></span>

<span data-ttu-id="5103c-139">Använd följande procedur för att välja vilka lagring mått att visa i ett diagram för mått.</span><span class="sxs-lookup"><span data-stu-id="5103c-139">Use the following procedure to choose which storage metrics to view in a metrics chart.</span></span> 

1. <span data-ttu-id="5103c-140">Starta genom att visa ett mått diagram för lagring i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5103c-140">Start by displaying a storage metric chart in the Azure portal.</span></span> <span data-ttu-id="5103c-141">Du hittar diagram på den **lagring kontoblad** och i den **mått** bladet för en enskild tjänst (blob, kön, tabell, fil).</span><span class="sxs-lookup"><span data-stu-id="5103c-141">You can find charts on the **storage account blade** and in the **Metrics** blade for an individual service (blob, queue, table, file).</span></span>

   <span data-ttu-id="5103c-142">I det här exemplet vi arbetar med följande diagram som visas på den **lagring kontoblad**:</span><span class="sxs-lookup"><span data-stu-id="5103c-142">In this example, we work with the following chart that appears on the **storage account blade**:</span></span>

   ![Val av diagram i Azure-portalen](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. <span data-ttu-id="5103c-144">Klicka någonstans i diagrammet för att öppna den **mått** bladet.</span><span class="sxs-lookup"><span data-stu-id="5103c-144">Next, click anywhere within the chart to open the **Metric** blade.</span></span> <span data-ttu-id="5103c-145">Välj **redigera diagram** att öppna den **redigera diagram** bladet.</span><span class="sxs-lookup"><span data-stu-id="5103c-145">Select **Edit chart** to open the **Edit Chart** blade.</span></span>

   ![Redigera knappen på bladet för diagram](./media/storage-monitor-storage-account/stg-customize-chart-01.png)

1. <span data-ttu-id="5103c-147">På den **redigera diagram** bladet väljer den **tidsintervall** mätvärden ska visas i diagrammet, och **service** (blob, kö, tabell, filen) vars mått som du vill visa.</span><span class="sxs-lookup"><span data-stu-id="5103c-147">On the **Edit Chart** blade, select the **Time Range** of the metrics to display in the chart, and the **service** (blob, queue, table, file) whose metrics you wish to display.</span></span> <span data-ttu-id="5103c-148">Här har vi valt för att visa den senaste veckan mätvärden för blob-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="5103c-148">Here we've selected to display the past week's metrics for the blob service:</span></span>

   ![Intervallet och tjänsten valet av tid i bladet redigera diagram](./media/storage-monitor-storage-account/stg-customize-chart-02.png)

1. <span data-ttu-id="5103c-150">Välj enskilda **mått** du hade som visas i diagrammet och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5103c-150">Select the individual **metrics** you'd like displayed in the chart, then click **OK**.</span></span> <span data-ttu-id="5103c-151">Till exempel här vi har valt att visa den *ContainerCount* och *ObjectCount* mått:</span><span class="sxs-lookup"><span data-stu-id="5103c-151">For example, here we've chosen to display the *ContainerCount* and *ObjectCount* metrics:</span></span>

   ![Enskilda mått val i bladet redigera diagram](./media/storage-monitor-storage-account/stg-customize-chart-03.png)

<span data-ttu-id="5103c-153">Diagrammet inställningarna påverkar inte samling, sammanställning och lagring av övervakningsdata i lagringskonto endast visning av mätvärdesdata.</span><span class="sxs-lookup"><span data-stu-id="5103c-153">Your chart settings do not affect the collection, aggregation, or storage of monitoring data in the storage account, only the viewing of metrics data.</span></span>

### <a name="metrics-availability-in-charts"></a><span data-ttu-id="5103c-154">Mått tillgänglighet i diagram</span><span class="sxs-lookup"><span data-stu-id="5103c-154">Metrics availability in charts</span></span>

<span data-ttu-id="5103c-155">Listan över tillgängliga mått ändras baserat på vilken tjänst som du har valt i listrutan och enhetstypen för diagrammet som du redigerar.</span><span class="sxs-lookup"><span data-stu-id="5103c-155">The list of available metrics changes based on which service you've chosen in the drop-down, and the unit type of the chart you're editing.</span></span> <span data-ttu-id="5103c-156">Du kan till exempel välja procentandel mått som *PercentNetworkError* och *PercentThrottlingError* bara om du redigerar ett diagram som visar enheter i procent:</span><span class="sxs-lookup"><span data-stu-id="5103c-156">For example, you can select percentage metrics like *PercentNetworkError* and *PercentThrottlingError* only if you're editing a chart that displays units in percentage:</span></span>

![Begäran fel procentandel diagram i Azure-portalen](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a><span data-ttu-id="5103c-158">Mått upplösning</span><span class="sxs-lookup"><span data-stu-id="5103c-158">Metrics resolution</span></span>

<span data-ttu-id="5103c-159">Mått som du har valt i diagnostik avgör upplösning för de mätvärden som är tillgängliga för ditt konto:</span><span class="sxs-lookup"><span data-stu-id="5103c-159">The metrics you selected in Diagnostics determines the resolution of the metrics that are available for your account:</span></span>

* <span data-ttu-id="5103c-160">**Sammanställd** övervakning innehåller mått, till exempel ingång-/ utgång, tillgänglighet, svarstid och lyckade procenttal.</span><span class="sxs-lookup"><span data-stu-id="5103c-160">**Aggregate** monitoring provides metrics such as ingress/egress, availability, latency, and success percentages.</span></span> <span data-ttu-id="5103c-161">De här måtten samman från blob-, tabell-, fil- och kötjänster.</span><span class="sxs-lookup"><span data-stu-id="5103c-161">These metrics are aggregated from the blob, table, file, and queue services.</span></span>
* <span data-ttu-id="5103c-162">**Per API** ger ökad upplösning med mått som är tillgängliga för enskilda lagringsåtgärder utöver servicenivåer mängder.</span><span class="sxs-lookup"><span data-stu-id="5103c-162">**Per API** provides finer resolution, with metrics available for individual storage operations, in addition to the service-level aggregates.</span></span>

## <a name="configure-metrics-alerts"></a><span data-ttu-id="5103c-163">Konfigurera aviseringar för mått</span><span class="sxs-lookup"><span data-stu-id="5103c-163">Configure metrics alerts</span></span>

<span data-ttu-id="5103c-164">Du kan skapa varningar som meddelar dig när tröskelvärdet har uppnåtts för storage resource mått.</span><span class="sxs-lookup"><span data-stu-id="5103c-164">You can create alerts to notify you when thresholds have been reached for storage resource metrics.</span></span>

1. <span data-ttu-id="5103c-165">Öppna den **Varningsregler bladet**, rulla ned till den **övervakning** avsnitt i den **menyn bladet** och välj **Varna regler**.</span><span class="sxs-lookup"><span data-stu-id="5103c-165">To open the **Alert rules blade**, scroll down to the **MONITORING** section of the **Menu blade** and select **Alert rules**.</span></span>
1. <span data-ttu-id="5103c-166">Välj **Lägg till avisering** att öppna den **lägga till en varningsregel** bladet</span><span class="sxs-lookup"><span data-stu-id="5103c-166">Select **Add alert** to open the **Add an alert rule** blade</span></span>
1. <span data-ttu-id="5103c-167">Välj en **resurs** (blob, fil, kö, tabell) i listrutan och ange en **namn** och **beskrivning** för din nya varningsregel.</span><span class="sxs-lookup"><span data-stu-id="5103c-167">Select a **Resource** (blob, file, queue, table) from the drop-down, and enter a **Name** and **Description** for your new alert rule.</span></span>
1. <span data-ttu-id="5103c-168">Välj den **mått** för vilket du vill lägga till en varning och en avisering **villkoret**, och en **tröskelvärdet**.</span><span class="sxs-lookup"><span data-stu-id="5103c-168">Select the **Metric** for which you'd like to add an alert, an alert **Condition**, and a **Threshold**.</span></span> <span data-ttu-id="5103c-169">Tröskelvärde för enheten skriver ändras beroende på det mått som du har valt.</span><span class="sxs-lookup"><span data-stu-id="5103c-169">The threshold unit type changes depending on the metric you've chosen.</span></span> <span data-ttu-id="5103c-170">Till exempel ”antal” är enhetstypen av för *ContainerCount*, medan enhet för den *PercentNetworkError* mått är en del.</span><span class="sxs-lookup"><span data-stu-id="5103c-170">For example, "count" is the unit type for *ContainerCount*, while the unit for the *PercentNetworkError* metric is a percentage.</span></span>
1. <span data-ttu-id="5103c-171">Välj den **Period**.</span><span class="sxs-lookup"><span data-stu-id="5103c-171">Select the **Period**.</span></span> <span data-ttu-id="5103c-172">Mått som når eller överskrider tröskeln inom tiden utlösa en avisering.</span><span class="sxs-lookup"><span data-stu-id="5103c-172">Metrics that reach or exceed the Threshold within the period trigger an alert.</span></span>
1. <span data-ttu-id="5103c-173">(Valfritt) Konfigurera **e-post** och **Webhook** meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5103c-173">(Optional) Configure **Email** and **Webhook** notifications.</span></span> <span data-ttu-id="5103c-174">Mer information om webhooks finns [konfigurera en webhook på en Azure mått avisering](../../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="5103c-174">For more information on webhooks, see [Configure a webhook on an Azure metric alert](../../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span> <span data-ttu-id="5103c-175">Om du inte konfigurerar e-post eller webhook meddelanden visas aviseringar endast i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5103c-175">If you do not configure email or webhook notifications, alerts will appear only in the Azure portal.</span></span>

![”Lägg till en varningsregel-bladet i Azure-portalen](./media/storage-monitor-storage-account/stg-alert-rules-01.png)

## <a name="add-metrics-charts-to-the-portal-dashboard"></a><span data-ttu-id="5103c-177">Lägga till mätvärden diagram i portalens instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="5103c-177">Add metrics charts to the portal dashboard</span></span>

<span data-ttu-id="5103c-178">Du kan lägga till Azure Storage metrics diagram för någon av dina lagringskonton instrumentpanelen i portalen.</span><span class="sxs-lookup"><span data-stu-id="5103c-178">You can add Azure Storage metrics charts for any of your storage accounts to your portal dashboard.</span></span>

1. <span data-ttu-id="5103c-179">Välj Klicka **redigera instrumentpanel** när du visar instrumentpanelen i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5103c-179">Select click **Edit dashboard** while viewing your dashboard in the [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="5103c-180">I den **panelen galleriet**väljer **hitta panelerna genom** > **typen**.</span><span class="sxs-lookup"><span data-stu-id="5103c-180">In the **Tile Gallery**, select **Find tiles by** > **Type**.</span></span>
1. <span data-ttu-id="5103c-181">Välj **typen** > **lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="5103c-181">Select **Type** > **Storage accounts**.</span></span>
1. <span data-ttu-id="5103c-182">I **resurser**, Välj lagringskonto vars mått som du vill lägga till på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="5103c-182">In **Resources**, select the storage account whose metrics you wish to add to the dashboard.</span></span>
1. <span data-ttu-id="5103c-183">Välj **kategorier** > **övervakning**.</span><span class="sxs-lookup"><span data-stu-id="5103c-183">Select **Categories** > **Monitoring**.</span></span>
1. <span data-ttu-id="5103c-184">Dra och släpp diagrammet panelen på instrumentpanelen för måttet som visas.</span><span class="sxs-lookup"><span data-stu-id="5103c-184">Drag-and-drop the chart tile onto your dashboard for the metric you'd like displayed.</span></span> <span data-ttu-id="5103c-185">Upprepa för alla mått som visas på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="5103c-185">Repeat for all metrics you'd like displayed on the dashboard.</span></span> <span data-ttu-id="5103c-186">Diagrammet ”BLOB - Totalt antal begäranden för” är markerad som ett exempel i följande bild, men alla diagram är tillgängliga för placering på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="5103c-186">In the following image, the "Blobs - Total requests" chart is highlighted as an example, but all the charts are available for placement on your dashboard.</span></span>

   ![Panelen galleri i Azure-portalen](./media/storage-monitor-storage-account/stg-customize-dashboard-01.png)
1. <span data-ttu-id="5103c-188">Välj **klar anpassa** överst på instrumentpanelen när du är klar att lägga till diagram.</span><span class="sxs-lookup"><span data-stu-id="5103c-188">Select **Done customizing** near the top of the dashboard when you're done adding charts.</span></span>

<span data-ttu-id="5103c-189">När du har lagt till diagram på instrumentpanelen, kan du anpassa dem enligt beskrivningen i [anpassa mått diagram](#how-to-customize-metrics-charts).</span><span class="sxs-lookup"><span data-stu-id="5103c-189">Once you've added charts to your dashboard, you can further customize them as described in [Customize metrics charts](#how-to-customize-metrics-charts).</span></span>

## <a name="configure-logging"></a><span data-ttu-id="5103c-190">Konfigurera loggning</span><span class="sxs-lookup"><span data-stu-id="5103c-190">Configure logging</span></span>

<span data-ttu-id="5103c-191">Du kan instruera Azure Storage spara diagnostik för läsa, skriva och ta bort begäranden för blob-, tabell- och queue-tjänster.</span><span class="sxs-lookup"><span data-stu-id="5103c-191">You can instruct Azure Storage to save diagnostics logs for read, write, and delete requests for the blob, table, and queue services.</span></span> <span data-ttu-id="5103c-192">Databevarandeprincip som du anger gäller även dessa loggar.</span><span class="sxs-lookup"><span data-stu-id="5103c-192">The data retention policy you set also applies to these logs.</span></span>

> [!NOTE]
> <span data-ttu-id="5103c-193">Azure File storage kan du för närvarande stöder Storage Analytics mätvärden, men ännu stöder inte loggning.</span><span class="sxs-lookup"><span data-stu-id="5103c-193">Azure File storage currently supports Storage Analytics metrics, but does not yet support logging.</span></span>
>

1. <span data-ttu-id="5103c-194">I den [Azure-portalen](https://portal.azure.com)väljer **lagringskonton**, sedan namnet på lagringskontot för att öppna bladet storage-konto.</span><span class="sxs-lookup"><span data-stu-id="5103c-194">In the [Azure portal](https://portal.azure.com), select **Storage accounts**, then the name of the storage account to open the storage account blade.</span></span>
1. <span data-ttu-id="5103c-195">Välj **diagnostik** i den **övervakning** på menyn bladet.</span><span class="sxs-lookup"><span data-stu-id="5103c-195">Select **Diagnostics** in the **MONITORING** section of the menu blade.</span></span>

    ![Diagnostik menyalternativet under övervakning i Azure-portalen.](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)
    
1. <span data-ttu-id="5103c-197">Se till att **Status** är inställd på **på**, och välj den **services** för vilken du vill aktivera loggning.</span><span class="sxs-lookup"><span data-stu-id="5103c-197">Ensure **Status** is set to **On**, and select the **services** for which you'd like to enable logging.</span></span>

    ![Konfigurera loggning i Azure-portalen.](./media/storage-monitor-storage-account/stg-enable-logging-01.png)
1. <span data-ttu-id="5103c-199">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5103c-199">Click **Save**.</span></span>

<span data-ttu-id="5103c-200">Diagnostik-loggar sparas i en blobbbehållare med namnet $logs i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="5103c-200">The diagnostics logs are saved in a blob container named $logs in your storage account.</span></span> <span data-ttu-id="5103c-201">Du kan visa loggdata med en lagringsutforskare som den [Microsoft Lagringsutforskaren](http://storageexplorer.com), eller via programmering med storage-klientbibliotek eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5103c-201">You can view the log data using a storage explorer like the [Microsoft Storage Explorer](http://storageexplorer.com), or programmatically using the storage client library or PowerShell.</span></span>

<span data-ttu-id="5103c-202">Information om åtkomst till behållaren $logs finns [aktivera loggning för lagring och åtkomst till loggdata](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).</span><span class="sxs-lookup"><span data-stu-id="5103c-202">For information about accessing the $logs container, see [Enabling Storage Logging and Accessing Log Data](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5103c-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5103c-203">Next steps</span></span>

* <span data-ttu-id="5103c-204">Hitta mer information om [statistik, loggning och fakturering](../storage-analytics.md) för Storage Analytics.</span><span class="sxs-lookup"><span data-stu-id="5103c-204">Find more details about [metrics, logging, and billing](../storage-analytics.md) for Storage Analytics.</span></span>
* <span data-ttu-id="5103c-205">[Aktivera Azure Storage-mätvärden och visa mätvärdesdata](../storage-enable-and-view-metrics.md) med hjälp av PowerShell och genom programmering med C#.</span><span class="sxs-lookup"><span data-stu-id="5103c-205">[Enable Azure Storage metrics and view metrics data](../storage-enable-and-view-metrics.md) by using PowerShell and programmatically with C#.</span></span>
