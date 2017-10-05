---
title: "Django och SQL Database på Azure med Python Tools 2.2 för Visual Studio"
description: "Lär dig hur du använder Python Tools för Visual Studio för att skapa en Django-webbapp som lagrar data i en SQL-databasinstans och distribuera den till Azure App Service Web Apps."
services: app-service\web
tags: python
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: 3a677e64-b5a9-4d43-b9c0-66246368b483
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: 65b59dee2b7bddca77d31c692dab713c68d67e24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="django-and-sql-database-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="37e6c-103">Django och SQL Database på Azure med Python Tools 2.2 för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37e6c-103">Django and SQL Database on Azure with Python Tools 2.2 for Visual Studio</span></span>
<span data-ttu-id="37e6c-104">I den här kursen använder vi [Python Tools för Visual Studio] du skapar ett enkelt avsökningar webbapp med någon av exempelmallarna i PTVS.</span><span class="sxs-lookup"><span data-stu-id="37e6c-104">In this tutorial, we'll use [Python Tools for Visual Studio] to create a simple polls web app using one of the PTVS sample templates.</span></span> <span data-ttu-id="37e6c-105">Den här kursen finns också tillgänglig som en [video](https://www.youtube.com/watch?v=ZwcoGcIeHF4).</span><span class="sxs-lookup"><span data-stu-id="37e6c-105">This tutorial is also available as a [video](https://www.youtube.com/watch?v=ZwcoGcIeHF4).</span></span>

<span data-ttu-id="37e6c-106">Vi lära dig hur du använder en SQL-databas i Azure, hur du konfigurerar webbprogram att använda en SQL-databas och hur du publicerar webbappen till [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="37e6c-106">We'll learn how to use a SQL database hosted on Azure, how to configure the web app to use a SQL database, and how to publish the web app to [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="37e6c-107">Finns det [Python Developer Center] fler artiklar om utvecklingen av Azure App Service Web Apps med PTVS med hjälp av Bottle, Flask och Django webbramverken med Azure Table Storage, MySQL och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="37e6c-107">See the [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with Azure Table Storage, MySQL and SQL Database services.</span></span> <span data-ttu-id="37e6c-108">Den här artikeln fokuserar på App Service, men stegen är ungefär desamma för att utveckla [Azure Cloud Services].</span><span class="sxs-lookup"><span data-stu-id="37e6c-108">While this article focuses on App Service, the steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37e6c-109">Krav</span><span class="sxs-lookup"><span data-stu-id="37e6c-109">Prerequisites</span></span>
* <span data-ttu-id="37e6c-110">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="37e6c-110">Visual Studio 2015</span></span>
* <span data-ttu-id="37e6c-111">[Python 2.7 32-bitars]</span><span class="sxs-lookup"><span data-stu-id="37e6c-111">[Python 2.7 32-bit]</span></span>
* <span data-ttu-id="37e6c-112">[Python Tools 2.2 för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="37e6c-112">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="37e6c-113">[Python Tools 2.2 för Visual Studio Samples VSIX]</span><span class="sxs-lookup"><span data-stu-id="37e6c-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="37e6c-114">[Azure SDK Tools för VS 2015]</span><span class="sxs-lookup"><span data-stu-id="37e6c-114">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="37e6c-115">Django 1.9 eller senare</span><span class="sxs-lookup"><span data-stu-id="37e6c-115">Django 1.9 or later</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="37e6c-116">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="37e6c-116">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="37e6c-117">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="37e6c-117">No credit cards required; no commitments.</span></span>
>
>

## <a name="create-the-project"></a><span data-ttu-id="37e6c-118">Skapa projektet</span><span class="sxs-lookup"><span data-stu-id="37e6c-118">Create the Project</span></span>
<span data-ttu-id="37e6c-119">I det här avsnittet skapar vi ett Visual Studio-projekt utifrån en exempelmall.</span><span class="sxs-lookup"><span data-stu-id="37e6c-119">In this section, we'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="37e6c-120">Vi skapa en virtuell miljö och installera nödvändiga paket.</span><span class="sxs-lookup"><span data-stu-id="37e6c-120">We'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="37e6c-121">Vi ska skapa en lokal databas med hjälp av sqlite.</span><span class="sxs-lookup"><span data-stu-id="37e6c-121">We'll create a local database using sqlite.</span></span> <span data-ttu-id="37e6c-122">Vi kommer kör webbappen lokalt.</span><span class="sxs-lookup"><span data-stu-id="37e6c-122">Then we'll run the web app locally.</span></span>

1. <span data-ttu-id="37e6c-123">I Visual Studio väljer du **Arkiv**, **Nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-123">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="37e6c-124">Du hittar projektmallarna från [Python Tools 2.2 för Visual Studio Samples VSIX] under **Python**, **Exempel**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-124">The project templates from the [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="37e6c-125">Välj **Polls Django Web Project** (Omröstningar, Django-webbprojekt) och skapa projektet genom att klicka på OK.</span><span class="sxs-lookup"><span data-stu-id="37e6c-125">Select **Polls Django Web Project** and click OK to create the project.</span></span>

     ![Dialogrutan Nytt projekt](./media/web-sites-python-ptvs-django-sql/PollsDjangoNewProject.png)
3. <span data-ttu-id="37e6c-127">Du uppmanas att installera externa paket.</span><span class="sxs-lookup"><span data-stu-id="37e6c-127">You will be prompted to install external packages.</span></span> <span data-ttu-id="37e6c-128">Välj **Install into a virtual environment** (Installera i en virtuell miljö).</span><span class="sxs-lookup"><span data-stu-id="37e6c-128">Select **Install into a virtual environment**.</span></span>

     ![Dialogrutan Externa paket](./media/web-sites-python-ptvs-django-sql/PollsDjangoExternalPackages.png)
4. <span data-ttu-id="37e6c-130">Välj **Python 2.7** som bastolk.</span><span class="sxs-lookup"><span data-stu-id="37e6c-130">Select **Python 2.7** as the base interpreter.</span></span>

     ![Dialogrutan Add Virtual Environment (Lägg till virtuell miljö)](./media/web-sites-python-ptvs-django-sql/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="37e6c-132">Högerklickar på projektnoden i **Solution Explorer** och välj först **Python** och sedan **Django Migrate**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-132">In **Solution Explorer**, right-click on the project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="37e6c-133">Välj därefter **Django – skapa superanvändare**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-133">Then select **Django Create Superuser**.</span></span>
6. <span data-ttu-id="37e6c-134">Öppnar Django Management Console och en sqlite-databas skapas i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="37e6c-134">This will open a Django Management Console and create a sqlite database in the project folder.</span></span> <span data-ttu-id="37e6c-135">Skapa en användare genom att följa anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="37e6c-135">Follow the prompts to create a user.</span></span>
7. <span data-ttu-id="37e6c-136">Bekräfta att programmet fungerar genom att trycka på <kbd>F5</kbd>.</span><span class="sxs-lookup"><span data-stu-id="37e6c-136">Confirm that the application works by pressing <kbd>F5</kbd>.</span></span>
8. <span data-ttu-id="37e6c-137">Klicka på **Log in** (Logga in) i navigeringsfältet längst upp.</span><span class="sxs-lookup"><span data-stu-id="37e6c-137">Click **Log in** from the navigation bar at the top.</span></span>

     ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalMenu.png)
9. <span data-ttu-id="37e6c-139">Ange autentiseringsuppgifterna för användaren du skapade när du synkroniserade databasen.</span><span class="sxs-lookup"><span data-stu-id="37e6c-139">Enter the credentials for the user you created when you synchronized the database.</span></span>

     ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalLogin.png)
10. <span data-ttu-id="37e6c-141">Klicka på **Create Sample Polls** (Skapa exempelomröstningar).</span><span class="sxs-lookup"><span data-stu-id="37e6c-141">Click **Create Sample Polls**.</span></span>

      ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserNoPolls.png)
11. <span data-ttu-id="37e6c-143">Klicka på en omröstning och rösta.</span><span class="sxs-lookup"><span data-stu-id="37e6c-143">Click on a poll and vote.</span></span>

      ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-sql-database"></a><span data-ttu-id="37e6c-145">Skapa en SQL Database</span><span class="sxs-lookup"><span data-stu-id="37e6c-145">Create a SQL Database</span></span>
<span data-ttu-id="37e6c-146">Vi ska skapa en Azure SQL database för databasen.</span><span class="sxs-lookup"><span data-stu-id="37e6c-146">For the database, we'll create an Azure SQL database.</span></span>

<span data-ttu-id="37e6c-147">Du kan skapa en databas genom att följa dessa steg.</span><span class="sxs-lookup"><span data-stu-id="37e6c-147">You can create a database by following these steps.</span></span>

1. <span data-ttu-id="37e6c-148">Logga in på den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="37e6c-148">Log into the [Azure Portal].</span></span>
2. <span data-ttu-id="37e6c-149">Längst ned i navigeringsfönstret klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-149">At the bottom of the navigation pane, click **NEW**.</span></span> <span data-ttu-id="37e6c-150">, klickar du på **Data + lagring** > **SQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-150">, click **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="37e6c-151">Konfigurera den nya SQL-databasen genom att skapa en ny resursgrupp och välja en lämplig plats för den.</span><span class="sxs-lookup"><span data-stu-id="37e6c-151">Configure the new SQL Database by creating a new resource group and select the appropriate location for it.</span></span>
4. <span data-ttu-id="37e6c-152">När SQL-databasen har skapats klickar du på **öppna i Visual Studio** i databasbladet.</span><span class="sxs-lookup"><span data-stu-id="37e6c-152">Once the SQL Database is created, click **Open in Visual Studio** in the database blade.</span></span>
5. <span data-ttu-id="37e6c-153">Klicka på **konfigurera brandväggen**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-153">Click **Configure your firewall**.</span></span>
6. <span data-ttu-id="37e6c-154">I den **brandväggsinställningar** bladet Lägg till en brandväggsregel med **första IP-** och **sista IP** inställd på utvecklingsdatorn offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="37e6c-154">In the **Firewall Settings** blade, add a firewall rule with **START IP** and **END IP** set to the public IP address of your development machine.</span></span> <span data-ttu-id="37e6c-155">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-155">Click **Save**.</span></span>

   <span data-ttu-id="37e6c-156">Detta tillåter anslutningar till databasservern från utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="37e6c-156">This will allow connections to the database server from your development machine.</span></span>
7. <span data-ttu-id="37e6c-157">Klicka på tillbaka i databasbladet **egenskaper**, klicka på **visa databasanslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-157">Back in the database blade, click **Properties**, then click **Show database connection strings**.</span></span>
8. <span data-ttu-id="37e6c-158">Använd kopieringsknappen för att ange värdet för **ADO.NET** i Urklipp.</span><span class="sxs-lookup"><span data-stu-id="37e6c-158">Use the copy button to put the value of **ADO.NET** on the clipboard.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="37e6c-159">Konfigurera projektet</span><span class="sxs-lookup"><span data-stu-id="37e6c-159">Configure the Project</span></span>
<span data-ttu-id="37e6c-160">I det här avsnittet konfigurerar vi webbappen att använda SQL-databas som vi just skapade.</span><span class="sxs-lookup"><span data-stu-id="37e6c-160">In this section, we'll configure our web app to use the SQL database we just created.</span></span> <span data-ttu-id="37e6c-161">Vi måste också installera ytterligare Python-paket som krävs för att använda SQL-databaser med Django.</span><span class="sxs-lookup"><span data-stu-id="37e6c-161">We'll also install additional Python packages required to use SQL databases with Django.</span></span> <span data-ttu-id="37e6c-162">Vi kommer kör webbappen lokalt.</span><span class="sxs-lookup"><span data-stu-id="37e6c-162">Then we'll run the web app locally.</span></span>

1. <span data-ttu-id="37e6c-163">I Visual Studio öppnar du **settings.py** i mappen *Projektnamn*.</span><span class="sxs-lookup"><span data-stu-id="37e6c-163">In Visual Studio, open **settings.py**, from the *ProjectName* folder.</span></span> <span data-ttu-id="37e6c-164">Klistra in anslutningssträngen i redigeringsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="37e6c-164">Temporarily paste the connection string in the editor.</span></span> <span data-ttu-id="37e6c-165">Anslutningssträngen har det här formatet:</span><span class="sxs-lookup"><span data-stu-id="37e6c-165">The connection string is in this format:</span></span>

       Server=<ServerName>,<ServerPort>;Database=<DatabaseName>;User ID=<UserName>;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

<span data-ttu-id="37e6c-166">Redigera definition av `DATABASES` värdena ovan ska användas.</span><span class="sxs-lookup"><span data-stu-id="37e6c-166">Edit the definition of `DATABASES` to use the values above.</span></span>

        DATABASES = {
            'default': {
                'ENGINE': 'sql_server.pyodbc',
                'NAME': '<DatabaseName>',
                'USER': '<UserName>',
                'PASSWORD': '{your_password_here}',
                'HOST': '<ServerName>',
                'PORT': '<ServerPort>',
                'OPTIONS': {
                    'driver': 'SQL Server Native Client 11.0',
                    'MARS_Connection': 'True',
                }
            }
        }

1. <span data-ttu-id="37e6c-167">I Solution Explorer under **Python Environments** (Python-miljöer), högerklickar du på den virtuella miljön och väljer **Install Python Package** (Installera Python-paket).</span><span class="sxs-lookup"><span data-stu-id="37e6c-167">In Solution Explorer, under **Python Environments**, right-click on the virtual environment and select **Install Python Package**.</span></span>
2. <span data-ttu-id="37e6c-168">Installera `pyodbc`-paketet med **pip**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-168">Install the package `pyodbc` using **pip**.</span></span>

     ![Python dialogrutan Installera paket](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackagePyodbc.png)
3. <span data-ttu-id="37e6c-170">Installera `django-pyodbc-azure`-paketet med **pip**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-170">Install the package `django-pyodbc-azure` using **pip**.</span></span>

     ![Python dialogrutan Installera paket](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackageDjangoPyodbcAzure.png)
4. <span data-ttu-id="37e6c-172">Högerklickar på projektnoden i **Solution Explorer** och välj först **Python** och sedan **Django Migrate**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-172">In **Solution Explorer**, right-click on the project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="37e6c-173">Välj därefter **Django – skapa superanvändare**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-173">Then select **Django Create Superuser**.</span></span>

   <span data-ttu-id="37e6c-174">Detta skapar tabeller för SQL-databas som vi skapade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="37e6c-174">This will create the tables for the SQL database we created in the previous section.</span></span> <span data-ttu-id="37e6c-175">Följ anvisningarna för att skapa en användare som inte behöver matcha användaren i sqlite-databas som skapats i det första avsnittet.</span><span class="sxs-lookup"><span data-stu-id="37e6c-175">Follow the prompts to create a user, which doesn't have to match the user in the sqlite database created in the first section.</span></span>
5. <span data-ttu-id="37e6c-176">Kör programmet med `F5`.</span><span class="sxs-lookup"><span data-stu-id="37e6c-176">Run the application with `F5`.</span></span> <span data-ttu-id="37e6c-177">Omröstningar som skapas med **skapa Exempelomröstningar** och de data som skickats vid röstning serialiseras i SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="37e6c-177">Polls that are created with **Create Sample Polls** and the data submitted by voting will be serialized in the SQL database.</span></span>

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="37e6c-178">Publicera webbappen i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="37e6c-178">Publish the web app to Azure App Service</span></span>
<span data-ttu-id="37e6c-179">Azure .NET SDK innehåller ett enkelt sätt att distribuera din web-webbapp till Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="37e6c-179">The Azure .NET SDK provides an easy way to deploy your web web app to Azure App Service Web Apps.</span></span>

1. <span data-ttu-id="37e6c-180">I **Solution Explorer** högerklickar du på projektnoden och väljer **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-180">In **Solution Explorer**, right-click on the project node and select **Publish**.</span></span>

     ![Dialogrutan Publicera webbplats](./media/web-sites-python-ptvs-django-sql/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="37e6c-182">Klicka på **Microsoft Azure Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-182">Click on **Microsoft Azure Web Apps**.</span></span>
3. <span data-ttu-id="37e6c-183">Skapa en ny webbapp genom att klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-183">Click on **New** to create a new web app.</span></span>
4. <span data-ttu-id="37e6c-184">Fyll i följande fält och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-184">Fill in the following fields and click **Create**.</span></span>

   * <span data-ttu-id="37e6c-185">**Webbappens namn**</span><span class="sxs-lookup"><span data-stu-id="37e6c-185">**Web App name**</span></span>
   * <span data-ttu-id="37e6c-186">**App Service-plan**</span><span class="sxs-lookup"><span data-stu-id="37e6c-186">**App Service plan**</span></span>
   * <span data-ttu-id="37e6c-187">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="37e6c-187">**Resource group**</span></span>
   * <span data-ttu-id="37e6c-188">**Region**</span><span class="sxs-lookup"><span data-stu-id="37e6c-188">**Region**</span></span>
   * <span data-ttu-id="37e6c-189">Lämna **Databasserver** inställd på **Ingen databas**</span><span class="sxs-lookup"><span data-stu-id="37e6c-189">Leave **Database server** set to **No database**</span></span>
5. <span data-ttu-id="37e6c-190">Godkänn alla standardvärden och klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="37e6c-190">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="37e6c-191">Den publicerade webbappen öppnas automatiskt i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="37e6c-191">Your web browser will open automatically to the published web app.</span></span> <span data-ttu-id="37e6c-192">Du bör se att webbappen fungerar som förväntat, med hjälp av den **SQL** databasen på Azure.</span><span class="sxs-lookup"><span data-stu-id="37e6c-192">You should see the web app working as expected, using the **SQL** database hosted on Azure.</span></span>

   <span data-ttu-id="37e6c-193">Grattis!</span><span class="sxs-lookup"><span data-stu-id="37e6c-193">Congratulations!</span></span>

     ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoAzureBrowser.png)

## <a name="next-steps"></a><span data-ttu-id="37e6c-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37e6c-195">Next steps</span></span>
<span data-ttu-id="37e6c-196">Följa dessa länkar om du vill veta mer om Python Tools för Visual Studio, Django och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="37e6c-196">Follow these links to learn more about Python Tools for Visual Studio, Django and SQL Database.</span></span>

* <span data-ttu-id="37e6c-197">[Dokumentation för Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="37e6c-197">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="37e6c-198">[Webbprojekt]</span><span class="sxs-lookup"><span data-stu-id="37e6c-198">[Web Projects]</span></span>
  * <span data-ttu-id="37e6c-199">[Molntjänstprojekt]</span><span class="sxs-lookup"><span data-stu-id="37e6c-199">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="37e6c-200">[Fjärrfelsökning i Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="37e6c-200">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="37e6c-201">[Django-dokumentation]</span><span class="sxs-lookup"><span data-stu-id="37e6c-201">[Django Documentation]</span></span>
* <span data-ttu-id="37e6c-202">[SQL Database]</span><span class="sxs-lookup"><span data-stu-id="37e6c-202">[SQL Database]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="37e6c-203">Nyheter</span><span class="sxs-lookup"><span data-stu-id="37e6c-203">What's changed</span></span>
* <span data-ttu-id="37e6c-204">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="37e6c-204">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
<span data-ttu-id="37e6c-205">[Python Developer Center]: /develop/python/</span><span class="sxs-lookup"><span data-stu-id="37e6c-205">[Python Developer Center]: /develop/python/</span></span>
<span data-ttu-id="37e6c-206">[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md</span><span class="sxs-lookup"><span data-stu-id="37e6c-206">[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md</span></span>

<!--External Link references-->
<span data-ttu-id="37e6c-207">[Azure-portalen]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="37e6c-207">[Azure Portal]: https://portal.azure.com</span></span>
<span data-ttu-id="37e6c-208">[Python Tools för Visual Studio]: http://aka.ms/ptvs</span><span class="sxs-lookup"><span data-stu-id="37e6c-208">[Python Tools for Visual Studio]: http://aka.ms/ptvs</span></span>
<span data-ttu-id="37e6c-209">[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span><span class="sxs-lookup"><span data-stu-id="37e6c-209">[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span></span>
<span data-ttu-id="37e6c-210">[Python Tools 2.2 för Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025</span><span class="sxs-lookup"><span data-stu-id="37e6c-210">[Python Tools 2.2 for Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025</span></span>
<span data-ttu-id="37e6c-211">[Azure SDK Tools för VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003</span><span class="sxs-lookup"><span data-stu-id="37e6c-211">[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003</span></span>
<span data-ttu-id="37e6c-212">[Python 2.7 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517190</span><span class="sxs-lookup"><span data-stu-id="37e6c-212">[Python 2.7 32-bit]: http://go.microsoft.com/fwlink/?LinkId=517190</span></span>
<span data-ttu-id="37e6c-213">[Dokumentation för Python Tools för Visual Studio]: http://aka.ms/ptvsdocs</span><span class="sxs-lookup"><span data-stu-id="37e6c-213">[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs</span></span>
<span data-ttu-id="37e6c-214">[Fjärrfelsökning i Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026</span><span class="sxs-lookup"><span data-stu-id="37e6c-214">[Remote Debugging on Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026</span></span>
<span data-ttu-id="37e6c-215">[Webbprojekt]: http://go.microsoft.com/fwlink/?LinkId=624027</span><span class="sxs-lookup"><span data-stu-id="37e6c-215">[Web Projects]: http://go.microsoft.com/fwlink/?LinkId=624027</span></span>
<span data-ttu-id="37e6c-216">[Molntjänstprojekt]: http://go.microsoft.com/fwlink/?LinkId=624028</span><span class="sxs-lookup"><span data-stu-id="37e6c-216">[Cloud Service Projects]: http://go.microsoft.com/fwlink/?LinkId=624028</span></span>
<span data-ttu-id="37e6c-217">[Django-dokumentation]: https://www.djangoproject.com/</span><span class="sxs-lookup"><span data-stu-id="37e6c-217">[Django Documentation]: https://www.djangoproject.com/</span></span>
<span data-ttu-id="37e6c-218">[SQL Database]: /documentation/services/sql-database/</span><span class="sxs-lookup"><span data-stu-id="37e6c-218">[SQL Database]: /documentation/services/sql-database/</span></span>
