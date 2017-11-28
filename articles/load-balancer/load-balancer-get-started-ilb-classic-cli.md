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
# <a name="get-started-creating-an-internal-load-balancer-classic-using-hello-azure-cli"></a><span data-ttu-id="462ff-103">Komma igång med en intern belastningsutjämnare (klassiskt) med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="462ff-103">Get started creating an internal load balancer (classic) using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="462ff-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="462ff-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="462ff-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="462ff-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="462ff-106">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="462ff-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="462ff-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="462ff-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="462ff-108">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="462ff-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="462ff-109">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="462ff-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="462ff-110">Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](load-balancer-get-started-ilb-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="462ff-110">Learn how too[perform these steps using hello Resource Manager model](load-balancer-get-started-ilb-arm-cli.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="toocreate-an-internal-load-balancer-set-for-virtual-machines"></a><span data-ttu-id="462ff-111">toocreate en intern belastningsutjämningsuppsättning för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="462ff-111">toocreate an internal load balancer set for virtual machines</span></span>

<span data-ttu-id="462ff-112">toocreate en intern belastningsutjämnare ange och hello servrar som skickar sina trafik tooit, måste du göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="462ff-112">toocreate an internal load balancer set and hello servers that will send their traffic tooit, you must do hello following:</span></span>

1. <span data-ttu-id="462ff-113">Skapa en instans av intern belastningsutjämning som kommer att vara hello-slutpunkten för inkommande trafik toobe belastningsutjämnas mellan hello en belastningsutjämnad uppsättning servrar.</span><span class="sxs-lookup"><span data-stu-id="462ff-113">Create an instance of Internal Load Balancing that will be hello endpoint of incoming traffic toobe load balanced across hello servers of a load-balanced set.</span></span>
2. <span data-ttu-id="462ff-114">Lägga till slutpunkter motsvarande toohello virtuella datorer som tar emot hello inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="462ff-114">Add endpoints corresponding toohello virtual machines that will be receiving hello incoming traffic.</span></span>
3. <span data-ttu-id="462ff-115">Konfigurera hello-servrar som kommer att skicka hello trafik toobe belastningsutjämnade toosend sina trafik toohello virtuella IP-adresser (VIP)-adressen för hello interna nätverksbelastning instans.</span><span class="sxs-lookup"><span data-stu-id="462ff-115">Configure hello servers that will be sending hello traffic toobe load balanced toosend their traffic toohello virtual IP (VIP) address of hello Internal Load Balancing instance.</span></span>

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a><span data-ttu-id="462ff-116">Stegvisa anvisningar som beskriver hur du skapar en intern belastningsutjämnare med hjälp av CLI</span><span class="sxs-lookup"><span data-stu-id="462ff-116">Step by step creating an internal load balancer using CLI</span></span>

<span data-ttu-id="462ff-117">Den här guiden visar hur toocreate en intern belastningsutjämnare baserat på hello scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="462ff-117">This guide shows how toocreate an internal load balancer based on hello scenario above.</span></span>

1. <span data-ttu-id="462ff-118">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="462ff-118">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="462ff-119">Kör hello **azure config mode** kommandot tooswitch tooclassic läge, som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="462ff-119">Run hello **azure config mode** command tooswitch tooclassic mode, as shown below.</span></span>

    ```azurecli
    azure config mode asm
    ```

    <span data-ttu-id="462ff-120">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="462ff-120">Expected output:</span></span>

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a><span data-ttu-id="462ff-121">Skapa slutpunkt och belastningsutjämningsuppsättning</span><span class="sxs-lookup"><span data-stu-id="462ff-121">Create endpoint and load balancer set</span></span>

<span data-ttu-id="462ff-122">hello scenariot förutsätter hello virtuella datorer ”DB1” och ”DB2” i en molntjänst som kallas ”mytestcloud”.</span><span class="sxs-lookup"><span data-stu-id="462ff-122">hello scenario assumes hello virtual machines "DB1" and "DB2" in a cloud service called "mytestcloud".</span></span> <span data-ttu-id="462ff-123">Båda de virtuella datorerna använder ett virtuellt nätverk med namnet ”testvnet” med undernätet ”subnet-1”.</span><span class="sxs-lookup"><span data-stu-id="462ff-123">Both virtual machines are using a virtual network called my "testvnet" with subnet "subnet-1".</span></span>

<span data-ttu-id="462ff-124">I den här guiden skapar vi en intern belastningsutjämningsuppsättning som använder port 1433 som privat port och 1433 som lokal port.</span><span class="sxs-lookup"><span data-stu-id="462ff-124">This guide will create an internal load balancer set using port 1433 as private port and 1433 as local port.</span></span>

<span data-ttu-id="462ff-125">Det här är ett vanligt scenario där du har SQL virtuella datorer på hello serverdelen med hjälp av en intern belastningsutjämnare tooguarantee hello databasservrar inte visas direkt med en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="462ff-125">This is a common scenario where you have SQL virtual machines on hello back end using an internal load balancer tooguarantee hello database servers won't be exposed directly using a public IP address.</span></span>

### <a name="step-1"></a><span data-ttu-id="462ff-126">Steg 1</span><span class="sxs-lookup"><span data-stu-id="462ff-126">Step 1</span></span>

<span data-ttu-id="462ff-127">Skapa en intern belastningsutjämningsuppsättning med hjälp av `azure network service internal-load-balancer add`.</span><span class="sxs-lookup"><span data-stu-id="462ff-127">Create an internal load balancer set using `azure network service internal-load-balancer add`.</span></span>

```azurecli
azure service internal-load-balancer add --serviceName mytestcloud --internalLBName ilbset --subnet-name subnet-1 --static-virtualnetwork-ipaddress 192.168.2.7
```

<span data-ttu-id="462ff-128">Mer information finns i `azure service internal-load-balancer --help`.</span><span class="sxs-lookup"><span data-stu-id="462ff-128">Check out `azure service internal-load-balancer --help` for more information.</span></span>

<span data-ttu-id="462ff-129">Du kan kontrollera hello interna belastningsutjämnarens egenskaper hello kommandot `azure service internal-load-balancer list` *molntjänstnamnet*.</span><span class="sxs-lookup"><span data-stu-id="462ff-129">You can check hello internal load balancer properties using hello command `azure service internal-load-balancer list` *cloud service name*.</span></span>

<span data-ttu-id="462ff-130">Här följer ett exempel på utdata hello:</span><span class="sxs-lookup"><span data-stu-id="462ff-130">Here follows an example of hello output:</span></span>

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


### <a name="step-2"></a><span data-ttu-id="462ff-131">Steg 2</span><span class="sxs-lookup"><span data-stu-id="462ff-131">Step 2</span></span>

<span data-ttu-id="462ff-132">Du kan konfigurera hello intern belastningsutjämningsuppsättning när du lägger till hello första slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="462ff-132">You configure hello internal load balancer set when you add hello first endpoint.</span></span> <span data-ttu-id="462ff-133">Du kopplar hello slutpunkt, virtuell dator och avsökning port toohello intern belastningsutjämningsuppsättning i det här steget.</span><span class="sxs-lookup"><span data-stu-id="462ff-133">You will associate hello endpoint, virtual machine and probe port toohello internal load balancer set in this step.</span></span>

```azurecli
azure vm endpoint create db1 1433 --local-port 1433 --protocol tcp --probe-port 1433 --probe-protocol tcp --probe-interval 300 --probe-timeout 600 --internal-load-balancer-name ilbset
```

### <a name="step-3"></a><span data-ttu-id="462ff-134">Steg 3</span><span class="sxs-lookup"><span data-stu-id="462ff-134">Step 3</span></span>

<span data-ttu-id="462ff-135">Kontrollera hello belastningen belastningsutjämnaren konfiguration av `azure vm show` *namn på virtuell dator*</span><span class="sxs-lookup"><span data-stu-id="462ff-135">Verify hello load balancer configuration using `azure vm show` *virtual machine name*</span></span>

```azurecli
azure vm show DB1
```

<span data-ttu-id="462ff-136">hello utdata blir:</span><span class="sxs-lookup"><span data-stu-id="462ff-136">hello output will be:</span></span>

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

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a><span data-ttu-id="462ff-137">Skapa en fjärrskrivbordsslutpunkt för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="462ff-137">Create a remote desktop endpoint for a virtual machine</span></span>

<span data-ttu-id="462ff-138">Du kan skapa en stationär fjärrslutpunkten tooforward nätverkstrafik från en offentlig port tooa lokal port för en specifik virtuell dator med hjälp av `azure vm endpoint create`.</span><span class="sxs-lookup"><span data-stu-id="462ff-138">You can create a remote desktop endpoint tooforward network traffic from a public port tooa local port for a specific virtual machine using `azure vm endpoint create`.</span></span>

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a><span data-ttu-id="462ff-139">Ta bort en virtuell dator från belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="462ff-139">Remove virtual machine from load balancer</span></span>

<span data-ttu-id="462ff-140">Du kan ta bort en virtuell dator från en intern belastningsutjämningsuppsättning genom att ta bort hello associerade slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="462ff-140">You can remove a virtual machine from an internal load balancer set by deleting hello associated endpoint.</span></span> <span data-ttu-id="462ff-141">När hello slutpunkt har tagits bort, tillhör inte hello virtuella datorn toohello belastningsutjämningsuppsättning längre.</span><span class="sxs-lookup"><span data-stu-id="462ff-141">Once hello endpoint is removed, hello virtual machine won't belong toohello load balancer set anymore.</span></span>

<span data-ttu-id="462ff-142">Använder hello-exemplet ovan, du kan ta bort hello-slutpunkt skapas för den virtuella datorn ”DB1” från interna belastningsutjämnaren ”ilbset” med kommandot hello `azure vm endpoint delete`.</span><span class="sxs-lookup"><span data-stu-id="462ff-142">Using hello example above, you can remove hello endpoint created for virtual machine "DB1" from internal load balancer "ilbset" by using hello command `azure vm endpoint delete`.</span></span>

```azurecli
azure vm endpoint delete DB1 tcp-1433-1433
```

<span data-ttu-id="462ff-143">Mer information finns i `azure vm endpoint --help`.</span><span class="sxs-lookup"><span data-stu-id="462ff-143">Check out `azure vm endpoint --help` for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="462ff-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="462ff-144">Next steps</span></span>

[<span data-ttu-id="462ff-145">Konfigurera ett distributionsläge för belastningsutjämnare med hjälp av käll-IP-tilldelning</span><span class="sxs-lookup"><span data-stu-id="462ff-145">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="462ff-146">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="462ff-146">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
