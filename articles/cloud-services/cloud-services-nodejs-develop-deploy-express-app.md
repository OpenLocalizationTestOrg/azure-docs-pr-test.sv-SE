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
# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a><span data-ttu-id="33674-103">Skapa en Node.js-webbapp med Express på en Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="33674-103">Build a Node.js web application using Express on an Azure Cloud Service</span></span>
<span data-ttu-id="33674-104">Node.js innehåller en minimal uppsättning funktioner i hello core runtime.</span><span class="sxs-lookup"><span data-stu-id="33674-104">Node.js includes a minimal set of functionality in hello core runtime.</span></span>
<span data-ttu-id="33674-105">Utvecklare kan du ofta använder 3 part moduler tooprovide ytterligare funktioner när du utvecklar ett Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="33674-105">Developers often use 3rd party modules tooprovide additional functionality when developing a Node.js application.</span></span> <span data-ttu-id="33674-106">I den här självstudiekursen skapar du ett nytt program med hjälp av hello [Express] [ Express] -modul som ger ett MVC-ramverk för att skapa webbprogram för Node.js.</span><span class="sxs-lookup"><span data-stu-id="33674-106">In this tutorial you will create a new application using hello [Express][Express] module, which provides an MVC framework for creating Node.js web applications.</span></span>

<span data-ttu-id="33674-107">En skärmbild av programmet hello slutförts understiger:</span><span class="sxs-lookup"><span data-stu-id="33674-107">A screenshot of hello completed application is below:</span></span>

![En webbläsare som visar Välkommen tooExpress i Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a><span data-ttu-id="33674-109">Skapa ett Molntjänstprojekt</span><span class="sxs-lookup"><span data-stu-id="33674-109">Create a Cloud Service Project</span></span>
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

<span data-ttu-id="33674-110">Utföra hello följande steg toocreate ett nytt molntjänstprojekt med namnet 'expressapp':</span><span class="sxs-lookup"><span data-stu-id="33674-110">Perform hello following steps toocreate a new cloud service project named 'expressapp':</span></span>

1. <span data-ttu-id="33674-111">Från hello **Start-menyn** eller **startskärmen**, söka efter **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="33674-111">From hello **Start Menu** or **Start Screen**, search for **Windows PowerShell**.</span></span> <span data-ttu-id="33674-112">Slutligen, högerklicka på **Windows PowerShell** och välj **kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="33674-112">Finally, right-click **Windows PowerShell** and select **Run As Administrator**.</span></span>
   
    ![Azure PowerShell-ikonen](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. <span data-ttu-id="33674-114">Ändra kataloger toohello **c:\\nod** katalog och ange sedan följande kommandon toocreate en ny lösning med namnet hello **expressapp** och en webbroll med namnet **WebRole1** :</span><span class="sxs-lookup"><span data-stu-id="33674-114">Change directories toohello **c:\\node** directory and then enter hello following commands toocreate a new solution named **expressapp** and a web role named **WebRole1**:</span></span>
   
        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   
    > [!NOTE]
    > <span data-ttu-id="33674-115">Som standard **Add-AzureNodeWebRole** använder en äldre version av Node.js.</span><span class="sxs-lookup"><span data-stu-id="33674-115">By default, **Add-AzureNodeWebRole** uses an older version of Node.js.</span></span> <span data-ttu-id="33674-116">Hej **Set AzureServiceProjectRole** instruktionen ovan instruerar Azure toouse v0.10.21 på noden.</span><span class="sxs-lookup"><span data-stu-id="33674-116">hello **Set-AzureServiceProjectRole** statement above instructs Azure toouse v0.10.21 of Node.</span></span>  <span data-ttu-id="33674-117">Obs hello parametrar är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="33674-117">Note hello parameters are case-sensitive.</span></span>  <span data-ttu-id="33674-118">Du kan verifiera hello rätt version av Node.js har valts genom att kontrollera hello **motorer** egenskap i **WebRole1\package.json**.</span><span class="sxs-lookup"><span data-stu-id="33674-118">You can verify hello correct version of Node.js has been selected by checking hello **engines** property in **WebRole1\package.json**.</span></span>
    > 
    > 

## <a name="install-express"></a><span data-ttu-id="33674-119">Installera Express</span><span class="sxs-lookup"><span data-stu-id="33674-119">Install Express</span></span>
1. <span data-ttu-id="33674-120">Installera hello Express generator genom att utfärda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="33674-120">Install hello Express generator by issuing hello following command:</span></span>
   
        PS C:\node\expressapp> npm install express-generator -g
   
    <span data-ttu-id="33674-121">hello hello npm kommandots utdata ska se ut ungefär toohello resultatet nedan.</span><span class="sxs-lookup"><span data-stu-id="33674-121">hello output of hello npm command should look similar toohello result below.</span></span> 
   
    ![Windows PowerShell visar hello utdata från hello npm express installationskommando.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. <span data-ttu-id="33674-123">Ändra kataloger toohello **WebRole1** katalogen och Använd hello express kommandot toogenerate ett nytt program:</span><span class="sxs-lookup"><span data-stu-id="33674-123">Change directories toohello **WebRole1** directory and use hello express command toogenerate a new application:</span></span>
   
        PS C:\node\expressapp\WebRole1> express
   
    <span data-ttu-id="33674-124">Du kommer att tillfrågas toooverwrite tidigare programmet.</span><span class="sxs-lookup"><span data-stu-id="33674-124">You will be prompted toooverwrite your earlier application.</span></span> <span data-ttu-id="33674-125">Ange **y** eller **Ja** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="33674-125">Enter **y** or **yes** toocontinue.</span></span> <span data-ttu-id="33674-126">Snabb genererar hello app.js fil- och en mappstruktur för att skapa ditt program.</span><span class="sxs-lookup"><span data-stu-id="33674-126">Express will generate hello app.js file and a folder structure for building your application.</span></span>
   
    ![hello hello express kommandots utdata](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. <span data-ttu-id="33674-128">tooinstall ytterligare beroenden som definierats i hello package.json-fil, ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="33674-128">tooinstall additional dependencies defined in hello package.json file, enter hello following command:</span></span>
   
       PS C:\node\expressapp\WebRole1> npm install
   
   ![hello utdata från hello npm installationskommando](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. <span data-ttu-id="33674-130">Använd hello följande kommando toocopy hello **bin/www** filen för**server.js**.</span><span class="sxs-lookup"><span data-stu-id="33674-130">Use hello following command toocopy hello **bin/www** file too**server.js**.</span></span> <span data-ttu-id="33674-131">Detta är så hello Molntjänsten kan hitta hello startpunkt för programmet.</span><span class="sxs-lookup"><span data-stu-id="33674-131">This is so hello cloud service can find hello entry point for this application.</span></span>
   
       PS C:\node\expressapp\WebRole1> copy bin/www server.js
   
   <span data-ttu-id="33674-132">När det här kommandot har slutförts, bör du ha en **server.js** fil i hello WebRole1.</span><span class="sxs-lookup"><span data-stu-id="33674-132">After this command completes, you should have a **server.js** file in hello WebRole1 directory.</span></span>
5. <span data-ttu-id="33674-133">Ändra hello **server.js** tooremove en hello '.' tecken från hello följande rad.</span><span class="sxs-lookup"><span data-stu-id="33674-133">Modify hello **server.js** tooremove one of hello '.' characters from hello following line.</span></span>
   
       var app = require('../app');
   
   <span data-ttu-id="33674-134">När du har gjort den här ändringen ska hello raden visas på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="33674-134">After making this modification, hello line should appear as follows.</span></span>
   
       var app = require('./app');
   
   <span data-ttu-id="33674-135">Den här ändringen krävs eftersom vi har flyttat filen hello (tidigare **bin/www**,) toohello samma katalog som hello app-filen som krävs.</span><span class="sxs-lookup"><span data-stu-id="33674-135">This change is required since we moved hello file (formerly **bin/www**,) toohello same directory as hello app file being required.</span></span> <span data-ttu-id="33674-136">När du har gjort den här ändringen spara hello **server.js** fil.</span><span class="sxs-lookup"><span data-stu-id="33674-136">After making this change, save hello **server.js** file.</span></span>
6. <span data-ttu-id="33674-137">Använd följande kommando toorun hello programmet hello Azure-emulatorn hello:</span><span class="sxs-lookup"><span data-stu-id="33674-137">Use hello following command toorun hello application in hello Azure emulator:</span></span>
   
       PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
   
    ![En webbsida som innehåller Välkommen tooexpress.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-hello-view"></a><span data-ttu-id="33674-139">Ändra hello vy</span><span class="sxs-lookup"><span data-stu-id="33674-139">Modifying hello View</span></span>
<span data-ttu-id="33674-140">Nu ändra vyn toodisplay hello hälsningsmeddelande ”Välkommen tooExpress i Azure”.</span><span class="sxs-lookup"><span data-stu-id="33674-140">Now modify hello view toodisplay hello message "Welcome tooExpress in Azure".</span></span>

1. <span data-ttu-id="33674-141">Ange följande index.jade kommandofilen tooopen hello hello:</span><span class="sxs-lookup"><span data-stu-id="33674-141">Enter hello following command tooopen hello index.jade file:</span></span>
   
       PS C:\node\expressapp\WebRole1> notepad views/index.jade
   
   ![hello innehållet i hello index.jade fil.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)
   
   <span data-ttu-id="33674-143">Jade är hello standard visningsmotor som används av Express-program.</span><span class="sxs-lookup"><span data-stu-id="33674-143">Jade is hello default view engine used by Express applications.</span></span> <span data-ttu-id="33674-144">Mer information om hello Jade visningsmotor finns [http://jade-lang.com][http://jade-lang.com].</span><span class="sxs-lookup"><span data-stu-id="33674-144">For more information on hello Jade view engine, see [http://jade-lang.com][http://jade-lang.com].</span></span>
2. <span data-ttu-id="33674-145">Ändra hello sista textraden genom att lägga till **i Azure**.</span><span class="sxs-lookup"><span data-stu-id="33674-145">Modify hello last line of text by appending **in Azure**.</span></span>
   
   ![Hej index.jade filen hello sista raden läser: p Välkommen för\#{title} i Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. <span data-ttu-id="33674-147">Spara hello-filen och avsluta Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="33674-147">Save hello file and exit Notepad.</span></span>
4. <span data-ttu-id="33674-148">Uppdatera webbläsaren och du kommer att se dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="33674-148">Refresh your browser and you will see your changes.</span></span>
   
   ![Ett webbläsarfönster hello innehåller Välkommen tooExpress i Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

<span data-ttu-id="33674-150">När testet hello program använda hello **stoppa AzureEmulator** cmdlet toostop hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="33674-150">After testing hello application, use hello **Stop-AzureEmulator** cmdlet toostop hello emulator.</span></span>

## <a name="publishing-hello-application-tooazure"></a><span data-ttu-id="33674-151">Publishing hello programmet tooAzure</span><span class="sxs-lookup"><span data-stu-id="33674-151">Publishing hello Application tooAzure</span></span>
<span data-ttu-id="33674-152">Använd hello i hello Azure PowerShell-fönster, **Publish-AzureServiceProject** cmdlet toodeploy hello programmet tooa Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="33674-152">In hello Azure PowerShell window, use hello **Publish-AzureServiceProject** cmdlet toodeploy hello application tooa cloud service</span></span>

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

<span data-ttu-id="33674-153">När hello distribution har slutförts, din webbläsare och visar webbsidan hello.</span><span class="sxs-lookup"><span data-stu-id="33674-153">Once hello deployment operation completes, your browser will open and display hello web page.</span></span>

![En webbläsare som visar hello Express-sidan.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a><span data-ttu-id="33674-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="33674-156">Next steps</span></span>
<span data-ttu-id="33674-157">Mer information finns i hello [Node.js Developer Center](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="33674-157">For more information, see hello [Node.js Developer Center](/develop/nodejs/).</span></span>

[Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: http://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com


