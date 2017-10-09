---
title: "aaaMonitor och få insikter om din logikapp köras med hjälp av OMS - Azure Logic Apps | Microsoft Docs"
description: "Övervaka logik appen körs med logganalys och Operations Management Suite (OMS) tooget insikter och bättre felsökning information för felsökning och diagnostik"
author: divyaswarnkar
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/9/2017
ms.author: LADocs; divswa
ms.openlocfilehash: a76fd6d1ff5c0010550be0f991514ce95f659fd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-operations-management-suite-oms-and-log-analytics"></a><span data-ttu-id="28009-103">Övervaka och få insikter om logik appen körs med Operations Management Suite (OMS) och logganalys</span><span class="sxs-lookup"><span data-stu-id="28009-103">Monitor and get insights about logic app runs with Operations Management Suite (OMS) and Log Analytics</span></span>

<span data-ttu-id="28009-104">Övervakning och bättre felsökning information kan du aktivera logganalys på hello samtidigt när du skapar en logikapp.</span><span class="sxs-lookup"><span data-stu-id="28009-104">For monitoring and richer debugging information, you can turn on Log Analytics at hello same time when you create a logic app.</span></span> <span data-ttu-id="28009-105">Log Analytics tillhandahåller diagnostik loggning och övervakning för din logikapp körs hello Operations Management Suite (OMS)-portalen.</span><span class="sxs-lookup"><span data-stu-id="28009-105">Log Analytics provides diagnostics logging and monitoring for your logic app runs through hello Operations Management Suite (OMS) portal.</span></span> <span data-ttu-id="28009-106">När du lägger till hello Logic Apps Management lösning tooOMS hämta aggregerad status för din logik app körs och en specifik information som status, körningstid, omsändning status och Korrelations-ID: N.</span><span class="sxs-lookup"><span data-stu-id="28009-106">When you add hello Logic Apps Management solution tooOMS, you get aggregated status for your logic app runs and specific details like status, execution time, resubmission status, and correlation IDs.</span></span>

<span data-ttu-id="28009-107">Det här avsnittet visar hur tooturn på logganalys eller installera hello Logic Apps hanteringslösning i OMS så att du kan visa runtime händelser och data för din logikapp kör.</span><span class="sxs-lookup"><span data-stu-id="28009-107">This topic shows how tooturn on Log Analytics or install hello Logic Apps Management solution in OMS so you can view runtime events and data for your logic app run.</span></span>

 > [!TIP]
 > <span data-ttu-id="28009-108">toomonitor dina befintliga logikappar, Följ dessa steg för [aktivera diagnostikloggning och skicka logik app runtime data tooOMS](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span><span class="sxs-lookup"><span data-stu-id="28009-108">toomonitor your existing logic apps, follow these steps too [turn on diagnostic logging and send logic app runtime data tooOMS](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span></span>

## <a name="requirements"></a><span data-ttu-id="28009-109">Krav</span><span class="sxs-lookup"><span data-stu-id="28009-109">Requirements</span></span>

<span data-ttu-id="28009-110">Innan du börjar måste du toohave en OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="28009-110">Before you start, you need toohave an OMS workspace.</span></span> <span data-ttu-id="28009-111">Läs [hur toocreate en OMS-arbetsyta](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="28009-111">Learn [how toocreate an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span> 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a><span data-ttu-id="28009-112">Aktivera diagnostikloggning när du skapar logikappar</span><span class="sxs-lookup"><span data-stu-id="28009-112">Turn on diagnostics logging when creating logic apps</span></span>

1. <span data-ttu-id="28009-113">I [Azure-portalen](https://portal.azure.com), skapa en logikapp.</span><span class="sxs-lookup"><span data-stu-id="28009-113">In [Azure portal](https://portal.azure.com), create a logic app.</span></span> <span data-ttu-id="28009-114">Välj **nya** > **företagsintegration** > **Logikapp** > **skapa**.</span><span class="sxs-lookup"><span data-stu-id="28009-114">Choose **New** > **Enterprise Integration** > **Logic App** > **Create**.</span></span>

   ![Skapa en logikapp](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. <span data-ttu-id="28009-116">I hello **skapa logikapp** utför dessa uppgifter som visas:</span><span class="sxs-lookup"><span data-stu-id="28009-116">In hello **Create logic app** page, perform these tasks as shown:</span></span>

   1. <span data-ttu-id="28009-117">Ange ett namn för din logikapp och välj din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="28009-117">Provide a name for your logic app and select your Azure subscription.</span></span> 
   2. <span data-ttu-id="28009-118">Skapa eller välj en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="28009-118">Create or select an Azure resource group.</span></span>
   3. <span data-ttu-id="28009-119">Ange **logganalys** för**på**.</span><span class="sxs-lookup"><span data-stu-id="28009-119">Set **Log Analytics** too**On**.</span></span> 
   <span data-ttu-id="28009-120">Välj hello OMS-arbetsyta där du vill skicka data för din logikapp för körs.</span><span class="sxs-lookup"><span data-stu-id="28009-120">Select hello OMS workspace where you want too send data for your logic app runs.</span></span> 
   4. <span data-ttu-id="28009-121">När du är klar kan du välja **PIN-kod toodashboard** > **skapa**.</span><span class="sxs-lookup"><span data-stu-id="28009-121">When you're ready, choose **Pin toodashboard** > **Create**.</span></span>

      ![Skapa logikapp](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      <span data-ttu-id="28009-123">När du har slutfört det här steget Azure skapar din logikapp nu som är associerade med din OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="28009-123">After you finish this step, Azure creates your logic app, which is now associated with your OMS workspace.</span></span> 
      <span data-ttu-id="28009-124">Det här steget installerar också automatiskt hello Logic Apps hanteringslösning i OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="28009-124">Also, this step also automatically installs hello Logic Apps Management solution in your OMS workspace.</span></span>

3. <span data-ttu-id="28009-125">tooview logikappen körs i OMS, [fortsätta med de här stegen](#view-logic-app-runs-oms).</span><span class="sxs-lookup"><span data-stu-id="28009-125">tooview your logic app runs in OMS, [continue with these steps](#view-logic-app-runs-oms).</span></span>

## <a name="install-hello-logic-apps-management-solution-in-oms"></a><span data-ttu-id="28009-126">Installera hello Logic Apps hanteringslösning i OMS</span><span class="sxs-lookup"><span data-stu-id="28009-126">Install hello Logic Apps Management solution in OMS</span></span>

<span data-ttu-id="28009-127">Om du redan aktiverat logganalys när du skapade din logikapp, hoppar du över det här steget.</span><span class="sxs-lookup"><span data-stu-id="28009-127">If you already turned on Log Analytics when you created your logic app, skip this step.</span></span> <span data-ttu-id="28009-128">Du har redan hello Logic Apps hanteringslösning installerats i OMS.</span><span class="sxs-lookup"><span data-stu-id="28009-128">You already have hello Logic Apps Management solution installed in OMS.</span></span>

1. <span data-ttu-id="28009-129">I hello [Azure-portalen](https://portal.azure.com), Välj **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="28009-129">In hello [Azure portal](https://portal.azure.com), choose **More Services**.</span></span> <span data-ttu-id="28009-130">Sök efter ”logganalys” som filter och välja **logganalys** som visas:</span><span class="sxs-lookup"><span data-stu-id="28009-130">Search for "log analytics" as your filter, and choose **Log Analytics** as shown:</span></span>

   ![Välj ”logganalys”](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. <span data-ttu-id="28009-132">Under **logganalys**, söka efter och välj din OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="28009-132">Under **Log Analytics**, find and select your OMS workspace.</span></span> 

   ![Välj din OMS-arbetsyta](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. <span data-ttu-id="28009-134">Under **Management**, Välj **OMS-portalen**.</span><span class="sxs-lookup"><span data-stu-id="28009-134">Under **Management**, choose **OMS Portal**.</span></span>

   ![Välj ”OMS-portalen”](media/logic-apps-monitor-your-logic-apps-oms/oms-portal-page.png)

4. <span data-ttu-id="28009-136">På startsidan OMS om hello uppgradera banderoll visas, väljer du hello banderoll så att du först uppgradera din OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="28009-136">On your OMS homepage, if hello upgrade banner appears, choose hello banner so that you upgrade your OMS workspace first.</span></span> <span data-ttu-id="28009-137">Välj **lösningar galleriet**.</span><span class="sxs-lookup"><span data-stu-id="28009-137">Then choose **Solutions Gallery**.</span></span>

   ![Välj ”lösningar galleriet”](media/logic-apps-monitor-your-logic-apps-oms/solutions-gallery.png)

5. <span data-ttu-id="28009-139">Under **alla lösningar för**, söka efter och välj hello panelen för hello **Logic Apps Management** lösning.</span><span class="sxs-lookup"><span data-stu-id="28009-139">Under **All solutions**, find and choose hello tile for hello **Logic Apps Management** solution.</span></span>

   ![Välj ”Logic Apps Management”](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-management-tile2.png)

6. <span data-ttu-id="28009-141">tooinstall hello lösning i OMS-arbetsyta, Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="28009-141">tooinstall hello solution in your OMS workspace, choose **Add**.</span></span>

   ![Välj ”Lägg till” för ”Logic Apps Management”](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-oms-workspace"></a><span data-ttu-id="28009-143">Visa din logikapp som körs i din OMS-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="28009-143">View your logic app runs in your OMS workspace</span></span>

1. <span data-ttu-id="28009-144">tooview hello antal och status för din logikapp körs gå toohello översiktssidan för din OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="28009-144">tooview hello count and status for your logic app runs, go toohello overview page for your OMS workspace.</span></span> <span data-ttu-id="28009-145">Granska hello information om hello **Logic Apps Management** panelen.</span><span class="sxs-lookup"><span data-stu-id="28009-145">Review hello details on hello **Logic Apps Management** tile.</span></span>

   ![Översikt över panelen visar logik app kör antal och status](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   > [!Note]
   > <span data-ttu-id="28009-147">Om den här uppgraderingen banderoll visas i stället för hello Logic Apps Management panelen, väljer du hello banderoll så att du först uppgradera din OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="28009-147">If this upgrade banner appears instead of hello Logic Apps Management tile, choose hello banner so that you upgrade your OMS workspace first.</span></span>
  
   > ![Uppgradera ”OMS-arbetsytan”](media/logic-apps-monitor-your-logic-apps-oms/oms-upgrade-banner.png)

2. <span data-ttu-id="28009-149">tooview en sammanfattning med mer information om logik appen körs, Välj hello **Logic Apps Management** panelen.</span><span class="sxs-lookup"><span data-stu-id="28009-149">tooview a summary with more details about your logic app runs, choose hello **Logic Apps Management** tile.</span></span>

   <span data-ttu-id="28009-150">Här kan grupperas logik appen körs efter namn eller Körstatus.</span><span class="sxs-lookup"><span data-stu-id="28009-150">Here, your logic app runs are grouped by name or by execution status.</span></span>

   ![Översikt över statusen för din logikapp körs](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
3. <span data-ttu-id="28009-152">tooview alla hello körs för en specifik logikapp eller status, Välj hello rad för en logikapp eller status.</span><span class="sxs-lookup"><span data-stu-id="28009-152">tooview all hello runs for a specific logic app or status, select hello row for a logic app or a status.</span></span>

   <span data-ttu-id="28009-153">Här är ett exempel som visar alla hello körs för en specifik logikapp:</span><span class="sxs-lookup"><span data-stu-id="28009-153">Here is an example that shows all hello runs for a specific logic app:</span></span>

   ![Vyn körs för en logikapp eller status](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   > [!NOTE]
   > <span data-ttu-id="28009-155">Hej **omsändning** kolumnen visar ”Ja” för Kör som uppstår vid körningen av en skickad igen.</span><span class="sxs-lookup"><span data-stu-id="28009-155">hello **Resubmission** column shows "Yes" for runs that result from a resubmitted run.</span></span>

4. <span data-ttu-id="28009-156">toofilter dessa resultat, du kan utföra både på klientsidan och serversidan filtrering.</span><span class="sxs-lookup"><span data-stu-id="28009-156">toofilter these results, you can perform both client-side and server-side filtering.</span></span>

   * <span data-ttu-id="28009-157">Klientsidans filter: Välj hello filter som du vill använda för varje kolumn.</span><span class="sxs-lookup"><span data-stu-id="28009-157">Client-side filter: For each column, choose hello filters that you want.</span></span> 
   <span data-ttu-id="28009-158">Här följer några exempel:</span><span class="sxs-lookup"><span data-stu-id="28009-158">Here are some examples:</span></span>

     ![Exempel kolumnfiltren](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * <span data-ttu-id="28009-160">Serversidan filter: toochoose en viss tid fönster eller toolimit hello antal som visas, Använd hello scope kontroll hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="28009-160">Server-side filter: toochoose a specific time window or toolimit hello number of runs that appear, use hello scope control at hello top of hello page.</span></span> 
   <span data-ttu-id="28009-161">Som standard visas endast 1 000 poster i taget.</span><span class="sxs-lookup"><span data-stu-id="28009-161">By default, only 1,000 records appear at a time.</span></span> 
   
     ![Ändra hello tidsfönstret](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
5. <span data-ttu-id="28009-163">alla tooview hello åtgärder och deras information för en specifik kör, väljer en rad som öppnas hello loggen söksidan.</span><span class="sxs-lookup"><span data-stu-id="28009-163">tooview all hello actions and their details for a specific run, select a row, which opens hello Log Search page.</span></span> 

   * <span data-ttu-id="28009-164">tooview informationen i en tabell, Välj **tabellen**.</span><span class="sxs-lookup"><span data-stu-id="28009-164">tooview this information in a table, choose **Table**.</span></span>
   * <span data-ttu-id="28009-165">Du kan redigera hello frågesträngen hello sökfältet toochange hello frågan.</span><span class="sxs-lookup"><span data-stu-id="28009-165">toochange hello query, you can edit hello query string in hello search bar.</span></span> 
   <span data-ttu-id="28009-166">En bättre upplevelse, Välj **Advanced Analytics**.</span><span class="sxs-lookup"><span data-stu-id="28009-166">For a better experience, choose **Advanced Analytics**.</span></span>

     ![Visa information för en logikapp som körs och åtgärder](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)

     <span data-ttu-id="28009-168">Här hello Azure logganalys på sidan kan du uppdatera frågor och visa hello fås hello tabell.</span><span class="sxs-lookup"><span data-stu-id="28009-168">Here on hello Azure Log Analytics page, you can update queries and view hello results from hello table.</span></span> 
     <span data-ttu-id="28009-169">Den här frågan använder [Kusto frågespråk](https://docs.loganalytics.io/learn/tutorials/getting_started_with_queries.html), som du kan redigera om du vill tooview olika resultat.</span><span class="sxs-lookup"><span data-stu-id="28009-169">This query uses [Kusto query language](https://docs.loganalytics.io/learn/tutorials/getting_started_with_queries.html), which you can edit if you want tooview different results.</span></span> 

     ![Azure logganalys - frågevy](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a><span data-ttu-id="28009-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28009-171">Next steps</span></span>

* [<span data-ttu-id="28009-172">Övervaka B2B-meddelanden</span><span class="sxs-lookup"><span data-stu-id="28009-172">Monitor B2B messages</span></span>](../logic-apps/logic-apps-monitor-b2b-message.md)
