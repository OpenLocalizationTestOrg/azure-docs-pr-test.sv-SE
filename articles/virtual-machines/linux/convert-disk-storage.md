---
title: "aaaConvert Azure hanterade diskar lagring från standard toopremium och vice versa | Microsoft Docs"
description: "Hur hanteras tooconvert Azure storage diskar från standard toopremium och vice versa med hjälp av Azure CLI."
services: virtual-machines-linux
documentationcenter: 
author: ramankum
manager: kavithag
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: 261d77202f7fd381085c4e25211a5d0026f43b01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="06d9b-103">Konvertera Azure hanterade diskar lagring från standard toopremium och vice versa</span><span class="sxs-lookup"><span data-stu-id="06d9b-103">Convert Azure managed disks storage from standard toopremium, and vice versa</span></span>

<span data-ttu-id="06d9b-104">Hanterade diskar erbjuder två alternativ för lagring: [Premium](../../storage/storage-premium-storage.md) (SSD-baserad) och [Standard](../../storage/storage-standard-storage.md) (HDD-baserat).</span><span class="sxs-lookup"><span data-stu-id="06d9b-104">Managed disks offers two storage options: [Premium](../../storage/storage-premium-storage.md) (SSD-based) and [Standard](../../storage/storage-standard-storage.md) (HDD-based).</span></span> <span data-ttu-id="06d9b-105">Det gör att du tooeasily växla mellan hello två alternativ med minimal avbrottstid baserat på dina prestandabehov.</span><span class="sxs-lookup"><span data-stu-id="06d9b-105">It allows you tooeasily switch between hello two options with minimal downtime based on your performance needs.</span></span> <span data-ttu-id="06d9b-106">Den här funktionen är inte tillgänglig för ohanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="06d9b-106">This capability is not available for unmanaged disks.</span></span> <span data-ttu-id="06d9b-107">Men du kan enkelt [konvertera toomanaged diskar](convert-unmanaged-to-managed-disks.md) tooeasily växla mellan hello två alternativ.</span><span class="sxs-lookup"><span data-stu-id="06d9b-107">But you can easily [convert toomanaged disks](convert-unmanaged-to-managed-disks.md) tooeasily switch between hello two options.</span></span>

<span data-ttu-id="06d9b-108">Den här artikeln visar hur tooconvert hanterade diskar från standard toopremium och vice versa med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="06d9b-108">This article shows you how tooconvert managed disks from standard toopremium, and vice versa by using Azure CLI.</span></span> <span data-ttu-id="06d9b-109">Om du behöver tooinstall eller uppgradera den [installera Azure CLI 2.0](/cli/azure/install-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="06d9b-109">If you need tooinstall or upgrade it, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli.md).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="06d9b-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="06d9b-110">Before you begin</span></span>

* <span data-ttu-id="06d9b-111">konvertering av hello kräver en omstart av hello VM, så schemalägga hello migrering av diskar lagringen för en befintlig underhållsfönster.</span><span class="sxs-lookup"><span data-stu-id="06d9b-111">hello conversion requires a restart of hello VM, so schedule hello migration of your disks storage during a pre-existing maintenance window.</span></span> 
* <span data-ttu-id="06d9b-112">Om du använder ohanterade diskar först [konvertera toomanaged diskar](convert-unmanaged-to-managed-disks.md) toouse den här artikeln tooswitch mellan hello två alternativ för lagring.</span><span class="sxs-lookup"><span data-stu-id="06d9b-112">If you are using unmanaged disks, first [convert toomanaged disks](convert-unmanaged-to-managed-disks.md) toouse this article tooswitch between hello two storage options.</span></span> 


## <a name="convert-all-hello-managed-disks-of-a-vm-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="06d9b-113">Konvertera alla hello hanterade diskar på en virtuell dator från standard toopremium och vice versa</span><span class="sxs-lookup"><span data-stu-id="06d9b-113">Convert all hello managed disks of a VM from standard toopremium, and vice versa</span></span>

<span data-ttu-id="06d9b-114">I följande exempel hello, visar vi hur tooswitch alla hello diskar på en virtuell dator från standard toopremium lagring.</span><span class="sxs-lookup"><span data-stu-id="06d9b-114">In hello following example, we show how tooswitch all hello disks of a VM from standard toopremium storage.</span></span> <span data-ttu-id="06d9b-115">toouse premium hanterade diskar måste den virtuella datorn använda en [VM-storlek](sizes.md) som har stöd för premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="06d9b-115">toouse premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="06d9b-116">Det här exemplet växlar även tooa storlek som har stöd för premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="06d9b-116">This example also switches tooa size that supports premium storage.</span></span>

 ```azurecli

#resource group that contains hello virtual machine
rgName='yourResourceGroup'

#Name of hello virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from standard toopremium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate hello VM before changing hello size of hello VM
az vm deallocate --name $vmName --resource-group $rgName

#Change hello VM size tooa size that supports premium storage 
#Skip this step if converting storage from premium toostandard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update hello sku of all hello data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update hello sku of hello OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="convert-a-managed-disk-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="06d9b-117">Konverterar en hanterad disk från standard toopremium och vice versa</span><span class="sxs-lookup"><span data-stu-id="06d9b-117">Convert a managed disk from standard toopremium, and vice versa</span></span>

<span data-ttu-id="06d9b-118">Du kanske vill dina kostnader toohave blandning av standard-och premium tooreduce för arbetsbelastningen för utveckling och testning.</span><span class="sxs-lookup"><span data-stu-id="06d9b-118">For your dev/test workload, you may want toohave mixture of standard and premium disks tooreduce your cost.</span></span> <span data-ttu-id="06d9b-119">Du kan göra det genom att uppgradera toopremium lagring, endast hello-diskar som kräver bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="06d9b-119">You can accomplish it by upgrading toopremium storage, only hello disks that require better performance.</span></span> <span data-ttu-id="06d9b-120">I följande exempel hello, visar vi hur tooswitch en enda disk av en virtuell dator från standard toopremium lagring och vice versa.</span><span class="sxs-lookup"><span data-stu-id="06d9b-120">In hello following example, we show how tooswitch a single disk of a VM from standard toopremium storage, and vice versa.</span></span> <span data-ttu-id="06d9b-121">toouse premium hanterade diskar måste den virtuella datorn använda en [VM-storlek](sizes.md) som har stöd för premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="06d9b-121">toouse premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="06d9b-122">Det här exemplet växlar även tooa storlek som har stöd för premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="06d9b-122">This example also switches tooa size that supports premium storage.</span></span>

 ```azurecli

#resource group that contains hello managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from standard toopremium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get hello parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate hello VM before changing hello size of hello VM
az vm deallocate --ids $vmId 

#Change hello VM size tooa size that supports premium storage 
#Skip this step if converting storage from premium toostandard
az vm resize --ids $vmId --size $size

# Update hello sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="next-steps"></a><span data-ttu-id="06d9b-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="06d9b-123">Next steps</span></span>

<span data-ttu-id="06d9b-124">Ta en skrivskyddad kopia av en virtuell dator med hjälp av [ögonblicksbilder](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="06d9b-124">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

