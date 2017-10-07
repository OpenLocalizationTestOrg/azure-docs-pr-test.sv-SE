---
title: "aaaSQL Server-Tillgänglighetsgrupper - virtuella datorer i Azure - katastrofåterställning | Microsoft Docs"
description: "Den här artikeln förklarar hur tooconfigure en SQL Server-tillgänglighet gruppen på virtuella Azure-datorer med en replik i en annan region."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 388c464e-a16e-4c9d-a0d5-bb7cf5974689
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: df6238dc61c5a56879c75c9bf7314c618f43c0ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-always-on-availability-group-on-azure-virtual-machines-in-different-regions"></a><span data-ttu-id="39aed-103">Konfigurera en Always On-tillgänglighetsgrupp på virtuella Azure-datorer i olika regioner</span><span class="sxs-lookup"><span data-stu-id="39aed-103">Configure an Always On availability group on Azure virtual machines in different regions</span></span>

<span data-ttu-id="39aed-104">Den här artikeln förklarar hur tooconfigure SQL Server Always On availability gruppera repliken på virtuella Azure-datorer på en fjärrplats för Azure.</span><span class="sxs-lookup"><span data-stu-id="39aed-104">This article explains how tooconfigure a SQL Server Always On availability group replica on Azure virtual machines in a remote Azure location.</span></span> <span data-ttu-id="39aed-105">Använd den här konfigurationen toosupport katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="39aed-105">Use this configuration toosupport disaster recovery.</span></span>

<span data-ttu-id="39aed-106">Den här artikeln gäller tooAzure virtuella datorer i Resource Manager-läge.</span><span class="sxs-lookup"><span data-stu-id="39aed-106">This article applies tooAzure Virtual Machines in Resource Manager mode.</span></span>

<span data-ttu-id="39aed-107">hello följande bild visar en gemensam distribution av en tillgänglighetsgrupp på virtuella Azure-datorer:</span><span class="sxs-lookup"><span data-stu-id="39aed-107">hello following image shows a common deployment of an availability group on Azure virtual machines:</span></span>

   ![Tillgänglighetsgruppen](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic.png)

<span data-ttu-id="39aed-109">I den här distributionen är alla virtuella datorer i en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="39aed-109">In this deployment, all virtual machines are in one Azure region.</span></span> <span data-ttu-id="39aed-110">Hej tillgänglighetsgrupprepliker kan ha synkront genomförande med automatisk redundans på SQL-1 och SQL-2.</span><span class="sxs-lookup"><span data-stu-id="39aed-110">hello availability group replicas can have synchronous commit with automatic failover on SQL-1 and SQL-2.</span></span> <span data-ttu-id="39aed-111">toobuild den här arkitekturen finns [Tillgänglighetsgruppen mall eller kursen](virtual-machines-windows-portal-sql-availability-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="39aed-111">toobuild this architecture, see [Availability Group template or tutorial](virtual-machines-windows-portal-sql-availability-group-overview.md).</span></span>

<span data-ttu-id="39aed-112">Den här arkitekturen är sårbar toodowntime om hello Azure-region blir otillgänglig.</span><span class="sxs-lookup"><span data-stu-id="39aed-112">This architecture is vulnerable toodowntime if hello Azure region becomes inaccessible.</span></span> <span data-ttu-id="39aed-113">tooovercome detta problem, lägga till en replik i en annan Azure-region.</span><span class="sxs-lookup"><span data-stu-id="39aed-113">tooovercome this vulnerability, add a replica in a different Azure region.</span></span> <span data-ttu-id="39aed-114">hello följande diagram visar hur hello ny arkitektur skulle se ut:</span><span class="sxs-lookup"><span data-stu-id="39aed-114">hello following diagram shows how hello new architecture would look:</span></span>

   ![Tillgänglighet grupp Katastrofåterställning](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic-dr.png)

<span data-ttu-id="39aed-116">hello visas föregående diagram en ny virtuell dator som kallas SQL-3.</span><span class="sxs-lookup"><span data-stu-id="39aed-116">hello preceding diagram shows a new virtual machine called SQL-3.</span></span> <span data-ttu-id="39aed-117">SQL-3 är i en annan Azure-region.</span><span class="sxs-lookup"><span data-stu-id="39aed-117">SQL-3 is in a different Azure region.</span></span> <span data-ttu-id="39aed-118">SQL-3 läggs toohello redundanskluster i Windows Server.</span><span class="sxs-lookup"><span data-stu-id="39aed-118">SQL-3 is added toohello Windows Server Failover Cluster.</span></span> <span data-ttu-id="39aed-119">SQL-3 kan vara värd för en tillgänglighetsreplik för gruppen.</span><span class="sxs-lookup"><span data-stu-id="39aed-119">SQL-3 can host an availability group replica.</span></span> <span data-ttu-id="39aed-120">Slutligen, Observera att hello Azure-region för SQL-3 har en ny Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="39aed-120">Finally, notice that hello Azure region for SQL-3 has a new Azure load balancer.</span></span>

>[!NOTE]
> <span data-ttu-id="39aed-121">Tillgänglighetsuppsättningen Azure krävs när mer än en virtuell dator är i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="39aed-121">An Azure availability set is required when more than one virtual machine is in hello same region.</span></span> <span data-ttu-id="39aed-122">Om bara en virtuell dator är i hello region, och sedan hello tillgänglighetsuppsättning inte krävs.</span><span class="sxs-lookup"><span data-stu-id="39aed-122">If only one virtual machine is in hello region, then hello availability set is not required.</span></span> <span data-ttu-id="39aed-123">Du kan bara placera en virtuell dator i en tillgänglighetsuppsättning vid skapandet.</span><span class="sxs-lookup"><span data-stu-id="39aed-123">You can only place a virtual machine in an availability set at creation time.</span></span> <span data-ttu-id="39aed-124">Hello virtuella datorn finns redan i en tillgänglighetsuppsättning, kan du lägga till en virtuell dator för ytterligare en replik senare.</span><span class="sxs-lookup"><span data-stu-id="39aed-124">If hello virtual machine is already in an availability set, you can add a virtual machine for an additional replica later.</span></span>

<span data-ttu-id="39aed-125">I den här arkitekturen konfigureras normalt hello repliken i hello remote region med tillgänglighetsläget för asynkront genomförande och manuellt redundansläge.</span><span class="sxs-lookup"><span data-stu-id="39aed-125">In this architecture, hello replica in hello remote region is normally configured with asynchronous commit availability mode and manual failover mode.</span></span>

<span data-ttu-id="39aed-126">När tillgänglighetsgrupprepliker finns på virtuella Azure-datorer i olika Azure-regioner, kräver varje region:</span><span class="sxs-lookup"><span data-stu-id="39aed-126">When availability group replicas are on Azure virtual machines in different Azure regions, each region requires:</span></span>

* <span data-ttu-id="39aed-127">En virtuell nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="39aed-127">A virtual network gateway</span></span>
* <span data-ttu-id="39aed-128">En gateway för virtuell nätverksanslutning</span><span class="sxs-lookup"><span data-stu-id="39aed-128">A virtual network gateway connection</span></span>

<span data-ttu-id="39aed-129">hello följande diagram visar hur hello nätverk kommunicera mellan datacenter.</span><span class="sxs-lookup"><span data-stu-id="39aed-129">hello following diagram shows how hello networks communicate between data centers.</span></span>

   ![Tillgänglighetsgruppen](./media/virtual-machines-windows-portal-sql-availability-group-dr/01-vpngateway-example.png)

>[!IMPORTANT]
><span data-ttu-id="39aed-131">Den här arkitekturen debiteras utgående data för data som replikeras mellan Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="39aed-131">This architecture incurs outbound data charges for data replicated between Azure regions.</span></span> <span data-ttu-id="39aed-132">Se [bandbredd priser](http://azure.microsoft.com/pricing/details/bandwidth/).</span><span class="sxs-lookup"><span data-stu-id="39aed-132">See [Bandwidth Pricing](http://azure.microsoft.com/pricing/details/bandwidth/).</span></span>  

## <a name="create-remote-replica"></a><span data-ttu-id="39aed-133">Skapa Fjärreplik</span><span class="sxs-lookup"><span data-stu-id="39aed-133">Create remote replica</span></span>

<span data-ttu-id="39aed-134">toocreate en replik i en fjärransluten Datacenter hello gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="39aed-134">toocreate a replica in a remote data center, do hello following steps:</span></span>

1. <span data-ttu-id="39aed-135">[Skapa ett virtuellt nätverk i hello nya region](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="39aed-135">[Create a virtual network in hello new region](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span></span>

1. <span data-ttu-id="39aed-136">[Konfigurera en VNet-till-VNet-anslutning med hello Azure-portalen](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="39aed-136">[Configure a VNet-to-VNet connection using hello Azure portal](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).</span></span>

   >[!NOTE]
   ><span data-ttu-id="39aed-137">I vissa fall kanske toouse PowerShell toocreate hello VNet-till-VNet-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="39aed-137">In some cases, you may have toouse PowerShell toocreate hello VNet-to-VNet connection.</span></span> <span data-ttu-id="39aed-138">Till exempel om du använder olika Azure-konton konfigurera du inte hello anslutning i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="39aed-138">For example, if you use different Azure accounts you cannot configure hello connection in hello portal.</span></span> <span data-ttu-id="39aed-139">I det här fallet finns [konfigurera VNet-till-VNet-anslutningen med hello Azure-portalen](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="39aed-139">In this case see, [Configure a VNet-to-VNet connection using hello Azure portal](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span></span>

1. <span data-ttu-id="39aed-140">[Skapa en domänkontrollant i hello nya region](../../../active-directory/active-directory-new-forest-virtual-machine.md).</span><span class="sxs-lookup"><span data-stu-id="39aed-140">[Create a domain controller in hello new region](../../../active-directory/active-directory-new-forest-virtual-machine.md).</span></span>

   <span data-ttu-id="39aed-141">Den här domänkontrollanten ger autentisering om hello domänkontrollant i hello primära platsen inte är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="39aed-141">This domain controller provides authentication if hello domain controller in hello primary site is not available.</span></span>

1. <span data-ttu-id="39aed-142">[Skapa en virtuell dator med SQL Server i hello nya region](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="39aed-142">[Create a SQL Server virtual machine in hello new region](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

1. <span data-ttu-id="39aed-143">[Skapa en Azure belastningsutjämnare i hello nätverket på hello ny region](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="39aed-143">[Create an Azure load balancer in hello network on hello new region](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span></span>

   <span data-ttu-id="39aed-144">Den här belastningsutjämnaren måste:</span><span class="sxs-lookup"><span data-stu-id="39aed-144">This load balancer must:</span></span>

   - <span data-ttu-id="39aed-145">I hello samma nätverk och undernät som hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="39aed-145">Be in hello same network and subnet as hello new virtual machine.</span></span>
   - <span data-ttu-id="39aed-146">Har en statisk IP-adress för hello tillgänglighetsgruppens lyssnare.</span><span class="sxs-lookup"><span data-stu-id="39aed-146">Have a static IP address for hello availability group listener.</span></span>
   - <span data-ttu-id="39aed-147">Inkludera en serverdelspool som består av endast hello virtuella datorer i hello samma region som hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="39aed-147">Include a backend pool consisting of only hello virtual machines in hello same region as hello load balancer.</span></span>
   - <span data-ttu-id="39aed-148">Använda en TCP-port avsökningen specifika toohello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="39aed-148">Use a TCP port probe specific toohello IP address.</span></span>
   - <span data-ttu-id="39aed-149">Har en belastningsutjämning regeln för specifika toohello SQL Server i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="39aed-149">Have a load balancing rule specific toohello SQL Server in hello same region.</span></span>  

1. <span data-ttu-id="39aed-150">[Lägg till redundanskluster funktionen toohello nya SQL-servern](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span><span class="sxs-lookup"><span data-stu-id="39aed-150">[Add Failover Clustering feature toohello new SQL Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span></span>

1. <span data-ttu-id="39aed-151">[Ansluta till hello nya SQL Server toohello domänen](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span><span class="sxs-lookup"><span data-stu-id="39aed-151">[Join hello new SQL Server toohello domain](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span></span>

1. <span data-ttu-id="39aed-152">[Ange ett domänkonto för hello nya SQL Server service-kontot toouse](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount).</span><span class="sxs-lookup"><span data-stu-id="39aed-152">[Set hello new SQL Server service account toouse a domain account](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount).</span></span>

1. <span data-ttu-id="39aed-153">[Lägg till hello nya SQL Server toohello Windows Server Failover Cluster](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode).</span><span class="sxs-lookup"><span data-stu-id="39aed-153">[Add hello new SQL Server toohello Windows Server Failover Cluster](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode).</span></span>

1. <span data-ttu-id="39aed-154">Skapa en IP-adressresurs på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="39aed-154">Create an IP address resource on hello cluster.</span></span>

   <span data-ttu-id="39aed-155">Du kan skapa hello IP-adressresurs i hanteraren för redundanskluster.</span><span class="sxs-lookup"><span data-stu-id="39aed-155">You can create hello IP address resource in Failover Cluster Manager.</span></span> <span data-ttu-id="39aed-156">Högerklicka på hello tillgänglighet grupp rollen klickar du på **Lägg till resurs**, **mer resurser**, och klicka på **IP-adress**.</span><span class="sxs-lookup"><span data-stu-id="39aed-156">Right-click hello availability group role, click **Add Resource**, **More Resources**, and click **IP Address**.</span></span>

   ![Skapa IP-adress](./media/virtual-machines-windows-portal-sql-availability-group-dr/20-add-ip-resource.png)

   <span data-ttu-id="39aed-158">Konfigurera IP-adressen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="39aed-158">Configure this IP address as follows:</span></span>

   - <span data-ttu-id="39aed-159">Använda hello nätverk från hello fjärrdata center.</span><span class="sxs-lookup"><span data-stu-id="39aed-159">Use hello network from hello remote data center.</span></span>
   - <span data-ttu-id="39aed-160">Tilldela hello IP-adress från hello nya Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="39aed-160">Assign hello IP address from hello new Azure load balancer.</span></span> 

1. <span data-ttu-id="39aed-161">Hej nya SQL-servern i SQL Server Configuration Manager på [aktivera Always On-Tillgänglighetsgrupper](http://msdn.microsoft.com/library/ff878259.aspx).</span><span class="sxs-lookup"><span data-stu-id="39aed-161">On hello new SQL Server in SQL Server Configuration Manager, [enable Always On Availability Groups](http://msdn.microsoft.com/library/ff878259.aspx).</span></span>

1. <span data-ttu-id="39aed-162">[Öppna brandväggsportar på hello nya SQL-servern](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span><span class="sxs-lookup"><span data-stu-id="39aed-162">[Open firewall ports on hello new SQL Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span></span>

   <span data-ttu-id="39aed-163">hello-portnummer som du måste tooopen beror på din miljö.</span><span class="sxs-lookup"><span data-stu-id="39aed-163">hello port numbers you need tooopen depend on your environment.</span></span> <span data-ttu-id="39aed-164">Öppna portar för hello spegling slutpunkt och Azure ladda belastningsutjämnaren, hälsoavsökningen.</span><span class="sxs-lookup"><span data-stu-id="39aed-164">Open ports for hello mirroring endpoint and Azure load balancer health probe.</span></span>

1. <span data-ttu-id="39aed-165">[Lägga till en replik toohello availability group på hello nya SQL-servern](http://msdn.microsoft.com/library/hh213239.aspx).</span><span class="sxs-lookup"><span data-stu-id="39aed-165">[Add a replica toohello availability group on hello new SQL Server](http://msdn.microsoft.com/library/hh213239.aspx).</span></span>

   <span data-ttu-id="39aed-166">För en replik i en fjärransluten Azure-region kan ställas in för asynkron replikering med manuell växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="39aed-166">For a replica in a remote Azure region, set it for asynchronous replication with manual failover.</span></span>  

1. <span data-ttu-id="39aed-167">Lägg till hello IP-adressresurs som ett beroende för hello lyssnare klienten åtkomst punkt (nätverksnamn) klustret.</span><span class="sxs-lookup"><span data-stu-id="39aed-167">Add hello IP address resource as a dependency for hello listener client access point (network name) cluster.</span></span>

   <span data-ttu-id="39aed-168">hello visar följande skärmbild en korrekt konfigurerad klusterresurs för IP-adress:</span><span class="sxs-lookup"><span data-stu-id="39aed-168">hello following screenshot shows a properly configured IP address cluster resource:</span></span>

   ![Tillgänglighetsgruppen](./media/virtual-machines-windows-portal-sql-availability-group-dr/50-configure-dependency-multiple-ip.png)

   >[!IMPORTANT]
   ><span data-ttu-id="39aed-170">hello klusterresursgrupp innehåller både IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="39aed-170">hello cluster resource group includes both IP addresses.</span></span> <span data-ttu-id="39aed-171">IP-adresserna är beroenden för hello lyssnare klientåtkomstpunkt.</span><span class="sxs-lookup"><span data-stu-id="39aed-171">Both IP addresses are dependencies for hello listener client access point.</span></span> <span data-ttu-id="39aed-172">Använd hello **eller** operator i hello beroende klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="39aed-172">Use hello **OR** operator in hello cluster dependency configuration.</span></span>

1. <span data-ttu-id="39aed-173">[Ange hello Klusterparametrar i PowerShell](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam).</span><span class="sxs-lookup"><span data-stu-id="39aed-173">[Set hello cluster parameters in PowerShell](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam).</span></span>

<span data-ttu-id="39aed-174">Kör hello PowerShell-skript med hello klustrets nätverksnamn och IP-adress avsökningsport som du har konfigurerat på hello belastningsutjämnare i hello ny region.</span><span class="sxs-lookup"><span data-stu-id="39aed-174">Run hello PowerShell script with hello cluster network name, IP address, and probe port that you configured on hello load balancer in hello new region.</span></span>

   ```PowerShell
   $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster name for hello network in hello new region (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name).
   $IPResourceName = "<IPResourceName>" # hello cluster name for hello new IP Address resource.
   $ILBIP = “<n.n.n.n>” # hello IP Address of hello Internal Load Balancer (ILB) in hello new region. This is hello static IP address for hello load balancer you configured in hello Azure portal.
   [int]$ProbePort = <nnnnn> # hello probe port you set on hello ILB.

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

## <a name="set-connection-for-multiple-subnets"></a><span data-ttu-id="39aed-175">Set-anslutning för flera undernät</span><span class="sxs-lookup"><span data-stu-id="39aed-175">Set connection for multiple subnets</span></span>

<span data-ttu-id="39aed-176">hello replik i hello fjärrdata center är en del av hello tillgänglighetsgruppen men det är i ett annat undernät.</span><span class="sxs-lookup"><span data-stu-id="39aed-176">hello replica in hello remote data center is part of hello availability group but it is in a different subnet.</span></span> <span data-ttu-id="39aed-177">Om den här repliken blir hello primära repliken, kan det uppstå programmet anslutnings-timeout.</span><span class="sxs-lookup"><span data-stu-id="39aed-177">If this replica becomes hello primary replica, application connection time-outs may occur.</span></span> <span data-ttu-id="39aed-178">Det här beteendet är hello samma som en lokal tillgänglighetsgrupp i en distribution med flera undernät.</span><span class="sxs-lookup"><span data-stu-id="39aed-178">This behavior is hello same as an on-premises availability group in a multi-subnet deployment.</span></span> <span data-ttu-id="39aed-179">tooallow anslutningar från klientprogram, uppdatera hello klientanslutning eller konfigurera namnmatchning cachelagring på hello nätverksnamnresursen för klustret.</span><span class="sxs-lookup"><span data-stu-id="39aed-179">tooallow connections from client applications, either update hello client connection or configure name resolution caching on hello cluster network name resource.</span></span>

<span data-ttu-id="39aed-180">Helst bör uppdatera hello klienten anslutning strängar tooset `MultiSubnetFailover=Yes`.</span><span class="sxs-lookup"><span data-stu-id="39aed-180">Preferably, update hello client connection strings tooset `MultiSubnetFailover=Yes`.</span></span> <span data-ttu-id="39aed-181">Se [ansluta med MultiSubnetFailover](http://msdn.microsoft.com/library/gg471494#Anchor_0).</span><span class="sxs-lookup"><span data-stu-id="39aed-181">See [Connecting With MultiSubnetFailover](http://msdn.microsoft.com/library/gg471494#Anchor_0).</span></span>

<span data-ttu-id="39aed-182">Om du inte kan ändra hello anslutningssträngar, kan du konfigurera name resolution cachelagring.</span><span class="sxs-lookup"><span data-stu-id="39aed-182">If you cannot modify hello connection strings, you can configure name resolution caching.</span></span> <span data-ttu-id="39aed-183">Se [timeout i flera undernät tillgänglighetsgrupp](http://blogs.msdn.microsoft.com/alwaysonpro/2014/06/03/connection-timeouts-in-multi-subnet-availability-group/).</span><span class="sxs-lookup"><span data-stu-id="39aed-183">See [Connection Timeouts in Multi-subnet Availability Group](http://blogs.msdn.microsoft.com/alwaysonpro/2014/06/03/connection-timeouts-in-multi-subnet-availability-group/).</span></span>

## <a name="fail-over-tooremote-region"></a><span data-ttu-id="39aed-184">Växla över tooremote region</span><span class="sxs-lookup"><span data-stu-id="39aed-184">Fail over tooremote region</span></span>

<span data-ttu-id="39aed-185">tootest lyssnare anslutningen toohello remote region, kan du växla över hello replik toohello remote region.</span><span class="sxs-lookup"><span data-stu-id="39aed-185">tootest listener connectivity toohello remote region, you can fail over hello replica toohello remote region.</span></span> <span data-ttu-id="39aed-186">Hello replik är asynkron, är redundans sårbar toopotential data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="39aed-186">While hello replica is asynchronous, failover is vulnerable toopotential data loss.</span></span> <span data-ttu-id="39aed-187">toofail över utan dataförlust, ändra hello tillgänglighet läge toosynchronous och ange hello redundans läge tooautomatic.</span><span class="sxs-lookup"><span data-stu-id="39aed-187">toofail over without data loss, change hello availability mode toosynchronous and set hello failover mode tooautomatic.</span></span> <span data-ttu-id="39aed-188">Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="39aed-188">Use hello following steps:</span></span>

1. <span data-ttu-id="39aed-189">I **Object Explorer**, Anslut toohello instansen av SQL Server som är värd för hello primära repliken.</span><span class="sxs-lookup"><span data-stu-id="39aed-189">In **Object Explorer**, connect toohello instance of SQL Server that hosts hello primary replica.</span></span>
1. <span data-ttu-id="39aed-190">Under **AlwaysOn Availability Groups**, **Tillgänglighetsgrupper**, högerklicka på tillgänglighetsgruppen och klickar på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="39aed-190">Under **AlwaysOn Availability Groups**, **Availability Groups**, right-click your availability group and click **Properties**.</span></span>
1. <span data-ttu-id="39aed-191">På hello **allmänna** sidan under **Tillgänglighetsrepliker**, ange hello sekundär replik i hello DR plats toouse **synkron genomför** tillgänglighetsläget och **Automatisk** redundansläge.</span><span class="sxs-lookup"><span data-stu-id="39aed-191">On hello **General** page, under **Availability Replicas**, set hello secondary replica in hello DR site toouse **Synchronous Commit** availability mode and **Automatic** failover mode.</span></span>
1. <span data-ttu-id="39aed-192">Om du har en sekundär replik på samma plats som den primära repliken för hög tillgänglighet kan du ställa in den här repliken också**asynkron genomför** och **manuell**.</span><span class="sxs-lookup"><span data-stu-id="39aed-192">If you have a secondary replica in same site as your primary replica for high availability, set this replica too**Asynchronous Commit** and **Manual**.</span></span>
1. <span data-ttu-id="39aed-193">Klicka på OK.</span><span class="sxs-lookup"><span data-stu-id="39aed-193">Click OK.</span></span>
1. <span data-ttu-id="39aed-194">I **Object Explorer**, högerklicka på hello tillgänglighetsgruppen och på **visa instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="39aed-194">In **Object Explorer**, right-click hello availability group, and click **Show Dashboard**.</span></span>
1. <span data-ttu-id="39aed-195">Kontrollera att hello på hello instrumentpanelen repliken på hello DR-plats har synkroniserats.</span><span class="sxs-lookup"><span data-stu-id="39aed-195">On hello dashboard, verify that hello replica on hello DR site is synchronized.</span></span>
1. <span data-ttu-id="39aed-196">I **Object Explorer**, högerklicka på hello tillgänglighetsgruppen och på **växling vid fel...** . SQL Server Management Studios öppnar en guide toofail över SQL Server.</span><span class="sxs-lookup"><span data-stu-id="39aed-196">In **Object Explorer**, right-click hello availability group, and click **Failover...**. SQL Server Management Studios opens a wizard toofail over SQL Server.</span></span>  
1. <span data-ttu-id="39aed-197">Klicka på **nästa**, och välj hello SQL Server-instansen i hello DR-plats.</span><span class="sxs-lookup"><span data-stu-id="39aed-197">Click **Next**, and select hello SQL Server instance in hello DR site.</span></span> <span data-ttu-id="39aed-198">Klicka på **nästa** igen.</span><span class="sxs-lookup"><span data-stu-id="39aed-198">Click **Next** again.</span></span>
1. <span data-ttu-id="39aed-199">Ansluta toohello SQL Server-instansen i hello DR-plats och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="39aed-199">Connect toohello SQL Server instance in hello DR site and click **Next**.</span></span>
1. <span data-ttu-id="39aed-200">På hello **sammanfattning** kontrollerar hello inställningar och klickar på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="39aed-200">On hello **Summary** page, verify hello settings and click **Finish**.</span></span>

<span data-ttu-id="39aed-201">Flytta hello primära repliken tillbaka tooyour primära data center och ange hello tillgänglighet läge tillbaka tootheir normal drift inställningar när du testar anslutningen.</span><span class="sxs-lookup"><span data-stu-id="39aed-201">After testing connectivity, move hello primary replica back tooyour primary data center and set hello availability mode back tootheir normal operating settings.</span></span> <span data-ttu-id="39aed-202">hello visar följande tabell hello normala driftinställningar för hello-arkitekturen som beskrivs i det här dokumentet:</span><span class="sxs-lookup"><span data-stu-id="39aed-202">hello following table shows hello normal operational settings for hello architecture described in this document:</span></span>

| <span data-ttu-id="39aed-203">Plats</span><span class="sxs-lookup"><span data-stu-id="39aed-203">Location</span></span> | <span data-ttu-id="39aed-204">Server-instans</span><span class="sxs-lookup"><span data-stu-id="39aed-204">Server Instance</span></span> | <span data-ttu-id="39aed-205">Roll</span><span class="sxs-lookup"><span data-stu-id="39aed-205">Role</span></span> | <span data-ttu-id="39aed-206">Tillgänglighetsläget</span><span class="sxs-lookup"><span data-stu-id="39aed-206">Availability Mode</span></span> | <span data-ttu-id="39aed-207">Redundansläge</span><span class="sxs-lookup"><span data-stu-id="39aed-207">Failover Mode</span></span>
| ----- | ----- | ----- | ----- | -----
| <span data-ttu-id="39aed-208">Primära Datacenter</span><span class="sxs-lookup"><span data-stu-id="39aed-208">Primary data center</span></span> | <span data-ttu-id="39aed-209">SQL-1</span><span class="sxs-lookup"><span data-stu-id="39aed-209">SQL-1</span></span> | <span data-ttu-id="39aed-210">Primär</span><span class="sxs-lookup"><span data-stu-id="39aed-210">Primary</span></span> | <span data-ttu-id="39aed-211">Synkron</span><span class="sxs-lookup"><span data-stu-id="39aed-211">Synchronous</span></span> | <span data-ttu-id="39aed-212">Automatisk</span><span class="sxs-lookup"><span data-stu-id="39aed-212">Automatic</span></span>
| <span data-ttu-id="39aed-213">Primära Datacenter</span><span class="sxs-lookup"><span data-stu-id="39aed-213">Primary data center</span></span> | <span data-ttu-id="39aed-214">SQL-2</span><span class="sxs-lookup"><span data-stu-id="39aed-214">SQL-2</span></span> | <span data-ttu-id="39aed-215">Sekundär</span><span class="sxs-lookup"><span data-stu-id="39aed-215">Secondary</span></span> | <span data-ttu-id="39aed-216">Synkron</span><span class="sxs-lookup"><span data-stu-id="39aed-216">Synchronous</span></span> | <span data-ttu-id="39aed-217">Automatisk</span><span class="sxs-lookup"><span data-stu-id="39aed-217">Automatic</span></span>
| <span data-ttu-id="39aed-218">Sekundär eller en fjärrdator Datacenter</span><span class="sxs-lookup"><span data-stu-id="39aed-218">Secondary or remote data center</span></span> | <span data-ttu-id="39aed-219">SQL-3</span><span class="sxs-lookup"><span data-stu-id="39aed-219">SQL-3</span></span> | <span data-ttu-id="39aed-220">Sekundär</span><span class="sxs-lookup"><span data-stu-id="39aed-220">Secondary</span></span> | <span data-ttu-id="39aed-221">Asynkron</span><span class="sxs-lookup"><span data-stu-id="39aed-221">Asynchronous</span></span> | <span data-ttu-id="39aed-222">Manuell</span><span class="sxs-lookup"><span data-stu-id="39aed-222">Manual</span></span>


### <a name="more-information-about-planned-and-forced-manual-failover"></a><span data-ttu-id="39aed-223">Mer information om planerade och påtvingad manuell redundansväxling</span><span class="sxs-lookup"><span data-stu-id="39aed-223">More information about planned and forced manual failover</span></span>

<span data-ttu-id="39aed-224">Mer information finns i följande avsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="39aed-224">For more information, see hello following topics:</span></span>

- [<span data-ttu-id="39aed-225">Utför en planerad manuell Redundansväxling av en tillgänglighetsgrupp (SQLServer)</span><span class="sxs-lookup"><span data-stu-id="39aed-225">Perform a Planned Manual Failover of an Availability Group (SQL Server)</span></span>](http://msdn.microsoft.com/library/hh231018.aspx)
- [<span data-ttu-id="39aed-226">Utför en påtvingad manuell Redundansväxling av en tillgänglighetsgrupp (SQLServer)</span><span class="sxs-lookup"><span data-stu-id="39aed-226">Perform a Forced Manual Failover of an Availability Group (SQL Server)</span></span>](http://msdn.microsoft.com/library/ff877957.aspx)

## <a name="additional-links"></a><span data-ttu-id="39aed-227">Ytterligare länkar</span><span class="sxs-lookup"><span data-stu-id="39aed-227">Additional Links</span></span>

* [<span data-ttu-id="39aed-228">Always On-Tillgänglighetsgrupper</span><span class="sxs-lookup"><span data-stu-id="39aed-228">Always On Availability Groups</span></span>](http://msdn.microsoft.com/library/hh510230.aspx)
* [<span data-ttu-id="39aed-229">Virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="39aed-229">Azure Virtual Machines</span></span>](http://docs.microsoft.com/azure/virtual-machines/windows/)
* [<span data-ttu-id="39aed-230">Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="39aed-230">Azure Load Balancers</span></span>](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer)
* [<span data-ttu-id="39aed-231">Azure Tillgänglighetsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="39aed-231">Azure Availability Sets</span></span>](../manage-availability.md)
