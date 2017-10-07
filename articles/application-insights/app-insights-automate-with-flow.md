---
title: aaaAutomate Azure Application Insights bearbetar med Microsoft Flow
description: "Lär dig hur du kan använda Microsoft Flow tooquickly automatisera repeterbara processer med hjälp av hello Application Insights connector."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: bwren
ms.openlocfilehash: b34488a6b8b8b0a6add960a67f1426cbbbc13552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-azure-application-insights-processes-with-hello-connector-for-microsoft-flow"></a><span data-ttu-id="e0488-103">Automatisera Azure Application Insights-processer med hello connector för Microsoft Flow</span><span class="sxs-lookup"><span data-stu-id="e0488-103">Automate Azure Application Insights processes with hello connector for Microsoft Flow</span></span>

<span data-ttu-id="e0488-104">Hittar du själv körs flera gånger hello samma frågor på dina telemetri data toocheck som din tjänst fungerar korrekt?</span><span class="sxs-lookup"><span data-stu-id="e0488-104">Do you find yourself repeatedly running hello same queries on your telemetry data toocheck that your service is functioning properly?</span></span> <span data-ttu-id="e0488-105">Är du söker tooautomate dessa frågor för att hitta trender och avvikelser och skapa egna arbetsflöden runt dem? hello Azure Application Insights connector (förhandsversion) för Microsoft Flow är hello rätt verktyg för dessa ändamål.</span><span class="sxs-lookup"><span data-stu-id="e0488-105">Are you looking tooautomate these queries for finding trends and anomalies and then build your own workflows around them? hello Azure Application Insights connector (preview) for Microsoft Flow is hello right tool for these purposes.</span></span>

<span data-ttu-id="e0488-106">Med den här integreringen kan du automatisera många processer nu utan att skriva en enda rad kod.</span><span class="sxs-lookup"><span data-stu-id="e0488-106">With this integration, you can now automate numerous processes without writing a single line of code.</span></span> <span data-ttu-id="e0488-107">När du har skapat ett flöde med hjälp av en Application Insights-åtgärden körs hello flödet automatiskt Application Insights Analytics-fråga.</span><span class="sxs-lookup"><span data-stu-id="e0488-107">After you create a flow by using an Application Insights action, hello flow automatically runs your Application Insights Analytics query.</span></span> 

<span data-ttu-id="e0488-108">Du kan lägga till ytterligare åtgärder samt.</span><span class="sxs-lookup"><span data-stu-id="e0488-108">You can add additional actions as well.</span></span> <span data-ttu-id="e0488-109">Microsoft Flow tillgängliggör hundratals åtgärder.</span><span class="sxs-lookup"><span data-stu-id="e0488-109">Microsoft Flow makes hundreds of actions available.</span></span> <span data-ttu-id="e0488-110">Du kan till exempel använda Microsoft Flow tooautomatically skicka ett e-postmeddelande eller skapa ett programfel i Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="e0488-110">For example, you can use Microsoft Flow tooautomatically send an email notification or create a bug in Visual Studio Team Services.</span></span> <span data-ttu-id="e0488-111">Du kan också använda en av hello många [mallar](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) som är tillgängliga för hello connector för Microsoft Flow.</span><span class="sxs-lookup"><span data-stu-id="e0488-111">You can also use one of hello many [templates](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) that are available for hello connector for Microsoft Flow.</span></span> <span data-ttu-id="e0488-112">Dessa mallar snabbare hello processen att skapa ett flöde.</span><span class="sxs-lookup"><span data-stu-id="e0488-112">These templates speed up hello process of creating a flow.</span></span> 

<!--hello Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a><span data-ttu-id="e0488-113">Skapa ett flöde för Application Insights</span><span class="sxs-lookup"><span data-stu-id="e0488-113">Create a flow for Application Insights</span></span>

<span data-ttu-id="e0488-114">I kursen får du lära dig hur toocreate ett flöde som använder hello Analytics automatiskt klustret algoritmen toogroup attribut i hello data för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="e0488-114">In this tutorial, you will learn how toocreate a flow that uses hello Analytics auto-cluster algorithm toogroup attributes in hello data for a web application.</span></span> <span data-ttu-id="e0488-115">hello flödet skickar automatiskt hello resultaten via e-post, bara ett exempel på hur du kan använda Microsoft Flow och Application Insights Analytics tillsammans.</span><span class="sxs-lookup"><span data-stu-id="e0488-115">hello flow automatically sends hello results by email, just one example of how you can use Microsoft Flow and Application Insights Analytics together.</span></span> 

### <a name="step-1-create-a-flow"></a><span data-ttu-id="e0488-116">Steg 1: Skapa ett flöde</span><span class="sxs-lookup"><span data-stu-id="e0488-116">Step 1: Create a flow</span></span>
1. <span data-ttu-id="e0488-117">Logga in för[Microsoft Flow](http://flow.microsoft.com), och välj sedan **Mina flödar**.</span><span class="sxs-lookup"><span data-stu-id="e0488-117">Sign in too[Microsoft Flow](http://flow.microsoft.com), and then select **My Flows**.</span></span>
2. <span data-ttu-id="e0488-118">Klicka på **skapa ett flöde från tomt**.</span><span class="sxs-lookup"><span data-stu-id="e0488-118">Click **Create a flow from blank**.</span></span>

### <a name="step-2-create-a-trigger-for-your-flow"></a><span data-ttu-id="e0488-119">Steg 2: Skapa en utlösare för din flöde</span><span class="sxs-lookup"><span data-stu-id="e0488-119">Step 2: Create a trigger for your flow</span></span>
1. <span data-ttu-id="e0488-120">Välj **schema**, och välj sedan **schema - upprepning**.</span><span class="sxs-lookup"><span data-stu-id="e0488-120">Select **Schedule**, and then select **Schedule - Recurrence**.</span></span>
2. <span data-ttu-id="e0488-121">I hello **frekvens** väljer **dag**, och i hello **intervall** ange **1**.</span><span class="sxs-lookup"><span data-stu-id="e0488-121">In hello **Frequency** box, select **Day**, and in hello **Interval** box, enter **1**.</span></span>

    ![Dialogrutan för Microsoft Flow utlösare](./media/app-insights-automate-with-flow/flow1.png)


### <a name="step-3-add-an-application-insights-action"></a><span data-ttu-id="e0488-123">Steg 3: Lägg till en Application Insights-åtgärd</span><span class="sxs-lookup"><span data-stu-id="e0488-123">Step 3: Add an Application Insights action</span></span>
1. <span data-ttu-id="e0488-124">Klicka på **nytt steg**, och klicka sedan på **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="e0488-124">Click **New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="e0488-125">Sök efter **Azure Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="e0488-125">Search for **Azure Application Insights**.</span></span>
3. <span data-ttu-id="e0488-126">Klicka på **Azure Application Insights – visualisera Analytics fråga Preview**.</span><span class="sxs-lookup"><span data-stu-id="e0488-126">Click **Azure Application Insights – Visualize Analytics query Preview**.</span></span>

    ![Kör Analytics query-fönstret](./media/app-insights-automate-with-flow/flow2.png)

### <a name="step-4-connect-tooan-application-insights-resource"></a><span data-ttu-id="e0488-128">Steg 4: Anslut tooan Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="e0488-128">Step 4: Connect tooan Application Insights resource</span></span>

<span data-ttu-id="e0488-129">toocomplete det här steget behöver du ett program-ID och en API-nyckel för din resurs.</span><span class="sxs-lookup"><span data-stu-id="e0488-129">toocomplete this step, you need an application ID and an API key for your resource.</span></span> <span data-ttu-id="e0488-130">Du kan hämta dem från hello Azure-portalen som visas i följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="e0488-130">You can retrieve them from hello Azure portal, as shown in hello following diagram:</span></span>

![Program-ID i hello Azure-portalen](./media/app-insights-automate-with-flow/appid.png) 

- <span data-ttu-id="e0488-132">Ange ett namn för anslutningen, tillsammans med hello program-ID och API-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="e0488-132">Provide a name for your connection, along with hello application ID and API key.</span></span>

    ![Fönstret för Microsoft Flow-anslutning](./media/app-insights-automate-with-flow/flow3.png)

### <a name="step-5-specify-hello-analytics-query-and-chart-type"></a><span data-ttu-id="e0488-134">Steg 5: Ange hello Analytics-fråga och -diagram</span><span class="sxs-lookup"><span data-stu-id="e0488-134">Step 5: Specify hello Analytics query and chart type</span></span>
<span data-ttu-id="e0488-135">Den här exempelfråga väljer hello misslyckades-förfrågningar inom hello sista dagen och korrelerar dem med undantag som uppstått som en del av hello igen.</span><span class="sxs-lookup"><span data-stu-id="e0488-135">This example query selects hello failed requests within hello last day and correlates them with exceptions that occurred as part of hello operation.</span></span> <span data-ttu-id="e0488-136">Analytics korrelerar dem baserat på hello operation_Id identifierare.</span><span class="sxs-lookup"><span data-stu-id="e0488-136">Analytics correlates them based on hello operation_Id identifier.</span></span> <span data-ttu-id="e0488-137">hello frågan segment sedan hello resultat med hjälp av hello autocluster algoritm.</span><span class="sxs-lookup"><span data-stu-id="e0488-137">hello query then segments hello results by using hello autocluster algorithm.</span></span> 

<span data-ttu-id="e0488-138">När du skapar egna frågor kan du kontrollera att de fungerar korrekt i Analytics innan du lägger till den tooyour flöde.</span><span class="sxs-lookup"><span data-stu-id="e0488-138">When you create your own queries, verify that they are working properly in Analytics before you add it tooyour flow.</span></span>

- <span data-ttu-id="e0488-139">Lägg till hello Analytics-fråga efter och välj sedan diagramtyp för hello HTML-tabellen.</span><span class="sxs-lookup"><span data-stu-id="e0488-139">Add hello following Analytics query, and then select hello HTML table chart type.</span></span> 

    ```
    requests
    | where timestamp > ago(1d)
    | where success == "False"
    | project name, operation_Id
    | join ( exceptions
        | project problemId, outerMessage, operation_Id
    ) on operation_Id
    | evaluate autocluster()
    ```
    
    ![Analytics frågefönstret konfiguration](./media/app-insights-automate-with-flow/flow4.png)

### <a name="step-6-configure-hello-flow-toosend-email"></a><span data-ttu-id="e0488-141">Steg 6: Konfigurera hello flödet toosend e-post</span><span class="sxs-lookup"><span data-stu-id="e0488-141">Step 6: Configure hello flow toosend email</span></span>

1. <span data-ttu-id="e0488-142">Klicka på **nytt steg**, och klicka sedan på **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="e0488-142">Click **New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="e0488-143">Sök efter **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="e0488-143">Search for **Office 365 Outlook**.</span></span>
3. <span data-ttu-id="e0488-144">Klicka på **Office 365 Outlook – skicka ett e-post**.</span><span class="sxs-lookup"><span data-stu-id="e0488-144">Click **Office 365 Outlook – Send an email**.</span></span>

    ![Urvalsfönster för Office 365 Outlook](./media/app-insights-automate-with-flow/flow2b.png)

4. <span data-ttu-id="e0488-146">I hello **skickar ett e-** fönstret hello följande:</span><span class="sxs-lookup"><span data-stu-id="e0488-146">In hello **Send an email** window, do hello following:</span></span>

   <span data-ttu-id="e0488-147">a.</span><span class="sxs-lookup"><span data-stu-id="e0488-147">a.</span></span> <span data-ttu-id="e0488-148">Ange hello e-postadressen till mottagaren hello.</span><span class="sxs-lookup"><span data-stu-id="e0488-148">Type hello email address of hello recipient.</span></span>

   <span data-ttu-id="e0488-149">b.</span><span class="sxs-lookup"><span data-stu-id="e0488-149">b.</span></span> <span data-ttu-id="e0488-150">Skriv ett ämne för e-post hello.</span><span class="sxs-lookup"><span data-stu-id="e0488-150">Type a subject for hello email.</span></span>

   <span data-ttu-id="e0488-151">c.</span><span class="sxs-lookup"><span data-stu-id="e0488-151">c.</span></span> <span data-ttu-id="e0488-152">Klicka någonstans i hello **brödtext** rutan och välj sedan på hello dynamiskt innehåll-menyn som öppnas på höger hello **brödtext**.</span><span class="sxs-lookup"><span data-stu-id="e0488-152">Click anywhere in hello **Body** box and then, on hello dynamic content menu that opens at hello right, select **Body**.</span></span>

   <span data-ttu-id="e0488-153">d.</span><span class="sxs-lookup"><span data-stu-id="e0488-153">d.</span></span> <span data-ttu-id="e0488-154">Klicka på **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="e0488-154">Click **Show advanced options**.</span></span>

    ![Outlook-konfiguration för Office 365](./media/app-insights-automate-with-flow/flow5.png)

5. <span data-ttu-id="e0488-156">Hello dynamiskt innehåll menyn hello följande:</span><span class="sxs-lookup"><span data-stu-id="e0488-156">On hello dynamic content menu, do hello following:</span></span>

    <span data-ttu-id="e0488-157">a.</span><span class="sxs-lookup"><span data-stu-id="e0488-157">a.</span></span> <span data-ttu-id="e0488-158">Välj **Bilagenamn**.</span><span class="sxs-lookup"><span data-stu-id="e0488-158">Select **Attachment Name**.</span></span>

    <span data-ttu-id="e0488-159">b.</span><span class="sxs-lookup"><span data-stu-id="e0488-159">b.</span></span> <span data-ttu-id="e0488-160">Välj **bifogade filer**.</span><span class="sxs-lookup"><span data-stu-id="e0488-160">Select **Attachment Content**.</span></span>
    
    <span data-ttu-id="e0488-161">c.</span><span class="sxs-lookup"><span data-stu-id="e0488-161">c.</span></span> <span data-ttu-id="e0488-162">I hello **är HTML** väljer **Ja**.</span><span class="sxs-lookup"><span data-stu-id="e0488-162">In hello **Is HTML** box, select **Yes**.</span></span>

    ![Fönster för Office 365 e-konfiguration](./media/app-insights-automate-with-flow/flow7.png)

### <a name="step-7-save-and-test-your-flow"></a><span data-ttu-id="e0488-164">Steg 7: Spara och testa din flöde</span><span class="sxs-lookup"><span data-stu-id="e0488-164">Step 7: Save and test your flow</span></span>
- <span data-ttu-id="e0488-165">I hello **flöde namnet** , lägga till ett namn för din flöde, och klicka sedan **skapa flödet**.</span><span class="sxs-lookup"><span data-stu-id="e0488-165">In hello **Flow name** box, add a name for your flow, and then click **Create flow**.</span></span>

    ![Skapande av flödet fönster](./media/app-insights-automate-with-flow/flow8.png)

<span data-ttu-id="e0488-167">Du kan vänta på hello utlösaren toorun åtgärden eller köra hello flödet direkt av [körs hello utlösare på begäran](https://flow.microsoft.com/blog/run-now-and-six-more-services/).</span><span class="sxs-lookup"><span data-stu-id="e0488-167">You can wait for hello trigger toorun this action, or you can run hello flow immediately by [running hello trigger on demand](https://flow.microsoft.com/blog/run-now-and-six-more-services/).</span></span>

<span data-ttu-id="e0488-168">När hello flödet körs får hello mottagare som du har angett i hello e-postlistan ett e-postmeddelande som ser ut som följande hello:</span><span class="sxs-lookup"><span data-stu-id="e0488-168">When hello flow runs, hello recipients you have specified in hello email list receive an email message that looks like hello following:</span></span>

![E-postexempel](./media/app-insights-automate-with-flow/flow9.png)


## <a name="next-steps"></a><span data-ttu-id="e0488-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0488-170">Next steps</span></span>

- <span data-ttu-id="e0488-171">Lär dig mer om hur du skapar [Analytics-frågor](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="e0488-171">Learn more about creating [Analytics queries](app-insights-analytics-using.md).</span></span>
- <span data-ttu-id="e0488-172">Lär dig mer om [Microsoft Flow](https://ms.flow.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="e0488-172">Learn more about [Microsoft Flow](https://ms.flow.microsoft.com).</span></span>



<!--Link references-->





