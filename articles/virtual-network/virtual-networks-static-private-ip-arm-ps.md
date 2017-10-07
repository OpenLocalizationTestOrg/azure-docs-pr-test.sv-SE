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
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-powershell"></a>Konfigurera privat IP-adresser för en virtuell dator med hjälp av PowerShell

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

Azure har två distributionsmodeller: Azure Resource Manager och klassisk. Microsoft rekommenderar att skapa resurser via hello Resource Manager-modellen. Mer om toolearn hello skillnaderna mellan hello två modeller, läsa hello [förstå Azure distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md) artikel. Den här artikeln beskriver hello Resource Manager-modellen. Du kan också [hantera statisk privat IP-adress i hello klassiska distributionsmodellen](virtual-networks-static-private-ip-classic-ps.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

hello exempel PowerShell-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan. Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö som beskrivs i [skapa ett vnet](virtual-networks-create-vnet-arm-ps.md).

## <a name="create-a-vm-with-a-static-private-ip-address"></a>Skapa en virtuell dator med en statisk privat IP-adress
toocreate en virtuell dator med namnet *DNS01* i hello *klientdel* undernätet i ett VNet med namnet *TestVNet* med en statisk privat IP-adress för *192.168.1.101*, Följ hello stegen nedan:

1. Ange variabler för hello lagringskonto, plats, resursgruppen och autentiseringsuppgifter toobe används. Du behöver tooenter ett användarnamn och lösenord för hello VM. hello konto- och lagringsgrupp måste redan finnas.

    ```powershell
    $stName  = "vnetstorage"
    $locName = "Central US"
    $rgName  = "TestRG"
    $cred    = Get-Credential -Message "Type hello name and password of hello local administrator account."
    ```

2. Hämta hello virtuellt nätverk och undernät som du vill toocreate hello VM i.

    ```powershell
    $vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    $subnet = $vnet.Subnets[0].Id
    ```

3. Om det behövs kan du skapa en offentlig IP-adress tooaccess hello VM från hello Internet.

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name TestPIP -ResourceGroupName $rgName `
    -Location $locName -AllocationMethod Dynamic
    ```

4. Skapa ett nätverkskort med hello statisk privat IP-adress du vill tooassign toohello VM. Kontrollera att hello IP är från hello undernätsintervall som du lägger till hello VM till. Detta är hello huvudsakliga steg för den här artikeln, där du ange hello privata IP-toobe statisk.

    ```powershell
    $nic = New-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName $rgName `
    -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id `
    -PrivateIpAddress 192.168.1.101
    ```

5. Skapa hello skapas den virtuella datorn med hjälp av hello NIC ovan.

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

    Förväntad utdata:
    
        EndTime             : [Date and time]
        Error               : 
        Output              : 
        StartTime           : [Date and time]
        Status              : Succeeded
        TrackingOperationId : [Id]
        RequestId           : [Id]
        StatusCode          : OK 

## <a name="retrieve-static-private-ip-address-information-for-a-network-interface"></a>Hämta statisk privat IP-adressinformation för ett nätverksgränssnitt
tooview hello statisk privat IP-information för hello skapas den virtuella datorn med hello skriptet ovan, kör följande PowerShell-kommando hello och se hello värden för *PrivateIpAddress* och  *PrivateIpAllocationMethod*:

```powershell
Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
```

Förväntad utdata:

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

## <a name="remove-a-static-private-ip-address-from-a-network-interface"></a>Ta bort en statisk privat IP-adress från ett nätverksgränssnitt
tooremove hello statisk privat IP-adress läggs toohello VM i hello skriptet ovan, kör hello följande PowerShell-kommandon:

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Dynamic"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Förväntad utdata:

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

## <a name="add-a-static-private-ip-address-tooa-network-interface"></a>Lägg till en statisk privat IP-adress tooa nätverksgränssnittet
tooadd en statisk privat IP-adress toohello VM som skapats med hjälp av hello skriptet ovan, kör hello följande kommandon:

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Static"
$nic.IpConfigurations[0].PrivateIpAddress = "192.168.1.101"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```
## <a name="change-hello-allocation-method-for-a-private-ip-address-assigned-tooa-network-interface"></a>Ändra hello allokeringsmetod för en privat IP-adress som tilldelats tooa nätverksgränssnittet

En privat IP-adress har tilldelats tooa NIC med hello statisk eller dynamisk tilldelningsmetod. Dynamiska IP-adresser kan ändra när du startar en virtuell dator som tidigare var i hello stoppats (frigjorts) tillstånd. Detta kan orsaka problem om hello VM är värd för en tjänst som kräver hello samma IP-adress, även efter omstart från en stoppats (frigjorts). Statiska IP-adresser behålls tills hello VM tas bort. toochange hello allokeringsmetod för en IP-adress, kör följande skript som ändras hello allokeringsmetod från dynamisk toostatic hello. Om hello allokeringsmetod för hello aktuella privata IP-adressen är statisk, ändra *Statiska* för*dynamiska* innan du kör hello skript.

```powershell
$RG = "TestRG"
$NIC_name = "testnic1"

$nic = Get-AzureRmNetworkInterface -ResourceGroupName $RG -Name $NIC_name
$nic.IpConfigurations[0].PrivateIpAllocationMethod = 'Static'
Set-AzureRmNetworkInterface -NetworkInterface $nic 
$IP = $nic.IpConfigurations[0].PrivateIpAddress

Write-Host "hello allocation method is now set to"$nic.IpConfigurations[0].PrivateIpAllocationMethod"for hello IP address" $IP"." -NoNewline
```

Om du inte vet hello namnet på hello NIC kan visa du en lista över nätverkskort inom en resursgrupp genom att ange hello följande kommando:

```powershell
Get-AzureRmNetworkInterface -ResourceGroupName $RG | Where-Object {$_.ProvisioningState -eq 'Succeeded'} 
```

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [reserverade offentliga IP-Adressen](virtual-networks-reserved-public-ip.md) adresser.
* Lär dig mer om [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) adresser.
* Kontakta hello [reserverade IP-REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx).

