---
title: aaaCreate en WordPress-webbapp i Azure App Service | Microsoft Docs
description: "Lär dig hur toocreate en ny Azure web app för en WordPress-blogg hello Azure-portalen."
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
ms.openlocfilehash: 3a95fcb6732c15a8200921ce474b6dde2298dec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-wordpress-web-app-in-azure-app-service"></a><span data-ttu-id="1118c-103">Skapa en WordPress-webbapp i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1118c-103">Create a WordPress web app in Azure App Service</span></span>
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

<span data-ttu-id="1118c-104">Den här kursen visar hur toodeploy en WordPress-blogg plats från hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="1118c-104">This tutorial shows how toodeploy a WordPress blog site from hello Azure Marketplace.</span></span>

<span data-ttu-id="1118c-105">När du är klar med hello kursen har du en egen WordPress-bloggwebbplats som körs i molnet hello.</span><span class="sxs-lookup"><span data-stu-id="1118c-105">When you're done with hello tutorial you'll have your own WordPress blog site up and running in hello cloud.</span></span>

![WordPress-webbplats](./media/web-sites-php-web-site-gallery/wpdashboard.png)

<span data-ttu-id="1118c-107">Du får lära dig:</span><span class="sxs-lookup"><span data-stu-id="1118c-107">You'll learn:</span></span>

* <span data-ttu-id="1118c-108">Hur toofind en programmall i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="1118c-108">How toofind an application template in hello Azure Marketplace.</span></span>
* <span data-ttu-id="1118c-109">Hur toocreate en webbapp i Azure App Service som baseras på hello mallen.</span><span class="sxs-lookup"><span data-stu-id="1118c-109">How toocreate a web app in Azure App Service that is based on hello template.</span></span>
* <span data-ttu-id="1118c-110">Hur tooconfigure Azure App Service-inställningar för nya hello web app och databasen.</span><span class="sxs-lookup"><span data-stu-id="1118c-110">How tooconfigure Azure App Service settings for hello new web app and database.</span></span>

<span data-ttu-id="1118c-111">hello Azure Marketplace blir tillgängligt på en mängd olika populära webbappar som har utvecklats av Microsoft och tredjepartsföretag programvara med öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="1118c-111">hello Azure Marketplace makes available a wide range of popular web apps developed by Microsoft, third party companies, and open source software initiatives.</span></span> <span data-ttu-id="1118c-112">hello webbprogram som bygger på en mängd olika populära ramverk, [PHP](/develop/nodejs/) i det här WordPress-exemplet [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), och [Python](/develop/python/), tooname ett fåtal.</span><span class="sxs-lookup"><span data-stu-id="1118c-112">hello web apps are built on a wide range of popular frameworks, such as [PHP](/develop/nodejs/) in this WordPress example, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), and [Python](/develop/python/), tooname a few.</span></span> <span data-ttu-id="1118c-113">toocreate en webbapp från Azure Marketplace hello endast programvara du behöver är hello webbläsare som du använder för hello hello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1118c-113">toocreate a web app from hello Azure Marketplace hello only software you need is hello browser that you use for hello [Azure Portal](https://portal.azure.com/).</span></span> 

<span data-ttu-id="1118c-114">hello WordPress-webbplats som du distribuerar i den här kursen används MySQL som hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="1118c-114">hello WordPress site that you deploy in this tutorial uses MySQL for hello database.</span></span> <span data-ttu-id="1118c-115">Om du vill tooinstead används SQL-databas för hello databasen finns [Project Nami](http://projectnami.org/).</span><span class="sxs-lookup"><span data-stu-id="1118c-115">If you wish tooinstead use SQL Database for hello database, see [Project Nami](http://projectnami.org/).</span></span> <span data-ttu-id="1118c-116">**Project Nami** är också tillgängliga via hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="1118c-116">**Project Nami** is also available through hello Marketplace.</span></span>

> [!NOTE]
> <span data-ttu-id="1118c-117">toocomplete den här självstudiekursen kommer du behöver ett Microsoft Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="1118c-117">toocomplete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="1118c-118">Om du inte har ett konto kan du [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) eller [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="1118c-118">If you don't have an account, you can [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
> 
> <span data-ttu-id="1118c-119">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/).</span><span class="sxs-lookup"><span data-stu-id="1118c-119">If you want tooget started with Azure App Service before you sign up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/).</span></span> <span data-ttu-id="1118c-120">Där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort behövs och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="1118c-120">There, you can immediately create a short-lived starter web app in App Service—no credit card required, and no commitments.</span></span>
> 
> 

## <a name="select-wordpress-and-configure-for-azure-app-service"></a><span data-ttu-id="1118c-121">Välj WordPress och konfigurera för Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1118c-121">Select WordPress and configure for Azure App Service</span></span>
1. <span data-ttu-id="1118c-122">Logga in toohello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1118c-122">Log in toohello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="1118c-123">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="1118c-123">Click **New**.</span></span>
   
    ![Skapa Ny][5]
3. <span data-ttu-id="1118c-125">Sök efter **WordPress** och klicka sedan på **WordPress**.</span><span class="sxs-lookup"><span data-stu-id="1118c-125">Search for **WordPress**, and then click **WordPress**.</span></span> <span data-ttu-id="1118c-126">Om du vill toouse SQL-databas i stället för MySQL söker du efter **Project Nami**.</span><span class="sxs-lookup"><span data-stu-id="1118c-126">If you wish toouse SQL Database instead of MySQL, search for **Project Nami**.</span></span>
   
    ![WordPress från lista][7]
4. <span data-ttu-id="1118c-128">När du har läst hello beskrivning av hello WordPress-appen klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="1118c-128">After reading hello description of hello WordPress app, click **Create**.</span></span>
   
    ![Skapa](./media/web-sites-php-web-site-gallery/create.png)
5. <span data-ttu-id="1118c-130">Ange ett namn för hello webbprogrammet i hello **webbprogrammet** rutan.</span><span class="sxs-lookup"><span data-stu-id="1118c-130">Enter a name for hello web app in hello **Web app** box.</span></span>
   
    <span data-ttu-id="1118c-131">Det här namnet måste vara unikt i hello azurewebsites.NET eftersom hello URL för hello webbprogrammet blir {name}. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="1118c-131">This name must be unique in hello azurewebsites.net domain because hello URL of hello web app will be {name}.azurewebsites.net.</span></span> <span data-ttu-id="1118c-132">Om hello namnet inte är unikt visas ett rött utropstecken i textrutan för hello.</span><span class="sxs-lookup"><span data-stu-id="1118c-132">If hello name you enter isn't unique, a red exclamation mark appears in hello text box.</span></span>
6. <span data-ttu-id="1118c-133">Om du har mer än en prenumeration, Välj hello önskat toouse.</span><span class="sxs-lookup"><span data-stu-id="1118c-133">If you have more than one subscription, choose hello one you want toouse.</span></span> 
7. <span data-ttu-id="1118c-134">Välj en **Resursgrupp** eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="1118c-134">Select a **Resource Group** or create a new one.</span></span>
   
    <span data-ttu-id="1118c-135">Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1118c-135">For more information about resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
8. <span data-ttu-id="1118c-136">Välj en **App Service-plan/plats** eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="1118c-136">Select an **App Service plan/Location** or create a new one.</span></span>
   
    <span data-ttu-id="1118c-137">Mer information om App Service-planer finns i [Översikt över Azure App Service-planer](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</span><span class="sxs-lookup"><span data-stu-id="1118c-137">For more information about App Service plans, see [Azure App Service plans overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</span></span>    
9. <span data-ttu-id="1118c-138">Klicka på **databasen**, och klicka sedan på hello **ny MySQL-databas** bladet ange hello krävs värden för att konfigurera MySQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="1118c-138">Click **Database**, and then in hello **New MySQL Database** blade provide hello required values for configuring your MySQL database.</span></span>
   
    <span data-ttu-id="1118c-139">a.</span><span class="sxs-lookup"><span data-stu-id="1118c-139">a.</span></span> <span data-ttu-id="1118c-140">Ange ett nytt namn eller lämna standardnamnet hello.</span><span class="sxs-lookup"><span data-stu-id="1118c-140">Enter a new name or leave hello default name.</span></span>
   
    <span data-ttu-id="1118c-141">b.</span><span class="sxs-lookup"><span data-stu-id="1118c-141">b.</span></span> <span data-ttu-id="1118c-142">Lämna hello **databastyp** ställa in också**delade**.</span><span class="sxs-lookup"><span data-stu-id="1118c-142">Leave hello **Database Type** set too**Shared**.</span></span>
   
    <span data-ttu-id="1118c-143">c.</span><span class="sxs-lookup"><span data-stu-id="1118c-143">c.</span></span> <span data-ttu-id="1118c-144">Välj hello samma plats som hello som du har valt för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1118c-144">Choose hello same location as hello one you chose for hello web app.</span></span>
   
    <span data-ttu-id="1118c-145">d.</span><span class="sxs-lookup"><span data-stu-id="1118c-145">d.</span></span> <span data-ttu-id="1118c-146">Välj en prisnivå.</span><span class="sxs-lookup"><span data-stu-id="1118c-146">Choose a pricing tier.</span></span> <span data-ttu-id="1118c-147">För kursen räcker det bra med Mercury (kostnadsfri med det lägsta antalet tillåtna anslutningar och lägst mängd diskutrymme).</span><span class="sxs-lookup"><span data-stu-id="1118c-147">Mercury (free with minimal allowed connections and disk space) is fine for this tutorial.</span></span>
10. <span data-ttu-id="1118c-148">I hello **ny MySQL-databas** bladet, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1118c-148">In hello **New MySQL Database** blade, click **OK**.</span></span> 
11. <span data-ttu-id="1118c-149">I hello **WordPress** bladet godkänner hello juridiska villkoren och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="1118c-149">In hello **WordPress** blade, accept hello legal terms, and then click **Create**.</span></span> 
    
     ![Konfigurera webbappen](./media/web-sites-php-web-site-gallery/configure.png)
    
     <span data-ttu-id="1118c-151">Azure App Service skapar hello webbapp vanligen på mindre än en minut.</span><span class="sxs-lookup"><span data-stu-id="1118c-151">Azure App Service creates hello web app, typically in less than a minute.</span></span> <span data-ttu-id="1118c-152">Du kan titta på hello förloppet genom att klicka på hello klockikonen överst hello i hello Portalsida.</span><span class="sxs-lookup"><span data-stu-id="1118c-152">You can watch hello progress by clicking hello bell icon at hello top of hello portal page.</span></span>
    
     ![Förloppsindikator](./media/web-sites-php-web-site-gallery/progress.png)

## <a name="launch-and-manage-your-wordpress-web-app"></a><span data-ttu-id="1118c-154">Starta och hantera WordPress-webbappen</span><span class="sxs-lookup"><span data-stu-id="1118c-154">Launch and manage your WordPress web app</span></span>
1. <span data-ttu-id="1118c-155">När hello webbappen har skapats, gå hello Azure Portal toohello resursgrupp som du skapat hello program och du kan se hello webbapp och hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="1118c-155">When hello web app creation is finished, navigate in hello Azure Portal toohello resource group in which you created hello application, and you can see hello web app and hello database.</span></span>
   
    <span data-ttu-id="1118c-156">Hej extraresursen med lampikonen hello är [Programinsikter](/services/application-insights/), som tillhandahåller övervakningstjänster för webbappen.</span><span class="sxs-lookup"><span data-stu-id="1118c-156">hello extra resource with hello light bulb icon is [Application Insights](/services/application-insights/), which provides monitoring services for your web app.</span></span>
2. <span data-ttu-id="1118c-157">I hello **resursgruppen** bladet, klickar du på webbappsraden hello.</span><span class="sxs-lookup"><span data-stu-id="1118c-157">In hello **Resource group** blade, click hello web app line.</span></span>
   
    ![Konfigurera webbappen](./media/web-sites-php-web-site-gallery/resourcegroup.png)
3. <span data-ttu-id="1118c-159">Hello Web app-bladet, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="1118c-159">In hello Web app blade, click **Browse**.</span></span>
   
    ![webbadress][browse]
4. <span data-ttu-id="1118c-161">I hello WordPress **Välkommen** sidan Ange hello konfigurationsinformation som krävs av WordPress och klickar sedan på **installera WordPress**.</span><span class="sxs-lookup"><span data-stu-id="1118c-161">In hello WordPress **Welcome** page, enter hello configuration information required by WordPress, and then click **Install WordPress**.</span></span>
   
    ![Konfigurera WordPress](./media/web-sites-php-web-site-gallery/wpconfigure.png)
5. <span data-ttu-id="1118c-163">Logga in med hello autentiseringsuppgifterna som du skapade på hello **Välkommen** sidan.</span><span class="sxs-lookup"><span data-stu-id="1118c-163">Log in using hello credentials you created on hello **Welcome** page.</span></span>  
6. <span data-ttu-id="1118c-164">Webbplatsens instrumentpanelssida öppnas.</span><span class="sxs-lookup"><span data-stu-id="1118c-164">Your site Dashboard page opens.</span></span>    
   
    ![WordPress-webbplats](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## <a name="next-steps"></a><span data-ttu-id="1118c-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1118c-166">Next steps</span></span>
<span data-ttu-id="1118c-167">Du har sett hur toocreate och distribuera en PHP-webbapp från galleriet hello.</span><span class="sxs-lookup"><span data-stu-id="1118c-167">You've seen how toocreate and deploy a PHP web app from hello gallery.</span></span> <span data-ttu-id="1118c-168">Mer information om hur du använder PHP i Azure finns hello [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="1118c-168">For more information about using PHP in Azure, see hello [PHP Developer Center](/develop/php/).</span></span>

<span data-ttu-id="1118c-169">Mer information om hur toowork med App Service Web Apps Se hello länkar på hello vänster sida av hello sidan (breda webbläsarfönster) eller hello överst i hello sidan (smala webbläsarfönster).</span><span class="sxs-lookup"><span data-stu-id="1118c-169">For more information about how toowork with App Service Web Apps, see hello links on hello left side of hello page (for wide browser windows) or at hello top of hello page (for narrow browser windows).</span></span> 

## <a name="whats-changed"></a><span data-ttu-id="1118c-170">Nyheter</span><span class="sxs-lookup"><span data-stu-id="1118c-170">What's changed</span></span>
* <span data-ttu-id="1118c-171">En guide toohello ändring från webbplatser tooApp tjänsten finns [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="1118c-171">For a guide toohello change from Websites tooApp Service, see [Azure App Service and its impact on existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png
