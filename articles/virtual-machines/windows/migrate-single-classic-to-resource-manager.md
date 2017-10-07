---
title: aaaMigrate klassiska VM-tooan ARM hanteras Disk VM | Microsoft Docs
description: "Migrera en enda Azure-VM från hello klassisk distribution modellen tooManaged diskar i hello Resource Manager-distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: d8c4b9431f5dd8a071fcbc2ee36581a33f76ba62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manually-migrate-a-classic-vm-tooa-new-arm-managed-disk-vm-from-hello-vhd"></a>Migrera en klassiska Virtuella tooa manuellt ny ARM hanteras Disk virtuell dator från hello VHD 


Det här avsnittet hjälper du toomigrate dina befintliga virtuella Azure-datorer från hello klassiska distributionsmodellen för[hanterade diskar](managed-disks-overview.md) i hello Resource Manager-distributionsmodellen.


## <a name="plan-for-hello-migration-toomanaged-disks"></a>Planera för migrering av hello tooManaged diskar

Det här avsnittet hjälper dig att toomake hello bästa beslut på VM- och diskresurser typer.


### <a name="location"></a>Plats

Välj en plats där Azure hanterade diskar är tillgängliga. Om du migrerar tooPremium hanterade diskar kan också se till att Premium-lagring finns i hello region där du planerar toomigrate till. Se [Azure Services byRegion](https://azure.microsoft.com/regions/#services) uppdaterad information om tillgängliga platser.

### <a name="vm-sizes"></a>VM-storlekar

Om du migrerar tooPremium hanterade diskar har tooupdate hello storleken på hello VM tooPremium kompatibla lagringsstorlek tillgängliga i hello-regionen där VM finns. Granska hello VM-storlekar som är Premium-lagring som är kompatibla. hello Azure VM storlek specifikationer listas i [storlekar för virtuella datorer](sizes.md).
Granska hello prestandaegenskaper för virtuella datorer som fungerar med Premium-lagring och välj hello lämpligaste VM-storlek som bäst passar din arbetsbelastning. Kontrollera att det finns tillräckligt med bandbredd på VM toodrive hello disk trafiken.

### <a name="disk-sizes"></a>Diskstorlekar

**Premium hanterade diskar**

Det finns sju typer av premium hanterade diskar som kan användas med den virtuella datorn var och en har särskilda IOPs och genomströmning gränser. Överväg att dessa begränsningar när du väljer hello Premium disktyp för den virtuella datorn baserat på hello behoven för ditt program vad gäller kapacitet, prestanda, skalbarhet och belastning läses in.

| Premium diskar typ  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Diskstorlek           | 128 GB| 512 GB| 128 GB| 512 GB            | 1 024 GB (1 TB)    | 2 048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPS per disk       | 120   | 240   | 500   | 2 300              | 5000              | 7500              | 7500              | 
| Dataflöde per disk | 25 MB per sekund  | 50 MB per sekund  | 100 MB per sekund | 150 MB per sekund | 200 MB per sekund | 250 MB per sekund | 250 MB per sekund | 

**Hanterade standarddiskar**

Det finns sju typer av Standard hanterade diskar som kan användas med den virtuella datorn. Var och en av dem har olika kapacitet men samma IOPS och genomströmning gränser. Välj hello typ av Standard hanterade diskar baserat på hello kapacitetsbehov för programmet.

| Disk av standardtyp  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Diskstorlek           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1 024 GB (1 TB)   | 2 048 GB (2TB)    | 4095 GB (4 TB)   | 
| IOPS per disk       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Dataflöde per disk | 60 MB per sekund | 60 MB per sekund | 60 MB per sekund | 60 MB per sekund | 60 MB per sekund | 60 MB per sekund | 60 MB per sekund | 


### <a name="disk-caching-policy"></a>Princip för cachelagring av disk 

**Premium hanterade diskar**

Princip för cachelagring av disk är som standard *skrivskyddad* för alla hello Premium datadiskar och *Read-Write* för hello Premium operativsystemdisken kopplade toohello VM. Den här konfigurationen rekommenderas tooachieve hello optimala prestanda för ditt program IOs. Inaktivera cachelagring på disk så att du kan få bättre prestanda för programmet för skrivintensiv eller lässkyddad datadiskar (till exempel loggfiler för SQL Server).

### <a name="pricing"></a>Prissättning

Granska hello [priser för hanterade diskar](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Priser för hanterade Premiumdiskar är samma som hello ohanterade Premiumdiskar. Men priser för hanterade standarddiskar skiljer sig från ohanterade standarddiskar.


## <a name="checklist"></a>Checklista

1.  Om du migrerar tooPremium hanterade diskar, kontrollera att den är tillgänglig i hello region du migrerar till.

2.  Bestäm hello ny virtuell dator-serie du kommer att använda. Det bör vara en Premium-lagring kan om du migrerar tooPremium hanterade diskar.

3.  Bestäm hello exakt VM-storlek du vill använda som är tillgängliga i hello region du migrerar till. VM-storlek måste toobe tillräckligt stor toosupport hello antalet datadiskar som du har. Om du har fyra datadiskar måste hello VM ha två eller flera kärnor. Du kan också processorkraft, minne och nätverksbandbredd måste.

4.  Ha hello aktuella VM information praktisk, inklusive hello lista över diskar och motsvarande VHD-blobbar.

Förbered ditt program för driftstopp. toodo en ren migrering, har du toostop all hello bearbetning i hello nuvarande systemet. Först då kan du få tooconsistent tillstånd som du kan migrera nya toohello-plattformen. Varaktighet för stilleståndstid är beroende av hello mängden data i hello diskar toomigrate.


## <a name="migrate-hello-vm"></a>Migrera hello VM

Förbered ditt program för driftstopp. toodo en ren migrering, har du toostop all hello bearbetning i hello nuvarande systemet. Först då kan du få tooconsistent tillstånd som du kan migrera nya toohello-plattformen. Varaktighet för stilleståndstid beror hello mängden data i hello diskar toomigrate.


1.  Ange först hello gemensamma parametrar:

    ```powershell
    $resourceGroupName = 'yourResourceGroupName'
    
    $location = 'your location' 
    
    $virtualNetworkName = 'yourExistingVirtualNetworkName'
    
    $virtualMachineName = 'yourVMName'
    
    $virtualMachineSize = 'Standard_DS3'
    
    $adminUserName = "youradminusername"
    
    $adminPassword = "yourpassword" | ConvertTo-SecureString -AsPlainText -Force
    
    $imageName = 'yourImageName'
    
    $osVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd'
    
    $dataVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk1.vhd'
    
    $dataDiskName = 'dataDisk1'
    ```

2.  Skapa en hanterad OS-disk hello VHD från hello klassiska VM.

    Se till att du har tillhandahållit hello slutföra hello OS VHD toohello $osVhdUri parameter-URI. Ange dessutom **- AccountType** som **PremiumLRS** eller **StandardLRS** baserat på typ av diskar (Standard eller Premium) du migrerar till.

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $osVhdUri) '
    -ResourceGroupName $resourceGroupName
    ```

3.  Koppla hello OS disk toohello ny virtuell dator.

    ```powershell
    $VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $osDisk.Id '
    -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

4.  Skapa en hanterad datadisk från hello data VHD-filen och Lägg till den toohello ny virtuell dator.

    ```powershell
    $dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreationDataCreateOption Import '
    -SourceUri $dataVhdUri ) -ResourceGroupName $resourceGroupName
    
    $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName '
    -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
    ```

5.  Skapa hello ny virtuell dator genom att ange offentlig IP, virtuella nätverk och nätverkskort.

    ```powershell
    $publicIp = New-AzureRmPublicIpAddress -Name ($VirtualMachineName.ToLower()+'_ip') '
    -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
    
    $vnet = Get-AzureRmVirtualNetwork -Name $virtualNetworkName -ResourceGroupName $resourceGroupName
    
    $nic = New-AzureRmNetworkInterface -Name ($VirtualMachineName.ToLower()+'_nic') '
    -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id '
    -PublicIpAddressId $publicIp.Id
    
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nic.Id
    
    New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $resourceGroupName -Location $location
    ```

> [!NOTE]
>Det kan finnas ytterligare steg krävs toosupport ditt program som inte omfattas av den här guiden.
>
>

## <a name="next-steps"></a>Nästa steg

- Ansluta toohello virtuella datorn. Instruktioner finns i [hur tooconnect och logga in tooan Azure virtuella datorn kör Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

