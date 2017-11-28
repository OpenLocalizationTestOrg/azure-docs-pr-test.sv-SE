---
title: Automatisera Azure logganalys-processer med Microsoft Flow
description: "Lär dig hur du kan använda Microsoft Flow för att automatisera snabbt repeterbara processer med hjälp av Azure logganalys-anslutningen."
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
ms.openlocfilehash: 4955f90de06cd720d78c5bb60c0adcd7dc633245
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="automate-log-analytics-processes-with-the-connector-for-microsoft-flow"></a><span data-ttu-id="a42b5-103">Automatisera logganalys processer med connector för Microsoft Flow</span><span class="sxs-lookup"><span data-stu-id="a42b5-103">Automate Log Analytics processes with the connector for Microsoft Flow</span></span>
<span data-ttu-id="a42b5-104">[Microsoft Flow](https://ms.flow.microsoft.com) kan du skapa automatiska arbetsflöden med hundratals åtgärder för en mängd olika tjänster.</span><span class="sxs-lookup"><span data-stu-id="a42b5-104">[Microsoft Flow](https://ms.flow.microsoft.com) allows you to create automated workflows using hundreds of actions for a variety of services.</span></span> <span data-ttu-id="a42b5-105">Utdata från en åtgärd kan användas som indata till en annan så att du kan skapa integration mellan olika tjänster.</span><span class="sxs-lookup"><span data-stu-id="a42b5-105">Output from one action can be used as input to another allowing you to create integration between different services.</span></span>  <span data-ttu-id="a42b5-106">Azure logganalys-koppling för Microsoft Flow kan du skapa arbetsflöden som innehåller data som hämtats av loggen söker i logganalys.</span><span class="sxs-lookup"><span data-stu-id="a42b5-106">The Azure Log Analytics connector for Microsoft Flow allow you to build workflows that include data retrieved by log searches in Log Analytics.</span></span>

<span data-ttu-id="a42b5-107">Du kan till exempel använda Microsoft Flow för att använda Log Analytics-data i ett e-postmeddelande från Office 365, skapa ett programfel i Visual Studio Team Services eller skicka ett Slack meddelande.</span><span class="sxs-lookup"><span data-stu-id="a42b5-107">For example, you can use Microsoft Flow to use Log Analytics data in an email notification from Office 365, create a bug in Visual Studio Team Services, or post a Slack message.</span></span>  <span data-ttu-id="a42b5-108">Du kan utlösa ett arbetsflöde med ett enkelt schema eller från en åtgärd i en ansluten tjänst, till exempel när ett e-postmeddelande eller en tweet tas emot.</span><span class="sxs-lookup"><span data-stu-id="a42b5-108">You can trigger a workflow by a simple schedule or from some action in a connected service such as when a mail or a tweet is received.</span></span>  


> [!NOTE]
> <span data-ttu-id="a42b5-109">Azure logganalys-koppling för Microsoft Flow kräver ditt arbetsområde uppgraderas till det nya Log Analytics-frågespråket.</span><span class="sxs-lookup"><span data-stu-id="a42b5-109">The Azure Log Analytics connector for Microsoft Flow requires your workspace to be upgraded to the new Log Analytics query language.</span></span> <span data-ttu-id="a42b5-110">Du lär dig mer om det nya språket och få proceduren för att uppgradera din arbetsyta på [uppgradera Azure logganalys-arbetsytan till ny logg sökning](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="a42b5-110">You can learn more about the new language and get the procedure to upgrade your workspace at [Upgrade your Azure Log Analytics workspace to new log search](log-analytics-log-search-upgrade.md).</span></span>  

<span data-ttu-id="a42b5-111">Kursen i den här artikeln visar hur du skapar ett flöde som automatiskt skickar resultaten av en logganalys loggen via e-post, bara ett exempel på hur du kan använda logganalys i Microsoft Flow.</span><span class="sxs-lookup"><span data-stu-id="a42b5-111">The tutorial in this article shows you how to create a flow that automatically sends the results of a Log Analytics log search by email, just one example of how you can use Log Analytics in Microsoft Flow.</span></span> 


## <a name="step-1-create-a-flow"></a><span data-ttu-id="a42b5-112">Steg 1: Skapa ett flöde</span><span class="sxs-lookup"><span data-stu-id="a42b5-112">Step 1: Create a flow</span></span>
1. <span data-ttu-id="a42b5-113">Logga in på [Microsoft Flow](http://flow.microsoft.com), och välj **Mina flödar**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-113">Sign in to [Microsoft Flow](http://flow.microsoft.com), and select **My Flows**.</span></span>
2. <span data-ttu-id="a42b5-114">Klicka på **+ skapa från tomt**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-114">Click **+ Create from blank**.</span></span>

## <a name="step-2-create-a-trigger-for-your-flow"></a><span data-ttu-id="a42b5-115">Steg 2: Skapa en utlösare för din flöde</span><span class="sxs-lookup"><span data-stu-id="a42b5-115">Step 2: Create a trigger for your flow</span></span>
1. <span data-ttu-id="a42b5-116">Klicka på **Sök hundratals kopplingar och utlösare**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-116">Click **Search hundreds of connectors and triggers**.</span></span>
2. <span data-ttu-id="a42b5-117">Typen **schema** i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="a42b5-117">Type **Schedule** in the search box.</span></span>
3. <span data-ttu-id="a42b5-118">Välj **schema**, och välj sedan **schema - upprepning**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-118">Select **Schedule**, and then select **Schedule - Recurrence**.</span></span>
4. <span data-ttu-id="a42b5-119">I den **frekvens** markerar du kryssrutan **dag** och i den **intervall** ange **1**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-119">In the **Frequency** box select **Day** and in the **Interval** box, enter **1**.</span></span><br><br><span data-ttu-id="a42b5-120">![Dialogrutan för Microsoft Flow utlösare](media/log-analytics-flow-tutorial/flow01.png)</span><span class="sxs-lookup"><span data-stu-id="a42b5-120">![Microsoft Flow trigger dialog box](media/log-analytics-flow-tutorial/flow01.png)</span></span>


## <a name="step-3-add-a-log-analytics-action"></a><span data-ttu-id="a42b5-121">Steg 3: Lägg till en Log Analytics-åtgärd</span><span class="sxs-lookup"><span data-stu-id="a42b5-121">Step 3: Add a Log Analytics action</span></span>
1. <span data-ttu-id="a42b5-122">Klicka på **+ nytt steg**, och klicka sedan på **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-122">Click **+ New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="a42b5-123">Sök efter **logga Analytics**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-123">Search for **Log Analytics**.</span></span>
3. <span data-ttu-id="a42b5-124">Klicka på **Azure Log Analytics – Kör fråga och visualisera resultat**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-124">Click **Azure Log Analytics – Run query and visualize results**.</span></span><br><br><span data-ttu-id="a42b5-125">![Kör frågefönstret logganalys](media/log-analytics-flow-tutorial/flow02.png)</span><span class="sxs-lookup"><span data-stu-id="a42b5-125">![Log Analytics run query window](media/log-analytics-flow-tutorial/flow02.png)</span></span>

## <a name="step-4-configure-the-log-analytics-action"></a><span data-ttu-id="a42b5-126">Steg 4: Konfigurera logganalys-åtgärd</span><span class="sxs-lookup"><span data-stu-id="a42b5-126">Step 4: Configure the Log Analytics action</span></span>

1. <span data-ttu-id="a42b5-127">Ange information för din arbetsyta inklusive prenumerations-ID, resursgrupp, och arbetsytans namn.</span><span class="sxs-lookup"><span data-stu-id="a42b5-127">Specify the details for your workspace including the Subscription ID, Resource Group, and Workspace Name.</span></span>
2. <span data-ttu-id="a42b5-128">Lägg till följande Log Analytics-fråga för att den **frågan** fönster.</span><span class="sxs-lookup"><span data-stu-id="a42b5-128">Add the following Log Analytics query to the **Query** window.</span></span>  <span data-ttu-id="a42b5-129">Detta är en exempelfråga och du kan ersätta med alla andra som returnerar data.</span><span class="sxs-lookup"><span data-stu-id="a42b5-129">This is only a sample query, and you can replace with any other that returns data.</span></span>
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computerindow. 
```

2. <span data-ttu-id="a42b5-130">Välj **HTML-tabell** för den **diagramtypen**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-130">Select **HTML Table** for the **Chart Type**.</span></span><br><br><span data-ttu-id="a42b5-131">![Log Analytics-åtgärd](media/log-analytics-flow-tutorial/flow03.png)</span><span class="sxs-lookup"><span data-stu-id="a42b5-131">![Log Analytics action](media/log-analytics-flow-tutorial/flow03.png)</span></span>

## <a name="step-5-configure-the-flow-to-send-email"></a><span data-ttu-id="a42b5-132">Steg 5: Konfigurera flödet för att skicka e-post</span><span class="sxs-lookup"><span data-stu-id="a42b5-132">Step 5: Configure the flow to send email</span></span>

1. <span data-ttu-id="a42b5-133">Klicka på **nytt steg**, och klicka sedan på **+ Lägg till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-133">Click **New step**, and then click **+ Add an action**.</span></span>
2. <span data-ttu-id="a42b5-134">Sök efter **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-134">Search for **Office 365 Outlook**.</span></span>
3. <span data-ttu-id="a42b5-135">Klicka på **Office 365 Outlook – skicka ett e-post**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-135">Click **Office 365 Outlook – Send an email**.</span></span><br><br><span data-ttu-id="a42b5-136">![Urvalsfönster för Office 365 Outlook](media/log-analytics-flow-tutorial/flow04.png)</span><span class="sxs-lookup"><span data-stu-id="a42b5-136">![Office 365 Outlook selection window](media/log-analytics-flow-tutorial/flow04.png)</span></span>

4. <span data-ttu-id="a42b5-137">Ange e-postadress för en mottagare i den **till** fönster och ett ämne för e-post i **ämne**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-137">Specify the email address of a recipient in the **To** window and a subject for the email in **Subject**.</span></span>
5. <span data-ttu-id="a42b5-138">Klicka någonstans i den **brödtext** rutan.</span><span class="sxs-lookup"><span data-stu-id="a42b5-138">Click anywhere in the **Body** box.</span></span>  <span data-ttu-id="a42b5-139">En **dynamiskt innehåll** öppnas med värden från tidigare åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a42b5-139">A **Dynamic content** window opens with values from previous actions.</span></span>  
6. <span data-ttu-id="a42b5-140">Välj **brödtext**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-140">Select **Body**.</span></span>  <span data-ttu-id="a42b5-141">Detta är resultatet av frågan i logganalys-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a42b5-141">This is the results of the query in the Log Analytics action.</span></span>
6. <span data-ttu-id="a42b5-142">Klicka på **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-142">Click **Show advanced options**.</span></span>
7. <span data-ttu-id="a42b5-143">I den **är HTML** väljer **Ja**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-143">In the **Is HTML** box, select **Yes**.</span></span><br><br><span data-ttu-id="a42b5-144">![Fönster för Office 365 e-konfiguration](media/log-analytics-flow-tutorial/flow05.png)</span><span class="sxs-lookup"><span data-stu-id="a42b5-144">![Office 365 email configuration window](media/log-analytics-flow-tutorial/flow05.png)</span></span>

## <a name="step-6-save-and-test-your-flow"></a><span data-ttu-id="a42b5-145">Steg 6: Spara och testa din flöde</span><span class="sxs-lookup"><span data-stu-id="a42b5-145">Step 6: Save and test your flow</span></span>
1. <span data-ttu-id="a42b5-146">I den **flöde namnet** , lägga till ett namn för din flöde, och klicka sedan **skapa flödet**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-146">In the **Flow name** box, add a name for your flow, and then click **Create flow**.</span></span><br><br><span data-ttu-id="a42b5-147">![Spara flöde](media/log-analytics-flow-tutorial/flow06.png)</span><span class="sxs-lookup"><span data-stu-id="a42b5-147">![Save flow](media/log-analytics-flow-tutorial/flow06.png)</span></span>
2. <span data-ttu-id="a42b5-148">Flödet skapas nu och kommer att köras efter en dag som är det schema som du har angett.</span><span class="sxs-lookup"><span data-stu-id="a42b5-148">The flow is now created and will run after a day which is the schedule you specified.</span></span> 
3. <span data-ttu-id="a42b5-149">Om du vill testa omedelbart flödet, klickar du på **kör nu** och sedan **kör flödet**.</span><span class="sxs-lookup"><span data-stu-id="a42b5-149">To immediately test the flow, click **Run Now** and then **Run flow**.</span></span><br><br><span data-ttu-id="a42b5-150">![Kör flöde](media/log-analytics-flow-tutorial/flow07.png)</span><span class="sxs-lookup"><span data-stu-id="a42b5-150">![Run flow](media/log-analytics-flow-tutorial/flow07.png)</span></span>
3. <span data-ttu-id="a42b5-151">Kontrollera e-post för mottagare som du angav när flödet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a42b5-151">When the flow completes, check the mail of the recipient that you specified.</span></span>  <span data-ttu-id="a42b5-152">Du bör ha fått ett e-postmeddelande med en text som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="a42b5-152">You should have received a mail with a body similar to the following:</span></span><br><br>![E-postexempel](media/log-analytics-flow-tutorial/flow08.png)


## <a name="next-steps"></a><span data-ttu-id="a42b5-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a42b5-154">Next steps</span></span>

- <span data-ttu-id="a42b5-155">Lär dig mer om [logga sökningar i logganalys](log-analytics-log-search-new.md).</span><span class="sxs-lookup"><span data-stu-id="a42b5-155">Learn more about [log searches in Log Analytics](log-analytics-log-search-new.md).</span></span>
- <span data-ttu-id="a42b5-156">Lär dig mer om [Microsoft Flow](https://ms.flow.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="a42b5-156">Learn more about [Microsoft Flow](https://ms.flow.microsoft.com).</span></span>



