---
title: "aaaEncrypt diskar på en Windows-dator i Azure | Microsoft Docs"
description: "Hur tooencrypt virtuella diskar på en Windows-VM för utökad säkerhet med hjälp av Azure PowerShell"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/10/2017
ms.author: iainfou
ms.openlocfilehash: 77c42a67cb57a9dc5fe3159fce0be75e3a965be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencrypt-virtual-disks-on-a-windows-vm"></a>Hur tooencrypt virtuella diskar på en Windows VM
För förbättrad virtuell dator (VM) säkerhet och efterlevnad krypteras virtuella diskar i Azure. Diskar krypteras med kryptografiska nycklar som skyddas i ett Azure Key Vault. Du styr dessa kryptografiska nycklar och granska deras användning. Den här artikeln information om hur tooencrypt virtuella diskar på en virtuell Windows-dator med hjälp av Azure PowerShell. Du kan också [kryptera en Linux VM som använder hello Azure CLI 2.0](../linux/encrypt-disks.md).

## <a name="overview-of-disk-encryption"></a>Översikt över diskkryptering
Virtuella diskar på virtuella Windows-datorer krypteras vilande med Bitlocker. Det är gratis för kryptering av virtuella diskar i Azure. Kryptografiska nycklar lagras i Azure Key Vault med skydd av programvara eller du kan importera eller generera dina nycklar i Maskinvarusäkerhetsmoduler (HSM) certifierade tooFIPS 140-2 level 2-standarder. Dessa kryptografiska nycklar används tooencrypt och dekryptera virtuella diskar anslutna tooyour VM. Du behålla kontrollen över dessa kryptografiska nycklar och granska deras användning. Ett Azure Active Directory-tjänstens huvudnamn ger en säker mekanism för att utfärda de kryptografiska nycklarna som virtuella datorer är igång på och stänga av.

hello-processen för att kryptera en virtuell dator är som följer:

1. Skapa en kryptografisk nyckel i en Azure Key Vault.
2. Konfigurera hello kryptografiska nyckel toobe kan användas för att kryptera diskar.
3. tooread hello krypteringsnyckeln från hello Azure Key Vault skapa en Azure Active Directory service principal med hello lämpliga behörigheter.
4. Utfärda hello kommandot tooencrypt din virtuella diskar, att ange hello Azure Active Directory tjänstens huvudnamn och lämpliga kryptografiska nyckel toobe används.
5. hello Azure Active Directory service principal begäranden hello krävs krypteringsnyckeln från Azure Key Vault.
6. hello virtuella diskar krypteras med hello tillhandahålls kryptografisk nyckel.

## <a name="encryption-process"></a>Krypteringsprocessen
Kryptering är beroende av hello följande ytterligare komponenter:

* **Azure Key Vault** -används toosafeguard kryptografiska nycklar och hemligheter som används för hello disk kryptering/dekryptering process. 
  * Om det finns ett kan du använda en befintlig Azure Key Vault. Du har inte toodedicate en Key Vault tooencrypting diskar.
  * Du kan skapa ett dedikerat Nyckelvalv tooseparate administrativa gränser och viktiga synlighet.
* **Azure Active Directory** - referenser hello säker utbyte av kryptografiska nycklar som krävs och autentisering för begärt åtgärder. 
  * Vanligtvis kan du använda en befintlig instans av Azure Active Directory för ditt program.
  * hello tjänstens huvudnamn tillhandahåller en mekanism för säker toorequest och utfärdas hello lämpliga kryptografiska nycklar. Du utvecklar inte ett verkligt program som integreras med Azure Active Directory.

## <a name="requirements-and-limitations"></a>Krav och begränsningar
Krav för diskkryptering och scenarier som stöds:

* Aktivera kryptering på den nya virtuella Windows-datorer från Azure Marketplace-bilder eller anpassade VHD-avbildningen.
* Aktivera kryptering på befintliga virtuella Windows-datorer i Azure.
* Aktivera kryptering på virtuella Windows-datorer som har konfigurerats med hjälp av lagringsutrymmen.
* Om du inaktiverar kryptering på Operativsystemet och enheter för Windows-datorer.
* Alla resurser (till exempel Nyckelvalv lagringskonto och VM) måste vara i hello samma Azure-region och prenumeration.
* Standardnivån virtuella datorer, till exempel A, D, DS, G och GS-serien virtuella datorer.

Diskkryptering stöds inte för närvarande i hello följande scenarier:

* Grundnivån virtuella datorer.
* Virtuella datorer skapas med hello klassiska distributionsmodellen.
* Uppdaterar hello Kryptografiska nycklar på en redan krypterade VM.
* Integrering med lokal nyckelhanteringstjänst.

## <a name="create-azure-key-vault-and-keys"></a>Skapa Azure Key Vault och nycklar
Innan du börjar bör du kontrollera om den senaste versionen hello av hello Azure PowerShell-modulen har installerats. Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview). Ersätt alla exempel parametrar med egna namn, plats och nyckelvärden i hela hello kommandoexempel. hello följande exempel används en konvention för *myResourceGroup*, *myKeyVault*, *myVM*osv.

hello första steget är toocreate ett Azure Key Vault toostore kryptografiska nycklar. Azure Key Vault kan lagra nycklar, hemligheter, eller lösenord som gör att du toosecurely implementeras i dina program och tjänster. För virtuell diskkryptering, och skapa en Key Vault-toostore en krypteringsnyckel som används tooencrypt eller dekryptera din virtuella diskar. 

Aktivera hello Azure Key Vault-providern i din Azure-prenumeration med [registrera AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). hello följande exempel skapas ett Resursgruppsnamn *myResourceGroup* i hello *östra USA* plats:

```powershell
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

hello Azure Key Vault som innehåller hello krypteringsnycklar och associerade beräkning resurser, till exempel lagring och hello Virtuella datorn måste finnas i hello samma region. Skapa ett Azure Key Vault med [ny AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) och aktivera hello Key Vault för användning med diskkryptering. Ange ett unikt namn för Key Vault för *keyVaultName* på följande sätt:

```powershell
$keyVaultName = "myUniqueKeyVaultName"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

Du kan lagra kryptografiska nycklar med hjälp av programvara eller maskinvara säkerhet modellen (HSM)-skydd. Använda en HSM kräver en premium Key Vault. Det finns ett extra kostnad toocreating bidrag Key Vault snarare än standard Key Vault som lagrar programvara-skyddade nycklar. toocreate premium Key Vault i föregående steg hello lägga till hello *- Sku ”Premium-* parametrar. hello följande exempel används programvaruskyddad nycklar eftersom vi har skapat ett Nyckelvalv som standard. 

För båda modellerna skydd måste hello Azure-plattformen toobe beviljas åtkomst toorequest hello Kryptografiska nycklar när hello VM startas toodecrypt hello virtuella diskar. Skapa en kryptografisk nyckel i ditt Nyckelvalv med [Lägg till AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey). hello följande exempel skapas en nyckel som heter *MinNyckel*:

```powershell
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```


## <a name="create-hello-azure-active-directory-service-principal"></a>Skapa hello Azure Active Directory-tjänstens huvudnamn
När virtuella diskar krypteras och dekrypteras, anger du ett konto toohandle hello autentisering och byta ut kryptografiska nycklar från Nyckelvalvet. Det här kontot, tjänstens huvudnamn för ett Azure Active Directory kan hello Azure-plattformen toorequest hello lämpliga krypteringsnycklar uppdrag hello VM. En standardinstans för Azure Active Directory finns i din prenumeration, även om många organisationer har särskilda Azure Active Directory-kataloger.

Skapa ett huvudnamn för tjänsten i Azure Active Directory med [ny AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal). toospecify ett säkert lösenord följer hello [lösenordsprinciper och begränsningar i Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):

```powershell
$appName = "My App"
$securePassword = "P@ssword!"
$app = New-AzureRmADApplication -DisplayName $appName `
    -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" `
    -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
```

toosuccessfully kryptera eller dekryptera virtuella diskar, behörigheter på hello kryptografiska nyckel som lagras i Nyckelvalvet måste vara set toopermit hello Azure Active Directory service principal tooread hello nycklar. Ange behörigheter för ditt Nyckelvalv med [Set AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "WrapKey" `
    -PermissionsToSecrets "Set"
```


## <a name="create-virtual-machine"></a>Skapa en virtuell dator
tootest Hej krypteringsprocessen, ska vi skapa en virtuell dator. hello följande exempel skapas en virtuell dator med namnet *myVM* med hjälp av en *Windows Server 2016 Datacenter* avbildningen:

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName `
    -Location $location `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress -ResourceGroupName $rgName `
    -Location $location `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "mypublicdns$(Get-Random)"

$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName `
    -Location $location `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface -Name myNic `
    -ResourceGroupName $rgName `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$cred = Get-Credential

$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig
```


## <a name="encrypt-virtual-machine"></a>Kryptera en virtuell dator
tooencrypt hello virtuella diskar kan du sammanföra alla tidigare hello-komponenter:

1. Ange hello Azure Active Directory-tjänstens huvudnamn och lösenord.
2. Ange hello Key Vault toostore hello metadata för krypterade diskarna.
3. Ange hello krypteringsnycklar toouse för hello faktiska kryptering och dekryptering.
4. Ange om du vill tooencrypt hello OS-disk, hello datadiskar eller alla.

Kryptera din virtuella dator med [Set AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) med hello Azure Key Vault-nyckel och autentiseringsuppgifter för Azure Active Directory-UPN. hello följande exempel hämtas alla hello viktig information och sedan krypterar hello virtuella datorn med namnet *myVM*:

```powershell
$keyVault = Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName $vmName `
    -AadClientID $app.ApplicationId `
    -AadClientSecret $securePassword `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

Acceptera hello fråga toocontinue med hello VM-kryptering. hello VM startas om under hello-processen. När hello krypteringsprocessen har slutförts och hello VM har startats om, granska hello krypteringsstatus med [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
```

hello utdata är liknande toohello följande exempel:

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Nästa steg
* Mer information om hur du hanterar Azure Key Vault finns [konfigurera Key Vault för virtuella datorer](key-vault-setup.md).
* Läs mer om diskkryptering, till exempel förbereda en krypterad anpassade VM tooupload tooAzure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
