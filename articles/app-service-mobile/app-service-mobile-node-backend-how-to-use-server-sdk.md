---
title: "Hur du arbetar med serverdelen Node.js SDK för Mobile Apps | Microsoft Docs"
description: "Lär dig hur du arbetar med serverdelen Node.js SDK för Azure Apptjänst Mobilappar."
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 1d3aa7a0089279a8eafeb0ded951a5238e189eaa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a><span data-ttu-id="5d088-103">Hur du använder Azure Mobile Apps Node.js SDK</span><span class="sxs-lookup"><span data-stu-id="5d088-103">How to use the Azure Mobile Apps Node.js SDK</span></span>
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

<span data-ttu-id="5d088-104">Den här artikeln innehåller detaljerad information och exempel som visar hur du arbetar med en Node.js-serverdel i Azure Apptjänst Mobilappar.</span><span class="sxs-lookup"><span data-stu-id="5d088-104">This article provides detailed information and examples showing how to work with a Node.js backend in Azure App Service Mobile Apps.</span></span>

## <span data-ttu-id="5d088-105"><a name="Introduction"></a>Introduktion</span><span class="sxs-lookup"><span data-stu-id="5d088-105"><a name="Introduction"></a>Introduction</span></span>
<span data-ttu-id="5d088-106">Azure Apptjänst Mobilappar ger möjlighet att lägga till en-optimerad dataåtkomst Web API till ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="5d088-106">Azure App Service Mobile Apps provides the capability to add a mobile-optimized data access Web API to a web application.</span></span>  <span data-ttu-id="5d088-107">Azure App Service Mobile Apps-SDK har angetts för ASP.NET och Node.js webbprogram.</span><span class="sxs-lookup"><span data-stu-id="5d088-107">The Azure App Service Mobile Apps SDK is provided for ASP.NET and Node.js web applications.</span></span>  <span data-ttu-id="5d088-108">SDK innehåller följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="5d088-108">The SDK provides the following operations:</span></span>

* <span data-ttu-id="5d088-109">Tabellåtgärder (Läs-, Insert-, Update-, Delete) för dataåtkomst</span><span class="sxs-lookup"><span data-stu-id="5d088-109">Table operations (Read, Insert, Update, Delete) for data access</span></span>
* <span data-ttu-id="5d088-110">Anpassade API-åtgärder</span><span class="sxs-lookup"><span data-stu-id="5d088-110">Custom API operations</span></span>

<span data-ttu-id="5d088-111">Både operations erbjuda autentisering över alla identitetsleverantörer som tillåts av Azure App Service, inklusive sociala identitetsleverantörer, till exempel Facebook, Twitter, Google och Microsoft samt Azure Active Directory för enterprise-identitet.</span><span class="sxs-lookup"><span data-stu-id="5d088-111">Both operations provide for authentication across all identity providers allowed by Azure App Service, including social identity providers such as Facebook, Twitter, Google and Microsoft as well as Azure Active Directory for enterprise identity.</span></span>

<span data-ttu-id="5d088-112">Du kan hitta exempel för varje användningsfall som i den [katalogen samples på GitHub].</span><span class="sxs-lookup"><span data-stu-id="5d088-112">You can find samples for each use case in the [samples directory on GitHub].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="5d088-113">Plattformar som stöds</span><span class="sxs-lookup"><span data-stu-id="5d088-113">Supported Platforms</span></span>
<span data-ttu-id="5d088-114">Azure Mobile Apps nod SDK har stöd för den aktuella LTS-versionen av noden och senare.</span><span class="sxs-lookup"><span data-stu-id="5d088-114">The Azure Mobile Apps Node SDK supports the current LTS release of Node and later.</span></span>  <span data-ttu-id="5d088-115">Från och med skrivning är den senaste versionen av LTS nod v4.5.0.</span><span class="sxs-lookup"><span data-stu-id="5d088-115">As of writing, the latest LTS version is Node v4.5.0.</span></span>  <span data-ttu-id="5d088-116">Andra versioner av noden fungerar men stöds inte.</span><span class="sxs-lookup"><span data-stu-id="5d088-116">Other versions of Node may work, but are not supported.</span></span>

<span data-ttu-id="5d088-117">Azure Mobile Apps nod SDK stöder två databasdrivrutiner för - nod mssql-drivrutinen har stöd för SQL Azure och lokala SQL Server-instanser.</span><span class="sxs-lookup"><span data-stu-id="5d088-117">The Azure Mobile Apps Node SDK supports two database drivers - the node-mssql driver supports SQL Azure and local SQL Server instances.</span></span>  <span data-ttu-id="5d088-118">Sqlite3-drivrutinen stöder SQLite databaser på en enda instans.</span><span class="sxs-lookup"><span data-stu-id="5d088-118">The sqlite3 driver supports SQLite databases on a single instance only.</span></span>

### <span data-ttu-id="5d088-119"><a name="howto-cmdline-basicapp"></a>Så här: skapa ett grundläggande Node.js-serverdel med hjälp av kommandoraden</span><span class="sxs-lookup"><span data-stu-id="5d088-119"><a name="howto-cmdline-basicapp"></a>How to: Create a Basic Node.js backend using the Command Line</span></span>
<span data-ttu-id="5d088-120">Varje Azure App Service Mobile App Node.js-serverdel startar som ett ExpressJS program.</span><span class="sxs-lookup"><span data-stu-id="5d088-120">Every Azure App Service Mobile App Node.js backend starts as an ExpressJS application.</span></span>  <span data-ttu-id="5d088-121">ExpressJS är det mest populära web service ramverket för Node.js.</span><span class="sxs-lookup"><span data-stu-id="5d088-121">ExpressJS is the most popular web service framework available for Node.js.</span></span>  <span data-ttu-id="5d088-122">Du kan skapa en grundläggande [Express] program på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5d088-122">You can create a basic [Express] application as follows:</span></span>

1. <span data-ttu-id="5d088-123">Skapa en katalog för ditt projekt i ett kommando eller PowerShell-fönstret.</span><span class="sxs-lookup"><span data-stu-id="5d088-123">In a command or PowerShell window, create a directory for your project.</span></span>

        mkdir basicapp
2. <span data-ttu-id="5d088-124">Kör npm init för att initiera paketet strukturen.</span><span class="sxs-lookup"><span data-stu-id="5d088-124">Run npm init to initialize the package structure.</span></span>

        cd basicapp
        npm init

    <span data-ttu-id="5d088-125">Kommandot npm init frågar en uppsättning frågor att starta projektet.</span><span class="sxs-lookup"><span data-stu-id="5d088-125">The npm init command asks a set of questions to initialize the project.</span></span>  <span data-ttu-id="5d088-126">Se de exempel på utdatan:</span><span class="sxs-lookup"><span data-stu-id="5d088-126">See the example output:</span></span>

    ![Npm init-utdata][0]
3. <span data-ttu-id="5d088-128">Installera express och azure mobilappar bibliotek från npm-databasen.</span><span class="sxs-lookup"><span data-stu-id="5d088-128">Install the express and azure-mobile-apps libraries from the npm repository.</span></span>

        npm install --save express azure-mobile-apps
4. <span data-ttu-id="5d088-129">Skapa en app.js-fil om du vill implementera grundläggande mobila server.</span><span class="sxs-lookup"><span data-stu-id="5d088-129">Create an app.js file to implement the basic mobile server.</span></span>

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

<span data-ttu-id="5d088-130">Det här programmet skapar en optimerad för-WebAPI med en enda slutpunkt (`/tables/TodoItem`) som ger obehörig åtkomst till en underliggande SQL-datalagret med en dynamisk schema.</span><span class="sxs-lookup"><span data-stu-id="5d088-130">This application creates a mobile-optimized WebAPI with a single endpoint (`/tables/TodoItem`) that provides unauthenticated access to an underlying SQL data store using a dynamic schema.</span></span>  <span data-ttu-id="5d088-131">Det är lämpligt för följande snabbstarter för klient-biblioteket:</span><span class="sxs-lookup"><span data-stu-id="5d088-131">It is suitable for following the client library quick starts:</span></span>

* <span data-ttu-id="5d088-132">[Snabbstart för Android-klienten]</span><span class="sxs-lookup"><span data-stu-id="5d088-132">[Android Client QuickStart]</span></span>
* <span data-ttu-id="5d088-133">[Snabbstart för Apache Cordova-klient]</span><span class="sxs-lookup"><span data-stu-id="5d088-133">[Apache Cordova Client QuickStart]</span></span>
* <span data-ttu-id="5d088-134">[iOS klienten Snabbstart]</span><span class="sxs-lookup"><span data-stu-id="5d088-134">[iOS Client QuickStart]</span></span>
* <span data-ttu-id="5d088-135">[Snabbstart för Windows Store-klienten]</span><span class="sxs-lookup"><span data-stu-id="5d088-135">[Windows Store Client QuickStart]</span></span>
* <span data-ttu-id="5d088-136">[Snabbstart för Xamarin.iOS-klient]</span><span class="sxs-lookup"><span data-stu-id="5d088-136">[Xamarin.iOS Client QuickStart]</span></span>
* <span data-ttu-id="5d088-137">[Snabbstart för Xamarin.Android-klient]</span><span class="sxs-lookup"><span data-stu-id="5d088-137">[Xamarin.Android Client QuickStart]</span></span>
* <span data-ttu-id="5d088-138">[Snabbstart för Xamarin.Forms-klient]</span><span class="sxs-lookup"><span data-stu-id="5d088-138">[Xamarin.Forms Client QuickStart]</span></span>

<span data-ttu-id="5d088-139">Du hittar koden för den här grundläggande program i den [basicapp exempel på GitHub].</span><span class="sxs-lookup"><span data-stu-id="5d088-139">You can find the code for this basic application in the [basicapp sample on GitHub].</span></span>

### <span data-ttu-id="5d088-140"><a name="howto-vs2015-basicapp"></a>Så här: skapa en nod serverdel med Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="5d088-140"><a name="howto-vs2015-basicapp"></a>How to: Create a Node backend with Visual Studio 2015</span></span>
<span data-ttu-id="5d088-141">Visual Studio 2015 kräver ett tillägg till utvecklar Node.js-program i IDE.</span><span class="sxs-lookup"><span data-stu-id="5d088-141">Visual Studio 2015 requires an extension to develop Node.js applications within the IDE.</span></span>  <span data-ttu-id="5d088-142">Starta genom att installera den [1.1 för Node.js-verktyg för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="5d088-142">To start, install the [Node.js Tools 1.1 for Visual Studio].</span></span>  <span data-ttu-id="5d088-143">När Node.js-verktyg för Visual Studio har installerats kan du skapa ett Express 4.x program:</span><span class="sxs-lookup"><span data-stu-id="5d088-143">Once the Node.js Tools for Visual Studio are installed, create an Express 4.x application:</span></span>

1. <span data-ttu-id="5d088-144">Öppna den **nytt projekt** dialogrutan (från **filen** > **ny** > **projekt...** ).</span><span class="sxs-lookup"><span data-stu-id="5d088-144">Open the **New Project** dialog (from **File** > **New** > **Project...**).</span></span>
2. <span data-ttu-id="5d088-145">Expandera **mallar** > **JavaScript** > **Node.js**.</span><span class="sxs-lookup"><span data-stu-id="5d088-145">Expand **Templates** > **JavaScript** > **Node.js**.</span></span>
3. <span data-ttu-id="5d088-146">Välj den **grundläggande Azure Node.js Express 4 programmet**.</span><span class="sxs-lookup"><span data-stu-id="5d088-146">Select the **Basic Azure Node.js Express 4 Application**.</span></span>
4. <span data-ttu-id="5d088-147">Fyll i projektets namn.</span><span class="sxs-lookup"><span data-stu-id="5d088-147">Fill in the project name.</span></span>  <span data-ttu-id="5d088-148">Klicka på *OK*.</span><span class="sxs-lookup"><span data-stu-id="5d088-148">Click *OK*.</span></span>

    ![Nytt projekt i Visual Studio 2015][1]
5. <span data-ttu-id="5d088-150">Högerklicka på den **npm** och välj **installera nya npm-paket...** .</span><span class="sxs-lookup"><span data-stu-id="5d088-150">Right-click the **npm** node and select **Install New npm packages...**.</span></span>
6. <span data-ttu-id="5d088-151">Du kan behöva uppdatera npm-katalogen för att skapa din första Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="5d088-151">You may need to refresh the npm catalog on creating your first Node.js application.</span></span>  <span data-ttu-id="5d088-152">Klicka på **uppdatera** om det behövs.</span><span class="sxs-lookup"><span data-stu-id="5d088-152">Click **Refresh** if necessary.</span></span>
7. <span data-ttu-id="5d088-153">Ange *azure mobilappar* i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="5d088-153">Enter *azure-mobile-apps* in the search box.</span></span>  <span data-ttu-id="5d088-154">Klicka på den **azure mobile apps 2.0.0** paketet och klicka sedan på **installera paket**.</span><span class="sxs-lookup"><span data-stu-id="5d088-154">Click the **azure-mobile-apps 2.0.0** package, then click **Install Package**.</span></span>

    ![Installera nya npm-paket][2]
8. <span data-ttu-id="5d088-156">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="5d088-156">Click **Close**.</span></span>
9. <span data-ttu-id="5d088-157">Öppna den *app.js* filen att ge stöd för Azure Mobile Apps-SDK.</span><span class="sxs-lookup"><span data-stu-id="5d088-157">Open the *app.js* file to add support for the Azure Mobile Apps SDK.</span></span>  <span data-ttu-id="5d088-158">Kräver uttryck på rad 6 at längst ned i biblioteket, Lägg till följande kod:</span><span class="sxs-lookup"><span data-stu-id="5d088-158">At line 6 at the bottom of the library require statements, add the following code:</span></span>

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    <span data-ttu-id="5d088-159">På cirka rad 27 efter andra app.use-instruktioner, lägger du till följande kod:</span><span class="sxs-lookup"><span data-stu-id="5d088-159">At approximately line 27 after the other app.use statements, add the following code:</span></span>

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    <span data-ttu-id="5d088-160">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="5d088-160">Save the file.</span></span>
10. <span data-ttu-id="5d088-161">Kör programmet lokalt (API: N hanteras på http://localhost: 3000) eller publicera till Azure.</span><span class="sxs-lookup"><span data-stu-id="5d088-161">Either run the application locally (the API is served on http://localhost:3000) or publish to Azure.</span></span>

### <span data-ttu-id="5d088-162"><a name="create-node-backend-portal"></a>Så här: skapa en Node.js-serverdel i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5d088-162"><a name="create-node-backend-portal"></a>How to: Create a Node.js backend using the Azure portal</span></span>
<span data-ttu-id="5d088-163">Du kan skapa en Mobilapp serverdel direkt i den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="5d088-163">You can create a Mobile App backend right in the [Azure portal].</span></span> <span data-ttu-id="5d088-164">Du kan följa anvisningarna nedan eller skapa en klient och server tillsammans genom att följa den [skapar en mobil app](app-service-mobile-ios-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="5d088-164">You can either follow the following steps or create a client and server together by following the [Create a mobile app](app-service-mobile-ios-get-started.md) tutorial.</span></span> <span data-ttu-id="5d088-165">Vägledningen innehåller en förenklad version av dessa anvisningar och är bäst för bevis på koncept projekt.</span><span class="sxs-lookup"><span data-stu-id="5d088-165">The tutorial contains a simplified version of these instructions and is best for proof of concept projects.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

<span data-ttu-id="5d088-166">I den *Kom igång* bladet under **skapa en tabell API**, Välj **Node.js** som din **språk för serverdel**.</span><span class="sxs-lookup"><span data-stu-id="5d088-166">Back in the *Get started* blade, under **Create a table API**, choose **Node.js** as your **Backend language**.</span></span>
<span data-ttu-id="5d088-167">Markera kryssrutan för ”**bekräftar jag att skrivs alla plats innehållet.**”, klicka på **skapa TodoItem-tabell**.</span><span class="sxs-lookup"><span data-stu-id="5d088-167">Check the box for "**I acknowledge that this will overwrite all site contents.**", then click **Create TodoItem table**.</span></span>

### <span data-ttu-id="5d088-168"><a name="download-quickstart"></a>Så här: ladda ned Node.js quickstart kod serverdelsprojektet med Git</span><span class="sxs-lookup"><span data-stu-id="5d088-168"><a name="download-quickstart"></a>How to: Download the Node.js backend quickstart code project using Git</span></span>
<span data-ttu-id="5d088-169">När du skapar en Node.js-mobilappsserverdel med hjälp av portalen **Snabbstart** bladet ett Node.js-projekt skapas för dig och distribueras till din webbplats.</span><span class="sxs-lookup"><span data-stu-id="5d088-169">When you create a Node.js Mobile App backend by using the portal **Quick start** blade, a Node.js project is created for you and deployed to your site.</span></span> <span data-ttu-id="5d088-170">Du kan lägga till tabeller och API: er och redigera kodfiler för Node.js-serverdel i portalen.</span><span class="sxs-lookup"><span data-stu-id="5d088-170">You can add tables and APIs and edit code files for the Node.js backend in the portal.</span></span> <span data-ttu-id="5d088-171">Du kan också använda olika distributionsverktyg för att hämta serverdelsprojektet så att du kan lägga till eller ändra tabeller och API: er och sedan publicera projektet.</span><span class="sxs-lookup"><span data-stu-id="5d088-171">You can also use various deployment tools to download the backend project so that you can add or modify tables and APIs, then republish the project.</span></span> <span data-ttu-id="5d088-172">Mer information finns i [Azure App Service Deployment Guide].</span><span class="sxs-lookup"><span data-stu-id="5d088-172">For more information, see the [Azure App Service Deployment Guide].</span></span> <span data-ttu-id="5d088-173">följande procedur används en Git-lagringsplats för att hämta Projektkod Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="5d088-173">the following procedure uses a Git repository to download the quickstart project code.</span></span>

1. <span data-ttu-id="5d088-174">Installera Git om du inte redan gjort det..</span><span class="sxs-lookup"><span data-stu-id="5d088-174">Install Git, if you haven't already done so.</span></span> <span data-ttu-id="5d088-175">Anvisningar för att installera Git variera mellan olika operativsystem.</span><span class="sxs-lookup"><span data-stu-id="5d088-175">The steps required to install Git vary between operating systems.</span></span> <span data-ttu-id="5d088-176">Se [installerar Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) för operativsystemspecifika distributioner och installationer.</span><span class="sxs-lookup"><span data-stu-id="5d088-176">See [Installing Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) for operating system-specific distributions and installation guidance.</span></span>
2. <span data-ttu-id="5d088-177">Följ stegen i [aktivera databasen för Apptjänst-app](../app-service-web/app-service-deploy-local-git.md#Step3) att aktivera Git-lagringsplats för backend-platsen, och ett meddelande om distributionen användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="5d088-177">Follow the steps in [Enable the App Service app repository](../app-service-web/app-service-deploy-local-git.md#Step3) to enable the Git repository for your backend site, making a note of the deployment username and password.</span></span>
3. <span data-ttu-id="5d088-178">I bladet för din mobilappsserverdel anteckna den **URL för Git-klon** inställningen.</span><span class="sxs-lookup"><span data-stu-id="5d088-178">In the blade for your Mobile App backend, make a note of the **Git clone URL** setting.</span></span>
4. <span data-ttu-id="5d088-179">Köra den `git clone` kommandot med Git klona URL, ange ditt lösenord vid behov, som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="5d088-179">Execute the `git clone` command using the Git clone URL, entering your password when required, as in the following example:</span></span>

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. <span data-ttu-id="5d088-180">Bläddra till lokal katalog, som i föregående exempel är /todolist, och Lägg märke till att projektfiler har laddats ned.</span><span class="sxs-lookup"><span data-stu-id="5d088-180">Browse to local directory, which in the preceding example is /todolist, and notice that project files have been downloaded.</span></span> <span data-ttu-id="5d088-181">Leta upp den `todoitem.json` filen i den `/tables` directory.</span><span class="sxs-lookup"><span data-stu-id="5d088-181">Locate the `todoitem.json` file in the `/tables` directory.</span></span>  <span data-ttu-id="5d088-182">Den här filen definierar behörigheter i tabellen.</span><span class="sxs-lookup"><span data-stu-id="5d088-182">This file defines permissions on the table.</span></span>  <span data-ttu-id="5d088-183">Också hitta den `todoitem.js` fil i samma katalog som definierar CRUD åtgärden skript för tabellen.</span><span class="sxs-lookup"><span data-stu-id="5d088-183">Also find the `todoitem.js` file in the same directory, which defines that CRUD operation scripts for the table.</span></span>
6. <span data-ttu-id="5d088-184">När du har gjort ändringar i projektfiler, kör du följande kommandon för att lägga till och genomför sedan överföra ändringarna till webbplatsen:</span><span class="sxs-lookup"><span data-stu-id="5d088-184">After you have made changes to project files, execute the following commands to add, commit, then upload the changes to the site:</span></span>

        $ git commit -m "updated the table script"
        $ git push origin master

    <span data-ttu-id="5d088-185">När du lägger till nya filer i projektet, måste du först köra den `git add .` kommando.</span><span class="sxs-lookup"><span data-stu-id="5d088-185">When you add new files to the project, you first need to execute the `git add .` command.</span></span>

<span data-ttu-id="5d088-186">Varje gång en ny uppsättning incheckningar skickas till platsen publiceras på webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="5d088-186">The site is republished every time a new set of commits is pushed to the site.</span></span>

### <span data-ttu-id="5d088-187"><a name="howto-publish-to-azure"></a>Så här: publicera din Node.js-serverdel i Azure</span><span class="sxs-lookup"><span data-stu-id="5d088-187"><a name="howto-publish-to-azure"></a>How to: Publish your Node.js backend to Azure</span></span>
<span data-ttu-id="5d088-188">Microsoft Azure tillhandahåller många metoder för att publicera dina Azure App Service Mobile Apps Node.js-serverdel till Azure-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5d088-188">Microsoft Azure provides many mechanisms for publishing your Azure App Service Mobile Apps Node.js backend to the Azure service.</span></span>  <span data-ttu-id="5d088-189">Dessa inkluderar använder distributionsverktyg integrerade i Visual Studio, kommandoradsverktyg och kontinuerlig distributionsalternativ baserat på källkontroll.</span><span class="sxs-lookup"><span data-stu-id="5d088-189">These include utilizing deployment tools integrated into Visual Studio, command-line tools, and continuous deployment options based on source control.</span></span>  <span data-ttu-id="5d088-190">Mer information om det här avsnittet finns det [Azure App Service Deployment Guide].</span><span class="sxs-lookup"><span data-stu-id="5d088-190">For more information on this topic, see the [Azure App Service Deployment Guide].</span></span>

<span data-ttu-id="5d088-191">Azure Apptjänst har råd för Node.js-program som du bör läsa innan du distribuerar:</span><span class="sxs-lookup"><span data-stu-id="5d088-191">Azure App Service has specific advice for Node.js application that you should review before deploying:</span></span>

* <span data-ttu-id="5d088-192">Så här [ange nod-Version]</span><span class="sxs-lookup"><span data-stu-id="5d088-192">How to [specify the Node Version]</span></span>
* <span data-ttu-id="5d088-193">Så här [använda noden moduler]</span><span class="sxs-lookup"><span data-stu-id="5d088-193">How to [use Node modules]</span></span>

### <span data-ttu-id="5d088-194"><a name="howto-enable-homepage"></a>Så här: aktivera en hemsida för ditt program</span><span class="sxs-lookup"><span data-stu-id="5d088-194"><a name="howto-enable-homepage"></a>How to: Enable a Home Page for your application</span></span>
<span data-ttu-id="5d088-195">Många program är en kombination av webb- och mobilappar och ExpressJS framework kan du kombinera de två aspekter.</span><span class="sxs-lookup"><span data-stu-id="5d088-195">Many applications are a combination of web and mobile apps and the ExpressJS framework allows you to combine the two facets.</span></span>  <span data-ttu-id="5d088-196">Ibland kan dock kan du bara implementera ett gränssnitt för mobila.</span><span class="sxs-lookup"><span data-stu-id="5d088-196">Sometimes, however, you may wish to only implement a mobile interface.</span></span>  <span data-ttu-id="5d088-197">Det är praktiskt att ge en landningssida för att undvika app-tjänsten är igång.</span><span class="sxs-lookup"><span data-stu-id="5d088-197">It is useful to provide a landing page to ensure the app service is up and running.</span></span>  <span data-ttu-id="5d088-198">Du kan ange en egen hemsida eller aktivera en tillfällig startsida.</span><span class="sxs-lookup"><span data-stu-id="5d088-198">You can either provide your own home page or enable a temporary home page.</span></span>  <span data-ttu-id="5d088-199">För att aktivera en tillfällig startsida, använda du följande för att skapa en instans av Azure Mobile Apps:</span><span class="sxs-lookup"><span data-stu-id="5d088-199">To enable a temporary home page, use the following to instantiate Azure Mobile Apps:</span></span>

    var mobile = azureMobileApps({ homePage: true });

<span data-ttu-id="5d088-200">Om du vill bara det här alternativet är tillgängligt när du utvecklar lokalt kan du lägga till den här inställningen för att din `azureMobile.js` fil.</span><span class="sxs-lookup"><span data-stu-id="5d088-200">If you only want this option available when developing locally, you can add this setting to your `azureMobile.js` file.</span></span>

## <span data-ttu-id="5d088-201"><a name="TableOperations"></a>Åtgärder för tabeller</span><span class="sxs-lookup"><span data-stu-id="5d088-201"><a name="TableOperations"></a>Table operations</span></span>
<span data-ttu-id="5d088-202">Node.js Server-SDK för azure mobilappar ger metoder för att visa datatabeller som lagras i Azure SQL Database som en WebAPI.</span><span class="sxs-lookup"><span data-stu-id="5d088-202">The azure-mobile-apps Node.js Server SDK provides mechanisms to expose data tables stored in Azure SQL Database as a WebAPI.</span></span>  <span data-ttu-id="5d088-203">Fem tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="5d088-203">Five operations are provided.</span></span>

| <span data-ttu-id="5d088-204">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="5d088-204">Operation</span></span> | <span data-ttu-id="5d088-205">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5d088-205">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5d088-206">GET-/tables/*tablename*</span><span class="sxs-lookup"><span data-stu-id="5d088-206">GET /tables/*tablename*</span></span> |<span data-ttu-id="5d088-207">Hämta alla poster i tabellen</span><span class="sxs-lookup"><span data-stu-id="5d088-207">Get all records in the table</span></span> |
| <span data-ttu-id="5d088-208">GET-/tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="5d088-208">GET /tables/*tablename*/:id</span></span> |<span data-ttu-id="5d088-209">Hämta en viss post i tabellen</span><span class="sxs-lookup"><span data-stu-id="5d088-209">Get a specific record in the table</span></span> |
| <span data-ttu-id="5d088-210">POST /tables/*tablename*</span><span class="sxs-lookup"><span data-stu-id="5d088-210">POST /tables/*tablename*</span></span> |<span data-ttu-id="5d088-211">Skapa en post i tabellen</span><span class="sxs-lookup"><span data-stu-id="5d088-211">Create a record in the table</span></span> |
| <span data-ttu-id="5d088-212">KORRIGERING /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="5d088-212">PATCH /tables/*tablename*/:id</span></span> |<span data-ttu-id="5d088-213">Uppdatera en post i tabellen</span><span class="sxs-lookup"><span data-stu-id="5d088-213">Update a record in the table</span></span> |
| <span data-ttu-id="5d088-214">Ta bort /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="5d088-214">DELETE /tables/*tablename*/:id</span></span> |<span data-ttu-id="5d088-215">Ta bort en post i tabellen</span><span class="sxs-lookup"><span data-stu-id="5d088-215">Delete a record in the table</span></span> |

<span data-ttu-id="5d088-216">Har stöd för den här WebAPI [OData] och utökar tabellschemat att stödja [datasynkronisering offline].</span><span class="sxs-lookup"><span data-stu-id="5d088-216">This WebAPI supports [OData] and extends the table schema to support [offline data sync].</span></span>

### <span data-ttu-id="5d088-217"><a name="howto-dynamicschema"></a>Så här: definiera tabeller med en dynamisk schema</span><span class="sxs-lookup"><span data-stu-id="5d088-217"><a name="howto-dynamicschema"></a>How to: Define tables using a dynamic schema</span></span>
<span data-ttu-id="5d088-218">Innan en tabell kan användas, måste ha definierats.</span><span class="sxs-lookup"><span data-stu-id="5d088-218">Before a table can be used, it must be defined.</span></span>  <span data-ttu-id="5d088-219">Tabeller kan definieras med en statisk schema (där definierar utvecklaren kolumner i schemat) eller dynamiskt (där SDK styr schema baserat på inkommande begäranden).</span><span class="sxs-lookup"><span data-stu-id="5d088-219">Tables can be defined with a static schema (where the developer defines the columns within the schema) or dynamically (where the SDK controls the schema based on incoming requests).</span></span> <span data-ttu-id="5d088-220">Utvecklaren kan dessutom styra vissa aspekter av WebAPI genom att lägga till Javascript-kod definitionen.</span><span class="sxs-lookup"><span data-stu-id="5d088-220">In addition, the developer can control specific aspects of the WebAPI by adding Javascript code to the definition.</span></span>

<span data-ttu-id="5d088-221">Som bästa praxis bör du definiera varje tabell i en Javascript-fil i katalogen tabeller och sedan använda tables.import()-metoden för att importera tabellerna.</span><span class="sxs-lookup"><span data-stu-id="5d088-221">As a best practice, you should define each table in a Javascript file in the tables directory, then use the tables.import() method to import the tables.</span></span>  <span data-ttu-id="5d088-222">Utöka appen basic skulle filen app.js ställas in:</span><span class="sxs-lookup"><span data-stu-id="5d088-222">Extending the basic-app, the app.js file would be adjusted:</span></span>

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

<span data-ttu-id="5d088-223">Definiera tabellen i-. / tables/TodoItem.js:</span><span class="sxs-lookup"><span data-stu-id="5d088-223">Define the table in ./tables/TodoItem.js:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

<span data-ttu-id="5d088-224">Dynamisk schemat använder som standard.</span><span class="sxs-lookup"><span data-stu-id="5d088-224">Tables use dynamic schema by default.</span></span>  <span data-ttu-id="5d088-225">Ange om du vill inaktivera dynamiska schemat globalt Appinställningen **MS_DynamicSchema** till FALSKT i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5d088-225">To turn off dynamic schema globally, set the App Setting **MS_DynamicSchema** to false within the Azure portal.</span></span>

<span data-ttu-id="5d088-226">Du hittar ett komplett exempel i den [todo prov på GitHub].</span><span class="sxs-lookup"><span data-stu-id="5d088-226">You can find a complete example in the [todo sample on GitHub].</span></span>

### <span data-ttu-id="5d088-227"><a name="howto-staticschema"></a>Så här: definiera tabeller med en statisk schema</span><span class="sxs-lookup"><span data-stu-id="5d088-227"><a name="howto-staticschema"></a>How to: Define tables using a static schema</span></span>
<span data-ttu-id="5d088-228">Du kan uttryckligen definiera kolumner att visa via WebAPI.</span><span class="sxs-lookup"><span data-stu-id="5d088-228">You can explicitly define the columns to expose via the WebAPI.</span></span>  <span data-ttu-id="5d088-229">Azure mobilappar Node.js SDK lägger automatiskt till alla andra kolumner som krävs för datasynkronisering offline i listan som du anger.</span><span class="sxs-lookup"><span data-stu-id="5d088-229">The azure-mobile-apps Node.js SDK automatically adds any additional columns required for offline data sync to the list that you provide.</span></span>  <span data-ttu-id="5d088-230">Exempelvis klientprogram Snabbstart kräver en tabell med två kolumner: text (en sträng) och slutföra (ett booleskt värde).</span><span class="sxs-lookup"><span data-stu-id="5d088-230">For example, the QuickStart client applications require a table with two columns: text (a string) and complete (a boolean).</span></span>  
<span data-ttu-id="5d088-231">Tabellen kan definieras i tabellen definition JavaScript-filen (som finns i katalogen tabeller) enligt följande:</span><span class="sxs-lookup"><span data-stu-id="5d088-231">The table can be defined in the table definition JavaScript file (located in the tables directory) as follows:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

<span data-ttu-id="5d088-232">Om du definierar tabeller statiskt, måste du också anropa metoden tables.initialize() för att skapa databasschemat vid start.</span><span class="sxs-lookup"><span data-stu-id="5d088-232">If you define tables statically, then you must also call the tables.initialize() method to create the database schema on startup.</span></span>  <span data-ttu-id="5d088-233">Tables.initialize()-metoden returnerar en [Promise] så att webbtjänsten inte hantera begäranden innan databasen initieras.</span><span class="sxs-lookup"><span data-stu-id="5d088-233">The tables.initialize() method returns a [Promise] so that the web service does not serve requests before the database being initialized.</span></span>

### <span data-ttu-id="5d088-234"><a name="howto-sqlexpress-setup"></a>Så här: använda SQL Express som en utveckling datalager på den lokala datorn</span><span class="sxs-lookup"><span data-stu-id="5d088-234"><a name="howto-sqlexpress-setup"></a>How to: Use SQL Express as a development data store on your local machine</span></span>
<span data-ttu-id="5d088-235">Azure Mobile Apps i AzureMobile appar nod SDK innehåller tre alternativ för betjänar data ur lådan: SDK innehåller tre alternativ för betjänar data ur lådan:</span><span class="sxs-lookup"><span data-stu-id="5d088-235">The Azure Mobile Apps The AzureMobile Apps Node SDK provides three options for serving data out of the box: SDK provides three options for serving data out of the box:</span></span>

* <span data-ttu-id="5d088-236">Använd den **minne** drivrutinen att tillhandahålla en icke-beständig exempel store</span><span class="sxs-lookup"><span data-stu-id="5d088-236">Use the **memory** driver to provide a non-persistent example store</span></span>
* <span data-ttu-id="5d088-237">Använd den **mssql** drivrutinen att tillhandahålla ett dataarkiv som SQL Express för utveckling</span><span class="sxs-lookup"><span data-stu-id="5d088-237">Use the **mssql** driver to provide a SQL Express data store for development</span></span>
* <span data-ttu-id="5d088-238">Använd den **mssql** drivrutinen att tillhandahålla en Azure SQL Database-datalagret för produktion</span><span class="sxs-lookup"><span data-stu-id="5d088-238">Use the **mssql** driver to provide an Azure SQL Database data store for production</span></span>

<span data-ttu-id="5d088-239">Azure Mobile Apps Node.js SDK använder den [mssql Node.js paketet] upprätta och använda en anslutning till SQL Express och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="5d088-239">The Azure Mobile Apps Node.js SDK uses the [mssql Node.js package] to establish and use a connection to both SQL Express and SQL Database.</span></span>  <span data-ttu-id="5d088-240">Det här paketet kräver att du aktiverar TCP-anslutningar på din SQL Express-instans.</span><span class="sxs-lookup"><span data-stu-id="5d088-240">This package requires that you enable TCP connections on your SQL Express instance.</span></span>

> [!TIP]
> <span data-ttu-id="5d088-241">Minne-drivrutinen ger inte en fullständig uppsättning funktioner för testning.</span><span class="sxs-lookup"><span data-stu-id="5d088-241">The memory driver does not provide a complete set of facilities for testing.</span></span>  <span data-ttu-id="5d088-242">Om du vill testa serverdelen lokalt rekommenderar vi användningen av ett SQL Express datalager och mssql-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="5d088-242">If you wish to test your backend locally, we recommend the use of a SQL Express data store and the mssql driver.</span></span>
>
>

1. <span data-ttu-id="5d088-243">Hämta och installera [Microsoft SQL Server 2014 Express].</span><span class="sxs-lookup"><span data-stu-id="5d088-243">Download and install [Microsoft SQL Server 2014 Express].</span></span>  <span data-ttu-id="5d088-244">Se till att du installerar SQL Server 2014 Express med verktyg edition.</span><span class="sxs-lookup"><span data-stu-id="5d088-244">Ensure you install the SQL Server 2014 Express with Tools edition.</span></span>  <span data-ttu-id="5d088-245">Om du inte uttryckligen behöver stöd för 64-bitars, förbrukar 32-bitarsversionen mindre minne när du kör.</span><span class="sxs-lookup"><span data-stu-id="5d088-245">Unless you explicitly require 64-bit support, the 32-bit version consumes less memory when running.</span></span>
2. <span data-ttu-id="5d088-246">Kör Konfigurationshanteraren för SQL Server 2014.</span><span class="sxs-lookup"><span data-stu-id="5d088-246">Run the SQL Server 2014 Configuration Manager.</span></span>

   1. <span data-ttu-id="5d088-247">Expandera den **SQL Server-nätverkskonfigurationen** nod i den vänstra träd-menyn.</span><span class="sxs-lookup"><span data-stu-id="5d088-247">Expand the **SQL Server Network Configuration** node in the left-hand tree menu.</span></span>
   2. <span data-ttu-id="5d088-248">Klicka på **protokoll för SQLEXPRESS**.</span><span class="sxs-lookup"><span data-stu-id="5d088-248">Click **Protocols for SQLEXPRESS**.</span></span>
   3. <span data-ttu-id="5d088-249">Högerklicka på **TCP/IP** och välj **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="5d088-249">Right-click **TCP/IP** and select **Enable**.</span></span>  <span data-ttu-id="5d088-250">Klicka på **OK** i popup-fönstret.</span><span class="sxs-lookup"><span data-stu-id="5d088-250">Click **OK** in the pop-up dialog.</span></span>
   4. <span data-ttu-id="5d088-251">Högerklicka på **TCP/IP** och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="5d088-251">Right-click **TCP/IP** and select **Properties**.</span></span>
   5. <span data-ttu-id="5d088-252">Klicka på den **IP-adresser** fliken.</span><span class="sxs-lookup"><span data-stu-id="5d088-252">Click the **IP Addresses** tab.</span></span>
   6. <span data-ttu-id="5d088-253">Hitta de **IPAll** nod.</span><span class="sxs-lookup"><span data-stu-id="5d088-253">Find the **IPAll** node.</span></span>  <span data-ttu-id="5d088-254">I den **TCP-Port** anger **1433**.</span><span class="sxs-lookup"><span data-stu-id="5d088-254">In the **TCP Port** field, enter **1433**.</span></span>

          ![Configure SQL Express for TCP/IP][3]
   7. <span data-ttu-id="5d088-255">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d088-255">Click **OK**.</span></span>  <span data-ttu-id="5d088-256">Klicka på **OK** i popup-fönstret.</span><span class="sxs-lookup"><span data-stu-id="5d088-256">Click **OK** in the pop-up dialog.</span></span>
   8. <span data-ttu-id="5d088-257">Klicka på **SQL Server Services** i den vänstra träd-menyn.</span><span class="sxs-lookup"><span data-stu-id="5d088-257">Click **SQL Server Services** in the left-hand tree menu.</span></span>
   9. <span data-ttu-id="5d088-258">Högerklicka på **SQL Server (SQLEXPRESS)** och välj **starta om**</span><span class="sxs-lookup"><span data-stu-id="5d088-258">Right-click **SQL Server (SQLEXPRESS)** and select **Restart**</span></span>
   10. <span data-ttu-id="5d088-259">Stäng Konfigurationshanteraren för SQL Server 2014.</span><span class="sxs-lookup"><span data-stu-id="5d088-259">Close the SQL Server 2014 Configuration Manager.</span></span>
3. <span data-ttu-id="5d088-260">Kör SQL Server 2014 Management Studio och ansluta till din lokala SQL Express-instans</span><span class="sxs-lookup"><span data-stu-id="5d088-260">Run the SQL Server 2014 Management Studio and connect to your local SQL Express instance</span></span>

   1. <span data-ttu-id="5d088-261">Högerklicka på din instans i Object Explorer och välj **egenskaper**</span><span class="sxs-lookup"><span data-stu-id="5d088-261">Right-click your instance in the Object Explorer and select **Properties**</span></span>
   2. <span data-ttu-id="5d088-262">Välj den **säkerhet** sidan.</span><span class="sxs-lookup"><span data-stu-id="5d088-262">Select the **Security** page.</span></span>
   3. <span data-ttu-id="5d088-263">Se till att den **SQL Server och Windows-autentisering läge** har valts</span><span class="sxs-lookup"><span data-stu-id="5d088-263">Ensure the **SQL Server and Windows Authentication mode** is selected</span></span>
   4. <span data-ttu-id="5d088-264">Klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="5d088-264">Click **OK**</span></span>

          ![Configure SQL Express Authentication][4]
   5. <span data-ttu-id="5d088-265">Expandera **säkerhet** > **inloggningar** i Object Explorer</span><span class="sxs-lookup"><span data-stu-id="5d088-265">Expand **Security** > **Logins** in the Object Explorer</span></span>
   6. <span data-ttu-id="5d088-266">Högerklicka på **inloggningar** och välj **ny inloggning...**</span><span class="sxs-lookup"><span data-stu-id="5d088-266">Right-click **Logins** and select **New Login...**</span></span>
   7. <span data-ttu-id="5d088-267">Ange ett inloggningsnamn.</span><span class="sxs-lookup"><span data-stu-id="5d088-267">Enter a Login name.</span></span>  <span data-ttu-id="5d088-268">Välj **SQL Server-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="5d088-268">Select **SQL Server authentication**.</span></span>  <span data-ttu-id="5d088-269">Ange ett lösenord och sedan ange samma lösenord i **Bekräfta lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5d088-269">Enter a Password, then enter the same password in **Confirm password**.</span></span>  <span data-ttu-id="5d088-270">Lösenordet måste uppfylla krav på komplexitet Windows.</span><span class="sxs-lookup"><span data-stu-id="5d088-270">The password must meet Windows complexity requirements.</span></span>
   8. <span data-ttu-id="5d088-271">Klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="5d088-271">Click **OK**</span></span>

          ![Add a new user to SQL Express][5]
   9. <span data-ttu-id="5d088-272">Högerklicka på en ny inloggning och välj **egenskaper**</span><span class="sxs-lookup"><span data-stu-id="5d088-272">Right-click your new login and select **Properties**</span></span>
   10. <span data-ttu-id="5d088-273">Välj den **serverroller** sidan</span><span class="sxs-lookup"><span data-stu-id="5d088-273">Select the **Server Roles** page</span></span>
   11. <span data-ttu-id="5d088-274">Markera kryssrutan bredvid den **dbcreator** serverroll</span><span class="sxs-lookup"><span data-stu-id="5d088-274">Check the box next to the **dbcreator** server role</span></span>
   12. <span data-ttu-id="5d088-275">Klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="5d088-275">Click **OK**</span></span>
   13. <span data-ttu-id="5d088-276">Stäng SQL Server 2015 Management Studio</span><span class="sxs-lookup"><span data-stu-id="5d088-276">Close the SQL Server 2015 Management Studio</span></span>

<span data-ttu-id="5d088-277">Se till att du anteckna användarnamnet och lösenordet som du har valt.</span><span class="sxs-lookup"><span data-stu-id="5d088-277">Ensure you record the username and password you selected.</span></span>  <span data-ttu-id="5d088-278">Du kan behöva tilldela ytterligare serverroller eller behörigheter beroende på dina specifika krav.</span><span class="sxs-lookup"><span data-stu-id="5d088-278">You may need to assign additional server roles or permissions depending on your specific database requirements.</span></span>

<span data-ttu-id="5d088-279">Node.js-program läser den **SQLCONNSTR_MS_TableConnectionString** miljövariabeln för anslutningssträngen för den här databasen.</span><span class="sxs-lookup"><span data-stu-id="5d088-279">The Node.js application reads the **SQLCONNSTR_MS_TableConnectionString** environment variable for the connection string for this database.</span></span>  <span data-ttu-id="5d088-280">Du kan ange den här variabeln i din miljö.</span><span class="sxs-lookup"><span data-stu-id="5d088-280">You can set this variable within your environment.</span></span>  <span data-ttu-id="5d088-281">Du kan till exempel använda PowerShell för att ange den här miljövariabeln:</span><span class="sxs-lookup"><span data-stu-id="5d088-281">For example, you can use PowerShell to set this environment variable:</span></span>

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

<span data-ttu-id="5d088-282">Åtkomst till databasen via en TCP/IP-anslutning och ange ett användarnamn och lösenord för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="5d088-282">Access the database through a TCP/IP connection and provide a username and password for the connection.</span></span>

### <span data-ttu-id="5d088-283"><a name="howto-config-localdev"></a>Så här: konfigurera ditt projekt för lokal utveckling</span><span class="sxs-lookup"><span data-stu-id="5d088-283"><a name="howto-config-localdev"></a>How to: Configure your project for local development</span></span>
<span data-ttu-id="5d088-284">Azure Mobile Apps läser en JavaScript-fil som heter *azureMobile.js* från det lokala filsystemet.</span><span class="sxs-lookup"><span data-stu-id="5d088-284">Azure Mobile Apps reads a JavaScript file called *azureMobile.js* from the local filesystem.</span></span>  <span data-ttu-id="5d088-285">Använd inte den här filen för att konfigurera Azure Mobile Apps-SDK i produktion - App-inställningar i den [Azure-portalen] i stället.</span><span class="sxs-lookup"><span data-stu-id="5d088-285">Do not use this file to configure the Azure Mobile Apps SDK in production - use App Settings within the [Azure portal] instead.</span></span>  <span data-ttu-id="5d088-286">Den *azureMobile.js* filen ska exportera ett konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="5d088-286">The *azureMobile.js* file should export a configuration object.</span></span>  <span data-ttu-id="5d088-287">De vanligaste inställningarna är:</span><span class="sxs-lookup"><span data-stu-id="5d088-287">The most common settings are:</span></span>

* <span data-ttu-id="5d088-288">Databasinställningar</span><span class="sxs-lookup"><span data-stu-id="5d088-288">Database Settings</span></span>
* <span data-ttu-id="5d088-289">Inställningar för diagnostikloggning</span><span class="sxs-lookup"><span data-stu-id="5d088-289">Diagnostic Logging Settings</span></span>
* <span data-ttu-id="5d088-290">Alternativa CORS-inställningarna</span><span class="sxs-lookup"><span data-stu-id="5d088-290">Alternate CORS Settings</span></span>

<span data-ttu-id="5d088-291">Ett exempel *azureMobile.js* filen implementera föregående databasinställningarna följer:</span><span class="sxs-lookup"><span data-stu-id="5d088-291">An example *azureMobile.js* file implementing the preceding database settings follows:</span></span>

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

<span data-ttu-id="5d088-292">Vi rekommenderar att du lägger till *azureMobile.js* till din *.gitignore* fil (eller andra källkodskontroll ignorera filen) till att förhindra att lösenord lagras i molnet.</span><span class="sxs-lookup"><span data-stu-id="5d088-292">We recommend that you add *azureMobile.js* to your *.gitignore* file (or other source code control ignore file) to prevent passwords from being stored in the cloud.</span></span>  <span data-ttu-id="5d088-293">Konfigurera inställningar för produktion alltid i App-inställningar i den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="5d088-293">Always configure production settings in App Settings within the [Azure portal].</span></span>

### <span data-ttu-id="5d088-294"><a name="howto-appsettings"></a>Så här: Konfigurera Appinställningar för din mobila App</span><span class="sxs-lookup"><span data-stu-id="5d088-294"><a name="howto-appsettings"></a>How: Configure App Settings for your Mobile App</span></span>
<span data-ttu-id="5d088-295">De flesta inställningar i den *azureMobile.js* filen har en motsvarande Appinställning den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="5d088-295">Most settings in the *azureMobile.js* file have an equivalent App Setting in the [Azure portal].</span></span>  <span data-ttu-id="5d088-296">Använd listan nedan för att konfigurera din app i App-inställningar:</span><span class="sxs-lookup"><span data-stu-id="5d088-296">Use the following list to configure your app in App Settings:</span></span>

| <span data-ttu-id="5d088-297">Appinställningen</span><span class="sxs-lookup"><span data-stu-id="5d088-297">App Setting</span></span> | <span data-ttu-id="5d088-298">*azureMobile.js* inställning</span><span class="sxs-lookup"><span data-stu-id="5d088-298">*azureMobile.js* Setting</span></span> | <span data-ttu-id="5d088-299">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5d088-299">Description</span></span> | <span data-ttu-id="5d088-300">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="5d088-300">Valid Values</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="5d088-301">**MS_MobileAppName**</span><span class="sxs-lookup"><span data-stu-id="5d088-301">**MS_MobileAppName**</span></span> |<span data-ttu-id="5d088-302">namn</span><span class="sxs-lookup"><span data-stu-id="5d088-302">name</span></span> |<span data-ttu-id="5d088-303">Namnet på appen</span><span class="sxs-lookup"><span data-stu-id="5d088-303">The name of the app</span></span> |<span data-ttu-id="5d088-304">Sträng</span><span class="sxs-lookup"><span data-stu-id="5d088-304">string</span></span> |
| <span data-ttu-id="5d088-305">**MS_MobileLoggingLevel**</span><span class="sxs-lookup"><span data-stu-id="5d088-305">**MS_MobileLoggingLevel**</span></span> |<span data-ttu-id="5d088-306">Logging.level</span><span class="sxs-lookup"><span data-stu-id="5d088-306">logging.level</span></span> |<span data-ttu-id="5d088-307">Lägsta loggningsnivån för meddelanden att logga</span><span class="sxs-lookup"><span data-stu-id="5d088-307">Minimum log level of messages to log</span></span> |<span data-ttu-id="5d088-308">fel, varning, information, verbose,-debug, Löjliga</span><span class="sxs-lookup"><span data-stu-id="5d088-308">error, warning, info, verbose, debug, silly</span></span> |
| <span data-ttu-id="5d088-309">**MS_DebugMode**</span><span class="sxs-lookup"><span data-stu-id="5d088-309">**MS_DebugMode**</span></span> |<span data-ttu-id="5d088-310">Felsöka</span><span class="sxs-lookup"><span data-stu-id="5d088-310">debug</span></span> |<span data-ttu-id="5d088-311">Aktivera eller inaktivera felsökningsläge</span><span class="sxs-lookup"><span data-stu-id="5d088-311">Enable or Disable debug mode</span></span> |<span data-ttu-id="5d088-312">SANT, FALSKT</span><span class="sxs-lookup"><span data-stu-id="5d088-312">true, false</span></span> |
| <span data-ttu-id="5d088-313">**MS_TableSchema**</span><span class="sxs-lookup"><span data-stu-id="5d088-313">**MS_TableSchema**</span></span> |<span data-ttu-id="5d088-314">data.schema</span><span class="sxs-lookup"><span data-stu-id="5d088-314">data.schema</span></span> |<span data-ttu-id="5d088-315">Schemat för standardnamnet för SQL-tabeller</span><span class="sxs-lookup"><span data-stu-id="5d088-315">Default schema name for SQL tables</span></span> |<span data-ttu-id="5d088-316">String (standard: dbo)</span><span class="sxs-lookup"><span data-stu-id="5d088-316">string (default: dbo)</span></span> |
| <span data-ttu-id="5d088-317">**MS_DynamicSchema**</span><span class="sxs-lookup"><span data-stu-id="5d088-317">**MS_DynamicSchema**</span></span> |<span data-ttu-id="5d088-318">data.dynamicSchema</span><span class="sxs-lookup"><span data-stu-id="5d088-318">data.dynamicSchema</span></span> |<span data-ttu-id="5d088-319">Aktivera eller inaktivera felsökningsläge</span><span class="sxs-lookup"><span data-stu-id="5d088-319">Enable or Disable debug mode</span></span> |<span data-ttu-id="5d088-320">SANT, FALSKT</span><span class="sxs-lookup"><span data-stu-id="5d088-320">true, false</span></span> |
| <span data-ttu-id="5d088-321">**MS_DisableVersionHeader**</span><span class="sxs-lookup"><span data-stu-id="5d088-321">**MS_DisableVersionHeader**</span></span> |<span data-ttu-id="5d088-322">(Ange till odefinierad) version</span><span class="sxs-lookup"><span data-stu-id="5d088-322">version (set to undefined)</span></span> |<span data-ttu-id="5d088-323">Inaktiverar huvudet X-ZUMO-Server-Version</span><span class="sxs-lookup"><span data-stu-id="5d088-323">Disables the X-ZUMO-Server-Version header</span></span> |<span data-ttu-id="5d088-324">SANT, FALSKT</span><span class="sxs-lookup"><span data-stu-id="5d088-324">true, false</span></span> |
| <span data-ttu-id="5d088-325">**MS_SkipVersionCheck**</span><span class="sxs-lookup"><span data-stu-id="5d088-325">**MS_SkipVersionCheck**</span></span> |<span data-ttu-id="5d088-326">skipversioncheck</span><span class="sxs-lookup"><span data-stu-id="5d088-326">skipversioncheck</span></span> |<span data-ttu-id="5d088-327">Inaktiverar version klientkontrollen API</span><span class="sxs-lookup"><span data-stu-id="5d088-327">Disables the client API version check</span></span> |<span data-ttu-id="5d088-328">SANT, FALSKT</span><span class="sxs-lookup"><span data-stu-id="5d088-328">true, false</span></span> |

<span data-ttu-id="5d088-329">Ange en inställning för appen:</span><span class="sxs-lookup"><span data-stu-id="5d088-329">To set an App Setting:</span></span>

1. <span data-ttu-id="5d088-330">Logga in på [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="5d088-330">Log in to the [Azure portal].</span></span>
2. <span data-ttu-id="5d088-331">Välj **alla resurser** eller **Apptjänster** klicka sedan på namnet på din Mobilapp.</span><span class="sxs-lookup"><span data-stu-id="5d088-331">Select **All resources** or **App Services** then click the name of your Mobile App.</span></span>
3. <span data-ttu-id="5d088-332">Inställningsbladet öppnas som standard.</span><span class="sxs-lookup"><span data-stu-id="5d088-332">The Settings blade opens by default.</span></span> <span data-ttu-id="5d088-333">Om den inte, klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="5d088-333">If it doesn't, click **Settings**.</span></span>
4. <span data-ttu-id="5d088-334">Klicka på **programinställningar** i menyn Allmänt.</span><span class="sxs-lookup"><span data-stu-id="5d088-334">Click **Application settings** in the GENERAL menu.</span></span>
5. <span data-ttu-id="5d088-335">Rulla ned till avsnittet App-inställningar.</span><span class="sxs-lookup"><span data-stu-id="5d088-335">Scroll to the App Settings section.</span></span>
6. <span data-ttu-id="5d088-336">Om din app inställningen redan finns, klickar du på värdet för appinställningen redigera värdet.</span><span class="sxs-lookup"><span data-stu-id="5d088-336">If your app setting already exists, click the value of the app setting to edit the value.</span></span>
7. <span data-ttu-id="5d088-337">Om din appinställning inte finns, anger du Appinställningen i rutan nyckel och värde i rutan värde.</span><span class="sxs-lookup"><span data-stu-id="5d088-337">If your app setting does not exist, enter the App Setting in the Key box and the value in the Value box.</span></span>
8. <span data-ttu-id="5d088-338">När du är klar klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="5d088-338">Once you are complete, click **Save**.</span></span>

<span data-ttu-id="5d088-339">Ändrar appinställningar för de flesta måste tjänsten startas om.</span><span class="sxs-lookup"><span data-stu-id="5d088-339">Changing most app settings requires a service restart.</span></span>

### <span data-ttu-id="5d088-340"><a name="howto-use-sqlazure"></a>Så här: använda SQL-databas som datalager produktion</span><span class="sxs-lookup"><span data-stu-id="5d088-340"><a name="howto-use-sqlazure"></a>How to: Use SQL Database as your production data store</span></span>
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

<span data-ttu-id="5d088-341">Med Azure SQL Database som ett datalager är identisk för alla typer i Azure App Service-programmet.</span><span class="sxs-lookup"><span data-stu-id="5d088-341">Using Azure SQL Database as a data store is identical across all Azure App Service application types.</span></span> <span data-ttu-id="5d088-342">Om du redan inte har gjort det, Följ dessa steg om du vill skapa en mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="5d088-342">If you have not done so already, follow these steps to create a Mobile App backend.</span></span>

1. <span data-ttu-id="5d088-343">Logga in på [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="5d088-343">Log in to the [Azure portal].</span></span>
2. <span data-ttu-id="5d088-344">Längst upp till vänster i fönstret, klickar du på den **+ ny** knappen > **webb + mobilt** > **Mobilapp**, ange ett namn för din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="5d088-344">In the top left of the window, click the **+NEW** button > **Web + Mobile** > **Mobile App**, then provide a name for your Mobile App backend.</span></span>
3. <span data-ttu-id="5d088-345">I den **resursgruppen** ange samma namn som din app.</span><span class="sxs-lookup"><span data-stu-id="5d088-345">In the **Resource Group** box, enter the same name as your app.</span></span>
4. <span data-ttu-id="5d088-346">Standard programtjänstplanen har valts.</span><span class="sxs-lookup"><span data-stu-id="5d088-346">The Default App Service plan is selected.</span></span>  <span data-ttu-id="5d088-347">Om du vill ändra din App Service-plan kan du göra det genom att klicka på Apptjänstplan > **+ skapa nya**.</span><span class="sxs-lookup"><span data-stu-id="5d088-347">If you wish to change your App Service plan, you can do so by clicking the App Service Plan > **+ Create New**.</span></span>  <span data-ttu-id="5d088-348">Ange ett namn för den nya programtjänstplanen och välj en lämplig plats.</span><span class="sxs-lookup"><span data-stu-id="5d088-348">Provide a name of the new App Service plan and select an appropriate location.</span></span>  <span data-ttu-id="5d088-349">Klicka på prisnivå och välj en lämplig prisnivån för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5d088-349">Click the Pricing tier and select an appropriate pricing tier for the service.</span></span> <span data-ttu-id="5d088-350">Välj **visa alla** att visa priser fler alternativ, exempelvis **lediga** och **delade**.</span><span class="sxs-lookup"><span data-stu-id="5d088-350">Select **View all** to view more pricing options, such as **Free** and **Shared**.</span></span>  <span data-ttu-id="5d088-351">När du har valt prisnivå, klickar du på den **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="5d088-351">Once you have selected the pricing tier, click the **Select** button.</span></span>  <span data-ttu-id="5d088-352">I den **programtjänstplanen** bladet, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d088-352">Back in the **App Service plan** blade, click **OK**.</span></span>
5. <span data-ttu-id="5d088-353">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5d088-353">Click **Create**.</span></span> <span data-ttu-id="5d088-354">Etablering av en mobilappsserverdel kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="5d088-354">Provisioning a Mobile App backend can take a couple of minutes.</span></span>  <span data-ttu-id="5d088-355">När serverdelen för Mobilappen etableras portalen öppnar den **inställningar** bladet för serverdelen för Mobilappen.</span><span class="sxs-lookup"><span data-stu-id="5d088-355">Once the Mobile App backend is provisioned, the portal opens the **Settings** blade for the Mobile App backend.</span></span>

<span data-ttu-id="5d088-356">När du har skapat serverdelen för Mobilappen kan du ansluta en befintlig SQL-databas till din mobilappsserverdel eller skapa en ny SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="5d088-356">Once the Mobile App backend is created, you can choose to either connect an existing SQL database to your Mobile App backend or create a new SQL database.</span></span>  <span data-ttu-id="5d088-357">I det här avsnittet skapar vi en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="5d088-357">In this section, we create a SQL database.</span></span>

> [!NOTE]
> <span data-ttu-id="5d088-358">Om du redan har en databas på samma plats som serverdelen för mobilappen kan du i stället väljer **Använd en befintlig databas** och välj sedan den databasen.</span><span class="sxs-lookup"><span data-stu-id="5d088-358">If you already have a database in the same location as the mobile app backend, you can instead choose **Use an existing database** and then select that database.</span></span> <span data-ttu-id="5d088-359">Det rekommenderas inte användning av en databas på en annan plats på grund av ökad latens.</span><span class="sxs-lookup"><span data-stu-id="5d088-359">The use of a database in a different location is not recommended because of higher latencies.</span></span>
>
>

1. <span data-ttu-id="5d088-360">Klicka på ny mobilappsserverdel och **inställningar** > **Mobilapp** > **Data** > **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5d088-360">In the new Mobile App backend, click **Settings** > **Mobile App** > **Data** > **+Add**.</span></span>
2. <span data-ttu-id="5d088-361">I den **Lägg till dataanslutning** bladet, klickar du på **SQL Database - konfigurera nödvändiga inställningar** > **skapa en ny databas**.</span><span class="sxs-lookup"><span data-stu-id="5d088-361">In the **Add data connection** blade, click **SQL Database - Configure required settings** > **Create a new database**.</span></span>  <span data-ttu-id="5d088-362">Ange namnet på den nya databasen i den **namn** fältet.</span><span class="sxs-lookup"><span data-stu-id="5d088-362">Enter the name of the new database in the **Name** field.</span></span>
3. <span data-ttu-id="5d088-363">Klicka på **Server**.</span><span class="sxs-lookup"><span data-stu-id="5d088-363">Click **Server**.</span></span>  <span data-ttu-id="5d088-364">I den **ny server** bladet, ange ett unikt servernamn i den **servernamn** fältet och ange ett lämpligt **inloggning för serveradministratör** och **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5d088-364">In the **New server** blade, enter a unique server name in the **Server name** field, and provide a suitable **Server admin login** and **Password**.</span></span>  <span data-ttu-id="5d088-365">Se till att **ge azure-tjänster åtkomst till servern** är markerad.</span><span class="sxs-lookup"><span data-stu-id="5d088-365">Ensure **Allow azure services to access server** is checked.</span></span>  <span data-ttu-id="5d088-366">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d088-366">Click **OK**.</span></span>

    ![Skapa en Azure SQL Database][6]
4. <span data-ttu-id="5d088-368">På den **ny databas** bladet, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d088-368">On the **New database** blade, click **OK**.</span></span>
5. <span data-ttu-id="5d088-369">Tillbaka på den **Lägg till dataanslutning** bladet väljer **anslutningssträngen**, anger inloggningsnamn och lösenord som du angav när du skapar databasen.</span><span class="sxs-lookup"><span data-stu-id="5d088-369">Back on the **Add data connection** blade, select **Connection string**, enter the login and password that you provided when creating the database.</span></span>  <span data-ttu-id="5d088-370">Ange inloggningsuppgifterna för databasen om du använder en befintlig databas.</span><span class="sxs-lookup"><span data-stu-id="5d088-370">If you use an existing database, provide the login credentials for that database.</span></span>  <span data-ttu-id="5d088-371">När du har angett, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d088-371">Once entered, click **OK**.</span></span>
6. <span data-ttu-id="5d088-372">Tillbaka på den **Lägg till dataanslutning** bladet Klicka på **OK** att skapa databasen.</span><span class="sxs-lookup"><span data-stu-id="5d088-372">Back on the **Add data connection** blade again, click **OK** to create the database.</span></span>

<!--- END OF ALTERNATE INCLUDE -->

<span data-ttu-id="5d088-373">Skapa databasen kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="5d088-373">Creation of the database can take a few minutes.</span></span>  <span data-ttu-id="5d088-374">Använd den **meddelanden** område för att övervaka förloppet för distributionen.</span><span class="sxs-lookup"><span data-stu-id="5d088-374">Use the **Notifications** area to monitor the progress of the deployment.</span></span>  <span data-ttu-id="5d088-375">Passera inte förrän databasen har distribuerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="5d088-375">Do not progress until the database has been deployed successfully.</span></span>  <span data-ttu-id="5d088-376">När har distribuerats, skapas en anslutningssträng för SQL Database-instans i din mobila serverdel App-inställningar.</span><span class="sxs-lookup"><span data-stu-id="5d088-376">Once successfully deployed, a Connection String is created for the SQL Database instance in your Mobile backend App Settings.</span></span>  <span data-ttu-id="5d088-377">Du kan se den här appen inställningen på den **inställningar** > **programinställningar** > **anslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="5d088-377">You can see this app setting in the **Settings** > **Application settings** > **Connection strings**.</span></span>

### <span data-ttu-id="5d088-378"><a name="howto-tables-auth"></a>Så här: kräver autentisering för åtkomst till tabeller</span><span class="sxs-lookup"><span data-stu-id="5d088-378"><a name="howto-tables-auth"></a>How to: Require authentication for access to tables</span></span>
<span data-ttu-id="5d088-379">Om du vill använda autentisering för App Service med tabeller slutpunkten måste du konfigurera App Service-autentisering i den [Azure-portalen] första.</span><span class="sxs-lookup"><span data-stu-id="5d088-379">If you wish to use App Service Authentication with the tables endpoint, you must configure App Service Authentication in the [Azure portal] first.</span></span>  <span data-ttu-id="5d088-380">Mer information om hur du konfigurerar autentisering i en Azure App Service granska i konfigurationsguiden för identitetsleverantör som du tänker använda:</span><span class="sxs-lookup"><span data-stu-id="5d088-380">For more details about configuring authentication in an Azure App Service, review the Configuration Guide for the identity provider you intend to use:</span></span>

* <span data-ttu-id="5d088-381">[Så här konfigurerar du Azure Active Directory-autentisering]</span><span class="sxs-lookup"><span data-stu-id="5d088-381">[How to configure Azure Active Directory Authentication]</span></span>
* <span data-ttu-id="5d088-382">[Hur du konfigurerar Facebook-autentisering]</span><span class="sxs-lookup"><span data-stu-id="5d088-382">[How to configure Facebook Authentication]</span></span>
* <span data-ttu-id="5d088-383">[Så här konfigurerar du autentisering med Google]</span><span class="sxs-lookup"><span data-stu-id="5d088-383">[How to configure Google Authentication]</span></span>
* <span data-ttu-id="5d088-384">[Så här konfigurerar du Microsoft Authentication]</span><span class="sxs-lookup"><span data-stu-id="5d088-384">[How to configure Microsoft Authentication]</span></span>
* <span data-ttu-id="5d088-385">[Hur du konfigurerar Twitter-autentisering]</span><span class="sxs-lookup"><span data-stu-id="5d088-385">[How to configure Twitter Authentication]</span></span>

<span data-ttu-id="5d088-386">Varje tabell har en åtkomst-egenskap som kan användas för att styra åtkomsten till tabellen.</span><span class="sxs-lookup"><span data-stu-id="5d088-386">Each table has an access property that can be used to control access to the table.</span></span>  <span data-ttu-id="5d088-387">I följande exempel visas en statiskt definierade tabell med autentisering krävs.</span><span class="sxs-lookup"><span data-stu-id="5d088-387">The following sample shows a statically defined table with authentication required.</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="5d088-388">Åtkomst-egenskapen kan ha ett av tre värden</span><span class="sxs-lookup"><span data-stu-id="5d088-388">The access property can take one of three values</span></span>

* <span data-ttu-id="5d088-389">*anonym* indikerar att klientprogrammet har tillåtelse att läsa data utan autentisering</span><span class="sxs-lookup"><span data-stu-id="5d088-389">*anonymous* indicates that the client application is allowed to read data without authentication</span></span>
* <span data-ttu-id="5d088-390">*autentiserad* indikerar att klientprogrammet måste skicka en giltig autentiserings-token med begäran</span><span class="sxs-lookup"><span data-stu-id="5d088-390">*authenticated* indicates that the client application must send a valid authentication token with the request</span></span>
* <span data-ttu-id="5d088-391">*inaktiverad* anger att den här tabellen är för närvarande inaktiverat</span><span class="sxs-lookup"><span data-stu-id="5d088-391">*disabled* indicates that this table is currently disabled</span></span>

<span data-ttu-id="5d088-392">Om åtkomst-egenskapen är odefinierad tillåts obehörig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="5d088-392">If the access property is undefined, unauthenticated access is allowed.</span></span>

### <span data-ttu-id="5d088-393"><a name="howto-tables-getidentity"></a>Så här: använda anspråk för autentisering med tabeller</span><span class="sxs-lookup"><span data-stu-id="5d088-393"><a name="howto-tables-getidentity"></a>How to: Use authentication claims with your tables</span></span>
<span data-ttu-id="5d088-394">Du kan ställa in olika anspråk som begärs när autentisering har ställts in.</span><span class="sxs-lookup"><span data-stu-id="5d088-394">You can set up various claims that are requested when authentication is set up.</span></span>  <span data-ttu-id="5d088-395">Dessa anspråk inte är normalt tillgängliga via den `context.user` objekt.</span><span class="sxs-lookup"><span data-stu-id="5d088-395">These claims are not normally available through the `context.user` object.</span></span>  <span data-ttu-id="5d088-396">Men de kan hämtas med hjälp av den `context.user.getIdentity()` metoden.</span><span class="sxs-lookup"><span data-stu-id="5d088-396">However, they can be retrieved using the `context.user.getIdentity()` method.</span></span>  <span data-ttu-id="5d088-397">Den `getIdentity()` metoden returnerar en Promise som matchar ett objekt.</span><span class="sxs-lookup"><span data-stu-id="5d088-397">The `getIdentity()` method returns a Promise that resolves to an object.</span></span>  <span data-ttu-id="5d088-398">Objektet aktiveras av autentiseringsmetod (facebook, google, twitter, microsoftaccount eller aad).</span><span class="sxs-lookup"><span data-stu-id="5d088-398">The object is keyed by the authentication method (facebook, google, twitter, microsoftaccount, or aad).</span></span>

<span data-ttu-id="5d088-399">Om du konfigurerar Account autentisering och begär anspråk på e-postadresser du lägga till e-postadressen till posten med följande tabell domänkontrollant:</span><span class="sxs-lookup"><span data-stu-id="5d088-399">For example, if you set up Microsoft Account authentication and request the email addresses claim, you can add the email address to the record with the following table controller:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

<span data-ttu-id="5d088-400">Om du vill se vilka anspråk som är tillgängliga, kan du använda en webbläsare för att visa den `/.auth/me` slutpunkten för din webbplats.</span><span class="sxs-lookup"><span data-stu-id="5d088-400">To see what claims are available, use a web browser to view the `/.auth/me` endpoint of your site.</span></span>

### <span data-ttu-id="5d088-401"><a name="howto-tables-disabled"></a>Så här: inaktivera åtkomst till specifika tabellåtgärder</span><span class="sxs-lookup"><span data-stu-id="5d088-401"><a name="howto-tables-disabled"></a>How to: Disable access to specific table operations</span></span>
<span data-ttu-id="5d088-402">Förutom som visas i tabellen, kan egenskapen åtkomst användas för att kontrollera enskilda operationer.</span><span class="sxs-lookup"><span data-stu-id="5d088-402">In addition to appearing on the table, the access property can be used to control individual operations.</span></span>  <span data-ttu-id="5d088-403">Det finns fyra åtgärder:</span><span class="sxs-lookup"><span data-stu-id="5d088-403">There are four operations:</span></span>

* <span data-ttu-id="5d088-404">*läsa* är RESTful GET-åtgärd på tabellen</span><span class="sxs-lookup"><span data-stu-id="5d088-404">*read* is the RESTful GET operation on the table</span></span>
* <span data-ttu-id="5d088-405">*Infoga* är RESTful POST-åtgärden för tabellen</span><span class="sxs-lookup"><span data-stu-id="5d088-405">*insert* is the RESTful POST operation on the table</span></span>
* <span data-ttu-id="5d088-406">*Uppdatera* korrigering av RESTful-åtgärd på tabellen</span><span class="sxs-lookup"><span data-stu-id="5d088-406">*update* is the RESTful PATCH operation on the table</span></span>
* <span data-ttu-id="5d088-407">*ta bort* är RESTful ta bort-åtgärden för tabellen</span><span class="sxs-lookup"><span data-stu-id="5d088-407">*delete* is the RESTful DELETE operation on the table</span></span>

<span data-ttu-id="5d088-408">Exempelvis kanske du vill ange en skrivskyddad oautentiserade tabell:</span><span class="sxs-lookup"><span data-stu-id="5d088-408">For example, you may wish to provide a read-only unauthenticated table:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <span data-ttu-id="5d088-409"><a name="howto-tables-query"></a>Så här: justera frågan som används med åtgärder för tabeller</span><span class="sxs-lookup"><span data-stu-id="5d088-409"><a name="howto-tables-query"></a>How to: Adjust the query that is used with table operations</span></span>
<span data-ttu-id="5d088-410">Ett vanligt krav för tabellåtgärder är att tillhandahålla en begränsad vy av data.</span><span class="sxs-lookup"><span data-stu-id="5d088-410">A common requirement for table operations is to provide a restricted view of the data.</span></span>  <span data-ttu-id="5d088-411">Du kan till exempel ange en tabell som är märkta med autentiserat användar-ID så att du kan bara läsa eller uppdatera dina egna poster.</span><span class="sxs-lookup"><span data-stu-id="5d088-411">For example, you may provide a table that is tagged with the authenticated user ID such that you can only read or update your own records.</span></span>  <span data-ttu-id="5d088-412">Följande tabelldefinitionen innehåller den här funktionen:</span><span class="sxs-lookup"><span data-stu-id="5d088-412">The following table definition provides this functionality:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

<span data-ttu-id="5d088-413">Åtgärder som normalt köra en fråga har en fråga-egenskap som du kan justera med where satsen.</span><span class="sxs-lookup"><span data-stu-id="5d088-413">Operations that normally execute a query have a query property that you can adjust with a where clause.</span></span> <span data-ttu-id="5d088-414">Frågeegenskapen är en [QueryJS] objekt som används för att konvertera en OData-frågan till något som kan bearbeta data serverdelen.</span><span class="sxs-lookup"><span data-stu-id="5d088-414">The query property is a [QueryJS] object that is used to convert an OData query to something that the data backend can process.</span></span>  <span data-ttu-id="5d088-415">För enkel likheten fall (se ovan), kan du använda en karta.</span><span class="sxs-lookup"><span data-stu-id="5d088-415">For simple equality cases (like the preceding one), a map can be used.</span></span> <span data-ttu-id="5d088-416">Du kan också lägga till specifika SQL-satser:</span><span class="sxs-lookup"><span data-stu-id="5d088-416">You can also add specific SQL clauses:</span></span>

    context.query.where('myfield eq ?', 'value');

### <span data-ttu-id="5d088-417"><a name="howto-tables-softdelete"></a>Så här: Konfigurera mjuk borttagning för en tabell</span><span class="sxs-lookup"><span data-stu-id="5d088-417"><a name="howto-tables-softdelete"></a>How to: Configure soft delete on a table</span></span>
<span data-ttu-id="5d088-418">Mjuk borttagning faktiskt bort inte poster.</span><span class="sxs-lookup"><span data-stu-id="5d088-418">Soft Delete does not actually delete records.</span></span>  <span data-ttu-id="5d088-419">I stället märks de som borttagna i databasen genom att den borttagna kolumnen till true.</span><span class="sxs-lookup"><span data-stu-id="5d088-419">Instead it marks them as deleted within the database by setting the deleted column to true.</span></span>  <span data-ttu-id="5d088-420">Azure Mobile Apps-SDK tar automatiskt bort ej permanent borttagna poster från resultat om Mobile klient-SDK använder IncludeDeleted().</span><span class="sxs-lookup"><span data-stu-id="5d088-420">The Azure Mobile Apps SDK automatically removes soft-deleted records from results unless the Mobile Client SDK uses IncludeDeleted().</span></span>  <span data-ttu-id="5d088-421">För att konfigurera en tabell för mjuk borttagning, ange den `softDelete` egenskapen i definitionsfilen för tabell:</span><span class="sxs-lookup"><span data-stu-id="5d088-421">To configure a table for soft delete, set the `softDelete` property in the table definition file:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="5d088-422">Du bör fastställa en mekanism för att rensa poster – antingen från ett klientprogram, via ett Webbjobb Azure-funktion eller via en anpassad API.</span><span class="sxs-lookup"><span data-stu-id="5d088-422">You should establish a mechanism for purging records - either from a client application, via a WebJob, Azure Function or through a custom API.</span></span>

### <span data-ttu-id="5d088-423"><a name="howto-tables-seeding"></a>Så här: startvärde för databasen med data</span><span class="sxs-lookup"><span data-stu-id="5d088-423"><a name="howto-tables-seeding"></a>How to: Seed your database with data</span></span>
<span data-ttu-id="5d088-424">När du skapar ett nytt program kan du initiera en tabell med data.</span><span class="sxs-lookup"><span data-stu-id="5d088-424">When creating a new application, you may wish to seed a table with data.</span></span>  <span data-ttu-id="5d088-425">Detta kan göras i tabellen definition JavaScript-fil på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5d088-425">This can be done within the table definition JavaScript file as follows:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="5d088-426">Dirigering av data utförs endast när tabellen har skapats med Azure Mobile Apps-SDK.</span><span class="sxs-lookup"><span data-stu-id="5d088-426">Seeding of data is only done when the table is created by the Azure Mobile Apps SDK.</span></span>  <span data-ttu-id="5d088-427">Om tabellen redan finns i databasen injekteras inga data i tabellen.</span><span class="sxs-lookup"><span data-stu-id="5d088-427">If the table already exists within the database, no data is injected into the table.</span></span>  <span data-ttu-id="5d088-428">Om dynamiska schemat är aktiverat härleda schemat från matade data.</span><span class="sxs-lookup"><span data-stu-id="5d088-428">If dynamic schema is turned on, then the schema is inferred from the seeded data.</span></span>

<span data-ttu-id="5d088-429">Vi rekommenderar att du explicit anropa den `tables.initialize()` metoden för att skapa tabellen när tjänsten startar.</span><span class="sxs-lookup"><span data-stu-id="5d088-429">We recommend that you explicitly call the `tables.initialize()` method to create the table when the service starts running.</span></span>

### <span data-ttu-id="5d088-430"><a name="Swagger"></a>Så här: aktivera stöd för Swagger</span><span class="sxs-lookup"><span data-stu-id="5d088-430"><a name="Swagger"></a>How to: Enable Swagger support</span></span>
<span data-ttu-id="5d088-431">Azure Apptjänst Mobilappar medföljer inbyggda [Swagger] stöd.</span><span class="sxs-lookup"><span data-stu-id="5d088-431">Azure App Service Mobile Apps comes with built-in [Swagger] support.</span></span>  <span data-ttu-id="5d088-432">Om du vill aktivera stöd för Swagger, installera swagger-användargränssnitt som ett beroende:</span><span class="sxs-lookup"><span data-stu-id="5d088-432">To enable Swagger support, first install the swagger-ui as a dependency:</span></span>

    npm install --save swagger-ui

<span data-ttu-id="5d088-433">När du har installerat kan du aktivera stöd för Swagger i Azure Mobile Apps-konstruktorn:</span><span class="sxs-lookup"><span data-stu-id="5d088-433">Once installed, you can enable Swagger support in the Azure Mobile Apps constructor:</span></span>

    var mobile = azureMobileApps({ swagger: true });

<span data-ttu-id="5d088-434">Du förmodligen bara vill aktivera stöd för Swagger i development utgåvor.</span><span class="sxs-lookup"><span data-stu-id="5d088-434">You probably only want to enable Swagger support in development editions.</span></span>  <span data-ttu-id="5d088-435">Du kan göra detta genom att använda den `NODE_ENV` appinställningen:</span><span class="sxs-lookup"><span data-stu-id="5d088-435">You can do this by utilizing the `NODE_ENV` app setting:</span></span>

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

<span data-ttu-id="5d088-436">Swagger-slutpunkt finns på http://*yoursite*.azurewebsites.net/swagger.</span><span class="sxs-lookup"><span data-stu-id="5d088-436">The swagger endpoint is located at http://*yoursite*.azurewebsites.net/swagger.</span></span>  <span data-ttu-id="5d088-437">Du kan komma åt Swagger-användargränssnitt via den `/swagger/ui` slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="5d088-437">You can access the Swagger UI via the `/swagger/ui` endpoint.</span></span>  <span data-ttu-id="5d088-438">Om du väljer att kräva autentisering över hela programmet genererar Swagger ett fel.</span><span class="sxs-lookup"><span data-stu-id="5d088-438">if you choose to require authentication across your entire application, Swagger produces an error.</span></span>  <span data-ttu-id="5d088-439">För bästa resultat bör du välja att tillåta oautentiserad förfrågningar via i Azure App Service-autentisering / auktoriseringsinställningar därefter kontrollera autentisering med hjälp av den `table.access` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="5d088-439">For best results, choose to allow unauthenticated requests through in the Azure App Service Authentication / Authorization settings, then control authentication using the `table.access` property.</span></span>

<span data-ttu-id="5d088-440">Du kan också lägga till alternativet Swagger till din `azureMobile.js` filen om du bara vill Swagger-stöd när du utvecklar lokalt.</span><span class="sxs-lookup"><span data-stu-id="5d088-440">You can also add the Swagger option to your `azureMobile.js` file if you only want Swagger support when developing locally.</span></span>

## <a name="a-namepushpush-notifications"></a><span data-ttu-id="5d088-441"><a name="push">Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="5d088-441"><a name="push">Push notifications</span></span>
<span data-ttu-id="5d088-442">Mobile Apps integreras med Azure Notification Hubs så att du kan skicka riktade push-meddelanden till miljontals enheter alla större plattformar.</span><span class="sxs-lookup"><span data-stu-id="5d088-442">Mobile Apps integrates with Azure Notification Hubs to enable you to send targeted push notifications to millions of devices across all major platforms.</span></span> <span data-ttu-id="5d088-443">Genom att använda Notification Hubs kan du skicka push-meddelanden till iOS, Android och Windows-enheter.</span><span class="sxs-lookup"><span data-stu-id="5d088-443">By using Notification Hubs, you can send push notifications to iOS, Android and Windows devices.</span></span> <span data-ttu-id="5d088-444">Mer information om alla som du kan göra med Notification Hubs finns [översikt över Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5d088-444">To learn more about all that you can do with Notification Hubs, see [Notification Hubs Overview](../notification-hubs/notification-hubs-push-notification-overview.md).</span></span>

### <span data-ttu-id="5d088-445"></a><a name="send-push"></a>Så här: skicka push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="5d088-445"></a><a name="send-push"></a>How to: Send push notifications</span></span>
<span data-ttu-id="5d088-446">Följande kod visar hur du använder push-objektet för att skicka skicka push-meddelanden till registrerade iOS-enheter:</span><span class="sxs-lookup"><span data-stu-id="5d088-446">The following code shows how to use the push object to send a broadcast push notification to registered iOS devices:</span></span>

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

<span data-ttu-id="5d088-447">Du kan i stället skicka en mall för push-meddelande till enheter på alla plattformar som stöds genom att skapa en mall för push-registrering från klienten.</span><span class="sxs-lookup"><span data-stu-id="5d088-447">By creating a template push registration from the client, you can instead send a template push message to devices on all supported platforms.</span></span> <span data-ttu-id="5d088-448">Följande kod visar hur du skickar ett meddelande om mallen:</span><span class="sxs-lookup"><span data-stu-id="5d088-448">The following code shows how to send a template notification:</span></span>

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


### <span data-ttu-id="5d088-449"><a name="push-user"></a>Så här: skicka push-meddelanden till en autentiserad användare med hjälp av taggar</span><span class="sxs-lookup"><span data-stu-id="5d088-449"><a name="push-user"></a>How to: Send push notifications to an authenticated user using tags</span></span>
<span data-ttu-id="5d088-450">När en autentiserad användare registrerar för push-meddelanden, läggs en tagg för användar-ID automatiskt till registreringen.</span><span class="sxs-lookup"><span data-stu-id="5d088-450">When an authenticated user registers for push notifications, a user ID tag is automatically added to the registration.</span></span> <span data-ttu-id="5d088-451">Du kan skicka push-meddelanden till alla enheter som registreras av en viss användare med hjälp av den här taggen.</span><span class="sxs-lookup"><span data-stu-id="5d088-451">By using this tag, you can send push notifications to all devices registered by a specific user.</span></span> <span data-ttu-id="5d088-452">Följande kod hämtar SID för användaren som skickar förfrågan och skickar en mall för push-meddelanden till alla enhetsregistrering för användaren:</span><span class="sxs-lookup"><span data-stu-id="5d088-452">The following code gets the SID of user making the request and sends a template push notification to every device registration for that user:</span></span>

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

<span data-ttu-id="5d088-453">När du registrerar dig för push-meddelanden från en autentiserad klient, se till att autentiseringen har slutförts innan du försöker utföra registreringen.</span><span class="sxs-lookup"><span data-stu-id="5d088-453">When registering for push notifications from an authenticated client, make sure that authentication is complete before attempting registration.</span></span>

## <span data-ttu-id="5d088-454"><a name="CustomAPI"></a>Anpassade API: er</span><span class="sxs-lookup"><span data-stu-id="5d088-454"><a name="CustomAPI"></a> Custom APIs</span></span>
### <span data-ttu-id="5d088-455"><a name="howto-customapi-basic"></a>Så här: definiera en anpassad API</span><span class="sxs-lookup"><span data-stu-id="5d088-455"><a name="howto-customapi-basic"></a>How to: Define a custom API</span></span>
<span data-ttu-id="5d088-456">Azure Mobile Apps kan tillhandahålla anpassade API täckning utöver dataåtkomst API via /tables slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="5d088-456">In addition to the data access API via the /tables endpoint, Azure Mobile Apps can provide custom API coverage.</span></span>  <span data-ttu-id="5d088-457">Anpassade API: er har definierats i ett liknande sätt att tabelldefinitionerna och har åtkomst till samma anläggningar, inklusive autentisering.</span><span class="sxs-lookup"><span data-stu-id="5d088-457">Custom APIs are defined in a similar way to the table definitions and can access all the same facilities, including authentication.</span></span>

<span data-ttu-id="5d088-458">Om du vill använda App Service-autentisering med en anpassad API måste du konfigurera App Service-autentisering i den [Azure-portalen] första.</span><span class="sxs-lookup"><span data-stu-id="5d088-458">If you wish to use App Service Authentication with a Custom API, you must configure App Service Authentication in the [Azure portal] first.</span></span>  <span data-ttu-id="5d088-459">Mer information om hur du konfigurerar autentisering i en Azure App Service granska i konfigurationsguiden för identitetsleverantör som du tänker använda:</span><span class="sxs-lookup"><span data-stu-id="5d088-459">For more details about configuring authentication in an Azure App Service, review the Configuration Guide for the identity provider you intend to use:</span></span>

* <span data-ttu-id="5d088-460">[Så här konfigurerar du Azure Active Directory-autentisering]</span><span class="sxs-lookup"><span data-stu-id="5d088-460">[How to configure Azure Active Directory Authentication]</span></span>
* <span data-ttu-id="5d088-461">[Hur du konfigurerar Facebook-autentisering]</span><span class="sxs-lookup"><span data-stu-id="5d088-461">[How to configure Facebook Authentication]</span></span>
* <span data-ttu-id="5d088-462">[Så här konfigurerar du autentisering med Google]</span><span class="sxs-lookup"><span data-stu-id="5d088-462">[How to configure Google Authentication]</span></span>
* <span data-ttu-id="5d088-463">[Så här konfigurerar du Microsoft Authentication]</span><span class="sxs-lookup"><span data-stu-id="5d088-463">[How to configure Microsoft Authentication]</span></span>
* <span data-ttu-id="5d088-464">[Hur du konfigurerar Twitter-autentisering]</span><span class="sxs-lookup"><span data-stu-id="5d088-464">[How to configure Twitter Authentication]</span></span>

<span data-ttu-id="5d088-465">Anpassade API: er har definierats i ungefär samma sätt som tabeller API.</span><span class="sxs-lookup"><span data-stu-id="5d088-465">Custom APIs are defined in much the same way as the Tables API.</span></span>

1. <span data-ttu-id="5d088-466">Skapa en **api** directory</span><span class="sxs-lookup"><span data-stu-id="5d088-466">Create an **api** directory</span></span>
2. <span data-ttu-id="5d088-467">Skapa en API-definitionen JavaScript-fil i den **api** directory.</span><span class="sxs-lookup"><span data-stu-id="5d088-467">Create an API definition JavaScript file in the **api** directory.</span></span>
3. <span data-ttu-id="5d088-468">Använda importmetoden för att importera den **api** directory.</span><span class="sxs-lookup"><span data-stu-id="5d088-468">Use the import method to import the **api** directory.</span></span>

<span data-ttu-id="5d088-469">Här är prototyp api-definition baserat på basic-app-exemplet som vi använde tidigare.</span><span class="sxs-lookup"><span data-stu-id="5d088-469">Here is the prototype api definition based on the basic-app sample we used earlier.</span></span>

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

<span data-ttu-id="5d088-470">Låt oss ta ett exempel API som returnerar datum server använder den *Date.now()* metod.</span><span class="sxs-lookup"><span data-stu-id="5d088-470">Let's take an example API that returns the server date using the *Date.now()* method.</span></span>  <span data-ttu-id="5d088-471">Här är filen api/date.js:</span><span class="sxs-lookup"><span data-stu-id="5d088-471">Here is the api/date.js file:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

<span data-ttu-id="5d088-472">Varje parameter är en standard RESTful-verb - GET, POST, uppdatera eller ta bort.</span><span class="sxs-lookup"><span data-stu-id="5d088-472">Each parameter is one of the standard RESTful verbs - GET, POST, PATCH, or DELETE.</span></span>  <span data-ttu-id="5d088-473">Metoden är en standard [ExpressJS Middleware] funktion som skickar utdata krävs.</span><span class="sxs-lookup"><span data-stu-id="5d088-473">The method is a standard [ExpressJS Middleware] function that sends the required output.</span></span>

### <span data-ttu-id="5d088-474"><a name="howto-customapi-auth"></a>Så här: kräver autentisering för åtkomst till en anpassad API</span><span class="sxs-lookup"><span data-stu-id="5d088-474"><a name="howto-customapi-auth"></a>How to: Require authentication for access to a custom API</span></span>
<span data-ttu-id="5d088-475">Azure Mobile Apps-SDK implementerar autentisering på samma sätt för både tabeller slutpunkt och anpassade API: er.</span><span class="sxs-lookup"><span data-stu-id="5d088-475">Azure Mobile Apps SDK implements authentication in the same way for both the tables endpoint and custom APIs.</span></span>  <span data-ttu-id="5d088-476">Om du vill lägga till autentisering i API: et som utvecklats i föregående avsnitt, lägga till en **åtkomst** egenskapen:</span><span class="sxs-lookup"><span data-stu-id="5d088-476">To add authentication to the API developed in the previous section, add an **access** property:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

<span data-ttu-id="5d088-477">Du kan också ange autentisering på specifika åtgärder:</span><span class="sxs-lookup"><span data-stu-id="5d088-477">You can also specify authentication on specific operations:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

<span data-ttu-id="5d088-478">Samma token som används för slutpunkten tabeller måste användas för anpassade API: er som kräver autentisering.</span><span class="sxs-lookup"><span data-stu-id="5d088-478">The same token that is used for the tables endpoint must be used for custom APIs requiring authentication.</span></span>

### <span data-ttu-id="5d088-479"><a name="howto-customapi-auth"></a>Så här: hantera stora filöverföringar</span><span class="sxs-lookup"><span data-stu-id="5d088-479"><a name="howto-customapi-auth"></a>How to: Handle large file uploads</span></span>
<span data-ttu-id="5d088-480">Azure Mobile Apps-SDK använder den [body-parsern mellanprogram](https://github.com/expressjs/body-parser) att acceptera och avkoda brödtext innehållet i din ansökan.</span><span class="sxs-lookup"><span data-stu-id="5d088-480">Azure Mobile Apps SDK uses the [body-parser middleware](https://github.com/expressjs/body-parser) to accept and decode body content in your submission.</span></span>  <span data-ttu-id="5d088-481">Du kan förkonfigurera body-parsern för att godkänna större filöverföringar:</span><span class="sxs-lookup"><span data-stu-id="5d088-481">You can pre-configure body-parser to accept larger file uploads:</span></span>

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

<span data-ttu-id="5d088-482">Filen är Base64-kodad före överföringen.</span><span class="sxs-lookup"><span data-stu-id="5d088-482">The file is base-64 encoded before transmission.</span></span>  <span data-ttu-id="5d088-483">Detta ökar storleken på den faktiska överföringen (och därför storleken du måste ta hänsyn till).</span><span class="sxs-lookup"><span data-stu-id="5d088-483">This increases the size of the actual upload (and hence the size you must account for).</span></span>

### <span data-ttu-id="5d088-484"><a name="howto-customapi-sql"></a>Så här: köra anpassade SQL-instruktioner</span><span class="sxs-lookup"><span data-stu-id="5d088-484"><a name="howto-customapi-sql"></a>How to: Execute custom SQL statements</span></span>
<span data-ttu-id="5d088-485">Azure Mobile Apps-SDK tillåter åtkomst till hela kontexten via Begäranobjektet, så att du kan köra SQL-instruktioner till definierade DataProvider enkelt:</span><span class="sxs-lookup"><span data-stu-id="5d088-485">The Azure Mobile Apps SDK allows access to the entire Context through the request object, allowing you to execute parameterized SQL statements to the defined data provider easily:</span></span>

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <span data-ttu-id="5d088-486"><a name="Debugging"></a>Felsökning, enkelt tabeller och enkelt API: er</span><span class="sxs-lookup"><span data-stu-id="5d088-486"><a name="Debugging"></a>Debugging, Easy Tables, and Easy APIs</span></span>
### <span data-ttu-id="5d088-487"><a name="howto-diagnostic-logs"></a>Så här: Felsök, diagnostisera och Felsök Azure Mobile apps</span><span class="sxs-lookup"><span data-stu-id="5d088-487"><a name="howto-diagnostic-logs"></a>How to: Debug, diagnose, and troubleshoot Azure Mobile apps</span></span>
<span data-ttu-id="5d088-488">Azure App Service tillhandahåller flera felsökning och felsökningstekniker för Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="5d088-488">The Azure App Service provides several debugging and troubleshooting techniques for Node.js applications.</span></span>
<span data-ttu-id="5d088-489">Se följande artiklar för att komma igång vid felsökning av din Node.js Mobile-serverdel:</span><span class="sxs-lookup"><span data-stu-id="5d088-489">Refer to the following articles to get started in troubleshooting your Node.js Mobile backend:</span></span>

* <span data-ttu-id="5d088-490">[Övervaka en Azure Apptjänst]</span><span class="sxs-lookup"><span data-stu-id="5d088-490">[Monitoring an Azure App Service]</span></span>
* <span data-ttu-id="5d088-491">[Aktivera diagnostikloggning i Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="5d088-491">[Enable Diagnostic Logging in Azure App Service]</span></span>
* <span data-ttu-id="5d088-492">[Felsöka en Azure Apptjänst i Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="5d088-492">[Troubleshoot an Azure App Service in Visual Studio]</span></span>

<span data-ttu-id="5d088-493">Node.js-program har åtkomst till en mängd olika verktyg diagnostiska loggen.</span><span class="sxs-lookup"><span data-stu-id="5d088-493">Node.js applications have access to a wide range of diagnostic log tools.</span></span>  <span data-ttu-id="5d088-494">Azure Mobile Apps Node.js SDK använder internt, [Winston] för diagnostikloggning.</span><span class="sxs-lookup"><span data-stu-id="5d088-494">Internally, the Azure Mobile Apps Node.js SDK uses [Winston] for diagnostic logging.</span></span>  <span data-ttu-id="5d088-495">Loggning aktiveras automatiskt genom att aktivera felsökningsläge eller genom att ange den **MS_DebugMode** appinställningen till true i den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="5d088-495">Logging is automatically enabled by enabling debug mode or by setting the **MS_DebugMode** app setting to true in the [Azure portal].</span></span> <span data-ttu-id="5d088-496">Genererade loggar visas i diagnostiska loggar på den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="5d088-496">Generated logs appear in the Diagnostic Logs on the [Azure portal].</span></span>

### <span data-ttu-id="5d088-497"><a name="in-portal-editing"></a><a name="work-easy-tables"></a>Så här: arbeta med enkelt tabeller i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5d088-497"><a name="in-portal-editing"></a><a name="work-easy-tables"></a>How to: Work with Easy Tables in the Azure portal</span></span>
<span data-ttu-id="5d088-498">Enkelt tabeller i portalen kan du skapa och arbeta med tabeller direkt i portalen.</span><span class="sxs-lookup"><span data-stu-id="5d088-498">Easy Tables in the portal let you create and work with tables right in the portal.</span></span> <span data-ttu-id="5d088-499">Du kan även redigera åtgärder för tabeller med App Service-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="5d088-499">You can even edit table operations using the App Service Editor.</span></span>

<span data-ttu-id="5d088-500">När du klickar på **enkelt tabeller** i dina backend-inställningar, du kan lägga till, ändra eller ta bort en tabell.</span><span class="sxs-lookup"><span data-stu-id="5d088-500">When you click **Easy tables** in your backend site settings, you can add, modify, or delete a table.</span></span> <span data-ttu-id="5d088-501">Du kan också se data i tabellen.</span><span class="sxs-lookup"><span data-stu-id="5d088-501">You can also see data in the table.</span></span>

![Arbeta med enkelt tabeller](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

<span data-ttu-id="5d088-503">Följande kommandon är tillgängliga i kommandofältet för en tabell:</span><span class="sxs-lookup"><span data-stu-id="5d088-503">The following commands are available on the command bar for a table:</span></span>

* <span data-ttu-id="5d088-504">**Ändra behörigheter** - ändringsbehörighet för läsning, infoga, uppdatera och ta bort tabellen.</span><span class="sxs-lookup"><span data-stu-id="5d088-504">**Change permissions** - modify the permission for read, insert, update and delete operations on the table.</span></span>
  <span data-ttu-id="5d088-505">Alternativen är att tillåta anonym åtkomst, att kräva autentisering eller inaktivera all åtkomst till åtgärden.</span><span class="sxs-lookup"><span data-stu-id="5d088-505">Options are to allow anonymous access, to require authentication, or to disable all access to the operation.</span></span>
* <span data-ttu-id="5d088-506">**Redigera skriptet** -skriptfil för tabellen öppnas i App Service Editor.</span><span class="sxs-lookup"><span data-stu-id="5d088-506">**Edit script** - the script file for the table is opened in the App Service Editor.</span></span>
* <span data-ttu-id="5d088-507">**Hantera schemat** - Lägg till eller ta bort kolumner eller ändra Tabellindex.</span><span class="sxs-lookup"><span data-stu-id="5d088-507">**Manage schema** - add or delete columns or change the table index.</span></span>
* <span data-ttu-id="5d088-508">**Rensa tabellen** -trunkerar en befintlig tabell är om du tar bort alla datarader men lämnar schemat oförändrat.</span><span class="sxs-lookup"><span data-stu-id="5d088-508">**Clear table** - truncates an existing table be deleting all data rows but leaving the schema unchanged.</span></span>
* <span data-ttu-id="5d088-509">**Ta bort rader** -ta bort enskilda rader med data.</span><span class="sxs-lookup"><span data-stu-id="5d088-509">**Delete rows** - delete individual rows of data.</span></span>
* <span data-ttu-id="5d088-510">**Visa direktuppspelningsloggar** -ansluter du till strömmande Loggtjänsten för webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="5d088-510">**View streaming logs** - connects you to the streaming log service for your site.</span></span>

### <span data-ttu-id="5d088-511"><a name="work-easy-apis"></a>Så här: arbeta med enkla API: er i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5d088-511"><a name="work-easy-apis"></a>How to: Work with Easy APIs in the Azure portal</span></span>
<span data-ttu-id="5d088-512">Enkelt API: er i portalen kan du skapa och arbeta med anpassade API: er direkt i portalen.</span><span class="sxs-lookup"><span data-stu-id="5d088-512">Easy APIs in the portal let you create and work with custom APIs right in the portal.</span></span> <span data-ttu-id="5d088-513">Du kan redigera API-skript med hjälp av Redigeraren för App Service.</span><span class="sxs-lookup"><span data-stu-id="5d088-513">You can edit API scripts using the App Service Editor.</span></span>

<span data-ttu-id="5d088-514">När du klickar på **enkelt API: er** i dina backend-inställningar, du kan lägga till, ändra eller ta bort en anpassad API-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="5d088-514">When you click **Easy APIs** in your backend site settings, you can add, modify, or delete a custom API endpoint.</span></span>

![Arbeta med enkla API: er](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

<span data-ttu-id="5d088-516">I portalen du ändra åtkomstbehörighet för en viss HTTP-åtgärd, redigera skriptfilen API i App Service Editor eller visa strömmande loggarna.</span><span class="sxs-lookup"><span data-stu-id="5d088-516">In the portal, you can change the access permissions for a given HTTP action, edit the API script file in the App Service Editor, or view the streaming logs.</span></span>

### <span data-ttu-id="5d088-517"><a name="online-editor"></a>Så här: redigera kod i App Service Editor</span><span class="sxs-lookup"><span data-stu-id="5d088-517"><a name="online-editor"></a>How to: Edit code in the App Service Editor</span></span>
<span data-ttu-id="5d088-518">Azure-portalen kan du redigera dina Node.js-serverdel skriptfiler i App Service Editor utan att behöva hämta projektet till den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="5d088-518">The Azure portal lets you edit your Node.js backend script files in the App Service Editor without having to download the project to your local computer.</span></span> <span data-ttu-id="5d088-519">Redigera filer i redigeraren online:</span><span class="sxs-lookup"><span data-stu-id="5d088-519">To edit script files in the online editor:</span></span>

1. <span data-ttu-id="5d088-520">I din Mobilapp backend-bladet klickar du på **alla inställningar** > antingen **enkelt tabeller** eller **enkelt API: er**, klicka på en tabell eller API: et och sedan på **redigera skriptet**.</span><span class="sxs-lookup"><span data-stu-id="5d088-520">In your Mobile App backend blade, click **All settings** > either **Easy tables** or **Easy APIs**, click a table or API, then click **Edit script**.</span></span> <span data-ttu-id="5d088-521">Skriptet öppnas i App Service Editor.</span><span class="sxs-lookup"><span data-stu-id="5d088-521">The script file is opened in the App Service Editor.</span></span>

    ![App Service Editor](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. <span data-ttu-id="5d088-523">Gör ändringarna i filen koden i redigeraren online.</span><span class="sxs-lookup"><span data-stu-id="5d088-523">Make your changes to the code file in the online editor.</span></span> <span data-ttu-id="5d088-524">Ändringarna sparas automatiskt när du skriver.</span><span class="sxs-lookup"><span data-stu-id="5d088-524">Changes are saved automatically as you type.</span></span>

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
<span data-ttu-id="5d088-525">[Snabbstart för Android-klienten]: app-service-mobile-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="5d088-525">[Android Client QuickStart]: app-service-mobile-android-get-started.md</span></span>
<span data-ttu-id="5d088-526">[Snabbstart för Apache Cordova-klient]: app-service-mobile-cordova-get-started.md</span><span class="sxs-lookup"><span data-stu-id="5d088-526">[Apache Cordova Client QuickStart]: app-service-mobile-cordova-get-started.md</span></span>
<span data-ttu-id="5d088-527">[iOS klienten Snabbstart]: app-service-mobile-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="5d088-527">[iOS Client QuickStart]: app-service-mobile-ios-get-started.md</span></span>
<span data-ttu-id="5d088-528">[Snabbstart för Xamarin.iOS-klient]: app-service-mobile-xamarin-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="5d088-528">[Xamarin.iOS Client QuickStart]: app-service-mobile-xamarin-ios-get-started.md</span></span>
<span data-ttu-id="5d088-529">[Snabbstart för Xamarin.Android-klient]: app-service-mobile-xamarin-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="5d088-529">[Xamarin.Android Client QuickStart]: app-service-mobile-xamarin-android-get-started.md</span></span>
<span data-ttu-id="5d088-530">[Snabbstart för Xamarin.Forms-klient]: app-service-mobile-xamarin-forms-get-started.md</span><span class="sxs-lookup"><span data-stu-id="5d088-530">[Xamarin.Forms Client QuickStart]: app-service-mobile-xamarin-forms-get-started.md</span></span>
<span data-ttu-id="5d088-531">[Snabbstart för Windows Store-klienten]: app-service-mobile-windows-store-dotnet-get-started.md</span><span class="sxs-lookup"><span data-stu-id="5d088-531">[Windows Store Client QuickStart]: app-service-mobile-windows-store-dotnet-get-started.md</span></span>
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
<span data-ttu-id="5d088-532">[datasynkronisering offline]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="5d088-532">[offline data sync]: app-service-mobile-offline-data-sync.md</span></span>
<span data-ttu-id="5d088-533">[Så här konfigurerar du Azure Active Directory-autentisering]: app-service-mobile-how-to-configure-active-directory-authentication.md</span><span class="sxs-lookup"><span data-stu-id="5d088-533">[How to configure Azure Active Directory Authentication]: app-service-mobile-how-to-configure-active-directory-authentication.md</span></span>
<span data-ttu-id="5d088-534">[Hur du konfigurerar Facebook-autentisering]: app-service-mobile-how-to-configure-facebook-authentication.md</span><span class="sxs-lookup"><span data-stu-id="5d088-534">[How to configure Facebook Authentication]: app-service-mobile-how-to-configure-facebook-authentication.md</span></span>
<span data-ttu-id="5d088-535">[Så här konfigurerar du autentisering med Google]: app-service-mobile-how-to-configure-google-authentication.md</span><span class="sxs-lookup"><span data-stu-id="5d088-535">[How to configure Google Authentication]: app-service-mobile-how-to-configure-google-authentication.md</span></span>
<span data-ttu-id="5d088-536">[Så här konfigurerar du Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md</span><span class="sxs-lookup"><span data-stu-id="5d088-536">[How to configure Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md</span></span>
<span data-ttu-id="5d088-537">[Hur du konfigurerar Twitter-autentisering]: app-service-mobile-how-to-configure-twitter-authentication.md</span><span class="sxs-lookup"><span data-stu-id="5d088-537">[How to configure Twitter Authentication]: app-service-mobile-how-to-configure-twitter-authentication.md</span></span>
<span data-ttu-id="5d088-538">[Azure App Service Deployment Guide]: ../app-service-web/web-sites-deploy.md</span><span class="sxs-lookup"><span data-stu-id="5d088-538">[Azure App Service Deployment Guide]: ../app-service-web/web-sites-deploy.md</span></span>
<span data-ttu-id="5d088-539">[Övervaka en Azure Apptjänst]: ../app-service-web/web-sites-monitor.md</span><span class="sxs-lookup"><span data-stu-id="5d088-539">[Monitoring an Azure App Service]: ../app-service-web/web-sites-monitor.md</span></span>
<span data-ttu-id="5d088-540">[Aktivera diagnostikloggning i Azure App Service]: ../app-service-web/web-sites-enable-diagnostic-log.md</span><span class="sxs-lookup"><span data-stu-id="5d088-540">[Enable Diagnostic Logging in Azure App Service]: ../app-service-web/web-sites-enable-diagnostic-log.md</span></span>
<span data-ttu-id="5d088-541">[Felsöka en Azure Apptjänst i Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md</span><span class="sxs-lookup"><span data-stu-id="5d088-541">[Troubleshoot an Azure App Service in Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md</span></span>
<span data-ttu-id="5d088-542">[ange nod-Version]: ../nodejs-specify-node-version-azure-apps.md</span><span class="sxs-lookup"><span data-stu-id="5d088-542">[specify the Node Version]: ../nodejs-specify-node-version-azure-apps.md</span></span>
<span data-ttu-id="5d088-543">[använda noden moduler]: ../nodejs-use-node-modules-azure-apps.md</span><span class="sxs-lookup"><span data-stu-id="5d088-543">[use Node modules]: ../nodejs-use-node-modules-azure-apps.md</span></span>
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
<span data-ttu-id="5d088-544">[Express]: http://expressjs.com/</span><span class="sxs-lookup"><span data-stu-id="5d088-544">[Express]: http://expressjs.com/</span></span>
<span data-ttu-id="5d088-545">[Swagger]: http://swagger.io/</span><span class="sxs-lookup"><span data-stu-id="5d088-545">[Swagger]: http://swagger.io/</span></span>

<span data-ttu-id="5d088-546">[Azure-portalen]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="5d088-546">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="5d088-547">[OData]: http://www.odata.org</span><span class="sxs-lookup"><span data-stu-id="5d088-547">[OData]: http://www.odata.org</span></span>
<span data-ttu-id="5d088-548">[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise</span><span class="sxs-lookup"><span data-stu-id="5d088-548">[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise</span></span>
<span data-ttu-id="5d088-549">[basicapp exempel på GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app</span><span class="sxs-lookup"><span data-stu-id="5d088-549">[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app</span></span>
<span data-ttu-id="5d088-550">[todo prov på GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo</span><span class="sxs-lookup"><span data-stu-id="5d088-550">[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo</span></span>
<span data-ttu-id="5d088-551">[katalogen samples på GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples</span><span class="sxs-lookup"><span data-stu-id="5d088-551">[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples</span></span>
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
<span data-ttu-id="5d088-552">[QueryJS]: https://github.com/Azure/queryjs</span><span class="sxs-lookup"><span data-stu-id="5d088-552">[QueryJS]: https://github.com/Azure/queryjs</span></span>
<span data-ttu-id="5d088-553">[1.1 för Node.js-verktyg för Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1</span><span class="sxs-lookup"><span data-stu-id="5d088-553">[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1</span></span>
<span data-ttu-id="5d088-554">[mssql Node.js paketet]: https://www.npmjs.com/package/mssql</span><span class="sxs-lookup"><span data-stu-id="5d088-554">[mssql Node.js package]: https://www.npmjs.com/package/mssql</span></span>
<span data-ttu-id="5d088-555">[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx</span><span class="sxs-lookup"><span data-stu-id="5d088-555">[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx</span></span>
<span data-ttu-id="5d088-556">[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html</span><span class="sxs-lookup"><span data-stu-id="5d088-556">[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html</span></span>
<span data-ttu-id="5d088-557">[Winston]: https://github.com/winstonjs/winston</span><span class="sxs-lookup"><span data-stu-id="5d088-557">[Winston]: https://github.com/winstonjs/winston</span></span>
