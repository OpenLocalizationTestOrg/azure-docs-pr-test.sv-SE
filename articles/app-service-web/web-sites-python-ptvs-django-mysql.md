---
title: "Django och MySQL på Azure med Python Tools 2.2 för Visual Studio"
description: "Läs om hur du använder Python Tools för Visual Studio till att skapa en Django-webbapp som lagrar data i en MySQL-databasinstans och distribuera den till webbappar i Azure App Service Web Apps."
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
ms.openlocfilehash: fd85337ecdc638a4c18065a0ce94f697da8197f1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="django-and-mysql-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="94d02-103">Django och MySQL på Azure med Python Tools 2.2 för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="94d02-103">Django and MySQL on Azure with Python Tools 2.2 for Visual Studio</span></span>
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

<span data-ttu-id="94d02-104">I den här kursen ska du använda [Python Tools för Visual Studio](https://www.visualstudio.com/vs/python) för att skapa en enkel webbapp för omröstning med någon av exempelmallarna i PTVS.</span><span class="sxs-lookup"><span data-stu-id="94d02-104">In this tutorial, you'll use [Python Tools for Visual Studio](https://www.visualstudio.com/vs/python) to create a simple polls web app using one of the PTVS sample templates.</span></span> <span data-ttu-id="94d02-105">Du får lära dig hur du använder en MySQL-tjänst som finns i Azure, hur du konfigurerar webbappen att använda MySQL och hur du publicerar webbappen till [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="94d02-105">You'll learn how to use a MySQL service hosted on Azure, how to configure the web app to use MySQL, and how to publish the web app to [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

> [!NOTE]
> <span data-ttu-id="94d02-106">Informationen i den här kursen finns också i följande video:</span><span class="sxs-lookup"><span data-stu-id="94d02-106">The information contained in this tutorial is also available in the following video:</span></span>
> 
> <span data-ttu-id="94d02-107">[PTVS 2.1: Django-app med MySQL][video]</span><span class="sxs-lookup"><span data-stu-id="94d02-107">[PTVS 2.1: Django app with MySQL][video]</span></span>
> 
> 

<span data-ttu-id="94d02-108">I [Python Developer Center] finns flera artiklar om att utveckla appar i Azure App Service Web Apps med PTVS med hjälp av webbramverken Bottle, Flask och Django med tjänsterna Azure Table Storage, MySQL och SQL Database.</span><span class="sxs-lookup"><span data-stu-id="94d02-108">See the [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with Azure Table Storage, MySQL, and SQL Database services.</span></span> <span data-ttu-id="94d02-109">Den här artikeln fokuserar på App Service, men stegen är ungefär desamma för att utveckla [Azure Cloud Services].</span><span class="sxs-lookup"><span data-stu-id="94d02-109">While this article focuses on App Service, the steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="94d02-110">Krav</span><span class="sxs-lookup"><span data-stu-id="94d02-110">Prerequisites</span></span>
* <span data-ttu-id="94d02-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="94d02-111">Visual Studio 2015</span></span>
* <span data-ttu-id="94d02-112">[Python 2.7 32-bitars] eller [Python 3.4 32-bitars]</span><span class="sxs-lookup"><span data-stu-id="94d02-112">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>
* <span data-ttu-id="94d02-113">[Python Tools 2.2 för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="94d02-113">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="94d02-114">[Python Tools 2.2 för Visual Studio Samples VSIX]</span><span class="sxs-lookup"><span data-stu-id="94d02-114">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="94d02-115">[Azure SDK Tools för VS 2015]</span><span class="sxs-lookup"><span data-stu-id="94d02-115">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="94d02-116">Django 1.9 eller senare</span><span class="sxs-lookup"><span data-stu-id="94d02-116">Django 1.9 or later</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of the the previous include. -->

> [!NOTE]
> <span data-ttu-id="94d02-117">Om du vill komma igång med Azure App Service innan du registrerar dig för ett Azure-konto kan du gå till [Prova App Service](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="94d02-117">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="94d02-118">Inget kreditkort krävs och du behöver inte göra några åtaganden.</span><span class="sxs-lookup"><span data-stu-id="94d02-118">No credit card is required, and no commitments are necessary.</span></span>
> 
> 

## <a name="create-the-project"></a><span data-ttu-id="94d02-119">Skapa projektet</span><span class="sxs-lookup"><span data-stu-id="94d02-119">Create the Project</span></span>
<span data-ttu-id="94d02-120">I det här avsnittet får du skapa ett Visual Studio-projekt utifrån en exempelmall.</span><span class="sxs-lookup"><span data-stu-id="94d02-120">In this section, you'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="94d02-121">Du får skapa en virtuell miljö och installera nödvändiga paket.</span><span class="sxs-lookup"><span data-stu-id="94d02-121">You'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="94d02-122">Du får skapa en lokal databas med hjälp av sqlite.</span><span class="sxs-lookup"><span data-stu-id="94d02-122">You'll create a local database using sqlite.</span></span> <span data-ttu-id="94d02-123">Sedan får du köra programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="94d02-123">Then you'll run the application locally.</span></span>

1. <span data-ttu-id="94d02-124">I Visual Studio väljer du **Arkiv**, **Nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="94d02-124">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="94d02-125">Du hittar projektmallarna från [Python Tools 2.2 för Visual Studio Samples VSIX] under **Python**, **Exempel**.</span><span class="sxs-lookup"><span data-stu-id="94d02-125">The project templates from the [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="94d02-126">Välj **Polls Django Web Project** (Omröstningar, Django-webbprojekt) och skapa projektet genom att klicka på OK.</span><span class="sxs-lookup"><span data-stu-id="94d02-126">Select **Polls Django Web Project** and click OK to create the project.</span></span>
   
    ![Dialogrutan Nytt projekt](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)
3. <span data-ttu-id="94d02-128">Du uppmanas att installera externa paket.</span><span class="sxs-lookup"><span data-stu-id="94d02-128">You will be prompted to install external packages.</span></span> <span data-ttu-id="94d02-129">Välj **Install into a virtual environment** (Installera i en virtuell miljö).</span><span class="sxs-lookup"><span data-stu-id="94d02-129">Select **Install into a virtual environment**.</span></span>
   
    ![Dialogrutan Externa paket](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)
4. <span data-ttu-id="94d02-131">Välj **Python 2.7** eller **Python 3.4** som bastolk.</span><span class="sxs-lookup"><span data-stu-id="94d02-131">Select **Python 2.7** or **Python 3.4** as the base interpreter.</span></span>
   
    ![Dialogrutan Add Virtual Environment (Lägg till virtuell miljö)](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="94d02-133">Högerklickar på projektnoden i **Solution Explorer** och välj först **Python** och sedan **Django Migrate**.</span><span class="sxs-lookup"><span data-stu-id="94d02-133">In **Solution Explorer**, right-click on the project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="94d02-134">Välj därefter **Django – skapa superanvändare**.</span><span class="sxs-lookup"><span data-stu-id="94d02-134">Then select **Django Create Superuser**.</span></span>
6. <span data-ttu-id="94d02-135">Öppnar Django Management Console och en sqlite-databas skapas i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="94d02-135">This will open a Django Management Console and create a sqlite database in the project folder.</span></span> <span data-ttu-id="94d02-136">Skapa en användare genom att följa anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="94d02-136">Follow the prompts to create a user.</span></span>
7. <span data-ttu-id="94d02-137">Bekräfta att programmet fungerar genom att trycka på `F5`.</span><span class="sxs-lookup"><span data-stu-id="94d02-137">Confirm that the application works by pressing `F5`.</span></span>
8. <span data-ttu-id="94d02-138">Klicka på **Log in** (Logga in) i navigeringsfältet längst upp.</span><span class="sxs-lookup"><span data-stu-id="94d02-138">Click **Log in** from the navigation bar at the top.</span></span>
   
    ![Navigeringsfältet i Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)
9. <span data-ttu-id="94d02-140">Ange autentiseringsuppgifterna för användaren du skapade när du synkroniserade databasen.</span><span class="sxs-lookup"><span data-stu-id="94d02-140">Enter the credentials for the user you created when you synchronized the database.</span></span>
   
    ![Inloggningsformulär](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)
10. <span data-ttu-id="94d02-142">Klicka på **Create Sample Polls** (Skapa exempelomröstningar).</span><span class="sxs-lookup"><span data-stu-id="94d02-142">Click **Create Sample Polls**.</span></span>
    
     ![Create Sample Polls (Skapa exempelomröstningar)](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)
11. <span data-ttu-id="94d02-144">Klicka på en omröstning och rösta.</span><span class="sxs-lookup"><span data-stu-id="94d02-144">Click on a poll and vote.</span></span>
    
     ![Rösta i exempelomröstningar](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-mysql-database"></a><span data-ttu-id="94d02-146">Skapa en MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="94d02-146">Create a MySQL Database</span></span>
<span data-ttu-id="94d02-147">Som databas skapar du en ClearDB MySQL-baserad databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="94d02-147">For the database, you'll create a ClearDB MySQL hosted database on Azure.</span></span>

<span data-ttu-id="94d02-148">Du kan också välja att skapa din egen virtuella dator som körs i Azure och sedan installera och administrera MySQL själv.</span><span class="sxs-lookup"><span data-stu-id="94d02-148">As an alternative, you can create your own Virtual Machine running in Azure, then install and administer MySQL yourself.</span></span>

<span data-ttu-id="94d02-149">Du kan skapa en databas med en kostnadsfri plan genom att följa dessa steg.</span><span class="sxs-lookup"><span data-stu-id="94d02-149">You can create a database with a free plan by following these steps.</span></span>

1. <span data-ttu-id="94d02-150">Logga in på [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="94d02-150">Log in to the [Azure Portal].</span></span>
2. <span data-ttu-id="94d02-151">Överst i navigeringsfönstret klickar du på **Nytt**, **Data + Storage** och sedan på **MySQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="94d02-151">At the Top of the navigation pane, click **NEW**, then click **Data + Storage**, and then click **MySQL Database**.</span></span>
3. <span data-ttu-id="94d02-152">Konfigurera den nya MySQL-databasen genom att skapa en ny resursgrupp och välja en lämplig plats för den.</span><span class="sxs-lookup"><span data-stu-id="94d02-152">Configure the new MySQL database by creating a new resource group and select the appropriate location for it.</span></span>
4. <span data-ttu-id="94d02-153">När MySQL-databasen har skapats klickar du på **Egenskaper** i databasbladet.</span><span class="sxs-lookup"><span data-stu-id="94d02-153">Once the MySQL database is created, click **Properties** in the database blade.</span></span>
5. <span data-ttu-id="94d02-154">Använd kopieringsknappen till att ange värdet för **CONNECTION STRING** (anslutningssträng) i Urklipp.</span><span class="sxs-lookup"><span data-stu-id="94d02-154">Use the copy button to put the value of **CONNECTION STRING** on the clipboard.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="94d02-155">Konfigurera projektet</span><span class="sxs-lookup"><span data-stu-id="94d02-155">Configure the Project</span></span>
<span data-ttu-id="94d02-156">I det här avsnittet får du konfigurera webbappen till att använda MySQL-databasen du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="94d02-156">In this section, you'll configure our web app to use the MySQL database you just created.</span></span> <span data-ttu-id="94d02-157">Du måste också installera ytterligare Python-paket som krävs för att använda MySQL-databaser med Django.</span><span class="sxs-lookup"><span data-stu-id="94d02-157">You'll also install additional Python packages required to use MySQL databases with Django.</span></span> <span data-ttu-id="94d02-158">Sedan kör du webbappen lokalt.</span><span class="sxs-lookup"><span data-stu-id="94d02-158">Then you'll run the web app locally.</span></span>

1. <span data-ttu-id="94d02-159">I Visual Studio öppnar du **settings.py** i mappen *Projektnamn*.</span><span class="sxs-lookup"><span data-stu-id="94d02-159">In Visual Studio, open **settings.py**, from the *ProjectName* folder.</span></span> <span data-ttu-id="94d02-160">Klistra in anslutningssträngen i redigeringsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="94d02-160">Temporarily paste the connection string in the editor.</span></span> <span data-ttu-id="94d02-161">Anslutningssträngen har det här formatet:</span><span class="sxs-lookup"><span data-stu-id="94d02-161">The connection string is in this format:</span></span>
   
        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>
   
    <span data-ttu-id="94d02-162">Ändra standarddatabasens **ENGINE** (motor) så att MySQL används och ange värden för **NAME**, **USER**, **PASSWORD** och **HOST** (Namn, användare, lösenord och värd) från **CONNECTIONSTRING** (Anslutningssträngen).</span><span class="sxs-lookup"><span data-stu-id="94d02-162">Change the default database **ENGINE** to use MySQL, and set the values for **NAME**, **USER**, **PASSWORD** and **HOST** from the **CONNECTIONSTRING**.</span></span>
   
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
2. <span data-ttu-id="94d02-163">I Solution Explorer under **Python Environments** (Python-miljöer), högerklickar du på den virtuella miljön och väljer **Install Python Package** (Installera Python-paket).</span><span class="sxs-lookup"><span data-stu-id="94d02-163">In Solution Explorer, under **Python Environments**, right-click on the virtual environment and select **Install Python Package**.</span></span>
3. <span data-ttu-id="94d02-164">Installera `mysqlclient`-paketet med **pip**.</span><span class="sxs-lookup"><span data-stu-id="94d02-164">Install the package `mysqlclient` using **pip**.</span></span>
   
    ![Dialogrutan Installera paket](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)
4. <span data-ttu-id="94d02-166">Högerklickar på projektnoden i **Solution Explorer** och välj först **Python** och sedan **Django Migrate**.</span><span class="sxs-lookup"><span data-stu-id="94d02-166">In **Solution Explorer**, right-click on the project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="94d02-167">Välj därefter **Django – skapa superanvändare**.</span><span class="sxs-lookup"><span data-stu-id="94d02-167">Then select **Django Create Superuser**.</span></span>
   
    <span data-ttu-id="94d02-168">När du gör det skapas tabellerna för MySQL-databasen du skapade i det föregående steget.</span><span class="sxs-lookup"><span data-stu-id="94d02-168">This will create the tables for the MySQL database you created in the previous section.</span></span> <span data-ttu-id="94d02-169">Följ anvisningarna för att skapa en användare som inte behöver matcha användaren i sqlite-databasen som skapades i den första delen av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="94d02-169">Follow the prompts to create a user, which doesn't have to match the user in the sqlite database created in the first section of this article.</span></span>
5. <span data-ttu-id="94d02-170">Kör programmet med `F5`.</span><span class="sxs-lookup"><span data-stu-id="94d02-170">Run the application with `F5`.</span></span> <span data-ttu-id="94d02-171">Omröstningar som skapas med **Create Sample Polls** (Skapa exempelomröstningar) och de data som skickats vid röstning serialiseras i MySQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="94d02-171">Polls that are created with **Create Sample Polls** and the data submitted by voting will be serialized in the MySQL database.</span></span>

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="94d02-172">Publicera webbappen i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="94d02-172">Publish the web app to Azure App Service</span></span>
<span data-ttu-id="94d02-173">Med Azure .NET SDK kan du enkelt distribuera webbappen till Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="94d02-173">The Azure .NET SDK provides an easy way to deploy your web app to Azure App Service.</span></span>

1. <span data-ttu-id="94d02-174">I **Solution Explorer** högerklickar du på projektnoden och väljer **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="94d02-174">In **Solution Explorer**, right-click on the project node and select **Publish**.</span></span>
   
    ![Dialogrutan Publicera webbplats](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="94d02-176">Klicka på **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="94d02-176">Click on **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="94d02-177">Skapa en ny webbapp genom att klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="94d02-177">Click on **New** to create a new web app.</span></span>
4. <span data-ttu-id="94d02-178">Fyll i följande fält och klicka på **Skapa**:</span><span class="sxs-lookup"><span data-stu-id="94d02-178">Fill in the following fields and click **Create**:</span></span>
   
   * <span data-ttu-id="94d02-179">**Webbappens namn**</span><span class="sxs-lookup"><span data-stu-id="94d02-179">**Web App name**</span></span>
   * <span data-ttu-id="94d02-180">**App Service-plan**</span><span class="sxs-lookup"><span data-stu-id="94d02-180">**App Service plan**</span></span>
   * <span data-ttu-id="94d02-181">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="94d02-181">**Resource group**</span></span>
   * <span data-ttu-id="94d02-182">**Region**</span><span class="sxs-lookup"><span data-stu-id="94d02-182">**Region**</span></span>
   * <span data-ttu-id="94d02-183">Lämna **Databasserver** inställd på **Ingen databas**</span><span class="sxs-lookup"><span data-stu-id="94d02-183">Leave **Database server** set to **No database**</span></span>
5. <span data-ttu-id="94d02-184">Godkänn alla standardvärden och klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="94d02-184">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="94d02-185">Den publicerade webbappen öppnas automatiskt i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="94d02-185">Your web browser will open automatically to the published web app.</span></span> <span data-ttu-id="94d02-186">Du bör kunna se att webbappen fungerar som förväntat med hjälp av **MySQL**-databasen på Azure.</span><span class="sxs-lookup"><span data-stu-id="94d02-186">You should see the web app working as expected, using the **MySQL** database hosted on Azure.</span></span>
   
    ![Webbläsare](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)
   
    <span data-ttu-id="94d02-188">Grattis!</span><span class="sxs-lookup"><span data-stu-id="94d02-188">Congratulations!</span></span> <span data-ttu-id="94d02-189">Du har publicerat din MySQL-baserade webbapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="94d02-189">You have successfully published your MySQL-based web app to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="94d02-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="94d02-190">Next steps</span></span>
<span data-ttu-id="94d02-191">Via de här länkarna hittar du mer information om Python Tools för Visual Studio, Django och MySQL:</span><span class="sxs-lookup"><span data-stu-id="94d02-191">Follow these links to learn more about Python Tools for Visual Studio, Django and MySQL.</span></span>

* <span data-ttu-id="94d02-192">[Dokumentation för Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="94d02-192">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="94d02-193">[Webbprojekt]</span><span class="sxs-lookup"><span data-stu-id="94d02-193">[Web Projects]</span></span>
  * <span data-ttu-id="94d02-194">[Molntjänstprojekt]</span><span class="sxs-lookup"><span data-stu-id="94d02-194">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="94d02-195">[Fjärrfelsökning i Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="94d02-195">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="94d02-196">[Django-dokumentation]</span><span class="sxs-lookup"><span data-stu-id="94d02-196">[Django Documentation]</span></span>
* <span data-ttu-id="94d02-197">[MySQL]</span><span class="sxs-lookup"><span data-stu-id="94d02-197">[MySQL]</span></span>

<span data-ttu-id="94d02-198">Mer information finns i [Python Developer Center](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="94d02-198">For more information, see the [Python Developer Center](/develop/python/).</span></span>

<!--Link references-->

[Python Developer Center]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->

[Azure Portal]: https://portal.azure.com
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
