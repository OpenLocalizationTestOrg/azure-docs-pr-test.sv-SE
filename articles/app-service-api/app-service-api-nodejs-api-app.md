---
title: "Node.js API-app i Azure Apptjänst | Microsoft Docs"
description: "Lär dig hur du skapar en Node.js RESTful-API och distribuera det till en API-app i Azure Apptjänst."
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
ms.openlocfilehash: 806585edd43b9d2d678bfa41523e4d9d40af8cba
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a><span data-ttu-id="7c770-103">Skapa en Node.js RESTful-API och distribuera den till en API-app i Azure</span><span class="sxs-lookup"><span data-stu-id="7c770-103">Build a Node.js RESTful API and deploy it to an API app in Azure</span></span>
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

<span data-ttu-id="7c770-104">I den här snabbstarten visas hur du skapar ett [Express](http://expressjs.com/)-ramverk med REST API för Node.js från en [Swagger](http://swagger.io/)-definition och distribuerar det som en [API-app](app-service-api-apps-why-best-platform.md) på Azure.</span><span class="sxs-lookup"><span data-stu-id="7c770-104">This quickstart shows how to create a REST API, written with Node.js [Express](http://expressjs.com/), using a [Swagger](http://swagger.io/) definition and deploying it as an [API app](app-service-api-apps-why-best-platform.md) on Azure.</span></span> <span data-ttu-id="7c770-105">Du skapar appen med hjälp av kommandoradsverktyg, konfigurerar resurser med [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) och distribuerar appen med Git.</span><span class="sxs-lookup"><span data-stu-id="7c770-105">You create the app using command-line tools, configure resources with the [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and deploy the app using Git.</span></span>  <span data-ttu-id="7c770-106">När du är klar har du ett fungerande exempel-REST-API som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="7c770-106">When you've finished, you have a working sample REST API running on Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c770-107">Krav</span><span class="sxs-lookup"><span data-stu-id="7c770-107">Prerequisites</span></span>

* [<span data-ttu-id="7c770-108">Git</span><span class="sxs-lookup"><span data-stu-id="7c770-108">Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="7c770-109">Node.js och NPM</span><span class="sxs-lookup"><span data-stu-id="7c770-109">Node.js and NPM</span></span>](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="7c770-110">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7c770-110">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="7c770-111">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="7c770-111">Run `az --version` to find the version.</span></span> <span data-ttu-id="7c770-112">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7c770-112">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prepare-your-environment"></a><span data-ttu-id="7c770-113">Förbered din miljö</span><span class="sxs-lookup"><span data-stu-id="7c770-113">Prepare your environment</span></span>

1. <span data-ttu-id="7c770-114">Kör följande kommando i ett terminalfönster för att klona exemplet till den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="7c770-114">In a terminal window, run the following command to clone the sample to your local machine.</span></span>

    ```bash
    git clone https://github.com/Azure-Samples/app-service-api-node-contact-list
    ```

2. <span data-ttu-id="7c770-115">Ändra till den katalog som innehåller exempelkoden.</span><span class="sxs-lookup"><span data-stu-id="7c770-115">Change to the directory that contains the sample code.</span></span>

    ```bash
    cd app-service-api-node-contact-list
    ```

3. <span data-ttu-id="7c770-116">Installera [Swaggerize](https://www.npmjs.com/package/swaggerize-express) på din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="7c770-116">Install [Swaggerize](https://www.npmjs.com/package/swaggerize-express) on your local machine.</span></span> <span data-ttu-id="7c770-117">Swaggerize är ett verktyg som genererar Node.js-kod för REST-API från en Swagger-definition.</span><span class="sxs-lookup"><span data-stu-id="7c770-117">Swaggerize is a tool that generates Node.js code for your REST API from a Swagger definition.</span></span>

    ```bash
    npm install -g yo
    npm install -g generator-swaggerize
    ```

## <a name="generate-nodejs-code"></a><span data-ttu-id="7c770-118">Generera Node.js-kod</span><span class="sxs-lookup"><span data-stu-id="7c770-118">Generate Node.js code</span></span> 

<span data-ttu-id="7c770-119">Det här avsnittet av kursen visar ett arbetsflöde för API-utveckling där du först skapar Swagger-metadata och använder dem för att autogenerera en serverkod för API:et.</span><span class="sxs-lookup"><span data-stu-id="7c770-119">This section of the tutorial models an API development workflow in which you create Swagger metadata first and use that to scaffold (auto-generate) server code for the API.</span></span> 

<span data-ttu-id="7c770-120">Gå till katalogen för mappen *Start* och kör `yo swaggerize`.</span><span class="sxs-lookup"><span data-stu-id="7c770-120">Change directory to the *start* folder, then run `yo swaggerize`.</span></span> <span data-ttu-id="7c770-121">Swaggerize skapar ett Node.js-projekt för ditt API från Swagger-definitionen i *api.json*.</span><span class="sxs-lookup"><span data-stu-id="7c770-121">Swaggerize creates a Node.js project for your API from the Swagger definition in *api.json*.</span></span>

```bash
cd start
yo swaggerize --apiPath api.json --framework express
```

<span data-ttu-id="7c770-122">Använd *ContactList* när du uppmanas ange projektnamn.</span><span class="sxs-lookup"><span data-stu-id="7c770-122">When Swaggerize asks for a project name, use *ContactList*.</span></span>
   
   ```bash
   Swaggerize Generator
   Tell us a bit about your application
   ? What would you like to call this project: ContactList
   ? Your name: Francis Totten
   ? Your github user name: fabfrank
   ? Your email: frank@fabrikam.net
   ```
   
## <a name="customize-the-project-code"></a><span data-ttu-id="7c770-123">Anpassa projektkoden</span><span class="sxs-lookup"><span data-stu-id="7c770-123">Customize the project code</span></span>

1. <span data-ttu-id="7c770-124">Kopiera mappen *lib* till mappen *Kontaktlista* som skapats av `yo swaggerize` och ändra sedan katalogen till *Kontaktlista*.</span><span class="sxs-lookup"><span data-stu-id="7c770-124">Copy the *lib* folder into the *ContactList* folder created by `yo swaggerize`, then change directory into *ContactList*.</span></span>

    ```bash
    cp -r lib/ ContactList/
    cd ContactList
    ```

2. <span data-ttu-id="7c770-125">Installera NPM-modulerna `jsonpath` och `swaggerize-ui`.</span><span class="sxs-lookup"><span data-stu-id="7c770-125">Install the `jsonpath` and `swaggerize-ui` NPM modules.</span></span> 

    ```bash
    npm install --save jsonpath swaggerize-ui
    ```

3. <span data-ttu-id="7c770-126">Ersätt koden i *handlers/contacts.js* med följande kod:</span><span class="sxs-lookup"><span data-stu-id="7c770-126">Replace the code in the *handlers/contacts.js* with the following code:</span></span> 
    ```javascript
    'use strict';

    var repository = require('../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.all())
        }
    };
    ```
    <span data-ttu-id="7c770-127">Den här koden använder JSON-data som lagras i *lib/contacts.json* som hanteras av *lib/contactRepository.js*.</span><span class="sxs-lookup"><span data-stu-id="7c770-127">This code uses the JSON data stored in *lib/contacts.json* served by *lib/contactRepository.js*.</span></span> <span data-ttu-id="7c770-128">Den nya *contacts.js*-koden returnerar alla kontakter i lagringsplatsen som en JSON-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="7c770-128">The new *contacts.js* code returns all contacts in the repository as a JSON payload.</span></span> 

4. <span data-ttu-id="7c770-129">Ersätt koden i filen **handlers/contacts/{id}.js** med följande kod:</span><span class="sxs-lookup"><span data-stu-id="7c770-129">Replace the code in the **handlers/contacts/{id}.js** file with the following code:</span></span>

    ```javascript
    'use strict';

    var repository = require('../../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.get(req.params['id']));
        }    
    };
    ```

    <span data-ttu-id="7c770-130">Med den här koden kan du använda en sökvägsvariabel för att endast returnera kontakten med det angivna ID:t.</span><span class="sxs-lookup"><span data-stu-id="7c770-130">This code lets you use a path variable to return only the contact with a given ID.</span></span>

5. <span data-ttu-id="7c770-131">Ersätt koden i **server.js** med följande kod:</span><span class="sxs-lookup"><span data-stu-id="7c770-131">Replace the code in **server.js** with the following code:</span></span>

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

    <span data-ttu-id="7c770-132">Den här koden gör några små ändringar så att den fungerar med Azure App Service och gör ett interaktivt webbgränssnitt tillgängligt för ditt API.</span><span class="sxs-lookup"><span data-stu-id="7c770-132">This code makes some small changes to let it work with Azure App Service and exposes an interactive web interface for your API.</span></span>

### <a name="test-the-api-locally"></a><span data-ttu-id="7c770-133">Testa API lokalt</span><span class="sxs-lookup"><span data-stu-id="7c770-133">Test the API locally</span></span>

1. <span data-ttu-id="7c770-134">Starta Node.js-appen</span><span class="sxs-lookup"><span data-stu-id="7c770-134">Start up the Node.js app</span></span>
    ```bash
    npm start
    ```
    
2. <span data-ttu-id="7c770-135">Bläddra till http://localhost:8000/contacts för att se JSON för hela kontaktlistan.</span><span class="sxs-lookup"><span data-stu-id="7c770-135">Browse to http://localhost:8000/contacts to view the JSON for the entire contact list.</span></span>
   
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

3. <span data-ttu-id="7c770-136">Bläddra till http://localhost:8000/contacts/2 för att se kontakten med en `id` för två.</span><span class="sxs-lookup"><span data-stu-id="7c770-136">Browse to http://localhost:8000/contacts/2 to view the contact with an `id` of two.</span></span>
   
    ```json
    { 
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    }
    ```

4. <span data-ttu-id="7c770-137">Testa API:et med Swagger-webbgränssnittet på http://localhost:8000/docs.</span><span class="sxs-lookup"><span data-stu-id="7c770-137">Test the API using the Swagger web interface at http://localhost:8000/docs.</span></span>
   
    ![Swagger-webbgränssnitt](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <span data-ttu-id="7c770-139"><a id="createapiapp"></a> Skapa en API-app</span><span class="sxs-lookup"><span data-stu-id="7c770-139"><a id="createapiapp"></a> Create an API App</span></span>

<span data-ttu-id="7c770-140">I det här avsnittet använder du Azure CLI 2.0 för att skapa de resurser som behövs när Azure App Service ska vara värd för ditt API.</span><span class="sxs-lookup"><span data-stu-id="7c770-140">In this section, you use the Azure CLI 2.0 to create the resources to host the API on Azure App Service.</span></span> 

1.  <span data-ttu-id="7c770-141">Logga in på Azure-prenumerationen med kommandot [az login](/cli/azure/#login) och följ anvisningarna på skärmen.</span><span class="sxs-lookup"><span data-stu-id="7c770-141">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span>

    ```azurecli-interactive
    az login
    ```

2. <span data-ttu-id="7c770-142">Om du har flera Azure-prenumerationer kan du ändra standardprenumerationen till den du vill använda.</span><span class="sxs-lookup"><span data-stu-id="7c770-142">If you have multiple Azure subscriptions, change the default subscription to the desired one.</span></span>

    ````azurecli-interactive
    az account set --subscription <name or id>
    ````

3. [!INCLUDE [Create resource group](../../includes/app-service-api-create-resource-group.md)] 

4. [!INCLUDE [Create app service plan](../../includes/app-service-api-create-app-service-plan.md)]

5. [!INCLUDE [Create API app](../../includes/app-service-api-create-api-app.md)] 


## <a name="deploy-the-api-with-git"></a><span data-ttu-id="7c770-143">Distribuera API med Git</span><span class="sxs-lookup"><span data-stu-id="7c770-143">Deploy the API with Git</span></span>

<span data-ttu-id="7c770-144">Distribuera din kod till API-appen genom att skicka incheckningar från din lokala Git-lagringsplats till Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="7c770-144">Deploy your code to the API app by pushing commits from your local Git repository to Azure App Service.</span></span>

1. [!INCLUDE [Configure your deployment credentials](../../includes/configure-deployment-user-no-h.md)] 

2. <span data-ttu-id="7c770-145">Initiera en ny lagringsplats i katalogen *Kontaktlista*.</span><span class="sxs-lookup"><span data-stu-id="7c770-145">Initialize a new repo in the *ContactList* directory.</span></span> 

    ```bash
    git init .
    ```

3. <span data-ttu-id="7c770-146">Exkludera katalogen *node_modules* som skapats av npm tidigare i självstudien från Git.</span><span class="sxs-lookup"><span data-stu-id="7c770-146">Exclude the *node_modules* directory created by npm in an earlier step in the tutorial from Git.</span></span> <span data-ttu-id="7c770-147">Skapa en ny `.gitignore`-fil i den aktuella katalogen och lägg till följande text på en ny rad var som helst i filen.</span><span class="sxs-lookup"><span data-stu-id="7c770-147">Create a new `.gitignore` file in the current directory and add the following text on a new line anywhere in the file.</span></span>

    ```
    node_modules/
    ```
    <span data-ttu-id="7c770-148">Bekräfta att mappen `node_modules` ignoreras med `git status`.</span><span class="sxs-lookup"><span data-stu-id="7c770-148">Confirm the `node_modules` folder is being ignored with  `git status`.</span></span>

4. <span data-ttu-id="7c770-149">Checka in ändringarna till lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="7c770-149">Commit the changes to the repo.</span></span>
    ```bash
    git add .
    git commit -m "initial version"
    ```

5. [!INCLUDE [Push to Azure](../../includes/app-service-api-git-push-to-azure.md)]  
 
## <a name="test-the-api--in-azure"></a><span data-ttu-id="7c770-150">Testa API:et i Azure</span><span class="sxs-lookup"><span data-stu-id="7c770-150">Test the API  in Azure</span></span>

1. <span data-ttu-id="7c770-151">Öppna en webbläsare och gå till http://app_name.azurewebsites.net/contacts.</span><span class="sxs-lookup"><span data-stu-id="7c770-151">Open a browser to http://app_name.azurewebsites.net/contacts.</span></span> <span data-ttu-id="7c770-152">Samma JSON returneras som när du gjorde begäran lokalt tidigare i självstudien.</span><span class="sxs-lookup"><span data-stu-id="7c770-152">You see the same JSON returned as when you made the request locally earlier in the tutorial.</span></span>

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

2. <span data-ttu-id="7c770-153">Gå till `http://app_name.azurewebsites.net/docs`-slutpunkten i en webbläsare för att testa Swagger-användargränssnittet som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="7c770-153">In a browser, go to the `http://app_name.azurewebsites.net/docs` endpoint to try out the Swagger UI running on Azure.</span></span>

    ![Swagger Ii](media/app-service-api-nodejs-api-app/swagger-azure-ui.png)

    <span data-ttu-id="7c770-155">Du kan nu distribuera uppdateringar till exempel-API:et till Azure genom att helt enkelt push-överföra incheckningar till Azure Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="7c770-155">You can now deploy updates to the sample API to Azure simply by pushing commits to the Azure Git repository.</span></span>

## <a name="clean-up"></a><span data-ttu-id="7c770-156">Rensa</span><span class="sxs-lookup"><span data-stu-id="7c770-156">Clean up</span></span>

<span data-ttu-id="7c770-157">Kör följande Azure CLI-kommando för att rensa resurserna som skapades av den här snabbstarten:</span><span class="sxs-lookup"><span data-stu-id="7c770-157">To clean up the resources created in this quickstart, run the following Azure CLI command:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-step"></a><span data-ttu-id="7c770-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c770-158">Next step</span></span> 
> [!div class="nextstepaction"]
> [<span data-ttu-id="7c770-159">Använd API-appar från JavaScript-klienter med CORS</span><span class="sxs-lookup"><span data-stu-id="7c770-159">Consume API apps from JavaScript clients with CORS</span></span>](app-service-api-cors-consume-javascript.md)

