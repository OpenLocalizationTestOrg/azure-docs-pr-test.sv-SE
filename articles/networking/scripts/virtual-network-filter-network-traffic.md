---
title: "Skriptexempel Azure CLI - Filter VM-nätverkstrafik | Microsoft Docs"
description: "Azure CLI-skript exempel – filtrera inkommande och utgående VM-nätverkstrafik."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: 68ee013cff4e0be15af30239e0314f779f50177a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a><span data-ttu-id="4f6d0-103">Filtrera inkommande och utgående nätverkstrafik för VM</span><span class="sxs-lookup"><span data-stu-id="4f6d0-103">Filter inbound and outbound VM network traffic</span></span>

<span data-ttu-id="4f6d0-104">Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="4f6d0-105">Inkommande nätverkstrafik till undernätet frontend är begränsad till HTTP, HTTPS och SSH, medan utgående trafik till Internet från backend-undernät inte är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-105">Inbound network traffic to the front-end subnet is limited to HTTP, HTTPS and SSH, while outbound traffic to the Internet from the back-end subnet is not permitted.</span></span> <span data-ttu-id="4f6d0-106">När du har kört skriptet har du en virtuell dator med två nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-106">After running the script, you will have one virtual machine with two NICs.</span></span> <span data-ttu-id="4f6d0-107">Varje nätverkskort är anslutet till ett annat undernät.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-107">Each NIC is connected to a different subnet.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="4f6d0-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="4f6d0-108">Sample script</span></span>


<span data-ttu-id="4f6d0-109">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM-nätverkstrafik")]</span><span class="sxs-lookup"><span data-stu-id="4f6d0-109">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM network traffic")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="4f6d0-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="4f6d0-110">Clean up deployment</span></span> 

<span data-ttu-id="4f6d0-111">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-111">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="4f6d0-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="4f6d0-112">Script explanation</span></span>

<span data-ttu-id="4f6d0-113">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuella nätverk och nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-113">This script uses the following commands to create a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="4f6d0-114">Varje kommando i tabellen länkar till kommando-specifik dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-114">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="4f6d0-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="4f6d0-115">Command</span></span> | <span data-ttu-id="4f6d0-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="4f6d0-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4f6d0-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="4f6d0-117">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="4f6d0-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4f6d0-119">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="4f6d0-119">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="4f6d0-120">Skapar en Azure-nätverket och frontend-undernät.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-120">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="4f6d0-121">Skapa AZ undernät</span><span class="sxs-lookup"><span data-stu-id="4f6d0-121">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="4f6d0-122">Skapar en backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-122">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="4f6d0-123">AZ network vnet undernät uppdatering</span><span class="sxs-lookup"><span data-stu-id="4f6d0-123">az network vnet subnet update</span></span>](/cli/azure/network/vnet/subnet#update) | <span data-ttu-id="4f6d0-124">Associerar NSG: er till undernät.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-124">Associates NSGs to subnets.</span></span> |
| [<span data-ttu-id="4f6d0-125">Skapa AZ nätverket offentliga-ip</span><span class="sxs-lookup"><span data-stu-id="4f6d0-125">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="4f6d0-126">Skapar en offentlig IP-adress för att komma åt den virtuella datorn från Internet.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-126">Creates a public IP address to access the VM from the Internet.</span></span> |
| [<span data-ttu-id="4f6d0-127">Skapa AZ nätverket nic</span><span class="sxs-lookup"><span data-stu-id="4f6d0-127">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="4f6d0-128">Skapar virtuella nätverksgränssnitt och kopplar dem till det virtuella nätverket frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-128">Creates virtual network interfaces and attaches them to the virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="4f6d0-129">Skapa AZ nätverket nsg</span><span class="sxs-lookup"><span data-stu-id="4f6d0-129">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="4f6d0-130">Skapar säkerhetsgrupper (NSG) som är kopplade till frontend och backend-undernät för nätverket.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-130">Creates network security groups (NSG) that are associated to the front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="4f6d0-131">Skapa AZ nätverket nsg regel</span><span class="sxs-lookup"><span data-stu-id="4f6d0-131">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) |<span data-ttu-id="4f6d0-132">Skapar NSG-regler som tillåter eller blockerar specifika portar till specifika undernät.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-132">Creates NSG rules that allow or block specific ports to specific subnets.</span></span> |
| [<span data-ttu-id="4f6d0-133">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="4f6d0-133">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="4f6d0-134">Skapar virtuella datorer och bifogar ett nätverkskort på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-134">Creates virtual machines and attaches a NIC to each VM.</span></span> <span data-ttu-id="4f6d0-135">Det här kommandot anger också avbildning av virtuell dator ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-135">This command also specifies the virtual machine image to use and administrative credentials.</span></span> |
| [<span data-ttu-id="4f6d0-136">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="4f6d0-136">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="4f6d0-137">Tar bort en resursgrupp och alla resurser som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="4f6d0-137">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4f6d0-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4f6d0-138">Next steps</span></span>

<span data-ttu-id="4f6d0-139">Mer information om Azure CLI finns [Azure CLI dokumentationen](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4f6d0-139">For more information on the Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="4f6d0-140">Ytterligare nätverk CLI skriptexempel finns i den [dokumentation för Azure-nätverk – översikt](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="4f6d0-140">Additional networking CLI script samples can be found in the [Azure Networking Overview documentation](../cli-samples.md)</span></span>