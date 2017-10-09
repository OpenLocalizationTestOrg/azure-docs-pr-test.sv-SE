---
title: "aaaAbout diskar och virtuella hårddiskar för virtuella datorer i Microsoft Azure Windows | Microsoft Docs"
description: "Lär dig mer om grundläggande hello av diskar och virtuella hårddiskar för Windows-datorer i Azure."
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
ms.openlocfilehash: 859e564dc74240bd7c70fec33255b8d071c4acf7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="about-disks-and-vhds-for-azure-windows-vms"></a><span data-ttu-id="5c751-103">Om diskar och virtuella hårddiskar för virtuella Azure Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="5c751-103">About disks and VHDs for Azure Windows VMs</span></span>
<span data-ttu-id="5c751-104">Precis som alla andra datorer använda virtuella datorer i Azure diskar som plats-toostore ett operativsystem, program och data.</span><span class="sxs-lookup"><span data-stu-id="5c751-104">Just like any other computer, virtual machines in Azure use disks as a place toostore an operating system, applications, and data.</span></span> <span data-ttu-id="5c751-105">Alla Azure virtuella datorer har minst två diskar – en disk i Windows-operativsystem och en tillfällig disk.</span><span class="sxs-lookup"><span data-stu-id="5c751-105">All Azure virtual machines have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="5c751-106">hello operativsystemdisk skapas från en avbildning och både hello operativsystemdisken och hello avbildningen är virtuella hårddiskar (VHD) lagras i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="5c751-106">hello operating system disk is created from an image, and both hello operating system disk and hello image are virtual hard disks (VHDs) stored in an Azure storage account.</span></span> <span data-ttu-id="5c751-107">Virtuella datorer kan också ha en eller flera datadiskar som lagras också som virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="5c751-107">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span> 

<span data-ttu-id="5c751-108">I den här artikeln ska vi pratar om hello olika användningsområden för hello diskar och diskutera hello olika typer av diskar som du kan skapa och använda.</span><span class="sxs-lookup"><span data-stu-id="5c751-108">In this article, we will talk about hello different uses for hello disks, and then discuss hello different types of disks you can create and use.</span></span> <span data-ttu-id="5c751-109">Den här artikeln är också tillgängligt för [virtuella Linux-datorer](storage-about-disks-and-vhds-linux.md).</span><span class="sxs-lookup"><span data-stu-id="5c751-109">This article is also available for [Linux virtual machines](storage-about-disks-and-vhds-linux.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a><span data-ttu-id="5c751-110">Diskar som används av virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="5c751-110">Disks used by VMs</span></span>

<span data-ttu-id="5c751-111">Låt oss ta en titt på hur hello diskarna används av hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5c751-111">Let's take a look at how hello disks are used by hello VMs.</span></span>

### <a name="operating-system-disk"></a><span data-ttu-id="5c751-112">Operativsystemdisk</span><span class="sxs-lookup"><span data-stu-id="5c751-112">Operating system disk</span></span>
<span data-ttu-id="5c751-113">Varje virtuell dator har en ansluten operativsystemdisk.</span><span class="sxs-lookup"><span data-stu-id="5c751-113">Every virtual machine has one attached operating system disk.</span></span> <span data-ttu-id="5c751-114">Den har registrerats som en SATA-enhet och märkta som hello C:-enheten som standard.</span><span class="sxs-lookup"><span data-stu-id="5c751-114">It's registered as a SATA drive and labeled as hello C: drive by default.</span></span> <span data-ttu-id="5c751-115">Den här disken har en maximal kapacitet på 2 048 gigabyte (GB).</span><span class="sxs-lookup"><span data-stu-id="5c751-115">This disk has a maximum capacity of 2048 gigabytes (GB).</span></span> 

### <a name="temporary-disk"></a><span data-ttu-id="5c751-116">Diskutrymme</span><span class="sxs-lookup"><span data-stu-id="5c751-116">Temporary disk</span></span>
<span data-ttu-id="5c751-117">Varje virtuell dator innehåller en tillfällig disk.</span><span class="sxs-lookup"><span data-stu-id="5c751-117">Each VM contains a temporary disk.</span></span> <span data-ttu-id="5c751-118">hello diskutrymme tillhandahåller kortsiktig lagring för program och processer och är avsedd tooonly lagra data, till exempel page eller växlingen filer.</span><span class="sxs-lookup"><span data-stu-id="5c751-118">hello temporary disk provides short-term storage for applications and processes and is intended tooonly store data such as page or swap files.</span></span> <span data-ttu-id="5c751-119">Data på hello diskutrymme kan gå förlorade under en [underhållshändelse](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) eller när du [distribuera en virtuell dator](../virtual-machines/windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5c751-119">Data on hello temporary disk may be lost during a [maintenance event](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) or when you [redeploy a VM](../virtual-machines/windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="5c751-120">Under en standard omstart av hello VM ska hello data på hello temporärkatalog sparas.</span><span class="sxs-lookup"><span data-stu-id="5c751-120">During a standard reboot of hello VM, hello data on hello temporary drive should persist.</span></span>

<span data-ttu-id="5c751-121">hello diskutrymme är märkt som hello D: enheten som standard och det används för att lagra pagefile.sys.</span><span class="sxs-lookup"><span data-stu-id="5c751-121">hello temporary disk is labeled as hello D: drive by default and it used for storing pagefile.sys.</span></span> <span data-ttu-id="5c751-122">tooremap den här disken tooa annan enhetsbeteckning, se [ändra hello enhetsbeteckningen för hello Windows diskutrymme](../virtual-machines/windows/change-drive-letter.md).</span><span class="sxs-lookup"><span data-stu-id="5c751-122">tooremap this disk tooa different drive letter, see [Change hello drive letter of hello Windows temporary disk](../virtual-machines/windows/change-drive-letter.md).</span></span> <span data-ttu-id="5c751-123">hello storleken på hello diskutrymme varierar utifrån hello storleken på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5c751-123">hello size of hello temporary disk varies, based on hello size of hello virtual machine.</span></span> <span data-ttu-id="5c751-124">Mer information finns i [storlekar för Windows-datorer](../virtual-machines/windows/sizes.md).</span><span class="sxs-lookup"><span data-stu-id="5c751-124">For more information, see [Sizes for Windows virtual machines](../virtual-machines/windows/sizes.md).</span></span>

<span data-ttu-id="5c751-125">Mer information om hur Azure använder hello diskutrymme finns [förstå hello tillfälliga enheten på virtuella datorer i Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="5c751-125">For more information on how Azure uses hello temporary disk, see [Understanding hello temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span></span>


### <a name="data-disk"></a><span data-ttu-id="5c751-126">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5c751-126">Data disk</span></span>
<span data-ttu-id="5c751-127">En datadisk är en virtuell Hårddisk som är bifogade tooa virtuella toostore programdata eller andra data som du behöver tookeep.</span><span class="sxs-lookup"><span data-stu-id="5c751-127">A data disk is a VHD that's attached tooa virtual machine toostore application data, or other data you need tookeep.</span></span> <span data-ttu-id="5c751-128">Datadiskar registreras som SCSI-enheter och är märkta med en bokstav som du väljer.</span><span class="sxs-lookup"><span data-stu-id="5c751-128">Data disks are registered as SCSI drives and are labeled with a letter that you choose.</span></span> <span data-ttu-id="5c751-129">Varje datadisk har en maximal kapacitet för 4095 GB.</span><span class="sxs-lookup"><span data-stu-id="5c751-129">Each data disk has a maximum capacity of 4095 GB.</span></span> <span data-ttu-id="5c751-130">hello storleken på hello virtuella avgör hur många datadiskar som du kan koppla tooit och hello typer av lagring som du kan använda toohost hello diskar.</span><span class="sxs-lookup"><span data-stu-id="5c751-130">hello size of hello virtual machine determines how many data disks you can attach tooit and hello type of storage you can use toohost hello disks.</span></span>

> [!NOTE]
> <span data-ttu-id="5c751-131">Mer information om kapacitet för virtuella datorer finns [storlekar för Windows-datorer](../virtual-machines/windows/sizes.md).</span><span class="sxs-lookup"><span data-stu-id="5c751-131">For more information about virtual machines capacities, see [Sizes for Windows virtual machines](../virtual-machines/windows/sizes.md).</span></span>
> 

<span data-ttu-id="5c751-132">Azure skapar en operativsystemdisk när du skapar en virtuell dator från en avbildning.</span><span class="sxs-lookup"><span data-stu-id="5c751-132">Azure creates an operating system disk when you create a virtual machine from an image.</span></span> <span data-ttu-id="5c751-133">Om du använder en avbildning som innehåller datadiskar skapar Azure även hello datadiskar när hello virtuell dator skapas.</span><span class="sxs-lookup"><span data-stu-id="5c751-133">If you use an image that includes data disks, Azure also creates hello data disks when it creates hello virtual machine.</span></span> <span data-ttu-id="5c751-134">Annars kan du lägga till datadiskar när du skapar hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5c751-134">Otherwise, you add data disks after you create hello virtual machine.</span></span>

<span data-ttu-id="5c751-135">Du kan lägga till data diskar tooa virtuell dator när som helst av **kopplar** hello disk toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5c751-135">You can add data disks tooa virtual machine at any time, by **attaching** hello disk toohello virtual machine.</span></span> <span data-ttu-id="5c751-136">Du kan använda en virtuell Hårddisk som du har laddat upp eller kopieras tooyour storage-konto eller något som Azure skapar åt dig.</span><span class="sxs-lookup"><span data-stu-id="5c751-136">You can use a VHD that you've uploaded or copied tooyour storage account, or one that Azure creates for you.</span></span> <span data-ttu-id="5c751-137">Koppla en datadisk associerar hello VHD-filen med hello VM genom att placera ett lån på hello VHD så den inte kan tas bort från lagring när den är fortfarande kopplad.</span><span class="sxs-lookup"><span data-stu-id="5c751-137">Attaching a data disk associates hello VHD file with hello VM by placing a 'lease' on hello VHD so it can't be deleted from storage while it's still attached.</span></span>


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a><span data-ttu-id="5c751-138">En sista rekommendation: Använd TRIMNING med ohanterad standarddiskar</span><span class="sxs-lookup"><span data-stu-id="5c751-138">One last recommendation: Use TRIM with unmanaged standard disks</span></span> 

<span data-ttu-id="5c751-139">Om du använder ohanterade standarddiskar (HDD), bör du aktivera TRIMNING.</span><span class="sxs-lookup"><span data-stu-id="5c751-139">If you use unmanaged standard disks (HDD), you should enable TRIM.</span></span> <span data-ttu-id="5c751-140">TRIMNING ignorerar oanvända block på disken hello så att endast debiteras du för lagring som du faktiskt använder.</span><span class="sxs-lookup"><span data-stu-id="5c751-140">TRIM discards unused blocks on hello disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="5c751-141">Om du skapar stora filer och ta bort dem kan detta spara på kostnader.</span><span class="sxs-lookup"><span data-stu-id="5c751-141">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="5c751-142">Du kan köra kommandot toocheck hello TRIM inställningen.</span><span class="sxs-lookup"><span data-stu-id="5c751-142">You can run this command toocheck hello TRIM setting.</span></span> <span data-ttu-id="5c751-143">Öppna en kommandotolk på din Windows-VM och skriv:</span><span class="sxs-lookup"><span data-stu-id="5c751-143">Open a command prompt on your Windows VM and type:</span></span>


```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="5c751-144">Om hello kommandot returnerar 0, är TRIMNING aktiverat på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="5c751-144">If hello command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="5c751-145">Om den returnerar 1, kör du följande kommando tooenable TRIMNING hello:</span><span class="sxs-lookup"><span data-stu-id="5c751-145">If it returns 1, run hello following command tooenable TRIM:</span></span>

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> <span data-ttu-id="5c751-146">Obs: Stöd för Trim börjar med Windows Server 2012 / Windows 8 och senare finns i avsnittet [nya API: et kan appar toosend ”rensa och ta bort mappning” tips toostorage media](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).</span><span class="sxs-lookup"><span data-stu-id="5c751-146">Note: Trim support starts with Windows Server 2012 / Windows 8 and above, see see [New API allows apps toosend "TRIM and Unmap" hints toostorage media](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).</span></span>
> 

<!-- Might want toomatch next-steps from overview of managed disks -->
## <a name="next-steps"></a><span data-ttu-id="5c751-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c751-147">Next steps</span></span>
* <span data-ttu-id="5c751-148">[Ansluta en disk](../virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooadd ytterligare lagringsutrymme för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5c751-148">[Attach a disk](../virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooadd additional storage for your VM.</span></span>
* <span data-ttu-id="5c751-149">[Ändra hello enhetsbeteckningen för hello Windows diskutrymme](../virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) så att programmet kan använda hello D: enhet för data.</span><span class="sxs-lookup"><span data-stu-id="5c751-149">[Change hello drive letter of hello Windows temporary disk](../virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) so your application can use hello D: drive for data.</span></span>

