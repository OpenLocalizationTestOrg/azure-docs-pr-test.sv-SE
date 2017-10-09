---
title: "aaaHow toowork med serverdelen för hello Node.js SDK för Mobile Apps | Microsoft Docs"
description: "Lär dig hur toowork med hello serverdelen för Node.js SDK för Azure Apptjänst Mobilappar."
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
ms.openlocfilehash: 2b1ea5fda6f6ca422b92fe29ff8d16bf035018d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-mobile-apps-nodejs-sdk"></a><span data-ttu-id="b49bc-103">Hur toouse hello Azure Mobile Apps Node.js SDK</span><span class="sxs-lookup"><span data-stu-id="b49bc-103">How toouse hello Azure Mobile Apps Node.js SDK</span></span>
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

<span data-ttu-id="b49bc-104">Den här artikeln innehåller detaljerad information och exempel som visar hur toowork med Node.js-serverdel i Azure Apptjänst Mobilappar.</span><span class="sxs-lookup"><span data-stu-id="b49bc-104">This article provides detailed information and examples showing how toowork with a Node.js backend in Azure App Service Mobile Apps.</span></span>

## <span data-ttu-id="b49bc-105"><a name="Introduction"></a>Introduktion</span><span class="sxs-lookup"><span data-stu-id="b49bc-105"><a name="Introduction"></a>Introduction</span></span>
<span data-ttu-id="b49bc-106">Azure Apptjänst Mobilappar tillhandahåller hello kapaciteten tooadd en mobile optimerad dataåtkomst Web API tooa webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b49bc-106">Azure App Service Mobile Apps provides hello capability tooadd a mobile-optimized data access Web API tooa web application.</span></span>  <span data-ttu-id="b49bc-107">hello Azure App Service Mobile Apps-SDK har angetts för ASP.NET och Node.js webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b49bc-107">hello Azure App Service Mobile Apps SDK is provided for ASP.NET and Node.js web applications.</span></span>  <span data-ttu-id="b49bc-108">hello SDK innehåller hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="b49bc-108">hello SDK provides hello following operations:</span></span>

* <span data-ttu-id="b49bc-109">Tabellåtgärder (Läs-, Insert-, Update-, Delete) för dataåtkomst</span><span class="sxs-lookup"><span data-stu-id="b49bc-109">Table operations (Read, Insert, Update, Delete) for data access</span></span>
* <span data-ttu-id="b49bc-110">Anpassade API-åtgärder</span><span class="sxs-lookup"><span data-stu-id="b49bc-110">Custom API operations</span></span>

<span data-ttu-id="b49bc-111">Både operations erbjuda autentisering över alla identitetsleverantörer som tillåts av Azure App Service, inklusive sociala identitetsleverantörer, till exempel Facebook, Twitter, Google och Microsoft samt Azure Active Directory för enterprise-identitet.</span><span class="sxs-lookup"><span data-stu-id="b49bc-111">Both operations provide for authentication across all identity providers allowed by Azure App Service, including social identity providers such as Facebook, Twitter, Google and Microsoft as well as Azure Active Directory for enterprise identity.</span></span>

<span data-ttu-id="b49bc-112">Du kan hitta exempel för varje användningsfall hello [katalogen samples på GitHub].</span><span class="sxs-lookup"><span data-stu-id="b49bc-112">You can find samples for each use case in hello [samples directory on GitHub].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="b49bc-113">Plattformar som stöds</span><span class="sxs-lookup"><span data-stu-id="b49bc-113">Supported Platforms</span></span>
<span data-ttu-id="b49bc-114">hello Azure Mobile Apps nod SDK stöder hello aktuella LTS viktiga för nod och senare.</span><span class="sxs-lookup"><span data-stu-id="b49bc-114">hello Azure Mobile Apps Node SDK supports hello current LTS release of Node and later.</span></span>  <span data-ttu-id="b49bc-115">Från och med skrivning är hello senaste LTS versionen noden v4.5.0.</span><span class="sxs-lookup"><span data-stu-id="b49bc-115">As of writing, hello latest LTS version is Node v4.5.0.</span></span>  <span data-ttu-id="b49bc-116">Andra versioner av noden fungerar men stöds inte.</span><span class="sxs-lookup"><span data-stu-id="b49bc-116">Other versions of Node may work, but are not supported.</span></span>

<span data-ttu-id="b49bc-117">hello Azure Mobile Apps nod SDK stöder två databasdrivrutiner för - hello nod mssql drivrutinen har stöd för SQL Azure och lokala SQL Server-instanser.</span><span class="sxs-lookup"><span data-stu-id="b49bc-117">hello Azure Mobile Apps Node SDK supports two database drivers - hello node-mssql driver supports SQL Azure and local SQL Server instances.</span></span>  <span data-ttu-id="b49bc-118">Hej sqlite3 drivrutinen stöder SQLite databaser på en enda instans.</span><span class="sxs-lookup"><span data-stu-id="b49bc-118">hello sqlite3 driver supports SQLite databases on a single instance only.</span></span>

### <span data-ttu-id="b49bc-119"><a name="howto-cmdline-basicapp"></a>Så här: skapa ett grundläggande Node.js-serverdel med hello kommandoraden</span><span class="sxs-lookup"><span data-stu-id="b49bc-119"><a name="howto-cmdline-basicapp"></a>How to: Create a Basic Node.js backend using hello Command Line</span></span>
<span data-ttu-id="b49bc-120">Varje Azure App Service Mobile App Node.js-serverdel startar som ett ExpressJS program.</span><span class="sxs-lookup"><span data-stu-id="b49bc-120">Every Azure App Service Mobile App Node.js backend starts as an ExpressJS application.</span></span>  <span data-ttu-id="b49bc-121">ExpressJS är hello populäraste web service ramverk för Node.js.</span><span class="sxs-lookup"><span data-stu-id="b49bc-121">ExpressJS is hello most popular web service framework available for Node.js.</span></span>  <span data-ttu-id="b49bc-122">Du kan skapa en grundläggande [Express] program på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b49bc-122">You can create a basic [Express] application as follows:</span></span>

1. <span data-ttu-id="b49bc-123">Skapa en katalog för ditt projekt i ett kommando eller PowerShell-fönstret.</span><span class="sxs-lookup"><span data-stu-id="b49bc-123">In a command or PowerShell window, create a directory for your project.</span></span>

        mkdir basicapp
2. <span data-ttu-id="b49bc-124">Kör npm init tooinitialize hello paketet struktur.</span><span class="sxs-lookup"><span data-stu-id="b49bc-124">Run npm init tooinitialize hello package structure.</span></span>

        cd basicapp
        npm init

    <span data-ttu-id="b49bc-125">Hej npm init kommandot frågar en uppsättning frågor tooinitialize hello projektet.</span><span class="sxs-lookup"><span data-stu-id="b49bc-125">hello npm init command asks a set of questions tooinitialize hello project.</span></span>  <span data-ttu-id="b49bc-126">Se hello exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="b49bc-126">See hello example output:</span></span>

    ![Hej npm init-utdata][0]
3. <span data-ttu-id="b49bc-128">Installera hello express och azure mobilappar bibliotek från hello npm-databasen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-128">Install hello express and azure-mobile-apps libraries from hello npm repository.</span></span>

        npm install --save express azure-mobile-apps
4. <span data-ttu-id="b49bc-129">Skapa en app.js tooimplement hello grundläggande mobila filserver.</span><span class="sxs-lookup"><span data-stu-id="b49bc-129">Create an app.js file tooimplement hello basic mobile server.</span></span>

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add hello mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

<span data-ttu-id="b49bc-130">Det här programmet skapar en optimerad för-WebAPI med en enda slutpunkt (`/tables/TodoItem`) som ger obehörig åtkomst tooan underliggande SQL-datalagret med en dynamisk schema.</span><span class="sxs-lookup"><span data-stu-id="b49bc-130">This application creates a mobile-optimized WebAPI with a single endpoint (`/tables/TodoItem`) that provides unauthenticated access tooan underlying SQL data store using a dynamic schema.</span></span>  <span data-ttu-id="b49bc-131">Det är lämpligt för följande snabbstarter för klient-biblioteket:</span><span class="sxs-lookup"><span data-stu-id="b49bc-131">It is suitable for following the client library quick starts:</span></span>

* <span data-ttu-id="b49bc-132">[Snabbstart för Android-klienten]</span><span class="sxs-lookup"><span data-stu-id="b49bc-132">[Android Client QuickStart]</span></span>
* <span data-ttu-id="b49bc-133">[Snabbstart för Apache Cordova-klient]</span><span class="sxs-lookup"><span data-stu-id="b49bc-133">[Apache Cordova Client QuickStart]</span></span>
* <span data-ttu-id="b49bc-134">[iOS klienten Snabbstart]</span><span class="sxs-lookup"><span data-stu-id="b49bc-134">[iOS Client QuickStart]</span></span>
* <span data-ttu-id="b49bc-135">[Snabbstart för Windows Store-klienten]</span><span class="sxs-lookup"><span data-stu-id="b49bc-135">[Windows Store Client QuickStart]</span></span>
* <span data-ttu-id="b49bc-136">[Snabbstart för Xamarin.iOS-klient]</span><span class="sxs-lookup"><span data-stu-id="b49bc-136">[Xamarin.iOS Client QuickStart]</span></span>
* <span data-ttu-id="b49bc-137">[Snabbstart för Xamarin.Android-klient]</span><span class="sxs-lookup"><span data-stu-id="b49bc-137">[Xamarin.Android Client QuickStart]</span></span>
* <span data-ttu-id="b49bc-138">[Snabbstart för Xamarin.Forms-klient]</span><span class="sxs-lookup"><span data-stu-id="b49bc-138">[Xamarin.Forms Client QuickStart]</span></span>

<span data-ttu-id="b49bc-139">Du hittar hello koden för grundläggande programmet i hello [basicapp exempel på GitHub].</span><span class="sxs-lookup"><span data-stu-id="b49bc-139">You can find hello code for this basic application in hello [basicapp sample on GitHub].</span></span>

### <span data-ttu-id="b49bc-140"><a name="howto-vs2015-basicapp"></a>Så här: skapa en nod serverdel med Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="b49bc-140"><a name="howto-vs2015-basicapp"></a>How to: Create a Node backend with Visual Studio 2015</span></span>
<span data-ttu-id="b49bc-141">Visual Studio 2015 kräver ett tillägg toodevelop Node.js-program inom hello IDE.</span><span class="sxs-lookup"><span data-stu-id="b49bc-141">Visual Studio 2015 requires an extension toodevelop Node.js applications within hello IDE.</span></span>  <span data-ttu-id="b49bc-142">toostart, installera hello [1.1 för Node.js-verktyg för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="b49bc-142">toostart, install hello [Node.js Tools 1.1 for Visual Studio].</span></span>  <span data-ttu-id="b49bc-143">När hello Node.js Tools för Visual Studio har installerats kan du skapa ett Express 4.x program:</span><span class="sxs-lookup"><span data-stu-id="b49bc-143">Once hello Node.js Tools for Visual Studio are installed, create an Express 4.x application:</span></span>

1. <span data-ttu-id="b49bc-144">Öppna hello **nytt projekt** dialogrutan (från **filen** > **ny** > **projekt...** ).</span><span class="sxs-lookup"><span data-stu-id="b49bc-144">Open hello **New Project** dialog (from **File** > **New** > **Project...**).</span></span>
2. <span data-ttu-id="b49bc-145">Expandera **mallar** > **JavaScript** > **Node.js**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-145">Expand **Templates** > **JavaScript** > **Node.js**.</span></span>
3. <span data-ttu-id="b49bc-146">Välj hello **grundläggande Azure Node.js Express 4 program**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-146">Select hello **Basic Azure Node.js Express 4 Application**.</span></span>
4. <span data-ttu-id="b49bc-147">Fyll i hello projektnamn.</span><span class="sxs-lookup"><span data-stu-id="b49bc-147">Fill in hello project name.</span></span>  <span data-ttu-id="b49bc-148">Klicka på *OK*.</span><span class="sxs-lookup"><span data-stu-id="b49bc-148">Click *OK*.</span></span>

    ![Nytt projekt i Visual Studio 2015][1]
5. <span data-ttu-id="b49bc-150">Högerklicka på hello **npm** och välj **installera nya npm-paket...** .</span><span class="sxs-lookup"><span data-stu-id="b49bc-150">Right-click hello **npm** node and select **Install New npm packages...**.</span></span>
6. <span data-ttu-id="b49bc-151">Du kan behöva toorefresh hello npm katalogen om hur du skapar din första Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="b49bc-151">You may need toorefresh hello npm catalog on creating your first Node.js application.</span></span>  <span data-ttu-id="b49bc-152">Klicka på **uppdatera** om det behövs.</span><span class="sxs-lookup"><span data-stu-id="b49bc-152">Click **Refresh** if necessary.</span></span>
7. <span data-ttu-id="b49bc-153">Ange *azure mobilappar* i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="b49bc-153">Enter *azure-mobile-apps* in hello search box.</span></span>  <span data-ttu-id="b49bc-154">Klicka på hello **azure mobile apps 2.0.0** paketet och klicka sedan på **installera paket**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-154">Click hello **azure-mobile-apps 2.0.0** package, then click **Install Package**.</span></span>

    ![Installera nya npm-paket][2]
8. <span data-ttu-id="b49bc-156">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-156">Click **Close**.</span></span>
9. <span data-ttu-id="b49bc-157">Öppna hello *app.js* tooadd stöd för hello Azure Mobile Apps-SDK.</span><span class="sxs-lookup"><span data-stu-id="b49bc-157">Open hello *app.js* file tooadd support for hello Azure Mobile Apps SDK.</span></span>  <span data-ttu-id="b49bc-158">Längst ned rad 6 at hello hello bibliotek kräver instruktioner, Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="b49bc-158">At line 6 at hello bottom of hello library require statements, add hello following code:</span></span>

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    <span data-ttu-id="b49bc-159">Lägg till hello följande kod på cirka rad 27 efter hello andra app.use instruktioner:</span><span class="sxs-lookup"><span data-stu-id="b49bc-159">At approximately line 27 after hello other app.use statements, add hello following code:</span></span>

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    <span data-ttu-id="b49bc-160">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-160">Save hello file.</span></span>
10. <span data-ttu-id="b49bc-161">Kör hello programmet lokalt (hello API levereras på http://localhost: 3000) eller publicera tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b49bc-161">Either run hello application locally (hello API is served on http://localhost:3000) or publish tooAzure.</span></span>

### <span data-ttu-id="b49bc-162"><a name="create-node-backend-portal"></a>Så här: skapa en Node.js-serverdel med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b49bc-162"><a name="create-node-backend-portal"></a>How to: Create a Node.js backend using hello Azure portal</span></span>
<span data-ttu-id="b49bc-163">Du kan skapa en Mobilapp backend rätt i hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="b49bc-163">You can create a Mobile App backend right in hello [Azure portal].</span></span> <span data-ttu-id="b49bc-164">Du kan antingen följa hello följande steg eller skapa en klient och server tillsammans med följande hello [skapar en mobil app](app-service-mobile-ios-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-164">You can either follow hello following steps or create a client and server together by following hello [Create a mobile app](app-service-mobile-ios-get-started.md) tutorial.</span></span> <span data-ttu-id="b49bc-165">hello självstudier innehåller en förenklad version av dessa anvisningar och är bäst för bevis på koncept projekt.</span><span class="sxs-lookup"><span data-stu-id="b49bc-165">hello tutorial contains a simplified version of these instructions and is best for proof of concept projects.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

<span data-ttu-id="b49bc-166">Tillbaka i hello *Kom igång* bladet under **skapa en tabell API**, Välj **Node.js** som din **språk för serverdel**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-166">Back in hello *Get started* blade, under **Create a table API**, choose **Node.js** as your **Backend language**.</span></span>
<span data-ttu-id="b49bc-167">Kryssrutan för hello ”**bekräftar jag att skrivs alla plats innehållet.**”, klicka på **skapa TodoItem-tabell**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-167">Check hello box for "**I acknowledge that this will overwrite all site contents.**", then click **Create TodoItem table**.</span></span>

### <span data-ttu-id="b49bc-168"><a name="download-quickstart"></a>Så här: hämta hello Node.js-serverdel kod snabbstartsprojekt med Git</span><span class="sxs-lookup"><span data-stu-id="b49bc-168"><a name="download-quickstart"></a>How to: Download hello Node.js backend quickstart code project using Git</span></span>
<span data-ttu-id="b49bc-169">När du skapar en Node.js mobilappsserverdel genom att använda hello portal **Snabbstart** bladet ett Node.js-projekt har skapats för dig och distribuerade tooyour plats.</span><span class="sxs-lookup"><span data-stu-id="b49bc-169">When you create a Node.js Mobile App backend by using hello portal **Quick start** blade, a Node.js project is created for you and deployed tooyour site.</span></span> <span data-ttu-id="b49bc-170">Du kan lägga till tabeller och API: er och redigera kodfiler för hello Node.js-serverdel i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-170">You can add tables and APIs and edit code files for hello Node.js backend in hello portal.</span></span> <span data-ttu-id="b49bc-171">Du kan också använda olika verktyg toodownload hello backend projekt för distribution så att du kan lägga till eller ändra tabeller och API: er och sedan publicera hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="b49bc-171">You can also use various deployment tools toodownload hello backend project so that you can add or modify tables and APIs, then republish hello project.</span></span> <span data-ttu-id="b49bc-172">Mer information finns i [Azure App Service Deployment Guide].</span><span class="sxs-lookup"><span data-stu-id="b49bc-172">For more information, see the [Azure App Service Deployment Guide].</span></span> <span data-ttu-id="b49bc-173">hello används följande procedur en Git-lagringsplatsen toodownload hello quickstart Projektkod.</span><span class="sxs-lookup"><span data-stu-id="b49bc-173">hello following procedure uses a Git repository toodownload hello quickstart project code.</span></span>

1. <span data-ttu-id="b49bc-174">Installera Git om du inte redan gjort det..</span><span class="sxs-lookup"><span data-stu-id="b49bc-174">Install Git, if you haven't already done so.</span></span> <span data-ttu-id="b49bc-175">hello steg krävs tooinstall Git variera mellan olika operativsystem.</span><span class="sxs-lookup"><span data-stu-id="b49bc-175">hello steps required tooinstall Git vary between operating systems.</span></span> <span data-ttu-id="b49bc-176">Se [installerar Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) för operativsystemspecifika distributioner och installationer.</span><span class="sxs-lookup"><span data-stu-id="b49bc-176">See [Installing Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) for operating system-specific distributions and installation guidance.</span></span>
2. <span data-ttu-id="b49bc-177">Gör så hello i [aktivera hello Apptjänst app databasen](../app-service-web/app-service-deploy-local-git.md#Step3) tooenable hello Git-lagringsplats för backend-platsen, och ett meddelande om hello distribution användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="b49bc-177">Follow hello steps in [Enable hello App Service app repository](../app-service-web/app-service-deploy-local-git.md#Step3) tooenable hello Git repository for your backend site, making a note of hello deployment username and password.</span></span>
3. <span data-ttu-id="b49bc-178">I hello-bladet för din mobilappsserverdel anteckna hello **URL för Git-klon** inställningen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-178">In hello blade for your Mobile App backend, make a note of hello **Git clone URL** setting.</span></span>
4. <span data-ttu-id="b49bc-179">Köra hello `git clone` kommandot med hello Git klon-URL att ange lösenordet när det behövs, som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b49bc-179">Execute hello `git clone` command using hello Git clone URL, entering your password when required, as in the following example:</span></span>

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. <span data-ttu-id="b49bc-180">Bläddra toolocal directory, som i föregående exempel hello /todolist, och Lägg märke till att projektfiler har hämtats.</span><span class="sxs-lookup"><span data-stu-id="b49bc-180">Browse toolocal directory, which in hello preceding example is /todolist, and notice that project files have been downloaded.</span></span> <span data-ttu-id="b49bc-181">Leta upp hello `todoitem.json` filen i hello `/tables` directory.</span><span class="sxs-lookup"><span data-stu-id="b49bc-181">Locate hello `todoitem.json` file in hello `/tables` directory.</span></span>  <span data-ttu-id="b49bc-182">Den här filen definierar behörigheter i tabellen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-182">This file defines permissions on the table.</span></span>  <span data-ttu-id="b49bc-183">Också hitta hello `todoitem.js` filen i hello samma katalog som anger att skript CRUD-åtgärden för hello tabellen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-183">Also find hello `todoitem.js` file in hello same directory, which defines that CRUD operation scripts for hello table.</span></span>
6. <span data-ttu-id="b49bc-184">När du har gjort ändringar tooproject filer, kör du följande kommandon tooadd, genomför och sedan överföra ändringar toohello plats hello:</span><span class="sxs-lookup"><span data-stu-id="b49bc-184">After you have made changes tooproject files, execute hello following commands tooadd, commit, then upload the changes toohello site:</span></span>

        $ git commit -m "updated hello table script"
        $ git push origin master

    <span data-ttu-id="b49bc-185">När du lägger till nya filer toohello projekt måste du först tooexecute hello `git add .` kommando.</span><span class="sxs-lookup"><span data-stu-id="b49bc-185">When you add new files toohello project, you first need tooexecute hello `git add .` command.</span></span>

<span data-ttu-id="b49bc-186">hello platsen publiceras varje gång en ny uppsättning incheckningar pushas toohello plats.</span><span class="sxs-lookup"><span data-stu-id="b49bc-186">hello site is republished every time a new set of commits is pushed toohello site.</span></span>

### <span data-ttu-id="b49bc-187"><a name="howto-publish-to-azure"></a>Så här: publicera din Node.js-serverdel tooAzure</span><span class="sxs-lookup"><span data-stu-id="b49bc-187"><a name="howto-publish-to-azure"></a>How to: Publish your Node.js backend tooAzure</span></span>
<span data-ttu-id="b49bc-188">Microsoft Azure tillhandahåller många metoder för att publicera din Azure App Service Mobile Apps Node.js-serverdel för att hello Azure-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b49bc-188">Microsoft Azure provides many mechanisms for publishing your Azure App Service Mobile Apps Node.js backend to hello Azure service.</span></span>  <span data-ttu-id="b49bc-189">Dessa inkluderar använder distributionsverktyg integrerade i Visual Studio, kommandoradsverktyg och kontinuerlig distributionsalternativ baserat på källkontroll.</span><span class="sxs-lookup"><span data-stu-id="b49bc-189">These include utilizing deployment tools integrated into Visual Studio, command-line tools, and continuous deployment options based on source control.</span></span>  <span data-ttu-id="b49bc-190">Mer information om det här avsnittet finns det [Azure App Service Deployment Guide].</span><span class="sxs-lookup"><span data-stu-id="b49bc-190">For more information on this topic, see the [Azure App Service Deployment Guide].</span></span>

<span data-ttu-id="b49bc-191">Azure Apptjänst har råd för Node.js-program som du bör läsa innan du distribuerar:</span><span class="sxs-lookup"><span data-stu-id="b49bc-191">Azure App Service has specific advice for Node.js application that you should review before deploying:</span></span>

* <span data-ttu-id="b49bc-192">Hur för[ange hello nod Version]</span><span class="sxs-lookup"><span data-stu-id="b49bc-192">How too[specify hello Node Version]</span></span>
* <span data-ttu-id="b49bc-193">Hur för[använda noden moduler]</span><span class="sxs-lookup"><span data-stu-id="b49bc-193">How too[use Node modules]</span></span>

### <span data-ttu-id="b49bc-194"><a name="howto-enable-homepage"></a>Så här: aktivera en hemsida för ditt program</span><span class="sxs-lookup"><span data-stu-id="b49bc-194"><a name="howto-enable-homepage"></a>How to: Enable a Home Page for your application</span></span>
<span data-ttu-id="b49bc-195">Hej ExpressJS framework kan du toocombine två aspekter och många program är en kombination av webb- och mobilappar.</span><span class="sxs-lookup"><span data-stu-id="b49bc-195">Many applications are a combination of web and mobile apps and hello ExpressJS framework allows you toocombine the two facets.</span></span>  <span data-ttu-id="b49bc-196">Ibland kan dock gärna tooonly implementera ett mobila gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="b49bc-196">Sometimes, however, you may wish tooonly implement a mobile interface.</span></span>  <span data-ttu-id="b49bc-197">Det är användbart tooprovide en landning sidan tooensure hello app-tjänsten är igång.</span><span class="sxs-lookup"><span data-stu-id="b49bc-197">It is useful tooprovide a landing page tooensure hello app service is up and running.</span></span>  <span data-ttu-id="b49bc-198">Du kan ange en egen hemsida eller aktivera en tillfällig startsida.</span><span class="sxs-lookup"><span data-stu-id="b49bc-198">You can either provide your own home page or enable a temporary home page.</span></span>  <span data-ttu-id="b49bc-199">tooenable en tillfällig startsida, Använd följande tooinstantiate Azure Mobile Apps hello:</span><span class="sxs-lookup"><span data-stu-id="b49bc-199">tooenable a temporary home page, use hello following tooinstantiate Azure Mobile Apps:</span></span>

    var mobile = azureMobileApps({ homePage: true });

<span data-ttu-id="b49bc-200">Om du vill bara det här alternativet är tillgängligt när du utvecklar lokalt kan du lägga till den här inställningen tooyour `azureMobile.js` fil.</span><span class="sxs-lookup"><span data-stu-id="b49bc-200">If you only want this option available when developing locally, you can add this setting tooyour `azureMobile.js` file.</span></span>

## <span data-ttu-id="b49bc-201"><a name="TableOperations"></a>Åtgärder för tabeller</span><span class="sxs-lookup"><span data-stu-id="b49bc-201"><a name="TableOperations"></a>Table operations</span></span>
<span data-ttu-id="b49bc-202">hello azure mobilappar Node.js Server SDK innehåller mekanismer tooexpose data tabeller som lagras i Azure SQL Database som en WebAPI.</span><span class="sxs-lookup"><span data-stu-id="b49bc-202">hello azure-mobile-apps Node.js Server SDK provides mechanisms tooexpose data tables stored in Azure SQL Database as a WebAPI.</span></span>  <span data-ttu-id="b49bc-203">Fem tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="b49bc-203">Five operations are provided.</span></span>

| <span data-ttu-id="b49bc-204">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="b49bc-204">Operation</span></span> | <span data-ttu-id="b49bc-205">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b49bc-205">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b49bc-206">GET-/tables/*tablename*</span><span class="sxs-lookup"><span data-stu-id="b49bc-206">GET /tables/*tablename*</span></span> |<span data-ttu-id="b49bc-207">Hämta alla poster i hello tabell</span><span class="sxs-lookup"><span data-stu-id="b49bc-207">Get all records in hello table</span></span> |
| <span data-ttu-id="b49bc-208">GET-/tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="b49bc-208">GET /tables/*tablename*/:id</span></span> |<span data-ttu-id="b49bc-209">Hämta en viss post i tabellen hello</span><span class="sxs-lookup"><span data-stu-id="b49bc-209">Get a specific record in hello table</span></span> |
| <span data-ttu-id="b49bc-210">POST /tables/*tablename*</span><span class="sxs-lookup"><span data-stu-id="b49bc-210">POST /tables/*tablename*</span></span> |<span data-ttu-id="b49bc-211">Skapa en post i tabellen hello</span><span class="sxs-lookup"><span data-stu-id="b49bc-211">Create a record in hello table</span></span> |
| <span data-ttu-id="b49bc-212">KORRIGERING /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="b49bc-212">PATCH /tables/*tablename*/:id</span></span> |<span data-ttu-id="b49bc-213">Uppdatera en post i tabellen hello</span><span class="sxs-lookup"><span data-stu-id="b49bc-213">Update a record in hello table</span></span> |
| <span data-ttu-id="b49bc-214">Ta bort /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="b49bc-214">DELETE /tables/*tablename*/:id</span></span> |<span data-ttu-id="b49bc-215">Ta bort en post i tabellen hello</span><span class="sxs-lookup"><span data-stu-id="b49bc-215">Delete a record in hello table</span></span> |

<span data-ttu-id="b49bc-216">Har stöd för den här WebAPI [OData] och utökar hello tabellen schema toosupport [datasynkronisering offline].</span><span class="sxs-lookup"><span data-stu-id="b49bc-216">This WebAPI supports [OData] and extends hello table schema toosupport [offline data sync].</span></span>

### <span data-ttu-id="b49bc-217"><a name="howto-dynamicschema"></a>Så här: definiera tabeller med en dynamisk schema</span><span class="sxs-lookup"><span data-stu-id="b49bc-217"><a name="howto-dynamicschema"></a>How to: Define tables using a dynamic schema</span></span>
<span data-ttu-id="b49bc-218">Innan en tabell kan användas, måste ha definierats.</span><span class="sxs-lookup"><span data-stu-id="b49bc-218">Before a table can be used, it must be defined.</span></span>  <span data-ttu-id="b49bc-219">Tabeller kan definieras med en statisk schema (där hello developer definierar hello kolumner inom hello schemat) eller dynamiskt (där hello SDK styr hello schema baserat på inkommande begäranden).</span><span class="sxs-lookup"><span data-stu-id="b49bc-219">Tables can be defined with a static schema (where hello developer defines hello columns within hello schema) or dynamically (where hello SDK controls hello schema based on incoming requests).</span></span> <span data-ttu-id="b49bc-220">Hello utvecklare kan dessutom styra vissa aspekter av hello WebAPI genom att lägga till Javascript-kod toohello definition.</span><span class="sxs-lookup"><span data-stu-id="b49bc-220">In addition, hello developer can control specific aspects of hello WebAPI by adding Javascript code toohello definition.</span></span>

<span data-ttu-id="b49bc-221">Som bästa praxis bör du definiera varje tabell i en Javascript-fil i hello tabeller och sedan använda tables.import() metoden tooimport hello tabeller.</span><span class="sxs-lookup"><span data-stu-id="b49bc-221">As a best practice, you should define each table in a Javascript file in hello tables directory, then use the tables.import() method tooimport hello tables.</span></span>  <span data-ttu-id="b49bc-222">Utöka hello basic-app skulle hello app.js filen ställas in:</span><span class="sxs-lookup"><span data-stu-id="b49bc-222">Extending hello basic-app, hello app.js file would be adjusted:</span></span>

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define hello database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add hello mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

<span data-ttu-id="b49bc-223">Definiera hello tabellen. / tables/TodoItem.js:</span><span class="sxs-lookup"><span data-stu-id="b49bc-223">Define hello table in ./tables/TodoItem.js:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for hello table goes here

    module.exports = table;

<span data-ttu-id="b49bc-224">Dynamisk schemat använder som standard.</span><span class="sxs-lookup"><span data-stu-id="b49bc-224">Tables use dynamic schema by default.</span></span>  <span data-ttu-id="b49bc-225">tooturn av dynamisk schemat anger globalt, hello Appinställningen **MS_DynamicSchema** toofalse inom hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-225">tooturn off dynamic schema globally, set hello App Setting **MS_DynamicSchema** toofalse within hello Azure portal.</span></span>

<span data-ttu-id="b49bc-226">Du hittar ett komplett exempel i hello [todo prov på GitHub].</span><span class="sxs-lookup"><span data-stu-id="b49bc-226">You can find a complete example in hello [todo sample on GitHub].</span></span>

### <span data-ttu-id="b49bc-227"><a name="howto-staticschema"></a>Så här: definiera tabeller med en statisk schema</span><span class="sxs-lookup"><span data-stu-id="b49bc-227"><a name="howto-staticschema"></a>How to: Define tables using a static schema</span></span>
<span data-ttu-id="b49bc-228">Du kan uttryckligen definiera hello kolumner tooexpose via hello WebAPI.</span><span class="sxs-lookup"><span data-stu-id="b49bc-228">You can explicitly define hello columns tooexpose via hello WebAPI.</span></span>  <span data-ttu-id="b49bc-229">hello azure mobilappar Node.js SDK lägger automatiskt till alla andra kolumner som krävs för offlinedata toohello Synkroniseringslista som du anger.</span><span class="sxs-lookup"><span data-stu-id="b49bc-229">hello azure-mobile-apps Node.js SDK automatically adds any additional columns required for offline data sync toohello list that you provide.</span></span>  <span data-ttu-id="b49bc-230">Exempelvis klientprogram Snabbstart kräver en tabell med två kolumner: text (en sträng) och slutföra (ett booleskt värde).</span><span class="sxs-lookup"><span data-stu-id="b49bc-230">For example, the QuickStart client applications require a table with two columns: text (a string) and complete (a boolean).</span></span>  
<span data-ttu-id="b49bc-231">hello tabellen kan definieras i hello tabell definition JavaScript-fil (finns i hello tabeller directory) enligt följande:</span><span class="sxs-lookup"><span data-stu-id="b49bc-231">hello table can be defined in hello table definition JavaScript file (located in hello tables directory) as follows:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

<span data-ttu-id="b49bc-232">Om du definierar tabeller statiskt, måste du också anropa hello tables.initialize() metoden toocreate hello-databasschemat vid start.</span><span class="sxs-lookup"><span data-stu-id="b49bc-232">If you define tables statically, then you must also call hello tables.initialize() method toocreate hello database schema on startup.</span></span>  <span data-ttu-id="b49bc-233">Hej tables.initialize() metoden returnerar en [Promise] så att hello webbtjänsten inte hantera begäranden innan hello databasen initieras.</span><span class="sxs-lookup"><span data-stu-id="b49bc-233">hello tables.initialize() method returns a [Promise] so that hello web service does not serve requests before hello database being initialized.</span></span>

### <span data-ttu-id="b49bc-234"><a name="howto-sqlexpress-setup"></a>Så här: använda SQL Express som en utveckling datalager på den lokala datorn</span><span class="sxs-lookup"><span data-stu-id="b49bc-234"><a name="howto-sqlexpress-setup"></a>How to: Use SQL Express as a development data store on your local machine</span></span>
<span data-ttu-id="b49bc-235">hello Azure Mobile Apps hello AzureMobile appar nod SDK innehåller tre alternativ för betjänar data out of box hello: SDK innehåller tre alternativ för betjänar data out of box hello:</span><span class="sxs-lookup"><span data-stu-id="b49bc-235">hello Azure Mobile Apps hello AzureMobile Apps Node SDK provides three options for serving data out of hello box: SDK provides three options for serving data out of hello box:</span></span>

* <span data-ttu-id="b49bc-236">Använd hello **minne** drivrutinsarkiv tooprovide en icke-beständig exempel</span><span class="sxs-lookup"><span data-stu-id="b49bc-236">Use hello **memory** driver tooprovide a non-persistent example store</span></span>
* <span data-ttu-id="b49bc-237">Använd hello **mssql** drivrutinen tooprovide SQL Express datalagret för utveckling</span><span class="sxs-lookup"><span data-stu-id="b49bc-237">Use hello **mssql** driver tooprovide a SQL Express data store for development</span></span>
* <span data-ttu-id="b49bc-238">Använd hello **mssql** drivrutinen tooprovide en Azure SQL Database-datalagret för produktion</span><span class="sxs-lookup"><span data-stu-id="b49bc-238">Use hello **mssql** driver tooprovide an Azure SQL Database data store for production</span></span>

<span data-ttu-id="b49bc-239">hello Azure Mobile Apps Node.js SDK använder hello [mssql Node.js paketet] tooestablish och använder en anslutning tooboth SQL Express och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b49bc-239">hello Azure Mobile Apps Node.js SDK uses hello [mssql Node.js package] tooestablish and use a connection tooboth SQL Express and SQL Database.</span></span>  <span data-ttu-id="b49bc-240">Det här paketet kräver att du aktiverar TCP-anslutningar på din SQL Express-instans.</span><span class="sxs-lookup"><span data-stu-id="b49bc-240">This package requires that you enable TCP connections on your SQL Express instance.</span></span>

> [!TIP]
> <span data-ttu-id="b49bc-241">hello minne drivrutinen ger inte en fullständig uppsättning funktioner för testning.</span><span class="sxs-lookup"><span data-stu-id="b49bc-241">hello memory driver does not provide a complete set of facilities for testing.</span></span>  <span data-ttu-id="b49bc-242">Om du vill tootest serverdelen lokalt kan vi rekommenderar hello användning av ett SQL Express datalager och hello mssql drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-242">If you wish tootest your backend locally, we recommend hello use of a SQL Express data store and hello mssql driver.</span></span>
>
>

1. <span data-ttu-id="b49bc-243">Hämta och installera [Microsoft SQL Server 2014 Express].</span><span class="sxs-lookup"><span data-stu-id="b49bc-243">Download and install [Microsoft SQL Server 2014 Express].</span></span>  <span data-ttu-id="b49bc-244">Se till att du installerar SQL Server 2014 Express hello med verktyg edition.</span><span class="sxs-lookup"><span data-stu-id="b49bc-244">Ensure you install hello SQL Server 2014 Express with Tools edition.</span></span>  <span data-ttu-id="b49bc-245">Om du uttryckligen kräver stöd för 64-bitars förbrukar hello 32-bitarsversionen mindre minne när du kör.</span><span class="sxs-lookup"><span data-stu-id="b49bc-245">Unless you explicitly require 64-bit support, hello 32-bit version consumes less memory when running.</span></span>
2. <span data-ttu-id="b49bc-246">Kör hello Konfigurationshanteraren för SQL Server 2014.</span><span class="sxs-lookup"><span data-stu-id="b49bc-246">Run hello SQL Server 2014 Configuration Manager.</span></span>

   1. <span data-ttu-id="b49bc-247">Expandera hello **SQL Server-nätverkskonfigurationen** nod i hello vänstra träd-menyn.</span><span class="sxs-lookup"><span data-stu-id="b49bc-247">Expand hello **SQL Server Network Configuration** node in hello left-hand tree menu.</span></span>
   2. <span data-ttu-id="b49bc-248">Klicka på **protokoll för SQLEXPRESS**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-248">Click **Protocols for SQLEXPRESS**.</span></span>
   3. <span data-ttu-id="b49bc-249">Högerklicka på **TCP/IP** och välj **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-249">Right-click **TCP/IP** and select **Enable**.</span></span>  <span data-ttu-id="b49bc-250">Klicka på **OK** i hello popup-fönstret.</span><span class="sxs-lookup"><span data-stu-id="b49bc-250">Click **OK** in hello pop-up dialog.</span></span>
   4. <span data-ttu-id="b49bc-251">Högerklicka på **TCP/IP** och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-251">Right-click **TCP/IP** and select **Properties**.</span></span>
   5. <span data-ttu-id="b49bc-252">Klicka på hello **IP-adresser** fliken.</span><span class="sxs-lookup"><span data-stu-id="b49bc-252">Click hello **IP Addresses** tab.</span></span>
   6. <span data-ttu-id="b49bc-253">Hitta hello **IPAll** nod.</span><span class="sxs-lookup"><span data-stu-id="b49bc-253">Find hello **IPAll** node.</span></span>  <span data-ttu-id="b49bc-254">I hello **TCP-Port** anger **1433**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-254">In hello **TCP Port** field, enter **1433**.</span></span>

          ![Configure SQL Express for TCP/IP][3]
   7. <span data-ttu-id="b49bc-255">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-255">Click **OK**.</span></span>  <span data-ttu-id="b49bc-256">Klicka på **OK** i hello popup-fönstret.</span><span class="sxs-lookup"><span data-stu-id="b49bc-256">Click **OK** in hello pop-up dialog.</span></span>
   8. <span data-ttu-id="b49bc-257">Klicka på **SQL Server Services** i hello vänstra träd-menyn.</span><span class="sxs-lookup"><span data-stu-id="b49bc-257">Click **SQL Server Services** in hello left-hand tree menu.</span></span>
   9. <span data-ttu-id="b49bc-258">Högerklicka på **SQL Server (SQLEXPRESS)** och välj **starta om**</span><span class="sxs-lookup"><span data-stu-id="b49bc-258">Right-click **SQL Server (SQLEXPRESS)** and select **Restart**</span></span>
   10. <span data-ttu-id="b49bc-259">Stäng hello Konfigurationshanteraren för SQL Server 2014.</span><span class="sxs-lookup"><span data-stu-id="b49bc-259">Close hello SQL Server 2014 Configuration Manager.</span></span>
3. <span data-ttu-id="b49bc-260">Kör hello SQL Server 2014 Management Studio och Anslut tooyour lokala SQL Express-instansen</span><span class="sxs-lookup"><span data-stu-id="b49bc-260">Run hello SQL Server 2014 Management Studio and connect tooyour local SQL Express instance</span></span>

   1. <span data-ttu-id="b49bc-261">Högerklicka på din instans i hello Object Explorer och välj **egenskaper**</span><span class="sxs-lookup"><span data-stu-id="b49bc-261">Right-click your instance in hello Object Explorer and select **Properties**</span></span>
   2. <span data-ttu-id="b49bc-262">Välj hello **säkerhet** sidan.</span><span class="sxs-lookup"><span data-stu-id="b49bc-262">Select hello **Security** page.</span></span>
   3. <span data-ttu-id="b49bc-263">Se till att hello **SQL Server och Windows-autentisering läge** har valts</span><span class="sxs-lookup"><span data-stu-id="b49bc-263">Ensure hello **SQL Server and Windows Authentication mode** is selected</span></span>
   4. <span data-ttu-id="b49bc-264">Klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="b49bc-264">Click **OK**</span></span>

          ![Configure SQL Express Authentication][4]
   5. <span data-ttu-id="b49bc-265">Expandera **säkerhet** > **inloggningar** i hello Object Explorer</span><span class="sxs-lookup"><span data-stu-id="b49bc-265">Expand **Security** > **Logins** in hello Object Explorer</span></span>
   6. <span data-ttu-id="b49bc-266">Högerklicka på **inloggningar** och välj **ny inloggning...**</span><span class="sxs-lookup"><span data-stu-id="b49bc-266">Right-click **Logins** and select **New Login...**</span></span>
   7. <span data-ttu-id="b49bc-267">Ange ett inloggningsnamn.</span><span class="sxs-lookup"><span data-stu-id="b49bc-267">Enter a Login name.</span></span>  <span data-ttu-id="b49bc-268">Välj **SQL Server-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-268">Select **SQL Server authentication**.</span></span>  <span data-ttu-id="b49bc-269">Ange ett lösenord och sedan hello samma lösenord i **Bekräfta lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-269">Enter a Password, then enter hello same password in **Confirm password**.</span></span>  <span data-ttu-id="b49bc-270">hello lösenord måste uppfylla krav på komplexitet Windows.</span><span class="sxs-lookup"><span data-stu-id="b49bc-270">hello password must meet Windows complexity requirements.</span></span>
   8. <span data-ttu-id="b49bc-271">Klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="b49bc-271">Click **OK**</span></span>

          ![Add a new user tooSQL Express][5]
   9. <span data-ttu-id="b49bc-272">Högerklicka på en ny inloggning och välj **egenskaper**</span><span class="sxs-lookup"><span data-stu-id="b49bc-272">Right-click your new login and select **Properties**</span></span>
   10. <span data-ttu-id="b49bc-273">Välj hello **serverroller** sidan</span><span class="sxs-lookup"><span data-stu-id="b49bc-273">Select hello **Server Roles** page</span></span>
   11. <span data-ttu-id="b49bc-274">Kontrollera hello rutan nästa toohello **dbcreator** serverroll</span><span class="sxs-lookup"><span data-stu-id="b49bc-274">Check hello box next toohello **dbcreator** server role</span></span>
   12. <span data-ttu-id="b49bc-275">Klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="b49bc-275">Click **OK**</span></span>
   13. <span data-ttu-id="b49bc-276">Stäng hello SQL Server 2015 Management Studio</span><span class="sxs-lookup"><span data-stu-id="b49bc-276">Close hello SQL Server 2015 Management Studio</span></span>

<span data-ttu-id="b49bc-277">Se till att du poster hello användarnamn och lösenord som du har valt.</span><span class="sxs-lookup"><span data-stu-id="b49bc-277">Ensure you record hello username and password you selected.</span></span>  <span data-ttu-id="b49bc-278">Du kanske måste tooassign ytterligare serverroller eller behörigheter beroende på dina specifika krav.</span><span class="sxs-lookup"><span data-stu-id="b49bc-278">You may need tooassign additional server roles or permissions depending on your specific database requirements.</span></span>

<span data-ttu-id="b49bc-279">Hej Node.js-program läser hello **SQLCONNSTR_MS_TableConnectionString** miljövariabeln för hello anslutningssträngen för den här databasen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-279">hello Node.js application reads hello **SQLCONNSTR_MS_TableConnectionString** environment variable for hello connection string for this database.</span></span>  <span data-ttu-id="b49bc-280">Du kan ange den här variabeln i din miljö.</span><span class="sxs-lookup"><span data-stu-id="b49bc-280">You can set this variable within your environment.</span></span>  <span data-ttu-id="b49bc-281">Du kan till exempel använda PowerShell tooset den här miljövariabeln:</span><span class="sxs-lookup"><span data-stu-id="b49bc-281">For example, you can use PowerShell tooset this environment variable:</span></span>

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

<span data-ttu-id="b49bc-282">Komma åt hello databasen via en TCP/IP-anslutning och ange ett användarnamn och lösenord för hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="b49bc-282">Access hello database through a TCP/IP connection and provide a username and password for hello connection.</span></span>

### <span data-ttu-id="b49bc-283"><a name="howto-config-localdev"></a>Så här: konfigurera ditt projekt för lokal utveckling</span><span class="sxs-lookup"><span data-stu-id="b49bc-283"><a name="howto-config-localdev"></a>How to: Configure your project for local development</span></span>
<span data-ttu-id="b49bc-284">Azure Mobile Apps läser en JavaScript-fil som heter *azureMobile.js* från hello lokala filsystem.</span><span class="sxs-lookup"><span data-stu-id="b49bc-284">Azure Mobile Apps reads a JavaScript file called *azureMobile.js* from hello local filesystem.</span></span>  <span data-ttu-id="b49bc-285">Inte använda den här filen tooconfigure hello Azure Mobile Apps-SDK i produktion – använda App-inställningar i hello [Azure-portalen] i stället.</span><span class="sxs-lookup"><span data-stu-id="b49bc-285">Do not use this file tooconfigure hello Azure Mobile Apps SDK in production - use App Settings within hello [Azure portal] instead.</span></span>  <span data-ttu-id="b49bc-286">Hej *azureMobile.js* filen ska exportera ett konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="b49bc-286">hello *azureMobile.js* file should export a configuration object.</span></span>  <span data-ttu-id="b49bc-287">hello de vanligaste inställningarna är:</span><span class="sxs-lookup"><span data-stu-id="b49bc-287">hello most common settings are:</span></span>

* <span data-ttu-id="b49bc-288">Databasinställningar</span><span class="sxs-lookup"><span data-stu-id="b49bc-288">Database Settings</span></span>
* <span data-ttu-id="b49bc-289">Inställningar för diagnostikloggning</span><span class="sxs-lookup"><span data-stu-id="b49bc-289">Diagnostic Logging Settings</span></span>
* <span data-ttu-id="b49bc-290">Alternativa CORS-inställningarna</span><span class="sxs-lookup"><span data-stu-id="b49bc-290">Alternate CORS Settings</span></span>

<span data-ttu-id="b49bc-291">Ett exempel *azureMobile.js* implementera hello föregående databasen inställningarna för följande fil:</span><span class="sxs-lookup"><span data-stu-id="b49bc-291">An example *azureMobile.js* file implementing hello preceding database settings follows:</span></span>

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

<span data-ttu-id="b49bc-292">Vi rekommenderar att du lägger till *azureMobile.js* tooyour *.gitignore* fil (eller andra källkodskontroll ignorera filen) tooprevent lösenord lagras i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="b49bc-292">We recommend that you add *azureMobile.js* tooyour *.gitignore* file (or other source code control ignore file) tooprevent passwords from being stored in hello cloud.</span></span>  <span data-ttu-id="b49bc-293">Konfigurera inställningar för produktion alltid i App-inställningar i hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="b49bc-293">Always configure production settings in App Settings within hello [Azure portal].</span></span>

### <span data-ttu-id="b49bc-294"><a name="howto-appsettings"></a>Så här: Konfigurera Appinställningar för din mobila App</span><span class="sxs-lookup"><span data-stu-id="b49bc-294"><a name="howto-appsettings"></a>How: Configure App Settings for your Mobile App</span></span>
<span data-ttu-id="b49bc-295">De flesta inställningar i hello *azureMobile.js* filen har en motsvarande Appinställning i hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="b49bc-295">Most settings in hello *azureMobile.js* file have an equivalent App Setting in hello [Azure portal].</span></span>  <span data-ttu-id="b49bc-296">Använd följande lista tooconfigure hello din app i App-inställningar:</span><span class="sxs-lookup"><span data-stu-id="b49bc-296">Use hello following list tooconfigure your app in App Settings:</span></span>

| <span data-ttu-id="b49bc-297">Appinställningen</span><span class="sxs-lookup"><span data-stu-id="b49bc-297">App Setting</span></span> | <span data-ttu-id="b49bc-298">*azureMobile.js* inställning</span><span class="sxs-lookup"><span data-stu-id="b49bc-298">*azureMobile.js* Setting</span></span> | <span data-ttu-id="b49bc-299">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b49bc-299">Description</span></span> | <span data-ttu-id="b49bc-300">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="b49bc-300">Valid Values</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b49bc-301">**MS_MobileAppName**</span><span class="sxs-lookup"><span data-stu-id="b49bc-301">**MS_MobileAppName**</span></span> |<span data-ttu-id="b49bc-302">namn</span><span class="sxs-lookup"><span data-stu-id="b49bc-302">name</span></span> |<span data-ttu-id="b49bc-303">hello namnet på hello app</span><span class="sxs-lookup"><span data-stu-id="b49bc-303">hello name of hello app</span></span> |<span data-ttu-id="b49bc-304">Sträng</span><span class="sxs-lookup"><span data-stu-id="b49bc-304">string</span></span> |
| <span data-ttu-id="b49bc-305">**MS_MobileLoggingLevel**</span><span class="sxs-lookup"><span data-stu-id="b49bc-305">**MS_MobileLoggingLevel**</span></span> |<span data-ttu-id="b49bc-306">Logging.level</span><span class="sxs-lookup"><span data-stu-id="b49bc-306">logging.level</span></span> |<span data-ttu-id="b49bc-307">Lägsta loggningsnivån för meddelanden toolog</span><span class="sxs-lookup"><span data-stu-id="b49bc-307">Minimum log level of messages toolog</span></span> |<span data-ttu-id="b49bc-308">fel, varning, information, verbose,-debug, Löjliga</span><span class="sxs-lookup"><span data-stu-id="b49bc-308">error, warning, info, verbose, debug, silly</span></span> |
| <span data-ttu-id="b49bc-309">**MS_DebugMode**</span><span class="sxs-lookup"><span data-stu-id="b49bc-309">**MS_DebugMode**</span></span> |<span data-ttu-id="b49bc-310">Felsöka</span><span class="sxs-lookup"><span data-stu-id="b49bc-310">debug</span></span> |<span data-ttu-id="b49bc-311">Aktivera eller inaktivera felsökningsläge</span><span class="sxs-lookup"><span data-stu-id="b49bc-311">Enable or Disable debug mode</span></span> |<span data-ttu-id="b49bc-312">SANT, FALSKT</span><span class="sxs-lookup"><span data-stu-id="b49bc-312">true, false</span></span> |
| <span data-ttu-id="b49bc-313">**MS_TableSchema**</span><span class="sxs-lookup"><span data-stu-id="b49bc-313">**MS_TableSchema**</span></span> |<span data-ttu-id="b49bc-314">data.schema</span><span class="sxs-lookup"><span data-stu-id="b49bc-314">data.schema</span></span> |<span data-ttu-id="b49bc-315">Schemat för standardnamnet för SQL-tabeller</span><span class="sxs-lookup"><span data-stu-id="b49bc-315">Default schema name for SQL tables</span></span> |<span data-ttu-id="b49bc-316">String (standard: dbo)</span><span class="sxs-lookup"><span data-stu-id="b49bc-316">string (default: dbo)</span></span> |
| <span data-ttu-id="b49bc-317">**MS_DynamicSchema**</span><span class="sxs-lookup"><span data-stu-id="b49bc-317">**MS_DynamicSchema**</span></span> |<span data-ttu-id="b49bc-318">data.dynamicSchema</span><span class="sxs-lookup"><span data-stu-id="b49bc-318">data.dynamicSchema</span></span> |<span data-ttu-id="b49bc-319">Aktivera eller inaktivera felsökningsläge</span><span class="sxs-lookup"><span data-stu-id="b49bc-319">Enable or Disable debug mode</span></span> |<span data-ttu-id="b49bc-320">SANT, FALSKT</span><span class="sxs-lookup"><span data-stu-id="b49bc-320">true, false</span></span> |
| <span data-ttu-id="b49bc-321">**MS_DisableVersionHeader**</span><span class="sxs-lookup"><span data-stu-id="b49bc-321">**MS_DisableVersionHeader**</span></span> |<span data-ttu-id="b49bc-322">version (set tooundefined)</span><span class="sxs-lookup"><span data-stu-id="b49bc-322">version (set tooundefined)</span></span> |<span data-ttu-id="b49bc-323">Inaktiverar hello X-ZUMO-Server-Version-huvud</span><span class="sxs-lookup"><span data-stu-id="b49bc-323">Disables hello X-ZUMO-Server-Version header</span></span> |<span data-ttu-id="b49bc-324">SANT, FALSKT</span><span class="sxs-lookup"><span data-stu-id="b49bc-324">true, false</span></span> |
| <span data-ttu-id="b49bc-325">**MS_SkipVersionCheck**</span><span class="sxs-lookup"><span data-stu-id="b49bc-325">**MS_SkipVersionCheck**</span></span> |<span data-ttu-id="b49bc-326">skipversioncheck</span><span class="sxs-lookup"><span data-stu-id="b49bc-326">skipversioncheck</span></span> |<span data-ttu-id="b49bc-327">Inaktiverar hello API-version klientkontroll</span><span class="sxs-lookup"><span data-stu-id="b49bc-327">Disables hello client API version check</span></span> |<span data-ttu-id="b49bc-328">SANT, FALSKT</span><span class="sxs-lookup"><span data-stu-id="b49bc-328">true, false</span></span> |

<span data-ttu-id="b49bc-329">tooset en Appinställning:</span><span class="sxs-lookup"><span data-stu-id="b49bc-329">tooset an App Setting:</span></span>

1. <span data-ttu-id="b49bc-330">Logga in toohello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="b49bc-330">Log in toohello [Azure portal].</span></span>
2. <span data-ttu-id="b49bc-331">Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din Mobilapp.</span><span class="sxs-lookup"><span data-stu-id="b49bc-331">Select **All resources** or **App Services** then click hello name of your Mobile App.</span></span>
3. <span data-ttu-id="b49bc-332">hello inställningsbladet öppnas som standard.</span><span class="sxs-lookup"><span data-stu-id="b49bc-332">hello Settings blade opens by default.</span></span> <span data-ttu-id="b49bc-333">Om den inte, klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-333">If it doesn't, click **Settings**.</span></span>
4. <span data-ttu-id="b49bc-334">Klicka på **programinställningar** allmänna hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="b49bc-334">Click **Application settings** in hello GENERAL menu.</span></span>
5. <span data-ttu-id="b49bc-335">Rulla toohello App-inställningar.</span><span class="sxs-lookup"><span data-stu-id="b49bc-335">Scroll toohello App Settings section.</span></span>
6. <span data-ttu-id="b49bc-336">Om din app inställningen redan finns på hello värdet av hello app tooedit hello värdet.</span><span class="sxs-lookup"><span data-stu-id="b49bc-336">If your app setting already exists, click hello value of hello app setting tooedit hello value.</span></span>
7. <span data-ttu-id="b49bc-337">Om din appinställning inte finns, ange hello Appinställningen i hello nyckel och hello värde i rutan för hello-värde.</span><span class="sxs-lookup"><span data-stu-id="b49bc-337">If your app setting does not exist, enter hello App Setting in hello Key box and hello value in hello Value box.</span></span>
8. <span data-ttu-id="b49bc-338">När du är klar klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-338">Once you are complete, click **Save**.</span></span>

<span data-ttu-id="b49bc-339">Ändrar appinställningar för de flesta måste tjänsten startas om.</span><span class="sxs-lookup"><span data-stu-id="b49bc-339">Changing most app settings requires a service restart.</span></span>

### <span data-ttu-id="b49bc-340"><a name="howto-use-sqlazure"></a>Så här: använda SQL-databas som datalager produktion</span><span class="sxs-lookup"><span data-stu-id="b49bc-340"><a name="howto-use-sqlazure"></a>How to: Use SQL Database as your production data store</span></span>
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

<span data-ttu-id="b49bc-341">Med Azure SQL Database som ett datalager är identisk för alla typer i Azure App Service-programmet.</span><span class="sxs-lookup"><span data-stu-id="b49bc-341">Using Azure SQL Database as a data store is identical across all Azure App Service application types.</span></span> <span data-ttu-id="b49bc-342">Om du redan inte har gjort det, följer du dessa steg toocreate en mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="b49bc-342">If you have not done so already, follow these steps toocreate a Mobile App backend.</span></span>

1. <span data-ttu-id="b49bc-343">Logga in toohello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="b49bc-343">Log in toohello [Azure portal].</span></span>
2. <span data-ttu-id="b49bc-344">I hello upp till vänster i hello-fönstret klickar du på hello **+ ny** knappen > **webb + mobilt** > **Mobilapp**, ange ett namn för din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="b49bc-344">In hello top left of hello window, click hello **+NEW** button > **Web + Mobile** > **Mobile App**, then provide a name for your Mobile App backend.</span></span>
3. <span data-ttu-id="b49bc-345">I hello **resursgruppen** ange hello samma namn som din app.</span><span class="sxs-lookup"><span data-stu-id="b49bc-345">In hello **Resource Group** box, enter hello same name as your app.</span></span>
4. <span data-ttu-id="b49bc-346">hello standard programtjänstplanen har valts.</span><span class="sxs-lookup"><span data-stu-id="b49bc-346">hello Default App Service plan is selected.</span></span>  <span data-ttu-id="b49bc-347">Om du vill toochange App Service-plan kan du göra det genom att klicka på hello Apptjänstplan > **+ skapa nya**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-347">If you wish toochange your App Service plan, you can do so by clicking hello App Service Plan > **+ Create New**.</span></span>  <span data-ttu-id="b49bc-348">Ange ett namn på hello nya App Service-plan och välj en lämplig plats.</span><span class="sxs-lookup"><span data-stu-id="b49bc-348">Provide a name of hello new App Service plan and select an appropriate location.</span></span>  <span data-ttu-id="b49bc-349">Klicka på hello prisnivå och välj en lämplig prisnivån för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b49bc-349">Click hello Pricing tier and select an appropriate pricing tier for hello service.</span></span> <span data-ttu-id="b49bc-350">Välj **visa alla** tooview priser fler alternativ, exempelvis **lediga** och **delade**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-350">Select **View all** tooview more pricing options, such as **Free** and **Shared**.</span></span>  <span data-ttu-id="b49bc-351">När du har valt prisnivå, klickar du på hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-351">Once you have selected the pricing tier, click hello **Select** button.</span></span>  <span data-ttu-id="b49bc-352">Tillbaka i hello **programtjänstplanen** bladet, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-352">Back in hello **App Service plan** blade, click **OK**.</span></span>
5. <span data-ttu-id="b49bc-353">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-353">Click **Create**.</span></span> <span data-ttu-id="b49bc-354">Etablering av en mobilappsserverdel kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="b49bc-354">Provisioning a Mobile App backend can take a couple of minutes.</span></span>  <span data-ttu-id="b49bc-355">När hello mobilappsserverdel etableras hello portalen öppnar hello **inställningar** bladet för hello mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="b49bc-355">Once hello Mobile App backend is provisioned, hello portal opens hello **Settings** blade for hello Mobile App backend.</span></span>

<span data-ttu-id="b49bc-356">När hello mobilappsserverdel har skapats kan du välja tooeither Anslut en befintlig SQL-databas tooyour mobilappsserverdel eller skapa en ny SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b49bc-356">Once hello Mobile App backend is created, you can choose tooeither connect an existing SQL database tooyour Mobile App backend or create a new SQL database.</span></span>  <span data-ttu-id="b49bc-357">I det här avsnittet skapar vi en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b49bc-357">In this section, we create a SQL database.</span></span>

> [!NOTE]
> <span data-ttu-id="b49bc-358">Om du redan har en databas i hello samma plats som hello mobilappsserverdel kan du istället välja **Använd en befintlig databas** och välj sedan den databasen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-358">If you already have a database in hello same location as hello mobile app backend, you can instead choose **Use an existing database** and then select that database.</span></span> <span data-ttu-id="b49bc-359">Det rekommenderas inte hello användning av en databas på en annan plats på grund av ökad latens.</span><span class="sxs-lookup"><span data-stu-id="b49bc-359">hello use of a database in a different location is not recommended because of higher latencies.</span></span>
>
>

1. <span data-ttu-id="b49bc-360">I hello ny mobilappsserverdel, klickar du på **inställningar** > **Mobilapp** > **Data** > **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-360">In hello new Mobile App backend, click **Settings** > **Mobile App** > **Data** > **+Add**.</span></span>
2. <span data-ttu-id="b49bc-361">I hello **Lägg till dataanslutning** bladet, klickar du på **SQL Database - konfigurera nödvändiga inställningar** > **skapa en ny databas**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-361">In hello **Add data connection** blade, click **SQL Database - Configure required settings** > **Create a new database**.</span></span>  <span data-ttu-id="b49bc-362">Ange hello namn hello ny databas i hello **namn** fältet.</span><span class="sxs-lookup"><span data-stu-id="b49bc-362">Enter hello name of hello new database in hello **Name** field.</span></span>
3. <span data-ttu-id="b49bc-363">Klicka på **Server**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-363">Click **Server**.</span></span>  <span data-ttu-id="b49bc-364">I hello **ny server** bladet, ange ett unikt servernamn i hello **servernamn** fältet och ange ett lämpligt **inloggning för serveradministratör** och **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-364">In hello **New server** blade, enter a unique server name in hello **Server name** field, and provide a suitable **Server admin login** and **Password**.</span></span>  <span data-ttu-id="b49bc-365">Se till att **Tillåt azure-tjänster tooaccess server** är markerad.</span><span class="sxs-lookup"><span data-stu-id="b49bc-365">Ensure **Allow azure services tooaccess server** is checked.</span></span>  <span data-ttu-id="b49bc-366">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-366">Click **OK**.</span></span>

    ![Skapa en Azure SQL Database][6]
4. <span data-ttu-id="b49bc-368">På hello **ny databas** bladet, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-368">On hello **New database** blade, click **OK**.</span></span>
5. <span data-ttu-id="b49bc-369">Tillbaka på hello **Lägg till dataanslutning** bladet väljer **anslutningssträngen**, ange hello inloggningsnamn och lösenord som du angav när du skapar hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-369">Back on hello **Add data connection** blade, select **Connection string**, enter hello login and password that you provided when creating hello database.</span></span>  <span data-ttu-id="b49bc-370">Ange hello inloggningsuppgifterna för databasen om du använder en befintlig databas.</span><span class="sxs-lookup"><span data-stu-id="b49bc-370">If you use an existing database, provide hello login credentials for that database.</span></span>  <span data-ttu-id="b49bc-371">När du har angett, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-371">Once entered, click **OK**.</span></span>
6. <span data-ttu-id="b49bc-372">Tillbaka på hello **Lägg till dataanslutning** bladet Klicka på **OK** toocreate hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-372">Back on hello **Add data connection** blade again, click **OK** toocreate hello database.</span></span>

<!--- END OF ALTERNATE INCLUDE -->

<span data-ttu-id="b49bc-373">Hello databasen kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="b49bc-373">Creation of hello database can take a few minutes.</span></span>  <span data-ttu-id="b49bc-374">Använd hello **meddelanden** område toomonitor hello fortskrider hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="b49bc-374">Use hello **Notifications** area toomonitor hello progress of hello deployment.</span></span>  <span data-ttu-id="b49bc-375">Passera inte förrän hello databas har distribuerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="b49bc-375">Do not progress until hello database has been deployed successfully.</span></span>  <span data-ttu-id="b49bc-376">När har distribuerats, skapas en anslutningssträng för hello SQL Database-instans i din mobila serverdel App-inställningar.</span><span class="sxs-lookup"><span data-stu-id="b49bc-376">Once successfully deployed, a Connection String is created for hello SQL Database instance in your Mobile backend App Settings.</span></span>  <span data-ttu-id="b49bc-377">Du kan se den här appen inställningen i hello **inställningar** > **programinställningar** > **anslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-377">You can see this app setting in hello **Settings** > **Application settings** > **Connection strings**.</span></span>

### <span data-ttu-id="b49bc-378"><a name="howto-tables-auth"></a>Så här: kräver autentisering för åtkomst tootables</span><span class="sxs-lookup"><span data-stu-id="b49bc-378"><a name="howto-tables-auth"></a>How to: Require authentication for access tootables</span></span>
<span data-ttu-id="b49bc-379">Om du vill toouse App Service-autentisering med hello tabeller slutpunkt måste du konfigurera App Service-autentisering i hello [Azure-portalen] första.</span><span class="sxs-lookup"><span data-stu-id="b49bc-379">If you wish toouse App Service Authentication with hello tables endpoint, you must configure App Service Authentication in hello [Azure portal] first.</span></span>  <span data-ttu-id="b49bc-380">För mer information om hur du konfigurerar autentisering i Azure App Service, granska hello konfigurationsguiden för hello identitetsleverantör avser toouse:</span><span class="sxs-lookup"><span data-stu-id="b49bc-380">For more details about configuring authentication in an Azure App Service, review hello Configuration Guide for hello identity provider you intend toouse:</span></span>

* <span data-ttu-id="b49bc-381">[Hur tooconfigure Azure Active Directory-autentisering]</span><span class="sxs-lookup"><span data-stu-id="b49bc-381">[How tooconfigure Azure Active Directory Authentication]</span></span>
* <span data-ttu-id="b49bc-382">[Hur tooconfigure Facebook-autentisering]</span><span class="sxs-lookup"><span data-stu-id="b49bc-382">[How tooconfigure Facebook Authentication]</span></span>
* <span data-ttu-id="b49bc-383">[Hur tooconfigure Google-autentisering]</span><span class="sxs-lookup"><span data-stu-id="b49bc-383">[How tooconfigure Google Authentication]</span></span>
* <span data-ttu-id="b49bc-384">[Hur tooconfigure Microsoft Authentication]</span><span class="sxs-lookup"><span data-stu-id="b49bc-384">[How tooconfigure Microsoft Authentication]</span></span>
* <span data-ttu-id="b49bc-385">[Hur tooconfigure Twitter-autentisering]</span><span class="sxs-lookup"><span data-stu-id="b49bc-385">[How tooconfigure Twitter Authentication]</span></span>

<span data-ttu-id="b49bc-386">Varje tabell har en åtkomst-egenskap som kan använda toocontrol access toohello-tabell.</span><span class="sxs-lookup"><span data-stu-id="b49bc-386">Each table has an access property that can be used toocontrol access toohello table.</span></span>  <span data-ttu-id="b49bc-387">hello följande exempel visas en statiskt definierade tabell med autentisering krävs.</span><span class="sxs-lookup"><span data-stu-id="b49bc-387">hello following sample shows a statically defined table with authentication required.</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="b49bc-388">hello åtkomst egenskapen kan ha ett av tre värden</span><span class="sxs-lookup"><span data-stu-id="b49bc-388">hello access property can take one of three values</span></span>

* <span data-ttu-id="b49bc-389">*anonym* anger hello klientprogrammet tillåts tooread data utan autentisering</span><span class="sxs-lookup"><span data-stu-id="b49bc-389">*anonymous* indicates that hello client application is allowed tooread data without authentication</span></span>
* <span data-ttu-id="b49bc-390">*autentiserad* anger att hello-klientprogrammet måste skicka en giltig autentiserings-token med hello begäran</span><span class="sxs-lookup"><span data-stu-id="b49bc-390">*authenticated* indicates that hello client application must send a valid authentication token with hello request</span></span>
* <span data-ttu-id="b49bc-391">*inaktiverad* anger att den här tabellen är för närvarande inaktiverat</span><span class="sxs-lookup"><span data-stu-id="b49bc-391">*disabled* indicates that this table is currently disabled</span></span>

<span data-ttu-id="b49bc-392">Om hello åtkomst egenskapen är odefinierad tillåts obehörig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b49bc-392">If hello access property is undefined, unauthenticated access is allowed.</span></span>

### <span data-ttu-id="b49bc-393"><a name="howto-tables-getidentity"></a>Så här: använda anspråk för autentisering med tabeller</span><span class="sxs-lookup"><span data-stu-id="b49bc-393"><a name="howto-tables-getidentity"></a>How to: Use authentication claims with your tables</span></span>
<span data-ttu-id="b49bc-394">Du kan ställa in olika anspråk som begärs när autentisering har ställts in.</span><span class="sxs-lookup"><span data-stu-id="b49bc-394">You can set up various claims that are requested when authentication is set up.</span></span>  <span data-ttu-id="b49bc-395">Dessa anspråk inte är normalt tillgängliga via hello `context.user` objekt.</span><span class="sxs-lookup"><span data-stu-id="b49bc-395">These claims are not normally available through hello `context.user` object.</span></span>  <span data-ttu-id="b49bc-396">Men de kan hämtas med hjälp av hello `context.user.getIdentity()` metod.</span><span class="sxs-lookup"><span data-stu-id="b49bc-396">However, they can be retrieved using hello `context.user.getIdentity()` method.</span></span>  <span data-ttu-id="b49bc-397">Hej `getIdentity()` metoden returnerar en Promise som löser tooan objekt.</span><span class="sxs-lookup"><span data-stu-id="b49bc-397">hello `getIdentity()` method returns a Promise that resolves tooan object.</span></span>  <span data-ttu-id="b49bc-398">hello-objekt aktiveras av autentiseringsmetod (facebook, google, twitter, microsoftaccount eller aad).</span><span class="sxs-lookup"><span data-stu-id="b49bc-398">hello object is keyed by the authentication method (facebook, google, twitter, microsoftaccount, or aad).</span></span>

<span data-ttu-id="b49bc-399">Om du konfigurerar Account autentisering och begäran hello e-postadresser anspråk du lägga till hello e-post toohello med hello efter tabellen domänkontrollant:</span><span class="sxs-lookup"><span data-stu-id="b49bc-399">For example, if you set up Microsoft Account authentication and request hello email addresses claim, you can add hello email address toohello record with hello following table controller:</span></span>

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
    * Limit hello context query toothose records with hello authenticated user email address
    * @param {Context} context hello operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds hello email address from hello claims toohello context item - used for
    * insert operations
    * @param {Context} context hello operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when hello client does a request
    // READ - only return records belonging toohello authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite hello userId based on hello authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong toohello authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong toohello authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

<span data-ttu-id="b49bc-400">toosee vilka anspråk är tillgängliga, använda en web webbläsare tooview hello `/.auth/me` slutpunkten för din webbplats.</span><span class="sxs-lookup"><span data-stu-id="b49bc-400">toosee what claims are available, use a web browser tooview hello `/.auth/me` endpoint of your site.</span></span>

### <span data-ttu-id="b49bc-401"><a name="howto-tables-disabled"></a>Så här: inaktivera åtkomståtgärder toospecific tabell</span><span class="sxs-lookup"><span data-stu-id="b49bc-401"><a name="howto-tables-disabled"></a>How to: Disable access toospecific table operations</span></span>
<span data-ttu-id="b49bc-402">I tillägg tooappearing för hello tabellen vara hello åtkomst egenskapen används toocontrol enskilda åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b49bc-402">In addition tooappearing on hello table, hello access property can be used toocontrol individual operations.</span></span>  <span data-ttu-id="b49bc-403">Det finns fyra åtgärder:</span><span class="sxs-lookup"><span data-stu-id="b49bc-403">There are four operations:</span></span>

* <span data-ttu-id="b49bc-404">*läsa* är hello RESTful GET-åtgärd på hello tabell</span><span class="sxs-lookup"><span data-stu-id="b49bc-404">*read* is hello RESTful GET operation on hello table</span></span>
* <span data-ttu-id="b49bc-405">*Infoga* hello RESTful efter åtgärd för hello tabellen</span><span class="sxs-lookup"><span data-stu-id="b49bc-405">*insert* is hello RESTful POST operation on hello table</span></span>
* <span data-ttu-id="b49bc-406">*Uppdatera* hello RESTful korrigering åtgärd på hello tabell</span><span class="sxs-lookup"><span data-stu-id="b49bc-406">*update* is hello RESTful PATCH operation on hello table</span></span>
* <span data-ttu-id="b49bc-407">*ta bort* hello RESTful ta bort åtgärd för hello tabellen</span><span class="sxs-lookup"><span data-stu-id="b49bc-407">*delete* is hello RESTful DELETE operation on hello table</span></span>

<span data-ttu-id="b49bc-408">Till exempel du tooprovide en skrivskyddad oautentiserade tabell:</span><span class="sxs-lookup"><span data-stu-id="b49bc-408">For example, you may wish tooprovide a read-only unauthenticated table:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <span data-ttu-id="b49bc-409"><a name="howto-tables-query"></a>Så här: justera hello-fråga som ska användas med åtgärder för tabeller</span><span class="sxs-lookup"><span data-stu-id="b49bc-409"><a name="howto-tables-query"></a>How to: Adjust hello query that is used with table operations</span></span>
<span data-ttu-id="b49bc-410">Ett vanligt krav för tabellåtgärder är tooprovide en begränsad vy av hello data.</span><span class="sxs-lookup"><span data-stu-id="b49bc-410">A common requirement for table operations is tooprovide a restricted view of hello data.</span></span>  <span data-ttu-id="b49bc-411">Du kan till exempel ange en tabell som är märkta med hello autentiserat användar-ID så att du kan bara läsa eller uppdatera dina egna poster.</span><span class="sxs-lookup"><span data-stu-id="b49bc-411">For example, you may provide a table that is tagged with hello authenticated user ID such that you can only read or update your own records.</span></span>  <span data-ttu-id="b49bc-412">hello följande tabelldefinitionen innehåller den här funktionen:</span><span class="sxs-lookup"><span data-stu-id="b49bc-412">hello following table definition provides this functionality:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for hello table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for hello authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite hello userId with hello authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

<span data-ttu-id="b49bc-413">Åtgärder som normalt köra en fråga har en fråga-egenskap som du kan justera med where satsen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-413">Operations that normally execute a query have a query property that you can adjust with a where clause.</span></span> <span data-ttu-id="b49bc-414">hello fråga egenskapen är en [QueryJS] objekt som används tooconvert toosomething en OData-frågan som hello data backend kan bearbeta.</span><span class="sxs-lookup"><span data-stu-id="b49bc-414">hello query property is a [QueryJS] object that is used tooconvert an OData query toosomething that hello data backend can process.</span></span>  <span data-ttu-id="b49bc-415">En mappning kan användas för enkla likheten fall (hello före en).</span><span class="sxs-lookup"><span data-stu-id="b49bc-415">For simple equality cases (like hello preceding one), a map can be used.</span></span> <span data-ttu-id="b49bc-416">Du kan också lägga till specifika SQL-satser:</span><span class="sxs-lookup"><span data-stu-id="b49bc-416">You can also add specific SQL clauses:</span></span>

    context.query.where('myfield eq ?', 'value');

### <span data-ttu-id="b49bc-417"><a name="howto-tables-softdelete"></a>Så här: Konfigurera mjuk borttagning för en tabell</span><span class="sxs-lookup"><span data-stu-id="b49bc-417"><a name="howto-tables-softdelete"></a>How to: Configure soft delete on a table</span></span>
<span data-ttu-id="b49bc-418">Mjuk borttagning faktiskt bort inte poster.</span><span class="sxs-lookup"><span data-stu-id="b49bc-418">Soft Delete does not actually delete records.</span></span>  <span data-ttu-id="b49bc-419">I stället märks de som borttagna i hello-databas genom att ange hello bort kolumnen tootrue.</span><span class="sxs-lookup"><span data-stu-id="b49bc-419">Instead it marks them as deleted within hello database by setting hello deleted column tootrue.</span></span>  <span data-ttu-id="b49bc-420">hello Azure Mobile Apps-SDK tar automatiskt bort ej permanent borttagna poster från resultat om hello Mobile klient-SDK använder IncludeDeleted().</span><span class="sxs-lookup"><span data-stu-id="b49bc-420">hello Azure Mobile Apps SDK automatically removes soft-deleted records from results unless hello Mobile Client SDK uses IncludeDeleted().</span></span>  <span data-ttu-id="b49bc-421">en tabell för mjuk bort tooconfigure ange hello `softDelete` egenskap i definitionsfilen för hello tabell:</span><span class="sxs-lookup"><span data-stu-id="b49bc-421">tooconfigure a table for soft delete, set hello `softDelete` property in hello table definition file:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="b49bc-422">Du bör fastställa en mekanism för att rensa poster – antingen från ett klientprogram, via ett Webbjobb Azure-funktion eller via en anpassad API.</span><span class="sxs-lookup"><span data-stu-id="b49bc-422">You should establish a mechanism for purging records - either from a client application, via a WebJob, Azure Function or through a custom API.</span></span>

### <span data-ttu-id="b49bc-423"><a name="howto-tables-seeding"></a>Så här: startvärde för databasen med data</span><span class="sxs-lookup"><span data-stu-id="b49bc-423"><a name="howto-tables-seeding"></a>How to: Seed your database with data</span></span>
<span data-ttu-id="b49bc-424">När du skapar ett nytt program kan du tooseed en tabell med data.</span><span class="sxs-lookup"><span data-stu-id="b49bc-424">When creating a new application, you may wish tooseed a table with data.</span></span>  <span data-ttu-id="b49bc-425">Detta kan göras i hello tabell definition JavaScript-fil på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b49bc-425">This can be done within hello table definition JavaScript file as follows:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
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

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="b49bc-426">Dirigering av data utförs endast när hello tabell skapas av hello Azure Mobile Apps-SDK.</span><span class="sxs-lookup"><span data-stu-id="b49bc-426">Seeding of data is only done when hello table is created by hello Azure Mobile Apps SDK.</span></span>  <span data-ttu-id="b49bc-427">Om hello tabellen redan finns i hello databas injekteras inga data i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="b49bc-427">If hello table already exists within hello database, no data is injected into hello table.</span></span>  <span data-ttu-id="b49bc-428">Om dynamiska schemat är aktiverat härleda schemat från hello dirigeras data.</span><span class="sxs-lookup"><span data-stu-id="b49bc-428">If dynamic schema is turned on, then the schema is inferred from hello seeded data.</span></span>

<span data-ttu-id="b49bc-429">Vi rekommenderar att du explicit anropa hello `tables.initialize()` metoden toocreate hello tabellen när hello-tjänsten startar.</span><span class="sxs-lookup"><span data-stu-id="b49bc-429">We recommend that you explicitly call hello `tables.initialize()` method toocreate hello table when hello service starts running.</span></span>

### <span data-ttu-id="b49bc-430"><a name="Swagger"></a>Så här: aktivera stöd för Swagger</span><span class="sxs-lookup"><span data-stu-id="b49bc-430"><a name="Swagger"></a>How to: Enable Swagger support</span></span>
<span data-ttu-id="b49bc-431">Azure Apptjänst Mobilappar medföljer inbyggda [Swagger] stöd.</span><span class="sxs-lookup"><span data-stu-id="b49bc-431">Azure App Service Mobile Apps comes with built-in [Swagger] support.</span></span>  <span data-ttu-id="b49bc-432">tooenable Swagger-support, kan du först installera hello swagger-användargränssnitt som ett beroende:</span><span class="sxs-lookup"><span data-stu-id="b49bc-432">tooenable Swagger support, first install hello swagger-ui as a dependency:</span></span>

    npm install --save swagger-ui

<span data-ttu-id="b49bc-433">När du har installerat kan du aktivera stöd för Swagger i hello Azure Mobile Apps-konstruktorn:</span><span class="sxs-lookup"><span data-stu-id="b49bc-433">Once installed, you can enable Swagger support in hello Azure Mobile Apps constructor:</span></span>

    var mobile = azureMobileApps({ swagger: true });

<span data-ttu-id="b49bc-434">Du förmodligen bara vill tooenable Swagger-stöd i development utgåvor.</span><span class="sxs-lookup"><span data-stu-id="b49bc-434">You probably only want tooenable Swagger support in development editions.</span></span>  <span data-ttu-id="b49bc-435">Du kan göra detta genom att använda den `NODE_ENV` appinställningen:</span><span class="sxs-lookup"><span data-stu-id="b49bc-435">You can do this by utilizing the `NODE_ENV` app setting:</span></span>

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

<span data-ttu-id="b49bc-436">Hej swagger-slutpunkt finns på http://*yoursite*.azurewebsites.net/swagger.</span><span class="sxs-lookup"><span data-stu-id="b49bc-436">hello swagger endpoint is located at http://*yoursite*.azurewebsites.net/swagger.</span></span>  <span data-ttu-id="b49bc-437">Du kan komma åt hello Swagger-användargränssnitt via hello `/swagger/ui` slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="b49bc-437">You can access hello Swagger UI via hello `/swagger/ui` endpoint.</span></span>  <span data-ttu-id="b49bc-438">Om du väljer toorequire autentisering över hela programmet genererar Swagger ett fel.</span><span class="sxs-lookup"><span data-stu-id="b49bc-438">if you choose toorequire authentication across your entire application, Swagger produces an error.</span></span>  <span data-ttu-id="b49bc-439">Välj tooallow oautentiserade begäranden via för bästa resultat i hello Azure App Service autentisering / auktoriseringsinställningar därefter kontrollera autentisering med hjälp av hello `table.access` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-439">For best results, choose tooallow unauthenticated requests through in hello Azure App Service Authentication / Authorization settings, then control authentication using hello `table.access` property.</span></span>

<span data-ttu-id="b49bc-440">Du kan också lägga till hello Swagger alternativet tooyour `azureMobile.js` filen om du bara vill Swagger-stöd när du utvecklar lokalt.</span><span class="sxs-lookup"><span data-stu-id="b49bc-440">You can also add hello Swagger option tooyour `azureMobile.js` file if you only want Swagger support when developing locally.</span></span>

## <a name="a-namepushpush-notifications"></a><span data-ttu-id="b49bc-441"><a name="push">Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="b49bc-441"><a name="push">Push notifications</span></span>
<span data-ttu-id="b49bc-442">Mobile Apps integreras med Azure Notification Hubs tooenable du toosend riktade push-meddelanden toomillions enheter alla större plattformar.</span><span class="sxs-lookup"><span data-stu-id="b49bc-442">Mobile Apps integrates with Azure Notification Hubs tooenable you toosend targeted push notifications toomillions of devices across all major platforms.</span></span> <span data-ttu-id="b49bc-443">Genom att använda Notification Hubs kan du skicka push meddelanden tooiOS, Android och Windows-enheter.</span><span class="sxs-lookup"><span data-stu-id="b49bc-443">By using Notification Hubs, you can send push notifications tooiOS, Android and Windows devices.</span></span> <span data-ttu-id="b49bc-444">toolearn mer om allt som du kan göra med Notification Hubs finns [översikt över Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b49bc-444">toolearn more about all that you can do with Notification Hubs, see [Notification Hubs Overview](../notification-hubs/notification-hubs-push-notification-overview.md).</span></span>

### <span data-ttu-id="b49bc-445"></a><a name="send-push"></a>Så här: skicka push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="b49bc-445"></a><a name="send-push"></a>How to: Send push notifications</span></span>
<span data-ttu-id="b49bc-446">hello följande kod visar hur toouse hello push objektet toosend en sändning push notification tooregistered iOS-enheter:</span><span class="sxs-lookup"><span data-stu-id="b49bc-446">hello following code shows how toouse hello push object toosend a broadcast push notification tooregistered iOS devices:</span></span>

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do hello push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }

<span data-ttu-id="b49bc-447">Du kan i stället skicka mallen push meddelandet toodevices på alla plattformar som stöds genom att skapa en mall för push-registrering från hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="b49bc-447">By creating a template push registration from hello client, you can instead send a template push message toodevices on all supported platforms.</span></span> <span data-ttu-id="b49bc-448">Hej följande kod visar hur toosend ett mall-meddelande:</span><span class="sxs-lookup"><span data-stu-id="b49bc-448">hello following code shows how toosend a template notification:</span></span>

    // Define hello template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do hello push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }


### <span data-ttu-id="b49bc-449"><a name="push-user"></a>Så här: skicka push-meddelanden tooan autentiserade användare med hjälp av taggar</span><span class="sxs-lookup"><span data-stu-id="b49bc-449"><a name="push-user"></a>How to: Send push notifications tooan authenticated user using tags</span></span>
<span data-ttu-id="b49bc-450">När en autentiserad användare registrerar för push-meddelanden, läggs en tagg för användar-ID automatiskt toohello registrering.</span><span class="sxs-lookup"><span data-stu-id="b49bc-450">When an authenticated user registers for push notifications, a user ID tag is automatically added toohello registration.</span></span> <span data-ttu-id="b49bc-451">Du kan skicka meddelanden tooall enheter som har registrerats av en viss användare push med hjälp av den här taggen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-451">By using this tag, you can send push notifications tooall devices registered by a specific user.</span></span> <span data-ttu-id="b49bc-452">hello följande kod hämtar hello SID för användaren att göra hello begäran och skickar en mall registreringen push-meddelanden tooevery enhet för användaren:</span><span class="sxs-lookup"><span data-stu-id="b49bc-452">hello following code gets hello SID of user making hello request and sends a template push notification tooevery device registration for that user:</span></span>

    // Only do hello push if configured
    if (context.push) {
        // Send a notification toohello current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }

<span data-ttu-id="b49bc-453">När du registrerar dig för push-meddelanden från en autentiserad klient, se till att autentiseringen har slutförts innan du försöker utföra registreringen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-453">When registering for push notifications from an authenticated client, make sure that authentication is complete before attempting registration.</span></span>

## <span data-ttu-id="b49bc-454"><a name="CustomAPI"></a>Anpassade API: er</span><span class="sxs-lookup"><span data-stu-id="b49bc-454"><a name="CustomAPI"></a> Custom APIs</span></span>
### <span data-ttu-id="b49bc-455"><a name="howto-customapi-basic"></a>Så här: definiera en anpassad API</span><span class="sxs-lookup"><span data-stu-id="b49bc-455"><a name="howto-customapi-basic"></a>How to: Define a custom API</span></span>
<span data-ttu-id="b49bc-456">Dessutom toohello dataåtkomst API via hello /tables slutpunkt, Azure Mobile Apps kan tillhandahålla anpassade API täckning.</span><span class="sxs-lookup"><span data-stu-id="b49bc-456">In addition toohello data access API via hello /tables endpoint, Azure Mobile Apps can provide custom API coverage.</span></span>  <span data-ttu-id="b49bc-457">Anpassade API: er som definieras i ett liknande sätt toohello definitioner och kan komma åt alla hello samma anläggningar, inklusive autentisering.</span><span class="sxs-lookup"><span data-stu-id="b49bc-457">Custom APIs are defined in a similar way toohello table definitions and can access all hello same facilities, including authentication.</span></span>

<span data-ttu-id="b49bc-458">Om du vill toouse App Service-autentisering med en anpassad API måste du konfigurera App Service-autentisering i hello [Azure-portalen] första.</span><span class="sxs-lookup"><span data-stu-id="b49bc-458">If you wish toouse App Service Authentication with a Custom API, you must configure App Service Authentication in hello [Azure portal] first.</span></span>  <span data-ttu-id="b49bc-459">För mer information om hur du konfigurerar autentisering i Azure App Service, granska hello konfigurationsguiden för hello identitetsleverantör avser toouse:</span><span class="sxs-lookup"><span data-stu-id="b49bc-459">For more details about configuring authentication in an Azure App Service, review hello Configuration Guide for hello identity provider you intend toouse:</span></span>

* <span data-ttu-id="b49bc-460">[Hur tooconfigure Azure Active Directory-autentisering]</span><span class="sxs-lookup"><span data-stu-id="b49bc-460">[How tooconfigure Azure Active Directory Authentication]</span></span>
* <span data-ttu-id="b49bc-461">[Hur tooconfigure Facebook-autentisering]</span><span class="sxs-lookup"><span data-stu-id="b49bc-461">[How tooconfigure Facebook Authentication]</span></span>
* <span data-ttu-id="b49bc-462">[Hur tooconfigure Google-autentisering]</span><span class="sxs-lookup"><span data-stu-id="b49bc-462">[How tooconfigure Google Authentication]</span></span>
* <span data-ttu-id="b49bc-463">[Hur tooconfigure Microsoft Authentication]</span><span class="sxs-lookup"><span data-stu-id="b49bc-463">[How tooconfigure Microsoft Authentication]</span></span>
* <span data-ttu-id="b49bc-464">[Hur tooconfigure Twitter-autentisering]</span><span class="sxs-lookup"><span data-stu-id="b49bc-464">[How tooconfigure Twitter Authentication]</span></span>

<span data-ttu-id="b49bc-465">Anpassade API: er som är definierade i mycket hello samma sätt som hello tabeller API.</span><span class="sxs-lookup"><span data-stu-id="b49bc-465">Custom APIs are defined in much hello same way as hello Tables API.</span></span>

1. <span data-ttu-id="b49bc-466">Skapa en **api** directory</span><span class="sxs-lookup"><span data-stu-id="b49bc-466">Create an **api** directory</span></span>
2. <span data-ttu-id="b49bc-467">Skapa en API-definitionen JavaScript-fil i hello **api** directory.</span><span class="sxs-lookup"><span data-stu-id="b49bc-467">Create an API definition JavaScript file in hello **api** directory.</span></span>
3. <span data-ttu-id="b49bc-468">Använd hello importera metoden tooimport hello **api** directory.</span><span class="sxs-lookup"><span data-stu-id="b49bc-468">Use hello import method tooimport hello **api** directory.</span></span>

<span data-ttu-id="b49bc-469">Här är hello prototyp api-definition baserat på hello basic-app sampel som vi använde tidigare.</span><span class="sxs-lookup"><span data-stu-id="b49bc-469">Here is hello prototype api definition based on hello basic-app sample we used earlier.</span></span>

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import hello Custom API
    mobile.api.import('./api');

    // Add hello mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

<span data-ttu-id="b49bc-470">Låt oss ta ett exempel API som returnerar hello server datum med hello *Date.now()* metod.</span><span class="sxs-lookup"><span data-stu-id="b49bc-470">Let's take an example API that returns hello server date using hello *Date.now()* method.</span></span>  <span data-ttu-id="b49bc-471">Här är hello api/date.js fil:</span><span class="sxs-lookup"><span data-stu-id="b49bc-471">Here is hello api/date.js file:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

<span data-ttu-id="b49bc-472">Varje parameter är en av hello RESTful standardverb - GET, POST, uppdatera eller ta bort.</span><span class="sxs-lookup"><span data-stu-id="b49bc-472">Each parameter is one of hello standard RESTful verbs - GET, POST, PATCH, or DELETE.</span></span>  <span data-ttu-id="b49bc-473">hello-metoden är en standard [ExpressJS Middleware] funktion som skickar utdata hello krävs.</span><span class="sxs-lookup"><span data-stu-id="b49bc-473">hello method is a standard [ExpressJS Middleware] function that sends hello required output.</span></span>

### <span data-ttu-id="b49bc-474"><a name="howto-customapi-auth"></a>Så här: kräver autentisering för åtkomst tooa anpassade API</span><span class="sxs-lookup"><span data-stu-id="b49bc-474"><a name="howto-customapi-auth"></a>How to: Require authentication for access tooa custom API</span></span>
<span data-ttu-id="b49bc-475">Azure Mobile Apps-SDK implementerar autentisering i hello samma sätt för anpassade API: erna och hello tabeller slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="b49bc-475">Azure Mobile Apps SDK implements authentication in hello same way for both hello tables endpoint and custom APIs.</span></span>  <span data-ttu-id="b49bc-476">Om du vill lägga till autentisering toohello API utvecklas i hello föregående avsnitt, lägga till en **åtkomst** egenskapen:</span><span class="sxs-lookup"><span data-stu-id="b49bc-476">To add authentication toohello API developed in hello previous section, add an **access** property:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

<span data-ttu-id="b49bc-477">Du kan också ange autentisering på specifika åtgärder:</span><span class="sxs-lookup"><span data-stu-id="b49bc-477">You can also specify authentication on specific operations:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // hello GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

<span data-ttu-id="b49bc-478">hello måste samma token som används för hello tabeller slutpunkt användas för anpassade API: er som kräver autentisering.</span><span class="sxs-lookup"><span data-stu-id="b49bc-478">hello same token that is used for hello tables endpoint must be used for custom APIs requiring authentication.</span></span>

### <span data-ttu-id="b49bc-479"><a name="howto-customapi-auth"></a>Så här: hantera stora filöverföringar</span><span class="sxs-lookup"><span data-stu-id="b49bc-479"><a name="howto-customapi-auth"></a>How to: Handle large file uploads</span></span>
<span data-ttu-id="b49bc-480">Azure Mobile Apps-SDK använder hello [body-parsern mellanprogram](https://github.com/expressjs/body-parser) tooaccept och avkoda brödtext innehållet i din ansökan.</span><span class="sxs-lookup"><span data-stu-id="b49bc-480">Azure Mobile Apps SDK uses hello [body-parser middleware](https://github.com/expressjs/body-parser) tooaccept and decode body content in your submission.</span></span>  <span data-ttu-id="b49bc-481">Du kan förkonfigurera body-parsern tooaccept större filöverföringar:</span><span class="sxs-lookup"><span data-stu-id="b49bc-481">You can pre-configure body-parser tooaccept larger file uploads:</span></span>

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import hello Custom API
    mobile.api.import('./api');

    // Add hello mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

<span data-ttu-id="b49bc-482">hello-filen är Base64-kodad före överföringen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-482">hello file is base-64 encoded before transmission.</span></span>  <span data-ttu-id="b49bc-483">Detta ökar hello storleken på hello faktiska överför (och därför hello storlek som du måste ta hänsyn till).</span><span class="sxs-lookup"><span data-stu-id="b49bc-483">This increases hello size of hello actual upload (and hence hello size you must account for).</span></span>

### <span data-ttu-id="b49bc-484"><a name="howto-customapi-sql"></a>Så här: köra anpassade SQL-instruktioner</span><span class="sxs-lookup"><span data-stu-id="b49bc-484"><a name="howto-customapi-sql"></a>How to: Execute custom SQL statements</span></span>
<span data-ttu-id="b49bc-485">hello Azure Mobile Apps-SDK tillåter åtkomst toohello hela kontexten via hello request-objektet, vilket gör att du tooexecute parametriserade SQL-instruktioner toohello definierade dataprovidern enkelt:</span><span class="sxs-lookup"><span data-stu-id="b49bc-485">hello Azure Mobile Apps SDK allows access toohello entire Context through hello request object, allowing you tooexecute parameterized SQL statements toohello defined data provider easily:</span></span>

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on tooa later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define hello query - anything that can be handled by hello mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute hello query.  hello context for Azure Mobile Apps is available through
            // request.azureMobile - hello data object contains hello configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <span data-ttu-id="b49bc-486"><a name="Debugging"></a>Felsökning, enkelt tabeller och enkelt API: er</span><span class="sxs-lookup"><span data-stu-id="b49bc-486"><a name="Debugging"></a>Debugging, Easy Tables, and Easy APIs</span></span>
### <span data-ttu-id="b49bc-487"><a name="howto-diagnostic-logs"></a>Så här: Felsök, diagnostisera och Felsök Azure Mobile apps</span><span class="sxs-lookup"><span data-stu-id="b49bc-487"><a name="howto-diagnostic-logs"></a>How to: Debug, diagnose, and troubleshoot Azure Mobile apps</span></span>
<span data-ttu-id="b49bc-488">hello Azure App Service tillhandahåller flera felsökning och felsökningstekniker för Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="b49bc-488">hello Azure App Service provides several debugging and troubleshooting techniques for Node.js applications.</span></span>
<span data-ttu-id="b49bc-489">Se följande artiklar tooget igång vid felsökning av din Node.js Mobile-serverdel toohello:</span><span class="sxs-lookup"><span data-stu-id="b49bc-489">Refer toohello following articles tooget started in troubleshooting your Node.js Mobile backend:</span></span>

* <span data-ttu-id="b49bc-490">[Övervaka en Azure Apptjänst]</span><span class="sxs-lookup"><span data-stu-id="b49bc-490">[Monitoring an Azure App Service]</span></span>
* <span data-ttu-id="b49bc-491">[Aktivera diagnostikloggning i Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="b49bc-491">[Enable Diagnostic Logging in Azure App Service]</span></span>
* <span data-ttu-id="b49bc-492">[Felsöka en Azure Apptjänst i Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="b49bc-492">[Troubleshoot an Azure App Service in Visual Studio]</span></span>

<span data-ttu-id="b49bc-493">Node.js-program har åtkomst tooa mängd diagnostiska loggen verktyg.</span><span class="sxs-lookup"><span data-stu-id="b49bc-493">Node.js applications have access tooa wide range of diagnostic log tools.</span></span>  <span data-ttu-id="b49bc-494">Internt hello Azure Mobile Apps Node.js SDK använder [Winston] för diagnostikloggning.</span><span class="sxs-lookup"><span data-stu-id="b49bc-494">Internally, hello Azure Mobile Apps Node.js SDK uses [Winston] for diagnostic logging.</span></span>  <span data-ttu-id="b49bc-495">Loggning aktiveras automatiskt genom att aktivera felsökningsläge eller genom att ange hello **MS_DebugMode** app inställningen tootrue i hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="b49bc-495">Logging is automatically enabled by enabling debug mode or by setting hello **MS_DebugMode** app setting tootrue in hello [Azure portal].</span></span> <span data-ttu-id="b49bc-496">Genererade loggar visas i hello diagnostikloggar på hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="b49bc-496">Generated logs appear in hello Diagnostic Logs on hello [Azure portal].</span></span>

### <span data-ttu-id="b49bc-497"><a name="in-portal-editing"></a><a name="work-easy-tables"></a>Så här: arbeta med enkelt tabeller i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b49bc-497"><a name="in-portal-editing"></a><a name="work-easy-tables"></a>How to: Work with Easy Tables in hello Azure portal</span></span>
<span data-ttu-id="b49bc-498">Enkelt tabeller i hello-portalen kan du skapa och arbeta med tabeller direkt i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-498">Easy Tables in hello portal let you create and work with tables right in hello portal.</span></span> <span data-ttu-id="b49bc-499">Du kan även redigera tabellåtgärder med hjälp av hello App Service Editor.</span><span class="sxs-lookup"><span data-stu-id="b49bc-499">You can even edit table operations using hello App Service Editor.</span></span>

<span data-ttu-id="b49bc-500">När du klickar på **enkelt tabeller** i dina backend-inställningar, du kan lägga till, ändra eller ta bort en tabell.</span><span class="sxs-lookup"><span data-stu-id="b49bc-500">When you click **Easy tables** in your backend site settings, you can add, modify, or delete a table.</span></span> <span data-ttu-id="b49bc-501">Du kan också se data i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="b49bc-501">You can also see data in hello table.</span></span>

![Arbeta med enkelt tabeller](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

<span data-ttu-id="b49bc-503">hello följande kommandon är tillgängliga i hello kommandofält för en tabell:</span><span class="sxs-lookup"><span data-stu-id="b49bc-503">hello following commands are available on hello command bar for a table:</span></span>

* <span data-ttu-id="b49bc-504">**Ändra behörigheter** - hello ändringsbehörighet för läsning, infoga, uppdatera och ta bort hello tabell.</span><span class="sxs-lookup"><span data-stu-id="b49bc-504">**Change permissions** - modify hello permission for read, insert, update and delete operations on hello table.</span></span>
  <span data-ttu-id="b49bc-505">Alternativen är tooallow anonym åtkomst, toorequire autentisering eller toodisable komma åt toohello igen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-505">Options are tooallow anonymous access, toorequire authentication, or toodisable all access toohello operation.</span></span>
* <span data-ttu-id="b49bc-506">**Redigera skriptet** -hello skriptfilen för hello tabellen öppnas i hello App Service Editor.</span><span class="sxs-lookup"><span data-stu-id="b49bc-506">**Edit script** - hello script file for hello table is opened in hello App Service Editor.</span></span>
* <span data-ttu-id="b49bc-507">**Hantera schemat** - Lägg till eller ta bort kolumner eller ändra hello index för tabellen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-507">**Manage schema** - add or delete columns or change hello table index.</span></span>
* <span data-ttu-id="b49bc-508">**Rensa tabellen** -trunkerar en befintlig tabell är om du tar bort alla datarader men lämnar hello schemat oförändrat.</span><span class="sxs-lookup"><span data-stu-id="b49bc-508">**Clear table** - truncates an existing table be deleting all data rows but leaving hello schema unchanged.</span></span>
* <span data-ttu-id="b49bc-509">**Ta bort rader** -ta bort enskilda rader med data.</span><span class="sxs-lookup"><span data-stu-id="b49bc-509">**Delete rows** - delete individual rows of data.</span></span>
* <span data-ttu-id="b49bc-510">**Visa direktuppspelningsloggar** -ansluter toohello strömning Loggtjänsten för webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-510">**View streaming logs** - connects you toohello streaming log service for your site.</span></span>

### <span data-ttu-id="b49bc-511"><a name="work-easy-apis"></a>Så här: arbeta med enkla API: er i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b49bc-511"><a name="work-easy-apis"></a>How to: Work with Easy APIs in hello Azure portal</span></span>
<span data-ttu-id="b49bc-512">Enkelt API: er i hello-portalen kan du skapa och arbeta med anpassade API: er direkt i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="b49bc-512">Easy APIs in hello portal let you create and work with custom APIs right in hello portal.</span></span> <span data-ttu-id="b49bc-513">Du kan redigera API-skript med hjälp av hello App Service Editor.</span><span class="sxs-lookup"><span data-stu-id="b49bc-513">You can edit API scripts using hello App Service Editor.</span></span>

<span data-ttu-id="b49bc-514">När du klickar på **enkelt API: er** i dina backend-inställningar, du kan lägga till, ändra eller ta bort en anpassad API-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="b49bc-514">When you click **Easy APIs** in your backend site settings, you can add, modify, or delete a custom API endpoint.</span></span>

![Arbeta med enkla API: er](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

<span data-ttu-id="b49bc-516">I hello portal du ändra hello åtkomstbehörighet för en viss HTTP-åtgärd, redigera hello API skriptfilen i App Service Editor eller visa hello direktuppspelningsloggar.</span><span class="sxs-lookup"><span data-stu-id="b49bc-516">In hello portal, you can change hello access permissions for a given HTTP action, edit hello API script file in the App Service Editor, or view hello streaming logs.</span></span>

### <span data-ttu-id="b49bc-517"><a name="online-editor"></a>Så här: redigera koden i hello App Service Editor</span><span class="sxs-lookup"><span data-stu-id="b49bc-517"><a name="online-editor"></a>How to: Edit code in hello App Service Editor</span></span>
<span data-ttu-id="b49bc-518">hello Azure-portalen kan du redigera skriptfilerna Node.js-serverdel i hello App Service Editor utan att behöva hämta hello projektet tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="b49bc-518">hello Azure portal lets you edit your Node.js backend script files in hello App Service Editor without having to download hello project tooyour local computer.</span></span> <span data-ttu-id="b49bc-519">tooedit skriptfiler i hello online redigeraren:</span><span class="sxs-lookup"><span data-stu-id="b49bc-519">tooedit script files in hello online editor:</span></span>

1. <span data-ttu-id="b49bc-520">I din Mobilapp backend-bladet klickar du på **alla inställningar** > antingen **enkelt tabeller** eller **enkelt API: er**, klicka på en tabell eller API: et och sedan på **redigera skriptet**.</span><span class="sxs-lookup"><span data-stu-id="b49bc-520">In your Mobile App backend blade, click **All settings** > either **Easy tables** or **Easy APIs**, click a table or API, then click **Edit script**.</span></span> <span data-ttu-id="b49bc-521">hello skriptfilen öppnas i hello App Service Editor.</span><span class="sxs-lookup"><span data-stu-id="b49bc-521">hello script file is opened in hello App Service Editor.</span></span>

    ![App Service Editor](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. <span data-ttu-id="b49bc-523">Se dina ändringar toohello kodfilen i hello online-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="b49bc-523">Make your changes toohello code file in hello online editor.</span></span> <span data-ttu-id="b49bc-524">Ändringarna sparas automatiskt när du skriver.</span><span class="sxs-lookup"><span data-stu-id="b49bc-524">Changes are saved automatically as you type.</span></span>

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Snabbstart för Android-klienten]: app-service-mobile-android-get-started.md
[Snabbstart för Apache Cordova-klient]: app-service-mobile-cordova-get-started.md
[iOS klienten Snabbstart]: app-service-mobile-ios-get-started.md
[Snabbstart för Xamarin.iOS-klient]: app-service-mobile-xamarin-ios-get-started.md
[Snabbstart för Xamarin.Android-klient]: app-service-mobile-xamarin-android-get-started.md
[Snabbstart för Xamarin.Forms-klient]: app-service-mobile-xamarin-forms-get-started.md
[Snabbstart för Windows Store-klienten]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[datasynkronisering offline]: app-service-mobile-offline-data-sync.md
[Hur tooconfigure Azure Active Directory-autentisering]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Hur tooconfigure Facebook-autentisering]: app-service-mobile-how-to-configure-facebook-authentication.md
[Hur tooconfigure Google-autentisering]: app-service-mobile-how-to-configure-google-authentication.md
[Hur tooconfigure Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Hur tooconfigure Twitter-autentisering]: app-service-mobile-how-to-configure-twitter-authentication.md
[Azure App Service Deployment Guide]: ../app-service-web/web-sites-deploy.md
[Övervaka en Azure Apptjänst]: ../app-service-web/web-sites-monitor.md
[Aktivera diagnostikloggning i Azure App Service]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Felsöka en Azure Apptjänst i Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[ange hello nod Version]: ../nodejs-specify-node-version-azure-apps.md
[använda noden moduler]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure-portalen]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp exempel på GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo prov på GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[katalogen samples på GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[1.1 för Node.js-verktyg för Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js paketet]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
