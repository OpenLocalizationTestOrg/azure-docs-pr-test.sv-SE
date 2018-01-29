---
title: Kontrollera routning i ett Azure Virtual Network - PowerShell - klassisk | Microsoft Docs
description: "Lär dig att styra routning i Vnet med PowerShell | Klassisk"
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
ms.openlocfilehash: e9564d223cb85529f1fa97bc398d35c6debcedae
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/21/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Kontrollera Routning och använder virtuella installationer (klassisk) med hjälp av PowerShell

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Mall](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klassisk)](virtual-network-create-udr-classic-ps.md)
> * [CLI (klassisk)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> Innan du börjar arbeta med Azure-resurser är det viktigt att du vet att Azure för närvarande har två distributionsmodeller: Azure Resource Manager och klassisk. Se till att du förstår [distributionsmodeller och verktyg](../azure-resource-manager/resource-manager-deployment-model.md) innan du börjar arbeta med Azure-resurser. Du kan visa i dokumentationen för olika verktyg genom att välja ett alternativ överst i den här artikeln. Den här artikeln beskriver hur du gör om du använder den klassiska distributionsmodellen.
> 

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Exemplet Azure PowerShell-kommandona nedan förväntar sig en enkel miljö som redan har skapats baserat på scenariot ovan. Om du vill köra kommandon som de visas i det här dokumentet, skapa miljön visas i [skapa ett virtuellt nätverk (klassiska) med hjälp av PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Skapa UDR för undernätet frontend
Följ stegen nedan om du vill skapa routningstabellen och väg för undernätet frontend baserat på scenariot ovan.

1. Kör följande kommando för att skapa en routningstabellen för undernätet frontend:

    ```powershell
    New-AzureRouteTable -Name UDR-FrontEnd -Location uswest `
    -Label "Route table for front end subnet"
    ```

2. Kör följande kommando för att skapa en väg i routningstabellen att skicka all trafik till backend-undernät (192.168.2.0/24) till den **FW1** VM (192.168.0.4):

    ```powershell
    Get-AzureRouteTable UDR-FrontEnd `
    |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Kör följande kommando för att associera routningstabellen med den **klientdel** undernät:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName FrontEnd `
    -RouteTableName UDR-FrontEnd
    ```

## <a name="create-the-udr-for-the-back-end-subnet"></a>Skapa UDR för backend-undernät
Utför följande steg för att skapa routningstabellen och väg som behövs för backend-undernät baserat på scenariot:

1. Kör följande kommando för att skapa en routingtabell för backend-undernät:

    ```powershell
    New-AzureRouteTable -Name UDR-BackEnd `
    -Location uswest `
    -Label "Route table for back end subnet"
    ```

2. Kör följande kommando för att skapa en väg i routningstabellen att skicka all trafik till frontend-undernät (192.168.1.0/24) till den **FW1** VM (192.168.0.4):

    ```powershell
    Get-AzureRouteTable UDR-BackEnd
    | Set-AzureRoute `
    -RouteName RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Kör följande kommando för att associera routningstabellen med den **BackEnd** undernät:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName BackEnd `
    -RouteTableName UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>Aktivera IP-vidarebefordran på den virtuella datorn FW1

Om du vill aktivera IP-vidarebefordring i FW1 VM, gör du följande:

1. Kör följande kommando för att kontrollera status för IP-vidarebefordran:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Get-AzureIPForwarding
    ```

2. Kör följande kommando för att aktivera IP-vidarebefordring för den *FW1* VM:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Set-AzureIPForwarding -Enable
    ```
