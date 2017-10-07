---
title: "aaaLocal Git-distribution tooAzure Apptjänst"
description: "Lär dig hur tooenable lokal Git-distribution tooAzure Apptjänst."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 1905e0b7acd58d8dd6496a14f6e4f38f9f8c0212
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="local-git-deployment-tooazure-app-service"></a>Lokal Git-distribution tooAzure Apptjänst
De här självstudierna visar hur toodeploy din app för[Azure App Service] från en Git-lagringsplats på den lokala datorn. Apptjänst har stöd för den här metoden med hello **lokala Git** distributionsalternativ i hello [Azure Portal].  
Många av hello Git-kommandon som beskrivs i den här artikeln ska utföras automatiskt när du skapar en Apptjänst-app med hello [Azure-kommandoradsgränssnittet] enligt [här](app-service-web-get-started.md).

## <a name="prerequisites"></a>Krav
toocomplete den här kursen behöver du:

* Git. Du kan hämta hello installationen binära [här](http://www.git-scm.com/downloads).  
* Grundläggande kunskaper om Git.
* Ett Microsoft Azure-konto. Om du inte har ett konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.  
> 
> 

## <a name="Step1"></a>Steg 1: Skapa en lokal databas
Utför följande uppgifter toocreate en ny Git-lagringsplats hello.

1. Starta ett kommandoradsverktyg, till exempel **GitBash** (Windows) eller **Bash** (Unix Shell). För OS X-datorer kan du komma åt hello kommandoradsverktyget via hello **Terminal** program.
2. Navigera toohello directory där hello innehåll toodeploy skulle hittas.
3. Använd följande kommando tooinitialize en ny Git-lagringsplats hello:

```bash  
git init
```

## <a name="Step2"></a>Steg 2: Bekräfta ditt innehåll
Apptjänst stöder program som skapats i en mängd olika programmeringsspråk. 

1. Om databasen innehåller redan innehåll hoppa över detta och flytta toopoint 2 nedan. Om databasen inte redan innehåller innehåll bara fylla i med en statisk HTML-fil på följande sätt: 
   
   * Med hjälp av en textredigerare, skapa en ny fil med namnet **index.html** hello roten i hello Git-lagringsplats
   * Lägg till följande text som hello innehållet för hello index.html filen och spara den hello: *Hello Git!*
2. Kontrollera att du är under hello roten för Git-lagringsplats från hello kommandoradsverktyg. Använd sedan följande kommando tooadd filer tooyour databasen hello:

```bash  
git add -A
```
3. Nästa, commit hello ändringar toohello databas med hjälp av hello följande kommando:

```bash  
git commit -m "Hello Azure App Service"
```  

## <a name="Step3"></a>Steg 3: Aktivera hello lagringsplatsen för Apptjänst-app
Utföra hello följande steg tooenable en Git-lagringsplats för din Apptjänst-app.

1. Logga in toohello [Azure Portal].
2. I bladet för din Apptjänst-app, klickar du på **Inställningar > distributionskälla**. Klicka på **Välj källa**, klicka på **lokal Git-lagringsplats**, och klicka sedan på **OK**.  
   
    ![Lokal Git-lagringsplats](./media/app-service-deploy-local-git/local_git_selection.png)
3. Om det är första gången du upp en databas i Azure, behöver du toocreate inloggningsuppgifterna för den. Du använder dem toolog i hello Azure-lagringsplatsen och pusha ändringarna från din lokala Git-lagringsplats. Från din app-bladet, klickar du på **Inställningar > autentiseringsuppgifter för distribution**, konfigurera distributionen användarnamn och lösenord. När du är klar klickar du på **spara**.
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Steg 4: Distribuera ditt projekt
Använd följande steg toopublish hello din app tooApp med hjälp av lokal Git.

1. I bladet för din app i hello Azure-portalen klickar du på **Inställningar > Egenskaper** för hello **URL för Git**.
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    **URL för Git** är hello fjärreferensdator toodeploy toofrom lokala databasen. I hello följande steg ska du använda denna URL.
2. Med hjälp av kommandoradsverktyget hello, kontrollera att du är i hello rot lokal Git-lagringsplats.
3. Använd `git remote` tooadd hello fjärreferensdator som anges i **URL för Git** från steg 1. Kommandot ser liknande toohello följande:
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > Hej **remote** kommando lägger till en namngiven referens tooa fjärransluten lagringsplats. I det här exemplet skapas en referens med namnet ”azure” för ditt webbprogram lagringsplats.
   > 
   > 
4. Push ditt innehåll tooApp tjänsten med hjälp av hello ny **azure** remote du nyss skapade.

```bash  
git push azure master
```
    You will be prompted for hello password you created earlier when you reset your deployment credentials in hello Azure Portal. Enter hello password (note that Gitbash does not echo asterisks toohello console as you type your password). 
5. Gå tillbaka tooyour app i hello Azure-portalen. En loggpost för din senaste push ska visas i hello **distributioner** bladet. 
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. Klicka på hello **Bläddra** knappen hello överst i hello appens blad tooverify hello innehåll har distribuerats. 

## <a name="Step5"></a>Felsökning
hello följande är fel eller problem som uppstår ofta när du använder Git toopublish tooan Apptjänst-app i Azure:

- - -
**Symtom**: Det gick inte tooaccess [siteURL]: Det gick inte tooconnect för [scmAddress]

**Orsak**: det här felet kan inträffa om hello appen inte är igång.

**Lösning**: Start hello app i hello Azure-portalen. Git-distribution fungerar inte om inte hello appen körs. 

- - -
**Symtom**: Det gick inte att matcha host värdnamn

**Orsak**: det här felet kan inträffa om hello adressinformation anges när du skapar hello ”azure” remote var felaktig.

**Lösning**: Använd hello `git remote -v` kommandot toolist alla fjärrkontroller tillsammans med hello associerade URL. Kontrollera att hello URL: en för hello ”azure” remote är korrekt. Om det behövs, ta bort och återskapa det här fjärråtkomst med hello rätt URL.

- - -
**Symtom**: inga refs i gemensamma och inget har angetts; gör ingenting. Du måste kanske ange en gren till exempel 'master'.

**Orsak**: det här felet kan inträffa om du inte anger en gren när en git push utförs och har inte angetts hello push.default värdet som används av Git.

**Lösning**: utföra hello push igen, ange hello mastergrenen. Exempel:

```bash  
git push azure master
```
- - -
**Symtom**: src refspec [branchname] matchar inte något.

**Orsak**: det här felet kan inträffa om du försöker toopush tooa gren Master på hello ”azure” remote.

**Lösning**: utföra hello push igen, ange hello mastergrenen. Exempel:

```bash  
git push azure master
```
- - -
**Symtom**: Det gick inte att RPC; leda = 22, HTTP-felkod = 502.

**Orsak**: det här felet kan inträffa om du försöker toopush en stor git-lagringsplats via HTTPS.

**Lösning**: ändra hello git-konfiguration på hello lokal dator toomake hello postBuffer större

```bash  
git config --global http.postBuffer 524288000
```
- - -
**Symtom**: fel - ändringar allokerat tooremote databasen men ditt webbprogram som inte har uppdaterats.

**Orsak**: det här felet kan inträffa om du distribuerar ett Node.js-app med en package.json-fil som anger ytterligare moduler som krävs.

**Lösning**: ytterligare meddelanden som innehåller 'npm fel ”! bör vara inloggade tidigare toothis fel och kan ge ytterligare sammanhang på hello-fel. hello följande är kända orsaker till felet och hello motsvarande 'npm fel ”! meddelande:

* **Felaktig package.json filen**: npm fel! Det gick inte att läsa beroenden.
* **Ursprunglig modul som inte har en binär distribution för Windows**:
  
  * npm fel! \`cmd ”/ c” ”nod gyp återskapa”\` misslyckades med 1
    
      ELLER
  * npm fel! [modulename@version] förinstallera: \`Se || gmake\`

## <a name="additional-resources"></a>Ytterligare resurser
* [Git-dokumentation](http://git-scm.com/documentation)
* [Projektet Kudu-dokumentation](https://github.com/projectkudu/kudu/wiki)
* [Kontinuerlig distribution tooAzure Apptjänst](app-service-continuous-deployment.md)
* [Hur toouse PowerShell för Azure](/powershell/azure/overview)
* [Hur toouse hello Azure-kommandoradsgränssnittet](../cli-install-nodejs.md)

[Azure App Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure Portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure-kommandoradsgränssnittet]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
