---
title: "aaaNetwork mappning mellan två Azure-regioner i Azure Site Recovery | Microsoft Docs"
description: "Azure Site Recovery samordnar hello replikering, redundans och återställning av virtuella datorer och fysiska servrar. Läs mer om tooAzure för växling vid fel eller ett sekundärt datacenter."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: pratshar
ms.openlocfilehash: 4f80c44e3f94eaf446bc01a7041d91fe34aa78d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="network-mapping-between-two-azure-regions"></a><span data-ttu-id="1c8a4-104">Nätverksmappningen mellan två Azure-regioner</span><span class="sxs-lookup"><span data-stu-id="1c8a4-104">Network mapping between two Azure regions</span></span>


<span data-ttu-id="1c8a4-105">Den här artikeln beskriver hur toomap Azure virtuella nätverk för två Azure-regioner med varandra.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-105">This article describes how toomap Azure virtual networks of two Azure regions with each other.</span></span> <span data-ttu-id="1c8a4-106">Nätverksmappning garanterar att den replikerade virtuella datorn skapas i mål-hello Azure-region, även skapas på hello virtuellt nätverk som är mappade toovirtual nätverk av hello virtuella källdatorn.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-106">Network mapping ensures that when replicated virtual machine is created in hello target Azure region, it is created on hello virtual network that is mapped toovirtual network of hello source virtual machine.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="1c8a4-107">Krav</span><span class="sxs-lookup"><span data-stu-id="1c8a4-107">Prerequisites</span></span>
<span data-ttu-id="1c8a4-108">Innan du mappa nätverk gör att du har skapat [virtuella Azure-nätverk](../virtual-network/virtual-networks-overview.md) i både källa och mål-Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-108">Before you map networks make sure, you have created [Azure virtual networks](../virtual-network/virtual-networks-overview.md) in both source and target Azure regions.</span></span>

## <a name="map-networks"></a><span data-ttu-id="1c8a4-109">Mappa nätverk</span><span class="sxs-lookup"><span data-stu-id="1c8a4-109">Map networks</span></span>

<span data-ttu-id="1c8a4-110">toomap virtuellt Azure-nätverk i en Azure-region tooanother virtuellt nätverk i en annan region, gå tooSite Recovery-infrastruktur -> nätverk-mappning (för Azure virtuella datorer) och skapa en nätverksmappning.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-110">toomap an Azure virtual network in one Azure region tooanother virtual network in another region, go tooSite Recovery Infrastructure -> Network Mapping (For Azure Virtual Machines) and create a network mapping.</span></span>

![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


<span data-ttu-id="1c8a4-112">Exemplet nedan min virtuella dator körs i Östasien region och håller på att replikeras i hello tooSoutheast Asien.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-112">In hello example below my virtual machine is running in East Asia region and is being replicated tooSoutheast Asia.</span></span>

<span data-ttu-id="1c8a4-113">Välj hello käll- och nätverk och klicka sedan på OK toocreate nätverksmappning från Östasien tooSoutheast Asien.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-113">Select hello source and target network and then click OK toocreate a network mapping from East Asia tooSoutheast Asia.</span></span>

![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


<span data-ttu-id="1c8a4-115">Hello samma sak toocreate nätverksmappning från Sydostasien tooEast Asien.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-115">Do hello same thing toocreate a network mapping from Southeast Asia tooEast Asia.</span></span>  
![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="mapping-network-when-enabling-replication"></a><span data-ttu-id="1c8a4-117">Mappa nätverk när du aktiverar replikering</span><span class="sxs-lookup"><span data-stu-id="1c8a4-117">Mapping network when enabling replication</span></span>

<span data-ttu-id="1c8a4-118">Om nätverksmappning inte görs när du replikerar en virtuell dator för hello första gången formuläret en Azure-region tooanother kan du välja målnätverket som en del av hello samma process.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-118">If network mapping is not done when you are replicating a virtual machine for hello first time form one Azure region tooanother, then you can choose target network as part of hello same process.</span></span> <span data-ttu-id="1c8a4-119">Nätverksmappningar skapar site Recovery från källan region tootarget region och region toosource målregionen baserat på det här valet.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-119">Site Recovery creates network mappings from source region tootarget region and from target region toosource region based on this selection.</span></span>   

![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

<span data-ttu-id="1c8a4-121">Som standard skapar Site Recovery ett nätverk i hello målregionen som är identiska toohello källnätverket och genom att lägga till ”-asr' som en suffix toohello namnet på hello Källnätverk.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-121">By default, Site Recovery creates a network in hello target region that is identical toohello source network and by adding '-asr' as a suffix toohello name of hello source network.</span></span> <span data-ttu-id="1c8a4-122">Du kan välja ett nätverk som redan har skapat genom att klicka på Anpassa.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-122">You can choose an already created network by clicking Customize.</span></span>

![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)


<span data-ttu-id="1c8a4-124">Om hello nätverksmappning redan har gjort kan du inte ändra virtuella hello målnätverket vid replikering.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-124">If hello network mapping is already done, you can't change hello target virtual network while enabling replication.</span></span> <span data-ttu-id="1c8a4-125">toochange, ändra befintlig nätverksmappning.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-125">toochange it, modify existing network mapping.</span></span>  

![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Nätverksmappning](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> <span data-ttu-id="1c8a4-128">Om du ändrar en nätverksmappning från region-1 tooregion-2, kontrollera att du ändrar hello nätverksmappning från region-2-tooregion-1 samt.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-128">If you modify a network mapping from region-1 tooregion-2, make sure you modify hello network mapping from region-2 tooregion-1 as well.</span></span>
>
>


## <a name="subnet-selection"></a><span data-ttu-id="1c8a4-129">Val av undernät</span><span class="sxs-lookup"><span data-stu-id="1c8a4-129">Subnet selection</span></span>
<span data-ttu-id="1c8a4-130">Undernätet för hello virtuella måldatorn har valts baserat på hello namn för hello undernätet för hello virtuella källdatorn.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-130">Subnet of hello target virtual machine is selected based on hello name of hello subnet of hello source virtual machine.</span></span> <span data-ttu-id="1c8a4-131">Om det finns ett undernät för hello namn samma som hello virtuella källdatorn finns i hello målnätverket och sedan som väljs för hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-131">If there is a subnet of hello same name as that of hello source virtual machine available in hello target network, then that is chosen for hello target virtual machine.</span></span> <span data-ttu-id="1c8a4-132">Om det finns inget undernät med hello namn samma i hello målnätverket och sedan alfabetiskt första undernätet är valt som hello mål undernät.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-132">If there is no subnet with hello same name in hello target network, then alphabetically first subnet is chosen as hello target subnet.</span></span> <span data-ttu-id="1c8a4-133">Du kan ändra det här undernätet genom att gå tooCompute och nätverksinställningarna för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-133">You can modify this subnet by going tooCompute and Network settings of hello virtual machine.</span></span>

![Ändra ett undernät](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a><span data-ttu-id="1c8a4-135">IP-adress</span><span class="sxs-lookup"><span data-stu-id="1c8a4-135">IP address</span></span>

<span data-ttu-id="1c8a4-136">IP-adress för varje hello nätverksgränssnittet för hello virtuella måldatorn är valt på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1c8a4-136">IP address for each of hello network interface of hello target virtual machine is chosen as follows:</span></span>

### <a name="dhcp"></a><span data-ttu-id="1c8a4-137">DHCP</span><span class="sxs-lookup"><span data-stu-id="1c8a4-137">DHCP</span></span>
<span data-ttu-id="1c8a4-138">Om hello nätverksgränssnittet för hello virtuella källdatorn använder DHCP, anges också nätverksgränssnittet för hello virtuella måldatorn som DHCP.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-138">If hello network interface of hello source virtual machine is using DHCP, then network interface of hello target virtual machine is also set as DHCP.</span></span>

### <a name="static-ip"></a><span data-ttu-id="1c8a4-139">Statisk IP-adress</span><span class="sxs-lookup"><span data-stu-id="1c8a4-139">Static IP</span></span>
<span data-ttu-id="1c8a4-140">Om hello nätverksgränssnittet för hello virtuella källdatorn använder statisk IP-adress, anges toouse statiska IP-Adressen också i nätverksgränssnittet för hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-140">If hello network interface of hello source virtual machine is using Static IP, then network interface of hello target virtual machine is also set toouse Static IP.</span></span> <span data-ttu-id="1c8a4-141">Statiska IP-Adressen är valt på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1c8a4-141">Static IP is chosen as follows:</span></span>

#### <a name="same-address-space"></a><span data-ttu-id="1c8a4-142">Samma-adressutrymme</span><span class="sxs-lookup"><span data-stu-id="1c8a4-142">Same address space</span></span>

<span data-ttu-id="1c8a4-143">Om hello käll-undernät och undernät för hello mål har hello adressutrymmet samma mål-IP anges samma som hello IP för hello gränssnitt på hello virtuella källdatorn.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-143">If hello source subnet and hello target subnet have hello same address space, then target IP is set same as hello IP of  hello network interface of hello source virtual machine.</span></span> <span data-ttu-id="1c8a4-144">Om samma IP inte är tillgänglig, ange vissa tillgängliga IP som hello mål-IP.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-144">If same IP is not available, then some other available IP is set as hello target IP.</span></span>

#### <a name="different-address-space"></a><span data-ttu-id="1c8a4-145">Olika adressutrymmen</span><span class="sxs-lookup"><span data-stu-id="1c8a4-145">Different address space</span></span>

<span data-ttu-id="1c8a4-146">Om hello käll-undernät och undernät för hello mål har olika adressutrymmen, ange mål-IP som alla tillgängliga IP-adresser i hello mål undernät.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-146">If hello source subnet and hello target subnet have different address space, then target IP is set as any available IP in hello target subnet.</span></span>

<span data-ttu-id="1c8a4-147">Du kan ändra hello mål-IP-adress på varje gränssnitt genom att gå tooCompute och nätverksinställningarna för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1c8a4-147">You can modify hello target IP on each network interface by going tooCompute and Network settings of hello virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1c8a4-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1c8a4-148">Next steps</span></span>

- <span data-ttu-id="1c8a4-149">Lär dig mer om [nätverk vägledning för att replikera virtuella datorer i Azure](site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="1c8a4-149">Learn about [networking guidance for replicating Azure VMs](site-recovery-azure-to-azure-networking-guidance.md).</span></span>
