---
title: aaaAzure CLI skriptexempel - skapa en Windows Server-VM | Microsoft Docs
description: Azure CLI Script exempel - skapa en Windows Server-dator
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: f4cb431a68c911f877f5af87c393849bd8d2676a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="9958d-103">Skapa en virtuell dator med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9958d-103">Create a virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="9958d-104">Det här skriptet skapar en Azure-dator som kör Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="9958d-104">This script creates an Azure Virtual Machine running Windows Server 2016.</span></span> <span data-ttu-id="9958d-105">Du kan komma åt hello virtuella datorn via en fjärrskrivbordsanslutning när du har kört hello skript.</span><span class="sxs-lookup"><span data-stu-id="9958d-105">After running hello script, you can access hello virtual machine through a Remote Desktop connection.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="9958d-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="9958d-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-windows-vm-detailed.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="9958d-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="9958d-107">Clean up deployment</span></span> 

<span data-ttu-id="9958d-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="9958d-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="9958d-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="9958d-109">Script explanation</span></span>

<span data-ttu-id="9958d-110">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, en virtuell dator, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="9958d-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="9958d-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="9958d-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="9958d-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="9958d-112">Command</span></span> | <span data-ttu-id="9958d-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="9958d-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9958d-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="9958d-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="9958d-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="9958d-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9958d-116">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="9958d-116">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="9958d-117">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="9958d-117">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="9958d-118">Skapa AZ nätverket offentliga-ip</span><span class="sxs-lookup"><span data-stu-id="9958d-118">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="9958d-119">Skapar en offentlig IP-adress med en statisk IP-adress och en tillhörande DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="9958d-119">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="9958d-120">Skapa AZ nätverket nsg</span><span class="sxs-lookup"><span data-stu-id="9958d-120">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="9958d-121">Skapar en nätverkssäkerhetsgrupp (NSG), vilket är en säkerhetsgräns mellan hello internet och hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9958d-121">Creates a network security group (NSG), which is a security boundary between hello internet and hello virtual machine.</span></span> |
| [<span data-ttu-id="9958d-122">Skapa AZ nätverket nic</span><span class="sxs-lookup"><span data-stu-id="9958d-122">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="9958d-123">Skapar ett virtuellt nätverkskort och bifogar den toohello virtuella nätverk och undernät NSG.</span><span class="sxs-lookup"><span data-stu-id="9958d-123">Creates a virtual network card and attaches it toohello virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="9958d-124">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="9958d-124">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="9958d-125">Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="9958d-125">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="9958d-126">Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="9958d-126">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="9958d-127">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="9958d-127">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="9958d-128">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="9958d-128">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9958d-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9958d-129">Next steps</span></span>

<span data-ttu-id="9958d-130">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9958d-130">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="9958d-131">Ytterligare virtuella CLI skriptexempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9958d-131">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
