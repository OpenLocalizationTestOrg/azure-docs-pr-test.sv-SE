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
# <a name="create-a-virtual-network-using-hello-azure-cli"></a>Skapa ett virtuellt nätverk med hello Azure CLI

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure har två distributionsmodeller: Azure Resource Manager och klassisk. Microsoft rekommenderar att skapa resurser via hello Resource Manager-modellen. Mer om toolearn hello skillnaderna mellan hello två modeller, läsa hello [förstå Azure distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md) artikel.

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering
- [Azure CLI 1.0](#create-a-virtual-network) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)

 
[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk

ett virtuellt nätverk som använder toocreate hello Azure CLI, fullständig hello följande steg:

1. Installera och konfigurera hello Azure CLI med följande hello steg i hello [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) artikel.

2. Skapa ett VNet och ett undernät:

    ```azurecli
    azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    ```

    Förväntad utdata:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    Parametrar som används:

   * **--vnet**. Namnet på hello VNet toobe skapas. I vårt exempel, *TestVNet*
   * **-e (eller---adressutrymmet)**. VNet-adressutrymmet. I vårt scenario, *192.168.0.0*
   * **-i (eller - cidr)**. Nätverksmasken i CIDR-format. I vårt scenario, *16*.
   * **-n (eller--undernätsnamn**). Namnet på hello första undernätet. I vårt scenario, *FrontEnd*.
   * **-p (eller--undernät-start-ip)**. Första IP-adressen för undernätet eller undernätsadressutrymmet. I vårt scenario, *192.168.1.0*.
   * **-r (eller--undernät cidr)**. Nätverksmasken i CIDR-format för undernätet. I vårt scenario, *24*.
   * **-l (eller --location)**. Azure-region där hello VNet har skapats. I vårt scenario, *centrala USA*.

3. Skapa ett undernät:

    ```azurecli
    azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    ```
   
    Förväntad utdata:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up hello subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    Parametrar som används:

   * **-t (eller--vnet-name**. Namnet på hello VNet där undernätet hello kommer att skapas. I vårt scenario, *TestVNet*.
   * **-n (eller --name)**. Namnet på hello Nytt undernät. I vårt scenario, *BackEnd*.
   * **-a (eller --address-prefix)**. CIDR-block för undernätet. Fyra vårt scenario, *192.168.2.0/24*.
   
4. tooview hello egenskaper för hello nya VNet:

    ```azurecli
    azure network vnet show
    ```
   
    Förväntad utdata:
   
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

## <a name="next-steps"></a>Nästa steg

Lär dig hur tooconnect:

- En virtuell dator (VM) tooa virtuellt nätverk genom att läsa hello [skapa ett Linux VM](../virtual-machines/linux/quick-create-cli.md) artikel. I stället för att skapa en VNet och undernät i hello steg hello artiklar, kan du välja ett befintligt VNet och undernät tooconnect en virtuell dator till.
- Hej virtuellt nätverk tooother virtuella nätverk genom att läsa hello [ansluta Vnet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) artikel.
- hello virtuellt nätverk tooan lokalt nätverk via ett plats-till-plats virtuellt privat nätverk (VPN) eller en ExpressRoute-kretsen. Lär dig hur genom att läsa hello [ansluta ett virtuellt nätverk tooan lokalt nätverk med hjälp av en plats-till-plats-VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) och [länka VNet-tooan ExpressRoute-krets](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).
