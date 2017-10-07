---
title: "aaaAzure Resource Manager-baserade PowerShell-kommandon för Azure Web App | Microsoft Docs"
description: "Lär dig hur hello toouse nya Azure Resource Manager-baserade PowerShell-kommandon toomanage Azure Web Apps."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 4231fbba-61e5-4f92-b958-531c066fb87f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: aelnably
ms.openlocfilehash: bbb821e89daa315280436e84e11316217bb644d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-based-powershell-toomanage-azure-web-apps"></a>Med hjälp av PowerShell Azure Resource Manager-Based tooManage Azure Web Apps
> [!div class="op_single_selector"]
> * [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)
> 
> 

Med Microsoft Azure PowerShell version 1.0.0 nya kommandon lades till, som ger hello användaren hello möjlighet toouse Azure Resource Manager-baserade PowerShell-kommandon toomanage Web Apps.

toolearn om hur du hanterar resursgrupper, se [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md). 

toolearn om hello fullständig lista över parametrar och alternativ för hello PowerShell-cmdlets finns hello [fullständig Cmdlet-referens för Web App Azure Resource Manager-baserade PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Hantera App Service-planer
### <a name="create-an-app-service-plan"></a>Skapa en Apptjänstplan
toocreate en apptjänstplan använda hello **ny AzureRmAppServicePlan** cmdlet.

Nedan följer beskrivningar av hello olika parametrar:

* **Namnet**: namn på hello app service-plan.
* **Plats**: service plan plats.
* **ResourceGroupName**: resursgruppen som innehåller hello nyskapad app service-plan.
* **Nivån**: hello önskade prisnivån (standard är ledig, andra alternativ är delade, Basic, Standard och Premium)
* **WorkerSize**: hello storleken på arbetare (standard är liten om hello nivå angavs som Basic, Standard och Premium. Andra alternativ är medelstora och stora.)
* **NumberofWorkers**: hello antalet arbetare i hello apptjänstplan (standardvärdet är 1). 

Exempel toouse denna cmdlet:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Skapa en Apptjänstplan i en Apptjänst-miljö
toocreate en app service-plan i en apptjänst-miljö, Använd hello samma kommando **ny AzureRmAppServicePlan** med extra parametrar toospecify hello ASES namn och ASES resursgruppens namn.

Exempel toouse denna cmdlet:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Mer om apptjänst-miljön, kontrollera toolearn [introduktion tooApp-miljö](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Visa en lista med befintliga App Service-planer
toolist hello befintliga apptjänstplaner, Använd **Get-AzureRmAppServicePlan** cmdlet.

toolist alla apptjänstplaner under din prenumeration, Använd: 

    Get-AzureRmAppServicePlan

toolist alla apptjänstplaner under en viss resursgrupp, Använd:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

tooget en viss app service-plan, Använd:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Konfigurera en befintlig App Service-Plan
toochange hello inställningar för en befintlig app service-plan, använda hello **Set AzureRmAppServicePlan** cmdlet. Du kan ändra hello nivå, worker storlek och hello antalet arbetare 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Skala en Apptjänstplan
Använd en befintlig App Service-Plan, tooscale:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-hello-worker-size-of-an-app-service-plan"></a>Ändra hello worker storlek på en App Service-Plan
toochange hello storlek för arbetare i en befintlig App Service-Plan, Använd:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-hello-tier-of-an-app-service-plan"></a>Ändra hello nivå i en Apptjänstplan
toochange hello nivå i en befintlig App Service-Plan, Använd:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Ta bort en befintlig App Service-Plan
toodelete en befintlig app service-plan alla tilldelade web apps behöver toobe flyttat eller tagit bort först. Med hello **ta bort AzureRmAppServicePlan** cmdlet som du kan ta bort hello app service-plan.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Hantera App Service Web Apps
### <a name="create-a-web-app"></a>Skapa en webbapp
toocreate en webbapp använder hello **ny AzureRmWebApp** cmdlet.

Nedan följer beskrivningar av hello olika parametrar:

* **Namnet**: namn för hello webbprogrammet.
* **AppServicePlan**: namn för hello serviceplan toohost hello-webbprogram.
* **ResourceGroupName**: resursgruppen som är värd för hello App service-plan.
* **Plats**: hello app webbplats.

Exempel toouse denna cmdlet:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Skapa en Webbapp i en Apptjänst-miljö
toocreate ett webbprogram i en App Service miljö (ASE). Använd hello samma **ny AzureRmWebApp** med extra parametrar toospecify hello ASE namn och hello resursgruppens namn som hello ASE tillhör.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Mer om apptjänst-miljön, kontrollera toolearn [introduktion tooApp-miljö](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Ta bort en befintlig Webbapp
toodelete en befintlig webbapp som du kan använda hello **ta bort AzureRmWebApp** cmdlet, behöver du toospecify hello namnet på webbprogrammet hello och hello resursgruppens namn.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Visa en lista med befintliga Web Apps
toolist hello befintliga web apps använda hello **Get-AzureRmWebApp** cmdlet.

toolist Använd för alla webbprogram i din prenumeration:

    Get-AzureRmWebApp

toolist Använd för alla webbprogram i en viss resursgrupp:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Använd tooget ett specifikt webbprogram:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Konfigurera en befintlig Webbapp
toochange hello inställningar och konfigurationer för en befintlig webbapp som använder hello **Set AzureRmWebApp** cmdlet. En fullständig lista över parametrar finns hello [Cmdlet referenslänk](https://msdn.microsoft.com/library/mt652487.aspx)

Exempel (1): Använd den här cmdlet toochange anslutningssträngar

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Exempel (2): lägga till eller ändra app-inställningar

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Exempel (3): Ange hello web app toorun i 64-bitarsläge

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-hello-state-of-an-existing-web-app"></a>Ändra hello tillståndet för en befintlig Webbapp
#### <a name="restart-a-web-app"></a>Starta om ett webbprogram
Du måste ange hello namn och resurs grupp hello webbprogrammet toorestart ett webbprogram.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Stoppa ett webbprogram
Du måste ange hello namn och resurs grupp hello webbprogrammet toostop ett webbprogram.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Starta ett webbprogram
Du måste ange hello namn och resurs grupp hello webbprogrammet toostart ett webbprogram.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Hantera webbpublicering App profiler
Varje webbprogram har en publiceringsprofil som kan använda toopublish dina appar, flera åtgärder kan utföras på Publicera profiler.

#### <a name="get-publishing-profile"></a>Hämta Publicera profil
tooget hello Publicera profil för en webbapp, Använd:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Det här kommandot ekon hello Publicera profil toohello kommandoraden samt utdata hello Publicera profil tooa textfil.

#### <a name="reset-publishing-profile"></a>Återställ Publicera profil
tooreset både hello publishing lösenord för FTP- och web distribuera för en webbapp, Använd:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Hantera certifikat för Web App
toolearn om hur toomanage web app certifikat finns [SSL-certifikat-bindning med PowerShell](app-service-web-app-powershell-ssl-binding.md)

### <a name="next-steps"></a>Nästa steg
* toolearn om Azure Resource Manager PowerShell-stöd finns [med hjälp av Azure PowerShell med Azure Resource Manager.](../powershell-azure-resource-manager.md)
* toolearn om Apptjänstmiljöer, se [introduktion tooApp-miljö.](app-service-app-service-environment-intro.md)
* toolearn om hur du hanterar App Service SSL-certifikat med hjälp av PowerShell, se [SSL-certifikat-bindning med PowerShell.](app-service-web-app-powershell-ssl-binding.md)
* toolearn om hello fullständig lista över Azure Resource Manager-baserade PowerShell-cmdlets för Azure Web Apps finns [Azure Cmdlet-referens för Web Apps Azure Resource Manager PowerShell-Cmdlets.](https://msdn.microsoft.com/library/mt619237.aspx)
* * toolearn om hur du hanterar App Service med hjälp av CLI, se [Using Azure Resource Manager-Based XPlat CLI för Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md)

