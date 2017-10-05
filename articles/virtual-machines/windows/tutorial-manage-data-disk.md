---
title: Hantera Azure-diskarna med Azure PowerShell | Microsoft Docs
description: "Självstudiekurs – hantera Azure-diskarna med Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 6f1bc9361745adc211f22416a7ba8ac1b8dc614e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-disks-with-powershell"></a><span data-ttu-id="613fb-103">Hantera Azure-diskar med PowerShell</span><span class="sxs-lookup"><span data-stu-id="613fb-103">Manage Azure disks with PowerShell</span></span>

<span data-ttu-id="613fb-104">Virtuella Azure-datorer använder diskar för att lagra virtuella datorer operativsystem, program och data.</span><span class="sxs-lookup"><span data-stu-id="613fb-104">Azure virtual machines use disks to store the VMs operating system, applications, and data.</span></span> <span data-ttu-id="613fb-105">När du skapar en virtuell dator är det viktigt att välja en diskstorleken och konfiguration som är lämpligt att förväntade arbetsbelastningen.</span><span class="sxs-lookup"><span data-stu-id="613fb-105">When creating a VM it is important to choose a disk size and configuration appropriate to the expected workload.</span></span> <span data-ttu-id="613fb-106">Den här kursen ingår distribuerar och hanterar Virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="613fb-106">This tutorial covers deploying and managing VM disks.</span></span> <span data-ttu-id="613fb-107">Du lär dig mer om:</span><span class="sxs-lookup"><span data-stu-id="613fb-107">You learn about:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="613fb-108">OS-diskar och tillfällig diskar</span><span class="sxs-lookup"><span data-stu-id="613fb-108">OS disks and temporary disks</span></span>
> * <span data-ttu-id="613fb-109">Datadiskar</span><span class="sxs-lookup"><span data-stu-id="613fb-109">Data disks</span></span>
> * <span data-ttu-id="613fb-110">Standard- och Premium-diskar</span><span class="sxs-lookup"><span data-stu-id="613fb-110">Standard and Premium disks</span></span>
> * <span data-ttu-id="613fb-111">Diskprestanda</span><span class="sxs-lookup"><span data-stu-id="613fb-111">Disk performance</span></span>
> * <span data-ttu-id="613fb-112">Koppla och förbereda datadiskar</span><span class="sxs-lookup"><span data-stu-id="613fb-112">Attaching and preparing data disks</span></span>

<span data-ttu-id="613fb-113">Den här självstudien kräver Azure PowerShell-modul version 3.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="613fb-113">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="613fb-114">Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="613fb-114">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="613fb-115">Om du behöver uppgradera [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="613fb-115">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="default-azure-disks"></a><span data-ttu-id="613fb-116">Standard Azure-diskar</span><span class="sxs-lookup"><span data-stu-id="613fb-116">Default Azure disks</span></span>

<span data-ttu-id="613fb-117">När en virtuell Azure-dator har skapats är automatiskt två diskar kopplade till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="613fb-117">When an Azure virtual machine is created, two disks are automatically attached to the virtual machine.</span></span> 

<span data-ttu-id="613fb-118">**Operativsystemdisken** -drift systemdiskar kan storleksändras upp till 1 terabyte och är värd för operativsystemet för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="613fb-118">**Operating system disk** - Operating system disks can be sized up to 1 terabyte, and hosts the VMs operating system.</span></span>  <span data-ttu-id="613fb-119">OS-disken har tilldelats en enhetsbokstav för *c:* som standard.</span><span class="sxs-lookup"><span data-stu-id="613fb-119">The OS disk is assigned a drive letter of *c:* by default.</span></span> <span data-ttu-id="613fb-120">Disken cachelagring konfigurationen av OS-disken har optimerats för OS-prestanda.</span><span class="sxs-lookup"><span data-stu-id="613fb-120">The disk caching configuration of the OS disk is optimized for OS performance.</span></span> <span data-ttu-id="613fb-121">OS-disk **bör inte** värd för program eller data.</span><span class="sxs-lookup"><span data-stu-id="613fb-121">The OS disk **should not** host applications or data.</span></span> <span data-ttu-id="613fb-122">Använd en datadisk, vilket beskrivs senare i den här artikeln för program och data.</span><span class="sxs-lookup"><span data-stu-id="613fb-122">For applications and data, use a data disk, which is detailed later in this article.</span></span>

<span data-ttu-id="613fb-123">**Diskutrymme** -tillfälliga diskar använder ett SSD-enhet som finns på samma Azure-värd som den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="613fb-123">**Temporary disk** - Temporary disks use a solid-state drive that is located on the same Azure host as the VM.</span></span> <span data-ttu-id="613fb-124">Temporär diskar har hög performant och kan användas för åtgärder som till exempel temporär databearbetning.</span><span class="sxs-lookup"><span data-stu-id="613fb-124">Temp disks are highly performant and may be used for operations such as temporary data processing.</span></span> <span data-ttu-id="613fb-125">Om den virtuella datorn flyttas till en ny värd bort data som lagrats på en tillfällig disk.</span><span class="sxs-lookup"><span data-stu-id="613fb-125">However, if the VM is moved to a new host, any data stored on a temporary disk is removed.</span></span> <span data-ttu-id="613fb-126">Storleken på den tillfälliga disken bestäms av VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="613fb-126">The size of the temporary disk is determined by the VM size.</span></span> <span data-ttu-id="613fb-127">Tillfällig diskar tilldelas en enhetsbeteckning för *d:* som standard.</span><span class="sxs-lookup"><span data-stu-id="613fb-127">Temporary disks are assigned a drive letter of *d:* by default.</span></span>

### <a name="temporary-disk-sizes"></a><span data-ttu-id="613fb-128">Tillfällig diskstorlekar</span><span class="sxs-lookup"><span data-stu-id="613fb-128">Temporary disk sizes</span></span>

| <span data-ttu-id="613fb-129">Typ</span><span class="sxs-lookup"><span data-stu-id="613fb-129">Type</span></span> | <span data-ttu-id="613fb-130">VM-storlek</span><span class="sxs-lookup"><span data-stu-id="613fb-130">VM Size</span></span> | <span data-ttu-id="613fb-131">Maxstorlek för temporär disk (GB)</span><span class="sxs-lookup"><span data-stu-id="613fb-131">Max temp disk size (GB)</span></span> |
|----|----|----|
| [<span data-ttu-id="613fb-132">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="613fb-132">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="613fb-133">A och D-serien</span><span class="sxs-lookup"><span data-stu-id="613fb-133">A and D series</span></span> | <span data-ttu-id="613fb-134">800</span><span class="sxs-lookup"><span data-stu-id="613fb-134">800</span></span> |
| [<span data-ttu-id="613fb-135">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="613fb-135">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="613fb-136">F-serien</span><span class="sxs-lookup"><span data-stu-id="613fb-136">F series</span></span> | <span data-ttu-id="613fb-137">800</span><span class="sxs-lookup"><span data-stu-id="613fb-137">800</span></span> |
| [<span data-ttu-id="613fb-138">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="613fb-138">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="613fb-139">D och G serien</span><span class="sxs-lookup"><span data-stu-id="613fb-139">D and G series</span></span> | <span data-ttu-id="613fb-140">6144</span><span class="sxs-lookup"><span data-stu-id="613fb-140">6144</span></span> |
| [<span data-ttu-id="613fb-141">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="613fb-141">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="613fb-142">Serie L</span><span class="sxs-lookup"><span data-stu-id="613fb-142">L series</span></span> | <span data-ttu-id="613fb-143">5630</span><span class="sxs-lookup"><span data-stu-id="613fb-143">5630</span></span> |
| [<span data-ttu-id="613fb-144">GPU</span><span class="sxs-lookup"><span data-stu-id="613fb-144">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="613fb-145">N serien</span><span class="sxs-lookup"><span data-stu-id="613fb-145">N series</span></span> | <span data-ttu-id="613fb-146">1440</span><span class="sxs-lookup"><span data-stu-id="613fb-146">1440</span></span> |
| [<span data-ttu-id="613fb-147">Hög prestanda</span><span class="sxs-lookup"><span data-stu-id="613fb-147">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="613fb-148">A och H serien</span><span class="sxs-lookup"><span data-stu-id="613fb-148">A and H series</span></span> | <span data-ttu-id="613fb-149">2000</span><span class="sxs-lookup"><span data-stu-id="613fb-149">2000</span></span> |

## <a name="azure-data-disks"></a><span data-ttu-id="613fb-150">Azure datadiskar</span><span class="sxs-lookup"><span data-stu-id="613fb-150">Azure data disks</span></span>

<span data-ttu-id="613fb-151">Ytterligare datadiskar kan läggas till för att installera program och lagra data.</span><span class="sxs-lookup"><span data-stu-id="613fb-151">Additional data disks can be added for installing applications and storing data.</span></span> <span data-ttu-id="613fb-152">Datadiskar som ska användas i en situation där varaktiga och känslig datalagring önskas.</span><span class="sxs-lookup"><span data-stu-id="613fb-152">Data disks should be used in any situation where durable and responsive data storage is desired.</span></span> <span data-ttu-id="613fb-153">Varje datadisk har en maximal kapacitet för 1 terabyte.</span><span class="sxs-lookup"><span data-stu-id="613fb-153">Each data disk has a maximum capacity of 1 terabyte.</span></span> <span data-ttu-id="613fb-154">Storleken på den virtuella datorn avgör hur många datadiskar som kan kopplas till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="613fb-154">The size of the virtual machine determines how many data disks can be attached to a VM.</span></span> <span data-ttu-id="613fb-155">För varje VM-core kan två diskar kopplas.</span><span class="sxs-lookup"><span data-stu-id="613fb-155">For each VM core, two data disks can be attached.</span></span> 

### <a name="max-data-disks-per-vm"></a><span data-ttu-id="613fb-156">Maximalt antal datadiskar per VM</span><span class="sxs-lookup"><span data-stu-id="613fb-156">Max data disks per VM</span></span>

| <span data-ttu-id="613fb-157">Typ</span><span class="sxs-lookup"><span data-stu-id="613fb-157">Type</span></span> | <span data-ttu-id="613fb-158">VM-storlek</span><span class="sxs-lookup"><span data-stu-id="613fb-158">VM Size</span></span> | <span data-ttu-id="613fb-159">Maximalt antal datadiskar per VM</span><span class="sxs-lookup"><span data-stu-id="613fb-159">Max data disks per VM</span></span> |
|----|----|----|
| [<span data-ttu-id="613fb-160">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="613fb-160">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="613fb-161">A och D-serien</span><span class="sxs-lookup"><span data-stu-id="613fb-161">A and D series</span></span> | <span data-ttu-id="613fb-162">32</span><span class="sxs-lookup"><span data-stu-id="613fb-162">32</span></span> |
| [<span data-ttu-id="613fb-163">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="613fb-163">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="613fb-164">F-serien</span><span class="sxs-lookup"><span data-stu-id="613fb-164">F series</span></span> | <span data-ttu-id="613fb-165">32</span><span class="sxs-lookup"><span data-stu-id="613fb-165">32</span></span> |
| [<span data-ttu-id="613fb-166">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="613fb-166">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="613fb-167">D och G serien</span><span class="sxs-lookup"><span data-stu-id="613fb-167">D and G series</span></span> | <span data-ttu-id="613fb-168">64</span><span class="sxs-lookup"><span data-stu-id="613fb-168">64</span></span> |
| [<span data-ttu-id="613fb-169">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="613fb-169">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="613fb-170">Serie L</span><span class="sxs-lookup"><span data-stu-id="613fb-170">L series</span></span> | <span data-ttu-id="613fb-171">64</span><span class="sxs-lookup"><span data-stu-id="613fb-171">64</span></span> |
| [<span data-ttu-id="613fb-172">GPU</span><span class="sxs-lookup"><span data-stu-id="613fb-172">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="613fb-173">N serien</span><span class="sxs-lookup"><span data-stu-id="613fb-173">N series</span></span> | <span data-ttu-id="613fb-174">48</span><span class="sxs-lookup"><span data-stu-id="613fb-174">48</span></span> |
| [<span data-ttu-id="613fb-175">Hög prestanda</span><span class="sxs-lookup"><span data-stu-id="613fb-175">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="613fb-176">A och H serien</span><span class="sxs-lookup"><span data-stu-id="613fb-176">A and H series</span></span> | <span data-ttu-id="613fb-177">32</span><span class="sxs-lookup"><span data-stu-id="613fb-177">32</span></span> |

## <a name="vm-disk-types"></a><span data-ttu-id="613fb-178">VM-disktyper</span><span class="sxs-lookup"><span data-stu-id="613fb-178">VM disk types</span></span>

<span data-ttu-id="613fb-179">Azure tillhandahåller två typer av disk.</span><span class="sxs-lookup"><span data-stu-id="613fb-179">Azure provides two types of disk.</span></span>

### <a name="standard-disk"></a><span data-ttu-id="613fb-180">Standard disk</span><span class="sxs-lookup"><span data-stu-id="613fb-180">Standard disk</span></span>

<span data-ttu-id="613fb-181">Standard Storage stöds av hårddiskar och levererar kostnadseffektiv lagring samtidigt som det är högpresterande.</span><span class="sxs-lookup"><span data-stu-id="613fb-181">Standard Storage is backed by HDDs, and delivers cost-effective storage while still being performant.</span></span> <span data-ttu-id="613fb-182">Standarddiskar är idealiskt för ett kostnadseffektivt dev och testa arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="613fb-182">Standard disks are ideal for a cost effective dev and test workload.</span></span>

### <a name="premium-disk"></a><span data-ttu-id="613fb-183">Premium-disk</span><span class="sxs-lookup"><span data-stu-id="613fb-183">Premium disk</span></span>

<span data-ttu-id="613fb-184">Premiumdiskar backas upp av SSD-baserad hög prestanda, låg latens disk.</span><span class="sxs-lookup"><span data-stu-id="613fb-184">Premium disks are backed by SSD-based high-performance, low-latency disk.</span></span> <span data-ttu-id="613fb-185">Perfekt för virtuella datorer som kör produktion arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="613fb-185">Perfect for VMs running production workload.</span></span> <span data-ttu-id="613fb-186">Premium-lagring stöder DS-serien, DSv2-serien GS-serien och FS-serien virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="613fb-186">Premium Storage supports DS-series, DSv2-series, GS-series, and FS-series VMs.</span></span> <span data-ttu-id="613fb-187">Premiumdiskar finns i tre olika typer (P10 P20, P30) och storleken på disken fastställer typ av disk.</span><span class="sxs-lookup"><span data-stu-id="613fb-187">Premium disks come in three types (P10, P20, P30), the size of the disk determines the disk type.</span></span> <span data-ttu-id="613fb-188">När du väljer, avrundat diskstorleken värdet till nästa typen.</span><span class="sxs-lookup"><span data-stu-id="613fb-188">When selecting, a disk size the value is rounded up to the next type.</span></span> <span data-ttu-id="613fb-189">Till exempel om storleken är lägre än 128 GB blir disktyp P10, mellan 129 och 512 P20 och över 512 P30.</span><span class="sxs-lookup"><span data-stu-id="613fb-189">For example, if the size is below 128 GB the disk type will be P10, between 129 and 512 P20, and over 512 P30.</span></span> 

### <a name="premium-disk-performance"></a><span data-ttu-id="613fb-190">Premium-diskprestanda</span><span class="sxs-lookup"><span data-stu-id="613fb-190">Premium disk performance</span></span>

|<span data-ttu-id="613fb-191">Premium-lagring disktyp</span><span class="sxs-lookup"><span data-stu-id="613fb-191">Premium storage disk type</span></span> | <span data-ttu-id="613fb-192">P10</span><span class="sxs-lookup"><span data-stu-id="613fb-192">P10</span></span> | <span data-ttu-id="613fb-193">P20</span><span class="sxs-lookup"><span data-stu-id="613fb-193">P20</span></span> | <span data-ttu-id="613fb-194">P30</span><span class="sxs-lookup"><span data-stu-id="613fb-194">P30</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="613fb-195">Diskens storlek (avrunda uppåt)</span><span class="sxs-lookup"><span data-stu-id="613fb-195">Disk size (round up)</span></span> | <span data-ttu-id="613fb-196">128 GB</span><span class="sxs-lookup"><span data-stu-id="613fb-196">128 GB</span></span> | <span data-ttu-id="613fb-197">512 GB</span><span class="sxs-lookup"><span data-stu-id="613fb-197">512 GB</span></span> | <span data-ttu-id="613fb-198">1 024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="613fb-198">1,024 GB (1 TB)</span></span> |
| <span data-ttu-id="613fb-199">IOPS per disk</span><span class="sxs-lookup"><span data-stu-id="613fb-199">IOPS per disk</span></span> | <span data-ttu-id="613fb-200">500</span><span class="sxs-lookup"><span data-stu-id="613fb-200">500</span></span> | <span data-ttu-id="613fb-201">2,300</span><span class="sxs-lookup"><span data-stu-id="613fb-201">2,300</span></span> | <span data-ttu-id="613fb-202">5,000</span><span class="sxs-lookup"><span data-stu-id="613fb-202">5,000</span></span> |
<span data-ttu-id="613fb-203">Dataflöde per disk</span><span class="sxs-lookup"><span data-stu-id="613fb-203">Throughput per disk</span></span> | <span data-ttu-id="613fb-204">100 MB/s</span><span class="sxs-lookup"><span data-stu-id="613fb-204">100 MB/s</span></span> | <span data-ttu-id="613fb-205">150 MB/s</span><span class="sxs-lookup"><span data-stu-id="613fb-205">150 MB/s</span></span> | <span data-ttu-id="613fb-206">200 MB/s</span><span class="sxs-lookup"><span data-stu-id="613fb-206">200 MB/s</span></span> |

<span data-ttu-id="613fb-207">När tabellen ovan identifierar högsta IOPS per disk, kan en högre nivå av prestanda uppnås genom striping flera datadiskar.</span><span class="sxs-lookup"><span data-stu-id="613fb-207">While the above table identifies max IOPS per disk, a higher level of performance can be achieved by striping multiple data disks.</span></span> <span data-ttu-id="613fb-208">Till exempel kan 64 diskar kopplas till Standard_GS5 VM.</span><span class="sxs-lookup"><span data-stu-id="613fb-208">For instance, 64 data disks can be attached to Standard_GS5 VM.</span></span> <span data-ttu-id="613fb-209">Om var och en av dessa diskar har storlek som en P30 kan högst 80 000 IOPS uppnås.</span><span class="sxs-lookup"><span data-stu-id="613fb-209">If each of these disks are sized as a P30, a maximum of 80,000 IOPS can be achieved.</span></span> <span data-ttu-id="613fb-210">Detaljerad information om högsta IOPS per VM finns [Linux VM-storlekar](./sizes.md).</span><span class="sxs-lookup"><span data-stu-id="613fb-210">For detailed information on max IOPS per VM, see [Linux VM sizes](./sizes.md).</span></span>

## <a name="create-and-attach-disks"></a><span data-ttu-id="613fb-211">Skapa och koppla diskar</span><span class="sxs-lookup"><span data-stu-id="613fb-211">Create and attach disks</span></span>

<span data-ttu-id="613fb-212">Du måste ha en befintlig virtuell dator för att slutföra exemplet i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="613fb-212">To complete the example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="613fb-213">Om det behövs, detta [skriptexempel](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) kan skapa en åt dig.</span><span class="sxs-lookup"><span data-stu-id="613fb-213">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="613fb-214">När gå igenom kursen, ersätter namn resursgrupp och VM där det behövs.</span><span class="sxs-lookup"><span data-stu-id="613fb-214">When working through the tutorial, replace the resource group and VM names where needed.</span></span>

<span data-ttu-id="613fb-215">Skapa den första konfigurationen med [ny AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig).</span><span class="sxs-lookup"><span data-stu-id="613fb-215">Create the initial configuration with [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig).</span></span> <span data-ttu-id="613fb-216">I följande exempel konfigureras en disk som är 128 GB i storlek.</span><span class="sxs-lookup"><span data-stu-id="613fb-216">The following example configures a disk that is 128 gigabytes in size.</span></span>

```powershell
$diskConfig = New-AzureRmDiskConfig -Location EastUS -CreateOption Empty -DiskSizeGB 128
```

<span data-ttu-id="613fb-217">Skapa datadisk med den [ny AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) kommando.</span><span class="sxs-lookup"><span data-stu-id="613fb-217">Create the data disk with the [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) command.</span></span>

```powershell
$dataDisk = New-AzureRmDisk -ResourceGroupName myResourceGroup -DiskName myDataDisk -Disk $diskConfig
```

<span data-ttu-id="613fb-218">Hämta den virtuella dator som du vill lägga till datadisk för med den [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) kommando.</span><span class="sxs-lookup"><span data-stu-id="613fb-218">Get the virtual machine that you want to add the data disk to with the [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) command.</span></span>

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

<span data-ttu-id="613fb-219">Lägg till datadisk i konfigurationen av virtuella datorn med den [Lägg till AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) kommando.</span><span class="sxs-lookup"><span data-stu-id="613fb-219">Add the data disk to the virtual machine configuration with the [Add-AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) command.</span></span>

```powershell
$vm = Add-AzureRmVMDataDisk -VM $vm -Name myDataDisk -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 1
```

<span data-ttu-id="613fb-220">Uppdatera den virtuella datorn med den [uppdatering AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) kommando.</span><span class="sxs-lookup"><span data-stu-id="613fb-220">Update the virtual machine with the [Update-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) command.</span></span>

```powershell
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm
```

## <a name="prepare-data-disks"></a><span data-ttu-id="613fb-221">Förbereda datadiskar</span><span class="sxs-lookup"><span data-stu-id="613fb-221">Prepare data disks</span></span>

<span data-ttu-id="613fb-222">När en disk har kopplats till den virtuella datorn, måste operativsystemet konfigureras för att använda disken.</span><span class="sxs-lookup"><span data-stu-id="613fb-222">Once a disk has been attached to the virtual machine, the operating system needs to be configured to use the disk.</span></span> <span data-ttu-id="613fb-223">I följande exempel visas hur du konfigurerar den första disk som lagts till i den virtuella datorn manuellt.</span><span class="sxs-lookup"><span data-stu-id="613fb-223">The following example shows how to manually configure the first disk added to the VM.</span></span> <span data-ttu-id="613fb-224">Den här processen kan också automatiseras med hjälp av den [tillägget för anpassat skript](./tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="613fb-224">This process can also be automated using the [custom script extension](./tutorial-automate-vm-deployment.md).</span></span>

### <a name="manual-configuration"></a><span data-ttu-id="613fb-225">Manuell konfiguration</span><span class="sxs-lookup"><span data-stu-id="613fb-225">Manual configuration</span></span>

<span data-ttu-id="613fb-226">Skapa en RDP-anslutning med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="613fb-226">Create an RDP connection with the virtual machine.</span></span> <span data-ttu-id="613fb-227">Öppna PowerShell och kör det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="613fb-227">Open up PowerShell and run this script.</span></span>

```powershell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a><span data-ttu-id="613fb-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="613fb-228">Next steps</span></span>

<span data-ttu-id="613fb-229">I kursen får du lärt dig om Virtuella diskar ämnen som:</span><span class="sxs-lookup"><span data-stu-id="613fb-229">In this tutorial, you learned about VM disks topics such as:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="613fb-230">OS-diskar och tillfällig diskar</span><span class="sxs-lookup"><span data-stu-id="613fb-230">OS disks and temporary disks</span></span>
> * <span data-ttu-id="613fb-231">Datadiskar</span><span class="sxs-lookup"><span data-stu-id="613fb-231">Data disks</span></span>
> * <span data-ttu-id="613fb-232">Standard- och Premium-diskar</span><span class="sxs-lookup"><span data-stu-id="613fb-232">Standard and Premium disks</span></span>
> * <span data-ttu-id="613fb-233">Diskprestanda</span><span class="sxs-lookup"><span data-stu-id="613fb-233">Disk performance</span></span>
> * <span data-ttu-id="613fb-234">Koppla och förbereda datadiskar</span><span class="sxs-lookup"><span data-stu-id="613fb-234">Attaching and preparing data disks</span></span>

<span data-ttu-id="613fb-235">Gå vidare till nästa kurs mer information om hur du automatiserar VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="613fb-235">Advance to the next tutorial to learn about automating VM configuration.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="613fb-236">Automatisera konfiguration av virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="613fb-236">Automate VM configuration</span></span>](./tutorial-automate-vm-deployment.md)
