---
title: aaaCreate en Node.js-chattprogram med Socket.IO i Azure App Service
description: "En genomgång som visar med socket.io i en node.js-webbapp i Azure."
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: c4c4af36-3ecf-4619-b586-ca90d53ce35b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 3bd7867ccc297dc0a21c7a00cc9db06358877f5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a><span data-ttu-id="93e92-103">Skapa ett Node.js-chattprogram med Socket.IO i Azure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="93e92-103">Create a Node.js chat application with Socket.IO in Azure App Service</span></span>
<span data-ttu-id="93e92-104">Socket.IO ger kommunikation mellan din node.js-server och klienter som använder WebSockets i realtid.</span><span class="sxs-lookup"><span data-stu-id="93e92-104">Socket.IO provides real-time communication between your node.js server and clients using WebSockets.</span></span> <span data-ttu-id="93e92-105">Det stöder också återställningsplats tooother transporter (till exempel lång avsökning) som fungerar med äldre webbläsare.</span><span class="sxs-lookup"><span data-stu-id="93e92-105">It also supports fallback tooother transports (such as long polling) that work with older browsers.</span></span> <span data-ttu-id="93e92-106">Den här självstudiekursen kommer igenom värd för en Socket.IO baserat chattprogram som en Azure webbapp och visa hur tooscale hello program använder [Azure Redis-Cache].</span><span class="sxs-lookup"><span data-stu-id="93e92-106">This tutorial will walk you through hosting a Socket.IO based chat application as an Azure web app, and show you how tooscale hello application using [Azure Redis Cache].</span></span> <span data-ttu-id="93e92-107">Mer information om Socket.IO finns <http://socket.io/>.</span><span class="sxs-lookup"><span data-stu-id="93e92-107">For more information on Socket.IO, see <http://socket.io/>.</span></span>

> [!NOTE]
> <span data-ttu-id="93e92-108">hello procedurer i det här steget gäller för[App Service Web Apps]; för molntjänster, se [skapa ett Node.js-chatta program med Socket.IO i Azure-molntjänst].</span><span class="sxs-lookup"><span data-stu-id="93e92-108">hello procedures in this task apply too[App Service Web Apps]; for Cloud Services, see [Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service].</span></span>
> 
> 

## <a name="download-hello-chat-example"></a><span data-ttu-id="93e92-109">Hämta hello chatt-exempel</span><span class="sxs-lookup"><span data-stu-id="93e92-109">Download hello chat example</span></span>
<span data-ttu-id="93e92-110">För det här projektet använder vi hello chatt exempel från hello [Socket.IO GitHub-lagringsplatsen].</span><span class="sxs-lookup"><span data-stu-id="93e92-110">For this project, we will use hello chat example from hello [Socket.IO GitHub repository].</span></span> <span data-ttu-id="93e92-111">Utför följande steg toodownload hello exempel hello och lägga till den toohello projekt som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="93e92-111">Perform hello following steps toodownload hello example and add it toohello project you previously created.</span></span>

1. <span data-ttu-id="93e92-112">Hämta en [ZIP- eller GZ arkiveras versionen] av hello Socket.IO projekt (version 1.3.5 har använts för det här dokumentet)</span><span class="sxs-lookup"><span data-stu-id="93e92-112">Download a [ZIP or GZ archived release] of hello Socket.IO project (version 1.3.5 was used for this document)</span></span>
2. <span data-ttu-id="93e92-113">Extrahera hello arkivet och kopiera hello **exempel\\chatt** directory tooa ny plats.</span><span class="sxs-lookup"><span data-stu-id="93e92-113">Extract hello archive and copy hello **examples\\chat** directory tooa new location.</span></span> <span data-ttu-id="93e92-114">Till exempel  **\\nod\\chatt**.</span><span class="sxs-lookup"><span data-stu-id="93e92-114">For example, **\\node\\chat**.</span></span>

## <a name="modify-appjs-and-install-modules"></a><span data-ttu-id="93e92-115">Ändra app.js och installera moduler</span><span class="sxs-lookup"><span data-stu-id="93e92-115">Modify app.js and install modules</span></span>
1. <span data-ttu-id="93e92-116">Byt namn på hello **index.js** filen för**app.js**.</span><span class="sxs-lookup"><span data-stu-id="93e92-116">Rename hello **index.js** file too**app.js**.</span></span> <span data-ttu-id="93e92-117">Detta gör att Azure toodetect att detta är ett Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="93e92-117">This allows Azure toodetect that this is a Node.js application.</span></span>
2. <span data-ttu-id="93e92-118">Öppna hello **app.js** i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="93e92-118">Open hello **app.js** file in a text editor.</span></span> <span data-ttu-id="93e92-119">Ändra hello rad som innehåller `var io = require('../..')(server);` enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="93e92-119">Change hello line containing `var io = require('../..')(server);` as shown below:</span></span>
   
       var express = require('express');
       var app = express();
       var server = require('http').createServer(app);
       // var io = require('../..')(server);
       // New:
       var io = require('socket.io')(server);
       var port = process.env.PORT || 3000;
3. <span data-ttu-id="93e92-120">Öppna hello **package.json** och Lägg till en referens toosocket.io under `dependencies`, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="93e92-120">Open hello **package.json** file and add a reference toosocket.io under `dependencies`, as shown below:</span></span>
   
        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }
4. <span data-ttu-id="93e92-121">Från kommandoraden hello, ändra toohello  **\\nod\\chatt** katalogen och Använd npm tooinstall hello moduler som krävs av programmet:</span><span class="sxs-lookup"><span data-stu-id="93e92-121">From hello command-line, change toohello **\\node\\chat** directory and use npm tooinstall hello modules required by this application:</span></span>
   
        npm install
   
    <span data-ttu-id="93e92-122">Detta kommer att installera hello moduler i en undermapp som heter **node_modules**.</span><span class="sxs-lookup"><span data-stu-id="93e92-122">This will install hello modules into a subfolder named **node_modules**.</span></span>

## <a name="create-an-azure-web-app"></a><span data-ttu-id="93e92-123">Skapa en Azure-Webbapp</span><span class="sxs-lookup"><span data-stu-id="93e92-123">Create an Azure Web App</span></span>
<span data-ttu-id="93e92-124">Följ dessa steg toocreate en Azure-webbapp, aktivera Git-publicering och sedan aktivera WebSocket-stöd för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="93e92-124">Follow these steps toocreate an Azure web app, enable Git publishing, and then enable WebSocket support for hello web app.</span></span>

> [!NOTE]
> <span data-ttu-id="93e92-125">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="93e92-125">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="93e92-126">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="93e92-126">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="93e92-127">Mer information om den <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">kostnadsfria utvärderingsversionen av Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="93e92-127">For details, see <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure Free Trial</a>.</span></span>
> 
> 

1. <span data-ttu-id="93e92-128">Installera hello Azure-kommandoradsgränssnittet (Azure CLI) och ansluta tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="93e92-128">Install hello Azure Command-Line Interface (Azure CLI) and connect tooyour Azure subscription.</span></span> <span data-ttu-id="93e92-129">Se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="93e92-129">See [Install and Configure hello Azure CLI](../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="93e92-130">Om det här är första gången du upp en databas i Azure måste toocreate inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="93e92-130">If this is your first time setting up a repository in Azure, you need toocreate login credentials.</span></span> <span data-ttu-id="93e92-131">Ange följande kommando hello från hello Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="93e92-131">From hello Azure CLI, enter hello following command:</span></span>
   
        azure site deployment user set [username] [password]
3. <span data-ttu-id="93e92-132">Ändra toohello  **\\node\chat** katalogen och Använd hello följande kommandot toocreate en ny Azure webbapp och en lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="93e92-132">Change toohello **\\node\chat** directory and use hello following command toocreate a new Azure web app and a local Git repository.</span></span> <span data-ttu-id="93e92-133">Det här kommandot skapar även en Git fjärråtkomst med namnet ”azure”.</span><span class="sxs-lookup"><span data-stu-id="93e92-133">This command also creates a Git remote named 'azure'.</span></span>
   
        azure site create mysitename --git
   
    <span data-ttu-id="93e92-134">Du måste ersätta 'mysitename' med ett unikt namn för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="93e92-134">You must replace 'mysitename' with a unique name for your web app.</span></span>
4. <span data-ttu-id="93e92-135">Genomför hello befintliga filer toohello lokal databas med hjälp av hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="93e92-135">Commit hello existing files toohello local repository by using hello following commands:</span></span>
   
        git add .
        git commit -m "Initial commit"
5. <span data-ttu-id="93e92-136">Push hello filer toohello Azure Web Apps databasen med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="93e92-136">Push hello files toohello Azure Web Apps repository with hello following command:</span></span>
   
        git push azure master
   
    <span data-ttu-id="93e92-137">När du uppmanas ange dina autentiseringsuppgifter från steg 2.</span><span class="sxs-lookup"><span data-stu-id="93e92-137">When prompted, enter your credentials from step 2.</span></span> <span data-ttu-id="93e92-138">Du tar emot statusmeddelanden som importeras moduler på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="93e92-138">You will receive status messages as modules are imported on hello server.</span></span> <span data-ttu-id="93e92-139">När den här processen är klar, finnas hello program på Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="93e92-139">Once this process has completed, hello application will be hosted on your Azure web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="93e92-140">Under installationen av modulen kan du eventuellt se felen som ' hello importerade projekt... hittades inte ”.</span><span class="sxs-lookup"><span data-stu-id="93e92-140">During module installation, you may notice errors that 'hello imported project ... was not found'.</span></span> <span data-ttu-id="93e92-141">Dessa kan ignoreras.</span><span class="sxs-lookup"><span data-stu-id="93e92-141">These can safely be ignored.</span></span>
   > 
   > 
6. <span data-ttu-id="93e92-142">Socket.IO använder WebSockets som inte är aktiverat som standard på Azure.</span><span class="sxs-lookup"><span data-stu-id="93e92-142">Socket.IO uses WebSockets, which are not enabled by default on Azure.</span></span> <span data-ttu-id="93e92-143">tooenable web sockets använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="93e92-143">tooenable web sockets, use hello following command:</span></span>
   
        azure site set -w
   
    <span data-ttu-id="93e92-144">Om du uppmanas ange hello namn på hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="93e92-144">If prompted, enter hello name of hello web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="93e92-145">Hej ”azure site set -w-kommandot kommer fungerar bara med version 0.7.4 eller senare av hello Azure-kommandoradsgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="93e92-145">hello 'azure site set -w' command will work only with version 0.7.4 or higher of hello Azure Command-Line Interface.</span></span> <span data-ttu-id="93e92-146">Du kan också aktivera WebSocket stöd med hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="93e92-146">You can also enable WebSocket support using hello [Azure Portal](https://portal.azure.com).</span></span>
   > 
   > <span data-ttu-id="93e92-147">tooenable WebSockets med hello Azure-portalen klickar du på hello webbprogrammet från hello Web Apps-bladet, klickar du på **alla inställningar** > **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="93e92-147">tooenable WebSockets using hello Azure Portal, click hello web app from hello Web Apps blade, click **All settings** > **Application settings**.</span></span> <span data-ttu-id="93e92-148">Under **Webbsocketar**, klickar du på **på**.</span><span class="sxs-lookup"><span data-stu-id="93e92-148">Under **Web Sockets**, click **On**.</span></span> <span data-ttu-id="93e92-149">Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="93e92-149">Then click **Save**.</span></span>
   > 
   > 
7. <span data-ttu-id="93e92-150">tooview hello-webbapp på Azure, Använd hello följande kommando toolaunch din webbläsare och gå toohello som är värd för webbprogram:</span><span class="sxs-lookup"><span data-stu-id="93e92-150">tooview hello web app on Azure, use hello following command toolaunch your web browser and navigate toohello hosted web app:</span></span>
   
        azure site browse

<span data-ttu-id="93e92-151">Appen körs nu på Azure och kan vidarebefordra chatt meddelanden mellan olika klienter med Socket.IO.</span><span class="sxs-lookup"><span data-stu-id="93e92-151">Your app is now running on Azure, and can relay chat messages between different clients using Socket.IO.</span></span>

## <a name="scale-out"></a><span data-ttu-id="93e92-152">Skala ut</span><span class="sxs-lookup"><span data-stu-id="93e92-152">Scale out</span></span>
<span data-ttu-id="93e92-153">Socket.IO program kan skaländras ut med hjälp av en **nätverkskort** toodistribute meddelanden och händelser mellan flera instanser av programmet.</span><span class="sxs-lookup"><span data-stu-id="93e92-153">Socket.IO applications can be scaled out by using an **adapter** toodistribute messages and events between multiple application instances.</span></span> <span data-ttu-id="93e92-154">Det finns flera nätverkskort, hello [socket.io redis] kortet enkelt kan användas med hello Azure Redis-Cache-funktionen.</span><span class="sxs-lookup"><span data-stu-id="93e92-154">While there are several adapters available, hello [socket.io-redis] adapter can be easily used with hello Azure Redis Cache feature.</span></span>

> [!NOTE]
> <span data-ttu-id="93e92-155">Ytterligare krav för att skala ut en Socket.IO lösning finns stöd för Fäst sessioner.</span><span class="sxs-lookup"><span data-stu-id="93e92-155">An additional requirement for scaling out a Socket.IO solution is support for sticky sessions.</span></span> <span data-ttu-id="93e92-156">Fäst sessioner är aktiverade som standard för Azure Web Apps via Azure begära routning.</span><span class="sxs-lookup"><span data-stu-id="93e92-156">Sticky sessions are enabled by default for Azure Web Apps through Azure Request Routing.</span></span> <span data-ttu-id="93e92-157">Mer information finns i [instans tillhörighet i Azure webbplatser].</span><span class="sxs-lookup"><span data-stu-id="93e92-157">For more information, see [Instance Affinity in Azure Web Sites].</span></span>
> 
> 

### <a name="create-a-redis-cache"></a><span data-ttu-id="93e92-158">Skapa ett Redis-cache</span><span class="sxs-lookup"><span data-stu-id="93e92-158">Create a Redis cache</span></span>
<span data-ttu-id="93e92-159">Utför hello steg i [skapa en cache i Azure Redis-Cache] toocreate ett nytt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="93e92-159">Perform hello steps in [Create a cache in Azure Redis Cache] toocreate a new cache.</span></span>

> [!NOTE]
> <span data-ttu-id="93e92-160">Spara hello **värdnamn** och **primärnyckel** för ditt cacheminne hello eftersom de behövs i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="93e92-160">Save hello **Host name** and **Primary key** for your cache, as these will be needed in hello next steps.</span></span>
> 
> 

### <a name="add-hello-redis-and-socketio-redis-modules"></a><span data-ttu-id="93e92-161">Lägg till hello redis och socket.io redis-moduler</span><span class="sxs-lookup"><span data-stu-id="93e92-161">Add hello redis and socket.io-redis modules</span></span>
1. <span data-ttu-id="93e92-162">Ändra toohello från en kommandorad  **\\nod\\chatt** katalogen och Använd hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="93e92-162">From a command-line, change toohello **\\node\\chat** directory and use hello following command.</span></span>
   
        npm install socket.io-redis@0.1.4 redis@0.12.1 --save
   
   > [!NOTE]
   > <span data-ttu-id="93e92-163">hello-versioner som anges i det här kommandot är hello-versioner som används vid testning av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="93e92-163">hello versions specified in this command are hello versions used when testing this article.</span></span>
   > 
   > 
2. <span data-ttu-id="93e92-164">Ändra hello **app.js** filen tooadd hello följande rader omedelbart efter`var io = require('socket.io')(server);`</span><span class="sxs-lookup"><span data-stu-id="93e92-164">Modify hello **app.js** file tooadd hello following lines immediately after `var io = require('socket.io')(server);`</span></span>
   
        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
   
        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));
   
    <span data-ttu-id="93e92-165">Ersätt **redishostname** och **rediskey** med hello värdnamnet och nyckeln för Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="93e92-165">Replace **redishostname** and **rediskey** with hello host name and key for your Redis cache.</span></span>
   
    <span data-ttu-id="93e92-166">Detta skapar en publicera och prenumerera på klienten toohello Redis-cache som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="93e92-166">This will create a publish and subscribe client toohello Redis cache created previously.</span></span> <span data-ttu-id="93e92-167">hello klienter sedan används med hello kortet tooconfigure Socket.IO toouse hello Redis-cache för att skicka meddelanden och händelser mellan instanser av programmet</span><span class="sxs-lookup"><span data-stu-id="93e92-167">hello clients are then used with hello adapter tooconfigure Socket.IO toouse hello Redis cache for passing messages and events between instances of your application</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="93e92-168">När hello **socket.io redis** nätverkskort kan kommunicera direkt tooRedis, hello aktuella versionen stöder inte hello-autentisering krävs av Azure Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="93e92-168">While hello **socket.io-redis** adapter can communicate directly tooRedis, hello current version does not support hello authentication required by Azure Redis cache.</span></span> <span data-ttu-id="93e92-169">Så hello första anslutningen skapas med hjälp av hello **redis** modulen, skickas sedan hello klienten toohello **socket.io redis** nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="93e92-169">So hello initial connection is created using hello **redis** module, then hello client is passed toohello **socket.io-redis** adapter.</span></span>
   > 
   > <span data-ttu-id="93e92-170">Azure Redis-Cache har stöd för säkra anslutningar som använder port 6380, men hello-moduler som används i det här exemplet stöder inte säkra anslutningar från 2014-7-14.</span><span class="sxs-lookup"><span data-stu-id="93e92-170">While Azure Redis Cache supports secure connections using port 6380, hello modules used in this example do not support secure connections as of 7/14/2014.</span></span> <span data-ttu-id="93e92-171">hello ovan koden använder hello standard osäkra port för 6379.</span><span class="sxs-lookup"><span data-stu-id="93e92-171">hello above code uses hello default, unsecure port of 6379.</span></span>
   > 
   > 
3. <span data-ttu-id="93e92-172">Spara hello ändrade **app.js**</span><span class="sxs-lookup"><span data-stu-id="93e92-172">Save hello modified **app.js**</span></span>

### <a name="commit-changes-and-redeploy"></a><span data-ttu-id="93e92-173">Genomför ändringar och distribuera</span><span class="sxs-lookup"><span data-stu-id="93e92-173">Commit changes and redeploy</span></span>
<span data-ttu-id="93e92-174">Från kommandoraden i hello hello  **\\nod\\chatt** directory, Använd följande kommandon toocommit ändringar hello och distribuera programmet hello.</span><span class="sxs-lookup"><span data-stu-id="93e92-174">From hello command-line in hello **\\node\\chat** directory, use hello following commands toocommit changes and redeploy hello application.</span></span>

    git add .
    git commit -m "implementing scale out"
    git push azure master

<span data-ttu-id="93e92-175">När hello ändringar har aviserats toohello server, kan du skala webbplatsen på flera instanser med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="93e92-175">Once hello changes have been pushed toohello server, you can scale your site across multiple instances by using hello following command.</span></span>

    azure site scale instances --instances #

<span data-ttu-id="93e92-176">Där  **#**  är hello antalet instanser toocreate.</span><span class="sxs-lookup"><span data-stu-id="93e92-176">Where **#** is hello number of instances toocreate.</span></span>

<span data-ttu-id="93e92-177">Du kan ansluta tooyour webbprogrammet från flera webbläsare eller datorer tooverify som meddelanden skickas korrekt tooall klienter.</span><span class="sxs-lookup"><span data-stu-id="93e92-177">You can connect tooyour web app from multiple browsers or computers tooverify that messages are correctly sent tooall clients.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="93e92-178">Felsökning</span><span class="sxs-lookup"><span data-stu-id="93e92-178">Troubleshooting</span></span>
### <a name="connection-limits"></a><span data-ttu-id="93e92-179">Anslutningsbegränsningar</span><span class="sxs-lookup"><span data-stu-id="93e92-179">Connection limits</span></span>
<span data-ttu-id="93e92-180">Azure Web Apps finns i flera SKU: er som avgör hello resurser tillgängliga tooyour plats.</span><span class="sxs-lookup"><span data-stu-id="93e92-180">Azure Web Apps is available in multiple SKUs, which determine hello resources available tooyour site.</span></span> <span data-ttu-id="93e92-181">Detta inkluderar hello antalet tillåtna WebSocket-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="93e92-181">This includes hello number of allowed WebSocket connections.</span></span> <span data-ttu-id="93e92-182">Mer information finns i hello [prissättning för Web Apps].</span><span class="sxs-lookup"><span data-stu-id="93e92-182">For more information, see hello [Web Apps Pricing page].</span></span>

### <a name="messages-arent-being-sent-using-websockets"></a><span data-ttu-id="93e92-183">Meddelanden skickas inte med WebSockets</span><span class="sxs-lookup"><span data-stu-id="93e92-183">Messages aren't being sent using WebSockets</span></span>
<span data-ttu-id="93e92-184">Om klientwebbläsare hålla återgång toolong avsökning istället för att använda WebSockets, kan det bero på något av följande hello.</span><span class="sxs-lookup"><span data-stu-id="93e92-184">If client browsers keep falling back toolong polling instead of using WebSockets, it may be because of one of hello following.</span></span>

* <span data-ttu-id="93e92-185">**Försök att begränsa hello transport toojust WebSockets**</span><span class="sxs-lookup"><span data-stu-id="93e92-185">**Try limiting hello transport toojust WebSockets**</span></span>
  
    <span data-ttu-id="93e92-186">För att Socket.IO toouse WebSockets som hello messaging transport både hello-servern och klienten måste ha stöd för WebSockets.</span><span class="sxs-lookup"><span data-stu-id="93e92-186">In order for Socket.IO toouse WebSockets as hello messaging transport, both hello server and client must support WebSockets.</span></span> <span data-ttu-id="93e92-187">Om en eller hello andra inte, förhandlar Socket.IO en annan transport, till exempel lång avsökning.</span><span class="sxs-lookup"><span data-stu-id="93e92-187">If one or hello other does not, Socket.IO will negotiate another transport, such as long polling.</span></span> <span data-ttu-id="93e92-188">hello standardlistan över transporter som används av Socket.IO är ` websocket, htmlfile, xhr-polling, jsonp-polling`.</span><span class="sxs-lookup"><span data-stu-id="93e92-188">hello default list of transports used by Socket.IO is ` websocket, htmlfile, xhr-polling, jsonp-polling`.</span></span> <span data-ttu-id="93e92-189">Du kan tvinga den tooonly Använd WebSockets genom att lägga till följande kod toohello hello **app.js** filen efter hello rad som innehåller `, nicknames = {};`.</span><span class="sxs-lookup"><span data-stu-id="93e92-189">You can force it tooonly use WebSockets by adding hello following code toohello **app.js** file, after hello line containing `, nicknames = {};`.</span></span>
  
        io.configure(function() {
          io.set('transports', ['websocket']);
        });
  
  > [!NOTE]
  > <span data-ttu-id="93e92-190">Observera att gamla webbläsare som inte stöder WebSockets inte kan tooconnect toohello plats medan hello ovan koden är aktiv, eftersom den begränsar bara kommunikation tooWebSockets.</span><span class="sxs-lookup"><span data-stu-id="93e92-190">Note that older browsers that do not support WebSockets will not be able tooconnect toohello site while hello above code is active, as it restricts communication tooWebSockets only.</span></span>
  > 
  > 
* <span data-ttu-id="93e92-191">**Använd SSL**</span><span class="sxs-lookup"><span data-stu-id="93e92-191">**Use SSL**</span></span>
  
    <span data-ttu-id="93e92-192">WebSockets förlitar sig på vissa mindre används HTTP-huvuden, till exempel hello **uppgradera** huvud.</span><span class="sxs-lookup"><span data-stu-id="93e92-192">WebSockets relies on some lesser used HTTP headers, such as hello **Upgrade** header.</span></span> <span data-ttu-id="93e92-193">Vissa mellanliggande nätverksenheter, till exempel webbproxyservrar, ta bort dessa huvuden.</span><span class="sxs-lookup"><span data-stu-id="93e92-193">Some intermediate network devices, such as web proxies, may remove these headers.</span></span> <span data-ttu-id="93e92-194">tooavoid det här problemet kan du upprätta hello WebSocket-anslutningen via SSL.</span><span class="sxs-lookup"><span data-stu-id="93e92-194">tooavoid this problem, you can establish hello WebSocket connection over SSL.</span></span>
  
    <span data-ttu-id="93e92-195">Ett enkelt sätt tooaccomplish detta är tooconfigure Socket.IO för`match origin protocol`.</span><span class="sxs-lookup"><span data-stu-id="93e92-195">An easy way tooaccomplish this is tooconfigure Socket.IO too`match origin protocol`.</span></span> <span data-ttu-id="93e92-196">Detta instruerar Socket.IO toosecure WebSockets kommunikation hello samma som hello ursprung HTTP/HTTPS-begäran för hello webbsida.</span><span class="sxs-lookup"><span data-stu-id="93e92-196">This instructs Socket.IO toosecure WebSockets communication hello same as hello originating HTTP/HTTPS request for hello web page.</span></span> <span data-ttu-id="93e92-197">Om en webbläsare använder en HTTPS-URL-toovisit din webbplats, vara framtida WebSocket-kommunikation via Socket.IO skyddad via SSL.</span><span class="sxs-lookup"><span data-stu-id="93e92-197">If a browser uses an HTTPS URL toovisit your website, subsequent WebSocket communications through Socket.IO will be secured over SSL.</span></span>
  
    <span data-ttu-id="93e92-198">toomodify det här exemplet tooenable den här konfigurationen, Lägg till följande kod toohello hello **app.js** filen efter hello rad som innehåller `, nicknames = {};`.</span><span class="sxs-lookup"><span data-stu-id="93e92-198">toomodify this example tooenable this configuration, add hello following code toohello **app.js** file after hello line containing `, nicknames = {};`.</span></span>
  
        io.configure(function() {
          io.set('match origin protocol', true);
        });
* <span data-ttu-id="93e92-199">**Kontrollera inställningarna för web.config**</span><span class="sxs-lookup"><span data-stu-id="93e92-199">**Verify web.config settings**</span></span>
  
    <span data-ttu-id="93e92-200">Azure-webbappar som värd för Node.js-program använda hello **web.config** filen tooroute inkommande begäranden toohello Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="93e92-200">Azure web apps that host Node.js applications use hello **web.config** file tooroute incoming requests toohello Node.js application.</span></span> <span data-ttu-id="93e92-201">WebSockets toofunction korrekt med Node.js-program, hello **web.config** måste innehålla följande post hello.</span><span class="sxs-lookup"><span data-stu-id="93e92-201">For WebSockets toofunction correctly with Node.js applications, hello **web.config** must contain hello following entry.</span></span>
  
        <webSocket enabled="false"/>
  
    <span data-ttu-id="93e92-202">Detta inaktiverar hello IIS WebSockets modulen som innehåller sin egen implementering av WebSockets och står i konflikt med Node.js specifika WebSocket-moduler, till exempel Socket.IO.</span><span class="sxs-lookup"><span data-stu-id="93e92-202">This disables hello IIS WebSockets module, which includes its own implementation of WebSockets and conflicts with Node.js specific WebSocket modules such as Socket.IO.</span></span> <span data-ttu-id="93e92-203">Om den här raden finns inte eller har angetts för`true`, kan detta hello orsaken till att hello WebSocket transport inte fungerar för ditt program.</span><span class="sxs-lookup"><span data-stu-id="93e92-203">If this line is not present, or is set too`true`, this may be hello reason that hello WebSocket transport is not working for your application.</span></span>
  
    <span data-ttu-id="93e92-204">Normalt sett Node.js-program innehåller inte en **web.config** -fil, så Azure Websites genererar automatiskt en för Node.js-program när de har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="93e92-204">Normally, Node.js applications do not include a **web.config** file, so Azure Websites will automatically generate one for Node.js applications when they are deployed.</span></span> <span data-ttu-id="93e92-205">Eftersom den här filen genereras automatiskt på hello-servern, måste du använda hello FTP eller FTPS URL för din webbplats tooview den här filen.</span><span class="sxs-lookup"><span data-stu-id="93e92-205">Since this file is automatically generated on hello server, you must use hello FTP or FTPS URL for your website tooview this file.</span></span> <span data-ttu-id="93e92-206">Du kan hitta hello FTP och FTPS URL: er för webbplatsen i hello klassiska portalen genom att välja ditt webbprogram och sedan hello **instrumentpanelen** länk.</span><span class="sxs-lookup"><span data-stu-id="93e92-206">You can find hello FTP and FTPS URLs for your site in hello classic portal by selecting your web app, and then hello **Dashboard** link.</span></span> <span data-ttu-id="93e92-207">hello URL: er visas i hello **snabböversikten** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="93e92-207">hello URLs are displayed in hello **quick glance** section.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="93e92-208">Hej **web.config** filen genereras bara av Azure Websites om programmet inte tillhandahåller någon.</span><span class="sxs-lookup"><span data-stu-id="93e92-208">hello **web.config** file is only generated by Azure Websites if your application does not provide one.</span></span> <span data-ttu-id="93e92-209">Om du anger en **web.config** filen i hello roten av projektet program kommer det att användas av Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="93e92-209">If you provide a **web.config** file in hello root of your application project, it will be used by Azure Web Apps.</span></span>
  > 
  > 
  
    <span data-ttu-id="93e92-210">Om hello posten finns inte eller är värdet tooa för `true`, bör du skapa en **web.config** i hello roten för ditt Node.js-program och ange ett värde för `false`.</span><span class="sxs-lookup"><span data-stu-id="93e92-210">If hello entry is not present, or is set tooa value of `true`, then you should create a **web.config** in hello root of your Node.js application and specify a value of `false`.</span></span>  <span data-ttu-id="93e92-211">För referens anger hello nedan är ett standardvärde **web.config** för ett program som använder **app.js** som hello startpunkt.</span><span class="sxs-lookup"><span data-stu-id="93e92-211">For reference, hello below is a default **web.config** for an application that uses **app.js** as hello entry point.</span></span>
  
        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used toorun node processes behind
             IIS or IIS Express.  For more information, visit:
  
             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->
  
        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that hello server.js file is a node.js web app toobe handled by hello iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>
  
                <!-- First we consider whether hello incoming URL matches a physical file in hello /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>
  
                <!-- All other URLs are mapped toohello node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using hello following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes toorestart hello server
                * node_env: will be propagated toonode as NODE_ENV environment variable
                * debuggingEnabled - controls whether hello built-in debugger is enabled
  
              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>
  
    <span data-ttu-id="93e92-212">Om programmet använder en startpunkt än **app.js**, måste du ersätta alla förekomster av **app.js** med hello rätt startpunkt.</span><span class="sxs-lookup"><span data-stu-id="93e92-212">If your application uses an entry point other than **app.js**, you must replace all occurrences of **app.js** with hello correct entry point.</span></span> <span data-ttu-id="93e92-213">Till exempel ersätta **app.js** med **server.js**.</span><span class="sxs-lookup"><span data-stu-id="93e92-213">For example, replacing **app.js** with **server.js**.</span></span>

> [!NOTE]
> <span data-ttu-id="93e92-214">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service], där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="93e92-214">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service], where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="93e92-215">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="93e92-215">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="93e92-216">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93e92-216">Next steps</span></span>
<span data-ttu-id="93e92-217">I den här självstudiekursen du lärt dig hur toocreate en chattprogram finns i en Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="93e92-217">In this tutorial you learned how toocreate a chat application hosted in an Azure web app.</span></span> <span data-ttu-id="93e92-218">Du kan också vara värd för programmet som en Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="93e92-218">You can also host this application as an Azure Cloud Service.</span></span> <span data-ttu-id="93e92-219">Stegvisa instruktioner för hur tooaccomplish detta, se [skapa ett Node.js-chatta program med Socket.IO i Azure-molntjänst].</span><span class="sxs-lookup"><span data-stu-id="93e92-219">For steps on how tooaccomplish this, see [Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service].</span></span>

<span data-ttu-id="93e92-220">Mer information finns också hello [Node.js Developer Center].</span><span class="sxs-lookup"><span data-stu-id="93e92-220">For more information, see also hello [Node.js Developer Center].</span></span>

## <a name="whats-changed"></a><span data-ttu-id="93e92-221">Nyheter</span><span class="sxs-lookup"><span data-stu-id="93e92-221">What's changed</span></span>
* <span data-ttu-id="93e92-222">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster].</span><span class="sxs-lookup"><span data-stu-id="93e92-222">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services].</span></span>

<!-- URL List -->

[Azure Redis-Cache]: /documentation/services/redis-cache/
[App Service Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714
[prissättning för Web Apps]: http://go.microsoft.com/fwlink/?LinkId=511643
[skapa ett Node.js-chatta program med Socket.IO i Azure-molntjänst]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure hello Azure CLI]: ../cli-install-nodejs.md
[Azure App Service och dess påverkan på befintliga Azure-tjänster]: http://go.microsoft.com/fwlink/?LinkId=529714
[Node.js Developer Center]: /develop/nodejs/
[prova App Service]: https://azure.microsoft.com/try/app-service/
[instans tillhörighet i Azure webbplatser]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[skapa en cache i Azure Redis-Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[socket.io redis]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub-lagringsplatsen]: https://github.com/socketio/socket.io
[ZIP- eller GZ arkiveras versionen]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
