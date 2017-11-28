---
title: "aaaGet igång med hello Azure CDN SDK för Node.js | Microsoft Docs"
description: "Lär dig hur toowrite Node.js-program toomanage Azure CDN."
services: cdn
documentationcenter: nodejs
author: zhangmanling
manager: erikre
editor: 
ms.assetid: c4bb6a61-de3d-4f0c-9dca-202554c43dfa
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 6c805e5fb8e0b471e8b248cb2f4b29efd6c85940
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-cdn-development"></a><span data-ttu-id="959dd-103">Kom igång med Azure CDN-utveckling</span><span class="sxs-lookup"><span data-stu-id="959dd-103">Get started with Azure CDN development</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="959dd-104">Node.js</span><span class="sxs-lookup"><span data-stu-id="959dd-104">Node.js</span></span>](cdn-app-dev-node.md)
> * [<span data-ttu-id="959dd-105">.NET</span><span class="sxs-lookup"><span data-stu-id="959dd-105">.NET</span></span>](cdn-app-dev-net.md)
> 
> 

<span data-ttu-id="959dd-106">Du kan använda hello [Azure CDN SDK för Node.js](https://www.npmjs.com/package/azure-arm-cdn) tooautomate skapande och hantering av CDN-profiler och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="959dd-106">You can use hello [Azure CDN SDK for Node.js](https://www.npmjs.com/package/azure-arm-cdn) tooautomate creation and management of CDN profiles and endpoints.</span></span>  <span data-ttu-id="959dd-107">Den här kursen går igenom hello skapandet av en enkel Node.js-konsolprogram som visar flera hello tillgängliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="959dd-107">This tutorial walks through hello creation of a simple Node.js console application that demonstrates several of hello available operations.</span></span>  <span data-ttu-id="959dd-108">Den här kursen är inte avsedd toodescribe alla aspekter av hello Azure CDN SDK för Node.js i detalj.</span><span class="sxs-lookup"><span data-stu-id="959dd-108">This tutorial is not intended toodescribe all aspects of hello Azure CDN SDK for Node.js in detail.</span></span>

<span data-ttu-id="959dd-109">toocomplete den här självstudiekursen kommer du bör redan ha [Node.js](http://www.nodejs.org) **4.x.x** eller senare installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="959dd-109">toocomplete this tutorial, you should already have [Node.js](http://www.nodejs.org) **4.x.x** or higher installed and configured.</span></span>  <span data-ttu-id="959dd-110">Du kan använda valfri textredigerare som du vill toocreate Node.js-programmet.</span><span class="sxs-lookup"><span data-stu-id="959dd-110">You can use any text editor you want toocreate your Node.js application.</span></span>  <span data-ttu-id="959dd-111">toowrite den här självstudiekursen jag använde [Visual Studio Code](https://code.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="959dd-111">toowrite this tutorial, I used [Visual Studio Code](https://code.visualstudio.com).</span></span>  

> [!TIP]
> <span data-ttu-id="959dd-112">Hej [slutförda projekt från den här självstudiekursen](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) är tillgänglig för hämtning på MSDN.</span><span class="sxs-lookup"><span data-stu-id="959dd-112">hello [completed project from this tutorial](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) is available for download on MSDN.</span></span>
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a><span data-ttu-id="959dd-113">Skapa projektet och Lägg till NPM-beroenden</span><span class="sxs-lookup"><span data-stu-id="959dd-113">Create your project and add NPM dependencies</span></span>
<span data-ttu-id="959dd-114">Nu när vi har skapat en resursgrupp för våra CDN-profiler och våra Azure AD behörighet toomanage CDN programprofiler och slutpunkter inom den gruppen, kan vi börja skapa vårt program.</span><span class="sxs-lookup"><span data-stu-id="959dd-114">Now that we've created a resource group for our CDN profiles and given our Azure AD application permission toomanage CDN profiles and endpoints within that group, we can start creating our application.</span></span>

<span data-ttu-id="959dd-115">Skapa en mapp toostore ditt program.</span><span class="sxs-lookup"><span data-stu-id="959dd-115">Create a folder toostore your application.</span></span>  <span data-ttu-id="959dd-116">Ange din aktuella plats toothis ny mapp och starta projektet genom att köra från en konsol med hello Node.js verktyg i din aktuella sökväg:</span><span class="sxs-lookup"><span data-stu-id="959dd-116">From a console with hello Node.js tools in your current path, set your current location toothis new folder and initialize your project by executing:</span></span>

    npm init

<span data-ttu-id="959dd-117">Du kan sedan visas ett antal frågor tooinitialize projektet.</span><span class="sxs-lookup"><span data-stu-id="959dd-117">You will then be presented a series of questions tooinitialize your project.</span></span>  <span data-ttu-id="959dd-118">För **startpunkten**, den här kursen använder *app.js*.</span><span class="sxs-lookup"><span data-stu-id="959dd-118">For **entry point**, this tutorial uses *app.js*.</span></span>  <span data-ttu-id="959dd-119">Du kan se min alternativen i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="959dd-119">You can see my other choices in hello following example.</span></span>

![NPM init-utdata](./media/cdn-app-dev-node/cdn-npm-init.png)

<span data-ttu-id="959dd-121">Projektet nu har initierats med ett *packages.json* fil.</span><span class="sxs-lookup"><span data-stu-id="959dd-121">Our project is now initialized with a *packages.json* file.</span></span>  <span data-ttu-id="959dd-122">Vår projektet kommer toouse vissa Azure-bibliotek finns i NPM-paket.</span><span class="sxs-lookup"><span data-stu-id="959dd-122">Our project is going toouse some Azure libraries contained in NPM packages.</span></span>  <span data-ttu-id="959dd-123">Vi använder hello Azure klienten Runtime för Node.js (ms-rest-azure) och hello Azure CDN-klientbibliotek för Node.js-azure-arm-CD: n.</span><span class="sxs-lookup"><span data-stu-id="959dd-123">We'll use hello Azure Client Runtime for Node.js (ms-rest-azure) and hello Azure CDN Client Library for Node.js (azure-arm-cd).</span></span>  <span data-ttu-id="959dd-124">Lägg till dessa toohello projekt som beroenden.</span><span class="sxs-lookup"><span data-stu-id="959dd-124">Let's add those toohello project as dependencies.</span></span>

    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

<span data-ttu-id="959dd-125">Efter hello paket är klar installerar, hello *package.json* filen bör se ut ungefär toothis exempel (version siffror kan skilja sig):</span><span class="sxs-lookup"><span data-stu-id="959dd-125">After hello packages are done installing, hello *package.json* file should look similar toothis example (version numbers may differ):</span></span>

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

<span data-ttu-id="959dd-126">Slutligen med en textredigerare, skapa en tom textfil och spara den i vår projektmappen som hello rot *app.js*.</span><span class="sxs-lookup"><span data-stu-id="959dd-126">Finally, using your text editor, create a blank text file and save it in hello root of our project folder as *app.js*.</span></span>  <span data-ttu-id="959dd-127">Vi är nu redo toobegin skriva kod.</span><span class="sxs-lookup"><span data-stu-id="959dd-127">We're now ready toobegin writing code.</span></span>

## <a name="requires-constants-authentication-and-structure"></a><span data-ttu-id="959dd-128">Kräver konstanter, autentisering och struktur</span><span class="sxs-lookup"><span data-stu-id="959dd-128">Requires, constants, authentication, and structure</span></span>
<span data-ttu-id="959dd-129">Med *app.js* öppna i vår redigerare, ska få hello grundstrukturen för vårt program som har skrivits.</span><span class="sxs-lookup"><span data-stu-id="959dd-129">With *app.js* open in our editor, let's get hello basic structure of our program written.</span></span>

1. <span data-ttu-id="959dd-130">Lägg till hello ”krävs” för våra NPM paket överst hello med hello följande:</span><span class="sxs-lookup"><span data-stu-id="959dd-130">Add hello "requires" for our NPM packages at hello top with hello following:</span></span>
   
    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```
2. <span data-ttu-id="959dd-131">Vi behöver toodefine vissa konstanter våra metoder ska använda.</span><span class="sxs-lookup"><span data-stu-id="959dd-131">We need toodefine some constants our methods will use.</span></span>  <span data-ttu-id="959dd-132">Lägg till följande hello.</span><span class="sxs-lookup"><span data-stu-id="959dd-132">Add hello following.</span></span>  <span data-ttu-id="959dd-133">Vara säker på att tooreplace hello platshållare, inklusive hello  **&lt;hakparenteser&gt;**, med egna värden efter behov.</span><span class="sxs-lookup"><span data-stu-id="959dd-133">Be sure tooreplace hello placeholders, including hello **&lt;angle brackets&gt;**, with your own values as needed.</span></span>
   
    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";
   
    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```
3. <span data-ttu-id="959dd-134">Därefter vi instansiera hello CDN Hanteringsklient och ge den våra autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="959dd-134">Next, we'll instantiate hello CDN management client and give it our credentials.</span></span>
   
    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    <span data-ttu-id="959dd-135">Om du använder enskilda användarautentisering ser dessa två rader lite annorlunda.</span><span class="sxs-lookup"><span data-stu-id="959dd-135">If you are using individual user authentication, these two lines will look slightly different.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="959dd-136">Använd bara det här kodexemplet om du väljer toohave enskilda användarautentisering i stället för en tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="959dd-136">Only use this code sample if you are choosing toohave individual user authentication instead of a service principal.</span></span>  <span data-ttu-id="959dd-137">Vara försiktig tooguard dina användarautentiseringsuppgifter för enskilda och håll dem hemliga.</span><span class="sxs-lookup"><span data-stu-id="959dd-137">Be careful tooguard your individual user credentials and keep them secret.</span></span>
   > 
   > 
   
    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    <span data-ttu-id="959dd-138">Hello objekten i vara säker på att tooreplace  **&lt;hakparenteser&gt;**  med hello rätt information.</span><span class="sxs-lookup"><span data-stu-id="959dd-138">Be sure tooreplace hello items in **&lt;angle brackets&gt;** with hello correct information.</span></span>  <span data-ttu-id="959dd-139">För `<redirect URI>`, använda hello omdirigerings-URI som du angav när du registrerade hello program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="959dd-139">For `<redirect URI>`, use hello redirect URI you entered when you registered hello application in Azure AD.</span></span>
4. <span data-ttu-id="959dd-140">Vår Node.js-konsolprogram kommer tootake vissa kommandoradsparametrar.</span><span class="sxs-lookup"><span data-stu-id="959dd-140">Our Node.js console application is going tootake some command-line parameters.</span></span>  <span data-ttu-id="959dd-141">Vi ska verifiera att minst en parameter skickades.</span><span class="sxs-lookup"><span data-stu-id="959dd-141">Let's validate that at least one parameter was passed.</span></span>
   
   ```javascript
   //Collect command-line parameters
   var parms = process.argv.slice(2);
   
   //Do we have parameters?
   if(parms == null || parms.length == 0)
   {
       console.log("Not enough parameters!");
       console.log("Valid commands are list, delete, create, and purge.");
       process.exit(1);
   }
   ```
5. <span data-ttu-id="959dd-142">Som ger oss toohello huvuddelen av vårt program där vi förgrening tooother funktioner baserat på vilka parametrar skickades.</span><span class="sxs-lookup"><span data-stu-id="959dd-142">That brings us toohello main part of our program, where we branch off tooother functions based on what parameters were passed.</span></span>
   
    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;
   
        case "create":
            cdnCreate();
            break;
   
        case "delete":
            cdnDelete();
            break;
   
        case "purge":
            cdnPurge();
            break;
   
        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```
6. <span data-ttu-id="959dd-143">Vi behöver toomake hello rätt antal parametrar skickades in och visa hjälp om de inte visas korrekt på flera platser i vårt program.</span><span class="sxs-lookup"><span data-stu-id="959dd-143">At several places in our program, we'll need toomake sure hello right number of parameters were passed in and display some help if they don't look correct.</span></span>  <span data-ttu-id="959dd-144">Nu ska vi skapa funktioner toodo som.</span><span class="sxs-lookup"><span data-stu-id="959dd-144">Let's create functions toodo that.</span></span>
   
   ```javascript
   function requireParms(parmCount) {
       if(parms.length < parmCount) {
           usageHelp(parms[0].toLowerCase());
           process.exit(1);
       }
   }
   
   function usageHelp(cmd) {
       console.log("Usage for " + cmd + ":");
       switch(cmd)
       {
           case "list":
               console.log("list profiles");
               console.log("list endpoints <profile name>");
               break;
   
           case "create":
               console.log("create profile <profile name>");
               console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
               break;
   
           case "delete":
               console.log("delete profile <profile name>");
               console.log("delete endpoint <profile name> <endpoint name>");
               break;
   
           case "purge":
               console.log("purge <profile name> <endpoint name> <path>");
               break;
   
           default:
               console.log("Invalid command.");
       }
   }
   ```
7. <span data-ttu-id="959dd-145">Slutligen är hello-funktioner som vi ska använda på hello CDN management-klienten asynkron så att de behöver en metod toocall tillbaka när de är klar.</span><span class="sxs-lookup"><span data-stu-id="959dd-145">Finally, hello functions we'll be using on hello CDN management client are asynchronous, so they need a method toocall back when they're done.</span></span>  <span data-ttu-id="959dd-146">Nu skapar vi ett som kan visa hello utdata från hello CDN management-klienten (om sådan finns) och hello programmet avslutas.</span><span class="sxs-lookup"><span data-stu-id="959dd-146">Let's make one that can display hello output from hello CDN management client (if any) and exit hello program gracefully.</span></span>
   
    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

<span data-ttu-id="959dd-147">Nu när hello grundstrukturen för vårt program skrivs ska vi skapa hello-funktioner som anropas baserat på vår parametrar.</span><span class="sxs-lookup"><span data-stu-id="959dd-147">Now that hello basic structure of our program is written, we should create hello functions called based on our parameters.</span></span>

## <a name="list-cdn-profiles-and-endpoints"></a><span data-ttu-id="959dd-148">Lista CDN profiler och slutpunkter</span><span class="sxs-lookup"><span data-stu-id="959dd-148">List CDN profiles and endpoints</span></span>
<span data-ttu-id="959dd-149">Vi börjar med koden toolist vår befintliga profiler och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="959dd-149">Let's start with code toolist our existing profiles and endpoints.</span></span>  <span data-ttu-id="959dd-150">Kommentarer koden ange hello förväntades syntax så att vi kan där varje parameter går.</span><span class="sxs-lookup"><span data-stu-id="959dd-150">My code comments provide hello expected syntax so we know where each parameter goes.</span></span>

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a><span data-ttu-id="959dd-151">Skapa CDN-profiler och slutpunkter</span><span class="sxs-lookup"><span data-stu-id="959dd-151">Create CDN profiles and endpoints</span></span>
<span data-ttu-id="959dd-152">Därefter skriver vi hello funktioner toocreate profiler och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="959dd-152">Next, we'll write hello functions toocreate profiles and endpoints.</span></span>

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a><span data-ttu-id="959dd-153">Rensa en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="959dd-153">Purge an endpoint</span></span>
<span data-ttu-id="959dd-154">Under förutsättning att hello slutpunkten har skapats, en gemensam uppgift att vi kanske vill tooperform i vårt program rensa innehållet i vår slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="959dd-154">Assuming hello endpoint has been created, one common task that we might want tooperform in our program is purging content in our endpoint.</span></span>

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a><span data-ttu-id="959dd-155">Ta bort CDN-profiler och slutpunkter</span><span class="sxs-lookup"><span data-stu-id="959dd-155">Delete CDN profiles and endpoints</span></span>
<span data-ttu-id="959dd-156">hello senast funktionen vi ska ta bort slutpunkter och profiler.</span><span class="sxs-lookup"><span data-stu-id="959dd-156">hello last function we will include deletes endpoints and profiles.</span></span>

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-hello-program"></a><span data-ttu-id="959dd-157">Hello-program som körs</span><span class="sxs-lookup"><span data-stu-id="959dd-157">Running hello program</span></span>
<span data-ttu-id="959dd-158">Vi kan nu köra vårt Node.js-program med hjälp av vår favorit felsökaren eller hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="959dd-158">We can now execute our Node.js program using our favorite debugger or at hello console.</span></span>

> [!TIP]
> <span data-ttu-id="959dd-159">Om du använder Visual Studio Code som felsökningsprogrammet behöver tooset upp din miljö toopass i hello kommandoradsparametrar.</span><span class="sxs-lookup"><span data-stu-id="959dd-159">If you're using Visual Studio Code as your debugger, you'll need tooset up your environment toopass in hello command-line parameters.</span></span>  <span data-ttu-id="959dd-160">Visual Studio Code görs på hello **lanuch.json** fil.</span><span class="sxs-lookup"><span data-stu-id="959dd-160">Visual Studio Code does this in hello **lanuch.json** file.</span></span>  <span data-ttu-id="959dd-161">Leta efter en egenskap med namnet **argument** och lägga till en matris med strängvärden för parametrarna, så att det ser ut ungefär toothis: `"args": ["list", "profiles"]`.</span><span class="sxs-lookup"><span data-stu-id="959dd-161">Look for a property named **args** and add an array of string values for your parameters, so that it looks similar toothis:  `"args": ["list", "profiles"]`.</span></span>
> 
> 

<span data-ttu-id="959dd-162">Låt oss börja med att visa en lista över våra profiler.</span><span class="sxs-lookup"><span data-stu-id="959dd-162">Let's start by listing our profiles.</span></span>

![Lista över profiler](./media/cdn-app-dev-node/cdn-list-profiles.png)

<span data-ttu-id="959dd-164">Vi fick tillbaka en tom matris.</span><span class="sxs-lookup"><span data-stu-id="959dd-164">We got back an empty array.</span></span>  <span data-ttu-id="959dd-165">Eftersom vi inte har några profiler i vår resursgruppen, som förväntas.</span><span class="sxs-lookup"><span data-stu-id="959dd-165">Since we don't have any profiles in our resource group, that's expected.</span></span>  <span data-ttu-id="959dd-166">Nu ska vi skapa en profil nu.</span><span class="sxs-lookup"><span data-stu-id="959dd-166">Let's create a profile now.</span></span>

![Skapa profil](./media/cdn-app-dev-node/cdn-create-profile.png)

<span data-ttu-id="959dd-168">Nu ska vi lägga till en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="959dd-168">Now, let's add an endpoint.</span></span>

![Skapa slutpunkten](./media/cdn-app-dev-node/cdn-create-endpoint.png)

<span data-ttu-id="959dd-170">Slutligen tar du bort våra profil.</span><span class="sxs-lookup"><span data-stu-id="959dd-170">Finally, let's delete our profile.</span></span>

![Ta bort profilen](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a><span data-ttu-id="959dd-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="959dd-172">Next Steps</span></span>
<span data-ttu-id="959dd-173">toosee hello slutförts projektet från den här genomgången [hämta hello exempel](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).</span><span class="sxs-lookup"><span data-stu-id="959dd-173">toosee hello completed project from this walkthrough, [download hello sample](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).</span></span>

<span data-ttu-id="959dd-174">toosee hello referens för hello Azure CDN SDK för Node.js, visa hello [referens](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).</span><span class="sxs-lookup"><span data-stu-id="959dd-174">toosee hello reference for hello Azure CDN SDK for Node.js, view hello [reference](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).</span></span>

<span data-ttu-id="959dd-175">toofind ytterligare dokumentation om hello Azure SDK för Node.js, visa hello [fullständig referens](http://azure.github.io/azure-sdk-for-node/).</span><span class="sxs-lookup"><span data-stu-id="959dd-175">toofind additional documentation on hello Azure SDK for Node.js, view hello [full reference](http://azure.github.io/azure-sdk-for-node/).</span></span>

<span data-ttu-id="959dd-176">Hantera dina CDN-resurser med [PowerShell](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="959dd-176">Manage your CDN resources with [PowerShell](cdn-manage-powershell.md).</span></span>

