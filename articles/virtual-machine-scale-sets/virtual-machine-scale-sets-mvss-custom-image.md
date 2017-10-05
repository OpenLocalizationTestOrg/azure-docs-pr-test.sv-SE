---
title: "Referera till en anpassad avbildning i en mall för Azure skala | Microsoft Docs"
description: "Lär dig hur du lägger till en anpassad avbildning i en befintlig mall för Azure Virtual Machine Scale Set"
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
ms.date: 5/10/2017
ms.author: negat
ms.openlocfilehash: cf52fc9e95267c4bc5c0106aadf626685ddd5c24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a><span data-ttu-id="70f83-103">Lägg till en anpassad avbildning i en mall för Azure skala</span><span class="sxs-lookup"><span data-stu-id="70f83-103">Add a custom image to an Azure scale set template</span></span>

<span data-ttu-id="70f83-104">Den här artikeln visar hur du ändrar den [lägsta lönsam skala ange mall](./virtual-machine-scale-sets-mvss-start.md) ska distribueras från anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="70f83-104">This article shows how to modify the [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) to deploy from custom image.</span></span>

## <a name="change-the-template-definition"></a><span data-ttu-id="70f83-105">Ändra malldefinitionen</span><span class="sxs-lookup"><span data-stu-id="70f83-105">Change the template definition</span></span>

<span data-ttu-id="70f83-106">Mall för våra lägsta lönsam skala kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), och våra mall för distribution av skalan som från en anpassad avbildning kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="70f83-106">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying the scale set from a custom image can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json).</span></span> <span data-ttu-id="70f83-107">Låt oss nu undersöka diff som används för att skapa den här mallen (`git diff minimum-viable-scale-set custom-image`) bit för bit:</span><span class="sxs-lookup"><span data-stu-id="70f83-107">Let's examine the diff used to create this template (`git diff minimum-viable-scale-set custom-image`) piece by piece:</span></span>

### <a name="creating-a-managed-disk-image"></a><span data-ttu-id="70f83-108">Skapa en hanterad avbildning</span><span class="sxs-lookup"><span data-stu-id="70f83-108">Creating a managed disk image</span></span>

<span data-ttu-id="70f83-109">Om du redan har en anpassad hanterad diskavbildning (en resurs av typen `Microsoft.Compute/images`), och du kan hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="70f83-109">If you already have a custom managed disk image (a resource of type `Microsoft.Compute/images`), then you can skip this section.</span></span>

<span data-ttu-id="70f83-110">Först måste vi lägga till en `sourceImageVhdUri` parametern, som är URI: N till generaliserad blobb i Azure Storage som innehåller den anpassade avbildningen ska distribueras från.</span><span class="sxs-lookup"><span data-stu-id="70f83-110">First, we add a `sourceImageVhdUri` parameter, which is the URI to the generalized blob in Azure Storage that contains the custom image to deploy from.</span></span>


```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "sourceImageVhdUri": {
+      "type": "string",
+      "metadata": {
+        "description": "The source of the generalized blob containing the custom image"
+      }
     }
   },
   "variables": {},
```

<span data-ttu-id="70f83-111">Nu ska vi lägga till en resurs av typen `Microsoft.Compute/images`, vilket är den hanterade diskavbildning baserat på generaliserad blob finns på URI `sourceImageVhdUri`.</span><span class="sxs-lookup"><span data-stu-id="70f83-111">Next, we add a resource of type `Microsoft.Compute/images`, which is the managed disk image based on the generalized blob located at URI `sourceImageVhdUri`.</span></span> <span data-ttu-id="70f83-112">Den här bilden måste vara i samma region som den skaluppsättning som använder den.</span><span class="sxs-lookup"><span data-stu-id="70f83-112">This image must be in the same region as the scale set that uses it.</span></span> <span data-ttu-id="70f83-113">I egenskaperna för avbildningen kan vi ange OS-typen platsen för blob (från den `sourceImageVhdUri` parametern), och lagringskontotypen:</span><span class="sxs-lookup"><span data-stu-id="70f83-113">In the properties of the image, we specify the OS type, the location of the blob (from the `sourceImageVhdUri` parameter), and the storage account type:</span></span>

```diff
   "resources": [
     {
+      "type": "Microsoft.Compute/images",
+      "apiVersion": "2016-04-30-preview",
+      "name": "myCustomImage",
+      "location": "[resourceGroup().location]",
+      "properties": {
+        "storageProfile": {
+          "osDisk": {
+            "osType": "Linux",
+            "osState": "Generalized",
+            "blobUri": "[parameters('sourceImageVhdUri')]",
+            "storageAccountType": "Standard_LRS"
+          }
+        }
+      }
+    },
+    {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "location": "[resourceGroup().location]",

```

<span data-ttu-id="70f83-114">Skalan ange resurs, vi lägga till en `dependsOn` instruktion refererar till den anpassade avbildningen för att kontrollera avbildningen skapas innan du försöker distribuera från den avbildningen skaluppsättning:</span><span class="sxs-lookup"><span data-stu-id="70f83-114">In the scale set resource, we add a `dependsOn` clause referring to the custom image to make sure the image gets created before the scale set tries to deploy from that image:</span></span>

```diff
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
       "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
+        "Microsoft.Network/virtualNetworks/myVnet",
+        "Microsoft.Compute/images/myCustomImage"
       ],
       "sku": {
         "name": "Standard_A1",

```

### <a name="changing-scale-set-properties-to-use-the-managed-disk-image"></a><span data-ttu-id="70f83-115">Ändra skala ange egenskaper för att använda den hanterade diskavbildning</span><span class="sxs-lookup"><span data-stu-id="70f83-115">Changing scale set properties to use the managed disk image</span></span>

<span data-ttu-id="70f83-116">I den `imageReference` skalans ange `storageProfile`, istället för att ange utgivare, erbjudande, sku och version av en plattformsavbildning anger vi den `id` av den `Microsoft.Compute/images` resursen:</span><span class="sxs-lookup"><span data-stu-id="70f83-116">In the `imageReference` of the scale set `storageProfile`, instead of specifying the publisher, offer, sku, and version of a platform image, we specify the `id` of the `Microsoft.Compute/images` resource:</span></span>

```diff
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
-              "publisher": "Canonical",
-              "offer": "UbuntuServer",
-              "sku": "16.04-LTS",
-              "version": "latest"
+              "id": "[resourceId('Microsoft.Compute/images', 'myCustomImage')]"
             }
           },
           "osProfile": {
```

<span data-ttu-id="70f83-117">I det här exemplet använder vi den `resourceId` funktion för att hämta resurs-ID för den avbildning som har skapats i samma mall.</span><span class="sxs-lookup"><span data-stu-id="70f83-117">In this example, we use the `resourceId` function to get the resource ID of the image created in the same template.</span></span> <span data-ttu-id="70f83-118">Om du har skapat den hanterade diskavbildning i förväg, ska du i stället ange id för avbildningen.</span><span class="sxs-lookup"><span data-stu-id="70f83-118">If you have created the managed disk image beforehand, you should provide the id of that image instead.</span></span> <span data-ttu-id="70f83-119">Detta id måste vara i formatet: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.</span><span class="sxs-lookup"><span data-stu-id="70f83-119">This id must be of the form: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.</span></span>


## <a name="next-steps"></a><span data-ttu-id="70f83-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="70f83-120">Next Steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
