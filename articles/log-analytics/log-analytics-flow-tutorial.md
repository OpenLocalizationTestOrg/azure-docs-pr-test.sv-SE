---
title: aaaAutomate Azure logganalys bearbetar med Microsoft Flow
description: "Lär dig hur du kan använda Microsoft Flow tooquickly automatisera repeterbara processer med hello Azure logganalys-anslutningen."
services: log-analytics
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: log-analytics
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: bwren
ms.openlocfilehash: 52026df968682842cc9f8d55f6f9875c5f9c10b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-log-analytics-processes-with-hello-connector-for-microsoft-flow"></a><span data-ttu-id="e6bbd-103">Automatisera logganalys processer med hello connector för Microsoft Flow</span><span class="sxs-lookup"><span data-stu-id="e6bbd-103">Automate Log Analytics processes with hello connector for Microsoft Flow</span></span>
<span data-ttu-id="e6bbd-104">[Microsoft Flow](https://ms.flow.microsoft.com) kan du toocreate automatiserade arbetsflöden med hjälp av hundratals åtgärder för en mängd olika tjänster.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-104">[Microsoft Flow](https://ms.flow.microsoft.com) allows you toocreate automated workflows using hundreds of actions for a variety of services.</span></span> <span data-ttu-id="e6bbd-105">Utdata från en åtgärd kan användas som indata tooanother så att du toocreate integrering mellan olika tjänster.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-105">Output from one action can be used as input tooanother allowing you toocreate integration between different services.</span></span>  <span data-ttu-id="e6bbd-106">hello Azure logganalys connector för Microsoft Flow Tillåt toobuild arbetsflöden som innehåller data som hämtats av loggen söker i logganalys.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-106">hello Azure Log Analytics connector for Microsoft Flow allow you toobuild workflows that include data retrieved by log searches in Log Analytics.</span></span>

<span data-ttu-id="e6bbd-107">Till exempel använda Microsoft Flow toouse logganalys data i ett e-postmeddelande från Office 365, skapa ett programfel i Visual Studio Team Services eller skicka en Slack meddelande.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-107">For example, you can use Microsoft Flow toouse Log Analytics data in an email notification from Office 365, create a bug in Visual Studio Team Services, or post a Slack message.</span></span>  <span data-ttu-id="e6bbd-108">Du kan utlösa ett arbetsflöde med ett enkelt schema eller från en åtgärd i en ansluten tjänst, till exempel när ett e-postmeddelande eller en tweet tas emot.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-108">You can trigger a workflow by a simple schedule or from some action in a connected service such as when a mail or a tweet is received.</span></span>  


> [!NOTE]
> <span data-ttu-id="e6bbd-109">hello Azure logganalys connector för Microsoft Flow kräver din arbetsyta toobe uppgraderas toohello nya logganalys frågespråk.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-109">hello Azure Log Analytics connector for Microsoft Flow requires your workspace toobe upgraded toohello new Log Analytics query language.</span></span> <span data-ttu-id="e6bbd-110">Du lär dig mer om hello nytt språk och få hello proceduren tooupgrade ditt arbetsområde på [uppgradera sökningen Azure logganalys arbetsytan toonew loggen](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="e6bbd-110">You can learn more about hello new language and get hello procedure tooupgrade your workspace at [Upgrade your Azure Log Analytics workspace toonew log search](log-analytics-log-search-upgrade.md).</span></span>  

<span data-ttu-id="e6bbd-111">hello kursen i den här artikeln visar hur toocreate ett flöde som automatiskt skickar hello resultaten av en sökning för logganalys-loggen via e-post, bara ett exempel på hur du kan använda logganalys i Microsoft Flow.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-111">hello tutorial in this article shows you how toocreate a flow that automatically sends hello results of a Log Analytics log search by email, just one example of how you can use Log Analytics in Microsoft Flow.</span></span> 


## <a name="step-1-create-a-flow"></a><span data-ttu-id="e6bbd-112">Steg 1: Skapa ett flöde</span><span class="sxs-lookup"><span data-stu-id="e6bbd-112">Step 1: Create a flow</span></span>
1. <span data-ttu-id="e6bbd-113">Logga in för[Microsoft Flow](http://flow.microsoft.com), och välj **Mina flödar**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-113">Sign in too[Microsoft Flow](http://flow.microsoft.com), and select **My Flows**.</span></span>
2. <span data-ttu-id="e6bbd-114">Klicka på **+ skapa från tomt**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-114">Click **+ Create from blank**.</span></span>

## <a name="step-2-create-a-trigger-for-your-flow"></a><span data-ttu-id="e6bbd-115">Steg 2: Skapa en utlösare för din flöde</span><span class="sxs-lookup"><span data-stu-id="e6bbd-115">Step 2: Create a trigger for your flow</span></span>
1. <span data-ttu-id="e6bbd-116">Klicka på **Sök hundratals kopplingar och utlösare**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-116">Click **Search hundreds of connectors and triggers**.</span></span>
2. <span data-ttu-id="e6bbd-117">Typen **schema** i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-117">Type **Schedule** in hello search box.</span></span>
3. <span data-ttu-id="e6bbd-118">Välj **schema**, och välj sedan **schema - upprepning**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-118">Select **Schedule**, and then select **Schedule - Recurrence**.</span></span>
4. <span data-ttu-id="e6bbd-119">I hello **frekvens** markerar du kryssrutan **dag** och i hello **intervall** ange **1**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-119">In hello **Frequency** box select **Day** and in hello **Interval** box, enter **1**.</span></span><br><br><span data-ttu-id="e6bbd-120">![Dialogrutan för Microsoft Flow utlösare](media/log-analytics-flow-tutorial/flow01.png)</span><span class="sxs-lookup"><span data-stu-id="e6bbd-120">![Microsoft Flow trigger dialog box](media/log-analytics-flow-tutorial/flow01.png)</span></span>


## <a name="step-3-add-a-log-analytics-action"></a><span data-ttu-id="e6bbd-121">Steg 3: Lägg till en Log Analytics-åtgärd</span><span class="sxs-lookup"><span data-stu-id="e6bbd-121">Step 3: Add a Log Analytics action</span></span>
1. <span data-ttu-id="e6bbd-122">Klicka på **+ nytt steg**, och klicka sedan på **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-122">Click **+ New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="e6bbd-123">Sök efter **logga Analytics**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-123">Search for **Log Analytics**.</span></span>
3. <span data-ttu-id="e6bbd-124">Klicka på **Azure Log Analytics – Kör fråga och visualisera resultat**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-124">Click **Azure Log Analytics – Run query and visualize results**.</span></span><br><br><span data-ttu-id="e6bbd-125">![Kör frågefönstret logganalys](media/log-analytics-flow-tutorial/flow02.png)</span><span class="sxs-lookup"><span data-stu-id="e6bbd-125">![Log Analytics run query window](media/log-analytics-flow-tutorial/flow02.png)</span></span>

## <a name="step-4-configure-hello-log-analytics-action"></a><span data-ttu-id="e6bbd-126">Steg 4: Konfigurera hello logganalys åtgärd</span><span class="sxs-lookup"><span data-stu-id="e6bbd-126">Step 4: Configure hello Log Analytics action</span></span>

1. <span data-ttu-id="e6bbd-127">Ange hello information för din arbetsyta inklusive hello prenumerations-ID, resursgrupp, och arbetsytans namn.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-127">Specify hello details for your workspace including hello Subscription ID, Resource Group, and Workspace Name.</span></span>
2. <span data-ttu-id="e6bbd-128">Lägg till följande logganalys frågan toohello hello **frågan** fönster.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-128">Add hello following Log Analytics query toohello **Query** window.</span></span>  <span data-ttu-id="e6bbd-129">Detta är en exempelfråga och du kan ersätta med alla andra som returnerar data.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-129">This is only a sample query, and you can replace with any other that returns data.</span></span>
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computerindow. 
```

2. <span data-ttu-id="e6bbd-130">Välj **HTML-tabell** för hello **diagramtyp**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-130">Select **HTML Table** for hello **Chart Type**.</span></span><br><br><span data-ttu-id="e6bbd-131">![Log Analytics-åtgärd](media/log-analytics-flow-tutorial/flow03.png)</span><span class="sxs-lookup"><span data-stu-id="e6bbd-131">![Log Analytics action](media/log-analytics-flow-tutorial/flow03.png)</span></span>

## <a name="step-5-configure-hello-flow-toosend-email"></a><span data-ttu-id="e6bbd-132">Steg 5: Konfigurera hello flödet toosend e-post</span><span class="sxs-lookup"><span data-stu-id="e6bbd-132">Step 5: Configure hello flow toosend email</span></span>

1. <span data-ttu-id="e6bbd-133">Klicka på **nytt steg**, och klicka sedan på **+ Lägg till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-133">Click **New step**, and then click **+ Add an action**.</span></span>
2. <span data-ttu-id="e6bbd-134">Sök efter **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-134">Search for **Office 365 Outlook**.</span></span>
3. <span data-ttu-id="e6bbd-135">Klicka på **Office 365 Outlook – skicka ett e-post**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-135">Click **Office 365 Outlook – Send an email**.</span></span><br><br><span data-ttu-id="e6bbd-136">![Urvalsfönster för Office 365 Outlook](media/log-analytics-flow-tutorial/flow04.png)</span><span class="sxs-lookup"><span data-stu-id="e6bbd-136">![Office 365 Outlook selection window](media/log-analytics-flow-tutorial/flow04.png)</span></span>

4. <span data-ttu-id="e6bbd-137">Ange hello e-postadress för en mottagare i hello **till** fönster och ett ämne för e-post för hello i **ämne**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-137">Specify hello email address of a recipient in hello **To** window and a subject for hello email in **Subject**.</span></span>
5. <span data-ttu-id="e6bbd-138">Klicka någonstans i hello **brödtext** rutan.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-138">Click anywhere in hello **Body** box.</span></span>  <span data-ttu-id="e6bbd-139">En **dynamiskt innehåll** öppnas med värden från tidigare åtgärder.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-139">A **Dynamic content** window opens with values from previous actions.</span></span>  
6. <span data-ttu-id="e6bbd-140">Välj **brödtext**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-140">Select **Body**.</span></span>  <span data-ttu-id="e6bbd-141">Detta är hello resultaten av hello frågan i hello logganalys-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-141">This is hello results of hello query in hello Log Analytics action.</span></span>
6. <span data-ttu-id="e6bbd-142">Klicka på **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-142">Click **Show advanced options**.</span></span>
7. <span data-ttu-id="e6bbd-143">I hello **är HTML** väljer **Ja**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-143">In hello **Is HTML** box, select **Yes**.</span></span><br><br><span data-ttu-id="e6bbd-144">![Fönster för Office 365 e-konfiguration](media/log-analytics-flow-tutorial/flow05.png)</span><span class="sxs-lookup"><span data-stu-id="e6bbd-144">![Office 365 email configuration window](media/log-analytics-flow-tutorial/flow05.png)</span></span>

## <a name="step-6-save-and-test-your-flow"></a><span data-ttu-id="e6bbd-145">Steg 6: Spara och testa din flöde</span><span class="sxs-lookup"><span data-stu-id="e6bbd-145">Step 6: Save and test your flow</span></span>
1. <span data-ttu-id="e6bbd-146">I hello **flöde namnet** , lägga till ett namn för din flöde, och klicka sedan **skapa flödet**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-146">In hello **Flow name** box, add a name for your flow, and then click **Create flow**.</span></span><br><br><span data-ttu-id="e6bbd-147">![Spara flöde](media/log-analytics-flow-tutorial/flow06.png)</span><span class="sxs-lookup"><span data-stu-id="e6bbd-147">![Save flow](media/log-analytics-flow-tutorial/flow06.png)</span></span>
2. <span data-ttu-id="e6bbd-148">hello flödet skapas nu och kommer att köras efter en dag som är hello-schema som du angett.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-148">hello flow is now created and will run after a day which is hello schedule you specified.</span></span> 
3. <span data-ttu-id="e6bbd-149">tooimmediately test hello flödet klickar du på **kör nu** och sedan **kör flödet**.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-149">tooimmediately test hello flow, click **Run Now** and then **Run flow**.</span></span><br><br><span data-ttu-id="e6bbd-150">![Kör flöde](media/log-analytics-flow-tutorial/flow07.png)</span><span class="sxs-lookup"><span data-stu-id="e6bbd-150">![Run flow](media/log-analytics-flow-tutorial/flow07.png)</span></span>
3. <span data-ttu-id="e6bbd-151">Kontrollera hello e-post för hello mottagare som du angav när hello flödet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="e6bbd-151">When hello flow completes, check hello mail of hello recipient that you specified.</span></span>  <span data-ttu-id="e6bbd-152">Du bör ha fått ett e-postmeddelande med en brödtext liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="e6bbd-152">You should have received a mail with a body similar toohello following:</span></span><br><br>![E-postexempel](media/log-analytics-flow-tutorial/flow08.png)


## <a name="next-steps"></a><span data-ttu-id="e6bbd-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6bbd-154">Next steps</span></span>

- <span data-ttu-id="e6bbd-155">Lär dig mer om [logga sökningar i logganalys](log-analytics-log-search-new.md).</span><span class="sxs-lookup"><span data-stu-id="e6bbd-155">Learn more about [log searches in Log Analytics](log-analytics-log-search-new.md).</span></span>
- <span data-ttu-id="e6bbd-156">Lär dig mer om [Microsoft Flow](https://ms.flow.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="e6bbd-156">Learn more about [Microsoft Flow](https://ms.flow.microsoft.com).</span></span>



