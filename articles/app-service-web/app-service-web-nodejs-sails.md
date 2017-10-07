---
title: "aaaDeploy en Sails.js web app tooAzure Apptjänst | Microsoft Docs"
description: "Lär dig hur toodeploy Node.js-programmet Azure App Service. Den här kursen visar hur toodeploy en Sails.js-webbapp."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 8877ddc8-1476-45ae-9e7f-3c75917b4564
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: f5b2518b9c87c040845f7268763862be8c15e83e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-sailsjs-web-app-tooazure-app-service"></a><span data-ttu-id="6e8ef-104">Distribuera en Sails.js web app tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="6e8ef-104">Deploy a Sails.js web app tooAzure App Service</span></span>
<span data-ttu-id="6e8ef-105">De här självstudierna visar hur toodeploy en Sails.js app tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-105">This tutorial shows you how toodeploy a Sails.js app tooAzure App Service.</span></span> <span data-ttu-id="6e8ef-106">Hello pågående, kan du glean vissa allmänna kunskaper om hur tooconfigure toorun din Node.js-app i App Service.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-106">In hello process, you can glean some general knowledge on how tooconfigure your Node.js app toorun in App Service.</span></span>

<span data-ttu-id="6e8ef-107">Här kan du lära dig användbara kunskaper som:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-107">Here, you will learn useful skills like:</span></span>

* <span data-ttu-id="6e8ef-108">Konfigurera en Sails.js-app som körs i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-108">Configure a Sails.js app run in App Service.</span></span>
* <span data-ttu-id="6e8ef-109">Distribuera en app tooApp tjänsten från hello kommandorad.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-109">Deploy an app tooApp Service from hello command line.</span></span>
* <span data-ttu-id="6e8ef-110">Läsa stderr och stdout loggar tootroubleshoot eventuella distributionsproblem.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-110">Read stderr and stdout logs tootroubleshoot any deployment issues.</span></span>
* <span data-ttu-id="6e8ef-111">Lagra miljövariabler utanför källkontroll.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-111">Store environment variables outside of source control.</span></span>
* <span data-ttu-id="6e8ef-112">Azure miljövariabler för åtkomst från din app.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-112">Access Azure environment variables from your app.</span></span>
* <span data-ttu-id="6e8ef-113">Ansluta tooa databasen (MongoDB).</span><span class="sxs-lookup"><span data-stu-id="6e8ef-113">Connect tooa database (MongoDB).</span></span>

<span data-ttu-id="6e8ef-114">Du bör ha kunskaper om Sails.js.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-114">You should have working knowledge of Sails.js.</span></span> <span data-ttu-id="6e8ef-115">Den här kursen är inte avsedda toohelp du med problem relaterade toorunning Sail.js i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-115">This tutorial is not intended toohelp you with issues related toorunning Sail.js in general.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="6e8ef-116">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="6e8ef-116">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="6e8ef-117">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-117">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="6e8ef-118">[Azure CLI 1.0](app-service-web-nodejs-sails-cli-nodejs.md) – våra CLI för hello klassisk och resurs management distributionsmodeller</span><span class="sxs-lookup"><span data-stu-id="6e8ef-118">[Azure CLI 1.0](app-service-web-nodejs-sails-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span>
- <span data-ttu-id="6e8ef-119">[Azure CLI 2.0](app-service-web-nodejs-sails.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="6e8ef-119">[Azure CLI 2.0](app-service-web-nodejs-sails.md) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e8ef-120">Krav</span><span class="sxs-lookup"><span data-stu-id="6e8ef-120">Prerequisites</span></span>
* [<span data-ttu-id="6e8ef-121">Node.js</span><span class="sxs-lookup"><span data-stu-id="6e8ef-121">Node.js</span></span>](https://nodejs.org/)
* [<span data-ttu-id="6e8ef-122">Sails.js</span><span class="sxs-lookup"><span data-stu-id="6e8ef-122">Sails.js</span></span>](http://sailsjs.org/get-started)
* [<span data-ttu-id="6e8ef-123">Git</span><span class="sxs-lookup"><span data-stu-id="6e8ef-123">Git</span></span>](http://www.git-scm.com/downloads)
* [<span data-ttu-id="6e8ef-124">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="6e8ef-124">Azure CLI 2.0</span></span>](/cli/azure/install-az-cli2)
* <span data-ttu-id="6e8ef-125">Ett Microsoft Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-125">A Microsoft Azure account.</span></span> <span data-ttu-id="6e8ef-126">Om du inte har ett konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="6e8ef-126">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="6e8ef-127">Du kan [Prova App Service](https://azure.microsoft.com/try/app-service/) utan ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-127">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="6e8ef-128">Skapa en startapp och spela upp med den för in tooan timme – inget kreditkort behövs, inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-128">Create a starter app and play with it for up tooan hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="step-1-create-and-configure-a-sailsjs-app-locally"></a><span data-ttu-id="6e8ef-129">Steg 1: Skapa och konfigurera en Sails.js-appen lokalt</span><span class="sxs-lookup"><span data-stu-id="6e8ef-129">Step 1: Create and configure a Sails.js app locally</span></span>
<span data-ttu-id="6e8ef-130">Först snabbt skapa en standard Sails.js-app i din utvecklingsmiljö genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-130">First, quickly create a default Sails.js app in your development environment by following these steps:</span></span>

1. <span data-ttu-id="6e8ef-131">Öppna hello kommandoradsverktyget terminal önskat och `CD` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-131">Open hello command-line terminal of your choice and `CD` tooa working directory.</span></span>
2. <span data-ttu-id="6e8ef-132">Skapa en Sails.js-app och kör:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-132">Create a Sails.js app and run it:</span></span>

        sails new <app_name>
        cd <app_name>
        sails lift

    <span data-ttu-id="6e8ef-133">Kontrollera att du kan navigera toohello standardstartsida på http://localhost:1377.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-133">Make sure you can navigate toohello default home page at http://localhost:1377.</span></span>

1. <span data-ttu-id="6e8ef-134">Därefter aktiverar du loggning för Azure.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-134">Next, enable logging for Azure.</span></span> <span data-ttu-id="6e8ef-135">Skapa en fil med namnet i din rotkatalog `iisnode.yml` och Lägg till följande två rader hello:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-135">In your root directory, create a file called `iisnode.yml` and add hello following two lines:</span></span>

        loggingEnabled: true
        logDirectory: iisnode

    <span data-ttu-id="6e8ef-136">Loggning har nu aktiverats för hello [iisnode](https://github.com/tjanczuk/iisnode) server att Azure Apptjänst använder toorun Node.js-appar.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-136">Logging is now enabled for hello [iisnode](https://github.com/tjanczuk/iisnode) server that Azure App Service uses toorun Node.js apps.</span></span> 
    <span data-ttu-id="6e8ef-137">Mer information om hur detta fungerar finns [hur toodebug en Node.js webbapp i Azure App Service](web-sites-nodejs-debug.md).</span><span class="sxs-lookup"><span data-stu-id="6e8ef-137">For more information on how this works, see [How toodebug a Node.js web app in Azure App Service](web-sites-nodejs-debug.md).</span></span>

2. <span data-ttu-id="6e8ef-138">Konfigurera Azure miljövariabler för hello Sails.js för app toouse.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-138">Next, configure hello Sails.js app toouse Azure environment variables.</span></span> <span data-ttu-id="6e8ef-139">Öppna config/env/production.js tooconfigure produktionsmiljön och ange `port` och `hookTimeout`:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-139">Open config/env/production.js tooconfigure your production environment, and set `port` and `hookTimeout`:</span></span>

        module.exports = {

            // Use process.env.port toohandle web requests toohello default HTTP port
            port: process.env.port,
            // Increase hooks timout too30 seconds
            // This avoids hello Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    <span data-ttu-id="6e8ef-140">Du hittar dokumentationen för dessa konfigurationsinställningar i de [Sails.js dokumentationen](http://sailsjs.org/documentation/reference/configuration/sails-config).</span><span class="sxs-lookup"><span data-stu-id="6e8ef-140">You can find documentation for these configuration settings in the  [Sails.js Documentation](http://sailsjs.org/documentation/reference/configuration/sails-config).</span></span>

4. <span data-ttu-id="6e8ef-141">Nästa, hårdkoda hello Node.js-version du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-141">Next, hardcode hello Node.js version you want toouse.</span></span> <span data-ttu-id="6e8ef-142">Lägg till följande hello i package.json, `engines` egenskapen tooset hello Node.js version tooone som vi vill.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-142">In package.json, add hello following `engines` property tooset hello Node.js version tooone that we want.</span></span>

        "engines": {
            "node": "6.9.1"
        },

5. <span data-ttu-id="6e8ef-143">Slutligen initiera en Git-lagringsplats och bekräfta dina filer.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-143">Finally, initialize a Git repository and commit your files.</span></span> <span data-ttu-id="6e8ef-144">I hello tillämpningsprogrammets rot (där package.json är), kör hello följande Git-kommandon:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-144">In hello application root (where package.json is), run hello following Git commands:</span></span>

        git init
        git add .
        git commit -m "<your commit message>"

<span data-ttu-id="6e8ef-145">Koden är klar toobe distribueras.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-145">Your code is ready toobe deployed.</span></span> 

## <a name="step-2-create-an-azure-app-and-deploy-sailsjs"></a><span data-ttu-id="6e8ef-146">Steg 2: Skapa en Azure-program och distribuera Sails.js</span><span class="sxs-lookup"><span data-stu-id="6e8ef-146">Step 2: Create an Azure app and deploy Sails.js</span></span>

<span data-ttu-id="6e8ef-147">Därefter skapar hello App Service-resurs i Azure och distribuera din app Sails.js-tooit.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-147">Next, create hello App Service resource in Azure and deploy your Sails.js app tooit.</span></span>

1. <span data-ttu-id="6e8ef-148">logga in tooAzure som detta:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-148">log in tooAzure like so:</span></span>

        az login

    <span data-ttu-id="6e8ef-149">Följ hello fråga toocontinue hello inloggning i en webbläsare med ett Microsoft-konto som har din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-149">Follow hello prompt toocontinue hello login in a browser with a Microsoft account that has your Azure subscription.</span></span>

3. <span data-ttu-id="6e8ef-150">Ange hello distribution användaren för Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-150">Set hello deployment user for App Service.</span></span> <span data-ttu-id="6e8ef-151">Du distribuerar kod med dessa autentiseringsuppgifterna senare.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-151">You will deploy code using these credentials later.</span></span>
   
        az appservice web deployment user set --user-name <username> --password <password>

3. <span data-ttu-id="6e8ef-152">Skapa en [resursgruppen](../azure-resource-manager/resource-group-overview.md) med ett namn.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-152">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with a name.</span></span> <span data-ttu-id="6e8ef-153">För den här självstudien om Node.js behöver du inte verkligen tooknow vad det är.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-153">For this Node.js tutorial, you don't really need tooknow what it is.</span></span>

        az group create --location "<location>" --name my-sailsjs-app-group

    <span data-ttu-id="6e8ef-154">toosee vilka möjliga värden du kan använda för `<location>`, använda hello `az appservice list-locations` CLI-kommando.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-154">toosee what possible values you can use for `<location>`, use hello `az appservice list-locations` CLI command.</span></span>

3. <span data-ttu-id="6e8ef-155">Skapa en ”gratis” [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) med ett namn.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-155">Create a "FREE" [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) with a name.</span></span> <span data-ttu-id="6e8ef-156">För den här självstudien om Node.js, ska du bara vet att du inte kommer att debiteras för webbprogram i den här planen.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-156">For this Node.js tutorial, just know that you won't be charged for web apps in this plan.</span></span>

        az appservice plan create --name my-sailsjs-appservice-plan --resource-group my-sailsjs-app-group --sku FREE

4. <span data-ttu-id="6e8ef-157">Skapa en ny webbapp med ett unikt namn i `<app_name>`.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-157">Create a new web app with a unique name in `<app_name>`.</span></span>

        az appservice web create --name <app_name> --resource-group my-sailsjs-app-group --plan my-sailsjs-appservice-plan

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a><span data-ttu-id="6e8ef-158">Steg 3: Konfigurera och distribuera en Sails.js-app</span><span class="sxs-lookup"><span data-stu-id="6e8ef-158">Step 3: Configure and deploy your Sails.js app</span></span>

1. <span data-ttu-id="6e8ef-159">Konfigurera lokal Git-distribution för din nya webbapp med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-159">Configure local Git deployment for your new web app with hello following command:</span></span>

        az appservice web source-control config-local-git --name <app_name> --resource-group my-sailsjs-app-group

    <span data-ttu-id="6e8ef-160">Du får en JSON-utdata som liknar detta, vilket innebär att hello fjärransluten Git-lagringsplats har konfigurerats:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-160">You will get a JSON output like this, which means that hello remote Git repository is configured:</span></span>

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. <span data-ttu-id="6e8ef-161">Lägg till hello URL i hello JSON som en fjärransluten Git för lokala databasen (kallas `azure` för enkelhetens skull).</span><span class="sxs-lookup"><span data-stu-id="6e8ef-161">Add hello URL in hello JSON as a Git remote for your local repository (called `azure` for simplicity).</span></span>

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. <span data-ttu-id="6e8ef-162">Distribuera din exempel kod toohello `azure` fjärransluten Git.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-162">Deploy your sample code toohello `azure` Git remote.</span></span> <span data-ttu-id="6e8ef-163">När du uppmanas, använda autentiseringsuppgifter för distribution hello du tidigare har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-163">When prompted, use hello deployment credentials you configured earlier.</span></span>

        git push azure master

7. <span data-ttu-id="6e8ef-164">Bara starta till sist din aktiva Azure-app i hello webbläsare:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-164">Finally, just launch your live Azure app in hello browser:</span></span>

        az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

    <span data-ttu-id="6e8ef-165">Du bör nu se hello samma Sails.js-startsidan.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-165">You should now see hello same Sails.js home page.</span></span>

    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a><span data-ttu-id="6e8ef-166">Felsöka distributionen</span><span class="sxs-lookup"><span data-stu-id="6e8ef-166">Troubleshoot your deployment</span></span>
<span data-ttu-id="6e8ef-167">Om tillämpningsprogrammet Sails.js misslyckas av någon anledning i App Service, hitta hello stderr-loggar toohelp felsöka den.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-167">If your Sails.js application fails for some reason in App Service, find hello stderr logs toohelp troubleshoot it.</span></span>
<span data-ttu-id="6e8ef-168">Mer information finns i [hur toodebug en Node.js webbapp i Azure App Service](web-sites-nodejs-debug.md).</span><span class="sxs-lookup"><span data-stu-id="6e8ef-168">For more information, see [How toodebug a Node.js web app in Azure App Service](web-sites-nodejs-debug.md).</span></span>
<span data-ttu-id="6e8ef-169">Om hello app har startats hello stdout loggen ska visa bekant hello-meddelande:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-169">If hello app has started successfully, hello stdout log should show you hello familiar message:</span></span>

                   .-..-.
    
       Sails              <|    .-..-.
       v0.12.11            |\
                          /|.\
                         / || \
                       ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
       __---___--___---___--___---___--___
     ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    toosee your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    tooshut down Sails, press <CTRL> + C at any time.

<span data-ttu-id="6e8ef-170">Du kan styra granularitet hello stdout loggar i hello [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) fil.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-170">You can control granularity of hello stdout logs in hello [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) file.</span></span>

## <a name="connect-tooa-database-in-azure"></a><span data-ttu-id="6e8ef-171">Ansluta tooa databas i Azure</span><span class="sxs-lookup"><span data-stu-id="6e8ef-171">Connect tooa database in Azure</span></span>
<span data-ttu-id="6e8ef-172">tooconnect tooa databas i Azure måste du skapar hello databasen önskat i Azure, till exempel Azure SQL Database, MySQL, MongoDB, Azure (Redis)-Cache, osv., och använder hello motsvarande [datastore kortet](https://github.com/balderdashy/sails#compatibility) tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-172">tooconnect tooa database in Azure, you create hello database of your choice in Azure, such as Azure SQL Database, MySQL, MongoDB, Azure (Redis) Cache, etc., and use hello corresponding [datastore adapter](https://github.com/balderdashy/sails#compatibility) tooconnect tooit.</span></span> <span data-ttu-id="6e8ef-173">hello steg i det här avsnittet visar hur tooconnect tooMongoDB med hjälp av en [Azure Cosmos DB](../documentdb/documentdb-protocol-mongodb.md) databas som har stöd för MongoDB-klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-173">hello steps in this section show you how tooconnect tooMongoDB by using an [Azure Cosmos DB](../documentdb/documentdb-protocol-mongodb.md) database, which can support MongoDB client connections.</span></span>

1. <span data-ttu-id="6e8ef-174">[Skapa ett Cosmos-DB-konto med Protokollstöd för MongoDB](../documentdb/documentdb-create-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="6e8ef-174">[Create a Cosmos DB account with MongoDB protocol support](../documentdb/documentdb-create-mongodb-account.md).</span></span>
2. <span data-ttu-id="6e8ef-175">[Skapa en Cosmos-DB-samling och databasen](../documentdb/documentdb-create-collection.md).</span><span class="sxs-lookup"><span data-stu-id="6e8ef-175">[Create a Cosmos DB collection and database](../documentdb/documentdb-create-collection.md).</span></span> <span data-ttu-id="6e8ef-176">hello namnet på hello samling spelar ingen roll, men du måste hello namnet på hello-databasen när du ansluter från Sails.js.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-176">hello name of hello collection doesn't matter, but you need hello name of hello database when you connect from Sails.js.</span></span>
3. <span data-ttu-id="6e8ef-177">[Hitta hello anslutningsinformation för databasen Cosmos DB](../cosmos-db/connect-mongodb-account.md#GetCustomConnection).</span><span class="sxs-lookup"><span data-stu-id="6e8ef-177">[Find hello connection information for your Cosmos DB database](../cosmos-db/connect-mongodb-account.md#GetCustomConnection).</span></span>
2. <span data-ttu-id="6e8ef-178">Installera hello MongoDB-kort från kommandoraden terminalen:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-178">From your command-line terminal, install hello MongoDB adapter:</span></span>

        npm install sails-mongo --save

3. <span data-ttu-id="6e8ef-179">Öppna config/connections.js och Lägg till hello efter anslutning objektlistan toohello:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-179">Open config/connections.js and add hello following connection object toohello list:</span></span>

        docDbMongo: {
            adapter: 'sails-mongo',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost,
            port: process.env.dbport,
            database: process.env.dbname,
            ssl: true
        },

    > [!NOTE] 
    > <span data-ttu-id="6e8ef-180">Hej `ssl: true` alternativet är viktigt eftersom [Cosmos DB kräver](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span><span class="sxs-lookup"><span data-stu-id="6e8ef-180">hello `ssl: true` option is important because [Cosmos DB requires it](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span></span> 
    >
    >

4. <span data-ttu-id="6e8ef-181">För varje miljövariabel (`process.env.*`), behöver du tooset den i App Service.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-181">For each environment variable (`process.env.*`), you need tooset it in App Service.</span></span> <span data-ttu-id="6e8ef-182">toodo detta, kör hello följande kommandon från terminalen.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-182">toodo this, run hello following commands from your terminal.</span></span> <span data-ttu-id="6e8ef-183">Använd hello anslutningsinformationen för Cosmos-DB.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-183">Use hello connection information for your Cosmos DB.</span></span>

        az appservice web config appsettings update --settings dbuser="<database user>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbpassword="<database password>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbhost="<database hostname>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbport="<database port>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbname="<database name>" --name <app_name> --resource-group my-sailsjs-app-group

    <span data-ttu-id="6e8ef-184">Om du placerar dina inställningar i Azure app-inställningar håller känsliga data från din källkontrollen (Git).</span><span class="sxs-lookup"><span data-stu-id="6e8ef-184">Putting your settings in Azure app settings keeps sensitive data out of your source control (Git).</span></span> <span data-ttu-id="6e8ef-185">Därefter konfigurerar du din development environment toouse hello samma anslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-185">Next, you will configure your development environment toouse hello same connection information.</span></span>
5. <span data-ttu-id="6e8ef-186">Öppna config/local.js och Lägg till följande anslutningar objektet hello:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-186">Open config/local.js and add hello following connections object:</span></span>

        connections: {
            docDbMongo: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>",
                database: "<database name>",
                ssl: true
            },
        },

    <span data-ttu-id="6e8ef-187">Den här konfigurationen åsidosätter hello inställningar i filen config/connections.js för hello lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-187">This configuration overrides hello settings in your config/connections.js file for hello local environment.</span></span> <span data-ttu-id="6e8ef-188">Den här filen är exkluderad av hello standard .gitignore i projektet, så att den inte lagras i Git.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-188">This file is excluded by hello default .gitignore in your project, so it will not be stored in Git.</span></span> <span data-ttu-id="6e8ef-189">Du är nu, kan tooconnect tooyour Cosmos DB (MongoDB)-databas från Azure-webbapp och från din lokala utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-189">Now, you are able tooconnect tooyour Cosmos DB (MongoDB) database both from your Azure web app and from your local development environment.</span></span>
6. <span data-ttu-id="6e8ef-190">Öppna config/env/production.js tooconfigure produktionsmiljön och Lägg till följande hello `models` objekt:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-190">Open config/env/production.js tooconfigure your production environment, and add hello following `models` object:</span></span>

        models: {
            connection: 'docDbMongo',
            migrate: 'safe'
        },
7. <span data-ttu-id="6e8ef-191">Öppna config/env/development.js tooconfigure utvecklingsmiljön och Lägg till följande hello `models` objekt:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-191">Open config/env/development.js tooconfigure your development environment, and add hello following `models` object:</span></span>

        models: {
            connection: 'docDbMongo',
            migrate: 'alter'
        },

    <span data-ttu-id="6e8ef-192">`migrate: 'alter'`kan du använda databasen migrering funktioner toocreate och enkelt uppdatera databasen samlingar eller tabeller.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-192">`migrate: 'alter'` lets you use database migration features toocreate and update database collections or tables easily.</span></span> <span data-ttu-id="6e8ef-193">Dock `migrate: 'safe'` används för miljön i Azure (produktion) eftersom Sails.js inte tillåter toouse `migrate: 'alter'` i en produktionsmiljö (se [Sails.js dokumentationen](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).</span><span class="sxs-lookup"><span data-stu-id="6e8ef-193">However, `migrate: 'safe'` is used for your Azure (production) environment because Sails.js does not allow you toouse `migrate: 'alter'` in a production environment (see [Sails.js Documentation](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).</span></span>
8. <span data-ttu-id="6e8ef-194">Från hello terminal, [generera](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) en Sails.js [modell API](http://sailsjs.org/documentation/concepts/blueprints) som vanligtvis skulle sedan kör du `sails lift` att skapa hello databas med Sails.js Databasmigrering.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-194">From hello terminal, [generate](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) a Sails.js [blueprint API](http://sailsjs.org/documentation/concepts/blueprints) like you normally would, then run `sails lift` to create hello database with Sails.js database migration.</span></span> <span data-ttu-id="6e8ef-195">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-195">For example:</span></span>

         sails generate api mywidget
         sails lift

    <span data-ttu-id="6e8ef-196">Hej `mywidget` modell som genererats av det här kommandot är tom, men vi kan använda den tooshow som vi har databasanslutning.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-196">hello `mywidget` model generated by this command is empty, but we can use it tooshow that we have database connectivity.</span></span>
    <span data-ttu-id="6e8ef-197">När du kör `sails lift`, skapar den hello saknas samlingar och tabeller för hello modeller använder din app.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-197">When you run `sails lift`, it creates hello missing collections and tables for hello models your app uses.</span></span>
9. <span data-ttu-id="6e8ef-198">Åtkomst hello utkast API du skapade i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-198">Access hello blueprint API you just created in hello browser.</span></span> <span data-ttu-id="6e8ef-199">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-199">For example:</span></span>

        http://localhost:1337/mywidget/create

    <span data-ttu-id="6e8ef-200">hello API ska returnera hello skapat posten tillbaka tooyou hello i fönster i webbläsaren, vilket innebär att din samling har skapats.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-200">hello API should return hello created entry back tooyou in hello browser window, which means that your collection is created  successfully.</span></span>

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}
10. <span data-ttu-id="6e8ef-201">Nu push tooAzure dina ändringar och bläddra tooyour app toomake att den fortfarande fungerar.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-201">Now, push your changes tooAzure, and browse tooyour app toomake sure it still works.</span></span>

         git add .
         git commit -m "<your commit message>"
         git push azure master
         az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

11. <span data-ttu-id="6e8ef-202">Åtkomst hello utkast API för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-202">Access hello blueprint API of your Azure web app.</span></span> <span data-ttu-id="6e8ef-203">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6e8ef-203">For example:</span></span>

         http://<appname>.azurewebsites.net/mywidget/create

     <span data-ttu-id="6e8ef-204">Om en annan ny post returnerar hello API pratar Azure-webbapp tooyour Cosmos DB (MongoDB)-databas.</span><span class="sxs-lookup"><span data-stu-id="6e8ef-204">If hello API returns another new entry, then your Azure web app is talking tooyour Cosmos DB (MongoDB) database.</span></span>

## <a name="more-resources"></a><span data-ttu-id="6e8ef-205">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="6e8ef-205">More resources</span></span>
* [<span data-ttu-id="6e8ef-206">Kom igång med Node.js-webbappar i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6e8ef-206">Get started with Node.js web apps in Azure App Service</span></span>](app-service-web-get-started-nodejs.md)
* [<span data-ttu-id="6e8ef-207">Använda Node.js-moduler med Azure-program</span><span class="sxs-lookup"><span data-stu-id="6e8ef-207">Using Node.js Modules with Azure applications</span></span>](../nodejs-use-node-modules-azure-apps.md)
