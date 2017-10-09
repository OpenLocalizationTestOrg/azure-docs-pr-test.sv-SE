---
title: "aaaDeploy WordPress-appen i hello Azure-portalen på fem minuter | Microsoft Docs"
description: "Lär dig hur lätt det är toorun webbappar i App Service genom att distribuera en WordPress-appen. Resultatet visas omedelbart."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: cephalin
ms.openlocfilehash: 4cd26bbacf89657997847ded1284e472288ddebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-wordpress-app-in-hello-azure-portal-in-five-minutes"></a><span data-ttu-id="a0c59-104">Distribuera en WordPress-appen i hello Azure-portalen på fem minuter</span><span class="sxs-lookup"><span data-stu-id="a0c59-104">Deploy a WordPress app in hello Azure portal in five minutes</span></span>

<span data-ttu-id="a0c59-105">De här självstudierna visar hur toodeploy din första [WordPress](https://wordpress.org/) webbapp för[Azure App Service](../app-service/app-service-value-prop-what-is.md) i minuter.</span><span class="sxs-lookup"><span data-stu-id="a0c59-105">This tutorial shows you how toodeploy your first [WordPress](https://wordpress.org/) web app too[Azure App Service](../app-service/app-service-value-prop-what-is.md) in minutes.</span></span>

![WordPress-webbplats](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a><span data-ttu-id="a0c59-107">Krav</span><span class="sxs-lookup"><span data-stu-id="a0c59-107">Prerequisites</span></span>
<span data-ttu-id="a0c59-108">Du behöver ett Microsoft Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a0c59-108">You need a Microsoft Azure account.</span></span> <span data-ttu-id="a0c59-109">Om du inte har ett konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="a0c59-109">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="a0c59-110">Du kan [Prova App Service](https://azure.microsoft.com/try/app-service/) utan ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a0c59-110">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="a0c59-111">Skapa en startapp och spela upp med den för in tooan timme – inget kreditkort behövs, inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="a0c59-111">Create a starter app and play with it for up tooan hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="deploy-hello-wordpress-app"></a><span data-ttu-id="a0c59-112">Distribuera hello WordPress-appen</span><span class="sxs-lookup"><span data-stu-id="a0c59-112">Deploy hello WordPress app</span></span>
1. <span data-ttu-id="a0c59-113">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a0c59-113">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="a0c59-114">Öppna [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).</span><span class="sxs-lookup"><span data-stu-id="a0c59-114">Open [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).</span></span>

    <span data-ttu-id="a0c59-115">Den här länken är en genväg tooimmediately konfigurerar en ny WordPress-app i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a0c59-115">This link is a shortcut tooimmediately configure a new WordPress app in hello Azure portal.</span></span>

3. <span data-ttu-id="a0c59-116">I **Appnamn** anger du webbappens namn.</span><span class="sxs-lookup"><span data-stu-id="a0c59-116">In **App name**, type a web app name.</span></span> <span data-ttu-id="a0c59-117">Visas en grön bock i hello rutan om hello namn är unikt i hello `azurewebsites.net` domän.</span><span class="sxs-lookup"><span data-stu-id="a0c59-117">You will see a green checkmark in hello box if hello name is unique in hello `azurewebsites.net` domain.</span></span>
   
5. <span data-ttu-id="a0c59-118">I **resursgruppen**, klickar du på **Skapa nytt** toocreate en ny [resursgruppen](../azure-resource-manager/resource-group-overview.md), ge den ett namn.</span><span class="sxs-lookup"><span data-stu-id="a0c59-118">In **Resource Group**, click **Create new** toocreate a new [resource group](../azure-resource-manager/resource-group-overview.md), then give it a name.</span></span>

6. <span data-ttu-id="a0c59-119">I **Databasprovider** väljer du **CleaDB**.</span><span class="sxs-lookup"><span data-stu-id="a0c59-119">In **Database Provider**, select **CleaDB**.</span></span>

7. <span data-ttu-id="a0c59-120">Klicka på **App Service plan/plats** > **Skapa ny**.</span><span class="sxs-lookup"><span data-stu-id="a0c59-120">Click **App Service plan/Location** > **Create New**.</span></span> <span data-ttu-id="a0c59-121">Konfigurera hello [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) som visas:</span><span class="sxs-lookup"><span data-stu-id="a0c59-121">Configure hello [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) as shown:</span></span>

    - <span data-ttu-id="a0c59-122">I **programtjänstplanen**, hello önskade namn.</span><span class="sxs-lookup"><span data-stu-id="a0c59-122">In **App Service plan**, type hello desired name.</span></span>
    - <span data-ttu-id="a0c59-123">I **plats**, Välj en plats toohost planen.</span><span class="sxs-lookup"><span data-stu-id="a0c59-123">In **Location**, choose a location toohost your plan.</span></span>
    - <span data-ttu-id="a0c59-124">Klicka på **Prisnivå**, välj **F1 Kostnadsfri** eller någon annan nivå som passar dig och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="a0c59-124">Click **Pricing tier**, then select **F1 Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="a0c59-125">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a0c59-125">Click **OK**.</span></span>

8. <span data-ttu-id="a0c59-126">Klicka på **Databas** > **Skapa ny**.</span><span class="sxs-lookup"><span data-stu-id="a0c59-126">Click **Database** > **Create New**.</span></span> <span data-ttu-id="a0c59-127">Konfigurera hello SQL-databas som visas:</span><span class="sxs-lookup"><span data-stu-id="a0c59-127">Configure hello SQL Database as shown:</span></span>

    - <span data-ttu-id="a0c59-128">I **Databasnamn** skriver du ett databasnamn.</span><span class="sxs-lookup"><span data-stu-id="a0c59-128">In **Database Name**, type a database name.</span></span> 
    - <span data-ttu-id="a0c59-129">I **plats**, Välj hello samma plats som hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="a0c59-129">In **Location**, choose hello same location as hello App Service plan.</span></span>
    - <span data-ttu-id="a0c59-130">Klicka på **Prisnivå**, välj **Mercury** eller någon annan nivå som passar dig och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="a0c59-130">Click **Pricing tier**, then select **Mercury** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="a0c59-131">Välj **Juridiska villkor** och klicka på **Köp**.</span><span class="sxs-lookup"><span data-stu-id="a0c59-131">Click **Legal Terms** and click **Purchase**.</span></span>
    - <span data-ttu-id="a0c59-132">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a0c59-132">Click **OK**.</span></span>

9. <span data-ttu-id="a0c59-133">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a0c59-133">Click **Create**.</span></span>

    <span data-ttu-id="a0c59-134">Azure skapar nu WordPress-appen utifrån din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a0c59-134">Azure now creates your WordPress app based on your configuration.</span></span> <span data-ttu-id="a0c59-135">Du bör se meddelandet **Distributionen har påbörjats**.</span><span class="sxs-lookup"><span data-stu-id="a0c59-135">You should see a **Deployment started...** notification.</span></span>

    ![Distributionen har startat – första WordPress i Azure App Service](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a><span data-ttu-id="a0c59-137">Starta och hantera WordPress-webbappen</span><span class="sxs-lookup"><span data-stu-id="a0c59-137">Launch and manage your WordPress web app</span></span>

<span data-ttu-id="a0c59-138">När appen har distribuerats i Azure visas ett nytt meddelande.</span><span class="sxs-lookup"><span data-stu-id="a0c59-138">When Azure completes app deployment you see another notification.</span></span>

![Distributionen har utförts – första WordPress i Azure App Service](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. <span data-ttu-id="a0c59-140">Klicka på hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="a0c59-140">Click hello notification.</span></span> <span data-ttu-id="a0c59-141">Om du har missat du alltid har åtkomst till den genom att klicka på hello notification bell (![meddelande nedan - första WordPress i Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span><span class="sxs-lookup"><span data-stu-id="a0c59-141">If you missed it, you can always access it by clicking hello notification bell (![Notification bellow - first WordPress in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span></span>

    <span data-ttu-id="a0c59-142">Du bör nu se webbappens [administrationsblad](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blad*: en portalsida som öppnas horisontellt).</span><span class="sxs-lookup"><span data-stu-id="a0c59-142">You should now see your web app's management [blade](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blade*: a portal page that opens horizontally).</span></span>

3. <span data-ttu-id="a0c59-143">Klicka i hello överkant hello översiktssidan **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="a0c59-143">In hello top of hello Overview page, click **Browse**.</span></span>
   
    ![Bläddra – första WordPress i Azure App Service](./media/app-service-web-get-started-php-portal/browse.png)

    <span data-ttu-id="a0c59-145">Nu visas hello WordPress **Välkommen** sidan.</span><span class="sxs-lookup"><span data-stu-id="a0c59-145">Now you see hello WordPress **Welcome** page.</span></span> <span data-ttu-id="a0c59-146">Konfigurera hello WordPress installationen och starta prova!</span><span class="sxs-lookup"><span data-stu-id="a0c59-146">Configure hello WordPress installation and start playing with it!</span></span>

    ![WordPress-konfiguration – första WordPress i Azure App Service](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a><span data-ttu-id="a0c59-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0c59-148">Next steps</span></span>
* <span data-ttu-id="a0c59-149">[Skapa, konfigurera och distribuera en Laravel web app tooAzure](app-service-web-php-get-started.md) -Läs hello grundläggande kunskaper du behöver toorun ett PHP-webbprogram i Azure, exempelvis:</span><span class="sxs-lookup"><span data-stu-id="a0c59-149">[Create, configure, and deploy a Laravel web app tooAzure](app-service-web-php-get-started.md) - Learn hello basic skills you need toorun any PHP web app in Azure, such as:</span></span>

    * <span data-ttu-id="a0c59-150">Skapa och konfigurera appar i Azure från PowerShell/Bash.</span><span class="sxs-lookup"><span data-stu-id="a0c59-150">Create and configure apps in Azure from PowerShell/Bash.</span></span>
    * <span data-ttu-id="a0c59-151">Ange PHP-version.</span><span class="sxs-lookup"><span data-stu-id="a0c59-151">Set PHP version.</span></span>
    * <span data-ttu-id="a0c59-152">Använda en startfil som inte är i hello rotkatalogen för programmet.</span><span class="sxs-lookup"><span data-stu-id="a0c59-152">Use a start file that is not in hello root application directory.</span></span>
    * <span data-ttu-id="a0c59-153">Aktivera Composer-automatisering.</span><span class="sxs-lookup"><span data-stu-id="a0c59-153">Enable Composer automation.</span></span>
    * <span data-ttu-id="a0c59-154">Få åtkomst till miljöspecifika variabler.</span><span class="sxs-lookup"><span data-stu-id="a0c59-154">Access environment-specific variables.</span></span>
    * <span data-ttu-id="a0c59-155">Felsöka vanliga fel.</span><span class="sxs-lookup"><span data-stu-id="a0c59-155">Troubleshoot common errors.</span></span>

* <span data-ttu-id="a0c59-156">[Distribuera din kod tooAzure Apptjänst](web-sites-deploy.md)– Lär dig hur toodeploy från FTP- eller från källan styra databaser.</span><span class="sxs-lookup"><span data-stu-id="a0c59-156">[Deploy your code tooAzure App Service](web-sites-deploy.md)- Learn how toodeploy from FTP or from source control repositories.</span></span>
* <span data-ttu-id="a0c59-157">[Lägg till funktioner tooyour första webbapp](app-service-web-get-started-2.md) -ta din Azure-app toohello nästa nivå.</span><span class="sxs-lookup"><span data-stu-id="a0c59-157">[Add functionality tooyour first web app](app-service-web-get-started-2.md) - Take your Azure app toohello next level.</span></span> <span data-ttu-id="a0c59-158">Autentisera användarna.</span><span class="sxs-lookup"><span data-stu-id="a0c59-158">Authenticate your users.</span></span> <span data-ttu-id="a0c59-159">Skala den på begäran.</span><span class="sxs-lookup"><span data-stu-id="a0c59-159">Scale it based on demand.</span></span> <span data-ttu-id="a0c59-160">Konfigurera prestandavarningar.</span><span class="sxs-lookup"><span data-stu-id="a0c59-160">Set up some performance alerts.</span></span> <span data-ttu-id="a0c59-161">Allt med några få klickningar.</span><span class="sxs-lookup"><span data-stu-id="a0c59-161">All with a few clicks.</span></span>
