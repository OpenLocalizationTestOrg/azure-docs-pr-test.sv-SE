---
title: aaaCreate PHP-MySQL webbapp i Azure App Service och distribuera med FTP
description: "En genomgång som visar hur toocreate en PHP webbapp som lagrar data i MySQL och Använd FTP-distribution tooAzure."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 6d9d1de5-5868-48fd-8bad-decb4979cd65
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 4d3b56a8ac63d0eba0dc0aec1b62e6d12f601bf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a><span data-ttu-id="18585-103">Skapa en PHP-MySQL-webbapp i Azure App Service och distribuera med FTP</span><span class="sxs-lookup"><span data-stu-id="18585-103">Create a PHP-MySQL web app in Azure App Service and deploy using FTP</span></span>
<span data-ttu-id="18585-104">Den här kursen visar hur toocreate PHP-MySQL web app och toodeploy den med hjälp av FTP.</span><span class="sxs-lookup"><span data-stu-id="18585-104">This tutorial shows you how toocreate a PHP-MySQL web app and how toodeploy it using FTP.</span></span> <span data-ttu-id="18585-105">Den här kursen förutsätter att du har [PHP][install-php], [MySQL][install-mysql], en webbserver och en FTP-klient installerad på datorn.</span><span class="sxs-lookup"><span data-stu-id="18585-105">This tutorial assumes you have [PHP][install-php], [MySQL][install-mysql], a web server, and an FTP client installed on your computer.</span></span> <span data-ttu-id="18585-106">hello anvisningarna i den här kursen kan tillämpas på alla operativsystem, inklusive Windows, Mac- och Linux.</span><span class="sxs-lookup"><span data-stu-id="18585-106">hello instructions in this tutorial can be followed on any operating system, including Windows, Mac, and  Linux.</span></span> <span data-ttu-id="18585-107">När du slutför den här guiden kommer du har ett webbprogram för PHP/MySQL som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="18585-107">Upon completing this guide, you will have a PHP/MySQL web app running in Azure.</span></span>

<span data-ttu-id="18585-108">Du kommer att lära dig:</span><span class="sxs-lookup"><span data-stu-id="18585-108">You will learn:</span></span>

* <span data-ttu-id="18585-109">Hur hello toocreate ett webbprogram och en MySQL-databas med hjälp av Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="18585-109">How toocreate a web app and a MySQL database using hello Azure Portal.</span></span> <span data-ttu-id="18585-110">Eftersom PHP är aktiverad i Web Apps som standard, inget särskilt är obligatoriska toorun PHP-kod.</span><span class="sxs-lookup"><span data-stu-id="18585-110">Because PHP is enabled in Web Apps by default, nothing special is required toorun your PHP code.</span></span>
* <span data-ttu-id="18585-111">Hur toopublish tooAzure appen med FTP.</span><span class="sxs-lookup"><span data-stu-id="18585-111">How toopublish your application tooAzure using FTP.</span></span>

<span data-ttu-id="18585-112">Genom att följa den här självstudiekursen skapar du en enkel registrering webbapp i PHP.</span><span class="sxs-lookup"><span data-stu-id="18585-112">By following this tutorial, you will build a simple registration web app in PHP.</span></span> <span data-ttu-id="18585-113">hello program ska finnas i en Webbapp.</span><span class="sxs-lookup"><span data-stu-id="18585-113">hello application will be hosted in a Web App.</span></span> <span data-ttu-id="18585-114">En skärmbild av programmet hello slutförts understiger:</span><span class="sxs-lookup"><span data-stu-id="18585-114">A screenshot of hello completed application is below:</span></span>

![Azure PHP-webbplats][running-app]

> [!NOTE]
> <span data-ttu-id="18585-116">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett konto, gå för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="18585-116">If you want tooget started with Azure App Service before signing up for an account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="18585-117">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="18585-117">No credit cards required, no commitments.</span></span> 
> 
> 

## <a name="create-a-web-app-and-set-up-ftp-publishing"></a><span data-ttu-id="18585-118">Skapa en webbapp och konfigurera FTP-publicering</span><span class="sxs-lookup"><span data-stu-id="18585-118">Create a web app and set up FTP publishing</span></span>
<span data-ttu-id="18585-119">Följ dessa steg toocreate ett webbprogram och en MySQL-databas:</span><span class="sxs-lookup"><span data-stu-id="18585-119">Follow these steps toocreate a web app and a MySQL database:</span></span>

1. <span data-ttu-id="18585-120">Inloggningen toohello [Azure Portal][management-portal].</span><span class="sxs-lookup"><span data-stu-id="18585-120">Login toohello [Azure Portal][management-portal].</span></span>
2. <span data-ttu-id="18585-121">Klicka på hello **+ ny** ikonen hello längst upp till vänster i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="18585-121">Click hello **+ New** icon on hello top left of hello Azure Portal.</span></span>
   
    ![Skapa ny Azure-webbplats][new-website]
3. <span data-ttu-id="18585-123">I hello söktyp **Webbapp + MySQL** och klicka på **Webbapp + MySQL**.</span><span class="sxs-lookup"><span data-stu-id="18585-123">In hello search type **Web app + MySQL** and click on **Web app + MySQL**.</span></span>
   
    ![Anpassade skapa en ny webbplats][custom-create]
4. <span data-ttu-id="18585-125">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="18585-125">Click **Create**.</span></span> <span data-ttu-id="18585-126">Ange ett unikt appnamn för tjänsten, ett giltigt namn för hello resursgruppens namn och en ny serviceplan.</span><span class="sxs-lookup"><span data-stu-id="18585-126">Enter a unique app service name, a valid name for hello resource group and a new service plan.</span></span>
   
    ![Ange resursgruppens namn][resource-group]
5. <span data-ttu-id="18585-128">Ange värden för den nya databasen, inklusive godkänner toohello juridiska villkor.</span><span class="sxs-lookup"><span data-stu-id="18585-128">Enter values for your new database, including agreeing toohello legal terms.</span></span>
   
    ![Skapa ny MySQL-databas][new-mysql-db]
6. <span data-ttu-id="18585-130">När hello webbprogrammet har skapats visas hello nya app service-bladet.</span><span class="sxs-lookup"><span data-stu-id="18585-130">When hello web app has been created, you will see hello new app service blade.</span></span>
7. <span data-ttu-id="18585-131">Klicka på **inställningar** > **distributionsbehörigheterna**.</span><span class="sxs-lookup"><span data-stu-id="18585-131">Click on **Settings** > **Deployment credentials**.</span></span> 
   
    ![Ange autentiseringsuppgifter för distribution][set-deployment-credentials]
8. <span data-ttu-id="18585-133">tooenable FTP-publicering, måste du ange ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="18585-133">tooenable FTP publishing, you must provide a user name and password.</span></span> <span data-ttu-id="18585-134">Spara hello autentiseringsuppgifter och anteckna hello användarnamn och lösenord som du skapar.</span><span class="sxs-lookup"><span data-stu-id="18585-134">Save hello credentials and make a note of hello user name and password you create.</span></span>
   
    ![Skapa autentiseringsuppgifter för publicering][portal-ftp-username-password]

## <a name="build-and-test-your-app-locally"></a><span data-ttu-id="18585-136">Skapa och testa appen lokalt</span><span class="sxs-lookup"><span data-stu-id="18585-136">Build and test your app locally</span></span>
<span data-ttu-id="18585-137">hello registreringen programmet är ett enkelt PHP-program som du kan använda tooregister för en händelse genom att tillhandahålla ditt namn och e-postadress.</span><span class="sxs-lookup"><span data-stu-id="18585-137">hello Registration application is a simple PHP application that allows you tooregister for an event by providing your name and email address.</span></span> <span data-ttu-id="18585-138">Information om föregående månaderna visas i en tabell.</span><span class="sxs-lookup"><span data-stu-id="18585-138">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="18585-139">Registreringsinformation lagras i en MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="18585-139">Registration information is stored in a MySQL database.</span></span> <span data-ttu-id="18585-140">hello app består av två filer:</span><span class="sxs-lookup"><span data-stu-id="18585-140">hello app consists of two files:</span></span>

* <span data-ttu-id="18585-141">**index.php**: Visar ett formulär för registrering och en tabell som innehåller registrant information.</span><span class="sxs-lookup"><span data-stu-id="18585-141">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>
* <span data-ttu-id="18585-142">**createtable.php**: skapar hello MySQL tabell för hello program.</span><span class="sxs-lookup"><span data-stu-id="18585-142">**createtable.php**: Creates hello MySQL table for hello application.</span></span> <span data-ttu-id="18585-143">Den här filen ska bara användas en gång.</span><span class="sxs-lookup"><span data-stu-id="18585-143">This file will only be used once.</span></span>

<span data-ttu-id="18585-144">toobuild och kör hello appen lokalt, gör hello nedan.</span><span class="sxs-lookup"><span data-stu-id="18585-144">toobuild and run hello app locally, follow hello steps below.</span></span> <span data-ttu-id="18585-145">Observera att dessa åtgärder förutsätter att du har PHP, MySQL och en webbserver som konfigurerats på den lokala datorn och att du har aktiverat hello [PDO-tillägget för MySQL][pdo-mysql].</span><span class="sxs-lookup"><span data-stu-id="18585-145">Note that these steps assume you have PHP, MySQL, and a web server set up on your local machine, and that you have enabled hello [PDO extension for MySQL][pdo-mysql].</span></span>

1. <span data-ttu-id="18585-146">Skapa en MySQL-databas som heter `registration`.</span><span class="sxs-lookup"><span data-stu-id="18585-146">Create a MySQL database called `registration`.</span></span> <span data-ttu-id="18585-147">Du kan göra detta från kommandoraden hello MySQL med det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="18585-147">You can do this from hello MySQL command prompt with this command:</span></span>
   
        mysql> create database registration;
2. <span data-ttu-id="18585-148">Skapa en mapp med namnet i rotkatalogen för din webbserver `registration` och skapa två filer i den - en kallas `createtable.php` och en kallas `index.php`.</span><span class="sxs-lookup"><span data-stu-id="18585-148">In your web server's root directory, create a folder called `registration` and create two files in it - one called `createtable.php` and one called `index.php`.</span></span>
3. <span data-ttu-id="18585-149">Öppna hello `createtable.php` filen i en textredigerare eller IDE och Lägg till hello koden nedan.</span><span class="sxs-lookup"><span data-stu-id="18585-149">Open hello `createtable.php` file in a text editor or IDE and add hello code below.</span></span> <span data-ttu-id="18585-150">Den här koden kommer att använda toocreate hello `registration_tbl` tabellen i hello `registration` databas.</span><span class="sxs-lookup"><span data-stu-id="18585-150">This code will be used toocreate hello `registration_tbl` table in hello `registration` database.</span></span>
   
        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
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
   
   > [!NOTE]
   > <span data-ttu-id="18585-151">Behöver du tooupdate hello värden för <code>$user</code> och <code>$pwd</code> med din lokala MySQL-användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="18585-151">You will need tooupdate hello values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
   > 
   > 
4. <span data-ttu-id="18585-152">Öppna en webbläsare och gå för[http://localhost/registration/createtable.php][localhost-createtable].</span><span class="sxs-lookup"><span data-stu-id="18585-152">Open a web browser and browse too[http://localhost/registration/createtable.php][localhost-createtable].</span></span> <span data-ttu-id="18585-153">Detta skapar hello `registration_tbl` tabellen i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="18585-153">This will create hello `registration_tbl` table in hello database.</span></span>
5. <span data-ttu-id="18585-154">Öppna hello **index.php** filen i en textredigerare eller IDE och Lägg till hello grundläggande HTML- eller CSS-kod för hello sida (hello PHP kod läggs i senare steg).</span><span class="sxs-lookup"><span data-stu-id="18585-154">Open hello **index.php** file in a text editor or IDE and add hello basic HTML and CSS code for hello page (hello PHP code will be added in later steps).</span></span>
   
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
6. <span data-ttu-id="18585-155">Lägga till PHP-kod för att ansluta databasen toohello inom hello PHP-taggar.</span><span class="sxs-lookup"><span data-stu-id="18585-155">Within hello PHP tags, add PHP code for connecting toohello database.</span></span>
   
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect toodatabase.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }
   
   > [!NOTE]
   > <span data-ttu-id="18585-156">Igen, behöver du tooupdate hello värden för <code>$user</code> och <code>$pwd</code> med din lokala MySQL-användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="18585-156">Again, you will need tooupdate hello values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
   > 
   > 
7. <span data-ttu-id="18585-157">Följande hello databasen anslutning kod, lägger du till kod för att infoga registreringsinformation i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="18585-157">Following hello database connection code, add code for inserting registration information into hello database.</span></span>
   
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
8. <span data-ttu-id="18585-158">Slutligen följande hello koden ovan, lägger du till kod för att hämta data från hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="18585-158">Finally, following hello code above, add code for retrieving data from hello database.</span></span>
   
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

<span data-ttu-id="18585-159">Du kan nu Bläddra för[http://localhost/registration/index.php] [ localhost-index] tootest hello app.</span><span class="sxs-lookup"><span data-stu-id="18585-159">You can now browse too[http://localhost/registration/index.php][localhost-index] tootest hello app.</span></span>

## <a name="get-mysql-and-ftp-connection-information"></a><span data-ttu-id="18585-160">Hämta MySQL- och FTP-anslutning</span><span class="sxs-lookup"><span data-stu-id="18585-160">Get MySQL and FTP connection information</span></span>
<span data-ttu-id="18585-161">tooconnect toohello MySQL-databas som körs i Web Apps din kommer måste hello anslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="18585-161">tooconnect toohello MySQL database that is running in Web Apps, your will need hello connection information.</span></span> <span data-ttu-id="18585-162">tooget MySQL anslutningsinformationen så här:</span><span class="sxs-lookup"><span data-stu-id="18585-162">tooget MySQL connection information, follow these steps:</span></span>

1. <span data-ttu-id="18585-163">Från hello app service web app-bladet klickar du på hello resurs grupp länk:</span><span class="sxs-lookup"><span data-stu-id="18585-163">From hello app service web app blade click on hello resource group link:</span></span>
   
    ![Välj resursgrupp][select-resourcegroup]
2. <span data-ttu-id="18585-165">Klicka på hello databas från din resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="18585-165">From your resource group, click hello database:</span></span>
   
    ![Välj databas][select-database]
3. <span data-ttu-id="18585-167">Hello databasen sammanfattning, Välj **inställningar** > **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="18585-167">From hello database summary, select **Settings** > **Properties**.</span></span>
   
    ![Välj egenskaper][select-properties]
4. <span data-ttu-id="18585-169">Anteckna hello värden för `Database`, `Host`, `User Id`, och `Password`.</span><span class="sxs-lookup"><span data-stu-id="18585-169">Make note of hello values for `Database`, `Host`, `User Id`, and `Password`.</span></span>
   
    ![Obs egenskaper][note-properties]
5. <span data-ttu-id="18585-171">Klicka på ditt webbprogram hello **hämta Publicera profil** längst hello nedre högra hörnet av hello sidan:</span><span class="sxs-lookup"><span data-stu-id="18585-171">From your web app, click hello **Download publish profile** link at hello bottom right corner of hello page:</span></span>
   
    ![Hämta Publicera profil][download-publish-profile]
6. <span data-ttu-id="18585-173">Öppna hello `.publishsettings` fil i en XML-redigerare.</span><span class="sxs-lookup"><span data-stu-id="18585-173">Open hello `.publishsettings` file in an XML editor.</span></span> 
7. <span data-ttu-id="18585-174">Hitta hello `<publishProfile >` element med `publishMethod="FTP"` som ser ut ungefär toothis:</span><span class="sxs-lookup"><span data-stu-id="18585-174">Find hello `<publishProfile >` element with `publishMethod="FTP"` that looks similar toothis:</span></span>
   
        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>

<span data-ttu-id="18585-175">Anteckna hello `publishUrl`, `userName`, och `userPWD` attribut.</span><span class="sxs-lookup"><span data-stu-id="18585-175">Make note of hello `publishUrl`, `userName`, and `userPWD` attributes.</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="18585-176">Publicera appen</span><span class="sxs-lookup"><span data-stu-id="18585-176">Publish your app</span></span>
<span data-ttu-id="18585-177">När du har testat appen lokalt kan publicera du den tooyour webbprogram med hjälp av FTP.</span><span class="sxs-lookup"><span data-stu-id="18585-177">After you have tested your app locally, you can publish it tooyour web app using FTP.</span></span> <span data-ttu-id="18585-178">Du måste dock först tooupdate hello databasens anslutningsinformation hello program.</span><span class="sxs-lookup"><span data-stu-id="18585-178">However, you first need tooupdate hello database connection information in hello application.</span></span> <span data-ttu-id="18585-179">Med hjälp av hello databasens anslutningsinformation som du tidigare hämtade (i hello **hämta MySQL och FTP-anslutningsinformationen** avsnitt), uppdatering hello följande information i **både** hello `createdatabase.php` och `index.php` filer med hello lämpliga värden:</span><span class="sxs-lookup"><span data-stu-id="18585-179">Using hello database connection information you obtained earlier (in hello **Get MySQL and FTP connection information** section), update hello following information in **both** hello `createdatabase.php` and `index.php` files with hello appropriate values:</span></span>

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

<span data-ttu-id="18585-180">Nu är du redo toopublish appen med hjälp av FTP.</span><span class="sxs-lookup"><span data-stu-id="18585-180">Now you are ready toopublish your app using FTP.</span></span>

1. <span data-ttu-id="18585-181">Öppna din FTP-klient föredrar.</span><span class="sxs-lookup"><span data-stu-id="18585-181">Open your FTP client of choice.</span></span>
2. <span data-ttu-id="18585-182">Ange hello *värdnamnsdelen* från hello `publishUrl` attributet ovan i FTP-klient.</span><span class="sxs-lookup"><span data-stu-id="18585-182">Enter hello *host name portion* from hello `publishUrl` attribute you noted above into your FTP client.</span></span>
3. <span data-ttu-id="18585-183">Ange hello `userName` och `userPWD` attribut som du antecknade ovan oförändrad i FTP-klient.</span><span class="sxs-lookup"><span data-stu-id="18585-183">Enter hello `userName` and `userPWD` attributes you noted above unchanged into your FTP client.</span></span>
4. <span data-ttu-id="18585-184">Upprätta en anslutning.</span><span class="sxs-lookup"><span data-stu-id="18585-184">Establish a connection.</span></span>

<span data-ttu-id="18585-185">När du har anslutit kommer du att kunna tooupload och hämta filer efter behov.</span><span class="sxs-lookup"><span data-stu-id="18585-185">After you have connected you will be able tooupload and download files as needed.</span></span> <span data-ttu-id="18585-186">Se till att du överför filer toohello rotkatalog, vilket är `/site/wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="18585-186">Be sure that you are uploading files toohello root directory, which is `/site/wwwroot`.</span></span>

<span data-ttu-id="18585-187">Efter överföring både `index.php` och `createtable.php`, bläddra för**http://[site name].azurewebsites.net/createtable.php** toocreate hello MySQL för hello programmet och sedan bläddra för**http://[site namn ].azurewebsites.net/index.php** toobegin med hello program.</span><span class="sxs-lookup"><span data-stu-id="18585-187">After uploading both `index.php` and `createtable.php`, browse too**http://[site name].azurewebsites.net/createtable.php** toocreate hello MySQL table for hello application, then browse too**http://[site name].azurewebsites.net/index.php** toobegin using hello application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18585-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18585-188">Next steps</span></span>
<span data-ttu-id="18585-189">Mer information finns i hello [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="18585-189">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png

