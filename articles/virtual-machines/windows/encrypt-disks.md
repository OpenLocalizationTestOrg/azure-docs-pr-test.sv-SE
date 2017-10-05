---
title: "Kryptera diskar på en Windows-dator i Azure | Microsoft Docs"
description: "Kryptera virtuella diskar på en Windows-VM för ökad säkerhet med hjälp av Azure PowerShell"
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
ms.openlocfilehash: 98b42b252a601af090579e3939f3c7ab91c3803b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-encrypt-virtual-disks-on-a-windows-vm"></a><span data-ttu-id="2973b-103">Kryptera virtuella diskar på en Windows VM</span><span class="sxs-lookup"><span data-stu-id="2973b-103">How to encrypt virtual disks on a Windows VM</span></span>
<span data-ttu-id="2973b-104">För förbättrad virtuell dator (VM) säkerhet och efterlevnad krypteras virtuella diskar i Azure.</span><span class="sxs-lookup"><span data-stu-id="2973b-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted.</span></span> <span data-ttu-id="2973b-105">Diskar krypteras med kryptografiska nycklar som skyddas i ett Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2973b-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="2973b-106">Du styr dessa kryptografiska nycklar och granska deras användning.</span><span class="sxs-lookup"><span data-stu-id="2973b-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="2973b-107">Den här artikeln beskrivs hur du krypterar virtuella diskar på en virtuell Windows-dator med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2973b-107">This article details how to encrypt virtual disks on a Windows VM using Azure PowerShell.</span></span> <span data-ttu-id="2973b-108">Du kan också [kryptera en Linux VM som använder Azure CLI 2.0](../linux/encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="2973b-108">You can also [encrypt a Linux VM using the Azure CLI 2.0](../linux/encrypt-disks.md).</span></span>

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="2973b-109">Översikt över diskkryptering</span><span class="sxs-lookup"><span data-stu-id="2973b-109">Overview of disk encryption</span></span>
<span data-ttu-id="2973b-110">Virtuella diskar på virtuella Windows-datorer krypteras vilande med Bitlocker.</span><span class="sxs-lookup"><span data-stu-id="2973b-110">Virtual disks on Windows VMs are encrypted at rest using Bitlocker.</span></span> <span data-ttu-id="2973b-111">Det är gratis för kryptering av virtuella diskar i Azure.</span><span class="sxs-lookup"><span data-stu-id="2973b-111">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="2973b-112">Kryptografiska nycklar lagras i Azure Key Vault med skydd av programvara eller du kan importera eller generera dina nycklar i Maskinvarusäkerhetsmoduler (HSM) certifierade FIPS 140-2 level 2-standarder.</span><span class="sxs-lookup"><span data-stu-id="2973b-112">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified to FIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="2973b-113">Dessa kryptografiska nycklar används för att kryptera och dekryptera virtuella diskar som är anslutna till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2973b-113">These cryptographic keys are used to encrypt and decrypt virtual disks attached to your VM.</span></span> <span data-ttu-id="2973b-114">Du behålla kontrollen över dessa kryptografiska nycklar och granska deras användning.</span><span class="sxs-lookup"><span data-stu-id="2973b-114">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="2973b-115">Ett Azure Active Directory-tjänstens huvudnamn ger en säker mekanism för att utfärda de kryptografiska nycklarna som virtuella datorer är igång på och stänga av.</span><span class="sxs-lookup"><span data-stu-id="2973b-115">An Azure Active Directory service principal provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="2973b-116">Processen för att kryptera en virtuell dator är följande:</span><span class="sxs-lookup"><span data-stu-id="2973b-116">The process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="2973b-117">Skapa en kryptografisk nyckel i en Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2973b-117">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="2973b-118">Konfigurera den kryptografiska nyckeln som kan användas för att kryptera diskar.</span><span class="sxs-lookup"><span data-stu-id="2973b-118">Configure the cryptographic key to be usable for encrypting disks.</span></span>
3. <span data-ttu-id="2973b-119">Om du vill läsa den kryptografiska nyckeln från Azure Key Vault, skapa en Azure Active Directory service principal med rätt behörighet.</span><span class="sxs-lookup"><span data-stu-id="2973b-119">To read the cryptographic key from the Azure Key Vault, create an Azure Active Directory service principal with the appropriate permissions.</span></span>
4. <span data-ttu-id="2973b-120">Utfärda kommandot för att kryptera din virtuella diskar, ange den Azure Active Directory service principal och lämpligt kryptografiska nyckeln som ska användas.</span><span class="sxs-lookup"><span data-stu-id="2973b-120">Issue the command to encrypt your virtual disks, specifying the Azure Active Directory service principal and appropriate cryptographic key to be used.</span></span>
5. <span data-ttu-id="2973b-121">Azure Active Directory-tjänstens huvudnamn begär den kryptografiska nyckeln som krävs från Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2973b-121">The Azure Active Directory service principal requests the required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="2973b-122">De virtuella diskarna krypteras med den angivna kryptografiska nyckeln.</span><span class="sxs-lookup"><span data-stu-id="2973b-122">The virtual disks are encrypted using the provided cryptographic key.</span></span>

## <a name="encryption-process"></a><span data-ttu-id="2973b-123">Krypteringsprocessen</span><span class="sxs-lookup"><span data-stu-id="2973b-123">Encryption process</span></span>
<span data-ttu-id="2973b-124">Kryptering är beroende av följande ytterligare komponenter:</span><span class="sxs-lookup"><span data-stu-id="2973b-124">Disk encryption relies on the following additional components:</span></span>

* <span data-ttu-id="2973b-125">**Azure Key Vault** – används för att skydda krypteringsnycklar och hemligheter som används för kryptering/dekryptering processen disk.</span><span class="sxs-lookup"><span data-stu-id="2973b-125">**Azure Key Vault** - used to safeguard cryptographic keys and secrets used for the disk encryption/decryption process.</span></span> 
  * <span data-ttu-id="2973b-126">Om det finns ett kan du använda en befintlig Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2973b-126">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="2973b-127">Du har inte kryptera diskar för ett Nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="2973b-127">You do not have to dedicate a Key Vault to encrypting disks.</span></span>
  * <span data-ttu-id="2973b-128">Du kan skapa ett dedikerat Nyckelvalv för att avgränsa administrativa gränser och viktiga synlighet.</span><span class="sxs-lookup"><span data-stu-id="2973b-128">To separate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="2973b-129">**Azure Active Directory** -hanterar säker utbyte av kryptografiska nycklar som krävs och autentisering för begärda åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2973b-129">**Azure Active Directory** - handles the secure exchanging of required cryptographic keys and authentication for requested actions.</span></span> 
  * <span data-ttu-id="2973b-130">Vanligtvis kan du använda en befintlig instans av Azure Active Directory för ditt program.</span><span class="sxs-lookup"><span data-stu-id="2973b-130">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="2973b-131">Tjänstens huvudnamn ger säker möjlighet att begära och utfärdas lämpliga kryptografiska nycklar.</span><span class="sxs-lookup"><span data-stu-id="2973b-131">The service principal provides a secure mechanism to request and be issued the appropriate cryptographic keys.</span></span> <span data-ttu-id="2973b-132">Du utvecklar inte ett verkligt program som integreras med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2973b-132">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="2973b-133">Krav och begränsningar</span><span class="sxs-lookup"><span data-stu-id="2973b-133">Requirements and limitations</span></span>
<span data-ttu-id="2973b-134">Krav för diskkryptering och scenarier som stöds:</span><span class="sxs-lookup"><span data-stu-id="2973b-134">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="2973b-135">Aktivera kryptering på den nya virtuella Windows-datorer från Azure Marketplace-bilder eller anpassade VHD-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="2973b-135">Enabling encryption on new Windows VMs from Azure Marketplace images or custom VHD image.</span></span>
* <span data-ttu-id="2973b-136">Aktivera kryptering på befintliga virtuella Windows-datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="2973b-136">Enabling encryption on existing Windows VMs in Azure.</span></span>
* <span data-ttu-id="2973b-137">Aktivera kryptering på virtuella Windows-datorer som har konfigurerats med hjälp av lagringsutrymmen.</span><span class="sxs-lookup"><span data-stu-id="2973b-137">Enabling encryption on Windows VMs that are configured using Storage Spaces.</span></span>
* <span data-ttu-id="2973b-138">Om du inaktiverar kryptering på Operativsystemet och enheter för Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="2973b-138">Disabling encryption on OS and data drives for Windows VMs.</span></span>
* <span data-ttu-id="2973b-139">Alla resurser (till exempel Nyckelvalv lagringskonto och VM) måste finnas i samma Azure-region och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2973b-139">All resources (such as Key Vault, Storage account, and VM) must be in the same Azure region and subscription.</span></span>
* <span data-ttu-id="2973b-140">Standardnivån virtuella datorer, till exempel A, D, DS, G och GS-serien virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2973b-140">Standard tier VMs, such as A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="2973b-141">Diskkryptering stöds inte i följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="2973b-141">Disk encryption is not currently supported in the following scenarios:</span></span>

* <span data-ttu-id="2973b-142">Grundnivån virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2973b-142">Basic tier VMs.</span></span>
* <span data-ttu-id="2973b-143">Virtuella datorer skapas med den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="2973b-143">VMs created using the Classic deployment model.</span></span>
* <span data-ttu-id="2973b-144">Uppdaterar de kryptografiska nycklarna på en redan krypterade VM.</span><span class="sxs-lookup"><span data-stu-id="2973b-144">Updating the cryptographic keys on an already encrypted VM.</span></span>
* <span data-ttu-id="2973b-145">Integrering med lokal nyckelhanteringstjänst.</span><span class="sxs-lookup"><span data-stu-id="2973b-145">Integration with on-prem Key Management Service.</span></span>

## <a name="create-azure-key-vault-and-keys"></a><span data-ttu-id="2973b-146">Skapa Azure Key Vault och nycklar</span><span class="sxs-lookup"><span data-stu-id="2973b-146">Create Azure Key Vault and keys</span></span>
<span data-ttu-id="2973b-147">Innan du börjar bör du kontrollera att den senaste versionen av Azure PowerShell-modulen har installerats.</span><span class="sxs-lookup"><span data-stu-id="2973b-147">Before you start, make sure that the latest version of the Azure PowerShell module has been installed.</span></span> <span data-ttu-id="2973b-148">Mer information finns i [Installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2973b-148">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="2973b-149">Ersätt alla exempel parametrar med egna namn, plats och nyckelvärden under hela kommandoexempel.</span><span class="sxs-lookup"><span data-stu-id="2973b-149">Throughout the command examples, replace all example parameters with your own names, location, and key values.</span></span> <span data-ttu-id="2973b-150">I följande exempel används en konvention för *myResourceGroup*, *myKeyVault*, *myVM*osv.</span><span class="sxs-lookup"><span data-stu-id="2973b-150">The following examples use a convention of *myResourceGroup*, *myKeyVault*, *myVM*, etc.</span></span>

<span data-ttu-id="2973b-151">Det första steget är att skapa en Azure Key Vault för att lagra kryptografiska nycklar.</span><span class="sxs-lookup"><span data-stu-id="2973b-151">The first step is to create an Azure Key Vault to store your cryptographic keys.</span></span> <span data-ttu-id="2973b-152">Azure Key Vault kan lagra nycklar, hemligheter eller lösenord som gör det möjligt att implementera dem på ett säkert sätt i dina program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="2973b-152">Azure Key Vault can store keys, secrets, or passwords that allow you to securely implement them in your applications and services.</span></span> <span data-ttu-id="2973b-153">För virtuell diskkryptering, kan du skapa ett Nyckelvalv för att lagra en krypteringsnyckel som används för att kryptera eller dekryptera din virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="2973b-153">For virtual disk encryption, you create a Key Vault to store a cryptographic key that is used to encrypt or decrypt your virtual disks.</span></span> 

<span data-ttu-id="2973b-154">Aktivera Azure Key Vault-providern i din Azure-prenumeration med [registrera AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="2973b-154">Enable the Azure Key Vault provider within your Azure subscription with [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), then create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="2973b-155">I följande exempel skapas ett Resursgruppsnamn *myResourceGroup* i den *östra USA* plats:</span><span class="sxs-lookup"><span data-stu-id="2973b-155">The following example creates a resource group name *myResourceGroup* in the *East US* location:</span></span>

```powershell
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

<span data-ttu-id="2973b-156">Azure Key Vault som innehåller kryptografiska nycklar och associerade beräkningsresurser som lagring och Virtuellt datorn måste finnas i samma region.</span><span class="sxs-lookup"><span data-stu-id="2973b-156">The Azure Key Vault containing the cryptographic keys and associated compute resources such as storage and the VM itself must reside in the same region.</span></span> <span data-ttu-id="2973b-157">Skapa ett Azure Key Vault med [ny AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) och aktivera Nyckelvalvet för användning med diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="2973b-157">Create an Azure Key Vault with [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) and enable the Key Vault for use with disk encryption.</span></span> <span data-ttu-id="2973b-158">Ange ett unikt namn för Key Vault för *keyVaultName* på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2973b-158">Specify a unique Key Vault name for *keyVaultName* as follows:</span></span>

```powershell
$keyVaultName = "myUniqueKeyVaultName"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

<span data-ttu-id="2973b-159">Du kan lagra kryptografiska nycklar med hjälp av programvara eller maskinvara säkerhet modellen (HSM)-skydd.</span><span class="sxs-lookup"><span data-stu-id="2973b-159">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="2973b-160">Använda en HSM kräver en premium Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2973b-160">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="2973b-161">Det finns en extra kostnad för att skapa en premium Key Vault snarare än standard Key Vault som lagrar programvara-skyddade nycklar.</span><span class="sxs-lookup"><span data-stu-id="2973b-161">There is an additional cost to creating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="2973b-162">När du skapar en premium Key Vault i föregående steg till den *- Sku ”Premium-* parametrar.</span><span class="sxs-lookup"><span data-stu-id="2973b-162">To create a premium Key Vault, in the preceding step add the *-Sku "Premium"* parameters.</span></span> <span data-ttu-id="2973b-163">I följande exempel används programvaruskyddad nycklar eftersom vi har skapat ett Nyckelvalv som standard.</span><span class="sxs-lookup"><span data-stu-id="2973b-163">The following example uses software-protected keys since we created a standard Key Vault.</span></span> 

<span data-ttu-id="2973b-164">För båda modellerna skydd måste Azure-plattformen beviljas åtkomst för att begära kryptografiska nycklar när den virtuella datorn startas för att dekryptera virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="2973b-164">For both protection models, the Azure platform needs to be granted access to request the cryptographic keys when the VM boots to decrypt the virtual disks.</span></span> <span data-ttu-id="2973b-165">Skapa en kryptografisk nyckel i ditt Nyckelvalv med [Lägg till AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey).</span><span class="sxs-lookup"><span data-stu-id="2973b-165">Create a cryptographic key in your Key Vault with [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey).</span></span> <span data-ttu-id="2973b-166">I följande exempel skapas en nyckel som heter *MinNyckel*:</span><span class="sxs-lookup"><span data-stu-id="2973b-166">The following example creates a key named *myKey*:</span></span>

```powershell
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```


## <a name="create-the-azure-active-directory-service-principal"></a><span data-ttu-id="2973b-167">Skapa Azure Active Directory huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="2973b-167">Create the Azure Active Directory service principal</span></span>
<span data-ttu-id="2973b-168">När virtuella diskar krypteras och dekrypteras, anger du ett konto för att hantera autentisering och byta ut kryptografiska nycklar från Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="2973b-168">When virtual disks are encrypted or decrypted, you specify an account to handle the authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="2973b-169">Det här kontot, tjänstens huvudnamn för ett Azure Active Directory kan Azure-plattformen begära lämpliga kryptografiska nycklar för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2973b-169">This account, an Azure Active Directory service principal, allows the Azure platform to request the appropriate cryptographic keys on behalf of the VM.</span></span> <span data-ttu-id="2973b-170">En standardinstans för Azure Active Directory finns i din prenumeration, även om många organisationer har särskilda Azure Active Directory-kataloger.</span><span class="sxs-lookup"><span data-stu-id="2973b-170">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="2973b-171">Skapa ett huvudnamn för tjänsten i Azure Active Directory med [ny AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal).</span><span class="sxs-lookup"><span data-stu-id="2973b-171">Create a service principal in Azure Active Directory with [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal).</span></span> <span data-ttu-id="2973b-172">Så här anger du ett säkert lösenord, den [lösenordsprinciper och begränsningar i Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):</span><span class="sxs-lookup"><span data-stu-id="2973b-172">To specify a secure password, follow the [Password policies and restrictions in Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):</span></span>

```powershell
$appName = "My App"
$securePassword = "P@ssword!"
$app = New-AzureRmADApplication -DisplayName $appName `
    -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" `
    -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
```

<span data-ttu-id="2973b-173">För att kunna kryptera eller dekryptera virtuella diskar, måste behörigheter på den kryptografiska nyckel som lagras i Nyckelvalvet anges för att tillåta Azure Active Directory-tjänstens huvudnamn att läsa nycklarna.</span><span class="sxs-lookup"><span data-stu-id="2973b-173">To successfully encrypt or decrypt virtual disks, permissions on the cryptographic key stored in Key Vault must be set to permit the Azure Active Directory service principal to read the keys.</span></span> <span data-ttu-id="2973b-174">Ange behörigheter för ditt Nyckelvalv med [Set AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):</span><span class="sxs-lookup"><span data-stu-id="2973b-174">Set permissions on your Key Vault with [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "WrapKey" `
    -PermissionsToSecrets "Set"
```


## <a name="create-virtual-machine"></a><span data-ttu-id="2973b-175">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="2973b-175">Create virtual machine</span></span>
<span data-ttu-id="2973b-176">Om du vill testa krypteringsprocessen nu ska vi skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2973b-176">To test the encryption process, let's create a VM.</span></span> <span data-ttu-id="2973b-177">I följande exempel skapas en virtuell dator med namnet *myVM* med hjälp av en *Windows Server 2016 Datacenter* avbildningen:</span><span class="sxs-lookup"><span data-stu-id="2973b-177">The following example creates a VM named *myVM* using a *Windows Server 2016 Datacenter* image:</span></span>

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


## <a name="encrypt-virtual-machine"></a><span data-ttu-id="2973b-178">Kryptera en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="2973b-178">Encrypt virtual machine</span></span>
<span data-ttu-id="2973b-179">Om du vill kryptera virtuella diskar samordnar du tidigare komponenter:</span><span class="sxs-lookup"><span data-stu-id="2973b-179">To encrypt the virtual disks, you bring together all the previous components:</span></span>

1. <span data-ttu-id="2973b-180">Ange tjänstens huvudnamn för Azure Active Directory och lösenord.</span><span class="sxs-lookup"><span data-stu-id="2973b-180">Specify the Azure Active Directory service principal and password.</span></span>
2. <span data-ttu-id="2973b-181">Ange Nyckelvalv för att lagra metadata för krypterade diskarna.</span><span class="sxs-lookup"><span data-stu-id="2973b-181">Specify the Key Vault to store the metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="2973b-182">Ange de kryptografiska nycklarna som ska användas för den faktiska kryptering och dekryptering.</span><span class="sxs-lookup"><span data-stu-id="2973b-182">Specify the cryptographic keys to use for the actual encryption and decryption.</span></span>
4. <span data-ttu-id="2973b-183">Ange om du vill kryptera OS-disk, datadiskar eller alla.</span><span class="sxs-lookup"><span data-stu-id="2973b-183">Specify whether you want to encrypt the OS disk, the data disks, or all.</span></span>

<span data-ttu-id="2973b-184">Kryptera din virtuella dator med [Set AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) med Azure Key Vault-nyckel och autentiseringsuppgifter för Azure Active Directory-UPN.</span><span class="sxs-lookup"><span data-stu-id="2973b-184">Encrypt your VM with [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) using the Azure Key Vault key and Azure Active Directory service principal credentials.</span></span> <span data-ttu-id="2973b-185">I följande exempel hämtas alla viktig information och krypterar den virtuella datorn med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="2973b-185">The following example retrieves all the key information then encrypts the VM named *myVM*:</span></span>

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

<span data-ttu-id="2973b-186">Acceptera uppmaningen om att fortsätta med VM-kryptering.</span><span class="sxs-lookup"><span data-stu-id="2973b-186">Accept the prompt to continue with the VM encryption.</span></span> <span data-ttu-id="2973b-187">Den virtuella datorn startas om under processen.</span><span class="sxs-lookup"><span data-stu-id="2973b-187">The VM restarts during the process.</span></span> <span data-ttu-id="2973b-188">När krypteringsprocessen har slutförts och den virtuella datorn har startats om, granska krypteringsstatus med [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):</span><span class="sxs-lookup"><span data-stu-id="2973b-188">Once the encryption process completes and the VM has rebooted, review the encryption status with [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):</span></span>

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
```

<span data-ttu-id="2973b-189">Utdata ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="2973b-189">The output is similar to the following example:</span></span>

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a><span data-ttu-id="2973b-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2973b-190">Next steps</span></span>
* <span data-ttu-id="2973b-191">Mer information om hur du hanterar Azure Key Vault finns [konfigurera Key Vault för virtuella datorer](key-vault-setup.md).</span><span class="sxs-lookup"><span data-stu-id="2973b-191">For more information about managing Azure Key Vault, see [Set up Key Vault for virtual machines](key-vault-setup.md).</span></span>
* <span data-ttu-id="2973b-192">Läs mer om diskkryptering, till exempel förbereda en krypterad anpassade VM att överföra till Azure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="2973b-192">For more information about disk encryption, such as preparing an encrypted custom VM to upload to Azure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
