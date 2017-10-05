---
title: Kontrollera Routning och virtuella installationer i Azure - PowerShell | Microsoft Docs
description: "Lär dig hur du styr Routning och virtuella installationer med hjälp av PowerShell."
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
ms.openlocfilehash: 3ab24f193c74449ae7414b4ea0675c0aae0211f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-user-defined-routes-udr-using-powershell"></a><span data-ttu-id="b8a65-103">Skapa användardefinierade vägar (UDR) med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="b8a65-103">Create User-Defined Routes (UDR) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b8a65-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b8a65-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="b8a65-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b8a65-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="b8a65-106">Mall</span><span class="sxs-lookup"><span data-stu-id="b8a65-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="b8a65-107">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="b8a65-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="b8a65-108">CLI (klassisk)</span><span class="sxs-lookup"><span data-stu-id="b8a65-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="b8a65-109">Innan du börjar arbeta med Azure-resurser är det viktigt att du vet att Azure för närvarande har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="b8a65-109">Before you work with Azure resources, it's important to understand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="b8a65-110">Se till att du förstår [distributionsmodeller och verktyg](../azure-resource-manager/resource-manager-deployment-model.md) innan du börjar arbeta med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="b8a65-110">Make sure you understand [deployment models and tools](../azure-resource-manager/resource-manager-deployment-model.md) before you work with any Azure resource.</span></span> <span data-ttu-id="b8a65-111">Du kan granska dokumentationen för olika verktyg genom att klicka på flikarna överst i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="b8a65-111">You can view the documentation for different tools by clicking the tabs at the top of this article.</span></span>
>

<span data-ttu-id="b8a65-112">Den här artikeln beskriver Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="b8a65-112">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="b8a65-113">Du kan också [skapa udr: er i den klassiska distributionsmodellen](virtual-network-create-udr-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b8a65-113">You can also [create UDRs in the classic deployment model](virtual-network-create-udr-classic-ps.md).</span></span>

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="b8a65-114">Exempel PowerShell-kommandona nedan förväntar sig en enkel miljö som redan har skapats baserat på scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="b8a65-114">The sample PowerShell commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="b8a65-115">Om du vill köra kommandon som de visas i det här dokumentet, först skapa testmiljön genom att distribuera [mallen](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klickar du på **till Azure**, ersätta Standardparametervärden om det behövs och följ instruktionerna i portalen.</span><span class="sxs-lookup"><span data-stu-id="b8a65-115">If you want to run the commands as they are displayed in this document, first build the test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a><span data-ttu-id="b8a65-116">Skapa UDR för undernätet frontend</span><span class="sxs-lookup"><span data-stu-id="b8a65-116">Create the UDR for the front-end subnet</span></span>
<span data-ttu-id="b8a65-117">Om du vill skapa routningstabellen och väg som behövs för frontend undernätet baserat på scenariot ovan, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="b8a65-117">To create the route table and route needed for the front-end subnet based on the scenario above, complete the following steps:</span></span>

1. <span data-ttu-id="b8a65-118">Skapa en väg som används för att skicka all trafik till backend-undernät (192.168.2.0/24) ska vidarebefordras till den **FW1** virtuell installation (192.168.0.4).</span><span class="sxs-lookup"><span data-stu-id="b8a65-118">Create a route used to send all traffic destined to the back-end subnet (192.168.2.0/24) to be routed to the **FW1** virtual appliance (192.168.0.4).</span></span>

    ```powershell
    $route = New-AzureRmRouteConfig -Name RouteToBackEnd `
    -AddressPrefix 192.168.2.0/24 -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

2. <span data-ttu-id="b8a65-119">Skapa en routingtabell som heter **UDR klientdel** i den **westus** region med vägen.</span><span class="sxs-lookup"><span data-stu-id="b8a65-119">Create a route table named **UDR-FrontEnd** in the **westus** region that contains the route.</span></span>

    ```powershell
    $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
    -Name UDR-FrontEnd -Route $route
    ```

3. <span data-ttu-id="b8a65-120">Skapa en variabel som innehåller VNet där undernätet är.</span><span class="sxs-lookup"><span data-stu-id="b8a65-120">Create a variable that contains the VNet where the subnet is.</span></span> <span data-ttu-id="b8a65-121">I vårt scenario, VNet heter **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="b8a65-121">In our scenario, the VNet is named **TestVNet**.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

4. <span data-ttu-id="b8a65-122">Associera routningstabellen skapade ovan till den **klientdel** undernät.</span><span class="sxs-lookup"><span data-stu-id="b8a65-122">Associate the route table created above to the **FrontEnd** subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
    -AddressPrefix 192.168.1.0/24 -RouteTable $routeTable
    ```

    > [!WARNING]
    > <span data-ttu-id="b8a65-123">Utdata för ovanstående kommando visar innehållet för konfigurationsobjektet virtuella nätverk som endast finns på datorn där du kör PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b8a65-123">The output for the command above shows the content for the virtual network configuration object, which only exists on the computer where you are running PowerShell.</span></span> <span data-ttu-id="b8a65-124">Du måste köra den **Set AzureVirtualNetwork** för att spara inställningarna till Azure.</span><span class="sxs-lookup"><span data-stu-id="b8a65-124">You need to run the **Set-AzureVirtualNetwork** cmdlet to save these settings to Azure.</span></span>
    > 

5. <span data-ttu-id="b8a65-125">Spara den nya konfigurationen av undernät i Azure.</span><span class="sxs-lookup"><span data-stu-id="b8a65-125">Save the new subnet configuration in Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="b8a65-126">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="b8a65-126">Expected output:</span></span>
   
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

## <a name="create-the-udr-for-the-back-end-subnet"></a><span data-ttu-id="b8a65-127">Skapa UDR för backend-undernät</span><span class="sxs-lookup"><span data-stu-id="b8a65-127">Create the UDR for the back-end subnet</span></span>

<span data-ttu-id="b8a65-128">Följ stegen nedan om du vill skapa routningstabellen och väg som behövs för backend-undernätet baserat på scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="b8a65-128">To create the route table and route needed for the back-end subnet based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="b8a65-129">Skapa en väg som används för att skicka all trafik till frontend-undernät (192.168.1.0/24) ska vidarebefordras till den **FW1** virtuell installation (192.168.0.4).</span><span class="sxs-lookup"><span data-stu-id="b8a65-129">Create a route used to send all traffic destined to the front-end subnet (192.168.1.0/24) to be routed to the **FW1** virtual appliance (192.168.0.4).</span></span>

    ```powershell
    $route = New-AzureRmRouteConfig -Name RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

2. <span data-ttu-id="b8a65-130">Skapa en routingtabell som heter **UDR BackEnd** i den **uswest** region med vägen skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="b8a65-130">Create a route table named **UDR-BackEnd** in the **uswest** region that contains the route created above.</span></span>

    ```
    $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
    -Name UDR-BackEnd -Route $route
    ```

3. <span data-ttu-id="b8a65-131">Associera routningstabellen skapade ovan till den **BackEnd** undernät.</span><span class="sxs-lookup"><span data-stu-id="b8a65-131">Associate the route table created above to the **BackEnd** subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd `
    -AddressPrefix 192.168.2.0/24 -RouteTable $routeTable
    ```

4. <span data-ttu-id="b8a65-132">Spara den nya konfigurationen av undernät i Azure.</span><span class="sxs-lookup"><span data-stu-id="b8a65-132">Save the new subnet configuration in Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="b8a65-133">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="b8a65-133">Expected output:</span></span>
   
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

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="b8a65-134">Aktivera IP-vidarebefordring på FW1</span><span class="sxs-lookup"><span data-stu-id="b8a65-134">Enable IP forwarding on FW1</span></span>
<span data-ttu-id="b8a65-135">Så här aktiverar du IP-vidarebefordring på det nätverkskort som används av **FW1**, Följ stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="b8a65-135">To enable IP forwarding in the NIC used by **FW1**, follow the steps below.</span></span>

1. <span data-ttu-id="b8a65-136">Skapa en variabel som innehåller inställningarna för det nätverkskort som används av FW1.</span><span class="sxs-lookup"><span data-stu-id="b8a65-136">Create a variable that contains the settings for the NIC used by FW1.</span></span> <span data-ttu-id="b8a65-137">I vårt scenario, NIC heter **NICFW1**.</span><span class="sxs-lookup"><span data-stu-id="b8a65-137">In our scenario, the NIC is named **NICFW1**.</span></span>

    ```powershell
    $nicfw1 = Get-AzureRmNetworkInterface -ResourceGroupName TestRG -Name NICFW1
    ```

2. <span data-ttu-id="b8a65-138">Aktivera IP-vidarebefordring och spara inställningarna för nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="b8a65-138">Enable IP forwarding, and save the NIC settings.</span></span>

    ```powershell
    $nicfw1.EnableIPForwarding = 1
    Set-AzureRmNetworkInterface -NetworkInterface $nicfw1
    ```
   
    <span data-ttu-id="b8a65-139">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="b8a65-139">Expected output:</span></span>
   
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

