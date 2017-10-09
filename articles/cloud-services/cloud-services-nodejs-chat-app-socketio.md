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
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Skapa en Node.js-chattprogram med Socket.IO i en Azure-molntjänst
Socket.IO ger realtid kommunikation mellan mellan din node.js-server och klienter. Den här kursen får du via värdskap en socket. I/o baserat på Azure-chattprogram. Mer information om Socket.IO finns <http://socket.io/>.

En skärmbild av programmet hello slutförts understiger:

![Ett webbläsarfönster som visar hello-tjänst som finns i Azure][completed-app]  

## <a name="prerequisites"></a>Krav
Se till att hello följande produkter och versioner som är installerade toosuccessfully fullständig hello exempel i den här artikeln:

* Installera [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)
* Installera [Node.js](https://nodejs.org/download/)
* Installera [Python version 2.7.10](https://www.python.org/)

## <a name="create-a-cloud-service-project"></a>Skapa ett Molntjänstprojekt
hello skapar med följande steg hello molntjänstprojekt som är värd för hello Socket.IO program.

1. Från hello **Start-menyn** eller **startskärmen**, söka efter **Windows PowerShell**. Slutligen, högerklicka på **Windows PowerShell** och välj **kör som administratör**.
   
    ![Azure PowerShell-ikonen][powershell-menu]
2. Skapa en katalog som heter **c:\\nod**. 
   
        PS C:\> md node
3. Ändra kataloger toohello **c:\\nod** directory
   
        PS C:\> cd node
4. Ange följande kommandon toocreate en ny lösning med namnet hello **chatapp** och en arbetsroll som heter **WorkerRole1**:
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    Hello efter svar visas:
   
    ![hello utdata från den nya hello-azureservice och lägga till azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-hello-chat-example"></a>Hämta hello chatta exempel
För det här projektet använder vi hello chatt exempel från hello [Socket.IO GitHub-lagringsplatsen]. Utför följande steg toodownload hello exempel hello och lägga till den toohello projekt som du skapade tidigare.

1. Skapa en lokal kopia av hello databasen med hjälp av hello **klona** knappen. Du kan också använda hello **ZIP** knappen toodownload hello projektet.
   
   ![Ett webbläsarfönster som visar https://github.com/LearnBoost/socket.io/tree/master/examples/chat, med hello ZIP download ikon markerat][chat-example-view]
2. Navigera hello katalogstruktur lokal databas för hello tills du når hello **exempel\\chatt** directory. Kopiera hello innehållet i den här katalogen toothe **C:\\nod\\chatapp\\WorkerRole1** katalog som skapades tidigare.
   
   ![Explorer som visar hello innehållet i hello exempel\\chatt directory extraheras från hello-Arkiv][chat-contents]
   
   hello markerade objekt i hello skärmbilden ovan är hello-filer som kopierats från hello **exempel\\chatt** directory
3. I hello **C:\\nod\\chatapp\\WorkerRole1** directory, ta bort hello **server.js** filen och Byt sedan namn på hello **app.js**filen för**server.js**. Detta tar bort hello standard **server.js** fil som skapats tidigare med hello **Lägg till AzureNodeWorkerRole** cmdlet och ersätter den med programmet hello filen från hello chatt exempel.

### <a name="modify-serverjs-and-install-modules"></a>Ändra Server.js och installera moduler
Innan tester hello-programmet i hello Azure-emulatorn, måste vi göra vissa mindre ändringar. Utför hello följande steg toothe server.js-fil:

1. Öppna hello **server.js** filen i Visual Studio eller valfri textredigerare.
2. Hitta hello **modulen beroenden** avsnittet hello början av server.js och ändra hello rad som innehåller **sio = require('.. //.. lib//socket.IO')** för**sio = require('socket.io')** enligt nedan:
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. tooensure hello program lyssnar på rätt port för hello, öppna server.js i anteckningar eller ditt favoritprogram redigerare och ändra följande rad genom att ersätta **3000** med **process.env.port** enligt nedan:
   
       //app.listen(3000, function () {            //Original
       app.listen(process.env.port, function () {  //Updated
         var addr = app.address();
         console.log('   app listening on http://' + addr.address + ':' + addr.port);
       });

När du har sparat hello ändringar för**server.js**, använda hello följande steg för att installera moduler som krävs och sedan testa programmet hello i Azure-emulatorn:

1. Med hjälp av **Azure PowerShell**, ändra kataloger toohello **C:\\nod\\chatapp\\WorkerRole1** katalogen och Använd hello efter kommandot tooinstall hello moduler som krävs av programmet:
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   Detta installerar hello-moduler som anges i hello package.json-filen. Du bör se utdata liknande toothe följande efter hello-kommandot har slutförts:
   
   ![hello utdata från hello npm installationskommando][The-output-of-the-npm-install-command]
2. Eftersom det här exemplet ursprungligen var en del av hello Socket.IO GitHub-lagringsplatsen och refererar direkt till hello Socket.IO biblioteket relativ sökväg, refererades inte Socket.IO i hello package.json-filen, så vi måste installera den genom att utfärda hello följande kommando:
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Testa och distribuera
1. Starta hello-emulatorn genom att utfärda hello följande kommando:
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > Om du får problem med att starta emulatorn, t.ex.: Start AzureEmulator: ett oväntat fel inträffade.  Information: Påträffade kan ett oväntat fel hello Kommunikationsobjekt System.ServiceModel.Channels.ServiceChannel, inte användas för kommunikation eftersom den är i hello status Faulted.
   
      installera om AzureAuthoringTools v 2.7.1 och AzureComputeEmulator v 2.7 - Kontrollera att versionen matchar.
   >
   >


2. Öppna en webbläsare och gå för**http://127.0.0.1**.
3. När hello fönster i webbläsaren öppnas, ange ett smeknamn och sedan hit.
   Detta gör att du toopost meddelanden som en specifik smeknamn. tootest funktion för flera användare kan öppna ytterligare fönster med samma URL och ange olika smeknamn.
   
   ![Två webbläsarfönster som visar några meddelanden från Användare1 och Användare2](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. Stoppa hello-emulatorn genom att utfärda kommandot efter tester hello program:
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. toodeploy hello programmet tooAzure, Använd den **Publish-AzureServiceProject** cmdlet. Exempel:
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > Vara säker på att toouse ett unikt namn, annars hello publicera processen misslyckas. När hello distributionen har slutförts hello webbläsare och gå toohello distribuerade tjänsten.
   > 
   > Om du får ett felmeddelande om att hello tillhandahålls prenumerationsnamn inte finns i hello importeras publiceringsprofil, måste du hämta och importera hello publiceringsprofilen för din prenumeration innan du distribuerar tooAzure. Se hello **distribuera hello programmet tooAzure** avsnitt i [skapa och distribuera en Node.js-programmet tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 
   
   ![Ett webbläsarfönster som visar hello-tjänst som finns i Azure][completed-app]
   
   > [!NOTE]
   > Om du får ett felmeddelande om att hello tillhandahålls prenumerationsnamn inte finns i hello importeras publiceringsprofil, måste du hämta och importera hello publiceringsprofilen för din prenumeration innan du distribuerar tooAzure. Se hello **distribuera hello programmet tooAzure** avsnitt i [skapa och distribuera en Node.js-programmet tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 

Programmet körs nu på Azure och kan vidarebefordra chatt meddelanden mellan olika klienter med Socket.IO.

> [!NOTE]
> För enkelhetens skull det här exemplet är begränsad toochatting mellan användare anslutna toohello samma instans. Det innebär att om hello Molntjänsten skapar två arbetarinstanser roll, användare endast ska kunna toochat med andra anslutna toohello samma instans för worker-rollen. tooscale hello programmet toowork med flera rollinstanser, du kan använda en teknik som Service Bus tooshare hello Socket.IO arkivtillståndet över instanser. Exempel finns exempel för hello-Service Bus-köer och ämnen användning i hello [Azure SDK för Node.js GitHub-lagringsplatsen](https://github.com/WindowsAzure/azure-sdk-for-node).
> 
> 

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen du lärt dig hur toocreate grundläggande chattprogram finns i en Azure-molntjänst. toolearn hur toohost det här programmet i en Azure-webbplats finns [och skapa en Node.js-chatta program med Socket.IO på en Azure-webbplats][chatwebsite].

Mer information finns också hello [Node.js Developer Center](/develop/nodejs/).

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


