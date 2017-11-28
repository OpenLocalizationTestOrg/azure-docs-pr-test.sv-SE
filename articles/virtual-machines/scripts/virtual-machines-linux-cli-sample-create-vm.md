---
title: aaaAzure CLI skriptexempel - skapa en Linux VM | Microsoft Docs
description: Azure CLI Script exempel - skapa en virtuell Linux-dator
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c9855204a85cc0f6cd758a1d20121fbeea070943
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-configured-virtual-machine"></a><span data-ttu-id="4a721-103">Skapa en helt konfigurerade virtuell dator</span><span class="sxs-lookup"><span data-stu-id="4a721-103">Create a fully configured virtual machine</span></span>

<span data-ttu-id="4a721-104">Det här skriptet skapar en virtuell dator i Azure med ett Ubuntu-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="4a721-104">This script creates an Azure Virtual Machine with an Ubuntu operating system.</span></span> <span data-ttu-id="4a721-105">Du kan komma åt hello virtuella datorn när du har kört hello skript via SSH.</span><span class="sxs-lookup"><span data-stu-id="4a721-105">After running hello script, you can access hello virtual machine over SSH.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="4a721-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="4a721-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="4a721-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="4a721-107">Clean up deployment</span></span> 

<span data-ttu-id="4a721-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="4a721-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="4a721-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="4a721-109">Script explanation</span></span>

<span data-ttu-id="4a721-110">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, en virtuell dator, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="4a721-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="4a721-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4a721-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4a721-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="4a721-112">Command</span></span> | <span data-ttu-id="4a721-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="4a721-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4a721-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="4a721-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="4a721-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="4a721-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4a721-116">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="4a721-116">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="4a721-117">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="4a721-117">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="4a721-118">Skapa AZ nätverket offentliga-ip</span><span class="sxs-lookup"><span data-stu-id="4a721-118">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="4a721-119">Skapar en offentlig IP-adress med en statisk IP-adress och en tillhörande DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="4a721-119">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="4a721-120">Skapa AZ nätverket nsg</span><span class="sxs-lookup"><span data-stu-id="4a721-120">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="4a721-121">Skapar en nätverkssäkerhetsgrupp (NSG), vilket är en säkerhetsgräns mellan hello internet och hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="4a721-121">Creates a network security group (NSG), which is a security boundary between hello internet and hello virtual machine.</span></span> |
| [<span data-ttu-id="4a721-122">Skapa AZ nätverket nsg regel</span><span class="sxs-lookup"><span data-stu-id="4a721-122">az network nsg rule create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="4a721-123">Skapar en NSG regeln tooallow inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="4a721-123">Creates an NSG rule tooallow inbound traffic.</span></span> <span data-ttu-id="4a721-124">I det här exemplet används port 22 för SSH-trafik.</span><span class="sxs-lookup"><span data-stu-id="4a721-124">In this sample, port 22 is opened for SSH traffic.</span></span> |
| [<span data-ttu-id="4a721-125">Skapa AZ nätverket nic</span><span class="sxs-lookup"><span data-stu-id="4a721-125">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="4a721-126">Skapar ett virtuellt nätverkskort och bifogar den toohello virtuella nätverk och undernät NSG.</span><span class="sxs-lookup"><span data-stu-id="4a721-126">Creates a virtual network card and attaches it toohello virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="4a721-127">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="4a721-127">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="4a721-128">Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="4a721-128">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="4a721-129">Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="4a721-129">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="4a721-130">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="4a721-130">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="4a721-131">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="4a721-131">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4a721-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a721-132">Next steps</span></span>

<span data-ttu-id="4a721-133">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4a721-133">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4a721-134">Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4a721-134">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
