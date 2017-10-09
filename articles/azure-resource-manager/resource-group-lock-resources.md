---
title: "aaaLock Azure-resurser tooprevent ändringar | Microsoft Docs"
description: "Förhindra användare från att uppdatera eller ta bort viktiga Azure-resurser genom att använda ett lås för alla användare och roller."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 53c57e8f-741c-4026-80e0-f4c02638c98b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: 1f0d8911b2b129069bd2f01a9a4ec0a3cf700118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="lock-resources-tooprevent-unexpected-changes"></a><span data-ttu-id="68bd4-103">Låsa resurser tooprevent oväntade ändringar</span><span class="sxs-lookup"><span data-stu-id="68bd4-103">Lock resources tooprevent unexpected changes</span></span> 
<span data-ttu-id="68bd4-104">Som administratör kan behöva du toolock en prenumeration, resursgrupp eller resurs tooprevent andra användare i din organisation av misstag tas bort eller ändra viktiga resurser.</span><span class="sxs-lookup"><span data-stu-id="68bd4-104">As an administrator, you may need toolock a subscription, resource group, or resource tooprevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="68bd4-105">Du kan ange hello Lås nivå för**CanNotDelete** eller **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="68bd4-105">You can set hello lock level too**CanNotDelete** or **ReadOnly**.</span></span> 

* <span data-ttu-id="68bd4-106">**CanNotDelete** innebär behöriga användare kan läsa och ändra en resurs fortfarande, men de kan inte ta bort hello resurs.</span><span class="sxs-lookup"><span data-stu-id="68bd4-106">**CanNotDelete** means authorized users can still read and modify a resource, but they can't delete hello resource.</span></span> 
* <span data-ttu-id="68bd4-107">**ReadOnly** innebär att behöriga användare kan läsa en resurs, men de kan inte ta bort eller uppdatera hello resursen.</span><span class="sxs-lookup"><span data-stu-id="68bd4-107">**ReadOnly** means authorized users can read a resource, but they can't delete or update hello resource.</span></span> <span data-ttu-id="68bd4-108">Tillämpa den här Lås är liknande toorestricting alla behöriga användare toohello behörigheterna av hello **Reader** roll.</span><span class="sxs-lookup"><span data-stu-id="68bd4-108">Applying this lock is similar toorestricting all authorized users toohello permissions granted by hello **Reader** role.</span></span> 

## <a name="how-locks-are-applied"></a><span data-ttu-id="68bd4-109">Hur Lås tillämpas</span><span class="sxs-lookup"><span data-stu-id="68bd4-109">How locks are applied</span></span>

<span data-ttu-id="68bd4-110">När du använder ett lås på en överordnad omfattning, alla resurser inom detta scope ärver hello samma Lås.</span><span class="sxs-lookup"><span data-stu-id="68bd4-110">When you apply a lock at a parent scope, all resources within that scope inherit hello same lock.</span></span> <span data-ttu-id="68bd4-111">Även resurser som du senare lägger till Ärv hello överordnade hello Lås.</span><span class="sxs-lookup"><span data-stu-id="68bd4-111">Even resources you add later inherit hello lock from hello parent.</span></span> <span data-ttu-id="68bd4-112">hello mest restriktiva Lås i hello arv företräde.</span><span class="sxs-lookup"><span data-stu-id="68bd4-112">hello most restrictive lock in hello inheritance takes precedence.</span></span>

<span data-ttu-id="68bd4-113">Till skillnad från rollbaserad åtkomstkontroll använder du management Lås tooapply en begränsning för alla användare och roller.</span><span class="sxs-lookup"><span data-stu-id="68bd4-113">Unlike role-based access control, you use management locks tooapply a restriction across all users and roles.</span></span> <span data-ttu-id="68bd4-114">toolearn om att ange behörigheter för användare och roller, se [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="68bd4-114">toolearn about setting permissions for users and roles, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>

<span data-ttu-id="68bd4-115">Hanteraren för filserverresurser Lås gäller toooperations som sker i plan för hello, som består av åtgärder som skickas för`https://management.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="68bd4-115">Resource Manager locks apply only toooperations that happen in hello management plane, which consists of operations sent too`https://management.azure.com`.</span></span> <span data-ttu-id="68bd4-116">hello Lås begränsar inte hur resurser utföra sina egna funktioner.</span><span class="sxs-lookup"><span data-stu-id="68bd4-116">hello locks do not restrict how resources perform their own functions.</span></span> <span data-ttu-id="68bd4-117">Resursändringar är begränsad, men resursåtgärder är inte begränsade.</span><span class="sxs-lookup"><span data-stu-id="68bd4-117">Resource changes are restricted, but resource operations are not restricted.</span></span> <span data-ttu-id="68bd4-118">Till exempel en ReadOnly-lås på en SQL-databas hindrar dig från att ta bort eller ändra hello-databasen, men det hindrar dig inte från att skapa, uppdatera eller ta bort data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="68bd4-118">For example, a ReadOnly lock on a SQL Database prevents you from deleting or modifying hello database, but it does not prevent you from creating, updating, or deleting data in hello database.</span></span> <span data-ttu-id="68bd4-119">Data transaktioner tillåts eftersom dessa åtgärder inte skickas för`https://management.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="68bd4-119">Data transactions are permitted because those operations are not sent too`https://management.azure.com`.</span></span>

<span data-ttu-id="68bd4-120">Tillämpa **ReadOnly** kan leda till toounexpected resultat eftersom vissa åtgärder som ser ut som att läsa operations faktiskt kräver ytterligare åtgärder.</span><span class="sxs-lookup"><span data-stu-id="68bd4-120">Applying **ReadOnly** can lead toounexpected results because some operations that seem like read operations actually require additional actions.</span></span> <span data-ttu-id="68bd4-121">Till exempel placera en **ReadOnly** lås på ett lagringskonto som förhindrar att alla användare med hello nycklar.</span><span class="sxs-lookup"><span data-stu-id="68bd4-121">For example, placing a **ReadOnly** lock on a storage account prevents all users from listing hello keys.</span></span> <span data-ttu-id="68bd4-122">hello lista nycklar operationen hanteras via en POST-begäran eftersom hello returnerade nycklar är tillgängliga för skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="68bd4-122">hello list keys operation is handled through a POST request because hello returned keys are available for write operations.</span></span> <span data-ttu-id="68bd4-123">För ett annat exempel är att placera en **ReadOnly** lås på en App Service-resurs som förhindrar att Visual Studio Server Explorer visar filer för hello resursen eftersom den interaktionen kräver skrivåtkomst.</span><span class="sxs-lookup"><span data-stu-id="68bd4-123">For another example, placing a **ReadOnly** lock on an App Service resource prevents Visual Studio Server Explorer from displaying files for hello resource because that interaction requires write access.</span></span>

## <a name="who-can-create-or-delete-locks-in-your-organization"></a><span data-ttu-id="68bd4-124">Vem kan skapa eller ta bort lås i din organisation</span><span class="sxs-lookup"><span data-stu-id="68bd4-124">Who can create or delete locks in your organization</span></span>
<span data-ttu-id="68bd4-125">toocreate eller ta bort lås för hantering, måste du ha tillgång för`Microsoft.Authorization/*` eller `Microsoft.Authorization/locks/*` åtgärder.</span><span class="sxs-lookup"><span data-stu-id="68bd4-125">toocreate or delete management locks, you must have access too`Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions.</span></span> <span data-ttu-id="68bd4-126">I hello inbyggda roller, endast **ägare** och **administratör för användaråtkomst** beviljas dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="68bd4-126">Of hello built-in roles, only **Owner** and **User Access Administrator** are granted those actions.</span></span>

## <a name="portal"></a><span data-ttu-id="68bd4-127">Portalen</span><span class="sxs-lookup"><span data-stu-id="68bd4-127">Portal</span></span>
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a><span data-ttu-id="68bd4-128">Mall</span><span class="sxs-lookup"><span data-stu-id="68bd4-128">Template</span></span>
<span data-ttu-id="68bd4-129">hello visas följande exempel en mall som skapar ett lås på ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="68bd4-129">hello following example shows a template that creates a lock on a storage account.</span></span> <span data-ttu-id="68bd4-130">hello storage-konto på vilka tooapply hello Lås har angetts som en parameter.</span><span class="sxs-lookup"><span data-stu-id="68bd4-130">hello storage account on which tooapply hello lock is provided as a parameter.</span></span> <span data-ttu-id="68bd4-131">hello namnet på hello Lås skapas genom att sammanbinda hello resursnamnet med **/Microsoft.Authorization/** och hello namnet på hello Lås i det här fallet **myLock**.</span><span class="sxs-lookup"><span data-stu-id="68bd4-131">hello name of hello lock is created by concatenating hello resource name with **/Microsoft.Authorization/** and hello name of hello lock, in this case **myLock**.</span></span>

<span data-ttu-id="68bd4-132">hello-typ som är specifika toohello resurstyp.</span><span class="sxs-lookup"><span data-stu-id="68bd4-132">hello type provided is specific toohello resource type.</span></span> <span data-ttu-id="68bd4-133">(Ange hello typen too"Microsoft.Storage/storageaccounts/providers/locks för lagring”,.</span><span class="sxs-lookup"><span data-stu-id="68bd4-133">For storage, set hello type too"Microsoft.Storage/storageaccounts/providers/locks".</span></span>

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="powershell"></a><span data-ttu-id="68bd4-134">PowerShell</span><span class="sxs-lookup"><span data-stu-id="68bd4-134">PowerShell</span></span>
<span data-ttu-id="68bd4-135">Du Lås distribueras resurser med Azure PowerShell med hjälp av hello [ny AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) kommando.</span><span class="sxs-lookup"><span data-stu-id="68bd4-135">You lock deployed resources with Azure PowerShell by using hello [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) command.</span></span>

<span data-ttu-id="68bd4-136">toolock en resurs, ange hello namnet på hello resursen och dess resurstypen dess resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="68bd4-136">toolock a resource, provide hello name of hello resource, its resource type, and its resource group name.</span></span>

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite `
  -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="68bd4-137">toolock en resursgrupp, ange hello namn hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="68bd4-137">toolock a resource group, provide hello name of hello resource group.</span></span>

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="68bd4-138">tooget information om ett lås används [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock).</span><span class="sxs-lookup"><span data-stu-id="68bd4-138">tooget information about a lock, use [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock).</span></span> <span data-ttu-id="68bd4-139">tooget alla hello Lås i din prenumeration, Använd:</span><span class="sxs-lookup"><span data-stu-id="68bd4-139">tooget all hello locks in your subscription, use:</span></span>

```powershell
Get-AzureRmResourceLock
```

<span data-ttu-id="68bd4-140">tooget alla låsningar för en resurs, Använd:</span><span class="sxs-lookup"><span data-stu-id="68bd4-140">tooget all locks for a resource, use:</span></span>

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="68bd4-141">tooget alla Lås för en resursgrupp, Använd:</span><span class="sxs-lookup"><span data-stu-id="68bd4-141">tooget all locks for a resource group, use:</span></span>

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="68bd4-142">Azure PowerShell innehåller andra kommandon för att arbeta lås som [Set AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) tooupdate ett lås och [ta bort AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) toodelete ett lås.</span><span class="sxs-lookup"><span data-stu-id="68bd4-142">Azure PowerShell provides other commands for working locks, such as [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) tooupdate a lock, and [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) toodelete a lock.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="68bd4-143">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="68bd4-143">Azure CLI</span></span>

<span data-ttu-id="68bd4-144">Du Lås distribueras resurser med Azure CLI med hjälp av hello [az Lås skapa](/cli/azure/lock#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="68bd4-144">You lock deployed resources with Azure CLI by using hello [az lock create](/cli/azure/lock#create) command.</span></span>

<span data-ttu-id="68bd4-145">toolock en resurs, ange hello namnet på hello resursen och dess resurstypen dess resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="68bd4-145">toolock a resource, provide hello name of hello resource, its resource type, and its resource group name.</span></span>

```azurecli
az lock create --name LockSite --lock-type CanNotDelete \
  --resource-group exampleresourcegroup --resource-name examplesite \
  --resource-type Microsoft.Web/sites
```

<span data-ttu-id="68bd4-146">toolock en resursgrupp, ange hello namn hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="68bd4-146">toolock a resource group, provide hello name of hello resource group.</span></span>

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete \
  --resource-group exampleresourcegroup
```

<span data-ttu-id="68bd4-147">tooget information om ett lås används [az Lås listan](/cli/azure/lock#list).</span><span class="sxs-lookup"><span data-stu-id="68bd4-147">tooget information about a lock, use [az lock list](/cli/azure/lock#list).</span></span> <span data-ttu-id="68bd4-148">tooget alla hello Lås i din prenumeration, Använd:</span><span class="sxs-lookup"><span data-stu-id="68bd4-148">tooget all hello locks in your subscription, use:</span></span>

```azurecli
az lock list
```

<span data-ttu-id="68bd4-149">tooget alla låsningar för en resurs, Använd:</span><span class="sxs-lookup"><span data-stu-id="68bd4-149">tooget all locks for a resource, use:</span></span>

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite \
  --namespace Microsoft.Web --resource-type sites --parent ""
```

<span data-ttu-id="68bd4-150">tooget alla Lås för en resursgrupp, Använd:</span><span class="sxs-lookup"><span data-stu-id="68bd4-150">tooget all locks for a resource group, use:</span></span>

```azurecli
az lock list --resource-group exampleresourcegroup
```

<span data-ttu-id="68bd4-151">Azure CLI finns andra kommandon för att arbeta lås som [az Lås uppdatering](/cli/azure/lock#update) tooupdate ett lås och [az Lås ta bort](/cli/azure/lock#delete) toodelete ett lås.</span><span class="sxs-lookup"><span data-stu-id="68bd4-151">Azure CLI provides other commands for working locks, such as [az lock update](/cli/azure/lock#update) tooupdate a lock, and [az lock delete](/cli/azure/lock#delete) toodelete a lock.</span></span>

## <a name="rest-api"></a><span data-ttu-id="68bd4-152">REST API</span><span class="sxs-lookup"><span data-stu-id="68bd4-152">REST API</span></span>
<span data-ttu-id="68bd4-153">Du kan låsa distribuerade resurser med hello [REST API för hantering av Lås](https://docs.microsoft.com/rest/api/resources/managementlocks).</span><span class="sxs-lookup"><span data-stu-id="68bd4-153">You can lock deployed resources with hello [REST API for management locks](https://docs.microsoft.com/rest/api/resources/managementlocks).</span></span> <span data-ttu-id="68bd4-154">hello REST-API kan du toocreate och ta bort lås och hämta information om befintliga Lås.</span><span class="sxs-lookup"><span data-stu-id="68bd4-154">hello REST API enables you toocreate and delete locks, and retrieve information about existing locks.</span></span>

<span data-ttu-id="68bd4-155">toocreate ett lås som kör:</span><span class="sxs-lookup"><span data-stu-id="68bd4-155">toocreate a lock, run:</span></span>

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

<span data-ttu-id="68bd4-156">hello scope kan vara en prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="68bd4-156">hello scope could be a subscription, resource group, or resource.</span></span> <span data-ttu-id="68bd4-157">hello lock-namn är vad du vill toocall hello Lås.</span><span class="sxs-lookup"><span data-stu-id="68bd4-157">hello lock-name is whatever you want toocall hello lock.</span></span> <span data-ttu-id="68bd4-158">Api-versionen, Använd **2015-01-01**.</span><span class="sxs-lookup"><span data-stu-id="68bd4-158">For api-version, use **2015-01-01**.</span></span>

<span data-ttu-id="68bd4-159">Inkludera en JSON-objekt som anger hello egenskaperna för hello Lås i hello begäran.</span><span class="sxs-lookup"><span data-stu-id="68bd4-159">In hello request, include a JSON object that specifies hello properties for hello lock.</span></span>

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a><span data-ttu-id="68bd4-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68bd4-160">Next steps</span></span>
* <span data-ttu-id="68bd4-161">Mer information om hur du arbetar med resurslås finns [Lås ned din Azure-resurser](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)</span><span class="sxs-lookup"><span data-stu-id="68bd4-161">For more information about working with resource locks, see [Lock Down Your Azure Resources](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)</span></span>
* <span data-ttu-id="68bd4-162">toolearn logiskt organisera dina resurser, se [med hjälp av taggar tooorganize dina resurser](resource-group-using-tags.md)</span><span class="sxs-lookup"><span data-stu-id="68bd4-162">toolearn about logically organizing your resources, see [Using tags tooorganize your resources](resource-group-using-tags.md)</span></span>
* <span data-ttu-id="68bd4-163">toochange vilken resursgrupp som en resurs finns i, se [flytta resurser toonew resursgruppen.](resource-group-move-resources.md)</span><span class="sxs-lookup"><span data-stu-id="68bd4-163">toochange which resource group a resource resides in, see [Move resources toonew resource group](resource-group-move-resources.md)</span></span>
* <span data-ttu-id="68bd4-164">Du kan tillämpa begränsningar och konventioner över din prenumeration med anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="68bd4-164">You can apply restrictions and conventions across your subscription with customized policies.</span></span> <span data-ttu-id="68bd4-165">Mer information finns i [använda princip toomanage resurser och kontrollera åtkomst](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="68bd4-165">For more information, see [Use Policy toomanage resources and control access](resource-manager-policy.md).</span></span>
* <span data-ttu-id="68bd4-166">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="68bd4-166">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

