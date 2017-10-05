---
title: "Skapa en intern belastningsutjämnare – klassiska Azure CLI | Microsoft Docs"
description: "Lär dig hur du skapar en intern belastningsutjämnare med hjälp av Azure CLI i den klassiska distributionsmodellen"
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
ms.openlocfilehash: d24b95f75b5ffd1116b07cf9f8bac33767a9c835
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a><span data-ttu-id="e5660-103">Komma igång med att skapa en intern belastningsutjämnare (klassisk) med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e5660-103">Get started creating an internal load balancer (classic) using the Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e5660-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e5660-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="e5660-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e5660-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="e5660-106">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="e5660-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="e5660-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e5660-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="e5660-108">Den här artikeln beskriver den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="e5660-108">This article covers using the classic deployment model.</span></span> <span data-ttu-id="e5660-109">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="e5660-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="e5660-110">Lär dig hur du [utför dessa steg med hjälp av Resource Manager-modellen](load-balancer-get-started-ilb-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e5660-110">Learn how to [perform these steps using the Resource Manager model](load-balancer-get-started-ilb-arm-cli.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a><span data-ttu-id="e5660-111">Så här skapar du en intern belastningsutjämningsuppsättning för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e5660-111">To create an internal load balancer set for virtual machines</span></span>

<span data-ttu-id="e5660-112">Följ dessa steg för att skapa en intern belastningsutjämningsuppsättning och de servrar som ska skicka sin trafik till den:</span><span class="sxs-lookup"><span data-stu-id="e5660-112">To create an internal load balancer set and the servers that will send their traffic to it, you must do the following:</span></span>

1. <span data-ttu-id="e5660-113">Skapa en instans för intern belastningsutjämning som ska vara slutpunkten för inkommande trafik som ska belastningsutjämnas mellan servrarna i en belastningsutjämnad uppsättning.</span><span class="sxs-lookup"><span data-stu-id="e5660-113">Create an instance of Internal Load Balancing that will be the endpoint of incoming traffic to be load balanced across the servers of a load-balanced set.</span></span>
2. <span data-ttu-id="e5660-114">Lägg till slutpunkter som motsvarar de virtuella datorerna som ska ta emot den inkommande trafiken.</span><span class="sxs-lookup"><span data-stu-id="e5660-114">Add endpoints corresponding to the virtual machines that will be receiving the incoming traffic.</span></span>
3. <span data-ttu-id="e5660-115">Konfigurera servrarna som ska skicka trafiken som ska belastningsutjämnas så att de skickar trafiken till den virtuella IP-adressen för instansen för intern belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="e5660-115">Configure the servers that will be sending the traffic to be load balanced to send their traffic to the virtual IP (VIP) address of the Internal Load Balancing instance.</span></span>

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a><span data-ttu-id="e5660-116">Stegvisa anvisningar som beskriver hur du skapar en intern belastningsutjämnare med hjälp av CLI</span><span class="sxs-lookup"><span data-stu-id="e5660-116">Step by step creating an internal load balancer using CLI</span></span>

<span data-ttu-id="e5660-117">Den här guiden beskriver hur du skapar en intern belastningsutjämnare baserat på scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="e5660-117">This guide shows how to create an internal load balancer based on the scenario above.</span></span>

1. <span data-ttu-id="e5660-118">Om du aldrig har använt Azure CLI, se [installera och konfigurera Azure CLI](../cli-install-nodejs.md) och följ instruktionerna upp till den punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e5660-118">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="e5660-119">Kör kommandot **azure config mode** för att växla till klassiskt läge, som du ser nedan.</span><span class="sxs-lookup"><span data-stu-id="e5660-119">Run the **azure config mode** command to switch to classic mode, as shown below.</span></span>

    ```azurecli
    azure config mode asm
    ```

    <span data-ttu-id="e5660-120">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="e5660-120">Expected output:</span></span>

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a><span data-ttu-id="e5660-121">Skapa slutpunkt och belastningsutjämningsuppsättning</span><span class="sxs-lookup"><span data-stu-id="e5660-121">Create endpoint and load balancer set</span></span>

<span data-ttu-id="e5660-122">I det här scenariot förutsätter vi att de virtuella datorerna ”DB1” och ”DB2” i en molntjänst med namnet ”mytestcloud” redan finns.</span><span class="sxs-lookup"><span data-stu-id="e5660-122">The scenario assumes the virtual machines "DB1" and "DB2" in a cloud service called "mytestcloud".</span></span> <span data-ttu-id="e5660-123">Båda de virtuella datorerna använder ett virtuellt nätverk med namnet ”testvnet” med undernätet ”subnet-1”.</span><span class="sxs-lookup"><span data-stu-id="e5660-123">Both virtual machines are using a virtual network called my "testvnet" with subnet "subnet-1".</span></span>

<span data-ttu-id="e5660-124">I den här guiden skapar vi en intern belastningsutjämningsuppsättning som använder port 1433 som privat port och 1433 som lokal port.</span><span class="sxs-lookup"><span data-stu-id="e5660-124">This guide will create an internal load balancer set using port 1433 as private port and 1433 as local port.</span></span>

<span data-ttu-id="e5660-125">Det här är ett vanligt scenario där du har virtuella SQL-datorer på backend-servern som använder en intern belastningsutjämnare för att säkerställa att databasservrarna inte exponeras direkt genom användningen av en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e5660-125">This is a common scenario where you have SQL virtual machines on the back end using an internal load balancer to guarantee the database servers won't be exposed directly using a public IP address.</span></span>

### <a name="step-1"></a><span data-ttu-id="e5660-126">Steg 1</span><span class="sxs-lookup"><span data-stu-id="e5660-126">Step 1</span></span>

<span data-ttu-id="e5660-127">Skapa en intern belastningsutjämningsuppsättning med hjälp av `azure network service internal-load-balancer add`.</span><span class="sxs-lookup"><span data-stu-id="e5660-127">Create an internal load balancer set using `azure network service internal-load-balancer add`.</span></span>

```azurecli
azure service internal-load-balancer add --serviceName mytestcloud --internalLBName ilbset --subnet-name subnet-1 --static-virtualnetwork-ipaddress 192.168.2.7
```

<span data-ttu-id="e5660-128">Mer information finns i `azure service internal-load-balancer --help`.</span><span class="sxs-lookup"><span data-stu-id="e5660-128">Check out `azure service internal-load-balancer --help` for more information.</span></span>

<span data-ttu-id="e5660-129">Du kan kontrollera egenskaperna för den interna belastningsutjämnaren med hjälp av kommandot `azure service internal-load-balancer list` *molntjänstens namn*.</span><span class="sxs-lookup"><span data-stu-id="e5660-129">You can check the internal load balancer properties using the command `azure service internal-load-balancer list` *cloud service name*.</span></span>

<span data-ttu-id="e5660-130">Här är ett exempel på utdata som kan returneras:</span><span class="sxs-lookup"><span data-stu-id="e5660-130">Here follows an example of the output:</span></span>

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


### <a name="step-2"></a><span data-ttu-id="e5660-131">Steg 2</span><span class="sxs-lookup"><span data-stu-id="e5660-131">Step 2</span></span>

<span data-ttu-id="e5660-132">Du konfigurerar den interna belastningsutjämningsuppsättningen när du lägger till den första slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="e5660-132">You configure the internal load balancer set when you add the first endpoint.</span></span> <span data-ttu-id="e5660-133">Du associerar slutpunkten, den virtuella datorn och avsökningsporten till den intern belastningsutjämningsuppsättningen i det här steget.</span><span class="sxs-lookup"><span data-stu-id="e5660-133">You will associate the endpoint, virtual machine and probe port to the internal load balancer set in this step.</span></span>

```azurecli
azure vm endpoint create db1 1433 --local-port 1433 --protocol tcp --probe-port 1433 --probe-protocol tcp --probe-interval 300 --probe-timeout 600 --internal-load-balancer-name ilbset
```

### <a name="step-3"></a><span data-ttu-id="e5660-134">Steg 3</span><span class="sxs-lookup"><span data-stu-id="e5660-134">Step 3</span></span>

<span data-ttu-id="e5660-135">Kontrollera belastningsutjämnarens konfiguration med hjälp av `azure vm show` *namn på virtuell dator*</span><span class="sxs-lookup"><span data-stu-id="e5660-135">Verify the load balancer configuration using `azure vm show` *virtual machine name*</span></span>

```azurecli
azure vm show DB1
```

<span data-ttu-id="e5660-136">Följande utdata returneras:</span><span class="sxs-lookup"><span data-stu-id="e5660-136">The output will be:</span></span>

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

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a><span data-ttu-id="e5660-137">Skapa en fjärrskrivbordsslutpunkt för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e5660-137">Create a remote desktop endpoint for a virtual machine</span></span>

<span data-ttu-id="e5660-138">Du kan skapa en fjärrskrivbordsslutpunkt för att vidarebefordra nätverkstrafik från en offentlig port till en lokal port för en specifik virtuell dator med hjälp av `azure vm endpoint create`.</span><span class="sxs-lookup"><span data-stu-id="e5660-138">You can create a remote desktop endpoint to forward network traffic from a public port to a local port for a specific virtual machine using `azure vm endpoint create`.</span></span>

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a><span data-ttu-id="e5660-139">Ta bort en virtuell dator från belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="e5660-139">Remove virtual machine from load balancer</span></span>

<span data-ttu-id="e5660-140">Du kan ta bort en virtuell dator från en intern belastningsutjämningsuppsättning genom att ta bort den associerade slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="e5660-140">You can remove a virtual machine from an internal load balancer set by deleting the associated endpoint.</span></span> <span data-ttu-id="e5660-141">När slutpunkten har tagits bort tillhör inte den virtuella datorn belastningsutjämningsuppsättningen längre.</span><span class="sxs-lookup"><span data-stu-id="e5660-141">Once the endpoint is removed, the virtual machine won't belong to the load balancer set anymore.</span></span>

<span data-ttu-id="e5660-142">Om du följer exemplet ovan kan du ta bort slutpunkten som skapats för den virtuella datorn ”DB1” från den intern belastningsutjämningsuppsättningen ”ilbset” med hjälp av kommandot `azure vm endpoint delete`.</span><span class="sxs-lookup"><span data-stu-id="e5660-142">Using the example above, you can remove the endpoint created for virtual machine "DB1" from internal load balancer "ilbset" by using the command `azure vm endpoint delete`.</span></span>

```azurecli
azure vm endpoint delete DB1 tcp-1433-1433
```

<span data-ttu-id="e5660-143">Mer information finns i `azure vm endpoint --help`.</span><span class="sxs-lookup"><span data-stu-id="e5660-143">Check out `azure vm endpoint --help` for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5660-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5660-144">Next steps</span></span>

[<span data-ttu-id="e5660-145">Konfigurera ett distributionsläge för belastningsutjämnare med hjälp av käll-IP-tilldelning</span><span class="sxs-lookup"><span data-stu-id="e5660-145">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="e5660-146">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="e5660-146">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
