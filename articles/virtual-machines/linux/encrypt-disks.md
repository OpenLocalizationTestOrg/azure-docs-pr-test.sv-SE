---
title: "aaaEncrypt diskar på en Linux-VM i Azure | Microsoft Docs"
description: "Hur tooencrypt virtuella diskar på en Linux-VM för ökad säkerhet med hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: d6197742bc8562630e8395588c072093fc01d614
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencrypt-virtual-disks-on-a-linux-vm"></a><span data-ttu-id="91abc-103">Hur tooencrypt virtuella diskar på en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="91abc-103">How tooencrypt virtual disks on a Linux VM</span></span>
<span data-ttu-id="91abc-104">För förbättrad virtuell dator (VM) säkerhet och efterlevnad krypteras virtuella diskar i Azure.</span><span class="sxs-lookup"><span data-stu-id="91abc-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted.</span></span> <span data-ttu-id="91abc-105">Diskar krypteras med kryptografiska nycklar som skyddas i ett Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="91abc-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="91abc-106">Du styr dessa kryptografiska nycklar och granska deras användning.</span><span class="sxs-lookup"><span data-stu-id="91abc-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="91abc-107">Den här artikeln beskrivs hur tooencrypt virtuella diskar på en Linux VM som använder hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="91abc-107">This article details how tooencrypt virtual disks on a Linux VM using hello Azure CLI 2.0.</span></span> <span data-ttu-id="91abc-108">Du kan också utföra dessa steg med hello [Azure CLI 1.0](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="91abc-108">You can also perform these steps with hello [Azure CLI 1.0](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="91abc-109">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="91abc-109">Quick commands</span></span>
<span data-ttu-id="91abc-110">Om du behöver tooquickly utföra hello uppgiften, hello följande avsnitt information hello grundläggande kommandon tooencrypt virtuella diskar på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="91abc-110">If you need tooquickly accomplish hello task, hello following section details hello base commands tooencrypt virtual disks on your VM.</span></span> <span data-ttu-id="91abc-111">Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](#overview-of-disk-encryption).</span><span class="sxs-lookup"><span data-stu-id="91abc-111">More detailed information and context for each step can be found hello rest of hello document, [starting here](#overview-of-disk-encryption).</span></span>

<span data-ttu-id="91abc-112">Du behöver hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="91abc-112">You need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="91abc-113">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="91abc-113">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="91abc-114">Exempel parameternamn inkluderar *myResourceGroup*, *MinNyckel*, och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="91abc-114">Example parameter names include *myResourceGroup*, *myKey*, and *myVM*.</span></span>

<span data-ttu-id="91abc-115">Först aktiverar hello Azure Key Vault-providern i din Azure-prenumeration med [az providern registrera](/cli/azure/provider#register) och skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="91abc-115">First, enable hello Azure Key Vault provider within your Azure subscription with [az provider register](/cli/azure/provider#register) and create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="91abc-116">hello följande exempel skapas ett Resursgruppsnamn *myResourceGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="91abc-116">hello following example creates a resource group name *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="91abc-117">Skapa ett Azure Key Vault med [az keyvault skapa](/cli/azure/keyvault#create) och aktivera hello Key Vault för användning med diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="91abc-117">Create an Azure Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable hello Key Vault for use with disk encryption.</span></span> <span data-ttu-id="91abc-118">Ange ett unikt namn för Key Vault för *keyvault_name* på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="91abc-118">Specify a unique Key Vault name for *keyvault_name* as follows:</span></span>

```azurecli
keyvault_name=mykeyvaultikf
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

<span data-ttu-id="91abc-119">Skapa en kryptografisk nyckel i ditt Nyckelvalv med [az keyvault nyckel skapa](/cli/azure/keyvault/key#create).</span><span class="sxs-lookup"><span data-stu-id="91abc-119">Create a cryptographic key in your Key Vault with [az keyvault key create](/cli/azure/keyvault/key#create).</span></span> <span data-ttu-id="91abc-120">hello följande exempel skapas en nyckel som heter *MinNyckel*:</span><span class="sxs-lookup"><span data-stu-id="91abc-120">hello following example creates a key named *myKey*:</span></span>

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```

<span data-ttu-id="91abc-121">Skapa ett huvudnamn för tjänsten med hjälp av Azure Active Directory med [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac).</span><span class="sxs-lookup"><span data-stu-id="91abc-121">Create a service principal using Azure Active Directory with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac).</span></span> <span data-ttu-id="91abc-122">hello service principal handtag hello autentisering och utbyte av kryptografiska nycklar från Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="91abc-122">hello service principal handles hello authentication and exchange of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="91abc-123">följande exempel hello läser i hello värden för tjänstens huvudnamn hello-Id och lösenord för användning i senare kommandon:</span><span class="sxs-lookup"><span data-stu-id="91abc-123">hello following example reads in hello values for hello service principal Id and password for use in later commands:</span></span>

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

<span data-ttu-id="91abc-124">hello lösenord är bara utdata när du skapar hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="91abc-124">hello password is only output when you create hello service principal.</span></span> <span data-ttu-id="91abc-125">Om du vill visa och registrera hello lösenord (`echo $sp_password`).</span><span class="sxs-lookup"><span data-stu-id="91abc-125">If desired, view and record hello password (`echo $sp_password`).</span></span> <span data-ttu-id="91abc-126">Du kan visa din service principal med [az sp annonslista](/cli/azure/ad/sp#list) och visa ytterligare information om en specifik tjänstens huvudnamn med [az ad sp visa](/cli/azure/ad/sp#show).</span><span class="sxs-lookup"><span data-stu-id="91abc-126">You can list your service principals with [az ad sp list](/cli/azure/ad/sp#list) and view additional information about a specific service principal with [az ad sp show](/cli/azure/ad/sp#show).</span></span>

<span data-ttu-id="91abc-127">Ange behörigheter för ditt Nyckelvalv med [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span><span class="sxs-lookup"><span data-stu-id="91abc-127">Set permissions on your Key Vault with [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span></span> <span data-ttu-id="91abc-128">I följande exempel hello, har hello service ägar-ID angetts från hello föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="91abc-128">In hello following example, hello service principal ID is supplied from hello preceding command:</span></span>

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
    --key-permissions wrapKey \
    --secret-permissions set
```

<span data-ttu-id="91abc-129">Skapa en virtuell dator med [az vm skapa](/cli/azure/vm#create) och bifoga en disk på 5 Gb data.</span><span class="sxs-lookup"><span data-stu-id="91abc-129">Create a VM with [az vm create](/cli/azure/vm#create) and attach a 5Gb data disk.</span></span> <span data-ttu-id="91abc-130">Endast vissa marketplace-bilder stöd för kryptering.</span><span class="sxs-lookup"><span data-stu-id="91abc-130">Only certain marketplace images support disk encryption.</span></span> <span data-ttu-id="91abc-131">hello följande exempel skapas en virtuell dator med namnet `myVM` med hjälp av en **CentOS 7.2n** avbildningen:</span><span class="sxs-lookup"><span data-stu-id="91abc-131">hello following example creates a VM named `myVM` using a **CentOS 7.2n** image:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

<span data-ttu-id="91abc-132">SSH tooyour VM med hello `publicIpAddress` visas i hello utdata från hello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="91abc-132">SSH tooyour VM using hello `publicIpAddress` shown in hello output of hello preceding command.</span></span> <span data-ttu-id="91abc-133">Skapa en partition och filsystemet och sedan montera hello datadisk.</span><span class="sxs-lookup"><span data-stu-id="91abc-133">Create a partition and filesystem, then mount hello data disk.</span></span> <span data-ttu-id="91abc-134">Mer information finns i [ansluta tooa Linux VM toomount hello ny disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="91abc-134">For more information, see [Connect tooa Linux VM toomount hello new disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> <span data-ttu-id="91abc-135">Stäng SSH-sessionen.</span><span class="sxs-lookup"><span data-stu-id="91abc-135">Close your SSH session.</span></span>

<span data-ttu-id="91abc-136">Kryptera din virtuella dator med [az vm-kryptering aktiverar](/cli/azure/vm/encryption#enable).</span><span class="sxs-lookup"><span data-stu-id="91abc-136">Encrypt your VM with [az vm encryption enable](/cli/azure/vm/encryption#enable).</span></span> <span data-ttu-id="91abc-137">hello följande exempel används hello `$sp_id` och `$sp_password` variabler från föregående hello `ad sp create-for-rbac` kommando:</span><span class="sxs-lookup"><span data-stu-id="91abc-137">hello following example uses hello `$sp_id` and `$sp_password` variables from hello preceding `ad sp create-for-rbac` command:</span></span>

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

<span data-ttu-id="91abc-138">Det tar en stund innan hello disk encryption processen toocomplete.</span><span class="sxs-lookup"><span data-stu-id="91abc-138">It takes some time for hello disk encryption process toocomplete.</span></span> <span data-ttu-id="91abc-139">Övervaka hello status hello processen med [az vm kryptering visa](/cli/azure/vm/encryption#show):</span><span class="sxs-lookup"><span data-stu-id="91abc-139">Monitor hello status of hello process with [az vm encryption show](/cli/azure/vm/encryption#show):</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="91abc-140">Hej status visas **EncryptionInProgress**.</span><span class="sxs-lookup"><span data-stu-id="91abc-140">hello status shows **EncryptionInProgress**.</span></span> <span data-ttu-id="91abc-141">Vänta tills hello status för hello OS disk rapporter **VMRestartPending**, starta om den virtuella datorn med [az vm omstart](/cli/azure/vm#restart):</span><span class="sxs-lookup"><span data-stu-id="91abc-141">Wait until hello status for hello OS disk reports **VMRestartPending**, then restart your VM with [az vm restart](/cli/azure/vm#restart):</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="91abc-142">hello disk krypteringsprocessen är slutförd under startprocessen hello, så Vänta några minuter innan kontrollerar hello statusen för kryptering igen med **az vm kryptering visa**:</span><span class="sxs-lookup"><span data-stu-id="91abc-142">hello disk encryption process is finalized during hello boot process, so wait a few minutes before checking hello status of encryption again with **az vm encryption show**:</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="91abc-143">hello status bör nu rapportera både hello OS-disk- och datadisk som **krypterade**.</span><span class="sxs-lookup"><span data-stu-id="91abc-143">hello status should now report both hello OS disk and data disk as **Encrypted**.</span></span>

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="91abc-144">Översikt över diskkryptering</span><span class="sxs-lookup"><span data-stu-id="91abc-144">Overview of disk encryption</span></span>
<span data-ttu-id="91abc-145">Virtuella diskar på virtuella Linux-datorer krypteras på resten med hjälp av [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span><span class="sxs-lookup"><span data-stu-id="91abc-145">Virtual disks on Linux VMs are encrypted at rest using [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span></span> <span data-ttu-id="91abc-146">Det är gratis för kryptering av virtuella diskar i Azure.</span><span class="sxs-lookup"><span data-stu-id="91abc-146">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="91abc-147">Kryptografiska nycklar lagras i Azure Key Vault med skydd av programvara eller du kan importera eller generera dina nycklar i Maskinvarusäkerhetsmoduler (HSM) certifierade tooFIPS 140-2 level 2-standarder.</span><span class="sxs-lookup"><span data-stu-id="91abc-147">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified tooFIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="91abc-148">Du behålla kontrollen över dessa kryptografiska nycklar och granska deras användning.</span><span class="sxs-lookup"><span data-stu-id="91abc-148">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="91abc-149">Dessa kryptografiska nycklar används tooencrypt och dekryptera virtuella diskar anslutna tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="91abc-149">These cryptographic keys are used tooencrypt and decrypt virtual disks attached tooyour VM.</span></span> <span data-ttu-id="91abc-150">Ett Azure Active Directory-tjänstens huvudnamn ger en säker mekanism för att utfärda de kryptografiska nycklarna som virtuella datorer är igång på och stänga av.</span><span class="sxs-lookup"><span data-stu-id="91abc-150">An Azure Active Directory service principal provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="91abc-151">hello-processen för att kryptera en virtuell dator är som följer:</span><span class="sxs-lookup"><span data-stu-id="91abc-151">hello process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="91abc-152">Skapa en kryptografisk nyckel i en Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="91abc-152">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="91abc-153">Konfigurera hello kryptografiska nyckel toobe kan användas för att kryptera diskar.</span><span class="sxs-lookup"><span data-stu-id="91abc-153">Configure hello cryptographic key toobe usable for encrypting disks.</span></span>
3. <span data-ttu-id="91abc-154">tooread hello krypteringsnyckeln från hello Azure Key Vault skapa en Azure Active Directory service principal med hello lämpliga behörigheter.</span><span class="sxs-lookup"><span data-stu-id="91abc-154">tooread hello cryptographic key from hello Azure Key Vault, create an Azure Active Directory service principal with hello appropriate permissions.</span></span>
4. <span data-ttu-id="91abc-155">Utfärda hello kommandot tooencrypt din virtuella diskar, att ange hello Azure Active Directory tjänstens huvudnamn och lämpliga kryptografiska nyckel toobe används.</span><span class="sxs-lookup"><span data-stu-id="91abc-155">Issue hello command tooencrypt your virtual disks, specifying hello Azure Active Directory service principal and appropriate cryptographic key toobe used.</span></span>
5. <span data-ttu-id="91abc-156">hello Azure Active Directory service principal begäranden hello krävs krypteringsnyckeln från Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="91abc-156">hello Azure Active Directory service principal requests hello required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="91abc-157">hello virtuella diskar krypteras med hello tillhandahålls kryptografisk nyckel.</span><span class="sxs-lookup"><span data-stu-id="91abc-157">hello virtual disks are encrypted using hello provided cryptographic key.</span></span>

## <a name="encryption-process"></a><span data-ttu-id="91abc-158">Krypteringsprocessen</span><span class="sxs-lookup"><span data-stu-id="91abc-158">Encryption process</span></span>
<span data-ttu-id="91abc-159">Kryptering är beroende av hello följande ytterligare komponenter:</span><span class="sxs-lookup"><span data-stu-id="91abc-159">Disk encryption relies on hello following additional components:</span></span>

* <span data-ttu-id="91abc-160">**Azure Key Vault** -används toosafeguard kryptografiska nycklar och hemligheter som används för hello disk kryptering/dekryptering process.</span><span class="sxs-lookup"><span data-stu-id="91abc-160">**Azure Key Vault** - used toosafeguard cryptographic keys and secrets used for hello disk encryption/decryption process.</span></span>
  * <span data-ttu-id="91abc-161">Om det finns ett kan du använda en befintlig Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="91abc-161">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="91abc-162">Du har inte toodedicate en Key Vault tooencrypting diskar.</span><span class="sxs-lookup"><span data-stu-id="91abc-162">You do not have toodedicate a Key Vault tooencrypting disks.</span></span>
  * <span data-ttu-id="91abc-163">Du kan skapa ett dedikerat Nyckelvalv tooseparate administrativa gränser och viktiga synlighet.</span><span class="sxs-lookup"><span data-stu-id="91abc-163">tooseparate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="91abc-164">**Azure Active Directory** - referenser hello säker utbyte av kryptografiska nycklar som krävs och autentisering för begärt åtgärder.</span><span class="sxs-lookup"><span data-stu-id="91abc-164">**Azure Active Directory** - handles hello secure exchanging of required cryptographic keys and authentication for requested actions.</span></span>
  * <span data-ttu-id="91abc-165">Vanligtvis kan du använda en befintlig instans av Azure Active Directory för ditt program.</span><span class="sxs-lookup"><span data-stu-id="91abc-165">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="91abc-166">hello tjänstens huvudnamn tillhandahåller en mekanism för säker toorequest och utfärdas hello lämpliga kryptografiska nycklar.</span><span class="sxs-lookup"><span data-stu-id="91abc-166">hello service principal provides a secure mechanism toorequest and be issued hello appropriate cryptographic keys.</span></span> <span data-ttu-id="91abc-167">Du utvecklar inte ett verkligt program som integreras med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="91abc-167">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="91abc-168">Krav och begränsningar</span><span class="sxs-lookup"><span data-stu-id="91abc-168">Requirements and limitations</span></span>
<span data-ttu-id="91abc-169">Krav för diskkryptering och scenarier som stöds:</span><span class="sxs-lookup"><span data-stu-id="91abc-169">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="91abc-170">hello följande Linux server SKU - Ubuntu, CentOS, SUSE och SUSE Linux Enterprise Server (SLES) och Red Hat Enterprise Linux.</span><span class="sxs-lookup"><span data-stu-id="91abc-170">hello following Linux server SKUs - Ubuntu, CentOS, SUSE and SUSE Linux Enterprise Server (SLES), and Red Hat Enterprise Linux.</span></span>
* <span data-ttu-id="91abc-171">Alla resurser (till exempel Nyckelvalv lagringskonto och VM) måste vara i hello samma Azure-region och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="91abc-171">All resources (such as Key Vault, Storage account, and VM) must be in hello same Azure region and subscription.</span></span>
* <span data-ttu-id="91abc-172">Standard A, D, DS, G och GS-serien virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="91abc-172">Standard A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="91abc-173">Diskkryptering stöds inte för närvarande i hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="91abc-173">Disk encryption is not currently supported in hello following scenarios:</span></span>

* <span data-ttu-id="91abc-174">Grundnivån virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="91abc-174">Basic tier VMs.</span></span>
* <span data-ttu-id="91abc-175">Virtuella datorer skapas med hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="91abc-175">VMs created using hello Classic deployment model.</span></span>
* <span data-ttu-id="91abc-176">Om du inaktiverar kryptering för OS-disk på virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="91abc-176">Disabling OS disk encryption on Linux VMs.</span></span>
* <span data-ttu-id="91abc-177">Uppdaterar hello Kryptografiska nycklar på en redan krypterade Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="91abc-177">Updating hello cryptographic keys on an already encrypted Linux VM.</span></span>

## <a name="create-azure-key-vault-and-keys"></a><span data-ttu-id="91abc-178">Skapa Azure Key Vault och nycklar</span><span class="sxs-lookup"><span data-stu-id="91abc-178">Create Azure Key Vault and keys</span></span>
<span data-ttu-id="91abc-179">Du behöver hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="91abc-179">You need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="91abc-180">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="91abc-180">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="91abc-181">Exempel parameternamn inkluderar *myResourceGroup*, *MinNyckel*, och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="91abc-181">Example parameter names include *myResourceGroup*, *myKey*, and *myVM*.</span></span>

<span data-ttu-id="91abc-182">hello första steget är toocreate ett Azure Key Vault toostore kryptografiska nycklar.</span><span class="sxs-lookup"><span data-stu-id="91abc-182">hello first step is toocreate an Azure Key Vault toostore your cryptographic keys.</span></span> <span data-ttu-id="91abc-183">Azure Key Vault kan lagra nycklar, hemligheter, eller lösenord som gör att du toosecurely implementeras i dina program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="91abc-183">Azure Key Vault can store keys, secrets, or passwords that allow you toosecurely implement them in your applications and services.</span></span> <span data-ttu-id="91abc-184">För virtuell diskkryptering, använda Key Vault toostore en krypteringsnyckel som används tooencrypt eller dekryptera din virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="91abc-184">For virtual disk encryption, you use Key Vault toostore a cryptographic key that is used tooencrypt or decrypt your virtual disks.</span></span>

<span data-ttu-id="91abc-185">Aktivera hello Azure Key Vault-providern i din Azure-prenumeration med [az providern registrera](/cli/azure/provider#register) och skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="91abc-185">Enable hello Azure Key Vault provider within your Azure subscription with [az provider register](/cli/azure/provider#register) and create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="91abc-186">hello följande exempel skapas ett Resursgruppsnamn *myResourceGroup* i hello `eastus` plats:</span><span class="sxs-lookup"><span data-stu-id="91abc-186">hello following example creates a resource group name *myResourceGroup* in hello `eastus` location:</span></span>

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="91abc-187">hello Azure Key Vault som innehåller hello krypteringsnycklar och associerade beräkning resurser, till exempel lagring och hello Virtuella datorn måste finnas i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="91abc-187">hello Azure Key Vault containing hello cryptographic keys and associated compute resources such as storage and hello VM itself must reside in hello same region.</span></span> <span data-ttu-id="91abc-188">Skapa ett Azure Key Vault med [az keyvault skapa](/cli/azure/keyvault#create) och aktivera hello Key Vault för användning med diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="91abc-188">Create an Azure Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable hello Key Vault for use with disk encryption.</span></span> <span data-ttu-id="91abc-189">Ange ett unikt namn för Key Vault för *keyvault_name* på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="91abc-189">Specify a unique Key Vault name for *keyvault_name* as follows:</span></span>

```azurecli
keyvault_name=myUniqueKeyVaultName
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

<span data-ttu-id="91abc-190">Du kan lagra kryptografiska nycklar med hjälp av programvara eller maskinvara säkerhet modellen (HSM)-skydd.</span><span class="sxs-lookup"><span data-stu-id="91abc-190">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="91abc-191">Använda en HSM kräver en premium Key Vault.</span><span class="sxs-lookup"><span data-stu-id="91abc-191">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="91abc-192">Det finns ett extra kostnad toocreating bidrag Key Vault snarare än standard Key Vault som lagrar programvara-skyddade nycklar.</span><span class="sxs-lookup"><span data-stu-id="91abc-192">There is an additional cost toocreating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="91abc-193">toocreate lägga till en premium Key Vault i föregående steg hello `--sku Premium` toohello kommando.</span><span class="sxs-lookup"><span data-stu-id="91abc-193">toocreate a premium Key Vault, in hello preceding step add `--sku Premium` toohello command.</span></span> <span data-ttu-id="91abc-194">hello följande exempel används programvaruskyddad nycklar eftersom vi har skapat ett Nyckelvalv som standard.</span><span class="sxs-lookup"><span data-stu-id="91abc-194">hello following example uses software-protected keys since we created a standard Key Vault.</span></span>

<span data-ttu-id="91abc-195">För båda modellerna skydd måste hello Azure-plattformen toobe beviljas åtkomst toorequest hello Kryptografiska nycklar när hello VM startas toodecrypt hello virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="91abc-195">For both protection models, hello Azure platform needs toobe granted access toorequest hello cryptographic keys when hello VM boots toodecrypt hello virtual disks.</span></span> <span data-ttu-id="91abc-196">Skapa en kryptografisk nyckel i ditt Nyckelvalv med [az keyvault nyckel skapa](/cli/azure/keyvault/key#create).</span><span class="sxs-lookup"><span data-stu-id="91abc-196">Create a cryptographic key in your Key Vault with [az keyvault key create](/cli/azure/keyvault/key#create).</span></span> <span data-ttu-id="91abc-197">hello följande exempel skapas en nyckel som heter *MinNyckel*:</span><span class="sxs-lookup"><span data-stu-id="91abc-197">hello following example creates a key named *myKey*:</span></span>

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-hello-azure-active-directory-service-principal"></a><span data-ttu-id="91abc-198">Skapa hello Azure Active Directory-tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="91abc-198">Create hello Azure Active Directory service principal</span></span>
<span data-ttu-id="91abc-199">När virtuella diskar krypteras och dekrypteras, anger du ett konto toohandle hello autentisering och byta ut kryptografiska nycklar från Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="91abc-199">When virtual disks are encrypted or decrypted, you specify an account toohandle hello authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="91abc-200">Det här kontot, tjänstens huvudnamn för ett Azure Active Directory kan hello Azure-plattformen toorequest hello lämpliga krypteringsnycklar uppdrag hello VM.</span><span class="sxs-lookup"><span data-stu-id="91abc-200">This account, an Azure Active Directory service principal, allows hello Azure platform toorequest hello appropriate cryptographic keys on behalf of hello VM.</span></span> <span data-ttu-id="91abc-201">En standardinstans för Azure Active Directory finns i din prenumeration, även om många organisationer har särskilda Azure Active Directory-kataloger.</span><span class="sxs-lookup"><span data-stu-id="91abc-201">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="91abc-202">Skapa ett huvudnamn för tjänsten med hjälp av Azure Active Directory med [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac).</span><span class="sxs-lookup"><span data-stu-id="91abc-202">Create a service principal using Azure Active Directory with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac).</span></span> <span data-ttu-id="91abc-203">följande exempel hello läser i hello värden för tjänstens huvudnamn hello-Id och lösenord för användning i senare kommandon:</span><span class="sxs-lookup"><span data-stu-id="91abc-203">hello following example reads in hello values for hello service principal Id and password for use in later commands:</span></span>

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

<span data-ttu-id="91abc-204">hello lösenord visas bara när du skapar hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="91abc-204">hello password is only displayed when you create hello service principal.</span></span> <span data-ttu-id="91abc-205">Om du vill visa och registrera hello lösenord (`echo $sp_password`).</span><span class="sxs-lookup"><span data-stu-id="91abc-205">If desired, view and record hello password (`echo $sp_password`).</span></span> <span data-ttu-id="91abc-206">Du kan visa din service principal med [az sp annonslista](/cli/azure/ad/sp#list) och visa ytterligare information om en specifik tjänstens huvudnamn med [az ad sp visa](/cli/azure/ad/sp#show).</span><span class="sxs-lookup"><span data-stu-id="91abc-206">You can list your service principals with [az ad sp list](/cli/azure/ad/sp#list) and view additional information about a specific service principal with [az ad sp show](/cli/azure/ad/sp#show).</span></span>

<span data-ttu-id="91abc-207">toosuccessfully kryptera eller dekryptera virtuella diskar, behörigheter på hello kryptografiska nyckel som lagras i Nyckelvalvet måste vara set toopermit hello Azure Active Directory service principal tooread hello nycklar.</span><span class="sxs-lookup"><span data-stu-id="91abc-207">toosuccessfully encrypt or decrypt virtual disks, permissions on hello cryptographic key stored in Key Vault must be set toopermit hello Azure Active Directory service principal tooread hello keys.</span></span> <span data-ttu-id="91abc-208">Ange behörigheter för ditt Nyckelvalv med [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span><span class="sxs-lookup"><span data-stu-id="91abc-208">Set permissions on your Key Vault with [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span></span> <span data-ttu-id="91abc-209">I följande exempel hello, har hello service ägar-ID angetts från hello föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="91abc-209">In hello following example, hello service principal ID is supplied from hello preceding command:</span></span>

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-virtual-machine"></a><span data-ttu-id="91abc-210">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="91abc-210">Create virtual machine</span></span>
<span data-ttu-id="91abc-211">tooactually kryptera vissa virtuella diskar, kan du skapa en virtuell dator och lägger till en datadisk.</span><span class="sxs-lookup"><span data-stu-id="91abc-211">tooactually encrypt some virtual disks, lets create a VM and add a data disk.</span></span> <span data-ttu-id="91abc-212">Skapa en VM-tooencrypt med [az vm skapa](/cli/azure/vm#create) och bifoga en disk på 5 Gb data.</span><span class="sxs-lookup"><span data-stu-id="91abc-212">Create a VM tooencrypt with [az vm create](/cli/azure/vm#create) and attach a 5Gb data disk.</span></span> <span data-ttu-id="91abc-213">Endast vissa marketplace-bilder stöd för kryptering.</span><span class="sxs-lookup"><span data-stu-id="91abc-213">Only certain marketplace images support disk encryption.</span></span> <span data-ttu-id="91abc-214">hello följande exempel skapas en virtuell dator med namnet *myVM* med hjälp av en **CentOS 7.2n** avbildningen:</span><span class="sxs-lookup"><span data-stu-id="91abc-214">hello following example creates a VM named *myVM* using a **CentOS 7.2n** image:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

<span data-ttu-id="91abc-215">SSH tooyour VM med hello `publicIpAddress` visas i hello utdata från hello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="91abc-215">SSH tooyour VM using hello `publicIpAddress` shown in hello output of hello preceding command.</span></span> <span data-ttu-id="91abc-216">Skapa en partition och filsystemet och sedan montera hello datadisk.</span><span class="sxs-lookup"><span data-stu-id="91abc-216">Create a partition and filesystem, then mount hello data disk.</span></span> <span data-ttu-id="91abc-217">Mer information finns i [ansluta tooa Linux VM toomount hello ny disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="91abc-217">For more information, see [Connect tooa Linux VM toomount hello new disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> <span data-ttu-id="91abc-218">Stäng SSH-sessionen.</span><span class="sxs-lookup"><span data-stu-id="91abc-218">Close your SSH session.</span></span>


## <a name="encrypt-virtual-machine"></a><span data-ttu-id="91abc-219">Kryptera en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="91abc-219">Encrypt virtual machine</span></span>
<span data-ttu-id="91abc-220">tooencrypt hello virtuella diskar kan du sammanföra alla tidigare hello-komponenter:</span><span class="sxs-lookup"><span data-stu-id="91abc-220">tooencrypt hello virtual disks, you bring together all hello previous components:</span></span>

1. <span data-ttu-id="91abc-221">Ange hello Azure Active Directory-tjänstens huvudnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="91abc-221">Specify hello Azure Active Directory service principal and password.</span></span>
2. <span data-ttu-id="91abc-222">Ange hello Key Vault toostore hello metadata för krypterade diskarna.</span><span class="sxs-lookup"><span data-stu-id="91abc-222">Specify hello Key Vault toostore hello metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="91abc-223">Ange hello krypteringsnycklar toouse för hello faktiska kryptering och dekryptering.</span><span class="sxs-lookup"><span data-stu-id="91abc-223">Specify hello cryptographic keys toouse for hello actual encryption and decryption.</span></span>
4. <span data-ttu-id="91abc-224">Ange om du vill tooencrypt hello OS-disk, hello datadiskar eller alla.</span><span class="sxs-lookup"><span data-stu-id="91abc-224">Specify whether you want tooencrypt hello OS disk, hello data disks, or all.</span></span>

<span data-ttu-id="91abc-225">Kryptera din virtuella dator med [az vm-kryptering aktiverar](/cli/azure/vm/encryption#enable).</span><span class="sxs-lookup"><span data-stu-id="91abc-225">Encrypt your VM with [az vm encryption enable](/cli/azure/vm/encryption#enable).</span></span> <span data-ttu-id="91abc-226">hello följande exempel används hello `$sp_id` och `$sp_password` variabler från föregående hello `ad sp create-for-rbac` kommando:</span><span class="sxs-lookup"><span data-stu-id="91abc-226">hello following example uses hello `$sp_id` and `$sp_password` variables from hello preceding `ad sp create-for-rbac` command:</span></span>

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

<span data-ttu-id="91abc-227">Det tar en stund innan hello disk encryption processen toocomplete.</span><span class="sxs-lookup"><span data-stu-id="91abc-227">It takes some time for hello disk encryption process toocomplete.</span></span> <span data-ttu-id="91abc-228">Övervaka hello status hello processen med [az vm kryptering visa](/cli/azure/vm/encryption#show):</span><span class="sxs-lookup"><span data-stu-id="91abc-228">Monitor hello status of hello process with [az vm encryption show](/cli/azure/vm/encryption#show):</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="91abc-229">hello utdata är liknande toohello följande trunkerat exempel:</span><span class="sxs-lookup"><span data-stu-id="91abc-229">hello output is similar toohello following truncated example:</span></span>

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress",
]
```

<span data-ttu-id="91abc-230">Vänta tills hello status för hello OS disk rapporter **VMRestartPending**, starta om den virtuella datorn med [az vm omstart](/cli/azure/vm#restart):</span><span class="sxs-lookup"><span data-stu-id="91abc-230">Wait until hello status for hello OS disk reports **VMRestartPending**, then restart your VM with [az vm restart](/cli/azure/vm#restart):</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="91abc-231">hello disk krypteringsprocessen är slutförd under startprocessen hello, så Vänta några minuter innan kontrollerar hello statusen för kryptering igen med **az vm kryptering visa**:</span><span class="sxs-lookup"><span data-stu-id="91abc-231">hello disk encryption process is finalized during hello boot process, so wait a few minutes before checking hello status of encryption again with **az vm encryption show**:</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="91abc-232">hello status bör nu rapportera både hello OS-disk- och datadisk som **krypterade**.</span><span class="sxs-lookup"><span data-stu-id="91abc-232">hello status should now report both hello OS disk and data disk as **Encrypted**.</span></span>


## <a name="add-additional-data-disks"></a><span data-ttu-id="91abc-233">Lägg till ytterligare datadiskar</span><span class="sxs-lookup"><span data-stu-id="91abc-233">Add additional data disks</span></span>
<span data-ttu-id="91abc-234">När du har krypterade datadiskarna kan du också kryptera dem senare lägga till ytterligare virtuella diskar tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="91abc-234">Once you have encrypted your data disks, you can later add additional virtual disks tooyour VM and also encrypt them.</span></span> <span data-ttu-id="91abc-235">Exempelvis kan du lägga till en andra virtuell disk tooyour VM enligt följande:</span><span class="sxs-lookup"><span data-stu-id="91abc-235">For example, lets add a second virtual disk tooyour VM as follows:</span></span>

```azurecli
az vm disk attach-new --resource-group myResourceGroup --vm-name myVM --size-in-gb 5
```

<span data-ttu-id="91abc-236">Kör hello kommandot tooencrypt hello virtuella diskar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="91abc-236">Re-run hello command tooencrypt hello virtual disks as follows:</span></span>

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```


## <a name="next-steps"></a><span data-ttu-id="91abc-237">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91abc-237">Next steps</span></span>
* <span data-ttu-id="91abc-238">Läs mer om hur du hanterar Azure Key Vault, inklusive ta bort krypteringsnycklarna och valv, [hantera Key Vault med hjälp av CLI](../../key-vault/key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="91abc-238">For more information about managing Azure Key Vault, including deleting cryptographic keys and vaults, see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md).</span></span>
* <span data-ttu-id="91abc-239">Läs mer om diskkryptering, till exempel förbereda en krypterad anpassade VM tooupload tooAzure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="91abc-239">For more information about disk encryption, such as preparing an encrypted custom VM tooupload tooAzure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
