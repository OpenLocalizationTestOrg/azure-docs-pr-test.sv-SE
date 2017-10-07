---
title: "aaaControl Routning och virtuella installationer med hjälp av hello Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur toocontrol Routning och virtuella installationer med hjälp av hello Azure CLI 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2017
ms.author: jdial
ms.openlocfilehash: 1c8a552d949521fa554880c00405e65fa47a8162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-hello-azure-cli-10"></a><span data-ttu-id="d183b-103">Skapa användardefinierade vägar (UDR) med hjälp av hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d183b-103">Create User-Defined Routes (UDR) using hello Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d183b-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d183b-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="d183b-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d183b-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="d183b-106">Mall</span><span class="sxs-lookup"><span data-stu-id="d183b-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="d183b-107">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="d183b-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="d183b-108">CLI (klassisk)</span><span class="sxs-lookup"><span data-stu-id="d183b-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)

<span data-ttu-id="d183b-109">Skapa anpassad Routning och virtuella installationer med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="d183b-109">Create custom routing and virtual appliances using hello Azure CLI.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="d183b-110">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="d183b-110">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="d183b-111">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="d183b-111">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="d183b-112">[Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="d183b-112">[Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="d183b-113">[Azure CLI 2.0](virtual-network-create-udr-arm-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="d183b-113">[Azure CLI 2.0](virtual-network-create-udr-arm-cli.md) - our next generation CLI for hello resource management deployment model</span></span> 


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="d183b-114">hello exempel Azure CLI-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="d183b-114">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="d183b-115">Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö genom att distribuera [mallen](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klickar du på **distribuera tooAzure**, Ersätt hello Standardparametervärden Om nödvändigt och hello anvisningar i hello portal.</span><span class="sxs-lookup"><span data-stu-id="d183b-115">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>


## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="d183b-116">Skapa hello UDR för hello frontend undernät</span><span class="sxs-lookup"><span data-stu-id="d183b-116">Create hello UDR for hello front-end subnet</span></span>
<span data-ttu-id="d183b-117">routningstabellen för toocreate hello och väg som behövs för hello klientdelen undernät baserat på hello scenariot ovan, följer du hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="d183b-117">toocreate hello route table and route needed for hello front end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="d183b-118">Kör följande kommando toocreate hello en routningstabell för hello frontend undernät:</span><span class="sxs-lookup"><span data-stu-id="d183b-118">Run hello following command toocreate a route table for hello front-end subnet:</span></span>

    ```azurecli
    azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest
    ```
   
    <span data-ttu-id="d183b-119">Resultat:</span><span class="sxs-lookup"><span data-stu-id="d183b-119">Output:</span></span>
   
        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK
   
    <span data-ttu-id="d183b-120">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="d183b-120">Parameters:</span></span>
   
   * <span data-ttu-id="d183b-121">**-g (eller --resource-group)**.</span><span class="sxs-lookup"><span data-stu-id="d183b-121">**-g (or --resource-group)**.</span></span> <span data-ttu-id="d183b-122">Namnet på hello resursgruppen där hello UDR kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="d183b-122">Name of hello resource group where hello UDR will be created.</span></span> <span data-ttu-id="d183b-123">I vårt exempel, *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="d183b-123">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="d183b-124">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="d183b-124">**-l (or --location)**.</span></span> <span data-ttu-id="d183b-125">Azure-region där hello nya UDR kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="d183b-125">Azure region where hello new UDR will be created.</span></span> <span data-ttu-id="d183b-126">I vårt scenario, *westus*.</span><span class="sxs-lookup"><span data-stu-id="d183b-126">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="d183b-127">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="d183b-127">**-n (or --name)**.</span></span> <span data-ttu-id="d183b-128">Namn för hello nya UDR.</span><span class="sxs-lookup"><span data-stu-id="d183b-128">Name for hello new UDR.</span></span> <span data-ttu-id="d183b-129">I vårt scenario, *UDR klientdel*.</span><span class="sxs-lookup"><span data-stu-id="d183b-129">For our scenario, *UDR-FrontEnd*.</span></span>
2. <span data-ttu-id="d183b-130">Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello backend-undernät (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="d183b-130">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello back-end subnet (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4
    ```
   
    <span data-ttu-id="d183b-131">Resultat:</span><span class="sxs-lookup"><span data-stu-id="d183b-131">Output:</span></span>
   
        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK
   
    <span data-ttu-id="d183b-132">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="d183b-132">Parameters:</span></span>
   
   * <span data-ttu-id="d183b-133">**-r (eller--routningstabellnamnet-)**.</span><span class="sxs-lookup"><span data-stu-id="d183b-133">**-r (or --route-table-name)**.</span></span> <span data-ttu-id="d183b-134">Namnet på hello routningstabellen där hello flödet kommer att läggas till.</span><span class="sxs-lookup"><span data-stu-id="d183b-134">Name of hello route table where hello route will be added.</span></span> <span data-ttu-id="d183b-135">I vårt scenario, *UDR klientdel*.</span><span class="sxs-lookup"><span data-stu-id="d183b-135">For our scenario, *UDR-FrontEnd*.</span></span>
   * <span data-ttu-id="d183b-136">**-a (eller --address-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="d183b-136">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="d183b-137">Adressprefixet för hello undernät där paket som är avsedda att.</span><span class="sxs-lookup"><span data-stu-id="d183b-137">Address prefix for hello subnet where packets are destined to.</span></span> <span data-ttu-id="d183b-138">I vårt scenario, *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="d183b-138">For our scenario, *192.168.2.0/24*.</span></span>
   * <span data-ttu-id="d183b-139">**-y (eller--nästa hopptyp)**.</span><span class="sxs-lookup"><span data-stu-id="d183b-139">**-y (or --next-hop-type)**.</span></span> <span data-ttu-id="d183b-140">Typ av objekt trafik skickas till.</span><span class="sxs-lookup"><span data-stu-id="d183b-140">Type of object traffic will be sent to.</span></span> <span data-ttu-id="d183b-141">Möjliga värden är *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, eller *ingen*.</span><span class="sxs-lookup"><span data-stu-id="d183b-141">Possible values are *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, or *None*.</span></span>
   * <span data-ttu-id="d183b-142">**-p (eller--nästa-hopp-ip-adress**).</span><span class="sxs-lookup"><span data-stu-id="d183b-142">**-p (or --next-hop-ip-address**).</span></span> <span data-ttu-id="d183b-143">IP-adressen för nästa hopp.</span><span class="sxs-lookup"><span data-stu-id="d183b-143">IP address for next hop.</span></span> <span data-ttu-id="d183b-144">I vårt scenario, *192.168.0.4*.</span><span class="sxs-lookup"><span data-stu-id="d183b-144">For our scenario, *192.168.0.4*.</span></span>
3. <span data-ttu-id="d183b-145">Hello kör följande kommando tooassociate hello routningstabellen skapade ovan med hello **klientdel** undernät:</span><span class="sxs-lookup"><span data-stu-id="d183b-145">Run hello following command tooassociate hello route table created above with hello **FrontEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    <span data-ttu-id="d183b-146">Resultat:</span><span class="sxs-lookup"><span data-stu-id="d183b-146">Output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up hello subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
   
    <span data-ttu-id="d183b-147">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="d183b-147">Parameters:</span></span>
   
   * <span data-ttu-id="d183b-148">**-e (eller--vnet-name)**.</span><span class="sxs-lookup"><span data-stu-id="d183b-148">**-e (or --vnet-name)**.</span></span> <span data-ttu-id="d183b-149">Namnet på hello VNet där undernätet hello finns.</span><span class="sxs-lookup"><span data-stu-id="d183b-149">Name of hello VNet where hello subnet is located.</span></span> <span data-ttu-id="d183b-150">I vårt scenario, *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="d183b-150">For our scenario, *TestVNet*.</span></span>

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="d183b-151">Skapa hello UDR för hello backend-undernät</span><span class="sxs-lookup"><span data-stu-id="d183b-151">Create hello UDR for hello back-end subnet</span></span>
<span data-ttu-id="d183b-152">toocreate hello routningstabellen och dirigera behövs för hello backend-undernät baserat på hello scenariot ovan fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d183b-152">toocreate hello route table and route needed for hello back-end subnet based on hello scenario above, complete hello following steps:</span></span>

1. <span data-ttu-id="d183b-153">Kör följande kommando toocreate hello en routningstabell för hello backend-undernät:</span><span class="sxs-lookup"><span data-stu-id="d183b-153">Run hello following command toocreate a route table for hello back-end subnet:</span></span>

    ```azurecli
    azure network route-table create -g TestRG -n UDR-BackEnd -l westus
    ```

2. <span data-ttu-id="d183b-154">Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello frontend undernät (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="d183b-154">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4
    ```

3. <span data-ttu-id="d183b-155">Kör hello följande kommando tooassociate hello routningstabellen med hello **BackEnd** undernät:</span><span class="sxs-lookup"><span data-stu-id="d183b-155">Run hello following command tooassociate hello route table with hello **BackEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="d183b-156">Aktivera IP-vidarebefordring på FW1</span><span class="sxs-lookup"><span data-stu-id="d183b-156">Enable IP forwarding on FW1</span></span>
<span data-ttu-id="d183b-157">tooenable IP-vidarebefordran i hello NIC som används av **FW1**, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d183b-157">tooenable IP forwarding in hello NIC used by **FW1**, complete hello following steps:</span></span>

1. <span data-ttu-id="d183b-158">Kör hello-kommando som följer och Observera hello värdet för **aktivera IP-vidarebefordring**.</span><span class="sxs-lookup"><span data-stu-id="d183b-158">Run hello command that follows and notice hello value for **Enable IP forwarding**.</span></span> <span data-ttu-id="d183b-159">Det ska ställas in för*FALSKT*.</span><span class="sxs-lookup"><span data-stu-id="d183b-159">It should be set too*false*.</span></span>

    ```azurecli
    azure network nic show -g TestRG -n NICFW1
    ```

    <span data-ttu-id="d183b-160">Resultat:</span><span class="sxs-lookup"><span data-stu-id="d183b-160">Output:</span></span>
   
        info:    Executing command network nic show
        info:    Looking up hello network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK
2. <span data-ttu-id="d183b-161">Kör följande kommando tooenable IP-vidarebefordring hello:</span><span class="sxs-lookup"><span data-stu-id="d183b-161">Run hello following command tooenable IP forwarding:</span></span>

    ```azurecli
    azure network nic set -g TestRG -n NICFW1 -f true
    ```
   
    <span data-ttu-id="d183b-162">Resultat:</span><span class="sxs-lookup"><span data-stu-id="d183b-162">Output:</span></span>
   
        info:    Executing command network nic set
        info:    Looking up hello network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up hello network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK
   
    <span data-ttu-id="d183b-163">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="d183b-163">Parameters:</span></span>
   
   * <span data-ttu-id="d183b-164">**-f (eller--aktivera IP-vidarebefordring)**.</span><span class="sxs-lookup"><span data-stu-id="d183b-164">**-f (or --enable-ip-forwarding)**.</span></span> <span data-ttu-id="d183b-165">*SANT* eller *FALSKT*.</span><span class="sxs-lookup"><span data-stu-id="d183b-165">*true* or *false*.</span></span>

