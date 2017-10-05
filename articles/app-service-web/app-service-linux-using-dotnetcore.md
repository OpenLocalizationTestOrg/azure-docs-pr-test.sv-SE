---
title: "Använda .NET Core i webbprogrammet på Linux | Microsoft Docs"
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
ms.openlocfilehash: 9226dfb90e52ac2cae2cfc4af7c0705a93f56b44
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-net-core-in-an-azure-web-app-on-linux"></a>Använda .NET Core i en Azure-webbapp på Linux #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

[Webbapp](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) på Linux ger en mycket skalbar, automatisk uppdatering värdtjänst med Linux-operativsystem. Den här självstudiekursen innehåller stegvisa instruktioner för hur du skapar en [.NET Core](https://docs.microsoft.com/aspnet/core/) app på Azure-webbapp på Linux. 

![Webbapp i Linux][10]

Du kan följa stegen nedan på en Mac-, Windows- eller Linux-dator.

## <a name="prerequisites"></a>Krav ##

För att slutföra den här kursen behöver du: 

* Installera den [.NET Core SDK](https://www.microsoft.com/net/download/core).
* Installera [Git](https://git-scm.com/downloads).

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-local-net-core-application"></a>Skapa ett lokalt program i .NET Core ##

Starta en ny terminalsession. Skapa en katalog med namnet `hellodotnetcore`, och ändra katalogen till den. Skriv följande: 

```
dotnet new web
``` 

  Det här kommandot skapar tre filer (*hellodotnetcore.csproj*, *Program.cs*, och *Startup.cs*) och en tom mapp (*wwwroot /*) under den aktuella katalogen. Innehållet i `.csproj` filen bör se ut ungefär så här: 

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

Eftersom den här appen är ett webbprogram, en referens till ett ASP.NET Core-paket har lagts till automatiskt i *hellodotnetcore.csproj* fil. Versionsnummer för paketet anges enligt den valda framework. Det här exemplet refererar till ASP.NET Core version 1.1.2 eftersom .NET Core 1.1 används.

## <a name="build-and-test-the-application-locally"></a>Skapa och testa programmet lokalt ##

Du kan skapa och köra appen med .NET Core den `dotnet restore` kommandot följt av den `dotnet run` kommandot som visas här:

```
dotnet restore
dotnet run
```


När programmet startas, visar ett meddelande om appen lyssnar på inkommande begäranden på en port. 

```bash
Hosting environment: Production
Content root path: C:\hellodotnetcore
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

Testa den genom att bläddra till `http://localhost:5000/` med din webbläsare. Om allt fungerar bra visas ”Hello World”! som resultattext.

![Testa med webbläsare][7]

## <a name="create-a-net-core-app-in-the-azure-portal"></a>Skapa ett .NET Core app i Azure-portalen ##

Du måste först skapa ett tomt webbprogram. Logga in på den [Azure-portalen](https://portal.azure.com/) och skapa en ny [webbprogrammet på Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).

![Skapa en webbapp][1]

När den **skapa** öppnas, innehåller information om ditt webbprogram:

![Om du väljer en .NET Core runtime stack][2]

Använd följande tabell som en vägledning för att fylla i den **skapa** sidan och välj sedan **OK** och **skapa** att skapa appen.

| Inställning      | Föreslaget värde  | Beskrivning                                        |
| ------------ | ---------------- | -------------------------------------------------- |
| Appnamn | hellodotnetcore  | Namnet på din app. Det här namnet måste vara unikt. |
| Prenumeration | Välj en befintlig prenumeration | Azure-prenumeration. |
| Resursgrupp | myResourceGroup |  Azure resursgruppen som innehåller webbappen. |
| App Service-plan | Namn på befintliga App Service-Plan |  App Service-plan.  |
| Konfigurera behållare | .NET core 1.1 | Typ av behållare för det här webbprogrammet: inbyggda, Docker eller privata registret. |
| Bildens källa  | Inbyggd  |  Bildens källa. |
| Runtime-stacken  | .NET core 1.1  | Runtime-stacken och version.  |

## <a name="deploy-your-application-via-git"></a>Distribuera programmet via Git ##

Använda Git för att distribuera programmet till Azure App Service Web App på Linux .NET Core.

Nya Azure-webbappen har redan konfigurerats för Git-distribution. Du hittar URL för Git-distribution genom att navigera till följande URL när du har infogat din webbprogrammets namn:

```https://{your web app name}.scm.azurewebsites.net/api/scm/info```

Git-URL har följande format baserat på din webbprogrammets namn:

```https://{your web app name}.scm.azurewebsites.net/{your web app name}.git```

Kör följande kommandon för att distribuera lokala programmet till Azure webbapp: 
 
```bash
git init
git remote add azure <Git deployment URL from above>
git add *.csproj *.cs
git commit -m "Initial deployment commit"
git push azure master
```

Du behöver inte push alla filer under *bin /* eller *obj /* kataloger eftersom programmet är inbyggd i molnet när programmets källfiler pushas till Azure. När build-processen är klar, binära filer kopieras till programmets katalog vid */home/plats/wwwroot/*.

Bekräfta att fjärråtkomst distributionsåtgärder rapporterar lyckades. Push-åtgärder kan ta ett tag sedan paketet upplösning och skapa process som körs i molnet. Du ser flera statusmeddelanden, inklusive de som anger att filerna har kopierats. Resultatet bör likna följande:

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
To https://hellodotnetcore.scm.azurewebsites.net/
 * [new branch]           master -> master

```

När distributionen är klar startar du om ditt webbprogram för att distributionen ska börja gälla. Gör du genom att gå till Azure-portalen och gå till den **översikt** sidan av ditt webbprogram. Välj den **starta om** knappen på sidan. När ett popup-fönster visas, väljer du **Ja** att bekräfta. Du kan sedan bläddra ditt webbprogram som visas här:

![.NET Core app distribueras till Azure App Service på Linux-surfning][10]

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nästa steg
* [Azure App Service Webbapp på Linux vanliga frågor och svar](./app-service-linux-faq.md)

[1]: ./media/app-service-linux-using-dotnetcore/top-level-create.png
[2]: ./media/app-service-linux-using-dotnetcore/dotnet-new-webapp.png
[7]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-local.png
[10]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-azure.png
