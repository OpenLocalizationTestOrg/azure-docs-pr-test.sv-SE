---
title: aaaMigrating VMs tooAzure Premium-lagring | Microsoft Docs
description: "Migrera dina befintliga virtuella datorer tooAzure Premium-lagring. Premium-lagring ger stöd för I/O-intensiva arbetsbelastningar som körs på Azure Virtual Machines diskar med hög prestanda, låg latens."
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
ms.openlocfilehash: cd812bdbe39f43fe053a998d96788045d5a43258
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-tooazure-premium-storage-unmanaged-disks"></a><span data-ttu-id="1d718-104">Migrera tooAzure Premium-lagring (ohanterade diskar)</span><span class="sxs-lookup"><span data-stu-id="1d718-104">Migrating tooAzure Premium Storage (Unmanaged Disks)</span></span>

> [!NOTE]
> <span data-ttu-id="1d718-105">Den här artikeln beskrivs hur toomigrate en virtuell dator som använder ohanterade standarddiskar tooa virtuell dator som använder ohanterad premiumdiskar.</span><span class="sxs-lookup"><span data-stu-id="1d718-105">This article discusses how toomigrate a VM that uses unmanaged standard disks tooa VM that uses unmanaged premium disks.</span></span> <span data-ttu-id="1d718-106">Vi rekommenderar att du använder Azure hanterade diskar för nya virtuella datorer och att du konverterar tidigare ohanterade diskar toomanaged diskarna.</span><span class="sxs-lookup"><span data-stu-id="1d718-106">We recommend that you use Azure Managed Disks for new VMs, and that you convert your previous unmanaged disks toomanaged disks.</span></span> <span data-ttu-id="1d718-107">Hanterade diskar-referensen hello underliggande storage-konton så du behöver.</span><span class="sxs-lookup"><span data-stu-id="1d718-107">Managed Disks handle hello underlying storage accounts so you don't have to.</span></span> <span data-ttu-id="1d718-108">Mer information, se vår [översikt för hanterade diskar](storage-managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1d718-108">For more information, please see our [Managed Disks Overview](storage-managed-disks-overview.md).</span></span>
>

<span data-ttu-id="1d718-109">Azure Premium Storage ger stöd för virtuella datorer som körs I/O-intensiva arbetsbelastningar diskar med hög prestanda, låg latens.</span><span class="sxs-lookup"><span data-stu-id="1d718-109">Azure Premium Storage delivers high-performance, low-latency disk support for virtual machines running I/O-intensive workloads.</span></span> <span data-ttu-id="1d718-110">Du kan dra nytta av hello hastighet och prestanda för dessa diskar genom att migrera programmets Virtuella diskar tooAzure Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="1d718-110">You can take advantage of hello speed and performance of these disks by migrating your application's VM disks tooAzure Premium Storage.</span></span>

<span data-ttu-id="1d718-111">hello syftet med den här guiden är toohelp nya användare på Azure Premium Storage bättre förbereda toomake en smidig övergång från deras aktuella system tooPremium lagring.</span><span class="sxs-lookup"><span data-stu-id="1d718-111">hello purpose of this guide is toohelp new users of Azure Premium Storage better prepare toomake a smooth transition from their current system tooPremium Storage.</span></span> <span data-ttu-id="1d718-112">hello guiden löser tre hello viktiga komponenter i den här processen:</span><span class="sxs-lookup"><span data-stu-id="1d718-112">hello guide addresses three of hello key components in this process:</span></span>

* [<span data-ttu-id="1d718-113">Planera för hello migrering tooPremium lagring</span><span class="sxs-lookup"><span data-stu-id="1d718-113">Plan for hello Migration tooPremium Storage</span></span>](#plan-the-migration-to-premium-storage)
* [<span data-ttu-id="1d718-114">Förbereda och kopiera virtuella hårddiskar (VHD) tooPremium lagring</span><span class="sxs-lookup"><span data-stu-id="1d718-114">Prepare and Copy Virtual Hard Disks (VHDs) tooPremium Storage</span></span>](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [<span data-ttu-id="1d718-115">Skapa Azure virtuell dator med Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="1d718-115">Create Azure Virtual Machine using Premium Storage</span></span>](#create-azure-virtual-machine-using-premium-storage)

<span data-ttu-id="1d718-116">Du kan migrera virtuella datorer från andra plattformar tooAzure Premium-lagring, eller så kan du migrera befintliga virtuella Azure-datorer från standardlagring tooPremium lagring.</span><span class="sxs-lookup"><span data-stu-id="1d718-116">You can either migrate VMs from other platforms tooAzure Premium Storage or migrate existing Azure VMs from Standard Storage tooPremium Storage.</span></span> <span data-ttu-id="1d718-117">Den här guiden beskriver steg för båda två scenarier.</span><span class="sxs-lookup"><span data-stu-id="1d718-117">This guide covers steps for both two scenarios.</span></span> <span data-ttu-id="1d718-118">Åtgärderna hello hello relevanta avsnitt beroende på ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="1d718-118">Follow hello steps specified in hello relevant section depending on your scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="1d718-119">Du hittar en översikt över funktioner och priser för Premium-lagring i Premium Storage: [högpresterande lagring för arbetsbelastningar på virtuella Azure](storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="1d718-119">You can find a feature overview and pricing of Premium Storage in Premium Storage: [High-Performance Storage for Azure Virtual Machine Workloads](storage-premium-storage.md).</span></span> <span data-ttu-id="1d718-120">Vi rekommenderar att du migrerar alla virtuella diskar som kräver hög IOPS tooAzure Premium-lagring för hello bästa prestanda för ditt program.</span><span class="sxs-lookup"><span data-stu-id="1d718-120">We recommend migrating any virtual machine disk requiring high IOPS tooAzure Premium Storage for hello best performance for your application.</span></span> <span data-ttu-id="1d718-121">Om disken inte kräver hög IOPS, kan du begränsa kostnader genom att upprätthålla i standardlagring som lagrar data för virtuell disk på hårddiskar (HDD) i stället för SSD-enheter.</span><span class="sxs-lookup"><span data-stu-id="1d718-121">If your disk does not require high IOPS, you can limit costs by maintaining it in Standard Storage, which stores virtual machine disk data on Hard Disk Drives (HDDs) instead of SSDs.</span></span>
>

<span data-ttu-id="1d718-122">Slutför hello migreringsprocessen i sin helhet kan kräva ytterligare åtgärder både före och efter hello stegen i den här guiden.</span><span class="sxs-lookup"><span data-stu-id="1d718-122">Completing hello migration process in its entirety may require additional actions both before and after hello steps provided in this guide.</span></span> <span data-ttu-id="1d718-123">Exempel inkluderar konfigurering av virtuella nätverk eller slutpunkter eller ändrar koden i hello själva programmet som kan kräva vissa avbrott i ditt program.</span><span class="sxs-lookup"><span data-stu-id="1d718-123">Examples include configuring virtual networks or endpoints or making code changes within hello application itself which may require some downtime in your application.</span></span> <span data-ttu-id="1d718-124">Dessa åtgärder är unika tooeach program och du bör utföra dem tillsammans med hello stegen i den här guiden toomake hello fullständig övergång tooPremium lagring som sömlös som möjligt.</span><span class="sxs-lookup"><span data-stu-id="1d718-124">These actions are unique tooeach application, and you should complete them along with hello steps provided in this guide toomake hello full transition tooPremium Storage as seamless as possible.</span></span>

## <span data-ttu-id="1d718-125"><a name="plan-the-migration-to-premium-storage"></a>Planera för hello migrering tooPremium lagring</span><span class="sxs-lookup"><span data-stu-id="1d718-125"><a name="plan-the-migration-to-premium-storage"></a>Plan for hello migration tooPremium Storage</span></span>
<span data-ttu-id="1d718-126">Det här avsnittet säkerställer att du är klar toofollow hello migreringssteg i den här artikeln och hjälper dig att toomake hello bästa beslut på VM- och diskresurser typer.</span><span class="sxs-lookup"><span data-stu-id="1d718-126">This section ensures that you are ready toofollow hello migration steps in this article, and helps you toomake hello best decision on VM and Disk types.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="1d718-127">Krav</span><span class="sxs-lookup"><span data-stu-id="1d718-127">Prerequisites</span></span>
* <span data-ttu-id="1d718-128">Du behöver en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1d718-128">You will need an Azure subscription.</span></span> <span data-ttu-id="1d718-129">Om du inte har någon, kan du skapa en månad [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/) prenumeration eller besök [priser för Azure](https://azure.microsoft.com/pricing/) fler alternativ.</span><span class="sxs-lookup"><span data-stu-id="1d718-129">If you don't have one, you can create a one-month [free trial](https://azure.microsoft.com/pricing/free-trial/) subscription or visit [Azure Pricing](https://azure.microsoft.com/pricing/) for more options.</span></span>
* <span data-ttu-id="1d718-130">tooexecute PowerShell-cmdlets måste hello Microsoft Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="1d718-130">tooexecute PowerShell cmdlets, you will need hello Microsoft Azure PowerShell module.</span></span> <span data-ttu-id="1d718-131">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för hello installera installationsplatser och installationsanvisningar.</span><span class="sxs-lookup"><span data-stu-id="1d718-131">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for hello install point and installation instructions.</span></span>
* <span data-ttu-id="1d718-132">När du planerar toouse Azure virtuella datorer som körs på Premium-lagring måste toouse hello Premium-lagring kan virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1d718-132">When you plan toouse Azure VMs running on Premium Storage, you need toouse hello Premium Storage capable VMs.</span></span> <span data-ttu-id="1d718-133">Du kan använda Standard- och Premium-lagring diskar med Premium-lagring kan virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1d718-133">You can use both Standard and Premium Storage disks with Premium Storage capable VMs.</span></span> <span data-ttu-id="1d718-134">Premium lagringsdiskar kan användas med flera VM-typer i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="1d718-134">Premium storage disks will be available with more VM types in hello future.</span></span> <span data-ttu-id="1d718-135">Läs mer på alla tillgängliga Virtuella Azure-disktyper och storlekar, [storlekar för virtuella datorer](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) och [storlekar för molntjänster](../cloud-services/cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="1d718-135">For more information on all available Azure VM disk types and sizes, see [Sizes for virtual machines](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md).</span></span>

### <a name="considerations"></a><span data-ttu-id="1d718-136">Överväganden</span><span class="sxs-lookup"><span data-stu-id="1d718-136">Considerations</span></span>
<span data-ttu-id="1d718-137">En Azure VM stöder bifoga flera Premium-lagring diskar så att dina program kan ha upp too256 TB lagringsutrymme per virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1d718-137">An Azure VM supports attaching several Premium Storage disks so that your applications can have up too256 TB of storage per VM.</span></span> <span data-ttu-id="1d718-138">Med Premium-lagring kan dina program uppnå (i/o-åtgärder per sekund) för 80 000 IOPS per virtuell dator och 2000 MB per andra diskgenomflödet per virtuell dator med mycket låg latens för läsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="1d718-138">With Premium Storage, your applications can achieve 80,000 IOPS (input/output operations per second) per VM and 2000 MB per second disk throughput per VM with extremely low latencies for read operations.</span></span> <span data-ttu-id="1d718-139">Har du alternativ på en mängd olika virtuella datorer och diskar.</span><span class="sxs-lookup"><span data-stu-id="1d718-139">You have options in a variety of VMs and Disks.</span></span> <span data-ttu-id="1d718-140">Det här avsnittet är toohelp toofind ett alternativ som bäst passar din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="1d718-140">This section is toohelp you toofind an option that best suits your workload.</span></span>

#### <a name="vm-sizes"></a><span data-ttu-id="1d718-141">VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="1d718-141">VM sizes</span></span>
<span data-ttu-id="1d718-142">hello Azure VM storlek specifikationer listas i [storlekar för virtuella datorer](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1d718-142">hello Azure VM size specifications are listed in [Sizes for virtual machines](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="1d718-143">Granska hello prestandaegenskaper för virtuella datorer som fungerar med Premium-lagring och välj hello lämpligaste VM-storlek som bäst passar din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="1d718-143">Review hello performance characteristics of virtual machines that work with Premium Storage and choose hello most appropriate VM size that best suits your workload.</span></span> <span data-ttu-id="1d718-144">Kontrollera att det finns tillräckligt med bandbredd på VM toodrive hello disk trafiken.</span><span class="sxs-lookup"><span data-stu-id="1d718-144">Make sure that there is sufficient bandwidth available on your VM toodrive hello disk traffic.</span></span>

#### <a name="disk-sizes"></a><span data-ttu-id="1d718-145">Diskstorlekar</span><span class="sxs-lookup"><span data-stu-id="1d718-145">Disk sizes</span></span>
<span data-ttu-id="1d718-146">Det finns fem typer av diskar som kan användas med den virtuella datorn och var och en har särskilda IOPs och genomströmning gränser.</span><span class="sxs-lookup"><span data-stu-id="1d718-146">There are five types of disks that can be used with your VM and each has specific IOPs and throughput limits.</span></span> <span data-ttu-id="1d718-147">Ta hänsyn till dessa gränser när välja hello disktyp för den virtuella datorn baserat på hello behoven för ditt program vad gäller kapacitet, prestanda, skalbarhet och belastning läses in.</span><span class="sxs-lookup"><span data-stu-id="1d718-147">Take into consideration these limits when choosing hello disk type for your VM based on hello needs of your application in terms of capacity, performance, scalability, and peak loads.</span></span>

| <span data-ttu-id="1d718-148">Premium diskar typ</span><span class="sxs-lookup"><span data-stu-id="1d718-148">Premium Disks Type</span></span>  | <span data-ttu-id="1d718-149">P10</span><span class="sxs-lookup"><span data-stu-id="1d718-149">P10</span></span>   | <span data-ttu-id="1d718-150">P20</span><span class="sxs-lookup"><span data-stu-id="1d718-150">P20</span></span>   | <span data-ttu-id="1d718-151">P30</span><span class="sxs-lookup"><span data-stu-id="1d718-151">P30</span></span>            | <span data-ttu-id="1d718-152">P40</span><span class="sxs-lookup"><span data-stu-id="1d718-152">P40</span></span>            | <span data-ttu-id="1d718-153">P50</span><span class="sxs-lookup"><span data-stu-id="1d718-153">P50</span></span>            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| <span data-ttu-id="1d718-154">Diskstorlek</span><span class="sxs-lookup"><span data-stu-id="1d718-154">Disk size</span></span>           | <span data-ttu-id="1d718-155">128 GB</span><span class="sxs-lookup"><span data-stu-id="1d718-155">128 GB</span></span>| <span data-ttu-id="1d718-156">512 GB</span><span class="sxs-lookup"><span data-stu-id="1d718-156">512 GB</span></span>| <span data-ttu-id="1d718-157">1 024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="1d718-157">1024 GB (1 TB)</span></span> | <span data-ttu-id="1d718-158">2 048 GB (2 TB)</span><span class="sxs-lookup"><span data-stu-id="1d718-158">2048 GB (2 TB)</span></span> | <span data-ttu-id="1d718-159">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="1d718-159">4095 GB (4 TB)</span></span> | 
| <span data-ttu-id="1d718-160">IOPS per disk</span><span class="sxs-lookup"><span data-stu-id="1d718-160">IOPS per disk</span></span>       | <span data-ttu-id="1d718-161">500</span><span class="sxs-lookup"><span data-stu-id="1d718-161">500</span></span>   | <span data-ttu-id="1d718-162">2 300</span><span class="sxs-lookup"><span data-stu-id="1d718-162">2300</span></span>  | <span data-ttu-id="1d718-163">5000</span><span class="sxs-lookup"><span data-stu-id="1d718-163">5000</span></span>           | <span data-ttu-id="1d718-164">7500</span><span class="sxs-lookup"><span data-stu-id="1d718-164">7500</span></span>           | <span data-ttu-id="1d718-165">7500</span><span class="sxs-lookup"><span data-stu-id="1d718-165">7500</span></span>           | 
| <span data-ttu-id="1d718-166">Dataflöde per disk</span><span class="sxs-lookup"><span data-stu-id="1d718-166">Throughput per disk</span></span> | <span data-ttu-id="1d718-167">100 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="1d718-167">100 MB per second</span></span> | <span data-ttu-id="1d718-168">150 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="1d718-168">150 MB per second</span></span> | <span data-ttu-id="1d718-169">200 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="1d718-169">200 MB per second</span></span> | <span data-ttu-id="1d718-170">250 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="1d718-170">250 MB per second</span></span> | <span data-ttu-id="1d718-171">250 MB per sekund</span><span class="sxs-lookup"><span data-stu-id="1d718-171">250 MB per second</span></span> |

<span data-ttu-id="1d718-172">Beroende på din arbetsbelastning avgör du om ytterligare hårddiskar krävs för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1d718-172">Depending on your workload, determine if additional data disks are necessary for your VM.</span></span> <span data-ttu-id="1d718-173">Du kan bifoga flera beständiga data diskar tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="1d718-173">You can attach several persistent data disks tooyour VM.</span></span> <span data-ttu-id="1d718-174">Om det behövs kan stripe du över hello diskar tooincrease hello kapacitet och prestanda för hello volym.</span><span class="sxs-lookup"><span data-stu-id="1d718-174">If needed, you can stripe across hello disks tooincrease hello capacity and performance of hello volume.</span></span> <span data-ttu-id="1d718-175">(Se vad som är Disk Striping [här](storage-premium-storage-performance.md#disk-striping).) Om du stripe-Premium-lagring datadiskar med hjälp av [lagringsutrymmen][4], bör du konfigurera den med en kolumn för varje disk som används.</span><span class="sxs-lookup"><span data-stu-id="1d718-175">(See what is Disk Striping [here](storage-premium-storage-performance.md#disk-striping).) If you stripe Premium Storage data disks using [Storage Spaces][4], you should configure it with one column for each disk that is used.</span></span> <span data-ttu-id="1d718-176">Annars vara hello övergripande prestanda för hello stripe-volym lägre än väntat på grund av toouneven fördelning av trafik över hello diskar.</span><span class="sxs-lookup"><span data-stu-id="1d718-176">Otherwise, hello overall performance of hello striped volume may be lower than expected due toouneven distribution of traffic across hello disks.</span></span> <span data-ttu-id="1d718-177">För virtuella Linux-datorer kan du använda hello *mdadm* verktyget tooachieve hello samma.</span><span class="sxs-lookup"><span data-stu-id="1d718-177">For Linux VMs you can use hello *mdadm* utility tooachieve hello same.</span></span> <span data-ttu-id="1d718-178">Se artikeln [konfigurera programvara RAID på Linux](../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mer information.</span><span class="sxs-lookup"><span data-stu-id="1d718-178">See article [Configure Software RAID on Linux](../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for details.</span></span>

#### <a name="storage-account-scalability-targets"></a><span data-ttu-id="1d718-179">Skalbarhetsmål för lagringskontot</span><span class="sxs-lookup"><span data-stu-id="1d718-179">Storage account scalability targets</span></span>
<span data-ttu-id="1d718-180">Premium-lagringskonton har hello följande skalbarhetsmål i tillägg toohello [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="1d718-180">Premium Storage accounts have hello following scalability targets in addition toohello [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).</span></span> <span data-ttu-id="1d718-181">Om kraven för application överskrider hello skalbarhetsmål för ett enda storage-konto, och skapa ditt program toouse flera lagringskonton och partitionera data mellan dessa lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="1d718-181">If your application requirements exceed hello scalability targets of a single storage account, build your application toouse multiple storage accounts, and partition your data across those storage accounts.</span></span>

| <span data-ttu-id="1d718-182">Total kapacitet</span><span class="sxs-lookup"><span data-stu-id="1d718-182">Total Account Capacity</span></span> | <span data-ttu-id="1d718-183">Totala bandbredden för ett lokalt Redundant Lagringskonto</span><span class="sxs-lookup"><span data-stu-id="1d718-183">Total Bandwidth for a Locally Redundant Storage Account</span></span> |
|:--- |:--- |
| <span data-ttu-id="1d718-184">Disken kapacitet: 35TB</span><span class="sxs-lookup"><span data-stu-id="1d718-184">Disk capacity: 35TB</span></span><br /><span data-ttu-id="1d718-185">Ögonblicksbild kapacitet: 10 TB</span><span class="sxs-lookup"><span data-stu-id="1d718-185">Snapshot capacity: 10 TB</span></span> |<span data-ttu-id="1d718-186">Konfigurera too50 Gigabit per sekund för inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="1d718-186">Up too50 gigabits per second for Inbound + Outbound</span></span> |

<span data-ttu-id="1d718-187">För mer information om specifikationer för Premium-lagring som Hej, kolla [skalbarhets- och prestandamål när du använder Premiumlagring](storage-premium-storage.md#scalability-and-performance-targets).</span><span class="sxs-lookup"><span data-stu-id="1d718-187">For hello more information on Premium Storage specifications, check out [Scalability and Performance Targets when using Premium Storage](storage-premium-storage.md#scalability-and-performance-targets).</span></span>

#### <a name="disk-caching-policy"></a><span data-ttu-id="1d718-188">Princip för cachelagring av disk</span><span class="sxs-lookup"><span data-stu-id="1d718-188">Disk caching policy</span></span>
<span data-ttu-id="1d718-189">Princip för cachelagring av disk är som standard *skrivskyddad* för alla hello Premium datadiskar och *Read-Write* för hello Premium operativsystemdisken kopplade toohello VM.</span><span class="sxs-lookup"><span data-stu-id="1d718-189">By default, disk caching policy is *Read-Only* for all hello Premium data disks, and *Read-Write* for hello Premium operating system disk attached toohello VM.</span></span> <span data-ttu-id="1d718-190">Den här konfigurationen rekommenderas tooachieve hello optimala prestanda för ditt program IOs.</span><span class="sxs-lookup"><span data-stu-id="1d718-190">This configuration setting is recommended tooachieve hello optimal performance for your application's IOs.</span></span> <span data-ttu-id="1d718-191">Inaktivera cachelagring på disk så att du kan få bättre prestanda för programmet för skrivintensiv eller lässkyddad datadiskar (till exempel loggfiler för SQL Server).</span><span class="sxs-lookup"><span data-stu-id="1d718-191">For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.</span></span> <span data-ttu-id="1d718-192">inställningar för cachelagring av hello för befintliga datadiskar kan uppdateras med hjälp av [Azure-portalen](https://portal.azure.com) eller hello *- HostCaching* parametern för hello *Set AzureDataDisk* cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1d718-192">hello cache settings for existing data disks can be updated using [Azure Portal](https://portal.azure.com) or hello *-HostCaching* parameter of hello *Set-AzureDataDisk* cmdlet.</span></span>

#### <a name="location"></a><span data-ttu-id="1d718-193">Plats</span><span class="sxs-lookup"><span data-stu-id="1d718-193">Location</span></span>
<span data-ttu-id="1d718-194">Välj en plats där Azure Premium-lagring är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="1d718-194">Pick a location where Azure Premium Storage is available.</span></span> <span data-ttu-id="1d718-195">Se [Azure-tjänster efter Region](https://azure.microsoft.com/regions/#services) uppdaterad information om tillgängliga platser.</span><span class="sxs-lookup"><span data-stu-id="1d718-195">See [Azure Services by Region](https://azure.microsoft.com/regions/#services) for up-to-date information on available locations.</span></span> <span data-ttu-id="1d718-196">Virtuella datorer finns i hello samma region som hello Storage-konto som lagrar hello diskar för hello VM ger mycket bättre prestanda än om de finns i olika områden.</span><span class="sxs-lookup"><span data-stu-id="1d718-196">VMs located in hello same region as hello Storage account that stores hello disks for hello VM will give much better performance than if they are in separate regions.</span></span>

#### <a name="other-azure-vm-configuration-settings"></a><span data-ttu-id="1d718-197">Andra Virtuella Azure-konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="1d718-197">Other Azure VM configuration settings</span></span>
<span data-ttu-id="1d718-198">När du skapar en Azure VM, blir du ombedd tooconfigure vissa VM-inställningar.</span><span class="sxs-lookup"><span data-stu-id="1d718-198">When creating an Azure VM, you will be asked tooconfigure certain VM settings.</span></span> <span data-ttu-id="1d718-199">Kom ihåg att några inställningar som är fasta under hello livstid hello VM, medan du kan ändra eller lägga till andra senare.</span><span class="sxs-lookup"><span data-stu-id="1d718-199">Remember, few settings are fixed for hello lifetime of hello VM, while you can modify or add others later.</span></span> <span data-ttu-id="1d718-200">Granska dessa Virtuella Azure-konfigurationsinställningar och se till att de är korrekt konfigurerad toomatch dina behov av arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="1d718-200">Review these Azure VM configuration settings and make sure that these are configured appropriately toomatch your workload requirements.</span></span>

### <a name="optimization"></a><span data-ttu-id="1d718-201">Optimering</span><span class="sxs-lookup"><span data-stu-id="1d718-201">Optimization</span></span>
<span data-ttu-id="1d718-202">[Azure Premium Storage: Utforma för högprestanda](storage-premium-storage-performance.md) innehåller riktlinjer för att skapa program med höga prestanda med Azure Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="1d718-202">[Azure Premium Storage: Design for High Performance](storage-premium-storage-performance.md) provides guidelines for building high-performance applications using Azure Premium Storage.</span></span> <span data-ttu-id="1d718-203">Du kan följa hello riktlinjer kombineras med prestanda bästa praxis tillämpliga tootechnologies används av ditt program.</span><span class="sxs-lookup"><span data-stu-id="1d718-203">You can follow hello guidelines combined with performance best practices applicable tootechnologies used by your application.</span></span>

## <span data-ttu-id="1d718-204"><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Förbereda och kopiera virtuella hårddiskar (VHD) tooPremium lagring</span><span class="sxs-lookup"><span data-stu-id="1d718-204"><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Prepare and copy virtual hard disks (VHDs) tooPremium Storage</span></span>
<span data-ttu-id="1d718-205">hello efter avsnittet innehåller riktlinjer för att förbereda virtuella hårddiskar från den virtuella datorn och kopiera virtuella hårddiskar tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="1d718-205">hello following section provides guidelines for preparing VHDs from your VM and copying VHDs tooAzure Storage.</span></span>

* [<span data-ttu-id="1d718-206">Scenario 1: ”jag migrera befintliga virtuella Azure-datorer tooAzure Premium-lagring”.</span><span class="sxs-lookup"><span data-stu-id="1d718-206">Scenario 1: "I am migrating existing Azure VMs tooAzure Premium Storage."</span></span>](#scenario1)
* [<span data-ttu-id="1d718-207">Scenario 2: ”jag migrera virtuella datorer från andra plattformar tooAzure Premium-lagring”.</span><span class="sxs-lookup"><span data-stu-id="1d718-207">Scenario 2: "I am migrating VMs from other platforms tooAzure Premium Storage."</span></span>](#scenario2)

### <a name="prerequisites"></a><span data-ttu-id="1d718-208">Krav</span><span class="sxs-lookup"><span data-stu-id="1d718-208">Prerequisites</span></span>
<span data-ttu-id="1d718-209">tooprepare hello virtuella hårddiskar för migrering, behöver du:</span><span class="sxs-lookup"><span data-stu-id="1d718-209">tooprepare hello VHDs for migration, you'll need:</span></span>

* <span data-ttu-id="1d718-210">En Azure-prenumeration, ett lagringskonto och en behållare i lagringsenheterna kontot toowhich kan du kopiera den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="1d718-210">An Azure subscription, a storage account, and a container in that storage account toowhich you can copy your VHD.</span></span> <span data-ttu-id="1d718-211">Observera att hello destinationslagringskontot kan vara ett Standard eller Premium-lagring konto utifrån dina behov.</span><span class="sxs-lookup"><span data-stu-id="1d718-211">Note that hello destination storage account can be a Standard or Premium Storage account depending on your requirement.</span></span>
* <span data-ttu-id="1d718-212">En verktyget toogeneralize hello VHD om du planerar toocreate flera VM-instanser från den.</span><span class="sxs-lookup"><span data-stu-id="1d718-212">A tool toogeneralize hello VHD if you plan toocreate multiple VM instances from it.</span></span> <span data-ttu-id="1d718-213">Till exempel sysprep för Windows eller Radnr i virtuell sysprep för Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="1d718-213">For example, sysprep for Windows or virt-sysprep for Ubuntu.</span></span>
* <span data-ttu-id="1d718-214">En verktyget tooupload hello VHD-filen toohello Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1d718-214">A tool tooupload hello VHD file toohello Storage account.</span></span> <span data-ttu-id="1d718-215">Se [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md) eller Använd en [Azure Lagringsutforskaren](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx).</span><span class="sxs-lookup"><span data-stu-id="1d718-215">See [Transfer data with hello AzCopy Command-Line Utility](storage-use-azcopy.md) or use an [Azure storage explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx).</span></span> <span data-ttu-id="1d718-216">Den här guiden beskriver kopiera den virtuella Hårddisken med hello AzCopy-verktyget.</span><span class="sxs-lookup"><span data-stu-id="1d718-216">This guide describes copying your VHD using hello AzCopy tool.</span></span>

> [!NOTE]
> <span data-ttu-id="1d718-217">Om du väljer synkron kopiera alternativet med AzCopy, för optimala prestanda, kopiera den virtuella Hårddisken genom att köra något av dessa verktyg från en Azure VM i hello samma region som hello mål-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="1d718-217">If you choose synchronous copy option with AzCopy, for optimal performance, copy your VHD by running one of these tools from an Azure VM that is in hello same region as hello destination storage account.</span></span> <span data-ttu-id="1d718-218">Om du kopierar en VHD från en Azure-dator i en annan region, gå din långsammare.</span><span class="sxs-lookup"><span data-stu-id="1d718-218">If you are copying a VHD from an Azure VM in a different region, your performance may be slower.</span></span>
>
> <span data-ttu-id="1d718-219">Överväg att kopiera stora mängder data över begränsad bandbredd, [med hello Azure Import/Export service tootransfer data tooBlob lagring](storage-import-export-service.md); detta kan du tootransfer dina data av leverans hårddisk enheter tooan Azure Datacenter.</span><span class="sxs-lookup"><span data-stu-id="1d718-219">For copying a large amount of data over limited bandwidth, consider [using hello Azure Import/Export service tootransfer data tooBlob Storage](storage-import-export-service.md); this allows you tootransfer your data by shipping hard disk drives tooan Azure datacenter.</span></span> <span data-ttu-id="1d718-220">Du kan använda hello Azure Import/Export service toocopy data tooa standardlagringskonto endast.</span><span class="sxs-lookup"><span data-stu-id="1d718-220">You can use hello Azure Import/Export service toocopy data tooa standard storage account only.</span></span> <span data-ttu-id="1d718-221">När hello data finns i ditt lagringskonto som standard, kan du använda antingen hello [kopiera Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) eller AzCopy tootransfer hello data tooyour premium storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1d718-221">Once hello data is in your standard storage account, you can use either hello [Copy Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) or AzCopy tootransfer hello data tooyour premium storage account.</span></span>
>
> <span data-ttu-id="1d718-222">Observera att Microsoft Azure bara stöder fast storlek VHD-filer.</span><span class="sxs-lookup"><span data-stu-id="1d718-222">Note that Microsoft Azure only supports fixed size VHD files.</span></span> <span data-ttu-id="1d718-223">VHDX-filer eller dynamiska virtuella hårddiskar stöds inte.</span><span class="sxs-lookup"><span data-stu-id="1d718-223">VHDX files or dynamic VHDs are not supported.</span></span> <span data-ttu-id="1d718-224">Om du har en dynamisk virtuell Hårddisk kan du konvertera den toofixed storlek med hello [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1d718-224">If you have a dynamic VHD, you can convert it toofixed size using hello [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) cmdlet.</span></span>
>
>

### <span data-ttu-id="1d718-225"><a name="scenario1"></a>Scenario 1: ”jag migrera befintliga virtuella Azure-datorer tooAzure Premium-lagring”.</span><span class="sxs-lookup"><span data-stu-id="1d718-225"><a name="scenario1"></a>Scenario 1: "I am migrating existing Azure VMs tooAzure Premium Storage."</span></span>
<span data-ttu-id="1d718-226">Om du migrerar befintliga virtuella Azure-datorer stoppa hello VM, Förbered virtuella hårddiskar per hello typ av virtuell Hårddisk som du vill och sedan kopiera hello VHD med AzCopy eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1d718-226">If you are migrating existing Azure VMs, stop hello VM, prepare VHDs per hello type of VHD you want, and then copy hello VHD with AzCopy or PowerShell.</span></span>

<span data-ttu-id="1d718-227">hello VM måste toobe helt ned toomigrate ett rent tillstånd.</span><span class="sxs-lookup"><span data-stu-id="1d718-227">hello VM needs toobe completely down toomigrate a clean state.</span></span> <span data-ttu-id="1d718-228">Det är inte ett driftstopp förrän hello migreringen är klar.</span><span class="sxs-lookup"><span data-stu-id="1d718-228">There will be a downtime until hello migration completes.</span></span>

#### <a name="step-1-prepare-vhds-for-migration"></a><span data-ttu-id="1d718-229">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="1d718-229">Step 1.</span></span> <span data-ttu-id="1d718-230">Förbereda virtuella hårddiskar för migrering</span><span class="sxs-lookup"><span data-stu-id="1d718-230">Prepare VHDs for migration</span></span>
<span data-ttu-id="1d718-231">Om du migrerar befintliga virtuella datorer i Azure tooPremium lagring kan vara den virtuella Hårddisken:</span><span class="sxs-lookup"><span data-stu-id="1d718-231">If you are migrating existing Azure VMs tooPremium Storage, your VHD may be:</span></span>

* <span data-ttu-id="1d718-232">En generaliserad operativsystemsavbildning</span><span class="sxs-lookup"><span data-stu-id="1d718-232">A generalized operating system image</span></span>
* <span data-ttu-id="1d718-233">En unik operativsystemdisk</span><span class="sxs-lookup"><span data-stu-id="1d718-233">A unique operating system disk</span></span>
* <span data-ttu-id="1d718-234">En datadisk</span><span class="sxs-lookup"><span data-stu-id="1d718-234">A data disk</span></span>

<span data-ttu-id="1d718-235">Nedan går vi igenom dessa 3 scenarier för att förbereda den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="1d718-235">Below we walk through these 3 scenarios for preparing your VHD.</span></span>

##### <a name="use-a-generalized-operating-system-vhd-toocreate-multiple-vm-instances"></a><span data-ttu-id="1d718-236">Använd en generaliserad virtuell Hårddisk för operativsystemet toocreate flera VM-instanser</span><span class="sxs-lookup"><span data-stu-id="1d718-236">Use a generalized Operating System VHD toocreate multiple VM instances</span></span>
<span data-ttu-id="1d718-237">Om du överför en virtuell Hårddisk som ska använda toocreate flera generiska Azure VM-instanser måste du först generalisera VHD med sysprep-verktyget.</span><span class="sxs-lookup"><span data-stu-id="1d718-237">If you are uploading a VHD that will be used toocreate multiple generic Azure VM instances, you must first generalize VHD using a sysprep utility.</span></span> <span data-ttu-id="1d718-238">Detta gäller tooa VHD som är lokalt eller i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="1d718-238">This applies tooa VHD that is on-premises or in hello cloud.</span></span> <span data-ttu-id="1d718-239">Sysprep tar bort alla datorspecifik information hello VHD.</span><span class="sxs-lookup"><span data-stu-id="1d718-239">Sysprep removes any machine-specific information from hello VHD.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1d718-240">Ta en ögonblicksbild eller säkerhetskopiera den virtuella datorn innan generalisera den.</span><span class="sxs-lookup"><span data-stu-id="1d718-240">Take a snapshot or backup your VM before generalizing it.</span></span> <span data-ttu-id="1d718-241">Kör sysprep avbryts och frigöra hello VM-instans.</span><span class="sxs-lookup"><span data-stu-id="1d718-241">Running sysprep will stop and deallocate hello VM instance.</span></span> <span data-ttu-id="1d718-242">Följ stegen nedan toosysprep en Windows OS-VHD.</span><span class="sxs-lookup"><span data-stu-id="1d718-242">Follow steps below toosysprep a Windows OS VHD.</span></span> <span data-ttu-id="1d718-243">Observera att köra hello Sysprep kommando kräver att du tooshut av hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1d718-243">Note that running hello Sysprep command will require you tooshut down hello virtual machine.</span></span> <span data-ttu-id="1d718-244">Mer information om Sysprep finns [Sysprep översikt](http://technet.microsoft.com/library/hh825209.aspx) eller [Teknisk referens för Sysprep](http://technet.microsoft.com/library/cc766049.aspx).</span><span class="sxs-lookup"><span data-stu-id="1d718-244">For more information about Sysprep, see [Sysprep Overview](http://technet.microsoft.com/library/hh825209.aspx) or [Sysprep Technical Reference](http://technet.microsoft.com/library/cc766049.aspx).</span></span>
>
>

1. <span data-ttu-id="1d718-245">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="1d718-245">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="1d718-246">Ange följande kommando tooopen Sysprep hello:</span><span class="sxs-lookup"><span data-stu-id="1d718-246">Enter hello following command tooopen Sysprep:</span></span>

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. <span data-ttu-id="1d718-247">Välj ange System Out of Box Experience (OOBE), Välj hello Generalize kryssrutan Markera i hello systemförberedelseverktyget, **avstängning**, och klicka sedan på **OK**som visas i hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="1d718-247">In hello System Preparation Tool, select Enter System Out-of-Box Experience (OOBE), select hello Generalize check box, select **Shutdown**, and then click **OK**, as shown in hello image below.</span></span> <span data-ttu-id="1d718-248">Sysprep generalize hello operativsystem och Stäng hello system.</span><span class="sxs-lookup"><span data-stu-id="1d718-248">Sysprep will generalize hello operating system and shut down hello system.</span></span>

    ![][1]

<span data-ttu-id="1d718-249">Använd Radnr i virtuell sysprep tooachieve för en Ubuntu VM hello samma.</span><span class="sxs-lookup"><span data-stu-id="1d718-249">For an Ubuntu VM, use virt-sysprep tooachieve hello same.</span></span> <span data-ttu-id="1d718-250">Se [Radnr i virtuell sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) för mer information.</span><span class="sxs-lookup"><span data-stu-id="1d718-250">See [virt-sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) for more details.</span></span> <span data-ttu-id="1d718-251">Se även några av hello öppen källkod [etablering av Linux-servrar programvara](http://www.cyberciti.biz/tips/server-provisioning-software.html) för andra Linux-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="1d718-251">See also some of hello open source [Linux Server Provisioning software](http://www.cyberciti.biz/tips/server-provisioning-software.html) for other Linux operating systems.</span></span>

##### <a name="use-a-unique-operating-system-vhd-toocreate-a-single-vm-instance"></a><span data-ttu-id="1d718-252">Använda en unik operativsystemet VHD toocreate en enda VM-instans</span><span class="sxs-lookup"><span data-stu-id="1d718-252">Use a unique Operating System VHD toocreate a single VM instance</span></span>
<span data-ttu-id="1d718-253">Om du har ett program som körs på hello VM som kräver hello datorn specifika data kan du inte generalisera hello VHD.</span><span class="sxs-lookup"><span data-stu-id="1d718-253">If you have an application running on hello VM which requires hello machine specific data, do not generalize hello VHD.</span></span> <span data-ttu-id="1d718-254">En ej generaliserad virtuell Hårddisk kan vara används toocreate ett unikt Virtuella Azure-instans.</span><span class="sxs-lookup"><span data-stu-id="1d718-254">A non-generalized VHD can be used toocreate a unique Azure VM instance.</span></span> <span data-ttu-id="1d718-255">Till exempel om du har en domänkontrollant på den virtuella Hårddisken kan gör köra sysprep det ineffektiv som en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="1d718-255">For example, if you have Domain Controller on your VHD, executing sysprep will make it ineffective as a Domain Controller.</span></span> <span data-ttu-id="1d718-256">Granska hello-program som körs på din virtuella datorn och hello effekten av att köra sysprep på dem. innan generalisera hello VHD.</span><span class="sxs-lookup"><span data-stu-id="1d718-256">Review hello applications running on your VM and hello impact of running sysprep on them before generalizing hello VHD.</span></span>

##### <a name="register-data-disk-vhd"></a><span data-ttu-id="1d718-257">Registrera datadisk VHD</span><span class="sxs-lookup"><span data-stu-id="1d718-257">Register data disk VHD</span></span>
<span data-ttu-id="1d718-258">Om du har migrerat datadiskar i Azure toobe, måste du se att hello VMs som använder dessa data diskar är avstängd.</span><span class="sxs-lookup"><span data-stu-id="1d718-258">If you have data disks in Azure toobe migrated, you must make sure hello VMs that use these data disks are shut down.</span></span>

<span data-ttu-id="1d718-259">Följ hello stegen som beskrivs nedan toocopy VHD tooAzure Premium-lagring och registrera den som en etablerade datadisk.</span><span class="sxs-lookup"><span data-stu-id="1d718-259">Follow hello steps described below toocopy VHD tooAzure Premium Storage and register it as a provisioned data disk.</span></span>

#### <a name="step-2-create-hello-destination-for-your-vhd"></a><span data-ttu-id="1d718-260">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="1d718-260">Step 2.</span></span> <span data-ttu-id="1d718-261">Skapa hello mål för den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="1d718-261">Create hello destination for your VHD</span></span>
<span data-ttu-id="1d718-262">Skapa ett lagringskonto för att underhålla de virtuella hårddiskarna.</span><span class="sxs-lookup"><span data-stu-id="1d718-262">Create a storage account for maintaining your VHDs.</span></span> <span data-ttu-id="1d718-263">Överväg följande punkter när du planerar var hello toostore de virtuella hårddiskarna:</span><span class="sxs-lookup"><span data-stu-id="1d718-263">Consider hello following points when planning where toostore your VHDs:</span></span>

* <span data-ttu-id="1d718-264">hello mål Premium storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1d718-264">hello target Premium storage account.</span></span>
* <span data-ttu-id="1d718-265">hello lagringskontoplatsen måste vara samma som Premium-lagring kan virtuella Azure-datorer skapas i hello sista steget.</span><span class="sxs-lookup"><span data-stu-id="1d718-265">hello storage account location must be same as Premium Storage capable Azure VMs you will create in hello final stage.</span></span> <span data-ttu-id="1d718-266">Du kan kopiera tooa nytt lagringskonto eller plan toouse hello samma lagringskonto baserat på dina behov.</span><span class="sxs-lookup"><span data-stu-id="1d718-266">You could copy tooa new storage account, or plan toouse hello same storage account based on your needs.</span></span>
* <span data-ttu-id="1d718-267">Kopiera och spara hello lagringskontonyckel av hello destinationslagringskontot för hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="1d718-267">Copy and save hello storage account key of hello destination storage account for hello next stage.</span></span>

<span data-ttu-id="1d718-268">Du kan välja tookeep vissa datadiskar i ett standardlagringskonto (till exempel diskar med kyligare lagring) för datadiskar, men rekommenderar vi du flytta alla data för produktion arbetsbelastning toouse premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="1d718-268">For data disks, you can choose tookeep some data disks in a standard storage account (for example, disks that have cooler storage), but we strongly recommend you moving all data for production workload toouse premium storage.</span></span>

#### <span data-ttu-id="1d718-269"><a name="copy-vhd-with-azcopy-or-powershell"></a>Steg 3.</span><span class="sxs-lookup"><span data-stu-id="1d718-269"><a name="copy-vhd-with-azcopy-or-powershell"></a>Step 3.</span></span> <span data-ttu-id="1d718-270">Kopiera VHD med AzCopy eller PowerShell</span><span class="sxs-lookup"><span data-stu-id="1d718-270">Copy VHD with AzCopy or PowerShell</span></span>
<span data-ttu-id="1d718-271">Du behöver toofind din sökvägen och lagring konto viktiga tooprocess något av dessa två alternativ.</span><span class="sxs-lookup"><span data-stu-id="1d718-271">You will need toofind your container path and storage account key tooprocess either of these two options.</span></span> <span data-ttu-id="1d718-272">Behållaren sökvägen och lagringsutrymmet kontonyckel finns i **Azure Portal** > **lagring**.</span><span class="sxs-lookup"><span data-stu-id="1d718-272">Container path and storage account key can be found in **Azure Portal** > **Storage**.</span></span> <span data-ttu-id="1d718-273">hello behållarens Webbadress kommer att som ”https://myaccount.blob.core.windows.net/mycontainer/”.</span><span class="sxs-lookup"><span data-stu-id="1d718-273">hello container URL will be like "https://myaccount.blob.core.windows.net/mycontainer/".</span></span>

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a><span data-ttu-id="1d718-274">Alternativ 1: Kopiera en virtuell Hårddisk med AzCopy (asynkron kopia)</span><span class="sxs-lookup"><span data-stu-id="1d718-274">Option 1: Copy a VHD with AzCopy (Asynchronous copy)</span></span>
<span data-ttu-id="1d718-275">Med AzCopy kan överföra du enkelt hello VHD över hello Internet.</span><span class="sxs-lookup"><span data-stu-id="1d718-275">Using AzCopy, you can easily upload hello VHD over hello Internet.</span></span> <span data-ttu-id="1d718-276">Detta kan ta tid beroende på hello stor hello virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="1d718-276">Depending on hello size of hello VHDs, this may take time.</span></span> <span data-ttu-id="1d718-277">Kom ihåg ingång-/ utgång lagringskontogränser för toocheck hello när du använder det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="1d718-277">Remember toocheck hello storage account ingress/egress limits when using this option.</span></span> <span data-ttu-id="1d718-278">Se [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="1d718-278">See [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) for details.</span></span>

1. <span data-ttu-id="1d718-279">Hämta och installera AzCopy härifrån: [senaste versionen av AzCopy](http://aka.ms/downloadazcopy)</span><span class="sxs-lookup"><span data-stu-id="1d718-279">Download and install AzCopy from here: [Latest version of AzCopy](http://aka.ms/downloadazcopy)</span></span>
2. <span data-ttu-id="1d718-280">Öppna Azure PowerShell och gå toohello mappen där AzCopy är installerat.</span><span class="sxs-lookup"><span data-stu-id="1d718-280">Open Azure PowerShell and go toohello folder where AzCopy is installed.</span></span>
3. <span data-ttu-id="1d718-281">Använd hello följande kommando toocopy hello VHD-filen från ”källa” för ”mål”.</span><span class="sxs-lookup"><span data-stu-id="1d718-281">Use hello following command toocopy hello VHD file from "Source" too"Destination".</span></span>

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    <span data-ttu-id="1d718-282">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1d718-282">Example:</span></span>

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```

    <span data-ttu-id="1d718-283">Nedan följer beskrivningar av hello parametrar som används i hello AzCopy-kommandot:</span><span class="sxs-lookup"><span data-stu-id="1d718-283">Here are descriptions of hello parameters used in hello AzCopy command:</span></span>

   * <span data-ttu-id="1d718-284">**/ Källa:  *&lt;källa&gt;:***  platsen för hello mapp eller lagring behållarens Webbadress som innehåller hello VHD.</span><span class="sxs-lookup"><span data-stu-id="1d718-284">**/Source: *&lt;source&gt;:*** Location of hello folder or storage container URL that contains hello VHD.</span></span>
   * <span data-ttu-id="1d718-285">**/ SourceKey:  *&lt;källa kontonyckel&gt;:***  lagringskontonyckel av hello källa storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1d718-285">**/SourceKey: *&lt;source-account-key&gt;:*** Storage account key of hello source storage account.</span></span>
   * <span data-ttu-id="1d718-286">**/ Dest:  *&lt;mål&gt;:***  lagring behållaren URL toocopy hello VHD till.</span><span class="sxs-lookup"><span data-stu-id="1d718-286">**/Dest: *&lt;destination&gt;:*** Storage container URL toocopy hello VHD to.</span></span>
   * <span data-ttu-id="1d718-287">**/ DestKey:  *&lt;dest kontonyckel&gt;:***  lagringskontonyckel för hello mål-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="1d718-287">**/DestKey: *&lt;dest-account-key&gt;:*** Storage account key of hello destination storage account.</span></span>
   * <span data-ttu-id="1d718-288">**/ Mönster:  *&lt;filnamn&gt;:***  ange hello filnamn hello VHD toocopy.</span><span class="sxs-lookup"><span data-stu-id="1d718-288">**/Pattern: *&lt;file-name&gt;:*** Specify hello file name of hello VHD toocopy.</span></span>

<span data-ttu-id="1d718-289">Mer information om hur du använder AzCopy verktyget, se [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="1d718-289">For details on using AzCopy tool, see [Transfer data with hello AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a><span data-ttu-id="1d718-290">Alternativ 2: Kopiera en VHD med PowerShell (Synchronized kopia)</span><span class="sxs-lookup"><span data-stu-id="1d718-290">Option 2: Copy a VHD with PowerShell (Synchronized copy)</span></span>
<span data-ttu-id="1d718-291">Du kan också kopiera hello VHD-fil med hello PowerShell-cmdleten Start-AzureStorageBlobCopy.</span><span class="sxs-lookup"><span data-stu-id="1d718-291">You can also copy hello VHD file using hello PowerShell cmdlet Start-AzureStorageBlobCopy.</span></span> <span data-ttu-id="1d718-292">Använd följande kommando på Azure PowerShell toocopy VHD hello.</span><span class="sxs-lookup"><span data-stu-id="1d718-292">Use hello following command on Azure PowerShell toocopy VHD.</span></span> <span data-ttu-id="1d718-293">Ersätt hello värden i <> med motsvarande värden från din käll- och storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1d718-293">Replace hello values in <> with corresponding values from your source and destination storage account.</span></span> <span data-ttu-id="1d718-294">Det här kommandot måste du ha en behållare som kallas virtuella hårddiskar i din destinationslagringskontot toouse.</span><span class="sxs-lookup"><span data-stu-id="1d718-294">toouse this command, you must have a container called vhds in your destination storage account.</span></span> <span data-ttu-id="1d718-295">Om hello-behållaren inte finns skapa en innan du kör hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="1d718-295">If hello container doesn't exist, create one before running hello command.</span></span>

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

<span data-ttu-id="1d718-296">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1d718-296">Example:</span></span>

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <span data-ttu-id="1d718-297"><a name="scenario2"></a>Scenario 2: ”jag migrera virtuella datorer från andra plattformar tooAzure Premium-lagring”.</span><span class="sxs-lookup"><span data-stu-id="1d718-297"><a name="scenario2"></a>Scenario 2: "I am migrating VMs from other platforms tooAzure Premium Storage."</span></span>
<span data-ttu-id="1d718-298">Om du migrerar VHD från icke - Azure Molnlagring tooAzure, måste du först exportera hello VHD tooa lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="1d718-298">If you are migrating VHD from non-Azure Cloud Storage tooAzure, you must first export hello VHD tooa local directory.</span></span> <span data-ttu-id="1d718-299">Har hello fullständig källsökvägen för hello lokal katalog där VHD lagras praktiska, och sedan använder AzCopy tooupload den tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="1d718-299">Have hello complete source path of hello local directory where VHD is stored handy, and then using AzCopy tooupload it tooAzure Storage.</span></span>

#### <a name="step-1-export-vhd-tooa-local-directory"></a><span data-ttu-id="1d718-300">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="1d718-300">Step 1.</span></span> <span data-ttu-id="1d718-301">Exportera VHD tooa lokal katalog</span><span class="sxs-lookup"><span data-stu-id="1d718-301">Export VHD tooa local directory</span></span>
##### <a name="copy-a-vhd-from-aws"></a><span data-ttu-id="1d718-302">Kopiera en VHD från AWS</span><span class="sxs-lookup"><span data-stu-id="1d718-302">Copy a VHD from AWS</span></span>
1. <span data-ttu-id="1d718-303">Om du använder AWS exportera hello EC2 instans tooa VHD i en Amazon S3-bucket.</span><span class="sxs-lookup"><span data-stu-id="1d718-303">If you are using AWS, export hello EC2 instance tooa VHD in an Amazon S3 bucket.</span></span> <span data-ttu-id="1d718-304">Följ hello stegen som beskrivs i hello Amazon-dokumentationen för export av instanser för Amazon EC2 tooinstall hello Amazon EC2 kommandoradsgränssnittet (CLI) verktyget och kör hello skapa-instans-export-kommandot aktivitet tooexport hello EC2 instans tooa VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="1d718-304">Follow hello steps described in hello Amazon documentation for Exporting Amazon EC2 Instances tooinstall hello Amazon EC2 command-line interface (CLI) tool and run hello create-instance-export-task command tooexport hello EC2 instance tooa VHD file.</span></span> <span data-ttu-id="1d718-305">Vara säker på att toouse **VHD** för hello DISK &#95; BILDEN & #95. FORMATET variabeln när du kör hello **skapa-instans-export-aktivitet** kommando.</span><span class="sxs-lookup"><span data-stu-id="1d718-305">Be sure toouse **VHD** for hello DISK&#95;IMAGE&#95;FORMAT variable when running hello **create-instance-export-task** command.</span></span> <span data-ttu-id="1d718-306">hello sparas exporterade VHD-filen i hello Amazon S3-bucket som du anger under den här processen.</span><span class="sxs-lookup"><span data-stu-id="1d718-306">hello exported VHD file is saved in hello Amazon S3 bucket you designate during that process.</span></span>

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. <span data-ttu-id="1d718-307">Hämta hello VHD-filen från hello S3-bucket.</span><span class="sxs-lookup"><span data-stu-id="1d718-307">Download hello VHD file from hello S3 bucket.</span></span> <span data-ttu-id="1d718-308">Välj hello VHD-filen, sedan **åtgärder** > **hämta**.</span><span class="sxs-lookup"><span data-stu-id="1d718-308">Select hello VHD file, then **Actions** > **Download**.</span></span>

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a><span data-ttu-id="1d718-309">Kopiera en VHD från andra Azure-moln</span><span class="sxs-lookup"><span data-stu-id="1d718-309">Copy a VHD from other non-Azure cloud</span></span>
<span data-ttu-id="1d718-310">Om du migrerar VHD från icke - Azure Molnlagring tooAzure, måste du först exportera hello VHD tooa lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="1d718-310">If you are migrating VHD from non-Azure Cloud Storage tooAzure, you must first export hello VHD tooa local directory.</span></span> <span data-ttu-id="1d718-311">Kopiera hello fullständig källsökvägen för hello lokal katalog där VHD lagras.</span><span class="sxs-lookup"><span data-stu-id="1d718-311">Copy hello complete source path of hello local directory where VHD is stored.</span></span>

##### <a name="copy-a-vhd-from-on-premises"></a><span data-ttu-id="1d718-312">Kopiera en VHD från lokala</span><span class="sxs-lookup"><span data-stu-id="1d718-312">Copy a VHD from on-premises</span></span>
<span data-ttu-id="1d718-313">Om du migrerar VHD från en lokal miljö, behöver hello fullständig sökväg där VHD lagras.</span><span class="sxs-lookup"><span data-stu-id="1d718-313">If you are migrating VHD from an on-premises environment, you will need hello complete source path where VHD is stored.</span></span> <span data-ttu-id="1d718-314">hello källsökväg kan vara en server eller en filresurs.</span><span class="sxs-lookup"><span data-stu-id="1d718-314">hello source path could be a server location or file share.</span></span>

#### <a name="step-2-create-hello-destination-for-your-vhd"></a><span data-ttu-id="1d718-315">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="1d718-315">Step 2.</span></span> <span data-ttu-id="1d718-316">Skapa hello mål för den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="1d718-316">Create hello destination for your VHD</span></span>
<span data-ttu-id="1d718-317">Skapa ett lagringskonto för att underhålla de virtuella hårddiskarna.</span><span class="sxs-lookup"><span data-stu-id="1d718-317">Create a storage account for maintaining your VHDs.</span></span> <span data-ttu-id="1d718-318">Överväg följande punkter när du planerar var hello toostore de virtuella hårddiskarna:</span><span class="sxs-lookup"><span data-stu-id="1d718-318">Consider hello following points when planning where toostore your VHDs:</span></span>

* <span data-ttu-id="1d718-319">hello mål-lagringskontot kan vara standard eller premium storage beroende på din programkrav.</span><span class="sxs-lookup"><span data-stu-id="1d718-319">hello target storage account could be standard or premium storage depending on your application requirement.</span></span>
* <span data-ttu-id="1d718-320">Hej lagringskontots region måste vara samma som Premium-lagring kan virtuella Azure-datorer skapas i hello sista steget.</span><span class="sxs-lookup"><span data-stu-id="1d718-320">hello storage account region must be same as Premium Storage capable Azure VMs you will create in hello final stage.</span></span> <span data-ttu-id="1d718-321">Du kan kopiera tooa nytt lagringskonto eller plan toouse hello samma lagringskonto baserat på dina behov.</span><span class="sxs-lookup"><span data-stu-id="1d718-321">You could copy tooa new storage account, or plan toouse hello same storage account based on your needs.</span></span>
* <span data-ttu-id="1d718-322">Kopiera och spara hello lagringskontonyckel av hello destinationslagringskontot för hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="1d718-322">Copy and save hello storage account key of hello destination storage account for hello next stage.</span></span>

<span data-ttu-id="1d718-323">Vi rekommenderar starkt att du flyttar alla data för produktion arbetsbelastning toouse premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="1d718-323">We strongly recommend you moving all data for production workload toouse premium storage.</span></span>

#### <a name="step-3-upload-hello-vhd-tooazure-storage"></a><span data-ttu-id="1d718-324">Steg 3.</span><span class="sxs-lookup"><span data-stu-id="1d718-324">Step 3.</span></span> <span data-ttu-id="1d718-325">Överför hello VHD tooAzure lagring</span><span class="sxs-lookup"><span data-stu-id="1d718-325">Upload hello VHD tooAzure Storage</span></span>
<span data-ttu-id="1d718-326">Nu när du har den virtuella Hårddisken i hello lokal katalog, kan du använda AzCopy eller AzurePowerShell tooupload hello VHD-filen tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="1d718-326">Now that you have your VHD in hello local directory, you can use AzCopy or AzurePowerShell tooupload hello .vhd file tooAzure Storage.</span></span> <span data-ttu-id="1d718-327">Båda alternativen finns här:</span><span class="sxs-lookup"><span data-stu-id="1d718-327">Both options are provided here:</span></span>

##### <a name="option-1-using-azure-powershell-add-azurevhd-tooupload-hello-vhd-file"></a><span data-ttu-id="1d718-328">Alternativ 1: Använda Azure PowerShell Add-AzureVhd tooupload hello VHD-filen</span><span class="sxs-lookup"><span data-stu-id="1d718-328">Option 1: Using Azure PowerShell Add-AzureVhd tooupload hello .vhd file</span></span>

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

<span data-ttu-id="1d718-329">Ett exempel <Uri> kanske ***”https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd”***.</span><span class="sxs-lookup"><span data-stu-id="1d718-329">An example <Uri> might be ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***.</span></span> <span data-ttu-id="1d718-330">Ett exempel <FileInfo> kanske ***”C:\path\to\upload.vhd”***.</span><span class="sxs-lookup"><span data-stu-id="1d718-330">An example <FileInfo> might be ***"C:\path\to\upload.vhd"***.</span></span>

##### <a name="option-2-using-azcopy-tooupload-hello-vhd-file"></a><span data-ttu-id="1d718-331">Alternativ 2: Använda AzCopy tooupload hello VHD-filen</span><span class="sxs-lookup"><span data-stu-id="1d718-331">Option 2: Using AzCopy tooupload hello .vhd file</span></span>
<span data-ttu-id="1d718-332">Med AzCopy kan överföra du enkelt hello VHD över hello Internet.</span><span class="sxs-lookup"><span data-stu-id="1d718-332">Using AzCopy, you can easily upload hello VHD over hello Internet.</span></span> <span data-ttu-id="1d718-333">Detta kan ta tid beroende på hello stor hello virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="1d718-333">Depending on hello size of hello VHDs, this may take time.</span></span> <span data-ttu-id="1d718-334">Kom ihåg ingång-/ utgång lagringskontogränser för toocheck hello när du använder det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="1d718-334">Remember toocheck hello storage account ingress/egress limits when using this option.</span></span> <span data-ttu-id="1d718-335">Se [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="1d718-335">See [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) for details.</span></span>

1. <span data-ttu-id="1d718-336">Hämta och installera AzCopy härifrån: [senaste versionen av AzCopy](http://aka.ms/downloadazcopy)</span><span class="sxs-lookup"><span data-stu-id="1d718-336">Download and install AzCopy from here: [Latest version of AzCopy](http://aka.ms/downloadazcopy)</span></span>
2. <span data-ttu-id="1d718-337">Öppna Azure PowerShell och gå toohello mappen där AzCopy är installerat.</span><span class="sxs-lookup"><span data-stu-id="1d718-337">Open Azure PowerShell and go toohello folder where AzCopy is installed.</span></span>
3. <span data-ttu-id="1d718-338">Använd hello följande kommando toocopy hello VHD-filen från ”källa” för ”mål”.</span><span class="sxs-lookup"><span data-stu-id="1d718-338">Use hello following command toocopy hello VHD file from "Source" too"Destination".</span></span>

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    <span data-ttu-id="1d718-339">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1d718-339">Example:</span></span>

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
    ```

    <span data-ttu-id="1d718-340">Nedan följer beskrivningar av hello parametrar som används i hello AzCopy-kommandot:</span><span class="sxs-lookup"><span data-stu-id="1d718-340">Here are descriptions of hello parameters used in hello AzCopy command:</span></span>

   * <span data-ttu-id="1d718-341">**/ Källa:  *&lt;källa&gt;:***  platsen för hello mapp eller lagring behållarens Webbadress som innehåller hello VHD.</span><span class="sxs-lookup"><span data-stu-id="1d718-341">**/Source: *&lt;source&gt;:*** Location of hello folder or storage container URL that contains hello VHD.</span></span>
   * <span data-ttu-id="1d718-342">**/ SourceKey:  *&lt;källa kontonyckel&gt;:***  lagringskontonyckel av hello källa storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1d718-342">**/SourceKey: *&lt;source-account-key&gt;:*** Storage account key of hello source storage account.</span></span>
   * <span data-ttu-id="1d718-343">**/ Dest:  *&lt;mål&gt;:***  lagring behållaren URL toocopy hello VHD till.</span><span class="sxs-lookup"><span data-stu-id="1d718-343">**/Dest: *&lt;destination&gt;:*** Storage container URL toocopy hello VHD to.</span></span>
   * <span data-ttu-id="1d718-344">**/ DestKey:  *&lt;dest kontonyckel&gt;:***  lagringskontonyckel för hello mål-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="1d718-344">**/DestKey: *&lt;dest-account-key&gt;:*** Storage account key of hello destination storage account.</span></span>
   * <span data-ttu-id="1d718-345">**/ BlobType: sidan:** anger att hello-målet är en sidblobb.</span><span class="sxs-lookup"><span data-stu-id="1d718-345">**/BlobType: page:** Specifies that hello destination is a page blob.</span></span>
   * <span data-ttu-id="1d718-346">**/ Mönster:  *&lt;filnamn&gt;:***  ange hello filnamn hello VHD toocopy.</span><span class="sxs-lookup"><span data-stu-id="1d718-346">**/Pattern: *&lt;file-name&gt;:*** Specify hello file name of hello VHD toocopy.</span></span>

<span data-ttu-id="1d718-347">Mer information om hur du använder AzCopy verktyget, se [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="1d718-347">For details on using AzCopy tool, see [Transfer data with hello AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

##### <a name="other-options-for-uploading-a-vhd"></a><span data-ttu-id="1d718-348">Andra alternativ för att överföra en virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="1d718-348">Other options for uploading a VHD</span></span>
<span data-ttu-id="1d718-349">Du kan också ladda upp en VHD tooyour storage-konto med en hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1d718-349">You can also upload a VHD tooyour storage account using one of hello following means:</span></span>

* [<span data-ttu-id="1d718-350">Azure Storage kopiera Blob API</span><span class="sxs-lookup"><span data-stu-id="1d718-350">Azure Storage Copy Blob API</span></span>](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [<span data-ttu-id="1d718-351">Azure Storage Explorer överför Blobbar</span><span class="sxs-lookup"><span data-stu-id="1d718-351">Azure Storage Explorer Uploading Blobs</span></span>](https://azurestorageexplorer.codeplex.com/)
* [<span data-ttu-id="1d718-352">Storage Import/Export Service REST API-referens</span><span class="sxs-lookup"><span data-stu-id="1d718-352">Storage Import/Export Service REST API Reference</span></span>](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> <span data-ttu-id="1d718-353">Du rekommenderas att använda Import/Export Service om överföring tid är längre än 7 dagar.</span><span class="sxs-lookup"><span data-stu-id="1d718-353">We recommend using Import/Export Service if estimated uploading time is longer than 7 days.</span></span> <span data-ttu-id="1d718-354">Du kan använda [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) tooestimate hello tiden från enhet för storlek och överföring av data.</span><span class="sxs-lookup"><span data-stu-id="1d718-354">You can use [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) tooestimate hello time from data size and transfer unit.</span></span>
>
> <span data-ttu-id="1d718-355">Import/Export kan vara används tooa toocopy standardlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="1d718-355">Import/Export can be used toocopy tooa standard storage account.</span></span> <span data-ttu-id="1d718-356">Du behöver toocopy från standardlagring toopremium storage-konto med ett verktyg som AzCopy.</span><span class="sxs-lookup"><span data-stu-id="1d718-356">You will need toocopy from standard storage toopremium storage account using a tool like AzCopy.</span></span>
>
>

## <span data-ttu-id="1d718-357"><a name="create-azure-virtual-machine-using-premium-storage"></a>Skapa virtuella Azure-datorer med Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="1d718-357"><a name="create-azure-virtual-machine-using-premium-storage"></a>Create Azure VMs using Premium Storage</span></span>
<span data-ttu-id="1d718-358">När hello VHD är överförda eller kopierade toohello önskad lagringskonto, följ hello instruktionerna i det här avsnittet tooregister hello VHD som en operativsystemsavbildning eller OS-disken beroende på ditt scenario och sedan skapa en VM-instans från den.</span><span class="sxs-lookup"><span data-stu-id="1d718-358">After hello VHD is uploaded or copied toohello desired storage account, follow hello instructions in this section tooregister hello VHD as an OS image, or OS disk depending on your scenario and then create a VM instance from it.</span></span> <span data-ttu-id="1d718-359">Hej datadisk VHD kan vara anslutna toohello VM när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="1d718-359">hello data disk VHD can be attached toohello VM once it is created.</span></span>
<span data-ttu-id="1d718-360">En migrering exempelskript tillhandahålls hello slutet av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1d718-360">A sample migration script is provided at hello end of this section.</span></span> <span data-ttu-id="1d718-361">Det här enkla skriptet matchar inte alla scenarier.</span><span class="sxs-lookup"><span data-stu-id="1d718-361">This simple script does not match all scenarios.</span></span> <span data-ttu-id="1d718-362">Du kan behöva tooupdate hello skriptet toomatch med din situation.</span><span class="sxs-lookup"><span data-stu-id="1d718-362">You may need tooupdate hello script toomatch with your specific scenario.</span></span> <span data-ttu-id="1d718-363">toosee om det här skriptet gäller tooyour scenariot nedan finns [ett exempelskript för migrering](#a-sample-migration-script).</span><span class="sxs-lookup"><span data-stu-id="1d718-363">toosee if this script applies tooyour scenario, see below [A Sample Migration Script](#a-sample-migration-script).</span></span>

### <a name="checklist"></a><span data-ttu-id="1d718-364">Checklista</span><span class="sxs-lookup"><span data-stu-id="1d718-364">Checklist</span></span>
1. <span data-ttu-id="1d718-365">Vänta tills alla hello VHD-diskar kopiera har slutförts.</span><span class="sxs-lookup"><span data-stu-id="1d718-365">Wait until all hello VHD disks copying is complete.</span></span>
2. <span data-ttu-id="1d718-366">Kontrollera att Premium-lagring finns i hello region du migrerar till.</span><span class="sxs-lookup"><span data-stu-id="1d718-366">Make sure Premium Storage is available in hello region you are migrating to.</span></span>
3. <span data-ttu-id="1d718-367">Bestäm hello ny virtuell dator-serie du kommer att använda.</span><span class="sxs-lookup"><span data-stu-id="1d718-367">Decide hello new VM series you will be using.</span></span> <span data-ttu-id="1d718-368">Det bör vara en Premium-lagring som är kompatibla och hello storlek bör vara beroende på hello tillgänglighet i hello region och beroende på dina behov.</span><span class="sxs-lookup"><span data-stu-id="1d718-368">It should be a Premium Storage capable, and hello size should be depending on hello availability in hello region and based on your needs.</span></span>
4. <span data-ttu-id="1d718-369">Bestäm hello exakt VM-storlek du ska använda.</span><span class="sxs-lookup"><span data-stu-id="1d718-369">Decide hello exact VM size you will use.</span></span> <span data-ttu-id="1d718-370">VM-storlek måste toobe tillräckligt stor toosupport hello antalet datadiskar som du har.</span><span class="sxs-lookup"><span data-stu-id="1d718-370">VM size needs toobe large enough toosupport hello number of data disks you have.</span></span> <span data-ttu-id="1d718-371">T.ex.</span><span class="sxs-lookup"><span data-stu-id="1d718-371">E.g.</span></span> <span data-ttu-id="1d718-372">Om du har 4 datadiskar har hello VM 2 eller flera kärnor.</span><span class="sxs-lookup"><span data-stu-id="1d718-372">if you have 4 data disks, hello VM must have 2 or more cores.</span></span> <span data-ttu-id="1d718-373">Du kan också processorkraft, minne och nätverksbandbredd måste.</span><span class="sxs-lookup"><span data-stu-id="1d718-373">Also, consider processing power, memory and network bandwidth needs.</span></span>
5. <span data-ttu-id="1d718-374">Skapa ett Premium Storage-konto i hello målregionen.</span><span class="sxs-lookup"><span data-stu-id="1d718-374">Create a Premium Storage account in hello target region.</span></span> <span data-ttu-id="1d718-375">Det här är hello-konto som ska användas för hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1d718-375">This is hello account you will use for hello new VM.</span></span>
6. <span data-ttu-id="1d718-376">Ha hello aktuella VM information praktisk, inklusive hello lista över diskar och motsvarande VHD-blobbar.</span><span class="sxs-lookup"><span data-stu-id="1d718-376">Have hello current VM details handy, including hello list of disks and corresponding VHD blobs.</span></span>

<span data-ttu-id="1d718-377">Förbered ditt program för driftstopp.</span><span class="sxs-lookup"><span data-stu-id="1d718-377">Prepare your application for downtime.</span></span> <span data-ttu-id="1d718-378">toodo en ren migrering, har du toostop all hello bearbetning i hello nuvarande systemet.</span><span class="sxs-lookup"><span data-stu-id="1d718-378">toodo a clean migration, you have toostop all hello processing in hello current system.</span></span> <span data-ttu-id="1d718-379">Först då kan du få tooconsistent tillstånd som du kan migrera nya toohello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="1d718-379">Only then you can get it tooconsistent state which you can migrate toohello new platform.</span></span> <span data-ttu-id="1d718-380">Varaktighet för stilleståndstid beror på hello mängden data i hello diskar toomigrate.</span><span class="sxs-lookup"><span data-stu-id="1d718-380">Downtime duration will depend on hello amount of data in hello disks toomigrate.</span></span>

> [!NOTE]
> <span data-ttu-id="1d718-381">Om du skapar en Azure Resource Manager-VM från en särskild VHD-Disk, se för[mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) för distribution av Resource Manager VM med hjälp av den befintliga disken.</span><span class="sxs-lookup"><span data-stu-id="1d718-381">If you are creating an Azure Resource Manager VM from a specialized VHD Disk, please refer too[this template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) for deploying Resource Manager VM using existing disk.</span></span>
>
>

### <a name="register-your-vhd"></a><span data-ttu-id="1d718-382">Registrera den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="1d718-382">Register your VHD</span></span>
<span data-ttu-id="1d718-383">toocreate en virtuell dator från OS VHD- eller tooattach tooa en data-disk ny virtuell dator, måste du först registrera den.</span><span class="sxs-lookup"><span data-stu-id="1d718-383">toocreate a VM from OS VHD or tooattach a data disk tooa new VM, you must first register them.</span></span> <span data-ttu-id="1d718-384">Följ instruktionerna nedan scenario för den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="1d718-384">Follow steps below depending on your VHD's scenario.</span></span>

#### <a name="generalized-operating-system-vhd-toocreate-multiple-azure-vm-instances"></a><span data-ttu-id="1d718-385">Generaliserad virtuell Hårddisk för operativsystemet toocreate flera Virtuella Azure-instanser</span><span class="sxs-lookup"><span data-stu-id="1d718-385">Generalized Operating System VHD toocreate multiple Azure VM instances</span></span>
<span data-ttu-id="1d718-386">När generaliserad OS-avbildningen VHD har överförts toohello storage-konto, kan du registrera den som en **Azure VM-avbildning** så att du kan skapa en eller flera VM-instanser av den.</span><span class="sxs-lookup"><span data-stu-id="1d718-386">After generalized OS image VHD is uploaded toohello storage account, register it as an **Azure VM Image** so that you can create one or more VM instances from it.</span></span> <span data-ttu-id="1d718-387">Använd följande PowerShell-cmdlets tooregister hello den virtuella Hårddisken som en Azure VM OS-bild.</span><span class="sxs-lookup"><span data-stu-id="1d718-387">Use hello following PowerShell cmdlets tooregister your VHD as an Azure VM OS image.</span></span> <span data-ttu-id="1d718-388">Ange URL för hello fullständig behållare där VHD kopierades till.</span><span class="sxs-lookup"><span data-stu-id="1d718-388">Provide hello complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

<span data-ttu-id="1d718-389">Kopiera och spara hello namnet på den här nya Azure VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="1d718-389">Copy and save hello name of this new Azure VM Image.</span></span> <span data-ttu-id="1d718-390">Hello-exemplet ovan, är det *OSImageName*.</span><span class="sxs-lookup"><span data-stu-id="1d718-390">In hello example above, it is *OSImageName*.</span></span>

#### <a name="unique-operating-system-vhd-toocreate-a-single-azure-vm-instance"></a><span data-ttu-id="1d718-391">Unik operativsystemet VHD toocreate en Azure VM-instans</span><span class="sxs-lookup"><span data-stu-id="1d718-391">Unique Operating System VHD toocreate a single Azure VM instance</span></span>
<span data-ttu-id="1d718-392">När hello unika OS VHD är överförda toohello lagring konto måste du registrera den som en **Azure OS-disken** så att du kan skapa en VM-instans av den.</span><span class="sxs-lookup"><span data-stu-id="1d718-392">After hello unique OS VHD is uploaded toohello storage account, register it as an **Azure OS Disk** so that you can create a VM instance from it.</span></span> <span data-ttu-id="1d718-393">Använd dessa PowerShell-cmdlets tooregister den virtuella Hårddisken som en Azure OS-Disk.</span><span class="sxs-lookup"><span data-stu-id="1d718-393">Use these PowerShell cmdlets tooregister your VHD as an Azure OS Disk.</span></span> <span data-ttu-id="1d718-394">Ange URL för hello fullständig behållare där VHD kopierades till.</span><span class="sxs-lookup"><span data-stu-id="1d718-394">Provide hello complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

<span data-ttu-id="1d718-395">Kopiera och spara hello namnet på den här nya Azure OS-disken.</span><span class="sxs-lookup"><span data-stu-id="1d718-395">Copy and save hello name of this new Azure OS Disk.</span></span> <span data-ttu-id="1d718-396">Hello-exemplet ovan, är det *OSDisk*.</span><span class="sxs-lookup"><span data-stu-id="1d718-396">In hello example above, it is *OSDisk*.</span></span>

#### <a name="data-disk-vhd-toobe-attached-toonew-azure-vm-instances"></a><span data-ttu-id="1d718-397">Data disk VHD toobe kopplade toonew Azure VM-instanser</span><span class="sxs-lookup"><span data-stu-id="1d718-397">Data disk VHD toobe attached toonew Azure VM instance(s)</span></span>
<span data-ttu-id="1d718-398">När hello datadisk VHD har överförts toostorage konto, registrera den som en Azure-datadisk så att det kan vara anslutna tooyour nya DS-serien, DSv2-serien eller GS-serien Azure VM-instans.</span><span class="sxs-lookup"><span data-stu-id="1d718-398">After hello data disk VHD is uploaded toostorage account, register it as an Azure Data Disk so that it can be attached tooyour new DS Series, DSv2 series or GS Series Azure VM instance.</span></span>

<span data-ttu-id="1d718-399">Använd följande PowerShell-cmdlets tooregister den virtuella Hårddisken som en Azure-datadisk.</span><span class="sxs-lookup"><span data-stu-id="1d718-399">Use these PowerShell cmdlets tooregister your VHD as an Azure Data Disk.</span></span> <span data-ttu-id="1d718-400">Ange URL för hello fullständig behållare där VHD kopierades till.</span><span class="sxs-lookup"><span data-stu-id="1d718-400">Provide hello complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

<span data-ttu-id="1d718-401">Kopiera och spara hello namnet på den här nya Azure-datadisk.</span><span class="sxs-lookup"><span data-stu-id="1d718-401">Copy and save hello name of this new Azure Data Disk.</span></span> <span data-ttu-id="1d718-402">Hello-exemplet ovan, är det *DataDisk*.</span><span class="sxs-lookup"><span data-stu-id="1d718-402">In hello example above, it is *DataDisk*.</span></span>

### <a name="create-a-premium-storage-capable-vm"></a><span data-ttu-id="1d718-403">Skapa en Premium-lagring kan virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1d718-403">Create a Premium Storage capable VM</span></span>
<span data-ttu-id="1d718-404">En gång hello OS-avbildningen eller OS-disken registreras, skapa en ny DS-serien, DSv2-serien eller GS-serien VM.</span><span class="sxs-lookup"><span data-stu-id="1d718-404">Once hello OS image or OS disk are registered, create a new DS-series, DSv2-series or GS-series VM.</span></span> <span data-ttu-id="1d718-405">Du kommer att använda hello operativsystemavbildning eller operativsystemet diskens namn som du har registrerat.</span><span class="sxs-lookup"><span data-stu-id="1d718-405">You will be using hello operating system image or operating system disk name that you registered.</span></span> <span data-ttu-id="1d718-406">Välj hello VM typen hello Premium lagringsnivå.</span><span class="sxs-lookup"><span data-stu-id="1d718-406">Select hello VM type from hello Premium Storage tier.</span></span> <span data-ttu-id="1d718-407">I exemplet nedan använder hello *Standard_DS2* VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="1d718-407">In example below, we are using hello *Standard_DS2* VM size.</span></span>

> [!NOTE]
> <span data-ttu-id="1d718-408">Uppdatera hello disk storlek toomake att den matchar din kapacitet och prestandakrav och hello Azure ledigt storlekar.</span><span class="sxs-lookup"><span data-stu-id="1d718-408">Update hello disk size toomake sure it matches your capacity and performance requirements and hello available Azure disk sizes.</span></span>
>
>

<span data-ttu-id="1d718-409">Följ hello steg för steg PowerShell-cmdlet nedan toocreate hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1d718-409">Follow hello step by step PowerShell cmdlets below toocreate hello new VM.</span></span> <span data-ttu-id="1d718-410">Ange först hello gemensamma parametrar:</span><span class="sxs-lookup"><span data-stu-id="1d718-410">First, set hello common parameters:</span></span>

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

<span data-ttu-id="1d718-411">Först skapa en molnbaserad tjänst där du ska vara värd för din nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1d718-411">First, create a cloud service in which you will be hosting your new VMs.</span></span>

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

<span data-ttu-id="1d718-412">Skapa sedan hello Azure VM-instans från hello OS-avbildningen eller OS-Disk som du har registrerat beroende på ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="1d718-412">Next, depending on your scenario, create hello Azure VM instance from either hello OS Image or OS Disk that you registered.</span></span>

#### <a name="generalized-operating-system-vhd-toocreate-multiple-azure-vm-instances"></a><span data-ttu-id="1d718-413">Generaliserad virtuell Hårddisk för operativsystemet toocreate flera Virtuella Azure-instanser</span><span class="sxs-lookup"><span data-stu-id="1d718-413">Generalized Operating System VHD toocreate multiple Azure VM instances</span></span>
<span data-ttu-id="1d718-414">Skapa hello en eller flera nya DS-serien Azure VM-instanser som använder hello **Azure OS-avbildningen** som har registrerats.</span><span class="sxs-lookup"><span data-stu-id="1d718-414">Create hello one or more new DS Series Azure VM instances using hello **Azure OS Image** that you registered.</span></span> <span data-ttu-id="1d718-415">Ange operativsystemsavbildning namnet hello VM-konfiguration när du skapar ny virtuell dator som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="1d718-415">Specify this OS Image name in hello VM configuration when creating new VM as shown below.</span></span>

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-toocreate-a-single-azure-vm-instance"></a><span data-ttu-id="1d718-416">Unik operativsystemet VHD toocreate en Azure VM-instans</span><span class="sxs-lookup"><span data-stu-id="1d718-416">Unique Operating System VHD toocreate a single Azure VM instance</span></span>
<span data-ttu-id="1d718-417">Skapa en ny DS-serien Virtuella Azure-instans med hello **Azure OS-disken** som har registrerats.</span><span class="sxs-lookup"><span data-stu-id="1d718-417">Create a new DS series Azure VM instance using hello **Azure OS Disk** that you registered.</span></span> <span data-ttu-id="1d718-418">Ange den här OS-disknamnet hello VM-konfiguration när hello skapar ny virtuell dator som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="1d718-418">Specify this OS Disk name in hello VM configuration when creating hello new VM as shown below.</span></span>

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

<span data-ttu-id="1d718-419">Ange andra Virtuella Azure-information, till exempel en molntjänst, region, storage-konto, tillgänglighetsuppsättning och cachelagringsprincipen.</span><span class="sxs-lookup"><span data-stu-id="1d718-419">Specify other Azure VM information, such as a cloud service, region, storage account, availability set, and caching policy.</span></span> <span data-ttu-id="1d718-420">Observera att hello VM-instans måste samordnas med associerade operativsystem eller hårddiskar, så hello valt cloud service, region och lagring konto måste vara i hello samma plats som hello underliggande virtuella hårddiskar för dessa diskar.</span><span class="sxs-lookup"><span data-stu-id="1d718-420">Note that hello VM instance must be co-located with associated operating system or data disks, so hello selected cloud service, region and storage account must all be in hello same location as hello underlying VHDs of those disks.</span></span>

### <a name="attach-data-disk"></a><span data-ttu-id="1d718-421">Anslut en datadisk</span><span class="sxs-lookup"><span data-stu-id="1d718-421">Attach data disk</span></span>
<span data-ttu-id="1d718-422">Slutligen, om du har registrerat data disk virtuella hårddiskar, bifoga dem toohello nya Premium-lagring kan Azure VM.</span><span class="sxs-lookup"><span data-stu-id="1d718-422">Lastly, if you have registered data disk VHDs, attach them toohello new Premium Storage capable Azure VM.</span></span>

<span data-ttu-id="1d718-423">Använd följande PowerShell-cmdlet tooattach data disk toohello ny virtuell dator och ange hello princip för cachelagring.</span><span class="sxs-lookup"><span data-stu-id="1d718-423">Use following PowerShell cmdlet tooattach data disk toohello new VM and specify hello caching policy.</span></span> <span data-ttu-id="1d718-424">I exemplet nedan hello cachelagring princip har angetts för*ReadOnly*.</span><span class="sxs-lookup"><span data-stu-id="1d718-424">In example below hello caching policy is set too*ReadOnly*.</span></span>

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> <span data-ttu-id="1d718-425">Det kan finnas ytterligare steg krävs toosupport ditt program som inte omfattas av den här guiden.</span><span class="sxs-lookup"><span data-stu-id="1d718-425">There may be additional steps necessary toosupport your application that is not be covered by this guide.</span></span>
>
>

### <a name="checking-and-plan-backup"></a><span data-ttu-id="1d718-426">Kontrollera och planera säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="1d718-426">Checking and plan backup</span></span>
<span data-ttu-id="1d718-427">En gång hello nya virtuella datorn är igång och körs, åtkomst till den med hjälp av hello samma inloggnings-id och lösenord som hello ursprungliga virtuella datorn och kontrollera att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="1d718-427">Once hello new VM is up and running, access it using hello same login id and password is as hello original VM, and verify that everything is working as expected.</span></span> <span data-ttu-id="1d718-428">Alla hello-inställningar, inklusive hello stripe-volymer, finnas i hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1d718-428">All hello settings, including hello striped volumes, would be present in hello new VM.</span></span>

<span data-ttu-id="1d718-429">hello sista steget är tooplan schema för säkerhetskopiering och underhåll för hello ny virtuell dator baserat på hello programbehov.</span><span class="sxs-lookup"><span data-stu-id="1d718-429">hello last step is tooplan backup and maintenance schedule for hello new VM based on hello application's needs.</span></span>

### <span data-ttu-id="1d718-430"><a name="a-sample-migration-script"></a>Ett exempelskript för migrering</span><span class="sxs-lookup"><span data-stu-id="1d718-430"><a name="a-sample-migration-script"></a>A sample migration script</span></span>
<span data-ttu-id="1d718-431">Om du har flera virtuella datorer toomigrate vara automation via PowerShell-skript användbara.</span><span class="sxs-lookup"><span data-stu-id="1d718-431">If you have multiple VMs toomigrate, automation through PowerShell scripts will be helpful.</span></span> <span data-ttu-id="1d718-432">Följande är ett exempelskript som automatiserar hello migreringen av en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1d718-432">Following is a sample script that automates hello migration of a VM.</span></span> <span data-ttu-id="1d718-433">Observera som skriptet nedan är bara ett exempel och det finns några antaganden om hello aktuella Virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="1d718-433">Note that below script is only an example, and there are few assumptions made about hello current VM disks.</span></span> <span data-ttu-id="1d718-434">Du kan behöva tooupdate hello skriptet toomatch med din situation.</span><span class="sxs-lookup"><span data-stu-id="1d718-434">You may need tooupdate hello script toomatch with your specific scenario.</span></span>

<span data-ttu-id="1d718-435">hello antaganden är:</span><span class="sxs-lookup"><span data-stu-id="1d718-435">hello assumptions are:</span></span>

* <span data-ttu-id="1d718-436">Du skapar klassiska virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="1d718-436">You are creating classic Azure VMs.</span></span>
* <span data-ttu-id="1d718-437">Källan OS diskar och källa Datadiskar finns i samma lagringskonto och samma behållare.</span><span class="sxs-lookup"><span data-stu-id="1d718-437">Your source OS Disks and source Data Disks are in same storage account and same container.</span></span> <span data-ttu-id="1d718-438">Om det inte i OS-diskar och Datadiskar hello samma placera, du kan använda AzCopy eller Azure PowerShell toocopy virtuella hårddiskar över storage-konton och behållare.</span><span class="sxs-lookup"><span data-stu-id="1d718-438">If your OS Disks and Data Disks are not in hello same place, you can use AzCopy or Azure PowerShell toocopy VHDs over storage accounts and containers.</span></span> <span data-ttu-id="1d718-439">Se toohello föregående steg: [kopiera VHD med AzCopy eller PowerShell](#copy-vhd-with-azcopy-or-powershell).</span><span class="sxs-lookup"><span data-stu-id="1d718-439">Refer toohello previous step: [Copy VHD with AzCopy or PowerShell](#copy-vhd-with-azcopy-or-powershell).</span></span> <span data-ttu-id="1d718-440">Redigera det här skriptet toomeet ditt scenario är ett annat alternativ, men vi rekommenderar att du använder AzCopy eller PowerShell eftersom det är enklare och snabbare.</span><span class="sxs-lookup"><span data-stu-id="1d718-440">Editing this script toomeet your scenario is another choice, but we recommend using AzCopy or PowerShell since it is easier and faster.</span></span>

<span data-ttu-id="1d718-441">Hej automatiseringsskriptet finns nedan.</span><span class="sxs-lookup"><span data-stu-id="1d718-441">hello automation script is provided below.</span></span> <span data-ttu-id="1d718-442">Ersätt texten med din information och uppdaterar hello skriptet toomatch med din situation.</span><span class="sxs-lookup"><span data-stu-id="1d718-442">Replace text with your information and update hello script toomatch with your specific scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="1d718-443">Med hjälp av hello befintliga skript bevaras inte hello nätverkskonfigurationen för ditt Virtuella källdatorn.</span><span class="sxs-lookup"><span data-stu-id="1d718-443">Using hello existing script does not preserve hello network configuration of your source VM.</span></span> <span data-ttu-id="1d718-444">Du behöver toore-config hello nätverksinställningar på de migrerade virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="1d718-444">You will need toore-config hello networking settings on your migrated VMs.</span></span>
>
>

```
    <#
    .Synopsis
    This script is provided as an EXAMPLE tooshow how toomigrate a VM from a standard storage account tooa premium storage account. You can customize it according tooyour specific requirements.

    .Description
    hello script will copy hello vhds (page blobs) of hello source VM toohello new storage account.
    And then it will create a new VM from these copied vhds based on hello inputs that you specified for hello new VM.
    You can modify hello script toosatisfy your specific requirement, but please be aware of hello items specified
    in hello Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED toohello IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. hello ENTIRE RISK OF USE, INABILITY tooUSE, OR
    RESULTS FROM hello USE OF THIS CODE REMAINS WITH hello USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    toofind more information about how tooset up Azure PowerShell, refer toohello following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # hello cloud service name of hello VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # hello VM name toocopy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # hello destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # hello destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # hello destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # hello destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # hello location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not toocopy hello os disk, hello default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently tooreport hello copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # hello name suffix tooadd toonew created disks tooavoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import hello Azure PowerShell module
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


    #Check hello Azure PowerShell module version
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
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer toothis article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ tooconnect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if hello VM is shut down
    #  Stopping hello VM is a required step so that hello file system is consistent when you do hello copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - hello source VM doesn't exist in hello current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping hello VM is a required step so that hello file system is consistent when you do hello copy operation. Azure does not support live migration at this time. If you'd like toocreate a VM from a generalized image, sys-prep hello Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish toostop $SourceVMName now? Input 'N' if you want tooshut down hello VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until hello VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName tooshut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting hello sourve vm tooa configuration file, you can restore hello original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration too$vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy hello vhds of hello source vm
    #  You can choose toocopy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly toobe $false. hello default is toocopy only data disk vhds
    #  and hello new VM will boot from hello original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering hello data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in hello destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need toocopy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting hello source VM so that dest VM can boot
        # from hello same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose toocopy data disks only. Moving VM requires removing hello original VM (hello disks and backing vhd files will NOT be deleted) so that hello new VM can boot from hello same vhd. This is an irreversible action. Do you wish tooproceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing hello original VM (hello vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill hello OS disk is released by source VM. This may take up tooseveral minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy hello os disk vhd
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
        # update hello media link toopoint toohello target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy toocomplete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
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
    #  hello new VM can be created from hello copied disks or hello original os disk.
    #  You can ddd your own logic here toosatisfy your specific requirements of hello vm.
    #######################################################################

    # Create a VM from hello existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached hello copied data disks toohello new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of hello source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want tooadd more custimization toohello new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <span data-ttu-id="1d718-445"><a name="optimization"></a>Optimering</span><span class="sxs-lookup"><span data-stu-id="1d718-445"><a name="optimization"></a>Optimization</span></span>
<span data-ttu-id="1d718-446">Din aktuella VM-konfiguration kan anpassas specifikt toowork bra med standarddiskar.</span><span class="sxs-lookup"><span data-stu-id="1d718-446">Your current VM configuration may be customized specifically toowork well with Standard disks.</span></span> <span data-ttu-id="1d718-447">Till exempel tooincrease hello prestanda med hjälp av många diskar i en stripe-volym.</span><span class="sxs-lookup"><span data-stu-id="1d718-447">For instance, tooincrease hello performance by using many disks in a striped volume.</span></span> <span data-ttu-id="1d718-448">Till exempel i stället för att använda 4 diskar separat på Premium-lagring, du kan toooptimize hello kostnader genom att ha en enda disk.</span><span class="sxs-lookup"><span data-stu-id="1d718-448">For example, instead of using 4 disks separately on Premium Storage, you may be able toooptimize hello cost by having a single disk.</span></span> <span data-ttu-id="1d718-449">Optimeringar som den här behovet toobe hanteras på ett fall till fall och kräver anpassade åtgärder efter migreringen hello.</span><span class="sxs-lookup"><span data-stu-id="1d718-449">Optimizations like this need toobe handled on a case by case basis and require custom steps after hello migration.</span></span> <span data-ttu-id="1d718-450">Observera också att den här processen och inte fungerar för databaser och program som beror på hello disklayouten som definierats i hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="1d718-450">Also, note that this process may not well work for databases and applications that depend on hello disk layout defined in hello setup.</span></span>

##### <a name="preparation"></a><span data-ttu-id="1d718-451">Förberedelse</span><span class="sxs-lookup"><span data-stu-id="1d718-451">Preparation</span></span>
1. <span data-ttu-id="1d718-452">Fullständig hello enkla migrering, enligt beskrivningen i hello tidigare avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1d718-452">Complete hello Simple Migration as described in hello earlier section.</span></span> <span data-ttu-id="1d718-453">Optimeringar kommer att utföras på hello ny virtuell dator efter hello migreringen.</span><span class="sxs-lookup"><span data-stu-id="1d718-453">Optimizations will be performed on hello new VM after hello migration.</span></span>
2. <span data-ttu-id="1d718-454">Definiera hello nya diskstorlekar krävs för att konfigurera hello optimerad.</span><span class="sxs-lookup"><span data-stu-id="1d718-454">Define hello new disk sizes needed for hello optimized configuration.</span></span>
3. <span data-ttu-id="1d718-455">Fastställa mappningen av hello aktuella diskar/volymer toohello nya disk specifikationer.</span><span class="sxs-lookup"><span data-stu-id="1d718-455">Determine mapping of hello current disks/volumes toohello new disk specifications.</span></span>

##### <a name="execution-steps"></a><span data-ttu-id="1d718-456">Utförande</span><span class="sxs-lookup"><span data-stu-id="1d718-456">Execution steps</span></span>
1. <span data-ttu-id="1d718-457">Skapa hello nya diskar med hello rätt storlek på hello Premium Storage VM.</span><span class="sxs-lookup"><span data-stu-id="1d718-457">Create hello new disks with hello right sizes on hello Premium Storage VM.</span></span>
2. <span data-ttu-id="1d718-458">Inloggningen toohello virtuella datorn och kopiera hello data från hello aktuella volymen toohello ny disk som mappar toothat volym.</span><span class="sxs-lookup"><span data-stu-id="1d718-458">Login toohello VM and copy hello data from hello current volume toohello new disk that maps toothat volume.</span></span> <span data-ttu-id="1d718-459">Gör detta för alla aktuella hello-volymer som behöver toomap tooa ny disk.</span><span class="sxs-lookup"><span data-stu-id="1d718-459">Do this for all hello current volumes that need toomap tooa new disk.</span></span>
3. <span data-ttu-id="1d718-460">Sedan ändra hello programmet inställningar tooswitch toohello nya diskar och koppla från hello gamla volymer.</span><span class="sxs-lookup"><span data-stu-id="1d718-460">Next, change hello application settings tooswitch toohello new disks, and detach hello old volumes.</span></span>

<span data-ttu-id="1d718-461">Justera hello program för bättre diskprestanda finns för[optimera programprestanda](storage-premium-storage-performance.md#optimizing-application-performance).</span><span class="sxs-lookup"><span data-stu-id="1d718-461">For tuning hello application for better disk performance, please refer too[Optimizing Application Performance](storage-premium-storage-performance.md#optimizing-application-performance).</span></span>

### <a name="application-migrations"></a><span data-ttu-id="1d718-462">Migrering av programmet</span><span class="sxs-lookup"><span data-stu-id="1d718-462">Application migrations</span></span>
<span data-ttu-id="1d718-463">Databaser och andra komplexa program kan kräva särskilda åtgärder som definierats av hello programmets provider för hello migrering.</span><span class="sxs-lookup"><span data-stu-id="1d718-463">Databases and other complex applications may require special steps as defined by hello application provider for hello migration.</span></span> <span data-ttu-id="1d718-464">Se dokumentationen toorespective.</span><span class="sxs-lookup"><span data-stu-id="1d718-464">Please refer toorespective application documentation.</span></span> <span data-ttu-id="1d718-465">T.ex.</span><span class="sxs-lookup"><span data-stu-id="1d718-465">E.g.</span></span> <span data-ttu-id="1d718-466">vanligtvis databaser kan migreras med säkerhetskopiering och återställning.</span><span class="sxs-lookup"><span data-stu-id="1d718-466">typically databases can be migrated through backup and restore.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d718-467">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d718-467">Next steps</span></span>
<span data-ttu-id="1d718-468">Se hello följande resurser för specifika scenarier för att migrera virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="1d718-468">See hello following resources for specific scenarios for migrating virtual machines:</span></span>

* [<span data-ttu-id="1d718-469">Migrera Azure virtuella datorer mellan Storage-konton</span><span class="sxs-lookup"><span data-stu-id="1d718-469">Migrate Azure Virtual Machines between Storage Accounts</span></span>](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [<span data-ttu-id="1d718-470">Skapa och ladda upp en Windows Server VHD-tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1d718-470">Create and upload a Windows Server VHD tooAzure.</span></span>](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="1d718-471">Skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem</span><span class="sxs-lookup"><span data-stu-id="1d718-471">Creating and Uploading a Virtual Hard Disk that Contains hello Linux Operating System</span></span>](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="1d718-472">Migrering av virtuella datorer från Amazon AWS tooMicrosoft Azure</span><span class="sxs-lookup"><span data-stu-id="1d718-472">Migrating Virtual Machines from Amazon AWS tooMicrosoft Azure</span></span>](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

<span data-ttu-id="1d718-473">Se även hello följande resurser toolearn mer om Azure Storage- och virtuella datorer i Azure:</span><span class="sxs-lookup"><span data-stu-id="1d718-473">Also, see hello following resources toolearn more about Azure Storage and Azure Virtual Machines:</span></span>

* [<span data-ttu-id="1d718-474">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="1d718-474">Azure Storage</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="1d718-475">Virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="1d718-475">Azure Virtual Machines</span></span>](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [<span data-ttu-id="1d718-476">Premium Storage: Lagring med höga prestanda för Azure Virtual Machines-arbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="1d718-476">Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads</span></span>](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
