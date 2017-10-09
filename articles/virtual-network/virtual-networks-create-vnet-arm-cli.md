---
title: "aaaCreate ett virtuellt nätverk - Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur toocreate ett virtuellt nätverk som använder hello Azure CLI 2.0."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 75966bcc-0056-4667-8482-6f08ca38e77a
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e79b7fe780fc81f4866f810d830824e43a5a43b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-hello-azure-cli-20"></a><span data-ttu-id="3762a-103">Skapa ett virtuellt nätverk med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="3762a-103">Create a virtual network using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="3762a-104">Azure har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="3762a-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="3762a-105">Microsoft rekommenderar att skapa resurser via hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="3762a-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="3762a-106">Mer om toolearn hello skillnaderna mellan hello två modeller, läsa hello [förstå Azure distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3762a-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="3762a-107">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="3762a-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="3762a-108">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="3762a-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="3762a-109">[Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md) – våra CLI för hello klassisk och resurs management distributionsmodeller</span><span class="sxs-lookup"><span data-stu-id="3762a-109">[Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span>
- <span data-ttu-id="3762a-110">[Azure CLI 2.0](#create-a-virtual-network) -vår nästa generations CLI för hello management resursdistributionsmodell (den här artikeln) ”</span><span class="sxs-lookup"><span data-stu-id="3762a-110">[Azure CLI 2.0](#create-a-virtual-network) - our next generation CLI for hello resource management deployment model (this article)\`</span></span>
 
    <span data-ttu-id="3762a-111">Du kan också skapa ett VNet via Resource Manager med andra verktyg eller skapa ett VNet via hello klassiska distributionsmodellen genom att välja ett annat alternativ hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="3762a-111">You can also create a VNet through Resource Manager using other tools or create a VNet through hello classic deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="3762a-112">Portal</span><span class="sxs-lookup"><span data-stu-id="3762a-112">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
> * [<span data-ttu-id="3762a-113">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3762a-113">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
> * [<span data-ttu-id="3762a-114">CLI</span><span class="sxs-lookup"><span data-stu-id="3762a-114">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
> * [<span data-ttu-id="3762a-115">Mall</span><span class="sxs-lookup"><span data-stu-id="3762a-115">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
> * [<span data-ttu-id="3762a-116">Portal (klassisk)</span><span class="sxs-lookup"><span data-stu-id="3762a-116">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
> * [<span data-ttu-id="3762a-117">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="3762a-117">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [<span data-ttu-id="3762a-118">CLI (klassisk)</span><span class="sxs-lookup"><span data-stu-id="3762a-118">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]


## <a name="create-a-virtual-network"></a><span data-ttu-id="3762a-119">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="3762a-119">Create a virtual network</span></span>

<span data-ttu-id="3762a-120">ett virtuellt nätverk som använder toocreate hello Azure CLI 2.0, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3762a-120">toocreate a virtual network using hello Azure CLI 2.0, complete hello following steps:</span></span>

1. <span data-ttu-id="3762a-121">Installera och konfigurera hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="3762a-121">Install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

2. <span data-ttu-id="3762a-122">Skapa en resursgrupp för ditt VNet med hjälp av hello [az gruppen skapa](/cli/azure/group#create) med hello `--name` och `--location` argument:</span><span class="sxs-lookup"><span data-stu-id="3762a-122">Create a resource group for your VNet using hello [az group create](/cli/azure/group#create) command with hello `--name` and `--location` arguments:</span></span>

    ```azurecli
    az group create --name TestRG --location centralus
    ```

3. <span data-ttu-id="3762a-123">Skapa ett VNet och ett undernät:</span><span class="sxs-lookup"><span data-stu-id="3762a-123">Create a VNet and a subnet:</span></span>

    ```azurecli
    az network vnet create \
    --name TestVNet \
    --resource-group TestRG \
    --location centralus \
    --address-prefix 192.168.0.0/16 \
    --subnet-name FrontEnd \
    --subnet-prefix 192.168.1.0/24
    ```

    <span data-ttu-id="3762a-124">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="3762a-124">Expected output:</span></span>
    
    ```json
    {
        "newVNet": {
            "addressSpace": {
            "addressPrefixes": [
            "192.168.0.0/16"
            ]
            },
            "dhcpOptions": {
            "dnsServers": []
            },
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>",
            "subnets": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                "name": "FrontEnd",
                "properties": {
                "addressPrefix": "192.168.1.0/24",
                "provisioningState": "Succeeded"
                },
                "resourceGroup": "TestRG"
            }
            ]
            }
    }
    ```

    <span data-ttu-id="3762a-125">Parametrar som används:</span><span class="sxs-lookup"><span data-stu-id="3762a-125">Parameters used:</span></span>

    - <span data-ttu-id="3762a-126">`--name TestVNet`: Namnet på hello VNet toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="3762a-126">`--name TestVNet`: Name of hello VNet toobe created.</span></span>
    - <span data-ttu-id="3762a-127">`--resource-group TestRG`: # hello resursgruppens namn som styr hello resurs.</span><span class="sxs-lookup"><span data-stu-id="3762a-127">`--resource-group TestRG`: # hello resource group name that controls hello resource.</span></span> 
    - <span data-ttu-id="3762a-128">`--location centralus`: hello plats till vilken toodeploy.</span><span class="sxs-lookup"><span data-stu-id="3762a-128">`--location centralus`: hello location into which toodeploy.</span></span>
    - <span data-ttu-id="3762a-129">`--address-prefix 192.168.0.0/16`: hello adressprefixet och blockera.</span><span class="sxs-lookup"><span data-stu-id="3762a-129">`--address-prefix 192.168.0.0/16`: hello address prefix and block.</span></span>  
    - <span data-ttu-id="3762a-130">`--subnet-name FrontEnd`: hello namnet på hello undernät.</span><span class="sxs-lookup"><span data-stu-id="3762a-130">`--subnet-name FrontEnd`: hello name of hello subnet.</span></span>
    - <span data-ttu-id="3762a-131">`--subnet-prefix 192.168.1.0/24`: hello adressprefixet och blockera.</span><span class="sxs-lookup"><span data-stu-id="3762a-131">`--subnet-prefix 192.168.1.0/24`: hello address prefix and block.</span></span>

    <span data-ttu-id="3762a-132">toolist hello grundläggande information toouse i hello nästa kommando, du kan fråga hello VNet med hjälp av en [frågefilter](/cli/azure/query-az-cli2):</span><span class="sxs-lookup"><span data-stu-id="3762a-132">toolist hello basic information toouse in hello next command, you can query hello VNet using a [query filter](/cli/azure/query-az-cli2):</span></span>

    ```azurecli
    az network vnet list --query '[?name==`TestVNet`].{Where:location,Name:name,Group:resourceGroup}' -o table
    ```

    <span data-ttu-id="3762a-133">Som ger hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="3762a-133">Which produces hello following output:</span></span>

        Where      Name      Group

        centralus  TestVNet  TestRG

4. <span data-ttu-id="3762a-134">Skapa ett undernät:</span><span class="sxs-lookup"><span data-stu-id="3762a-134">Create a subnet:</span></span>

    ```azurecli
    az network vnet subnet create \
    --address-prefix 192.168.2.0/24 \
    --name BackEnd \
    --resource-group TestRG \
    --vnet-name TestVNet
    ```

    <span data-ttu-id="3762a-135">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="3762a-135">Expected output:</span></span>

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid> \"",
    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "TestRG",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```

    <span data-ttu-id="3762a-136">Parametrar som används:</span><span class="sxs-lookup"><span data-stu-id="3762a-136">Parameters used:</span></span>

    - <span data-ttu-id="3762a-137">`--address-prefix 192.168.2.0/24`: Undernät CIDR-block.</span><span class="sxs-lookup"><span data-stu-id="3762a-137">`--address-prefix 192.168.2.0/24`: Subnet CIDR block.</span></span>
    - <span data-ttu-id="3762a-138">`--name BackEnd`: Namnet på hello Nytt undernät.</span><span class="sxs-lookup"><span data-stu-id="3762a-138">`--name BackEnd`: Name of hello new subnet.</span></span>
    - <span data-ttu-id="3762a-139">`--resource-group TestRG`: hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="3762a-139">`--resource-group TestRG`: hello resource group.</span></span>
    - <span data-ttu-id="3762a-140">`--vnet-name TestVNet`: hello namnet på hello äger VNet.</span><span class="sxs-lookup"><span data-stu-id="3762a-140">`--vnet-name TestVNet`: hello name of hello owning VNet.</span></span>

5. <span data-ttu-id="3762a-141">Frågan hello egenskaper för hello nya VNet:</span><span class="sxs-lookup"><span data-stu-id="3762a-141">Query hello properties of hello new VNet:</span></span>

    ```azurecli
    az network vnet show \
    -g TestRG \
    -n TestVNet \
    --query '{Name:name,Where:location,Group:resourceGroup,Status:provisioningState,SubnetCount:subnets | length(@)}' \
    -o table
    ```

    <span data-ttu-id="3762a-142">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="3762a-142">Expected output:</span></span>

        Name      Where      Group    Status       SubnetCount

        TestVNet  centralus  TestRG   Succeeded              2

6. <span data-ttu-id="3762a-143">Fråga hello egenskaper för hello undernät:</span><span class="sxs-lookup"><span data-stu-id="3762a-143">Query hello properties of hello subnets:</span></span>

    ```azurecli
    az network vnet subnet list \
    -g TestRG \
    --vnet-name testvnet \
    --query '[].{Name:name,CIDR:addressPrefix,Status:provisioningState}' \
    -o table
    ```

    <span data-ttu-id="3762a-144">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="3762a-144">Expected output:</span></span>

        Name      CIDR            Status

        FrontEnd  192.168.1.0/24  Succeeded
        BackEnd   192.168.2.0/24  Succeeded

## <a name="next-steps"></a><span data-ttu-id="3762a-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3762a-145">Next steps</span></span>

<span data-ttu-id="3762a-146">Lär dig hur tooconnect:</span><span class="sxs-lookup"><span data-stu-id="3762a-146">Learn how tooconnect:</span></span>

- <span data-ttu-id="3762a-147">En virtuell dator (VM) tooa virtuellt nätverk genom att läsa hello [skapa ett Linux VM](../virtual-machines/linux/quick-create-cli.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3762a-147">A virtual machine (VM) tooa virtual network by reading hello [Create a Linux VM](../virtual-machines/linux/quick-create-cli.md) article.</span></span> <span data-ttu-id="3762a-148">I stället för att skapa en VNet och undernät i hello steg hello artiklar, kan du välja ett befintligt VNet och undernät tooconnect en virtuell dator till.</span><span class="sxs-lookup"><span data-stu-id="3762a-148">Instead of creating a VNet and subnet in hello steps of hello articles, you can select an existing VNet and subnet tooconnect a VM to.</span></span>
- <span data-ttu-id="3762a-149">Hej virtuellt nätverk tooother virtuella nätverk genom att läsa hello [ansluta Vnet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3762a-149">hello virtual network tooother virtual networks by reading hello [Connect VNets](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) article.</span></span>
- <span data-ttu-id="3762a-150">hello virtuellt nätverk tooan lokalt nätverk via ett plats-till-plats virtuellt privat nätverk (VPN) eller en ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="3762a-150">hello virtual network tooan on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="3762a-151">Lär dig hur genom att läsa hello [ansluta ett virtuellt nätverk tooan lokalt nätverk med hjälp av en plats-till-plats-VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) och [länka VNet-tooan ExpressRoute-krets](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="3762a-151">Learn how by reading hello [Connect a VNet tooan on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet tooan ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span></span>
