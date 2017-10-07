---
title: "aaaControl Routning och virtuella installationer med hjälp av hello Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur toocontrol Routning och virtuella installationer med hjälp av hello Azure CLI 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5452a0b8-21a6-4699-8d6a-e2d8faf32c25
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/12/2017
ms.author: jdial
ms.openlocfilehash: 79b908848932a4365dab1b7497b6a0dbf44bbaf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-hello-azure-cli-20"></a>Skapa användardefinierade vägar (UDR) med hjälp av hello Azure CLI 2.0

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Mall](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klassisk distribution)](virtual-network-create-udr-classic-ps.md)
> * [CLI (klassisk distribution)](virtual-network-create-udr-classic-cli.md)

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet 

Du kan göra hello med hjälp av något av följande versioner av CLI hello: 

- [Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – våra CLI för hello klassisk och resurs management distributionsmodeller 
- [Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) -vår nästa generations CLI för hello management resursdistributionsmodell (den här artikeln)

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

hello exempel Azure CLI-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan. Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö genom att distribuera [mallen](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klickar du på **distribuera tooAzure**, Ersätt hello Standardparametervärden Om nödvändigt och hello anvisningar i hello portal.


## <a name="create-hello-udr-for-hello-front-end-subnet"></a>Skapa hello UDR för hello frontend undernät
routningstabellen för toocreate hello och väg som behövs för hello klientdelen undernät baserat på hello scenariot ovan, följer du hello stegen nedan.

1. Skapa en routingtabell för hello frontend undernät med hello [az nätverket routningstabellen skapa](/cli/azure/network/route-table#create) kommando:

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --location centralus \
    --name UDR-FrontEnd
    ```

    Resultat:

    ```json
    {
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
    "location": "centralus",
    "name": "UDR-FrontEnd",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "routes": [],
    "subnets": null,
    "tags": null,
    "type": "Microsoft.Network/routeTables"
    }
    ```

2. Skapa en väg som skickar all trafik toohello backend-undernät (192.168.2.0/24) toohello **FW1** VM (192.168.0.4) med hjälp av hello [az nätverksväg routningstabellen skapa](/cli/azure/network/route-table/route#create) kommando:

    ```azurecli 
    az network route-table route create \
    --resource-group testrg \
    --name RouteToBackEnd \
    --route-table-name UDR-FrontEnd \
    --address-prefix 192.168.2.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

    Resultat:

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd/routes/RouteToBackEnd",
    "name": "RouteToBackEnd",
    "nextHopIpAddress": "192.168.0.4",
    "nextHopType": "VirtualAppliance",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg"
    }
    ```
    Parametrar:

    * **--routningstabellnamnet-**. Namnet på hello routningstabellen där hello flödet kommer att läggas till. I vårt scenario, *UDR klientdel*.
    * **--address-prefix**. Adressprefixet för hello undernät där paket som är avsedda att. I vårt scenario, *192.168.2.0/24*.
    * **--Nästa hopptyp**. Typ av objekt trafik skickas till. Möjliga värden är *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, eller *ingen*.
    * **--Nästa-hopp-ip-adress**. IP-adressen för nästa hopp. I vårt scenario, *192.168.0.4*.

3. Kör hello [az network vnet undernät uppdatering](/cli/azure/network/vnet/subnet#update) routningstabellen för kommandot tooassociate hello skapade ovan med hello **klientdel** undernät:

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name FrontEnd \
    --route-table UDR-FrontEnd
    ```

    Resultat:

    ```json
    {
    "addressPrefix": "192.168.1.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/FrontEnd",
    "ipConfigurations": null,
    "name": "FrontEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": {
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
        "location": null,
        "name": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "routes": null,
        "subnets": null,
        "tags": null,
        "type": null
        }
    }
    ```

    Parametrar:
    
    * **--vnet-name**. Namnet på hello VNet där undernätet hello finns. I vårt scenario, *TestVNet*.

## <a name="create-hello-udr-for-hello-back-end-subnet"></a>Skapa hello UDR för hello backend-undernät

toocreate hello routningstabellen och dirigera behövs för hello backend-undernät baserat på hello scenariot ovan fullständig hello följande steg:

1. Kör följande kommando toocreate hello en routningstabell för hello backend-undernät:

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --name UDR-BackEnd \
    --location centralus
    ```

2. Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello frontend undernät (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):

    ```azurecli
    az network route-table route create \
    --resource-group testrg \
    --name RouteToFrontEnd \
    --route-table-name UDR-BackEnd \
    --address-prefix 192.168.1.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

3. Kör hello följande kommando tooassociate hello routningstabellen med hello **BackEnd** undernät:

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name BackEnd \
    --route-table UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>Aktivera IP-vidarebefordring på FW1

tooenable IP-vidarebefordran i hello NIC som används av **FW1**, fullständig hello följande steg:

1. Kör hello [az nätverket nic visa](/cli/azure/network/nic#show) med en JMESPATH filter toodisplay hello aktuella **aktivera IP-vidarebefordring** värde för **aktivera IP-vidarebefordring**. Det ska ställas in för*FALSKT*.

    ```azurecli
    az network nic show \
    --resource-group testrg \
    --nname nicfw1 \
    --query 'enableIpForwarding' -o tsv
    ```

    Resultat:

        false

2. Kör följande kommando tooenable IP-vidarebefordring hello:

    ```azurecli
    az network nic update \
    --resource-group testrg \
    --name nicfw1 \
    --ip-forwarding true
    ```

    Du kan granska hello utdata direktuppspelat toohello konsolen eller bara testa om för specifika hello **enableIpForwarding** värde:

    ```azurecli
    az network nic show -g testrg -n nicfw1 --query 'enableIpForwarding' -o tsv
    ```

    Resultat:

        true

    Parametrar:

    **– ip-vidarebefordran**: *SANT* eller *FALSKT*.

