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
# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a><span data-ttu-id="a520b-103">Använder Azure Resource Manager-baserade PowerShell för att hantera Azure-Webbappar</span><span class="sxs-lookup"><span data-stu-id="a520b-103">Using Azure Resource Manager-Based PowerShell to Manage Azure Web Apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a520b-104">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a520b-104">Azure CLI</span></span>](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [<span data-ttu-id="a520b-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a520b-105">Azure PowerShell</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
> 
> 

<span data-ttu-id="a520b-106">Med Microsoft Azure PowerShell version 1.0.0 nya kommandon lades till, som kan ge användaren möjlighet att använda Azure Resource Manager-baserade PowerShell-kommandon för att hantera Web Apps.</span><span class="sxs-lookup"><span data-stu-id="a520b-106">With Microsoft Azure PowerShell version 1.0.0 new commands have been added, that give the user the ability to use Azure Resource Manager-based PowerShell commands to manage Web Apps.</span></span>

<span data-ttu-id="a520b-107">Läs om hur du hanterar resursgrupper i [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="a520b-107">To learn about managing Resource Groups, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span> 

<span data-ttu-id="a520b-108">Läs om en fullständig lista över parametrar och alternativ för PowerShell-cmdlets i den [fullständig Cmdlet-referens för Web App Azure Resource Manager-baserade PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt619237.aspx)</span><span class="sxs-lookup"><span data-stu-id="a520b-108">To learn about the full list of parameters and options for the PowerShell cmdlets, see the [full Cmdlet Reference of Web App Azure Resource Manager-based PowerShell Cmdlets](https://msdn.microsoft.com/library/mt619237.aspx)</span></span>

## <a name="managing-app-service-plans"></a><span data-ttu-id="a520b-109">Hantera App Service-planer</span><span class="sxs-lookup"><span data-stu-id="a520b-109">Managing App Service Plans</span></span>
### <a name="create-an-app-service-plan"></a><span data-ttu-id="a520b-110">Skapa en Apptjänstplan</span><span class="sxs-lookup"><span data-stu-id="a520b-110">Create an App Service Plan</span></span>
<span data-ttu-id="a520b-111">Använd för att skapa en apptjänstplan i **ny AzureRmAppServicePlan** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a520b-111">To create an app service plan, use the **New-AzureRmAppServicePlan** cmdlet.</span></span>

<span data-ttu-id="a520b-112">Nedan följer beskrivningar av olika parametrar:</span><span class="sxs-lookup"><span data-stu-id="a520b-112">Following are descriptions of the different parameters:</span></span>

* <span data-ttu-id="a520b-113">**Namnet**: namn på app service-plan.</span><span class="sxs-lookup"><span data-stu-id="a520b-113">**Name**: name of the app service plan.</span></span>
* <span data-ttu-id="a520b-114">**Plats**: service plan plats.</span><span class="sxs-lookup"><span data-stu-id="a520b-114">**Location**: service plan location.</span></span>
* <span data-ttu-id="a520b-115">**ResourceGroupName**: resursgruppen som innehåller den nyligen skapade apptjänstplanen.</span><span class="sxs-lookup"><span data-stu-id="a520b-115">**ResourceGroupName**: resource group that includes the newly created app service plan.</span></span>
* <span data-ttu-id="a520b-116">**Nivån**: den önskade prisnivån (standard är ledig, andra alternativ är delade, Basic, Standard och Premium)</span><span class="sxs-lookup"><span data-stu-id="a520b-116">**Tier**:  the desired pricing tier (Default is Free, other options are Shared, Basic, Standard, and Premium.)</span></span>
* <span data-ttu-id="a520b-117">**WorkerSize**: storleken på arbetare (standard är liten om nivå-parameter har angetts som Basic, Standard och Premium.</span><span class="sxs-lookup"><span data-stu-id="a520b-117">**WorkerSize**: the size of workers (Default is small if the Tier parameter was specified as Basic, Standard, or Premium.</span></span> <span data-ttu-id="a520b-118">Andra alternativ är medelstora och stora.)</span><span class="sxs-lookup"><span data-stu-id="a520b-118">Other options are Medium, and Large.)</span></span>
* <span data-ttu-id="a520b-119">**NumberofWorkers**: antal arbetare i appen tjänsten plan (standardvärdet är 1).</span><span class="sxs-lookup"><span data-stu-id="a520b-119">**NumberofWorkers**: the number of workers in the app service plan (Default value is 1).</span></span> 

<span data-ttu-id="a520b-120">Exempel för att använda denna cmdlet:</span><span class="sxs-lookup"><span data-stu-id="a520b-120">Example to use this cmdlet:</span></span>

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a><span data-ttu-id="a520b-121">Skapa en Apptjänstplan i en Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="a520b-121">Create an App Service Plan in an App Service Environment</span></span>
<span data-ttu-id="a520b-122">Använd samma kommando för att skapa en apptjänstplan i en apptjänst-miljö **ny AzureRmAppServicePlan** med extra parametrar för att ange den ASE namn och en ASES resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="a520b-122">To create an app service plan in an app service environment, use the same command **New-AzureRmAppServicePlan** command with extra parameters to specify the ASE's name and ASE's resource group name.</span></span>

<span data-ttu-id="a520b-123">Exempel för att använda denna cmdlet:</span><span class="sxs-lookup"><span data-stu-id="a520b-123">Example to use this cmdlet:</span></span>

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

<span data-ttu-id="a520b-124">Mer information om apptjänstmiljö du [introduktion till Apptjänst-miljö](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="a520b-124">To learn more about app service environment, check [Introduction to App Service Environment](app-service-app-service-environment-intro.md)</span></span>

### <a name="list-existing-app-service-plans"></a><span data-ttu-id="a520b-125">Visa en lista med befintliga App Service-planer</span><span class="sxs-lookup"><span data-stu-id="a520b-125">List Existing App Service Plans</span></span>
<span data-ttu-id="a520b-126">Om du vill visa befintliga apptjänstplaner använder **Get-AzureRmAppServicePlan** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a520b-126">To list the existing app service plans, use **Get-AzureRmAppServicePlan** cmdlet.</span></span>

<span data-ttu-id="a520b-127">Om du vill visa en lista med alla apptjänstplaner under din prenumeration, använder du:</span><span class="sxs-lookup"><span data-stu-id="a520b-127">To list all app service plans under your subscription, use:</span></span> 

    Get-AzureRmAppServicePlan

<span data-ttu-id="a520b-128">Om du vill visa en lista med alla apptjänstplaner under en viss resursgrupp, använder du:</span><span class="sxs-lookup"><span data-stu-id="a520b-128">To list all app service plans under a specific resource group, use:</span></span>

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

<span data-ttu-id="a520b-129">För att få en viss app service-plan, använder du:</span><span class="sxs-lookup"><span data-stu-id="a520b-129">To get a specific app service plan, use:</span></span>

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a><span data-ttu-id="a520b-130">Konfigurera en befintlig App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="a520b-130">Configure an existing App Service Plan</span></span>
<span data-ttu-id="a520b-131">Om du vill ändra inställningarna för en befintlig app service-plan i **Set AzureRmAppServicePlan** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a520b-131">To change the settings for an existing app service plan, use the **Set-AzureRmAppServicePlan** cmdlet.</span></span> <span data-ttu-id="a520b-132">Du kan ändra nivån, worker storlek och antalet arbetare</span><span class="sxs-lookup"><span data-stu-id="a520b-132">You can change the tier, worker size, and the number of workers</span></span> 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a><span data-ttu-id="a520b-133">Skala en Apptjänstplan</span><span class="sxs-lookup"><span data-stu-id="a520b-133">Scaling an App Service Plan</span></span>
<span data-ttu-id="a520b-134">Om du vill skala en befintlig App Service-Plan, använder du:</span><span class="sxs-lookup"><span data-stu-id="a520b-134">To scale an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a><span data-ttu-id="a520b-135">Ändrar storlek på worker av en App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="a520b-135">Changing the worker size of an App Service Plan</span></span>
<span data-ttu-id="a520b-136">Om du vill ändra storlek på arbetare i en befintlig App Service-Plan, använder du:</span><span class="sxs-lookup"><span data-stu-id="a520b-136">To change the size of workers in an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a><span data-ttu-id="a520b-137">Ändra nivå i en Apptjänstplan</span><span class="sxs-lookup"><span data-stu-id="a520b-137">Changing the Tier of an App Service Plan</span></span>
<span data-ttu-id="a520b-138">Om du vill ändra nivå i en befintlig App Service-Plan, använder du:</span><span class="sxs-lookup"><span data-stu-id="a520b-138">To change the tier of an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a><span data-ttu-id="a520b-139">Ta bort en befintlig App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="a520b-139">Delete an existing App Service Plan</span></span>
<span data-ttu-id="a520b-140">Om du vill ta bort en befintlig app service-plan måste alla tilldelade webbprogram som ska flyttas eller tas bort först.</span><span class="sxs-lookup"><span data-stu-id="a520b-140">To delete an existing app service plan, all assigned web apps need to be moved or deleted first.</span></span> <span data-ttu-id="a520b-141">Med hjälp av den **ta bort AzureRmAppServicePlan** cmdlet som du kan ta bort app service-plan.</span><span class="sxs-lookup"><span data-stu-id="a520b-141">Then using the **Remove-AzureRmAppServicePlan** cmdlet you can delete the app service plan.</span></span>

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a><span data-ttu-id="a520b-142">Hantera App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="a520b-142">Managing App Service Web Apps</span></span>
### <a name="create-a-web-app"></a><span data-ttu-id="a520b-143">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="a520b-143">Create a Web App</span></span>
<span data-ttu-id="a520b-144">Använd för att skapa en webbapp i **ny AzureRmWebApp** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a520b-144">To create a web app, use the **New-AzureRmWebApp** cmdlet.</span></span>

<span data-ttu-id="a520b-145">Nedan följer beskrivningar av olika parametrar:</span><span class="sxs-lookup"><span data-stu-id="a520b-145">Following are descriptions of the different parameters:</span></span>

* <span data-ttu-id="a520b-146">**Namnet**: namn för webbappen.</span><span class="sxs-lookup"><span data-stu-id="a520b-146">**Name**: name for the web app.</span></span>
* <span data-ttu-id="a520b-147">**AppServicePlan**: namn för service-plan som används som värd för webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a520b-147">**AppServicePlan**: name for the service plan used to host the web app.</span></span>
* <span data-ttu-id="a520b-148">**ResourceGroupName**: resursgruppen som är värd för App service-plan.</span><span class="sxs-lookup"><span data-stu-id="a520b-148">**ResourceGroupName**: resource group that hosts the App service plan.</span></span>
* <span data-ttu-id="a520b-149">**Plats**: app webbplats.</span><span class="sxs-lookup"><span data-stu-id="a520b-149">**Location**: the web app location.</span></span>

<span data-ttu-id="a520b-150">Exempel för att använda denna cmdlet:</span><span class="sxs-lookup"><span data-stu-id="a520b-150">Example to use this cmdlet:</span></span>

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a><span data-ttu-id="a520b-151">Skapa en Webbapp i en Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="a520b-151">Create a Web App in an App Service Environment</span></span>
<span data-ttu-id="a520b-152">Att skapa en webbapp i en App Service miljö (ASE).</span><span class="sxs-lookup"><span data-stu-id="a520b-152">To create a web app in an App Service Environment (ASE).</span></span> <span data-ttu-id="a520b-153">Använda samma **ny AzureRmWebApp** kommando med extra parametrar för att ange ASE namnet och resursgruppens namn som namnet på ASE tillhör.</span><span class="sxs-lookup"><span data-stu-id="a520b-153">Use the same **New-AzureRmWebApp** command with extra parameters to specify the ASE name and the resource group name that the ASE belongs to.</span></span>

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

<span data-ttu-id="a520b-154">Mer information om apptjänstmiljö du [introduktion till Apptjänst-miljö](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="a520b-154">To learn more about app service environment, check [Introduction to App Service Environment](app-service-app-service-environment-intro.md)</span></span>

### <a name="delete-an-existing-web-app"></a><span data-ttu-id="a520b-155">Ta bort en befintlig Webbapp</span><span class="sxs-lookup"><span data-stu-id="a520b-155">Delete an existing Web App</span></span>
<span data-ttu-id="a520b-156">Ta bort en befintlig webbapp som du kan använda den **ta bort AzureRmWebApp** cmdlet, måste du ange namnet på webbprogrammet och resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="a520b-156">To delete an existing web app you can use the **Remove-AzureRmWebApp** cmdlet, you need to specify the name of the web app and the resource group name.</span></span>

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a><span data-ttu-id="a520b-157">Visa en lista med befintliga Web Apps</span><span class="sxs-lookup"><span data-stu-id="a520b-157">List existing Web Apps</span></span>
<span data-ttu-id="a520b-158">Om du vill visa en lista med befintliga webbappar, Använd den **Get-AzureRmWebApp** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a520b-158">To list the existing web apps, use the **Get-AzureRmWebApp** cmdlet.</span></span>

<span data-ttu-id="a520b-159">Om du vill visa en lista med alla webbprogram i din prenumeration, använder du:</span><span class="sxs-lookup"><span data-stu-id="a520b-159">To list all web apps under your subscription, use:</span></span>

    Get-AzureRmWebApp

<span data-ttu-id="a520b-160">Om du vill visa en lista med alla webbprogram i en viss resursgrupp, använder du:</span><span class="sxs-lookup"><span data-stu-id="a520b-160">To list all web apps under a specific resource group, use:</span></span>

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

<span data-ttu-id="a520b-161">För att få ett specifikt webbprogram, använder du:</span><span class="sxs-lookup"><span data-stu-id="a520b-161">To get a specific web app, use:</span></span>

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a><span data-ttu-id="a520b-162">Konfigurera en befintlig Webbapp</span><span class="sxs-lookup"><span data-stu-id="a520b-162">Configure an existing Web App</span></span>
<span data-ttu-id="a520b-163">Om du vill ändra inställningar och konfigurationer för en befintlig webbapp som använder den **Set AzureRmWebApp** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a520b-163">To change the settings and configurations for an existing web app, use the **Set-AzureRmWebApp** cmdlet.</span></span> <span data-ttu-id="a520b-164">En fullständig lista över parametrar, markera den [Cmdlet referenslänk](https://msdn.microsoft.com/library/mt652487.aspx)</span><span class="sxs-lookup"><span data-stu-id="a520b-164">For a full list of parameters, check the [Cmdlet reference link](https://msdn.microsoft.com/library/mt652487.aspx)</span></span>

<span data-ttu-id="a520b-165">Exempel (1): Använd denna cmdlet för att ändra anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="a520b-165">Example (1): use this cmdlet to change connection strings</span></span>

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

<span data-ttu-id="a520b-166">Exempel (2): lägga till eller ändra app-inställningar</span><span class="sxs-lookup"><span data-stu-id="a520b-166">Example (2): add or change app settings</span></span>

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


<span data-ttu-id="a520b-167">Exempel (3): ange webbprogrammet för körning i 64-bitars läge</span><span class="sxs-lookup"><span data-stu-id="a520b-167">Example (3):  set the web app to run in 64-bit mode</span></span>

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a><span data-ttu-id="a520b-168">Ändra tillstånd för en befintlig Webbapp</span><span class="sxs-lookup"><span data-stu-id="a520b-168">Change the state of an existing Web App</span></span>
#### <a name="restart-a-web-app"></a><span data-ttu-id="a520b-169">Starta om ett webbprogram</span><span class="sxs-lookup"><span data-stu-id="a520b-169">Restart a web app</span></span>
<span data-ttu-id="a520b-170">Om du vill starta om en webbapp, måste du ange gruppen namn och en resurs i webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a520b-170">To restart a web app, you must specify the name and resource group of the web app.</span></span>

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a><span data-ttu-id="a520b-171">Stoppa ett webbprogram</span><span class="sxs-lookup"><span data-stu-id="a520b-171">Stop a web app</span></span>
<span data-ttu-id="a520b-172">Om du vill stoppa en webbapp, måste du ange gruppen namn och en resurs i webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a520b-172">To stop a web app, you must specify the name and resource group of the web app.</span></span>

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a><span data-ttu-id="a520b-173">Starta ett webbprogram</span><span class="sxs-lookup"><span data-stu-id="a520b-173">Start a web app</span></span>
<span data-ttu-id="a520b-174">Om du vill starta en webbapp, måste du ange gruppen namn och en resurs i webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a520b-174">To start a web app, you must specify the name and resource group of the web app.</span></span>

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a><span data-ttu-id="a520b-175">Hantera webbpublicering App profiler</span><span class="sxs-lookup"><span data-stu-id="a520b-175">Manage Web App Publishing profiles</span></span>
<span data-ttu-id="a520b-176">Varje webbprogram har en publiceringsprofil som kan användas för att publicera dina appar, flera åtgärder kan utföras på Publicera profiler.</span><span class="sxs-lookup"><span data-stu-id="a520b-176">Each web app has a publishing profile that can be used to publish your apps, several operations can be executed on publishing profiles.</span></span>

#### <a name="get-publishing-profile"></a><span data-ttu-id="a520b-177">Hämta Publicera profil</span><span class="sxs-lookup"><span data-stu-id="a520b-177">Get Publishing Profile</span></span>
<span data-ttu-id="a520b-178">För att hämta publiceringsprofilen för en webbapp, använder du:</span><span class="sxs-lookup"><span data-stu-id="a520b-178">To get the publishing profile for a web app, use:</span></span>

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

<span data-ttu-id="a520b-179">Det här kommandot ekon publiceringsprofilen till kommandoraden som också utdata publiceringsprofilen till en textfil.</span><span class="sxs-lookup"><span data-stu-id="a520b-179">This command echoes the publishing profile to the command line as well output the publishing profile to a text file.</span></span>

#### <a name="reset-publishing-profile"></a><span data-ttu-id="a520b-180">Återställ Publicera profil</span><span class="sxs-lookup"><span data-stu-id="a520b-180">Reset Publishing Profile</span></span>
<span data-ttu-id="a520b-181">Om du vill återställa både distribuera publishing lösenordet för FTP och webbtjänster för en webbapp, Använd:</span><span class="sxs-lookup"><span data-stu-id="a520b-181">To reset both the publishing password for FTP and web deploy for a web app, use:</span></span>

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a><span data-ttu-id="a520b-182">Hantera certifikat för Web App</span><span class="sxs-lookup"><span data-stu-id="a520b-182">Manage Web App Certificates</span></span>
<span data-ttu-id="a520b-183">Läs om hur du hanterar web app certifikat i [SSL-certifikat-bindning med PowerShell](app-service-web-app-powershell-ssl-binding.md)</span><span class="sxs-lookup"><span data-stu-id="a520b-183">To learn about how to manage web app certificates, see [SSL Certificates binding using PowerShell](app-service-web-app-powershell-ssl-binding.md)</span></span>

### <a name="next-steps"></a><span data-ttu-id="a520b-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a520b-184">Next Steps</span></span>
* <span data-ttu-id="a520b-185">Läs om Azure Resource Manager PowerShell-stöd i [med hjälp av Azure PowerShell med Azure Resource Manager.](../powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="a520b-185">To learn about Azure Resource Manager PowerShell support, see [Using Azure PowerShell with Azure Resource Manager.](../powershell-azure-resource-manager.md)</span></span>
* <span data-ttu-id="a520b-186">Läs om Apptjänstmiljöer i [introduktion till Apptjänst-miljö.](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="a520b-186">To learn about App Service Environments, see [Introduction to App Service Environment.](app-service-app-service-environment-intro.md)</span></span>
* <span data-ttu-id="a520b-187">Läs om hur du hanterar App Service SSL-certifikat med PowerShell i [SSL-certifikat-bindning med PowerShell.](app-service-web-app-powershell-ssl-binding.md)</span><span class="sxs-lookup"><span data-stu-id="a520b-187">To learn about managing App Service SSL certificates using PowerShell, see [SSL Certificates binding using PowerShell.](app-service-web-app-powershell-ssl-binding.md)</span></span>
* <span data-ttu-id="a520b-188">Mer information om en fullständig lista över Azure Resource Manager-baserade PowerShell-cmdlets för Azure Web Apps finns [Azure Cmdlet-referens för Web Apps Azure Resource Manager PowerShell-Cmdlets.](https://msdn.microsoft.com/library/mt619237.aspx)</span><span class="sxs-lookup"><span data-stu-id="a520b-188">To learn about the full list of Azure Resource Manager-based PowerShell cmdlets for Azure Web Apps, see [Azure Cmdlet Reference of Web Apps Azure Resource Manager PowerShell Cmdlets.](https://msdn.microsoft.com/library/mt619237.aspx)</span></span>
* * <span data-ttu-id="a520b-189">Läs om hur du hanterar App Service med hjälp av CLI i [Using Azure Resource Manager-Based XPlat CLI för Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md)</span><span class="sxs-lookup"><span data-stu-id="a520b-189">To learn about managing App Service using CLI, see [Using Azure Resource Manager-Based XPlat CLI for Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md)</span></span>

