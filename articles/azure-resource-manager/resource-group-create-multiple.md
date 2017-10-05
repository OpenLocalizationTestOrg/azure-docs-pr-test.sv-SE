---
title: Distribuera flera instanser av Azure-resurser | Microsoft Docs
description: "Använd kopieringen och matriser i en Azure Resource Manager-mall för att iterera flera gånger när du distribuerar resurser."
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
ms.openlocfilehash: ed8e3081d2b2e07938d7cf3aa5f95f6dde81bc66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a><span data-ttu-id="21edb-103">Distribuera flera instanser av en resurs eller en egenskap i Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="21edb-103">Deploy multiple instances of a resource or property in Azure Resource Manager templates</span></span>
<span data-ttu-id="21edb-104">Det här avsnittet visar hur du iterera i Azure Resource Manager-mall för att skapa flera instanser av en resurs, eller flera instanser av en egenskap för en resurs.</span><span class="sxs-lookup"><span data-stu-id="21edb-104">This topic shows you how to iterate in your Azure Resource Manager template to create multiple instances of a resource, or multiple instances of a property on a resource.</span></span>

<span data-ttu-id="21edb-105">Om du behöver lägga till logik i mallen som du kan ange om en resurs har distribuerats, se [villkorligt distribuera resurs](#conditionally-deploy-resource).</span><span class="sxs-lookup"><span data-stu-id="21edb-105">If you need to add logic to your template that enables you to specify whether a resource is deployed, see [Conditionally deploy resource](#conditionally-deploy-resource).</span></span>

## <a name="resource-iteration"></a><span data-ttu-id="21edb-106">Resursen upprepning</span><span class="sxs-lookup"><span data-stu-id="21edb-106">Resource iteration</span></span>
<span data-ttu-id="21edb-107">När du skapar flera instanser av en resurstyp, till en `copy` elementet för resurstypen.</span><span class="sxs-lookup"><span data-stu-id="21edb-107">To create multiple instances of a resource type, add a `copy` element to the resource type.</span></span> <span data-ttu-id="21edb-108">I elementet kopia anger du antalet upprepningar och ett namn för den här loop.</span><span class="sxs-lookup"><span data-stu-id="21edb-108">In the copy element, you specify the number of iterations and a name for this loop.</span></span> <span data-ttu-id="21edb-109">Värdet för antal måste vara ett positivt heltal och får inte överskrida 800.</span><span class="sxs-lookup"><span data-stu-id="21edb-109">The count value must be a positive integer and cannot exceed 800.</span></span> <span data-ttu-id="21edb-110">Hanteraren för filserverresurser skapar resurserna parallellt.</span><span class="sxs-lookup"><span data-stu-id="21edb-110">Resource Manager creates the resources in parallel.</span></span> <span data-ttu-id="21edb-111">Den ordning som de har skapats kan därför inte garanteras.</span><span class="sxs-lookup"><span data-stu-id="21edb-111">Therefore, the order in which they are created is not guaranteed.</span></span> <span data-ttu-id="21edb-112">För att skapa hävdade resurser i följd, se [seriella kopiera](#serial-copy).</span><span class="sxs-lookup"><span data-stu-id="21edb-112">To create iterated resources in sequence, see [Serial copy](#serial-copy).</span></span> 

<span data-ttu-id="21edb-113">Resursen ska skapa flera gånger tar följande format:</span><span class="sxs-lookup"><span data-stu-id="21edb-113">The resource to create multiple times takes the following format:</span></span>

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

<span data-ttu-id="21edb-114">Observera att varje resurs ingår i `copyIndex()` som returnerar den aktuella upprepningen i en slinga.</span><span class="sxs-lookup"><span data-stu-id="21edb-114">Notice that the name of each resource includes the `copyIndex()` function, which returns the current iteration in the loop.</span></span> <span data-ttu-id="21edb-115">`copyIndex()`är nollbaserade.</span><span class="sxs-lookup"><span data-stu-id="21edb-115">`copyIndex()` is zero-based.</span></span> <span data-ttu-id="21edb-116">Det, i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="21edb-116">So, the following example:</span></span>

```json
"name": "[concat('storage', copyIndex())]",
```

<span data-ttu-id="21edb-117">Skapar dessa namn:</span><span class="sxs-lookup"><span data-stu-id="21edb-117">Creates these names:</span></span>

* <span data-ttu-id="21edb-118">storage0</span><span class="sxs-lookup"><span data-stu-id="21edb-118">storage0</span></span>
* <span data-ttu-id="21edb-119">storage1</span><span class="sxs-lookup"><span data-stu-id="21edb-119">storage1</span></span>
* <span data-ttu-id="21edb-120">storage2.</span><span class="sxs-lookup"><span data-stu-id="21edb-120">storage2.</span></span>

<span data-ttu-id="21edb-121">Om du vill förskjuta indexvärdet skickar du ett värde i funktionen copyIndex().</span><span class="sxs-lookup"><span data-stu-id="21edb-121">To offset the index value, you can pass a value in the copyIndex() function.</span></span> <span data-ttu-id="21edb-122">Antal iterationer för att utföra fortfarande har angetts i elementet kopia, men värdet för copyIndex förskjutas av det angivna värdet.</span><span class="sxs-lookup"><span data-stu-id="21edb-122">The number of iterations to perform is still specified in the copy element, but the value of copyIndex is offset by the specified value.</span></span> <span data-ttu-id="21edb-123">Det, i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="21edb-123">So, the following example:</span></span>

```json
"name": "[concat('storage', copyIndex(1))]",
```

<span data-ttu-id="21edb-124">Skapar dessa namn:</span><span class="sxs-lookup"><span data-stu-id="21edb-124">Creates these names:</span></span>

* <span data-ttu-id="21edb-125">storage1</span><span class="sxs-lookup"><span data-stu-id="21edb-125">storage1</span></span>
* <span data-ttu-id="21edb-126">storage2</span><span class="sxs-lookup"><span data-stu-id="21edb-126">storage2</span></span>
* <span data-ttu-id="21edb-127">storage3</span><span class="sxs-lookup"><span data-stu-id="21edb-127">storage3</span></span>

<span data-ttu-id="21edb-128">Kopieringen är användbart när du arbetar med matriser eftersom du kan söka igenom varje element i matrisen.</span><span class="sxs-lookup"><span data-stu-id="21edb-128">The copy operation is helpful when working with arrays because you can iterate through each element in the array.</span></span> <span data-ttu-id="21edb-129">Använd den `length` funktion på matrisen för att ange antalet för iterationer, och `copyIndex` att hämta det aktuella indexet i matrisen.</span><span class="sxs-lookup"><span data-stu-id="21edb-129">Use the `length` function on the array to specify the count for iterations, and `copyIndex` to retrieve the current index in the array.</span></span> <span data-ttu-id="21edb-130">Det, i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="21edb-130">So, the following example:</span></span>

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

<span data-ttu-id="21edb-131">Skapar dessa namn:</span><span class="sxs-lookup"><span data-stu-id="21edb-131">Creates these names:</span></span>

* <span data-ttu-id="21edb-132">storagecontoso</span><span class="sxs-lookup"><span data-stu-id="21edb-132">storagecontoso</span></span>
* <span data-ttu-id="21edb-133">storagefabrikam</span><span class="sxs-lookup"><span data-stu-id="21edb-133">storagefabrikam</span></span>
* <span data-ttu-id="21edb-134">storagecoho</span><span class="sxs-lookup"><span data-stu-id="21edb-134">storagecoho</span></span>

## <a name="serial-copy"></a><span data-ttu-id="21edb-135">Seriell kopia</span><span class="sxs-lookup"><span data-stu-id="21edb-135">Serial copy</span></span>

<span data-ttu-id="21edb-136">När du använder elementet kopiera för att skapa flera instanser av en resurstyp, hanteraren för filserverresurser, som standard distribuerar dessa instanser parallellt.</span><span class="sxs-lookup"><span data-stu-id="21edb-136">When you use the copy element to create multiple instances of a resource type, Resource Manager, by default, deploys those instances in parallel.</span></span> <span data-ttu-id="21edb-137">Du kanske vill ange att resurserna som distribueras i följd.</span><span class="sxs-lookup"><span data-stu-id="21edb-137">However, you may want to specify that the resources are deployed in sequence.</span></span> <span data-ttu-id="21edb-138">Till exempel när du uppdaterar en produktionsmiljö måste du kanske vill sprida uppdateringar så bara ett visst antal uppdateras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="21edb-138">For example, when updating a production environment, you may want to stagger the updates so only a certain number are updated at any one time.</span></span>

<span data-ttu-id="21edb-139">Resource Manager tillhandahåller egenskaper i elementet kopia som gör det möjligt att distribuera seriellt flera instanser.</span><span class="sxs-lookup"><span data-stu-id="21edb-139">Resource Manager provides properties on the copy element that enable you to serially deploy multiple instances.</span></span> <span data-ttu-id="21edb-140">I elementet kopiera ange `mode` till **seriella** och `batchSize` med antalet instanser som ska distribueras i taget.</span><span class="sxs-lookup"><span data-stu-id="21edb-140">In the copy element, set `mode` to **serial** and `batchSize` to the number of instances to deploy at a time.</span></span> <span data-ttu-id="21edb-141">Med seriella läge skapar Resource Manager ett beroende på tidigare instanser i en slinga så att den inte startar en batch tills den föregående batchen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="21edb-141">With serial mode, Resource Manager creates a dependency on earlier instances in the loop, so it does not start one batch until the previous batch completes.</span></span>

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

<span data-ttu-id="21edb-142">Egenskapen läget accepterar också **parallella**, vilket är standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="21edb-142">The mode property also accepts **parallel**, which is the default value.</span></span>

<span data-ttu-id="21edb-143">Använd följande mall som distribuerar tom kapslade mallar för att testa seriella kopia utan att skapa verkliga resurser:</span><span class="sxs-lookup"><span data-stu-id="21edb-143">To test serial copy without creating actual resources, use the following template that deploys empty nested templates:</span></span>

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

<span data-ttu-id="21edb-144">Observera att kapslade distributioner bearbetas i följd i distributionshistoriken för.</span><span class="sxs-lookup"><span data-stu-id="21edb-144">In the deployment history, notice that the nested deployments are processed in sequence.</span></span>

![seriell distribution](./media/resource-group-create-multiple/serial-copy.png)

<span data-ttu-id="21edb-146">För en mer realistisk scenario distribuerar i följande exempel två instanser samtidigt för en Linux-VM från en kapslad mall:</span><span class="sxs-lookup"><span data-stu-id="21edb-146">For a more realistic scenario, the following example deploys two instances at a time of a Linux VM from a nested template:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "User name for the Virtual Machine."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for the Virtual Machine."
            }
        },
        "dnsLabelPrefix": {
            "type": "string",
            "metadata": {
                "description": "Unique DNS Name for the Public IP used to access the Virtual Machine."
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
                "description": "The Ubuntu version for the VM. This will pick a fully patched image of this given Ubuntu version."
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

## <a name="property-iteration"></a><span data-ttu-id="21edb-147">Egenskapen upprepning</span><span class="sxs-lookup"><span data-stu-id="21edb-147">Property iteration</span></span>

<span data-ttu-id="21edb-148">Om du vill skapa flera värden för en egenskap för en resurs lägger du till en `copy` matris i elementet egenskaper.</span><span class="sxs-lookup"><span data-stu-id="21edb-148">To create multiple values for a property on a resource, add a `copy` array in the properties element.</span></span> <span data-ttu-id="21edb-149">Denna matris innehåller objekt och alla objekt som har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="21edb-149">This array contains objects, and each object has the following properties:</span></span>

* <span data-ttu-id="21edb-150">namn – namnet på egenskapen för att skapa flera värden för</span><span class="sxs-lookup"><span data-stu-id="21edb-150">name - the name of the property to create multiple values for</span></span>
* <span data-ttu-id="21edb-151">Antal - värden för att skapa</span><span class="sxs-lookup"><span data-stu-id="21edb-151">count - the number of values to create</span></span>
* <span data-ttu-id="21edb-152">indata - ett objekt som innehåller värdena som att tilldela egenskapen</span><span class="sxs-lookup"><span data-stu-id="21edb-152">input - an object that contains the values to assign to the property</span></span>  

<span data-ttu-id="21edb-153">I följande exempel visas hur du använder `copy` dataDisks-egenskapen på en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="21edb-153">The following example shows how to apply `copy` to the dataDisks property on a virtual machine:</span></span>

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

<span data-ttu-id="21edb-154">Observera att när du använder `copyIndex` inuti en egenskapen iteration måste du ange namnet på upprepning.</span><span class="sxs-lookup"><span data-stu-id="21edb-154">Notice that when using `copyIndex` inside a property iteration, you must provide the name of the iteration.</span></span> <span data-ttu-id="21edb-155">Du behöver inte ange namnet när det används med resurs iteration.</span><span class="sxs-lookup"><span data-stu-id="21edb-155">You do not have to provide the name when used with resource iteration.</span></span>

<span data-ttu-id="21edb-156">Hanteraren för filserverresurser expanderar den `copy` matris under distributionen.</span><span class="sxs-lookup"><span data-stu-id="21edb-156">Resource Manager expands the `copy` array during deployment.</span></span> <span data-ttu-id="21edb-157">Namnet på matrisen blir namnet på egenskapen.</span><span class="sxs-lookup"><span data-stu-id="21edb-157">The name of the array becomes the name of the property.</span></span> <span data-ttu-id="21edb-158">Indatavärdena bli objektets egenskaper.</span><span class="sxs-lookup"><span data-stu-id="21edb-158">The input values become the object properties.</span></span> <span data-ttu-id="21edb-159">Den distribuerade mallen blir:</span><span class="sxs-lookup"><span data-stu-id="21edb-159">The deployed template becomes:</span></span>

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

<span data-ttu-id="21edb-160">Du kan använda resursen och egenskapen iteration tillsammans.</span><span class="sxs-lookup"><span data-stu-id="21edb-160">You can use resource and property iteration together.</span></span> <span data-ttu-id="21edb-161">Referens egenskapen iteration efter namn.</span><span class="sxs-lookup"><span data-stu-id="21edb-161">Reference the property iteration by name.</span></span>

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

<span data-ttu-id="21edb-162">Du får bara innehålla ett kopiera element i egenskaperna för varje resurs.</span><span class="sxs-lookup"><span data-stu-id="21edb-162">You can only include one copy element in the properties for each resource.</span></span> <span data-ttu-id="21edb-163">Definiera flera objekt i matrisen kopia om du vill ange en iteration loop för mer än en egenskap.</span><span class="sxs-lookup"><span data-stu-id="21edb-163">To specify an iteration loop for more than one property, define multiple objects in the copy array.</span></span> <span data-ttu-id="21edb-164">Varje objekt är hävdade separat.</span><span class="sxs-lookup"><span data-stu-id="21edb-164">Each object is iterated separately.</span></span> <span data-ttu-id="21edb-165">Till exempel för att skapa flera instanser av både den `frontendIPConfigurations` egenskapen och `loadBalancingRules` egenskapen för en belastningsutjämnare definiera både objekt i en enda kopia-elementet:</span><span class="sxs-lookup"><span data-stu-id="21edb-165">For example, to create multiple instances of both the `frontendIPConfigurations` property and the `loadBalancingRules` property on a load balancer, define both objects in a single copy element:</span></span> 

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

## <a name="depend-on-resources-in-a-loop"></a><span data-ttu-id="21edb-166">Beroende av resurser i en loop</span><span class="sxs-lookup"><span data-stu-id="21edb-166">Depend on resources in a loop</span></span>
<span data-ttu-id="21edb-167">Du anger att en resurs distribueras efter en annan resurs med hjälp av den `dependsOn` element.</span><span class="sxs-lookup"><span data-stu-id="21edb-167">You specify that a resource is deployed after another resource by using the `dependsOn` element.</span></span> <span data-ttu-id="21edb-168">Ange namnet på kopian loop i dependsOn-elementet för att distribuera en resurs som är beroende av mängd resurser i en slinga.</span><span class="sxs-lookup"><span data-stu-id="21edb-168">To deploy a resource that depends on the collection of resources in a loop, provide the name of the copy loop in the dependsOn element.</span></span> <span data-ttu-id="21edb-169">I följande exempel visas hur du distribuerar tre storage-konton innan du distribuerar den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="21edb-169">The following example shows how to deploy three storage accounts before deploying the Virtual Machine.</span></span> <span data-ttu-id="21edb-170">Fullständig definitionen för virtuell dator visas inte.</span><span class="sxs-lookup"><span data-stu-id="21edb-170">The full Virtual Machine definition is not shown.</span></span> <span data-ttu-id="21edb-171">Observera att kopiera elementet har name angivet till `storagecopy` och dependsOn-elementet för virtuella datorer har också `storagecopy`.</span><span class="sxs-lookup"><span data-stu-id="21edb-171">Notice that the copy element has name set to `storagecopy` and the dependsOn element for the Virtual Machines is also set to `storagecopy`.</span></span>

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

## <a name="create-multiple-instances-of-a-child-resource"></a><span data-ttu-id="21edb-172">Skapa flera instanser av en underordnad-resurs</span><span class="sxs-lookup"><span data-stu-id="21edb-172">Create multiple instances of a child resource</span></span>
<span data-ttu-id="21edb-173">Du kan inte använda en kopia skapas för en underordnad resurs.</span><span class="sxs-lookup"><span data-stu-id="21edb-173">You cannot use a copy loop for a child resource.</span></span> <span data-ttu-id="21edb-174">Du måste i stället skapa resursen som översta resurs för att skapa flera instanser av en resurs som du vanligtvis definiera som kapslad i en annan resurs.</span><span class="sxs-lookup"><span data-stu-id="21edb-174">To create multiple instances of a resource that you typically define as nested within another resource, you must instead create that resource as a top-level resource.</span></span> <span data-ttu-id="21edb-175">Du definierar relationen med den överordnade resursen via egenskaperna typ och namn.</span><span class="sxs-lookup"><span data-stu-id="21edb-175">You define the relationship with the parent resource through the type and name properties.</span></span>

<span data-ttu-id="21edb-176">Anta exempelvis att du definierar en datamängd vanligtvis som en underordnad resurs i en datafabrik.</span><span class="sxs-lookup"><span data-stu-id="21edb-176">For example, suppose you typically define a dataset as a child resource within a data factory.</span></span>

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

<span data-ttu-id="21edb-177">Om du vill skapa flera instanser av datauppsättningar flyttas utanför datafabriken.</span><span class="sxs-lookup"><span data-stu-id="21edb-177">To create multiple instances of data sets, move it outside of the data factory.</span></span> <span data-ttu-id="21edb-178">Dataset måste vara på samma nivå som data factory, men det är fortfarande en underordnad resurs i data factory.</span><span class="sxs-lookup"><span data-stu-id="21edb-178">The dataset must be at the same level as the data factory, but it is still a child resource of the data factory.</span></span> <span data-ttu-id="21edb-179">Du bevara relationen mellan datauppsättning och datafabriken via egenskaperna typ och namn.</span><span class="sxs-lookup"><span data-stu-id="21edb-179">You preserve the relationship between data set and data factory through the type and name properties.</span></span> <span data-ttu-id="21edb-180">Eftersom typen inte kan härledas från dess position i mallen, måste du ange typen av fullständigt kvalificerat i formatet: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.</span><span class="sxs-lookup"><span data-stu-id="21edb-180">Since type can no longer be inferred from its position in the template, you must provide the fully qualified type in the format: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.</span></span>

<span data-ttu-id="21edb-181">Ange ett namn för den datamängd som innehåller namnet på överordnade resurs för att upprätta en överordnad-underordnad relation med en instans av datafabriken.</span><span class="sxs-lookup"><span data-stu-id="21edb-181">To establish a parent/child relationship with an instance of the data factory, provide a name for the data set that includes the parent resource name.</span></span> <span data-ttu-id="21edb-182">Använd formatet: `{parent-resource-name}/{child-resource-name}`.</span><span class="sxs-lookup"><span data-stu-id="21edb-182">Use the format: `{parent-resource-name}/{child-resource-name}`.</span></span>  

<span data-ttu-id="21edb-183">I följande exempel visas implementeringen:</span><span class="sxs-lookup"><span data-stu-id="21edb-183">The following example shows the implementation:</span></span>

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

## <a name="conditionally-deploy-resource"></a><span data-ttu-id="21edb-184">Villkorligt distribuera resurs</span><span class="sxs-lookup"><span data-stu-id="21edb-184">Conditionally deploy resource</span></span>

<span data-ttu-id="21edb-185">Anger om en resurs har distribuerats på `condition` element.</span><span class="sxs-lookup"><span data-stu-id="21edb-185">To specify whether a resource is deployed, use the `condition` element.</span></span> <span data-ttu-id="21edb-186">Värdet för det här elementet matchar true eller false.</span><span class="sxs-lookup"><span data-stu-id="21edb-186">The value for this element resolves to true or false.</span></span> <span data-ttu-id="21edb-187">När värdet är true, distribueras resursen.</span><span class="sxs-lookup"><span data-stu-id="21edb-187">When the value is true, the resource is deployed.</span></span> <span data-ttu-id="21edb-188">Om värdet är FALSKT har resursen inte distribuerats.</span><span class="sxs-lookup"><span data-stu-id="21edb-188">When the value is false, the resource is not deployed.</span></span> <span data-ttu-id="21edb-189">Till exempel vill ange om ett nytt lagringskonto distribueras eller ett befintligt lagringskonto används, använder du:</span><span class="sxs-lookup"><span data-stu-id="21edb-189">For example, to specify whether a new storage account is deployed or an existing storage account is used, use:</span></span>

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

<span data-ttu-id="21edb-190">Ett exempel på hur du använder en ny eller befintlig resurs finns [ny eller befintlig mall för villkoret](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span><span class="sxs-lookup"><span data-stu-id="21edb-190">For an example of using a new or existing resource, see [New or existing condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span></span>

<span data-ttu-id="21edb-191">Ett exempel på med ett lösenord eller SSH-nyckel för att distribuera den virtuella datorn, se [användarnamn eller SSH villkoret mallen](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span><span class="sxs-lookup"><span data-stu-id="21edb-191">For an example of using a password or SSH key to deploy virtual machine, see [Username or SSH condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="21edb-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="21edb-192">Next steps</span></span>
* <span data-ttu-id="21edb-193">Om du vill veta om avsnitt i en mall finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="21edb-193">If you want to learn about the sections of a template, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="21edb-194">Information om hur du distribuerar mallen finns [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="21edb-194">To learn how to deploy your template, see [Deploy an application with Azure Resource Manager Template](resource-group-template-deploy.md).</span></span>

