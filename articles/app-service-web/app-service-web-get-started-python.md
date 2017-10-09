---
title: aaaCreate en Python-webbapp i Azure | Microsoft Docs
description: "Distribuera din första Hello World-app med Python i Azure App Service Web Apps på bara några minuter."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 928ee2e5-6143-4c0c-8546-366f5a3d80ce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/17/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 42178d490d8aa8eaf93710667aad598794c62c8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-python-web-app-in-azure"></a>Skapa en Python-webbapp i Azure

Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.  Denna Snabbstart går igenom hur toodevelop och distribuera en Python app tooAzure Web Apps. Du skapar hello webbapp med hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), och du kan använda Git toodeploy Python-kod toohello exempelwebbapp.

![Exempelapp som körs i Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

Du kan följa hello stegen nedan använder en Mac, Windows eller Linux-dator. När hello nödvändiga komponenter har installerats, tar cirka fem minuter toocomplete hello steg.
## <a name="prerequisites"></a>Krav

toocomplete den här kursen:

1. [Installera Git](https://git-scm.com/)
1. [Installera Python](https://www.python.org/downloads/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="download-hello-sample"></a>Hämta hello-exempel

Kör hello efter kommandot tooclone hello exempel app databasen tooyour lokala datorn i ett terminalfönster.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Du använder den här terminalfönster toorun alla hello-kommandon i denna Snabbstart.

Ändra toohello katalog som innehåller hello exempelkod.

```bash
cd Python-docs-hello-world
```

## <a name="run-hello-app-locally"></a>Kör hello appen lokalt

Installera hello krävs paket med hjälp av `pip`.

```bash
pip install -r requirements.txt
```

Kör programmet hello lokalt genom att öppna ett terminalfönster och hello `Python` kommandot toolaunch hello inbyggda Python webbservern.

```bash
python main.py
```

Öppna en webbläsare och gå toohello exempelapp på http://localhost:5000.

Du kan se hello **Hello World** meddelande från hello sample-appen visas i hello-sidan.

![Exempelapp som körs lokalt](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

Tryck på i ditt terminalfönster **Ctrl + C** tooexit hello-webbserver.

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Sida för tom webbapp](media/app-service-web-get-started-python/app-service-web-service-created.png)

Nu har du skapat en ny tom webbapp på Azure.

## <a name="configure-toouse-python"></a>Konfigurera toouse Python

Använd hello [az webapp konfigurationsuppsättning](/cli/azure/webapp/config#set) kommandot tooconfigure hello app toouse Python webbversionen `3.4`.

```azurecli-interactive
az webapp config set --python-version 3.4 --name <app_name> --resource-group myResourceGroup
```


Ange hello Python-versionen sätt använder en standardbehållare som tillhandahålls av hello-plattformen. toouse egna behållaren finns hello CLI-referens för hello [az webapp konfigurationsuppsättning behållaren](/cli/azure/webapp/config/container#set) kommando.

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 18, done.
Delta compression using up too4 threads.
Compressing objects: 100% (16/16), done.
Writing objects: 100% (18/18), 4.31 KiB | 0 bytes/s, done.
Total 18 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '44e74fe7dd'.
remote: Generating deployment script.
remote: Generating deployment script for python Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling python deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'main.py'
remote: Copying file: 'README.md'
remote: Copying file: 'requirements.txt'
remote: Copying file: 'virtualenv_proxy.py'
remote: Copying file: 'web.2.7.config'
remote: Copying file: 'web.3.4.config'
remote: Detected requirements.txt.  You can skip Python specific steps with a .skipPythonDeployment file.
remote: Detecting Python runtime from site configuration
remote: Detected python-3.4
remote: Creating python-3.4 virtual environment.
remote: .................................
remote: Pip install requirements.
remote: Successfully installed Flask click itsdangerous Jinja2 Werkzeug MarkupSafe
remote: Cleaning up...
remote: .
remote: Overwriting web.config with web.3.4.config
remote:         1 file(s) copied.
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-toohello-app"></a>Bläddra toohello app

Bläddra toohello distribuerat program med hjälp av webbläsaren.

```bash
http://<app_name>.azurewebsites.net
```

hello Python exempelkod körs i en webbapp i Azure App Service.

![Exempelapp som körs i Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

**Grattis!** Du har distribuerat din första Python app tooApp Service.

## <a name="update-and-redeploy-hello-code"></a>Uppdatera och distribuera hello kod

Med en lokal textredigerare öppna hello `main.py` filen i hello Python-appen och göra en mindre ändring toohello text nästa toohello `return` instruktionen:

```python
return 'Hello, Azure!'
```

Genomför ändringarna i Git och skicka sedan hello kod ändringar tooAzure.

```bash
git commit -am "updated output"
git push azure master
```

När distributionen är klar, kan du växla tillbaka toohello webbläsarfönster som öppnas i hello [Bläddra toohello app](#browse-to-the-app) steg och uppdatera hello sidan.

![Uppdaterad exempelapp som körs i Azure](media/app-service-web-get-started-python/hello-azure-in-browser.png)

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
> [Python med PostgreSQL](app-service-web-tutorial-docker-python-postgresql-app.md)
