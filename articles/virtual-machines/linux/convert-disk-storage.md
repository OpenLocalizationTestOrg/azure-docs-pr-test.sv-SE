---
title: "Konvertera Azure hanterade diskar lagring från standard till premium, och vice versa | Microsoft Docs"
description: "Så här konverterar du Azure hanterade diskar lagring från standard till premium och vice versa med hjälp av Azure CLI."
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
ms.openlocfilehash: 0380b4aaa23b4aaba4c67d05e2d62f3ef41d6a32
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a><span data-ttu-id="24a21-103">Konvertera Azure hanterade diskar lagring från standard till premium, och vice versa</span><span class="sxs-lookup"><span data-stu-id="24a21-103">Convert Azure managed disks storage from standard to premium, and vice versa</span></span>

<span data-ttu-id="24a21-104">Hanterade diskar erbjuder två alternativ för lagring: [Premium](../../storage/storage-premium-storage.md) (SSD-baserad) och [Standard](../../storage/storage-standard-storage.md) (HDD-baserat).</span><span class="sxs-lookup"><span data-stu-id="24a21-104">Managed disks offers two storage options: [Premium](../../storage/storage-premium-storage.md) (SSD-based) and [Standard](../../storage/storage-standard-storage.md) (HDD-based).</span></span> <span data-ttu-id="24a21-105">På så sätt kan du enkelt växla mellan de två alternativen med minimal avbrottstid baserat på dina prestandabehov.</span><span class="sxs-lookup"><span data-stu-id="24a21-105">It allows you to easily switch between the two options with minimal downtime based on your performance needs.</span></span> <span data-ttu-id="24a21-106">Den här funktionen är inte tillgänglig för ohanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="24a21-106">This capability is not available for unmanaged disks.</span></span> <span data-ttu-id="24a21-107">Men du kan enkelt [konvertera till hanterade diskar](convert-unmanaged-to-managed-disks.md) enkelt växla mellan de två alternativen.</span><span class="sxs-lookup"><span data-stu-id="24a21-107">But you can easily [convert to managed disks](convert-unmanaged-to-managed-disks.md) to easily switch between the two options.</span></span>

<span data-ttu-id="24a21-108">Den här artikeln visar hur du konverterar hanterade diskar från standard till premium och vice versa med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="24a21-108">This article shows you how to convert managed disks from standard to premium, and vice versa by using Azure CLI.</span></span> <span data-ttu-id="24a21-109">Om du behöver installera eller uppgradera den, se [installera Azure CLI 2.0](/cli/azure/install-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="24a21-109">If you need to install or upgrade it, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli.md).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="24a21-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="24a21-110">Before you begin</span></span>

* <span data-ttu-id="24a21-111">Konverteringen kräver en omstart av den virtuella datorn, så Schemalägg migrering av lagringsenheterna diskar under en befintlig underhållsperiod.</span><span class="sxs-lookup"><span data-stu-id="24a21-111">The conversion requires a restart of the VM, so schedule the migration of your disks storage during a pre-existing maintenance window.</span></span> 
* <span data-ttu-id="24a21-112">Om du använder ohanterade diskar först [konvertera till hanterade diskar](convert-unmanaged-to-managed-disks.md) för att växla mellan två alternativ för lagring med hjälp av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="24a21-112">If you are using unmanaged disks, first [convert to managed disks](convert-unmanaged-to-managed-disks.md) to use this article to switch between the two storage options.</span></span> 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a><span data-ttu-id="24a21-113">Konvertera alla hanterade diskar på en virtuell dator från standard till premium, och vice versa</span><span class="sxs-lookup"><span data-stu-id="24a21-113">Convert all the managed disks of a VM from standard to premium, and vice versa</span></span>

<span data-ttu-id="24a21-114">I följande exempel visar vi hur du växla alla diskar på en virtuell dator från standard till premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="24a21-114">In the following example, we show how to switch all the disks of a VM from standard to premium storage.</span></span> <span data-ttu-id="24a21-115">Du använder premiumdiskar hanteras genom den virtuella datorn måste använda en [VM-storlek](sizes.md) som har stöd för premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="24a21-115">To use premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="24a21-116">Det här exemplet växlar också till en storlek som har stöd för premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="24a21-116">This example also switches to a size that supports premium storage.</span></span>

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the sku of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the sku of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a><span data-ttu-id="24a21-117">Konverterar en hanterad disk från standard till premium, och vice versa</span><span class="sxs-lookup"><span data-stu-id="24a21-117">Convert a managed disk from standard to premium, and vice versa</span></span>

<span data-ttu-id="24a21-118">För din arbetsbelastning för utveckling och testning, kanske du vill ha blandning av standard och premium diskar för att minska dina kostnader.</span><span class="sxs-lookup"><span data-stu-id="24a21-118">For your dev/test workload, you may want to have mixture of standard and premium disks to reduce your cost.</span></span> <span data-ttu-id="24a21-119">Du kan göra det genom att uppgradera till premium-lagring på diskar som kräver bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="24a21-119">You can accomplish it by upgrading to premium storage, only the disks that require better performance.</span></span> <span data-ttu-id="24a21-120">I följande exempel visar vi hur du växlar en enda disk av en virtuell dator från standard till premium-lagring och vice versa.</span><span class="sxs-lookup"><span data-stu-id="24a21-120">In the following example, we show how to switch a single disk of a VM from standard to premium storage, and vice versa.</span></span> <span data-ttu-id="24a21-121">Du använder premiumdiskar hanteras genom den virtuella datorn måste använda en [VM-storlek](sizes.md) som har stöd för premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="24a21-121">To use premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="24a21-122">Det här exemplet växlar också till en storlek som har stöd för premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="24a21-122">This example also switches to a size that supports premium storage.</span></span>

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --ids $vmId --size $size

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="next-steps"></a><span data-ttu-id="24a21-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24a21-123">Next steps</span></span>

<span data-ttu-id="24a21-124">Ta en skrivskyddad kopia av en virtuell dator med hjälp av [ögonblicksbilder](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="24a21-124">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

