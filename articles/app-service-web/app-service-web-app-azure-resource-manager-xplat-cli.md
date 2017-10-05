---
title: "Azure Resource Manager-baserade plattformar kommandoradsverktyg för Azure Web App | Microsoft Docs"
description: "Lär dig hur du använder den nya Azure Resource Manager-baserade plattformar kommandoradsverktyg för att hantera dina Azure-webbprogram."
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
ms.openlocfilehash: 7a03e1417617453c43edcc3787da10d171359757
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-app-service"></a><span data-ttu-id="743fa-103">Med hjälp av Azure Resource Manager-baserade XPlat CLI för Azure App Service</span><span class="sxs-lookup"><span data-stu-id="743fa-103">Using Azure Resource Manager-Based XPlat CLI for Azure App Service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="743fa-104">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="743fa-104">Azure CLI</span></span>](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [<span data-ttu-id="743fa-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="743fa-105">Azure PowerShell</span></span>](app-service-web-app-azure-resource-manager-powershell.md)

<span data-ttu-id="743fa-106">Nya kommandon har lagts till med lanseringen av Microsoft Azure plattformsoberoende kommandoradsverktyg version 0.10.5.</span><span class="sxs-lookup"><span data-stu-id="743fa-106">With the release of Microsoft Azure Cross-platform Command-Line Tools version 0.10.5, new commands have been added.</span></span> <span data-ttu-id="743fa-107">Dessa kommandon ge användaren möjlighet att använda Azure Resource Manager-baserade PowerShell-kommandon för att hantera App Service.</span><span class="sxs-lookup"><span data-stu-id="743fa-107">These commands give the user the ability to use Azure Resource Manager-based PowerShell commands to manage App Service.</span></span>

<span data-ttu-id="743fa-108">Läs om hur du hanterar resursgrupper i [använda Azure CLI för att hantera Azure-resurser och resursgrupper](../azure-resource-manager/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="743fa-108">To learn about managing Resource Groups, see [Use the Azure CLI to manage Azure resources and resource groups](../azure-resource-manager/xplat-cli-azure-resource-manager.md).</span></span> 

> [!NOTE] 
> <span data-ttu-id="743fa-109">Prova också [Azure CLI 2.0](https://github.com/Azure/azure-cli), en nästa generationens CLI som skrivits i Python för resursdistributionsmodell för hantering.</span><span class="sxs-lookup"><span data-stu-id="743fa-109">Also, try out [Azure CLI 2.0](https://github.com/Azure/azure-cli), a next-generation CLI written in Python for the resource management deployment model.</span></span>
>
>

## <a name="managing-app-service-plans"></a><span data-ttu-id="743fa-110">Hantera App Service-planer</span><span class="sxs-lookup"><span data-stu-id="743fa-110">Managing App Service Plans</span></span>
### <a name="create-an-app-service-plan"></a><span data-ttu-id="743fa-111">Skapa en Apptjänstplan</span><span class="sxs-lookup"><span data-stu-id="743fa-111">Create an App Service Plan</span></span>
<span data-ttu-id="743fa-112">Använd för att skapa en apptjänstplan i **azure appserviceplan skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="743fa-112">To create an app service plan, use the **azure appserviceplan create** command.</span></span>

<span data-ttu-id="743fa-113">Nedan följer beskrivningar av olika parametrar:</span><span class="sxs-lookup"><span data-stu-id="743fa-113">Following are descriptions of the different parameters:</span></span>

* <span data-ttu-id="743fa-114">**--resursgrupp**: resursgruppen som innehåller den nyligen skapade apptjänstplanen.</span><span class="sxs-lookup"><span data-stu-id="743fa-114">**--resource-group**: resource group that includes the newly created app service plan.</span></span>
* <span data-ttu-id="743fa-115">**--namnet**: namn på app service-plan.</span><span class="sxs-lookup"><span data-stu-id="743fa-115">**--name**: name of the app service plan.</span></span>
* <span data-ttu-id="743fa-116">**--plats**: plats för app service-plan.</span><span class="sxs-lookup"><span data-stu-id="743fa-116">**--location**: app service plan location.</span></span>
* <span data-ttu-id="743fa-117">**--sku**: önskade prisnivå SKU: n (alternativen är: F1 (Free).</span><span class="sxs-lookup"><span data-stu-id="743fa-117">**--sku**:  the desired pricing sku (The options are: F1 (Free).</span></span> <span data-ttu-id="743fa-118">D1 (delad).</span><span class="sxs-lookup"><span data-stu-id="743fa-118">D1 (Shared).</span></span> <span data-ttu-id="743fa-119">B1 (grundläggande liten) B2 (grundläggande medel), och B3 (grundläggande stora).</span><span class="sxs-lookup"><span data-stu-id="743fa-119">B1 (Basic Small), B2 (Basic Medium), and B3 (Basic Large).</span></span> <span data-ttu-id="743fa-120">S1 (Standard liten) S2 (Standard medel), och S3 (Standard är stora).</span><span class="sxs-lookup"><span data-stu-id="743fa-120">S1 (Standard Small), S2 (Standard Medium), and S3 (Standard Large).</span></span> <span data-ttu-id="743fa-121">P1 (liten Premium), P2 (Premium medel), och P3 (stora Premium).)</span><span class="sxs-lookup"><span data-stu-id="743fa-121">P1 (Premium Small), P2 (Premium Medium), and P3 (Premium Large).)</span></span>
* <span data-ttu-id="743fa-122">**--instanser**: antal arbetare i appen tjänsten plan (standardvärdet är 1).</span><span class="sxs-lookup"><span data-stu-id="743fa-122">**--instances**: the number of workers in the app service plan (Default value is 1).</span></span>

<span data-ttu-id="743fa-123">Exempel för att använda denna cmdlet:</span><span class="sxs-lookup"><span data-stu-id="743fa-123">Example to use this cmdlet:</span></span>

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

#### <a name="create-a-linux-app-service-plan"></a><span data-ttu-id="743fa-124">Skapa en Linux-Programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="743fa-124">Create a Linux App Service Plan</span></span>

<span data-ttu-id="743fa-125">Använder samma **azure appserviceplan skapa** kommandot med extraparametern **--islinux true**.</span><span class="sxs-lookup"><span data-stu-id="743fa-125">Using the same **azure appserviceplan create** command, with the extra parameter **--islinux true**.</span></span> <span data-ttu-id="743fa-126">Observera de begränsningar och regioner som beskrivs i [introduktion till App Service på Linux](app-service-linux-intro.md)</span><span class="sxs-lookup"><span data-stu-id="743fa-126">Note the restrictions and regions outlined in [Introduction to App Service on Linux](app-service-linux-intro.md)</span></span>

### <a name="list-existing-app-service-plans"></a><span data-ttu-id="743fa-127">Visa en lista med befintliga App Service-planer</span><span class="sxs-lookup"><span data-stu-id="743fa-127">List Existing App Service Plans</span></span>
<span data-ttu-id="743fa-128">Om du vill visa befintliga apptjänstplaner använder **azure appserviceplan lista** kommando.</span><span class="sxs-lookup"><span data-stu-id="743fa-128">To list the existing app service plans, use **azure appserviceplan list** command.</span></span>

<span data-ttu-id="743fa-129">Om du vill visa en lista med alla apptjänstplaner under en viss resursgrupp, använder du:</span><span class="sxs-lookup"><span data-stu-id="743fa-129">To list all app service plans under a specific resource group, use:</span></span>

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

<span data-ttu-id="743fa-130">För att få en viss app service-plan kan använda **azure appserviceplan visa** kommando:</span><span class="sxs-lookup"><span data-stu-id="743fa-130">To get a specific app service plan, use **azure appserviceplan show** command:</span></span>

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a><span data-ttu-id="743fa-131">Konfigurera en befintlig App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="743fa-131">Configure an existing App Service Plan</span></span>
<span data-ttu-id="743fa-132">Om du vill ändra inställningarna för en befintlig app service-plan i **azure appserviceplan config** kommando.</span><span class="sxs-lookup"><span data-stu-id="743fa-132">To change the settings for an existing app service plan, use the **azure appserviceplan config** command.</span></span> <span data-ttu-id="743fa-133">Du kan ändra SKU: n och antalet arbetare</span><span class="sxs-lookup"><span data-stu-id="743fa-133">You can change the sku, and the number of workers</span></span> 

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a><span data-ttu-id="743fa-134">Skala en Apptjänstplan</span><span class="sxs-lookup"><span data-stu-id="743fa-134">Scaling an App Service Plan</span></span>
<span data-ttu-id="743fa-135">Om du vill skala en befintlig App Service-Plan, använder du:</span><span class="sxs-lookup"><span data-stu-id="743fa-135">To scale an existing App Service Plan, use:</span></span>

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a><span data-ttu-id="743fa-136">Ändra en Apptjänstplan SKU</span><span class="sxs-lookup"><span data-stu-id="743fa-136">Changing the SKU of an App Service Plan</span></span>
<span data-ttu-id="743fa-137">Om du vill ändra sku av en befintlig App Service-Plan, använder du:</span><span class="sxs-lookup"><span data-stu-id="743fa-137">To change the sku of an existing App Service Plan, use:</span></span>

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a><span data-ttu-id="743fa-138">Ta bort en befintlig App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="743fa-138">Delete an existing App Service Plan</span></span>
<span data-ttu-id="743fa-139">Om du vill ta bort en befintlig app service-plan måste alla tilldelade appar som ska flyttas eller tas bort först.</span><span class="sxs-lookup"><span data-stu-id="743fa-139">To delete an existing app service plan, all assigned apps need to be moved or deleted first.</span></span> <span data-ttu-id="743fa-140">Med hjälp av den **azure webapp ta bort** kommandot kan du ta bort app service-plan.</span><span class="sxs-lookup"><span data-stu-id="743fa-140">Then using the **azure webapp delete** command you can delete the app service plan.</span></span>

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-apps"></a><span data-ttu-id="743fa-141">Hantera Apptjänst-appar</span><span class="sxs-lookup"><span data-stu-id="743fa-141">Managing App Service apps</span></span>
### <a name="create-a-web-app"></a><span data-ttu-id="743fa-142">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="743fa-142">Create a web app</span></span>
<span data-ttu-id="743fa-143">Använd för att skapa en webbapp i **azure webapp skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="743fa-143">To create a web app, use the **azure webapp create** command.</span></span>

<span data-ttu-id="743fa-144">Nedan följer beskrivningar av olika parametrar:</span><span class="sxs-lookup"><span data-stu-id="743fa-144">Following are descriptions of the different parameters:</span></span>

* <span data-ttu-id="743fa-145">**--namnet**: namn för webbappen.</span><span class="sxs-lookup"><span data-stu-id="743fa-145">**--name**: name for the web app.</span></span>
* <span data-ttu-id="743fa-146">**--plan**: namn för service-plan som används som värd för webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="743fa-146">**--plan**: name for the service plan used to host the web app.</span></span>
* <span data-ttu-id="743fa-147">**--resursgrupp**: resursgruppen som är värd för App service-plan.</span><span class="sxs-lookup"><span data-stu-id="743fa-147">**--resource-group**: resource group that hosts the App service plan.</span></span>
* <span data-ttu-id="743fa-148">**--plats**: app webbplats.</span><span class="sxs-lookup"><span data-stu-id="743fa-148">**--location**: the web app location.</span></span>

<span data-ttu-id="743fa-149">Exempel för att använda denna cmdlet:</span><span class="sxs-lookup"><span data-stu-id="743fa-149">Example to use this cmdlet:</span></span>

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-app"></a><span data-ttu-id="743fa-150">Ta bort en befintlig app</span><span class="sxs-lookup"><span data-stu-id="743fa-150">Delete an existing app</span></span>
<span data-ttu-id="743fa-151">Om du vill ta bort en befintlig app som du kan använda den **azure webapp ta bort** kommando.</span><span class="sxs-lookup"><span data-stu-id="743fa-151">To delete an existing app, you can use the **azure webapp delete** command.</span></span> <span data-ttu-id="743fa-152">Du måste ange namnet på appen och resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="743fa-152">You need to specify the name of the app and the resource group name.</span></span>

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-apps"></a><span data-ttu-id="743fa-153">Visa en lista med befintliga appar</span><span class="sxs-lookup"><span data-stu-id="743fa-153">List existing apps</span></span>
<span data-ttu-id="743fa-154">Om du vill visa befintliga appar använder den **azure webapp listan** kommando.</span><span class="sxs-lookup"><span data-stu-id="743fa-154">To list the existing apps, use the **azure webapp list** command.</span></span>

<span data-ttu-id="743fa-155">Om du vill visa en lista över alla program i en viss resursgrupp, använder du:</span><span class="sxs-lookup"><span data-stu-id="743fa-155">To list all apps in a specific resource group, use:</span></span>

    azure webapp list --resource-group ContosoAzureResourceGroup

<span data-ttu-id="743fa-156">För att få en viss app måste du använda den **azure webapp visa** kommando.</span><span class="sxs-lookup"><span data-stu-id="743fa-156">To get a specific app, use the **azure webapp show** command.</span></span>

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-app"></a><span data-ttu-id="743fa-157">Konfigurera en befintlig app</span><span class="sxs-lookup"><span data-stu-id="743fa-157">Configure an existing app</span></span>
<span data-ttu-id="743fa-158">Om du vill ändra inställningar och konfigurationer för en befintlig app som använder den **azure webapp konfigurationsuppsättning** kommando.</span><span class="sxs-lookup"><span data-stu-id="743fa-158">To change the settings and configurations for an existing app, use the **azure webapp config set** command.</span></span>

<span data-ttu-id="743fa-159">Exempel (1): ändra php-versionen av en app</span><span class="sxs-lookup"><span data-stu-id="743fa-159">Example (1): change the php version of a app</span></span> 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

<span data-ttu-id="743fa-160">Exempel (2): lägga till eller ändra app-inställningar</span><span class="sxs-lookup"><span data-stu-id="743fa-160">Example (2): add or change app settings</span></span>

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

<span data-ttu-id="743fa-161">Du behöver veta vad annan konfiguration kan vara ändras, använder den **azure webapp ställas -h** kommando.</span><span class="sxs-lookup"><span data-stu-id="743fa-161">To know what other configuration can be changed, use the **azure webapp config set -h** command.</span></span>

### <a name="change-the-state-of-an-existing-app"></a><span data-ttu-id="743fa-162">Ändra tillstånd för en befintlig app</span><span class="sxs-lookup"><span data-stu-id="743fa-162">Change the state of an existing app</span></span>
#### <a name="restart-an-app"></a><span data-ttu-id="743fa-163">Starta om en app</span><span class="sxs-lookup"><span data-stu-id="743fa-163">Restart an app</span></span>
<span data-ttu-id="743fa-164">Om du vill starta om en app måste du ange gruppen namn och en resurs i appen.</span><span class="sxs-lookup"><span data-stu-id="743fa-164">To restart an app, you must specify the name and resource group of the app.</span></span>

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-an-app"></a><span data-ttu-id="743fa-165">Stoppa en app</span><span class="sxs-lookup"><span data-stu-id="743fa-165">Stop an app</span></span>
<span data-ttu-id="743fa-166">Om du vill stoppa en app måste du ange gruppen namn och en resurs i appen.</span><span class="sxs-lookup"><span data-stu-id="743fa-166">To stop an app, you must specify the name and resource group of the app.</span></span>

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-an-app"></a><span data-ttu-id="743fa-167">Starta en app</span><span class="sxs-lookup"><span data-stu-id="743fa-167">Start an app</span></span>
<span data-ttu-id="743fa-168">Om du vill starta en app måste du ange gruppen namn och en resurs i appen.</span><span class="sxs-lookup"><span data-stu-id="743fa-168">To start an app, you must specify the name and resource group of the app.</span></span>

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-publishing-profiles-of-an-app"></a><span data-ttu-id="743fa-169">Hantera publishing profiler av en app</span><span class="sxs-lookup"><span data-stu-id="743fa-169">Manage publishing profiles of an app</span></span>
<span data-ttu-id="743fa-170">Varje app har en publiceringsprofil som kan användas för att publicera din kod.</span><span class="sxs-lookup"><span data-stu-id="743fa-170">Each app has a publishing profile that can be used to publish your code.</span></span>

#### <a name="get-publishing-profile"></a><span data-ttu-id="743fa-171">Hämta Publicera profil</span><span class="sxs-lookup"><span data-stu-id="743fa-171">Get Publishing Profile</span></span>
<span data-ttu-id="743fa-172">Använd följande för att hämta publiceringsprofilen för en app:</span><span class="sxs-lookup"><span data-stu-id="743fa-172">To get the publishing profile for a app, use:</span></span>

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

<span data-ttu-id="743fa-173">Det här kommandot ekon publishing profil användarnamn och lösenord till kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="743fa-173">This command echoes the publishing profile username and password to the command line.</span></span>

### <a name="manage-app-hostnames"></a><span data-ttu-id="743fa-174">Hantera appen värdnamn</span><span class="sxs-lookup"><span data-stu-id="743fa-174">Manage app hostnames</span></span>
<span data-ttu-id="743fa-175">Om du vill hantera värdnamnsbindningar för din app använder det **azure webapp config värdnamn** kommando</span><span class="sxs-lookup"><span data-stu-id="743fa-175">To manage hostname bindings for your app, use the **azure webapp config hostnames** command</span></span>  

#### <a name="list-hostname-bindings"></a><span data-ttu-id="743fa-176">Lista värdnamnsbindningar</span><span class="sxs-lookup"><span data-stu-id="743fa-176">List hostname bindings</span></span>
<span data-ttu-id="743fa-177">Om du vill hämta de aktuella värdnamnsbindningar för en app, använder du:</span><span class="sxs-lookup"><span data-stu-id="743fa-177">To get the current hostname bindings for an app, use:</span></span>

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a><span data-ttu-id="743fa-178">Lägg till värdnamnsbindningar</span><span class="sxs-lookup"><span data-stu-id="743fa-178">Add hostname bindings</span></span>
<span data-ttu-id="743fa-179">Lägg till värdnamnsbindningar i en app, använder du:</span><span class="sxs-lookup"><span data-stu-id="743fa-179">To add hostname bindings to an app, use:</span></span>

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a><span data-ttu-id="743fa-180">Ta bort värdnamnsbindningar</span><span class="sxs-lookup"><span data-stu-id="743fa-180">Delete hostname bindings</span></span>
<span data-ttu-id="743fa-181">Om du vill ta bort värdnamnsbindningar, använder du:</span><span class="sxs-lookup"><span data-stu-id="743fa-181">To delete hostname bindings, use:</span></span>

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

## <a name="next-steps"></a><span data-ttu-id="743fa-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="743fa-182">Next Steps</span></span>
* <span data-ttu-id="743fa-183">Läs om Azure Resource Manager CLI-stöd i [använda Azure CLI för att hantera Azure-resurser och resursgrupper.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="743fa-183">To learn about Azure Resource Manager CLI support, see [Use the Azure CLI to manage Azure resources and resource groups.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)</span></span>
* <span data-ttu-id="743fa-184">Läs om hur du hanterar App Service med PowerShell i [Using Azure Resource Manager-Based PowerShell för att hantera Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="743fa-184">To learn about managing App Service using PowerShell, see [Using Azure Resource Manager-Based PowerShell to Manage Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)</span></span>
* <span data-ttu-id="743fa-185">Mer information om Azure App Service på Linux, se [introduktion till App Service på Linux](app-service-linux-intro.md)</span><span class="sxs-lookup"><span data-stu-id="743fa-185">To learn about Azure App Service on Linux, see [Introduction to App Service on Linux](app-service-linux-intro.md)</span></span>
