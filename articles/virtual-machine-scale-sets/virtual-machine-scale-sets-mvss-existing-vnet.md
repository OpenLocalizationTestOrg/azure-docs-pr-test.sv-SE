---
title: "Referera till ett befintligt virtuellt nätverk i en mall för Azure skala | Microsoft Docs"
description: "Lär dig hur tooadd ett virtuellt nätverk tooan befintlig Azure Virtual Machine Scale Set-mall"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: negat
ms.openlocfilehash: c3034b577e17abc4643dc26d7c38ad643fa26322
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-reference-tooan-existing-virtual-network-in-an-azure-scale-set-template"></a>Lägg till referens tooan befintligt virtuellt nätverk i en mall för Azure skala

Den här artikeln visar hur toomodify hello [lägsta lönsam skala ange mall](./virtual-machine-scale-sets-mvss-start.md) toodeploy till ett befintligt virtuellt nätverk i stället för att skapa en ny.

## <a name="change-hello-template-definition"></a>Ändra hello malldefinitionen

Mall för våra lägsta lönsam skala kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), och våra mallen för distribution av hello skala till ett befintligt virtuellt nätverk kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json). Låt oss nu undersöka hello diff används toocreate mallen (`git diff minimum-viable-scale-set existing-vnet`) bit för bit:

Först måste vi lägga till en `subnetId` parameter. Den här strängen skickas till hello skala ange konfiguration så att hello skaluppsättning tooidentify hello förskapade undernät toodeploy virtuella datorer i. Den här strängen måste ha formatet hello: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`. Till exempel toodeploy hello skalningsuppsättningen i ett befintligt virtuellt nätverk med namnet `myvnet`, undernät `mysubnet`, resursgrupp `myrg`, och prenumeration `00000000-0000-0000-0000-000000000000`, hello subnetId skulle vara: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

Vi kan sedan ta bort hello virtuella nätverksresurs från hello `resources` matrisen eftersom vi använder ett befintligt virtuellt nätverk och behöver inte toodeploy en ny.

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2016-12-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

hello virtuella nätverk finns redan innan hello mallen distribueras, så det finns inget behov av toospecify dependsOn-satsen från hello skala anger toohello virtuellt nätverk. Därför vi ta bort dessa rader:

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

Slutligen vi skicka in hello `subnetId` parameteruppsättning av hello användare (istället för att använda `resourceId` tooget hello-id för ett virtuellt nätverk i hello har samma-distribution är vilka hello lägsta lönsam skala ange mall).

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a>Nästa steg

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
