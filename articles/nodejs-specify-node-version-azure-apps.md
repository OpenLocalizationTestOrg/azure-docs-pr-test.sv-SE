---
title: Ange en Node.js-Version
description: "Lär dig att ange version för Node.js som används av Azure-webbplatser och molntjänster"
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
ms.openlocfilehash: 89627e6a877c9f65a5216c55f58f1c707ea25d44
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="specifying-a-nodejs-version-in-an-azure-application"></a><span data-ttu-id="bfd8d-103">Ange en Node.js-version i ett Azure-program</span><span class="sxs-lookup"><span data-stu-id="bfd8d-103">Specifying a Node.js version in an Azure application</span></span>
<span data-ttu-id="bfd8d-104">När värd för ett Node.js-program, kanske du vill kontrollera att ditt program använder en viss version av Node.js.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-104">When hosting a Node.js application, you may want to ensure that your application uses a specific version of Node.js.</span></span> <span data-ttu-id="bfd8d-105">Det finns flera sätt att göra detta för program som finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-105">There are several ways to accomplish this for applications hosted on Azure.</span></span>

## <a name="default-versions"></a><span data-ttu-id="bfd8d-106">Standardversioner</span><span class="sxs-lookup"><span data-stu-id="bfd8d-106">Default versions</span></span>
<span data-ttu-id="bfd8d-107">Node.js-versioner som tillhandahålls av Azure uppdateras kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-107">The Node.js versions provided by Azure are constantly updated.</span></span> <span data-ttu-id="bfd8d-108">Om inget annat anges standardversionen som anges i den `WEBSITE_NODE_DEFAULT_VERSION` miljövariabeln kommer att användas.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-108">Unless otherwise specified, the default version that is specified in the `WEBSITE_NODE_DEFAULT_VERSION` environment variable will be used.</span></span> <span data-ttu-id="bfd8d-109">Följ stegen i följande avsnitt i den här artikeln om du vill åsidosätta det här värdet</span><span class="sxs-lookup"><span data-stu-id="bfd8d-109">To override this default value, follow the steps in following sections of this article</span></span>

> [!NOTE]
> <span data-ttu-id="bfd8d-110">Om du är värd för ditt program i Azure Cloud Service (webb eller arbetare roll), och det är första gången du har distribuerat programmet, Azure kommer att försöka använda samma version av Node.js som du har installerat på din utvecklingsmiljö om det matchar ett t han standardversioner på Azure.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-110">If you are hosting your application in an Azure Cloud Service (web or worker role,) and it is the first time you have deployed the application, Azure will attempt to use the same version of Node.js as you have installed on your development environment if it matches one of the default versions available on Azure.</span></span>
>
>

## <a name="versioning-with-packagejson"></a><span data-ttu-id="bfd8d-111">Versionshantering med package.json</span><span class="sxs-lookup"><span data-stu-id="bfd8d-111">Versioning with package.json</span></span>
<span data-ttu-id="bfd8d-112">Du kan ange vilken version av Node.js som ska användas genom att lägga till följande för att din **package.json** fil:</span><span class="sxs-lookup"><span data-stu-id="bfd8d-112">You can specify the version of Node.js to be used by adding the following to your **package.json** file:</span></span>

    "engines":{"node":version}

<span data-ttu-id="bfd8d-113">Där *version* är att använda specifika versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-113">Where *version* is the specific version number to use.</span></span> <span data-ttu-id="bfd8d-114">Du kan ange mer komplexa villkor för versionen, exempelvis:</span><span class="sxs-lookup"><span data-stu-id="bfd8d-114">You can specify more complex conditions for version, such as:</span></span>

    "engines":{"node": "0.6.22 || 0.8.x"}

<span data-ttu-id="bfd8d-115">Eftersom 0.6.22 inte är en av versionerna som är tillgängliga i värdmiljön, används den senaste versionen av 0,8 serie som är tillgängliga kommer att i stället - 0.8.4.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-115">Since 0.6.22 is not one of the versions available in the hosting environment, the highest version of the 0.8 series that is available will be used instead - 0.8.4.</span></span>

## <a name="versioning-websites-with-app-settings"></a><span data-ttu-id="bfd8d-116">Versionshantering webbplatser med App-inställningar</span><span class="sxs-lookup"><span data-stu-id="bfd8d-116">Versioning Websites with App Settings</span></span>
<span data-ttu-id="bfd8d-117">Om du är värd för programmet på en webbplats, kan du ange miljövariabeln **WEBSITE_NODE_DEFAULT_VERSION** till den önskade versionen.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-117">If you are hosting the application in a Website, you can set the environment variable **WEBSITE_NODE_DEFAULT_VERSION** to the desired version.</span></span>

## <a name="versioning-cloud-services-with-powershell"></a><span data-ttu-id="bfd8d-118">Versionshantering molntjänster med PowerShell</span><span class="sxs-lookup"><span data-stu-id="bfd8d-118">Versioning Cloud Services with PowerShell</span></span>
<span data-ttu-id="bfd8d-119">Om du är värd för programmet i en molntjänst och distribuerar program med hjälp av Azure PowerShell, kan du åsidosätta Node.js-standardversion med hjälp av den **Set AzureServiceProjectRole** PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-119">If you are hosting the application in a Cloud Service, and are deploying the application using Azure PowerShell, you can override the default Node.js version by using the **Set-AzureServiceProjectRole** PowerShell cmdlet.</span></span> <span data-ttu-id="bfd8d-120">Exempel:</span><span class="sxs-lookup"><span data-stu-id="bfd8d-120">For example:</span></span>

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

<span data-ttu-id="bfd8d-121">Observera att parametrarna i ovanstående är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-121">Note the parameters in the above statement are case-sensitive.</span></span>  <span data-ttu-id="bfd8d-122">Du kan kontrollera att rätt version av Node.js har markerats som kontrollerar den **motorer** egenskap i din roll **package.json**.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-122">You can verify the correct version of Node.js has been selected by checking the **engines** property in your role's **package.json**.</span></span>

<span data-ttu-id="bfd8d-123">Du kan också använda den **Get-AzureServiceProjectRoleRuntime** att hämta en lista över Node.js-versioner som är tillgängliga för program som finns som en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-123">You can also use the **Get-AzureServiceProjectRoleRuntime** to retrieve a list of Node.js versions available for applications hosted as a Cloud Service.</span></span>  <span data-ttu-id="bfd8d-124">Kontrollera alltid versionen av Node.js projektet beroende är i den här listan.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-124">Always verify the version of Node.js your project depends on is in this list.</span></span>

## <a name="using-a-custom-version-with-azure-websites"></a><span data-ttu-id="bfd8d-125">Använda en anpassad version med Azure Websites</span><span class="sxs-lookup"><span data-stu-id="bfd8d-125">Using a custom version with Azure Websites</span></span>
<span data-ttu-id="bfd8d-126">Medan Azure innehåller flera standardversioner av Node.js, kanske du vill använda en version som inte har angetts som standard.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-126">While Azure provides several default versions of Node.js, you may want to use a version that is not provided by default.</span></span> <span data-ttu-id="bfd8d-127">Om du är värd för programmet som en Azure-webbplats, du kan göra detta med hjälp av den **iisnode.yml** fil.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-127">If your application is hosted as an Azure Website, you can accomplish this by using the **iisnode.yml** file.</span></span> <span data-ttu-id="bfd8d-128">Följande steg igenom processen med att använda en anpassad version av Node.Js med en Azure-webbplats:</span><span class="sxs-lookup"><span data-stu-id="bfd8d-128">The following steps walk through the process of using a custom version of Node.Js with an Azure Website:</span></span>

1. <span data-ttu-id="bfd8d-129">Skapa en ny katalog och skapa sedan en **server.js** fil i katalogen.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-129">Create a new directory, and then create a **server.js** file within the directory.</span></span> <span data-ttu-id="bfd8d-130">Den **server.js** -filen ska innehålla följande:</span><span class="sxs-lookup"><span data-stu-id="bfd8d-130">The **server.js** file should contain the following:</span></span>

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    <span data-ttu-id="bfd8d-131">Node.js-version som används när du bläddrar till webbplatsen visas.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-131">This will display the Node.js version being used when you browse the website.</span></span>
2. <span data-ttu-id="bfd8d-132">Skapa en ny webbplats och notera namnet på platsen.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-132">Create a new Website and note the name of the site.</span></span> <span data-ttu-id="bfd8d-133">Till exempel på följande sätt i [Azure kommandoradsverktyg] att skapa en ny Azure-webbplats med namnet **minwebbplats**, och sedan aktivera en Git-lagringsplats för webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-133">For example, the following uses the [Azure Command-line tools] to create a new Azure Website named **mywebsite**, and then enable a Git repository for the website.</span></span>

        azure site create mywebsite --git
3. <span data-ttu-id="bfd8d-134">Skapa en ny katalog med namnet **bin** som underordnad till den katalog som innehåller den **server.js** fil.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-134">Create a new directory named **bin** as a child of the directory containing the **server.js** file.</span></span>
4. <span data-ttu-id="bfd8d-135">Hämta versionen av **node.exe** (Windows-version) som du vill använda med ditt program.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-135">Download the specific version of **node.exe** (the Windows version) that you wish to use with your application.</span></span> <span data-ttu-id="bfd8d-136">Till exempel på följande sätt **curl** att hämta versionen 0.8.1:</span><span class="sxs-lookup"><span data-stu-id="bfd8d-136">For example, the following uses **curl** to download version 0.8.1:</span></span>

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    <span data-ttu-id="bfd8d-137">Spara den **node.exe** filen till den **bin** mapp som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-137">Save the **node.exe** file into the **bin** folder created previously.</span></span>
5. <span data-ttu-id="bfd8d-138">Skapa en **iisnode.yml** fil i samma katalog som den **server.js** filen och Lägg sedan till följande innehåll till den **iisnode.yml** fil:</span><span class="sxs-lookup"><span data-stu-id="bfd8d-138">Create an **iisnode.yml** file in the same directory as the **server.js** file, and then add the following content to the **iisnode.yml** file:</span></span>

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    <span data-ttu-id="bfd8d-139">Standardsökvägen är den **node.exe** fil i projektet kommer att finnas när du har publicerat programmet till Azure-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-139">This path is where the **node.exe** file within your project will be located once you have published your application to the Azure Website.</span></span>
6. <span data-ttu-id="bfd8d-140">Publicera programmet.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-140">Publish your application.</span></span> <span data-ttu-id="bfd8d-141">Till exempel sedan jag tidigare skapade en ny webbplats med parametern – git kommer följande kommandon Lägg till programmets filer till min lokala Git-lagringsplats och skickar dem sedan till lagringsplatsen webbplats:</span><span class="sxs-lookup"><span data-stu-id="bfd8d-141">For example, since I created a new website with the --git parameter earlier, the following commands will add the application files to my local Git repository, and then push them to the website repository:</span></span>

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    <span data-ttu-id="bfd8d-142">Öppna webbplatsen i en webbläsare när programmet har publicerats.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-142">After the application has published, open the website in a browser.</span></span> <span data-ttu-id="bfd8d-143">Du bör se ett meddelande om ”Hello från Azure körs nod-version: v0.8.1”.</span><span class="sxs-lookup"><span data-stu-id="bfd8d-143">You should see a message stating "Hello from Azure running node version: v0.8.1".</span></span>

## <a name="next-steps"></a><span data-ttu-id="bfd8d-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bfd8d-144">Next Steps</span></span>
<span data-ttu-id="bfd8d-145">Nu när du förstår hur ange version för Node.js som används av ditt program, Läs mer om hur du [fungerar med moduler], [skapa och distribuera en Node.js-webbplats](app-service-web/app-service-web-get-started-nodejs.md), och [hur du använder Azure Kommandoradsverktyg för Mac- och Linux].</span><span class="sxs-lookup"><span data-stu-id="bfd8d-145">Now that you understand how to specify the version of Node.js used by your application, learn how to [work with modules], [build and deploy a Node.js Web Site](app-service-web/app-service-web-get-started-nodejs.md), and [How to use the Azure Command-Line Tools for Mac and Linux].</span></span>

<span data-ttu-id="bfd8d-146">Mer information finns i [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="bfd8d-146">For more information, see the [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/).</span></span>

<span data-ttu-id="bfd8d-147">[hur du använder Azure Kommandoradsverktyg för Mac- och Linux]:cli-install-nodejs.md</span><span class="sxs-lookup"><span data-stu-id="bfd8d-147">[How to use the Azure Command-Line Tools for Mac and Linux]:cli-install-nodejs.md</span></span>
<span data-ttu-id="bfd8d-148">[Azure kommandoradsverktyg]:cli-install-nodejs.md</span><span class="sxs-lookup"><span data-stu-id="bfd8d-148">[Azure Command-line tools]:cli-install-nodejs.md</span></span>
<span data-ttu-id="bfd8d-149">[fungerar med moduler]: nodejs-use-node-modules-azure-apps.md</span><span class="sxs-lookup"><span data-stu-id="bfd8d-149">[work with modules]: nodejs-use-node-modules-azure-apps.md</span></span>
[build and deploy a Node.js Web Site]: app-service-web/app-service-web-get-started-nodejs.md
