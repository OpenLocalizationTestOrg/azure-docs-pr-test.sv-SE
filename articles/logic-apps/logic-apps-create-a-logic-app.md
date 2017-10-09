---
title: "aaaCreate första arbetsflödet mellan molnappar & molntjänster - Azure Logic Apps | Microsoft Docs"
description: "Automatisera affärsprocesser för systemintegrering och EAI-scenarion (Enterprise Application Integration) genom att skapa och köra arbetsflöden i Azure Logic Apps"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
keywords: workflow, cloud apps, cloud services, business processes, system integration, enterprise application integration, EAI
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/31/2017
ms.author: LADocs; jehollan; estfan
ms.openlocfilehash: 17ec589b1c8923b5ad3e6479fc856b6ac81754ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-logic-app-workflow-tooautomate-processes-between-cloud-apps-and-cloud-services"></a><span data-ttu-id="324cb-104">Skapa din första logikapp tooautomate arbetsflödesprocesser mellan molnappar och molntjänster</span><span class="sxs-lookup"><span data-stu-id="324cb-104">Create your first logic app workflow tooautomate processes between cloud apps and cloud services</span></span>

<span data-ttu-id="324cb-105">Utan att skriva kod kan du automatisera affärsprocesser enklare och snabbare när du skapar och kör arbetsflöden med [Azure Logic Apps](logic-apps-what-are-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="324cb-105">Without writing any code, you can automate business processes more easily and quickly when you create and run workflows with [Azure Logic Apps](logic-apps-what-are-logic-apps.md).</span></span> <span data-ttu-id="324cb-106">Den här första exemplet visar hur toocreate ett grundläggande logiken app arbetsflöde som söker en RSS-feed för nytt innehåll på en webbplats.</span><span class="sxs-lookup"><span data-stu-id="324cb-106">This first example shows how toocreate a basic logic app workflow that checks an RSS feed for new content on a website.</span></span> <span data-ttu-id="324cb-107">När nya objekt visas i hello webbplats feed skickar hello logikapp e-post från ett Outlook eller Gmail-konto.</span><span class="sxs-lookup"><span data-stu-id="324cb-107">When new items appear in hello website's feed, hello logic app sends email from an Outlook or Gmail account.</span></span>

<span data-ttu-id="324cb-108">toocreate och kör en logikapp, vad du behöver:</span><span class="sxs-lookup"><span data-stu-id="324cb-108">toocreate and run a logic app, you need these items:</span></span>

* <span data-ttu-id="324cb-109">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="324cb-109">An Azure subscription.</span></span> <span data-ttu-id="324cb-110">Om du inte har en prenumeration kan du [börja med ett kostnadsfritt Azure-konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="324cb-110">If you don't have a subscription, you can [start with a free Azure account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="324cb-111">Annars kan du [registrera dig för en prenumeration enligt principen Betala per användning](https://azure.microsoft.com/pricing/purchase-options/).</span><span class="sxs-lookup"><span data-stu-id="324cb-111">Otherwise, you can [sign up for a Pay-As-You-Go subscription](https://azure.microsoft.com/pricing/purchase-options/).</span></span>

  <span data-ttu-id="324cb-112">Din Azure-prenumeration används för fakturering av användning av logikappar.</span><span class="sxs-lookup"><span data-stu-id="324cb-112">Your Azure subscription is used for billing logic app usage.</span></span> <span data-ttu-id="324cb-113">Lär dig hur [användningsmätning](../logic-apps/logic-apps-pricing.md) och [prissättning](https://azure.microsoft.com/pricing/details/logic-apps) fungerar för Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="324cb-113">Learn how [usage metering](../logic-apps/logic-apps-pricing.md) and [pricing](https://azure.microsoft.com/pricing/details/logic-apps) work for Azure Logic Apps.</span></span>

<span data-ttu-id="324cb-114">Det här exemplet kräver också dessa objekt:</span><span class="sxs-lookup"><span data-stu-id="324cb-114">Also, this example requires these items:</span></span>

* <span data-ttu-id="324cb-115">Ett Outlook.com-, Office 365 Outlook- eller Gmail-konto</span><span class="sxs-lookup"><span data-stu-id="324cb-115">An Outlook.com, Office 365 Outlook, or Gmail account</span></span>

    > [!TIP]
    > <span data-ttu-id="324cb-116">Om du har ett personligt [Microsoft-konto](https://account.microsoft.com/account) har du ett Outlook.com-konto.</span><span class="sxs-lookup"><span data-stu-id="324cb-116">If you have a personal [Microsoft account](https://account.microsoft.com/account), you have an Outlook.com account.</span></span> <span data-ttu-id="324cb-117">Om du har ett Azure-arbetskonto eller -skolkonto har du annars ett **Office 365 Outlook**-konto.</span><span class="sxs-lookup"><span data-stu-id="324cb-117">Otherwise, if you have an Azure work or school account, you have an **Office 365 Outlook** account.</span></span>

* <span data-ttu-id="324cb-118">En länk tooa webbplatsens RSS-feed.</span><span class="sxs-lookup"><span data-stu-id="324cb-118">A link tooa website's RSS feed.</span></span> <span data-ttu-id="324cb-119">Det här exemplet används hello [RSS-feed för det här numret från hello CNN.com webbplats](http://rss.cnn.com/rss/cnn_topstories.rss):`http://rss.cnn.com/rss/cnn_topstories.rss`</span><span class="sxs-lookup"><span data-stu-id="324cb-119">This example uses hello [RSS feed for top stories from hello CNN.com website](http://rss.cnn.com/rss/cnn_topstories.rss): `http://rss.cnn.com/rss/cnn_topstories.rss`</span></span>

## <a name="add-a-trigger-that-starts-your-workflow"></a><span data-ttu-id="324cb-120">Lägg till en utlösare som startar arbetsflödet</span><span class="sxs-lookup"><span data-stu-id="324cb-120">Add a trigger that starts your workflow</span></span>

<span data-ttu-id="324cb-121">En [ *utlösaren* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) är en händelse som startar logik app arbetsflödet och hello första element som din logikapp måste.</span><span class="sxs-lookup"><span data-stu-id="324cb-121">A [*trigger*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is an event that starts your logic app workflow and is hello first item that your logic app needs.</span></span>

1. <span data-ttu-id="324cb-122">Logga in toohello [Azure-portalen](https://portal.azure.com "Azure-portalen").</span><span class="sxs-lookup"><span data-stu-id="324cb-122">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span>

2. <span data-ttu-id="324cb-123">Välj hello vänstra menyn **ny** > **Enterprise Integration** > **Logikapp** som visas här:</span><span class="sxs-lookup"><span data-stu-id="324cb-123">From hello left menu, choose **New** > **Enterprise Integration** > **Logic App** as shown here:</span></span>

     ![Azure Portal, Ny, Enterprise-integration, Logikapp](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > <span data-ttu-id="324cb-125">Du kan också välja **ny**, Skriv i sökrutan hello `logic app`, och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="324cb-125">You can also choose **New**, then in hello search box, type `logic app`, and press Enter.</span></span> <span data-ttu-id="324cb-126">Välj **Logikapp** > **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="324cb-126">Then choose **Logic App** > **Create**.</span></span>

3. <span data-ttu-id="324cb-127">Namnge logikappen och välj din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="324cb-127">Name your logic app and select your Azure subscription.</span></span> <span data-ttu-id="324cb-128">Skapa eller välj nu en Azure-resursgrupp som hjälper dig att organisera och hantera relaterade Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="324cb-128">Now create or select an Azure resource group, which helps you organize and manage related Azure resources.</span></span> <span data-ttu-id="324cb-129">Välj slutligen hello datacenter plats som värd för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="324cb-129">Finally, select hello datacenter location for hosting your logic app.</span></span> <span data-ttu-id="324cb-130">När du är klar kan du välja **PIN-kod toodashboard** och sedan **skapa**.</span><span class="sxs-lookup"><span data-stu-id="324cb-130">When you're ready, choose **Pin toodashboard** and then **Create**.</span></span>

     ![Information om logikappar](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > <span data-ttu-id="324cb-132">När du väljer **PIN-kod toodashboard**, logikappen visas på hello Azure instrumentpanelen efter distributionen och öppnas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="324cb-132">When you select **Pin toodashboard**, your logic app appears on hello Azure dashboard after deployment, and opens automatically.</span></span> <span data-ttu-id="324cb-133">Om din logikapp inte visas på instrumentpanelen hello på hello **alla resurser** panelen, väljer **finns mer**, och välj din logikapp.</span><span class="sxs-lookup"><span data-stu-id="324cb-133">If your logic app doesn't appear on hello dashboard, on hello **All resources** tile, choose **See More**, and select your logic app.</span></span> <span data-ttu-id="324cb-134">Eller välj hello vänstra menyn **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="324cb-134">Or on hello left menu, choose **More services**.</span></span> <span data-ttu-id="324cb-135">Välj **Logic Apps** under **Enterprise-integration** och välj sedan din logikapp.</span><span class="sxs-lookup"><span data-stu-id="324cb-135">Under **Enterprise Integration**, choose **Logic Apps**, and select your logic app.</span></span>

4. <span data-ttu-id="324cb-136">När du öppnar din logikapp för hello första gången, visar hello logik App Designer mallar som du kan använda tooget igång.</span><span class="sxs-lookup"><span data-stu-id="324cb-136">When you open your logic app for hello first time, hello Logic App Designer shows templates that you can use tooget started.</span></span> <span data-ttu-id="324cb-137">Välj nu **Tom logikapp** så att du kan bygga din logikapp från början.</span><span class="sxs-lookup"><span data-stu-id="324cb-137">For now, choose **Blank Logic App** so you can build your logic app from scratch.</span></span>

    <span data-ttu-id="324cb-138">hello logik App Designer öppnas och visar tillgängliga tjänster och *utlösare* som du kan använda i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="324cb-138">hello Logic App Designer opens and shows  available services and *triggers* that  you can use in your logic app.</span></span>

5. <span data-ttu-id="324cb-139">Skriv i sökrutan hello `RSS`, och välj den här utlösaren: **RSS - när en feed objekt publiceras**</span><span class="sxs-lookup"><span data-stu-id="324cb-139">In hello search box, type `RSS`, and select this trigger: **RSS - When a feed item is published**</span></span> 

    ![RSS-utlösare](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. <span data-ttu-id="324cb-141">Ange hello länk hello webbplats RSS-feed som du vill tootrack.</span><span class="sxs-lookup"><span data-stu-id="324cb-141">Enter hello link for hello website's RSS feed that you want tootrack.</span></span> 

     <span data-ttu-id="324cb-142">Du kan också ändra **Frekvens** och **Intervall**.</span><span class="sxs-lookup"><span data-stu-id="324cb-142">You can also change **Frequency** and **Interval**.</span></span> 
     <span data-ttu-id="324cb-143">Inställningarna avgör hur ofta logikappen ska söka efter nya objekt och returnera alla objekt som hittas under det angivna tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="324cb-143">These settings determine how often your logic app checks for new items and returns all items found during that time span.</span></span>

     <span data-ttu-id="324cb-144">I det här exemplet ska vi Kontrollera att varje dag för aktuellt Bokförd toohello CNN webbplats.</span><span class="sxs-lookup"><span data-stu-id="324cb-144">For this example, let's check every day for   top stories posted toohello CNN website.</span></span>

     ![Konfigurera utlösare med RSS-flöde, frekvens och intervall](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. <span data-ttu-id="324cb-146">Spara ditt arbete nu.</span><span class="sxs-lookup"><span data-stu-id="324cb-146">Save your work for now.</span></span> <span data-ttu-id="324cb-147">(På hello designer kommandofältet väljer **spara**.)</span><span class="sxs-lookup"><span data-stu-id="324cb-147">(On hello designer command bar, choose **Save**.)</span></span>

   ![Spara din logikapp](media/logic-apps-create-a-logic-app/save-logic-app.png)

   <span data-ttu-id="324cb-149">När du sparar, logikappen lanseras, men för närvarande logikappen endast söker efter nya objekt i angivna hello RSS-feed.</span><span class="sxs-lookup"><span data-stu-id="324cb-149">When you save, your logic app goes live, but currently, your logic app only checks for new items in hello specified RSS feed.</span></span> 
   <span data-ttu-id="324cb-150">toomake utlöses för det här exemplet är mer användbar vi lägga till en åtgärd som din logikapp utför efter utlösaren.</span><span class="sxs-lookup"><span data-stu-id="324cb-150">toomake this example more useful, we add an action that your logic app performs after your trigger fires.</span></span>

## <a name="add-an-action-that-responds-tooyour-trigger"></a><span data-ttu-id="324cb-151">Lägg till en åtgärd som svarar tooyour utlösare</span><span class="sxs-lookup"><span data-stu-id="324cb-151">Add an action that responds tooyour trigger</span></span>

<span data-ttu-id="324cb-152">En [*åtgärd*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) är en aktivitet som utförs av logikapparbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="324cb-152">An [*action*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is a task performed by your logic app workflow.</span></span> <span data-ttu-id="324cb-153">När du lägger till en utlösare tooyour logikapp, du kan lägga till en åtgärd tooperform åtgärder med data som genereras av den här utlösaren.</span><span class="sxs-lookup"><span data-stu-id="324cb-153">After you add a trigger tooyour logic app, you can add an action tooperform operations with data generated by that trigger.</span></span> <span data-ttu-id="324cb-154">I vårt exempel vi nu lägga till en åtgärd som skickar e-postmeddelande när nya objekt visas i hello webbplats RSS-feed.</span><span class="sxs-lookup"><span data-stu-id="324cb-154">For our example, we now add an action that sends email when new items appear in hello website's RSS feed.</span></span>

1. <span data-ttu-id="324cb-155">Hello designer under utlösaren, Välj **nytt steg** > **lägga till en åtgärd** som visas här:</span><span class="sxs-lookup"><span data-stu-id="324cb-155">In hello designer, under your trigger, choose **New step** > **Add an action** as shown here:</span></span>

   ![Lägga till en åtgärd](media/logic-apps-create-a-logic-app/add-new-action.png)

   <span data-ttu-id="324cb-157">Hej designer visar [tillgängliga kopplingar](../connectors/apis-list.md) så att du kan välja en tooperform åtgärd när utlösaren utlöses.</span><span class="sxs-lookup"><span data-stu-id="324cb-157">hello designer shows [available connectors](../connectors/apis-list.md) so that you can select an action tooperform when your trigger fires.</span></span>

2. <span data-ttu-id="324cb-158">Baserat på ditt e-postkonto, åtgärderna hello för Outlook eller Gmail.</span><span class="sxs-lookup"><span data-stu-id="324cb-158">Based on your email account, follow hello steps for Outlook or Gmail.</span></span>

   * <span data-ttu-id="324cb-159">Ange toosend e-post från kontot Outlook i sökrutan hello `outlook`.</span><span class="sxs-lookup"><span data-stu-id="324cb-159">toosend email from your Outlook account, in hello search box, enter `outlook`.</span></span> <span data-ttu-id="324cb-160">Under **Tjänster** väljer du **Outlook.com** för personliga Microsoft-konton eller välj **Office 365 Outlook** Azure-arbetskonton eller -skolkonton.</span><span class="sxs-lookup"><span data-stu-id="324cb-160">Under **Services**, choose **Outlook.com** for personal Microsoft accounts, or choose **Office 365 Outlook** for Azure work or school accounts.</span></span> 
   <span data-ttu-id="324cb-161">Gå till **Åtgärder** och välj **Skicka ett e-postmeddelande**.</span><span class="sxs-lookup"><span data-stu-id="324cb-161">Under **Actions**, select **Send an email**.</span></span>

       ![Välj Outlook-åtgärden Skicka ett e-postmeddelande](media/logic-apps-create-a-logic-app/actions.png)

   * <span data-ttu-id="324cb-163">Ange toosend e-post från kontot Gmail i sökrutan hello `gmail`.</span><span class="sxs-lookup"><span data-stu-id="324cb-163">toosend email from your Gmail account, in hello search box, enter `gmail`.</span></span> 
   <span data-ttu-id="324cb-164">Gå till **Åtgärder** och välj **Skicka e-post**.</span><span class="sxs-lookup"><span data-stu-id="324cb-164">Under **Actions**, select **Send email**.</span></span>

       ![Välj ”Gmail – Skicka e-post”](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. <span data-ttu-id="324cb-166">När du uppmanas att ange autentiseringsuppgifter kan du logga in med hello användarnamnet och lösenordet för ditt e-postkonto.</span><span class="sxs-lookup"><span data-stu-id="324cb-166">When you're prompted for credentials, sign in with hello username and password for your email account.</span></span> 

4. <span data-ttu-id="324cb-167">Ange hello information för den här åtgärden som hello mål e-postadress och välj hello parametrar för hello data tooinclude i hello e-post, till exempel:</span><span class="sxs-lookup"><span data-stu-id="324cb-167">Provide hello details for this action, like hello destination email address, and choose hello parameters for hello data tooinclude in hello email, for example:</span></span>

   ![Välj data tooinclude i e-post](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    <span data-ttu-id="324cb-169">Så om du väljer Outlook kan logikappen exempelvis se ut så här:</span><span class="sxs-lookup"><span data-stu-id="324cb-169">So if you chose Outlook,  your logic app might look like this example:</span></span>

    ![Slutförd logikapp](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.  <span data-ttu-id="324cb-171">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="324cb-171">Save your changes.</span></span> <span data-ttu-id="324cb-172">(På hello designer kommandofältet väljer **spara**.)</span><span class="sxs-lookup"><span data-stu-id="324cb-172">(On hello designer command bar, choose **Save**.)</span></span>

6. <span data-ttu-id="324cb-173">Du kan nu manuellt köra din logikapp för testning.</span><span class="sxs-lookup"><span data-stu-id="324cb-173">You can now manually run your logic app for testing.</span></span> <span data-ttu-id="324cb-174">På hello designer kommandofältet väljer **kör**.</span><span class="sxs-lookup"><span data-stu-id="324cb-174">On hello designer command bar, choose **Run**.</span></span> <span data-ttu-id="324cb-175">Annars kan du låta din logikapp Kontrollera hello angetts baserat på hello-schema som du ställer in en RSS-feed.</span><span class="sxs-lookup"><span data-stu-id="324cb-175">Otherwise, you can let your logic app check hello specified RSS feed based on hello schedule that you set up.</span></span>

   <span data-ttu-id="324cb-176">Om din logikapp hittar nya objekt, skickar hello logikapp e-postmeddelande som innehåller valda data.</span><span class="sxs-lookup"><span data-stu-id="324cb-176">If your logic app finds new items, hello logic app sends email that includes your selected data.</span></span> 
   <span data-ttu-id="324cb-177">Om det finns inga nya objekt logikappen hoppar över hello-åtgärd som skickar e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="324cb-177">If no new items are found, your logic app skips hello action that sends email.</span></span>

7. <span data-ttu-id="324cb-178">toomonitor och kontrollera din logikapp är kör och utlösa historik på logiken app-menyn väljer **översikt**.</span><span class="sxs-lookup"><span data-stu-id="324cb-178">toomonitor and check your logic app's run and trigger history, on your logic app menu, choose **Overview**.</span></span>

   ![Övervaka och visa logikappens körnings- och utlösningshistorik](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > <span data-ttu-id="324cb-180">Om du inte hittar hello data som du förväntar dig i hello kommandofält försöka **uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="324cb-180">If you don't find hello data that you expect, on hello command bar, try choosing **Refresh**.</span></span>

   <span data-ttu-id="324cb-181">Mer information om status för din logikapp toolearn eller kör och utlösa historiken eller toodiagnose din logikapp, se [felsöka din logikapp](logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="324cb-181">toolearn more about your logic app's status or run and trigger history, or toodiagnose your logic app, see [Troubleshoot your logic app](logic-apps-diagnosing-failures.md).</span></span>

      > [!NOTE]
      > <span data-ttu-id="324cb-182">Logikappen fortsätter att köra tills du stänger av den.</span><span class="sxs-lookup"><span data-stu-id="324cb-182">Your logic app continues running until you turn off your app.</span></span> <span data-ttu-id="324cb-183">tooturn av din app just nu på logiken app-menyn, Välj **översikt**.</span><span class="sxs-lookup"><span data-stu-id="324cb-183">tooturn off your app for now, on your logic app menu, choose **Overview**.</span></span> <span data-ttu-id="324cb-184">Välj på hello kommandofältet **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="324cb-184">On hello command bar, choose **Disable**.</span></span>

<span data-ttu-id="324cb-185">Gratulerar! Du har precis konfigurerat och kört din första baslogikapp.</span><span class="sxs-lookup"><span data-stu-id="324cb-185">Congratulations, you just set up and run your first basic logic app.</span></span> <span data-ttu-id="324cb-186">Du fick också lära dig hur enkelt du kan skapa arbetsflöden för att automatisera processer och integrera molnappar och molntjänster – utan att skriva kod.</span><span class="sxs-lookup"><span data-stu-id="324cb-186">You also learned how easily you can create workflows that automate processes, and integrate cloud apps and cloud services - all without code.</span></span>

## <a name="manage-your-logic-app"></a><span data-ttu-id="324cb-187">Hantera din logikapp</span><span class="sxs-lookup"><span data-stu-id="324cb-187">Manage your logic app</span></span>

<span data-ttu-id="324cb-188">toomanage din app kan du utföra uppgifter som Kontrollera hello status, redigera, visa historik, inaktivera eller ta bort din logikapp.</span><span class="sxs-lookup"><span data-stu-id="324cb-188">toomanage your app, you can perform tasks like check hello status, edit, view history, turn off, or delete your logic app.</span></span>

1. <span data-ttu-id="324cb-189">Logga in toohello [Azure-portalen](https://portal.azure.com "Azure-portalen").</span><span class="sxs-lookup"><span data-stu-id="324cb-189">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span>

2. <span data-ttu-id="324cb-190">Välj hello vänstra menyn **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="324cb-190">On hello left menu, choose **More services**.</span></span> <span data-ttu-id="324cb-191">Välj **Logikappar** under **Enterprise-integration**.</span><span class="sxs-lookup"><span data-stu-id="324cb-191">Under **Enterprise Integration**, choose **Logic Apps**.</span></span> <span data-ttu-id="324cb-192">Välj din logikapp.</span><span class="sxs-lookup"><span data-stu-id="324cb-192">Select your logic app.</span></span> 

   <span data-ttu-id="324cb-193">Hello logik app menyn hittar du dessa hanteringsaktiviteter för logik app:</span><span class="sxs-lookup"><span data-stu-id="324cb-193">In hello logic app menu, you can find these logic app management tasks:</span></span>

   |<span data-ttu-id="324cb-194">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="324cb-194">Task</span></span>|<span data-ttu-id="324cb-195">Steg</span><span class="sxs-lookup"><span data-stu-id="324cb-195">Steps</span></span>| 
   |:---|:---| 
   | <span data-ttu-id="324cb-196">Visa appens status, körningshistorik och allmän information</span><span class="sxs-lookup"><span data-stu-id="324cb-196">View your app's status, execution history, and general information</span></span>| <span data-ttu-id="324cb-197">Välj **Översikt**.</span><span class="sxs-lookup"><span data-stu-id="324cb-197">Choose **Overview**.</span></span>| 
   | <span data-ttu-id="324cb-198">Redigera din app</span><span class="sxs-lookup"><span data-stu-id="324cb-198">Edit your app</span></span> | <span data-ttu-id="324cb-199">Välj **Logic App Designer**.</span><span class="sxs-lookup"><span data-stu-id="324cb-199">Choose **Logic App Designer**.</span></span> | 
   | <span data-ttu-id="324cb-200">Visa arbetsflödet för JSON-definitionen för appen</span><span class="sxs-lookup"><span data-stu-id="324cb-200">View your app's workflow JSON definition</span></span> | <span data-ttu-id="324cb-201">Välj **Logikapp kodvy**.</span><span class="sxs-lookup"><span data-stu-id="324cb-201">Choose **Logic App Code View**.</span></span> | 
   | <span data-ttu-id="324cb-202">Visa åtgärder som utförs i logikappen</span><span class="sxs-lookup"><span data-stu-id="324cb-202">View operations performed on your logic app</span></span> | <span data-ttu-id="324cb-203">Välj **Aktivitetslogg**.</span><span class="sxs-lookup"><span data-stu-id="324cb-203">Choose **Activity log**.</span></span> | 
   | <span data-ttu-id="324cb-204">Visa tidigare versioner för din logikapp</span><span class="sxs-lookup"><span data-stu-id="324cb-204">View past versions for your logic app</span></span> | <span data-ttu-id="324cb-205">Välj **Versioner**.</span><span class="sxs-lookup"><span data-stu-id="324cb-205">Choose **Versions**.</span></span> | 
   | <span data-ttu-id="324cb-206">Inaktivera din app tillfälligt</span><span class="sxs-lookup"><span data-stu-id="324cb-206">Turn off your app temporarily</span></span> | <span data-ttu-id="324cb-207">Välj **översikt**, på hello kommandofältet Välj **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="324cb-207">Choose **Overview**, then on hello command bar, choose **Disable**.</span></span> | 
   | <span data-ttu-id="324cb-208">Ta bort din app</span><span class="sxs-lookup"><span data-stu-id="324cb-208">Delete your app</span></span> | <span data-ttu-id="324cb-209">Välj **översikt**, på hello kommandofältet Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="324cb-209">Choose **Overview**, then on hello command bar, choose **Delete**.</span></span> <span data-ttu-id="324cb-210">Ange logikappens namn och välj **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="324cb-210">Enter your logic app's name, and choose **Delete**.</span></span> | 

## <a name="get-help"></a><span data-ttu-id="324cb-211">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="324cb-211">Get help</span></span>

<span data-ttu-id="324cb-212">tooask frågor besvara frågor och lär dig vilka andra Azure Logikappar användarna gör besök hello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="324cb-212">tooask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="324cb-213">toohelp förbättra Azure Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Azure Logikappar användare feedbackwebbplats](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="324cb-213">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="324cb-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="324cb-214">Next steps</span></span>

*  [<span data-ttu-id="324cb-215">Lägga till villkor och kör arbetsflöden</span><span class="sxs-lookup"><span data-stu-id="324cb-215">Add conditions and run workflows</span></span>](../logic-apps/logic-apps-use-logic-app-features.md)
*    [<span data-ttu-id="324cb-216">Mallar för logikappar</span><span class="sxs-lookup"><span data-stu-id="324cb-216">Logic app templates</span></span>](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [<span data-ttu-id="324cb-217">Skapa logikappar från Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="324cb-217">Create logic apps from Azure Resource Manager templates</span></span>](../logic-apps/logic-apps-arm-provision.md)
