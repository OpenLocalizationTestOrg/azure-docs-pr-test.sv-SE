---
title: "aaaCreate en intern belastningsutjämnare - Azure CLI klassiska | Microsoft Docs"
description: "Lär dig hur toocreate med en intern belastningsutjämnare hello Azure CLI i hello klassiska distributionsmodellen"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: becbbbde-a118-4269-9444-d3153f00bf34
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: ef29dfda5f7a75a411bbabe8b688a31c6bf81113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-hello-azure-cli"></a>Komma igång med en intern belastningsutjämnare (klassiskt) med hjälp av hello Azure CLI

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [Molntjänster](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).  Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](load-balancer-get-started-ilb-arm-cli.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="toocreate-an-internal-load-balancer-set-for-virtual-machines"></a>toocreate en intern belastningsutjämningsuppsättning för virtuella datorer

toocreate en intern belastningsutjämnare ange och hello servrar som skickar sina trafik tooit, måste du göra hello följande:

1. Skapa en instans av intern belastningsutjämning som kommer att vara hello-slutpunkten för inkommande trafik toobe belastningsutjämnas mellan hello en belastningsutjämnad uppsättning servrar.
2. Lägga till slutpunkter motsvarande toohello virtuella datorer som tar emot hello inkommande trafik.
3. Konfigurera hello-servrar som kommer att skicka hello trafik toobe belastningsutjämnade toosend sina trafik toohello virtuella IP-adresser (VIP)-adressen för hello interna nätverksbelastning instans.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Stegvisa anvisningar som beskriver hur du skapar en intern belastningsutjämnare med hjälp av CLI

Den här guiden visar hur toocreate en intern belastningsutjämnare baserat på hello scenariot ovan.

1. Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.
2. Kör hello **azure config mode** kommandot tooswitch tooclassic läge, som visas nedan.

    ```azurecli
    azure config mode asm
    ```

    Förväntad utdata:

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a>Skapa slutpunkt och belastningsutjämningsuppsättning

hello scenariot förutsätter hello virtuella datorer ”DB1” och ”DB2” i en molntjänst som kallas ”mytestcloud”. Båda de virtuella datorerna använder ett virtuellt nätverk med namnet ”testvnet” med undernätet ”subnet-1”.

I den här guiden skapar vi en intern belastningsutjämningsuppsättning som använder port 1433 som privat port och 1433 som lokal port.

Det här är ett vanligt scenario där du har SQL virtuella datorer på hello serverdelen med hjälp av en intern belastningsutjämnare tooguarantee hello databasservrar inte visas direkt med en offentlig IP-adress.

### <a name="step-1"></a>Steg 1

Skapa en intern belastningsutjämningsuppsättning med hjälp av `azure network service internal-load-balancer add`.

```azurecli
azure service internal-load-balancer add --serviceName mytestcloud --internalLBName ilbset --subnet-name subnet-1 --static-virtualnetwork-ipaddress 192.168.2.7
```

Mer information finns i `azure service internal-load-balancer --help`.

Du kan kontrollera hello interna belastningsutjämnarens egenskaper hello kommandot `azure service internal-load-balancer list` *molntjänstnamnet*.

Här följer ett exempel på utdata hello:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


### <a name="step-2"></a>Steg 2

Du kan konfigurera hello intern belastningsutjämningsuppsättning när du lägger till hello första slutpunkten. Du kopplar hello slutpunkt, virtuell dator och avsökning port toohello intern belastningsutjämningsuppsättning i det här steget.

```azurecli
azure vm endpoint create db1 1433 --local-port 1433 --protocol tcp --probe-port 1433 --probe-protocol tcp --probe-interval 300 --probe-timeout 600 --internal-load-balancer-name ilbset
```

### <a name="step-3"></a>Steg 3

Kontrollera hello belastningen belastningsutjämnaren konfiguration av `azure vm show` *namn på virtuell dator*

```azurecli
azure vm show DB1
```

hello utdata blir:

    azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Skapa en fjärrskrivbordsslutpunkt för en virtuell dator

Du kan skapa en stationär fjärrslutpunkten tooforward nätverkstrafik från en offentlig port tooa lokal port för en specifik virtuell dator med hjälp av `azure vm endpoint create`.

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a>Ta bort en virtuell dator från belastningsutjämnaren

Du kan ta bort en virtuell dator från en intern belastningsutjämningsuppsättning genom att ta bort hello associerade slutpunkt. När hello slutpunkt har tagits bort, tillhör inte hello virtuella datorn toohello belastningsutjämningsuppsättning längre.

Använder hello-exemplet ovan, du kan ta bort hello-slutpunkt skapas för den virtuella datorn ”DB1” från interna belastningsutjämnaren ”ilbset” med kommandot hello `azure vm endpoint delete`.

```azurecli
azure vm endpoint delete DB1 tcp-1433-1433
```

Mer information finns i `azure vm endpoint --help`.

## <a name="next-steps"></a>Nästa steg

[Konfigurera ett distributionsläge för belastningsutjämnare med hjälp av käll-IP-tilldelning](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)
