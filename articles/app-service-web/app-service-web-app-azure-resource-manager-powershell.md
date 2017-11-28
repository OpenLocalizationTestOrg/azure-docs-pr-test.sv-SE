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
# <a name="using-azure-resource-manager-based-powershell-toomanage-azure-web-apps"></a><span data-ttu-id="b1834-103">Med hjälp av PowerShell Azure Resource Manager-Based tooManage Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="b1834-103">Using Azure Resource Manager-Based PowerShell tooManage Azure Web Apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b1834-104">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b1834-104">Azure CLI</span></span>](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [<span data-ttu-id="b1834-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b1834-105">Azure PowerShell</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
> 
> 

<span data-ttu-id="b1834-106">Med Microsoft Azure PowerShell version 1.0.0 nya kommandon lades till, som ger hello användaren hello möjlighet toouse Azure Resource Manager-baserade PowerShell-kommandon toomanage Web Apps.</span><span class="sxs-lookup"><span data-stu-id="b1834-106">With Microsoft Azure PowerShell version 1.0.0 new commands have been added, that give hello user hello ability toouse Azure Resource Manager-based PowerShell commands toomanage Web Apps.</span></span>

<span data-ttu-id="b1834-107">toolearn om hur du hanterar resursgrupper, se [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b1834-107">toolearn about managing Resource Groups, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span> 

<span data-ttu-id="b1834-108">toolearn om hello fullständig lista över parametrar och alternativ för hello PowerShell-cmdlets finns hello [fullständig Cmdlet-referens för Web App Azure Resource Manager-baserade PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt619237.aspx)</span><span class="sxs-lookup"><span data-stu-id="b1834-108">toolearn about hello full list of parameters and options for hello PowerShell cmdlets, see hello [full Cmdlet Reference of Web App Azure Resource Manager-based PowerShell Cmdlets](https://msdn.microsoft.com/library/mt619237.aspx)</span></span>

## <a name="managing-app-service-plans"></a><span data-ttu-id="b1834-109">Hantera App Service-planer</span><span class="sxs-lookup"><span data-stu-id="b1834-109">Managing App Service Plans</span></span>
### <a name="create-an-app-service-plan"></a><span data-ttu-id="b1834-110">Skapa en Apptjänstplan</span><span class="sxs-lookup"><span data-stu-id="b1834-110">Create an App Service Plan</span></span>
<span data-ttu-id="b1834-111">toocreate en apptjänstplan använda hello **ny AzureRmAppServicePlan** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b1834-111">toocreate an app service plan, use hello **New-AzureRmAppServicePlan** cmdlet.</span></span>

<span data-ttu-id="b1834-112">Nedan följer beskrivningar av hello olika parametrar:</span><span class="sxs-lookup"><span data-stu-id="b1834-112">Following are descriptions of hello different parameters:</span></span>

* <span data-ttu-id="b1834-113">**Namnet**: namn på hello app service-plan.</span><span class="sxs-lookup"><span data-stu-id="b1834-113">**Name**: name of hello app service plan.</span></span>
* <span data-ttu-id="b1834-114">**Plats**: service plan plats.</span><span class="sxs-lookup"><span data-stu-id="b1834-114">**Location**: service plan location.</span></span>
* <span data-ttu-id="b1834-115">**ResourceGroupName**: resursgruppen som innehåller hello nyskapad app service-plan.</span><span class="sxs-lookup"><span data-stu-id="b1834-115">**ResourceGroupName**: resource group that includes hello newly created app service plan.</span></span>
* <span data-ttu-id="b1834-116">**Nivån**: hello önskade prisnivån (standard är ledig, andra alternativ är delade, Basic, Standard och Premium)</span><span class="sxs-lookup"><span data-stu-id="b1834-116">**Tier**:  hello desired pricing tier (Default is Free, other options are Shared, Basic, Standard, and Premium.)</span></span>
* <span data-ttu-id="b1834-117">**WorkerSize**: hello storleken på arbetare (standard är liten om hello nivå angavs som Basic, Standard och Premium.</span><span class="sxs-lookup"><span data-stu-id="b1834-117">**WorkerSize**: hello size of workers (Default is small if hello Tier parameter was specified as Basic, Standard, or Premium.</span></span> <span data-ttu-id="b1834-118">Andra alternativ är medelstora och stora.)</span><span class="sxs-lookup"><span data-stu-id="b1834-118">Other options are Medium, and Large.)</span></span>
* <span data-ttu-id="b1834-119">**NumberofWorkers**: hello antalet arbetare i hello apptjänstplan (standardvärdet är 1).</span><span class="sxs-lookup"><span data-stu-id="b1834-119">**NumberofWorkers**: hello number of workers in hello app service plan (Default value is 1).</span></span> 

<span data-ttu-id="b1834-120">Exempel toouse denna cmdlet:</span><span class="sxs-lookup"><span data-stu-id="b1834-120">Example toouse this cmdlet:</span></span>

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a><span data-ttu-id="b1834-121">Skapa en Apptjänstplan i en Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="b1834-121">Create an App Service Plan in an App Service Environment</span></span>
<span data-ttu-id="b1834-122">toocreate en app service-plan i en apptjänst-miljö, Använd hello samma kommando **ny AzureRmAppServicePlan** med extra parametrar toospecify hello ASES namn och ASES resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="b1834-122">toocreate an app service plan in an app service environment, use hello same command **New-AzureRmAppServicePlan** command with extra parameters toospecify hello ASE's name and ASE's resource group name.</span></span>

<span data-ttu-id="b1834-123">Exempel toouse denna cmdlet:</span><span class="sxs-lookup"><span data-stu-id="b1834-123">Example toouse this cmdlet:</span></span>

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

<span data-ttu-id="b1834-124">Mer om apptjänst-miljön, kontrollera toolearn [introduktion tooApp-miljö](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="b1834-124">toolearn more about app service environment, check [Introduction tooApp Service Environment](app-service-app-service-environment-intro.md)</span></span>

### <a name="list-existing-app-service-plans"></a><span data-ttu-id="b1834-125">Visa en lista med befintliga App Service-planer</span><span class="sxs-lookup"><span data-stu-id="b1834-125">List Existing App Service Plans</span></span>
<span data-ttu-id="b1834-126">toolist hello befintliga apptjänstplaner, Använd **Get-AzureRmAppServicePlan** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b1834-126">toolist hello existing app service plans, use **Get-AzureRmAppServicePlan** cmdlet.</span></span>

<span data-ttu-id="b1834-127">toolist alla apptjänstplaner under din prenumeration, Använd:</span><span class="sxs-lookup"><span data-stu-id="b1834-127">toolist all app service plans under your subscription, use:</span></span> 

    Get-AzureRmAppServicePlan

<span data-ttu-id="b1834-128">toolist alla apptjänstplaner under en viss resursgrupp, Använd:</span><span class="sxs-lookup"><span data-stu-id="b1834-128">toolist all app service plans under a specific resource group, use:</span></span>

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

<span data-ttu-id="b1834-129">tooget en viss app service-plan, Använd:</span><span class="sxs-lookup"><span data-stu-id="b1834-129">tooget a specific app service plan, use:</span></span>

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a><span data-ttu-id="b1834-130">Konfigurera en befintlig App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="b1834-130">Configure an existing App Service Plan</span></span>
<span data-ttu-id="b1834-131">toochange hello inställningar för en befintlig app service-plan, använda hello **Set AzureRmAppServicePlan** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b1834-131">toochange hello settings for an existing app service plan, use hello **Set-AzureRmAppServicePlan** cmdlet.</span></span> <span data-ttu-id="b1834-132">Du kan ändra hello nivå, worker storlek och hello antalet arbetare</span><span class="sxs-lookup"><span data-stu-id="b1834-132">You can change hello tier, worker size, and hello number of workers</span></span> 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a><span data-ttu-id="b1834-133">Skala en Apptjänstplan</span><span class="sxs-lookup"><span data-stu-id="b1834-133">Scaling an App Service Plan</span></span>
<span data-ttu-id="b1834-134">Använd en befintlig App Service-Plan, tooscale:</span><span class="sxs-lookup"><span data-stu-id="b1834-134">tooscale an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-hello-worker-size-of-an-app-service-plan"></a><span data-ttu-id="b1834-135">Ändra hello worker storlek på en App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="b1834-135">Changing hello worker size of an App Service Plan</span></span>
<span data-ttu-id="b1834-136">toochange hello storlek för arbetare i en befintlig App Service-Plan, Använd:</span><span class="sxs-lookup"><span data-stu-id="b1834-136">toochange hello size of workers in an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-hello-tier-of-an-app-service-plan"></a><span data-ttu-id="b1834-137">Ändra hello nivå i en Apptjänstplan</span><span class="sxs-lookup"><span data-stu-id="b1834-137">Changing hello Tier of an App Service Plan</span></span>
<span data-ttu-id="b1834-138">toochange hello nivå i en befintlig App Service-Plan, Använd:</span><span class="sxs-lookup"><span data-stu-id="b1834-138">toochange hello tier of an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a><span data-ttu-id="b1834-139">Ta bort en befintlig App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="b1834-139">Delete an existing App Service Plan</span></span>
<span data-ttu-id="b1834-140">toodelete en befintlig app service-plan alla tilldelade web apps behöver toobe flyttat eller tagit bort först.</span><span class="sxs-lookup"><span data-stu-id="b1834-140">toodelete an existing app service plan, all assigned web apps need toobe moved or deleted first.</span></span> <span data-ttu-id="b1834-141">Med hello **ta bort AzureRmAppServicePlan** cmdlet som du kan ta bort hello app service-plan.</span><span class="sxs-lookup"><span data-stu-id="b1834-141">Then using hello **Remove-AzureRmAppServicePlan** cmdlet you can delete hello app service plan.</span></span>

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a><span data-ttu-id="b1834-142">Hantera App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="b1834-142">Managing App Service Web Apps</span></span>
### <a name="create-a-web-app"></a><span data-ttu-id="b1834-143">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="b1834-143">Create a Web App</span></span>
<span data-ttu-id="b1834-144">toocreate en webbapp använder hello **ny AzureRmWebApp** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b1834-144">toocreate a web app, use hello **New-AzureRmWebApp** cmdlet.</span></span>

<span data-ttu-id="b1834-145">Nedan följer beskrivningar av hello olika parametrar:</span><span class="sxs-lookup"><span data-stu-id="b1834-145">Following are descriptions of hello different parameters:</span></span>

* <span data-ttu-id="b1834-146">**Namnet**: namn för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="b1834-146">**Name**: name for hello web app.</span></span>
* <span data-ttu-id="b1834-147">**AppServicePlan**: namn för hello serviceplan toohost hello-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b1834-147">**AppServicePlan**: name for hello service plan used toohost hello web app.</span></span>
* <span data-ttu-id="b1834-148">**ResourceGroupName**: resursgruppen som är värd för hello App service-plan.</span><span class="sxs-lookup"><span data-stu-id="b1834-148">**ResourceGroupName**: resource group that hosts hello App service plan.</span></span>
* <span data-ttu-id="b1834-149">**Plats**: hello app webbplats.</span><span class="sxs-lookup"><span data-stu-id="b1834-149">**Location**: hello web app location.</span></span>

<span data-ttu-id="b1834-150">Exempel toouse denna cmdlet:</span><span class="sxs-lookup"><span data-stu-id="b1834-150">Example toouse this cmdlet:</span></span>

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a><span data-ttu-id="b1834-151">Skapa en Webbapp i en Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="b1834-151">Create a Web App in an App Service Environment</span></span>
<span data-ttu-id="b1834-152">toocreate ett webbprogram i en App Service miljö (ASE).</span><span class="sxs-lookup"><span data-stu-id="b1834-152">toocreate a web app in an App Service Environment (ASE).</span></span> <span data-ttu-id="b1834-153">Använd hello samma **ny AzureRmWebApp** med extra parametrar toospecify hello ASE namn och hello resursgruppens namn som hello ASE tillhör.</span><span class="sxs-lookup"><span data-stu-id="b1834-153">Use hello same **New-AzureRmWebApp** command with extra parameters toospecify hello ASE name and hello resource group name that hello ASE belongs to.</span></span>

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

<span data-ttu-id="b1834-154">Mer om apptjänst-miljön, kontrollera toolearn [introduktion tooApp-miljö](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="b1834-154">toolearn more about app service environment, check [Introduction tooApp Service Environment](app-service-app-service-environment-intro.md)</span></span>

### <a name="delete-an-existing-web-app"></a><span data-ttu-id="b1834-155">Ta bort en befintlig Webbapp</span><span class="sxs-lookup"><span data-stu-id="b1834-155">Delete an existing Web App</span></span>
<span data-ttu-id="b1834-156">toodelete en befintlig webbapp som du kan använda hello **ta bort AzureRmWebApp** cmdlet, behöver du toospecify hello namnet på webbprogrammet hello och hello resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="b1834-156">toodelete an existing web app you can use hello **Remove-AzureRmWebApp** cmdlet, you need toospecify hello name of hello web app and hello resource group name.</span></span>

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a><span data-ttu-id="b1834-157">Visa en lista med befintliga Web Apps</span><span class="sxs-lookup"><span data-stu-id="b1834-157">List existing Web Apps</span></span>
<span data-ttu-id="b1834-158">toolist hello befintliga web apps använda hello **Get-AzureRmWebApp** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b1834-158">toolist hello existing web apps, use hello **Get-AzureRmWebApp** cmdlet.</span></span>

<span data-ttu-id="b1834-159">toolist Använd för alla webbprogram i din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="b1834-159">toolist all web apps under your subscription, use:</span></span>

    Get-AzureRmWebApp

<span data-ttu-id="b1834-160">toolist Använd för alla webbprogram i en viss resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="b1834-160">toolist all web apps under a specific resource group, use:</span></span>

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

<span data-ttu-id="b1834-161">Använd tooget ett specifikt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="b1834-161">tooget a specific web app, use:</span></span>

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a><span data-ttu-id="b1834-162">Konfigurera en befintlig Webbapp</span><span class="sxs-lookup"><span data-stu-id="b1834-162">Configure an existing Web App</span></span>
<span data-ttu-id="b1834-163">toochange hello inställningar och konfigurationer för en befintlig webbapp som använder hello **Set AzureRmWebApp** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b1834-163">toochange hello settings and configurations for an existing web app, use hello **Set-AzureRmWebApp** cmdlet.</span></span> <span data-ttu-id="b1834-164">En fullständig lista över parametrar finns hello [Cmdlet referenslänk](https://msdn.microsoft.com/library/mt652487.aspx)</span><span class="sxs-lookup"><span data-stu-id="b1834-164">For a full list of parameters, check hello [Cmdlet reference link](https://msdn.microsoft.com/library/mt652487.aspx)</span></span>

<span data-ttu-id="b1834-165">Exempel (1): Använd den här cmdlet toochange anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="b1834-165">Example (1): use this cmdlet toochange connection strings</span></span>

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

<span data-ttu-id="b1834-166">Exempel (2): lägga till eller ändra app-inställningar</span><span class="sxs-lookup"><span data-stu-id="b1834-166">Example (2): add or change app settings</span></span>

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


<span data-ttu-id="b1834-167">Exempel (3): Ange hello web app toorun i 64-bitarsläge</span><span class="sxs-lookup"><span data-stu-id="b1834-167">Example (3):  set hello web app toorun in 64-bit mode</span></span>

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-hello-state-of-an-existing-web-app"></a><span data-ttu-id="b1834-168">Ändra hello tillståndet för en befintlig Webbapp</span><span class="sxs-lookup"><span data-stu-id="b1834-168">Change hello state of an existing Web App</span></span>
#### <a name="restart-a-web-app"></a><span data-ttu-id="b1834-169">Starta om ett webbprogram</span><span class="sxs-lookup"><span data-stu-id="b1834-169">Restart a web app</span></span>
<span data-ttu-id="b1834-170">Du måste ange hello namn och resurs grupp hello webbprogrammet toorestart ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b1834-170">toorestart a web app, you must specify hello name and resource group of hello web app.</span></span>

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a><span data-ttu-id="b1834-171">Stoppa ett webbprogram</span><span class="sxs-lookup"><span data-stu-id="b1834-171">Stop a web app</span></span>
<span data-ttu-id="b1834-172">Du måste ange hello namn och resurs grupp hello webbprogrammet toostop ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b1834-172">toostop a web app, you must specify hello name and resource group of hello web app.</span></span>

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a><span data-ttu-id="b1834-173">Starta ett webbprogram</span><span class="sxs-lookup"><span data-stu-id="b1834-173">Start a web app</span></span>
<span data-ttu-id="b1834-174">Du måste ange hello namn och resurs grupp hello webbprogrammet toostart ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b1834-174">toostart a web app, you must specify hello name and resource group of hello web app.</span></span>

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a><span data-ttu-id="b1834-175">Hantera webbpublicering App profiler</span><span class="sxs-lookup"><span data-stu-id="b1834-175">Manage Web App Publishing profiles</span></span>
<span data-ttu-id="b1834-176">Varje webbprogram har en publiceringsprofil som kan använda toopublish dina appar, flera åtgärder kan utföras på Publicera profiler.</span><span class="sxs-lookup"><span data-stu-id="b1834-176">Each web app has a publishing profile that can be used toopublish your apps, several operations can be executed on publishing profiles.</span></span>

#### <a name="get-publishing-profile"></a><span data-ttu-id="b1834-177">Hämta Publicera profil</span><span class="sxs-lookup"><span data-stu-id="b1834-177">Get Publishing Profile</span></span>
<span data-ttu-id="b1834-178">tooget hello Publicera profil för en webbapp, Använd:</span><span class="sxs-lookup"><span data-stu-id="b1834-178">tooget hello publishing profile for a web app, use:</span></span>

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

<span data-ttu-id="b1834-179">Det här kommandot ekon hello Publicera profil toohello kommandoraden samt utdata hello Publicera profil tooa textfil.</span><span class="sxs-lookup"><span data-stu-id="b1834-179">This command echoes hello publishing profile toohello command line as well output hello publishing profile tooa text file.</span></span>

#### <a name="reset-publishing-profile"></a><span data-ttu-id="b1834-180">Återställ Publicera profil</span><span class="sxs-lookup"><span data-stu-id="b1834-180">Reset Publishing Profile</span></span>
<span data-ttu-id="b1834-181">tooreset både hello publishing lösenord för FTP- och web distribuera för en webbapp, Använd:</span><span class="sxs-lookup"><span data-stu-id="b1834-181">tooreset both hello publishing password for FTP and web deploy for a web app, use:</span></span>

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a><span data-ttu-id="b1834-182">Hantera certifikat för Web App</span><span class="sxs-lookup"><span data-stu-id="b1834-182">Manage Web App Certificates</span></span>
<span data-ttu-id="b1834-183">toolearn om hur toomanage web app certifikat finns [SSL-certifikat-bindning med PowerShell](app-service-web-app-powershell-ssl-binding.md)</span><span class="sxs-lookup"><span data-stu-id="b1834-183">toolearn about how toomanage web app certificates, see [SSL Certificates binding using PowerShell](app-service-web-app-powershell-ssl-binding.md)</span></span>

### <a name="next-steps"></a><span data-ttu-id="b1834-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b1834-184">Next Steps</span></span>
* <span data-ttu-id="b1834-185">toolearn om Azure Resource Manager PowerShell-stöd finns [med hjälp av Azure PowerShell med Azure Resource Manager.](../powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="b1834-185">toolearn about Azure Resource Manager PowerShell support, see [Using Azure PowerShell with Azure Resource Manager.](../powershell-azure-resource-manager.md)</span></span>
* <span data-ttu-id="b1834-186">toolearn om Apptjänstmiljöer, se [introduktion tooApp-miljö.](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="b1834-186">toolearn about App Service Environments, see [Introduction tooApp Service Environment.](app-service-app-service-environment-intro.md)</span></span>
* <span data-ttu-id="b1834-187">toolearn om hur du hanterar App Service SSL-certifikat med hjälp av PowerShell, se [SSL-certifikat-bindning med PowerShell.](app-service-web-app-powershell-ssl-binding.md)</span><span class="sxs-lookup"><span data-stu-id="b1834-187">toolearn about managing App Service SSL certificates using PowerShell, see [SSL Certificates binding using PowerShell.](app-service-web-app-powershell-ssl-binding.md)</span></span>
* <span data-ttu-id="b1834-188">toolearn om hello fullständig lista över Azure Resource Manager-baserade PowerShell-cmdlets för Azure Web Apps finns [Azure Cmdlet-referens för Web Apps Azure Resource Manager PowerShell-Cmdlets.](https://msdn.microsoft.com/library/mt619237.aspx)</span><span class="sxs-lookup"><span data-stu-id="b1834-188">toolearn about hello full list of Azure Resource Manager-based PowerShell cmdlets for Azure Web Apps, see [Azure Cmdlet Reference of Web Apps Azure Resource Manager PowerShell Cmdlets.](https://msdn.microsoft.com/library/mt619237.aspx)</span></span>
* * <span data-ttu-id="b1834-189">toolearn om hur du hanterar App Service med hjälp av CLI, se [Using Azure Resource Manager-Based XPlat CLI för Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md)</span><span class="sxs-lookup"><span data-stu-id="b1834-189">toolearn about managing App Service using CLI, see [Using Azure Resource Manager-Based XPlat CLI for Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md)</span></span>

