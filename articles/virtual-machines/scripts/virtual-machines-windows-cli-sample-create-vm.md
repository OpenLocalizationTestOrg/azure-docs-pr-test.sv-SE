---
title: Azure CLI Script exempel - skapa en Windows Server-dator | Microsoft Docs
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
ms.openlocfilehash: 9690e743e498a55d7339b6f51861fac37c9b0f56
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-with-the-azure-cli"></a><span data-ttu-id="cbbb6-103">Skapa en virtuell dator med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="cbbb6-103">Create a virtual machine with the Azure CLI</span></span>

<span data-ttu-id="cbbb6-104">Det här skriptet skapar en Azure-dator som kör Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="cbbb6-104">This script creates an Azure Virtual Machine running Windows Server 2016.</span></span> <span data-ttu-id="cbbb6-105">När skriptet har körts kan du komma åt den virtuella datorn via en fjärrskrivbordsanslutning.</span><span class="sxs-lookup"><span data-stu-id="cbbb6-105">After running the script, you can access the virtual machine through a Remote Desktop connection.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="cbbb6-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="cbbb6-106">Sample script</span></span>

<span data-ttu-id="cbbb6-107">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-vm-detailed/create-windows-vm-detailed.sh "snabbt skapa virtuell dator")]</span><span class="sxs-lookup"><span data-stu-id="cbbb6-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-windows-vm-detailed.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="cbbb6-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="cbbb6-108">Clean up deployment</span></span> 

<span data-ttu-id="cbbb6-109">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="cbbb6-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="cbbb6-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="cbbb6-110">Script explanation</span></span>

<span data-ttu-id="cbbb6-111">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="cbbb6-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="cbbb6-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="cbbb6-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="cbbb6-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="cbbb6-113">Command</span></span> | <span data-ttu-id="cbbb6-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="cbbb6-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cbbb6-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="cbbb6-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="cbbb6-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="cbbb6-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="cbbb6-117">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="cbbb6-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="cbbb6-118">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="cbbb6-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="cbbb6-119">Skapa AZ nätverket offentliga-ip</span><span class="sxs-lookup"><span data-stu-id="cbbb6-119">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="cbbb6-120">Skapar en offentlig IP-adress med en statisk IP-adress och en tillhörande DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="cbbb6-120">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="cbbb6-121">Skapa AZ nätverket nsg</span><span class="sxs-lookup"><span data-stu-id="cbbb6-121">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="cbbb6-122">Skapar en nätverkssäkerhetsgrupp (NSG), vilket är en säkerhetsgräns mellan internet och den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="cbbb6-122">Creates a network security group (NSG), which is a security boundary between the internet and the virtual machine.</span></span> |
| [<span data-ttu-id="cbbb6-123">Skapa AZ nätverket nic</span><span class="sxs-lookup"><span data-stu-id="cbbb6-123">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="cbbb6-124">Skapar ett virtuellt nätverkskort som kopplas till den virtuella nätverk och undernät NSG.</span><span class="sxs-lookup"><span data-stu-id="cbbb6-124">Creates a virtual network card and attaches it to the virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="cbbb6-125">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="cbbb6-125">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="cbbb6-126">Skapar den virtuella datorn och ansluter till nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="cbbb6-126">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="cbbb6-127">Det här kommandot anger också avbildning av virtuell dator som ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="cbbb6-127">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="cbbb6-128">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="cbbb6-128">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="cbbb6-129">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="cbbb6-129">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cbbb6-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cbbb6-130">Next steps</span></span>

<span data-ttu-id="cbbb6-131">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cbbb6-131">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="cbbb6-132">Ytterligare virtuella CLI skriptexempel finns i den [Virtuella för Windows Azure-dokumentationen](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cbbb6-132">Additional virtual machine CLI script samples can be found in the [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
