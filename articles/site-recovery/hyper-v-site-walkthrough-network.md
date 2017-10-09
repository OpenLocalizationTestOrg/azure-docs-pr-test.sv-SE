---
title: "aaaPlan nätverk för Hyper-V (utan VMM för System Center) tooAzure replikering med Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskrivs planering krävs när du replikerar virtuella Hyper-V-datorer (utan VMM) tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5ca12254-975d-48e8-a84d-422825dac9b2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: a35de72b53ca921a7b9bfca00a0edcb9416d5aa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-plan-networking-for-hyper-v-tooazure-replication"></a><span data-ttu-id="7319a-103">Steg 4: Planera nätverk för Hyper-V tooAzure replikering</span><span class="sxs-lookup"><span data-stu-id="7319a-103">Step 4: Plan networking for Hyper-V tooAzure replication</span></span>

<span data-ttu-id="7319a-104">Den här artikeln sammanfattar nätverket några saker att tänka på när du replikera lokala virtuella Hyper-V-datorer (utan VMM för System Center) tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="7319a-104">This article summarizes network planning considerations when replicating on-premises Hyper-V VMs (without System Center VMM) tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="7319a-105">Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="7319a-105">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="connecting-tooreplica-vms-after-failover"></a><span data-ttu-id="7319a-106">Ansluta tooreplica virtuella datorer efter redundans</span><span class="sxs-lookup"><span data-stu-id="7319a-106">Connecting tooreplica VMs after failover</span></span>

<span data-ttu-id="7319a-107">När du planerar din replikering och redundansstrategi, en av hello viktiga frågor är hur tooconnect toohello virtuella Azure-datorn efter redundans.</span><span class="sxs-lookup"><span data-stu-id="7319a-107">When planning your replication and failover strategy, one of hello key questions is how tooconnect toohello Azure VM after failover.</span></span> <span data-ttu-id="7319a-108">Det finns ett par alternativ när du utformar din strategi för nätverket för replik virtuella Azure-datorer:</span><span class="sxs-lookup"><span data-stu-id="7319a-108">There are a couple of choices when designing your network strategy for replica Azure VMs:</span></span>

- <span data-ttu-id="7319a-109">**Annan IP-adress**: du kan välja toouse en annan IP-adressintervall för hello replikerade Virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="7319a-109">**Different IP address**: You can select toouse a different IP address range for hello replicated Azure VM network.</span></span> <span data-ttu-id="7319a-110">I det här scenariot hello VM hämtar en ny IP-adress efter växling vid fel och en DNS-uppdatering krävs.</span><span class="sxs-lookup"><span data-stu-id="7319a-110">In this scenario hello VM gets a new IP address after failover, and a DNS update is required.</span></span> [<span data-ttu-id="7319a-111">Läs mer</span><span class="sxs-lookup"><span data-stu-id="7319a-111">Learn more</span></span>](site-recovery-test-failover-vmm-to-vmm.md#prepare-the-infrastructure-for-test-failover)
- <span data-ttu-id="7319a-112">**Samma IP-adress**: du kanske vill toouse hello samma IP-adressintervall som som i din primära lokala nätverk, för hello Azure-nätverket efter redundans.</span><span class="sxs-lookup"><span data-stu-id="7319a-112">**Same IP address**: You might want toouse hello same IP address range as that in your primary on-premises network, for hello Azure network after failover.</span></span>  <span data-ttu-id="7319a-113">Att hålla hello samma IP-adresser förenklar hello återställning genom att minska nätverk problemen efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="7319a-113">Keeping hello same IP addresses simplifies hello recovery by reducing network related issues after failover.</span></span> <span data-ttu-id="7319a-114">Men när du replikerar tooAzure måste tooupdate vägar med hello nya plats hello IP-adresser efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="7319a-114">However, when you're replicating tooAzure, you will need tooupdate routes with hello new location of hello IP addresses after failover.</span></span>


## <a name="retain-ip-addresses"></a><span data-ttu-id="7319a-115">Behåll IP-adresser</span><span class="sxs-lookup"><span data-stu-id="7319a-115">Retain IP addresses</span></span>

<span data-ttu-id="7319a-116">Site Recovery tillhandahåller hello kapaciteten tooretain fasta IP-adresser när växling tooAzure med ett undernät redundans.</span><span class="sxs-lookup"><span data-stu-id="7319a-116">Site Recovery provides hello capability tooretain fixed IP addresses when failing over tooAzure, with a subnet failover.</span></span>

<span data-ttu-id="7319a-117">Med undernätet redundans finns ett specifikt undernät på plats 1 eller 2 för platsen, men aldrig på båda platser samtidigt.</span><span class="sxs-lookup"><span data-stu-id="7319a-117">With subnet failover, a specific subnet is present at Site 1 or Site 2, but never at both sites simultaneously.</span></span> <span data-ttu-id="7319a-118">I ordning toomaintain hello IP-adressutrymme i hello händelse av en växling vid fel ordna du programmässigt för hello router infrastruktur toomove hello undernät från en plats tooanother.</span><span class="sxs-lookup"><span data-stu-id="7319a-118">In order toomaintain hello IP address space in hello event of a failover, you programmatically arrange for hello router infrastructure toomove hello subnets from one site tooanother.</span></span> <span data-ttu-id="7319a-119">Under växling vid fel associerade hello undernät flytta med hello skyddade virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="7319a-119">During failover, hello subnets move with hello associated protected VMs.</span></span> <span data-ttu-id="7319a-120">hello huvudsakliga Nackdelen är att i hello händelse av fel du toomove hello hela undernätet.</span><span class="sxs-lookup"><span data-stu-id="7319a-120">hello main drawback is that in hello event of a failure, you have toomove hello whole subnet.</span></span>


### <a name="failover-example"></a><span data-ttu-id="7319a-121">Exempel för växling vid fel</span><span class="sxs-lookup"><span data-stu-id="7319a-121">Failover example</span></span>

<span data-ttu-id="7319a-122">Nu ska vi titta på ett exempel på tooAzure för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="7319a-122">Let's look at an example for failover tooAzure.</span></span>

- <span data-ttu-id="7319a-123">Ett ficticious företag, Woodgrove Bank har en lokal infrastruktur som är värd för sina appar.</span><span class="sxs-lookup"><span data-stu-id="7319a-123">A ficticious company, Woodgrove Bank, has an on-premises infrastructure hosting their business apps.</span></span> <span data-ttu-id="7319a-124">Sina mobila program finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="7319a-124">Their mobile applications are hosted on Azure.</span></span>
- <span data-ttu-id="7319a-125">Anslutningen mellan Woodgrove Bank virtuella datorer i Azure och lokala servrar som en plats-till-plats (VPN)-anslutning mellan hello lokala edge nätverk och hello virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="7319a-125">Connectivity between Woodgrove Bank VMs in Azure and on-premises servers is provided by a site-to-site (VPN) connection between hello on-premises edge network and hello Azure virtual network.</span></span>
- <span data-ttu-id="7319a-126">Den här VPN-innebär att hello företagets virtuella nätverk i Azure visas som ett tillägg för sina lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="7319a-126">This VPN means that hello company's virtual network in Azure appears as an extension of their on-premises network.</span></span>
- <span data-ttu-id="7319a-127">Woodgrove vill toouse Site Recovery tooreplicate lokala arbetsbelastningar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7319a-127">Woodgrove wants toouse Site Recovery tooreplicate on-premises workloads tooAzure.</span></span>
 - <span data-ttu-id="7319a-128">Woodgrove har toodeal med program och konfigurationer som är beroende av hårdkodade IP-adresser och därför behöver tooretain IP-adresser för sina program efter växling vid fel tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7319a-128">Woodgrove has toodeal with applications and configurations which depend on hard-coded IP addresses, and thus need tooretain IP addresses for their applications after failover tooAzure.</span></span>
 - <span data-ttu-id="7319a-129">Woodgrove har tilldelats IP-adresser från intervallet 172.16.1.0/24 172.16.2.0/24 tooits resurser som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="7319a-129">Woodgrove has assigned IP addresses from range 172.16.1.0/24, 172.16.2.0/24 tooits resources running in Azure.</span></span>


<span data-ttu-id="7319a-130">Woodgrove toobe kan tooreplicate tooAzure dess virtuella datorer medan behålla hello IP-adresser, är här i vilka hello företaget måste toodo:</span><span class="sxs-lookup"><span data-stu-id="7319a-130">For Woodgrove toobe able tooreplicate its VMs tooAzure while retaining hello IP addresses, here's what hello company needs toodo:</span></span>

1. <span data-ttu-id="7319a-131">Skapa ett virtuellt Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="7319a-131">Create an Azure virtual network.</span></span> <span data-ttu-id="7319a-132">Det bör vara en förlängning av hello lokalt nätverk, så att program kan växla över sömlöst.</span><span class="sxs-lookup"><span data-stu-id="7319a-132">It should be an extension of hello on-premises network, so that applications can fail over seamlessly.</span></span>
2. <span data-ttu-id="7319a-133">Azure kan du tooadd plats-till-plats VPN-anslutning dessutom toopoint-till-plats-anslutning toohello virtuella nätverk som skapats i Azure.</span><span class="sxs-lookup"><span data-stu-id="7319a-133">Azure allows you tooadd site-to-site VPN connectivity, in addition toopoint-to-site connectivity toohello virtual networks created in Azure.</span></span>
3. <span data-ttu-id="7319a-134">När du konfigurerar hello plats-till-plats-anslutning i hello Azure nätverk, du kan vidarebefordra trafiken toohello lokal plats (lokalt nätverk) endast om hello IP-adressintervall skiljer sig från hello lokala IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="7319a-134">When setting up hello site-to-site connection, in hello Azure network, you can route traffic toohello on-premises location (local-network) only if hello IP address range is different from hello on-premises IP address range.</span></span>
    - <span data-ttu-id="7319a-135">Det beror på att Azure inte stöder sträckta undernät.</span><span class="sxs-lookup"><span data-stu-id="7319a-135">This is because Azure doesn’t support stretched subnets.</span></span> <span data-ttu-id="7319a-136">Så om du har undernät 192.168.1.0/24 lokalt kan du inte lägga till ett lokalt nätverk 192.168.1.0/24 i hello Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="7319a-136">So if you have subnet 192.168.1.0/24 on-premises, you can’t add a local-network 192.168.1.0/24 in hello Azure network.</span></span>
    - <span data-ttu-id="7319a-137">Detta är förväntat eftersom Azure inte vet att det finns inga aktiva virtuella datorer i hello undernät och hello undernätet skapas för katastrofåterställning endast.</span><span class="sxs-lookup"><span data-stu-id="7319a-137">This is expected because Azure doesn’t know that there are no active VMs in hello subnet, and that hello subnet is being created for disaster recovery only.</span></span>
    - <span data-ttu-id="7319a-138">toobe kan toocorrectly dirigera nätverkstrafik utanför en Azure-nätverk hello undernät i hello nätverk och hello lokala-nätverk får inte står i konflikt.</span><span class="sxs-lookup"><span data-stu-id="7319a-138">toobe able toocorrectly route network traffic out of an Azure network hello subnets in hello network and hello local-network mustn't conflict.</span></span>

![Innan du undernät redundans](./media/hyper-v-site-walkthrough-network/network-design7.png)

### <a name="before-failover"></a><span data-ttu-id="7319a-140">Innan du redundans</span><span class="sxs-lookup"><span data-stu-id="7319a-140">Before failover</span></span>

1. <span data-ttu-id="7319a-141">Skapa ytterligare ett nätverk (till exempel återställning nätverk).</span><span class="sxs-lookup"><span data-stu-id="7319a-141">Create an additional network (for example Recovery Network).</span></span> <span data-ttu-id="7319a-142">Detta är hello nätverk som redundansväxlade virtuella datorer skapas.</span><span class="sxs-lookup"><span data-stu-id="7319a-142">This is hello network in which failed over VMs are created.</span></span>
2. <span data-ttu-id="7319a-143">tooensure som hello IP-adress för en virtuell dator finns kvar efter en redundansväxling i hello VM Egenskaper > **konfigurera**, ange hello samma IP-adressen som hello VM har lokalt och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="7319a-143">tooensure that hello IP address for a VM is retained after a failover, in hello VM properties > **Configure**, specify hello same IP address that hello VM has on-premises, and click **Save**.</span></span>
3. <span data-ttu-id="7319a-144">När hello VM växlas över, tilldelar Azure Site Recovery hello angivna IP-adressen tooit.</span><span class="sxs-lookup"><span data-stu-id="7319a-144">When hello VM is failed over, Azure Site Recovery will assign hello provided IP address tooit.</span></span>

    ![Egenskaper för nätverk](./media/hyper-v-site-walkthrough-network/network-design8.png)

4. <span data-ttu-id="7319a-146">När redundansväxlingen är utlösare utlöses och hello virtuella datorer skapas i Azure med hello som krävs för IP-adress, du kan ansluta toohello med en [Vnet tooVnet vonnection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span><span class="sxs-lookup"><span data-stu-id="7319a-146">After failover is trigger is triggered, and hello VMs are created in Azure with hello required IP address, you can connect toohello network using a [Vnet tooVnet vonnection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span></span> <span data-ttu-id="7319a-147">Den här åtgärden kan skriptas.</span><span class="sxs-lookup"><span data-stu-id="7319a-147">This action can be scripted.</span></span>
5. <span data-ttu-id="7319a-148">Vägar måste korrekt ändrade toobe tooreflect som 192.168.1.0/24 har nu flyttats tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7319a-148">Routes need toobe appropriately modified, tooreflect that 192.168.1.0/24 has now moved tooAzure.</span></span>

    ![Efter undernät redundans](./media/hyper-v-site-walkthrough-network/network-design9.png)

### <a name="after-failover"></a><span data-ttu-id="7319a-150">Efter växling vid fel</span><span class="sxs-lookup"><span data-stu-id="7319a-150">After failover</span></span>

<span data-ttu-id="7319a-151">Om du inte har ett Azure-nätverk som visas ovan, kan du skapa en plats-till-plats VPN-anslutning mellan den primära platsen och Azure, när failvoer.</span><span class="sxs-lookup"><span data-stu-id="7319a-151">If you don't have an Azure network as illustrated above, you can create a site-to-site VPN connection between your primary site and Azure, after failvoer.</span></span>

## <a name="change-ip-addresses"></a><span data-ttu-id="7319a-152">Ändra IP-adresser</span><span class="sxs-lookup"><span data-stu-id="7319a-152">Change IP addresses</span></span>

<span data-ttu-id="7319a-153">Detta [blogginlägget](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) förklarar hur tooset in hello Azure nätverksinfrastruktur när du inte behöver tooretain IP-adresser efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="7319a-153">This [blog post](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) explains how tooset up hello Azure networking infrastructure when you don't need tooretain IP addresses after failover.</span></span> <span data-ttu-id="7319a-154">Den börjar med en beskrivning av programmet, ser på hur tooset in nätverk lokalt och i Azure och avslutas med information om att köra redundansväxlingar.</span><span class="sxs-lookup"><span data-stu-id="7319a-154">It starts with an application description, looks at how tooset up networking on-premises and in Azure, and concludes with information about running failovers.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="7319a-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7319a-155">Next steps</span></span>

<span data-ttu-id="7319a-156">Gå för[steg 5: Förbered Azure](hyper-v-site-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="7319a-156">Go too[Step 5: Prepare Azure](hyper-v-site-walkthrough-prepare-azure.md)</span></span>
