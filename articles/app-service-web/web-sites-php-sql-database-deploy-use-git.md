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
# <a name="create-a-php-sql-web-app-and-deploy-tooazure-app-service-using-git"></a>Skapa en PHP-SQL-webbapp och distribuera tooAzure Apptjänst med Git
Den här kursen visar hur toocreate en PHP web app i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) som ansluter tooAzure SQL-databas och hur toodeploy den med hjälp av Git. Den här kursen förutsätter att du har [PHP][install-php], [SQL Server Express][install-SQLExpress], hello [Drivers Microsoft för SQL Server för PHP ](http://www.microsoft.com/download/en/details.aspx?id=20098), och [Git] [ install-git] installerat på datorn. När du slutför den här guiden kommer du har ett PHP-SQL-webbprogram som körs i Azure.

> [!NOTE]
> Du kan installera och konfigurera PHP, SQL Server Express och hello Microsoft Drivers för SQL Server för PHP med hello [Microsoft Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx).
> 
> 

Du kommer att lära dig:

* Hur toocreate en Azure web app och en SQL-databas med hjälp av hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715). Eftersom PHP är aktiverad i App Service Web Apps som standard, inget särskilt är obligatoriska toorun PHP-kod.
* Hur toopublish och publicera dina program tooAzure med Git.

Genom att följa den här självstudiekursen skapar du en enkel registrering webbprogram i PHP. hello program ska finnas i en Azure-webbplats. En skärmbild av programmet hello slutförts understiger:

![Azure PHP-webbplats](./media/web-sites-php-sql-database-deploy-use-git/running_app_3.png)

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="create-an-azure-web-app-and-set-up-git-publishing"></a>Skapa en Azure webbapp och konfigurera Git-publicering
Följ dessa steg toocreate en Azure webbapp och en SQL-databas:

1. Logga in toohello [Azure Portal](https://portal.azure.com/).
2. Öppna hello Azure Marketplace genom att klicka på hello **ny** ikonen hello längst upp till vänster i hello instrumentpanelen, klicka på **Markera alla** nästa tooMarketplace och välja **webb + mobilt**.
3. Välj i hello Marketplace, **webb + mobilt**.
4. Klicka på hello **Webbapp + SQL** ikon.
5. När du har läst hello beskrivning av hello webbapp + SQL appen, Välj **skapa**.
6. Klicka på varje del (**resursgruppen**, **Web App**, **databasen**, och **prenumeration**) och ange eller Välj värden för hello krävs fält:
   
   * Ange ett URL-namn du väljer    
   * Konfigurera servern Databasautentiseringsuppgifter
   * Välj hello region närmaste tooyou
     
     ![konfigurera appen](./media/web-sites-php-sql-database-deploy-use-git/configure-db-settings.png)
7. När du definierat hello webbprogram, klickar du på **skapa**.
   
    När hello webbprogrammet har skapats, hello **meddelanden** knappen blinkar en grön **lyckade** och hello resurs grupp blad öppna tooshow båda hello web app och hello SQL-databas i hello grupp.
8. Klicka på hello webbapp ikon i hello resurs grupp bladet tooopen hello webbapps blad.
   
    ![Web Apps resursgrupp](./media/web-sites-php-sql-database-deploy-use-git/resource-group-blade.png)
9. I **inställningar** klickar du på **kontinuerlig distribution** > **konfigurera nödvändiga inställningar**. Välj **lokal Git-lagringsplats** och på **OK**.
   
    ![var finns i källkoden](./media/web-sites-php-sql-database-deploy-use-git/setup-local-git.png)
   
    Om du inte har registrerat en Git-lagringsplats innan måste du ange ett användarnamn och lösenord. toodo, genom att välja **inställningar** > **distributionsbehörigheterna** i hello webbapps blad.
   
    ![](./media/web-sites-php-sql-database-deploy-use-git/deployment-credentials.png)
10. I **inställningar** klickar du på **egenskaper** toosee hello Git fjärr-URL måste toouse toodeploy appen PHP senare.

## <a name="get-sql-database-connection-information"></a>Hämta anslutningsinformationen för SQL-databas
tooconnect toohello SQL Database-instans som är länkade tooyour webbprogram, måste din kommer hello anslutningsinformation som du angav när du skapade hello-databasen. tooget hello anslutningsinformationen för SQL-databas, så här:

1. Tillbaka i bladet hello resursgruppens namn, klickar du på ikonen hello SQL database.
2. Hello SQL database-bladet klickar du på **inställningar** > **egenskaper**, klicka på **visa databasanslutningssträngar**. 
   
    ![Visa egenskaper för databas](./media/web-sites-php-sql-database-deploy-use-git/view-database-properties.png)
3. Från hello **PHP** avsnittet för hello resulterande dialog, anteckna hello värden för `Server`, `SQL Database`, och `User Name`. Du använder dessa värden senare när publicera din PHP web app tooAzure Apptjänst.

## <a name="build-and-test-your-application-locally"></a>Skapa och testa programmet lokalt
hello registreringen programmet är ett enkelt PHP-program som du kan använda tooregister för en händelse genom att tillhandahålla ditt namn och e-postadress. Information om föregående månaderna visas i en tabell. Registreringsinformation lagras i en instans av SQL-databas. hello program består av två filer (kopiera och klistra in koden finns nedan):

* **index.php**: Visar ett formulär för registrering och en tabell som innehåller registrant information.
* **createtable.php**: skapar hello SQL-databastabell för hello program. Den här filen ska bara användas en gång.

toorun hello programmet lokalt, följ hello stegen nedan. Observera att de här stegen förutsätter att du har PHP och SQL Server Express ställa in på den lokala datorn och att du har aktiverat hello [PDO-tillägg för SQL Server][pdo-sqlsrv].

1. Skapa en SQL Server-databas som heter `registration`. Du kan göra detta från hello `sqlcmd` Kommandotolken med följande kommandon:
   
        >sqlcmd -S localhost\sqlexpress -U <local user name> -P <local password>
        1> create database registration
        2> GO    
2. Skapa två filer i den - en kallas i din tillämpningsprogrammets rotkatalog `createtable.php` och en kallas `index.php`.
3. Öppna hello `createtable.php` filen i en textredigerare eller IDE och Lägg till hello koden nedan. Den här koden kommer att använda toocreate hello `registration_tbl` tabellen i hello `registration` databas.
   
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
   
    Observera att du måste tooupdate hello värden för <code>$user</code> och <code>$pwd</code> med dina lokala SQL Server-användarnamn och lösenord.
4. I en terminal på hello rotkatalog hello programmet typen hello följande kommando:
   
        php -S localhost:8000
5. Öppna en webbläsare och gå för**http://localhost:8000/createtable.php**. Detta skapar hello `registration_tbl` tabellen i hello-databasen.
6. Öppna hello **index.php** filen i en textredigerare eller IDE och Lägg till hello grundläggande HTML- eller CSS-kod för hello sida (hello PHP kod läggs i senare steg).
   
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
7. Lägga till PHP-kod för att ansluta databasen toohello inom hello PHP-taggar.
   
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
   
    Igen, behöver du tooupdate hello värden för <code>$user</code> och <code>$pwd</code> med din lokala MySQL-användarnamn och lösenord.
8. Följande hello databasen anslutning kod, lägger du till kod för att infoga registreringsinformation i hello-databasen.
   
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
9. Slutligen följande hello koden ovan, lägger du till kod för att hämta data från hello-databasen.
   
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

Du kan nu Bläddra för**http://localhost:8000/index.php** tootest hello program.

## <a name="publish-your-application"></a>Publicera programmet
När du har testat programmet lokalt, kan du publicera den tooApp Service Web Apps med Git. Du måste dock först tooupdate hello databasens anslutningsinformation hello program. Med hjälp av hello databasens anslutningsinformation som du tidigare hämtade (i hello **Hämta SQL-databas anslutningsinformationen** avsnitt), uppdatering hello följande information i **både** hello `createdatabase.php` och `index.php` filer med hello lämpliga värden:

    // DB connection info
    $host = "tcp:<value of Server>";
    $user = "<value of User Name>";
    $pwd = "<your password>";
    $db = "<value of SQL Database>";

> [!NOTE]
> I hello <code>$host</code>, hello värdet på servern måste vara föregås av <code>tcp:</code>.
> 
> 

Nu är du redo tooset in Git-publicering och publicera programmet hello.

> [!NOTE]
> Det här är samma steg som anges i hello slutet av hello hello **skapa en Azure webbapp och konfigurera Git-publicering** ovan.
> 
> 

1. Öppna GitBash (eller en terminal om Git i din `PATH`), ändra kataloger toohello rotkatalogen för programmet (hello **registrering** directory), och kör hello följande kommandon:
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    Du uppmanas att hello-lösenord som du skapade tidigare.
2. Bläddra för**http://[web app name].azurewebsites.net/createtable.php** toocreate hello SQL-databastabell för hello program.
3. Bläddra för**http://[web app name].azurewebsites.net/index.php** toobegin med hello program.

När du har publicerat programmet, du kan göra ändringar tooit och använda Git toopublish dem. 

## <a name="publish-changes-tooyour-application"></a>Publicera ändringar tooyour program
toopublish ändras tooapplication, gör du följande:

1. Ändra tooyour programmet lokalt.
2. Öppna GitBash (eller en terminal it Git finns i din `PATH`), ändra kataloger toohello rotkatalogen för programmet och kör följande kommandon hello:
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    Du uppmanas att hello-lösenord som du skapade tidigare.
3. Bläddra för**http://[web app name].azurewebsites.net/index.php** toosee ändringarna.

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[pdo-sqlsrv]: http://php.net/pdo_sqlsrv

