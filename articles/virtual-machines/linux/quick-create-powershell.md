---
title: aaaAzure Snabbstart - skapa VM PowerShell | Microsoft Docs
description: "Lär dig snabbt toocreate en Linux virtuella datorer med PowerShell"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: f05ea7fedafe4fda660dc6084ae57ebf9dced473
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-powershell"></a>Skapa en virtuell Linux-dator med PowerShell

hello Azure PowerShell-modulen är används toocreate och hantera Azure-resurser från hello PowerShell-Kommandotolken eller i skript. Den här guiden information med hjälp av hello Azure PowerShell-modulen toodeploy en virtuell dator som kör Ubuntu server. När hello server har distribuerats, en SSH-anslutning skapas och en NGINX-webbserver är installerad.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

Denna Snabbstart kräver hello Azure PowerShell module 3,6 eller senare. Kör ` Get-Module -ListAvailable AzureRM` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).

Slutligen en offentlig SSH-nyckel med namnet hello *id_rsa.pub* måste lagras i hello toobe *.ssh* på din Windows-användarprofil. Mer detaljerad information om hur du skapar SSH-nycklar för Azure finns i [Create SSH keys for Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Skapa SSH-nycklar för Azure).

## <a name="log-in-tooazure"></a>Logga in tooAzure

Logga in tooyour Azure-prenumeration med hello `Login-AzureRmAccount` kommando och följ hello på skärmen riktningar.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Skapa resursgrupp

Skapa en Azure-resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
```

## <a name="create-networking-resources"></a>Skapa nätverksresurser

Skapa ett virtuellt nätverk, undernät och offentlig IP-adress. Dessa resurser är används tooprovide network connectivity toohello virtuell dator och koppla den toohello internet.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location eastus `
-Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location eastus `
-AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

Skapa en nätverkssäkerhetsgrupp och en regel för nätverkssäkerhetsgrupp. Hej nätverkssäkerhetsgruppen säkrar hello virtuell dator med regler för inkommande och utgående. I detta fall skapas en regel för inkommande trafik för port 22, som tillåter inkommande SSH-anslutningar. Vi vill även toocreate en regel för inkommande trafik för port 80, vilket gör att inkommande webbtrafik.

```powershell
# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleSSH  -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location eastus `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

Skapa ett nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) för hello virtuella datorn. hello nätverkskortet ansluter hello virtuella tooa undernät, nätverkssäkerhetsgrupp och offentliga IP-adressen.

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location eastus `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a>Skapa en virtuell dator

Skapa en virtuell datorkonfiguration. Den här konfigurationen innehåller hello-inställningar som används när du distribuerar hello virtuell dator till exempel en bild, storlek och autentisering konfiguration av virtuell dator.

```powershell
# Define a credential object
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName myVM -Credential $cred -DisablePasswordAuthentication | `
Set-AzureRmVMSourceImage -PublisherName Canonical -Offer UbuntuServer -Skus 14.04.2-LTS -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"
```

Skapa hello virtuell dator med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location eastus -VM $vmConfig
```

## <a name="connect-toovirtual-machine"></a>Ansluta toovirtual datorn

Skapa en SSH-anslutning med hello virtuella datorn när hello distributionen har slutförts.

Använd hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) kommandot tooreturn hello offentliga IP-adressen för hello virtuell dator.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Använda hello följande kommando från ett system med SSH installerat tooconnect toohello virtuella datorn. Om du arbetar med Windows, [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) kan vara används toocreate hello anslutning. 

```bash 
ssh <Public IP Address>
```

När du uppmanas hello användarnamn är *azureuser*. Om en lösenfras angavs när du skapar SSH-nycklar måste det här samt tooenter.


## <a name="install-nginx"></a>Installera NGINX

Använd hello följande bash skriptet tooupdate paketet källor och installera hello senaste NGINX-paketet. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-hello-ngix-welcome-page"></a>Visa hello NGIX välkomstsidan

Du kan använda en webbläsare för dina val tooview hello NGINX välkomstsidan med NGINX installerat och port 80 som nu är öppet på den virtuella datorn från hello Internet. Vara säker på att toouse hello offentliga IP-adressen du dokumenterade över toovisit hello standardsida. 

![NGINX-standardwebbplats](./media/quick-create-cli/nginx.png) 

## <a name="clean-up-resources"></a>Rensa resurser

När du inte längre behövs kan du använda hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kommandot tooremove hello resursgrupp, virtuell dator och alla relaterade resurser.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten har du distribuerat en virtuell dator och en regel för nätverkssäkerhetsgrupp samt installerat en webbserver. toolearn mer om Azure-datorer, fortsätta toohello självstudier för Linux virtuella datorer.

> [!div class="nextstepaction"]
> [Självstudier om virtuella Azure Linux-datorer](./tutorial-manage-vm.md)
