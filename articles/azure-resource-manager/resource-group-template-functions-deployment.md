---
title: "Mallfunktioner för aaaAzure Resource Manager - distribution | Microsoft Docs"
description: Beskriver hello funktioner toouse i en Azure Resource Manager mallen tooretrieve distributionsinformation.
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
ms.date: 06/13/2017
ms.author: tomfitz
ms.openlocfilehash: 458c3f740504fdd6799ed24cc386219726737636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="3214e-103">Distribution av funktioner för Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="3214e-103">Deployment functions for Azure Resource Manager templates</span></span> 

<span data-ttu-id="3214e-104">Resource Manager tillhandahåller hello följande funktioner för att hämta värden från delar av hello mall och värden relaterade toohello distribution:</span><span class="sxs-lookup"><span data-stu-id="3214e-104">Resource Manager provides hello following functions for getting values from sections of hello template and values related toohello deployment:</span></span>

* [<span data-ttu-id="3214e-105">distribution</span><span class="sxs-lookup"><span data-stu-id="3214e-105">deployment</span></span>](#deployment)
* [<span data-ttu-id="3214e-106">parametrar</span><span class="sxs-lookup"><span data-stu-id="3214e-106">parameters</span></span>](#parameters)
* [<span data-ttu-id="3214e-107">variabler</span><span class="sxs-lookup"><span data-stu-id="3214e-107">variables</span></span>](#variables)

<span data-ttu-id="3214e-108">tooget värden från resurser, resursgrupper eller prenumerationer, finns [resursfunktioner](resource-group-template-functions-resource.md).</span><span class="sxs-lookup"><span data-stu-id="3214e-108">tooget values from resources, resource groups, or subscriptions, see [Resource functions](resource-group-template-functions-resource.md).</span></span>

<a id="deployment" />

## <a name="deployment"></a><span data-ttu-id="3214e-109">distribution</span><span class="sxs-lookup"><span data-stu-id="3214e-109">deployment</span></span>
`deployment()`

<span data-ttu-id="3214e-110">Returnerar information om hello aktuella distributionsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="3214e-110">Returns information about hello current deployment operation.</span></span>

### <a name="return-value"></a><span data-ttu-id="3214e-111">Returvärde</span><span class="sxs-lookup"><span data-stu-id="3214e-111">Return value</span></span>

<span data-ttu-id="3214e-112">Den här funktionen returnerar hello-objektet som överförs under distributionen.</span><span class="sxs-lookup"><span data-stu-id="3214e-112">This function returns hello object that is passed during deployment.</span></span> <span data-ttu-id="3214e-113">hello egenskaper i hello returnerade objekt variera beroende på om hello distributionsobjektet skickas som en länk eller som ett objekt i rad.</span><span class="sxs-lookup"><span data-stu-id="3214e-113">hello properties in hello returned object differ based on whether hello deployment object is passed as a link or as an in-line object.</span></span> <span data-ttu-id="3214e-114">När hello distributionsobjektet skickas i rad, som när du använder hello **- TemplateFile** parameter i Azure PowerShell toopoint tooa lokal fil, hello returnerade objekt har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="3214e-114">When hello deployment object is passed in-line, such as when using hello **-TemplateFile** parameter in Azure PowerShell toopoint tooa local file, hello returned object has hello following format:</span></span>

```json
{
    "name": "",
    "properties": {
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [
            ],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

<span data-ttu-id="3214e-115">När hello objektet skickas som en länk till exempel när du använder hello **- TemplateUri** parametern toopoint tooa remote objekt hello objekt returneras i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="3214e-115">When hello object is passed as a link, such as when using hello **-TemplateUri** parameter toopoint tooa remote object, hello object is returned in hello following format:</span></span> 

```json
{
    "name": "",
    "properties": {
        "templateLink": {
            "uri": ""
        },
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

### <a name="remarks"></a><span data-ttu-id="3214e-116">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="3214e-116">Remarks</span></span>

<span data-ttu-id="3214e-117">Du kan använda deployment() toolink tooanother mall baserat på hello URI för hello överordnade mallen.</span><span class="sxs-lookup"><span data-stu-id="3214e-117">You can use deployment() toolink tooanother template based on hello URI of hello parent template.</span></span>

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

### <a name="example"></a><span data-ttu-id="3214e-118">Exempel</span><span class="sxs-lookup"><span data-stu-id="3214e-118">Example</span></span>

<span data-ttu-id="3214e-119">hello returnerar följande exempel hello distributionsobjektet:</span><span class="sxs-lookup"><span data-stu-id="3214e-119">hello following example returns hello deployment object:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[deployment()]",
            "type" : "object"
        }
    }
}
```

<span data-ttu-id="3214e-120">hello returnerar föregående exempel hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="3214e-120">hello preceding example returns hello following object:</span></span>

```json
{
  "name": "deployment",
  "properties": {
    "template": {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "resources": [],
      "outputs": {
        "subscriptionOutput": {
          "type": "Object",
          "value": "[deployment()]"
        }
      }
    },
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Accepted"
  }
}
```

<a id="parameters" />

## <a name="parameters"></a><span data-ttu-id="3214e-121">parameters</span><span class="sxs-lookup"><span data-stu-id="3214e-121">parameters</span></span>
`parameters(parameterName)`

<span data-ttu-id="3214e-122">Returnerar ett parametervärde.</span><span class="sxs-lookup"><span data-stu-id="3214e-122">Returns a parameter value.</span></span> <span data-ttu-id="3214e-123">hello angivna parameternamnet måste definieras under hello parametrar i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="3214e-123">hello specified parameter name must be defined in hello parameters section of hello template.</span></span>

### <a name="parameters"></a><span data-ttu-id="3214e-124">Parametrar</span><span class="sxs-lookup"><span data-stu-id="3214e-124">Parameters</span></span>

| <span data-ttu-id="3214e-125">Parameter</span><span class="sxs-lookup"><span data-stu-id="3214e-125">Parameter</span></span> | <span data-ttu-id="3214e-126">Krävs</span><span class="sxs-lookup"><span data-stu-id="3214e-126">Required</span></span> | <span data-ttu-id="3214e-127">Typ</span><span class="sxs-lookup"><span data-stu-id="3214e-127">Type</span></span> | <span data-ttu-id="3214e-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3214e-128">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="3214e-129">parameterName</span><span class="sxs-lookup"><span data-stu-id="3214e-129">parameterName</span></span> |<span data-ttu-id="3214e-130">Ja</span><span class="sxs-lookup"><span data-stu-id="3214e-130">Yes</span></span> |<span data-ttu-id="3214e-131">Sträng</span><span class="sxs-lookup"><span data-stu-id="3214e-131">string</span></span> |<span data-ttu-id="3214e-132">hello namnet på hello parametern tooreturn.</span><span class="sxs-lookup"><span data-stu-id="3214e-132">hello name of hello parameter tooreturn.</span></span> |

### <a name="return-value"></a><span data-ttu-id="3214e-133">Returvärde</span><span class="sxs-lookup"><span data-stu-id="3214e-133">Return value</span></span>

<span data-ttu-id="3214e-134">hello-värdet för hello angetts parametern.</span><span class="sxs-lookup"><span data-stu-id="3214e-134">hello value of hello specified parameter.</span></span>

### <a name="remarks"></a><span data-ttu-id="3214e-135">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="3214e-135">Remarks</span></span>

<span data-ttu-id="3214e-136">Normalt använder du parametrarna tooset resurs-värden.</span><span class="sxs-lookup"><span data-stu-id="3214e-136">Typically, you use parameters tooset resource values.</span></span> <span data-ttu-id="3214e-137">hello anger följande exempel hello namnet på webbplatsen toohello parametervärdet skickades under distributionen.</span><span class="sxs-lookup"><span data-stu-id="3214e-137">hello following example sets hello name of web site toohello parameter value passed in during deployment.</span></span>

```json
"parameters": { 
  "siteName": {
      "type": "string"
  }
},
"resources": [
   {
      "apiVersion": "2016-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/Sites",
      ...
   }
]
```

### <a name="example"></a><span data-ttu-id="3214e-138">Exempel</span><span class="sxs-lookup"><span data-stu-id="3214e-138">Example</span></span>

<span data-ttu-id="3214e-139">hello visas följande exempel en förenklad användning av funktionen för hello-parametrar.</span><span class="sxs-lookup"><span data-stu-id="3214e-139">hello following example shows a simplified use of hello parameters function.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringParameter": {
            "type" : "string",
            "defaultValue": "option 1"
        },
        "intParameter": {
            "type": "int",
            "defaultValue": 1
        },
        "objectParameter": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b"}
        },
        "arrayParameter": {
            "type": "array",
            "defaultValue": [1, 2, 3]
        },
        "crossParameter": {
            "type": "string",
            "defaultValue": "[parameters('stringParameter')]"
        }
    },
    "variables": {},
    "resources": [],
    "outputs": {
        "stringOutput": {
            "value": "[parameters('stringParameter')]",
            "type" : "string"
        },
        "intOutput": {
            "value": "[parameters('intParameter')]",
            "type" : "int"
        },
        "objectOutput": {
            "value": "[parameters('objectParameter')]",
            "type" : "object"
        },
        "arrayOutput": {
            "value": "[parameters('arrayParameter')]",
            "type" : "array"
        },
        "crossOutput": {
            "value": "[parameters('crossParameter')]",
            "type" : "string"
        }
    }
}
```

<span data-ttu-id="3214e-140">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="3214e-140">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="3214e-141">Namn</span><span class="sxs-lookup"><span data-stu-id="3214e-141">Name</span></span> | <span data-ttu-id="3214e-142">Typ</span><span class="sxs-lookup"><span data-stu-id="3214e-142">Type</span></span> | <span data-ttu-id="3214e-143">Värde</span><span class="sxs-lookup"><span data-stu-id="3214e-143">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="3214e-144">stringOutput</span><span class="sxs-lookup"><span data-stu-id="3214e-144">stringOutput</span></span> | <span data-ttu-id="3214e-145">Sträng</span><span class="sxs-lookup"><span data-stu-id="3214e-145">String</span></span> | <span data-ttu-id="3214e-146">Alternativ 1</span><span class="sxs-lookup"><span data-stu-id="3214e-146">option 1</span></span> |
| <span data-ttu-id="3214e-147">intOutput</span><span class="sxs-lookup"><span data-stu-id="3214e-147">intOutput</span></span> | <span data-ttu-id="3214e-148">int</span><span class="sxs-lookup"><span data-stu-id="3214e-148">Int</span></span> | <span data-ttu-id="3214e-149">1</span><span class="sxs-lookup"><span data-stu-id="3214e-149">1</span></span> |
| <span data-ttu-id="3214e-150">objectOutput</span><span class="sxs-lookup"><span data-stu-id="3214e-150">objectOutput</span></span> | <span data-ttu-id="3214e-151">Objekt</span><span class="sxs-lookup"><span data-stu-id="3214e-151">Object</span></span> | <span data-ttu-id="3214e-152">{”1”: ”a”, ”två”: ”b”}</span><span class="sxs-lookup"><span data-stu-id="3214e-152">{"one": "a", "two": "b"}</span></span> |
| <span data-ttu-id="3214e-153">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="3214e-153">arrayOutput</span></span> | <span data-ttu-id="3214e-154">Matris</span><span class="sxs-lookup"><span data-stu-id="3214e-154">Array</span></span> | <span data-ttu-id="3214e-155">[1, 2, 3]</span><span class="sxs-lookup"><span data-stu-id="3214e-155">[1, 2, 3]</span></span> |
| <span data-ttu-id="3214e-156">crossOutput</span><span class="sxs-lookup"><span data-stu-id="3214e-156">crossOutput</span></span> | <span data-ttu-id="3214e-157">Sträng</span><span class="sxs-lookup"><span data-stu-id="3214e-157">String</span></span> | <span data-ttu-id="3214e-158">Alternativ 1</span><span class="sxs-lookup"><span data-stu-id="3214e-158">option 1</span></span> |

<a id="variables" />

## <a name="variables"></a><span data-ttu-id="3214e-159">variabler</span><span class="sxs-lookup"><span data-stu-id="3214e-159">variables</span></span>
`variables(variableName)`

<span data-ttu-id="3214e-160">Returnerar hello värdet för variabeln.</span><span class="sxs-lookup"><span data-stu-id="3214e-160">Returns hello value of variable.</span></span> <span data-ttu-id="3214e-161">hello angivna variabelnamnet måste definieras under hello variabler i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="3214e-161">hello specified variable name must be defined in hello variables section of hello template.</span></span>

### <a name="parameters"></a><span data-ttu-id="3214e-162">Parametrar</span><span class="sxs-lookup"><span data-stu-id="3214e-162">Parameters</span></span>

| <span data-ttu-id="3214e-163">Parameter</span><span class="sxs-lookup"><span data-stu-id="3214e-163">Parameter</span></span> | <span data-ttu-id="3214e-164">Krävs</span><span class="sxs-lookup"><span data-stu-id="3214e-164">Required</span></span> | <span data-ttu-id="3214e-165">Typ</span><span class="sxs-lookup"><span data-stu-id="3214e-165">Type</span></span> | <span data-ttu-id="3214e-166">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3214e-166">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="3214e-167">Variabelnamn</span><span class="sxs-lookup"><span data-stu-id="3214e-167">variableName</span></span> |<span data-ttu-id="3214e-168">Ja</span><span class="sxs-lookup"><span data-stu-id="3214e-168">Yes</span></span> |<span data-ttu-id="3214e-169">Sträng</span><span class="sxs-lookup"><span data-stu-id="3214e-169">String</span></span> |<span data-ttu-id="3214e-170">hello namnet på variabeln hello-tooreturn.</span><span class="sxs-lookup"><span data-stu-id="3214e-170">hello name of hello variable tooreturn.</span></span> |

### <a name="return-value"></a><span data-ttu-id="3214e-171">Returvärde</span><span class="sxs-lookup"><span data-stu-id="3214e-171">Return value</span></span>

<span data-ttu-id="3214e-172">hello-värdet för angivna hello-variabeln.</span><span class="sxs-lookup"><span data-stu-id="3214e-172">hello value of hello specified variable.</span></span>

### <a name="remarks"></a><span data-ttu-id="3214e-173">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="3214e-173">Remarks</span></span>

<span data-ttu-id="3214e-174">Normalt använder du variabler toosimplify mallen genom att skapa komplexa värden bara en gång.</span><span class="sxs-lookup"><span data-stu-id="3214e-174">Typically, you use variables toosimplify your template by constructing complex values only once.</span></span> <span data-ttu-id="3214e-175">hello skapas följande exempel ett unikt namn för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3214e-175">hello following example constructs a unique name for a storage account.</span></span>

```json
"variables": {
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
},
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
    },
    {
        "type": "Microsoft.Compute/virtualMachines",
        "dependsOn": [
            "[variables('storageName')]"
        ],
        ...
    }
],
```

### <a name="example"></a><span data-ttu-id="3214e-176">Exempel</span><span class="sxs-lookup"><span data-stu-id="3214e-176">Example</span></span>

<span data-ttu-id="3214e-177">hello exempelmall returnerar olika variabelvärden.</span><span class="sxs-lookup"><span data-stu-id="3214e-177">hello example template returns different variable values.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {
        "var1": "myVariable",
        "var2": [ 1,2,3,4 ],
        "var3": "[ variables('var1') ]",
        "var4": {
            "property1": "value1",
            "property2": "value2"
        }
    },
    "resources": [],
    "outputs": {
        "exampleOutput1": {
            "value": "[variables('var1')]",
            "type" : "string"
        },
        "exampleOutput2": {
            "value": "[variables('var2')]",
            "type" : "array"
        },
        "exampleOutput3": {
            "value": "[variables('var3')]",
            "type" : "string"
        },
        "exampleOutput4": {
            "value": "[variables('var4')]",
            "type" : "object"
        }
    }
}
```

<span data-ttu-id="3214e-178">hello utdata från hello föregående exempel med hello standardvärden är:</span><span class="sxs-lookup"><span data-stu-id="3214e-178">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="3214e-179">Namn</span><span class="sxs-lookup"><span data-stu-id="3214e-179">Name</span></span> | <span data-ttu-id="3214e-180">Typ</span><span class="sxs-lookup"><span data-stu-id="3214e-180">Type</span></span> | <span data-ttu-id="3214e-181">Värde</span><span class="sxs-lookup"><span data-stu-id="3214e-181">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="3214e-182">exampleOutput1</span><span class="sxs-lookup"><span data-stu-id="3214e-182">exampleOutput1</span></span> | <span data-ttu-id="3214e-183">Sträng</span><span class="sxs-lookup"><span data-stu-id="3214e-183">String</span></span> | <span data-ttu-id="3214e-184">myVariable</span><span class="sxs-lookup"><span data-stu-id="3214e-184">myVariable</span></span> |
| <span data-ttu-id="3214e-185">exampleOutput2</span><span class="sxs-lookup"><span data-stu-id="3214e-185">exampleOutput2</span></span> | <span data-ttu-id="3214e-186">Matris</span><span class="sxs-lookup"><span data-stu-id="3214e-186">Array</span></span> | <span data-ttu-id="3214e-187">[1, 2, 3, 4]</span><span class="sxs-lookup"><span data-stu-id="3214e-187">[1, 2, 3, 4]</span></span> |
| <span data-ttu-id="3214e-188">exampleOutput3</span><span class="sxs-lookup"><span data-stu-id="3214e-188">exampleOutput3</span></span> | <span data-ttu-id="3214e-189">Sträng</span><span class="sxs-lookup"><span data-stu-id="3214e-189">String</span></span> | <span data-ttu-id="3214e-190">myVariable</span><span class="sxs-lookup"><span data-stu-id="3214e-190">myVariable</span></span> |
| <span data-ttu-id="3214e-191">exampleOutput4</span><span class="sxs-lookup"><span data-stu-id="3214e-191">exampleOutput4</span></span> |  <span data-ttu-id="3214e-192">Objekt</span><span class="sxs-lookup"><span data-stu-id="3214e-192">Object</span></span> | <span data-ttu-id="3214e-193">{”egenskap1”: ”värde1”, ”egenskap2”: ”value2”}</span><span class="sxs-lookup"><span data-stu-id="3214e-193">{"property1": "value1", "property2": "value2"}</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3214e-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3214e-194">Next steps</span></span>
* <span data-ttu-id="3214e-195">En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="3214e-195">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="3214e-196">flera mallar finns i toomerge [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="3214e-196">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="3214e-197">tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="3214e-197">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="3214e-198">toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="3214e-198">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

