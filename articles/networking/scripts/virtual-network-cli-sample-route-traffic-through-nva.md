---
title: "aaaAzure CLI skriptexempel - vägtrafik via en virtuell nätverksenhet | Microsoft Docs"
description: "Azure CLI skriptexempel - vägtrafik via en virtuell nätverksenhet för brandväggen."
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
ms.openlocfilehash: 981d6073be04a7ebaf96b657fbab8a378e7a995e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a><span data-ttu-id="2c507-103">Vidarebefordra trafik via en virtuell nätverksenhet</span><span class="sxs-lookup"><span data-stu-id="2c507-103">Route traffic through a network virtual appliance</span></span>

<span data-ttu-id="2c507-104">Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="2c507-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="2c507-105">Dessutom skapas en virtuell dator med IP-vidarebefordring aktiverade tooroute trafik mellan hello två undernät.</span><span class="sxs-lookup"><span data-stu-id="2c507-105">It also creates a VM with IP forwarding enabled tooroute traffic between hello two subnets.</span></span> <span data-ttu-id="2c507-106">Du kan distribuera programvara för nätverk, till exempel en brandvägg programmet toohello VM när du har kört hello skript.</span><span class="sxs-lookup"><span data-stu-id="2c507-106">After running hello script you can deploy network software, such as a firewall application, toohello VM.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="2c507-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="2c507-107">Sample script</span></span>


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a><span data-ttu-id="2c507-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="2c507-108">Clean up deployment</span></span> 

<span data-ttu-id="2c507-109">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="2c507-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="2c507-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="2c507-110">Script explanation</span></span>

<span data-ttu-id="2c507-111">Det här skriptet använder följande kommandon toocreate hello en resursgrupp, virtuella nätverk och nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="2c507-111">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="2c507-112">Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="2c507-112">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="2c507-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="2c507-113">Command</span></span> | <span data-ttu-id="2c507-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="2c507-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2c507-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="2c507-115">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="2c507-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="2c507-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2c507-117">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="2c507-117">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="2c507-118">Skapar en Azure-nätverket och frontend-undernät.</span><span class="sxs-lookup"><span data-stu-id="2c507-118">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="2c507-119">Skapa AZ undernät</span><span class="sxs-lookup"><span data-stu-id="2c507-119">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="2c507-120">Skapar backend- och DMZ undernät.</span><span class="sxs-lookup"><span data-stu-id="2c507-120">Creates back-end and DMZ subnets.</span></span> |
| [<span data-ttu-id="2c507-121">Skapa AZ nätverket offentliga-ip</span><span class="sxs-lookup"><span data-stu-id="2c507-121">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="2c507-122">Skapar en offentlig IP-adress tooaccess hello VM från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="2c507-122">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="2c507-123">Skapa AZ nätverket nic</span><span class="sxs-lookup"><span data-stu-id="2c507-123">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="2c507-124">Skapar ett virtuellt nätverksgränssnitt och aktivera IP-vidarebefordring för den.</span><span class="sxs-lookup"><span data-stu-id="2c507-124">Creates a virtual network interface and enable IP forwarding for it.</span></span> |
| [<span data-ttu-id="2c507-125">Skapa AZ nätverket nsg</span><span class="sxs-lookup"><span data-stu-id="2c507-125">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="2c507-126">Skapar en nätverkssäkerhetsgrupp (NSG).</span><span class="sxs-lookup"><span data-stu-id="2c507-126">Creates a network security group (NSG).</span></span> |
| [<span data-ttu-id="2c507-127">Skapa AZ nätverket nsg regel</span><span class="sxs-lookup"><span data-stu-id="2c507-127">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) | <span data-ttu-id="2c507-128">Skapar NSG-regler som tillåter HTTP och HTTPS-portar inkommande toohello VM.</span><span class="sxs-lookup"><span data-stu-id="2c507-128">Creates NSG rules that allow HTTP and HTTPS ports inbound toohello VM.</span></span> |
| [<span data-ttu-id="2c507-129">AZ network vnet undernät uppdatering</span><span class="sxs-lookup"><span data-stu-id="2c507-129">az network vnet subnet update</span></span>](/cli/azure/network/vnet/subnet#update)| <span data-ttu-id="2c507-130">Associerats hello NSG: er och väg tabeller toosubnets.</span><span class="sxs-lookup"><span data-stu-id="2c507-130">Associates hello NSGs and route tables toosubnets.</span></span> |
| [<span data-ttu-id="2c507-131">Skapa AZ-routningstabellen för nätverk</span><span class="sxs-lookup"><span data-stu-id="2c507-131">az network route-table create</span></span>](/cli/azure/network/route-table#create)| <span data-ttu-id="2c507-132">Skapar en routningstabell för alla flöden.</span><span class="sxs-lookup"><span data-stu-id="2c507-132">Creates a route table for all routes.</span></span> |
| [<span data-ttu-id="2c507-133">Skapa AZ nätverksväg routningstabellen</span><span class="sxs-lookup"><span data-stu-id="2c507-133">az network route-table route create</span></span>](/cli/azure/network/route-table/route#create)| <span data-ttu-id="2c507-134">Skapar vägar tooroute trafik mellan undernät och hello Internet via hello VM.</span><span class="sxs-lookup"><span data-stu-id="2c507-134">Creates routes tooroute traffic between subnets and hello Internet through hello VM.</span></span> |
| [<span data-ttu-id="2c507-135">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="2c507-135">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="2c507-136">Skapar en virtuell dator och bifogar hello NIC tooit.</span><span class="sxs-lookup"><span data-stu-id="2c507-136">Creates a virtual machine and attaches hello NIC tooit.</span></span> <span data-ttu-id="2c507-137">Det här kommandot anger också hello virtuella avbildningen toouse och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2c507-137">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="2c507-138">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="2c507-138">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="2c507-139">Tar bort en resursgrupp och alla resurser som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="2c507-139">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2c507-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2c507-140">Next steps</span></span>

<span data-ttu-id="2c507-141">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2c507-141">For more information on hello Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="2c507-142">Ytterligare nätverk CLI skriptexempel finns i hello [dokumentation för Azure-nätverk – översikt](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="2c507-142">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md)</span></span>
