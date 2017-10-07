---
title: aaaNode.js API-app i Azure App Service | Microsoft Docs
description: "Lär dig hur toocreate en Node.js RESTful-API och distribuera den tooan API-app i Azure App Service."
services: app-service\api
documentationcenter: node
author: bradygaster
manager: erikre
editor: 
ms.assetid: a820e400-06af-4852-8627-12b3db4a8e70
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: get-started-article
ms.date: 06/13/2017
ms.author: rachelap
ms.openlocfilehash: 3b3229c1453b6ca4d06bef26f476e92afda4e244
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-restful-api-and-deploy-it-tooan-api-app-in-azure"></a><span data-ttu-id="099d1-103">Skapa en Node.js RESTful-API och distribuera den tooan API-app i Azure</span><span class="sxs-lookup"><span data-stu-id="099d1-103">Build a Node.js RESTful API and deploy it tooan API app in Azure</span></span>
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

<span data-ttu-id="099d1-104">Den här snabbstarten visar hur toocreate REST-API med Node.js [Express](http://expressjs.com/)med hjälp av en [Swagger](http://swagger.io/) definition och distribuera den som en [API-app](app-service-api-apps-why-best-platform.md) på Azure.</span><span class="sxs-lookup"><span data-stu-id="099d1-104">This quickstart shows how toocreate a REST API, written with Node.js [Express](http://expressjs.com/), using a [Swagger](http://swagger.io/) definition and deploying it as an [API app](app-service-api-apps-why-best-platform.md) on Azure.</span></span> <span data-ttu-id="099d1-105">Du kan skapa hello appen med hjälp av kommandoradsverktyg, konfigurera resurser med hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), och distribuera hello app med Git.</span><span class="sxs-lookup"><span data-stu-id="099d1-105">You create hello app using command-line tools, configure resources with hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and deploy hello app using Git.</span></span>  <span data-ttu-id="099d1-106">När du är klar har du ett fungerande exempel-REST-API som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="099d1-106">When you've finished, you have a working sample REST API running on Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="099d1-107">Krav</span><span class="sxs-lookup"><span data-stu-id="099d1-107">Prerequisites</span></span>

* [<span data-ttu-id="099d1-108">Git</span><span class="sxs-lookup"><span data-stu-id="099d1-108">Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="099d1-109">Node.js och NPM</span><span class="sxs-lookup"><span data-stu-id="099d1-109">Node.js and NPM</span></span>](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="099d1-110">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="099d1-110">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="099d1-111">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="099d1-111">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="099d1-112">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="099d1-112">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prepare-your-environment"></a><span data-ttu-id="099d1-113">Förbered din miljö</span><span class="sxs-lookup"><span data-stu-id="099d1-113">Prepare your environment</span></span>

1. <span data-ttu-id="099d1-114">Kör hello efter kommandot tooclone hello exempel tooyour lokala datorn i ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="099d1-114">In a terminal window, run hello following command tooclone hello sample tooyour local machine.</span></span>

    ```bash
    git clone https://github.com/Azure-Samples/app-service-api-node-contact-list
    ```

2. <span data-ttu-id="099d1-115">Ändra toohello katalog som innehåller hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="099d1-115">Change toohello directory that contains hello sample code.</span></span>

    ```bash
    cd app-service-api-node-contact-list
    ```

3. <span data-ttu-id="099d1-116">Installera [Swaggerize](https://www.npmjs.com/package/swaggerize-express) på din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="099d1-116">Install [Swaggerize](https://www.npmjs.com/package/swaggerize-express) on your local machine.</span></span> <span data-ttu-id="099d1-117">Swaggerize är ett verktyg som genererar Node.js-kod för REST-API från en Swagger-definition.</span><span class="sxs-lookup"><span data-stu-id="099d1-117">Swaggerize is a tool that generates Node.js code for your REST API from a Swagger definition.</span></span>

    ```bash
    npm install -g yo
    npm install -g generator-swaggerize
    ```

## <a name="generate-nodejs-code"></a><span data-ttu-id="099d1-118">Generera Node.js-kod</span><span class="sxs-lookup"><span data-stu-id="099d1-118">Generate Node.js code</span></span> 

<span data-ttu-id="099d1-119">Det här avsnittet av kursen hello modeller ett arbetsflöde för API-utveckling där du först skapar Swagger-metadata och använder den tooscaffold (Autogenerera) serverkod för hello API.</span><span class="sxs-lookup"><span data-stu-id="099d1-119">This section of hello tutorial models an API development workflow in which you create Swagger metadata first and use that tooscaffold (auto-generate) server code for hello API.</span></span> 

<span data-ttu-id="099d1-120">Ändra katalogen toohello *starta* mapp, kör `yo swaggerize`.</span><span class="sxs-lookup"><span data-stu-id="099d1-120">Change directory toohello *start* folder, then run `yo swaggerize`.</span></span> <span data-ttu-id="099d1-121">Swaggerize skapar en Node.js-projekt för din API från hello Swagger-definition i *api.json*.</span><span class="sxs-lookup"><span data-stu-id="099d1-121">Swaggerize creates a Node.js project for your API from hello Swagger definition in *api.json*.</span></span>

```bash
cd start
yo swaggerize --apiPath api.json --framework express
```

<span data-ttu-id="099d1-122">Använd *ContactList* när du uppmanas ange projektnamn.</span><span class="sxs-lookup"><span data-stu-id="099d1-122">When Swaggerize asks for a project name, use *ContactList*.</span></span>
   
   ```bash
   Swaggerize Generator
   Tell us a bit about your application
   ? What would you like toocall this project: ContactList
   ? Your name: Francis Totten
   ? Your github user name: fabfrank
   ? Your email: frank@fabrikam.net
   ```
   
## <a name="customize-hello-project-code"></a><span data-ttu-id="099d1-123">Anpassa hello Projektkod</span><span class="sxs-lookup"><span data-stu-id="099d1-123">Customize hello project code</span></span>

1. <span data-ttu-id="099d1-124">Kopiera hello *lib* till hello mappen *kontaktlista* mapp som skapats av `yo swaggerize`, ändra katalogen till *kontaktlista*.</span><span class="sxs-lookup"><span data-stu-id="099d1-124">Copy hello *lib* folder into hello *ContactList* folder created by `yo swaggerize`, then change directory into *ContactList*.</span></span>

    ```bash
    cp -r lib/ ContactList/
    cd ContactList
    ```

2. <span data-ttu-id="099d1-125">Installera hello `jsonpath` och `swaggerize-ui` NPM-modulerna.</span><span class="sxs-lookup"><span data-stu-id="099d1-125">Install hello `jsonpath` and `swaggerize-ui` NPM modules.</span></span> 

    ```bash
    npm install --save jsonpath swaggerize-ui
    ```

3. <span data-ttu-id="099d1-126">Ersätt hello koden i hello *handlers/contacts.js* med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="099d1-126">Replace hello code in hello *handlers/contacts.js* with hello following code:</span></span> 
    ```javascript
    'use strict';

    var repository = require('../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.all())
        }
    };
    ```
    <span data-ttu-id="099d1-127">Den här koden använder hello JSON-data som lagras i *lib/contacts.json* hanteras av *lib/contactRepository.js*.</span><span class="sxs-lookup"><span data-stu-id="099d1-127">This code uses hello JSON data stored in *lib/contacts.json* served by *lib/contactRepository.js*.</span></span> <span data-ttu-id="099d1-128">hello nya *contacts.js* koden returnerar alla kontakter i hello databasen som en JSON-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="099d1-128">hello new *contacts.js* code returns all contacts in hello repository as a JSON payload.</span></span> 

4. <span data-ttu-id="099d1-129">Ersätt hello koden i hello **handlers/contacts/{id}.js** fil med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="099d1-129">Replace hello code in hello **handlers/contacts/{id}.js** file with hello following code:</span></span>

    ```javascript
    'use strict';

    var repository = require('../../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.get(req.params['id']));
        }    
    };
    ```

    <span data-ttu-id="099d1-130">Den här koden kan du använda en sökväg variabeln tooreturn endast hello kontakt med angivet ID.</span><span class="sxs-lookup"><span data-stu-id="099d1-130">This code lets you use a path variable tooreturn only hello contact with a given ID.</span></span>

5. <span data-ttu-id="099d1-131">Ersätt hello koden i **server.js** med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="099d1-131">Replace hello code in **server.js** with hello following code:</span></span>

    ```javascript
    'use strict';

    var port = process.env.PORT || 8000; 

    var http = require('http');
    var express = require('express');
    var bodyParser = require('body-parser');
    var swaggerize = require('swaggerize-express');
    var swaggerUi = require('swaggerize-ui'); 
    var path = require('path');
    var fs = require("fs");
    
    fs.existsSync = fs.existsSync || require('path').existsSync;

    var app = express();

    var server = http.createServer(app);

    app.use(bodyParser.json());

    app.use(swaggerize({
        api: path.resolve('./config/swagger.json'),
        handlers: path.resolve('./handlers'),
        docspath: '/swagger' 
    }));

    // change four
    app.use('/docs', swaggerUi({
        docs: '/swagger'  
    }));

    server.listen(port, function () { 
    });
    ```   

    <span data-ttu-id="099d1-132">Den här koden gör vissa små ändringar toolet den fungerar med Azure App Service och visar en interaktiv webbgränssnittet för ditt API.</span><span class="sxs-lookup"><span data-stu-id="099d1-132">This code makes some small changes toolet it work with Azure App Service and exposes an interactive web interface for your API.</span></span>

### <a name="test-hello-api-locally"></a><span data-ttu-id="099d1-133">Testa hello API lokalt</span><span class="sxs-lookup"><span data-stu-id="099d1-133">Test hello API locally</span></span>

1. <span data-ttu-id="099d1-134">Starta hello Node.js-app</span><span class="sxs-lookup"><span data-stu-id="099d1-134">Start up hello Node.js app</span></span>
    ```bash
    npm start
    ```
    
2. <span data-ttu-id="099d1-135">Bläddra toohttp://localhost:8000 / kontaktar tooview hello JSON för hello hela kontaktlista.</span><span class="sxs-lookup"><span data-stu-id="099d1-135">Browse toohttp://localhost:8000/contacts tooview hello JSON for hello entire contact list.</span></span>
   
   ```json
    {
        "id": 1,
        "name": "Barney Poland",
        "email": "barney@contoso.com"
    },
    {
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    },
    {
        "id": 3,
        "name": "Lora Riggs",
        "email": "lora@contoso.com"
    }
   ```

3. <span data-ttu-id="099d1-136">Bläddra toohttp://localhost:8000/kontakter/2 tooview hello kontakt med en `id` två.</span><span class="sxs-lookup"><span data-stu-id="099d1-136">Browse toohttp://localhost:8000/contacts/2 tooview hello contact with an `id` of two.</span></span>
   
    ```json
    { 
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    }
    ```

4. <span data-ttu-id="099d1-137">Testa hello API: et med hello Swagger webbgränssnitt på http://localhost: 8000/docs.</span><span class="sxs-lookup"><span data-stu-id="099d1-137">Test hello API using hello Swagger web interface at http://localhost:8000/docs.</span></span>
   
    ![Swagger-webbgränssnitt](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <span data-ttu-id="099d1-139"><a id="createapiapp"></a> Skapa en API-app</span><span class="sxs-lookup"><span data-stu-id="099d1-139"><a id="createapiapp"></a> Create an API App</span></span>

<span data-ttu-id="099d1-140">I det här avsnittet använder du hello Azure CLI 2.0 toocreate hello resurser toohost hello API på Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="099d1-140">In this section, you use hello Azure CLI 2.0 toocreate hello resources toohost hello API on Azure App Service.</span></span> 

1.  <span data-ttu-id="099d1-141">Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="099d1-141">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

    ```azurecli-interactive
    az login
    ```

2. <span data-ttu-id="099d1-142">Om du har flera Azure-prenumerationer kan önskade ändra hello standard prenumeration toohello en.</span><span class="sxs-lookup"><span data-stu-id="099d1-142">If you have multiple Azure subscriptions, change hello default subscription toohello desired one.</span></span>

    ````azurecli-interactive
    az account set --subscription <name or id>
    ````

3. [!INCLUDE [Create resource group](../../includes/app-service-api-create-resource-group.md)] 

4. [!INCLUDE [Create app service plan](../../includes/app-service-api-create-app-service-plan.md)]

5. [!INCLUDE [Create API app](../../includes/app-service-api-create-api-app.md)] 


## <a name="deploy-hello-api-with-git"></a><span data-ttu-id="099d1-143">Distribuera hello-API med Git</span><span class="sxs-lookup"><span data-stu-id="099d1-143">Deploy hello API with Git</span></span>

<span data-ttu-id="099d1-144">Distribuera din kod toohello API-app genom att skicka incheckningar från din lokala Git-lagringsplatsen tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="099d1-144">Deploy your code toohello API app by pushing commits from your local Git repository tooAzure App Service.</span></span>

1. [!INCLUDE [Configure your deployment credentials](../../includes/configure-deployment-user-no-h.md)] 

2. <span data-ttu-id="099d1-145">Initiera en ny lagringsplatsen i hello *kontaktlista* directory.</span><span class="sxs-lookup"><span data-stu-id="099d1-145">Initialize a new repo in hello *ContactList* directory.</span></span> 

    ```bash
    git init .
    ```

3. <span data-ttu-id="099d1-146">Exkludera hello *node_modules* directory som skapats av npm tidigare i kursen hello från Git.</span><span class="sxs-lookup"><span data-stu-id="099d1-146">Exclude hello *node_modules* directory created by npm in an earlier step in hello tutorial from Git.</span></span> <span data-ttu-id="099d1-147">Skapa en ny `.gitignore` i hello katalogen och Lägg till hello efter texten på en ny rad var som helst i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="099d1-147">Create a new `.gitignore` file in hello current directory and add hello following text on a new line anywhere in hello file.</span></span>

    ```
    node_modules/
    ```
    <span data-ttu-id="099d1-148">Bekräfta hello `node_modules` mappen ignoreras med `git status`.</span><span class="sxs-lookup"><span data-stu-id="099d1-148">Confirm hello `node_modules` folder is being ignored with  `git status`.</span></span>

4. <span data-ttu-id="099d1-149">Bekräfta hello ändringar toohello lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="099d1-149">Commit hello changes toohello repo.</span></span>
    ```bash
    git add .
    git commit -m "initial version"
    ```

5. [!INCLUDE [Push tooAzure](../../includes/app-service-api-git-push-to-azure.md)]  
 
## <a name="test-hello-api--in-azure"></a><span data-ttu-id="099d1-150">Testa hello API i Azure</span><span class="sxs-lookup"><span data-stu-id="099d1-150">Test hello API  in Azure</span></span>

1. <span data-ttu-id="099d1-151">Öppna en webbläsare toohttp://app_name.azurewebsites.net/contacts.</span><span class="sxs-lookup"><span data-stu-id="099d1-151">Open a browser toohttp://app_name.azurewebsites.net/contacts.</span></span> <span data-ttu-id="099d1-152">Du kan se hello samma JSON returneras som när du har gjort hello begäran lokalt tidigare i kursen hello.</span><span class="sxs-lookup"><span data-stu-id="099d1-152">You see hello same JSON returned as when you made hello request locally earlier in hello tutorial.</span></span>

   ```json
   {
       "id": 1,
       "name": "Barney Poland",
       "email": "barney@contoso.com"
   },
   {
       "id": 2,
       "name": "Lacy Barrera",
       "email": "lacy@contoso.com"
   },
   {
       "id": 3,
       "name": "Lora Riggs",
       "email": "lora@contoso.com"
   }
   ```

2. <span data-ttu-id="099d1-153">I en webbläsare går du toohello `http://app_name.azurewebsites.net/docs` endpoint tootry ut hello Swagger-användargränssnitt som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="099d1-153">In a browser, go toohello `http://app_name.azurewebsites.net/docs` endpoint tootry out hello Swagger UI running on Azure.</span></span>

    ![Swagger Ii](media/app-service-api-nodejs-api-app/swagger-azure-ui.png)

    <span data-ttu-id="099d1-155">Du kan nu distribuera uppdateringar toohello exempel API tooAzure genom att trycka på incheckningar toohello Azure Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="099d1-155">You can now deploy updates toohello sample API tooAzure simply by pushing commits toohello Azure Git repository.</span></span>

## <a name="clean-up"></a><span data-ttu-id="099d1-156">Rensa</span><span class="sxs-lookup"><span data-stu-id="099d1-156">Clean up</span></span>

<span data-ttu-id="099d1-157">tooclean hello resurser som skapats i denna Snabbstart kör hello följande Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="099d1-157">tooclean up hello resources created in this quickstart, run hello following Azure CLI command:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-step"></a><span data-ttu-id="099d1-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="099d1-158">Next step</span></span> 
> [!div class="nextstepaction"]
> [<span data-ttu-id="099d1-159">Använd API-appar från JavaScript-klienter med CORS</span><span class="sxs-lookup"><span data-stu-id="099d1-159">Consume API apps from JavaScript clients with CORS</span></span>](app-service-api-cors-consume-javascript.md)

