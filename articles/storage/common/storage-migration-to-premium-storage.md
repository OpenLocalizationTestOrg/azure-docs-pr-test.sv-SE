---
title: Migrera virtuella datorer till Azure Premium-lagring | Microsoft Docs
description: "Migrera dina befintliga virtuella datorer till Azure Premium-lagring. Premium-lagring ger stöd för I/O-intensiva arbetsbelastningar som körs på Azure Virtual Machines diskar med hög prestanda, låg latens."
services: storage
documentationcenter: na
author: yuemlu
manager: tadb
editor: tysonn
ms.assetid: 272250b3-fd4e-41d2-8e34-fd8cc341ec87
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: yuemlu
ms.openlocfilehash: ca893f87b155a92c457e3bf6d9d39aaf86bf5fb3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="migrating-to-azure-premium-storage-unmanaged-disks"></a><span data-ttu-id="6f94d-104">Migrera till Azure Premium-lagring (ohanterade diskar)</span><span class="sxs-lookup"><span data-stu-id="6f94d-104">Migrating to Azure Premium Storage (Unmanaged Disks)</span></span>

> [!NOTE]
> <span data-ttu-id="6f94d-105">Den här artikeln beskriver hur du migrerar en virtuell dator som använder ohanterade standarddiskar till en virtuell dator som använder ohanterade premiumdiskar.</span><span class="sxs-lookup"><span data-stu-id="6f94d-105">This article discusses how to migrate a VM that uses unmanaged standard disks to a VM that uses unmanaged premium disks.</span></span> <span data-ttu-id="6f94d-106">Vi rekommenderar att du använder Azure hanterade diskar för nya virtuella datorer och att du konverterar tidigare ohanterade diskar till hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="6f94d-106">We recommend that you use Azure Managed Disks for new VMs, and that you convert your previous unmanaged disks to managed disks.</span></span> <span data-ttu-id="6f94d-107">Hanterade diskar referensen underliggande storage-konton, så du behöver.</span><span class="sxs-lookup"><span data-stu-id="6f94d-107">Managed Disks handle the underlying storage accounts so you don't have to.</span></span> <span data-ttu-id="6f94d-108">Mer information, se vår [översikt för hanterade diskar](../../virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6f94d-108">For more information, please see our [Managed Disks Overview](../../virtual-machines/windows/managed-disks-overview.md).</span></span>
>

<span data-ttu-id="6f94d-109">Azure Premium Storage ger stöd för virtuella datorer som körs I/O-intensiva arbetsbelastningar diskar med hög prestanda, låg latens.</span><span class="sxs-lookup"><span data-stu-id="6f94d-109">Azure Premium Storage delivers high-performance, low-latency disk support for virtual machines running I/O-intensive workloads.</span></span> <span data-ttu-id="6f94d-110">Du kan dra nytta av hastighet och prestanda för dessa diskar genom att migrera programmets Virtuella diskar till Azure Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="6f94d-110">You can take advantage of the speed and performance of these disks by migrating your application's VM disks to Azure Premium Storage.</span></span>

<span data-ttu-id="6f94d-111">Syftet med den här guiden är att nya användare på Azure Premium Storage bättre förbereda för att få en smidig övergång från systemet till Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="6f94d-111">The purpose of this guide is to help new users of Azure Premium Storage better prepare to make a smooth transition from their current system to Premium Storage.</span></span> <span data-ttu-id="6f94d-112">Guiden löser tre huvudkomponenter i den här processen:</span><span class="sxs-lookup"><span data-stu-id="6f94d-112">The guide addresses three of the key components in this process:</span></span>

* [<span data-ttu-id="6f94d-113">Planera för migrering till Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="6f94d-113">Plan for the Migration to Premium Storage</span></span>](#plan-the-migration-to-premium-storage)
* [<span data-ttu-id="6f94d-114">Förbereda och kopiera virtuella hårddiskar (VHD) till Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="6f94d-114">Prepare and Copy Virtual Hard Disks (VHDs) to Premium Storage</span></span>](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [<span data-ttu-id="6f94d-115">Skapa Azure virtuell dator med Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="6f94d-115">Create Azure Virtual Machine using Premium Storage</span></span>](#create-azure-virtual-machine-using-premium-storage)

<span data-ttu-id="6f94d-116">Du kan migrera virtuella datorer från andra plattformar till Azure Premium-lagring, eller så kan du migrera befintliga virtuella Azure-datorer från standardlagring till Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="6f94d-116">You can either migrate VMs from other platforms to Azure Premium Storage or migrate existing Azure VMs from Standard Storage to Premium Storage.</span></span> <span data-ttu-id="6f94d-117">Den här guiden beskriver steg för båda två scenarier.</span><span class="sxs-lookup"><span data-stu-id="6f94d-117">This guide covers steps for both two scenarios.</span></span> <span data-ttu-id="6f94d-118">Följ stegen som anges i avsnittet relevanta beroende på ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="6f94d-118">Follow the steps specified in the relevant section depending on your scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="6f94d-119">Du hittar en översikt över funktioner och priser för Premium-lagring i Premium Storage: [högpresterande lagring för arbetsbelastningar på virtuella Azure](storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="6f94d-119">You can find a feature overview and pricing of Premium Storage in Premium Storage: [High-Performance Storage for Azure Virtual Machine Workloads](storage-premium-storage.md).</span></span> <span data-ttu-id="6f94d-120">Vi rekommenderar att du migrerar alla virtuella diskar som kräver hög IOPS till Azure Premium-lagring för bästa prestanda för ditt program.</span><span class="sxs-lookup"><span data-stu-id="6f94d-120">We recommend migrating any virtual machine disk requiring high IOPS to Azure Premium Storage for the best performance for your application.</span></span> <span data-ttu-id="6f94d-121">Om disken inte kräver hög IOPS, kan du begränsa kostnader genom att upprätthålla i standardlagring som lagrar data för virtuell disk på hårddiskar (HDD) i stället för SSD-enheter.</span><span class="sxs-lookup"><span data-stu-id="6f94d-121">If your disk does not require high IOPS, you can limit costs by maintaining it in Standard Storage, which stores virtual machine disk data on Hard Disk Drives (HDDs) instead of SSDs.</span></span>
>

<span data-ttu-id="6f94d-122">Slutföra migreringsprocessen i sin helhet kan kräva ytterligare åtgärder både före och efter stegen i den här guiden.</span><span class="sxs-lookup"><span data-stu-id="6f94d-122">Completing the migration process in its entirety may require additional actions both before and after the steps provided in this guide.</span></span> <span data-ttu-id="6f94d-123">Exempel inkluderar konfigurering av virtuella nätverk eller slutpunkter eller ändrar koden själva programmet som kan kräva vissa avbrott i ditt program.</span><span class="sxs-lookup"><span data-stu-id="6f94d-123">Examples include configuring virtual networks or endpoints or making code changes within the application itself which may require some downtime in your application.</span></span> <span data-ttu-id="6f94d-124">Dessa åtgärder är unika för varje program och du bör utföra dem tillsammans med stegen i den här guiden för att göra fullständiga övergången till Premium-lagring som sömlös som möjligt.</span><span class="sxs-lookup"><span data-stu-id="6f94d-124">These actions are unique to each application, and you should complete them along with the steps provided in this guide to make the full transition to Premium Storage as seamless as possible.</span></span>

## <span data-ttu-id="6f94d-125"><a name="plan-the-migration-to-premium-storage"></a>Planera för migrering till Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="6f94d-125"><a name="plan-the-migration-to-premium-storage"></a>Plan for the migration to Premium Storage</span></span>
<span data-ttu-id="6f94d-126">Det här avsnittet säkerställer att du är redo att följa stegen i migreringen i den här artikeln och hjälper dig att göra det bästa på VM- och diskresurser typer.</span><span class="sxs-lookup"><span data-stu-id="6f94d-126">This section ensures that you are ready to follow the migration steps in this article, and helps you to make the best decision on VM and Disk types.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="6f94d-127">Krav</span><span class="sxs-lookup"><span data-stu-id="6f94d-127">Prerequisites</span></span>
* <span data-ttu-id="6f94d-128">Du behöver en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6f94d-128">You will need an Azure subscription.</span></span> <span data-ttu-id="6f94d-129">Om du inte har någon, kan du skapa en månad [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/) prenumeration eller besök [priser för Azure](https://azure.microsoft.com/pricing/) fler alternativ.</span><span class="sxs-lookup"><span data-stu-id="6f94d-129">If you don't have one, you can create a one-month [free trial](https://azure.microsoft.com/pricing/free-trial/) subscription or visit [Azure Pricing](https://azure.microsoft.com/pricing/) for more options.</span></span>
* <span data-ttu-id="6f94d-130">Om du vill köra PowerShell-cmdlets, måste Microsoft Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="6f94d-130">To execute PowerShell cmdlets, you will need the Microsoft Azure PowerShell module.</span></span> <span data-ttu-id="6f94d-131">Information om installationsplatser och installationsanvisningar finns i [Installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6f94d-131">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for the install point and installation instructions.</span></span>
* <span data-ttu-id="6f94d-132">När du planerar att använda Azure virtuella datorer som körs på Premium-lagring måste du använda Premium-lagring kan virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6f94d-132">When you plan to use Azure VMs running on Premium Storage, you need to use the Premium Storage capable VMs.</span></span> <span data-ttu-id="6f94d-133">Du kan använda Standard- och Premium-lagring diskar med Premium-lagring kan virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6f94d-133">You can use both Standard and Premium Storage disks with Premium Storage capable VMs.</span></span> <span data-ttu-id="6f94d-134">Premium-lagringsdiskar kommer att vara tillgänglig med flera VM-typer i framtiden.</span><span class="sxs-lookup"><span data-stu-id="6f94d-134">Premium storage disks will be available with more VM types in the future.</span></span> <span data-ttu-id="6f94d-135">Läs mer på alla tillgängliga Virtuella Azure-disktyper och storlekar, [storlekar för virtuella datorer](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) och [storlekar för molntjänster](../../cloud-services/cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="6f94d-135">For more information on all available Azure VM disk types and sizes, see [Sizes for virtual machines](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Sizes for Cloud Services](../../cloud-services/cloud-services-sizes-specs.md).</span></span>

### <a name="considerations"></a><span data-ttu-id="6f94d-136">Överväganden</span><span class="sxs-lookup"><span data-stu-id="6f94d-136">Considerations</span></span>
<span data-ttu-id="6f94d-137">En Azure VM stöder bifoga flera Premium-lagring diskar så att dina program kan ha upp till 256 TB lagringsutrymme per virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="6f94d-137">An Azure VM supports attaching several Premium Storage disks so that your applications can have up to 256 TB of storage per VM.</span></span> <span data-ttu-id="6f94d-138">Med Premium-lagring kan dina program uppnå (i/o-åtgärder per sekund) för 80 000 IOPS per virtuell dator och 2000 MB per andra diskgenomflödet per virtuell dator med mycket låg latens för läsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="6f94d-138">With Premium Storage, your applications can achieve 80,000 IOPS (input/output operations per second) per VM and 2000 MB per second disk throughput per VM with extremely low latencies for read operations.</span></span> <span data-ttu-id="6f94d-139">Har du alternativ på en mängd olika virtuella datorer och diskar.</span><span class="sxs-lookup"><span data-stu-id="6f94d-139">You have options in a variety of VMs and Disks.</span></span> <span data-ttu-id="6f94d-140">Det här avsnittet är att hjälpa dig att hitta ett alternativ som bäst passar din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="6f94d-140">This section is to help you to find an option that best suits your workload.</span></span>

#### <a name="vm-sizes"></a><span data-ttu-id="6f94d-141">VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="6f94d-141">VM sizes</span></span>
<span data-ttu-id="6f94d-142">Specifikationer för Azure VM-storleken anges i [storlekar för virtuella datorer](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6f94d-142">The Azure VM size specifications are listed in [Sizes for virtual machines](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="6f94d-143">Granska prestandaegenskaper för virtuella datorer som fungerar med Premium-lagring och välja den lämpligaste VM-storlek som bäst passar din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="6f94d-143">Review the performance characteristics of virtual machines that work with Premium Storage and choose the most appropriate VM size that best suits your workload.</span></span> <span data-ttu-id="6f94d-144">Kontrollera att det finns tillräckligt med bandbredd på den virtuella datorn för att ge disk-trafik.</span><span class="sxs-lookup"><span data-stu-id="6f94d-144">Make sure that there is sufficient bandwidth available on your VM to drive the disk traffic.</span></span>

#### <a name="disk-sizes"></a><span data-ttu-id="6f94d-145">Diskstorlekar</span><span class="sxs-lookup"><span data-stu-id="6f94d-145">Disk sizes</span></span>
<span data-ttu-id="6f94d-146">Det finns fem typer av diskar som kan användas med den virtuella datorn och var och en har särskilda IOPs och genomströmning gränser.</span><span class="sxs-lookup"><span data-stu-id="6f94d-146">There are five types of disks that can be used with your VM and each has specific IOPs and throughput limits.</span></span> <span data-ttu-id="6f94d-147">Ta hänsyn till dessa begränsningar när du väljer typ av disk för den virtuella datorn baserat på dina behov av ditt program vad gäller kapacitet, prestanda, skalbarhet och belastning läser in.</span><span class="sxs-lookup"><span data-stu-id="6f94d-147">Take into consideration these limits when choosing the disk type for your VM based on the needs of your application in terms of capacity, performance, scalability, and peak loads.</span></span>

| <span data-ttu-id="6f94d-148">Premium diskar typ</span><span class="sxs-lookup"><span data-stu-id="6f94d-148">Premium Disks Type</span></span>  | <span data-ttu-id="6f94d-149">P10</span><span class="sxs-lookup"><span data-stu-id="6f94d-149">P10</span></span>   | <span data-ttu-id="6f94d-150">P20</span><span class="sxs-lookup"><span data-stu-id="6f94d-150">P20</span></span>   | <span data-ttu-id="6f94d-151">P30</span><span class="sxs-lookup"><span data-stu-id="6f94d-151">P30</span></span>            | <span data-ttu-id="6f94d-152">P40</span><span class="sxs-lookup"><span data-stu-id="6f94d-152">P40</span></span>            | <span data-ttu-id="6f94d-153">P50</span><span class="sxs-lookup"><span data-stu-id="6f94d-153">P50</span></span>            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| <span data-ttu-id="6f94d-154">Diskstorlek</span><span class="sxs-lookup"><span data-stu-id="6f94d-154">Disk size</span></span>           | <span data-ttu-id="6f94d-155">128 GB</span><span class="sxs-lookup"><span data-stu-id="6f94d-155">128 GB</span></span>| <span data-ttu-id="6f94d-156">512 GB</span><span class="sxs-lookup"><span data-stu-id="6f94d-156">512 GB</span></span>| <span data-ttu-id="6f94d-157">1 024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="6f94d-157">1024 GB (1 TB)</span></span> | <span data-ttu-id="6f94d-158">2 048 GB (2 TB)</span><span class="sxs-lookup"><span data-stu-id="6f94d-158">2048 GB (2 TB)</span></span> | <span data-ttu-id="6f94d-159">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="6f94d-159">4095 GB (4 TB)</span></span> | 
| <span data-ttu-id="6f94d-160">IOPS per disk</span><span class="sxs-lookup"><span data-stu-id="6f94d-160">IOPS per disk</span></span>       | <span data-ttu-id="6f94d-161">500</span><span class="sxs-lookup"><span data-stu-id="6f94d-161">500</span></span>   | <span data-ttu-id="6f94d-162">2 300</span><span class="sxs-lookup"><span data-stu-id="6f94d-162">2300</span></span>  | <span data-ttu-id="6f94d-163">5000</span><span class="sxs-lookup"><span data-stu-id="6f94d-163">5000</span></span>           | <span data-ttu-id="6f94d-164">7500</span><span class="sxs-lookup"><span data-stu-id="6f94d-164">7500</span></span>           | <span data-ttu-id="6f94d-165">7500</span><span class="sxs-lookup"><span data-stu-id="6f94d-165">7500</span></span>           | 
| <span data-ttu-id="6f94d-166">Dataflöde per disk</span><span class="sxs-lookup"><span data-stu-id="6f94d-166">Throughput per disk</span></span> | <span data-ttu-id="6f94d-167">100 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="6f94d-167">100 MB per second</span></span> | <span data-ttu-id="6f94d-168">150 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="6f94d-168">150 MB per second</span></span> | <span data-ttu-id="6f94d-169">200 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="6f94d-169">200 MB per second</span></span> | <span data-ttu-id="6f94d-170">250 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="6f94d-170">250 MB per second</span></span> | <span data-ttu-id="6f94d-171">250 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="6f94d-171">250 MB per second</span></span> |

<span data-ttu-id="6f94d-172">Beroende på din arbetsbelastning avgör du om ytterligare hårddiskar krävs för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="6f94d-172">Depending on your workload, determine if additional data disks are necessary for your VM.</span></span> <span data-ttu-id="6f94d-173">Du kan bifoga flera beständiga datadiskar till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="6f94d-173">You can attach several persistent data disks to your VM.</span></span> <span data-ttu-id="6f94d-174">Om det behövs kan stripe du över diskarna att öka kapaciteten och prestandan för volymen.</span><span class="sxs-lookup"><span data-stu-id="6f94d-174">If needed, you can stripe across the disks to increase the capacity and performance of the volume.</span></span> <span data-ttu-id="6f94d-175">(Se vad som är Disk Striping [här](storage-premium-storage-performance.md#disk-striping).) Om du stripe-Premium-lagring datadiskar med hjälp av [lagringsutrymmen][4], bör du konfigurera den med en kolumn för varje disk som används.</span><span class="sxs-lookup"><span data-stu-id="6f94d-175">(See what is Disk Striping [here](storage-premium-storage-performance.md#disk-striping).) If you stripe Premium Storage data disks using [Storage Spaces][4], you should configure it with one column for each disk that is used.</span></span> <span data-ttu-id="6f94d-176">Annars vara prestandan stripe-volym lägre än väntat på grund av en ojämn fördelning av trafik över diskar.</span><span class="sxs-lookup"><span data-stu-id="6f94d-176">Otherwise, the overall performance of the striped volume may be lower than expected due to uneven distribution of traffic across the disks.</span></span> <span data-ttu-id="6f94d-177">För virtuella Linux-datorer kan du använda den *mdadm* verktyg för att uppnå samma.</span><span class="sxs-lookup"><span data-stu-id="6f94d-177">For Linux VMs you can use the *mdadm* utility to achieve the same.</span></span> <span data-ttu-id="6f94d-178">Se artikeln [konfigurera programvara RAID på Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mer information.</span><span class="sxs-lookup"><span data-stu-id="6f94d-178">See article [Configure Software RAID on Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for details.</span></span>

#### <a name="storage-account-scalability-targets"></a><span data-ttu-id="6f94d-179">Skalbarhetsmål för lagringskontot</span><span class="sxs-lookup"><span data-stu-id="6f94d-179">Storage account scalability targets</span></span>
<span data-ttu-id="6f94d-180">Premium-lagringskonton har följande skalbarhetsmål förutom det [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="6f94d-180">Premium Storage accounts have the following scalability targets in addition to the [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).</span></span> <span data-ttu-id="6f94d-181">Om din programkrav överskrider skalbarhetsmål för ett enda lagringskonto, skapa ditt program att använda flera lagringskonton och partitionera data mellan dessa lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="6f94d-181">If your application requirements exceed the scalability targets of a single storage account, build your application to use multiple storage accounts, and partition your data across those storage accounts.</span></span>

| <span data-ttu-id="6f94d-182">Total kapacitet</span><span class="sxs-lookup"><span data-stu-id="6f94d-182">Total Account Capacity</span></span> | <span data-ttu-id="6f94d-183">Totala bandbredden för ett lokalt Redundant Lagringskonto</span><span class="sxs-lookup"><span data-stu-id="6f94d-183">Total Bandwidth for a Locally Redundant Storage Account</span></span> |
|:--- |:--- |
| <span data-ttu-id="6f94d-184">Disken kapacitet: 35TB</span><span class="sxs-lookup"><span data-stu-id="6f94d-184">Disk capacity: 35TB</span></span><br /><span data-ttu-id="6f94d-185">Ögonblicksbild kapacitet: 10 TB</span><span class="sxs-lookup"><span data-stu-id="6f94d-185">Snapshot capacity: 10 TB</span></span> |<span data-ttu-id="6f94d-186">Upp till 50 Gigabit per sekund för inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="6f94d-186">Up to 50 gigabits per second for Inbound + Outbound</span></span> |

<span data-ttu-id="6f94d-187">Mer information om specifikationer för Premium-lagring, kolla [skalbarhets- och prestandamål när du använder Premiumlagring](storage-premium-storage.md#scalability-and-performance-targets).</span><span class="sxs-lookup"><span data-stu-id="6f94d-187">For the more information on Premium Storage specifications, check out [Scalability and Performance Targets when using Premium Storage](storage-premium-storage.md#scalability-and-performance-targets).</span></span>

#### <a name="disk-caching-policy"></a><span data-ttu-id="6f94d-188">Princip för cachelagring av disk</span><span class="sxs-lookup"><span data-stu-id="6f94d-188">Disk caching policy</span></span>
<span data-ttu-id="6f94d-189">Princip för cachelagring av disk är som standard *skrivskyddad* för alla Premium datadiskar, och *Read-Write* för Premium operativsystemets disk som är kopplade till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="6f94d-189">By default, disk caching policy is *Read-Only* for all the Premium data disks, and *Read-Write* for the Premium operating system disk attached to the VM.</span></span> <span data-ttu-id="6f94d-190">Den här inställningen rekommenderas för att uppnå optimal prestanda för ditt program IOs.</span><span class="sxs-lookup"><span data-stu-id="6f94d-190">This configuration setting is recommended to achieve the optimal performance for your application's IOs.</span></span> <span data-ttu-id="6f94d-191">Inaktivera cachelagring på disk så att du kan få bättre prestanda för programmet för skrivintensiv eller lässkyddad datadiskar (till exempel loggfiler för SQL Server).</span><span class="sxs-lookup"><span data-stu-id="6f94d-191">For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.</span></span> <span data-ttu-id="6f94d-192">Inställningar för cachelagring för befintliga datadiskar kan uppdateras med hjälp av [Azure Portal](https://portal.azure.com) eller *- HostCaching* parameter för den *Set AzureDataDisk* cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6f94d-192">The cache settings for existing data disks can be updated using [Azure Portal](https://portal.azure.com) or the *-HostCaching* parameter of the *Set-AzureDataDisk* cmdlet.</span></span>

#### <a name="location"></a><span data-ttu-id="6f94d-193">Plats</span><span class="sxs-lookup"><span data-stu-id="6f94d-193">Location</span></span>
<span data-ttu-id="6f94d-194">Välj en plats där Azure Premium-lagring är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="6f94d-194">Pick a location where Azure Premium Storage is available.</span></span> <span data-ttu-id="6f94d-195">Se [Azure-tjänster efter Region](https://azure.microsoft.com/regions/#services) uppdaterad information om tillgängliga platser.</span><span class="sxs-lookup"><span data-stu-id="6f94d-195">See [Azure Services by Region](https://azure.microsoft.com/regions/#services) for up-to-date information on available locations.</span></span> <span data-ttu-id="6f94d-196">Virtuella datorer finns i samma region som det lagringskonto som lagrar diskarna för den virtuella datorn får mycket bättre prestanda än om de finns i olika områden.</span><span class="sxs-lookup"><span data-stu-id="6f94d-196">VMs located in the same region as the Storage account that stores the disks for the VM will give much better performance than if they are in separate regions.</span></span>

#### <a name="other-azure-vm-configuration-settings"></a><span data-ttu-id="6f94d-197">Andra Virtuella Azure-konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="6f94d-197">Other Azure VM configuration settings</span></span>
<span data-ttu-id="6f94d-198">När du skapar en Azure VM, uppmanas du att konfigurera vissa inställningar för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="6f94d-198">When creating an Azure VM, you will be asked to configure certain VM settings.</span></span> <span data-ttu-id="6f94d-199">Kom ihåg att några inställningar korrigeras livslängden för den virtuella datorn medan du kan ändra eller lägga till andra senare.</span><span class="sxs-lookup"><span data-stu-id="6f94d-199">Remember, few settings are fixed for the lifetime of the VM, while you can modify or add others later.</span></span> <span data-ttu-id="6f94d-200">Granska dessa Virtuella Azure-konfigurationsinställningar och se till att dessa är korrekt konfigurerade som motsvarar dina behov av arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="6f94d-200">Review these Azure VM configuration settings and make sure that these are configured appropriately to match your workload requirements.</span></span>

### <a name="optimization"></a><span data-ttu-id="6f94d-201">Optimering</span><span class="sxs-lookup"><span data-stu-id="6f94d-201">Optimization</span></span>
<span data-ttu-id="6f94d-202">[Azure Premium Storage: Utforma för högprestanda](storage-premium-storage-performance.md) innehåller riktlinjer för att skapa program med höga prestanda med Azure Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="6f94d-202">[Azure Premium Storage: Design for High Performance](storage-premium-storage-performance.md) provides guidelines for building high-performance applications using Azure Premium Storage.</span></span> <span data-ttu-id="6f94d-203">Du kan följa riktlinjerna i kombination med prestandarelaterade metodtips gäller för tekniker som används av ditt program.</span><span class="sxs-lookup"><span data-stu-id="6f94d-203">You can follow the guidelines combined with performance best practices applicable to technologies used by your application.</span></span>

## <span data-ttu-id="6f94d-204"><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Förbereda och kopiera virtuella hårddiskar (VHD) till Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="6f94d-204"><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Prepare and copy virtual hard disks (VHDs) to Premium Storage</span></span>
<span data-ttu-id="6f94d-205">Följande avsnitt innehåller riktlinjer för att förbereda virtuella hårddiskar från den virtuella datorn och kopiera virtuella hårddiskar till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6f94d-205">The following section provides guidelines for preparing VHDs from your VM and copying VHDs to Azure Storage.</span></span>

* [<span data-ttu-id="6f94d-206">Scenario 1: ”jag migrera befintliga virtuella Azure-datorer till Azure Premium-lagring”.</span><span class="sxs-lookup"><span data-stu-id="6f94d-206">Scenario 1: "I am migrating existing Azure VMs to Azure Premium Storage."</span></span>](#scenario1)
* [<span data-ttu-id="6f94d-207">Scenario 2: ”jag migrera virtuella datorer från andra plattformar till Azure Premium-lagring”.</span><span class="sxs-lookup"><span data-stu-id="6f94d-207">Scenario 2: "I am migrating VMs from other platforms to Azure Premium Storage."</span></span>](#scenario2)

### <a name="prerequisites"></a><span data-ttu-id="6f94d-208">Krav</span><span class="sxs-lookup"><span data-stu-id="6f94d-208">Prerequisites</span></span>
<span data-ttu-id="6f94d-209">För att förbereda de virtuella hårddiskarna för migrering, behöver du:</span><span class="sxs-lookup"><span data-stu-id="6f94d-209">To prepare the VHDs for migration, you'll need:</span></span>

* <span data-ttu-id="6f94d-210">En Azure-prenumeration, ett lagringskonto och en behållare i det lagringskonto som du kan kopiera den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="6f94d-210">An Azure subscription, a storage account, and a container in that storage account to which you can copy your VHD.</span></span> <span data-ttu-id="6f94d-211">Observera att mål-lagringskontot kan vara ett Standard eller Premium-lagring konto utifrån dina behov.</span><span class="sxs-lookup"><span data-stu-id="6f94d-211">Note that the destination storage account can be a Standard or Premium Storage account depending on your requirement.</span></span>
* <span data-ttu-id="6f94d-212">Ett verktyg för att generalisera den virtuella Hårddisken om du tänker skapa flera VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="6f94d-212">A tool to generalize the VHD if you plan to create multiple VM instances from it.</span></span> <span data-ttu-id="6f94d-213">Till exempel sysprep för Windows eller Radnr i virtuell sysprep för Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="6f94d-213">For example, sysprep for Windows or virt-sysprep for Ubuntu.</span></span>
* <span data-ttu-id="6f94d-214">Ett verktyg för att överföra VHD-filen till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="6f94d-214">A tool to upload the VHD file to the Storage account.</span></span> <span data-ttu-id="6f94d-215">Se [överföra data med kommandoradsverktyget Azcopy](storage-use-azcopy.md) eller Använd en [Azure Lagringsutforskaren](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx).</span><span class="sxs-lookup"><span data-stu-id="6f94d-215">See [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md) or use an [Azure storage explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx).</span></span> <span data-ttu-id="6f94d-216">Den här guiden beskriver kopiera den virtuella Hårddisken med hjälp av verktyget AzCopy.</span><span class="sxs-lookup"><span data-stu-id="6f94d-216">This guide describes copying your VHD using the AzCopy tool.</span></span>

> [!NOTE]
> <span data-ttu-id="6f94d-217">Om du väljer alternativet för synkron kopia med AzCopy, kopierar du den virtuella Hårddisken genom att köra något av dessa verktyg från en virtuell Azure-dator som är i samma region som lagringskontot mål för optimala prestanda.</span><span class="sxs-lookup"><span data-stu-id="6f94d-217">If you choose synchronous copy option with AzCopy, for optimal performance, copy your VHD by running one of these tools from an Azure VM that is in the same region as the destination storage account.</span></span> <span data-ttu-id="6f94d-218">Om du kopierar en VHD från en Azure-dator i en annan region, gå din långsammare.</span><span class="sxs-lookup"><span data-stu-id="6f94d-218">If you are copying a VHD from an Azure VM in a different region, your performance may be slower.</span></span>
>
> <span data-ttu-id="6f94d-219">Överväg att kopiera stora mängder data över begränsad bandbredd, [använder tjänsten Azure Import/Export för att överföra data till Blob Storage](../storage-import-export-service.md); du kan överföra dina data genom att leverera hårddiskar till ett Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="6f94d-219">For copying a large amount of data over limited bandwidth, consider [using the Azure Import/Export service to transfer data to Blob Storage](../storage-import-export-service.md); this allows you to transfer your data by shipping hard disk drives to an Azure datacenter.</span></span> <span data-ttu-id="6f94d-220">Du kan använda tjänsten Azure Import/Export för att kopiera data till ett standardlagringskonto endast.</span><span class="sxs-lookup"><span data-stu-id="6f94d-220">You can use the Azure Import/Export service to copy data to a standard storage account only.</span></span> <span data-ttu-id="6f94d-221">När data är i ditt lagringskonto som standard, kan du använda antingen den [kopiera Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) eller AzCopy för att överföra data till premium-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="6f94d-221">Once the data is in your standard storage account, you can use either the [Copy Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) or AzCopy to transfer the data to your premium storage account.</span></span>
>
> <span data-ttu-id="6f94d-222">Observera att Microsoft Azure bara stöder fast storlek VHD-filer.</span><span class="sxs-lookup"><span data-stu-id="6f94d-222">Note that Microsoft Azure only supports fixed size VHD files.</span></span> <span data-ttu-id="6f94d-223">VHDX-filer eller dynamiska virtuella hårddiskar stöds inte.</span><span class="sxs-lookup"><span data-stu-id="6f94d-223">VHDX files or dynamic VHDs are not supported.</span></span> <span data-ttu-id="6f94d-224">Om du har en dynamisk virtuell Hårddisk kan du konvertera den till fast storlek med den [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6f94d-224">If you have a dynamic VHD, you can convert it to fixed size using the [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) cmdlet.</span></span>
>
>

### <span data-ttu-id="6f94d-225"><a name="scenario1"></a>Scenario 1: ”jag migrera befintliga virtuella Azure-datorer till Azure Premium-lagring”.</span><span class="sxs-lookup"><span data-stu-id="6f94d-225"><a name="scenario1"></a>Scenario 1: "I am migrating existing Azure VMs to Azure Premium Storage."</span></span>
<span data-ttu-id="6f94d-226">Om du migrerar befintliga virtuella Azure-datorer, stoppa den virtuella datorn och förbereda virtuella hårddiskar per typ av virtuell Hårddisk som du vill kopiera den virtuella Hårddisken med AzCopy eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6f94d-226">If you are migrating existing Azure VMs, stop the VM, prepare VHDs per the type of VHD you want, and then copy the VHD with AzCopy or PowerShell.</span></span>

<span data-ttu-id="6f94d-227">Den virtuella datorn måste vara helt på att migrera ett rent tillstånd.</span><span class="sxs-lookup"><span data-stu-id="6f94d-227">The VM needs to be completely down to migrate a clean state.</span></span> <span data-ttu-id="6f94d-228">Det blir ett driftstopp tills migreringen är klar.</span><span class="sxs-lookup"><span data-stu-id="6f94d-228">There will be a downtime until the migration completes.</span></span>

#### <a name="step-1-prepare-vhds-for-migration"></a><span data-ttu-id="6f94d-229">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="6f94d-229">Step 1.</span></span> <span data-ttu-id="6f94d-230">Förbereda virtuella hårddiskar för migrering</span><span class="sxs-lookup"><span data-stu-id="6f94d-230">Prepare VHDs for migration</span></span>
<span data-ttu-id="6f94d-231">Om du migrerar befintliga virtuella Azure-datorer till Premium-lagring kan vara den virtuella Hårddisken:</span><span class="sxs-lookup"><span data-stu-id="6f94d-231">If you are migrating existing Azure VMs to Premium Storage, your VHD may be:</span></span>

* <span data-ttu-id="6f94d-232">En generaliserad operativsystemsavbildning</span><span class="sxs-lookup"><span data-stu-id="6f94d-232">A generalized operating system image</span></span>
* <span data-ttu-id="6f94d-233">En unik operativsystemdisk</span><span class="sxs-lookup"><span data-stu-id="6f94d-233">A unique operating system disk</span></span>
* <span data-ttu-id="6f94d-234">En datadisk</span><span class="sxs-lookup"><span data-stu-id="6f94d-234">A data disk</span></span>

<span data-ttu-id="6f94d-235">Nedan går vi igenom dessa 3 scenarier för att förbereda den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="6f94d-235">Below we walk through these 3 scenarios for preparing your VHD.</span></span>

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a><span data-ttu-id="6f94d-236">Använd en generaliserad virtuell Hårddisk med operativsystemet för att skapa flera VM-instanser</span><span class="sxs-lookup"><span data-stu-id="6f94d-236">Use a generalized Operating System VHD to create multiple VM instances</span></span>
<span data-ttu-id="6f94d-237">Om du överför en virtuell Hårddisk som ska användas för att skapa flera generiska Azure VM-instanser måste du först generalisera VHD med sysprep-verktyget.</span><span class="sxs-lookup"><span data-stu-id="6f94d-237">If you are uploading a VHD that will be used to create multiple generic Azure VM instances, you must first generalize VHD using a sysprep utility.</span></span> <span data-ttu-id="6f94d-238">Detta gäller en virtuell Hårddisk som är lokalt eller i molnet.</span><span class="sxs-lookup"><span data-stu-id="6f94d-238">This applies to a VHD that is on-premises or in the cloud.</span></span> <span data-ttu-id="6f94d-239">Sysprep tar bort alla datorspecifik information från den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="6f94d-239">Sysprep removes any machine-specific information from the VHD.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6f94d-240">Ta en ögonblicksbild eller säkerhetskopiera den virtuella datorn innan generalisera den.</span><span class="sxs-lookup"><span data-stu-id="6f94d-240">Take a snapshot or backup your VM before generalizing it.</span></span> <span data-ttu-id="6f94d-241">Kör sysprep avbryts och frigöra VM-instans.</span><span class="sxs-lookup"><span data-stu-id="6f94d-241">Running sysprep will stop and deallocate the VM instance.</span></span> <span data-ttu-id="6f94d-242">Följ stegen nedan för att sysprep en Windows OS-VHD.</span><span class="sxs-lookup"><span data-stu-id="6f94d-242">Follow steps below to sysprep a Windows OS VHD.</span></span> <span data-ttu-id="6f94d-243">Observera att köra kommandot Sysprep måste du stänga av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="6f94d-243">Note that running the Sysprep command will require you to shut down the virtual machine.</span></span> <span data-ttu-id="6f94d-244">Mer information om Sysprep finns [Sysprep översikt](http://technet.microsoft.com/library/hh825209.aspx) eller [Teknisk referens för Sysprep](http://technet.microsoft.com/library/cc766049.aspx).</span><span class="sxs-lookup"><span data-stu-id="6f94d-244">For more information about Sysprep, see [Sysprep Overview](http://technet.microsoft.com/library/hh825209.aspx) or [Sysprep Technical Reference](http://technet.microsoft.com/library/cc766049.aspx).</span></span>
>
>

1. <span data-ttu-id="6f94d-245">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="6f94d-245">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="6f94d-246">Ange följande kommando för att öppna Sysprep:</span><span class="sxs-lookup"><span data-stu-id="6f94d-246">Enter the following command to open Sysprep:</span></span>

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. <span data-ttu-id="6f94d-247">Välj ange System Out of Box Experience (OOBE) i systemförberedelseverktyget, väljer kryssrutan Generalize, väljer **avstängning**, och klicka sedan på **OK**som visas i bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="6f94d-247">In the System Preparation Tool, select Enter System Out-of-Box Experience (OOBE), select the Generalize check box, select **Shutdown**, and then click **OK**, as shown in the image below.</span></span> <span data-ttu-id="6f94d-248">Sysprep att generalisera operativsystemet och stänga av datorn.</span><span class="sxs-lookup"><span data-stu-id="6f94d-248">Sysprep will generalize the operating system and shut down the system.</span></span>

    ![][1]

<span data-ttu-id="6f94d-249">För en Ubuntu VM, använda Radnr i virtuell sysprep för att uppnå samma.</span><span class="sxs-lookup"><span data-stu-id="6f94d-249">For an Ubuntu VM, use virt-sysprep to achieve the same.</span></span> <span data-ttu-id="6f94d-250">Se [Radnr i virtuell sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6f94d-250">See [virt-sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) for more details.</span></span> <span data-ttu-id="6f94d-251">Se även vissa med öppen källkod [etablering av Linux-servrar programvara](http://www.cyberciti.biz/tips/server-provisioning-software.html) för andra Linux-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="6f94d-251">See also some of the open source [Linux Server Provisioning software](http://www.cyberciti.biz/tips/server-provisioning-software.html) for other Linux operating systems.</span></span>

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a><span data-ttu-id="6f94d-252">Använda en unik virtuell Hårddisk med operativsystemet för att skapa en enda VM-instans</span><span class="sxs-lookup"><span data-stu-id="6f94d-252">Use a unique Operating System VHD to create a single VM instance</span></span>
<span data-ttu-id="6f94d-253">Om du har ett program som körs på den virtuella datorn som kräver att datorn specifika data kan du inte generalisera den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="6f94d-253">If you have an application running on the VM which requires the machine specific data, do not generalize the VHD.</span></span> <span data-ttu-id="6f94d-254">En ej generaliserad virtuell Hårddisk kan användas för att skapa en unik Virtuella Azure-instans.</span><span class="sxs-lookup"><span data-stu-id="6f94d-254">A non-generalized VHD can be used to create a unique Azure VM instance.</span></span> <span data-ttu-id="6f94d-255">Till exempel om du har en domänkontrollant på den virtuella Hårddisken kan gör köra sysprep det ineffektiv som en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="6f94d-255">For example, if you have Domain Controller on your VHD, executing sysprep will make it ineffective as a Domain Controller.</span></span> <span data-ttu-id="6f94d-256">Granska de program som körs på den virtuella datorn och effekten av att köra sysprep på dem. innan generalisera den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="6f94d-256">Review the applications running on your VM and the impact of running sysprep on them before generalizing the VHD.</span></span>

##### <a name="register-data-disk-vhd"></a><span data-ttu-id="6f94d-257">Registrera datadisk VHD</span><span class="sxs-lookup"><span data-stu-id="6f94d-257">Register data disk VHD</span></span>
<span data-ttu-id="6f94d-258">Om du har datadiskar i Azure som ska migreras, måste du kontrollera de virtuella datorer som använder diskarna data är avstängd.</span><span class="sxs-lookup"><span data-stu-id="6f94d-258">If you have data disks in Azure to be migrated, you must make sure the VMs that use these data disks are shut down.</span></span>

<span data-ttu-id="6f94d-259">Följ stegen nedan för att kopiera VHD till Azure Premium-lagring och registrera den som en etablerade datadisk.</span><span class="sxs-lookup"><span data-stu-id="6f94d-259">Follow the steps described below to copy VHD to Azure Premium Storage and register it as a provisioned data disk.</span></span>

#### <a name="step-2-create-the-destination-for-your-vhd"></a><span data-ttu-id="6f94d-260">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="6f94d-260">Step 2.</span></span> <span data-ttu-id="6f94d-261">Skapa mål för den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="6f94d-261">Create the destination for your VHD</span></span>
<span data-ttu-id="6f94d-262">Skapa ett lagringskonto för att underhålla de virtuella hårddiskarna.</span><span class="sxs-lookup"><span data-stu-id="6f94d-262">Create a storage account for maintaining your VHDs.</span></span> <span data-ttu-id="6f94d-263">Tänk på följande när du planerar var du vill lagra de virtuella hårddiskarna:</span><span class="sxs-lookup"><span data-stu-id="6f94d-263">Consider the following points when planning where to store your VHDs:</span></span>

* <span data-ttu-id="6f94d-264">Mål Premium storage-konto.</span><span class="sxs-lookup"><span data-stu-id="6f94d-264">The target Premium storage account.</span></span>
* <span data-ttu-id="6f94d-265">Lagringsplats för kontot måste vara samma som Premium-lagring kan virtuella Azure-datorer skapas i det sista steget.</span><span class="sxs-lookup"><span data-stu-id="6f94d-265">The storage account location must be same as Premium Storage capable Azure VMs you will create in the final stage.</span></span> <span data-ttu-id="6f94d-266">Du kan kopiera till ett nytt lagringskonto eller planerar att använda samma lagringskonto baserat på dina behov.</span><span class="sxs-lookup"><span data-stu-id="6f94d-266">You could copy to a new storage account, or plan to use the same storage account based on your needs.</span></span>
* <span data-ttu-id="6f94d-267">Kopiera och spara lagringskontonyckel på mål-lagringskontot för nästa steg.</span><span class="sxs-lookup"><span data-stu-id="6f94d-267">Copy and save the storage account key of the destination storage account for the next stage.</span></span>

<span data-ttu-id="6f94d-268">Du kan välja att behålla vissa datadiskar i ett standardlagringskonto (till exempel diskar med kyligare lagring) för datadiskar, men rekommenderas du att flytta alla data för produktion arbetsbelastningen att använda premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="6f94d-268">For data disks, you can choose to keep some data disks in a standard storage account (for example, disks that have cooler storage), but we strongly recommend you moving all data for production workload to use premium storage.</span></span>

#### <span data-ttu-id="6f94d-269"><a name="copy-vhd-with-azcopy-or-powershell"></a>Steg 3.</span><span class="sxs-lookup"><span data-stu-id="6f94d-269"><a name="copy-vhd-with-azcopy-or-powershell"></a>Step 3.</span></span> <span data-ttu-id="6f94d-270">Kopiera VHD med AzCopy eller PowerShell</span><span class="sxs-lookup"><span data-stu-id="6f94d-270">Copy VHD with AzCopy or PowerShell</span></span>
<span data-ttu-id="6f94d-271">Du behöver att hitta din behållaren sökvägen och lagringsutrymmet kontonyckel bearbeta någon av de här två alternativen.</span><span class="sxs-lookup"><span data-stu-id="6f94d-271">You will need to find your container path and storage account key to process either of these two options.</span></span> <span data-ttu-id="6f94d-272">Behållaren sökvägen och lagringsutrymmet kontonyckel finns i **Azure Portal** > **lagring**.</span><span class="sxs-lookup"><span data-stu-id="6f94d-272">Container path and storage account key can be found in **Azure Portal** > **Storage**.</span></span> <span data-ttu-id="6f94d-273">Behållarens Webbadress kommer att som ”https://myaccount.blob.core.windows.net/mycontainer/”.</span><span class="sxs-lookup"><span data-stu-id="6f94d-273">The container URL will be like "https://myaccount.blob.core.windows.net/mycontainer/".</span></span>

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a><span data-ttu-id="6f94d-274">Alternativ 1: Kopiera en virtuell Hårddisk med AzCopy (asynkron kopia)</span><span class="sxs-lookup"><span data-stu-id="6f94d-274">Option 1: Copy a VHD with AzCopy (Asynchronous copy)</span></span>
<span data-ttu-id="6f94d-275">Med AzCopy kan överföra du enkelt den virtuella Hårddisken via Internet.</span><span class="sxs-lookup"><span data-stu-id="6f94d-275">Using AzCopy, you can easily upload the VHD over the Internet.</span></span> <span data-ttu-id="6f94d-276">Detta kan ta tid beroende på storleken på de virtuella hårddiskarna.</span><span class="sxs-lookup"><span data-stu-id="6f94d-276">Depending on the size of the VHDs, this may take time.</span></span> <span data-ttu-id="6f94d-277">Kom ihåg att kontrollera ingång-/ utgång lagringskontogränser när du använder det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="6f94d-277">Remember to check the storage account ingress/egress limits when using this option.</span></span> <span data-ttu-id="6f94d-278">Se [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="6f94d-278">See [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) for details.</span></span>

1. <span data-ttu-id="6f94d-279">Hämta och installera AzCopy härifrån: [senaste versionen av AzCopy](http://aka.ms/downloadazcopy)</span><span class="sxs-lookup"><span data-stu-id="6f94d-279">Download and install AzCopy from here: [Latest version of AzCopy](http://aka.ms/downloadazcopy)</span></span>
2. <span data-ttu-id="6f94d-280">Öppna Azure PowerShell och gå till mappen där AzCopy är installerat.</span><span class="sxs-lookup"><span data-stu-id="6f94d-280">Open Azure PowerShell and go to the folder where AzCopy is installed.</span></span>
3. <span data-ttu-id="6f94d-281">Använd följande kommando för att kopiera VHD-filen från ”källa” till ”mål”.</span><span class="sxs-lookup"><span data-stu-id="6f94d-281">Use the following command to copy the VHD file from "Source" to "Destination".</span></span>

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    <span data-ttu-id="6f94d-282">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6f94d-282">Example:</span></span>

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```

    <span data-ttu-id="6f94d-283">Nedan följer beskrivningar av de parametrar som används i AzCopy-kommandot:</span><span class="sxs-lookup"><span data-stu-id="6f94d-283">Here are descriptions of the parameters used in the AzCopy command:</span></span>

   * <span data-ttu-id="6f94d-284">**/ Källa:  *&lt;källa&gt;:***  platsen för mappen eller lagring behållarens Webbadress som innehåller den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="6f94d-284">**/Source: *&lt;source&gt;:*** Location of the folder or storage container URL that contains the VHD.</span></span>
   * <span data-ttu-id="6f94d-285">**/ SourceKey:  *&lt;källa kontonyckel&gt;:***  lagringskontonyckel på lagringskontot för källa.</span><span class="sxs-lookup"><span data-stu-id="6f94d-285">**/SourceKey: *&lt;source-account-key&gt;:*** Storage account key of the source storage account.</span></span>
   * <span data-ttu-id="6f94d-286">**/ Dest:  *&lt;mål&gt;:***  lagring behållar-URL för att kopiera den virtuella Hårddisken till.</span><span class="sxs-lookup"><span data-stu-id="6f94d-286">**/Dest: *&lt;destination&gt;:*** Storage container URL to copy the VHD to.</span></span>
   * <span data-ttu-id="6f94d-287">**/ DestKey:  *&lt;dest kontonyckel&gt;:***  lagringskontonyckel på mål-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="6f94d-287">**/DestKey: *&lt;dest-account-key&gt;:*** Storage account key of the destination storage account.</span></span>
   * <span data-ttu-id="6f94d-288">**/ Mönster:  *&lt;filnamn&gt;:***  ange filnamnet för den virtuella Hårddisken ska kopieras.</span><span class="sxs-lookup"><span data-stu-id="6f94d-288">**/Pattern: *&lt;file-name&gt;:*** Specify the file name of the VHD to copy.</span></span>

<span data-ttu-id="6f94d-289">Mer information om hur du använder AzCopy verktyget, se [överföra data med kommandoradsverktyget Azcopy](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="6f94d-289">For details on using AzCopy tool, see [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a><span data-ttu-id="6f94d-290">Alternativ 2: Kopiera en VHD med PowerShell (Synchronized kopia)</span><span class="sxs-lookup"><span data-stu-id="6f94d-290">Option 2: Copy a VHD with PowerShell (Synchronized copy)</span></span>
<span data-ttu-id="6f94d-291">Du kan också kopiera VHD-filen med hjälp av PowerShell-cmdleten Start-AzureStorageBlobCopy.</span><span class="sxs-lookup"><span data-stu-id="6f94d-291">You can also copy the VHD file using the PowerShell cmdlet Start-AzureStorageBlobCopy.</span></span> <span data-ttu-id="6f94d-292">Använd följande kommando på Azure PowerShell för att kopiera VHD.</span><span class="sxs-lookup"><span data-stu-id="6f94d-292">Use the following command on Azure PowerShell to copy VHD.</span></span> <span data-ttu-id="6f94d-293">Ersätt värdena i <> med motsvarande värden från din käll- och storage-konto.</span><span class="sxs-lookup"><span data-stu-id="6f94d-293">Replace the values in <> with corresponding values from your source and destination storage account.</span></span> <span data-ttu-id="6f94d-294">Om du vill använda det här kommandot måste du ha en behållare som kallas virtuella hårddiskar på ditt mål-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="6f94d-294">To use this command, you must have a container called vhds in your destination storage account.</span></span> <span data-ttu-id="6f94d-295">Om behållaren inte finns skapa en innan kommandot körs.</span><span class="sxs-lookup"><span data-stu-id="6f94d-295">If the container doesn't exist, create one before running the command.</span></span>

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

<span data-ttu-id="6f94d-296">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6f94d-296">Example:</span></span>

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <span data-ttu-id="6f94d-297"><a name="scenario2"></a>Scenario 2: ”jag migrera virtuella datorer från andra plattformar till Azure Premium-lagring”.</span><span class="sxs-lookup"><span data-stu-id="6f94d-297"><a name="scenario2"></a>Scenario 2: "I am migrating VMs from other platforms to Azure Premium Storage."</span></span>
<span data-ttu-id="6f94d-298">Om du migrerar VHD från icke - Azure lagringsutrymmet i molnet till Azure, måste du först exportera den virtuella Hårddisken till en lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="6f94d-298">If you are migrating VHD from non-Azure Cloud Storage to Azure, you must first export the VHD to a local directory.</span></span> <span data-ttu-id="6f94d-299">Ha fullständig källsökvägen för den lokala katalogen där VHD lagras praktiska och sedan använda AzCopy för att överföra den till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6f94d-299">Have the complete source path of the local directory where VHD is stored handy, and then using AzCopy to upload it to Azure Storage.</span></span>

#### <a name="step-1-export-vhd-to-a-local-directory"></a><span data-ttu-id="6f94d-300">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="6f94d-300">Step 1.</span></span> <span data-ttu-id="6f94d-301">Exportera virtuell Hårddisk till en lokal katalog</span><span class="sxs-lookup"><span data-stu-id="6f94d-301">Export VHD to a local directory</span></span>
##### <a name="copy-a-vhd-from-aws"></a><span data-ttu-id="6f94d-302">Kopiera en VHD från AWS</span><span class="sxs-lookup"><span data-stu-id="6f94d-302">Copy a VHD from AWS</span></span>
1. <span data-ttu-id="6f94d-303">Om du använder AWS exportera EC2-instansen till en virtuell Hårddisk i ett Amazon S3-bucket.</span><span class="sxs-lookup"><span data-stu-id="6f94d-303">If you are using AWS, export the EC2 instance to a VHD in an Amazon S3 bucket.</span></span> <span data-ttu-id="6f94d-304">Följ stegen som beskrivs i dokumentationen för Amazon för export av Amazon EC2 instanser som du vill installera verktyget Amazon EC2 kommandoradsgränssnittet (CLI) och kör kommandot create-instans-export-aktivitet för att exportera EC2-instansen till en VHD-fil.</span><span class="sxs-lookup"><span data-stu-id="6f94d-304">Follow the steps described in the Amazon documentation for Exporting Amazon EC2 Instances to install the Amazon EC2 command-line interface (CLI) tool and run the create-instance-export-task command to export the EC2 instance to a VHD file.</span></span> <span data-ttu-id="6f94d-305">Se till att använda **VHD** för DISKEN &#95; BILDEN & #95. FORMATET variabeln när du kör den **skapa-instans-export-aktivitet** kommando.</span><span class="sxs-lookup"><span data-stu-id="6f94d-305">Be sure to use **VHD** for the DISK&#95;IMAGE&#95;FORMAT variable when running the **create-instance-export-task** command.</span></span> <span data-ttu-id="6f94d-306">Exporterade VHD-filen sparas i en Amazon S3-bucket som du anger under den här processen.</span><span class="sxs-lookup"><span data-stu-id="6f94d-306">The exported VHD file is saved in the Amazon S3 bucket you designate during that process.</span></span>

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. <span data-ttu-id="6f94d-307">Hämta VHD-filen från S3-bucket.</span><span class="sxs-lookup"><span data-stu-id="6f94d-307">Download the VHD file from the S3 bucket.</span></span> <span data-ttu-id="6f94d-308">Välj sedan VHD-filen **åtgärder** > **hämta**.</span><span class="sxs-lookup"><span data-stu-id="6f94d-308">Select the VHD file, then **Actions** > **Download**.</span></span>

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a><span data-ttu-id="6f94d-309">Kopiera en VHD från andra Azure-moln</span><span class="sxs-lookup"><span data-stu-id="6f94d-309">Copy a VHD from other non-Azure cloud</span></span>
<span data-ttu-id="6f94d-310">Om du migrerar VHD från icke - Azure lagringsutrymmet i molnet till Azure, måste du först exportera den virtuella Hårddisken till en lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="6f94d-310">If you are migrating VHD from non-Azure Cloud Storage to Azure, you must first export the VHD to a local directory.</span></span> <span data-ttu-id="6f94d-311">Kopiera fullständiga sökvägen till den lokala katalogen där VHD lagras.</span><span class="sxs-lookup"><span data-stu-id="6f94d-311">Copy the complete source path of the local directory where VHD is stored.</span></span>

##### <a name="copy-a-vhd-from-on-premises"></a><span data-ttu-id="6f94d-312">Kopiera en VHD från lokala</span><span class="sxs-lookup"><span data-stu-id="6f94d-312">Copy a VHD from on-premises</span></span>
<span data-ttu-id="6f94d-313">Om du migrerar VHD från en lokal miljö, behöver fullständiga källsökvägen där VHD lagras.</span><span class="sxs-lookup"><span data-stu-id="6f94d-313">If you are migrating VHD from an on-premises environment, you will need the complete source path where VHD is stored.</span></span> <span data-ttu-id="6f94d-314">Sökvägen kan vara en server eller en filresurs.</span><span class="sxs-lookup"><span data-stu-id="6f94d-314">The source path could be a server location or file share.</span></span>

#### <a name="step-2-create-the-destination-for-your-vhd"></a><span data-ttu-id="6f94d-315">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="6f94d-315">Step 2.</span></span> <span data-ttu-id="6f94d-316">Skapa mål för den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="6f94d-316">Create the destination for your VHD</span></span>
<span data-ttu-id="6f94d-317">Skapa ett lagringskonto för att underhålla de virtuella hårddiskarna.</span><span class="sxs-lookup"><span data-stu-id="6f94d-317">Create a storage account for maintaining your VHDs.</span></span> <span data-ttu-id="6f94d-318">Tänk på följande när du planerar var du vill lagra de virtuella hårddiskarna:</span><span class="sxs-lookup"><span data-stu-id="6f94d-318">Consider the following points when planning where to store your VHDs:</span></span>

* <span data-ttu-id="6f94d-319">Mål-lagringskontot kan vara standard eller premium storage beroende på din programkrav.</span><span class="sxs-lookup"><span data-stu-id="6f94d-319">The target storage account could be standard or premium storage depending on your application requirement.</span></span>
* <span data-ttu-id="6f94d-320">Lagringskontots region måste vara samma som Premium-lagring kan virtuella Azure-datorer skapas i det sista steget.</span><span class="sxs-lookup"><span data-stu-id="6f94d-320">The storage account region must be same as Premium Storage capable Azure VMs you will create in the final stage.</span></span> <span data-ttu-id="6f94d-321">Du kan kopiera till ett nytt lagringskonto eller planerar att använda samma lagringskonto baserat på dina behov.</span><span class="sxs-lookup"><span data-stu-id="6f94d-321">You could copy to a new storage account, or plan to use the same storage account based on your needs.</span></span>
* <span data-ttu-id="6f94d-322">Kopiera och spara lagringskontonyckel på mål-lagringskontot för nästa steg.</span><span class="sxs-lookup"><span data-stu-id="6f94d-322">Copy and save the storage account key of the destination storage account for the next stage.</span></span>

<span data-ttu-id="6f94d-323">Vi rekommenderar starkt att du flyttar alla data för produktion arbetsbelastningen att använda premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="6f94d-323">We strongly recommend you moving all data for production workload to use premium storage.</span></span>

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a><span data-ttu-id="6f94d-324">Steg 3.</span><span class="sxs-lookup"><span data-stu-id="6f94d-324">Step 3.</span></span> <span data-ttu-id="6f94d-325">Överför den virtuella Hårddisken till Azure-lagring</span><span class="sxs-lookup"><span data-stu-id="6f94d-325">Upload the VHD to Azure Storage</span></span>
<span data-ttu-id="6f94d-326">Nu när du har den virtuella Hårddisken i den lokala katalogen kan använda du AzCopy eller AzurePowerShell för att överföra VHD-filen till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6f94d-326">Now that you have your VHD in the local directory, you can use AzCopy or AzurePowerShell to upload the .vhd file to Azure Storage.</span></span> <span data-ttu-id="6f94d-327">Båda alternativen finns här:</span><span class="sxs-lookup"><span data-stu-id="6f94d-327">Both options are provided here:</span></span>

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a><span data-ttu-id="6f94d-328">Alternativ 1: Använda Azure PowerShell Add-AzureVhd för att överföra VHD-filen</span><span class="sxs-lookup"><span data-stu-id="6f94d-328">Option 1: Using Azure PowerShell Add-AzureVhd to upload the .vhd file</span></span>

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

<span data-ttu-id="6f94d-329">Ett exempel <Uri> kanske ***”https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd”***.</span><span class="sxs-lookup"><span data-stu-id="6f94d-329">An example <Uri> might be ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***.</span></span> <span data-ttu-id="6f94d-330">Ett exempel <FileInfo> kanske ***”C:\path\to\upload.vhd”***.</span><span class="sxs-lookup"><span data-stu-id="6f94d-330">An example <FileInfo> might be ***"C:\path\to\upload.vhd"***.</span></span>

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a><span data-ttu-id="6f94d-331">Alternativ 2: Använder AzCopy för att överföra VHD-filen</span><span class="sxs-lookup"><span data-stu-id="6f94d-331">Option 2: Using AzCopy to upload the .vhd file</span></span>
<span data-ttu-id="6f94d-332">Med AzCopy kan överföra du enkelt den virtuella Hårddisken via Internet.</span><span class="sxs-lookup"><span data-stu-id="6f94d-332">Using AzCopy, you can easily upload the VHD over the Internet.</span></span> <span data-ttu-id="6f94d-333">Detta kan ta tid beroende på storleken på de virtuella hårddiskarna.</span><span class="sxs-lookup"><span data-stu-id="6f94d-333">Depending on the size of the VHDs, this may take time.</span></span> <span data-ttu-id="6f94d-334">Kom ihåg att kontrollera ingång-/ utgång lagringskontogränser när du använder det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="6f94d-334">Remember to check the storage account ingress/egress limits when using this option.</span></span> <span data-ttu-id="6f94d-335">Se [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="6f94d-335">See [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) for details.</span></span>

1. <span data-ttu-id="6f94d-336">Hämta och installera AzCopy härifrån: [senaste versionen av AzCopy](http://aka.ms/downloadazcopy)</span><span class="sxs-lookup"><span data-stu-id="6f94d-336">Download and install AzCopy from here: [Latest version of AzCopy](http://aka.ms/downloadazcopy)</span></span>
2. <span data-ttu-id="6f94d-337">Öppna Azure PowerShell och gå till mappen där AzCopy är installerat.</span><span class="sxs-lookup"><span data-stu-id="6f94d-337">Open Azure PowerShell and go to the folder where AzCopy is installed.</span></span>
3. <span data-ttu-id="6f94d-338">Använd följande kommando för att kopiera VHD-filen från ”källa” till ”mål”.</span><span class="sxs-lookup"><span data-stu-id="6f94d-338">Use the following command to copy the VHD file from "Source" to "Destination".</span></span>

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    <span data-ttu-id="6f94d-339">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6f94d-339">Example:</span></span>

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
    ```

    <span data-ttu-id="6f94d-340">Nedan följer beskrivningar av de parametrar som används i AzCopy-kommandot:</span><span class="sxs-lookup"><span data-stu-id="6f94d-340">Here are descriptions of the parameters used in the AzCopy command:</span></span>

   * <span data-ttu-id="6f94d-341">**/ Källa:  *&lt;källa&gt;:***  platsen för mappen eller lagring behållarens Webbadress som innehåller den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="6f94d-341">**/Source: *&lt;source&gt;:*** Location of the folder or storage container URL that contains the VHD.</span></span>
   * <span data-ttu-id="6f94d-342">**/ SourceKey:  *&lt;källa kontonyckel&gt;:***  lagringskontonyckel på lagringskontot för källa.</span><span class="sxs-lookup"><span data-stu-id="6f94d-342">**/SourceKey: *&lt;source-account-key&gt;:*** Storage account key of the source storage account.</span></span>
   * <span data-ttu-id="6f94d-343">**/ Dest:  *&lt;mål&gt;:***  lagring behållar-URL för att kopiera den virtuella Hårddisken till.</span><span class="sxs-lookup"><span data-stu-id="6f94d-343">**/Dest: *&lt;destination&gt;:*** Storage container URL to copy the VHD to.</span></span>
   * <span data-ttu-id="6f94d-344">**/ DestKey:  *&lt;dest kontonyckel&gt;:***  lagringskontonyckel på mål-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="6f94d-344">**/DestKey: *&lt;dest-account-key&gt;:*** Storage account key of the destination storage account.</span></span>
   * <span data-ttu-id="6f94d-345">**/ BlobType: sidan:** anger att målet är en sidblobb.</span><span class="sxs-lookup"><span data-stu-id="6f94d-345">**/BlobType: page:** Specifies that the destination is a page blob.</span></span>
   * <span data-ttu-id="6f94d-346">**/ Mönster:  *&lt;filnamn&gt;:***  ange filnamnet för den virtuella Hårddisken ska kopieras.</span><span class="sxs-lookup"><span data-stu-id="6f94d-346">**/Pattern: *&lt;file-name&gt;:*** Specify the file name of the VHD to copy.</span></span>

<span data-ttu-id="6f94d-347">Mer information om hur du använder AzCopy verktyget, se [överföra data med kommandoradsverktyget Azcopy](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="6f94d-347">For details on using AzCopy tool, see [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

##### <a name="other-options-for-uploading-a-vhd"></a><span data-ttu-id="6f94d-348">Andra alternativ för att överföra en virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="6f94d-348">Other options for uploading a VHD</span></span>
<span data-ttu-id="6f94d-349">Du kan också ladda upp en virtuell Hårddisk till ditt lagringskonto med någon av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="6f94d-349">You can also upload a VHD to your storage account using one of the following means:</span></span>

* [<span data-ttu-id="6f94d-350">Azure Storage kopiera Blob API</span><span class="sxs-lookup"><span data-stu-id="6f94d-350">Azure Storage Copy Blob API</span></span>](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [<span data-ttu-id="6f94d-351">Azure Storage Explorer överför Blobbar</span><span class="sxs-lookup"><span data-stu-id="6f94d-351">Azure Storage Explorer Uploading Blobs</span></span>](https://azurestorageexplorer.codeplex.com/)
* [<span data-ttu-id="6f94d-352">Storage Import/Export Service REST API-referens</span><span class="sxs-lookup"><span data-stu-id="6f94d-352">Storage Import/Export Service REST API Reference</span></span>](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> <span data-ttu-id="6f94d-353">Du rekommenderas att använda Import/Export Service om överföring tid är längre än 7 dagar.</span><span class="sxs-lookup"><span data-stu-id="6f94d-353">We recommend using Import/Export Service if estimated uploading time is longer than 7 days.</span></span> <span data-ttu-id="6f94d-354">Du kan använda [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) att uppskatta storlek och överför dataenheten tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="6f94d-354">You can use [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) to estimate the time from data size and transfer unit.</span></span>
>
> <span data-ttu-id="6f94d-355">Import/Export kan användas för att kopiera till ett standardlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="6f94d-355">Import/Export can be used to copy to a standard storage account.</span></span> <span data-ttu-id="6f94d-356">Du måste kopiera från standardlagring till premium storage-konto med ett verktyg som AzCopy.</span><span class="sxs-lookup"><span data-stu-id="6f94d-356">You will need to copy from standard storage to premium storage account using a tool like AzCopy.</span></span>
>
>

## <span data-ttu-id="6f94d-357"><a name="create-azure-virtual-machine-using-premium-storage"></a>Skapa virtuella Azure-datorer med Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="6f94d-357"><a name="create-azure-virtual-machine-using-premium-storage"></a>Create Azure VMs using Premium Storage</span></span>
<span data-ttu-id="6f94d-358">När den virtuella Hårddisken överförs eller kopieras till önskat lagringskonto, följer du instruktionerna i det här avsnittet för att registrera den virtuella Hårddisken som en operativsystemsavbildning eller OS-disken beroende på ditt scenario och sedan skapa en VM-instans från den.</span><span class="sxs-lookup"><span data-stu-id="6f94d-358">After the VHD is uploaded or copied to the desired storage account, follow the instructions in this section to register the VHD as an OS image, or OS disk depending on your scenario and then create a VM instance from it.</span></span> <span data-ttu-id="6f94d-359">Datadisken VHD kan kopplas till den virtuella datorn när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="6f94d-359">The data disk VHD can be attached to the VM once it is created.</span></span>
<span data-ttu-id="6f94d-360">Ett exempelskript för migrering finns i slutet av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6f94d-360">A sample migration script is provided at the end of this section.</span></span> <span data-ttu-id="6f94d-361">Det här enkla skriptet matchar inte alla scenarier.</span><span class="sxs-lookup"><span data-stu-id="6f94d-361">This simple script does not match all scenarios.</span></span> <span data-ttu-id="6f94d-362">Du kan behöva uppdatera skriptet att överensstämmer med din situation.</span><span class="sxs-lookup"><span data-stu-id="6f94d-362">You may need to update the script to match with your specific scenario.</span></span> <span data-ttu-id="6f94d-363">Mer information om det här skriptet gäller för ditt scenario finns under [ett exempelskript för migrering](#a-sample-migration-script).</span><span class="sxs-lookup"><span data-stu-id="6f94d-363">To see if this script applies to your scenario, see below [A Sample Migration Script](#a-sample-migration-script).</span></span>

### <a name="checklist"></a><span data-ttu-id="6f94d-364">Checklista</span><span class="sxs-lookup"><span data-stu-id="6f94d-364">Checklist</span></span>
1. <span data-ttu-id="6f94d-365">Vänta tills alla VHD-diskar kopiera har slutförts.</span><span class="sxs-lookup"><span data-stu-id="6f94d-365">Wait until all the VHD disks copying is complete.</span></span>
2. <span data-ttu-id="6f94d-366">Kontrollera att Premium-lagring är tillgänglig i den region du migrerar till.</span><span class="sxs-lookup"><span data-stu-id="6f94d-366">Make sure Premium Storage is available in the region you are migrating to.</span></span>
3. <span data-ttu-id="6f94d-367">Bestäm den nya VM-serien som du kommer att använda.</span><span class="sxs-lookup"><span data-stu-id="6f94d-367">Decide the new VM series you will be using.</span></span> <span data-ttu-id="6f94d-368">Det bör vara en Premium-lagring som är kompatibla och storlek bör vara beroende av tillgängligheten i region och baserat på dina behov.</span><span class="sxs-lookup"><span data-stu-id="6f94d-368">It should be a Premium Storage capable, and the size should be depending on the availability in the region and based on your needs.</span></span>
4. <span data-ttu-id="6f94d-369">Bestäm den exakta VM-storlek som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="6f94d-369">Decide the exact VM size you will use.</span></span> <span data-ttu-id="6f94d-370">VM-storlek måste vara tillräckligt stor för att stödja antalet datadiskar som du har.</span><span class="sxs-lookup"><span data-stu-id="6f94d-370">VM size needs to be large enough to support the number of data disks you have.</span></span> <span data-ttu-id="6f94d-371">T.ex.</span><span class="sxs-lookup"><span data-stu-id="6f94d-371">E.g.</span></span> <span data-ttu-id="6f94d-372">Om du har 4 datadiskar, måste den virtuella datorn har 2 eller flera kärnor.</span><span class="sxs-lookup"><span data-stu-id="6f94d-372">if you have 4 data disks, the VM must have 2 or more cores.</span></span> <span data-ttu-id="6f94d-373">Du kan också processorkraft, minne och nätverksbandbredd måste.</span><span class="sxs-lookup"><span data-stu-id="6f94d-373">Also, consider processing power, memory and network bandwidth needs.</span></span>
5. <span data-ttu-id="6f94d-374">Skapa ett Premium Storage-konto i mål-region.</span><span class="sxs-lookup"><span data-stu-id="6f94d-374">Create a Premium Storage account in the target region.</span></span> <span data-ttu-id="6f94d-375">Detta är det konto som ska användas för den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="6f94d-375">This is the account you will use for the new VM.</span></span>
6. <span data-ttu-id="6f94d-376">Har den aktuella informationen för VM praktisk, inklusive en lista över diskar och motsvarande VHD-blobbar.</span><span class="sxs-lookup"><span data-stu-id="6f94d-376">Have the current VM details handy, including the list of disks and corresponding VHD blobs.</span></span>

<span data-ttu-id="6f94d-377">Förbered ditt program för driftstopp.</span><span class="sxs-lookup"><span data-stu-id="6f94d-377">Prepare your application for downtime.</span></span> <span data-ttu-id="6f94d-378">För att göra en ren migrering, måste du stoppa all bearbetning i det aktuella systemet.</span><span class="sxs-lookup"><span data-stu-id="6f94d-378">To do a clean migration, you have to stop all the processing in the current system.</span></span> <span data-ttu-id="6f94d-379">Först då kan du få till konsekvent tillstånd som du kan migrera till den nya plattformen.</span><span class="sxs-lookup"><span data-stu-id="6f94d-379">Only then you can get it to consistent state which you can migrate to the new platform.</span></span> <span data-ttu-id="6f94d-380">Varaktighet för stilleståndstid beror på mängden data på diskarna att migrera.</span><span class="sxs-lookup"><span data-stu-id="6f94d-380">Downtime duration will depend on the amount of data in the disks to migrate.</span></span>

> [!NOTE]
> <span data-ttu-id="6f94d-381">Om du skapar en Azure Resource Manager-VM från en särskild VHD-Disk, se [mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) för distribution av Resource Manager VM med hjälp av den befintliga disken.</span><span class="sxs-lookup"><span data-stu-id="6f94d-381">If you are creating an Azure Resource Manager VM from a specialized VHD Disk, please refer to [this template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) for deploying Resource Manager VM using existing disk.</span></span>
>
>

### <a name="register-your-vhd"></a><span data-ttu-id="6f94d-382">Registrera den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="6f94d-382">Register your VHD</span></span>
<span data-ttu-id="6f94d-383">Skapa en virtuell dator från OS VHD eller ansluta en datadisk till en ny virtuell dator, måste du först registrera dem.</span><span class="sxs-lookup"><span data-stu-id="6f94d-383">To create a VM from OS VHD or to attach a data disk to a new VM, you must first register them.</span></span> <span data-ttu-id="6f94d-384">Följ instruktionerna nedan scenario för den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="6f94d-384">Follow steps below depending on your VHD's scenario.</span></span>

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a><span data-ttu-id="6f94d-385">Generaliserad operativsystemet VHD att skapa flera Virtuella Azure-instanser</span><span class="sxs-lookup"><span data-stu-id="6f94d-385">Generalized Operating System VHD to create multiple Azure VM instances</span></span>
<span data-ttu-id="6f94d-386">När generaliserad virtuell Hårddisk för OS-avbildningen har överförts till lagringskontot, registrera den som en **Azure VM-avbildning** så att du kan skapa en eller flera VM-instanser av den.</span><span class="sxs-lookup"><span data-stu-id="6f94d-386">After generalized OS image VHD is uploaded to the storage account, register it as an **Azure VM Image** so that you can create one or more VM instances from it.</span></span> <span data-ttu-id="6f94d-387">Använd följande PowerShell-cmdlets för att registrera den virtuella Hårddisken i en Azure VM OS-bild.</span><span class="sxs-lookup"><span data-stu-id="6f94d-387">Use the following PowerShell cmdlets to register your VHD as an Azure VM OS image.</span></span> <span data-ttu-id="6f94d-388">Ange fullständig behållarens Webbadress där VHD kopierades till.</span><span class="sxs-lookup"><span data-stu-id="6f94d-388">Provide the complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

<span data-ttu-id="6f94d-389">Kopiera och spara namnet på den här nya Azure VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="6f94d-389">Copy and save the name of this new Azure VM Image.</span></span> <span data-ttu-id="6f94d-390">I exemplet ovan är det *OSImageName*.</span><span class="sxs-lookup"><span data-stu-id="6f94d-390">In the example above, it is *OSImageName*.</span></span>

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a><span data-ttu-id="6f94d-391">Unik operativsystemet VHD att skapa en instans av Azure VM</span><span class="sxs-lookup"><span data-stu-id="6f94d-391">Unique Operating System VHD to create a single Azure VM instance</span></span>
<span data-ttu-id="6f94d-392">När den unika OS virtuella Hårddisken har överförts till lagringskontot, registrera den som en **Azure OS-disken** så att du kan skapa en VM-instans av den.</span><span class="sxs-lookup"><span data-stu-id="6f94d-392">After the unique OS VHD is uploaded to the storage account, register it as an **Azure OS Disk** so that you can create a VM instance from it.</span></span> <span data-ttu-id="6f94d-393">Använd dessa PowerShell-cmdlets för att registrera den virtuella Hårddisken som en Azure OS-Disk.</span><span class="sxs-lookup"><span data-stu-id="6f94d-393">Use these PowerShell cmdlets to register your VHD as an Azure OS Disk.</span></span> <span data-ttu-id="6f94d-394">Ange fullständig behållarens Webbadress där VHD kopierades till.</span><span class="sxs-lookup"><span data-stu-id="6f94d-394">Provide the complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

<span data-ttu-id="6f94d-395">Kopiera och spara namnet på den här nya Azure OS-disken.</span><span class="sxs-lookup"><span data-stu-id="6f94d-395">Copy and save the name of this new Azure OS Disk.</span></span> <span data-ttu-id="6f94d-396">I exemplet ovan är det *OSDisk*.</span><span class="sxs-lookup"><span data-stu-id="6f94d-396">In the example above, it is *OSDisk*.</span></span>

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a><span data-ttu-id="6f94d-397">Data disk VHD som ska kopplas till nya Virtuella Azure-instanser</span><span class="sxs-lookup"><span data-stu-id="6f94d-397">Data disk VHD to be attached to new Azure VM instance(s)</span></span>
<span data-ttu-id="6f94d-398">När datadisk VHD har överförts till storage-konto du registrera den som en Azure-datadisk så att den kan kopplas till din nya DS-serien, DSv2-serien eller GS-serien Azure VM-instans.</span><span class="sxs-lookup"><span data-stu-id="6f94d-398">After the data disk VHD is uploaded to storage account, register it as an Azure Data Disk so that it can be attached to your new DS Series, DSv2 series or GS Series Azure VM instance.</span></span>

<span data-ttu-id="6f94d-399">Använd följande PowerShell-cmdlets för att registrera den virtuella Hårddisken som en Azure-datadisk.</span><span class="sxs-lookup"><span data-stu-id="6f94d-399">Use these PowerShell cmdlets to register your VHD as an Azure Data Disk.</span></span> <span data-ttu-id="6f94d-400">Ange fullständig behållarens Webbadress där VHD kopierades till.</span><span class="sxs-lookup"><span data-stu-id="6f94d-400">Provide the complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

<span data-ttu-id="6f94d-401">Kopiera och spara namnet på den här nya Azure-datadisk.</span><span class="sxs-lookup"><span data-stu-id="6f94d-401">Copy and save the name of this new Azure Data Disk.</span></span> <span data-ttu-id="6f94d-402">I exemplet ovan är det *DataDisk*.</span><span class="sxs-lookup"><span data-stu-id="6f94d-402">In the example above, it is *DataDisk*.</span></span>

### <a name="create-a-premium-storage-capable-vm"></a><span data-ttu-id="6f94d-403">Skapa en Premium-lagring kan virtuell dator</span><span class="sxs-lookup"><span data-stu-id="6f94d-403">Create a Premium Storage capable VM</span></span>
<span data-ttu-id="6f94d-404">När OS-avbildningen eller OS-disken har registrerats, skapar en ny DS-serien, DSv2-serien eller GS-serien VM.</span><span class="sxs-lookup"><span data-stu-id="6f94d-404">Once the OS image or OS disk are registered, create a new DS-series, DSv2-series or GS-series VM.</span></span> <span data-ttu-id="6f94d-405">Du kommer att använda operativsystemavbildningen eller operativsystemet diskens namn som du har registrerat.</span><span class="sxs-lookup"><span data-stu-id="6f94d-405">You will be using the operating system image or operating system disk name that you registered.</span></span> <span data-ttu-id="6f94d-406">Välj typ av VM Premium lagringsnivå.</span><span class="sxs-lookup"><span data-stu-id="6f94d-406">Select the VM type from the Premium Storage tier.</span></span> <span data-ttu-id="6f94d-407">I exemplet nedan använder den *Standard_DS2* VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="6f94d-407">In example below, we are using the *Standard_DS2* VM size.</span></span>

> [!NOTE]
> <span data-ttu-id="6f94d-408">Uppdatera diskstorleken på att se till att den matchar din kapacitet och prestandakrav och tillgängliga Azure diskstorlekar.</span><span class="sxs-lookup"><span data-stu-id="6f94d-408">Update the disk size to make sure it matches your capacity and performance requirements and the available Azure disk sizes.</span></span>
>
>

<span data-ttu-id="6f94d-409">Följ steg för steg PowerShell-cmdlet nedan för att skapa den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="6f94d-409">Follow the step by step PowerShell cmdlets below to create the new VM.</span></span> <span data-ttu-id="6f94d-410">Innan du kan definiera de gemensamma parametrarna:</span><span class="sxs-lookup"><span data-stu-id="6f94d-410">First, set the common parameters:</span></span>

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

<span data-ttu-id="6f94d-411">Först skapa en molnbaserad tjänst där du ska vara värd för din nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6f94d-411">First, create a cloud service in which you will be hosting your new VMs.</span></span>

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

<span data-ttu-id="6f94d-412">Skapa sedan den Virtuella Azure-instansen från OS-avbildningen eller OS-Disk som du har registrerat beroende på ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="6f94d-412">Next, depending on your scenario, create the Azure VM instance from either the OS Image or OS Disk that you registered.</span></span>

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a><span data-ttu-id="6f94d-413">Generaliserad operativsystemet VHD att skapa flera Virtuella Azure-instanser</span><span class="sxs-lookup"><span data-stu-id="6f94d-413">Generalized Operating System VHD to create multiple Azure VM instances</span></span>
<span data-ttu-id="6f94d-414">Skapa en eller flera nya DS-serien Azure VM-instanser med hjälp av den **Azure OS-avbildningen** som har registrerats.</span><span class="sxs-lookup"><span data-stu-id="6f94d-414">Create the one or more new DS Series Azure VM instances using the **Azure OS Image** that you registered.</span></span> <span data-ttu-id="6f94d-415">Ange namnet OS-avbildningen i VM-konfiguration när du skapar ny virtuell dator som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="6f94d-415">Specify this OS Image name in the VM configuration when creating new VM as shown below.</span></span>

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a><span data-ttu-id="6f94d-416">Unik operativsystemet VHD att skapa en instans av Azure VM</span><span class="sxs-lookup"><span data-stu-id="6f94d-416">Unique Operating System VHD to create a single Azure VM instance</span></span>
<span data-ttu-id="6f94d-417">Skapa en ny DS serien Azure VM instans med hjälp av **Azure OS-disken** som har registrerats.</span><span class="sxs-lookup"><span data-stu-id="6f94d-417">Create a new DS series Azure VM instance using the **Azure OS Disk** that you registered.</span></span> <span data-ttu-id="6f94d-418">Ange den här OS-disknamnet VM-konfiguration när du skapar den nya virtuella datorn som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="6f94d-418">Specify this OS Disk name in the VM configuration when creating the new VM as shown below.</span></span>

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

<span data-ttu-id="6f94d-419">Ange andra Virtuella Azure-information, till exempel en molntjänst, region, storage-konto, tillgänglighetsuppsättning och cachelagringsprincipen.</span><span class="sxs-lookup"><span data-stu-id="6f94d-419">Specify other Azure VM information, such as a cloud service, region, storage account, availability set, and caching policy.</span></span> <span data-ttu-id="6f94d-420">Observera att VM-instans måste vara samordnad med associerade operativsystem eller hårddiskar, så det valda moln service, region och lagring kontot måste vara på samma plats som de underliggande virtuella hårddiskarna för dessa diskar.</span><span class="sxs-lookup"><span data-stu-id="6f94d-420">Note that the VM instance must be co-located with associated operating system or data disks, so the selected cloud service, region and storage account must all be in the same location as the underlying VHDs of those disks.</span></span>

### <a name="attach-data-disk"></a><span data-ttu-id="6f94d-421">Anslut en datadisk</span><span class="sxs-lookup"><span data-stu-id="6f94d-421">Attach data disk</span></span>
<span data-ttu-id="6f94d-422">Slutligen, om du har registrerat datadisk virtuella hårddiskar, bifoga dem i den nya Premium-lagring kan Azure VM.</span><span class="sxs-lookup"><span data-stu-id="6f94d-422">Lastly, if you have registered data disk VHDs, attach them to the new Premium Storage capable Azure VM.</span></span>

<span data-ttu-id="6f94d-423">Använda följande PowerShell-cmdlet för att ansluta en datadisk till den nya virtuella datorn och ange principen för cachelagring.</span><span class="sxs-lookup"><span data-stu-id="6f94d-423">Use following PowerShell cmdlet to attach data disk to the new VM and specify the caching policy.</span></span> <span data-ttu-id="6f94d-424">I exemplet nedan anges cachelagringsprincipen till *ReadOnly*.</span><span class="sxs-lookup"><span data-stu-id="6f94d-424">In example below the caching policy is set to *ReadOnly*.</span></span>

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> <span data-ttu-id="6f94d-425">Det kan finnas ytterligare steg behövs för ditt program som inte omfattas av den här guiden.</span><span class="sxs-lookup"><span data-stu-id="6f94d-425">There may be additional steps necessary to support your application that is not be covered by this guide.</span></span>
>
>

### <a name="checking-and-plan-backup"></a><span data-ttu-id="6f94d-426">Kontrollera och planera säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="6f94d-426">Checking and plan backup</span></span>
<span data-ttu-id="6f94d-427">När den nya virtuella datorn är igång kan komma åt den med hjälp av samma inloggnings-id och lösenord är som den ursprungliga virtuella datorn och kontrollera att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="6f94d-427">Once the new VM is up and running, access it using the same login id and password is as the original VM, and verify that everything is working as expected.</span></span> <span data-ttu-id="6f94d-428">Alla inställningar, inklusive stripe-volymer kan finnas i den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="6f94d-428">All the settings, including the striped volumes, would be present in the new VM.</span></span>

<span data-ttu-id="6f94d-429">Det sista steget är att planera säkerhetskopiering och underhållsschemat för den nya virtuella datorn baserat på programmets behov.</span><span class="sxs-lookup"><span data-stu-id="6f94d-429">The last step is to plan backup and maintenance schedule for the new VM based on the application's needs.</span></span>

### <span data-ttu-id="6f94d-430"><a name="a-sample-migration-script"></a>Ett exempelskript för migrering</span><span class="sxs-lookup"><span data-stu-id="6f94d-430"><a name="a-sample-migration-script"></a>A sample migration script</span></span>
<span data-ttu-id="6f94d-431">Om du har flera virtuella datorer för att migrera vara automation via PowerShell-skript användbara.</span><span class="sxs-lookup"><span data-stu-id="6f94d-431">If you have multiple VMs to migrate, automation through PowerShell scripts will be helpful.</span></span> <span data-ttu-id="6f94d-432">Följande är ett exempelskript som automatiserar migreringen av en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="6f94d-432">Following is a sample script that automates the migration of a VM.</span></span> <span data-ttu-id="6f94d-433">Observera som skriptet nedan är bara ett exempel och det finns några antaganden om de aktuella Virtuella diskarna.</span><span class="sxs-lookup"><span data-stu-id="6f94d-433">Note that below script is only an example, and there are few assumptions made about the current VM disks.</span></span> <span data-ttu-id="6f94d-434">Du kan behöva uppdatera skriptet att överensstämmer med din situation.</span><span class="sxs-lookup"><span data-stu-id="6f94d-434">You may need to update the script to match with your specific scenario.</span></span>

<span data-ttu-id="6f94d-435">Antaganden är:</span><span class="sxs-lookup"><span data-stu-id="6f94d-435">The assumptions are:</span></span>

* <span data-ttu-id="6f94d-436">Du skapar klassiska virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="6f94d-436">You are creating classic Azure VMs.</span></span>
* <span data-ttu-id="6f94d-437">Källan OS diskar och källa Datadiskar finns i samma lagringskonto och samma behållare.</span><span class="sxs-lookup"><span data-stu-id="6f94d-437">Your source OS Disks and source Data Disks are in same storage account and same container.</span></span> <span data-ttu-id="6f94d-438">Om OS-diskar och Datadiskar som inte är på samma plats kan använda du AzCopy eller Azure PowerShell för att kopiera virtuella hårddiskar över storage-konton och behållare.</span><span class="sxs-lookup"><span data-stu-id="6f94d-438">If your OS Disks and Data Disks are not in the same place, you can use AzCopy or Azure PowerShell to copy VHDs over storage accounts and containers.</span></span> <span data-ttu-id="6f94d-439">Referera till det föregående steget: [kopiera VHD med AzCopy eller PowerShell](#copy-vhd-with-azcopy-or-powershell).</span><span class="sxs-lookup"><span data-stu-id="6f94d-439">Refer to the previous step: [Copy VHD with AzCopy or PowerShell](#copy-vhd-with-azcopy-or-powershell).</span></span> <span data-ttu-id="6f94d-440">Redigera det här skriptet för att möta ditt scenario är ett annat alternativ, men vi rekommenderar att du använder AzCopy eller PowerShell eftersom det är enklare och snabbare.</span><span class="sxs-lookup"><span data-stu-id="6f94d-440">Editing this script to meet your scenario is another choice, but we recommend using AzCopy or PowerShell since it is easier and faster.</span></span>

<span data-ttu-id="6f94d-441">Automation-skript finns nedan.</span><span class="sxs-lookup"><span data-stu-id="6f94d-441">The automation script is provided below.</span></span> <span data-ttu-id="6f94d-442">Ersätt texten med din information och uppdatera skriptet att överensstämmer med din situation.</span><span class="sxs-lookup"><span data-stu-id="6f94d-442">Replace text with your information and update the script to match with your specific scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="6f94d-443">Med det befintliga skriptet bevaras inte nätverkskonfigurationen för ditt Virtuella källdatorn.</span><span class="sxs-lookup"><span data-stu-id="6f94d-443">Using the existing script does not preserve the network configuration of your source VM.</span></span> <span data-ttu-id="6f94d-444">Du behöver re-config nätverksinställningarna på de migrerade virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="6f94d-444">You will need to re-config the networking settings on your migrated VMs.</span></span>
>
>

```
    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you'd like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <span data-ttu-id="6f94d-445"><a name="optimization"></a>Optimering</span><span class="sxs-lookup"><span data-stu-id="6f94d-445"><a name="optimization"></a>Optimization</span></span>
<span data-ttu-id="6f94d-446">Din aktuella VM-konfiguration kan anpassas specifikt fungerar bra med standarddiskar.</span><span class="sxs-lookup"><span data-stu-id="6f94d-446">Your current VM configuration may be customized specifically to work well with Standard disks.</span></span> <span data-ttu-id="6f94d-447">Till exempel att öka prestandan genom att använda många diskar i en stripe-volym.</span><span class="sxs-lookup"><span data-stu-id="6f94d-447">For instance, to increase the performance by using many disks in a striped volume.</span></span> <span data-ttu-id="6f94d-448">Till exempel i stället för att använda 4 diskar separat på Premium-lagring, du kan optimera kostnaden genom att ha en enda disk.</span><span class="sxs-lookup"><span data-stu-id="6f94d-448">For example, instead of using 4 disks separately on Premium Storage, you may be able to optimize the cost by having a single disk.</span></span> <span data-ttu-id="6f94d-449">Optimeringar som den här behöver hanteras på ett fall till fall och kräver anpassade åtgärder efter migreringen.</span><span class="sxs-lookup"><span data-stu-id="6f94d-449">Optimizations like this need to be handled on a case by case basis and require custom steps after the migration.</span></span> <span data-ttu-id="6f94d-450">Observera också att den här processen och inte fungerar för databaser och program som beror på disklayouten definierat i inställningarna.</span><span class="sxs-lookup"><span data-stu-id="6f94d-450">Also, note that this process may not well work for databases and applications that depend on the disk layout defined in the setup.</span></span>

##### <a name="preparation"></a><span data-ttu-id="6f94d-451">Förberedelse</span><span class="sxs-lookup"><span data-stu-id="6f94d-451">Preparation</span></span>
1. <span data-ttu-id="6f94d-452">Slutföra migreringen enkla enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="6f94d-452">Complete the Simple Migration as described in the earlier section.</span></span> <span data-ttu-id="6f94d-453">Optimeringar kommer att utföras på den nya virtuella datorn efter migreringen.</span><span class="sxs-lookup"><span data-stu-id="6f94d-453">Optimizations will be performed on the new VM after the migration.</span></span>
2. <span data-ttu-id="6f94d-454">Ange nya storlekar för diskar som behövs för den optimerade konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="6f94d-454">Define the new disk sizes needed for the optimized configuration.</span></span>
3. <span data-ttu-id="6f94d-455">Fastställa mappningen av aktuella diskar/volymer till de nya disken specifikationerna.</span><span class="sxs-lookup"><span data-stu-id="6f94d-455">Determine mapping of the current disks/volumes to the new disk specifications.</span></span>

##### <a name="execution-steps"></a><span data-ttu-id="6f94d-456">Utförande</span><span class="sxs-lookup"><span data-stu-id="6f94d-456">Execution steps</span></span>
1. <span data-ttu-id="6f94d-457">Skapa nya diskar med rätt storlek på den virtuella datorn Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="6f94d-457">Create the new disks with the right sizes on the Premium Storage VM.</span></span>
2. <span data-ttu-id="6f94d-458">Logga in på den virtuella datorn och kopiera data från den aktuella volymen till den nya disken som mappar till volymen.</span><span class="sxs-lookup"><span data-stu-id="6f94d-458">Login to the VM and copy the data from the current volume to the new disk that maps to that volume.</span></span> <span data-ttu-id="6f94d-459">Gör detta för alla aktuella volymer som ska mappas till en ny disk.</span><span class="sxs-lookup"><span data-stu-id="6f94d-459">Do this for all the current volumes that need to map to a new disk.</span></span>
3. <span data-ttu-id="6f94d-460">Sedan ändra programinställningarna växla till nya diskar och koppla från de gamla volymerna.</span><span class="sxs-lookup"><span data-stu-id="6f94d-460">Next, change the application settings to switch to the new disks, and detach the old volumes.</span></span>

<span data-ttu-id="6f94d-461">För att finjustera programmet för bättre diskprestanda, se [optimera programprestanda](storage-premium-storage-performance.md#optimizing-application-performance).</span><span class="sxs-lookup"><span data-stu-id="6f94d-461">For tuning the application for better disk performance, please refer to [Optimizing Application Performance](storage-premium-storage-performance.md#optimizing-application-performance).</span></span>

### <a name="application-migrations"></a><span data-ttu-id="6f94d-462">Migrering av programmet</span><span class="sxs-lookup"><span data-stu-id="6f94d-462">Application migrations</span></span>
<span data-ttu-id="6f94d-463">Databaser och andra komplexa program kan kräva särskilda åtgärder som definierats av programmet providern för migreringen.</span><span class="sxs-lookup"><span data-stu-id="6f94d-463">Databases and other complex applications may require special steps as defined by the application provider for the migration.</span></span> <span data-ttu-id="6f94d-464">Se programdokumentationen för respektive.</span><span class="sxs-lookup"><span data-stu-id="6f94d-464">Please refer to respective application documentation.</span></span> <span data-ttu-id="6f94d-465">T.ex.</span><span class="sxs-lookup"><span data-stu-id="6f94d-465">E.g.</span></span> <span data-ttu-id="6f94d-466">vanligtvis databaser kan migreras med säkerhetskopiering och återställning.</span><span class="sxs-lookup"><span data-stu-id="6f94d-466">typically databases can be migrated through backup and restore.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6f94d-467">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6f94d-467">Next steps</span></span>
<span data-ttu-id="6f94d-468">Se följande resurser för specifika scenarier för att migrera virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="6f94d-468">See the following resources for specific scenarios for migrating virtual machines:</span></span>

* [<span data-ttu-id="6f94d-469">Migrera Azure virtuella datorer mellan Storage-konton</span><span class="sxs-lookup"><span data-stu-id="6f94d-469">Migrate Azure Virtual Machines between Storage Accounts</span></span>](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [<span data-ttu-id="6f94d-470">Skapa och ladda upp en Windows Server VHD till Azure.</span><span class="sxs-lookup"><span data-stu-id="6f94d-470">Create and upload a Windows Server VHD to Azure.</span></span>](../../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="6f94d-471">Skapa och ladda upp en virtuell hårddisk som innehåller Linux-operativsystem</span><span class="sxs-lookup"><span data-stu-id="6f94d-471">Creating and Uploading a Virtual Hard Disk that Contains the Linux Operating System</span></span>](../../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="6f94d-472">Migrering av virtuella datorer från Amazon AWS till Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="6f94d-472">Migrating Virtual Machines from Amazon AWS to Microsoft Azure</span></span>](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

<span data-ttu-id="6f94d-473">Se även följande resurser för att du lär dig mer om Azure Storage- och virtuella datorer i Azure:</span><span class="sxs-lookup"><span data-stu-id="6f94d-473">Also, see the following resources to learn more about Azure Storage and Azure Virtual Machines:</span></span>

* [<span data-ttu-id="6f94d-474">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="6f94d-474">Azure Storage</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="6f94d-475">Virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="6f94d-475">Azure Virtual Machines</span></span>](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [<span data-ttu-id="6f94d-476">Premium Storage: Lagring med höga prestanda för Azure Virtual Machines-arbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="6f94d-476">Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads</span></span>](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
