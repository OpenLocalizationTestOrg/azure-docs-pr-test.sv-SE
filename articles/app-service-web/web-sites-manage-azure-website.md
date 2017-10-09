---
title: aaaManage en webbapp i Azure App Service
description: "Länkar tooresources för att hantera en webbapp i Azure App Service."
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
ms.openlocfilehash: daf69245e66068b0e97e3ae1c3fb5fce45605b91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-web-app-in-azure-app-service"></a>Hantera en webbapp i Azure App Service
Det här avsnittet innehåller länkar tooresources för att hantera en webbapp i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Hantering innehåller alla hello-åtgärder som håller din webbapp körs utan problem. 

Över hello livslängden för en webbapp, ska du utföra olika hanteringsuppgifter, när du flyttar från första distributionen toonormal åtgärden, underhåll och uppdateringar.

Många aktiviteter för webb-app kan utföras i hello Azure-portalen.

## <a name="before-you-deploy-your-web-app-tooproduction"></a>Innan du distribuerar din web app tooproduction
### <a name="choose-a-tier"></a>Välj en nivå
Azure Apptjänst finns med i fem nivåer: Frigör delade, Basic, Standard och Premium. Information om hello funktioner och priser för varje nivå finns [prisinformationen](https://azure.microsoft.com/pricing/details/app-service/). 

* [Apptjänstplaner](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) kan du gruppera flera webbprogram under hello samma nivå.
* Du kan alltid [växla nivåer](web-sites-scale.md) när du har skapat ditt webbprogram.

### <a name="configuration"></a>Konfiguration
Använd hello [Azure Portal](https://portal.azure.com/) tooset olika konfigurationsalternativ. Mer information finns i [konfigurera webbappar i Azure App Service](web-sites-configure.md). Här är en snabb Checklista:

* Välj **runtime versioner** för .NET, PHP, Java och Python, om det behövs.
* Aktivera **WebSockets** om ditt webbprogram använder hello WebSocket-protokollet. (Detta omfattar appar som använder [ASP.NET SignalR](http://www.asp.net/signalr) eller [socket.io](web-sites-nodejs-chat-app-socketio.md).)
* Kör du webbjobb? I så fall, aktivera **alltid på**.
* Ange hello **standarddokument**, till exempel index.html.

I tillägg toothese grundläggande konfigurationsinställningar, kanske du vill tooconfigure hello följande:

* **Secure Socket Layer (SSL)** kryptering. toouse SSL med ett anpassat domännamn måste du få ett SSL-certifikat och konfigurera din web app toouse den. Se [aktivera HTTPS för en webbapp i Azure App Service](app-service-web-tutorial-custom-ssl.md).
* **Anpassade domännamn.** Ditt webbprogram har automatiskt en underdomän under azurewebsites.net. Du kan associera ett anpassat domännamn, t.ex contoso.com. Se [konfigurera ett anpassat domännamn i Azure App Service](app-service-web-tutorial-custom-domain.md).

Språkspecifika konfiguration:

* **PHP**: [konfigurerar du PHP i Azure App Service Web Apps](web-sites-php-configure.md).
* **Python**: [konfigurera Python med Azure App Service Web Apps](web-sites-python-configure.md)

## <a name="while-your-web-app-is-running"></a>När ditt webbprogram kör
När din webbapp körs, vill du att den är tillgänglig och att det skalas toomeet användaraktiviteten toomake. Du kanske också måste tootroubleshoot fel.

### <a name="monitoring"></a>Övervakning
* Via hello Azure-portalen, kan du [lägga till prestandamått](web-sites-monitor.md) , till exempel CPU-användning och antalet klientbegäranden.
* [Skala ditt webbprogram](web-sites-scale.md) i svaret tootraffic. Beroende på din nivå kan du skala hello antal virtuella datorer och/eller hello storleken på hello VM-instanser. I hello Standard och Premium-nivåer, kan du också ställa in autoskalning, så att ditt webbprogram skalar automatiskt enligt ett fast schema eller i svaret tooload.  

### <a name="backups"></a>Säkerhetskopior
* Ange [automatiska säkerhetskopieringar](web-sites-backup.md) av ditt webbprogram. Mer information om säkerhetskopior i [den här videon](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).
* Lär dig mer om hello alternativ för [återställning av databas](../sql-database/sql-database-business-continuity.md) i Azure SQL Database.

### <a name="troubleshooting"></a>Felsökning
* Om något går fel kan du [felsökning i Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), med hjälp av diagnostiska loggar och live-felsökning i hello molnet. 
* Det finns olika sätt toocollect diagnostikloggar utanför Visual Studio. Se [aktivera diagnostikloggning för web apps i Azure App Service](web-sites-enable-diagnostic-log.md).
* Node.js-program finns i [hur toodebug en Node.js webbapp i Azure App Service](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Återställa Data
* [Återställa](web-sites-restore.md) ett webbprogram som tidigare har säkerhetskopierats.

## <a name="when-you-update-your-web-app"></a>När du uppdaterar ditt webbprogram
Om du inte har aktiverat automatiska säkerhetskopieringar kan du skapa en [manuell säkerhetskopiering](web-sites-backup.md).

Överväg att använda [stegvis distribution](web-sites-staged-publishing.md). Det här alternativet kan du publicera uppdateringar tooa mellanlagring av distribution som körs sida-vid-sida med din Produktionsdistribution. 


<!-- Anchors. -->

[Before you deploy your site tooproduction]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website


