---
title: "Kom igång med Node.js | Microsoft Docs"
description: "Lär dig att skapa en enkel Node.js-webbapp och distribuera den till en Azure-molntjänst."
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
ms.openlocfilehash: b1e08e79c7fe2acbdb9c17607641612ffa2934ee
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="build-and-deploy-a-nodejs-application-to-an-azure-cloud-service"></a>Skapa och distribuera en Node.js-app till en Azure-molntjänst

I den här kursen får du lära dig hur du skapar en enkel Node.js-app som körs i en Azure-molntjänst. Cloud Services är byggblocken för skalbara molnprogram i Azure. De gör att det går att separera och på ett oberoende sätt hantera och skala ut klient- och serverkomponenter i ditt program.  Cloud Services ger en robust avsedd virtuell dator som på ett pålitligt sätt kan vara värd åt varje roll.

Mer information om Cloud Services, och vilka skillnader de har jämfört med Azure Websites and Virtual Machines finns i [Jämförelse mellan Azure Websites, Cloud Services och Virtual Machines].

> [!TIP]
> Vill du skapa en enkel webbplats? Om du planerar att använda en enkel webbplatsklient kan du [använda en förenklad webbapp]. Det är lätt att uppgradera till en molntjänst efter hand som webbappen växer och dina behov förändras.

Följ den här kursen för att skapa en enkel webbapp som värdhanteras i en webbroll. Under kursen kommer du att använda beräkningsemulatorn för att testa ditt program lokalt, och sedan distribuera det med PowerShell-kommandoradsverktyg.

Programmet är ett enkelt ”hello world”-program:

![En webbläsare som visar webbsidan Hello World][A web browser displaying the Hello World web page]

## <a name="prerequisites"></a>Krav
> [!NOTE]
> I den här kursen används Azure PowerShell, vilket kräver Windows.

* Installera och konfigurera [Azure PowerShell].
* Ladda ned och installera [Azure SDK för .NET 2.7]. Välj följande vid installationen:
  * MicrosoftAzureAuthoringTools
  * MicrosoftAzureComputeEmulator

## <a name="create-an-azure-cloud-service-project"></a>Skapa ett Azure Cloud Service-projekt
Utför följande uppgifter för att skapa ett nytt Azure Cloud Service-projekt med grundläggande scaffold-teknik för Node.js:

1. Kör **Windows PowerShell** med administratörsbehörighet. Öppna **Start-menyn** eller **Startskärmen** och sök efter **Windows PowerShell**.
2. [Anslut PowerShell] till din prenumeration.
3. Ange följande PowerShell-cmdlet för att skapa projektet:

        New-AzureServiceProject helloworld

    ![Resultatet av kommandot New-AzureService helloworld][The result of the New-AzureService helloworld command]

    Cmdleten **New-AzureServiceProject** skapar en grundläggande struktur för att publicera ett Node.js-program till en molntjänst. Den innehåller konfigurationsfiler som krävs för publicering till Azure. Cmdleten ändrar även arbetskatalogen till katalogen för tjänsten.

    Cmdleten skapar följande filer:

   * **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** och **ServiceDefinition.csdef**: Azure-specifika filer som krävs för publicering av programmet. Mer information finns i [Översikt över att skapa en värdbaserad tjänst för Azure].
   * **deploymentSettings.json**: Lagrar lokala inställningar som används av Azure PowerShell-cmdletarna för distribution.
4. Ange följande kommando för att lägga till en ny webbroll:

       Add-AzureNodeWebRole

   ![Resultatet av kommandot Add-AzureNodeWebRole][The output of the Add-AzureNodeWebRole command]

   Cmdleten **Add-AzureNodeWebRole** skapar ett grundläggande Node.js-program. Den modifierar även **.csfg**- och **.csdef**-filerna att lägga till konfigurationsposter för den nya rollen.

   > [!NOTE]
   > Om du inte anger ett rollnamn används ett standardnamn. Du kan ange ett namn som den första cmdlet-parametern: `Add-AzureNodeWebRole MyRole`

Node.js-appen definieras i filen **server.js**, som finns i katalogen för webbrollen (**WebRole1** som standard). Här är koden:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Den här koden är i stort sett densamma som i exemplet ”Hello World” på webbplatsen [nodejs.org], förutom att det använder portnumret som har tilldelats av molnmiljön.

## <a name="deploy-the-application-to-azure"></a>Distribuera programmet till Azure

> [!NOTE]
> Du behöver ett Azure-konto för att slutföra den här självstudien. Du kan [aktivera dina MSDN-prenumerationsfördelar](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) eller [registrera dig för ett kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="download-the-azure-publishing-settings"></a>Hämta Azure-publiceringsinställningarna
Om du vill distribuera programmet till Azure måste du först hämta publiceringsinställningarna för din Azure-prenumeration.

1. Kör följande Azure PowerShell-cmdlet:

       Get-AzurePublishSettingsFile

   Den använder webbläsaren för att navigera till hämtningssidan för publiceringsinställningarna. Du uppmanas eventuellt att logga in med ett Microsoft-konto. I så fall använder du det konto som är kopplat till din Azure-prenumeration.

   Spara den hämtade profilen på en plats som du enkelt kan komma åt.
2. Kör följande cmdlet för att importera publiceringsprofilen som du hämtade:

       Import-AzurePublishSettingsFile [path to file]

    > [!NOTE]
    > När du har importerat publiceringsinställningarna bör du ta bort den hämtade .publishSettings-filen eftersom den innehåller information som kan göra det möjligt för någon att komma åt ditt konto.

### <a name="publish-the-application"></a>Publicera programmet
Kör följande kommandon för att publicera:

      $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

* **-ServiceName** anger namnet på distributionen. Det här måste vara ett unikt namn, annars misslyckas publiceringsprocessen. Kommandot **Get-Date** lägger till en datum/tid-sträng som bör göra namnet unikt.
* **-Location** anger datacentret där programmet ska värdhanteras. Om du vill se en lista över tillgängliga datacenter ska du använda cmdleten **Get-AzureLocation**.
* **-Launch** öppnar ett webbläsarfönster och navigerar till den värdbaserade tjänsten när distributionen har slutförts.

När publiceringen har lyckats ser du ett svar som liknar följande:

![Resultatet av kommandot Publish-AzureService][The output of the Publish-AzureService command]

> [!NOTE]
> Det kan ta flera minuter för programmet att distribueras och bli tillgängligt när det publiceras för första gången.

När distributionen är klar öppnas ett webbläsarfönster som navigerar till molntjänsten.

![Ett webbläsarfönster som visar Hello World-sidan, URL:en anger att sidan värdhanteras på Azure.][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

Programmet körs nu på Azure.

Cmdleten **Publish-AzureServiceProject** utför följande steg:

1. Skapar ett paket som ska distribueras. Paketet innehåller alla filer i programmappen.
2. Skapar ett nytt **lagringskonto** om det inte finns ett. Azure-lagringskontot används för att lagra programpaketet under distributionen. När distributionen är klar kan du ta bort lagringskontot.
3. Skapar en ny **molntjänst** om det inte redan finns en. En **molntjänst** är behållaren som är värd för programmet när det distribueras till Azure. Mer information finns i [Översikt över att skapa en värdbaserad tjänst för Azure].
4. Publicerar distributionspaketet till Azure.

## <a name="stopping-and-deleting-your-application"></a>Stoppa och ta bort programmet
När du har distribuerat programmet vill du kanske inaktivera det för att undvika extra kostnader. Azure fakturerar webbrollsinstanser per timme förbrukad servertid. Servertid förbrukas när programmet har distribuerats, även om instanserna inte körs och är i stoppat tillstånd.

1. Stoppa tjänstdistributionen som skapades i föregående avsnitt med följande cmdlet i Windows PowerShell-fönstret:

       Stop-AzureService

   Det kan ta flera minuter att stoppa tjänsten. När tjänsten har stoppats får du ett meddelande som anger att den har stoppats.

   ![Status för kommandot Stop-AzureService][The status of the Stop-AzureService command]
2. Ta bort tjänsten genom att anropa följande cmdlet:

       Remove-AzureService

   När du uppmanas, anger du **Y** för att ta bort tjänsten.

   Det kan ta flera minuter att ta bort tjänsten. När tjänsten har tagits bort får du ett meddelande som anger att tjänsten har tagits bort.

   ![Status för kommandot Remove-AzureService][The status of the Remove-AzureService command]

   > [!NOTE]
   > Om du tar bort tjänsten tar det inte bort lagringskontot som skapades när tjänsten inledningsvis publicerades, och du debiteras även i fortsättningen för det lagringsutrymme som används. Om lagringen inte används kan det vara lämpligt att ta bort den.

## <a name="next-steps"></a>Nästa steg
Mer information finns i [Node.js Developer Center].

<!-- URL List -->

[Jämförelse mellan Azure Websites, Cloud Services och Virtual Machines]: ../app-service/choose-web-site-cloud-service-vm.md
[använda en förenklad webbapp]: ../app-service/app-service-web-get-started-nodejs.md
[Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Azure SDK för .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[Anslut PowerShell]: /powershell/azureps-cmdlets-docs#step-3-connect
[nodejs.org]: http://nodejs.org/
[Översikt över att skapa en värdbaserad tjänst för Azure]: https://azure.microsoft.com/documentation/services/cloud-services/
[Node.js Developer Center]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
