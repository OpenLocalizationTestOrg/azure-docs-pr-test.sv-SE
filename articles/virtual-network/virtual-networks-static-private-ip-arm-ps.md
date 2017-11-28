---
title: "aaaConfigure privata IP-adresser för virtuella datorer – Azure PowerShell | Microsoft Docs"
description: "Lär dig hur tooconfigure privata IP-adresser för virtuella datorer med hjälp av PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: d5f18929-15e3-40a2-9ee3-8188bc248ed8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4a3eb67de583e08208fcab40de1c2a8a9b65618c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-powershell"></a><span data-ttu-id="d1b87-103">Konfigurera privat IP-adresser för en virtuell dator med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="d1b87-103">Configure private IP addresses for a virtual machine using PowerShell</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

<span data-ttu-id="d1b87-104">Azure har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="d1b87-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="d1b87-105">Microsoft rekommenderar att skapa resurser via hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="d1b87-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="d1b87-106">Mer om toolearn hello skillnaderna mellan hello två modeller, läsa hello [förstå Azure distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="d1b87-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span> <span data-ttu-id="d1b87-107">Den här artikeln beskriver hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="d1b87-107">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="d1b87-108">Du kan också [hantera statisk privat IP-adress i hello klassiska distributionsmodellen](virtual-networks-static-private-ip-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="d1b87-108">You can also [manage static private IP address in hello classic deployment model](virtual-networks-static-private-ip-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="d1b87-109">hello exempel PowerShell-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="d1b87-109">hello sample PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="d1b87-110">Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö som beskrivs i [skapa ett vnet](virtual-networks-create-vnet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="d1b87-110">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-arm-ps.md).</span></span>

## <a name="create-a-vm-with-a-static-private-ip-address"></a><span data-ttu-id="d1b87-111">Skapa en virtuell dator med en statisk privat IP-adress</span><span class="sxs-lookup"><span data-stu-id="d1b87-111">Create a VM with a static private IP address</span></span>
<span data-ttu-id="d1b87-112">toocreate en virtuell dator med namnet *DNS01* i hello *klientdel* undernätet i ett VNet med namnet *TestVNet* med en statisk privat IP-adress för *192.168.1.101*, Följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="d1b87-112">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow hello steps below:</span></span>

1. <span data-ttu-id="d1b87-113">Ange variabler för hello lagringskonto, plats, resursgruppen och autentiseringsuppgifter toobe används.</span><span class="sxs-lookup"><span data-stu-id="d1b87-113">Set variables for hello storage account, location, resource group, and credentials toobe used.</span></span> <span data-ttu-id="d1b87-114">Du behöver tooenter ett användarnamn och lösenord för hello VM.</span><span class="sxs-lookup"><span data-stu-id="d1b87-114">You will need tooenter a user name and password for hello VM.</span></span> <span data-ttu-id="d1b87-115">hello konto- och lagringsgrupp måste redan finnas.</span><span class="sxs-lookup"><span data-stu-id="d1b87-115">hello storage account and resource group must already exist.</span></span>

    ```powershell
    $stName  = "vnetstorage"
    $locName = "Central US"
    $rgName  = "TestRG"
    $cred    = Get-Credential -Message "Type hello name and password of hello local administrator account."
    ```

2. <span data-ttu-id="d1b87-116">Hämta hello virtuellt nätverk och undernät som du vill toocreate hello VM i.</span><span class="sxs-lookup"><span data-stu-id="d1b87-116">Retrieve hello virtual network and subnet you want toocreate hello VM in.</span></span>

    ```powershell
    $vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    $subnet = $vnet.Subnets[0].Id
    ```

3. <span data-ttu-id="d1b87-117">Om det behövs kan du skapa en offentlig IP-adress tooaccess hello VM från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="d1b87-117">If necessary, create a public IP address tooaccess hello VM from hello Internet.</span></span>

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name TestPIP -ResourceGroupName $rgName `
    -Location $locName -AllocationMethod Dynamic
    ```

4. <span data-ttu-id="d1b87-118">Skapa ett nätverkskort med hello statisk privat IP-adress du vill tooassign toohello VM.</span><span class="sxs-lookup"><span data-stu-id="d1b87-118">Create a NIC using hello static private IP address you want tooassign toohello VM.</span></span> <span data-ttu-id="d1b87-119">Kontrollera att hello IP är från hello undernätsintervall som du lägger till hello VM till.</span><span class="sxs-lookup"><span data-stu-id="d1b87-119">Make sure hello IP is from hello subnet range you are adding hello VM to.</span></span> <span data-ttu-id="d1b87-120">Detta är hello huvudsakliga steg för den här artikeln, där du ange hello privata IP-toobe statisk.</span><span class="sxs-lookup"><span data-stu-id="d1b87-120">This is hello main step for this article, where you set hello private IP toobe static.</span></span>

    ```powershell
    $nic = New-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName $rgName `
    -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id `
    -PrivateIpAddress 192.168.1.101
    ```

5. <span data-ttu-id="d1b87-121">Skapa hello skapas den virtuella datorn med hjälp av hello NIC ovan.</span><span class="sxs-lookup"><span data-stu-id="d1b87-121">Create hello VM using hello NIC created above.</span></span>

    ```powershell
    $vm = New-AzureRmVMConfig -VMName DNS01 -VMSize "Standard_A1"
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName DNS01 `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/WindowsVMosDisk.vhd"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name "windowsvmosdisk" -VhdUri $osDiskUri `
    -CreateOption fromImage
    New-AzureRmVM -ResourceGroupName $rgName -Location $locName -VM $vm 
    ```

    <span data-ttu-id="d1b87-122">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="d1b87-122">Expected output:</span></span>
    
        EndTime             : [Date and time]
        Error               : 
        Output              : 
        StartTime           : [Date and time]
        Status              : Succeeded
        TrackingOperationId : [Id]
        RequestId           : [Id]
        StatusCode          : OK 

## <a name="retrieve-static-private-ip-address-information-for-a-network-interface"></a><span data-ttu-id="d1b87-123">Hämta statisk privat IP-adressinformation för ett nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="d1b87-123">Retrieve static private IP address information for a network interface</span></span>
<span data-ttu-id="d1b87-124">tooview hello statisk privat IP-information för hello skapas den virtuella datorn med hello skriptet ovan, kör följande PowerShell-kommando hello och se hello värden för *PrivateIpAddress* och  *PrivateIpAllocationMethod*:</span><span class="sxs-lookup"><span data-stu-id="d1b87-124">tooview hello static private IP address information for hello VM created with hello script above, run hello following PowerShell command and observe hello values for *PrivateIpAddress* and *PrivateIpAllocationMethod*:</span></span>

```powershell
Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
```

<span data-ttu-id="d1b87-125">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="d1b87-125">Expected output:</span></span>

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    Etag                 : W/"[Id]"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"[Id]\"",
                               "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Static",
                               "Subnet": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="remove-a-static-private-ip-address-from-a-network-interface"></a><span data-ttu-id="d1b87-126">Ta bort en statisk privat IP-adress från ett nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="d1b87-126">Remove a static private IP address from a network interface</span></span>
<span data-ttu-id="d1b87-127">tooremove hello statisk privat IP-adress läggs toohello VM i hello skriptet ovan, kör hello följande PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="d1b87-127">tooremove hello static private IP address added toohello VM in hello script above, run hello following PowerShell commands:</span></span>

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Dynamic"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="d1b87-128">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="d1b87-128">Expected output:</span></span>

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    Etag                 : W/"[Id]"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/WindowsVM"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"[Id]\"",
                               "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Dynamic",
                               "Subnet": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="add-a-static-private-ip-address-tooa-network-interface"></a><span data-ttu-id="d1b87-129">Lägg till en statisk privat IP-adress tooa nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="d1b87-129">Add a static private IP address tooa network interface</span></span>
<span data-ttu-id="d1b87-130">tooadd en statisk privat IP-adress toohello VM som skapats med hjälp av hello skriptet ovan, kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="d1b87-130">tooadd a static private IP address toohello VM created using hello script above, run hello following commands:</span></span>

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Static"
$nic.IpConfigurations[0].PrivateIpAddress = "192.168.1.101"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```
## <a name="change-hello-allocation-method-for-a-private-ip-address-assigned-tooa-network-interface"></a><span data-ttu-id="d1b87-131">Ändra hello allokeringsmetod för en privat IP-adress som tilldelats tooa nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="d1b87-131">Change hello allocation method for a private IP address assigned tooa network interface</span></span>

<span data-ttu-id="d1b87-132">En privat IP-adress har tilldelats tooa NIC med hello statisk eller dynamisk tilldelningsmetod.</span><span class="sxs-lookup"><span data-stu-id="d1b87-132">A private IP address is assigned tooa NIC with hello static or dynamic allocation method.</span></span> <span data-ttu-id="d1b87-133">Dynamiska IP-adresser kan ändra när du startar en virtuell dator som tidigare var i hello stoppats (frigjorts) tillstånd.</span><span class="sxs-lookup"><span data-stu-id="d1b87-133">Dynamic IP addresses can change after starting a VM that was previously in hello stopped (deallocated) state.</span></span> <span data-ttu-id="d1b87-134">Detta kan orsaka problem om hello VM är värd för en tjänst som kräver hello samma IP-adress, även efter omstart från en stoppats (frigjorts).</span><span class="sxs-lookup"><span data-stu-id="d1b87-134">This can potentially cause issues if hello VM is hosting a service that requires hello same IP address, even after restarts from a stopped (deallocated) state.</span></span> <span data-ttu-id="d1b87-135">Statiska IP-adresser behålls tills hello VM tas bort.</span><span class="sxs-lookup"><span data-stu-id="d1b87-135">Static IP addresses are retained until hello VM is deleted.</span></span> <span data-ttu-id="d1b87-136">toochange hello allokeringsmetod för en IP-adress, kör följande skript som ändras hello allokeringsmetod från dynamisk toostatic hello.</span><span class="sxs-lookup"><span data-stu-id="d1b87-136">toochange hello allocation method of an IP address, run hello following script, which changes hello allocation method from dynamic toostatic.</span></span> <span data-ttu-id="d1b87-137">Om hello allokeringsmetod för hello aktuella privata IP-adressen är statisk, ändra *Statiska* för*dynamiska* innan du kör hello skript.</span><span class="sxs-lookup"><span data-stu-id="d1b87-137">If hello allocation method for hello current private IP address is static, change *Static* too*Dynamic* before executing hello script.</span></span>

```powershell
$RG = "TestRG"
$NIC_name = "testnic1"

$nic = Get-AzureRmNetworkInterface -ResourceGroupName $RG -Name $NIC_name
$nic.IpConfigurations[0].PrivateIpAllocationMethod = 'Static'
Set-AzureRmNetworkInterface -NetworkInterface $nic 
$IP = $nic.IpConfigurations[0].PrivateIpAddress

Write-Host "hello allocation method is now set to"$nic.IpConfigurations[0].PrivateIpAllocationMethod"for hello IP address" $IP"." -NoNewline
```

<span data-ttu-id="d1b87-138">Om du inte vet hello namnet på hello NIC kan visa du en lista över nätverkskort inom en resursgrupp genom att ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d1b87-138">If you don't know hello name of hello NIC, you can view a list of NICs within a resource group by entering hello following command:</span></span>

```powershell
Get-AzureRmNetworkInterface -ResourceGroupName $RG | Where-Object {$_.ProvisioningState -eq 'Succeeded'} 
```

## <a name="next-steps"></a><span data-ttu-id="d1b87-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d1b87-139">Next steps</span></span>
* <span data-ttu-id="d1b87-140">Lär dig mer om [reserverade offentliga IP-Adressen](virtual-networks-reserved-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="d1b87-140">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="d1b87-141">Lär dig mer om [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="d1b87-141">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="d1b87-142">Kontakta hello [reserverade IP-REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="d1b87-142">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

