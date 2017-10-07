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
# <a name="convert-a-scale-set-template-tooa-managed-disk-scale-set-template"></a><span data-ttu-id="6172f-104">Konvertera en scale set mallen tooa hanterade diskar skala mall</span><span class="sxs-lookup"><span data-stu-id="6172f-104">Convert a scale set template tooa managed disk scale set template</span></span>

<span data-ttu-id="6172f-105">Kunder med en Resource Manager-mall för att skapa en skala som anges inte med hanterade diskar vill toomodify den toouse hanterade disken.</span><span class="sxs-lookup"><span data-stu-id="6172f-105">Customers with a Resource Manager template for creating a scale set not using managed disk may wish toomodify it toouse managed disk.</span></span> <span data-ttu-id="6172f-106">Den här artikeln visar hur toodo detta, med hjälp av exempelvis en pull-begäran från hello [Azure Quickstart mallar](https://github.com/Azure/azure-quickstart-templates), en community-driven lagringsplatsen för Resource Manager exempelmallarna.</span><span class="sxs-lookup"><span data-stu-id="6172f-106">This article shows how toodo this, using as an example a pull request from hello [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates), a community-driven repo for sample Resource Manager templates.</span></span> <span data-ttu-id="6172f-107">hello fullständig pull-begäran kan visas här: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), och hello relevanta delar av hello diff nedan, tillsammans med förklaringar:</span><span class="sxs-lookup"><span data-stu-id="6172f-107">hello full pull request can be seen here: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), and hello relevant parts of hello diff are below, along with explanations:</span></span>

## <a name="making-hello-os-disks-managed"></a><span data-ttu-id="6172f-108">Gör hello OS diskar som hanteras</span><span class="sxs-lookup"><span data-stu-id="6172f-108">Making hello OS disks managed</span></span>

<span data-ttu-id="6172f-109">I hello diff nedan ser vi att vi har tagit bort flera variabler relaterade toostorage disk egenskaper och.</span><span class="sxs-lookup"><span data-stu-id="6172f-109">In hello diff below, we can see that we have removed several variables related toostorage account and disk properties.</span></span> <span data-ttu-id="6172f-110">Lagringskontotypen inte längre behövs (Standard_LRS är hello standard), men vi kan ange den fortfarande om vi önskade.</span><span class="sxs-lookup"><span data-stu-id="6172f-110">Storage account type is no longer necessary (Standard_LRS is hello default), but we could still specify it if we wished to.</span></span> <span data-ttu-id="6172f-111">Endast Standard_LRS och Premium_LRS stöds med hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="6172f-111">Only Standard_LRS and Premium_LRS are supported with managed disk.</span></span> <span data-ttu-id="6172f-112">Nytt suffix för storage-konto och unika Strängmatrisen sa antal användes i hello gamla mallen toogenerate lagringskontonamn.</span><span class="sxs-lookup"><span data-stu-id="6172f-112">New storage account suffix, unique string array, and sa count were used in hello old template toogenerate storage account names.</span></span> <span data-ttu-id="6172f-113">Dessa variabler är inte längre behövs i hello mall eftersom hanterade diskar automatiskt skapar storage-konton för hello kundens räkning.</span><span class="sxs-lookup"><span data-stu-id="6172f-113">These variables are no longer necessary in hello new template because managed disk automatically creates storage accounts on hello customer's behalf.</span></span> <span data-ttu-id="6172f-114">På samma sätt vhd behållarens namn och os-diskens namn är inte längre nödvändigt eftersom hanterade diskar automatiskt namnet hello underliggande storage blob-behållare och diskar.</span><span class="sxs-lookup"><span data-stu-id="6172f-114">Similarly, vhd container name and os disk name are no longer necessary because managed disk automatically names hello underlying storage blob containers and disks.</span></span>

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


<span data-ttu-id="6172f-115">I hello diff nedan, vi kan se att vi uppdaterat hello beräkna api-version too2016-04-30-preview, vilket är hello tidigaste version som krävs för stöd för hanterade diskar med skaluppsättningar.</span><span class="sxs-lookup"><span data-stu-id="6172f-115">In hello diff below, we can see that we updated hello compute api version too2016-04-30-preview, which is hello earliest required version for managed disk support with scale sets.</span></span> <span data-ttu-id="6172f-116">Observera att vi kan fortfarande ohanterad diskar i hello nya api-versionen med hello gamla syntax om du vill.</span><span class="sxs-lookup"><span data-stu-id="6172f-116">Note that we could still use unmanaged disks in hello new api version with hello old syntax if desired.</span></span> <span data-ttu-id="6172f-117">Med andra ord, om vi bara uppdatera hello compute api-versionen och inte ändra någonting annat, hello mallen ska fortsätta toowork som innan.</span><span class="sxs-lookup"><span data-stu-id="6172f-117">In other words, if we only update hello compute api version and don't change anything else, hello template should continue toowork as before.</span></span>

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

<span data-ttu-id="6172f-118">I hello diff nedan ser vi att vi tar bort hello lagringsresurs konto från hello resurser matris helt.</span><span class="sxs-lookup"><span data-stu-id="6172f-118">In hello diff below, we can see that we are removing hello storage account resource from hello resources array completely.</span></span> <span data-ttu-id="6172f-119">Vi inte längre behöver dem. eftersom hanterade diskar skapar dem automatiskt för vår räkning.</span><span class="sxs-lookup"><span data-stu-id="6172f-119">We no longer need them since managed disk creates them automatically on our behalf.</span></span>

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

<span data-ttu-id="6172f-120">I hello diff nedan, vi kan se att vi tar bort hello beroende satsen refererar från hello scale set toohello loop som skapade storage-konton.</span><span class="sxs-lookup"><span data-stu-id="6172f-120">In hello diff below, we can see that we are removing hello depends on clause referring from hello scale set toohello loop that was creating storage accounts.</span></span> <span data-ttu-id="6172f-121">I hello gamla mallen Detta säkerställer att hello storage-konton har skapats innan hello skaluppsättning började skapas, men den här satsen är inte längre nödvändigt med hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="6172f-121">In hello old template, this was ensuring that hello storage accounts were created before hello scale set began creation, but this clause is no longer necessary with managed disk.</span></span> <span data-ttu-id="6172f-122">Vi också ta bort hello vhd-behållare egenskap och hello namnegenskapen för os-disk som dessa egenskaper hanteras automatiskt under huven hello av hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="6172f-122">We also remove hello vhd containers property, and hello os disk name property as these properties are automatically handled under hello hood by managed disk.</span></span> <span data-ttu-id="6172f-123">Om vi önskas, vi kan lägga till `"managedDisk": { "storageAccountType": "Premium_LRS" }` i hello ”osDisk” konfiguration om vi vill OS premiumdiskar.</span><span class="sxs-lookup"><span data-stu-id="6172f-123">If we wished, we could add `"managedDisk": { "storageAccountType": "Premium_LRS" }` in hello "osDisk" configuration if we wanted premium OS disks.</span></span> <span data-ttu-id="6172f-124">Endast virtuella datorer med en versal eller gemens ' i hello VM sku använda premiumdiskar.</span><span class="sxs-lookup"><span data-stu-id="6172f-124">Only VMs with an uppercase or lowercase 's' in hello VM sku can use premium disks.</span></span>

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

<span data-ttu-id="6172f-125">Det finns ingen uttrycklig egenskap i hello skala ange konfiguration för om toouse hanterade eller ohanterade disk.</span><span class="sxs-lookup"><span data-stu-id="6172f-125">There is no explicit property in hello scale set configuration for whether toouse managed or unmanaged disk.</span></span> <span data-ttu-id="6172f-126">Hej skaluppsättning vet vilken toouse baserat på hello egenskaper som finns i hello lagringsprofilen.</span><span class="sxs-lookup"><span data-stu-id="6172f-126">hello scale set knows which toouse based on hello properties that are present in hello storage profile.</span></span> <span data-ttu-id="6172f-127">Det är därför viktigt när du ändrar hello mallen tooensure som hello rätt egenskaper i hello lagringsprofilen för hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="6172f-127">Thus, it is important when modifying hello template tooensure that hello right properties are in hello storage profile of hello scale set.</span></span>


## <a name="data-disks"></a><span data-ttu-id="6172f-128">Datadiskar</span><span class="sxs-lookup"><span data-stu-id="6172f-128">Data disks</span></span>

<span data-ttu-id="6172f-129">Med ovanstående hello ändringar, hello scale set använder hanterade diskar för hello OS disk, men vad om datadiskar? tooadd datadiskar, Lägg till hello ”dataDisks”-egenskap under ”storageProfile” på samma nivå som ”osDisk” Hej.</span><span class="sxs-lookup"><span data-stu-id="6172f-129">With hello changes above, hello scale set uses managed disks for hello OS disk, but what about data disks? tooadd data disks, add hello "dataDisks" property under "storageProfile" at hello same level as "osDisk".</span></span> <span data-ttu-id="6172f-130">hello värdet för hello-egenskapen är en JSON-lista över objekt som har egenskaper ”lun” (som måste vara unikt för varje datadisk på en virtuell dator) ”createOption” (”tom” är för närvarande hello stöds alternativet endast), och ”diskSizeGB” (hello storleken på hello disken i gigabyte; måste vara större än 0 och mindre än 1024) som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="6172f-130">hello value of hello property is a JSON list of objects, each of which has properties "lun" (which must be unique per data disk on a VM), "createOption" ("empty" is currently hello only supported option), and "diskSizeGB" (hello size of hello disk in gigabytes; must be greater than 0 and less than 1024) as in hello following example:</span></span> 

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

<span data-ttu-id="6172f-131">Om du anger `n` diskar i denna matris varje virtuell dator i hello skala ange hämtar `n` datadiskar.</span><span class="sxs-lookup"><span data-stu-id="6172f-131">If you specify `n` disks in this array, each VM in hello scale set gets `n` data disks.</span></span> <span data-ttu-id="6172f-132">Observera dock att dessa datadiskar är raw-enheter.</span><span class="sxs-lookup"><span data-stu-id="6172f-132">Do note, however, that these data disks are raw devices.</span></span> <span data-ttu-id="6172f-133">De har inte formaterats.</span><span class="sxs-lookup"><span data-stu-id="6172f-133">They are not formatted.</span></span> <span data-ttu-id="6172f-134">Är det upp toohello kunden tooattach, partitionen och format hello diskar innan du använder dem.</span><span class="sxs-lookup"><span data-stu-id="6172f-134">It is up toohello customer tooattach, paritition, and format hello disks before using them.</span></span> <span data-ttu-id="6172f-135">Vi kan också ange eventuellt `"managedDisk": { "storageAccountType": "Premium_LRS" }` i varje data disk objektet toospecify att det ska vara en disk för premium-data.</span><span class="sxs-lookup"><span data-stu-id="6172f-135">Optionally, we could also specify `"managedDisk": { "storageAccountType": "Premium_LRS" }` in each data disk object toospecify that it should be a premium data disk.</span></span> <span data-ttu-id="6172f-136">Endast virtuella datorer med en versal eller gemens ' i hello VM sku använda premiumdiskar.</span><span class="sxs-lookup"><span data-stu-id="6172f-136">Only VMs with an uppercase or lowercase 's' in hello VM sku can use premium disks.</span></span>

<span data-ttu-id="6172f-137">toolearn mer information om hur du använder datadiskar med skalningsuppsättningar, se [i den här artikeln](./virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="6172f-137">toolearn more about using data disks with scale sets, see [this article](./virtual-machine-scale-sets-attached-disks.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="6172f-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6172f-138">Next steps</span></span>
<span data-ttu-id="6172f-139">Till exempel Resource Manager-mallar med hjälp av skalningsuppsättningar, söka efter ”vmss” i hello [github-repo-mallar för Azure Quickstart](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="6172f-139">For example Resource Manager templates using scale sets, search for "vmss" in hello [Azure Quickstart Templates github repo](https://github.com/Azure/azure-quickstart-templates).</span></span>

<span data-ttu-id="6172f-140">Allmän information kolla hello [huvudsakliga Landningssida för skalningsuppsättningar](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span><span class="sxs-lookup"><span data-stu-id="6172f-140">For general information, check out hello [main landing page for scale sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span></span>

