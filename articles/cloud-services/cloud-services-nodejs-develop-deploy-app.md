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
# <a name="build-and-deploy-a-nodejs-application-tooan-azure-cloud-service"></a><span data-ttu-id="a667c-103">Skapa och distribuera en Node.js-programmet tooan Azure Cloud Service</span><span class="sxs-lookup"><span data-stu-id="a667c-103">Build and deploy a Node.js application tooan Azure Cloud Service</span></span>

<span data-ttu-id="a667c-104">Den här kursen visar hur toocreate en enkel Node.js program som körs i en Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="a667c-104">This tutorial shows how toocreate a simple Node.js application running in an Azure Cloud Service.</span></span> <span data-ttu-id="a667c-105">Molntjänster är hello byggstenarna för skalbara molnprogram i Azure.</span><span class="sxs-lookup"><span data-stu-id="a667c-105">Cloud Services are hello building blocks of scalable cloud applications in Azure.</span></span> <span data-ttu-id="a667c-106">De kan hello uppdelning och oberoende sätt hantera och skala ut klient- och serverkomponenter i ditt program.</span><span class="sxs-lookup"><span data-stu-id="a667c-106">They allow hello separation and independent management and scale-out of front-end and back-end components of your application.</span></span>  <span data-ttu-id="a667c-107">Cloud Services ger en robust avsedd virtuell dator som på ett pålitligt sätt kan vara värd åt varje roll.</span><span class="sxs-lookup"><span data-stu-id="a667c-107">Cloud Services provide a robust dedicated virtual machine for hosting each role reliably.</span></span>

<span data-ttu-id="a667c-108">Mer information om molntjänster och hur de förhåller tooAzure webbplatser och virtuella datorer finns [jämförelse mellan Azure Websites, Cloud Services och virtuella datorer].</span><span class="sxs-lookup"><span data-stu-id="a667c-108">For more information on Cloud Services, and how they compare tooAzure Websites and Virtual machines, see [Azure Websites, Cloud Services and Virtual Machines comparison].</span></span>

> [!TIP]
> <span data-ttu-id="a667c-109">Letar du efter toobuild en enkel webbplats?</span><span class="sxs-lookup"><span data-stu-id="a667c-109">Looking toobuild a simple website?</span></span> <span data-ttu-id="a667c-110">Om du planerar att använda en enkel webbplatsklient kan du [använda en förenklad webbapp].</span><span class="sxs-lookup"><span data-stu-id="a667c-110">If your scenario involves just a simple website front-end, consider [using a lightweight web app].</span></span> <span data-ttu-id="a667c-111">Du kan lätt att uppgradera tooa Molntjänsten som webbappen växer och dina behov förändras.</span><span class="sxs-lookup"><span data-stu-id="a667c-111">You can easily upgrade tooa Cloud Service as your web app grows and your requirements change.</span></span>

<span data-ttu-id="a667c-112">Följ den här kursen för att skapa en enkel webbapp som värdhanteras i en webbroll.</span><span class="sxs-lookup"><span data-stu-id="a667c-112">By following this tutorial, you will build a simple web application hosted inside a web role.</span></span> <span data-ttu-id="a667c-113">Du kommer använda hello compute emulator tootest programmet lokalt och sedan distribuera dem med hjälp av PowerShell-kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="a667c-113">You will use hello compute emulator tootest your application locally, then deploy it using PowerShell command-line tools.</span></span>

<span data-ttu-id="a667c-114">hello programmet är ett enkelt ”hello world” program:</span><span class="sxs-lookup"><span data-stu-id="a667c-114">hello application is a simple "hello world" application:</span></span>

![En webbläsare som visar hello World Hello-webbsida][A web browser displaying hello Hello World web page]

## <a name="prerequisites"></a><span data-ttu-id="a667c-116">Krav</span><span class="sxs-lookup"><span data-stu-id="a667c-116">Prerequisites</span></span>
> [!NOTE]
> <span data-ttu-id="a667c-117">I den här kursen används Azure PowerShell, vilket kräver Windows.</span><span class="sxs-lookup"><span data-stu-id="a667c-117">This tutorial uses Azure PowerShell, which requires Windows.</span></span>

* <span data-ttu-id="a667c-118">Installera och konfigurera [Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="a667c-118">Install and configure [Azure Powershell].</span></span>
* <span data-ttu-id="a667c-119">Hämta och installera hello [Azure SDK för .NET 2.7].</span><span class="sxs-lookup"><span data-stu-id="a667c-119">Download and install hello [Azure SDK for .NET 2.7].</span></span> <span data-ttu-id="a667c-120">I hello installerar installationsprogrammet, Välj:</span><span class="sxs-lookup"><span data-stu-id="a667c-120">In hello install setup, select:</span></span>
  * <span data-ttu-id="a667c-121">MicrosoftAzureAuthoringTools</span><span class="sxs-lookup"><span data-stu-id="a667c-121">MicrosoftAzureAuthoringTools</span></span>
  * <span data-ttu-id="a667c-122">MicrosoftAzureComputeEmulator</span><span class="sxs-lookup"><span data-stu-id="a667c-122">MicrosoftAzureComputeEmulator</span></span>

## <a name="create-an-azure-cloud-service-project"></a><span data-ttu-id="a667c-123">Skapa ett Azure Cloud Service-projekt</span><span class="sxs-lookup"><span data-stu-id="a667c-123">Create an Azure Cloud Service project</span></span>
<span data-ttu-id="a667c-124">Utför följande uppgifter toocreate ett nytt Azure Cloud Service-projekt, tillsammans med grundläggande scaffold-teknik för Node.js hello:</span><span class="sxs-lookup"><span data-stu-id="a667c-124">Perform hello following tasks toocreate a new Azure Cloud Service project, along with basic Node.js scaffolding:</span></span>

1. <span data-ttu-id="a667c-125">Kör **Windows PowerShell** som administratör; från hello **Start-menyn** eller **startskärmen**, söka efter **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="a667c-125">Run **Windows PowerShell** as Administrator; from hello **Start Menu** or **Start Screen**, search for **Windows PowerShell**.</span></span>
2. <span data-ttu-id="a667c-126">[Anslut PowerShell] tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a667c-126">[Connect PowerShell] tooyour subscription.</span></span>
3. <span data-ttu-id="a667c-127">Ange hello följande PowerShell-cmdlet toocreate toocreate hello projektet:</span><span class="sxs-lookup"><span data-stu-id="a667c-127">Enter hello following PowerShell cmdlet toocreate toocreate hello project:</span></span>

        New-AzureServiceProject helloworld

    ![hello resultatet av hello New-AzureService helloworld kommandot][hello result of hello New-AzureService helloworld command]

    <span data-ttu-id="a667c-129">Hej **New-AzureServiceProject** cmdlet skapar en grundläggande struktur för att publicera ett Node.js-programmet tooa tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="a667c-129">hello **New-AzureServiceProject** cmdlet generates a basic structure for publishing a Node.js application tooa Cloud Service.</span></span> <span data-ttu-id="a667c-130">Den innehåller konfigurationsfiler som krävs för publicering tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a667c-130">It contains configuration files necessary for publishing tooAzure.</span></span> <span data-ttu-id="a667c-131">hello cmdleten ändrar även directory toohello arbetskatalogen för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a667c-131">hello cmdlet also changes your working directory toohello directory for hello service.</span></span>

    <span data-ttu-id="a667c-132">hello cmdlet skapar hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="a667c-132">hello cmdlet creates hello following files:</span></span>

   * <span data-ttu-id="a667c-133">**ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** och **ServiceDefinition.csdef**: Azure-specifika filer som krävs för publicering av programmet.</span><span class="sxs-lookup"><span data-stu-id="a667c-133">**ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** and **ServiceDefinition.csdef**: Azure-specific files necessary for publishing your application.</span></span> <span data-ttu-id="a667c-134">Mer information finns i [Översikt över att skapa en värdbaserad tjänst för Azure].</span><span class="sxs-lookup"><span data-stu-id="a667c-134">For more information, see [Overview of Creating a Hosted Service for Azure].</span></span>
   * <span data-ttu-id="a667c-135">**deploymentSettings.json**: lagrar lokala inställningar som används av hello Azure PowerShell-cmdlets för distribution.</span><span class="sxs-lookup"><span data-stu-id="a667c-135">**deploymentSettings.json**: Stores local settings that are used by hello Azure PowerShell deployment cmdlets.</span></span>
4. <span data-ttu-id="a667c-136">Ange följande kommando tooadd en ny webbroll hello:</span><span class="sxs-lookup"><span data-stu-id="a667c-136">Enter hello following command tooadd a new web role:</span></span>

       Add-AzureNodeWebRole

   ![hello utdata från hello kommandot Add-AzureNodeWebRole][hello output of hello Add-AzureNodeWebRole command]

   <span data-ttu-id="a667c-138">Hej **Add-AzureNodeWebRole** cmdlet skapar ett grundläggande Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="a667c-138">hello **Add-AzureNodeWebRole** cmdlet creates a basic Node.js application.</span></span> <span data-ttu-id="a667c-139">Den modifierar även hello **.csfg** och **.csdef** filer tooadd konfigurationsposter för hello nya rollen.</span><span class="sxs-lookup"><span data-stu-id="a667c-139">It also modifies hello **.csfg** and **.csdef** files tooadd configuration entries for hello new role.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a667c-140">Om du inte anger ett rollnamn används ett standardnamn.</span><span class="sxs-lookup"><span data-stu-id="a667c-140">If you do not specify a role name, a default name is used.</span></span> <span data-ttu-id="a667c-141">Du kan ange ett namn som hello första cmdlet-parameter:`Add-AzureNodeWebRole MyRole`</span><span class="sxs-lookup"><span data-stu-id="a667c-141">You can provide a name as hello first cmdlet parameter: `Add-AzureNodeWebRole MyRole`</span></span>

<span data-ttu-id="a667c-142">Hej Node.js-app är definierad i hello **server.js**, som finns i hello katalogen för webbrollen hello (**WebRole1** som standard).</span><span class="sxs-lookup"><span data-stu-id="a667c-142">hello Node.js app is defined in hello file **server.js**, located in hello directory for hello web role (**WebRole1** by default).</span></span> <span data-ttu-id="a667c-143">Här är hello kod:</span><span class="sxs-lookup"><span data-stu-id="a667c-143">Here is hello code:</span></span>

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

<span data-ttu-id="a667c-144">Den här koden är i stort sett hello samma som hello ”Hello World” exempel på hello [nodejs.org] webbplats, förutom att det använder hello-portnummer som tilldelats av hello molnmiljö.</span><span class="sxs-lookup"><span data-stu-id="a667c-144">This code is essentially hello same as hello "Hello World" sample on hello [nodejs.org] website, except it uses hello port number assigned by hello cloud environment.</span></span>

## <a name="deploy-hello-application-tooazure"></a><span data-ttu-id="a667c-145">Distribuera hello programmet tooAzure</span><span class="sxs-lookup"><span data-stu-id="a667c-145">Deploy hello application tooAzure</span></span>

> [!NOTE]
> <span data-ttu-id="a667c-146">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a667c-146">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="a667c-147">Du kan [aktivera dina MSDN-prenumerationsfördelar](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) eller [registrera dig för ett kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).</span><span class="sxs-lookup"><span data-stu-id="a667c-147">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) or [sign up for a free account](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).</span></span>

### <a name="download-hello-azure-publishing-settings"></a><span data-ttu-id="a667c-148">Hämta hello Azure publicering inställningar</span><span class="sxs-lookup"><span data-stu-id="a667c-148">Download hello Azure publishing settings</span></span>
<span data-ttu-id="a667c-149">toodeploy tooAzure ditt program, måste du först hämta hello publicering av inställningar för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a667c-149">toodeploy your application tooAzure, you must first download hello publishing settings for your Azure subscription.</span></span>

1. <span data-ttu-id="a667c-150">Kör hello följande Azure PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="a667c-150">Run hello following Azure PowerShell cmdlet:</span></span>

       Get-AzurePublishSettingsFile

   <span data-ttu-id="a667c-151">Det här använder webbläsaren toonavigate toohello publicera hämtningssidan för publiceringsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="a667c-151">This will use your browser toonavigate toohello publish settings download page.</span></span> <span data-ttu-id="a667c-152">Du kanske ange toolog in med ett Microsoft-Account.</span><span class="sxs-lookup"><span data-stu-id="a667c-152">You may be prompted toolog in with a Microsoft Account.</span></span> <span data-ttu-id="a667c-153">I så fall använder du hello-konto som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a667c-153">If so, use hello account associated with your Azure subscription.</span></span>

   <span data-ttu-id="a667c-154">Spara hello hämtade profilen tooa filplats du enkelt kan komma åt.</span><span class="sxs-lookup"><span data-stu-id="a667c-154">Save hello downloaded profile tooa file location you can easily access.</span></span>
2. <span data-ttu-id="a667c-155">Kör följande cmdlet tooimport hello Publicera profil som du hämtade:</span><span class="sxs-lookup"><span data-stu-id="a667c-155">Run following cmdlet tooimport hello publishing profile you downloaded:</span></span>

       Import-AzurePublishSettingsFile [path toofile]

    > [!NOTE]
    > <span data-ttu-id="a667c-156">När du har importerat hello publiceringsinställningarna bör du överväga att ta bort hello hämtade .publishSettings-filen eftersom den innehåller information som möjliggör någon tooaccess ditt konto.</span><span class="sxs-lookup"><span data-stu-id="a667c-156">After importing hello publish settings, consider deleting hello downloaded .publishSettings file, because it contains information that could allow someone tooaccess your account.</span></span>

### <a name="publish-hello-application"></a><span data-ttu-id="a667c-157">Publicera programmet hello</span><span class="sxs-lookup"><span data-stu-id="a667c-157">Publish hello application</span></span>
<span data-ttu-id="a667c-158">Kör följande kommandon hello toopublish:</span><span class="sxs-lookup"><span data-stu-id="a667c-158">toopublish, run hello following commands:</span></span>

      $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

* <span data-ttu-id="a667c-159">**-ServiceName** anger hello namn hello-distributionen.</span><span class="sxs-lookup"><span data-stu-id="a667c-159">**-ServiceName** specifies hello name for hello deployment.</span></span> <span data-ttu-id="a667c-160">Det här måste vara ett unikt namn, annars hello publicera processen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="a667c-160">This must be a unique name, otherwise hello publish process will fail.</span></span> <span data-ttu-id="a667c-161">Hej **Get-Date** lägger till en datum/tid-sträng som bör göra hello namnet unikt.</span><span class="sxs-lookup"><span data-stu-id="a667c-161">hello **Get-Date** command tacks on a date/time string that should make hello name unique.</span></span>
* <span data-ttu-id="a667c-162">**-Plats** anger hello datacenter som hello program ska finnas i.</span><span class="sxs-lookup"><span data-stu-id="a667c-162">**-Location** specifies hello datacenter that hello application will be hosted in.</span></span> <span data-ttu-id="a667c-163">toosee en lista över tillgängliga datacenter, Använd hello **Get-AzureLocation** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a667c-163">toosee a list of available datacenters, use hello **Get-AzureLocation** cmdlet.</span></span>
* <span data-ttu-id="a667c-164">**-Launch** öppnar ett webbläsarfönster och navigerar toohello värdbaserade tjänsten när distributionen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a667c-164">**-Launch** opens a browser window and navigates toohello hosted service after deployment has completed.</span></span>

<span data-ttu-id="a667c-165">När publiceringen har lyckats, visas ett svar liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="a667c-165">After publishing succeeds, you will see a response similar toohello following:</span></span>

![hello utdata från hello kommandot Publish-AzureService][hello output of hello Publish-AzureService command]

> [!NOTE]
> <span data-ttu-id="a667c-167">Det kan ta flera minuter innan hello programmet toodeploy och bli tillgängligt när det publiceras för första gången.</span><span class="sxs-lookup"><span data-stu-id="a667c-167">It can take several minutes for hello application toodeploy and become available when first published.</span></span>

<span data-ttu-id="a667c-168">När hello distributionen är klar, ett webbläsarfönster och navigera toohello Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="a667c-168">Once hello deployment has completed, a browser window will open and navigate toohello cloud service.</span></span>

![Ett webbläsarfönster som visar hello hello world-sidan. hello URL: en anger hello sidan värdhanteras på Azure.][A browser window displaying hello hello world page; hello URL indicates hello page is hosted on Azure.]

<span data-ttu-id="a667c-170">Programmet körs nu på Azure.</span><span class="sxs-lookup"><span data-stu-id="a667c-170">Your application is now running on Azure.</span></span>

<span data-ttu-id="a667c-171">Hej **Publish-AzureServiceProject** cmdlet utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a667c-171">hello **Publish-AzureServiceProject** cmdlet performs hello following steps:</span></span>

1. <span data-ttu-id="a667c-172">Skapar ett paket toodeploy.</span><span class="sxs-lookup"><span data-stu-id="a667c-172">Creates a package toodeploy.</span></span> <span data-ttu-id="a667c-173">hello paketet innehåller alla hello-filer i programmappen.</span><span class="sxs-lookup"><span data-stu-id="a667c-173">hello package contains all hello files in your application folder.</span></span>
2. <span data-ttu-id="a667c-174">Skapar ett nytt **lagringskonto** om det inte finns ett.</span><span class="sxs-lookup"><span data-stu-id="a667c-174">Creates a new **storage account** if one does not exist.</span></span> <span data-ttu-id="a667c-175">hello Azure storage-konto är används toostore hello programpaketet under distributionen.</span><span class="sxs-lookup"><span data-stu-id="a667c-175">hello Azure storage account is used toostore hello application package during deployment.</span></span> <span data-ttu-id="a667c-176">När distributionen är klar kan du ta bort hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a667c-176">You can safely delete hello storage account after deployment is done.</span></span>
3. <span data-ttu-id="a667c-177">Skapar en ny **molntjänst** om det inte redan finns en.</span><span class="sxs-lookup"><span data-stu-id="a667c-177">Creates a new **cloud service** if one does not already exist.</span></span> <span data-ttu-id="a667c-178">En **Molntjänsten** är hello behållare som är värd för programmet när det är distribuerade tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a667c-178">A **cloud service** is hello container in which your application is hosted when it is deployed tooAzure.</span></span> <span data-ttu-id="a667c-179">Mer information finns i [Översikt över att skapa en värdbaserad tjänst för Azure].</span><span class="sxs-lookup"><span data-stu-id="a667c-179">For more information, see [Overview of Creating a Hosted Service for Azure].</span></span>
4. <span data-ttu-id="a667c-180">Publicerar hello distribution paketet tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a667c-180">Publishes hello deployment package tooAzure.</span></span>

## <a name="stopping-and-deleting-your-application"></a><span data-ttu-id="a667c-181">Stoppa och ta bort programmet</span><span class="sxs-lookup"><span data-stu-id="a667c-181">Stopping and deleting your application</span></span>
<span data-ttu-id="a667c-182">När du har distribuerat programmet vill du kanske toodisable den så att du kan undvika extra kostnader.</span><span class="sxs-lookup"><span data-stu-id="a667c-182">After deploying your application, you may want toodisable it so you can avoid extra costs.</span></span> <span data-ttu-id="a667c-183">Azure fakturerar webbrollsinstanser per timme förbrukad servertid.</span><span class="sxs-lookup"><span data-stu-id="a667c-183">Azure bills web role instances per hour of server time consumed.</span></span> <span data-ttu-id="a667c-184">Servertid förbrukas när programmet har distribuerats, även om hello-instanser körs inte och har statusen hello stoppades.</span><span class="sxs-lookup"><span data-stu-id="a667c-184">Server time is consumed once your application is deployed, even if hello instances are not running and are in hello stopped state.</span></span>

1. <span data-ttu-id="a667c-185">Stoppa hello tjänstdistributionen som skapades i föregående avsnitt i hello med hello följande cmdlet i Windows PowerShell-fönstret hello:</span><span class="sxs-lookup"><span data-stu-id="a667c-185">In hello Windows PowerShell window, stop hello service deployment created in hello previous section with hello following cmdlet:</span></span>

       Stop-AzureService

   <span data-ttu-id="a667c-186">Hello-tjänsten stoppas kan det ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="a667c-186">Stopping hello service may take several minutes.</span></span> <span data-ttu-id="a667c-187">När hello har stoppats, kan du få ett meddelande som anger att den har stoppats.</span><span class="sxs-lookup"><span data-stu-id="a667c-187">When hello service is stopped, you receive a message indicating that it has stopped.</span></span>

   ![hello status för kommandot hello Stop-AzureService][hello status of hello Stop-AzureService command]
2. <span data-ttu-id="a667c-189">toodelete hello service, anrop hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="a667c-189">toodelete hello service, call hello following cmdlet:</span></span>

       Remove-AzureService

   <span data-ttu-id="a667c-190">När du uppmanas, anger **Y** toodelete hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a667c-190">When prompted, enter **Y** toodelete hello service.</span></span>

   <span data-ttu-id="a667c-191">Om du tar bort hello service kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="a667c-191">Deleting hello service may take several minutes.</span></span> <span data-ttu-id="a667c-192">När hello-tjänsten har tagits bort visas ett meddelande som anger att hello-tjänsten har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="a667c-192">After hello service has been deleted you receive a message indicating that hello service was deleted.</span></span>

   ![hello status för kommandot hello Remove-AzureService][hello status of hello Remove-AzureService command]

   > [!NOTE]
   > <span data-ttu-id="a667c-194">Tar bort hello-tjänsten inte bort hello storage-konto som skapades när hello tjänsten inledningsvis publicerades och du kommer att fortsätta toobe debiteras för lagringsutrymme som används.</span><span class="sxs-lookup"><span data-stu-id="a667c-194">Deleting hello service does not delete hello storage account that was created when hello service was initially published, and you will continue toobe billed for storage used.</span></span> <span data-ttu-id="a667c-195">Om inget annat använder hello lagring, kanske du vill toodelete den.</span><span class="sxs-lookup"><span data-stu-id="a667c-195">If nothing else is using hello storage, you may want toodelete it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a667c-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a667c-196">Next steps</span></span>
<span data-ttu-id="a667c-197">Mer information finns i hello [Node.js Developer Center].</span><span class="sxs-lookup"><span data-stu-id="a667c-197">For more information, see hello [Node.js Developer Center].</span></span>

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
