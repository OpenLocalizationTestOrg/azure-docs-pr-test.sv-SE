---
title: "Skapa ditt första arbetsflöde mellan molnappar och molntjänster – Azure Logic Apps | Microsoft-dokument"
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
ms.openlocfilehash: 204bf123509729b60b55c306050cef54aa7fecc5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-first-logic-app-workflow-to-automate-processes-between-cloud-apps-and-cloud-services"></a><span data-ttu-id="3eb20-104">Skapa ditt första logikapparbetsflöde för att automatisera processer mellan molnappar och molntjänster</span><span class="sxs-lookup"><span data-stu-id="3eb20-104">Create your first logic app workflow to automate processes between cloud apps and cloud services</span></span>

<span data-ttu-id="3eb20-105">Utan att skriva kod kan du automatisera affärsprocesser enklare och snabbare när du skapar och kör arbetsflöden med [Azure Logic Apps](logic-apps-what-are-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="3eb20-105">Without writing any code, you can automate business processes more easily and quickly when you create and run workflows with [Azure Logic Apps](logic-apps-what-are-logic-apps.md).</span></span> <span data-ttu-id="3eb20-106">Det här första exemplet visar hur du skapar ett basarbetsflöde för logikappar som kontrollerar ett RSS-flöde för att se om det finns nytt innehåll på en webbplats.</span><span class="sxs-lookup"><span data-stu-id="3eb20-106">This first example shows how to create a basic logic app workflow that checks an RSS feed for new content on a website.</span></span> <span data-ttu-id="3eb20-107">När nya objekt visas i webbplatsens flöde skickar logikappen e-post från ett Outlook- eller Gmail-konto.</span><span class="sxs-lookup"><span data-stu-id="3eb20-107">When new items appear in the website's feed, the logic app sends email from an Outlook or Gmail account.</span></span>

<span data-ttu-id="3eb20-108">Du behöver dessa objekt för att skapa och köra en logikapp:</span><span class="sxs-lookup"><span data-stu-id="3eb20-108">To create and run a logic app, you need these items:</span></span>

* <span data-ttu-id="3eb20-109">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3eb20-109">An Azure subscription.</span></span> <span data-ttu-id="3eb20-110">Om du inte har en prenumeration kan du [börja med ett kostnadsfritt Azure-konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="3eb20-110">If you don't have a subscription, you can [start with a free Azure account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="3eb20-111">Annars kan du [registrera dig för en prenumeration enligt principen Betala per användning](https://azure.microsoft.com/pricing/purchase-options/).</span><span class="sxs-lookup"><span data-stu-id="3eb20-111">Otherwise, you can [sign up for a Pay-As-You-Go subscription](https://azure.microsoft.com/pricing/purchase-options/).</span></span>

  <span data-ttu-id="3eb20-112">Din Azure-prenumeration används för fakturering av användning av logikappar.</span><span class="sxs-lookup"><span data-stu-id="3eb20-112">Your Azure subscription is used for billing logic app usage.</span></span> <span data-ttu-id="3eb20-113">Lär dig hur [användningsmätning](../logic-apps/logic-apps-pricing.md) och [prissättning](https://azure.microsoft.com/pricing/details/logic-apps) fungerar för Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="3eb20-113">Learn how [usage metering](../logic-apps/logic-apps-pricing.md) and [pricing](https://azure.microsoft.com/pricing/details/logic-apps) work for Azure Logic Apps.</span></span>

<span data-ttu-id="3eb20-114">Det här exemplet kräver också dessa objekt:</span><span class="sxs-lookup"><span data-stu-id="3eb20-114">Also, this example requires these items:</span></span>

* <span data-ttu-id="3eb20-115">Ett Outlook.com-, Office 365 Outlook- eller Gmail-konto</span><span class="sxs-lookup"><span data-stu-id="3eb20-115">An Outlook.com, Office 365 Outlook, or Gmail account</span></span>

    > [!TIP]
    > <span data-ttu-id="3eb20-116">Om du har ett personligt [Microsoft-konto](https://account.microsoft.com/account) har du ett Outlook.com-konto.</span><span class="sxs-lookup"><span data-stu-id="3eb20-116">If you have a personal [Microsoft account](https://account.microsoft.com/account), you have an Outlook.com account.</span></span> <span data-ttu-id="3eb20-117">Om du har ett Azure-arbetskonto eller -skolkonto har du annars ett **Office 365 Outlook**-konto.</span><span class="sxs-lookup"><span data-stu-id="3eb20-117">Otherwise, if you have an Azure work or school account, you have an **Office 365 Outlook** account.</span></span>

* <span data-ttu-id="3eb20-118">En länk till RSS-flödet på en webbplats.</span><span class="sxs-lookup"><span data-stu-id="3eb20-118">A link to a website's RSS feed.</span></span> <span data-ttu-id="3eb20-119">Det här exemplet använder [RSS-feed för de viktigaste berättelserna från webbplatsen CNN.com](http://rss.cnn.com/rss/cnn_topstories.rss):`http://rss.cnn.com/rss/cnn_topstories.rss`</span><span class="sxs-lookup"><span data-stu-id="3eb20-119">This example uses the [RSS feed for top stories from the CNN.com website](http://rss.cnn.com/rss/cnn_topstories.rss): `http://rss.cnn.com/rss/cnn_topstories.rss`</span></span>

## <a name="add-a-trigger-that-starts-your-workflow"></a><span data-ttu-id="3eb20-120">Lägg till en utlösare som startar arbetsflödet</span><span class="sxs-lookup"><span data-stu-id="3eb20-120">Add a trigger that starts your workflow</span></span>

<span data-ttu-id="3eb20-121">En [*utlösare*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) är en händelse som startar logikappens arbetsflöde och är det första objekt som krävs av din logikapp.</span><span class="sxs-lookup"><span data-stu-id="3eb20-121">A [*trigger*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is an event that starts your logic app workflow and is the first item that your logic app needs.</span></span>

1. <span data-ttu-id="3eb20-122">Logga in på [Azure Portal](https://portal.azure.com "Azure Portal").</span><span class="sxs-lookup"><span data-stu-id="3eb20-122">Sign in to the [Azure portal](https://portal.azure.com "Azure portal").</span></span>

2. <span data-ttu-id="3eb20-123">Välj **Ny** > **Enterprise-integration** > **Logikapp** från den vänstra menyn på det sätt som visas här:</span><span class="sxs-lookup"><span data-stu-id="3eb20-123">From the left menu, choose **New** > **Enterprise Integration** > **Logic App** as shown here:</span></span>

     ![Azure Portal, Ny, Enterprise-integration, Logikapp](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > <span data-ttu-id="3eb20-125">Du kan också välja **Ny**, skriva `logic app` i sökrutan och sedan trycka på Retur.</span><span class="sxs-lookup"><span data-stu-id="3eb20-125">You can also choose **New**, then in the search box, type `logic app`, and press Enter.</span></span> <span data-ttu-id="3eb20-126">Välj **Logikapp** > **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3eb20-126">Then choose **Logic App** > **Create**.</span></span>

3. <span data-ttu-id="3eb20-127">Namnge logikappen och välj din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3eb20-127">Name your logic app and select your Azure subscription.</span></span> <span data-ttu-id="3eb20-128">Skapa eller välj nu en Azure-resursgrupp som hjälper dig att organisera och hantera relaterade Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="3eb20-128">Now create or select an Azure resource group, which helps you organize and manage related Azure resources.</span></span> <span data-ttu-id="3eb20-129">Välj slutligen datacenterplatsen som ska fungera som värd för logikappen.</span><span class="sxs-lookup"><span data-stu-id="3eb20-129">Finally, select the datacenter location for hosting your logic app.</span></span> <span data-ttu-id="3eb20-130">När du är klar väljer du **Fäst på instrumentpanelen** och sedan **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3eb20-130">When you're ready, choose **Pin to dashboard** and then **Create**.</span></span>

     ![Information om logikappar](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > <span data-ttu-id="3eb20-132">När du väljer **Fäst på instrumentpanelen** visas logikappen på Azure-instrumentpanelen efter distributionen och öppnas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3eb20-132">When you select **Pin to dashboard**, your logic app appears on the Azure dashboard after deployment, and opens automatically.</span></span> <span data-ttu-id="3eb20-133">Om logikappen inte visas på instrumentpanelen går du till ikonen **Alla resurser** och väljer **Visa mer**, och sedan din logikapp.</span><span class="sxs-lookup"><span data-stu-id="3eb20-133">If your logic app doesn't appear on the dashboard, on the **All resources** tile, choose **See More**, and select your logic app.</span></span> <span data-ttu-id="3eb20-134">Klicka på **Fler tjänster** på den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="3eb20-134">Or on the left menu, choose **More services**.</span></span> <span data-ttu-id="3eb20-135">Välj **Logic Apps** under **Enterprise-integration** och välj sedan din logikapp.</span><span class="sxs-lookup"><span data-stu-id="3eb20-135">Under **Enterprise Integration**, choose **Logic Apps**, and select your logic app.</span></span>

4. <span data-ttu-id="3eb20-136">När du öppnar logikappen för första gången visas Logic App Designer med mallar som du kan använda för att komma igång.</span><span class="sxs-lookup"><span data-stu-id="3eb20-136">When you open your logic app for the first time, the Logic App Designer shows templates that you can use to get started.</span></span> <span data-ttu-id="3eb20-137">Välj nu **Tom logikapp** så att du kan bygga din logikapp från början.</span><span class="sxs-lookup"><span data-stu-id="3eb20-137">For now, choose **Blank Logic App** so you can build your logic app from scratch.</span></span>

    <span data-ttu-id="3eb20-138">Logik App Designer öppnas och visar tillgängliga tjänster och *utlösare* som du kan använda i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="3eb20-138">The Logic App Designer opens and shows  available services and *triggers* that  you can use in your logic app.</span></span>

5. <span data-ttu-id="3eb20-139">Gå till sökrutan och skriv `RSS`, och välj den här utlösaren: **RSS – När ett flödesobjekt publiceras**</span><span class="sxs-lookup"><span data-stu-id="3eb20-139">In the search box, type `RSS`, and select this trigger: **RSS - When a feed item is published**</span></span> 

    ![RSS-utlösare](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. <span data-ttu-id="3eb20-141">Ange länken till det RSS-flöde för webbplatsen som du vill spåra.</span><span class="sxs-lookup"><span data-stu-id="3eb20-141">Enter the link for the website's RSS feed that you want to track.</span></span> 

     <span data-ttu-id="3eb20-142">Du kan också ändra **Frekvens** och **Intervall**.</span><span class="sxs-lookup"><span data-stu-id="3eb20-142">You can also change **Frequency** and **Interval**.</span></span> 
     <span data-ttu-id="3eb20-143">Inställningarna avgör hur ofta logikappen ska söka efter nya objekt och returnera alla objekt som hittas under det angivna tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="3eb20-143">These settings determine how often your logic app checks for new items and returns all items found during that time span.</span></span>

     <span data-ttu-id="3eb20-144">I det här exemplet ska vi kontrollera varje dag om det finns nya objekt upplagda på CNN:s webbplats.</span><span class="sxs-lookup"><span data-stu-id="3eb20-144">For this example, let's check every day for   top stories posted to the CNN website.</span></span>

     ![Konfigurera utlösare med RSS-flöde, frekvens och intervall](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. <span data-ttu-id="3eb20-146">Spara ditt arbete nu.</span><span class="sxs-lookup"><span data-stu-id="3eb20-146">Save your work for now.</span></span> <span data-ttu-id="3eb20-147">(Välj **Spara** i designerkommandofältet.)</span><span class="sxs-lookup"><span data-stu-id="3eb20-147">(On the designer command bar, choose **Save**.)</span></span>

   ![Spara din logikapp](media/logic-apps-create-a-logic-app/save-logic-app.png)

   <span data-ttu-id="3eb20-149">När du sparar aktiveras logikappen, men den kontrollerar för närvarande endast om det finns nya objekt i det angivna RSS-flödet.</span><span class="sxs-lookup"><span data-stu-id="3eb20-149">When you save, your logic app goes live, but currently, your logic app only checks for new items in the specified RSS feed.</span></span> 
   <span data-ttu-id="3eb20-150">För att göra det här exemplet mer användbart lägger vi till en åtgärd som din logikapp utför när utlösaren har utlösts.</span><span class="sxs-lookup"><span data-stu-id="3eb20-150">To make this example more useful, we add an action that your logic app performs after your trigger fires.</span></span>

## <a name="add-an-action-that-responds-to-your-trigger"></a><span data-ttu-id="3eb20-151">Lägga till en åtgärd som svarar på utlösaren</span><span class="sxs-lookup"><span data-stu-id="3eb20-151">Add an action that responds to your trigger</span></span>

<span data-ttu-id="3eb20-152">En [*åtgärd*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) är en aktivitet som utförs av logikapparbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="3eb20-152">An [*action*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is a task performed by your logic app workflow.</span></span> <span data-ttu-id="3eb20-153">När du lägger till en utlösare i logikappen kan du lägga till en åtgärd för att utföra åtgärder med data som genereras av den utlösaren.</span><span class="sxs-lookup"><span data-stu-id="3eb20-153">After you add a trigger to your logic app, you can add an action to perform operations with data generated by that trigger.</span></span> <span data-ttu-id="3eb20-154">I vårt exempel lägger vi nu till en åtgärd som skickar e-post när nya objekt visas i webbplatsens RSS-flöde.</span><span class="sxs-lookup"><span data-stu-id="3eb20-154">For our example, we now add an action that sends email when new items appear in the website's RSS feed.</span></span>

1. <span data-ttu-id="3eb20-155">I designverktyget går du till utlösaren och väljer **Nytt steg** > **Lägg till en åtgärd** enligt följande:</span><span class="sxs-lookup"><span data-stu-id="3eb20-155">In the designer, under your trigger, choose **New step** > **Add an action** as shown here:</span></span>

   ![Lägga till en åtgärd](media/logic-apps-create-a-logic-app/add-new-action.png)

   <span data-ttu-id="3eb20-157">I designern visas [tillgängliga anslutningsappar](../connectors/apis-list.md) så att du kan välja en åtgärd som ska utföras när utlösaren utlöses.</span><span class="sxs-lookup"><span data-stu-id="3eb20-157">The designer shows [available connectors](../connectors/apis-list.md) so that you can select an action to perform when your trigger fires.</span></span>

2. <span data-ttu-id="3eb20-158">Baserat på ditt e-postkonto följer du stegen för Outlook eller Gmail.</span><span class="sxs-lookup"><span data-stu-id="3eb20-158">Based on your email account, follow the steps for Outlook or Gmail.</span></span>

   * <span data-ttu-id="3eb20-159">Om du vill skicka e-post från Outlook-kontot skriver du `outlook` i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="3eb20-159">To send email from your Outlook account, in the search box, enter `outlook`.</span></span> <span data-ttu-id="3eb20-160">Under **Tjänster** väljer du **Outlook.com** för personliga Microsoft-konton eller välj **Office 365 Outlook** Azure-arbetskonton eller -skolkonton.</span><span class="sxs-lookup"><span data-stu-id="3eb20-160">Under **Services**, choose **Outlook.com** for personal Microsoft accounts, or choose **Office 365 Outlook** for Azure work or school accounts.</span></span> 
   <span data-ttu-id="3eb20-161">Gå till **Åtgärder** och välj **Skicka ett e-postmeddelande**.</span><span class="sxs-lookup"><span data-stu-id="3eb20-161">Under **Actions**, select **Send an email**.</span></span>

       ![Välj Outlook-åtgärden Skicka ett e-postmeddelande](media/logic-apps-create-a-logic-app/actions.png)

   * <span data-ttu-id="3eb20-163">Om du vill skicka e-post från ditt Gmail-konto skriver du `gmail` i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="3eb20-163">To send email from your Gmail account, in the search box, enter `gmail`.</span></span> 
   <span data-ttu-id="3eb20-164">Gå till **Åtgärder** och välj **Skicka e-post**.</span><span class="sxs-lookup"><span data-stu-id="3eb20-164">Under **Actions**, select **Send email**.</span></span>

       ![Välj ”Gmail – Skicka e-post”](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. <span data-ttu-id="3eb20-166">När du uppmanas att ange autentiseringsuppgifter kan du logga in med användarnamnet och lösenordet till ditt e-postkonto.</span><span class="sxs-lookup"><span data-stu-id="3eb20-166">When you're prompted for credentials, sign in with the username and password for your email account.</span></span> 

4. <span data-ttu-id="3eb20-167">Ange uppgifter för den här åtgärden, som e-postmåladress, och välj parametrarna för data som ska inkluderas i e-postmeddelandet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="3eb20-167">Provide the details for this action, like the destination email address, and choose the parameters for the data to include in the email, for example:</span></span>

   ![Välj data som ska ingå i e-postmeddelandet](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    <span data-ttu-id="3eb20-169">Så om du väljer Outlook kan logikappen exempelvis se ut så här:</span><span class="sxs-lookup"><span data-stu-id="3eb20-169">So if you chose Outlook,  your logic app might look like this example:</span></span>

    ![Slutförd logikapp](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.  <span data-ttu-id="3eb20-171">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="3eb20-171">Save your changes.</span></span> <span data-ttu-id="3eb20-172">(Välj **Spara** i designerkommandofältet.)</span><span class="sxs-lookup"><span data-stu-id="3eb20-172">(On the designer command bar, choose **Save**.)</span></span>

6. <span data-ttu-id="3eb20-173">Du kan nu manuellt köra din logikapp för testning.</span><span class="sxs-lookup"><span data-stu-id="3eb20-173">You can now manually run your logic app for testing.</span></span> <span data-ttu-id="3eb20-174">Gå till designerkommandofältet och välj **Kör**.</span><span class="sxs-lookup"><span data-stu-id="3eb20-174">On the designer command bar, choose **Run**.</span></span> <span data-ttu-id="3eb20-175">Annars kan du låta din logikapp kontrollera det angivna RSS-flödet enligt det schema som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="3eb20-175">Otherwise, you can let your logic app check the specified RSS feed based on the schedule that you set up.</span></span>

   <span data-ttu-id="3eb20-176">Om logikappen hittar nya objekt skickar logikappen ett e-postmeddelande som innehåller dina valda data.</span><span class="sxs-lookup"><span data-stu-id="3eb20-176">If your logic app finds new items, the logic app sends email that includes your selected data.</span></span> 
   <span data-ttu-id="3eb20-177">Om inga nya objekt hittas hoppar logikappen över den åtgärd som skickar e-post.</span><span class="sxs-lookup"><span data-stu-id="3eb20-177">If no new items are found, your logic app skips the action that sends email.</span></span>

7. <span data-ttu-id="3eb20-178">Välj **Översikt** på logikappsmenyn om du vill övervaka och kontrollera din logikapps körnings- och utlösningshistorik.</span><span class="sxs-lookup"><span data-stu-id="3eb20-178">To monitor and check your logic app's run and trigger history, on your logic app menu, choose **Overview**.</span></span>

   ![Övervaka och visa logikappens körnings- och utlösningshistorik](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > <span data-ttu-id="3eb20-180">Om inte den information du förväntar dig visas kan du välja **Uppdatera** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="3eb20-180">If you don't find the data that you expect, on the command bar, try choosing **Refresh**.</span></span>

   <span data-ttu-id="3eb20-181">Läs mer om logikappens status eller körnings- och utlösningshistorik, eller diagnostisera logikappen i avsnittet [Troubleshoot your logic app (Felsöka logikappen)](logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="3eb20-181">To learn more about your logic app's status or run and trigger history, or to diagnose your logic app, see [Troubleshoot your logic app](logic-apps-diagnosing-failures.md).</span></span>

      > [!NOTE]
      > <span data-ttu-id="3eb20-182">Logikappen fortsätter att köra tills du stänger av den.</span><span class="sxs-lookup"><span data-stu-id="3eb20-182">Your logic app continues running until you turn off your app.</span></span> <span data-ttu-id="3eb20-183">Välj **Översikt** på logikappmenyn om du vill inaktivera din app tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="3eb20-183">To turn off your app for now, on your logic app menu, choose **Overview**.</span></span> <span data-ttu-id="3eb20-184">Välj **Inaktivera** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="3eb20-184">On the command bar, choose **Disable**.</span></span>

<span data-ttu-id="3eb20-185">Gratulerar! Du har precis konfigurerat och kört din första baslogikapp.</span><span class="sxs-lookup"><span data-stu-id="3eb20-185">Congratulations, you just set up and run your first basic logic app.</span></span> <span data-ttu-id="3eb20-186">Du fick också lära dig hur enkelt du kan skapa arbetsflöden för att automatisera processer och integrera molnappar och molntjänster – utan att skriva kod.</span><span class="sxs-lookup"><span data-stu-id="3eb20-186">You also learned how easily you can create workflows that automate processes, and integrate cloud apps and cloud services - all without code.</span></span>

## <a name="manage-your-logic-app"></a><span data-ttu-id="3eb20-187">Hantera din logikapp</span><span class="sxs-lookup"><span data-stu-id="3eb20-187">Manage your logic app</span></span>

<span data-ttu-id="3eb20-188">I din app kan du utföra uppgifter som att kontrollera status, redigera, visa historik, inaktivera eller ta bort din logikapp.</span><span class="sxs-lookup"><span data-stu-id="3eb20-188">To manage your app, you can perform tasks like check the status, edit, view history, turn off, or delete your logic app.</span></span>

1. <span data-ttu-id="3eb20-189">Logga in på [Azure Portal](https://portal.azure.com "Azure Portal").</span><span class="sxs-lookup"><span data-stu-id="3eb20-189">Sign in to the [Azure portal](https://portal.azure.com "Azure portal").</span></span>

2. <span data-ttu-id="3eb20-190">Klicka på **Fler tjänster** på den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="3eb20-190">On the left menu, choose **More services**.</span></span> <span data-ttu-id="3eb20-191">Välj **Logikappar** under **Enterprise-integration**.</span><span class="sxs-lookup"><span data-stu-id="3eb20-191">Under **Enterprise Integration**, choose **Logic Apps**.</span></span> <span data-ttu-id="3eb20-192">Välj din logikapp.</span><span class="sxs-lookup"><span data-stu-id="3eb20-192">Select your logic app.</span></span> 

   <span data-ttu-id="3eb20-193">Du kan hitta dessa uppgifter för hantering av logikappar i logikappsmenyn:</span><span class="sxs-lookup"><span data-stu-id="3eb20-193">In the logic app menu, you can find these logic app management tasks:</span></span>

   |<span data-ttu-id="3eb20-194">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="3eb20-194">Task</span></span>|<span data-ttu-id="3eb20-195">Steg</span><span class="sxs-lookup"><span data-stu-id="3eb20-195">Steps</span></span>| 
   |:---|:---| 
   | <span data-ttu-id="3eb20-196">Visa appens status, körningshistorik och allmän information</span><span class="sxs-lookup"><span data-stu-id="3eb20-196">View your app's status, execution history, and general information</span></span>| <span data-ttu-id="3eb20-197">Välj **Översikt**.</span><span class="sxs-lookup"><span data-stu-id="3eb20-197">Choose **Overview**.</span></span>| 
   | <span data-ttu-id="3eb20-198">Redigera din app</span><span class="sxs-lookup"><span data-stu-id="3eb20-198">Edit your app</span></span> | <span data-ttu-id="3eb20-199">Välj **Logic App Designer**.</span><span class="sxs-lookup"><span data-stu-id="3eb20-199">Choose **Logic App Designer**.</span></span> | 
   | <span data-ttu-id="3eb20-200">Visa arbetsflödet för JSON-definitionen för appen</span><span class="sxs-lookup"><span data-stu-id="3eb20-200">View your app's workflow JSON definition</span></span> | <span data-ttu-id="3eb20-201">Välj **Logikapp kodvy**.</span><span class="sxs-lookup"><span data-stu-id="3eb20-201">Choose **Logic App Code View**.</span></span> | 
   | <span data-ttu-id="3eb20-202">Visa åtgärder som utförs i logikappen</span><span class="sxs-lookup"><span data-stu-id="3eb20-202">View operations performed on your logic app</span></span> | <span data-ttu-id="3eb20-203">Välj **Aktivitetslogg**.</span><span class="sxs-lookup"><span data-stu-id="3eb20-203">Choose **Activity log**.</span></span> | 
   | <span data-ttu-id="3eb20-204">Visa tidigare versioner för din logikapp</span><span class="sxs-lookup"><span data-stu-id="3eb20-204">View past versions for your logic app</span></span> | <span data-ttu-id="3eb20-205">Välj **Versioner**.</span><span class="sxs-lookup"><span data-stu-id="3eb20-205">Choose **Versions**.</span></span> | 
   | <span data-ttu-id="3eb20-206">Inaktivera din app tillfälligt</span><span class="sxs-lookup"><span data-stu-id="3eb20-206">Turn off your app temporarily</span></span> | <span data-ttu-id="3eb20-207">Välj **Översikt** och sedan **Inaktivera** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="3eb20-207">Choose **Overview**, then on the command bar, choose **Disable**.</span></span> | 
   | <span data-ttu-id="3eb20-208">Ta bort din app</span><span class="sxs-lookup"><span data-stu-id="3eb20-208">Delete your app</span></span> | <span data-ttu-id="3eb20-209">Välj **Översikt** och sedan **Ta bort** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="3eb20-209">Choose **Overview**, then on the command bar, choose **Delete**.</span></span> <span data-ttu-id="3eb20-210">Ange logikappens namn och välj **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="3eb20-210">Enter your logic app's name, and choose **Delete**.</span></span> | 

## <a name="get-help"></a><span data-ttu-id="3eb20-211">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="3eb20-211">Get help</span></span>

<span data-ttu-id="3eb20-212">I [Azure Logic Apps-forumet](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) kan du ställa frågor, besvara frågor och se vad andra Azure Logic Apps-användare håller på med.</span><span class="sxs-lookup"><span data-stu-id="3eb20-212">To ask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="3eb20-213">På [webbplatsen för Azure Logic Apps-användarfeedback](http://aka.ms/logicapps-wish) kan du hjälpa till med att förbättra Azure Logic Apps och anslutningsapparna genom att rösta på förslag eller komma med egna förslag på förbättringar.</span><span class="sxs-lookup"><span data-stu-id="3eb20-213">To help improve Azure Logic Apps and connectors, vote on or submit ideas at the [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3eb20-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3eb20-214">Next steps</span></span>

*  [<span data-ttu-id="3eb20-215">Lägga till villkor och kör arbetsflöden</span><span class="sxs-lookup"><span data-stu-id="3eb20-215">Add conditions and run workflows</span></span>](../logic-apps/logic-apps-use-logic-app-features.md)
*    [<span data-ttu-id="3eb20-216">Mallar för logikappar</span><span class="sxs-lookup"><span data-stu-id="3eb20-216">Logic app templates</span></span>](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [<span data-ttu-id="3eb20-217">Skapa logikappar från Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="3eb20-217">Create logic apps from Azure Resource Manager templates</span></span>](../logic-apps/logic-apps-arm-provision.md)
