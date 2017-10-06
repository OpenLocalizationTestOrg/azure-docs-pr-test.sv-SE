---
title: aaaCreate en PHP-webbapp i Azure | Microsoft Docs
description: "Distribuera din första Hello World-app i PHP i Azure App Service Web Apps på bara några minuter."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/21/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 8e1022889ca162f8f15ce7435cc9393cc6efef06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-php-web-app-in-azure"></a>Skapa en PHP-webbapp i Azure

Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.  Den här snabbstartsguide visar hur toodeploy en PHP-app tooAzure Web Apps. Du skapar hello webbapp med hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) i molnet Shell, och du kan använda Git toodeploy PHP kod toohello exempelwebbapp.

![Sample app running in Azure]](media/app-service-web-get-started-php/hello-world-in-browser.png)

Du kan följa hello stegen nedan använder en Mac, Windows eller Linux-dator. När hello nödvändiga komponenter har installerats, tar cirka fem minuter toocomplete hello steg.

## <a name="prerequisites"></a>Krav

toocomplete denna Snabbstart:

* [Installera Git](https://git-scm.com/)
* [Installera PHP](https://php.net)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample-locally"></a>Hämta hello exempel lokalt

Kör hello följande kommandon i ett terminalfönster. Detta klona hello exempel programmet tooyour lokala datorn och navigera toohello katalog som innehåller hello exempelkod.

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
cd php-docs-hello-world
```

## <a name="run-hello-app-locally"></a>Kör hello appen lokalt

Kör programmet hello lokalt genom att öppna ett terminalfönster och hello `php` kommandot toolaunch hello inbyggda PHP webbservern.

```bash
php -S localhost:8080
```

Öppna en webbläsare och gå toohello sample-appen på http://localhost: 8080.

Du ser hello **Hello World!** meddelande från hello sample-appen visas i hello-sidan.

![Exempelapp som körs lokalt](media/app-service-web-get-started-php/localhost-hello-world-in-browser.png)

Tryck på i ditt terminalfönster **Ctrl + C** tooexit hello-webbserver.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)]

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)]

![Sida för tom webbapp](media/app-service-web-get-started-php/app-service-web-service-created.png)

Nu har du skapat en ny tom webbapp på Azure.

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 2, done.
Delta compression using up too4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 352 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '25f18051e9'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.php'
remote: Ignoring: .git
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
   cc39b1e..25f1805  master -> master
```

## <a name="browse-toohello-app-locally"></a>Bläddra toohello appen lokalt

Bläddra toohello distribuerat program med hjälp av webbläsaren.

```bash
http://<app_name>.azurewebsites.net
```

hello PHP exempelkod körs i en webbapp i Azure App Service.

![Exempelapp som körs i Azure](media/app-service-web-get-started-php/hello-world-in-browser.png)

**Grattis!** Du har distribuerat din första PHP app tooApp Service.

## <a name="update-locally-and-redeploy-hello-code"></a>Uppdatera lokalt och omdistribuera hello kod

Med en lokal textredigerare öppna hello `index.php` filen i hello PHP-app och göra en mindre ändring toohello texten inom hello sträng bredvid för`echo`:

```php
echo "Hello Azure!";
```

Genomför ändringarna i Git och skicka sedan hello kod ändringar tooAzure.

```bash
git commit -am "updated output"
git push azure master
```

När distributionen är klar, kan du växla tillbaka toohello webbläsarfönster som öppnas i hello **Bläddra toohello app** steg och uppdatera hello sidan.

![Uppdaterad exempelapp som körs i Azure](media/app-service-web-get-started-php/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Hantera din nya Azure-webbapp

Gå toohello <a href="https://portal.azure.com" target="_blank">Azure-portalen</a> toomanage hello webbprogram som du skapade.

Hello vänstra menyn klickar du på **Apptjänster**, och klicka sedan på hello namnet på din Azure webbapp.

![Portalen navigering tooAzure webbprogram](./media/app-service-web-get-started-php/php-docs-hello-world-app-service-list.png)

Nu visas sidan Översikt för din webbapp. Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.

![App Service-blad på Azure Portal](media/app-service-web-get-started-php/php-docs-hello-world-app-service-detail.png)

hello vänstra menyn innehåller olika sidor för att konfigurera din app. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [PHP med MySQL](app-service-web-tutorial-php-mysql.md)
