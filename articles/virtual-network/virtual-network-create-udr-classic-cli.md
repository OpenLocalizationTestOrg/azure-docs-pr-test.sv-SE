---
title: aaaControl routning i ett Azure Virtual Network - CLI - klassisk | Microsoft Docs
description: "Lär dig hur toocontrol routning i Vnet med hjälp av hello Azure CLI i hello klassiska distributionsmodellen"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: ca2b4638-8777-4d30-b972-eb790a7c804f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 07dde573f1a605bf280156c261d51e213ede0cdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-hello-azure-cli"></a>Kontrollen Routning och använda virtuella installationer (klassiskt) med hjälp av hello Azure CLI

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Mall](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klassisk)](virtual-network-create-udr-classic-ps.md)
> * [CLI (klassisk)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Den här artikeln beskriver hello klassiska distributionsmodellen. Du kan också [styra Routning och använder virtuella installationer i hello Resource Manager-distributionsmodellen](virtual-network-create-udr-arm-cli.md).

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

hello exempel Azure CLI-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan. Om du vill toorun hello kommandon som de visas i det här dokumentet, skapa hello miljö som visas i [skapa ett VNet (klassisk) med hello Azure CLI](virtual-networks-create-vnet-classic-cli.md).

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-hello-udr-for-hello-front-end-subnet"></a>Skapa hello UDR för hello klientdelen undernät
routningstabellen för toocreate hello och väg som behövs för hello klientdelen undernät baserat på hello scenariot ovan, följer du hello stegen nedan.

1. Kör följande kommando tooswitch tooclassic läge hello:

    ```azurecli
    azure config mode asm
    ```

    Resultat:

        info:    New mode is asm

2. Kör följande kommando toocreate hello en routningstabell för hello frontend undernät:

    ```azurecli
    azure network route-table create -n UDR-FrontEnd -l uswest
    ```
   
    Resultat:
   
        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK
   
    Parametrar:
   
   * **-l (eller --location)**. Azure-region där hello ny NSG kommer att skapas. I vårt scenario, *westus*.
   * **-n (eller --name)**. Namn för hello ny NSG. I vårt scenario, *NSG-klientdel*.
3. Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello backend-undernät (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

    Resultat:
   
        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK
   
    Parametrar:
   
   * **-r (eller--routningstabellnamnet-)**. Namnet på hello routningstabellen där hello flödet kommer att läggas till. I vårt scenario, *UDR klientdel*.
   * **-a (eller --address-prefix)**. Adressprefixet för hello undernät där paket som är avsedda att. I vårt scenario, *192.168.2.0/24*.
   * **-t (eller--nästa hopptyp)**. Typ av objekt trafik skickas till. Möjliga värden är *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, eller *ingen*.
   * **-p (eller--nästa-hopp-ip-adress**). IP-adressen för nästa hopp. I vårt scenario, *192.168.0.4*.
4. Hello kör följande kommando tooassociate hello routningstabellen som skapats med hello **klientdel** undernät:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Resultat:
   
        info:    Executing command network vnet subnet route-table add
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK    
   
    Parametrar:
   
   * **-t (eller--vnet-name)**. Namnet på hello VNet där undernätet hello finns. I vårt scenario, *TestVNet*.
   * **-n (eller--undernätsnamn**. Namnet på hello routningstabellen för undernätet hello kommer att läggas till. I vårt scenario, *FrontEnd*.

## <a name="create-hello-udr-for-hello-back-end-subnet"></a>Skapa hello UDR för hello backend-undernät
routningstabellen för toocreate hello och väg som behövs för hello backend-undernät baserat på hello scenariot fullständig hello följande steg:

1. Kör följande kommando toocreate hello en routningstabell för hello backend-undernät:

    ```azurecli
    azure network route-table create -n UDR-BackEnd -l uswest
    ```

2. Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello frontend undernät (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

3. Kör hello följande kommando tooassociate hello routningstabellen med hello **BackEnd** undernät:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd
    ```

