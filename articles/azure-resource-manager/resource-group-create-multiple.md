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
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Distribuera flera instanser av en resurs eller en egenskap i Azure Resource Manager-mallar
Det här avsnittet beskrivs hur du tooiterate i din Azure Resource Manager-mall toocreate flera instanser av en resurs eller flera instanser av en egenskap för en resurs.

Om du behöver tooadd logik tooyour mall som du kan använda toospecify om en resurs har distribuerats, se [villkorligt distribuera resurs](#conditionally-deploy-resource).

## <a name="resource-iteration"></a>Resursen upprepning
toocreate flera instanser av en resurstyp, lägga till en `copy` elementtypen toohello resurs. Ange hello antalet upprepningar och ett namn för den här loop i hello kopiera element. hello count-värdet måste vara ett positivt heltal och får inte överskrida 800. Hanteraren för filserverresurser skapar hello resurser parallellt. Därför kan inte garanteras hello ordning som de skapades. toocreate hävdade resurser i följd, se [seriella kopiera](#serial-copy). 

hello resurs toocreate tar flera gånger hello följande format:

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

Observera att hello varje resurs ingår hello `copyIndex()` som returnerar hello aktuella iteration i hello loop. `copyIndex()`är nollbaserade. I så fall hello följande exempel:

```json
"name": "[concat('storage', copyIndex())]",
```

Skapar dessa namn:

* storage0
* storage1
* storage2.

indexvärdet för toooffset hello du skickar ett värde i hello copyIndex() funktion. hello antal upprepningar tooperform fortfarande anges i hello kopiera element, men hello värdet för copyIndex förskjutas av hello anges värdet. I så fall hello följande exempel:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Skapar dessa namn:

* storage1
* storage2
* storage3

hello kopieringsåtgärden är användbart när du arbetar med matriser eftersom du kan söka igenom varje element i matrisen hello. Använd hello `length` fungerar på hello matris toospecify hello-antal för iterationer, och `copyIndex` tooretrieve hello aktuellt index i hello matris. I så fall hello följande exempel:

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

Skapar dessa namn:

* storagecontoso
* storagefabrikam
* storagecoho

## <a name="serial-copy"></a>Seriell kopia

När du använder hello kopiera elementet toocreate flera instanser av en resurstyp, hanteraren för filserverresurser, som standard distribuerar dessa instanser parallellt. Men kanske du toospecify som hello resurser distribueras i följd. När du uppdaterar en produktionsmiljö, du kan exempelvis toostagger hello uppdateras så att bara ett visst antal uppdateras samtidigt.

Resource Manager innehåller egenskaper för hello kopiera element som aktiverar du tooserially distribuera flera instanser. Hello kopiera element, ange `mode` för**seriella** och `batchSize` toohello antal instanser toodeploy i taget. Med seriella läge skapar Resource Manager ett beroende på tidigare instanser i hello loop så att den inte startar en batch tills hello föregående batchen har slutförts.

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

hello egenskap accepterar också **parallella**, vilket är standardvärdet för hello.

tootest seriella kopia utan att skapa verkliga resurser används hello följande mall som distribuerar tom kapslade mallar:

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

Observera att hello kapslade distributioner bearbetas hello distributionshistoriken i följd.

![seriell distribution](./media/resource-group-create-multiple/serial-copy.png)

För en mer realistisk scenariot distribuerar hello följande exempel två instanser samtidigt för en Linux-VM från en kapslad mall:

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

## <a name="property-iteration"></a>Egenskapen upprepning

toocreate flera värden för en egenskap för en resurs lägger du till en `copy` matris i hello egenskaper för elementet. Denna matris innehåller objekt och alla objekt har hello följande egenskaper:

* namn - hello av hello egenskapen toocreate flera värden för
* Antal - hello värden toocreate
* indata - ett objekt som innehåller hello värden tooassign toohello egenskap  

följande exempel visar hur hello tooapply `copy` toohello dataDisks egenskapen på en virtuell dator:

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

Observera att när du använder `copyIndex` inuti en egenskapen iteration måste du ange hello namnet på hello iteration. Du har inte tooprovide hello namn när det används med resurs iteration.

Resource Manager utökas hello `copy` matris under distributionen. hello namn för hello matrisen blir hello namnet på hello-egenskap. hello indatavärden bli hello objektets egenskaper. Det blir hello distribueras mallen:

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

Du kan använda resursen och egenskapen iteration tillsammans. Referens hello egenskapen iteration efter namn.

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

Du får bara innehålla ett kopiera element i hello egenskaper för varje resurs. toospecify en iteration loop för mer än en egenskap, definiera flera objekt i hello Kopiera matris. Varje objekt är hävdade separat. Till exempel toocreate flera instanser av båda hello `frontendIPConfigurations` egenskap och hello `loadBalancingRules` egenskapen för en belastningsutjämnare definiera både objekt i en enda kopia-elementet: 

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

## <a name="depend-on-resources-in-a-loop"></a>Beroende av resurser i en loop
Du anger att en resurs distribueras efter en annan resurs med hjälp av hello `dependsOn` element. toodeploy en resurs som är beroende av hello mängd resurser i en slinga ange hello namnet för hello kopiera loop i hello dependsOn-element. hello som följande exempel visar hur toodeploy tre storage-konton innan du distribuerar hello virtuell dator. hello fullständig definition av virtuell dator visas inte. Observera att kopiera hello-element har namn som angetts för`storagecopy` och hello dependsOn-element för hello virtuella datorer har också angetts för`storagecopy`.

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

## <a name="create-multiple-instances-of-a-child-resource"></a>Skapa flera instanser av en underordnad-resurs
Du kan inte använda en kopia skapas för en underordnad resurs. toocreate flera instanser av en resurs som du vanligtvis definiera som kapslas i en annan resurs, måste du i stället skapa resursen som översta resurs. Du definierar hello relation med hello överordnade resursen via egenskaper för hello typ och namn.

Anta exempelvis att du definierar en datamängd vanligtvis som en underordnad resurs i en datafabrik.

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

toocreate flera instanser av datauppsättningar, flyttas utanför hello data factory. hello dataset måste finnas på samma nivå som hello data factory hello, men det är fortfarande en underordnad resurs i hello data factory. Du bevara hello förhållandet mellan datauppsättning och datafabriken via egenskaper för hello typ och namn. Eftersom typen inte kan härledas från dess placering i hello mall, måste du ange hello fullständigt kvalificerade typen hello format: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

tooestablish en överordnad-underordnad relation med en instans av hello data factory, ange ett namn för hello datauppsättning som innehåller hello överordnade resursnamnet. Använd hello format: `{parent-resource-name}/{child-resource-name}`.  

hello visar följande exempel hello implementering:

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

## <a name="conditionally-deploy-resource"></a>Villkorligt distribuera resurs

toospecify om en resurs har distribuerats, använda hello `condition` element. hello-värdet för det här elementet löser tootrue eller false. När hello-värdet är true, distribueras hello resurs. När hello-värdet är false har hello resursen inte distribuerats. Till exempel toospecify om ett nytt lagringskonto distribueras eller ett befintligt lagringskonto används, Använd:

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

Ett exempel på hur du använder en ny eller befintlig resurs finns [ny eller befintlig mall för villkoret](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).

Ett exempel på med ett lösenord eller SSH-nyckel toodeploy virtuella finns [användarnamn eller SSH villkoret mallen](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).

## <a name="next-steps"></a>Nästa steg
* Om du vill toolearn om hello avsnitt i en mall finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* toolearn hur toodeploy din mall finns [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).

