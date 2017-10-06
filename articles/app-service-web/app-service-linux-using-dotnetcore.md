---
title: "aaaUse .NET Core i webbprogrammet på Linux | Microsoft Docs"
description: "Använd .NET Core i webbprogrammet på Linux."
keywords: "Azure apptjänst, webbprogram, dotnet, core, linux, oss"
services: app-service
documentationCenter: 
authors: michimune, rachelappel
manager: erikre
editor: 
ms.assetid: c02959e6-7220-496a-a417-9b2147638e2e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: aelnably;wesmc;mikono;rachelap
ms.openlocfilehash: 9b7fb7185dff2c99ed88e7937d455177504937b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-net-core-in-an-azure-web-app-on-linux"></a>Använda .NET Core i en Azure-webbapp på Linux #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

[Webbapp](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) på Linux innehåller en mycket skalbar, automatisk uppdatering värd webbtjänsten använder hello Linux-operativsystem. Den här självstudiekursen innehåller stegvisa instruktioner som visar hur toocreate en [.NET Core](https://docs.microsoft.com/aspnet/core/) app på Azure-webbapp på Linux. 

![Webbapp i Linux][10]

Du kan följa hello stegen nedan använder en Mac, Windows eller Linux-dator.

## <a name="prerequisites"></a>Krav ##

toocomplete den här kursen: 

* Installera hello [.NET Core SDK](https://www.microsoft.com/net/download/core).
* Installera [Git](https://git-scm.com/downloads).

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-local-net-core-application"></a>Skapa ett lokalt program i .NET Core ##

Starta en ny terminalsession. Skapa en katalog med namnet `hellodotnetcore`, och ändra hello aktuella directory tooit. Skriv hello följande: 

```
dotnet new web
``` 

  Det här kommandot skapar tre filer (*hellodotnetcore.csproj*, *Program.cs*, och *Startup.cs*) och en tom mapp (*wwwroot /*) under hello aktuell katalog. innehållet i hello `.csproj` filen bör se ut hello följande: 

```xml
  <!-- Empty lines are omitted. -->

  <Project Sdk="Microsoft.NET.Sdk.Web">
        <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        </PropertyGroup>
        <ItemGroup>
        <Folder Include="wwwroot\" />
        </ItemGroup>
        <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore" Version="1.1.2" />
        </ItemGroup>
  </Project>
```

Eftersom den här appen är ett webbprogram, läggs en referens tooan ASP.NET Core-paketet har automatiskt toohello *hellodotnetcore.csproj* fil. hello versionsnumret för hello paketet anges enligt toohello valt framework. Det här exemplet refererar till ASP.NET Core version 1.1.2 eftersom .NET Core 1.1 används.

## <a name="build-and-test-hello-application-locally"></a>Skapa och testa hello programmet lokalt ##

Du kan skapa och köra appen .NET Core med hello `dotnet restore` kommandot följt av hello `dotnet run` kommandot som visas här:

```
dotnet restore
dotnet run
```


När programmet hello startar visar ett meddelande om hello app lyssnar tooincoming begäranden på en port. 

```bash
Hosting environment: Production
Content root path: C:\hellodotnetcore
Now listening on: http://localhost:5000
Application started. Press Ctrl+C tooshut down.
```

Testa den genom att bläddra för`http://localhost:5000/` med din webbläsare. Om allt fungerar bra visas ”Hello World”! som hello resultattext.

![Testa med webbläsare][7]

## <a name="create-a-net-core-app-in-hello-azure-portal"></a>Skapa en .NET Core-app i hello Azure-portalen ##

Du måste först toocreate ett tomt webbprogram. Logga in toohello [Azure-portalen](https://portal.azure.com/) och skapa en ny [webbprogrammet på Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).

![Skapa en webbapp][1]

När hello **skapa** öppnas, innehåller information om ditt webbprogram:

![Om du väljer en .NET Core runtime stack][2]

Använd hello följande tabell som en guide toofill ut hello **skapa** sidan och välj sedan **OK** och **skapa** toocreate hello app.

| Inställning      | Föreslaget värde  | Beskrivning                                        |
| ------------ | ---------------- | -------------------------------------------------- |
| Appnamn | hellodotnetcore  | hello namnet på din app. Det här namnet måste vara unikt. |
| Prenumeration | Välj en befintlig prenumeration | hello Azure-prenumeration. |
| Resursgrupp | myResourceGroup |  hello Azure-resurs grupp toocontain hello webbapp. |
| App Service-plan | Namn på befintliga App Service-Plan |  hello App Service-plan.  |
| Konfigurera behållare | .NET core 1.1 | Hej typ av behållare för det här webbprogrammet: inbyggda, Docker eller privata registret. |
| Bildens källa  | Inbyggd  |  hello källa hello avbildningen. |
| Runtime-stacken  | .NET core 1.1  | hello runtime-stacken och version.  |

## <a name="deploy-your-application-via-git"></a>Distribuera programmet via Git ##

Använda Git toodeploy hello .NET Core programmet tooAzure App Service Web App på Linux.

hello nya Azure-webbapp har redan konfigurerats Git-distribution. Du hittar hello URL för Git-distribution genom att gå toohello följande URL när du har infogat din webbprogrammets namn:

```https://{your web app name}.scm.azurewebsites.net/api/scm/info```

hello URL för Git har hello följande baserat på din webbprogrammets namn:

```https://{your web app name}.scm.azurewebsites.net/{your web app name}.git```

Kör följande kommandon toodeploy hello lokalt program tooyour Azure webbapp hello: 
 
```bash
git init
git remote add azure <Git deployment URL from above>
git add *.csproj *.cs
git commit -m "Initial deployment commit"
git push azure master
```

Du behöver inte toopush alla filer under *bin /* eller *obj /* kataloger eftersom programmet är inbyggd i molnet hello när hello programmets tooAzure push-källfiler. När hello build-processen är klar, binära filer kopieras till hello tillämpningsprogrammets katalog på */home/plats/wwwroot/*.

Bekräfta att hello remote distributionsåtgärder rapporterar lyckades. Push-åtgärder kan ta ett tag sedan paketet upplösning och skapa processen genom att köra i hello molnet. Du ser flera statusmeddelanden, inklusive de som anger att filerna har kopierats. hello utdata ska se ut ungefär toohello följande:

```bash
/* some output has been removed for brevity */
remote: Copying file: 'System.Net.Websockets.dll' 
remote: Copying file: 'System.Runtime.CompilerServices.Unsafe.dll' 
remote: Copying file: 'System.Runtime.Serialization.Primitives.dll' 
remote: Copying file: 'System.Text.Encodings.Web.dll' 
remote: Copying file: 'hellodotnetcore.deps.json' 
remote: Copying file: 'hellodotnetcore.dll' 
remote: Omitting next output lines...
remote: Finished successfully.
remote: Running post deployment commands...
remote: Deployment successful.
toohttps://hellodotnetcore.scm.azurewebsites.net/
 * [new branch]           master -> master

```

Starta om ditt webbprogram för hello distribution tootake effekt när hello distributionen är klar. toodo kan gå toohello Azure-portalen och navigera toohello **översikt** sidan av ditt webbprogram. Välj hello **starta om** knappen hello på sidan. När ett popup-fönster visas, väljer du **Ja** tooconfirm. Du kan sedan bläddra ditt webbprogram som visas här:

![Bläddra .NET Core distribuerats appen tooAzure Apptjänst på Linux][10]

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nästa steg
* [Azure App Service Webbapp på Linux vanliga frågor och svar](./app-service-linux-faq.md)

[1]: ./media/app-service-linux-using-dotnetcore/top-level-create.png
[2]: ./media/app-service-linux-using-dotnetcore/dotnet-new-webapp.png
[7]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-local.png
[10]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-azure.png
