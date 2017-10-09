---
title: "aaaEncrypt diskar på en Linux-VM med hello Azure CLI 1.0 | Microsoft Docs"
description: "Hur tooencrypt diskar på en Linux VM som använder hello Azure CLI 1.0 och hello Resource Manager-distributionsmodellen"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/06/2017
ms.author: iainfou
ms.openlocfilehash: 68a0394d366c3c6941e2c6db0d4263123f951946
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-disks-on-a-linux-vm-using-hello-azure-cli-10"></a><span data-ttu-id="ccdf4-103">Kryptera diskar på en Linux VM som använder hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="ccdf4-103">Encrypt disks on a Linux VM using hello Azure CLI 1.0</span></span>
<span data-ttu-id="ccdf4-104">För förbättrad virtuell dator (VM) säkerhet och efterlevnad, kan virtuella diskar i Azure krypteras i vila.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted at rest.</span></span> <span data-ttu-id="ccdf4-105">Diskar krypteras med kryptografiska nycklar som skyddas i ett Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="ccdf4-106">Du styr dessa kryptografiska nycklar och granska deras användning.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="ccdf4-107">Den här artikeln beskrivs hur tooencrypt virtuella diskar på en Linux VM som använder hello Azure CLI 1.0 och hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-107">This article details how tooencrypt virtual disks on a Linux VM using hello Azure CLI 1.0 and hello Resource Manager deployment model.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="ccdf4-108">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="ccdf4-108">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="ccdf4-109">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-109">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="ccdf4-110">[Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="ccdf4-110">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="ccdf4-111">[Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="ccdf4-111">[Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="quick-commands"></a><span data-ttu-id="ccdf4-112">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="ccdf4-112">Quick commands</span></span>
<span data-ttu-id="ccdf4-113">Om du behöver tooquickly utföra hello uppgiften, hello följande avsnitt information hello grundläggande kommandon tooencrypt virtuella diskar på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-113">If you need tooquickly accomplish hello task, hello following section details hello base commands tooencrypt virtual disks on your VM.</span></span> <span data-ttu-id="ccdf4-114">Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](#overview-of-disk-encryption).</span><span class="sxs-lookup"><span data-stu-id="ccdf4-114">More detailed information and context for each step can be found hello rest of hello document, [starting here](#overview-of-disk-encryption).</span></span>

<span data-ttu-id="ccdf4-115">Du behöver hello [senaste Azure CLI 1.0](../../xplat-cli-install.md) installerad och logga in med hello Resource Manager-läge på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-115">You need hello [latest Azure CLI 1.0](../../xplat-cli-install.md) installed and logged in using hello Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="ccdf4-116">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-116">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="ccdf4-117">Exempel parameternamn inkluderar `myResourceGroup`, `myKeyVault`, och `myVM`.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-117">Example parameter names include `myResourceGroup`, `myKeyVault`, and `myVM`.</span></span>

<span data-ttu-id="ccdf4-118">Först aktivera hello Azure Key Vault-providern i din Azure-prenumeration och skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-118">First, enable hello Azure Key Vault provider within your Azure subscription and create a resource group.</span></span> <span data-ttu-id="ccdf4-119">hello följande exempel skapas ett Resursgruppsnamn `myResourceGroup` i hello `WestUS` plats:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-119">hello following example creates a resource group name `myResourceGroup` in hello `WestUS` location:</span></span>

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

<span data-ttu-id="ccdf4-120">Skapa ett Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-120">Create an Azure Key Vault.</span></span> <span data-ttu-id="ccdf4-121">hello följande exempel skapas ett Nyckelvalv med namnet `myKeyVault`:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-121">hello following example creates a Key Vault named `myKeyVault`:</span></span>

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

<span data-ttu-id="ccdf4-122">Skapa en kryptografisk nyckel i ditt Nyckelvalv och aktivera kryptering.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-122">Create a cryptographic key in your Key Vault and enable it for disk encryption.</span></span> <span data-ttu-id="ccdf4-123">hello följande exempel skapas en nyckel som heter `myKey`:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-123">hello following example creates a key named `myKey`:</span></span>

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

<span data-ttu-id="ccdf4-124">Skapa en slutpunkt som använder Azure Active Directory för att hantera hello autentisering och byta ut kryptografiska nycklar från Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-124">Create an endpoint using Azure Active Directory for handling hello authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="ccdf4-125">Hej `--home-page` och `--identifier-uris` behöver inte toobe faktiska dirigerbara adresser.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-125">hello `--home-page` and `--identifier-uris` do not need toobe actual routable address.</span></span> <span data-ttu-id="ccdf4-126">Hello högsta säkerhetsnivå, ska klienthemlighet användas i stället för lösenord.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-126">For hello highest level of security, client secrets should be used instead of passwords.</span></span> <span data-ttu-id="ccdf4-127">hello Azure CLI generera inte för närvarande klienthemlighet.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-127">hello Azure CLI cannot currently generate client secrets.</span></span> <span data-ttu-id="ccdf4-128">Klienthemlighet kan bara skapas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-128">Client secrets can only be generated in hello Azure portal.</span></span> <span data-ttu-id="ccdf4-129">hello följande exempel skapas en Azure Active Directory-slutpunkt med namnet `myAADApp` och använder ett lösenord på `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-129">hello following example creates an Azure Active Directory endpoint named `myAADApp` and uses a password of `myPassword`.</span></span> <span data-ttu-id="ccdf4-130">Ange ditt eget lösenord på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-130">Specify your own password as follows:</span></span>

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

<span data-ttu-id="ccdf4-131">Obs hello `applicationId` visas i hello utdata från hello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-131">Note hello `applicationId` shown in hello output from hello preceding command.</span></span> <span data-ttu-id="ccdf4-132">Program-ID används i hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-132">This application ID is used in hello following steps:</span></span>

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

<span data-ttu-id="ccdf4-133">Lägg till en befintlig virtuell dator i data disk tooan.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-133">Add a data disk tooan existing VM.</span></span> <span data-ttu-id="ccdf4-134">hello följande exempel lägger till en data disk tooa virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-134">hello following example adds a data disk tooa VM named `myVM`:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="ccdf4-135">Granska hello detaljer för din Key Vault och hello nyckel som du skapade.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-135">Review hello details for your Key Vault and hello key you created.</span></span> <span data-ttu-id="ccdf4-136">Du behöver hello Key Vault-ID, URI och nyckel URL: en i hello sista steget.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-136">You need hello Key Vault ID, URI, and key URL in hello final step.</span></span> <span data-ttu-id="ccdf4-137">hello följande exempel granskar hello information för ett Nyckelvalv med namnet `myKeyVault` och nyckel som heter `myKey`:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-137">hello following example reviews hello details for a Key Vault named `myKeyVault` and key named `myKey`:</span></span>

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

<span data-ttu-id="ccdf4-138">Kryptera din diskar på följande sätt att ange egna parameternamn i hela:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-138">Encrypt your disks as follows, entering your own parameter names throughout:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

<span data-ttu-id="ccdf4-139">hello Azure CLI anger inte utförlig fel under hello krypteringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-139">hello Azure CLI doesn't provide verbose errors during hello encryption process.</span></span> <span data-ttu-id="ccdf4-140">Ytterligare felsökningsinformation `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-140">For additional troubleshooting information, review `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`.</span></span> <span data-ttu-id="ccdf4-141">Föregående kommando har många variabler som hello och kan ge mycket uppgift som toowhy hello misslyckas, blir fullständiga exemplet som följer:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-141">As hello preceding command has many variables and you may not get much indication as toowhy hello process fails, a complete command example would be as follows:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

<span data-ttu-id="ccdf4-142">Slutligen granska hello krypteringsstatus igen tooconfirm virtuella diskarna har nu krypterats.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-142">Finally, review hello encryption status again tooconfirm that your virtual disks have now been encrypted.</span></span> <span data-ttu-id="ccdf4-143">hello följande exempel kontrollerar hello status för en virtuell dator med namnet `myVM` i hello `myResourceGroup` resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-143">hello following example checks hello status of a VM named `myVM` in hello `myResourceGroup` resource group:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="ccdf4-144">Översikt över diskkryptering</span><span class="sxs-lookup"><span data-stu-id="ccdf4-144">Overview of disk encryption</span></span>
<span data-ttu-id="ccdf4-145">Virtuella diskar på virtuella Linux-datorer krypteras på resten med hjälp av [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span><span class="sxs-lookup"><span data-stu-id="ccdf4-145">Virtual disks on Linux VMs are encrypted at rest using [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span></span> <span data-ttu-id="ccdf4-146">Det är gratis för kryptering av virtuella diskar i Azure.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-146">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="ccdf4-147">Kryptografiska nycklar lagras i Azure Key Vault med skydd av programvara eller du kan importera eller generera dina nycklar i Maskinvarusäkerhetsmoduler (HSM) certifierade tooFIPS 140-2 level 2-standarder.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-147">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified tooFIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="ccdf4-148">Du behålla kontrollen över dessa kryptografiska nycklar och granska deras användning.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-148">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="ccdf4-149">Dessa kryptografiska nycklar används tooencrypt och dekryptera virtuella diskar anslutna tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-149">These cryptographic keys are used tooencrypt and decrypt virtual disks attached tooyour VM.</span></span> <span data-ttu-id="ccdf4-150">En slutpunkt för Azure Active Directory ger en säker mekanism för att utfärda de kryptografiska nycklarna som virtuella datorer är igång på och stänga av.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-150">An Azure Active Directory endpoint provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="ccdf4-151">hello-processen för att kryptera en virtuell dator är som följer:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-151">hello process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="ccdf4-152">Skapa en kryptografisk nyckel i en Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-152">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="ccdf4-153">Konfigurera hello kryptografiska nyckel toobe kan användas för att kryptera diskar.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-153">Configure hello cryptographic key toobe usable for encrypting disks.</span></span>
3. <span data-ttu-id="ccdf4-154">tooread hello krypteringsnyckeln från hello Azure Key Vault skapa en slutpunkt som använder Azure Active Directory med hello lämpliga behörigheter.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-154">tooread hello cryptographic key from hello Azure Key Vault, create an endpoint using Azure Active Directory with hello appropriate permissions.</span></span>
4. <span data-ttu-id="ccdf4-155">Utfärda hello kommandot tooencrypt din virtuella diskar, att ange hello Azure Active Directory-slutpunkten och lämpliga kryptografiska nyckel toobe används.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-155">Issue hello command tooencrypt your virtual disks, specifying hello Azure Active Directory endpoint and appropriate cryptographic key toobe used.</span></span>
5. <span data-ttu-id="ccdf4-156">hello Azure Active Directory endpoint begär hello nödvändig kryptografiska nyckel från Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-156">hello Azure Active Directory endpoint requests hello required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="ccdf4-157">hello virtuella diskar krypteras med hello tillhandahålls kryptografisk nyckel.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-157">hello virtual disks are encrypted using hello provided cryptographic key.</span></span>

## <a name="supporting-services-and-encryption-process"></a><span data-ttu-id="ccdf4-158">Stöd för tjänster och kryptering</span><span class="sxs-lookup"><span data-stu-id="ccdf4-158">Supporting services and encryption process</span></span>
<span data-ttu-id="ccdf4-159">Kryptering är beroende av hello följande ytterligare komponenter:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-159">Disk encryption relies on hello following additional components:</span></span>

* <span data-ttu-id="ccdf4-160">**Azure Key Vault** -används toosafeguard kryptografiska nycklar och hemligheter som används för hello disk kryptering/dekryptering process.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-160">**Azure Key Vault** - used toosafeguard cryptographic keys and secrets used for hello disk encryption/decryption process.</span></span>
  * <span data-ttu-id="ccdf4-161">Om det finns ett kan du använda en befintlig Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-161">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="ccdf4-162">Du har inte toodedicate en Key Vault tooencrypting diskar.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-162">You do not have toodedicate a Key Vault tooencrypting disks.</span></span>
  * <span data-ttu-id="ccdf4-163">Du kan skapa ett dedikerat Nyckelvalv tooseparate administrativa gränser och viktiga synlighet.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-163">tooseparate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="ccdf4-164">**Azure Active Directory** - referenser hello säker utbyte av kryptografiska nycklar som krävs och autentisering för begärt åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-164">**Azure Active Directory** - handles hello secure exchanging of required cryptographic keys and authentication for requested actions.</span></span>
  * <span data-ttu-id="ccdf4-165">Vanligtvis kan du använda en befintlig instans av Azure Active Directory för ditt program.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-165">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="ccdf4-166">hello program är en slutpunkt för hello Nyckelvalvet och virtuella services toorequest och hämta utfärdade hello lämpliga kryptografiska nycklar.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-166">hello application is more of an endpoint for hello Key Vault and Virtual Machine services toorequest and get issued hello appropriate cryptographic keys.</span></span> <span data-ttu-id="ccdf4-167">Du utvecklar inte ett verkligt program som integreras med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-167">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="ccdf4-168">Krav och begränsningar</span><span class="sxs-lookup"><span data-stu-id="ccdf4-168">Requirements and limitations</span></span>
<span data-ttu-id="ccdf4-169">Krav för diskkryptering och scenarier som stöds:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-169">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="ccdf4-170">hello följande Linux server SKU - Ubuntu, CentOS, SUSE och SUSE Linux Enterprise Server (SLES) och Red Hat Enterprise Linux.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-170">hello following Linux server SKUs - Ubuntu, CentOS, SUSE and SUSE Linux Enterprise Server (SLES), and Red Hat Enterprise Linux.</span></span>
* <span data-ttu-id="ccdf4-171">Alla resurser (till exempel Nyckelvalv lagringskonto och VM) måste vara i hello samma Azure-region och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-171">All resources (such as Key Vault, Storage account, and VM) must be in hello same Azure region and subscription.</span></span>
* <span data-ttu-id="ccdf4-172">Standard A, D, DS, G och GS-serien virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-172">Standard A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="ccdf4-173">Diskkryptering stöds inte för närvarande i hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-173">Disk encryption is not currently supported in hello following scenarios:</span></span>

* <span data-ttu-id="ccdf4-174">Grundnivån virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-174">Basic tier VMs.</span></span>
* <span data-ttu-id="ccdf4-175">Virtuella datorer skapas med hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-175">VMs created using hello Classic deployment model.</span></span>
* <span data-ttu-id="ccdf4-176">Om du inaktiverar kryptering för OS-disk på virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-176">Disabling OS disk encryption on Linux VMs.</span></span>
* <span data-ttu-id="ccdf4-177">Uppdaterar hello Kryptografiska nycklar på en redan krypterade Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-177">Updating hello cryptographic keys on an already encrypted Linux VM.</span></span>

## <a name="create-hello-azure-key-vault-and-keys"></a><span data-ttu-id="ccdf4-178">Skapa hello Azure Key Vault och nycklar</span><span class="sxs-lookup"><span data-stu-id="ccdf4-178">Create hello Azure Key Vault and keys</span></span>
<span data-ttu-id="ccdf4-179">toocomplete hello resten av den här guiden, måste hello [senaste Azure CLI 1.0](../../xplat-cli-install.md) installerad och logga in med hello Resource Manager-läge på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-179">toocomplete hello remainder of this guide, you need hello [latest Azure CLI 1.0](../../xplat-cli-install.md) installed and logged in using hello Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="ccdf4-180">Ersätt alla exempel parametrar med egna namn, plats och nyckelvärden i hela hello kommandoexempel.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-180">Throughout hello command examples, replace all example parameters with your own names, location, and key values.</span></span> <span data-ttu-id="ccdf4-181">hello följande exempel används en konvention för `myResourceGroup`, `myKeyVault`, `myAADApp`osv.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-181">hello following examples use a convention of `myResourceGroup`, `myKeyVault`, `myAADApp`, etc.</span></span>

<span data-ttu-id="ccdf4-182">hello första steget är toocreate ett Azure Key Vault toostore kryptografiska nycklar.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-182">hello first step is toocreate an Azure Key Vault toostore your cryptographic keys.</span></span> <span data-ttu-id="ccdf4-183">Azure Key Vault kan lagra nycklar, hemligheter, eller lösenord som gör att du toosecurely implementeras i dina program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-183">Azure Key Vault can store keys, secrets, or passwords that allow you toosecurely implement them in your applications and services.</span></span> <span data-ttu-id="ccdf4-184">För virtuell diskkryptering, använda Key Vault toostore en krypteringsnyckel som används tooencrypt eller dekryptera din virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-184">For virtual disk encryption, you use Key Vault toostore a cryptographic key that is used tooencrypt or decrypt your virtual disks.</span></span>

<span data-ttu-id="ccdf4-185">Aktivera hello Azure Key Vault-providern i din Azure-prenumeration och sedan skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-185">Enable hello Azure Key Vault provider in your Azure subscription, then create a resource group.</span></span> <span data-ttu-id="ccdf4-186">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `WestUS` plats:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-186">hello following example creates a resource group named `myResourceGroup` in hello `WestUS` location:</span></span>

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

<span data-ttu-id="ccdf4-187">hello Azure Key Vault som innehåller hello krypteringsnycklar och associerade beräkning resurser, till exempel lagring och hello Virtuella datorn måste finnas i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-187">hello Azure Key Vault containing hello cryptographic keys and associated compute resources such as storage and hello VM itself must reside in hello same region.</span></span> <span data-ttu-id="ccdf4-188">hello följande exempel skapas en Azure Key Vault som heter `myKeyVault`:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-188">hello following example creates an Azure Key Vault named `myKeyVault`:</span></span>

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

<span data-ttu-id="ccdf4-189">Du kan lagra kryptografiska nycklar med hjälp av programvara eller maskinvara säkerhet modellen (HSM)-skydd.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-189">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="ccdf4-190">Använda en HSM kräver en premium Key Vault.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-190">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="ccdf4-191">Det finns ett extra kostnad toocreating bidrag Key Vault snarare än standard Key Vault som lagrar programvara-skyddade nycklar.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-191">There is an additional cost toocreating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="ccdf4-192">toocreate lägga till en premium Key Vault i föregående steg hello `--sku Premium` toohello kommando.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-192">toocreate a premium Key Vault, in hello preceding step add `--sku Premium` toohello command.</span></span> <span data-ttu-id="ccdf4-193">hello följande exempel används programvaruskyddad nycklar eftersom vi har skapat ett Nyckelvalv som standard.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-193">hello following example uses software-protected keys since we created a standard Key Vault.</span></span>

<span data-ttu-id="ccdf4-194">För båda modellerna skydd måste hello Azure-plattformen toobe beviljas åtkomst toorequest hello Kryptografiska nycklar när hello VM startas toodecrypt hello virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-194">For both protection models, hello Azure platform needs toobe granted access toorequest hello cryptographic keys when hello VM boots toodecrypt hello virtual disks.</span></span> <span data-ttu-id="ccdf4-195">Skapa en krypteringsnyckel i ditt Nyckelvalv och sedan aktivera den för användning med virtuell diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-195">Create an encryption key within your Key Vault, then enable it for use with virtual disk encryption.</span></span> <span data-ttu-id="ccdf4-196">hello följande exempel skapas en nyckel som heter `myKey` och gör det möjligt för diskkryptering:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-196">hello following example creates a key named `myKey` and then enables it for disk encryption:</span></span>

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-hello-azure-active-directory-application"></a><span data-ttu-id="ccdf4-197">Skapa hello Azure Active Directory-program</span><span class="sxs-lookup"><span data-stu-id="ccdf4-197">Create hello Azure Active Directory application</span></span>
<span data-ttu-id="ccdf4-198">När virtuella diskar krypteras och dekrypteras, använder du en slutpunkt toohandle hello autentisering och byta ut kryptografiska nycklar från Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-198">When virtual disks are encrypted or decrypted, you use an endpoint toohandle hello authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="ccdf4-199">Den här slutpunkten måste ett Azure Active Directory-program kan hello Azure-plattformen toorequest hello lämpliga krypteringsnycklar uppdrag hello VM.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-199">This endpoint, an Azure Active Directory application, allows hello Azure platform toorequest hello appropriate cryptographic keys on behalf of hello VM.</span></span> <span data-ttu-id="ccdf4-200">En standardinstans för Azure Active Directory finns i din prenumeration, även om många organisationer har särskilda Azure Active Directory-kataloger.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-200">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="ccdf4-201">Eftersom du inte skapar en fullständig Azure Active Directory-programmet hello `--home-page` och `--identifier-uris` parametrar i följande exempel hello behöver inte toobe faktiska dirigerbara adresser.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-201">As you are not creating a full Azure Active Directory application, hello `--home-page` and `--identifier-uris` parameters in hello following example do not need toobe actual routable address.</span></span> <span data-ttu-id="ccdf4-202">hello anger följande exempel också en lösenordsbaserad hemlighet i stället för att nycklarna från i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-202">hello following example also specifies a password-based secret rather than generating keys from within hello Azure portal.</span></span> <span data-ttu-id="ccdf4-203">Som den här tiden kan kan generera nycklar inte utföras från hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-203">As this time, generating keys cannot be done from hello Azure CLI.</span></span>

<span data-ttu-id="ccdf4-204">Skapa Azure Active Directory-program.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-204">Create your Azure Active Directory application.</span></span> <span data-ttu-id="ccdf4-205">hello följande exempel skapar ett program med namnet `myAADApp` och använder ett lösenord på `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-205">hello following example creates an application named `myAADApp` and uses a password of `myPassword`.</span></span> <span data-ttu-id="ccdf4-206">Ange ditt eget lösenord på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-206">Specify your own password as follows:</span></span>

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

<span data-ttu-id="ccdf4-207">Anteckna hello `applicationId` som returneras i hello utdata från hello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-207">Make a note of hello `applicationId` that is returned in hello output from hello preceding command.</span></span> <span data-ttu-id="ccdf4-208">Program-ID används i vissa hello återstående steg.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-208">This application ID is used in some of hello remaining steps.</span></span> <span data-ttu-id="ccdf4-209">Skapa sedan en tjänstens huvudnamn (SPN) så att programmet hello är tillgänglig i din miljö.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-209">Next, create a service principal name (SPN) so that hello application is accessible within your environment.</span></span> <span data-ttu-id="ccdf4-210">toosuccessfully kryptera eller dekryptera virtuella diskar, behörigheter på hello kryptografiska nyckel som lagras i Nyckelvalvet måste vara set toopermit hello Azure Active Directory application tooread hello nycklar.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-210">toosuccessfully encrypt or decrypt virtual disks, permissions on hello cryptographic key stored in Key Vault must be set toopermit hello Azure Active Directory application tooread hello keys.</span></span>

<span data-ttu-id="ccdf4-211">Skapa hello SPN och ange hello lämpliga behörigheter på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-211">Create hello SPN and set hello appropriate permissions as follows:</span></span>

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a><span data-ttu-id="ccdf4-212">Lägg till en virtuell disk och granska krypteringsstatus</span><span class="sxs-lookup"><span data-stu-id="ccdf4-212">Add a virtual disk and review encryption status</span></span>
<span data-ttu-id="ccdf4-213">tooactually kryptera vissa virtuella diskar, kan du lägga till en disk tooan befintliga VM.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-213">tooactually encrypt some virtual disks, lets add a disk tooan existing VM.</span></span> <span data-ttu-id="ccdf4-214">Lägg till 5Gb data disk tooan befintlig virtuell dator på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-214">Add a 5Gb data disk tooan existing VM as follows:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="ccdf4-215">hello virtuella diskar krypteras inte.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-215">hello virtual disks are not currently encrypted.</span></span> <span data-ttu-id="ccdf4-216">Granska hello aktuella krypteringsstatus för den virtuella datorn på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-216">Review hello current encryption status of your VM as follows:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a><span data-ttu-id="ccdf4-217">Kryptera virtuella diskar</span><span class="sxs-lookup"><span data-stu-id="ccdf4-217">Encrypt virtual disks</span></span>
<span data-ttu-id="ccdf4-218">toonow kryptera hello virtuella diskar, du samordnar alla hello tidigare komponenter:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-218">toonow encrypt hello virtual disks, you bring together all hello previous components:</span></span>

1. <span data-ttu-id="ccdf4-219">Ange hello Azure Active Directory-program och lösenord.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-219">Specify hello Azure Active Directory application and password.</span></span>
2. <span data-ttu-id="ccdf4-220">Ange hello Key Vault toostore hello metadata för krypterade diskarna.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-220">Specify hello Key Vault toostore hello metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="ccdf4-221">Ange hello krypteringsnycklar toouse för hello faktiska kryptering och dekryptering.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-221">Specify hello cryptographic keys toouse for hello actual encryption and decryption.</span></span>
4. <span data-ttu-id="ccdf4-222">Ange om du vill tooencrypt hello OS-disk, hello datadiskar eller alla.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-222">Specify whether you want tooencrypt hello OS disk, hello data disks, or all.</span></span>

<span data-ttu-id="ccdf4-223">Kan granska hello detaljer för din Azure Key Vault och hello nyckel som du skapade när du behöver hello Key Vault-ID, URI, och ange sedan URL hello sista steget:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-223">Lets review hello details for your Azure Key Vault and hello key you created, as you need hello Key Vault ID, URI, and then key URL in hello final step:</span></span>

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

<span data-ttu-id="ccdf4-224">Kryptera din virtuella diskar med hjälp av hello utdata från hello `azure keyvault show` och `azure keyvault key show` kommandon på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-224">Encrypt your virtual disks using hello output from hello `azure keyvault show` and `azure keyvault key show` commands as follows:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

<span data-ttu-id="ccdf4-225">Som hello föregående kommando har många variabler, hello följande exempel är hello fullständiga kommandot referens:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-225">As hello preceding command has many variables, hello following example is hello complete command for reference:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

<span data-ttu-id="ccdf4-226">hello Azure CLI anger inte utförlig fel under hello krypteringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-226">hello Azure CLI doesn't provide verbose errors during hello encryption process.</span></span> <span data-ttu-id="ccdf4-227">Ytterligare felsökningsinformation `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` på hello VM som du krypterar.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-227">For additional troubleshooting information, review `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` on hello VM you are encrypting.</span></span>

<span data-ttu-id="ccdf4-228">Slutligen kan granska hello krypteringsstatus igen tooconfirm virtuella diskarna har nu krypterats:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-228">Finally, lets review hello encryption status again tooconfirm that your virtual disks have now been encrypted:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a><span data-ttu-id="ccdf4-229">Lägg till ytterligare datadiskar</span><span class="sxs-lookup"><span data-stu-id="ccdf4-229">Add additional data disks</span></span>
<span data-ttu-id="ccdf4-230">När du har krypterade datadiskarna kan du också kryptera dem senare lägga till ytterligare virtuella diskar tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-230">Once you have encrypted your data disks, you can later add additional virtual disks tooyour VM and also encrypt them.</span></span> <span data-ttu-id="ccdf4-231">När du kör hello `azure vm enable-disk-encryption` kommandot, öka hello sekvens version med hello `--sequence-version` parameter.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-231">When you run hello `azure vm enable-disk-encryption` command, increment hello sequence version using hello `--sequence-version` parameter.</span></span> <span data-ttu-id="ccdf4-232">Den här aktivitetssekvensen versionsparametern kan du tooperform upprepade åtgärder på hello samma virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="ccdf4-232">This sequence version parameter allows you tooperform repeated operations on hello same VM.</span></span>

<span data-ttu-id="ccdf4-233">Exempelvis kan du lägga till en andra virtuell disk tooyour VM enligt följande:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-233">For example, lets add a second virtual disk tooyour VM as follows:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="ccdf4-234">Kör hello kommandot tooencrypt hello virtuella diskar, nu lägga till hello `--sequence-version` parameter och ökar hello värde från våra första kör på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ccdf4-234">Rerun hello command tooencrypt hello virtual disks, this time adding hello `--sequence-version` parameter, and incrementing hello value from our first run as follows:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a><span data-ttu-id="ccdf4-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ccdf4-235">Next steps</span></span>
* <span data-ttu-id="ccdf4-236">Läs mer om hur du hanterar Azure Key Vault, inklusive ta bort krypteringsnycklarna och valv, [hantera Key Vault med hjälp av CLI](../../key-vault/key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="ccdf4-236">For more information about managing Azure Key Vault, including deleting cryptographic keys and vaults, see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md).</span></span>
* <span data-ttu-id="ccdf4-237">Läs mer om diskkryptering, till exempel förbereda en krypterad anpassade VM tooupload tooAzure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="ccdf4-237">For more information about disk encryption, such as preparing an encrypted custom VM tooupload tooAzure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
