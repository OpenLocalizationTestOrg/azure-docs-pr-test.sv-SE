---
title: aaaControl routning i ett Azure Virtual Network - PowerShell - klassisk | Microsoft Docs
description: "Lär dig hur toocontrol routning i Vnet med PowerShell | Klassisk"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: d8d07c16-cbe5-4536-acd6-870269346fe3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 36edf263fb434d5fb13310d4324da20e57f016a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a><span data-ttu-id="e25a5-103">Kontrollera Routning och använder virtuella installationer (klassisk) med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="e25a5-103">Control routing and use virtual appliances (classic) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e25a5-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e25a5-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="e25a5-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e25a5-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="e25a5-106">Mall</span><span class="sxs-lookup"><span data-stu-id="e25a5-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="e25a5-107">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="e25a5-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="e25a5-108">CLI (klassisk)</span><span class="sxs-lookup"><span data-stu-id="e25a5-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="e25a5-109">Innan du börjar arbeta med Azure-resurser, är det viktigt toounderstand att Azure har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="e25a5-109">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="e25a5-110">Se till att du förstår [distributionsmodeller och verktyg](../azure-resource-manager/resource-manager-deployment-model.md) innan du börjar arbeta med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="e25a5-110">Make sure you understand [deployment models and tools](../azure-resource-manager/resource-manager-deployment-model.md) before you work with any Azure resource.</span></span> <span data-ttu-id="e25a5-111">Du kan visa hello dokumentationen för olika verktyg genom att välja ett alternativ hello överst i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e25a5-111">You can view hello documentation for different tools by selecting an option at hello top of this article.</span></span> <span data-ttu-id="e25a5-112">Den här artikeln beskriver hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="e25a5-112">This article covers hello classic deployment model.</span></span>
> 

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="e25a5-113">hello exempel Azure PowerShell-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="e25a5-113">hello sample Azure PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="e25a5-114">Om du vill toorun hello kommandon som de visas i det här dokumentet, skapa hello miljö som visas i [skapa ett virtuellt nätverk (klassiska) med hjälp av PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).</span><span class="sxs-lookup"><span data-stu-id="e25a5-114">If you want toorun hello commands as they are displayed in this document, create hello environment shown in [create a VNet (classic) using PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="e25a5-115">Skapa hello UDR för hello klientdelen undernät</span><span class="sxs-lookup"><span data-stu-id="e25a5-115">Create hello UDR for hello front end subnet</span></span>
<span data-ttu-id="e25a5-116">routningstabellen för toocreate hello och väg som behövs för hello klientdelen undernät baserat på hello scenariot ovan, följer du hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="e25a5-116">toocreate hello route table and route needed for hello front end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="e25a5-117">Kör följande kommando toocreate hello en routningstabell för hello frontend undernät:</span><span class="sxs-lookup"><span data-stu-id="e25a5-117">Run hello following command toocreate a route table for hello front-end subnet:</span></span>

    ```powershell
    New-AzureRouteTable -Name UDR-FrontEnd -Location uswest `
    -Label "Route table for front end subnet"
    ```

2. <span data-ttu-id="e25a5-118">Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello backend-undernät (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="e25a5-118">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello back-end subnet (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```powershell
    Get-AzureRouteTable UDR-FrontEnd `
    |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. <span data-ttu-id="e25a5-119">Kör hello följande kommando tooassociate hello routningstabellen med hello **klientdel** undernät:</span><span class="sxs-lookup"><span data-stu-id="e25a5-119">Run hello following command tooassociate hello route table with hello **FrontEnd** subnet:</span></span>

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName FrontEnd `
    -RouteTableName UDR-FrontEnd
    ```

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="e25a5-120">Skapa hello UDR för hello backend-undernät</span><span class="sxs-lookup"><span data-stu-id="e25a5-120">Create hello UDR for hello back-end subnet</span></span>
<span data-ttu-id="e25a5-121">toocreate hello routningstabellen och flöde som behövs för hello tillbaka avslutas undernät baserat på hello scenario, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e25a5-121">toocreate hello route table and route needed for hello back end subnet based on hello scenario, complete hello following steps:</span></span>

1. <span data-ttu-id="e25a5-122">Kör följande kommando toocreate hello en routningstabell för hello backend-undernät:</span><span class="sxs-lookup"><span data-stu-id="e25a5-122">Run hello following command toocreate a route table for hello back-end subnet:</span></span>

    ```powershell
    New-AzureRouteTable -Name UDR-BackEnd `
    -Location uswest `
    -Label "Route table for back end subnet"
    ```

2. <span data-ttu-id="e25a5-123">Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello frontend undernät (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="e25a5-123">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```powershell
    Get-AzureRouteTable UDR-BackEnd
    | Set-AzureRoute `
    -RouteName RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. <span data-ttu-id="e25a5-124">Kör hello följande kommando tooassociate hello routningstabellen med hello **BackEnd** undernät:</span><span class="sxs-lookup"><span data-stu-id="e25a5-124">Run hello following command tooassociate hello route table with hello **BackEnd** subnet:</span></span>

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName BackEnd `
    -RouteTableName UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-hello-fw1-vm"></a><span data-ttu-id="e25a5-125">Aktivera IP-vidarebefordring på hello FW1 VM</span><span class="sxs-lookup"><span data-stu-id="e25a5-125">Enable IP forwarding on hello FW1 VM</span></span>

<span data-ttu-id="e25a5-126">tooenable IP-vidarebefordran i hello FW1 VM, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e25a5-126">tooenable IP forwarding in hello FW1 VM, complete hello following steps:</span></span>

1. <span data-ttu-id="e25a5-127">Kör följande kommando toocheck hello status för IP-vidarebefordring hello:</span><span class="sxs-lookup"><span data-stu-id="e25a5-127">Run hello following command toocheck hello status of IP forwarding:</span></span>

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Get-AzureIPForwarding
    ```

2. <span data-ttu-id="e25a5-128">Hello kör följande kommando tooenable IP-vidarebefordring för hello *FW1* VM:</span><span class="sxs-lookup"><span data-stu-id="e25a5-128">Run hello following command tooenable IP forwarding for hello *FW1* VM:</span></span>

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Set-AzureIPForwarding -Enable
    ```
