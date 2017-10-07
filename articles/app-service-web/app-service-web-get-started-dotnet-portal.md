---
title: "aaaDeploy en Umbraco webbapp i hello Azure-portalen på fem minuter | Microsoft Docs"
description: "Lär dig hur lätt det är toorun webbappar i App Service genom att distribuera en exempelapp för ASP.NET. Resultatet visas omedelbart."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: cephalin
ms.openlocfilehash: 6da45f99b3043a3684e3d99c14e6443d597b5212
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-umbraco-web-app-in-hello-azure-portal-in-five-minutes"></a><span data-ttu-id="75833-104">Distribuera en Umbraco webbapp i hello Azure-portalen på fem minuter</span><span class="sxs-lookup"><span data-stu-id="75833-104">Deploy an Umbraco web app in hello Azure portal in five minutes</span></span>

<span data-ttu-id="75833-105">Den här kursen hjälper dig att distribuera n [Umbraco](https://our.umbraco.org/) webbapp för[Azure App Service](../app-service/app-service-value-prop-what-is.md) i minuter.</span><span class="sxs-lookup"><span data-stu-id="75833-105">This tutorial helps you deploy n [Umbraco](https://our.umbraco.org/) web app too[Azure App Service](../app-service/app-service-value-prop-what-is.md) in minutes.</span></span>

![Umbraco-app](./media/app-service-web-get-started-dotnet-portal/defaultpage.png)

## <a name="prerequisites"></a><span data-ttu-id="75833-107">Krav</span><span class="sxs-lookup"><span data-stu-id="75833-107">Prerequisites</span></span>
<span data-ttu-id="75833-108">Du behöver ett Microsoft Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="75833-108">You need a Microsoft Azure account.</span></span> <span data-ttu-id="75833-109">Om du inte har ett konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="75833-109">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="75833-110">Du kan [Prova App Service](https://azure.microsoft.com/try/app-service/) utan ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="75833-110">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="75833-111">Skapa en startapp och spela upp med den för in tooan timme – inget kreditkort behövs, inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="75833-111">Create a starter app and play with it for up tooan hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="deploy-hello-aspnet-app"></a><span data-ttu-id="75833-112">Distribuera hello ASP.NET-app</span><span class="sxs-lookup"><span data-stu-id="75833-112">Deploy hello ASP.NET app</span></span>
1. <span data-ttu-id="75833-113">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="75833-113">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="75833-114">Öppna [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).</span><span class="sxs-lookup"><span data-stu-id="75833-114">Open [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).</span></span>

    <span data-ttu-id="75833-115">Den här länken är en genväg tooimmediately konfigurerar en ny Umbraco app i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="75833-115">This link is a shortcut tooimmediately configure a new Umbraco app in hello Azure portal.</span></span>

3. <span data-ttu-id="75833-116">I **Appnamn** anger du webbappens namn.</span><span class="sxs-lookup"><span data-stu-id="75833-116">In **App name**, type a web app name.</span></span> <span data-ttu-id="75833-117">Visas en grön bock i hello rutan om hello namn är unikt i hello `azurewebsites.net` domän.</span><span class="sxs-lookup"><span data-stu-id="75833-117">You will see a green checkmark in hello box if hello name is unique in hello `azurewebsites.net` domain.</span></span>
   
5. <span data-ttu-id="75833-118">I **resursgruppen**, klickar du på **Skapa nytt** toocreate en ny [resursgruppen](../azure-resource-manager/resource-group-overview.md), ge den ett namn.</span><span class="sxs-lookup"><span data-stu-id="75833-118">In **Resource Group**, click **Create new** toocreate a new [resource group](../azure-resource-manager/resource-group-overview.md), then give it a name.</span></span>

7. <span data-ttu-id="75833-119">Klicka på **App Service plan/plats** > **Skapa ny**.</span><span class="sxs-lookup"><span data-stu-id="75833-119">Click **App Service plan/Location** > **Create New**.</span></span> <span data-ttu-id="75833-120">Konfigurera hello [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) som visas:</span><span class="sxs-lookup"><span data-stu-id="75833-120">Configure hello [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) as shown:</span></span>

    - <span data-ttu-id="75833-121">I **programtjänstplanen**, hello önskade namn.</span><span class="sxs-lookup"><span data-stu-id="75833-121">In **App Service plan**, type hello desired name.</span></span>
    - <span data-ttu-id="75833-122">I **plats**, Välj en plats toohost planen.</span><span class="sxs-lookup"><span data-stu-id="75833-122">In **Location**, choose a location toohost your plan.</span></span>
    - <span data-ttu-id="75833-123">Klicka på **Prisnivå**, välj **F1 Kostnadsfri** eller någon annan nivå som passar dig och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="75833-123">Click **Pricing tier**, then select **F1 Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="75833-124">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="75833-124">Click **OK**.</span></span>

    <span data-ttu-id="75833-125">Konfigurationen Umbraco CMS bör nu se ut som följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="75833-125">Your Umbraco CMS configuration should now look like hello following screenshot:</span></span>

    ![Konfigurationen pågår – första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-in-progress.png)

12. <span data-ttu-id="75833-127">Klicka på **SQL Database** > **Skapa en ny databas**.</span><span class="sxs-lookup"><span data-stu-id="75833-127">Click **SQL Database** > **Create a new database**.</span></span> <span data-ttu-id="75833-128">Konfigurera hello SQL-databas som visas:</span><span class="sxs-lookup"><span data-stu-id="75833-128">Configure hello SQL Database as shown:</span></span>

    - <span data-ttu-id="75833-129">Ange ett namn i fältet **Namn**, till exempel **minDB**.</span><span class="sxs-lookup"><span data-stu-id="75833-129">In **Name**, type a name, such as **myDB**.</span></span>
    - <span data-ttu-id="75833-130">Klicka på **Prisnivå**, välj **1 Kostnadsfri** eller någon annan nivå som passar dig och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="75833-130">Click **Pricing tier**, then select **F Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="75833-131">Klicka på **Målserver** > **Skapa en ny server**.</span><span class="sxs-lookup"><span data-stu-id="75833-131">Click **Target server** > **Create a new server**.</span></span> <span data-ttu-id="75833-132">Konfigurera hello databasserver som visas:</span><span class="sxs-lookup"><span data-stu-id="75833-132">Configure hello database server as shown:</span></span>

        - <span data-ttu-id="75833-133">Ange ett servernamn i fältet **Servernamn**.</span><span class="sxs-lookup"><span data-stu-id="75833-133">In **Server name**, type a server name.</span></span> <span data-ttu-id="75833-134">Visas en grön bock i hello rutan om hello namn är unikt i hello `.database.windows.net` domän.</span><span class="sxs-lookup"><span data-stu-id="75833-134">You will see a green checkmark in hello box if hello name is unique in hello `.database.windows.net` domain.</span></span>
        - <span data-ttu-id="75833-135">I **inloggning för serveradministratör**, typen hello önskad administratörsbehörighet användarnamn.</span><span class="sxs-lookup"><span data-stu-id="75833-135">In **Server admin login**, type hello desired admininistrator username.</span></span>
        - <span data-ttu-id="75833-136">I **lösenord** och **Bekräfta lösenord**, ange hello önskade lösenord.</span><span class="sxs-lookup"><span data-stu-id="75833-136">In **Password** and **Confirm password**, type hello desired password.</span></span>
        - <span data-ttu-id="75833-137">Välj hello på plats, samma plats som du använder för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="75833-137">In Location, select hello same location you use for hello web app.</span></span>
        - <span data-ttu-id="75833-138">Kontrollera att **Tillåt azure-tjänster tooaccess server** är markerad.</span><span class="sxs-lookup"><span data-stu-id="75833-138">Make sure **Allow azure services tooaccess server** is selected.</span></span>
        - <span data-ttu-id="75833-139">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="75833-139">Click **Select**.</span></span>
    
    - <span data-ttu-id="75833-140">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="75833-140">Click **Select**.</span></span>

13. <span data-ttu-id="75833-141">Klicka på **Web app-inställningar**anger hello databasanvändarnamn och lösenord och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="75833-141">Click **Web app settings**, specify hello database username and password, and click **OK**.</span></span>

    <span data-ttu-id="75833-142">Konfigurationen Umbraco CMS bör nu se ut som följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="75833-142">Your Umbraco CMS configuration should now look like hello following screenshot:</span></span>

    ![Konfigurationen är slutförd – första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-complete.png)

14. <span data-ttu-id="75833-144">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="75833-144">Click **Create**.</span></span>
    
    <span data-ttu-id="75833-145">Azure skapar nu Umbraco-appen utifrån din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="75833-145">Azure now creates your Umbraco app based on your configuration.</span></span> <span data-ttu-id="75833-146">Du bör se meddelandet **Distributionen har påbörjats**.</span><span class="sxs-lookup"><span data-stu-id="75833-146">You should see a **Deployment started...** notification.</span></span>

    ![Distributionen har utförts – första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-umrbaco-web-app"></a><span data-ttu-id="75833-148">Starta och hantera Umbraco-webbappen</span><span class="sxs-lookup"><span data-stu-id="75833-148">Launch and manage your Umrbaco web app</span></span>

<span data-ttu-id="75833-149">När appen har distribuerats i Azure visas ett nytt meddelande.</span><span class="sxs-lookup"><span data-stu-id="75833-149">When Azure completes app deployment you see another notification.</span></span>

![Distributionen har utförts – första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-succeeded.png)

1. <span data-ttu-id="75833-151">Klicka på hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="75833-151">Click hello notification.</span></span> <span data-ttu-id="75833-152">Om du har missat du alltid har åtkomst till den genom att klicka på hello notification bell (![meddelande nedan - första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span><span class="sxs-lookup"><span data-stu-id="75833-152">If you missed it, you can always access it by clicking hello notification bell (![Notification bellow - first Umbraco in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span></span>

    <span data-ttu-id="75833-153">Du bör nu se webbappens [administrationsblad](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blad*: en portalsida som öppnas horisontellt).</span><span class="sxs-lookup"><span data-stu-id="75833-153">You should now see your web app's management [blade](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blade*: a portal page that opens horizontally).</span></span>

3. <span data-ttu-id="75833-154">Klicka i hello överkant hello översiktssidan **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="75833-154">In hello top of hello Overview page, click **Browse**.</span></span>
   
    ![Bläddra – första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/browse.png)

    <span data-ttu-id="75833-156">Nu visas hello Umbraco **Välkommen** sidan.</span><span class="sxs-lookup"><span data-stu-id="75833-156">Now you see hello Umbraco **Welcome** page.</span></span> <span data-ttu-id="75833-157">Konfigurera hello Umbraco installationen och starta prova!</span><span class="sxs-lookup"><span data-stu-id="75833-157">Configure hello Umbraco installation and start playing with it!</span></span>

    ![Umbraco-konfiguration – första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/umbraco-config.png)
    
## <a name="next-steps"></a><span data-ttu-id="75833-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75833-159">Next steps</span></span>
* <span data-ttu-id="75833-160">[Distribuera ett ASP.NET web app tooAzure App Service med Visual Studio](app-service-web-get-started-dotnet.md) – Lär dig hur toocreate en ny Azure-webbapp från Visual Studio, med någon av hello med programmallar.</span><span class="sxs-lookup"><span data-stu-id="75833-160">[Deploy an ASP.NET web app tooAzure App Service, using Visual Studio](app-service-web-get-started-dotnet.md) - Learn how toocreate a new Azure web app from Visual Studio, using any one of hello included application templates.</span></span>
* <span data-ttu-id="75833-161">[Distribuera din kod tooAzure Apptjänst](web-sites-deploy.md)– Lär dig hur toodeploy från FTP- eller från källan styra databaser.</span><span class="sxs-lookup"><span data-stu-id="75833-161">[Deploy your code tooAzure App Service](web-sites-deploy.md)- Learn how toodeploy from FTP or from source control repositories.</span></span>
* <span data-ttu-id="75833-162">[Lägg till funktioner tooyour första webbapp](app-service-web-get-started-2.md) -ta din Azure-app toohello nästa nivå.</span><span class="sxs-lookup"><span data-stu-id="75833-162">[Add functionality tooyour first web app](app-service-web-get-started-2.md) - Take your Azure app toohello next level.</span></span> <span data-ttu-id="75833-163">Autentisera användarna.</span><span class="sxs-lookup"><span data-stu-id="75833-163">Authenticate your users.</span></span> <span data-ttu-id="75833-164">Skala den på begäran.</span><span class="sxs-lookup"><span data-stu-id="75833-164">Scale it based on demand.</span></span> <span data-ttu-id="75833-165">Konfigurera prestandavarningar.</span><span class="sxs-lookup"><span data-stu-id="75833-165">Set up some performance alerts.</span></span> <span data-ttu-id="75833-166">Allt med några få klickningar.</span><span class="sxs-lookup"><span data-stu-id="75833-166">All with a few clicks.</span></span>
