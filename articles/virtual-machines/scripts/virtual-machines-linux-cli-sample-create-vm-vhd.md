---
title: "Azure CLI Script exempel - skapa en virtuell dator med en virtuell Hårddisk | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en virtuell dator med hjälp av en virtuell hårddisk."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/09/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: fab65296a552c1839522c5254a868a3dc96227f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a><span data-ttu-id="c414e-103">Skapa en virtuell dator med en virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="c414e-103">Create a VM with a virtual hard disk</span></span>

<span data-ttu-id="c414e-104">Det här exemplet skapar en virtuell dator med en virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="c414e-104">This example creates a virtual machine using a VHD.</span></span>
<span data-ttu-id="c414e-105">En resursgrupp, ett lagringskonto och en behållare skapas och den skapar en virtuell dator genom att överföra den virtuella Hårddisken till behållaren.</span><span class="sxs-lookup"><span data-stu-id="c414e-105">It creates a resource group, a storage account, and a container, then it creates a VM by uploading the VHD to the container.</span></span>
<span data-ttu-id="c414e-106">Ersätter den ssh offentlig nyckel med den offentliga nyckeln så som du har åtkomst till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c414e-106">It replaces the ssh public key with your public key so that you have access to the VM.</span></span>

<span data-ttu-id="c414e-107">Du behöver en startbar virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="c414e-107">You'll need a bootable VHD.</span></span>
<span data-ttu-id="c414e-108">Du kan hämta den virtuella Hårddisken som vi använde från https://azclisamples.blob.core.windows.net/vhds/sample.vhd eller använda din egen VHD.</span><span class="sxs-lookup"><span data-stu-id="c414e-108">You can download the VHD that we used from https://azclisamples.blob.core.windows.net/vhds/sample.vhd, or use your own VHD.</span></span> <span data-ttu-id="c414e-109">Skriptet söker efter `~/sample.vhd`.</span><span class="sxs-lookup"><span data-stu-id="c414e-109">The script looks for `~/sample.vhd`.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="c414e-110">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="c414e-110">Sample script</span></span>

<span data-ttu-id="c414e-111">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Skapa virtuell dator med en virtuell Hårddisk")]</span><span class="sxs-lookup"><span data-stu-id="c414e-111">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="c414e-112">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="c414e-112">Clean up deployment</span></span> 

<span data-ttu-id="c414e-113">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="c414e-113">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a><span data-ttu-id="c414e-114">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="c414e-114">Script explanation</span></span>

<span data-ttu-id="c414e-115">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="c414e-115">This script uses the following commands to create a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="c414e-116">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="c414e-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c414e-117">Kommando</span><span class="sxs-lookup"><span data-stu-id="c414e-117">Command</span></span> | <span data-ttu-id="c414e-118">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="c414e-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c414e-119">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="c414e-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="c414e-120">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="c414e-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c414e-121">AZ lagringskontolistan</span><span class="sxs-lookup"><span data-stu-id="c414e-121">az storage account list</span></span>](https://docs.microsoft.com/cli/azure/storage/account#list) | <span data-ttu-id="c414e-122">Visar storage-konton</span><span class="sxs-lookup"><span data-stu-id="c414e-122">Lists storage accounts</span></span> |
| [<span data-ttu-id="c414e-123">AZ check-lagringskontonamnet</span><span class="sxs-lookup"><span data-stu-id="c414e-123">az storage account check-name</span></span>](https://docs.microsoft.com/cli/azure/storage/account#check-name) | <span data-ttu-id="c414e-124">Kontrollerar att namnet på ett lagringskonto är giltig och att den inte redan finns</span><span class="sxs-lookup"><span data-stu-id="c414e-124">Checks that a storage account name is valid and that it doesn't already exist</span></span> |
| [<span data-ttu-id="c414e-125">AZ nycklar lagringskontolistan</span><span class="sxs-lookup"><span data-stu-id="c414e-125">az storage account keys list</span></span>](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | <span data-ttu-id="c414e-126">Visar en lista över nycklar för storage-konton</span><span class="sxs-lookup"><span data-stu-id="c414e-126">Lists keys for the storage accounts</span></span> |
| [<span data-ttu-id="c414e-127">AZ lagringsblob finns</span><span class="sxs-lookup"><span data-stu-id="c414e-127">az storage blob exists</span></span>](https://docs.microsoft.com/cli/azure/storage/blob#exists) | <span data-ttu-id="c414e-128">Kontrollerar om blob finns</span><span class="sxs-lookup"><span data-stu-id="c414e-128">Checks whether the blob exists</span></span> |
| [<span data-ttu-id="c414e-129">Skapa AZ lagringsbehållare</span><span class="sxs-lookup"><span data-stu-id="c414e-129">az storage container create</span></span>](https://docs.microsoft.com/cli/azure/storage/container#create) | <span data-ttu-id="c414e-130">Skapar en behållare i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="c414e-130">Creates a container in a storage account.</span></span> |
| [<span data-ttu-id="c414e-131">ladda upp AZ storage-blob</span><span class="sxs-lookup"><span data-stu-id="c414e-131">az storage blob upload</span></span>](https://docs.microsoft.com/cli/azure/storage/blob#upload) | <span data-ttu-id="c414e-132">Skapar en blob-behållare genom att överföra den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="c414e-132">Creates a blob in the container by uploading the VHD.</span></span> |
| [<span data-ttu-id="c414e-133">AZ vm lista</span><span class="sxs-lookup"><span data-stu-id="c414e-133">az vm list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="c414e-134">Används med `--query` kontrollera om VM-namn används.</span><span class="sxs-lookup"><span data-stu-id="c414e-134">Used with `--query` check whether the VM name is in use.</span></span> | 
| [<span data-ttu-id="c414e-135">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="c414e-135">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="c414e-136">Skapar de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="c414e-136">Creates the virtual machines.</span></span> |
| [<span data-ttu-id="c414e-137">AZ vm åtkomst set linux-användare</span><span class="sxs-lookup"><span data-stu-id="c414e-137">az vm access set-linux-user</span></span>](https://docs.microsoft.com/cli/azure/vm/access#set-linux-user) | <span data-ttu-id="c414e-138">Återställer SSH-nyckel om du vill ge den aktuella användaråtkomsten till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c414e-138">Resets the SSH key to give the current user access to the VM.</span></span> |
| [<span data-ttu-id="c414e-139">AZ vm lista-ip-adresser</span><span class="sxs-lookup"><span data-stu-id="c414e-139">az vm list-ip-addresses</span></span>](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | <span data-ttu-id="c414e-140">Hämtar IP-adressen för den virtuella datorn som har skapats.</span><span class="sxs-lookup"><span data-stu-id="c414e-140">Gets the IP address of the VM that was created.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c414e-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c414e-141">Next steps</span></span>

<span data-ttu-id="c414e-142">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c414e-142">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c414e-143">Ytterligare virtuella CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c414e-143">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
