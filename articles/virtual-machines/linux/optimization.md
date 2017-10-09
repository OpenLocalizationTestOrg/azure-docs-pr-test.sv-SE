---
title: aaaOptimize din Linux VM i Azure | Microsoft Docs
description: "Lär dig vissa optimering tips toomake att du har lagt upp ditt Linux VM för optimala prestanda på Azure"
keywords: Linux-dator, virtuella linux ubuntu virtuell dator
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 8baa30c8-d40e-41ac-93d0-74e96fe18d4c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: rclaus
ms.openlocfilehash: 89a9ac022928a2801a9a15e1c172340352745354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-linux-vm-on-azure"></a>Optimera din virtuella Linux-dator på Azure
Att skapa en Linux-dator (VM) är enkelt toodo från kommandoraden hello eller hello-portalen. Den här kursen visar hur tooensure du har ställt in toooptimize dess prestanda på hello Microsoft Azure-plattformen. Det här avsnittet använder en virtuell Ubuntu Server-dator, men du kan också skapa Linux virtuella datorer med hjälp av [egna avbildningar som mallar](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="prerequisites"></a>Krav
Det här avsnittet förutsätter att du redan har ett aktivt Azure-prenumeration ([gratis utvärderingsversion registrering](https://azure.microsoft.com/pricing/free-trial/)) och redan har etablerat en virtuell dator i din Azure-prenumeration. Se till att du har hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad tooyour Azure-prenumeration med [az inloggningen](/cli/azure/#login) innan du [skapa en virtuell dator](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-os-disk"></a>Azure OS-Disk
När du skapar en Linux VM i Azure har två diskar som är kopplade till den. **/ dev/sda** är OS-disken **/dev/sdb** är tillfällig disken.  Använd inte hello huvudsakliga OS-disken (**/dev/sda**) för något annat än hello operativsystemet som den är optimerad för snabb VM starttiden och ger inte bra prestanda för din arbetsbelastning. Du vill tooattach en eller flera diskar tooyour VM tooget beständiga och optimerad lagring för dina data. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Lägga till diskar för mål storlek och prestanda
Baserat på hello VM-storlek, kan du koppla upp too16 ytterligare diskar på en A-Series, 32 diskar på D-serien och 64 diskar på en dator G-serien - varje in too1 TB i storlek. Du kan lägga till extra diskar som behövs per utrymme krav på och IOps. Varje disk har ett mål för prestanda för 500 IOps standardlagring och uppåt too5000 IOps per disk för Premium-lagring.  Läs mer om Premium-lagring diskar [Premium-lagring: högpresterande lagring för virtuella Azure-datorer](../../storage/common/storage-premium-storage.md)

tooachieve hello högsta IOps på Premium-lagring diskar där cacheinställningarna har ställts in tooeither **ReadOnly** eller **ingen**, måste du inaktivera **barriärer** medan Montera hello file system i Linux. Du behöver inte barriärer eftersom hello skriver tooPremium lagring säkerhetskopieras diskar är beständig för dessa inställningar för cachelagring.

* Om du använder **reiserFS**, inaktivera barriärer hello montera alternativet `barrier=none` (Använd för att aktivera barriärer `barrier=flush`)
* Om du använder **ext3/ext4**, inaktivera barriärer hello montera alternativet `barrier=0` (Använd för att aktivera barriärer `barrier=1`)
* Om du använder **XFS**, inaktivera barriärer hello montera alternativet `nobarrier` (för att aktivera barriärer alternativet hello `barrier`)

## <a name="unmanaged-storage-account-considerations"></a>Ohanterad lagring överväganden för användarkonton
hello standardåtgärden när du skapar en virtuell dator med hello Azure CLI 2.0 är toouse Azure hanterade diskar.  Diskarna som hanteras av hello Azure-plattformen och kräver inte någon förberedelse eller plats toostore dem.  Ohanterad diskar krävs ett lagringskonto och har vissa ytterligare prestandaöverväganden.  Mer information om hanterade diskar finns i [Översikt över Azure Managed Disks](../windows/managed-disks-overview.md).  hello följande avsnitt beskrivs prestandaöverväganden bara när du använder ohanterade diskar.  Hej igen, standard och rekommenderade lagringslösning är toouse hanterade diskar.

Om du skapar en virtuell dator med ohanterad diskar, se till att du bifoga diskar från storage-konton som finns i hello samma region som din Virtuella tooensure närhet och minimerar Nätverksfördröjningen.  Varje standardlagringskonto har högst 20 k IOps och kapaciteten 500 TB storlek.  Den här gränsen fungerar tooapproximately 40 belastat diskar inklusive hello OS-disk- och eventuella hårddiskar som du skapar. Det finns ingen gräns för högsta IOps för Premium Storage-konton, men det finns en storleksgräns för 32 TB. 

När med hög IOps arbetsbelastningar och du har valt standardlagring för diskarna kan behöva toosplit hello diskar över flera storage-konton toomake säker på att du inte har nått hello 20 000 IOps begränsa för Standard Storage-konton. Den virtuella datorn kan innehålla en blandning av diskar från över olika lagringskonton och storage-konto typer tooachieve optimala konfigurationen.
 

## <a name="your-vm-temporary-drive"></a>Den tillfälliga Virtuella enheten
Som standard när du skapar en virtuell dator Azure ger dig en OS-disk (**/dev/sda**) och en tillfällig disk (**/dev/sdb**).  Alla ytterligare diskar som du lägger till visa upp **/dev/sdc**, **/dev/sdd**, **/dev/sde** och så vidare. Alla data på hårddisken tillfälliga (**/dev/sdb**) är inte beständiga och kan gå förlorad om specifika händelser som VM storleksändring Omdistributionen, eller underhåll tvingar fram en omstart av den virtuella datorn.  hello storlek och typ av tillfälliga disken är relaterade toohello VM-storlek som du väljer vid tidpunkten för distribution. Alla hello premium storlek virtuella datorer (DS, G och DS_V2-serien) hello temporärkatalog backas upp av en lokal SSD för ytterligare prestanda för in too48k IOps. 

## <a name="linux-swap-file"></a>Växlingsfil för Linux
Om din Azure VM är från en Ubuntu eller virtuell CoreOS-avbildning, kan du använda CustomData toosend en moln-config toocloud initiering. Om du [överföra en anpassad avbildning Linux](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) som använder molnet init, du också konfigurera växlingen partitioner som använder molnet initiering.

På Ubuntu molnet bilder, måste du använda molnet init tooconfigure hello växlingen partition. Mer information finns i [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

För bilder utan stöd för molnet init har VM-avbildningar som distribueras från hello Azure Marketplace en virtuell Linux-agenten integrerad med hello OS. Den här agenten kan hello VM toointeract med olika Azure-tjänster. Under förutsättning att du har distribuerat en standardiserad avbildning från hello Azure Marketplace, behöver du toodo konfigurera hello följande toocorrectly Linux växlingsfilens inställningar:

Leta upp och ändra två poster i hello **/etc/waagent.conf** fil. De styr hello förekomsten av en dedikerad växlingsfil och storleken på växlingsfilen hello. hello-parametrar som du söker toomodify är `ResourceDisk.EnableSwap=N` och`ResourceDisk.SwapSizeMB=0` 

Ändra hello parametrar toohello följande inställningar:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size i MB toomeet dina behov} 

När du har gjort hello ändra måste toorestart hello waagent eller starta om Linux VM-tooreflect ändringarna.  Du vet hello ändringar har införts och en växlingsfil har skapats när du använder hello `free` kommandot tooview ledigt utrymme. hello följande exempel har en 512MB växlingsfil som skapas när du ändrar hello **waagent.conf** fil:

```bash
azuseruser@myVM:~$ free
            total       used       free     shared    buffers     cached
Mem:       3525156     804168    2720988        408       8428     633192
-/+ buffers/cache:     162548    3362608
Swap:       524284          0     524284
```

## <a name="io-scheduling-algorithm-for-premium-storage"></a>I/o-schemaläggning algoritm för Premium-lagring
Med hello 2.6.18 Linux kernel har hello standard-i/o: schemaläggning-algoritmen ändrats från tidsgräns tooCFQ (helt verkliga queuing algoritmen). Det finns försumbar skillnad i prestanda skillnader mellan CFQ och tidsgräns för direktåtkomst i/o-mönster.  För SSD-baserad diskar där hello mönster för disk-i/o är huvudsakligen sekventiella, växla tillbaka toohello NOOP eller tidsgräns algoritmen kan uppnå högre i/o-prestanda.

### <a name="view-hello-current-io-scheduler"></a>Visa hello aktuella i/o-Schemaläggaren
Använd hello följande kommando:  

```bash
cat /sys/block/sda/queue/scheduler
```

Du se följande utdata som visar hello aktuella Schemaläggaren.  

```bash
noop [deadline] cfq
```

### <a name="change-hello-current-device-devsda-of-io-scheduling-algorithm"></a>Ändra hello aktuell enhet (/ dev/sda) av i/o schemaläggning algoritm
Använd hello följande kommandon:  

```bash
azureuser@myVM:~$ sudo su -
root@myVM:~# echo "noop" >/sys/block/sda/queue/scheduler
root@myVM:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
root@myVM:~# update-grub
```

> [!NOTE]
> Tillämpa den här inställningen för **/dev/sda** fristående är inte användbar. Ange på alla datadiskar där sekventiella i/o dominerar hello i/o-mönster.  

Du bör se hello följande utdata, vilket betyder att **grub.cfg** har återskapats och den hello standard Schemaläggaren har uppdaterats tooNOOP.  

```bash
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.13.0-34-generic
Found initrd image: /boot/initrd.img-3.13.0-34-generic
Found linux image: /boot/vmlinuz-3.13.0-32-generic
Found initrd image: /boot/initrd.img-3.13.0-32-generic
Found memtest86+ image: /memtest86+.elf
Found memtest86+ image: /memtest86+.bin
done
```

För hello Redhat distribution familj behöver du bara hello följande kommando:   

```bash
echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local
```

## <a name="using-software-raid-tooachieve-higher-iops"></a>Med hjälp av programvara RAID tooachieve högre I / Ops
Om din arbetsbelastning behöver fler IOps än vad en enda disk, måste toouse programvara RAID-konfiguration av flera diskar. Eftersom Azure utför redan disk återhämtning i hello lokala fabric-lagret, kan du uppnå hello högsta prestanda från en konfiguration med RAID-0 striping.  Etablera och skapa diskar i hello Azure-miljön och bifogar dem tooyour Linux VM innan partitionering, formatering och montera hello enheter.  Mer information om hur du konfigurerar en RAID konfiguration av programvara på din Linux VM i azure finns i hello  **[konfigurera programvara RAID på Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**  dokumentet.

## <a name="next-steps"></a>Nästa steg
Kom ihåg att med alla optimering diskussioner du behöver tooperform tester före och efter varje ändring toomeasure hello påverkan hello ändringen har.  Optimering är en steg-för-steg-process som har olika resultat på olika datorer i din miljö.  Vad fungerar för en konfiguration fungerar inte för andra.

Vissa länkar tooadditional resurser: 

* [Premium Storage: Lagring med höga prestanda för Azure Virtual Machines-arbetsbelastningar](../../storage/common/storage-premium-storage.md)
* [Användarhandboken för Azure Linux-Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Optimera MySQL prestanda på virtuella Azure Linux-datorer](classic/optimize-mysql.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Konfigurera programvarubaserad RAID på Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

