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
# <a name="add-reference-tooan-existing-virtual-network-in-an-azure-scale-set-template"></a><span data-ttu-id="05e11-103">Lägg till referens tooan befintligt virtuellt nätverk i en mall för Azure skala</span><span class="sxs-lookup"><span data-stu-id="05e11-103">Add reference tooan existing virtual network in an Azure scale set template</span></span>

<span data-ttu-id="05e11-104">Den här artikeln visar hur toomodify hello [lägsta lönsam skala ange mall](./virtual-machine-scale-sets-mvss-start.md) toodeploy till ett befintligt virtuellt nätverk i stället för att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="05e11-104">This article shows how toomodify hello [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) toodeploy into an existing virtual network instead of creating a new one.</span></span>

## <a name="change-hello-template-definition"></a><span data-ttu-id="05e11-105">Ändra hello malldefinitionen</span><span class="sxs-lookup"><span data-stu-id="05e11-105">Change hello template definition</span></span>

<span data-ttu-id="05e11-106">Mall för våra lägsta lönsam skala kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), och våra mallen för distribution av hello skala till ett befintligt virtuellt nätverk kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="05e11-106">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying hello scale set into an existing virtual network can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json).</span></span> <span data-ttu-id="05e11-107">Låt oss nu undersöka hello diff används toocreate mallen (`git diff minimum-viable-scale-set existing-vnet`) bit för bit:</span><span class="sxs-lookup"><span data-stu-id="05e11-107">Let's examine hello diff used toocreate this template (`git diff minimum-viable-scale-set existing-vnet`) piece by piece:</span></span>

<span data-ttu-id="05e11-108">Först måste vi lägga till en `subnetId` parameter.</span><span class="sxs-lookup"><span data-stu-id="05e11-108">First, we add a `subnetId` parameter.</span></span> <span data-ttu-id="05e11-109">Den här strängen skickas till hello skala ange konfiguration så att hello skaluppsättning tooidentify hello förskapade undernät toodeploy virtuella datorer i.</span><span class="sxs-lookup"><span data-stu-id="05e11-109">This string will be passed into hello scale set configuration, allowing hello scale set tooidentify hello pre-created subnet toodeploy virtual machines into.</span></span> <span data-ttu-id="05e11-110">Den här strängen måste ha formatet hello: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`.</span><span class="sxs-lookup"><span data-stu-id="05e11-110">This string must be of hello form: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`.</span></span> <span data-ttu-id="05e11-111">Till exempel toodeploy hello skalningsuppsättningen i ett befintligt virtuellt nätverk med namnet `myvnet`, undernät `mysubnet`, resursgrupp `myrg`, och prenumeration `00000000-0000-0000-0000-000000000000`, hello subnetId skulle vara: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.</span><span class="sxs-lookup"><span data-stu-id="05e11-111">For instance, toodeploy hello scale set into an existing virtual network with name `myvnet`, subnet `mysubnet`, resource group `myrg`, and subscription `00000000-0000-0000-0000-000000000000`, hello subnetId would be: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.</span></span>

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

<span data-ttu-id="05e11-112">Vi kan sedan ta bort hello virtuella nätverksresurs från hello `resources` matrisen eftersom vi använder ett befintligt virtuellt nätverk och behöver inte toodeploy en ny.</span><span class="sxs-lookup"><span data-stu-id="05e11-112">Next, we can delete hello virtual network resource from hello `resources` array, since we are using an existing virtual network and don't need toodeploy a new one.</span></span>

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

<span data-ttu-id="05e11-113">hello virtuella nätverk finns redan innan hello mallen distribueras, så det finns inget behov av toospecify dependsOn-satsen från hello skala anger toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="05e11-113">hello virtual network already exists before hello template is deployed, so there is no need toospecify a dependsOn clause from hello scale set toohello virtual network.</span></span> <span data-ttu-id="05e11-114">Därför vi ta bort dessa rader:</span><span class="sxs-lookup"><span data-stu-id="05e11-114">Thus, we delete these lines:</span></span>

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

<span data-ttu-id="05e11-115">Slutligen vi skicka in hello `subnetId` parameteruppsättning av hello användare (istället för att använda `resourceId` tooget hello-id för ett virtuellt nätverk i hello har samma-distribution är vilka hello lägsta lönsam skala ange mall).</span><span class="sxs-lookup"><span data-stu-id="05e11-115">Finally, we pass in hello `subnetId` parameter set by hello user (instead of using `resourceId` tooget hello id of a vnet in hello same deployment, which is what hello minimum viable scale set template does).</span></span>

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




## <a name="next-steps"></a><span data-ttu-id="05e11-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="05e11-116">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
