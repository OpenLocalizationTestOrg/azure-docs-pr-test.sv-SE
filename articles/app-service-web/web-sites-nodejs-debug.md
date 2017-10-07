---
title: aaaHow toodebug Node.js-webbapp i Azure App Service
description: "Lär dig hur toodebug en Node.js webbapp i Azure App Service."
tags: azure-portal
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: a48f906c-1a3e-43bc-ae84-7d2dde175b15
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 888ec5c3f92cfc3aeea4ea86005b9b6a0d1306ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodebug-a-nodejs-web-app-in-azure-app-service"></a>Hur toodebug en Node.js webbapp i Azure App Service
Azure tillhandahåller inbyggd diagnostik tooassist med felsökning Node.js-program finns i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps. I den här artikeln får du lära dig hur tooenable loggning av stdout och stderr, visa felinformation i hello webbläsare och hur toodownload och visa loggfiler.

Diagnostik för Node.js-program i Azure tillhandahålls av [IISNode]. Den här artikeln beskrivs hello de vanligaste inställningarna för att samla in diagnostikinformation, innehåller det inte en fullständig referens för att arbeta med IISNode. Mer information om hur du arbetar med IISNode finns hello [IISNode Readme] på GitHub.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Aktivera loggning
Som standard sparas endast en App Service webbapp diagnostisk information om distributioner, till exempel när du distribuerar ett webbprogram med Git. Den här informationen är användbar om du har problem under distributionen, till exempel ett fel när du installerar en modul som refereras i **package.json**, eller om du använder en anpassad distribution med skriptet.

tooenable hello loggning av stdout och stderr-strömmar, måste du skapa en **IISNode.yml** hello roten i Node.js-programmet och Lägg till hello följande:

    loggingEnabled: true

Detta gör att hello loggning av stderr och stdout från Node.js-programmet.

Hej **IISNode.yml** filen kan även vara används toocontrol om egna felmeddelanden eller utvecklare fel returneras toohello webbläsare när ett fel inträffar. tooenable developer fel, Lägg till följande rad toohello hello **IISNode.yml** fil:

    devErrorsEnabled: true

När detta alternativ är aktiverat returnerar IISNode hello senaste 64 kB i informationen som skickas toostderr i stället för ett eget fel som ”ett internt serverfel uppstod”.

> [!NOTE]
> DevErrorsEnabled är användbart när diagnostisera problem under utveckling, kan aktiverar du den i en produktionsmiljö leda till utveckling fel skickas tooend användare.
> 
> 

Om hello **IISNode.yml** filen redan finns inte i ditt program, måste du starta om ditt webbprogram när du har publicerat programmet hello uppdateras. Om du bara ändrar inställningarna i en befintlig **IISNode.yml** -fil som tidigare har publicerats, ingen omstart krävs.

> [!NOTE]
> Om ditt webbprogram har skapats med hello Azure-kommandoradsverktyg eller Azure PowerShell-cmdletar, standard **IISNode.yml** fil skapas automatiskt.
> 
> 

toorestart hello webbapp väljer hello webbprogram i hello [Azure Portal](https://portal.azure.com), och klicka sedan på **starta om** knappen:

![Starta om knappen][restart-button]

Kommandoradsverktyg för hello Azure är installerade i din utvecklingsmiljö, kan du använda hello efter kommandot toorestart hello-webbprogram:

    azure site restart [sitename]

> [!NOTE]
> LoggingEnabled och devErrorsEnabled är de vanligaste hello IISNode.yml konfigurationsalternativ för att samla in diagnostikinformation, vara IISNode.yml används tooconfigure en mängd olika alternativ för din värdmiljö. En fullständig lista över konfigurationsalternativ för hello finns hello [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) fil.
> 
> 

<a id="viewlogs"></a>

## <a name="accessing-logs"></a>Åtkomst till loggar
Diagnostikloggar kan nås på tre sätt; Med hjälp av hello protokollet FTP (File Transfer), hämtar ett Zip-arkiv eller som en live uppdateras dataströmmen hello-loggen (även kallat pilslut). Hämta hello Zip-arkiv hello loggfiler eller visa hello direktsänd dataström kräver hello Azure-kommandoradsverktyg. Dessa kan installeras med hjälp av hello följande kommando:

    npm install azure-cli -g

När du har installerat kan hello verktyg kommas åt kommandot hello ”azure”. Hej kommandoradsverktyg måste först vara konfigurerade toouse din Azure-prenumeration. Information om hur tooaccomplish detta uppgift finns hello **hur toodownload och importera Publiceringsinställningar** avsnitt i hello [hur tooUse hello Azure kommandoradsverktyg](../xplat-cli-connect.md) artikel.

### <a name="ftp"></a>FTP
tooaccess hello diagnostisk information via FTP, besök hello [Azure Portal](https://portal.azure.com), Välj ditt webbprogram och väljer sedan hello **INSTRUMENTPANELEN**. I hello **snabblänkar** avsnittet hello **FTP DIAGNOSTIKLOGGAR** och **FTPS DIAGNOSTIKLOGGAR** länkar ger toohello åtkomstloggar med hello FTP-protokollet.

> [!NOTE]
> Om du inte tidigare har konfigurerat användarnamn och lösenord för FTP- eller distributionen, kan du göra det från hello **Quickstart** sidan för hantering genom att välja **konfigurerat distributionsuppgifter**.
> 
> 

hello FTP-URL som returneras i hello instrumentpanelen är för hello **loggfiler** katalog som ska innehålla hello efter underkataloger:

* [Distributionsmetoden](web-sites-deploy.md) -om du använder en distributionsmetod som till exempel Git en katalog med hello samma namn skapas och innehåller information relaterad toodeployments.
* nodejs - Stdout och stderr information som hämtats från alla instanser av programmet (när loggingEnabled är true.)

### <a name="zip-archive"></a>ZIP-Arkiv
toodownload ett Zip-arkiv hello diagnostiska loggar, Använd hello följande kommando från hello Azure kommandoradsverktyg:

    azure site log download [sitename]

Detta kommer att ladda ned en **diagnostics.zip** i hello aktuella katalogen. Det här arkivet innehåller hello följande katalogstruktur:

* distributioner – en logg över information om distribution av programmet
* Loggfiler
  
  * [Distributionsmetoden](web-sites-deploy.md) -om du använder en distributionsmetod som till exempel Git en katalog med hello samma namn skapas och innehåller information relaterad toodeployments.
  * nodejs - Stdout och stderr information som hämtats från alla instanser av programmet (när loggingEnabled är true.)

### <a name="live-stream-tail"></a>Direktsänd dataström (pilslut)
tooview en direktsänd dataström av diagnostiska logginformation, Använd hello följande kommando från hello Azure kommandoradsverktyg:

    azure site log tail [sitename]

Detta returnerar en dataström med logghändelser som uppdateras allteftersom de sker på hello-servern. Den här dataströmmen returnerar information om programdistribution samt stdout och stderr (när loggingEnabled är true.)

<a id="nextsteps"></a>

## <a name="next-steps"></a>Nästa steg
I denna artikel du lärt dig hur tooenable och åtkomst diagnostikinformationen för Azure. När den här informationen är användbar Förstå problem med ditt program, kan den peka tooa problem med en modul som du använder eller som hello-versionen av Node.js som används av App Service Web Apps skiljer sig från hello som används i distributionen miljö.

Information arbetar tillsammans med moduler i Azure finns [med Node.js-moduler med Azure-program](../nodejs-use-node-modules-azure-apps.md).

Information om hur du anger en Node.js-version för ditt program finns i [ange en Node.js-version i ett Azure-program].

Mer information finns också hello [Node.js Developer Center](/develop/nodejs/).

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

[IISNode]: https://github.com/tjanczuk/iisnode
[IISNode Readme]: https://github.com/tjanczuk/iisnode#readme
[How tooUse hello Azure Command-Line Interface]:../cli-install-nodejs.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[ange en Node.js-version i ett Azure-program]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png

