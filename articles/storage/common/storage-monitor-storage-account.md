---
title: aaaHow toomonitor ett Azure Storage-konto | Microsoft Docs
description: "Lär dig hur toomonitor ett lagringskonto i Azure med hjälp av hello Azure-portalen."
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
ms.openlocfilehash: 9a939e0b5db687c1b7b7857399321f681df2056a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-storage-account-in-hello-azure-portal"></a><span data-ttu-id="070a0-103">Övervaka ett lagringskonto i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="070a0-103">Monitor a storage account in hello Azure portal</span></span>

<span data-ttu-id="070a0-104">[Azure Storage Analytics](../storage-analytics.md) ger mått för alla lagringstjänster och loggar för BLOB, köer och tabeller.</span><span class="sxs-lookup"><span data-stu-id="070a0-104">[Azure Storage Analytics](../storage-analytics.md) provides metrics for all storage services, and logs for blobs, queues, and tables.</span></span> <span data-ttu-id="070a0-105">Du kan använda hello [Azure-portalen](https://portal.azure.com) tooconfigure vilka mått och loggar för ditt konto har registrerats och konfigurera diagram som ger visuella representationer av mått-data.</span><span class="sxs-lookup"><span data-stu-id="070a0-105">You can use hello [Azure portal](https://portal.azure.com) tooconfigure which metrics and logs are recorded for your account, and configure charts that provide visual representations of your metrics data.</span></span>

> [!NOTE]
> <span data-ttu-id="070a0-106">Det finns kostnader i samband med att undersöka övervakningsdata i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="070a0-106">There are costs associated with examining monitoring data in hello Azure portal.</span></span> <span data-ttu-id="070a0-107">Mer information finns i [Storage Analytics och fakturering för](/rest/api/storageservices/Storage-Analytics-and-Billing).</span><span class="sxs-lookup"><span data-stu-id="070a0-107">For more information, see [Storage Analytics and Billing](/rest/api/storageservices/Storage-Analytics-and-Billing).</span></span>
>
> <span data-ttu-id="070a0-108">Azure File storage kan du för närvarande stöder Storage Analytics mätvärden, men ännu stöder inte loggning.</span><span class="sxs-lookup"><span data-stu-id="070a0-108">Azure File storage currently supports Storage Analytics metrics, but does not yet support logging.</span></span>
>
> <span data-ttu-id="070a0-109">Storage-konton med en typ av replikering av Zonredundant lagring (ZRS) för närvarande har inte hello mått eller loggningsfunktioner aktiverad.</span><span class="sxs-lookup"><span data-stu-id="070a0-109">Storage accounts with a replication type of Zone-Redundant Storage (ZRS) currently do not have hello metrics or logging capability enabled.</span></span>
> 
> <span data-ttu-id="070a0-110">Utförliga instruktioner om hur du använder Storage Analytics och andra verktyg tooidentify, diagnostisera i, och felsöka problem med Azure Storage, se [övervaka, diagnostisera och felsöka Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="070a0-110">For an in-depth guide on using Storage Analytics and other tools tooidentify, diagnose, and troubleshoot Azure Storage-related issues, see [Monitor, diagnose, and troubleshoot Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).</span></span>
>

## <a name="configure-monitoring-for-a-storage-account"></a><span data-ttu-id="070a0-111">Konfigurera övervakning för ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="070a0-111">Configure monitoring for a storage account</span></span>

1. <span data-ttu-id="070a0-112">I hello [Azure-portalen](https://portal.azure.com)väljer **lagringskonton**, och sedan hello konto namn tooopen hello konto lagringsinstrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="070a0-112">In hello [Azure portal](https://portal.azure.com), select **Storage accounts**, then hello storage account name tooopen hello account dashboard.</span></span>
1. <span data-ttu-id="070a0-113">Välj **diagnostik** i hello **övervakning** avsnitt i hello menyn bladet.</span><span class="sxs-lookup"><span data-stu-id="070a0-113">Select **Diagnostics** in hello **MONITORING** section of hello menu blade.</span></span>

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)

1. <span data-ttu-id="070a0-115">Välj hello **typen** av mätvärdesdata för varje **service** du vill toomonitor och hello **bevarandeprincip** för hello data.</span><span class="sxs-lookup"><span data-stu-id="070a0-115">Select hello **type** of metrics data for each **service** you wish toomonitor, and hello **retention policy** for hello data.</span></span> <span data-ttu-id="070a0-116">Du kan också inaktivera övervakning genom att ange **Status** för**av**.</span><span class="sxs-lookup"><span data-stu-id="070a0-116">You can also disable monitoring by setting **Status** too**Off**.</span></span>

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-01.png)

   <span data-ttu-id="070a0-118">Det finns två typer av mått som du kan aktivera för varje tjänst som är aktiverad som standard för nya storage-konton:</span><span class="sxs-lookup"><span data-stu-id="070a0-118">There are two types of metrics you can enable for each service, both of which are enabled by default for new storage accounts:</span></span>

   * <span data-ttu-id="070a0-119">**Sammanställd**: samlar in mätvärden, till exempel ingång-/ utgång, tillgänglighet, svarstid och lyckade procenttal.</span><span class="sxs-lookup"><span data-stu-id="070a0-119">**Aggregate**: Collects metrics such as ingress/egress, availability, latency, and success percentages.</span></span> <span data-ttu-id="070a0-120">De här måtten samman för hello blob, kön, tabell och Filtjänster.</span><span class="sxs-lookup"><span data-stu-id="070a0-120">These metrics are aggregated for hello blob, queue, table, and file services.</span></span>
   * <span data-ttu-id="070a0-121">**Per API**: I tillägg toohello sammanställd statistik och, samlar in hello samma uppsättning mätvärden för varje Lagringsåtgärden i hello Azure Storage service API.</span><span class="sxs-lookup"><span data-stu-id="070a0-121">**Per API**: In addition toohello aggregate metrics, collects hello same set of metrics for each storage operation in hello Azure Storage service API.</span></span>

   <span data-ttu-id="070a0-122">tooset hello databevarandeprincip, flytta hello **bevarande (dagar)** skjutreglaget eller ange hello antal dagar som data tooretain från 1 too365.</span><span class="sxs-lookup"><span data-stu-id="070a0-122">tooset hello data retention policy, move hello **Retention (days)** slider or enter hello number of days of data tooretain, from 1 too365.</span></span> <span data-ttu-id="070a0-123">hello standard för nya storage-konton är sju dagar.</span><span class="sxs-lookup"><span data-stu-id="070a0-123">hello default for new storage accounts is seven days.</span></span> <span data-ttu-id="070a0-124">Om du inte vill att tooset en bevarandeprincip ange noll.</span><span class="sxs-lookup"><span data-stu-id="070a0-124">If you do not want tooset a retention policy, enter zero.</span></span> <span data-ttu-id="070a0-125">Om det finns inga bevarandeprincip, är det upp tooyou toodelete hello övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="070a0-125">If there is no retention policy, it is up tooyou toodelete hello monitoring data.</span></span>

   > [!WARNING]
   > <span data-ttu-id="070a0-126">Du debiteras när du manuellt ta bort mått data.</span><span class="sxs-lookup"><span data-stu-id="070a0-126">You are charged when you manually delete metrics data.</span></span> <span data-ttu-id="070a0-127">Inaktuella analysdata (data som är äldre än din bevarandeprincip) tas bort av hello system utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="070a0-127">Stale analytics data (data older than your retention policy) is deleted by hello system at no cost.</span></span> <span data-ttu-id="070a0-128">Vi rekommenderar en bevarandeprincip baserat på hur länge du vill tooretain storage analytics-data för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="070a0-128">We recommend setting a retention policy based on how long you want tooretain storage analytics data for your account.</span></span> <span data-ttu-id="070a0-129">Se [vilka avgifter gör uppkommer när du aktiverar storage-mätvärden?](../common/storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) för mer information.</span><span class="sxs-lookup"><span data-stu-id="070a0-129">See [What charges do you incur when you enable storage metrics?](../common/storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) for more information.</span></span>
   >

1. <span data-ttu-id="070a0-130">När du är klar hello övervakningskonfigurationen Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="070a0-130">When you finish hello monitoring configuration, select **Save**.</span></span>

<span data-ttu-id="070a0-131">En standarduppsättning mått visas i diagram på hello lagring konto-bladet, samt hello enskild tjänst blad (blob, kön, tabell och filen).</span><span class="sxs-lookup"><span data-stu-id="070a0-131">A default set of metrics is displayed in charts on hello storage account blade, as well as hello individual service blades (blob, queue, table, and file).</span></span> <span data-ttu-id="070a0-132">När du har aktiverat mått för en tjänst, kan det ta upp tooan timme för data tooappear i dess diagram.</span><span class="sxs-lookup"><span data-stu-id="070a0-132">Once you've enabled metrics for a service, it may take up tooan hour for data tooappear in its charts.</span></span> <span data-ttu-id="070a0-133">Du kan välja **redigera** alla mått diagram för[konfigurera vilka mått](#how-to-customize-metrics-charts) visas i hello diagram.</span><span class="sxs-lookup"><span data-stu-id="070a0-133">You can select **Edit** on any metric chart too[configure which metrics](#how-to-customize-metrics-charts) are displayed in hello chart.</span></span>

<span data-ttu-id="070a0-134">Du kan inaktivera insamling av mätvärden och loggning genom att ange **Status** för**av**.</span><span class="sxs-lookup"><span data-stu-id="070a0-134">You can disable metrics collection and logging by setting **Status** too**Off**.</span></span>

> [!NOTE]
> <span data-ttu-id="070a0-135">Azure Storage använder [tabell lagring](../common/storage-introduction.md#table-storage) toostore hello mätvärden för ditt lagringskonto och lagrar hello mått i tabeller i ditt konto.</span><span class="sxs-lookup"><span data-stu-id="070a0-135">Azure Storage uses [table storage](../common/storage-introduction.md#table-storage) toostore hello metrics for your storage account, and stores hello metrics in tables in your account.</span></span> <span data-ttu-id="070a0-136">Mer information finns i.</span><span class="sxs-lookup"><span data-stu-id="070a0-136">For more information, see.</span></span> <span data-ttu-id="070a0-137">[Hur mått lagras](../common/storage-analytics.md#how-metrics-are-stored).</span><span class="sxs-lookup"><span data-stu-id="070a0-137">[How metrics are stored](../common/storage-analytics.md#how-metrics-are-stored).</span></span>
>

## <a name="customize-metrics-charts"></a><span data-ttu-id="070a0-138">Anpassa mått diagram</span><span class="sxs-lookup"><span data-stu-id="070a0-138">Customize metrics charts</span></span>

<span data-ttu-id="070a0-139">Använda följande procedur toochoose hello vilka lagring mått tooview i ett mått diagram.</span><span class="sxs-lookup"><span data-stu-id="070a0-139">Use hello following procedure toochoose which storage metrics tooview in a metrics chart.</span></span> 

1. <span data-ttu-id="070a0-140">Starta genom att visa ett mått diagram för lagring i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="070a0-140">Start by displaying a storage metric chart in hello Azure portal.</span></span> <span data-ttu-id="070a0-141">Du kan hitta diagram på hello **lagring kontoblad** och i hello **mått** bladet för en enskild tjänst (blob, kön, tabell, fil).</span><span class="sxs-lookup"><span data-stu-id="070a0-141">You can find charts on hello **storage account blade** and in hello **Metrics** blade for an individual service (blob, queue, table, file).</span></span>

   <span data-ttu-id="070a0-142">I det här exemplet vi arbetar med hello följande diagram som visas på hello **lagring kontoblad**:</span><span class="sxs-lookup"><span data-stu-id="070a0-142">In this example, we work with hello following chart that appears on hello **storage account blade**:</span></span>

   ![Val av diagram i Azure-portalen](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. <span data-ttu-id="070a0-144">Klicka någonstans i hello diagram tooopen hello **mått** bladet.</span><span class="sxs-lookup"><span data-stu-id="070a0-144">Next, click anywhere within hello chart tooopen hello **Metric** blade.</span></span> <span data-ttu-id="070a0-145">Välj **redigera diagram** tooopen hello **redigera diagram** bladet.</span><span class="sxs-lookup"><span data-stu-id="070a0-145">Select **Edit chart** tooopen hello **Edit Chart** blade.</span></span>

   ![Redigera knappen på bladet för diagram](./media/storage-monitor-storage-account/stg-customize-chart-01.png)

1. <span data-ttu-id="070a0-147">På hello **redigera diagram** bladet, Välj hello **tidsintervall** av hello mått toodisplay i hello diagram och hello **service** (blob, kö, tabell, filen) vars mått som du vill toodisplay.</span><span class="sxs-lookup"><span data-stu-id="070a0-147">On hello **Edit Chart** blade, select hello **Time Range** of hello metrics toodisplay in hello chart, and hello **service** (blob, queue, table, file) whose metrics you wish toodisplay.</span></span> <span data-ttu-id="070a0-148">Här har vi valt toodisplay hello tidigare veckans mätvärden för hello blob-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="070a0-148">Here we've selected toodisplay hello past week's metrics for hello blob service:</span></span>

   ![Intervallet och tjänsten valet av tid hello redigera diagram-bladet](./media/storage-monitor-storage-account/stg-customize-chart-02.png)

1. <span data-ttu-id="070a0-150">Välj hello individuella **mått** du hade som visas i diagrammet hello och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="070a0-150">Select hello individual **metrics** you'd like displayed in hello chart, then click **OK**.</span></span> <span data-ttu-id="070a0-151">Till exempel här vi valt toodisplay hello *ContainerCount* och *ObjectCount* mått:</span><span class="sxs-lookup"><span data-stu-id="070a0-151">For example, here we've chosen toodisplay hello *ContainerCount* and *ObjectCount* metrics:</span></span>

   ![Enskilda mått val i bladet redigera diagram](./media/storage-monitor-storage-account/stg-customize-chart-03.png)

<span data-ttu-id="070a0-153">I diagrammet inte påverkar hello samling eller aggregering lagring av övervakningsdata i hello storage-konto, endast hello visning av mätvärdesdata.</span><span class="sxs-lookup"><span data-stu-id="070a0-153">Your chart settings do not affect hello collection, aggregation, or storage of monitoring data in hello storage account, only hello viewing of metrics data.</span></span>

### <a name="metrics-availability-in-charts"></a><span data-ttu-id="070a0-154">Mått tillgänglighet i diagram</span><span class="sxs-lookup"><span data-stu-id="070a0-154">Metrics availability in charts</span></span>

<span data-ttu-id="070a0-155">hello lista över tillgängliga mått ändringar baserat på vilken tjänst som du har valt i hello listrutan och hello typ av diagram hello du redigerar.</span><span class="sxs-lookup"><span data-stu-id="070a0-155">hello list of available metrics changes based on which service you've chosen in hello drop-down, and hello unit type of hello chart you're editing.</span></span> <span data-ttu-id="070a0-156">Du kan till exempel välja procentandel mått som *PercentNetworkError* och *PercentThrottlingError* bara om du redigerar ett diagram som visar enheter i procent:</span><span class="sxs-lookup"><span data-stu-id="070a0-156">For example, you can select percentage metrics like *PercentNetworkError* and *PercentThrottlingError* only if you're editing a chart that displays units in percentage:</span></span>

![Begäran fel procentandel diagram i hello Azure-portalen](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a><span data-ttu-id="070a0-158">Mått upplösning</span><span class="sxs-lookup"><span data-stu-id="070a0-158">Metrics resolution</span></span>

<span data-ttu-id="070a0-159">hello mått som du har valt i diagnostik avgör hello lösning av hello mätvärden som är tillgängliga för ditt konto:</span><span class="sxs-lookup"><span data-stu-id="070a0-159">hello metrics you selected in Diagnostics determines hello resolution of hello metrics that are available for your account:</span></span>

* <span data-ttu-id="070a0-160">**Sammanställd** övervakning innehåller mått, till exempel ingång-/ utgång, tillgänglighet, svarstid och lyckade procenttal.</span><span class="sxs-lookup"><span data-stu-id="070a0-160">**Aggregate** monitoring provides metrics such as ingress/egress, availability, latency, and success percentages.</span></span> <span data-ttu-id="070a0-161">De här måtten samman från hello blob-, tabell-, fil- och queue-tjänster.</span><span class="sxs-lookup"><span data-stu-id="070a0-161">These metrics are aggregated from hello blob, table, file, and queue services.</span></span>
* <span data-ttu-id="070a0-162">**Per API** ger ökad upplösning mätvärden som är tillgängliga för enskilda lagringsåtgärder dessutom toohello servicenivåer mängder.</span><span class="sxs-lookup"><span data-stu-id="070a0-162">**Per API** provides finer resolution, with metrics available for individual storage operations, in addition toohello service-level aggregates.</span></span>

## <a name="configure-metrics-alerts"></a><span data-ttu-id="070a0-163">Konfigurera aviseringar för mått</span><span class="sxs-lookup"><span data-stu-id="070a0-163">Configure metrics alerts</span></span>

<span data-ttu-id="070a0-164">Du kan skapa aviseringar toonotify när tröskelvärdet har uppnåtts för storage resource mått.</span><span class="sxs-lookup"><span data-stu-id="070a0-164">You can create alerts toonotify you when thresholds have been reached for storage resource metrics.</span></span>

1. <span data-ttu-id="070a0-165">tooopen hello **Varningsregler bladet**, bläddra nedåt toohello **övervakning** avsnitt i hello **menyn bladet** och välj **Varna regler**.</span><span class="sxs-lookup"><span data-stu-id="070a0-165">tooopen hello **Alert rules blade**, scroll down toohello **MONITORING** section of hello **Menu blade** and select **Alert rules**.</span></span>
1. <span data-ttu-id="070a0-166">Välj **Lägg till avisering** tooopen hello **lägga till en varningsregel** bladet</span><span class="sxs-lookup"><span data-stu-id="070a0-166">Select **Add alert** tooopen hello **Add an alert rule** blade</span></span>
1. <span data-ttu-id="070a0-167">Välj en **resurs** (blob, fil, kö, tabell) från hello listrutan och ange en **namn** och **beskrivning** för din nya varningsregel.</span><span class="sxs-lookup"><span data-stu-id="070a0-167">Select a **Resource** (blob, file, queue, table) from hello drop-down, and enter a **Name** and **Description** for your new alert rule.</span></span>
1. <span data-ttu-id="070a0-168">Välj hello **mått** som du vill att tooadd en varning och en avisering **villkoret**, och en **tröskelvärdet**.</span><span class="sxs-lookup"><span data-stu-id="070a0-168">Select hello **Metric** for which you'd like tooadd an alert, an alert **Condition**, and a **Threshold**.</span></span> <span data-ttu-id="070a0-169">hello tröskelvärdet enhet ändras beroende på hello mått som du har valt.</span><span class="sxs-lookup"><span data-stu-id="070a0-169">hello threshold unit type changes depending on hello metric you've chosen.</span></span> <span data-ttu-id="070a0-170">Till exempel är ”antal” hello enhetstyp för *ContainerCount*, medan hello enhet för hello *PercentNetworkError* mått är en del.</span><span class="sxs-lookup"><span data-stu-id="070a0-170">For example, "count" is hello unit type for *ContainerCount*, while hello unit for hello *PercentNetworkError* metric is a percentage.</span></span>
1. <span data-ttu-id="070a0-171">Välj hello **Period**.</span><span class="sxs-lookup"><span data-stu-id="070a0-171">Select hello **Period**.</span></span> <span data-ttu-id="070a0-172">Mått som når eller överskrider hello tröskeln inom hello period utlösa en avisering.</span><span class="sxs-lookup"><span data-stu-id="070a0-172">Metrics that reach or exceed hello Threshold within hello period trigger an alert.</span></span>
1. <span data-ttu-id="070a0-173">(Valfritt) Konfigurera **e-post** och **Webhook** meddelanden.</span><span class="sxs-lookup"><span data-stu-id="070a0-173">(Optional) Configure **Email** and **Webhook** notifications.</span></span> <span data-ttu-id="070a0-174">Mer information om webhooks finns [konfigurera en webhook på en Azure mått avisering](../../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="070a0-174">For more information on webhooks, see [Configure a webhook on an Azure metric alert](../../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span> <span data-ttu-id="070a0-175">Om du inte konfigurerar e-post eller webhook meddelanden visas aviseringar endast i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="070a0-175">If you do not configure email or webhook notifications, alerts will appear only in hello Azure portal.</span></span>

![”Lägg till en varningsregel-bladet i hello Azure-portalen](./media/storage-monitor-storage-account/stg-alert-rules-01.png)

## <a name="add-metrics-charts-toohello-portal-dashboard"></a><span data-ttu-id="070a0-177">Lägg till mått diagram toohello portalens instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="070a0-177">Add metrics charts toohello portal dashboard</span></span>

<span data-ttu-id="070a0-178">Du kan lägga till Azure Storage metrics diagram för någon av dina konton tooyour portal lagringsinstrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="070a0-178">You can add Azure Storage metrics charts for any of your storage accounts tooyour portal dashboard.</span></span>

1. <span data-ttu-id="070a0-179">Välj Klicka **redigera instrumentpanel** när du visar instrumentpanelen i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="070a0-179">Select click **Edit dashboard** while viewing your dashboard in hello [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="070a0-180">I hello **panelen galleriet**väljer **hitta panelerna genom** > **typen**.</span><span class="sxs-lookup"><span data-stu-id="070a0-180">In hello **Tile Gallery**, select **Find tiles by** > **Type**.</span></span>
1. <span data-ttu-id="070a0-181">Välj **typen** > **lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="070a0-181">Select **Type** > **Storage accounts**.</span></span>
1. <span data-ttu-id="070a0-182">I **resurser**, Välj hello lagringskonto vars mått som du vill tooadd toohello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="070a0-182">In **Resources**, select hello storage account whose metrics you wish tooadd toohello dashboard.</span></span>
1. <span data-ttu-id="070a0-183">Välj **kategorier** > **övervakning**.</span><span class="sxs-lookup"><span data-stu-id="070a0-183">Select **Categories** > **Monitoring**.</span></span>
1. <span data-ttu-id="070a0-184">Dra och släpp hello diagram panelen på instrumentpanelen för hello mått som visas.</span><span class="sxs-lookup"><span data-stu-id="070a0-184">Drag-and-drop hello chart tile onto your dashboard for hello metric you'd like displayed.</span></span> <span data-ttu-id="070a0-185">Upprepa för alla mått som visas på hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="070a0-185">Repeat for all metrics you'd like displayed on hello dashboard.</span></span> <span data-ttu-id="070a0-186">I följande bild hello, hello ”BLOB - Totalt antal begäranden för” diagrammet markeras som exempel, men alla hello diagram är tillgängliga för placering på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="070a0-186">In hello following image, hello "Blobs - Total requests" chart is highlighted as an example, but all hello charts are available for placement on your dashboard.</span></span>

   ![Panelen galleri i Azure-portalen](./media/storage-monitor-storage-account/stg-customize-dashboard-01.png)
1. <span data-ttu-id="070a0-188">Välj **klar anpassa** hello övre delen av hello instrumentpanelen när du är klar att lägga till diagram.</span><span class="sxs-lookup"><span data-stu-id="070a0-188">Select **Done customizing** near hello top of hello dashboard when you're done adding charts.</span></span>

<span data-ttu-id="070a0-189">När du har lagt till diagram tooyour instrumentpanelen kan du anpassa dem enligt beskrivningen i [anpassa mått diagram](#how-to-customize-metrics-charts).</span><span class="sxs-lookup"><span data-stu-id="070a0-189">Once you've added charts tooyour dashboard, you can further customize them as described in [Customize metrics charts](#how-to-customize-metrics-charts).</span></span>

## <a name="configure-logging"></a><span data-ttu-id="070a0-190">Konfigurera loggning</span><span class="sxs-lookup"><span data-stu-id="070a0-190">Configure logging</span></span>

<span data-ttu-id="070a0-191">Du kan instruera Azure Storage toosave diagnostik loggar för läsa, skriva och ta bort begäranden för hello blob-, tabell- och kötjänster.</span><span class="sxs-lookup"><span data-stu-id="070a0-191">You can instruct Azure Storage toosave diagnostics logs for read, write, and delete requests for hello blob, table, and queue services.</span></span> <span data-ttu-id="070a0-192">Hej databevarandeprincip som du anger gäller även toothese loggar.</span><span class="sxs-lookup"><span data-stu-id="070a0-192">hello data retention policy you set also applies toothese logs.</span></span>

> [!NOTE]
> <span data-ttu-id="070a0-193">Azure File storage kan du för närvarande stöder Storage Analytics mätvärden, men ännu stöder inte loggning.</span><span class="sxs-lookup"><span data-stu-id="070a0-193">Azure File storage currently supports Storage Analytics metrics, but does not yet support logging.</span></span>
>

1. <span data-ttu-id="070a0-194">I hello [Azure-portalen](https://portal.azure.com)väljer **lagringskonton**, hello namnet på hello lagring konto tooopen hello storage-konto-bladet.</span><span class="sxs-lookup"><span data-stu-id="070a0-194">In hello [Azure portal](https://portal.azure.com), select **Storage accounts**, then hello name of hello storage account tooopen hello storage account blade.</span></span>
1. <span data-ttu-id="070a0-195">Välj **diagnostik** i hello **övervakning** avsnitt i hello menyn bladet.</span><span class="sxs-lookup"><span data-stu-id="070a0-195">Select **Diagnostics** in hello **MONITORING** section of hello menu blade.</span></span>

    ![Diagnostik menyalternativet under övervakning i hello Azure-portalen.](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)
    
1. <span data-ttu-id="070a0-197">Se till att **Status** har angetts för**på**, och välj hello **services** som du vill att tooenable loggning.</span><span class="sxs-lookup"><span data-stu-id="070a0-197">Ensure **Status** is set too**On**, and select hello **services** for which you'd like tooenable logging.</span></span>

    ![Konfigurera loggning i hello Azure-portalen.](./media/storage-monitor-storage-account/stg-enable-logging-01.png)
1. <span data-ttu-id="070a0-199">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="070a0-199">Click **Save**.</span></span>

<span data-ttu-id="070a0-200">hello diagnostik loggar sparas i en blobbbehållare med namnet $logs i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="070a0-200">hello diagnostics logs are saved in a blob container named $logs in your storage account.</span></span> <span data-ttu-id="070a0-201">Du kan visa hello loggdata med en lagringsutforskare som hello [Microsoft Lagringsutforskaren](http://storageexplorer.com), eller via programmering med storage-klientbibliotek för hello eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="070a0-201">You can view hello log data using a storage explorer like hello [Microsoft Storage Explorer](http://storageexplorer.com), or programmatically using hello storage client library or PowerShell.</span></span>

<span data-ttu-id="070a0-202">Information om att komma åt hello $logs behållaren finns [aktivera loggning för lagring och åtkomst till loggdata](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).</span><span class="sxs-lookup"><span data-stu-id="070a0-202">For information about accessing hello $logs container, see [Enabling Storage Logging and Accessing Log Data](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).</span></span>

## <a name="next-steps"></a><span data-ttu-id="070a0-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="070a0-203">Next steps</span></span>

* <span data-ttu-id="070a0-204">Hitta mer information om [statistik, loggning och fakturering](../storage-analytics.md) för Storage Analytics.</span><span class="sxs-lookup"><span data-stu-id="070a0-204">Find more details about [metrics, logging, and billing](../storage-analytics.md) for Storage Analytics.</span></span>
* <span data-ttu-id="070a0-205">[Aktivera Azure Storage-mätvärden och visa mätvärdesdata](../storage-enable-and-view-metrics.md) med hjälp av PowerShell och genom programmering med C#.</span><span class="sxs-lookup"><span data-stu-id="070a0-205">[Enable Azure Storage metrics and view metrics data](../storage-enable-and-view-metrics.md) by using PowerShell and programmatically with C#.</span></span>
