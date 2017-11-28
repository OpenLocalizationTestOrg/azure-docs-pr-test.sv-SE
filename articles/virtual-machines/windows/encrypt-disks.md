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
# <a name="how-tooencrypt-virtual-disks-on-a-windows-vm"></a><span data-ttu-id="d84f8-103">Hur tooencrypt virtuella diskar på en Windows VM</span><span class="sxs-lookup"><span data-stu-id="d84f8-103">How tooencrypt virtual disks on a Windows VM</span></span>
<span data-ttu-id="d84f8-104">För förbättrad virtuell dator (VM) säkerhet och efterlevnad krypteras virtuella diskar i Azure.</span><span class="sxs-lookup"><span data-stu-id="d84f8-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted.</span></span> <span data-ttu-id="d84f8-105">Diskar krypteras med kryptografiska nycklar som skyddas i ett Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d84f8-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="d84f8-106">Du styr dessa kryptografiska nycklar och granska deras användning.</span><span class="sxs-lookup"><span data-stu-id="d84f8-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="d84f8-107">Den här artikeln information om hur tooencrypt virtuella diskar på en virtuell Windows-dator med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d84f8-107">This article details how tooencrypt virtual disks on a Windows VM using Azure PowerShell.</span></span> <span data-ttu-id="d84f8-108">Du kan också [kryptera en Linux VM som använder hello Azure CLI 2.0](../linux/encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="d84f8-108">You can also [encrypt a Linux VM using hello Azure CLI 2.0](../linux/encrypt-disks.md).</span></span>

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="d84f8-109">Översikt över diskkryptering</span><span class="sxs-lookup"><span data-stu-id="d84f8-109">Overview of disk encryption</span></span>
<span data-ttu-id="d84f8-110">Virtuella diskar på virtuella Windows-datorer krypteras vilande med Bitlocker.</span><span class="sxs-lookup"><span data-stu-id="d84f8-110">Virtual disks on Windows VMs are encrypted at rest using Bitlocker.</span></span> <span data-ttu-id="d84f8-111">Det är gratis för kryptering av virtuella diskar i Azure.</span><span class="sxs-lookup"><span data-stu-id="d84f8-111">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="d84f8-112">Kryptografiska nycklar lagras i Azure Key Vault med skydd av programvara eller du kan importera eller generera dina nycklar i Maskinvarusäkerhetsmoduler (HSM) certifierade tooFIPS 140-2 level 2-standarder.</span><span class="sxs-lookup"><span data-stu-id="d84f8-112">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified tooFIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="d84f8-113">Dessa kryptografiska nycklar används tooencrypt och dekryptera virtuella diskar anslutna tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="d84f8-113">These cryptographic keys are used tooencrypt and decrypt virtual disks attached tooyour VM.</span></span> <span data-ttu-id="d84f8-114">Du behålla kontrollen över dessa kryptografiska nycklar och granska deras användning.</span><span class="sxs-lookup"><span data-stu-id="d84f8-114">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="d84f8-115">Ett Azure Active Directory-tjänstens huvudnamn ger en säker mekanism för att utfärda de kryptografiska nycklarna som virtuella datorer är igång på och stänga av.</span><span class="sxs-lookup"><span data-stu-id="d84f8-115">An Azure Active Directory service principal provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="d84f8-116">hello-processen för att kryptera en virtuell dator är som följer:</span><span class="sxs-lookup"><span data-stu-id="d84f8-116">hello process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="d84f8-117">Skapa en kryptografisk nyckel i en Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d84f8-117">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="d84f8-118">Konfigurera hello kryptografiska nyckel toobe kan användas för att kryptera diskar.</span><span class="sxs-lookup"><span data-stu-id="d84f8-118">Configure hello cryptographic key toobe usable for encrypting disks.</span></span>
3. <span data-ttu-id="d84f8-119">tooread hello krypteringsnyckeln från hello Azure Key Vault skapa en Azure Active Directory service principal med hello lämpliga behörigheter.</span><span class="sxs-lookup"><span data-stu-id="d84f8-119">tooread hello cryptographic key from hello Azure Key Vault, create an Azure Active Directory service principal with hello appropriate permissions.</span></span>
4. <span data-ttu-id="d84f8-120">Utfärda hello kommandot tooencrypt din virtuella diskar, att ange hello Azure Active Directory tjänstens huvudnamn och lämpliga kryptografiska nyckel toobe används.</span><span class="sxs-lookup"><span data-stu-id="d84f8-120">Issue hello command tooencrypt your virtual disks, specifying hello Azure Active Directory service principal and appropriate cryptographic key toobe used.</span></span>
5. <span data-ttu-id="d84f8-121">hello Azure Active Directory service principal begäranden hello krävs krypteringsnyckeln från Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d84f8-121">hello Azure Active Directory service principal requests hello required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="d84f8-122">hello virtuella diskar krypteras med hello tillhandahålls kryptografisk nyckel.</span><span class="sxs-lookup"><span data-stu-id="d84f8-122">hello virtual disks are encrypted using hello provided cryptographic key.</span></span>

## <a name="encryption-process"></a><span data-ttu-id="d84f8-123">Krypteringsprocessen</span><span class="sxs-lookup"><span data-stu-id="d84f8-123">Encryption process</span></span>
<span data-ttu-id="d84f8-124">Kryptering är beroende av hello följande ytterligare komponenter:</span><span class="sxs-lookup"><span data-stu-id="d84f8-124">Disk encryption relies on hello following additional components:</span></span>

* <span data-ttu-id="d84f8-125">**Azure Key Vault** -används toosafeguard kryptografiska nycklar och hemligheter som används för hello disk kryptering/dekryptering process.</span><span class="sxs-lookup"><span data-stu-id="d84f8-125">**Azure Key Vault** - used toosafeguard cryptographic keys and secrets used for hello disk encryption/decryption process.</span></span> 
  * <span data-ttu-id="d84f8-126">Om det finns ett kan du använda en befintlig Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d84f8-126">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="d84f8-127">Du har inte toodedicate en Key Vault tooencrypting diskar.</span><span class="sxs-lookup"><span data-stu-id="d84f8-127">You do not have toodedicate a Key Vault tooencrypting disks.</span></span>
  * <span data-ttu-id="d84f8-128">Du kan skapa ett dedikerat Nyckelvalv tooseparate administrativa gränser och viktiga synlighet.</span><span class="sxs-lookup"><span data-stu-id="d84f8-128">tooseparate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="d84f8-129">**Azure Active Directory** - referenser hello säker utbyte av kryptografiska nycklar som krävs och autentisering för begärt åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d84f8-129">**Azure Active Directory** - handles hello secure exchanging of required cryptographic keys and authentication for requested actions.</span></span> 
  * <span data-ttu-id="d84f8-130">Vanligtvis kan du använda en befintlig instans av Azure Active Directory för ditt program.</span><span class="sxs-lookup"><span data-stu-id="d84f8-130">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="d84f8-131">hello tjänstens huvudnamn tillhandahåller en mekanism för säker toorequest och utfärdas hello lämpliga kryptografiska nycklar.</span><span class="sxs-lookup"><span data-stu-id="d84f8-131">hello service principal provides a secure mechanism toorequest and be issued hello appropriate cryptographic keys.</span></span> <span data-ttu-id="d84f8-132">Du utvecklar inte ett verkligt program som integreras med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d84f8-132">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="d84f8-133">Krav och begränsningar</span><span class="sxs-lookup"><span data-stu-id="d84f8-133">Requirements and limitations</span></span>
<span data-ttu-id="d84f8-134">Krav för diskkryptering och scenarier som stöds:</span><span class="sxs-lookup"><span data-stu-id="d84f8-134">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="d84f8-135">Aktivera kryptering på den nya virtuella Windows-datorer från Azure Marketplace-bilder eller anpassade VHD-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="d84f8-135">Enabling encryption on new Windows VMs from Azure Marketplace images or custom VHD image.</span></span>
* <span data-ttu-id="d84f8-136">Aktivera kryptering på befintliga virtuella Windows-datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="d84f8-136">Enabling encryption on existing Windows VMs in Azure.</span></span>
* <span data-ttu-id="d84f8-137">Aktivera kryptering på virtuella Windows-datorer som har konfigurerats med hjälp av lagringsutrymmen.</span><span class="sxs-lookup"><span data-stu-id="d84f8-137">Enabling encryption on Windows VMs that are configured using Storage Spaces.</span></span>
* <span data-ttu-id="d84f8-138">Om du inaktiverar kryptering på Operativsystemet och enheter för Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="d84f8-138">Disabling encryption on OS and data drives for Windows VMs.</span></span>
* <span data-ttu-id="d84f8-139">Alla resurser (till exempel Nyckelvalv lagringskonto och VM) måste vara i hello samma Azure-region och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d84f8-139">All resources (such as Key Vault, Storage account, and VM) must be in hello same Azure region and subscription.</span></span>
* <span data-ttu-id="d84f8-140">Standardnivån virtuella datorer, till exempel A, D, DS, G och GS-serien virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d84f8-140">Standard tier VMs, such as A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="d84f8-141">Diskkryptering stöds inte för närvarande i hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="d84f8-141">Disk encryption is not currently supported in hello following scenarios:</span></span>

* <span data-ttu-id="d84f8-142">Grundnivån virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d84f8-142">Basic tier VMs.</span></span>
* <span data-ttu-id="d84f8-143">Virtuella datorer skapas med hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="d84f8-143">VMs created using hello Classic deployment model.</span></span>
* <span data-ttu-id="d84f8-144">Uppdaterar hello Kryptografiska nycklar på en redan krypterade VM.</span><span class="sxs-lookup"><span data-stu-id="d84f8-144">Updating hello cryptographic keys on an already encrypted VM.</span></span>
* <span data-ttu-id="d84f8-145">Integrering med lokal nyckelhanteringstjänst.</span><span class="sxs-lookup"><span data-stu-id="d84f8-145">Integration with on-prem Key Management Service.</span></span>

## <a name="create-azure-key-vault-and-keys"></a><span data-ttu-id="d84f8-146">Skapa Azure Key Vault och nycklar</span><span class="sxs-lookup"><span data-stu-id="d84f8-146">Create Azure Key Vault and keys</span></span>
<span data-ttu-id="d84f8-147">Innan du börjar bör du kontrollera om den senaste versionen hello av hello Azure PowerShell-modulen har installerats.</span><span class="sxs-lookup"><span data-stu-id="d84f8-147">Before you start, make sure that hello latest version of hello Azure PowerShell module has been installed.</span></span> <span data-ttu-id="d84f8-148">Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d84f8-148">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="d84f8-149">Ersätt alla exempel parametrar med egna namn, plats och nyckelvärden i hela hello kommandoexempel.</span><span class="sxs-lookup"><span data-stu-id="d84f8-149">Throughout hello command examples, replace all example parameters with your own names, location, and key values.</span></span> <span data-ttu-id="d84f8-150">hello följande exempel används en konvention för *myResourceGroup*, *myKeyVault*, *myVM*osv.</span><span class="sxs-lookup"><span data-stu-id="d84f8-150">hello following examples use a convention of *myResourceGroup*, *myKeyVault*, *myVM*, etc.</span></span>

<span data-ttu-id="d84f8-151">hello första steget är toocreate ett Azure Key Vault toostore kryptografiska nycklar.</span><span class="sxs-lookup"><span data-stu-id="d84f8-151">hello first step is toocreate an Azure Key Vault toostore your cryptographic keys.</span></span> <span data-ttu-id="d84f8-152">Azure Key Vault kan lagra nycklar, hemligheter, eller lösenord som gör att du toosecurely implementeras i dina program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="d84f8-152">Azure Key Vault can store keys, secrets, or passwords that allow you toosecurely implement them in your applications and services.</span></span> <span data-ttu-id="d84f8-153">För virtuell diskkryptering, och skapa en Key Vault-toostore en krypteringsnyckel som används tooencrypt eller dekryptera din virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="d84f8-153">For virtual disk encryption, you create a Key Vault toostore a cryptographic key that is used tooencrypt or decrypt your virtual disks.</span></span> 

<span data-ttu-id="d84f8-154">Aktivera hello Azure Key Vault-providern i din Azure-prenumeration med [registrera AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="d84f8-154">Enable hello Azure Key Vault provider within your Azure subscription with [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), then create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="d84f8-155">hello följande exempel skapas ett Resursgruppsnamn *myResourceGroup* i hello *östra USA* plats:</span><span class="sxs-lookup"><span data-stu-id="d84f8-155">hello following example creates a resource group name *myResourceGroup* in hello *East US* location:</span></span>

```powershell
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

<span data-ttu-id="d84f8-156">hello Azure Key Vault som innehåller hello krypteringsnycklar och associerade beräkning resurser, till exempel lagring och hello Virtuella datorn måste finnas i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="d84f8-156">hello Azure Key Vault containing hello cryptographic keys and associated compute resources such as storage and hello VM itself must reside in hello same region.</span></span> <span data-ttu-id="d84f8-157">Skapa ett Azure Key Vault med [ny AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) och aktivera hello Key Vault för användning med diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="d84f8-157">Create an Azure Key Vault with [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) and enable hello Key Vault for use with disk encryption.</span></span> <span data-ttu-id="d84f8-158">Ange ett unikt namn för Key Vault för *keyVaultName* på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d84f8-158">Specify a unique Key Vault name for *keyVaultName* as follows:</span></span>

```powershell
$keyVaultName = "myUniqueKeyVaultName"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

<span data-ttu-id="d84f8-159">Du kan lagra kryptografiska nycklar med hjälp av programvara eller maskinvara säkerhet modellen (HSM)-skydd.</span><span class="sxs-lookup"><span data-stu-id="d84f8-159">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="d84f8-160">Använda en HSM kräver en premium Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d84f8-160">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="d84f8-161">Det finns ett extra kostnad toocreating bidrag Key Vault snarare än standard Key Vault som lagrar programvara-skyddade nycklar.</span><span class="sxs-lookup"><span data-stu-id="d84f8-161">There is an additional cost toocreating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="d84f8-162">toocreate premium Key Vault i föregående steg hello lägga till hello *- Sku ”Premium-* parametrar.</span><span class="sxs-lookup"><span data-stu-id="d84f8-162">toocreate a premium Key Vault, in hello preceding step add hello *-Sku "Premium"* parameters.</span></span> <span data-ttu-id="d84f8-163">hello följande exempel används programvaruskyddad nycklar eftersom vi har skapat ett Nyckelvalv som standard.</span><span class="sxs-lookup"><span data-stu-id="d84f8-163">hello following example uses software-protected keys since we created a standard Key Vault.</span></span> 

<span data-ttu-id="d84f8-164">För båda modellerna skydd måste hello Azure-plattformen toobe beviljas åtkomst toorequest hello Kryptografiska nycklar när hello VM startas toodecrypt hello virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="d84f8-164">For both protection models, hello Azure platform needs toobe granted access toorequest hello cryptographic keys when hello VM boots toodecrypt hello virtual disks.</span></span> <span data-ttu-id="d84f8-165">Skapa en kryptografisk nyckel i ditt Nyckelvalv med [Lägg till AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey).</span><span class="sxs-lookup"><span data-stu-id="d84f8-165">Create a cryptographic key in your Key Vault with [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey).</span></span> <span data-ttu-id="d84f8-166">hello följande exempel skapas en nyckel som heter *MinNyckel*:</span><span class="sxs-lookup"><span data-stu-id="d84f8-166">hello following example creates a key named *myKey*:</span></span>

```powershell
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```


## <a name="create-hello-azure-active-directory-service-principal"></a><span data-ttu-id="d84f8-167">Skapa hello Azure Active Directory-tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="d84f8-167">Create hello Azure Active Directory service principal</span></span>
<span data-ttu-id="d84f8-168">När virtuella diskar krypteras och dekrypteras, anger du ett konto toohandle hello autentisering och byta ut kryptografiska nycklar från Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="d84f8-168">When virtual disks are encrypted or decrypted, you specify an account toohandle hello authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="d84f8-169">Det här kontot, tjänstens huvudnamn för ett Azure Active Directory kan hello Azure-plattformen toorequest hello lämpliga krypteringsnycklar uppdrag hello VM.</span><span class="sxs-lookup"><span data-stu-id="d84f8-169">This account, an Azure Active Directory service principal, allows hello Azure platform toorequest hello appropriate cryptographic keys on behalf of hello VM.</span></span> <span data-ttu-id="d84f8-170">En standardinstans för Azure Active Directory finns i din prenumeration, även om många organisationer har särskilda Azure Active Directory-kataloger.</span><span class="sxs-lookup"><span data-stu-id="d84f8-170">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="d84f8-171">Skapa ett huvudnamn för tjänsten i Azure Active Directory med [ny AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal).</span><span class="sxs-lookup"><span data-stu-id="d84f8-171">Create a service principal in Azure Active Directory with [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal).</span></span> <span data-ttu-id="d84f8-172">toospecify ett säkert lösenord följer hello [lösenordsprinciper och begränsningar i Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):</span><span class="sxs-lookup"><span data-stu-id="d84f8-172">toospecify a secure password, follow hello [Password policies and restrictions in Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):</span></span>

```powershell
$appName = "My App"
$securePassword = "P@ssword!"
$app = New-AzureRmADApplication -DisplayName $appName `
    -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" `
    -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
```

<span data-ttu-id="d84f8-173">toosuccessfully kryptera eller dekryptera virtuella diskar, behörigheter på hello kryptografiska nyckel som lagras i Nyckelvalvet måste vara set toopermit hello Azure Active Directory service principal tooread hello nycklar.</span><span class="sxs-lookup"><span data-stu-id="d84f8-173">toosuccessfully encrypt or decrypt virtual disks, permissions on hello cryptographic key stored in Key Vault must be set toopermit hello Azure Active Directory service principal tooread hello keys.</span></span> <span data-ttu-id="d84f8-174">Ange behörigheter för ditt Nyckelvalv med [Set AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):</span><span class="sxs-lookup"><span data-stu-id="d84f8-174">Set permissions on your Key Vault with [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "WrapKey" `
    -PermissionsToSecrets "Set"
```


## <a name="create-virtual-machine"></a><span data-ttu-id="d84f8-175">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d84f8-175">Create virtual machine</span></span>
<span data-ttu-id="d84f8-176">tootest Hej krypteringsprocessen, ska vi skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d84f8-176">tootest hello encryption process, let's create a VM.</span></span> <span data-ttu-id="d84f8-177">hello följande exempel skapas en virtuell dator med namnet *myVM* med hjälp av en *Windows Server 2016 Datacenter* avbildningen:</span><span class="sxs-lookup"><span data-stu-id="d84f8-177">hello following example creates a VM named *myVM* using a *Windows Server 2016 Datacenter* image:</span></span>

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


## <a name="encrypt-virtual-machine"></a><span data-ttu-id="d84f8-178">Kryptera en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d84f8-178">Encrypt virtual machine</span></span>
<span data-ttu-id="d84f8-179">tooencrypt hello virtuella diskar kan du sammanföra alla tidigare hello-komponenter:</span><span class="sxs-lookup"><span data-stu-id="d84f8-179">tooencrypt hello virtual disks, you bring together all hello previous components:</span></span>

1. <span data-ttu-id="d84f8-180">Ange hello Azure Active Directory-tjänstens huvudnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="d84f8-180">Specify hello Azure Active Directory service principal and password.</span></span>
2. <span data-ttu-id="d84f8-181">Ange hello Key Vault toostore hello metadata för krypterade diskarna.</span><span class="sxs-lookup"><span data-stu-id="d84f8-181">Specify hello Key Vault toostore hello metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="d84f8-182">Ange hello krypteringsnycklar toouse för hello faktiska kryptering och dekryptering.</span><span class="sxs-lookup"><span data-stu-id="d84f8-182">Specify hello cryptographic keys toouse for hello actual encryption and decryption.</span></span>
4. <span data-ttu-id="d84f8-183">Ange om du vill tooencrypt hello OS-disk, hello datadiskar eller alla.</span><span class="sxs-lookup"><span data-stu-id="d84f8-183">Specify whether you want tooencrypt hello OS disk, hello data disks, or all.</span></span>

<span data-ttu-id="d84f8-184">Kryptera din virtuella dator med [Set AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) med hello Azure Key Vault-nyckel och autentiseringsuppgifter för Azure Active Directory-UPN.</span><span class="sxs-lookup"><span data-stu-id="d84f8-184">Encrypt your VM with [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) using hello Azure Key Vault key and Azure Active Directory service principal credentials.</span></span> <span data-ttu-id="d84f8-185">hello följande exempel hämtas alla hello viktig information och sedan krypterar hello virtuella datorn med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="d84f8-185">hello following example retrieves all hello key information then encrypts hello VM named *myVM*:</span></span>

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

<span data-ttu-id="d84f8-186">Acceptera hello fråga toocontinue med hello VM-kryptering.</span><span class="sxs-lookup"><span data-stu-id="d84f8-186">Accept hello prompt toocontinue with hello VM encryption.</span></span> <span data-ttu-id="d84f8-187">hello VM startas om under hello-processen.</span><span class="sxs-lookup"><span data-stu-id="d84f8-187">hello VM restarts during hello process.</span></span> <span data-ttu-id="d84f8-188">När hello krypteringsprocessen har slutförts och hello VM har startats om, granska hello krypteringsstatus med [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):</span><span class="sxs-lookup"><span data-stu-id="d84f8-188">Once hello encryption process completes and hello VM has rebooted, review hello encryption status with [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):</span></span>

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
```

<span data-ttu-id="d84f8-189">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="d84f8-189">hello output is similar toohello following example:</span></span>

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a><span data-ttu-id="d84f8-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d84f8-190">Next steps</span></span>
* <span data-ttu-id="d84f8-191">Mer information om hur du hanterar Azure Key Vault finns [konfigurera Key Vault för virtuella datorer](key-vault-setup.md).</span><span class="sxs-lookup"><span data-stu-id="d84f8-191">For more information about managing Azure Key Vault, see [Set up Key Vault for virtual machines](key-vault-setup.md).</span></span>
* <span data-ttu-id="d84f8-192">Läs mer om diskkryptering, till exempel förbereda en krypterad anpassade VM tooupload tooAzure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="d84f8-192">For more information about disk encryption, such as preparing an encrypted custom VM tooupload tooAzure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
