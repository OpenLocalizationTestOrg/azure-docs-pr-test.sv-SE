---
title: aaaDeploy flera instanser av Azure-resurser | Microsoft Docs
description: "Använda kopieringen och matriser i tooiterate en Azure Resource Manager-mall flera gånger när du distribuerar resurser."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 94d95810-a87b-460f-8e82-c69d462ac3ca
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2017
ms.author: tomfitz
ms.openlocfilehash: a3bd42f694053317c30b639c33dc4efae41a9a9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a><span data-ttu-id="41f51-103">Distribuera flera instanser av en resurs eller en egenskap i Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="41f51-103">Deploy multiple instances of a resource or property in Azure Resource Manager templates</span></span>
<span data-ttu-id="41f51-104">Det här avsnittet beskrivs hur du tooiterate i din Azure Resource Manager-mall toocreate flera instanser av en resurs eller flera instanser av en egenskap för en resurs.</span><span class="sxs-lookup"><span data-stu-id="41f51-104">This topic shows you how tooiterate in your Azure Resource Manager template toocreate multiple instances of a resource, or multiple instances of a property on a resource.</span></span>

<span data-ttu-id="41f51-105">Om du behöver tooadd logik tooyour mall som du kan använda toospecify om en resurs har distribuerats, se [villkorligt distribuera resurs](#conditionally-deploy-resource).</span><span class="sxs-lookup"><span data-stu-id="41f51-105">If you need tooadd logic tooyour template that enables you toospecify whether a resource is deployed, see [Conditionally deploy resource](#conditionally-deploy-resource).</span></span>

## <a name="resource-iteration"></a><span data-ttu-id="41f51-106">Resursen upprepning</span><span class="sxs-lookup"><span data-stu-id="41f51-106">Resource iteration</span></span>
<span data-ttu-id="41f51-107">toocreate flera instanser av en resurstyp, lägga till en `copy` elementtypen toohello resurs.</span><span class="sxs-lookup"><span data-stu-id="41f51-107">toocreate multiple instances of a resource type, add a `copy` element toohello resource type.</span></span> <span data-ttu-id="41f51-108">Ange hello antalet upprepningar och ett namn för den här loop i hello kopiera element.</span><span class="sxs-lookup"><span data-stu-id="41f51-108">In hello copy element, you specify hello number of iterations and a name for this loop.</span></span> <span data-ttu-id="41f51-109">hello count-värdet måste vara ett positivt heltal och får inte överskrida 800.</span><span class="sxs-lookup"><span data-stu-id="41f51-109">hello count value must be a positive integer and cannot exceed 800.</span></span> <span data-ttu-id="41f51-110">Hanteraren för filserverresurser skapar hello resurser parallellt.</span><span class="sxs-lookup"><span data-stu-id="41f51-110">Resource Manager creates hello resources in parallel.</span></span> <span data-ttu-id="41f51-111">Därför kan inte garanteras hello ordning som de skapades.</span><span class="sxs-lookup"><span data-stu-id="41f51-111">Therefore, hello order in which they are created is not guaranteed.</span></span> <span data-ttu-id="41f51-112">toocreate hävdade resurser i följd, se [seriella kopiera](#serial-copy).</span><span class="sxs-lookup"><span data-stu-id="41f51-112">toocreate iterated resources in sequence, see [Serial copy](#serial-copy).</span></span> 

<span data-ttu-id="41f51-113">hello resurs toocreate tar flera gånger hello följande format:</span><span class="sxs-lookup"><span data-stu-id="41f51-113">hello resource toocreate multiple times takes hello following format:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        }
    ],
    "outputs": {}
}
```

<span data-ttu-id="41f51-114">Observera att hello varje resurs ingår hello `copyIndex()` som returnerar hello aktuella iteration i hello loop.</span><span class="sxs-lookup"><span data-stu-id="41f51-114">Notice that hello name of each resource includes hello `copyIndex()` function, which returns hello current iteration in hello loop.</span></span> <span data-ttu-id="41f51-115">`copyIndex()`är nollbaserade.</span><span class="sxs-lookup"><span data-stu-id="41f51-115">`copyIndex()` is zero-based.</span></span> <span data-ttu-id="41f51-116">I så fall hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="41f51-116">So, hello following example:</span></span>

```json
"name": "[concat('storage', copyIndex())]",
```

<span data-ttu-id="41f51-117">Skapar dessa namn:</span><span class="sxs-lookup"><span data-stu-id="41f51-117">Creates these names:</span></span>

* <span data-ttu-id="41f51-118">storage0</span><span class="sxs-lookup"><span data-stu-id="41f51-118">storage0</span></span>
* <span data-ttu-id="41f51-119">storage1</span><span class="sxs-lookup"><span data-stu-id="41f51-119">storage1</span></span>
* <span data-ttu-id="41f51-120">storage2.</span><span class="sxs-lookup"><span data-stu-id="41f51-120">storage2.</span></span>

<span data-ttu-id="41f51-121">indexvärdet för toooffset hello du skickar ett värde i hello copyIndex() funktion.</span><span class="sxs-lookup"><span data-stu-id="41f51-121">toooffset hello index value, you can pass a value in hello copyIndex() function.</span></span> <span data-ttu-id="41f51-122">hello antal upprepningar tooperform fortfarande anges i hello kopiera element, men hello värdet för copyIndex förskjutas av hello anges värdet.</span><span class="sxs-lookup"><span data-stu-id="41f51-122">hello number of iterations tooperform is still specified in hello copy element, but hello value of copyIndex is offset by hello specified value.</span></span> <span data-ttu-id="41f51-123">I så fall hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="41f51-123">So, hello following example:</span></span>

```json
"name": "[concat('storage', copyIndex(1))]",
```

<span data-ttu-id="41f51-124">Skapar dessa namn:</span><span class="sxs-lookup"><span data-stu-id="41f51-124">Creates these names:</span></span>

* <span data-ttu-id="41f51-125">storage1</span><span class="sxs-lookup"><span data-stu-id="41f51-125">storage1</span></span>
* <span data-ttu-id="41f51-126">storage2</span><span class="sxs-lookup"><span data-stu-id="41f51-126">storage2</span></span>
* <span data-ttu-id="41f51-127">storage3</span><span class="sxs-lookup"><span data-stu-id="41f51-127">storage3</span></span>

<span data-ttu-id="41f51-128">hello kopieringsåtgärden är användbart när du arbetar med matriser eftersom du kan söka igenom varje element i matrisen hello.</span><span class="sxs-lookup"><span data-stu-id="41f51-128">hello copy operation is helpful when working with arrays because you can iterate through each element in hello array.</span></span> <span data-ttu-id="41f51-129">Använd hello `length` fungerar på hello matris toospecify hello-antal för iterationer, och `copyIndex` tooretrieve hello aktuellt index i hello matris.</span><span class="sxs-lookup"><span data-stu-id="41f51-129">Use hello `length` function on hello array toospecify hello count for iterations, and `copyIndex` tooretrieve hello current index in hello array.</span></span> <span data-ttu-id="41f51-130">I så fall hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="41f51-130">So, hello following example:</span></span>

```json
"parameters": { 
  "org": { 
     "type": "array", 
     "defaultValue": [ 
         "contoso", 
         "fabrikam", 
         "coho" 
      ] 
  }
}, 
"resources": [ 
  { 
      "name": "[concat('storage', parameters('org')[copyIndex()])]", 
      "copy": { 
         "name": "storagecopy", 
         "count": "[length(parameters('org'))]" 
      }, 
      ...
  } 
]
```

<span data-ttu-id="41f51-131">Skapar dessa namn:</span><span class="sxs-lookup"><span data-stu-id="41f51-131">Creates these names:</span></span>

* <span data-ttu-id="41f51-132">storagecontoso</span><span class="sxs-lookup"><span data-stu-id="41f51-132">storagecontoso</span></span>
* <span data-ttu-id="41f51-133">storagefabrikam</span><span class="sxs-lookup"><span data-stu-id="41f51-133">storagefabrikam</span></span>
* <span data-ttu-id="41f51-134">storagecoho</span><span class="sxs-lookup"><span data-stu-id="41f51-134">storagecoho</span></span>

## <a name="serial-copy"></a><span data-ttu-id="41f51-135">Seriell kopia</span><span class="sxs-lookup"><span data-stu-id="41f51-135">Serial copy</span></span>

<span data-ttu-id="41f51-136">När du använder hello kopiera elementet toocreate flera instanser av en resurstyp, hanteraren för filserverresurser, som standard distribuerar dessa instanser parallellt.</span><span class="sxs-lookup"><span data-stu-id="41f51-136">When you use hello copy element toocreate multiple instances of a resource type, Resource Manager, by default, deploys those instances in parallel.</span></span> <span data-ttu-id="41f51-137">Men kanske du toospecify som hello resurser distribueras i följd.</span><span class="sxs-lookup"><span data-stu-id="41f51-137">However, you may want toospecify that hello resources are deployed in sequence.</span></span> <span data-ttu-id="41f51-138">När du uppdaterar en produktionsmiljö, du kan exempelvis toostagger hello uppdateras så att bara ett visst antal uppdateras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="41f51-138">For example, when updating a production environment, you may want toostagger hello updates so only a certain number are updated at any one time.</span></span>

<span data-ttu-id="41f51-139">Resource Manager innehåller egenskaper för hello kopiera element som aktiverar du tooserially distribuera flera instanser.</span><span class="sxs-lookup"><span data-stu-id="41f51-139">Resource Manager provides properties on hello copy element that enable you tooserially deploy multiple instances.</span></span> <span data-ttu-id="41f51-140">Hello kopiera element, ange `mode` för**seriella** och `batchSize` toohello antal instanser toodeploy i taget.</span><span class="sxs-lookup"><span data-stu-id="41f51-140">In hello copy element, set `mode` too**serial** and `batchSize` toohello number of instances toodeploy at a time.</span></span> <span data-ttu-id="41f51-141">Med seriella läge skapar Resource Manager ett beroende på tidigare instanser i hello loop så att den inte startar en batch tills hello föregående batchen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="41f51-141">With serial mode, Resource Manager creates a dependency on earlier instances in hello loop, so it does not start one batch until hello previous batch completes.</span></span>

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

<span data-ttu-id="41f51-142">hello egenskap accepterar också **parallella**, vilket är standardvärdet för hello.</span><span class="sxs-lookup"><span data-stu-id="41f51-142">hello mode property also accepts **parallel**, which is hello default value.</span></span>

<span data-ttu-id="41f51-143">tootest seriella kopia utan att skapa verkliga resurser används hello följande mall som distribuerar tom kapslade mallar:</span><span class="sxs-lookup"><span data-stu-id="41f51-143">tootest serial copy without creating actual resources, use hello following template that deploys empty nested templates:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberToDeploy": {
      "type": "int",
      "minValue": 2,
      "defaultValue": 5
    }
  },
  "resources": [
    {
      "apiVersion": "2015-01-01",
      "type": "Microsoft.Resources/deployments",
      "name": "[concat('loop-', copyIndex())]",
      "copy": {
        "name": "iterator",
        "count": "[parameters('numberToDeploy')]",
        "mode": "serial",
        "batchSize": 1
      },
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [],
          "outputs": {
          }
        }
      }
    }
  ],
  "outputs": {
  }
}
```

<span data-ttu-id="41f51-144">Observera att hello kapslade distributioner bearbetas hello distributionshistoriken i följd.</span><span class="sxs-lookup"><span data-stu-id="41f51-144">In hello deployment history, notice that hello nested deployments are processed in sequence.</span></span>

![seriell distribution](./media/resource-group-create-multiple/serial-copy.png)

<span data-ttu-id="41f51-146">För en mer realistisk scenariot distribuerar hello följande exempel två instanser samtidigt för en Linux-VM från en kapslad mall:</span><span class="sxs-lookup"><span data-stu-id="41f51-146">For a more realistic scenario, hello following example deploys two instances at a time of a Linux VM from a nested template:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "User name for hello Virtual Machine."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for hello Virtual Machine."
            }
        },
        "dnsLabelPrefix": {
            "type": "string",
            "metadata": {
                "description": "Unique DNS Name for hello Public IP used tooaccess hello Virtual Machine."
            }
        },
        "ubuntuOSVersion": {
            "type": "string",
            "defaultValue": "16.04.0-LTS",
            "allowedValues": [
                "12.04.5-LTS",
                "14.04.5-LTS",
                "15.10",
                "16.04.0-LTS"
            ],
            "metadata": {
                "description": "hello Ubuntu version for hello VM. This will pick a fully patched image of this given Ubuntu version."
            }
        }
    },
    "variables": {
        "templatelink": "https://raw.githubusercontent.com/rjmax/Build2017/master/Act1.TemplateEnhancements/Chapter03.LinuxVM.json"
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "[concat('nestedDeployment',copyIndex())]",
            "type": "Microsoft.Resources/deployments",
            "copy": {
                "name": "myCopySet",
                "count": 4,
                "mode": "serial",
                "batchSize": 2
            },
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "adminUsername": {
                        "value": "[parameters('adminUsername')]"
                    },
                    "adminPassword": {
                        "value": "[parameters('adminPassword')]"
                    },
                    "dnsLabelPrefix": {
                        "value": "[parameters('dnsLabelPrefix')]"
                    },
                    "ubuntuOSVersion": {
                        "value": "[parameters('ubuntuOSVersion')]"
                    },
                    "index":{
                        "value": "[copyIndex()]"
                    }
                }
            }
        }
    ]
}
```

## <a name="property-iteration"></a><span data-ttu-id="41f51-147">Egenskapen upprepning</span><span class="sxs-lookup"><span data-stu-id="41f51-147">Property iteration</span></span>

<span data-ttu-id="41f51-148">toocreate flera värden för en egenskap för en resurs lägger du till en `copy` matris i hello egenskaper för elementet.</span><span class="sxs-lookup"><span data-stu-id="41f51-148">toocreate multiple values for a property on a resource, add a `copy` array in hello properties element.</span></span> <span data-ttu-id="41f51-149">Denna matris innehåller objekt och alla objekt har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="41f51-149">This array contains objects, and each object has hello following properties:</span></span>

* <span data-ttu-id="41f51-150">namn - hello av hello egenskapen toocreate flera värden för</span><span class="sxs-lookup"><span data-stu-id="41f51-150">name - hello name of hello property toocreate multiple values for</span></span>
* <span data-ttu-id="41f51-151">Antal - hello värden toocreate</span><span class="sxs-lookup"><span data-stu-id="41f51-151">count - hello number of values toocreate</span></span>
* <span data-ttu-id="41f51-152">indata - ett objekt som innehåller hello värden tooassign toohello egenskap</span><span class="sxs-lookup"><span data-stu-id="41f51-152">input - an object that contains hello values tooassign toohello property</span></span>  

<span data-ttu-id="41f51-153">följande exempel visar hur hello tooapply `copy` toohello dataDisks egenskapen på en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="41f51-153">hello following example shows how tooapply `copy` toohello dataDisks property on a virtual machine:</span></span>

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "copy": [{
          "name": "dataDisks",
          "count": 3,
          "input": {
              "lun": "[copyIndex('dataDisks')]",
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

<span data-ttu-id="41f51-154">Observera att när du använder `copyIndex` inuti en egenskapen iteration måste du ange hello namnet på hello iteration.</span><span class="sxs-lookup"><span data-stu-id="41f51-154">Notice that when using `copyIndex` inside a property iteration, you must provide hello name of hello iteration.</span></span> <span data-ttu-id="41f51-155">Du har inte tooprovide hello namn när det används med resurs iteration.</span><span class="sxs-lookup"><span data-stu-id="41f51-155">You do not have tooprovide hello name when used with resource iteration.</span></span>

<span data-ttu-id="41f51-156">Resource Manager utökas hello `copy` matris under distributionen.</span><span class="sxs-lookup"><span data-stu-id="41f51-156">Resource Manager expands hello `copy` array during deployment.</span></span> <span data-ttu-id="41f51-157">hello namn för hello matrisen blir hello namnet på hello-egenskap.</span><span class="sxs-lookup"><span data-stu-id="41f51-157">hello name of hello array becomes hello name of hello property.</span></span> <span data-ttu-id="41f51-158">hello indatavärden bli hello objektets egenskaper.</span><span class="sxs-lookup"><span data-stu-id="41f51-158">hello input values become hello object properties.</span></span> <span data-ttu-id="41f51-159">Det blir hello distribueras mallen:</span><span class="sxs-lookup"><span data-stu-id="41f51-159">hello deployed template becomes:</span></span>

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
          {
              "lun": 0,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 1,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 2,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

<span data-ttu-id="41f51-160">Du kan använda resursen och egenskapen iteration tillsammans.</span><span class="sxs-lookup"><span data-stu-id="41f51-160">You can use resource and property iteration together.</span></span> <span data-ttu-id="41f51-161">Referens hello egenskapen iteration efter namn.</span><span class="sxs-lookup"><span data-stu-id="41f51-161">Reference hello property iteration by name.</span></span>

```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[concat(parameters('vnetname'), copyIndex())]",
    "apiVersion": "2016-06-01",
    "copy":{
        "count": 2,
        "name": "vnetloop"
    },
    "location": "[resourceGroup().location]",
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "copy": [
            {
                "name": "subnets",
                "count": 2,
                "input": {
                    "name": "[concat('subnet-', copyIndex('subnets'))]",
                    "properties": {
                        "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
                    }
                }
            }
        ]
    }
}
```

<span data-ttu-id="41f51-162">Du får bara innehålla ett kopiera element i hello egenskaper för varje resurs.</span><span class="sxs-lookup"><span data-stu-id="41f51-162">You can only include one copy element in hello properties for each resource.</span></span> <span data-ttu-id="41f51-163">toospecify en iteration loop för mer än en egenskap, definiera flera objekt i hello Kopiera matris.</span><span class="sxs-lookup"><span data-stu-id="41f51-163">toospecify an iteration loop for more than one property, define multiple objects in hello copy array.</span></span> <span data-ttu-id="41f51-164">Varje objekt är hävdade separat.</span><span class="sxs-lookup"><span data-stu-id="41f51-164">Each object is iterated separately.</span></span> <span data-ttu-id="41f51-165">Till exempel toocreate flera instanser av båda hello `frontendIPConfigurations` egenskap och hello `loadBalancingRules` egenskapen för en belastningsutjämnare definiera både objekt i en enda kopia-elementet:</span><span class="sxs-lookup"><span data-stu-id="41f51-165">For example, toocreate multiple instances of both hello `frontendIPConfigurations` property and hello `loadBalancingRules` property on a load balancer, define both objects in a single copy element:</span></span> 

```json
{
    "name": "[variables('loadBalancerName')]",
    "type": "Microsoft.Network/loadBalancers",
    "properties": {
        "copy": [
          {
              "name": "frontendIPConfigurations",
              "count": 2,
              "input": {
                  "name": "[concat('loadBalancerFrontEnd', copyIndex('frontendIPConfigurations', 1))]",
                  "properties": {
                      "publicIPAddress": {
                          "id": "[variables(concat('publicIPAddressID', copyIndex('frontendIPConfigurations', 1)))]"
                      }
                  }
              }
          },
          {
              "name": "loadBalancingRules",
              "count": 2,
              "input": {
                  "name": "[concat('LBRuleForVIP', copyIndex('loadBalancingRules', 1))]",
                  "properties": {
                      "frontendIPConfiguration": {
                          "id": "[variables(concat('frontEndIPConfigID', copyIndex('loadBalancingRules', 1)))]"
                      },
                      "backendAddressPool": {
                          "id": "[variables('lbBackendPoolID')]"
                      },
                      "protocol": "tcp",
                      "frontendPort": "[variables(concat('frontEndPort' copyIndex('loadBalancingRules', 1))]",
                      "backendPort": "[variables(concat('backEndPort' copyIndex('loadBalancingRules', 1))]",
                      "probe": {
                          "id": "[variables('lbProbeID')]"
                      }
                  }
              }
          }
        ],
        ...
    }
}
```

## <a name="depend-on-resources-in-a-loop"></a><span data-ttu-id="41f51-166">Beroende av resurser i en loop</span><span class="sxs-lookup"><span data-stu-id="41f51-166">Depend on resources in a loop</span></span>
<span data-ttu-id="41f51-167">Du anger att en resurs distribueras efter en annan resurs med hjälp av hello `dependsOn` element.</span><span class="sxs-lookup"><span data-stu-id="41f51-167">You specify that a resource is deployed after another resource by using hello `dependsOn` element.</span></span> <span data-ttu-id="41f51-168">toodeploy en resurs som är beroende av hello mängd resurser i en slinga ange hello namnet för hello kopiera loop i hello dependsOn-element.</span><span class="sxs-lookup"><span data-stu-id="41f51-168">toodeploy a resource that depends on hello collection of resources in a loop, provide hello name of hello copy loop in hello dependsOn element.</span></span> <span data-ttu-id="41f51-169">hello som följande exempel visar hur toodeploy tre storage-konton innan du distribuerar hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="41f51-169">hello following example shows how toodeploy three storage accounts before deploying hello Virtual Machine.</span></span> <span data-ttu-id="41f51-170">hello fullständig definition av virtuell dator visas inte.</span><span class="sxs-lookup"><span data-stu-id="41f51-170">hello full Virtual Machine definition is not shown.</span></span> <span data-ttu-id="41f51-171">Observera att kopiera hello-element har namn som angetts för`storagecopy` och hello dependsOn-element för hello virtuella datorer har också angetts för`storagecopy`.</span><span class="sxs-lookup"><span data-stu-id="41f51-171">Notice that hello copy element has name set too`storagecopy` and hello dependsOn element for hello Virtual Machines is also set too`storagecopy`.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        },
        {
            "apiVersion": "2015-06-15", 
            "type": "Microsoft.Compute/virtualMachines", 
            "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
            "dependsOn": ["storagecopy"],
            ...
        }
    ],
    "outputs": {}
}
```

## <a name="create-multiple-instances-of-a-child-resource"></a><span data-ttu-id="41f51-172">Skapa flera instanser av en underordnad-resurs</span><span class="sxs-lookup"><span data-stu-id="41f51-172">Create multiple instances of a child resource</span></span>
<span data-ttu-id="41f51-173">Du kan inte använda en kopia skapas för en underordnad resurs.</span><span class="sxs-lookup"><span data-stu-id="41f51-173">You cannot use a copy loop for a child resource.</span></span> <span data-ttu-id="41f51-174">toocreate flera instanser av en resurs som du vanligtvis definiera som kapslas i en annan resurs, måste du i stället skapa resursen som översta resurs.</span><span class="sxs-lookup"><span data-stu-id="41f51-174">toocreate multiple instances of a resource that you typically define as nested within another resource, you must instead create that resource as a top-level resource.</span></span> <span data-ttu-id="41f51-175">Du definierar hello relation med hello överordnade resursen via egenskaper för hello typ och namn.</span><span class="sxs-lookup"><span data-stu-id="41f51-175">You define hello relationship with hello parent resource through hello type and name properties.</span></span>

<span data-ttu-id="41f51-176">Anta exempelvis att du definierar en datamängd vanligtvis som en underordnad resurs i en datafabrik.</span><span class="sxs-lookup"><span data-stu-id="41f51-176">For example, suppose you typically define a dataset as a child resource within a data factory.</span></span>

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
    "resources": [
    {
        "type": "datasets",
        "name": "exampleDataSet",
        "dependsOn": [
            "exampleDataFactory"
        ],
        ...
    }
}]
```

<span data-ttu-id="41f51-177">toocreate flera instanser av datauppsättningar, flyttas utanför hello data factory.</span><span class="sxs-lookup"><span data-stu-id="41f51-177">toocreate multiple instances of data sets, move it outside of hello data factory.</span></span> <span data-ttu-id="41f51-178">hello dataset måste finnas på samma nivå som hello data factory hello, men det är fortfarande en underordnad resurs i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="41f51-178">hello dataset must be at hello same level as hello data factory, but it is still a child resource of hello data factory.</span></span> <span data-ttu-id="41f51-179">Du bevara hello förhållandet mellan datauppsättning och datafabriken via egenskaper för hello typ och namn.</span><span class="sxs-lookup"><span data-stu-id="41f51-179">You preserve hello relationship between data set and data factory through hello type and name properties.</span></span> <span data-ttu-id="41f51-180">Eftersom typen inte kan härledas från dess placering i hello mall, måste du ange hello fullständigt kvalificerade typen hello format: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.</span><span class="sxs-lookup"><span data-stu-id="41f51-180">Since type can no longer be inferred from its position in hello template, you must provide hello fully qualified type in hello format: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.</span></span>

<span data-ttu-id="41f51-181">tooestablish en överordnad-underordnad relation med en instans av hello data factory, ange ett namn för hello datauppsättning som innehåller hello överordnade resursnamnet.</span><span class="sxs-lookup"><span data-stu-id="41f51-181">tooestablish a parent/child relationship with an instance of hello data factory, provide a name for hello data set that includes hello parent resource name.</span></span> <span data-ttu-id="41f51-182">Använd hello format: `{parent-resource-name}/{child-resource-name}`.</span><span class="sxs-lookup"><span data-stu-id="41f51-182">Use hello format: `{parent-resource-name}/{child-resource-name}`.</span></span>  

<span data-ttu-id="41f51-183">hello visar följande exempel hello implementering:</span><span class="sxs-lookup"><span data-stu-id="41f51-183">hello following example shows hello implementation:</span></span>

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
},
{
    "type": "Microsoft.DataFactory/datafactories/datasets",
    "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
    "dependsOn": [
        "exampleDataFactory"
    ],
    "copy": { 
        "name": "datasetcopy", 
        "count": "3" 
    } 
    ...
}]
```

## <a name="conditionally-deploy-resource"></a><span data-ttu-id="41f51-184">Villkorligt distribuera resurs</span><span class="sxs-lookup"><span data-stu-id="41f51-184">Conditionally deploy resource</span></span>

<span data-ttu-id="41f51-185">toospecify om en resurs har distribuerats, använda hello `condition` element.</span><span class="sxs-lookup"><span data-stu-id="41f51-185">toospecify whether a resource is deployed, use hello `condition` element.</span></span> <span data-ttu-id="41f51-186">hello-värdet för det här elementet löser tootrue eller false.</span><span class="sxs-lookup"><span data-stu-id="41f51-186">hello value for this element resolves tootrue or false.</span></span> <span data-ttu-id="41f51-187">När hello-värdet är true, distribueras hello resurs.</span><span class="sxs-lookup"><span data-stu-id="41f51-187">When hello value is true, hello resource is deployed.</span></span> <span data-ttu-id="41f51-188">När hello-värdet är false har hello resursen inte distribuerats.</span><span class="sxs-lookup"><span data-stu-id="41f51-188">When hello value is false, hello resource is not deployed.</span></span> <span data-ttu-id="41f51-189">Till exempel toospecify om ett nytt lagringskonto distribueras eller ett befintligt lagringskonto används, Använd:</span><span class="sxs-lookup"><span data-stu-id="41f51-189">For example, toospecify whether a new storage account is deployed or an existing storage account is used, use:</span></span>

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

<span data-ttu-id="41f51-190">Ett exempel på hur du använder en ny eller befintlig resurs finns [ny eller befintlig mall för villkoret](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span><span class="sxs-lookup"><span data-stu-id="41f51-190">For an example of using a new or existing resource, see [New or existing condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span></span>

<span data-ttu-id="41f51-191">Ett exempel på med ett lösenord eller SSH-nyckel toodeploy virtuella finns [användarnamn eller SSH villkoret mallen](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span><span class="sxs-lookup"><span data-stu-id="41f51-191">For an example of using a password or SSH key toodeploy virtual machine, see [Username or SSH condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="41f51-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="41f51-192">Next steps</span></span>
* <span data-ttu-id="41f51-193">Om du vill toolearn om hello avsnitt i en mall finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="41f51-193">If you want toolearn about hello sections of a template, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="41f51-194">toolearn hur toodeploy din mall finns [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="41f51-194">toolearn how toodeploy your template, see [Deploy an application with Azure Resource Manager Template](resource-group-template-deploy.md).</span></span>

