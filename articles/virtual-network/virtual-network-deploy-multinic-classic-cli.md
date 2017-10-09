---
title: "aaaCreate en virtuell dator (klassisk) med flera nätverkskort – Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur toocreate en virtuell dator (klassisk) med flera nätverkskort med hello Azure-kommandoradsgränssnittet (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: b436e41e-866c-439f-a7c7-7b4b041725ef
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 181bfb28027caff33410ca94744e79206a2a0d0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics-using-hello-azure-cli-10"></a><span data-ttu-id="08f41-103">Skapa en virtuell dator (klassisk) med flera nätverkskort med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="08f41-103">Create a VM (Classic) with multiple NICs using hello Azure CLI 1.0</span></span>

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

<span data-ttu-id="08f41-104">Du kan skapa virtuella datorer (VM) i Azure och bifoga flera nätverk gränssnitt (NIC) tooeach för dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="08f41-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) tooeach of your VMs.</span></span> <span data-ttu-id="08f41-105">Flera nätverkskort aktivera uppdelning av trafiktyper mellan nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="08f41-105">Multiple NICs enable separation of traffic types across NICs.</span></span> <span data-ttu-id="08f41-106">Till exempel anslutna en NIC kan kommunicera med hello Internet, medan en annan inte kommunicerar endast med interna resurser toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="08f41-106">For example, one NIC might communicate with hello Internet, while another communicates only with internal resources not connected toohello Internet.</span></span> <span data-ttu-id="08f41-107">hello möjlighet tooseparate nätverkstrafik på flera nätverkskort krävs för många virtuella nätverksenheter, till exempel leverans av program och optimering av WAN-lösningar.</span><span class="sxs-lookup"><span data-stu-id="08f41-107">hello ability tooseparate network traffic across multiple NICs is required for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="08f41-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="08f41-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="08f41-109">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="08f41-109">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="08f41-110">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="08f41-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="08f41-111">Lär dig hur tooperform dessa steg med hello [Resource Manager-distributionsmodellen](virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="08f41-111">Learn how tooperform these steps using hello [Resource Manager deployment model](virtual-network-deploy-multinic-arm-cli.md).</span></span>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="08f41-112">hello så här använder du en resursgrupp med namnet *IaaSStory* för hello webbservrar och en resursgrupp med namnet *IaaSStory BackEnd* för hello DB-servrar.</span><span class="sxs-lookup"><span data-stu-id="08f41-112">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="08f41-113">Krav</span><span class="sxs-lookup"><span data-stu-id="08f41-113">Prerequisites</span></span>
<span data-ttu-id="08f41-114">Innan du kan skapa hello DB-servrar, behöver du toocreate hello *IaaSStory* resursgrupp med alla hello nödvändiga resurser för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="08f41-114">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="08f41-115">toocreate dessa resurser, fullständig hello steg som följer.</span><span class="sxs-lookup"><span data-stu-id="08f41-115">toocreate these resources, complete hello steps that follow.</span></span> <span data-ttu-id="08f41-116">Skapa ett virtuellt nätverk genom att följa stegen hello i hello [skapa ett virtuellt nätverk](virtual-networks-create-vnet-classic-cli.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="08f41-116">Create a virtual network by following hello steps in hello [Create a virtual network](virtual-networks-create-vnet-classic-cli.md) article.</span></span>

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-hello-back-end-vms"></a><span data-ttu-id="08f41-117">Distribuera hello backend-virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="08f41-117">Deploy hello back-end VMs</span></span>
<span data-ttu-id="08f41-118">hello beror backend-VMs på hello skapande av hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="08f41-118">hello back-end VMs depend on hello creation of hello following resources:</span></span>

* <span data-ttu-id="08f41-119">**Storage-konto för datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="08f41-119">**Storage account for data disks**.</span></span> <span data-ttu-id="08f41-120">För bättre prestanda använder hello datadiskar på hello databasservrar Solid-State-hårddisk (SSD)-teknik som kräver ett premiumlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="08f41-120">For better performance, hello data disks on hello database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="08f41-121">Se till att hello Azure-plats som du distribuerar toosupport premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="08f41-121">Make sure hello Azure location you deploy toosupport premium storage.</span></span>
* <span data-ttu-id="08f41-122">**Nätverkskort**.</span><span class="sxs-lookup"><span data-stu-id="08f41-122">**NICs**.</span></span> <span data-ttu-id="08f41-123">Varje virtuell dator har två nätverkskort, ett för åtkomst till databasen, och en för hantering.</span><span class="sxs-lookup"><span data-stu-id="08f41-123">Each VM will have two NICs, one for database access, and one for management.</span></span>
* <span data-ttu-id="08f41-124">**Tillgänglighetsuppsättningen**.</span><span class="sxs-lookup"><span data-stu-id="08f41-124">**Availability set**.</span></span> <span data-ttu-id="08f41-125">Alla databasservrar läggs tooa enda tillgänglighetsuppsättning, tooensure minst en av hello virtuella datorer är igång och körs under underhåll.</span><span class="sxs-lookup"><span data-stu-id="08f41-125">All database servers will be added tooa single availability set, tooensure at least one of hello VMs is up and running during maintenance.</span></span>

### <a name="step-1---start-your-script"></a><span data-ttu-id="08f41-126">Steg 1 – starta skriptet</span><span class="sxs-lookup"><span data-stu-id="08f41-126">Step 1 - Start your script</span></span>
<span data-ttu-id="08f41-127">Du kan hämta hello fullständig bash-skript används [här](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh).</span><span class="sxs-lookup"><span data-stu-id="08f41-127">You can download hello full bash script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh).</span></span> <span data-ttu-id="08f41-128">Slutför hello följande steg toochange hello skriptet toowork i din miljö:</span><span class="sxs-lookup"><span data-stu-id="08f41-128">Complete hello following steps toochange hello script toowork in your environment:</span></span>

1. <span data-ttu-id="08f41-129">Ändra hello värdena för hello variabler nedan baserat på en befintlig resursgrupp distribuerade ovan i [krav](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="08f41-129">Change hello values of hello variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```azurecli
    location="useast2"
    vnetName="WTestVNet"
    backendSubnetName="BackEnd"
    ```
2. <span data-ttu-id="08f41-130">Ändra hello värden hello variabler nedan baserat på hello värden som du vill toouse för backend-distribution.</span><span class="sxs-lookup"><span data-stu-id="08f41-130">Change hello values of hello variables below based on hello values you want toouse for your backend deployment.</span></span>

    ```azurecli
    backendCSName="IaaSStory-Backend"
    prmStorageAccountName="iaasstoryprmstorage"
    image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
    avSetName="ASDB"
    vmSize="Standard_DS3"
    diskSize=127
    vmNamePrefix="DB"
    osDiskName="osdiskdb"
    dataDiskPrefix="db"
    dataDiskName="datadisk"
    ipAddressPrefix="192.168.2."
    username='adminuser'
    password='adminP@ssw0rd'
    numberOfVMs=2
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="08f41-131">Steg 2 – skapa nödvändiga resurser för din virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="08f41-131">Step 2 - Create necessary resources for your VMs</span></span>
1. <span data-ttu-id="08f41-132">Skapa en ny molntjänst för alla virtuella datorer i serverdelen.</span><span class="sxs-lookup"><span data-stu-id="08f41-132">Create a new cloud service for all backend VMs.</span></span> <span data-ttu-id="08f41-133">Meddelande hello användning av hello `$backendCSName` variabel för hello resursgruppens namn, och `$location` för hello Azure-region.</span><span class="sxs-lookup"><span data-stu-id="08f41-133">Notice hello use of hello `$backendCSName` variable for hello resource group name, and `$location` for hello Azure region.</span></span>

    ```azurecli
    azure service create --serviceName $backendCSName \
        --location $location
    ```

2. <span data-ttu-id="08f41-134">Skapa ett premiumlagringskonto för hello OS och data diskar toobe används av dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="08f41-134">Create a premium storage account for hello OS and data disks toobe used by yours VMs.</span></span>

    ```azurecli
    azure storage account create $prmStorageAccountName \
        --location $location \
        --type PLRS
    ```

### <a name="step-3---create-vms-with-multiple-nics"></a><span data-ttu-id="08f41-135">Steg 3 – skapa virtuella datorer med flera nätverkskort</span><span class="sxs-lookup"><span data-stu-id="08f41-135">Step 3 - Create VMs with multiple NICs</span></span>
1. <span data-ttu-id="08f41-136">Starta en loop toocreate flera virtuella datorer, baserat på hello `numberOfVMs` variabler.</span><span class="sxs-lookup"><span data-stu-id="08f41-136">Start a loop toocreate multiple VMs, based on hello `numberOfVMs` variables.</span></span>

    ```azurecli
    for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
    do
    ```

2. <span data-ttu-id="08f41-137">Ange hello namn och IP-adressen för varje hello två nätverkskort för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="08f41-137">For each VM, specify hello name and IP address of each of hello two NICs.</span></span>

    ```azurecli
    nic1Name=$vmNamePrefix$suffixNumber-DA
    x=$((suffixNumber+3))
    ipAddress1=$ipAddressPrefix$x

    nic2Name=$vmNamePrefix$suffixNumber-RA
    x=$((suffixNumber+53))
    ipAddress2=$ipAddressPrefix$x
    ```

3. <span data-ttu-id="08f41-138">Skapa hello VM.</span><span class="sxs-lookup"><span data-stu-id="08f41-138">Create hello VM.</span></span> <span data-ttu-id="08f41-139">Lägg märke till hello användning av hello `--nic-config` parametern, som innehåller en lista över alla nätverkskort med namnet, undernät och IP-adress.</span><span class="sxs-lookup"><span data-stu-id="08f41-139">Notice hello usage of hello `--nic-config` parameter, containing a list of all NICs with name, subnet, and IP address.</span></span>

    ```azurecli
    azure vm create $backendCSName $image $username $password \
        --connect $backendCSName \
        --vm-name $vmNamePrefix$suffixNumber \
        --vm-size $vmSize \
        --availability-set $avSetName \
        --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
        --virtual-network-name $vnetName \
        --subnet-names $backendSubnetName \
        --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::
    ```

4. <span data-ttu-id="08f41-140">Skapa två hårddiskar för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="08f41-140">For each VM, create two data disks.</span></span>

    ```azurecli
    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
    done
    ```

### <a name="step-4---run-hello-script"></a><span data-ttu-id="08f41-141">Steg 4 – kör hello skript</span><span class="sxs-lookup"><span data-stu-id="08f41-141">Step 4 - Run hello script</span></span>
<span data-ttu-id="08f41-142">Nu när du hämtade och ändrade hello skript baserat på dina behov, kör hello skriptet toocreate hello tillbaka avslutas databasen virtuella datorer med flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="08f41-142">Now that you downloaded and changed hello script based on your needs, run hello script toocreate hello back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="08f41-143">Spara skriptet och kör det från din **Bash** terminal.</span><span class="sxs-lookup"><span data-stu-id="08f41-143">Save your script and run it from your **Bash** terminal.</span></span> <span data-ttu-id="08f41-144">Hello inledande utdata visas enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="08f41-144">You will see hello initial output, as shown below.</span></span>

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. <span data-ttu-id="08f41-145">Hello körningen avslutas efter några minuter och du ser hello resten av hello utdata som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="08f41-145">After a few minutes, hello execution will end and you will see hello rest of hello output as shown below.</span></span>

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
