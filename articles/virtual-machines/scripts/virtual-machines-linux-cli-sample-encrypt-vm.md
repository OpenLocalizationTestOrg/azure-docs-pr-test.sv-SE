---
title: aaaAzure CLI skriptexempel - kryptera en Linux VM | Microsoft Docs
description: Azure CLI Script exempel - kryptera en virtuell Linux-dator
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.openlocfilehash: 1e455da4a8ea6d75b6d0d74b338d2e4d84973413
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="53967-103">Kryptera en virtuell Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="53967-103">Encrypt a Linux virtual machine in Azure</span></span>

<span data-ttu-id="53967-104">Det här skriptet skapar en säker Azure Key Vault, krypteringsnycklar, Azure Active Directory-tjänstens huvudnamn och en Linux-dator (VM).</span><span class="sxs-lookup"><span data-stu-id="53967-104">This script creates a secure Azure Key Vault, encryption keys, Azure Active Directory service principal, and a Linux virtual machine (VM).</span></span> <span data-ttu-id="53967-105">hello VM krypteras sedan med hello krypteringsnyckeln från Nyckelvalvet och tjänstens huvudnamn autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="53967-105">hello VM is then encrypted using hello encryption key from Key Vault and service principal credentials.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="53967-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="53967-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a><span data-ttu-id="53967-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="53967-107">Clean up deployment</span></span> 

<span data-ttu-id="53967-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="53967-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="53967-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="53967-109">Script explanation</span></span>

<span data-ttu-id="53967-110">Det här skriptet använder följande kommandon toocreate hello en resurs grupp, Azure Key Vault service principal, virtuell dator, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="53967-110">This script uses hello following commands toocreate a resource group, Azure Key Vault, service principal, virtual machine, and all related resources.</span></span> <span data-ttu-id="53967-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="53967-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="53967-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="53967-112">Command</span></span> | <span data-ttu-id="53967-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="53967-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="53967-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="53967-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="53967-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="53967-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="53967-116">Skapa AZ keyvault</span><span class="sxs-lookup"><span data-stu-id="53967-116">az keyvault create</span></span>](https://docs.microsoft.com/cli/azure/keyvault#create) | <span data-ttu-id="53967-117">Skapar ett Azure Key Vault toostore säkra data, till exempel krypteringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="53967-117">Creates an Azure Key Vault toostore secure data such as encryption keys.</span></span> |
| [<span data-ttu-id="53967-118">Skapa AZ keyvault nyckel</span><span class="sxs-lookup"><span data-stu-id="53967-118">az keyvault key create</span></span>](https://docs.microsoft.com/cli/azure/keyvault/key#create) | <span data-ttu-id="53967-119">Skapar en krypteringsnyckel i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="53967-119">Creates an encryption key in Key Vault.</span></span> |
| [<span data-ttu-id="53967-120">AZ ad sp skapa-för-rbac</span><span class="sxs-lookup"><span data-stu-id="53967-120">az ad sp create-for-rbac</span></span>](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) | <span data-ttu-id="53967-121">Skapar ett Azure Active Directory service principal toosecurely autentisera och styra tooencryption åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="53967-121">Creates an Azure Active Directory service principal toosecurely authenticate and control access tooencryption keys.</span></span> |
| [<span data-ttu-id="53967-122">Ange en AZ keyvault-princip</span><span class="sxs-lookup"><span data-stu-id="53967-122">az keyvault set-policy</span></span>](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | <span data-ttu-id="53967-123">Anger behörigheter på hello Key Vault toogrant hello service principal tooencryption åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="53967-123">Sets permissions on hello Key Vault toogrant hello service principal access tooencryption keys.</span></span> |
| [<span data-ttu-id="53967-124">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="53967-124">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="53967-125">Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="53967-125">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="53967-126">Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="53967-126">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="53967-127">AZ vm kryptering aktivera</span><span class="sxs-lookup"><span data-stu-id="53967-127">az vm encryption enable</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#enable) | <span data-ttu-id="53967-128">Aktiverar kryptering på en virtuell dator med hjälp av hello service principal autentiseringsuppgifter och krypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="53967-128">Enables encryption on a VM using hello service principal credentials and encryption key.</span></span> |
| [<span data-ttu-id="53967-129">AZ vm kryptering visa</span><span class="sxs-lookup"><span data-stu-id="53967-129">az vm encryption show</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#show) | <span data-ttu-id="53967-130">Visar hello status hello VM krypteringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="53967-130">Shows hello status of hello VM encryption process.</span></span> |
| [<span data-ttu-id="53967-131">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="53967-131">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="53967-132">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="53967-132">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="53967-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53967-133">Next steps</span></span>

<span data-ttu-id="53967-134">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="53967-134">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="53967-135">Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="53967-135">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
