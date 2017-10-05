---
title: Skapa en WordPress-webbapp i Azure App Service | Microsoft Docs
description: "Lär dig hur du skapar en ny Azure-webbapp för en WordPress-blogg i Azure Portal."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 193ae094-0d7c-4749-a09b-ff4b1240149e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 460afdabed947fb4018a9ea8a7a5bc7dc5bc89c7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-wordpress-web-app-in-azure-app-service"></a><span data-ttu-id="47f4f-103">Skapa en WordPress-webbapp i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="47f4f-103">Create a WordPress web app in Azure App Service</span></span>
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

<span data-ttu-id="47f4f-104">I den här kursen får du veta hur du distribuerar en WordPress-bloggwebbplats från Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="47f4f-104">This tutorial shows how to deploy a WordPress blog site from the Azure Marketplace.</span></span>

<span data-ttu-id="47f4f-105">När du har gått igenom kursen har du en egen WordPress-bloggwebbplats som körs i molnet.</span><span class="sxs-lookup"><span data-stu-id="47f4f-105">When you're done with the tutorial you'll have your own WordPress blog site up and running in the cloud.</span></span>

![WordPress-webbplats](./media/web-sites-php-web-site-gallery/wpdashboard.png)

<span data-ttu-id="47f4f-107">Du får lära dig att:</span><span class="sxs-lookup"><span data-stu-id="47f4f-107">You'll learn:</span></span>

* <span data-ttu-id="47f4f-108">hitta en programmall i Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="47f4f-108">How to find an application template in the Azure Marketplace.</span></span>
* <span data-ttu-id="47f4f-109">skapa en webbapp i Azure App Service som baseras på mallen.</span><span class="sxs-lookup"><span data-stu-id="47f4f-109">How to create a web app in Azure App Service that is based on the template.</span></span>
* <span data-ttu-id="47f4f-110">konfigurera inställningarna för Azure App Service för nya webbappar och databaser.</span><span class="sxs-lookup"><span data-stu-id="47f4f-110">How to configure Azure App Service settings for the new web app and database.</span></span>

<span data-ttu-id="47f4f-111">Via Azure Marketplace får du tillgång till en mängd olika populära webbappar som har utvecklats av Microsoft och tredje part samt programvara med öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="47f4f-111">The Azure Marketplace makes available a wide range of popular web apps developed by Microsoft, third party companies, and open source software initiatives.</span></span> <span data-ttu-id="47f4f-112">Webbapparna baseras på en mängd olika populära ramverk, bland annat [PHP](/develop/nodejs/), som i det här WordPress-exemplet, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/) och [Python](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="47f4f-112">The web apps are built on a wide range of popular frameworks, such as [PHP](/develop/nodejs/) in this WordPress example, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), and [Python](/develop/python/), to name a few.</span></span> <span data-ttu-id="47f4f-113">När du skapar en webbapp från Azure Marketplace är den enda programvara du behöver den webbläsare du använder för [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="47f4f-113">To create a web app from the Azure Marketplace the only software you need is the browser that you use for the [Azure Portal](https://portal.azure.com/).</span></span> 

<span data-ttu-id="47f4f-114">För WordPress-webbplatsen du distribuerar under den här kursen används MySQL som databas.</span><span class="sxs-lookup"><span data-stu-id="47f4f-114">The WordPress site that you deploy in this tutorial uses MySQL for the database.</span></span> <span data-ttu-id="47f4f-115">Se [Project Nami](http://projectnami.org/) om du hellre vill använda SQL Database som databas.</span><span class="sxs-lookup"><span data-stu-id="47f4f-115">If you wish to instead use SQL Database for the database, see [Project Nami](http://projectnami.org/).</span></span> <span data-ttu-id="47f4f-116">**Project Nami** också är tillgängligt via Marketplace.</span><span class="sxs-lookup"><span data-stu-id="47f4f-116">**Project Nami** is also available through the Marketplace.</span></span>

> [!NOTE]
> <span data-ttu-id="47f4f-117">För den här kursen behöver du ett Microsoft Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="47f4f-117">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="47f4f-118">Om du inte har ett konto kan du [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) eller [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="47f4f-118">If you don't have an account, you can [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
> 
> <span data-ttu-id="47f4f-119">Om du vill komma igång med Azure App Service innan du registrerar dig för ett Azure-konto kan du gå till [Prova App Service](https://azure.microsoft.com/try/app-service/).</span><span class="sxs-lookup"><span data-stu-id="47f4f-119">If you want to get started with Azure App Service before you sign up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/).</span></span> <span data-ttu-id="47f4f-120">Där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort behövs och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="47f4f-120">There, you can immediately create a short-lived starter web app in App Service—no credit card required, and no commitments.</span></span>
> 
> 

## <a name="select-wordpress-and-configure-for-azure-app-service"></a><span data-ttu-id="47f4f-121">Välj WordPress och konfigurera för Azure App Service</span><span class="sxs-lookup"><span data-stu-id="47f4f-121">Select WordPress and configure for Azure App Service</span></span>
1. <span data-ttu-id="47f4f-122">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="47f4f-122">Log in to the [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="47f4f-123">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="47f4f-123">Click **New**.</span></span>
   
    ![Skapa Ny][5]
3. <span data-ttu-id="47f4f-125">Sök efter **WordPress** och klicka sedan på **WordPress**.</span><span class="sxs-lookup"><span data-stu-id="47f4f-125">Search for **WordPress**, and then click **WordPress**.</span></span> <span data-ttu-id="47f4f-126">Om du vill använda SQL Database i stället för MySQL söker du efter **Project Nami**.</span><span class="sxs-lookup"><span data-stu-id="47f4f-126">If you wish to use SQL Database instead of MySQL, search for **Project Nami**.</span></span>
   
    ![WordPress från lista][7]
4. <span data-ttu-id="47f4f-128">När du har läst igenom beskrivningen av WordPress-appen klickar du på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="47f4f-128">After reading the description of the WordPress app, click **Create**.</span></span>
   
    ![Skapa](./media/web-sites-php-web-site-gallery/create.png)
5. <span data-ttu-id="47f4f-130">Ange ett namn på webbappen i rutan **Webbapp**.</span><span class="sxs-lookup"><span data-stu-id="47f4f-130">Enter a name for the web app in the **Web app** box.</span></span>
   
    <span data-ttu-id="47f4f-131">Det här namnet måste vara unikt i domänen azurewebsites.net eftersom webbappens webbadress är {namn}.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="47f4f-131">This name must be unique in the azurewebsites.net domain because the URL of the web app will be {name}.azurewebsites.net.</span></span> <span data-ttu-id="47f4f-132">Om namnet inte är unikt visas ett rött utropstecken i textrutan.</span><span class="sxs-lookup"><span data-stu-id="47f4f-132">If the name you enter isn't unique, a red exclamation mark appears in the text box.</span></span>
6. <span data-ttu-id="47f4f-133">Om du har fler än en prenumeration väljer du den du vill använda.</span><span class="sxs-lookup"><span data-stu-id="47f4f-133">If you have more than one subscription, choose the one you want to use.</span></span> 
7. <span data-ttu-id="47f4f-134">Välj en **Resursgrupp** eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="47f4f-134">Select a **Resource Group** or create a new one.</span></span>
   
    <span data-ttu-id="47f4f-135">Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="47f4f-135">For more information about resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
8. <span data-ttu-id="47f4f-136">Välj en **App Service-plan/plats** eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="47f4f-136">Select an **App Service plan/Location** or create a new one.</span></span>
   
    <span data-ttu-id="47f4f-137">Mer information om App Service-planer finns i [Översikt över Azure App Service-planer](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</span><span class="sxs-lookup"><span data-stu-id="47f4f-137">For more information about App Service plans, see [Azure App Service plans overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</span></span>    
9. <span data-ttu-id="47f4f-138">Klicka på **Databas** och ange sedan i bladet **Ny MySQL-databas** de värden som krävs för att konfigurera MySQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="47f4f-138">Click **Database**, and then in the **New MySQL Database** blade provide the required values for configuring your MySQL database.</span></span>
   
    <span data-ttu-id="47f4f-139">a.</span><span class="sxs-lookup"><span data-stu-id="47f4f-139">a.</span></span> <span data-ttu-id="47f4f-140">Ange ett nytt namn eller låt standardnamnet stå kvar.</span><span class="sxs-lookup"><span data-stu-id="47f4f-140">Enter a new name or leave the default name.</span></span>
   
    <span data-ttu-id="47f4f-141">b.</span><span class="sxs-lookup"><span data-stu-id="47f4f-141">b.</span></span> <span data-ttu-id="47f4f-142">Lämna **Databastyp** inställt på **Delad**.</span><span class="sxs-lookup"><span data-stu-id="47f4f-142">Leave the **Database Type** set to **Shared**.</span></span>
   
    <span data-ttu-id="47f4f-143">c.</span><span class="sxs-lookup"><span data-stu-id="47f4f-143">c.</span></span> <span data-ttu-id="47f4f-144">Välj samma plats som du valde för webbappen.</span><span class="sxs-lookup"><span data-stu-id="47f4f-144">Choose the same location as the one you chose for the web app.</span></span>
   
    <span data-ttu-id="47f4f-145">d.</span><span class="sxs-lookup"><span data-stu-id="47f4f-145">d.</span></span> <span data-ttu-id="47f4f-146">Välj en prisnivå.</span><span class="sxs-lookup"><span data-stu-id="47f4f-146">Choose a pricing tier.</span></span> <span data-ttu-id="47f4f-147">För kursen räcker det bra med Mercury (kostnadsfri med det lägsta antalet tillåtna anslutningar och lägst mängd diskutrymme).</span><span class="sxs-lookup"><span data-stu-id="47f4f-147">Mercury (free with minimal allowed connections and disk space) is fine for this tutorial.</span></span>
10. <span data-ttu-id="47f4f-148">I bladet **Ny MySQL-databas** klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="47f4f-148">In the **New MySQL Database** blade, click **OK**.</span></span> 
11. <span data-ttu-id="47f4f-149">I bladet **WordPress** godkänner du de juridiska villkoren och klickar på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="47f4f-149">In the **WordPress** blade, accept the legal terms, and then click **Create**.</span></span> 
    
     ![Konfigurera webbappen](./media/web-sites-php-web-site-gallery/configure.png)
    
     <span data-ttu-id="47f4f-151">Azure App Service skapar den nya webbappen. Det går vanligen på mindre än en minut.</span><span class="sxs-lookup"><span data-stu-id="47f4f-151">Azure App Service creates the web app, typically in less than a minute.</span></span> <span data-ttu-id="47f4f-152">Du kan se förloppet genom att klicka på klockikonen överst på portalsidan.</span><span class="sxs-lookup"><span data-stu-id="47f4f-152">You can watch the progress by clicking the bell icon at the top of the portal page.</span></span>
    
     ![Förloppsindikator](./media/web-sites-php-web-site-gallery/progress.png)

## <a name="launch-and-manage-your-wordpress-web-app"></a><span data-ttu-id="47f4f-154">Starta och hantera WordPress-webbappen</span><span class="sxs-lookup"><span data-stu-id="47f4f-154">Launch and manage your WordPress web app</span></span>
1. <span data-ttu-id="47f4f-155">När webbappen har skapats kan du visa webbappen och databasen genom att gå till den resursgrupp i Azure Portal där du skapade programmet.</span><span class="sxs-lookup"><span data-stu-id="47f4f-155">When the web app creation is finished, navigate in the Azure Portal to the resource group in which you created the application, and you can see the web app and the database.</span></span>
   
    <span data-ttu-id="47f4f-156">Extraresursen med lampikonen är [Application Insights](/services/application-insights/) som tillhandahåller övervakningstjänster för webbappen.</span><span class="sxs-lookup"><span data-stu-id="47f4f-156">The extra resource with the light bulb icon is [Application Insights](/services/application-insights/), which provides monitoring services for your web app.</span></span>
2. <span data-ttu-id="47f4f-157">I bladet **Resursgrupp** klickar du på raden webbapp.</span><span class="sxs-lookup"><span data-stu-id="47f4f-157">In the **Resource group** blade, click the web app line.</span></span>
   
    ![Konfigurera webbappen](./media/web-sites-php-web-site-gallery/resourcegroup.png)
3. <span data-ttu-id="47f4f-159">I bladet Webbapp klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="47f4f-159">In the Web app blade, click **Browse**.</span></span>
   
    ![webbadress][browse]
4. <span data-ttu-id="47f4f-161">På WordPress sida **Välkommen** anger du de konfigurationsuppgifter som krävs av WordPress och klickar på **Installera WordPress**.</span><span class="sxs-lookup"><span data-stu-id="47f4f-161">In the WordPress **Welcome** page, enter the configuration information required by WordPress, and then click **Install WordPress**.</span></span>
   
    ![Konfigurera WordPress](./media/web-sites-php-web-site-gallery/wpconfigure.png)
5. <span data-ttu-id="47f4f-163">Logga in med autentiseringsuppgifterna du skapade på sidan **Välkommen**.</span><span class="sxs-lookup"><span data-stu-id="47f4f-163">Log in using the credentials you created on the **Welcome** page.</span></span>  
6. <span data-ttu-id="47f4f-164">Webbplatsens instrumentpanelssida öppnas.</span><span class="sxs-lookup"><span data-stu-id="47f4f-164">Your site Dashboard page opens.</span></span>    
   
    ![WordPress-webbplats](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## <a name="next-steps"></a><span data-ttu-id="47f4f-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="47f4f-166">Next steps</span></span>
<span data-ttu-id="47f4f-167">Du har fått veta hur du skapar och distribuerar en PHP-webbapp från galleriet.</span><span class="sxs-lookup"><span data-stu-id="47f4f-167">You've seen how to create and deploy a PHP web app from the gallery.</span></span> <span data-ttu-id="47f4f-168">Mer information om hur du använder PHP i Azure finns i [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="47f4f-168">For more information about using PHP in Azure, see the [PHP Developer Center](/develop/php/).</span></span>

<span data-ttu-id="47f4f-169">Du hittar mer information om hur du arbetar med App Service Web Apps via länkarna till vänster på sidan (breda webbläsarfönster) eller längst upp på sidan (smala webbläsarfönster).</span><span class="sxs-lookup"><span data-stu-id="47f4f-169">For more information about how to work with App Service Web Apps, see the links on the left side of the page (for wide browser windows) or at the top of the page (for narrow browser windows).</span></span> 

## <a name="whats-changed"></a><span data-ttu-id="47f4f-170">Nyheter</span><span class="sxs-lookup"><span data-stu-id="47f4f-170">What's changed</span></span>
* <span data-ttu-id="47f4f-171">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="47f4f-171">For a guide to the change from Websites to App Service, see [Azure App Service and its impact on existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png
