---
title: "aaaUsing Windows PowerShell-skript tooPublish tooDev och testmiljöer | Microsoft Docs"
description: "Lär dig hur toouse Windows PowerShell-skript från Visual Studio toopublish toodevelopment- och testmiljöer."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5fff1301-5469-4d97-be88-c85c30f837c1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 491a058f96255576afa74f6156f20ae9559bb9f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-windows-powershell-scripts-toopublish-toodev-and-test-environments"></a><span data-ttu-id="1cb2d-103">Med Windows PowerShell-skript toopublish toodev- och testmiljöer</span><span class="sxs-lookup"><span data-stu-id="1cb2d-103">Using Windows PowerShell scripts toopublish toodev and test environments</span></span>
<span data-ttu-id="1cb2d-104">När du skapar ett webbprogram i Visual Studio kan generera du en Windows PowerShell-skript som du kan använda senare tooautomate hello publicering av din webbplats tooAzure som en Webbapp i Azure App Service eller en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-104">When you create a web application in Visual Studio, you can generate a Windows PowerShell script that you can use later tooautomate hello publishing of your website tooAzure as a Web App in Azure App Service or a virtual machine.</span></span> <span data-ttu-id="1cb2d-105">Du kan redigera och utöka hello Windows PowerShell-skript i hello Visual Studio-redigeraren toosuit dina krav eller integrera hello skript med befintliga bygga, testa och publishing skript.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-105">You can edit and extend hello Windows PowerShell script in hello Visual Studio editor toosuit your requirements, or integrate hello script with existing build, test, and publishing scripts.</span></span>

<span data-ttu-id="1cb2d-106">Med dessa skript kan etablera du anpassade versioner (även kallat utvecklings- och testmiljöer) för din plats för temporär användning.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-106">Using these scripts, you can provision customized versions (also known as dev and test environments) of your site for temporary use.</span></span> <span data-ttu-id="1cb2d-107">Du kan till exempel konfigurera en viss version av din webbplats på en virtuell Azure-dator eller på hello mellanlagringsplatsen på en webbplats toorun ett test-paket, återskapa ett programfel, och testa en felkorrigering, utvärderingsversion föreslagna ändringen eller konfigurera en anpassad miljö för demonstration eller presentation.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-107">For example, you might set up a particular version of your website on an Azure virtual machine or on hello staging slot on a website toorun a test suite, reproduce a bug, test a bug fix, trial a proposed change, or set up a custom environment for a demo or presentation.</span></span> <span data-ttu-id="1cb2d-108">När du har skapat ett skript som publicerar ditt projekt kan du återskapa identiska miljöer genom att köra hello skript efter behov eller kör hello skript med din egen version av din web application toocreate en anpassad miljö för att testa.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-108">After you've created a script that publishes your project, you can recreate identical environments by re-running hello script as needed, or run hello script with your own build of your web application toocreate a custom environment for testing.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="1cb2d-109">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="1cb2d-109">What you need</span></span>
* <span data-ttu-id="1cb2d-110">Azure SDK 2.3 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-110">Azure SDK 2.3 or later.</span></span> <span data-ttu-id="1cb2d-111">Se [Visual Studio-hämtningar](http://go.microsoft.com/fwlink/?LinkID=624384) för mer information.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-111">See [Visual Studio Downloads](http://go.microsoft.com/fwlink/?LinkID=624384) for more information.</span></span>

<span data-ttu-id="1cb2d-112">Du behöver inte hello Azure SDK toogenerate hello skript för webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-112">You do not need hello Azure SDK toogenerate hello scripts for web projects.</span></span> <span data-ttu-id="1cb2d-113">Den här funktionen är för webbprojekt, inte webbroller i molntjänster.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-113">This feature is for web projects, not web roles in cloud services.</span></span>

* <span data-ttu-id="1cb2d-114">Azure PowerShell 0.7.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-114">Azure PowerShell 0.7.4 or later.</span></span> <span data-ttu-id="1cb2d-115">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-115">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span>
* <span data-ttu-id="1cb2d-116">[Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) eller senare.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-116">[Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) or later.</span></span>

## <a name="additional-tools"></a><span data-ttu-id="1cb2d-117">Ytterligare verktyg</span><span class="sxs-lookup"><span data-stu-id="1cb2d-117">Additional tools</span></span>
<span data-ttu-id="1cb2d-118">Det finns ytterligare verktyg och resurser för att arbeta med PowerShell i Visual Studio för Azure-utveckling.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-118">Additional tools and resources for working with PowerShell in Visual Studio for Azure development are available.</span></span> <span data-ttu-id="1cb2d-119">Se [PowerShell-verktyg för Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).</span><span class="sxs-lookup"><span data-stu-id="1cb2d-119">See [PowerShell Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).</span></span>

## <a name="generating-hello-publish-scripts"></a><span data-ttu-id="1cb2d-120">Generera hello publicera skript</span><span class="sxs-lookup"><span data-stu-id="1cb2d-120">Generating hello publish scripts</span></span>
<span data-ttu-id="1cb2d-121">Du kan generera hello publicera skript för en virtuell dator som är värd för din webbplats när du skapar ett nytt projekt genom att följa [instruktionerna](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1cb2d-121">You can generate hello publish scripts for a virtual machine that hosts your website when you create a new project by following [these instructions](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="1cb2d-122">Du kan också [generera publicera skript för web apps i Azure App Service](app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="1cb2d-122">You can also [generate publish scripts for web apps in Azure App Service](app-service-web/app-service-web-get-started-dotnet.md).</span></span>

## <a name="scripts-that-visual-studio-generates"></a><span data-ttu-id="1cb2d-123">Skript som skapas av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1cb2d-123">Scripts that Visual Studio generates</span></span>
<span data-ttu-id="1cb2d-124">Visual Studio skapar en lösning på objektnivå mapp med namnet **PublishScripts** som innehåller två filer i Windows PowerShell, ett publicera-skript för den virtuella datorn eller webbplats och en modul som innehåller funktioner som du kan använda i hello skript.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-124">Visual Studio generates a solution-level folder called **PublishScripts** that contains two Windows PowerShell files, a publish script for your virtual machine or website, and a module that contains functions that you can use in hello scripts.</span></span> <span data-ttu-id="1cb2d-125">Visual Studio genererar också en fil i hello JSON-format som anger hello detaljer i hello-projekt som du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-125">Visual Studio also generates a file in hello JSON format that specifies hello details of hello project you are deploying.</span></span>

### <a name="windows-powershell-publish-script"></a><span data-ttu-id="1cb2d-126">Windows PowerShell publicera skript</span><span class="sxs-lookup"><span data-stu-id="1cb2d-126">Windows PowerShell publish script</span></span>
<span data-ttu-id="1cb2d-127">hello publicera skript innehåller specifika publicera steg för att distribuera tooa webbplats eller virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-127">hello publish script contains specific publish steps for deploying tooa website or virtual machine.</span></span> <span data-ttu-id="1cb2d-128">Visual Studio tillhandahåller syntax färgläggning för Windows PowerShell-utveckling.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-128">Visual Studio provides syntax coloring for Windows PowerShell development.</span></span> <span data-ttu-id="1cb2d-129">Hjälp för hello funktioner är tillgänglig och kan du fritt redigera hello funktioner i hello skriptet toosuit föränderliga behov.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-129">Help for hello functions is available, and you can freely edit hello functions in hello script toosuit your changing requirements.</span></span>

### <a name="windows-powershell-module"></a><span data-ttu-id="1cb2d-130">Windows PowerShell-modulen</span><span class="sxs-lookup"><span data-stu-id="1cb2d-130">Windows PowerShell module</span></span>
<span data-ttu-id="1cb2d-131">hello Windows PowerShell-modulen som Visual Studio genererar innehåller funktioner som hello publicera skriptet använder.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-131">hello Windows PowerShell module that Visual Studio generates contains functions that hello publish script uses.</span></span> <span data-ttu-id="1cb2d-132">Dessa är Azure PowerShell-funktioner och är inte avsedda toobe ändras.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-132">These are Azure PowerShell functions and are not intended toobe modified.</span></span> <span data-ttu-id="1cb2d-133">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-133">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span>

### <a name="json-configuration-file"></a><span data-ttu-id="1cb2d-134">JSON-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="1cb2d-134">JSON configuration file</span></span>
<span data-ttu-id="1cb2d-135">hello JSON-filen skapas i hello **konfigurationer** mapp och innehåller konfigurationsdata som anger exakt vilka resurser toodeploy tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-135">hello JSON file is created in hello **Configurations** folder and contains configuration data that specifies exactly which resources toodeploy tooAzure.</span></span> <span data-ttu-id="1cb2d-136">hello heter hello-filen som Visual Studio genererar projekt-namn-WAWS-dev.json om du har skapat en webbplats eller projekt namnet-VM-dev.json om du har skapat en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-136">hello name of hello file that Visual Studio generates is project-name-WAWS-dev.json if you created a website, or project name-VM-dev.json if you created a virtual machine.</span></span> <span data-ttu-id="1cb2d-137">Här är ett exempel på en JSON-konfigurationsfil som genereras när du skapar en webbplats.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-137">Here's an example of a JSON configuration file that's generated when you create a website.</span></span> <span data-ttu-id="1cb2d-138">De flesta hello värden är självförklarande.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-138">Most of hello values are self-explanatory.</span></span> <span data-ttu-id="1cb2d-139">hello webbplatsnamn genereras av Azure, så den inte kan matcha ditt projektnamn.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-139">hello website name is generated by Azure, so it might not match your project name.</span></span>

```json
{
    "environmentSettings": {
        "webSite": {
            "name": "WebApplication26632",
            "location": "West US"
        },
        "databases": [{
            "connectionStringName": "DefaultConnection",
            "databaseName": "WebApplication26632_db",
            "serverName": "YourDatabaseServerName",
            "user": "sqluser2",
            "password": "",
            "edition": "",
            "size": "",
            "collation": "",
            "location": "West US"
        }]
    }
}
```
<span data-ttu-id="1cb2d-140">När du skapar en virtuell dator verkar liknande toohello följande hello JSON-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-140">When you create a virtual machine, hello JSON configuration file looks similar toohello following.</span></span> <span data-ttu-id="1cb2d-141">Observera att en tjänst i molnet har skapats som en behållare för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-141">Note that a cloud service is created as a container for hello virtual machine.</span></span> <span data-ttu-id="1cb2d-142">hello virtuella dator innehåller hello vanliga slutpunkter för webbåtkomst via HTTP och HTTPS, samt slutpunkter för webbdistribution där du kan publicera toohello webbplatsen från din lokala dator, fjärrskrivbord och Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-142">hello virtual machine contains hello usual endpoints for web access through HTTP and HTTPS, as well as endpoints for Web Deploy, which lets you publish toohello website from your local machine, Remote Desktop, and Windows PowerShell.</span></span>

```json
{
    "environmentSettings": {
        "cloudService": {
            "name": "myusernamevm1",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myusernamevm1",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [{
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [{
            "connectionStringName": "",
            "databaseName": "",
            "serverName": "",
            "user": "",
            "password": ""
        }],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

<span data-ttu-id="1cb2d-143">Du kan redigera hello JSON configuration toochange vad som händer när du kör hello publicera skript.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-143">You can edit hello JSON configuration toochange what happens when you run hello publish scripts.</span></span> <span data-ttu-id="1cb2d-144">Hej `cloudService` och `virtualMachine` avsnitt krävs, men du kan ta bort hello `databases` avsnittet om du inte behöver den.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-144">hello `cloudService` and `virtualMachine` sections are required, but you can delete hello `databases` section if you don't need it.</span></span> <span data-ttu-id="1cb2d-145">hello-egenskaper som är tomma i konfigurationsfilen för hello standard som genereras av Visual Studio är valfritt. de som har värden i hello standardkonfigurationsfilen krävs.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-145">hello properties that are empty in hello default configuration file that Visual Studio generates are optional; those that have values in hello default configuration file are required.</span></span>

<span data-ttu-id="1cb2d-146">Om du har en webbplats som har flera distributionsmiljöer (kallas även fack) i stället för en enda produktionsplatsen i Azure, kan du inkludera hello platsnamnet i hello namnet på hello-webbplats i hello JSON-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-146">If you have a website that has multiple deployment environments (known as slots) instead of a single production site in Azure, you can include hello slot name in hello name of hello website in hello JSON configuration file.</span></span> <span data-ttu-id="1cb2d-147">Om du har en webbplats som heter exempelvis **webbplats** och en plats för den namngivna **testa** hello URI är test.cloudapp.net webbplats, men hello rätt namn toouse i konfigurationsfilen för hello är mysite(test) .</span><span class="sxs-lookup"><span data-stu-id="1cb2d-147">For example, if you have a website that's named **mysite** and a slot for it named **test** then hello URI is mysite-test.cloudapp.net, but hello correct name toouse in hello configuration file is mysite(test).</span></span> <span data-ttu-id="1cb2d-148">Du kan göra detta endast om hello webbplats och distributionsplatser som redan finns i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-148">You can only do this if hello website and slots already exist in your subscription.</span></span> <span data-ttu-id="1cb2d-149">Om de inte redan finns, skapa hello webbplats genom att köra hello skript utan att ange hello plats och skapa sedan hello plats i hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), och därefter köra hello skript med hello ändrade webbplatsnamn.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-149">If they don't exist, create hello website by running hello script without specifying hello slot, then create hello slot in hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), and thereafter run hello script with hello modified website name.</span></span> <span data-ttu-id="1cb2d-150">Mer information om distributionsplatser för webbprogram finns [skapa mellanlagringsmiljöer för web apps i Azure App Service](app-service-web/web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="1cb2d-150">For more information about deployment slots for web apps, see [Set up staging environments for web apps in Azure App Service](app-service-web/web-sites-staged-publishing.md).</span></span>

## <a name="how-toorun-hello-publish-scripts"></a><span data-ttu-id="1cb2d-151">Hur toorun hello publicera skript</span><span class="sxs-lookup"><span data-stu-id="1cb2d-151">How toorun hello publish scripts</span></span>
<span data-ttu-id="1cb2d-152">Om du aldrig har kört Windows PowerShell-skript före, måste du först ställa in hello körning princip tooenable skript toorun.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-152">If you have never run a Windows PowerShell script before, you must first set hello execution policy tooenable scripts toorun.</span></span> <span data-ttu-id="1cb2d-153">Detta är säkerhet funktionen tooprevent användare från att köra Windows PowerShell-skript om de är sårbar toomalware eller virus som rör skript körs.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-153">This is a security feature tooprevent users from running Windows PowerShell scripts if they're vulnerable toomalware or viruses that involve executing scripts.</span></span>

### <a name="run-hello-script"></a><span data-ttu-id="1cb2d-154">Kör hello-skript</span><span class="sxs-lookup"><span data-stu-id="1cb2d-154">Run hello script</span></span>
1. <span data-ttu-id="1cb2d-155">Skapa hello Web Deploy-paket för projektet.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-155">Create hello Web Deploy package for your project.</span></span> <span data-ttu-id="1cb2d-156">Ett Web Deploy-paket är en komprimerad fil (ZIP-fil) som innehåller filer som du vill toocopy tooyour webbplats eller virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-156">A Web Deploy package is a compressed archive (.zip file) that contain files that you want toocopy tooyour website or virtual machine.</span></span> <span data-ttu-id="1cb2d-157">Du kan skapa Web Deploy-paket i Visual Studio för alla webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-157">You can create Web Deploy packages in Visual Studio for any web application.</span></span>

![Skapa webb distribuera paket](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

<span data-ttu-id="1cb2d-159">Mer information finns i [så här: skapa ett Webbdistributionspaket i Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span><span class="sxs-lookup"><span data-stu-id="1cb2d-159">For more information, see [How to: Create a Web Deployment Package in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span></span> <span data-ttu-id="1cb2d-160">Du kan också automatisera hello skapandet av Web Deploy-paket, enligt beskrivningen i avsnittet hello **anpassa och utöka hello publicera skript** senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-160">You can also automate hello creation of your Web Deploy package, as described in hello section **Customizing and extending hello publish scripts** later in this topic.</span></span>

1. <span data-ttu-id="1cb2d-161">I **Solution Explorer**, öppna hello snabbmenyn för hello skript och välj sedan **öppna med PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-161">In **Solution Explorer**, open hello context menu for hello script, and then choose **Open with PowerShell ISE**.</span></span>
2. <span data-ttu-id="1cb2d-162">Om detta är hello första gången som du har kört Windows PowerShell-skript på den här datorn, öppna Kommandotolken med administratörsbehörighet och typen hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1cb2d-162">If this is hello first time you've run Windows PowerShell scripts on this computer, open a command prompt window with Administrator privileges and type hello following command:</span></span>

    ```powershell
    Set-ExecutionPolicy RemoteSigned
    ```

3. <span data-ttu-id="1cb2d-163">Logga in tooAzure med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-163">Sign in tooAzure by using hello following command.</span></span>

    ```powershell
    Add-AzureAccount
    ```

    <span data-ttu-id="1cb2d-164">När du uppmanas, ange ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-164">When prompted, supply your username and password.</span></span>

    <span data-ttu-id="1cb2d-165">Observera att den här metoden för att tillhandahålla autentiseringsuppgifter för Azure inte fungerar när du automatiserar hello skript.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-165">Note that when you automate hello script, this method of providing Azure credentials won't work.</span></span> <span data-ttu-id="1cb2d-166">I stället bör du använda hello .publishsettings-fil tooprovide autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-166">Instead, you should use hello .publishsettings file tooprovide credentials.</span></span> <span data-ttu-id="1cb2d-167">En gång, kommandot hello **Get-AzurePublishSettingsFile** toodownload hello från Azure och därefter använda **importera AzurePublishSettingsFile** tooimport hello-filen.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-167">One time only, you use hello command **Get-AzurePublishSettingsFile** toodownload hello file from Azure, and thereafter use **Import-AzurePublishSettingsFile** tooimport hello file.</span></span> <span data-ttu-id="1cb2d-168">Detaljerade instruktioner finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1cb2d-168">For detailed instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

4. <span data-ttu-id="1cb2d-169">(Valfritt) Om du vill toocreate Azure resurser, till exempel hello virtuell dator, databas och webbplats utan att publicera ditt webbprogram använder hello **publicera WebApplication.ps1** med hello **-konfiguration** argumentet inställt toohello JSON-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-169">(Optional) If you want toocreate Azure resources such as hello virtual machine, database, and website without publishing your web application, use hello **Publish-WebApplication.ps1** command with hello **-Configuration** argument set toohello JSON configuration file.</span></span> <span data-ttu-id="1cb2d-170">Den här kommandoraden använder hello JSON configuration file toodetermine toocreate vilka resurser.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-170">This command line uses hello JSON configuration file toodetermine which resources toocreate.</span></span> <span data-ttu-id="1cb2d-171">Eftersom den använder hello standardinställningarna för andra kommandoradsargument skapar hello resurser, men publicera inte ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-171">Because it uses hello default settings for other command-line arguments, it creates hello resources, but doesn't publish your web application.</span></span> <span data-ttu-id="1cb2d-172">hello – får utförlig alternativet du mer information om vad som händer.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-172">hello –Verbose option gives you more information about what's happening.</span></span>

    ```powershell
    Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json
    ```

5. <span data-ttu-id="1cb2d-173">Använd hello **publicera WebApplication.ps1** kommandot som visas i något av följande exempel tooinvoke hello skript hello och publicera ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-173">Use hello **Publish-WebApplication.ps1** command as shown in one of hello following examples tooinvoke hello script and publish your web application.</span></span> <span data-ttu-id="1cb2d-174">Om du behöver toooverride hello standardinställningarna för alla hello andra argument, till exempel hello prenumerationsnamn publicera paketnamn, autentiseringsuppgifter för virtuell dator eller server Databasautentiseringsuppgifter kan du ange parametrarna.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-174">If you need toooverride hello default settings for any of hello other arguments, such as hello subscription name, publish package name, virtual machine credentials, or database server credentials, you can specify those parameters.</span></span> <span data-ttu-id="1cb2d-175">Använd hello **– utförlig** alternativet toosee mer information om hello förloppet för hello publiceringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-175">Use hello **–Verbose** option toosee more information about hello progress of hello publishing process.</span></span>

    ```powershell
    Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
    –SubscriptionName Contoso `
    -WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
    -DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
    -Verbose
    ```

    <span data-ttu-id="1cb2d-176">Om du skapar en virtuell dator, hello kommando som ser ut som följande hello.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-176">If you're creating a virtual machine, hello command looks like hello following.</span></span> <span data-ttu-id="1cb2d-177">Det här exemplet visar även hur toospecify hello autentiseringsuppgifter för flera databaser.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-177">This example also shows how toospecify hello credentials for multiple databases.</span></span> <span data-ttu-id="1cb2d-178">Hello virtuella datorer som dessa skript skapar är hello SSL-certifikatet inte från en betrodd rotcertifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-178">For hello virtual machines that these scripts create, hello SSL certificate is not from a trusted root authority.</span></span> <span data-ttu-id="1cb2d-179">Du måste därför toouse hello **– AllowUntrusted** alternativet.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-179">Therefore, you need toouse hello **–AllowUntrusted** option.</span></span>

    ```powershell
    Publish-WebApplication.ps1 `
    -Configuration C:\Path\ADVM-VM-test.json `
    -SubscriptionName Contoso `
    -WebDeployPackage C:\Path\ADVM.zip `
    -AllowUntrusted `
    -VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
    -DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
    -Verbose
    ```

    <span data-ttu-id="1cb2d-180">hello skript kan skapa databaser, men det skapar inte databasservrar.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-180">hello script can create databases, but it doesn't create database servers.</span></span> <span data-ttu-id="1cb2d-181">Om du vill toocreate en databasserver, kan du använda hello **ny AzureSqlDatabaseServer** funktion i hello Azure-modulen.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-181">If you want toocreate a database server, you can use hello **New-AzureSqlDatabaseServer** function in hello Azure module.</span></span>

## <a name="customizing-and-extending-hello-publish-scripts"></a><span data-ttu-id="1cb2d-182">Anpassa och utöka hello publicera skript</span><span class="sxs-lookup"><span data-stu-id="1cb2d-182">Customizing and extending hello publish scripts</span></span>
<span data-ttu-id="1cb2d-183">Du kan anpassa hello publicera skript och JSON-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-183">You can customize hello publish script and JSON configuration file.</span></span> <span data-ttu-id="1cb2d-184">Hej funktioner i Windows PowerShell-modulen för hello **AzureWebAppPublishModule.psm1** är inte avsedda toobe ändras.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-184">hello functions in hello Windows PowerShell module **AzureWebAppPublishModule.psm1** are not intended toobe modified.</span></span> <span data-ttu-id="1cb2d-185">Om du bara vill toospecify en annan databas eller ändra några av hello egenskaper för hello virtuell dator kan du redigera hello JSON-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-185">If you just want toospecify a different database or change some of hello properties of hello virtual machine, edit hello JSON configuration file.</span></span> <span data-ttu-id="1cb2d-186">Om du vill använda tooextend hello av hello skriptet tooautomate bygger och testar ditt projekt, kan du implementera funktionen stub-rutiner i **publicera WebApplication.ps1**.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-186">If you want tooextend hello functionality of hello script tooautomate building and testing your project, you can implement function stubs in **Publish-WebApplication.ps1**.</span></span>

<span data-ttu-id="1cb2d-187">tooautomate bygga projektet, lägga till kod som anropar MSBuild för`New-WebDeployPackage` som visas i det här kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-187">tooautomate building your project, add code that calls MSBuild too`New-WebDeployPackage` as shown in this code example.</span></span> <span data-ttu-id="1cb2d-188">hello sökvägen toohello MSBuild-kommandot är olika beroende på hello version av Visual Studio som du har installerat.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-188">hello path toohello MSBuild command is different depending on hello version of Visual Studio you have installed.</span></span> <span data-ttu-id="1cb2d-189">Hej tooget sökväg, som du kan använda funktionen för hello **Get-MSBuildCmd**som visas i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-189">tooget hello correct path, you can use hello function **Get-MSBuildCmd**, as shown in this example.</span></span>

### <a name="tooautomate-building-your-project"></a><span data-ttu-id="1cb2d-190">tooautomate skapa projektet</span><span class="sxs-lookup"><span data-stu-id="1cb2d-190">tooautomate building your project</span></span>
1. <span data-ttu-id="1cb2d-191">Lägg till hello `$ProjectFile` parameter i hello globala param-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-191">Add hello `$ProjectFile` parameter in hello global param section.</span></span>

    ```powershell
    [Parameter(Mandatory = $false)]
    [ValidateScript({Test-Path $_ -PathType Leaf})]
    [String]
    $ProjectFile,
    ```

2. <span data-ttu-id="1cb2d-192">Kopiera hello funktionen `Get-MSBuildCmd` till skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-192">Copy hello function `Get-MSBuildCmd` into your script file.</span></span>

    ```powershell
    function Get-MSBuildCmd
    {
            process
    {

                $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                    Sort-Object {[double]$_.PSChildName} -Descending |
                                    Select-Object -First 1 |
                                    Get-ItemProperty -Name MSBuildToolsPath |
                                    Select -ExpandProperty MSBuildToolsPath

                $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

            return Get-Item $path
        }
    }
    ```

3. <span data-ttu-id="1cb2d-193">Ersätt `New-WebDeployPackage` med hello följande kod och Ersätt hello platshållare i hello rad konstruera `$msbuildCmd`.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-193">Replace `New-WebDeployPackage` with hello following code and replace hello placeholders in hello line constructing `$msbuildCmd`.</span></span> <span data-ttu-id="1cb2d-194">Den här koden är för Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-194">This code is for Visual Studio 2015.</span></span> <span data-ttu-id="1cb2d-195">Om du använder Visual Studio 2013, ändra hello **VisualStudioVersion** egenskapen nedan för`12.0`.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-195">If you're using Visual Studio 2013, change hello **VisualStudioVersion** property below too`12.0`.</span></span>

    ```powershell
    function New-WebDeployPackage
    {
        #Write a function toobuild and package your web application
    ```

    <span data-ttu-id="1cb2d-196">toobuild ditt webbprogram som använder MsBuild.exe.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-196">toobuild your web application, use MsBuild.exe.</span></span> <span data-ttu-id="1cb2d-197">Mer information finns i MSBuild Kommandoradsreferens på: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)</span><span class="sxs-lookup"><span data-stu-id="1cb2d-197">For help, see MSBuild Command-Line Reference at: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)</span></span>

    ```powershell
    Write-VerboseWithTime 'Build-WebDeployPackage: Start'

    $msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory

    Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
    ```

### <a name="start-execution-of-hello-build-command"></a><span data-ttu-id="1cb2d-198">Starta körning av hello build-kommando</span><span class="sxs-lookup"><span data-stu-id="1cb2d-198">Start execution of hello build command</span></span>

```powershell
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru

if ($job.ExitCode -ne 0) {
    throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain hello project name
$projectName = (Get-Item $ProjectFile).BaseName

#Construct hello path tooweb deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName

#Get hello full path for hello web deploy zip package. This is required for MSDeploy toowork
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir

Write-VerboseWithTime 'Build-WebDeployPackage: End'

return $WebDeployPackage
}
```

1. <span data-ttu-id="1cb2d-199">Anropa hello `New-WebDeployPackage` funktionen innan den här raden: `$Config = Read-ConfigFile $Configuration` för webbappar eller `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-199">Call hello `New-WebDeployPackage` function before this line: `$Config = Read-ConfigFile $Configuration` for web apps or `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` for virtual machines.</span></span>

    ```powershell
    if($ProjectFile)
    {
    $WebDeployPackage = New-WebDeployPackage
    }
    ```

2. <span data-ttu-id="1cb2d-200">Anropa hello anpassade skript från kommandoraden med hjälp av passera hello `$Project` argument, som i följande exempel kommandoraden hello.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-200">Invoke hello customized script from command line using passing hello `$Project` argument, as in hello following example command line.</span></span>

    ```powershell
    .\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
    -ProjectFile ..\WebApplication5\WebApplication5.csproj `
    -VMPassword @{Name="VMUser";Password="Test.123"} `
    -AllowUntrusted `
    -Verbose
    ```

    <span data-ttu-id="1cb2d-201">tooautomate testning av programmet, lägger du till kod för`Test-WebApplication`.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-201">tooautomate testing of your application, add code too`Test-WebApplication`.</span></span> <span data-ttu-id="1cb2d-202">Vara säker på att toouncomment hello rader i **publicera WebApplication.ps1** där dessa funktioner kallas.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-202">Be sure toouncomment hello lines in **Publish-WebApplication.ps1** where these functions are called.</span></span> <span data-ttu-id="1cb2d-203">Om du inte anger en implementering kan du skapa manuellt ditt projekt i Visual Studio och kör hello publicera skriptet toopublish tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-203">If you don't provide an implementation, you can manually build your project with Visual Studio, and then run hello publish script toopublish tooAzure.</span></span>

## <a name="publishing-function-summary"></a><span data-ttu-id="1cb2d-204">Sammanfattning av Publishing funktioner</span><span class="sxs-lookup"><span data-stu-id="1cb2d-204">Publishing function summary</span></span>
<span data-ttu-id="1cb2d-205">tooget hjälp för funktioner som du kan använda Kommandotolken hello Windows PowerShell, Använd hello kommando `Get-Help function-name`.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-205">tooget help for functions you can use at hello Windows PowerShell command prompt, use hello command `Get-Help function-name`.</span></span> <span data-ttu-id="1cb2d-206">hello-hjälpen innehåller parametern hjälp och exempel.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-206">hello help includes parameter help and examples.</span></span> <span data-ttu-id="1cb2d-207">hello samma hjälptexten finns också i hello skript källfiler **AzureWebAppPublishModule.psm1** och **publicera WebApplication.ps1**.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-207">hello same help text is also in hello script source files, **AzureWebAppPublishModule.psm1** and **Publish-WebApplication.ps1**.</span></span> <span data-ttu-id="1cb2d-208">hello skript och hjälper dig lokaliserade i Visual Studio-språk.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-208">hello script and help are localized in your Visual Studio language.</span></span>

<span data-ttu-id="1cb2d-209">**AzureWebAppPublishModule**</span><span class="sxs-lookup"><span data-stu-id="1cb2d-209">**AzureWebAppPublishModule**</span></span>

| <span data-ttu-id="1cb2d-210">Funktionsnamn</span><span class="sxs-lookup"><span data-stu-id="1cb2d-210">Function name</span></span> | <span data-ttu-id="1cb2d-211">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1cb2d-211">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1cb2d-212">Lägg till AzureSQLDatabase</span><span class="sxs-lookup"><span data-stu-id="1cb2d-212">Add-AzureSQLDatabase</span></span> |<span data-ttu-id="1cb2d-213">Skapar en ny Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-213">Creates a new Azure SQL database.</span></span> |
| <span data-ttu-id="1cb2d-214">Lägg till AzureSQLDatabases</span><span class="sxs-lookup"><span data-stu-id="1cb2d-214">Add-AzureSQLDatabases</span></span> |<span data-ttu-id="1cb2d-215">Skapar Azure SQL-databaser från hello-värdena i hello JSON-konfigurationsfil som genereras av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-215">Creates Azure SQL databases from hello values in hello JSON configuration file that Visual Studio generates.</span></span> |
| <span data-ttu-id="1cb2d-216">Lägg till AzureVM</span><span class="sxs-lookup"><span data-stu-id="1cb2d-216">Add-AzureVM</span></span> |<span data-ttu-id="1cb2d-217">Skapar en virtuell dator i Azure och returnerar hello URL för hello distribueras VM.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-217">Creates a Azure virtual machine and returns hello URL of hello deployed VM.</span></span> <span data-ttu-id="1cb2d-218">hello funktionen ställer in hello förutsättningar och sedan anropar hello **ny AzureVM** fungera (Azure-modulen) toocreate en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-218">hello function sets up hello prerequisites and then calls hello **New-AzureVM** function (Azure module) toocreate a new virtual machine.</span></span> |
| <span data-ttu-id="1cb2d-219">Lägg till AzureVMEndpoints</span><span class="sxs-lookup"><span data-stu-id="1cb2d-219">Add-AzureVMEndpoints</span></span> |<span data-ttu-id="1cb2d-220">Lägger till nya virtuella datorn för inkommande slutpunkter tooa och returnerar hello virtuell dator med hello ny slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-220">Adds new input endpoints tooa virtual machine and returns hello virtual machine with hello new endpoint.</span></span> |
| <span data-ttu-id="1cb2d-221">Lägg till AzureVMStorage</span><span class="sxs-lookup"><span data-stu-id="1cb2d-221">Add-AzureVMStorage</span></span> |<span data-ttu-id="1cb2d-222">Skapar ett nytt Azure storage-konto i hello nuvarande prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-222">Creates a new Azure storage account in hello current subscription.</span></span> <span data-ttu-id="1cb2d-223">hello namnet på hello konto börjar med ”devtest” följt av en unik alfanumerisk sträng.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-223">hello name of hello account begins with "devtest" followed by a unique alphanumeric string.</span></span> <span data-ttu-id="1cb2d-224">hello returneras hello namnet på hello nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-224">hello function returns hello name of hello new storage account.</span></span> <span data-ttu-id="1cb2d-225">Du måste ange en plats eller en tillhörighetsgrupp för hello nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-225">You must specify either a location or an affinity group for hello new storage account.</span></span> |
| <span data-ttu-id="1cb2d-226">Lägg till AzureWebsite</span><span class="sxs-lookup"><span data-stu-id="1cb2d-226">Add-AzureWebsite</span></span> |<span data-ttu-id="1cb2d-227">Skapar en webbplats med angivna hello-namn och plats.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-227">Creates a website with hello specified name and location.</span></span> <span data-ttu-id="1cb2d-228">Den här funktionen anropar hello **ny AzureWebsite** funktion i hello Azure-modulen.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-228">This function calls hello **New-AzureWebsite** function in hello Azure module.</span></span> <span data-ttu-id="1cb2d-229">Om hello prenumerationen inte redan innehåller en webbplats med angivna hello-namnet, den här funktionen skapar hello webbplats och returnerar ett objekt för webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-229">If hello subscription doesn't already include a website with hello specified name, this function creates hello website and returns a website object.</span></span> <span data-ttu-id="1cb2d-230">Annars returneras `$null`.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-230">Otherwise, it returns `$null`.</span></span> |
| <span data-ttu-id="1cb2d-231">Backup-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1cb2d-231">Backup-Subscription</span></span> |<span data-ttu-id="1cb2d-232">Sparar hello aktuella Azure-prenumeration i hello `$Script:originalSubscription` variabeln i skriptet omfång. Den här funktionen sparar hello aktuella Azure-prenumeration (som erhålls av `Get-AzureSubscription -Current`) och storage-konto, och hello-prenumeration som har ändrats med det här skriptet (lagrad i hello variabel `$UserSpecifiedSubscription`) och dess storage-kontot i skriptet omfång.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-232">Saves hello current Azure subscription in hello `$Script:originalSubscription` variable in script scope.This function saves hello current Azure subscription (as obtained by `Get-AzureSubscription -Current`) and its storage account, and hello subscription that is changed by this script (stored in hello variable `$UserSpecifiedSubscription`) and its storage account, in script scope.</span></span> <span data-ttu-id="1cb2d-233">Genom att spara hello värden kan du använda en funktion som `Restore-Subscription`, toorestore hello ursprungliga aktuell prenumeration och lagring toocurrent kontostatus om hello aktuell status har ändrats.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-233">By saving hello values, you can use a function, such as `Restore-Subscription`, toorestore hello original current subscription and storage account toocurrent status if hello current status has changed.</span></span> |
| <span data-ttu-id="1cb2d-234">Hitta AzureVM</span><span class="sxs-lookup"><span data-stu-id="1cb2d-234">Find-AzureVM</span></span> |<span data-ttu-id="1cb2d-235">Hämtar hello angivna virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-235">Gets hello specified Azure virtual machine.</span></span> |
| <span data-ttu-id="1cb2d-236">Formatet DevTestMessageWithTime</span><span class="sxs-lookup"><span data-stu-id="1cb2d-236">Format-DevTestMessageWithTime</span></span> |<span data-ttu-id="1cb2d-237">Annat datum och tid tooa hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-237">Prepends hello date and time tooa message.</span></span> <span data-ttu-id="1cb2d-238">Den här funktionen är avsedd för meddelanden som skrivs toohello fel och utförlig dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-238">This function is designed for messages written toohello Error and Verbose streams.</span></span> |
| <span data-ttu-id="1cb2d-239">Get-AzureSQLDatabaseConnectionString</span><span class="sxs-lookup"><span data-stu-id="1cb2d-239">Get-AzureSQLDatabaseConnectionString</span></span> |<span data-ttu-id="1cb2d-240">Monterar en anslutning sträng tooconnect tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-240">Assembles a connection string tooconnect tooan Azure SQL database.</span></span> |
| <span data-ttu-id="1cb2d-241">Get-AzureVMStorage</span><span class="sxs-lookup"><span data-stu-id="1cb2d-241">Get-AzureVMStorage</span></span> |<span data-ttu-id="1cb2d-242">Returnerar hello namnet på hello första lagringskonto med hello namnmönster ”devtest*” (skiftlägeskänsligt) i hello angivna plats eller tillhörighet gruppen. Om hello ”devtest*” lagringskontot matchar inte hello platsen eller tillhörighetsgruppen, hello funktionen ignoreras.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-242">Returns hello name of hello first storage account with hello name pattern "devtest*" (case insensitive) in hello specified location or affinity group. If hello "devtest*" storage account doesn't match hello location or affinity group, hello function ignores it.</span></span> <span data-ttu-id="1cb2d-243">Du måste ange en plats eller en tillhörighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-243">You must specify either a location or an affinity group.</span></span> |
| <span data-ttu-id="1cb2d-244">Get-MSDeployCmd</span><span class="sxs-lookup"><span data-stu-id="1cb2d-244">Get-MSDeployCmd</span></span> |<span data-ttu-id="1cb2d-245">Returnerar ett kommando toorun hello MsDeploy.exe verktyg.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-245">Returns a command toorun hello MsDeploy.exe tool.</span></span> |
| <span data-ttu-id="1cb2d-246">Ny AzureVMEnvironment</span><span class="sxs-lookup"><span data-stu-id="1cb2d-246">New-AzureVMEnvironment</span></span> |<span data-ttu-id="1cb2d-247">Söker efter eller skapar en virtuell dator i hello-prenumeration som matchar hello värden i hello JSON-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-247">Finds or creates a virtual machine in hello subscription that matches hello values in hello JSON configuration file.</span></span> |
| <span data-ttu-id="1cb2d-248">Publicera WebPackage</span><span class="sxs-lookup"><span data-stu-id="1cb2d-248">Publish-WebPackage</span></span> |<span data-ttu-id="1cb2d-249">Publicera paketet använder MsDeploy.exe och en webbplats. ZIP-filen toodeploy resurser tooa webbplats.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-249">Uses MsDeploy.exe and a web publish package .Zip file toodeploy resources tooa website.</span></span> <span data-ttu-id="1cb2d-250">Den här funktionen genererar inte inga utdata.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-250">This function doesn't generate any output.</span></span> <span data-ttu-id="1cb2d-251">Om hello anropet tooMSDeploy.exe misslyckas genereras hello ett undantag.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-251">If hello call tooMSDeploy.exe fails, hello function throws an exception.</span></span> <span data-ttu-id="1cb2d-252">tooget mer detaljerade utdata, Använd hello **-Verbose** alternativet.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-252">tooget more detailed output, use hello **-Verbose** option.</span></span> |
| <span data-ttu-id="1cb2d-253">Publicera WebPackageToVM</span><span class="sxs-lookup"><span data-stu-id="1cb2d-253">Publish-WebPackageToVM</span></span> |<span data-ttu-id="1cb2d-254">Verifierar hello parametervärden och anropar sedan hello **publicera WebPackage** funktion.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-254">Verifies hello parameter values, and then calls hello **Publish-WebPackage** function.</span></span> |
| <span data-ttu-id="1cb2d-255">Läs ConfigFile</span><span class="sxs-lookup"><span data-stu-id="1cb2d-255">Read-ConfigFile</span></span> |<span data-ttu-id="1cb2d-256">Validerar hello JSON-konfigurationsfil och returnerar en hash-tabell för valda värden.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-256">Validates hello JSON configuration file and returns a hash table of selected values.</span></span> |
| <span data-ttu-id="1cb2d-257">Restore-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1cb2d-257">Restore-Subscription</span></span> |<span data-ttu-id="1cb2d-258">Återställer hello aktuell prenumeration toohello ursprungliga prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-258">Resets hello current subscription toohello original subscription.</span></span> |
| <span data-ttu-id="1cb2d-259">Testa AzureModule</span><span class="sxs-lookup"><span data-stu-id="1cb2d-259">Test-AzureModule</span></span> |<span data-ttu-id="1cb2d-260">Returnerar `$true` om hello installerade Azure Modulversion är 0.7.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-260">Returns `$true` if hello installed Azure module version is 0.7.4 or later.</span></span> <span data-ttu-id="1cb2d-261">Returnerar `$false` om hello modulen är inte installerat eller är en tidigare version.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-261">Returns `$false` if hello module isn't installed or is an earlier version.</span></span> <span data-ttu-id="1cb2d-262">Den här funktionen har inga parametrar.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-262">This function has no parameters.</span></span> |
| <span data-ttu-id="1cb2d-263">Testa AzureModuleVersion</span><span class="sxs-lookup"><span data-stu-id="1cb2d-263">Test-AzureModuleVersion</span></span> |<span data-ttu-id="1cb2d-264">Returnerar `$true` hello versionen om hello Azure-modulen är 0.7.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-264">Returns `$true` if hello version of hello Azure module is 0.7.4 or later.</span></span> <span data-ttu-id="1cb2d-265">Returnerar `$false` om hello modulen är inte installerat eller är en tidigare version.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-265">Returns `$false` if hello module isn't installed or is an earlier version.</span></span> <span data-ttu-id="1cb2d-266">Den här funktionen har inga parametrar.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-266">This function has no parameters.</span></span> |
| <span data-ttu-id="1cb2d-267">Testa HttpsUrl</span><span class="sxs-lookup"><span data-stu-id="1cb2d-267">Test-HttpsUrl</span></span> |<span data-ttu-id="1cb2d-268">Konverterar hello inkommande URL tooa System.Uri-objekt.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-268">Converts hello input URL tooa System.Uri object.</span></span> <span data-ttu-id="1cb2d-269">Returnerar `$True` om hello URL är absolut och dess schemat är https.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-269">Returns `$True` if hello URL is absolute and its scheme is https.</span></span> <span data-ttu-id="1cb2d-270">Returnerar `$false` om hello URL: en är relativ, dess schema inte är HTTPS eller hello Indatasträngen går inte att konvertera tooa URL.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-270">Returns `$false` if hello URL is relative, its scheme isn't HTTPS, or hello input string can't be converted tooa URL.</span></span> |
| <span data-ttu-id="1cb2d-271">Test-medlem</span><span class="sxs-lookup"><span data-stu-id="1cb2d-271">Test-Member</span></span> |<span data-ttu-id="1cb2d-272">Returnerar `$true` om en egenskap eller metod är medlem i hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-272">Returns `$true` if a property or method is a member of hello object.</span></span> <span data-ttu-id="1cb2d-273">Annars returnerar `$false`.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-273">Otherwise, returns `$false`.</span></span> |
| <span data-ttu-id="1cb2d-274">Skriv ErrorWithTime</span><span class="sxs-lookup"><span data-stu-id="1cb2d-274">Write-ErrorWithTime</span></span> |<span data-ttu-id="1cb2d-275">Skriver ett felmeddelande med hello prefixet aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-275">Writes an error message prefixed with hello current time.</span></span> <span data-ttu-id="1cb2d-276">Den här funktionen anropar hello **Format DevTestMessageWithTime** funktionen tooprepend hello tid innan skrivning hello meddelandeströmmen toohello fel.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-276">This function calls hello **Format-DevTestMessageWithTime** function tooprepend hello time before writing hello message toohello Error stream.</span></span> |
| <span data-ttu-id="1cb2d-277">Skriv HostWithTime</span><span class="sxs-lookup"><span data-stu-id="1cb2d-277">Write-HostWithTime</span></span> |<span data-ttu-id="1cb2d-278">Skriver ett meddelande toohello värdprogram (**Write-Host**) med hello prefixet aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-278">Writes a message toohello host program (**Write-Host**) prefixed with hello current time.</span></span> <span data-ttu-id="1cb2d-279">hello effekten av skriva toohello värdprogrammet varierar.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-279">hello effect of writing toohello host program varies.</span></span> <span data-ttu-id="1cb2d-280">De flesta program som är värdar för Windows PowerShell skriver dessa meddelanden toostandard utdata.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-280">Most programs that host Windows PowerShell write these messages toostandard output.</span></span> |
| <span data-ttu-id="1cb2d-281">Skriv VerboseWithTime</span><span class="sxs-lookup"><span data-stu-id="1cb2d-281">Write-VerboseWithTime</span></span> |<span data-ttu-id="1cb2d-282">Skriver ett utförligt meddelande med hello prefixet aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-282">Writes a verbose message prefixed with hello current time.</span></span> <span data-ttu-id="1cb2d-283">Eftersom det anropar **Write-Verbose**, hello-meddelande visas endast när hello körs skriptet med hello **utförlig** parametern eller när hello **VerbosePreference** inställningar är ställa in också**Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-283">Because it calls **Write-Verbose**, hello message displays only when hello script runs with hello **Verbose** parameter or when hello **VerbosePreference** preference is set too**Continue**.</span></span> |

<span data-ttu-id="1cb2d-284">**Publicera WebApplication**</span><span class="sxs-lookup"><span data-stu-id="1cb2d-284">**Publish-WebApplication**</span></span>

| <span data-ttu-id="1cb2d-285">Funktionsnamn</span><span class="sxs-lookup"><span data-stu-id="1cb2d-285">Function name</span></span> | <span data-ttu-id="1cb2d-286">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1cb2d-286">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1cb2d-287">Ny AzureWebApplicationEnvironment</span><span class="sxs-lookup"><span data-stu-id="1cb2d-287">New-AzureWebApplicationEnvironment</span></span> |<span data-ttu-id="1cb2d-288">Skapar Azure-resurser, till exempel en webbplats eller virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-288">Creates Azure resources, such as a website or virtual machine.</span></span> |
| <span data-ttu-id="1cb2d-289">Ny WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="1cb2d-289">New-WebDeployPackage</span></span> |<span data-ttu-id="1cb2d-290">Den här funktionen är inte implementerad.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-290">This function isn't implemented.</span></span> <span data-ttu-id="1cb2d-291">Du kan lägga till kommandon i den här funktionen toobuild projektet.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-291">You can add commands in this function toobuild your project.</span></span> |
| <span data-ttu-id="1cb2d-292">Publicera AzureWebApplication</span><span class="sxs-lookup"><span data-stu-id="1cb2d-292">Publish-AzureWebApplication</span></span> |<span data-ttu-id="1cb2d-293">Publicerar en web application tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-293">Publishes a web application tooAzure.</span></span> |
| <span data-ttu-id="1cb2d-294">Publicera WebApplication</span><span class="sxs-lookup"><span data-stu-id="1cb2d-294">Publish-WebApplication</span></span> |<span data-ttu-id="1cb2d-295">Skapar och distribuerar Web Apps, virtuella datorer, SQL-databaser och storage-konton för ett webbprojekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-295">Creates and deploys Web Apps, virtual machines, SQL databases, and storage accounts for a Visual Studio web project.</span></span> |
| <span data-ttu-id="1cb2d-296">Test-WebApplication</span><span class="sxs-lookup"><span data-stu-id="1cb2d-296">Test-WebApplication</span></span> |<span data-ttu-id="1cb2d-297">Den här funktionen är inte implementerad.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-297">This function isn't implemented.</span></span> <span data-ttu-id="1cb2d-298">Du kan lägga till kommandon i den här funktionen tootest ditt program.</span><span class="sxs-lookup"><span data-stu-id="1cb2d-298">You can add commands in this function tootest your application.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1cb2d-299">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1cb2d-299">Next steps</span></span>
<span data-ttu-id="1cb2d-300">Mer information om PowerShell-skript genom att läsa [med Windows PowerShell-skript](https://technet.microsoft.com/library/bb978526.aspx) och se andra Azure PowerShell-skript på hello [Script Center](https://azure.microsoft.com/documentation/scripts/).</span><span class="sxs-lookup"><span data-stu-id="1cb2d-300">Learn more about PowerShell scripting by reading [Scripting with Windows PowerShell](https://technet.microsoft.com/library/bb978526.aspx) and see other Azure PowerShell scripts at hello [Script Center](https://azure.microsoft.com/documentation/scripts/).</span></span>
