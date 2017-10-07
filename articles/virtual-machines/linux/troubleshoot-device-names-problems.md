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
# <a name="troubleshooting-linux-vm-device-names-are-changed"></a><span data-ttu-id="9508d-103">Felsöka: Linux VM enhetsnamn har ändrats</span><span class="sxs-lookup"><span data-stu-id="9508d-103">Troubleshooting: Linux VM device names are changed</span></span>

<span data-ttu-id="9508d-104">hello artikeln förklarar varför enhetsnamn ändras efter att du startar om en Linux-dator (VM) eller återansluta hello diskar.</span><span class="sxs-lookup"><span data-stu-id="9508d-104">hello article explains why device names are changed after you restart a Linux virtual machine (VM), or reattach hello disks.</span></span> <span data-ttu-id="9508d-105">Det ger också hello lösning på problemet.</span><span class="sxs-lookup"><span data-stu-id="9508d-105">It also provides hello solution for this problem.</span></span>

## <a name="symptom"></a><span data-ttu-id="9508d-106">Symtom</span><span class="sxs-lookup"><span data-stu-id="9508d-106">Symptom</span></span>

<span data-ttu-id="9508d-107">Det kan uppstå hello följande problem när du kör virtuella Linux-datorer i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9508d-107">You may experience hello following problems when running Linux VMs in Microsoft Azure.</span></span>

- <span data-ttu-id="9508d-108">hello VM misslyckas tooboot efter en omstart.</span><span class="sxs-lookup"><span data-stu-id="9508d-108">hello VM fails tooboot after a restart.</span></span>

- <span data-ttu-id="9508d-109">Om datadiskar oberoende och anbringas på nytt, ändra hello enheter namnet för diskar.</span><span class="sxs-lookup"><span data-stu-id="9508d-109">If data disks are detached and reattached, hello devices names for disks are changed.</span></span>

- <span data-ttu-id="9508d-110">Det går inte att ett program eller skript som refererar till en disk med hjälp av enhetsnamn.</span><span class="sxs-lookup"><span data-stu-id="9508d-110">An application or script that references a disk by using device name fails.</span></span> <span data-ttu-id="9508d-111">Du hittar den hello enhetsnamn hello disken har ändrats.</span><span class="sxs-lookup"><span data-stu-id="9508d-111">You find that hello device name of hello disk is changed.</span></span>

## <a name="cause"></a><span data-ttu-id="9508d-112">Orsak</span><span class="sxs-lookup"><span data-stu-id="9508d-112">Cause</span></span>

<span data-ttu-id="9508d-113">Sökvägar till enheter i Linux är inte garanterat toobe konsekvent mellan olika omstarter.</span><span class="sxs-lookup"><span data-stu-id="9508d-113">Device paths in Linux are not guaranteed toobe consistent across restarts.</span></span> <span data-ttu-id="9508d-114">Enhetsnamn består av större (bokstav) och mindre siffror.</span><span class="sxs-lookup"><span data-stu-id="9508d-114">Device names consist of major (letter) and minor numbers.</span></span>  <span data-ttu-id="9508d-115">När drivrutinen för hello Linux lagring identifierar en ny enhet, tilldelas enheten högre och lägre nummer tooit från hello intervall.</span><span class="sxs-lookup"><span data-stu-id="9508d-115">When hello Linux storage device driver detects a new device, it assigns major and minor device numbers tooit from hello available range.</span></span> <span data-ttu-id="9508d-116">När en enhet tas bort är hello enhetsnummer frigjort toobe återanvändas.</span><span class="sxs-lookup"><span data-stu-id="9508d-116">When a device is removed, hello device numbers are freed toobe reused later.</span></span>

<span data-ttu-id="9508d-117">hello uppstår problemet eftersom hello skanning i Linux schemalagda av undersystem för hello SCSI-enhet händer asynkront.</span><span class="sxs-lookup"><span data-stu-id="9508d-117">hello problem occurs because hello device scanning in Linux scheduled by hello SCSI subsystem happens asynchronously.</span></span> <span data-ttu-id="9508d-118">namngivning av hello sista enhet sökvägen kan variera mellan olika omstarter.</span><span class="sxs-lookup"><span data-stu-id="9508d-118">hello final device path naming may vary across restarts.</span></span> 

## <a name="solution"></a><span data-ttu-id="9508d-119">Lösning</span><span class="sxs-lookup"><span data-stu-id="9508d-119">Solution</span></span>

<span data-ttu-id="9508d-120">tooresolve det här problemet använder beständiga naming.</span><span class="sxs-lookup"><span data-stu-id="9508d-120">tooresolve this problem, use persistent naming.</span></span> <span data-ttu-id="9508d-121">Det finns fyra metoderna toopersistent naming - av filsystem etikett, uuid, efter id och sökväg.</span><span class="sxs-lookup"><span data-stu-id="9508d-121">There are four methods toopersistent naming - by filesystem label, by uuid, by id and by path.</span></span> <span data-ttu-id="9508d-122">Vi rekommenderar hello filesystem etikett och UUID metoder för virtuella Azure Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="9508d-122">We recommend hello filesystem label and UUID methods for Azure Linux VMs.</span></span> 

<span data-ttu-id="9508d-123">De flesta distributioner ger också antingen hello **nofail** eller **nobootwait** fstab alternativ.</span><span class="sxs-lookup"><span data-stu-id="9508d-123">Most distributions also provide either hello **nofail** or **nobootwait** fstab options.</span></span> <span data-ttu-id="9508d-124">De här alternativen kan ett system tooboot även om hello disk kraschar toomount vid start.</span><span class="sxs-lookup"><span data-stu-id="9508d-124">These options enable a system tooboot even if hello disk fails toomount at startup.</span></span> <span data-ttu-id="9508d-125">Hello distribution i dokumentationen för mer information om dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="9508d-125">Check hello distribution's documentation for more information about these parameters.</span></span> <span data-ttu-id="9508d-126">Mer information om hur tooconfigure Linux VM-toouse en UUID när du lägger till en datadisk Se [ansluta toohello Linux VM toomount hello ny disk](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="9508d-126">For more information about how tooconfigure a Linux VM toouse a UUID when you add a data disk, see [Connect toohello Linux VM toomount hello new disk](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> 

<span data-ttu-id="9508d-127">När hello Azure Linux-agenten är installerad på en virtuell dator, används Udev regler tooconstruct en uppsättning symboliska länkar under **/dev/disk/azure**.</span><span class="sxs-lookup"><span data-stu-id="9508d-127">When hello Azure Linux agent is installed on a VM, it uses Udev rules tooconstruct a set of symbolic links under **/dev/disk/azure**.</span></span> <span data-ttu-id="9508d-128">Reglerna Udev kan användas av program och skript tooidentify diskar som är anslutna toohello VM, typ, och hello LUN.</span><span class="sxs-lookup"><span data-stu-id="9508d-128">These Udev rules can be used by applications and scripts tooidentify disks are attached toohello VM, their type, and hello LUN.</span></span>

## <a name="more-information"></a><span data-ttu-id="9508d-129">Mer information</span><span class="sxs-lookup"><span data-stu-id="9508d-129">More information</span></span>

### <a name="identify-disk-luns"></a><span data-ttu-id="9508d-130">Identifiera disk LUN</span><span class="sxs-lookup"><span data-stu-id="9508d-130">Identify disk LUNs</span></span>

<span data-ttu-id="9508d-131">Ett program kan använda LUN toofind alla hello anslutna diskar och konstruera symboliska länkar.</span><span class="sxs-lookup"><span data-stu-id="9508d-131">An application can use LUNs toofind all hello attached disks and constructing symbolic links.</span></span> <span data-ttu-id="9508d-132">hello Azure Linux-agenten innehåller nu udev regler som ställts in symboliska länkar från en LUN toohello enheter på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9508d-132">hello Azure Linux agent now includes udev rules that set up symbolic links from a LUN toohello devices, as follows:</span></span>

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
                                 

<span data-ttu-id="9508d-133">LUN-information kan också hämtas från hello Linux gäst lsscsi eller liknande verktyg på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="9508d-133">LUN information can also be retrieved from hello Linux guest using lsscsi or similar tool as follows.</span></span>

       $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

<span data-ttu-id="9508d-134">Den här gäst LUN-information kan användas med Azure-prenumeration metadata tooidentify hello platsen i Azure storage hello VHD som lagrar hello partition data.</span><span class="sxs-lookup"><span data-stu-id="9508d-134">This guest LUN information can be used with Azure subscription metadata tooidentify hello location in Azure storage of hello VHD which stores hello partition data.</span></span> <span data-ttu-id="9508d-135">Till exempel använda hello az cli:</span><span class="sxs-lookup"><span data-stu-id="9508d-135">For example, use hello az cli:</span></span>

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

### <a name="discover-filesystem-uuids-by-using-blkid"></a><span data-ttu-id="9508d-136">Identifiera filesystem UUID: er med hjälp av blkid</span><span class="sxs-lookup"><span data-stu-id="9508d-136">Discover filesystem UUIDs by using blkid</span></span>

<span data-ttu-id="9508d-137">Ett skript eller program kan läsa hello utdata från blkid eller liknande källor till information och skapa symboliska länkar i **/dev** för användning.</span><span class="sxs-lookup"><span data-stu-id="9508d-137">A script or application can read hello output of blkid, or similar sources of information, and construct symbolic links in **/dev** for use.</span></span> <span data-ttu-id="9508d-138">hello utdata visar hello UUID: er för alla diskar kopplade toohello VM och hello enhet filen toowhich de är kopplade:</span><span class="sxs-lookup"><span data-stu-id="9508d-138">hello output will show hello UUIDs of all disks attached toohello VM and hello device file toowhich they are associated:</span></span>

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

<span data-ttu-id="9508d-139">Hej waagent udev regler skapar en uppsättning symboliska länkar under **/dev/disk/azure**:</span><span class="sxs-lookup"><span data-stu-id="9508d-139">hello waagent udev rules construct a set of symbolic links under **/dev/disk/azure**:</span></span>


    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1


<span data-ttu-id="9508d-140">hello program kan använda den här informationen identifierar hello disk startenheten och hello resurs (tillfälliga) disk.</span><span class="sxs-lookup"><span data-stu-id="9508d-140">hello application can use this information identify hello boot disk device and hello resource (ephemeral) disk.</span></span> <span data-ttu-id="9508d-141">I Azure, program för vända**/dev/disk/azure/root-part1** eller **/dev/disk/azure-resource-part1** toodiscover partitionerna.</span><span class="sxs-lookup"><span data-stu-id="9508d-141">In Azure, applications should refer too**/dev/disk/azure/root-part1** or **/dev/disk/azure-resource-part1** toodiscover these partitions.</span></span>

<span data-ttu-id="9508d-142">Det finns ytterligare partitioner från hello blkid lista, ligga på en datadisk.</span><span class="sxs-lookup"><span data-stu-id="9508d-142">If there are additional partitions from hello blkid list, they reside on a data disk.</span></span> <span data-ttu-id="9508d-143">Program kan underhålla hello UUID för dessa partitioner och använda en sökväg som hello nedan toodiscover hello enhetsnamn vid körning:</span><span class="sxs-lookup"><span data-stu-id="9508d-143">Applications can maintain hello UUID for these partitions and use a path like hello below toodiscover hello device name at runtime:</span></span>

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1

    
### <a name="get-hello-latest-azure-storage-rules"></a><span data-ttu-id="9508d-144">Hämta hello senaste Azure storage-regler</span><span class="sxs-lookup"><span data-stu-id="9508d-144">Get hello latest Azure storage rules</span></span>

<span data-ttu-id="9508d-145">toohello senaste Azure storage-regler, kör behandlas följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="9508d-145">toohello latest Azure storage rules, run th following commands:</span></span>

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block


<span data-ttu-id="9508d-146">Mer information finns i följande artiklar hello:</span><span class="sxs-lookup"><span data-stu-id="9508d-146">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="9508d-147">Ubuntu: Med UUID</span><span class="sxs-lookup"><span data-stu-id="9508d-147">Ubuntu: Using UUID</span></span>](https://help.ubuntu.com/community/UsingUUID)

- [<span data-ttu-id="9508d-148">Red Hat: Beständiga namngivning av</span><span class="sxs-lookup"><span data-stu-id="9508d-148">Red Hat: Persistent Naming</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)

- [<span data-ttu-id="9508d-149">Linux: Vad UUID: er kan du göra</span><span class="sxs-lookup"><span data-stu-id="9508d-149">Linux: What UUIDs can do for you</span></span>](https://www.linux.com/news/what-uuids-can-do-you)

- [<span data-ttu-id="9508d-150">Udev: Introduktion tooDevice Management i moderna Linux-System</span><span class="sxs-lookup"><span data-stu-id="9508d-150">Udev: Introduction tooDevice Management In Modern Linux System</span></span>](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

