---
title: "Planera nätverk för Hyper-V (utan System Center VMM) till Azure replikering med Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskrivs planering av krävs när du replikerar virtuella Hyper-V-datorer (utan VMM) till Azure"
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
ms.openlocfilehash: 100b9d8a55c2c163e7a04680f0f7d7963315ee73
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="step-4-plan-networking-for-hyper-v-to-azure-replication"></a><span data-ttu-id="e8000-103">Steg 4: Planera nätverk för Hyper-V-replikering Azure</span><span class="sxs-lookup"><span data-stu-id="e8000-103">Step 4: Plan networking for Hyper-V to Azure replication</span></span>

<span data-ttu-id="e8000-104">Den här artikeln sammanfattar nätverket några saker att tänka på när du replikera lokala virtuella Hyper-V-datorer (utan System Center VMM) till Azure med hjälp av den [Azure Site Recovery](site-recovery-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="e8000-104">This article summarizes network planning considerations when replicating on-premises Hyper-V VMs (without System Center VMM) to Azure using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="e8000-105">Skriv dina kommentarer eller frågor längst ned i den här artikeln eller i [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="e8000-105">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="connecting-to-replica-vms-after-failover"></a><span data-ttu-id="e8000-106">Ansluter till Replikdatorerna efter växling vid fel</span><span class="sxs-lookup"><span data-stu-id="e8000-106">Connecting to replica VMs after failover</span></span>

<span data-ttu-id="e8000-107">När du planerar din replikering och redundansstrategi, är en av viktiga frågor hur du ansluter till den virtuella Azure-datorn efter redundans.</span><span class="sxs-lookup"><span data-stu-id="e8000-107">When planning your replication and failover strategy, one of the key questions is how to connect to the Azure VM after failover.</span></span> <span data-ttu-id="e8000-108">Det finns ett par alternativ när du utformar din strategi för nätverket för replik virtuella Azure-datorer:</span><span class="sxs-lookup"><span data-stu-id="e8000-108">There are a couple of choices when designing your network strategy for replica Azure VMs:</span></span>

- <span data-ttu-id="e8000-109">**Annan IP-adress**: du kan välja för att använda en annan IP-adressintervall för replikerade Virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="e8000-109">**Different IP address**: You can select to use a different IP address range for the replicated Azure VM network.</span></span> <span data-ttu-id="e8000-110">I det här scenariot den virtuella datorn får en ny IP-adress efter växling vid fel och en DNS-uppdatering krävs.</span><span class="sxs-lookup"><span data-stu-id="e8000-110">In this scenario the VM gets a new IP address after failover, and a DNS update is required.</span></span> [<span data-ttu-id="e8000-111">Läs mer</span><span class="sxs-lookup"><span data-stu-id="e8000-111">Learn more</span></span>](site-recovery-test-failover-vmm-to-vmm.md#prepare-the-infrastructure-for-test-failover)
- <span data-ttu-id="e8000-112">**Samma IP-adress**: du kanske vill använda samma IP-adressintervall som i ditt primära lokala nätverk för Azure-nätverket efter redundans.</span><span class="sxs-lookup"><span data-stu-id="e8000-112">**Same IP address**: You might want to use the same IP address range as that in your primary on-premises network, for the Azure network after failover.</span></span>  <span data-ttu-id="e8000-113">Behålla samma IP adresser förenklar återställning genom att minska nätverksrelaterade problem efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="e8000-113">Keeping the same IP addresses simplifies the recovery by reducing network related issues after failover.</span></span> <span data-ttu-id="e8000-114">Men när du replikerar till Azure måste uppdatera vägar med den nya platsen för IP-adresser efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="e8000-114">However, when you're replicating to Azure, you will need to update routes with the new location of the IP addresses after failover.</span></span>


## <a name="retain-ip-addresses"></a><span data-ttu-id="e8000-115">Behåll IP-adresser</span><span class="sxs-lookup"><span data-stu-id="e8000-115">Retain IP addresses</span></span>

<span data-ttu-id="e8000-116">Site Recovery tillhandahåller möjligheten att behålla fasta IP-adresser när du redundansväxlar till Azure, med ett undernät redundans.</span><span class="sxs-lookup"><span data-stu-id="e8000-116">Site Recovery provides the capability to retain fixed IP addresses when failing over to Azure, with a subnet failover.</span></span>

<span data-ttu-id="e8000-117">Med undernätet redundans finns ett specifikt undernät på plats 1 eller 2 för platsen, men aldrig på båda platser samtidigt.</span><span class="sxs-lookup"><span data-stu-id="e8000-117">With subnet failover, a specific subnet is present at Site 1 or Site 2, but never at both sites simultaneously.</span></span> <span data-ttu-id="e8000-118">För att upprätthålla IP-adressutrymme vid en växling vid fel, ordna du programmässigt router-infrastruktur att flytta undernäten från en plats till en annan.</span><span class="sxs-lookup"><span data-stu-id="e8000-118">In order to maintain the IP address space in the event of a failover, you programmatically arrange for the router infrastructure to move the subnets from one site to another.</span></span> <span data-ttu-id="e8000-119">Under växling vid fel flytta undernäten med de associera skyddade virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="e8000-119">During failover, the subnets move with the associated protected VMs.</span></span> <span data-ttu-id="e8000-120">Den största nackdelen är att vid fel, måste du flytta hela undernätet.</span><span class="sxs-lookup"><span data-stu-id="e8000-120">The main drawback is that in the event of a failure, you have to move the whole subnet.</span></span>


### <a name="failover-example"></a><span data-ttu-id="e8000-121">Exempel för växling vid fel</span><span class="sxs-lookup"><span data-stu-id="e8000-121">Failover example</span></span>

<span data-ttu-id="e8000-122">Nu ska vi titta på ett exempel för redundans till Azure.</span><span class="sxs-lookup"><span data-stu-id="e8000-122">Let's look at an example for failover to Azure.</span></span>

- <span data-ttu-id="e8000-123">Ett ficticious företag, Woodgrove Bank har en lokal infrastruktur som är värd för sina appar.</span><span class="sxs-lookup"><span data-stu-id="e8000-123">A ficticious company, Woodgrove Bank, has an on-premises infrastructure hosting their business apps.</span></span> <span data-ttu-id="e8000-124">Sina mobila program finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="e8000-124">Their mobile applications are hosted on Azure.</span></span>
- <span data-ttu-id="e8000-125">Anslutningen mellan Woodgrove Bank virtuella datorer i Azure och lokala servrar som en plats-till-plats (VPN)-anslutning mellan lokala edge nätverk och virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="e8000-125">Connectivity between Woodgrove Bank VMs in Azure and on-premises servers is provided by a site-to-site (VPN) connection between the on-premises edge network and the Azure virtual network.</span></span>
- <span data-ttu-id="e8000-126">Den här VPN innebär att företagets virtuella nätverk i Azure visas som ett tillägg för sina lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="e8000-126">This VPN means that the company's virtual network in Azure appears as an extension of their on-premises network.</span></span>
- <span data-ttu-id="e8000-127">Woodgrove vill använda Site Recovery replikera lokala arbetsbelastningar till Azure.</span><span class="sxs-lookup"><span data-stu-id="e8000-127">Woodgrove wants to use Site Recovery to replicate on-premises workloads to Azure.</span></span>
 - <span data-ttu-id="e8000-128">Woodgrove måste hantera program och konfigurationer som är beroende av hårdkodade IP-adresser och måste därför behålla IP-adresser för sina program efter en redundansväxling till Azure.</span><span class="sxs-lookup"><span data-stu-id="e8000-128">Woodgrove has to deal with applications and configurations which depend on hard-coded IP addresses, and thus need to retain IP addresses for their applications after failover to Azure.</span></span>
 - <span data-ttu-id="e8000-129">Woodgrove har tilldelats IP-adresser från intervallet 172.16.1.0/24, 172.16.2.0/24 till dess resurser som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="e8000-129">Woodgrove has assigned IP addresses from range 172.16.1.0/24, 172.16.2.0/24 to its resources running in Azure.</span></span>


<span data-ttu-id="e8000-130">För Woodgrove ska kunna behöver replikera dess virtuella datorer till Azure samtidigt som de IP-adresserna, härs vad företaget göra:</span><span class="sxs-lookup"><span data-stu-id="e8000-130">For Woodgrove to be able to replicate its VMs to Azure while retaining the IP addresses, here's what the company needs to do:</span></span>

1. <span data-ttu-id="e8000-131">Skapa ett virtuellt Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="e8000-131">Create an Azure virtual network.</span></span> <span data-ttu-id="e8000-132">Det bör vara ett tillägg till det lokala nätverket, så att program kan växla över sömlöst.</span><span class="sxs-lookup"><span data-stu-id="e8000-132">It should be an extension of the on-premises network, so that applications can fail over seamlessly.</span></span>
2. <span data-ttu-id="e8000-133">Azure kan du lägga till plats-till-plats VPN-anslutning förutom punkt-till-plats-anslutning till virtuella nätverk som skapats i Azure.</span><span class="sxs-lookup"><span data-stu-id="e8000-133">Azure allows you to add site-to-site VPN connectivity, in addition to point-to-site connectivity to the virtual networks created in Azure.</span></span>
3. <span data-ttu-id="e8000-134">När du ställer in plats-till-plats-anslutningen i Azure-nätverk kan du dirigera trafik till lokal plats (lokalt nätverk) bara om IP-adressintervallet skiljer sig från det lokala IP-adressintervallet.</span><span class="sxs-lookup"><span data-stu-id="e8000-134">When setting up the site-to-site connection, in the Azure network, you can route traffic to the on-premises location (local-network) only if the IP address range is different from the on-premises IP address range.</span></span>
    - <span data-ttu-id="e8000-135">Det beror på att Azure inte stöder sträckta undernät.</span><span class="sxs-lookup"><span data-stu-id="e8000-135">This is because Azure doesn’t support stretched subnets.</span></span> <span data-ttu-id="e8000-136">Så om du har undernät 192.168.1.0/24 lokalt kan du inte lägga till 192.168.1.0/24 ett lokalt nätverk i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="e8000-136">So if you have subnet 192.168.1.0/24 on-premises, you can’t add a local-network 192.168.1.0/24 in the Azure network.</span></span>
    - <span data-ttu-id="e8000-137">Detta är förväntat eftersom Azure inte vet att det inte finns några aktiva virtuella datorer i undernät och undernätet skapas för endast katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="e8000-137">This is expected because Azure doesn’t know that there are no active VMs in the subnet, and that the subnet is being created for disaster recovery only.</span></span>
    - <span data-ttu-id="e8000-138">Om du vill kunna korrekt undernät i nätverket och det lokala nätverket dirigera nätverkstrafik utanför ett Azure-nätverk får inte är i konflikt.</span><span class="sxs-lookup"><span data-stu-id="e8000-138">To be able to correctly route network traffic out of an Azure network the subnets in the network and the local-network mustn't conflict.</span></span>

![Innan du undernät redundans](./media/hyper-v-site-walkthrough-network/network-design7.png)

### <a name="before-failover"></a><span data-ttu-id="e8000-140">Innan du redundans</span><span class="sxs-lookup"><span data-stu-id="e8000-140">Before failover</span></span>

1. <span data-ttu-id="e8000-141">Skapa ytterligare ett nätverk (till exempel återställning nätverk).</span><span class="sxs-lookup"><span data-stu-id="e8000-141">Create an additional network (for example Recovery Network).</span></span> <span data-ttu-id="e8000-142">Det här är nätverket som redundansväxlade virtuella datorer skapas.</span><span class="sxs-lookup"><span data-stu-id="e8000-142">This is the network in which failed over VMs are created.</span></span>
2. <span data-ttu-id="e8000-143">Att se till att IP-adressen för en virtuell dator finns kvar efter en redundansväxling i egenskaperna för virtuell dator > **konfigurera**, ange samma IP-adress som den virtuella datorn har lokalt och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="e8000-143">To ensure that the IP address for a VM is retained after a failover, in the VM properties > **Configure**, specify the same IP address that the VM has on-premises, and click **Save**.</span></span>
3. <span data-ttu-id="e8000-144">När den virtuella datorn har redundansväxlats tilldelas den angivna IP-adressen till den i Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="e8000-144">When the VM is failed over, Azure Site Recovery will assign the provided IP address to it.</span></span>

    ![Egenskaper för nätverk](./media/hyper-v-site-walkthrough-network/network-design8.png)

4. <span data-ttu-id="e8000-146">När redundansväxlingen är utlösare utlöses och de virtuella datorerna ska skapas i Azure med nödvändiga IP-adress, du kan ansluta till nätverket med en [Vnet till Vnet vonnection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span><span class="sxs-lookup"><span data-stu-id="e8000-146">After failover is trigger is triggered, and the VMs are created in Azure with the required IP address, you can connect to the network using a [Vnet to Vnet vonnection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span></span> <span data-ttu-id="e8000-147">Den här åtgärden kan skriptas.</span><span class="sxs-lookup"><span data-stu-id="e8000-147">This action can be scripted.</span></span>
5. <span data-ttu-id="e8000-148">Vägar behöver ändras på lämpligt sätt så att den återger den 192.168.1.0/24 har nu flyttats till Azure.</span><span class="sxs-lookup"><span data-stu-id="e8000-148">Routes need to be appropriately modified, to reflect that 192.168.1.0/24 has now moved to Azure.</span></span>

    ![Efter undernät redundans](./media/hyper-v-site-walkthrough-network/network-design9.png)

### <a name="after-failover"></a><span data-ttu-id="e8000-150">Efter växling vid fel</span><span class="sxs-lookup"><span data-stu-id="e8000-150">After failover</span></span>

<span data-ttu-id="e8000-151">Om du inte har ett Azure-nätverk som visas ovan, kan du skapa en plats-till-plats VPN-anslutning mellan den primära platsen och Azure, när failvoer.</span><span class="sxs-lookup"><span data-stu-id="e8000-151">If you don't have an Azure network as illustrated above, you can create a site-to-site VPN connection between your primary site and Azure, after failvoer.</span></span>

## <a name="change-ip-addresses"></a><span data-ttu-id="e8000-152">Ändra IP-adresser</span><span class="sxs-lookup"><span data-stu-id="e8000-152">Change IP addresses</span></span>

<span data-ttu-id="e8000-153">Detta [blogginlägget](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) förklarar hur du ställer in Azure nätverksinfrastrukturen när du inte behöver behålla IP-adresser efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="e8000-153">This [blog post](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) explains how to set up the Azure networking infrastructure when you don't need to retain IP addresses after failover.</span></span> <span data-ttu-id="e8000-154">Den börjar med en beskrivning av programmet, ta en titt på hur du ställer in nätverk lokalt och i Azure och avslutas med information om att köra redundansväxlingar.</span><span class="sxs-lookup"><span data-stu-id="e8000-154">It starts with an application description, looks at how to set up networking on-premises and in Azure, and concludes with information about running failovers.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="e8000-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e8000-155">Next steps</span></span>

<span data-ttu-id="e8000-156">Gå till [steg 5: Förbered Azure](hyper-v-site-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="e8000-156">Go to [Step 5: Prepare Azure](hyper-v-site-walkthrough-prepare-azure.md)</span></span>
