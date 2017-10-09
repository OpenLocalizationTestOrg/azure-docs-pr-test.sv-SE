---
title: aaaNode.js program med Socket.io | Microsoft Docs
description: "Lär dig hur toouse socket.io i ett node.js-program finns i Azure."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 7f9435e0-7732-4aa1-a4df-ea0e894b847f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 47c6c4a748938959315b880340f41f31faab4ea9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a><span data-ttu-id="1cc64-103">Skapa en Node.js-chattprogram med Socket.IO i en Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="1cc64-103">Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service</span></span>
<span data-ttu-id="1cc64-104">Socket.IO ger realtid kommunikation mellan mellan din node.js-server och klienter.</span><span class="sxs-lookup"><span data-stu-id="1cc64-104">Socket.IO provides realtime communication between between your node.js server and clients.</span></span> <span data-ttu-id="1cc64-105">Den här kursen får du via värdskap en socket. I/o baserat på Azure-chattprogram.</span><span class="sxs-lookup"><span data-stu-id="1cc64-105">This tutorial will walk you through hosting a socket.IO based chat application on Azure.</span></span> <span data-ttu-id="1cc64-106">Mer information om Socket.IO finns <http://socket.io/>.</span><span class="sxs-lookup"><span data-stu-id="1cc64-106">For more information on Socket.IO, see <http://socket.io/>.</span></span>

<span data-ttu-id="1cc64-107">En skärmbild av programmet hello slutförts understiger:</span><span class="sxs-lookup"><span data-stu-id="1cc64-107">A screenshot of hello completed application is below:</span></span>

![Ett webbläsarfönster som visar hello-tjänst som finns i Azure][completed-app]  

## <a name="prerequisites"></a><span data-ttu-id="1cc64-109">Krav</span><span class="sxs-lookup"><span data-stu-id="1cc64-109">Prerequisites</span></span>
<span data-ttu-id="1cc64-110">Se till att hello följande produkter och versioner som är installerade toosuccessfully fullständig hello exempel i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="1cc64-110">Ensure that hello following products and versions are installed toosuccessfully complete hello example in this article:</span></span>

* <span data-ttu-id="1cc64-111">Installera [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)</span><span class="sxs-lookup"><span data-stu-id="1cc64-111">Install [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)</span></span>
* <span data-ttu-id="1cc64-112">Installera [Node.js](https://nodejs.org/download/)</span><span class="sxs-lookup"><span data-stu-id="1cc64-112">Install [Node.js](https://nodejs.org/download/)</span></span>
* <span data-ttu-id="1cc64-113">Installera [Python version 2.7.10](https://www.python.org/)</span><span class="sxs-lookup"><span data-stu-id="1cc64-113">Install [Python version 2.7.10](https://www.python.org/)</span></span>

## <a name="create-a-cloud-service-project"></a><span data-ttu-id="1cc64-114">Skapa ett Molntjänstprojekt</span><span class="sxs-lookup"><span data-stu-id="1cc64-114">Create a Cloud Service Project</span></span>
<span data-ttu-id="1cc64-115">hello skapar med följande steg hello molntjänstprojekt som är värd för hello Socket.IO program.</span><span class="sxs-lookup"><span data-stu-id="1cc64-115">hello following steps create hello cloud service project that will host hello Socket.IO application.</span></span>

1. <span data-ttu-id="1cc64-116">Från hello **Start-menyn** eller **startskärmen**, söka efter **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="1cc64-116">From hello **Start Menu** or **Start Screen**, search for **Windows PowerShell**.</span></span> <span data-ttu-id="1cc64-117">Slutligen, högerklicka på **Windows PowerShell** och välj **kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="1cc64-117">Finally, right-click **Windows PowerShell** and select **Run As Administrator**.</span></span>
   
    ![Azure PowerShell-ikonen][powershell-menu]
2. <span data-ttu-id="1cc64-119">Skapa en katalog som heter **c:\\nod**.</span><span class="sxs-lookup"><span data-stu-id="1cc64-119">Create a directory called **c:\\node**.</span></span> 
   
        PS C:\> md node
3. <span data-ttu-id="1cc64-120">Ändra kataloger toohello **c:\\nod** directory</span><span class="sxs-lookup"><span data-stu-id="1cc64-120">Change directories toohello **c:\\node** directory</span></span>
   
        PS C:\> cd node
4. <span data-ttu-id="1cc64-121">Ange följande kommandon toocreate en ny lösning med namnet hello **chatapp** och en arbetsroll som heter **WorkerRole1**:</span><span class="sxs-lookup"><span data-stu-id="1cc64-121">Enter hello following commands toocreate a new solution named **chatapp** and a worker role named **WorkerRole1**:</span></span>
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    <span data-ttu-id="1cc64-122">Hello efter svar visas:</span><span class="sxs-lookup"><span data-stu-id="1cc64-122">You will see hello following response:</span></span>
   
    ![hello utdata från den nya hello-azureservice och lägga till azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-hello-chat-example"></a><span data-ttu-id="1cc64-124">Hämta hello chatta exempel</span><span class="sxs-lookup"><span data-stu-id="1cc64-124">Download hello Chat Example</span></span>
<span data-ttu-id="1cc64-125">För det här projektet använder vi hello chatt exempel från hello [Socket.IO GitHub-lagringsplatsen].</span><span class="sxs-lookup"><span data-stu-id="1cc64-125">For this project, we will use hello chat example from hello [Socket.IO GitHub repository].</span></span> <span data-ttu-id="1cc64-126">Utför följande steg toodownload hello exempel hello och lägga till den toohello projekt som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="1cc64-126">Perform hello following steps toodownload hello example and add it toohello project you previously created.</span></span>

1. <span data-ttu-id="1cc64-127">Skapa en lokal kopia av hello databasen med hjälp av hello **klona** knappen.</span><span class="sxs-lookup"><span data-stu-id="1cc64-127">Create a local copy of hello repository by using hello **Clone** button.</span></span> <span data-ttu-id="1cc64-128">Du kan också använda hello **ZIP** knappen toodownload hello projektet.</span><span class="sxs-lookup"><span data-stu-id="1cc64-128">You may also use hello **ZIP** button toodownload hello project.</span></span>
   
   ![Ett webbläsarfönster som visar https://github.com/LearnBoost/socket.io/tree/master/examples/chat, med hello ZIP download ikon markerat][chat-example-view]
2. <span data-ttu-id="1cc64-130">Navigera hello katalogstruktur lokal databas för hello tills du når hello **exempel\\chatt** directory.</span><span class="sxs-lookup"><span data-stu-id="1cc64-130">Navigate hello directory structure of hello local repository until you arrive at hello **examples\\chat** directory.</span></span> <span data-ttu-id="1cc64-131">Kopiera hello innehållet i den här katalogen toothe **C:\\nod\\chatapp\\WorkerRole1** katalog som skapades tidigare.</span><span class="sxs-lookup"><span data-stu-id="1cc64-131">Copy hello contents of this directory toothe **C:\\node\\chatapp\\WorkerRole1** directory created earlier.</span></span>
   
   ![Explorer som visar hello innehållet i hello exempel\\chatt directory extraheras från hello-Arkiv][chat-contents]
   
   <span data-ttu-id="1cc64-133">hello markerade objekt i hello skärmbilden ovan är hello-filer som kopierats från hello **exempel\\chatt** directory</span><span class="sxs-lookup"><span data-stu-id="1cc64-133">hello highlighted items in hello screenshot above are hello files copied from hello **examples\\chat** directory</span></span>
3. <span data-ttu-id="1cc64-134">I hello **C:\\nod\\chatapp\\WorkerRole1** directory, ta bort hello **server.js** filen och Byt sedan namn på hello **app.js**filen för**server.js**.</span><span class="sxs-lookup"><span data-stu-id="1cc64-134">In hello **C:\\node\\chatapp\\WorkerRole1** directory, delete hello **server.js** file, and then rename hello **app.js** file too**server.js**.</span></span> <span data-ttu-id="1cc64-135">Detta tar bort hello standard **server.js** fil som skapats tidigare med hello **Lägg till AzureNodeWorkerRole** cmdlet och ersätter den med programmet hello filen från hello chatt exempel.</span><span class="sxs-lookup"><span data-stu-id="1cc64-135">This removes hello default **server.js** file created previously by hello **Add-AzureNodeWorkerRole** cmdlet and replaces it with hello application file from hello chat example.</span></span>

### <a name="modify-serverjs-and-install-modules"></a><span data-ttu-id="1cc64-136">Ändra Server.js och installera moduler</span><span class="sxs-lookup"><span data-stu-id="1cc64-136">Modify Server.js and Install Modules</span></span>
<span data-ttu-id="1cc64-137">Innan tester hello-programmet i hello Azure-emulatorn, måste vi göra vissa mindre ändringar.</span><span class="sxs-lookup"><span data-stu-id="1cc64-137">Before testing hello application in hello Azure emulator, we must make some minor modifications.</span></span> <span data-ttu-id="1cc64-138">Utför hello följande steg toothe server.js-fil:</span><span class="sxs-lookup"><span data-stu-id="1cc64-138">Perform hello following steps toothe server.js file:</span></span>

1. <span data-ttu-id="1cc64-139">Öppna hello **server.js** filen i Visual Studio eller valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="1cc64-139">Open hello **server.js** file in Visual Studio or any text editor.</span></span>
2. <span data-ttu-id="1cc64-140">Hitta hello **modulen beroenden** avsnittet hello början av server.js och ändra hello rad som innehåller **sio = require('.. //.. lib//socket.IO')** för**sio = require('socket.io')** enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="1cc64-140">Find hello **Module dependencies** section at hello beginning of server.js and change hello line containing **sio = require('..//..//lib//socket.io')** too**sio = require('socket.io')** as shown below:</span></span>
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. <span data-ttu-id="1cc64-141">tooensure hello program lyssnar på rätt port för hello, öppna server.js i anteckningar eller ditt favoritprogram redigerare och ändra följande rad genom att ersätta **3000** med **process.env.port** enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="1cc64-141">tooensure hello application listens on hello correct port, open server.js in Notepad or your favorite editor, and then change the following line by replacing **3000** with **process.env.port** as shown below:</span></span>
   
       //app.listen(3000, function () {            //Original
       app.listen(process.env.port, function () {  //Updated
         var addr = app.address();
         console.log('   app listening on http://' + addr.address + ':' + addr.port);
       });

<span data-ttu-id="1cc64-142">När du har sparat hello ändringar för**server.js**, använda hello följande steg för att installera moduler som krävs och sedan testa programmet hello i Azure-emulatorn:</span><span class="sxs-lookup"><span data-stu-id="1cc64-142">After saving hello changes too**server.js**, use hello following steps to install required modules, and then test hello application in the Azure emulator:</span></span>

1. <span data-ttu-id="1cc64-143">Med hjälp av **Azure PowerShell**, ändra kataloger toohello **C:\\nod\\chatapp\\WorkerRole1** katalogen och Använd hello efter kommandot tooinstall hello moduler som krävs av programmet:</span><span class="sxs-lookup"><span data-stu-id="1cc64-143">Using **Azure PowerShell**, change directories toohello **C:\\node\\chatapp\\WorkerRole1** directory and use hello following command tooinstall hello modules required by this application:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   <span data-ttu-id="1cc64-144">Detta installerar hello-moduler som anges i hello package.json-filen.</span><span class="sxs-lookup"><span data-stu-id="1cc64-144">This will install hello modules listed in hello package.json file.</span></span> <span data-ttu-id="1cc64-145">Du bör se utdata liknande toothe följande efter hello-kommandot har slutförts:</span><span class="sxs-lookup"><span data-stu-id="1cc64-145">After hello command completes, you should see output similar toothe following:</span></span>
   
   ![hello utdata från hello npm installationskommando][The-output-of-the-npm-install-command]
2. <span data-ttu-id="1cc64-147">Eftersom det här exemplet ursprungligen var en del av hello Socket.IO GitHub-lagringsplatsen och refererar direkt till hello Socket.IO biblioteket relativ sökväg, refererades inte Socket.IO i hello package.json-filen, så vi måste installera den genom att utfärda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1cc64-147">Since this example was originally a part of hello Socket.IO GitHub repository, and directly referenced hello Socket.IO library by relative path, Socket.IO was not referenced in hello package.json file, so we must install it by issuing hello following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a><span data-ttu-id="1cc64-148">Testa och distribuera</span><span class="sxs-lookup"><span data-stu-id="1cc64-148">Test and Deploy</span></span>
1. <span data-ttu-id="1cc64-149">Starta hello-emulatorn genom att utfärda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1cc64-149">Launch hello emulator by issuing hello following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > <span data-ttu-id="1cc64-150">Om du får problem med att starta emulatorn, t.ex.: Start AzureEmulator: ett oväntat fel inträffade.</span><span class="sxs-lookup"><span data-stu-id="1cc64-150">If you encounter issues with launching emulator, eg.: Start-AzureEmulator : An unexpected failure occurred.</span></span>  <span data-ttu-id="1cc64-151">Information: Påträffade kan ett oväntat fel hello Kommunikationsobjekt System.ServiceModel.Channels.ServiceChannel, inte användas för kommunikation eftersom den är i hello status Faulted.</span><span class="sxs-lookup"><span data-stu-id="1cc64-151">Details: Encountered an unexpected error hello communication object,  System.ServiceModel.Channels.ServiceChannel, cannot be used for communication because it is in hello Faulted state.</span></span>
   
      <span data-ttu-id="1cc64-152">installera om AzureAuthoringTools v 2.7.1 och AzureComputeEmulator v 2.7 - Kontrollera att versionen matchar.</span><span class="sxs-lookup"><span data-stu-id="1cc64-152">reinstall AzureAuthoringTools v 2.7.1 and AzureComputeEmulator v 2.7 - make sure that version matches.</span></span>
   >
   >


2. <span data-ttu-id="1cc64-153">Öppna en webbläsare och gå för**http://127.0.0.1**.</span><span class="sxs-lookup"><span data-stu-id="1cc64-153">Open a browser and navigate too**http://127.0.0.1**.</span></span>
3. <span data-ttu-id="1cc64-154">När hello fönster i webbläsaren öppnas, ange ett smeknamn och sedan hit.</span><span class="sxs-lookup"><span data-stu-id="1cc64-154">When hello browser window opens, enter a nickname and then hit enter.</span></span>
   <span data-ttu-id="1cc64-155">Detta gör att du toopost meddelanden som en specifik smeknamn.</span><span class="sxs-lookup"><span data-stu-id="1cc64-155">This will allow you toopost messages as a specific nickname.</span></span> <span data-ttu-id="1cc64-156">tootest funktion för flera användare kan öppna ytterligare fönster med samma URL och ange olika smeknamn.</span><span class="sxs-lookup"><span data-stu-id="1cc64-156">tootest multi-user functionality, open additional browser windows using the same URL and enter different nicknames.</span></span>
   
   ![Två webbläsarfönster som visar några meddelanden från Användare1 och Användare2](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. <span data-ttu-id="1cc64-158">Stoppa hello-emulatorn genom att utfärda kommandot efter tester hello program:</span><span class="sxs-lookup"><span data-stu-id="1cc64-158">After testing hello application, stop hello emulator by issuing the following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. <span data-ttu-id="1cc64-159">toodeploy hello programmet tooAzure, Använd den **Publish-AzureServiceProject** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1cc64-159">toodeploy hello application tooAzure, use the **Publish-AzureServiceProject** cmdlet.</span></span> <span data-ttu-id="1cc64-160">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1cc64-160">For example:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > <span data-ttu-id="1cc64-161">Vara säker på att toouse ett unikt namn, annars hello publicera processen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="1cc64-161">Be sure toouse a unique name, otherwise hello publish process will fail.</span></span> <span data-ttu-id="1cc64-162">När hello distributionen har slutförts hello webbläsare och gå toohello distribuerade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1cc64-162">After hello deployment has completed, hello browser will open and navigate toohello deployed service.</span></span>
   > 
   > <span data-ttu-id="1cc64-163">Om du får ett felmeddelande om att hello tillhandahålls prenumerationsnamn inte finns i hello importeras publiceringsprofil, måste du hämta och importera hello publiceringsprofilen för din prenumeration innan du distribuerar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1cc64-163">If you receive an error stating that hello provided subscription name doesn't exist in hello imported publish profile, you must download and import hello publishing profile for your subscription before deploying tooAzure.</span></span> <span data-ttu-id="1cc64-164">Se hello **distribuera hello programmet tooAzure** avsnitt i [skapa och distribuera en Node.js-programmet tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span><span class="sxs-lookup"><span data-stu-id="1cc64-164">See hello **Deploying hello Application tooAzure** section of [Build and deploy a Node.js application tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span></span>
   > 
   > 
   
   ![Ett webbläsarfönster som visar hello-tjänst som finns i Azure][completed-app]
   
   > [!NOTE]
   > <span data-ttu-id="1cc64-166">Om du får ett felmeddelande om att hello tillhandahålls prenumerationsnamn inte finns i hello importeras publiceringsprofil, måste du hämta och importera hello publiceringsprofilen för din prenumeration innan du distribuerar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1cc64-166">If you receive an error stating that hello provided subscription name doesn't exist in hello imported publish profile, you must download and import hello publishing profile for your subscription before deploying tooAzure.</span></span> <span data-ttu-id="1cc64-167">Se hello **distribuera hello programmet tooAzure** avsnitt i [skapa och distribuera en Node.js-programmet tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span><span class="sxs-lookup"><span data-stu-id="1cc64-167">See hello **Deploying hello Application tooAzure** section of [Build and deploy a Node.js application tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span></span>
   > 
   > 

<span data-ttu-id="1cc64-168">Programmet körs nu på Azure och kan vidarebefordra chatt meddelanden mellan olika klienter med Socket.IO.</span><span class="sxs-lookup"><span data-stu-id="1cc64-168">Your application is now running on Azure, and can relay chat messages between different clients using Socket.IO.</span></span>

> [!NOTE]
> <span data-ttu-id="1cc64-169">För enkelhetens skull det här exemplet är begränsad toochatting mellan användare anslutna toohello samma instans.</span><span class="sxs-lookup"><span data-stu-id="1cc64-169">For simplicity, this sample is limited toochatting between users connected toohello same instance.</span></span> <span data-ttu-id="1cc64-170">Det innebär att om hello Molntjänsten skapar två arbetarinstanser roll, användare endast ska kunna toochat med andra anslutna toohello samma instans för worker-rollen.</span><span class="sxs-lookup"><span data-stu-id="1cc64-170">This means that if hello cloud service creates two worker role instances, users will only be able toochat with others connected toohello same worker role instance.</span></span> <span data-ttu-id="1cc64-171">tooscale hello programmet toowork med flera rollinstanser, du kan använda en teknik som Service Bus tooshare hello Socket.IO arkivtillståndet över instanser.</span><span class="sxs-lookup"><span data-stu-id="1cc64-171">tooscale hello application toowork with multiple role instances, you could use a technology like Service Bus tooshare hello Socket.IO store state across instances.</span></span> <span data-ttu-id="1cc64-172">Exempel finns exempel för hello-Service Bus-köer och ämnen användning i hello [Azure SDK för Node.js GitHub-lagringsplatsen](https://github.com/WindowsAzure/azure-sdk-for-node).</span><span class="sxs-lookup"><span data-stu-id="1cc64-172">For examples, see hello Service Bus Queues and Topics usage samples in hello [Azure SDK for Node.js GitHub repository](https://github.com/WindowsAzure/azure-sdk-for-node).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="1cc64-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1cc64-173">Next steps</span></span>
<span data-ttu-id="1cc64-174">I den här självstudiekursen du lärt dig hur toocreate grundläggande chattprogram finns i en Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="1cc64-174">In this tutorial you learned how toocreate a basic chat application hosted in an Azure Cloud Service.</span></span> <span data-ttu-id="1cc64-175">toolearn hur toohost det här programmet i en Azure-webbplats finns [och skapa en Node.js-chatta program med Socket.IO på en Azure-webbplats][chatwebsite].</span><span class="sxs-lookup"><span data-stu-id="1cc64-175">toolearn how toohost this application in an Azure Website, see [Build a Node.js Chat Application with Socket.IO on an Azure Web Site][chatwebsite].</span></span>

<span data-ttu-id="1cc64-176">Mer information finns också hello [Node.js Developer Center](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="1cc64-176">For more information, see also hello [Node.js Developer Center](/develop/nodejs/).</span></span>

[chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

[Azure SLA]: http://www.windowsazure.com/support/sla/
[Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
[completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
[Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Socket.IO GitHub-lagringsplatsen]: https://github.com/LearnBoost/socket.io/tree/0.9.14
[Azure Considerations]: #windowsazureconsiderations
[Hosting hello Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
[Summary and Next Steps]: #summary
[powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

[chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
[chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png


[chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
[The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
[hello output of hello Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png


