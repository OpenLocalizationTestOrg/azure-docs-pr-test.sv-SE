---
title: Skapa en PHP-MySQL-webbapp i Azure App Service och distribuera med Git
description: "En genomgång som visar hur du skapar en PHP-webbapp som lagrar data i MySQL och använda Git-distribution till Azure."
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
ms.openlocfilehash: aa845eb474dbd42ae2c31880690d4ced059eb448
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a><span data-ttu-id="a263a-103">Skapa en PHP-MySQL-webbapp i Azure App Service och distribuera med Git</span><span class="sxs-lookup"><span data-stu-id="a263a-103">Create a PHP-MySQL web app in Azure App Service and deploy using Git</span></span>
<span data-ttu-id="a263a-104">Den här kursen visar hur du skapar ett webbprogram för PHP-MySQL och hur du distribuerar det till [Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) med Git.</span><span class="sxs-lookup"><span data-stu-id="a263a-104">This tutorial shows you how to create a PHP-MySQL web app and how to deploy it to [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) using Git.</span></span> <span data-ttu-id="a263a-105">Du kommer att använda [PHP][install-php], MySQL-kommandoradsverktyget (en del av [MySQL][install-mysql]), och [Git] [ install-git] installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="a263a-105">You will use [PHP][install-php], the MySQL Command-Line Tool (part of [MySQL][install-mysql]), and [Git][install-git] installed on your computer.</span></span> <span data-ttu-id="a263a-106">Anvisningarna i den här kursen kan tillämpas på alla operativsystem, inklusive Windows, Mac- och Linux.</span><span class="sxs-lookup"><span data-stu-id="a263a-106">The instructions in this tutorial can be followed on any operating system, including Windows, Mac, and  Linux.</span></span> <span data-ttu-id="a263a-107">När du slutför den här guiden kommer du har ett webbprogram för PHP/MySQL som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="a263a-107">Upon completing this guide, you will have a PHP/MySQL web app running in Azure.</span></span>

<span data-ttu-id="a263a-108">Du kommer att lära dig:</span><span class="sxs-lookup"><span data-stu-id="a263a-108">You will learn:</span></span>

* <span data-ttu-id="a263a-109">Hur du skapar ett webbprogram och en MySQL-databas med den [Azure Portal][management-portal].</span><span class="sxs-lookup"><span data-stu-id="a263a-109">How to create a web app and a MySQL database using the [Azure Portal][management-portal].</span></span> <span data-ttu-id="a263a-110">Eftersom PHP är aktiverat i [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) som standard inget särskilt krävs för att köra din PHP-kod.</span><span class="sxs-lookup"><span data-stu-id="a263a-110">Because PHP is enabled in [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) by default, nothing special is required to run your PHP code.</span></span>
* <span data-ttu-id="a263a-111">Hur man publicerar och publicera programmet till Azure med Git.</span><span class="sxs-lookup"><span data-stu-id="a263a-111">How to publish and re-publish your application to Azure using Git.</span></span>
* <span data-ttu-id="a263a-112">Så här aktiverar du tillägget Composer att automatisera Composer uppgifter vid varje `git push`.</span><span class="sxs-lookup"><span data-stu-id="a263a-112">How to enable the Composer extension to automate Composer tasks at every `git push`.</span></span>

<span data-ttu-id="a263a-113">Genom att följa den här självstudiekursen skapar du en enkel registrering webbapp i PHP.</span><span class="sxs-lookup"><span data-stu-id="a263a-113">By following this tutorial, you will build a simple registration web app in PHP.</span></span> <span data-ttu-id="a263a-114">Programmet ska finnas i Web Apps.</span><span class="sxs-lookup"><span data-stu-id="a263a-114">The application will be hosted in Web Apps.</span></span> <span data-ttu-id="a263a-115">En skärmbild av det färdiga programmet understiger:</span><span class="sxs-lookup"><span data-stu-id="a263a-115">A screenshot of the completed application is below:</span></span>

![Azure PHP-webbplats][running-app]

## <a name="set-up-the-development-environment"></a><span data-ttu-id="a263a-117">Konfigurera utvecklingsmiljön</span><span class="sxs-lookup"><span data-stu-id="a263a-117">Set up the development environment</span></span>
<span data-ttu-id="a263a-118">Den här kursen förutsätter att du har [PHP][install-php], MySQL-kommandoradsverktyget (en del av [MySQL][install-mysql]), och [Git] [ install-git] installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="a263a-118">This tutorial assumes you have [PHP][install-php], the MySQL Command-Line Tool (part of [MySQL][install-mysql]), and [Git][install-git] installed on your computer.</span></span>

<a id="create-web-site-and-set-up-git"></a>

## <a name="create-a-web-app-and-set-up-git-publishing"></a><span data-ttu-id="a263a-119">Skapa en webbapp och konfigurera Git-publicering</span><span class="sxs-lookup"><span data-stu-id="a263a-119">Create a web app and set up Git publishing</span></span>
<span data-ttu-id="a263a-120">Följ dessa steg för att skapa en webbapp och en MySQL-databas:</span><span class="sxs-lookup"><span data-stu-id="a263a-120">Follow these steps to create a web app and a MySQL database:</span></span>

1. <span data-ttu-id="a263a-121">Logga in på den [Azure-portalen][management-portal].</span><span class="sxs-lookup"><span data-stu-id="a263a-121">Login to the [Azure Portal][management-portal].</span></span>
2. <span data-ttu-id="a263a-122">Klicka på den **ny** ikon.</span><span class="sxs-lookup"><span data-stu-id="a263a-122">Click the **New** icon.</span></span>
3. <span data-ttu-id="a263a-123">Klicka på **finns alla** bredvid **Marketplace**.</span><span class="sxs-lookup"><span data-stu-id="a263a-123">Click **See All** next to **Marketplace**.</span></span> 
4. <span data-ttu-id="a263a-124">Klicka på **webb + mobilt**, sedan **Webbapp + MySQL**.</span><span class="sxs-lookup"><span data-stu-id="a263a-124">Click **Web + Mobile**, then **Web app + MySQL**.</span></span> <span data-ttu-id="a263a-125">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a263a-125">Then, click **Create**.</span></span>
5. <span data-ttu-id="a263a-126">Ange ett giltigt namn för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="a263a-126">Enter a valid name for your resource group.</span></span>
   
    ![Ange resursgruppens namn][resource-group]
6. <span data-ttu-id="a263a-128">Ange värden för din nya webbapp.</span><span class="sxs-lookup"><span data-stu-id="a263a-128">Enter values for your new web app.</span></span>
   
    ![Skapa webbapp][new-web-app]
7. <span data-ttu-id="a263a-130">Ange värden för den nya databasen, inklusive godkänner du de juridiska villkoren.</span><span class="sxs-lookup"><span data-stu-id="a263a-130">Enter values for your new database, including agreeing to the legal terms.</span></span>
   
    ![Skapa ny MySQL-databas][new-mysql-db]
8. <span data-ttu-id="a263a-132">När webbappen har skapats visas bladet ny webbapp.</span><span class="sxs-lookup"><span data-stu-id="a263a-132">When the web app has been created, you will see the new web app blade.</span></span>
9. <span data-ttu-id="a263a-133">I **inställningar** klickar du på **kontinuerlig distribution**, klickar du på *konfigurera nödvändiga inställningar*.</span><span class="sxs-lookup"><span data-stu-id="a263a-133">In **Settings** click on **Continuous Deployment**, then click on *Configure required settings*.</span></span>
   
    ![Konfigurera Git-publicering][setup-publishing]
10. <span data-ttu-id="a263a-135">Välj **lokal Git-lagringsplats** för datakällan.</span><span class="sxs-lookup"><span data-stu-id="a263a-135">Select **Local Git Repository** for the source.</span></span>
    
     ![Konfigurera Git-lagringsplats][setup-repository]
11. <span data-ttu-id="a263a-137">Du måste ange ett användarnamn och lösenord om du vill aktivera Git-publicering.</span><span class="sxs-lookup"><span data-stu-id="a263a-137">To enable Git publishing, you must provide a user name and password.</span></span> <span data-ttu-id="a263a-138">Anteckna användarnamnet och lösenordet som du skapar.</span><span class="sxs-lookup"><span data-stu-id="a263a-138">Make a note of the user name and password you create.</span></span> <span data-ttu-id="a263a-139">(Om du har ställt in en Git-lagringsplats innan det här steget hoppas över.)</span><span class="sxs-lookup"><span data-stu-id="a263a-139">(If you have set up a Git repository before, this step will be skipped.)</span></span>
    
     ![Skapa autentiseringsuppgifter för publicering][credentials]

## <a name="get-remote-mysql-connection-information"></a><span data-ttu-id="a263a-141">Hämta anslutningsinformation för MySQL</span><span class="sxs-lookup"><span data-stu-id="a263a-141">Get remote MySQL connection information</span></span>
<span data-ttu-id="a263a-142">Att ansluta till MySQL-databas som körs i Web Apps din kommer måste anslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="a263a-142">To connect to the MySQL database that is running in Web Apps, your will need the connection information.</span></span> <span data-ttu-id="a263a-143">Följ dessa steg för att få information om MySQL anslutning:</span><span class="sxs-lookup"><span data-stu-id="a263a-143">To get MySQL connection information, follow these steps:</span></span>

1. <span data-ttu-id="a263a-144">Klicka på databasen från din resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="a263a-144">From your resource group, click the database:</span></span>
   
    ![Välj databas][select-database]
2. <span data-ttu-id="a263a-146">Från databasen **inställningar**väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="a263a-146">From the database **Settings**, select **Properties**.</span></span>
   
    ![Välj egenskaper][select-properties]
3. <span data-ttu-id="a263a-148">Anteckna värdena för `Database`, `Host`, `User Id`, och `Password`.</span><span class="sxs-lookup"><span data-stu-id="a263a-148">Make note of the values for `Database`, `Host`, `User Id`, and `Password`.</span></span>
   
    ![Obs egenskaper][note-properties]

## <a name="build-and-test-your-app-locally"></a><span data-ttu-id="a263a-150">Skapa och testa appen lokalt</span><span class="sxs-lookup"><span data-stu-id="a263a-150">Build and test your app locally</span></span>
<span data-ttu-id="a263a-151">Nu när du har skapat ett webbprogram, kan du utveckla ditt program lokalt och sedan distribuera efter testning.</span><span class="sxs-lookup"><span data-stu-id="a263a-151">Now that you have created a web app, you can develop your application locally, then deploy it after testing.</span></span>

<span data-ttu-id="a263a-152">Registreringen programmet är ett enkelt PHP-program som gör att du kan registrera dig för en händelse genom att tillhandahålla ditt namn och e-postadress.</span><span class="sxs-lookup"><span data-stu-id="a263a-152">The Registration application is a simple PHP application that allows you to register for an event by providing your name and email address.</span></span> <span data-ttu-id="a263a-153">Information om föregående månaderna visas i en tabell.</span><span class="sxs-lookup"><span data-stu-id="a263a-153">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="a263a-154">Registreringsinformation lagras i en MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a263a-154">Registration information is stored in a MySQL database.</span></span> <span data-ttu-id="a263a-155">Programmet består av en fil (kopiera och klistra in koden finns nedan):</span><span class="sxs-lookup"><span data-stu-id="a263a-155">The application consists of one file (copy/paste code available below):</span></span>

* <span data-ttu-id="a263a-156">**index.php**: Visar ett formulär för registrering och en tabell som innehåller registrant information.</span><span class="sxs-lookup"><span data-stu-id="a263a-156">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>

<span data-ttu-id="a263a-157">Följ stegen nedan om du vill skapa och köra programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="a263a-157">To build and run the application locally, follow the steps below.</span></span> <span data-ttu-id="a263a-158">Observera att de här stegen förutsätter att du har PHP och MySQL kommandoradsverktyget (del av MySQL) på den lokala datorn och att du har aktiverat den [PDO-tillägget för MySQL][pdo-mysql].</span><span class="sxs-lookup"><span data-stu-id="a263a-158">Note that these steps assume you have the PHP and MySQL Command-Line Tool (part of MySQL) set up on your local machine, and that you have enabled the [PDO extension for MySQL][pdo-mysql].</span></span>

1. <span data-ttu-id="a263a-159">Ansluta till fjärrservern i MySQL med hjälp av värdet för `Data Source`, `User Id`, `Password`, och `Database` som du hämtade tidigare:</span><span class="sxs-lookup"><span data-stu-id="a263a-159">Connect to the remote MySQL server, using the value for `Data Source`, `User Id`, `Password`, and `Database` that you retrieved earlier:</span></span>
   
        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]
2. <span data-ttu-id="a263a-160">MySQL-kommandotolken visas:</span><span class="sxs-lookup"><span data-stu-id="a263a-160">The MySQL command prompt will appear:</span></span>
   
        mysql>
3. <span data-ttu-id="a263a-161">Klistra in följande `CREATE TABLE` kommando för att skapa den `registration_tbl` tabell i databasen:</span><span class="sxs-lookup"><span data-stu-id="a263a-161">Paste in the following `CREATE TABLE` command to create the `registration_tbl` table in your database:</span></span>
   
        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);
4. <span data-ttu-id="a263a-162">Skapa i roten av den lokala programmappen **index.php** fil.</span><span class="sxs-lookup"><span data-stu-id="a263a-162">In the root of your local application folder create **index.php** file.</span></span>
5. <span data-ttu-id="a263a-163">Öppna den **index.php** filen i en textredigerare eller IDE och Lägg till följande kod och slutföra de nödvändiga ändringarna som markerats med `//TODO:` kommentarer.</span><span class="sxs-lookup"><span data-stu-id="a263a-163">Open the **index.php** file in a text editor or IDE and add the following code, and complete the necessary changes marked with `//TODO:` comments.</span></span>

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
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
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

1. <span data-ttu-id="a263a-164">Gå till programmappen i en terminal och Skriv följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a263a-164">In a terminal go to your application folder and type the following command:</span></span>
   
       php -S localhost:8000

<span data-ttu-id="a263a-165">Du kan nu bläddra till **http://localhost: 8000 /** för att testa programmet.</span><span class="sxs-lookup"><span data-stu-id="a263a-165">You can now browse to **http://localhost:8000/** to test the application.</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="a263a-166">Publicera din app</span><span class="sxs-lookup"><span data-stu-id="a263a-166">Publish your app</span></span>
<span data-ttu-id="a263a-167">När du har testat appen lokalt kan publicera du den till Webbappar med Git.</span><span class="sxs-lookup"><span data-stu-id="a263a-167">After you have tested your app locally, you can publish it to Web Apps using Git.</span></span> <span data-ttu-id="a263a-168">Du initiera lokal Git-lagringsplats och publicera programmet.</span><span class="sxs-lookup"><span data-stu-id="a263a-168">You will initialize your local Git repository and publish the application.</span></span>

> [!NOTE]
> <span data-ttu-id="a263a-169">Det här är samma steg som visas i Azure-portalen i slutet av den skapa en webbapp och uppsättningen in Git Publishing ovan.</span><span class="sxs-lookup"><span data-stu-id="a263a-169">These are the same steps shown in the Azure Portal at the end of the Create a web app and Set up Git Publishing section above.</span></span>
> 
> 

1. <span data-ttu-id="a263a-170">(Valfritt)  Om du har glömt eller tappas bort Git remote repostitory URL: en, navigera till egenskaperna web app på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="a263a-170">(Optional)  If you've forgotten or misplaced your Git remote repostitory URL, navigate to the web app properties on the Azure Portal.</span></span>
2. <span data-ttu-id="a263a-171">Öppna GitBash (eller en terminal om Git i din `PATH`), ändrar sökvägen till rotkatalogen för programmet och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="a263a-171">Open GitBash (or a terminal, if Git is in your `PATH`), change directories to the root directory of your application, and run the following commands:</span></span>
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    <span data-ttu-id="a263a-172">Du uppmanas att lösenordet du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a263a-172">You will be prompted for the password you created earlier.</span></span>
   
    ![Inledande Push till Azure via Git][git-initial-push]
3. <span data-ttu-id="a263a-174">Bläddra till **http://[site name].azurewebsites.net/index.php** kan börja använda programmet (den här informationen lagras på instrumentpanelen konto):</span><span class="sxs-lookup"><span data-stu-id="a263a-174">Browse to **http://[site name].azurewebsites.net/index.php** to begin using the application (this information will be stored on your account dashboard):</span></span>
   
    ![Azure PHP-webbplats][running-app]

<span data-ttu-id="a263a-176">När du har publicerat din app kan du använda Git för att publicera dem börjar att göra ändringar.</span><span class="sxs-lookup"><span data-stu-id="a263a-176">After you have published your app, you can begin making changes to it and use Git to publish them.</span></span>

## <a name="publish-changes-to-your-app"></a><span data-ttu-id="a263a-177">Publicera ändringar i din app</span><span class="sxs-lookup"><span data-stu-id="a263a-177">Publish changes to your app</span></span>
<span data-ttu-id="a263a-178">Följ dessa steg om du vill publicera ändringar i appen:</span><span class="sxs-lookup"><span data-stu-id="a263a-178">To publish changes to your app, follow these steps:</span></span>

1. <span data-ttu-id="a263a-179">Göra ändringar i din app lokalt.</span><span class="sxs-lookup"><span data-stu-id="a263a-179">Make changes to your app locally.</span></span>
2. <span data-ttu-id="a263a-180">Öppna GitBash (eller en terminal it Git finns i din `PATH`), ändrar sökvägen till rotkatalogen för din app och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="a263a-180">Open GitBash (or a terminal, it Git is in your `PATH`), change directories to the root directory of your app, and run the following commands:</span></span>
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    <span data-ttu-id="a263a-181">Du uppmanas att lösenordet du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a263a-181">You will be prompted for the password you created earlier.</span></span>
   
    ![Skicka ändringar till Azure via Git][git-change-push]
3. <span data-ttu-id="a263a-183">Bläddra till **http://[site name].azurewebsites.net/index.php** att se din app och eventuella ändringar som du har gjort:</span><span class="sxs-lookup"><span data-stu-id="a263a-183">Browse to **http://[site name].azurewebsites.net/index.php** to see your app and any changes you may have made:</span></span>
   
    ![Azure PHP-webbplats][running-app]

> [!NOTE]
> <span data-ttu-id="a263a-185">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="a263a-185">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="a263a-186">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="a263a-186">No credit cards required; no commitments.</span></span>
> 
> 

<a name="composer"></a>

## <a name="enable-composer-automation-with-the-composer-extension"></a><span data-ttu-id="a263a-187">Aktivera Composer automatisering med tillägget Composer</span><span class="sxs-lookup"><span data-stu-id="a263a-187">Enable Composer automation with the Composer extension</span></span>
<span data-ttu-id="a263a-188">Som standard göra git-Distributionsprocess i App Service inte något med composer.json, om du har en i PHP-projektet.</span><span class="sxs-lookup"><span data-stu-id="a263a-188">By default, the git deployment process in App Service doesn't do anything with composer.json, if you have one in your PHP project.</span></span> <span data-ttu-id="a263a-189">Du kan aktivera composer.json bearbetning under `git push` genom att aktivera tillägget Composer.</span><span class="sxs-lookup"><span data-stu-id="a263a-189">You can enable composer.json processing during `git push` by enabling the Composer extension.</span></span>

1. <span data-ttu-id="a263a-190">I din PHP web appens blad i den [Azure-portalen][management-portal], klickar du på **verktyg** > **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="a263a-190">In your PHP web app's blade in the [Azure portal][management-portal], click **Tools** > **Extensions**.</span></span>
   
    ![Composer tilläggsinställningar][composer-extension-settings]
2. <span data-ttu-id="a263a-192">Klicka på **Lägg till**, klicka på **Composer**.</span><span class="sxs-lookup"><span data-stu-id="a263a-192">Click **Add**, then click **Composer**.</span></span>
   
    ![Lägg till tillägget Composer][composer-extension-add]
3. <span data-ttu-id="a263a-194">Klicka på **OK** att acceptera villkoren.</span><span class="sxs-lookup"><span data-stu-id="a263a-194">Click **OK** to accept legal terms.</span></span> <span data-ttu-id="a263a-195">Klicka på **OK** igen för att lägga till filnamnstillägget.</span><span class="sxs-lookup"><span data-stu-id="a263a-195">Click **OK** again to add the extension.</span></span>
   
    <span data-ttu-id="a263a-196">Den **installerat tillägg** bladet visas nu tillägget Composer.</span><span class="sxs-lookup"><span data-stu-id="a263a-196">The **Installed extensions** blade will now show the Composer extension.</span></span>  
    <span data-ttu-id="a263a-197">![Composer tillägg View][composer-extension-view]</span><span class="sxs-lookup"><span data-stu-id="a263a-197">![Composer Extension View][composer-extension-view]</span></span>
4. <span data-ttu-id="a263a-198">Nu kan utföra `git add`, `git commit`, och `git push` precis som i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a263a-198">Now, perform `git add`, `git commit`, and `git push` like in the previous section.</span></span> <span data-ttu-id="a263a-199">Du ser nu att installeras Composer beroenden som definierats i composer.json.</span><span class="sxs-lookup"><span data-stu-id="a263a-199">You'll now see that Composer is installing dependencies defined in composer.json.</span></span>
   
    ![Composer tillägget lyckades][composer-extension-success]

## <a name="next-steps"></a><span data-ttu-id="a263a-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a263a-201">Next steps</span></span>
<span data-ttu-id="a263a-202">Mer information finns i [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="a263a-202">For more information, see the [PHP Developer Center](/develop/php/).</span></span>

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
