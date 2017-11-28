---
title: "Referera till ett befintligt virtuellt nätverk i en mall för Azure skala | Microsoft Docs"
description: "Lär dig hur du lägger till ett virtuellt nätverk i en befintlig mall för Azure Virtual Machine Scale Set"
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
ms.openlocfilehash: 28117d467b491704aed8d45e5eba42530579dfa2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a><span data-ttu-id="a9f30-103">Lägg till referens till ett befintligt virtuellt nätverk i en mall för Azure skala</span><span class="sxs-lookup"><span data-stu-id="a9f30-103">Add reference to an existing virtual network in an Azure scale set template</span></span>

<span data-ttu-id="a9f30-104">Den här artikeln visar hur du ändrar den [lägsta lönsam skala ange mall](./virtual-machine-scale-sets-mvss-start.md) att distribuera till ett befintligt virtuellt nätverk i stället för att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="a9f30-104">This article shows how to modify the [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) to deploy into an existing virtual network instead of creating a new one.</span></span>

## <a name="change-the-template-definition"></a><span data-ttu-id="a9f30-105">Ändra malldefinitionen</span><span class="sxs-lookup"><span data-stu-id="a9f30-105">Change the template definition</span></span>

<span data-ttu-id="a9f30-106">Mall för våra lägsta lönsam skala kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), och våra mallen för distribution av skalan i ett befintligt virtuellt nätverk kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="a9f30-106">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying the scale set into an existing virtual network can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json).</span></span> <span data-ttu-id="a9f30-107">Låt oss nu undersöka diff som används för att skapa den här mallen (`git diff minimum-viable-scale-set existing-vnet`) bit för bit:</span><span class="sxs-lookup"><span data-stu-id="a9f30-107">Let's examine the diff used to create this template (`git diff minimum-viable-scale-set existing-vnet`) piece by piece:</span></span>

<span data-ttu-id="a9f30-108">Först måste vi lägga till en `subnetId` parameter.</span><span class="sxs-lookup"><span data-stu-id="a9f30-108">First, we add a `subnetId` parameter.</span></span> <span data-ttu-id="a9f30-109">Den här strängen skickas till skala ange konfiguration så att skaluppsättningen att identifiera om du vill distribuera virtuella datorer i förväg skapade undernätet.</span><span class="sxs-lookup"><span data-stu-id="a9f30-109">This string will be passed into the scale set configuration, allowing the scale set to identify the pre-created subnet to deploy virtual machines into.</span></span> <span data-ttu-id="a9f30-110">Strängen måste vara i formatet: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`.</span><span class="sxs-lookup"><span data-stu-id="a9f30-110">This string must be of the form: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`.</span></span> <span data-ttu-id="a9f30-111">Ange till exempel att distribuera skalan i ett befintligt virtuellt nätverk med namnet `myvnet`, undernät `mysubnet`, resursgrupp `myrg`, och prenumeration `00000000-0000-0000-0000-000000000000`, subnetId skulle vara: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.</span><span class="sxs-lookup"><span data-stu-id="a9f30-111">For instance, to deploy the scale set into an existing virtual network with name `myvnet`, subnet `mysubnet`, resource group `myrg`, and subscription `00000000-0000-0000-0000-000000000000`, the subnetId would be: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.</span></span>

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

<span data-ttu-id="a9f30-112">Nu ska vi kan ta bort resursen virtuellt nätverk från den `resources` matrisen eftersom vi använder ett befintligt virtuellt nätverk och behöver inte distribuera en ny.</span><span class="sxs-lookup"><span data-stu-id="a9f30-112">Next, we can delete the virtual network resource from the `resources` array, since we are using an existing virtual network and don't need to deploy a new one.</span></span>

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

<span data-ttu-id="a9f30-113">Det virtuella nätverket redan innan mallen har distribuerats, så inte behöver du ange en dependsOn-sats från skaluppsättningen till det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="a9f30-113">The virtual network already exists before the template is deployed, so there is no need to specify a dependsOn clause from the scale set to the virtual network.</span></span> <span data-ttu-id="a9f30-114">Därför vi ta bort dessa rader:</span><span class="sxs-lookup"><span data-stu-id="a9f30-114">Thus, we delete these lines:</span></span>

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

<span data-ttu-id="a9f30-115">Slutligen kan vi skicka in den `subnetId` parameter har angetts av användaren (istället för att använda `resourceId` för att hämta id för ett vnet i samma distribution, vilket är vad den lägsta lönsam skalan ange mall har).</span><span class="sxs-lookup"><span data-stu-id="a9f30-115">Finally, we pass in the `subnetId` parameter set by the user (instead of using `resourceId` to get the id of a vnet in the same deployment, which is what the minimum viable scale set template does).</span></span>

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




## <a name="next-steps"></a><span data-ttu-id="a9f30-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9f30-116">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
