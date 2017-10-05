---
title: "Azure CLI-skript exempel – skapa två virtuella datorer med en interna och externa NSG | Microsoft Docs"
description: "Azure CLI-skript exempel – skapa två virtuella datorer med interna och externa NSG"
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
ms.openlocfilehash: 7bd8c315ab0b7b3bcbbd9ebc8f17728079611f9c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="secure-network-traffic-between-virtual-machines"></a><span data-ttu-id="98d31-103">Säkra nätverkstrafik mellan virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="98d31-103">Secure network traffic between virtual machines</span></span>

<span data-ttu-id="98d31-104">Det här skriptet skapar två virtuella datorer och skyddar inkommande trafik till båda.</span><span class="sxs-lookup"><span data-stu-id="98d31-104">This script creates two virtual machines and secures incoming traffic to both.</span></span> <span data-ttu-id="98d31-105">En virtuell dator är tillgänglig på internet och har en nätverkssäkerhetsgrupp (NSG) konfigurerat för att tillåta trafik på port 22 och port 80.</span><span class="sxs-lookup"><span data-stu-id="98d31-105">One virtual machine is accessible on the internet and has a network security group (NSG) configured to allow traffic on port 22 and port 80.</span></span> <span data-ttu-id="98d31-106">Den andra virtuella datorn är inte tillgänglig på internet och har en NSG som konfigurerats för att endast tillåta trafik från den första virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="98d31-106">The second virtual machine is not accessible on the internet, and has an NSG configured to only allow traffic from the first virtual machine.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="98d31-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="98d31-107">Sample script</span></span>

<span data-ttu-id="98d31-108">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-vm-nsg/create-vm-nsg.sh "Skapa virtuell dator med NSG")]</span><span class="sxs-lookup"><span data-stu-id="98d31-108">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-vm-nsg.sh "Create VM with NSG")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="98d31-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="98d31-109">Clean up deployment</span></span> 

<span data-ttu-id="98d31-110">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="98d31-110">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="98d31-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="98d31-111">Script explanation</span></span>

<span data-ttu-id="98d31-112">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="98d31-112">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="98d31-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="98d31-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="98d31-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="98d31-114">Command</span></span> | <span data-ttu-id="98d31-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="98d31-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="98d31-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="98d31-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="98d31-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="98d31-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="98d31-118">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="98d31-118">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="98d31-119">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="98d31-119">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="98d31-120">Skapa AZ undernät för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="98d31-120">az network vnet subnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="98d31-121">Skapar ett undernät.</span><span class="sxs-lookup"><span data-stu-id="98d31-121">Creates a subnet.</span></span> |
| [<span data-ttu-id="98d31-122">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="98d31-122">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="98d31-123">Skapar den virtuella datorn och ansluter till nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="98d31-123">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="98d31-124">Det här kommandot anger också avbildning av virtuell dator som ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="98d31-124">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="98d31-125">listan över AZ nätverk nsg regel</span><span class="sxs-lookup"><span data-stu-id="98d31-125">az network nsg rule list</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#list) | <span data-ttu-id="98d31-126">Returnerar information om en grupp för nätverkssäkerhetsregeln.</span><span class="sxs-lookup"><span data-stu-id="98d31-126">Returns information about a network security group rule.</span></span> <span data-ttu-id="98d31-127">I det här exemplet lagras Regelnamnet i en variabel för användning senare i skriptet.</span><span class="sxs-lookup"><span data-stu-id="98d31-127">In this sample, the rule name is stored in a variable for use later in the script.</span></span> |
| [<span data-ttu-id="98d31-128">Uppdatera regel för AZ nätverket nsg</span><span class="sxs-lookup"><span data-stu-id="98d31-128">az network nsg rule update</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#update) | <span data-ttu-id="98d31-129">Uppdaterar en NSG-regel.</span><span class="sxs-lookup"><span data-stu-id="98d31-129">Updates an NSG rule.</span></span> <span data-ttu-id="98d31-130">I det här exemplet uppdateras backend-regeln för att släppa igenom trafik från frontend undernätet.</span><span class="sxs-lookup"><span data-stu-id="98d31-130">In this sample, the back-end rule is updated to pass through traffic only from the front-end subnet.</span></span> |
| [<span data-ttu-id="98d31-131">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="98d31-131">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="98d31-132">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="98d31-132">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="98d31-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="98d31-133">Next steps</span></span>

<span data-ttu-id="98d31-134">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="98d31-134">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="98d31-135">Ytterligare virtuella CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="98d31-135">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
