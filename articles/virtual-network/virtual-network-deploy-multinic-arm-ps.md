---
title: "aaaCreate en virtuell dator med flera nätverkskort – Azure PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en virtuell dator med flera nätverkskort med hjälp av PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 88880483-8f9e-4eeb-b783-64b8613407d9
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 507a413510da3ee69aefed324977ee40e442268b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-powershell"></a><span data-ttu-id="7ca75-103">Skapa en virtuell dator med flera nätverkskort med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="7ca75-103">Create a VM with multiple NICs using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7ca75-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7ca75-104">PowerShell</span></span>](virtual-network-deploy-multinic-arm-ps.md)
> * [<span data-ttu-id="7ca75-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7ca75-105">Azure CLI</span></span>](virtual-network-deploy-multinic-arm-cli.md)
> * [<span data-ttu-id="7ca75-106">Mall</span><span class="sxs-lookup"><span data-stu-id="7ca75-106">Template</span></span>](virtual-network-deploy-multinic-arm-template.md)
> * [<span data-ttu-id="7ca75-107">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="7ca75-107">PowerShell (Classic)</span></span>](virtual-network-deploy-multinic-classic-ps.md)
> * [<span data-ttu-id="7ca75-108">Azure CLI (klassisk)</span><span class="sxs-lookup"><span data-stu-id="7ca75-108">Azure CLI (Classic)</span></span>](virtual-network-deploy-multinic-classic-cli.md)

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="7ca75-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7ca75-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="7ca75-110">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="7ca75-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](virtual-network-deploy-multinic-classic-ps.md).</span></span>
>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="7ca75-111">hello så här använder du en resursgrupp med namnet *IaaSStory* för hello webbservrar och en resursgrupp med namnet *IaaSStory BackEnd* för hello DB-servrar.</span><span class="sxs-lookup"><span data-stu-id="7ca75-111">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7ca75-112">Krav</span><span class="sxs-lookup"><span data-stu-id="7ca75-112">Prerequisites</span></span>
<span data-ttu-id="7ca75-113">Innan du kan skapa hello DB-servrar, behöver du toocreate hello *IaaSStory* resursgrupp med alla hello nödvändiga resurser för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="7ca75-113">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="7ca75-114">toocreate dessa resurser, Slutför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7ca75-114">toocreate these resources, complete hello following steps:</span></span>

1. <span data-ttu-id="7ca75-115">Navigera för[hello mallsidan](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="7ca75-115">Navigate too[hello template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="7ca75-116">Hello mallen sidan toohello höger i **överordnade resursgruppen**, klickar du på **distribuera tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="7ca75-116">In hello template page, toohello right of **Parent resource group**, click **Deploy tooAzure**.</span></span>
3. <span data-ttu-id="7ca75-117">Om det behövs, ändra hello parametervärden sedan gör hello i hello Azure preview portal toodeploy hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7ca75-117">If needed, change hello parameter values, then follow hello steps in hello Azure preview portal toodeploy hello resource group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ca75-118">Kontrollera att din lagringskontonamn är unika.</span><span class="sxs-lookup"><span data-stu-id="7ca75-118">Make sure your storage account names are unique.</span></span> <span data-ttu-id="7ca75-119">Du kan inte ha dubbla lagringskontonamn i Azure.</span><span class="sxs-lookup"><span data-stu-id="7ca75-119">You cannot have duplicate storage account names in Azure.</span></span>
> 

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-back-end-vms"></a><span data-ttu-id="7ca75-120">Skapa hello backend-virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="7ca75-120">Create hello back-end VMs</span></span>
<span data-ttu-id="7ca75-121">hello beror backend-VMs på hello skapande av hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="7ca75-121">hello back-end VMs depend on hello creation of hello following resources:</span></span>

* <span data-ttu-id="7ca75-122">**Storage-konto för datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="7ca75-122">**Storage account for data disks**.</span></span> <span data-ttu-id="7ca75-123">För bättre prestanda använder hello datadiskar på hello databasservrar Solid-State-hårddisk (SSD)-teknik som kräver ett premiumlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7ca75-123">For better performance, hello data disks on hello database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="7ca75-124">Se till att hello Azure-plats som du distribuerar toosupport premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="7ca75-124">Make sure hello Azure location you deploy toosupport premium storage.</span></span>
* <span data-ttu-id="7ca75-125">**Nätverkskort**.</span><span class="sxs-lookup"><span data-stu-id="7ca75-125">**NICs**.</span></span> <span data-ttu-id="7ca75-126">Varje virtuell dator har två nätverkskort, ett för åtkomst till databasen, och en för hantering.</span><span class="sxs-lookup"><span data-stu-id="7ca75-126">Each VM will have two NICs, one for database access, and one for management.</span></span>
* <span data-ttu-id="7ca75-127">**Tillgänglighetsuppsättningen**.</span><span class="sxs-lookup"><span data-stu-id="7ca75-127">**Availability set**.</span></span> <span data-ttu-id="7ca75-128">Alla databasservrar läggs tooa enda tillgänglighetsuppsättning, tooensure minst en av hello virtuella datorer är igång och körs under underhåll.</span><span class="sxs-lookup"><span data-stu-id="7ca75-128">All database servers will be added tooa single availability set, tooensure at least one of hello VMs is up and running during maintenance.</span></span>  

### <a name="step-1---start-your-script"></a><span data-ttu-id="7ca75-129">Steg 1 – starta skriptet</span><span class="sxs-lookup"><span data-stu-id="7ca75-129">Step 1 - Start your script</span></span>
<span data-ttu-id="7ca75-130">Du kan hämta hello fullständig PowerShell-skript används [här](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1).</span><span class="sxs-lookup"><span data-stu-id="7ca75-130">You can download hello full PowerShell script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1).</span></span> <span data-ttu-id="7ca75-131">Följ hello steg nedan toochange hello skriptet toowork i din miljö.</span><span class="sxs-lookup"><span data-stu-id="7ca75-131">Follow hello steps below toochange hello script toowork in your environment.</span></span>

1. <span data-ttu-id="7ca75-132">Ändra hello värdena för hello variabler nedan baserat på en befintlig resursgrupp distribuerade ovan i [krav](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="7ca75-132">Change hello values of hello variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```powershell
    $existingRGName        = "IaaSStory"
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    $remoteAccessNSGName   = "NSG-RemoteAccess"
    $stdStorageAccountName = "wtestvnetstoragestd"
    ```

2. <span data-ttu-id="7ca75-133">Ändra hello värden hello variabler nedan baserat på hello värden som du vill toouse för backend-distribution.</span><span class="sxs-lookup"><span data-stu-id="7ca75-133">Change hello values of hello variables below based on hello values you want toouse for your backend deployment.</span></span>

    ```powershell
    $backendRGName         = "IaaSStory-Backend"
    $prmStorageAccountName = "wtestvnetstorageprm"
    $avSetName             = "ASDB"
    $vmSize                = "Standard_DS3"
    $publisher             = "MicrosoftSQLServer"
    $offer                 = "SQL2014SP1-WS2012R2"
    $sku                   = "Standard"
    $version               = "latest"
    $vmNamePrefix          = "DB"
    $osDiskPrefix          = "osdiskdb"
    $dataDiskPrefix        = "datadisk"
    $diskSize               = "120"    
    $nicNamePrefix         = "NICDB"
    $ipAddressPrefix       = "192.168.2."
    $numberOfVMs           = 2
    ```
3. <span data-ttu-id="7ca75-134">Hämta hello befintliga resurser som krävs för din distribution.</span><span class="sxs-lookup"><span data-stu-id="7ca75-134">Retrieve hello existing resources needed for your deployment.</span></span>

    ```powershell
    $vnet                  = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $existingRGName
    $backendSubnet         = $vnet.Subnets|?{$_.Name -eq $backendSubnetName}
    $remoteAccessNSG       = Get-AzureRmNetworkSecurityGroup -Name $remoteAccessNSGName -ResourceGroupName $existingRGName
    $stdStorageAccount     = Get-AzureRmStorageAccount -Name $stdStorageAccountName -ResourceGroupName $existingRGName
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="7ca75-135">Steg 2 – skapa nödvändiga resurser för din virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="7ca75-135">Step 2 - Create necessary resources for your VMs</span></span>
<span data-ttu-id="7ca75-136">Behöver du toocreate en ny resursgrupp i ett lagringskonto för hello datadiskar och en tillgänglighetsuppsättning för alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7ca75-136">You need toocreate a new resource group, a storage account for hello data disks, and an availability set for all VMs.</span></span> <span data-ttu-id="7ca75-137">Alos måste hello autentiseringsuppgifter för lokal administratör för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7ca75-137">You alos need hello local administrator account credentials for each VM.</span></span> <span data-ttu-id="7ca75-138">toocreate dessa resurser, köra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="7ca75-138">toocreate these resources, execute hello following steps.</span></span>

1. <span data-ttu-id="7ca75-139">Skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7ca75-139">Create a new resource group.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name $backendRGName -Location $location
    ```
2. <span data-ttu-id="7ca75-140">Skapa ett nytt premium-lagringskonto i hello resursgruppen skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="7ca75-140">Create a new premium storage account in hello resource group created above.</span></span>

    ```powershell
    $prmStorageAccount = New-AzureRmStorageAccount -Name $prmStorageAccountName `
    -ResourceGroupName $backendRGName -Type Premium_LRS -Location $location
    ```
3. <span data-ttu-id="7ca75-141">Skapa en ny tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="7ca75-141">Create a new availability set.</span></span>

    ```powershell
    $avSet = New-AzureRmAvailabilitySet -Name $avSetName -ResourceGroupName $backendRGName -Location $location
    ```
4. <span data-ttu-id="7ca75-142">Hämta hello lokal administratör konto autentiseringsuppgifter toobe används för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7ca75-142">Get hello local administrator account credentials toobe used for each VM.</span></span>

    ```powershell
    $cred = Get-Credential -Message "Type hello name and password for hello local administrator account."
    ```

### <a name="step-3---create-hello-nics-and-back-end-vms"></a><span data-ttu-id="7ca75-143">Steg 3 – skapa hello nätverkskort och backend-virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="7ca75-143">Step 3 - Create hello NICs and back-end VMs</span></span>
<span data-ttu-id="7ca75-144">Du måste toouse toocreate en loop som många virtuella datorer som du vill och skapa hello nödvändiga nätverkskort och virtuella datorer i hello loop.</span><span class="sxs-lookup"><span data-stu-id="7ca75-144">You need toouse a loop toocreate as many VMs as you want, and create hello necessary NICs and VMs within hello loop.</span></span> <span data-ttu-id="7ca75-145">toocreate hello nätverkskort och virtuella datorer kan du köra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="7ca75-145">toocreate hello NICs and VMs, execute hello following steps.</span></span>

1. <span data-ttu-id="7ca75-146">Starta en `for` loop toorepeat hello kommandon toocreate en virtuell dator och två nätverkskort så många gånger som behövs, beroende på hello värde hello `$numberOfVMs` variabeln.</span><span class="sxs-lookup"><span data-stu-id="7ca75-146">Start a `for` loop toorepeat hello commands toocreate a VM and two NICs as many times as necessary, based on hello value of hello `$numberOfVMs` variable.</span></span>
   
    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. <span data-ttu-id="7ca75-147">Skapa hello nätverkskort används för åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="7ca75-147">Create hello NIC used for database access.</span></span>

    ```powershell
    $nic1Name = $nicNamePrefix + $suffixNumber + "-DA"
    $ipAddress1 = $ipAddressPrefix + ($suffixNumber + 3)
    $nic1 = New-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $backendRGName `
    -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress1
    ```

3. <span data-ttu-id="7ca75-148">Skapa hello nätverkskort används för fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="7ca75-148">Create hello NIC used for remote access.</span></span> <span data-ttu-id="7ca75-149">Observera hur det här nätverkskortet har en NSG som kopplats tooit.</span><span class="sxs-lookup"><span data-stu-id="7ca75-149">Notice how this NIC has an NSG associated tooit.</span></span>

    ```powershell
    $nic2Name = $nicNamePrefix + $suffixNumber + "-RA"
    $ipAddress2 = $ipAddressPrefix + (53 + $suffixNumber)
    $nic2 = New-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $backendRGName `
    -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress2 `
    -NetworkSecurityGroupId $remoteAccessNSG.Id
    ```

4. <span data-ttu-id="7ca75-150">Skapa `vmConfig` objekt.</span><span class="sxs-lookup"><span data-stu-id="7ca75-150">Create `vmConfig` object.</span></span>

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id
    ```

5. <span data-ttu-id="7ca75-151">Skapa två hårddiskar per virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7ca75-151">Create two data disks per VM.</span></span> <span data-ttu-id="7ca75-152">Lägg märke till att hello datadiskar hello premiumlagringskonto skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="7ca75-152">Notice that hello data disks are in hello premium storage account created earlier.</span></span>

    ```powershell
    $dataDisk1Name = $vmName + "-" + $osDiskPrefix + "-1"
    $data1VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk1Name + ".vhd"
    Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk1Name -DiskSizeInGB $diskSize `
    -VhdUri $data1VhdUri -CreateOption empty -Lun 0

    $dataDisk2Name = $vmName + "-" + $dataDiskPrefix + "-2"
    $data2VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk2Name + ".vhd"
    Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk2Name -DiskSizeInGB $diskSize `
    -VhdUri $data2VhdUri -CreateOption empty -Lun 1
    ```

6. <span data-ttu-id="7ca75-153">Konfigurera hello operativsystem och avbildning toobe används för hello VM.</span><span class="sxs-lookup"><span data-stu-id="7ca75-153">Configure hello operating system, and image toobe used for hello VM.</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher -Offer $offer -Skus $sku -Version $version
    ```

7. <span data-ttu-id="7ca75-154">Lägga till hello två nätverkskort som skapade ovan toohello `vmConfig` objekt.</span><span class="sxs-lookup"><span data-stu-id="7ca75-154">Add hello two NICs created above toohello `vmConfig` object.</span></span>

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic2.Id
    ```

8. <span data-ttu-id="7ca75-155">Skapa hello OS-disken och skapa hello VM.</span><span class="sxs-lookup"><span data-stu-id="7ca75-155">Create hello OS disk and create hello VM.</span></span> <span data-ttu-id="7ca75-156">Meddelande hello `}` slutar hello `for` loop.</span><span class="sxs-lookup"><span data-stu-id="7ca75-156">Notice hello `}` ending hello `for` loop.</span></span>

    ```powershell
    $osDiskName = $vmName + "-" + $osDiskSuffix
    $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
    $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
    New-AzureRmVM -VM $vmConfig -ResourceGroupName $backendRGName -Location $location
    }
    ```

### <a name="step-4---run-hello-script"></a><span data-ttu-id="7ca75-157">Steg 4 – kör hello skript</span><span class="sxs-lookup"><span data-stu-id="7ca75-157">Step 4 - Run hello script</span></span>
<span data-ttu-id="7ca75-158">Nu när du hämtade och ändrade utifrån hello skript dina behov, runt han skript toocreate hello serverdel databasen virtuella datorer med flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="7ca75-158">Now that you downloaded and changed hello script based on your needs, runt he script toocreate hello back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="7ca75-159">Spara skriptet och kör det från hello **PowerShell** Kommandotolken eller **PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="7ca75-159">Save your script and run it from hello **PowerShell** command prompt, or **PowerShell ISE**.</span></span> <span data-ttu-id="7ca75-160">Visas hello inledande utgående, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="7ca75-160">You will see hello initial output, as follows:</span></span>

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
            Actions  NotActions
            =======  ==========
                *                  

        ResourceId        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend

2. <span data-ttu-id="7ca75-161">Efter några minuter att fylla i hello autentiseringsuppgifter efterfrågas och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ca75-161">After a few minutes, fill out hello credentials prompt and click **OK**.</span></span> <span data-ttu-id="7ca75-162">hello utdata nedan representerar en enda virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7ca75-162">hello output below represents a single VM.</span></span> <span data-ttu-id="7ca75-163">Meddelande hello hela processen tog 8 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="7ca75-163">Notice hello entire process took 8 minutes toocomplete.</span></span>

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText :  {
                                    "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Compute/availabilitySets/ASDB"
                                    }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                        "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           },
                                           {
                                             "Lun": 1,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-2",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-2.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1, DB2-datadisk-2}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0
        EndTime             : [Date] [Time]
        Error               :
        Output              :
        StartTime           : [Date] [Time]
        Status              : Succeeded
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        StatusCode          : OK
