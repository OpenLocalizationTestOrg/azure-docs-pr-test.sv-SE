---
title: aaaSpecifying en Node.js-Version
description: "Lär dig hur toospecify hello version av Node.js som används av Azure-webbplatser och molntjänster"
services: 
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d0e15278-2ab4-4ec8-8256-913839c6d5ef
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 09c27bfc43c132b6d66f9a2943179e06ee75bedc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-a-nodejs-version-in-an-azure-application"></a><span data-ttu-id="eb8f7-103">Ange en Node.js-version i ett Azure-program</span><span class="sxs-lookup"><span data-stu-id="eb8f7-103">Specifying a Node.js version in an Azure application</span></span>
<span data-ttu-id="eb8f7-104">När värd för ett Node.js-program, kanske du vill att ditt program använder en viss version av Node.js tooensure.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-104">When hosting a Node.js application, you may want tooensure that your application uses a specific version of Node.js.</span></span> <span data-ttu-id="eb8f7-105">Det finns flera sätt tooaccomplish detta för program som finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-105">There are several ways tooaccomplish this for applications hosted on Azure.</span></span>

## <a name="default-versions"></a><span data-ttu-id="eb8f7-106">Standardversioner</span><span class="sxs-lookup"><span data-stu-id="eb8f7-106">Default versions</span></span>
<span data-ttu-id="eb8f7-107">Hej Node.js-versioner som tillhandahålls av Azure uppdateras kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-107">hello Node.js versions provided by Azure are constantly updated.</span></span> <span data-ttu-id="eb8f7-108">Om inget annat anges hello standardversion som anges i hello `WEBSITE_NODE_DEFAULT_VERSION` miljövariabeln kommer att användas.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-108">Unless otherwise specified, hello default version that is specified in hello `WEBSITE_NODE_DEFAULT_VERSION` environment variable will be used.</span></span> <span data-ttu-id="eb8f7-109">toooverride standardvärdet, följ hello stegen i följande avsnitt i den här artikeln</span><span class="sxs-lookup"><span data-stu-id="eb8f7-109">toooverride this default value, follow hello steps in following sections of this article</span></span>

> [!NOTE]
> <span data-ttu-id="eb8f7-110">Om du är värd för ditt program i Azure Cloud Service (webb eller arbetare roll) och det är hello första gången som du har distribuerat programmet hello, Azure försöker toouse hello samma version av Node.js som du har installerat på din utvecklingsmiljö om den matchar hello standardversioner på Azure.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-110">If you are hosting your application in an Azure Cloud Service (web or worker role,) and it is hello first time you have deployed hello application, Azure will attempt toouse hello same version of Node.js as you have installed on your development environment if it matches one of hello default versions available on Azure.</span></span>
>
>

## <a name="versioning-with-packagejson"></a><span data-ttu-id="eb8f7-111">Versionshantering med package.json</span><span class="sxs-lookup"><span data-stu-id="eb8f7-111">Versioning with package.json</span></span>
<span data-ttu-id="eb8f7-112">Du kan ange hello version av Node.js toobe används genom att lägga till följande tooyour hello **package.json** fil:</span><span class="sxs-lookup"><span data-stu-id="eb8f7-112">You can specify hello version of Node.js toobe used by adding hello following tooyour **package.json** file:</span></span>

    "engines":{"node":version}

<span data-ttu-id="eb8f7-113">Där *version* är hello specifik version nummer toouse.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-113">Where *version* is hello specific version number toouse.</span></span> <span data-ttu-id="eb8f7-114">Du kan ange mer komplexa villkor för versionen, exempelvis:</span><span class="sxs-lookup"><span data-stu-id="eb8f7-114">You can specify more complex conditions for version, such as:</span></span>

    "engines":{"node": "0.6.22 || 0.8.x"}

<span data-ttu-id="eb8f7-115">Eftersom 0.6.22 inte är någon av hello-versioner som är tillgängliga i hello värdmiljön användas hello högsta version av hello 0,8 serie som är tillgängliga kommer att istället - 0.8.4.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-115">Since 0.6.22 is not one of hello versions available in hello hosting environment, hello highest version of hello 0.8 series that is available will be used instead - 0.8.4.</span></span>

## <a name="versioning-websites-with-app-settings"></a><span data-ttu-id="eb8f7-116">Versionshantering webbplatser med App-inställningar</span><span class="sxs-lookup"><span data-stu-id="eb8f7-116">Versioning Websites with App Settings</span></span>
<span data-ttu-id="eb8f7-117">Om du är värd för hello program på en webbplats, kan du ange miljövariabeln hello **WEBSITE_NODE_DEFAULT_VERSION** toohello önskade versionen.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-117">If you are hosting hello application in a Website, you can set hello environment variable **WEBSITE_NODE_DEFAULT_VERSION** toohello desired version.</span></span>

## <a name="versioning-cloud-services-with-powershell"></a><span data-ttu-id="eb8f7-118">Versionshantering molntjänster med PowerShell</span><span class="sxs-lookup"><span data-stu-id="eb8f7-118">Versioning Cloud Services with PowerShell</span></span>
<span data-ttu-id="eb8f7-119">Om du är värd för programmet hello i en molntjänst och distribuerar hello program med hjälp av Azure PowerShell, kan du åsidosätta hello Node.js-standardversion med hjälp av hello **Set AzureServiceProjectRole** PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-119">If you are hosting hello application in a Cloud Service, and are deploying hello application using Azure PowerShell, you can override hello default Node.js version by using hello **Set-AzureServiceProjectRole** PowerShell cmdlet.</span></span> <span data-ttu-id="eb8f7-120">Exempel:</span><span class="sxs-lookup"><span data-stu-id="eb8f7-120">For example:</span></span>

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

<span data-ttu-id="eb8f7-121">Obs hello parametrar i hello ovan instruktionen är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-121">Note hello parameters in hello above statement are case-sensitive.</span></span>  <span data-ttu-id="eb8f7-122">Du kan verifiera hello rätt version av Node.js har valts genom att kontrollera hello **motorer** egenskap i din roll **package.json**.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-122">You can verify hello correct version of Node.js has been selected by checking hello **engines** property in your role's **package.json**.</span></span>

<span data-ttu-id="eb8f7-123">Du kan också använda hello **Get-AzureServiceProjectRoleRuntime** tooretrieve en lista över Node.js-versioner som är tillgängliga för program som finns som en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-123">You can also use hello **Get-AzureServiceProjectRoleRuntime** tooretrieve a list of Node.js versions available for applications hosted as a Cloud Service.</span></span>  <span data-ttu-id="eb8f7-124">Kontrollera alltid hello-versionen av Node.js projektet beroende är i den här listan.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-124">Always verify hello version of Node.js your project depends on is in this list.</span></span>

## <a name="using-a-custom-version-with-azure-websites"></a><span data-ttu-id="eb8f7-125">Använda en anpassad version med Azure Websites</span><span class="sxs-lookup"><span data-stu-id="eb8f7-125">Using a custom version with Azure Websites</span></span>
<span data-ttu-id="eb8f7-126">Medan Azure innehåller flera standardversioner av Node.js, kan du kanske vill toouse en version som inte har angetts som standard.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-126">While Azure provides several default versions of Node.js, you may want toouse a version that is not provided by default.</span></span> <span data-ttu-id="eb8f7-127">Om du är värd för programmet som en Azure-webbplats, du kan göra detta med hjälp av hello **iisnode.yml** fil.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-127">If your application is hosted as an Azure Website, you can accomplish this by using hello **iisnode.yml** file.</span></span> <span data-ttu-id="eb8f7-128">hello igenom följande steg hello processen med att använda en anpassad version av Node.Js med en Azure-webbplats:</span><span class="sxs-lookup"><span data-stu-id="eb8f7-128">hello following steps walk through hello process of using a custom version of Node.Js with an Azure Website:</span></span>

1. <span data-ttu-id="eb8f7-129">Skapa en ny katalog och skapa sedan en **server.js** filen i hello katalog.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-129">Create a new directory, and then create a **server.js** file within hello directory.</span></span> <span data-ttu-id="eb8f7-130">Hej **server.js** -filen ska innehålla hello följande:</span><span class="sxs-lookup"><span data-stu-id="eb8f7-130">hello **server.js** file should contain hello following:</span></span>

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    <span data-ttu-id="eb8f7-131">Hej Node.js-version som används när du bläddrar hello webbplatsen visas.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-131">This will display hello Node.js version being used when you browse hello website.</span></span>
2. <span data-ttu-id="eb8f7-132">Skapa en ny webbplats och Observera hello namnet på hello-platsen.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-132">Create a new Website and note hello name of hello site.</span></span> <span data-ttu-id="eb8f7-133">Till exempel hello följande använder hello [Azure kommandoradsverktyg] toocreate en ny Azure-webbplats med namnet **minwebbplats**, och sedan aktivera en Git-lagringsplats för hello webbplats.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-133">For example, hello following uses hello [Azure Command-line tools] toocreate a new Azure Website named **mywebsite**, and then enable a Git repository for hello website.</span></span>

        azure site create mywebsite --git
3. <span data-ttu-id="eb8f7-134">Skapa en ny katalog med namnet **bin** som underordnad till hello katalog som innehåller hello **server.js** fil.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-134">Create a new directory named **bin** as a child of hello directory containing hello **server.js** file.</span></span>
4. <span data-ttu-id="eb8f7-135">Hämta hello specifik version av **node.exe** (version för Windows hello) som du vill toouse med ditt program.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-135">Download hello specific version of **node.exe** (hello Windows version) that you wish toouse with your application.</span></span> <span data-ttu-id="eb8f7-136">Till exempel hello följande använder **curl** toodownload version 0.8.1:</span><span class="sxs-lookup"><span data-stu-id="eb8f7-136">For example, hello following uses **curl** toodownload version 0.8.1:</span></span>

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    <span data-ttu-id="eb8f7-137">Spara hello **node.exe** filen till hello **bin** mapp som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-137">Save hello **node.exe** file into hello **bin** folder created previously.</span></span>
5. <span data-ttu-id="eb8f7-138">Skapa en **iisnode.yml** filen i hello samma katalog som hello **server.js** filen och Lägg sedan till följande innehåll toohello hello **iisnode.yml** fil:</span><span class="sxs-lookup"><span data-stu-id="eb8f7-138">Create an **iisnode.yml** file in hello same directory as hello **server.js** file, and then add hello following content toohello **iisnode.yml** file:</span></span>

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    <span data-ttu-id="eb8f7-139">Den här sökvägen är där hello **node.exe** fil i projektet kommer att finnas när du har publicerat din programmet toohello Azure-webbplats.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-139">This path is where hello **node.exe** file within your project will be located once you have published your application toohello Azure Website.</span></span>
6. <span data-ttu-id="eb8f7-140">Publicera programmet.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-140">Publish your application.</span></span> <span data-ttu-id="eb8f7-141">Till exempel sedan jag tidigare skapade en ny webbplats med hello--git-parametern hello följande kommandon ska lägga till hello programmet filer toomy lokal Git-lagringsplats sedan push-installera dem toohello webbplats databasen:</span><span class="sxs-lookup"><span data-stu-id="eb8f7-141">For example, since I created a new website with hello --git parameter earlier, hello following commands will add hello application files toomy local Git repository, and then push them toohello website repository:</span></span>

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    <span data-ttu-id="eb8f7-142">Öppna hello webbplatsen i en webbläsare när hello programmet har publicerats.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-142">After hello application has published, open hello website in a browser.</span></span> <span data-ttu-id="eb8f7-143">Du bör se ett meddelande om ”Hello från Azure körs nod-version: v0.8.1”.</span><span class="sxs-lookup"><span data-stu-id="eb8f7-143">You should see a message stating "Hello from Azure running node version: v0.8.1".</span></span>

## <a name="next-steps"></a><span data-ttu-id="eb8f7-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eb8f7-144">Next Steps</span></span>
<span data-ttu-id="eb8f7-145">Nu när du förstår hur toospecify hello version av Node.js används av ditt program, lär du dig hur för[fungerar med moduler], [skapa och distribuera en Node.js-webbplats](app-service-web/app-service-web-get-started-nodejs.md), och [hur toouse hello Azure Kommandoradsverktyg för Mac- och Linux].</span><span class="sxs-lookup"><span data-stu-id="eb8f7-145">Now that you understand how toospecify hello version of Node.js used by your application, learn how too[work with modules], [build and deploy a Node.js Web Site](app-service-web/app-service-web-get-started-nodejs.md), and [How toouse hello Azure Command-Line Tools for Mac and Linux].</span></span>

<span data-ttu-id="eb8f7-146">Mer information finns i hello [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="eb8f7-146">For more information, see hello [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/).</span></span>

[hur toouse hello Azure Kommandoradsverktyg för Mac- och Linux]:cli-install-nodejs.md
[Azure kommandoradsverktyg]:cli-install-nodejs.md
[fungerar med moduler]: nodejs-use-node-modules-azure-apps.md
[build and deploy a Node.js Web Site]: app-service-web/app-service-web-get-started-nodejs.md
