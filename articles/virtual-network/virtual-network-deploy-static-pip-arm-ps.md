---
title: aaaCreate en virtuell dator med en statisk offentlig IP-adress - Azure PowerShell | Microsoft Docs
description: "Lär dig hur toocreate en virtuell dator med en statisk offentlig IP-adress med hjälp av PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ad975ab9-d69f-45c1-9e45-0d3f0f51e87e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d2b88319cb114b8616f60dbee41e8fdc6d8b1b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-powershell"></a><span data-ttu-id="13155-103">Skapa en virtuell dator med en statisk offentlig IP-adress med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="13155-103">Create a VM with a static public IP address using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="13155-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="13155-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="13155-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="13155-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="13155-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="13155-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="13155-107">Mall</span><span class="sxs-lookup"><span data-stu-id="13155-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="13155-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="13155-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="13155-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="13155-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="13155-110">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="13155-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="step-1---start-your-script"></a><span data-ttu-id="13155-111">Steg 1 – starta skriptet</span><span class="sxs-lookup"><span data-stu-id="13155-111">Step 1 - Start your script</span></span>
<span data-ttu-id="13155-112">Du kan hämta hello fullständig PowerShell-skript används [här](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-ps.ps1).</span><span class="sxs-lookup"><span data-stu-id="13155-112">You can download hello full PowerShell script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-ps.ps1).</span></span> <span data-ttu-id="13155-113">Följ hello steg nedan toochange hello skriptet toowork i din miljö.</span><span class="sxs-lookup"><span data-stu-id="13155-113">Follow hello steps below toochange hello script toowork in your environment.</span></span>

<span data-ttu-id="13155-114">Ändra hello värden hello variabler nedan baserat på hello värden som du vill toouse för din distribution.</span><span class="sxs-lookup"><span data-stu-id="13155-114">Change hello values of hello variables below based on hello values you want toouse for your deployment.</span></span> <span data-ttu-id="13155-115">hello följande värden kartan toohello scenario används i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="13155-115">hello following values map toohello scenario used in this article:</span></span>

```powershell
# Set variables resource group
$rgName                = "IaaSStory"
$location              = "West US"

# Set variables for VNet
$vnetName              = "WTestVNet"
$vnetPrefix            = "192.168.0.0/16"
$subnetName            = "FrontEnd"
$subnetPrefix          = "192.168.1.0/24"

# Set variables for storage
$stdStorageAccountName = "iaasstorystorage"

# Set variables for VM
$vmSize                = "Standard_A1"
$diskSize              = 127
$publisher             = "MicrosoftWindowsServer"
$offer                 = "WindowsServer"
$sku                   = "2012-R2-Datacenter"
$version               = "latest"
$vmName                = "WEB1"
$osDiskName            = "osdisk"
$nicName               = "NICWEB1"
$privateIPAddress      = "192.168.1.101"
$pipName               = "PIPWEB1"
$dnsName               = "iaasstoryws1"
```

## <a name="step-2---create-hello-necessary-resources-for-your-vm"></a><span data-ttu-id="13155-116">Steg 2 – Skapa hello nödvändiga resurser för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="13155-116">Step 2 - Create hello necessary resources for your VM</span></span>
<span data-ttu-id="13155-117">Innan du skapar en virtuell dator, måste en resursgrupp, virtuella nätverk, offentliga IP- och NIC toobe som används av hello VM.</span><span class="sxs-lookup"><span data-stu-id="13155-117">Before creating a VM, you need a resource group, VNet, public IP, and NIC toobe used by hello VM.</span></span>

1. <span data-ttu-id="13155-118">Skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="13155-118">Create a new resource group.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name $rgName -Location $location
    ```

2. <span data-ttu-id="13155-119">Skapa hello VNet och undernät.</span><span class="sxs-lookup"><span data-stu-id="13155-119">Create hello VNet and subnet.</span></span>

    ```powershell
    $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName `
        -AddressPrefix $vnetPrefix -Location $location

    Add-AzureRmVirtualNetworkSubnetConfig -Name $subnetName `
        -VirtualNetwork $vnet -AddressPrefix $subnetPrefix

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

3. <span data-ttu-id="13155-120">Skapa hello offentliga IP-resurs.</span><span class="sxs-lookup"><span data-stu-id="13155-120">Create hello public IP resource.</span></span> 

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName `
        -AllocationMethod Static -DomainNameLabel $dnsName -Location $location
    ```

4. <span data-ttu-id="13155-121">Skapa hello nätverksgränssnitt (NIC) för hello VM i hello undernät ovanstående med hello offentlig IP.</span><span class="sxs-lookup"><span data-stu-id="13155-121">Create hello network interface (NIC) for hello VM in hello subnet created above, with hello public IP.</span></span> <span data-ttu-id="13155-122">Lägg märke till hello första cmdlet hämtar hello virtuella nätverk från Azure, detta är nödvändigt eftersom en `Set-AzureRmVirtualNetwork` har utförd toochange hello befintliga VNet.</span><span class="sxs-lookup"><span data-stu-id="13155-122">Notice hello first cmdlet retrieving hello VNet from Azure, this is necessary since a `Set-AzureRmVirtualNetwork` was executed toochange hello existing VNet.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
        -Subnet $subnet -Location $location -PrivateIpAddress $privateIPAddress `
        -PublicIpAddress $pip
    ```

5. <span data-ttu-id="13155-123">Skapa en storage-konto toohost hello VM OS-enhet.</span><span class="sxs-lookup"><span data-stu-id="13155-123">Create a storage account toohost hello VM OS drive.</span></span>

    ```powershell
    $stdStorageAccount = New-AzureRmStorageAccount -Name $stdStorageAccountName `
    -ResourceGroupName $rgName -Type Standard_LRS -Location $location
    ```

## <a name="step-3---create-hello-vm"></a><span data-ttu-id="13155-124">Steg 3 – skapa hello VM</span><span class="sxs-lookup"><span data-stu-id="13155-124">Step 3 - Create hello VM</span></span>
<span data-ttu-id="13155-125">Nu när alla nödvändiga resurser är på plats kan skapa du en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="13155-125">Now that all necessary resources are in place, you can create a new VM.</span></span>

1. <span data-ttu-id="13155-126">Skapa hello konfigurationsobjektet för hello VM.</span><span class="sxs-lookup"><span data-stu-id="13155-126">Create hello configuration object for hello VM.</span></span>

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    ```

2. <span data-ttu-id="13155-127">Hämta autentiseringsuppgifter för hello VM lokala administratörskontot.</span><span class="sxs-lookup"><span data-stu-id="13155-127">Get credentials for hello VM local administrator account.</span></span>

    ```powershell
    $cred = Get-Credential -Message "Type hello name and password for hello local administrator account."
    ```

3. <span data-ttu-id="13155-128">Skapa ett konfigurationsobjekt för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="13155-128">Create a VM configuration object.</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```

4. <span data-ttu-id="13155-129">Ange hello operativsystemsavbildning för hello VM.</span><span class="sxs-lookup"><span data-stu-id="13155-129">Set hello operating system image for hello VM.</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher `
        -Offer $offer -Skus $sku -Version $version
    ```

5. <span data-ttu-id="13155-130">Konfigurera hello OS-disk.</span><span class="sxs-lookup"><span data-stu-id="13155-130">Configure hello OS disk.</span></span>

    ```powershell
    $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
    $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
    ```

6. <span data-ttu-id="13155-131">Lägg till hello NIC toohello VM.</span><span class="sxs-lookup"><span data-stu-id="13155-131">Add hello NIC toohello VM.</span></span>

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id -Primary
    ```

7. <span data-ttu-id="13155-132">Skapa hello VM.</span><span class="sxs-lookup"><span data-stu-id="13155-132">Create hello VM.</span></span>

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName $rgName -Location $location
    ```

8. <span data-ttu-id="13155-133">Spara hello skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="13155-133">Save hello script file.</span></span>

## <a name="step-4---run-hello-script"></a><span data-ttu-id="13155-134">Steg 4 – kör hello skript</span><span class="sxs-lookup"><span data-stu-id="13155-134">Step 4 - Run hello script</span></span>
<span data-ttu-id="13155-135">Visa ovan, kör hello skript efter göra nödvändiga ändringar och förstå hello skript.</span><span class="sxs-lookup"><span data-stu-id="13155-135">After making any necessary changes, and understanding hello script show above, run hello script.</span></span> 

1. <span data-ttu-id="13155-136">Från PowerShell-konsolen eller PowerShell ISE, kör du hello skriptet ovan.</span><span class="sxs-lookup"><span data-stu-id="13155-136">From a PowerShell console, or PowerShell ISE, run hello script above.</span></span>
2. <span data-ttu-id="13155-137">hello följande utdata ska visas efter några minuter:</span><span class="sxs-lookup"><span data-stu-id="13155-137">hello following output should be displayed after a few minutes:</span></span>
   
        ResourceGroupName : IaaSStory
        Location          : westus
        ProvisioningState : Succeeded
        Tags              : 
        ResourceId        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory
   
        AddressSpace      : Microsoft.Azure.Commands.Network.Models.PSAddressSpace
        DhcpOptions       : Microsoft.Azure.Commands.Network.Models.PSDhcpOptions
        Subnets           : {FrontEnd}
        ProvisioningState : Succeeded
        AddressSpaceText  : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptionsText   : {}
        SubnetsText       : [
                              {
                                "Name": "FrontEnd",
                                "AddressPrefix": "192.168.1.0/24"
                              }
                            ]
        ResourceGroupName : IaaSStory
        Location          : westus
        ResourceGuid      : [Id]
        Tag               : {}
        TagsTable         : 
        Name              : WTestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        Id                : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet
   
        AddressSpace      : Microsoft.Azure.Commands.Network.Models.PSAddressSpace
        DhcpOptions       : Microsoft.Azure.Commands.Network.Models.PSDhcpOptions
        Subnets           : {FrontEnd}
        ProvisioningState : Succeeded
        AddressSpaceText  : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptionsText   : {
                              "DnsServers": []
                            }
        SubnetsText       : [
                              {
                                "Name": "FrontEnd",
                                "Etag": [Id],
                                "Id": "/subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "ProvisioningState": "Succeeded"
                              }
                            ]
        ResourceGroupName : IaaSStory
        Location          : westus
        ResourceGuid      : [Id]
        Tag               : {}
        TagsTable         : 
        Name              : WTestVNet
        Etag              : [Id]
        Id                : /subscriptions/[Subscription Id]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet
   
        TrackingOperationId : [Id]
        RequestId           : [Id]
        Status              : Succeeded
        StatusCode          : OK
        Output              : 
        StartTime           : [Subscription Id]
        EndTime             : [Subscription Id]
        Error               : 
        ErrorText           : 

