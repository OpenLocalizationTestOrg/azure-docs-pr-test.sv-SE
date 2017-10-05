---
title: "Testa din Data tjänsterbjudande på Marketplace | Microsoft Docs"
description: "Förstå hur du testar dina Data Service-erbjudande för Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e861bd11-f74d-4d77-b4b5-23fb463644ad
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 56a8aad7484fed18b74200ffa7acf22363625a15
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a><span data-ttu-id="f8b7d-103">Testa din Data tjänsterbjudande i Förproduktion</span><span class="sxs-lookup"><span data-stu-id="f8b7d-103">Testing your Data Service offer in Staging</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f8b7d-104">**Just nu vi inte längre onboarding några nya Data Service-utgivare. Nya dataservices kommer inte godkännas för lista.**</span><span class="sxs-lookup"><span data-stu-id="f8b7d-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="f8b7d-105">Om du har ett SaaS-affärsprogram som du vill publicera på AppSource hittar du mer information [här](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="f8b7d-105">If you have a SaaS business application you would like to publish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="f8b7d-106">Om du har en IaaS-program eller developer-tjänsten som du vill publicera på Azure Marketplace du hittar mer information [här](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="f8b7d-106">If you have an IaaS applications or developer service you would like to publish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="f8b7d-107">När du har slutfört de två första stegen av [skapar Microsoft Developer-konto](marketplace-publishing-accounts-creation-registration.md) och [skapar ditt Data Service erbjuder i Publishing Portal](marketplace-publishing-data-service-creation.md) är du redo för att göra erbjudandet tillgängliga i Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-107">After completing the first two steps of [Creating your Microsoft Developer account](marketplace-publishing-accounts-creation-registration.md) and [Creating your Data Service Offer in Publishing Portal](marketplace-publishing-data-service-creation.md) you’re ready for making your offer available in the Azure Marketplace.</span></span> <span data-ttu-id="f8b7d-108">Det här avsnittet vägleder dig genom de första, mellanliggande steg som kallas ”mellanlagring”</span><span class="sxs-lookup"><span data-stu-id="f8b7d-108">This topic will walk you through the first, intermediate, step called “Staging”</span></span>

<span data-ttu-id="f8b7d-109">Mellanlagring innebär distribuera erbjudandet i ett privat ”sandbox” där du kan testa och verifiera dess funktioner innan du skickar den till produktion.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-109">Staging means deploying your offer in a private "sandbox" where you can test and verify its functionality before pushing it to production.</span></span> <span data-ttu-id="f8b7d-110">Erbjudandet visas i Förproduktion precis som när en kund som har distribuerat den.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-110">The offer will appear in staging just as it would to a customer who has deployed it.</span></span>

## <a name="step-1-pushing-your-offer-to-staging"></a><span data-ttu-id="f8b7d-111">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-111">Step 1.</span></span> <span data-ttu-id="f8b7d-112">Push-överföring erbjudandet till Förproduktion</span><span class="sxs-lookup"><span data-stu-id="f8b7d-112">Pushing your offer to staging</span></span>
<span data-ttu-id="f8b7d-113">Push-överföring till Förproduktion erbjudandet kan du testa erbjudandet innan den blir tillgänglig för framtida prenumeranter.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-113">Pushing your offer to staging allows you to test the offer before it becomes available to future subscribers.</span></span>  <span data-ttu-id="f8b7d-114">Du kan se hur erbjudandet visas och fungerar för de prenumerera på dina data.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-114">You can see how your offer will appear and function for those subscribing to your data.</span></span>  

  ![Rita](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. <span data-ttu-id="f8b7d-116">Logga in på den [publicering av portalen](https://publish.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="f8b7d-116">Login into the [Publishing Portal](https://publish.windowsazure.com)</span></span>
2. <span data-ttu-id="f8b7d-117">Välj **datatjänster** i fönstret vänstra navigeringsfönstret</span><span class="sxs-lookup"><span data-stu-id="f8b7d-117">Select **Data Services** in the left navigation window</span></span>
3. <span data-ttu-id="f8b7d-118">Välj ditt erbjudande som du vill skicka till Förproduktion.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-118">Select your offer you want to push to staging.</span></span> <span data-ttu-id="f8b7d-119">Du ser skärmen ovan.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-119">You will see the above screen.</span></span>
4. <span data-ttu-id="f8b7d-120">Klicka på **Push till Förproduktion** knappen.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-120">Click **Push To Staging** button.</span></span>  
5. <span data-ttu-id="f8b7d-121">Om det finns problem med erbjudandet som behövs för att slutföras innan du sänder till mellanlagring, visas en lista över.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-121">If there are issues with the offer that needed to be completed prior to pushing to staging, you will see a list displayed.</span></span>  <span data-ttu-id="f8b7d-122">Korrigera objekten genom att klicka på varje objekt i listan.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-122">Correct these items by clicking on each item in the list.</span></span> <span data-ttu-id="f8b7d-123">När alla ändringar som gjorts, klickar på **Push till Förproduktion** igen.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-123">When all corrections made, click **Push to Staging** button again.</span></span>

<span data-ttu-id="f8b7d-124">Om det inte finns några problem med erbjudandet visas i popup-fönstret nedan.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-124">If there are no issues with your offer you will see the popup window below.</span></span>  

<span data-ttu-id="f8b7d-125">Om du är inte planera ej godkänt för att ansluta erbjudandet i Azure-portalen (för närvarande har begränsad kapacitet), bara stänga popupfönstret.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-125">If you’re not planning/not approved to surface your offer in Azure Portal (currently has limited capacity), then just close the pop-up window.</span></span>

<span data-ttu-id="f8b7d-126">Om du vill testa tjänsten Data i Azure-portalen (förutom DataMarket portal), behöver du ett Azure-prenumerationens ID att testa med.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-126">To test your Data Service in Azure Portal (in addition to the DataMarket portal), you will need an Azure Subscription ID to test with.</span></span>  <span data-ttu-id="f8b7d-127">Prenumerations-ID identifierar det konto som ska kunna testa erbjudandet.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-127">This Subscription ID will identify the account that will be allowed to test your offer.</span></span>  

<span data-ttu-id="f8b7d-128">Klipp ut och klistra in ditt prenumerations-ID och klicka på bockmarkeringen för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-128">Cut and paste your Subscription ID and click the checkmark to continue.</span></span>

  ![Rita](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> <span data-ttu-id="f8b7d-130">Dessa ID: N för Azure-prenumerationer är bara krävs för att testa och mellanlagring i den [Azure-hanteringsportalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="f8b7d-130">These Azure subscriptions IDs are only required for testing and staging in the [Azure Management Portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="f8b7d-131">De krävs inte för att testa i Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-131">They are not required to test in Azure Marketplace.</span></span>
> 
> 

<span data-ttu-id="f8b7d-132">Nästa skärm som visar att publicering sker genom att visa ”pågående”-ikonen visas markerad gul nedan.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-132">The next screen that appears shows that publishing is taking place by displaying the “In progress” icon highlighted yellow below.</span></span> <span data-ttu-id="f8b7d-133">Push-överföring till Förproduktion tar mellan 10 – 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-133">Pushing to staging takes between 10 to 15 minutes.</span></span>  <span data-ttu-id="f8b7d-134">Om det tar längre tid först uppdatera webbläsaren (tryck på F5 i IE).</span><span class="sxs-lookup"><span data-stu-id="f8b7d-134">If it takes longer, first refresh your browser (press F5 in IE).</span></span>  <span data-ttu-id="f8b7d-135">I sällsynta fall där erbjudandet fortfarande pushar till Förproduktion efter en timme, klickar du på länken Kontakta oss att berätta för oss att det finns ett problem.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-135">In the rare cases where your offer is still pushing to staging after an hour, click the contact us link to let us know that there is an issue.</span></span>

  ![Rita](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

<span data-ttu-id="f8b7d-137">När Push till Förproduktion har slutförts ”pågående”-ikonen ska stoppa flytta och status uppdateras till ”mellanlagrad”.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-137">When the Push to Staging completes the “In progress” icon will stop moving and the status will be updated to “Staged”.</span></span>  <span data-ttu-id="f8b7d-138">Du är nu redo att testa ditt erbjudande.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-138">You are now ready to test your offer.</span></span>  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a><span data-ttu-id="f8b7d-139">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-139">Step 2.</span></span> <span data-ttu-id="f8b7d-140">Testa mellanlagrade erbjudandet i DataMarket</span><span class="sxs-lookup"><span data-stu-id="f8b7d-140">Test your staged offer in DataMarket</span></span>
<span data-ttu-id="f8b7d-141">Klickar du på följande texten **”se din tjänst som erbjuder i...”**</span><span class="sxs-lookup"><span data-stu-id="f8b7d-141">Click the link following the text **“See Your service offer at…”**</span></span> <span data-ttu-id="f8b7d-142">på skärmen att prenumeranten kommer att se när erbjudandet går till produktion och visas i DataMarket.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-142">to display the screen that the subscriber will see when your offer goes to production and will appear in DataMarket.</span></span>

  ![Rita](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

<span data-ttu-id="f8b7d-144">Testa eller verifiera 12 objekten markerad ovan för alla logotyper, priser-transaktioner, text, bilder, dokumentation och länkar är korrekta och fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-144">Test or verify each of the 12 items marked above to ensure all logos, prices/transactions, text, images, documentation, and links are correct and working properly.</span></span>  <span data-ttu-id="f8b7d-145">Det här är ett bra tillfälle att se till att alla testvärden som du angav när du skapar din prenumeration har ersatts med faktiska värden.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-145">This is a good time to ensure any test values you entered when creating your offer have been replaced with actual values.</span></span>

1. <span data-ttu-id="f8b7d-146">Logotyp för erbjudande</span><span class="sxs-lookup"><span data-stu-id="f8b7d-146">Offer logo</span></span>
2. <span data-ttu-id="f8b7d-147">Erbjudandenamn</span><span class="sxs-lookup"><span data-stu-id="f8b7d-147">Offer name</span></span>
3. <span data-ttu-id="f8b7d-148">Utgivarens namn/länk till ditt företags webbplats</span><span class="sxs-lookup"><span data-stu-id="f8b7d-148">Publisher name/link to your company's website</span></span>
4. <span data-ttu-id="f8b7d-149">Sök efter kategorier för ditt erbjudande</span><span class="sxs-lookup"><span data-stu-id="f8b7d-149">Search categories for your offer</span></span>
5. <span data-ttu-id="f8b7d-150">Länk till ditt erbjudande support att hjälpa prenumeranter</span><span class="sxs-lookup"><span data-stu-id="f8b7d-150">Your offer's support link to assist subscribers</span></span>
6. <span data-ttu-id="f8b7d-151">Kontextuella beskrivning för ditt erbjudande</span><span class="sxs-lookup"><span data-stu-id="f8b7d-151">Contextual description for your offer</span></span>
7. <span data-ttu-id="f8b7d-152">Erbjudande plan illustrerar faktureringsinformation</span><span class="sxs-lookup"><span data-stu-id="f8b7d-152">Offer plan depicting billing details</span></span>
8. <span data-ttu-id="f8b7d-153">Länka till implementeringskod</span><span class="sxs-lookup"><span data-stu-id="f8b7d-153">Link to implementation code</span></span>
9. <span data-ttu-id="f8b7d-154">Exempel bilder som illustrerar användning av erbjudandet data</span><span class="sxs-lookup"><span data-stu-id="f8b7d-154">Sample images that illustrate use of offer data</span></span>
10. <span data-ttu-id="f8b7d-155">I/o-metadata för varje tjänst i erbjudandet</span><span class="sxs-lookup"><span data-stu-id="f8b7d-155">Input/Output metadata for each service within the offer</span></span>
11. <span data-ttu-id="f8b7d-156">Erbjudandet, villkor för användning</span><span class="sxs-lookup"><span data-stu-id="f8b7d-156">Offer's Terms of Use</span></span>
12. <span data-ttu-id="f8b7d-157">Förhandsgranskning av det erbjudande data</span><span class="sxs-lookup"><span data-stu-id="f8b7d-157">Preview of the offer's data</span></span>

<span data-ttu-id="f8b7d-158">Kontrollera slutligen tjänsten fungerar via Datamarket genom att klicka på länken ”Utforska den här DATAUPPSÄTTNINGEN”.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-158">Finally, check the service will work through the Datamarket by clicking the link “EXPLORE THIS DATASET”.</span></span>  <span data-ttu-id="f8b7d-159">Öppnar ett nytt fönster i verktyget vi kallar ”Service Explorer” så att du kan förhandsgranska resultaten av en fråga mot din tjänst.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-159">A new window will open in the tool we call “Service Explorer” so you can preview the results of a query against your service.</span></span>  <span data-ttu-id="f8b7d-160">I det här fönstret kan du ange parametrar som behövs och se resultaten från en fråga till tjänsten visas.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-160">In this window, you can enter the parameters needed and see the results displayed from a query against your service.</span></span>   <span data-ttu-id="f8b7d-161">Dessutom är visas URL-Adressen för din fråga.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-161">Also, displayed is the URL for your Query.</span></span>  

> [!NOTE]
> <span data-ttu-id="f8b7d-162">Se till att granska textbeskrivning av tjänsten visas längst upp.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-162">Be sure to review the textual description of the service displayed at the top.</span></span>  <span data-ttu-id="f8b7d-163">Och om erbjudandet består av fler än en tjänst anropa, klicka på flikarna längst ned för att växla till nästa service för att granska och testa.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-163">And if your offer consists of more than one service call, click the tabs at the bottom to switch to the next service to review and test.</span></span>
> 
> 

## <a name="next-step"></a><span data-ttu-id="f8b7d-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f8b7d-164">Next step</span></span>
<span data-ttu-id="f8b7d-165">Om du har problem och behöver hjälp med att lösa dem. Kontakta [Azure utgivarens Support](http://go.microsoft.com/fwlink/?LinkId=272975).</span><span class="sxs-lookup"><span data-stu-id="f8b7d-165">If you are having issues and need help resolving them please contact [Azure Publisher Support](http://go.microsoft.com/fwlink/?LinkId=272975).</span></span>

<span data-ttu-id="f8b7d-166">Om du är nöjd och redo att publicera erbjudandet Läs den [begäran om godkännande Push till produktion](marketplace-publishing-push-to-production.md) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f8b7d-166">If you are satisfied and ready to publish your offer please read the [Request Approval to Push To Production](marketplace-publishing-push-to-production.md) documentation.</span></span>

## <a name="see-also"></a><span data-ttu-id="f8b7d-167">Se även</span><span class="sxs-lookup"><span data-stu-id="f8b7d-167">See Also</span></span>
* [<span data-ttu-id="f8b7d-168">Komma igång: Hur du publicerar ett erbjudande på Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="f8b7d-168">Getting Started: How to publish an offer to the Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

