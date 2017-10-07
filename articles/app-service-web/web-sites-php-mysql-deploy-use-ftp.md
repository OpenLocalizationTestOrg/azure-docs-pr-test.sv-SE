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
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>Skapa en PHP-MySQL-webbapp i Azure App Service och distribuera med FTP
Den här kursen visar hur toocreate PHP-MySQL web app och toodeploy den med hjälp av FTP. Den här kursen förutsätter att du har [PHP][install-php], [MySQL][install-mysql], en webbserver och en FTP-klient installerad på datorn. hello anvisningarna i den här kursen kan tillämpas på alla operativsystem, inklusive Windows, Mac- och Linux. När du slutför den här guiden kommer du har ett webbprogram för PHP/MySQL som körs i Azure.

Du kommer att lära dig:

* Hur hello toocreate ett webbprogram och en MySQL-databas med hjälp av Azure Portal. Eftersom PHP är aktiverad i Web Apps som standard, inget särskilt är obligatoriska toorun PHP-kod.
* Hur toopublish tooAzure appen med FTP.

Genom att följa den här självstudiekursen skapar du en enkel registrering webbapp i PHP. hello program ska finnas i en Webbapp. En skärmbild av programmet hello slutförts understiger:

![Azure PHP-webbplats][running-app]

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett konto, gå för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden. 
> 
> 

## <a name="create-a-web-app-and-set-up-ftp-publishing"></a>Skapa en webbapp och konfigurera FTP-publicering
Följ dessa steg toocreate ett webbprogram och en MySQL-databas:

1. Inloggningen toohello [Azure Portal][management-portal].
2. Klicka på hello **+ ny** ikonen hello längst upp till vänster i hello Azure-portalen.
   
    ![Skapa ny Azure-webbplats][new-website]
3. I hello söktyp **Webbapp + MySQL** och klicka på **Webbapp + MySQL**.
   
    ![Anpassade skapa en ny webbplats][custom-create]
4. Klicka på **Skapa**. Ange ett unikt appnamn för tjänsten, ett giltigt namn för hello resursgruppens namn och en ny serviceplan.
   
    ![Ange resursgruppens namn][resource-group]
5. Ange värden för den nya databasen, inklusive godkänner toohello juridiska villkor.
   
    ![Skapa ny MySQL-databas][new-mysql-db]
6. När hello webbprogrammet har skapats visas hello nya app service-bladet.
7. Klicka på **inställningar** > **distributionsbehörigheterna**. 
   
    ![Ange autentiseringsuppgifter för distribution][set-deployment-credentials]
8. tooenable FTP-publicering, måste du ange ett användarnamn och lösenord. Spara hello autentiseringsuppgifter och anteckna hello användarnamn och lösenord som du skapar.
   
    ![Skapa autentiseringsuppgifter för publicering][portal-ftp-username-password]

## <a name="build-and-test-your-app-locally"></a>Skapa och testa appen lokalt
hello registreringen programmet är ett enkelt PHP-program som du kan använda tooregister för en händelse genom att tillhandahålla ditt namn och e-postadress. Information om föregående månaderna visas i en tabell. Registreringsinformation lagras i en MySQL-databas. hello app består av två filer:

* **index.php**: Visar ett formulär för registrering och en tabell som innehåller registrant information.
* **createtable.php**: skapar hello MySQL tabell för hello program. Den här filen ska bara användas en gång.

toobuild och kör hello appen lokalt, gör hello nedan. Observera att dessa åtgärder förutsätter att du har PHP, MySQL och en webbserver som konfigurerats på den lokala datorn och att du har aktiverat hello [PDO-tillägget för MySQL][pdo-mysql].

1. Skapa en MySQL-databas som heter `registration`. Du kan göra detta från kommandoraden hello MySQL med det här kommandot:
   
        mysql> create database registration;
2. Skapa en mapp med namnet i rotkatalogen för din webbserver `registration` och skapa två filer i den - en kallas `createtable.php` och en kallas `index.php`.
3. Öppna hello `createtable.php` filen i en textredigerare eller IDE och Lägg till hello koden nedan. Den här koden kommer att använda toocreate hello `registration_tbl` tabellen i hello `registration` databas.
   
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
   > Behöver du tooupdate hello värden för <code>$user</code> och <code>$pwd</code> med din lokala MySQL-användarnamn och lösenord.
   > 
   > 
4. Öppna en webbläsare och gå för[http://localhost/registration/createtable.php][localhost-createtable]. Detta skapar hello `registration_tbl` tabellen i hello-databasen.
5. Öppna hello **index.php** filen i en textredigerare eller IDE och Lägg till hello grundläggande HTML- eller CSS-kod för hello sida (hello PHP kod läggs i senare steg).
   
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
6. Lägga till PHP-kod för att ansluta databasen toohello inom hello PHP-taggar.
   
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
   > Igen, behöver du tooupdate hello värden för <code>$user</code> och <code>$pwd</code> med din lokala MySQL-användarnamn och lösenord.
   > 
   > 
7. Följande hello databasen anslutning kod, lägger du till kod för att infoga registreringsinformation i hello-databasen.
   
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
8. Slutligen följande hello koden ovan, lägger du till kod för att hämta data från hello-databasen.
   
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

Du kan nu Bläddra för[http://localhost/registration/index.php] [ localhost-index] tootest hello app.

## <a name="get-mysql-and-ftp-connection-information"></a>Hämta MySQL- och FTP-anslutning
tooconnect toohello MySQL-databas som körs i Web Apps din kommer måste hello anslutningsinformationen. tooget MySQL anslutningsinformationen så här:

1. Från hello app service web app-bladet klickar du på hello resurs grupp länk:
   
    ![Välj resursgrupp][select-resourcegroup]
2. Klicka på hello databas från din resursgrupp:
   
    ![Välj databas][select-database]
3. Hello databasen sammanfattning, Välj **inställningar** > **egenskaper**.
   
    ![Välj egenskaper][select-properties]
4. Anteckna hello värden för `Database`, `Host`, `User Id`, och `Password`.
   
    ![Obs egenskaper][note-properties]
5. Klicka på ditt webbprogram hello **hämta Publicera profil** längst hello nedre högra hörnet av hello sidan:
   
    ![Hämta Publicera profil][download-publish-profile]
6. Öppna hello `.publishsettings` fil i en XML-redigerare. 
7. Hitta hello `<publishProfile >` element med `publishMethod="FTP"` som ser ut ungefär toothis:
   
        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>

Anteckna hello `publishUrl`, `userName`, och `userPWD` attribut.

## <a name="publish-your-app"></a>Publicera appen
När du har testat appen lokalt kan publicera du den tooyour webbprogram med hjälp av FTP. Du måste dock först tooupdate hello databasens anslutningsinformation hello program. Med hjälp av hello databasens anslutningsinformation som du tidigare hämtade (i hello **hämta MySQL och FTP-anslutningsinformationen** avsnitt), uppdatering hello följande information i **både** hello `createdatabase.php` och `index.php` filer med hello lämpliga värden:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Nu är du redo toopublish appen med hjälp av FTP.

1. Öppna din FTP-klient föredrar.
2. Ange hello *värdnamnsdelen* från hello `publishUrl` attributet ovan i FTP-klient.
3. Ange hello `userName` och `userPWD` attribut som du antecknade ovan oförändrad i FTP-klient.
4. Upprätta en anslutning.

När du har anslutit kommer du att kunna tooupload och hämta filer efter behov. Se till att du överför filer toohello rotkatalog, vilket är `/site/wwwroot`.

Efter överföring både `index.php` och `createtable.php`, bläddra för**http://[site name].azurewebsites.net/createtable.php** toocreate hello MySQL för hello programmet och sedan bläddra för**http://[site namn ].azurewebsites.net/index.php** toobegin med hello program.

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello [PHP Developer Center](/develop/php/).

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

