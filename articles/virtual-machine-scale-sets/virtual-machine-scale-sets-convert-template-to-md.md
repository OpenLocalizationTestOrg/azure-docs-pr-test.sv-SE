---
title: aaaConvert en Azure Resource Manager skala ange mall toouse hanterade diskar | Microsoft Docs
description: Konvertera en scale set mallen tooa hanterade diskar skala mall.
keywords: "Skaluppsättningar för den virtuella datorn"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: bc8c377a-8c3f-45b8-8b2d-acc2d6d0b1e8
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/18/2017
ms.author: negat
ms.openlocfilehash: 66c2217647e57ed2cfa39660c0175710ae2e63be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="convert-a-scale-set-template-tooa-managed-disk-scale-set-template"></a>Konvertera en scale set mallen tooa hanterade diskar skala mall

Kunder med en Resource Manager-mall för att skapa en skala som anges inte med hanterade diskar vill toomodify den toouse hanterade disken. Den här artikeln visar hur toodo detta, med hjälp av exempelvis en pull-begäran från hello [Azure Quickstart mallar](https://github.com/Azure/azure-quickstart-templates), en community-driven lagringsplatsen för Resource Manager exempelmallarna. hello fullständig pull-begäran kan visas här: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), och hello relevanta delar av hello diff nedan, tillsammans med förklaringar:

## <a name="making-hello-os-disks-managed"></a>Gör hello OS diskar som hanteras

I hello diff nedan ser vi att vi har tagit bort flera variabler relaterade toostorage disk egenskaper och. Lagringskontotypen inte längre behövs (Standard_LRS är hello standard), men vi kan ange den fortfarande om vi önskade. Endast Standard_LRS och Premium_LRS stöds med hanterade diskar. Nytt suffix för storage-konto och unika Strängmatrisen sa antal användes i hello gamla mallen toogenerate lagringskontonamn. Dessa variabler är inte längre behövs i hello mall eftersom hanterade diskar automatiskt skapar storage-konton för hello kundens räkning. På samma sätt vhd behållarens namn och os-diskens namn är inte längre nödvändigt eftersom hanterade diskar automatiskt namnet hello underliggande storage blob-behållare och diskar.

```diff
   "variables": {
-    "storageAccountType": "Standard_LRS",
     "namingInfix": "[toLower(substring(concat(parameters('vmssName'), uniqueString(resourceGroup().id)), 0, 9))]",
     "longNamingInfix": "[toLower(parameters('vmssName'))]",
-    "newStorageAccountSuffix": "[concat(variables('namingInfix'), 'sa')]",
-    "uniqueStringArray": [
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '0')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '1')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '2')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '3')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '4')))]"
-    ],
-    "saCount": "[length(variables('uniqueStringArray'))]",
-    "vhdContainerName": "[concat(variables('namingInfix'), 'vhd')]",
-    "osDiskName": "[concat(variables('namingInfix'), 'osdisk')]",
     "addressPrefix": "10.0.0.0/16",
     "subnetPrefix": "10.0.0.0/24",
     "virtualNetworkName": "[concat(variables('namingInfix'), 'vnet')]",
```


I hello diff nedan, vi kan se att vi uppdaterat hello beräkna api-version too2016-04-30-preview, vilket är hello tidigaste version som krävs för stöd för hanterade diskar med skaluppsättningar. Observera att vi kan fortfarande ohanterad diskar i hello nya api-versionen med hello gamla syntax om du vill. Med andra ord, om vi bara uppdatera hello compute api-versionen och inte ändra någonting annat, hello mallen ska fortsätta toowork som innan.

```diff
@@ -86,7 +74,7 @@
       "version": "latest"
     },
     "imageReference": "[variables('osType')]",
-    "computeApiVersion": "2016-03-30",
+    "computeApiVersion": "2016-04-30-preview",
     "networkApiVersion": "2016-03-30",
     "storageApiVersion": "2015-06-15"
   },
```

I hello diff nedan ser vi att vi tar bort hello lagringsresurs konto från hello resurser matris helt. Vi inte längre behöver dem. eftersom hanterade diskar skapar dem automatiskt för vår räkning.

```diff
@@ -113,19 +101,6 @@
       }
     },
-    {
-      "type": "Microsoft.Storage/storageAccounts",
-      "name": "[concat(variables('uniqueStringArray')[copyIndex()], variables('newStorageAccountSuffix'))]",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "[variables('storageApiVersion')]",
-      "copy": {
-        "name": "storageLoop",
-        "count": "[variables('saCount')]"
-      },
-      "properties": {
-        "accountType": "[variables('storageAccountType')]"
-      }
-    },
     {
       "type": "Microsoft.Network/publicIPAddresses",
       "name": "[variables('publicIPAddressName')]",
       "location": "[resourceGroup().location]",
```

I hello diff nedan, vi kan se att vi tar bort hello beroende satsen refererar från hello scale set toohello loop som skapade storage-konton. I hello gamla mallen Detta säkerställer att hello storage-konton har skapats innan hello skaluppsättning började skapas, men den här satsen är inte längre nödvändigt med hanterade diskar. Vi också ta bort hello vhd-behållare egenskap och hello namnegenskapen för os-disk som dessa egenskaper hanteras automatiskt under huven hello av hanterade diskar. Om vi önskas, vi kan lägga till `"managedDisk": { "storageAccountType": "Premium_LRS" }` i hello ”osDisk” konfiguration om vi vill OS premiumdiskar. Endast virtuella datorer med en versal eller gemens ' i hello VM sku använda premiumdiskar.

```diff
@@ -183,7 +158,6 @@
       "location": "[resourceGroup().location]",
       "apiVersion": "[variables('computeApiVersion')]",
       "dependsOn": [
-        "storageLoop",
         "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
         "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
       ],
@@ -200,16 +174,8 @@
         "virtualMachineProfile": {
           "storageProfile": {
             "osDisk": {
-              "vhdContainers": [
-                "[concat('https://', variables('uniqueStringArray')[0], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[1], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[2], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[3], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[4], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]"
-              ],
-              "name": "[variables('osDiskName')]",
             },
             "imageReference": "[variables('imageReference')]"
           },

```

Det finns ingen uttrycklig egenskap i hello skala ange konfiguration för om toouse hanterade eller ohanterade disk. Hej skaluppsättning vet vilken toouse baserat på hello egenskaper som finns i hello lagringsprofilen. Det är därför viktigt när du ändrar hello mallen tooensure som hello rätt egenskaper i hello lagringsprofilen för hello skaluppsättning.


## <a name="data-disks"></a>Datadiskar

Med ovanstående hello ändringar, hello scale set använder hanterade diskar för hello OS disk, men vad om datadiskar? tooadd datadiskar, Lägg till hello ”dataDisks”-egenskap under ”storageProfile” på samma nivå som ”osDisk” Hej. hello värdet för hello-egenskapen är en JSON-lista över objekt som har egenskaper ”lun” (som måste vara unikt för varje datadisk på en virtuell dator) ”createOption” (”tom” är för närvarande hello stöds alternativet endast), och ”diskSizeGB” (hello storleken på hello disken i gigabyte; måste vara större än 0 och mindre än 1024) som i följande exempel hello: 

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

Om du anger `n` diskar i denna matris varje virtuell dator i hello skala ange hämtar `n` datadiskar. Observera dock att dessa datadiskar är raw-enheter. De har inte formaterats. Är det upp toohello kunden tooattach, partitionen och format hello diskar innan du använder dem. Vi kan också ange eventuellt `"managedDisk": { "storageAccountType": "Premium_LRS" }` i varje data disk objektet toospecify att det ska vara en disk för premium-data. Endast virtuella datorer med en versal eller gemens ' i hello VM sku använda premiumdiskar.

toolearn mer information om hur du använder datadiskar med skalningsuppsättningar, se [i den här artikeln](./virtual-machine-scale-sets-attached-disks.md).


## <a name="next-steps"></a>Nästa steg
Till exempel Resource Manager-mallar med hjälp av skalningsuppsättningar, söka efter ”vmss” i hello [github-repo-mallar för Azure Quickstart](https://github.com/Azure/azure-quickstart-templates).

Allmän information kolla hello [huvudsakliga Landningssida för skalningsuppsättningar](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

