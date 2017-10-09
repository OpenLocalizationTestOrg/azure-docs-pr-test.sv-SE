---
title: aaaAzure WebJobs-dokumentation
description: "Rekommenderade inlärningsresurser för hur toouse Azure WebJobs och hello Azure WebJobs SDK."
services: app-service
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: ed005e56-4334-4641-a5e5-15435c2be36b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2017
ms.author: glenga
ms.openlocfilehash: 6616a9d97c9637ec64cb8743dded6ba409a521ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-webjobs-documentation-resources"></a>Azure WebJobs-dokumentation
## <a name="overview"></a>Översikt
Det här avsnittet länkar toodocumentation resurser om hur toouse Azure WebJobs och hello Azure WebJobs SDK. Azure WebJobs ger ett enkelt sätt toorun skript eller program som bakgrund processer i hello kontexten för en [App Service webbapp, API-app eller mobilapp](../app-service/app-service-value-prop-what-is.md). Du kan ladda upp och köra en körbar fil som som cmd bat exe (.NET) ps1, del, php, kopiera, js, och jar. Dessa program fungerar som WebJobs enligt ett schema (cron) eller kontinuerligt.

Hej syftet hello [WebJobs SDK](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk) är toosimplify hello koden du skriver för vanliga uppgifter som ett Webbjobb kan utföra, till exempel bild bearbetning, kö bearbetning, RSS aggregering, filunderhåll, och skicka e-post. Hej WebJobs SDK har inbyggda funktioner för att arbeta med Azure Storage- och Service Bus för schemalagda aktiviteter och hantera fel och för många andra vanliga scenarier. Dessutom har utformats för toobe extensible och det finns en [öppen källkod lagringsplatsen för tillägg](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Azure Functions](../azure-functions/functions-overview.md) (för närvarande i förhandsversion) är baserad på en version av hello WebJobs SDK som fungerar med C# skript, Node.js och andra språk. 

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

Skapa, distribuera och hantera WebJobs är integrerad med integrerad verktygsuppsättning i Visual Studio. Du kan skapa WebJobs från mallar, publicera och hantera (köra, stoppa, övervaka och felsöka) dem. 

Hej WebJobs-instrumentpanelen i hello Azure-portalen innehåller kraftfulla funktioner som ger dig fullständig kontroll över hello körningen av WebJobs, inklusive hello möjlighet tooinvoke enskilda funktioner i WebJobs. hello instrumentpanelen visar även funktionen körningar och loggning utdata. 

## <a name="getstarted"></a>Komma igång med WebJobs och hello WebJobs SDK
* [Introduktion tooAzure WebJobs](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure WebJobs är helt otroligt och du bör börja använda dem just nu!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Blogginlägg av Troy Hunt).
* [Azure WebJobs-funktioner](https://azure.microsoft.com/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Vad är hello WebJobs SDK](websites-dotnet-webjobs-sdk.md)
* [Bakgrund jobb vägledning av Microsoft Patterns and Practices](https://docs.microsoft.com/azure/architecture/best-practices/background-jobs)
* [Om hello 1.1.0 RTM för Microsoft Azure WebJobs SDK](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Kom igång med hello Azure WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md)
* [Hur toouse Azure kö lagring med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Hur toouse Azure blob storage med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Hur toouse Azure table storage med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Hur toouse Azure Service Bus med hello WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)
* [Azure WebJobs SDK Snabbreferens (PDF nedladdning)](https://go.microsoft.com/fwlink/p/?linkid=845558)
* [Dokumentation för WebJobs-inställningar i GitHub](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Videoklipp
  * [Webbjobb och hello WebJobs SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
  * [Azure WebJobs-videoserie på Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
  * [Introduktion till WebJobs Tooling för Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
  * [WebJobs verktygsuppsättning och fjärrfelsökning](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
  * [Azure WebJobs-uppdatering med Pranav Rastogi - utökningsbarhet i version 1.1](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Se även följande avsnitt hello [distribuera WebJobs](#deploy) och [testning och felsökning WebJobs](#debug).

## <a name="deploy"></a>Distribuera WebJobs
* [Hur tooDeploy Azure WebJobs med Visual Studio](websites-dotnet-deploy-webjobs.md)
* [Hur toodeploy WebJobs med hello Azure-portalen](web-sites-create-web-jobs.md)
* [Aktivera kommandorads- eller kontinuerlig leverans av Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [Distribuerar .NET konsolen app tooAzure med WebJobs Git](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Distribuera en F # Webjobs tooAzure](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Distribuera anpassade tjänster som Azure Webjobs](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Videoklipp
  * [Introduktion till WebJobs Tooling för Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
  * [WebJobs verktygsuppsättning och fjärrfelsökning](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

## <a name="schedule"></a>Schemalägga WebJobs
* [hello lägga till Azure Webjobs dialogrutan](websites-dotnet-deploy-webjobs.md#configure)
* [Skapa ett schemalagda Webbjobb i hello Azure-portalen](web-sites-create-web-jobs.md#CreateScheduled)
* [Kopplar upp en scheduler jobbet tooa Webbjobbet](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Schemalägga Azure WebJobs med cron-uttryck](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Schemalägga enskilda Webbjobb funktioner med hjälp av hello WebJobs SDK TimerTrigger](websites-dotnet-webjobs-sdk.md#schedule)

## <a name="debug"></a>Testa och felsöka WebJobs
* [Nya utvecklare och felsökning funktioner för Azure WebJobs i Visual Studio](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [Visa hello WebJobs-instrumentpanelen](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Hur toowrite loggar med hello WebJobs-SDK och visa dem i hello instrumentpanelen](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Fjärråtkomst felsökning WebJobs](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Som skrev den blobben?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [Värd för interaktiva koden i hello moln](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Lägga till Trace tooAzure WebJobs](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Övervaka, diagnostisera och felsöka Microsoft Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md)
* Videoklipp
  * [WebJobs verktygsuppsättning och fjärrfelsökning](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

## <a name="scale"></a>Skalning WebJobs
* [Skala ditt webbprogram med Azure Websites](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Azure Apptjänst: Utforma massiv skala redo för Business Web Apps](https://channel9.msdn.com/Events/Build/2014/3-626). Omfattar skalning av webbprogram med WebJobs, inklusive hello WebJobs-SDK.
* Videoklipp
  * [Skala ut WebJobs](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

## <a name="additional"></a>Ytterligare resurser för WebJobs
* [Azure WebJobs GA blogginlägget av Magnus Mårtensson](http://magnusmartensson.com/azure-webjobs-ga)
* [Powershell Web jobb som körs på Azure Apptjänst](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Hämtar ett meddelande när din Azure utlöst WebJobs har slutförts](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Enkel Web App säkerhetskopiering bevarandeprincip med WebJobs](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Azure-Webbappar och molntjänster långsamma på första begäran](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Visar hur toouse WebJobs toosimulate hello AlwaysOn-funktionen är endast tillgängligt för hello Standard prisnivå.
* [WebJobs korrekt avslutning](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). WebJobs SDK korrekt avslutning, se [korrekt avslutning](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Automatiserad säkerhetskopiering med Azure WebJobs & AzCopy](http://markjbrown.com/azure-webjobs-azcopy/)
* Videoklipp
  * [Azure WebJobs-videor av Magnus Mårtensson](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
  * [Azure WebJobs-videoserie på Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

## <a name="additionalsdk"></a>Ytterligare resurser för WebJobs SDK
* [WebJobs SDK viktig information](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [Källkoden för WebJobs-SDK](https://github.com/Azure/azure-webjobs-sdk)
* [WebJobs-SDK-tilläggen källkod](https://github.com/Azure/azure-webjobs-sdk-extensions), med [detaljerad guide-modellen för utökning av toohello](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).  
* [WebJobs SDK Skriptets källkod](https://github.com/Azure/azure-webjobs-sdk-script/) (används för [Azure Functions](../azure-functions/functions-overview.md))
* [Webbjobbet tooupload FREB filer tooAzure Storage med hello WebJobs SDK](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Värd för Azure webjobs utanför Azure, värd med hello loggning fördelar från ett Azure webbjobbet](http://bypassion.dk/?p=510)
* [Skapa ett verktyg för Import av Data med Azure WebJobs](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Översikt över Azure Functions](../azure-functions/functions-overview.md)
* Videoklipp
  * [Azure WebJobs-videoserie på Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

## <a name="samples"></a>Webbjobbet exempelprogram
* [Exempelprogram som tillhandahålls av hello WebJobs-teamet på GitHub](https://github.com/azure/azure-webjobs-sdk-samples)
* [Enkel Azure Web App med WebJobs Backend med hjälp av hello WebJobs SDK](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Visar använder schemalagda och händelsedriven WebJobs. Finns i blogginlägget hello [Rebuilding hello SiteMonitR med Azure WebJobs SDK](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs).

## <a name="blogs"></a>Bloggar
* [Azure blogg](/blog)
* [Amit Apple blogg](http://blog.amitapple.com/). Fokuserar på WebJobs (inte hello SDK).
* [Magnus Mårtensson blogg](http://magnusmartensson.com/)

## <a name="gethelp"></a>Få hjälp med WebJobs
* [StackOverflow för WebJobs](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [StackOverflow för hello WebJobs SDK](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [StackOverflow för Azure Functions](http://stackoverflow.com/questions/tagged/azure-functions)
* [Azure- och ASP.NET-forum](http://forums.asp.net/1247.aspx)
* [Azure App Service Web Apps-forum](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure Web Apps User Voice-plats](https://feedback.azure.com/forums/169385-websites/)
* [Twitter](http://twitter.com/). Använd hello hashtaggar #AzureWebJobs.
* [Rapportera ett WebJobs fel eller problem](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

