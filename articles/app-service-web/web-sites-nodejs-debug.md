---
title: "Felsöka en Node.js-webbapp i Azure Apptjänst"
description: "Lär dig att felsöka en Node.js-webbapp i Azure App Service."
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
ms.openlocfilehash: 5e302a4c58a171d40e43a22c34c724e868019ec8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>Felsöka en Node.js-webbapp i Azure Apptjänst
Azure tillhandahåller inbyggda diagnostik för att underlätta felsökning Node.js-program finns i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps. I den här artikeln får du lära dig hur visas information om fel i webbläsaren om du vill aktivera loggning av stdout och stderr och hur du hämtar och visa loggfiler.

Diagnostik för Node.js-program i Azure tillhandahålls av [IISNode]. Den här artikeln beskrivs de vanligaste inställningarna för att samla in diagnostikinformation, innehåller det inte en fullständig referens för att arbeta med IISNode. Mer information om hur du arbetar med IISNode finns i [IISNode Readme] på GitHub.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Aktivera loggning
Som standard sparas endast en App Service webbapp diagnostisk information om distributioner, till exempel när du distribuerar ett webbprogram med Git. Den här informationen är användbar om du har problem under distributionen, till exempel ett fel när du installerar en modul som refereras i **package.json**, eller om du använder en anpassad distribution med skriptet.

Om du vill aktivera loggning av stdout och stderr-strömmar, måste du skapa en **IISNode.yml** filen i roten på Node.js-programmet och Lägg till följande:

    loggingEnabled: true

Detta gör att loggning av stderr och stdout från Node.js-programmet.

Den **IISNode.yml** filen kan också användas för kontrollen om egna felmeddelanden eller utvecklare fel skickas till webbläsaren när ett fel uppstår. Om du vill aktivera developer fel, lägger du till följande rad i den **IISNode.yml** fil:

    devErrorsEnabled: true

När detta alternativ är aktiverat returnerar IISNode informationen som skickas till stderr i stället för ett eget fel som ”ett internt serverfel uppstod” senaste 64K.

> [!NOTE]
> DevErrorsEnabled är användbart när diagnostisera problem under utveckling, kan aktivera den i en produktionsmiljö leda till utveckling fel skickas till slutanvändare.
> 
> 

Om den **IISNode.yml** filen redan finns inte i ditt program, måste du starta om ditt webbprogram när du har publicerat programmet uppdaterade. Om du bara ändrar inställningarna i en befintlig **IISNode.yml** -fil som tidigare har publicerats, ingen omstart krävs.

> [!NOTE]
> Om ditt webbprogram har skapats med hjälp av Azure-kommandoradsverktyg eller Azure PowerShell-Cmdlets, standard **IISNode.yml** fil skapas automatiskt.
> 
> 

Om du vill starta om webbprogrammet, Välj webbappen i den [Azure Portal](https://portal.azure.com), och klicka sedan på **starta om** knappen:

![Starta om knappen][restart-button]

Om Azure-kommandoradsverktyg är installerade i din utvecklingsmiljö använda du följande kommando för att starta om webbprogrammet:

    azure site restart [sitename]

> [!NOTE]
> LoggingEnabled och devErrorsEnabled är de vanligaste IISNode.yml konfigurationsalternativ för att samla in diagnostikinformation, användas IISNode.yml för att konfigurera en mängd olika alternativ för din värdmiljö. En fullständig lista över konfigurationsalternativ finns i [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) fil.
> 
> 

<a id="viewlogs"></a>

## <a name="accessing-logs"></a>Åtkomst till loggar
Diagnostikloggar kan nås på tre sätt; Med den protokollet FTP (File Transfer), hämtar ett Zip-arkiv eller som en live uppdateras dataströmmen loggens (även kallat pilslut). Hämta Zip-arkivet loggfiler eller visa den direktsända dataströmmen kräver Azure-kommandoradsverktyg. Dessa kan installeras med hjälp av följande kommando:

    npm install azure-cli -g

När du har installerat kan verktygen nås med hjälp av kommandot ”azure”. Kommandoradsverktygen måste först konfigureras för att använda din Azure-prenumeration. Information om hur du gör detta finns i **hur du laddar ned och importera Publiceringsinställningar** avsnitt i den [så används den Azure-kommandoradsverktyg](../xplat-cli-connect.md) artikel.

### <a name="ftp"></a>FTP
Om du vill komma åt diagnostisk information via FTP finns i [Azure-portalen](https://portal.azure.com), Välj ditt webbprogram och välj sedan den **INSTRUMENTPANELEN**. I den **snabblänkar** avsnittet den **FTP DIAGNOSTIKLOGGAR** och **FTPS DIAGNOSTIKLOGGAR** länkar ger åtkomst till loggar med hjälp av FTP-protokollet.

> [!NOTE]
> Om du inte tidigare har konfigurerat användarnamn och lösenord för FTP- eller distributionen, kan du göra det från den **Quickstart** sidan för hantering genom att välja **konfigurerat distributionsuppgifter**.
> 
> 

FTP-URL som returneras i instrumentpanelen avser den **loggfiler** katalog som innehåller följande underordnade kataloger:

* [Distributionsmetoden](web-sites-deploy.md) -om du använder en distributionsmetod som till exempel Git en katalog med samma namn skapas och innehåller information relaterad till distributioner.
* nodejs - Stdout och stderr information som hämtats från alla instanser av programmet (när loggingEnabled är true.)

### <a name="zip-archive"></a>ZIP-Arkiv
Använd följande kommando från Azure-kommandoradsverktyg för att hämta ett Zip-arkiv diagnostiska loggar:

    azure site log download [sitename]

Detta kommer att ladda ned en **diagnostics.zip** i den aktuella katalogen. Det här arkivet innehåller följande katalogstruktur:

* distributioner – en logg över information om distribution av programmet
* Loggfiler
  
  * [Distributionsmetoden](web-sites-deploy.md) -om du använder en distributionsmetod som till exempel Git en katalog med samma namn skapas och innehåller information relaterad till distributioner.
  * nodejs - Stdout och stderr information som hämtats från alla instanser av programmet (när loggingEnabled är true.)

### <a name="live-stream-tail"></a>Direktsänd dataström (pilslut)
Använd följande kommando från Azure-kommandoradsverktyg för att visa en direktsänd dataström med diagnostisk logginformation:

    azure site log tail [sitename]

Detta returnerar en dataström med logghändelser som uppdateras när de inträffar på servern. Den här dataströmmen returnerar information om programdistribution samt stdout och stderr (när loggingEnabled är true.)

<a id="nextsteps"></a>

## <a name="next-steps"></a>Nästa steg
I den här artikeln beskrivs hur du aktiverar och komma åt diagnostikinformationen för Azure. Den här informationen är användbar i Förstå problem med ditt program, kan den peka på ett problem med en modul som du använder eller som versionen av Node.js som används av App Service Web Apps är annorlunda än den som används i din distributionsmiljö.

Information arbetar tillsammans med moduler i Azure finns [med Node.js-moduler med Azure-program](../nodejs-use-node-modules-azure-apps.md).

Information om hur du anger en Node.js-version för ditt program finns i [ange en Node.js-version i ett Azure-program].

Mer information finns också i [Node.js Developer Center](/develop/nodejs/).

## <a name="whats-changed"></a>Nyheter
* En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

[IISNode]: https://github.com/tjanczuk/iisnode
[IISNode Readme]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]:../cli-install-nodejs.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[ange en Node.js-version i ett Azure-program]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png

