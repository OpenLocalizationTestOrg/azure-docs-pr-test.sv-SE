---
title: aaaCreate statiska HTML-webbapp i Azure | Microsoft Docs
description: "Lär dig hur toorun web apps i Azure App Service genom att distribuera en statisk HTML sample-appen."
services: app-service\web
documentationcenter: 
author: rick-anderson
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 05/26/2017
ms.author: riande
ms.custom: mvc
ms.openlocfilehash: efd8c8189a3aa1ac35602b688eeb31bff6f5a373
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-static-html-web-app-in-azure"></a>Skapa en statisk HTML-webbapp i Azure

Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.  Den här snabbstarten visar hur toodeploy grundläggande HTML + CSS plats tooAzure Web Apps. Du skapar hello webbapp med hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), och du kan använda Git toodeploy HTML-innehåll toohello exempelwebbapp.

![Startsida för exempelapp](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

Du kan följa hello stegen nedan använder en Mac, Windows eller Linux-dator. När hello nödvändiga komponenter har installerats, tar cirka fem minuter toocomplete hello steg.

## <a name="prerequisites"></a>Krav

toocomplete denna Snabbstart:

- [Installera Git](https://git-scm.com/)
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="download-hello-sample"></a>Hämta hello-exempel

Kör hello efter kommandot tooclone hello exempel app databasen tooyour lokala datorn i ett terminalfönster.

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

Du använder den här terminalfönster toorun alla hello-kommandon i denna Snabbstart.

## <a name="view-hello-html"></a>Visa hello HTML

Navigera toohello katalog som innehåller exempel på hello HTML. Öppna hello *index.html* filen i din webbläsare.

![Exempelstartsida för app](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Sida för tom webbapp](media/app-service-web-get-started-html/app-service-web-service-created.png)

Nu har du skapat en ny tom webbapp på Azure.

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 13, done.
Delta compression using up too4 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (13/13), 2.07 KiB | 0 bytes/s, done.
Total 13 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'cc39b1e4cb'.
remote: Generating deployment script.
remote: Generating deployment script for Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-toohello-app"></a>Bläddra toohello app

Gå toohello Azure web app-URL i en webbläsare:

```
http://<app_name>.azurewebsites.net
```

hello sidan körs som en webbapp i Azure App Service.

![Exempelstartsida för app](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

**Grattis!** Du har distribuerat din första HTML-app tooApp Service.

## <a name="update-and-redeploy-hello-app"></a>Uppdatera och distribuera hello app

Öppna hello *index.html* filen i en textredigerare och gör en ändring toohello markeringar. Ändra exempelvis hello H1 rubriken ”Azure App Service – statiska HTML-Exempelplats” toojust ”Azure App Service”.

Genomför ändringarna i Git och skicka sedan hello kod ändringar tooAzure.

```bash
git commit -am "updated HTML"
git push azure master
```

Uppdatera din webbläsare toosee hello ändringar när distributionen är klar.

![Uppdaterad exempelstartsida för app](media/app-service-web-get-started-html/hello-azure-in-browser-az.png)

## <a name="manage-your-new-azure-web-app"></a>Hantera din nya Azure-webbapp

Gå toohello <a href="https://portal.azure.com" target="_blank">Azure-portalen</a> toomanage hello webbprogram som du skapade.

Hello vänstra menyn klickar du på **Apptjänster**, och klicka sedan på hello namnet på din Azure webbapp.

![Portalen navigering tooAzure webbprogram](./media/app-service-web-get-started-html/portal1.png)

Nu visas sidan Översikt för din webbapp. Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort. 

![App Service-blad på Azure Portal](./media/app-service-web-get-started-html/portal2.png)

hello vänstra menyn innehåller olika sidor för att konfigurera din app. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Mappa anpassad domän](app-service-web-tutorial-custom-domain.md)
