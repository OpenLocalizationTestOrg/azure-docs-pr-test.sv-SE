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
# <a name="build-a-nodejs-restful-api-and-deploy-it-tooan-api-app-in-azure"></a>Skapa en Node.js RESTful-API och distribuera den tooan API-app i Azure
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Den här snabbstarten visar hur toocreate REST-API med Node.js [Express](http://expressjs.com/)med hjälp av en [Swagger](http://swagger.io/) definition och distribuera den som en [API-app](app-service-api-apps-why-best-platform.md) på Azure. Du kan skapa hello appen med hjälp av kommandoradsverktyg, konfigurera resurser med hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), och distribuera hello app med Git.  När du är klar har du ett fungerande exempel-REST-API som körs på Azure.

## <a name="prerequisites"></a>Krav

* [Git](https://git-scm.com/)
* [Node.js och NPM](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="prepare-your-environment"></a>Förbered din miljö

1. Kör hello efter kommandot tooclone hello exempel tooyour lokala datorn i ett terminalfönster.

    ```bash
    git clone https://github.com/Azure-Samples/app-service-api-node-contact-list
    ```

2. Ändra toohello katalog som innehåller hello exempelkod.

    ```bash
    cd app-service-api-node-contact-list
    ```

3. Installera [Swaggerize](https://www.npmjs.com/package/swaggerize-express) på din lokala dator. Swaggerize är ett verktyg som genererar Node.js-kod för REST-API från en Swagger-definition.

    ```bash
    npm install -g yo
    npm install -g generator-swaggerize
    ```

## <a name="generate-nodejs-code"></a>Generera Node.js-kod 

Det här avsnittet av kursen hello modeller ett arbetsflöde för API-utveckling där du först skapar Swagger-metadata och använder den tooscaffold (Autogenerera) serverkod för hello API. 

Ändra katalogen toohello *starta* mapp, kör `yo swaggerize`. Swaggerize skapar en Node.js-projekt för din API från hello Swagger-definition i *api.json*.

```bash
cd start
yo swaggerize --apiPath api.json --framework express
```

Använd *ContactList* när du uppmanas ange projektnamn.
   
   ```bash
   Swaggerize Generator
   Tell us a bit about your application
   ? What would you like toocall this project: ContactList
   ? Your name: Francis Totten
   ? Your github user name: fabfrank
   ? Your email: frank@fabrikam.net
   ```
   
## <a name="customize-hello-project-code"></a>Anpassa hello Projektkod

1. Kopiera hello *lib* till hello mappen *kontaktlista* mapp som skapats av `yo swaggerize`, ändra katalogen till *kontaktlista*.

    ```bash
    cp -r lib/ ContactList/
    cd ContactList
    ```

2. Installera hello `jsonpath` och `swaggerize-ui` NPM-modulerna. 

    ```bash
    npm install --save jsonpath swaggerize-ui
    ```

3. Ersätt hello koden i hello *handlers/contacts.js* med hello följande kod: 
    ```javascript
    'use strict';

    var repository = require('../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.all())
        }
    };
    ```
    Den här koden använder hello JSON-data som lagras i *lib/contacts.json* hanteras av *lib/contactRepository.js*. hello nya *contacts.js* koden returnerar alla kontakter i hello databasen som en JSON-nyttolast. 

4. Ersätt hello koden i hello **handlers/contacts/{id}.js** fil med hello följande kod:

    ```javascript
    'use strict';

    var repository = require('../../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.get(req.params['id']));
        }    
    };
    ```

    Den här koden kan du använda en sökväg variabeln tooreturn endast hello kontakt med angivet ID.

5. Ersätt hello koden i **server.js** med hello följande kod:

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

    Den här koden gör vissa små ändringar toolet den fungerar med Azure App Service och visar en interaktiv webbgränssnittet för ditt API.

### <a name="test-hello-api-locally"></a>Testa hello API lokalt

1. Starta hello Node.js-app
    ```bash
    npm start
    ```
    
2. Bläddra toohttp://localhost:8000 / kontaktar tooview hello JSON för hello hela kontaktlista.
   
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

3. Bläddra toohttp://localhost:8000/kontakter/2 tooview hello kontakt med en `id` två.
   
    ```json
    { 
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    }
    ```

4. Testa hello API: et med hello Swagger webbgränssnitt på http://localhost: 8000/docs.
   
    ![Swagger-webbgränssnitt](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a> Skapa en API-app

I det här avsnittet använder du hello Azure CLI 2.0 toocreate hello resurser toohost hello API på Azure App Service. 

1.  Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar.

    ```azurecli-interactive
    az login
    ```

2. Om du har flera Azure-prenumerationer kan önskade ändra hello standard prenumeration toohello en.

    ````azurecli-interactive
    az account set --subscription <name or id>
    ````

3. [!INCLUDE [Create resource group](../../includes/app-service-api-create-resource-group.md)] 

4. [!INCLUDE [Create app service plan](../../includes/app-service-api-create-app-service-plan.md)]

5. [!INCLUDE [Create API app](../../includes/app-service-api-create-api-app.md)] 


## <a name="deploy-hello-api-with-git"></a>Distribuera hello-API med Git

Distribuera din kod toohello API-app genom att skicka incheckningar från din lokala Git-lagringsplatsen tooAzure Apptjänst.

1. [!INCLUDE [Configure your deployment credentials](../../includes/configure-deployment-user-no-h.md)] 

2. Initiera en ny lagringsplatsen i hello *kontaktlista* directory. 

    ```bash
    git init .
    ```

3. Exkludera hello *node_modules* directory som skapats av npm tidigare i kursen hello från Git. Skapa en ny `.gitignore` i hello katalogen och Lägg till hello efter texten på en ny rad var som helst i hello-filen.

    ```
    node_modules/
    ```
    Bekräfta hello `node_modules` mappen ignoreras med `git status`.

4. Bekräfta hello ändringar toohello lagringsplatsen.
    ```bash
    git add .
    git commit -m "initial version"
    ```

5. [!INCLUDE [Push tooAzure](../../includes/app-service-api-git-push-to-azure.md)]  
 
## <a name="test-hello-api--in-azure"></a>Testa hello API i Azure

1. Öppna en webbläsare toohttp://app_name.azurewebsites.net/contacts. Du kan se hello samma JSON returneras som när du har gjort hello begäran lokalt tidigare i kursen hello.

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

2. I en webbläsare går du toohello `http://app_name.azurewebsites.net/docs` endpoint tootry ut hello Swagger-användargränssnitt som körs på Azure.

    ![Swagger Ii](media/app-service-api-nodejs-api-app/swagger-azure-ui.png)

    Du kan nu distribuera uppdateringar toohello exempel API tooAzure genom att trycka på incheckningar toohello Azure Git-lagringsplatsen.

## <a name="clean-up"></a>Rensa

tooclean hello resurser som skapats i denna Snabbstart kör hello följande Azure CLI-kommando:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-step"></a>Nästa steg 
> [!div class="nextstepaction"]
> [Använd API-appar från JavaScript-klienter med CORS](app-service-api-cors-consume-javascript.md)

