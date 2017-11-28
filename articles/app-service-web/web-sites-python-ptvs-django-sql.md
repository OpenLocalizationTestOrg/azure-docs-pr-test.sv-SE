---
title: "aaaDjango och SQL-databas på Azure med Python Tools 2.2 för Visual Studio"
description: "Lär dig hur toouse hello Python Tools för Visual Studio toocreate en Django-webbapp som lagrar data i en SQL-databasinstans och distribuera den tooAzure App Service Web Apps."
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
ms.openlocfilehash: b5b2ef4f3292e7df85007465c5394c8660a7d231
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="django-and-sql-database-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="4c0f5-103">Django och SQL Database på Azure med Python Tools 2.2 för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4c0f5-103">Django and SQL Database on Azure with Python Tools 2.2 for Visual Studio</span></span>
<span data-ttu-id="4c0f5-104">I den här kursen använder vi [Python Tools för Visual Studio] toocreate ett enkelt avfrågar webbapp med någon av exempelmallarna i hello PTVS.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-104">In this tutorial, we'll use [Python Tools for Visual Studio] toocreate a simple polls web app using one of hello PTVS sample templates.</span></span> <span data-ttu-id="4c0f5-105">Den här kursen finns också tillgänglig som en [video](https://www.youtube.com/watch?v=ZwcoGcIeHF4).</span><span class="sxs-lookup"><span data-stu-id="4c0f5-105">This tutorial is also available as a [video](https://www.youtube.com/watch?v=ZwcoGcIeHF4).</span></span>

<span data-ttu-id="4c0f5-106">Vi får lära dig hur toouse en SQL-databas finns i Azure, hur hello tooconfigure web app toouse en SQL-databas och hur toopublish hello webbprogrammet för[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="4c0f5-106">We'll learn how toouse a SQL database hosted on Azure, how tooconfigure hello web app toouse a SQL database, and how toopublish hello web app too[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="4c0f5-107">Se hello [Python Developer Center] fler artiklar om utvecklingen av Azure App Service Web Apps med PTVS med hjälp av Bottle, Flask och Django webbramverken med Azure Table Storage, MySQL och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-107">See hello [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with Azure Table Storage, MySQL and SQL Database services.</span></span> <span data-ttu-id="4c0f5-108">Den här artikeln fokuserar på App Service, hello stegen är liknande när du utvecklar [Azure Cloud Services].</span><span class="sxs-lookup"><span data-stu-id="4c0f5-108">While this article focuses on App Service, hello steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4c0f5-109">Krav</span><span class="sxs-lookup"><span data-stu-id="4c0f5-109">Prerequisites</span></span>
* <span data-ttu-id="4c0f5-110">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="4c0f5-110">Visual Studio 2015</span></span>
* <span data-ttu-id="4c0f5-111">[Python 2.7 32-bitars]</span><span class="sxs-lookup"><span data-stu-id="4c0f5-111">[Python 2.7 32-bit]</span></span>
* <span data-ttu-id="4c0f5-112">[Python Tools 2.2 för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="4c0f5-112">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="4c0f5-113">[Python Tools 2.2 för Visual Studio Samples VSI]</span><span class="sxs-lookup"><span data-stu-id="4c0f5-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="4c0f5-114">[Azure SDK Tools för VS 2015]</span><span class="sxs-lookup"><span data-stu-id="4c0f5-114">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="4c0f5-115">Django 1.9 eller senare</span><span class="sxs-lookup"><span data-stu-id="4c0f5-115">Django 1.9 or later</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="4c0f5-116">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-116">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="4c0f5-117">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-117">No credit cards required; no commitments.</span></span>
>
>

## <a name="create-hello-project"></a><span data-ttu-id="4c0f5-118">Skapa hello projekt</span><span class="sxs-lookup"><span data-stu-id="4c0f5-118">Create hello Project</span></span>
<span data-ttu-id="4c0f5-119">I det här avsnittet skapar vi ett Visual Studio-projekt utifrån en exempelmall.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-119">In this section, we'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="4c0f5-120">Vi skapa en virtuell miljö och installera nödvändiga paket.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-120">We'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="4c0f5-121">Vi ska skapa en lokal databas med hjälp av sqlite.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-121">We'll create a local database using sqlite.</span></span> <span data-ttu-id="4c0f5-122">Vi kommer kör hello webbappen lokalt.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-122">Then we'll run hello web app locally.</span></span>

1. <span data-ttu-id="4c0f5-123">I Visual Studio väljer du **Arkiv**, **Nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-123">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="4c0f5-124">Hej projektmallar från hello [Python Tools 2.2 för Visual Studio Samples VSI] är tillgängliga under **Python**, **exempel**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-124">hello project templates from hello [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="4c0f5-125">Välj **Polls Django Web Project** och klicka på OK toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-125">Select **Polls Django Web Project** and click OK toocreate hello project.</span></span>

     ![Dialogrutan Nytt projekt](./media/web-sites-python-ptvs-django-sql/PollsDjangoNewProject.png)
3. <span data-ttu-id="4c0f5-127">Du kommer att tillfrågas tooinstall externa paket.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-127">You will be prompted tooinstall external packages.</span></span> <span data-ttu-id="4c0f5-128">Välj **Install into a virtual environment** (Installera i en virtuell miljö).</span><span class="sxs-lookup"><span data-stu-id="4c0f5-128">Select **Install into a virtual environment**.</span></span>

     ![Dialogrutan Externa paket](./media/web-sites-python-ptvs-django-sql/PollsDjangoExternalPackages.png)
4. <span data-ttu-id="4c0f5-130">Välj **Python 2.7** som hello bastolk.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-130">Select **Python 2.7** as hello base interpreter.</span></span>

     ![Dialogrutan Add Virtual Environment (Lägg till virtuell miljö)](./media/web-sites-python-ptvs-django-sql/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="4c0f5-132">I **Solution Explorer**, högerklicka på hello projektnoden och väljer **Python**, och välj sedan **Django migrera**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-132">In **Solution Explorer**, right-click on hello project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="4c0f5-133">Välj därefter **Django – skapa superanvändare**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-133">Then select **Django Create Superuser**.</span></span>
6. <span data-ttu-id="4c0f5-134">Detta öppnar Django Management Console och skapa en sqlite-databas i hello projektmappen.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-134">This will open a Django Management Console and create a sqlite database in hello project folder.</span></span> <span data-ttu-id="4c0f5-135">Följ hello prompter toocreate en användare.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-135">Follow hello prompts toocreate a user.</span></span>
7. <span data-ttu-id="4c0f5-136">Bekräfta att hello programmet fungerar genom att trycka på <kbd>F5</kbd>.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-136">Confirm that hello application works by pressing <kbd>F5</kbd>.</span></span>
8. <span data-ttu-id="4c0f5-137">Klicka på **logga in** från hello navigeringsfältet hello överst.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-137">Click **Log in** from hello navigation bar at hello top.</span></span>

     ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalMenu.png)
9. <span data-ttu-id="4c0f5-139">Ange hello autentiseringsuppgifter för hello användaren du skapade när du synkroniserade hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-139">Enter hello credentials for hello user you created when you synchronized hello database.</span></span>

     ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalLogin.png)
10. <span data-ttu-id="4c0f5-141">Klicka på **Create Sample Polls** (Skapa exempelomröstningar).</span><span class="sxs-lookup"><span data-stu-id="4c0f5-141">Click **Create Sample Polls**.</span></span>

      ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserNoPolls.png)
11. <span data-ttu-id="4c0f5-143">Klicka på en omröstning och rösta.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-143">Click on a poll and vote.</span></span>

      ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-sql-database"></a><span data-ttu-id="4c0f5-145">Skapa en SQL Database</span><span class="sxs-lookup"><span data-stu-id="4c0f5-145">Create a SQL Database</span></span>
<span data-ttu-id="4c0f5-146">Hello-databas ska du skapa en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-146">For hello database, we'll create an Azure SQL database.</span></span>

<span data-ttu-id="4c0f5-147">Du kan skapa en databas genom att följa dessa steg.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-147">You can create a database by following these steps.</span></span>

1. <span data-ttu-id="4c0f5-148">Logga in på hello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="4c0f5-148">Log into hello [Azure Portal].</span></span>
2. <span data-ttu-id="4c0f5-149">Hello längst ned i navigeringsfönstret hello klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-149">At hello bottom of hello navigation pane, click **NEW**.</span></span> <span data-ttu-id="4c0f5-150">, klickar du på **Data + lagring** > **SQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-150">, click **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="4c0f5-151">Konfigurera hello ny SQL-databas genom att skapa en ny resursgrupp och välj hello lämplig plats för den.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-151">Configure hello new SQL Database by creating a new resource group and select hello appropriate location for it.</span></span>
4. <span data-ttu-id="4c0f5-152">När hello SQL-databas har skapats klickar du på **öppna i Visual Studio** i hello databasbladet.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-152">Once hello SQL Database is created, click **Open in Visual Studio** in hello database blade.</span></span>
5. <span data-ttu-id="4c0f5-153">Klicka på **konfigurera brandväggen**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-153">Click **Configure your firewall**.</span></span>
6. <span data-ttu-id="4c0f5-154">I hello **brandväggsinställningar** bladet Lägg till en brandväggsregel med **första IP-** och **sista IP** toohello offentliga IP-adressen på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-154">In hello **Firewall Settings** blade, add a firewall rule with **START IP** and **END IP** set toohello public IP address of your development machine.</span></span> <span data-ttu-id="4c0f5-155">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-155">Click **Save**.</span></span>

   <span data-ttu-id="4c0f5-156">Detta gör att anslutningar toohello databasservern från utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-156">This will allow connections toohello database server from your development machine.</span></span>
7. <span data-ttu-id="4c0f5-157">Klicka på tillbaka i hello databasbladet **egenskaper**, klicka på **visa databasanslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-157">Back in hello database blade, click **Properties**, then click **Show database connection strings**.</span></span>
8. <span data-ttu-id="4c0f5-158">Använda hello Kopiera knappen tooput hello värdet **ADO.NET** hello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-158">Use hello copy button tooput hello value of **ADO.NET** on hello clipboard.</span></span>

## <a name="configure-hello-project"></a><span data-ttu-id="4c0f5-159">Konfigurera hello projekt</span><span class="sxs-lookup"><span data-stu-id="4c0f5-159">Configure hello Project</span></span>
<span data-ttu-id="4c0f5-160">I det här avsnittet konfigurerar vi våra web app toouse hello SQL-databas vi just skapade.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-160">In this section, we'll configure our web app toouse hello SQL database we just created.</span></span> <span data-ttu-id="4c0f5-161">Vi måste också installera ytterligare Python-paket krävs toouse SQL-databaser med Django.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-161">We'll also install additional Python packages required toouse SQL databases with Django.</span></span> <span data-ttu-id="4c0f5-162">Vi kommer kör hello webbappen lokalt.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-162">Then we'll run hello web app locally.</span></span>

1. <span data-ttu-id="4c0f5-163">Öppna i Visual Studio **settings.py**, från hello *projektnamn* mapp.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-163">In Visual Studio, open **settings.py**, from hello *ProjectName* folder.</span></span> <span data-ttu-id="4c0f5-164">Klistra in hello anslutningssträngen i hello-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-164">Temporarily paste hello connection string in hello editor.</span></span> <span data-ttu-id="4c0f5-165">hello anslutningssträngen är i formatet:</span><span class="sxs-lookup"><span data-stu-id="4c0f5-165">hello connection string is in this format:</span></span>

       Server=<ServerName>,<ServerPort>;Database=<DatabaseName>;User ID=<UserName>;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

<span data-ttu-id="4c0f5-166">Redigera hello definitionen av `DATABASES` toouse hello värdena ovan.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-166">Edit hello definition of `DATABASES` toouse hello values above.</span></span>

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

1. <span data-ttu-id="4c0f5-167">I Solution Explorer under **Python-miljöer**, högerklicka på hello virtuella miljön och välj **Install Python Package**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-167">In Solution Explorer, under **Python Environments**, right-click on hello virtual environment and select **Install Python Package**.</span></span>
2. <span data-ttu-id="4c0f5-168">Installera hello paketet `pyodbc` med **pip**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-168">Install hello package `pyodbc` using **pip**.</span></span>

     ![Python dialogrutan Installera paket](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackagePyodbc.png)
3. <span data-ttu-id="4c0f5-170">Installera hello paketet `django-pyodbc-azure` med **pip**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-170">Install hello package `django-pyodbc-azure` using **pip**.</span></span>

     ![Python dialogrutan Installera paket](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackageDjangoPyodbcAzure.png)
4. <span data-ttu-id="4c0f5-172">I **Solution Explorer**, högerklicka på hello projektnoden och väljer **Python**, och välj sedan **Django migrera**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-172">In **Solution Explorer**, right-click on hello project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="4c0f5-173">Välj därefter **Django – skapa superanvändare**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-173">Then select **Django Create Superuser**.</span></span>

   <span data-ttu-id="4c0f5-174">Detta skapar hello tabeller för hello SQL-databas som vi skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-174">This will create hello tables for hello SQL database we created in hello previous section.</span></span> <span data-ttu-id="4c0f5-175">Följ hello prompter toocreate en användare som inte har toomatch hello användaren hello sqlite-databas som skapats i hello första avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-175">Follow hello prompts toocreate a user, which doesn't have toomatch hello user in hello sqlite database created in hello first section.</span></span>
5. <span data-ttu-id="4c0f5-176">Kör programmet hello med `F5`.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-176">Run hello application with `F5`.</span></span> <span data-ttu-id="4c0f5-177">Omröstningar som skapas med **skapa Exempelomröstningar** och hello-data som skickats vid röstning serialiseras i hello SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-177">Polls that are created with **Create Sample Polls** and hello data submitted by voting will be serialized in hello SQL database.</span></span>

## <a name="publish-hello-web-app-tooazure-app-service"></a><span data-ttu-id="4c0f5-178">Publicera hello web app tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="4c0f5-178">Publish hello web app tooAzure App Service</span></span>
<span data-ttu-id="4c0f5-179">hello Azure .NET SDK innehåller ett enkelt sätt toodeploy din web web app tooAzure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-179">hello Azure .NET SDK provides an easy way toodeploy your web web app tooAzure App Service Web Apps.</span></span>

1. <span data-ttu-id="4c0f5-180">I **Solution Explorer**, högerklicka på hello projektnoden och väljer **publicera**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-180">In **Solution Explorer**, right-click on hello project node and select **Publish**.</span></span>

     ![Dialogrutan Publicera webbplats](./media/web-sites-python-ptvs-django-sql/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="4c0f5-182">Klicka på **Microsoft Azure Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-182">Click on **Microsoft Azure Web Apps**.</span></span>
3. <span data-ttu-id="4c0f5-183">Klicka på **ny** toocreate ett nytt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-183">Click on **New** toocreate a new web app.</span></span>
4. <span data-ttu-id="4c0f5-184">Fyll i följande fält hello och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-184">Fill in hello following fields and click **Create**.</span></span>

   * <span data-ttu-id="4c0f5-185">**Webbappens namn**</span><span class="sxs-lookup"><span data-stu-id="4c0f5-185">**Web App name**</span></span>
   * <span data-ttu-id="4c0f5-186">**App Service-plan**</span><span class="sxs-lookup"><span data-stu-id="4c0f5-186">**App Service plan**</span></span>
   * <span data-ttu-id="4c0f5-187">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="4c0f5-187">**Resource group**</span></span>
   * <span data-ttu-id="4c0f5-188">**Region**</span><span class="sxs-lookup"><span data-stu-id="4c0f5-188">**Region**</span></span>
   * <span data-ttu-id="4c0f5-189">Lämna **databasserver** ställa in också**ingen databas**</span><span class="sxs-lookup"><span data-stu-id="4c0f5-189">Leave **Database server** set too**No database**</span></span>
5. <span data-ttu-id="4c0f5-190">Godkänn alla standardvärden och klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-190">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="4c0f5-191">Webbläsaren öppnas automatiskt toohello publicerade webbprogram.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-191">Your web browser will open automatically toohello published web app.</span></span> <span data-ttu-id="4c0f5-192">Du bör se hello web app fungerar som förväntat, med hjälp av hello **SQL** databasen på Azure.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-192">You should see hello web app working as expected, using hello **SQL** database hosted on Azure.</span></span>

   <span data-ttu-id="4c0f5-193">Grattis!</span><span class="sxs-lookup"><span data-stu-id="4c0f5-193">Congratulations!</span></span>

     ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoAzureBrowser.png)

## <a name="next-steps"></a><span data-ttu-id="4c0f5-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4c0f5-195">Next steps</span></span>
<span data-ttu-id="4c0f5-196">Följ dessa länkar toolearn mer om Python Tools för Visual Studio, Django och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-196">Follow these links toolearn more about Python Tools for Visual Studio, Django and SQL Database.</span></span>

* <span data-ttu-id="4c0f5-197">[Dokumentation för Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="4c0f5-197">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="4c0f5-198">[Webbprojekt]</span><span class="sxs-lookup"><span data-stu-id="4c0f5-198">[Web Projects]</span></span>
  * <span data-ttu-id="4c0f5-199">[Molntjänstprojekt]</span><span class="sxs-lookup"><span data-stu-id="4c0f5-199">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="4c0f5-200">[Fjärrfelsökning i Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="4c0f5-200">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="4c0f5-201">[Django-dokumentation]</span><span class="sxs-lookup"><span data-stu-id="4c0f5-201">[Django Documentation]</span></span>
* <span data-ttu-id="4c0f5-202">[SQL Database]</span><span class="sxs-lookup"><span data-stu-id="4c0f5-202">[SQL Database]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="4c0f5-203">Nyheter</span><span class="sxs-lookup"><span data-stu-id="4c0f5-203">What's changed</span></span>
* <span data-ttu-id="4c0f5-204">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="4c0f5-204">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[Python Developer Center]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->
[Azure Portal]: https://portal.azure.com
[Python Tools för Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 för Visual Studio Samples VSI]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools för VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517190
[Dokumentation för Python Tools för Visual Studio]: http://aka.ms/ptvsdocs
[Fjärrfelsökning i Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webbprojekt]: http://go.microsoft.com/fwlink/?LinkId=624027
[Molntjänstprojekt]: http://go.microsoft.com/fwlink/?LinkId=624028
[Django-dokumentation]: https://www.djangoproject.com/
[SQL Database]: /documentation/services/sql-database/
