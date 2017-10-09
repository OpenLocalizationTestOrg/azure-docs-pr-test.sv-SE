---
title: "aaaQuery för B2B-meddelanden i Operations Management Suite - Azure Logic Apps | Microsoft Docs"
description: "Skapa frågor tootrack AS2-, X 12- och EDIFACT-meddelanden i hello Operations Management Suite"
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
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: aee6644ff19add8f074ed5f1725db87b1d3b74b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="query-for-as2-x12-and-edifact-messages-in-hello-microsoft-operations-management-suite-oms"></a><span data-ttu-id="9c1c0-103">Fråga efter AS2-, X 12- och EDIFACT-meddelanden i hello Microsoft Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="9c1c0-103">Query for AS2, X12, and EDIFACT messages in hello Microsoft Operations Management Suite (OMS)</span></span>

<span data-ttu-id="9c1c0-104">toofind hello AS2-, X 12 eller EDIFACT-meddelanden som du spårar med [Azure logganalys](../log-analytics/log-analytics-overview.md) i hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), du kan skapa frågor som filtrerar åtgärder baserat på specifika kriterier.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-104">toofind hello AS2, X12, or EDIFACT messages that you're tracking with [Azure Log Analytics](../log-analytics/log-analytics-overview.md) in hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), you can create queries that filter actions based on specific criteria.</span></span> <span data-ttu-id="9c1c0-105">Du kan till exempel hitta meddelanden baserat på specifika interchange kontroll.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-105">For example, you can find messages based on a specific interchange control number.</span></span>

## <a name="requirements"></a><span data-ttu-id="9c1c0-106">Krav</span><span class="sxs-lookup"><span data-stu-id="9c1c0-106">Requirements</span></span>

* <span data-ttu-id="9c1c0-107">En logikapp som har konfigurerats med diagnostikloggning.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-107">A logic app that's set up with diagnostics logging.</span></span> <span data-ttu-id="9c1c0-108">Läs [hur toocreate en logikapp](../logic-apps/logic-apps-create-a-logic-app.md) och [hur tooset in loggning för att logikapp](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span><span class="sxs-lookup"><span data-stu-id="9c1c0-108">Learn [how toocreate a logic app](../logic-apps/logic-apps-create-a-logic-app.md) and [how tooset up logging for that logic app](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span></span>

* <span data-ttu-id="9c1c0-109">Ett integration-konto som har konfigurerats med övervakning och loggning.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-109">An integration account that's set up with monitoring and logging.</span></span> <span data-ttu-id="9c1c0-110">Läs [hur toocreate kontonamn integration](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) och [hur tooset in övervakning och loggning för det kontot](../logic-apps/logic-apps-monitor-b2b-message.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c0-110">Learn [how toocreate an integration account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) and [how tooset up monitoring and logging for that account](../logic-apps/logic-apps-monitor-b2b-message.md).</span></span>

* <span data-ttu-id="9c1c0-111">Om du inte redan har gjort [publicera diagnostikdata tooLog Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) och [konfigurera meddelandespårning i OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c0-111">If you haven't already, [publish diagnostic data tooLog Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) and [set up message tracking in OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>

> [!NOTE]
> <span data-ttu-id="9c1c0-112">När du har uppfyllt kraven för hello tidigare, bör du ha en arbetsyta i hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c0-112">After you've met hello previous requirements, you should have a workspace in hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span></span> <span data-ttu-id="9c1c0-113">Du bör använda hello samma OMS-arbetsyta för att spåra dina B2B-kommunikation i OMS.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-113">You should use hello same OMS workspace for tracking your B2B communication in OMS.</span></span> 
>  
> <span data-ttu-id="9c1c0-114">Lär dig mer om du inte har en OMS-arbetsyta [hur toocreate en OMS-arbetsyta](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c0-114">If you don't have an OMS workspace, learn [how toocreate an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span>

## <a name="create-message-queries-with-filters-in-hello-operations-management-suite-portal"></a><span data-ttu-id="9c1c0-115">Skapa frågor med filter i hello Operations Management Suite-portalen</span><span class="sxs-lookup"><span data-stu-id="9c1c0-115">Create message queries with filters in hello Operations Management Suite portal</span></span>

<span data-ttu-id="9c1c0-116">Det här exemplet visar hur du kan hitta meddelanden baserat på deras interchange kontrollnummer.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-116">This example shows how you can find messages based on their interchange control number.</span></span>

> [!TIP] 
> <span data-ttu-id="9c1c0-117">Om du vet namnet på din OMS-arbetsytan gå tooyour arbetsytan startsidan (`https://{your-workspace-name}.portal.mms.microsoft.com`), och starta i steg 4.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-117">If you know your OMS workspace name, go tooyour workspace home page (`https://{your-workspace-name}.portal.mms.microsoft.com`), and start at Step 4.</span></span> <span data-ttu-id="9c1c0-118">Annars starta i steg 1.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-118">Otherwise, start at Step 1.</span></span>

1. <span data-ttu-id="9c1c0-119">I hello [Azure-portalen](https://portal.azure.com), Välj **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-119">In hello [Azure portal](https://portal.azure.com), choose **More Services**.</span></span> <span data-ttu-id="9c1c0-120">Sök efter ”logganalys” och välj sedan **logganalys** som visas här:</span><span class="sxs-lookup"><span data-stu-id="9c1c0-120">Search for "log analytics", and then choose **Log Analytics** as shown here:</span></span>

   ![Hitta logganalys](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. <span data-ttu-id="9c1c0-122">Under **logganalys**, söka efter och välj din OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-122">Under **Log Analytics**, find and select your OMS workspace.</span></span>

   ![Välj din OMS-arbetsyta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. <span data-ttu-id="9c1c0-124">Under **Management**, Välj **OMS-portalen**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-124">Under **Management**, choose **OMS Portal**.</span></span>

   ![Välj OMS-portalen](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/omsportalpage.png)

4. <span data-ttu-id="9c1c0-126">Välj på startsidan OMS **loggen Sök**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-126">On your OMS home page, choose **Log Search**.</span></span>

   ![På startsidan OMS väljer du ”loggen Sök”](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   <span data-ttu-id="9c1c0-128">ELLER</span><span class="sxs-lookup"><span data-stu-id="9c1c0-128">-or-</span></span>

   ![Välj ”Sök loggen” på hello OMS-menyn](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

5. <span data-ttu-id="9c1c0-130">Ange ett fält som du vill toofind, i hello sökrutan och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-130">In hello search box, enter a field that you want toofind, and press **Enter**.</span></span> <span data-ttu-id="9c1c0-131">När du börjar skriva in visas OMS möjliga matchningar och åtgärder som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-131">When you start typing, OMS shows you possible matches and operations that you can use.</span></span> <span data-ttu-id="9c1c0-132">Lär dig mer om [hur toofind data i logganalys](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c0-132">Learn more about [how toofind data in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

   <span data-ttu-id="9c1c0-133">Det här exemplet söker efter händelser med **typ = AzureDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-133">This example searches for events with **Type=AzureDiagnostics**.</span></span>

   ![Börja skriva frågesträng](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

6. <span data-ttu-id="9c1c0-135">I vänster hello-fältet väljer hello tidsram som du vill tooview.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-135">In hello left bar, choose hello timeframe that you want tooview.</span></span> <span data-ttu-id="9c1c0-136">tooadd tooyour filterfråga, Välj **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-136">tooadd a filter tooyour query, choose **+Add**.</span></span>

   ![Lägg till filter tooquery](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

7. <span data-ttu-id="9c1c0-138">Under **Lägg till filter**, ange hello Filternamn så att du kan hitta hello filter som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-138">Under **Add Filters**, enter hello filter name so you can find hello filter you want.</span></span> <span data-ttu-id="9c1c0-139">Välj hello filter och välj **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-139">Select hello filter, and choose **+Add**.</span></span>

   <span data-ttu-id="9c1c0-140">toofind hello interchange kontrollnummer i det här exemplet söker efter hello ordet ”interchange” och väljer **event_record_messageProperties_interchangeControlNumber_s** som hello filter.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-140">toofind hello interchange control number, this example searches for hello word "interchange", and selects **event_record_messageProperties_interchangeControlNumber_s** as hello filter.</span></span>

   ![Välj filter](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

9. <span data-ttu-id="9c1c0-142">I vänster hello-fältet väljer hello filtervärdet som du vill toouse och välj **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-142">In hello left bar, select hello filter value that you want toouse, and choose **Apply**.</span></span>

   <span data-ttu-id="9c1c0-143">Det här exemplet väljer hello interchange kontrollnummer för hälsningsmeddelande som vi vill.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-143">This example selects hello interchange control number for hello messages we want.</span></span>

   ![Välj filtervärdet](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

10. <span data-ttu-id="9c1c0-145">Returnerar nu toohello fråga som du utvecklar.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-145">Now return toohello query that you're building.</span></span> <span data-ttu-id="9c1c0-146">Frågan har uppdaterats med ditt valda filter-händelsen och värdet.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-146">Your query has been updated with your selected filter event and value.</span></span> <span data-ttu-id="9c1c0-147">Tidigare resultaten filtreras nu för.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-147">Your previous results are now filtered too.</span></span>

    ![Returnera tooyour fråga med filtrerade resultat](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a><span data-ttu-id="9c1c0-149">Spara frågan för framtida användning</span><span class="sxs-lookup"><span data-stu-id="9c1c0-149">Save your query for future use</span></span>

1. <span data-ttu-id="9c1c0-150">Från din fråga på hello **loggen Sök** väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-150">From your query on hello **Log Search** page, choose **Save**.</span></span> <span data-ttu-id="9c1c0-151">Namnge din fråga, Välj en kategori och välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-151">Give your query a name, select a category, and choose **Save**.</span></span>

   ![Ge din fråga namn och kategori](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. <span data-ttu-id="9c1c0-153">tooview frågan, Välj **Favoriter**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-153">tooview your query, choose **Favorites**.</span></span>

   ![Välj ”Favoriter”](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. <span data-ttu-id="9c1c0-155">Under **sparade sökningar**, Markera frågan så att du kan visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-155">Under **Saved Searches**, select your query so that you can view hello results.</span></span> <span data-ttu-id="9c1c0-156">tooupdate hello frågan så att du kan hitta olika resultat redigera hello fråga.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-156">tooupdate hello query so you can find different results, edit hello query.</span></span>

   ![Välj din fråga](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-hello-operations-management-suite-portal"></a><span data-ttu-id="9c1c0-158">Hitta och köra sparade frågor i hello Operations Management Suite-portalen</span><span class="sxs-lookup"><span data-stu-id="9c1c0-158">Find and run saved queries in hello Operations Management Suite portal</span></span>

1. <span data-ttu-id="9c1c0-159">Öppna startsidan för OMS-arbetsytan (`https://{your-workspace-name}.portal.mms.microsoft.com`), och välj **loggen Sök**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-159">Open your OMS workspace home page (`https://{your-workspace-name}.portal.mms.microsoft.com`), and choose **Log Search**.</span></span>

   ![På startsidan OMS väljer du ”loggen Sök”](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   <span data-ttu-id="9c1c0-161">ELLER</span><span class="sxs-lookup"><span data-stu-id="9c1c0-161">-or-</span></span>

   ![Välj ”Sök loggen” på hello OMS-menyn](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. <span data-ttu-id="9c1c0-163">På hello **loggen Sök** startsidan, Välj **Favoriter**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-163">On hello **Log Search** home page, choose **Favorites**.</span></span>

   ![Välj ”Favoriter”](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. <span data-ttu-id="9c1c0-165">Under **sparade sökningar**, Markera frågan så att du kan visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-165">Under **Saved Searches**, select your query so that you can view hello results.</span></span> <span data-ttu-id="9c1c0-166">tooupdate hello frågan så att du kan hitta olika resultat redigera hello fråga.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-166">tooupdate hello query so you can find different results, edit hello query.</span></span>

   ![Välj din fråga](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a><span data-ttu-id="9c1c0-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9c1c0-168">Next steps</span></span>

* [<span data-ttu-id="9c1c0-169">AS2-spårningsscheman</span><span class="sxs-lookup"><span data-stu-id="9c1c0-169">AS2 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [<span data-ttu-id="9c1c0-170">X12-spårningsscheman</span><span class="sxs-lookup"><span data-stu-id="9c1c0-170">X12 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [<span data-ttu-id="9c1c0-171">Spårning av anpassade scheman</span><span class="sxs-lookup"><span data-stu-id="9c1c0-171">Custom tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)