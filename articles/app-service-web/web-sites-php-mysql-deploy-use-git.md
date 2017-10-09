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
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>Skapa en PHP-MySQL-webbapp i Azure App Service och distribuera med Git
Den här kursen visar hur toocreate PHP-MySQL web app och toodeploy den för[Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) med Git. Du kommer att använda [PHP][install-php], hello MySQL-kommandoradsverktyget (en del av [MySQL][install-mysql]), och [Git] [ install-git] installerat på datorn. hello anvisningarna i den här kursen kan tillämpas på alla operativsystem, inklusive Windows, Mac- och Linux. När du slutför den här guiden kommer du har ett webbprogram för PHP/MySQL som körs i Azure.

Du kommer att lära dig:

* Hur toocreate ett webbprogram och en MySQL-databas med hjälp av hello [Azure Portal][management-portal]. Eftersom PHP är aktiverat i [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) inget särskilt är som standard krävs toorun PHP-kod.
* Hur toopublish och publicera dina program tooAzure med Git.
* Hur tooenable hello Composer tillägget tooautomate Composer uppgifter vid varje `git push`.

Genom att följa den här självstudiekursen skapar du en enkel registrering webbapp i PHP. hello program ska finnas i Web Apps. En skärmbild av programmet hello slutförts understiger:

![Azure PHP-webbplats][running-app]

## <a name="set-up-hello-development-environment"></a>Ställa in hello utvecklingsmiljö
Den här kursen förutsätter att du har [PHP][install-php], hello MySQL-kommandoradsverktyget (en del av [MySQL][install-mysql]), och [Git] [ install-git] installerat på datorn.

<a id="create-web-site-and-set-up-git"></a>

## <a name="create-a-web-app-and-set-up-git-publishing"></a>Skapa en webbapp och konfigurera Git-publicering
Följ dessa steg toocreate ett webbprogram och en MySQL-databas:

1. Inloggningen toohello [Azure Portal][management-portal].
2. Klicka på hello **ny** ikon.
3. Klicka på **finns alla** nästa för**Marketplace**. 
4. Klicka på **webb + mobilt**, sedan **Webbapp + MySQL**. Klicka på **Skapa**.
5. Ange ett giltigt namn för resursgruppen.
   
    ![Ange resursgruppens namn][resource-group]
6. Ange värden för din nya webbapp.
   
    ![Skapa webbapp][new-web-app]
7. Ange värden för den nya databasen, inklusive godkänner toohello juridiska villkor.
   
    ![Skapa ny MySQL-databas][new-mysql-db]
8. När hello webbprogrammet har skapats visas hello nytt web app blad.
9. I **inställningar** klickar du på **kontinuerlig distribution**, klickar du på *konfigurera nödvändiga inställningar*.
   
    ![Konfigurera Git-publicering][setup-publishing]
10. Välj **lokal Git-lagringsplats** för hello källa.
    
     ![Konfigurera Git-lagringsplats][setup-repository]
11. tooenable Git publicering, måste du ange ett användarnamn och lösenord. Anteckna hello användarnamn och lösenord som du skapar. (Om du har ställt in en Git-lagringsplats innan det här steget hoppas över.)
    
     ![Skapa autentiseringsuppgifter för publicering][credentials]

## <a name="get-remote-mysql-connection-information"></a>Hämta anslutningsinformation för MySQL
tooconnect toohello MySQL-databas som körs i Web Apps din kommer måste hello anslutningsinformationen. tooget MySQL anslutningsinformationen så här:

1. Klicka på hello databas från din resursgrupp:
   
    ![Välj databas][select-database]
2. Från hello databasen **inställningar**väljer **egenskaper**.
   
    ![Välj egenskaper][select-properties]
3. Anteckna hello värden för `Database`, `Host`, `User Id`, och `Password`.
   
    ![Obs egenskaper][note-properties]

## <a name="build-and-test-your-app-locally"></a>Skapa och testa appen lokalt
Nu när du har skapat ett webbprogram, kan du utveckla ditt program lokalt och sedan distribuera efter testning.

hello registreringen programmet är ett enkelt PHP-program som du kan använda tooregister för en händelse genom att tillhandahålla ditt namn och e-postadress. Information om föregående månaderna visas i en tabell. Registreringsinformation lagras i en MySQL-databas. hello program består av en fil (kopiera och klistra in koden finns nedan):

* **index.php**: Visar ett formulär för registrering och en tabell som innehåller registrant information.

toobuild och kör hello programmet lokalt, följer du hello stegen nedan. Observera att de här stegen förutsätter att du har hello PHP och MySQL kommandoradsverktyget (del av MySQL) på den lokala datorn och att du har aktiverat hello [PDO-tillägget för MySQL][pdo-mysql].

1. Ansluta toohello MySQL fjärrserver, genom att använda hello värde för `Data Source`, `User Id`, `Password`, och `Database` som du hämtade tidigare:
   
        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]
2. hello MySQL kommandotolken visas:
   
        mysql>
3. Klistra in följande hello `CREATE TABLE` kommandot toocreate hello `registration_tbl` tabell i databasen:
   
        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);
4. Skapa i hello roten för lokala programmappen **index.php** fil.
5. Öppna hello **index.php** filen i en textredigerare eller IDE och Lägg till hello följande kod och fullständig hello nödvändiga ändringar som har markerats med `//TODO:` kommentarer.

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

1. I en terminal gå tooyour program-mappen och Skriv hello följande kommando:
   
       php -S localhost:8000

Du kan nu Bläddra för**http://localhost: 8000 /** tootest hello program.

## <a name="publish-your-app"></a>Publicera appen
När du har testat appen lokalt kan publicera du den tooWeb appar med Git. Du initiera lokal Git-lagringsplats och publicera programmet hello.

> [!NOTE]
> Dessa är hello samma steg som visas i hello Azure Portal hello slutet av hello skapa en webbapp och ange in Git Publishing ovan.
> 
> 

1. (Valfritt)  Om du har glömt eller tappas bort Git remote repostitory URL: en, navigera toohello web appegenskaper för hello Azure-portalen.
2. Öppna GitBash (eller en terminal om Git i din `PATH`), ändra kataloger toohello rotkatalogen för programmet och kör följande kommandon hello:
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    Du uppmanas att hello-lösenord som du skapade tidigare.
   
    ![Den inledande Push tooAzure via Git][git-initial-push]
3. Bläddra för**http://[site name].azurewebsites.net/index.php** toobegin med hjälp av hello (den här informationen lagras på instrumentpanelen konto):
   
    ![Azure PHP-webbplats][running-app]

När du har publicerat din app, kan du göra ändringar tooit och använda Git toopublish dem.

## <a name="publish-changes-tooyour-app"></a>Publicera ändringar tooyour app
toopublish ändringar tooyour appen, gör du följande:

1. Göra ändringar tooyour appen lokalt.
2. Öppna GitBash (eller en terminal it Git finns i din `PATH`), ändra kataloger toohello rotkatalogen för din app och kör följande kommandon hello:
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    Du uppmanas att hello-lösenord som du skapade tidigare.
   
    ![Skicka ändringar av platsen tooAzure via Git][git-change-push]
3. Bläddra för**http://[site name].azurewebsites.net/index.php** toosee din app och eventuella ändringar som du har gjort:
   
    ![Azure PHP-webbplats][running-app]

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

<a name="composer"></a>

## <a name="enable-composer-automation-with-hello-composer-extension"></a>Aktivera Composer automatisering med hello tillägget Composer
Som standard göra hello git-Distributionsprocess i App Service inte något med composer.json, om du har en i PHP-projektet. Du kan aktivera composer.json bearbetning under `git push` genom att aktivera hello Composer tillägg.

1. I din PHP web Apps bladet i hello [Azure-portalen][management-portal], klickar du på **verktyg** > **tillägg**.
   
    ![Composer tilläggsinställningar][composer-extension-settings]
2. Klicka på **Lägg till**, klicka på **Composer**.
   
    ![Lägg till tillägget Composer][composer-extension-add]
3. Klicka på **OK** tooaccept juridiska villkor. Klicka på **OK** igen tooadd hello-tillägget.
   
    Hej **installerat tillägg** bladet visas nu hello Composer tillägg.  
    ![Composer tillägg View][composer-extension-view]
4. Nu kan utföra `git add`, `git commit`, och `git push` precis som i hello föregående avsnitt. Du ser nu att installeras Composer beroenden som definierats i composer.json.
   
    ![Composer tillägget lyckades][composer-extension-success]

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello [PHP Developer Center](/develop/php/).

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
