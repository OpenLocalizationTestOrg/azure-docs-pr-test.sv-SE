---
title: aaaControl routning i ett Azure Virtual Network - CLI - klassisk | Microsoft Docs
description: "Lär dig hur toocontrol routning i Vnet med hjälp av hello Azure CLI i hello klassiska distributionsmodellen"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: ca2b4638-8777-4d30-b972-eb790a7c804f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 07dde573f1a605bf280156c261d51e213ede0cdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-hello-azure-cli"></a><span data-ttu-id="f7acd-103">Kontrollen Routning och använda virtuella installationer (klassiskt) med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f7acd-103">Control routing and use virtual appliances (classic) using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f7acd-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f7acd-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="f7acd-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f7acd-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="f7acd-106">Mall</span><span class="sxs-lookup"><span data-stu-id="f7acd-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="f7acd-107">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="f7acd-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="f7acd-108">CLI (klassisk)</span><span class="sxs-lookup"><span data-stu-id="f7acd-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="f7acd-109">Den här artikeln beskriver hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="f7acd-109">This article covers hello classic deployment model.</span></span> <span data-ttu-id="f7acd-110">Du kan också [styra Routning och använder virtuella installationer i hello Resource Manager-distributionsmodellen](virtual-network-create-udr-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f7acd-110">You can also [control routing and use virtual appliances in hello Resource Manager deployment model](virtual-network-create-udr-arm-cli.md).</span></span>

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="f7acd-111">hello exempel Azure CLI-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="f7acd-111">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="f7acd-112">Om du vill toorun hello kommandon som de visas i det här dokumentet, skapa hello miljö som visas i [skapa ett VNet (klassisk) med hello Azure CLI](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f7acd-112">If you want toorun hello commands as they are displayed in this document, create hello environment shown in [create a VNet (classic) using hello Azure CLI](virtual-networks-create-vnet-classic-cli.md).</span></span>

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="f7acd-113">Skapa hello UDR för hello klientdelen undernät</span><span class="sxs-lookup"><span data-stu-id="f7acd-113">Create hello UDR for hello front end subnet</span></span>
<span data-ttu-id="f7acd-114">routningstabellen för toocreate hello och väg som behövs för hello klientdelen undernät baserat på hello scenariot ovan, följer du hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="f7acd-114">toocreate hello route table and route needed for hello front end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="f7acd-115">Kör följande kommando tooswitch tooclassic läge hello:</span><span class="sxs-lookup"><span data-stu-id="f7acd-115">Run hello following command tooswitch tooclassic mode:</span></span>

    ```azurecli
    azure config mode asm
    ```

    <span data-ttu-id="f7acd-116">Resultat:</span><span class="sxs-lookup"><span data-stu-id="f7acd-116">Output:</span></span>

        info:    New mode is asm

2. <span data-ttu-id="f7acd-117">Kör följande kommando toocreate hello en routningstabell för hello frontend undernät:</span><span class="sxs-lookup"><span data-stu-id="f7acd-117">Run hello following command toocreate a route table for hello front-end subnet:</span></span>

    ```azurecli
    azure network route-table create -n UDR-FrontEnd -l uswest
    ```
   
    <span data-ttu-id="f7acd-118">Resultat:</span><span class="sxs-lookup"><span data-stu-id="f7acd-118">Output:</span></span>
   
        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK
   
    <span data-ttu-id="f7acd-119">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="f7acd-119">Parameters:</span></span>
   
   * <span data-ttu-id="f7acd-120">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="f7acd-120">**-l (or --location)**.</span></span> <span data-ttu-id="f7acd-121">Azure-region där hello ny NSG kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="f7acd-121">Azure region where hello new NSG will be created.</span></span> <span data-ttu-id="f7acd-122">I vårt scenario, *westus*.</span><span class="sxs-lookup"><span data-stu-id="f7acd-122">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="f7acd-123">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="f7acd-123">**-n (or --name)**.</span></span> <span data-ttu-id="f7acd-124">Namn för hello ny NSG.</span><span class="sxs-lookup"><span data-stu-id="f7acd-124">Name for hello new NSG.</span></span> <span data-ttu-id="f7acd-125">I vårt scenario, *NSG-klientdel*.</span><span class="sxs-lookup"><span data-stu-id="f7acd-125">For our scenario, *NSG-FrontEnd*.</span></span>
3. <span data-ttu-id="f7acd-126">Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello backend-undernät (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="f7acd-126">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello back-end subnet (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

    <span data-ttu-id="f7acd-127">Resultat:</span><span class="sxs-lookup"><span data-stu-id="f7acd-127">Output:</span></span>
   
        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK
   
    <span data-ttu-id="f7acd-128">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="f7acd-128">Parameters:</span></span>
   
   * <span data-ttu-id="f7acd-129">**-r (eller--routningstabellnamnet-)**.</span><span class="sxs-lookup"><span data-stu-id="f7acd-129">**-r (or --route-table-name)**.</span></span> <span data-ttu-id="f7acd-130">Namnet på hello routningstabellen där hello flödet kommer att läggas till.</span><span class="sxs-lookup"><span data-stu-id="f7acd-130">Name of hello route table where hello route will be added.</span></span> <span data-ttu-id="f7acd-131">I vårt scenario, *UDR klientdel*.</span><span class="sxs-lookup"><span data-stu-id="f7acd-131">For our scenario, *UDR-FrontEnd*.</span></span>
   * <span data-ttu-id="f7acd-132">**-a (eller --address-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="f7acd-132">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="f7acd-133">Adressprefixet för hello undernät där paket som är avsedda att.</span><span class="sxs-lookup"><span data-stu-id="f7acd-133">Address prefix for hello subnet where packets are destined to.</span></span> <span data-ttu-id="f7acd-134">I vårt scenario, *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="f7acd-134">For our scenario, *192.168.2.0/24*.</span></span>
   * <span data-ttu-id="f7acd-135">**-t (eller--nästa hopptyp)**.</span><span class="sxs-lookup"><span data-stu-id="f7acd-135">**-t (or --next-hop-type)**.</span></span> <span data-ttu-id="f7acd-136">Typ av objekt trafik skickas till.</span><span class="sxs-lookup"><span data-stu-id="f7acd-136">Type of object traffic will be sent to.</span></span> <span data-ttu-id="f7acd-137">Möjliga värden är *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, eller *ingen*.</span><span class="sxs-lookup"><span data-stu-id="f7acd-137">Possible values are *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, or *None*.</span></span>
   * <span data-ttu-id="f7acd-138">**-p (eller--nästa-hopp-ip-adress**).</span><span class="sxs-lookup"><span data-stu-id="f7acd-138">**-p (or --next-hop-ip-address**).</span></span> <span data-ttu-id="f7acd-139">IP-adressen för nästa hopp.</span><span class="sxs-lookup"><span data-stu-id="f7acd-139">IP address for next hop.</span></span> <span data-ttu-id="f7acd-140">I vårt scenario, *192.168.0.4*.</span><span class="sxs-lookup"><span data-stu-id="f7acd-140">For our scenario, *192.168.0.4*.</span></span>
4. <span data-ttu-id="f7acd-141">Hello kör följande kommando tooassociate hello routningstabellen som skapats med hello **klientdel** undernät:</span><span class="sxs-lookup"><span data-stu-id="f7acd-141">Run hello following command tooassociate hello route table created with hello **FrontEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    <span data-ttu-id="f7acd-142">Resultat:</span><span class="sxs-lookup"><span data-stu-id="f7acd-142">Output:</span></span>
   
        info:    Executing command network vnet subnet route-table add
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK    
   
    <span data-ttu-id="f7acd-143">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="f7acd-143">Parameters:</span></span>
   
   * <span data-ttu-id="f7acd-144">**-t (eller--vnet-name)**.</span><span class="sxs-lookup"><span data-stu-id="f7acd-144">**-t (or --vnet-name)**.</span></span> <span data-ttu-id="f7acd-145">Namnet på hello VNet där undernätet hello finns.</span><span class="sxs-lookup"><span data-stu-id="f7acd-145">Name of hello VNet where hello subnet is located.</span></span> <span data-ttu-id="f7acd-146">I vårt scenario, *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="f7acd-146">For our scenario, *TestVNet*.</span></span>
   * <span data-ttu-id="f7acd-147">**-n (eller--undernätsnamn**.</span><span class="sxs-lookup"><span data-stu-id="f7acd-147">**-n (or --subnet-name**.</span></span> <span data-ttu-id="f7acd-148">Namnet på hello routningstabellen för undernätet hello kommer att läggas till.</span><span class="sxs-lookup"><span data-stu-id="f7acd-148">Name of hello subnet hello route table will be added to.</span></span> <span data-ttu-id="f7acd-149">I vårt scenario, *FrontEnd*.</span><span class="sxs-lookup"><span data-stu-id="f7acd-149">For our scenario, *FrontEnd*.</span></span>

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="f7acd-150">Skapa hello UDR för hello backend-undernät</span><span class="sxs-lookup"><span data-stu-id="f7acd-150">Create hello UDR for hello back-end subnet</span></span>
<span data-ttu-id="f7acd-151">routningstabellen för toocreate hello och väg som behövs för hello backend-undernät baserat på hello scenariot fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f7acd-151">toocreate hello route table and route needed for hello back-end subnet based on hello scenario, complete hello following steps:</span></span>

1. <span data-ttu-id="f7acd-152">Kör följande kommando toocreate hello en routningstabell för hello backend-undernät:</span><span class="sxs-lookup"><span data-stu-id="f7acd-152">Run hello following command toocreate a route table for hello back-end subnet:</span></span>

    ```azurecli
    azure network route-table create -n UDR-BackEnd -l uswest
    ```

2. <span data-ttu-id="f7acd-153">Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello frontend undernät (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="f7acd-153">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

3. <span data-ttu-id="f7acd-154">Kör hello följande kommando tooassociate hello routningstabellen med hello **BackEnd** undernät:</span><span class="sxs-lookup"><span data-stu-id="f7acd-154">Run hello following command tooassociate hello route table with hello **BackEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd
    ```

