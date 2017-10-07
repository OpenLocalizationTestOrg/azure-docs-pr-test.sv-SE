---
title: "aaaCheck status, ställa in loggning och få varningar – Azure Logic Apps | Microsoft Docs"
description: "Övervaka statusen och prestanda för logic apps loggar diagnostikdata och ställa in aviseringar"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 5c1b1e15-3b6c-49dc-98a6-bdbe7cb75339
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 81f186e11a669b710f4c06089597eb5a76f7a44e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-status-set-up-diagnostics-logging-and-turn-on-alerts-for-azure-logic-apps"></a><span data-ttu-id="d74c4-103">Övervaka status, konfigurera diagnostikloggning och aktivera aviseringar för Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="d74c4-103">Monitor status, set up diagnostics logging, and turn on alerts for Azure Logic Apps</span></span>

<span data-ttu-id="d74c4-104">När du [skapa och köra en logikapp](../logic-apps/logic-apps-create-a-logic-app.md), du kan kontrollera dess körs historik, utlösare historik, status och prestanda.</span><span class="sxs-lookup"><span data-stu-id="d74c4-104">After you [create and run a logic app](../logic-apps/logic-apps-create-a-logic-app.md), you can check its runs history, trigger history, status, and performance.</span></span> <span data-ttu-id="d74c4-105">För händelseövervakning och bättre felsökning, ställa in [diagnostikloggning](#azure-diagnostics) för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="d74c4-105">For real-time event monitoring and richer debugging, set up [diagnostics logging](#azure-diagnostics) for your logic app.</span></span> <span data-ttu-id="d74c4-106">På så sätt kan du kan [söka efter och visa händelser](#find-events), t.ex. utlösaren händelser, kör händelser och händelser för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d74c4-106">That way, you can [find and view events](#find-events), like trigger events, run events, and action events.</span></span> <span data-ttu-id="d74c4-107">Du kan också använda detta [diagnostikdata med andra tjänster](#extend-diagnostic-data), t.ex. Azure Storage och Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="d74c4-107">You can also use this [diagnostics data with other services](#extend-diagnostic-data), like Azure Storage and Azure Event Hubs.</span></span> 

<span data-ttu-id="d74c4-108">tooget meddelanden om fel eller andra möjliga problem, Ställ in [aviseringar](#add-azure-alerts).</span><span class="sxs-lookup"><span data-stu-id="d74c4-108">tooget notifications about failures or other possible problems, set up [alerts](#add-azure-alerts).</span></span> <span data-ttu-id="d74c4-109">Du kan till exempel skapa en avisering som identifierar ”när fler än fem körs inte på en timme”.</span><span class="sxs-lookup"><span data-stu-id="d74c4-109">For example, you can create an alert that detects "when more than five runs fail in an hour."</span></span> <span data-ttu-id="d74c4-110">Du kan även konfigurera övervakning, spårning och loggning programmässigt med hjälp av [egenskaper och inställningar för Azure-diagnostik händelse](#diagnostic-event-properties).</span><span class="sxs-lookup"><span data-stu-id="d74c4-110">You can also set up monitoring, tracking, and logging programmatically by using [Azure Diagnostics event settings and properties](#diagnostic-event-properties).</span></span>

## <a name="view-runs-and-trigger-history-for-your-logic-app"></a><span data-ttu-id="d74c4-111">Visa körs och utlösa historik för din logikapp</span><span class="sxs-lookup"><span data-stu-id="d74c4-111">View runs and trigger history for your logic app</span></span>

1. <span data-ttu-id="d74c4-112">toofind logikappen i hello [Azure-portalen](https://portal.azure.com), på hello Azure huvudmenyn, Välj **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-112">toofind your logic app in hello [Azure portal](https://portal.azure.com), on hello main Azure menu, choose **More services**.</span></span> <span data-ttu-id="d74c4-113">Sök efter ”logikappar” i hello sökrutan och väljer **logikappar**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-113">In hello search box, find "logic apps", and choose **Logic apps**.</span></span>

   ![Hitta din logikapp](./media/logic-apps-monitor-your-logic-apps/find-your-logic-app.png)

   <span data-ttu-id="d74c4-115">hello Azure-portalen visar hello logikappar som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d74c4-115">hello Azure portal shows all hello logic apps that are associated with your Azure subscription.</span></span> 

2. <span data-ttu-id="d74c4-116">Välj din logikapp och sedan **översikt**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-116">Select your logic app, then choose **Overview**.</span></span>

   <span data-ttu-id="d74c4-117">hello Azure-portalen visar hello körs historik och utlösare historik för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="d74c4-117">hello Azure portal shows hello runs history and trigger history for your logic app.</span></span> <span data-ttu-id="d74c4-118">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d74c4-118">For example:</span></span>

   ![Logikapp körs historik och utlösare historik](media/logic-apps-monitor-your-logic-apps/overview.png)

   * <span data-ttu-id="d74c4-120">**Kör historik** visar alla hello körs för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="d74c4-120">**Runs history** shows all hello runs for your logic app.</span></span> 
   * <span data-ttu-id="d74c4-121">**Utlösa historik** visar alla hello Utlösaraktivitet för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="d74c4-121">**Trigger History** shows all hello trigger activity for your logic app.</span></span>

   <span data-ttu-id="d74c4-122">Status finns i [felsöka din logikapp](../logic-apps/logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="d74c4-122">For status descriptions, see [Troubleshoot your logic app](../logic-apps/logic-apps-diagnosing-failures.md).</span></span>

   > [!TIP]
   > <span data-ttu-id="d74c4-123">Om du inte hittar hello data som du förväntar dig hello i verktygsfältet väljer du **uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-123">If you don't find hello data that you expect, on hello toolbar, choose **Refresh**.</span></span>

3. <span data-ttu-id="d74c4-124">tooview hello steg från en specifik kördes under **körs historik**, Välj som körs.</span><span class="sxs-lookup"><span data-stu-id="d74c4-124">tooview hello steps from a specific run, under **Runs history**, select that run.</span></span> 

   <span data-ttu-id="d74c4-125">hello visas Övervakare varje steg i som körs.</span><span class="sxs-lookup"><span data-stu-id="d74c4-125">hello monitor view shows each step in that run.</span></span> <span data-ttu-id="d74c4-126">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d74c4-126">For example:</span></span>

   ![Åtgärder för en specifik kör](media/logic-apps-monitor-your-logic-apps/monitor-view-updated.png)

4. <span data-ttu-id="d74c4-128">tooget mer information om hello kör, Välj **kör information**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-128">tooget more details about hello run, choose **Run Details**.</span></span> <span data-ttu-id="d74c4-129">Den här informationen sammanfattar hello steg, status, indata och utdata för hello kör.</span><span class="sxs-lookup"><span data-stu-id="d74c4-129">This information summarizes hello steps, status, inputs, and outputs for hello run.</span></span> 

   ![Välj ”Kör information”](media/logic-apps-monitor-your-logic-apps/run-details.png)

   <span data-ttu-id="d74c4-131">Du kan till exempel få hello körs **Korrelations-ID**, som du kan behöva när du använder hello [REST API för Logic Apps](https://docs.microsoft.com/rest/api/logic).</span><span class="sxs-lookup"><span data-stu-id="d74c4-131">For example, you can get hello run's **Correlation ID**, which you might need when you use hello [REST API for Logic Apps](https://docs.microsoft.com/rest/api/logic).</span></span>

5. <span data-ttu-id="d74c4-132">tooget information om ett specifikt steg, väljer du steget.</span><span class="sxs-lookup"><span data-stu-id="d74c4-132">tooget details about a specific step, choose that step.</span></span> <span data-ttu-id="d74c4-133">Nu kan du granska information som indata, utdata och eventuella fel som inträffade för steget.</span><span class="sxs-lookup"><span data-stu-id="d74c4-133">You can now review details like inputs, outputs, and any errors that happened for that step.</span></span> <span data-ttu-id="d74c4-134">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d74c4-134">For example:</span></span>

   ![Steg-information](media/logic-apps-monitor-your-logic-apps/monitor-view-details.png)
   
   > [!NOTE]
   > <span data-ttu-id="d74c4-136">Alla runtime information och händelser krypteras inom hello Logic Apps-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d74c4-136">All runtime details and events are encrypted within hello Logic Apps service.</span></span> <span data-ttu-id="d74c4-137">De dekrypteras endast när en användare begär tooview data.</span><span class="sxs-lookup"><span data-stu-id="d74c4-137">They are decrypted only when a user requests tooview that data.</span></span> <span data-ttu-id="d74c4-138">Du kan också styra åtkomst toothese händelser med [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="d74c4-138">You can also control access toothese events with [Azure Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-what-is.md).</span></span>

6. <span data-ttu-id="d74c4-139">tooget information om en specifik utlösare, gå tillbaka toohello **översikt** fönstret.</span><span class="sxs-lookup"><span data-stu-id="d74c4-139">tooget details about a specific trigger event, go back toohello **Overview** pane.</span></span> <span data-ttu-id="d74c4-140">Under **utlösa historik**väljer hello-utlösare.</span><span class="sxs-lookup"><span data-stu-id="d74c4-140">Under **Trigger history**, select hello trigger event.</span></span> <span data-ttu-id="d74c4-141">Nu kan du granska information som indata och utdata, till exempel:</span><span class="sxs-lookup"><span data-stu-id="d74c4-141">You can now review details like inputs and outputs, for example:</span></span>

   ![Utlösaren händelseinformationen för utdata](media/logic-apps-monitor-your-logic-apps/trigger-details.png)

<a name="azure-diagnostics"></a>

## <a name="turn-on-diagnostics-logging-for-your-logic-app"></a><span data-ttu-id="d74c4-143">Aktivera loggning för din logikapp diagnostik</span><span class="sxs-lookup"><span data-stu-id="d74c4-143">Turn on diagnostics logging for your logic app</span></span>

<span data-ttu-id="d74c4-144">För bättre felsökning med runtime information och händelser som du kan ställa in diagnostik loggning med [Azure logganalys](../log-analytics/log-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d74c4-144">For richer debugging with runtime details and events, you can set up diagnostics logging with [Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span></span> <span data-ttu-id="d74c4-145">Log Analytics är en tjänst i [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) som övervakar molnet och lokala miljöer toohelp du upprätthålla sin tillgänglighet och prestanda.</span><span class="sxs-lookup"><span data-stu-id="d74c4-145">Log Analytics is a service in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) that monitors your cloud and on-premises environments toohelp you maintain their availability and performance.</span></span> 

<span data-ttu-id="d74c4-146">Innan du börjar måste du toohave en OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="d74c4-146">Before you start, you need toohave an OMS workspace.</span></span> <span data-ttu-id="d74c4-147">Läs [hur toocreate en OMS-arbetsyta](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d74c4-147">Learn [how toocreate an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span>

1. <span data-ttu-id="d74c4-148">I hello [Azure-portalen](https://portal.azure.com), söka efter och välj din logikapp.</span><span class="sxs-lookup"><span data-stu-id="d74c4-148">In hello [Azure portal](https://portal.azure.com), find and select your logic app.</span></span> 

2. <span data-ttu-id="d74c4-149">Hello logik app bladet menyn under **övervakning**, Välj **diagnostik** > **diagnostikinställningar**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-149">On hello logic app blade menu, under **Monitoring**, choose **Diagnostics** > **Diagnostic Settings**.</span></span>

   ![Gå tooMonitoring, diagnostik, diagnostikinställningar](media/logic-apps-monitor-your-logic-apps/logic-app-diagnostics.png)

3. <span data-ttu-id="d74c4-151">Under **diagnostikinställningarna**, Välj **på**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-151">Under **Diagnostics settings**, choose **On**.</span></span>

   ![Aktivera diagnostikloggar](media/logic-apps-monitor-your-logic-apps/turn-on-diagnostics-logic-app.png)

4. <span data-ttu-id="d74c4-153">Nu välja hello OMS arbetsytan och händelse kategori för loggning som visas:</span><span class="sxs-lookup"><span data-stu-id="d74c4-153">Now select hello OMS workspace and event category for logging as shown:</span></span>

   1. <span data-ttu-id="d74c4-154">Välj **skicka tooLog Analytics**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-154">Select **Send tooLog Analytics**.</span></span> 
   2. <span data-ttu-id="d74c4-155">Under **logganalys**, Välj **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-155">Under **Log Analytics**, choose **Configure**.</span></span> 
   3. <span data-ttu-id="d74c4-156">Under **OMS arbetsytor**, Välj hello OMS arbetsytan toouse för loggning.</span><span class="sxs-lookup"><span data-stu-id="d74c4-156">Under **OMS Workspaces**, select hello OMS workspace toouse for logging.</span></span>
   4. <span data-ttu-id="d74c4-157">Under **loggen**väljer hello **WorkflowRuntime** kategori.</span><span class="sxs-lookup"><span data-stu-id="d74c4-157">Under **Log**, select hello **WorkflowRuntime** category.</span></span>
   5. <span data-ttu-id="d74c4-158">Välj hello mått intervall.</span><span class="sxs-lookup"><span data-stu-id="d74c4-158">Choose hello metric interval.</span></span>
   6. <span data-ttu-id="d74c4-159">När du är klar väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-159">When you're done, choose **Save**.</span></span>

   ![Välj OMS-arbetsytan och data för loggning](media/logic-apps-monitor-your-logic-apps/send-diagnostics-data-log-analytics-workspace.png)

<span data-ttu-id="d74c4-161">Nu hittar du händelser och andra data för utlösaren händelser, kör händelser och händelser för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d74c4-161">Now, you can find events and other data for trigger events, run events, and action events.</span></span>

<a name="find-events"></a>

## <a name="find-events-and-data-for-your-logic-app"></a><span data-ttu-id="d74c4-162">Sök efter händelser och data för din logikapp</span><span class="sxs-lookup"><span data-stu-id="d74c4-162">Find events and data for your logic app</span></span>

<span data-ttu-id="d74c4-163">toofind och visa händelser i din logikapp som utlösare händelser, kör händelser, och åtgärden händelser, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="d74c4-163">toofind and view events in your logic app, like trigger events, run events, and action events, follow these steps.</span></span>

1. <span data-ttu-id="d74c4-164">I hello [Azure-portalen](https://portal.azure.com), Välj **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-164">In hello [Azure portal](https://portal.azure.com), choose **More Services**.</span></span> <span data-ttu-id="d74c4-165">Sök efter ”logganalys” och välj sedan **logganalys** som visas här:</span><span class="sxs-lookup"><span data-stu-id="d74c4-165">Search for "log analytics", then choose **Log Analytics** as shown here:</span></span>

   ![Välj ”logganalys”](media/logic-apps-monitor-your-logic-apps/browseloganalytics.png)

2. <span data-ttu-id="d74c4-167">Under **logganalys**, söka efter och välj din OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="d74c4-167">Under **Log Analytics**, find and select your OMS workspace.</span></span> 

   ![Välj din OMS-arbetsyta](media/logic-apps-monitor-your-logic-apps/selectla.png)

3. <span data-ttu-id="d74c4-169">Under **Management**, Välj **OMS-portalen**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-169">Under **Management**, choose **OMS Portal**.</span></span>

   ![Välj ”OMS-portalen”](media/logic-apps-monitor-your-logic-apps/omsportalpage.png)

4. <span data-ttu-id="d74c4-171">Välj på startsidan OMS **loggen Sök**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-171">On your OMS home page, choose **Log Search**.</span></span>

   ![På startsidan OMS väljer du ”loggen Sök”](media/logic-apps-monitor-your-logic-apps/logsearch.png)

   <span data-ttu-id="d74c4-173">ELLER</span><span class="sxs-lookup"><span data-stu-id="d74c4-173">-or-</span></span>

   ![Välj ”Sök loggen” på hello OMS-menyn](media/logic-apps-monitor-your-logic-apps/logsearch-2.png)

5. <span data-ttu-id="d74c4-175">Ange ett fält som du vill toofind, i hello sökrutan och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-175">In hello search box, specify a field that you want toofind, and press **Enter**.</span></span> <span data-ttu-id="d74c4-176">När du börjar skriva in visas OMS möjliga matchningar och åtgärder som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="d74c4-176">When you start typing, OMS shows you possible matches and operations that you can use.</span></span> 

   <span data-ttu-id="d74c4-177">Till exempel toofind hello topp 10 händelser som inträffade, ange och välj sökfrågan: **kategori = WorkflowRuntime | topp 10**</span><span class="sxs-lookup"><span data-stu-id="d74c4-177">For example, toofind hello top 10 events that happened, enter and select this search query: **Category=WorkflowRuntime |top 10**</span></span>

   ![Ange söksträngen](media/logic-apps-monitor-your-logic-apps/oms-start-query.png)

   <span data-ttu-id="d74c4-179">Lär dig mer om [hur toofind data i logganalys](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="d74c4-179">Learn more about [how toofind data in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

6. <span data-ttu-id="d74c4-180">Välj hello tidsram på hello resultatsidan i vänstra hello-fältet som du vill tooview.</span><span class="sxs-lookup"><span data-stu-id="d74c4-180">On hello results page, in hello left bar, choose hello timeframe that you want tooview.</span></span>
<span data-ttu-id="d74c4-181">toorefine frågan genom att lägga till ett filter, Välj **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-181">toorefine your query by adding a filter, choose **+Add**.</span></span>

   ![Välj tidsram för frågeresultat](media/logic-apps-monitor-your-logic-apps/query-results.png)

7. <span data-ttu-id="d74c4-183">Under **Lägg till filter**, ange hello Filternamn så att du kan hitta hello filter som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="d74c4-183">Under **Add Filters**, enter hello filter name so you can find hello filter you want.</span></span> <span data-ttu-id="d74c4-184">Välj hello filter och välj **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-184">Select hello filter, and choose **+Add**.</span></span>

   <span data-ttu-id="d74c4-185">Det här exemplet använder hello ordet ”status” toofind misslyckades händelser under **AzureDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-185">This example uses hello word "status" toofind failed events under **AzureDiagnostics**.</span></span>
   <span data-ttu-id="d74c4-186">Här hello filter för **status_s** har redan valts.</span><span class="sxs-lookup"><span data-stu-id="d74c4-186">Here hello filter for **status_s** is already selected.</span></span>

   ![Välj filter](media/logic-apps-monitor-your-logic-apps/log-search-add-filter.png)

8. <span data-ttu-id="d74c4-188">I vänster hello-fältet väljer hello filtervärdet som du vill toouse och välj **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-188">In hello left bar, select hello filter value that you want toouse, and choose **Apply**.</span></span>

   ![Välj filtervärdet, välj ”Använd”](media/logic-apps-monitor-your-logic-apps/log-search-apply-filter.png)

9. <span data-ttu-id="d74c4-190">Returnerar nu toohello fråga som du utvecklar.</span><span class="sxs-lookup"><span data-stu-id="d74c4-190">Now return toohello query that you're building.</span></span> <span data-ttu-id="d74c4-191">Frågan har uppdaterats med ditt valda filter och värdet.</span><span class="sxs-lookup"><span data-stu-id="d74c4-191">Your query is updated with your selected filter and value.</span></span> <span data-ttu-id="d74c4-192">Tidigare resultaten filtreras nu för.</span><span class="sxs-lookup"><span data-stu-id="d74c4-192">Your previous results are now filtered too.</span></span>

   ![Returnera tooyour fråga med filtrerade resultat](media/logic-apps-monitor-your-logic-apps/log-search-query-filtered-results.png)

10. <span data-ttu-id="d74c4-194">toosave frågan för framtida användning, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="d74c4-194">toosave your query for future use, choose **Save**.</span></span> <span data-ttu-id="d74c4-195">Läs [hur toosave frågan](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).</span><span class="sxs-lookup"><span data-stu-id="d74c4-195">Learn [how toosave your query](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).</span></span>

<a name="extend-diagnostic-data"></a>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a><span data-ttu-id="d74c4-196">Utöka hur och när du använder diagnostikdata med andra tjänster</span><span class="sxs-lookup"><span data-stu-id="d74c4-196">Extend how and where you use diagnostic data with other services</span></span>

<span data-ttu-id="d74c4-197">Du kan utöka hur du använder din logikapp diagnostikdata med andra Azure-tjänster, till exempel tillsammans med Azure Log Analytics:</span><span class="sxs-lookup"><span data-stu-id="d74c4-197">Along with Azure Log Analytics, you can extend how you use your logic app's diagnostic data with other Azure services, for example:</span></span> 

* [<span data-ttu-id="d74c4-198">Arkivera Azure Diagnostics loggar i Azure-lagring</span><span class="sxs-lookup"><span data-stu-id="d74c4-198">Archive Azure Diagnostics Logs in Azure Storage</span></span>](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [<span data-ttu-id="d74c4-199">Dataströmmen Azure Diagnostics loggar tooAzure Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="d74c4-199">Stream Azure Diagnostics Logs tooAzure Event Hubs</span></span>](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

<span data-ttu-id="d74c4-200">Du kan sedan get realtid övervakning med hjälp av telemetri och analyser från andra tjänster som [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) och [Power BI](../log-analytics/log-analytics-powerbi.md).</span><span class="sxs-lookup"><span data-stu-id="d74c4-200">You can then get real-time monitoring by using telemetry and analytics from other services, like [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) and [Power BI](../log-analytics/log-analytics-powerbi.md).</span></span> <span data-ttu-id="d74c4-201">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d74c4-201">For example:</span></span>

* [<span data-ttu-id="d74c4-202">Dataströmmen data från Händelsehubbar tooStream Analytics</span><span class="sxs-lookup"><span data-stu-id="d74c4-202">Stream data from Event Hubs tooStream Analytics</span></span>](../stream-analytics/stream-analytics-define-inputs.md)
* [<span data-ttu-id="d74c4-203">Analysera strömmande data med Stream Analytics och skapa en instrumentpanel för analys i realtid i Power BI</span><span class="sxs-lookup"><span data-stu-id="d74c4-203">Analyze streaming data with Stream Analytics and create a real-time analytics dashboard in Power BI</span></span>](../stream-analytics/stream-analytics-power-bi-dashboard.md)

<span data-ttu-id="d74c4-204">Baserat på hello-alternativ som du vill konfigurera, ska du se till att du första [skapa ett Azure storage-konto](../storage/common/storage-create-storage-account.md) eller [skapar en Azure händelsehubb](../event-hubs/event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="d74c4-204">Based on hello options that you want set up, make sure that you first [create an Azure storage account](../storage/common/storage-create-storage-account.md) or [create an Azure event hub](../event-hubs/event-hubs-create.md).</span></span> <span data-ttu-id="d74c4-205">Välj sedan hello alternativ för där du vill toosend diagnostikdata:</span><span class="sxs-lookup"><span data-stu-id="d74c4-205">Then select hello options for where you want toosend diagnostic data:</span></span>

![Skicka data tooAzure lagring konto eller event hub](./media/logic-apps-monitor-your-logic-apps/storage-account-event-hubs.png)

> [!NOTE]
> <span data-ttu-id="d74c4-207">Kvarhållningsperioder gäller bara när du väljer toouse ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d74c4-207">Retention periods apply only when you choose toouse a storage account.</span></span>

<a name="add-azure-alerts"></a>

## <a name="set-up-alerts-for-your-logic-app"></a><span data-ttu-id="d74c4-208">Konfigurera aviseringar för din logikapp</span><span class="sxs-lookup"><span data-stu-id="d74c4-208">Set up alerts for your logic app</span></span>

<span data-ttu-id="d74c4-209">ställa in toomonitor specifika mått eller överskred tröskelvärden för din logikapp [aviseringar i Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="d74c4-209">toomonitor specific metrics or exceeded thresholds for your logic app, set up [alerts in Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md).</span></span> <span data-ttu-id="d74c4-210">Lär dig mer om [mått i Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="d74c4-210">Learn about [metrics in Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).</span></span> 

<span data-ttu-id="d74c4-211">tooset in aviseringar utan [Azure logganalys](../log-analytics/log-analytics-overview.md), Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="d74c4-211">tooset up alerts without [Azure Log Analytics](../log-analytics/log-analytics-overview.md), follow these steps.</span></span> <span data-ttu-id="d74c4-212">För mer avancerade kriterier som aviseringar och åtgärder, [konfigurera logganalys](#azure-diagnostics) för.</span><span class="sxs-lookup"><span data-stu-id="d74c4-212">For more advanced alerts criteria and actions, [set up Log Analytics](#azure-diagnostics) too.</span></span>

1. <span data-ttu-id="d74c4-213">Hello logik app bladet menyn under **övervakning**, Välj **diagnostik** > **Varna regler** > **Lägg till avisering**som visas här:</span><span class="sxs-lookup"><span data-stu-id="d74c4-213">On hello logic app blade menu, under **Monitoring**, choose **Diagnostics** > **Alert rules** > **Add alert** as shown here:</span></span>

   ![Lägga till en avisering för din logikapp](media/logic-apps-monitor-your-logic-apps/set-up-alerts.png)

2. <span data-ttu-id="d74c4-215">På hello **lägga till en varningsregel** bladet skapa aviseringen som visas:</span><span class="sxs-lookup"><span data-stu-id="d74c4-215">On hello **Add an alert rule** blade, create your alert as shown:</span></span>

   1. <span data-ttu-id="d74c4-216">Under **resurs**, Välj din logikapp, om det inte redan är markerad.</span><span class="sxs-lookup"><span data-stu-id="d74c4-216">Under **Resource**, select your logic app, if not already selected.</span></span> 
   2. <span data-ttu-id="d74c4-217">Ange ett namn och beskrivning för aviseringen.</span><span class="sxs-lookup"><span data-stu-id="d74c4-217">Give a name and description for your alert.</span></span>
   3. <span data-ttu-id="d74c4-218">Välj en **mått** eller den händelse som du vill tootrack.</span><span class="sxs-lookup"><span data-stu-id="d74c4-218">Select a **Metric** or event that you want tootrack.</span></span>
   4. <span data-ttu-id="d74c4-219">Välj en **villkoret**, ange en **tröskelvärdet** för hello mått och välj hello **Period** för övervakning av det här måttet.</span><span class="sxs-lookup"><span data-stu-id="d74c4-219">Select a **Condition**, specify a **Threshold** for hello metric, and select hello **Period** for monitoring this metric.</span></span>
   5. <span data-ttu-id="d74c4-220">Välj om e-toosend för hello aviseringen.</span><span class="sxs-lookup"><span data-stu-id="d74c4-220">Select whether toosend mail for hello alert.</span></span> 
   6. <span data-ttu-id="d74c4-221">Ange andra e-postadresser för att skicka hello avisering.</span><span class="sxs-lookup"><span data-stu-id="d74c4-221">Specify any other email addresses for sending hello alert.</span></span> 
   <span data-ttu-id="d74c4-222">Du kan också ange en Webhooksadressen där du vill toosend hello avisering.</span><span class="sxs-lookup"><span data-stu-id="d74c4-222">You can also specify a webhook URL where you want toosend hello alert.</span></span>

   <span data-ttu-id="d74c4-223">Till exempel regeln skickar en avisering när fem eller fler körningar misslyckas på en timme:</span><span class="sxs-lookup"><span data-stu-id="d74c4-223">For example, this rule sends an alert when five or more runs fail in an hour:</span></span>

   ![Skapa mått varningsregel](media/logic-apps-monitor-your-logic-apps/create-alert-rule.png)

> [!TIP]
> <span data-ttu-id="d74c4-225">toorun en logikapp från en varning och du kan inkludera hello [begäran utlösaren](../connectors/connectors-native-reqres.md) i arbetsflödet, där du kan utföra åtgärder som de här exemplen:</span><span class="sxs-lookup"><span data-stu-id="d74c4-225">toorun a logic app from an alert, you can include hello [request trigger](../connectors/connectors-native-reqres.md) in your workflow, which lets you perform tasks like these examples:</span></span>
> 
> * [<span data-ttu-id="d74c4-226">Bokför tooSlack</span><span class="sxs-lookup"><span data-stu-id="d74c4-226">Post tooSlack</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [<span data-ttu-id="d74c4-227">Skicka en text</span><span class="sxs-lookup"><span data-stu-id="d74c4-227">Send a text</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [<span data-ttu-id="d74c4-228">Lägg till en meddelandekö tooa</span><span class="sxs-lookup"><span data-stu-id="d74c4-228">Add a message tooa queue</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

<a name="diagnostic-event-properties"></a>

## <a name="azure-diagnostics-event-settings-and-details"></a><span data-ttu-id="d74c4-229">Azure Diagnostics händelse inställningar och information</span><span class="sxs-lookup"><span data-stu-id="d74c4-229">Azure Diagnostics event settings and details</span></span>

<span data-ttu-id="d74c4-230">Varje diagnostiska händelsen innehåller information om din logikapp och att händelsen, till exempel hello status, starttid, Sluttid och så vidare.</span><span class="sxs-lookup"><span data-stu-id="d74c4-230">Each diagnostic event has details about your logic app and that event, for example, hello status, start time, end time, and so on.</span></span> <span data-ttu-id="d74c4-231">tooprogrammatically Konfigurera övervakning, spårning och loggning, organisationsenheten kan använda dessa uppgifter med hello [REST API: et för Azure Logikappar](https://docs.microsoft.com/rest/api/logic) och hello [REST API för Azure-diagnostik](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).</span><span class="sxs-lookup"><span data-stu-id="d74c4-231">tooprogrammatically set up monitoring, tracking, and logging, ou can use these details with hello [REST API for Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) and hello [REST API for Azure Diagnostics](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).</span></span>

<span data-ttu-id="d74c4-232">Till exempel hello `ActionCompleted` händelsen har hello `clientTrackingId` och `trackedProperties` egenskaper som du kan använda för att spåra och övervaka:</span><span class="sxs-lookup"><span data-stu-id="d74c4-232">For example, hello `ActionCompleted` event has hello `clientTrackingId` and `trackedProperties` properties that you can use for tracking and monitoring:</span></span>

``` json
{
    "time": "2016-07-09T17:09:54.4773148Z",
    "workflowId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
    "resourceId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
    "category": "WorkflowRuntime",
    "level": "Information",
    "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
    "properties": {
        "$schema": "2016-06-01",
        "startTime": "2016-07-09T17:09:53.4336305Z",
        "endTime": "2016-07-09T17:09:53.5430281Z",
        "status": "Succeeded",
        "code": "OK",
        "resource": {
            "subscriptionId": "<subscription-ID>",
            "resourceGroupName": "MyResourceGroup",
            "workflowId": "cff00d5458f944d5a766f2f9ad142553",
            "workflowName": "MyLogicApp",
            "runId": "08587361146922712057",
            "location": "westus",
            "actionName": "Http"
        },
        "correlation": {
            "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
            "clientTrackingId": "<my-custom-tracking-ID>"
        },
        "trackedProperties": {
            "myTrackedProperty": "<value>"
        }
    }
}
```

* <span data-ttu-id="d74c4-233">`clientTrackingId`: Om det inte angetts i Azure genererar detta ID och korrelerar händelser över en logikapp som körs automatiskt, inklusive alla kapslade arbetsflöden som anropas från hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="d74c4-233">`clientTrackingId`: If not provided, Azure automatically generates this ID and correlates events across a logic app run, including any nested workflows that are called from hello logic app.</span></span> <span data-ttu-id="d74c4-234">Du kan ange detta ID från en utlösare manuellt genom att skicka en `x-ms-client-tracking-id` huvud med din anpassade ID-värde i hello utlösarbegäran.</span><span class="sxs-lookup"><span data-stu-id="d74c4-234">You can manually specify this ID from a trigger by passing a `x-ms-client-tracking-id` header with your custom ID value in hello trigger request.</span></span> <span data-ttu-id="d74c4-235">Du kan använda en utlösare för begäran, http-utlösaren eller webhook-utlösare.</span><span class="sxs-lookup"><span data-stu-id="d74c4-235">You can use a request trigger, HTTP trigger, or webhook trigger.</span></span>

* <span data-ttu-id="d74c4-236">`trackedProperties`: tootrack indata eller utdata i diagnostikdata, du kan lägga till egenskaper för spårade tooactions i din logikapp JSON-definitionen.</span><span class="sxs-lookup"><span data-stu-id="d74c4-236">`trackedProperties`: tootrack inputs or outputs in diagnostics data, you can add tracked properties tooactions in your logic app's JSON definition.</span></span> <span data-ttu-id="d74c4-237">Spårade egenskaper kan spåra bara en enda åtgärd indata och utdata, men du kan använda hello `correlation` egenskaper för händelser toocorrelate över åtgärder körs.</span><span class="sxs-lookup"><span data-stu-id="d74c4-237">Tracked properties can track only a single action's inputs and outputs, but you can use hello `correlation` properties of events toocorrelate across actions in a run.</span></span>

  <span data-ttu-id="d74c4-238">tootrack en eller flera egenskaper, lägga till hello `trackedProperties` avsnittet och hello egenskaper för definition av toohello åtgärd.</span><span class="sxs-lookup"><span data-stu-id="d74c4-238">tootrack one or more properties, add hello `trackedProperties` section and hello properties you want toohello action definition.</span></span> <span data-ttu-id="d74c4-239">Anta att du vill tootrack data som en ”order-ID” i din telemetri:</span><span class="sxs-lookup"><span data-stu-id="d74c4-239">For example, suppose you want tootrack data like an "order ID" in your telemetry:</span></span>

  ``` json
  "myAction": {
    "type": "http",
    "inputs": {
        "uri": "http://uri",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": "@triggerBody()"
    },
    "trackedProperties": {
        "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
        "myActionHTTPValue": "@action()['outputs']['body']['<content>']",
        "transactionId": "@action()['inputs']['body']['<content>']"
    }
  }
  ```

## <a name="next-steps"></a><span data-ttu-id="d74c4-240">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d74c4-240">Next steps</span></span>

* [<span data-ttu-id="d74c4-241">Skapa mallar för logik app-distribution och versionshantering</span><span class="sxs-lookup"><span data-stu-id="d74c4-241">Create templates for logic app deployment and release management</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
* [<span data-ttu-id="d74c4-242">B2B-scenarier med Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="d74c4-242">B2B scenarios with Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [<span data-ttu-id="d74c4-243">Övervaka B2B-meddelanden</span><span class="sxs-lookup"><span data-stu-id="d74c4-243">Monitor B2B messages</span></span>](../logic-apps/logic-apps-monitor-b2b-message.md)