---
title: "aaaManage Azure lösningar med PowerShell | Microsoft Docs"
description: "Använda Azure PowerShell och Resource Manager toomanage dina resurser."
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
ms.openlocfilehash: 47a91af8d7eb59585bcfd43571ce76b90a0d7971
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a><span data-ttu-id="70d42-103">Hantera resurser med Azure PowerShell och Resource Manager</span><span class="sxs-lookup"><span data-stu-id="70d42-103">Manage resources with Azure PowerShell and Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="70d42-104">Portal</span><span class="sxs-lookup"><span data-stu-id="70d42-104">Portal</span></span>](resource-group-portal.md)
> * [<span data-ttu-id="70d42-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="70d42-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="70d42-106">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="70d42-106">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="70d42-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="70d42-107">REST API</span></span>](resource-manager-rest-api.md)
>
>

<span data-ttu-id="70d42-108">I den här artikeln får du lära dig hur toomanage dina lösningar med Azure PowerShell och Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="70d42-108">In this article, you learn how toomanage your solutions with Azure PowerShell and Azure Resource Manager.</span></span> <span data-ttu-id="70d42-109">Om du inte är bekant med Resource Manager finns [översikt över Resource Manager](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="70d42-109">If you are not familiar with Resource Manager, see [Resource Manager Overview](resource-group-overview.md).</span></span> <span data-ttu-id="70d42-110">Det här avsnittet fokuserar på hanteringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="70d42-110">This topic focuses on management tasks.</span></span> <span data-ttu-id="70d42-111">Du kommer att:</span><span class="sxs-lookup"><span data-stu-id="70d42-111">You will:</span></span>

1. <span data-ttu-id="70d42-112">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="70d42-112">Create a resource group</span></span>
2. <span data-ttu-id="70d42-113">Lägg till en resurs toohello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="70d42-113">Add a resource toohello resource group</span></span>
3. <span data-ttu-id="70d42-114">Lägg till en tagg toohello resurs</span><span class="sxs-lookup"><span data-stu-id="70d42-114">Add a tag toohello resource</span></span>
4. <span data-ttu-id="70d42-115">Fråga resurser baserat på namn eller värden</span><span class="sxs-lookup"><span data-stu-id="70d42-115">Query resources based on names or tag values</span></span>
5. <span data-ttu-id="70d42-116">Använda och ta bort ett lås på hello resurs</span><span class="sxs-lookup"><span data-stu-id="70d42-116">Apply and remove a lock on hello resource</span></span>
6. <span data-ttu-id="70d42-117">Ta bort en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="70d42-117">Delete a resource group</span></span>

<span data-ttu-id="70d42-118">Den här artikeln visar inte hur toodeploy en Resource Manager mallen tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="70d42-118">This article does not show how toodeploy a Resource Manager template tooyour subscription.</span></span> <span data-ttu-id="70d42-119">Den här informationen finns [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="70d42-119">For that information, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>

## <a name="get-started-with-azure-powershell"></a><span data-ttu-id="70d42-120">Kom igång med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="70d42-120">Get started with Azure PowerShell</span></span>

<span data-ttu-id="70d42-121">Om du inte har installerat Azure PowerShell, se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="70d42-121">If you have not installed Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="70d42-122">Överväg att installera hello senaste versionen om du har installerat Azure PowerShell i hello tidigare men inte har uppdaterat den nyligen.</span><span class="sxs-lookup"><span data-stu-id="70d42-122">If you have installed Azure PowerShell in hello past but have not updated it recently, consider installing hello latest version.</span></span> <span data-ttu-id="70d42-123">Du kan uppdatera hello version via hello samma metod som du använde tooinstall den.</span><span class="sxs-lookup"><span data-stu-id="70d42-123">You can update hello version through hello same method you used tooinstall it.</span></span> <span data-ttu-id="70d42-124">Om du använde hello installationsprogram för webbplattform, starta den igen och leta efter en uppdatering.</span><span class="sxs-lookup"><span data-stu-id="70d42-124">For example, if you used hello Web Platform Installer, launch it again and look for an update.</span></span>

<span data-ttu-id="70d42-125">toocheck din version av hello Azure-resurser modulen, använder hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="70d42-125">toocheck your version of hello Azure Resources module, use hello following cmdlet:</span></span>

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

<span data-ttu-id="70d42-126">Det här avsnittet har uppdaterats för version 3.3.0.</span><span class="sxs-lookup"><span data-stu-id="70d42-126">This topic was updated for version 3.3.0.</span></span> <span data-ttu-id="70d42-127">Om du har en tidigare version överensstämmer hello steg som visas i det här avsnittet inte med din upplevelse.</span><span class="sxs-lookup"><span data-stu-id="70d42-127">If you have an earlier version, your experience might not match hello steps shown in this topic.</span></span> <span data-ttu-id="70d42-128">Dokumentation om hello-cmdlets i den här versionen finns [AzureRM.Resources modulen](/powershell/module/azurerm.resources).</span><span class="sxs-lookup"><span data-stu-id="70d42-128">For documentation about hello cmdlets in this version, see [AzureRM.Resources Module](/powershell/module/azurerm.resources).</span></span>

## <a name="log-in-tooyour-azure-account"></a><span data-ttu-id="70d42-129">Logga in tooyour Azure-konto</span><span class="sxs-lookup"><span data-stu-id="70d42-129">Log in tooyour Azure account</span></span>
<span data-ttu-id="70d42-130">Du måste logga in tooyour konto innan du arbetar med din lösning.</span><span class="sxs-lookup"><span data-stu-id="70d42-130">Before working on your solution, you must log in tooyour account.</span></span>

<span data-ttu-id="70d42-131">toolog i tooyour Azure-konto, använder hello **Login-AzureRmAccount** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="70d42-131">toolog in tooyour Azure account, use hello **Login-AzureRmAccount** cmdlet.</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="70d42-132">hello cmdlet efterfrågar hello inloggningsuppgifterna för ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="70d42-132">hello cmdlet prompts you for hello login credentials for your Azure account.</span></span> <span data-ttu-id="70d42-133">När du loggar in hämtar den inställningarna för ditt konto, så att de är tillgängliga tooAzure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="70d42-133">After logging in, it downloads your account settings so they are available tooAzure PowerShell.</span></span>

<span data-ttu-id="70d42-134">hello cmdlet returnerar information om ditt konto och hello prenumeration toouse för hello uppgifter.</span><span class="sxs-lookup"><span data-stu-id="70d42-134">hello cmdlet returns information about your account and hello subscription toouse for hello tasks.</span></span>

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

<span data-ttu-id="70d42-135">Om du har mer än en prenumeration kan växla du tooa annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="70d42-135">If you have more than one subscription, you can switch tooa different subscription.</span></span> <span data-ttu-id="70d42-136">Först ska vi se alla hello prenumerationer för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="70d42-136">First, let's see all hello subscriptions for your account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="70d42-137">Den returnerar aktiverade och inaktiverade prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="70d42-137">It returns enabled and disabled subscriptions.</span></span>

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

<span data-ttu-id="70d42-138">tooswitch tooa annan prenumeration, ge hello prenumerationsnamn hello **Set-AzureRmContext** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="70d42-138">tooswitch tooa different subscription, provide hello subscription name with hello **Set-AzureRmContext** cmdlet.</span></span>

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a><span data-ttu-id="70d42-139">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="70d42-139">Create a resource group</span></span>
<span data-ttu-id="70d42-140">Innan du distribuerar någon resurser tooyour prenumeration, måste du skapa en resursgrupp som innehåller hello resurser.</span><span class="sxs-lookup"><span data-stu-id="70d42-140">Before deploying any resources tooyour subscription, you must create a resource group that will contain hello resources.</span></span>

<span data-ttu-id="70d42-141">toocreate en resursgrupp, använda hello **New-AzureRmResourceGroup** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="70d42-141">toocreate a resource group, use hello **New-AzureRmResourceGroup** cmdlet.</span></span> <span data-ttu-id="70d42-142">hello kommando använder hello **namn** parametern toospecify ett namn för resursgruppen hello och hello **plats** parametern toospecify dess plats.</span><span class="sxs-lookup"><span data-stu-id="70d42-142">hello command uses hello **Name** parameter toospecify a name for hello resource group and hello **Location** parameter toospecify its location.</span></span>

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

<span data-ttu-id="70d42-143">hello utdata har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="70d42-143">hello output is in hello following format:</span></span>

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

<span data-ttu-id="70d42-144">Om du behöver tooretrieve hello resursgruppen senare, Använd hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="70d42-144">If you need tooretrieve hello resource group later, use hello following cmdlet:</span></span>

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

<span data-ttu-id="70d42-145">tooget alla Hej resursgrupper i din prenumeration, ange inte ett namn:</span><span class="sxs-lookup"><span data-stu-id="70d42-145">tooget all hello resource groups in your subscription, do not specify a name:</span></span>

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-tooa-resource-group"></a><span data-ttu-id="70d42-146">Lägg till resurser tooa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="70d42-146">Add resources tooa resource group</span></span>
<span data-ttu-id="70d42-147">tooadd en resurs toohello resursgrupp som du kan använda hello **ny AzureRmResource** cmdlet eller en cmdlet som är specifika toohello typ av resurs som du skapar (t.ex. **New-AzureRmStorageAccount**).</span><span class="sxs-lookup"><span data-stu-id="70d42-147">tooadd a resource toohello resource group, you can use hello **New-AzureRmResource** cmdlet or a cmdlet that is specific toohello type of resource you are creating (like **New-AzureRmStorageAccount**).</span></span> <span data-ttu-id="70d42-148">Det kan vara enklare toouse en cmdlet som är specifika tooa resurstyp eftersom den innehåller parametrar för hello egenskaper som krävs för hello ny resurs.</span><span class="sxs-lookup"><span data-stu-id="70d42-148">You might find it easier toouse a cmdlet that is specific tooa resource type because it includes parameters for hello properties that are needed for hello new resource.</span></span> <span data-ttu-id="70d42-149">toouse **ny AzureRmResource**, måste du känna till alla hello egenskaper tooset utan som efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="70d42-149">toouse **New-AzureRmResource**, you must know all hello properties tooset without being prompted for them.</span></span>

<span data-ttu-id="70d42-150">Lägga till en resurs via cmdlets kan vara förvirrande för framtida hello ny resurs finns inte i en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="70d42-150">However, adding a resource through cmdlets might cause future confusion because hello new resource does not exist in a Resource Manager template.</span></span> <span data-ttu-id="70d42-151">Microsoft rekommenderar att definiera hello infrastrukturen för Azure lösningen i en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="70d42-151">Microsoft recommends defining hello infrastructure for your Azure solution in a Resource Manager template.</span></span> <span data-ttu-id="70d42-152">Mallar kan du tooreliably och upprepade gånger distribuera din lösning.</span><span class="sxs-lookup"><span data-stu-id="70d42-152">Templates enable you tooreliably and repeatedly deploy your solution.</span></span> <span data-ttu-id="70d42-153">Du skapar ett lagringskonto med en PowerShell-cmdlet för det här avsnittet, men senare du generera en mall från resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="70d42-153">For this topic, you create a storage account with a PowerShell cmdlet, but later you generate a template from your resource group.</span></span>

<span data-ttu-id="70d42-154">hello följande cmdlet skapar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="70d42-154">hello following cmdlet creates a storage account.</span></span> <span data-ttu-id="70d42-155">Ange ett unikt namn för hello storage-konto istället för att använda hello-namnet som visas i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="70d42-155">Instead of using hello name shown in hello example, provide a unique name for hello storage account.</span></span> <span data-ttu-id="70d42-156">hello namn måste vara mellan 3 och 24 tecken långt och innehålla endast siffror och gemener.</span><span class="sxs-lookup"><span data-stu-id="70d42-156">hello name must be between 3 and 24 characters in length, and use only numbers and lower-case letters.</span></span> <span data-ttu-id="70d42-157">Om du använder hello-namnet som visas i hello exempel får ett felmeddelande eftersom namnet redan används.</span><span class="sxs-lookup"><span data-stu-id="70d42-157">If you use hello name shown in hello example, you receive an error because that name is already in use.</span></span>

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

<span data-ttu-id="70d42-158">Om du behöver tooretrieve resursen senare, Använd hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="70d42-158">If you need tooretrieve this resource later, use hello following cmdlet:</span></span>

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a><span data-ttu-id="70d42-159">Lägga till en tagg</span><span class="sxs-lookup"><span data-stu-id="70d42-159">Add a tag</span></span>

<span data-ttu-id="70d42-160">Taggar kan du tooorganize dina resurser enligt toodifferent egenskaper.</span><span class="sxs-lookup"><span data-stu-id="70d42-160">Tags enable you tooorganize your resources according toodifferent properties.</span></span> <span data-ttu-id="70d42-161">Du kan till exempel har flera resurser i olika resursgrupper som tillhör toohello samma avdelning.</span><span class="sxs-lookup"><span data-stu-id="70d42-161">For example, you may have several resources in different resource groups that belong toohello same department.</span></span> <span data-ttu-id="70d42-162">Du kan tillämpa en avdelning taggen och värdet toothose resurser toomark dem som tillhör toohello samma kategori.</span><span class="sxs-lookup"><span data-stu-id="70d42-162">You can apply a department tag and value toothose resources toomark them as belonging toohello same category.</span></span> <span data-ttu-id="70d42-163">Eller så kan du markera om en resurs används i en produktionsmiljö eller testmiljö.</span><span class="sxs-lookup"><span data-stu-id="70d42-163">Or, you can mark whether a resource is used in a production or test environment.</span></span> <span data-ttu-id="70d42-164">Du använder taggar tooonly en resurs i det här avsnittet, men i din miljö är det mest sannolika klokt tooapply taggar tooall dina resurser.</span><span class="sxs-lookup"><span data-stu-id="70d42-164">In this topic, you apply tags tooonly one resource, but in your environment it most likely makes sense tooapply tags tooall your resources.</span></span>

<span data-ttu-id="70d42-165">följande cmdlet hello gäller två taggar tooyour storage-konto:</span><span class="sxs-lookup"><span data-stu-id="70d42-165">hello following cmdlet applies two tags tooyour storage account:</span></span>

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

<span data-ttu-id="70d42-166">Taggar uppdateras som ett enskilt objekt.</span><span class="sxs-lookup"><span data-stu-id="70d42-166">Tags are updated as a single object.</span></span> <span data-ttu-id="70d42-167">tooadd en tagg tooa resurs som redan omfattar taggar, först hämta hello befintliga taggar.</span><span class="sxs-lookup"><span data-stu-id="70d42-167">tooadd a tag tooa resource that already includes tags, first retrieve hello existing tags.</span></span> <span data-ttu-id="70d42-168">Lägg till hello ny tagg toohello objekt som innehåller hello befintliga taggar och tillämpa alla hello taggar toohello resurs.</span><span class="sxs-lookup"><span data-stu-id="70d42-168">Add hello new tag toohello object that contains hello existing tags, and reapply all hello tags toohello resource.</span></span>

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a><span data-ttu-id="70d42-169">Sök efter resurser</span><span class="sxs-lookup"><span data-stu-id="70d42-169">Search for resources</span></span>

<span data-ttu-id="70d42-170">Använd hello **hitta AzureRmResource** cmdlet tooretrieve resurser för olika sökvillkor.</span><span class="sxs-lookup"><span data-stu-id="70d42-170">Use hello **Find-AzureRmResource** cmdlet tooretrieve resources for different search conditions.</span></span>

* <span data-ttu-id="70d42-171">tooget en resurs med namnet, ange hello **ResourceNameContains** parameter:</span><span class="sxs-lookup"><span data-stu-id="70d42-171">tooget a resource by name, provide hello **ResourceNameContains** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* <span data-ttu-id="70d42-172">tooget alla hello resurser i en resursgrupp, ange hello **ResourceGroupNameContains** parameter:</span><span class="sxs-lookup"><span data-stu-id="70d42-172">tooget all hello resources in a resource group, provide hello **ResourceGroupNameContains** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* <span data-ttu-id="70d42-173">tooget alla hello resurser med taggnamn och värde, ange hello **TagName** och **TagValue** parametrar:</span><span class="sxs-lookup"><span data-stu-id="70d42-173">tooget all hello resources with a tag name and value, provide hello **TagName** and **TagValue** parameters:</span></span>

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* <span data-ttu-id="70d42-174">tooall hello resurser med en viss resurstyp innehåller hello **ResourceType** parameter:</span><span class="sxs-lookup"><span data-stu-id="70d42-174">tooall hello resources with a particular resource type, provide hello **ResourceType** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="lock-a-resource"></a><span data-ttu-id="70d42-175">Låsa en resurs</span><span class="sxs-lookup"><span data-stu-id="70d42-175">Lock a resource</span></span>

<span data-ttu-id="70d42-176">När du behöver toomake att en kritisk resurs tas inte bort av misstag eller ändras kan tillämpa en toohello lock-resurs.</span><span class="sxs-lookup"><span data-stu-id="70d42-176">When you need toomake sure a critical resource is not accidentally deleted or modified, apply a lock toohello resource.</span></span> <span data-ttu-id="70d42-177">Du kan ange antingen en **CanNotDelete** eller **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="70d42-177">You can specify either a **CanNotDelete** or **ReadOnly**.</span></span>

<span data-ttu-id="70d42-178">toocreate eller ta bort lås för hantering, måste du ha tillgång för`Microsoft.Authorization/*` eller `Microsoft.Authorization/locks/*` åtgärder.</span><span class="sxs-lookup"><span data-stu-id="70d42-178">toocreate or delete management locks, you must have access too`Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions.</span></span> <span data-ttu-id="70d42-179">I hello inbyggda roller beviljas endast ägare och administratör för användaråtkomst dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="70d42-179">Of hello built-in roles, only Owner and User Access Administrator are granted those actions.</span></span>

<span data-ttu-id="70d42-180">tooapply ett lås Använd hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="70d42-180">tooapply a lock, use hello following cmdlet:</span></span>

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

<span data-ttu-id="70d42-181">hello kan inte låst resurs i föregående exempel hello tas bort förrän hello låset tas bort.</span><span class="sxs-lookup"><span data-stu-id="70d42-181">hello locked resource in hello preceding example cannot be deleted until hello lock is removed.</span></span> <span data-ttu-id="70d42-182">Använd tooremove ett lås:</span><span class="sxs-lookup"><span data-stu-id="70d42-182">tooremove a lock, use:</span></span>

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

<span data-ttu-id="70d42-183">Mer information om inställningen Lås finns [låsa resurser med Azure Resource Manager](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="70d42-183">For more information about setting locks, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

## <a name="remove-resources-or-resource-group"></a><span data-ttu-id="70d42-184">Ta bort resurser eller resursgrupp</span><span class="sxs-lookup"><span data-stu-id="70d42-184">Remove resources or resource group</span></span>
<span data-ttu-id="70d42-185">Du kan ta bort en resurs eller en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="70d42-185">You can remove a resource or resource group.</span></span> <span data-ttu-id="70d42-186">När du tar bort en resursgrupp kan du också ta bort alla hello resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="70d42-186">When you remove a resource group, you also remove all hello resources within that resource group.</span></span>

* <span data-ttu-id="70d42-187">toodelete en resurs från hello resursgrupp, Använd hello **ta bort AzureRmResource** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="70d42-187">toodelete a resource from hello resource group, use hello **Remove-AzureRmResource** cmdlet.</span></span> <span data-ttu-id="70d42-188">Denna cmdlet tar bort hello resurs, men tar inte bort hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="70d42-188">This cmdlet deletes hello resource, but does not delete hello resource group.</span></span>

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* <span data-ttu-id="70d42-189">toodelete en resursgrupp och alla dess resurser använder hello **Remove-AzureRmResourceGroup** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="70d42-189">toodelete a resource group and all its resources, use hello **Remove-AzureRmResourceGroup** cmdlet.</span></span>

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

<span data-ttu-id="70d42-190">För båda cmdlets uppmanas tooconfirm du vill tooremove hello resurs eller resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="70d42-190">For both cmdlets, you are asked tooconfirm that you wish tooremove hello resource or resource group.</span></span> <span data-ttu-id="70d42-191">Om hello har tas bort hello resurs eller resursgrupp, returnerar **SANT**.</span><span class="sxs-lookup"><span data-stu-id="70d42-191">If hello operation successfully deletes hello resource or resource group, it returns **True**.</span></span>

## <a name="run-resource-manager-scripts-with-azure-automation"></a><span data-ttu-id="70d42-192">Kör skript för Resource Manager med Azure Automation</span><span class="sxs-lookup"><span data-stu-id="70d42-192">Run Resource Manager scripts with Azure Automation</span></span>

<span data-ttu-id="70d42-193">Det här avsnittet beskrivs hur du tooperform grundläggande åtgärder på resurser med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="70d42-193">This topic shows you how tooperform basic operations on your resources with Azure PowerShell.</span></span> <span data-ttu-id="70d42-194">Mer avancerade scenarier med hantering av du vanligtvis vill toocreate ett skript och återanvända skriptet efter behov eller enligt ett schema.</span><span class="sxs-lookup"><span data-stu-id="70d42-194">For more advanced management scenarios, you typically want toocreate a script, and reuse that script as needed or on a schedule.</span></span> <span data-ttu-id="70d42-195">[Azure Automation](../automation/automation-intro.md) gör det möjligt för dig tooautomate vanliga skript som hanterar dina Azure-lösningar.</span><span class="sxs-lookup"><span data-stu-id="70d42-195">[Azure Automation](../automation/automation-intro.md) provides a way for you tooautomate frequently used scripts that manage your Azure solutions.</span></span>

<span data-ttu-id="70d42-196">hello visar följande avsnitt hur toouse Azure Automation Resource Manager och PowerShell tooeffectively utföra administrativa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="70d42-196">hello following topics show you how toouse Azure Automation, Resource Manager, and PowerShell tooeffectively perform management tasks:</span></span>

- <span data-ttu-id="70d42-197">Information om hur du skapar en runbook finns [min första PowerShell-runbook](../automation/automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="70d42-197">For information about creating a runbook, see [My first PowerShell runbook](../automation/automation-first-runbook-textual-powershell.md).</span></span>
- <span data-ttu-id="70d42-198">Information om hur du arbetar med gallerier av skript finns [Azure Automation Runbook- och stänga](../automation/automation-runbook-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="70d42-198">For information about working with galleries of scripts, see [Runbook and module galleries for Azure Automation](../automation/automation-runbook-gallery.md).</span></span>
- <span data-ttu-id="70d42-199">Runbooks som kan starta och stoppa virtuella datorer, se [Azure Automation-scenario: använda JSON-formaterad taggar toocreate ett schema för Virtuella Azure-start och stopp](../automation/automation-scenario-start-stop-vm-wjson-tags.md).</span><span class="sxs-lookup"><span data-stu-id="70d42-199">For runbooks that start and stop virtual machines, see [Azure Automation scenario: Using JSON-formatted tags toocreate a schedule for Azure VM startup and shutdown](../automation/automation-scenario-start-stop-vm-wjson-tags.md).</span></span>
- <span data-ttu-id="70d42-200">Runbooks som kan starta och stoppa virtuella datorer låg belastning på nätverket, se [Starta/stoppa virtuella datorer vid låg belastning på nätverket lösning i Automation](../automation/automation-solution-vm-management.md).</span><span class="sxs-lookup"><span data-stu-id="70d42-200">For runbooks that start and stop virtual machines off-hours, see [Start/Stop VMs during off-hours solution in Automation](../automation/automation-solution-vm-management.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="70d42-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="70d42-201">Next steps</span></span>
* <span data-ttu-id="70d42-202">toolearn om hur du skapar Resource Manager-mallar finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="70d42-202">toolearn about creating Resource Manager templates, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="70d42-203">toolearn om hur du distribuerar mallar, se [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="70d42-203">toolearn about deploying templates, see [Deploy an application with Azure Resource Manager Template](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="70d42-204">Du kan flytta befintliga resurser tooa ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="70d42-204">You can move existing resources tooa new resource group.</span></span> <span data-ttu-id="70d42-205">Exempel finns i [flytta resurser tooNew resursgrupp eller prenumeration](resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="70d42-205">For examples, see [Move Resources tooNew Resource Group or Subscription](resource-group-move-resources.md).</span></span>
* <span data-ttu-id="70d42-206">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="70d42-206">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

