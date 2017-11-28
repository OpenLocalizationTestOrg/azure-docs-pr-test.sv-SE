---
title: "aaaCreate en virtuell dator (klassisk) med flera nätverkskort med hjälp av PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate och konfigurera virtuella datorer med flera nätverkskort med hjälp av PowerShell."
services: virtual-network, virtual-machines
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a1a3952c-2dcc-4977-bd7a-52d623c1fb07
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 8ef35bd4cfd7e6a527080f1cfc541275ca86f5e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics"></a><span data-ttu-id="9f5a3-103">Skapa en virtuell dator (klassisk) med flera nätverkskort</span><span class="sxs-lookup"><span data-stu-id="9f5a3-103">Create a VM (Classic) with multiple NICs</span></span>
<span data-ttu-id="9f5a3-104">Du kan skapa virtuella datorer (VM) i Azure och bifoga flera nätverk gränssnitt (NIC) tooeach för dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) tooeach of your VMs.</span></span> <span data-ttu-id="9f5a3-105">Flera nätverkskort är ett krav för många virtuella nätverksenheter, till exempel leverans av program och optimering av WAN-lösningar.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-105">Multiple NICs are a requirement for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span> <span data-ttu-id="9f5a3-106">Flera nätverkskort kan också tillhandahålla isolering för trafik mellan nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-106">Multiple NICs also provide isolation of traffic between NICs.</span></span>

![Flera nätverkskort för den virtuella datorn](./media/virtual-networks-multiple-nics/IC757773.png)

<span data-ttu-id="9f5a3-108">hello bild visas en virtuell dator med tre nätverkskort, var och en ansluten tooa olika undernät.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-108">hello figure shows a VM with three NICs, each connected tooa different subnet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9f5a3-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9f5a3-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="9f5a3-110">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-110">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="9f5a3-111">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-111">Microsoft recommends that most new deployments use Resource Manager.</span></span>

* <span data-ttu-id="9f5a3-112">Internet-riktade VIP (klassiska distributioner) stöds bara på hello ”default” NIC.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-112">Internet-facing VIP (classic deployments) is only supported on hello "default" NIC.</span></span> <span data-ttu-id="9f5a3-113">Det finns endast en VIP toohello IP hello standard NIC.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-113">There is only one VIP toohello IP of hello default NIC.</span></span>
* <span data-ttu-id="9f5a3-114">Instans nivå offentlig IP (LPIP)-adresser (klassiska distributioner) stöds inte för flera virtuella datorer NIC för tillfället.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-114">At this time, Instance Level Public IP (LPIP) addresses (classic deployments) are not supported for multi NIC VMs.</span></span>
* <span data-ttu-id="9f5a3-115">Hej ordning hello nätverkskort från inuti hello VM slumpmässigt och kan också ändra över Azure-infrastrukturuppdateringar.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-115">hello order of hello NICs from inside hello VM will be random, and could also change across Azure infrastructure updates.</span></span> <span data-ttu-id="9f5a3-116">Hej IP-adresser och hello motsvarande ethernet MAC adresser förblir hello samma.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-116">However, hello IP addresses, and hello corresponding ethernet MAC addresses will remain hello same.</span></span> <span data-ttu-id="9f5a3-117">Anta till exempel **Eth1** har 10.1.0.100 och MAC-adress 00-0D-3A-B0-39-0D; när en uppdatering för Azure-infrastrukturen och omstart, den kan ändras för**Eth2**, men hello IP- och MAC länkning kommer förblir hello samma.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-117">For example, assume **Eth1** has IP address 10.1.0.100 and MAC address 00-0D-3A-B0-39-0D; after an Azure infrastructure update and reboot, it could be changed too**Eth2**, but hello IP and MAC pairing will remain hello same.</span></span> <span data-ttu-id="9f5a3-118">Om en omstart kund-initierad hello NIC ordning förblir hello samma.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-118">When a restart is customer-initiated, hello NIC order will remain hello same.</span></span>
* <span data-ttu-id="9f5a3-119">hello-adress för varje nätverkskort på varje virtuell dator måste finnas i ett undernät, flera nätverkskort på en enda VM kan varje tilldelas adresser i hello samma undernät.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-119">hello address for each NIC on each VM must be located in a subnet, multiple NICs on a single VM can each be assigned addresses that are in hello same subnet.</span></span>
* <span data-ttu-id="9f5a3-120">hello VM-storlek anger hello antal nätverkskort som du kan skapa för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-120">hello VM size determines hello number of NICS that you can create for a VM.</span></span> <span data-ttu-id="9f5a3-121">Referens hello [Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) och [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) VM-storlekar artiklar toodetermine hur många nätverkskort som har stöd för varje VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-121">Reference hello [Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) VM sizes articles toodetermine how many NICS each VM size supports.</span></span> 

## <a name="network-security-groups-nsgs"></a><span data-ttu-id="9f5a3-122">Nätverkssäkerhetsgrupper (NSG: er)</span><span class="sxs-lookup"><span data-stu-id="9f5a3-122">Network Security Groups (NSGs)</span></span>
<span data-ttu-id="9f5a3-123">I en Resource Manager-distribution, kan ett nätverkskort på en virtuell dator associeras med en Nätverkssäkerhetsgrupp (NSG), inklusive alla nätverkskort på en virtuell dator som har flera nätverkskort som är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-123">In a Resource Manager deployment, any NIC on a VM may be associated with a Network Security Group (NSG), including any NICs on a VM that has multiple NICs enabled.</span></span> <span data-ttu-id="9f5a3-124">Om ett nätverkskort tilldelas en adress i ett undernät där hello undernätet är kopplat till en NSG, sedan gäller hello regler i hello undernät NSG även toothat NIC.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-124">If a NIC is assigned an address within a subnet where hello subnet is associated with an NSG, then hello rules in hello subnet’s NSG also apply toothat NIC.</span></span> <span data-ttu-id="9f5a3-125">Du kan även associera ett nätverkskort med en NSG i tillägg tooassociating undernät med NSG: er.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-125">In addition tooassociating subnets with NSGs, you can also associate a NIC with an NSG.</span></span>

<span data-ttu-id="9f5a3-126">Om ett undernät som är associerad med en NSG och ett nätverkskort i undernätet är individuellt associerad med en NSG, hello associerade NSG-regler tillämpas i **flöda ordning** enligt toohello trafikriktning hello som skickas till eller från hello NIC:</span><span class="sxs-lookup"><span data-stu-id="9f5a3-126">If a subnet is associated with an NSG, and a NIC within that subnet is individually associated with an NSG, hello associated NSG rules are applied in **flow order** according toohello direction of hello traffic being passed into or out of hello NIC:</span></span>

* <span data-ttu-id="9f5a3-127">**Inkommande trafik** vars mål är hello NIC i fråga först flödar genom hello undernät, utlöser hello undernät NSG-regler innan du skickar till hello NIC och sedan utlösa hello NIC NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-127">**Incoming traffic** whose destination is hello NIC in question flows first through hello subnet, triggering hello subnet’s NSG rules, before passing into hello NIC, then triggering hello NIC’s NSG rules.</span></span>
* <span data-ttu-id="9f5a3-128">**Utgående trafik** vars källan är hello NIC i fråga flödar först ut från hello NIC, utlöser hello NIC NSG-regler innan passera hello undernät och utlösa hello undernät NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-128">**Outgoing traffic** whose source is hello NIC in question flows first out from hello NIC, triggering hello NIC’s NSG rules, before passing through hello subnet, then triggering hello subnet’s NSG rules.</span></span>

<span data-ttu-id="9f5a3-129">Lär dig mer om [Nätverkssäkerhetsgrupper](virtual-networks-nsg.md) och hur de används baserat på kopplingarna toosubnets, virtuella datorer och nätverkskort...</span><span class="sxs-lookup"><span data-stu-id="9f5a3-129">Learn more about [Network Security Groups](virtual-networks-nsg.md) and how they are applied based on associations toosubnets, VMs, and NICs..</span></span>

## <a name="how-tooconfigure-a-multi-nic-vm-in-a-classic-deployment"></a><span data-ttu-id="9f5a3-130">Hur tooConfigure multi NIC VM i en klassisk distribution</span><span class="sxs-lookup"><span data-stu-id="9f5a3-130">How tooConfigure a multi NIC VM in a classic deployment</span></span>
<span data-ttu-id="9f5a3-131">hello anvisningarna nedan hjälper dig att skapa flera NIC VM som innehåller 3 nätverkskort: standard NIC och två ytterligare nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-131">hello instructions below will help you create a multi NIC VM containing 3 NICs: a default NIC and two additional NICs.</span></span> <span data-ttu-id="9f5a3-132">hello konfigurationssteg skapar en virtuell dator som kommer att konfigureras enligt toohello service configuration file fragment nedan:</span><span class="sxs-lookup"><span data-stu-id="9f5a3-132">hello configuration steps will create a VM that will be configured according toohello service configuration file fragment below:</span></span>

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over hello remainder section …
    </VirtualNetworkSite>


<span data-ttu-id="9f5a3-133">Du måste följande krav innan du försöker toorun hello PowerShell-kommandon i hello exempel hello.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-133">You need hello following prerequisites before trying toorun hello PowerShell commands in hello example.</span></span>

* <span data-ttu-id="9f5a3-134">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-134">An Azure subscription.</span></span>
* <span data-ttu-id="9f5a3-135">Konfigurerade virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-135">A configured virtual network.</span></span> <span data-ttu-id="9f5a3-136">Se [översikt över virtuella nätverk](virtual-networks-overview.md) mer information om Vnet.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-136">See [Virtual Network Overview](virtual-networks-overview.md) for more information about VNets.</span></span>
* <span data-ttu-id="9f5a3-137">hello senaste versionen av Azure PowerShell hämtas och installeras.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-137">hello latest version of Azure PowerShell downloaded and installed.</span></span> <span data-ttu-id="9f5a3-138">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9f5a3-138">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="9f5a3-139">toocreate en virtuell dator med flera nätverkskort, fullständig hello följande genom att ange varje kommando i en enda PowerShell-session:</span><span class="sxs-lookup"><span data-stu-id="9f5a3-139">toocreate a VM with multiple NICs, complete hello following steps by entering each command within a single PowerShell session:</span></span>

1. <span data-ttu-id="9f5a3-140">Välj en VM-avbildning från galleriet för Virtuella Azure-avbildning.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-140">Select a VM image from Azure VM image gallery.</span></span> <span data-ttu-id="9f5a3-141">Observera att bilder ändras ofta och är tillgängliga efter region.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-141">Note that images change frequently and are available by region.</span></span> <span data-ttu-id="9f5a3-142">hello avbildning som anges i hello exemplet nedan får ändra eller kanske inte i din region, så Tänk toospecify hello bilden som du behöver.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-142">hello image specified in hello example below may change or might not be in your region, so be sure toospecify hello image you need.</span></span>

    ```powershell
    $image = Get-AzureVMImage `
    -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"
    ```

2. <span data-ttu-id="9f5a3-143">Skapa en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-143">Create a VM configuration.</span></span>

    ```powershell
    $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
    -Image $image.ImageName –AvailabilitySetName "MyAVSet"
    ```

3. <span data-ttu-id="9f5a3-144">Skapa hello standard administratörsinloggning.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-144">Create hello default administrator login.</span></span>

    ```powershell
    Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
    -Password "<YourAdminPassword>"
    ```

4. <span data-ttu-id="9f5a3-145">Lägg till ytterligare nätverkskort toohello VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-145">Add additional NICs toohello VM configuration.</span></span>

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
    -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
    Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
    -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm
    ```

5. <span data-ttu-id="9f5a3-146">Ange hello undernät och IP-adress för hello standard nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-146">Specify hello subnet and IP address for hello default NIC.</span></span>

    ```powershell
    Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
    Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm
    ```

6. <span data-ttu-id="9f5a3-147">Skapa hello VM i ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-147">Create hello VM in your virtual network.</span></span>

    ```powershell
    New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm
    ```

    > [!NOTE]
    > <span data-ttu-id="9f5a3-148">Hej VNet som du anger här måste redan finnas (som anges i hello krav).</span><span class="sxs-lookup"><span data-stu-id="9f5a3-148">hello VNet that you specify here must already exist (as mentioned in hello prerequisites).</span></span> <span data-ttu-id="9f5a3-149">hello exemplet nedan anger ett virtuellt nätverk med namnet **MultiNIC VNet**.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-149">hello example below specifies a virtual network named **MultiNIC-VNet**.</span></span>
    >

## <a name="limitations"></a><span data-ttu-id="9f5a3-150">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="9f5a3-150">Limitations</span></span>
<span data-ttu-id="9f5a3-151">hello följande begränsningar gäller när du använder flera nätverkskort:</span><span class="sxs-lookup"><span data-stu-id="9f5a3-151">hello following limitations are applicable when using multiple NICs:</span></span>

* <span data-ttu-id="9f5a3-152">Virtuella datorer med flera nätverkskort måste skapas i virtuella Azure-nätverk (Vnet).</span><span class="sxs-lookup"><span data-stu-id="9f5a3-152">VMs with multiple NICs must be created in Azure virtual networks (VNets).</span></span> <span data-ttu-id="9f5a3-153">Icke-VNet virtuella datorer kan inte konfigureras med flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-153">Non-VNet VMs cannot be configured with multiple NICs.</span></span>
* <span data-ttu-id="9f5a3-154">Alla virtuella datorer i en tillgänglighetsgrupp ange måste toouse flera nätverkskort eller ett enda nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-154">All VMs in an availability set need toouse either multiple NICs or a single NIC.</span></span> <span data-ttu-id="9f5a3-155">Det får inte vara en blandning av flera NIC virtuella datorer och en enda NIC virtuella datorer i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-155">There cannot be a mixture of multiple NIC VMs and single NIC VMs within an availability set.</span></span> <span data-ttu-id="9f5a3-156">Samma regler gäller för virtuella datorer i en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-156">Same rules apply for VMs in a cloud service.</span></span> <span data-ttu-id="9f5a3-157">För flera nätverkskort virtuella datorer kan inte de nödvändiga toohave hello samma antal nätverkskort, så länge som de har minst två.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-157">For multiple NIC VMs, they aren't required toohave hello same number of NICs, as long as they each have at least two.</span></span>
* <span data-ttu-id="9f5a3-158">En virtuell dator med en enda nätverkskort kan inte konfigureras med flera nätverkskort (och vice versa) när den har distribuerats, utan att ta bort och återskapa den.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-158">A VM with a single NIC cannot be configured with multi NICs (and vice-versa) once it is deployed, without deleting and re-creating it.</span></span>

## <a name="secondary-nics-access-tooother-subnets"></a><span data-ttu-id="9f5a3-159">Sekundär nätverkskort åtkomst tooother undernät</span><span class="sxs-lookup"><span data-stu-id="9f5a3-159">Secondary NICs access tooother subnets</span></span>
<span data-ttu-id="9f5a3-160">Blir som standard sekundära nätverkskort inte konfigureras med en standard-gateway på grund av toowhich hello trafikflödet på hello sekundära nätverkskort begränsad toobe inom hello, samma undernät.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-160">By default secondary NICs will not be configured with a default gateway, due toowhich hello traffic flow on hello secondary NICs will be limited toobe within hello same subnet.</span></span> <span data-ttu-id="9f5a3-161">Om hello användare vill tooenable sekundära nätverkskort tootalk utanför sin egen undernät, måste de tooadd en post i hello routning tabell tooconfigure hello-gateway som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-161">If hello users want tooenable secondary NICs tootalk outside their own subnet, they will need tooadd an entry in hello routing table tooconfigure hello gateway as described below.</span></span>

> [!NOTE]
> <span data-ttu-id="9f5a3-162">Virtuella datorer som skapades innan juli 2015 kan ha en standard-gateway som konfigurerats för alla nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-162">VMs created before July 2015 may have a default gateway configured for all NICs.</span></span> <span data-ttu-id="9f5a3-163">hello standardgateway för sekundära nätverkskort tas inte bort förrän dessa virtuella datorer startas om.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-163">hello default gateway for secondary NICs will not be removed until these VMs are rebooted.</span></span> <span data-ttu-id="9f5a3-164">I operativsystem som använder hello svaga värden routning modell, till exempel Linux kan Internetanslutning dela om hello ingående och utgående trafik använder olika nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-164">In Operating systems that use hello weak host routing model, such as Linux, Internet connectivity can break if hello ingress and egress traffic use different NICs.</span></span>
> 

### <a name="configure-windows-vms"></a><span data-ttu-id="9f5a3-165">Konfigurera virtuella Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="9f5a3-165">Configure Windows VMs</span></span>
<span data-ttu-id="9f5a3-166">Anta att du har en virtuell Windows-dator med två nätverkskort på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9f5a3-166">Suppose that you have a Windows VM with two NICs as follows:</span></span>

* <span data-ttu-id="9f5a3-167">Primär NIC IP-adress: 192.168.1.4</span><span class="sxs-lookup"><span data-stu-id="9f5a3-167">Primary NIC IP address: 192.168.1.4</span></span>
* <span data-ttu-id="9f5a3-168">Sekundär NIC IP-adress: 192.168.2.5</span><span class="sxs-lookup"><span data-stu-id="9f5a3-168">Secondary NIC IP address: 192.168.2.5</span></span>

<span data-ttu-id="9f5a3-169">hello IPv4-routningstabellen för den här virtuella datorn skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="9f5a3-169">hello IPv4 route table for this VM would look like this:</span></span>

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

<span data-ttu-id="9f5a3-170">Observera att hello standardvägen (0.0.0.0) är bara tillgängliga toohello primära nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-170">Notice that hello default route (0.0.0.0) is only available toohello primary NIC.</span></span> <span data-ttu-id="9f5a3-171">Du kommer inte att kunna tooaccess resurser utanför hello undernät för hello sekundära NIC, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="9f5a3-171">You will not be able tooaccess resources outside hello subnet for hello secondary NIC, as seen below:</span></span>

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

<span data-ttu-id="9f5a3-172">tooadd standard dirigera på hello sekundära NIC, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="9f5a3-172">tooadd a default route on hello secondary NIC, follow hello steps below:</span></span>

1. <span data-ttu-id="9f5a3-173">Från Kommandotolken kör hello-kommandot nedan tooidentify hello Indextalet för hello sekundära NIC:</span><span class="sxs-lookup"><span data-stu-id="9f5a3-173">From a command prompt, run hello command below tooidentify hello index number for hello secondary NIC:</span></span>
   
        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================
2. <span data-ttu-id="9f5a3-174">Observera hello andra post hello tabell med ett index över 27 (i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="9f5a3-174">Notice hello second entry in hello table, with an index of 27 (in this example).</span></span>
3. <span data-ttu-id="9f5a3-175">Hello-Kommandotolken kör hello **väg lägga till** som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-175">From hello command prompt, run hello **route add** command as shown below.</span></span> <span data-ttu-id="9f5a3-176">I det här exemplet anger du 192.168.2.1 som hello standardgateway för hello sekundära NIC:</span><span class="sxs-lookup"><span data-stu-id="9f5a3-176">In this example, you are specifying 192.168.2.1 as hello default gateway for hello secondary NIC:</span></span>
   
        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27
4. <span data-ttu-id="9f5a3-177">anslutning till tootest, gå tillbaka toohello Kommandotolken och försök tooping ett annat undernät från hello sekundära NIC som visas int FT exemplet nedan:</span><span class="sxs-lookup"><span data-stu-id="9f5a3-177">tootest connectivity, go back toohello command prompt and try tooping a different subnet from hello secondary NIC as shown int eh example below:</span></span>
   
        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5
   
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
5. <span data-ttu-id="9f5a3-178">Du kan också kontrollera din väg tabell toocheck hello nyligen tillagda flödet, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="9f5a3-178">You can also check your route table toocheck hello newly added route, as shown below:</span></span>
   
        C:\Users\Administrator>route print
   
        ...
   
        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a><span data-ttu-id="9f5a3-179">Konfigurera virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="9f5a3-179">Configure Linux VMs</span></span>
<span data-ttu-id="9f5a3-180">För Linux virtuella datorer, eftersom hello standardbeteendet använder svaga värden routning, rekommenderar vi att hello sekundära nätverkskort begränsade tootraffic flöden endast inom hello samma undernät.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-180">For Linux VMs, since hello default behavior uses weak host routing, we recommend that hello secondary NICs are restricted tootraffic flows only within hello same subnet.</span></span> <span data-ttu-id="9f5a3-181">Men om vissa scenarier kräver anslutningar utanför hello undernät, användare bör aktivera principbaserad routning tooensure som hello ingång och utgång trafik använder hello samma nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9f5a3-181">However if certain scenarios demand connectivity outside hello subnet, users should enable policy based routing tooensure that hello ingress and egress traffic uses hello same NIC.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f5a3-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9f5a3-182">Next steps</span></span>
* <span data-ttu-id="9f5a3-183">Distribuera [MultiNIC virtuella datorer i ett scenario med 2-nivåprogram i en Resource Manager distribution](virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="9f5a3-183">Deploy [MultiNIC VMs in a 2-tier application scenario in a Resource Manager deployment](virtual-network-deploy-multinic-arm-template.md).</span></span>
* <span data-ttu-id="9f5a3-184">Distribuera [MultiNIC virtuella datorer i ett scenario 2-nivåprogram klassisk distribution](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="9f5a3-184">Deploy [MultiNIC VMs in a 2-tier application scenario in a classic deployment](virtual-network-deploy-multinic-classic-ps.md).</span></span>

