---
title: "aaaCreate en virtuell dator med flera nätverkskort – Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur toocreate en virtuell dator med flera nätverkskort med hello Azure CLI 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8e906a4b-8583-4a97-9416-ee34cfa09a98
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 07c660b632bcdc004365a6f910ecf8a5c13cbc6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-hello-azure-cli-10"></a><span data-ttu-id="3c69c-103">Skapa en virtuell dator med flera nätverkskort med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="3c69c-103">Create a VM with multiple NICs using hello Azure CLI 1.0</span></span>

[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="3c69c-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3c69c-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="3c69c-105">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](virtual-network-deploy-multinic-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="3c69c-105">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](virtual-network-deploy-multinic-classic-cli.md).</span></span>
>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="3c69c-106">hello så här använder du en resursgrupp med namnet *IaaSStory* för hello webbservrar och en resursgrupp med namnet *IaaSStory BackEnd* för hello DB-servrar.</span><span class="sxs-lookup"><span data-stu-id="3c69c-106">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span> <span data-ttu-id="3c69c-107">Du kan göra detta med hjälp av hello Azure CLI 1.0 (den här artikeln) eller hello [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="3c69c-107">You can complete this task using hello Azure CLI 1.0 (this article) or hello [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md).</span></span> <span data-ttu-id="3c69c-108">Hej värdena i ”” hello variabler i hello steg som följer skapa resurser med inställningar från hello scenario.</span><span class="sxs-lookup"><span data-stu-id="3c69c-108">hello values in "" for hello variables in hello steps that follow create resources with settings from hello scenario.</span></span> <span data-ttu-id="3c69c-109">Ändra hello värden som är lämpliga för din miljö.</span><span class="sxs-lookup"><span data-stu-id="3c69c-109">Change hello values, as appropriate, for your environment.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c69c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3c69c-110">Prerequisites</span></span>
<span data-ttu-id="3c69c-111">Innan du kan skapa hello DB-servrar, behöver du toocreate hello *IaaSStory* resursgrupp med alla hello nödvändiga resurser för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="3c69c-111">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="3c69c-112">toocreate dessa resurser, Slutför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3c69c-112">toocreate these resources, complete hello following steps:</span></span>

1. <span data-ttu-id="3c69c-113">Navigera för[hello mallsidan](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="3c69c-113">Navigate too[hello template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="3c69c-114">Hello mallen sidan toohello höger i **överordnade resursgruppen**, klickar du på **distribuera tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="3c69c-114">In hello template page, toohello right of **Parent resource group**, click **Deploy tooAzure**.</span></span>
3. <span data-ttu-id="3c69c-115">Om det behövs, ändra hello parametervärden sedan gör hello i hello Azure preview portal toodeploy hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="3c69c-115">If needed, change hello parameter values, then follow hello steps in hello Azure preview portal toodeploy hello resource group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3c69c-116">Kontrollera att din lagringskontonamn är unika.</span><span class="sxs-lookup"><span data-stu-id="3c69c-116">Make sure your storage account names are unique.</span></span> <span data-ttu-id="3c69c-117">Du kan inte ha dubbla lagringskontonamn i Azure.</span><span class="sxs-lookup"><span data-stu-id="3c69c-117">You cannot have duplicate storage account names in Azure.</span></span>
> 

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-hello-back-end-vms"></a><span data-ttu-id="3c69c-118">Skapa hello backend-virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3c69c-118">Create hello back-end VMs</span></span>
<span data-ttu-id="3c69c-119">hello beror backend-VMs på hello skapande av hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="3c69c-119">hello back-end VMs depend on hello creation of hello following resources:</span></span>

* <span data-ttu-id="3c69c-120">**Storage-konto för datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="3c69c-120">**Storage account for data disks**.</span></span> <span data-ttu-id="3c69c-121">För bättre prestanda använder hello datadiskar på hello databasservrar Solid-State-hårddisk (SSD)-teknik som kräver ett premiumlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3c69c-121">For better performance, hello data disks on hello database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="3c69c-122">Se till att hello Azure-plats som du distribuerar toosupport premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="3c69c-122">Make sure hello Azure location you deploy toosupport premium storage.</span></span>
* <span data-ttu-id="3c69c-123">**Nätverkskort**.</span><span class="sxs-lookup"><span data-stu-id="3c69c-123">**NICs**.</span></span> <span data-ttu-id="3c69c-124">Varje virtuell dator har två nätverkskort, ett för åtkomst till databasen, och en för hantering.</span><span class="sxs-lookup"><span data-stu-id="3c69c-124">Each VM will have two NICs, one for database access, and one for management.</span></span>
* <span data-ttu-id="3c69c-125">**Tillgänglighetsuppsättningen**.</span><span class="sxs-lookup"><span data-stu-id="3c69c-125">**Availability set**.</span></span> <span data-ttu-id="3c69c-126">Alla databasservrar läggs tooa enda tillgänglighetsuppsättning, tooensure minst en av hello virtuella datorer är igång och körs under underhåll.</span><span class="sxs-lookup"><span data-stu-id="3c69c-126">All database servers will be added tooa single availability set, tooensure at least one of hello VMs is up and running during maintenance.</span></span>

### <a name="step-1---start-your-script"></a><span data-ttu-id="3c69c-127">Steg 1 – starta skriptet</span><span class="sxs-lookup"><span data-stu-id="3c69c-127">Step 1 - Start your script</span></span>
<span data-ttu-id="3c69c-128">Du kan hämta hello fullständig bash-skript används [här](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-cli.sh).</span><span class="sxs-lookup"><span data-stu-id="3c69c-128">You can download hello full bash script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-cli.sh).</span></span> <span data-ttu-id="3c69c-129">Följ hello steg nedan toochange hello skriptet toowork i din miljö.</span><span class="sxs-lookup"><span data-stu-id="3c69c-129">Follow hello steps below toochange hello script toowork in your environment.</span></span>

1. <span data-ttu-id="3c69c-130">Ändra hello värdena för hello variabler nedan baserat på en befintlig resursgrupp distribuerade ovan i [krav](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="3c69c-130">Change hello values of hello variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```azurecli
    existingRGName="IaaSStory"
    location="westus"
    vnetName="WTestVNet"
    backendSubnetName="BackEnd"
    remoteAccessNSGName="NSG-RemoteAccess"
    ```
2. <span data-ttu-id="3c69c-131">Ändra hello värden hello variabler nedan baserat på hello värden som du vill toouse för backend-distribution.</span><span class="sxs-lookup"><span data-stu-id="3c69c-131">Change hello values of hello variables below based on hello values you want toouse for your backend deployment.</span></span>

    ```azurecli
    backendRGName="IaaSStory-Backend"
    prmStorageAccountName="wtestvnetstorageprm"
    avSetName="ASDB"
    vmSize="Standard_DS3"
    diskSize=127
    publisher="Canonical"
    offer="UbuntuServer"
    sku="14.04.2-LTS"
    version="latest"
    vmNamePrefix="DB"
    osDiskName="osdiskdb"
    dataDiskName="datadisk"
    nicNamePrefix="NICDB"
    ipAddressPrefix="192.168.2."
    username='adminuser'
    password='adminP@ssw0rd'
    numberOfVMs=2
    ```

3. <span data-ttu-id="3c69c-132">Hämta hello-ID för hello `BackEnd` undernät där hello virtuella datorer kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="3c69c-132">Retrieve hello ID for hello `BackEnd` subnet where hello VMs will be created.</span></span> <span data-ttu-id="3c69c-133">Du behöver toodo detta eftersom hello nätverkskort toobe associerade toothis undernät i en annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3c69c-133">You need toodo this since hello NICs toobe associated toothis subnet are in a different resource group.</span></span>

    ```azurecli
    subnetId="$(azure network vnet subnet show --resource-group $existingRGName \
            --vnet-name $vnetName \
            --name $backendSubnetName|grep Id)"
    subnetId=${subnetId#*/}
    ```

   > [!TIP]
   > <span data-ttu-id="3c69c-134">Hej första kommandot ovan använder [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) och [string manipulering](http://tldp.org/LDP/abs/html/string-manipulation.html) (mer specifikt delsträngen tas bort).</span><span class="sxs-lookup"><span data-stu-id="3c69c-134">hello first command above uses [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) and [string manipulation](http://tldp.org/LDP/abs/html/string-manipulation.html) (more specifically, substring removal).</span></span>
   >

4. <span data-ttu-id="3c69c-135">Hämta hello-ID för hello `NSG-RemoteAccess` NSG.</span><span class="sxs-lookup"><span data-stu-id="3c69c-135">Retrieve hello ID for hello `NSG-RemoteAccess` NSG.</span></span> <span data-ttu-id="3c69c-136">Du behöver toodo detta eftersom hello nätverkskort toobe associerade toothis NSG tillhör en annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3c69c-136">You need toodo this since hello NICs toobe associated toothis NSG are in a different resource group.</span></span>

    ```azurecli
    nsgId="$(azure network nsg show --resource-group $existingRGName \
        --name $remoteAccessNSGName|grep Id)"
        nsgId=${nsgId#*/}
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="3c69c-137">Steg 2 – skapa nödvändiga resurser för din virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3c69c-137">Step 2 - Create necessary resources for your VMs</span></span>

1. <span data-ttu-id="3c69c-138">Skapa en ny resursgrupp för alla backend-resurser.</span><span class="sxs-lookup"><span data-stu-id="3c69c-138">Create a new resource group for all backend resources.</span></span> <span data-ttu-id="3c69c-139">Meddelande hello användning av hello `$backendRGName` variabel för hello resursgruppens namn, och `$location` för hello Azure-region.</span><span class="sxs-lookup"><span data-stu-id="3c69c-139">Notice hello use of hello `$backendRGName` variable for hello resource group name, and `$location` for hello Azure region.</span></span>

    ```azurecli
    azure group create $backendRGName $location
    ```

2. <span data-ttu-id="3c69c-140">Skapa ett premiumlagringskonto för hello OS och data diskar toobe används av dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3c69c-140">Create a premium storage account for hello OS and data disks toobe used by yours VMs.</span></span>

    ```azurecli
    azure storage account create $prmStorageAccountName \
        --resource-group $backendRGName \
        --location $location \
        --type PLRS
    ```

3. <span data-ttu-id="3c69c-141">Skapa en tillgänglighetsuppsättning för hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3c69c-141">Create an availability set for hello VMs.</span></span>

    ```azurecli
    azure availset create --resource-group $backendRGName \
        --location $location \
        --name $avSetName
    ```

### <a name="step-3---create-hello-nics-and-back-end-vms"></a><span data-ttu-id="3c69c-142">Steg 3 – skapa hello nätverkskort och backend-virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3c69c-142">Step 3 - Create hello NICs and back-end VMs</span></span>

1. <span data-ttu-id="3c69c-143">Starta en loop toocreate flera virtuella datorer, baserat på hello `numberOfVMs` variabler.</span><span class="sxs-lookup"><span data-stu-id="3c69c-143">Start a loop toocreate multiple VMs, based on hello `numberOfVMs` variables.</span></span>

    ```azurecli
    for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
    do
    ```

2. <span data-ttu-id="3c69c-144">Skapa ett nätverkskort för åtkomst till databasen för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3c69c-144">For each VM, create a NIC for database access.</span></span>

    ```azurecli
    nic1Name=$nicNamePrefix$suffixNumber-DA
    x=$((suffixNumber+3))
    ipAddress1=$ipAddressPrefix$x
    azure network nic create --name $nic1Name \
        --resource-group $backendRGName \
        --location $location \
        --private-ip-address $ipAddress1 \
        --subnet-id $subnetId
    ```

3. <span data-ttu-id="3c69c-145">Skapa ett nätverkskort för fjärråtkomst för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3c69c-145">For each VM, create a NIC for remote access.</span></span> <span data-ttu-id="3c69c-146">Meddelande hello `--network-security-group` parameter, används tooassociate hello NIC tooan NSG.</span><span class="sxs-lookup"><span data-stu-id="3c69c-146">Notice hello `--network-security-group` parameter, used tooassociate hello NIC tooan NSG.</span></span>

    ```azurecli
    nic2Name=$nicNamePrefix$suffixNumber-RA
    x=$((suffixNumber+53))
    ipAddress2=$ipAddressPrefix$x
    azure network nic create --name $nic2Name \
        --resource-group $backendRGName \
        --location $location \
        --private-ip-address $ipAddress2 \
        --subnet-id $subnetId $vnetName \
        --network-security-group-id $nsgId
    ```

4. <span data-ttu-id="3c69c-147">Skapa hello VM.</span><span class="sxs-lookup"><span data-stu-id="3c69c-147">Create hello VM.</span></span>

    ```azurecli
    azure vm create --resource-group $backendRGName \
        --name $vmNamePrefix$suffixNumber \
        --location $location \
        --vm-size $vmSize \
        --subnet-id $subnetId \
        --availset-name $avSetName \
        --nic-names $nic1Name,$nic2Name \
        --os-type linux \
        --image-urn $publisher:$offer:$sku:$version \
        --storage-account-name $prmStorageAccountName \
        --storage-account-container-name vhds \
        --os-disk-vhd $osDiskName$suffixNumber.vhd \
        --admin-username $username \
        --admin-password $password
    ```

5. <span data-ttu-id="3c69c-148">För varje virtuell dator skapar du två diskar och avsluta hello loop med hello `done` kommando.</span><span class="sxs-lookup"><span data-stu-id="3c69c-148">For each VM, create two data disks, and end hello loop with hello `done` command.</span></span>

    ```azurecli
    azure vm disk attach-new --resource-group $backendRGName \
        --vm-name $vmNamePrefix$suffixNumber \
        --storage-account-name $prmStorageAccountName \
        --storage-account-container-name vhds \
        --vhd-name $dataDiskName$suffixNumber-1.vhd \
        --size-in-gb $diskSize \
        --lun 0

    azure vm disk attach-new --resource-group $backendRGName \
        --vm-name $vmNamePrefix$suffixNumber \        
        --storage-account-name $prmStorageAccountName \
        --storage-account-container-name vhds \
        --vhd-name $dataDiskName$suffixNumber-2.vhd \
        --size-in-gb $diskSize \
        --lun 1
        done
    ```

### <a name="step-4---run-hello-script"></a><span data-ttu-id="3c69c-149">Steg 4 – kör hello skript</span><span class="sxs-lookup"><span data-stu-id="3c69c-149">Step 4 - Run hello script</span></span>
<span data-ttu-id="3c69c-150">Nu när du hämtade och ändrade hello skript baserat på dina behov, kör hello skriptet toocreate hello tillbaka avslutas databasen virtuella datorer med flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="3c69c-150">Now that you downloaded and changed hello script based on your needs, run hello script toocreate hello back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="3c69c-151">Spara skriptet och kör det från din **Bash** terminal.</span><span class="sxs-lookup"><span data-stu-id="3c69c-151">Save your script and run it from your **Bash** terminal.</span></span> <span data-ttu-id="3c69c-152">Hello inledande utdata visas enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="3c69c-152">You will see hello initial output, as shown below.</span></span>
   
        info:    Executing command group create
        info:    Getting resource group IaaSStory-Backend
        info:    Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command availset create
        info:    Looking up hello availability set "ASDB"
        info:    Creating availability set "ASDB"
        info:    availset create command OK
        info:    Executing command network nic create
        info:    Looking up hello network interface "NICDB1-DA"
        info:    Creating network interface "NICDB1-DA"
        info:    Looking up hello network interface "NICDB1-DA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-DA
        data:    Name                            : NICDB1-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up hello network interface "NICDB1-RA"
        info:    Creating network interface "NICDB1-RA"
        info:    Looking up hello network interface "NICDB1-RA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-RA
        data:    Name                            : NICDB1-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.54
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up hello VM "DB1"
        info:    Using hello VM Size "Standard_DS3"
        info:    hello [OS, Data] Disk or image configuration requires storage account
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    Looking up hello availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up hello NIC "NICDB1-DA"
        info:    Looking up hello NIC "NICDB1-RA"
        info:    Creating VM "DB1"
2. <span data-ttu-id="3c69c-153">Hello körningen avslutas efter några minuter och du ser hello resten av hello utdata som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="3c69c-153">After a few minutes, hello execution will end and you will see hello rest of hello output as shown below.</span></span>
   
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up hello VM "DB1"
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-1.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up hello VM "DB1"
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-2.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command network nic create
        info:    Looking up hello network interface "NICDB2-DA"
        info:    Creating network interface "NICDB2-DA"
        info:    Looking up hello network interface "NICDB2-DA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-DA
        data:    Name                            : NICDB2-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.5
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up hello network interface "NICDB2-RA"
        info:    Creating network interface "NICDB2-RA"
        info:    Looking up hello network interface "NICDB2-RA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-RA
        data:    Name                            : NICDB2-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.55
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up hello VM "DB2"
        info:    Using hello VM Size "Standard_DS3"
        info:    hello [OS, Data] Disk or image configuration requires storage account
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    Looking up hello availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up hello NIC "NICDB2-DA"
        info:    Looking up hello NIC "NICDB2-RA"
        info:    Creating VM "DB2"
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up hello VM "DB2"
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-1.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up hello VM "DB2"
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-2.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK

