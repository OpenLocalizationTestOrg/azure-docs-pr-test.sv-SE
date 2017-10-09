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
# <a name="create-a-virtual-network-using-powershell"></a>Skapa ett virtuellt nätverk med PowerShell

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure har två distributionsmodeller: Azure Resource Manager och klassisk. Microsoft rekommenderar att skapa resurser via hello Resource Manager-modellen. Mer om toolearn hello skillnaderna mellan hello två modeller, läsa hello [förstå Azure distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md) artikel.
 
Den här artikeln förklarar hur toocreate ett VNet via hello Resource Manager distribution modellen med hjälp av PowerShell. Du kan också skapa ett VNet via Resource Manager med andra verktyg eller skapa ett VNet via hello klassiska distributionsmodellen genom att välja ett annat alternativ hello följande lista:

> [!div class="op_single_selector"]
> * [Portal](virtual-networks-create-vnet-arm-pportal.md)
> * [PowerShell](virtual-networks-create-vnet-arm-ps.md)
> * [CLI](virtual-networks-create-vnet-arm-cli.md)
> * [Mall](virtual-networks-create-vnet-arm-template-click.md)
> * [Portal (klassisk)](virtual-networks-create-vnet-classic-pportal.md)
> * [PowerShell (klassisk)](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [CLI (klassisk)](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk

toocreate ett virtuellt nätverk med PowerShell fullständig hello följande steg:

1. Installera och konfigurera Azure PowerShell genom att följa stegen hello i hello [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.

2. Vid behov kan du skapa en ny resursgrupp som visas nedan. I det här scenariot skapar du en resursgrupp med namnet *TestRG*. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

    ```powershell   
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    Förväntad utdata:

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG    
3. Skapa ett nytt virtuellt nätverk med namnet *TestVNet*:

    ```powershell
    New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
    -AddressPrefix 192.168.0.0/16 -Location centralus
    ```

    Förväntad utdata:

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
4. Lagra hello virtuella nätverksobjektet i en variabel:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

   > [!TIP]
   > Du kan kombinera steg 3 och 4 genom att köra `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.
   > 

5. Lägg till ett undernät toohello nya VNet-variabeln:

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
    ```

    Förväntad utdata:
   
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

6. Upprepa steg 5 ovan för varje undernät som du vill toocreate. hello följande kommando skapar hello *BackEnd* undernät för hello scenariot:

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24
    ```

7. Även om du skapar undernät, finnas de för närvarande endast i hello lokala variabeln används tooretrieve hello VNet som du skapar i steg 4 ovan. toosave hello ändringar tooAzure, kör hello följande kommando:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```
   
    Förväntad utdata:
   
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

## <a name="next-steps"></a>Nästa steg

Lär dig hur tooconnect:

- En virtuell dator (VM) tooa virtuellt nätverk genom att läsa hello [skapa en virtuell Windows-dator](../virtual-machines/virtual-machines-windows-ps-create.md) artikel. I stället för att skapa en VNet och undernät i hello steg hello artiklar, kan du välja ett befintligt VNet och undernät tooconnect en virtuell dator till.
- Hej virtuellt nätverk tooother virtuella nätverk genom att läsa hello [ansluta Vnet](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) artikel.
- hello virtuellt nätverk tooan lokalt nätverk via ett plats-till-plats virtuellt privat nätverk (VPN) eller en ExpressRoute-kretsen. Lär dig hur genom att läsa hello [ansluta ett virtuellt nätverk tooan lokalt nätverk med hjälp av en plats-till-plats-VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) och [länka VNet-tooan ExpressRoute-krets](../expressroute/expressroute-howto-linkvnet-arm.md) artiklar.
