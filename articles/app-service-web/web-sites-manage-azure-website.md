---
title: Hantera en webbapp i Azure App Service
description: "Länkar till resurser för att hantera en webbapp i Azure App Service."
services: app-service\web
documentationcenter: 
author: erikre
manager: erikre
editor: 
ms.assetid: d5e2887a-84f9-4747-a573-867635cb8b39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: rachelap
ms.openlocfilehash: 9e19618a1b24bbdf3163ddfc3423c5c932dcd7af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="manage-a-web-app-in-azure-app-service"></a>Hantera en webbapp i Azure App Service
Det här avsnittet innehåller länkar till resurser för att hantera en webbapp i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Hantering innehåller alla åtgärder som håller din webbapp körs utan problem. 

Över livslängden för en webbapp ska du utföra olika hanteringsuppgifter, när du flyttar från första distributionen till normal drift, underhåll och uppdateringar.

Många aktiviteter för webb-app kan utföras i Azure Portal.

## <a name="before-you-deploy-your-web-app-to-production"></a>Innan du distribuerar ditt webbprogram till produktion
### <a name="choose-a-tier"></a>Välj en nivå
Azure Apptjänst finns med i fem nivåer: Frigör delade, Basic, Standard och Premium. Information om funktionerna och priser för varje nivå finns [prisinformationen](https://azure.microsoft.com/pricing/details/app-service/). 

* [Apptjänstplaner](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) kan du gruppera flera webbprogram under samma nivå.
* Du kan alltid [växla nivåer](web-sites-scale.md) när du har skapat ditt webbprogram.

### <a name="configuration"></a>Konfiguration
Använd den [Azure Portal](https://portal.azure.com/) att ställa in olika konfigurationsalternativ. Mer information finns i [konfigurera webbappar i Azure App Service](web-sites-configure.md). Här är en snabb Checklista:

* Välj **runtime versioner** för .NET, PHP, Java och Python, om det behövs.
* Aktivera **WebSockets** om ditt webbprogram använder WebSocket-protokollet. (Detta omfattar appar som använder [ASP.NET SignalR](http://www.asp.net/signalr) eller [socket.io](web-sites-nodejs-chat-app-socketio.md).)
* Kör du webbjobb? I så fall, aktivera **alltid på**.
* Ange den **standarddokument**, till exempel index.html.

Förutom dessa inställningar för grundläggande konfiguration kan du vill konfigurera följande:

* **Secure Socket Layer (SSL)** kryptering. Om du vill använda SSL med ett anpassat domännamn, måste du få ett SSL-certifikat och konfigurerar ditt webbprogram för att använda den. Se [aktivera HTTPS för en webbapp i Azure App Service](app-service-web-tutorial-custom-ssl.md).
* **Anpassade domännamn.** Ditt webbprogram har automatiskt en underdomän under azurewebsites.net. Du kan associera ett anpassat domännamn, t.ex contoso.com. Se [konfigurera ett anpassat domännamn i Azure App Service](app-service-web-tutorial-custom-domain.md).

Språkspecifika konfiguration:

* **PHP**: [konfigurerar du PHP i Azure App Service Web Apps](web-sites-php-configure.md).
* **Python**: [konfigurera Python med Azure App Service Web Apps](web-sites-python-configure.md)

## <a name="while-your-web-app-is-running"></a>När ditt webbprogram kör
När din webbapp körs, som du vill kontrollera att den är tillgänglig och att det skalas för att uppfylla användaraktiviteten. Du kan också behöva felsöka.

### <a name="monitoring"></a>Övervakning
* Azure-portalen kan du [lägga till prestandamått](web-sites-monitor.md) , till exempel CPU-användning och antalet klientbegäranden.
* [Skala ditt webbprogram](web-sites-scale.md) som svar på trafik. Du kan skala antalet virtuella datorer och/eller storleken på VM-instanser beroende på din nivå. I Standard- och Premium-nivåer, kan du också ställa in autoskalning, så att ditt webbprogram automatiskt, skalar på ett fast schema eller på att läsa in.  

### <a name="backups"></a>Säkerhetskopior
* Ange [automatiska säkerhetskopieringar](web-sites-backup.md) av ditt webbprogram. Mer information om säkerhetskopior i [den här videon](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).
* Lär dig mer om alternativen för [återställning av databas](../sql-database/sql-database-business-continuity.md) i Azure SQL Database.

### <a name="troubleshooting"></a>Felsökning
* Om något går fel kan du [felsökning i Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), med hjälp av diagnostiska loggar och live-felsökning i molnet. 
* Det finns olika sätt att samla in diagnostikloggar utanför Visual Studio. Se [aktivera diagnostikloggning för web apps i Azure App Service](web-sites-enable-diagnostic-log.md).
* Node.js-program finns i [felsöka en Node.js-webbapp i Azure App Service](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Återställa Data
* [Återställa](web-sites-restore.md) ett webbprogram som tidigare har säkerhetskopierats.

## <a name="when-you-update-your-web-app"></a>När du uppdaterar ditt webbprogram
Om du inte har aktiverat automatiska säkerhetskopieringar kan du skapa en [manuell säkerhetskopiering](web-sites-backup.md).

Överväg att använda [stegvis distribution](web-sites-staged-publishing.md). Det här alternativet kan du publicera uppdateringar till en fristående distribution som körs sida-vid-sida med din Produktionsdistribution. 


<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website


