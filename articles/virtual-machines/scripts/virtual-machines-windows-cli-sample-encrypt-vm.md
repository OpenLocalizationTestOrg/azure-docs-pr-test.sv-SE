---
title: Azure CLI Script exempel - kryptera Windows VM | Microsoft Docs
description: Azure CLI Script exempel - kryptera Windows VM
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.openlocfilehash: 9574d779982c939aa970cd4bdf7df6e66793e3b9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="d5ee0-103">Kryptera en Windows-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="d5ee0-103">Encrypt a Windows virtual machine in Azure</span></span>

<span data-ttu-id="d5ee0-104">Det här skriptet skapar en säker Azure Key Vault, krypteringsnycklar, Azure Active Directory-tjänstens huvudnamn och ett Windows-dator (VM).</span><span class="sxs-lookup"><span data-stu-id="d5ee0-104">This script creates a secure Azure Key Vault, encryption keys, Azure Active Directory service principal, and a Windows virtual machine (VM).</span></span> <span data-ttu-id="d5ee0-105">Den virtuella datorn krypteras sedan med hjälp av krypteringsnyckeln från Nyckelvalvet och tjänstens huvudnamn autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-105">The VM is then encrypted using the encryption key from Key Vault and service principal credentials.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="d5ee0-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="d5ee0-106">Sample script</span></span>

<span data-ttu-id="d5ee0-107">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "kryptera Virtuella diskar")]</span><span class="sxs-lookup"><span data-stu-id="d5ee0-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "Encrypt VM disks")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="d5ee0-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="d5ee0-108">Clean up deployment</span></span> 

<span data-ttu-id="d5ee0-109">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="d5ee0-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="d5ee0-110">Script explanation</span></span>

<span data-ttu-id="d5ee0-111">Det här skriptet använder följande kommandon för att skapa en resursgrupp, Azure Key Vault, tjänstens huvudnamn, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-111">This script uses the following commands to create a resource group, Azure Key Vault, service principal, virtual machine, and all related resources.</span></span> <span data-ttu-id="d5ee0-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="d5ee0-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="d5ee0-113">Command</span></span> | <span data-ttu-id="d5ee0-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="d5ee0-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d5ee0-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="d5ee0-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="d5ee0-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d5ee0-117">Skapa AZ keyvault</span><span class="sxs-lookup"><span data-stu-id="d5ee0-117">az keyvault create</span></span>](https://docs.microsoft.com/cli/azure/keyvault#create) | <span data-ttu-id="d5ee0-118">Skapar ett Azure Key Vault för lagring av säkra data, till exempel krypteringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-118">Creates an Azure Key Vault to store secure data such as encryption keys.</span></span> |
| [<span data-ttu-id="d5ee0-119">Skapa AZ keyvault nyckel</span><span class="sxs-lookup"><span data-stu-id="d5ee0-119">az keyvault key create</span></span>](https://docs.microsoft.com/cli/azure/keyvault/key#create) | <span data-ttu-id="d5ee0-120">Skapar en krypteringsnyckel i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-120">Creates an encryption key in Key Vault.</span></span> |
| [<span data-ttu-id="d5ee0-121">AZ ad sp skapa-för-rbac</span><span class="sxs-lookup"><span data-stu-id="d5ee0-121">az ad sp create-for-rbac</span></span>](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) | <span data-ttu-id="d5ee0-122">Skapar ett Azure Active Directory-tjänstens huvudnamn för att autentisera och kontrollera åtkomst till krypteringsnycklarna på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-122">Creates an Azure Active Directory service principal to securely authenticate and control access to encryption keys.</span></span> |
| [<span data-ttu-id="d5ee0-123">Ange en AZ keyvault-princip</span><span class="sxs-lookup"><span data-stu-id="d5ee0-123">az keyvault set-policy</span></span>](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | <span data-ttu-id="d5ee0-124">Anger behörigheter på Nyckelvalvet att ge service principal åtkomst till krypteringsnycklarna.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-124">Sets permissions on the Key Vault to grant the service principal access to encryption keys.</span></span> |
| [<span data-ttu-id="d5ee0-125">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="d5ee0-125">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="d5ee0-126">Skapar den virtuella datorn och ansluter till nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-126">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="d5ee0-127">Det här kommandot anger också avbildning av virtuell dator som ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-127">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="d5ee0-128">AZ vm kryptering aktivera</span><span class="sxs-lookup"><span data-stu-id="d5ee0-128">az vm encryption enable</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#enable) | <span data-ttu-id="d5ee0-129">Aktiverar kryptering på en virtuell dator med hjälp av service principal autentiseringsuppgifter och krypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-129">Enables encryption on a VM using the service principal credentials and encryption key.</span></span> |
| [<span data-ttu-id="d5ee0-130">AZ vm kryptering visa</span><span class="sxs-lookup"><span data-stu-id="d5ee0-130">az vm encryption show</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#show) | <span data-ttu-id="d5ee0-131">Visar status för krypteringsprocessen VM.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-131">Shows the status of the VM encryption process.</span></span> |
| [<span data-ttu-id="d5ee0-132">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="d5ee0-132">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="d5ee0-133">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="d5ee0-133">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d5ee0-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d5ee0-134">Next steps</span></span>

<span data-ttu-id="d5ee0-135">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d5ee0-135">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d5ee0-136">Ytterligare virtuella CLI skriptexempel finns i den [Virtuella för Windows Azure-dokumentationen](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d5ee0-136">Additional virtual machine CLI script samples can be found in the [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).</span></span>
