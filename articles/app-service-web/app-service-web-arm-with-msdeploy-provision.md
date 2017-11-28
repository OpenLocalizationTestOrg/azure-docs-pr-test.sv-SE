---
title: "aaaDeploy ett webbprogram med hjälp av MSDeploy med värdnamn och ssl-certifikat"
description: "Använda en Azure Resource Manager-mall toodeploy ett webbprogram med hjälp av MSDeploy och konfigurera anpassade värdnamnet och ett SSL-certifikat"
services: app-service\web
manager: erikre
documentationcenter: 
author: jodehavi
ms.assetid: 66366a72-cef7-4d75-8779-f4d32ed33cf7
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2016
ms.author: jodehavi
ms.openlocfilehash: ac13f4a7d14ae182e8e7ced5adff30491422d1e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a><span data-ttu-id="62555-103">Distribuera en webbapp med MSDeploy, anpassade värdnamnet och SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="62555-103">Deploy a web app with MSDeploy, custom hostname and SSL certificate</span></span>
<span data-ttu-id="62555-104">Den här guiden går igenom skapar en distribution för slutpunkt till slutpunkt för en Azure-Webbapp, utnyttja MSDeploy såväl som att lägga till ett anpassat värdnamn och en SSL-certifikat toohello ARM-mall.</span><span class="sxs-lookup"><span data-stu-id="62555-104">This guide walks through creating an end-to-end deployment for an Azure Web App, leveraging MSDeploy as well as adding a custom hostname and an SSL certificate toohello ARM template.</span></span>

<span data-ttu-id="62555-105">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="62555-105">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

### <a name="create-sample-application"></a><span data-ttu-id="62555-106">Skapa exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="62555-106">Create Sample Application</span></span>
<span data-ttu-id="62555-107">Du ska distribuera ett ASP.NET-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="62555-107">You will be deploying an ASP.NET web application.</span></span> <span data-ttu-id="62555-108">hello första steget är toocreate en enkel webbapp (eller kan du välja toouse till en befintlig, i så fall kan du hoppa över det här steget).</span><span class="sxs-lookup"><span data-stu-id="62555-108">hello first step is toocreate a simple web application (or you could choose toouse an existing one - in which case you can skip this step).</span></span>

<span data-ttu-id="62555-109">Öppna Visual Studio 2015 och välj Arkiv > Nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="62555-109">Open Visual Studio 2015 and choose File > New Project.</span></span> <span data-ttu-id="62555-110">På hello dialogrutan som visas väljer du webb > ASP.NET-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="62555-110">On hello dialog that appears choose Web > ASP.NET Web Application.</span></span> <span data-ttu-id="62555-111">Välj webbprogram och välj hello MVC-mallen under mallar.</span><span class="sxs-lookup"><span data-stu-id="62555-111">Under Templates choose Web and choose hello MVC template.</span></span> <span data-ttu-id="62555-112">Välj *ändra autentiseringstyp* för*ingen autentisering*.</span><span class="sxs-lookup"><span data-stu-id="62555-112">Select *Change authentication type* too*No Authentication*.</span></span> <span data-ttu-id="62555-113">Detta är bara toomake hello exempelprogrammet så enkel som möjligt.</span><span class="sxs-lookup"><span data-stu-id="62555-113">This is just toomake hello sample application as simple as possible.</span></span>

<span data-ttu-id="62555-114">Nu har du en grundläggande ASP.Net web app redo toouse som en del av distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="62555-114">At this point you will have a basic ASP.Net web app ready toouse as part of your deployment process.</span></span>

### <a name="create-msdeploy-package"></a><span data-ttu-id="62555-115">Skapa MSDeploy-paket</span><span class="sxs-lookup"><span data-stu-id="62555-115">Create MSDeploy package</span></span>
<span data-ttu-id="62555-116">Nästa steg är toocreate hello paketet toodeploy hello web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="62555-116">Next step is toocreate hello package toodeploy hello web app tooAzure.</span></span> <span data-ttu-id="62555-117">toodo, spara projektet och sedan köra hello följande från hello kommandorad:</span><span class="sxs-lookup"><span data-stu-id="62555-117">toodo this, save your project and then run hello following from hello command line:</span></span>

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

<span data-ttu-id="62555-118">Detta skapar en komprimerade paketet hello anges i PackageLocation i mappen.</span><span class="sxs-lookup"><span data-stu-id="62555-118">This will create a zipped package under hello PackageLocation folder.</span></span> <span data-ttu-id="62555-119">hello program är nu redo toobe har distribuerats, som du kan nu skapa ut toodo en Azure Resource Manager-mall som.</span><span class="sxs-lookup"><span data-stu-id="62555-119">hello application is now ready toobe deployed, which you can now build out an Azure Resource Manager template toodo that.</span></span>

### <a name="create-arm-template"></a><span data-ttu-id="62555-120">Skapa ARM-mall</span><span class="sxs-lookup"><span data-stu-id="62555-120">Create ARM Template</span></span>
<span data-ttu-id="62555-121">Först måste börja med en grundläggande ARM-mall som skapar ett webbprogram och en plan för värd (Observera att parametrar och variabler som används inte visas planeringsaspekter).</span><span class="sxs-lookup"><span data-stu-id="62555-121">First, let's start with a basic ARM template that will create a web application and a hosting plan (note that parameters and variables are not shown for brevity).</span></span>

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

<span data-ttu-id="62555-122">Därefter måste toomodify hello web app resurs tootake en kapslad MSDeploy-resurs.</span><span class="sxs-lookup"><span data-stu-id="62555-122">Next, you will need toomodify hello web app resource tootake a nested MSDeploy resource.</span></span> <span data-ttu-id="62555-123">Detta tillåter du tooreference hello paketet skapade tidigare och tala om för Azure Resource Manager toouse MSDeploy toodeploy hello paketet toohello Azure WebApp.</span><span class="sxs-lookup"><span data-stu-id="62555-123">This will allow you tooreference hello package created earlier and tell Azure Resource Manager toouse MSDeploy toodeploy hello package toohello Azure WebApp.</span></span> <span data-ttu-id="62555-124">hello följande visar hello Microsoft.Web/sites resursen med hello kapslade MSDeploy resurs:</span><span class="sxs-lookup"><span data-stu-id="62555-124">hello following shows hello Microsoft.Web/sites resource with hello nested MSDeploy resource:</span></span>

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

<span data-ttu-id="62555-125">Nu kommer du att märka att hello MSDeploy resurs tar en **packageUri** -egenskap som definieras på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="62555-125">Now you will notice that hello MSDeploy resource takes a **packageUri** property which is defined as follows:</span></span>

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

<span data-ttu-id="62555-126">Detta **packageUri** tar hello lagringskonto-uri som pekar toohello storage-konto där du kommer att överföra din zip paketet till.</span><span class="sxs-lookup"><span data-stu-id="62555-126">This **packageUri** takes hello storage account uri which points toohello storage account where you will upload your package zip to.</span></span> <span data-ttu-id="62555-127">hello Azure Resource Manager använder [signaturer för delad åtkomst](../storage/common/storage-dotnet-shared-access-signature-part-1.md) toopull hello paketet ned lokalt från hello storage-konto när du distribuerar hello mallen.</span><span class="sxs-lookup"><span data-stu-id="62555-127">hello Azure Resource Manager will leverage [Shared Access Signatures](../storage/common/storage-dotnet-shared-access-signature-part-1.md) toopull hello package down locally from hello storage account when you deploy hello template.</span></span> <span data-ttu-id="62555-128">Den här processen automatiskt via ett PowerShell-skript som överföra hello-paket och anropa hello Azure Management API toocreate hello nycklar som krävs och skicka dem till hello mall som parametrar (*_artifactsLocation* och *_artifactsLocationSasToken*).</span><span class="sxs-lookup"><span data-stu-id="62555-128">This process will be automated via a PowerShell script that will upload hello package and call hello Azure Management API toocreate hello keys required and pass those into hello template as parameters (*_artifactsLocation* and *_artifactsLocationSasToken*).</span></span> <span data-ttu-id="62555-129">Du behöver toodefine parametrar för hello mapp och filnamn hello paketet är överförda toounder hello lagringsbehållaren.</span><span class="sxs-lookup"><span data-stu-id="62555-129">You will need toodefine parameters for hello folder and filename hello package is uploaded toounder hello storage container.</span></span>

<span data-ttu-id="62555-130">Du måste sedan tooadd i en annan kapslad resurs toosetup hello hostname bindningar tooleverage anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="62555-130">Next you need tooadd in another nested resource toosetup hello hostname bindings tooleverage a custom domain.</span></span> <span data-ttu-id="62555-131">Du kommer första tooensure behovet av att du äger hello värdnamn och konfigurera toobe verifieras av Azure att du äger den - se [konfigurera ett anpassat domännamn i Azure App Service](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="62555-131">You will first need tooensure that you own hello hostname and set it up toobe verified by Azure that you own it - see [Configure a custom domain name in Azure App Service](app-service-web-tutorial-custom-domain.md).</span></span> <span data-ttu-id="62555-132">När det är klart kan du lägga till hello följande tooyour mallen under avsnittet för hello Microsoft.Web/sites resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="62555-132">Once that is done you can add hello following tooyour template under hello Microsoft.Web/sites resource section:</span></span>

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

<span data-ttu-id="62555-133">Slutligen måste tooadd en annan toppnivåresurs, Microsoft.Web/certificates.</span><span class="sxs-lookup"><span data-stu-id="62555-133">Finally you need tooadd another top level resource, Microsoft.Web/certificates.</span></span> <span data-ttu-id="62555-134">Den här resursen innehåller SSL-certifikat och kommer att finnas på samma nivå som ditt webbprogram och värd planera hello.</span><span class="sxs-lookup"><span data-stu-id="62555-134">This resource will contain your SSL certificate and will exist at hello same level as your web app and hosting plan.</span></span>

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

<span data-ttu-id="62555-135">Du behöver toohave ett giltigt SSL-certifikat i ordning tooset upp den här resursen.</span><span class="sxs-lookup"><span data-stu-id="62555-135">You will need toohave a valid SSL certificate in order tooset up this resource.</span></span> <span data-ttu-id="62555-136">När du har det giltigt certifikatet måste tooextract hello pfx byte som en base64-sträng.</span><span class="sxs-lookup"><span data-stu-id="62555-136">Once you have that valid certificate then you need tooextract hello pfx bytes as a base64 string.</span></span> <span data-ttu-id="62555-137">Ett alternativ tooextract är toouse hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="62555-137">One option tooextract this is toouse hello following PowerShell command:</span></span>

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

<span data-ttu-id="62555-138">Du kan sedan överföra detta som en parameter tooyour ARM Distributionsmall.</span><span class="sxs-lookup"><span data-stu-id="62555-138">You could then pass this as a parameter tooyour ARM deployment template.</span></span>

<span data-ttu-id="62555-139">Hello ARM-mallen är nu klar.</span><span class="sxs-lookup"><span data-stu-id="62555-139">At this point hello ARM template is ready.</span></span>

### <a name="deploy-template"></a><span data-ttu-id="62555-140">Distribuera mallen</span><span class="sxs-lookup"><span data-stu-id="62555-140">Deploy Template</span></span>
<span data-ttu-id="62555-141">Hej sista stegen är toopiece detta alla tillsammans i en fullständig slutpunkt till slutpunkt-distribution.</span><span class="sxs-lookup"><span data-stu-id="62555-141">hello final steps are toopiece this all together into a full end-to-end deployment.</span></span> <span data-ttu-id="62555-142">toomake distribution enklare kan du använda hello **distribuera AzureResourceGroup.ps1** PowerShell-skript som läggs till när du skapar en Azure-resursgrupp-projekt i Visual Studio toohelp med överföra alla artefakter som krävs i hello mall.</span><span class="sxs-lookup"><span data-stu-id="62555-142">toomake deployment easier you can leverage hello **Deploy-AzureResourceGroup.ps1** PowerShell script that is added when you create an Azure Resource Group project in Visual Studio toohelp with uploading of any artifacts required in hello template.</span></span> <span data-ttu-id="62555-143">Det krävs toohave skapat ett lagringskonto som du vill toouse i förväg.</span><span class="sxs-lookup"><span data-stu-id="62555-143">It requires you toohave created a storage account you want toouse ahead of time.</span></span> <span data-ttu-id="62555-144">Jag har skapat ett konto för delad lagring för hello package.zip toobe upp i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="62555-144">For this example, I created a shared storage account for hello package.zip toobe uploaded.</span></span> <span data-ttu-id="62555-145">hello skriptet använder AzCopy tooupload hello paketet toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="62555-145">hello script will leverage AzCopy tooupload hello package toohello storage account.</span></span> <span data-ttu-id="62555-146">Du skickar i din sökväg till artefakt och hello skriptet kommer automatiskt att överföra alla filer i den katalogen toohello med namnet lagringsbehållaren.</span><span class="sxs-lookup"><span data-stu-id="62555-146">You pass in your artifact folder location and hello script will automatically upload all files within that directory toohello named storage container.</span></span> <span data-ttu-id="62555-147">Efter att distribuera AzureResourceGroup.ps1 har toothen uppdatering hello SSL-bindningar toomap hello anpassade värdnamnet med SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="62555-147">After calling Deploy-AzureResourceGroup.ps1 you have toothen update hello SSL bindings toomap hello custom hostname with your SSL certificate.</span></span>

<span data-ttu-id="62555-148">följande PowerShell visar hello hello slutförd distribution anropa hello distribuera-AzureResourceGroup.ps1:</span><span class="sxs-lookup"><span data-stu-id="62555-148">hello following PowerShell shows hello complete deployment calling hello Deploy-AzureResourceGroup.ps1:</span></span>

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script toodeploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app toobind ssl certificate toohostname. This has toobe done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

<span data-ttu-id="62555-149">Nu ditt program bör har distribuerats, och du kan toobrowse tooit via https://www.yourcustomdomain.com</span><span class="sxs-lookup"><span data-stu-id="62555-149">At this point your application should have been deployed and you should be able toobrowse tooit via https://www.yourcustomdomain.com</span></span>

