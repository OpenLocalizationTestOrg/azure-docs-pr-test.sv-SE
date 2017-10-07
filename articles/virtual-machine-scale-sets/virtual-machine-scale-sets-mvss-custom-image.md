---
title: "Referera till en anpassad avbildning i en mall för Azure skala | Microsoft Docs"
description: "Lär dig hur tooadd en anpassad avbildning tooan befintlig Skaluppsättning för virtuell Azure-mall"
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
ms.openlocfilehash: 6a17d989e44d241b460238c0106350c3ef038e56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-custom-image-tooan-azure-scale-set-template"></a><span data-ttu-id="34d79-103">Lägga till en anpassad avbildning tooan Azure skala mall</span><span class="sxs-lookup"><span data-stu-id="34d79-103">Add a custom image tooan Azure scale set template</span></span>

<span data-ttu-id="34d79-104">Den här artikeln visar hur toomodify hello [lägsta lönsam skala ange mall](./virtual-machine-scale-sets-mvss-start.md) toodeploy från anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="34d79-104">This article shows how toomodify hello [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) toodeploy from custom image.</span></span>

## <a name="change-hello-template-definition"></a><span data-ttu-id="34d79-105">Ändra hello malldefinitionen</span><span class="sxs-lookup"><span data-stu-id="34d79-105">Change hello template definition</span></span>

<span data-ttu-id="34d79-106">Mall för våra lägsta lönsam skala kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), och våra mallen för distribution av hello skala från en anpassad avbildning kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="34d79-106">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying hello scale set from a custom image can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json).</span></span> <span data-ttu-id="34d79-107">Låt oss nu undersöka hello diff används toocreate mallen (`git diff minimum-viable-scale-set custom-image`) bit för bit:</span><span class="sxs-lookup"><span data-stu-id="34d79-107">Let's examine hello diff used toocreate this template (`git diff minimum-viable-scale-set custom-image`) piece by piece:</span></span>

### <a name="creating-a-managed-disk-image"></a><span data-ttu-id="34d79-108">Skapa en hanterad avbildning</span><span class="sxs-lookup"><span data-stu-id="34d79-108">Creating a managed disk image</span></span>

<span data-ttu-id="34d79-109">Om du redan har en anpassad hanterad diskavbildning (en resurs av typen `Microsoft.Compute/images`), och du kan hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="34d79-109">If you already have a custom managed disk image (a resource of type `Microsoft.Compute/images`), then you can skip this section.</span></span>

<span data-ttu-id="34d79-110">Först måste vi lägga till en `sourceImageVhdUri` parameter som hello URI toohello generaliserad blob i Azure Storage som innehåller hello anpassad avbildning toodeploy från.</span><span class="sxs-lookup"><span data-stu-id="34d79-110">First, we add a `sourceImageVhdUri` parameter, which is hello URI toohello generalized blob in Azure Storage that contains hello custom image toodeploy from.</span></span>


```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "sourceImageVhdUri": {
+      "type": "string",
+      "metadata": {
+        "description": "hello source of hello generalized blob containing hello custom image"
+      }
     }
   },
   "variables": {},
```

<span data-ttu-id="34d79-111">Nu ska vi lägga till en resurs av typen `Microsoft.Compute/images`, vilket är hello hanterade diskavbildning baserat på hello generaliserad blob finns på URI `sourceImageVhdUri`.</span><span class="sxs-lookup"><span data-stu-id="34d79-111">Next, we add a resource of type `Microsoft.Compute/images`, which is hello managed disk image based on hello generalized blob located at URI `sourceImageVhdUri`.</span></span> <span data-ttu-id="34d79-112">Den här bilden måste vara i hello samma region som hello skaluppsättning som använder den.</span><span class="sxs-lookup"><span data-stu-id="34d79-112">This image must be in hello same region as hello scale set that uses it.</span></span> <span data-ttu-id="34d79-113">I hello egenskaper hello avbildningen kan vi ange hello OS-typ, hello platsen för hello blob (från hello `sourceImageVhdUri` parametern), och hello lagringskontotypen:</span><span class="sxs-lookup"><span data-stu-id="34d79-113">In hello properties of hello image, we specify hello OS type, hello location of hello blob (from hello `sourceImageVhdUri` parameter), and hello storage account type:</span></span>

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

<span data-ttu-id="34d79-114">I hello skaluppsättning resurs, vi lägga till en `dependsOn` satsen hänvisar toohello anpassad avbildning toomake att hello avbildningen skapas innan hello skaluppsättning försöker toodeploy från den avbildningen:</span><span class="sxs-lookup"><span data-stu-id="34d79-114">In hello scale set resource, we add a `dependsOn` clause referring toohello custom image toomake sure hello image gets created before hello scale set tries toodeploy from that image:</span></span>

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

### <a name="changing-scale-set-properties-toouse-hello-managed-disk-image"></a><span data-ttu-id="34d79-115">Ändra skala ange egenskaperna toouse hello hanterad avbildning</span><span class="sxs-lookup"><span data-stu-id="34d79-115">Changing scale set properties toouse hello managed disk image</span></span>

<span data-ttu-id="34d79-116">I hello `imageReference` hello skalan ange `storageProfile`, i stället för att ange hello utgivare, erbjudande, sku och version av en plattformsavbildning kan vi ange hello `id` av hello `Microsoft.Compute/images` resurs:</span><span class="sxs-lookup"><span data-stu-id="34d79-116">In hello `imageReference` of hello scale set `storageProfile`, instead of specifying hello publisher, offer, sku, and version of a platform image, we specify hello `id` of hello `Microsoft.Compute/images` resource:</span></span>

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

<span data-ttu-id="34d79-117">I det här exemplet använder vi hello `resourceId` funktionen tooget hello resurs-ID för hello avbildningen skapas i hello samma mall.</span><span class="sxs-lookup"><span data-stu-id="34d79-117">In this example, we use hello `resourceId` function tooget hello resource ID of hello image created in hello same template.</span></span> <span data-ttu-id="34d79-118">Om du har skapat hello hanterade diskavbildning i förväg, ska du ange hello-id för den bilden i stället.</span><span class="sxs-lookup"><span data-stu-id="34d79-118">If you have created hello managed disk image beforehand, you should provide hello id of that image instead.</span></span> <span data-ttu-id="34d79-119">Detta id måste ha formatet hello: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.</span><span class="sxs-lookup"><span data-stu-id="34d79-119">This id must be of hello form: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.</span></span>


## <a name="next-steps"></a><span data-ttu-id="34d79-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34d79-120">Next Steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
