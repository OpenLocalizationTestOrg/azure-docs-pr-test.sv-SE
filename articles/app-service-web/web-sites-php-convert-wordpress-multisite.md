---
title: aaaConvert WordPress tooMultisite i Azure App Service
description: "Lär dig hur tootake en befintlig WordPress-webbapp skapats via hello galleri i Azure och konvertera det tooWordPress Multisite"
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
ms.openlocfilehash: 1153f0a8043de875f081704cd0a124776758878c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="convert-wordpress-toomultisite-in-azure-app-service"></a>Konvertera WordPress tooMultisite i Azure App Service
## <a name="overview"></a>Översikt
*Av [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies, Inc.][ms-open-tech]*

I kursen får du lära dig hur tootake en befintlig WordPress-webbapp skapats via hello galleri i Azure och konvertera den till en WordPress-Multisite installeras. Dessutom får du lära dig hur tooassign en anpassad domän tooeach av hello underwebbplatser inom din installation.

Det förutsätts att du har en befintlig installation av WordPress. Om du inte följer du riktlinjerna i hello [skapa en WordPress-webbplats från hello galleriet i Azure][website-from-gallery].

Konvertera en befintlig WordPress tooMultisite för installation av enskild plats är vanligtvis ganska enkel och många hello första stegen här kommer direkt från hello [skapa ett nätverk] [ wordpress-codex-create-a-network] sida på hello [WordPress Codex](http://codex.wordpress.org).

Nu sätter vi igång.

## <a name="allow-multisite"></a>Tillåt Multisite
Du måste först tooenable Multisite via hello `wp-config.php` fil med hello **WP\_TILLÅT\_MULTISITE** konstant. Det finns två metoder tooedit web appfilerna: hello först är via FTP- och hello andra via Git. Om du inte känner till hur toosetup någon av dessa metoder finns toohello följande kurser:

* [PHP-webbplats med MySQL och FTP][website-w-mysql-and-ftp-ftp-setup]
* [PHP-webbplats med MySQL och Git][website-w-mysql-and-git-git-setup]

Öppna hello `wp-config.php` med hello redigeringsprogram väljer och Lägg till följande hello ovan hello `/* That's all, stop editing! Happy blogging. */` rad.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Vara säker på att toosave hello-fil och överför den bakre toohello servern!

## <a name="network-setup"></a>Nätverkskonfiguration
Logga in toohello *wp-admin* del av ditt webbprogram och du bör se ett nytt objekt under hello **verktyg** menyn kallas **nätverkskonfiguration**. Klicka på **nätverkskonfiguration** och Fyll i hello information om nätverket.

![Installationsprogram för nätverk][wordpress-network-setup]

Den här kursen använder hello *underkataloger* plats schemat eftersom den alltid ska fungera, och vi vill ställa in anpassade domäner för varje underwebbplats senare i självstudiekursen hello. Det bör dock vara möjligt toosetup en underdomän installera om du mappa en domän via hello [Azure Portal](https://portal.azure.com) och konfigurera jokertecken DNS korrekt.

Mer information om underordnade domän vs underkatalog inställningar finns hello [typer av nätverk på flera platser] [ wordpress-codex-types-of-networks] artikel på hello WordPress Codex.

## <a name="enable-hello-network"></a>Aktivera hello nätverk
hello nätverk har konfigurerats i hello-databasen, men det finns en återstående steg tooenable hello nätverksfunktioner. Slutför hello `wp-config.php` inställningar och kontrollera `web.config` korrekt dirigerar varje plats.

När du klickar på hello **installera** hello-knappen *nätverkskonfiguration* sidan WordPress försöker tooupdate hello `wp-config.php` och `web.config` filer. Dock bör du alltid kontrollera hello filer tooensure hello lyckades. Om inte den här skärmen visas hello nödvändiga uppdateringar. Redigera och spara hello-filer.

När du gör tillbaka dessa uppdateringar behöver du toolog ut och logga till hello wp-admin-instrumentpanelen.

Nu ska det finnas en ytterligare meny på hello admin fält med namnet **Mina webbplatser**. Den här menyn kan du toocontrol ditt nya nätverk via hello **nätverk Admin** instrumentpanelen.

## <a name="adding-custom-domains"></a>Lägga till anpassade domäner
Hej [WordPress MU domän mappning] [ wordpress-plugin-wordpress-mu-domain-mapping] plugin-program gör den till en snabbt tooadd anpassade domäner tooany plats i nätverket. För att hello plugin-programmet toooperate korrekt, måste toodo vissa ytterligare inställningar på hello Portal och också hos din domänregistrator.

## <a name="enable-domain-mapping-toohello-web-app"></a>Aktivera domain mappning toohello webbapp
Hej **lediga** [Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) plan läge inte stöder att lägga till anpassade domäner tooWeb appar. Du behöver tooswitch för**delade** eller **Standard** läge. toodo detta:

* Logga in toohello Azure-portalen och leta upp ditt webbprogram. 
* Klicka på hello **skala upp** fliken i **inställningar**.
* Under **allmänna**, väljer du antingen *delade* eller *STANDARD*
* Klicka på **spara**

Du kan ta emot ett meddelande som ber tooverify hello ändringen och bekräfta ditt webbprogram nu kan innebära en kostnad, beroende på användning och hello andra konfigurationsuppsättning.

Det tar några sekunder tooprocess hello nya inställningarna, så nu är ett bra tillfälle toostart ställa in din domän.

## <a name="verify-your-domain"></a>Verifiera din domän
Innan Azure Web Apps låter toomap en domän toohello plats, måste du först tooverify att du har hello auktorisering toomap hello domän. toodo så måste du lägga till en ny CNAME post tooyour DNS-post.

* Logga in tooyour domänens DNS-hanteraren
* Skapa ett nytt CNAME *awverify*
* Punkt *awverify* för*awverify. YOUR_DOMAIN.azurewebsites.NET*

Det kan ta lite tid innan hello DNS-ändringarna toogo till full effekt, så om hello följa stegen inte fungerar omedelbart, gå en Kaffekopp, och sedan gå tillbaka och försök igen.

## <a name="add-hello-domain-toohello-web-app"></a>Lägg till hello domän toohello webbapp
Returnerar tooyour webbprogram via hello Azure-portalen klickar du på **inställningar**, och klicka sedan på **anpassade domäner och SSL**.

När hello *SSL-inställningar* är visas, visas hello fält där du ska ange alla hello-domäner som du önskar tooassign tooyour webbprogram. Om en domän inte visas här, blir inte tillgängliga för mappning i WordPress, oavsett hur hello domain DNS har konfigurerats.

![Hantera dialogrutan för anpassade domäner][wordpress-manage-domains]

När du har skrivit din domän i textrutan för hello verifierar Azure hello CNAME-post som du skapade tidigare. Om hello DNS inte har fullständigt sprids, visas en röd indikator. Om den lyckades visas en grön bockmarkering. 

Anteckna hello IP-adress som anges längst ned hello hello dialogrutan. Du behöver den här toosetup hello en post för din domän.

## <a name="setup-hello-domain-a-record"></a>Konfigurera hello domän A-post
Om hello andra steg har genomförts, kan du nu tilldela hello domän tooyour Azure-webbapp via en DNS A-post. 

Det är viktigt toonote här som Azure-webbappar accepterar CNAME- och A-poster, men du *måste* använda mappningen för en A-poster tooenable rätt domän. En CNAME-post kan inte vidarebefordras tooanother CNAME, vilket är vad Azure skapas automatiskt med YOUR_DOMAIN.azurewebsites.net.

Med hello IP-adress från föregående steg i hello returnera tooyour DNS-hanteraren och installationsprogrammet hello en post toopoint toothat IP-adress.

## <a name="install-and-setup-hello-plugin"></a>Installera och konfigurera hello plugin-program
WordPress Multisite har för närvarande inte en inbyggd metod toomap anpassade domäner. Det finns emellertid ett plugin-program som kallas [WordPress MU domän mappning] [ wordpress-plugin-wordpress-mu-domain-mapping] som lägger till hello funktioner du. Logga in toohello nätverk Admin del av din webbplats och installera hello **WordPress MU domän mappning** plugin-programmet.

Efter installation och aktivering av hello plugin finns **inställningar** > **domän mappning** tooconfigure hello plugin-programmet. I hello första textruta *serverns IP-adress*, inkommande hello IP-adress som du använde toosetup hello en post för hello domän. Ange något *Domänalternativen* du önskar (hello standard är ofta bra) och klicka på **spara**.

## <a name="map-hello-domain"></a>Mappa hello domän
Besök hello **instrumentpanelen** för hello plats du vill toomap hello domän till. Klicka på **verktyg** > **domän mappning** och typen hello ny domän i hello textrutan och klicka på **Lägg till**.

Som standard ska hello ny domän vara omskrivet toohello automatiskt genererade plats. Om du vill toohave all trafik som skickas toohello ny domän, kontrollera hello *primär domän för den här bloggen* rutan innan du sparar. Du kan lägga till ett obegränsat antal domäner tooa plats, men endast en kan vara primär.

## <a name="do-it-again"></a>Göra det igen
Azure Web Apps kan du tooadd ett obegränsat antal domäner tooa webbprogram. tooadd en annan domän som du behöver tooexecute hello **verifiera din domän** och **installationsprogrammet hello domän A-post** avsnitt för varje domän.    

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

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


