---
title: aaaCreate en Ruby-App med Webbappar i Linux | Microsoft Docs
description: "Läs toocreate Ruby appar med Azure web wpp på Linux."
keywords: "Azure apptjänst, linux, oss, ruby"
services: app-service
documentationcenter: 
author: wesmc7777
manager: erikre
editor: 
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: wesmc;rachelap
ms.openlocfilehash: 99ce3b5ee16703a147787387bb02973defce8190
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-ruby-app-with-web-apps-on-linux"></a>Skapa en Ruby App med Webbappar i Linux 

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst. Den här snabbstarten visar hur toocreate en grundläggande Ruby på spår programmet du sedan distribuera den tooAzure som ett webbprogram på Linux.

![Hello world](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

## <a name="prerequisites"></a>Krav

* [Ruby 2.4.1 eller högre](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).
* [Git](https://git-scm.com/downloads).
* En [aktiv Azure-prenumeration](https://azure.microsoft.com/pricing/free-trial/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample"></a>Hämta hello-exempel

Kör hello efter kommandot tooclone hello exempel app databasen tooyour lokala datorn i ett terminalfönster:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="run-hello-application-locally"></a>Kör hello programmet lokalt

Köra hello spår server för hello programmet toowork. Ändra toohello *hello world* katalog och hello `rails server` kommando startar hello server.

```bash
cd hello-world\bin
rails server
```
    
Använd din webbläsare, navigera för`http://localhost:3000` tootest hello appen lokalt.  

![Hello world](./media/app-service-linux-ruby-get-started/hello-world.png)

## <a name="modify-app-toodisplay-welcome-message"></a>Ändra app toodisplay välkomstmeddelande

Ändra hello program så att det visar ett välkomstmeddelande. Ändra hello programmet domänkontrollant så att den returnerar hello-meddelande som HTML toohello webbläsare. 

Öppna *~/workspace/hello-world/app/controllers/application_controller.rb* för redigering. Ändra hello `ApplicationController` klassen toolook som hello följande kodexempel:

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception 
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

Appen är nu konfigurerad. Använd din webbläsare, navigera för`http://localhost:3000` Landningssida för tooconfirm hello rot.

![Hello World konfigurerad](./media/app-service-linux-ruby-get-started/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-ruby-web-app-on-azure"></a>Skapa en Ruby webbapp på Azure

Använd hello [az programtjänstplan skapa](https://docs.microsoft.com/cli/azure/appservice/plan#create) kommandot toocreate en apptjänstplan för ditt webbprogram. 
 
```azurecli-interactive
  az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

Därefter utfärda hello [az webapp skapa](https://docs.microsoft.com/cli/azure/webapp) kommandot toocreate hello med webbprogram som använder hello nyskapad service-plan. Observera att hello-körning har angetts för`ruby|2.3`. Glöm inte tooreplace `<app name>` med ett unikt appnamn.

```azurecli-interactive
  az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --runtime "ruby|2.3" --deployment-local-git
```

När hello webbprogram skapas en **översikt** är tillgängliga tooview. Navigera tooit. hello följande välkomstskärmen sida visas:

![Välkomstskärmen sida](./media/app-service-linux-ruby-get-started/splash-page.png)


## <a name="deploy-your-application"></a>Distribuera programmet

Använda Git toodeploy hello Ruby programmet tooAzure. hello webbprogrammet har redan en Git-distribution som har konfigurerats. Du kan hämta hello distributionens URL genom att utfärda ett [az webapp distribution](https://docs.microsoft.com/cli/azure/webapp/deployment) kommando.  

```bash
az webapp deployment source show --name <app name> --resource-group myResourceGroup
```

Observera att hello URL för Git har hello följande baserat på din webbprogrammets namn:

```bash
https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
```

[!INCLUDE [Clean-up section](../../includes/configure-deployment-user-no-h.md)]

Kör följande kommandon toodeploy hello lokalt program tooyour Azure-webbplatsen hello:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Bekräfta att hello remote distributionsåtgärder rapporterar lyckades. hello kommandon producerar utdata liknande toohello följande text:

```bash
remote: Using sass-rails 5.0.6
remote: Updating files in vendor/cache
remote: Bundle gems are installed into ./vendor/bundle
remote: Updating files in vendor/cache
remote: ~site/repository
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<your web app name>.scm.azurewebsites.net/<your web app name>.git
  579ccb....2ca5f31  master -> master
myuser@ubuntu1234:~workspace/<app name>$
```

När hello distributionen är klar startar du om ditt webbprogram för hello distribution tootake effekt med hjälp av hello [az webapp omstart](https://docs.microsoft.com/cli/azure/webapp#restart) kommandot som visas här:

```azurecli-interactive 
az webapp restart --name <app name> --resource-group myResourceGroup
```

Navigera tooyour plats och kontrollera hello resultat.

```bash
http://<your web app name>.azurewebsites.net
```
![uppdaterade för webbprogram](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

> [!NOTE]
> Medan hello appen startas försöker toobrowse hello plats resulterar i en HTTP-statuskod `Error 503 Server unavailable`. Det kan ta några minuter toofully omstart.
>

[!INCLUDE [Clean-up section](../../includes/cli-script-clean-up.md)]


## <a name="next-steps"></a>Nästa steg

[Azure App Service Webbapp på Linux vanliga frågor och svar](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)