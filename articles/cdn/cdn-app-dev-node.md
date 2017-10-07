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
# <a name="get-started-with-azure-cdn-development"></a>Kom igång med Azure CDN-utveckling
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

Du kan använda hello [Azure CDN SDK för Node.js](https://www.npmjs.com/package/azure-arm-cdn) tooautomate skapande och hantering av CDN-profiler och slutpunkter.  Den här kursen går igenom hello skapandet av en enkel Node.js-konsolprogram som visar flera hello tillgängliga åtgärder.  Den här kursen är inte avsedd toodescribe alla aspekter av hello Azure CDN SDK för Node.js i detalj.

toocomplete den här självstudiekursen kommer du bör redan ha [Node.js](http://www.nodejs.org) **4.x.x** eller senare installeras och konfigureras.  Du kan använda valfri textredigerare som du vill toocreate Node.js-programmet.  toowrite den här självstudiekursen jag använde [Visual Studio Code](https://code.visualstudio.com).  

> [!TIP]
> Hej [slutförda projekt från den här självstudiekursen](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) är tillgänglig för hämtning på MSDN.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Skapa projektet och Lägg till NPM-beroenden
Nu när vi har skapat en resursgrupp för våra CDN-profiler och våra Azure AD behörighet toomanage CDN programprofiler och slutpunkter inom den gruppen, kan vi börja skapa vårt program.

Skapa en mapp toostore ditt program.  Ange din aktuella plats toothis ny mapp och starta projektet genom att köra från en konsol med hello Node.js verktyg i din aktuella sökväg:

    npm init

Du kan sedan visas ett antal frågor tooinitialize projektet.  För **startpunkten**, den här kursen använder *app.js*.  Du kan se min alternativen i följande exempel hello.

![NPM init-utdata](./media/cdn-app-dev-node/cdn-npm-init.png)

Projektet nu har initierats med ett *packages.json* fil.  Vår projektet kommer toouse vissa Azure-bibliotek finns i NPM-paket.  Vi använder hello Azure klienten Runtime för Node.js (ms-rest-azure) och hello Azure CDN-klientbibliotek för Node.js-azure-arm-CD: n.  Lägg till dessa toohello projekt som beroenden.

    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Efter hello paket är klar installerar, hello *package.json* filen bör se ut ungefär toothis exempel (version siffror kan skilja sig):

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

Slutligen med en textredigerare, skapa en tom textfil och spara den i vår projektmappen som hello rot *app.js*.  Vi är nu redo toobegin skriva kod.

## <a name="requires-constants-authentication-and-structure"></a>Kräver konstanter, autentisering och struktur
Med *app.js* öppna i vår redigerare, ska få hello grundstrukturen för vårt program som har skrivits.

1. Lägg till hello ”krävs” för våra NPM paket överst hello med hello följande:
   
    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```
2. Vi behöver toodefine vissa konstanter våra metoder ska använda.  Lägg till följande hello.  Vara säker på att tooreplace hello platshållare, inklusive hello  **&lt;hakparenteser&gt;**, med egna värden efter behov.
   
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
3. Därefter vi instansiera hello CDN Hanteringsklient och ge den våra autentiseringsuppgifter.
   
    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Om du använder enskilda användarautentisering ser dessa två rader lite annorlunda.
   
   > [!IMPORTANT]
   > Använd bara det här kodexemplet om du väljer toohave enskilda användarautentisering i stället för en tjänstens huvudnamn.  Vara försiktig tooguard dina användarautentiseringsuppgifter för enskilda och håll dem hemliga.
   > 
   > 
   
    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Hello objekten i vara säker på att tooreplace  **&lt;hakparenteser&gt;**  med hello rätt information.  För `<redirect URI>`, använda hello omdirigerings-URI som du angav när du registrerade hello program i Azure AD.
4. Vår Node.js-konsolprogram kommer tootake vissa kommandoradsparametrar.  Vi ska verifiera att minst en parameter skickades.
   
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
5. Som ger oss toohello huvuddelen av vårt program där vi förgrening tooother funktioner baserat på vilka parametrar skickades.
   
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
6. Vi behöver toomake hello rätt antal parametrar skickades in och visa hjälp om de inte visas korrekt på flera platser i vårt program.  Nu ska vi skapa funktioner toodo som.
   
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
7. Slutligen är hello-funktioner som vi ska använda på hello CDN management-klienten asynkron så att de behöver en metod toocall tillbaka när de är klar.  Nu skapar vi ett som kan visa hello utdata från hello CDN management-klienten (om sådan finns) och hello programmet avslutas.
   
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

Nu när hello grundstrukturen för vårt program skrivs ska vi skapa hello-funktioner som anropas baserat på vår parametrar.

## <a name="list-cdn-profiles-and-endpoints"></a>Lista CDN profiler och slutpunkter
Vi börjar med koden toolist vår befintliga profiler och slutpunkter.  Kommentarer koden ange hello förväntades syntax så att vi kan där varje parameter går.

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

## <a name="create-cdn-profiles-and-endpoints"></a>Skapa CDN-profiler och slutpunkter
Därefter skriver vi hello funktioner toocreate profiler och slutpunkter.

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

## <a name="purge-an-endpoint"></a>Rensa en slutpunkt
Under förutsättning att hello slutpunkten har skapats, en gemensam uppgift att vi kanske vill tooperform i vårt program rensa innehållet i vår slutpunkt.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>Ta bort CDN-profiler och slutpunkter
hello senast funktionen vi ska ta bort slutpunkter och profiler.

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

## <a name="running-hello-program"></a>Hello-program som körs
Vi kan nu köra vårt Node.js-program med hjälp av vår favorit felsökaren eller hello-konsolen.

> [!TIP]
> Om du använder Visual Studio Code som felsökningsprogrammet behöver tooset upp din miljö toopass i hello kommandoradsparametrar.  Visual Studio Code görs på hello **lanuch.json** fil.  Leta efter en egenskap med namnet **argument** och lägga till en matris med strängvärden för parametrarna, så att det ser ut ungefär toothis: `"args": ["list", "profiles"]`.
> 
> 

Låt oss börja med att visa en lista över våra profiler.

![Lista över profiler](./media/cdn-app-dev-node/cdn-list-profiles.png)

Vi fick tillbaka en tom matris.  Eftersom vi inte har några profiler i vår resursgruppen, som förväntas.  Nu ska vi skapa en profil nu.

![Skapa profil](./media/cdn-app-dev-node/cdn-create-profile.png)

Nu ska vi lägga till en slutpunkt.

![Skapa slutpunkten](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Slutligen tar du bort våra profil.

![Ta bort profilen](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Nästa steg
toosee hello slutförts projektet från den här genomgången [hämta hello exempel](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

toosee hello referens för hello Azure CDN SDK för Node.js, visa hello [referens](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).

toofind ytterligare dokumentation om hello Azure SDK för Node.js, visa hello [fullständig referens](http://azure.github.io/azure-sdk-for-node/).

Hantera dina CDN-resurser med [PowerShell](cdn-manage-powershell.md).

