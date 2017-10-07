---
title: "aaaAzure Resource Manager-baserade plattformar kommandoradsverktyg för Azure Web App | Microsoft Docs"
description: "Lär dig hur hello toouse nya Azure Resource Manager-baserade plattformar kommandoradsverktyg toomanage Azure Web Apps."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: d415b195-4262-416f-b59f-7e1aef200054
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: aelnably
ms.openlocfilehash: 5f5e03edcb01154aef3bd220cd27358f69656ef4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-app-service"></a><span data-ttu-id="db8e4-103">Med hjälp av Azure Resource Manager-baserade XPlat CLI för Azure App Service</span><span class="sxs-lookup"><span data-stu-id="db8e4-103">Using Azure Resource Manager-Based XPlat CLI for Azure App Service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="db8e4-104">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="db8e4-104">Azure CLI</span></span>](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [<span data-ttu-id="db8e4-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="db8e4-105">Azure PowerShell</span></span>](app-service-web-app-azure-resource-manager-powershell.md)

<span data-ttu-id="db8e4-106">Nya kommandon har lagts till med hello-versionen av Microsoft Azure plattformsoberoende kommandoradsverktyg version 0.10.5.</span><span class="sxs-lookup"><span data-stu-id="db8e4-106">With hello release of Microsoft Azure Cross-platform Command-Line Tools version 0.10.5, new commands have been added.</span></span> <span data-ttu-id="db8e4-107">De här kommandona ger hello användaren hello möjlighet toouse Azure Resource Manager-baserade PowerShell-kommandon toomanage Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="db8e4-107">These commands give hello user hello ability toouse Azure Resource Manager-based PowerShell commands toomanage App Service.</span></span>

<span data-ttu-id="db8e4-108">toolearn om hur du hanterar resursgrupper, se [använda hello Azure CLI toomanage Azure resurser och resursgrupper](../azure-resource-manager/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="db8e4-108">toolearn about managing Resource Groups, see [Use hello Azure CLI toomanage Azure resources and resource groups](../azure-resource-manager/xplat-cli-azure-resource-manager.md).</span></span> 

> [!NOTE] 
> <span data-ttu-id="db8e4-109">Prova också [Azure CLI 2.0](https://github.com/Azure/azure-cli), en nästa generationens CLI som skrivits i Python för hello resursdistributionsmodell för hantering.</span><span class="sxs-lookup"><span data-stu-id="db8e4-109">Also, try out [Azure CLI 2.0](https://github.com/Azure/azure-cli), a next-generation CLI written in Python for hello resource management deployment model.</span></span>
>
>

## <a name="managing-app-service-plans"></a><span data-ttu-id="db8e4-110">Hantera App Service-planer</span><span class="sxs-lookup"><span data-stu-id="db8e4-110">Managing App Service Plans</span></span>
### <a name="create-an-app-service-plan"></a><span data-ttu-id="db8e4-111">Skapa en Apptjänstplan</span><span class="sxs-lookup"><span data-stu-id="db8e4-111">Create an App Service Plan</span></span>
<span data-ttu-id="db8e4-112">toocreate en apptjänstplan använda hello **azure appserviceplan skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="db8e4-112">toocreate an app service plan, use hello **azure appserviceplan create** command.</span></span>

<span data-ttu-id="db8e4-113">Nedan följer beskrivningar av hello olika parametrar:</span><span class="sxs-lookup"><span data-stu-id="db8e4-113">Following are descriptions of hello different parameters:</span></span>

* <span data-ttu-id="db8e4-114">**--resursgrupp**: resursgruppen som innehåller hello nyskapad app service-plan.</span><span class="sxs-lookup"><span data-stu-id="db8e4-114">**--resource-group**: resource group that includes hello newly created app service plan.</span></span>
* <span data-ttu-id="db8e4-115">**--namnet**: namn på hello app service-plan.</span><span class="sxs-lookup"><span data-stu-id="db8e4-115">**--name**: name of hello app service plan.</span></span>
* <span data-ttu-id="db8e4-116">**--plats**: plats för app service-plan.</span><span class="sxs-lookup"><span data-stu-id="db8e4-116">**--location**: app service plan location.</span></span>
* <span data-ttu-id="db8e4-117">**--sku**: hello önskad prisnivå sku (hello alternativ är: F1 (Free).</span><span class="sxs-lookup"><span data-stu-id="db8e4-117">**--sku**:  hello desired pricing sku (hello options are: F1 (Free).</span></span> <span data-ttu-id="db8e4-118">D1 (delad).</span><span class="sxs-lookup"><span data-stu-id="db8e4-118">D1 (Shared).</span></span> <span data-ttu-id="db8e4-119">B1 (grundläggande liten) B2 (grundläggande medel), och B3 (grundläggande stora).</span><span class="sxs-lookup"><span data-stu-id="db8e4-119">B1 (Basic Small), B2 (Basic Medium), and B3 (Basic Large).</span></span> <span data-ttu-id="db8e4-120">S1 (Standard liten) S2 (Standard medel), och S3 (Standard är stora).</span><span class="sxs-lookup"><span data-stu-id="db8e4-120">S1 (Standard Small), S2 (Standard Medium), and S3 (Standard Large).</span></span> <span data-ttu-id="db8e4-121">P1 (liten Premium), P2 (Premium medel), och P3 (stora Premium).)</span><span class="sxs-lookup"><span data-stu-id="db8e4-121">P1 (Premium Small), P2 (Premium Medium), and P3 (Premium Large).)</span></span>
* <span data-ttu-id="db8e4-122">**--instanser**: hello antalet arbetare i hello apptjänstplan (standardvärdet är 1).</span><span class="sxs-lookup"><span data-stu-id="db8e4-122">**--instances**: hello number of workers in hello app service plan (Default value is 1).</span></span>

<span data-ttu-id="db8e4-123">Exempel toouse denna cmdlet:</span><span class="sxs-lookup"><span data-stu-id="db8e4-123">Example toouse this cmdlet:</span></span>

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

#### <a name="create-a-linux-app-service-plan"></a><span data-ttu-id="db8e4-124">Skapa en Linux-Programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="db8e4-124">Create a Linux App Service Plan</span></span>

<span data-ttu-id="db8e4-125">Med hjälp av hello samma **azure appserviceplan skapa** kommandot med hello extraparametern **--islinux true**.</span><span class="sxs-lookup"><span data-stu-id="db8e4-125">Using hello same **azure appserviceplan create** command, with hello extra parameter **--islinux true**.</span></span> <span data-ttu-id="db8e4-126">Observera hello begränsningar och regioner som beskrivs i [introduktion tooApp-tjänsten på Linux](app-service-linux-intro.md)</span><span class="sxs-lookup"><span data-stu-id="db8e4-126">Note hello restrictions and regions outlined in [Introduction tooApp Service on Linux](app-service-linux-intro.md)</span></span>

### <a name="list-existing-app-service-plans"></a><span data-ttu-id="db8e4-127">Visa en lista med befintliga App Service-planer</span><span class="sxs-lookup"><span data-stu-id="db8e4-127">List Existing App Service Plans</span></span>
<span data-ttu-id="db8e4-128">toolist hello befintliga apptjänstplaner, Använd **azure appserviceplan lista** kommando.</span><span class="sxs-lookup"><span data-stu-id="db8e4-128">toolist hello existing app service plans, use **azure appserviceplan list** command.</span></span>

<span data-ttu-id="db8e4-129">toolist alla apptjänstplaner under en viss resursgrupp, Använd:</span><span class="sxs-lookup"><span data-stu-id="db8e4-129">toolist all app service plans under a specific resource group, use:</span></span>

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

<span data-ttu-id="db8e4-130">Använd tooget en viss app service-plan **azure appserviceplan visa** kommando:</span><span class="sxs-lookup"><span data-stu-id="db8e4-130">tooget a specific app service plan, use **azure appserviceplan show** command:</span></span>

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a><span data-ttu-id="db8e4-131">Konfigurera en befintlig App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="db8e4-131">Configure an existing App Service Plan</span></span>
<span data-ttu-id="db8e4-132">toochange hello inställningar för en befintlig app service-plan, använda hello **azure appserviceplan config** kommando.</span><span class="sxs-lookup"><span data-stu-id="db8e4-132">toochange hello settings for an existing app service plan, use hello **azure appserviceplan config** command.</span></span> <span data-ttu-id="db8e4-133">Du kan ändra hello sku och hello antalet arbetare</span><span class="sxs-lookup"><span data-stu-id="db8e4-133">You can change hello sku, and hello number of workers</span></span> 

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a><span data-ttu-id="db8e4-134">Skala en Apptjänstplan</span><span class="sxs-lookup"><span data-stu-id="db8e4-134">Scaling an App Service Plan</span></span>
<span data-ttu-id="db8e4-135">Använd en befintlig App Service-Plan, tooscale:</span><span class="sxs-lookup"><span data-stu-id="db8e4-135">tooscale an existing App Service Plan, use:</span></span>

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-hello-sku-of-an-app-service-plan"></a><span data-ttu-id="db8e4-136">Ändra hello SKU av en App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="db8e4-136">Changing hello SKU of an App Service Plan</span></span>
<span data-ttu-id="db8e4-137">toochange hello sku av en befintlig App Service-Plan, Använd:</span><span class="sxs-lookup"><span data-stu-id="db8e4-137">toochange hello sku of an existing App Service Plan, use:</span></span>

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a><span data-ttu-id="db8e4-138">Ta bort en befintlig App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="db8e4-138">Delete an existing App Service Plan</span></span>
<span data-ttu-id="db8e4-139">toodelete en befintlig app service-plan, tilldelas alla appar behöver toobe flyttat eller tagit bort först.</span><span class="sxs-lookup"><span data-stu-id="db8e4-139">toodelete an existing app service plan, all assigned apps need toobe moved or deleted first.</span></span> <span data-ttu-id="db8e4-140">Med hello **azure webapp ta bort** kommandot kan du ta bort hello app service-plan.</span><span class="sxs-lookup"><span data-stu-id="db8e4-140">Then using hello **azure webapp delete** command you can delete hello app service plan.</span></span>

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-apps"></a><span data-ttu-id="db8e4-141">Hantera Apptjänst-appar</span><span class="sxs-lookup"><span data-stu-id="db8e4-141">Managing App Service apps</span></span>
### <a name="create-a-web-app"></a><span data-ttu-id="db8e4-142">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="db8e4-142">Create a web app</span></span>
<span data-ttu-id="db8e4-143">toocreate en webbapp använder hello **azure webapp skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="db8e4-143">toocreate a web app, use hello **azure webapp create** command.</span></span>

<span data-ttu-id="db8e4-144">Nedan följer beskrivningar av hello olika parametrar:</span><span class="sxs-lookup"><span data-stu-id="db8e4-144">Following are descriptions of hello different parameters:</span></span>

* <span data-ttu-id="db8e4-145">**--namnet**: namn för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="db8e4-145">**--name**: name for hello web app.</span></span>
* <span data-ttu-id="db8e4-146">**--plan**: namn för hello serviceplan toohost hello-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="db8e4-146">**--plan**: name for hello service plan used toohost hello web app.</span></span>
* <span data-ttu-id="db8e4-147">**--resursgrupp**: resursgruppen som är värd för hello App service-plan.</span><span class="sxs-lookup"><span data-stu-id="db8e4-147">**--resource-group**: resource group that hosts hello App service plan.</span></span>
* <span data-ttu-id="db8e4-148">**--plats**: hello app webbplats.</span><span class="sxs-lookup"><span data-stu-id="db8e4-148">**--location**: hello web app location.</span></span>

<span data-ttu-id="db8e4-149">Exempel toouse denna cmdlet:</span><span class="sxs-lookup"><span data-stu-id="db8e4-149">Example toouse this cmdlet:</span></span>

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-app"></a><span data-ttu-id="db8e4-150">Ta bort en befintlig app</span><span class="sxs-lookup"><span data-stu-id="db8e4-150">Delete an existing app</span></span>
<span data-ttu-id="db8e4-151">toodelete en befintlig app som du kan använda hello **azure webapp ta bort** kommando.</span><span class="sxs-lookup"><span data-stu-id="db8e4-151">toodelete an existing app, you can use hello **azure webapp delete** command.</span></span> <span data-ttu-id="db8e4-152">Du behöver toospecify hello namnet på hello app och hello resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="db8e4-152">You need toospecify hello name of hello app and hello resource group name.</span></span>

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-apps"></a><span data-ttu-id="db8e4-153">Visa en lista med befintliga appar</span><span class="sxs-lookup"><span data-stu-id="db8e4-153">List existing apps</span></span>
<span data-ttu-id="db8e4-154">toolist hello befintliga appar använder hello **azure webapp listan** kommando.</span><span class="sxs-lookup"><span data-stu-id="db8e4-154">toolist hello existing apps, use hello **azure webapp list** command.</span></span>

<span data-ttu-id="db8e4-155">toolist Använd för alla program i en viss resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="db8e4-155">toolist all apps in a specific resource group, use:</span></span>

    azure webapp list --resource-group ContosoAzureResourceGroup

<span data-ttu-id="db8e4-156">tooget en viss app använder hello **azure webapp visa** kommando.</span><span class="sxs-lookup"><span data-stu-id="db8e4-156">tooget a specific app, use hello **azure webapp show** command.</span></span>

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-app"></a><span data-ttu-id="db8e4-157">Konfigurera en befintlig app</span><span class="sxs-lookup"><span data-stu-id="db8e4-157">Configure an existing app</span></span>
<span data-ttu-id="db8e4-158">toochange hello inställningar och konfigurationer för en befintlig app använder hello **azure webapp konfigurationsuppsättning** kommando.</span><span class="sxs-lookup"><span data-stu-id="db8e4-158">toochange hello settings and configurations for an existing app, use hello **azure webapp config set** command.</span></span>

<span data-ttu-id="db8e4-159">Exempel (1): ändra hello php-versionen av en app</span><span class="sxs-lookup"><span data-stu-id="db8e4-159">Example (1): change hello php version of a app</span></span> 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

<span data-ttu-id="db8e4-160">Exempel (2): lägga till eller ändra app-inställningar</span><span class="sxs-lookup"><span data-stu-id="db8e4-160">Example (2): add or change app settings</span></span>

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

<span data-ttu-id="db8e4-161">tooknow vilka andra konfigurationsobjekt kan ändras, Använd hello **azure webapp ställas -h** kommando.</span><span class="sxs-lookup"><span data-stu-id="db8e4-161">tooknow what other configuration can be changed, use hello **azure webapp config set -h** command.</span></span>

### <a name="change-hello-state-of-an-existing-app"></a><span data-ttu-id="db8e4-162">Ändra hello tillståndet för en befintlig app</span><span class="sxs-lookup"><span data-stu-id="db8e4-162">Change hello state of an existing app</span></span>
#### <a name="restart-an-app"></a><span data-ttu-id="db8e4-163">Starta om en app</span><span class="sxs-lookup"><span data-stu-id="db8e4-163">Restart an app</span></span>
<span data-ttu-id="db8e4-164">Du måste ange hello namn och resurs grupp hello app toorestart en app.</span><span class="sxs-lookup"><span data-stu-id="db8e4-164">toorestart an app, you must specify hello name and resource group of hello app.</span></span>

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-an-app"></a><span data-ttu-id="db8e4-165">Stoppa en app</span><span class="sxs-lookup"><span data-stu-id="db8e4-165">Stop an app</span></span>
<span data-ttu-id="db8e4-166">Du måste ange hello namn och resurs grupp hello app toostop en app.</span><span class="sxs-lookup"><span data-stu-id="db8e4-166">toostop an app, you must specify hello name and resource group of hello app.</span></span>

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-an-app"></a><span data-ttu-id="db8e4-167">Starta en app</span><span class="sxs-lookup"><span data-stu-id="db8e4-167">Start an app</span></span>
<span data-ttu-id="db8e4-168">Du måste ange hello namn och resurs grupp hello app toostart en app.</span><span class="sxs-lookup"><span data-stu-id="db8e4-168">toostart an app, you must specify hello name and resource group of hello app.</span></span>

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-publishing-profiles-of-an-app"></a><span data-ttu-id="db8e4-169">Hantera publishing profiler av en app</span><span class="sxs-lookup"><span data-stu-id="db8e4-169">Manage publishing profiles of an app</span></span>
<span data-ttu-id="db8e4-170">Varje app har en publiceringsprofil som kan använda toopublish din kod.</span><span class="sxs-lookup"><span data-stu-id="db8e4-170">Each app has a publishing profile that can be used toopublish your code.</span></span>

#### <a name="get-publishing-profile"></a><span data-ttu-id="db8e4-171">Hämta Publicera profil</span><span class="sxs-lookup"><span data-stu-id="db8e4-171">Get Publishing Profile</span></span>
<span data-ttu-id="db8e4-172">tooget hello Publicera profil för en app, Använd:</span><span class="sxs-lookup"><span data-stu-id="db8e4-172">tooget hello publishing profile for a app, use:</span></span>

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

<span data-ttu-id="db8e4-173">Det här kommandot ekon hello Publicera profil användarnamn och lösenord toohello kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="db8e4-173">This command echoes hello publishing profile username and password toohello command line.</span></span>

### <a name="manage-app-hostnames"></a><span data-ttu-id="db8e4-174">Hantera appen värdnamn</span><span class="sxs-lookup"><span data-stu-id="db8e4-174">Manage app hostnames</span></span>
<span data-ttu-id="db8e4-175">toomanage värdnamnsbindningar för din app använder hello **azure webapp config värdnamn** kommando</span><span class="sxs-lookup"><span data-stu-id="db8e4-175">toomanage hostname bindings for your app, use hello **azure webapp config hostnames** command</span></span>  

#### <a name="list-hostname-bindings"></a><span data-ttu-id="db8e4-176">Lista värdnamnsbindningar</span><span class="sxs-lookup"><span data-stu-id="db8e4-176">List hostname bindings</span></span>
<span data-ttu-id="db8e4-177">tooget hello aktuella värdnamnsbindningar för en app, Använd:</span><span class="sxs-lookup"><span data-stu-id="db8e4-177">tooget hello current hostname bindings for an app, use:</span></span>

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a><span data-ttu-id="db8e4-178">Lägg till värdnamnsbindningar</span><span class="sxs-lookup"><span data-stu-id="db8e4-178">Add hostname bindings</span></span>
<span data-ttu-id="db8e4-179">tooadd hostname bindningar tooan appen, Använd:</span><span class="sxs-lookup"><span data-stu-id="db8e4-179">tooadd hostname bindings tooan app, use:</span></span>

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a><span data-ttu-id="db8e4-180">Ta bort värdnamnsbindningar</span><span class="sxs-lookup"><span data-stu-id="db8e4-180">Delete hostname bindings</span></span>
<span data-ttu-id="db8e4-181">toodelete värdnamnsbindningar, Använd:</span><span class="sxs-lookup"><span data-stu-id="db8e4-181">toodelete hostname bindings, use:</span></span>

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

## <a name="next-steps"></a><span data-ttu-id="db8e4-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db8e4-182">Next Steps</span></span>
* <span data-ttu-id="db8e4-183">toolearn om Azure Resource Manager CLI-stöd finns [använda hello Azure CLI toomanage Azure resurser och resursgrupper.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="db8e4-183">toolearn about Azure Resource Manager CLI support, see [Use hello Azure CLI toomanage Azure resources and resource groups.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)</span></span>
* <span data-ttu-id="db8e4-184">toolearn om hur du hanterar App Service med PowerShell, se [Using Azure Resource Manager-Based PowerShell tooManage Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="db8e4-184">toolearn about managing App Service using PowerShell, see [Using Azure Resource Manager-Based PowerShell tooManage Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)</span></span>
* <span data-ttu-id="db8e4-185">toolearn om Azure App Service på Linux, se [introduktion tooApp-tjänsten på Linux](app-service-linux-intro.md)</span><span class="sxs-lookup"><span data-stu-id="db8e4-185">toolearn about Azure App Service on Linux, see [Introduction tooApp Service on Linux](app-service-linux-intro.md)</span></span>
