---
title: "aaaCreate en virtuell dator (klassisk) med flera nätverkskort – Azure PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en virtuell dator (klassisk) med flera nätverkskort med hjälp av PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 6e50f39a-2497-4845-a5d4-7332dbc203c5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 90c967929bb418042c3fb7079e0f69246faac53c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics-using-powershell"></a><span data-ttu-id="25c4e-103">Skapa en virtuell dator (klassisk) med flera nätverkskort med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="25c4e-103">Create a VM (Classic) with multiple NICs using PowerShell</span></span>

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

<span data-ttu-id="25c4e-104">Du kan skapa virtuella datorer (VM) i Azure och bifoga flera nätverk gränssnitt (NIC) tooeach för dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="25c4e-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) tooeach of your VMs.</span></span> <span data-ttu-id="25c4e-105">Flera nätverkskort aktivera uppdelning av trafiktyper mellan nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="25c4e-105">Multiple NICs enable separation of traffic types across NICs.</span></span> <span data-ttu-id="25c4e-106">Till exempel anslutna en NIC kan kommunicera med hello Internet, medan en annan inte kommunicerar endast med interna resurser toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="25c4e-106">For example, one NIC might communicate with hello Internet, while another communicates only with internal resources not connected toohello Internet.</span></span> <span data-ttu-id="25c4e-107">hello möjlighet tooseparate nätverkstrafik på flera nätverkskort krävs för många virtuella nätverksenheter, till exempel leverans av program och optimering av WAN-lösningar.</span><span class="sxs-lookup"><span data-stu-id="25c4e-107">hello ability tooseparate network traffic across multiple NICs is required for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="25c4e-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="25c4e-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="25c4e-109">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="25c4e-109">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="25c4e-110">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="25c4e-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="25c4e-111">Lär dig hur tooperform dessa steg med hello [Resource Manager-distributionsmodellen](virtual-network-deploy-multinic-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="25c4e-111">Learn how tooperform these steps using hello [Resource Manager deployment model](virtual-network-deploy-multinic-arm-ps.md).</span></span>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="25c4e-112">hello så här använder du en resursgrupp med namnet *IaaSStory* för hello webbservrar och en resursgrupp med namnet *IaaSStory BackEnd* för hello DB-servrar.</span><span class="sxs-lookup"><span data-stu-id="25c4e-112">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25c4e-113">Krav</span><span class="sxs-lookup"><span data-stu-id="25c4e-113">Prerequisites</span></span>

<span data-ttu-id="25c4e-114">Innan du kan skapa hello DB-servrar, behöver du toocreate hello *IaaSStory* resursgrupp med alla hello nödvändiga resurser för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="25c4e-114">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="25c4e-115">toocreate dessa resurser, fullständig hello steg som följer.</span><span class="sxs-lookup"><span data-stu-id="25c4e-115">toocreate these resources, complete hello steps that follow.</span></span> <span data-ttu-id="25c4e-116">Skapa ett virtuellt nätverk genom att följa stegen hello i hello [skapa ett virtuellt nätverk](virtual-networks-create-vnet-classic-netcfg-ps.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="25c4e-116">Create a virtual network by following hello steps in hello [Create a virtual network](virtual-networks-create-vnet-classic-netcfg-ps.md) article.</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-back-end-vms"></a><span data-ttu-id="25c4e-117">Skapa hello backend-virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="25c4e-117">Create hello back-end VMs</span></span>
<span data-ttu-id="25c4e-118">hello beror backend-VMs på hello skapande av hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="25c4e-118">hello back-end VMs depend on hello creation of hello following resources:</span></span>

* <span data-ttu-id="25c4e-119">**Backend-undernät**.</span><span class="sxs-lookup"><span data-stu-id="25c4e-119">**Backend subnet**.</span></span> <span data-ttu-id="25c4e-120">hello databasservrar ska ingå i ett separat undernät, toosegregate trafik.</span><span class="sxs-lookup"><span data-stu-id="25c4e-120">hello database servers will be part of a separate subnet, toosegregate traffic.</span></span> <span data-ttu-id="25c4e-121">hello-skriptet nedan kommer det här undernätet tooexist i ett vnet med namnet *WTestVnet*.</span><span class="sxs-lookup"><span data-stu-id="25c4e-121">hello script below expects this subnet tooexist in a vnet named *WTestVnet*.</span></span>
* <span data-ttu-id="25c4e-122">**Storage-konto för datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="25c4e-122">**Storage account for data disks**.</span></span> <span data-ttu-id="25c4e-123">För bättre prestanda använder hello datadiskar på hello databasservrar Solid-State-hårddisk (SSD)-teknik som kräver ett premiumlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="25c4e-123">For better performance, hello data disks on hello database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="25c4e-124">Se till att hello Azure-plats som du distribuerar toosupport premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="25c4e-124">Make sure hello Azure location you deploy toosupport premium storage.</span></span>
* <span data-ttu-id="25c4e-125">**Tillgänglighetsuppsättningen**.</span><span class="sxs-lookup"><span data-stu-id="25c4e-125">**Availability set**.</span></span> <span data-ttu-id="25c4e-126">Alla databasservrar läggs tooa enda tillgänglighetsuppsättning, tooensure minst en av hello virtuella datorer är igång och körs under underhåll.</span><span class="sxs-lookup"><span data-stu-id="25c4e-126">All database servers will be added tooa single availability set, tooensure at least one of hello VMs is up and running during maintenance.</span></span>

### <a name="step-1---start-your-script"></a><span data-ttu-id="25c4e-127">Steg 1 – starta skriptet</span><span class="sxs-lookup"><span data-stu-id="25c4e-127">Step 1 - Start your script</span></span>
<span data-ttu-id="25c4e-128">Du kan hämta hello fullständig PowerShell-skript används [här](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1).</span><span class="sxs-lookup"><span data-stu-id="25c4e-128">You can download hello full PowerShell script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1).</span></span> <span data-ttu-id="25c4e-129">Följ hello steg nedan toochange hello skriptet toowork i din miljö.</span><span class="sxs-lookup"><span data-stu-id="25c4e-129">Follow hello steps below toochange hello script toowork in your environment.</span></span>

1. <span data-ttu-id="25c4e-130">Ändra hello värdena för hello variabler nedan baserat på en befintlig resursgrupp distribuerade ovan i [krav](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="25c4e-130">Change hello values of hello variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```powershell
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    ```

2. <span data-ttu-id="25c4e-131">Ändra hello värden hello variabler nedan baserat på hello värden som du vill toouse för backend-distribution.</span><span class="sxs-lookup"><span data-stu-id="25c4e-131">Change hello values of hello variables below based on hello values you want toouse for your backend deployment.</span></span>

    ```powershell
    $backendCSName         = "IaaSStory-Backend"
    $prmStorageAccountName = "iaasstoryprmstorage"
    $avSetName             = "ASDB"
    $vmSize                = "Standard_DS3"
    $diskSize              = 127
    $vmNamePrefix          = "DB"
    $dataDiskSuffix        = "datadisk"
    $ipAddressPrefix       = "192.168.2."
    $numberOfVMs           = 2
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="25c4e-132">Steg 2 – skapa nödvändiga resurser för din virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="25c4e-132">Step 2 - Create necessary resources for your VMs</span></span>
<span data-ttu-id="25c4e-133">Du måste toocreate en ny molntjänst och en lagringsplats för kontot för hello datadiskar för alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="25c4e-133">You need toocreate a new cloud service and a storage account for hello data disks for all VMs.</span></span> <span data-ttu-id="25c4e-134">Du måste också toospecify en bild och ett lokalt administratörskonto för hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="25c4e-134">You also need toospecify an image, and a local administrator account for hello VMs.</span></span> <span data-ttu-id="25c4e-135">toocreate dessa resurser, Slutför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="25c4e-135">toocreate these resources, complete hello following steps:</span></span>

1. <span data-ttu-id="25c4e-136">Skapa en ny molntjänst.</span><span class="sxs-lookup"><span data-stu-id="25c4e-136">Create a new cloud service.</span></span>

    ```powershell
    New-AzureService -ServiceName $backendCSName -Location $location
    ```

2. <span data-ttu-id="25c4e-137">Skapa ett nytt premium-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="25c4e-137">Create a new premium storage account.</span></span>

    ```powershell
    New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
    -Location $location -Type Premium_LRS
    ```
3. <span data-ttu-id="25c4e-138">Ange hello lagringskonto skapade ovan som hello aktuella lagringskontot för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="25c4e-138">Set hello storage account created above as hello current storage account for your subscription.</span></span>

    ```powershell
    $subscription = Get-AzureSubscription | where {$_.IsCurrent -eq $true}  
    Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
    -CurrentStorageAccountName $prmStorageAccountName
    ```

4. <span data-ttu-id="25c4e-139">Välj en bild för hello VM.</span><span class="sxs-lookup"><span data-stu-id="25c4e-139">Select an image for hello VM.</span></span>

    ```powershell
    $image = Get-AzureVMImage `
    | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
    | sort PublishedDate -Descending `
    | select -ExpandProperty ImageName -First 1
    ```

5. <span data-ttu-id="25c4e-140">Ange autentiseringsuppgifter för hello lokala administratörskontot.</span><span class="sxs-lookup"><span data-stu-id="25c4e-140">Set hello local administrator account credentials.</span></span>

    ```powershell
    $cred = Get-Credential -Message "Enter username and password for local admin account"
    ```

### <a name="step-3---create-vms"></a><span data-ttu-id="25c4e-141">Steg 3 – skapa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="25c4e-141">Step 3 - Create VMs</span></span>
<span data-ttu-id="25c4e-142">Du måste toouse toocreate en loop som många virtuella datorer som du vill och skapa hello nödvändiga nätverkskort och virtuella datorer i hello loop.</span><span class="sxs-lookup"><span data-stu-id="25c4e-142">You need toouse a loop toocreate as many VMs as you want, and create hello necessary NICs and VMs within hello loop.</span></span> <span data-ttu-id="25c4e-143">toocreate hello nätverkskort och virtuella datorer kan du köra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="25c4e-143">toocreate hello NICs and VMs, execute hello following steps.</span></span>

1. <span data-ttu-id="25c4e-144">Starta en `for` loop toorepeat hello kommandon toocreate en virtuell dator och två nätverkskort så många gånger som behövs, beroende på hello värde hello `$numberOfVMs` variabeln.</span><span class="sxs-lookup"><span data-stu-id="25c4e-144">Start a `for` loop toorepeat hello commands toocreate a VM and two NICs as many times as necessary, based on hello value of hello `$numberOfVMs` variable.</span></span>

    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. <span data-ttu-id="25c4e-145">Skapa en `VMConfig` -objekt som anger hello bild, storlek och tillgänglighetsuppsättning för hello VM.</span><span class="sxs-lookup"><span data-stu-id="25c4e-145">Create a `VMConfig` object specifying hello image, size, and availability set for hello VM.</span></span>

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureVMConfig -Name $vmName `
        -ImageName $image `
        -InstanceSize $vmSize `
        -AvailabilitySetName $avSetName
    ```

3. <span data-ttu-id="25c4e-146">Etablera hello VM som en Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="25c4e-146">Provision hello VM as a Windows VM.</span></span>

    ```powershell
    Add-AzureProvisioningConfig -VM $vmConfig -Windows `
        -AdminUsername $cred.UserName `
        -Password $cred.GetNetworkCredential().Password
    ```

4. <span data-ttu-id="25c4e-147">Ange hello standard nätverkskort och tilldela den en statisk IP-adress.</span><span class="sxs-lookup"><span data-stu-id="25c4e-147">Set hello default NIC and assign it a static IP address.</span></span>

    ```powershell
    Set-AzureSubnet         -SubnetNames $backendSubnetName -VM $vmConfig
    Set-AzureStaticVNetIP   -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig
    ```

5. <span data-ttu-id="25c4e-148">Lägga till ett andra nätverkskort för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="25c4e-148">Add a second NIC for each VM.</span></span>

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
    -SubnetName $backendSubnetName `
    -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
    -VM $vmConfig
    ```

6. <span data-ttu-id="25c4e-149">Skapa toodata diskar för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="25c4e-149">Create toodata disks for each VM.</span></span>

    ```powershell
    $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk1Name `
    -LUN 0

    $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk2Name `
    -LUN 1
    ```

7. <span data-ttu-id="25c4e-150">Skapa varje virtuell dator och avsluta hello loop.</span><span class="sxs-lookup"><span data-stu-id="25c4e-150">Create each VM, and end hello loop.</span></span>

    ```powershell
    New-AzureVM -VM $vmConfig `
    -ServiceName $backendCSName `
    -Location $location `
    -VNetName $vnetName
    }
    ```

### <a name="step-4---run-hello-script"></a><span data-ttu-id="25c4e-151">Steg 4 – kör hello skript</span><span class="sxs-lookup"><span data-stu-id="25c4e-151">Step 4 - Run hello script</span></span>
<span data-ttu-id="25c4e-152">Nu när du hämtade och ändrade utifrån hello skript dina behov, runt han skript toocreate hello serverdel databasen virtuella datorer med flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="25c4e-152">Now that you downloaded and changed hello script based on your needs, runt he script toocreate hello back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="25c4e-153">Spara skriptet och kör det från hello **PowerShell** Kommandotolken eller **PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="25c4e-153">Save your script and run it from hello **PowerShell** command prompt, or **PowerShell ISE**.</span></span> <span data-ttu-id="25c4e-154">Hello inledande utdata visas enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="25c4e-154">You will see hello initial output, as shown below.</span></span>

        OperationDescription    OperationId                          OperationStatus

        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        
        WARNING: No deployment found in service: 'IaaSStory-Backend'.
2. <span data-ttu-id="25c4e-155">Fyll i hello information som behövs i hello autentiseringsuppgifter efterfrågas och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="25c4e-155">Fill out hello information needed in hello credentials prompt and click **OK**.</span></span> <span data-ttu-id="25c4e-156">hello utdata nedan returneras.</span><span class="sxs-lookup"><span data-stu-id="25c4e-156">hello output below is returned.</span></span>

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
