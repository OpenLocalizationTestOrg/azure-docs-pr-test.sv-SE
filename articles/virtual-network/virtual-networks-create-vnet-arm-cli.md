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
# <a name="create-a-virtual-network-using-hello-azure-cli-20"></a>Skapa ett virtuellt nätverk med hello Azure CLI 2.0

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure har två distributionsmodeller: Azure Resource Manager och klassisk. Microsoft rekommenderar att skapa resurser via hello Resource Manager-modellen. Mer om toolearn hello skillnaderna mellan hello två modeller, läsa hello [förstå Azure distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md) artikel.

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md) – våra CLI för hello klassisk och resurs management distributionsmodeller
- [Azure CLI 2.0](#create-a-virtual-network) -vår nästa generations CLI för hello management resursdistributionsmodell (den här artikeln) ”
 
    Du kan också skapa ett VNet via Resource Manager med andra verktyg eller skapa ett VNet via hello klassiska distributionsmodellen genom att välja ett annat alternativ hello följande lista:

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

ett virtuellt nätverk som använder toocreate hello Azure CLI 2.0, fullständig hello följande steg:

1. Installera och konfigurera hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

2. Skapa en resursgrupp för ditt VNet med hjälp av hello [az gruppen skapa](/cli/azure/group#create) med hello `--name` och `--location` argument:

    ```azurecli
    az group create --name TestRG --location centralus
    ```

3. Skapa ett VNet och ett undernät:

    ```azurecli
    az network vnet create \
    --name TestVNet \
    --resource-group TestRG \
    --location centralus \
    --address-prefix 192.168.0.0/16 \
    --subnet-name FrontEnd \
    --subnet-prefix 192.168.1.0/24
    ```

    Förväntad utdata:
    
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

    Parametrar som används:

    - `--name TestVNet`: Namnet på hello VNet toobe skapas.
    - `--resource-group TestRG`: # hello resursgruppens namn som styr hello resurs. 
    - `--location centralus`: hello plats till vilken toodeploy.
    - `--address-prefix 192.168.0.0/16`: hello adressprefixet och blockera.  
    - `--subnet-name FrontEnd`: hello namnet på hello undernät.
    - `--subnet-prefix 192.168.1.0/24`: hello adressprefixet och blockera.

    toolist hello grundläggande information toouse i hello nästa kommando, du kan fråga hello VNet med hjälp av en [frågefilter](/cli/azure/query-az-cli2):

    ```azurecli
    az network vnet list --query '[?name==`TestVNet`].{Where:location,Name:name,Group:resourceGroup}' -o table
    ```

    Som ger hello följande utdata:

        Where      Name      Group

        centralus  TestVNet  TestRG

4. Skapa ett undernät:

    ```azurecli
    az network vnet subnet create \
    --address-prefix 192.168.2.0/24 \
    --name BackEnd \
    --resource-group TestRG \
    --vnet-name TestVNet
    ```

    Förväntad utdata:

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

    Parametrar som används:

    - `--address-prefix 192.168.2.0/24`: Undernät CIDR-block.
    - `--name BackEnd`: Namnet på hello Nytt undernät.
    - `--resource-group TestRG`: hello resursgruppen.
    - `--vnet-name TestVNet`: hello namnet på hello äger VNet.

5. Frågan hello egenskaper för hello nya VNet:

    ```azurecli
    az network vnet show \
    -g TestRG \
    -n TestVNet \
    --query '{Name:name,Where:location,Group:resourceGroup,Status:provisioningState,SubnetCount:subnets | length(@)}' \
    -o table
    ```

    Förväntad utdata:

        Name      Where      Group    Status       SubnetCount

        TestVNet  centralus  TestRG   Succeeded              2

6. Fråga hello egenskaper för hello undernät:

    ```azurecli
    az network vnet subnet list \
    -g TestRG \
    --vnet-name testvnet \
    --query '[].{Name:name,CIDR:addressPrefix,Status:provisioningState}' \
    -o table
    ```

    Förväntad utdata:

        Name      CIDR            Status

        FrontEnd  192.168.1.0/24  Succeeded
        BackEnd   192.168.2.0/24  Succeeded

## <a name="next-steps"></a>Nästa steg

Lär dig hur tooconnect:

- En virtuell dator (VM) tooa virtuellt nätverk genom att läsa hello [skapa ett Linux VM](../virtual-machines/linux/quick-create-cli.md) artikel. I stället för att skapa en VNet och undernät i hello steg hello artiklar, kan du välja ett befintligt VNet och undernät tooconnect en virtuell dator till.
- Hej virtuellt nätverk tooother virtuella nätverk genom att läsa hello [ansluta Vnet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) artikel.
- hello virtuellt nätverk tooan lokalt nätverk via ett plats-till-plats virtuellt privat nätverk (VPN) eller en ExpressRoute-kretsen. Lär dig hur genom att läsa hello [ansluta ett virtuellt nätverk tooan lokalt nätverk med hjälp av en plats-till-plats-VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) och [länka VNet-tooan ExpressRoute-krets](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).
