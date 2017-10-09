---
title: aaaCreate PHP-MySQL webbapp i Azure App Service och distribuera med Git
description: "En genomgång som visar hur toocreate en PHP webbapp som lagrar data i MySQL och använda tooAzure för Git-distribution."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
tags: mysql
ms.assetid: 7454475f-e275-4bc7-9f09-1ef07382e5da
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 9c22946777598cc973cd9dfc8d2a258bd08cc39a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a><span data-ttu-id="61ad2-103">Skapa en PHP-MySQL-webbapp i Azure App Service och distribuera med Git</span><span class="sxs-lookup"><span data-stu-id="61ad2-103">Create a PHP-MySQL web app in Azure App Service and deploy using Git</span></span>
<span data-ttu-id="61ad2-104">Den här kursen visar hur toocreate PHP-MySQL web app och toodeploy den för[Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) med Git.</span><span class="sxs-lookup"><span data-stu-id="61ad2-104">This tutorial shows you how toocreate a PHP-MySQL web app and how toodeploy it too[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) using Git.</span></span> <span data-ttu-id="61ad2-105">Du kommer att använda [PHP][install-php], hello MySQL-kommandoradsverktyget (en del av [MySQL][install-mysql]), och [Git] [ install-git] installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="61ad2-105">You will use [PHP][install-php], hello MySQL Command-Line Tool (part of [MySQL][install-mysql]), and [Git][install-git] installed on your computer.</span></span> <span data-ttu-id="61ad2-106">hello anvisningarna i den här kursen kan tillämpas på alla operativsystem, inklusive Windows, Mac- och Linux.</span><span class="sxs-lookup"><span data-stu-id="61ad2-106">hello instructions in this tutorial can be followed on any operating system, including Windows, Mac, and  Linux.</span></span> <span data-ttu-id="61ad2-107">När du slutför den här guiden kommer du har ett webbprogram för PHP/MySQL som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="61ad2-107">Upon completing this guide, you will have a PHP/MySQL web app running in Azure.</span></span>

<span data-ttu-id="61ad2-108">Du kommer att lära dig:</span><span class="sxs-lookup"><span data-stu-id="61ad2-108">You will learn:</span></span>

* <span data-ttu-id="61ad2-109">Hur toocreate ett webbprogram och en MySQL-databas med hjälp av hello [Azure Portal][management-portal].</span><span class="sxs-lookup"><span data-stu-id="61ad2-109">How toocreate a web app and a MySQL database using hello [Azure Portal][management-portal].</span></span> <span data-ttu-id="61ad2-110">Eftersom PHP är aktiverat i [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) inget särskilt är som standard krävs toorun PHP-kod.</span><span class="sxs-lookup"><span data-stu-id="61ad2-110">Because PHP is enabled in [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) by default, nothing special is required toorun your PHP code.</span></span>
* <span data-ttu-id="61ad2-111">Hur toopublish och publicera dina program tooAzure med Git.</span><span class="sxs-lookup"><span data-stu-id="61ad2-111">How toopublish and re-publish your application tooAzure using Git.</span></span>
* <span data-ttu-id="61ad2-112">Hur tooenable hello Composer tillägget tooautomate Composer uppgifter vid varje `git push`.</span><span class="sxs-lookup"><span data-stu-id="61ad2-112">How tooenable hello Composer extension tooautomate Composer tasks at every `git push`.</span></span>

<span data-ttu-id="61ad2-113">Genom att följa den här självstudiekursen skapar du en enkel registrering webbapp i PHP.</span><span class="sxs-lookup"><span data-stu-id="61ad2-113">By following this tutorial, you will build a simple registration web app in PHP.</span></span> <span data-ttu-id="61ad2-114">hello program ska finnas i Web Apps.</span><span class="sxs-lookup"><span data-stu-id="61ad2-114">hello application will be hosted in Web Apps.</span></span> <span data-ttu-id="61ad2-115">En skärmbild av programmet hello slutförts understiger:</span><span class="sxs-lookup"><span data-stu-id="61ad2-115">A screenshot of hello completed application is below:</span></span>

![Azure PHP-webbplats][running-app]

## <a name="set-up-hello-development-environment"></a><span data-ttu-id="61ad2-117">Ställa in hello utvecklingsmiljö</span><span class="sxs-lookup"><span data-stu-id="61ad2-117">Set up hello development environment</span></span>
<span data-ttu-id="61ad2-118">Den här kursen förutsätter att du har [PHP][install-php], hello MySQL-kommandoradsverktyget (en del av [MySQL][install-mysql]), och [Git] [ install-git] installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="61ad2-118">This tutorial assumes you have [PHP][install-php], hello MySQL Command-Line Tool (part of [MySQL][install-mysql]), and [Git][install-git] installed on your computer.</span></span>

<a id="create-web-site-and-set-up-git"></a>

## <a name="create-a-web-app-and-set-up-git-publishing"></a><span data-ttu-id="61ad2-119">Skapa en webbapp och konfigurera Git-publicering</span><span class="sxs-lookup"><span data-stu-id="61ad2-119">Create a web app and set up Git publishing</span></span>
<span data-ttu-id="61ad2-120">Följ dessa steg toocreate ett webbprogram och en MySQL-databas:</span><span class="sxs-lookup"><span data-stu-id="61ad2-120">Follow these steps toocreate a web app and a MySQL database:</span></span>

1. <span data-ttu-id="61ad2-121">Inloggningen toohello [Azure Portal][management-portal].</span><span class="sxs-lookup"><span data-stu-id="61ad2-121">Login toohello [Azure Portal][management-portal].</span></span>
2. <span data-ttu-id="61ad2-122">Klicka på hello **ny** ikon.</span><span class="sxs-lookup"><span data-stu-id="61ad2-122">Click hello **New** icon.</span></span>
3. <span data-ttu-id="61ad2-123">Klicka på **finns alla** nästa för**Marketplace**.</span><span class="sxs-lookup"><span data-stu-id="61ad2-123">Click **See All** next too**Marketplace**.</span></span> 
4. <span data-ttu-id="61ad2-124">Klicka på **webb + mobilt**, sedan **Webbapp + MySQL**.</span><span class="sxs-lookup"><span data-stu-id="61ad2-124">Click **Web + Mobile**, then **Web app + MySQL**.</span></span> <span data-ttu-id="61ad2-125">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="61ad2-125">Then, click **Create**.</span></span>
5. <span data-ttu-id="61ad2-126">Ange ett giltigt namn för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="61ad2-126">Enter a valid name for your resource group.</span></span>
   
    ![Ange resursgruppens namn][resource-group]
6. <span data-ttu-id="61ad2-128">Ange värden för din nya webbapp.</span><span class="sxs-lookup"><span data-stu-id="61ad2-128">Enter values for your new web app.</span></span>
   
    ![Skapa webbapp][new-web-app]
7. <span data-ttu-id="61ad2-130">Ange värden för den nya databasen, inklusive godkänner toohello juridiska villkor.</span><span class="sxs-lookup"><span data-stu-id="61ad2-130">Enter values for your new database, including agreeing toohello legal terms.</span></span>
   
    ![Skapa ny MySQL-databas][new-mysql-db]
8. <span data-ttu-id="61ad2-132">När hello webbprogrammet har skapats visas hello nytt web app blad.</span><span class="sxs-lookup"><span data-stu-id="61ad2-132">When hello web app has been created, you will see hello new web app blade.</span></span>
9. <span data-ttu-id="61ad2-133">I **inställningar** klickar du på **kontinuerlig distribution**, klickar du på *konfigurera nödvändiga inställningar*.</span><span class="sxs-lookup"><span data-stu-id="61ad2-133">In **Settings** click on **Continuous Deployment**, then click on *Configure required settings*.</span></span>
   
    ![Konfigurera Git-publicering][setup-publishing]
10. <span data-ttu-id="61ad2-135">Välj **lokal Git-lagringsplats** för hello källa.</span><span class="sxs-lookup"><span data-stu-id="61ad2-135">Select **Local Git Repository** for hello source.</span></span>
    
     ![Konfigurera Git-lagringsplats][setup-repository]
11. <span data-ttu-id="61ad2-137">tooenable Git publicering, måste du ange ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="61ad2-137">tooenable Git publishing, you must provide a user name and password.</span></span> <span data-ttu-id="61ad2-138">Anteckna hello användarnamn och lösenord som du skapar.</span><span class="sxs-lookup"><span data-stu-id="61ad2-138">Make a note of hello user name and password you create.</span></span> <span data-ttu-id="61ad2-139">(Om du har ställt in en Git-lagringsplats innan det här steget hoppas över.)</span><span class="sxs-lookup"><span data-stu-id="61ad2-139">(If you have set up a Git repository before, this step will be skipped.)</span></span>
    
     ![Skapa autentiseringsuppgifter för publicering][credentials]

## <a name="get-remote-mysql-connection-information"></a><span data-ttu-id="61ad2-141">Hämta anslutningsinformation för MySQL</span><span class="sxs-lookup"><span data-stu-id="61ad2-141">Get remote MySQL connection information</span></span>
<span data-ttu-id="61ad2-142">tooconnect toohello MySQL-databas som körs i Web Apps din kommer måste hello anslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="61ad2-142">tooconnect toohello MySQL database that is running in Web Apps, your will need hello connection information.</span></span> <span data-ttu-id="61ad2-143">tooget MySQL anslutningsinformationen så här:</span><span class="sxs-lookup"><span data-stu-id="61ad2-143">tooget MySQL connection information, follow these steps:</span></span>

1. <span data-ttu-id="61ad2-144">Klicka på hello databas från din resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="61ad2-144">From your resource group, click hello database:</span></span>
   
    ![Välj databas][select-database]
2. <span data-ttu-id="61ad2-146">Från hello databasen **inställningar**väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="61ad2-146">From hello database **Settings**, select **Properties**.</span></span>
   
    ![Välj egenskaper][select-properties]
3. <span data-ttu-id="61ad2-148">Anteckna hello värden för `Database`, `Host`, `User Id`, och `Password`.</span><span class="sxs-lookup"><span data-stu-id="61ad2-148">Make note of hello values for `Database`, `Host`, `User Id`, and `Password`.</span></span>
   
    ![Obs egenskaper][note-properties]

## <a name="build-and-test-your-app-locally"></a><span data-ttu-id="61ad2-150">Skapa och testa appen lokalt</span><span class="sxs-lookup"><span data-stu-id="61ad2-150">Build and test your app locally</span></span>
<span data-ttu-id="61ad2-151">Nu när du har skapat ett webbprogram, kan du utveckla ditt program lokalt och sedan distribuera efter testning.</span><span class="sxs-lookup"><span data-stu-id="61ad2-151">Now that you have created a web app, you can develop your application locally, then deploy it after testing.</span></span>

<span data-ttu-id="61ad2-152">hello registreringen programmet är ett enkelt PHP-program som du kan använda tooregister för en händelse genom att tillhandahålla ditt namn och e-postadress.</span><span class="sxs-lookup"><span data-stu-id="61ad2-152">hello Registration application is a simple PHP application that allows you tooregister for an event by providing your name and email address.</span></span> <span data-ttu-id="61ad2-153">Information om föregående månaderna visas i en tabell.</span><span class="sxs-lookup"><span data-stu-id="61ad2-153">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="61ad2-154">Registreringsinformation lagras i en MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="61ad2-154">Registration information is stored in a MySQL database.</span></span> <span data-ttu-id="61ad2-155">hello program består av en fil (kopiera och klistra in koden finns nedan):</span><span class="sxs-lookup"><span data-stu-id="61ad2-155">hello application consists of one file (copy/paste code available below):</span></span>

* <span data-ttu-id="61ad2-156">**index.php**: Visar ett formulär för registrering och en tabell som innehåller registrant information.</span><span class="sxs-lookup"><span data-stu-id="61ad2-156">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>

<span data-ttu-id="61ad2-157">toobuild och kör hello programmet lokalt, följer du hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="61ad2-157">toobuild and run hello application locally, follow hello steps below.</span></span> <span data-ttu-id="61ad2-158">Observera att de här stegen förutsätter att du har hello PHP och MySQL kommandoradsverktyget (del av MySQL) på den lokala datorn och att du har aktiverat hello [PDO-tillägget för MySQL][pdo-mysql].</span><span class="sxs-lookup"><span data-stu-id="61ad2-158">Note that these steps assume you have hello PHP and MySQL Command-Line Tool (part of MySQL) set up on your local machine, and that you have enabled hello [PDO extension for MySQL][pdo-mysql].</span></span>

1. <span data-ttu-id="61ad2-159">Ansluta toohello MySQL fjärrserver, genom att använda hello värde för `Data Source`, `User Id`, `Password`, och `Database` som du hämtade tidigare:</span><span class="sxs-lookup"><span data-stu-id="61ad2-159">Connect toohello remote MySQL server, using hello value for `Data Source`, `User Id`, `Password`, and `Database` that you retrieved earlier:</span></span>
   
        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]
2. <span data-ttu-id="61ad2-160">hello MySQL kommandotolken visas:</span><span class="sxs-lookup"><span data-stu-id="61ad2-160">hello MySQL command prompt will appear:</span></span>
   
        mysql>
3. <span data-ttu-id="61ad2-161">Klistra in följande hello `CREATE TABLE` kommandot toocreate hello `registration_tbl` tabell i databasen:</span><span class="sxs-lookup"><span data-stu-id="61ad2-161">Paste in hello following `CREATE TABLE` command toocreate hello `registration_tbl` table in your database:</span></span>
   
        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);
4. <span data-ttu-id="61ad2-162">Skapa i hello roten för lokala programmappen **index.php** fil.</span><span class="sxs-lookup"><span data-stu-id="61ad2-162">In hello root of your local application folder create **index.php** file.</span></span>
5. <span data-ttu-id="61ad2-163">Öppna hello **index.php** filen i en textredigerare eller IDE och Lägg till hello följande kod och fullständig hello nödvändiga ändringar som har markerats med `//TODO:` kommentarer.</span><span class="sxs-lookup"><span data-stu-id="61ad2-163">Open hello **index.php** file in a text editor or IDE and add hello following code, and complete hello necessary changes marked with `//TODO:` comments.</span></span>

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
            // DB connection info
            //TODO: Update hello values for $host, $user, $pwd, and $db
            //using hello values you retrieved earlier from hello Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect toodatabase.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
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
            // Retrieve data
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
        ?>
        </body>
        </html>

1. <span data-ttu-id="61ad2-164">I en terminal gå tooyour program-mappen och Skriv hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="61ad2-164">In a terminal go tooyour application folder and type hello following command:</span></span>
   
       php -S localhost:8000

<span data-ttu-id="61ad2-165">Du kan nu Bläddra för**http://localhost: 8000 /** tootest hello program.</span><span class="sxs-lookup"><span data-stu-id="61ad2-165">You can now browse too**http://localhost:8000/** tootest hello application.</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="61ad2-166">Publicera appen</span><span class="sxs-lookup"><span data-stu-id="61ad2-166">Publish your app</span></span>
<span data-ttu-id="61ad2-167">När du har testat appen lokalt kan publicera du den tooWeb appar med Git.</span><span class="sxs-lookup"><span data-stu-id="61ad2-167">After you have tested your app locally, you can publish it tooWeb Apps using Git.</span></span> <span data-ttu-id="61ad2-168">Du initiera lokal Git-lagringsplats och publicera programmet hello.</span><span class="sxs-lookup"><span data-stu-id="61ad2-168">You will initialize your local Git repository and publish hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="61ad2-169">Dessa är hello samma steg som visas i hello Azure Portal hello slutet av hello skapa en webbapp och ange in Git Publishing ovan.</span><span class="sxs-lookup"><span data-stu-id="61ad2-169">These are hello same steps shown in hello Azure Portal at hello end of hello Create a web app and Set up Git Publishing section above.</span></span>
> 
> 

1. <span data-ttu-id="61ad2-170">(Valfritt)  Om du har glömt eller tappas bort Git remote repostitory URL: en, navigera toohello web appegenskaper för hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="61ad2-170">(Optional)  If you've forgotten or misplaced your Git remote repostitory URL, navigate toohello web app properties on hello Azure Portal.</span></span>
2. <span data-ttu-id="61ad2-171">Öppna GitBash (eller en terminal om Git i din `PATH`), ändra kataloger toohello rotkatalogen för programmet och kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="61ad2-171">Open GitBash (or a terminal, if Git is in your `PATH`), change directories toohello root directory of your application, and run hello following commands:</span></span>
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    <span data-ttu-id="61ad2-172">Du uppmanas att hello-lösenord som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="61ad2-172">You will be prompted for hello password you created earlier.</span></span>
   
    ![Den inledande Push tooAzure via Git][git-initial-push]
3. <span data-ttu-id="61ad2-174">Bläddra för**http://[site name].azurewebsites.net/index.php** toobegin med hjälp av hello (den här informationen lagras på instrumentpanelen konto):</span><span class="sxs-lookup"><span data-stu-id="61ad2-174">Browse too**http://[site name].azurewebsites.net/index.php** toobegin using hello application (this information will be stored on your account dashboard):</span></span>
   
    ![Azure PHP-webbplats][running-app]

<span data-ttu-id="61ad2-176">När du har publicerat din app, kan du göra ändringar tooit och använda Git toopublish dem.</span><span class="sxs-lookup"><span data-stu-id="61ad2-176">After you have published your app, you can begin making changes tooit and use Git toopublish them.</span></span>

## <a name="publish-changes-tooyour-app"></a><span data-ttu-id="61ad2-177">Publicera ändringar tooyour app</span><span class="sxs-lookup"><span data-stu-id="61ad2-177">Publish changes tooyour app</span></span>
<span data-ttu-id="61ad2-178">toopublish ändringar tooyour appen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="61ad2-178">toopublish changes tooyour app, follow these steps:</span></span>

1. <span data-ttu-id="61ad2-179">Göra ändringar tooyour appen lokalt.</span><span class="sxs-lookup"><span data-stu-id="61ad2-179">Make changes tooyour app locally.</span></span>
2. <span data-ttu-id="61ad2-180">Öppna GitBash (eller en terminal it Git finns i din `PATH`), ändra kataloger toohello rotkatalogen för din app och kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="61ad2-180">Open GitBash (or a terminal, it Git is in your `PATH`), change directories toohello root directory of your app, and run hello following commands:</span></span>
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    <span data-ttu-id="61ad2-181">Du uppmanas att hello-lösenord som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="61ad2-181">You will be prompted for hello password you created earlier.</span></span>
   
    ![Skicka ändringar av platsen tooAzure via Git][git-change-push]
3. <span data-ttu-id="61ad2-183">Bläddra för**http://[site name].azurewebsites.net/index.php** toosee din app och eventuella ändringar som du har gjort:</span><span class="sxs-lookup"><span data-stu-id="61ad2-183">Browse too**http://[site name].azurewebsites.net/index.php** toosee your app and any changes you may have made:</span></span>
   
    ![Azure PHP-webbplats][running-app]

> [!NOTE]
> <span data-ttu-id="61ad2-185">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="61ad2-185">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="61ad2-186">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="61ad2-186">No credit cards required; no commitments.</span></span>
> 
> 

<a name="composer"></a>

## <a name="enable-composer-automation-with-hello-composer-extension"></a><span data-ttu-id="61ad2-187">Aktivera Composer automatisering med hello tillägget Composer</span><span class="sxs-lookup"><span data-stu-id="61ad2-187">Enable Composer automation with hello Composer extension</span></span>
<span data-ttu-id="61ad2-188">Som standard göra hello git-Distributionsprocess i App Service inte något med composer.json, om du har en i PHP-projektet.</span><span class="sxs-lookup"><span data-stu-id="61ad2-188">By default, hello git deployment process in App Service doesn't do anything with composer.json, if you have one in your PHP project.</span></span> <span data-ttu-id="61ad2-189">Du kan aktivera composer.json bearbetning under `git push` genom att aktivera hello Composer tillägg.</span><span class="sxs-lookup"><span data-stu-id="61ad2-189">You can enable composer.json processing during `git push` by enabling hello Composer extension.</span></span>

1. <span data-ttu-id="61ad2-190">I din PHP web Apps bladet i hello [Azure-portalen][management-portal], klickar du på **verktyg** > **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="61ad2-190">In your PHP web app's blade in hello [Azure portal][management-portal], click **Tools** > **Extensions**.</span></span>
   
    ![Composer tilläggsinställningar][composer-extension-settings]
2. <span data-ttu-id="61ad2-192">Klicka på **Lägg till**, klicka på **Composer**.</span><span class="sxs-lookup"><span data-stu-id="61ad2-192">Click **Add**, then click **Composer**.</span></span>
   
    ![Lägg till tillägget Composer][composer-extension-add]
3. <span data-ttu-id="61ad2-194">Klicka på **OK** tooaccept juridiska villkor.</span><span class="sxs-lookup"><span data-stu-id="61ad2-194">Click **OK** tooaccept legal terms.</span></span> <span data-ttu-id="61ad2-195">Klicka på **OK** igen tooadd hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="61ad2-195">Click **OK** again tooadd hello extension.</span></span>
   
    <span data-ttu-id="61ad2-196">Hej **installerat tillägg** bladet visas nu hello Composer tillägg.</span><span class="sxs-lookup"><span data-stu-id="61ad2-196">hello **Installed extensions** blade will now show hello Composer extension.</span></span>  
    <span data-ttu-id="61ad2-197">![Composer tillägg View][composer-extension-view]</span><span class="sxs-lookup"><span data-stu-id="61ad2-197">![Composer Extension View][composer-extension-view]</span></span>
4. <span data-ttu-id="61ad2-198">Nu kan utföra `git add`, `git commit`, och `git push` precis som i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="61ad2-198">Now, perform `git add`, `git commit`, and `git push` like in hello previous section.</span></span> <span data-ttu-id="61ad2-199">Du ser nu att installeras Composer beroenden som definierats i composer.json.</span><span class="sxs-lookup"><span data-stu-id="61ad2-199">You'll now see that Composer is installing dependencies defined in composer.json.</span></span>
   
    ![Composer tillägget lyckades][composer-extension-success]

## <a name="next-steps"></a><span data-ttu-id="61ad2-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="61ad2-201">Next steps</span></span>
<span data-ttu-id="61ad2-202">Mer information finns i hello [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="61ad2-202">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
