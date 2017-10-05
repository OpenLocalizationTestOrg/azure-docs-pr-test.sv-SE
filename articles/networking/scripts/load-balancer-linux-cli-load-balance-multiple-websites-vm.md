---
title: "Azure CLI skriptexempel - belastningsutjämning för flera webbplatser med Azure CLI | Microsoft Docs"
description: "Azure CLI skriptexempel - belastningsutjämning för flera webbplatser till samma virtuella dator"
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
ms.openlocfilehash: c5a584b33025122033b930822ae0a0864a7ec1cb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="load-balance-multiple-websites"></a><span data-ttu-id="2af9a-103">Belastningsutjämning för flera webbplatser</span><span class="sxs-lookup"><span data-stu-id="2af9a-103">Load balance multiple websites</span></span>

<span data-ttu-id="2af9a-104">Det här exemplet i skriptet skapar ett virtuellt nätverk med två virtuella datorer (VM) som är medlemmar i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2af9a-104">This script sample creates a virtual network with two virtual machines (VM) that are members of an availability set.</span></span> <span data-ttu-id="2af9a-105">En belastningsutjämnare dirigerar trafik för två olika IP-adresser till de två virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="2af9a-105">A load balancer directs traffic for two separate IP addresses to the two VMs.</span></span> <span data-ttu-id="2af9a-106">När skriptet har körts kan du distribuera programvara för webbserver för virtuella datorer och värden flera webbplatser, var och en med sin egen IP-adress.</span><span class="sxs-lookup"><span data-stu-id="2af9a-106">After running the script, you could deploy web server software to the VMs and host multiple web sites, each with its own IP address.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="2af9a-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="2af9a-107">Sample script</span></span>


<span data-ttu-id="2af9a-108">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "belastningsutjämning för flera webbplatser")]</span><span class="sxs-lookup"><span data-stu-id="2af9a-108">[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="2af9a-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="2af9a-109">Clean up deployment</span></span> 

<span data-ttu-id="2af9a-110">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="2af9a-110">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="2af9a-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="2af9a-111">Script explanation</span></span>

<span data-ttu-id="2af9a-112">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuella nätverk, belastningsutjämnare och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="2af9a-112">This script uses the following commands to create a resource group, virtual network, load balancer, and all related resources.</span></span> <span data-ttu-id="2af9a-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="2af9a-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="2af9a-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="2af9a-114">Command</span></span> | <span data-ttu-id="2af9a-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="2af9a-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2af9a-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="2af9a-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="2af9a-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="2af9a-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2af9a-118">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="2af9a-118">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="2af9a-119">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="2af9a-119">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="2af9a-120">Skapa AZ nätverket offentliga-ip</span><span class="sxs-lookup"><span data-stu-id="2af9a-120">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="2af9a-121">Skapar en offentlig IP-adress med en statisk IP-adress och en tillhörande DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="2af9a-121">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="2af9a-122">Skapa AZ nätverket lb</span><span class="sxs-lookup"><span data-stu-id="2af9a-122">az network lb create</span></span>](https://docs.microsoft.com/cli/azure/network/lb#create) | <span data-ttu-id="2af9a-123">Skapar en Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="2af9a-123">Creates an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="2af9a-124">Skapa AZ nätverket lb avsökning</span><span class="sxs-lookup"><span data-stu-id="2af9a-124">az network lb probe create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | <span data-ttu-id="2af9a-125">Skapar en belastningsutjämningsavsökning.</span><span class="sxs-lookup"><span data-stu-id="2af9a-125">Creates a load balancer probe.</span></span> <span data-ttu-id="2af9a-126">En belastningsutjämningsavsökning används för att övervaka varje virtuell dator i uppsättningen av belastningen belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="2af9a-126">A load balancer probe is used to monitor each VM in the load balancer set.</span></span> <span data-ttu-id="2af9a-127">Om någon virtuell dator blir otillgänglig, dirigeras trafik inte till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2af9a-127">If any VM becomes inaccessible, traffic is not routed to the VM.</span></span> |
| [<span data-ttu-id="2af9a-128">Skapa AZ nätverket lb regel</span><span class="sxs-lookup"><span data-stu-id="2af9a-128">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="2af9a-129">Skapar en regel för belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="2af9a-129">Creates a load balancer rule.</span></span> <span data-ttu-id="2af9a-130">I det här exemplet skapas en regel för port 80.</span><span class="sxs-lookup"><span data-stu-id="2af9a-130">In this sample, a rule is created for port 80.</span></span> <span data-ttu-id="2af9a-131">När HTTP-trafik anländer på belastningsutjämnaren, dirigeras till port 80 något av de virtuella datorerna i uppsättningen av belastningen belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="2af9a-131">As HTTP traffic arrives at the load balancer, it is routed to port 80 one of the VMs in the load balancer set.</span></span> |
| [<span data-ttu-id="2af9a-132">Skapa AZ nätverket lb klientdels-ip</span><span class="sxs-lookup"><span data-stu-id="2af9a-132">az network lb frontend-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) | <span data-ttu-id="2af9a-133">Skapa en klientdelens IP-adress för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="2af9a-133">Create a frontend IP address for the Load Balancer.</span></span> |
| [<span data-ttu-id="2af9a-134">Skapa AZ lb-nätverksadresspool</span><span class="sxs-lookup"><span data-stu-id="2af9a-134">az network lb address-pool create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) | <span data-ttu-id="2af9a-135">Skapar en backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="2af9a-135">Creates a backend address pool.</span></span> |
| [<span data-ttu-id="2af9a-136">Skapa AZ nätverket nic</span><span class="sxs-lookup"><span data-stu-id="2af9a-136">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="2af9a-137">Skapar ett virtuellt nätverkskort och kopplar den till virtuellt nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="2af9a-137">Creates a virtual network card and attaches it to the virtual network, and subnet.</span></span> |
| [<span data-ttu-id="2af9a-138">Skapa AZ vm tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="2af9a-138">az vm availability-set create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="2af9a-139">Skapar en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2af9a-139">Creates an availability set.</span></span> <span data-ttu-id="2af9a-140">Tillgänglighetsuppsättningar garantera programmets drifttid genom att sprida virtuella datorer mellan fysiska resurser så att hela uppsättningen inte sker om fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="2af9a-140">Availability sets ensure application uptime by spreading the virtual machines across physical resources such that if failure occurs, the entire set is not effected.</span></span> |
| [<span data-ttu-id="2af9a-141">Skapa AZ nätverket nic ip-konfiguration</span><span class="sxs-lookup"><span data-stu-id="2af9a-141">az network nic ip-config create</span></span>](https://docs.microsoft.com/cli/azure/network/nic/ip-config#create) | <span data-ttu-id="2af9a-142">Skapar en IP-confiuration.</span><span class="sxs-lookup"><span data-stu-id="2af9a-142">Creates an IP confiuration.</span></span> <span data-ttu-id="2af9a-143">Du måste ha funktionen Microsoft.Network/AllowMultipleIpConfigurationsPerNic aktiverad för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2af9a-143">You must have the Microsoft.Network/AllowMultipleIpConfigurationsPerNic feature enabled for your subscription.</span></span> <span data-ttu-id="2af9a-144">Bara en konfiguration kan utses som den primära IP-konfigurationen per NIC, med hjälp av flaggan--gör primär.</span><span class="sxs-lookup"><span data-stu-id="2af9a-144">Only one configuration may be designated as the primary IP configuration per NIC, using the --make-primary flag.</span></span> |
| [<span data-ttu-id="2af9a-145">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="2af9a-145">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="2af9a-146">Skapar den virtuella datorn och ansluter till nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="2af9a-146">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="2af9a-147">Det här kommandot anger också avbildningen av virtuella datorn ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2af9a-147">This command also specifies the virtual machine image to be used and administrative credentials.</span></span>  |
| [<span data-ttu-id="2af9a-148">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="2af9a-148">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="2af9a-149">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="2af9a-149">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2af9a-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2af9a-150">Next steps</span></span>

<span data-ttu-id="2af9a-151">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2af9a-151">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="2af9a-152">Ytterligare nätverk CLI skriptexempel finns i den [dokumentation för Azure-nätverk – översikt](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2af9a-152">Additional networking CLI script samples can be found in the [Azure Networking Overview documentation](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
