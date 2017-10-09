---
title: aaaControl Routning och virtuella installationer i Azure - PowerShell | Microsoft Docs
description: "Lär dig hur toocontrol Routning och virtuella installationer med hjälp av PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 9582fdaa-249c-4c98-9618-8c30d496940f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.openlocfilehash: b7b8717529eb2cd8b1d28b8ab9c6e21159d14882
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-powershell"></a><span data-ttu-id="d03a3-103">Skapa användardefinierade vägar (UDR) med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="d03a3-103">Create User-Defined Routes (UDR) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d03a3-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d03a3-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="d03a3-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d03a3-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="d03a3-106">Mall</span><span class="sxs-lookup"><span data-stu-id="d03a3-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="d03a3-107">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="d03a3-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="d03a3-108">CLI (klassisk)</span><span class="sxs-lookup"><span data-stu-id="d03a3-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="d03a3-109">Innan du börjar arbeta med Azure-resurser, är det viktigt toounderstand att Azure har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="d03a3-109">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="d03a3-110">Se till att du förstår [distributionsmodeller och verktyg](../azure-resource-manager/resource-manager-deployment-model.md) innan du börjar arbeta med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="d03a3-110">Make sure you understand [deployment models and tools](../azure-resource-manager/resource-manager-deployment-model.md) before you work with any Azure resource.</span></span> <span data-ttu-id="d03a3-111">Du kan visa hello dokumentationen för olika verktyg genom att klicka på flikarna hello hello överst i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="d03a3-111">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span>
>

<span data-ttu-id="d03a3-112">Den här artikeln beskriver hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="d03a3-112">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="d03a3-113">Du kan också [skapa udr: er i hello klassiska distributionsmodellen](virtual-network-create-udr-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="d03a3-113">You can also [create UDRs in hello classic deployment model](virtual-network-create-udr-classic-ps.md).</span></span>

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="d03a3-114">hello exempel PowerShell-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="d03a3-114">hello sample PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="d03a3-115">Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö genom att distribuera [mallen](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klickar du på **distribuera tooAzure**, Ersätt hello Standardparametervärden Om nödvändigt och hello anvisningar i hello portal.</span><span class="sxs-lookup"><span data-stu-id="d03a3-115">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="d03a3-116">Skapa hello UDR för hello frontend undernät</span><span class="sxs-lookup"><span data-stu-id="d03a3-116">Create hello UDR for hello front-end subnet</span></span>
<span data-ttu-id="d03a3-117">routningstabellen för toocreate hello och väg som behövs för hello frontend undernät baserat på hello scenariot ovan fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d03a3-117">toocreate hello route table and route needed for hello front-end subnet based on hello scenario above, complete hello following steps:</span></span>

1. <span data-ttu-id="d03a3-118">Skapa en väg används toosend alla trafik toohello backend-undernät (192.168.2.0/24) toobe dirigeras toohello **FW1** virtuell installation (192.168.0.4).</span><span class="sxs-lookup"><span data-stu-id="d03a3-118">Create a route used toosend all traffic destined toohello back-end subnet (192.168.2.0/24) toobe routed toohello **FW1** virtual appliance (192.168.0.4).</span></span>

    ```powershell
    $route = New-AzureRmRouteConfig -Name RouteToBackEnd `
    -AddressPrefix 192.168.2.0/24 -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

2. <span data-ttu-id="d03a3-119">Skapa en routingtabell som heter **UDR klientdel** i hello **westus** region med hello vägen.</span><span class="sxs-lookup"><span data-stu-id="d03a3-119">Create a route table named **UDR-FrontEnd** in hello **westus** region that contains hello route.</span></span>

    ```powershell
    $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
    -Name UDR-FrontEnd -Route $route
    ```

3. <span data-ttu-id="d03a3-120">Skapa en variabel som innehåller hello VNet där undernätet hello är.</span><span class="sxs-lookup"><span data-stu-id="d03a3-120">Create a variable that contains hello VNet where hello subnet is.</span></span> <span data-ttu-id="d03a3-121">I vårt scenario, hello VNet heter **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="d03a3-121">In our scenario, hello VNet is named **TestVNet**.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

4. <span data-ttu-id="d03a3-122">Associera hello routningstabellen skapade ovan toohello **klientdel** undernät.</span><span class="sxs-lookup"><span data-stu-id="d03a3-122">Associate hello route table created above toohello **FrontEnd** subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
    -AddressPrefix 192.168.1.0/24 -RouteTable $routeTable
    ```

    > [!WARNING]
    > <span data-ttu-id="d03a3-123">hello utdata för hello ovanstående kommando visar hello innehållet för hello virtuellt nätverk konfigurationsobjekt, som endast finns på hello datorn där du kör PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d03a3-123">hello output for hello command above shows hello content for hello virtual network configuration object, which only exists on hello computer where you are running PowerShell.</span></span> <span data-ttu-id="d03a3-124">Du behöver toorun hello **Set AzureVirtualNetwork** cmdlet toosave tooAzure dessa inställningar.</span><span class="sxs-lookup"><span data-stu-id="d03a3-124">You need toorun hello **Set-AzureVirtualNetwork** cmdlet toosave these settings tooAzure.</span></span>
    > 

5. <span data-ttu-id="d03a3-125">Spara hello ny konfiguration av undernät i Azure.</span><span class="sxs-lookup"><span data-stu-id="d03a3-125">Save hello new subnet configuration in Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="d03a3-126">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="d03a3-126">Expected output:</span></span>
   
        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : westus
        Id                : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              : 
                            Name         Value
                            ===========  =====
                            displayName  VNet 
   
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                                ...,
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [
                                  {
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConfigurations/ipconfig1"
                                  },
                                  {
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                                  }
                                ],
                                "NetworkSecurityGroup": {
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                },
                                "RouteTable": {
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                                },
                                "ProvisioningState": "Succeeded"
                              },
                                ...
                            ]    

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="d03a3-127">Skapa hello UDR för hello backend-undernät</span><span class="sxs-lookup"><span data-stu-id="d03a3-127">Create hello UDR for hello back-end subnet</span></span>

<span data-ttu-id="d03a3-128">routningstabellen för toocreate hello och väg som behövs för hello backend-undernät baserat på hello scenariot ovan, följer du hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="d03a3-128">toocreate hello route table and route needed for hello back-end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="d03a3-129">Skapa en väg används toosend alla trafik toohello frontend undernät (192.168.1.0/24) toobe dirigeras toohello **FW1** virtuell installation (192.168.0.4).</span><span class="sxs-lookup"><span data-stu-id="d03a3-129">Create a route used toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toobe routed toohello **FW1** virtual appliance (192.168.0.4).</span></span>

    ```powershell
    $route = New-AzureRmRouteConfig -Name RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

2. <span data-ttu-id="d03a3-130">Skapa en routingtabell som heter **UDR BackEnd** i hello **uswest** region med hello flödet skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="d03a3-130">Create a route table named **UDR-BackEnd** in hello **uswest** region that contains hello route created above.</span></span>

    ```
    $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
    -Name UDR-BackEnd -Route $route
    ```

3. <span data-ttu-id="d03a3-131">Associera hello routningstabellen skapade ovan toohello **BackEnd** undernät.</span><span class="sxs-lookup"><span data-stu-id="d03a3-131">Associate hello route table created above toohello **BackEnd** subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd `
    -AddressPrefix 192.168.2.0/24 -RouteTable $routeTable
    ```

4. <span data-ttu-id="d03a3-132">Spara hello ny konfiguration av undernät i Azure.</span><span class="sxs-lookup"><span data-stu-id="d03a3-132">Save hello new subnet configuration in Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="d03a3-133">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="d03a3-133">Expected output:</span></span>
   
        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : westus
        Id                : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              : 
                            Name         Value
                            ===========  =====
                            displayName  VNet 
   
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              ...,
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [
                                  {
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL2/ipConfigurations/ipconfig1"
                                  },
                                  {
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL1/ipConfigurations/ipconfig1"
                                  }
                                ],
                                "NetworkSecurityGroup": {
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BacEnd"
                                },
                                "RouteTable": {
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd"
                                },
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="d03a3-134">Aktivera IP-vidarebefordring på FW1</span><span class="sxs-lookup"><span data-stu-id="d03a3-134">Enable IP forwarding on FW1</span></span>
<span data-ttu-id="d03a3-135">tooenable IP-vidarebefordran i hello NIC som används av **FW1**, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="d03a3-135">tooenable IP forwarding in hello NIC used by **FW1**, follow hello steps below.</span></span>

1. <span data-ttu-id="d03a3-136">Skapa en variabel som innehåller hello inställningar för hello NIC som används av FW1.</span><span class="sxs-lookup"><span data-stu-id="d03a3-136">Create a variable that contains hello settings for hello NIC used by FW1.</span></span> <span data-ttu-id="d03a3-137">I vårt scenario, hello NIC heter **NICFW1**.</span><span class="sxs-lookup"><span data-stu-id="d03a3-137">In our scenario, hello NIC is named **NICFW1**.</span></span>

    ```powershell
    $nicfw1 = Get-AzureRmNetworkInterface -ResourceGroupName TestRG -Name NICFW1
    ```

2. <span data-ttu-id="d03a3-138">Aktivera IP-vidarebefordring och spara hello inställningarna för nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="d03a3-138">Enable IP forwarding, and save hello NIC settings.</span></span>

    ```powershell
    $nicfw1.EnableIPForwarding = 1
    Set-AzureRmNetworkInterface -NetworkInterface $nicfw1
    ```
   
    <span data-ttu-id="d03a3-139">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="d03a3-139">Expected output:</span></span>
   
        Name                 : NICFW1
        ResourceGroupName    : TestRG
        Location             : westus
        Id                   : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1
        Etag                 : W/"[Id]"
        ProvisioningState    : Succeeded
        Tags                 : 
                               Name         Value                  
                               ===========  =======================
                               displayName  NetworkInterfaces - DMZ
   
        VirtualMachine       : {
                                 "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/FW1"
                               }
        IpConfigurations     : [
                                 {
                                   "Name": "ipconfig1",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1/ipConfigurations/ipconfig1",
                                   "PrivateIpAddress": "192.168.0.4",
                                   "PrivateIpAllocationMethod": "Static",
                                   "Subnet": {
                                     "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/DMZ"
                                   },
                                   "PublicIpAddress": {
                                     "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPFW1"
                                   },
                                   "LoadBalancerBackendAddressPools": [],
                                   "LoadBalancerInboundNatRules": [],
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]
        DnsSettings          : {
                                 "DnsServers": [],
                                 "AppliedDnsServers": [],
                                 "InternalDnsNameLabel": null,
                                 "InternalFqdn": null
                               }
        EnableIPForwarding   : True
        NetworkSecurityGroup : null
        Primary              : True

