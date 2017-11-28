---
title: aaaMigrate klassiska VM-tooan ARM hanteras Disk VM | Microsoft Docs
description: "Migrera en enda Azure-VM från hello klassisk distribution modellen tooManaged diskar i hello Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: d8c4b9431f5dd8a071fcbc2ee36581a33f76ba62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manually-migrate-a-classic-vm-tooa-new-arm-managed-disk-vm-from-hello-vhd"></a><span data-ttu-id="2e7f2-103">Migrera en klassiska Virtuella tooa manuellt ny ARM hanteras Disk virtuell dator från hello VHD</span><span class="sxs-lookup"><span data-stu-id="2e7f2-103">Manually migrate a Classic VM tooa new ARM Managed Disk VM from hello VHD</span></span> 


<span data-ttu-id="2e7f2-104">Det här avsnittet hjälper du toomigrate dina befintliga virtuella Azure-datorer från hello klassiska distributionsmodellen för[hanterade diskar](managed-disks-overview.md) i hello Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-104">This section helps you toomigrate your existing Azure VMs from hello classic deployment model too[Managed Disks](managed-disks-overview.md) in hello Resource Manager deployment model.</span></span>


## <a name="plan-for-hello-migration-toomanaged-disks"></a><span data-ttu-id="2e7f2-105">Planera för migrering av hello tooManaged diskar</span><span class="sxs-lookup"><span data-stu-id="2e7f2-105">Plan for hello migration tooManaged Disks</span></span>

<span data-ttu-id="2e7f2-106">Det här avsnittet hjälper dig att toomake hello bästa beslut på VM- och diskresurser typer.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-106">This section helps you toomake hello best decision on VM and disk types.</span></span>


### <a name="location"></a><span data-ttu-id="2e7f2-107">Plats</span><span class="sxs-lookup"><span data-stu-id="2e7f2-107">Location</span></span>

<span data-ttu-id="2e7f2-108">Välj en plats där Azure hanterade diskar är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-108">Pick a location where Azure Managed Disks are available.</span></span> <span data-ttu-id="2e7f2-109">Om du migrerar tooPremium hanterade diskar kan också se till att Premium-lagring finns i hello region där du planerar toomigrate till.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-109">If you are migrating tooPremium Managed Disks, also ensure that Premium storage is available in hello region where you are planning toomigrate to.</span></span> <span data-ttu-id="2e7f2-110">Se [Azure Services byRegion](https://azure.microsoft.com/regions/#services) uppdaterad information om tillgängliga platser.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-110">See [Azure Services byRegion](https://azure.microsoft.com/regions/#services) for up-to-date information on available locations.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="2e7f2-111">VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="2e7f2-111">VM sizes</span></span>

<span data-ttu-id="2e7f2-112">Om du migrerar tooPremium hanterade diskar har tooupdate hello storleken på hello VM tooPremium kompatibla lagringsstorlek tillgängliga i hello-regionen där VM finns.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-112">If you are migrating tooPremium Managed Disks, you have tooupdate hello size of hello VM tooPremium Storage capable size available in hello region where VM is located.</span></span> <span data-ttu-id="2e7f2-113">Granska hello VM-storlekar som är Premium-lagring som är kompatibla.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-113">Review hello VM sizes that are Premium Storage capable.</span></span> <span data-ttu-id="2e7f2-114">hello Azure VM storlek specifikationer listas i [storlekar för virtuella datorer](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="2e7f2-114">hello Azure VM size specifications are listed in [Sizes for virtual machines](sizes.md).</span></span>
<span data-ttu-id="2e7f2-115">Granska hello prestandaegenskaper för virtuella datorer som fungerar med Premium-lagring och välj hello lämpligaste VM-storlek som bäst passar din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-115">Review hello performance characteristics of virtual machines that work with Premium Storage and choose hello most appropriate VM size that best suits your workload.</span></span> <span data-ttu-id="2e7f2-116">Kontrollera att det finns tillräckligt med bandbredd på VM toodrive hello disk trafiken.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-116">Make sure that there is sufficient bandwidth available on your VM toodrive hello disk traffic.</span></span>

### <a name="disk-sizes"></a><span data-ttu-id="2e7f2-117">Diskstorlekar</span><span class="sxs-lookup"><span data-stu-id="2e7f2-117">Disk sizes</span></span>

<span data-ttu-id="2e7f2-118">**Premium hanterade diskar**</span><span class="sxs-lookup"><span data-stu-id="2e7f2-118">**Premium Managed Disks**</span></span>

<span data-ttu-id="2e7f2-119">Det finns sju typer av premium hanterade diskar som kan användas med den virtuella datorn var och en har särskilda IOPs och genomströmning gränser.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-119">There are seven types of premium Managed disks that can be used with your VM and each has specific IOPs and throughput limits.</span></span> <span data-ttu-id="2e7f2-120">Överväg att dessa begränsningar när du väljer hello Premium disktyp för den virtuella datorn baserat på hello behoven för ditt program vad gäller kapacitet, prestanda, skalbarhet och belastning läses in.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-120">Consider these limits when choosing hello Premium disk type for your VM based on hello needs of your application in terms of capacity, performance, scalability, and peak loads.</span></span>

| <span data-ttu-id="2e7f2-121">Premium diskar typ</span><span class="sxs-lookup"><span data-stu-id="2e7f2-121">Premium Disks Type</span></span>  | <span data-ttu-id="2e7f2-122">P4</span><span class="sxs-lookup"><span data-stu-id="2e7f2-122">P4</span></span>    | <span data-ttu-id="2e7f2-123">P6</span><span class="sxs-lookup"><span data-stu-id="2e7f2-123">P6</span></span>    | <span data-ttu-id="2e7f2-124">P10</span><span class="sxs-lookup"><span data-stu-id="2e7f2-124">P10</span></span>   | <span data-ttu-id="2e7f2-125">P20</span><span class="sxs-lookup"><span data-stu-id="2e7f2-125">P20</span></span>   | <span data-ttu-id="2e7f2-126">P30</span><span class="sxs-lookup"><span data-stu-id="2e7f2-126">P30</span></span>   | <span data-ttu-id="2e7f2-127">P40</span><span class="sxs-lookup"><span data-stu-id="2e7f2-127">P40</span></span>   | <span data-ttu-id="2e7f2-128">P50</span><span class="sxs-lookup"><span data-stu-id="2e7f2-128">P50</span></span>   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| <span data-ttu-id="2e7f2-129">Diskstorlek</span><span class="sxs-lookup"><span data-stu-id="2e7f2-129">Disk size</span></span>           | <span data-ttu-id="2e7f2-130">128 GB</span><span class="sxs-lookup"><span data-stu-id="2e7f2-130">128 GB</span></span>| <span data-ttu-id="2e7f2-131">512 GB</span><span class="sxs-lookup"><span data-stu-id="2e7f2-131">512 GB</span></span>| <span data-ttu-id="2e7f2-132">128 GB</span><span class="sxs-lookup"><span data-stu-id="2e7f2-132">128 GB</span></span>| <span data-ttu-id="2e7f2-133">512 GB</span><span class="sxs-lookup"><span data-stu-id="2e7f2-133">512 GB</span></span>            | <span data-ttu-id="2e7f2-134">1 024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="2e7f2-134">1024 GB (1 TB)</span></span>    | <span data-ttu-id="2e7f2-135">2 048 GB (2 TB)</span><span class="sxs-lookup"><span data-stu-id="2e7f2-135">2048 GB (2 TB)</span></span>    | <span data-ttu-id="2e7f2-136">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="2e7f2-136">4095 GB (4 TB)</span></span>    | 
| <span data-ttu-id="2e7f2-137">IOPS per disk</span><span class="sxs-lookup"><span data-stu-id="2e7f2-137">IOPS per disk</span></span>       | <span data-ttu-id="2e7f2-138">120</span><span class="sxs-lookup"><span data-stu-id="2e7f2-138">120</span></span>   | <span data-ttu-id="2e7f2-139">240</span><span class="sxs-lookup"><span data-stu-id="2e7f2-139">240</span></span>   | <span data-ttu-id="2e7f2-140">500</span><span class="sxs-lookup"><span data-stu-id="2e7f2-140">500</span></span>   | <span data-ttu-id="2e7f2-141">2 300</span><span class="sxs-lookup"><span data-stu-id="2e7f2-141">2300</span></span>              | <span data-ttu-id="2e7f2-142">5000</span><span class="sxs-lookup"><span data-stu-id="2e7f2-142">5000</span></span>              | <span data-ttu-id="2e7f2-143">7500</span><span class="sxs-lookup"><span data-stu-id="2e7f2-143">7500</span></span>              | <span data-ttu-id="2e7f2-144">7500</span><span class="sxs-lookup"><span data-stu-id="2e7f2-144">7500</span></span>              | 
| <span data-ttu-id="2e7f2-145">Dataflöde per disk</span><span class="sxs-lookup"><span data-stu-id="2e7f2-145">Throughput per disk</span></span> | <span data-ttu-id="2e7f2-146">25 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-146">25 MB per second</span></span>  | <span data-ttu-id="2e7f2-147">50 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-147">50 MB per second</span></span>  | <span data-ttu-id="2e7f2-148">100 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-148">100 MB per second</span></span> | <span data-ttu-id="2e7f2-149">150 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-149">150 MB per second</span></span> | <span data-ttu-id="2e7f2-150">200 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-150">200 MB per second</span></span> | <span data-ttu-id="2e7f2-151">250 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-151">250 MB per second</span></span> | <span data-ttu-id="2e7f2-152">250 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-152">250 MB per second</span></span> | 

<span data-ttu-id="2e7f2-153">**Hanterade standarddiskar**</span><span class="sxs-lookup"><span data-stu-id="2e7f2-153">**Standard Managed Disks**</span></span>

<span data-ttu-id="2e7f2-154">Det finns sju typer av Standard hanterade diskar som kan användas med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-154">There are seven types of Standard Managed disks that can be used with your VM.</span></span> <span data-ttu-id="2e7f2-155">Var och en av dem har olika kapacitet men samma IOPS och genomströmning gränser.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-155">Each of them have different capacity but have same IOPS and throughput limits.</span></span> <span data-ttu-id="2e7f2-156">Välj hello typ av Standard hanterade diskar baserat på hello kapacitetsbehov för programmet.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-156">Choose hello type of Standard Managed disks based on hello capacity needs of your application.</span></span>

| <span data-ttu-id="2e7f2-157">Disk av standardtyp</span><span class="sxs-lookup"><span data-stu-id="2e7f2-157">Standard Disk Type</span></span>  | <span data-ttu-id="2e7f2-158">S4</span><span class="sxs-lookup"><span data-stu-id="2e7f2-158">S4</span></span>               | <span data-ttu-id="2e7f2-159">S6</span><span class="sxs-lookup"><span data-stu-id="2e7f2-159">S6</span></span>               | <span data-ttu-id="2e7f2-160">S10</span><span class="sxs-lookup"><span data-stu-id="2e7f2-160">S10</span></span>              | <span data-ttu-id="2e7f2-161">S20</span><span class="sxs-lookup"><span data-stu-id="2e7f2-161">S20</span></span>              | <span data-ttu-id="2e7f2-162">S30</span><span class="sxs-lookup"><span data-stu-id="2e7f2-162">S30</span></span>              | <span data-ttu-id="2e7f2-163">S40</span><span class="sxs-lookup"><span data-stu-id="2e7f2-163">S40</span></span>              | <span data-ttu-id="2e7f2-164">S50</span><span class="sxs-lookup"><span data-stu-id="2e7f2-164">S50</span></span>              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| <span data-ttu-id="2e7f2-165">Diskstorlek</span><span class="sxs-lookup"><span data-stu-id="2e7f2-165">Disk size</span></span>           | <span data-ttu-id="2e7f2-166">30 GB</span><span class="sxs-lookup"><span data-stu-id="2e7f2-166">30 GB</span></span>            | <span data-ttu-id="2e7f2-167">64 GB</span><span class="sxs-lookup"><span data-stu-id="2e7f2-167">64 GB</span></span>            | <span data-ttu-id="2e7f2-168">128 GB</span><span class="sxs-lookup"><span data-stu-id="2e7f2-168">128 GB</span></span>           | <span data-ttu-id="2e7f2-169">512 GB</span><span class="sxs-lookup"><span data-stu-id="2e7f2-169">512 GB</span></span>           | <span data-ttu-id="2e7f2-170">1 024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="2e7f2-170">1024 GB (1 TB)</span></span>   | <span data-ttu-id="2e7f2-171">2 048 GB (2TB)</span><span class="sxs-lookup"><span data-stu-id="2e7f2-171">2048 GB (2TB)</span></span>    | <span data-ttu-id="2e7f2-172">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="2e7f2-172">4095 GB (4 TB)</span></span>   | 
| <span data-ttu-id="2e7f2-173">IOPS per disk</span><span class="sxs-lookup"><span data-stu-id="2e7f2-173">IOPS per disk</span></span>       | <span data-ttu-id="2e7f2-174">500</span><span class="sxs-lookup"><span data-stu-id="2e7f2-174">500</span></span>              | <span data-ttu-id="2e7f2-175">500</span><span class="sxs-lookup"><span data-stu-id="2e7f2-175">500</span></span>              | <span data-ttu-id="2e7f2-176">500</span><span class="sxs-lookup"><span data-stu-id="2e7f2-176">500</span></span>              | <span data-ttu-id="2e7f2-177">500</span><span class="sxs-lookup"><span data-stu-id="2e7f2-177">500</span></span>              | <span data-ttu-id="2e7f2-178">500</span><span class="sxs-lookup"><span data-stu-id="2e7f2-178">500</span></span>              | <span data-ttu-id="2e7f2-179">500</span><span class="sxs-lookup"><span data-stu-id="2e7f2-179">500</span></span>             | <span data-ttu-id="2e7f2-180">500</span><span class="sxs-lookup"><span data-stu-id="2e7f2-180">500</span></span>              | 
| <span data-ttu-id="2e7f2-181">Dataflöde per disk</span><span class="sxs-lookup"><span data-stu-id="2e7f2-181">Throughput per disk</span></span> | <span data-ttu-id="2e7f2-182">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-182">60 MB per second</span></span> | <span data-ttu-id="2e7f2-183">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-183">60 MB per second</span></span> | <span data-ttu-id="2e7f2-184">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-184">60 MB per second</span></span> | <span data-ttu-id="2e7f2-185">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-185">60 MB per second</span></span> | <span data-ttu-id="2e7f2-186">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-186">60 MB per second</span></span> | <span data-ttu-id="2e7f2-187">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-187">60 MB per second</span></span> | <span data-ttu-id="2e7f2-188">60 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="2e7f2-188">60 MB per second</span></span> | 


### <a name="disk-caching-policy"></a><span data-ttu-id="2e7f2-189">Princip för cachelagring av disk</span><span class="sxs-lookup"><span data-stu-id="2e7f2-189">Disk caching policy</span></span> 

<span data-ttu-id="2e7f2-190">**Premium hanterade diskar**</span><span class="sxs-lookup"><span data-stu-id="2e7f2-190">**Premium Managed Disks**</span></span>

<span data-ttu-id="2e7f2-191">Princip för cachelagring av disk är som standard *skrivskyddad* för alla hello Premium datadiskar och *Read-Write* för hello Premium operativsystemdisken kopplade toohello VM.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-191">By default, disk caching policy is *Read-Only* for all hello Premium data disks, and *Read-Write* for hello Premium operating system disk attached toohello VM.</span></span> <span data-ttu-id="2e7f2-192">Den här konfigurationen rekommenderas tooachieve hello optimala prestanda för ditt program IOs.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-192">This configuration setting is recommended tooachieve hello optimal performance for your application’s IOs.</span></span> <span data-ttu-id="2e7f2-193">Inaktivera cachelagring på disk så att du kan få bättre prestanda för programmet för skrivintensiv eller lässkyddad datadiskar (till exempel loggfiler för SQL Server).</span><span class="sxs-lookup"><span data-stu-id="2e7f2-193">For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.</span></span>

### <a name="pricing"></a><span data-ttu-id="2e7f2-194">Prissättning</span><span class="sxs-lookup"><span data-stu-id="2e7f2-194">Pricing</span></span>

<span data-ttu-id="2e7f2-195">Granska hello [priser för hanterade diskar](https://azure.microsoft.com/en-us/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="2e7f2-195">Review hello [pricing for Managed Disks](https://azure.microsoft.com/en-us/pricing/details/managed-disks/).</span></span> <span data-ttu-id="2e7f2-196">Priser för hanterade Premiumdiskar är samma som hello ohanterade Premiumdiskar.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-196">Pricing of Premium Managed Disks is same as hello Premium Unmanaged Disks.</span></span> <span data-ttu-id="2e7f2-197">Men priser för hanterade standarddiskar skiljer sig från ohanterade standarddiskar.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-197">But pricing for Standard Managed Disks is different than Standard Unmanaged Disks.</span></span>


## <a name="checklist"></a><span data-ttu-id="2e7f2-198">Checklista</span><span class="sxs-lookup"><span data-stu-id="2e7f2-198">Checklist</span></span>

1.  <span data-ttu-id="2e7f2-199">Om du migrerar tooPremium hanterade diskar, kontrollera att den är tillgänglig i hello region du migrerar till.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-199">If you are migrating tooPremium Managed Disks, make sure it is available in hello region you are migrating to.</span></span>

2.  <span data-ttu-id="2e7f2-200">Bestäm hello ny virtuell dator-serie du kommer att använda.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-200">Decide hello new VM series you will be using.</span></span> <span data-ttu-id="2e7f2-201">Det bör vara en Premium-lagring kan om du migrerar tooPremium hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-201">It should be a Premium Storage capable if you are migrating tooPremium Managed Disks.</span></span>

3.  <span data-ttu-id="2e7f2-202">Bestäm hello exakt VM-storlek du vill använda som är tillgängliga i hello region du migrerar till.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-202">Decide hello exact VM size you will use which are available in hello region you are migrating to.</span></span> <span data-ttu-id="2e7f2-203">VM-storlek måste toobe tillräckligt stor toosupport hello antalet datadiskar som du har.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-203">VM size needs toobe large enough toosupport hello number of data disks you have.</span></span> <span data-ttu-id="2e7f2-204">Om du har fyra datadiskar måste hello VM ha två eller flera kärnor.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-204">For example, if you have four data disks, hello VM must have two or more cores.</span></span> <span data-ttu-id="2e7f2-205">Du kan också processorkraft, minne och nätverksbandbredd måste.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-205">Also, consider processing power, memory and network bandwidth needs.</span></span>

4.  <span data-ttu-id="2e7f2-206">Ha hello aktuella VM information praktisk, inklusive hello lista över diskar och motsvarande VHD-blobbar.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-206">Have hello current VM details handy, including hello list of disks and corresponding VHD blobs.</span></span>

<span data-ttu-id="2e7f2-207">Förbered ditt program för driftstopp.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-207">Prepare your application for downtime.</span></span> <span data-ttu-id="2e7f2-208">toodo en ren migrering, har du toostop all hello bearbetning i hello nuvarande systemet.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-208">toodo a clean migration, you have toostop all hello processing in hello current system.</span></span> <span data-ttu-id="2e7f2-209">Först då kan du få tooconsistent tillstånd som du kan migrera nya toohello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-209">Only then you can get it tooconsistent state which you can migrate toohello new platform.</span></span> <span data-ttu-id="2e7f2-210">Varaktighet för stilleståndstid är beroende av hello mängden data i hello diskar toomigrate.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-210">Downtime duration depends on hello amount of data in hello disks toomigrate.</span></span>


## <a name="migrate-hello-vm"></a><span data-ttu-id="2e7f2-211">Migrera hello VM</span><span class="sxs-lookup"><span data-stu-id="2e7f2-211">Migrate hello VM</span></span>

<span data-ttu-id="2e7f2-212">Förbered ditt program för driftstopp.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-212">Prepare your application for downtime.</span></span> <span data-ttu-id="2e7f2-213">toodo en ren migrering, har du toostop all hello bearbetning i hello nuvarande systemet.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-213">toodo a clean migration, you have toostop all hello processing in hello current system.</span></span> <span data-ttu-id="2e7f2-214">Först då kan du få tooconsistent tillstånd som du kan migrera nya toohello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-214">Only then you can get it tooconsistent state which you can migrate toohello new platform.</span></span> <span data-ttu-id="2e7f2-215">Varaktighet för stilleståndstid beror hello mängden data i hello diskar toomigrate.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-215">Downtime duration depends hello amount of data in hello disks toomigrate.</span></span>


1.  <span data-ttu-id="2e7f2-216">Ange först hello gemensamma parametrar:</span><span class="sxs-lookup"><span data-stu-id="2e7f2-216">First, set hello common parameters:</span></span>

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

2.  <span data-ttu-id="2e7f2-217">Skapa en hanterad OS-disk hello VHD från hello klassiska VM.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-217">Create a managed OS disk using hello VHD from hello classic VM.</span></span>

    <span data-ttu-id="2e7f2-218">Se till att du har tillhandahållit hello slutföra hello OS VHD toohello $osVhdUri parameter-URI.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-218">Ensure that you have provided hello complete URI of hello OS VHD toohello $osVhdUri parameter.</span></span> <span data-ttu-id="2e7f2-219">Ange dessutom **- AccountType** som **PremiumLRS** eller **StandardLRS** baserat på typ av diskar (Standard eller Premium) du migrerar till.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-219">Also, enter **-AccountType** as **PremiumLRS** or **StandardLRS** based on type of disks (Premium or Standard) you are migrating to.</span></span>

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $osVhdUri) '
    -ResourceGroupName $resourceGroupName
    ```

3.  <span data-ttu-id="2e7f2-220">Koppla hello OS disk toohello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-220">Attach hello OS disk toohello new VM.</span></span>

    ```powershell
    $VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $osDisk.Id '
    -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

4.  <span data-ttu-id="2e7f2-221">Skapa en hanterad datadisk från hello data VHD-filen och Lägg till den toohello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-221">Create a managed data disk from hello data VHD file and add it toohello new VM.</span></span>

    ```powershell
    $dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreationDataCreateOption Import '
    -SourceUri $dataVhdUri ) -ResourceGroupName $resourceGroupName
    
    $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName '
    -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
    ```

5.  <span data-ttu-id="2e7f2-222">Skapa hello ny virtuell dator genom att ange offentlig IP, virtuella nätverk och nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-222">Create hello new VM by setting public IP, Virtual Network and NIC.</span></span>

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
><span data-ttu-id="2e7f2-223">Det kan finnas ytterligare steg krävs toosupport ditt program som inte omfattas av den här guiden.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-223">There may be additional steps necessary toosupport your application that is not be covered by this guide.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="2e7f2-224">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2e7f2-224">Next steps</span></span>

- <span data-ttu-id="2e7f2-225">Ansluta toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2e7f2-225">Connect toohello virtual machine.</span></span> <span data-ttu-id="2e7f2-226">Instruktioner finns i [hur tooconnect och logga in tooan Azure virtuella datorn kör Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2e7f2-226">For instructions, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

