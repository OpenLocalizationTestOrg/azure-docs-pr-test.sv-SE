---
title: aaaManage Azure diskar med hello Azure PowerShell | Microsoft Docs
description: "Självstudiekurs – hantera Azure-diskarna med hello Azure PowerShell"
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
ms.openlocfilehash: 2f61ad18bc94bab527d7ae593da603c6073adc89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-disks-with-powershell"></a><span data-ttu-id="fe035-103">Hantera Azure-diskar med PowerShell</span><span class="sxs-lookup"><span data-stu-id="fe035-103">Manage Azure disks with PowerShell</span></span>

<span data-ttu-id="fe035-104">Azure virtual machines använder diskar toostore hello virtuella datorer, operativsystem, program och data.</span><span class="sxs-lookup"><span data-stu-id="fe035-104">Azure virtual machines use disks toostore hello VMs operating system, applications, and data.</span></span> <span data-ttu-id="fe035-105">När du skapar en virtuell dator är det viktigt toochoose storleken för en disk och konfiguration lämpliga toohello förväntades arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="fe035-105">When creating a VM it is important toochoose a disk size and configuration appropriate toohello expected workload.</span></span> <span data-ttu-id="fe035-106">Den här kursen ingår distribuerar och hanterar Virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="fe035-106">This tutorial covers deploying and managing VM disks.</span></span> <span data-ttu-id="fe035-107">Du lär dig mer om:</span><span class="sxs-lookup"><span data-stu-id="fe035-107">You learn about:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fe035-108">OS-diskar och tillfällig diskar</span><span class="sxs-lookup"><span data-stu-id="fe035-108">OS disks and temporary disks</span></span>
> * <span data-ttu-id="fe035-109">Datadiskar</span><span class="sxs-lookup"><span data-stu-id="fe035-109">Data disks</span></span>
> * <span data-ttu-id="fe035-110">Standard- och Premium-diskar</span><span class="sxs-lookup"><span data-stu-id="fe035-110">Standard and Premium disks</span></span>
> * <span data-ttu-id="fe035-111">Diskprestanda</span><span class="sxs-lookup"><span data-stu-id="fe035-111">Disk performance</span></span>
> * <span data-ttu-id="fe035-112">Koppla och förbereda datadiskar</span><span class="sxs-lookup"><span data-stu-id="fe035-112">Attaching and preparing data disks</span></span>

<span data-ttu-id="fe035-113">Den här kursen kräver hello Azure PowerShell module 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="fe035-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="fe035-114">Kör ` Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="fe035-114">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="fe035-115">Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="fe035-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="default-azure-disks"></a><span data-ttu-id="fe035-116">Standard Azure-diskar</span><span class="sxs-lookup"><span data-stu-id="fe035-116">Default Azure disks</span></span>

<span data-ttu-id="fe035-117">När en virtuell dator i Azure skapas är två diskar automatiskt bifogade toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="fe035-117">When an Azure virtual machine is created, two disks are automatically attached toohello virtual machine.</span></span> 

<span data-ttu-id="fe035-118">**Operativsystemdisken** - operativsystemet diskar kan storleksändras in too1 terabyte och värdar hello operativsystemet för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="fe035-118">**Operating system disk** - Operating system disks can be sized up too1 terabyte, and hosts hello VMs operating system.</span></span>  <span data-ttu-id="fe035-119">hello OS-disken har kopplats enhetsbeteckningen för *c:* som standard.</span><span class="sxs-lookup"><span data-stu-id="fe035-119">hello OS disk is assigned a drive letter of *c:* by default.</span></span> <span data-ttu-id="fe035-120">hello disk cachelagring konfigurationen av hello OS-disken har optimerats för OS-prestanda.</span><span class="sxs-lookup"><span data-stu-id="fe035-120">hello disk caching configuration of hello OS disk is optimized for OS performance.</span></span> <span data-ttu-id="fe035-121">hello OS-disk **bör inte** värd för program eller data.</span><span class="sxs-lookup"><span data-stu-id="fe035-121">hello OS disk **should not** host applications or data.</span></span> <span data-ttu-id="fe035-122">Använd en datadisk, vilket beskrivs senare i den här artikeln för program och data.</span><span class="sxs-lookup"><span data-stu-id="fe035-122">For applications and data, use a data disk, which is detailed later in this article.</span></span>

<span data-ttu-id="fe035-123">**Diskutrymme** -tillfälliga diskar använder ett SSD-enhet som finns på hello samma Azure-värd som hello VM.</span><span class="sxs-lookup"><span data-stu-id="fe035-123">**Temporary disk** - Temporary disks use a solid-state drive that is located on hello same Azure host as hello VM.</span></span> <span data-ttu-id="fe035-124">Temporär diskar har hög performant och kan användas för åtgärder som till exempel temporär databearbetning.</span><span class="sxs-lookup"><span data-stu-id="fe035-124">Temp disks are highly performant and may be used for operations such as temporary data processing.</span></span> <span data-ttu-id="fe035-125">Om hello VM är flyttade tooa nya värden kan bort data som lagrats på en tillfällig disk.</span><span class="sxs-lookup"><span data-stu-id="fe035-125">However, if hello VM is moved tooa new host, any data stored on a temporary disk is removed.</span></span> <span data-ttu-id="fe035-126">hello hello tillfälliga diskens storlek bestäms av hello VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="fe035-126">hello size of hello temporary disk is determined by hello VM size.</span></span> <span data-ttu-id="fe035-127">Tillfällig diskar tilldelas en enhetsbeteckning för *d:* som standard.</span><span class="sxs-lookup"><span data-stu-id="fe035-127">Temporary disks are assigned a drive letter of *d:* by default.</span></span>

### <a name="temporary-disk-sizes"></a><span data-ttu-id="fe035-128">Tillfällig diskstorlekar</span><span class="sxs-lookup"><span data-stu-id="fe035-128">Temporary disk sizes</span></span>

| <span data-ttu-id="fe035-129">Typ</span><span class="sxs-lookup"><span data-stu-id="fe035-129">Type</span></span> | <span data-ttu-id="fe035-130">VM-storlek</span><span class="sxs-lookup"><span data-stu-id="fe035-130">VM Size</span></span> | <span data-ttu-id="fe035-131">Maxstorlek för temporär disk (GB)</span><span class="sxs-lookup"><span data-stu-id="fe035-131">Max temp disk size (GB)</span></span> |
|----|----|----|
| [<span data-ttu-id="fe035-132">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="fe035-132">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="fe035-133">A och D-serien</span><span class="sxs-lookup"><span data-stu-id="fe035-133">A and D series</span></span> | <span data-ttu-id="fe035-134">800</span><span class="sxs-lookup"><span data-stu-id="fe035-134">800</span></span> |
| [<span data-ttu-id="fe035-135">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="fe035-135">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="fe035-136">F-serien</span><span class="sxs-lookup"><span data-stu-id="fe035-136">F series</span></span> | <span data-ttu-id="fe035-137">800</span><span class="sxs-lookup"><span data-stu-id="fe035-137">800</span></span> |
| [<span data-ttu-id="fe035-138">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="fe035-138">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="fe035-139">D och G serien</span><span class="sxs-lookup"><span data-stu-id="fe035-139">D and G series</span></span> | <span data-ttu-id="fe035-140">6144</span><span class="sxs-lookup"><span data-stu-id="fe035-140">6144</span></span> |
| [<span data-ttu-id="fe035-141">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="fe035-141">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="fe035-142">Serie L</span><span class="sxs-lookup"><span data-stu-id="fe035-142">L series</span></span> | <span data-ttu-id="fe035-143">5630</span><span class="sxs-lookup"><span data-stu-id="fe035-143">5630</span></span> |
| [<span data-ttu-id="fe035-144">GPU</span><span class="sxs-lookup"><span data-stu-id="fe035-144">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="fe035-145">N serien</span><span class="sxs-lookup"><span data-stu-id="fe035-145">N series</span></span> | <span data-ttu-id="fe035-146">1440</span><span class="sxs-lookup"><span data-stu-id="fe035-146">1440</span></span> |
| [<span data-ttu-id="fe035-147">Hög prestanda</span><span class="sxs-lookup"><span data-stu-id="fe035-147">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="fe035-148">A och H serien</span><span class="sxs-lookup"><span data-stu-id="fe035-148">A and H series</span></span> | <span data-ttu-id="fe035-149">2000</span><span class="sxs-lookup"><span data-stu-id="fe035-149">2000</span></span> |

## <a name="azure-data-disks"></a><span data-ttu-id="fe035-150">Azure datadiskar</span><span class="sxs-lookup"><span data-stu-id="fe035-150">Azure data disks</span></span>

<span data-ttu-id="fe035-151">Ytterligare datadiskar kan läggas till för att installera program och lagra data.</span><span class="sxs-lookup"><span data-stu-id="fe035-151">Additional data disks can be added for installing applications and storing data.</span></span> <span data-ttu-id="fe035-152">Datadiskar som ska användas i en situation där varaktiga och känslig datalagring önskas.</span><span class="sxs-lookup"><span data-stu-id="fe035-152">Data disks should be used in any situation where durable and responsive data storage is desired.</span></span> <span data-ttu-id="fe035-153">Varje datadisk har en maximal kapacitet för 1 terabyte.</span><span class="sxs-lookup"><span data-stu-id="fe035-153">Each data disk has a maximum capacity of 1 terabyte.</span></span> <span data-ttu-id="fe035-154">hello storleken på hello virtuella datorn anger hur många datadiskar kan vara anslutna tooa VM.</span><span class="sxs-lookup"><span data-stu-id="fe035-154">hello size of hello virtual machine determines how many data disks can be attached tooa VM.</span></span> <span data-ttu-id="fe035-155">För varje VM-core kan två diskar kopplas.</span><span class="sxs-lookup"><span data-stu-id="fe035-155">For each VM core, two data disks can be attached.</span></span> 

### <a name="max-data-disks-per-vm"></a><span data-ttu-id="fe035-156">Maximalt antal datadiskar per VM</span><span class="sxs-lookup"><span data-stu-id="fe035-156">Max data disks per VM</span></span>

| <span data-ttu-id="fe035-157">Typ</span><span class="sxs-lookup"><span data-stu-id="fe035-157">Type</span></span> | <span data-ttu-id="fe035-158">VM-storlek</span><span class="sxs-lookup"><span data-stu-id="fe035-158">VM Size</span></span> | <span data-ttu-id="fe035-159">Maximalt antal datadiskar per VM</span><span class="sxs-lookup"><span data-stu-id="fe035-159">Max data disks per VM</span></span> |
|----|----|----|
| [<span data-ttu-id="fe035-160">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="fe035-160">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="fe035-161">A och D-serien</span><span class="sxs-lookup"><span data-stu-id="fe035-161">A and D series</span></span> | <span data-ttu-id="fe035-162">32</span><span class="sxs-lookup"><span data-stu-id="fe035-162">32</span></span> |
| [<span data-ttu-id="fe035-163">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="fe035-163">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="fe035-164">F-serien</span><span class="sxs-lookup"><span data-stu-id="fe035-164">F series</span></span> | <span data-ttu-id="fe035-165">32</span><span class="sxs-lookup"><span data-stu-id="fe035-165">32</span></span> |
| [<span data-ttu-id="fe035-166">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="fe035-166">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="fe035-167">D och G serien</span><span class="sxs-lookup"><span data-stu-id="fe035-167">D and G series</span></span> | <span data-ttu-id="fe035-168">64</span><span class="sxs-lookup"><span data-stu-id="fe035-168">64</span></span> |
| [<span data-ttu-id="fe035-169">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="fe035-169">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="fe035-170">Serie L</span><span class="sxs-lookup"><span data-stu-id="fe035-170">L series</span></span> | <span data-ttu-id="fe035-171">64</span><span class="sxs-lookup"><span data-stu-id="fe035-171">64</span></span> |
| [<span data-ttu-id="fe035-172">GPU</span><span class="sxs-lookup"><span data-stu-id="fe035-172">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="fe035-173">N serien</span><span class="sxs-lookup"><span data-stu-id="fe035-173">N series</span></span> | <span data-ttu-id="fe035-174">48</span><span class="sxs-lookup"><span data-stu-id="fe035-174">48</span></span> |
| [<span data-ttu-id="fe035-175">Hög prestanda</span><span class="sxs-lookup"><span data-stu-id="fe035-175">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="fe035-176">A och H serien</span><span class="sxs-lookup"><span data-stu-id="fe035-176">A and H series</span></span> | <span data-ttu-id="fe035-177">32</span><span class="sxs-lookup"><span data-stu-id="fe035-177">32</span></span> |

## <a name="vm-disk-types"></a><span data-ttu-id="fe035-178">VM-disktyper</span><span class="sxs-lookup"><span data-stu-id="fe035-178">VM disk types</span></span>

<span data-ttu-id="fe035-179">Azure tillhandahåller två typer av disk.</span><span class="sxs-lookup"><span data-stu-id="fe035-179">Azure provides two types of disk.</span></span>

### <a name="standard-disk"></a><span data-ttu-id="fe035-180">Standard disk</span><span class="sxs-lookup"><span data-stu-id="fe035-180">Standard disk</span></span>

<span data-ttu-id="fe035-181">Standard Storage stöds av hårddiskar och levererar kostnadseffektiv lagring samtidigt som det är högpresterande.</span><span class="sxs-lookup"><span data-stu-id="fe035-181">Standard Storage is backed by HDDs, and delivers cost-effective storage while still being performant.</span></span> <span data-ttu-id="fe035-182">Standarddiskar är idealiskt för ett kostnadseffektivt dev och testa arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="fe035-182">Standard disks are ideal for a cost effective dev and test workload.</span></span>

### <a name="premium-disk"></a><span data-ttu-id="fe035-183">Premium-disk</span><span class="sxs-lookup"><span data-stu-id="fe035-183">Premium disk</span></span>

<span data-ttu-id="fe035-184">Premiumdiskar backas upp av SSD-baserad hög prestanda, låg latens disk.</span><span class="sxs-lookup"><span data-stu-id="fe035-184">Premium disks are backed by SSD-based high-performance, low-latency disk.</span></span> <span data-ttu-id="fe035-185">Perfekt för virtuella datorer som kör produktion arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="fe035-185">Perfect for VMs running production workload.</span></span> <span data-ttu-id="fe035-186">Premium-lagring stöder DS-serien, DSv2-serien GS-serien och FS-serien virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="fe035-186">Premium Storage supports DS-series, DSv2-series, GS-series, and FS-series VMs.</span></span> <span data-ttu-id="fe035-187">Premiumdiskar finns i tre olika typer (P10 P20, P30), hello hello diskens storlek avgör hello disktyp.</span><span class="sxs-lookup"><span data-stu-id="fe035-187">Premium disks come in three types (P10, P20, P30), hello size of hello disk determines hello disk type.</span></span> <span data-ttu-id="fe035-188">När du väljer, avrundat ett värde för disk hello toohello nästa typen.</span><span class="sxs-lookup"><span data-stu-id="fe035-188">When selecting, a disk size hello value is rounded up toohello next type.</span></span> <span data-ttu-id="fe035-189">Till exempel om hello storlek är lägre än 128 GB hello disktyp blir P10, mellan 129 och 512 P20 och över 512 P30.</span><span class="sxs-lookup"><span data-stu-id="fe035-189">For example, if hello size is below 128 GB hello disk type will be P10, between 129 and 512 P20, and over 512 P30.</span></span> 

### <a name="premium-disk-performance"></a><span data-ttu-id="fe035-190">Premium-diskprestanda</span><span class="sxs-lookup"><span data-stu-id="fe035-190">Premium disk performance</span></span>

|<span data-ttu-id="fe035-191">Premium-lagring disktyp</span><span class="sxs-lookup"><span data-stu-id="fe035-191">Premium storage disk type</span></span> | <span data-ttu-id="fe035-192">P10</span><span class="sxs-lookup"><span data-stu-id="fe035-192">P10</span></span> | <span data-ttu-id="fe035-193">P20</span><span class="sxs-lookup"><span data-stu-id="fe035-193">P20</span></span> | <span data-ttu-id="fe035-194">P30</span><span class="sxs-lookup"><span data-stu-id="fe035-194">P30</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="fe035-195">Diskens storlek (avrunda uppåt)</span><span class="sxs-lookup"><span data-stu-id="fe035-195">Disk size (round up)</span></span> | <span data-ttu-id="fe035-196">128 GB</span><span class="sxs-lookup"><span data-stu-id="fe035-196">128 GB</span></span> | <span data-ttu-id="fe035-197">512 GB</span><span class="sxs-lookup"><span data-stu-id="fe035-197">512 GB</span></span> | <span data-ttu-id="fe035-198">1 024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="fe035-198">1,024 GB (1 TB)</span></span> |
| <span data-ttu-id="fe035-199">IOPS per disk</span><span class="sxs-lookup"><span data-stu-id="fe035-199">IOPS per disk</span></span> | <span data-ttu-id="fe035-200">500</span><span class="sxs-lookup"><span data-stu-id="fe035-200">500</span></span> | <span data-ttu-id="fe035-201">2,300</span><span class="sxs-lookup"><span data-stu-id="fe035-201">2,300</span></span> | <span data-ttu-id="fe035-202">5,000</span><span class="sxs-lookup"><span data-stu-id="fe035-202">5,000</span></span> |
<span data-ttu-id="fe035-203">Dataflöde per disk</span><span class="sxs-lookup"><span data-stu-id="fe035-203">Throughput per disk</span></span> | <span data-ttu-id="fe035-204">100 MB/s</span><span class="sxs-lookup"><span data-stu-id="fe035-204">100 MB/s</span></span> | <span data-ttu-id="fe035-205">150 MB/s</span><span class="sxs-lookup"><span data-stu-id="fe035-205">150 MB/s</span></span> | <span data-ttu-id="fe035-206">200 MB/s</span><span class="sxs-lookup"><span data-stu-id="fe035-206">200 MB/s</span></span> |

<span data-ttu-id="fe035-207">Medan hello ovanför tabell identifierar högsta IOPS per disk, kan en högre nivå av prestanda uppnås genom striping flera datadiskar.</span><span class="sxs-lookup"><span data-stu-id="fe035-207">While hello above table identifies max IOPS per disk, a higher level of performance can be achieved by striping multiple data disks.</span></span> <span data-ttu-id="fe035-208">64 data diskar kan vara kopplad till exempel tooStandard_GS5 VM.</span><span class="sxs-lookup"><span data-stu-id="fe035-208">For instance, 64 data disks can be attached tooStandard_GS5 VM.</span></span> <span data-ttu-id="fe035-209">Om var och en av dessa diskar har storlek som en P30 kan högst 80 000 IOPS uppnås.</span><span class="sxs-lookup"><span data-stu-id="fe035-209">If each of these disks are sized as a P30, a maximum of 80,000 IOPS can be achieved.</span></span> <span data-ttu-id="fe035-210">Detaljerad information om högsta IOPS per VM finns [Linux VM-storlekar](./sizes.md).</span><span class="sxs-lookup"><span data-stu-id="fe035-210">For detailed information on max IOPS per VM, see [Linux VM sizes](./sizes.md).</span></span>

## <a name="create-and-attach-disks"></a><span data-ttu-id="fe035-211">Skapa och koppla diskar</span><span class="sxs-lookup"><span data-stu-id="fe035-211">Create and attach disks</span></span>

<span data-ttu-id="fe035-212">toocomplete hello exempel i den här självstudiekursen, måste du ha en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="fe035-212">toocomplete hello example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="fe035-213">Om det behövs, detta [skriptexempel](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) kan skapa en åt dig.</span><span class="sxs-lookup"><span data-stu-id="fe035-213">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="fe035-214">När gå igenom självstudiekursen hello ersätter namn hello resursgrupp och VM där det behövs.</span><span class="sxs-lookup"><span data-stu-id="fe035-214">When working through hello tutorial, replace hello resource group and VM names where needed.</span></span>

<span data-ttu-id="fe035-215">Skapa hello startkonfiguration med [ny AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig).</span><span class="sxs-lookup"><span data-stu-id="fe035-215">Create hello initial configuration with [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig).</span></span> <span data-ttu-id="fe035-216">hello följande exempel konfigurerar en disk som är 128 GB i storlek.</span><span class="sxs-lookup"><span data-stu-id="fe035-216">hello following example configures a disk that is 128 gigabytes in size.</span></span>

```powershell
$diskConfig = New-AzureRmDiskConfig -Location EastUS -CreateOption Empty -DiskSizeGB 128
```

<span data-ttu-id="fe035-217">Skapa hello-datadisk med hello [ny AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) kommando.</span><span class="sxs-lookup"><span data-stu-id="fe035-217">Create hello data disk with hello [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) command.</span></span>

```powershell
$dataDisk = New-AzureRmDisk -ResourceGroupName myResourceGroup -DiskName myDataDisk -Disk $diskConfig
```

<span data-ttu-id="fe035-218">Get hello virtuell dator som du vill tooadd hello data disk toowith hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) kommando.</span><span class="sxs-lookup"><span data-stu-id="fe035-218">Get hello virtual machine that you want tooadd hello data disk toowith hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) command.</span></span>

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

<span data-ttu-id="fe035-219">Lägg till hello data disk toohello konfiguration av virtuell dator med hello [Lägg till AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) kommando.</span><span class="sxs-lookup"><span data-stu-id="fe035-219">Add hello data disk toohello virtual machine configuration with hello [Add-AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) command.</span></span>

```powershell
$vm = Add-AzureRmVMDataDisk -VM $vm -Name myDataDisk -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 1
```

<span data-ttu-id="fe035-220">Uppdatera hello virtuella datorn med hello [uppdatering AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) kommando.</span><span class="sxs-lookup"><span data-stu-id="fe035-220">Update hello virtual machine with hello [Update-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) command.</span></span>

```powershell
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm
```

## <a name="prepare-data-disks"></a><span data-ttu-id="fe035-221">Förbereda datadiskar</span><span class="sxs-lookup"><span data-stu-id="fe035-221">Prepare data disks</span></span>

<span data-ttu-id="fe035-222">När en disk ansluten toohello virtuell dator, måste hello operativsystemet toobe konfigurerats toouse hello disk.</span><span class="sxs-lookup"><span data-stu-id="fe035-222">Once a disk has been attached toohello virtual machine, hello operating system needs toobe configured toouse hello disk.</span></span> <span data-ttu-id="fe035-223">hello följande exempel visar hur toomanually konfigurera hello första diskar som lagts toohello VM.</span><span class="sxs-lookup"><span data-stu-id="fe035-223">hello following example shows how toomanually configure hello first disk added toohello VM.</span></span> <span data-ttu-id="fe035-224">Den här processen kan också automatiseras med hjälp av hello [tillägget för anpassat skript](./tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="fe035-224">This process can also be automated using hello [custom script extension](./tutorial-automate-vm-deployment.md).</span></span>

### <a name="manual-configuration"></a><span data-ttu-id="fe035-225">Manuell konfiguration</span><span class="sxs-lookup"><span data-stu-id="fe035-225">Manual configuration</span></span>

<span data-ttu-id="fe035-226">Skapa en RDP-anslutning med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="fe035-226">Create an RDP connection with hello virtual machine.</span></span> <span data-ttu-id="fe035-227">Öppna PowerShell och kör det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="fe035-227">Open up PowerShell and run this script.</span></span>

```powershell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a><span data-ttu-id="fe035-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fe035-228">Next steps</span></span>

<span data-ttu-id="fe035-229">I kursen får du lärt dig om Virtuella diskar ämnen som:</span><span class="sxs-lookup"><span data-stu-id="fe035-229">In this tutorial, you learned about VM disks topics such as:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fe035-230">OS-diskar och tillfällig diskar</span><span class="sxs-lookup"><span data-stu-id="fe035-230">OS disks and temporary disks</span></span>
> * <span data-ttu-id="fe035-231">Datadiskar</span><span class="sxs-lookup"><span data-stu-id="fe035-231">Data disks</span></span>
> * <span data-ttu-id="fe035-232">Standard- och Premium-diskar</span><span class="sxs-lookup"><span data-stu-id="fe035-232">Standard and Premium disks</span></span>
> * <span data-ttu-id="fe035-233">Diskprestanda</span><span class="sxs-lookup"><span data-stu-id="fe035-233">Disk performance</span></span>
> * <span data-ttu-id="fe035-234">Koppla och förbereda datadiskar</span><span class="sxs-lookup"><span data-stu-id="fe035-234">Attaching and preparing data disks</span></span>

<span data-ttu-id="fe035-235">Avancera toohello nästa självstudiekurs toolearn om hur du automatiserar VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="fe035-235">Advance toohello next tutorial toolearn about automating VM configuration.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fe035-236">Automatisera konfiguration av virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="fe035-236">Automate VM configuration</span></span>](./tutorial-automate-vm-deployment.md)
