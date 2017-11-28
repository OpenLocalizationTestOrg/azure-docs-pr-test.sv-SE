---
title: "Distribuera en WordPress-app i Azure Portal på fem minuter | Microsoft Docs"
description: "Distribuera en WordPress-app och se hur enkelt det är att köra webbappar i App Service. Resultatet visas omedelbart."
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
ms.openlocfilehash: ef3be9823384bd8dc978c86f5cfde00d1117c4a2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-a-wordpress-app-in-the-azure-portal-in-five-minutes"></a><span data-ttu-id="12888-104">Distribuera en WordPress-app i Azure Portal på fem minuter</span><span class="sxs-lookup"><span data-stu-id="12888-104">Deploy a WordPress app in the Azure portal in five minutes</span></span>

<span data-ttu-id="12888-105">I den här kursen får du lära dig hur du distribuerar din första [WordPress](https://wordpress.org/)-webbapp till [Azure App Service](../app-service/app-service-value-prop-what-is.md) på några minuter.</span><span class="sxs-lookup"><span data-stu-id="12888-105">This tutorial shows you how to deploy your first [WordPress](https://wordpress.org/) web app to [Azure App Service](../app-service/app-service-value-prop-what-is.md) in minutes.</span></span>

![WordPress-webbplats](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a><span data-ttu-id="12888-107">Krav</span><span class="sxs-lookup"><span data-stu-id="12888-107">Prerequisites</span></span>
<span data-ttu-id="12888-108">Du behöver ett Microsoft Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="12888-108">You need a Microsoft Azure account.</span></span> <span data-ttu-id="12888-109">Om du inte har ett konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="12888-109">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="12888-110">Du kan [Prova App Service](https://azure.microsoft.com/try/app-service/) utan ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="12888-110">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="12888-111">Skapa en startapp och testa den i upp till en timme – inget kreditkort behövs, inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="12888-111">Create a starter app and play with it for up to an hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="deploy-the-wordpress-app"></a><span data-ttu-id="12888-112">Distribuera WordPress-appen</span><span class="sxs-lookup"><span data-stu-id="12888-112">Deploy the WordPress app</span></span>
1. <span data-ttu-id="12888-113">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="12888-113">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="12888-114">Öppna [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).</span><span class="sxs-lookup"><span data-stu-id="12888-114">Open [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).</span></span>

    <span data-ttu-id="12888-115">Den här länken är en genväg du kan använda när du vill konfigurera en ny WordPress-app direkt i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="12888-115">This link is a shortcut to immediately configure a new WordPress app in the Azure portal.</span></span>

3. <span data-ttu-id="12888-116">I **Appnamn** anger du webbappens namn.</span><span class="sxs-lookup"><span data-stu-id="12888-116">In **App name**, type a web app name.</span></span> <span data-ttu-id="12888-117">Om namnet är unikt för domänen `azurewebsites.net` så visas en grön bockmarkering i rutan.</span><span class="sxs-lookup"><span data-stu-id="12888-117">You will see a green checkmark in the box if the name is unique in the `azurewebsites.net` domain.</span></span>
   
5. <span data-ttu-id="12888-118">Klicka på **Skapa ny** i **Resursgrupp** om du vill skapa en ny [resursgrupp](../azure-resource-manager/resource-group-overview.md) och ge den sedan ett namn.</span><span class="sxs-lookup"><span data-stu-id="12888-118">In **Resource Group**, click **Create new** to create a new [resource group](../azure-resource-manager/resource-group-overview.md), then give it a name.</span></span>

6. <span data-ttu-id="12888-119">I **Databasprovider** väljer du **CleaDB**.</span><span class="sxs-lookup"><span data-stu-id="12888-119">In **Database Provider**, select **CleaDB**.</span></span>

7. <span data-ttu-id="12888-120">Klicka på **App Service plan/plats** > **Skapa ny**.</span><span class="sxs-lookup"><span data-stu-id="12888-120">Click **App Service plan/Location** > **Create New**.</span></span> <span data-ttu-id="12888-121">Så här konfigurerar du din [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md):</span><span class="sxs-lookup"><span data-stu-id="12888-121">Configure the [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) as shown:</span></span>

    - <span data-ttu-id="12888-122">Ange önskat namn i **App Service plan**.</span><span class="sxs-lookup"><span data-stu-id="12888-122">In **App Service plan**, type the desired name.</span></span>
    - <span data-ttu-id="12888-123">Välj en plats för planens värd i **Plats**.</span><span class="sxs-lookup"><span data-stu-id="12888-123">In **Location**, choose a location to host your plan.</span></span>
    - <span data-ttu-id="12888-124">Klicka på **Prisnivå**, välj **F1 Kostnadsfri** eller någon annan nivå som passar dig och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="12888-124">Click **Pricing tier**, then select **F1 Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="12888-125">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="12888-125">Click **OK**.</span></span>

8. <span data-ttu-id="12888-126">Klicka på **Databas** > **Skapa ny**.</span><span class="sxs-lookup"><span data-stu-id="12888-126">Click **Database** > **Create New**.</span></span> <span data-ttu-id="12888-127">Konfigurera SQL-databasen så här:</span><span class="sxs-lookup"><span data-stu-id="12888-127">Configure the SQL Database as shown:</span></span>

    - <span data-ttu-id="12888-128">I **Databasnamn** skriver du ett databasnamn.</span><span class="sxs-lookup"><span data-stu-id="12888-128">In **Database Name**, type a database name.</span></span> 
    - <span data-ttu-id="12888-129">I **Plats** väljer du samma plats som App Service- planen.</span><span class="sxs-lookup"><span data-stu-id="12888-129">In **Location**, choose the same location as the App Service plan.</span></span>
    - <span data-ttu-id="12888-130">Klicka på **Prisnivå**, välj **Mercury** eller någon annan nivå som passar dig och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="12888-130">Click **Pricing tier**, then select **Mercury** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="12888-131">Välj **Juridiska villkor** och klicka på **Köp**.</span><span class="sxs-lookup"><span data-stu-id="12888-131">Click **Legal Terms** and click **Purchase**.</span></span>
    - <span data-ttu-id="12888-132">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="12888-132">Click **OK**.</span></span>

9. <span data-ttu-id="12888-133">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="12888-133">Click **Create**.</span></span>

    <span data-ttu-id="12888-134">Azure skapar nu WordPress-appen utifrån din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="12888-134">Azure now creates your WordPress app based on your configuration.</span></span> <span data-ttu-id="12888-135">Du bör se meddelandet **Distributionen har påbörjats**.</span><span class="sxs-lookup"><span data-stu-id="12888-135">You should see a **Deployment started...** notification.</span></span>

    ![Distributionen har startat – första WordPress i Azure App Service](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a><span data-ttu-id="12888-137">Starta och hantera WordPress-webbappen</span><span class="sxs-lookup"><span data-stu-id="12888-137">Launch and manage your WordPress web app</span></span>

<span data-ttu-id="12888-138">När appen har distribuerats i Azure visas ett nytt meddelande.</span><span class="sxs-lookup"><span data-stu-id="12888-138">When Azure completes app deployment you see another notification.</span></span>

![Distributionen har utförts – första WordPress i Azure App Service](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. <span data-ttu-id="12888-140">Klicka på meddelandet.</span><span class="sxs-lookup"><span data-stu-id="12888-140">Click the notification.</span></span> <span data-ttu-id="12888-141">Om du missade meddelandet kan du alltid visa det genom att klicka på meddelandeikonen (![Meddelandeikon – första WordPress i Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span><span class="sxs-lookup"><span data-stu-id="12888-141">If you missed it, you can always access it by clicking the notification bell (![Notification bellow - first WordPress in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span></span>

    <span data-ttu-id="12888-142">Du bör nu se webbappens [administrationsblad](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blad*: en portalsida som öppnas horisontellt).</span><span class="sxs-lookup"><span data-stu-id="12888-142">You should now see your web app's management [blade](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blade*: a portal page that opens horizontally).</span></span>

3. <span data-ttu-id="12888-143">Klicka på **Bläddra** längst upp på sidan Översikt.</span><span class="sxs-lookup"><span data-stu-id="12888-143">In the top of the Overview page, click **Browse**.</span></span>
   
    ![Bläddra – första WordPress i Azure App Service](./media/app-service-web-get-started-php-portal/browse.png)

    <span data-ttu-id="12888-145">Nu visas **välkomstsidan** för WordPress.</span><span class="sxs-lookup"><span data-stu-id="12888-145">Now you see the WordPress **Welcome** page.</span></span> <span data-ttu-id="12888-146">Konfigurera WordPress-installationen och prova några funktioner!</span><span class="sxs-lookup"><span data-stu-id="12888-146">Configure the WordPress installation and start playing with it!</span></span>

    ![WordPress-konfiguration – första WordPress i Azure App Service](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a><span data-ttu-id="12888-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12888-148">Next steps</span></span>
* <span data-ttu-id="12888-149">[Skapa, konfigurera och distribuera en Laravel-webbapp till Azure](app-service-web-php-get-started.md) – Lär dig grunderna du behöver för att köra valfri PHP-webbapp i Azure, som:</span><span class="sxs-lookup"><span data-stu-id="12888-149">[Create, configure, and deploy a Laravel web app to Azure](app-service-web-php-get-started.md) - Learn the basic skills you need to run any PHP web app in Azure, such as:</span></span>

    * <span data-ttu-id="12888-150">Skapa och konfigurera appar i Azure från PowerShell/Bash.</span><span class="sxs-lookup"><span data-stu-id="12888-150">Create and configure apps in Azure from PowerShell/Bash.</span></span>
    * <span data-ttu-id="12888-151">Ange PHP-version.</span><span class="sxs-lookup"><span data-stu-id="12888-151">Set PHP version.</span></span>
    * <span data-ttu-id="12888-152">Använda en startfil som inte finns i rotkatalogen för programmet.</span><span class="sxs-lookup"><span data-stu-id="12888-152">Use a start file that is not in the root application directory.</span></span>
    * <span data-ttu-id="12888-153">Aktivera Composer-automatisering.</span><span class="sxs-lookup"><span data-stu-id="12888-153">Enable Composer automation.</span></span>
    * <span data-ttu-id="12888-154">Få åtkomst till miljöspecifika variabler.</span><span class="sxs-lookup"><span data-stu-id="12888-154">Access environment-specific variables.</span></span>
    * <span data-ttu-id="12888-155">Felsöka vanliga fel.</span><span class="sxs-lookup"><span data-stu-id="12888-155">Troubleshoot common errors.</span></span>

* <span data-ttu-id="12888-156">[Distribuera din kod till Azure App Service](web-sites-deploy.md) – lär dig hur du distribuerar via FTP eller källkodsdatabaser.</span><span class="sxs-lookup"><span data-stu-id="12888-156">[Deploy your code to Azure App Service](web-sites-deploy.md)- Learn how to deploy from FTP or from source control repositories.</span></span>
* <span data-ttu-id="12888-157">[Lägg in funktioner i din första webbapp](app-service-web-get-started-2.md) – ta din Azure-app till nästa nivå.</span><span class="sxs-lookup"><span data-stu-id="12888-157">[Add functionality to your first web app](app-service-web-get-started-2.md) - Take your Azure app to the next level.</span></span> <span data-ttu-id="12888-158">Autentisera användarna.</span><span class="sxs-lookup"><span data-stu-id="12888-158">Authenticate your users.</span></span> <span data-ttu-id="12888-159">Skala den på begäran.</span><span class="sxs-lookup"><span data-stu-id="12888-159">Scale it based on demand.</span></span> <span data-ttu-id="12888-160">Konfigurera prestandavarningar.</span><span class="sxs-lookup"><span data-stu-id="12888-160">Set up some performance alerts.</span></span> <span data-ttu-id="12888-161">Allt med några få klickningar.</span><span class="sxs-lookup"><span data-stu-id="12888-161">All with a few clicks.</span></span>
