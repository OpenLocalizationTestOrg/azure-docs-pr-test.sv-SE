---
title: aaaWeb App med snabb (Node.js) | Microsoft Docs
description: "En självstudiekurs som bygger på hello cloud service självstudier och visar hur toouse hello Express-modulen."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 24f8e7ef-e90d-4554-9b1e-a9b31d5824e5
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 91921bfbe137eeca9a110d4cb18eb57b46b0060e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Skapa en Node.js-webbapp med Express på en Azure-molntjänst
Node.js innehåller en minimal uppsättning funktioner i hello core runtime.
Utvecklare kan du ofta använder 3 part moduler tooprovide ytterligare funktioner när du utvecklar ett Node.js-program. I den här självstudiekursen skapar du ett nytt program med hjälp av hello [Express] [ Express] -modul som ger ett MVC-ramverk för att skapa webbprogram för Node.js.

En skärmbild av programmet hello slutförts understiger:

![En webbläsare som visar Välkommen tooExpress i Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a>Skapa ett Molntjänstprojekt
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

Utföra hello följande steg toocreate ett nytt molntjänstprojekt med namnet 'expressapp':

1. Från hello **Start-menyn** eller **startskärmen**, söka efter **Windows PowerShell**. Slutligen, högerklicka på **Windows PowerShell** och välj **kör som administratör**.
   
    ![Azure PowerShell-ikonen](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. Ändra kataloger toohello **c:\\nod** katalog och ange sedan följande kommandon toocreate en ny lösning med namnet hello **expressapp** och en webbroll med namnet **WebRole1** :
   
        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   
    > [!NOTE]
    > Som standard **Add-AzureNodeWebRole** använder en äldre version av Node.js. Hej **Set AzureServiceProjectRole** instruktionen ovan instruerar Azure toouse v0.10.21 på noden.  Obs hello parametrar är skiftlägeskänsliga.  Du kan verifiera hello rätt version av Node.js har valts genom att kontrollera hello **motorer** egenskap i **WebRole1\package.json**.
    > 
    > 

## <a name="install-express"></a>Installera Express
1. Installera hello Express generator genom att utfärda hello följande kommando:
   
        PS C:\node\expressapp> npm install express-generator -g
   
    hello hello npm kommandots utdata ska se ut ungefär toohello resultatet nedan. 
   
    ![Windows PowerShell visar hello utdata från hello npm express installationskommando.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. Ändra kataloger toohello **WebRole1** katalogen och Använd hello express kommandot toogenerate ett nytt program:
   
        PS C:\node\expressapp\WebRole1> express
   
    Du kommer att tillfrågas toooverwrite tidigare programmet. Ange **y** eller **Ja** toocontinue. Snabb genererar hello app.js fil- och en mappstruktur för att skapa ditt program.
   
    ![hello hello express kommandots utdata](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. tooinstall ytterligare beroenden som definierats i hello package.json-fil, ange hello följande kommando:
   
       PS C:\node\expressapp\WebRole1> npm install
   
   ![hello utdata från hello npm installationskommando](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. Använd hello följande kommando toocopy hello **bin/www** filen för**server.js**. Detta är så hello Molntjänsten kan hitta hello startpunkt för programmet.
   
       PS C:\node\expressapp\WebRole1> copy bin/www server.js
   
   När det här kommandot har slutförts, bör du ha en **server.js** fil i hello WebRole1.
5. Ändra hello **server.js** tooremove en hello '.' tecken från hello följande rad.
   
       var app = require('../app');
   
   När du har gjort den här ändringen ska hello raden visas på följande sätt.
   
       var app = require('./app');
   
   Den här ändringen krävs eftersom vi har flyttat filen hello (tidigare **bin/www**,) toohello samma katalog som hello app-filen som krävs. När du har gjort den här ändringen spara hello **server.js** fil.
6. Använd följande kommando toorun hello programmet hello Azure-emulatorn hello:
   
       PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
   
    ![En webbsida som innehåller Välkommen tooexpress.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-hello-view"></a>Ändra hello vy
Nu ändra vyn toodisplay hello hälsningsmeddelande ”Välkommen tooExpress i Azure”.

1. Ange följande index.jade kommandofilen tooopen hello hello:
   
       PS C:\node\expressapp\WebRole1> notepad views/index.jade
   
   ![hello innehållet i hello index.jade fil.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)
   
   Jade är hello standard visningsmotor som används av Express-program. Mer information om hello Jade visningsmotor finns [http://jade-lang.com][http://jade-lang.com].
2. Ändra hello sista textraden genom att lägga till **i Azure**.
   
   ![Hej index.jade filen hello sista raden läser: p Välkommen för\#{title} i Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. Spara hello-filen och avsluta Anteckningar.
4. Uppdatera webbläsaren och du kommer att se dina ändringar.
   
   ![Ett webbläsarfönster hello innehåller Välkommen tooExpress i Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

När testet hello program använda hello **stoppa AzureEmulator** cmdlet toostop hello-emulatorn.

## <a name="publishing-hello-application-tooazure"></a>Publishing hello programmet tooAzure
Använd hello i hello Azure PowerShell-fönster, **Publish-AzureServiceProject** cmdlet toodeploy hello programmet tooa Molntjänsten

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

När hello distribution har slutförts, din webbläsare och visar webbsidan hello.

![En webbläsare som visar hello Express-sidan. hello URL: en anger finns nu på Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello [Node.js Developer Center](/develop/nodejs/).

[Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: http://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com


