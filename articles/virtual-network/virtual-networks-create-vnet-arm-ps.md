---
title: "aaaCreate ett virtuellt nätverk - Azure PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate ett virtuellt nätverk med hjälp av PowerShell."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a31f4f12-54ee-4339-b968-1a8097ca77d3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d6e395a77f71de9f94b6304b05450e46b47544f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-powershell"></a><span data-ttu-id="ce0e1-103">Skapa ett virtuellt nätverk med PowerShell</span><span class="sxs-lookup"><span data-stu-id="ce0e1-103">Create a virtual network using PowerShell</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="ce0e1-104">Azure har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="ce0e1-105">Microsoft rekommenderar att skapa resurser via hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="ce0e1-106">Mer om toolearn hello skillnaderna mellan hello två modeller, läsa hello [förstå Azure distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>
 
<span data-ttu-id="ce0e1-107">Den här artikeln förklarar hur toocreate ett VNet via hello Resource Manager distribution modellen med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-107">This article explains how toocreate a VNet through hello Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="ce0e1-108">Du kan också skapa ett VNet via Resource Manager med andra verktyg eller skapa ett VNet via hello klassiska distributionsmodellen genom att välja ett annat alternativ hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="ce0e1-108">You can also create a VNet through Resource Manager using other tools or create a VNet through hello classic deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ce0e1-109">Portal</span><span class="sxs-lookup"><span data-stu-id="ce0e1-109">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
> * [<span data-ttu-id="ce0e1-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ce0e1-110">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
> * [<span data-ttu-id="ce0e1-111">CLI</span><span class="sxs-lookup"><span data-stu-id="ce0e1-111">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
> * [<span data-ttu-id="ce0e1-112">Mall</span><span class="sxs-lookup"><span data-stu-id="ce0e1-112">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
> * [<span data-ttu-id="ce0e1-113">Portal (klassisk)</span><span class="sxs-lookup"><span data-stu-id="ce0e1-113">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
> * [<span data-ttu-id="ce0e1-114">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="ce0e1-114">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [<span data-ttu-id="ce0e1-115">CLI (klassisk)</span><span class="sxs-lookup"><span data-stu-id="ce0e1-115">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a><span data-ttu-id="ce0e1-116">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="ce0e1-116">Create a virtual network</span></span>

<span data-ttu-id="ce0e1-117">toocreate ett virtuellt nätverk med PowerShell fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ce0e1-117">toocreate a virtual network using PowerShell, complete hello following steps:</span></span>

1. <span data-ttu-id="ce0e1-118">Installera och konfigurera Azure PowerShell genom att följa stegen hello i hello [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-118">Install and configure Azure PowerShell, by following hello steps in hello [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>

2. <span data-ttu-id="ce0e1-119">Vid behov kan du skapa en ny resursgrupp som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-119">If necessary, create a new resource group, as shown below.</span></span> <span data-ttu-id="ce0e1-120">I det här scenariot skapar du en resursgrupp med namnet *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-120">For this scenario, create a resource group named *TestRG*.</span></span> <span data-ttu-id="ce0e1-121">Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ce0e1-121">For more information about resource groups, visit [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    ```powershell   
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    <span data-ttu-id="ce0e1-122">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="ce0e1-122">Expected output:</span></span>

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG    
3. <span data-ttu-id="ce0e1-123">Skapa ett nytt virtuellt nätverk med namnet *TestVNet*:</span><span class="sxs-lookup"><span data-stu-id="ce0e1-123">Create a new VNet named *TestVNet*:</span></span>

    ```powershell
    New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
    -AddressPrefix 192.168.0.0/16 -Location centralus
    ```

    <span data-ttu-id="ce0e1-124">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="ce0e1-124">Expected output:</span></span>

        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                   : W/"[Id]"
        ProvisioningState          : Succeeded
        Tags                       : 
        AddressSpace               : {
                                   "AddressPrefixes": [
                                     "192.168.0.0/16"
                                   ]
                                  }
        DhcpOptions                : {}
        Subnets                    : []
        VirtualNetworkPeerings     : []
4. <span data-ttu-id="ce0e1-125">Lagra hello virtuella nätverksobjektet i en variabel:</span><span class="sxs-lookup"><span data-stu-id="ce0e1-125">Store hello virtual network object in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

   > [!TIP]
   > <span data-ttu-id="ce0e1-126">Du kan kombinera steg 3 och 4 genom att köra `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-126">You can combine steps 3 and 4 by running `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.</span></span>
   > 

5. <span data-ttu-id="ce0e1-127">Lägg till ett undernät toohello nya VNet-variabeln:</span><span class="sxs-lookup"><span data-stu-id="ce0e1-127">Add a subnet toohello new VNet variable:</span></span>

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
    ```

    <span data-ttu-id="ce0e1-128">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="ce0e1-128">Expected output:</span></span>
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"[Id]"
        ProvisioningState     : Succeeded
        Tags                  :
        AddressSpace          : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions           : {}
        Subnets             : [
                                  {
                                    "Name": "FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24"
                                  }
                                ]
        VirtualNetworkPeerings     : []

6. <span data-ttu-id="ce0e1-129">Upprepa steg 5 ovan för varje undernät som du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-129">Repeat step 5 above for each subnet you want toocreate.</span></span> <span data-ttu-id="ce0e1-130">hello följande kommando skapar hello *BackEnd* undernät för hello scenariot:</span><span class="sxs-lookup"><span data-stu-id="ce0e1-130">hello following command creates hello *BackEnd* subnet for hello scenario:</span></span>

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24
    ```

7. <span data-ttu-id="ce0e1-131">Även om du skapar undernät, finnas de för närvarande endast i hello lokala variabeln används tooretrieve hello VNet som du skapar i steg 4 ovan.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-131">Although you create subnets, they currently only exist in hello local variable used tooretrieve hello VNet you create in step 4 above.</span></span> <span data-ttu-id="ce0e1-132">toosave hello ändringar tooAzure, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ce0e1-132">toosave hello changes tooAzure, run hello following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```
   
    <span data-ttu-id="ce0e1-133">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="ce0e1-133">Expected output:</span></span>
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"[Id]"
        ProvisioningState     : Succeeded
        Tags                  :
        AddressSpace          : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions           : {
                                  "DnsServers": []
                                }
        Subnets               : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  },
                                  {
                                    "Name": "BackEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                    "AddressPrefix": "192.168.2.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  }
                                ]
        VirtualNetworkPeerings : []

## <a name="next-steps"></a><span data-ttu-id="ce0e1-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ce0e1-134">Next steps</span></span>

<span data-ttu-id="ce0e1-135">Lär dig hur tooconnect:</span><span class="sxs-lookup"><span data-stu-id="ce0e1-135">Learn how tooconnect:</span></span>

- <span data-ttu-id="ce0e1-136">En virtuell dator (VM) tooa virtuellt nätverk genom att läsa hello [skapa en virtuell Windows-dator](../virtual-machines/virtual-machines-windows-ps-create.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-136">A virtual machine (VM) tooa virtual network by reading hello [Create a Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md) article.</span></span> <span data-ttu-id="ce0e1-137">I stället för att skapa en VNet och undernät i hello steg hello artiklar, kan du välja ett befintligt VNet och undernät tooconnect en virtuell dator till.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-137">Instead of creating a VNet and subnet in hello steps of hello articles, you can select an existing VNet and subnet tooconnect a VM to.</span></span>
- <span data-ttu-id="ce0e1-138">Hej virtuellt nätverk tooother virtuella nätverk genom att läsa hello [ansluta Vnet](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-138">hello virtual network tooother virtual networks by reading hello [Connect VNets](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) article.</span></span>
- <span data-ttu-id="ce0e1-139">hello virtuellt nätverk tooan lokalt nätverk via ett plats-till-plats virtuellt privat nätverk (VPN) eller en ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-139">hello virtual network tooan on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="ce0e1-140">Lär dig hur genom att läsa hello [ansluta ett virtuellt nätverk tooan lokalt nätverk med hjälp av en plats-till-plats-VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) och [länka VNet-tooan ExpressRoute-krets](../expressroute/expressroute-howto-linkvnet-arm.md) artiklar.</span><span class="sxs-lookup"><span data-stu-id="ce0e1-140">Learn how by reading hello [Connect a VNet tooan on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet tooan ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-arm.md) articles.</span></span>
