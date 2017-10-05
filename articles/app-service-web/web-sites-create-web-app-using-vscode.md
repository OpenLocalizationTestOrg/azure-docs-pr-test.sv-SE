---
title: Skapa en ASP.NET Core webbapp i Visual Studio Code
description: "Den här kursen visar hur du kan skapa en ASP.NET Core webbprogram med hjälp av Visual Studio-koden."
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
ms.openlocfilehash: 46e3852dc84265de41bb358f482dec06608e7efa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-aspnet-core-web-app-in-visual-studio-code"></a>Skapa en ASP.NET Core webbapp i Visual Studio Code
## <a name="overview"></a>Översikt
Den här kursen visar hur du skapar en ASP.NET Core web app med hjälp av [Visual Studio-koden (VS)](http://code.visualstudio.com//Docs/whyvscode) och distribuerar det till [Azure App Service](../app-service/app-service-value-prop-what-is.md). 

> [!NOTE]
> Även om den här artikeln handlar om webbappar, så gäller den även för API-appar och mobilappar. 
> 
> 

ASP.NET Core är en helt ny utformning av ASP.NET. ASP.NET Core är ett nytt öppen källkod och plattformsoberoende ramverk för att bygga moderna molnbaserade webbprogram med hjälp av .NET. Mer information finns i [introduktion till ASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html). Information om Azure App Service web apps finns [översikt över Web Apps](app-service-web-overview.md).

[!INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## <a name="prerequisites"></a>Krav
* Installera [VS koden](http://code.visualstudio.com/Docs/setup).
* Installera Git - du kan installera den från någon av dessa platser: [Chocolatey](https://chocolatey.org/packages/git) eller [git scm.com](http://git-scm.com/downloads). Om du är nybörjare på Git väljer [git scm.com](http://git-scm.com/downloads) och väljer alternativet att **använda Git från Kommandotolken i Windows**. När du installerar Git, behöver du också ange Git-användarnamn och e-eftersom det krävs senare under kursen (när du utför ett genomförande från VS-kod).  

## <a name="install-aspnet-core"></a>Installera ASP.NET Core
ASP.NET Core är en lean .NET-stack för att bygga moderna molnet och webb-appar som körs på OS X, Linux och Windows. Det har skapats från grunden ge en optimerad utvecklingsramverk för appar som distribuerats till molnet eller kör lokalt. Det består av modulära komponenter med minimal kostnader, så att du behåller flexibilitet när dina lösningar.

Den här kursen är avsedd att komma igång utveckling av program med de senaste utvecklingen versionerna av ASP.NET Core. Följande instruktioner är specifika för Windows. Installationsinstruktioner för OS X, Linux och Windows, se [komma igång med ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started). 


> [!NOTE]
> Mer detaljerad Installationsinstruktioner för OS X, Linux och Windows finns [installera ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx). 
> 
> 

## <a name="create-the-web-app"></a>Skapa webbappen
Det här avsnittet visar hur du Autogenerera en ny app ASP.NET webbapp med hjälp av verktyget .NET CLI. 

1. Skriv följande i Kommandotolken för att skapa projektmappen och Autogenerera appen.
   
```terminal
mkdir SampleWebApp
cd SampleWebApp
dotnet new mvc
```
![DotNet CLI - ASP.NET Core generator](./media/web-sites-create-web-app-using-vscode/dotnetcore-mvc-01.png)

2. Kör följande kommando för att återställa nödvändiga NuGet-paket:
   
    ```terminal
    dotnet restore
    ```

## <a name="run-the-web-app-locally"></a>Kör webbappen lokalt
Nu när du har skapat webbappen och hämta alla NuGet-paket för appen, kan du köra webbappen lokalt.

1. Kör appen (den `dotnet run` kommando skapar appen när den är för gammal):
    ```terminal
    dotnet run
    ```
2. Öppna en webbläsare och gå till följande URL.
   
    **http://localhost:5000**
   
    Webbprogrammet standardsida visas på följande sätt.
   
    ![Lokala webbprogram i en webbläsare](./media/web-sites-create-web-app-using-vscode/08-web-app.png)
3. Stäng webbläsaren. I den **kommandofönstret**, tryck på **Ctrl + C** att stänga av programmet och Stäng den **kommandofönstret**. 

## <a name="create-a-web-app-in-the-azure-portal"></a>Skapa en webbapp i Azure Portal
Följande steg hjälper dig att skapa en webbapp i Azure Portal.

1. Logga in på [Azure Portal](https://portal.azure.com).
2. Klicka på **ny** längst upp till vänster i portalen.
3. Klicka på **Web Apps > Webbapp**.
   
    ![Nytt webbprogram för Azure](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)
4. Ange ett värde för **namn**, som **SampleWebAppDemo**. Observera att det här namnet måste vara unikt och portal fram som när du försöker ange namnet. Därför, om du väljer att ange ett annat värde du behöver ersätta värdet för varje förekomst av **SampleWebAppDemo** som visas i den här kursen. 
5. Välj en befintlig **Apptjänstplan** eller skapa en ny. Om du skapar en ny plan, Välj prisnivån, plats och andra alternativ. Mer information om apptjänstplaner finns i artikeln [Azure App Service-planer djupgående översikt över](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
    ![Azure web app-bladet](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)
6. Klicka på **Skapa**.
   
    ![Web app bladet](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Aktivera Git-publicering för den nya webbappen
Git är ett kontrollsystem för distribuerade versioner som du kan använda för att distribuera din webbapp i Azure App Service. Du lagrar koden du skriver för webbappen på en lokal Git-lagringsplats och du distribuerar koden till Azure genom att push-överföra den till en fjärransluten lagringsplats.   

1. Logga in på den [Azure-portalen](https://portal.azure.com).
2. Klicka på **Browse** (Bläddra).
3. Klicka på **Web Apps** att visa en lista över webbappar som är associerade med din Azure-prenumeration.
4. Välj webbprogram som du skapade i den här kursen.
5. I bladet webbapp, klickar du på **inställningar** > **kontinuerlig distribution**. 
   
    ![Azure web app värden](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)
6. Klicka på **Välj källa > lokal Git-lagringsplats**.
7. Klicka på **OK**.
   
    ![Azure lokal Git-lagringsplatsen](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)
8. Om du inte tidigare registrerat autentiseringsuppgifter för distribution för att publicera en webbapp eller andra Apptjänst-app, skapa dem nu:
   
   * Klicka på **inställningar** > **distributionsbehörigheterna**. Den **ange autentiseringsuppgifter för distribution** bladet visas.
   * Skapa ett användarnamn och lösenord.  Du behöver lösenordet senare när du ställer in Git.
   * Klicka på **Spara**.
9. I din webbapps blad klickar du på **Inställningar > Egenskaper**. URL till en fjärransluten Git-lagringsplats som du distribuerar till visas under **URL för GIT**.
10. Kopiera den **URL för GIT** värde för senare användning i självstudiekursen.
    
    ![Azure Git-URL](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a>Publicera webbappen till Azure App Service
I det här avsnittet skapas en lokal Git-lagringsplats och push från den lagringsplatsen till Azure för att distribuera webbappen till Azure.

1. I VS-kod, väljer du den **Git** alternativ i det vänstra navigeringsfältet.
   
    ![Git-ikonen i VS-kod](./media/web-sites-create-web-app-using-vscode/git-icon.png)
2. Välj **initiera git-lagringsplats** att säkerställa att din arbetsyta är git källkontroll. 
   
    ![Starta Git](./media/web-sites-create-web-app-using-vscode/19-initgit.png)
3. Öppna Kommandotolken och ändra sökvägen till katalogen för ditt webbprogram. Ange sedan följande kommando:
   
        git config core.autocrlf false
   
    Det här kommandot förhindrar att ett problem om text där CRLF ändelser och LF ändelser ingår.
4. Lägga till ett genomförande meddelande i VS-kod, och klicka på den **Commit alla** kryssikon.
   
    ![Git genomför alla](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)
5. När Git har bearbetat ser du att det inte finns några filer som visas i fönstret Git under **ändringar**. 
   
    ![Git inga ändringar](./media/web-sites-create-web-app-using-vscode/no-changes.png)
6. Ändra tillbaka till kommandofönstret där Kommandotolken pekar på katalogen där ditt webbprogram finns.
7. Skapa en fjärransluten referens för push-överföra uppdateringar till ditt webbprogram med hjälp av Git-URL (slutar med ”.git”) som du kopierade tidigare.
   
        git remote add azure [URL for remote repository]
8. Konfigurera Git för att spara dina autentiseringsuppgifter lokalt så att de automatiskt läggs till ditt push-kommandon som genereras från VS-kod.
   
        git config credential.helper store
9. Pusha ändringarna till Azure genom att ange följande kommando. Du kommer att kunna göra push-kommandon från VS kod efter den här första push till Azure. 
   
        git push -u azure master
   
    Du ombeds ange lösenordet du skapade tidigare i Azure. **Obs: Lösenordet visas inte.**
   
    Utdata från kommandot ovan slutar med ett meddelande om att distributionen har lyckats.
   
        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
        [new branch]      master -> master

> [!NOTE]
> Om du gör ändringar i din app kan du publicera direkt i VS kod med inbyggda funktioner för Git genom att välja den **genomför alla** alternativet följt av den **Push** alternativet. Du hittar den **Push** alternativ tillgängliga i den nedrullningsbara menyn bredvid den **genomför alla** och **uppdatera** knappar.
> 
> 

Om du behöver samarbeta kring ett projekt bör du sänder till GitHub between push-överföring till Azure.

## <a name="run-the-app-in-azure"></a>Kör appen i Azure
Nu när du har distribuerat ditt webbprogram kan vi köra appen medan finns i Azure. 

Detta kan göras på två sätt:

* Öppna en webbläsare och ange namnet på ditt webbprogram på följande sätt.   
  
        http://SampleWebAppDemo.azurewebsites.net
* Leta upp bladet webbapp för webbappen i Azure-portalen och på **Bläddra** att visa din app 
* i din standardwebbläsare.

![Azure-webbapp](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## <a name="summary"></a>Sammanfattning
I den här självstudiekursen beskrivs hur du skapar en webbapp i VS-kod och distribuerar den till Azure. Mer information om VS kod finns i artikeln [varför Visual Studio-koden?](https://code.visualstudio.com/Docs/) Information om App Service web apps finns [översikt över Web Apps](app-service-web-overview.md). 

