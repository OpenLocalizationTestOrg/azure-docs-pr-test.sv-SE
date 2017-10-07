---
title: "aaaTesting Data Service-erbjudande för hello Marketplace | Microsoft Docs"
description: "Förstå hur tootest Data Service-erbjudande för hello Azure Marketplace."
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
ms.openlocfilehash: 1b9c7027d8e0818b9bdee5cfca971bab25dd1959
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a><span data-ttu-id="eae56-103">Testa din Data tjänsterbjudande i Förproduktion</span><span class="sxs-lookup"><span data-stu-id="eae56-103">Testing your Data Service offer in Staging</span></span>
> [!IMPORTANT]
> <span data-ttu-id="eae56-104">**Just nu vi inte längre onboarding några nya Data Service-utgivare. Nya dataservices kommer inte godkännas för lista.**</span><span class="sxs-lookup"><span data-stu-id="eae56-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="eae56-105">Om du har ett SaaS-affärsprogram som toopublish på AppSource hittar du mer information [här](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="eae56-105">If you have a SaaS business application you would like toopublish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="eae56-106">Om du har en IaaS-program eller utvecklare tjänst du skulle t.ex toopublish på Azure Marketplace hittar du mer information [här](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="eae56-106">If you have an IaaS applications or developer service you would like toopublish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="eae56-107">När du har slutfört hello två första stegen av [skapar Microsoft Developer-konto](marketplace-publishing-accounts-creation-registration.md) och [skapar ditt Data Service erbjuder i Publishing Portal](marketplace-publishing-data-service-creation.md) är du redo för att göra erbjudandet tillgängliga i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="eae56-107">After completing hello first two steps of [Creating your Microsoft Developer account](marketplace-publishing-accounts-creation-registration.md) and [Creating your Data Service Offer in Publishing Portal](marketplace-publishing-data-service-creation.md) you’re ready for making your offer available in hello Azure Marketplace.</span></span> <span data-ttu-id="eae56-108">Det här avsnittet kommer vägleder dig genom hello först, mellanliggande, steg kallas ”mellanlagring”</span><span class="sxs-lookup"><span data-stu-id="eae56-108">This topic will walk you through hello first, intermediate, step called “Staging”</span></span>

<span data-ttu-id="eae56-109">Mellanlagring sätt distribuera erbjudandet i ett privat ”sandbox” där du kan testa och verifiera dess funktioner innan du skickar den tooproduction.</span><span class="sxs-lookup"><span data-stu-id="eae56-109">Staging means deploying your offer in a private "sandbox" where you can test and verify its functionality before pushing it tooproduction.</span></span> <span data-ttu-id="eae56-110">hello erbjudandet visas i Förproduktion precis som tooa kund som har distribuerat den.</span><span class="sxs-lookup"><span data-stu-id="eae56-110">hello offer will appear in staging just as it would tooa customer who has deployed it.</span></span>

## <a name="step-1-pushing-your-offer-toostaging"></a><span data-ttu-id="eae56-111">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="eae56-111">Step 1.</span></span> <span data-ttu-id="eae56-112">Push-överföring erbjudande-toostaging</span><span class="sxs-lookup"><span data-stu-id="eae56-112">Pushing your offer toostaging</span></span>
<span data-ttu-id="eae56-113">Push-överföring erbjudande-toostaging kan du tootest hello erbjudande innan den blir tillgänglig toofuture prenumeranter.</span><span class="sxs-lookup"><span data-stu-id="eae56-113">Pushing your offer toostaging allows you tootest hello offer before it becomes available toofuture subscribers.</span></span>  <span data-ttu-id="eae56-114">Du kan se hur erbjudandet visas och fungerar för de prenumerera tooyour data.</span><span class="sxs-lookup"><span data-stu-id="eae56-114">You can see how your offer will appear and function for those subscribing tooyour data.</span></span>  

  ![Rita](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. <span data-ttu-id="eae56-116">Logga in på hello [Publishing Portal](https://publish.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="eae56-116">Login into hello [Publishing Portal](https://publish.windowsazure.com)</span></span>
2. <span data-ttu-id="eae56-117">Välj **datatjänster** i hello vänstra navigeringsfönstret fönster</span><span class="sxs-lookup"><span data-stu-id="eae56-117">Select **Data Services** in hello left navigation window</span></span>
3. <span data-ttu-id="eae56-118">Välj önskade toopush toostaging erbjudandet.</span><span class="sxs-lookup"><span data-stu-id="eae56-118">Select your offer you want toopush toostaging.</span></span> <span data-ttu-id="eae56-119">Hej ovanför skärmen visas.</span><span class="sxs-lookup"><span data-stu-id="eae56-119">You will see hello above screen.</span></span>
4. <span data-ttu-id="eae56-120">Klicka på **Push tooStaging** knappen.</span><span class="sxs-lookup"><span data-stu-id="eae56-120">Click **Push tooStaging** button.</span></span>  
5. <span data-ttu-id="eae56-121">Om det finns problem med hello erbjudande som behövs toobe slutfört tidigare toopushing toostaging, visas en lista som visas.</span><span class="sxs-lookup"><span data-stu-id="eae56-121">If there are issues with hello offer that needed toobe completed prior toopushing toostaging, you will see a list displayed.</span></span>  <span data-ttu-id="eae56-122">Korrigera objekten genom att klicka på varje objekt i listan hello.</span><span class="sxs-lookup"><span data-stu-id="eae56-122">Correct these items by clicking on each item in hello list.</span></span> <span data-ttu-id="eae56-123">När alla ändringar som gjorts, klickar på **Push tooStaging** igen.</span><span class="sxs-lookup"><span data-stu-id="eae56-123">When all corrections made, click **Push tooStaging** button again.</span></span>

<span data-ttu-id="eae56-124">Om det inte finns några problem med erbjudandet visas hello popup-fönstret nedan.</span><span class="sxs-lookup"><span data-stu-id="eae56-124">If there are no issues with your offer you will see hello popup window below.</span></span>  

<span data-ttu-id="eae56-125">Om du inte planerar ej godkända toosurface erbjudandet i Azure-portalen (för närvarande har begränsad kapacitet), och sedan bara Stäng hello popup-fönster.</span><span class="sxs-lookup"><span data-stu-id="eae56-125">If you’re not planning/not approved toosurface your offer in Azure Portal (currently has limited capacity), then just close hello pop-up window.</span></span>

<span data-ttu-id="eae56-126">tootest dina Data i Azure Portal-tjänsten (i tillägg toohello DataMarket portal), behöver du ett Azure-prenumerationens ID tootest med.</span><span class="sxs-lookup"><span data-stu-id="eae56-126">tootest your Data Service in Azure Portal (in addition toohello DataMarket portal), you will need an Azure Subscription ID tootest with.</span></span>  <span data-ttu-id="eae56-127">Prenumerations-ID identifierar hello-konto som ska vara tillåtna tootest erbjudandet.</span><span class="sxs-lookup"><span data-stu-id="eae56-127">This Subscription ID will identify hello account that will be allowed tootest your offer.</span></span>  

<span data-ttu-id="eae56-128">Klipp ut och klistra in ditt prenumerations-ID och klicka på hello markering toocontinue.</span><span class="sxs-lookup"><span data-stu-id="eae56-128">Cut and paste your Subscription ID and click hello checkmark toocontinue.</span></span>

  ![Rita](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> <span data-ttu-id="eae56-130">Dessa ID: N för Azure-prenumerationer är bara krävs för att testa och mellanlagring i hello [Azure-hanteringsportalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="eae56-130">These Azure subscriptions IDs are only required for testing and staging in hello [Azure Management Portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="eae56-131">De är inte obligatoriska tootest i Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="eae56-131">They are not required tootest in Azure Marketplace.</span></span>
> 
> 

<span data-ttu-id="eae56-132">hello nästa skärm visas visar att publicering sker genom att visa hello ”pågående” ikonen markerat gul nedan.</span><span class="sxs-lookup"><span data-stu-id="eae56-132">hello next screen that appears shows that publishing is taking place by displaying hello “In progress” icon highlighted yellow below.</span></span> <span data-ttu-id="eae56-133">Push-överföring toostaging tar mellan 10 too15 minuter.</span><span class="sxs-lookup"><span data-stu-id="eae56-133">Pushing toostaging takes between 10 too15 minutes.</span></span>  <span data-ttu-id="eae56-134">Om det tar längre tid först uppdatera webbläsaren (tryck på F5 i IE).</span><span class="sxs-lookup"><span data-stu-id="eae56-134">If it takes longer, first refresh your browser (press F5 in IE).</span></span>  <span data-ttu-id="eae56-135">Klicka på hello länken Kontakta oss toolet oss om att det finns ett problem i hello sällsynta fall där erbjudandet är fortfarande pushar toostaging efter en timme.</span><span class="sxs-lookup"><span data-stu-id="eae56-135">In hello rare cases where your offer is still pushing toostaging after an hour, click hello contact us link toolet us know that there is an issue.</span></span>

  ![Rita](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

<span data-ttu-id="eae56-137">När hello Push tooStaging slutförs hello ”pågående” ikonen slutar att flytta och hello status uppdateras för ”mellanlagrad”.</span><span class="sxs-lookup"><span data-stu-id="eae56-137">When hello Push tooStaging completes hello “In progress” icon will stop moving and hello status will be updated too“Staged”.</span></span>  <span data-ttu-id="eae56-138">Du är nu redo tootest erbjudandet.</span><span class="sxs-lookup"><span data-stu-id="eae56-138">You are now ready tootest your offer.</span></span>  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a><span data-ttu-id="eae56-139">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="eae56-139">Step 2.</span></span> <span data-ttu-id="eae56-140">Testa mellanlagrade erbjudandet i DataMarket</span><span class="sxs-lookup"><span data-stu-id="eae56-140">Test your staged offer in DataMarket</span></span>
<span data-ttu-id="eae56-141">Klicka på länken för hello efter hello texten **”se din tjänst som erbjuder på...”**</span><span class="sxs-lookup"><span data-stu-id="eae56-141">Click hello link following hello text **“See Your service offer at…”**</span></span> <span data-ttu-id="eae56-142">toodisplay hello-skärmen som hello prenumeranten kommer att se när erbjudandet går tooproduction och visas i DataMarket.</span><span class="sxs-lookup"><span data-stu-id="eae56-142">toodisplay hello screen that hello subscriber will see when your offer goes tooproduction and will appear in DataMarket.</span></span>

  ![Rita](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

<span data-ttu-id="eae56-144">Testa eller kontrollera var och en av hello 12 objekt markeras ovan tooensure alla logotyper, priser-transaktioner, text, bilder, dokumentation och länkar är korrekta och fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="eae56-144">Test or verify each of hello 12 items marked above tooensure all logos, prices/transactions, text, images, documentation, and links are correct and working properly.</span></span>  <span data-ttu-id="eae56-145">Det här är ett bra tillfälle tooensure alla testvärden som du angav när du skapar din prenumeration har ersatts med faktiska värden.</span><span class="sxs-lookup"><span data-stu-id="eae56-145">This is a good time tooensure any test values you entered when creating your offer have been replaced with actual values.</span></span>

1. <span data-ttu-id="eae56-146">Logotyp för erbjudande</span><span class="sxs-lookup"><span data-stu-id="eae56-146">Offer logo</span></span>
2. <span data-ttu-id="eae56-147">Erbjudandenamn</span><span class="sxs-lookup"><span data-stu-id="eae56-147">Offer name</span></span>
3. <span data-ttu-id="eae56-148">Utgivarens namn/länk tooyour företags webbplats</span><span class="sxs-lookup"><span data-stu-id="eae56-148">Publisher name/link tooyour company's website</span></span>
4. <span data-ttu-id="eae56-149">Sök efter kategorier för ditt erbjudande</span><span class="sxs-lookup"><span data-stu-id="eae56-149">Search categories for your offer</span></span>
5. <span data-ttu-id="eae56-150">Ditt erbjudande stöd länken tooassist prenumeranter</span><span class="sxs-lookup"><span data-stu-id="eae56-150">Your offer's support link tooassist subscribers</span></span>
6. <span data-ttu-id="eae56-151">Kontextuella beskrivning för ditt erbjudande</span><span class="sxs-lookup"><span data-stu-id="eae56-151">Contextual description for your offer</span></span>
7. <span data-ttu-id="eae56-152">Erbjudande plan illustrerar faktureringsinformation</span><span class="sxs-lookup"><span data-stu-id="eae56-152">Offer plan depicting billing details</span></span>
8. <span data-ttu-id="eae56-153">Tooimplementation kod</span><span class="sxs-lookup"><span data-stu-id="eae56-153">Link tooimplementation code</span></span>
9. <span data-ttu-id="eae56-154">Exempel bilder som illustrerar användning av erbjudandet data</span><span class="sxs-lookup"><span data-stu-id="eae56-154">Sample images that illustrate use of offer data</span></span>
10. <span data-ttu-id="eae56-155">I/o-metadata för varje tjänst inom hello erbjudande</span><span class="sxs-lookup"><span data-stu-id="eae56-155">Input/Output metadata for each service within hello offer</span></span>
11. <span data-ttu-id="eae56-156">Erbjudandet, villkor för användning</span><span class="sxs-lookup"><span data-stu-id="eae56-156">Offer's Terms of Use</span></span>
12. <span data-ttu-id="eae56-157">Förhandsgranskning av hello erbjudande data</span><span class="sxs-lookup"><span data-stu-id="eae56-157">Preview of hello offer's data</span></span>

<span data-ttu-id="eae56-158">Kontrollera slutligen hello-tjänsten fungerar via hello Datamarket genom att klicka på hello länken ”Utforska den här DATAUPPSÄTTNINGEN”.</span><span class="sxs-lookup"><span data-stu-id="eae56-158">Finally, check hello service will work through hello Datamarket by clicking hello link “EXPLORE THIS DATASET”.</span></span>  <span data-ttu-id="eae56-159">Öppnar ett nytt fönster i hello verktyget vi kallar ”Service Explorer” så att du kan förhandsgranska hello resultatet av en fråga mot din tjänst.</span><span class="sxs-lookup"><span data-stu-id="eae56-159">A new window will open in hello tool we call “Service Explorer” so you can preview hello results of a query against your service.</span></span>  <span data-ttu-id="eae56-160">I det här fönstret kan ange du hello parametrar som behövs och se hello resultat från en fråga till tjänsten visas.</span><span class="sxs-lookup"><span data-stu-id="eae56-160">In this window, you can enter hello parameters needed and see hello results displayed from a query against your service.</span></span>   <span data-ttu-id="eae56-161">Dessutom är visas hello URL för din fråga.</span><span class="sxs-lookup"><span data-stu-id="eae56-161">Also, displayed is hello URL for your Query.</span></span>  

> [!NOTE]
> <span data-ttu-id="eae56-162">Vara säker på att tooreview hello textbeskrivning av hello tjänsten visas överst hello.</span><span class="sxs-lookup"><span data-stu-id="eae56-162">Be sure tooreview hello textual description of hello service displayed at hello top.</span></span>  <span data-ttu-id="eae56-163">Och om erbjudandet består av fler än en tjänst anropa på hello flikar på hello nedre tooswitch toohello nästa service tooreview och testa.</span><span class="sxs-lookup"><span data-stu-id="eae56-163">And if your offer consists of more than one service call, click hello tabs at hello bottom tooswitch toohello next service tooreview and test.</span></span>
> 
> 

## <a name="next-step"></a><span data-ttu-id="eae56-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eae56-164">Next step</span></span>
<span data-ttu-id="eae56-165">Om du har problem och behöver hjälp med att lösa dem. Kontakta [Azure utgivarens Support](http://go.microsoft.com/fwlink/?LinkId=272975).</span><span class="sxs-lookup"><span data-stu-id="eae56-165">If you are having issues and need help resolving them please contact [Azure Publisher Support](http://go.microsoft.com/fwlink/?LinkId=272975).</span></span>

<span data-ttu-id="eae56-166">Om du är nöjd och redo toopublish erbjudandet Läs hello [begäran om godkännande av tooPush tooProduction](marketplace-publishing-push-to-production.md) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="eae56-166">If you are satisfied and ready toopublish your offer please read hello [Request Approval tooPush tooProduction](marketplace-publishing-push-to-production.md) documentation.</span></span>

## <a name="see-also"></a><span data-ttu-id="eae56-167">Se även</span><span class="sxs-lookup"><span data-stu-id="eae56-167">See Also</span></span>
* [<span data-ttu-id="eae56-168">Komma igång: Hur toopublish ett erbjudande toohello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="eae56-168">Getting Started: How toopublish an offer toohello Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

