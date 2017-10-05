---
title: "Azure CLI-skript exempel – skapa ett nätverk för flera nivåer program | Microsoft Docs"
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
ms.openlocfilehash: de65d820f2d9eea49b58185c81d815675fd76740
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a><span data-ttu-id="97b36-103">Skapa ett nätverk för flera nivåer program</span><span class="sxs-lookup"><span data-stu-id="97b36-103">Create a network for multi-tier applications</span></span>

<span data-ttu-id="97b36-104">Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="97b36-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="97b36-105">Trafik till undernätet för frontend är begränsad till http- och SSH, medan trafik till backend-undernät är begränsad till MySQL port 3306.</span><span class="sxs-lookup"><span data-stu-id="97b36-105">Traffic to the front-end subnet is limited to HTTP and SSH, while traffic to the back-end subnet is limited to MySQL, port 3306.</span></span> <span data-ttu-id="97b36-106">När du har kört skriptet har två virtuella datorer i varje undernät som du kan distribuera webbservern och MySQL programvaran till.</span><span class="sxs-lookup"><span data-stu-id="97b36-106">After running the script, you will have two virtual machines, one in each subnet that you can deploy web server and MySQL software to.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="97b36-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="97b36-107">Sample script</span></span>


<span data-ttu-id="97b36-108">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "för flera nivåer program för virtuella nätverk")]</span><span class="sxs-lookup"><span data-stu-id="97b36-108">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="97b36-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="97b36-109">Clean up deployment</span></span> 

<span data-ttu-id="97b36-110">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="97b36-110">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="97b36-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="97b36-111">Script explanation</span></span>

<span data-ttu-id="97b36-112">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuella nätverk och nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="97b36-112">This script uses the following commands to create a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="97b36-113">Varje kommando i tabellen länkar till kommando-specifik dokumentation.</span><span class="sxs-lookup"><span data-stu-id="97b36-113">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="97b36-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="97b36-114">Command</span></span> | <span data-ttu-id="97b36-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="97b36-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="97b36-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="97b36-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="97b36-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="97b36-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="97b36-118">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="97b36-118">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="97b36-119">Skapar en Azure-nätverket och frontend-undernät.</span><span class="sxs-lookup"><span data-stu-id="97b36-119">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="97b36-120">Skapa AZ undernät</span><span class="sxs-lookup"><span data-stu-id="97b36-120">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="97b36-121">Skapar en backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="97b36-121">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="97b36-122">Skapa AZ nätverket offentliga-ip</span><span class="sxs-lookup"><span data-stu-id="97b36-122">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="97b36-123">Skapar en offentlig IP-adress för att komma åt den virtuella datorn från Internet.</span><span class="sxs-lookup"><span data-stu-id="97b36-123">Creates a public IP address to access the VM from the Internet.</span></span> |
| [<span data-ttu-id="97b36-124">Skapa AZ nätverket nic</span><span class="sxs-lookup"><span data-stu-id="97b36-124">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="97b36-125">Skapar virtuella nätverksgränssnitt och kopplar dem till det virtuella nätverket frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="97b36-125">Creates virtual network interfaces and attaches them to the virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="97b36-126">Skapa AZ nätverket nsg</span><span class="sxs-lookup"><span data-stu-id="97b36-126">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="97b36-127">Skapar säkerhetsgrupper (NSG) som är kopplade till frontend och backend-undernät för nätverket.</span><span class="sxs-lookup"><span data-stu-id="97b36-127">Creates network security groups (NSG) that are associated to the front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="97b36-128">Skapa AZ nätverket nsg regel</span><span class="sxs-lookup"><span data-stu-id="97b36-128">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) |<span data-ttu-id="97b36-129">Skapar NSG-regler som tillåter eller blockerar specifika portar till specifika undernät.</span><span class="sxs-lookup"><span data-stu-id="97b36-129">Creates NSG rules that allow or block specific ports to specific subnets.</span></span> |
| [<span data-ttu-id="97b36-130">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="97b36-130">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="97b36-131">Skapar virtuella datorer och bifogar ett nätverkskort på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="97b36-131">Creates virtual machines and attaches a NIC to each VM.</span></span> <span data-ttu-id="97b36-132">Det här kommandot anger också avbildning av virtuell dator ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="97b36-132">This command also specifies the virtual machine image to use and administrative credentials.</span></span> |
| [<span data-ttu-id="97b36-133">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="97b36-133">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="97b36-134">Tar bort en resursgrupp och alla resurser som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="97b36-134">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="97b36-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="97b36-135">Next steps</span></span>

<span data-ttu-id="97b36-136">Mer information om Azure CLI finns [Azure CLI dokumentationen](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="97b36-136">For more information on the Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="97b36-137">Ytterligare nätverk CLI skriptexempel finns i den [dokumentation för Azure-nätverk – översikt](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="97b36-137">Additional networking CLI script samples can be found in the [Azure Networking Overview documentation](../cli-samples.md)</span></span>