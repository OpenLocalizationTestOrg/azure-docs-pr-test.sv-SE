---
title: "Låsa Azure-resurser ska kunna ändras | Microsoft Docs"
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
ms.openlocfilehash: 44c87b00f4fc63dbfd45a07d9a8cddc5eaf1a65c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a><span data-ttu-id="70eff-103">Låsa resurser för att förhindra oväntade ändringar</span><span class="sxs-lookup"><span data-stu-id="70eff-103">Lock resources to prevent unexpected changes</span></span> 
<span data-ttu-id="70eff-104">Som administratör kan behöva du låsa en prenumeration, resursgrupp eller resurs för att förhindra andra användare i din organisation av misstag tas bort eller ändra viktiga resurser.</span><span class="sxs-lookup"><span data-stu-id="70eff-104">As an administrator, you may need to lock a subscription, resource group, or resource to prevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="70eff-105">Du kan ange låset för **CanNotDelete** eller **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="70eff-105">You can set the lock level to **CanNotDelete** or **ReadOnly**.</span></span> 

* <span data-ttu-id="70eff-106">**CanNotDelete** innebär behöriga användare kan läsa och ändra en resurs fortfarande, men de kan inte ta bort resursen.</span><span class="sxs-lookup"><span data-stu-id="70eff-106">**CanNotDelete** means authorized users can still read and modify a resource, but they can't delete the resource.</span></span> 
* <span data-ttu-id="70eff-107">**ReadOnly** innebär att behöriga användare kan läsa en resurs, men de kan inte ta bort eller uppdatera resursen.</span><span class="sxs-lookup"><span data-stu-id="70eff-107">**ReadOnly** means authorized users can read a resource, but they can't delete or update the resource.</span></span> <span data-ttu-id="70eff-108">Tillämpa den här Lås liknar begränsa alla behöriga användare till de behörigheter som utfärdats av den **Reader** roll.</span><span class="sxs-lookup"><span data-stu-id="70eff-108">Applying this lock is similar to restricting all authorized users to the permissions granted by the **Reader** role.</span></span> 

## <a name="how-locks-are-applied"></a><span data-ttu-id="70eff-109">Hur Lås tillämpas</span><span class="sxs-lookup"><span data-stu-id="70eff-109">How locks are applied</span></span>

<span data-ttu-id="70eff-110">När du använder ett lås på en överordnad omfattning, ärver alla resurser i omfattningen samma låset.</span><span class="sxs-lookup"><span data-stu-id="70eff-110">When you apply a lock at a parent scope, all resources within that scope inherit the same lock.</span></span> <span data-ttu-id="70eff-111">Även resurser som du senare lägger till ärver låset från överordnat.</span><span class="sxs-lookup"><span data-stu-id="70eff-111">Even resources you add later inherit the lock from the parent.</span></span> <span data-ttu-id="70eff-112">Det mest restriktiva låset i ärvda företräde.</span><span class="sxs-lookup"><span data-stu-id="70eff-112">The most restrictive lock in the inheritance takes precedence.</span></span>

<span data-ttu-id="70eff-113">Till skillnad från rollbaserad åtkomstkontroll använder du management Lås för att tillämpa en begränsning för alla användare och roller.</span><span class="sxs-lookup"><span data-stu-id="70eff-113">Unlike role-based access control, you use management locks to apply a restriction across all users and roles.</span></span> <span data-ttu-id="70eff-114">Läs om hur du anger behörigheter för användare och roller i [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="70eff-114">To learn about setting permissions for users and roles, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>

<span data-ttu-id="70eff-115">Hanteraren för filserverresurser Lås gäller endast för åtgärder som sker i management-plan, som består av åtgärder som skickas till `https://management.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="70eff-115">Resource Manager locks apply only to operations that happen in the management plane, which consists of operations sent to `https://management.azure.com`.</span></span> <span data-ttu-id="70eff-116">Lås begränsar inte hur resurser utföra sina egna funktioner.</span><span class="sxs-lookup"><span data-stu-id="70eff-116">The locks do not restrict how resources perform their own functions.</span></span> <span data-ttu-id="70eff-117">Resursändringar är begränsad, men resursåtgärder är inte begränsade.</span><span class="sxs-lookup"><span data-stu-id="70eff-117">Resource changes are restricted, but resource operations are not restricted.</span></span> <span data-ttu-id="70eff-118">Till exempel en ReadOnly-lås på en SQL-databas hindrar dig från att ta bort eller ändra databasen, men det förhindrar inte du att skapa, uppdatera eller ta bort data i databasen.</span><span class="sxs-lookup"><span data-stu-id="70eff-118">For example, a ReadOnly lock on a SQL Database prevents you from deleting or modifying the database, but it does not prevent you from creating, updating, or deleting data in the database.</span></span> <span data-ttu-id="70eff-119">Data transaktioner tillåts eftersom dessa åtgärder inte skickas till `https://management.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="70eff-119">Data transactions are permitted because those operations are not sent to `https://management.azure.com`.</span></span>

<span data-ttu-id="70eff-120">Tillämpa **ReadOnly** kan leda till oväntade resultat eftersom vissa åtgärder som ser ut som att läsa operations faktiskt kräver ytterligare åtgärder.</span><span class="sxs-lookup"><span data-stu-id="70eff-120">Applying **ReadOnly** can lead to unexpected results because some operations that seem like read operations actually require additional actions.</span></span> <span data-ttu-id="70eff-121">Till exempel placera en **ReadOnly** lås på ett lagringskonto som förhindrar att alla användare lista nycklarna.</span><span class="sxs-lookup"><span data-stu-id="70eff-121">For example, placing a **ReadOnly** lock on a storage account prevents all users from listing the keys.</span></span> <span data-ttu-id="70eff-122">Listan med nycklar operationen hanteras via en POST-begäran eftersom returnerade nycklar är tillgängliga för skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="70eff-122">The list keys operation is handled through a POST request because the returned keys are available for write operations.</span></span> <span data-ttu-id="70eff-123">För ett annat exempel är att placera en **ReadOnly** lås på en App Service-resurs som förhindrar att Visual Studio Server Explorer visar filer för resursen eftersom den interaktionen kräver skrivåtkomst.</span><span class="sxs-lookup"><span data-stu-id="70eff-123">For another example, placing a **ReadOnly** lock on an App Service resource prevents Visual Studio Server Explorer from displaying files for the resource because that interaction requires write access.</span></span>

## <a name="who-can-create-or-delete-locks-in-your-organization"></a><span data-ttu-id="70eff-124">Vem kan skapa eller ta bort lås i din organisation</span><span class="sxs-lookup"><span data-stu-id="70eff-124">Who can create or delete locks in your organization</span></span>
<span data-ttu-id="70eff-125">För att skapa eller ta bort management lås, måste du ha åtkomst till `Microsoft.Authorization/*` eller `Microsoft.Authorization/locks/*` åtgärder.</span><span class="sxs-lookup"><span data-stu-id="70eff-125">To create or delete management locks, you must have access to `Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions.</span></span> <span data-ttu-id="70eff-126">I de inbyggda rollerna, endast **ägare** och **administratör för användaråtkomst** beviljas dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="70eff-126">Of the built-in roles, only **Owner** and **User Access Administrator** are granted those actions.</span></span>

## <a name="portal"></a><span data-ttu-id="70eff-127">Portalen</span><span class="sxs-lookup"><span data-stu-id="70eff-127">Portal</span></span>
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a><span data-ttu-id="70eff-128">Mall</span><span class="sxs-lookup"><span data-stu-id="70eff-128">Template</span></span>
<span data-ttu-id="70eff-129">I följande exempel visas en mall som skapar ett lås på ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="70eff-129">The following example shows a template that creates a lock on a storage account.</span></span> <span data-ttu-id="70eff-130">Storage-konto som ska tillämpas låset tillhandahålls som en parameter.</span><span class="sxs-lookup"><span data-stu-id="70eff-130">The storage account on which to apply the lock is provided as a parameter.</span></span> <span data-ttu-id="70eff-131">Namnet på låset har skapats genom att resursnamnet med **/Microsoft.Authorization/** och namnet på Lås i det här fallet **myLock**.</span><span class="sxs-lookup"><span data-stu-id="70eff-131">The name of the lock is created by concatenating the resource name with **/Microsoft.Authorization/** and the name of the lock, in this case **myLock**.</span></span>

<span data-ttu-id="70eff-132">Typen som är specifika för resurstypen.</span><span class="sxs-lookup"><span data-stu-id="70eff-132">The type provided is specific to the resource type.</span></span> <span data-ttu-id="70eff-133">Ange till ”Microsoft.Storage/storageaccounts/providers/locks” för lagring.</span><span class="sxs-lookup"><span data-stu-id="70eff-133">For storage, set the type to "Microsoft.Storage/storageaccounts/providers/locks".</span></span>

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

## <a name="powershell"></a><span data-ttu-id="70eff-134">PowerShell</span><span class="sxs-lookup"><span data-stu-id="70eff-134">PowerShell</span></span>
<span data-ttu-id="70eff-135">Du Lås distribueras resurser med Azure PowerShell med hjälp av den [ny AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) kommando.</span><span class="sxs-lookup"><span data-stu-id="70eff-135">You lock deployed resources with Azure PowerShell by using the [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) command.</span></span>

<span data-ttu-id="70eff-136">Ange namnet på resursen och dess resurstypen dess resursgruppens namn om du vill låsa en resurs.</span><span class="sxs-lookup"><span data-stu-id="70eff-136">To lock a resource, provide the name of the resource, its resource type, and its resource group name.</span></span>

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite `
  -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="70eff-137">Om du vill låsa en resursgrupp, ange namnet på resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="70eff-137">To lock a resource group, provide the name of the resource group.</span></span>

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="70eff-138">För att få information om ett lås använda [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock).</span><span class="sxs-lookup"><span data-stu-id="70eff-138">To get information about a lock, use [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock).</span></span> <span data-ttu-id="70eff-139">Om du vill hämta alla Lås i din prenumeration, använder du:</span><span class="sxs-lookup"><span data-stu-id="70eff-139">To get all the locks in your subscription, use:</span></span>

```powershell
Get-AzureRmResourceLock
```

<span data-ttu-id="70eff-140">Om du vill hämta alla låsningar för en resurs, använder du:</span><span class="sxs-lookup"><span data-stu-id="70eff-140">To get all locks for a resource, use:</span></span>

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="70eff-141">Om du vill hämta alla låsningar för en resursgrupp, använder du:</span><span class="sxs-lookup"><span data-stu-id="70eff-141">To get all locks for a resource group, use:</span></span>

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="70eff-142">Azure PowerShell innehåller andra kommandon för att arbeta lås som [Set AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) att uppdatera ett lås och [ta bort AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) att ta bort ett lås.</span><span class="sxs-lookup"><span data-stu-id="70eff-142">Azure PowerShell provides other commands for working locks, such as [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) to update a lock, and [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) to delete a lock.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="70eff-143">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="70eff-143">Azure CLI</span></span>

<span data-ttu-id="70eff-144">Du Lås distribueras resurser med Azure CLI med hjälp av den [az Lås skapa](/cli/azure/lock#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="70eff-144">You lock deployed resources with Azure CLI by using the [az lock create](/cli/azure/lock#create) command.</span></span>

<span data-ttu-id="70eff-145">Ange namnet på resursen och dess resurstypen dess resursgruppens namn om du vill låsa en resurs.</span><span class="sxs-lookup"><span data-stu-id="70eff-145">To lock a resource, provide the name of the resource, its resource type, and its resource group name.</span></span>

```azurecli
az lock create --name LockSite --lock-type CanNotDelete \
  --resource-group exampleresourcegroup --resource-name examplesite \
  --resource-type Microsoft.Web/sites
```

<span data-ttu-id="70eff-146">Om du vill låsa en resursgrupp, ange namnet på resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="70eff-146">To lock a resource group, provide the name of the resource group.</span></span>

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete \
  --resource-group exampleresourcegroup
```

<span data-ttu-id="70eff-147">För att få information om ett lås använda [az Lås listan](/cli/azure/lock#list).</span><span class="sxs-lookup"><span data-stu-id="70eff-147">To get information about a lock, use [az lock list](/cli/azure/lock#list).</span></span> <span data-ttu-id="70eff-148">Om du vill hämta alla Lås i din prenumeration, använder du:</span><span class="sxs-lookup"><span data-stu-id="70eff-148">To get all the locks in your subscription, use:</span></span>

```azurecli
az lock list
```

<span data-ttu-id="70eff-149">Om du vill hämta alla låsningar för en resurs, använder du:</span><span class="sxs-lookup"><span data-stu-id="70eff-149">To get all locks for a resource, use:</span></span>

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite \
  --namespace Microsoft.Web --resource-type sites --parent ""
```

<span data-ttu-id="70eff-150">Om du vill hämta alla låsningar för en resursgrupp, använder du:</span><span class="sxs-lookup"><span data-stu-id="70eff-150">To get all locks for a resource group, use:</span></span>

```azurecli
az lock list --resource-group exampleresourcegroup
```

<span data-ttu-id="70eff-151">Azure CLI finns andra kommandon för att arbeta lås som [az Lås uppdatering](/cli/azure/lock#update) att uppdatera ett lås och [az Lås ta bort](/cli/azure/lock#delete) att ta bort ett lås.</span><span class="sxs-lookup"><span data-stu-id="70eff-151">Azure CLI provides other commands for working locks, such as [az lock update](/cli/azure/lock#update) to update a lock, and [az lock delete](/cli/azure/lock#delete) to delete a lock.</span></span>

## <a name="rest-api"></a><span data-ttu-id="70eff-152">REST API</span><span class="sxs-lookup"><span data-stu-id="70eff-152">REST API</span></span>
<span data-ttu-id="70eff-153">Du kan låsa distribuerade resurser med den [REST API för hantering av Lås](https://docs.microsoft.com/rest/api/resources/managementlocks).</span><span class="sxs-lookup"><span data-stu-id="70eff-153">You can lock deployed resources with the [REST API for management locks](https://docs.microsoft.com/rest/api/resources/managementlocks).</span></span> <span data-ttu-id="70eff-154">REST-API kan du skapa och ta bort lås och hämta information om befintliga Lås.</span><span class="sxs-lookup"><span data-stu-id="70eff-154">The REST API enables you to create and delete locks, and retrieve information about existing locks.</span></span>

<span data-ttu-id="70eff-155">Om du vill skapa ett lås, kör du:</span><span class="sxs-lookup"><span data-stu-id="70eff-155">To create a lock, run:</span></span>

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

<span data-ttu-id="70eff-156">Omfattningen kan vara en prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="70eff-156">The scope could be a subscription, resource group, or resource.</span></span> <span data-ttu-id="70eff-157">Lås-namnet är vad du vill anropa låset.</span><span class="sxs-lookup"><span data-stu-id="70eff-157">The lock-name is whatever you want to call the lock.</span></span> <span data-ttu-id="70eff-158">Api-versionen, Använd **2015-01-01**.</span><span class="sxs-lookup"><span data-stu-id="70eff-158">For api-version, use **2015-01-01**.</span></span>

<span data-ttu-id="70eff-159">I en begäran innehåller ett JSON-objekt som anger egenskaperna för låset.</span><span class="sxs-lookup"><span data-stu-id="70eff-159">In the request, include a JSON object that specifies the properties for the lock.</span></span>

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a><span data-ttu-id="70eff-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="70eff-160">Next steps</span></span>
* <span data-ttu-id="70eff-161">Mer information om hur du arbetar med resurslås finns [Lås ned din Azure-resurser](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)</span><span class="sxs-lookup"><span data-stu-id="70eff-161">For more information about working with resource locks, see [Lock Down Your Azure Resources](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)</span></span>
* <span data-ttu-id="70eff-162">Läs om hur du ordnar dina resurser i [med taggar för att organisera dina resurser](resource-group-using-tags.md)</span><span class="sxs-lookup"><span data-stu-id="70eff-162">To learn about logically organizing your resources, see [Using tags to organize your resources](resource-group-using-tags.md)</span></span>
* <span data-ttu-id="70eff-163">Om du vill ändra vilken resursgrupp som en resurs som finns i [flytta resurser till den nya resursgruppen.](resource-group-move-resources.md)</span><span class="sxs-lookup"><span data-stu-id="70eff-163">To change which resource group a resource resides in, see [Move resources to new resource group](resource-group-move-resources.md)</span></span>
* <span data-ttu-id="70eff-164">Du kan tillämpa begränsningar och konventioner över din prenumeration med anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="70eff-164">You can apply restrictions and conventions across your subscription with customized policies.</span></span> <span data-ttu-id="70eff-165">Mer information finns i [Hantera resurser och kontrollera åtkomsten med hjälp av principer](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="70eff-165">For more information, see [Use Policy to manage resources and control access](resource-manager-policy.md).</span></span>
* <span data-ttu-id="70eff-166">Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="70eff-166">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

