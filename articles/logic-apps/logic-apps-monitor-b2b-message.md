---
title: aaaMonitor B2B-transaktioner och konfigurera loggning - Azure Logic Apps | Microsoft Docs
description: "Övervaka AS2-, X 12- och EDIFACT-meddelanden, start diagnostikloggning för ditt konto för integrering"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: a6745ebf41aab331020bfec072f5806711d125bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-set-up-diagnostics-logging-for-b2b-communication-in-integration-accounts"></a><span data-ttu-id="0b3af-103">Övervaka och konfigurera diagnostikloggning för B2B-kommunikation i integrationskonton</span><span class="sxs-lookup"><span data-stu-id="0b3af-103">Monitor and set up diagnostics logging for B2B communication in integration accounts</span></span>

<span data-ttu-id="0b3af-104">När du har skapat B2B-kommunikation mellan två kör affärsprocesser eller program via kontot integration dessa enheter kan utbyta meddelanden med varandra.</span><span class="sxs-lookup"><span data-stu-id="0b3af-104">After you set up B2B communication between two running business processes or applications through your integration account, those entities can exchange messages with each other.</span></span> <span data-ttu-id="0b3af-105">tooconfirm kommunikationen fungerar som förväntat och du kan konfigurera övervakning för AS2-, X12, EDIFACT-meddelanden, tillsammans med diagnostikloggning för integrering kontot via hello [Azure logganalys](../log-analytics/log-analytics-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="0b3af-105">tooconfirm this communication works as expected, you can set up monitoring for AS2, X12, and EDIFACT messages, along with diagnostics logging for your integration account through hello [Azure Log Analytics](../log-analytics/log-analytics-overview.md) service.</span></span> <span data-ttu-id="0b3af-106">Den här tjänsten i [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) övervakar molnet och lokala miljöer, vilket hjälper dig att upprätthålla sin tillgänglighet och prestanda, och samlar även in information om körning och händelser för bättre felsökning.</span><span class="sxs-lookup"><span data-stu-id="0b3af-106">This service in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) monitors your cloud and on-premises environments, helping you maintain their availability and performance, and also collects runtime details and events for richer debugging.</span></span> <span data-ttu-id="0b3af-107">Du kan också [använda diagnostiska data med andra tjänster](#extend-diagnostic-data), t.ex. Azure Storage och Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="0b3af-107">You can also [use your diagnostic data with other services](#extend-diagnostic-data), like Azure Storage and Azure Event Hubs.</span></span>

## <a name="requirements"></a><span data-ttu-id="0b3af-108">Krav</span><span class="sxs-lookup"><span data-stu-id="0b3af-108">Requirements</span></span>

* <span data-ttu-id="0b3af-109">En logikapp som har konfigurerats med diagnostikloggning.</span><span class="sxs-lookup"><span data-stu-id="0b3af-109">A logic app that's set up with diagnostics logging.</span></span> <span data-ttu-id="0b3af-110">Läs [hur tooset in loggning för att logikapp](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span><span class="sxs-lookup"><span data-stu-id="0b3af-110">Learn [how tooset up logging for that logic app](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span></span>

  > [!NOTE]
  > <span data-ttu-id="0b3af-111">När du har uppfyllt detta krav, bör du ha en arbetsyta i hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0b3af-111">After you've met this requirement, you should have a workspace in hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span></span> <span data-ttu-id="0b3af-112">Du bör använda hello samma OMS-arbetsyta när du ställer in loggning för ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="0b3af-112">You should use hello same OMS workspace when you set up logging for your integration account.</span></span> <span data-ttu-id="0b3af-113">Lär dig mer om du inte har en OMS-arbetsyta [hur toocreate en OMS-arbetsyta](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0b3af-113">If you don't have an OMS workspace, learn [how toocreate an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span>

* <span data-ttu-id="0b3af-114">Ett integration-konto som har länkade tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="0b3af-114">An integration account that's linked tooyour logic app.</span></span> <span data-ttu-id="0b3af-115">Läs [hur toocreate integrering med ett konto med en länk tooyour logikapp](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).</span><span class="sxs-lookup"><span data-stu-id="0b3af-115">Learn [how toocreate an integration account with a link tooyour logic app](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).</span></span>

## <a name="turn-on-diagnostics-logging-for-your-integration-account"></a><span data-ttu-id="0b3af-116">Aktivera diagnostik loggning för ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="0b3af-116">Turn on diagnostics logging for your integration account</span></span>

<span data-ttu-id="0b3af-117">Du kan aktivera loggning antingen direkt från ditt konto för integrering eller [via hello Azure-Monitor-tjänsten](#azure-monitor-service).</span><span class="sxs-lookup"><span data-stu-id="0b3af-117">You can turn on logging either directly from your integration account or [through hello Azure Monitor service](#azure-monitor-service).</span></span> <span data-ttu-id="0b3af-118">Övervakare för Azure tillhandahåller grundläggande övervakning med infrastrukturnivå data.</span><span class="sxs-lookup"><span data-stu-id="0b3af-118">Azure Monitor provides basic monitoring with infrastructure-level data.</span></span> <span data-ttu-id="0b3af-119">Lär dig mer om [Azure-Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="0b3af-119">Learn more about [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md).</span></span>

### <a name="turn-on-diagnostics-logging-directly-from-your-integration-account"></a><span data-ttu-id="0b3af-120">Aktivera diagnostik loggning direkt från ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="0b3af-120">Turn on diagnostics logging directly from your integration account</span></span>

1. <span data-ttu-id="0b3af-121">I hello [Azure-portalen](https://portal.azure.com), söka efter och välj ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="0b3af-121">In hello [Azure portal](https://portal.azure.com), find and select your integration account.</span></span> <span data-ttu-id="0b3af-122">Under **övervakning**, Välj **diagnostik loggar** som visas här:</span><span class="sxs-lookup"><span data-stu-id="0b3af-122">Under **Monitoring**, choose **Diagnostics logs** as shown here:</span></span>

   ![Hitta och markera kontot integration, Välj ”diagnostikloggar”](media/logic-apps-monitor-b2b-message/integration-account-diagnostics.png)

2. <span data-ttu-id="0b3af-124">När du har valt kontot integration väljs hello följande värden automatiskt.</span><span class="sxs-lookup"><span data-stu-id="0b3af-124">After you select your integration account, hello following values are automatically selected.</span></span> <span data-ttu-id="0b3af-125">Om dessa värden är korrekta, välja **aktivera diagnostiken**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-125">If these values are correct, choose **Turn on diagnostics**.</span></span> <span data-ttu-id="0b3af-126">Annars Välj hello-värden som du vill:</span><span class="sxs-lookup"><span data-stu-id="0b3af-126">Otherwise, select hello values that you want:</span></span>

   1. <span data-ttu-id="0b3af-127">Under **prenumeration**, Välj hello Azure-prenumeration som du använder med ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="0b3af-127">Under **Subscription**, select hello Azure subscription that you use with your integration account.</span></span>
   2. <span data-ttu-id="0b3af-128">Under **resursgruppen**väljer hello resursgrupp som du använder med ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="0b3af-128">Under **Resource group**, select hello resource group that you use with your integration account.</span></span>
   3. <span data-ttu-id="0b3af-129">Under **resurstypen**väljer **integrationskonton**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-129">Under **Resource type**, select **Integration accounts**.</span></span> 
   4. <span data-ttu-id="0b3af-130">Under **resurs**, Välj ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="0b3af-130">Under **Resource**, select your integration account.</span></span> 
   5. <span data-ttu-id="0b3af-131">Välj **aktivera diagnostiken**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-131">Choose **Turn on diagnostics**.</span></span>

   ![Konfigurera diagnostik för ditt konto för integrering](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. <span data-ttu-id="0b3af-133">Under **diagnostikinställningarna**, och sedan **Status**, Välj **på**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-133">Under **Diagnostics settings**, and then **Status**, choose **On**.</span></span>

   ![Aktivera Azure-diagnostik](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. <span data-ttu-id="0b3af-135">Välj nu hello OMS arbetsytan och data toouse för loggning som visas:</span><span class="sxs-lookup"><span data-stu-id="0b3af-135">Now select hello OMS workspace and data toouse for logging as shown:</span></span>

   1. <span data-ttu-id="0b3af-136">Välj **skicka tooLog Analytics**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-136">Select **Send tooLog Analytics**.</span></span> 
   2. <span data-ttu-id="0b3af-137">Under **logganalys**, Välj **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-137">Under **Log Analytics**, choose **Configure**.</span></span> 
   3. <span data-ttu-id="0b3af-138">Under **OMS arbetsytor**, Välj hello OMS arbetsytan toouse för loggning.</span><span class="sxs-lookup"><span data-stu-id="0b3af-138">Under **OMS Workspaces**, select hello OMS workspace toouse for logging.</span></span>
   4. <span data-ttu-id="0b3af-139">Under **loggen**väljer hello **IntegrationAccountTrackingEvents** kategori.</span><span class="sxs-lookup"><span data-stu-id="0b3af-139">Under **Log**, select hello **IntegrationAccountTrackingEvents** category.</span></span>
   5. <span data-ttu-id="0b3af-140">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-140">Choose **Save**.</span></span>

   ![Konfigurera logganalys så kan du skicka diagnostik data tooa logg](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. <span data-ttu-id="0b3af-142">Nu [konfigurera spårning för dina B2B-meddelanden i OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="0b3af-142">Now [set up tracking for your B2B messages in OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>

<a name="azure-monitor-service"></a>

### <a name="turn-on-diagnostics-logging-through-azure-monitor"></a><span data-ttu-id="0b3af-143">Aktivera diagnostikloggning via Azure-Monitor</span><span class="sxs-lookup"><span data-stu-id="0b3af-143">Turn on diagnostics logging through Azure Monitor</span></span>

1. <span data-ttu-id="0b3af-144">I hello [Azure-portalen](https://portal.azure.com), på hello Azure huvudmenyn, Välj **övervakaren**, **diagnostik loggar**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-144">In hello [Azure portal](https://portal.azure.com), on hello main Azure menu, choose **Monitor**, **Diagnostics logs**.</span></span> <span data-ttu-id="0b3af-145">Välj ditt konto, integration som visas här:</span><span class="sxs-lookup"><span data-stu-id="0b3af-145">Then select your integration account as shown here:</span></span>

   ![Välj ”övervaka”, ”diagnostikloggar”, Välj integration-konto](media/logic-apps-monitor-b2b-message/monitor-service-diagnostics-logs.png)

2. <span data-ttu-id="0b3af-147">När du har valt kontot integration väljs hello följande värden automatiskt.</span><span class="sxs-lookup"><span data-stu-id="0b3af-147">After you select your integration account, hello following values are automatically selected.</span></span> <span data-ttu-id="0b3af-148">Om dessa värden är korrekta, välja **aktivera diagnostiken**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-148">If these values are correct, choose **Turn on diagnostics**.</span></span> <span data-ttu-id="0b3af-149">Annars Välj hello-värden som du vill:</span><span class="sxs-lookup"><span data-stu-id="0b3af-149">Otherwise, select hello values that you want:</span></span>

   1. <span data-ttu-id="0b3af-150">Under **prenumeration**, Välj hello Azure-prenumeration som du använder med ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="0b3af-150">Under **Subscription**, select hello Azure subscription that you use with your integration account.</span></span>
   2. <span data-ttu-id="0b3af-151">Under **resursgruppen**väljer hello resursgrupp som du använder med ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="0b3af-151">Under **Resource group**, select hello resource group that you use with your integration account.</span></span>
   3. <span data-ttu-id="0b3af-152">Under **resurstypen**väljer **integrationskonton**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-152">Under **Resource type**, select **Integration accounts**.</span></span>
   4. <span data-ttu-id="0b3af-153">Under **resurs**, Välj ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="0b3af-153">Under **Resource**, select your integration account.</span></span>
   5. <span data-ttu-id="0b3af-154">Välj **aktivera diagnostiken**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-154">Choose **Turn on diagnostics**.</span></span>

   ![Konfigurera diagnostik för ditt konto för integrering](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. <span data-ttu-id="0b3af-156">Under **diagnostikinställningarna**, Välj **på**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-156">Under **Diagnostics settings**, choose **On**.</span></span>

   ![Aktivera Azure-diagnostik](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. <span data-ttu-id="0b3af-158">Nu välja hello OMS arbetsytan och händelse kategori för loggning som visas:</span><span class="sxs-lookup"><span data-stu-id="0b3af-158">Now select hello OMS workspace and event category for logging as shown:</span></span>

   1. <span data-ttu-id="0b3af-159">Välj **skicka tooLog Analytics**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-159">Select **Send tooLog Analytics**.</span></span> 
   2. <span data-ttu-id="0b3af-160">Under **logganalys**, Välj **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-160">Under **Log Analytics**, choose **Configure**.</span></span> 
   3. <span data-ttu-id="0b3af-161">Under **OMS arbetsytor**, Välj hello OMS arbetsytan toouse för loggning.</span><span class="sxs-lookup"><span data-stu-id="0b3af-161">Under **OMS Workspaces**, select hello OMS workspace toouse for logging.</span></span>
   4. <span data-ttu-id="0b3af-162">Under **loggen**väljer hello **IntegrationAccountTrackingEvents** kategori.</span><span class="sxs-lookup"><span data-stu-id="0b3af-162">Under **Log**, select hello **IntegrationAccountTrackingEvents** category.</span></span>
   5. <span data-ttu-id="0b3af-163">När du är klar väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="0b3af-163">When you're done, choose **Save**.</span></span>

   ![Konfigurera logganalys så kan du skicka diagnostik data tooa logg](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. <span data-ttu-id="0b3af-165">Nu [konfigurera spårning för dina B2B-meddelanden i OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="0b3af-165">Now [set up tracking for your B2B messages in OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a><span data-ttu-id="0b3af-166">Utöka hur och när du använder diagnostikdata med andra tjänster</span><span class="sxs-lookup"><span data-stu-id="0b3af-166">Extend how and where you use diagnostic data with other services</span></span>

<span data-ttu-id="0b3af-167">Du kan utöka hur du använder din logikapp diagnostikdata med andra Azure-tjänster, till exempel tillsammans med Azure Log Analytics:</span><span class="sxs-lookup"><span data-stu-id="0b3af-167">Along with Azure Log Analytics, you can extend how you use your logic app's diagnostic data with other Azure services, for example:</span></span> 

* [<span data-ttu-id="0b3af-168">Arkivera Azure Diagnostics loggar i Azure-lagring</span><span class="sxs-lookup"><span data-stu-id="0b3af-168">Archive Azure Diagnostics Logs in Azure Storage</span></span>](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [<span data-ttu-id="0b3af-169">Dataströmmen Azure Diagnostics loggar tooAzure Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="0b3af-169">Stream Azure Diagnostics Logs tooAzure Event Hubs</span></span>](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

<span data-ttu-id="0b3af-170">Du kan sedan get realtid övervakning med hjälp av telemetri och analyser från andra tjänster som [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) och [Power BI](../log-analytics/log-analytics-powerbi.md).</span><span class="sxs-lookup"><span data-stu-id="0b3af-170">You can then get real-time monitoring by using telemetry and analytics from other services, like [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) and [Power BI](../log-analytics/log-analytics-powerbi.md).</span></span> <span data-ttu-id="0b3af-171">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0b3af-171">For example:</span></span>

* [<span data-ttu-id="0b3af-172">Dataströmmen data från Händelsehubbar tooStream Analytics</span><span class="sxs-lookup"><span data-stu-id="0b3af-172">Stream data from Event Hubs tooStream Analytics</span></span>](../stream-analytics/stream-analytics-define-inputs.md)
* [<span data-ttu-id="0b3af-173">Analysera strömmande data med Stream Analytics och skapa en instrumentpanel för analys i realtid i Power BI</span><span class="sxs-lookup"><span data-stu-id="0b3af-173">Analyze streaming data with Stream Analytics and create a real-time analytics dashboard in Power BI</span></span>](../stream-analytics/stream-analytics-power-bi-dashboard.md)

<span data-ttu-id="0b3af-174">Baserat på hello-alternativ som du vill konfigurera, ska du se till att du första [skapa ett Azure storage-konto](../storage/common/storage-create-storage-account.md) eller [skapar en Azure händelsehubb](../event-hubs/event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="0b3af-174">Based on hello options that you want set up, make sure that you first [create an Azure storage account](../storage/common/storage-create-storage-account.md) or [create an Azure event hub](../event-hubs/event-hubs-create.md).</span></span> <span data-ttu-id="0b3af-175">Välj sedan hello alternativ för där du vill toosend diagnostikdata:</span><span class="sxs-lookup"><span data-stu-id="0b3af-175">Then select hello options for where you want toosend diagnostic data:</span></span>

![Skicka data tooAzure lagring konto eller event hub](./media/logic-apps-monitor-b2b-message/storage-account-event-hubs.png)

> [!NOTE]
> <span data-ttu-id="0b3af-177">Kvarhållningsperioder gäller bara när du väljer toouse ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0b3af-177">Retention periods apply only when you choose toouse a storage account.</span></span>

## <a name="supported-tracking-schemas"></a><span data-ttu-id="0b3af-178">Spåra scheman som stöds</span><span class="sxs-lookup"><span data-stu-id="0b3af-178">Supported tracking schemas</span></span>

<span data-ttu-id="0b3af-179">Azure stöder detta spårning schematyper, som har åtgärdats scheman förutom hello typen anpassad.</span><span class="sxs-lookup"><span data-stu-id="0b3af-179">Azure supports these tracking schema types, which all have fixed schemas except hello Custom type.</span></span>

* [<span data-ttu-id="0b3af-180">AS2-spårningsschema</span><span class="sxs-lookup"><span data-stu-id="0b3af-180">AS2 tracking schema</span></span>](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [<span data-ttu-id="0b3af-181">X12-spårningsschema</span><span class="sxs-lookup"><span data-stu-id="0b3af-181">X12 tracking schema</span></span>](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [<span data-ttu-id="0b3af-182">Anpassat spårningsschema</span><span class="sxs-lookup"><span data-stu-id="0b3af-182">Custom tracking schema</span></span>](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a><span data-ttu-id="0b3af-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0b3af-183">Next steps</span></span>

* [<span data-ttu-id="0b3af-184">Spåra B2B-meddelanden i OMS</span><span class="sxs-lookup"><span data-stu-id="0b3af-184">Track B2B messages in OMS</span></span>](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "spåra B2B-meddelanden i OMS")
* [<span data-ttu-id="0b3af-185">Mer information om hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="0b3af-185">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")

