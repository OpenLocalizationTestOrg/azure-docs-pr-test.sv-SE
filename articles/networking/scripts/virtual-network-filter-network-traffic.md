---
title: "aaaAzure CLI skriptexempel - Filter VM-nätverkstrafik | Microsoft Docs"
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
ms.openlocfilehash: c2f14e54bc96c99420b4300d1c24a457ac8c948c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a><span data-ttu-id="cd3fd-103">Filtrera inkommande och utgående nätverkstrafik för VM</span><span class="sxs-lookup"><span data-stu-id="cd3fd-103">Filter inbound and outbound VM network traffic</span></span>

<span data-ttu-id="cd3fd-104">Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="cd3fd-105">Inkommande nätverkstrafik toohello frontend undernät är begränsad tooHTTP, HTTPS och SSH, medan utgående trafik toohello Internet från hello backend-undernät inte är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-105">Inbound network traffic toohello front-end subnet is limited tooHTTP, HTTPS and SSH, while outbound traffic toohello Internet from hello back-end subnet is not permitted.</span></span> <span data-ttu-id="cd3fd-106">När du har kört hello skript har du en virtuell dator med två nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-106">After running hello script, you will have one virtual machine with two NICs.</span></span> <span data-ttu-id="cd3fd-107">Varje nätverkskort är anslutet tooa olika undernät.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-107">Each NIC is connected tooa different subnet.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="cd3fd-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="cd3fd-108">Sample script</span></span>


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a><span data-ttu-id="cd3fd-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="cd3fd-109">Clean up deployment</span></span> 

<span data-ttu-id="cd3fd-110">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-110">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="cd3fd-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="cd3fd-111">Script explanation</span></span>

<span data-ttu-id="cd3fd-112">Det här skriptet använder följande kommandon toocreate hello en resursgrupp, virtuella nätverk och nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-112">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="cd3fd-113">Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-113">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="cd3fd-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="cd3fd-114">Command</span></span> | <span data-ttu-id="cd3fd-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="cd3fd-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cd3fd-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="cd3fd-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="cd3fd-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="cd3fd-118">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="cd3fd-118">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="cd3fd-119">Skapar en Azure-nätverket och frontend-undernät.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-119">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="cd3fd-120">Skapa AZ undernät</span><span class="sxs-lookup"><span data-stu-id="cd3fd-120">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="cd3fd-121">Skapar en backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-121">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="cd3fd-122">AZ network vnet undernät uppdatering</span><span class="sxs-lookup"><span data-stu-id="cd3fd-122">az network vnet subnet update</span></span>](/cli/azure/network/vnet/subnet#update) | <span data-ttu-id="cd3fd-123">Associerar NSG: er toosubnets.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-123">Associates NSGs toosubnets.</span></span> |
| [<span data-ttu-id="cd3fd-124">Skapa AZ nätverket offentliga-ip</span><span class="sxs-lookup"><span data-stu-id="cd3fd-124">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="cd3fd-125">Skapar en offentlig IP-adress tooaccess hello VM från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-125">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="cd3fd-126">Skapa AZ nätverket nic</span><span class="sxs-lookup"><span data-stu-id="cd3fd-126">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="cd3fd-127">Skapar virtuella nätverksgränssnitt och bifogar dem toohello virtuellt frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-127">Creates virtual network interfaces and attaches them toohello virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="cd3fd-128">Skapa AZ nätverket nsg</span><span class="sxs-lookup"><span data-stu-id="cd3fd-128">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="cd3fd-129">Skapar nätverkssäkerhetsgrupper (NSG) som är associerade toohello frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-129">Creates network security groups (NSG) that are associated toohello front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="cd3fd-130">Skapa AZ nätverket nsg regel</span><span class="sxs-lookup"><span data-stu-id="cd3fd-130">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) |<span data-ttu-id="cd3fd-131">Skapar NSG-regler som tillåter eller blockerar specifika portar toospecific undernät.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-131">Creates NSG rules that allow or block specific ports toospecific subnets.</span></span> |
| [<span data-ttu-id="cd3fd-132">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="cd3fd-132">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="cd3fd-133">Skapar virtuella datorer och bifogar ett NIC-tooeach VM.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-133">Creates virtual machines and attaches a NIC tooeach VM.</span></span> <span data-ttu-id="cd3fd-134">Det här kommandot anger också hello virtuella avbildningen toouse och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-134">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="cd3fd-135">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="cd3fd-135">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="cd3fd-136">Tar bort en resursgrupp och alla resurser som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="cd3fd-136">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cd3fd-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cd3fd-137">Next steps</span></span>

<span data-ttu-id="cd3fd-138">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cd3fd-138">For more information on hello Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="cd3fd-139">Ytterligare nätverk CLI skriptexempel finns i hello [dokumentation för Azure-nätverk – översikt](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="cd3fd-139">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md)</span></span>
