---
title: aaaNode.js Getting Started Guide | Microsoft Docs
description: "Lär dig hur toocreate en enkel Node.js-webbprogram och distribuera den tooan Azure-molntjänst."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 50951a87-fed4-48e0-bcfa-453b9e50452e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 22945bfcc1b0e5da2a2d37dc5cc86be013cc0b5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-and-deploy-a-nodejs-application-tooan-azure-cloud-service"></a>Skapa och distribuera en Node.js-programmet tooan Azure Cloud Service

Den här kursen visar hur toocreate en enkel Node.js program som körs i en Azure-molntjänst. Molntjänster är hello byggstenarna för skalbara molnprogram i Azure. De kan hello uppdelning och oberoende sätt hantera och skala ut klient- och serverkomponenter i ditt program.  Cloud Services ger en robust avsedd virtuell dator som på ett pålitligt sätt kan vara värd åt varje roll.

Mer information om molntjänster och hur de förhåller tooAzure webbplatser och virtuella datorer finns [jämförelse mellan Azure Websites, Cloud Services och virtuella datorer].

> [!TIP]
> Letar du efter toobuild en enkel webbplats? Om du planerar att använda en enkel webbplatsklient kan du [använda en förenklad webbapp]. Du kan lätt att uppgradera tooa Molntjänsten som webbappen växer och dina behov förändras.

Följ den här kursen för att skapa en enkel webbapp som värdhanteras i en webbroll. Du kommer använda hello compute emulator tootest programmet lokalt och sedan distribuera dem med hjälp av PowerShell-kommandoradsverktyg.

hello programmet är ett enkelt ”hello world” program:

![En webbläsare som visar hello World Hello-webbsida][A web browser displaying hello Hello World web page]

## <a name="prerequisites"></a>Krav
> [!NOTE]
> I den här kursen används Azure PowerShell, vilket kräver Windows.

* Installera och konfigurera [Azure PowerShell].
* Hämta och installera hello [Azure SDK för .NET 2.7]. I hello installerar installationsprogrammet, Välj:
  * MicrosoftAzureAuthoringTools
  * MicrosoftAzureComputeEmulator

## <a name="create-an-azure-cloud-service-project"></a>Skapa ett Azure Cloud Service-projekt
Utför följande uppgifter toocreate ett nytt Azure Cloud Service-projekt, tillsammans med grundläggande scaffold-teknik för Node.js hello:

1. Kör **Windows PowerShell** som administratör; från hello **Start-menyn** eller **startskärmen**, söka efter **Windows PowerShell**.
2. [Anslut PowerShell] tooyour prenumeration.
3. Ange hello följande PowerShell-cmdlet toocreate toocreate hello projektet:

        New-AzureServiceProject helloworld

    ![hello resultatet av hello New-AzureService helloworld kommandot][hello result of hello New-AzureService helloworld command]

    Hej **New-AzureServiceProject** cmdlet skapar en grundläggande struktur för att publicera ett Node.js-programmet tooa tjänst i molnet. Den innehåller konfigurationsfiler som krävs för publicering tooAzure. hello cmdleten ändrar även directory toohello arbetskatalogen för hello-tjänsten.

    hello cmdlet skapar hello följande filer:

   * **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** och **ServiceDefinition.csdef**: Azure-specifika filer som krävs för publicering av programmet. Mer information finns i [Översikt över att skapa en värdbaserad tjänst för Azure].
   * **deploymentSettings.json**: lagrar lokala inställningar som används av hello Azure PowerShell-cmdlets för distribution.
4. Ange följande kommando tooadd en ny webbroll hello:

       Add-AzureNodeWebRole

   ![hello utdata från hello kommandot Add-AzureNodeWebRole][hello output of hello Add-AzureNodeWebRole command]

   Hej **Add-AzureNodeWebRole** cmdlet skapar ett grundläggande Node.js-program. Den modifierar även hello **.csfg** och **.csdef** filer tooadd konfigurationsposter för hello nya rollen.

   > [!NOTE]
   > Om du inte anger ett rollnamn används ett standardnamn. Du kan ange ett namn som hello första cmdlet-parameter:`Add-AzureNodeWebRole MyRole`

Hej Node.js-app är definierad i hello **server.js**, som finns i hello katalogen för webbrollen hello (**WebRole1** som standard). Här är hello kod:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Den här koden är i stort sett hello samma som hello ”Hello World” exempel på hello [nodejs.org] webbplats, förutom att det använder hello-portnummer som tilldelats av hello molnmiljö.

## <a name="deploy-hello-application-tooazure"></a>Distribuera hello programmet tooAzure

> [!NOTE]
> toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Du kan [aktivera dina MSDN-prenumerationsfördelar](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) eller [registrera dig för ett kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="download-hello-azure-publishing-settings"></a>Hämta hello Azure publicering inställningar
toodeploy tooAzure ditt program, måste du först hämta hello publicering av inställningar för din Azure-prenumeration.

1. Kör hello följande Azure PowerShell-cmdlet:

       Get-AzurePublishSettingsFile

   Det här använder webbläsaren toonavigate toohello publicera hämtningssidan för publiceringsinställningarna. Du kanske ange toolog in med ett Microsoft-Account. I så fall använder du hello-konto som är associerade med din Azure-prenumeration.

   Spara hello hämtade profilen tooa filplats du enkelt kan komma åt.
2. Kör följande cmdlet tooimport hello Publicera profil som du hämtade:

       Import-AzurePublishSettingsFile [path toofile]

    > [!NOTE]
    > När du har importerat hello publiceringsinställningarna bör du överväga att ta bort hello hämtade .publishSettings-filen eftersom den innehåller information som möjliggör någon tooaccess ditt konto.

### <a name="publish-hello-application"></a>Publicera programmet hello
Kör följande kommandon hello toopublish:

      $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

* **-ServiceName** anger hello namn hello-distributionen. Det här måste vara ett unikt namn, annars hello publicera processen misslyckas. Hej **Get-Date** lägger till en datum/tid-sträng som bör göra hello namnet unikt.
* **-Plats** anger hello datacenter som hello program ska finnas i. toosee en lista över tillgängliga datacenter, Använd hello **Get-AzureLocation** cmdlet.
* **-Launch** öppnar ett webbläsarfönster och navigerar toohello värdbaserade tjänsten när distributionen har slutförts.

När publiceringen har lyckats, visas ett svar liknande toohello följande:

![hello utdata från hello kommandot Publish-AzureService][hello output of hello Publish-AzureService command]

> [!NOTE]
> Det kan ta flera minuter innan hello programmet toodeploy och bli tillgängligt när det publiceras för första gången.

När hello distributionen är klar, ett webbläsarfönster och navigera toohello Molntjänsten.

![Ett webbläsarfönster som visar hello hello world-sidan. hello URL: en anger hello sidan värdhanteras på Azure.][A browser window displaying hello hello world page; hello URL indicates hello page is hosted on Azure.]

Programmet körs nu på Azure.

Hej **Publish-AzureServiceProject** cmdlet utför hello följande steg:

1. Skapar ett paket toodeploy. hello paketet innehåller alla hello-filer i programmappen.
2. Skapar ett nytt **lagringskonto** om det inte finns ett. hello Azure storage-konto är används toostore hello programpaketet under distributionen. När distributionen är klar kan du ta bort hello storage-konto.
3. Skapar en ny **molntjänst** om det inte redan finns en. En **Molntjänsten** är hello behållare som är värd för programmet när det är distribuerade tooAzure. Mer information finns i [Översikt över att skapa en värdbaserad tjänst för Azure].
4. Publicerar hello distribution paketet tooAzure.

## <a name="stopping-and-deleting-your-application"></a>Stoppa och ta bort programmet
När du har distribuerat programmet vill du kanske toodisable den så att du kan undvika extra kostnader. Azure fakturerar webbrollsinstanser per timme förbrukad servertid. Servertid förbrukas när programmet har distribuerats, även om hello-instanser körs inte och har statusen hello stoppades.

1. Stoppa hello tjänstdistributionen som skapades i föregående avsnitt i hello med hello följande cmdlet i Windows PowerShell-fönstret hello:

       Stop-AzureService

   Hello-tjänsten stoppas kan det ta flera minuter. När hello har stoppats, kan du få ett meddelande som anger att den har stoppats.

   ![hello status för kommandot hello Stop-AzureService][hello status of hello Stop-AzureService command]
2. toodelete hello service, anrop hello följande cmdlet:

       Remove-AzureService

   När du uppmanas, anger **Y** toodelete hello-tjänsten.

   Om du tar bort hello service kan ta några minuter. När hello-tjänsten har tagits bort visas ett meddelande som anger att hello-tjänsten har tagits bort.

   ![hello status för kommandot hello Remove-AzureService][hello status of hello Remove-AzureService command]

   > [!NOTE]
   > Tar bort hello-tjänsten inte bort hello storage-konto som skapades när hello tjänsten inledningsvis publicerades och du kommer att fortsätta toobe debiteras för lagringsutrymme som används. Om inget annat använder hello lagring, kanske du vill toodelete den.

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello [Node.js Developer Center].

<!-- URL List -->

[jämförelse mellan Azure Websites, Cloud Services och virtuella datorer]: ../app-service-web/choose-web-site-cloud-service-vm.md
[använda en förenklad webbapp]: ../app-service-web/app-service-web-get-started-nodejs.md
[Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Azure SDK för .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[Anslut PowerShell]: /powershell/azureps-cmdlets-docs#step-3-connect
[nodejs.org]: http://nodejs.org/
[Översikt över att skapa en värdbaserad tjänst för Azure]: https://azure.microsoft.com/documentation/services/cloud-services/
[Node.js Developer Center]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[hello result of hello New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[hello output of hello Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying hello Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[hello output of hello Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying hello hello world page; hello URL indicates hello page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[hello status of hello Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[hello status of hello Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
