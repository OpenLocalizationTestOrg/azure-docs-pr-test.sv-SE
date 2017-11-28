---
title: "aaaAutomate Azure Application Insights bearbetar med hjälp av Logic Apps."
description: "Lär dig hur du snabbt kan automatisera repeterbara processer genom att lägga till hello Application Insights connector tooyour logikapp."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: bwren
ms.openlocfilehash: 8565aadf0644ffb10da8a0964821901907db2e27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a><span data-ttu-id="882ac-103">Automatisera processer för Application Insights med hjälp av Logic Apps</span><span class="sxs-lookup"><span data-stu-id="882ac-103">Automate Application Insights processes by using Logic Apps</span></span>

<span data-ttu-id="882ac-104">Hittar du själv upprepade gånger körs hello samma frågor på dina telemetri data toocheck om tjänsten fungerar korrekt?</span><span class="sxs-lookup"><span data-stu-id="882ac-104">Do you find yourself repeatedly running hello same queries on your telemetry data toocheck whether your service is functioning properly?</span></span> <span data-ttu-id="882ac-105">Är du söker tooautomate dessa frågor för att hitta trender och avvikelser och skapa egna arbetsflöden runt dem? hello Azure Application Insights connector (förhandsversion) för Logic Apps är hello rätt verktyg för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="882ac-105">Are you looking tooautomate these queries for finding trends and anomalies and then build your own workflows around them? hello Azure Application Insights connector (preview) for Logic Apps is hello right tool for this purpose.</span></span>

<span data-ttu-id="882ac-106">Med den här integreringen kan du automatisera många processer utan att skriva en enda rad kod.</span><span class="sxs-lookup"><span data-stu-id="882ac-106">With this integration, you can automate numerous processes without writing a single line of code.</span></span> <span data-ttu-id="882ac-107">Du kan skapa en logikapp med hello Application Insights connector tooquickly automatisera processer Application Insights.</span><span class="sxs-lookup"><span data-stu-id="882ac-107">You can create a logic app with hello Application Insights connector tooquickly automate any Application Insights process.</span></span> 

<span data-ttu-id="882ac-108">Du kan lägga till ytterligare åtgärder samt.</span><span class="sxs-lookup"><span data-stu-id="882ac-108">You can add additional actions as well.</span></span> <span data-ttu-id="882ac-109">hello Logic Apps-funktionen i Azure Apptjänst tillgängliggör hundratals åtgärder.</span><span class="sxs-lookup"><span data-stu-id="882ac-109">hello Logic Apps feature of Azure App Service makes hundreds of actions available.</span></span> <span data-ttu-id="882ac-110">Till exempel med hjälp av en logikapp, kan du automatiskt skicka ett e-postmeddelande eller skapa ett programfel i Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="882ac-110">For example, by using a logic app, you can automatically send an email notification or create a bug in Visual Studio Team Services.</span></span> <span data-ttu-id="882ac-111">Du kan också använda en av hello många tillgängliga [mallar](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) toohelp påskynda hello processen att skapa din logikapp.</span><span class="sxs-lookup"><span data-stu-id="882ac-111">You can also use one of hello many available [templates](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) toohelp speed up hello process of creating your logic app.</span></span> 

## <a name="create-a-logic-app-for-application-insights"></a><span data-ttu-id="882ac-112">Skapa en logikapp för Application Insights</span><span class="sxs-lookup"><span data-stu-id="882ac-112">Create a logic app for Application Insights</span></span>

<span data-ttu-id="882ac-113">I kursen får du lära dig hur toocreate en logikapp som använder hello Analytics autocluster algoritmen toogroup attribut i hello data för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="882ac-113">In this tutorial, you learn how toocreate a logic app that uses hello Analytics autocluster algorithm toogroup attributes in hello data for a web application.</span></span> <span data-ttu-id="882ac-114">hello flödet skickar automatiskt hello resultaten via e-post, bara ett exempel på hur du kan använda Application Insights Analytics och Logic Apps tillsammans.</span><span class="sxs-lookup"><span data-stu-id="882ac-114">hello flow automatically sends hello results by email, just one example of how you can use Application Insights Analytics and Logic Apps together.</span></span> 

### <a name="step-1-create-a-logic-app"></a><span data-ttu-id="882ac-115">Steg 1: Skapa en logikapp</span><span class="sxs-lookup"><span data-stu-id="882ac-115">Step 1: Create a logic app</span></span>
1. <span data-ttu-id="882ac-116">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="882ac-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="882ac-117">I hello **ny** väljer **webb + mobilt**, och välj sedan **Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="882ac-117">In hello **New** pane, select **Web + Mobile**, and then select **Logic App**.</span></span>

    ![Nytt logik app fönster](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a><span data-ttu-id="882ac-119">Steg 2: Skapa en utlösare för din logikapp</span><span class="sxs-lookup"><span data-stu-id="882ac-119">Step 2: Create a trigger for your logic app</span></span>
1. <span data-ttu-id="882ac-120">I hello **logik App Designer** fönstret under **börja med en gemensam utlösare**väljer **återkommande**.</span><span class="sxs-lookup"><span data-stu-id="882ac-120">In hello **Logic App Designer** window, under **Start with a common trigger**, select **Recurrence**.</span></span>

    ![Logik App Designer fönster](./media/automate-with-logic-apps/logicapp2.png)

2. <span data-ttu-id="882ac-122">I hello **frekvens** väljer **dag** och sedan, i hello **intervall** skriver **1**.</span><span class="sxs-lookup"><span data-stu-id="882ac-122">In hello **Frequency** box, select **Day** and then, in hello **Interval** box, type **1**.</span></span>

    ![Logik App Designer ”återkommande” fönster](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a><span data-ttu-id="882ac-124">Steg 3: Lägg till en Application Insights-åtgärd</span><span class="sxs-lookup"><span data-stu-id="882ac-124">Step 3: Add an Application Insights action</span></span>
1. <span data-ttu-id="882ac-125">Klicka på **nytt steg**, och klicka sedan på **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="882ac-125">Click **New step**, and then click **Add an action**.</span></span>

2. <span data-ttu-id="882ac-126">I hello **välja en åtgärd** sökrutan, Skriv **Azure Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="882ac-126">In hello **Choose an action** search box, type **Azure Application Insights**.</span></span>

3. <span data-ttu-id="882ac-127">Under **åtgärder**, klickar du på **Azure Application Insights – visualisera Analytics fråga Preview**.</span><span class="sxs-lookup"><span data-stu-id="882ac-127">Under **Actions**, click **Azure Application Insights – Visualize Analytics query Preview**.</span></span>

    ![Logik App Designer ”Välj åtgärden” fönster](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-tooan-application-insights-resource"></a><span data-ttu-id="882ac-129">Steg 4: Anslut tooan Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="882ac-129">Step 4: Connect tooan Application Insights resource</span></span>

<span data-ttu-id="882ac-130">toocomplete det här steget behöver du ett program-ID och en API-nyckel för din resurs.</span><span class="sxs-lookup"><span data-stu-id="882ac-130">toocomplete this step, you need an application ID and an API key for your resource.</span></span> <span data-ttu-id="882ac-131">Du kan hämta dem från hello Azure-portalen som visas i följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="882ac-131">You can retrieve them from hello Azure portal, as shown in hello following diagram:</span></span>

![Program-ID i hello Azure-portalen](./media/automate-with-logic-apps/appid.png) 

<span data-ttu-id="882ac-133">Ange ett namn för anslutningen, hello program-ID och hello API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="882ac-133">Provide a name for your connection, hello application ID, and hello API key.</span></span>

![Fönstret för logik App Designer flödet anslutning](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-hello-analytics-query-and-chart-type"></a><span data-ttu-id="882ac-135">Steg 5: Ange hello Analytics-fråga och -diagram</span><span class="sxs-lookup"><span data-stu-id="882ac-135">Step 5: Specify hello Analytics query and chart type</span></span>
<span data-ttu-id="882ac-136">I följande exempel hello, hello frågan väljer hello misslyckades-förfrågningar inom hello sista dagen och korrelerar dem med undantag som uppstått som en del av hello igen.</span><span class="sxs-lookup"><span data-stu-id="882ac-136">In hello following example, hello query selects hello failed requests within hello last day and correlates them with exceptions that occurred as part of hello operation.</span></span> <span data-ttu-id="882ac-137">Analytics korrelerar hello misslyckades-förfrågningar, baserat på hello operation_Id identifierare.</span><span class="sxs-lookup"><span data-stu-id="882ac-137">Analytics correlates hello failed requests, based on hello operation_Id identifier.</span></span> <span data-ttu-id="882ac-138">hello frågan segment sedan hello resultat med hjälp av hello autocluster algoritm.</span><span class="sxs-lookup"><span data-stu-id="882ac-138">hello query then segments hello results by using hello autocluster algorithm.</span></span> 

<span data-ttu-id="882ac-139">När du skapar egna frågor kan du kontrollera att de fungerar korrekt i Analytics innan du lägger till den tooyour flöde.</span><span class="sxs-lookup"><span data-stu-id="882ac-139">When you create your own queries, verify that they are working properly in Analytics before you add it tooyour flow.</span></span>

1. <span data-ttu-id="882ac-140">I hello **frågan** lägger du till hello följande Analytics-fråga:</span><span class="sxs-lookup"><span data-stu-id="882ac-140">In hello **Query** box, add hello following Analytics query:</span></span> 

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

2. <span data-ttu-id="882ac-141">I hello **diagramtyp** väljer **HTML-tabell**.</span><span class="sxs-lookup"><span data-stu-id="882ac-141">In hello **Chart Type** box, select **Html Table**.</span></span>

    ![Analytics frågefönstret konfiguration](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-hello-logic-app-toosend-email"></a><span data-ttu-id="882ac-143">Steg 6: Konfigurera hello logik app toosend e-post</span><span class="sxs-lookup"><span data-stu-id="882ac-143">Step 6: Configure hello logic app toosend email</span></span>

1. <span data-ttu-id="882ac-144">Klicka på **nytt steg**, och välj sedan **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="882ac-144">Click **New step**, and then select **Add an action**.</span></span>

2. <span data-ttu-id="882ac-145">Skriv i sökrutan hello **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="882ac-145">In hello search box, type **Office 365 Outlook**.</span></span>

3. <span data-ttu-id="882ac-146">Klicka på **Office 365 Outlook – skicka ett e-post**.</span><span class="sxs-lookup"><span data-stu-id="882ac-146">Click **Office 365 Outlook – Send an email**.</span></span>

    ![Val av Office 365 Outlook](./media/automate-with-logic-apps/flow2b.png)

4. <span data-ttu-id="882ac-148">I hello **skickar ett e-** fönstret hello följande:</span><span class="sxs-lookup"><span data-stu-id="882ac-148">In hello **Send an email** window, do hello following:</span></span>

   <span data-ttu-id="882ac-149">a.</span><span class="sxs-lookup"><span data-stu-id="882ac-149">a.</span></span> <span data-ttu-id="882ac-150">Ange hello e-postadressen till mottagaren hello.</span><span class="sxs-lookup"><span data-stu-id="882ac-150">Type hello email address of hello recipient.</span></span>

   <span data-ttu-id="882ac-151">b.</span><span class="sxs-lookup"><span data-stu-id="882ac-151">b.</span></span> <span data-ttu-id="882ac-152">Skriv ett ämne för e-post hello.</span><span class="sxs-lookup"><span data-stu-id="882ac-152">Type a subject for hello email.</span></span>

   <span data-ttu-id="882ac-153">c.</span><span class="sxs-lookup"><span data-stu-id="882ac-153">c.</span></span> <span data-ttu-id="882ac-154">Klicka någonstans i hello **brödtext** rutan och välj sedan på hello dynamiskt innehåll-menyn som öppnas på höger hello **brödtext**.</span><span class="sxs-lookup"><span data-stu-id="882ac-154">Click anywhere in hello **Body** box and then, on hello dynamic content menu that opens at hello right, select **Body**.</span></span>

   <span data-ttu-id="882ac-155">d.</span><span class="sxs-lookup"><span data-stu-id="882ac-155">d.</span></span> <span data-ttu-id="882ac-156">Klicka på **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="882ac-156">Click **Show advanced options**.</span></span>

      ![Outlook-konfiguration för Office 365](./media/automate-with-logic-apps/flow5.png)

5. <span data-ttu-id="882ac-158">Hello dynamiskt innehåll menyn hello följande:</span><span class="sxs-lookup"><span data-stu-id="882ac-158">On hello dynamic content menu, do hello following:</span></span>

    <span data-ttu-id="882ac-159">a.</span><span class="sxs-lookup"><span data-stu-id="882ac-159">a.</span></span> <span data-ttu-id="882ac-160">Välj **Bilagenamn**.</span><span class="sxs-lookup"><span data-stu-id="882ac-160">Select **Attachment Name**.</span></span>

    <span data-ttu-id="882ac-161">b.</span><span class="sxs-lookup"><span data-stu-id="882ac-161">b.</span></span> <span data-ttu-id="882ac-162">Välj **bifogade filer**.</span><span class="sxs-lookup"><span data-stu-id="882ac-162">Select **Attachment Content**.</span></span>
    
    <span data-ttu-id="882ac-163">c.</span><span class="sxs-lookup"><span data-stu-id="882ac-163">c.</span></span> <span data-ttu-id="882ac-164">I hello **är HTML** väljer **Ja**.</span><span class="sxs-lookup"><span data-stu-id="882ac-164">In hello **Is HTML** box, select **Yes**.</span></span>

      ![Skärm för konfiguration av Office 365 e-post](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a><span data-ttu-id="882ac-166">Steg 7: Spara och testa din logikapp</span><span class="sxs-lookup"><span data-stu-id="882ac-166">Step 7: Save and test your logic app</span></span>
* <span data-ttu-id="882ac-167">Klicka på **spara** toosave ändringarna.</span><span class="sxs-lookup"><span data-stu-id="882ac-167">Click **Save** toosave your changes.</span></span>

<span data-ttu-id="882ac-168">Du kan vänta hello utlösaren toorun hello logikapp eller köra omedelbart hello logikapp genom att välja **kör**.</span><span class="sxs-lookup"><span data-stu-id="882ac-168">You can wait for hello trigger toorun hello logic app, or you can run hello logic app immediately by selecting **Run**.</span></span>

![Logik app skapa skärmen](./media/automate-with-logic-apps/step7.png)

<span data-ttu-id="882ac-170">När logikappen körs får hello mottagare som du angav i hello e-postlistan ett e-postmeddelande som ser ut som följande hello:</span><span class="sxs-lookup"><span data-stu-id="882ac-170">When your logic app runs, hello recipients you specified in hello email list will receive an email that looks like hello following:</span></span>

![Logik appen e-postmeddelande](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a><span data-ttu-id="882ac-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="882ac-172">Next steps</span></span>

- <span data-ttu-id="882ac-173">Lär dig mer om hur du skapar [Analytics-frågor](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="882ac-173">Learn more about creating [Analytics queries](app-insights-analytics-using.md).</span></span>
- <span data-ttu-id="882ac-174">Lär dig mer om [Logikappar](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).</span><span class="sxs-lookup"><span data-stu-id="882ac-174">Learn more about [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).</span></span>



<!--Link references-->





