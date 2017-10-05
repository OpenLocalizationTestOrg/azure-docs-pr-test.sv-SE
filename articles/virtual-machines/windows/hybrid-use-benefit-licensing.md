---
title: "Hybridrapportering i Azure används förmån för Windows Server och Windows-klient | Microsoft Docs"
description: "Lär dig att maximera dina Windows Software Assurance-förmåner för att försätta lokalt licenser i Azure"
services: virtual-machines-windows
documentationcenter: 
author: kmouss
manager: timlt
editor: 
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 5/26/2017
ms.author: xujing
ms.openlocfilehash: 210635624946ddb293427167e9d476c377bcc9b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-hybrid-use-benefit-for-windows-server-and-windows-client"></a>Hybridrapportering i Azure används förmån för Windows Server och Windows-klient
För kunder med Software Assurance kan Azure Hybrid använda förmånen du använda dina lokala Windows Server och Windows-klient licenser och köra virtuella Windows-datorer i Azure till en lägre kostnad. Azure Hybrid Använd förmån för Windows Server innehåller Windows Server 2008R2, Windows Server 2012, Windows Server 2012 R2 och Windows Server 2016. Azure Hybrid Använd förmån för Windows-klienten innehåller Windows 10. Mer information finns i [Azure Hybrid Använd förmånen licensiering sidan](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

>[!IMPORTANT]
>Azure Hybrid Använd fördelar för Windows-klienten är för närvarande under förhandsgranskning med hjälp av Windows 10-avbildning i Azure Marketplace. Bara Enterprise-kunder med Windows 10 Enterprise E3/E5 per användare eller Windows VDA per användare (användare prenumerationslicenser eller tillägg användarlicenser prenumeration) (”kvalificerande licenser”) är tillgängliga.
>
>

## <a name="ways-to-use-azure-hybrid-use-benefit"></a>Sätt att använda Azure Hybrid använda förmånen
Det finns ett par olika sätt att distribuera virtuella Windows-datorer och de fördelar som Azure Hybrid användning:

1. Du kan distribuera virtuella datorer från [specifika Marketplace-bilder](#deploy-a-vm-using-the-azure-marketplace) som är förkonfigurerad med Azure Hybrid Använd förmån - Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 och Windows Server 2008SP1.
2. Du kan [ladda upp en anpassad VM](#upload-a-windows-vhd) och [distribueras med hjälp av en Resource Manager-mall](#deploy-a-vm-via-resource-manager) eller [Azure PowerShell](#detailed-powershell-deployment-walkthrough).

## <a name="deploy-a-vm-using-the-azure-marketplace"></a>Distribuera en virtuell dator med hjälp av Azure Marketplace
Följande bilder är tillgängliga i Marketplace förkonfigurerade med Azure Hybrid Använd förmån: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 och Windows Server 2008SP1. Dessa avbildningar kan distribueras direkt från Azure-portalen, Resource Manager-mallar eller Azure PowerShell.

Du kan distribuera dessa avbildningar direkt från Azure-portalen. För användning i Resource Manager-mallar med Azure PowerShell, visas en lista med bilder på följande sätt:

För Windows Server:
```powershell
Get-AzureRmVMImagesku -Location westus -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```
- 2016 Datacenter version 2016.127.20170406 eller senare

- 2012 R2 Datacenter version 4.127.20170406 eller senare

- 2012 Datacenter version 3.127.20170406 eller senare

- 2008 R2 SP1-version 2.127.20170406 eller senare

För Windows-klient:
```powershell
Get-AzureRMVMImageSku -Location "West US" -Publisher "MicrosoftWindowsServer" `
    -Offer "Windows-HUB"
```

## <a name="upload-a-windows-server-vhd"></a>Överför en Windows Server VHD
Om du vill distribuera en Windows Server-VM i Azure måste du först skapa en virtuell Hårddisk som innehåller din grundläggande Windows-version. Den här virtuella Hårddisken måste förberedas på rätt sätt via Sysprep innan du överför den till Azure. Du kan [Läs mer om kraven för virtuell Hårddisk och Sysprep-processen](upload-generalized-managed.md) och [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Säkerhetskopiera den virtuella datorn innan du kör Sysprep. 

Kontrollera att du har [installerat och konfigurerat den senaste Azure PowerShell](/powershell/azure/overview). När du har skapat den virtuella Hårddisken kan överföra den virtuella Hårddisken till din Azure Storage-konto med den `Add-AzureRmVhd` cmdlet enligt följande:

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> Microsoft SQL Server, SharePoint Server och Dynamics kan också använda din Software Assurance-licensiering. Du måste fortfarande förbereda Windows Server-avbildning genom att installera programkomponenterna och tillhandahåller licensnycklar i enlighet med detta och sedan ladda upp disk image till Azure. Dokumentationen från lämplig kör Sysprep med ditt program, till exempel [överväganden för installation av SQL Server med hjälp av Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) eller [skapa SharePoint Server 2016 referensbild (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).
>
>

Du kan också läsa mer om [överför den virtuella Hårddisken till Azure-processen](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)


## <a name="deploy-a-vm-via-resource-manager-template"></a>Distribuera en virtuell dator via Resource Manager-mall
I Resource Manager-mallar, en ytterligare parameter för `licenseType` kan anges. Du kan läsa mer om [skapa mallar för Azure Resource Manager](../../resource-group-authoring-templates.md). När du har den virtuella Hårddisken överförs till Azure kan redigera Resource Manager-mall om du vill inkludera licenstypen som en del av compute-providern och distribuera mallen som vanligt:

För Windows Server:
```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

För Windows-klienten bara ska använda med Azure Marketplace-avbildning:
```json
"properties": {  
   "licenseType": "Windows_Client",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="deploy-a-vm-via-powershell-quickstart"></a>Distribuera en virtuell dator via PowerShell-Snabbstart
När du distribuerar Windows Server-VM via PowerShell kan du ha en ytterligare parameter för `-LicenseType`. När du har den virtuella Hårddisken överförs till Azure kan du skapa en virtuell dator med hjälp av `New-AzureRmVM` och ange licensiering på följande sätt:

För Windows Server:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

För Windows-klienten bara ska använda med Azure Marketplace-avbildning:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

Du kan [läsa en mer detaljerad genomgång om hur du distribuerar en virtuell dator i Azure via PowerShell](hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) nedan, eller en mer beskrivande guide på olika steg för att läsa [skapa en virtuell Windows-dator med Resource Manager och PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Kontrollera den virtuella datorn använder licensiering fördelen
När du har distribuerat den virtuella datorn via antingen PowerShell eller Resource Manager distributionsmetoden, kontrollera licenstypen med `Get-AzureRmVM` på följande sätt:

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Utdata liknar följande exempel för Windows Server:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Detta utdata skiljer från med följande VM distribueras utan Azure Hybrid Använd förmånen licensiering, till exempel en virtuell dator distribueras direkt från Azure-galleriet:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="detailed-powershell-deployment-walkthrough"></a>Detaljerad genomgång för distribution av PowerShell
I följande steg för detaljerad PowerShell visas en fullständig distribution av en virtuell dator. Du kan läsa mer kontext faktiska cmdlets och olika komponenter som skapas i [skapa en virtuell Windows-dator med Resource Manager och PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Du steg för steg att skapa din resursgrupp, storage-konto och virtuella nätverk, och sedan definiera den virtuella datorn och slutligen skapa den virtuella datorn.

Först på ett säkert sätt hämta autentiseringsuppgifter genom att ange en plats och resursgruppens namn:

```powershell
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "myResourceGroup"
```

Skapa en offentlig IP-adress:

```powershell
$publicIPName = "myPublicIP"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName `
    -Location $location -AllocationMethod "Dynamic"
```

Definiera din undernät, nätverkskort och virtuella nätverk:

```powershell
$subnetName = "mySubnet"
$nicName = "myNIC"
$vnetName = "myVnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location `
    -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Namnge den virtuella datorn och skapa en VM-konfiguration:

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Definiera ditt operativsystem:

```powershell
$computerName = "myVM"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

Lägg till nätverkskortet i den virtuella datorn:

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Definiera lagringskontot för att använda:

```powershell
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName mystorageaccount
```

Överför den virtuella Hårddisken på lämpligt sätt förberedd och ansluta till den virtuella datorn för användning:

```powershell
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/vhd/myvhd.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage `
    -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Slutligen skapar den virtuella datorn och definiera licensiering för att använda Azure Hybrid Använd förmånen:

För Windows Server:
```powershell
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType "Windows_Server"
```

## <a name="deploy-a-virtual-machine-scale-set-via-resource-manager-template"></a>Distribuera en virtuell dator skala in via Resource Manager-mall
Inom en ytterligare parameter för dina VMSS Resource Manager-mallar `licenseType` måste anges. Du kan läsa mer om [skapa mallar för Azure Resource Manager](../../resource-group-authoring-templates.md). Redigera Resource Manager-mall om du vill inkludera egenskapen licenseType som en del av den skaluppsättning virtualMachineProfile och distribuera mallen som vanligt - finns i exemplet nedan med hjälp av Windows Server 2016-avbildning:


```json
"virtualMachineProfile": {
    "storageProfile": {
        "osDisk": {
            "createOption": "FromImage"
        },
        "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2016-Datacenter",
            "version": "latest"
        }
    },
    "licenseType": "Windows_Server",
    "osProfile": {
            "computerNamePrefix": "[parameters('vmssName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
    }
```

> [!NOTE]
> Stöd för att distribuera en virtuell dator-skala med AHUB fördelarna via PowerShell och andra SDK-verktyg kommer snart.
>

## <a name="next-steps"></a>Nästa steg
Läs mer om [Azure Hybrid Använd förmånen licensiering](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Lär dig mer om [med hjälp av Resource Manager-mallar](../../azure-resource-manager/resource-group-overview.md).

Lär dig mer om [Azure Hybrid Använd nytta och Azure Site Recovery göra migrera program till Azure ännu mer kostnadseffektiv](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/).
