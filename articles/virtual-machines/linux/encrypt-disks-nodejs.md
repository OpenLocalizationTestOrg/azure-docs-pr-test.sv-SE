---
title: "Kryptera diskar på en Linux-VM med Azure CLI 1.0 | Microsoft Docs"
description: "Hur du krypterar diskar på en Linux VM som använder Azure CLI 1.0 och Resource Manager-distributionsmodellen"
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
ms.openlocfilehash: b436f2d43c41000f4385889edb3fa3983d4a8c66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli-10"></a><span data-ttu-id="70f4d-103">Kryptera diskar på en Linux VM som använder Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="70f4d-103">Encrypt disks on a Linux VM using the Azure CLI 1.0</span></span>
<span data-ttu-id="70f4d-104">För förbättrad virtuell dator (VM) säkerhet och efterlevnad, kan virtuella diskar i Azure krypteras i vila.</span><span class="sxs-lookup"><span data-stu-id="70f4d-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted at rest.</span></span> <span data-ttu-id="70f4d-105">Diskar krypteras med kryptografiska nycklar som skyddas i ett Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="70f4d-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="70f4d-106">Du styr dessa kryptografiska nycklar och granska deras användning.</span><span class="sxs-lookup"><span data-stu-id="70f4d-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="70f4d-107">Den här artikeln beskrivs hur du krypterar virtuella diskar på en Linux VM som använder Azure CLI 1.0 och Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="70f4d-107">This article details how to encrypt virtual disks on a Linux VM using the Azure CLI 1.0 and the Resource Manager deployment model.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="70f4d-108">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="70f4d-108">CLI versions to complete the task</span></span>
<span data-ttu-id="70f4d-109">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="70f4d-109">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="70f4d-110">[Azure CLI 1.0](#quick-commands) – våra CLI för klassisk och resurs management på distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="70f4d-110">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="70f4d-111">[Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="70f4d-111">[Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>

## <a name="quick-commands"></a><span data-ttu-id="70f4d-112">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="70f4d-112">Quick commands</span></span>
<span data-ttu-id="70f4d-113">Om du behöver utföra aktiviteten följande avsnittet beskriver grundläggande kommandon för att kryptera virtuella diskar på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="70f4d-113">If you need to quickly accomplish the task, the following section details the base commands to encrypt virtual disks on your VM.</span></span> <span data-ttu-id="70f4d-114">Mer detaljerad information och kontext för varje steg finns resten av dokumentet [startar här](#overview-of-disk-encryption).</span><span class="sxs-lookup"><span data-stu-id="70f4d-114">More detailed information and context for each step can be found the rest of the document, [starting here](#overview-of-disk-encryption).</span></span>

<span data-ttu-id="70f4d-115">Du behöver den [senaste Azure CLI 1.0](../../xplat-cli-install.md) installerad och logga in med Resource Manager-läget på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70f4d-115">You need the [latest Azure CLI 1.0](../../xplat-cli-install.md) installed and logged in using the Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="70f4d-116">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="70f4d-116">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="70f4d-117">Exempel parameternamn inkluderar `myResourceGroup`, `myKeyVault`, och `myVM`.</span><span class="sxs-lookup"><span data-stu-id="70f4d-117">Example parameter names include `myResourceGroup`, `myKeyVault`, and `myVM`.</span></span>

<span data-ttu-id="70f4d-118">Först aktivera Azure Key Vault-providern i din Azure-prenumeration och skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="70f4d-118">First, enable the Azure Key Vault provider within your Azure subscription and create a resource group.</span></span> <span data-ttu-id="70f4d-119">I följande exempel skapas ett Resursgruppsnamn `myResourceGroup` i den `WestUS` plats:</span><span class="sxs-lookup"><span data-stu-id="70f4d-119">The following example creates a resource group name `myResourceGroup` in the `WestUS` location:</span></span>

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

<span data-ttu-id="70f4d-120">Skapa ett Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="70f4d-120">Create an Azure Key Vault.</span></span> <span data-ttu-id="70f4d-121">I följande exempel skapas ett Nyckelvalv med namnet `myKeyVault`:</span><span class="sxs-lookup"><span data-stu-id="70f4d-121">The following example creates a Key Vault named `myKeyVault`:</span></span>

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

<span data-ttu-id="70f4d-122">Skapa en kryptografisk nyckel i ditt Nyckelvalv och aktivera kryptering.</span><span class="sxs-lookup"><span data-stu-id="70f4d-122">Create a cryptographic key in your Key Vault and enable it for disk encryption.</span></span> <span data-ttu-id="70f4d-123">I följande exempel skapas en nyckel som heter `myKey`:</span><span class="sxs-lookup"><span data-stu-id="70f4d-123">The following example creates a key named `myKey`:</span></span>

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

<span data-ttu-id="70f4d-124">Skapa en slutpunkt som använder Azure Active Directory för att hantera autentisering och byta ut kryptografiska nycklar från Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="70f4d-124">Create an endpoint using Azure Active Directory for handling the authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="70f4d-125">Den `--home-page` och `--identifier-uris` inte behöver vara faktiska dirigerbara adresser.</span><span class="sxs-lookup"><span data-stu-id="70f4d-125">The `--home-page` and `--identifier-uris` do not need to be actual routable address.</span></span> <span data-ttu-id="70f4d-126">Högsta möjliga säkerhetsnivå ska klienthemlighet användas i stället för lösenord.</span><span class="sxs-lookup"><span data-stu-id="70f4d-126">For the highest level of security, client secrets should be used instead of passwords.</span></span> <span data-ttu-id="70f4d-127">Azure CLI generera inte för närvarande klienthemlighet.</span><span class="sxs-lookup"><span data-stu-id="70f4d-127">The Azure CLI cannot currently generate client secrets.</span></span> <span data-ttu-id="70f4d-128">Klienthemlighet kan bara skapas i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="70f4d-128">Client secrets can only be generated in the Azure portal.</span></span> <span data-ttu-id="70f4d-129">I följande exempel skapas en Azure Active Directory-slutpunkt med namnet `myAADApp` och använder ett lösenord på `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="70f4d-129">The following example creates an Azure Active Directory endpoint named `myAADApp` and uses a password of `myPassword`.</span></span> <span data-ttu-id="70f4d-130">Ange ditt eget lösenord på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70f4d-130">Specify your own password as follows:</span></span>

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

<span data-ttu-id="70f4d-131">Observera den `applicationId` visas i utdata från kommandot ovan.</span><span class="sxs-lookup"><span data-stu-id="70f4d-131">Note the `applicationId` shown in the output from the preceding command.</span></span> <span data-ttu-id="70f4d-132">Program-ID används i följande steg:</span><span class="sxs-lookup"><span data-stu-id="70f4d-132">This application ID is used in the following steps:</span></span>

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

<span data-ttu-id="70f4d-133">Lägg till en datadisk till en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="70f4d-133">Add a data disk to an existing VM.</span></span> <span data-ttu-id="70f4d-134">I följande exempel läggs en datadisk till en virtuell dator med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="70f4d-134">The following example adds a data disk to a VM named `myVM`:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="70f4d-135">Granska informationen för Key Vault och den nyckel som du skapade.</span><span class="sxs-lookup"><span data-stu-id="70f4d-135">Review the details for your Key Vault and the key you created.</span></span> <span data-ttu-id="70f4d-136">Key Vault-ID, URI och nyckel måste URL: en i det sista steget.</span><span class="sxs-lookup"><span data-stu-id="70f4d-136">You need the Key Vault ID, URI, and key URL in the final step.</span></span> <span data-ttu-id="70f4d-137">I följande exempel granskar detaljerna för ett Nyckelvalv med namnet `myKeyVault` och nyckel som heter `myKey`:</span><span class="sxs-lookup"><span data-stu-id="70f4d-137">The following example reviews the details for a Key Vault named `myKeyVault` and key named `myKey`:</span></span>

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

<span data-ttu-id="70f4d-138">Kryptera din diskar på följande sätt att ange egna parameternamn i hela:</span><span class="sxs-lookup"><span data-stu-id="70f4d-138">Encrypt your disks as follows, entering your own parameter names throughout:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

<span data-ttu-id="70f4d-139">Azure CLI anger inte utförlig fel under krypteringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="70f4d-139">The Azure CLI doesn't provide verbose errors during the encryption process.</span></span> <span data-ttu-id="70f4d-140">Ytterligare felsökningsinformation `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`.</span><span class="sxs-lookup"><span data-stu-id="70f4d-140">For additional troubleshooting information, review `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`.</span></span> <span data-ttu-id="70f4d-141">Som föregående kommando har många variabler och kan ge mycket uppgift om varför processen misslyckas, är ett exempel på fullständiga kommandot på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70f4d-141">As the preceding command has many variables and you may not get much indication as to why the process fails, a complete command example would be as follows:</span></span>

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

<span data-ttu-id="70f4d-142">Slutligen granska krypteringsstatus igen för att bekräfta att din virtuella diskar nu har krypterats.</span><span class="sxs-lookup"><span data-stu-id="70f4d-142">Finally, review the encryption status again to confirm that your virtual disks have now been encrypted.</span></span> <span data-ttu-id="70f4d-143">I följande exempel kontrollerar status för en virtuell dator med namnet `myVM` i den `myResourceGroup` resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="70f4d-143">The following example checks the status of a VM named `myVM` in the `myResourceGroup` resource group:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="70f4d-144">Översikt över diskkryptering</span><span class="sxs-lookup"><span data-stu-id="70f4d-144">Overview of disk encryption</span></span>
<span data-ttu-id="70f4d-145">Virtuella diskar på virtuella Linux-datorer krypteras på resten med hjälp av [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span><span class="sxs-lookup"><span data-stu-id="70f4d-145">Virtual disks on Linux VMs are encrypted at rest using [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span></span> <span data-ttu-id="70f4d-146">Det är gratis för kryptering av virtuella diskar i Azure.</span><span class="sxs-lookup"><span data-stu-id="70f4d-146">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="70f4d-147">Kryptografiska nycklar lagras i Azure Key Vault med skydd av programvara eller du kan importera eller generera dina nycklar i Maskinvarusäkerhetsmoduler (HSM) certifierade FIPS 140-2 level 2-standarder.</span><span class="sxs-lookup"><span data-stu-id="70f4d-147">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified to FIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="70f4d-148">Du behålla kontrollen över dessa kryptografiska nycklar och granska deras användning.</span><span class="sxs-lookup"><span data-stu-id="70f4d-148">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="70f4d-149">Dessa kryptografiska nycklar används för att kryptera och dekryptera virtuella diskar som är anslutna till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="70f4d-149">These cryptographic keys are used to encrypt and decrypt virtual disks attached to your VM.</span></span> <span data-ttu-id="70f4d-150">En slutpunkt för Azure Active Directory ger en säker mekanism för att utfärda de kryptografiska nycklarna som virtuella datorer är igång på och stänga av.</span><span class="sxs-lookup"><span data-stu-id="70f4d-150">An Azure Active Directory endpoint provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="70f4d-151">Processen för att kryptera en virtuell dator är följande:</span><span class="sxs-lookup"><span data-stu-id="70f4d-151">The process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="70f4d-152">Skapa en kryptografisk nyckel i en Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="70f4d-152">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="70f4d-153">Konfigurera den kryptografiska nyckeln som kan användas för att kryptera diskar.</span><span class="sxs-lookup"><span data-stu-id="70f4d-153">Configure the cryptographic key to be usable for encrypting disks.</span></span>
3. <span data-ttu-id="70f4d-154">Skapa en slutpunkt som använder Azure Active Directory med rätt behörighet för att läsa den kryptografiska nyckeln från Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="70f4d-154">To read the cryptographic key from the Azure Key Vault, create an endpoint using Azure Active Directory with the appropriate permissions.</span></span>
4. <span data-ttu-id="70f4d-155">Utfärda kommandot för att kryptera din virtuella diskar, Azure Active Directory-slutpunkten och lämpliga kryptografiska nyckel som ska användas.</span><span class="sxs-lookup"><span data-stu-id="70f4d-155">Issue the command to encrypt your virtual disks, specifying the Azure Active Directory endpoint and appropriate cryptographic key to be used.</span></span>
5. <span data-ttu-id="70f4d-156">Azure Active Directory-slutpunkten begär den kryptografiska nyckeln som krävs från Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="70f4d-156">The Azure Active Directory endpoint requests the required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="70f4d-157">De virtuella diskarna krypteras med den angivna kryptografiska nyckeln.</span><span class="sxs-lookup"><span data-stu-id="70f4d-157">The virtual disks are encrypted using the provided cryptographic key.</span></span>

## <a name="supporting-services-and-encryption-process"></a><span data-ttu-id="70f4d-158">Stöd för tjänster och kryptering</span><span class="sxs-lookup"><span data-stu-id="70f4d-158">Supporting services and encryption process</span></span>
<span data-ttu-id="70f4d-159">Kryptering är beroende av följande ytterligare komponenter:</span><span class="sxs-lookup"><span data-stu-id="70f4d-159">Disk encryption relies on the following additional components:</span></span>

* <span data-ttu-id="70f4d-160">**Azure Key Vault** – används för att skydda krypteringsnycklar och hemligheter som används för kryptering/dekryptering processen disk.</span><span class="sxs-lookup"><span data-stu-id="70f4d-160">**Azure Key Vault** - used to safeguard cryptographic keys and secrets used for the disk encryption/decryption process.</span></span>
  * <span data-ttu-id="70f4d-161">Om det finns ett kan du använda en befintlig Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="70f4d-161">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="70f4d-162">Du har inte kryptera diskar för ett Nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="70f4d-162">You do not have to dedicate a Key Vault to encrypting disks.</span></span>
  * <span data-ttu-id="70f4d-163">Du kan skapa ett dedikerat Nyckelvalv för att avgränsa administrativa gränser och viktiga synlighet.</span><span class="sxs-lookup"><span data-stu-id="70f4d-163">To separate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="70f4d-164">**Azure Active Directory** -hanterar säker utbyte av kryptografiska nycklar som krävs och autentisering för begärda åtgärder.</span><span class="sxs-lookup"><span data-stu-id="70f4d-164">**Azure Active Directory** - handles the secure exchanging of required cryptographic keys and authentication for requested actions.</span></span>
  * <span data-ttu-id="70f4d-165">Vanligtvis kan du använda en befintlig instans av Azure Active Directory för ditt program.</span><span class="sxs-lookup"><span data-stu-id="70f4d-165">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="70f4d-166">Programmet är av en slutpunkt för Nyckelvalvet och virtuella datorn att begära och hämta utfärdade lämpliga kryptografiska nycklar.</span><span class="sxs-lookup"><span data-stu-id="70f4d-166">The application is more of an endpoint for the Key Vault and Virtual Machine services to request and get issued the appropriate cryptographic keys.</span></span> <span data-ttu-id="70f4d-167">Du utvecklar inte ett verkligt program som integreras med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="70f4d-167">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="70f4d-168">Krav och begränsningar</span><span class="sxs-lookup"><span data-stu-id="70f4d-168">Requirements and limitations</span></span>
<span data-ttu-id="70f4d-169">Krav för diskkryptering och scenarier som stöds:</span><span class="sxs-lookup"><span data-stu-id="70f4d-169">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="70f4d-170">Följande Linux-servern SKU - Ubuntu, CentOS, SUSE och SUSE Linux Enterprise Server (SLES) och Red Hat Enterprise Linux.</span><span class="sxs-lookup"><span data-stu-id="70f4d-170">The following Linux server SKUs - Ubuntu, CentOS, SUSE and SUSE Linux Enterprise Server (SLES), and Red Hat Enterprise Linux.</span></span>
* <span data-ttu-id="70f4d-171">Alla resurser (till exempel Nyckelvalv lagringskonto och VM) måste finnas i samma Azure-region och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="70f4d-171">All resources (such as Key Vault, Storage account, and VM) must be in the same Azure region and subscription.</span></span>
* <span data-ttu-id="70f4d-172">Standard A, D, DS, G och GS-serien virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="70f4d-172">Standard A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="70f4d-173">Diskkryptering stöds inte i följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="70f4d-173">Disk encryption is not currently supported in the following scenarios:</span></span>

* <span data-ttu-id="70f4d-174">Grundnivån virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="70f4d-174">Basic tier VMs.</span></span>
* <span data-ttu-id="70f4d-175">Virtuella datorer skapas med den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="70f4d-175">VMs created using the Classic deployment model.</span></span>
* <span data-ttu-id="70f4d-176">Om du inaktiverar kryptering för OS-disk på virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="70f4d-176">Disabling OS disk encryption on Linux VMs.</span></span>
* <span data-ttu-id="70f4d-177">Uppdaterar de kryptografiska nycklarna på en redan krypterade Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="70f4d-177">Updating the cryptographic keys on an already encrypted Linux VM.</span></span>

## <a name="create-the-azure-key-vault-and-keys"></a><span data-ttu-id="70f4d-178">Skapa Azure Key Vault och nycklar</span><span class="sxs-lookup"><span data-stu-id="70f4d-178">Create the Azure Key Vault and keys</span></span>
<span data-ttu-id="70f4d-179">Slutför resten av den här guiden, måste den [senaste Azure CLI 1.0](../../xplat-cli-install.md) installerad och logga in med Resource Manager-läget på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70f4d-179">To complete the remainder of this guide, you need the [latest Azure CLI 1.0](../../xplat-cli-install.md) installed and logged in using the Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="70f4d-180">Ersätt alla exempel parametrar med egna namn, plats och nyckelvärden under hela kommandoexempel.</span><span class="sxs-lookup"><span data-stu-id="70f4d-180">Throughout the command examples, replace all example parameters with your own names, location, and key values.</span></span> <span data-ttu-id="70f4d-181">I följande exempel används en konvention för `myResourceGroup`, `myKeyVault`, `myAADApp`osv.</span><span class="sxs-lookup"><span data-stu-id="70f4d-181">The following examples use a convention of `myResourceGroup`, `myKeyVault`, `myAADApp`, etc.</span></span>

<span data-ttu-id="70f4d-182">Det första steget är att skapa en Azure Key Vault för att lagra kryptografiska nycklar.</span><span class="sxs-lookup"><span data-stu-id="70f4d-182">The first step is to create an Azure Key Vault to store your cryptographic keys.</span></span> <span data-ttu-id="70f4d-183">Azure Key Vault kan lagra nycklar, hemligheter eller lösenord som gör det möjligt att implementera dem på ett säkert sätt i dina program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="70f4d-183">Azure Key Vault can store keys, secrets, or passwords that allow you to securely implement them in your applications and services.</span></span> <span data-ttu-id="70f4d-184">För virtuell diskkryptering använder du Key Vault för att lagra en krypteringsnyckel som används för att kryptera eller dekryptera din virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="70f4d-184">For virtual disk encryption, you use Key Vault to store a cryptographic key that is used to encrypt or decrypt your virtual disks.</span></span>

<span data-ttu-id="70f4d-185">Aktivera Azure Key Vault-providern i din Azure-prenumeration och sedan skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="70f4d-185">Enable the Azure Key Vault provider in your Azure subscription, then create a resource group.</span></span> <span data-ttu-id="70f4d-186">I följande exempel skapas en resursgrupp med namnet `myResourceGroup` i den `WestUS` plats:</span><span class="sxs-lookup"><span data-stu-id="70f4d-186">The following example creates a resource group named `myResourceGroup` in the `WestUS` location:</span></span>

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

<span data-ttu-id="70f4d-187">Azure Key Vault som innehåller kryptografiska nycklar och associerade beräkningsresurser som lagring och Virtuellt datorn måste finnas i samma region.</span><span class="sxs-lookup"><span data-stu-id="70f4d-187">The Azure Key Vault containing the cryptographic keys and associated compute resources such as storage and the VM itself must reside in the same region.</span></span> <span data-ttu-id="70f4d-188">I följande exempel skapas en Azure Key Vault som heter `myKeyVault`:</span><span class="sxs-lookup"><span data-stu-id="70f4d-188">The following example creates an Azure Key Vault named `myKeyVault`:</span></span>

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

<span data-ttu-id="70f4d-189">Du kan lagra kryptografiska nycklar med hjälp av programvara eller maskinvara säkerhet modellen (HSM)-skydd.</span><span class="sxs-lookup"><span data-stu-id="70f4d-189">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="70f4d-190">Använda en HSM kräver en premium Key Vault.</span><span class="sxs-lookup"><span data-stu-id="70f4d-190">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="70f4d-191">Det finns en extra kostnad för att skapa en premium Key Vault snarare än standard Key Vault som lagrar programvara-skyddade nycklar.</span><span class="sxs-lookup"><span data-stu-id="70f4d-191">There is an additional cost to creating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="70f4d-192">När du skapar en premium Key Vault i föregående steg till `--sku Premium` i kommandot.</span><span class="sxs-lookup"><span data-stu-id="70f4d-192">To create a premium Key Vault, in the preceding step add `--sku Premium` to the command.</span></span> <span data-ttu-id="70f4d-193">I följande exempel används programvaruskyddad nycklar eftersom vi har skapat ett Nyckelvalv som standard.</span><span class="sxs-lookup"><span data-stu-id="70f4d-193">The following example uses software-protected keys since we created a standard Key Vault.</span></span>

<span data-ttu-id="70f4d-194">För båda modellerna skydd måste Azure-plattformen beviljas åtkomst för att begära kryptografiska nycklar när den virtuella datorn startas för att dekryptera virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="70f4d-194">For both protection models, the Azure platform needs to be granted access to request the cryptographic keys when the VM boots to decrypt the virtual disks.</span></span> <span data-ttu-id="70f4d-195">Skapa en krypteringsnyckel i ditt Nyckelvalv och sedan aktivera den för användning med virtuell diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="70f4d-195">Create an encryption key within your Key Vault, then enable it for use with virtual disk encryption.</span></span> <span data-ttu-id="70f4d-196">I följande exempel skapas en nyckel som heter `myKey` och gör det möjligt för diskkryptering:</span><span class="sxs-lookup"><span data-stu-id="70f4d-196">The following example creates a key named `myKey` and then enables it for disk encryption:</span></span>

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a><span data-ttu-id="70f4d-197">Skapa Azure Active Directory-program</span><span class="sxs-lookup"><span data-stu-id="70f4d-197">Create the Azure Active Directory application</span></span>
<span data-ttu-id="70f4d-198">När virtuella diskar krypteras och dekrypteras, använder du en slutpunkt för att hantera autentisering och byta ut kryptografiska nycklar från Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="70f4d-198">When virtual disks are encrypted or decrypted, you use an endpoint to handle the authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="70f4d-199">Den här slutpunkten måste ett Azure Active Directory-program kan Azure-plattformen begära lämpliga kryptografiska nycklar för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="70f4d-199">This endpoint, an Azure Active Directory application, allows the Azure platform to request the appropriate cryptographic keys on behalf of the VM.</span></span> <span data-ttu-id="70f4d-200">En standardinstans för Azure Active Directory finns i din prenumeration, även om många organisationer har särskilda Azure Active Directory-kataloger.</span><span class="sxs-lookup"><span data-stu-id="70f4d-200">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="70f4d-201">När du inte skapar en fullständig Azure Active Directory-program i `--home-page` och `--identifier-uris` parametrar i följande exempel behöver inte vara faktiska dirigerbara adresser.</span><span class="sxs-lookup"><span data-stu-id="70f4d-201">As you are not creating a full Azure Active Directory application, the `--home-page` and `--identifier-uris` parameters in the following example do not need to be actual routable address.</span></span> <span data-ttu-id="70f4d-202">I följande exempel anger också en lösenordsbaserad hemlighet i stället för att nycklarna från i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="70f4d-202">The following example also specifies a password-based secret rather than generating keys from within the Azure portal.</span></span> <span data-ttu-id="70f4d-203">Som den här tiden kan kan generera nycklar inte utföras från Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="70f4d-203">As this time, generating keys cannot be done from the Azure CLI.</span></span>

<span data-ttu-id="70f4d-204">Skapa Azure Active Directory-program.</span><span class="sxs-lookup"><span data-stu-id="70f4d-204">Create your Azure Active Directory application.</span></span> <span data-ttu-id="70f4d-205">I följande exempel skapas ett program med namnet `myAADApp` och använder ett lösenord på `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="70f4d-205">The following example creates an application named `myAADApp` and uses a password of `myPassword`.</span></span> <span data-ttu-id="70f4d-206">Ange ditt eget lösenord på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70f4d-206">Specify your own password as follows:</span></span>

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

<span data-ttu-id="70f4d-207">Anteckna den `applicationId` som returneras i utdata från kommandot ovan.</span><span class="sxs-lookup"><span data-stu-id="70f4d-207">Make a note of the `applicationId` that is returned in the output from the preceding command.</span></span> <span data-ttu-id="70f4d-208">Program-ID används i vissa av stegen.</span><span class="sxs-lookup"><span data-stu-id="70f4d-208">This application ID is used in some of the remaining steps.</span></span> <span data-ttu-id="70f4d-209">Skapa sedan en tjänstens huvudnamn (SPN) så att programmet är tillgänglig i din miljö.</span><span class="sxs-lookup"><span data-stu-id="70f4d-209">Next, create a service principal name (SPN) so that the application is accessible within your environment.</span></span> <span data-ttu-id="70f4d-210">För att kunna kryptera eller dekryptera virtuella diskar, måste behörigheter på den kryptografiska nyckel som lagras i Nyckelvalvet anges för att tillåta Azure Active Directory-programmet att läsa nycklarna.</span><span class="sxs-lookup"><span data-stu-id="70f4d-210">To successfully encrypt or decrypt virtual disks, permissions on the cryptographic key stored in Key Vault must be set to permit the Azure Active Directory application to read the keys.</span></span>

<span data-ttu-id="70f4d-211">Skapa ett SPN och ange de behörigheter som krävs på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70f4d-211">Create the SPN and set the appropriate permissions as follows:</span></span>

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a><span data-ttu-id="70f4d-212">Lägg till en virtuell disk och granska krypteringsstatus</span><span class="sxs-lookup"><span data-stu-id="70f4d-212">Add a virtual disk and review encryption status</span></span>
<span data-ttu-id="70f4d-213">Att faktiskt kryptera vissa virtuella diskar, kan du lägga till en disk i en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="70f4d-213">To actually encrypt some virtual disks, lets add a disk to an existing VM.</span></span> <span data-ttu-id="70f4d-214">Lägg till en disk på 5Gb data till en befintlig virtuell dator på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70f4d-214">Add a 5Gb data disk to an existing VM as follows:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="70f4d-215">De virtuella diskarna krypteras inte.</span><span class="sxs-lookup"><span data-stu-id="70f4d-215">The virtual disks are not currently encrypted.</span></span> <span data-ttu-id="70f4d-216">Granska aktuella krypteringsstatus för den virtuella datorn på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70f4d-216">Review the current encryption status of your VM as follows:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a><span data-ttu-id="70f4d-217">Kryptera virtuella diskar</span><span class="sxs-lookup"><span data-stu-id="70f4d-217">Encrypt virtual disks</span></span>
<span data-ttu-id="70f4d-218">För att kryptera nu virtuella diskar, samordnar du tidigare komponenter:</span><span class="sxs-lookup"><span data-stu-id="70f4d-218">To now encrypt the virtual disks, you bring together all the previous components:</span></span>

1. <span data-ttu-id="70f4d-219">Ange Azure Active Directory-program och lösenord.</span><span class="sxs-lookup"><span data-stu-id="70f4d-219">Specify the Azure Active Directory application and password.</span></span>
2. <span data-ttu-id="70f4d-220">Ange Nyckelvalv för att lagra metadata för krypterade diskarna.</span><span class="sxs-lookup"><span data-stu-id="70f4d-220">Specify the Key Vault to store the metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="70f4d-221">Ange de kryptografiska nycklarna som ska användas för den faktiska kryptering och dekryptering.</span><span class="sxs-lookup"><span data-stu-id="70f4d-221">Specify the cryptographic keys to use for the actual encryption and decryption.</span></span>
4. <span data-ttu-id="70f4d-222">Ange om du vill kryptera OS-disk, datadiskar eller alla.</span><span class="sxs-lookup"><span data-stu-id="70f4d-222">Specify whether you want to encrypt the OS disk, the data disks, or all.</span></span>

<span data-ttu-id="70f4d-223">Kan granska informationen för Azure Key Vault och den nyckel som du skapade när du behöver Key Vault ID URI, och ange sedan URL det sista steget:</span><span class="sxs-lookup"><span data-stu-id="70f4d-223">Lets review the details for your Azure Key Vault and the key you created, as you need the Key Vault ID, URI, and then key URL in the final step:</span></span>

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

<span data-ttu-id="70f4d-224">Kryptera din virtuella diskar med hjälp av utdata från den `azure keyvault show` och `azure keyvault key show` kommandon på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70f4d-224">Encrypt your virtual disks using the output from the `azure keyvault show` and `azure keyvault key show` commands as follows:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

<span data-ttu-id="70f4d-225">Som det föregående kommandot har många variabler, är följande exempel det fullständiga kommandot referens:</span><span class="sxs-lookup"><span data-stu-id="70f4d-225">As the preceding command has many variables, the following example is the complete command for reference:</span></span>

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

<span data-ttu-id="70f4d-226">Azure CLI anger inte utförlig fel under krypteringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="70f4d-226">The Azure CLI doesn't provide verbose errors during the encryption process.</span></span> <span data-ttu-id="70f4d-227">Ytterligare felsökningsinformation `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` på den virtuella datorn du krypterar.</span><span class="sxs-lookup"><span data-stu-id="70f4d-227">For additional troubleshooting information, review `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` on the VM you are encrypting.</span></span>

<span data-ttu-id="70f4d-228">Slutligen kan du granska krypteringsstatus igen för att bekräfta att din virtuella diskar nu har krypterats:</span><span class="sxs-lookup"><span data-stu-id="70f4d-228">Finally, lets review the encryption status again to confirm that your virtual disks have now been encrypted:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a><span data-ttu-id="70f4d-229">Lägg till ytterligare datadiskar</span><span class="sxs-lookup"><span data-stu-id="70f4d-229">Add additional data disks</span></span>
<span data-ttu-id="70f4d-230">När du har krypterade datadiskarna kan du också kryptera dem senare lägga till ytterligare virtuella diskar till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="70f4d-230">Once you have encrypted your data disks, you can later add additional virtual disks to your VM and also encrypt them.</span></span> <span data-ttu-id="70f4d-231">När du kör den `azure vm enable-disk-encryption` kommandot, öka sekvens version använder den `--sequence-version` parameter.</span><span class="sxs-lookup"><span data-stu-id="70f4d-231">When you run the `azure vm enable-disk-encryption` command, increment the sequence version using the `--sequence-version` parameter.</span></span> <span data-ttu-id="70f4d-232">Den här aktivitetssekvensen versionsparametern kan du utföra upprepade åtgärder på samma virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="70f4d-232">This sequence version parameter allows you to perform repeated operations on the same VM.</span></span>

<span data-ttu-id="70f4d-233">Exempelvis kan du lägga till en andra virtuell disk till den virtuella datorn på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70f4d-233">For example, lets add a second virtual disk to your VM as follows:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="70f4d-234">Kör kommandot för att kryptera de virtuella diskarna denna tid att lägga till den `--sequence-version` parameter och öka värdet från våra första gången du kör på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70f4d-234">Rerun the command to encrypt the virtual disks, this time adding the `--sequence-version` parameter, and incrementing the value from our first run as follows:</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="70f4d-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="70f4d-235">Next steps</span></span>
* <span data-ttu-id="70f4d-236">Läs mer om hur du hanterar Azure Key Vault, inklusive ta bort krypteringsnycklarna och valv, [hantera Key Vault med hjälp av CLI](../../key-vault/key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="70f4d-236">For more information about managing Azure Key Vault, including deleting cryptographic keys and vaults, see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md).</span></span>
* <span data-ttu-id="70f4d-237">Läs mer om diskkryptering, till exempel förbereda en krypterad anpassade VM att överföra till Azure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="70f4d-237">For more information about disk encryption, such as preparing an encrypted custom VM to upload to Azure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
