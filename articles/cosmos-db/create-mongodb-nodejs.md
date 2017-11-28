---
title: "Anslut en MongoDB-app till Azure Cosmos DB med hjälp av Node.js | Microsoft Docs"
description: "Läs hur du ansluter en befintlig Node.js MongoDB app till Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 06/19/2017
ms.author: mimig
ms.openlocfilehash: a26477d692cc98ed16c195233ade5434cc536a36
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-migrate-an-existing-nodejs-mongodb-web-app"></a><span data-ttu-id="31dfa-103">Azure Cosmos DB: Migrera en befintlig Node.js MongoDB-webbapp</span><span class="sxs-lookup"><span data-stu-id="31dfa-103">Azure Cosmos DB: Migrate an existing Node.js MongoDB web app</span></span> 

<span data-ttu-id="31dfa-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="31dfa-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="31dfa-105">Du kan snabbt skapa och ställa frågor mot databaser med dokument, nyckel/värde-par och grafer. Du får fördelar av den globala distributionen och den horisontella skalningsförmågan som ligger i grunden hos Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="31dfa-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="31dfa-106">Den här snabbstarten visar hur du använder en befintlig [MongoDB](mongodb-introduction.md)-app som skrivits i Node.js och ansluter den till din Azure Cosmos DB-databas, som stöder MongoDB-klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="31dfa-106">This quickstart demonstrates how to use an existing [MongoDB](mongodb-introduction.md) app written in Node.js and connect it to your Azure Cosmos DB database, which supports MongoDB client connections.</span></span> <span data-ttu-id="31dfa-107">Med andra ord vet ditt Node.js program bara att det ansluter till en databas som använder MongoDB-API:er.</span><span class="sxs-lookup"><span data-stu-id="31dfa-107">In other words, your Node.js application only knows that it's connecting to a database using MongoDB APIs.</span></span> <span data-ttu-id="31dfa-108">Det är transparent för programmet att data lagras i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="31dfa-108">It is transparent to the application that the data is stored in Azure Cosmos DB.</span></span>

<span data-ttu-id="31dfa-109">När du är klar har du ett MEAN-program (MongoDB, Express, AngularJS och Node.js) som kör på [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="31dfa-109">When you are done, you will have a MEAN application (MongoDB, Express, AngularJS, and Node.js) running on [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span> 

![MEAN.js-app som körs i Azure App Service](./media/create-mongodb-nodejs/meanjs-in-azure.png)


[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="31dfa-111">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="31dfa-111">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="31dfa-112">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="31dfa-112">Run `az --version` to find the version.</span></span> <span data-ttu-id="31dfa-113">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="31dfa-113">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="31dfa-114">Krav</span><span class="sxs-lookup"><span data-stu-id="31dfa-114">Prerequisites</span></span> 
<span data-ttu-id="31dfa-115">Utöver Azure CLI, behöver du [Node.js](https://nodejs.org/) och [Git](http://www.git-scm.com/downloads) installerat lokalt om du vill köra `npm` och `git` kommandon.</span><span class="sxs-lookup"><span data-stu-id="31dfa-115">In addition to Azure CLI, you need [Node.js](https://nodejs.org/) and [Git](http://www.git-scm.com/downloads) installed locally to run `npm` and `git` commands.</span></span>

<span data-ttu-id="31dfa-116">Du bör ha kunskaper inom Node.js.</span><span class="sxs-lookup"><span data-stu-id="31dfa-116">You should have working knowledge of Node.js.</span></span> <span data-ttu-id="31dfa-117">Den här snabbstarten är inte avsedd att hjälpa dig med att utveckla Node.js-program i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="31dfa-117">This quickstart is not intended to help you with developing Node.js applications in general.</span></span>

## <a name="clone-the-sample-application"></a><span data-ttu-id="31dfa-118">Klona exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="31dfa-118">Clone the sample application</span></span>

<span data-ttu-id="31dfa-119">Öppna ett git-terminalfönster, till exempel git bash och `cd` till en arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="31dfa-119">Open a git terminal window, such as git bash, and `cd` to a working directory.</span></span>  

<span data-ttu-id="31dfa-120">Kör följande kommandon för att klona exempellagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="31dfa-120">Run the following commands to clone the sample repository.</span></span> <span data-ttu-id="31dfa-121">Den här exempellagringsplatsen innehåller standard-[MEAN.js](http://meanjs.org/)-programmet.</span><span class="sxs-lookup"><span data-stu-id="31dfa-121">This sample repository contains the default [MEAN.js](http://meanjs.org/) application.</span></span> 

```bash
git clone https://github.com/prashanthmadi/mean
```

## <a name="run-the-application"></a><span data-ttu-id="31dfa-122">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="31dfa-122">Run the application</span></span>

<span data-ttu-id="31dfa-123">Installera de nödvändiga paketen och starta programmet.</span><span class="sxs-lookup"><span data-stu-id="31dfa-123">Install the required packages and start the application.</span></span>

```bash
cd mean
npm install
npm start
```

## <a name="log-in-to-azure"></a><span data-ttu-id="31dfa-124">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="31dfa-124">Log in to Azure</span></span>

<span data-ttu-id="31dfa-125">Om du använder en installerad Azure CLI loggar du in på Azure-prenumerationen med kommandot [az login](/cli/azure/#login) och följer anvisningarna på skärmen.</span><span class="sxs-lookup"><span data-stu-id="31dfa-125">If you are using an installed Azure CLI, log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span> <span data-ttu-id="31dfa-126">Du kan hoppa över det här steget om du använder Azure Cloud-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="31dfa-126">You can skip this step if you're using the Azure Cloud Shell.</span></span>

```azurecli
az login 
``` 
   
## <a name="add-the-azure-cosmos-db-module"></a><span data-ttu-id="31dfa-127">Lägg till Azure Cosmos DB-modulen</span><span class="sxs-lookup"><span data-stu-id="31dfa-127">Add the Azure Cosmos DB module</span></span>

<span data-ttu-id="31dfa-128">Om du använder en installerad Azure CLI, kontrollera om `cosmosdb`-komponenten har installerats genom att köra kommandot `az`.</span><span class="sxs-lookup"><span data-stu-id="31dfa-128">If you are using an installed Azure CLI, check to see if the `cosmosdb` component is already installed by running the `az` command.</span></span> <span data-ttu-id="31dfa-129">Om `cosmosdb` är i listan över grundläggande kommandon, fortsätter du med nästa kommando.</span><span class="sxs-lookup"><span data-stu-id="31dfa-129">If `cosmosdb` is in the list of base commands, proceed to the next command.</span></span> <span data-ttu-id="31dfa-130">Du kan hoppa över det här steget om du använder Azure Cloud-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="31dfa-130">You can skip this step if you're using the Azure Cloud Shell.</span></span>

<span data-ttu-id="31dfa-131">Om `cosmosdb` inte är i listan över grundläggande kommandon, installerar du om [Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="31dfa-131">If `cosmosdb` is not in the list of base commands, reinstall [Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="31dfa-132">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="31dfa-132">Create a resource group</span></span>

<span data-ttu-id="31dfa-133">Skapa en [resursgrupp](../azure-resource-manager/resource-group-overview.md) med [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="31dfa-133">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with the [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="31dfa-134">En Azure-resursgrupp är en logisk behållare som Azure-resurser (t.ex. webbappar, databaser och lagringskonton) distribueras och hanteras i.</span><span class="sxs-lookup"><span data-stu-id="31dfa-134">An Azure resource group is a logical container into which Azure resources like web apps, databases and storage accounts are deployed and managed.</span></span> 

<span data-ttu-id="31dfa-135">Följande exempel skapar en resursgrupp i regionen västeuropa.</span><span class="sxs-lookup"><span data-stu-id="31dfa-135">The following example creates a resource group in the West Europe region.</span></span> <span data-ttu-id="31dfa-136">Välj ett unikt namn för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="31dfa-136">Choose a unique name for the resource group.</span></span>

<span data-ttu-id="31dfa-137">Om du använder Azure Cloud Shell, klickar du på **Prova**, följer anvisningarna på skärmen för att logga in och kopierar sedan kommandot i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="31dfa-137">If you are using Azure Cloud Shell, click **Try It**, follow the onscreen prompts to login, then copy the command into the command prompt.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="31dfa-138">Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="31dfa-138">Create an Azure Cosmos DB account</span></span>

<span data-ttu-id="31dfa-139">Skapa ett Azure Cosmos DB-konto med kommandot [az cosmosdb create](/cli/azure/cosmosdb#create).</span><span class="sxs-lookup"><span data-stu-id="31dfa-139">Create an Azure Cosmos DB account with the [az cosmosdb create](/cli/azure/cosmosdb#create) command.</span></span>

<span data-ttu-id="31dfa-140">I följande kommando, ersätter du ditt eget unika Azure Cosmos DB-kontonamn där du ser platshållaren `<cosmosdb-name>`.</span><span class="sxs-lookup"><span data-stu-id="31dfa-140">In the following command, please substitute your own unique Azure Cosmos DB account name where you see the `<cosmosdb-name>` placeholder.</span></span> <span data-ttu-id="31dfa-141">Den här unika namnet kommer att användas som en del av din Azure Cosmos DB-slutpunkt (`https://<cosmosdb-name>.documents.azure.com/`) så namnet måste vara unikt för alla Azure Cosmos DB-konton i Azure.</span><span class="sxs-lookup"><span data-stu-id="31dfa-141">This unique name will be used as part of your Azure Cosmos DB endpoint (`https://<cosmosdb-name>.documents.azure.com/`), so the name needs to be unique across all Azure Cosmos DB accounts in Azure.</span></span> 

```azurecli-interactive
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

<span data-ttu-id="31dfa-142">Parametern `--kind MongoDB` aktiverar MongoDB-klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="31dfa-142">The `--kind MongoDB` parameter enables MongoDB client connections.</span></span>

<span data-ttu-id="31dfa-143">När Azure Cosmos DB-kontot har skapats, visar Azure CLI information liknande följande exempel.</span><span class="sxs-lookup"><span data-stu-id="31dfa-143">When the Azure Cosmos DB account is created, the Azure CLI shows information similar to the following example.</span></span> 

> [!NOTE]
> <span data-ttu-id="31dfa-144">I det här exemplet används JSON som standardalternativ för Azure CLI-utdataformat.</span><span class="sxs-lookup"><span data-stu-id="31dfa-144">This example uses JSON as the Azure CLI output format, which is the default.</span></span> <span data-ttu-id="31dfa-145">Om du vill använda andra format läser du [Utdataformat för Azure CLI 2.0-kommandon](https://docs.microsoft.com/cli/azure/format-output-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="31dfa-145">To use another output format, see [Output formats for Azure CLI 2.0 commands](https://docs.microsoft.com/cli/azure/format-output-azure-cli).</span></span>

```json
{
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb-name>.documents.azure.com:443/",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Document
DB/databaseAccounts/<cosmosdb-name>",
  "kind": "MongoDB",
  "location": "West Europe",
  "name": "<cosmosdb-name>",
  "readLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ],
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "writeLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ]
} 
```

## <a name="connect-your-nodejs-application-to-the-database"></a><span data-ttu-id="31dfa-146">Anslut ditt Node.js-program till databasen</span><span class="sxs-lookup"><span data-stu-id="31dfa-146">Connect your Node.js application to the database</span></span>

<span data-ttu-id="31dfa-147">I det här steget, ansluter du ditt MEAN.js-exempelprogram till en Azure Cosmos DB-databas som du just skapade med en MongoDB-anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="31dfa-147">In this step, you connect your MEAN.js sample application to an Azure Cosmos DB database you just created, using a MongoDB connection string.</span></span> 

<a name="devconfig"></a>
## <a name="configure-the-connection-string-in-your-nodejs-application"></a><span data-ttu-id="31dfa-148">Konfigurera anslutningssträngen i ditt Node.js-program</span><span class="sxs-lookup"><span data-stu-id="31dfa-148">Configure the connection string in your Node.js application</span></span>

<span data-ttu-id="31dfa-149">I din MEAN.js-lagringsplats, öppnar du `config/env/local-development.js`.</span><span class="sxs-lookup"><span data-stu-id="31dfa-149">In your MEAN.js repository, open `config/env/local-development.js`.</span></span>

<span data-ttu-id="31dfa-150">Ersätt innehållet i filen med följande kod.</span><span class="sxs-lookup"><span data-stu-id="31dfa-150">Replace the content of this file with the following code.</span></span> <span data-ttu-id="31dfa-151">Se till att även ersätta två `<cosmosdb-name>` platshållare med namnet på ditt Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="31dfa-151">Be sure to also replace the two `<cosmosdb-name>` placeholders with your Azure Cosmos DB account name.</span></span>

```javascript
'use strict';

module.exports = {
  db: {
    uri: 'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean-dev?ssl=true&sslverifycertificate=false'
  }
};
```

## <a name="retrieve-the-key"></a><span data-ttu-id="31dfa-152">Hämta nyckeln</span><span class="sxs-lookup"><span data-stu-id="31dfa-152">Retrieve the key</span></span>

<span data-ttu-id="31dfa-153">För att kunna ansluta till en Azure Cosmos DB-databas, behöver du databasnyckeln.</span><span class="sxs-lookup"><span data-stu-id="31dfa-153">In order to connect to an Azure Cosmos DB database, you need the database key.</span></span> <span data-ttu-id="31dfa-154">Använd kommandot [az documentdb list-keys](/cli/azure/cosmosdb#list-keys) för att hämta den primära nyckeln.</span><span class="sxs-lookup"><span data-stu-id="31dfa-154">Use the [az cosmosdb list-keys](/cli/azure/cosmosdb#list-keys) command to retrieve the primary key.</span></span>

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb-name> --resource-group myResourceGroup --query "primaryMasterKey"
```

<span data-ttu-id="31dfa-155">Azure CLI matar ut information som liknar följande exempel.</span><span class="sxs-lookup"><span data-stu-id="31dfa-155">The Azure CLI outputs information similar to the following example.</span></span> 

```json
"RUayjYjixJDWG5xTqIiXjC..."
```

<span data-ttu-id="31dfa-156">Kopiera värdet för `primaryMasterKey`.</span><span class="sxs-lookup"><span data-stu-id="31dfa-156">Copy the value of `primaryMasterKey`.</span></span> <span data-ttu-id="31dfa-157">Klistra in det över `<primary_master_key>` i `local-development.js`.</span><span class="sxs-lookup"><span data-stu-id="31dfa-157">Paste this over the  `<primary_master_key>` in `local-development.js`.</span></span>

<span data-ttu-id="31dfa-158">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="31dfa-158">Save your changes.</span></span>

### <a name="run-the-application-again"></a><span data-ttu-id="31dfa-159">Kör programmet igen.</span><span class="sxs-lookup"><span data-stu-id="31dfa-159">Run the application again.</span></span>

<span data-ttu-id="31dfa-160">Kör `npm start` igen.</span><span class="sxs-lookup"><span data-stu-id="31dfa-160">Run `npm start` again.</span></span> 

```bash
npm start
```

<span data-ttu-id="31dfa-161">Ett konsolmeddelande borde nu komma upp som säger att utvecklingsmiljön är klar och igång.</span><span class="sxs-lookup"><span data-stu-id="31dfa-161">A console message should now tell you that the development environment is up and running.</span></span> 

<span data-ttu-id="31dfa-162">Gå till `http://localhost:3000` i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="31dfa-162">Navigate to `http://localhost:3000` in a browser.</span></span> <span data-ttu-id="31dfa-163">Klicka på **registrera dig** i huvudmenyn och försök skapa två låtsasanvändare.</span><span class="sxs-lookup"><span data-stu-id="31dfa-163">Click **Sign Up** in the top menu and try to create two dummy users.</span></span> 

<span data-ttu-id="31dfa-164">MEAN.js-exempelprogrammet lagrar användardata i databasen.</span><span class="sxs-lookup"><span data-stu-id="31dfa-164">The MEAN.js sample application stores user data in the database.</span></span> <span data-ttu-id="31dfa-165">Om du har lyckats och MEAN.js automatiskt loggar in på den skapade användaren så fungerar din Azure Cosmos DB-anslutning.</span><span class="sxs-lookup"><span data-stu-id="31dfa-165">If you are successful and MEAN.js automatically signs into the created user, then your Azure Cosmos DB connection is working.</span></span> 

![MEAN.js ansluter till MongoDB](./media/create-mongodb-nodejs/mongodb-connect-success.png)

## <a name="view-data-in-data-explorer"></a><span data-ttu-id="31dfa-167">Visa data i datautforskaren</span><span class="sxs-lookup"><span data-stu-id="31dfa-167">View data in Data Explorer</span></span>

<span data-ttu-id="31dfa-168">Data som lagras av en Azure Cosmos DB finns tillgänglig för att se, fråga och köra affärslogik på i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="31dfa-168">Data stored by an Azure Cosmos DB is available to view, query, and run business-logic on in the Azure portal.</span></span>

<span data-ttu-id="31dfa-169">Om du vill visa, fråga och arbeta med användardata som skapats i föregående steg, loggar du in på [Azure-portalen](https://portal.azure.com) i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="31dfa-169">To view, query, and work with the user data created in the previous step, login to the [Azure portal](https://portal.azure.com) in your web browser.</span></span>

<span data-ttu-id="31dfa-170">I den övre sökrutan skriver du in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="31dfa-170">In the top Search box, type Azure Cosmos DB.</span></span> <span data-ttu-id="31dfa-171">När ditt Cosmos DB-kontoblad öppnas, väljer du ditt Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="31dfa-171">When your Cosmos DB account blade opens, select your Cosmos DB account.</span></span> <span data-ttu-id="31dfa-172">I det vänstra navigeringsfönstret, klickar du på datautforskaren.</span><span class="sxs-lookup"><span data-stu-id="31dfa-172">In the left navigation, click Data Explorer.</span></span> <span data-ttu-id="31dfa-173">Utöka din samling i samlings-fönstret så kan du visa dokumenten i samlingen, fråga data och skapa och köra lagrade procedurer, utlösare och UDF:er.</span><span class="sxs-lookup"><span data-stu-id="31dfa-173">Expand your collection in the Collections pane, and then you can view the documents in the collection, query the data, and even create and run stored procedures, triggers, and UDFs.</span></span> 

![Datautforskaren i Azure-portalen](./media/create-mongodb-nodejs/cosmosdb-connect-mongodb-data-explorer.png)


## <a name="deploy-the-nodejs-application-to-azure"></a><span data-ttu-id="31dfa-175">Distribuera Node.js-programmet till Azure</span><span class="sxs-lookup"><span data-stu-id="31dfa-175">Deploy the Node.js application to Azure</span></span>

<span data-ttu-id="31dfa-176">I det här steget, distribuerar du ditt MongoDB-anslutna Node.js-program till Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="31dfa-176">In this step, you deploy your MongoDB-connected Node.js application to Azure Cosmos DB.</span></span>

<span data-ttu-id="31dfa-177">Du kanske har märkt att konfigurationsfilen du ändrade tidigare är för utvecklingsmiljön (`/config/env/local-development.js`).</span><span class="sxs-lookup"><span data-stu-id="31dfa-177">You may have noticed that the configuration file that you changed earlier is for the development environment (`/config/env/local-development.js`).</span></span> <span data-ttu-id="31dfa-178">När du distribuerar ditt program till App Service, körs det i produktionsmiljön som standard.</span><span class="sxs-lookup"><span data-stu-id="31dfa-178">When you deploy your application to App Service, it will run in the production environment by default.</span></span> <span data-ttu-id="31dfa-179">Nu behöver du göra samma ändring till respektive konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="31dfa-179">So now, you need to make the same change to the respective configuration file.</span></span>

<span data-ttu-id="31dfa-180">I din MEAN.js-lagringsplats, öppnar du `config/env/production.js`.</span><span class="sxs-lookup"><span data-stu-id="31dfa-180">In your MEAN.js repository, open `config/env/production.js`.</span></span>

<span data-ttu-id="31dfa-181">I objektet `db`, ersätter du värdet `uri` som det visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="31dfa-181">In the `db` object, replace the value of `uri` as show in the following example.</span></span> <span data-ttu-id="31dfa-182">Se till att ersätta platshållarna som innan.</span><span class="sxs-lookup"><span data-stu-id="31dfa-182">Be sure to replace the placeholders as before.</span></span>

```javascript
'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean?ssl=true&sslverifycertificate=false',
```

> [!NOTE] 
> <span data-ttu-id="31dfa-183">Alternativet `ssl=true` är viktigt eftersom [Azure Cosmos DB kräver SSL](connect-mongodb-account.md#connection-string-requirements).</span><span class="sxs-lookup"><span data-stu-id="31dfa-183">The `ssl=true` option is important because [Azure Cosmos DB requires SSL](connect-mongodb-account.md#connection-string-requirements).</span></span> 
>
>

<span data-ttu-id="31dfa-184">Spara dina ändringar till Git i terminalen.</span><span class="sxs-lookup"><span data-stu-id="31dfa-184">In the terminal, commit all your changes into Git.</span></span> <span data-ttu-id="31dfa-185">Du kan kopiera bägge kommandona för att köra dem tillsammans.</span><span class="sxs-lookup"><span data-stu-id="31dfa-185">You can copy both commands to run them together.</span></span>

```bash
git add .
git commit -m "configured MongoDB connection string"
```
## <a name="clean-up-resources"></a><span data-ttu-id="31dfa-186">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="31dfa-186">Clean up resources</span></span>

<span data-ttu-id="31dfa-187">Om du inte planerar att fortsätta använda den här appen tar du bort alla resurser som skapades i snabbstarten i Azure Portal med följande steg:</span><span class="sxs-lookup"><span data-stu-id="31dfa-187">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="31dfa-188">Klicka på **Resursgrupper** på den vänstra menyn i Azure Portal och sedan på namnet på den resurs du skapade.</span><span class="sxs-lookup"><span data-stu-id="31dfa-188">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="31dfa-189">På sidan med resursgrupper klickar du på **Ta bort**, skriver in namnet på resursen att ta bort i textrutan och klickar sedan på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="31dfa-189">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="31dfa-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="31dfa-190">Next steps</span></span>

<span data-ttu-id="31dfa-191">I den här snabbstarten har du lärt dig hur man skapar ett Azure Cosmos DB-konto och skapar en MongoDB-samling med datautforskaren.</span><span class="sxs-lookup"><span data-stu-id="31dfa-191">In this quickstart, you've learned how to create an Azure Cosmos DB account and create a MongoDB collection using the Data Explorer.</span></span> <span data-ttu-id="31dfa-192">Nu kan du migrera dina MongoDB-data till Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="31dfa-192">You can now migrate your MongoDB data to Azure Cosmos DB.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="31dfa-193">Importera MongoDB-data till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="31dfa-193">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)
