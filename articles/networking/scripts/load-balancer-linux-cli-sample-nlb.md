---
title: "aaaAzure skriptexempel CLI - belastningen belastningsutjämna trafik tooVMs för hög tillgänglighet | Microsoft Docs"
description: "Skriptexempel Azure CLI - belastningen belastningsutjämna trafik tooVMs för hög tillgänglighet"
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 0954b5c261512724dfb9c6e7be123c9d45624f4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-traffic-toovms-for-high-availability"></a><span data-ttu-id="aa97f-103">Läsa in belastningsutjämna trafik tooVMs för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="aa97f-103">Load balance traffic tooVMs for high availability</span></span>

<span data-ttu-id="aa97f-104">Det här exemplet i skriptet skapar allt som behövs för toorun flera Ubuntu virtuella datorer som konfigurerats i en hög tillgänglighet och läsa in den belastningsutjämnade konfiguration.</span><span class="sxs-lookup"><span data-stu-id="aa97f-104">This script sample creates everything needed toorun several Ubuntu virtual machines configured in a highly available and load balanced configuration.</span></span> <span data-ttu-id="aa97f-105">När du köra hello-skriptet kommer du ha tre virtuella datorer kopplade tooan Azure Tillgänglighetsuppsättning och kan nås via en Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="aa97f-105">After running hello script, you will have three virtual machines, joined tooan Azure Availability Set, and accessible through an Azure Load Balancer.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="aa97f-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="aa97f-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="aa97f-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="aa97f-107">Clean up deployment</span></span> 

<span data-ttu-id="aa97f-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="aa97f-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="aa97f-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="aa97f-109">Script explanation</span></span>

<span data-ttu-id="aa97f-110">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="aa97f-110">This script uses hello following commands toocreate a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="aa97f-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="aa97f-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="aa97f-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="aa97f-112">Command</span></span> | <span data-ttu-id="aa97f-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="aa97f-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="aa97f-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="aa97f-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="aa97f-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="aa97f-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="aa97f-116">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="aa97f-116">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="aa97f-117">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="aa97f-117">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="aa97f-118">Skapa AZ nätverket offentliga-ip</span><span class="sxs-lookup"><span data-stu-id="aa97f-118">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="aa97f-119">Skapar en offentlig IP-adress med en statisk IP-adress och en tillhörande DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="aa97f-119">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="aa97f-120">Skapa AZ nätverket lb</span><span class="sxs-lookup"><span data-stu-id="aa97f-120">az network lb create</span></span>](https://docs.microsoft.com/cli/azure/network/lb#create) | <span data-ttu-id="aa97f-121">Skapar en Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="aa97f-121">Creates an Azure load balancer.</span></span> |
| [<span data-ttu-id="aa97f-122">Skapa AZ nätverket lb avsökning</span><span class="sxs-lookup"><span data-stu-id="aa97f-122">az network lb probe create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | <span data-ttu-id="aa97f-123">Skapar en belastningsutjämningsavsökning.</span><span class="sxs-lookup"><span data-stu-id="aa97f-123">Creates a load balancer probe.</span></span> <span data-ttu-id="aa97f-124">En belastningsutjämningsavsökning är används toomonitor varje virtuell dator i hello belastningen belastningsutjämnaren uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="aa97f-124">A load balancer probe is used toomonitor each VM in hello load balancer set.</span></span> <span data-ttu-id="aa97f-125">Om någon virtuell dator blir otillgänglig, är inte trafik dirigeras toohello VM.</span><span class="sxs-lookup"><span data-stu-id="aa97f-125">If any VM becomes inaccessible, traffic is not routed toohello VM.</span></span> |
| [<span data-ttu-id="aa97f-126">Skapa AZ nätverket lb regel</span><span class="sxs-lookup"><span data-stu-id="aa97f-126">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="aa97f-127">Skapar en regel för belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="aa97f-127">Creates a load balancer rule.</span></span> <span data-ttu-id="aa97f-128">I det här exemplet skapas en regel för port 80.</span><span class="sxs-lookup"><span data-stu-id="aa97f-128">In this sample, a rule is created for port 80.</span></span> <span data-ttu-id="aa97f-129">HTTP-trafik anländer till hello belastningsutjämnare, är det routade tooport 80 en hello virtuella datorer i hello LB-uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="aa97f-129">As HTTP traffic arrives at hello load balancer, it is routed tooport 80 one of hello VMs in hello LB set.</span></span> |
| [<span data-ttu-id="aa97f-130">Skapa AZ nätverket lb inkommande nat-regel</span><span class="sxs-lookup"><span data-stu-id="aa97f-130">az network lb inbound-nat-rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) | <span data-ttu-id="aa97f-131">Skapar en regel för belastningsutjämnare NAT (Network Address Translation).</span><span class="sxs-lookup"><span data-stu-id="aa97f-131">Creates load balancer Network Address Translation (NAT) rule.</span></span>  <span data-ttu-id="aa97f-132">NAT-regler mappa en port i hello belastningen belastningsutjämnaren tooa port på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="aa97f-132">NAT rules map a port of hello load balancer tooa port on a VM.</span></span> <span data-ttu-id="aa97f-133">I det här exemplet skapas en NAT-regel för SSH-trafik tooeach VM i hello belastningen belastningsutjämnaren uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="aa97f-133">In this sample, a NAT rule is created for SSH traffic tooeach VM in hello load balancer set.</span></span>  |
| [<span data-ttu-id="aa97f-134">Skapa AZ nätverket nsg</span><span class="sxs-lookup"><span data-stu-id="aa97f-134">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="aa97f-135">Skapar en nätverkssäkerhetsgrupp (NSG), vilket är en säkerhetsgräns mellan hello internet och hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="aa97f-135">Creates a network security group (NSG), which is a security boundary between hello internet and hello virtual machine.</span></span> |
| [<span data-ttu-id="aa97f-136">Skapa AZ nätverket nsg regel</span><span class="sxs-lookup"><span data-stu-id="aa97f-136">az network nsg rule create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="aa97f-137">Skapar en NSG regeln tooallow inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="aa97f-137">Creates an NSG rule tooallow inbound traffic.</span></span> <span data-ttu-id="aa97f-138">I det här exemplet används port 22 för SSH-trafik.</span><span class="sxs-lookup"><span data-stu-id="aa97f-138">In this sample, port 22 is opened for SSH traffic.</span></span> |
| [<span data-ttu-id="aa97f-139">Skapa AZ nätverket nic</span><span class="sxs-lookup"><span data-stu-id="aa97f-139">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="aa97f-140">Skapar ett virtuellt nätverkskort och bifogar den toohello virtuella nätverk och undernät NSG.</span><span class="sxs-lookup"><span data-stu-id="aa97f-140">Creates a virtual network card and attaches it toohello virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="aa97f-141">Skapa AZ vm tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="aa97f-141">az vm availability-set create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="aa97f-142">Skapar en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="aa97f-142">Creates an availability set.</span></span> <span data-ttu-id="aa97f-143">Tillgänglighetsuppsättningar garantera programmets drifttid genom att sprida hello virtuella datorer mellan fysiska resurser så att om fel uppstår, hello hela uppsättningen inte sker.</span><span class="sxs-lookup"><span data-stu-id="aa97f-143">Availability sets ensure application uptime by spreading hello virtual machines across physical resources such that if failure occurs, hello entire set is not effected.</span></span> |
| [<span data-ttu-id="aa97f-144">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="aa97f-144">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="aa97f-145">Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="aa97f-145">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="aa97f-146">Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="aa97f-146">This command also specifies hello virtual machine image toobe used and administrative credentials.</span></span>  |
| [<span data-ttu-id="aa97f-147">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="aa97f-147">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="aa97f-148">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="aa97f-148">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="aa97f-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aa97f-149">Next steps</span></span>

<span data-ttu-id="aa97f-150">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="aa97f-150">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="aa97f-151">Ytterligare Azure-nätverk CLI skriptexempel finns i hello [Azure nätverk dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="aa97f-151">Additional Azure Networking CLI script samples can be found in hello [Azure Networking documentation](../cli-samples.md).</span></span>
