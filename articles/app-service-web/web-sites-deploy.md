---
title: "aaaDeploy din app tooAzure Apptjänst | Microsoft Docs"
description: "Lär dig hur toodeploy din app tooAzure Apptjänst."
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
ms.openlocfilehash: 5c84e4ca502874209d750c94efeb86a59aa71a48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-tooazure-app-service"></a>Distribuera din app tooAzure Apptjänst
Den här artikeln hjälper dig att avgöra hello bästa alternativet toodeploy hello filer för ditt webbprogram, mobilappsserverdel eller API-app för[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), och hjälper dig tooappropriate resurser med instruktioner specifika tooyour önskat Kontaktalternativ.

## <a name="overview"></a>Distributionsöversikt för Azure App Service
Azure Apptjänst underhåller hello application framework för du (ASP.NET, PHP, Node.js och så vidare). Vissa ramverk är aktiverade som standard medan andra, som Java och Python, behöver en enkel markering configuration tooenable den. Dessutom kan du anpassa din programramverk, till exempel hello PHP-version eller hello bitness för din runtime. Mer information finns i [konfigurera din app i Azure App Service](web-sites-configure.md).

Eftersom du inte har tooworry om hello web server eller application framework kan distribuera din app tooApp Service är en fråga för att distribuera din kod, binärfiler, innehållsfiler och deras respektive katalogstruktur toohello [   **/webbplats /Wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) i Azure (eller hello **/platsen/wwwroot/App_Data/jobb/** katalogen för WebJobs). Apptjänst stöder tre olika distributionsprocesser. Alla hello distributionsmetoder i den här artikeln använder någon av följande processer hello: 

* [FTP- eller FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): Använd din favorit FTP eller FTPS aktiverad verktyget toomove dina filer tooAzure från [FileZilla](https://filezilla-project.org) toofull aktuella IDEs som [NetBeans](https://netbeans.org). Detta är endast ett uppladdning av fil. Inga ytterligare tjänster som tillhandahålls av App Service versionskontroll, hantering av strukturen, t.ex. 
* [Kudu (Git för ett eller OneDrive/Dropbox)](https://github.com/projectkudu/kudu/wiki/Deployment): Kudu är hello [distributionsmotorn](https://github.com/projectkudu/kudu/wiki) i App Service. Skicka din kod tooKudu direkt från alla databaser. Kudu tillhandahåller även tillagda tjänster när koden skickas tooit, inklusive versionskontroll, paketet återställning MSBuild, och [web hook](https://github.com/projectkudu/kudu/wiki/Web-hooks) för kontinuerlig distribution och andra automation-aktiviteter. Hej Kudu-distributionsmotorn stöder 3 olika typer av datakällor för distribution:   
  
  * Innehåll sync från OneDrive och Dropbox   
  * Databas-baserade kontinuerlig distribution med automatisk synkronisering från GitHub och Bitbucket Visual Studio Team Services  
  * Databas-baserad distribution med manuell synkronisering från lokala Git  
* [Web Deploy](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): distribuera kod tooApp tjänst direkt från din favorit Microsoft verktyg som Visual Studio med hello samma verktygsuppsättning som automatiserar distributionen tooIIS servrar. Det här verktyget stöder endast diff-distribution, skapa databasen, transformeringar anslutningssträngar, etc. Webbdistribution skiljer sig från Kudu att programmet binärfiler skapas innan de distribueras tooAzure. Liknande tooFTP inga ytterligare tjänster som tillhandahålls av App Service.

Populära web utvecklingsverktyg stöd för en eller flera av dessa distributionsprocesser. Medan hello verktyg du väljer avgör hello distributionsprocesser du kan utnyttja, hello faktiska DevOps-funktionerna till din förfogande beror på hello kombination av hello distributionsprocessen och hello specifika verktyg du väljer. Till exempel om du utför webbdistribution från [Visual Studio med Azure SDK](#vspros), även om du inte får automation från Kudu, du får paketet återställning och MSBuild-automatisering i Visual Studio. 

> [!NOTE]
> Dessa distributionsprocesser inte faktiskt [etablera hello Azure-resurser](../azure-resource-manager/resource-group-template-deploy-portal.md) som din app kan behöva. Men visar de flesta av hello länkade hur-tooarticles hur tooprovision hello app och distribuera din kod tooit slutpunkt till slutpunkt. Du kan också hitta ytterligare alternativ för att etablera Azure-resurser i hello [automatisera distributionen med hjälp av kommandoradsverktygen](#automate) avsnitt.
> 
> 

## <a name="ftp"></a>Distribuera manuellt genom att överföra filer med FTP
Om du har använt toomanually kopiera webbservern innehåll tooa kan du använda en [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) verktyget toocopy filer, till exempel Utforskaren eller [FileZilla](https://filezilla-project.org/).

hello-tekniker för att kopiera filer manuellt är:

* Förtrogenhet och minimal komplexitet för FTP-tooling. 
* Att veta exakt där filerna ska.
* Ökad säkerhet med FTPS.

hello nackdelar med kopierar filerna manuellt är:

* Med tooknow hur toodeploy filer toohello rätt kataloger i App Service. 
* Ingen versionskontroll för återställning när fel uppstår.
* Inga inbyggda distributionshistoriken för felsökning av distributionsproblem med.
* Potentiella lång distribution gånger eftersom många FTP-verktyg inte ger endast diff-kopiering och helt enkelt kopiera alla hello-filer.  

### <a name="howtoftp"></a>Hur tooupload filer med FTP
Hej [Azure Portal](https://portal.azure.com) innehåller alla hello information som du behöver tooconnect tooyour app kataloger med hjälp av FTP- eller FTPS.

* [Distribuera din app tooAzure Apptjänst med hjälp av FTP](app-service-deploy-ftp.md)

## <a name="dropbox"></a>Distribuera genom att synkronisera med en moln-mapp
Ett bra alternativ för[kopierar filerna manuellt](#ftp) synkroniserar filer och mappar tooApp tjänsten från en molntjänst för lagring som OneDrive och Dropbox. Synkroniserar med en mapp i molnet använder hello Kudu-processen för distribution (se [översikt över distributionsprocessen](#overview)).

hello-tekniker för synkronisering med en mapp i molnet är:

* Enkelhet för distributionen. Tjänster som OneDrive och Dropbox ger skrivbord sync-klienter, så att din lokala arbetskatalogen är också distributionskatalogen.
* Distribution av en enda klickning.
* Alla funktioner i hello Kudu-distributionsmotorn är tillgänglig (t.ex. paketet återställning, automation).

hello nackdelar med synkroniserar med en mapp i molnet är:

* Ingen versionskontroll för återställning när fel uppstår.
* Ingen automatisk distribution manuell synkronisering krävs.

### <a name="howtodropbox"></a>Hur toodeploy som synkroniserar med en mapp i molnet
I hello [Azure Portal](https://portal.azure.com), kan du ange en mapp för synkronisering av innehåll i molnlagringen OneDrive eller Dropbox, arbeta med din app koden och innehållet i mappen och synkronisera tooApp Service med hello på en knapp.

* [Synkronisera innehåll från ett moln mappen tooAzure Apptjänst](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>Distribuera kontinuerligt från en molnbaserad källa kontroll-tjänst
Om Utvecklingsteamet använder en molnbaserad källa kod management (SCM)-tjänst som [Visual Studio Team Services](http://www.visualstudio.com/), [GitHub](https://www.github.com), eller [BitBucket](https://bitbucket.org/), kan du konfigurera appen Tjänsten toointegrate med databasen och distribuera kontinuerligt. 

hello-tekniker för att distribuera från en molnbaserad källa kontrollen tjänst är:

* Version kontrollen tooenable återställning.
* Möjlighet tooconfigure kontinuerlig distribution för Git (och ett tillämpliga) databaser. 
* Branschspecifika distribution kan distribuera olika filialer toodifferent [fack](web-sites-staged-publishing.md).
* Alla funktioner i hello Kudu-distributionsmotorn är tillgänglig (t.ex. distribution versionshantering, återställning, paketet återställning, automation).

hello con för att distribuera från en molnbaserad källa kontrollen tjänst är:

* Vissa kunskap om hello respektive SCM tjänst som krävs.

### <a name="vsts"></a>Hur toodeploy kontinuerligt från en molnbaserad källa styra service
I hello [Azure Portal](https://portal.azure.com), kan du konfigurera kontinuerlig distribution från Visual Studio Team Services, GitHub och Bitbucket.

* [Kontinuerlig distribution tooAzure Apptjänst](app-service-continuous-deployment.md). 

toofind ut hur tooconfigure kontinuerlig distribution manuellt från en moln-databas som inte visas av hello Azure-portalen (exempelvis [GitLab](https://gitlab.com/)), se [ställa in kontinuerlig distribution med manuella steg](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="localgitdeployment"></a>Distribuera från lokala Git
Om Utvecklingsteamet använder en lokal lokal källa kod (SCM) hanteringstjänsten baserat på Git, kan du konfigurera detta som en distribution källa tooApp Service. 

Tekniker för att distribuera från lokala Git är:

* Version kontrollen tooenable återställning.
* Branschspecifika distribution kan distribuera olika filialer toodifferent [fack](web-sites-staged-publishing.md).
* Alla funktioner i hello Kudu-distributionsmotorn är tillgänglig (t.ex. distribution versionshantering, återställning, paketet återställning, automation).

Nackdelar med att distribuera från lokala Git är:

* Vissa kunskap om hello respektive SCM-system som krävs.
* Inga nyckelfärdig lösningar för kontinuerlig distribution. 

### <a name="vsts"></a>Hur toodeploy från lokala Git
I hello [Azure Portal](https://portal.azure.com), kan du konfigurera lokal Git-distribution.

* [Lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md). 
* [Publicering av tooWeb appar från alla git/hg lagringsplatsen](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Distribuera med IDE-miljö
Om du redan använder [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) med en [Azure SDK](https://azure.microsoft.com/downloads/), eller andra IDE-paket som [Xcode](https://developer.apple.com/xcode/), [Eclipse](https://www.eclipse.org), och [ IntelliJ IDEA](https://www.jetbrains.com/idea/), kan du distribuera tooAzure direkt från inom din IDE. Det här alternativet är idealiskt för ett enskilt utvecklare.

Visual Studio stöder alla tre distributionsprocesser (FTP, Git och webbdistribution), beroende på dina inställningar, medan andra IDEs kan distribuera tooApp Service om de har FTP- eller Git-integrering (se [översikt över distributionsprocessen](#overview)).

hello-tekniker för att distribuera med IDE-miljö är:

* Minimera potentiellt hello tooling för din slutpunkt till slutpunkt programmet livscykel. Utveckla, felsöka, spåra och distribuera din app tooAzure alla utan att flytta utanför din IDE. 

hello nackdelar med distribution med IDE-miljö är:

* Tillagda komplexiteten i verktygsuppsättning.
* Kräver en källkontrollsystem fortfarande för en grupprojekt.

<a name="vspros"></a>Ytterligare tekniker för att distribuera med Visual Studio med Azure SDK är:

* Azure SDK gör Azure-resurser förstklassigt medborgarna i Visual Studio. Skapa, ta bort, redigera, starta och stoppa appar, fråga hello backend SQL-databas, live-debug hello Azure-program och mycket mer. 
* Live redigering av kodfiler på Azure.
* Live felsökning av appar i Azure.
* Integrerad Azure explorer.
* Diff-distribution. 

### <a name="vs"></a>Hur toodeploy direkt från Visual Studio
* [Kom igång med Azure och ASP.NET](app-service-web-get-started-dotnet.md). Hur toocreate och distribuera ett enkelt ASP.NET MVC-webbprojekt med hjälp av Visual Studio och webbdistribution.
* [Hur tooDeploy Azure WebJobs med Visual Studio](websites-dotnet-deploy-webjobs.md). Hur tooconfigure konsolprogram projekt så att de distribuera som WebJobs.  
* [ASP.NET Web-distribution med Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). En självstudiekurs serie om 12 delar som omfattar en fullständig uppsättning distributionsuppgifter än hello andra i den här listan. Vissa funktioner i Azure-distribution har lagts eftersom hello kursen skrevs, men information som läggs till senare förklara vad som saknas.
* [Distribuera ett ASP.NET-webbplats tooAzure i Visual Studio 2012 från en Git-lagringsplats direkt](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Förklarar hur toodeploy ett ASP.NET webb-projekt i Visual Studio, använda hello Git plugin toocommit hello kod tooGit och ansluta Azure toohello Git-lagringsplats. Från och med Visual Studio 2013, Git-stödet är inbyggt och behöver inte installera ett plugin-program.

### <a name="aztk"></a>Hur toodeploy med hello Azure-verktyg för Eclipse och IntelliJ IDEA
Microsoft gör det möjligt toodeploy Web Apps tooAzure direkt från Eclipse och IntelliJ via hello [Azure Toolkit för Eclipse](../azure-toolkit-for-eclipse.md) och [Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij.md). hello visar följande kurser hello steg som ingår i distributionen av enkla en ”Hello” world webbprogrammet tooAzure med hjälp av antingen IDE:

* [Skapa en Hello World-Webbapp för Azure i Eclipse](app-service-web-eclipse-create-hello-world-web-app.md). Den här kursen visar hur toouse hello Azure Toolkit för Eclipse toocreate och distribuera en Hello World-Webbapp för Azure.
* [Skapa en Hello World-Webbapp för Azure i IntelliJ](app-service-web-intellij-create-hello-world-web-app.md). Den här kursen visar hur toouse hello Azure Toolkit för IntelliJ toocreate och distribuera en Hello World-Webbapp för Azure.

## <a name="automate"></a>Automatisera distributionen med hjälp av kommandoradsverktyg
Om du föredrar hello kommandoradsverktyget terminal som hello utvecklingsmiljö väljer skript du distributionsuppgifter för din Apptjänst-app med hjälp av kommandoradsverktyg. 

Tekniker för att distribuera med hjälp av kommandoradsverktyg är:

* Aktiverar skripta distributionsscenarier.
* Integrera etablering av Azure-resurser och koden distribution.
* Integrera Azure-distribution i befintliga kontinuerlig integration skript.

Nackdelar med att distribuera med hjälp av kommandoradsverktyg är:

* Inte för GUI-föredrar utvecklare.

### <a name="automatehow"></a>Hur tooautomate distribution med kommandoradsverktyg

Se [automatisera distributionen av din Azure-app med kommandoradsverktyg](app-service-deploy-command-line.md) för en lista med kommandoradsverktyget verktyg och länkar tootutorials. 

## <a name="nextsteps"></a>Nästa steg
I vissa fall kanske du vill toobe kan tooeasily växla fram och tillbaka mellan en mellanlagrings- och en produktionsversionen av din app. Mer information finns i [mellanlagrad distributionen på Web Apps](web-sites-staged-publishing.md).

Att ha en plan för säkerhetskopiering och återställning är en viktig del av arbetsflöde för distribution. Information om hello Apptjänst säkerhetskopiering och återställning funktionen finns [Web Apps säkerhetskopieringar](web-sites-backup.md).  

Information om hur toouse Azure rollbaserad åtkomstkontroll toomanage åt tooApp tjänstdistributionen finns [RBAC och webbpublicering App](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).

