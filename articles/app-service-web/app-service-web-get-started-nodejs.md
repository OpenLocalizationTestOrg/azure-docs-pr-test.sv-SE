---
title: aaaCreate en Node.js-webbapp i Azure | Microsoft Docs
description: "Distribuera din första Node.js-Hello World-app i Azure App Service Web Apps på bara några minuter."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 05/05/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 163edf83b2353755fc9fa2d75aed489038cf7c81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-nodejs-web-app-in-azure"></a>Skapa en Node.js-webbapp i Azure

Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.  Den här snabbstarten visar hur toodeploy en Node.js-app tooAzure Web Apps. Du skapar hello webbapp med hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), och du kan använda Git toodeploy Node.js-kod toohello exempelwebbapp.

![Exempelapp som körs i Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

Du kan följa hello stegen nedan använder en Mac, Windows eller Linux-dator. När hello nödvändiga komponenter har installerats, tar cirka fem minuter toocomplete hello steg.   

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-Node-Developers/Create-a-Nodejs-app-in-Azure-Quickstart/player]   


## <a name="prerequisites"></a>Krav

toocomplete denna Snabbstart:

* [Installera Git](https://git-scm.com/)
* [Installera Node.js och NPM](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="download-hello-sample"></a>Hämta hello-exempel

Kör hello efter kommandot tooclone hello exempel app databasen tooyour lokala datorn i ett terminalfönster.

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

Du använder den här terminalfönster toorun alla hello-kommandon i denna Snabbstart.

Ändra toohello katalog som innehåller hello exempelkod.

```bash
cd nodejs-docs-hello-world
```

## <a name="run-hello-app-locally"></a>Kör hello appen lokalt

Kör programmet hello lokalt genom att öppna ett terminalfönster och hello `npm start` skriptet toolaunch hello inbyggda Node.js HTTP-servern.

```bash
npm start
```

Öppna en webbläsare och gå toohello sample-appen på http://localhost: 1337.

Du ser hello **Hello World** meddelande från hello sample-appen visas i hello-sidan.

![Exempelapp som körs lokalt](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

Tryck på i ditt terminalfönster **Ctrl + C** tooexit hello-webbserver.

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Sida för tom webbapp](media/app-service-web-get-started-php/app-service-web-service-created.png)

Nu har du skapat en ny tom webbapp på Azure.

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 23, done.
Delta compression using up too4 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (23/23), 3.71 KiB | 0 bytes/s, done.
Total 23 (delta 8), reused 7 (delta 1)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'bf114df591'.
remote: Generating deployment script.
remote: Generating deployment script for node.js Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling node.js deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.js'
remote: Copying file: 'package.json'
remote: Copying file: 'process.json'
remote: Deleting file: 'hostingstart.html'
remote: Ignoring: .git
remote: Using start-up script index.js from package.json.
remote: Node.js versions available on hello platform are: 4.4.7, 4.5.0, 6.2.2, 6.6.0, 6.9.1.
remote: Selected node.js version 6.9.1. Use package.json file toochoose a different version.
remote: Selected npm version 3.10.8
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net:443/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-toohello-app"></a>Bläddra toohello app

Bläddra toohello distribuerat program med hjälp av webbläsaren.

```bash
http://<app_name>.azurewebsites.net
```

Hej Node.js exempelkod körs i en webbapp i Azure App Service.

![Exempelapp som körs i Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

**Grattis!** Du har distribuerat din första Node.js app tooApp Service.

## <a name="update-and-redeploy-hello-code"></a>Uppdatera och distribuera hello kod

Använd en textredigerare och öppna hello `index.js` filen i hello Node.js-app och göra en mindre ändring toohello text i hello anrop för`response.end`:

```nodejs
response.end("Hello Azure!");
```

Genomför ändringarna i Git och skicka sedan hello kod ändringar tooAzure.

```bash
git commit -am "updated output"
git push azure master
```

När distributionen är klar, kan du växla tillbaka toohello webbläsarfönster som öppnas i hello **Bläddra toohello app** steg och klicka på Uppdatera.

![Uppdaterad exempelapp som körs i Azure](media/app-service-web-get-started-nodejs-poc/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Hantera din nya Azure-webbapp

Gå toohello <a href="https://portal.azure.com" target="_blank">Azure-portalen</a> toomanage hello webbprogram som du skapade.

Hello vänstra menyn klickar du på **Apptjänster**, och klicka sedan på hello namnet på din Azure webbapp.

![Portalen navigering tooAzure webbprogram](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Nu visas sidan Översikt för din webbapp. Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort. 

![App Service-blad på Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

hello vänstra menyn innehåller olika sidor för att konfigurera din app. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Node.js med MongoDB](app-service-web-tutorial-nodejs-mongodb-app.md)
