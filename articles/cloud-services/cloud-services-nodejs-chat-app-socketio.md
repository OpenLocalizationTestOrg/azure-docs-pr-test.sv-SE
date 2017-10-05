---
title: Node.js-program med Socket.io | Microsoft Docs
description: "Lär dig hur du använder socket.io i ett node.js-program i Azure."
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
ms.openlocfilehash: a85d4348a13b79b5b7542362de9956aa3398375a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a><span data-ttu-id="8f58e-103">Skapa en Node.js-chattprogram med Socket.IO i en Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="8f58e-103">Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service</span></span>
<span data-ttu-id="8f58e-104">Socket.IO ger realtid kommunikation mellan mellan din node.js-server och klienter.</span><span class="sxs-lookup"><span data-stu-id="8f58e-104">Socket.IO provides realtime communication between between your node.js server and clients.</span></span> <span data-ttu-id="8f58e-105">Den här kursen får du via värdskap en socket. I/o baserat på Azure-chattprogram.</span><span class="sxs-lookup"><span data-stu-id="8f58e-105">This tutorial will walk you through hosting a socket.IO based chat application on Azure.</span></span> <span data-ttu-id="8f58e-106">Mer information om Socket.IO finns <http://socket.io/>.</span><span class="sxs-lookup"><span data-stu-id="8f58e-106">For more information on Socket.IO, see <http://socket.io/>.</span></span>

<span data-ttu-id="8f58e-107">En skärmbild av det färdiga programmet understiger:</span><span class="sxs-lookup"><span data-stu-id="8f58e-107">A screenshot of the completed application is below:</span></span>

![Ett webbläsarfönster som visar den tjänst som finns i Azure][completed-app]  

## <a name="prerequisites"></a><span data-ttu-id="8f58e-109">Krav</span><span class="sxs-lookup"><span data-stu-id="8f58e-109">Prerequisites</span></span>
<span data-ttu-id="8f58e-110">Kontrollera att följande produkter och versioner är installerade för att slutföra exemplet i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="8f58e-110">Ensure that the following products and versions are installed to successfully complete the example in this article:</span></span>

* <span data-ttu-id="8f58e-111">Installera [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)</span><span class="sxs-lookup"><span data-stu-id="8f58e-111">Install [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)</span></span>
* <span data-ttu-id="8f58e-112">Installera [Node.js](https://nodejs.org/download/)</span><span class="sxs-lookup"><span data-stu-id="8f58e-112">Install [Node.js](https://nodejs.org/download/)</span></span>
* <span data-ttu-id="8f58e-113">Installera [Python version 2.7.10](https://www.python.org/)</span><span class="sxs-lookup"><span data-stu-id="8f58e-113">Install [Python version 2.7.10](https://www.python.org/)</span></span>

## <a name="create-a-cloud-service-project"></a><span data-ttu-id="8f58e-114">Skapa ett Molntjänstprojekt</span><span class="sxs-lookup"><span data-stu-id="8f58e-114">Create a Cloud Service Project</span></span>
<span data-ttu-id="8f58e-115">Följande steg skapar molntjänstprojektet som är värd för programmet Socket.IO.</span><span class="sxs-lookup"><span data-stu-id="8f58e-115">The following steps create the cloud service project that will host the Socket.IO application.</span></span>

1. <span data-ttu-id="8f58e-116">Från den **Start-menyn** eller **startskärmen**, söka efter **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="8f58e-116">From the **Start Menu** or **Start Screen**, search for **Windows PowerShell**.</span></span> <span data-ttu-id="8f58e-117">Slutligen, högerklicka på **Windows PowerShell** och välj **kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="8f58e-117">Finally, right-click **Windows PowerShell** and select **Run As Administrator**.</span></span>
   
    ![Azure PowerShell-ikonen][powershell-menu]
2. <span data-ttu-id="8f58e-119">Skapa en katalog som heter **c:\\nod**.</span><span class="sxs-lookup"><span data-stu-id="8f58e-119">Create a directory called **c:\\node**.</span></span> 
   
        PS C:\> md node
3. <span data-ttu-id="8f58e-120">Ändra sökvägen till den **c:\\nod** directory</span><span class="sxs-lookup"><span data-stu-id="8f58e-120">Change directories to the **c:\\node** directory</span></span>
   
        PS C:\> cd node
4. <span data-ttu-id="8f58e-121">Ange följande kommandon för att skapa en ny lösning med namnet **chatapp** och en arbetsroll som heter **WorkerRole1**:</span><span class="sxs-lookup"><span data-stu-id="8f58e-121">Enter the following commands to create a new solution named **chatapp** and a worker role named **WorkerRole1**:</span></span>
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    <span data-ttu-id="8f58e-122">Följande meddelande visas:</span><span class="sxs-lookup"><span data-stu-id="8f58e-122">You will see the following response:</span></span>
   
    ![Utdata från den nya azureservice och lägga till azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a><span data-ttu-id="8f58e-124">Hämta chatt-exempel</span><span class="sxs-lookup"><span data-stu-id="8f58e-124">Download the Chat Example</span></span>
<span data-ttu-id="8f58e-125">För det här projektet kommer vi att använda chatt-exempel från den [Socket.IO GitHub-lagringsplatsen].</span><span class="sxs-lookup"><span data-stu-id="8f58e-125">For this project, we will use the chat example from the [Socket.IO GitHub repository].</span></span> <span data-ttu-id="8f58e-126">Utför följande steg för att hämta exemplet och Lägg till den i projektet som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="8f58e-126">Perform the following steps to download the example and add it to the project you previously created.</span></span>

1. <span data-ttu-id="8f58e-127">Skapa en lokal kopia av databasen med hjälp av den **klona** knappen.</span><span class="sxs-lookup"><span data-stu-id="8f58e-127">Create a local copy of the repository by using the **Clone** button.</span></span> <span data-ttu-id="8f58e-128">Du kan också använda den **ZIP-** för att hämta projektet.</span><span class="sxs-lookup"><span data-stu-id="8f58e-128">You may also use the **ZIP** button to download the project.</span></span>
   
   ![Ett fönster i webbläsaren visa https://github.com/LearnBoost/socket.io/tree/master/examples/chat, med ikonen ZIP download markerat][chat-example-view]
2. <span data-ttu-id="8f58e-130">Navigera katalogstruktur lokal databas tills du når den **exempel\\chatt** directory.</span><span class="sxs-lookup"><span data-stu-id="8f58e-130">Navigate the directory structure of the local repository until you arrive at the **examples\\chat** directory.</span></span> <span data-ttu-id="8f58e-131">Kopiera innehållet i den här katalogen för den **C:\\nod\\chatapp\\WorkerRole1** katalog som skapades tidigare.</span><span class="sxs-lookup"><span data-stu-id="8f58e-131">Copy the contents of this directory to the **C:\\node\\chatapp\\WorkerRole1** directory created earlier.</span></span>
   
   ![Explorer visar innehållet i exemplen\\chatt directory extraheras från arkivet][chat-contents]
   
   <span data-ttu-id="8f58e-133">De markerade objekten i skärmbilden ovan är de filer som kopieras från den **exempel\\chatt** directory</span><span class="sxs-lookup"><span data-stu-id="8f58e-133">The highlighted items in the screenshot above are the files copied from the **examples\\chat** directory</span></span>
3. <span data-ttu-id="8f58e-134">I den **C:\\nod\\chatapp\\WorkerRole1** directory, ta bort den **server.js** filen och Byt sedan namn på den **app.js** filen till **server.js**.</span><span class="sxs-lookup"><span data-stu-id="8f58e-134">In the **C:\\node\\chatapp\\WorkerRole1** directory, delete the **server.js** file, and then rename the **app.js** file to **server.js**.</span></span> <span data-ttu-id="8f58e-135">Detta tar bort standard **server.js** fil som skapats tidigare av den **Lägg till AzureNodeWorkerRole** cmdlet och ersätter den med programfilen från chatt-exempel.</span><span class="sxs-lookup"><span data-stu-id="8f58e-135">This removes the default **server.js** file created previously by the **Add-AzureNodeWorkerRole** cmdlet and replaces it with the application file from the chat example.</span></span>

### <a name="modify-serverjs-and-install-modules"></a><span data-ttu-id="8f58e-136">Ändra Server.js och installera moduler</span><span class="sxs-lookup"><span data-stu-id="8f58e-136">Modify Server.js and Install Modules</span></span>
<span data-ttu-id="8f58e-137">Innan du testar programmet i Azure-emulatorn måste vi göra vissa mindre ändringar.</span><span class="sxs-lookup"><span data-stu-id="8f58e-137">Before testing the application in the Azure emulator, we must make some minor modifications.</span></span> <span data-ttu-id="8f58e-138">Utför följande steg för att server.js-fil:</span><span class="sxs-lookup"><span data-stu-id="8f58e-138">Perform the following steps to the server.js file:</span></span>

1. <span data-ttu-id="8f58e-139">Öppna den **server.js** filen i Visual Studio eller valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="8f58e-139">Open the **server.js** file in Visual Studio or any text editor.</span></span>
2. <span data-ttu-id="8f58e-140">Hitta de **modulen beroenden** avsnittet i början av server.js och ändra den rad som innehåller **sio = require('.. //.. lib//socket.IO')** till **sio = require('socket.io')** enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="8f58e-140">Find the **Module dependencies** section at the beginning of server.js and change the line containing **sio = require('..//..//lib//socket.io')** to **sio = require('socket.io')** as shown below:</span></span>
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. <span data-ttu-id="8f58e-141">Öppna server.js i anteckningar eller ditt favoritprogram redigerare för att säkerställa att programmet lyssnar på rätt port, och ändra sedan följande rad genom att ersätta **3000** med **process.env.port** enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="8f58e-141">To ensure the application listens on the correct port, open server.js in Notepad or your favorite editor, and then change the following line by replacing **3000** with **process.env.port** as shown below:</span></span>
   
       //app.listen(3000, function () {            //Original
       app.listen(process.env.port, function () {  //Updated
         var addr = app.address();
         console.log('   app listening on http://' + addr.address + ':' + addr.port);
       });

<span data-ttu-id="8f58e-142">När du har sparat ändringarna **server.js**, Använd följande steg för att installera moduler som krävs och testa programmet i Azure-emulatorn:</span><span class="sxs-lookup"><span data-stu-id="8f58e-142">After saving the changes to **server.js**, use the following steps to install required modules, and then test the application in the Azure emulator:</span></span>

1. <span data-ttu-id="8f58e-143">Med hjälp av **Azure PowerShell**, ändrar du katalog till den **C:\\nod\\chatapp\\WorkerRole1** katalogen och Använd följande kommando för att installera modulerna krävs för det här programmet:</span><span class="sxs-lookup"><span data-stu-id="8f58e-143">Using **Azure PowerShell**, change directories to the **C:\\node\\chatapp\\WorkerRole1** directory and use the following command to install the modules required by this application:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   <span data-ttu-id="8f58e-144">Detta installerar de moduler som anges i package.json-filen.</span><span class="sxs-lookup"><span data-stu-id="8f58e-144">This will install the modules listed in the package.json file.</span></span> <span data-ttu-id="8f58e-145">När kommandot har slutförts kan bör du se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="8f58e-145">After the command completes, you should see output similar to the following:</span></span>
   
   ![Utdata från npm installationskommando][The-output-of-the-npm-install-command]
2. <span data-ttu-id="8f58e-147">Eftersom det här exemplet ursprungligen var en del av Socket.IO GitHub-lagringsplatsen och refererar direkt till biblioteket Socket.IO relativ sökväg, refererades inte Socket.IO i package.json-filen, så vi måste installera den genom att följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8f58e-147">Since this example was originally a part of the Socket.IO GitHub repository, and directly referenced the Socket.IO library by relative path, Socket.IO was not referenced in the package.json file, so we must install it by issuing the following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a><span data-ttu-id="8f58e-148">Testa och distribuera</span><span class="sxs-lookup"><span data-stu-id="8f58e-148">Test and Deploy</span></span>
1. <span data-ttu-id="8f58e-149">Starta emulatorn genom att följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8f58e-149">Launch the emulator by issuing the following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > <span data-ttu-id="8f58e-150">Om du får problem med att starta emulatorn, t.ex.: Start AzureEmulator: ett oväntat fel inträffade.</span><span class="sxs-lookup"><span data-stu-id="8f58e-150">If you encounter issues with launching emulator, eg.: Start-AzureEmulator : An unexpected failure occurred.</span></span>  <span data-ttu-id="8f58e-151">Information: Påträffade kan ett oväntat fel Kommunikationsobjekt System.ServiceModel.Channels.ServiceChannel, inte användas för kommunikation eftersom den är i tillståndet Faulted.</span><span class="sxs-lookup"><span data-stu-id="8f58e-151">Details: Encountered an unexpected error The communication object,  System.ServiceModel.Channels.ServiceChannel, cannot be used for communication because it is in the Faulted state.</span></span>
   
      <span data-ttu-id="8f58e-152">installera om AzureAuthoringTools v 2.7.1 och AzureComputeEmulator v 2.7 - Kontrollera att versionen matchar.</span><span class="sxs-lookup"><span data-stu-id="8f58e-152">reinstall AzureAuthoringTools v 2.7.1 and AzureComputeEmulator v 2.7 - make sure that version matches.</span></span>
   >
   >


2. <span data-ttu-id="8f58e-153">Öppna en webbläsare och gå till **http://127.0.0.1**.</span><span class="sxs-lookup"><span data-stu-id="8f58e-153">Open a browser and navigate to **http://127.0.0.1**.</span></span>
3. <span data-ttu-id="8f58e-154">När webbläsarfönstret öppnas, ange ett smeknamn och sedan hit.</span><span class="sxs-lookup"><span data-stu-id="8f58e-154">When the browser window opens, enter a nickname and then hit enter.</span></span>
   <span data-ttu-id="8f58e-155">Detta kan du skicka meddelanden som en specifik smeknamn.</span><span class="sxs-lookup"><span data-stu-id="8f58e-155">This will allow you to post messages as a specific nickname.</span></span> <span data-ttu-id="8f58e-156">Om du vill testa funktionerna i flera användare öppna ytterligare fönster med samma URL och ange olika smeknamn.</span><span class="sxs-lookup"><span data-stu-id="8f58e-156">To test multi-user functionality, open additional browser windows using the same URL and enter different nicknames.</span></span>
   
   ![Två webbläsarfönster som visar några meddelanden från Användare1 och Användare2](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. <span data-ttu-id="8f58e-158">När du testar programmet, stoppar du emulatorn genom att utfärda kommandot:</span><span class="sxs-lookup"><span data-stu-id="8f58e-158">After testing the application, stop the emulator by issuing the following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. <span data-ttu-id="8f58e-159">Distribuera programmet till Azure med hjälp av **Publish-AzureServiceProject** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8f58e-159">To deploy the application to Azure, use the **Publish-AzureServiceProject** cmdlet.</span></span> <span data-ttu-id="8f58e-160">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8f58e-160">For example:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > <span data-ttu-id="8f58e-161">Se till att använda ett unikt namn, annars misslyckas publiceringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="8f58e-161">Be sure to use a unique name, otherwise the publish process will fail.</span></span> <span data-ttu-id="8f58e-162">När distributionen är klar, öppna webbläsaren och navigera till den distribuerade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8f58e-162">After the deployment has completed, the browser will open and navigate to the deployed service.</span></span>
   > 
   > <span data-ttu-id="8f58e-163">Om du får ett felmeddelande om att tillhandahållna prenumerations-namnet inte finns i den importera profilen, måste du hämta och importera publiceringsprofilen för din prenumeration innan du distribuerar till Azure.</span><span class="sxs-lookup"><span data-stu-id="8f58e-163">If you receive an error stating that the provided subscription name doesn't exist in the imported publish profile, you must download and import the publishing profile for your subscription before deploying to Azure.</span></span> <span data-ttu-id="8f58e-164">Finns det **distribuera programmet till Azure** avsnitt i [skapa och distribuera ett Node.js-program till en Azure-molntjänst](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span><span class="sxs-lookup"><span data-stu-id="8f58e-164">See the **Deploying the Application to Azure** section of [Build and deploy a Node.js application to an Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span></span>
   > 
   > 
   
   ![Ett webbläsarfönster som visar den tjänst som finns i Azure][completed-app]
   
   > [!NOTE]
   > <span data-ttu-id="8f58e-166">Om du får ett felmeddelande om att tillhandahållna prenumerations-namnet inte finns i den importera profilen, måste du hämta och importera publiceringsprofilen för din prenumeration innan du distribuerar till Azure.</span><span class="sxs-lookup"><span data-stu-id="8f58e-166">If you receive an error stating that the provided subscription name doesn't exist in the imported publish profile, you must download and import the publishing profile for your subscription before deploying to Azure.</span></span> <span data-ttu-id="8f58e-167">Finns det **distribuera programmet till Azure** avsnitt i [skapa och distribuera ett Node.js-program till en Azure-molntjänst](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span><span class="sxs-lookup"><span data-stu-id="8f58e-167">See the **Deploying the Application to Azure** section of [Build and deploy a Node.js application to an Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span></span>
   > 
   > 

<span data-ttu-id="8f58e-168">Programmet körs nu på Azure och kan vidarebefordra chatt meddelanden mellan olika klienter med Socket.IO.</span><span class="sxs-lookup"><span data-stu-id="8f58e-168">Your application is now running on Azure, and can relay chat messages between different clients using Socket.IO.</span></span>

> [!NOTE]
> <span data-ttu-id="8f58e-169">Det här exemplet är begränsad till chatta mellan användare som är anslutna till samma-instans för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="8f58e-169">For simplicity, this sample is limited to chatting between users connected to the same instance.</span></span> <span data-ttu-id="8f58e-170">Det innebär att om Molntjänsten skapar två arbetarinstanser roll, användare kommer bara att kunna chatta med andra anslutna till samma worker rollinstans.</span><span class="sxs-lookup"><span data-stu-id="8f58e-170">This means that if the cloud service creates two worker role instances, users will only be able to chat with others connected to the same worker role instance.</span></span> <span data-ttu-id="8f58e-171">Om du vill skala programmet att fungera med flera rollinstanser, kunde du använda en teknik som Service Bus för att dela arkivtillståndet Socket.IO över instanser.</span><span class="sxs-lookup"><span data-stu-id="8f58e-171">To scale the application to work with multiple role instances, you could use a technology like Service Bus to share the Socket.IO store state across instances.</span></span> <span data-ttu-id="8f58e-172">Exempel finns i Service Bus-köer och ämnen användning exempel i den [Azure SDK för Node.js GitHub-lagringsplatsen](https://github.com/WindowsAzure/azure-sdk-for-node).</span><span class="sxs-lookup"><span data-stu-id="8f58e-172">For examples, see the Service Bus Queues and Topics usage samples in the [Azure SDK for Node.js GitHub repository](https://github.com/WindowsAzure/azure-sdk-for-node).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="8f58e-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f58e-173">Next steps</span></span>
<span data-ttu-id="8f58e-174">I den här självstudiekursen du har lärt dig hur du skapar en grundläggande chattprogram finns i en Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="8f58e-174">In this tutorial you learned how to create a basic chat application hosted in an Azure Cloud Service.</span></span> <span data-ttu-id="8f58e-175">Information om hur som värd för det här programmet i en Azure-webbplats finns [och skapa en Node.js-chatta program med Socket.IO på en Azure-webbplats][chatwebsite].</span><span class="sxs-lookup"><span data-stu-id="8f58e-175">To learn how to host this application in an Azure Website, see [Build a Node.js Chat Application with Socket.IO on an Azure Web Site][chatwebsite].</span></span>

<span data-ttu-id="8f58e-176">Mer information finns också i [Node.js Developer Center](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="8f58e-176">For more information, see also the [Node.js Developer Center](/develop/nodejs/).</span></span>

[chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

[Azure SLA]: http://www.windowsazure.com/support/sla/
[Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
[completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
[Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
<span data-ttu-id="8f58e-177">[Socket.IO GitHub-lagringsplatsen]: https://github.com/LearnBoost/socket.io/tree/0.9.14</span><span class="sxs-lookup"><span data-stu-id="8f58e-177">[Socket.IO GitHub repository]: https://github.com/LearnBoost/socket.io/tree/0.9.14</span></span>
[Azure Considerations]: #windowsazureconsiderations
[Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
[Summary and Next Steps]: #summary
[powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

[chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
[chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png


[chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
[The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png


