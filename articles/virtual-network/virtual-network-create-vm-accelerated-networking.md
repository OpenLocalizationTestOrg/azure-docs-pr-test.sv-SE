---
title: "aaaCreate en virtuell Azure-dator med snabbare nätverk | Microsoft Docs"
description: "Lär dig hur toocreate en virtuell dator med snabbare nätverk."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 241362891bfe083ab32c2f558be54f63f87c0693
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-with-accelerated-networking"></a><span data-ttu-id="7a7b1-103">Skapa en virtuell dator med snabbare nätverk</span><span class="sxs-lookup"><span data-stu-id="7a7b1-103">Create a virtual machine with Accelerated Networking</span></span>

<span data-ttu-id="7a7b1-104">I kursen får du lära dig hur toocreate ett Azure Virtual Machine (virtuell dator) med snabbare nätverk.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-104">In this tutorial, you learn how toocreate an Azure Virtual Machine (VM) with Accelerated Networking.</span></span> <span data-ttu-id="7a7b1-105">Snabbare nätverksfunktioner är GA för Windows och en offentlig förhandsgranskning för specifika Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-105">Accelerated Networking is GA for Windows and in a Public Preview for specific Linux distributions.</span></span> <span data-ttu-id="7a7b1-106">Snabbare nätverk gör det möjligt för enskild rot i/o-virtualisering (SR-IOV) tooa VM, vilket avsevärt minskar tiden dess nätverksprestanda.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-106">Accelerated networking enables single root I/O virtualization (SR-IOV) tooa VM, greatly improving its networking performance.</span></span> <span data-ttu-id="7a7b1-107">Den här sökvägen för högpresterande kringgår hello värden från hello datapath minskar latens och jitter CPU-belastningen för användning med hello mest krävande nätverksbelastning på VM-typer som stöds.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-107">This high-performance path bypasses hello host from hello datapath reducing latency, jitter, and CPU utilization, for use with hello most demanding network workloads on supported VM types.</span></span> <span data-ttu-id="7a7b1-108">Följande bild visar kommunikation mellan två virtuella datorer (VM) med och utan snabbare nätverksfunktioner hello:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-108">hello following picture shows communication between two virtual machines (VM) with and without accelerated networking:</span></span>

![Jämförelse](./media/virtual-network-create-vm-accelerated-networking/image1.png)

<span data-ttu-id="7a7b1-110">Utan snabbare nätverk måste alla nätverkstrafiken till och från hello VM passerar hello värden och hello virtuella växeln.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-110">Without accelerated networking, all networking traffic in and out of hello VM must traverse hello host and hello virtual switch.</span></span> <span data-ttu-id="7a7b1-111">hello virtuella växeln tillhandahåller alla tvingande, så som nätverkssäkerhetsgrupper, komma åt Kontrollistor, isolering och andra virtualiserade services toonetwork nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-111">hello virtual switch provides all policy enforcement, such as network security groups, access control lists, isolation, and other network virtualized services toonetwork traffic.</span></span> <span data-ttu-id="7a7b1-112">Mer om virtuella växlar, läsa hello toolearn [Hyper-V-nätverksvirtualisering och den virtuella växeln](https://technet.microsoft.com/library/jj945275.aspx) artikel.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-112">toolearn more about virtual switches, read hello [Hyper-V network virtualization and virtual switch](https://technet.microsoft.com/library/jj945275.aspx) article.</span></span>

<span data-ttu-id="7a7b1-113">Med snabbare nätverk nätverkstrafik når hello Virtuella datorns nätverksgränssnitt (NIC) och sedan vidarebefordras toohello VM.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-113">With accelerated networking, network traffic arrives at hello VM's network interface (NIC), and is then forwarded toohello VM.</span></span> <span data-ttu-id="7a7b1-114">Alla nätverksprinciper som hello virtuell växel gäller utan snabbare nätverksfunktioner avläst och används i maskinvara.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-114">All network policies that hello virtual switch applies without accelerated networking are offloaded and applied in hardware.</span></span> <span data-ttu-id="7a7b1-115">Tillämpa principen i maskinvara aktiverar hello NIC tooforward nätverkstrafik direkt toohello VM, kringgå hello värden och hello-växel, samtidigt som alla hello principen tillämpades på hello värden.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-115">Applying policy in hardware enables hello NIC tooforward network traffic directly toohello VM, bypassing hello host and hello virtual switch, while maintaining all hello policy it applied in hello host.</span></span>

<span data-ttu-id="7a7b1-116">hello gäller fördelarna med snabbare nätverksfunktioner endast toohello virtuell dator som den är aktiverad på.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-116">hello benefits of accelerated networking only apply toohello VM that it is enabled on.</span></span> <span data-ttu-id="7a7b1-117">För bästa resultat hello är det perfekta tooenable funktionen på minst två virtuella datorer anslutna toohello samma Azure Virtual Network (VNet).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-117">For hello best results, it is ideal tooenable this feature on at least two VMs connected toohello same Azure Virtual Network (VNet).</span></span> <span data-ttu-id="7a7b1-118">Vid kommunikation över Vnet eller anslutande lokalt har funktionen minimal inverkan toooverall svarstid.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-118">When communicating across VNets or connecting on-premises, this feature has minimal impact toooverall latency.</span></span>

> [!WARNING]
> <span data-ttu-id="7a7b1-119">Den här Linux Public Preview inte kan ha hello samma nivå av tillgänglighet och tillförlitlighet som funktioner som är i allmänhet tillgänglighetsversion.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-119">This Linux Public Preview may not have hello same level of availability and reliability as features that are in general availability release.</span></span> <span data-ttu-id="7a7b1-120">hello-funktionen stöds inte, kan ha begränsad kapacitet och kanske inte tillgänglig på alla platser i Azure.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-120">hello feature is not supported, may have constrained capabilities, and may not be available in all Azure locations.</span></span> <span data-ttu-id="7a7b1-121">Kontrollera hello Azure Virtual Network uppdateringar sida för hello senaste meddelanden på tillgänglighet och status för den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-121">For hello most up-to-date notifications on availability and status of this feature, check hello Azure Virtual Network updates page.</span></span>

## <a name="benefits"></a><span data-ttu-id="7a7b1-122">Fördelar</span><span class="sxs-lookup"><span data-stu-id="7a7b1-122">Benefits</span></span>
* <span data-ttu-id="7a7b1-123">**Lägre latens / högre paket per sekund (pps):** tar bort hello virtuell växel från hello datapath tar bort hello tid paket i hello värden för behandling av princip för och ökar hello antalet paket som kan bearbetas i hello VM.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-123">**Lower Latency / Higher packets per second (pps):** Removing hello virtual switch from hello datapath removes hello time packets spend in hello host for policy processing and increases hello number of packets that can be processed inside hello VM.</span></span>
* <span data-ttu-id="7a7b1-124">**Minskar jitter:** virtuella växeln bearbetning beror på hello mängden principinformation som måste tillämpas toobe och hello arbetsbelastning av hello CPU som gör hello bearbetning.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-124">**Reduced jitter:** Virtual switch processing depends on hello amount of policy that needs toobe applied and hello workload of hello CPU that is doing hello processing.</span></span> <span data-ttu-id="7a7b1-125">Avlastning hello princip tvingande toohello maskinvara tar bort den variationen genom att leverera paket direkt toohello VM, tar bort hello värden tooVM kommunikation och alla programvara avbrott och kontext växlar.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-125">Offloading hello policy enforcement toohello hardware removes that variability by delivering packets directly toohello VM, removing hello host tooVM communication and all software interrupts and context switches.</span></span>
* <span data-ttu-id="7a7b1-126">**Minskas CPU-användning:** Bypassing hello virtuella växeln på värden för hello leder tooless CPU-belastningen för bearbetning av nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-126">**Decreased CPU utilization:** Bypassing hello virtual switch in hello host leads tooless CPU utilization for processing network traffic.</span></span>

## <span data-ttu-id="7a7b1-127"><a name="Limitations"></a>Begränsningar</span><span class="sxs-lookup"><span data-stu-id="7a7b1-127"><a name="Limitations"></a>Limitations</span></span>
<span data-ttu-id="7a7b1-128">hello följande begränsningar gäller när du använder den här funktionen:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-128">hello following limitations exist when using this capability:</span></span>

* <span data-ttu-id="7a7b1-129">**Network interface skapa:** Accelerated nätverk kan bara aktiveras för en ny nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-129">**Network interface creation:** Accelerated networking can only be enabled for a new NIC.</span></span> <span data-ttu-id="7a7b1-130">Det går inte att aktivera för en befintlig nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-130">It cannot be enabled for an existing NIC.</span></span>
* <span data-ttu-id="7a7b1-131">**Skapa en virtuell dator:** A nätverkskortet med snabbare nätverksfunktioner som är aktiverad kan bara vara anslutna tooa VM när hello virtuell dator skapas.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-131">**VM creation:** A NIC with accelerated networking enabled can only be attached tooa VM when hello VM is created.</span></span> <span data-ttu-id="7a7b1-132">hello NIC får inte vara anslutna tooan befintliga VM.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-132">hello NIC cannot be attached tooan existing VM.</span></span>
* <span data-ttu-id="7a7b1-133">**Regioner:** virtuella Windows-datorer med snabbare nätverksfunktioner erbjuds i de flesta Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-133">**Regions:** Windows VMs with accelerated networking are offered in most Azure regions.</span></span> <span data-ttu-id="7a7b1-134">Linux virtuella datorer med snabbare nätverksfunktioner erbjuds i flera områden.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-134">Linux VMs with accelerated networking are offered in multiple regions.</span></span> <span data-ttu-id="7a7b1-135">hello-regioner som den här funktionen finns i expanderar.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-135">hello regions this capability is available in is expanding.</span></span> <span data-ttu-id="7a7b1-136">Se hello Azure virtuella nätverk uppdaterar blogg nedan hello senaste informationen.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-136">Please see hello Azure Virtual Networking Updates blog below for hello latest information.</span></span>   
* <span data-ttu-id="7a7b1-137">**Operativsystem som stöds:** Windows: Microsoft Windows Server 2012 R2 Datacenter och Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-137">**Supported operating systems:** Windows: Microsoft Windows Server 2012 R2 Datacenter and Windows Server 2016.</span></span> <span data-ttu-id="7a7b1-138">Linux: Ubuntu Server 16.04 LTS med kernel 4.4.0-77 eller högre, SLES 12 SP2, RHEL 7.3 och CentOS 7.3 (publicerad av ”falsk Wave programvara”).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-138">Linux: Ubuntu Server 16.04 LTS with kernel 4.4.0-77 or higher, SLES 12 SP2, RHEL 7.3 and CentOS 7.3 (Published by “Rogue Wave Software”).</span></span>
* <span data-ttu-id="7a7b1-139">**VM-storlek:** generella och beräknings-optimerad instans storlekar med minst åtta kärnor.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-139">**VM Size:** General purpose and compute-optimized instance sizes with eight or more cores.</span></span> <span data-ttu-id="7a7b1-140">Mer information finns i hello [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) och [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-storlekar artiklar.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-140">For more information, see hello [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) and [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM sizes articles.</span></span> <span data-ttu-id="7a7b1-141">hello uppsättning stöds storlekar på VM-instansen kommer att expandera i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-141">hello set of supported VM instance sizes will expand in hello future.</span></span>
* <span data-ttu-id="7a7b1-142">**Distribution via Azure Resource Manager (ARM):** snabbare nätverk är inte tillgänglig för distribution via ASM/RDFE.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-142">**Deployment through Azure Resource Manager (ARM) only:** Accelerated Networking is not available for deployment through ASM/RDFE.</span></span>

<span data-ttu-id="7a7b1-143">Ändringar toothese begränsningar meddelas via hello [virtuella Azure-nätverk uppdaterar](https://azure.microsoft.com/updates/accelerated-networking-in-preview) sidan.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-143">Changes toothese limitations are announced through hello [Azure Virtual Networking updates](https://azure.microsoft.com/updates/accelerated-networking-in-preview) page.</span></span>

## <a name="create-a-windows-vm"></a><span data-ttu-id="7a7b1-144">Skapa en virtuell Windows-dator</span><span class="sxs-lookup"><span data-stu-id="7a7b1-144">Create a Windows VM</span></span>
<span data-ttu-id="7a7b1-145">Du kan använda hello Azure-portalen eller Azure [PowerShell](#windows-powershell) toocreate hello VM.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-145">You can use hello Azure portal or Azure [PowerShell](#windows-powershell) toocreate hello VM.</span></span>

### <span data-ttu-id="7a7b1-146"><a name="windows-portal"></a>Portal</span><span class="sxs-lookup"><span data-stu-id="7a7b1-146"><a name="windows-portal"></a>Portal</span></span>

1. <span data-ttu-id="7a7b1-147">Öppna en webbläsare hello Azure [portal](https://portal.azure.com) och logga in med ditt Azure [konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-147">From an Internet browser, open hello Azure [portal](https://portal.azure.com) and sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="7a7b1-148">Om du inte redan har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-148">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="7a7b1-149">I hello-portalen klickar du på **+ ny** > **Compute** > **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-149">In hello portal, click **+ New** > **Compute** > **Windows Server 2016 Datacenter**.</span></span> 
3. <span data-ttu-id="7a7b1-150">I hello **Windows Server 2016 Datacenter** bladet som visas, lämna *Resource Manager* valda under **Välj en distributionsmodell**, och klicka på **skapa** .</span><span class="sxs-lookup"><span data-stu-id="7a7b1-150">In hello **Windows Server 2016 Datacenter** blade that appears, leave *Resource Manager* selected under **Select a deployment model**, and click **Create**.</span></span>
4. <span data-ttu-id="7a7b1-151">I hello **grunderna** bladet som visas, ange hello följande värden, lämna hello återstående standardalternativen eller Välj eller ange egna värden och på hello **OK** knappen:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-151">In hello **Basics** blade that appears, enter hello following values, leave hello remaining default options or select or enter your own values, and click hello **OK** button:</span></span>

    |<span data-ttu-id="7a7b1-152">Inställning</span><span class="sxs-lookup"><span data-stu-id="7a7b1-152">Setting</span></span>|<span data-ttu-id="7a7b1-153">Värde</span><span class="sxs-lookup"><span data-stu-id="7a7b1-153">Value</span></span>|
    |---|---|
    |<span data-ttu-id="7a7b1-154">Namn</span><span class="sxs-lookup"><span data-stu-id="7a7b1-154">Name</span></span>|<span data-ttu-id="7a7b1-155">MyVm</span><span class="sxs-lookup"><span data-stu-id="7a7b1-155">MyVm</span></span>|
    |<span data-ttu-id="7a7b1-156">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="7a7b1-156">Resource group</span></span>|<span data-ttu-id="7a7b1-157">Lämna **Skapa nytt** markerad och ange *MyResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="7a7b1-157">Leave **Create new** selected and enter *MyResourceGroup*</span></span>|
    |<span data-ttu-id="7a7b1-158">Plats</span><span class="sxs-lookup"><span data-stu-id="7a7b1-158">Location</span></span>|<span data-ttu-id="7a7b1-159">Västra USA 2</span><span class="sxs-lookup"><span data-stu-id="7a7b1-159">West US 2</span></span>|

    <span data-ttu-id="7a7b1-160">Om du är ny tooAzure lär du dig mer om [resursgrupper](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [prenumerationer](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), och [platser](https://azure.microsoft.com/regions) (som även kallas tooas regioner).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-160">If you're new tooAzure, learn more about [Resource groups](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [subscriptions](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), and [locations](https://azure.microsoft.com/regions) (which are also referred tooas regions).</span></span>
5. <span data-ttu-id="7a7b1-161">I hello **välja en storlek** bladet som visas, ange *8* i hello **minsta kärnor** rutan och klicka sedan på **visa alla**.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-161">In hello **Choose a size** blade that appears, enter *8* in hello **Minimum cores** box, then click **View all**.</span></span>
6. <span data-ttu-id="7a7b1-162">Klicka på **DS4_V2 Standard**, eller någon stöds VM, klicka på hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-162">Click **DS4_V2 Standard**, or any supported VM, then click hello **Select** button.</span></span>
7. <span data-ttu-id="7a7b1-163">I hello **inställningar** bladet som visas, lämnar du alla inställningar som-är undantag för **aktiverad** under **snabbare nätverk**, klicka på hello **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-163">In hello **Settings** blade that appears, leave all settings as-is, except click **Enabled** under **Accelerated networking**, then click hello **OK** button.</span></span> <span data-ttu-id="7a7b1-164">**Obs:** om föregående steg du valde i värdena för VM-storlek, operativsystem eller plats som inte listas i hello [begränsningar](#Limitations) i den här artikeln **Accelerated nätverk**inte visas.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-164">**Note:** If, in previous steps, you selected values for VM size, operating system, or location that aren't listed in hello [Limitations](#Limitations) section of this article, **Accelerated networking** isn't visible.</span></span>
8. <span data-ttu-id="7a7b1-165">I hello **sammanfattning** bladet som visas, klicka på hello **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-165">In hello **Summary** blade that appears, click hello **OK** button.</span></span> <span data-ttu-id="7a7b1-166">Azure startar skapa hello VM.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-166">Azure starts creating hello VM.</span></span> <span data-ttu-id="7a7b1-167">Skapa en virtuell dator tar några minuter.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-167">VM creation takes a few minutes.</span></span>
9. <span data-ttu-id="7a7b1-168">tooinstall hello snabbare nätverksdrivrutin för Windows, fullständig hello stegen i hello [konfigurera Windows](#configure-windows) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-168">tooinstall hello accelerated networking driver for Windows, complete hello steps in hello [Configure Windows](#configure-windows) section of this article.</span></span>

### <span data-ttu-id="7a7b1-169"><a name="windows-powershell"></a>PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a7b1-169"><a name="windows-powershell"></a>PowerShell</span></span>
1. <span data-ttu-id="7a7b1-170">Installera hello senaste versionen av hello Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-170">Install hello latest version of hello Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module.</span></span> <span data-ttu-id="7a7b1-171">Om du är ny tooAzure PowerShell, läsa hello [översikt över Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-171">If you're new tooAzure PowerShell, read hello [Azure PowerShell overview](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) article.</span></span>
2. <span data-ttu-id="7a7b1-172">Starta en PowerShell-session genom att klicka på Start-knappen hello att skriva **powershell**, klicka på **PowerShell** från hello sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-172">Start a PowerShell session by clicking hello Windows Start button, typing **powershell**, then clicking **PowerShell** from hello search results.</span></span>
3. <span data-ttu-id="7a7b1-173">Ange hello i PowerShell-fönstret `login-azurermaccount` kommandot toosign in med ditt Azure [konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-173">In your PowerShell window, enter hello `login-azurermaccount` command toosign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="7a7b1-174">Om du inte redan har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-174">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
4. <span data-ttu-id="7a7b1-175">Kopiera hello följande skript i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-175">In your browser, copy hello following script:</span></span>
    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Windows `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id 

    # Create hello virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    #
    ```
5. <span data-ttu-id="7a7b1-176">Högerklicka på toopaste hello skriptet i PowerShell-fönster och starta kör den.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-176">In your PowerShell window, right-click toopaste hello script and start executing it.</span></span> <span data-ttu-id="7a7b1-177">Du tillfrågas om användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-177">You are prompted for a username and password.</span></span> <span data-ttu-id="7a7b1-178">Använd dessa autentiseringsuppgifter toolog i toohello VM när du ansluter tooit i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-178">Use these credentials toolog in toohello VM when connecting tooit in hello next step.</span></span> <span data-ttu-id="7a7b1-179">Om hello skriptet misslyckas och du har ändrat värdena i hello skriptet innan du kör den, bekräfta hello-värden som du använde för VM-storlek, operativsystem, och platsen visas i hello [begränsningar](#Limitations) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-179">If hello script fails, and you changed values in hello script before executing it, confirm hello values you used for VM size, operating system, and location are listed in hello [Limitations](#Limitations) section of this article.</span></span>
6. <span data-ttu-id="7a7b1-180">tooinstall hello snabbare nätverksdrivrutin för Windows, fullständig hello stegen i hello [konfigurera Windows](#configure-windows) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-180">tooinstall hello accelerated networking driver for Windows, complete hello steps in hello [Configure Windows](#configure-windows) section of this article.</span></span>

### <span data-ttu-id="7a7b1-181"><a name="configure-windows"></a>Konfigurera Windows</span><span class="sxs-lookup"><span data-stu-id="7a7b1-181"><a name="configure-windows"></a>Configure Windows</span></span>
<span data-ttu-id="7a7b1-182">När du har skapat hello VM i Azure måste du installera hello snabbare nätverksdrivrutin för Windows.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-182">Once you create hello VM in Azure, you must install hello accelerated networking driver for Windows.</span></span> <span data-ttu-id="7a7b1-183">Innan du slutför följande steg hello, måste du skapa en virtuell Windows-dator med snabbare nätverk med hjälp av antingen hello [portal](#windows-portal) eller [PowerShell](#windows-powershell) steg i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-183">Before completing hello following steps, you must have created a Windows VM with accelerated networking using either hello [portal](#windows-portal) or [PowerShell](#windows-powershell) steps in this article.</span></span> 

1. <span data-ttu-id="7a7b1-184">Öppna en webbläsare hello Azure [portal](https://portal.azure.com) och logga in med ditt Azure [konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-184">From an Internet browser, open hello Azure [portal](https://portal.azure.com) and sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="7a7b1-185">Om du inte redan har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-185">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="7a7b1-186">I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *MyVm*.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-186">In hello box that contains hello text *Search resources* at hello top of hello Azure portal, type *MyVm*.</span></span> <span data-ttu-id="7a7b1-187">När **MyVm** visas i sökresultaten hello klickar du på den.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-187">When **MyVm** appears in hello search results, click it.</span></span>
3. <span data-ttu-id="7a7b1-188">I hello **MyVm** bladet som visas, klicka på hello **Anslut** knappen i hello övre vänstra hörnet av hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-188">In hello **MyVm** blade that appears, click hello **Connect** button in hello top left corner of hello blade.</span></span> <span data-ttu-id="7a7b1-189">**Obs:** om **skapa** är synlig under hello **Anslut** knappen Azure inte ännu har skapar hello VM.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-189">**Note:** If **Creating** is visible under hello **Connect** button, Azure has not yet finished creating hello VM.</span></span> <span data-ttu-id="7a7b1-190">Klicka på **Anslut** efter att du inte längre se **skapa** under hello **Anslut** knappen.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-190">Click **Connect** only after you no longer see **Creating** under hello **Connect** button.</span></span>
4. <span data-ttu-id="7a7b1-191">Tillåt din webbläsare toodownload hello **MyVm.rdp** fil.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-191">Allow your browser toodownload hello **MyVm.rdp** file.</span></span>  <span data-ttu-id="7a7b1-192">När du har hämtat, klickar du på hello filen tooopen den.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-192">Once downloaded, click hello file tooopen it.</span></span> 
5. <span data-ttu-id="7a7b1-193">Klicka på hello **Anslut** knapp i hello **anslutning till fjärrskrivbord** som visas får ett meddelande om att hello utgivaren av hello anslutning inte kan identifieras.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-193">Click hello **Connect** button in hello **Remote Desktop Connection** box that appears, notifying you that hello publisher of hello remote connection can't be identified.</span></span>
6. <span data-ttu-id="7a7b1-194">I hello **Windows-säkerhet** som visas, klicka på **fler alternativ**, klicka på **Använd ett annat konto**.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-194">In hello **Windows Security** box that appears, click **More choices**, then click **Use a different account**.</span></span> <span data-ttu-id="7a7b1-195">Ange hello användarnamn och lösenord som du angav i steg 4 och klicka sedan på hello **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-195">Enter hello username and password you entered in step 4, then click hello **OK** button.</span></span>
7. <span data-ttu-id="7a7b1-196">Klicka på hello **Ja** knappen i hello anslutning till fjärrskrivbord som visas, får ett meddelande om att hello identitet hello fjärrdatorn inte kan verifieras.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-196">Click hello **Yes** button in hello Remote Desktop Connection box that appears, notifying you that hello identity of hello remote computer cannot be verified.</span></span>
8. <span data-ttu-id="7a7b1-197">Högerklicka på hello Start-knappen och klicka på **Enhetshanteraren**.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-197">Right-click hello Windows Start button and click **Device Manager**.</span></span> <span data-ttu-id="7a7b1-198">Expandera hello **nätverkskort** nod.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-198">Expand hello **Network adapters** node.</span></span> <span data-ttu-id="7a7b1-199">Bekräfta att hello **Mellanox ConnectX 3 virtuella funktionen Ethernet-nätverkskort** visas som i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-199">Confirm that hello **Mellanox ConnectX-3 Virtual Function Ethernet Adapter** appears, as shown in hello following picture:</span></span>
   
    ![Enhetshanteraren](./media/virtual-network-create-vm-accelerated-networking/image2.png)

9. <span data-ttu-id="7a7b1-201">Snabbare nätverksfunktioner har nu aktiverats för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-201">Accelerated Networking is now enabled for your VM.</span></span>

## <a name="create-a-linux-vm"></a><span data-ttu-id="7a7b1-202">Skapa en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="7a7b1-202">Create a Linux VM</span></span>
<span data-ttu-id="7a7b1-203">Du kan använda hello Azure-portalen eller Azure [PowerShell](#linux-powershell) toocreate en Ubuntu eller SLES VM.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-203">You can use hello Azure portal or Azure [PowerShell](#linux-powershell) toocreate an Ubuntu or SLES VM.</span></span> <span data-ttu-id="7a7b1-204">Det finns ett annat arbetsflöde RHEL och CentOS virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-204">For RHEL and CentOS VMs there is a different workflow.</span></span>  <span data-ttu-id="7a7b1-205">Se hello anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-205">Please see hello instructions below.</span></span>

### <span data-ttu-id="7a7b1-206"><a name="linux-portal"></a>Portal</span><span class="sxs-lookup"><span data-stu-id="7a7b1-206"><a name="linux-portal"></a>Portal</span></span>
1. <span data-ttu-id="7a7b1-207">Registrera dig för hello snabbare nätverk för Linux-förhandsgranskning genom att följa steg 1-5 för hello [och skapar en Linux VM - PowerShell](#linux-powershell) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-207">Register for hello accelerated networking for Linux preview by completing steps 1-5 of hello [Create a Linux VM - PowerShell](#linux-powershell) section of this article.</span></span>  <span data-ttu-id="7a7b1-208">Du kan inte registrera för hello förhandsgranskning i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-208">You cannot register for hello preview in hello portal.</span></span>
2. <span data-ttu-id="7a7b1-209">Slutför stegen 1 – 8 i hello [skapa en virtuell dator i Windows - portal](#windows-portal) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-209">Complete steps 1-8 in hello [Create a Windows VM - portal](#windows-portal) section of this article.</span></span> <span data-ttu-id="7a7b1-210">I steg 2, klickar du på **Ubuntu Server 16.04 LTS** i stället för **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-210">In step 2, click **Ubuntu Server 16.04 LTS** instead of **Windows Server 2016 Datacenter**.</span></span> <span data-ttu-id="7a7b1-211">För den här självstudiekursen Välj toouse lösenord istället för en SSH-nyckel om för Produktionsdistribution, du kan använda antingen.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-211">For this tutorial, choose toouse a password, rather than an SSH key, though for production deployments, you can use either.</span></span> <span data-ttu-id="7a7b1-212">Om **snabbare nätverk** visas inte när du har slutfört steg 7 i hello [skapa en virtuell dator i Windows - portal](#windows-portal) avsnitt i den här artikeln är förmodligen för en av följande orsaker hello:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-212">If **Accelerated networking** does not appear when you complete step 7 of hello [Create a Windows VM - portal](#windows-portal) section of this article, it's likely for one of hello following reasons:</span></span>
    - <span data-ttu-id="7a7b1-213">Du ännu inte har registrerats för hello förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-213">You are not yet registered for hello preview.</span></span> <span data-ttu-id="7a7b1-214">Kontrollera att din registreringstillstånd **registrerade**, enligt beskrivningen i steg 4 i hello [och skapar en Linux VM - Powershell](#linux-powershell) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-214">Confirm that your registration state is **Registered**, as explained in step 4 of hello [Create a Linux VM - Powershell](#linux-powershell) section of this article.</span></span> <span data-ttu-id="7a7b1-215">**Obs:** om du har deltagit i hello Accelerated nätverk för virtuella Windows-datorer preview (det är inte längre nödvändigt tooregister toouse snabbare nätverk för virtuella Windows-datorer), du inte är automatiskt registrerad för hello Accelerated nätverk för Förhandsgranskning av virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-215">**Note:** If you participated in hello Accelerated networking for Windows VMs preview (it's no longer necessary tooregister toouse Accelerated networking for Windows VMs), you are not automatically registered for hello Accelerated networking for Linux VMs preview.</span></span> <span data-ttu-id="7a7b1-216">Du måste registrera dig för hello Accelerated nätverk för virtuella Linux-datorer Förhandsgranska tooparticipate i den.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-216">You must register for hello Accelerated networking for Linux VMs preview tooparticipate in it.</span></span>
    - <span data-ttu-id="7a7b1-217">Du har inte valt VM-storlek, operativsystem eller plats som anges i hello [begränsningar](#limitations) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-217">You have not selected a VM size, operating system, or location listed in hello [Limitations](#limitations) section of this article.</span></span>
3. <span data-ttu-id="7a7b1-218">tooinstall hello snabbare nätverksdrivrutin för Linux, fullständig hello stegen i hello [konfigurera Linux](#configure-linux) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-218">tooinstall hello accelerated networking driver for Linux, complete hello steps in hello [Configure Linux](#configure-linux) section of this article.</span></span>

### <span data-ttu-id="7a7b1-219"><a name="linux-powershell"></a>PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a7b1-219"><a name="linux-powershell"></a>PowerShell</span></span>

>[!WARNING]
><span data-ttu-id="7a7b1-220">Om du skapar virtuella Linux-datorer med snabbare nätverk i en prenumeration och försök sedan toocreate en virtuell Windows-dator med snabbare nätverksfunktioner i hello samma prenumeration hello skapa en virtuell Windows-dator kan misslyckas.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-220">If you create Linux VMs with accelerated networking in a subscription, and then attempt toocreate a Windows VM with accelerated networking in hello same subscription, hello Windows VM creation may fail.</span></span> <span data-ttu-id="7a7b1-221">Den här förhandsversionen rekommenderas att du testar Linux och Windows-datorer med snabbare nätverk i separata prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-221">During this preview, it's recommended that you test Linux and Windows VMs with accelerated networking in separate subscriptions.</span></span>
>

1. <span data-ttu-id="7a7b1-222">Installera hello senaste versionen av hello Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-222">Install hello latest version of hello Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module.</span></span> <span data-ttu-id="7a7b1-223">Om du är ny tooAzure PowerShell, läsa hello [översikt över Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-223">If you're new tooAzure PowerShell, read hello [Azure PowerShell overview](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) article.</span></span>
2. <span data-ttu-id="7a7b1-224">Starta en PowerShell-session genom att klicka på Start-knappen hello att skriva **powershell**, klicka på **PowerShell** från hello sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-224">Start a PowerShell session by clicking hello Windows Start button, typing **powershell**, then clicking **PowerShell** from hello search results.</span></span>
3. <span data-ttu-id="7a7b1-225">Ange hello i PowerShell-fönstret `login-azurermaccount` kommandot toosign in med ditt Azure [konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-225">In your PowerShell window, enter hello `login-azurermaccount` command toosign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="7a7b1-226">Om du inte redan har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-226">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
4. <span data-ttu-id="7a7b1-227">Registrera dig för hello snabbare nätverk för Azure preview genom att slutföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-227">Register for hello accelerated networking for Azure preview by completing hello following steps:</span></span>
    - <span data-ttu-id="7a7b1-228">Skicka ett e-postmeddelande för[ axnpreview@microsoft.com ](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) med Azure prenumerations-ID och avsedd att användas.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-228">Send an email too[axnpreview@microsoft.com](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) with your Azure subscription ID and intended use.</span></span> <span data-ttu-id="7a7b1-229">Vänta tills en e-postbekräftelse från Microsoft om prenumerationen har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-229">Please wait for an email confirmation from Microsoft about your subscription being enabled.</span></span>
    - <span data-ttu-id="7a7b1-230">Ange hello efter kommandot tooconfirm du är registrerad för hello preview:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-230">Enter hello following command tooconfirm you are registered for hello preview:</span></span>
    
        ```powershell
        Get-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingForLinux -ProviderNamespace Microsoft.Network
        ```

        <span data-ttu-id="7a7b1-231">Fortsätt inte med steg 5 tills **registrerade** visas i hello utdata när du har angett hello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-231">Do not continue with step 5 until **Registered** appears in hello output after you enter hello previous command.</span></span> <span data-ttu-id="7a7b1-232">Din utdata måste se ut så hello följande utdata innan du fortsätter:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-232">Your output must look like hello following output before continuing:</span></span>
    
        ```powershell
        FeatureName                        ProviderName      RegistrationState
        -----------                        ------------      -----------------
        AllowAcceleratedNetworkingForLinux Microsoft.Network Registered
        ```
        
      >[!NOTE]
      ><span data-ttu-id="7a7b1-233">Om du har deltagit i hello Accelerated nätverk för virtuella Windows-datorer preview (det är inte längre nödvändigt tooregister toouse snabbare nätverk för virtuella Windows-datorer), du inte är automatiskt registrerad för hello Accelerated nätverk för virtuella Linux-datorer förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-233">If you participated in hello Accelerated networking for Windows VMs preview (it's no longer necessary tooregister toouse Accelerated networking for Windows VMs), you are not automatically registered for hello Accelerated networking for Linux VMs preview.</span></span> <span data-ttu-id="7a7b1-234">Du måste registrera dig för hello Accelerated nätverk för virtuella Linux-datorer Förhandsgranska tooparticipate i den.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-234">You must register for hello Accelerated networking for Linux VMs preview tooparticipate in it.</span></span>
      >
5. <span data-ttu-id="7a7b1-235">Kopiera hello följande skript i webbläsaren ersätter Ubuntu eller SLES efter behov.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-235">In your browser, copy hello following script substituting Ubuntu or SLES as desired.</span></span>  <span data-ttu-id="7a7b1-236">Igen, Redhat och CentOS har ett annat arbetsflöde som beskrivs nedan:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-236">Again, Redhat and CentOS have a different workflow outlined below:</span></span>

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Create a new Storage account and define hello new VM’s OSDisk name and its URI
    # Must end with ".vhd" extension
    $OSDiskName = "MyOsDiskName.vhd"
    # Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.
    $OSDiskSAName = "thestorageaccountname"  
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $RgName -Name $OSDiskSAName -Type "Standard_GRS" -Location $Location
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName
 
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Linux `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName Canonical `
      -Offer UbuntuServer `
      -Skus 16.04-LTS `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk -Name $OSDiskName `
      -VhdUri $OSDiskUri `
      -CreateOption FromImage 

    # Create hello virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    ```
    
6. <span data-ttu-id="7a7b1-237">Högerklicka på toopaste hello skriptet i PowerShell-fönster och starta kör den.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-237">In your PowerShell window, right-click toopaste hello script and start executing it.</span></span> <span data-ttu-id="7a7b1-238">Du tillfrågas om användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-238">You are prompted for a username and password.</span></span> <span data-ttu-id="7a7b1-239">Använd dessa autentiseringsuppgifter toolog i toohello VM när du ansluter tooit i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-239">Use these credentials toolog in toohello VM when connecting tooit in hello next step.</span></span> <span data-ttu-id="7a7b1-240">Om det inte går att hello skript, bekräftar du att:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-240">If hello script fails, confirm that:</span></span>
    - <span data-ttu-id="7a7b1-241">Du är registrerad för hello förhandsgranskning, enligt beskrivningen i steg 4</span><span class="sxs-lookup"><span data-stu-id="7a7b1-241">You are registered for hello preview, as explained in step 4</span></span>
    - <span data-ttu-id="7a7b1-242">Om du har ändrat VM-storlek, typ av operativsystem eller plats värden i hello skript innan den körs, att hello värden finns i hello [begränsningar](#Limitations) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-242">If you changed VM size, operating system type, or location values in hello script before executing it, that hello values are listed in hello [Limitations](#Limitations) section of this article.</span></span>
7. <span data-ttu-id="7a7b1-243">tooinstall hello snabbare nätverksdrivrutin för Linux, fullständig hello stegen i hello [konfigurera Linux](#configure-linux) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-243">tooinstall hello accelerated networking driver for Linux, complete hello steps in hello [Configure Linux](#configure-linux) section of this article.</span></span>

### <span data-ttu-id="7a7b1-244"><a name="configure-linux"></a>Konfigurera Linux</span><span class="sxs-lookup"><span data-stu-id="7a7b1-244"><a name="configure-linux"></a>Configure Linux</span></span>

<span data-ttu-id="7a7b1-245">När du har skapat hello VM i Azure måste du installera hello snabbare nätverksdrivrutin för Linux.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-245">Once you create hello VM in Azure, you must install hello accelerated networking driver for Linux.</span></span> <span data-ttu-id="7a7b1-246">Innan du slutför följande steg hello, måste du skapa en Linux VM snabbare nätverk med hjälp av antingen hello [portal](#linux-portal) eller [PowerShell](#linux-powershell) steg i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-246">Before completing hello following steps, you must have created a Linux VM with accelerated networking using either hello [portal](#linux-portal) or [PowerShell](#linux-powershell) steps in this article.</span></span> 

1. <span data-ttu-id="7a7b1-247">Öppna en webbläsare hello Azure [portal](https://portal.azure.com) och logga in med ditt Azure [konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-247">From an Internet browser, open hello Azure [portal](https://portal.azure.com) and sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="7a7b1-248">Om du inte redan har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-248">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="7a7b1-249">Hello överst i hello portalen toohello höger om hello *söka resurser* Klicka hello **> _** ikonen toostart ett Bash molnet gränssnitt (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="7a7b1-249">At hello top of hello portal, toohello right of hello *Search resources* bar, click hello **>_** icon toostart a Bash cloud shell (Preview).</span></span> <span data-ttu-id="7a7b1-250">hello Bash molnet shell fönstret visas längst ned hello hello portal och efter några sekunder visas en  **username@Azure:~ $** prompt.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-250">hello Bash cloud shell pane appears at hello bottom of hello portal and after a few seconds, presents a **username@Azure:~$** prompt.</span></span> <span data-ttu-id="7a7b1-251">Om du kan SSH toohello VM från din dator i stället för på hello molnet shell förutsätts hello i den här självstudiekursen att du använder hello molnet shell.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-251">Though you can SSH toohello VM from your computer, rather than hello cloud shell, hello instructions in this tutorial assume you're using hello cloud shell.</span></span>
3. <span data-ttu-id="7a7b1-252">Hello över hello portal hello i rutan som innehåller hello text *söka resurser*, typen *MyVm*.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-252">At hello top of hello portal, in hello box that contains hello text *Search resources*, type *MyVm*.</span></span> <span data-ttu-id="7a7b1-253">När **MyVm** visas i sökresultaten hello klickar du på den.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-253">When **MyVm** appears in hello search results, click it.</span></span>
4. <span data-ttu-id="7a7b1-254">I hello **MyVm** bladet som visas, klicka på hello **Anslut** knappen i hello övre vänstra hörnet av hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-254">In hello **MyVm** blade that appears, click hello **Connect** button in hello top left corner of hello blade.</span></span> <span data-ttu-id="7a7b1-255">**Obs:** om **skapa** är synlig under hello **Anslut** knappen Azure inte ännu har skapar hello VM.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-255">**Note:** If **Creating** is visible under hello **Connect** button, Azure has not yet finished creating hello VM.</span></span> <span data-ttu-id="7a7b1-256">Klicka på **Anslut** efter att du inte längre se **skapa** under hello **Anslut** knappen.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-256">Click **Connect** only after you no longer see **Creating** under hello **Connect** button.</span></span>
5. <span data-ttu-id="7a7b1-257">Azure öppnar en ruta som uppmanar tooenter hello `ssh adminuser@<ipaddress>`.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-257">Azure opens a box telling you tooenter hello `ssh adminuser@<ipaddress>`.</span></span> <span data-ttu-id="7a7b1-258">Ange det här kommandot i hello molnet shell (eller kopiera den från hello ruta som fanns i steg 4 och klistra in den i toohello moln shell), och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-258">Enter this command in hello cloud shell (or copy it from hello box that appeared in step 4 and paste it in toohello cloud shell), then press Enter.</span></span>
6. <span data-ttu-id="7a7b1-259">Ange **Ja** toohello fråga tillfrågas du om du vill ansluta toocontinue, tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-259">Enter **yes** toohello question asking you if you want toocontinue connecting, then press Enter.</span></span>
7. <span data-ttu-id="7a7b1-260">Ange hello lösenordet du angav när du skapar hello VM.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-260">Enter hello password you entered when creating hello VM.</span></span> <span data-ttu-id="7a7b1-261">En gång loggat in toohello VM, visas en adminuser@MyVm:~ $ prompten.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-261">Once successfully logged in toohello VM, you see an adminuser@MyVm:~$ prompt.</span></span> <span data-ttu-id="7a7b1-262">Du är nu inloggad toohello VM via hello molnet shell session.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-262">You are now logged in toohello VM through hello cloud shell session.</span></span> <span data-ttu-id="7a7b1-263">**Obs:** moln shell sessioner timeout efter 10 minuter av inaktivitet.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-263">**Note:** Cloud shell sessions time out after 10 minutes of inactivity.</span></span>

<span data-ttu-id="7a7b1-264">Nu hello instruktioner kan variera beroende på hello-distribution som du använder.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-264">At this point, hello instructions vary based on hello distribution you are using.</span></span> 

#### <a name="ubuntusles"></a><span data-ttu-id="7a7b1-265">Ubuntu/SLES</span><span class="sxs-lookup"><span data-stu-id="7a7b1-265">Ubuntu/SLES</span></span>

1. <span data-ttu-id="7a7b1-266">I Kommandotolken hello ange `uname -r` och bekräfta hello-version för:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-266">At hello prompt, enter `uname -r` and confirm hello version for:</span></span>

    * <span data-ttu-id="7a7b1-267">Ubuntu är ”4.4.0-77-generic” eller högre</span><span class="sxs-lookup"><span data-stu-id="7a7b1-267">Ubuntu is "4.4.0-77-generic," or greater</span></span>
    * <span data-ttu-id="7a7b1-268">SLES är ”4.4.59-92.20-default” eller högre</span><span class="sxs-lookup"><span data-stu-id="7a7b1-268">SLES is "4.4.59-92.20-default" or greater</span></span>

2. <span data-ttu-id="7a7b1-269">Skapa en avkastningen mellan hello standard nätverk vNIC och hello snabbare nätverk vNIC genom att köra hello-kommandon som följer.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-269">Create a bond between hello standard networking vNIC and hello accelerated networking vNIC by running hello commands that follow.</span></span> <span data-ttu-id="7a7b1-270">Nätverkstrafik använder hello högre prestanda snabbare nätverk vNIC medan hello avkastningen garanterar att nätverkstrafiken inte avbryts över vissa ändringar i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-270">Network traffic uses hello higher performing accelerated networking vNIC, while hello bond ensures that networking traffic is not interrupted across certain configuration changes.</span></span>
          
     ```bash
     wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/configure_hv_sriov.sh
     chmod +x ./configure_hv_sriov.sh
     sudo ./configure_hv_sriov.sh
     ```
3. <span data-ttu-id="7a7b1-271">Efter att köra hello skriptet hello VM startar om efter 60 sekunder pausa.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-271">After running hello script, hello VM will restart after a 60 second pause.</span></span>
4. <span data-ttu-id="7a7b1-272">Hello VM startas om och återansluter en gång tooit genom att följa steg 5 – 7 igen.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-272">Once hello VM is restarted, reconnect tooit by completing steps 5-7 again.</span></span>
5. <span data-ttu-id="7a7b1-273">Kör hello `ifconfig` kommando och bekräfta att bond0 är nu tillgänglig och hello gränssnittet visas som upp.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-273">Run hello `ifconfig` command and confirm that bond0 has come up and hello interface is showing as UP.</span></span> 
 
 >[!NOTE]
      ><span data-ttu-id="7a7b1-274">Program med snabbare nätverk måste kommunicera över hello *bond0* gränssnitt inte *eth0*.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-274">Applications using accelerated networking must communicate over hello *bond0* interface, not *eth0*.</span></span>  <span data-ttu-id="7a7b1-275">hello Gränssnittsnamnet ändras innan snabbare nätverksfunktioner når allmän tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-275">hello interface name may change before accelerated networking reaches general availability.</span></span>

#### <a name="rhelcentos"></a><span data-ttu-id="7a7b1-276">RHEL/CentOS</span><span class="sxs-lookup"><span data-stu-id="7a7b1-276">RHEL/CentOS</span></span>

<span data-ttu-id="7a7b1-277">Skapar en Red Hat Enterprise Linux eller CentOS 7.3 VM kräver vissa extra steg tooload hello senaste drivrutiner som behövs för SR-IOV och hello VF (Virtual Function) drivrutinen för hello nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-277">Creating a Red Hat Enterprise Linux or CentOS 7.3 VM requires some extra steps tooload hello latest drivers needed for SR-IOV and hello Virtual Function (VF) driver for hello network card.</span></span> <span data-ttu-id="7a7b1-278">hello förbereds första fasen av hello instruktioner en avbildning som kan använda toomake en eller flera virtuella datorer som har hello drivrutiner som redan har lästs in.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-278">hello first phase of hello instructions prepares an image that can be used toomake one or more virtual machines that have hello drivers pre-loaded.</span></span>

##### <a name="phase-one-prepare-a-red-hat-enterprise-linux-or-centos-73-base-image"></a><span data-ttu-id="7a7b1-279">Steg ett: Förbered en Red Hat Enterprise Linux eller CentOS 7.3 basavbildning.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-279">Phase one: prepare a Red Hat Enterprise Linux or CentOS 7.3 base image.</span></span> 

1.  <span data-ttu-id="7a7b1-280">Etablera en icke - SRIOV CentOS 7.3 VM på Azure</span><span class="sxs-lookup"><span data-stu-id="7a7b1-280">Provision a non-SRIOV CentOS 7.3 VM on Azure</span></span>

2.  <span data-ttu-id="7a7b1-281">Installera LIS 4.2.2:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-281">Install LIS 4.2.2:</span></span>
    
    ```bash
    wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
    tar -xvf lis-rpms-4.2.2-2.tar.gz
    cd LISISO && sudo ./install.sh
    ```

3.  <span data-ttu-id="7a7b1-282">Hämta konfigurationsfiler</span><span class="sxs-lookup"><span data-stu-id="7a7b1-282">Download config files</span></span>
    
    ```bash
    cd /etc/udev/rules.d/  
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/60-hyperv-vf-name.rules 
    cd /usr/sbin/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/hv_vf_name 
    sudo chmod +x hv_vf_name
    cd /etc/sysconfig/network-scripts/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/ifcfg-vf1   
    ```

4.  <span data-ttu-id="7a7b1-283">Ta bort etableringen av den här virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="7a7b1-283">Deprovision this VM</span></span>

    ```bash
    sudo waagent -deprovision+user 
    ```

5.  <span data-ttu-id="7a7b1-284">Stoppa den virtuella datorn, från Azure-portalen och gå Toovm's ”diskar”, avbilda hello OSDisk VHD-URI.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-284">From Azure portal, stop this VM; and go tooVM’s "Disks", capture hello OSDisk’s VHD URI.</span></span> <span data-ttu-id="7a7b1-285">Den här URI: N innehåller hello basavbildning VHD namn och dess storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-285">This URI contains hello base image’s VHD name and its storage account.</span></span> 
 
##### <a name="phase-two-provision-new-vms-on-azure"></a><span data-ttu-id="7a7b1-286">Steg två: etablera nya virtuella datorer på Azure</span><span class="sxs-lookup"><span data-stu-id="7a7b1-286">Phase two: Provision new VMs on Azure</span></span>

1.  <span data-ttu-id="7a7b1-287">Etablera nya virtuella datorer baserade med ny AzureRMVMConfig med hello basavbildning VHD till i den första fasen, med AcceleratedNetworking aktiverad på hello virtuellt nätverkskort:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-287">Provision new VMs based with New-AzureRMVMConfig using hello base image VHD captured in phase one, with AcceleratedNetworking enabled on hello vNIC:</span></span>

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
     -Name $RgName `
     -Location $Location

    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
     -Name MySubnet `
     -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
     -ResourceGroupName $RgName `
     -Location $Location `
     -Name MyVnet `
     -AddressPrefix 10.0.0.0/16 `
     -Subnet $Subnet
    
    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
     -Name MyPublicIp `
     -ResourceGroupName $RgName `
     -Location $Location `
     -AllocationMethod Static
    
    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
     -Name MyNic `
     -ResourceGroupName $RgName `
     -Location $Location `
     -SubnetId $Vnet.Subnets[0].Id `
     -PublicIpAddressId $Pip.Id `
     -EnableAcceleratedNetworking
    
    # Specify hello base image's VHD URI (from phase one step 5). 
    # Note: hello storage account of this base image vhd should have "Storage service encryption" disabled
    # See more from here: https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption
    # This is just an example URI, you will need tooreplace this when running this script
    $sourceUri="https://myexamplesa.blob.core.windows.net/vhds/CentOS73-Base-Test120170629111341.vhd" 

    # Specify a URI for hello location from which hello new image binary large object (BLOB) is copied toostart hello virtual machine. 
    # Must end with ".vhd" extension
    $OsDiskName = "MyOsDiskName.vhd" 
    $destOsDiskUri = "https://myexamplesa.blob.core.windows.net/vhds/" + $OsDiskName
    
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential
    
    # Create a custom virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
     -VMName MyVM -VMSize Standard_DS4_v2 | `
    Set-AzureRmVMOperatingSystem `
     -Linux `
     -ComputerName myVM `
     -Credential $Cred | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk `
     -Name $OsDiskName `
     -SourceImageUri $sourceUri `
     -VhdUri $destOsDiskUri `
     -CreateOption FromImage `
     -Linux
    
    # Create hello virtual machine.    
    New-AzureRmVM `
     -ResourceGroupName $RgName `
     -Location $Location `
     -VM $VmConfig
    ```

2.  <span data-ttu-id="7a7b1-288">När virtuella datorer starta hello VF enhet genom att ”lspci” och kontrollera hello Mellanox posten.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-288">After VMs boot up, check hello VF device by "lspci" and check hello Mellanox entry.</span></span> <span data-ttu-id="7a7b1-289">Vi bör till exempel se det här objektet i hello lspci utdata:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-289">For example, we should see this item in hello lspci output:</span></span>
    
    ```
    0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
    ```
    
3.  <span data-ttu-id="7a7b1-290">Kör hello partnerskap skript genom att:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-290">Run hello bonding script by:</span></span>

    ```bash
    sudo bondvf.sh
    ```

4.  <span data-ttu-id="7a7b1-291">Starta om hello nya virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="7a7b1-291">Reboot hello new VMs:</span></span>

    ```bash
    sudo reboot
    ```

<span data-ttu-id="7a7b1-292">hello VM ska starta med bond0 konfigurerats och hello snabbare nätverk sökväg aktiverad.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-292">hello VM should start with bond0 configured and hello Accelerated Networking path enabled.</span></span>  <span data-ttu-id="7a7b1-293">Kör `ifconfig` tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="7a7b1-293">Run `ifconfig` tooconfirm.</span></span>
