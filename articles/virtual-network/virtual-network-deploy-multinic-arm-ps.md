---
title: "Skapa en virtuell dator med flera nätverkskort – Azure PowerShell | Microsoft Docs"
description: "Lär dig hur du skapar en virtuell dator med flera nätverkskort med hjälp av PowerShell."
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
ms.openlocfilehash: f3a11afd8fbd6a5e6b94cf1ebee7ea20665421bd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-multiple-nics-using-powershell"></a><span data-ttu-id="e5dde-103">Skapa en virtuell dator med flera nätverkskort med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="e5dde-103">Create a VM with multiple NICs using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e5dde-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e5dde-104">PowerShell</span></span>](virtual-network-deploy-multinic-arm-ps.md)
> * [<span data-ttu-id="e5dde-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e5dde-105">Azure CLI</span></span>](virtual-network-deploy-multinic-arm-cli.md)
> * [<span data-ttu-id="e5dde-106">Mall</span><span class="sxs-lookup"><span data-stu-id="e5dde-106">Template</span></span>](virtual-network-deploy-multinic-arm-template.md)
> * [<span data-ttu-id="e5dde-107">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="e5dde-107">PowerShell (Classic)</span></span>](virtual-network-deploy-multinic-classic-ps.md)
> * [<span data-ttu-id="e5dde-108">Azure CLI (klassisk)</span><span class="sxs-lookup"><span data-stu-id="e5dde-108">Azure CLI (Classic)</span></span>](virtual-network-deploy-multinic-classic-cli.md)

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="e5dde-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e5dde-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="e5dde-110">Den här artikeln beskriver Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för [den klassiska distributionsmodellen](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="e5dde-110">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](virtual-network-deploy-multinic-classic-ps.md).</span></span>
>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="e5dde-111">Följande steg använder en resursgrupp med namnet *IaaSStory* för webbservrar och en resursgrupp med namnet *IaaSStory BackEnd* för DB-servrar.</span><span class="sxs-lookup"><span data-stu-id="e5dde-111">The following steps use a resource group named *IaaSStory* for the WEB servers and a resource group named *IaaSStory-BackEnd* for the DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e5dde-112">Krav</span><span class="sxs-lookup"><span data-stu-id="e5dde-112">Prerequisites</span></span>
<span data-ttu-id="e5dde-113">Innan du kan skapa DB-servrar, måste du skapa den *IaaSStory* resursgrupp med alla nödvändiga resurser för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="e5dde-113">Before you can create the DB servers, you need to create the *IaaSStory* resource group with all the necessary resources for this scenario.</span></span> <span data-ttu-id="e5dde-114">För att skapa dessa resurser, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="e5dde-114">To create these resources, complete the following steps:</span></span>

1. <span data-ttu-id="e5dde-115">Gå till [på mallsidan](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="e5dde-115">Navigate to [the template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="e5dde-116">På mallsidan till höger om **överordnade resursgruppen**, klickar du på **till Azure**.</span><span class="sxs-lookup"><span data-stu-id="e5dde-116">In the template page, to the right of **Parent resource group**, click **Deploy to Azure**.</span></span>
3. <span data-ttu-id="e5dde-117">Om det behövs, ändra parametervärden och följ stegen i Azure preview portal för att distribuera resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e5dde-117">If needed, change the parameter values, then follow the steps in the Azure preview portal to deploy the resource group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e5dde-118">Kontrollera att din lagringskontonamn är unika.</span><span class="sxs-lookup"><span data-stu-id="e5dde-118">Make sure your storage account names are unique.</span></span> <span data-ttu-id="e5dde-119">Du kan inte ha dubbla lagringskontonamn i Azure.</span><span class="sxs-lookup"><span data-stu-id="e5dde-119">You cannot have duplicate storage account names in Azure.</span></span>
> 

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-back-end-vms"></a><span data-ttu-id="e5dde-120">Skapa de virtuella datorerna serverdel</span><span class="sxs-lookup"><span data-stu-id="e5dde-120">Create the back-end VMs</span></span>
<span data-ttu-id="e5dde-121">Backend-VMs beror på att skapa följande resurser:</span><span class="sxs-lookup"><span data-stu-id="e5dde-121">The back-end VMs depend on the creation of the following resources:</span></span>

* <span data-ttu-id="e5dde-122">**Storage-konto för datadiskar**.</span><span class="sxs-lookup"><span data-stu-id="e5dde-122">**Storage account for data disks**.</span></span> <span data-ttu-id="e5dde-123">För bättre prestanda använder datadiskar på databasservrarna Solid-State-hårddisk (SSD)-teknik som kräver ett premiumlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e5dde-123">For better performance, the data disks on the database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="e5dde-124">Kontrollera att den Azure-plats som du distribuerar för att stödja premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="e5dde-124">Make sure the Azure location you deploy to support premium storage.</span></span>
* <span data-ttu-id="e5dde-125">**Nätverkskort**.</span><span class="sxs-lookup"><span data-stu-id="e5dde-125">**NICs**.</span></span> <span data-ttu-id="e5dde-126">Varje virtuell dator har två nätverkskort, ett för åtkomst till databasen, och en för hantering.</span><span class="sxs-lookup"><span data-stu-id="e5dde-126">Each VM will have two NICs, one for database access, and one for management.</span></span>
* <span data-ttu-id="e5dde-127">**Tillgänglighetsuppsättningen**.</span><span class="sxs-lookup"><span data-stu-id="e5dde-127">**Availability set**.</span></span> <span data-ttu-id="e5dde-128">Alla databasservrar läggs till en enda tillgänglighet ange att se till att minst en av de virtuella datorerna är igång och körs under underhåll.</span><span class="sxs-lookup"><span data-stu-id="e5dde-128">All database servers will be added to a single availability set, to ensure at least one of the VMs is up and running during maintenance.</span></span>  

### <a name="step-1---start-your-script"></a><span data-ttu-id="e5dde-129">Steg 1 – starta skriptet</span><span class="sxs-lookup"><span data-stu-id="e5dde-129">Step 1 - Start your script</span></span>
<span data-ttu-id="e5dde-130">Du kan hämta den fullständiga PowerShell-skript som används [här](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1).</span><span class="sxs-lookup"><span data-stu-id="e5dde-130">You can download the full PowerShell script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1).</span></span> <span data-ttu-id="e5dde-131">Följ stegen nedan för att ändra skriptet fungerar i din miljö.</span><span class="sxs-lookup"><span data-stu-id="e5dde-131">Follow the steps below to change the script to work in your environment.</span></span>

1. <span data-ttu-id="e5dde-132">Ändra värdena för variablerna nedan baserat på en befintlig resursgrupp distribuerade ovan i [krav](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="e5dde-132">Change the values of the variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```powershell
    $existingRGName        = "IaaSStory"
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    $remoteAccessNSGName   = "NSG-RemoteAccess"
    $stdStorageAccountName = "wtestvnetstoragestd"
    ```

2. <span data-ttu-id="e5dde-133">Ändra värdena för variabler nedan baserat på de värden som du vill använda för backend-distribution.</span><span class="sxs-lookup"><span data-stu-id="e5dde-133">Change the values of the variables below based on the values you want to use for your backend deployment.</span></span>

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
3. <span data-ttu-id="e5dde-134">Hämta de befintliga resurser som krävs för din distribution.</span><span class="sxs-lookup"><span data-stu-id="e5dde-134">Retrieve the existing resources needed for your deployment.</span></span>

    ```powershell
    $vnet                  = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $existingRGName
    $backendSubnet         = $vnet.Subnets|?{$_.Name -eq $backendSubnetName}
    $remoteAccessNSG       = Get-AzureRmNetworkSecurityGroup -Name $remoteAccessNSGName -ResourceGroupName $existingRGName
    $stdStorageAccount     = Get-AzureRmStorageAccount -Name $stdStorageAccountName -ResourceGroupName $existingRGName
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="e5dde-135">Steg 2 – skapa nödvändiga resurser för din virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e5dde-135">Step 2 - Create necessary resources for your VMs</span></span>
<span data-ttu-id="e5dde-136">Du måste skapa en ny resursgrupp, ett lagringskonto för datadiskar och en tillgänglighetsuppsättning för alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e5dde-136">You need to create a new resource group, a storage account for the data disks, and an availability set for all VMs.</span></span> <span data-ttu-id="e5dde-137">Alos måste autentiseringsuppgifter för lokala administratörskontot för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5dde-137">You alos need the local administrator account credentials for each VM.</span></span> <span data-ttu-id="e5dde-138">Utför följande steg för att skapa dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="e5dde-138">To create these resources, execute the following steps.</span></span>

1. <span data-ttu-id="e5dde-139">Skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="e5dde-139">Create a new resource group.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name $backendRGName -Location $location
    ```
2. <span data-ttu-id="e5dde-140">Skapa ett nytt premium-lagringskonto i resursgruppen skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="e5dde-140">Create a new premium storage account in the resource group created above.</span></span>

    ```powershell
    $prmStorageAccount = New-AzureRmStorageAccount -Name $prmStorageAccountName `
    -ResourceGroupName $backendRGName -Type Premium_LRS -Location $location
    ```
3. <span data-ttu-id="e5dde-141">Skapa en ny tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="e5dde-141">Create a new availability set.</span></span>

    ```powershell
    $avSet = New-AzureRmAvailabilitySet -Name $avSetName -ResourceGroupName $backendRGName -Location $location
    ```
4. <span data-ttu-id="e5dde-142">Hämta den lokala administratören kontoautentiseringsuppgifter som ska användas för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5dde-142">Get the local administrator account credentials to be used for each VM.</span></span>

    ```powershell
    $cred = Get-Credential -Message "Type the name and password for the local administrator account."
    ```

### <a name="step-3---create-the-nics-and-back-end-vms"></a><span data-ttu-id="e5dde-143">Steg 3 – skapa nätverkskort och backend-virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e5dde-143">Step 3 - Create the NICs and back-end VMs</span></span>
<span data-ttu-id="e5dde-144">Du måste använda en loop skapa så många virtuella datorer som du vill och skapa de nödvändiga nätverkskort och virtuella datorer inom loopen.</span><span class="sxs-lookup"><span data-stu-id="e5dde-144">You need to use a loop to create as many VMs as you want, and create the necessary NICs and VMs within the loop.</span></span> <span data-ttu-id="e5dde-145">Utför följande steg för att skapa nätverkskort och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e5dde-145">To create the NICs and VMs, execute the following steps.</span></span>

1. <span data-ttu-id="e5dde-146">Starta en `for` slinga om du vill upprepa kommandona för att skapa en virtuell dator och två nätverkskort så många gånger som behövs, baserat på värdet för den `$numberOfVMs` variabeln.</span><span class="sxs-lookup"><span data-stu-id="e5dde-146">Start a `for` loop to repeat the commands to create a VM and two NICs as many times as necessary, based on the value of the `$numberOfVMs` variable.</span></span>
   
    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. <span data-ttu-id="e5dde-147">Skapa det nätverkskort som används för åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="e5dde-147">Create the NIC used for database access.</span></span>

    ```powershell
    $nic1Name = $nicNamePrefix + $suffixNumber + "-DA"
    $ipAddress1 = $ipAddressPrefix + ($suffixNumber + 3)
    $nic1 = New-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $backendRGName `
    -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress1
    ```

3. <span data-ttu-id="e5dde-148">Skapa det nätverkskort som används för fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="e5dde-148">Create the NIC used for remote access.</span></span> <span data-ttu-id="e5dde-149">Observera hur det här nätverkskortet har en NSG som kopplats till den.</span><span class="sxs-lookup"><span data-stu-id="e5dde-149">Notice how this NIC has an NSG associated to it.</span></span>

    ```powershell
    $nic2Name = $nicNamePrefix + $suffixNumber + "-RA"
    $ipAddress2 = $ipAddressPrefix + (53 + $suffixNumber)
    $nic2 = New-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $backendRGName `
    -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress2 `
    -NetworkSecurityGroupId $remoteAccessNSG.Id
    ```

4. <span data-ttu-id="e5dde-150">Skapa `vmConfig` objekt.</span><span class="sxs-lookup"><span data-stu-id="e5dde-150">Create `vmConfig` object.</span></span>

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id
    ```

5. <span data-ttu-id="e5dde-151">Skapa två hårddiskar per virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5dde-151">Create two data disks per VM.</span></span> <span data-ttu-id="e5dde-152">Observera att datadiskar som finns i premium storage-konto som skapades tidigare.</span><span class="sxs-lookup"><span data-stu-id="e5dde-152">Notice that the data disks are in the premium storage account created earlier.</span></span>

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

6. <span data-ttu-id="e5dde-153">Konfigurera operativsystemet och bild som ska användas för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e5dde-153">Configure the operating system, and image to be used for the VM.</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher -Offer $offer -Skus $sku -Version $version
    ```

7. <span data-ttu-id="e5dde-154">Lägg till de två nätverkskorten skapade ovan till den `vmConfig` objekt.</span><span class="sxs-lookup"><span data-stu-id="e5dde-154">Add the two NICs created above to the `vmConfig` object.</span></span>

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic2.Id
    ```

8. <span data-ttu-id="e5dde-155">Skapa OS-disken och skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e5dde-155">Create the OS disk and create the VM.</span></span> <span data-ttu-id="e5dde-156">Observera den `}` slutar den `for` loop.</span><span class="sxs-lookup"><span data-stu-id="e5dde-156">Notice the `}` ending the `for` loop.</span></span>

    ```powershell
    $osDiskName = $vmName + "-" + $osDiskSuffix
    $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
    $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
    New-AzureRmVM -VM $vmConfig -ResourceGroupName $backendRGName -Location $location
    }
    ```

### <a name="step-4---run-the-script"></a><span data-ttu-id="e5dde-157">Steg 4 – kör skriptet</span><span class="sxs-lookup"><span data-stu-id="e5dde-157">Step 4 - Run the script</span></span>
<span data-ttu-id="e5dde-158">Nu när du har hämtat och ändra skriptet baserat på dina behov runt han använda skript för att skapa serverdelen databasen virtuella datorer med flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="e5dde-158">Now that you downloaded and changed the script based on your needs, runt he script to create the back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="e5dde-159">Spara skriptet och kör det från den **PowerShell** Kommandotolken eller **PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="e5dde-159">Save your script and run it from the **PowerShell** command prompt, or **PowerShell ISE**.</span></span> <span data-ttu-id="e5dde-160">Visas första utdata, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="e5dde-160">You will see the initial output, as follows:</span></span>

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
            Actions  NotActions
            =======  ==========
                *                  

        ResourceId        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend

2. <span data-ttu-id="e5dde-161">Efter några minuter att fylla i fråga om autentiseringsuppgifterna och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5dde-161">After a few minutes, fill out the credentials prompt and click **OK**.</span></span> <span data-ttu-id="e5dde-162">Utdata nedan representerar en enda virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5dde-162">The output below represents a single VM.</span></span> <span data-ttu-id="e5dde-163">Observera att hela processen tog 8 minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="e5dde-163">Notice the entire process took 8 minutes to complete.</span></span>

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
