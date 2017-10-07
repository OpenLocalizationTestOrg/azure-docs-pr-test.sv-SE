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
# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>Distribuera en webbapp med MSDeploy, anpassade värdnamnet och SSL-certifikat
Den här guiden går igenom skapar en distribution för slutpunkt till slutpunkt för en Azure-Webbapp, utnyttja MSDeploy såväl som att lägga till ett anpassat värdnamn och en SSL-certifikat toohello ARM-mall.

Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).

### <a name="create-sample-application"></a>Skapa exempelprogrammet
Du ska distribuera ett ASP.NET-webbprogram. hello första steget är toocreate en enkel webbapp (eller kan du välja toouse till en befintlig, i så fall kan du hoppa över det här steget).

Öppna Visual Studio 2015 och välj Arkiv > Nytt projekt. På hello dialogrutan som visas väljer du webb > ASP.NET-webbprogram. Välj webbprogram och välj hello MVC-mallen under mallar. Välj *ändra autentiseringstyp* för*ingen autentisering*. Detta är bara toomake hello exempelprogrammet så enkel som möjligt.

Nu har du en grundläggande ASP.Net web app redo toouse som en del av distributionsprocessen.

### <a name="create-msdeploy-package"></a>Skapa MSDeploy-paket
Nästa steg är toocreate hello paketet toodeploy hello web app tooAzure. toodo, spara projektet och sedan köra hello följande från hello kommandorad:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

Detta skapar en komprimerade paketet hello anges i PackageLocation i mappen. hello program är nu redo toobe har distribuerats, som du kan nu skapa ut toodo en Azure Resource Manager-mall som.

### <a name="create-arm-template"></a>Skapa ARM-mall
Först måste börja med en grundläggande ARM-mall som skapar ett webbprogram och en plan för värd (Observera att parametrar och variabler som används inte visas planeringsaspekter).

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

Därefter måste toomodify hello web app resurs tootake en kapslad MSDeploy-resurs. Detta tillåter du tooreference hello paketet skapade tidigare och tala om för Azure Resource Manager toouse MSDeploy toodeploy hello paketet toohello Azure WebApp. hello följande visar hello Microsoft.Web/sites resursen med hello kapslade MSDeploy resurs:

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

Nu kommer du att märka att hello MSDeploy resurs tar en **packageUri** -egenskap som definieras på följande sätt:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

Detta **packageUri** tar hello lagringskonto-uri som pekar toohello storage-konto där du kommer att överföra din zip paketet till. hello Azure Resource Manager använder [signaturer för delad åtkomst](../storage/common/storage-dotnet-shared-access-signature-part-1.md) toopull hello paketet ned lokalt från hello storage-konto när du distribuerar hello mallen. Den här processen automatiskt via ett PowerShell-skript som överföra hello-paket och anropa hello Azure Management API toocreate hello nycklar som krävs och skicka dem till hello mall som parametrar (*_artifactsLocation* och *_artifactsLocationSasToken*). Du behöver toodefine parametrar för hello mapp och filnamn hello paketet är överförda toounder hello lagringsbehållaren.

Du måste sedan tooadd i en annan kapslad resurs toosetup hello hostname bindningar tooleverage anpassade domäner. Du kommer första tooensure behovet av att du äger hello värdnamn och konfigurera toobe verifieras av Azure att du äger den - se [konfigurera ett anpassat domännamn i Azure App Service](app-service-web-tutorial-custom-domain.md). När det är klart kan du lägga till hello följande tooyour mallen under avsnittet för hello Microsoft.Web/sites resursgrupp:

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

Slutligen måste tooadd en annan toppnivåresurs, Microsoft.Web/certificates. Den här resursen innehåller SSL-certifikat och kommer att finnas på samma nivå som ditt webbprogram och värd planera hello.

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

Du behöver toohave ett giltigt SSL-certifikat i ordning tooset upp den här resursen. När du har det giltigt certifikatet måste tooextract hello pfx byte som en base64-sträng. Ett alternativ tooextract är toouse hello följande PowerShell-kommando:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

Du kan sedan överföra detta som en parameter tooyour ARM Distributionsmall.

Hello ARM-mallen är nu klar.

### <a name="deploy-template"></a>Distribuera mallen
Hej sista stegen är toopiece detta alla tillsammans i en fullständig slutpunkt till slutpunkt-distribution. toomake distribution enklare kan du använda hello **distribuera AzureResourceGroup.ps1** PowerShell-skript som läggs till när du skapar en Azure-resursgrupp-projekt i Visual Studio toohelp med överföra alla artefakter som krävs i hello mall. Det krävs toohave skapat ett lagringskonto som du vill toouse i förväg. Jag har skapat ett konto för delad lagring för hello package.zip toobe upp i det här exemplet. hello skriptet använder AzCopy tooupload hello paketet toohello storage-konto. Du skickar i din sökväg till artefakt och hello skriptet kommer automatiskt att överföra alla filer i den katalogen toohello med namnet lagringsbehållaren. Efter att distribuera AzureResourceGroup.ps1 har toothen uppdatering hello SSL-bindningar toomap hello anpassade värdnamnet med SSL-certifikat.

följande PowerShell visar hello hello slutförd distribution anropa hello distribuera-AzureResourceGroup.ps1:

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

Nu ditt program bör har distribuerats, och du kan toobrowse tooit via https://www.yourcustomdomain.com

