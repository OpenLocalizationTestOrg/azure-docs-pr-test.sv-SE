---
title: aaaConnect en MongoDB app tooAzure Cosmos DB med Node.js | Microsoft Docs
description: "Lär dig hur tooconnect en befintlig Node.js MongoDB app tooAzure Cosmos DB"
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
ms.openlocfilehash: 4bc4f17a31d8c18d1ce5e3f002462f4d48eeb1f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-migrate-an-existing-nodejs-mongodb-web-app"></a><span data-ttu-id="a0020-103">Azure Cosmos DB: Migrera en befintlig Node.js MongoDB-webbapp</span><span class="sxs-lookup"><span data-stu-id="a0020-103">Azure Cosmos DB: Migrate an existing Node.js MongoDB web app</span></span> 

<span data-ttu-id="a0020-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="a0020-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="a0020-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a0020-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="a0020-106">Den här snabbstarten visar hur en befintlig toouse [MongoDB](mongodb-introduction.md) app skriven i Node.js och anslut den tooyour Azure DB som Cosmos-databasen, som har stöd för MongoDB-klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="a0020-106">This quickstart demonstrates how toouse an existing [MongoDB](mongodb-introduction.md) app written in Node.js and connect it tooyour Azure Cosmos DB database, which supports MongoDB client connections.</span></span> <span data-ttu-id="a0020-107">Med andra ord, vet Node.js-programmet bara att den ansluter tooa databasen med MongoDB APIs.</span><span class="sxs-lookup"><span data-stu-id="a0020-107">In other words, your Node.js application only knows that it's connecting tooa database using MongoDB APIs.</span></span> <span data-ttu-id="a0020-108">Den är öppet toohello program som hello data lagras i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a0020-108">It is transparent toohello application that hello data is stored in Azure Cosmos DB.</span></span>

<span data-ttu-id="a0020-109">När du är klar har du ett MEAN-program (MongoDB, Express, AngularJS och Node.js) som kör på [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="a0020-109">When you are done, you will have a MEAN application (MongoDB, Express, AngularJS, and Node.js) running on [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span> 

![MEAN.js-app som körs i Azure App Service](./media/create-mongodb-nodejs/meanjs-in-azure.png)


[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a0020-111">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a0020-111">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="a0020-112">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="a0020-112">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="a0020-113">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a0020-113">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a0020-114">Krav</span><span class="sxs-lookup"><span data-stu-id="a0020-114">Prerequisites</span></span> 
<span data-ttu-id="a0020-115">Dessutom tooAzure CLI, behöver du [Node.js](https://nodejs.org/) och [Git](http://www.git-scm.com/downloads) installeras lokalt toorun `npm` och `git` kommandon.</span><span class="sxs-lookup"><span data-stu-id="a0020-115">In addition tooAzure CLI, you need [Node.js](https://nodejs.org/) and [Git](http://www.git-scm.com/downloads) installed locally toorun `npm` and `git` commands.</span></span>

<span data-ttu-id="a0020-116">Du bör ha kunskaper inom Node.js.</span><span class="sxs-lookup"><span data-stu-id="a0020-116">You should have working knowledge of Node.js.</span></span> <span data-ttu-id="a0020-117">Denna Snabbstart är inte avsedda toohelp du utvecklar Node.js-program i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="a0020-117">This quickstart is not intended toohelp you with developing Node.js applications in general.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="a0020-118">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="a0020-118">Clone hello sample application</span></span>

<span data-ttu-id="a0020-119">Öppna ett git terminalfönster, till exempel git bash och `cd` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="a0020-119">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

<span data-ttu-id="a0020-120">Kör hello följande kommandon tooclone hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="a0020-120">Run hello following commands tooclone hello sample repository.</span></span> <span data-ttu-id="a0020-121">Det här exemplet lagringsplatsen innehåller hello standard [MEAN.js](http://meanjs.org/) program.</span><span class="sxs-lookup"><span data-stu-id="a0020-121">This sample repository contains hello default [MEAN.js](http://meanjs.org/) application.</span></span> 

```bash
git clone https://github.com/prashanthmadi/mean
```

## <a name="run-hello-application"></a><span data-ttu-id="a0020-122">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="a0020-122">Run hello application</span></span>

<span data-ttu-id="a0020-123">Installera hello krävs paket och starta programmet hello.</span><span class="sxs-lookup"><span data-stu-id="a0020-123">Install hello required packages and start hello application.</span></span>

```bash
cd mean
npm install
npm start
```

## <a name="log-in-tooazure"></a><span data-ttu-id="a0020-124">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="a0020-124">Log in tooAzure</span></span>

<span data-ttu-id="a0020-125">Om du använder en installerade Azure CLI, logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="a0020-125">If you are using an installed Azure CLI, log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> <span data-ttu-id="a0020-126">Du kan hoppa över det här steget om du använder hello Azure Cloud-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a0020-126">You can skip this step if you're using hello Azure Cloud Shell.</span></span>

```azurecli
az login 
``` 
   
## <a name="add-hello-azure-cosmos-db-module"></a><span data-ttu-id="a0020-127">Lägg till hello Azure DB som Cosmos-modul</span><span class="sxs-lookup"><span data-stu-id="a0020-127">Add hello Azure Cosmos DB module</span></span>

<span data-ttu-id="a0020-128">Om du använder en installerade Azure CLI, kontrollera toosee om hello `cosmosdb` komponent har installerats genom att köra hello `az` kommando.</span><span class="sxs-lookup"><span data-stu-id="a0020-128">If you are using an installed Azure CLI, check toosee if hello `cosmosdb` component is already installed by running hello `az` command.</span></span> <span data-ttu-id="a0020-129">Om `cosmosdb` är i Hej lista över grundläggande kommandon, fortsätta toohello nästa kommando.</span><span class="sxs-lookup"><span data-stu-id="a0020-129">If `cosmosdb` is in hello list of base commands, proceed toohello next command.</span></span> <span data-ttu-id="a0020-130">Du kan hoppa över det här steget om du använder hello Azure Cloud-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a0020-130">You can skip this step if you're using hello Azure Cloud Shell.</span></span>

<span data-ttu-id="a0020-131">Om `cosmosdb` är inte i hello lista över grundläggande kommandon, installera om [Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a0020-131">If `cosmosdb` is not in hello list of base commands, reinstall [Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="a0020-132">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a0020-132">Create a resource group</span></span>

<span data-ttu-id="a0020-133">Skapa en [resursgruppen](../azure-resource-manager/resource-group-overview.md) med hello [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="a0020-133">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with hello [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="a0020-134">En Azure-resursgrupp är en logisk behållare som Azure-resurser (t.ex. webbappar, databaser och lagringskonton) distribueras och hanteras i.</span><span class="sxs-lookup"><span data-stu-id="a0020-134">An Azure resource group is a logical container into which Azure resources like web apps, databases and storage accounts are deployed and managed.</span></span> 

<span data-ttu-id="a0020-135">hello följande exempel skapas en resursgrupp i hello Västeuropa region.</span><span class="sxs-lookup"><span data-stu-id="a0020-135">hello following example creates a resource group in hello West Europe region.</span></span> <span data-ttu-id="a0020-136">Välj ett unikt namn för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a0020-136">Choose a unique name for hello resource group.</span></span>

<span data-ttu-id="a0020-137">Om du använder Azure Cloud Shell, klickar du på **prova**, följ hello anvisningarna toologin och sedan kopiera hello-kommando i Kommandotolken för hello.</span><span class="sxs-lookup"><span data-stu-id="a0020-137">If you are using Azure Cloud Shell, click **Try It**, follow hello onscreen prompts toologin, then copy hello command into hello command prompt.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="a0020-138">Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="a0020-138">Create an Azure Cosmos DB account</span></span>

<span data-ttu-id="a0020-139">Skapa ett Azure DB som Cosmos-konto med hello [az cosmosdb skapa](/cli/azure/cosmosdb#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="a0020-139">Create an Azure Cosmos DB account with hello [az cosmosdb create](/cli/azure/cosmosdb#create) command.</span></span>

<span data-ttu-id="a0020-140">I hello du följande kommando, ersätta dina egna unika Azure DB som Cosmos-kontonamnet där du ser hello `<cosmosdb-name>` platshållare.</span><span class="sxs-lookup"><span data-stu-id="a0020-140">In hello following command, please substitute your own unique Azure Cosmos DB account name where you see hello `<cosmosdb-name>` placeholder.</span></span> <span data-ttu-id="a0020-141">Detta unika namn som ska användas som en del av din Azure DB som Cosmos-slutpunkt (`https://<cosmosdb-name>.documents.azure.com/`), så hello namn måste toobe unikt över alla Azure DB som Cosmos-konton i Azure.</span><span class="sxs-lookup"><span data-stu-id="a0020-141">This unique name will be used as part of your Azure Cosmos DB endpoint (`https://<cosmosdb-name>.documents.azure.com/`), so hello name needs toobe unique across all Azure Cosmos DB accounts in Azure.</span></span> 

```azurecli-interactive
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

<span data-ttu-id="a0020-142">Hej `--kind MongoDB` parametern aktiverar MongoDB-klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="a0020-142">hello `--kind MongoDB` parameter enables MongoDB client connections.</span></span>

<span data-ttu-id="a0020-143">När hello Azure Cosmos DB konto skapas visar hello Azure CLI information liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="a0020-143">When hello Azure Cosmos DB account is created, hello Azure CLI shows information similar toohello following example.</span></span> 

> [!NOTE]
> <span data-ttu-id="a0020-144">Det här exemplet använder JSON som hello Azure CLI utdataformat, vilket är hello som standard.</span><span class="sxs-lookup"><span data-stu-id="a0020-144">This example uses JSON as hello Azure CLI output format, which is hello default.</span></span> <span data-ttu-id="a0020-145">toouse en annan utdata format, se [utdataformat för Azure CLI 2.0 kommandon](https://docs.microsoft.com/cli/azure/format-output-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a0020-145">toouse another output format, see [Output formats for Azure CLI 2.0 commands](https://docs.microsoft.com/cli/azure/format-output-azure-cli).</span></span>

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

## <a name="connect-your-nodejs-application-toohello-database"></a><span data-ttu-id="a0020-146">Ansluta toohello för Node.js programdatabasen</span><span class="sxs-lookup"><span data-stu-id="a0020-146">Connect your Node.js application toohello database</span></span>

<span data-ttu-id="a0020-147">I det här steget kan ansluta du din MEAN.js programmet tooan Azure Cosmos DB exempeldatabasen du precis skapade, med hjälp av en anslutningssträng för MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a0020-147">In this step, you connect your MEAN.js sample application tooan Azure Cosmos DB database you just created, using a MongoDB connection string.</span></span> 

<a name="devconfig"></a>
## <a name="configure-hello-connection-string-in-your-nodejs-application"></a><span data-ttu-id="a0020-148">Konfigurera hello anslutningssträngen i Node.js-programmet</span><span class="sxs-lookup"><span data-stu-id="a0020-148">Configure hello connection string in your Node.js application</span></span>

<span data-ttu-id="a0020-149">I din MEAN.js-lagringsplats, öppnar du `config/env/local-development.js`.</span><span class="sxs-lookup"><span data-stu-id="a0020-149">In your MEAN.js repository, open `config/env/local-development.js`.</span></span>

<span data-ttu-id="a0020-150">Ersätt hello innehållet i den här filen med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="a0020-150">Replace hello content of this file with hello following code.</span></span> <span data-ttu-id="a0020-151">Glöm inte tooalso ersätta hello två `<cosmosdb-name>` platshållarna med namnet på ditt Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a0020-151">Be sure tooalso replace hello two `<cosmosdb-name>` placeholders with your Azure Cosmos DB account name.</span></span>

```javascript
'use strict';

module.exports = {
  db: {
    uri: 'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean-dev?ssl=true&sslverifycertificate=false'
  }
};
```

## <a name="retrieve-hello-key"></a><span data-ttu-id="a0020-152">Hämta hello nyckel</span><span class="sxs-lookup"><span data-stu-id="a0020-152">Retrieve hello key</span></span>

<span data-ttu-id="a0020-153">I ordning tooconnect tooan Azure DB som Cosmos-databasen behöver du hello databasnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a0020-153">In order tooconnect tooan Azure Cosmos DB database, you need hello database key.</span></span> <span data-ttu-id="a0020-154">Använd hello [az cosmosdb lista nycklar](/cli/azure/cosmosdb#list-keys) kommandot tooretrieve hello primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="a0020-154">Use hello [az cosmosdb list-keys](/cli/azure/cosmosdb#list-keys) command tooretrieve hello primary key.</span></span>

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb-name> --resource-group myResourceGroup --query "primaryMasterKey"
```

<span data-ttu-id="a0020-155">hello Azure CLI matar ut information liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="a0020-155">hello Azure CLI outputs information similar toohello following example.</span></span> 

```json
"RUayjYjixJDWG5xTqIiXjC..."
```

<span data-ttu-id="a0020-156">Kopiera hello värdet för `primaryMasterKey`.</span><span class="sxs-lookup"><span data-stu-id="a0020-156">Copy hello value of `primaryMasterKey`.</span></span> <span data-ttu-id="a0020-157">Klistra in det över hello `<primary_master_key>` i `local-development.js`.</span><span class="sxs-lookup"><span data-stu-id="a0020-157">Paste this over hello  `<primary_master_key>` in `local-development.js`.</span></span>

<span data-ttu-id="a0020-158">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="a0020-158">Save your changes.</span></span>

### <a name="run-hello-application-again"></a><span data-ttu-id="a0020-159">Kör hello programmet igen.</span><span class="sxs-lookup"><span data-stu-id="a0020-159">Run hello application again.</span></span>

<span data-ttu-id="a0020-160">Kör `npm start` igen.</span><span class="sxs-lookup"><span data-stu-id="a0020-160">Run `npm start` again.</span></span> 

```bash
npm start
```

<span data-ttu-id="a0020-161">Ett konsolmeddelande bör nu meddelar att hello utvecklingsmiljö är igång.</span><span class="sxs-lookup"><span data-stu-id="a0020-161">A console message should now tell you that hello development environment is up and running.</span></span> 

<span data-ttu-id="a0020-162">Navigera för`http://localhost:3000` i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a0020-162">Navigate too`http://localhost:3000` in a browser.</span></span> <span data-ttu-id="a0020-163">Klicka på **registrera dig** i hello översta menyn och försök toocreate två dummy användare.</span><span class="sxs-lookup"><span data-stu-id="a0020-163">Click **Sign Up** in hello top menu and try toocreate two dummy users.</span></span> 

<span data-ttu-id="a0020-164">Hej MEAN.js exempelprogrammet lagrar användardata i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="a0020-164">hello MEAN.js sample application stores user data in hello database.</span></span> <span data-ttu-id="a0020-165">Om du lyckas och MEAN.js automatiskt loggar in på hello användare och Azure Cosmos DB anslutningen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a0020-165">If you are successful and MEAN.js automatically signs into hello created user, then your Azure Cosmos DB connection is working.</span></span> 

![MEAN.js ansluter har tooMongoDB](./media/create-mongodb-nodejs/mongodb-connect-success.png)

## <a name="view-data-in-data-explorer"></a><span data-ttu-id="a0020-167">Visa data i datautforskaren</span><span class="sxs-lookup"><span data-stu-id="a0020-167">View data in Data Explorer</span></span>

<span data-ttu-id="a0020-168">Data som lagras av en Azure-Cosmos-databas är tillgängliga tooview frågan och kör affärslogik på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a0020-168">Data stored by an Azure Cosmos DB is available tooview, query, and run business-logic on in hello Azure portal.</span></span>

<span data-ttu-id="a0020-169">tooview, fråga och arbeta med hello användardata som skapats i hello föregående steg, inloggning toohello [Azure-portalen](https://portal.azure.com) i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="a0020-169">tooview, query, and work with hello user data created in hello previous step, login toohello [Azure portal](https://portal.azure.com) in your web browser.</span></span>

<span data-ttu-id="a0020-170">Ange Azure Cosmos DB i hello övre sökrutan.</span><span class="sxs-lookup"><span data-stu-id="a0020-170">In hello top Search box, type Azure Cosmos DB.</span></span> <span data-ttu-id="a0020-171">När ditt Cosmos DB-kontoblad öppnas, väljer du ditt Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="a0020-171">When your Cosmos DB account blade opens, select your Cosmos DB account.</span></span> <span data-ttu-id="a0020-172">I vänster navigeringsfält hello, klickar du på Data Explorer.</span><span class="sxs-lookup"><span data-stu-id="a0020-172">In hello left navigation, click Data Explorer.</span></span> <span data-ttu-id="a0020-173">Utöka din samling hello samlingar i fönstret och sedan du kan visa hello dokument i hello samling, fråga hello data, och även skapa och köra lagrade procedurer, utlösare och UDF: er.</span><span class="sxs-lookup"><span data-stu-id="a0020-173">Expand your collection in hello Collections pane, and then you can view hello documents in hello collection, query hello data, and even create and run stored procedures, triggers, and UDFs.</span></span> 

![Data Explorer i hello Azure-portalen](./media/create-mongodb-nodejs/cosmosdb-connect-mongodb-data-explorer.png)


## <a name="deploy-hello-nodejs-application-tooazure"></a><span data-ttu-id="a0020-175">Distribuera hello Node.js-program tooAzure</span><span class="sxs-lookup"><span data-stu-id="a0020-175">Deploy hello Node.js application tooAzure</span></span>

<span data-ttu-id="a0020-176">I det här steget kan distribuera du din MongoDB-anslutna Node.js-programmet tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a0020-176">In this step, you deploy your MongoDB-connected Node.js application tooAzure Cosmos DB.</span></span>

<span data-ttu-id="a0020-177">Kanske såg du att hello-konfigurationsfil som du tidigare har ändrat finns för hello utvecklingsmiljö (`/config/env/local-development.js`).</span><span class="sxs-lookup"><span data-stu-id="a0020-177">You may have noticed that hello configuration file that you changed earlier is for hello development environment (`/config/env/local-development.js`).</span></span> <span data-ttu-id="a0020-178">När du distribuerar ditt program tooApp Service körs i produktionsmiljö hello som standard.</span><span class="sxs-lookup"><span data-stu-id="a0020-178">When you deploy your application tooApp Service, it will run in hello production environment by default.</span></span> <span data-ttu-id="a0020-179">Så nu behöver du toomake hello samma ändra toohello respektive konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="a0020-179">So now, you need toomake hello same change toohello respective configuration file.</span></span>

<span data-ttu-id="a0020-180">I din MEAN.js-lagringsplats, öppnar du `config/env/production.js`.</span><span class="sxs-lookup"><span data-stu-id="a0020-180">In your MEAN.js repository, open `config/env/production.js`.</span></span>

<span data-ttu-id="a0020-181">I hello `db` objekt, Ersätt hello värdet för `uri` som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="a0020-181">In hello `db` object, replace hello value of `uri` as show in hello following example.</span></span> <span data-ttu-id="a0020-182">Vara säker på att tooreplace hello platshållare som innan.</span><span class="sxs-lookup"><span data-stu-id="a0020-182">Be sure tooreplace hello placeholders as before.</span></span>

```javascript
'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean?ssl=true&sslverifycertificate=false',
```

> [!NOTE] 
> <span data-ttu-id="a0020-183">Hej `ssl=true` alternativet är viktigt eftersom [Azure Cosmos DB kräver SSL](connect-mongodb-account.md#connection-string-requirements).</span><span class="sxs-lookup"><span data-stu-id="a0020-183">hello `ssl=true` option is important because [Azure Cosmos DB requires SSL](connect-mongodb-account.md#connection-string-requirements).</span></span> 
>
>

<span data-ttu-id="a0020-184">I hello terminal, genomför du alla dina ändringar i Git.</span><span class="sxs-lookup"><span data-stu-id="a0020-184">In hello terminal, commit all your changes into Git.</span></span> <span data-ttu-id="a0020-185">Du kan kopiera båda kommandon toorun dem tillsammans.</span><span class="sxs-lookup"><span data-stu-id="a0020-185">You can copy both commands toorun them together.</span></span>

```bash
git add .
git commit -m "configured MongoDB connection string"
```
## <a name="clean-up-resources"></a><span data-ttu-id="a0020-186">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="a0020-186">Clean up resources</span></span>

<span data-ttu-id="a0020-187">Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a0020-187">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="a0020-188">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="a0020-188">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="a0020-189">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="a0020-189">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0020-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0020-190">Next steps</span></span>

<span data-ttu-id="a0020-191">I den här snabbstarten du har lärt dig hur toocreate en Cosmos-databas med Azure-konto och skapa en MongoDB-samling med hello Data Explorer.</span><span class="sxs-lookup"><span data-stu-id="a0020-191">In this quickstart, you've learned how toocreate an Azure Cosmos DB account and create a MongoDB collection using hello Data Explorer.</span></span> <span data-ttu-id="a0020-192">Nu kan du migrera din MongoDB data tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a0020-192">You can now migrate your MongoDB data tooAzure Cosmos DB.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="a0020-193">Importera MongoDB-data till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a0020-193">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)
