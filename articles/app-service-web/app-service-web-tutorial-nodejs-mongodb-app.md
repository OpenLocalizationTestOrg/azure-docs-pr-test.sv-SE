---
title: Skapa en Node.js och MongoDB-webbapp i Azure | Microsoft Docs
description: "Lär dig hur du hämtar en Node.js-app som arbetar i Azure, med anslutning till en Cosmos-DB-databas med en anslutningssträng för MongoDB."
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
ms.openlocfilehash: 3b309382be8cdf8d48b396207fd482a5dc5ed934
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="build-a-nodejs-and-mongodb-web-app-in-azure"></a><span data-ttu-id="b7f1d-103">Skapa en Node.js och MongoDB-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="b7f1d-103">Build a Node.js and MongoDB web app in Azure</span></span>

<span data-ttu-id="b7f1d-104">Azure Web Apps ger en mycket skalbar, automatisk uppdatering värdtjänst.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="b7f1d-105">Den här kursen visar hur du skapar en Node.js-webbapp i Azure och ansluta den till en MongoDB-databas.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-105">This tutorial shows how to create a Node.js web app in Azure and connect it to a MongoDB database.</span></span> <span data-ttu-id="b7f1d-106">När du är klar har du en medelvärde program (MongoDB, snabb, AngularJS och Node.js) som körs i [Azure App Service](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b7f1d-106">When you're done, you'll have a MEAN application (MongoDB, Express, AngularJS, and Node.js) running in [Azure App Service](app-service-web-overview.md).</span></span> <span data-ttu-id="b7f1d-107">För enkelhetens skull exempelprogrammet använder den [MEAN.js webbramverk](http://meanjs.org/).</span><span class="sxs-lookup"><span data-stu-id="b7f1d-107">For simplicity, the sample application uses the [MEAN.js web framework](http://meanjs.org/).</span></span>

![MEAN.js-app som körs i Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

<span data-ttu-id="b7f1d-109">Vad du lära dig:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-109">What you'll learn:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b7f1d-110">Skapa en MongoDB-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="b7f1d-110">Create a MongoDB database in Azure</span></span>
> * <span data-ttu-id="b7f1d-111">Ansluta en Node.js-app till MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7f1d-111">Connect a Node.js app to MongoDB</span></span>
> * <span data-ttu-id="b7f1d-112">Distribuera appen till Azure</span><span class="sxs-lookup"><span data-stu-id="b7f1d-112">Deploy the app to Azure</span></span>
> * <span data-ttu-id="b7f1d-113">Uppdatera datamodellen och distribuera appen</span><span class="sxs-lookup"><span data-stu-id="b7f1d-113">Update the data model and redeploy the app</span></span>
> * <span data-ttu-id="b7f1d-114">Dataströmmen diagnostiska loggar från Azure</span><span class="sxs-lookup"><span data-stu-id="b7f1d-114">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="b7f1d-115">Hantera appen i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b7f1d-115">Manage the app in the Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7f1d-116">Krav</span><span class="sxs-lookup"><span data-stu-id="b7f1d-116">Prerequisites</span></span>

<span data-ttu-id="b7f1d-117">För att slutföra den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-117">To complete this tutorial:</span></span>

1. [<span data-ttu-id="b7f1d-118">Installera Git</span><span class="sxs-lookup"><span data-stu-id="b7f1d-118">Install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="b7f1d-119">Installera Node.js och NPM</span><span class="sxs-lookup"><span data-stu-id="b7f1d-119">Install Node.js and NPM</span></span>](https://nodejs.org/)
1. <span data-ttu-id="b7f1d-120">[Installera Gulp.js](http://gulpjs.com/) (krävs av [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))</span><span class="sxs-lookup"><span data-stu-id="b7f1d-120">[Install Gulp.js](http://gulpjs.com/) (required by [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))</span></span>
1. [<span data-ttu-id="b7f1d-121">Installera och köra MongoDB Community Edition</span><span class="sxs-lookup"><span data-stu-id="b7f1d-121">Install and run MongoDB Community Edition</span></span>](https://docs.mongodb.com/manual/administration/install-community/) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b7f1d-122">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-122">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="b7f1d-123">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-123">Run `az --version` to find the version.</span></span> <span data-ttu-id="b7f1d-124">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b7f1d-124">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="test-local-mongodb"></a><span data-ttu-id="b7f1d-125">Testa lokala MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7f1d-125">Test local MongoDB</span></span>

<span data-ttu-id="b7f1d-126">Öppna fönstret terminal och `cd` till den `bin` katalogen MongoDB-installationen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-126">Open the terminal window and `cd` to the `bin` directory of your MongoDB installation.</span></span> <span data-ttu-id="b7f1d-127">Du kan använda den här terminalfönster för att köra alla kommandon i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-127">You can use this terminal window to run all the commands in this tutorial.</span></span>

<span data-ttu-id="b7f1d-128">Kör `mongo` i terminalen för att ansluta till din lokala MongoDB-servern.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-128">Run `mongo` in the terminal to connect to your local MongoDB server.</span></span>

```bash
mongo
```

<span data-ttu-id="b7f1d-129">Om anslutningen lyckas körs redan MongoDB-databas.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-129">If your connection is successful, then your MongoDB database is already running.</span></span> <span data-ttu-id="b7f1d-130">Om inte, kontrollera att den lokala MongoDB-databasen har startats genom att följa stegen i [installera MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/).</span><span class="sxs-lookup"><span data-stu-id="b7f1d-130">If not, make sure that your local MongoDB database is started by following the steps at [Install MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/).</span></span> <span data-ttu-id="b7f1d-131">Ofta MongoDB installeras, men du måste fortfarande starta genom att köra `mongod`.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-131">Often, MongoDB is installed, but you still need to start it by running `mongod`.</span></span> 

<span data-ttu-id="b7f1d-132">När du är klar testning MongoDB-databas, Skriv `Ctrl+C` i terminalen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-132">When you're done testing your MongoDB database, type `Ctrl+C` in the terminal.</span></span> 

## <a name="create-local-nodejs-app"></a><span data-ttu-id="b7f1d-133">Skapa lokal Node.js-app</span><span class="sxs-lookup"><span data-stu-id="b7f1d-133">Create local Node.js app</span></span>

<span data-ttu-id="b7f1d-134">I det här steget kan du ställa in det lokala Node.js-projektet.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-134">In this step, you set up the local Node.js project.</span></span>

### <a name="clone-the-sample-application"></a><span data-ttu-id="b7f1d-135">Klona exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="b7f1d-135">Clone the sample application</span></span>

<span data-ttu-id="b7f1d-136">I fönstret terminal `cd` till en arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-136">In the terminal window, `cd` to a working directory.</span></span>  

<span data-ttu-id="b7f1d-137">Klona exempellagringsplatsen med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-137">Run the following command to clone the sample repository.</span></span> 

```bash
git clone https://github.com/Azure-Samples/meanjs.git
```

<span data-ttu-id="b7f1d-138">Det här exemplet databasen innehåller en kopia av den [MEAN.js databasen](https://github.com/meanjs/mean).</span><span class="sxs-lookup"><span data-stu-id="b7f1d-138">This sample repository contains a copy of the [MEAN.js repository](https://github.com/meanjs/mean).</span></span> <span data-ttu-id="b7f1d-139">Den ändras för att köras på Apptjänst (Mer information finns i databasen MEAN.js [filen Viktigt](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).</span><span class="sxs-lookup"><span data-stu-id="b7f1d-139">It is modified to run on App Service (for more information, see the MEAN.js repository [README file](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).</span></span>

### <a name="run-the-application"></a><span data-ttu-id="b7f1d-140">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="b7f1d-140">Run the application</span></span>

<span data-ttu-id="b7f1d-141">Kör följande kommandon för att installera de nödvändiga paketen och starta programmet.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-141">Run the following commands to install the required packages and start the application.</span></span>

```bash
cd meanjs
npm install
npm start
```

<span data-ttu-id="b7f1d-142">När appen har lästs in helt, se något som liknar följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-142">When the app is fully loaded, you see something similar to the following message:</span></span>

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

<span data-ttu-id="b7f1d-143">Gå till http://localhost: 3000 i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-143">Navigate to http://localhost:3000 in a browser.</span></span> <span data-ttu-id="b7f1d-144">Klicka på **registrera dig** i den översta menyn och skapa en testanvändare.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-144">Click **Sign Up** in the top menu and create a test user.</span></span> 

<span data-ttu-id="b7f1d-145">MEAN.js-exempelprogrammet lagrar användardata i databasen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-145">The MEAN.js sample application stores user data in the database.</span></span> <span data-ttu-id="b7f1d-146">Om du lyckas skapa en användare och logga in sedan skriver din app data till den lokala MongoDB-databasen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-146">If you are successful at creating a user and signing in, then your app is writing data to the local MongoDB database.</span></span>

![MEAN.js ansluter till MongoDB](./media/app-service-web-tutorial-nodejs-mongodb-app/mongodb-connect-success.png)

<span data-ttu-id="b7f1d-148">Välj **Admin > Hantera artiklar** att lägga till vissa artiklar.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-148">Select **Admin > Manage Articles** to add some articles.</span></span>

<span data-ttu-id="b7f1d-149">Stoppa Node.js när som helst genom att trycka på `Ctrl+C` i terminalen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-149">To stop Node.js at any time, press `Ctrl+C` in the terminal.</span></span> 

## <a name="create-production-mongodb"></a><span data-ttu-id="b7f1d-150">Skapa produktion MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7f1d-150">Create production MongoDB</span></span>

<span data-ttu-id="b7f1d-151">I det här steget skapar du en MongoDB-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-151">In this step, you create a MongoDB database in Azure.</span></span> <span data-ttu-id="b7f1d-152">När appen har distribuerats till Azure, använder den här databasen i molnet.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-152">When your app is deployed to Azure, it uses this cloud database.</span></span>

<span data-ttu-id="b7f1d-153">Den här kursen används för MongoDB, [Azure Cosmos DB](/azure/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="b7f1d-153">For MongoDB, this tutorial uses [Azure Cosmos DB](/azure/documentdb/).</span></span> <span data-ttu-id="b7f1d-154">Cosmos DB stöder MongoDB-klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-154">Cosmos DB supports MongoDB client connections.</span></span>

### <a name="log-in-to-azure"></a><span data-ttu-id="b7f1d-155">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="b7f1d-155">Log in to Azure</span></span>

<span data-ttu-id="b7f1d-156">Du använder Azure CLI 2.0 till att skapa de resurser som behövs när Azure ska vara värd för din app.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-156">You'll use the Azure CLI 2.0 to create the resources needed to host your app in Azure.</span></span> <span data-ttu-id="b7f1d-157">Logga in på Azure-prenumerationen med kommandot [az login](/cli/azure/#login) och följ anvisningarna på skärmen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-157">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span>

```azurecli-interactive
az login
```   

### <a name="create-a-resource-group"></a><span data-ttu-id="b7f1d-158">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="b7f1d-158">Create a resource group</span></span>

<span data-ttu-id="b7f1d-159">Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="b7f1d-159">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

<span data-ttu-id="b7f1d-160">Följande exempel skapar en resursgrupp i regionen västeuropa.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-160">The following example creates a resource group in the West Europe region.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

<span data-ttu-id="b7f1d-161">Använd den [az apptjänst lista-platser](/cli/azure/appservice#list-locations) Azure CLI-kommando för att lista över tillgängliga platser.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-161">Use the [az appservice list-locations](/cli/azure/appservice#list-locations) Azure CLI command to list available locations.</span></span> 

### <a name="create-a-cosmos-db-account"></a><span data-ttu-id="b7f1d-162">Skapa ett Cosmos-DB-konto</span><span class="sxs-lookup"><span data-stu-id="b7f1d-162">Create a Cosmos DB account</span></span>

<span data-ttu-id="b7f1d-163">Skapa ett Cosmos-DB-konto med den [az cosmosdb skapa](/cli/azure/cosmosdb#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-163">Create a Cosmos DB account with the [az cosmosdb create](/cli/azure/cosmosdb#create) command.</span></span>

<span data-ttu-id="b7f1d-164">I följande kommando i stället använda ett unikt Cosmos-databasnamn för den  *\<cosmosdb_name >* platshållare.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-164">In the following command, substitute a unique Cosmos DB name for the *\<cosmosdb_name>* placeholder.</span></span> <span data-ttu-id="b7f1d-165">Det här namnet används som en del av Cosmos-DB-slutpunkten `https://<cosmosdb_name>.documents.azure.com/`, så namnet måste vara unikt för alla Cosmos-DB-konton i Azure.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-165">This name is used as the part of the Cosmos DB endpoint, `https://<cosmosdb_name>.documents.azure.com/`, so the name needs to be unique across all Cosmos DB accounts in Azure.</span></span> <span data-ttu-id="b7f1d-166">Namnet måste innehålla endast små bokstäver, siffror och bindestreck (-) och måste vara mellan 3 och 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-166">The name must contain only lowercase letters, numbers, and the hyphen (-) character, and must be between 3 and 50 characters long.</span></span>

```azurecli-interactive
az cosmosdb create \
    --name <cosmosdb_name> \
    --resource-group myResourceGroup \
    --kind MongoDB
```

<span data-ttu-id="b7f1d-167">Den *--kind MongoDB* parametern aktiverar MongoDB-klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-167">The *--kind MongoDB* parameter enables MongoDB client connections.</span></span>

<span data-ttu-id="b7f1d-168">När Cosmos-DB-kontot har skapats visas Azure CLI information liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-168">When the Cosmos DB account is created, the Azure CLI shows information similar to the following example:</span></span>

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

## <a name="connect-app-to-production-mongodb"></a><span data-ttu-id="b7f1d-169">Anslut appen till produktion MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7f1d-169">Connect app to production MongoDB</span></span>

<span data-ttu-id="b7f1d-170">I det här steget kan ansluta du MEAN.js exempelprogrammet till Cosmos-DB-databasen som du precis skapade, med hjälp av en anslutningssträng för MongoDB.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-170">In this step, you connect your MEAN.js sample application to the Cosmos DB database you just created, using a MongoDB connection string.</span></span> 

### <a name="retrieve-the-database-key"></a><span data-ttu-id="b7f1d-171">Hämta databasens nyckel</span><span class="sxs-lookup"><span data-stu-id="b7f1d-171">Retrieve the database key</span></span>

<span data-ttu-id="b7f1d-172">För att ansluta till databasen Cosmos DB, måste databasnyckeln för.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-172">To connect to the Cosmos DB database, you need the database key.</span></span> <span data-ttu-id="b7f1d-173">Använd kommandot [az documentdb list-keys](/cli/azure/cosmosdb#list-keys) för att hämta den primära nyckeln.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-173">Use the [az cosmosdb list-keys](/cli/azure/cosmosdb#list-keys) command to retrieve the primary key.</span></span>

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

<span data-ttu-id="b7f1d-174">Azure CLI visar information som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-174">The Azure CLI shows information similar to the following example:</span></span>

```json
{
  "primaryMasterKey": "RS4CmUwzGRASJPMoc0kiEvdnKmxyRILC9BWisAYh3Hq4zBYKr0XQiSE4pqx3UchBeO4QRCzUt1i7w0rOkitoJw==",
  "primaryReadonlyMasterKey": "HvitsjIYz8TwRmIuPEUAALRwqgKOzJUjW22wPL2U8zoMVhGvregBkBk9LdMTxqBgDETSq7obbwZtdeFY7hElTg==",
  "secondaryMasterKey": "Lu9aeZTiXU4PjuuyGBbvS1N9IRG3oegIrIh95U6VOstf9bJiiIpw3IfwSUgQWSEYM3VeEyrhHJ4rn3Ci0vuFqA==",
  "secondaryReadonlyMasterKey": "LpsCicpVZqHRy7qbMgrzbRKjbYCwCKPQRl0QpgReAOxMcggTvxJFA94fTi0oQ7xtxpftTJcXkjTirQ0pT7QFrQ=="
}
```

Kopiera värdet för `primaryMasterKey`. <span data-ttu-id="b7f1d-176">Du behöver den här informationen i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-176">You need this information in the next step.</span></span>

<a name="devconfig"></a>
### <a name="configure-the-connection-string-in-your-nodejs-application"></a><span data-ttu-id="b7f1d-177">Konfigurera anslutningssträngen i ditt Node.js-program</span><span class="sxs-lookup"><span data-stu-id="b7f1d-177">Configure the connection string in your Node.js application</span></span>

<span data-ttu-id="b7f1d-178">Öppna din databas MEAN.js _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-178">In your MEAN.js repository, open _config/env/production.js_.</span></span>

<span data-ttu-id="b7f1d-179">I den `db` objekt, uppdatera värdet i `uri`:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-179">In the `db` object, update the value of `uri`:</span></span>

* <span data-ttu-id="b7f1d-180">Ersätt två  *\<cosmosdb_name >* platshållare med databasnamnet Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-180">Replace the two *\<cosmosdb_name>* placeholders with your Cosmos DB database name.</span></span>
* <span data-ttu-id="b7f1d-181">Ersätt den  *\<primary_master_key >* med den nyckel som du kopierade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-181">Replace the *\<primary_master_key>* placeholder with the key you copied in the previous step.</span></span>

<span data-ttu-id="b7f1d-182">Följande kod visar den `db` objekt:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-182">The following code shows the `db` object:</span></span>

```javascript
db: {
  uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false',
  ...
},
```

<span data-ttu-id="b7f1d-183">Den `ssl=true` alternativet krävs eftersom [Cosmos DB kräver SSL](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span><span class="sxs-lookup"><span data-stu-id="b7f1d-183">The `ssl=true` option is required because [Cosmos DB requires SSL](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span></span> 

<span data-ttu-id="b7f1d-184">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-184">Save your changes.</span></span>

### <a name="test-the-application-in-production-mode"></a><span data-ttu-id="b7f1d-185">Testa programmet i Produktionsläge</span><span class="sxs-lookup"><span data-stu-id="b7f1d-185">Test the application in production mode</span></span> 

<span data-ttu-id="b7f1d-186">Kör följande kommando för att minify och sedan paketera skript för produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-186">Run the following command to minify and bundle scripts for the production environment.</span></span> <span data-ttu-id="b7f1d-187">Den här processen genererar filer som behövs i produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-187">This process generates the files needed by the production environment.</span></span>

```bash
gulp prod
```

<span data-ttu-id="b7f1d-188">Kör följande kommando för att använda den anslutningssträng som du konfigurerade i _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-188">Run the following command to use the connection string you configured in _config/env/production.js_.</span></span>

```bash
NODE_ENV=production node server.js
```

<span data-ttu-id="b7f1d-189">`NODE_ENV=production`anger miljövariabeln som talar om Node.js körs i produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-189">`NODE_ENV=production` sets the environment variable that tells Node.js to run in the production environment.</span></span>  <span data-ttu-id="b7f1d-190">`node server.js`startar Node.js-server med `server.js` i Lagringsplatsens rot.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-190">`node server.js` starts the Node.js server with `server.js` in your repository root.</span></span> <span data-ttu-id="b7f1d-191">Detta är hur Node.js-programmet läses in i Azure.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-191">This is how your Node.js application is loaded in Azure.</span></span> 

<span data-ttu-id="b7f1d-192">När appen har lästs in, kontrollera att den körs i produktionsmiljön:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-192">When the app is loaded, check to make sure that it's running in the production environment:</span></span>

```
--
MEAN.JS

Environment:     production
Server:          http://0.0.0.0:8443
Database:        mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false
App version:     0.5.0
MEAN.JS version: 0.5.0
```

<span data-ttu-id="b7f1d-193">Navigera till http://localhost:8443 i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-193">Navigate to http://localhost:8443 in a browser.</span></span> <span data-ttu-id="b7f1d-194">Klicka på **registrera dig** i den översta menyn och skapa en testanvändare.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-194">Click **Sign Up** in the top menu and create a test user.</span></span> <span data-ttu-id="b7f1d-195">Om du lyckas skapa en användare och logga in, sedan skriver din app data till Cosmos-DB-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-195">If you are successful creating a user and signing in, then your app is writing data to the Cosmos DB database in Azure.</span></span> 

<span data-ttu-id="b7f1d-196">Stoppa Node.js genom att skriva till terminalen och `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-196">In the terminal, stop Node.js by typing `Ctrl+C`.</span></span> 

## <a name="deploy-app-to-azure"></a><span data-ttu-id="b7f1d-197">Distribuera appen till Azure</span><span class="sxs-lookup"><span data-stu-id="b7f1d-197">Deploy app to Azure</span></span>

<span data-ttu-id="b7f1d-198">I det här steget kan distribuera du MongoDB-anslutna Node.js-programmet till Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-198">In this step, you deploy your MongoDB-connected Node.js application to Azure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="b7f1d-199">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="b7f1d-199">Create an App Service plan</span></span>

<span data-ttu-id="b7f1d-200">Skapa en App Service-plan med kommandot [az appservice plan create](/cli/azure/appservice/plan#create).</span><span class="sxs-lookup"><span data-stu-id="b7f1d-200">Create an App Service plan with the [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="b7f1d-201">I följande exempel skapas en apptjänstplan med namnet _myAppServicePlan_ med hjälp av den **lediga** prisnivån:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-201">The following example creates an App Service plan named _myAppServicePlan_ using the **FREE** pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

<span data-ttu-id="b7f1d-202">När programtjänstplanen har skapats visas Azure CLI information liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-202">When the App Service plan is created, the Azure CLI shows information similar to the following example:</span></span>

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

### <a name="create-a-web-app"></a><span data-ttu-id="b7f1d-203">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="b7f1d-203">Create a web app</span></span>

<span data-ttu-id="b7f1d-204">Skapa en webbapp i den `myAppServicePlan` App Service-plan med de [az webapp skapa](/cli/azure/webapp#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-204">Create a web app in the `myAppServicePlan` App Service plan with the [az webapp create](/cli/azure/webapp#create) command.</span></span> 

<span data-ttu-id="b7f1d-205">Webbappen ger dig en värd med utrymme för att distribuera din kod och ger en URL som du kan visa det distribuerade programmet.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-205">The web app gives you a hosting space to deploy your code and provides a URL for you to view the deployed application.</span></span> <span data-ttu-id="b7f1d-206">Använd för att skapa webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-206">Use  to create the web app.</span></span> 

<span data-ttu-id="b7f1d-207">I följande kommando och ersätter den  *\<appnamn >* med ett unikt appnamn.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-207">In the following command, replace the *\<app_name>* placeholder with a unique app name.</span></span> <span data-ttu-id="b7f1d-208">Det här namnet används som en del av standard-URL för webbapp så att namnet måste vara unikt över alla program i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-208">This name is used as the part of the default URL for the web app, so the name needs to be unique across all apps in Azure App Service.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="b7f1d-209">När webbappen har skapats visar Azure CLI information liknande den i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-209">When the web app has been created, the Azure CLI shows information similar to the following example:</span></span> 

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

### <a name="configure-an-environment-variable"></a><span data-ttu-id="b7f1d-210">Konfigurera en miljövariabel</span><span class="sxs-lookup"><span data-stu-id="b7f1d-210">Configure an environment variable</span></span>

<span data-ttu-id="b7f1d-211">Tidigare i självstudierna hårdkodad databasanslutningen sträng i _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-211">Earlier in the tutorial, you hardcoded the database connection string in _config/env/production.js_.</span></span> <span data-ttu-id="b7f1d-212">Inom ramen för säkerhets skull bör vill du behålla dessa känsliga data utanför Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-212">In keeping with security best practice, you want to keep this sensitive data out of your Git repository.</span></span> <span data-ttu-id="b7f1d-213">För din app som körs i Azure, ska du använda en miljövariabel i stället.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-213">For your app running in Azure, you'll use an environment variable instead.</span></span>

<span data-ttu-id="b7f1d-214">I App Service som du anger miljövariabler som _appinställningar_ med hjälp av den [az webapp config appsettings uppdatera](/cli/azure/webapp/config/appsettings#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-214">In App Service, you set environment variables as _app settings_ by using the [az webapp config appsettings update](/cli/azure/webapp/config/appsettings#update) command.</span></span> 

<span data-ttu-id="b7f1d-215">I följande exempel konfigureras en `MONGODB_URI` appinställningen i ditt Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-215">The following example configures a `MONGODB_URI` app setting in your Azure web app.</span></span> <span data-ttu-id="b7f1d-216">Ersätt den  *\<appnamn >*,  *\<cosmosdb_name >*, och  *\<primary_master_key >* platshållare.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-216">Replace the *\<app_name>*, *\<cosmosdb_name>*, and *\<primary_master_key>* placeholders.</span></span>

```azurecli-interactive
az webapp config appsettings update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

<span data-ttu-id="b7f1d-217">I Node.js-kod du åtkomst till den här appinställning med `process.env.MONGODB_URI`, precis som du skulle använda en miljövariabel.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-217">In Node.js code, you access this app setting with `process.env.MONGODB_URI`, just like you would access any environment variable.</span></span> 

<span data-ttu-id="b7f1d-218">Nu kan ångra ändringarna till _config/env/production.js_ med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-218">Now, undo your changes to _config/env/production.js_ with the following command:</span></span>

```bash
git checkout -- .
```

<span data-ttu-id="b7f1d-219">Öppna _config/env/production.js_ igen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-219">Open _config/env/production.js_ again.</span></span> <span data-ttu-id="b7f1d-220">Observera att MEAN.js standardappen har redan konfigurerats för att använda den `MONGODB_URI` miljövariabel som du skapade.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-220">Note that the default MEAN.js app is already configured to use the `MONGODB_URI` environment variable that you created.</span></span>

```javascript
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```

### <a name="configure-local-git-deployment"></a><span data-ttu-id="b7f1d-221">Konfigurera lokal git-distribution</span><span class="sxs-lookup"><span data-stu-id="b7f1d-221">Configure local git deployment</span></span> 

<span data-ttu-id="b7f1d-222">Använd den [az webapp distributionsanvändare har angetts](/cli/azure/webapp/deployment/user#set) kommando för att skapa autentiseringsuppgifter för distribution.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-222">Use the [az webapp deployment user set](/cli/azure/webapp/deployment/user#set) command to create credentials for deployment.</span></span>

<span data-ttu-id="b7f1d-223">Du kan distribuera programmet till Azure App Service på olika sätt, inklusive FTP, lokal Git, GitHub, Visual Studio Team Services och BitBucket.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-223">You can deploy your application to Azure App Service in various ways including FTP, local Git, GitHub, Visual Studio Team Services, and BitBucket.</span></span> <span data-ttu-id="b7f1d-224">För FTP- och lokala Git är det nödvändigt att ha en distribution konfigurerade användaren på servern att autentisera din distribution.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-224">For FTP and local Git, it is necessary to have a deployment user configured on the server to authenticate your deployment.</span></span> <span data-ttu-id="b7f1d-225">Den här distributionen användaren är kontonivå och skiljer sig från ditt konto i Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-225">This deployment user is account-level and is different from your Azure subscription account.</span></span> <span data-ttu-id="b7f1d-226">Du behöver bara konfigurera den här distributionen användaren en gång.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-226">You only need to configure this deployment user once.</span></span>

<span data-ttu-id="b7f1d-227">I följande kommando ersätter du  *\<användarnamn >* och  *\<lösenord >* med ett nytt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-227">In the following command, replace *\<user-name>* and *\<password>* with a new user name and password.</span></span> <span data-ttu-id="b7f1d-228">Användarnamnet måste vara unikt.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-228">The user name must be unique.</span></span> <span data-ttu-id="b7f1d-229">Lösenordet måste innehålla minst åtta tecken, med två av följande tre element: bokstäver, siffror, symboler.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-229">The password must be at least eight characters long, with two of the following three elements:  letters, numbers, symbols.</span></span> <span data-ttu-id="b7f1d-230">Om du ser felet ` 'Conflict'. Details: 409` ska du byta användarnamn.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-230">If you get a ` 'Conflict'. Details: 409` error, change the username.</span></span> <span data-ttu-id="b7f1d-231">Om du ser felet ` 'Bad Request'. Details: 400` ska du använda ett starkare lösenord.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-231">If you get a ` 'Bad Request'. Details: 400` error, use a stronger password.</span></span>

```azurecli-interactive
az appservice web deployment user set --user-name <username> --password <password>
```

<span data-ttu-id="b7f1d-232">Registrera användarnamn och lösenord som ska användas i senare steg när du distribuerar appen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-232">Record the user name and password for use in later steps when you deploy the app.</span></span>

<span data-ttu-id="b7f1d-233">Använd den [az webapp distribution källa config-lokal-git](/cli/azure/webapp/deployment/source#config-local-git) kommando för att konfigurera lokal Git-åtkomst till Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-233">Use the [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git) command to configure local Git access to the Azure web app.</span></span> 

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup
```

<span data-ttu-id="b7f1d-234">När användaren distribution är konfigurerad, visar Azure CLI distribution URL-Adressen för din Azure-webbapp i följande format:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-234">When the deployment user is configured, the Azure CLI shows the deployment URL for your Azure web app in the following format:</span></span>

```bash 
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git 
``` 

<span data-ttu-id="b7f1d-235">Kopiera utdata från terminalen och som kommer att användas i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-235">Copy the output from the terminal, as it will be used in the next step.</span></span> 

### <a name="push-to-azure-from-git"></a><span data-ttu-id="b7f1d-236">Skicka till Azure från Git</span><span class="sxs-lookup"><span data-stu-id="b7f1d-236">Push to Azure from Git</span></span>

<span data-ttu-id="b7f1d-237">Lägg till en Azure-fjärrdatabas till din lokala Git-databas.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-237">Add an Azure remote to your local Git repository.</span></span> 

```bash
git remote add azure <paste_copied_url_here> 
```

<span data-ttu-id="b7f1d-238">Skicka till Azure remote distribuerar Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-238">Push to the Azure remote to deploy your Node.js application.</span></span> <span data-ttu-id="b7f1d-239">Du uppmanas att ange lösenordet du angav tidigare. Det behövs för att skapa distributionsanvändaren.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-239">You will be prompted for the password you supplied earlier as part of the creation of the deployment user.</span></span> 

```bash
git push azure master
```

<span data-ttu-id="b7f1d-240">Under distributionen kommunicerar förloppet med Git i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-240">During deployment, Azure App Service communicates its progress with Git.</span></span>

```bash
Counting objects: 5, done.
Delta compression using up to 4 threads.
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
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
``` 

<span data-ttu-id="b7f1d-241">Det kan hända att distributionen körs [Gulp](http://gulpjs.com/) när `npm install`.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-241">You may notice that the deployment process runs [Gulp](http://gulpjs.com/) after `npm install`.</span></span> <span data-ttu-id="b7f1d-242">App Service körs inte Gulp eller Grunt uppgifter under distributionen, så att den här lagringsplatsen exempel har två ytterligare filer i rotkatalogen för att den:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-242">App Service does not run Gulp or Grunt tasks during deployment, so this sample repository has two additional files in its root directory to enable it:</span></span> 

- <span data-ttu-id="b7f1d-243">_.Deployment_ -den här filen talar om App Service för att köra `bash deploy.sh` som anpassade distributions-skriptet.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-243">_.deployment_ - This file tells App Service to run `bash deploy.sh` as the custom deployment script.</span></span>
- <span data-ttu-id="b7f1d-244">_Deploy.SH_ -skriptet anpassad distribution.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-244">_deploy.sh_ - The custom deployment script.</span></span> <span data-ttu-id="b7f1d-245">Om du granska filen ser du att den körs `gulp prod` när `npm install` och `bower install`.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-245">If you review the file, you will see that it runs `gulp prod` after `npm install` and `bower install`.</span></span> 

<span data-ttu-id="b7f1d-246">Du kan använda den här metoden för att lägga till något steg i distributionen Git-baserade.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-246">You can use this approach to add any step to your Git-based deployment.</span></span> <span data-ttu-id="b7f1d-247">Om du startar om din Azure-webbapp när som helst kör inte App Service dessa automation-aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-247">If you restart your Azure web app at any point, App Service doesn't rerun these automation tasks.</span></span>

### <a name="browse-to-the-azure-web-app"></a><span data-ttu-id="b7f1d-248">Bläddra till Azure-webbappen</span><span class="sxs-lookup"><span data-stu-id="b7f1d-248">Browse to the Azure web app</span></span> 

<span data-ttu-id="b7f1d-249">Bläddra till den distribuerade webbappens med hjälp av webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-249">Browse to the deployed web app using your web browser.</span></span> 

```bash 
http://<app_name>.azurewebsites.net 
``` 

<span data-ttu-id="b7f1d-250">Klicka på **registrera dig** i den översta menyn och skapa en dummy användare.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-250">Click **Sign Up** in the top menu and create a dummy user.</span></span> 

<span data-ttu-id="b7f1d-251">Om du har lyckats och appen automatiskt loggar in på den skapade användaren, har MEAN.js-app i Azure anslutningen till databasen för MongoDB (Cosmos-DB).</span><span class="sxs-lookup"><span data-stu-id="b7f1d-251">If you are successful and the app automatically signs in to the created user, then your MEAN.js app in Azure has connectivity to the MongoDB (Cosmos DB) database.</span></span> 

![MEAN.js-app som körs i Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

<span data-ttu-id="b7f1d-253">Välj **Admin > Hantera artiklar** att lägga till vissa artiklar.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-253">Select **Admin > Manage Articles** to add some articles.</span></span> 

<span data-ttu-id="b7f1d-254">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="b7f1d-254">**Congratulations!**</span></span> <span data-ttu-id="b7f1d-255">Du kör en datadrivna Node.js-app i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-255">You're running a data-driven Node.js app in Azure App Service.</span></span>

## <a name="update-data-model-and-redeploy"></a><span data-ttu-id="b7f1d-256">Uppdatera datamodellen och distribuera om</span><span class="sxs-lookup"><span data-stu-id="b7f1d-256">Update data model and redeploy</span></span>

<span data-ttu-id="b7f1d-257">I det här steget kan du ändra den `article` data modell och publicera ändringarna till Azure.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-257">In this step, you change the `article` data model and publish your change to Azure.</span></span>

### <a name="update-the-data-model"></a><span data-ttu-id="b7f1d-258">Uppdatera datamodellen</span><span class="sxs-lookup"><span data-stu-id="b7f1d-258">Update the data model</span></span>

<span data-ttu-id="b7f1d-259">Öppna _modules/articles/server/models/article.server.model.js_.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-259">Open _modules/articles/server/models/article.server.model.js_.</span></span>

<span data-ttu-id="b7f1d-260">I `ArticleSchema`, lägga till en `String` typ som kallas `comment`.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-260">In `ArticleSchema`, add a `String` type called `comment`.</span></span> <span data-ttu-id="b7f1d-261">När du är klar schemat koden se ut så här:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-261">When you're done, your schema code should look like this:</span></span>

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

### <a name="update-the-articles-code"></a><span data-ttu-id="b7f1d-262">Uppdatera artiklar kod</span><span class="sxs-lookup"><span data-stu-id="b7f1d-262">Update the articles code</span></span>

<span data-ttu-id="b7f1d-263">Uppdatera resten av dina `articles` kod för att använda `comment`.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-263">Update the rest of your `articles` code to use `comment`.</span></span>

<span data-ttu-id="b7f1d-264">Det finns fem filer som du behöver ändra: server-domänkontrollant och fyra klienten vyer.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-264">There are five files you need to modify: the server controller and the four client views.</span></span> 

<span data-ttu-id="b7f1d-265">Öppna _modules/articles/server/controllers/articles.server.controller.js_.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-265">Open _modules/articles/server/controllers/articles.server.controller.js_.</span></span>

<span data-ttu-id="b7f1d-266">I den `update` fungera, lägga till en tilldelning för `article.comment`.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-266">In the `update` function, add an assignment for `article.comment`.</span></span> <span data-ttu-id="b7f1d-267">Följande kod visar den färdiga `update` funktionen:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-267">The following code shows the completed `update` function:</span></span>

```javascript
exports.update = function (req, res) {
  var article = req.article;

  article.title = req.body.title;
  article.content = req.body.content;
  article.comment = req.body.comment;

  ...
};
```

<span data-ttu-id="b7f1d-268">Öppna _modules/articles/client/views/view-article.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-268">Open _modules/articles/client/views/view-article.client.view.html_.</span></span>

<span data-ttu-id="b7f1d-269">Ovanför avslutande `</section>` tagg, Lägg till följande rad att visa `comment` tillsammans med resten av artikeldata:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-269">Just above the closing `</section>` tag, add the following line to display `comment` along with the rest of the article data:</span></span>

```HTML
<p class="lead" ng-bind="vm.article.comment"></p>
```

<span data-ttu-id="b7f1d-270">Öppna _modules/articles/client/views/list-articles.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-270">Open _modules/articles/client/views/list-articles.client.view.html_.</span></span>

<span data-ttu-id="b7f1d-271">Ovanför avslutande `</a>` tagg, Lägg till följande rad att visa `comment` tillsammans med resten av artikeldata:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-271">Just above the closing `</a>` tag, add the following line to display `comment` along with the rest of the article data:</span></span>

```HTML
<p class="list-group-item-text" ng-bind="article.comment"></p>
```

<span data-ttu-id="b7f1d-272">Öppna _modules/articles/client/views/admin/list-articles.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-272">Open _modules/articles/client/views/admin/list-articles.client.view.html_.</span></span>

<span data-ttu-id="b7f1d-273">I den `<div class="list-group">` element och ovanför avslutande `</a>` tagg, Lägg till följande rad att visa `comment` tillsammans med resten av artikeldata:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-273">Inside the `<div class="list-group">` element and just above the closing `</a>` tag, add the following line to display `comment` along with the rest of the article data:</span></span>

```HTML
<p class="list-group-item-text" data-ng-bind="article.comment"></p>
```

<span data-ttu-id="b7f1d-274">Öppna _modules/articles/client/views/admin/form-article.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-274">Open _modules/articles/client/views/admin/form-article.client.view.html_.</span></span>

<span data-ttu-id="b7f1d-275">Hitta de `<div class="form-group">` element som innehåller skickaknappen som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-275">Find the `<div class="form-group">` element that contains the submit button, which looks like this:</span></span>

```HTML
<div class="form-group">
  <button type="submit" class="btn btn-default">{{vm.article._id ? 'Update' : 'Create'}}</button>
</div>
```

<span data-ttu-id="b7f1d-276">Lägga till en annan ovanför den här taggen `<div class="form-group">` element som används för att redigera den `comment` fältet.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-276">Just above this tag, add another `<div class="form-group">` element that lets people edit the `comment` field.</span></span> <span data-ttu-id="b7f1d-277">Din nya element ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-277">Your new element should look like this:</span></span>

```HTML
<div class="form-group">
  <label class="control-label" for="comment">Comment</label>
  <textarea name="comment" data-ng-model="vm.article.comment" id="comment" class="form-control" cols="30" rows="10" placeholder="Comment"></textarea>
</div>
```

### <a name="test-your-changes-locally"></a><span data-ttu-id="b7f1d-278">Testa ändringarna lokalt</span><span class="sxs-lookup"><span data-stu-id="b7f1d-278">Test your changes locally</span></span>

<span data-ttu-id="b7f1d-279">Spara alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-279">Save all your changes.</span></span>

<span data-ttu-id="b7f1d-280">Testa dina ändringar i Produktionsläge igen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-280">Test your changes in production mode again.</span></span>

```bash
gulp prod
NODE_ENV=production node server.js
```

> [!NOTE]
> <span data-ttu-id="b7f1d-281">Kom ihåg att din _config/env/production.js_ har återställts och `MONGODB_URI` miljövariabeln anges endast i ditt Azure webbapp och inte på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-281">Remember that your _config/env/production.js_ has been reverted, and the `MONGODB_URI` environment variable is only set in your Azure web app and not on your local machine.</span></span> <span data-ttu-id="b7f1d-282">Om du tittar på konfigurationsfilen upptäcker du att produktion konfigurationen standardvärden om du vill använda en lokal MongoDB-databas.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-282">If you look at the config file, you find that the production configuration defaults to use a local MongoDB database.</span></span> <span data-ttu-id="b7f1d-283">Detta säkerställer att du inte rör produktionsdata när du testar din kodändringar lokalt.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-283">This makes sure that you don't touch production data when you test your code changes locally.</span></span>

<span data-ttu-id="b7f1d-284">Navigera till `http://localhost:8443` i en webbläsare och se till att du har loggat in.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-284">Navigate to `http://localhost:8443` in a browser and make sure that you're signed in.</span></span>

<span data-ttu-id="b7f1d-285">Välj **Admin > Hantera artiklar**, lägga till en artikel genom att välja den  **+**  knappen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-285">Select **Admin > Manage Articles**, then add an article by selecting the **+** button.</span></span>

<span data-ttu-id="b7f1d-286">Du ser den nya `Comment` textruta nu.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-286">You see the new `Comment` textbox now.</span></span>

![Tillagda kommentar fältet till artiklar](./media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field.png)

<span data-ttu-id="b7f1d-288">Stoppa Node.js genom att skriva till terminalen och `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-288">In the terminal, stop Node.js by typing `Ctrl+C`.</span></span> 

### <a name="publish-changes-to-azure"></a><span data-ttu-id="b7f1d-289">Publicera ändringar i Azure</span><span class="sxs-lookup"><span data-stu-id="b7f1d-289">Publish changes to Azure</span></span>

<span data-ttu-id="b7f1d-290">Genomför ändringarna i Git och skicka koden ändringar till Azure.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-290">Commit your changes in Git, then push the code changes to Azure.</span></span>

```bash
git commit -am "added article comment"
git push azure master
```

<span data-ttu-id="b7f1d-291">En gång i `git push` är klar, gå till Azure webbapp och prova att använda de nya funktionerna.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-291">Once the `git push` is complete, navigate to your Azure web app and try out the new functionality.</span></span>

![Ändringar i modellen och databasen publiceras på Azure](media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field-published.png)

<span data-ttu-id="b7f1d-293">Om du lagt till alla artiklar tidigare kan kan du fortfarande se dem.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-293">If you added any articles earlier, you still can see them.</span></span> <span data-ttu-id="b7f1d-294">Befintliga data i Cosmos-databasen är inte förlorade.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-294">Existing data in your Cosmos DB is not lost.</span></span> <span data-ttu-id="b7f1d-295">Dessutom dataschemat uppdateringarna och lämnar företaget dina befintliga data.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-295">Also, your updates to the data schema and leaves your existing data intact.</span></span>

## <a name="stream-diagnostic-logs"></a><span data-ttu-id="b7f1d-296">Dataströmmen diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="b7f1d-296">Stream diagnostic logs</span></span> 

<span data-ttu-id="b7f1d-297">Du kan hämta loggarna för konsolen skickas till terminalen när Node.js-programmet körs i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-297">While your Node.js application runs in Azure App Service, you can get the console logs piped to your terminal.</span></span> <span data-ttu-id="b7f1d-298">På så sätt kan du få samma diagnostiska meddelanden för att felsöka programfel.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-298">That way, you can get the same diagnostic messages to help you debug application errors.</span></span>

<span data-ttu-id="b7f1d-299">Starta loggen strömning med den [az webapp loggen pilslut](/cli/azure/webapp/log#tail) kommando.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-299">To start log streaming, use the [az webapp log tail](/cli/azure/webapp/log#tail) command.</span></span>

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
``` 

<span data-ttu-id="b7f1d-300">Uppdatera din Azure-webbapp i webbläsaren att hämta vissa webbtrafik när loggen streaming har startats.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-300">Once log streaming has started, refresh your Azure web app in the browser to get some web traffic.</span></span> <span data-ttu-id="b7f1d-301">Nu kan du se loggarna för konsolen skickas till terminalen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-301">You now see console logs piped to your terminal.</span></span>

<span data-ttu-id="b7f1d-302">Stopploggen strömning när som helst genom att skriva `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-302">Stop log streaming at any time by typing `Ctrl+C`.</span></span> 

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="b7f1d-303">Hantera Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="b7f1d-303">Manage your Azure web app</span></span>

<span data-ttu-id="b7f1d-304">Gå till den [Azure-portalen](https://portal.azure.com) att se att webbappen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-304">Go to the [Azure portal](https://portal.azure.com) to see the web app you created.</span></span>

<span data-ttu-id="b7f1d-305">Klicka på **Apptjänster** på menyn till vänster och klicka sedan på namnet på din Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-305">From the left menu, click **App Services**, then click the name of your Azure web app.</span></span>

![Navigera till webbappen på Azure Portal](./media/app-service-web-tutorial-nodejs-mongodb-app/access-portal.png)

<span data-ttu-id="b7f1d-307">Som standard visas på portalen ditt webbprogram **översikt** sidan.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-307">By default, the portal shows your web app's **Overview** page.</span></span> <span data-ttu-id="b7f1d-308">På den här sidan får du en översikt över hur det går för appen.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-308">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="b7f1d-309">Här kan du också utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-309">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="b7f1d-310">Flikar till vänster på sidan Visa sidorna annan konfiguration som du kan öppna.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-310">The tabs on the left side of the page show the different configuration pages you can open.</span></span>

![App Service-sidan på Azure Portal](./media/app-service-web-tutorial-nodejs-mongodb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a><span data-ttu-id="b7f1d-312">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7f1d-312">Next steps</span></span>

<span data-ttu-id="b7f1d-313">Vad du lärt dig:</span><span class="sxs-lookup"><span data-stu-id="b7f1d-313">What you learned:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b7f1d-314">Skapa en MongoDB-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="b7f1d-314">Create a MongoDB database in Azure</span></span>
> * <span data-ttu-id="b7f1d-315">Ansluta en Node.js-app till MongoDB</span><span class="sxs-lookup"><span data-stu-id="b7f1d-315">Connect a Node.js app to MongoDB</span></span>
> * <span data-ttu-id="b7f1d-316">Distribuera appen till Azure</span><span class="sxs-lookup"><span data-stu-id="b7f1d-316">Deploy the app to Azure</span></span>
> * <span data-ttu-id="b7f1d-317">Uppdatera datamodellen och distribuera appen</span><span class="sxs-lookup"><span data-stu-id="b7f1d-317">Update the data model and redeploy the app</span></span>
> * <span data-ttu-id="b7f1d-318">Dataströmmen loggas från Azure till terminalen</span><span class="sxs-lookup"><span data-stu-id="b7f1d-318">Stream logs from Azure to your terminal</span></span>
> * <span data-ttu-id="b7f1d-319">Hantera appen i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b7f1d-319">Manage the app in the Azure portal</span></span>

<span data-ttu-id="b7f1d-320">Gå vidare till nästa kurs att lära dig hur du mappar en anpassad DNS-namn till ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b7f1d-320">Advance to the next tutorial to learn how to map a custom DNS name to your web app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="b7f1d-321">Mappa ett befintligt anpassat DNS-namn till Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="b7f1d-321">Map an existing custom DNS name to Azure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
