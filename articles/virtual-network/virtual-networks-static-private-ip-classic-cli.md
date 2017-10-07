---
title: "aaaConfigure privata IP-adresser för virtuella datorer (klassisk) - Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur tooconfigure privata IP-adresser för virtuella datorer (klassisk) med hello Azure-kommandoradsgränssnittet (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17386acf-c708-4103-9b22-ff9bf04b778d
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 417a57181bcf5c2e6101bf3bdf63fc94ebc99df5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-hello-azure-cli-10"></a><span data-ttu-id="1a387-103">Konfigurera privat IP-adresser för en virtuell dator (klassisk) med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="1a387-103">Configure private IP addresses for a virtual machine (Classic) using hello Azure CLI 1.0</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="1a387-104">Den här artikeln beskriver hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="1a387-104">This article covers hello classic deployment model.</span></span> <span data-ttu-id="1a387-105">Du kan också [hantera en statisk privat IP-adress i hello Resource Manager-distributionsmodellen](virtual-networks-static-private-ip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1a387-105">You can also [manage a static private IP address in hello Resource Manager deployment model](virtual-networks-static-private-ip-arm-cli.md).</span></span>

<span data-ttu-id="1a387-106">hello exempel Azure CLI-kommandona nedan förväntar sig en enkel miljö som redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="1a387-106">hello sample Azure CLI commands below expect a simple environment already created.</span></span> <span data-ttu-id="1a387-107">Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö som beskrivs i [skapa ett vnet](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1a387-107">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-classic-cli.md).</span></span>

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="1a387-108">Hur toospecify en statisk privat IP-adress när du skapar en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1a387-108">How toospecify a static private IP address when creating a VM</span></span>
<span data-ttu-id="1a387-109">toocreate en ny virtuell dator med namnet *DNS01* i en ny molntjänst med namnet *TestService* baserat på hello scenariot ovan, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="1a387-109">toocreate a new VM named *DNS01* in a new cloud service named *TestService* based on hello scenario above, follow these steps:</span></span>

1. <span data-ttu-id="1a387-110">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1a387-110">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="1a387-111">Kör hello **azure-tjänsten skapar** kommandot toocreate hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="1a387-111">Run hello **azure service create** command toocreate hello cloud service.</span></span>
   
        azure service create TestService --location uscentral
   
    <span data-ttu-id="1a387-112">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="1a387-112">Expected output:</span></span>
   
        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
3. <span data-ttu-id="1a387-113">Kör hello **azure skapa vm** kommandot toocreate hello VM.</span><span class="sxs-lookup"><span data-stu-id="1a387-113">Run hello **azure create vm** command toocreate hello VM.</span></span> <span data-ttu-id="1a387-114">Observera hello-värdet för en statisk privat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="1a387-114">Notice hello value for a static private IP address.</span></span> <span data-ttu-id="1a387-115">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="1a387-115">hello list shown after hello output explains hello parameters used.</span></span>
   
        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd
   
    <span data-ttu-id="1a387-116">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="1a387-116">Expected output:</span></span>
   
        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting too"Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK
   
   * <span data-ttu-id="1a387-117">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="1a387-117">**-l (or --location)**.</span></span> <span data-ttu-id="1a387-118">Azure-region där hello VM kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="1a387-118">Azure region where hello VM will be created.</span></span> <span data-ttu-id="1a387-119">I vårt scenario, *centralus*.</span><span class="sxs-lookup"><span data-stu-id="1a387-119">For our scenario, *centralus*.</span></span>
   * <span data-ttu-id="1a387-120">**-n (eller--vm-namn)**.</span><span class="sxs-lookup"><span data-stu-id="1a387-120">**-n (or --vm-name)**.</span></span> <span data-ttu-id="1a387-121">Namnet på hello VM toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="1a387-121">Name of hello VM toobe created.</span></span>
   * <span data-ttu-id="1a387-122">**-w (eller--virtuella nätverksnamnet)**.</span><span class="sxs-lookup"><span data-stu-id="1a387-122">**-w (or --virtual-network-name)**.</span></span> <span data-ttu-id="1a387-123">Namnet på hello VNet där hello VM kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="1a387-123">Name of hello VNet where hello VM will be created.</span></span> 
   * <span data-ttu-id="1a387-124">**-S (eller--statisk ip)**.</span><span class="sxs-lookup"><span data-stu-id="1a387-124">**-S (or --static-ip)**.</span></span> <span data-ttu-id="1a387-125">Statisk privat IP-adress för hello VM.</span><span class="sxs-lookup"><span data-stu-id="1a387-125">Static private IP address for hello VM.</span></span>
   * <span data-ttu-id="1a387-126">**TestService**.</span><span class="sxs-lookup"><span data-stu-id="1a387-126">**TestService**.</span></span> <span data-ttu-id="1a387-127">Namnet på hello molnbaserad tjänst där hello VM kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="1a387-127">Name of hello cloud service where hello VM will be created.</span></span>
   * <span data-ttu-id="1a387-128">**bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**.</span><span class="sxs-lookup"><span data-stu-id="1a387-128">**bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**.</span></span> <span data-ttu-id="1a387-129">Bild som används för toocreate hello VM.</span><span class="sxs-lookup"><span data-stu-id="1a387-129">Image used toocreate hello VM.</span></span>
   * <span data-ttu-id="1a387-130">**adminuser**.</span><span class="sxs-lookup"><span data-stu-id="1a387-130">**adminuser**.</span></span> <span data-ttu-id="1a387-131">Lokal administratör för hello Windows VM.</span><span class="sxs-lookup"><span data-stu-id="1a387-131">Local administrator for hello Windows VM.</span></span>
   * <span data-ttu-id="1a387-132">**AdminP@ssw0rd**.</span><span class="sxs-lookup"><span data-stu-id="1a387-132">**AdminP@ssw0rd**.</span></span> <span data-ttu-id="1a387-133">Lokala administratörslösenordet för hello Windows VM.</span><span class="sxs-lookup"><span data-stu-id="1a387-133">Local administrator password for hello Windows VM.</span></span>

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="1a387-134">Hur tooretrieve statiska privata IP-information för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1a387-134">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="1a387-135">tooview hello statiska privata IP-information för hello skapas den virtuella datorn med hello skriptet ovan, kör följande Azure CLI kommando hello och se hello värde för *nätverk StaticIP*:</span><span class="sxs-lookup"><span data-stu-id="1a387-135">tooview hello static private IP address information for hello VM created with hello script above, run hello following Azure CLI command and observe hello value for *Network StaticIP*:</span></span>

    azure vm static-ip show DNS01

<span data-ttu-id="1a387-136">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="1a387-136">Expected output:</span></span>

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="1a387-137">Hur tooremove en statisk privat IP-adress från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1a387-137">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="1a387-138">tooremove hello statisk privat IP-adress läggs toohello VM i hello skriptet ovan, kör hello följande Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="1a387-138">tooremove hello static private IP address added toohello VM in hello script above, run hello following Azure CLI command:</span></span>

    azure vm static-ip remove DNS01

<span data-ttu-id="1a387-139">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="1a387-139">Expected output:</span></span>

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-tooadd-a-static-private-ip-tooan-existing-vm"></a><span data-ttu-id="1a387-140">Hur tooadd en statisk privat IP-tooan befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1a387-140">How tooadd a static private IP tooan existing VM</span></span>
<span data-ttu-id="1a387-141">tooadd en statisk privat IP-adress toohello VM som skapats med hjälp av hello skriptet ovan runt han följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1a387-141">tooadd a static private IP address toohello VM created using hello script above, runt he following command:</span></span>

    azure vm static-ip set DNS01 192.168.1.101

<span data-ttu-id="1a387-142">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="1a387-142">Expected output:</span></span>

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a><span data-ttu-id="1a387-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a387-143">Next steps</span></span>
* <span data-ttu-id="1a387-144">Lär dig mer om [reserverade offentliga IP-Adressen](virtual-networks-reserved-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="1a387-144">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="1a387-145">Lär dig mer om [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="1a387-145">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="1a387-146">Kontakta hello [reserverade IP-REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="1a387-146">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

