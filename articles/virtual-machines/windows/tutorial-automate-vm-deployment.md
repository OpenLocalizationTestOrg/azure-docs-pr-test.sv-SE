---
title: aaaCustomize en Windows-dator i Azure | Microsoft Docs
description: "Lär dig hur toouse hello tillägget för anpassat skript och Key Vault toocustomize virtuella Windows-datorer i Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c03b2bb6d70875134c63ea2fe4c2e2c1777c2188
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocustomize-a-windows-virtual-machine-in-azure"></a>Hur toocustomize en Windows-dator i Azure
är vanligtvis det önskade tooconfigure virtuella maskiner (VMs) på ett snabbt och konsekvent sätt någon form av automatisering. En gemensam metod toocustomize en Windows VM är toouse [anpassade skript tillägget för Windows](extensions-customscript.md). I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Använd hello tillägget för anpassat skript tooinstall IIS
> * Skapa en virtuell dator som använder hello tillägget för anpassat skript
> * Visa en IIS-webbplats som körs efter hello tillägget används

Den här kursen kräver hello Azure PowerShell module 3,6 eller senare. Kör ` Get-Module -ListAvailable AzureRM` toofind hello version. Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).


## <a name="custom-script-extension-overview"></a>Översikt över tillägget för anpassat skript
hello tillägget för anpassat skript hämtar och kör skript på Azure Virtual Machines. Det här tillägget är användbart för post distributionskonfiguration, Programvaruinstallation eller någon annan konfiguration / hanteringsaktivitet. Skript kan hämtas från Azure storage eller GitHub eller tillhandahålls toohello Azure-portalen på tillägget körtiden.

hello tillägget för anpassat skript kan integreras med Azure Resource Manager-mallar och kan också köras med hello Azure CLI, PowerShell, Azure-portalen eller hello Azure virtuella REST API.

Du kan använda hello tillägget för anpassat skript med Windows och Linux virtuella datorer.


## <a name="create-virtual-machine"></a>Skapa en virtuell dator
Innan du kan skapa en virtuell dator, skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). hello följande exempel skapar en resursgrupp med namnet *myResourceGroupAutomate* i hello *EastUS* plats:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

Ange en administratör användarnamn och lösenord för hello virtuella datorer med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Nu kan du skapa hello virtuell dator med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). hello följande exempel skapar hello krävs virtuella nätverkskomponenter hello OS-konfiguration, och skapar sedan en virtuell dator med namnet *myVM*:

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleWWW  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName myResourceGroupAutomate -Location EastUS -VM $vmConfig
```

Det tar några minuter för hello resurser och VM-toobe skapas.


## <a name="automate-iis-install"></a>Automatisera installationen av IIS
Använd [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello tillägget för anpassat skript. Hej tillägget körs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS-webbserver och uppdateringar hello *Default.htm* sidan tooshow hello värdnamnet för hello VM:

```powershell
Set-AzureRmVMExtension -ResourceGroupName myResourceGroupAutomate `
    -ExtensionName IIS `
    -VMName myVM `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
```


## <a name="test-web-site"></a>Test-webbplats
Hämta hello offentliga IP-adressen för din belastningsutjämnare med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). hello följande exempel hämtar hello IP-adress för *myPublicIP* skapade tidigare:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

Du kan sedan ange hello offentliga IP-adressen i tooa webbläsare. hello webbplatsen visas, inklusive hello värdnamnet för hello VM som hello belastningsutjämnaren distribuerade trafik tooas i följande exempel hello:

![Kör IIS-webbplats](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen automatiserad hello IIS installeras på en virtuell dator. Du har lärt dig att:

> [!div class="checklist"]
> * Använd hello tillägget för anpassat skript tooinstall IIS
> * Skapa en virtuell dator som använder hello tillägget för anpassat skript
> * Visa en IIS-webbplats som körs efter hello tillägget används

I förväg toohello nästa självstudiekurs toolearn hur toocreate anpassade VM-avbildningar.

> [!div class="nextstepaction"]
> [Skapa anpassade avbildningar av en virtuell dator](./tutorial-custom-images.md)
