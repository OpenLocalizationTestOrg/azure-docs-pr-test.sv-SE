# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a><span data-ttu-id="abf63-101">Vanliga frågor och svar om Azure IaaS-VM och hanterade och ohanterade premiumdiskar</span><span class="sxs-lookup"><span data-stu-id="abf63-101">Frequently asked questions about Azure IaaS VM disks and managed and unmanaged premium disks</span></span>

<span data-ttu-id="abf63-102">Den här artikeln besvarar några vanliga frågor om Azure hanterade diskar och Azure Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="abf63-102">This article answers some frequently asked questions about Azure Managed Disks and Azure Premium Storage.</span></span>

## <a name="managed-disks"></a><span data-ttu-id="abf63-103">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="abf63-103">Managed Disks</span></span>

<span data-ttu-id="abf63-104">**Vad är Azure hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-104">**What is Azure Managed Disks?**</span></span>

<span data-ttu-id="abf63-105">Hanterade diskar är en funktion som förenklar Diskhantering för virtuella Azure IaaS-datorer genom att hantera lagringshantering konto för dig.</span><span class="sxs-lookup"><span data-stu-id="abf63-105">Managed Disks is a feature that simplifies disk management for Azure IaaS VMs by handling storage account management for you.</span></span> <span data-ttu-id="abf63-106">Mer information finns i [översikt för hanterade diskar](../articles/virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="abf63-106">For more information, see the [Managed Disks overview](../articles/virtual-machines/windows/managed-disks-overview.md).</span></span>

<span data-ttu-id="abf63-107">**Om jag skapa en standard hanterade diskar från en befintlig virtuell Hårddisk som är 80 GB, hur mycket som kostar mig?**</span><span class="sxs-lookup"><span data-stu-id="abf63-107">**If I create a standard managed disk from an existing VHD that's 80 GB, how much will that cost me?**</span></span>

<span data-ttu-id="abf63-108">En standard hanterade diskar som skapats från en virtuell Hårddisk på 80 GB behandlas som nästa tillgängliga standard diskens storlek, vilket är en S10 disk.</span><span class="sxs-lookup"><span data-stu-id="abf63-108">A standard managed disk created from an 80-GB VHD is treated as the next available standard disk size, which is an S10 disk.</span></span> <span data-ttu-id="abf63-109">Du är debiteras enligt S10 disk priser.</span><span class="sxs-lookup"><span data-stu-id="abf63-109">You're charged according to the S10 disk pricing.</span></span> <span data-ttu-id="abf63-110">Mer information finns på sidan med [priser](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="abf63-110">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="abf63-111">**Finns det några transaktionskostnader för hanterade standarddiskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-111">**Are there any transaction costs for standard managed disks?**</span></span>

<span data-ttu-id="abf63-112">Ja.</span><span class="sxs-lookup"><span data-stu-id="abf63-112">Yes.</span></span> <span data-ttu-id="abf63-113">Du är debiteras för varje transaktion.</span><span class="sxs-lookup"><span data-stu-id="abf63-113">You're charged for each transaction.</span></span> <span data-ttu-id="abf63-114">Mer information finns på sidan med [priser](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="abf63-114">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="abf63-115">**För en standard hanterade disk jag debiteras för den verkliga storleken hos data på disken eller diskens etablerad kapacitet?**</span><span class="sxs-lookup"><span data-stu-id="abf63-115">**For a standard managed disk, will I be charged for the actual size of the data on the disk or for the provisioned capacity of the disk?**</span></span>

<span data-ttu-id="abf63-116">Du är debiteras baserat på diskens etablerad kapacitet.</span><span class="sxs-lookup"><span data-stu-id="abf63-116">You're charged based on the provisioned capacity of the disk.</span></span> <span data-ttu-id="abf63-117">Mer information finns på sidan med [priser](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="abf63-117">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="abf63-118">**Hur är prissättningen av hanterade premiumdiskar skiljer sig från ohanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-118">**How is pricing of premium managed disks different from unmanaged disks?**</span></span>

<span data-ttu-id="abf63-119">Priser för premiumdiskar hanteras är samma som ohanterad premiumdiskar.</span><span class="sxs-lookup"><span data-stu-id="abf63-119">The pricing of premium managed disks is the same as unmanaged premium disks.</span></span>

<span data-ttu-id="abf63-120">**Kan jag ändra lagringskontotypen (Standard eller Premium) av min hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-120">**Can I change the storage account type (Standard or Premium) of my managed disks?**</span></span>

<span data-ttu-id="abf63-121">Ja.</span><span class="sxs-lookup"><span data-stu-id="abf63-121">Yes.</span></span> <span data-ttu-id="abf63-122">Du kan ändra lagringskontotypen hanterade diskar med hjälp av Azure portal, PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="abf63-122">You can change the storage account type of your managed disks by using the Azure portal, PowerShell, or the Azure CLI.</span></span>

<span data-ttu-id="abf63-123">**Finns det ett sätt att jag kan kopiera eller exportera hanterade diskar till ett privat storage-konto?**</span><span class="sxs-lookup"><span data-stu-id="abf63-123">**Is there a way that I can copy or export a managed disk to a private storage account?**</span></span>

<span data-ttu-id="abf63-124">Ja.</span><span class="sxs-lookup"><span data-stu-id="abf63-124">Yes.</span></span> <span data-ttu-id="abf63-125">Du kan exportera dina hanterade diskar med hjälp av Azure portal, PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="abf63-125">You can export your managed disks by using the Azure portal, PowerShell, or the Azure CLI.</span></span>

<span data-ttu-id="abf63-126">**Kan jag använda en VHD-fil i ett Azure storage-konto för att skapa en hanterad disk med en annan prenumeration?**</span><span class="sxs-lookup"><span data-stu-id="abf63-126">**Can I use a VHD file in an Azure storage account to create a managed disk with a different subscription?**</span></span>

<span data-ttu-id="abf63-127">Nej.</span><span class="sxs-lookup"><span data-stu-id="abf63-127">No.</span></span>

<span data-ttu-id="abf63-128">**Kan jag använda en VHD-fil i ett Azure storage-konto för att skapa en hanterad disk i en annan region?**</span><span class="sxs-lookup"><span data-stu-id="abf63-128">**Can I use a VHD file in an Azure storage account to create a managed disk in a different region?**</span></span>

<span data-ttu-id="abf63-129">Nej.</span><span class="sxs-lookup"><span data-stu-id="abf63-129">No.</span></span>

<span data-ttu-id="abf63-130">**Finns det några begränsningar för skalan för kunder som använder hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-130">**Are there any scale limitations for customers that use managed disks?**</span></span>

<span data-ttu-id="abf63-131">Hanterade diskar eliminerar de gränser som är kopplade till storage-konton.</span><span class="sxs-lookup"><span data-stu-id="abf63-131">Managed Disks eliminates the limits associated with storage accounts.</span></span> <span data-ttu-id="abf63-132">Antalet hanterade diskar per prenumeration är dock begränsad till 2 000 som standard.</span><span class="sxs-lookup"><span data-stu-id="abf63-132">However, the number of managed disks per subscription is limited to 2,000 by default.</span></span> <span data-ttu-id="abf63-133">Du kan kontakta support om du vill öka antalet.</span><span class="sxs-lookup"><span data-stu-id="abf63-133">You can call support to increase this number.</span></span>

<span data-ttu-id="abf63-134">**Kan jag göra en inkrementell ögonblicksbild av hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-134">**Can I take an incremental snapshot of a managed disk?**</span></span>

<span data-ttu-id="abf63-135">Nej.</span><span class="sxs-lookup"><span data-stu-id="abf63-135">No.</span></span> <span data-ttu-id="abf63-136">Den aktuella kapaciteten för ögonblicksbild gör en fullständig kopia av hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="abf63-136">The current snapshot capability makes a full copy of a managed disk.</span></span> <span data-ttu-id="abf63-137">Men planerar vi att stödja inkrementell ögonblicksbilder i framtiden.</span><span class="sxs-lookup"><span data-stu-id="abf63-137">However, we are planning to support incremental snapshots in the future.</span></span>

<span data-ttu-id="abf63-138">**Virtuella datorer i en tillgänglighetsuppsättning kan bestå av en kombination av hanterade och ohanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-138">**Can VMs in an availability set consist of a combination of managed and unmanaged disks?**</span></span>

<span data-ttu-id="abf63-139">Nej.</span><span class="sxs-lookup"><span data-stu-id="abf63-139">No.</span></span> <span data-ttu-id="abf63-140">De virtuella datorerna i en tillgänglighetsuppsättning måste använda alla hanterade diskar eller ohanterad diskarna.</span><span class="sxs-lookup"><span data-stu-id="abf63-140">The VMs in an availability set must use either all managed disks or all unmanaged disks.</span></span> <span data-ttu-id="abf63-141">När du skapar en tillgänglighetsuppsättning, kan du välja vilken typ av diskar som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="abf63-141">When you create an availability set, you can choose which type of disks you want to use.</span></span>

<span data-ttu-id="abf63-142">**Är standardalternativet i Azure-portalen för hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-142">**Is Managed Disks the default option in the Azure portal?**</span></span>

<span data-ttu-id="abf63-143">För närvarande inte, men det blir standardvärdet i framtiden.</span><span class="sxs-lookup"><span data-stu-id="abf63-143">Not currently, but it will become the default in the future.</span></span>

<span data-ttu-id="abf63-144">**Kan jag skapa en tom disk som hanterade?**</span><span class="sxs-lookup"><span data-stu-id="abf63-144">**Can I create an empty managed disk?**</span></span>

<span data-ttu-id="abf63-145">Ja.</span><span class="sxs-lookup"><span data-stu-id="abf63-145">Yes.</span></span> <span data-ttu-id="abf63-146">Du kan skapa en tom disk.</span><span class="sxs-lookup"><span data-stu-id="abf63-146">You can create an empty disk.</span></span> <span data-ttu-id="abf63-147">Hanterade diskar kan skapas oberoende av en virtuell dator, till exempel utan kopplar den till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="abf63-147">A managed disk can be created independently of a VM, for example, without attaching it to a VM.</span></span>

<span data-ttu-id="abf63-148">**Vad stöds feldomänsantalet för tillgänglighet anges som använder hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-148">**What is the supported fault domain count for an availability set that uses Managed Disks?**</span></span>

<span data-ttu-id="abf63-149">Beroende på den region där den tillgänglighetsuppsättningen som använder hanterade diskar finns, är stöds feldomänsantalet 2 eller 3.</span><span class="sxs-lookup"><span data-stu-id="abf63-149">Depending on the region where the availability set that uses Managed Disks is located, the supported fault domain count is 2 or 3.</span></span>

<span data-ttu-id="abf63-150">**Hur är standard lagringskontot för diagnostik konfigurera?**</span><span class="sxs-lookup"><span data-stu-id="abf63-150">**How is the standard storage account for diagnostics set up?**</span></span>

<span data-ttu-id="abf63-151">Du kan konfigurera ett konto för privat lagring för diagnostik för Virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="abf63-151">You set up a private storage account for VM diagnostics.</span></span> <span data-ttu-id="abf63-152">I framtiden kommer planerar vi att växla diagnostik samt till hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="abf63-152">In the future, we plan to switch diagnostics to Managed Disks as well.</span></span>

<span data-ttu-id="abf63-153">**Vilken typ av rollbaserad åtkomstkontroll support är tillgänglig för hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-153">**What kind of Role-Based Access Control support is available for Managed Disks?**</span></span>

<span data-ttu-id="abf63-154">Hanterade diskar stöder tre viktiga standardroller:</span><span class="sxs-lookup"><span data-stu-id="abf63-154">Managed Disks supports three key default roles:</span></span>

* <span data-ttu-id="abf63-155">Ägare: Kan hantera allt, inklusive åtkomst</span><span class="sxs-lookup"><span data-stu-id="abf63-155">Owner: Can manage everything, including access</span></span>
* <span data-ttu-id="abf63-156">Deltagare: Kan hantera allt utom åtkomst</span><span class="sxs-lookup"><span data-stu-id="abf63-156">Contributor: Can manage everything except access</span></span>
* <span data-ttu-id="abf63-157">Läsare: Kan visa allt, men det går inte att göra ändringar</span><span class="sxs-lookup"><span data-stu-id="abf63-157">Reader: Can view everything, but can't make changes</span></span>

<span data-ttu-id="abf63-158">**Finns det ett sätt att jag kan kopiera eller exportera hanterade diskar till ett privat storage-konto?**</span><span class="sxs-lookup"><span data-stu-id="abf63-158">**Is there a way that I can copy or export a managed disk to a private storage account?**</span></span>

<span data-ttu-id="abf63-159">Du kan hämta en skrivskyddad delad åtkomstsignatur URI för hanterade diskar och använda den för att kopiera innehållet till en privat lagring konto eller lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="abf63-159">You can get a read-only shared access signature URI for the managed disk and use it to copy the contents to a private storage account or on-premises storage.</span></span>

<span data-ttu-id="abf63-160">**Kan jag skapa en kopia av min hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-160">**Can I create a copy of my managed disk?**</span></span>

<span data-ttu-id="abf63-161">Kunder kan ta en ögonblicksbild av deras hanterade diskar och sedan använda ögonblicksbilden för att skapa en annan hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="abf63-161">Customers can take a snapshot of their managed disks and then use the snapshot to create another managed disk.</span></span>

<span data-ttu-id="abf63-162">**Ohanterad diskar stöds fortfarande?**</span><span class="sxs-lookup"><span data-stu-id="abf63-162">**Are unmanaged disks still supported?**</span></span>

<span data-ttu-id="abf63-163">Ja.</span><span class="sxs-lookup"><span data-stu-id="abf63-163">Yes.</span></span> <span data-ttu-id="abf63-164">Vi stöder ohanterade och hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="abf63-164">We support unmanaged and managed disks.</span></span> <span data-ttu-id="abf63-165">Vi rekommenderar att du använder hanterade diskar för nya arbetsbelastningar och migrera dina aktuella arbetsbelastningar till hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="abf63-165">We recommend that you use managed disks for new workloads and migrate your current workloads to managed disks.</span></span>


<span data-ttu-id="abf63-166">**Om jag skapa en 128 GB disk och sedan öka storleken till 130 GB jag debiteras för nästa diskens storlek (512 GB)?**</span><span class="sxs-lookup"><span data-stu-id="abf63-166">**If I create a 128-GB disk and then increase the size to 130 GB, will I be charged for the next disk size (512 GB)?**</span></span>

<span data-ttu-id="abf63-167">Ja.</span><span class="sxs-lookup"><span data-stu-id="abf63-167">Yes.</span></span>

<span data-ttu-id="abf63-168">**Kan jag skapa lokalt redundant lagring, geo-redundant lagring och zonredundant lagring hanteras diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-168">**Can I create locally redundant storage, geo-redundant storage, and zone-redundant storage managed disks?**</span></span>

<span data-ttu-id="abf63-169">Azure-hanterade diskar stöder för närvarande endast lokalt redundant lagring hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="abf63-169">Azure Managed Disks currently supports only locally redundant storage managed disks.</span></span>

<span data-ttu-id="abf63-170">**Kan jag krympa eller downsize min hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-170">**Can I shrink or downsize my managed disks?**</span></span>

<span data-ttu-id="abf63-171">Nej.</span><span class="sxs-lookup"><span data-stu-id="abf63-171">No.</span></span> <span data-ttu-id="abf63-172">Den här funktionen stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="abf63-172">This feature is not supported currently.</span></span> 

<span data-ttu-id="abf63-173">**Kan jag ändra egenskapen name för datorn när en särskild (inte skapats med hjälp av systemförberedelseverktyget eller generaliserad) operativsystemdisk används för att etablera en virtuell dator?**</span><span class="sxs-lookup"><span data-stu-id="abf63-173">**Can I change the computer name property when a specialized (not created by using the System Preparation tool or generalized) operating system disk is used to provision a VM?**</span></span>

<span data-ttu-id="abf63-174">Nej.</span><span class="sxs-lookup"><span data-stu-id="abf63-174">No.</span></span> <span data-ttu-id="abf63-175">Du kan inte uppdatera egenskapen name för datorn.</span><span class="sxs-lookup"><span data-stu-id="abf63-175">You can't update the computer name property.</span></span> <span data-ttu-id="abf63-176">Den nya virtuella datorn ärver den från överordnat virtuella datorn som användes för att skapa operativsystemets disk.</span><span class="sxs-lookup"><span data-stu-id="abf63-176">The new VM inherits it from the parent VM, which was used to create the operating system disk.</span></span> 

<span data-ttu-id="abf63-177">**Var hittar jag exempel Azure Resource Manager-mallar för att skapa virtuella datorer med hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-177">**Where can I find sample Azure Resource Manager templates to create VMs with managed disks?**</span></span>
* [<span data-ttu-id="abf63-178">Lista över mallar med hjälp av hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="abf63-178">List of templates using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="abf63-179">https://github.com/chagarw/MDPP</span><span class="sxs-lookup"><span data-stu-id="abf63-179">https://github.com/chagarw/MDPP</span></span>

## <a name="managed-disks-and-storage-service-encryption"></a><span data-ttu-id="abf63-180">Hanterade diskar och Storage Service-kryptering</span><span class="sxs-lookup"><span data-stu-id="abf63-180">Managed Disks and Storage Service Encryption</span></span> 

<span data-ttu-id="abf63-181">**Är Azure Storage Service-kryptering aktiverat som standard när jag skapar hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-181">**Is Azure Storage Service Encryption enabled by default when I create a managed disk?**</span></span>

<span data-ttu-id="abf63-182">Ja.</span><span class="sxs-lookup"><span data-stu-id="abf63-182">Yes.</span></span>

<span data-ttu-id="abf63-183">**Som hanterar krypteringsnycklarna?**</span><span class="sxs-lookup"><span data-stu-id="abf63-183">**Who manages the encryption keys?**</span></span>

<span data-ttu-id="abf63-184">Microsoft hanterar krypteringsnycklarna.</span><span class="sxs-lookup"><span data-stu-id="abf63-184">Microsoft manages the encryption keys.</span></span>

<span data-ttu-id="abf63-185">**Kan jag inaktivera Storage Service-kryptering för min hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-185">**Can I disable Storage Service Encryption for my managed disks?**</span></span>

<span data-ttu-id="abf63-186">Nej.</span><span class="sxs-lookup"><span data-stu-id="abf63-186">No.</span></span>

<span data-ttu-id="abf63-187">**Är Lagringstjänstens kryptering endast tillgänglig i vissa områden?**</span><span class="sxs-lookup"><span data-stu-id="abf63-187">**Is Storage Service Encryption only available in specific regions?**</span></span>

<span data-ttu-id="abf63-188">Nej.</span><span class="sxs-lookup"><span data-stu-id="abf63-188">No.</span></span> <span data-ttu-id="abf63-189">Den är tillgänglig i alla regioner där hanterade diskar är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="abf63-189">It's available in all the regions where Managed Disks is available.</span></span> <span data-ttu-id="abf63-190">Hanterade diskar är tillgänglig i alla offentliga regioner och Tyskland.</span><span class="sxs-lookup"><span data-stu-id="abf63-190">Managed Disks is available in all public regions and Germany.</span></span>

<span data-ttu-id="abf63-191">**Hur kan jag ta reda om hanterade disken krypteras?**</span><span class="sxs-lookup"><span data-stu-id="abf63-191">**How can I find out if my managed disk is encrypted?**</span></span>

<span data-ttu-id="abf63-192">Du hittar den tid då en hanterad disk skapades från Azure-portalen, Azure CLI och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="abf63-192">You can find out the time when a managed disk was created from the Azure portal, the Azure CLI, and PowerShell.</span></span> <span data-ttu-id="abf63-193">Om tiden efter den 9 juni 2017 krypteras disken.</span><span class="sxs-lookup"><span data-stu-id="abf63-193">If the time is after June 9, 2017, then your disk is encrypted.</span></span> 

<span data-ttu-id="abf63-194">**Hur kan jag kryptera Mina befintliga diskar som skapades före 10 juni 2017?**</span><span class="sxs-lookup"><span data-stu-id="abf63-194">**How can I encrypt my existing disks that were created before June 10, 2017?**</span></span>

<span data-ttu-id="abf63-195">Från och med 10 juni 2017 krypteras automatiskt nya data skrivs till befintliga hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="abf63-195">As of June 10, 2017, new data written to existing managed disks is automatically encrypted.</span></span> <span data-ttu-id="abf63-196">Vi också planerar att kryptera befintliga data och kryptering sker asynkront i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="abf63-196">We are also planning to encrypt existing data, and the encryption will happen asynchronously in the background.</span></span> <span data-ttu-id="abf63-197">Om du måste nu krypterar befintliga data måste du skapa en kopia av disken.</span><span class="sxs-lookup"><span data-stu-id="abf63-197">If you must encrypt existing data now, create a copy of your disk.</span></span> <span data-ttu-id="abf63-198">Nya diskar krypteras.</span><span class="sxs-lookup"><span data-stu-id="abf63-198">New disks will be encrypted.</span></span>

* [<span data-ttu-id="abf63-199">Kopiera hanterade diskar med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="abf63-199">Copy managed disks by using the Azure CLI</span></span>](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)
* [<span data-ttu-id="abf63-200">Kopiera hanterade diskar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="abf63-200">Copy managed disks by using PowerShell</span></span>](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="abf63-201">**Är hanterad ögonblicksbilder och bilder som krypteras?**</span><span class="sxs-lookup"><span data-stu-id="abf63-201">**Are managed snapshots and images encrypted?**</span></span>

<span data-ttu-id="abf63-202">Ja.</span><span class="sxs-lookup"><span data-stu-id="abf63-202">Yes.</span></span> <span data-ttu-id="abf63-203">Alla hanterade ögonblicksbilder och bilder som skapats efter den 9 juni 2017 krypteras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="abf63-203">All managed snapshots and images created after June 9, 2017, are automatically encrypted.</span></span> 

<span data-ttu-id="abf63-204">**Kan jag konvertera virtuella datorer med ohanterad diskar som finns på lagringskonton som är eller krypterats till hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-204">**Can I convert VMs with unmanaged disks that are located on storage accounts that are or were previously encrypted to managed disks?**</span></span>

<span data-ttu-id="abf63-205">Ja</span><span class="sxs-lookup"><span data-stu-id="abf63-205">Yes</span></span>

<span data-ttu-id="abf63-206">**En exporterad VHD från en hanterad disk eller en ögonblicksbild även krypteras?**</span><span class="sxs-lookup"><span data-stu-id="abf63-206">**Will an exported VHD from a managed disk or a snapshot also be encrypted?**</span></span>

<span data-ttu-id="abf63-207">Nej.</span><span class="sxs-lookup"><span data-stu-id="abf63-207">No.</span></span> <span data-ttu-id="abf63-208">Men om du exporterar en virtuell Hårddisk till en krypterad storage-konto från ett krypterat hanteras disk eller en ögonblicksbild och är krypterad.</span><span class="sxs-lookup"><span data-stu-id="abf63-208">But if you export a VHD to an encrypted storage account from an encrypted managed disk or snapshot, then it's encrypted.</span></span> 

## <a name="premium-disks-managed-and-unmanaged"></a><span data-ttu-id="abf63-209">Premiumdiskar: hanterade och ohanterade</span><span class="sxs-lookup"><span data-stu-id="abf63-209">Premium disks: Managed and unmanaged</span></span>

<span data-ttu-id="abf63-210">**Om en virtuell dator använder en serie med storlek som har stöd för Premium-lagring, till exempel en DSv2 kan jag koppla premium- och datadiskar som standard?**</span><span class="sxs-lookup"><span data-stu-id="abf63-210">**If a VM uses a size series that supports Premium Storage, such as a DSv2, can I attach both premium and standard data disks?**</span></span> 

<span data-ttu-id="abf63-211">Ja.</span><span class="sxs-lookup"><span data-stu-id="abf63-211">Yes.</span></span>

<span data-ttu-id="abf63-212">**Kan jag koppla premium- och datadiskar som standard till en rad med storleken som inte stöder Premium-lagring, till exempel D, Dv2, G eller F serien?**</span><span class="sxs-lookup"><span data-stu-id="abf63-212">**Can I attach both premium and standard data disks to a size series that doesn't support Premium Storage, such as D, Dv2, G, or F series?**</span></span>

<span data-ttu-id="abf63-213">Nej.</span><span class="sxs-lookup"><span data-stu-id="abf63-213">No.</span></span> <span data-ttu-id="abf63-214">Du kan koppla endast standard datadiskar till virtuella datorer som inte använder en serie med storlek som har stöd för Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="abf63-214">You can attach only standard data disks to VMs that don't use a size series that supports Premium Storage.</span></span>

<span data-ttu-id="abf63-215">**Om jag skapa en disk för premium-data från en befintlig virtuell Hårddisk som är 80 GB, hur mycket som kostar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-215">**If I create a premium data disk from an existing VHD that was 80 GB, how much will that cost?**</span></span>

<span data-ttu-id="abf63-216">En disk för premium-data som skapas från en virtuell Hårddisk på 80 GB behandlas som nästa tillgängliga premium diskens storlek är en P10 disk.</span><span class="sxs-lookup"><span data-stu-id="abf63-216">A premium data disk created from an 80-GB VHD is treated as the next-available premium disk size, which is a P10 disk.</span></span> <span data-ttu-id="abf63-217">Du är debiteras enligt P10 disk priser.</span><span class="sxs-lookup"><span data-stu-id="abf63-217">You're charged according to the P10 disk pricing.</span></span>

<span data-ttu-id="abf63-218">**Finns det transaktionskostnader använda Premium Storage?**</span><span class="sxs-lookup"><span data-stu-id="abf63-218">**Are there transaction costs to use Premium Storage?**</span></span>

<span data-ttu-id="abf63-219">Det finns en fast kostnad för varje diskstorlek som hämtas etablerade med specifika gränser på IOPS och genomflöde.</span><span class="sxs-lookup"><span data-stu-id="abf63-219">There is a fixed cost for each disk size, which comes provisioned with specific limits on IOPS and throughput.</span></span> <span data-ttu-id="abf63-220">Övriga kostnader är utgående bandbredd och ögonblicksbild kapacitet, om tillämpligt.</span><span class="sxs-lookup"><span data-stu-id="abf63-220">The other costs are outbound bandwidth and snapshot capacity, if applicable.</span></span> <span data-ttu-id="abf63-221">Mer information finns på sidan med [priser](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="abf63-221">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="abf63-222">**Vad är gränser för IOPS och dataflöde som jag kan från diskcache?**</span><span class="sxs-lookup"><span data-stu-id="abf63-222">**What are the limits for IOPS and throughput that I can get from the disk cache?**</span></span>

<span data-ttu-id="abf63-223">De kombinerade gränsvärdena för cache och lokala SSD för DS-serien är 4 000 IOPS per kärna och 33 MB per sekund per kärna.</span><span class="sxs-lookup"><span data-stu-id="abf63-223">The combined limits for cache and local SSD for a DS series are 4,000 IOPS per core and 33 MB per second per core.</span></span> <span data-ttu-id="abf63-224">GS-serien ger 5 000 IOPS per kärna och 50 MB per sekund per kärna.</span><span class="sxs-lookup"><span data-stu-id="abf63-224">The GS series offers 5,000 IOPS per core and 50 MB per second per core.</span></span>

<span data-ttu-id="abf63-225">**Stöds lokal SSD för en virtuell för hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-225">**Is the local SSD supported for a Managed Disks VM?**</span></span>

<span data-ttu-id="abf63-226">Lokal SSD är tillfälligt lagringsutrymme som ingår i en hanterad diskar i virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="abf63-226">The local SSD is temporary storage that is included with a Managed Disks VM.</span></span> <span data-ttu-id="abf63-227">Det finns inget extra kostnad för den här tillfällig lagring.</span><span class="sxs-lookup"><span data-stu-id="abf63-227">There is no extra cost for this temporary storage.</span></span> <span data-ttu-id="abf63-228">Vi rekommenderar att du inte använder den här lokala SSD för att lagra dina programdata eftersom det inte finns kvar i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="abf63-228">We recommend that you do not use this local SSD to store your application data because it isn't persisted in Azure Blob storage.</span></span>

<span data-ttu-id="abf63-229">**Finns det några konsekvenser för användning av TRIMNING på premiumdiskar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-229">**Are there any repercussions for the use of TRIM on premium disks?**</span></span>

<span data-ttu-id="abf63-230">Det finns inga Nackdelen med att användningen av TRIMNING på Azure-diskar på antingen premium eller standarddiskar.</span><span class="sxs-lookup"><span data-stu-id="abf63-230">There is no downside to the use of TRIM on Azure disks on either premium or standard disks.</span></span>

## <a name="new-disk-sizes-managed-and-unmanaged"></a><span data-ttu-id="abf63-231">Nya diskstorlekar: hanterade och ohanterade</span><span class="sxs-lookup"><span data-stu-id="abf63-231">New disk sizes: Managed and unmanaged</span></span>

<span data-ttu-id="abf63-232">**Vad är den största diskstorleken användas för operativsystemet och datadiskarna?**</span><span class="sxs-lookup"><span data-stu-id="abf63-232">**What is the largest disk size supported for operating system and data disks?**</span></span>

<span data-ttu-id="abf63-233">Den partitionstypen som Azure har stöd för en operativsystemdisk är master boot record (MBR).</span><span class="sxs-lookup"><span data-stu-id="abf63-233">The partition type that Azure supports for an operating system disk is the master boot record (MBR).</span></span> <span data-ttu-id="abf63-234">MBR-formatet stöder en disk ändra storlek på upp till 2 TB.</span><span class="sxs-lookup"><span data-stu-id="abf63-234">The MBR format supports a disk size up to 2 TB.</span></span> <span data-ttu-id="abf63-235">Den största storlek som Azure har stöd för en operativsystemdisk är 2 TB.</span><span class="sxs-lookup"><span data-stu-id="abf63-235">The largest size that Azure supports for an operating system disk is 2 TB.</span></span> <span data-ttu-id="abf63-236">Azure har stöd för upp till 4 TB för datadiskar.</span><span class="sxs-lookup"><span data-stu-id="abf63-236">Azure supports up to 4 TB for data disks.</span></span> 

<span data-ttu-id="abf63-237">**Vad är den största blob sidstorlek som stöds?**</span><span class="sxs-lookup"><span data-stu-id="abf63-237">**What is the largest page blob size that's supported?**</span></span>

<span data-ttu-id="abf63-238">Den största blob sidstorlek som har stöd för Azure är 8 TB (8191 GB).</span><span class="sxs-lookup"><span data-stu-id="abf63-238">The largest page blob size that Azure supports is 8 TB (8,191 GB).</span></span> <span data-ttu-id="abf63-239">Vi stöder inte sidblobbar som är större än 4 TB (4,095 GB) ansluten till en virtuell dator som data eller operativsystemet diskar.</span><span class="sxs-lookup"><span data-stu-id="abf63-239">We don't support page blobs larger than 4 TB (4,095 GB) attached to a VM as data or operating system disks.</span></span>

<span data-ttu-id="abf63-240">**Måste jag använda en ny version av Azure-verktyg för att skapa, bifoga, ändra storlek och överför diskar som är större än 1 TB?**</span><span class="sxs-lookup"><span data-stu-id="abf63-240">**Do I need to use a new version of Azure tools to create, attach, resize, and upload disks larger than 1 TB?**</span></span>

<span data-ttu-id="abf63-241">Du behöver inte uppgradera din befintliga Azure-verktyg för att skapa, koppla eller ändra storlek på diskar som är större än 1 TB.</span><span class="sxs-lookup"><span data-stu-id="abf63-241">You don't need to upgrade your existing Azure tools to create, attach, or resize disks larger than 1 TB.</span></span> <span data-ttu-id="abf63-242">Om du vill överföra VHD-filen från lokala direkt till Azure som en sidblob eller ohanterad disk måste du använda senaste verktyget:</span><span class="sxs-lookup"><span data-stu-id="abf63-242">To upload your VHD file from on-premises directly to Azure as a page blob or unmanaged disk, you need to use the latest tool sets:</span></span>

|<span data-ttu-id="abf63-243">Azure-verktyg</span><span class="sxs-lookup"><span data-stu-id="abf63-243">Azure tools</span></span>      | <span data-ttu-id="abf63-244">Versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="abf63-244">Supported versions</span></span>                                |
|-----------------|---------------------------------------------------|
|<span data-ttu-id="abf63-245">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="abf63-245">Azure PowerShell</span></span> | <span data-ttu-id="abf63-246">Versionsnumret 4.1.0: juni 2017 släpper eller senare</span><span class="sxs-lookup"><span data-stu-id="abf63-246">Version number 4.1.0: June 2017 release or later</span></span>|
|<span data-ttu-id="abf63-247">Azure CLI v1</span><span class="sxs-lookup"><span data-stu-id="abf63-247">Azure CLI v1</span></span>     | <span data-ttu-id="abf63-248">Versionsnumret 0.10.13: släpp kan 2017 eller senare</span><span class="sxs-lookup"><span data-stu-id="abf63-248">Version number 0.10.13: May 2017 release or later</span></span>|
|<span data-ttu-id="abf63-249">AzCopy</span><span class="sxs-lookup"><span data-stu-id="abf63-249">AzCopy</span></span>           | <span data-ttu-id="abf63-250">Versionsnumret 6.1.0: juni 2017 släpper eller senare</span><span class="sxs-lookup"><span data-stu-id="abf63-250">Version number 6.1.0: June 2017 release or later</span></span>|

<span data-ttu-id="abf63-251">Stöd för Azure CLI v2 och Azure Lagringsutforskaren kommer snart.</span><span class="sxs-lookup"><span data-stu-id="abf63-251">The support for Azure CLI v2 and Azure Storage Explorer is coming soon.</span></span> 

<span data-ttu-id="abf63-252">**Stöds P4 och P6 diskstorlekar för ohanterade diskar eller sidblobbar?**</span><span class="sxs-lookup"><span data-stu-id="abf63-252">**Are P4 and P6 disk sizes supported for unmanaged disks or page blobs?**</span></span>

<span data-ttu-id="abf63-253">Nej.</span><span class="sxs-lookup"><span data-stu-id="abf63-253">No.</span></span> <span data-ttu-id="abf63-254">P4 (32 GB) och P6 diskstorlekar (64 GB) stöds endast för hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="abf63-254">P4 (32 GB) and P6 (64 GB) disk sizes are supported only for managed disks.</span></span> <span data-ttu-id="abf63-255">Stöd för ohanterade diskar och sidblobbar kommer snart.</span><span class="sxs-lookup"><span data-stu-id="abf63-255">Support for unmanaged disks and page blobs is coming soon.</span></span>

<span data-ttu-id="abf63-256">**Om min befintliga premium hanteras disk mindre än 64 GB skapades innan liten disk har aktiverats (runt den 15 juni 2017), hur den faktureras?**</span><span class="sxs-lookup"><span data-stu-id="abf63-256">**If my existing premium managed disk less than 64 GB was created before the small disk was enabled (around June 15, 2017), how is it billed?**</span></span>

<span data-ttu-id="abf63-257">Befintliga små premium diskar mindre än 64 GB fortsätta debiteras enligt P10 prisnivå.</span><span class="sxs-lookup"><span data-stu-id="abf63-257">Existing small premium disks less than 64 GB continue to be billed according to the P10 pricing tier.</span></span> 

<span data-ttu-id="abf63-258">**Hur kan jag byta disk nivån av små premiumdiskar som är mindre än 64 GB från P10 P4 eller P6?**</span><span class="sxs-lookup"><span data-stu-id="abf63-258">**How can I switch the disk tier of small premium disks less than 64 GB from P10 to P4 or P6?**</span></span>

<span data-ttu-id="abf63-259">Du kan ta en ögonblicksbild av små diskar och sedan skapa en disk för att automatiskt växla prisnivån till P4 eller P6 baserat på etablerade storlek.</span><span class="sxs-lookup"><span data-stu-id="abf63-259">You can take a snapshot of your small disks and then create a disk to automatically switch the pricing tier to P4 or P6 based on the provisioned size.</span></span> 


## <a name="what-if-my-question-isnt-answered-here"></a><span data-ttu-id="abf63-260">Vad gör jag om min fråga inte besvaras här?</span><span class="sxs-lookup"><span data-stu-id="abf63-260">What if my question isn't answered here?</span></span>

<span data-ttu-id="abf63-261">Om din fråga inte finns med här kan för oss berätta och vi hjälper dig att hitta ett svar.</span><span class="sxs-lookup"><span data-stu-id="abf63-261">If your question isn't listed here, let us know and we'll help you find an answer.</span></span> <span data-ttu-id="abf63-262">Du kan ställa en fråga i slutet av den här artikeln i kommentarerna.</span><span class="sxs-lookup"><span data-stu-id="abf63-262">You can post a question at the end of this article in the comments.</span></span> <span data-ttu-id="abf63-263">Om du vill interagera med Azure Storage-teamet och andra gruppmedlemmar om den här artikeln använder MSDN [Azure Storage-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span><span class="sxs-lookup"><span data-stu-id="abf63-263">To engage with the Azure Storage team and other community members about this article, use the MSDN [Azure Storage forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span></span>

<span data-ttu-id="abf63-264">Skicka din begäran och idéer om du vill begära funktioner i [Azure Storage Feedbackforum](https://feedback.azure.com/forums/217298-storage).</span><span class="sxs-lookup"><span data-stu-id="abf63-264">To request features, submit your requests and ideas to the [Azure Storage feedback forum](https://feedback.azure.com/forums/217298-storage).</span></span>
