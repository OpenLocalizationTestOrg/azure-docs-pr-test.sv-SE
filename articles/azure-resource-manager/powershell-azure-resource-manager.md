---
title: "Hantera Azure lösningar med PowerShell | Microsoft Docs"
description: "Använda Azure PowerShell och Resource Manager för att hantera dina resurser."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: ff42e5cb29005c5f4b97babdae58bef9382071f0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a><span data-ttu-id="7e429-103">Hantera resurser med Azure PowerShell och Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7e429-103">Manage resources with Azure PowerShell and Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7e429-104">Portal</span><span class="sxs-lookup"><span data-stu-id="7e429-104">Portal</span></span>](resource-group-portal.md)
> * [<span data-ttu-id="7e429-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7e429-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="7e429-106">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7e429-106">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="7e429-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="7e429-107">REST API</span></span>](resource-manager-rest-api.md)
>
>

<span data-ttu-id="7e429-108">I den här artikeln lär du dig att hantera dina lösningar med Azure PowerShell och Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7e429-108">In this article, you learn how to manage your solutions with Azure PowerShell and Azure Resource Manager.</span></span> <span data-ttu-id="7e429-109">Om du inte är bekant med Resource Manager finns [översikt över Resource Manager](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7e429-109">If you are not familiar with Resource Manager, see [Resource Manager Overview](resource-group-overview.md).</span></span> <span data-ttu-id="7e429-110">Det här avsnittet fokuserar på hanteringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="7e429-110">This topic focuses on management tasks.</span></span> <span data-ttu-id="7e429-111">Du kommer att:</span><span class="sxs-lookup"><span data-stu-id="7e429-111">You will:</span></span>

1. <span data-ttu-id="7e429-112">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="7e429-112">Create a resource group</span></span>
2. <span data-ttu-id="7e429-113">Lägg till en resurs i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7e429-113">Add a resource to the resource group</span></span>
3. <span data-ttu-id="7e429-114">Lägga till en etikett till resursen</span><span class="sxs-lookup"><span data-stu-id="7e429-114">Add a tag to the resource</span></span>
4. <span data-ttu-id="7e429-115">Fråga resurser baserat på namn eller värden</span><span class="sxs-lookup"><span data-stu-id="7e429-115">Query resources based on names or tag values</span></span>
5. <span data-ttu-id="7e429-116">Använda och ta bort ett lås på resursen</span><span class="sxs-lookup"><span data-stu-id="7e429-116">Apply and remove a lock on the resource</span></span>
6. <span data-ttu-id="7e429-117">Ta bort en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="7e429-117">Delete a resource group</span></span>

<span data-ttu-id="7e429-118">Den här artikeln visar inte hur du distribuerar en Resource Manager-mall till din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7e429-118">This article does not show how to deploy a Resource Manager template to your subscription.</span></span> <span data-ttu-id="7e429-119">Den här informationen finns [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="7e429-119">For that information, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>

## <a name="get-started-with-azure-powershell"></a><span data-ttu-id="7e429-120">Kom igång med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7e429-120">Get started with Azure PowerShell</span></span>

<span data-ttu-id="7e429-121">Om du inte har installerat Azure PowerShell, se [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7e429-121">If you have not installed Azure PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="7e429-122">Om du har installerat Azure PowerShell tidigare men inte har uppdaterat den nyligen kan du överväga att installera den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="7e429-122">If you have installed Azure PowerShell in the past but have not updated it recently, consider installing the latest version.</span></span> <span data-ttu-id="7e429-123">Du kan uppdatera versionen via samma metod som du använde för att installera den.</span><span class="sxs-lookup"><span data-stu-id="7e429-123">You can update the version through the same method you used to install it.</span></span> <span data-ttu-id="7e429-124">Om du använder Web Platform Installer, starta den igen och leta efter en uppdatering.</span><span class="sxs-lookup"><span data-stu-id="7e429-124">For example, if you used the Web Platform Installer, launch it again and look for an update.</span></span>

<span data-ttu-id="7e429-125">Om du vill kontrollera din version av Azure-resurser modulen, använder du följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="7e429-125">To check your version of the Azure Resources module, use the following cmdlet:</span></span>

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

<span data-ttu-id="7e429-126">Det här avsnittet har uppdaterats för version 3.3.0.</span><span class="sxs-lookup"><span data-stu-id="7e429-126">This topic was updated for version 3.3.0.</span></span> <span data-ttu-id="7e429-127">Om du har en tidigare version, överensstämmer din upplevelse inte med de steg som visas i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="7e429-127">If you have an earlier version, your experience might not match the steps shown in this topic.</span></span> <span data-ttu-id="7e429-128">Dokumentation om cmdletar i den här versionen finns [AzureRM.Resources modulen](/powershell/module/azurerm.resources).</span><span class="sxs-lookup"><span data-stu-id="7e429-128">For documentation about the cmdlets in this version, see [AzureRM.Resources Module](/powershell/module/azurerm.resources).</span></span>

## <a name="log-in-to-your-azure-account"></a><span data-ttu-id="7e429-129">Logga in på ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="7e429-129">Log in to your Azure account</span></span>
<span data-ttu-id="7e429-130">Innan du arbetar med din lösning, måste du logga in på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="7e429-130">Before working on your solution, you must log in to your account.</span></span>

<span data-ttu-id="7e429-131">Om du vill logga in på ditt Azure-konto, använder den **Login-AzureRmAccount** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7e429-131">To log in to your Azure account, use the **Login-AzureRmAccount** cmdlet.</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="7e429-132">Den här cmdleten uppmanar dig att ange inloggningsuppgifterna för ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="7e429-132">The cmdlet prompts you for the login credentials for your Azure account.</span></span> <span data-ttu-id="7e429-133">När du har loggat in hämtas dina kontoinställningar så att de blir tillgängliga för Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7e429-133">After logging in, it downloads your account settings so they are available to Azure PowerShell.</span></span>

<span data-ttu-id="7e429-134">Cmdlet returnerar information om ditt konto och prenumeration ska användas för uppgifter.</span><span class="sxs-lookup"><span data-stu-id="7e429-134">The cmdlet returns information about your account and the subscription to use for the tasks.</span></span>

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

<span data-ttu-id="7e429-135">Om du har mer än en prenumeration kan växla du till en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7e429-135">If you have more than one subscription, you can switch to a different subscription.</span></span> <span data-ttu-id="7e429-136">Först ska vi se alla prenumerationer för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="7e429-136">First, let's see all the subscriptions for your account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="7e429-137">Den returnerar aktiverade och inaktiverade prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="7e429-137">It returns enabled and disabled subscriptions.</span></span>

```powershell
SubscriptionName : Example Subscription One
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Two
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Three
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Disabled
```

<span data-ttu-id="7e429-138">Ange om du vill växla till en annan prenumeration prenumerationsnamn med den **Set-AzureRmContext** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7e429-138">To switch to a different subscription, provide the subscription name with the **Set-AzureRmContext** cmdlet.</span></span>

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a><span data-ttu-id="7e429-139">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="7e429-139">Create a resource group</span></span>
<span data-ttu-id="7e429-140">Innan du distribuerar några resurser till din prenumeration, måste du skapa en resursgrupp som innehåller resurser.</span><span class="sxs-lookup"><span data-stu-id="7e429-140">Before deploying any resources to your subscription, you must create a resource group that will contain the resources.</span></span>

<span data-ttu-id="7e429-141">Du kan skapa en resursgrupp med det **New-AzureRmResourceGroup** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7e429-141">To create a resource group, use the **New-AzureRmResourceGroup** cmdlet.</span></span> <span data-ttu-id="7e429-142">Kommandot använder den **namn** parametern för att ange ett namn för resursgruppen och **plats** parametern för att ange dess plats.</span><span class="sxs-lookup"><span data-stu-id="7e429-142">The command uses the **Name** parameter to specify a name for the resource group and the **Location** parameter to specify its location.</span></span>

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

<span data-ttu-id="7e429-143">Utdata är i följande format:</span><span class="sxs-lookup"><span data-stu-id="7e429-143">The output is in the following format:</span></span>

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

<span data-ttu-id="7e429-144">Om du behöver hämta resursgruppen senare använder du följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="7e429-144">If you need to retrieve the resource group later, use the following cmdlet:</span></span>

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

<span data-ttu-id="7e429-145">För att få alla resursgrupper i din prenumeration kan du inte ange ett namn:</span><span class="sxs-lookup"><span data-stu-id="7e429-145">To get all the resource groups in your subscription, do not specify a name:</span></span>

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-to-a-resource-group"></a><span data-ttu-id="7e429-146">Lägg till resurser i en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="7e429-146">Add resources to a resource group</span></span>
<span data-ttu-id="7e429-147">Om du vill lägga till en resurs i resursgruppen som du kan använda den **ny AzureRmResource** cmdlet eller en cmdlet som är specifik för typ av resurs som skapas (t.ex. **New-AzureRmStorageAccount**).</span><span class="sxs-lookup"><span data-stu-id="7e429-147">To add a resource to the resource group, you can use the **New-AzureRmResource** cmdlet or a cmdlet that is specific to the type of resource you are creating (like **New-AzureRmStorageAccount**).</span></span> <span data-ttu-id="7e429-148">Det kan vara enklare att använda en cmdlet som är specifika för en resurstyp eftersom den innehåller parametrar för de egenskaper som behövs för den nya resursen.</span><span class="sxs-lookup"><span data-stu-id="7e429-148">You might find it easier to use a cmdlet that is specific to a resource type because it includes parameters for the properties that are needed for the new resource.</span></span> <span data-ttu-id="7e429-149">Att använda **ny AzureRmResource**, måste du känna till alla egenskaperna för att ange utan som efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="7e429-149">To use **New-AzureRmResource**, you must know all the properties to set without being prompted for them.</span></span>

<span data-ttu-id="7e429-150">Lägga till en resurs via cmdlets kan orsaka framtida förvirring eftersom den nya resursen inte finns i en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="7e429-150">However, adding a resource through cmdlets might cause future confusion because the new resource does not exist in a Resource Manager template.</span></span> <span data-ttu-id="7e429-151">Microsoft rekommenderar att du definierar infrastrukturen för din Azure-lösning i en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="7e429-151">Microsoft recommends defining the infrastructure for your Azure solution in a Resource Manager template.</span></span> <span data-ttu-id="7e429-152">Mallar kan du distribuerar din lösning på ett tillförlitligt sätt och flera gånger.</span><span class="sxs-lookup"><span data-stu-id="7e429-152">Templates enable you to reliably and repeatedly deploy your solution.</span></span> <span data-ttu-id="7e429-153">Du skapar ett lagringskonto med en PowerShell-cmdlet för det här avsnittet, men senare du generera en mall från resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7e429-153">For this topic, you create a storage account with a PowerShell cmdlet, but later you generate a template from your resource group.</span></span>

<span data-ttu-id="7e429-154">Följande cmdlet skapar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7e429-154">The following cmdlet creates a storage account.</span></span> <span data-ttu-id="7e429-155">Ange ett unikt namn för storage-konto i stället för med hjälp av namnet som visas i exemplet.</span><span class="sxs-lookup"><span data-stu-id="7e429-155">Instead of using the name shown in the example, provide a unique name for the storage account.</span></span> <span data-ttu-id="7e429-156">Namnet måste vara mellan 3 och 24 tecken långt och innehålla endast siffror och gemener.</span><span class="sxs-lookup"><span data-stu-id="7e429-156">The name must be between 3 and 24 characters in length, and use only numbers and lower-case letters.</span></span> <span data-ttu-id="7e429-157">Om du använder det namn som visas i exemplet felmeddelande ett eftersom namnet redan används.</span><span class="sxs-lookup"><span data-stu-id="7e429-157">If you use the name shown in the example, you receive an error because that name is already in use.</span></span>

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

<span data-ttu-id="7e429-158">Om du behöver hämta den här resursen senare använder du följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="7e429-158">If you need to retrieve this resource later, use the following cmdlet:</span></span>

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a><span data-ttu-id="7e429-159">Lägga till en tagg</span><span class="sxs-lookup"><span data-stu-id="7e429-159">Add a tag</span></span>

<span data-ttu-id="7e429-160">Taggar kan du organisera dina resurser efter olika egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7e429-160">Tags enable you to organize your resources according to different properties.</span></span> <span data-ttu-id="7e429-161">Du kan till exempel ha flera resurser i olika resursgrupper som tillhör samma avdelning.</span><span class="sxs-lookup"><span data-stu-id="7e429-161">For example, you may have several resources in different resource groups that belong to the same department.</span></span> <span data-ttu-id="7e429-162">Du kan använda en avdelning taggen och ett värde till dessa resurser för att markera dem som tillhör samma kategori.</span><span class="sxs-lookup"><span data-stu-id="7e429-162">You can apply a department tag and value to those resources to mark them as belonging to the same category.</span></span> <span data-ttu-id="7e429-163">Eller så kan du markera om en resurs används i en produktionsmiljö eller testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7e429-163">Or, you can mark whether a resource is used in a production or test environment.</span></span> <span data-ttu-id="7e429-164">Du lägger till taggar till en resurs i det här avsnittet, men i din miljö det mest sannolika klokt att lägga till taggar för dina resurser.</span><span class="sxs-lookup"><span data-stu-id="7e429-164">In this topic, you apply tags to only one resource, but in your environment it most likely makes sense to apply tags to all your resources.</span></span>

<span data-ttu-id="7e429-165">Följande cmdlet gäller två taggar för ditt lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="7e429-165">The following cmdlet applies two tags to your storage account:</span></span>

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

<span data-ttu-id="7e429-166">Taggar uppdateras som ett enskilt objekt.</span><span class="sxs-lookup"><span data-stu-id="7e429-166">Tags are updated as a single object.</span></span> <span data-ttu-id="7e429-167">Om du vill lägga till en tagg i en resurs som redan omfattar taggar, att hämta befintliga taggar.</span><span class="sxs-lookup"><span data-stu-id="7e429-167">To add a tag to a resource that already includes tags, first retrieve the existing tags.</span></span> <span data-ttu-id="7e429-168">Lägg till ny tagg i det objekt som innehåller befintliga taggar och tillämpa alla taggar till resursen.</span><span class="sxs-lookup"><span data-stu-id="7e429-168">Add the new tag to the object that contains the existing tags, and reapply all the tags to the resource.</span></span>

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a><span data-ttu-id="7e429-169">Sök efter resurser</span><span class="sxs-lookup"><span data-stu-id="7e429-169">Search for resources</span></span>

<span data-ttu-id="7e429-170">Använd den **hitta AzureRmResource** för att hämta resurser för olika sökvillkor.</span><span class="sxs-lookup"><span data-stu-id="7e429-170">Use the **Find-AzureRmResource** cmdlet to retrieve resources for different search conditions.</span></span>

* <span data-ttu-id="7e429-171">För att få en resurs med namnet, ange den **ResourceNameContains** parameter:</span><span class="sxs-lookup"><span data-stu-id="7e429-171">To get a resource by name, provide the **ResourceNameContains** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* <span data-ttu-id="7e429-172">Om du vill hämta alla resurser i en resursgrupp, ange den **ResourceGroupNameContains** parameter:</span><span class="sxs-lookup"><span data-stu-id="7e429-172">To get all the resources in a resource group, provide the **ResourceGroupNameContains** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* <span data-ttu-id="7e429-173">Om du vill hämta alla resurser med taggnamn och värde, ange den **TagName** och **TagValue** parametrar:</span><span class="sxs-lookup"><span data-stu-id="7e429-173">To get all the resources with a tag name and value, provide the **TagName** and **TagValue** parameters:</span></span>

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* <span data-ttu-id="7e429-174">Alla resurser med en viss resurstyp, ange den **ResourceType** parameter:</span><span class="sxs-lookup"><span data-stu-id="7e429-174">To all the resources with a particular resource type, provide the **ResourceType** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="lock-a-resource"></a><span data-ttu-id="7e429-175">Låsa en resurs</span><span class="sxs-lookup"><span data-stu-id="7e429-175">Lock a resource</span></span>

<span data-ttu-id="7e429-176">När du behöver se till att en kritisk resurs tas inte bort av misstag eller ändras kan tillämpa ett lås på resursen.</span><span class="sxs-lookup"><span data-stu-id="7e429-176">When you need to make sure a critical resource is not accidentally deleted or modified, apply a lock to the resource.</span></span> <span data-ttu-id="7e429-177">Du kan ange antingen en **CanNotDelete** eller **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="7e429-177">You can specify either a **CanNotDelete** or **ReadOnly**.</span></span>

<span data-ttu-id="7e429-178">För att skapa eller ta bort management lås, måste du ha åtkomst till `Microsoft.Authorization/*` eller `Microsoft.Authorization/locks/*` åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7e429-178">To create or delete management locks, you must have access to `Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions.</span></span> <span data-ttu-id="7e429-179">I de inbyggda rollerna beviljas endast ägare och administratör för användaråtkomst dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7e429-179">Of the built-in roles, only Owner and User Access Administrator are granted those actions.</span></span>

<span data-ttu-id="7e429-180">Om du vill använda ett lås, använder du följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="7e429-180">To apply a lock, use the following cmdlet:</span></span>

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

<span data-ttu-id="7e429-181">Låst resursen i föregående exempel kan inte tas bort förrän låset tas bort.</span><span class="sxs-lookup"><span data-stu-id="7e429-181">The locked resource in the preceding example cannot be deleted until the lock is removed.</span></span> <span data-ttu-id="7e429-182">Ta bort ett lås med:</span><span class="sxs-lookup"><span data-stu-id="7e429-182">To remove a lock, use:</span></span>

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

<span data-ttu-id="7e429-183">Mer information om inställningen Lås finns [låsa resurser med Azure Resource Manager](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="7e429-183">For more information about setting locks, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

## <a name="remove-resources-or-resource-group"></a><span data-ttu-id="7e429-184">Ta bort resurser eller resursgrupp</span><span class="sxs-lookup"><span data-stu-id="7e429-184">Remove resources or resource group</span></span>
<span data-ttu-id="7e429-185">Du kan ta bort en resurs eller en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7e429-185">You can remove a resource or resource group.</span></span> <span data-ttu-id="7e429-186">När du tar bort en resursgrupp kan du också ta bort alla resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7e429-186">When you remove a resource group, you also remove all the resources within that resource group.</span></span>

* <span data-ttu-id="7e429-187">Ta bort en resurs från resursgruppen genom att använda den **ta bort AzureRmResource** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7e429-187">To delete a resource from the resource group, use the **Remove-AzureRmResource** cmdlet.</span></span> <span data-ttu-id="7e429-188">Den här cmdleten tar bort resursen, men tar inte bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7e429-188">This cmdlet deletes the resource, but does not delete the resource group.</span></span>

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* <span data-ttu-id="7e429-189">Ta bort en resursgrupp och alla dess resurser genom att använda den **Remove-AzureRmResourceGroup** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7e429-189">To delete a resource group and all its resources, use the **Remove-AzureRmResourceGroup** cmdlet.</span></span>

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

<span data-ttu-id="7e429-190">För båda cmdlets uppmanas du att bekräfta att du vill ta bort resurs eller resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7e429-190">For both cmdlets, you are asked to confirm that you wish to remove the resource or resource group.</span></span> <span data-ttu-id="7e429-191">Om åtgärden har tar bort en resurs eller en resursgrupp, returnerar **SANT**.</span><span class="sxs-lookup"><span data-stu-id="7e429-191">If the operation successfully deletes the resource or resource group, it returns **True**.</span></span>

## <a name="run-resource-manager-scripts-with-azure-automation"></a><span data-ttu-id="7e429-192">Kör skript för Resource Manager med Azure Automation</span><span class="sxs-lookup"><span data-stu-id="7e429-192">Run Resource Manager scripts with Azure Automation</span></span>

<span data-ttu-id="7e429-193">Det här avsnittet visar hur du utför grundläggande åtgärder på resurser med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7e429-193">This topic shows you how to perform basic operations on your resources with Azure PowerShell.</span></span> <span data-ttu-id="7e429-194">För mer avancerade scenarier vill du förmodligen att skapa ett skript och återanvända skriptet efter behov eller enligt ett schema.</span><span class="sxs-lookup"><span data-stu-id="7e429-194">For more advanced management scenarios, you typically want to create a script, and reuse that script as needed or on a schedule.</span></span> <span data-ttu-id="7e429-195">[Azure Automation](../automation/automation-intro.md) ger ett sätt att automatisera vanliga skript som hanterar dina Azure-lösningar som används.</span><span class="sxs-lookup"><span data-stu-id="7e429-195">[Azure Automation](../automation/automation-intro.md) provides a way for you to automate frequently used scripts that manage your Azure solutions.</span></span>

<span data-ttu-id="7e429-196">De följande avsnitten visar hur du använder Azure Automation, Resource Manager och PowerShell för att effektivt utföra administrativa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="7e429-196">The following topics show you how to use Azure Automation, Resource Manager, and PowerShell to effectively perform management tasks:</span></span>

- <span data-ttu-id="7e429-197">Information om hur du skapar en runbook finns [min första PowerShell-runbook](../automation/automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7e429-197">For information about creating a runbook, see [My first PowerShell runbook](../automation/automation-first-runbook-textual-powershell.md).</span></span>
- <span data-ttu-id="7e429-198">Information om hur du arbetar med gallerier av skript finns [Azure Automation Runbook- och stänga](../automation/automation-runbook-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="7e429-198">For information about working with galleries of scripts, see [Runbook and module galleries for Azure Automation](../automation/automation-runbook-gallery.md).</span></span>
- <span data-ttu-id="7e429-199">Runbooks som kan starta och stoppa virtuella datorer, se [Azure Automation-scenario: använda JSON-formaterad taggar om du vill skapa ett schema för Virtuella Azure-start och stopp](../automation/automation-scenario-start-stop-vm-wjson-tags.md).</span><span class="sxs-lookup"><span data-stu-id="7e429-199">For runbooks that start and stop virtual machines, see [Azure Automation scenario: Using JSON-formatted tags to create a schedule for Azure VM startup and shutdown](../automation/automation-scenario-start-stop-vm-wjson-tags.md).</span></span>
- <span data-ttu-id="7e429-200">Runbooks som kan starta och stoppa virtuella datorer låg belastning på nätverket, se [Starta/stoppa virtuella datorer vid låg belastning på nätverket lösning i Automation](../automation/automation-solution-vm-management.md).</span><span class="sxs-lookup"><span data-stu-id="7e429-200">For runbooks that start and stop virtual machines off-hours, see [Start/Stop VMs during off-hours solution in Automation](../automation/automation-solution-vm-management.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e429-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e429-201">Next steps</span></span>
* <span data-ttu-id="7e429-202">Läs om hur du skapar Resource Manager-mallar i [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="7e429-202">To learn about creating Resource Manager templates, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="7e429-203">Läs om hur du distribuerar mallar i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="7e429-203">To learn about deploying templates, see [Deploy an application with Azure Resource Manager Template](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="7e429-204">Du kan flytta befintliga resurser till en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7e429-204">You can move existing resources to a new resource group.</span></span> <span data-ttu-id="7e429-205">Exempel finns i [flytta resurser till ny resursgrupp eller prenumeration](resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="7e429-205">For examples, see [Move Resources to New Resource Group or Subscription](resource-group-move-resources.md).</span></span>
* <span data-ttu-id="7e429-206">Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="7e429-206">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

