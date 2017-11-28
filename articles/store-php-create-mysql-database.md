---
title: aaaCreate och ansluta tooa MySQL-databas i Azure
description: "Lär dig hur toouse hello Azure portal toocreate en MySQL-databas och anslut sedan tooit från en PHP-webbapp i Azure."
documentationcenter: php
services: app-service\web
author: cephalin
manager: erikre
editor: 
tags: mysql
ms.assetid: 55465a9a-7e65-4fd9-8a65-dd83ee41f3e5
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;cephalin
ms.openlocfilehash: 3abc02f8887432625666afd847e9dc0c0a4db2e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-connect-tooa-mysql-database-in-azure"></a><span data-ttu-id="f30db-103">Skapa och ansluta tooa MySQL-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="f30db-103">Create and connect tooa MySQL database in Azure</span></span>
<span data-ttu-id="f30db-104">Den här kursen visar hur toocreate en MySQL-databas i hello [Azure-portalen](https://portal.azure.com) (providern är [ClearDB](http://www.cleardb.com/)) och hur tooconnect tooit från en PHP web app som körs i [Azure App Service](app-service/app-service-value-prop-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="f30db-104">This tutorial shows you how toocreate a MySQL database in hello [Azure portal](https://portal.azure.com) (provider is [ClearDB](http://www.cleardb.com/)) and how tooconnect tooit from a PHP web app running in [Azure App Service](app-service/app-service-value-prop-what-is.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f30db-105">Du kan också skapa en MySQL-databas som en del av en [Marketplace-appmallen](app-service-web/app-service-web-create-web-app-from-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="f30db-105">You can also create a MySQL database as part of a [Marketplace app template](app-service-web/app-service-web-create-web-app-from-marketplace.md).</span></span>
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a><span data-ttu-id="f30db-106">Skapa en MySQL-databas på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f30db-106">Create a MySQL database in Azure portal</span></span>
<span data-ttu-id="f30db-107">toocreate en MySQL-databas i hello Azure-portalen hello följande:</span><span class="sxs-lookup"><span data-stu-id="f30db-107">toocreate a MySQL database in hello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="f30db-108">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f30db-108">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f30db-109">Hello vänstra menyn klickar du på **ny** > **Data + lagring** > **MySQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="f30db-109">From hello left menu, click **New** > **Data + Storage** > **MySQL Database**.</span></span>

    ![Skapa en MySQL-databas i Azure - start](./media/store-php-create-mysql-database/create-db-1-start.png)
3. <span data-ttu-id="f30db-111">I hello ny MySQL-databas [bladet](azure-portal-overview.md), konfigurera den nya MySQL-databasen på följande sätt (*bladet*: en Portalsida som öppnar vågrätt):</span><span class="sxs-lookup"><span data-stu-id="f30db-111">In hello New MySQL Database [blade](azure-portal-overview.md), configure your new MySQL database as follows (*blade*: a portal page that opens horizontally):</span></span>

   * <span data-ttu-id="f30db-112">**Databasnamn**: Ange en unik namn</span><span class="sxs-lookup"><span data-stu-id="f30db-112">**Database Name**: Type a uniquely identifiable name</span></span>
   * <span data-ttu-id="f30db-113">**Prenumerationen**: Välj hello prenumeration toouse</span><span class="sxs-lookup"><span data-stu-id="f30db-113">**Subscription**: Choose hello subscription toouse</span></span>
   * <span data-ttu-id="f30db-114">**Typ av databas**: Välj **delade** för låg kostnad eller kostnadsfria nivåer eller **dedikerad** tooget särskilda resurser.</span><span class="sxs-lookup"><span data-stu-id="f30db-114">**Database Type**: Select **Shared** for low-cost or free tiers, or **Dedicated** tooget dedicated resources.</span></span>
   * <span data-ttu-id="f30db-115">**Resursgruppen**: Lägg till hello MySQL database tooan befintlig [resursgruppen](azure-resource-manager/resource-group-overview.md) eller placera den i en ny.</span><span class="sxs-lookup"><span data-stu-id="f30db-115">**Resource group**: Add hello MySQL database tooan existing [resource group](azure-resource-manager/resource-group-overview.md) or put it in a new one.</span></span> <span data-ttu-id="f30db-116">Resurser i samma grupp kan enkelt hanteras tillsammans hello.</span><span class="sxs-lookup"><span data-stu-id="f30db-116">Resources in hello same group can be easily managed together.</span></span>
   * <span data-ttu-id="f30db-117">**Plats**: Välj en plats Stäng tooyou.</span><span class="sxs-lookup"><span data-stu-id="f30db-117">**Location**: Select a location close tooyou.</span></span> <span data-ttu-id="f30db-118">När du lägger till tooan befintliga resursgruppen är låst toohello resursgruppens plats.</span><span class="sxs-lookup"><span data-stu-id="f30db-118">When adding tooan existing resource group, you're locked toohello resource group's location.</span></span>
   * <span data-ttu-id="f30db-119">**Prisnivån**: klickar du på **prisnivån**, Välj en prisnivå alternativ (**Merkurius** nivå är ledigt), och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="f30db-119">**Pricing Tier**: Click **Pricing Tier**, then select a pricing option (**Mercury** tier is free), and then click **Select**.</span></span>
   * <span data-ttu-id="f30db-120">**Juridiska villkor**: Klicka på **juridiska villkor**, granska hello inköpsinformation och på **köpa**.</span><span class="sxs-lookup"><span data-stu-id="f30db-120">**Legal Terms**: Click **Legal Terms**, review hello purchase details, and click **Purchase**.</span></span>
   * <span data-ttu-id="f30db-121">**PIN-kod toodashboard**: Välj om du vill tooaccess den direkt från hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f30db-121">**Pin toodashboard**: Select if you want tooaccess it directly from hello dashboard.</span></span> <span data-ttu-id="f30db-122">Detta är särskilt användbart om du inte är bekant med portalen navigering ännu.</span><span class="sxs-lookup"><span data-stu-id="f30db-122">This is especially helpful if you aren't familiar with portal navigation yet.</span></span>

     <span data-ttu-id="f30db-123">följande skärmbild hello är bara ett exempel på hur du kan konfigurera MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="f30db-123">hello following screenshot is just an example of how you can configure your MySQL database.</span></span>  
     ![Skapa en MySQL-databas i Azure - konfigurera](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. <span data-ttu-id="f30db-125">När du är klar konfigurerar, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="f30db-125">When you're done configuring, click **Create**.</span></span>

    ![Skapa en MySQL-databas i Azure - skapa](./media/store-php-create-mysql-database/create-db-3-create.png)

    <span data-ttu-id="f30db-127">Du ser ett popup-meddelande postmeddelande med information om att distributionen har startats.</span><span class="sxs-lookup"><span data-stu-id="f30db-127">You will see a pop-up notification letting you know that deployment has started.</span></span>

    ![Skapa en MySQL-databas i Azure - pågår](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    <span data-ttu-id="f30db-129">Du får en annan popup när distributionen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="f30db-129">You will get another pop-up once deployment has succeeded.</span></span> <span data-ttu-id="f30db-130">hello portalen öppnas automatiskt även ditt MySQL-databas-blad.</span><span class="sxs-lookup"><span data-stu-id="f30db-130">hello portal will also open your MySQL database blade automatically.</span></span>

<a name="connect"></a>

## <a name="connect-tooyour-mysql-database"></a><span data-ttu-id="f30db-131">Ansluta tooyour MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="f30db-131">Connect tooyour MySQL database</span></span>
<span data-ttu-id="f30db-132">toosee hello anslutningsinformationen för den nya MySQL-databasen, klicka bara på **egenskaper** i din webbapps blad.</span><span class="sxs-lookup"><span data-stu-id="f30db-132">toosee hello connection information for your new MySQL database, just click **Properties** in your web app's blade.</span></span>

![Skapa en MySQL-databas i Azure - bladet för MySQL-databas](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

<span data-ttu-id="f30db-134">Du kan nu använda att anslutningsinformationen i ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="f30db-134">You can now use that connection information in any web app.</span></span> <span data-ttu-id="f30db-135">Ett exempel som visar hur toouse hello anslutningsinformation från en enkel PHP-app är tillgänglig [här](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).</span><span class="sxs-lookup"><span data-stu-id="f30db-135">A sample that shows how toouse hello connection information from a simple PHP app is available [here](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).</span></span>

## <a name="connect-a-laravel-web-app-from-hello-php-get-started-tutorial"></a><span data-ttu-id="f30db-136">Ansluta en webbapp för Laravel (från hello PHP get igång-kursen)</span><span class="sxs-lookup"><span data-stu-id="f30db-136">Connect a Laravel web app (from hello PHP get started tutorial)</span></span>
<span data-ttu-id="f30db-137">Anta att du bara klar hello kursen [skapa, konfigurera och distribuera en PHP web app tooAzure](app-service-web/app-service-web-php-get-started.md) och har en [Laravel](https://www.laravel.com/) webbprogram som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="f30db-137">Suppose you just finished hello tutorial [Create, configure, and deploy a PHP web app tooAzure](app-service-web/app-service-web-php-get-started.md) and have a [Laravel](https://www.laravel.com/) web app running in Azure.</span></span> <span data-ttu-id="f30db-138">Du kan enkelt lägga till databasen funktioner tooyour Laravel app.</span><span class="sxs-lookup"><span data-stu-id="f30db-138">You can easily add database capabilities tooyour Laravel app.</span></span> <span data-ttu-id="f30db-139">Följ bara hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="f30db-139">Just follow hello steps below:</span></span>

> [!NOTE]
> <span data-ttu-id="f30db-140">hello följande steg förutsätter att du har slutfört hello kursen [skapa, konfigurera och distribuera en PHP web app tooAzure](app-service-web/app-service-web-php-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f30db-140">hello following steps assume that you have finished hello tutorial [Create, configure, and deploy a PHP web app tooAzure](app-service-web/app-service-web-php-get-started.md).</span></span>
>
>

1. <span data-ttu-id="f30db-141">Konfigurera hello Laravel app i din lokala development environment toopoint toohello MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="f30db-141">Configure hello Laravel app in your local development environment toopoint toohello MySQL database.</span></span> <span data-ttu-id="f30db-142">toodo detta, öppna `.env` från appen Laravel rotkatalogen och konfigurera alternativ för hello MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="f30db-142">toodo this, open `.env` from your Laravel app's root directory and configure hello MySQL database options.</span></span>

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

   > [!NOTE]
   > <span data-ttu-id="f30db-143">I hello **egenskaper** bladet hello namnet på din MySQL-databas kanske eller kanske inte hello en visas i hello **DATABASNAMNET** fältet.</span><span class="sxs-lookup"><span data-stu-id="f30db-143">In hello **Properties** blade, hello name of your MySQL database may or may not be hello one shown in hello **DATABASE NAME** field.</span></span> <span data-ttu-id="f30db-144">Det är bättre toocheck hello databasparameter i hello **ANSLUTNINGSSTRÄNGEN** fältet.</span><span class="sxs-lookup"><span data-stu-id="f30db-144">It's better toocheck hello Database parameter in hello **CONNECTION STRING** field.</span></span>    
   >
   > ![Skapa en MySQL-databas i Azure - pågår](./media/store-php-create-mysql-database/connect-db-1-database-name.png)
   >
   >
2. <span data-ttu-id="f30db-146">hello snabbaste sättet tooverify att du har MySQL åtkomst nu är toouse [scaffold-teknik för autentisering för Laravels standard](https://laravel.com/docs/5.2/authentication#authentication-quickstart).</span><span class="sxs-lookup"><span data-stu-id="f30db-146">hello quickest way tooverify that you have MySQL access now is toouse [Laravel's default authentication scaffolding](https://laravel.com/docs/5.2/authentication#authentication-quickstart).</span></span>
   <span data-ttu-id="f30db-147">Kör följande kommandon från Laravel appens rotkatalog hello i hello kommandoradsverktyget terminalen:</span><span class="sxs-lookup"><span data-stu-id="f30db-147">In hello command-line terminal, run hello following commands from your Laravel app's root directory:</span></span>

         php artisan migrate
         php artisan make:auth

    <span data-ttu-id="f30db-148">hello första kommandot skapar hello tabeller i Azure baserat på fördefinierade migreringar i hello `database/migrations` directory och hello andra kommandot autogenererar hello grundläggande vyerna och vägarna för användarregistrering och autentisering.</span><span class="sxs-lookup"><span data-stu-id="f30db-148">hello first command creates hello tables in Azure based on predefined migrations in hello `database/migrations` directory, and hello second  command scaffolds hello basic views and routes for user registration and authentication.</span></span>
3. <span data-ttu-id="f30db-149">Kör hello utvecklingsserver nu:</span><span class="sxs-lookup"><span data-stu-id="f30db-149">Run hello development server now:</span></span>

        php artisan serve
4. <span data-ttu-id="f30db-150">I hello webbläsare, navigera toohttp://localhost:8000 och registrera en ny användare som visas:</span><span class="sxs-lookup"><span data-stu-id="f30db-150">In hello browser, navigate toohttp://localhost:8000 and register a new user as shown:</span></span>

    ![Ansluta tooMySQL databas i Azure - registrera användare](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    <span data-ttu-id="f30db-152">Följ hello UI fråga fullständig hello registrering.</span><span class="sxs-lookup"><span data-stu-id="f30db-152">Follow hello UI prompt complete hello registration.</span></span> <span data-ttu-id="f30db-153">När registreringen är klar ska du vara inloggad.</span><span class="sxs-lookup"><span data-stu-id="f30db-153">Once registration completes, you will be logged in.</span></span>

    ![Ansluta tooMySQL databas i Azure - registrera användare](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    <span data-ttu-id="f30db-155">Du nu utvecklar en app mot hello MySQL-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="f30db-155">You are now developing your app against hello MySQL database in Azure.</span></span>
5. <span data-ttu-id="f30db-156">Nu behöver du bara tooreplicate din `.env` inställningar tooyour Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="f30db-156">Now, you just need tooreplicate your `.env` settings tooyour Azure web app.</span></span> <span data-ttu-id="f30db-157">Kör följande Azure CLI-kommandona hello:</span><span class="sxs-lookup"><span data-stu-id="f30db-157">Run hello following Azure CLI commands:</span></span>

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

6. <span data-ttu-id="f30db-158">Därefter genomför och skicka tooAzure hello lokala ändringar som gjorts tidigare när du kör `php artisan make:auth`.</span><span class="sxs-lookup"><span data-stu-id="f30db-158">Next, commit and push tooAzure hello local changes made earlier while running `php artisan make:auth`.</span></span>

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master
7. <span data-ttu-id="f30db-159">Bläddra toohello Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="f30db-159">Browse toohello Azure web app.</span></span>

        azure site browse
8. <span data-ttu-id="f30db-160">Logga in med hello-autentiseringsuppgifter som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="f30db-160">Log in using hello user credentials you created earlier.</span></span>

    ![Ansluta tooMySQL databas i Azure - Bläddra tooAzure webbprogram](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    <span data-ttu-id="f30db-162">När du loggar in, bör du se hello eget efter inloggningsskärmen.</span><span class="sxs-lookup"><span data-stu-id="f30db-162">After you log in, you should see hello friendly post-login screen.</span></span>

    ![Ansluta tooMySQL databas i Azure - inloggad](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    <span data-ttu-id="f30db-164">Grattis, PHP-webbapp i Azure är nu kommer åt data från din MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="f30db-164">Congratulations, your PHP web app in Azure is now accessing data from your MySQL database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f30db-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f30db-165">Next steps</span></span>
<span data-ttu-id="f30db-166">Mer information finns i hello [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="f30db-166">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>
