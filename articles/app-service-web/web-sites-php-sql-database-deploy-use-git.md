---
title: "aaaCreate PHP-SQL webbapp och distribuera tooAzure Apptjänst med Git"
description: "En genomgång som visar hur toocreate en PHP webbapp som lagrar data i Azure SQL Database och använda Git-distribution tooAzure Apptjänst."
services: app-service\web, sql-database
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 6b090bf6-31d8-4b74-81eb-050ef95929ca
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: aaacb2fe0787bbcdafa72871912e8d08792be29d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-php-sql-web-app-and-deploy-tooazure-app-service-using-git"></a><span data-ttu-id="22a72-103">Skapa en PHP-SQL-webbapp och distribuera tooAzure Apptjänst med Git</span><span class="sxs-lookup"><span data-stu-id="22a72-103">Create a PHP-SQL web app and deploy tooAzure App Service using Git</span></span>
<span data-ttu-id="22a72-104">Den här kursen visar hur toocreate en PHP web app i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) som ansluter tooAzure SQL-databas och hur toodeploy den med hjälp av Git.</span><span class="sxs-lookup"><span data-stu-id="22a72-104">This tutorial shows you how toocreate a PHP web app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) that connects tooAzure SQL Database and how toodeploy it using Git.</span></span> <span data-ttu-id="22a72-105">Den här kursen förutsätter att du har [PHP][install-php], [SQL Server Express][install-SQLExpress], hello [Drivers Microsoft för SQL Server för PHP ](http://www.microsoft.com/download/en/details.aspx?id=20098), och [Git] [ install-git] installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="22a72-105">This tutorial assumes you have [PHP][install-php], [SQL Server Express][install-SQLExpress], hello [Microsoft Drivers for SQL Server for PHP](http://www.microsoft.com/download/en/details.aspx?id=20098), and [Git][install-git] installed on your computer.</span></span> <span data-ttu-id="22a72-106">När du slutför den här guiden kommer du har ett PHP-SQL-webbprogram som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="22a72-106">Upon completing this guide, you will have a PHP-SQL web app running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="22a72-107">Du kan installera och konfigurera PHP, SQL Server Express och hello Microsoft Drivers för SQL Server för PHP med hello [Microsoft Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="22a72-107">You can install and configure PHP, SQL Server Express, and hello Microsoft Drivers for SQL Server for PHP using hello [Microsoft Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> 
> 

<span data-ttu-id="22a72-108">Du kommer att lära dig:</span><span class="sxs-lookup"><span data-stu-id="22a72-108">You will learn:</span></span>

* <span data-ttu-id="22a72-109">Hur toocreate en Azure web app och en SQL-databas med hjälp av hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span><span class="sxs-lookup"><span data-stu-id="22a72-109">How toocreate an Azure web app and a SQL Database using hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span></span> <span data-ttu-id="22a72-110">Eftersom PHP är aktiverad i App Service Web Apps som standard, inget särskilt är obligatoriska toorun PHP-kod.</span><span class="sxs-lookup"><span data-stu-id="22a72-110">Because PHP is enabled in App Service Web Apps by default, nothing special is required toorun your PHP code.</span></span>
* <span data-ttu-id="22a72-111">Hur toopublish och publicera dina program tooAzure med Git.</span><span class="sxs-lookup"><span data-stu-id="22a72-111">How toopublish and re-publish your application tooAzure using Git.</span></span>

<span data-ttu-id="22a72-112">Genom att följa den här självstudiekursen skapar du en enkel registrering webbprogram i PHP.</span><span class="sxs-lookup"><span data-stu-id="22a72-112">By following this tutorial, you will build a simple registration web application in PHP.</span></span> <span data-ttu-id="22a72-113">hello program ska finnas i en Azure-webbplats.</span><span class="sxs-lookup"><span data-stu-id="22a72-113">hello application will be hosted in an Azure Website.</span></span> <span data-ttu-id="22a72-114">En skärmbild av programmet hello slutförts understiger:</span><span class="sxs-lookup"><span data-stu-id="22a72-114">A screenshot of hello completed application is below:</span></span>

![Azure PHP-webbplats](./media/web-sites-php-sql-database-deploy-use-git/running_app_3.png)

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="22a72-116">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="22a72-116">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="22a72-117">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="22a72-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-an-azure-web-app-and-set-up-git-publishing"></a><span data-ttu-id="22a72-118">Skapa en Azure webbapp och konfigurera Git-publicering</span><span class="sxs-lookup"><span data-stu-id="22a72-118">Create an Azure web app and set up Git publishing</span></span>
<span data-ttu-id="22a72-119">Följ dessa steg toocreate en Azure webbapp och en SQL-databas:</span><span class="sxs-lookup"><span data-stu-id="22a72-119">Follow these steps toocreate an Azure web app and a SQL Database:</span></span>

1. <span data-ttu-id="22a72-120">Logga in toohello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="22a72-120">Log in toohello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="22a72-121">Öppna hello Azure Marketplace genom att klicka på hello **ny** ikonen hello längst upp till vänster i hello instrumentpanelen, klicka på **Markera alla** nästa tooMarketplace och välja **webb + mobilt**.</span><span class="sxs-lookup"><span data-stu-id="22a72-121">Open hello Azure Marketplace by clicking hello **New** icon on hello top left of hello dashboard, click on **Select All** next tooMarketplace and selecting **Web + Mobile**.</span></span>
3. <span data-ttu-id="22a72-122">Välj i hello Marketplace, **webb + mobilt**.</span><span class="sxs-lookup"><span data-stu-id="22a72-122">In hello Marketplace, select **Web + Mobile**.</span></span>
4. <span data-ttu-id="22a72-123">Klicka på hello **Webbapp + SQL** ikon.</span><span class="sxs-lookup"><span data-stu-id="22a72-123">Click hello **Web app + SQL** icon.</span></span>
5. <span data-ttu-id="22a72-124">När du har läst hello beskrivning av hello webbapp + SQL appen, Välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="22a72-124">After reading hello description of hello Web app + SQL app, select **Create**.</span></span>
6. <span data-ttu-id="22a72-125">Klicka på varje del (**resursgruppen**, **Web App**, **databasen**, och **prenumeration**) och ange eller Välj värden för hello krävs fält:</span><span class="sxs-lookup"><span data-stu-id="22a72-125">Click on each part (**Resource Group**, **Web App**, **Database**, and **Subscription**) and enter or select values for hello required fields:</span></span>
   
   * <span data-ttu-id="22a72-126">Ange ett URL-namn du väljer</span><span class="sxs-lookup"><span data-stu-id="22a72-126">Enter a URL name of your choice</span></span>    
   * <span data-ttu-id="22a72-127">Konfigurera servern Databasautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="22a72-127">Configure database server credentials</span></span>
   * <span data-ttu-id="22a72-128">Välj hello region närmaste tooyou</span><span class="sxs-lookup"><span data-stu-id="22a72-128">Select hello region closest tooyou</span></span>
     
     ![konfigurera appen](./media/web-sites-php-sql-database-deploy-use-git/configure-db-settings.png)
7. <span data-ttu-id="22a72-130">När du definierat hello webbprogram, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="22a72-130">When finished defining hello web app, click **Create**.</span></span>
   
    <span data-ttu-id="22a72-131">När hello webbprogrammet har skapats, hello **meddelanden** knappen blinkar en grön **lyckade** och hello resurs grupp blad öppna tooshow båda hello web app och hello SQL-databas i hello grupp.</span><span class="sxs-lookup"><span data-stu-id="22a72-131">When hello web app has been created, hello **Notifications** button will flash a green **SUCCESS** and hello resource group blade open tooshow both hello web app and hello SQL database in hello group.</span></span>
8. <span data-ttu-id="22a72-132">Klicka på hello webbapp ikon i hello resurs grupp bladet tooopen hello webbapps blad.</span><span class="sxs-lookup"><span data-stu-id="22a72-132">Click hello web app's icon in hello resource group blade tooopen hello web app's blade.</span></span>
   
    ![Web Apps resursgrupp](./media/web-sites-php-sql-database-deploy-use-git/resource-group-blade.png)
9. <span data-ttu-id="22a72-134">I **inställningar** klickar du på **kontinuerlig distribution** > **konfigurera nödvändiga inställningar**.</span><span class="sxs-lookup"><span data-stu-id="22a72-134">In **Settings** click **Continuous deployment** > **Configure required settings**.</span></span> <span data-ttu-id="22a72-135">Välj **lokal Git-lagringsplats** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="22a72-135">Select **Local Git Repository** and click **OK**.</span></span>
   
    ![var finns i källkoden](./media/web-sites-php-sql-database-deploy-use-git/setup-local-git.png)
   
    <span data-ttu-id="22a72-137">Om du inte har registrerat en Git-lagringsplats innan måste du ange ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="22a72-137">If you have not set up a Git repository before, you must provide a user name and password.</span></span> <span data-ttu-id="22a72-138">toodo, genom att välja **inställningar** > **distributionsbehörigheterna** i hello webbapps blad.</span><span class="sxs-lookup"><span data-stu-id="22a72-138">toodo this, click **Settings** > **Deployment credentials** in hello web app's blade.</span></span>
   
    ![](./media/web-sites-php-sql-database-deploy-use-git/deployment-credentials.png)
10. <span data-ttu-id="22a72-139">I **inställningar** klickar du på **egenskaper** toosee hello Git fjärr-URL måste toouse toodeploy appen PHP senare.</span><span class="sxs-lookup"><span data-stu-id="22a72-139">In **Settings** click on **Properties** toosee hello Git remote URL you need toouse toodeploy your PHP app later.</span></span>

## <a name="get-sql-database-connection-information"></a><span data-ttu-id="22a72-140">Hämta anslutningsinformationen för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="22a72-140">Get SQL Database connection information</span></span>
<span data-ttu-id="22a72-141">tooconnect toohello SQL Database-instans som är länkade tooyour webbprogram, måste din kommer hello anslutningsinformation som du angav när du skapade hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="22a72-141">tooconnect toohello SQL Database instance that is linked tooyour web app, your will need hello connection information, which you specified when you created hello database.</span></span> <span data-ttu-id="22a72-142">tooget hello anslutningsinformationen för SQL-databas, så här:</span><span class="sxs-lookup"><span data-stu-id="22a72-142">tooget hello SQL Database connection information, follow these steps:</span></span>

1. <span data-ttu-id="22a72-143">Tillbaka i bladet hello resursgruppens namn, klickar du på ikonen hello SQL database.</span><span class="sxs-lookup"><span data-stu-id="22a72-143">Back in hello resource group's blade, click hello SQL database's icon.</span></span>
2. <span data-ttu-id="22a72-144">Hello SQL database-bladet klickar du på **inställningar** > **egenskaper**, klicka på **visa databasanslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="22a72-144">In hello SQL database's blade, click **Settings** > **Properties**, then click **Show database connection strings**.</span></span> 
   
    ![Visa egenskaper för databas](./media/web-sites-php-sql-database-deploy-use-git/view-database-properties.png)
3. <span data-ttu-id="22a72-146">Från hello **PHP** avsnittet för hello resulterande dialog, anteckna hello värden för `Server`, `SQL Database`, och `User Name`.</span><span class="sxs-lookup"><span data-stu-id="22a72-146">From hello **PHP** section of hello resulting dialog, make note of hello values for `Server`, `SQL Database`, and `User Name`.</span></span> <span data-ttu-id="22a72-147">Du använder dessa värden senare när publicera din PHP web app tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="22a72-147">You will use these values later when publishing your PHP web app tooAzure App Service.</span></span>

## <a name="build-and-test-your-application-locally"></a><span data-ttu-id="22a72-148">Skapa och testa programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="22a72-148">Build and test your application locally</span></span>
<span data-ttu-id="22a72-149">hello registreringen programmet är ett enkelt PHP-program som du kan använda tooregister för en händelse genom att tillhandahålla ditt namn och e-postadress.</span><span class="sxs-lookup"><span data-stu-id="22a72-149">hello Registration application is a simple PHP application that allows you tooregister for an event by providing your name and email address.</span></span> <span data-ttu-id="22a72-150">Information om föregående månaderna visas i en tabell.</span><span class="sxs-lookup"><span data-stu-id="22a72-150">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="22a72-151">Registreringsinformation lagras i en instans av SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="22a72-151">Registration information is stored in a SQL Database instance.</span></span> <span data-ttu-id="22a72-152">hello program består av två filer (kopiera och klistra in koden finns nedan):</span><span class="sxs-lookup"><span data-stu-id="22a72-152">hello application consists of two files (copy/paste code available below):</span></span>

* <span data-ttu-id="22a72-153">**index.php**: Visar ett formulär för registrering och en tabell som innehåller registrant information.</span><span class="sxs-lookup"><span data-stu-id="22a72-153">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>
* <span data-ttu-id="22a72-154">**createtable.php**: skapar hello SQL-databastabell för hello program.</span><span class="sxs-lookup"><span data-stu-id="22a72-154">**createtable.php**: Creates hello SQL Database table for hello application.</span></span> <span data-ttu-id="22a72-155">Den här filen ska bara användas en gång.</span><span class="sxs-lookup"><span data-stu-id="22a72-155">This file will only be used once.</span></span>

<span data-ttu-id="22a72-156">toorun hello programmet lokalt, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="22a72-156">toorun hello application locally, follow hello steps below.</span></span> <span data-ttu-id="22a72-157">Observera att de här stegen förutsätter att du har PHP och SQL Server Express ställa in på den lokala datorn och att du har aktiverat hello [PDO-tillägg för SQL Server][pdo-sqlsrv].</span><span class="sxs-lookup"><span data-stu-id="22a72-157">Note that these steps assume you have PHP and SQL Server Express set up on your local machine, and that you have enabled hello [PDO extension for SQL Server][pdo-sqlsrv].</span></span>

1. <span data-ttu-id="22a72-158">Skapa en SQL Server-databas som heter `registration`.</span><span class="sxs-lookup"><span data-stu-id="22a72-158">Create a SQL Server database called `registration`.</span></span> <span data-ttu-id="22a72-159">Du kan göra detta från hello `sqlcmd` Kommandotolken med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="22a72-159">You can do this from hello `sqlcmd` command prompt with these commands:</span></span>
   
        >sqlcmd -S localhost\sqlexpress -U <local user name> -P <local password>
        1> create database registration
        2> GO    
2. <span data-ttu-id="22a72-160">Skapa två filer i den - en kallas i din tillämpningsprogrammets rotkatalog `createtable.php` och en kallas `index.php`.</span><span class="sxs-lookup"><span data-stu-id="22a72-160">In your application root directory, create two files in it - one called `createtable.php` and one called `index.php`.</span></span>
3. <span data-ttu-id="22a72-161">Öppna hello `createtable.php` filen i en textredigerare eller IDE och Lägg till hello koden nedan.</span><span class="sxs-lookup"><span data-stu-id="22a72-161">Open hello `createtable.php` file in a text editor or IDE and add hello code below.</span></span> <span data-ttu-id="22a72-162">Den här koden kommer att använda toocreate hello `registration_tbl` tabellen i hello `registration` databas.</span><span class="sxs-lookup"><span data-stu-id="22a72-162">This code will be used toocreate hello `registration_tbl` table in hello `registration` database.</span></span>
   
        <?php
        // DB connection info
        $host = "localhost\sqlexpress";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "sqlsrv:Server= $host ; Database = $db ", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
            id INT NOT NULL IDENTITY(1,1) 
            PRIMARY KEY(id),
            name VARCHAR(30),
            email VARCHAR(30),
            date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>
   
    <span data-ttu-id="22a72-163">Observera att du måste tooupdate hello värden för <code>$user</code> och <code>$pwd</code> med dina lokala SQL Server-användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="22a72-163">Note that you will need tooupdate hello values for <code>$user</code> and <code>$pwd</code> with your local SQL Server user name and password.</span></span>
4. <span data-ttu-id="22a72-164">I en terminal på hello rotkatalog hello programmet typen hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="22a72-164">In a terminal at hello root directory of hello application type hello following command:</span></span>
   
        php -S localhost:8000
5. <span data-ttu-id="22a72-165">Öppna en webbläsare och gå för**http://localhost:8000/createtable.php**.</span><span class="sxs-lookup"><span data-stu-id="22a72-165">Open a web browser and browse too**http://localhost:8000/createtable.php**.</span></span> <span data-ttu-id="22a72-166">Detta skapar hello `registration_tbl` tabellen i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="22a72-166">This will create hello `registration_tbl` table in hello database.</span></span>
6. <span data-ttu-id="22a72-167">Öppna hello **index.php** filen i en textredigerare eller IDE och Lägg till hello grundläggande HTML- eller CSS-kod för hello sida (hello PHP kod läggs i senare steg).</span><span class="sxs-lookup"><span data-stu-id="22a72-167">Open hello **index.php** file in a text editor or IDE and add hello basic HTML and CSS code for hello page (hello PHP code will be added in later steps).</span></span>
   
        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> tooregister.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php
   
        ?>
        </body>
        </html>
7. <span data-ttu-id="22a72-168">Lägga till PHP-kod för att ansluta databasen toohello inom hello PHP-taggar.</span><span class="sxs-lookup"><span data-stu-id="22a72-168">Within hello PHP tags, add PHP code for connecting toohello database.</span></span>
   
        // DB connection info
        $host = "localhost\sqlexpress";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect toodatabase.
        try {
            $conn = new PDO( "sqlsrv:Server= $host ; Database = $db ", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }
   
    <span data-ttu-id="22a72-169">Igen, behöver du tooupdate hello värden för <code>$user</code> och <code>$pwd</code> med din lokala MySQL-användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="22a72-169">Again, you will need tooupdate hello values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
8. <span data-ttu-id="22a72-170">Följande hello databasen anslutning kod, lägger du till kod för att infoga registreringsinformation i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="22a72-170">Following hello database connection code, add code for inserting registration information into hello database.</span></span>
   
        if(!empty($_POST)) {
        try {
            $name = $_POST['name'];
            $email = $_POST['email'];
            $date = date("Y-m-d");
            // Insert data
            $sql_insert = "INSERT INTO registration_tbl (name, email, date) 
                           VALUES (?,?,?)";
            $stmt = $conn->prepare($sql_insert);
            $stmt->bindValue(1, $name);
            $stmt->bindValue(2, $email);
            $stmt->bindValue(3, $date);
            $stmt->execute();
        }
        catch(Exception $e) {
            die(var_dump($e));
        }
        echo "<h3>Your're registered!</h3>";
        }
9. <span data-ttu-id="22a72-171">Slutligen följande hello koden ovan, lägger du till kod för att hämta data från hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="22a72-171">Finally, following hello code above, add code for retrieving data from hello database.</span></span>
   
        $sql_select = "SELECT * FROM registration_tbl";
        $stmt = $conn->query($sql_select);
        $registrants = $stmt->fetchAll(); 
        if(count($registrants) > 0) {
            echo "<h2>People who are registered:</h2>";
            echo "<table>";
            echo "<tr><th>Name</th>";
            echo "<th>Email</th>";
            echo "<th>Date</th></tr>";
            foreach($registrants as $registrant) {
                echo "<tr><td>".$registrant['name']."</td>";
                echo "<td>".$registrant['email']."</td>";
                echo "<td>".$registrant['date']."</td></tr>";
            }
             echo "</table>";
        } else {
            echo "<h3>No one is currently registered.</h3>";
        }

<span data-ttu-id="22a72-172">Du kan nu Bläddra för**http://localhost:8000/index.php** tootest hello program.</span><span class="sxs-lookup"><span data-stu-id="22a72-172">You can now browse too**http://localhost:8000/index.php** tootest hello application.</span></span>

## <a name="publish-your-application"></a><span data-ttu-id="22a72-173">Publicera programmet</span><span class="sxs-lookup"><span data-stu-id="22a72-173">Publish your application</span></span>
<span data-ttu-id="22a72-174">När du har testat programmet lokalt, kan du publicera den tooApp Service Web Apps med Git.</span><span class="sxs-lookup"><span data-stu-id="22a72-174">After you have tested your application locally, you can publish it tooApp Service Web Apps using Git.</span></span> <span data-ttu-id="22a72-175">Du måste dock först tooupdate hello databasens anslutningsinformation hello program.</span><span class="sxs-lookup"><span data-stu-id="22a72-175">However, you first need tooupdate hello database connection information in hello application.</span></span> <span data-ttu-id="22a72-176">Med hjälp av hello databasens anslutningsinformation som du tidigare hämtade (i hello **Hämta SQL-databas anslutningsinformationen** avsnitt), uppdatering hello följande information i **både** hello `createdatabase.php` och `index.php` filer med hello lämpliga värden:</span><span class="sxs-lookup"><span data-stu-id="22a72-176">Using hello database connection information you obtained earlier (in hello **Get SQL Database connection information** section), update hello following information in **both** hello `createdatabase.php` and `index.php` files with hello appropriate values:</span></span>

    // DB connection info
    $host = "tcp:<value of Server>";
    $user = "<value of User Name>";
    $pwd = "<your password>";
    $db = "<value of SQL Database>";

> [!NOTE]
> <span data-ttu-id="22a72-177">I hello <code>$host</code>, hello värdet på servern måste vara föregås av <code>tcp:</code>.</span><span class="sxs-lookup"><span data-stu-id="22a72-177">In hello <code>$host</code>, hello value of Server must be prepended with <code>tcp:</code>.</span></span>
> 
> 

<span data-ttu-id="22a72-178">Nu är du redo tooset in Git-publicering och publicera programmet hello.</span><span class="sxs-lookup"><span data-stu-id="22a72-178">Now, you are ready tooset up Git publishing and publish hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="22a72-179">Det här är samma steg som anges i hello slutet av hello hello **skapa en Azure webbapp och konfigurera Git-publicering** ovan.</span><span class="sxs-lookup"><span data-stu-id="22a72-179">These are hello same steps noted at hello end of hello **Create an Azure web app and set up Git publishing** section above.</span></span>
> 
> 

1. <span data-ttu-id="22a72-180">Öppna GitBash (eller en terminal om Git i din `PATH`), ändra kataloger toohello rotkatalogen för programmet (hello **registrering** directory), och kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="22a72-180">Open GitBash (or a terminal, if Git is in your `PATH`), change directories toohello root directory of your application (hello **registration** directory), and run hello following commands:</span></span>
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    <span data-ttu-id="22a72-181">Du uppmanas att hello-lösenord som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="22a72-181">You will be prompted for hello password you created earlier.</span></span>
2. <span data-ttu-id="22a72-182">Bläddra för**http://[web app name].azurewebsites.net/createtable.php** toocreate hello SQL-databastabell för hello program.</span><span class="sxs-lookup"><span data-stu-id="22a72-182">Browse too**http://[web app name].azurewebsites.net/createtable.php** toocreate hello SQL database table for hello application.</span></span>
3. <span data-ttu-id="22a72-183">Bläddra för**http://[web app name].azurewebsites.net/index.php** toobegin med hello program.</span><span class="sxs-lookup"><span data-stu-id="22a72-183">Browse too**http://[web app name].azurewebsites.net/index.php** toobegin using hello application.</span></span>

<span data-ttu-id="22a72-184">När du har publicerat programmet, du kan göra ändringar tooit och använda Git toopublish dem.</span><span class="sxs-lookup"><span data-stu-id="22a72-184">After you have published your application, you can begin making changes tooit and use Git toopublish them.</span></span> 

## <a name="publish-changes-tooyour-application"></a><span data-ttu-id="22a72-185">Publicera ändringar tooyour program</span><span class="sxs-lookup"><span data-stu-id="22a72-185">Publish changes tooyour application</span></span>
<span data-ttu-id="22a72-186">toopublish ändras tooapplication, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="22a72-186">toopublish changes tooapplication, follow these steps:</span></span>

1. <span data-ttu-id="22a72-187">Ändra tooyour programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="22a72-187">Make changes tooyour application locally.</span></span>
2. <span data-ttu-id="22a72-188">Öppna GitBash (eller en terminal it Git finns i din `PATH`), ändra kataloger toohello rotkatalogen för programmet och kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="22a72-188">Open GitBash (or a terminal, it Git is in your `PATH`), change directories toohello root directory of your application, and run hello following commands:</span></span>
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    <span data-ttu-id="22a72-189">Du uppmanas att hello-lösenord som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="22a72-189">You will be prompted for hello password you created earlier.</span></span>
3. <span data-ttu-id="22a72-190">Bläddra för**http://[web app name].azurewebsites.net/index.php** toosee ändringarna.</span><span class="sxs-lookup"><span data-stu-id="22a72-190">Browse too**http://[web app name].azurewebsites.net/index.php** toosee your changes.</span></span>

## <a name="whats-changed"></a><span data-ttu-id="22a72-191">Nyheter</span><span class="sxs-lookup"><span data-stu-id="22a72-191">What's changed</span></span>
* <span data-ttu-id="22a72-192">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="22a72-192">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[pdo-sqlsrv]: http://php.net/pdo_sqlsrv

