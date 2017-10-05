---
title: Azure CLI Script exempel - skapa en virtuell Linux-dator med NLB | Microsoft Docs
description: "Azure CLI Script exempel - skapa en virtuell Linux-dator med Utjämning av nätverksbelastning"
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
ms.openlocfilehash: a0052605da9f0023d11cc9253d8aecfb1d452e1a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-highly-available-vm"></a><span data-ttu-id="89e15-103">Skapa en högtillgänglig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="89e15-103">Create a highly available VM</span></span>

<span data-ttu-id="89e15-104">Det här exemplet i skriptet skapar allt som behövs för att köra flera Ubuntu virtuella datorer som konfigurerats i en hög tillgänglighet och läsa in belastningsutjämnade konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="89e15-104">This script sample creates everything needed to run several Ubuntu virtual machines configured in a highly available and load balanced configuration.</span></span> <span data-ttu-id="89e15-105">När du har kört skriptet har tre virtuella datorer, ansluten till en Azure Tillgänglighetsuppsättning, och kan öppnas via en Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="89e15-105">After running the script, you will have three virtual machines, joined to an Azure Availability Set, and accessible through an Azure Load Balancer.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="89e15-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="89e15-106">Sample script</span></span>

<span data-ttu-id="89e15-107">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "snabbt skapa virtuell dator")]</span><span class="sxs-lookup"><span data-stu-id="89e15-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="89e15-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="89e15-108">Clean up deployment</span></span> 

<span data-ttu-id="89e15-109">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="89e15-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="89e15-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="89e15-110">Script explanation</span></span>

<span data-ttu-id="89e15-111">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="89e15-111">This script uses the following commands to create a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="89e15-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="89e15-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="89e15-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="89e15-113">Command</span></span> | <span data-ttu-id="89e15-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="89e15-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="89e15-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="89e15-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="89e15-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="89e15-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="89e15-117">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="89e15-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="89e15-118">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="89e15-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="89e15-119">Skapa AZ nätverket offentliga-ip</span><span class="sxs-lookup"><span data-stu-id="89e15-119">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="89e15-120">Skapar en offentlig IP-adress med en statisk IP-adress och en tillhörande DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="89e15-120">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="89e15-121">Skapa AZ nätverket lb</span><span class="sxs-lookup"><span data-stu-id="89e15-121">az network lb create</span></span>](https://docs.microsoft.com/cli/azure/network/lb#create) | <span data-ttu-id="89e15-122">Skapar ett Azure Utjämning av nätverksbelastning (NLB).</span><span class="sxs-lookup"><span data-stu-id="89e15-122">Creates an Azure Network Load Balancer (NLB).</span></span> |
| [<span data-ttu-id="89e15-123">Skapa AZ nätverket lb avsökning</span><span class="sxs-lookup"><span data-stu-id="89e15-123">az network lb probe create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | <span data-ttu-id="89e15-124">Skapar ett NLB-avsökning.</span><span class="sxs-lookup"><span data-stu-id="89e15-124">Creates an NLB probe.</span></span> <span data-ttu-id="89e15-125">Ett NLB-avsökningen används för att övervaka varje virtuell dator i NLB-uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="89e15-125">An NLB probe is used to monitor each VM in the NLB set.</span></span> <span data-ttu-id="89e15-126">Om någon virtuell dator blir otillgänglig, dirigeras trafik inte till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="89e15-126">If any VM becomes inaccessible, traffic is not routed to the VM.</span></span> |
| [<span data-ttu-id="89e15-127">Skapa AZ nätverket lb regel</span><span class="sxs-lookup"><span data-stu-id="89e15-127">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="89e15-128">Skapar en regel för Utjämning av nätverksbelastning.</span><span class="sxs-lookup"><span data-stu-id="89e15-128">Creates an NLB rule.</span></span> <span data-ttu-id="89e15-129">I det här exemplet skapas en regel för port 80.</span><span class="sxs-lookup"><span data-stu-id="89e15-129">In this sample, a rule is created for port 80.</span></span> <span data-ttu-id="89e15-130">När HTTP-trafik anländer på Utjämning av nätverksbelastning, dirigeras till port 80 något av de virtuella datorerna i NLB-uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="89e15-130">As HTTP traffic arrives at the NLB, it is routed to port 80 one of the VMs in the NLB set.</span></span> |
| [<span data-ttu-id="89e15-131">Skapa AZ nätverket lb inkommande nat-regel</span><span class="sxs-lookup"><span data-stu-id="89e15-131">az network lb inbound-nat-rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) | <span data-ttu-id="89e15-132">Skapar en regel för NLB (Network Address Translation Translation).</span><span class="sxs-lookup"><span data-stu-id="89e15-132">Creates an NLB Network Address Translation (NAT) rule.</span></span>  <span data-ttu-id="89e15-133">NAT-regler kan du mappa en port i NLB-klustret till en port på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="89e15-133">NAT rules map a port of the NLB to a port on a VM.</span></span> <span data-ttu-id="89e15-134">I det här exemplet skapas en NAT-regel för SSH-trafik på varje virtuell dator i NLB-uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="89e15-134">In this sample, a NAT rule is created for SSH traffic to each VM in the NLB set.</span></span>  |
| [<span data-ttu-id="89e15-135">Skapa AZ nätverket nsg</span><span class="sxs-lookup"><span data-stu-id="89e15-135">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="89e15-136">Skapar en nätverkssäkerhetsgrupp (NSG), vilket är en säkerhetsgräns mellan internet och den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="89e15-136">Creates a network security group (NSG), which is a security boundary between the internet and the virtual machine.</span></span> |
| [<span data-ttu-id="89e15-137">Skapa AZ nätverket nsg regel</span><span class="sxs-lookup"><span data-stu-id="89e15-137">az network nsg rule create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="89e15-138">Skapar en NSG-regel för att tillåta inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="89e15-138">Creates an NSG rule to allow inbound traffic.</span></span> <span data-ttu-id="89e15-139">I det här exemplet används port 22 för SSH-trafik.</span><span class="sxs-lookup"><span data-stu-id="89e15-139">In this sample, port 22 is opened for SSH traffic.</span></span> |
| [<span data-ttu-id="89e15-140">Skapa AZ nätverket nic</span><span class="sxs-lookup"><span data-stu-id="89e15-140">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="89e15-141">Skapar ett virtuellt nätverkskort som kopplas till den virtuella nätverk och undernät NSG.</span><span class="sxs-lookup"><span data-stu-id="89e15-141">Creates a virtual network card and attaches it to the virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="89e15-142">Skapa AZ vm tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="89e15-142">az vm availability-set create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="89e15-143">Skapar en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="89e15-143">Creates an availability set.</span></span> <span data-ttu-id="89e15-144">Tillgänglighetsuppsättningar garantera programmets drifttid genom att sprida virtuella datorer mellan fysiska resurser så att hela uppsättningen inte sker om fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="89e15-144">Availability sets ensure application uptime by spreading the virtual machines across physical resources such that if failure occurs, the entire set is not effected.</span></span> |
| [<span data-ttu-id="89e15-145">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="89e15-145">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="89e15-146">Skapar den virtuella datorn och ansluter till nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="89e15-146">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="89e15-147">Det här kommandot anger också avbildningen av virtuella datorn ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="89e15-147">This command also specifies the virtual machine image to be used and administrative credentials.</span></span>  |
| [<span data-ttu-id="89e15-148">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="89e15-148">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="89e15-149">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="89e15-149">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="89e15-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89e15-150">Next steps</span></span>

<span data-ttu-id="89e15-151">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="89e15-151">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="89e15-152">Ytterligare virtuella CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="89e15-152">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
