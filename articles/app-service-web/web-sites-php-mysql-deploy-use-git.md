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
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>Skapa en PHP-MySQL-webbapp i Azure App Service och distribuera med Git
Den här kursen visar hur du skapar ett webbprogram för PHP-MySQL och hur du distribuerar det till [Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) med Git. Du kommer att använda [PHP][install-php], MySQL-kommandoradsverktyget (en del av [MySQL][install-mysql]), och [Git] [ install-git] installerat på datorn. Anvisningarna i den här kursen kan tillämpas på alla operativsystem, inklusive Windows, Mac- och Linux. När du slutför den här guiden kommer du har ett webbprogram för PHP/MySQL som körs i Azure.

Du kommer att lära dig:

* Hur du skapar ett webbprogram och en MySQL-databas med den [Azure Portal][management-portal]. Eftersom PHP är aktiverat i [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) som standard inget särskilt krävs för att köra din PHP-kod.
* Hur man publicerar och publicera programmet till Azure med Git.
* Så här aktiverar du tillägget Composer att automatisera Composer uppgifter vid varje `git push`.

Genom att följa den här självstudiekursen skapar du en enkel registrering webbapp i PHP. Programmet ska finnas i Web Apps. En skärmbild av det färdiga programmet understiger:

![Azure PHP-webbplats][running-app]

## <a name="set-up-the-development-environment"></a>Konfigurera utvecklingsmiljön
Den här kursen förutsätter att du har [PHP][install-php], MySQL-kommandoradsverktyget (en del av [MySQL][install-mysql]), och [Git] [ install-git] installerat på datorn.

<a id="create-web-site-and-set-up-git"></a>

## <a name="create-a-web-app-and-set-up-git-publishing"></a>Skapa en webbapp och konfigurera Git-publicering
Följ dessa steg för att skapa en webbapp och en MySQL-databas:

1. Logga in på den [Azure-portalen][management-portal].
2. Klicka på den **ny** ikon.
3. Klicka på **finns alla** bredvid **Marketplace**. 
4. Klicka på **webb + mobilt**, sedan **Webbapp + MySQL**. Klicka på **Skapa**.
5. Ange ett giltigt namn för resursgruppen.
   
    ![Ange resursgruppens namn][resource-group]
6. Ange värden för din nya webbapp.
   
    ![Skapa webbapp][new-web-app]
7. Ange värden för den nya databasen, inklusive godkänner du de juridiska villkoren.
   
    ![Skapa ny MySQL-databas][new-mysql-db]
8. När webbappen har skapats visas bladet ny webbapp.
9. I **inställningar** klickar du på **kontinuerlig distribution**, klickar du på *konfigurera nödvändiga inställningar*.
   
    ![Konfigurera Git-publicering][setup-publishing]
10. Välj **lokal Git-lagringsplats** för datakällan.
    
     ![Konfigurera Git-lagringsplats][setup-repository]
11. Du måste ange ett användarnamn och lösenord om du vill aktivera Git-publicering. Anteckna användarnamnet och lösenordet som du skapar. (Om du har ställt in en Git-lagringsplats innan det här steget hoppas över.)
    
     ![Skapa autentiseringsuppgifter för publicering][credentials]

## <a name="get-remote-mysql-connection-information"></a>Hämta anslutningsinformation för MySQL
Att ansluta till MySQL-databas som körs i Web Apps din kommer måste anslutningsinformationen. Följ dessa steg för att få information om MySQL anslutning:

1. Klicka på databasen från din resursgrupp:
   
    ![Välj databas][select-database]
2. Från databasen **inställningar**väljer **egenskaper**.
   
    ![Välj egenskaper][select-properties]
3. Anteckna värdena för `Database`, `Host`, `User Id`, och `Password`.
   
    ![Obs egenskaper][note-properties]

## <a name="build-and-test-your-app-locally"></a>Skapa och testa appen lokalt
Nu när du har skapat ett webbprogram, kan du utveckla ditt program lokalt och sedan distribuera efter testning.

Registreringen programmet är ett enkelt PHP-program som gör att du kan registrera dig för en händelse genom att tillhandahålla ditt namn och e-postadress. Information om föregående månaderna visas i en tabell. Registreringsinformation lagras i en MySQL-databas. Programmet består av en fil (kopiera och klistra in koden finns nedan):

* **index.php**: Visar ett formulär för registrering och en tabell som innehåller registrant information.

Följ stegen nedan om du vill skapa och köra programmet lokalt. Observera att de här stegen förutsätter att du har PHP och MySQL kommandoradsverktyget (del av MySQL) på den lokala datorn och att du har aktiverat den [PDO-tillägget för MySQL][pdo-mysql].

1. Ansluta till fjärrservern i MySQL med hjälp av värdet för `Data Source`, `User Id`, `Password`, och `Database` som du hämtade tidigare:
   
        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]
2. MySQL-kommandotolken visas:
   
        mysql>
3. Klistra in följande `CREATE TABLE` kommando för att skapa den `registration_tbl` tabell i databasen:
   
        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);
4. Skapa i roten av den lokala programmappen **index.php** fil.
5. Öppna den **index.php** filen i en textredigerare eller IDE och Lägg till följande kod och slutföra de nödvändiga ändringarna som markerats med `//TODO:` kommentarer.

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

1. Gå till programmappen i en terminal och Skriv följande kommando:
   
       php -S localhost:8000

Du kan nu bläddra till **http://localhost: 8000 /** för att testa programmet.

## <a name="publish-your-app"></a>Publicera din app
När du har testat appen lokalt kan publicera du den till Webbappar med Git. Du initiera lokal Git-lagringsplats och publicera programmet.

> [!NOTE]
> Det här är samma steg som visas i Azure-portalen i slutet av den skapa en webbapp och uppsättningen in Git Publishing ovan.
> 
> 

1. (Valfritt)  Om du har glömt eller tappas bort Git remote repostitory URL: en, navigera till egenskaperna web app på Azure Portal.
2. Öppna GitBash (eller en terminal om Git i din `PATH`), ändrar sökvägen till rotkatalogen för programmet och kör följande kommandon:
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    Du uppmanas att lösenordet du skapade tidigare.
   
    ![Inledande Push till Azure via Git][git-initial-push]
3. Bläddra till **http://[site name].azurewebsites.net/index.php** kan börja använda programmet (den här informationen lagras på instrumentpanelen konto):
   
    ![Azure PHP-webbplats][running-app]

När du har publicerat din app kan du använda Git för att publicera dem börjar att göra ändringar.

## <a name="publish-changes-to-your-app"></a>Publicera ändringar i din app
Följ dessa steg om du vill publicera ändringar i appen:

1. Göra ändringar i din app lokalt.
2. Öppna GitBash (eller en terminal it Git finns i din `PATH`), ändrar sökvägen till rotkatalogen för din app och kör följande kommandon:
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    Du uppmanas att lösenordet du skapade tidigare.
   
    ![Skicka ändringar till Azure via Git][git-change-push]
3. Bläddra till **http://[site name].azurewebsites.net/index.php** att se din app och eventuella ändringar som du har gjort:
   
    ![Azure PHP-webbplats][running-app]

> [!NOTE]
> Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

<a name="composer"></a>

## <a name="enable-composer-automation-with-the-composer-extension"></a>Aktivera Composer automatisering med tillägget Composer
Som standard göra git-Distributionsprocess i App Service inte något med composer.json, om du har en i PHP-projektet. Du kan aktivera composer.json bearbetning under `git push` genom att aktivera tillägget Composer.

1. I din PHP web appens blad i den [Azure-portalen][management-portal], klickar du på **verktyg** > **tillägg**.
   
    ![Composer tilläggsinställningar][composer-extension-settings]
2. Klicka på **Lägg till**, klicka på **Composer**.
   
    ![Lägg till tillägget Composer][composer-extension-add]
3. Klicka på **OK** att acceptera villkoren. Klicka på **OK** igen för att lägga till filnamnstillägget.
   
    Den **installerat tillägg** bladet visas nu tillägget Composer.  
    ![Composer tillägg View][composer-extension-view]
4. Nu kan utföra `git add`, `git commit`, och `git push` precis som i föregående avsnitt. Du ser nu att installeras Composer beroenden som definierats i composer.json.
   
    ![Composer tillägget lyckades][composer-extension-success]

## <a name="next-steps"></a>Nästa steg
Mer information finns i [PHP Developer Center](/develop/php/).

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
