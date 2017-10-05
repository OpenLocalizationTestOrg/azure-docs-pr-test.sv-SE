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
# <a name="troubleshooting-linux-vm-device-names-are-changed"></a><span data-ttu-id="6b166-103">Felsöka: Linux VM enhetsnamn har ändrats</span><span class="sxs-lookup"><span data-stu-id="6b166-103">Troubleshooting: Linux VM device names are changed</span></span>

<span data-ttu-id="6b166-104">Artikeln förklarar varför enhetsnamn ändras efter att du startar om en Linux-dator (VM) eller återansluta diskarna.</span><span class="sxs-lookup"><span data-stu-id="6b166-104">The article explains why device names are changed after you restart a Linux virtual machine (VM), or reattach the disks.</span></span> <span data-ttu-id="6b166-105">Det ger också lösningen på problemet.</span><span class="sxs-lookup"><span data-stu-id="6b166-105">It also provides the solution for this problem.</span></span>

## <a name="symptom"></a><span data-ttu-id="6b166-106">Symtom</span><span class="sxs-lookup"><span data-stu-id="6b166-106">Symptom</span></span>

<span data-ttu-id="6b166-107">Följande problem kan uppstå när du kör virtuella Linux-datorer i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="6b166-107">You may experience the following problems when running Linux VMs in Microsoft Azure.</span></span>

- <span data-ttu-id="6b166-108">Den virtuella datorn inte startar efter en omstart.</span><span class="sxs-lookup"><span data-stu-id="6b166-108">The VM fails to boot after a restart.</span></span>

- <span data-ttu-id="6b166-109">Om datadiskar oberoende och anbringas på nytt, ändra enheter namnet för diskar.</span><span class="sxs-lookup"><span data-stu-id="6b166-109">If data disks are detached and reattached, the devices names for disks are changed.</span></span>

- <span data-ttu-id="6b166-110">Det går inte att ett program eller skript som refererar till en disk med hjälp av enhetsnamn.</span><span class="sxs-lookup"><span data-stu-id="6b166-110">An application or script that references a disk by using device name fails.</span></span> <span data-ttu-id="6b166-111">Du hittar enhetens namn på disken ändras.</span><span class="sxs-lookup"><span data-stu-id="6b166-111">You find that the device name of the disk is changed.</span></span>

## <a name="cause"></a><span data-ttu-id="6b166-112">Orsak</span><span class="sxs-lookup"><span data-stu-id="6b166-112">Cause</span></span>

<span data-ttu-id="6b166-113">Sökvägar till enheter i Linux är inte garanterat vara konsekvent mellan olika omstarter.</span><span class="sxs-lookup"><span data-stu-id="6b166-113">Device paths in Linux are not guaranteed to be consistent across restarts.</span></span> <span data-ttu-id="6b166-114">Enhetsnamn består av större (bokstav) och mindre siffror.</span><span class="sxs-lookup"><span data-stu-id="6b166-114">Device names consist of major (letter) and minor numbers.</span></span>  <span data-ttu-id="6b166-115">När drivrutinen Linux lagring identifierar en ny enhet, tilldelar högre och lägre enhetsnummer till den från intervall.</span><span class="sxs-lookup"><span data-stu-id="6b166-115">When the Linux storage device driver detects a new device, it assigns major and minor device numbers to it from the available range.</span></span> <span data-ttu-id="6b166-116">När en enhet tas bort frigörs siffror för enheten för att senare.</span><span class="sxs-lookup"><span data-stu-id="6b166-116">When a device is removed, the device numbers are freed to be reused later.</span></span>

<span data-ttu-id="6b166-117">Problemet beror på att enheten skanning i Linux schemalagda av SCSI-undersystemet sker asynkront.</span><span class="sxs-lookup"><span data-stu-id="6b166-117">The problem occurs because the device scanning in Linux scheduled by the SCSI subsystem happens asynchronously.</span></span> <span data-ttu-id="6b166-118">Sista enhet sökväg namngivningen kan variera mellan olika omstarter.</span><span class="sxs-lookup"><span data-stu-id="6b166-118">The final device path naming may vary across restarts.</span></span> 

## <a name="solution"></a><span data-ttu-id="6b166-119">Lösning</span><span class="sxs-lookup"><span data-stu-id="6b166-119">Solution</span></span>

<span data-ttu-id="6b166-120">Använd beständiga naming för att lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="6b166-120">To resolve this problem, use persistent naming.</span></span> <span data-ttu-id="6b166-121">Det finns fyra metoder att namngivning av beständiga – av filsystem etikett, uuid, efter id och sökväg.</span><span class="sxs-lookup"><span data-stu-id="6b166-121">There are four methods to persistent naming - by filesystem label, by uuid, by id and by path.</span></span> <span data-ttu-id="6b166-122">Vi rekommenderar filesystem etikett och UUID metoder för virtuella Azure Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="6b166-122">We recommend the filesystem label and UUID methods for Azure Linux VMs.</span></span> 

<span data-ttu-id="6b166-123">De flesta distributioner ange antingen det **nofail** eller **nobootwait** fstab alternativ.</span><span class="sxs-lookup"><span data-stu-id="6b166-123">Most distributions also provide either the **nofail** or **nobootwait** fstab options.</span></span> <span data-ttu-id="6b166-124">De här alternativen kan ett system för att starta även om disken inte montera vid start.</span><span class="sxs-lookup"><span data-stu-id="6b166-124">These options enable a system to boot even if the disk fails to mount at startup.</span></span> <span data-ttu-id="6b166-125">Dokumentationen för den distribution för mer information om dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="6b166-125">Check the distribution's documentation for more information about these parameters.</span></span> <span data-ttu-id="6b166-126">Läs mer om hur du konfigurerar en Linux VM för att använda en UUID när du lägger till en datadisk [Anslut till Linux-VM för att montera den nya disken](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="6b166-126">For more information about how to configure a Linux VM to use a UUID when you add a data disk, see [Connect to the Linux VM to mount the new disk](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> 

<span data-ttu-id="6b166-127">När Azure Linux-agenten är installerad på en virtuell dator, används Udev regler för att skapa en uppsättning symboliska länkar under **/dev/disk/azure**.</span><span class="sxs-lookup"><span data-stu-id="6b166-127">When the Azure Linux agent is installed on a VM, it uses Udev rules to construct a set of symbolic links under **/dev/disk/azure**.</span></span> <span data-ttu-id="6b166-128">De här Udev-reglerna kan användas av program och skript för att identifiera diskar som är kopplade till den virtuella datorn, typ och LUN.</span><span class="sxs-lookup"><span data-stu-id="6b166-128">These Udev rules can be used by applications and scripts to identify disks are attached to the VM, their type, and the LUN.</span></span>

## <a name="more-information"></a><span data-ttu-id="6b166-129">Mer information</span><span class="sxs-lookup"><span data-stu-id="6b166-129">More information</span></span>

### <a name="identify-disk-luns"></a><span data-ttu-id="6b166-130">Identifiera disk LUN</span><span class="sxs-lookup"><span data-stu-id="6b166-130">Identify disk LUNs</span></span>

<span data-ttu-id="6b166-131">Ett program kan använda LUN för att hitta alla anslutna diskar och konstruera symboliska länkar.</span><span class="sxs-lookup"><span data-stu-id="6b166-131">An application can use LUNs to find all the attached disks and constructing symbolic links.</span></span> <span data-ttu-id="6b166-132">Azure Linux-agenten har nu udev regler som konfigurera symboliska länkar från en LUN till enheterna, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="6b166-132">The Azure Linux agent now includes udev rules that set up symbolic links from a LUN to the devices, as follows:</span></span>

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
                                 

<span data-ttu-id="6b166-133">LUN-information kan också hämtas från Linux gästen använder lsscsi eller liknande verktyg på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="6b166-133">LUN information can also be retrieved from the Linux guest using lsscsi or similar tool as follows.</span></span>

       $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

<span data-ttu-id="6b166-134">Den här gäst LUN-information kan användas med Azure-prenumeration metadata som identifierar platsen i Azure-lagring för den virtuella Hårddisken som lagrar data för partitionen.</span><span class="sxs-lookup"><span data-stu-id="6b166-134">This guest LUN information can be used with Azure subscription metadata to identify the location in Azure storage of the VHD which stores the partition data.</span></span> <span data-ttu-id="6b166-135">Till exempel använda az cli:</span><span class="sxs-lookup"><span data-stu-id="6b166-135">For example, use the az cli:</span></span>

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

### <a name="discover-filesystem-uuids-by-using-blkid"></a><span data-ttu-id="6b166-136">Identifiera filesystem UUID: er med hjälp av blkid</span><span class="sxs-lookup"><span data-stu-id="6b166-136">Discover filesystem UUIDs by using blkid</span></span>

<span data-ttu-id="6b166-137">Ett skript eller program kan läsa utdata från blkid eller liknande källor till information och skapa symboliska länkar i **/dev** för användning.</span><span class="sxs-lookup"><span data-stu-id="6b166-137">A script or application can read the output of blkid, or similar sources of information, and construct symbolic links in **/dev** for use.</span></span> <span data-ttu-id="6b166-138">Utdata visar UUID: er för alla diskar som är kopplade till den virtuella datorn och enhetsfilen som de är kopplade:</span><span class="sxs-lookup"><span data-stu-id="6b166-138">The output will show the UUIDs of all disks attached to the VM and the device file to which they are associated:</span></span>

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

<span data-ttu-id="6b166-139">Waagent udev regler skapar en uppsättning symboliska länkar under **/dev/disk/azure**:</span><span class="sxs-lookup"><span data-stu-id="6b166-139">The waagent udev rules construct a set of symbolic links under **/dev/disk/azure**:</span></span>


    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1


<span data-ttu-id="6b166-140">Programmet kan använda den här informationen identifierar disk startenheten och resurs (tillfälliga) disken.</span><span class="sxs-lookup"><span data-stu-id="6b166-140">The application can use this information identify the boot disk device and the resource (ephemeral) disk.</span></span> <span data-ttu-id="6b166-141">I Azure, bör avser program **/dev/disk/azure/root-part1** eller **/dev/disk/azure-resource-part1** att identifiera dessa partitioner.</span><span class="sxs-lookup"><span data-stu-id="6b166-141">In Azure, applications should refer to **/dev/disk/azure/root-part1** or **/dev/disk/azure-resource-part1** to discover these partitions.</span></span>

<span data-ttu-id="6b166-142">Det finns ytterligare partitioner från listan över blkid, ligga på en datadisk.</span><span class="sxs-lookup"><span data-stu-id="6b166-142">If there are additional partitions from the blkid list, they reside on a data disk.</span></span> <span data-ttu-id="6b166-143">Program kan underhålla UUID för dessa partitioner och använda en sökväg som den nedan för att identifiera namnet på en enhet vid körning:</span><span class="sxs-lookup"><span data-stu-id="6b166-143">Applications can maintain the UUID for these partitions and use a path like the below to discover the device name at runtime:</span></span>

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1

    
### <a name="get-the-latest-azure-storage-rules"></a><span data-ttu-id="6b166-144">Hämta de senaste Azure storage-reglerna</span><span class="sxs-lookup"><span data-stu-id="6b166-144">Get the latest Azure storage rules</span></span>

<span data-ttu-id="6b166-145">Senaste Azure storage-reglerna kör behandlas följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="6b166-145">To the latest Azure storage rules, run th following commands:</span></span>

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block


<span data-ttu-id="6b166-146">Mer information finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="6b166-146">For more information, see the following articles:</span></span>

- [<span data-ttu-id="6b166-147">Ubuntu: Med UUID</span><span class="sxs-lookup"><span data-stu-id="6b166-147">Ubuntu: Using UUID</span></span>](https://help.ubuntu.com/community/UsingUUID)

- [<span data-ttu-id="6b166-148">Red Hat: Beständiga namngivning av</span><span class="sxs-lookup"><span data-stu-id="6b166-148">Red Hat: Persistent Naming</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)

- [<span data-ttu-id="6b166-149">Linux: Vad UUID: er kan du göra</span><span class="sxs-lookup"><span data-stu-id="6b166-149">Linux: What UUIDs can do for you</span></span>](https://www.linux.com/news/what-uuids-can-do-you)

- [<span data-ttu-id="6b166-150">Udev: Introduktion till hantering av enheter i moderna Linux-System</span><span class="sxs-lookup"><span data-stu-id="6b166-150">Udev: Introduction to Device Management In Modern Linux System</span></span>](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

