---
title: Azure CLI Script exempel - skapa en virtuell Linux-dator | Microsoft Docs
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
ms.openlocfilehash: dc7e7276bcea32c59c67a42dedaffcedfd0cf907
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-fully-configured-virtual-machine"></a><span data-ttu-id="47af5-103">Skapa en helt konfigurerade virtuell dator</span><span class="sxs-lookup"><span data-stu-id="47af5-103">Create a fully configured virtual machine</span></span>

<span data-ttu-id="47af5-104">Det här skriptet skapar en virtuell dator i Azure med ett Ubuntu-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="47af5-104">This script creates an Azure Virtual Machine with an Ubuntu operating system.</span></span> <span data-ttu-id="47af5-105">När skriptet har körts kan du komma åt den virtuella datorn via SSH.</span><span class="sxs-lookup"><span data-stu-id="47af5-105">After running the script, you can access the virtual machine over SSH.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="47af5-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="47af5-106">Sample script</span></span>

<span data-ttu-id="47af5-107">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.sh "snabbt skapa virtuell dator")]</span><span class="sxs-lookup"><span data-stu-id="47af5-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="47af5-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="47af5-108">Clean up deployment</span></span> 

<span data-ttu-id="47af5-109">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="47af5-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="47af5-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="47af5-110">Script explanation</span></span>

<span data-ttu-id="47af5-111">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="47af5-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="47af5-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="47af5-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="47af5-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="47af5-113">Command</span></span> | <span data-ttu-id="47af5-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="47af5-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="47af5-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="47af5-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="47af5-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="47af5-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="47af5-117">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="47af5-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="47af5-118">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="47af5-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="47af5-119">Skapa AZ nätverket offentliga-ip</span><span class="sxs-lookup"><span data-stu-id="47af5-119">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="47af5-120">Skapar en offentlig IP-adress med en statisk IP-adress och en tillhörande DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="47af5-120">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="47af5-121">Skapa AZ nätverket nsg</span><span class="sxs-lookup"><span data-stu-id="47af5-121">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="47af5-122">Skapar en nätverkssäkerhetsgrupp (NSG), vilket är en säkerhetsgräns mellan internet och den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="47af5-122">Creates a network security group (NSG), which is a security boundary between the internet and the virtual machine.</span></span> |
| [<span data-ttu-id="47af5-123">Skapa AZ nätverket nsg regel</span><span class="sxs-lookup"><span data-stu-id="47af5-123">az network nsg rule create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="47af5-124">Skapar en NSG-regel för att tillåta inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="47af5-124">Creates an NSG rule to allow inbound traffic.</span></span> <span data-ttu-id="47af5-125">I det här exemplet används port 22 för SSH-trafik.</span><span class="sxs-lookup"><span data-stu-id="47af5-125">In this sample, port 22 is opened for SSH traffic.</span></span> |
| [<span data-ttu-id="47af5-126">Skapa AZ nätverket nic</span><span class="sxs-lookup"><span data-stu-id="47af5-126">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="47af5-127">Skapar ett virtuellt nätverkskort som kopplas till den virtuella nätverk och undernät NSG.</span><span class="sxs-lookup"><span data-stu-id="47af5-127">Creates a virtual network card and attaches it to the virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="47af5-128">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="47af5-128">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="47af5-129">Skapar den virtuella datorn och ansluter till nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="47af5-129">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="47af5-130">Det här kommandot anger också avbildning av virtuell dator som ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="47af5-130">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="47af5-131">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="47af5-131">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="47af5-132">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="47af5-132">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="47af5-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="47af5-133">Next steps</span></span>

<span data-ttu-id="47af5-134">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="47af5-134">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="47af5-135">Ytterligare virtuella CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="47af5-135">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
