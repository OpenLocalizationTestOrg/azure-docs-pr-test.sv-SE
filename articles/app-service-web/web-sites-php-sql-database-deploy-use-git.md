---
title: Skapa en PHP-SQL-webbapp och distribuera till Azure App Service med Git
description: "En genomgång som visar hur du skapar en PHP-webbapp som lagrar data i Azure SQL Database och använda Git-distribution till Azure App Service."
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
ms.openlocfilehash: 0baa3eced3824fec0907ca937c594f127a2bdf8b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-php-sql-web-app-and-deploy-to-azure-app-service-using-git"></a><span data-ttu-id="a00c8-103">Skapa en PHP-SQL-webbapp och distribuera till Azure App Service med Git</span><span class="sxs-lookup"><span data-stu-id="a00c8-103">Create a PHP-SQL web app and deploy to Azure App Service using Git</span></span>
<span data-ttu-id="a00c8-104">Den här kursen visar hur du skapar en PHP-webbapp i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) som ansluter till Azure SQL Database och hur du distribuerar den med Git.</span><span class="sxs-lookup"><span data-stu-id="a00c8-104">This tutorial shows you how to create a PHP web app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) that connects to Azure SQL Database and how to deploy it using Git.</span></span> <span data-ttu-id="a00c8-105">Den här kursen förutsätter att du har [PHP][install-php], [SQL Server Express][install-SQLExpress], [Drivers Microsoft för SQL Server för PHP ](http://www.microsoft.com/download/en/details.aspx?id=20098), och [Git] [ install-git] installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="a00c8-105">This tutorial assumes you have [PHP][install-php], [SQL Server Express][install-SQLExpress], the [Microsoft Drivers for SQL Server for PHP](http://www.microsoft.com/download/en/details.aspx?id=20098), and [Git][install-git] installed on your computer.</span></span> <span data-ttu-id="a00c8-106">När du slutför den här guiden kommer du har ett PHP-SQL-webbprogram som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="a00c8-106">Upon completing this guide, you will have a PHP-SQL web app running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="a00c8-107">Du kan installera och konfigurera PHP, SQL Server Express och Microsoft Drivers för SQL Server för PHP med hjälp av den [Microsoft Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="a00c8-107">You can install and configure PHP, SQL Server Express, and the Microsoft Drivers for SQL Server for PHP using the [Microsoft Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> 
> 

<span data-ttu-id="a00c8-108">Du kommer att lära dig:</span><span class="sxs-lookup"><span data-stu-id="a00c8-108">You will learn:</span></span>

* <span data-ttu-id="a00c8-109">Så här skapar du ett Azure webbapp och en SQL-databas med hjälp av den [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span><span class="sxs-lookup"><span data-stu-id="a00c8-109">How to create an Azure web app and a SQL Database using the [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span></span> <span data-ttu-id="a00c8-110">Eftersom PHP är aktiverad i App Service Web Apps som standard, krävs inget särskilt att köra din PHP-kod.</span><span class="sxs-lookup"><span data-stu-id="a00c8-110">Because PHP is enabled in App Service Web Apps by default, nothing special is required to run your PHP code.</span></span>
* <span data-ttu-id="a00c8-111">Hur man publicerar och publicera programmet till Azure med Git.</span><span class="sxs-lookup"><span data-stu-id="a00c8-111">How to publish and re-publish your application to Azure using Git.</span></span>

<span data-ttu-id="a00c8-112">Genom att följa den här självstudiekursen skapar du en enkel registrering webbprogram i PHP.</span><span class="sxs-lookup"><span data-stu-id="a00c8-112">By following this tutorial, you will build a simple registration web application in PHP.</span></span> <span data-ttu-id="a00c8-113">Programmet ska finnas i en Azure-webbplats.</span><span class="sxs-lookup"><span data-stu-id="a00c8-113">The application will be hosted in an Azure Website.</span></span> <span data-ttu-id="a00c8-114">En skärmbild av det färdiga programmet understiger:</span><span class="sxs-lookup"><span data-stu-id="a00c8-114">A screenshot of the completed application is below:</span></span>

![Azure PHP-webbplats](./media/web-sites-php-sql-database-deploy-use-git/running_app_3.png)

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="a00c8-116">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="a00c8-116">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="a00c8-117">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="a00c8-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-an-azure-web-app-and-set-up-git-publishing"></a><span data-ttu-id="a00c8-118">Skapa en Azure webbapp och konfigurera Git-publicering</span><span class="sxs-lookup"><span data-stu-id="a00c8-118">Create an Azure web app and set up Git publishing</span></span>
<span data-ttu-id="a00c8-119">Följ dessa steg om du vill skapa ett Azure webbapp och en SQL-databas:</span><span class="sxs-lookup"><span data-stu-id="a00c8-119">Follow these steps to create an Azure web app and a SQL Database:</span></span>

1. <span data-ttu-id="a00c8-120">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a00c8-120">Log in to the [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="a00c8-121">Öppna Azure Marketplace genom att klicka på **ny** ikonen längst upp till vänster i instrumentpanelen, klicka på **Markera alla** bredvid Marketplace och välja **webb + mobilt**.</span><span class="sxs-lookup"><span data-stu-id="a00c8-121">Open the Azure Marketplace by clicking the **New** icon on the top left of the dashboard, click on **Select All** next to Marketplace and selecting **Web + Mobile**.</span></span>
3. <span data-ttu-id="a00c8-122">Välj i Marketplace, **webb + mobilt**.</span><span class="sxs-lookup"><span data-stu-id="a00c8-122">In the Marketplace, select **Web + Mobile**.</span></span>
4. <span data-ttu-id="a00c8-123">Klicka på den **Webbapp + SQL** ikon.</span><span class="sxs-lookup"><span data-stu-id="a00c8-123">Click the **Web app + SQL** icon.</span></span>
5. <span data-ttu-id="a00c8-124">När du har läst igenom beskrivningen av webbapp + SQL app markerar **skapa**.</span><span class="sxs-lookup"><span data-stu-id="a00c8-124">After reading the description of the Web app + SQL app, select **Create**.</span></span>
6. <span data-ttu-id="a00c8-125">Klicka på varje del (**resursgruppen**, **Webbapp**, **databasen**, och **prenumeration**) och ange eller Välj värden för de obligatoriska fälten :</span><span class="sxs-lookup"><span data-stu-id="a00c8-125">Click on each part (**Resource Group**, **Web App**, **Database**, and **Subscription**) and enter or select values for the required fields:</span></span>
   
   * <span data-ttu-id="a00c8-126">Ange ett URL-namn du väljer</span><span class="sxs-lookup"><span data-stu-id="a00c8-126">Enter a URL name of your choice</span></span>    
   * <span data-ttu-id="a00c8-127">Konfigurera servern Databasautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="a00c8-127">Configure database server credentials</span></span>
   * <span data-ttu-id="a00c8-128">Välj regionen som är närmast dig</span><span class="sxs-lookup"><span data-stu-id="a00c8-128">Select the region closest to you</span></span>
     
     ![konfigurera appen](./media/web-sites-php-sql-database-deploy-use-git/configure-db-settings.png)
7. <span data-ttu-id="a00c8-130">När du definierat webbappen, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="a00c8-130">When finished defining the web app, click **Create**.</span></span>
   
    <span data-ttu-id="a00c8-131">När webbappen har skapats kan den **meddelanden** knappen blinkar en grön **lyckade** och bladet för resursgruppen öppna om du vill visa både webbappen och SQL-databas i gruppen.</span><span class="sxs-lookup"><span data-stu-id="a00c8-131">When the web app has been created, the **Notifications** button will flash a green **SUCCESS** and the resource group blade open to show both the web app and the SQL database in the group.</span></span>
8. <span data-ttu-id="a00c8-132">Klicka på ikonen för webbappens i bladet för resursgruppen att öppna den webbapps blad.</span><span class="sxs-lookup"><span data-stu-id="a00c8-132">Click the web app's icon in the resource group blade to open the web app's blade.</span></span>
   
    ![Web Apps resursgrupp](./media/web-sites-php-sql-database-deploy-use-git/resource-group-blade.png)
9. <span data-ttu-id="a00c8-134">I **inställningar** klickar du på **kontinuerlig distribution** > **konfigurera nödvändiga inställningar**.</span><span class="sxs-lookup"><span data-stu-id="a00c8-134">In **Settings** click **Continuous deployment** > **Configure required settings**.</span></span> <span data-ttu-id="a00c8-135">Välj **lokal Git-lagringsplats** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a00c8-135">Select **Local Git Repository** and click **OK**.</span></span>
   
    ![var finns i källkoden](./media/web-sites-php-sql-database-deploy-use-git/setup-local-git.png)
   
    <span data-ttu-id="a00c8-137">Om du inte har registrerat en Git-lagringsplats innan måste du ange ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="a00c8-137">If you have not set up a Git repository before, you must provide a user name and password.</span></span> <span data-ttu-id="a00c8-138">Gör detta genom att klicka på **inställningar** > **distributionsbehörigheterna** webbappens-bladet.</span><span class="sxs-lookup"><span data-stu-id="a00c8-138">To do this, click **Settings** > **Deployment credentials** in the web app's blade.</span></span>
   
    ![](./media/web-sites-php-sql-database-deploy-use-git/deployment-credentials.png)
10. <span data-ttu-id="a00c8-139">I **inställningar** klickar du på **egenskaper** att se Git extern URL som du behöver använda för att distribuera din PHP-app senare.</span><span class="sxs-lookup"><span data-stu-id="a00c8-139">In **Settings** click on **Properties** to see the Git remote URL you need to use to deploy your PHP app later.</span></span>

## <a name="get-sql-database-connection-information"></a><span data-ttu-id="a00c8-140">Hämta anslutningsinformationen för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="a00c8-140">Get SQL Database connection information</span></span>
<span data-ttu-id="a00c8-141">Att ansluta till SQL Database-instans som är kopplad till ditt webbprogram, din kommer måste anslutningsinformation som du angav när du skapade databasen.</span><span class="sxs-lookup"><span data-stu-id="a00c8-141">To connect to the SQL Database instance that is linked to your web app, your will need the connection information, which you specified when you created the database.</span></span> <span data-ttu-id="a00c8-142">Följ dessa steg för att hämta anslutningsinformationen för SQL-databasen:</span><span class="sxs-lookup"><span data-stu-id="a00c8-142">To get the SQL Database connection information, follow these steps:</span></span>

1. <span data-ttu-id="a00c8-143">Tillbaka i den resursgrupp-bladet klickar du på ikonen för SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="a00c8-143">Back in the resource group's blade, click the SQL database's icon.</span></span>
2. <span data-ttu-id="a00c8-144">I bladet för SQL-databasen, klickar du på **inställningar** > **egenskaper**, klicka på **visa databasanslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="a00c8-144">In the SQL database's blade, click **Settings** > **Properties**, then click **Show database connection strings**.</span></span> 
   
    ![Visa egenskaper för databas](./media/web-sites-php-sql-database-deploy-use-git/view-database-properties.png)
3. <span data-ttu-id="a00c8-146">Från den **PHP** avsnitt i dialogrutan resulterande anteckna värdena för `Server`, `SQL Database`, och `User Name`.</span><span class="sxs-lookup"><span data-stu-id="a00c8-146">From the **PHP** section of the resulting dialog, make note of the values for `Server`, `SQL Database`, and `User Name`.</span></span> <span data-ttu-id="a00c8-147">Du använder dessa värden senare när du publicerar PHP-webbapp till Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="a00c8-147">You will use these values later when publishing your PHP web app to Azure App Service.</span></span>

## <a name="build-and-test-your-application-locally"></a><span data-ttu-id="a00c8-148">Skapa och testa programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="a00c8-148">Build and test your application locally</span></span>
<span data-ttu-id="a00c8-149">Registreringen programmet är ett enkelt PHP-program som gör att du kan registrera dig för en händelse genom att tillhandahålla ditt namn och e-postadress.</span><span class="sxs-lookup"><span data-stu-id="a00c8-149">The Registration application is a simple PHP application that allows you to register for an event by providing your name and email address.</span></span> <span data-ttu-id="a00c8-150">Information om föregående månaderna visas i en tabell.</span><span class="sxs-lookup"><span data-stu-id="a00c8-150">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="a00c8-151">Registreringsinformation lagras i en instans av SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a00c8-151">Registration information is stored in a SQL Database instance.</span></span> <span data-ttu-id="a00c8-152">Programmet består av två filer (kopiera och klistra in koden finns nedan):</span><span class="sxs-lookup"><span data-stu-id="a00c8-152">The application consists of two files (copy/paste code available below):</span></span>

* <span data-ttu-id="a00c8-153">**index.php**: Visar ett formulär för registrering och en tabell som innehåller registrant information.</span><span class="sxs-lookup"><span data-stu-id="a00c8-153">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>
* <span data-ttu-id="a00c8-154">**createtable.php**: skapar SQL-databastabell för programmet.</span><span class="sxs-lookup"><span data-stu-id="a00c8-154">**createtable.php**: Creates the SQL Database table for the application.</span></span> <span data-ttu-id="a00c8-155">Den här filen ska bara användas en gång.</span><span class="sxs-lookup"><span data-stu-id="a00c8-155">This file will only be used once.</span></span>

<span data-ttu-id="a00c8-156">Följ stegen nedan om du vill köra programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="a00c8-156">To run the application locally, follow the steps below.</span></span> <span data-ttu-id="a00c8-157">Observera att de här stegen förutsätter att du har PHP och SQL Server Express på den lokala datorn och att du har aktiverat den [PDO-tillägg för SQL Server][pdo-sqlsrv].</span><span class="sxs-lookup"><span data-stu-id="a00c8-157">Note that these steps assume you have PHP and SQL Server Express set up on your local machine, and that you have enabled the [PDO extension for SQL Server][pdo-sqlsrv].</span></span>

1. <span data-ttu-id="a00c8-158">Skapa en SQL Server-databas som heter `registration`.</span><span class="sxs-lookup"><span data-stu-id="a00c8-158">Create a SQL Server database called `registration`.</span></span> <span data-ttu-id="a00c8-159">Du kan göra detta från den `sqlcmd` Kommandotolken med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="a00c8-159">You can do this from the `sqlcmd` command prompt with these commands:</span></span>
   
        >sqlcmd -S localhost\sqlexpress -U <local user name> -P <local password>
        1> create database registration
        2> GO    
2. <span data-ttu-id="a00c8-160">Skapa två filer i den - en kallas i din tillämpningsprogrammets rotkatalog `createtable.php` och en kallas `index.php`.</span><span class="sxs-lookup"><span data-stu-id="a00c8-160">In your application root directory, create two files in it - one called `createtable.php` and one called `index.php`.</span></span>
3. <span data-ttu-id="a00c8-161">Öppna den `createtable.php` filen i en textredigerare eller IDE och lägga till koden nedan.</span><span class="sxs-lookup"><span data-stu-id="a00c8-161">Open the `createtable.php` file in a text editor or IDE and add the code below.</span></span> <span data-ttu-id="a00c8-162">Den här koden används för att skapa den `registration_tbl` tabell i den `registration` databas.</span><span class="sxs-lookup"><span data-stu-id="a00c8-162">This code will be used to create the `registration_tbl` table in the `registration` database.</span></span>
   
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
   
    <span data-ttu-id="a00c8-163">Observera att du måste uppdatera värdena för <code>$user</code> och <code>$pwd</code> med dina lokala SQL Server-användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="a00c8-163">Note that you will need to update the values for <code>$user</code> and <code>$pwd</code> with your local SQL Server user name and password.</span></span>
4. <span data-ttu-id="a00c8-164">Skriv följande kommando i en terminal i rotkatalogen för programmet:</span><span class="sxs-lookup"><span data-stu-id="a00c8-164">In a terminal at the root directory of the application type the following command:</span></span>
   
        php -S localhost:8000
5. <span data-ttu-id="a00c8-165">Öppna en webbläsare och gå till **http://localhost:8000/createtable.php**.</span><span class="sxs-lookup"><span data-stu-id="a00c8-165">Open a web browser and browse to **http://localhost:8000/createtable.php**.</span></span> <span data-ttu-id="a00c8-166">Detta skapar den `registration_tbl` tabell i databasen.</span><span class="sxs-lookup"><span data-stu-id="a00c8-166">This will create the `registration_tbl` table in the database.</span></span>
6. <span data-ttu-id="a00c8-167">Öppna den **index.php** filen i en textredigerare eller IDE och Lägg till grundläggande HTML- och CSS kod på sidan (PHP-koden kommer att läggas till i senare steg).</span><span class="sxs-lookup"><span data-stu-id="a00c8-167">Open the **index.php** file in a text editor or IDE and add the basic HTML and CSS code for the page (the PHP code will be added in later steps).</span></span>
   
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
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php
   
        ?>
        </body>
        </html>
7. <span data-ttu-id="a00c8-168">Lägga till PHP-kod för att ansluta till databasen i PHP-taggarna.</span><span class="sxs-lookup"><span data-stu-id="a00c8-168">Within the PHP tags, add PHP code for connecting to the database.</span></span>
   
        // DB connection info
        $host = "localhost\sqlexpress";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "sqlsrv:Server= $host ; Database = $db ", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }
   
    <span data-ttu-id="a00c8-169">Igen, måste du uppdatera värdena för <code>$user</code> och <code>$pwd</code> med din lokala MySQL-användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="a00c8-169">Again, you will need to update the values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
8. <span data-ttu-id="a00c8-170">Följande kod för databas-anslutningsfel lägger du till kod för att infoga registreringsinformation i databasen.</span><span class="sxs-lookup"><span data-stu-id="a00c8-170">Following the database connection code, add code for inserting registration information into the database.</span></span>
   
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
9. <span data-ttu-id="a00c8-171">Slutligen följande koden ovan lägger du till kod för att hämta data från databasen.</span><span class="sxs-lookup"><span data-stu-id="a00c8-171">Finally, following the code above, add code for retrieving data from the database.</span></span>
   
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

<span data-ttu-id="a00c8-172">Du kan nu bläddra till **http://localhost:8000/index.php** för att testa programmet.</span><span class="sxs-lookup"><span data-stu-id="a00c8-172">You can now browse to **http://localhost:8000/index.php** to test the application.</span></span>

## <a name="publish-your-application"></a><span data-ttu-id="a00c8-173">Publicera programmet</span><span class="sxs-lookup"><span data-stu-id="a00c8-173">Publish your application</span></span>
<span data-ttu-id="a00c8-174">När du har testat programmet lokalt, kan du publicera den till App Service Web Apps med Git.</span><span class="sxs-lookup"><span data-stu-id="a00c8-174">After you have tested your application locally, you can publish it to App Service Web Apps using Git.</span></span> <span data-ttu-id="a00c8-175">Men måste du först uppdatera anslutningsinformationen i programmet.</span><span class="sxs-lookup"><span data-stu-id="a00c8-175">However, you first need to update the database connection information in the application.</span></span> <span data-ttu-id="a00c8-176">Använder anslutningsinformationen för databasen som du tidigare hämtade (i den **Hämta SQL-databas anslutningsinformationen** avsnitt), uppdatera följande information i **både** i `createdatabase.php` och `index.php` filer med lämpliga värden:</span><span class="sxs-lookup"><span data-stu-id="a00c8-176">Using the database connection information you obtained earlier (in the **Get SQL Database connection information** section), update the following information in **both** the `createdatabase.php` and `index.php` files with the appropriate values:</span></span>

    // DB connection info
    $host = "tcp:<value of Server>";
    $user = "<value of User Name>";
    $pwd = "<your password>";
    $db = "<value of SQL Database>";

> [!NOTE]
> <span data-ttu-id="a00c8-177">I den <code>$host</code>, värdet för Server måste vara föregås av <code>tcp:</code>.</span><span class="sxs-lookup"><span data-stu-id="a00c8-177">In the <code>$host</code>, the value of Server must be prepended with <code>tcp:</code>.</span></span>
> 
> 

<span data-ttu-id="a00c8-178">Nu är du redo att konfigurera Git-publicering och publicera programmet.</span><span class="sxs-lookup"><span data-stu-id="a00c8-178">Now, you are ready to set up Git publishing and publish the application.</span></span>

> [!NOTE]
> <span data-ttu-id="a00c8-179">Det här är samma steg som anges i slutet av den **skapa en Azure webbapp och konfigurera Git-publicering** ovan.</span><span class="sxs-lookup"><span data-stu-id="a00c8-179">These are the same steps noted at the end of the **Create an Azure web app and set up Git publishing** section above.</span></span>
> 
> 

1. <span data-ttu-id="a00c8-180">Öppna GitBash (eller en terminal om Git i din `PATH`), ändrar sökvägen till rotkatalogen för programmet (den **registrering** directory), och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="a00c8-180">Open GitBash (or a terminal, if Git is in your `PATH`), change directories to the root directory of your application (the **registration** directory), and run the following commands:</span></span>
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    <span data-ttu-id="a00c8-181">Du uppmanas att lösenordet du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a00c8-181">You will be prompted for the password you created earlier.</span></span>
2. <span data-ttu-id="a00c8-182">Bläddra till **http://[web app name].azurewebsites.net/createtable.php** att skapa SQL-databastabell för programmet.</span><span class="sxs-lookup"><span data-stu-id="a00c8-182">Browse to **http://[web app name].azurewebsites.net/createtable.php** to create the SQL database table for the application.</span></span>
3. <span data-ttu-id="a00c8-183">Bläddra till **http://[web app name].azurewebsites.net/index.php** kan börja använda programmet.</span><span class="sxs-lookup"><span data-stu-id="a00c8-183">Browse to **http://[web app name].azurewebsites.net/index.php** to begin using the application.</span></span>

<span data-ttu-id="a00c8-184">När du har publicerat programmet kan du använda Git för att publicera dem börjar att göra ändringar.</span><span class="sxs-lookup"><span data-stu-id="a00c8-184">After you have published your application, you can begin making changes to it and use Git to publish them.</span></span> 

## <a name="publish-changes-to-your-application"></a><span data-ttu-id="a00c8-185">Publicera ändringar i programmet</span><span class="sxs-lookup"><span data-stu-id="a00c8-185">Publish changes to your application</span></span>
<span data-ttu-id="a00c8-186">Följ dessa steg om du vill publicera ändringar för programmet:</span><span class="sxs-lookup"><span data-stu-id="a00c8-186">To publish changes to application, follow these steps:</span></span>

1. <span data-ttu-id="a00c8-187">Göra ändringar i programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="a00c8-187">Make changes to your application locally.</span></span>
2. <span data-ttu-id="a00c8-188">Öppna GitBash (eller en terminal it Git finns i din `PATH`), ändrar sökvägen till rotkatalogen för programmet och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="a00c8-188">Open GitBash (or a terminal, it Git is in your `PATH`), change directories to the root directory of your application, and run the following commands:</span></span>
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    <span data-ttu-id="a00c8-189">Du uppmanas att lösenordet du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a00c8-189">You will be prompted for the password you created earlier.</span></span>
3. <span data-ttu-id="a00c8-190">Bläddra till **http://[web app name].azurewebsites.net/index.php** att se dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="a00c8-190">Browse to **http://[web app name].azurewebsites.net/index.php** to see your changes.</span></span>

## <a name="whats-changed"></a><span data-ttu-id="a00c8-191">Nyheter</span><span class="sxs-lookup"><span data-stu-id="a00c8-191">What's changed</span></span>
* <span data-ttu-id="a00c8-192">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="a00c8-192">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[pdo-sqlsrv]: http://php.net/pdo_sqlsrv

