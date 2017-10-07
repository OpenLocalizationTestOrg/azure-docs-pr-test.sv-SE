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
# <a name="convert-azure-managed-disks-storage-from-standard-toopremium-and-vice-versa"></a>Konvertera Azure hanterade diskar lagring från standard toopremium och vice versa

Hanterade diskar erbjuder två alternativ för lagring: [Premium](../../storage/storage-premium-storage.md) (SSD-baserad) och [Standard](../../storage/storage-standard-storage.md) (HDD-baserat). Det gör att du tooeasily växla mellan hello två alternativ med minimal avbrottstid baserat på dina prestandabehov. Den här funktionen är inte tillgänglig för ohanterade diskar. Men du kan enkelt [konvertera toomanaged diskar](convert-unmanaged-to-managed-disks.md) tooeasily växla mellan hello två alternativ.

Den här artikeln visar hur tooconvert hanterade diskar från standard toopremium och vice versa med hjälp av Azure CLI. Om du behöver tooinstall eller uppgradera den [installera Azure CLI 2.0](/cli/azure/install-azure-cli.md). 

## <a name="before-you-begin"></a>Innan du börjar

* konvertering av hello kräver en omstart av hello VM, så schemalägga hello migrering av diskar lagringen för en befintlig underhållsfönster. 
* Om du använder ohanterade diskar först [konvertera toomanaged diskar](convert-unmanaged-to-managed-disks.md) toouse den här artikeln tooswitch mellan hello två alternativ för lagring. 


## <a name="convert-all-hello-managed-disks-of-a-vm-from-standard-toopremium-and-vice-versa"></a>Konvertera alla hello hanterade diskar på en virtuell dator från standard toopremium och vice versa

I följande exempel hello, visar vi hur tooswitch alla hello diskar på en virtuell dator från standard toopremium lagring. toouse premium hanterade diskar måste den virtuella datorn använda en [VM-storlek](sizes.md) som har stöd för premium-lagring. Det här exemplet växlar även tooa storlek som har stöd för premium-lagring.

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
## <a name="convert-a-managed-disk-from-standard-toopremium-and-vice-versa"></a>Konverterar en hanterad disk från standard toopremium och vice versa

Du kanske vill dina kostnader toohave blandning av standard-och premium tooreduce för arbetsbelastningen för utveckling och testning. Du kan göra det genom att uppgradera toopremium lagring, endast hello-diskar som kräver bättre prestanda. I följande exempel hello, visar vi hur tooswitch en enda disk av en virtuell dator från standard toopremium lagring och vice versa. toouse premium hanterade diskar måste den virtuella datorn använda en [VM-storlek](sizes.md) som har stöd för premium-lagring. Det här exemplet växlar även tooa storlek som har stöd för premium-lagring.

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

## <a name="next-steps"></a>Nästa steg

Ta en skrivskyddad kopia av en virtuell dator med hjälp av [ögonblicksbilder](snapshot-copy-managed-disk.md).

