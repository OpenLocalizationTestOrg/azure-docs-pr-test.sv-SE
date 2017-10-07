---
title: "aaaAzure CLI skript exempel – skapa ett nätverk för flera nivåer program | Microsoft Docs"
description: "Azure CLI-skript exempel – skapa ett virtuellt nätverk för program på flera nivåer."
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
ms.openlocfilehash: deeb3f459499cebd1b8ded6a299eb759d49cf08d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a><span data-ttu-id="17030-103">Skapa ett nätverk för flera nivåer program</span><span class="sxs-lookup"><span data-stu-id="17030-103">Create a network for multi-tier applications</span></span>

<span data-ttu-id="17030-104">Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="17030-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="17030-105">Trafik toohello frontend undernät är begränsad tooHTTP och SSH, medan trafik toohello backend-undernät är begränsad tooMySQL, port 3306.</span><span class="sxs-lookup"><span data-stu-id="17030-105">Traffic toohello front-end subnet is limited tooHTTP and SSH, while traffic toohello back-end subnet is limited tooMySQL, port 3306.</span></span> <span data-ttu-id="17030-106">Efter körs hello skript har två virtuella datorer i varje undernät som du kan distribuera webbservern och MySQL programvaran till.</span><span class="sxs-lookup"><span data-stu-id="17030-106">After running hello script, you will have two virtual machines, one in each subnet that you can deploy web server and MySQL software to.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="17030-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="17030-107">Sample script</span></span>


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a><span data-ttu-id="17030-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="17030-108">Clean up deployment</span></span> 

<span data-ttu-id="17030-109">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="17030-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="17030-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="17030-110">Script explanation</span></span>

<span data-ttu-id="17030-111">Det här skriptet använder följande kommandon toocreate hello en resursgrupp, virtuella nätverk och nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="17030-111">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="17030-112">Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="17030-112">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="17030-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="17030-113">Command</span></span> | <span data-ttu-id="17030-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="17030-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="17030-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="17030-115">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="17030-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="17030-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="17030-117">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="17030-117">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="17030-118">Skapar en Azure-nätverket och frontend-undernät.</span><span class="sxs-lookup"><span data-stu-id="17030-118">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="17030-119">Skapa AZ undernät</span><span class="sxs-lookup"><span data-stu-id="17030-119">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="17030-120">Skapar en backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="17030-120">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="17030-121">Skapa AZ nätverket offentliga-ip</span><span class="sxs-lookup"><span data-stu-id="17030-121">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="17030-122">Skapar en offentlig IP-adress tooaccess hello VM från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="17030-122">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="17030-123">Skapa AZ nätverket nic</span><span class="sxs-lookup"><span data-stu-id="17030-123">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="17030-124">Skapar virtuella nätverksgränssnitt och bifogar dem toohello virtuellt frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="17030-124">Creates virtual network interfaces and attaches them toohello virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="17030-125">Skapa AZ nätverket nsg</span><span class="sxs-lookup"><span data-stu-id="17030-125">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="17030-126">Skapar nätverkssäkerhetsgrupper (NSG) som är associerade toohello frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="17030-126">Creates network security groups (NSG) that are associated toohello front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="17030-127">Skapa AZ nätverket nsg regel</span><span class="sxs-lookup"><span data-stu-id="17030-127">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) |<span data-ttu-id="17030-128">Skapar NSG-regler som tillåter eller blockerar specifika portar toospecific undernät.</span><span class="sxs-lookup"><span data-stu-id="17030-128">Creates NSG rules that allow or block specific ports toospecific subnets.</span></span> |
| [<span data-ttu-id="17030-129">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="17030-129">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="17030-130">Skapar virtuella datorer och bifogar ett NIC-tooeach VM.</span><span class="sxs-lookup"><span data-stu-id="17030-130">Creates virtual machines and attaches a NIC tooeach VM.</span></span> <span data-ttu-id="17030-131">Det här kommandot anger också hello virtuella avbildningen toouse och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="17030-131">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="17030-132">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="17030-132">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="17030-133">Tar bort en resursgrupp och alla resurser som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="17030-133">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="17030-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="17030-134">Next steps</span></span>

<span data-ttu-id="17030-135">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="17030-135">For more information on hello Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="17030-136">Ytterligare nätverk CLI skriptexempel finns i hello [dokumentation för Azure-nätverk – översikt](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="17030-136">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md)</span></span>
