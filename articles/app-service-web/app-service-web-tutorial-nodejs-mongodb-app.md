---
title: aaaBuild en Node.js och MongoDB-webbapp i Azure | Microsoft Docs
description: "Lär dig hur tooget en Node.js-app som arbetar i Azure med anslutning tooa Cosmos DB-databas med en anslutningssträng för MongoDB."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 532251c51ed6f8513e6e366393e889b67a85e5b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-and-mongodb-web-app-in-azure"></a><span data-ttu-id="02832-103">Skapa en Node.js och MongoDB-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="02832-103">Build a Node.js and MongoDB web app in Azure</span></span>

<span data-ttu-id="02832-104">Azure Web Apps ger en mycket skalbar, automatisk uppdatering värdtjänst.</span><span class="sxs-lookup"><span data-stu-id="02832-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="02832-105">Den här kursen visar hur toocreate en Node.js webbapp i Azure och koppla den tooa MongoDB-databas.</span><span class="sxs-lookup"><span data-stu-id="02832-105">This tutorial shows how toocreate a Node.js web app in Azure and connect it tooa MongoDB database.</span></span> <span data-ttu-id="02832-106">När du är klar har du en medelvärde program (MongoDB, snabb, AngularJS och Node.js) som körs i [Azure App Service](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="02832-106">When you're done, you'll have a MEAN application (MongoDB, Express, AngularJS, and Node.js) running in [Azure App Service](app-service-web-overview.md).</span></span> <span data-ttu-id="02832-107">För enkelhetens skull använder hello exempelprogrammet hello [MEAN.js webbramverk](http://meanjs.org/).</span><span class="sxs-lookup"><span data-stu-id="02832-107">For simplicity, hello sample application uses hello [MEAN.js web framework](http://meanjs.org/).</span></span>

![MEAN.js-app som körs i Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

<span data-ttu-id="02832-109">Vad du lära dig:</span><span class="sxs-lookup"><span data-stu-id="02832-109">What you'll learn:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="02832-110">Skapa en MongoDB-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="02832-110">Create a MongoDB database in Azure</span></span>
> * <span data-ttu-id="02832-111">Ansluta tooMongoDB en Node.js-app</span><span class="sxs-lookup"><span data-stu-id="02832-111">Connect a Node.js app tooMongoDB</span></span>
> * <span data-ttu-id="02832-112">Distribuera hello app tooAzure</span><span class="sxs-lookup"><span data-stu-id="02832-112">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="02832-113">Uppdatera hello-datamodell och omdistribuera hello app</span><span class="sxs-lookup"><span data-stu-id="02832-113">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="02832-114">Dataströmmen diagnostiska loggar från Azure</span><span class="sxs-lookup"><span data-stu-id="02832-114">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="02832-115">Hantera hello appen i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="02832-115">Manage hello app in hello Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="02832-116">Krav</span><span class="sxs-lookup"><span data-stu-id="02832-116">Prerequisites</span></span>

<span data-ttu-id="02832-117">toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="02832-117">toocomplete this tutorial:</span></span>

1. [<span data-ttu-id="02832-118">Installera Git</span><span class="sxs-lookup"><span data-stu-id="02832-118">Install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="02832-119">Installera Node.js och NPM</span><span class="sxs-lookup"><span data-stu-id="02832-119">Install Node.js and NPM</span></span>](https://nodejs.org/)
1. <span data-ttu-id="02832-120">[Installera Gulp.js](http://gulpjs.com/) (krävs av [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))</span><span class="sxs-lookup"><span data-stu-id="02832-120">[Install Gulp.js](http://gulpjs.com/) (required by [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))</span></span>
1. [<span data-ttu-id="02832-121">Installera och köra MongoDB Community Edition</span><span class="sxs-lookup"><span data-stu-id="02832-121">Install and run MongoDB Community Edition</span></span>](https://docs.mongodb.com/manual/administration/install-community/) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="02832-122">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="02832-122">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="02832-123">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="02832-123">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="02832-124">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="02832-124">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="test-local-mongodb"></a><span data-ttu-id="02832-125">Testa lokala MongoDB</span><span class="sxs-lookup"><span data-stu-id="02832-125">Test local MongoDB</span></span>

<span data-ttu-id="02832-126">Öppna hello terminalfönster och `cd` toohello `bin` katalogen MongoDB-installationen.</span><span class="sxs-lookup"><span data-stu-id="02832-126">Open hello terminal window and `cd` toohello `bin` directory of your MongoDB installation.</span></span> <span data-ttu-id="02832-127">Du kan använda den här terminalfönster toorun alla hello-kommandon i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="02832-127">You can use this terminal window toorun all hello commands in this tutorial.</span></span>

<span data-ttu-id="02832-128">Kör `mongo` i hello-terminal tooconnect tooyour lokala MongoDB-servern.</span><span class="sxs-lookup"><span data-stu-id="02832-128">Run `mongo` in hello terminal tooconnect tooyour local MongoDB server.</span></span>

```bash
mongo
```

<span data-ttu-id="02832-129">Om anslutningen lyckas körs redan MongoDB-databas.</span><span class="sxs-lookup"><span data-stu-id="02832-129">If your connection is successful, then your MongoDB database is already running.</span></span> <span data-ttu-id="02832-130">Om inte, kontrollera att den lokala MongoDB-databasen har startats genom att följa stegen hello på [installera MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/).</span><span class="sxs-lookup"><span data-stu-id="02832-130">If not, make sure that your local MongoDB database is started by following hello steps at [Install MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/).</span></span> <span data-ttu-id="02832-131">Ofta MongoDB installeras, men du behöver fortfarande toostart den genom att köra `mongod`.</span><span class="sxs-lookup"><span data-stu-id="02832-131">Often, MongoDB is installed, but you still need toostart it by running `mongod`.</span></span> 

<span data-ttu-id="02832-132">När du är klar testning MongoDB-databas, Skriv `Ctrl+C` i hello terminal.</span><span class="sxs-lookup"><span data-stu-id="02832-132">When you're done testing your MongoDB database, type `Ctrl+C` in hello terminal.</span></span> 

## <a name="create-local-nodejs-app"></a><span data-ttu-id="02832-133">Skapa lokal Node.js-app</span><span class="sxs-lookup"><span data-stu-id="02832-133">Create local Node.js app</span></span>

<span data-ttu-id="02832-134">I det här steget kan du ställa in hello lokalt Node.js-projekt.</span><span class="sxs-lookup"><span data-stu-id="02832-134">In this step, you set up hello local Node.js project.</span></span>

### <a name="clone-hello-sample-application"></a><span data-ttu-id="02832-135">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="02832-135">Clone hello sample application</span></span>

<span data-ttu-id="02832-136">I hello terminalfönster, `cd` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="02832-136">In hello terminal window, `cd` tooa working directory.</span></span>  

<span data-ttu-id="02832-137">Hello kör följande kommando tooclone hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="02832-137">Run hello following command tooclone hello sample repository.</span></span> 

```bash
git clone https://github.com/Azure-Samples/meanjs.git
```

<span data-ttu-id="02832-138">Det här exemplet databasen innehåller en kopia av hello [MEAN.js databasen](https://github.com/meanjs/mean).</span><span class="sxs-lookup"><span data-stu-id="02832-138">This sample repository contains a copy of hello [MEAN.js repository](https://github.com/meanjs/mean).</span></span> <span data-ttu-id="02832-139">Är det ändrade toorun på Apptjänst (Mer information finns i databasen för hello MEAN.js [filen Viktigt](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).</span><span class="sxs-lookup"><span data-stu-id="02832-139">It is modified toorun on App Service (for more information, see hello MEAN.js repository [README file](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).</span></span>

### <a name="run-hello-application"></a><span data-ttu-id="02832-140">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="02832-140">Run hello application</span></span>

<span data-ttu-id="02832-141">Kör följande kommandon tooinstall hello krävs paket hello och starta programmet hello.</span><span class="sxs-lookup"><span data-stu-id="02832-141">Run hello following commands tooinstall hello required packages and start hello application.</span></span>

```bash
cd meanjs
npm install
npm start
```

<span data-ttu-id="02832-142">När hello app har lästs in helt, se något liknande toohello följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="02832-142">When hello app is fully loaded, you see something similar toohello following message:</span></span>

```
--
MEAN.JS - Development Environment

Environment:     development
Server:          http://0.0.0.0:3000
Database:        mongodb://localhost/mean-dev
App version:     0.5.0
MEAN.JS version: 0.5.0
--
```

<span data-ttu-id="02832-143">Navigera toohttp://localhost:3000 i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="02832-143">Navigate toohttp://localhost:3000 in a browser.</span></span> <span data-ttu-id="02832-144">Klicka på **registrera dig** i hello översta menyn och skapa en testanvändare.</span><span class="sxs-lookup"><span data-stu-id="02832-144">Click **Sign Up** in hello top menu and create a test user.</span></span> 

<span data-ttu-id="02832-145">Hej MEAN.js exempelprogrammet lagrar användardata i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="02832-145">hello MEAN.js sample application stores user data in hello database.</span></span> <span data-ttu-id="02832-146">Om du lyckas skapa en användare och logga in är appen skriver data toohello lokala MongoDB-databas.</span><span class="sxs-lookup"><span data-stu-id="02832-146">If you are successful at creating a user and signing in, then your app is writing data toohello local MongoDB database.</span></span>

![MEAN.js ansluter har tooMongoDB](./media/app-service-web-tutorial-nodejs-mongodb-app/mongodb-connect-success.png)

<span data-ttu-id="02832-148">Välj **Admin > Hantera artiklar** tooadd vissa artiklar.</span><span class="sxs-lookup"><span data-stu-id="02832-148">Select **Admin > Manage Articles** tooadd some articles.</span></span>

<span data-ttu-id="02832-149">Tryck på toostop Node.js när som helst `Ctrl+C` i hello terminal.</span><span class="sxs-lookup"><span data-stu-id="02832-149">toostop Node.js at any time, press `Ctrl+C` in hello terminal.</span></span> 

## <a name="create-production-mongodb"></a><span data-ttu-id="02832-150">Skapa produktion MongoDB</span><span class="sxs-lookup"><span data-stu-id="02832-150">Create production MongoDB</span></span>

<span data-ttu-id="02832-151">I det här steget skapar du en MongoDB-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="02832-151">In this step, you create a MongoDB database in Azure.</span></span> <span data-ttu-id="02832-152">När din app är distribuerade tooAzure, använder den här databasen i molnet.</span><span class="sxs-lookup"><span data-stu-id="02832-152">When your app is deployed tooAzure, it uses this cloud database.</span></span>

<span data-ttu-id="02832-153">Den här kursen används för MongoDB, [Azure Cosmos DB](/azure/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="02832-153">For MongoDB, this tutorial uses [Azure Cosmos DB](/azure/documentdb/).</span></span> <span data-ttu-id="02832-154">Cosmos DB stöder MongoDB-klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="02832-154">Cosmos DB supports MongoDB client connections.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="02832-155">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="02832-155">Log in tooAzure</span></span>

<span data-ttu-id="02832-156">Du ska använda hello Azure CLI 2.0 toocreate hello resurser som krävs för toohost din app i Azure.</span><span class="sxs-lookup"><span data-stu-id="02832-156">You'll use hello Azure CLI 2.0 toocreate hello resources needed toohost your app in Azure.</span></span> <span data-ttu-id="02832-157">Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="02832-157">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

```azurecli-interactive
az login
```   

### <a name="create-a-resource-group"></a><span data-ttu-id="02832-158">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="02832-158">Create a resource group</span></span>

<span data-ttu-id="02832-159">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="02832-159">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

<span data-ttu-id="02832-160">hello följande exempel skapas en resursgrupp i hello Västeuropa region.</span><span class="sxs-lookup"><span data-stu-id="02832-160">hello following example creates a resource group in hello West Europe region.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

<span data-ttu-id="02832-161">Använd hello [az apptjänst lista-platser](/cli/azure/appservice#list-locations) Azure CLI kommandot toolist tillgängliga platser.</span><span class="sxs-lookup"><span data-stu-id="02832-161">Use hello [az appservice list-locations](/cli/azure/appservice#list-locations) Azure CLI command toolist available locations.</span></span> 

### <a name="create-a-cosmos-db-account"></a><span data-ttu-id="02832-162">Skapa ett Cosmos-DB-konto</span><span class="sxs-lookup"><span data-stu-id="02832-162">Create a Cosmos DB account</span></span>

<span data-ttu-id="02832-163">Skapa ett Cosmos-DB-konto med hello [az cosmosdb skapa](/cli/azure/cosmosdb#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="02832-163">Create a Cosmos DB account with hello [az cosmosdb create](/cli/azure/cosmosdb#create) command.</span></span>

<span data-ttu-id="02832-164">I hello följande kommando, ersätta ett unikt Cosmos-databasnamn för hello  *\<cosmosdb_name >* platshållare.</span><span class="sxs-lookup"><span data-stu-id="02832-164">In hello following command, substitute a unique Cosmos DB name for hello *\<cosmosdb_name>* placeholder.</span></span> <span data-ttu-id="02832-165">Det här namnet används som en del hello hello Cosmos DB slutpunkt `https://<cosmosdb_name>.documents.azure.com/`, så hello namn måste toobe unikt över alla Cosmos-DB-konton i Azure.</span><span class="sxs-lookup"><span data-stu-id="02832-165">This name is used as hello part of hello Cosmos DB endpoint, `https://<cosmosdb_name>.documents.azure.com/`, so hello name needs toobe unique across all Cosmos DB accounts in Azure.</span></span> <span data-ttu-id="02832-166">hello namn får innehålla endast små bokstäver, siffror och hello bindestreck (-), och måste vara mellan 3 och 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="02832-166">hello name must contain only lowercase letters, numbers, and hello hyphen (-) character, and must be between 3 and 50 characters long.</span></span>

```azurecli-interactive
az cosmosdb create \
    --name <cosmosdb_name> \
    --resource-group myResourceGroup \
    --kind MongoDB
```

<span data-ttu-id="02832-167">Hej *--kind MongoDB* parametern aktiverar MongoDB-klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="02832-167">hello *--kind MongoDB* parameter enables MongoDB client connections.</span></span>

<span data-ttu-id="02832-168">När hello Cosmos-DB-konto har skapats, visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="02832-168">When hello Cosmos DB account is created, hello Azure CLI shows information similar toohello following example:</span></span>

```json
{
  "consistencyPolicy":
  {
    "defaultConsistencyLevel": "Session",
    "maxIntervalInSeconds": 5,
    "maxStalenessPrefix": 100
  },
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb_name>.documents.azure.com:443/",
  "failoverPolicies": 
  ...
  < Output truncated for readability >
}
```

## <a name="connect-app-tooproduction-mongodb"></a><span data-ttu-id="02832-169">Ansluta appen tooproduction MongoDB</span><span class="sxs-lookup"><span data-stu-id="02832-169">Connect app tooproduction MongoDB</span></span>

<span data-ttu-id="02832-170">I det här steget kan ansluta du din MEAN.js programmet toohello Cosmos DB exempeldatabasen du precis skapade, med hjälp av en anslutningssträng för MongoDB.</span><span class="sxs-lookup"><span data-stu-id="02832-170">In this step, you connect your MEAN.js sample application toohello Cosmos DB database you just created, using a MongoDB connection string.</span></span> 

### <a name="retrieve-hello-database-key"></a><span data-ttu-id="02832-171">Hämta hello-databas</span><span class="sxs-lookup"><span data-stu-id="02832-171">Retrieve hello database key</span></span>

<span data-ttu-id="02832-172">tooconnect toohello Cosmos DB databasen måste hello databasens nyckel.</span><span class="sxs-lookup"><span data-stu-id="02832-172">tooconnect toohello Cosmos DB database, you need hello database key.</span></span> <span data-ttu-id="02832-173">Använd hello [az cosmosdb lista nycklar](/cli/azure/cosmosdb#list-keys) kommandot tooretrieve hello primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="02832-173">Use hello [az cosmosdb list-keys](/cli/azure/cosmosdb#list-keys) command tooretrieve hello primary key.</span></span>

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

<span data-ttu-id="02832-174">hello Azure CLI visar information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="02832-174">hello Azure CLI shows information similar toohello following example:</span></span>

```json
{
  "primaryMasterKey": "RS4CmUwzGRASJPMoc0kiEvdnKmxyRILC9BWisAYh3Hq4zBYKr0XQiSE4pqx3UchBeO4QRCzUt1i7w0rOkitoJw==",
  "primaryReadonlyMasterKey": "HvitsjIYz8TwRmIuPEUAALRwqgKOzJUjW22wPL2U8zoMVhGvregBkBk9LdMTxqBgDETSq7obbwZtdeFY7hElTg==",
  "secondaryMasterKey": "Lu9aeZTiXU4PjuuyGBbvS1N9IRG3oegIrIh95U6VOstf9bJiiIpw3IfwSUgQWSEYM3VeEyrhHJ4rn3Ci0vuFqA==",
  "secondaryReadonlyMasterKey": "LpsCicpVZqHRy7qbMgrzbRKjbYCwCKPQRl0QpgReAOxMcggTvxJFA94fTi0oQ7xtxpftTJcXkjTirQ0pT7QFrQ=="
}
```

Kopiera hello värdet för `primaryMasterKey`. <span data-ttu-id="02832-176">Du behöver den här informationen i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="02832-176">You need this information in hello next step.</span></span>

<a name="devconfig"></a>
### <a name="configure-hello-connection-string-in-your-nodejs-application"></a><span data-ttu-id="02832-177">Konfigurera hello anslutningssträngen i Node.js-programmet</span><span class="sxs-lookup"><span data-stu-id="02832-177">Configure hello connection string in your Node.js application</span></span>

<span data-ttu-id="02832-178">Öppna din databas MEAN.js _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="02832-178">In your MEAN.js repository, open _config/env/production.js_.</span></span>

<span data-ttu-id="02832-179">I hello `db` objekt, uppdatera hello värdet för `uri`:</span><span class="sxs-lookup"><span data-stu-id="02832-179">In hello `db` object, update hello value of `uri`:</span></span>

* <span data-ttu-id="02832-180">Ersätt hello två  *\<cosmosdb_name >* platshållare med databasnamnet Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="02832-180">Replace hello two *\<cosmosdb_name>* placeholders with your Cosmos DB database name.</span></span>
* <span data-ttu-id="02832-181">Ersätt hello  *\<primary_master_key >* platshållaren med hello-nyckel som du kopierade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="02832-181">Replace hello *\<primary_master_key>* placeholder with hello key you copied in hello previous step.</span></span>

<span data-ttu-id="02832-182">hello följande kod visar hello `db` objekt:</span><span class="sxs-lookup"><span data-stu-id="02832-182">hello following code shows hello `db` object:</span></span>

```javascript
db: {
  uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false',
  ...
},
```

<span data-ttu-id="02832-183">Hej `ssl=true` alternativet krävs eftersom [Cosmos DB kräver SSL](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span><span class="sxs-lookup"><span data-stu-id="02832-183">hello `ssl=true` option is required because [Cosmos DB requires SSL](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span></span> 

<span data-ttu-id="02832-184">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="02832-184">Save your changes.</span></span>

### <a name="test-hello-application-in-production-mode"></a><span data-ttu-id="02832-185">Testa programmet hello i Produktionsläge</span><span class="sxs-lookup"><span data-stu-id="02832-185">Test hello application in production mode</span></span> 

<span data-ttu-id="02832-186">Kör hello följande toominify och paket kommandoskript för hello-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="02832-186">Run hello following command toominify and bundle scripts for hello production environment.</span></span> <span data-ttu-id="02832-187">Den här processen genererar hello-filer som behövs i hello-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="02832-187">This process generates hello files needed by hello production environment.</span></span>

```bash
gulp prod
```

<span data-ttu-id="02832-188">Kör hello efter kommandot toouse hello-anslutningssträng som du konfigurerade i _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="02832-188">Run hello following command toouse hello connection string you configured in _config/env/production.js_.</span></span>

```bash
NODE_ENV=production node server.js
```

<span data-ttu-id="02832-189">`NODE_ENV=production`Anger hello miljövariabel som anger Node.js toorun i hello-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="02832-189">`NODE_ENV=production` sets hello environment variable that tells Node.js toorun in hello production environment.</span></span>  <span data-ttu-id="02832-190">`node server.js`startar hello Node.js server med `server.js` i Lagringsplatsens rot.</span><span class="sxs-lookup"><span data-stu-id="02832-190">`node server.js` starts hello Node.js server with `server.js` in your repository root.</span></span> <span data-ttu-id="02832-191">Detta är hur Node.js-programmet läses in i Azure.</span><span class="sxs-lookup"><span data-stu-id="02832-191">This is how your Node.js application is loaded in Azure.</span></span> 

<span data-ttu-id="02832-192">När hello app har lästs in, kontrollera att den körs i produktionsmiljö hello toomake:</span><span class="sxs-lookup"><span data-stu-id="02832-192">When hello app is loaded, check toomake sure that it's running in hello production environment:</span></span>

```
--
MEAN.JS

Environment:     production
Server:          http://0.0.0.0:8443
Database:        mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false
App version:     0.5.0
MEAN.JS version: 0.5.0
```

<span data-ttu-id="02832-193">Navigera toohttp://localhost:8443 i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="02832-193">Navigate toohttp://localhost:8443 in a browser.</span></span> <span data-ttu-id="02832-194">Klicka på **registrera dig** i hello översta menyn och skapa en testanvändare.</span><span class="sxs-lookup"><span data-stu-id="02832-194">Click **Sign Up** in hello top menu and create a test user.</span></span> <span data-ttu-id="02832-195">Om du lyckas skapa en användare och logga in, sedan skriver din app data toohello Cosmos-DB-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="02832-195">If you are successful creating a user and signing in, then your app is writing data toohello Cosmos DB database in Azure.</span></span> 

<span data-ttu-id="02832-196">Stoppa Node.js genom att skriva i hello terminal, `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="02832-196">In hello terminal, stop Node.js by typing `Ctrl+C`.</span></span> 

## <a name="deploy-app-tooazure"></a><span data-ttu-id="02832-197">Distribuera appen tooAzure</span><span class="sxs-lookup"><span data-stu-id="02832-197">Deploy app tooAzure</span></span>

<span data-ttu-id="02832-198">I det här steget kan distribuera du din MongoDB-anslutna Node.js-programmet tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="02832-198">In this step, you deploy your MongoDB-connected Node.js application tooAzure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="02832-199">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="02832-199">Create an App Service plan</span></span>

<span data-ttu-id="02832-200">Skapa en apptjänstplan med hello [az programtjänstplan skapa](/cli/azure/appservice/plan#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="02832-200">Create an App Service plan with hello [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="02832-201">hello följande exempel skapas en apptjänstplan med namnet _myAppServicePlan_ med hello **lediga** prisnivån:</span><span class="sxs-lookup"><span data-stu-id="02832-201">hello following example creates an App Service plan named _myAppServicePlan_ using hello **FREE** pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

<span data-ttu-id="02832-202">När hello programtjänstplanen har skapats, visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="02832-202">When hello App Service plan is created, hello Azure CLI shows information similar toohello following example:</span></span>

```json 
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
```

### <a name="create-a-web-app"></a><span data-ttu-id="02832-203">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="02832-203">Create a web app</span></span>

<span data-ttu-id="02832-204">Skapa en webbapp i hello `myAppServicePlan` App Service-plan med hello [az webapp skapa](/cli/azure/webapp#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="02832-204">Create a web app in hello `myAppServicePlan` App Service plan with hello [az webapp create](/cli/azure/webapp#create) command.</span></span> 

<span data-ttu-id="02832-205">hello web app ger du en värd utrymme toodeploy koden och ger en URL för du tooview hello distribuerat program.</span><span class="sxs-lookup"><span data-stu-id="02832-205">hello web app gives you a hosting space toodeploy your code and provides a URL for you tooview hello deployed application.</span></span> <span data-ttu-id="02832-206">Använda toocreate hello webbapp.</span><span class="sxs-lookup"><span data-stu-id="02832-206">Use  toocreate hello web app.</span></span> 

<span data-ttu-id="02832-207">Följande kommando, Ersätt hello i hello  *\<appnamn >* med ett unikt appnamn.</span><span class="sxs-lookup"><span data-stu-id="02832-207">In hello following command, replace hello *\<app_name>* placeholder with a unique app name.</span></span> <span data-ttu-id="02832-208">Det här namnet används under hello hello standard-URL för hello webbprogrammet så hello namn måste toobe unikt över alla program i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="02832-208">This name is used as hello part of hello default URL for hello web app, so hello name needs toobe unique across all apps in Azure App Service.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="02832-209">När hello webbprogrammet har skapats, visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="02832-209">When hello web app has been created, hello Azure CLI shows information similar toohello following example:</span></span> 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-an-environment-variable"></a><span data-ttu-id="02832-210">Konfigurera en miljövariabel</span><span class="sxs-lookup"><span data-stu-id="02832-210">Configure an environment variable</span></span>

<span data-ttu-id="02832-211">Tidigare i hello självstudier, du hårdkodad hello databasanslutningssträng i _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="02832-211">Earlier in hello tutorial, you hardcoded hello database connection string in _config/env/production.js_.</span></span> <span data-ttu-id="02832-212">Inom ramen för säkerhets skull bör du tookeep känsliga data utanför Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="02832-212">In keeping with security best practice, you want tookeep this sensitive data out of your Git repository.</span></span> <span data-ttu-id="02832-213">För din app som körs i Azure, ska du använda en miljövariabel i stället.</span><span class="sxs-lookup"><span data-stu-id="02832-213">For your app running in Azure, you'll use an environment variable instead.</span></span>

<span data-ttu-id="02832-214">I App Service som du anger miljövariabler som _appinställningar_ med hjälp av hello [az webapp config appsettings uppdatera](/cli/azure/webapp/config/appsettings#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="02832-214">In App Service, you set environment variables as _app settings_ by using hello [az webapp config appsettings update](/cli/azure/webapp/config/appsettings#update) command.</span></span> 

<span data-ttu-id="02832-215">hello följande exempel konfigureras en `MONGODB_URI` appinställningen i ditt Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="02832-215">hello following example configures a `MONGODB_URI` app setting in your Azure web app.</span></span> <span data-ttu-id="02832-216">Ersätt hello  *\<appnamn >*,  *\<cosmosdb_name >*, och  *\<primary_master_key >* platshållare.</span><span class="sxs-lookup"><span data-stu-id="02832-216">Replace hello *\<app_name>*, *\<cosmosdb_name>*, and *\<primary_master_key>* placeholders.</span></span>

```azurecli-interactive
az webapp config appsettings update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

<span data-ttu-id="02832-217">I Node.js-kod du åtkomst till den här appinställning med `process.env.MONGODB_URI`, precis som du skulle använda en miljövariabel.</span><span class="sxs-lookup"><span data-stu-id="02832-217">In Node.js code, you access this app setting with `process.env.MONGODB_URI`, just like you would access any environment variable.</span></span> 

<span data-ttu-id="02832-218">Nu kan ångra ändringar-too_config/env/production.js_ med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="02832-218">Now, undo your changes too_config/env/production.js_ with hello following command:</span></span>

```bash
git checkout -- .
```

<span data-ttu-id="02832-219">Öppna _config/env/production.js_ igen.</span><span class="sxs-lookup"><span data-stu-id="02832-219">Open _config/env/production.js_ again.</span></span> <span data-ttu-id="02832-220">Observera att MEAN.js standardappen hello är redan konfigurerade toouse hello `MONGODB_URI` miljövariabel som du skapade.</span><span class="sxs-lookup"><span data-stu-id="02832-220">Note that hello default MEAN.js app is already configured toouse hello `MONGODB_URI` environment variable that you created.</span></span>

```javascript
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```

### <a name="configure-local-git-deployment"></a><span data-ttu-id="02832-221">Konfigurera lokal git-distribution</span><span class="sxs-lookup"><span data-stu-id="02832-221">Configure local git deployment</span></span> 

<span data-ttu-id="02832-222">Använd hello [az webapp distributionsanvändare har angetts](/cli/azure/webapp/deployment/user#set) kommandot toocreate autentiseringsuppgifter för distribution.</span><span class="sxs-lookup"><span data-stu-id="02832-222">Use hello [az webapp deployment user set](/cli/azure/webapp/deployment/user#set) command toocreate credentials for deployment.</span></span>

<span data-ttu-id="02832-223">Du kan distribuera ditt program tooAzure Apptjänst på olika sätt, inklusive FTP, lokal Git, GitHub, Visual Studio Team Services och BitBucket.</span><span class="sxs-lookup"><span data-stu-id="02832-223">You can deploy your application tooAzure App Service in various ways including FTP, local Git, GitHub, Visual Studio Team Services, and BitBucket.</span></span> <span data-ttu-id="02832-224">FTP- och lokala Git, är det nödvändigt toohave en distribution av användare som har konfigurerats på hello server tooauthenticate din distribution.</span><span class="sxs-lookup"><span data-stu-id="02832-224">For FTP and local Git, it is necessary toohave a deployment user configured on hello server tooauthenticate your deployment.</span></span> <span data-ttu-id="02832-225">Den här distributionen användaren är kontonivå och skiljer sig från ditt konto i Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="02832-225">This deployment user is account-level and is different from your Azure subscription account.</span></span> <span data-ttu-id="02832-226">Du behöver bara tooconfigure distribution användaren en gång.</span><span class="sxs-lookup"><span data-stu-id="02832-226">You only need tooconfigure this deployment user once.</span></span>

<span data-ttu-id="02832-227">Följande kommando, Ersätt i hello  *\<användarnamn >* och  *\<lösenord >* med ett nytt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="02832-227">In hello following command, replace *\<user-name>* and *\<password>* with a new user name and password.</span></span> <span data-ttu-id="02832-228">hello användarnamnet måste vara unika.</span><span class="sxs-lookup"><span data-stu-id="02832-228">hello user name must be unique.</span></span> <span data-ttu-id="02832-229">hello lösenord måste innehålla minst åtta tecken, med två av följande tre element hello: bokstäver, siffror, symboler.</span><span class="sxs-lookup"><span data-stu-id="02832-229">hello password must be at least eight characters long, with two of hello following three elements:  letters, numbers, symbols.</span></span> <span data-ttu-id="02832-230">Om du får en ` 'Conflict'. Details: 409` fel, ändra hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="02832-230">If you get a ` 'Conflict'. Details: 409` error, change hello username.</span></span> <span data-ttu-id="02832-231">Om du ser felet ` 'Bad Request'. Details: 400` ska du använda ett starkare lösenord.</span><span class="sxs-lookup"><span data-stu-id="02832-231">If you get a ` 'Bad Request'. Details: 400` error, use a stronger password.</span></span>

```azurecli-interactive
az appservice web deployment user set --user-name <username> --password <password>
```

<span data-ttu-id="02832-232">Posten hello användarnamn och lösenord för användning i senare steg när du distribuerar hello app.</span><span class="sxs-lookup"><span data-stu-id="02832-232">Record hello user name and password for use in later steps when you deploy hello app.</span></span>

<span data-ttu-id="02832-233">Använd hello [az webapp distribution källa config-lokal-git](/cli/azure/webapp/deployment/source#config-local-git) kommandot tooconfigure lokal Git åtkomst toohello Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="02832-233">Use hello [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git) command tooconfigure local Git access toohello Azure web app.</span></span> 

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup
```

<span data-ttu-id="02832-234">När hello distribution användaren är konfigurerad, visar hello Azure CLI hello distributionens URL för din Azure-webbapp i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="02832-234">When hello deployment user is configured, hello Azure CLI shows hello deployment URL for your Azure web app in hello following format:</span></span>

```bash 
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git 
``` 

<span data-ttu-id="02832-235">Kopiera hello utdata från hello terminal som kommer att användas i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="02832-235">Copy hello output from hello terminal, as it will be used in hello next step.</span></span> 

### <a name="push-tooazure-from-git"></a><span data-ttu-id="02832-236">Push-tooAzure från Git</span><span class="sxs-lookup"><span data-stu-id="02832-236">Push tooAzure from Git</span></span>

<span data-ttu-id="02832-237">Lägg till en Azure remote tooyour lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="02832-237">Add an Azure remote tooyour local Git repository.</span></span> 

```bash
git remote add azure <paste_copied_url_here> 
```

<span data-ttu-id="02832-238">Skicka toohello Azure remote toodeploy Node.js-programmet.</span><span class="sxs-lookup"><span data-stu-id="02832-238">Push toohello Azure remote toodeploy your Node.js application.</span></span> <span data-ttu-id="02832-239">Du uppmanas att hello lösenordet du angav tidigare som en del av hello skapandet av hello distribution av användaren.</span><span class="sxs-lookup"><span data-stu-id="02832-239">You will be prompted for hello password you supplied earlier as part of hello creation of hello deployment user.</span></span> 

```bash
git push azure master
```

<span data-ttu-id="02832-240">Under distributionen kommunicerar förloppet med Git i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="02832-240">During deployment, Azure App Service communicates its progress with Git.</span></span>

```bash
Counting objects: 5, done.
Delta compression using up too4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 489 bytes | 0 bytes/s, done.
Total 5 (delta 3), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6c7c716eee'.
remote: Running custom deployment command...
remote: Running deployment command...
remote: Handling node.js deployment.
.
.
.
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
``` 

<span data-ttu-id="02832-241">Det kan hända att hello distributionsprocessen körs [Gulp](http://gulpjs.com/) när `npm install`.</span><span class="sxs-lookup"><span data-stu-id="02832-241">You may notice that hello deployment process runs [Gulp](http://gulpjs.com/) after `npm install`.</span></span> <span data-ttu-id="02832-242">App Service körs inte Gulp eller Grunt uppgifter under distributionen, så att den här lagringsplatsen exempel har två ytterligare filer i dess rot directory tooenable som:</span><span class="sxs-lookup"><span data-stu-id="02832-242">App Service does not run Gulp or Grunt tasks during deployment, so this sample repository has two additional files in its root directory tooenable it:</span></span> 

- <span data-ttu-id="02832-243">_.Deployment_ -den här filen talar om Apptjänst toorun `bash deploy.sh` som hello distribution av anpassade skript.</span><span class="sxs-lookup"><span data-stu-id="02832-243">_.deployment_ - This file tells App Service toorun `bash deploy.sh` as hello custom deployment script.</span></span>
- <span data-ttu-id="02832-244">_Deploy.SH_ -hello anpassat distributionsskriptet.</span><span class="sxs-lookup"><span data-stu-id="02832-244">_deploy.sh_ - hello custom deployment script.</span></span> <span data-ttu-id="02832-245">Om du har läst hello-fil, ser du att den körs `gulp prod` när `npm install` och `bower install`.</span><span class="sxs-lookup"><span data-stu-id="02832-245">If you review hello file, you will see that it runs `gulp prod` after `npm install` and `bower install`.</span></span> 

<span data-ttu-id="02832-246">Du kan använda den här metoden tooadd alla steg tooyour Git-baserad distribution.</span><span class="sxs-lookup"><span data-stu-id="02832-246">You can use this approach tooadd any step tooyour Git-based deployment.</span></span> <span data-ttu-id="02832-247">Om du startar om din Azure-webbapp när som helst kör inte App Service dessa automation-aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="02832-247">If you restart your Azure web app at any point, App Service doesn't rerun these automation tasks.</span></span>

### <a name="browse-toohello-azure-web-app"></a><span data-ttu-id="02832-248">Bläddra toohello Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="02832-248">Browse toohello Azure web app</span></span> 

<span data-ttu-id="02832-249">Bläddra toohello distribuera webbprogram med hjälp av webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="02832-249">Browse toohello deployed web app using your web browser.</span></span> 

```bash 
http://<app_name>.azurewebsites.net 
``` 

<span data-ttu-id="02832-250">Klicka på **registrera dig** i hello översta menyn och skapa en dummy användare.</span><span class="sxs-lookup"><span data-stu-id="02832-250">Click **Sign Up** in hello top menu and create a dummy user.</span></span> 

<span data-ttu-id="02832-251">Om du har lyckats och hello appen automatiskt loggar in har toohello skapade användaren sedan MEAN.js-app i Azure anslutningen toohello MongoDB (Cosmos-DB) databas.</span><span class="sxs-lookup"><span data-stu-id="02832-251">If you are successful and hello app automatically signs in toohello created user, then your MEAN.js app in Azure has connectivity toohello MongoDB (Cosmos DB) database.</span></span> 

![MEAN.js-app som körs i Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

<span data-ttu-id="02832-253">Välj **Admin > Hantera artiklar** tooadd vissa artiklar.</span><span class="sxs-lookup"><span data-stu-id="02832-253">Select **Admin > Manage Articles** tooadd some articles.</span></span> 

<span data-ttu-id="02832-254">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="02832-254">**Congratulations!**</span></span> <span data-ttu-id="02832-255">Du kör en datadrivna Node.js-app i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="02832-255">You're running a data-driven Node.js app in Azure App Service.</span></span>

## <a name="update-data-model-and-redeploy"></a><span data-ttu-id="02832-256">Uppdatera datamodellen och distribuera om</span><span class="sxs-lookup"><span data-stu-id="02832-256">Update data model and redeploy</span></span>

<span data-ttu-id="02832-257">I det här steget kan du ändra hello `article` data modell och publicera din ändring tooAzure.</span><span class="sxs-lookup"><span data-stu-id="02832-257">In this step, you change hello `article` data model and publish your change tooAzure.</span></span>

### <a name="update-hello-data-model"></a><span data-ttu-id="02832-258">Uppdatera hello-datamodell</span><span class="sxs-lookup"><span data-stu-id="02832-258">Update hello data model</span></span>

<span data-ttu-id="02832-259">Öppna _modules/articles/server/models/article.server.model.js_.</span><span class="sxs-lookup"><span data-stu-id="02832-259">Open _modules/articles/server/models/article.server.model.js_.</span></span>

<span data-ttu-id="02832-260">I `ArticleSchema`, lägga till en `String` typ som kallas `comment`.</span><span class="sxs-lookup"><span data-stu-id="02832-260">In `ArticleSchema`, add a `String` type called `comment`.</span></span> <span data-ttu-id="02832-261">När du är klar schemat koden se ut så här:</span><span class="sxs-lookup"><span data-stu-id="02832-261">When you're done, your schema code should look like this:</span></span>

```javascript
var ArticleSchema = new Schema({
  ...,
  user: {
    type: Schema.ObjectId,
    ref: 'User'
  },
  comment: {
    type: String,
    default: '',
    trim: true
  }
});
```

### <a name="update-hello-articles-code"></a><span data-ttu-id="02832-262">Uppdatera hello artiklar kod</span><span class="sxs-lookup"><span data-stu-id="02832-262">Update hello articles code</span></span>

<span data-ttu-id="02832-263">Uppdatera hello resten av dina `articles` code toouse `comment`.</span><span class="sxs-lookup"><span data-stu-id="02832-263">Update hello rest of your `articles` code toouse `comment`.</span></span>

<span data-ttu-id="02832-264">Det finns fem filer som du behöver toomodify: hello server domänkontrollant och hello fyra klienten vyer.</span><span class="sxs-lookup"><span data-stu-id="02832-264">There are five files you need toomodify: hello server controller and hello four client views.</span></span> 

<span data-ttu-id="02832-265">Öppna _modules/articles/server/controllers/articles.server.controller.js_.</span><span class="sxs-lookup"><span data-stu-id="02832-265">Open _modules/articles/server/controllers/articles.server.controller.js_.</span></span>

<span data-ttu-id="02832-266">I hello `update` fungera, lägga till en tilldelning för `article.comment`.</span><span class="sxs-lookup"><span data-stu-id="02832-266">In hello `update` function, add an assignment for `article.comment`.</span></span> <span data-ttu-id="02832-267">hello följande kod visar hello slutförts `update` funktionen:</span><span class="sxs-lookup"><span data-stu-id="02832-267">hello following code shows hello completed `update` function:</span></span>

```javascript
exports.update = function (req, res) {
  var article = req.article;

  article.title = req.body.title;
  article.content = req.body.content;
  article.comment = req.body.comment;

  ...
};
```

<span data-ttu-id="02832-268">Öppna _modules/articles/client/views/view-article.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="02832-268">Open _modules/articles/client/views/view-article.client.view.html_.</span></span>

<span data-ttu-id="02832-269">Ovanför hello avslutande `</section>` tagg, Lägg till följande rad toodisplay hello `comment` tillsammans med hello resten av hello artikeldata:</span><span class="sxs-lookup"><span data-stu-id="02832-269">Just above hello closing `</section>` tag, add hello following line toodisplay `comment` along with hello rest of hello article data:</span></span>

```HTML
<p class="lead" ng-bind="vm.article.comment"></p>
```

<span data-ttu-id="02832-270">Öppna _modules/articles/client/views/list-articles.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="02832-270">Open _modules/articles/client/views/list-articles.client.view.html_.</span></span>

<span data-ttu-id="02832-271">Ovanför hello avslutande `</a>` tagg, Lägg till följande rad toodisplay hello `comment` tillsammans med hello resten av hello artikeldata:</span><span class="sxs-lookup"><span data-stu-id="02832-271">Just above hello closing `</a>` tag, add hello following line toodisplay `comment` along with hello rest of hello article data:</span></span>

```HTML
<p class="list-group-item-text" ng-bind="article.comment"></p>
```

<span data-ttu-id="02832-272">Öppna _modules/articles/client/views/admin/list-articles.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="02832-272">Open _modules/articles/client/views/admin/list-articles.client.view.html_.</span></span>

<span data-ttu-id="02832-273">I hello `<div class="list-group">` element och ovanför hello avslutande `</a>` tagg, Lägg till följande rad toodisplay hello `comment` tillsammans med hello resten av hello artikeldata:</span><span class="sxs-lookup"><span data-stu-id="02832-273">Inside hello `<div class="list-group">` element and just above hello closing `</a>` tag, add hello following line toodisplay `comment` along with hello rest of hello article data:</span></span>

```HTML
<p class="list-group-item-text" data-ng-bind="article.comment"></p>
```

<span data-ttu-id="02832-274">Öppna _modules/articles/client/views/admin/form-article.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="02832-274">Open _modules/articles/client/views/admin/form-article.client.view.html_.</span></span>

<span data-ttu-id="02832-275">Hitta hello `<div class="form-group">` element som innehåller hello knappen Skicka som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="02832-275">Find hello `<div class="form-group">` element that contains hello submit button, which looks like this:</span></span>

```HTML
<div class="form-group">
  <button type="submit" class="btn btn-default">{{vm.article._id ? 'Update' : 'Create'}}</button>
</div>
```

<span data-ttu-id="02832-276">Lägga till en annan ovanför den här taggen `<div class="form-group">` element som används för att redigera hello `comment` fältet.</span><span class="sxs-lookup"><span data-stu-id="02832-276">Just above this tag, add another `<div class="form-group">` element that lets people edit hello `comment` field.</span></span> <span data-ttu-id="02832-277">Din nya element ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="02832-277">Your new element should look like this:</span></span>

```HTML
<div class="form-group">
  <label class="control-label" for="comment">Comment</label>
  <textarea name="comment" data-ng-model="vm.article.comment" id="comment" class="form-control" cols="30" rows="10" placeholder="Comment"></textarea>
</div>
```

### <a name="test-your-changes-locally"></a><span data-ttu-id="02832-278">Testa ändringarna lokalt</span><span class="sxs-lookup"><span data-stu-id="02832-278">Test your changes locally</span></span>

<span data-ttu-id="02832-279">Spara alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="02832-279">Save all your changes.</span></span>

<span data-ttu-id="02832-280">Testa dina ändringar i Produktionsläge igen.</span><span class="sxs-lookup"><span data-stu-id="02832-280">Test your changes in production mode again.</span></span>

```bash
gulp prod
NODE_ENV=production node server.js
```

> [!NOTE]
> <span data-ttu-id="02832-281">Kom ihåg att din _config/env/production.js_ har återställts och hello `MONGODB_URI` miljövariabeln anges endast i ditt Azure webbapp och inte på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="02832-281">Remember that your _config/env/production.js_ has been reverted, and hello `MONGODB_URI` environment variable is only set in your Azure web app and not on your local machine.</span></span> <span data-ttu-id="02832-282">Om du tittar på hello config-fil som du hittar den hello produktion configuration standardvärden toouse en lokal MongoDB-databas.</span><span class="sxs-lookup"><span data-stu-id="02832-282">If you look at hello config file, you find that hello production configuration defaults toouse a local MongoDB database.</span></span> <span data-ttu-id="02832-283">Detta säkerställer att du inte rör produktionsdata när du testar din kodändringar lokalt.</span><span class="sxs-lookup"><span data-stu-id="02832-283">This makes sure that you don't touch production data when you test your code changes locally.</span></span>

<span data-ttu-id="02832-284">Navigera för`http://localhost:8443` i en webbläsare och se till att du har loggat in.</span><span class="sxs-lookup"><span data-stu-id="02832-284">Navigate too`http://localhost:8443` in a browser and make sure that you're signed in.</span></span>

<span data-ttu-id="02832-285">Välj **Admin > Hantera artiklar**, lägga till en artikel genom att välja hello  **+**  knappen.</span><span class="sxs-lookup"><span data-stu-id="02832-285">Select **Admin > Manage Articles**, then add an article by selecting hello **+** button.</span></span>

<span data-ttu-id="02832-286">Du ser hello nya `Comment` textruta nu.</span><span class="sxs-lookup"><span data-stu-id="02832-286">You see hello new `Comment` textbox now.</span></span>

![Tillagda kommentar fältet tooArticles](./media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field.png)

<span data-ttu-id="02832-288">Stoppa Node.js genom att skriva i hello terminal, `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="02832-288">In hello terminal, stop Node.js by typing `Ctrl+C`.</span></span> 

### <a name="publish-changes-tooazure"></a><span data-ttu-id="02832-289">Publicera ändringar tooAzure</span><span class="sxs-lookup"><span data-stu-id="02832-289">Publish changes tooAzure</span></span>

<span data-ttu-id="02832-290">Genomför ändringarna i Git och sedan push hello kod ändringar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="02832-290">Commit your changes in Git, then push hello code changes tooAzure.</span></span>

```bash
git commit -am "added article comment"
git push azure master
```

<span data-ttu-id="02832-291">En gång hello `git push` är klar, navigera tooyour Azure webbapp och testa hello nya funktioner.</span><span class="sxs-lookup"><span data-stu-id="02832-291">Once hello `git push` is complete, navigate tooyour Azure web app and try out hello new functionality.</span></span>

![Ändringar i modellen och databasen publicerade tooAzure](media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field-published.png)

<span data-ttu-id="02832-293">Om du lagt till alla artiklar tidigare kan kan du fortfarande se dem.</span><span class="sxs-lookup"><span data-stu-id="02832-293">If you added any articles earlier, you still can see them.</span></span> <span data-ttu-id="02832-294">Befintliga data i Cosmos-databasen är inte förlorade.</span><span class="sxs-lookup"><span data-stu-id="02832-294">Existing data in your Cosmos DB is not lost.</span></span> <span data-ttu-id="02832-295">Dessutom uppdateringar toohello dataschemat och lämnar företaget dina befintliga data.</span><span class="sxs-lookup"><span data-stu-id="02832-295">Also, your updates toohello data schema and leaves your existing data intact.</span></span>

## <a name="stream-diagnostic-logs"></a><span data-ttu-id="02832-296">Dataströmmen diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="02832-296">Stream diagnostic logs</span></span> 

<span data-ttu-id="02832-297">När din Node.js-program körs i Azure App Service, kan du få hello konsolen loggar via rörledningar tooyour terminal.</span><span class="sxs-lookup"><span data-stu-id="02832-297">While your Node.js application runs in Azure App Service, you can get hello console logs piped tooyour terminal.</span></span> <span data-ttu-id="02832-298">På så sätt kan du hello diagnostiska meddelanden för samma toohelp du felsöka programfel.</span><span class="sxs-lookup"><span data-stu-id="02832-298">That way, you can get hello same diagnostic messages toohelp you debug application errors.</span></span>

<span data-ttu-id="02832-299">toostart loggen direktuppspelning, Använd hello [az webapp loggen pilslut](/cli/azure/webapp/log#tail) kommando.</span><span class="sxs-lookup"><span data-stu-id="02832-299">toostart log streaming, use hello [az webapp log tail](/cli/azure/webapp/log#tail) command.</span></span>

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
``` 

<span data-ttu-id="02832-300">Uppdatera din Azure-webbapp i hello webbläsare tooget vissa webbtrafik när loggen streaming har startats.</span><span class="sxs-lookup"><span data-stu-id="02832-300">Once log streaming has started, refresh your Azure web app in hello browser tooget some web traffic.</span></span> <span data-ttu-id="02832-301">Du kan nu se loggarna för konsolen skickas tooyour terminal.</span><span class="sxs-lookup"><span data-stu-id="02832-301">You now see console logs piped tooyour terminal.</span></span>

<span data-ttu-id="02832-302">Stopploggen strömning när som helst genom att skriva `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="02832-302">Stop log streaming at any time by typing `Ctrl+C`.</span></span> 

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="02832-303">Hantera Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="02832-303">Manage your Azure web app</span></span>

<span data-ttu-id="02832-304">Gå toohello [Azure-portalen](https://portal.azure.com) toosee hello webbprogram som du skapade.</span><span class="sxs-lookup"><span data-stu-id="02832-304">Go toohello [Azure portal](https://portal.azure.com) toosee hello web app you created.</span></span>

<span data-ttu-id="02832-305">Hello vänstra menyn klickar du på **Apptjänster**, klicka på hello namnet på din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="02832-305">From hello left menu, click **App Services**, then click hello name of your Azure web app.</span></span>

![Portalen navigering tooAzure webbprogram](./media/app-service-web-tutorial-nodejs-mongodb-app/access-portal.png)

<span data-ttu-id="02832-307">Som standard visar hello portal ditt webbprogram **översikt** sidan.</span><span class="sxs-lookup"><span data-stu-id="02832-307">By default, hello portal shows your web app's **Overview** page.</span></span> <span data-ttu-id="02832-308">På den här sidan får du en översikt över hur det går för appen.</span><span class="sxs-lookup"><span data-stu-id="02832-308">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="02832-309">Här kan du också utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="02832-309">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="02832-310">hello flikar på hello vänster hello sidan visar hello annan konfigurationssidor som du kan öppna.</span><span class="sxs-lookup"><span data-stu-id="02832-310">hello tabs on hello left side of hello page show hello different configuration pages you can open.</span></span>

![App Service-sidan på Azure Portal](./media/app-service-web-tutorial-nodejs-mongodb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a><span data-ttu-id="02832-312">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="02832-312">Next steps</span></span>

<span data-ttu-id="02832-313">Vad du lärt dig:</span><span class="sxs-lookup"><span data-stu-id="02832-313">What you learned:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="02832-314">Skapa en MongoDB-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="02832-314">Create a MongoDB database in Azure</span></span>
> * <span data-ttu-id="02832-315">Ansluta tooMongoDB en Node.js-app</span><span class="sxs-lookup"><span data-stu-id="02832-315">Connect a Node.js app tooMongoDB</span></span>
> * <span data-ttu-id="02832-316">Distribuera hello app tooAzure</span><span class="sxs-lookup"><span data-stu-id="02832-316">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="02832-317">Uppdatera hello-datamodell och omdistribuera hello app</span><span class="sxs-lookup"><span data-stu-id="02832-317">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="02832-318">Dataströmmen loggas från Azure tooyour terminal</span><span class="sxs-lookup"><span data-stu-id="02832-318">Stream logs from Azure tooyour terminal</span></span>
> * <span data-ttu-id="02832-319">Hantera hello appen i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="02832-319">Manage hello app in hello Azure portal</span></span>

<span data-ttu-id="02832-320">I förväg toohello nästa självstudiekurs toolearn hur toomap en anpassad DNS namn tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="02832-320">Advance toohello next tutorial toolearn how toomap a custom DNS name tooyour web app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="02832-321">Mappa en befintlig anpassad DNS-namnet tooAzure Web Apps</span><span class="sxs-lookup"><span data-stu-id="02832-321">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
