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
# <a name="create-a-php-sql-web-app-and-deploy-to-azure-app-service-using-git"></a>Skapa en PHP-SQL-webbapp och distribuera till Azure App Service med Git
Den här kursen visar hur du skapar en PHP-webbapp i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) som ansluter till Azure SQL Database och hur du distribuerar den med Git. Den här kursen förutsätter att du har [PHP][install-php], [SQL Server Express][install-SQLExpress], [Drivers Microsoft för SQL Server för PHP ](http://www.microsoft.com/download/en/details.aspx?id=20098), och [Git] [ install-git] installerat på datorn. När du slutför den här guiden kommer du har ett PHP-SQL-webbprogram som körs i Azure.

> [!NOTE]
> Du kan installera och konfigurera PHP, SQL Server Express och Microsoft Drivers för SQL Server för PHP med hjälp av den [Microsoft Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx).
> 
> 

Du kommer att lära dig:

* Så här skapar du ett Azure webbapp och en SQL-databas med hjälp av den [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715). Eftersom PHP är aktiverad i App Service Web Apps som standard, krävs inget särskilt att köra din PHP-kod.
* Hur man publicerar och publicera programmet till Azure med Git.

Genom att följa den här självstudiekursen skapar du en enkel registrering webbprogram i PHP. Programmet ska finnas i en Azure-webbplats. En skärmbild av det färdiga programmet understiger:

![Azure PHP-webbplats](./media/web-sites-php-sql-database-deploy-use-git/running_app_3.png)

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="create-an-azure-web-app-and-set-up-git-publishing"></a>Skapa en Azure webbapp och konfigurera Git-publicering
Följ dessa steg om du vill skapa ett Azure webbapp och en SQL-databas:

1. Logga in på [Azure Portal](https://portal.azure.com/).
2. Öppna Azure Marketplace genom att klicka på **ny** ikonen längst upp till vänster i instrumentpanelen, klicka på **Markera alla** bredvid Marketplace och välja **webb + mobilt**.
3. Välj i Marketplace, **webb + mobilt**.
4. Klicka på den **Webbapp + SQL** ikon.
5. När du har läst igenom beskrivningen av webbapp + SQL app markerar **skapa**.
6. Klicka på varje del (**resursgruppen**, **Webbapp**, **databasen**, och **prenumeration**) och ange eller Välj värden för de obligatoriska fälten :
   
   * Ange ett URL-namn du väljer    
   * Konfigurera servern Databasautentiseringsuppgifter
   * Välj regionen som är närmast dig
     
     ![konfigurera appen](./media/web-sites-php-sql-database-deploy-use-git/configure-db-settings.png)
7. När du definierat webbappen, klickar du på **skapa**.
   
    När webbappen har skapats kan den **meddelanden** knappen blinkar en grön **lyckade** och bladet för resursgruppen öppna om du vill visa både webbappen och SQL-databas i gruppen.
8. Klicka på ikonen för webbappens i bladet för resursgruppen att öppna den webbapps blad.
   
    ![Web Apps resursgrupp](./media/web-sites-php-sql-database-deploy-use-git/resource-group-blade.png)
9. I **inställningar** klickar du på **kontinuerlig distribution** > **konfigurera nödvändiga inställningar**. Välj **lokal Git-lagringsplats** och på **OK**.
   
    ![var finns i källkoden](./media/web-sites-php-sql-database-deploy-use-git/setup-local-git.png)
   
    Om du inte har registrerat en Git-lagringsplats innan måste du ange ett användarnamn och lösenord. Gör detta genom att klicka på **inställningar** > **distributionsbehörigheterna** webbappens-bladet.
   
    ![](./media/web-sites-php-sql-database-deploy-use-git/deployment-credentials.png)
10. I **inställningar** klickar du på **egenskaper** att se Git extern URL som du behöver använda för att distribuera din PHP-app senare.

## <a name="get-sql-database-connection-information"></a>Hämta anslutningsinformationen för SQL-databas
Att ansluta till SQL Database-instans som är kopplad till ditt webbprogram, din kommer måste anslutningsinformation som du angav när du skapade databasen. Följ dessa steg för att hämta anslutningsinformationen för SQL-databasen:

1. Tillbaka i den resursgrupp-bladet klickar du på ikonen för SQL-databasen.
2. I bladet för SQL-databasen, klickar du på **inställningar** > **egenskaper**, klicka på **visa databasanslutningssträngar**. 
   
    ![Visa egenskaper för databas](./media/web-sites-php-sql-database-deploy-use-git/view-database-properties.png)
3. Från den **PHP** avsnitt i dialogrutan resulterande anteckna värdena för `Server`, `SQL Database`, och `User Name`. Du använder dessa värden senare när du publicerar PHP-webbapp till Azure App Service.

## <a name="build-and-test-your-application-locally"></a>Skapa och testa programmet lokalt
Registreringen programmet är ett enkelt PHP-program som gör att du kan registrera dig för en händelse genom att tillhandahålla ditt namn och e-postadress. Information om föregående månaderna visas i en tabell. Registreringsinformation lagras i en instans av SQL-databas. Programmet består av två filer (kopiera och klistra in koden finns nedan):

* **index.php**: Visar ett formulär för registrering och en tabell som innehåller registrant information.
* **createtable.php**: skapar SQL-databastabell för programmet. Den här filen ska bara användas en gång.

Följ stegen nedan om du vill köra programmet lokalt. Observera att de här stegen förutsätter att du har PHP och SQL Server Express på den lokala datorn och att du har aktiverat den [PDO-tillägg för SQL Server][pdo-sqlsrv].

1. Skapa en SQL Server-databas som heter `registration`. Du kan göra detta från den `sqlcmd` Kommandotolken med följande kommandon:
   
        >sqlcmd -S localhost\sqlexpress -U <local user name> -P <local password>
        1> create database registration
        2> GO    
2. Skapa två filer i den - en kallas i din tillämpningsprogrammets rotkatalog `createtable.php` och en kallas `index.php`.
3. Öppna den `createtable.php` filen i en textredigerare eller IDE och lägga till koden nedan. Den här koden används för att skapa den `registration_tbl` tabell i den `registration` databas.
   
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
   
    Observera att du måste uppdatera värdena för <code>$user</code> och <code>$pwd</code> med dina lokala SQL Server-användarnamn och lösenord.
4. Skriv följande kommando i en terminal i rotkatalogen för programmet:
   
        php -S localhost:8000
5. Öppna en webbläsare och gå till **http://localhost:8000/createtable.php**. Detta skapar den `registration_tbl` tabell i databasen.
6. Öppna den **index.php** filen i en textredigerare eller IDE och Lägg till grundläggande HTML- och CSS kod på sidan (PHP-koden kommer att läggas till i senare steg).
   
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
7. Lägga till PHP-kod för att ansluta till databasen i PHP-taggarna.
   
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
   
    Igen, måste du uppdatera värdena för <code>$user</code> och <code>$pwd</code> med din lokala MySQL-användarnamn och lösenord.
8. Följande kod för databas-anslutningsfel lägger du till kod för att infoga registreringsinformation i databasen.
   
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
9. Slutligen följande koden ovan lägger du till kod för att hämta data från databasen.
   
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

Du kan nu bläddra till **http://localhost:8000/index.php** för att testa programmet.

## <a name="publish-your-application"></a>Publicera programmet
När du har testat programmet lokalt, kan du publicera den till App Service Web Apps med Git. Men måste du först uppdatera anslutningsinformationen i programmet. Använder anslutningsinformationen för databasen som du tidigare hämtade (i den **Hämta SQL-databas anslutningsinformationen** avsnitt), uppdatera följande information i **både** i `createdatabase.php` och `index.php` filer med lämpliga värden:

    // DB connection info
    $host = "tcp:<value of Server>";
    $user = "<value of User Name>";
    $pwd = "<your password>";
    $db = "<value of SQL Database>";

> [!NOTE]
> I den <code>$host</code>, värdet för Server måste vara föregås av <code>tcp:</code>.
> 
> 

Nu är du redo att konfigurera Git-publicering och publicera programmet.

> [!NOTE]
> Det här är samma steg som anges i slutet av den **skapa en Azure webbapp och konfigurera Git-publicering** ovan.
> 
> 

1. Öppna GitBash (eller en terminal om Git i din `PATH`), ändrar sökvägen till rotkatalogen för programmet (den **registrering** directory), och kör följande kommandon:
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    Du uppmanas att lösenordet du skapade tidigare.
2. Bläddra till **http://[web app name].azurewebsites.net/createtable.php** att skapa SQL-databastabell för programmet.
3. Bläddra till **http://[web app name].azurewebsites.net/index.php** kan börja använda programmet.

När du har publicerat programmet kan du använda Git för att publicera dem börjar att göra ändringar. 

## <a name="publish-changes-to-your-application"></a>Publicera ändringar i programmet
Följ dessa steg om du vill publicera ändringar för programmet:

1. Göra ändringar i programmet lokalt.
2. Öppna GitBash (eller en terminal it Git finns i din `PATH`), ändrar sökvägen till rotkatalogen för programmet och kör följande kommandon:
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    Du uppmanas att lösenordet du skapade tidigare.
3. Bläddra till **http://[web app name].azurewebsites.net/index.php** att se dina ändringar.

## <a name="whats-changed"></a>Nyheter
* En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[pdo-sqlsrv]: http://php.net/pdo_sqlsrv

