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
# <a name="add-a-custom-image-tooan-azure-scale-set-template"></a>Lägga till en anpassad avbildning tooan Azure skala mall

Den här artikeln visar hur toomodify hello [lägsta lönsam skala ange mall](./virtual-machine-scale-sets-mvss-start.md) toodeploy från anpassad avbildning.

## <a name="change-hello-template-definition"></a>Ändra hello malldefinitionen

Mall för våra lägsta lönsam skala kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), och våra mallen för distribution av hello skala från en anpassad avbildning kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json). Låt oss nu undersöka hello diff används toocreate mallen (`git diff minimum-viable-scale-set custom-image`) bit för bit:

### <a name="creating-a-managed-disk-image"></a>Skapa en hanterad avbildning

Om du redan har en anpassad hanterad diskavbildning (en resurs av typen `Microsoft.Compute/images`), och du kan hoppa över det här avsnittet.

Först måste vi lägga till en `sourceImageVhdUri` parameter som hello URI toohello generaliserad blob i Azure Storage som innehåller hello anpassad avbildning toodeploy från.


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

Nu ska vi lägga till en resurs av typen `Microsoft.Compute/images`, vilket är hello hanterade diskavbildning baserat på hello generaliserad blob finns på URI `sourceImageVhdUri`. Den här bilden måste vara i hello samma region som hello skaluppsättning som använder den. I hello egenskaper hello avbildningen kan vi ange hello OS-typ, hello platsen för hello blob (från hello `sourceImageVhdUri` parametern), och hello lagringskontotypen:

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

I hello skaluppsättning resurs, vi lägga till en `dependsOn` satsen hänvisar toohello anpassad avbildning toomake att hello avbildningen skapas innan hello skaluppsättning försöker toodeploy från den avbildningen:

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

### <a name="changing-scale-set-properties-toouse-hello-managed-disk-image"></a>Ändra skala ange egenskaperna toouse hello hanterad avbildning

I hello `imageReference` hello skalan ange `storageProfile`, i stället för att ange hello utgivare, erbjudande, sku och version av en plattformsavbildning kan vi ange hello `id` av hello `Microsoft.Compute/images` resurs:

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

I det här exemplet använder vi hello `resourceId` funktionen tooget hello resurs-ID för hello avbildningen skapas i hello samma mall. Om du har skapat hello hanterade diskavbildning i förväg, ska du ange hello-id för den bilden i stället. Detta id måste ha formatet hello: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.


## <a name="next-steps"></a>Nästa steg

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
