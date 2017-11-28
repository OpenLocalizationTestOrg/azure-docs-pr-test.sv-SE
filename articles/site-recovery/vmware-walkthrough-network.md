---
title: "aaaPlan nätverk för VMware tooAzure replikering | Microsoft Docs"
description: "Den här artikeln beskrivs planering krävs vid replikering av virtuella VMware-datorer tooAzure nätverk"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5578a0b4-0352-41c3-9cce-56beb17a4d97
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 2b4f385c768cc7f5e98abae0afb8258b00f3724f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-plan-networking-for-vmware-tooazure-replication"></a><span data-ttu-id="3a95c-103">Steg 4: Planera nätverk för VMware tooAzure replikering</span><span class="sxs-lookup"><span data-stu-id="3a95c-103">Step 4: Plan networking for VMware tooAzure replication</span></span>

<span data-ttu-id="3a95c-104">Den här artikeln sammanfattar nätverket några saker att tänka på när du replikera lokala virtuella VMware-datorer tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="3a95c-104">This article summarizes network planning considerations when replicating on-premises VMware VMs tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="3a95c-105">Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="3a95c-105">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="connect-tooreplica-vms"></a><span data-ttu-id="3a95c-106">Ansluta tooreplica virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3a95c-106">Connect tooreplica VMs</span></span>

<span data-ttu-id="3a95c-107">När du planerar din replikering och redundansstrategi, en av hello viktiga frågor är hur tooconnect toohello virtuella Azure-datorn efter redundans.</span><span class="sxs-lookup"><span data-stu-id="3a95c-107">When planning your replication and failover strategy, one of hello key questions is how tooconnect toohello Azure VM after failover.</span></span> <span data-ttu-id="3a95c-108">Det finns ett par alternativ när du utformar din strategi för nätverket för replik virtuella Azure-datorer:</span><span class="sxs-lookup"><span data-stu-id="3a95c-108">There are a couple of choices when designing your network strategy for replica Azure VMs:</span></span>

- <span data-ttu-id="3a95c-109">**Använd en annan IP-adress**: du kan välja toouse en annan IP-adressintervall för hello replikerade Virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="3a95c-109">**Use different IP address**: You can select toouse a different IP address range for hello replicated Azure VM network.</span></span> <span data-ttu-id="3a95c-110">I det här scenariot hello VM hämtar en ny IP-adress efter växling vid fel och en DNS-uppdatering krävs.</span><span class="sxs-lookup"><span data-stu-id="3a95c-110">In this scenario hello VM gets a new IP address after failover, and a DNS update is required.</span></span>
- <span data-ttu-id="3a95c-111">**Behålla samma IP-adress**: du kanske vill toouse hello samma IP-adressintervall som primär lokal webbplatsen för hello Azure-nätverket efter redundans.</span><span class="sxs-lookup"><span data-stu-id="3a95c-111">**Retain same IP address**: You might want toouse hello same IP address range as that in your primary on-premises site, for hello Azure network after failover.</span></span> <span data-ttu-id="3a95c-112">Att hålla hello samma IP-adresser förenklar hello återställning genom att minska nätverk problemen efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="3a95c-112">Keeping hello same IP addresses simplifies hello recovery by reducing network related issues after failover.</span></span> <span data-ttu-id="3a95c-113">Men när du replikerar tooAzure måste tooupdate vägar med hello nya plats hello IP-adresser efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="3a95c-113">However, when you're replicating tooAzure, you will need tooupdate routes with hello new location of hello IP addresses after failover.</span></span> 


## <a name="retain-ip-addresses"></a><span data-ttu-id="3a95c-114">Behåll IP-adresser</span><span class="sxs-lookup"><span data-stu-id="3a95c-114">Retain IP addresses</span></span>

<span data-ttu-id="3a95c-115">Site Recovery tillhandahåller hello kapaciteten tooretain fasta IP-adresser när växling tooAzure med ett undernät redundans.</span><span class="sxs-lookup"><span data-stu-id="3a95c-115">Site Recovery provides hello capability tooretain fixed IP addresses when failing over tooAzure, with a subnet failover.</span></span>

<span data-ttu-id="3a95c-116">Med undernätet redundans finns ett specifikt undernät på plats 1 eller 2 för platsen, men aldrig på båda platser samtidigt.</span><span class="sxs-lookup"><span data-stu-id="3a95c-116">With subnet failover, a specific subnet is present at Site 1 or Site 2, but never at both sites simultaneously.</span></span> <span data-ttu-id="3a95c-117">I ordning toomaintain hello IP-adressutrymme i hello händelse av en växling vid fel ordna du programmässigt för hello router infrastruktur toomove hello undernät från en plats tooanother.</span><span class="sxs-lookup"><span data-stu-id="3a95c-117">In order toomaintain hello IP address space in hello event of a failover, you programmatically arrange for hello router infrastructure toomove hello subnets from one site tooanother.</span></span> <span data-ttu-id="3a95c-118">Under växling vid fel associerade hello undernät flytta med hello skyddade virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="3a95c-118">During failover, hello subnets move with hello associated protected VMs.</span></span> <span data-ttu-id="3a95c-119">hello huvudsakliga Nackdelen är att i hello händelse av fel du toomove hello hela undernätet.</span><span class="sxs-lookup"><span data-stu-id="3a95c-119">hello main drawback is that in hello event of a failure, you have toomove hello whole subnet.</span></span>


### <a name="failover-example"></a><span data-ttu-id="3a95c-120">Exempel för växling vid fel</span><span class="sxs-lookup"><span data-stu-id="3a95c-120">Failover example</span></span>

<span data-ttu-id="3a95c-121">Nu ska vi titta på ett exempel på tooAzure för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="3a95c-121">Let's look at an example for failover tooAzure.</span></span>

- <span data-ttu-id="3a95c-122">Ett ficticious företag, Woodgrove Bank har en lokal infrastruktur som är värd för sina appar.</span><span class="sxs-lookup"><span data-stu-id="3a95c-122">A ficticious company, Woodgrove Bank, has an on-premises infrastructure hosting their business apps.</span></span> <span data-ttu-id="3a95c-123">Sina mobila program finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="3a95c-123">Their mobile applications are hosted on Azure.</span></span>
- <span data-ttu-id="3a95c-124">Anslutningen mellan Woodgrove Bank virtuella datorer i Azure och lokala servrar som en plats-till-plats (VPN)-anslutning mellan hello lokala edge nätverk och hello virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="3a95c-124">Connectivity between Woodgrove Bank VMs in Azure and on-premises servers is provided by a site-to-site (VPN) connection between hello on-premises edge network and hello Azure virtual network.</span></span>
- <span data-ttu-id="3a95c-125">Den här VPN-innebär att hello företagets virtuella nätverk i Azure visas som ett tillägg för sina lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="3a95c-125">This VPN means that hello company's virtual network in Azure appears as an extension of their on-premises network.</span></span>
- <span data-ttu-id="3a95c-126">Woodgrove vill toouse Site Recovery tooreplicate lokala arbetsbelastningar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="3a95c-126">Woodgrove wants toouse Site Recovery tooreplicate on-premises workloads tooAzure.</span></span>
 - <span data-ttu-id="3a95c-127">Woodgrove har toodeal med program och konfigurationer som är beroende av hårdkodade IP-adresser och därför behöver tooretain IP-adresser för sina program efter växling vid fel tooAzure.</span><span class="sxs-lookup"><span data-stu-id="3a95c-127">Woodgrove has toodeal with applications and configurations which depend on hard-coded IP addresses, and thus need tooretain IP addresses for their applications after failover tooAzure.</span></span>
 - <span data-ttu-id="3a95c-128">Woodgrove har tilldelats IP-adresser från intervallet 172.16.1.0/24 172.16.2.0/24 tooits resurser som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="3a95c-128">Woodgrove has assigned IP addresses from range 172.16.1.0/24, 172.16.2.0/24 tooits resources running in Azure.</span></span>


<span data-ttu-id="3a95c-129">Woodgrove toobe kan tooreplicate tooAzure dess virtuella datorer medan behålla hello IP-adresser, är här i vilka hello företaget måste toodo:</span><span class="sxs-lookup"><span data-stu-id="3a95c-129">For Woodgrove toobe able tooreplicate its VMS tooAzure while retaining hello IP addresses, here's what hello company needs toodo:</span></span>

1. <span data-ttu-id="3a95c-130">Skapa ett virtuellt Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="3a95c-130">Create an Azure virtual network.</span></span> <span data-ttu-id="3a95c-131">Det bör vara en förlängning av hello lokalt nätverk, så att program kan växla över sömlöst.</span><span class="sxs-lookup"><span data-stu-id="3a95c-131">It should be an extension of hello on-premises network, so that applications can fail over seamlessly.</span></span>
2. <span data-ttu-id="3a95c-132">Azure kan du tooadd plats-till-plats VPN-anslutning dessutom toopoint-till-plats-anslutning toohello virtuella nätverk som skapats i Azure.</span><span class="sxs-lookup"><span data-stu-id="3a95c-132">Azure allows you tooadd site-to-site VPN connectivity, in addition toopoint-to-site connectivity toohello virtual networks created in Azure.</span></span>
3. <span data-ttu-id="3a95c-133">När du konfigurerar hello plats-till-plats-anslutning i hello Azure nätverk, du kan vidarebefordra trafiken toohello lokal plats (lokalt nätverk) endast om hello IP-adressintervall skiljer sig från hello lokala IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="3a95c-133">When setting up hello site-to-site connection, in hello Azure network, you can route traffic toohello on-premises location (local-network) only if hello IP address range is different from hello on-premises IP address range.</span></span>
    - <span data-ttu-id="3a95c-134">Det beror på att Azure inte stöder sträckta undernät.</span><span class="sxs-lookup"><span data-stu-id="3a95c-134">This is because Azure doesn’t support stretched subnets.</span></span> <span data-ttu-id="3a95c-135">Så om du har undernät 192.168.1.0/24 lokalt kan du inte lägga till ett lokalt nätverk 192.168.1.0/24 i hello Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="3a95c-135">So if you have subnet 192.168.1.0/24 on-premises, you can’t add a local-network 192.168.1.0/24 in hello Azure network.</span></span>
    - <span data-ttu-id="3a95c-136">Detta är förväntat eftersom Azure inte vet att det finns inga aktiva virtuella datorer i hello undernät och hello undernätet skapas för katastrofåterställning endast.</span><span class="sxs-lookup"><span data-stu-id="3a95c-136">This is expected because Azure doesn’t know that there are no active VMs in hello subnet, and that hello subnet is being created for disaster recovery only.</span></span>
    - <span data-ttu-id="3a95c-137">toobe kan toocorrectly dirigera nätverkstrafik utanför en Azure-nätverk hello undernät i hello nätverk och hello lokala-nätverk får inte står i konflikt.</span><span class="sxs-lookup"><span data-stu-id="3a95c-137">toobe able toocorrectly route network traffic out of an Azure network hello subnets in hello network and hello local-network mustn't conflict.</span></span>

![Innan du undernät redundans](./media/site-recovery-network-design/network-design7.png)

### <a name="before-failover"></a><span data-ttu-id="3a95c-139">Innan du redundans</span><span class="sxs-lookup"><span data-stu-id="3a95c-139">Before failover</span></span>

1. <span data-ttu-id="3a95c-140">Skapa ytterligare ett nätverk (till exempel återställning nätverk).</span><span class="sxs-lookup"><span data-stu-id="3a95c-140">Create an additional network (for example Recovery Network).</span></span> <span data-ttu-id="3a95c-141">Detta är hello nätverk som redundansväxlade virtuella datorer skapas.</span><span class="sxs-lookup"><span data-stu-id="3a95c-141">This is hello network in which failed over VMs are created.</span></span>
2. <span data-ttu-id="3a95c-142">tooensure som hello IP-adress för en virtuell dator finns kvar efter en redundansväxling i hello VM Egenskaper > **konfigurera**, ange hello samma IP-adressen som hello VM har lokalt och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3a95c-142">tooensure that hello IP address for a VM is retained after a failover, in hello VM properties > **Configure**, specify hello same IP address that hello VM has on-premises, and click **Save**.</span></span>
3. <span data-ttu-id="3a95c-143">När hello VM växlas över, tilldelar Azure Site Recovery hello angivna IP-adressen tooit.</span><span class="sxs-lookup"><span data-stu-id="3a95c-143">When hello VM is failed over, Azure Site Recovery will assign hello provided IP address tooit.</span></span>

    ![Egenskaper för nätverk](./media/site-recovery-network-design/network-design8.png)

4. <span data-ttu-id="3a95c-145">När redundansväxlingen är utlösare utlöses och hello virtuella datorer skapas i Azure med hello som krävs för IP-adress, du kan ansluta toohello med en [Vnet tooVnet anslutning](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span><span class="sxs-lookup"><span data-stu-id="3a95c-145">After failover is trigger is triggered, and hello VMs are created in Azure with hello required IP address, you can connect toohello network using a [Vnet tooVnet connection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span></span> <span data-ttu-id="3a95c-146">Den här åtgärden kan skriptas.</span><span class="sxs-lookup"><span data-stu-id="3a95c-146">This action can be scripted.</span></span>
5. <span data-ttu-id="3a95c-147">Vägar måste korrekt ändrade toobe tooreflect som 192.168.1.0/24 har nu flyttats tooAzure.</span><span class="sxs-lookup"><span data-stu-id="3a95c-147">Routes need toobe appropriately modified, tooreflect that 192.168.1.0/24 has now moved tooAzure.</span></span>

    ![Efter undernät redundans](./media/site-recovery-network-design/network-design9.png)

### <a name="after-failover"></a><span data-ttu-id="3a95c-149">Efter växling vid fel</span><span class="sxs-lookup"><span data-stu-id="3a95c-149">After failover</span></span>

<span data-ttu-id="3a95c-150">Om du inte har ett Azure-nätverk som visas ovan, kan du skapa en plats-till-plats VPN-anslutning mellan den primära platsen och Azure, efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="3a95c-150">If you don't have an Azure network as illustrated above, you can create a site-to-site VPN connection between your primary site and Azure, after failover.</span></span>

## <a name="change-ip-addresses"></a><span data-ttu-id="3a95c-151">Ändra IP-adresser</span><span class="sxs-lookup"><span data-stu-id="3a95c-151">Change IP addresses</span></span>

<span data-ttu-id="3a95c-152">Detta [blogginlägget](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) förklarar hur tooset in hello Azure nätverksinfrastruktur när du inte behöver tooretain IP-adresser efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="3a95c-152">This [blog post](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) explains how tooset up hello Azure networking infrastructure when you don't need tooretain IP addresses after failover.</span></span> <span data-ttu-id="3a95c-153">Den börjar med en beskrivning av programmet, ser på hur tooset in nätverk lokalt och i Azure och avslutas med information om att köra redundansväxlingar.</span><span class="sxs-lookup"><span data-stu-id="3a95c-153">It starts with an application description, looks at how tooset up networking on-premises and in Azure, and concludes with information about running failovers.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="3a95c-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a95c-154">Next steps</span></span>

<span data-ttu-id="3a95c-155">Gå för[steg 5: Förbered Azure](vmware-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="3a95c-155">Go too[Step 5: Prepare Azure](vmware-walkthrough-prepare-azure.md)</span></span>
