---
title: "Konfigurera privata IP-adresser för virtuella datorer – Azure PowerShell | Microsoft Docs"
description: "Lär dig hur du konfigurerar den privata IP-adresser för virtuella datorer med hjälp av PowerShell."
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
ms.openlocfilehash: 2810190897c44c944912ef3325b1f40479aa3078
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-powershell"></a><span data-ttu-id="dea38-103">Konfigurera privat IP-adresser för en virtuell dator med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="dea38-103">Configure private IP addresses for a virtual machine using PowerShell</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

<span data-ttu-id="dea38-104">Azure har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="dea38-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="dea38-105">Microsoft rekommenderar att skapa resurser med Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="dea38-105">Microsoft recommends creating resources through the Resource Manager deployment model.</span></span> <span data-ttu-id="dea38-106">Mer information om skillnaderna mellan de två modellerna finns i artikeln [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) (Förstå Azure-distributionsmodellerna).</span><span class="sxs-lookup"><span data-stu-id="dea38-106">To learn more about the differences between the two models, read the [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span> <span data-ttu-id="dea38-107">Den här artikeln beskriver Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="dea38-107">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="dea38-108">Du kan också [hantera statisk privat IP-adress i den klassiska distributionsmodellen](virtual-networks-static-private-ip-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="dea38-108">You can also [manage static private IP address in the classic deployment model](virtual-networks-static-private-ip-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="dea38-109">Exempel PowerShell-kommandona nedan förväntar sig en enkel miljö som redan har skapats baserat på scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="dea38-109">The sample PowerShell commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="dea38-110">Om du vill köra kommandon som de visas i det här dokumentet, först skapa testmiljön som beskrivs i [skapa ett vnet](virtual-networks-create-vnet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="dea38-110">If you want to run the commands as they are displayed in this document, first build the test environment described in [create a vnet](virtual-networks-create-vnet-arm-ps.md).</span></span>

## <a name="create-a-vm-with-a-static-private-ip-address"></a><span data-ttu-id="dea38-111">Skapa en virtuell dator med en statisk privat IP-adress</span><span class="sxs-lookup"><span data-stu-id="dea38-111">Create a VM with a static private IP address</span></span>
<span data-ttu-id="dea38-112">Skapa en virtuell dator med namnet *DNS01* i den *klientdel* undernätet i ett VNet med namnet *TestVNet* med en statisk privat IP-adress för *192.168.1.101*, Följ stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="dea38-112">To create a VM named *DNS01* in the *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow the steps below:</span></span>

1. <span data-ttu-id="dea38-113">Ange variabler för lagringskonto, plats, resursgruppen och autentiseringsuppgifter som ska användas.</span><span class="sxs-lookup"><span data-stu-id="dea38-113">Set variables for the storage account, location, resource group, and credentials to be used.</span></span> <span data-ttu-id="dea38-114">Du måste ange ett användarnamn och lösenord för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="dea38-114">You will need to enter a user name and password for the VM.</span></span> <span data-ttu-id="dea38-115">Konto- och lagringsgruppen måste redan finnas.</span><span class="sxs-lookup"><span data-stu-id="dea38-115">The storage account and resource group must already exist.</span></span>

    ```powershell
    $stName  = "vnetstorage"
    $locName = "Central US"
    $rgName  = "TestRG"
    $cred    = Get-Credential -Message "Type the name and password of the local administrator account."
    ```

2. <span data-ttu-id="dea38-116">Hämta det virtuella nätverk och undernät som du vill skapa den virtuella datorn i.</span><span class="sxs-lookup"><span data-stu-id="dea38-116">Retrieve the virtual network and subnet you want to create the VM in.</span></span>

    ```powershell
    $vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    $subnet = $vnet.Subnets[0].Id
    ```

3. <span data-ttu-id="dea38-117">Vid behov kan du skapa en offentlig IP-adress för att komma åt den virtuella datorn från Internet.</span><span class="sxs-lookup"><span data-stu-id="dea38-117">If necessary, create a public IP address to access the VM from the Internet.</span></span>

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name TestPIP -ResourceGroupName $rgName `
    -Location $locName -AllocationMethod Dynamic
    ```

4. <span data-ttu-id="dea38-118">Skapa ett nätverkskort med statisk privat IP-adress du vill tilldela till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="dea38-118">Create a NIC using the static private IP address you want to assign to the VM.</span></span> <span data-ttu-id="dea38-119">Kontrollera IP-Adressen i undernätet intervallet som du lägger till den virtuella datorn till.</span><span class="sxs-lookup"><span data-stu-id="dea38-119">Make sure the IP is from the subnet range you are adding the VM to.</span></span> <span data-ttu-id="dea38-120">Detta är det viktigaste steget för den här artikeln, där du kan ange privata IP-Adressen ska vara statisk.</span><span class="sxs-lookup"><span data-stu-id="dea38-120">This is the main step for this article, where you set the private IP to be static.</span></span>

    ```powershell
    $nic = New-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName $rgName `
    -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id `
    -PrivateIpAddress 192.168.1.101
    ```

5. <span data-ttu-id="dea38-121">Skapa den virtuella datorn med hjälp av NIC skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="dea38-121">Create the VM using the NIC created above.</span></span>

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

    <span data-ttu-id="dea38-122">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="dea38-122">Expected output:</span></span>
    
        EndTime             : [Date and time]
        Error               : 
        Output              : 
        StartTime           : [Date and time]
        Status              : Succeeded
        TrackingOperationId : [Id]
        RequestId           : [Id]
        StatusCode          : OK 

## <a name="retrieve-static-private-ip-address-information-for-a-network-interface"></a><span data-ttu-id="dea38-123">Hämta statisk privat IP-adressinformation för ett nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="dea38-123">Retrieve static private IP address information for a network interface</span></span>
<span data-ttu-id="dea38-124">Kör följande PowerShell-kommando för att visa statisk privat IP-adressinformation för den virtuella datorn skapas med skriptet ovan, och notera att värdena för *PrivateIpAddress* och *PrivateIpAllocationMethod*:</span><span class="sxs-lookup"><span data-stu-id="dea38-124">To view the static private IP address information for the VM created with the script above, run the following PowerShell command and observe the values for *PrivateIpAddress* and *PrivateIpAllocationMethod*:</span></span>

```powershell
Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
```

<span data-ttu-id="dea38-125">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="dea38-125">Expected output:</span></span>

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

## <a name="remove-a-static-private-ip-address-from-a-network-interface"></a><span data-ttu-id="dea38-126">Ta bort en statisk privat IP-adress från ett nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="dea38-126">Remove a static private IP address from a network interface</span></span>
<span data-ttu-id="dea38-127">Ta bort statisk privat IP-adress till den virtuella datorn i skriptet ovan, kör följande PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="dea38-127">To remove the static private IP address added to the VM in the script above, run the following PowerShell commands:</span></span>

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Dynamic"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="dea38-128">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="dea38-128">Expected output:</span></span>

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

## <a name="add-a-static-private-ip-address-to-a-network-interface"></a><span data-ttu-id="dea38-129">Lägg till en statisk privat IP-adress till ett nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="dea38-129">Add a static private IP address to a network interface</span></span>
<span data-ttu-id="dea38-130">Kör följande kommandon för att lägga till en statisk privat IP-adress till den virtuella datorn skapas med skriptet ovan:</span><span class="sxs-lookup"><span data-stu-id="dea38-130">To add a static private IP address to the VM created using the script above, run the following commands:</span></span>

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Static"
$nic.IpConfigurations[0].PrivateIpAddress = "192.168.1.101"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```
## <a name="change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface"></a><span data-ttu-id="dea38-131">Ändra allokeringsmetod för en privat IP-adress till ett nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="dea38-131">Change the allocation method for a private IP address assigned to a network interface</span></span>

<span data-ttu-id="dea38-132">En privat IP-adress har tilldelats ett nätverkskort med statisk eller dynamisk tilldelningsmetod.</span><span class="sxs-lookup"><span data-stu-id="dea38-132">A private IP address is assigned to a NIC with the static or dynamic allocation method.</span></span> <span data-ttu-id="dea38-133">Dynamiska IP-adresser kan ändra när du har startat en virtuell dator som tidigare var i tillståndet Stoppad (frigjord).</span><span class="sxs-lookup"><span data-stu-id="dea38-133">Dynamic IP addresses can change after starting a VM that was previously in the stopped (deallocated) state.</span></span> <span data-ttu-id="dea38-134">Detta kan orsaka problem om den virtuella datorn är värd för en tjänst som behöver samma IP-adress, även efter omstart från en stoppats (frigjorts).</span><span class="sxs-lookup"><span data-stu-id="dea38-134">This can potentially cause issues if the VM is hosting a service that requires the same IP address, even after restarts from a stopped (deallocated) state.</span></span> <span data-ttu-id="dea38-135">Statiska IP-adresser behålls tills den virtuella datorn tas bort.</span><span class="sxs-lookup"><span data-stu-id="dea38-135">Static IP addresses are retained until the VM is deleted.</span></span> <span data-ttu-id="dea38-136">Om du vill ändra allokeringsmetod för en IP-adress, kör du följande skript, som ändrar allokeringsmetoden från dynamisk till statisk.</span><span class="sxs-lookup"><span data-stu-id="dea38-136">To change the allocation method of an IP address, run the following script, which changes the allocation method from dynamic to static.</span></span> <span data-ttu-id="dea38-137">Om allokeringsmetoden för den aktuella privata IP-adressen är statisk, ändra *Statiska* till *dynamiska* innan du kör skriptet.</span><span class="sxs-lookup"><span data-stu-id="dea38-137">If the allocation method for the current private IP address is static, change *Static* to *Dynamic* before executing the script.</span></span>

```powershell
$RG = "TestRG"
$NIC_name = "testnic1"

$nic = Get-AzureRmNetworkInterface -ResourceGroupName $RG -Name $NIC_name
$nic.IpConfigurations[0].PrivateIpAllocationMethod = 'Static'
Set-AzureRmNetworkInterface -NetworkInterface $nic 
$IP = $nic.IpConfigurations[0].PrivateIpAddress

Write-Host "The allocation method is now set to"$nic.IpConfigurations[0].PrivateIpAllocationMethod"for the IP address" $IP"." -NoNewline
```

<span data-ttu-id="dea38-138">Om du inte vet namnet på nätverkskortet, kan du visa en lista över nätverkskort inom en resursgrupp genom att ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="dea38-138">If you don't know the name of the NIC, you can view a list of NICs within a resource group by entering the following command:</span></span>

```powershell
Get-AzureRmNetworkInterface -ResourceGroupName $RG | Where-Object {$_.ProvisioningState -eq 'Succeeded'} 
```

## <a name="next-steps"></a><span data-ttu-id="dea38-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dea38-139">Next steps</span></span>
* <span data-ttu-id="dea38-140">Lär dig mer om [reserverade offentliga IP-Adressen](virtual-networks-reserved-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="dea38-140">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="dea38-141">Lär dig mer om [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="dea38-141">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="dea38-142">Läs den [reserverade IP-REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="dea38-142">Consult the [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

