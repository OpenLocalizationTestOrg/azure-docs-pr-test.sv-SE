---
title: "Azure CLI-skript exempel – skapa två virtuella datorer med en interna och externa NSG | Microsoft Docs"
description: "Azure CLI-skript exempel – skapa två virtuella datorer med interna och externa NSG"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: cc80e3fc5aaa12200e9a441db9d4a9c613c0548a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="secure-network-traffic-between-virtual-machines"></a><span data-ttu-id="6a482-103">Säkra nätverkstrafik mellan virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="6a482-103">Secure network traffic between virtual machines</span></span>

<span data-ttu-id="6a482-104">Det här skriptet skapar två virtuella datorer och skyddar inkommande trafik till båda.</span><span class="sxs-lookup"><span data-stu-id="6a482-104">This script creates two virtual machines and secures incoming traffic to both.</span></span> <span data-ttu-id="6a482-105">En virtuell dator är tillgänglig på internet och har en nätverkssäkerhetsgrupp (NSG) konfigurerat för att tillåta trafik på port 3389 och port 80.</span><span class="sxs-lookup"><span data-stu-id="6a482-105">One virtual machine is accessible on the internet and has a network security group (NSG) configured to allow traffic on port 3389 and port 80.</span></span> <span data-ttu-id="6a482-106">Den andra virtuella datorn är inte tillgänglig på internet och har en NSG som konfigurerats för att endast tillåta trafik från den första virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="6a482-106">The second virtual machine is not accessible on the internet, and has an NSG configured to only allow traffic from the first virtual machine.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="6a482-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="6a482-107">Sample script</span></span>

<span data-ttu-id="6a482-108">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-vm-nsg/create-windows-vm-nsg.sh "Skapa virtuell dator med NSG")]</span><span class="sxs-lookup"><span data-stu-id="6a482-108">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-windows-vm-nsg.sh "Create VM with NSG")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="6a482-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="6a482-109">Clean up deployment</span></span> 

<span data-ttu-id="6a482-110">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="6a482-110">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="6a482-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="6a482-111">Script explanation</span></span>

<span data-ttu-id="6a482-112">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="6a482-112">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="6a482-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="6a482-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="6a482-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="6a482-114">Command</span></span> | <span data-ttu-id="6a482-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="6a482-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6a482-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="6a482-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="6a482-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="6a482-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="6a482-118">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="6a482-118">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="6a482-119">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="6a482-119">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="6a482-120">Skapa AZ undernät för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="6a482-120">az network vnet subnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="6a482-121">Skapar ett undernät.</span><span class="sxs-lookup"><span data-stu-id="6a482-121">Creates a subnet.</span></span> |
| [<span data-ttu-id="6a482-122">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="6a482-122">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="6a482-123">Skapar den virtuella datorn och ansluter till nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="6a482-123">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="6a482-124">Det här kommandot anger också avbildning av virtuell dator som ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="6a482-124">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="6a482-125">Uppdatera regel för AZ nätverket nsg</span><span class="sxs-lookup"><span data-stu-id="6a482-125">az network nsg rule update</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#update) | <span data-ttu-id="6a482-126">Uppdaterar en NSG-regel.</span><span class="sxs-lookup"><span data-stu-id="6a482-126">Updates an NSG rule.</span></span> <span data-ttu-id="6a482-127">I det här exemplet uppdateras backend-regeln för att släppa igenom trafik från frontend undernätet.</span><span class="sxs-lookup"><span data-stu-id="6a482-127">In this sample, the back-end rule is updated to pass through traffic only from the front-end subnet.</span></span> |
| [<span data-ttu-id="6a482-128">listan över AZ nätverk nsg regel</span><span class="sxs-lookup"><span data-stu-id="6a482-128">az network nsg rule list</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#list) | <span data-ttu-id="6a482-129">Returnerar information om en grupp för nätverkssäkerhetsregeln.</span><span class="sxs-lookup"><span data-stu-id="6a482-129">Returns information about a network security group rule.</span></span> <span data-ttu-id="6a482-130">I det här exemplet lagras Regelnamnet i en variabel för användning senare i skriptet.</span><span class="sxs-lookup"><span data-stu-id="6a482-130">In this sample, the rule name is stored in a variable for use later in the script.</span></span> |
| [<span data-ttu-id="6a482-131">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="6a482-131">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="6a482-132">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="6a482-132">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6a482-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a482-133">Next steps</span></span>

<span data-ttu-id="6a482-134">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6a482-134">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="6a482-135">Ytterligare virtuella CLI skriptexempel finns i den [Virtuella för Windows Azure-dokumentationen](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6a482-135">Additional virtual machine CLI script samples can be found in the [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
