---
title: Migrera en klassisk virtuell dator till en hanterad ARM-Disk VM | Microsoft Docs
description: "Migrera en enda Azure-VM från den klassiska distributionsmodellen till hanterade diskar i Resource Manager-distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 82389834d85981c0ed71bdcc891fbfdbe1377654
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manually-migrate-a-classic-vm-to-a-new-arm-managed-disk-vm-from-the-vhd"></a><span data-ttu-id="8303b-103">Migrera manuellt en klassisk virtuell dator till en ny ARM hanteras Disk virtuell dator från den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="8303b-103">Manually migrate a Classic VM to a new ARM Managed Disk VM from the VHD</span></span> 


<span data-ttu-id="8303b-104">Det här avsnittet hjälper dig att migrera dina befintliga virtuella Azure-datorer från den klassiska distributionsmodellen till [hanterade diskar](managed-disks-overview.md) i Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="8303b-104">This section helps you to migrate your existing Azure VMs from the classic deployment model to [Managed Disks](managed-disks-overview.md) in the Resource Manager deployment model.</span></span>


## <a name="plan-for-the-migration-to-managed-disks"></a><span data-ttu-id="8303b-105">Planera för migrering till hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="8303b-105">Plan for the migration to Managed Disks</span></span>

<span data-ttu-id="8303b-106">Det här avsnittet hjälper dig att göra det bästa på VM- och diskresurser typer.</span><span class="sxs-lookup"><span data-stu-id="8303b-106">This section helps you to make the best decision on VM and disk types.</span></span>


### <a name="location"></a><span data-ttu-id="8303b-107">Plats</span><span class="sxs-lookup"><span data-stu-id="8303b-107">Location</span></span>

<span data-ttu-id="8303b-108">Välj en plats där Azure hanterade diskar är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="8303b-108">Pick a location where Azure Managed Disks are available.</span></span> <span data-ttu-id="8303b-109">Om du migrerar till hanterade Premiumdiskar också se till att Premium-lagring är tillgänglig i den region där du planerar att migrera till.</span><span class="sxs-lookup"><span data-stu-id="8303b-109">If you are migrating to Premium Managed Disks, also ensure that Premium storage is available in the region where you are planning to migrate to.</span></span> <span data-ttu-id="8303b-110">Se [Azure Services byRegion](https://azure.microsoft.com/regions/#services) uppdaterad information om tillgängliga platser.</span><span class="sxs-lookup"><span data-stu-id="8303b-110">See [Azure Services byRegion](https://azure.microsoft.com/regions/#services) for up-to-date information on available locations.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="8303b-111">VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="8303b-111">VM sizes</span></span>

<span data-ttu-id="8303b-112">Om du migrerar till Premium hanterade diskar som du behöver uppdatera storleken på den virtuella datorn till Premium-lagring kan storlek i den region där VM finns.</span><span class="sxs-lookup"><span data-stu-id="8303b-112">If you are migrating to Premium Managed Disks, you have to update the size of the VM to Premium Storage capable size available in the region where VM is located.</span></span> <span data-ttu-id="8303b-113">Granska storlek på Virtuella datorer som är Premium-lagring som är kompatibla.</span><span class="sxs-lookup"><span data-stu-id="8303b-113">Review the VM sizes that are Premium Storage capable.</span></span> <span data-ttu-id="8303b-114">Specifikationer för Azure VM-storleken anges i [storlekar för virtuella datorer](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="8303b-114">The Azure VM size specifications are listed in [Sizes for virtual machines](sizes.md).</span></span>
<span data-ttu-id="8303b-115">Granska prestandaegenskaper för virtuella datorer som fungerar med Premium-lagring och välja den lämpligaste VM-storlek som bäst passar din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="8303b-115">Review the performance characteristics of virtual machines that work with Premium Storage and choose the most appropriate VM size that best suits your workload.</span></span> <span data-ttu-id="8303b-116">Kontrollera att det finns tillräckligt med bandbredd på den virtuella datorn för att ge disk-trafik.</span><span class="sxs-lookup"><span data-stu-id="8303b-116">Make sure that there is sufficient bandwidth available on your VM to drive the disk traffic.</span></span>

### <a name="disk-sizes"></a><span data-ttu-id="8303b-117">Diskstorlekar</span><span class="sxs-lookup"><span data-stu-id="8303b-117">Disk sizes</span></span>

<span data-ttu-id="8303b-118">**Premium hanterade diskar**</span><span class="sxs-lookup"><span data-stu-id="8303b-118">**Premium Managed Disks**</span></span>

<span data-ttu-id="8303b-119">Det finns sju typer av premium hanterade diskar som kan användas med den virtuella datorn var och en har särskilda IOPs och genomströmning gränser.</span><span class="sxs-lookup"><span data-stu-id="8303b-119">There are seven types of premium Managed disks that can be used with your VM and each has specific IOPs and throughput limits.</span></span> <span data-ttu-id="8303b-120">Överväg att dessa begränsningar när du väljer Premium disktyp för den virtuella datorn baserat på dina behov av ditt program vad gäller kapacitet, prestanda, skalbarhet och belastning läses in.</span><span class="sxs-lookup"><span data-stu-id="8303b-120">Consider these limits when choosing the Premium disk type for your VM based on the needs of your application in terms of capacity, performance, scalability, and peak loads.</span></span>

| <span data-ttu-id="8303b-121">Premium diskar typ</span><span class="sxs-lookup"><span data-stu-id="8303b-121">Premium Disks Type</span></span>  | <span data-ttu-id="8303b-122">P4</span><span class="sxs-lookup"><span data-stu-id="8303b-122">P4</span></span>    | <span data-ttu-id="8303b-123">P6</span><span class="sxs-lookup"><span data-stu-id="8303b-123">P6</span></span>    | <span data-ttu-id="8303b-124">P10</span><span class="sxs-lookup"><span data-stu-id="8303b-124">P10</span></span>   | <span data-ttu-id="8303b-125">P20</span><span class="sxs-lookup"><span data-stu-id="8303b-125">P20</span></span>   | <span data-ttu-id="8303b-126">P30</span><span class="sxs-lookup"><span data-stu-id="8303b-126">P30</span></span>   | <span data-ttu-id="8303b-127">P40</span><span class="sxs-lookup"><span data-stu-id="8303b-127">P40</span></span>   | <span data-ttu-id="8303b-128">P50</span><span class="sxs-lookup"><span data-stu-id="8303b-128">P50</span></span>   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| <span data-ttu-id="8303b-129">Diskstorlek</span><span class="sxs-lookup"><span data-stu-id="8303b-129">Disk size</span></span>           | <span data-ttu-id="8303b-130">128 GB</span><span class="sxs-lookup"><span data-stu-id="8303b-130">128 GB</span></span>| <span data-ttu-id="8303b-131">512 GB</span><span class="sxs-lookup"><span data-stu-id="8303b-131">512 GB</span></span>| <span data-ttu-id="8303b-132">128 GB</span><span class="sxs-lookup"><span data-stu-id="8303b-132">128 GB</span></span>| <span data-ttu-id="8303b-133">512 GB</span><span class="sxs-lookup"><span data-stu-id="8303b-133">512 GB</span></span>            | <span data-ttu-id="8303b-134">1 024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="8303b-134">1024 GB (1 TB)</span></span>    | <span data-ttu-id="8303b-135">2 048 GB (2 TB)</span><span class="sxs-lookup"><span data-stu-id="8303b-135">2048 GB (2 TB)</span></span>    | <span data-ttu-id="8303b-136">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="8303b-136">4095 GB (4 TB)</span></span>    | 
| <span data-ttu-id="8303b-137">IOPS per disk</span><span class="sxs-lookup"><span data-stu-id="8303b-137">IOPS per disk</span></span>       | <span data-ttu-id="8303b-138">120</span><span class="sxs-lookup"><span data-stu-id="8303b-138">120</span></span>   | <span data-ttu-id="8303b-139">240</span><span class="sxs-lookup"><span data-stu-id="8303b-139">240</span></span>   | <span data-ttu-id="8303b-140">500</span><span class="sxs-lookup"><span data-stu-id="8303b-140">500</span></span>   | <span data-ttu-id="8303b-141">2 300</span><span class="sxs-lookup"><span data-stu-id="8303b-141">2300</span></span>              | <span data-ttu-id="8303b-142">5000</span><span class="sxs-lookup"><span data-stu-id="8303b-142">5000</span></span>              | <span data-ttu-id="8303b-143">7500</span><span class="sxs-lookup"><span data-stu-id="8303b-143">7500</span></span>              | <span data-ttu-id="8303b-144">7500</span><span class="sxs-lookup"><span data-stu-id="8303b-144">7500</span></span>              | 
| <span data-ttu-id="8303b-145">Dataflöde per disk</span><span class="sxs-lookup"><span data-stu-id="8303b-145">Throughput per disk</span></span> | <span data-ttu-id="8303b-146">25 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-146">25 MB per second</span></span>  | <span data-ttu-id="8303b-147">50 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-147">50 MB per second</span></span>  | <span data-ttu-id="8303b-148">100 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-148">100 MB per second</span></span> | <span data-ttu-id="8303b-149">150 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-149">150 MB per second</span></span> | <span data-ttu-id="8303b-150">200 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-150">200 MB per second</span></span> | <span data-ttu-id="8303b-151">250 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-151">250 MB per second</span></span> | <span data-ttu-id="8303b-152">250 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-152">250 MB per second</span></span> | 

<span data-ttu-id="8303b-153">**Hanterade standarddiskar**</span><span class="sxs-lookup"><span data-stu-id="8303b-153">**Standard Managed Disks**</span></span>

<span data-ttu-id="8303b-154">Det finns sju typer av Standard hanterade diskar som kan användas med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8303b-154">There are seven types of Standard Managed disks that can be used with your VM.</span></span> <span data-ttu-id="8303b-155">Var och en av dem har olika kapacitet men samma IOPS och genomströmning gränser.</span><span class="sxs-lookup"><span data-stu-id="8303b-155">Each of them have different capacity but have same IOPS and throughput limits.</span></span> <span data-ttu-id="8303b-156">Välj typ av Standard hanterade diskar baserat på kapacitetsbehov för programmet.</span><span class="sxs-lookup"><span data-stu-id="8303b-156">Choose the type of Standard Managed disks based on the capacity needs of your application.</span></span>

| <span data-ttu-id="8303b-157">Disk av standardtyp</span><span class="sxs-lookup"><span data-stu-id="8303b-157">Standard Disk Type</span></span>  | <span data-ttu-id="8303b-158">S4</span><span class="sxs-lookup"><span data-stu-id="8303b-158">S4</span></span>               | <span data-ttu-id="8303b-159">S6</span><span class="sxs-lookup"><span data-stu-id="8303b-159">S6</span></span>               | <span data-ttu-id="8303b-160">S10</span><span class="sxs-lookup"><span data-stu-id="8303b-160">S10</span></span>              | <span data-ttu-id="8303b-161">S20</span><span class="sxs-lookup"><span data-stu-id="8303b-161">S20</span></span>              | <span data-ttu-id="8303b-162">S30</span><span class="sxs-lookup"><span data-stu-id="8303b-162">S30</span></span>              | <span data-ttu-id="8303b-163">S40</span><span class="sxs-lookup"><span data-stu-id="8303b-163">S40</span></span>              | <span data-ttu-id="8303b-164">S50</span><span class="sxs-lookup"><span data-stu-id="8303b-164">S50</span></span>              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| <span data-ttu-id="8303b-165">Diskstorlek</span><span class="sxs-lookup"><span data-stu-id="8303b-165">Disk size</span></span>           | <span data-ttu-id="8303b-166">30 GB</span><span class="sxs-lookup"><span data-stu-id="8303b-166">30 GB</span></span>            | <span data-ttu-id="8303b-167">64 GB</span><span class="sxs-lookup"><span data-stu-id="8303b-167">64 GB</span></span>            | <span data-ttu-id="8303b-168">128 GB</span><span class="sxs-lookup"><span data-stu-id="8303b-168">128 GB</span></span>           | <span data-ttu-id="8303b-169">512 GB</span><span class="sxs-lookup"><span data-stu-id="8303b-169">512 GB</span></span>           | <span data-ttu-id="8303b-170">1 024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="8303b-170">1024 GB (1 TB)</span></span>   | <span data-ttu-id="8303b-171">2 048 GB (2TB)</span><span class="sxs-lookup"><span data-stu-id="8303b-171">2048 GB (2TB)</span></span>    | <span data-ttu-id="8303b-172">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="8303b-172">4095 GB (4 TB)</span></span>   | 
| <span data-ttu-id="8303b-173">IOPS per disk</span><span class="sxs-lookup"><span data-stu-id="8303b-173">IOPS per disk</span></span>       | <span data-ttu-id="8303b-174">500</span><span class="sxs-lookup"><span data-stu-id="8303b-174">500</span></span>              | <span data-ttu-id="8303b-175">500</span><span class="sxs-lookup"><span data-stu-id="8303b-175">500</span></span>              | <span data-ttu-id="8303b-176">500</span><span class="sxs-lookup"><span data-stu-id="8303b-176">500</span></span>              | <span data-ttu-id="8303b-177">500</span><span class="sxs-lookup"><span data-stu-id="8303b-177">500</span></span>              | <span data-ttu-id="8303b-178">500</span><span class="sxs-lookup"><span data-stu-id="8303b-178">500</span></span>              | <span data-ttu-id="8303b-179">500</span><span class="sxs-lookup"><span data-stu-id="8303b-179">500</span></span>             | <span data-ttu-id="8303b-180">500</span><span class="sxs-lookup"><span data-stu-id="8303b-180">500</span></span>              | 
| <span data-ttu-id="8303b-181">Dataflöde per disk</span><span class="sxs-lookup"><span data-stu-id="8303b-181">Throughput per disk</span></span> | <span data-ttu-id="8303b-182">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-182">60 MB per second</span></span> | <span data-ttu-id="8303b-183">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-183">60 MB per second</span></span> | <span data-ttu-id="8303b-184">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-184">60 MB per second</span></span> | <span data-ttu-id="8303b-185">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-185">60 MB per second</span></span> | <span data-ttu-id="8303b-186">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-186">60 MB per second</span></span> | <span data-ttu-id="8303b-187">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-187">60 MB per second</span></span> | <span data-ttu-id="8303b-188">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="8303b-188">60 MB per second</span></span> | 


### <a name="disk-caching-policy"></a><span data-ttu-id="8303b-189">Princip för cachelagring av disk</span><span class="sxs-lookup"><span data-stu-id="8303b-189">Disk caching policy</span></span> 

<span data-ttu-id="8303b-190">**Premium hanterade diskar**</span><span class="sxs-lookup"><span data-stu-id="8303b-190">**Premium Managed Disks**</span></span>

<span data-ttu-id="8303b-191">Princip för cachelagring av disk är som standard *skrivskyddad* för alla Premium datadiskar, och *Read-Write* för Premium operativsystemets disk som är kopplade till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8303b-191">By default, disk caching policy is *Read-Only* for all the Premium data disks, and *Read-Write* for the Premium operating system disk attached to the VM.</span></span> <span data-ttu-id="8303b-192">Den här inställningen rekommenderas för att uppnå optimal prestanda för ditt program IOs.</span><span class="sxs-lookup"><span data-stu-id="8303b-192">This configuration setting is recommended to achieve the optimal performance for your application’s IOs.</span></span> <span data-ttu-id="8303b-193">Inaktivera cachelagring på disk så att du kan få bättre prestanda för programmet för skrivintensiv eller lässkyddad datadiskar (till exempel loggfiler för SQL Server).</span><span class="sxs-lookup"><span data-stu-id="8303b-193">For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.</span></span>

### <a name="pricing"></a><span data-ttu-id="8303b-194">Prissättning</span><span class="sxs-lookup"><span data-stu-id="8303b-194">Pricing</span></span>

<span data-ttu-id="8303b-195">Granska de [priser för hanterade diskar](https://azure.microsoft.com/en-us/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="8303b-195">Review the [pricing for Managed Disks](https://azure.microsoft.com/en-us/pricing/details/managed-disks/).</span></span> <span data-ttu-id="8303b-196">Priser för hanterade Premiumdiskar är samma som ohanterad Premiumdiskar.</span><span class="sxs-lookup"><span data-stu-id="8303b-196">Pricing of Premium Managed Disks is same as the Premium Unmanaged Disks.</span></span> <span data-ttu-id="8303b-197">Men priser för hanterade standarddiskar skiljer sig från ohanterade standarddiskar.</span><span class="sxs-lookup"><span data-stu-id="8303b-197">But pricing for Standard Managed Disks is different than Standard Unmanaged Disks.</span></span>


## <a name="checklist"></a><span data-ttu-id="8303b-198">Checklista</span><span class="sxs-lookup"><span data-stu-id="8303b-198">Checklist</span></span>

1.  <span data-ttu-id="8303b-199">Om du migrerar till hanterade Premiumdiskar, kontrollera att den är tillgänglig i den region du migrerar till.</span><span class="sxs-lookup"><span data-stu-id="8303b-199">If you are migrating to Premium Managed Disks, make sure it is available in the region you are migrating to.</span></span>

2.  <span data-ttu-id="8303b-200">Bestäm den nya VM-serien som du kommer att använda.</span><span class="sxs-lookup"><span data-stu-id="8303b-200">Decide the new VM series you will be using.</span></span> <span data-ttu-id="8303b-201">Om du migrerar till hanterade Premiumdiskar bör det vara en Premium-lagring som är kompatibla.</span><span class="sxs-lookup"><span data-stu-id="8303b-201">It should be a Premium Storage capable if you are migrating to Premium Managed Disks.</span></span>

3.  <span data-ttu-id="8303b-202">Bestäm den exakta VM-storlek du vill använda som är tillgängliga i den region du migrerar till.</span><span class="sxs-lookup"><span data-stu-id="8303b-202">Decide the exact VM size you will use which are available in the region you are migrating to.</span></span> <span data-ttu-id="8303b-203">VM-storlek måste vara tillräckligt stor för att stödja antalet datadiskar som du har.</span><span class="sxs-lookup"><span data-stu-id="8303b-203">VM size needs to be large enough to support the number of data disks you have.</span></span> <span data-ttu-id="8303b-204">Om du har fyra datadiskar, måste den virtuella datorn ha minst två kärnor.</span><span class="sxs-lookup"><span data-stu-id="8303b-204">For example, if you have four data disks, the VM must have two or more cores.</span></span> <span data-ttu-id="8303b-205">Du kan också processorkraft, minne och nätverksbandbredd måste.</span><span class="sxs-lookup"><span data-stu-id="8303b-205">Also, consider processing power, memory and network bandwidth needs.</span></span>

4.  <span data-ttu-id="8303b-206">Har den aktuella informationen för VM praktisk, inklusive en lista över diskar och motsvarande VHD-blobbar.</span><span class="sxs-lookup"><span data-stu-id="8303b-206">Have the current VM details handy, including the list of disks and corresponding VHD blobs.</span></span>

<span data-ttu-id="8303b-207">Förbered ditt program för driftstopp.</span><span class="sxs-lookup"><span data-stu-id="8303b-207">Prepare your application for downtime.</span></span> <span data-ttu-id="8303b-208">För att göra en ren migrering, måste du stoppa all bearbetning i det aktuella systemet.</span><span class="sxs-lookup"><span data-stu-id="8303b-208">To do a clean migration, you have to stop all the processing in the current system.</span></span> <span data-ttu-id="8303b-209">Först då kan du få till konsekvent tillstånd som du kan migrera till den nya plattformen.</span><span class="sxs-lookup"><span data-stu-id="8303b-209">Only then you can get it to consistent state which you can migrate to the new platform.</span></span> <span data-ttu-id="8303b-210">Varaktighet för stilleståndstid beror på mängden data på diskarna att migrera.</span><span class="sxs-lookup"><span data-stu-id="8303b-210">Downtime duration depends on the amount of data in the disks to migrate.</span></span>


## <a name="migrate-the-vm"></a><span data-ttu-id="8303b-211">Migrera den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="8303b-211">Migrate the VM</span></span>

<span data-ttu-id="8303b-212">Förbered ditt program för driftstopp.</span><span class="sxs-lookup"><span data-stu-id="8303b-212">Prepare your application for downtime.</span></span> <span data-ttu-id="8303b-213">För att göra en ren migrering, måste du stoppa all bearbetning i det aktuella systemet.</span><span class="sxs-lookup"><span data-stu-id="8303b-213">To do a clean migration, you have to stop all the processing in the current system.</span></span> <span data-ttu-id="8303b-214">Först då kan du få till konsekvent tillstånd som du kan migrera till den nya plattformen.</span><span class="sxs-lookup"><span data-stu-id="8303b-214">Only then you can get it to consistent state which you can migrate to the new platform.</span></span> <span data-ttu-id="8303b-215">Varaktighet för stilleståndstid beror mängden data på diskarna att migrera.</span><span class="sxs-lookup"><span data-stu-id="8303b-215">Downtime duration depends the amount of data in the disks to migrate.</span></span>


1.  <span data-ttu-id="8303b-216">Innan du kan definiera de gemensamma parametrarna:</span><span class="sxs-lookup"><span data-stu-id="8303b-216">First, set the common parameters:</span></span>

    ```powershell
    $resourceGroupName = 'yourResourceGroupName'
    
    $location = 'your location' 
    
    $virtualNetworkName = 'yourExistingVirtualNetworkName'
    
    $virtualMachineName = 'yourVMName'
    
    $virtualMachineSize = 'Standard_DS3'
    
    $adminUserName = "youradminusername"
    
    $adminPassword = "yourpassword" | ConvertTo-SecureString -AsPlainText -Force
    
    $imageName = 'yourImageName'
    
    $osVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd'
    
    $dataVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk1.vhd'
    
    $dataDiskName = 'dataDisk1'
    ```

2.  <span data-ttu-id="8303b-217">Skapa en hanterad OS-disk med hjälp av den virtuella Hårddisken från den klassiska virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8303b-217">Create a managed OS disk using the VHD from the classic VM.</span></span>

    <span data-ttu-id="8303b-218">Se till att du har angett fullständiga URI för OS-VHD i parametern $osVhdUri.</span><span class="sxs-lookup"><span data-stu-id="8303b-218">Ensure that you have provided the complete URI of the OS VHD to the $osVhdUri parameter.</span></span> <span data-ttu-id="8303b-219">Ange dessutom **- AccountType** som **PremiumLRS** eller **StandardLRS** baserat på typ av diskar (Standard eller Premium) du migrerar till.</span><span class="sxs-lookup"><span data-stu-id="8303b-219">Also, enter **-AccountType** as **PremiumLRS** or **StandardLRS** based on type of disks (Premium or Standard) you are migrating to.</span></span>

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $osVhdUri) '
    -ResourceGroupName $resourceGroupName
    ```

3.  <span data-ttu-id="8303b-220">Koppla OS-disken till den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8303b-220">Attach the OS disk to the new VM.</span></span>

    ```powershell
    $VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $osDisk.Id '
    -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

4.  <span data-ttu-id="8303b-221">Skapa en hanterad datadisk från VHD-datafilen och Lägg till den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8303b-221">Create a managed data disk from the data VHD file and add it to the new VM.</span></span>

    ```powershell
    $dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreationDataCreateOption Import '
    -SourceUri $dataVhdUri ) -ResourceGroupName $resourceGroupName
    
    $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName '
    -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
    ```

5.  <span data-ttu-id="8303b-222">Skapa den nya virtuella datorn genom att ange offentlig IP, virtuella nätverk och nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="8303b-222">Create the new VM by setting public IP, Virtual Network and NIC.</span></span>

    ```powershell
    $publicIp = New-AzureRmPublicIpAddress -Name ($VirtualMachineName.ToLower()+'_ip') '
    -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
    
    $vnet = Get-AzureRmVirtualNetwork -Name $virtualNetworkName -ResourceGroupName $resourceGroupName
    
    $nic = New-AzureRmNetworkInterface -Name ($VirtualMachineName.ToLower()+'_nic') '
    -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id '
    -PublicIpAddressId $publicIp.Id
    
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nic.Id
    
    New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $resourceGroupName -Location $location
    ```

> [!NOTE]
><span data-ttu-id="8303b-223">Det kan finnas ytterligare steg behövs för ditt program som inte omfattas av den här guiden.</span><span class="sxs-lookup"><span data-stu-id="8303b-223">There may be additional steps necessary to support your application that is not be covered by this guide.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="8303b-224">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8303b-224">Next steps</span></span>

- <span data-ttu-id="8303b-225">Ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8303b-225">Connect to the virtual machine.</span></span> <span data-ttu-id="8303b-226">Instruktioner finns i [ansluta och logga in på en virtuell Azure-dator kör Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8303b-226">For instructions, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

