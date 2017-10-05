---
title: "Linux VM enhetsnamn har ändrats i Azure | Microsoft Docs"
description: "Förklarar varför enhetsnamn ändras och ger lösning på problemet."
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
ms.openlocfilehash: 789f4580901a22dc3aaae9599c7205c76f268403
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="troubleshooting-linux-vm-device-names-are-changed"></a>Felsöka: Linux VM enhetsnamn har ändrats

Artikeln förklarar varför enhetsnamn ändras efter att du startar om en Linux-dator (VM) eller återansluta diskarna. Det ger också lösningen på problemet.

## <a name="symptom"></a>Symtom

Följande problem kan uppstå när du kör virtuella Linux-datorer i Microsoft Azure.

- Den virtuella datorn inte startar efter en omstart.

- Om datadiskar oberoende och anbringas på nytt, ändra enheter namnet för diskar.

- Det går inte att ett program eller skript som refererar till en disk med hjälp av enhetsnamn. Du hittar enhetens namn på disken ändras.

## <a name="cause"></a>Orsak

Sökvägar till enheter i Linux är inte garanterat vara konsekvent mellan olika omstarter. Enhetsnamn består av större (bokstav) och mindre siffror.  När drivrutinen Linux lagring identifierar en ny enhet, tilldelar högre och lägre enhetsnummer till den från intervall. När en enhet tas bort frigörs siffror för enheten för att senare.

Problemet beror på att enheten skanning i Linux schemalagda av SCSI-undersystemet sker asynkront. Sista enhet sökväg namngivningen kan variera mellan olika omstarter. 

## <a name="solution"></a>Lösning

Använd beständiga naming för att lösa problemet. Det finns fyra metoder att namngivning av beständiga – av filsystem etikett, uuid, efter id och sökväg. Vi rekommenderar filesystem etikett och UUID metoder för virtuella Azure Linux-datorer. 

De flesta distributioner ange antingen det **nofail** eller **nobootwait** fstab alternativ. De här alternativen kan ett system för att starta även om disken inte montera vid start. Dokumentationen för den distribution för mer information om dessa parametrar. Läs mer om hur du konfigurerar en Linux VM för att använda en UUID när du lägger till en datadisk [Anslut till Linux-VM för att montera den nya disken](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk). 

När Azure Linux-agenten är installerad på en virtuell dator, används Udev regler för att skapa en uppsättning symboliska länkar under **/dev/disk/azure**. De här Udev-reglerna kan användas av program och skript för att identifiera diskar som är kopplade till den virtuella datorn, typ och LUN.

## <a name="more-information"></a>Mer information

### <a name="identify-disk-luns"></a>Identifiera disk LUN

Ett program kan använda LUN för att hitta alla anslutna diskar och konstruera symboliska länkar. Azure Linux-agenten har nu udev regler som konfigurera symboliska länkar från en LUN till enheterna, enligt följande:

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
                                 

LUN-information kan också hämtas från Linux gästen använder lsscsi eller liknande verktyg på följande sätt.

       $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

Den här gäst LUN-information kan användas med Azure-prenumeration metadata som identifierar platsen i Azure-lagring för den virtuella Hårddisken som lagrar data för partitionen. Till exempel använda az cli:

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

Ett skript eller program kan läsa utdata från blkid eller liknande källor till information och skapa symboliska länkar i **/dev** för användning. Utdata visar UUID: er för alla diskar som är kopplade till den virtuella datorn och enhetsfilen som de är kopplade:

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

Waagent udev regler skapar en uppsättning symboliska länkar under **/dev/disk/azure**:


    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1


Programmet kan använda den här informationen identifierar disk startenheten och resurs (tillfälliga) disken. I Azure, bör avser program **/dev/disk/azure/root-part1** eller **/dev/disk/azure-resource-part1** att identifiera dessa partitioner.

Det finns ytterligare partitioner från listan över blkid, ligga på en datadisk. Program kan underhålla UUID för dessa partitioner och använda en sökväg som den nedan för att identifiera namnet på en enhet vid körning:

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1

    
### <a name="get-the-latest-azure-storage-rules"></a>Hämta de senaste Azure storage-reglerna

Senaste Azure storage-reglerna kör behandlas följande kommandon:

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block


Mer information finns i följande artiklar:

- [Ubuntu: Med UUID](https://help.ubuntu.com/community/UsingUUID)

- [Red Hat: Beständiga namngivning av](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)

- [Linux: Vad UUID: er kan du göra](https://www.linux.com/news/what-uuids-can-do-you)

- [Udev: Introduktion till hantering av enheter i moderna Linux-System](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

