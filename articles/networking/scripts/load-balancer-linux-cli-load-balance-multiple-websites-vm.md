---
title: "aaaAzure CLI skriptexempel - belastningsutjämning för flera webbplatser med hello Azure CLI | Microsoft Docs"
description: "Azure CLI skriptexempel - belastningsutjämning för flera webbplatser toohello samma virtuella dator"
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
ms.openlocfilehash: 136da5d1783fb9f9dc87f1ffad8eec7b95c6bd7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-multiple-websites"></a><span data-ttu-id="b6e97-103">Belastningsutjämning för flera webbplatser</span><span class="sxs-lookup"><span data-stu-id="b6e97-103">Load balance multiple websites</span></span>

<span data-ttu-id="b6e97-104">Det här exemplet i skriptet skapar ett virtuellt nätverk med två virtuella datorer (VM) som är medlemmar i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="b6e97-104">This script sample creates a virtual network with two virtual machines (VM) that are members of an availability set.</span></span> <span data-ttu-id="b6e97-105">En belastningsutjämnare dirigerar trafik för två separata IP-adresser toohello två virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b6e97-105">A load balancer directs traffic for two separate IP addresses toohello two VMs.</span></span> <span data-ttu-id="b6e97-106">Efter köra hello skriptet kan du distribuera web server programvara toohello virtuella datorer och vara värd för flera webbplatser, var och en med sin egen IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b6e97-106">After running hello script, you could deploy web server software toohello VMs and host multiple web sites, each with its own IP address.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="b6e97-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="b6e97-107">Sample script</span></span>


[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a><span data-ttu-id="b6e97-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="b6e97-108">Clean up deployment</span></span> 

<span data-ttu-id="b6e97-109">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="b6e97-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="b6e97-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="b6e97-110">Script explanation</span></span>

<span data-ttu-id="b6e97-111">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella nätverk, läsa in belastningsutjämning och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="b6e97-111">This script uses hello following commands toocreate a resource group, virtual network, load balancer, and all related resources.</span></span> <span data-ttu-id="b6e97-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="b6e97-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="b6e97-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="b6e97-113">Command</span></span> | <span data-ttu-id="b6e97-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="b6e97-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b6e97-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="b6e97-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="b6e97-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="b6e97-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b6e97-117">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="b6e97-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="b6e97-118">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="b6e97-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="b6e97-119">Skapa AZ nätverket offentliga-ip</span><span class="sxs-lookup"><span data-stu-id="b6e97-119">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="b6e97-120">Skapar en offentlig IP-adress med en statisk IP-adress och en tillhörande DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="b6e97-120">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="b6e97-121">Skapa AZ nätverket lb</span><span class="sxs-lookup"><span data-stu-id="b6e97-121">az network lb create</span></span>](https://docs.microsoft.com/cli/azure/network/lb#create) | <span data-ttu-id="b6e97-122">Skapar en Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="b6e97-122">Creates an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="b6e97-123">Skapa AZ nätverket lb avsökning</span><span class="sxs-lookup"><span data-stu-id="b6e97-123">az network lb probe create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | <span data-ttu-id="b6e97-124">Skapar en belastningsutjämningsavsökning.</span><span class="sxs-lookup"><span data-stu-id="b6e97-124">Creates a load balancer probe.</span></span> <span data-ttu-id="b6e97-125">En belastningsutjämningsavsökning är används toomonitor varje virtuell dator i hello belastningen belastningsutjämnaren uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="b6e97-125">A load balancer probe is used toomonitor each VM in hello load balancer set.</span></span> <span data-ttu-id="b6e97-126">Om någon virtuell dator blir otillgänglig, är inte trafik dirigeras toohello VM.</span><span class="sxs-lookup"><span data-stu-id="b6e97-126">If any VM becomes inaccessible, traffic is not routed toohello VM.</span></span> |
| [<span data-ttu-id="b6e97-127">Skapa AZ nätverket lb regel</span><span class="sxs-lookup"><span data-stu-id="b6e97-127">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="b6e97-128">Skapar en regel för belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="b6e97-128">Creates a load balancer rule.</span></span> <span data-ttu-id="b6e97-129">I det här exemplet skapas en regel för port 80.</span><span class="sxs-lookup"><span data-stu-id="b6e97-129">In this sample, a rule is created for port 80.</span></span> <span data-ttu-id="b6e97-130">HTTP-trafik anländer till hello belastningsutjämnare, är det routade tooport 80 en hello virtuella datorer i hello belastningen belastningsutjämnaren uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="b6e97-130">As HTTP traffic arrives at hello load balancer, it is routed tooport 80 one of hello VMs in hello load balancer set.</span></span> |
| [<span data-ttu-id="b6e97-131">Skapa AZ nätverket lb klientdels-ip</span><span class="sxs-lookup"><span data-stu-id="b6e97-131">az network lb frontend-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) | <span data-ttu-id="b6e97-132">Skapa en IP-adress för klientdel för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="b6e97-132">Create a frontend IP address for hello Load Balancer.</span></span> |
| [<span data-ttu-id="b6e97-133">Skapa AZ lb-nätverksadresspool</span><span class="sxs-lookup"><span data-stu-id="b6e97-133">az network lb address-pool create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) | <span data-ttu-id="b6e97-134">Skapar en backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="b6e97-134">Creates a backend address pool.</span></span> |
| [<span data-ttu-id="b6e97-135">Skapa AZ nätverket nic</span><span class="sxs-lookup"><span data-stu-id="b6e97-135">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="b6e97-136">Skapar ett virtuellt nätverkskort och bifogar den toohello virtuellt nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="b6e97-136">Creates a virtual network card and attaches it toohello virtual network, and subnet.</span></span> |
| [<span data-ttu-id="b6e97-137">Skapa AZ vm tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="b6e97-137">az vm availability-set create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="b6e97-138">Skapar en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="b6e97-138">Creates an availability set.</span></span> <span data-ttu-id="b6e97-139">Tillgänglighetsuppsättningar garantera programmets drifttid genom att sprida hello virtuella datorer mellan fysiska resurser så att om fel uppstår, hello hela uppsättningen inte sker.</span><span class="sxs-lookup"><span data-stu-id="b6e97-139">Availability sets ensure application uptime by spreading hello virtual machines across physical resources such that if failure occurs, hello entire set is not effected.</span></span> |
| [<span data-ttu-id="b6e97-140">Skapa AZ nätverket nic ip-konfiguration</span><span class="sxs-lookup"><span data-stu-id="b6e97-140">az network nic ip-config create</span></span>](https://docs.microsoft.com/cli/azure/network/nic/ip-config#create) | <span data-ttu-id="b6e97-141">Skapar en IP-confiuration.</span><span class="sxs-lookup"><span data-stu-id="b6e97-141">Creates an IP confiuration.</span></span> <span data-ttu-id="b6e97-142">Du måste ha hello Microsoft.Network/AllowMultipleIpConfigurationsPerNic funktionen är aktiverad för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b6e97-142">You must have hello Microsoft.Network/AllowMultipleIpConfigurationsPerNic feature enabled for your subscription.</span></span> <span data-ttu-id="b6e97-143">Bara en konfiguration kan utses som hello primära IP-konfiguration per NIC, med hjälp av hello--gör primär flaggan.</span><span class="sxs-lookup"><span data-stu-id="b6e97-143">Only one configuration may be designated as hello primary IP configuration per NIC, using hello --make-primary flag.</span></span> |
| [<span data-ttu-id="b6e97-144">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="b6e97-144">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="b6e97-145">Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="b6e97-145">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="b6e97-146">Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b6e97-146">This command also specifies hello virtual machine image toobe used and administrative credentials.</span></span>  |
| [<span data-ttu-id="b6e97-147">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="b6e97-147">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="b6e97-148">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="b6e97-148">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b6e97-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b6e97-149">Next steps</span></span>

<span data-ttu-id="b6e97-150">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b6e97-150">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="b6e97-151">Ytterligare nätverk CLI skriptexempel finns i hello [dokumentation för Azure-nätverk – översikt](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b6e97-151">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
