---
title: aaaManage Azure diskar med hello Azure CLI | Microsoft Docs
description: "Självstudiekurs – hantera Azure-diskarna med hello Azure CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: ad29cfbec5f9738f47705b19d0450c9a2f555492
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-disks-with-hello-azure-cli"></a>Hantera Azure-diskarna med hello Azure CLI

Azure virtual machines använder diskar toostore hello virtuella datorer, operativsystem, program och data. När du skapar en virtuell dator är det viktigt toochoose storleken för en disk och konfiguration lämpliga toohello förväntades arbetsbelastning. Den här kursen ingår distribuerar och hanterar Virtuella diskar. Du lär dig mer om:

> [!div class="checklist"]
> * OS-diskar och tillfällig diskar
> * Datadiskar
> * Standard- och Premium-diskar
> * Diskprestanda
> * Koppla och förbereda datadiskar
> * Ändra storlek på diskar
> * Diskbilder


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="default-azure-disks"></a>Standard Azure-diskar

När en virtuell dator i Azure skapas är två diskar automatiskt bifogade toohello virtuella datorn. 

**Operativsystemdisken** - operativsystemet diskar kan storleksändras in too1 terabyte och värdar hello operativsystemet för virtuella datorer. hello OS-disken är märkt */dev/sda* som standard. hello disk cachelagring konfigurationen av hello OS-disken har optimerats för OS-prestanda. Hej OS-disken på grund av den här konfigurationen **bör inte** värd för program eller data. Använd datadiskar som beskrivs senare i den här artikeln för program och data. 

**Diskutrymme** -tillfälliga diskar använder ett SSD-enhet som finns på hello samma Azure-värd som hello VM. Temporär diskar har hög performant och kan användas för åtgärder som till exempel temporär databearbetning. Om hello VM är flyttade tooa nya värden kan bort data som lagrats på en tillfällig disk. hello hello tillfälliga diskens storlek bestäms av hello VM-storlek. Tillfällig diskar är märkta */dev/sdb* och har en monteringspunkt för */mnt*.

### <a name="temporary-disk-sizes"></a>Tillfällig diskstorlekar

| Typ | VM-storlek | Maxstorlek för temporär disk (GB) |
|----|----|----|
| [Generellt syfte](sizes-general.md) | A och D-serien | 800 |
| [Beräkningsoptimerad](sizes-compute.md) | F-serien | 800 |
| [Minnesoptimerad](../virtual-machines-windows-sizes-memory.md) | D och G serien | 6144 |
| [Lagringsoptimerad](../virtual-machines-windows-sizes-storage.md) | Serie L | 5630 |
| [GPU](sizes-gpu.md) | N serien | 1440 |
| [Hög prestanda](sizes-hpc.md) | A och H serien | 2000 |

## <a name="azure-data-disks"></a>Azure datadiskar

Ytterligare datadiskar kan läggas till för att installera program och lagra data. Datadiskar som ska användas i en situation där varaktiga och känslig datalagring önskas. Varje datadisk har en maximal kapacitet för 1 terabyte. hello storleken på hello virtuella datorn anger hur många datadiskar kan vara anslutna tooa VM. För varje VM-core kan två diskar kopplas. 

### <a name="max-data-disks-per-vm"></a>Maximalt antal datadiskar per VM

| Typ | VM-storlek | Maximalt antal datadiskar per VM |
|----|----|----|
| [Generellt syfte](sizes-general.md) | A och D-serien | 32 |
| [Beräkningsoptimerad](sizes-compute.md) | F-serien | 32 |
| [Minnesoptimerad](../virtual-machines-windows-sizes-memory.md) | D och G serien | 64 |
| [Lagringsoptimerad](../virtual-machines-windows-sizes-storage.md) | Serie L | 64 |
| [GPU](sizes-gpu.md) | N serien | 48 |
| [Hög prestanda](sizes-hpc.md) | A och H serien | 32 |

## <a name="vm-disk-types"></a>VM-disktyper

Azure tillhandahåller två typer av disk.

### <a name="standard-disk"></a>Standard disk

Standard Storage stöds av hårddiskar och levererar kostnadseffektiv lagring samtidigt som det är högpresterande. Standarddiskar är idealiskt för ett kostnadseffektivt dev och testa arbetsbelastning.

### <a name="premium-disk"></a>Premium-disk

Premiumdiskar backas upp av SSD-baserad hög prestanda, låg latens disk. Perfekt för virtuella datorer som kör produktion arbetsbelastning. Premium-lagring stöder DS-serien, DSv2-serien GS-serien och FS-serien virtuella datorer. Premiumdiskar finns i tre olika typer (P10 P20, P30), hello hello diskens storlek avgör hello disktyp. När du väljer, avrundat ett värde för disk hello toohello nästa typen. Om hello diskens storlek är mindre än 128 GB, är hello disktypen P10. Om hello diskens storlek är mellan 129 och 512 GB, är hello storleken en P20. Något över 512 GB hello storlek är en P30.

### <a name="premium-disk-performance"></a>Premium-diskprestanda

|Premium-lagring disktyp | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Diskens storlek (avrunda uppåt) | 128 GB | 512 GB | 1 024 GB (1 TB) |
| Högsta IOPS per disk | 500 | 2,300 | 5,000 |
Dataflöde per disk | 100 MB/s | 150 MB/s | 200 MB/s |

Medan hello ovanför tabell identifierar högsta IOPS per disk, kan en högre nivå av prestanda uppnås genom striping flera datadiskar. Exempelvis kan en virtuell dator Standard_GS5 uppnå högst 80 000 IOPS. Detaljerad information om högsta IOPS per VM finns [Linux VM-storlekar](sizes.md).

## <a name="create-and-attach-disks"></a>Skapa och koppla diskar

Datadiskar kan skapas och kopplade på VM-tiden för skapandet eller tooan befintliga VM.

### <a name="attach-disk-at-vm-creation"></a>Bifoga disken på Skapa en virtuell dator

Skapa en resursgrupp med hello [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) kommando. 

```azurecli-interactive 
az group create --name myResourceGroupDisk --location eastus
```

Skapa en virtuell dator med hjälp av hello [az vm skapa]( /cli/azure/vm#create) kommando. Hej `--datadisk-sizes-gb` argumentet är används toospecify att ytterligare en disk som ska skapas och bifogas toohello virtuella datorn. toocreate och koppla mer än en disk, Använd en mellanslags-avgränsad lista med värden för disk-storlek. I följande exempel hello, skapas en virtuell dator med två diskar, både 128 GB. Eftersom hello storlekar för diskar är 128 GB, diskarna båda är konfigurerade som P10s som ger högsta 500 IOPS per disk.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --data-disk-sizes-gb 128 128 \
  --generate-ssh-keys
```

### <a name="attach-disk-tooexisting-vm"></a>Bifoga disken tooexisting VM

toocreate och koppla en ny disk tooan befintlig virtuell dator kan använda hello [az vm disk bifoga](/cli/azure/vm/disk#attach) kommando. hello följande exempel skapar en premium disk, 128 GB i storlek, och bifogar den toohello skapas den virtuella datorn i hello sista steget.

```azurecli-interactive 
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a>Förbereda datadiskar

När en disk ansluten toohello virtuell dator, måste hello operativsystemet toobe konfigurerats toouse hello disk. hello som följande exempel visar hur toomanually konfigurera en disk. Den här processen kan också automatiseras med hjälp av molnet initiering, som beskrivs i en [senare kursen](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Manuell konfiguration

Skapa en SSH-anslutning med hello virtuell dator. Ersätt hello IP-adressen med hello offentliga IP-Adressen för hello virtuell dator.

```azurecli-interactive 
ssh 52.174.34.95
```

Hej partitionsdisk med `fdisk`.

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

Skriva toohello en filsystempartition med hjälp av hello `mkfs` kommando.

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Montera hello ny disk så att den är tillgänglig i hello-operativsystemet.

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

hello disk nu kan nås via hello *datadrive* monteringspunkt, som kan verifieras genom att köra hello `df -h` kommando. 

```bash
df -h
```

hello utdata visar hello nya enheten monteras på */datadrive*.

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

tooensure som hello enheten är på nytt efter en omstart, måste det läggas toohello */etc/fstab* fil. toodo så få hello UUID för hello disk med hello `blkid` verktyget.

```bash
sudo -i blkid
```

hello utdata visar hello UUID hello enhetens `/dev/sdc1` i det här fallet.

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

Lägg till en rad liknande toohello följande toohello */etc/fstab* fil. Även Observera att skriva barriärer kan inaktiveras med *barriären = 0*, den här konfigurationen kan förbättra diskprestanda. 

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail,barrier=0   1  2
```

Nu när hello disk har konfigurerats, Stäng hello SSH-session.

```bash
exit
```

## <a name="resize-vm-disk"></a>Ändra storlek på virtuell disk

När en virtuell dator har distribuerats, ökas hello operativsystemdisk eller eventuella anslutna hårddiskar i storlek. Det är bra att öka hello storleken på en disk när du behöver mer lagringsutrymme eller högre prestanda (P10, P20, P30). Observera att diskar inte kan minskas i storlek.

Innan du ökar diskstorleken hello Id eller namnet på hello disk behövs. Använd hello [az Disklista](/cli/azure/vm/disk#list) kommandot tooreturn alla diskar i en resursgrupp. Anteckna hello diskens namn som du vill att tooresize.

```azurecli-interactive 
az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

hello VM måste också vara frigjord. Använd hello [az vm frigöra]( /cli/azure/vm#deallocate) kommando toostop och frigöra hello VM.

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

Använd hello [az disk uppdatering](/cli/azure/vm/disk#update) kommandot tooresize hello disk. Det här exemplet ändrar storlek på en disk med namnet *myDataDisk* too1 terabyte.

```azurecli-interactive 
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

Starta hello VM när hello storleksändring åtgärden har slutförts.

```azurecli-interactive 
az vm start --resource-group myResourceGroupDisk --name myVM
```

Om du har ändrat storlek hello operativsystem systemdisken utökas automatiskt hello partition. Om du har ändrat storlek en datadisk, måste alla aktuella partitioner toobe expanderas i hello operativsystemet för virtuella datorer.

## <a name="snapshot-azure-disks"></a>Ögonblicksbild Azure-diskar

Ta en ögonblicksbild av disk skapar en skrivskyddad, men i tidpunkt kopia av hello disk. Azure VM ögonblicksbilder är användbara för att snabbt spara hello tillståndet för en virtuell dator innan du gör ändringar i konfigurationen. I händelse av hello hello konfigurationsändringar bevisa toobe områdena, tillstånd för virtuell dator kan återställas med hello ögonblicksbild. När en virtuell dator har mer än en disk, en ögonblicksbild tas för varje disk oberoende av hello andra. Överväg att stoppa hello VM innan du tar diskbilder för säkerhetskopiering programmet konsekvent. Du kan också använda hello [Azure Backup-tjänsten](/azure/backup/), vilket aktiverar du tooperform automatisk säkerhetskopior vid hello virtuella datorn körs.

### <a name="create-snapshot"></a>Skapa en ögonblicksbild

Innan du skapar en virtuell disk ögonblicksbild krävs hello-Id eller namn för hello disk. Använd hello [az vm visa](https://docs.microsoft.com/en-us/cli/azure/vm#show) kommandot tooreturn hello disk-id. I det här exemplet lagras hello disk-id i en variabel så att den kan användas i ett senare steg.

```azurecli-interactive 
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Nu när du har hello-id för hello virtuella disken hello följande kommando skapar en ögonblicksbild av hello disk.

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>Skapa disk från en ögonblicksbild

Den här ögonblicksbilden kan sedan konverteras till en disk, vilket används toorecreate hello virtuell dator.

```azurecli-interactive 
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>Återställa en virtuell dator från en ögonblicksbild

toodemonstrate återställning av virtuell dator, ta bort hello befintlig virtuell dator. 

```azurecli-interactive 
az vm delete --resource-group myResourceGroupDisk --name myVM
```

Skapa en ny virtuell dator från hello ögonblicksbild disken.

```azurecli-interactive 
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a>Återanslut datadisk

Alla datadiskar måste toobe anbringas på nytt toohello virtuella datorn.

Hitta först hello data diskens namn med hello [az Disklista](https://docs.microsoft.com/cli/azure/disk#list) kommando. Det här exemplet platser hello namnet på hello disk i en variabel med namnet *datadisk*, som används i hello nästa steg.

```azurecli-interactive 
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

Använd hello [az vm disk bifoga](https://docs.microsoft.com/cli/azure/vm/disk#attach) kommandot tooattach hello disk.

```azurecli-interactive 
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a>Nästa steg

I kursen får du lärt dig om Virtuella diskar ämnen som:

> [!div class="checklist"]
> * OS-diskar och tillfällig diskar
> * Datadiskar
> * Standard- och Premium-diskar
> * Diskprestanda
> * Koppla och förbereda datadiskar
> * Ändra storlek på diskar
> * Diskbilder

Avancera toohello nästa självstudiekurs toolearn om hur du automatiserar VM-konfiguration.

> [!div class="nextstepaction"]
> [Automatisera konfiguration av virtuella datorer](./tutorial-automate-vm-deployment.md)
