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
# <a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Kontrollera Routning och använder virtuella installationer (klassisk) med hjälp av PowerShell

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Mall](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klassisk)](virtual-network-create-udr-classic-ps.md)
> * [CLI (klassisk)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> Innan du börjar arbeta med Azure-resurser, är det viktigt toounderstand att Azure har två distributionsmodeller: Azure Resource Manager och klassisk. Se till att du förstår [distributionsmodeller och verktyg](../azure-resource-manager/resource-manager-deployment-model.md) innan du börjar arbeta med Azure-resurser. Du kan visa hello dokumentationen för olika verktyg genom att välja ett alternativ hello överst i den här artikeln. Den här artikeln beskriver hello klassiska distributionsmodellen.
> 

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

hello exempel Azure PowerShell-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan. Om du vill toorun hello kommandon som de visas i det här dokumentet, skapa hello miljö som visas i [skapa ett virtuellt nätverk (klassiska) med hjälp av PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-udr-for-hello-front-end-subnet"></a>Skapa hello UDR för hello klientdelen undernät
routningstabellen för toocreate hello och väg som behövs för hello klientdelen undernät baserat på hello scenariot ovan, följer du hello stegen nedan.

1. Kör följande kommando toocreate hello en routningstabell för hello frontend undernät:

    ```powershell
    New-AzureRouteTable -Name UDR-FrontEnd -Location uswest `
    -Label "Route table for front end subnet"
    ```

2. Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello backend-undernät (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):

    ```powershell
    Get-AzureRouteTable UDR-FrontEnd `
    |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Kör hello följande kommando tooassociate hello routningstabellen med hello **klientdel** undernät:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName FrontEnd `
    -RouteTableName UDR-FrontEnd
    ```

## <a name="create-hello-udr-for-hello-back-end-subnet"></a>Skapa hello UDR för hello backend-undernät
toocreate hello routningstabellen och flöde som behövs för hello tillbaka avslutas undernät baserat på hello scenario, fullständig hello följande steg:

1. Kör följande kommando toocreate hello en routningstabell för hello backend-undernät:

    ```powershell
    New-AzureRouteTable -Name UDR-BackEnd `
    -Location uswest `
    -Label "Route table for back end subnet"
    ```

2. Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello frontend undernät (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):

    ```powershell
    Get-AzureRouteTable UDR-BackEnd
    | Set-AzureRoute `
    -RouteName RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Kör hello följande kommando tooassociate hello routningstabellen med hello **BackEnd** undernät:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName BackEnd `
    -RouteTableName UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-hello-fw1-vm"></a>Aktivera IP-vidarebefordring på hello FW1 VM

tooenable IP-vidarebefordran i hello FW1 VM, fullständig hello följande steg:

1. Kör följande kommando toocheck hello status för IP-vidarebefordring hello:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Get-AzureIPForwarding
    ```

2. Hello kör följande kommando tooenable IP-vidarebefordring för hello *FW1* VM:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Set-AzureIPForwarding -Enable
    ```
