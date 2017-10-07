---
title: "ett virtuellt nätverk som använder aaaCreate hello Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur toocreate ett virtuellt nätverk som använder hello Azure CLI 1.0 | Resource Manager."
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.openlocfilehash: f48a8b23a5994164b71c9b51ee8a6810d17f9392
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-hello-azure-cli"></a><span data-ttu-id="44a21-103">Skapa ett virtuellt nätverk med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="44a21-103">Create a virtual network using hello Azure CLI</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="44a21-104">Azure har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="44a21-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="44a21-105">Microsoft rekommenderar att skapa resurser via hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="44a21-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="44a21-106">Mer om toolearn hello skillnaderna mellan hello två modeller, läsa hello [förstå Azure distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="44a21-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="44a21-107">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="44a21-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="44a21-108">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="44a21-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="44a21-109">[Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="44a21-109">[Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md) - our next generation CLI for hello resource management deployment model</span></span>
- <span data-ttu-id="44a21-110">[Azure CLI 1.0](#create-a-virtual-network) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="44a21-110">[Azure CLI 1.0](#create-a-virtual-network) – our CLI for hello classic and resource management deployment models (this article)</span></span>

 
[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a><span data-ttu-id="44a21-111">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="44a21-111">Create a virtual network</span></span>

<span data-ttu-id="44a21-112">ett virtuellt nätverk som använder toocreate hello Azure CLI, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="44a21-112">toocreate a virtual network using hello Azure CLI, complete hello following steps:</span></span>

1. <span data-ttu-id="44a21-113">Installera och konfigurera hello Azure CLI med följande hello steg i hello [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="44a21-113">Install and configure hello Azure CLI by following hello steps in hello [Install and Configure hello Azure CLI](../cli-install-nodejs.md) article.</span></span>

2. <span data-ttu-id="44a21-114">Skapa ett VNet och ett undernät:</span><span class="sxs-lookup"><span data-stu-id="44a21-114">Create a VNet and a subnet:</span></span>

    ```azurecli
    azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    ```

    <span data-ttu-id="44a21-115">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="44a21-115">Expected output:</span></span>
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    <span data-ttu-id="44a21-116">Parametrar som används:</span><span class="sxs-lookup"><span data-stu-id="44a21-116">Parameters used:</span></span>

   * <span data-ttu-id="44a21-117">**--vnet**.</span><span class="sxs-lookup"><span data-stu-id="44a21-117">**--vnet**.</span></span> <span data-ttu-id="44a21-118">Namnet på hello VNet toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="44a21-118">Name of hello VNet toobe created.</span></span> <span data-ttu-id="44a21-119">I vårt exempel, *TestVNet*</span><span class="sxs-lookup"><span data-stu-id="44a21-119">For our scenario, *TestVNet*</span></span>
   * <span data-ttu-id="44a21-120">**-e (eller---adressutrymmet)**.</span><span class="sxs-lookup"><span data-stu-id="44a21-120">**-e (or --address-space)**.</span></span> <span data-ttu-id="44a21-121">VNet-adressutrymmet.</span><span class="sxs-lookup"><span data-stu-id="44a21-121">VNet address space.</span></span> <span data-ttu-id="44a21-122">I vårt scenario, *192.168.0.0*</span><span class="sxs-lookup"><span data-stu-id="44a21-122">For our scenario, *192.168.0.0*</span></span>
   * <span data-ttu-id="44a21-123">**-i (eller - cidr)**.</span><span class="sxs-lookup"><span data-stu-id="44a21-123">**-i (or -cidr)**.</span></span> <span data-ttu-id="44a21-124">Nätverksmasken i CIDR-format.</span><span class="sxs-lookup"><span data-stu-id="44a21-124">Network mask in CIDR format.</span></span> <span data-ttu-id="44a21-125">I vårt scenario, *16*.</span><span class="sxs-lookup"><span data-stu-id="44a21-125">For our scenario, *16*.</span></span>
   * <span data-ttu-id="44a21-126">**-n (eller--undernätsnamn**).</span><span class="sxs-lookup"><span data-stu-id="44a21-126">**-n (or --subnet-name**).</span></span> <span data-ttu-id="44a21-127">Namnet på hello första undernätet.</span><span class="sxs-lookup"><span data-stu-id="44a21-127">Name of hello first subnet.</span></span> <span data-ttu-id="44a21-128">I vårt scenario, *FrontEnd*.</span><span class="sxs-lookup"><span data-stu-id="44a21-128">For our scenario, *FrontEnd*.</span></span>
   * <span data-ttu-id="44a21-129">**-p (eller--undernät-start-ip)**.</span><span class="sxs-lookup"><span data-stu-id="44a21-129">**-p (or --subnet-start-ip)**.</span></span> <span data-ttu-id="44a21-130">Första IP-adressen för undernätet eller undernätsadressutrymmet.</span><span class="sxs-lookup"><span data-stu-id="44a21-130">Starting IP address for subnet, or subnet address space.</span></span> <span data-ttu-id="44a21-131">I vårt scenario, *192.168.1.0*.</span><span class="sxs-lookup"><span data-stu-id="44a21-131">For our scenario, *192.168.1.0*.</span></span>
   * <span data-ttu-id="44a21-132">**-r (eller--undernät cidr)**.</span><span class="sxs-lookup"><span data-stu-id="44a21-132">**-r (or --subnet-cidr)**.</span></span> <span data-ttu-id="44a21-133">Nätverksmasken i CIDR-format för undernätet.</span><span class="sxs-lookup"><span data-stu-id="44a21-133">Network mask in CIDR format for subnet.</span></span> <span data-ttu-id="44a21-134">I vårt scenario, *24*.</span><span class="sxs-lookup"><span data-stu-id="44a21-134">For our scenario, *24*.</span></span>
   * <span data-ttu-id="44a21-135">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="44a21-135">**-l (or --location)**.</span></span> <span data-ttu-id="44a21-136">Azure-region där hello VNet har skapats.</span><span class="sxs-lookup"><span data-stu-id="44a21-136">Azure region where hello VNet is created.</span></span> <span data-ttu-id="44a21-137">I vårt scenario, *centrala USA*.</span><span class="sxs-lookup"><span data-stu-id="44a21-137">For our scenario, *Central US*.</span></span>

3. <span data-ttu-id="44a21-138">Skapa ett undernät:</span><span class="sxs-lookup"><span data-stu-id="44a21-138">Create a subnet:</span></span>

    ```azurecli
    azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    ```
   
    <span data-ttu-id="44a21-139">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="44a21-139">Expected output:</span></span>

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up hello subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    <span data-ttu-id="44a21-140">Parametrar som används:</span><span class="sxs-lookup"><span data-stu-id="44a21-140">Parameters used:</span></span>

   * <span data-ttu-id="44a21-141">**-t (eller--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="44a21-141">**-t (or --vnet-name**.</span></span> <span data-ttu-id="44a21-142">Namnet på hello VNet där undernätet hello kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="44a21-142">Name of hello VNet where hello subnet will be created.</span></span> <span data-ttu-id="44a21-143">I vårt scenario, *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="44a21-143">For our scenario, *TestVNet*.</span></span>
   * <span data-ttu-id="44a21-144">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="44a21-144">**-n (or --name)**.</span></span> <span data-ttu-id="44a21-145">Namnet på hello Nytt undernät.</span><span class="sxs-lookup"><span data-stu-id="44a21-145">Name of hello new subnet.</span></span> <span data-ttu-id="44a21-146">I vårt scenario, *BackEnd*.</span><span class="sxs-lookup"><span data-stu-id="44a21-146">For our scenario, *BackEnd*.</span></span>
   * <span data-ttu-id="44a21-147">**-a (eller --address-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="44a21-147">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="44a21-148">CIDR-block för undernätet.</span><span class="sxs-lookup"><span data-stu-id="44a21-148">Subnet CIDR block.</span></span> <span data-ttu-id="44a21-149">Fyra vårt scenario, *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="44a21-149">Four our scenario, *192.168.2.0/24*.</span></span>
   
4. <span data-ttu-id="44a21-150">tooview hello egenskaper för hello nya VNet:</span><span class="sxs-lookup"><span data-stu-id="44a21-150">tooview hello properties of hello new VNet:</span></span>

    ```azurecli
    azure network vnet show
    ```
   
    <span data-ttu-id="44a21-151">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="44a21-151">Expected output:</span></span>
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up hello virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

## <a name="next-steps"></a><span data-ttu-id="44a21-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="44a21-152">Next steps</span></span>

<span data-ttu-id="44a21-153">Lär dig hur tooconnect:</span><span class="sxs-lookup"><span data-stu-id="44a21-153">Learn how tooconnect:</span></span>

- <span data-ttu-id="44a21-154">En virtuell dator (VM) tooa virtuellt nätverk genom att läsa hello [skapa ett Linux VM](../virtual-machines/linux/quick-create-cli.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="44a21-154">A virtual machine (VM) tooa virtual network by reading hello [Create a Linux VM](../virtual-machines/linux/quick-create-cli.md) article.</span></span> <span data-ttu-id="44a21-155">I stället för att skapa en VNet och undernät i hello steg hello artiklar, kan du välja ett befintligt VNet och undernät tooconnect en virtuell dator till.</span><span class="sxs-lookup"><span data-stu-id="44a21-155">Instead of creating a VNet and subnet in hello steps of hello articles, you can select an existing VNet and subnet tooconnect a VM to.</span></span>
- <span data-ttu-id="44a21-156">Hej virtuellt nätverk tooother virtuella nätverk genom att läsa hello [ansluta Vnet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="44a21-156">hello virtual network tooother virtual networks by reading hello [Connect VNets](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) article.</span></span>
- <span data-ttu-id="44a21-157">hello virtuellt nätverk tooan lokalt nätverk via ett plats-till-plats virtuellt privat nätverk (VPN) eller en ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="44a21-157">hello virtual network tooan on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="44a21-158">Lär dig hur genom att läsa hello [ansluta ett virtuellt nätverk tooan lokalt nätverk med hjälp av en plats-till-plats-VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) och [länka VNet-tooan ExpressRoute-krets](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="44a21-158">Learn how by reading hello [Connect a VNet tooan on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet tooan ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span></span>
