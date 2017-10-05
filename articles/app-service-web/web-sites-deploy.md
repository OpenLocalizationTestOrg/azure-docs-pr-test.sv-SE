---
title: Distribuera din app till Azure App Service | Microsoft Docs
description: "Lär dig mer om att distribuera din app till Azure App Service."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: f1464f71-2624-400e-86a2-e687e385804f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2017
ms.author: cephalin
ms.openlocfilehash: f41be4e00a9250b07ca260c2858e5fc45143f746
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-your-app-to-azure-app-service"></a>Distribuera din app till Azure App Service
Den här artikeln hjälper dig att avgöra det bästa alternativet för att distribuera filerna för ditt webbprogram, mobilappsserverdel eller API-appen [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), och hjälper dig att lämpliga resurser med instruktioner som är specifika för önskat Kontaktalternativ.

## <a name="overview"></a>Distributionsöversikt för Azure App Service
Azure Apptjänst underhåller application framework för (ASP.NET, PHP, Node.js och så vidare). Vissa ramverk är aktiverade som standard medan andra, som Java och Python, behöver en enkel markering konfiguration för att aktivera den. Dessutom kan du anpassa din programramverk, till exempel PHP-version eller bitness för din runtime. Mer information finns i [konfigurera din app i Azure App Service](web-sites-configure.md).

Eftersom du inte behöver bry dig om web server eller application framework distribuera din app till App Service är en fråga för att distribuera din kod, binärfiler, innehållsfiler och deras respektive katalogstrukturen till den [ **/platsen/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) i Azure (eller **/platsen/wwwroot/App_Data/jobb/** katalogen för WebJobs). Apptjänst stöder tre olika distributionsprocesser. Alla metoder för distribution i den här artikeln använder någon av följande processer: 

* [FTP- eller FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): Använd din favorit FTP eller FTPS aktiverad verktyg för att flytta filerna till Azure från [FileZilla](https://filezilla-project.org) till komplett IDEs som [NetBeans](https://netbeans.org). Detta är endast ett uppladdning av fil. Inga ytterligare tjänster som tillhandahålls av App Service versionskontroll, hantering av strukturen, t.ex. 
* [Kudu (Git för ett eller OneDrive/Dropbox)](https://github.com/projectkudu/kudu/wiki/Deployment): Kudu är den [distributionsmotorn](https://github.com/projectkudu/kudu/wiki) i App Service. Skicka koden till Kudu direkt från alla databaser. Kudu tillhandahåller även tillagda tjänster när koden skickas till den, inklusive versionskontroll, paketet återställning MSBuild, och [web hook](https://github.com/projectkudu/kudu/wiki/Web-hooks) för kontinuerlig distribution och andra automation-aktiviteter. Kudu-distributionsmotorn stöder 3 olika typer av datakällor för distribution:   
  
  * Innehåll sync från OneDrive och Dropbox   
  * Databas-baserade kontinuerlig distribution med automatisk synkronisering från GitHub och Bitbucket Visual Studio Team Services  
  * Databas-baserad distribution med manuell synkronisering från lokala Git  
* [Web Deploy](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): distribuera kod till App Service direkt från din favorit Microsoft verktyg som Visual Studio som använder samma tooling som automatiserar distributionen till IIS-servrar. Det här verktyget stöder endast diff-distribution, skapa databasen, transformeringar anslutningssträngar, etc. Webbdistribution skiljer sig från Kudu i den programbinärer skapas innan de distribueras till Azure. Liknar FTP, inga ytterligare tjänster som tillhandahålls av App Service.

Populära web utvecklingsverktyg stöd för en eller flera av dessa distributionsprocesser. Medan verktyget du väljer avgör distributionsprocesser som du kan utnyttja, beror den faktiska DevOps-funktionerna på tillgänglig på kombinationen av distributionsprocessen och de verktyg du väljer. Till exempel om du utför webbdistribution från [Visual Studio med Azure SDK](#vspros), även om du inte får automation från Kudu, du får paketet återställning och MSBuild-automatisering i Visual Studio. 

> [!NOTE]
> Dessa distributionsprocesser inte faktiskt [etablera Azure-resurser](../azure-resource-manager/resource-group-template-deploy-portal.md) som din app kan behöva. Men de flesta av de länkade instruktionsartiklar visar hur du etablerar appen och distribuera din kod till den slutpunkt till slutpunkt. Du kan också hitta ytterligare alternativ för att etablera Azure-resurser i den [automatisera distributionen med hjälp av kommandoradsverktygen](#automate) avsnitt.
> 
> 

## <a name="ftp"></a>Distribuera manuellt genom att överföra filer med FTP
Om du används för att manuellt kopiera innehållet till en webbserver, kan du använda en [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) verktyg för att kopiera filer, till exempel Utforskaren eller [FileZilla](https://filezilla-project.org/).

Tekniker för att kopiera filer manuellt är:

* Förtrogenhet och minimal komplexitet för FTP-tooling. 
* Att veta exakt där filerna ska.
* Ökad säkerhet med FTPS.

Nackdelar med kopierar filerna manuellt är:

* Ha kunskap om att distribuera filer till rätt kataloger i App Service. 
* Ingen versionskontroll för återställning när fel uppstår.
* Inga inbyggda distributionshistoriken för felsökning av distributionsproblem med.
* Potentiella lång distribution gånger eftersom många FTP-verktyg inte ger endast diff-kopiering och helt enkelt kopiera alla filer.  

### <a name="howtoftp"></a>Hur du överför filer med FTP
Den [Azure Portal](https://portal.azure.com) ger dig all information du behöver ansluta till din app genom att ange FTP- eller FTPS.

* [Distribuera din app till Azure App Service med FTP](app-service-deploy-ftp.md)

## <a name="dropbox"></a>Distribuera genom att synkronisera med en moln-mapp
Ett bra alternativ för att [kopierar filerna manuellt](#ftp) synkroniserar filer och mappar till App Service från en molntjänst för lagring som OneDrive och Dropbox. Synkroniserar med en mapp i molnet använder Kudu-processen för distribution (se [översikt över distributionsprocessen](#overview)).

Tekniker för synkronisering med en mapp i molnet är:

* Enkelhet för distributionen. Tjänster som OneDrive och Dropbox ger skrivbord sync-klienter, så att din lokala arbetskatalogen är också distributionskatalogen.
* Distribution av en enda klickning.
* Alla funktioner i Kudu-distributionsmotorn är tillgänglig (t.ex. paketet återställning, automation).

Nackdelar med synkroniserar med en mapp i molnet är:

* Ingen versionskontroll för återställning när fel uppstår.
* Ingen automatisk distribution manuell synkronisering krävs.

### <a name="howtodropbox"></a>Hur man distribuerar synkroniserar med en mapp i molnet
I den [Azure Portal](https://portal.azure.com), du kan ange en mapp för synkronisering av innehåll i molnlagringen OneDrive eller Dropbox, arbeta med din app koden och innehållet i mappen och synkronisera till App Service klickar på en knapp.

* [Synkronisera innehåll från en mapp i molnet till Azure App Service](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>Distribuera kontinuerligt från en molnbaserad källa kontroll-tjänst
Om Utvecklingsteamet använder en molnbaserad källa kod management (SCM)-tjänst som [Visual Studio Team Services](http://www.visualstudio.com/), [GitHub](https://www.github.com), eller [BitBucket](https://bitbucket.org/), du kan konfigurera App Service för att integrera med din databas och distribuera kontinuerligt. 

Tekniker för att distribuera från en molnbaserad källa kontrollen tjänst är:

* Kontrollerar att aktivera återställning.
* Möjligheten att konfigurera kontinuerlig distribution för Git (och ett tillämpliga) databaser. 
* Branschspecifika distribution, kan du distribuera olika filialer till olika [fack](web-sites-staged-publishing.md).
* Alla funktioner i Kudu-distributionsmotorn är tillgänglig (t.ex. distribution versionshantering, återställning, paketet återställning, automation).

Con för att distribuera från en molnbaserad källa kontrollen tjänst är:

* Vissa kunskap om respektive SCM-tjänst som krävs.

### <a name="vsts"></a>Hur du distribuerar kontinuerligt från en molnbaserad källa kontroll-tjänst
I den [Azure Portal](https://portal.azure.com), kan du konfigurera kontinuerlig distribution från Visual Studio Team Services, GitHub och Bitbucket.

* [Kontinuerlig distribution till Azure App Service](app-service-continuous-deployment.md). 

Ta reda på hur du konfigurerar kontinuerlig distribution manuellt från en moln-databas som inte finns med i Azure Portal (t.ex [GitLab](https://gitlab.com/)), se [ställa in kontinuerlig distribution med manuella steg](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="localgitdeployment"></a>Distribuera från lokala Git
Om Utvecklingsteamet använder en lokal lokal källa kod (SCM) hanteringstjänsten baserat på Git, kan du konfigurera detta som en distributionskälla till App Service. 

Tekniker för att distribuera från lokala Git är:

* Kontrollerar att aktivera återställning.
* Branschspecifika distribution, kan du distribuera olika filialer till olika [fack](web-sites-staged-publishing.md).
* Alla funktioner i Kudu-distributionsmotorn är tillgänglig (t.ex. distribution versionshantering, återställning, paketet återställning, automation).

Nackdelar med att distribuera från lokala Git är:

* Vissa kunskap om respektive SCM-systemet som krävs.
* Inga nyckelfärdig lösningar för kontinuerlig distribution. 

### <a name="vsts"></a>Hur du distribuerar från lokala Git
I den [Azure Portal](https://portal.azure.com), kan du konfigurera lokal Git-distribution.

* [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md). 
* [Publicering till Web Apps från alla git/hg lagringsplatsen](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Distribuera med IDE-miljö
Om du redan använder [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) med en [Azure SDK](https://azure.microsoft.com/downloads/), eller andra IDE-paket som [Xcode](https://developer.apple.com/xcode/), [Eclipse](https://www.eclipse.org), och [IntelliJ IDEA](https://www.jetbrains.com/idea/), du kan distribuera till Azure direkt från inom din IDE. Det här alternativet är idealiskt för ett enskilt utvecklare.

Visual Studio stöder alla tre distributionsprocesser (FTP, Git och webbdistribution), beroende på dina inställningar, medan andra IDEs kan distribuera till App Service om de har FTP- eller Git-integrering (se [översikt över distributionsprocessen](#overview)).

Tekniker för att distribuera med IDE-miljö är:

* Minimera potentiellt verktygsuppsättning för din slutpunkt till slutpunkt programmet livscykel. Utveckla, felsöka, spåra och distribuera din app till Azure alla utan att flytta utanför din IDE. 

Nackdelar med distribution med IDE-miljö är:

* Tillagda komplexiteten i verktygsuppsättning.
* Kräver en källkontrollsystem fortfarande för en grupprojekt.

<a name="vspros"></a>Ytterligare tekniker för att distribuera med Visual Studio med Azure SDK är:

* Azure SDK gör Azure-resurser förstklassigt medborgarna i Visual Studio. Skapa, ta bort, redigera, starta, och stoppa appar, frågar backend SQL-databas, live-debug Azure-program och mycket mer. 
* Live redigering av kodfiler på Azure.
* Live felsökning av appar i Azure.
* Integrerad Azure explorer.
* Diff-distribution. 

### <a name="vs"></a>Så här distribuerar du direkt från Visual Studio
* [Kom igång med Azure och ASP.NET](app-service-web-get-started-dotnet.md). Så här skapar och distribuerar en enkel ASP.NET MVC-webbprojekt med hjälp av Visual Studio och webbdistribution.
* [Hur du distribuerar Azure WebJobs med Visual Studio](websites-dotnet-deploy-webjobs.md). Hur du konfigurerar konsolprogram projekt så att de distribuera som WebJobs.  
* [ASP.NET Web-distribution med Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). En självstudiekurs serie om 12 delar som omfattar en fullständig uppsättning distributionsuppgifter än andra i den här listan. Vissa funktioner i Azure-distribution har lagts eftersom kursen skrevs, men information som läggs till senare förklara vad som saknas.
* [Distribuera en ASP.NET-webbplats till Azure i Visual Studio 2012 från en Git-lagringsplats direkt](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Beskriver hur du distribuerar ett ASP.NET-webbprojekt i Visual Studio med Git plugin-program för att utföra koden i Git och anslutande Azure Git-lagringsplatsen. Från och med Visual Studio 2013, Git-stödet är inbyggt och behöver inte installera ett plugin-program.

### <a name="aztk"></a>Så här distribuerar du använder Azure-verktyg för Eclipse och IntelliJ IDEA
Microsoft gör det möjligt att distribuera Webbappar till Azure direkt från Eclipse och IntelliJ via den [Azure Toolkit för Eclipse](../azure-toolkit-for-eclipse.md) och [Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij.md). Följande kurser visar steg som ingår i distributionen av enkla en ”Hello” world Web App till Azure med hjälp av antingen IDE:

* [Skapa en Hello World-Webbapp för Azure i Eclipse](app-service-web-eclipse-create-hello-world-web-app.md). Den här kursen visar hur du använder Azure-verktygen för Eclipse för att skapa och distribuera en Hello World-Webbapp för Azure.
* [Skapa en Hello World-Webbapp för Azure i IntelliJ](app-service-web-intellij-create-hello-world-web-app.md). Den här kursen visar hur du använder Azure-verktygen för IntelliJ att skapa och distribuera en Hello World-Webbapp för Azure.

## <a name="automate"></a>Automatisera distributionen med hjälp av kommandoradsverktyg
Om du föredrar kommandoradsverktyget terminal som utvecklingsmiljö väljer skript du distributionsuppgifter för din Apptjänst-app med hjälp av kommandoradsverktyg. 

Tekniker för att distribuera med hjälp av kommandoradsverktyg är:

* Aktiverar skripta distributionsscenarier.
* Integrera etablering av Azure-resurser och koden distribution.
* Integrera Azure-distribution i befintliga kontinuerlig integration skript.

Nackdelar med att distribuera med hjälp av kommandoradsverktyg är:

* Inte för GUI-föredrar utvecklare.

### <a name="automatehow"></a>Automatisera distributionen av kommandoradsverktyg

Se [automatisera distributionen av din Azure-app med kommandoradsverktyg](app-service-deploy-command-line.md) en lista över kommandoradsverktyg och länkar till självstudier. 

## <a name="nextsteps"></a>Nästa steg
I vissa fall kanske du vill kunna enkelt växla mellan en mellanlagrings- och en produktionsversionen av din app. Mer information finns i [mellanlagrad distributionen på Web Apps](web-sites-staged-publishing.md).

Att ha en plan för säkerhetskopiering och återställning är en viktig del av arbetsflöde för distribution. Information om App Service säkerhetskopiera och återställa funktionen, se [Web Apps säkerhetskopieringar](web-sites-backup.md).  

Information om hur du använder Azure rollbaserad åtkomstkontroll för att hantera åtkomst till App Service-distributionen finns [RBAC och webbpublicering App](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).

