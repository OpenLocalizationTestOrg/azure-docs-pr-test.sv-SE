---
title: Omvandla WordPress till Multisite i Azure App Service
description: "Lär dig hur du utför en befintlig WordPress-webbapp som skapats via galleriet i Azure och konvertera det till WordPress Multisite"
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: fe52dbf4-179c-42f1-adf9-d6a9af920c39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 4a15fb5e97d2ca57e5883c07651c372c54021c92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>Omvandla WordPress till Multisite i Azure App Service
## <a name="overview"></a>Översikt
*Av [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies, Inc.][ms-open-tech]*

I kursen får lära du dig att ta en befintlig WordPress-webbapp som skapats via galleriet i Azure och omvandla dem till en WordPress Multisite-installation. Dessutom du lära dig hur du tilldelar en anpassad domän till var och en av underplatser i din installation.

Det förutsätts att du har en befintlig installation av WordPress. Om du inte följer du riktlinjerna i [skapa en WordPress-webbplats från galleriet i Azure][website-from-gallery].

Konvertera en befintlig WordPress plats installera Multisite är vanligtvis ganska enkel och många av de första stegen kommer direkt från den [skapa ett nätverk] [ wordpress-codex-create-a-network] sida på den [ WordPress Codex](http://codex.wordpress.org).

Nu sätter vi igång.

## <a name="allow-multisite"></a>Tillåt Multisite
Du måste först aktivera Multisite via den `wp-config.php` filen med den **WP\_TILLÅT\_MULTISITE** konstant. Det finns två metoder för att redigera filerna web app: först är via FTP- och andra via Git. Om du inte känner till hur du ställer in någon av följande metoder, finns följande kurser:

* [PHP-webbplats med MySQL och FTP][website-w-mysql-and-ftp-ftp-setup]
* [PHP-webbplats med MySQL och Git][website-w-mysql-and-git-git-setup]

Öppna den `wp-config.php` med redigeraren väljer och Lägg till följande ovan den `/* That's all, stop editing! Happy blogging. */` rad.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Glöm inte att spara filen och överföra den till servern!

## <a name="network-setup"></a>Nätverkskonfiguration
Logga in på den *wp-admin* del av ditt webbprogram och du bör se ett nytt objekt under den **verktyg** menyn kallas **nätverkskonfiguration**. Klicka på **nätverkskonfiguration** och Fyll i informationen för ditt nätverk.

![Installationsprogram för nätverk][wordpress-network-setup]

Den här kursen använder den *underkataloger* plats schemat eftersom den alltid ska fungera, och vi vill ställa in anpassade domäner för varje underwebbplats senare under kursen. Det bör dock vara möjligt att installera en underdomän om du mappar en domän via den [Azure Portal](https://portal.azure.com) och konfigurera jokertecken DNS korrekt.

Mer information om underordnade domän vs underkatalog inställningar finns i [typer av nätverk på flera platser] [ wordpress-codex-types-of-networks] artikel på WordPress Codex.

## <a name="enable-the-network"></a>Aktivera nätverket
Nätverket är nu konfigurerad i databasen, men det finns ett återstående steg för att aktivera funktionen för nätverket. Slutför den `wp-config.php` inställningar och kontrollera `web.config` korrekt dirigerar varje plats.

När du klickar på den **installera** knappen på den *nätverkskonfiguration* sidan WordPress görs ett försök att uppdatera den `wp-config.php` och `web.config` filer. Du bör alltid kontrollera filerna för att se till att uppdateringarna har genomförts. Om inte den här skärmen visas nödvändiga uppdateringar. Redigera och spara filerna.

När du gör tillbaka dessa uppdateringar behöver du logga ut och logga till wp-admin-instrumentpanelen.

Nu ska det finnas en ytterligare meny på admin-fält med namnet **Mina webbplatser**. Den här menyn kan du kontrollera det nya nätverket via den **nätverk Admin** instrumentpanelen.

## <a name="adding-custom-domains"></a>Lägga till anpassade domäner
Den [WordPress MU domän mappning] [ wordpress-plugin-wordpress-mu-domain-mapping] plugin-programmet blir det enkelt att lägga till anpassade domäner till en plats i nätverket. För plugin-programmet ska fungera korrekt måste du göra vissa ytterligare inställningar på portalen och hos din domänregistrator.

## <a name="enable-domain-mapping-to-the-web-app"></a>Aktivera domain mappning till webbappen
Den **lediga** [Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) plan läge inte stöder att lägga till anpassade domäner i Web Apps. Du måste växla till **delade** eller **Standard** läge. Gör så här:

* Logga in på Azure Portal och leta upp ditt webbprogram. 
* Klicka på den **skala upp** fliken i **inställningar**.
* Under **allmänna**, väljer du antingen *delade* eller *STANDARD*
* Klicka på **spara**

Du får ett meddelande där du ombeds bekräfta ändringen och bekräfta ditt webbprogram nu kan innebära en kostnad, beroende på användning och den konfiguration som du anger.

Det tar några sekunder att bearbeta de nya inställningarna nu är dags att börja konfigurera din domän.

## <a name="verify-your-domain"></a>Verifiera din domän
Innan Azure Web Apps kan du mappa en domän till platsen, måste du först kontrollera att du har tillstånd att mappa till domänen. Om du vill göra det, måste du lägga till en ny CNAME DNS-post.

* Logga in på din domän DNS-hanteraren
* Skapa ett nytt CNAME *awverify*
* Punkt *awverify* till *awverify. YOUR_DOMAIN.azurewebsites.NET*

Det kan ta lite tid för att DNS-ändringarna ska börja gälla fullständig, så om följande inte fungerar omedelbart, gå en Kaffekopp, och sedan gå tillbaka och försök igen.

## <a name="add-the-domain-to-the-web-app"></a>Lägg till domänen till webbappen
Gå tillbaka till ditt webbprogram via Azure-portalen klickar du på **inställningar**, och klicka sedan på **anpassade domäner och SSL**.

När den *SSL-inställningar* är visas, visas fält där du ska ange alla domäner som du vill tilldela till ditt webbprogram. Om en domän inte visas här, blir inte tillgängliga för mappning i WordPress, oavsett hur domänen DNS har konfigurerats.

![Hantera dialogrutan för anpassade domäner][wordpress-manage-domains]

När du har skrivit din domän i textrutan, kommer Azure verifiera CNAME-post som du skapade tidigare. Om DNS-servern inte har fullständigt sprids, visas en röd indikator. Om den lyckades visas en grön bockmarkering. 

Anteckna den IP-adress som anges längst ned i dialogrutan. Du behöver detta för att konfigurera A-posten för din domän.

## <a name="setup-the-domain-a-record"></a>Konfigurera en post för domänen
Om andra steg har genomförts, kan du nu tilldela domänen i Azure-webbappen via en DNS A-post. 

Det är viktigt att Observera att Azure-webbappar accepterar CNAME- och A-poster, men du *måste* Använd en A-post för att aktivera domänmappning av rätt. En CNAME-post kan inte vidarebefordras till en annan CNAME, vilket är vad Azure skapas automatiskt med YOUR_DOMAIN.azurewebsites.net.

Med IP-adress från föregående steg, gå tillbaka till din DNS-hanteraren och Ställ in A-post som pekar på den IP.

## <a name="install-and-setup-the-plugin"></a>Installera och konfigurera plugin-programmet
WordPress Multisite har för närvarande inte en inbyggd metod för att mappa anpassade domäner. Det finns emellertid ett plugin-program som kallas [WordPress MU domän mappning] [ wordpress-plugin-wordpress-mu-domain-mapping] som lägger till funktionen för dig. Logga in på nätverket Admin-delen av din webbplats och installera den **WordPress MU domän mappning** plugin-programmet.

När du installerar och aktiverar plugin-programmet kan du besöka **inställningar** > **domän mappning** så här konfigurerar du plugin-programmet. I textrutan första *serverns IP-adress*, ange IP-adressen som du använde för att konfigurera en A-post för domänen. Ange något *Domänalternativen* du önskan (standardvärdena är ofta bra) och klicka på **spara**.

## <a name="map-the-domain"></a>Mappa domänen
Besök den **instrumentpanelen** för den plats som du vill mappa en domän att. Klicka på **verktyg** > **domän mappning** och skriver den nya domänen i textrutan och klicka på **Lägg till**.

Som standard kommer den nya domänen skrivas till domänen automatiskt genererade plats. Om du vill att all trafik som skickas till den nya domänen, kontrollera den *primär domän för den här bloggen* rutan innan du sparar. Du kan lägga till ett obegränsat antal domäner till en plats, men endast en kan vara primär.

## <a name="do-it-again"></a>Göra det igen
Azure Web Apps kan du lägga till ett obegränsat antal domäner till en webbapp. Lägga till en annan domän måste du köra den **verifiera din domän** och **konfigurera domänen en post** avsnitt för varje domän.    

> [!NOTE]
> Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png


