---
title: "namn på aaaLinux Virtuella enheter har ändrats i Azure | Microsoft Docs"
description: "Förklarar hello varför enhetsnamn ändras och ger lösning på problemet."
services: virtual-machines-linux
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: 
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 07/12/2017
ms.author: genli
ms.openlocfilehash: 4d3a5853d61edd2c8e8b85ab69e5ed3b3bc00bb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-linux-vm-device-names-are-changed"></a>Felsöka: Linux VM enhetsnamn har ändrats

hello artikeln förklarar varför enhetsnamn ändras efter att du startar om en Linux-dator (VM) eller återansluta hello diskar. Det ger också hello lösning på problemet.

## <a name="symptom"></a>Symtom

Det kan uppstå hello följande problem när du kör virtuella Linux-datorer i Microsoft Azure.

- hello VM misslyckas tooboot efter en omstart.

- Om datadiskar oberoende och anbringas på nytt, ändra hello enheter namnet för diskar.

- Det går inte att ett program eller skript som refererar till en disk med hjälp av enhetsnamn. Du hittar den hello enhetsnamn hello disken har ändrats.

## <a name="cause"></a>Orsak

Sökvägar till enheter i Linux är inte garanterat toobe konsekvent mellan olika omstarter. Enhetsnamn består av större (bokstav) och mindre siffror.  När drivrutinen för hello Linux lagring identifierar en ny enhet, tilldelas enheten högre och lägre nummer tooit från hello intervall. När en enhet tas bort är hello enhetsnummer frigjort toobe återanvändas.

hello uppstår problemet eftersom hello skanning i Linux schemalagda av undersystem för hello SCSI-enhet händer asynkront. namngivning av hello sista enhet sökvägen kan variera mellan olika omstarter. 

## <a name="solution"></a>Lösning

tooresolve det här problemet använder beständiga naming. Det finns fyra metoderna toopersistent naming - av filsystem etikett, uuid, efter id och sökväg. Vi rekommenderar hello filesystem etikett och UUID metoder för virtuella Azure Linux-datorer. 

De flesta distributioner ger också antingen hello **nofail** eller **nobootwait** fstab alternativ. De här alternativen kan ett system tooboot även om hello disk kraschar toomount vid start. Hello distribution i dokumentationen för mer information om dessa parametrar. Mer information om hur tooconfigure Linux VM-toouse en UUID när du lägger till en datadisk Se [ansluta toohello Linux VM toomount hello ny disk](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk). 

När hello Azure Linux-agenten är installerad på en virtuell dator, används Udev regler tooconstruct en uppsättning symboliska länkar under **/dev/disk/azure**. Reglerna Udev kan användas av program och skript tooidentify diskar som är anslutna toohello VM, typ, och hello LUN.

## <a name="more-information"></a>Mer information

### <a name="identify-disk-luns"></a>Identifiera disk LUN

Ett program kan använda LUN toofind alla hello anslutna diskar och konstruera symboliska länkar. hello Azure Linux-agenten innehåller nu udev regler som ställts in symboliska länkar från en LUN toohello enheter på följande sätt:

    $ tree /dev/disk/azure

    /dev/disk/azure
    ├── resource -> ../../sdb
    ├── resource-part1 -> ../../sdb1
    ├── root -> ../../sda
    ├── root-part1 -> ../../sda1
    └── scsi1
        ├── lun0 -> ../../../sdc
        ├── lun0-part1 -> ../../../sdc1
        ├── lun1 -> ../../../sdd
        ├── lun1-part1 -> ../../../sdd1
        ├── lun1-part2 -> ../../../sdd2
        └── lun1-part3 -> ../../../sdd3                                    
                                 

LUN-information kan också hämtas från hello Linux gäst lsscsi eller liknande verktyg på följande sätt.

       $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

Den här gäst LUN-information kan användas med Azure-prenumeration metadata tooidentify hello platsen i Azure storage hello VHD som lagrar hello partition data. Till exempel använda hello az cli:

    $ az vm show --resource-group testVM --name testVM | jq -r .storageProfile.dataDisks                                        
    [                                                                                                                                                                  
    {                                                                                                                                                                  
    "caching": "None",                                                                                                                                              
      "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 1023,                                                                                                                                             
      "image": null,                                                                                                                                                   
    "lun": 0,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-114353",                                                                                                                    
    "vhd": {                                                                                                                                                          
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-114353.vhd"                                                       
    }                                                                                                                                                              
    },                                                                                                                                                                
    {                                                                                                                                                                   
    "caching": "None",                                                                                                                                               
    "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 512,                                                                                                                                              
    "image": null,                                                                                                                                                   
    "lun": 1,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-121516",                                                                                                                    
    "vhd": {                                                                                                                                                           
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-121516.vhd"                                                       
      }                                                                                                                                                             
      }                                                                                                                                                             
    ]

### <a name="discover-filesystem-uuids-by-using-blkid"></a>Identifiera filesystem UUID: er med hjälp av blkid

Ett skript eller program kan läsa hello utdata från blkid eller liknande källor till information och skapa symboliska länkar i **/dev** för användning. hello utdata visar hello UUID: er för alla diskar kopplade toohello VM och hello enhet filen toowhich de är kopplade:

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

Hej waagent udev regler skapar en uppsättning symboliska länkar under **/dev/disk/azure**:


    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1


hello program kan använda den här informationen identifierar hello disk startenheten och hello resurs (tillfälliga) disk. I Azure, program för vända**/dev/disk/azure/root-part1** eller **/dev/disk/azure-resource-part1** toodiscover partitionerna.

Det finns ytterligare partitioner från hello blkid lista, ligga på en datadisk. Program kan underhålla hello UUID för dessa partitioner och använda en sökväg som hello nedan toodiscover hello enhetsnamn vid körning:

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1

    
### <a name="get-hello-latest-azure-storage-rules"></a>Hämta hello senaste Azure storage-regler

toohello senaste Azure storage-regler, kör behandlas följande kommandon:

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block


Mer information finns i följande artiklar hello:

- [Ubuntu: Med UUID](https://help.ubuntu.com/community/UsingUUID)

- [Red Hat: Beständiga namngivning av](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)

- [Linux: Vad UUID: er kan du göra](https://www.linux.com/news/what-uuids-can-do-you)

- [Udev: Introduktion tooDevice Management i moderna Linux-System](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

