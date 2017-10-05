---
title: Mallfunktioner Azure Resource Manager - resurser | Microsoft Docs
description: "Beskriver funktionerna du använder i en Azure Resource Manager-mall för att hämta värden om resurser."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2017
ms.author: tomfitz
ms.openlocfilehash: 494ade55f21c19d9c68d5cc52756528401d9bb77
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="54f73-103">Resursfunktioner för Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="54f73-103">Resource functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="54f73-104">Hanteraren för filserverresurser innehåller följande funktioner för att hämta resurs värden:</span><span class="sxs-lookup"><span data-stu-id="54f73-104">Resource Manager provides the following functions for getting resource values:</span></span>

* [<span data-ttu-id="54f73-105">listKeys och lista {Value}</span><span class="sxs-lookup"><span data-stu-id="54f73-105">listKeys and list{Value}</span></span>](#listkeys)
* [<span data-ttu-id="54f73-106">providers</span><span class="sxs-lookup"><span data-stu-id="54f73-106">providers</span></span>](#providers)
* [<span data-ttu-id="54f73-107">referens</span><span class="sxs-lookup"><span data-stu-id="54f73-107">reference</span></span>](#reference)
* [<span data-ttu-id="54f73-108">resourceGroup</span><span class="sxs-lookup"><span data-stu-id="54f73-108">resourceGroup</span></span>](#resourcegroup)
* [<span data-ttu-id="54f73-109">resurs-ID</span><span class="sxs-lookup"><span data-stu-id="54f73-109">resourceId</span></span>](#resourceid)
* [<span data-ttu-id="54f73-110">prenumeration</span><span class="sxs-lookup"><span data-stu-id="54f73-110">subscription</span></span>](#subscription)

<span data-ttu-id="54f73-111">Om du vill hämta värden från parametrar, variabler eller den aktuella distributionen finns [distribution värdet funktioner](resource-group-template-functions-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="54f73-111">To get values from parameters, variables, or the current deployment, see [Deployment value functions](resource-group-template-functions-deployment.md).</span></span>

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-and-listvalue"></a><span data-ttu-id="54f73-112">listKeys och lista {Value}</span><span class="sxs-lookup"><span data-stu-id="54f73-112">listKeys and list{Value}</span></span>
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

<span data-ttu-id="54f73-113">Returnerar värden för någon resurstyp av som har stöd för listan igen.</span><span class="sxs-lookup"><span data-stu-id="54f73-113">Returns the values for any resource type that supports the list operation.</span></span> <span data-ttu-id="54f73-114">Den vanligaste användningen är `listKeys`.</span><span class="sxs-lookup"><span data-stu-id="54f73-114">The most common usage is `listKeys`.</span></span> 

### <a name="parameters"></a><span data-ttu-id="54f73-115">Parametrar</span><span class="sxs-lookup"><span data-stu-id="54f73-115">Parameters</span></span>

| <span data-ttu-id="54f73-116">Parameter</span><span class="sxs-lookup"><span data-stu-id="54f73-116">Parameter</span></span> | <span data-ttu-id="54f73-117">Krävs</span><span class="sxs-lookup"><span data-stu-id="54f73-117">Required</span></span> | <span data-ttu-id="54f73-118">Typ</span><span class="sxs-lookup"><span data-stu-id="54f73-118">Type</span></span> | <span data-ttu-id="54f73-119">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="54f73-119">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="54f73-120">resourceName eller resourceIdentifier</span><span class="sxs-lookup"><span data-stu-id="54f73-120">resourceName or resourceIdentifier</span></span> |<span data-ttu-id="54f73-121">Ja</span><span class="sxs-lookup"><span data-stu-id="54f73-121">Yes</span></span> |<span data-ttu-id="54f73-122">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-122">string</span></span> |<span data-ttu-id="54f73-123">Unik identifierare för resursen.</span><span class="sxs-lookup"><span data-stu-id="54f73-123">Unique identifier for the resource.</span></span> |
| <span data-ttu-id="54f73-124">apiVersion</span><span class="sxs-lookup"><span data-stu-id="54f73-124">apiVersion</span></span> |<span data-ttu-id="54f73-125">Ja</span><span class="sxs-lookup"><span data-stu-id="54f73-125">Yes</span></span> |<span data-ttu-id="54f73-126">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-126">string</span></span> |<span data-ttu-id="54f73-127">API-versionen av resursen runtime-tillståndet.</span><span class="sxs-lookup"><span data-stu-id="54f73-127">API version of resource runtime state.</span></span> <span data-ttu-id="54f73-128">Normalt i formatet, **åååå-mm-dd**.</span><span class="sxs-lookup"><span data-stu-id="54f73-128">Typically, in the format, **yyyy-mm-dd**.</span></span> |

### <a name="return-value"></a><span data-ttu-id="54f73-129">Returvärde</span><span class="sxs-lookup"><span data-stu-id="54f73-129">Return value</span></span>

<span data-ttu-id="54f73-130">Det returnerade objektet från listKeys har följande format:</span><span class="sxs-lookup"><span data-stu-id="54f73-130">The returned object from listKeys has the following format:</span></span>

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

<span data-ttu-id="54f73-131">Andra listfunktioner har olika returnerade format.</span><span class="sxs-lookup"><span data-stu-id="54f73-131">Other list functions have different return formats.</span></span> <span data-ttu-id="54f73-132">Om du vill se format för en funktion, inkludera den i avsnittet utdata som visas i exemplet mallen.</span><span class="sxs-lookup"><span data-stu-id="54f73-132">To see the format of a function, include it in the outputs section as shown in the example template.</span></span> 

### <a name="remarks"></a><span data-ttu-id="54f73-133">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="54f73-133">Remarks</span></span>

<span data-ttu-id="54f73-134">Alla åtgärder som börjar med **lista** kan användas som en funktion i mallen.</span><span class="sxs-lookup"><span data-stu-id="54f73-134">Any operation that starts with **list** can be used as a function in your template.</span></span> <span data-ttu-id="54f73-135">De tillgängliga åtgärderna innehåller inte bara listKeys, men även åtgärder som `list`, `listAdminKeys`, och `listStatus`.</span><span class="sxs-lookup"><span data-stu-id="54f73-135">The available operations include not only listKeys, but also operations like `list`, `listAdminKeys`, and `listStatus`.</span></span> <span data-ttu-id="54f73-136">Du kan dock använda **lista** åtgärder som kräver att värden i begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="54f73-136">However, you cannot use **list** operations that require values in the request body.</span></span> <span data-ttu-id="54f73-137">Till exempel den [lista kontots SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) åtgärden kräver begärantext som *signedExpiry*, så du inte kan använda den i en mall.</span><span class="sxs-lookup"><span data-stu-id="54f73-137">For example, the [List Account SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) operation requires request body parameters like *signedExpiry*, so you cannot use it within a template.</span></span>

<span data-ttu-id="54f73-138">För att avgöra vilka resurstyper har en liståtgärd har du följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="54f73-138">To determine which resource types have a list operation, you have the following options:</span></span>

* <span data-ttu-id="54f73-139">Visa den [REST-API: et](/rest/api/) för resursprovidern och leta efter Liståtgärder.</span><span class="sxs-lookup"><span data-stu-id="54f73-139">View the [REST API operations](/rest/api/) for a resource provider, and look for list operations.</span></span> <span data-ttu-id="54f73-140">Till exempel lagringskonton har den [listKeys åtgärden](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).</span><span class="sxs-lookup"><span data-stu-id="54f73-140">For example, storage accounts have the [listKeys operation](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).</span></span>
* <span data-ttu-id="54f73-141">Använd den [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="54f73-141">Use the [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell cmdlet.</span></span> <span data-ttu-id="54f73-142">I följande exempel hämtas alla Liståtgärder för storage-konton:</span><span class="sxs-lookup"><span data-stu-id="54f73-142">The following example gets all list operations for storage accounts:</span></span>

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* <span data-ttu-id="54f73-143">Du kan använda kommandot Azure CLI för att filtrera listan åtgärderna:</span><span class="sxs-lookup"><span data-stu-id="54f73-143">Use the following Azure CLI command to filter only the list operations:</span></span>

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

<span data-ttu-id="54f73-144">Ange resursen med hjälp av antingen den [resourceId funktionen](#resourceid), eller formatet `{providerNamespace}/{resourceType}/{resourceName}`.</span><span class="sxs-lookup"><span data-stu-id="54f73-144">Specify the resource by using either the [resourceId function](#resourceid), or the format `{providerNamespace}/{resourceType}/{resourceName}`.</span></span>


### <a name="example"></a><span data-ttu-id="54f73-145">Exempel</span><span class="sxs-lookup"><span data-stu-id="54f73-145">Example</span></span>

<span data-ttu-id="54f73-146">I följande exempel visas hur du återställer de primära och sekundära nycklarna från ett lagringskonto i avsnittet utdata.</span><span class="sxs-lookup"><span data-stu-id="54f73-146">The following example shows how to return the primary and secondary keys from a storage account in the outputs section.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountId": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "storageKeysOutput": {
            "value": "[listKeys(parameters('storageAccountId'), '2016-01-01')]",
            "type" : "object"
        }
    }
}
``` 

<a id="providers" />

## <a name="providers"></a><span data-ttu-id="54f73-147">providers</span><span class="sxs-lookup"><span data-stu-id="54f73-147">providers</span></span>
`providers(providerNamespace, [resourceType])`

<span data-ttu-id="54f73-148">Returnerar information om en resursprovider och dess resurstyper som stöds.</span><span class="sxs-lookup"><span data-stu-id="54f73-148">Returns information about a resource provider and its supported resource types.</span></span> <span data-ttu-id="54f73-149">Om du inte anger en resurstyp returnerar funktionen typerna som stöds för resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="54f73-149">If you do not provide a resource type, the function returns all the supported types for the resource provider.</span></span>

### <a name="parameters"></a><span data-ttu-id="54f73-150">Parametrar</span><span class="sxs-lookup"><span data-stu-id="54f73-150">Parameters</span></span>

| <span data-ttu-id="54f73-151">Parameter</span><span class="sxs-lookup"><span data-stu-id="54f73-151">Parameter</span></span> | <span data-ttu-id="54f73-152">Krävs</span><span class="sxs-lookup"><span data-stu-id="54f73-152">Required</span></span> | <span data-ttu-id="54f73-153">Typ</span><span class="sxs-lookup"><span data-stu-id="54f73-153">Type</span></span> | <span data-ttu-id="54f73-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="54f73-154">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="54f73-155">providerNamespace</span><span class="sxs-lookup"><span data-stu-id="54f73-155">providerNamespace</span></span> |<span data-ttu-id="54f73-156">Ja</span><span class="sxs-lookup"><span data-stu-id="54f73-156">Yes</span></span> |<span data-ttu-id="54f73-157">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-157">string</span></span> |<span data-ttu-id="54f73-158">Namespace av providern</span><span class="sxs-lookup"><span data-stu-id="54f73-158">Namespace of the provider</span></span> |
| <span data-ttu-id="54f73-159">resourceType</span><span class="sxs-lookup"><span data-stu-id="54f73-159">resourceType</span></span> |<span data-ttu-id="54f73-160">Nej</span><span class="sxs-lookup"><span data-stu-id="54f73-160">No</span></span> |<span data-ttu-id="54f73-161">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-161">string</span></span> |<span data-ttu-id="54f73-162">Typ av resurs i det angivna namnområdet.</span><span class="sxs-lookup"><span data-stu-id="54f73-162">The type of resource within the specified namespace.</span></span> |

### <a name="return-value"></a><span data-ttu-id="54f73-163">Returvärde</span><span class="sxs-lookup"><span data-stu-id="54f73-163">Return value</span></span>

<span data-ttu-id="54f73-164">Varje typ som stöds returneras i följande format:</span><span class="sxs-lookup"><span data-stu-id="54f73-164">Each supported type is returned in the following format:</span></span> 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

<span data-ttu-id="54f73-165">Matrisen sorteringen av de returnerade värdena är inte säkert.</span><span class="sxs-lookup"><span data-stu-id="54f73-165">Array ordering of the returned values is not guaranteed.</span></span>

### <a name="example"></a><span data-ttu-id="54f73-166">Exempel</span><span class="sxs-lookup"><span data-stu-id="54f73-166">Example</span></span>

<span data-ttu-id="54f73-167">I följande exempel visas hur du använder funktionen providern:</span><span class="sxs-lookup"><span data-stu-id="54f73-167">The following example shows how to use the provider function:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "providerOutput": {
            "value": "[providers('Microsoft.Web', 'sites')]",
            "type" : "object"
        }
    }
}
```

<span data-ttu-id="54f73-168">Föregående exempel returnerar ett objekt i följande format:</span><span class="sxs-lookup"><span data-stu-id="54f73-168">The preceding example returns an object in the following format:</span></span>

```json
{
  "resourceType": "sites",
  "locations": [
    "South Central US",
    "North Europe",
    "West Europe",
    "Southeast Asia",
    ...
  ],
  "apiVersions": [
    "2016-08-01",
    "2016-03-01",
    "2015-08-01-preview",
    "2015-08-01",
    ...
  ]
}
```

<a id="reference" />

## <a name="reference"></a><span data-ttu-id="54f73-169">Referens</span><span class="sxs-lookup"><span data-stu-id="54f73-169">reference</span></span>
`reference(resourceName or resourceIdentifier, [apiVersion])`

<span data-ttu-id="54f73-170">Returnerar ett objekt som representerar en resurs runtime-tillståndet.</span><span class="sxs-lookup"><span data-stu-id="54f73-170">Returns an object representing a resource's runtime state.</span></span>

### <a name="parameters"></a><span data-ttu-id="54f73-171">Parametrar</span><span class="sxs-lookup"><span data-stu-id="54f73-171">Parameters</span></span>

| <span data-ttu-id="54f73-172">Parameter</span><span class="sxs-lookup"><span data-stu-id="54f73-172">Parameter</span></span> | <span data-ttu-id="54f73-173">Krävs</span><span class="sxs-lookup"><span data-stu-id="54f73-173">Required</span></span> | <span data-ttu-id="54f73-174">Typ</span><span class="sxs-lookup"><span data-stu-id="54f73-174">Type</span></span> | <span data-ttu-id="54f73-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="54f73-175">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="54f73-176">resourceName eller resourceIdentifier</span><span class="sxs-lookup"><span data-stu-id="54f73-176">resourceName or resourceIdentifier</span></span> |<span data-ttu-id="54f73-177">Ja</span><span class="sxs-lookup"><span data-stu-id="54f73-177">Yes</span></span> |<span data-ttu-id="54f73-178">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-178">string</span></span> |<span data-ttu-id="54f73-179">Namn eller unik identifierare för en resurs.</span><span class="sxs-lookup"><span data-stu-id="54f73-179">Name or unique identifier of a resource.</span></span> |
| <span data-ttu-id="54f73-180">apiVersion</span><span class="sxs-lookup"><span data-stu-id="54f73-180">apiVersion</span></span> |<span data-ttu-id="54f73-181">Nej</span><span class="sxs-lookup"><span data-stu-id="54f73-181">No</span></span> |<span data-ttu-id="54f73-182">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-182">string</span></span> |<span data-ttu-id="54f73-183">API-versionen av den angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="54f73-183">API version of the specified resource.</span></span> <span data-ttu-id="54f73-184">Inkludera den här parametern när resursen inte är etablerad inom samma mall.</span><span class="sxs-lookup"><span data-stu-id="54f73-184">Include this parameter when the resource is not provisioned within same template.</span></span> <span data-ttu-id="54f73-185">Normalt i formatet, **åååå-mm-dd**.</span><span class="sxs-lookup"><span data-stu-id="54f73-185">Typically, in the format, **yyyy-mm-dd**.</span></span> |

### <a name="return-value"></a><span data-ttu-id="54f73-186">Returvärde</span><span class="sxs-lookup"><span data-stu-id="54f73-186">Return value</span></span>

<span data-ttu-id="54f73-187">Varje resurstypen returnerar andra egenskaper för funktionen referens.</span><span class="sxs-lookup"><span data-stu-id="54f73-187">Every resource type returns different properties for the reference function.</span></span> <span data-ttu-id="54f73-188">Funktionen returnerar inte ett enda fördefinierade format.</span><span class="sxs-lookup"><span data-stu-id="54f73-188">The function does not return a single, predefined format.</span></span> <span data-ttu-id="54f73-189">Returnera objektet om du vill visa egenskaperna för en resurstyp i avsnittet utdata som visas i exemplet.</span><span class="sxs-lookup"><span data-stu-id="54f73-189">To see the properties for a resource type, return the object in the outputs section as shown in the example.</span></span>

### <a name="remarks"></a><span data-ttu-id="54f73-190">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="54f73-190">Remarks</span></span>

<span data-ttu-id="54f73-191">Funktionen referens hämtar sitt värde från en runtime-tillståndet och kan därför inte användas i avsnittet variables.</span><span class="sxs-lookup"><span data-stu-id="54f73-191">The reference function derives its value from a runtime state, and therefore cannot be used in the variables section.</span></span> <span data-ttu-id="54f73-192">Det kan användas i utdata avsnitt i en mall.</span><span class="sxs-lookup"><span data-stu-id="54f73-192">It can be used in outputs section of a template.</span></span> 

<span data-ttu-id="54f73-193">Med hjälp av funktionen referens deklarera du implicit att en resurs beror på en annan resurs om den refererade resursen etableras inom samma mall.</span><span class="sxs-lookup"><span data-stu-id="54f73-193">By using the reference function, you implicitly declare that one resource depends on another resource if the referenced resource is provisioned within same template.</span></span> <span data-ttu-id="54f73-194">Du behöver inte också använda dependsOn-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="54f73-194">You do not need to also use the dependsOn property.</span></span> <span data-ttu-id="54f73-195">Funktionen utvärderas inte förrän den refererade resursen har slutfört distributionen.</span><span class="sxs-lookup"><span data-stu-id="54f73-195">The function is not evaluated until the referenced resource has completed deployment.</span></span>

<span data-ttu-id="54f73-196">Skapa en mall som returnerar objektet i avsnittet utdata om du vill se egenskapsnamn och värden för en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="54f73-196">To see the property names and values for a resource type, create a template that returns the object in the outputs section.</span></span> <span data-ttu-id="54f73-197">Om du har en befintlig resurs av den typen returnerar mallen för objektet utan att distribuera nya resurser.</span><span class="sxs-lookup"><span data-stu-id="54f73-197">If you have an existing resource of that type, your template returns the object without deploying any new resources.</span></span> 

<span data-ttu-id="54f73-198">Normalt använder du den **referens** funktionen för att returnera ett visst värde från ett objekt, till exempel slutpunkt för blob-URI eller fullständigt domännamn.</span><span class="sxs-lookup"><span data-stu-id="54f73-198">Typically, you use the **reference** function to return a particular value from an object, such as the blob endpoint URI or fully qualified domain name.</span></span>

```json
"outputs": {
    "BlobUri": {
        "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
        "type" : "string"
    },
    "FQDN": {
        "value": "[reference(concat('Microsoft.Network/publicIPAddresses/', parameters('ipAddressName')), '2016-03-30').dnsSettings.fqdn]",
        "type" : "string"
    }
}
```

### <a name="example"></a><span data-ttu-id="54f73-199">Exempel</span><span class="sxs-lookup"><span data-stu-id="54f73-199">Example</span></span>

<span data-ttu-id="54f73-200">Använd följande för att distribuera och referera till resursen i samma mall:</span><span class="sxs-lookup"><span data-stu-id="54f73-200">To deploy and reference the resource in the same template, use:</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[reference(parameters('storageAccountName'))]"
      }
    }
}
``` 

<span data-ttu-id="54f73-201">Föregående exempel returnerar ett objekt i följande format:</span><span class="sxs-lookup"><span data-stu-id="54f73-201">The preceding example returns an object in the following format:</span></span>

```json
{
   "creationTime": "2017-06-13T21:24:46.618364Z",
   "primaryEndpoints": {
     "blob": "https://examplestorage.blob.core.windows.net/",
     "file": "https://examplestorage.file.core.windows.net/",
     "queue": "https://examplestorage.queue.core.windows.net/",
     "table": "https://examplestorage.table.core.windows.net/"
   },
   "primaryLocation": "southcentralus",
   "provisioningState": "Succeeded",
   "statusOfPrimary": "available",
   "supportsHttpsTrafficOnly": false
}
```

<span data-ttu-id="54f73-202">I följande exempel refererar till ett lagringskonto som inte har distribuerats i den här mallen.</span><span class="sxs-lookup"><span data-stu-id="54f73-202">The following example references a storage account that is not deployed in this template.</span></span> <span data-ttu-id="54f73-203">Storage-konto finns redan i samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="54f73-203">The storage account already exists within the same resource group.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }
}
```

<a id="resourcegroup" />

## <a name="resourcegroup"></a><span data-ttu-id="54f73-204">resourceGroup</span><span class="sxs-lookup"><span data-stu-id="54f73-204">resourceGroup</span></span>
`resourceGroup()`

<span data-ttu-id="54f73-205">Returnerar ett objekt som representerar den aktuella resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="54f73-205">Returns an object that represents the current resource group.</span></span> 

### <a name="return-value"></a><span data-ttu-id="54f73-206">Returvärde</span><span class="sxs-lookup"><span data-stu-id="54f73-206">Return value</span></span>

<span data-ttu-id="54f73-207">Det returnerade objektet är i följande format:</span><span class="sxs-lookup"><span data-stu-id="54f73-207">The returned object is in the following format:</span></span>

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

### <a name="remarks"></a><span data-ttu-id="54f73-208">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="54f73-208">Remarks</span></span>

<span data-ttu-id="54f73-209">Ett vanligt användningsområde för funktionen resourceGroup är att skapa resurser på samma plats som resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="54f73-209">A common use of the resourceGroup function is to create resources in the same location as the resource group.</span></span> <span data-ttu-id="54f73-210">I följande exempel används resursgruppens plats tilldelas platsen för en webbplats.</span><span class="sxs-lookup"><span data-stu-id="54f73-210">The following example uses the resource group location to assign the location for a web site.</span></span>

```json
"resources": [
   {
      "apiVersion": "2016-08-01",
      "type": "Microsoft.Web/sites",
      "name": "[parameters('siteName')]",
      "location": "[resourceGroup().location]",
      ...
   }
]
```

### <a name="example"></a><span data-ttu-id="54f73-211">Exempel</span><span class="sxs-lookup"><span data-stu-id="54f73-211">Example</span></span>

<span data-ttu-id="54f73-212">Följande mall returnerar egenskaperna för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="54f73-212">The following template returns the properties of the resource group.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[resourceGroup()]",
            "type" : "object"
        }
    }
}
```

<span data-ttu-id="54f73-213">Föregående exempel returnerar ett objekt i följande format:</span><span class="sxs-lookup"><span data-stu-id="54f73-213">The preceding example returns an object in the following format:</span></span>

```json
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/examplegroup",
  "name": "examplegroup",
  "location": "southcentralus",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

<a id="resourceid" />

## <a name="resourceid"></a><span data-ttu-id="54f73-214">resourceId</span><span class="sxs-lookup"><span data-stu-id="54f73-214">resourceId</span></span>
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

<span data-ttu-id="54f73-215">Returnerar den unika identifieraren för en resurs.</span><span class="sxs-lookup"><span data-stu-id="54f73-215">Returns the unique identifier of a resource.</span></span> <span data-ttu-id="54f73-216">Du kan använda den här funktionen när resursnamnet är tvetydigt eller inte etablerade inom samma mall.</span><span class="sxs-lookup"><span data-stu-id="54f73-216">You use this function when the resource name is ambiguous or not provisioned within the same template.</span></span> 

### <a name="parameters"></a><span data-ttu-id="54f73-217">Parametrar</span><span class="sxs-lookup"><span data-stu-id="54f73-217">Parameters</span></span>

| <span data-ttu-id="54f73-218">Parameter</span><span class="sxs-lookup"><span data-stu-id="54f73-218">Parameter</span></span> | <span data-ttu-id="54f73-219">Krävs</span><span class="sxs-lookup"><span data-stu-id="54f73-219">Required</span></span> | <span data-ttu-id="54f73-220">Typ</span><span class="sxs-lookup"><span data-stu-id="54f73-220">Type</span></span> | <span data-ttu-id="54f73-221">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="54f73-221">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="54f73-222">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="54f73-222">subscriptionId</span></span> |<span data-ttu-id="54f73-223">Nej</span><span class="sxs-lookup"><span data-stu-id="54f73-223">No</span></span> |<span data-ttu-id="54f73-224">sträng (i GUID-format)</span><span class="sxs-lookup"><span data-stu-id="54f73-224">string (In GUID format)</span></span> |<span data-ttu-id="54f73-225">Standardvärdet är den aktuella prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="54f73-225">Default value is the current subscription.</span></span> <span data-ttu-id="54f73-226">Ange det här värdet när du vill hämta en resurs i en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="54f73-226">Specify this value when you need to retrieve a resource in another subscription.</span></span> |
| <span data-ttu-id="54f73-227">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="54f73-227">resourceGroupName</span></span> |<span data-ttu-id="54f73-228">Nej</span><span class="sxs-lookup"><span data-stu-id="54f73-228">No</span></span> |<span data-ttu-id="54f73-229">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-229">string</span></span> |<span data-ttu-id="54f73-230">Standardvärdet är aktuella resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="54f73-230">Default value is current resource group.</span></span> <span data-ttu-id="54f73-231">Ange det här värdet när du vill hämta en resurs i en annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="54f73-231">Specify this value when you need to retrieve a resource in another resource group.</span></span> |
| <span data-ttu-id="54f73-232">resourceType</span><span class="sxs-lookup"><span data-stu-id="54f73-232">resourceType</span></span> |<span data-ttu-id="54f73-233">Ja</span><span class="sxs-lookup"><span data-stu-id="54f73-233">Yes</span></span> |<span data-ttu-id="54f73-234">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-234">string</span></span> |<span data-ttu-id="54f73-235">Typ av resurs inklusive resursleverantörens namnrymd.</span><span class="sxs-lookup"><span data-stu-id="54f73-235">Type of resource including resource provider namespace.</span></span> |
| <span data-ttu-id="54f73-236">resourceName1</span><span class="sxs-lookup"><span data-stu-id="54f73-236">resourceName1</span></span> |<span data-ttu-id="54f73-237">Ja</span><span class="sxs-lookup"><span data-stu-id="54f73-237">Yes</span></span> |<span data-ttu-id="54f73-238">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-238">string</span></span> |<span data-ttu-id="54f73-239">Namnet på resursen.</span><span class="sxs-lookup"><span data-stu-id="54f73-239">Name of resource.</span></span> |
| <span data-ttu-id="54f73-240">resourceName2</span><span class="sxs-lookup"><span data-stu-id="54f73-240">resourceName2</span></span> |<span data-ttu-id="54f73-241">Nej</span><span class="sxs-lookup"><span data-stu-id="54f73-241">No</span></span> |<span data-ttu-id="54f73-242">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-242">string</span></span> |<span data-ttu-id="54f73-243">Nästa resurs namn segment om resursen är kapslad.</span><span class="sxs-lookup"><span data-stu-id="54f73-243">Next resource name segment if resource is nested.</span></span> |

### <a name="return-value"></a><span data-ttu-id="54f73-244">Returvärde</span><span class="sxs-lookup"><span data-stu-id="54f73-244">Return value</span></span>

<span data-ttu-id="54f73-245">Identifieraren returneras i följande format:</span><span class="sxs-lookup"><span data-stu-id="54f73-245">The identifier is returned in the following format:</span></span>

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a><span data-ttu-id="54f73-246">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="54f73-246">Remarks</span></span>

<span data-ttu-id="54f73-247">De parametervärden som du anger beror på om resursen är i samma prenumeration och resurs grupp som den aktuella distributionen.</span><span class="sxs-lookup"><span data-stu-id="54f73-247">The parameter values you specify depend on whether the resource is in the same subscription and resource group as the current deployment.</span></span>

<span data-ttu-id="54f73-248">För att få resurs-ID för ett lagringskonto i samma prenumeration och resursgrupp, använder du:</span><span class="sxs-lookup"><span data-stu-id="54f73-248">To get the resource ID for a storage account in the same subscription and resource group, use:</span></span>

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="54f73-249">För att få resurs-ID för ett lagringskonto i samma prenumeration, men en annan resursgrupp, använder du:</span><span class="sxs-lookup"><span data-stu-id="54f73-249">To get the resource ID for a storage account in the same subscription but a different resource group, use:</span></span>

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="54f73-250">För att få resurs-ID för ett lagringskonto i en annan prenumeration och resursgrupp, använder du:</span><span class="sxs-lookup"><span data-stu-id="54f73-250">To get the resource ID for a storage account in a different subscription and resource group, use:</span></span>

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="54f73-251">För att få resurs-ID för en databas i en annan resursgrupp, använder du:</span><span class="sxs-lookup"><span data-stu-id="54f73-251">To get the resource ID for a database in a different resource group, use:</span></span>

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

<span data-ttu-id="54f73-252">Du behöver ofta, Använd den här funktionen när du använder ett lagringskonto eller ett virtuellt nätverk i en annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="54f73-252">Often, you need to use this function when using a storage account or virtual network in an alternate resource group.</span></span> <span data-ttu-id="54f73-253">I följande exempel visas hur en resurs från en extern resursgrupp enkelt kan användas:</span><span class="sxs-lookup"><span data-stu-id="54f73-253">The following example shows how a resource from an external resource group can easily be used:</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualNetworkName": {
          "type": "string"
      },
      "virtualNetworkResourceGroup": {
          "type": "string"
      },
      "subnet1Name": {
          "type": "string"
      },
      "nicName": {
          "type": "string"
      }
  },
  "variables": {
      "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
      "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
  },
  "resources": [
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
          "ipConfigurations": [{
              "name": "ipconfig1",
              "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "subnet": {
                      "id": "[variables('subnet1Ref')]"
                  }
              }
          }]
       }
  }]
}
```

### <a name="example"></a><span data-ttu-id="54f73-254">Exempel</span><span class="sxs-lookup"><span data-stu-id="54f73-254">Example</span></span>

<span data-ttu-id="54f73-255">I följande exempel returneras resurs-ID för ett lagringskonto i resursgruppen:</span><span class="sxs-lookup"><span data-stu-id="54f73-255">The following example returns the resource ID for a storage account in the resource group:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "sameRGOutput": {
            "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentRGOutput": {
            "value": "[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentSubOutput": {
            "value": "[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "nestedResourceOutput": {
            "value": "[resourceId('Microsoft.SQL/servers/databases', 'serverName', 'databaseName')]",
            "type" : "string"
        }
    }
}
```

<span data-ttu-id="54f73-256">Utdata från det föregående exemplet med standardvärdena är:</span><span class="sxs-lookup"><span data-stu-id="54f73-256">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="54f73-257">Namn</span><span class="sxs-lookup"><span data-stu-id="54f73-257">Name</span></span> | <span data-ttu-id="54f73-258">Typ</span><span class="sxs-lookup"><span data-stu-id="54f73-258">Type</span></span> | <span data-ttu-id="54f73-259">Värde</span><span class="sxs-lookup"><span data-stu-id="54f73-259">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="54f73-260">sameRGOutput</span><span class="sxs-lookup"><span data-stu-id="54f73-260">sameRGOutput</span></span> | <span data-ttu-id="54f73-261">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-261">String</span></span> | <span data-ttu-id="54f73-262">/subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="54f73-262">/subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="54f73-263">differentRGOutput</span><span class="sxs-lookup"><span data-stu-id="54f73-263">differentRGOutput</span></span> | <span data-ttu-id="54f73-264">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-264">String</span></span> | <span data-ttu-id="54f73-265">/subscriptions/{Current-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="54f73-265">/subscriptions/{current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="54f73-266">differentSubOutput</span><span class="sxs-lookup"><span data-stu-id="54f73-266">differentSubOutput</span></span> | <span data-ttu-id="54f73-267">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-267">String</span></span> | <span data-ttu-id="54f73-268">/subscriptions/{different-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="54f73-268">/subscriptions/{different-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="54f73-269">nestedResourceOutput</span><span class="sxs-lookup"><span data-stu-id="54f73-269">nestedResourceOutput</span></span> | <span data-ttu-id="54f73-270">Sträng</span><span class="sxs-lookup"><span data-stu-id="54f73-270">String</span></span> | <span data-ttu-id="54f73-271">/subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.SQL/Servers/ServerName/Databases/databaseName</span><span class="sxs-lookup"><span data-stu-id="54f73-271">/subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databases/databaseName</span></span> |

<a id="subscription" />

## <a name="subscription"></a><span data-ttu-id="54f73-272">prenumeration</span><span class="sxs-lookup"><span data-stu-id="54f73-272">subscription</span></span>
`subscription()`

<span data-ttu-id="54f73-273">Returnerar information om prenumerationen för den aktuella distributionen.</span><span class="sxs-lookup"><span data-stu-id="54f73-273">Returns details about the subscription for the current deployment.</span></span> 

### <a name="return-value"></a><span data-ttu-id="54f73-274">Returvärde</span><span class="sxs-lookup"><span data-stu-id="54f73-274">Return value</span></span>

<span data-ttu-id="54f73-275">Funktionen returnerar följande format:</span><span class="sxs-lookup"><span data-stu-id="54f73-275">The function returns the following format:</span></span>

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a><span data-ttu-id="54f73-276">Exempel</span><span class="sxs-lookup"><span data-stu-id="54f73-276">Example</span></span>

<span data-ttu-id="54f73-277">I följande exempel visas funktionen prenumeration anropas i avsnittet utdata.</span><span class="sxs-lookup"><span data-stu-id="54f73-277">The following example shows the subscription function called in the outputs section.</span></span> 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[subscription()]",
            "type" : "object"
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="54f73-278">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="54f73-278">Next steps</span></span>
* <span data-ttu-id="54f73-279">En beskrivning av avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="54f73-279">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="54f73-280">Om du vill slå samman flera mallar, se [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="54f73-280">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="54f73-281">Iterera ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="54f73-281">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="54f73-282">Information om hur du distribuerar mallen som du har skapat finns [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="54f73-282">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

