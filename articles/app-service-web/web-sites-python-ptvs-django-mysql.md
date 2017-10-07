---
title: "aaaDjango och MySQL på Azure med Python Tools 2.2 för Visual Studio"
description: "Lär dig hur toouse hello Python Tools för Visual Studio toocreate en Django-webbapp som lagrar data i en MySQL-databasinstans och distribuera den tooAzure App Service Web Apps."
services: app-service\web
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: c60a50b5-8b5e-4818-a442-16362273dabb
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: 1597c391d20c8e8ef629b4e4d05c9eb64c83bffc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="django-and-mysql-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="d199b-103">Django och MySQL på Azure med Python Tools 2.2 för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d199b-103">Django and MySQL on Azure with Python Tools 2.2 for Visual Studio</span></span>
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

<span data-ttu-id="d199b-104">I den här kursen ska du använda [Python Tools för Visual Studio](https://www.visualstudio.com/vs/python) toocreate ett enkelt avfrågar webbapp med någon av exempelmallarna i hello PTVS.</span><span class="sxs-lookup"><span data-stu-id="d199b-104">In this tutorial, you'll use [Python Tools for Visual Studio](https://www.visualstudio.com/vs/python) toocreate a simple polls web app using one of hello PTVS sample templates.</span></span> <span data-ttu-id="d199b-105">Du lär dig hur toouse en MySQL-tjänst finns i Azure, hur tooconfigure hello web app toouse MySQL och hur toopublish hello webbprogrammet för[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="d199b-105">You'll learn how toouse a MySQL service hosted on Azure, how tooconfigure hello web app toouse MySQL, and how toopublish hello web app too[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

> [!NOTE]
> <span data-ttu-id="d199b-106">hello informationen i den här kursen finns också i följande video hello:</span><span class="sxs-lookup"><span data-stu-id="d199b-106">hello information contained in this tutorial is also available in hello following video:</span></span>
> 
> <span data-ttu-id="d199b-107">[PTVS 2.1: Django-app med MySQL][video]</span><span class="sxs-lookup"><span data-stu-id="d199b-107">[PTVS 2.1: Django app with MySQL][video]</span></span>
> 
> 

<span data-ttu-id="d199b-108">Se hello [Python Developer Center] fler artiklar om utvecklingen av Azure App Service Web Apps med PTVS med hjälp av Bottle, Flask och Django webbramverken med Azure Table Storage, MySQL och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="d199b-108">See hello [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with Azure Table Storage, MySQL, and SQL Database services.</span></span> <span data-ttu-id="d199b-109">Den här artikeln fokuserar på App Service, hello stegen är liknande när du utvecklar [Azure Cloud Services].</span><span class="sxs-lookup"><span data-stu-id="d199b-109">While this article focuses on App Service, hello steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d199b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d199b-110">Prerequisites</span></span>
* <span data-ttu-id="d199b-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="d199b-111">Visual Studio 2015</span></span>
* <span data-ttu-id="d199b-112">[Python 2.7 32-bitars] eller [Python 3.4 32-bitars]</span><span class="sxs-lookup"><span data-stu-id="d199b-112">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>
* <span data-ttu-id="d199b-113">[Python Tools 2.2 för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="d199b-113">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="d199b-114">[Python Tools 2.2 för Visual Studio Samples VSIX]</span><span class="sxs-lookup"><span data-stu-id="d199b-114">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="d199b-115">[Azure SDK Tools för VS 2015]</span><span class="sxs-lookup"><span data-stu-id="d199b-115">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="d199b-116">Django 1.9 eller senare</span><span class="sxs-lookup"><span data-stu-id="d199b-116">Django 1.9 or later</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of hello hello previous include. -->

> [!NOTE]
> <span data-ttu-id="d199b-117">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="d199b-117">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="d199b-118">Inget kreditkort krävs och du behöver inte göra några åtaganden.</span><span class="sxs-lookup"><span data-stu-id="d199b-118">No credit card is required, and no commitments are necessary.</span></span>
> 
> 

## <a name="create-hello-project"></a><span data-ttu-id="d199b-119">Skapa hello projekt</span><span class="sxs-lookup"><span data-stu-id="d199b-119">Create hello Project</span></span>
<span data-ttu-id="d199b-120">I det här avsnittet får du skapa ett Visual Studio-projekt utifrån en exempelmall.</span><span class="sxs-lookup"><span data-stu-id="d199b-120">In this section, you'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="d199b-121">Du får skapa en virtuell miljö och installera nödvändiga paket.</span><span class="sxs-lookup"><span data-stu-id="d199b-121">You'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="d199b-122">Du får skapa en lokal databas med hjälp av sqlite.</span><span class="sxs-lookup"><span data-stu-id="d199b-122">You'll create a local database using sqlite.</span></span> <span data-ttu-id="d199b-123">Du måste köra hello programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="d199b-123">Then you'll run hello application locally.</span></span>

1. <span data-ttu-id="d199b-124">I Visual Studio väljer du **Arkiv**, **Nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="d199b-124">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="d199b-125">Hej projektmallar från hello [Python Tools 2.2 för Visual Studio Samples VSI] är tillgängliga under **Python**, **exempel**.</span><span class="sxs-lookup"><span data-stu-id="d199b-125">hello project templates from hello [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="d199b-126">Välj **Polls Django Web Project** och klicka på OK toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="d199b-126">Select **Polls Django Web Project** and click OK toocreate hello project.</span></span>
   
    ![Dialogrutan Nytt projekt](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)
3. <span data-ttu-id="d199b-128">Du kommer att tillfrågas tooinstall externa paket.</span><span class="sxs-lookup"><span data-stu-id="d199b-128">You will be prompted tooinstall external packages.</span></span> <span data-ttu-id="d199b-129">Välj **Install into a virtual environment** (Installera i en virtuell miljö).</span><span class="sxs-lookup"><span data-stu-id="d199b-129">Select **Install into a virtual environment**.</span></span>
   
    ![Dialogrutan Externa paket](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)
4. <span data-ttu-id="d199b-131">Välj **Python 2.7** eller **Python 3.4** som hello bastolk.</span><span class="sxs-lookup"><span data-stu-id="d199b-131">Select **Python 2.7** or **Python 3.4** as hello base interpreter.</span></span>
   
    ![Dialogrutan Add Virtual Environment (Lägg till virtuell miljö)](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="d199b-133">I **Solution Explorer**, högerklicka på hello projektnoden och väljer **Python**, och välj sedan **Django migrera**.</span><span class="sxs-lookup"><span data-stu-id="d199b-133">In **Solution Explorer**, right-click on hello project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="d199b-134">Välj därefter **Django – skapa superanvändare**.</span><span class="sxs-lookup"><span data-stu-id="d199b-134">Then select **Django Create Superuser**.</span></span>
6. <span data-ttu-id="d199b-135">Detta öppnar Django Management Console och skapa en sqlite-databas i hello projektmappen.</span><span class="sxs-lookup"><span data-stu-id="d199b-135">This will open a Django Management Console and create a sqlite database in hello project folder.</span></span> <span data-ttu-id="d199b-136">Följ hello prompter toocreate en användare.</span><span class="sxs-lookup"><span data-stu-id="d199b-136">Follow hello prompts toocreate a user.</span></span>
7. <span data-ttu-id="d199b-137">Bekräfta att hello programmet fungerar genom att trycka på `F5`.</span><span class="sxs-lookup"><span data-stu-id="d199b-137">Confirm that hello application works by pressing `F5`.</span></span>
8. <span data-ttu-id="d199b-138">Klicka på **logga in** från hello navigeringsfältet hello överst.</span><span class="sxs-lookup"><span data-stu-id="d199b-138">Click **Log in** from hello navigation bar at hello top.</span></span>
   
    ![Navigeringsfältet i Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)
9. <span data-ttu-id="d199b-140">Ange hello autentiseringsuppgifter för hello användaren du skapade när du synkroniserade hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="d199b-140">Enter hello credentials for hello user you created when you synchronized hello database.</span></span>
   
    ![Inloggningsformulär](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)
10. <span data-ttu-id="d199b-142">Klicka på **Create Sample Polls** (Skapa exempelomröstningar).</span><span class="sxs-lookup"><span data-stu-id="d199b-142">Click **Create Sample Polls**.</span></span>
    
     ![Create Sample Polls (Skapa exempelomröstningar)](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)
11. <span data-ttu-id="d199b-144">Klicka på en omröstning och rösta.</span><span class="sxs-lookup"><span data-stu-id="d199b-144">Click on a poll and vote.</span></span>
    
     ![Rösta i exempelomröstningar](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-mysql-database"></a><span data-ttu-id="d199b-146">Skapa en MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="d199b-146">Create a MySQL Database</span></span>
<span data-ttu-id="d199b-147">Hello-databas ska du skapa en värdbaserad ClearDB MySQL-databas på Azure.</span><span class="sxs-lookup"><span data-stu-id="d199b-147">For hello database, you'll create a ClearDB MySQL hosted database on Azure.</span></span>

<span data-ttu-id="d199b-148">Du kan också välja att skapa din egen virtuella dator som körs i Azure och sedan installera och administrera MySQL själv.</span><span class="sxs-lookup"><span data-stu-id="d199b-148">As an alternative, you can create your own Virtual Machine running in Azure, then install and administer MySQL yourself.</span></span>

<span data-ttu-id="d199b-149">Du kan skapa en databas med en kostnadsfri plan genom att följa dessa steg.</span><span class="sxs-lookup"><span data-stu-id="d199b-149">You can create a database with a free plan by following these steps.</span></span>

1. <span data-ttu-id="d199b-150">Logga in toohello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="d199b-150">Log in toohello [Azure Portal].</span></span>
2. <span data-ttu-id="d199b-151">Hello överkant hello navigeringsfönstret, klickar du på **ny**, klicka på **Data + lagring**, och klicka sedan på **MySQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="d199b-151">At hello Top of hello navigation pane, click **NEW**, then click **Data + Storage**, and then click **MySQL Database**.</span></span>
3. <span data-ttu-id="d199b-152">Konfigurera hello ny MySQL-databas genom att skapa en ny resursgrupp och välj hello lämplig plats för den.</span><span class="sxs-lookup"><span data-stu-id="d199b-152">Configure hello new MySQL database by creating a new resource group and select hello appropriate location for it.</span></span>
4. <span data-ttu-id="d199b-153">När hello MySQL-databas har skapats klickar du på **egenskaper** i hello databasbladet.</span><span class="sxs-lookup"><span data-stu-id="d199b-153">Once hello MySQL database is created, click **Properties** in hello database blade.</span></span>
5. <span data-ttu-id="d199b-154">Använda hello Kopiera knappen tooput hello värdet **ANSLUTNINGSSTRÄNGEN** hello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="d199b-154">Use hello copy button tooput hello value of **CONNECTION STRING** on hello clipboard.</span></span>

## <a name="configure-hello-project"></a><span data-ttu-id="d199b-155">Konfigurera hello projekt</span><span class="sxs-lookup"><span data-stu-id="d199b-155">Configure hello Project</span></span>
<span data-ttu-id="d199b-156">I det här avsnittet får du konfigurera våra web app toouse hello MySQL-databasen du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="d199b-156">In this section, you'll configure our web app toouse hello MySQL database you just created.</span></span> <span data-ttu-id="d199b-157">Du måste också installera ytterligare Python-paket krävs toouse MySQL-databaser med Django.</span><span class="sxs-lookup"><span data-stu-id="d199b-157">You'll also install additional Python packages required toouse MySQL databases with Django.</span></span> <span data-ttu-id="d199b-158">Du måste köra hello webbappen lokalt.</span><span class="sxs-lookup"><span data-stu-id="d199b-158">Then you'll run hello web app locally.</span></span>

1. <span data-ttu-id="d199b-159">Öppna i Visual Studio **settings.py**, från hello *projektnamn* mapp.</span><span class="sxs-lookup"><span data-stu-id="d199b-159">In Visual Studio, open **settings.py**, from hello *ProjectName* folder.</span></span> <span data-ttu-id="d199b-160">Klistra in hello anslutningssträngen i hello-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="d199b-160">Temporarily paste hello connection string in hello editor.</span></span> <span data-ttu-id="d199b-161">hello anslutningssträngen är i formatet:</span><span class="sxs-lookup"><span data-stu-id="d199b-161">hello connection string is in this format:</span></span>
   
        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>
   
    <span data-ttu-id="d199b-162">Ändra hello standarddatabasen **motorn** toouse MySQL och ange hello värden för **namn**, **användare**, **lösenord** och ** VÄRDEN** från hello **CONNECTIONSTRING**.</span><span class="sxs-lookup"><span data-stu-id="d199b-162">Change hello default database **ENGINE** toouse MySQL, and set hello values for **NAME**, **USER**, **PASSWORD** and **HOST** from hello **CONNECTIONSTRING**.</span></span>
   
        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.mysql',
                'NAME': '<Database>',
                'USER': '<User Id>',
                'PASSWORD': '<Password>',
                'HOST': '<Data Source>',
                'PORT': '',
            }
        }
2. <span data-ttu-id="d199b-163">I Solution Explorer under **Python-miljöer**, högerklicka på hello virtuella miljön och välj **Install Python Package**.</span><span class="sxs-lookup"><span data-stu-id="d199b-163">In Solution Explorer, under **Python Environments**, right-click on hello virtual environment and select **Install Python Package**.</span></span>
3. <span data-ttu-id="d199b-164">Installera hello paketet `mysqlclient` med **pip**.</span><span class="sxs-lookup"><span data-stu-id="d199b-164">Install hello package `mysqlclient` using **pip**.</span></span>
   
    ![Dialogrutan Installera paket](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)
4. <span data-ttu-id="d199b-166">I **Solution Explorer**, högerklicka på hello projektnoden och väljer **Python**, och välj sedan **Django migrera**.</span><span class="sxs-lookup"><span data-stu-id="d199b-166">In **Solution Explorer**, right-click on hello project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="d199b-167">Välj därefter **Django – skapa superanvändare**.</span><span class="sxs-lookup"><span data-stu-id="d199b-167">Then select **Django Create Superuser**.</span></span>
   
    <span data-ttu-id="d199b-168">Detta skapar hello tabeller för hello MySQL-databas som du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="d199b-168">This will create hello tables for hello MySQL database you created in hello previous section.</span></span> <span data-ttu-id="d199b-169">Följ hello prompter toocreate en användare som inte har toomatch hello användaren hello sqlite-databas som skapats i hello första delen av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="d199b-169">Follow hello prompts toocreate a user, which doesn't have toomatch hello user in hello sqlite database created in hello first section of this article.</span></span>
5. <span data-ttu-id="d199b-170">Kör programmet hello med `F5`.</span><span class="sxs-lookup"><span data-stu-id="d199b-170">Run hello application with `F5`.</span></span> <span data-ttu-id="d199b-171">Omröstningar som skapas med **skapa Exempelomröstningar** och hello-data som skickats vid röstning serialiseras i hello MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="d199b-171">Polls that are created with **Create Sample Polls** and hello data submitted by voting will be serialized in hello MySQL database.</span></span>

## <a name="publish-hello-web-app-tooazure-app-service"></a><span data-ttu-id="d199b-172">Publicera hello web app tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="d199b-172">Publish hello web app tooAzure App Service</span></span>
<span data-ttu-id="d199b-173">hello Azure .NET SDK innehåller ett enkelt sätt toodeploy din web app tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="d199b-173">hello Azure .NET SDK provides an easy way toodeploy your web app tooAzure App Service.</span></span>

1. <span data-ttu-id="d199b-174">I **Solution Explorer**, högerklicka på hello projektnoden och väljer **publicera**.</span><span class="sxs-lookup"><span data-stu-id="d199b-174">In **Solution Explorer**, right-click on hello project node and select **Publish**.</span></span>
   
    ![Dialogrutan Publicera webbplats](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="d199b-176">Klicka på **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="d199b-176">Click on **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="d199b-177">Klicka på **ny** toocreate ett nytt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d199b-177">Click on **New** toocreate a new web app.</span></span>
4. <span data-ttu-id="d199b-178">Fyll i följande fält hello och på **skapa**:</span><span class="sxs-lookup"><span data-stu-id="d199b-178">Fill in hello following fields and click **Create**:</span></span>
   
   * <span data-ttu-id="d199b-179">**Webbappens namn**</span><span class="sxs-lookup"><span data-stu-id="d199b-179">**Web App name**</span></span>
   * <span data-ttu-id="d199b-180">**App Service-plan**</span><span class="sxs-lookup"><span data-stu-id="d199b-180">**App Service plan**</span></span>
   * <span data-ttu-id="d199b-181">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d199b-181">**Resource group**</span></span>
   * <span data-ttu-id="d199b-182">**Region**</span><span class="sxs-lookup"><span data-stu-id="d199b-182">**Region**</span></span>
   * <span data-ttu-id="d199b-183">Lämna **databasserver** ställa in också**ingen databas**</span><span class="sxs-lookup"><span data-stu-id="d199b-183">Leave **Database server** set too**No database**</span></span>
5. <span data-ttu-id="d199b-184">Godkänn alla standardvärden och klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="d199b-184">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="d199b-185">Webbläsaren öppnas automatiskt toohello publicerade webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d199b-185">Your web browser will open automatically toohello published web app.</span></span> <span data-ttu-id="d199b-186">Du bör se hello web app fungerar som förväntat, med hjälp av hello **MySQL** databasen på Azure.</span><span class="sxs-lookup"><span data-stu-id="d199b-186">You should see hello web app working as expected, using hello **MySQL** database hosted on Azure.</span></span>
   
    ![Webbläsare](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)
   
    <span data-ttu-id="d199b-188">Grattis!</span><span class="sxs-lookup"><span data-stu-id="d199b-188">Congratulations!</span></span> <span data-ttu-id="d199b-189">Du har publicerat din MySQL-baserade web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d199b-189">You have successfully published your MySQL-based web app tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d199b-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d199b-190">Next steps</span></span>
<span data-ttu-id="d199b-191">Följ dessa länkar toolearn mer om Python Tools för Visual Studio, Django och MySQL.</span><span class="sxs-lookup"><span data-stu-id="d199b-191">Follow these links toolearn more about Python Tools for Visual Studio, Django and MySQL.</span></span>

* <span data-ttu-id="d199b-192">[Dokumentation för Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="d199b-192">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="d199b-193">[Webbprojekt]</span><span class="sxs-lookup"><span data-stu-id="d199b-193">[Web Projects]</span></span>
  * <span data-ttu-id="d199b-194">[Molntjänstprojekt]</span><span class="sxs-lookup"><span data-stu-id="d199b-194">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="d199b-195">[Fjärrfelsökning i Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="d199b-195">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="d199b-196">[Django-dokumentation]</span><span class="sxs-lookup"><span data-stu-id="d199b-196">[Django Documentation]</span></span>
* <span data-ttu-id="d199b-197">[MySQL]</span><span class="sxs-lookup"><span data-stu-id="d199b-197">[MySQL]</span></span>

<span data-ttu-id="d199b-198">Mer information finns i hello [Python Developer Center](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="d199b-198">For more information, see hello [Python Developer Center](/develop/python/).</span></span>

<!--Link references-->

[Python Developer Center]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->

[Azure-portalen]: https://portal.azure.com
[Python Tools for Visual Studio]: https://www.visualstudio.com/vs/python/
[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 för Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools för VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517191
[Dokumentation för Python Tools för Visual Studio]: http://aka.ms/ptvsdocs
[Fjärrfelsökning i Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webbprojekt]: http://go.microsoft.com/fwlink/?LinkId=624027
[Molntjänstprojekt]: http://go.microsoft.com/fwlink/?LinkId=624028
[Django-dokumentation]: https://www.djangoproject.com/
[MySQL]: http://www.mysql.com/
[video]: http://youtu.be/oKCApIrS0Lo
