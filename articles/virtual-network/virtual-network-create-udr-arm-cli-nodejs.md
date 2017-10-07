---
title: "aaaControl Routning och virtuella installationer med hjälp av hello Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur toocontrol Routning och virtuella installationer med hjälp av hello Azure CLI 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2017
ms.author: jdial
ms.openlocfilehash: 1c8a552d949521fa554880c00405e65fa47a8162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-hello-azure-cli-10"></a>Skapa användardefinierade vägar (UDR) med hjälp av hello Azure CLI 1.0

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Mall](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klassisk)](virtual-network-create-udr-classic-ps.md)
> * [CLI (klassisk)](virtual-network-create-udr-classic-cli.md)

Skapa anpassad Routning och virtuella installationer med hjälp av hello Azure CLI.

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet 

Du kan göra hello med hjälp av något av följande versioner av CLI hello: 

- [Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](virtual-network-create-udr-arm-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering 


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

hello exempel Azure CLI-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan. Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö genom att distribuera [mallen](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klickar du på **distribuera tooAzure**, Ersätt hello Standardparametervärden Om nödvändigt och hello anvisningar i hello portal.


## <a name="create-hello-udr-for-hello-front-end-subnet"></a>Skapa hello UDR för hello frontend undernät
routningstabellen för toocreate hello och väg som behövs för hello klientdelen undernät baserat på hello scenariot ovan, följer du hello stegen nedan.

1. Kör följande kommando toocreate hello en routningstabell för hello frontend undernät:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest
    ```
   
    Resultat:
   
        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK
   
    Parametrar:
   
   * **-g (eller --resource-group)**. Namnet på hello resursgruppen där hello UDR kommer att skapas. I vårt exempel, *TestRG*.
   * **-l (eller --location)**. Azure-region där hello nya UDR kommer att skapas. I vårt scenario, *westus*.
   * **-n (eller --name)**. Namn för hello nya UDR. I vårt scenario, *UDR klientdel*.
2. Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello backend-undernät (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4
    ```
   
    Resultat:
   
        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK
   
    Parametrar:
   
   * **-r (eller--routningstabellnamnet-)**. Namnet på hello routningstabellen där hello flödet kommer att läggas till. I vårt scenario, *UDR klientdel*.
   * **-a (eller --address-prefix)**. Adressprefixet för hello undernät där paket som är avsedda att. I vårt scenario, *192.168.2.0/24*.
   * **-y (eller--nästa hopptyp)**. Typ av objekt trafik skickas till. Möjliga värden är *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, eller *ingen*.
   * **-p (eller--nästa-hopp-ip-adress**). IP-adressen för nästa hopp. I vårt scenario, *192.168.0.4*.
3. Hello kör följande kommando tooassociate hello routningstabellen skapade ovan med hello **klientdel** undernät:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Resultat:
   
        info:    Executing command network vnet subnet set
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up hello subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
   
    Parametrar:
   
   * **-e (eller--vnet-name)**. Namnet på hello VNet där undernätet hello finns. I vårt scenario, *TestVNet*.

## <a name="create-hello-udr-for-hello-back-end-subnet"></a>Skapa hello UDR för hello backend-undernät
toocreate hello routningstabellen och dirigera behövs för hello backend-undernät baserat på hello scenariot ovan fullständig hello följande steg:

1. Kör följande kommando toocreate hello en routningstabell för hello backend-undernät:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-BackEnd -l westus
    ```

2. Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello frontend undernät (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4
    ```

3. Kör hello följande kommando tooassociate hello routningstabellen med hello **BackEnd** undernät:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>Aktivera IP-vidarebefordring på FW1
tooenable IP-vidarebefordran i hello NIC som används av **FW1**, fullständig hello följande steg:

1. Kör hello-kommando som följer och Observera hello värdet för **aktivera IP-vidarebefordring**. Det ska ställas in för*FALSKT*.

    ```azurecli
    azure network nic show -g TestRG -n NICFW1
    ```

    Resultat:
   
        info:    Executing command network nic show
        info:    Looking up hello network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK
2. Kör följande kommando tooenable IP-vidarebefordring hello:

    ```azurecli
    azure network nic set -g TestRG -n NICFW1 -f true
    ```
   
    Resultat:
   
        info:    Executing command network nic set
        info:    Looking up hello network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up hello network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK
   
    Parametrar:
   
   * **-f (eller--aktivera IP-vidarebefordring)**. *SANT* eller *FALSKT*.

