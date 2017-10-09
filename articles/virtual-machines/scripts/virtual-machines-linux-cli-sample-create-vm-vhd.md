---
title: "aaaAzure CLI skriptexempel - skapa en virtuell dator med en virtuell Hårddisk | Microsoft Docs"
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
ms.openlocfilehash: ce39092697a51e4e8a8e59ba8eb919955f616458
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a><span data-ttu-id="7a066-103">Skapa en virtuell dator med en virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="7a066-103">Create a VM with a virtual hard disk</span></span>

<span data-ttu-id="7a066-104">Det här exemplet skapar en virtuell dator med en virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="7a066-104">This example creates a virtual machine using a VHD.</span></span>
<span data-ttu-id="7a066-105">En resursgrupp, ett lagringskonto och en behållare skapas och den skapar en virtuell dator genom att ladda upp toohello hello VHD-behållare.</span><span class="sxs-lookup"><span data-stu-id="7a066-105">It creates a resource group, a storage account, and a container, then it creates a VM by uploading hello VHD toohello container.</span></span>
<span data-ttu-id="7a066-106">Ersätter hello ssh offentlig nyckel med den offentliga nyckeln så att du har åtkomst toohello VM.</span><span class="sxs-lookup"><span data-stu-id="7a066-106">It replaces hello ssh public key with your public key so that you have access toohello VM.</span></span>

<span data-ttu-id="7a066-107">Du behöver en startbar virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="7a066-107">You'll need a bootable VHD.</span></span>
<span data-ttu-id="7a066-108">Du kan hämta hello VHD som vi använde från https://azclisamples.blob.core.windows.net/vhds/sample.vhd eller använda din egen VHD.</span><span class="sxs-lookup"><span data-stu-id="7a066-108">You can download hello VHD that we used from https://azclisamples.blob.core.windows.net/vhds/sample.vhd, or use your own VHD.</span></span> <span data-ttu-id="7a066-109">hello skript söker efter `~/sample.vhd`.</span><span class="sxs-lookup"><span data-stu-id="7a066-109">hello script looks for `~/sample.vhd`.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="7a066-110">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="7a066-110">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a><span data-ttu-id="7a066-111">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="7a066-111">Clean up deployment</span></span> 

<span data-ttu-id="7a066-112">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="7a066-112">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a><span data-ttu-id="7a066-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="7a066-113">Script explanation</span></span>

<span data-ttu-id="7a066-114">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="7a066-114">This script uses hello following commands toocreate a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="7a066-115">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="7a066-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="7a066-116">Kommando</span><span class="sxs-lookup"><span data-stu-id="7a066-116">Command</span></span> | <span data-ttu-id="7a066-117">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="7a066-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7a066-118">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="7a066-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="7a066-119">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="7a066-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7a066-120">AZ lagringskontolistan</span><span class="sxs-lookup"><span data-stu-id="7a066-120">az storage account list</span></span>](https://docs.microsoft.com/cli/azure/storage/account#list) | <span data-ttu-id="7a066-121">Visar storage-konton</span><span class="sxs-lookup"><span data-stu-id="7a066-121">Lists storage accounts</span></span> |
| [<span data-ttu-id="7a066-122">AZ check-lagringskontonamnet</span><span class="sxs-lookup"><span data-stu-id="7a066-122">az storage account check-name</span></span>](https://docs.microsoft.com/cli/azure/storage/account#check-name) | <span data-ttu-id="7a066-123">Kontrollerar att namnet på ett lagringskonto är giltig och att den inte redan finns</span><span class="sxs-lookup"><span data-stu-id="7a066-123">Checks that a storage account name is valid and that it doesn't already exist</span></span> |
| [<span data-ttu-id="7a066-124">AZ nycklar lagringskontolistan</span><span class="sxs-lookup"><span data-stu-id="7a066-124">az storage account keys list</span></span>](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | <span data-ttu-id="7a066-125">Visar en lista över nycklar för hello storage-konton</span><span class="sxs-lookup"><span data-stu-id="7a066-125">Lists keys for hello storage accounts</span></span> |
| [<span data-ttu-id="7a066-126">AZ lagringsblob finns</span><span class="sxs-lookup"><span data-stu-id="7a066-126">az storage blob exists</span></span>](https://docs.microsoft.com/cli/azure/storage/blob#exists) | <span data-ttu-id="7a066-127">Kontrollerar om hello blob finns</span><span class="sxs-lookup"><span data-stu-id="7a066-127">Checks whether hello blob exists</span></span> |
| [<span data-ttu-id="7a066-128">Skapa AZ lagringsbehållare</span><span class="sxs-lookup"><span data-stu-id="7a066-128">az storage container create</span></span>](https://docs.microsoft.com/cli/azure/storage/container#create) | <span data-ttu-id="7a066-129">Skapar en behållare i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7a066-129">Creates a container in a storage account.</span></span> |
| [<span data-ttu-id="7a066-130">ladda upp AZ storage-blob</span><span class="sxs-lookup"><span data-stu-id="7a066-130">az storage blob upload</span></span>](https://docs.microsoft.com/cli/azure/storage/blob#upload) | <span data-ttu-id="7a066-131">Skapar en blob i hello-behållaren genom att ladda upp hello VHD.</span><span class="sxs-lookup"><span data-stu-id="7a066-131">Creates a blob in hello container by uploading hello VHD.</span></span> |
| [<span data-ttu-id="7a066-132">AZ vm lista</span><span class="sxs-lookup"><span data-stu-id="7a066-132">az vm list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="7a066-133">Används med `--query` kontrollera om hello VM namn används.</span><span class="sxs-lookup"><span data-stu-id="7a066-133">Used with `--query` check whether hello VM name is in use.</span></span> | 
| [<span data-ttu-id="7a066-134">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="7a066-134">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="7a066-135">Skapar hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7a066-135">Creates hello virtual machines.</span></span> |
| [<span data-ttu-id="7a066-136">AZ vm åtkomst set linux-användare</span><span class="sxs-lookup"><span data-stu-id="7a066-136">az vm access set-linux-user</span></span>](https://docs.microsoft.com/cli/azure/vm/access#set-linux-user) | <span data-ttu-id="7a066-137">Återställer hello SSH key toogive hello aktuella användare åtkomst toohello VM.</span><span class="sxs-lookup"><span data-stu-id="7a066-137">Resets hello SSH key toogive hello current user access toohello VM.</span></span> |
| [<span data-ttu-id="7a066-138">AZ vm lista-ip-adresser</span><span class="sxs-lookup"><span data-stu-id="7a066-138">az vm list-ip-addresses</span></span>](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | <span data-ttu-id="7a066-139">Hämtar hello IP-adressen för hello VM som har skapats.</span><span class="sxs-lookup"><span data-stu-id="7a066-139">Gets hello IP address of hello VM that was created.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7a066-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7a066-140">Next steps</span></span>

<span data-ttu-id="7a066-141">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7a066-141">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="7a066-142">Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7a066-142">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
