---
title: "aaaOpen-teknikerna för vanliga frågor och svar för Azure-webbappar | Microsoft Docs"
description: "Få svar toofrequently frågor och svar om öppen källkod tekniker i hello Web Apps-funktionen i Azure App Service."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 35cff4f322859d25972747cf55aa7c4316381a51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="open-source-technologies-faqs-for-web-apps-in-azure"></a>Öppen källkod tekniker vanliga frågor och svar för Web Apps i Azure

Den här artikeln har svaren toofrequently frågor (FAQ) och om kända problem med öppen källkod tekniker för hello [funktionen Web Apps i Azure App Service](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-cleardb-database-is-down-how-do-i-resolve-this"></a>ClearDB databasen är inte tillgänglig. Hur lösa problemet?

Problem med databasen Kontakta [ClearDB support](https://www.cleardb.com/developers/help/support). 

Läs svaren toocommon frågor om ClearDB, [ClearDB vanliga frågor och svar](https://docs.microsoft.com/azure/store-cleardb-faq/).

## <a name="why-isnt-my-cleardb-database-listed-in-hello-portal"></a>Varför är min ClearDB databas listad i hello portal?

Om du skapar en ClearDB-databas i hello [Azure-portalen](http://portal.azure.com/), hello databasen inte visas i hello [klassiska Azure-portalen](http://manage.windowsazure.com/). toowork runt problemet kan du manuellt koppla databasen toohello webbappen.

På samma sätt om du skapar en ClearDB-databas i hello [klassiska Azure-portalen](http://manage.windowsazure.com/), visas inte databasen i hello [Azure-portalen](http://portal.azure.com/). I detta fall finns ingen lösning. 

Mer information finns i [vanliga frågor och svar för ClearDB MySQL-databaser med Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).

## <a name="why-wasnt-my-cleardb-database-migrated-during-my-subscription-migration"></a>Varför inte ClearDB databasen migreras under prenumerationsmigreringen min?

Vissa begränsningar gäller när du utför resursmigrering över prenumerationer. En ClearDB MySQL-databas är en tjänst från tredje part och migreras inte under migreringen för en Azure-prenumeration.

Om du inte hanterar hello migrering av MySQL-databasen innan du migrerar resurserna i Azure, kan ClearDB MySQL-databasen vara otillgänglig. tooavoid detta först migrerar ClearDB databasen manuellt och sedan migrera hello Azure-prenumeration för din webbapp.

Mer information finns i [vanliga frågor och svar för ClearDB MySQL-databaser med Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).

## <a name="how-do-i-turn-on-php-logging-tootroubleshoot-php-issues"></a>Hur aktiverar jag PHP loggning tootroubleshoot PHP problem?

tooturn PHP loggning:

1. Logga in tooyour [Kudu webbplats](https://*yourwebsitename*.scm.azurewebsites.net).
2. Välj hello översta menyn **Felsökningskonsolen** > **CMD**.
3. Välj hello **plats** mapp.
4. Välj hello **wwwroot** mapp.
5. Välj hello  **+**  ikon och väljer sedan **ny fil**.
6. Ange hello filnamn för**. user.ini**.
7. Välj hello pennikonen bredvid för**. user.ini**.
8. Lägg till den här koden i hello-filen:`log_errors=on`
9. Välj **Spara**.
10. Välj hello pennikonen bredvid för**wp config.php**.
11. Ändra hello text toohello följande kod:
   ```
   //Enable WP_DEBUG modedefine('WP_DEBUG', true);//Enable debug logging too/wp-content/debug.logdefine('WP_DEBUG_LOG', true);
   //Supress errors and warnings tooscreendefine('WP_DEBUG_DISPLAY', false);//Supress PHP errors tooscreenini_set('display_errors', 0);
   ```
12. Starta om ditt webbprogram i hello Azure-portalen hello web app-menyn.

Mer information finns i [aktivera WordPress felloggar](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

## <a name="how-do-i-log-python-application-errors-in-apps-that-are-hosted-in-app-service"></a>Hur loggar Python-programfel i appar som finns i App Service?

toocapture Python-programfel:

1. Välj i hello Azure-portalen i ditt webbprogram **inställningar**.
2. På hello **inställningar** väljer **programinställningar**.
3. Under **appinställningar**, anger du följande nyckel/värde-par hello:
    * Nyckel: WSGI_LOG
    * Värde: D:\home\site\wwwroot\logs.txt (Ange önskat filnamn)

Du bör nu se felen i hello logs.txt fil i mappen för hello wwwroot.

## <a name="how-do-i-change-hello-version-of-hello-nodejs-application-that-is-hosted-in-app-service"></a>Hur ändrar jag hello version av hello Node.js-program som finns i App Service?

toochange hello versionen av hello Node.js-program kan du använda en hello följande alternativ:

*   I hello Azure-portalen, använda **appinställningar**.
    1. Gå tooyour webbprogram i hello Azure-portalen.
    2. På hello **inställningar** bladet väljer **programinställningar**.
    3. I **appinställningar**, du kan inkludera WEBSITE_NODE_DEFAULT_VERSION som hello nyckel och hello Node.js-version som ska vara hello värde.
    4. Gå tooyour [Kudu konsolen](https://*yourwebsitename*.scm.azurewebsites.net).
    5. toocheck Hej Node.js-version, ange hello följande kommando:  
   ```
   node -v
   ```
*   Ändra hello iisnode.yml filen. Ändra hello Node.js-version i hello iisnode.yml filen anger hello körningsmiljö där iisnode används. Kudu-cmd och andra fortfarande använda hello Node.js-version som har angetts i **appinställningar** i hello Azure-portalen.

    tooset hello iisnode.yml manuellt skapa en iisnode.yml-fil i rotmappen för din app. Inkludera hello följande rad i hello-filen:
   ```
   nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
   ```
   
*   Ange hello iisnode.yml filen med package.json under distributionen av käll-kontroll.
    hello Azure källa kontrollen distributionsprocessen omfattar hello följande steg:
    1. Flyttar innehåll toohello Azure webbapp.
    2. Skapar en standard-distributionsskriptet, om det inte finns något (deploy.cmd, .deployment-filer) i rotmappen för hello web app.
    3. Kör ett skript för distribution där den skapar en fil i iisnode.yml om du nämna hello Node.js-version i hello package.json fil > motorn`"engines": {"node": "5.9.1","npm": "3.7.3"}`
    4. Hej iisnode.yml fil har följande kodrad hello:
        ```
        nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
        ```

## <a name="i-see-hello-message-error-establishing-a-database-connection-in-my-wordpress-app-thats-hosted-in-app-service-how-do-i-troubleshoot-this"></a>Hello-meddelande ”Error upprätta en databasanslutning” visas i min WordPress-appen i App Service. Hur felsöker jag?

Om du ser felet i Azure WordPress-appen, tooenable php_errors.log och debug.log fullständig hello stegen som beskrivs i [aktivera WordPress felloggar](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

När hello loggar är aktiverade, återskapa hello fel och kontrollera sedan hello loggar toosee om du börjar få slut på anslutningar:
```
[09-Oct-2015 00:03:13 UTC] PHP Warning: mysqli_real_connect(): (HY000/1226): User ‘abcdefghijk79' has exceeded hello ‘max_user_connections’ resource (current value: 4) in D:\home\site\wwwroot\wp-includes\wp-db.php on line 1454
```

Om du ser felet i filerna debug.log eller php_errors.log överstiger appen hello antal anslutningar. Om du är värd för ClearDB Kontrollera hello antalet anslutningar som är tillgängliga i din [serviceplan](https://www.cleardb.com/pricing.view).

## <a name="how-do-i-debug-a-nodejs-app-thats-hosted-in-app-service"></a>Hur jag för att felsöka en Node.js-app i App Service?

1.  Gå tooyour [Kudu konsolen](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).
2.  Gå tooyour loggar programmappen (D:\home\LogFiles\Application).
3.  Kontrollera innehållet i hello logging_errors.txt-filen.

## <a name="how-do-i-install-native-python-modules-in-an-app-service-web-app-or-api-app"></a>Hur installera ursprungliga Python-moduler i en App Service webbapp eller API-app?

Vissa paket kan inte installeras med pip i Azure. hello paketet kanske inte är tillgänglig på hello Python Package Index eller kan krävas en kompilator (en kompilator är inte tillgängliga på hello-dator som kör hello-webbapp i App Service). Information om hur du installerar inbyggda moduler i App Service web apps och API apps finns i [installera Python-moduler i App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).

## <a name="how-do-i-deploy-a-django-app-tooapp-service-by-using-git-and-hello-new-version-of-python"></a>Hur distribuerar tooApp en Django-app Service med hjälp av Git och hello ny version av Python?

Information om hur du installerar Django finns [distribuera en tjänst med Django app tooApp](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).

## <a name="where-are-hello-tomcat-log-files-located"></a>Var finns hello Tomcat loggfiler finns?

För Azure Marketplace och anpassade distributioner:

* Mapp: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs
* Filer av intresse:
    * catalina. *åååå-mm-dd*.log
    * värd-hanteraren. *åååå-mm-dd*.log
    * localhost. *åååå-mm-dd*.log
    * Manager. *åååå-mm-dd*.log
    * site_access_log. *åååå-mm-dd*.log


För portalen **appinställningar** distributioner:

* Mapp: D:\home\LogFiles
* Filer av intresse:
    * catalina. *åååå-mm-dd*.log
    * värd-hanteraren. *åååå-mm-dd*.log
    * localhost. *åååå-mm-dd*.log
    * Manager. *åååå-mm-dd*.log
    * site_access_log. *åååå-mm-dd*.log

## <a name="how-do-i-troubleshoot-jdbc-driver-connection-errors"></a>Hur felsöker jag JDBC driver-anslutningsfel

Du kan se hello efter meddelande i Tomcat-loggar:

```
hello web application[ROOT] registered hello JDBC driver [com.mysql.jdbc.Driver] but failed toounregister it when hello web application was stopped. tooprevent a memory leak,hello JDBC Driver has been forcibly unregistered
```

tooresolve hello-fel:

1. Ta bort hello sqljdbc*.jar fil från din app/lib mapp.
2. Om du använder hello anpassade Tomcat eller Azure Marketplace Tomcat webbserver, kopierar du .jar filen toohello Tomcat lib mappen.
3. Om du aktiverar Java från hello Azure-portalen (Välj **Java 1.8** > **Tomcat server**), kopiera hello sqljdbc.* jar-filen i hello-mapp som är parallella tooyour app. Lägg sedan till hello följande klassökvägen inställningen toohello web.config-filen:

    ```
    <httpPlatform>
    <environmentVariables>
    <environmentVariablename ="JAVA_OPTS" value=" -Djava.net.preferIPv4Stack=true
    -Xms128M -classpath %CLASSPATH%;[Path toohello sqljdbc*.jarfile]" />
    </environmentVariables>
    </httpPlatform>
    ```

## <a name="why-do-i-see-errors-when-i-attempt-toocopy-live-log-files"></a>Varför visas fel när jag försöker toocopy live loggfiler?

Om du försöker toocopy live loggfiler för Java-app (t.ex, Tomcat), kan du se den här FTP-fel:

```
Error transferring file [filename] Copying files from remote side failed.
    
hello process cannot access hello file because it is being used by another process.
```

hello felmeddelandet kan variera beroende på hello FTP-klienten.

Alla Java-appar har problemet låsning. Endast Kudu stöder hämta filen medan hello appen körs.

Stoppa hello app åtkomst FTP toothese filer.

En annan lösning är toowrite ett Webbjobb som körs enligt ett schema och kopierar dessa filer tooa annan katalog. En exempelprojektet finns hello [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) projekt.

## <a name="where-do-i-find-hello-log-files-for-jetty"></a>Var hittar jag hello loggfiler för Jetty

För Marketplace och anpassade distributioner finns hello loggfilen i hello D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs mapp. Observera att mappen hello beroende hello version av Jetty som du använder. Till exempel hello sökväg som angetts här för Jetty 9.1.2. Leta efter jetty_*YYYY_MM_DD*. stderrout.log.

Hello loggfilen är i D:\home\LogFiles för portalen Appinställningen distributioner. Leta efter jetty_*YYYY_MM_DD*. stderrout.log

## <a name="can-i-send-email-from-my-azure-web-app"></a>Kan jag skicka e-post från mitt Azure webbapp?

Apptjänst har inte en inbyggda e-funktion. För några bra alternativ för att skicka e-post från din app se den här [Stack Overflow diskussion](http://stackoverflow.com/questions/17666161/sending-email-from-azure).

## <a name="why-does-my-wordpress-site-redirect-tooanother-url"></a>Varför min WordPress-webbplats URL för omdirigering tooanother?

Om du nyligen har migrerats tooAzure kan WordPress omdirigera toohello gamla domän-URL. Detta orsakas av en inställning i hello MySQL-databas.

WordPress-representant + är ett tillägg för Azure-plats som som du kan använda tooupdate hello omdirigerings-URL direkt i hello-databasen. Mer information om hur du använder WordPress-representant + finns [WordPress verktyg och MySQL-migrering med WordPress-representant +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

Om du föredrar toomanually uppdatering hello omdirigerings-URL genom att använda SQL-frågor eller PHPMyAdmin Se [WordPress: omdirigera URL toowrong](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).

## <a name="how-do-i-change-my-wordpress-sign-in-password"></a>Hur ändrar lösenordet för WordPress?

Om du har glömt lösenordet för WordPress, kan du använda WordPress-representant + tooupdate den. ditt lösenord, installera hello WordPress representant + Azure Webbplatstillägg och sedan utför hello stegen som beskrivs i tooreset [WordPress verktyg och MySQL-migrering med WordPress-representant +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="i-cant-sign-in-toowordpress-how-do-i-resolve-this"></a>Jag kan inte logga in tooWordPress. Hur lösa problemet?

Om du märker att du har låsts ute från WordPress när du har installerat nyligen ett plugin-program kan kanske du en felaktig plugin-programmet. WordPress-representant + är ett tillägg för Azure plats som kan hjälpa dig att inaktivera plugin-program i WordPress. Mer information finns i [WordPress verktyg och MySQL-migrering med WordPress-representant +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="how-do-i-migrate-my-wordpress-database"></a>Hur migrerar WordPress databasen?

Du har flera alternativ för att migrera hello MySQL-databas som är anslutna tooyour WordPress-webbplats:

* Utvecklare: Använd hello [kommandotolk eller PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)
* Icke-utvecklare: Använd [WordPress-representant +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)

## <a name="how-do-i-help-make-wordpress-more-secure"></a>Hur jag gör WordPress säkrare?

toolearn om rekommenderade säkerhetsmetoder för WordPress, se [bästa praxis för WordPress-säkerhet i Azure](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).

## <a name="i-am-trying-toouse-phpmyadmin-and-i-see-hello-message-access-denied-how-do-i-resolve-this"></a>Jag försöker toouse PHPMyAdmin och hello-meddelande ”åtkomst nekad”. Hur lösa problemet?

Det här problemet kan uppstå om hello MySQL i appen funktionen ännu inte körs i den här Apptjänst-instansen. tooresolve hello problemet, försök tooaccess din webbplats. Detta startar hello krävs processer, inklusive hello MySQL app process. tooverify som MySQL i appen körs i processen Explorer, kontrollerar du att mysqld.exe visas i hello processer.

När du har kontrollerat att MySQL app körs försök toouse PHPMyAdmin.

## <a name="i-get-an-http-403-error-when-i-try-tooimport-or-export-my-mysql-in-app-database-by-using-phpmyadmin-how-do-i-resolve-this"></a>Jag får felmeddelandet HTTP 403 när jag försöker tooimport eller exportera min app MySQL-databas med hjälp av PHPMyadmin. Hur lösa problemet?

Om du använder en äldre version av Chrome, kan som uppstå ett känt fel. tooresolve hello problemet, uppgradera tooa nyare version av Chrome. Också försöka med en annan webbläsare som Internet Explorer eller Edge, där hello utan problem.
