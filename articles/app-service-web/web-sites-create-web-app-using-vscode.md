---
title: aaaCreate en ASP.NET Core webbapp i Visual Studio Code
description: "Den här kursen visar hur toocreate en ASP.NET Core web app med Visual Studio-koden."
services: app-service\web
documentationcenter: .net
author: erikre
manager: erikre
editor: jimbe
ms.assetid: 877bff08-9ef7-405a-a1ca-1194f33c55f2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: cephalin
ms.openlocfilehash: 1c18c94984d71e88d2a5b792d68cb1c81e4a96d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-core-web-app-in-visual-studio-code"></a>Skapa en ASP.NET Core webbapp i Visual Studio Code
## <a name="overview"></a>Översikt
Den här kursen visar hur toocreate en ASP.NET Core web app med hjälp av [Visual Studio-koden (VS)](http://code.visualstudio.com//Docs/whyvscode) och distribuera det för[Azure App Service](../app-service/app-service-value-prop-what-is.md). 

> [!NOTE]
> Även om den här artikeln handlar tooweb appar, gäller även tooAPI apps och mobilappar. 
> 
> 

ASP.NET Core är en helt ny utformning av ASP.NET. ASP.NET Core är ett nytt öppen källkod och plattformsoberoende ramverk för att bygga moderna molnbaserade webbprogram med hjälp av .NET. Mer information finns i [introduktion tooASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html). Information om Azure App Service web apps finns [översikt över Web Apps](app-service-web-overview.md).

[!INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## <a name="prerequisites"></a>Krav
* Installera [VS koden](http://code.visualstudio.com/Docs/setup).
* Installera Git - du kan installera den från någon av dessa platser: [Chocolatey](https://chocolatey.org/packages/git) eller [git scm.com](http://git-scm.com/downloads). Om du är ny tooGit Välj [git scm.com](http://git-scm.com/downloads) och välj alternativet för hello för**använda Git från hello Windows kommandotolk**. När du installerar Git måste du också tooset hello Git-användarnamn och e-post eftersom det krävs senare i självstudiekursen hello (när du utför ett genomförande från VS-kod).  

## <a name="install-aspnet-core"></a>Installera ASP.NET Core
ASP.NET Core är en lean .NET-stack för att bygga moderna molnet och webb-appar som körs på OS X, Linux och Windows. Den byggts från hello bakgrund in tooprovide en optimerad utvecklingsramverk för appar som är antingen distribuerade toohello moln eller kör lokalt. Det består av modulära komponenter med minimal kostnader, så att du behåller flexibilitet när dina lösningar.

Den här kursen är utformad tooget som du startade utveckling av program med hello senaste utveckling versioner av ASP.NET Core. hello följande instruktioner är specifika tooWindows. Installationsinstruktioner för OS X, Linux och Windows, se [komma igång med ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started). 


> [!NOTE]
> Mer detaljerad Installationsinstruktioner för OS X, Linux och Windows finns [installera ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx). 
> 
> 

## <a name="create-hello-web-app"></a>Skapa hello-webbprogram
Det här avsnittet visar hur tooscaffold en ny app ASP.NET web app med hello .NET CLI-verktyget. 

1. Ange följande hello i hello kommandotolk toocreate hello projektet mappen och kodskelett hello app.
   
```terminal
mkdir SampleWebApp
cd SampleWebApp
dotnet new mvc
```
![DotNet CLI - ASP.NET Core generator](./media/web-sites-create-web-app-using-vscode/dotnetcore-mvc-01.png)

2. toorestore Hej nödvändiga NuGet-paket, kör hello följande kommando:
   
    ```terminal
    dotnet restore
    ```

## <a name="run-hello-web-app-locally"></a>Kör hello webbappen lokalt
Nu när du har skapat hello webbapp och hämta alla hello NuGet-paket för hello app, kan du köra hello webbappen lokalt.

1. Kör hello app (hello `dotnet run` kommando skapar hello app när den är för gammal):
    ```terminal
    dotnet run
    ```
2. Öppna en webbläsare och gå toohello följande URL: en.
   
    **http://localhost:5000**
   
    Så här visas hello standardsida av hello webbprogram.
   
    ![Lokala webbprogram i en webbläsare](./media/web-sites-create-web-app-using-vscode/08-web-app.png)
3. Stäng webbläsaren. I hello **kommandofönstret**, tryck på **Ctrl + C** tooshut hello programmet och Stäng hello **kommandofönstret**. 

## <a name="create-a-web-app-in-hello-azure-portal"></a>Skapa en webbapp i hello Azure-portalen
hello hjälper följande steg dig att skapa en webbapp i hello Azure-portalen.

1. Logga in toohello [Azure Portal](https://portal.azure.com).
2. Klicka på **ny** på hello upp till vänster i hello Portal.
3. Klicka på **Web Apps > Webbapp**.
   
    ![Nytt webbprogram för Azure](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)
4. Ange ett värde för **namn**, som **SampleWebAppDemo**. Observera att det här namnet måste toobe unika och hello portal kommer att verkställa som när du försöker tooenter hello namn. Därför, om du väljer att ange ett annat värde måste toosubstitute som värde för varje förekomst av **SampleWebAppDemo** som visas i den här kursen. 
5. Välj en befintlig **Apptjänstplan** eller skapa en ny. Om du skapar en ny plan, Välj hello prisnivå, plats, och andra alternativ. Mer information om apptjänstplaner finns hello artikeln [Azure App Service-planer djupgående översikt över](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
    ![Azure web app-bladet](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)
6. Klicka på **Skapa**.
   
    ![Web app bladet](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## <a name="enable-git-publishing-for-hello-new-web-app"></a>Aktivera Git-publicering för hello ny webbapp
Git är ett kontrollsystem för distribuerade versioner som du kan använda toodeploy din webbapp i Azure App Service. Du lagrar hello koden du skriver för webbappen i en lokal Git-lagringsplats och du kommer att distribuera din kod tooAzure genom att trycka på tooa fjärrdatabasen.   

1. Logga in på hello [Azure Portal](https://portal.azure.com).
2. Klicka på **Browse** (Bläddra).
3. Klicka på **Web Apps** tooview en lista över hello webbprogram som är associerade med din Azure-prenumeration.
4. Välj hello webbprogram som du skapade i den här kursen.
5. Hello web app-bladet, klickar du på **inställningar** > **kontinuerlig distribution**. 
   
    ![Azure web app värden](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)
6. Klicka på **Välj källa > lokal Git-lagringsplats**.
7. Klicka på **OK**.
   
    ![Azure lokal Git-lagringsplatsen](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)
8. Om du inte tidigare registrerat autentiseringsuppgifter för distribution för att publicera en webbapp eller andra Apptjänst-app, skapa dem nu:
   
   * Klicka på **inställningar** > **distributionsbehörigheterna**. Hej **ange autentiseringsuppgifter för distribution** bladet visas.
   * Skapa ett användarnamn och lösenord.  Du behöver lösenordet senare när du ställer in Git.
   * Klicka på **Spara**.
9. I din webbapps blad klickar du på **Inställningar > Egenskaper**. hello-URL för hello fjärransluten Git-lagringsplats som du ska distribuera toois visas under **URL för GIT**.
10. Kopiera hello **URL för GIT** värde för senare användning i hello självstudiekursen.
    
    ![Azure Git-URL](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## <a name="publish-your-web-app-tooazure-app-service"></a>Publicera din web app tooAzure Apptjänst
I det här avsnittet skapar en lokal Git-lagringsplats och skicka från den databasen tooAzure toodeploy tooAzure dina webb-app.

1. Välj hello i VS-kod, **Git** alternativet i hello vänstra navigeringsfältet.
   
    ![Git-ikonen i VS-kod](./media/web-sites-create-web-app-using-vscode/git-icon.png)
2. Välj **initiera git-lagringsplats** toomake till din arbetsyta källkontroll används git. 
   
    ![Starta Git](./media/web-sites-create-web-app-using-vscode/19-initgit.png)
3. Öppna hello-kommandofönster och ändra kataloger toohello katalogen för ditt webbprogram. Ange sedan hello följande kommando:
   
        git config core.autocrlf false
   
    Det här kommandot förhindrar att ett problem om text där CRLF ändelser och LF ändelser ingår.
4. Lägga till ett genomförande meddelande i VS-kod, och klicka på hello **Commit alla** kryssikon.
   
    ![Git genomför alla](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)
5. När Git har bearbetat ser du att det inte finns några filer som anges i hello Git fönstret under **ändringar**. 
   
    ![Git inga ändringar](./media/web-sites-create-web-app-using-vscode/no-changes.png)
6. Ändra tillbaka toohello kommandofönster där hello kommandotolk pekar toohello katalog där ditt webbprogram finns.
7. Skapa en fjärransluten referens för push-överföra uppdateringar tooyour webbprogram med hjälp av hello Git-URL (slutar med ”.git”) som du kopierade tidigare.
   
        git remote add azure [URL for remote repository]
8. Konfigurera Git toosave dina autentiseringsuppgifter lokalt så att de blir automatiskt tillagda tooyour push-kommandon som genereras från VS-kod.
   
        git config credential.helper store
9. Skicka ändringar-tooAzure genom att ange hello följande kommando. Efter den här första push-tooAzure kommer du att kunna toodo alla hello push-kommandon från VS-kod. 
   
        git push -u azure master
   
    Du ombeds hello-lösenord som du skapade tidigare i Azure. **Obs: Lösenordet visas inte.**
   
    hello utdata från hello ovan kommandot slutar med ett meddelande om att distributionen har lyckats.
   
        remote: Deployment successful.
        toohttps://user@testsite.scm.azurewebsites.net/testsite.git
        [new branch]      master -> master

> [!NOTE]
> Om du gör ändringar tooyour app, kan du publicera direkt i VS kod med hello inbyggda funktioner för Git genom att välja hello **genomför alla** alternativet följt av hello **Push** alternativet. Du hittar hello **Push** alternativ som finns i hello nedrullningsbara menyn nästa toohello **genomför alla** och **uppdatera** knappar.
> 
> 

Om du behöver toocollaborate med ett projekt, bör du överföra tooGitHub between trycka tooAzure.

## <a name="run-hello-app-in-azure"></a>Kör hello app i Azure
Nu när du har distribuerat ditt webbprogram kör vi hello app medan finns i Azure. 

Detta kan göras på två sätt:

* Öppna en webbläsare och ange hello namnet på ditt webbprogram på följande sätt.   
  
        http://SampleWebAppDemo.azurewebsites.net
* I hello Azure-portalen, leta upp hello web app bladet för din webbapp och på **Bläddra** tooview din app 
* i din standardwebbläsare.

![Azure-webbapp](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## <a name="summary"></a>Sammanfattning
I kursen får du lärt dig hur toocreate en webbapp i VS-kod och distribuerar den tooAzure. Mer information om VS koden finns hello artikeln [varför Visual Studio-koden?](https://code.visualstudio.com/Docs/) Information om App Service web apps finns [översikt över Web Apps](app-service-web-overview.md). 

