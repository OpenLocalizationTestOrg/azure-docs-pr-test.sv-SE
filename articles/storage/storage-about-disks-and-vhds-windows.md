---
title: "Om diskar och virtuella hårddiskar för virtuella datorer i Microsoft Azure Windows | Microsoft Docs"
description: "Lär dig grunderna om diskar och virtuella hårddiskar för Windows-datorer i Azure."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 0142c64d-5e8c-4d62-aa6f-06d6261f485a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: 34a4d8fa176484fbadb1b385d794cada5be607c8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="about-disks-and-vhds-for-azure-windows-vms"></a><span data-ttu-id="23968-103">Om diskar och virtuella hårddiskar för virtuella Azure Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="23968-103">About disks and VHDs for Azure Windows VMs</span></span>
<span data-ttu-id="23968-104">Precis som andra dator använder virtuella datorer i Azure diskar som en plats för att lagra ett operativsystem, program och data.</span><span class="sxs-lookup"><span data-stu-id="23968-104">Just like any other computer, virtual machines in Azure use disks as a place to store an operating system, applications, and data.</span></span> <span data-ttu-id="23968-105">Alla Azure virtuella datorer har minst två diskar – en disk i Windows-operativsystem och en tillfällig disk.</span><span class="sxs-lookup"><span data-stu-id="23968-105">All Azure virtual machines have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="23968-106">Operativsystemdisken har skapats från en avbildning och både operativsystemdisken och image är virtuella hårddiskar (VHD) lagras i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="23968-106">The operating system disk is created from an image, and both the operating system disk and the image are virtual hard disks (VHDs) stored in an Azure storage account.</span></span> <span data-ttu-id="23968-107">Virtuella datorer kan också ha en eller flera datadiskar som lagras också som virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="23968-107">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span> 

<span data-ttu-id="23968-108">I den här artikeln ska vi pratar om olika användningsområden för diskar och beskrivs de olika typerna av diskar som du kan skapa och använda.</span><span class="sxs-lookup"><span data-stu-id="23968-108">In this article, we will talk about the different uses for the disks, and then discuss the different types of disks you can create and use.</span></span> <span data-ttu-id="23968-109">Den här artikeln är också tillgängligt för [virtuella Linux-datorer](storage-about-disks-and-vhds-linux.md).</span><span class="sxs-lookup"><span data-stu-id="23968-109">This article is also available for [Linux virtual machines](storage-about-disks-and-vhds-linux.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a><span data-ttu-id="23968-110">Diskar som används av virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="23968-110">Disks used by VMs</span></span>

<span data-ttu-id="23968-111">Låt oss ta en titt på hur diskarna som används av de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="23968-111">Let's take a look at how the disks are used by the VMs.</span></span>

### <a name="operating-system-disk"></a><span data-ttu-id="23968-112">Operativsystemdisk</span><span class="sxs-lookup"><span data-stu-id="23968-112">Operating system disk</span></span>
<span data-ttu-id="23968-113">Varje virtuell dator har en ansluten operativsystemdisk.</span><span class="sxs-lookup"><span data-stu-id="23968-113">Every virtual machine has one attached operating system disk.</span></span> <span data-ttu-id="23968-114">Den har registrerats som en SATA-enhet och märkta som enhet C: som standard.</span><span class="sxs-lookup"><span data-stu-id="23968-114">It's registered as a SATA drive and labeled as the C: drive by default.</span></span> <span data-ttu-id="23968-115">Den här disken har en maximal kapacitet på 2 048 gigabyte (GB).</span><span class="sxs-lookup"><span data-stu-id="23968-115">This disk has a maximum capacity of 2048 gigabytes (GB).</span></span> 

### <a name="temporary-disk"></a><span data-ttu-id="23968-116">Diskutrymme</span><span class="sxs-lookup"><span data-stu-id="23968-116">Temporary disk</span></span>
<span data-ttu-id="23968-117">Varje virtuell dator innehåller en tillfällig disk.</span><span class="sxs-lookup"><span data-stu-id="23968-117">Each VM contains a temporary disk.</span></span> <span data-ttu-id="23968-118">Den tillfälliga disken tillhandahåller kortsiktig lagring för program och processer och är avsedd att bara lagra data, till exempel sida eller växla filer.</span><span class="sxs-lookup"><span data-stu-id="23968-118">The temporary disk provides short-term storage for applications and processes and is intended to only store data such as page or swap files.</span></span> <span data-ttu-id="23968-119">Data på den tillfälliga disken kan gå förlorade under en [underhållshändelse](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) eller när du [distribuera en virtuell dator](../virtual-machines/windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="23968-119">Data on the temporary disk may be lost during a [maintenance event](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) or when you [redeploy a VM](../virtual-machines/windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="23968-120">Data på den tillfälliga enheten under en standard omstart av den virtuella datorn ska sparas.</span><span class="sxs-lookup"><span data-stu-id="23968-120">During a standard reboot of the VM, the data on the temporary drive should persist.</span></span>

<span data-ttu-id="23968-121">Den tillfälliga disken är märkt som D: enhet som standard och det används för att lagra pagefile.sys.</span><span class="sxs-lookup"><span data-stu-id="23968-121">The temporary disk is labeled as the D: drive by default and it used for storing pagefile.sys.</span></span> <span data-ttu-id="23968-122">Om du vill mappa om den här disken till en annan enhetsbeteckning finns [ändra enhetsbeteckningen för den tillfälliga disken i Windows](../virtual-machines/windows/change-drive-letter.md).</span><span class="sxs-lookup"><span data-stu-id="23968-122">To remap this disk to a different drive letter, see [Change the drive letter of the Windows temporary disk](../virtual-machines/windows/change-drive-letter.md).</span></span> <span data-ttu-id="23968-123">Storleken på den tillfälliga disken beror på storleken på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="23968-123">The size of the temporary disk varies, based on the size of the virtual machine.</span></span> <span data-ttu-id="23968-124">Mer information finns i [storlekar för Windows-datorer](../virtual-machines/windows/sizes.md).</span><span class="sxs-lookup"><span data-stu-id="23968-124">For more information, see [Sizes for Windows virtual machines](../virtual-machines/windows/sizes.md).</span></span>

<span data-ttu-id="23968-125">Mer information om hur Azure använder temporär disk finns [förstå den tillfälliga enheten på virtuella datorer i Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="23968-125">For more information on how Azure uses the temporary disk, see [Understanding the temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span></span>


### <a name="data-disk"></a><span data-ttu-id="23968-126">Datadisk</span><span class="sxs-lookup"><span data-stu-id="23968-126">Data disk</span></span>
<span data-ttu-id="23968-127">En datadisk är en virtuell Hårddisk som är kopplad till en virtuell dator kan lagra programdata och andra data som du behöver.</span><span class="sxs-lookup"><span data-stu-id="23968-127">A data disk is a VHD that's attached to a virtual machine to store application data, or other data you need to keep.</span></span> <span data-ttu-id="23968-128">Datadiskar registreras som SCSI-enheter och är märkta med en bokstav som du väljer.</span><span class="sxs-lookup"><span data-stu-id="23968-128">Data disks are registered as SCSI drives and are labeled with a letter that you choose.</span></span> <span data-ttu-id="23968-129">Varje datadisk har en maximal kapacitet för 4095 GB.</span><span class="sxs-lookup"><span data-stu-id="23968-129">Each data disk has a maximum capacity of 4095 GB.</span></span> <span data-ttu-id="23968-130">Storleken på den virtuella datorn avgör hur många datadiskar som du kan ansluta till den och typen av lagring som du kan använda som värd för diskarna.</span><span class="sxs-lookup"><span data-stu-id="23968-130">The size of the virtual machine determines how many data disks you can attach to it and the type of storage you can use to host the disks.</span></span>

> [!NOTE]
> <span data-ttu-id="23968-131">Mer information om kapacitet för virtuella datorer finns [storlekar för Windows-datorer](../virtual-machines/windows/sizes.md).</span><span class="sxs-lookup"><span data-stu-id="23968-131">For more information about virtual machines capacities, see [Sizes for Windows virtual machines](../virtual-machines/windows/sizes.md).</span></span>
> 

<span data-ttu-id="23968-132">Azure skapar en operativsystemdisk när du skapar en virtuell dator från en avbildning.</span><span class="sxs-lookup"><span data-stu-id="23968-132">Azure creates an operating system disk when you create a virtual machine from an image.</span></span> <span data-ttu-id="23968-133">Om du använder en avbildning med datadiskar skapar Azure även datadiskar när den virtuella datorn skapas.</span><span class="sxs-lookup"><span data-stu-id="23968-133">If you use an image that includes data disks, Azure also creates the data disks when it creates the virtual machine.</span></span> <span data-ttu-id="23968-134">Annars kan du lägga till datadiskar när du har skapat den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="23968-134">Otherwise, you add data disks after you create the virtual machine.</span></span>

<span data-ttu-id="23968-135">Du kan lägga till diskar till en virtuell dator när som helst av **kopplar** disken till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="23968-135">You can add data disks to a virtual machine at any time, by **attaching** the disk to the virtual machine.</span></span> <span data-ttu-id="23968-136">Du kan använda en virtuell Hårddisk som du har laddat upp eller kopieras till ditt lagringskonto, eller en som Azure skapar åt dig.</span><span class="sxs-lookup"><span data-stu-id="23968-136">You can use a VHD that you've uploaded or copied to your storage account, or one that Azure creates for you.</span></span> <span data-ttu-id="23968-137">Koppla en datadisk associerar VHD-filen med den virtuella datorn genom att placera ett lån på den virtuella Hårddisken, så den inte kan tas bort från lagring när den är kopplad.</span><span class="sxs-lookup"><span data-stu-id="23968-137">Attaching a data disk associates the VHD file with the VM by placing a 'lease' on the VHD so it can't be deleted from storage while it's still attached.</span></span>


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a><span data-ttu-id="23968-138">En sista rekommendation: Använd TRIMNING med ohanterad standarddiskar</span><span class="sxs-lookup"><span data-stu-id="23968-138">One last recommendation: Use TRIM with unmanaged standard disks</span></span> 

<span data-ttu-id="23968-139">Om du använder ohanterade standarddiskar (HDD), bör du aktivera TRIMNING.</span><span class="sxs-lookup"><span data-stu-id="23968-139">If you use unmanaged standard disks (HDD), you should enable TRIM.</span></span> <span data-ttu-id="23968-140">TRIMNING ignorerar oanvända block på disken så att endast debiteras du för lagring som du faktiskt använder.</span><span class="sxs-lookup"><span data-stu-id="23968-140">TRIM discards unused blocks on the disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="23968-141">Om du skapar stora filer och ta bort dem kan detta spara på kostnader.</span><span class="sxs-lookup"><span data-stu-id="23968-141">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="23968-142">Du kan köra det här kommandot för att kontrollera TRIM inställningen.</span><span class="sxs-lookup"><span data-stu-id="23968-142">You can run this command to check the TRIM setting.</span></span> <span data-ttu-id="23968-143">Öppna en kommandotolk på din Windows-VM och skriv:</span><span class="sxs-lookup"><span data-stu-id="23968-143">Open a command prompt on your Windows VM and type:</span></span>


```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="23968-144">Om kommandot returnerar 0, är TRIMNING aktiverat på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="23968-144">If the command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="23968-145">Om den returnerar 1, kör du följande kommando för att aktivera TRIMNING:</span><span class="sxs-lookup"><span data-stu-id="23968-145">If it returns 1, run the following command to enable TRIM:</span></span>

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> <span data-ttu-id="23968-146">Obs: Stöd för Trim börjar med Windows Server 2012 / Windows 8 och senare finns i avsnittet [nya API: et kan appar skicka ”rensa och ta bort mappning” tips till lagringsmedia](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).</span><span class="sxs-lookup"><span data-stu-id="23968-146">Note: Trim support starts with Windows Server 2012 / Windows 8 and above, see see [New API allows apps to send "TRIM and Unmap" hints to storage media](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).</span></span>
> 

<!-- Might want to match next-steps from overview of managed disks -->
## <a name="next-steps"></a><span data-ttu-id="23968-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="23968-147">Next steps</span></span>
* <span data-ttu-id="23968-148">[Ansluta en disk](../virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) att lägga till ytterligare lagringsutrymme för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="23968-148">[Attach a disk](../virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) to add additional storage for your VM.</span></span>
* <span data-ttu-id="23968-149">[Ändra enhetsbeteckningen för den tillfälliga disken i Windows](../virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) så att programmet kan använda enheten D: för data.</span><span class="sxs-lookup"><span data-stu-id="23968-149">[Change the drive letter of the Windows temporary disk](../virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) so your application can use the D: drive for data.</span></span>

