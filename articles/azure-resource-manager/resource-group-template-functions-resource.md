---
title: "Mallfunktioner för aaaAzure Resource Manager - resurser | Microsoft Docs"
description: "Beskriver hello funktioner toouse i en Azure Resource Manager mallen tooretrieve värden om resurser."
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
ms.openlocfilehash: c9d524b338b8b7ea6d8c9e0135d48e4fb8f167c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="1a4fb-103">Resursfunktioner för Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="1a4fb-103">Resource functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="1a4fb-104">Resource Manager tillhandahåller hello följande funktioner för att hämta resurs värden:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-104">Resource Manager provides hello following functions for getting resource values:</span></span>

* [<span data-ttu-id="1a4fb-105">listKeys och lista {Value}</span><span class="sxs-lookup"><span data-stu-id="1a4fb-105">listKeys and list{Value}</span></span>](#listkeys)
* [<span data-ttu-id="1a4fb-106">providers</span><span class="sxs-lookup"><span data-stu-id="1a4fb-106">providers</span></span>](#providers)
* [<span data-ttu-id="1a4fb-107">referens</span><span class="sxs-lookup"><span data-stu-id="1a4fb-107">reference</span></span>](#reference)
* [<span data-ttu-id="1a4fb-108">resourceGroup</span><span class="sxs-lookup"><span data-stu-id="1a4fb-108">resourceGroup</span></span>](#resourcegroup)
* [<span data-ttu-id="1a4fb-109">resurs-ID</span><span class="sxs-lookup"><span data-stu-id="1a4fb-109">resourceId</span></span>](#resourceid)
* [<span data-ttu-id="1a4fb-110">prenumeration</span><span class="sxs-lookup"><span data-stu-id="1a4fb-110">subscription</span></span>](#subscription)

<span data-ttu-id="1a4fb-111">tooget värden från parametrar, variabler eller hello aktuella distribution, se [distribution värdet funktioner](resource-group-template-functions-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-111">tooget values from parameters, variables, or hello current deployment, see [Deployment value functions](resource-group-template-functions-deployment.md).</span></span>

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-and-listvalue"></a><span data-ttu-id="1a4fb-112">listKeys och lista {Value}</span><span class="sxs-lookup"><span data-stu-id="1a4fb-112">listKeys and list{Value}</span></span>
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

<span data-ttu-id="1a4fb-113">Returnerar hello värden för någon resurstyp av som har stöd för hello listan igen.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-113">Returns hello values for any resource type that supports hello list operation.</span></span> <span data-ttu-id="1a4fb-114">hello vanligaste användningen är `listKeys`.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-114">hello most common usage is `listKeys`.</span></span> 

### <a name="parameters"></a><span data-ttu-id="1a4fb-115">Parametrar</span><span class="sxs-lookup"><span data-stu-id="1a4fb-115">Parameters</span></span>

| <span data-ttu-id="1a4fb-116">Parameter</span><span class="sxs-lookup"><span data-stu-id="1a4fb-116">Parameter</span></span> | <span data-ttu-id="1a4fb-117">Krävs</span><span class="sxs-lookup"><span data-stu-id="1a4fb-117">Required</span></span> | <span data-ttu-id="1a4fb-118">Typ</span><span class="sxs-lookup"><span data-stu-id="1a4fb-118">Type</span></span> | <span data-ttu-id="1a4fb-119">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1a4fb-119">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1a4fb-120">resourceName eller resourceIdentifier</span><span class="sxs-lookup"><span data-stu-id="1a4fb-120">resourceName or resourceIdentifier</span></span> |<span data-ttu-id="1a4fb-121">Ja</span><span class="sxs-lookup"><span data-stu-id="1a4fb-121">Yes</span></span> |<span data-ttu-id="1a4fb-122">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-122">string</span></span> |<span data-ttu-id="1a4fb-123">Unik identifierare för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-123">Unique identifier for hello resource.</span></span> |
| <span data-ttu-id="1a4fb-124">apiVersion</span><span class="sxs-lookup"><span data-stu-id="1a4fb-124">apiVersion</span></span> |<span data-ttu-id="1a4fb-125">Ja</span><span class="sxs-lookup"><span data-stu-id="1a4fb-125">Yes</span></span> |<span data-ttu-id="1a4fb-126">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-126">string</span></span> |<span data-ttu-id="1a4fb-127">API-versionen av resursen runtime-tillståndet.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-127">API version of resource runtime state.</span></span> <span data-ttu-id="1a4fb-128">Normalt i hello-format, **åååå-mm-dd**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-128">Typically, in hello format, **yyyy-mm-dd**.</span></span> |

### <a name="return-value"></a><span data-ttu-id="1a4fb-129">Returvärde</span><span class="sxs-lookup"><span data-stu-id="1a4fb-129">Return value</span></span>

<span data-ttu-id="1a4fb-130">hello returnerade objekt från listKeys har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-130">hello returned object from listKeys has hello following format:</span></span>

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

<span data-ttu-id="1a4fb-131">Andra listfunktioner har olika returnerade format.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-131">Other list functions have different return formats.</span></span> <span data-ttu-id="1a4fb-132">toosee hello format för en funktion, inkludera den i hello utdata avsnittet som visas i hello exempelmall.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-132">toosee hello format of a function, include it in hello outputs section as shown in hello example template.</span></span> 

### <a name="remarks"></a><span data-ttu-id="1a4fb-133">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="1a4fb-133">Remarks</span></span>

<span data-ttu-id="1a4fb-134">Alla åtgärder som börjar med **lista** kan användas som en funktion i mallen.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-134">Any operation that starts with **list** can be used as a function in your template.</span></span> <span data-ttu-id="1a4fb-135">hello tillgängliga åtgärder innehåller inte bara listKeys, men även åtgärder som `list`, `listAdminKeys`, och `listStatus`.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-135">hello available operations include not only listKeys, but also operations like `list`, `listAdminKeys`, and `listStatus`.</span></span> <span data-ttu-id="1a4fb-136">Du kan dock använda **lista** åtgärder som kräver att värden i hello begäran.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-136">However, you cannot use **list** operations that require values in hello request body.</span></span> <span data-ttu-id="1a4fb-137">Till exempel hello [lista kontots SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) åtgärden kräver begärantext som *signedExpiry*, så du inte kan använda den i en mall.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-137">For example, hello [List Account SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) operation requires request body parameters like *signedExpiry*, so you cannot use it within a template.</span></span>

<span data-ttu-id="1a4fb-138">toodetermine vilka resurstyper har en liståtgärd, har du hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-138">toodetermine which resource types have a list operation, you have hello following options:</span></span>

* <span data-ttu-id="1a4fb-139">Visa hello [REST-API: et](/rest/api/) för resursprovidern och leta efter Liståtgärder.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-139">View hello [REST API operations](/rest/api/) for a resource provider, and look for list operations.</span></span> <span data-ttu-id="1a4fb-140">Till exempel storage-konton har hello [listKeys åtgärden](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-140">For example, storage accounts have hello [listKeys operation](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).</span></span>
* <span data-ttu-id="1a4fb-141">Använd hello [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-141">Use hello [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell cmdlet.</span></span> <span data-ttu-id="1a4fb-142">hello hämtas följande exempel alla Liståtgärder för storage-konton:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-142">hello following example gets all list operations for storage accounts:</span></span>

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* <span data-ttu-id="1a4fb-143">Använd följande Azure CLI kommando toofilter endast hello Liståtgärder hello:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-143">Use hello following Azure CLI command toofilter only hello list operations:</span></span>

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

<span data-ttu-id="1a4fb-144">Ange hello resursen med hjälp av antingen hello [resourceId funktionen](#resourceid), eller hello format `{providerNamespace}/{resourceType}/{resourceName}`.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-144">Specify hello resource by using either hello [resourceId function](#resourceid), or hello format `{providerNamespace}/{resourceType}/{resourceName}`.</span></span>


### <a name="example"></a><span data-ttu-id="1a4fb-145">Exempel</span><span class="sxs-lookup"><span data-stu-id="1a4fb-145">Example</span></span>

<span data-ttu-id="1a4fb-146">hello följande exempel visas hur tooreturn hello primära och sekundära nycklarna från ett lagringskonto i hello matar ut avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-146">hello following example shows how tooreturn hello primary and secondary keys from a storage account in hello outputs section.</span></span>

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

## <a name="providers"></a><span data-ttu-id="1a4fb-147">providers</span><span class="sxs-lookup"><span data-stu-id="1a4fb-147">providers</span></span>
`providers(providerNamespace, [resourceType])`

<span data-ttu-id="1a4fb-148">Returnerar information om en resursprovider och dess resurstyper som stöds.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-148">Returns information about a resource provider and its supported resource types.</span></span> <span data-ttu-id="1a4fb-149">Om du inte anger en resurstyp returneras hello alla typer av hello stöds för hello resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-149">If you do not provide a resource type, hello function returns all hello supported types for hello resource provider.</span></span>

### <a name="parameters"></a><span data-ttu-id="1a4fb-150">Parametrar</span><span class="sxs-lookup"><span data-stu-id="1a4fb-150">Parameters</span></span>

| <span data-ttu-id="1a4fb-151">Parameter</span><span class="sxs-lookup"><span data-stu-id="1a4fb-151">Parameter</span></span> | <span data-ttu-id="1a4fb-152">Krävs</span><span class="sxs-lookup"><span data-stu-id="1a4fb-152">Required</span></span> | <span data-ttu-id="1a4fb-153">Typ</span><span class="sxs-lookup"><span data-stu-id="1a4fb-153">Type</span></span> | <span data-ttu-id="1a4fb-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1a4fb-154">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1a4fb-155">providerNamespace</span><span class="sxs-lookup"><span data-stu-id="1a4fb-155">providerNamespace</span></span> |<span data-ttu-id="1a4fb-156">Ja</span><span class="sxs-lookup"><span data-stu-id="1a4fb-156">Yes</span></span> |<span data-ttu-id="1a4fb-157">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-157">string</span></span> |<span data-ttu-id="1a4fb-158">Namespace hello-provider</span><span class="sxs-lookup"><span data-stu-id="1a4fb-158">Namespace of hello provider</span></span> |
| <span data-ttu-id="1a4fb-159">resourceType</span><span class="sxs-lookup"><span data-stu-id="1a4fb-159">resourceType</span></span> |<span data-ttu-id="1a4fb-160">Nej</span><span class="sxs-lookup"><span data-stu-id="1a4fb-160">No</span></span> |<span data-ttu-id="1a4fb-161">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-161">string</span></span> |<span data-ttu-id="1a4fb-162">hello typ av resurs i hello angetts namnområde.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-162">hello type of resource within hello specified namespace.</span></span> |

### <a name="return-value"></a><span data-ttu-id="1a4fb-163">Returvärde</span><span class="sxs-lookup"><span data-stu-id="1a4fb-163">Return value</span></span>

<span data-ttu-id="1a4fb-164">Varje typ som stöds returneras i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-164">Each supported type is returned in hello following format:</span></span> 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

<span data-ttu-id="1a4fb-165">Matrisen sorteringen av hello returnerade värden inte är säkert.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-165">Array ordering of hello returned values is not guaranteed.</span></span>

### <a name="example"></a><span data-ttu-id="1a4fb-166">Exempel</span><span class="sxs-lookup"><span data-stu-id="1a4fb-166">Example</span></span>

<span data-ttu-id="1a4fb-167">hello som följande exempel visar hur toouse hello providern funktionen:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-167">hello following example shows how toouse hello provider function:</span></span>

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

<span data-ttu-id="1a4fb-168">hello föregående exempel returnerar ett objekt i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-168">hello preceding example returns an object in hello following format:</span></span>

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

## <a name="reference"></a><span data-ttu-id="1a4fb-169">Referens</span><span class="sxs-lookup"><span data-stu-id="1a4fb-169">reference</span></span>
`reference(resourceName or resourceIdentifier, [apiVersion])`

<span data-ttu-id="1a4fb-170">Returnerar ett objekt som representerar en resurs runtime-tillståndet.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-170">Returns an object representing a resource's runtime state.</span></span>

### <a name="parameters"></a><span data-ttu-id="1a4fb-171">Parametrar</span><span class="sxs-lookup"><span data-stu-id="1a4fb-171">Parameters</span></span>

| <span data-ttu-id="1a4fb-172">Parameter</span><span class="sxs-lookup"><span data-stu-id="1a4fb-172">Parameter</span></span> | <span data-ttu-id="1a4fb-173">Krävs</span><span class="sxs-lookup"><span data-stu-id="1a4fb-173">Required</span></span> | <span data-ttu-id="1a4fb-174">Typ</span><span class="sxs-lookup"><span data-stu-id="1a4fb-174">Type</span></span> | <span data-ttu-id="1a4fb-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1a4fb-175">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1a4fb-176">resourceName eller resourceIdentifier</span><span class="sxs-lookup"><span data-stu-id="1a4fb-176">resourceName or resourceIdentifier</span></span> |<span data-ttu-id="1a4fb-177">Ja</span><span class="sxs-lookup"><span data-stu-id="1a4fb-177">Yes</span></span> |<span data-ttu-id="1a4fb-178">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-178">string</span></span> |<span data-ttu-id="1a4fb-179">Namn eller unik identifierare för en resurs.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-179">Name or unique identifier of a resource.</span></span> |
| <span data-ttu-id="1a4fb-180">apiVersion</span><span class="sxs-lookup"><span data-stu-id="1a4fb-180">apiVersion</span></span> |<span data-ttu-id="1a4fb-181">Nej</span><span class="sxs-lookup"><span data-stu-id="1a4fb-181">No</span></span> |<span data-ttu-id="1a4fb-182">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-182">string</span></span> |<span data-ttu-id="1a4fb-183">API-versionen av hello angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-183">API version of hello specified resource.</span></span> <span data-ttu-id="1a4fb-184">Inkludera den här parametern när hello resursen inte har etablerats inom samma mall.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-184">Include this parameter when hello resource is not provisioned within same template.</span></span> <span data-ttu-id="1a4fb-185">Normalt i hello-format, **åååå-mm-dd**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-185">Typically, in hello format, **yyyy-mm-dd**.</span></span> |

### <a name="return-value"></a><span data-ttu-id="1a4fb-186">Returvärde</span><span class="sxs-lookup"><span data-stu-id="1a4fb-186">Return value</span></span>

<span data-ttu-id="1a4fb-187">Varje resurstypen returnerar andra egenskaper för hello referens-funktionen.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-187">Every resource type returns different properties for hello reference function.</span></span> <span data-ttu-id="1a4fb-188">hello funktionen returnerar inte ett enda fördefinierade format.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-188">hello function does not return a single, predefined format.</span></span> <span data-ttu-id="1a4fb-189">toosee hello egenskaper för en resurstyp returnerar hello objekt i hello matar ut avsnittet enligt hello exempel.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-189">toosee hello properties for a resource type, return hello object in hello outputs section as shown in hello example.</span></span>

### <a name="remarks"></a><span data-ttu-id="1a4fb-190">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="1a4fb-190">Remarks</span></span>

<span data-ttu-id="1a4fb-191">hello referens funktionen hämtar sitt värde från en runtime-tillståndet och kan därför inte användas i hello variabler avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-191">hello reference function derives its value from a runtime state, and therefore cannot be used in hello variables section.</span></span> <span data-ttu-id="1a4fb-192">Det kan användas i utdata avsnitt i en mall.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-192">It can be used in outputs section of a template.</span></span> 

<span data-ttu-id="1a4fb-193">Med funktionen hello referens deklarera du implicit att en resurs beror på en annan resurs om hello refererar till resursen har etablerats i samma mall.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-193">By using hello reference function, you implicitly declare that one resource depends on another resource if hello referenced resource is provisioned within same template.</span></span> <span data-ttu-id="1a4fb-194">Du behöver inte tooalso Använd hello dependsOn-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-194">You do not need tooalso use hello dependsOn property.</span></span> <span data-ttu-id="1a4fb-195">hello utvärderas funktionen inte förrän hello refererade resursen har slutfört distributionen.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-195">hello function is not evaluated until hello referenced resource has completed deployment.</span></span>

<span data-ttu-id="1a4fb-196">Skapa en mall som hämtar hello objekt i hello utdata toosee hello egenskapsnamn och värden för en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-196">toosee hello property names and values for a resource type, create a template that returns hello object in hello outputs section.</span></span> <span data-ttu-id="1a4fb-197">Om du har en befintlig resurs av den typen returnerar mallen hello-objekt utan att distribuera nya resurser.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-197">If you have an existing resource of that type, your template returns hello object without deploying any new resources.</span></span> 

<span data-ttu-id="1a4fb-198">Normalt använder du hello **referens** fungerar tooreturn ett visst värde från ett objekt, t.ex slutpunkt för blob-hello URI eller fullständigt domännamn.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-198">Typically, you use hello **reference** function tooreturn a particular value from an object, such as hello blob endpoint URI or fully qualified domain name.</span></span>

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

### <a name="example"></a><span data-ttu-id="1a4fb-199">Exempel</span><span class="sxs-lookup"><span data-stu-id="1a4fb-199">Example</span></span>

<span data-ttu-id="1a4fb-200">referens- och toodeploy hello-resurs i hello samma mall, Använd:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-200">toodeploy and reference hello resource in hello same template, use:</span></span>

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

<span data-ttu-id="1a4fb-201">hello föregående exempel returnerar ett objekt i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-201">hello preceding example returns an object in hello following format:</span></span>

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

<span data-ttu-id="1a4fb-202">hello följande exempel refererar till ett lagringskonto som inte har distribuerats i den här mallen.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-202">hello following example references a storage account that is not deployed in this template.</span></span> <span data-ttu-id="1a4fb-203">hello storage-konto finns redan i hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-203">hello storage account already exists within hello same resource group.</span></span>

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

## <a name="resourcegroup"></a><span data-ttu-id="1a4fb-204">resourceGroup</span><span class="sxs-lookup"><span data-stu-id="1a4fb-204">resourceGroup</span></span>
`resourceGroup()`

<span data-ttu-id="1a4fb-205">Returnerar ett objekt som representerar hello aktuella resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-205">Returns an object that represents hello current resource group.</span></span> 

### <a name="return-value"></a><span data-ttu-id="1a4fb-206">Returvärde</span><span class="sxs-lookup"><span data-stu-id="1a4fb-206">Return value</span></span>

<span data-ttu-id="1a4fb-207">hello returnerade objektet är i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-207">hello returned object is in hello following format:</span></span>

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

### <a name="remarks"></a><span data-ttu-id="1a4fb-208">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="1a4fb-208">Remarks</span></span>

<span data-ttu-id="1a4fb-209">Ett vanligt användningsområde för hello resourceGroup funktionen är toocreate resurser i hello samma plats som hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-209">A common use of hello resourceGroup function is toocreate resources in hello same location as hello resource group.</span></span> <span data-ttu-id="1a4fb-210">hello följande exempel används hello resursgruppens plats tooassign hello plats för en webbplats.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-210">hello following example uses hello resource group location tooassign hello location for a web site.</span></span>

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

### <a name="example"></a><span data-ttu-id="1a4fb-211">Exempel</span><span class="sxs-lookup"><span data-stu-id="1a4fb-211">Example</span></span>

<span data-ttu-id="1a4fb-212">hello returnerar följande mall hello hello resursgruppens egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-212">hello following template returns hello properties of hello resource group.</span></span>

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

<span data-ttu-id="1a4fb-213">hello föregående exempel returnerar ett objekt i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-213">hello preceding example returns an object in hello following format:</span></span>

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

## <a name="resourceid"></a><span data-ttu-id="1a4fb-214">resourceId</span><span class="sxs-lookup"><span data-stu-id="1a4fb-214">resourceId</span></span>
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

<span data-ttu-id="1a4fb-215">Returnerar hello Unik identifierare för en resurs.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-215">Returns hello unique identifier of a resource.</span></span> <span data-ttu-id="1a4fb-216">Du använder den här funktionen när hello resursnamnet är tvetydigt eller inte etablerade inom hello samma mall.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-216">You use this function when hello resource name is ambiguous or not provisioned within hello same template.</span></span> 

### <a name="parameters"></a><span data-ttu-id="1a4fb-217">Parametrar</span><span class="sxs-lookup"><span data-stu-id="1a4fb-217">Parameters</span></span>

| <span data-ttu-id="1a4fb-218">Parameter</span><span class="sxs-lookup"><span data-stu-id="1a4fb-218">Parameter</span></span> | <span data-ttu-id="1a4fb-219">Krävs</span><span class="sxs-lookup"><span data-stu-id="1a4fb-219">Required</span></span> | <span data-ttu-id="1a4fb-220">Typ</span><span class="sxs-lookup"><span data-stu-id="1a4fb-220">Type</span></span> | <span data-ttu-id="1a4fb-221">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1a4fb-221">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1a4fb-222">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="1a4fb-222">subscriptionId</span></span> |<span data-ttu-id="1a4fb-223">Nej</span><span class="sxs-lookup"><span data-stu-id="1a4fb-223">No</span></span> |<span data-ttu-id="1a4fb-224">sträng (i GUID-format)</span><span class="sxs-lookup"><span data-stu-id="1a4fb-224">string (In GUID format)</span></span> |<span data-ttu-id="1a4fb-225">Standardvärdet är hello aktuell prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-225">Default value is hello current subscription.</span></span> <span data-ttu-id="1a4fb-226">Ange det här värdet när du behöver tooretrieve en resurs i en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-226">Specify this value when you need tooretrieve a resource in another subscription.</span></span> |
| <span data-ttu-id="1a4fb-227">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="1a4fb-227">resourceGroupName</span></span> |<span data-ttu-id="1a4fb-228">Nej</span><span class="sxs-lookup"><span data-stu-id="1a4fb-228">No</span></span> |<span data-ttu-id="1a4fb-229">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-229">string</span></span> |<span data-ttu-id="1a4fb-230">Standardvärdet är aktuella resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-230">Default value is current resource group.</span></span> <span data-ttu-id="1a4fb-231">Ange det här värdet när du behöver tooretrieve en resurs i en annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-231">Specify this value when you need tooretrieve a resource in another resource group.</span></span> |
| <span data-ttu-id="1a4fb-232">resourceType</span><span class="sxs-lookup"><span data-stu-id="1a4fb-232">resourceType</span></span> |<span data-ttu-id="1a4fb-233">Ja</span><span class="sxs-lookup"><span data-stu-id="1a4fb-233">Yes</span></span> |<span data-ttu-id="1a4fb-234">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-234">string</span></span> |<span data-ttu-id="1a4fb-235">Typ av resurs inklusive resursleverantörens namnrymd.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-235">Type of resource including resource provider namespace.</span></span> |
| <span data-ttu-id="1a4fb-236">resourceName1</span><span class="sxs-lookup"><span data-stu-id="1a4fb-236">resourceName1</span></span> |<span data-ttu-id="1a4fb-237">Ja</span><span class="sxs-lookup"><span data-stu-id="1a4fb-237">Yes</span></span> |<span data-ttu-id="1a4fb-238">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-238">string</span></span> |<span data-ttu-id="1a4fb-239">Namnet på resursen.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-239">Name of resource.</span></span> |
| <span data-ttu-id="1a4fb-240">resourceName2</span><span class="sxs-lookup"><span data-stu-id="1a4fb-240">resourceName2</span></span> |<span data-ttu-id="1a4fb-241">Nej</span><span class="sxs-lookup"><span data-stu-id="1a4fb-241">No</span></span> |<span data-ttu-id="1a4fb-242">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-242">string</span></span> |<span data-ttu-id="1a4fb-243">Nästa resurs namn segment om resursen är kapslad.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-243">Next resource name segment if resource is nested.</span></span> |

### <a name="return-value"></a><span data-ttu-id="1a4fb-244">Returvärde</span><span class="sxs-lookup"><span data-stu-id="1a4fb-244">Return value</span></span>

<span data-ttu-id="1a4fb-245">hello identifierare returneras i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-245">hello identifier is returned in hello following format:</span></span>

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a><span data-ttu-id="1a4fb-246">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="1a4fb-246">Remarks</span></span>

<span data-ttu-id="1a4fb-247">hello parametervärden som du anger beror på om hello resursen är i hello samma prenumeration och resurs grupp som hello aktuella distributionen.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-247">hello parameter values you specify depend on whether hello resource is in hello same subscription and resource group as hello current deployment.</span></span>

<span data-ttu-id="1a4fb-248">tooget hello resurs-ID för ett lagringskonto i hello samma prenumeration och resursgrupp, Använd:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-248">tooget hello resource ID for a storage account in hello same subscription and resource group, use:</span></span>

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="1a4fb-249">tooget hello resurs-ID för ett lagringskonto i hello samma prenumeration, men en annan resursgrupp, Använd:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-249">tooget hello resource ID for a storage account in hello same subscription but a different resource group, use:</span></span>

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="1a4fb-250">tooget hello resurs-ID för ett lagringskonto i en annan prenumeration och resursgrupp, Använd:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-250">tooget hello resource ID for a storage account in a different subscription and resource group, use:</span></span>

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="1a4fb-251">Använd tooget hello resurs-ID för en databas i en annan resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-251">tooget hello resource ID for a database in a different resource group, use:</span></span>

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

<span data-ttu-id="1a4fb-252">Ofta måste toouse den här funktionen när du använder ett lagringskonto eller ett virtuellt nätverk i en annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-252">Often, you need toouse this function when using a storage account or virtual network in an alternate resource group.</span></span> <span data-ttu-id="1a4fb-253">hello följande exempel visas hur en resurs från en extern resursgrupp enkelt kan användas:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-253">hello following example shows how a resource from an external resource group can easily be used:</span></span>

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

### <a name="example"></a><span data-ttu-id="1a4fb-254">Exempel</span><span class="sxs-lookup"><span data-stu-id="1a4fb-254">Example</span></span>

<span data-ttu-id="1a4fb-255">hello följande exempel returnerar hello resurs-ID för ett lagringskonto i hello resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-255">hello following example returns hello resource ID for a storage account in hello resource group:</span></span>

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

<span data-ttu-id="1a4fb-256">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-256">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="1a4fb-257">Namn</span><span class="sxs-lookup"><span data-stu-id="1a4fb-257">Name</span></span> | <span data-ttu-id="1a4fb-258">Typ</span><span class="sxs-lookup"><span data-stu-id="1a4fb-258">Type</span></span> | <span data-ttu-id="1a4fb-259">Värde</span><span class="sxs-lookup"><span data-stu-id="1a4fb-259">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="1a4fb-260">sameRGOutput</span><span class="sxs-lookup"><span data-stu-id="1a4fb-260">sameRGOutput</span></span> | <span data-ttu-id="1a4fb-261">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-261">String</span></span> | <span data-ttu-id="1a4fb-262">/subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="1a4fb-262">/subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="1a4fb-263">differentRGOutput</span><span class="sxs-lookup"><span data-stu-id="1a4fb-263">differentRGOutput</span></span> | <span data-ttu-id="1a4fb-264">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-264">String</span></span> | <span data-ttu-id="1a4fb-265">/subscriptions/{Current-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="1a4fb-265">/subscriptions/{current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="1a4fb-266">differentSubOutput</span><span class="sxs-lookup"><span data-stu-id="1a4fb-266">differentSubOutput</span></span> | <span data-ttu-id="1a4fb-267">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-267">String</span></span> | <span data-ttu-id="1a4fb-268">/subscriptions/{different-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="1a4fb-268">/subscriptions/{different-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="1a4fb-269">nestedResourceOutput</span><span class="sxs-lookup"><span data-stu-id="1a4fb-269">nestedResourceOutput</span></span> | <span data-ttu-id="1a4fb-270">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a4fb-270">String</span></span> | <span data-ttu-id="1a4fb-271">/subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.SQL/Servers/ServerName/Databases/databaseName</span><span class="sxs-lookup"><span data-stu-id="1a4fb-271">/subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databases/databaseName</span></span> |

<a id="subscription" />

## <a name="subscription"></a><span data-ttu-id="1a4fb-272">prenumeration</span><span class="sxs-lookup"><span data-stu-id="1a4fb-272">subscription</span></span>
`subscription()`

<span data-ttu-id="1a4fb-273">Returnerar information om hello prenumerationen för hello aktuella distributionen.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-273">Returns details about hello subscription for hello current deployment.</span></span> 

### <a name="return-value"></a><span data-ttu-id="1a4fb-274">Returvärde</span><span class="sxs-lookup"><span data-stu-id="1a4fb-274">Return value</span></span>

<span data-ttu-id="1a4fb-275">hello funktionen returnerar hello följande format:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-275">hello function returns hello following format:</span></span>

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a><span data-ttu-id="1a4fb-276">Exempel</span><span class="sxs-lookup"><span data-stu-id="1a4fb-276">Example</span></span>

<span data-ttu-id="1a4fb-277">hello visar följande exempel hello prenumeration funktionen anropas under hello utdata.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-277">hello following example shows hello subscription function called in hello outputs section.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="1a4fb-278">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a4fb-278">Next steps</span></span>
* <span data-ttu-id="1a4fb-279">En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-279">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="1a4fb-280">flera mallar finns i toomerge [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-280">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="1a4fb-281">tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-281">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="1a4fb-282">toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-282">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

