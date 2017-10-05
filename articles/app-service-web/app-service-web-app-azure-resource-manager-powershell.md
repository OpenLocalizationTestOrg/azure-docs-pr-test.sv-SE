---
title: "Azure Resource Manager-baserade PowerShell-kommandon för Azure Web App | Microsoft Docs"
description: "Lär dig hur du använder de nya Azure Resource Manager-baserade PowerShell-kommandon för att hantera dina Azure-webbprogram."
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
ms.openlocfilehash: 8d574f051a327ba0409e6f25a5886af673d3d5e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Använder Azure Resource Manager-baserade PowerShell för att hantera Azure-Webbappar
> [!div class="op_single_selector"]
> * [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)
> 
> 

Med Microsoft Azure PowerShell version 1.0.0 nya kommandon lades till, som kan ge användaren möjlighet att använda Azure Resource Manager-baserade PowerShell-kommandon för att hantera Web Apps.

Läs om hur du hanterar resursgrupper i [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md). 

Läs om en fullständig lista över parametrar och alternativ för PowerShell-cmdlets i den [fullständig Cmdlet-referens för Web App Azure Resource Manager-baserade PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Hantera App Service-planer
### <a name="create-an-app-service-plan"></a>Skapa en Apptjänstplan
Använd för att skapa en apptjänstplan i **ny AzureRmAppServicePlan** cmdlet.

Nedan följer beskrivningar av olika parametrar:

* **Namnet**: namn på app service-plan.
* **Plats**: service plan plats.
* **ResourceGroupName**: resursgruppen som innehåller den nyligen skapade apptjänstplanen.
* **Nivån**: den önskade prisnivån (standard är ledig, andra alternativ är delade, Basic, Standard och Premium)
* **WorkerSize**: storleken på arbetare (standard är liten om nivå-parameter har angetts som Basic, Standard och Premium. Andra alternativ är medelstora och stora.)
* **NumberofWorkers**: antal arbetare i appen tjänsten plan (standardvärdet är 1). 

Exempel för att använda denna cmdlet:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Skapa en Apptjänstplan i en Apptjänst-miljö
Använd samma kommando för att skapa en apptjänstplan i en apptjänst-miljö **ny AzureRmAppServicePlan** med extra parametrar för att ange den ASE namn och en ASES resursgruppens namn.

Exempel för att använda denna cmdlet:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Mer information om apptjänstmiljö du [introduktion till Apptjänst-miljö](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Visa en lista med befintliga App Service-planer
Om du vill visa befintliga apptjänstplaner använder **Get-AzureRmAppServicePlan** cmdlet.

Om du vill visa en lista med alla apptjänstplaner under din prenumeration, använder du: 

    Get-AzureRmAppServicePlan

Om du vill visa en lista med alla apptjänstplaner under en viss resursgrupp, använder du:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

För att få en viss app service-plan, använder du:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Konfigurera en befintlig App Service-Plan
Om du vill ändra inställningarna för en befintlig app service-plan i **Set AzureRmAppServicePlan** cmdlet. Du kan ändra nivån, worker storlek och antalet arbetare 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Skala en Apptjänstplan
Om du vill skala en befintlig App Service-Plan, använder du:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Ändrar storlek på worker av en App Service-Plan
Om du vill ändra storlek på arbetare i en befintlig App Service-Plan, använder du:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>Ändra nivå i en Apptjänstplan
Om du vill ändra nivå i en befintlig App Service-Plan, använder du:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Ta bort en befintlig App Service-Plan
Om du vill ta bort en befintlig app service-plan måste alla tilldelade webbprogram som ska flyttas eller tas bort först. Med hjälp av den **ta bort AzureRmAppServicePlan** cmdlet som du kan ta bort app service-plan.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Hantera App Service Web Apps
### <a name="create-a-web-app"></a>Skapa en webbapp
Använd för att skapa en webbapp i **ny AzureRmWebApp** cmdlet.

Nedan följer beskrivningar av olika parametrar:

* **Namnet**: namn för webbappen.
* **AppServicePlan**: namn för service-plan som används som värd för webbprogrammet.
* **ResourceGroupName**: resursgruppen som är värd för App service-plan.
* **Plats**: app webbplats.

Exempel för att använda denna cmdlet:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Skapa en Webbapp i en Apptjänst-miljö
Att skapa en webbapp i en App Service miljö (ASE). Använda samma **ny AzureRmWebApp** kommando med extra parametrar för att ange ASE namnet och resursgruppens namn som namnet på ASE tillhör.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Mer information om apptjänstmiljö du [introduktion till Apptjänst-miljö](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Ta bort en befintlig Webbapp
Ta bort en befintlig webbapp som du kan använda den **ta bort AzureRmWebApp** cmdlet, måste du ange namnet på webbprogrammet och resursgruppens namn.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Visa en lista med befintliga Web Apps
Om du vill visa en lista med befintliga webbappar, Använd den **Get-AzureRmWebApp** cmdlet.

Om du vill visa en lista med alla webbprogram i din prenumeration, använder du:

    Get-AzureRmWebApp

Om du vill visa en lista med alla webbprogram i en viss resursgrupp, använder du:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

För att få ett specifikt webbprogram, använder du:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Konfigurera en befintlig Webbapp
Om du vill ändra inställningar och konfigurationer för en befintlig webbapp som använder den **Set AzureRmWebApp** cmdlet. En fullständig lista över parametrar, markera den [Cmdlet referenslänk](https://msdn.microsoft.com/library/mt652487.aspx)

Exempel (1): Använd denna cmdlet för att ändra anslutningssträngar

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Exempel (2): lägga till eller ändra app-inställningar

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Exempel (3): ange webbprogrammet för körning i 64-bitars läge

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>Ändra tillstånd för en befintlig Webbapp
#### <a name="restart-a-web-app"></a>Starta om ett webbprogram
Om du vill starta om en webbapp, måste du ange gruppen namn och en resurs i webbprogrammet.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Stoppa ett webbprogram
Om du vill stoppa en webbapp, måste du ange gruppen namn och en resurs i webbprogrammet.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Starta ett webbprogram
Om du vill starta en webbapp, måste du ange gruppen namn och en resurs i webbprogrammet.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Hantera webbpublicering App profiler
Varje webbprogram har en publiceringsprofil som kan användas för att publicera dina appar, flera åtgärder kan utföras på Publicera profiler.

#### <a name="get-publishing-profile"></a>Hämta Publicera profil
För att hämta publiceringsprofilen för en webbapp, använder du:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Det här kommandot ekon publiceringsprofilen till kommandoraden som också utdata publiceringsprofilen till en textfil.

#### <a name="reset-publishing-profile"></a>Återställ Publicera profil
Om du vill återställa både distribuera publishing lösenordet för FTP och webbtjänster för en webbapp, Använd:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Hantera certifikat för Web App
Läs om hur du hanterar web app certifikat i [SSL-certifikat-bindning med PowerShell](app-service-web-app-powershell-ssl-binding.md)

### <a name="next-steps"></a>Nästa steg
* Läs om Azure Resource Manager PowerShell-stöd i [med hjälp av Azure PowerShell med Azure Resource Manager.](../powershell-azure-resource-manager.md)
* Läs om Apptjänstmiljöer i [introduktion till Apptjänst-miljö.](app-service-app-service-environment-intro.md)
* Läs om hur du hanterar App Service SSL-certifikat med PowerShell i [SSL-certifikat-bindning med PowerShell.](app-service-web-app-powershell-ssl-binding.md)
* Mer information om en fullständig lista över Azure Resource Manager-baserade PowerShell-cmdlets för Azure Web Apps finns [Azure Cmdlet-referens för Web Apps Azure Resource Manager PowerShell-Cmdlets.](https://msdn.microsoft.com/library/mt619237.aspx)
* * Läs om hur du hanterar App Service med hjälp av CLI i [Using Azure Resource Manager-Based XPlat CLI för Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md)

