---
title: "aaaConfigure privata IP-adresser för virtuella datorer – Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur tooconfigure privata IP-adresser för virtuella datorer med hello Azure-kommandoradsgränssnittet (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 40b03a1a-ea00-454c-b716-7574cea49ac0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6caae79c98c7bc5f246b7bb3bb8bd8f032eb2d8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-cli-10"></a><span data-ttu-id="8ebb7-103">Konfigurera privat IP-adresser för en virtuell dator med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8ebb7-103">Configure private IP addresses for a virtual machine using hello Azure CLI 1.0</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="8ebb7-104">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="8ebb7-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="8ebb7-105">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="8ebb7-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="8ebb7-106">[Azure CLI 1.0](#how-to-specify-a-static-private-ip-address-when-creating-a-vm) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="8ebb7-106">[Azure CLI 1.0](#how-to-specify-a-static-private-ip-address-when-creating-a-vm) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="8ebb7-107">[Azure CLI 2.0](virtual-networks-static-private-ip-arm-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="8ebb7-107">[Azure CLI 2.0](virtual-networks-static-private-ip-arm-cli.md) - our next-generation CLI for hello resource management deployment model</span></span> 

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="8ebb7-108">Den här artikeln beskriver hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="8ebb7-109">Du kan också [hantera statisk privat IP-adress i hello klassiska distributionsmodellen](virtual-networks-static-private-ip-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="8ebb7-109">You can also [manage static private IP address in hello classic deployment model](virtual-networks-static-private-ip-classic-cli.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="8ebb7-110">hello exempel Azure CLI-kommandona nedan förväntar sig en enkel miljö som redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-110">hello sample Azure CLI commands below expect a simple environment already created.</span></span> <span data-ttu-id="8ebb7-111">Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö som beskrivs i [skapa ett vnet](virtual-networks-create-vnet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="8ebb7-111">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-arm-cli.md).</span></span>

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="8ebb7-112">Hur toospecify en statisk privat IP-adress när du skapar en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="8ebb7-112">How toospecify a static private IP address when creating a VM</span></span>
<span data-ttu-id="8ebb7-113">toocreate en virtuell dator med namnet *DNS01* i hello *klientdel* undernätet i ett VNet med namnet *TestVNet* med en statisk privat IP-adress för *192.168.1.101*, Följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="8ebb7-113">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow hello steps below:</span></span>

1. <span data-ttu-id="8ebb7-114">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-114">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="8ebb7-115">Kör hello **azure config mode** kommandot tooswitch tooResource Manager-läge enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-115">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>
   
        azure config mode arm
   
    <span data-ttu-id="8ebb7-116">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="8ebb7-116">Expected output:</span></span>
   
        info:    New mode is arm
3. <span data-ttu-id="8ebb7-117">Kör hello **azure-nätverk offentliga IP-skapa** toocreate en offentlig IP-adress för hello VM.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-117">Run hello **azure network public-ip create** toocreate a public IP for hello VM.</span></span> <span data-ttu-id="8ebb7-118">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-118">hello list shown after hello output explains hello parameters used.</span></span>
   
        azure network public-ip create -g TestRG -n TestPIP -l centralus
   
    <span data-ttu-id="8ebb7-119">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="8ebb7-119">Expected output:</span></span>
   
        info:    Executing command network public-ip create
        + Looking up hello public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up hello public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK
   
   * <span data-ttu-id="8ebb7-120">**-g (eller --resource-group)**.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-120">**-g (or --resource-group)**.</span></span> <span data-ttu-id="8ebb7-121">Namnet på hello resurs grupp hello offentliga IP-Adressen kommer att skapas i.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-121">Name of hello resource group hello public IP will be created in.</span></span>
   * <span data-ttu-id="8ebb7-122">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-122">**-n (or --name)**.</span></span> <span data-ttu-id="8ebb7-123">Namnet på hello offentlig IP.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-123">Name of hello public IP.</span></span>
   * <span data-ttu-id="8ebb7-124">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-124">**-l (or --location)**.</span></span> <span data-ttu-id="8ebb7-125">Azure-region där hello offentliga IP-Adressen kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-125">Azure region where hello public IP will be created.</span></span> <span data-ttu-id="8ebb7-126">I vårt scenario, *centralus*.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-126">For our scenario, *centralus*.</span></span>
4. <span data-ttu-id="8ebb7-127">Kör hello **azure-nätverk nic skapa** kommandot toocreate ett nätverkskort med en statisk privat IP.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-127">Run hello **azure network nic create** command toocreate a NIC with a static private IP.</span></span> <span data-ttu-id="8ebb7-128">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-128">hello list shown after hello output explains hello parameters used.</span></span>
   
        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd
   
    <span data-ttu-id="8ebb7-129">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="8ebb7-129">Expected output:</span></span>
   
        + Looking up hello network interface "TestNIC"
        + Looking up hello subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up hello network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK
   
   * <span data-ttu-id="8ebb7-130">**-a (eller--privata IP-adress)**.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-130">**-a (or --private-ip-address)**.</span></span> <span data-ttu-id="8ebb7-131">Statisk privat IP-adress för hello NIC.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-131">Static private IP address for hello NIC.</span></span>
   * <span data-ttu-id="8ebb7-132">**-m (eller--vnet undernätsnamn)**.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-132">**-m (or --subnet-vnet-name)**.</span></span> <span data-ttu-id="8ebb7-133">Namnet på hello VNet där hello NIC kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-133">Name of hello VNet where hello NIC will be created.</span></span>
   * <span data-ttu-id="8ebb7-134">**-k (eller--undernät-namn)**.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-134">**-k (or --subnet-name)**.</span></span> <span data-ttu-id="8ebb7-135">Namnet på hello undernät där hello NIC kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-135">Name of hello subnet where hello NIC will be created.</span></span>
5. <span data-ttu-id="8ebb7-136">Kör hello **azure vm skapa** kommandot toocreate hello skapas den virtuella datorn med hjälp av hello offentlig IP-adress och NIC ovan.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-136">Run hello **azure vm create** command toocreate hello VM using hello public IP and NIC created above.</span></span> <span data-ttu-id="8ebb7-137">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-137">hello list shown after hello output explains hello parameters used.</span></span>
   
        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd
   
    <span data-ttu-id="8ebb7-138">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="8ebb7-138">Expected output:</span></span>
   
        info:    Executing command vm create
        + Looking up hello VM "DNS01"
        info:    Using hello VM Size "Standard_A1"
        warn:    hello image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    hello [OS, Data] Disk or image configuration requires storage account
        + Looking up hello storage account vnetstorage
        + Looking up hello NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in hello NIC "TestNIC"
        info:    Found public ip parameters, trying toosetup PublicIP profile
        + Looking up hello public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up hello NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK
   
   * <span data-ttu-id="8ebb7-139">**-y (eller--os-typ)**.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-139">**-y (or --os-type)**.</span></span> <span data-ttu-id="8ebb7-140">Typ av operativsystem för hello VM, antingen *Windows* eller *Linux*.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-140">Type of operating system for hello VM, either *Windows* or *Linux*.</span></span>
   * <span data-ttu-id="8ebb7-141">**-f (eller--nic-namn)**.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-141">**-f (or --nic-name)**.</span></span> <span data-ttu-id="8ebb7-142">Namnet på hello NIC hello VM använder.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-142">Name of hello NIC hello VM will use.</span></span>
   * <span data-ttu-id="8ebb7-143">**-i (eller--offentliga IP-namn)**.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-143">**-i (or --public-ip-name)**.</span></span> <span data-ttu-id="8ebb7-144">Namnet på hello offentliga IP-hello VM använder.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-144">Name of hello public IP hello VM will use.</span></span>
   * <span data-ttu-id="8ebb7-145">**-F (eller--vnet-name)**.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-145">**-F (or --vnet-name)**.</span></span> <span data-ttu-id="8ebb7-146">Namnet på hello VNet där hello VM kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-146">Name of hello VNet where hello VM will be created.</span></span>
   * <span data-ttu-id="8ebb7-147">**-j (eller--vnet undernätsnamn)**.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-147">**-j (or --vnet-subnet-name)**.</span></span> <span data-ttu-id="8ebb7-148">Namnet på hello undernät där hello VM kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-148">Name of hello subnet where hello VM will be created.</span></span>

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="8ebb7-149">Hur tooretrieve statiska privata IP-information för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="8ebb7-149">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="8ebb7-150">tooview hello statiska privata IP-information för hello skapas den virtuella datorn med hello skriptet ovan, kör följande Azure CLI kommando hello och se hello värden för *privat IP allokeringsenhets-metoden* och *privata IP-adressen*:</span><span class="sxs-lookup"><span data-stu-id="8ebb7-150">tooview hello static private IP address information for hello VM created with hello script above, run hello following Azure CLI command and observe hello values for *Private IP alloc-method* and *Private IP address*:</span></span>

    azure vm show -g TestRG -n DNS01

<span data-ttu-id="8ebb7-151">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="8ebb7-151">Expected output:</span></span>

    info:    Executing command vm show
    + Looking up hello VM "DNS01"
    + Looking up hello NIC "TestNIC"
    + Looking up hello public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="8ebb7-152">Hur tooremove en statisk privat IP-adress från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="8ebb7-152">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="8ebb7-153">Du kan inte ta bort en statisk privat IP-adress från ett nätverkskort i Azure CLI för Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-153">You cannot remove a static private IP address from a NIC in Azure CLI for Resource Manager.</span></span> <span data-ttu-id="8ebb7-154">Du måste skapa ett nytt nätverkskort som använder en dynamisk IP-adress, ta bort hello tidigare NIC från hello VM och Lägg sedan till hello nya NIC toohello VM.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-154">You must create a new NIC that uses a dynamic IP, remove hello previous NIC from hello VM, and then add hello new NIC toohello VM.</span></span> <span data-ttu-id="8ebb7-155">toochange Hej NIC för hello VM används int FT kommandona ovan, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-155">toochange hello NIC for hello VM used int eh commands above, follow hello steps below.</span></span>

1. <span data-ttu-id="8ebb7-156">Kör hello **azure-nätverk nic skapa** kommandot toocreate ett nytt nätverkskort med hjälp av dynamisk IP-allokering.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-156">Run hello **azure network nic create** command toocreate a new NIC using dynamic IP allocation.</span></span> <span data-ttu-id="8ebb7-157">Observera hur behöver du inte toospecify hello IP-adress nu.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-157">Notice how you do not need toospecify hello IP address this time.</span></span>
   
        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd
   
    <span data-ttu-id="8ebb7-158">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="8ebb7-158">Expected output:</span></span>
   
        info:    Executing command network nic create
        + Looking up hello network interface "TestNIC2"
        + Looking up hello subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up hello network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK
2. <span data-ttu-id="8ebb7-159">Kör hello **azure vm set** kommandot toochange hello NIC som används av hello VM.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-159">Run hello **azure vm set** command toochange hello NIC used by hello VM.</span></span>
   
        azure vm set -g TestRG -n DNS01 -N TestNIC2
   
    <span data-ttu-id="8ebb7-160">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="8ebb7-160">Expected output:</span></span>
   
        info:    Executing command vm set
        + Looking up hello VM "DNS01"
        + Looking up hello NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK
3. <span data-ttu-id="8ebb7-161">Om du vill kan du köra hello **ta bort azure-nätverk nic** kommandot toodelete hello gamla NIC.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-161">If wanted, run hello **azure network nic delete** command toodelete hello old NIC.</span></span>
   
        azure network nic delete -g TestRG -n TestNIC --quiet
   
    <span data-ttu-id="8ebb7-162">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="8ebb7-162">Expected output:</span></span>
   
        info:    Executing command network nic delete
        + Looking up hello network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a><span data-ttu-id="8ebb7-163">Hur tooadd en statisk privat IP-adress tooan befintliga VM</span><span class="sxs-lookup"><span data-stu-id="8ebb7-163">How tooadd a static private IP address tooan existing VM</span></span>
<span data-ttu-id="8ebb7-164">tooadd en statisk privat IP-adress toohello nätverkskort som används av den virtuella datorn skapas med hjälp av hello skriptet ovan, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8ebb7-164">tooadd a static private IP address toohello NIC used by teh VM created using hello script above, run hello following command:</span></span>

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

<span data-ttu-id="8ebb7-165">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="8ebb7-165">Expected output:</span></span>

    info:    Executing command network nic set
    + Looking up hello network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up hello network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a><span data-ttu-id="8ebb7-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8ebb7-166">Next steps</span></span>
* <span data-ttu-id="8ebb7-167">Lär dig mer om [reserverade offentliga IP-Adressen](virtual-networks-reserved-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-167">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="8ebb7-168">Lär dig mer om [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="8ebb7-168">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="8ebb7-169">Kontakta hello [reserverade IP-REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="8ebb7-169">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

