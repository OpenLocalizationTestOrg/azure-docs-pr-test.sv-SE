---
title: "aaaCreate en Internetriktade belastningsutjämnare - Azure CLI klassiska | Microsoft Docs"
description: "Lär dig hur toocreate ett Internet belastningsutjämnare i den klassiska modellen med hello Azure CLI"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: e433a824-4a8a-44d2-8765-a74f52d4e584
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: e6070cbc574f74bca0cccb960ff192847d6511bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-hello-azure-cli"></a><span data-ttu-id="e8811-103">Komma igång med en Internetuppkopplad belastningsutjämnare (klassisk) i hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e8811-103">Get started creating an Internet facing load balancer (classic) in hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e8811-104">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="e8811-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="e8811-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e8811-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="e8811-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e8811-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="e8811-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="e8811-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="e8811-108">Innan du börjar arbeta med Azure-resurser, är det viktigt toounderstand att Azure har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="e8811-108">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="e8811-109">Se till att du förstår [distributionsmodeller och verktyg](../azure-classic-rm.md) innan du börjar arbeta med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="e8811-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="e8811-110">Du kan visa hello dokumentationen för olika verktyg genom att klicka på flikarna hello hello överst i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e8811-110">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="e8811-111">Den här artikeln beskriver hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="e8811-111">This article covers hello classic deployment model.</span></span> <span data-ttu-id="e8811-112">Du kan också [Lär dig hur toocreate Internet-riktade belastningsutjämnaren med Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="e8811-112">You can also [Learn how toocreate an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a><span data-ttu-id="e8811-113">Stegvisa anvisningar som beskriver hur du skapar en Internetuppkopplad belastningsutjämnare med CLI</span><span class="sxs-lookup"><span data-stu-id="e8811-113">Step by step creating an Internet facing load balancer using CLI</span></span>

<span data-ttu-id="e8811-114">Den här guiden visar hur toocreate en Internet-belastningsutjämnare baserat på hello scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="e8811-114">This guide shows how toocreate an Internet load balancer based on hello scenario above.</span></span>

1. <span data-ttu-id="e8811-115">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e8811-115">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="e8811-116">Kör hello **azure config mode** kommandot tooswitch tooclassic läge, som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="e8811-116">Run hello **azure config mode** command tooswitch tooclassic mode, as shown below.</span></span>

    ```azurecli
    azure config mode asm
    ```

    <span data-ttu-id="e8811-117">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="e8811-117">Expected output:</span></span>

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a><span data-ttu-id="e8811-118">Skapa slutpunkt och belastningsutjämningsuppsättning</span><span class="sxs-lookup"><span data-stu-id="e8811-118">Create endpoint and load balancer set</span></span>

<span data-ttu-id="e8811-119">hello scenariot förutsätter hello virtuella datorer ”web1” och ”web2” har skapats.</span><span class="sxs-lookup"><span data-stu-id="e8811-119">hello scenario assumes hello virtual machines "web1" and "web2" were created.</span></span>
<span data-ttu-id="e8811-120">I den här guiden skapar vi en belastningsutjämningsuppsättning som använder port 80 som offentlig port och port 80 som lokal port.</span><span class="sxs-lookup"><span data-stu-id="e8811-120">This guide will create a load balancer set using port 80 as public port and port 80 as local port.</span></span> <span data-ttu-id="e8811-121">En avsökningsport konfigureras också på port 80 och namngivna hello belastningsutjämnaren ange ”lbset”.</span><span class="sxs-lookup"><span data-stu-id="e8811-121">A probe port is also configured on port 80 and named hello load balancer set "lbset".</span></span>

### <a name="step-1"></a><span data-ttu-id="e8811-122">Steg 1</span><span class="sxs-lookup"><span data-stu-id="e8811-122">Step 1</span></span>

<span data-ttu-id="e8811-123">Skapa hello första slutpunkt och belastningsutjämnare som anges med `azure network vm endpoint create` för den virtuella datorn ”web1”.</span><span class="sxs-lookup"><span data-stu-id="e8811-123">Create hello first endpoint and load balancer set using `azure network vm endpoint create` for virtual machine "web1".</span></span>

```azurecli
azure vm endpoint create web1 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

## <a name="step-2"></a><span data-ttu-id="e8811-124">Steg 2</span><span class="sxs-lookup"><span data-stu-id="e8811-124">Step 2</span></span>

<span data-ttu-id="e8811-125">Lägg till en annan virtuell dator ”web2” toohello belastningen belastningsutjämnaren mängd.</span><span class="sxs-lookup"><span data-stu-id="e8811-125">Add a second virtual machine "web2" toohello load balancer set.</span></span>

```azurecli
azure vm endpoint create web2 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

## <a name="step-3"></a><span data-ttu-id="e8811-126">Steg 3</span><span class="sxs-lookup"><span data-stu-id="e8811-126">Step 3</span></span>

<span data-ttu-id="e8811-127">Kontrollera hello belastningen belastningsutjämnaren konfiguration av `azure vm show` .</span><span class="sxs-lookup"><span data-stu-id="e8811-127">Verify hello load balancer configuration using `azure vm show` .</span></span>

```azurecli
azure vm show web1
```

<span data-ttu-id="e8811-128">hello utdata blir:</span><span class="sxs-lookup"><span data-stu-id="e8811-128">hello output will be:</span></span>

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a><span data-ttu-id="e8811-129">Skapa en fjärrskrivbordsslutpunkt för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e8811-129">Create a remote desktop endpoint for a virtual machine</span></span>

<span data-ttu-id="e8811-130">Du kan skapa en stationär fjärrslutpunkten tooforward nätverkstrafik från en offentlig port tooa lokal port för en specifik virtuell dator med hjälp av `azure vm endpoint create`.</span><span class="sxs-lookup"><span data-stu-id="e8811-130">You can create a remote desktop endpoint tooforward network traffic from a public port tooa local port for a specific virtual machine using `azure vm endpoint create`.</span></span>

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a><span data-ttu-id="e8811-131">Ta bort en virtuell dator från belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="e8811-131">Remove virtual machine from load balancer</span></span>

<span data-ttu-id="e8811-132">Du har toodelete hello slutpunkten som är associerad toohello läsa in belastningsutjämning set från hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e8811-132">You have toodelete hello endpoint associated toohello load balancer set from hello virtual machine.</span></span> <span data-ttu-id="e8811-133">När hello slutpunkt har tagits bort, hör inte toohello belastningsutjämningsuppsättning längre hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e8811-133">Once hello endpoint is removed, hello virtual machine doesn't belong toohello load balancer set anymore.</span></span>

<span data-ttu-id="e8811-134">Med hello-exemplet ovan kan du ta bort hello-slutpunkt skapas för den virtuella datorn ”web1” från belastningsutjämnaren ”lbset” hello kommandot `azure vm endpoint delete`.</span><span class="sxs-lookup"><span data-stu-id="e8811-134">Using hello example above, you can remove hello endpoint created for virtual machine "web1" from load balancer "lbset" using hello command `azure vm endpoint delete`.</span></span>

```azurecli
azure vm endpoint delete web1 tcp-80-80
```

> [!NOTE]
> <span data-ttu-id="e8811-135">Du kan utforska fler alternativ toomanage slutpunkter hello kommandot`azure vm endpoint --help`</span><span class="sxs-lookup"><span data-stu-id="e8811-135">You can explore more options toomanage endpoints using hello command `azure vm endpoint --help`</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8811-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e8811-136">Next steps</span></span>

[<span data-ttu-id="e8811-137">Komma igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="e8811-137">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="e8811-138">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="e8811-138">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="e8811-139">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="e8811-139">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
