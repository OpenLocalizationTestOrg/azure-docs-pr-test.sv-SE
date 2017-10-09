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
# <a name="manage-azure-disks-with-hello-azure-cli"></a><span data-ttu-id="a79f3-103">Hantera Azure-diskarna med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a79f3-103">Manage Azure disks with hello Azure CLI</span></span>

<span data-ttu-id="a79f3-104">Azure virtual machines använder diskar toostore hello virtuella datorer, operativsystem, program och data.</span><span class="sxs-lookup"><span data-stu-id="a79f3-104">Azure virtual machines use disks toostore hello VMs operating system, applications, and data.</span></span> <span data-ttu-id="a79f3-105">När du skapar en virtuell dator är det viktigt toochoose storleken för en disk och konfiguration lämpliga toohello förväntades arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="a79f3-105">When creating a VM it is important toochoose a disk size and configuration appropriate toohello expected workload.</span></span> <span data-ttu-id="a79f3-106">Den här kursen ingår distribuerar och hanterar Virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="a79f3-106">This tutorial covers deploying and managing VM disks.</span></span> <span data-ttu-id="a79f3-107">Du lär dig mer om:</span><span class="sxs-lookup"><span data-stu-id="a79f3-107">You learn about:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a79f3-108">OS-diskar och tillfällig diskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-108">OS disks and temporary disks</span></span>
> * <span data-ttu-id="a79f3-109">Datadiskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-109">Data disks</span></span>
> * <span data-ttu-id="a79f3-110">Standard- och Premium-diskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-110">Standard and Premium disks</span></span>
> * <span data-ttu-id="a79f3-111">Diskprestanda</span><span class="sxs-lookup"><span data-stu-id="a79f3-111">Disk performance</span></span>
> * <span data-ttu-id="a79f3-112">Koppla och förbereda datadiskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-112">Attaching and preparing data disks</span></span>
> * <span data-ttu-id="a79f3-113">Ändra storlek på diskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-113">Resizing disks</span></span>
> * <span data-ttu-id="a79f3-114">Diskbilder</span><span class="sxs-lookup"><span data-stu-id="a79f3-114">Disk snapshots</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a79f3-115">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a79f3-115">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="a79f3-116">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="a79f3-116">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="a79f3-117">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a79f3-117">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="default-azure-disks"></a><span data-ttu-id="a79f3-118">Standard Azure-diskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-118">Default Azure disks</span></span>

<span data-ttu-id="a79f3-119">När en virtuell dator i Azure skapas är två diskar automatiskt bifogade toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a79f3-119">When an Azure virtual machine is created, two disks are automatically attached toohello virtual machine.</span></span> 

<span data-ttu-id="a79f3-120">**Operativsystemdisken** - operativsystemet diskar kan storleksändras in too1 terabyte och värdar hello operativsystemet för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a79f3-120">**Operating system disk** - Operating system disks can be sized up too1 terabyte, and hosts hello VMs operating system.</span></span> <span data-ttu-id="a79f3-121">hello OS-disken är märkt */dev/sda* som standard.</span><span class="sxs-lookup"><span data-stu-id="a79f3-121">hello OS disk is labeled */dev/sda* by default.</span></span> <span data-ttu-id="a79f3-122">hello disk cachelagring konfigurationen av hello OS-disken har optimerats för OS-prestanda.</span><span class="sxs-lookup"><span data-stu-id="a79f3-122">hello disk caching configuration of hello OS disk is optimized for OS performance.</span></span> <span data-ttu-id="a79f3-123">Hej OS-disken på grund av den här konfigurationen **bör inte** värd för program eller data.</span><span class="sxs-lookup"><span data-stu-id="a79f3-123">Because of this configuration, hello OS disk **should not** host applications or data.</span></span> <span data-ttu-id="a79f3-124">Använd datadiskar som beskrivs senare i den här artikeln för program och data.</span><span class="sxs-lookup"><span data-stu-id="a79f3-124">For applications and data, use data disks, which are detailed later in this article.</span></span> 

<span data-ttu-id="a79f3-125">**Diskutrymme** -tillfälliga diskar använder ett SSD-enhet som finns på hello samma Azure-värd som hello VM.</span><span class="sxs-lookup"><span data-stu-id="a79f3-125">**Temporary disk** - Temporary disks use a solid-state drive that is located on hello same Azure host as hello VM.</span></span> <span data-ttu-id="a79f3-126">Temporär diskar har hög performant och kan användas för åtgärder som till exempel temporär databearbetning.</span><span class="sxs-lookup"><span data-stu-id="a79f3-126">Temp disks are highly performant and may be used for operations such as temporary data processing.</span></span> <span data-ttu-id="a79f3-127">Om hello VM är flyttade tooa nya värden kan bort data som lagrats på en tillfällig disk.</span><span class="sxs-lookup"><span data-stu-id="a79f3-127">However, if hello VM is moved tooa new host, any data stored on a temporary disk is removed.</span></span> <span data-ttu-id="a79f3-128">hello hello tillfälliga diskens storlek bestäms av hello VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="a79f3-128">hello size of hello temporary disk is determined by hello VM size.</span></span> <span data-ttu-id="a79f3-129">Tillfällig diskar är märkta */dev/sdb* och har en monteringspunkt för */mnt*.</span><span class="sxs-lookup"><span data-stu-id="a79f3-129">Temporary disks are labeled */dev/sdb* and have a mountpoint of */mnt*.</span></span>

### <a name="temporary-disk-sizes"></a><span data-ttu-id="a79f3-130">Tillfällig diskstorlekar</span><span class="sxs-lookup"><span data-stu-id="a79f3-130">Temporary disk sizes</span></span>

| <span data-ttu-id="a79f3-131">Typ</span><span class="sxs-lookup"><span data-stu-id="a79f3-131">Type</span></span> | <span data-ttu-id="a79f3-132">VM-storlek</span><span class="sxs-lookup"><span data-stu-id="a79f3-132">VM Size</span></span> | <span data-ttu-id="a79f3-133">Maxstorlek för temporär disk (GB)</span><span class="sxs-lookup"><span data-stu-id="a79f3-133">Max temp disk size (GB)</span></span> |
|----|----|----|
| [<span data-ttu-id="a79f3-134">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="a79f3-134">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="a79f3-135">A och D-serien</span><span class="sxs-lookup"><span data-stu-id="a79f3-135">A and D series</span></span> | <span data-ttu-id="a79f3-136">800</span><span class="sxs-lookup"><span data-stu-id="a79f3-136">800</span></span> |
| [<span data-ttu-id="a79f3-137">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="a79f3-137">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="a79f3-138">F-serien</span><span class="sxs-lookup"><span data-stu-id="a79f3-138">F series</span></span> | <span data-ttu-id="a79f3-139">800</span><span class="sxs-lookup"><span data-stu-id="a79f3-139">800</span></span> |
| [<span data-ttu-id="a79f3-140">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="a79f3-140">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="a79f3-141">D och G serien</span><span class="sxs-lookup"><span data-stu-id="a79f3-141">D and G series</span></span> | <span data-ttu-id="a79f3-142">6144</span><span class="sxs-lookup"><span data-stu-id="a79f3-142">6144</span></span> |
| [<span data-ttu-id="a79f3-143">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="a79f3-143">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="a79f3-144">Serie L</span><span class="sxs-lookup"><span data-stu-id="a79f3-144">L series</span></span> | <span data-ttu-id="a79f3-145">5630</span><span class="sxs-lookup"><span data-stu-id="a79f3-145">5630</span></span> |
| [<span data-ttu-id="a79f3-146">GPU</span><span class="sxs-lookup"><span data-stu-id="a79f3-146">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="a79f3-147">N serien</span><span class="sxs-lookup"><span data-stu-id="a79f3-147">N series</span></span> | <span data-ttu-id="a79f3-148">1440</span><span class="sxs-lookup"><span data-stu-id="a79f3-148">1440</span></span> |
| [<span data-ttu-id="a79f3-149">Hög prestanda</span><span class="sxs-lookup"><span data-stu-id="a79f3-149">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="a79f3-150">A och H serien</span><span class="sxs-lookup"><span data-stu-id="a79f3-150">A and H series</span></span> | <span data-ttu-id="a79f3-151">2000</span><span class="sxs-lookup"><span data-stu-id="a79f3-151">2000</span></span> |

## <a name="azure-data-disks"></a><span data-ttu-id="a79f3-152">Azure datadiskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-152">Azure data disks</span></span>

<span data-ttu-id="a79f3-153">Ytterligare datadiskar kan läggas till för att installera program och lagra data.</span><span class="sxs-lookup"><span data-stu-id="a79f3-153">Additional data disks can be added for installing applications and storing data.</span></span> <span data-ttu-id="a79f3-154">Datadiskar som ska användas i en situation där varaktiga och känslig datalagring önskas.</span><span class="sxs-lookup"><span data-stu-id="a79f3-154">Data disks should be used in any situation where durable and responsive data storage is desired.</span></span> <span data-ttu-id="a79f3-155">Varje datadisk har en maximal kapacitet för 1 terabyte.</span><span class="sxs-lookup"><span data-stu-id="a79f3-155">Each data disk has a maximum capacity of 1 terabyte.</span></span> <span data-ttu-id="a79f3-156">hello storleken på hello virtuella datorn anger hur många datadiskar kan vara anslutna tooa VM.</span><span class="sxs-lookup"><span data-stu-id="a79f3-156">hello size of hello virtual machine determines how many data disks can be attached tooa VM.</span></span> <span data-ttu-id="a79f3-157">För varje VM-core kan två diskar kopplas.</span><span class="sxs-lookup"><span data-stu-id="a79f3-157">For each VM core, two data disks can be attached.</span></span> 

### <a name="max-data-disks-per-vm"></a><span data-ttu-id="a79f3-158">Maximalt antal datadiskar per VM</span><span class="sxs-lookup"><span data-stu-id="a79f3-158">Max data disks per VM</span></span>

| <span data-ttu-id="a79f3-159">Typ</span><span class="sxs-lookup"><span data-stu-id="a79f3-159">Type</span></span> | <span data-ttu-id="a79f3-160">VM-storlek</span><span class="sxs-lookup"><span data-stu-id="a79f3-160">VM Size</span></span> | <span data-ttu-id="a79f3-161">Maximalt antal datadiskar per VM</span><span class="sxs-lookup"><span data-stu-id="a79f3-161">Max data disks per VM</span></span> |
|----|----|----|
| [<span data-ttu-id="a79f3-162">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="a79f3-162">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="a79f3-163">A och D-serien</span><span class="sxs-lookup"><span data-stu-id="a79f3-163">A and D series</span></span> | <span data-ttu-id="a79f3-164">32</span><span class="sxs-lookup"><span data-stu-id="a79f3-164">32</span></span> |
| [<span data-ttu-id="a79f3-165">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="a79f3-165">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="a79f3-166">F-serien</span><span class="sxs-lookup"><span data-stu-id="a79f3-166">F series</span></span> | <span data-ttu-id="a79f3-167">32</span><span class="sxs-lookup"><span data-stu-id="a79f3-167">32</span></span> |
| [<span data-ttu-id="a79f3-168">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="a79f3-168">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="a79f3-169">D och G serien</span><span class="sxs-lookup"><span data-stu-id="a79f3-169">D and G series</span></span> | <span data-ttu-id="a79f3-170">64</span><span class="sxs-lookup"><span data-stu-id="a79f3-170">64</span></span> |
| [<span data-ttu-id="a79f3-171">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="a79f3-171">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="a79f3-172">Serie L</span><span class="sxs-lookup"><span data-stu-id="a79f3-172">L series</span></span> | <span data-ttu-id="a79f3-173">64</span><span class="sxs-lookup"><span data-stu-id="a79f3-173">64</span></span> |
| [<span data-ttu-id="a79f3-174">GPU</span><span class="sxs-lookup"><span data-stu-id="a79f3-174">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="a79f3-175">N serien</span><span class="sxs-lookup"><span data-stu-id="a79f3-175">N series</span></span> | <span data-ttu-id="a79f3-176">48</span><span class="sxs-lookup"><span data-stu-id="a79f3-176">48</span></span> |
| [<span data-ttu-id="a79f3-177">Hög prestanda</span><span class="sxs-lookup"><span data-stu-id="a79f3-177">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="a79f3-178">A och H serien</span><span class="sxs-lookup"><span data-stu-id="a79f3-178">A and H series</span></span> | <span data-ttu-id="a79f3-179">32</span><span class="sxs-lookup"><span data-stu-id="a79f3-179">32</span></span> |

## <a name="vm-disk-types"></a><span data-ttu-id="a79f3-180">VM-disktyper</span><span class="sxs-lookup"><span data-stu-id="a79f3-180">VM disk types</span></span>

<span data-ttu-id="a79f3-181">Azure tillhandahåller två typer av disk.</span><span class="sxs-lookup"><span data-stu-id="a79f3-181">Azure provides two types of disk.</span></span>

### <a name="standard-disk"></a><span data-ttu-id="a79f3-182">Standard disk</span><span class="sxs-lookup"><span data-stu-id="a79f3-182">Standard disk</span></span>

<span data-ttu-id="a79f3-183">Standard Storage stöds av hårddiskar och levererar kostnadseffektiv lagring samtidigt som det är högpresterande.</span><span class="sxs-lookup"><span data-stu-id="a79f3-183">Standard Storage is backed by HDDs, and delivers cost-effective storage while still being performant.</span></span> <span data-ttu-id="a79f3-184">Standarddiskar är idealiskt för ett kostnadseffektivt dev och testa arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="a79f3-184">Standard disks are ideal for a cost effective dev and test workload.</span></span>

### <a name="premium-disk"></a><span data-ttu-id="a79f3-185">Premium-disk</span><span class="sxs-lookup"><span data-stu-id="a79f3-185">Premium disk</span></span>

<span data-ttu-id="a79f3-186">Premiumdiskar backas upp av SSD-baserad hög prestanda, låg latens disk.</span><span class="sxs-lookup"><span data-stu-id="a79f3-186">Premium disks are backed by SSD-based high-performance, low-latency disk.</span></span> <span data-ttu-id="a79f3-187">Perfekt för virtuella datorer som kör produktion arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="a79f3-187">Perfect for VMs running production workload.</span></span> <span data-ttu-id="a79f3-188">Premium-lagring stöder DS-serien, DSv2-serien GS-serien och FS-serien virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a79f3-188">Premium Storage supports DS-series, DSv2-series, GS-series, and FS-series VMs.</span></span> <span data-ttu-id="a79f3-189">Premiumdiskar finns i tre olika typer (P10 P20, P30), hello hello diskens storlek avgör hello disktyp.</span><span class="sxs-lookup"><span data-stu-id="a79f3-189">Premium disks come in three types (P10, P20, P30), hello size of hello disk determines hello disk type.</span></span> <span data-ttu-id="a79f3-190">När du väljer, avrundat ett värde för disk hello toohello nästa typen.</span><span class="sxs-lookup"><span data-stu-id="a79f3-190">When selecting, a disk size hello value is rounded up toohello next type.</span></span> <span data-ttu-id="a79f3-191">Om hello diskens storlek är mindre än 128 GB, är hello disktypen P10.</span><span class="sxs-lookup"><span data-stu-id="a79f3-191">For example, if hello disk size is less than 128 GB, hello disk type is P10.</span></span> <span data-ttu-id="a79f3-192">Om hello diskens storlek är mellan 129 och 512 GB, är hello storleken en P20.</span><span class="sxs-lookup"><span data-stu-id="a79f3-192">If hello disk size is between 129 GB and 512 GB, hello size is a P20.</span></span> <span data-ttu-id="a79f3-193">Något över 512 GB hello storlek är en P30.</span><span class="sxs-lookup"><span data-stu-id="a79f3-193">Anything over 512 GB, hello size is a P30.</span></span>

### <a name="premium-disk-performance"></a><span data-ttu-id="a79f3-194">Premium-diskprestanda</span><span class="sxs-lookup"><span data-stu-id="a79f3-194">Premium disk performance</span></span>

|<span data-ttu-id="a79f3-195">Premium-lagring disktyp</span><span class="sxs-lookup"><span data-stu-id="a79f3-195">Premium storage disk type</span></span> | <span data-ttu-id="a79f3-196">P10</span><span class="sxs-lookup"><span data-stu-id="a79f3-196">P10</span></span> | <span data-ttu-id="a79f3-197">P20</span><span class="sxs-lookup"><span data-stu-id="a79f3-197">P20</span></span> | <span data-ttu-id="a79f3-198">P30</span><span class="sxs-lookup"><span data-stu-id="a79f3-198">P30</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a79f3-199">Diskens storlek (avrunda uppåt)</span><span class="sxs-lookup"><span data-stu-id="a79f3-199">Disk size (round up)</span></span> | <span data-ttu-id="a79f3-200">128 GB</span><span class="sxs-lookup"><span data-stu-id="a79f3-200">128 GB</span></span> | <span data-ttu-id="a79f3-201">512 GB</span><span class="sxs-lookup"><span data-stu-id="a79f3-201">512 GB</span></span> | <span data-ttu-id="a79f3-202">1 024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="a79f3-202">1,024 GB (1 TB)</span></span> |
| <span data-ttu-id="a79f3-203">Högsta IOPS per disk</span><span class="sxs-lookup"><span data-stu-id="a79f3-203">Max IOPS per disk</span></span> | <span data-ttu-id="a79f3-204">500</span><span class="sxs-lookup"><span data-stu-id="a79f3-204">500</span></span> | <span data-ttu-id="a79f3-205">2,300</span><span class="sxs-lookup"><span data-stu-id="a79f3-205">2,300</span></span> | <span data-ttu-id="a79f3-206">5,000</span><span class="sxs-lookup"><span data-stu-id="a79f3-206">5,000</span></span> |
<span data-ttu-id="a79f3-207">Dataflöde per disk</span><span class="sxs-lookup"><span data-stu-id="a79f3-207">Throughput per disk</span></span> | <span data-ttu-id="a79f3-208">100 MB/s</span><span class="sxs-lookup"><span data-stu-id="a79f3-208">100 MB/s</span></span> | <span data-ttu-id="a79f3-209">150 MB/s</span><span class="sxs-lookup"><span data-stu-id="a79f3-209">150 MB/s</span></span> | <span data-ttu-id="a79f3-210">200 MB/s</span><span class="sxs-lookup"><span data-stu-id="a79f3-210">200 MB/s</span></span> |

<span data-ttu-id="a79f3-211">Medan hello ovanför tabell identifierar högsta IOPS per disk, kan en högre nivå av prestanda uppnås genom striping flera datadiskar.</span><span class="sxs-lookup"><span data-stu-id="a79f3-211">While hello above table identifies max IOPS per disk, a higher level of performance can be achieved by striping multiple data disks.</span></span> <span data-ttu-id="a79f3-212">Exempelvis kan en virtuell dator Standard_GS5 uppnå högst 80 000 IOPS.</span><span class="sxs-lookup"><span data-stu-id="a79f3-212">For instance, a Standard_GS5 VM can achieve a maximum of 80,000 IOPS.</span></span> <span data-ttu-id="a79f3-213">Detaljerad information om högsta IOPS per VM finns [Linux VM-storlekar](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="a79f3-213">For detailed information on max IOPS per VM, see [Linux VM sizes](sizes.md).</span></span>

## <a name="create-and-attach-disks"></a><span data-ttu-id="a79f3-214">Skapa och koppla diskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-214">Create and attach disks</span></span>

<span data-ttu-id="a79f3-215">Datadiskar kan skapas och kopplade på VM-tiden för skapandet eller tooan befintliga VM.</span><span class="sxs-lookup"><span data-stu-id="a79f3-215">Data disks can be created and attached at VM creation time or tooan existing VM.</span></span>

### <a name="attach-disk-at-vm-creation"></a><span data-ttu-id="a79f3-216">Bifoga disken på Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a79f3-216">Attach disk at VM creation</span></span>

<span data-ttu-id="a79f3-217">Skapa en resursgrupp med hello [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="a79f3-217">Create a resource group with hello [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> 

```azurecli-interactive 
az group create --name myResourceGroupDisk --location eastus
```

<span data-ttu-id="a79f3-218">Skapa en virtuell dator med hjälp av hello [az vm skapa]( /cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="a79f3-218">Create a VM using hello [az vm create]( /cli/azure/vm#create) command.</span></span> <span data-ttu-id="a79f3-219">Hej `--datadisk-sizes-gb` argumentet är används toospecify att ytterligare en disk som ska skapas och bifogas toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a79f3-219">hello `--datadisk-sizes-gb` argument is used toospecify that an additional disk should be created and attached toohello virtual machine.</span></span> <span data-ttu-id="a79f3-220">toocreate och koppla mer än en disk, Använd en mellanslags-avgränsad lista med värden för disk-storlek.</span><span class="sxs-lookup"><span data-stu-id="a79f3-220">toocreate and attach more than one disk, use a space-delimited list of disk size values.</span></span> <span data-ttu-id="a79f3-221">I följande exempel hello, skapas en virtuell dator med två diskar, både 128 GB.</span><span class="sxs-lookup"><span data-stu-id="a79f3-221">In hello following example, a VM is created with two data disks, both 128 GB.</span></span> <span data-ttu-id="a79f3-222">Eftersom hello storlekar för diskar är 128 GB, diskarna båda är konfigurerade som P10s som ger högsta 500 IOPS per disk.</span><span class="sxs-lookup"><span data-stu-id="a79f3-222">Because hello disk sizes are 128 GB, these disks are both configured as P10s, which provide maximum 500 IOPS per disk.</span></span>

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --data-disk-sizes-gb 128 128 \
  --generate-ssh-keys
```

### <a name="attach-disk-tooexisting-vm"></a><span data-ttu-id="a79f3-223">Bifoga disken tooexisting VM</span><span class="sxs-lookup"><span data-stu-id="a79f3-223">Attach disk tooexisting VM</span></span>

<span data-ttu-id="a79f3-224">toocreate och koppla en ny disk tooan befintlig virtuell dator kan använda hello [az vm disk bifoga](/cli/azure/vm/disk#attach) kommando.</span><span class="sxs-lookup"><span data-stu-id="a79f3-224">toocreate and attach a new disk tooan existing virtual machine, use hello [az vm disk attach](/cli/azure/vm/disk#attach) command.</span></span> <span data-ttu-id="a79f3-225">hello följande exempel skapar en premium disk, 128 GB i storlek, och bifogar den toohello skapas den virtuella datorn i hello sista steget.</span><span class="sxs-lookup"><span data-stu-id="a79f3-225">hello following example creates a premium disk, 128 gigabytes in size, and attaches it toohello VM created in hello last step.</span></span>

```azurecli-interactive 
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a><span data-ttu-id="a79f3-226">Förbereda datadiskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-226">Prepare data disks</span></span>

<span data-ttu-id="a79f3-227">När en disk ansluten toohello virtuell dator, måste hello operativsystemet toobe konfigurerats toouse hello disk.</span><span class="sxs-lookup"><span data-stu-id="a79f3-227">Once a disk has been attached toohello virtual machine, hello operating system needs toobe configured toouse hello disk.</span></span> <span data-ttu-id="a79f3-228">hello som följande exempel visar hur toomanually konfigurera en disk.</span><span class="sxs-lookup"><span data-stu-id="a79f3-228">hello following example shows how toomanually configure a disk.</span></span> <span data-ttu-id="a79f3-229">Den här processen kan också automatiseras med hjälp av molnet initiering, som beskrivs i en [senare kursen](./tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="a79f3-229">This process can also be automated using cloud-init, which is covered in a [later tutorial](./tutorial-automate-vm-deployment.md).</span></span>

### <a name="manual-configuration"></a><span data-ttu-id="a79f3-230">Manuell konfiguration</span><span class="sxs-lookup"><span data-stu-id="a79f3-230">Manual configuration</span></span>

<span data-ttu-id="a79f3-231">Skapa en SSH-anslutning med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a79f3-231">Create an SSH connection with hello virtual machine.</span></span> <span data-ttu-id="a79f3-232">Ersätt hello IP-adressen med hello offentliga IP-Adressen för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a79f3-232">Replace hello example IP address with hello public IP of hello virtual machine.</span></span>

```azurecli-interactive 
ssh 52.174.34.95
```

<span data-ttu-id="a79f3-233">Hej partitionsdisk med `fdisk`.</span><span class="sxs-lookup"><span data-stu-id="a79f3-233">Partition hello disk with `fdisk`.</span></span>

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

<span data-ttu-id="a79f3-234">Skriva toohello en filsystempartition med hjälp av hello `mkfs` kommando.</span><span class="sxs-lookup"><span data-stu-id="a79f3-234">Write a file system toohello partition by using hello `mkfs` command.</span></span>

```bash
sudo mkfs -t ext4 /dev/sdc1
```

<span data-ttu-id="a79f3-235">Montera hello ny disk så att den är tillgänglig i hello-operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="a79f3-235">Mount hello new disk so that it is accessible in hello operating system.</span></span>

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

<span data-ttu-id="a79f3-236">hello disk nu kan nås via hello *datadrive* monteringspunkt, som kan verifieras genom att köra hello `df -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="a79f3-236">hello disk can now be accessed through hello *datadrive* mountpoint, which can be verified by running hello `df -h` command.</span></span> 

```bash
df -h
```

<span data-ttu-id="a79f3-237">hello utdata visar hello nya enheten monteras på */datadrive*.</span><span class="sxs-lookup"><span data-stu-id="a79f3-237">hello output shows hello new drive mounted on */datadrive*.</span></span>

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

<span data-ttu-id="a79f3-238">tooensure som hello enheten är på nytt efter en omstart, måste det läggas toohello */etc/fstab* fil.</span><span class="sxs-lookup"><span data-stu-id="a79f3-238">tooensure that hello drive is remounted after a reboot, it must be added toohello */etc/fstab* file.</span></span> <span data-ttu-id="a79f3-239">toodo så få hello UUID för hello disk med hello `blkid` verktyget.</span><span class="sxs-lookup"><span data-stu-id="a79f3-239">toodo so, get hello UUID of hello disk with hello `blkid` utility.</span></span>

```bash
sudo -i blkid
```

<span data-ttu-id="a79f3-240">hello utdata visar hello UUID hello enhetens `/dev/sdc1` i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="a79f3-240">hello output displays hello UUID of hello drive, `/dev/sdc1` in this case.</span></span>

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

<span data-ttu-id="a79f3-241">Lägg till en rad liknande toohello följande toohello */etc/fstab* fil.</span><span class="sxs-lookup"><span data-stu-id="a79f3-241">Add a line similar toohello following toohello */etc/fstab* file.</span></span> <span data-ttu-id="a79f3-242">Även Observera att skriva barriärer kan inaktiveras med *barriären = 0*, den här konfigurationen kan förbättra diskprestanda.</span><span class="sxs-lookup"><span data-stu-id="a79f3-242">Also note that write barriers can be disabled using *barrier=0*, this configuration can improve disk performance.</span></span> 

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail,barrier=0   1  2
```

<span data-ttu-id="a79f3-243">Nu när hello disk har konfigurerats, Stäng hello SSH-session.</span><span class="sxs-lookup"><span data-stu-id="a79f3-243">Now that hello disk has been configured, close hello SSH session.</span></span>

```bash
exit
```

## <a name="resize-vm-disk"></a><span data-ttu-id="a79f3-244">Ändra storlek på virtuell disk</span><span class="sxs-lookup"><span data-stu-id="a79f3-244">Resize VM disk</span></span>

<span data-ttu-id="a79f3-245">När en virtuell dator har distribuerats, ökas hello operativsystemdisk eller eventuella anslutna hårddiskar i storlek.</span><span class="sxs-lookup"><span data-stu-id="a79f3-245">Once a VM has been deployed, hello operating system disk or any attached data disks can be increased in size.</span></span> <span data-ttu-id="a79f3-246">Det är bra att öka hello storleken på en disk när du behöver mer lagringsutrymme eller högre prestanda (P10, P20, P30).</span><span class="sxs-lookup"><span data-stu-id="a79f3-246">Increasing hello size of a disk is beneficial when needing more storage space or a higher level of performance (P10, P20, P30).</span></span> <span data-ttu-id="a79f3-247">Observera att diskar inte kan minskas i storlek.</span><span class="sxs-lookup"><span data-stu-id="a79f3-247">Note, disks cannot be decreased in size.</span></span>

<span data-ttu-id="a79f3-248">Innan du ökar diskstorleken hello Id eller namnet på hello disk behövs.</span><span class="sxs-lookup"><span data-stu-id="a79f3-248">Before increasing disk size, hello Id or name of hello disk is needed.</span></span> <span data-ttu-id="a79f3-249">Använd hello [az Disklista](/cli/azure/vm/disk#list) kommandot tooreturn alla diskar i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a79f3-249">Use hello [az disk list](/cli/azure/vm/disk#list) command tooreturn all disks in a resource group.</span></span> <span data-ttu-id="a79f3-250">Anteckna hello diskens namn som du vill att tooresize.</span><span class="sxs-lookup"><span data-stu-id="a79f3-250">Take note of hello disk name that you would like tooresize.</span></span>

```azurecli-interactive 
az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

<span data-ttu-id="a79f3-251">hello VM måste också vara frigjord.</span><span class="sxs-lookup"><span data-stu-id="a79f3-251">hello VM must also be deallocated.</span></span> <span data-ttu-id="a79f3-252">Använd hello [az vm frigöra]( /cli/azure/vm#deallocate) kommando toostop och frigöra hello VM.</span><span class="sxs-lookup"><span data-stu-id="a79f3-252">Use hello [az vm deallocate]( /cli/azure/vm#deallocate) command toostop and deallocate hello VM.</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

<span data-ttu-id="a79f3-253">Använd hello [az disk uppdatering](/cli/azure/vm/disk#update) kommandot tooresize hello disk.</span><span class="sxs-lookup"><span data-stu-id="a79f3-253">Use hello [az disk update](/cli/azure/vm/disk#update) command tooresize hello disk.</span></span> <span data-ttu-id="a79f3-254">Det här exemplet ändrar storlek på en disk med namnet *myDataDisk* too1 terabyte.</span><span class="sxs-lookup"><span data-stu-id="a79f3-254">This example resizes a disk named *myDataDisk* too1 terabyte.</span></span>

```azurecli-interactive 
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

<span data-ttu-id="a79f3-255">Starta hello VM när hello storleksändring åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a79f3-255">Once hello resize operation has completed, start hello VM.</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupDisk --name myVM
```

<span data-ttu-id="a79f3-256">Om du har ändrat storlek hello operativsystem systemdisken utökas automatiskt hello partition.</span><span class="sxs-lookup"><span data-stu-id="a79f3-256">If you’ve resized hello operating system disk, hello partition is automatically be expanded.</span></span> <span data-ttu-id="a79f3-257">Om du har ändrat storlek en datadisk, måste alla aktuella partitioner toobe expanderas i hello operativsystemet för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a79f3-257">If you have resized a data disk, any current partitions need toobe expanded in hello VMs operating system.</span></span>

## <a name="snapshot-azure-disks"></a><span data-ttu-id="a79f3-258">Ögonblicksbild Azure-diskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-258">Snapshot Azure disks</span></span>

<span data-ttu-id="a79f3-259">Ta en ögonblicksbild av disk skapar en skrivskyddad, men i tidpunkt kopia av hello disk.</span><span class="sxs-lookup"><span data-stu-id="a79f3-259">Taking a disk snapshot creates a read only, point-in-time copy of hello disk.</span></span> <span data-ttu-id="a79f3-260">Azure VM ögonblicksbilder är användbara för att snabbt spara hello tillståndet för en virtuell dator innan du gör ändringar i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a79f3-260">Azure VM snapshots are useful for quickly saving hello state of a VM before making configuration changes.</span></span> <span data-ttu-id="a79f3-261">I händelse av hello hello konfigurationsändringar bevisa toobe områdena, tillstånd för virtuell dator kan återställas med hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="a79f3-261">In hello event hello configuration changes prove toobe undesired, VM state can be restored using hello snapshot.</span></span> <span data-ttu-id="a79f3-262">När en virtuell dator har mer än en disk, en ögonblicksbild tas för varje disk oberoende av hello andra.</span><span class="sxs-lookup"><span data-stu-id="a79f3-262">When a VM has more than one disk, a snapshot is taken of each disk independently of hello others.</span></span> <span data-ttu-id="a79f3-263">Överväg att stoppa hello VM innan du tar diskbilder för säkerhetskopiering programmet konsekvent.</span><span class="sxs-lookup"><span data-stu-id="a79f3-263">For taking application consistent backups, consider stopping hello VM before taking disk snapshots.</span></span> <span data-ttu-id="a79f3-264">Du kan också använda hello [Azure Backup-tjänsten](/azure/backup/), vilket aktiverar du tooperform automatisk säkerhetskopior vid hello virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="a79f3-264">Alternatively, use hello [Azure Backup service](/azure/backup/), which enables you tooperform automated backups while hello VM is running.</span></span>

### <a name="create-snapshot"></a><span data-ttu-id="a79f3-265">Skapa en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="a79f3-265">Create snapshot</span></span>

<span data-ttu-id="a79f3-266">Innan du skapar en virtuell disk ögonblicksbild krävs hello-Id eller namn för hello disk.</span><span class="sxs-lookup"><span data-stu-id="a79f3-266">Before creating a virtual machine disk snapshot, hello Id or name of hello disk is needed.</span></span> <span data-ttu-id="a79f3-267">Använd hello [az vm visa](https://docs.microsoft.com/en-us/cli/azure/vm#show) kommandot tooreturn hello disk-id. I det här exemplet lagras hello disk-id i en variabel så att den kan användas i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="a79f3-267">Use hello [az vm show](https://docs.microsoft.com/en-us/cli/azure/vm#show) command tooreturn hello disk id. In this example, hello disk id is stored in a variable so that it can be used in a later step.</span></span>

```azurecli-interactive 
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

<span data-ttu-id="a79f3-268">Nu när du har hello-id för hello virtuella disken hello följande kommando skapar en ögonblicksbild av hello disk.</span><span class="sxs-lookup"><span data-stu-id="a79f3-268">Now that you have hello id of hello virtual machine disk, hello following command creates a snapshot of hello disk.</span></span>

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a><span data-ttu-id="a79f3-269">Skapa disk från en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="a79f3-269">Create disk from snapshot</span></span>

<span data-ttu-id="a79f3-270">Den här ögonblicksbilden kan sedan konverteras till en disk, vilket används toorecreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a79f3-270">This snapshot can then be converted into a disk, which can be used toorecreate hello virtual machine.</span></span>

```azurecli-interactive 
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a><span data-ttu-id="a79f3-271">Återställa en virtuell dator från en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="a79f3-271">Restore virtual machine from snapshot</span></span>

<span data-ttu-id="a79f3-272">toodemonstrate återställning av virtuell dator, ta bort hello befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a79f3-272">toodemonstrate virtual machine recovery, delete hello existing virtual machine.</span></span> 

```azurecli-interactive 
az vm delete --resource-group myResourceGroupDisk --name myVM
```

<span data-ttu-id="a79f3-273">Skapa en ny virtuell dator från hello ögonblicksbild disken.</span><span class="sxs-lookup"><span data-stu-id="a79f3-273">Create a new virtual machine from hello snapshot disk.</span></span>

```azurecli-interactive 
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a><span data-ttu-id="a79f3-274">Återanslut datadisk</span><span class="sxs-lookup"><span data-stu-id="a79f3-274">Reattach data disk</span></span>

<span data-ttu-id="a79f3-275">Alla datadiskar måste toobe anbringas på nytt toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a79f3-275">All data disks need toobe reattached toohello virtual machine.</span></span>

<span data-ttu-id="a79f3-276">Hitta först hello data diskens namn med hello [az Disklista](https://docs.microsoft.com/cli/azure/disk#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="a79f3-276">First find hello data disk name using hello [az disk list](https://docs.microsoft.com/cli/azure/disk#list) command.</span></span> <span data-ttu-id="a79f3-277">Det här exemplet platser hello namnet på hello disk i en variabel med namnet *datadisk*, som används i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="a79f3-277">This example places hello name of hello disk in a variable named *datadisk*, which is used in hello next step.</span></span>

```azurecli-interactive 
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

<span data-ttu-id="a79f3-278">Använd hello [az vm disk bifoga](https://docs.microsoft.com/cli/azure/vm/disk#attach) kommandot tooattach hello disk.</span><span class="sxs-lookup"><span data-stu-id="a79f3-278">Use hello [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#attach) command tooattach hello disk.</span></span>

```azurecli-interactive 
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a><span data-ttu-id="a79f3-279">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a79f3-279">Next steps</span></span>

<span data-ttu-id="a79f3-280">I kursen får du lärt dig om Virtuella diskar ämnen som:</span><span class="sxs-lookup"><span data-stu-id="a79f3-280">In this tutorial, you learned about VM disks topics such as:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a79f3-281">OS-diskar och tillfällig diskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-281">OS disks and temporary disks</span></span>
> * <span data-ttu-id="a79f3-282">Datadiskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-282">Data disks</span></span>
> * <span data-ttu-id="a79f3-283">Standard- och Premium-diskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-283">Standard and Premium disks</span></span>
> * <span data-ttu-id="a79f3-284">Diskprestanda</span><span class="sxs-lookup"><span data-stu-id="a79f3-284">Disk performance</span></span>
> * <span data-ttu-id="a79f3-285">Koppla och förbereda datadiskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-285">Attaching and preparing data disks</span></span>
> * <span data-ttu-id="a79f3-286">Ändra storlek på diskar</span><span class="sxs-lookup"><span data-stu-id="a79f3-286">Resizing disks</span></span>
> * <span data-ttu-id="a79f3-287">Diskbilder</span><span class="sxs-lookup"><span data-stu-id="a79f3-287">Disk snapshots</span></span>

<span data-ttu-id="a79f3-288">Avancera toohello nästa självstudiekurs toolearn om hur du automatiserar VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a79f3-288">Advance toohello next tutorial toolearn about automating VM configuration.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a79f3-289">Automatisera konfiguration av virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a79f3-289">Automate VM configuration</span></span>](./tutorial-automate-vm-deployment.md)
