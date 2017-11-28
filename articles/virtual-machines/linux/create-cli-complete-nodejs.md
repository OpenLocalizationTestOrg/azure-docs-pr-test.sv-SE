---
title: "aaaCreate en fullständig Linux-miljö med hello Azure CLI 1.0 | Microsoft Docs"
description: "Skapa lagring, en Linux VM, ett virtuellt nätverk och undernät, belastningsutjämning, ett nätverkskort, en offentlig IP-adress och en nätverkssäkerhetsgrupp allt från hello bakgrund med hjälp av hello Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 7fe00e138704fe9c9a1c9b87a7dd1afd6174e527
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-complete-linux-environment-with-hello-azure-cli-10"></a><span data-ttu-id="ef3b9-103">Skapa en fullständig Linux-miljö med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="ef3b9-103">Create a complete Linux environment with hello Azure CLI 1.0</span></span>
<span data-ttu-id="ef3b9-104">I den här artikeln skapar vi ett enkelt nätverk med en belastningsutjämnare och ett par med virtuella datorer som är användbara för utveckling och enkel datoranvändning.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-104">In this article, we build a simple network with a load balancer and a pair of VMs that are useful for development and simple computing.</span></span> <span data-ttu-id="ef3b9-105">Vi går igenom processen hello kommandot och kommandot, tills du har två fungerande säkra virtuella Linux-datorer toowhich du ansluter från var som helst på hello Internet.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-105">We walk through hello process command by command, until you have two working, secure Linux VMs toowhich you can connect from anywhere on hello Internet.</span></span> <span data-ttu-id="ef3b9-106">Du kan flytta på toomore komplexa nätverk och miljöer.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-106">Then you can move on toomore complex networks and environments.</span></span>

<span data-ttu-id="ef3b9-107">Hello vägen du lär dig mer om hello beroende hierarkin att hello Resource Manager-distributionsmodellen ger dig och om hur mycket energi ger.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-107">Along hello way, you learn about hello dependency hierarchy that hello Resource Manager deployment model gives you, and about how much power it provides.</span></span> <span data-ttu-id="ef3b9-108">När du ser hur hello systemet byggs, kan du återskapa den mycket snabbare med hjälp av [Azure Resource Manager-mallar](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-108">After you see how hello system is built, you can rebuild it much more quickly by using [Azure Resource Manager templates](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="ef3b9-109">Även efter att du lär dig hur hello delar av din miljö fungerar ihop, hur du skapar mallar tooautomate dem blir enklare.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-109">Also, after you learn how hello parts of your environment fit together, creating templates tooautomate them becomes easier.</span></span>

<span data-ttu-id="ef3b9-110">hello miljö innehåller:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-110">hello environment contains:</span></span>

* <span data-ttu-id="ef3b9-111">Två virtuella datorer i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-111">Two VMs inside an availability set.</span></span>
* <span data-ttu-id="ef3b9-112">En belastningsutjämnare med en regel för belastningsutjämning på port 80.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-112">A load balancer with a load-balancing rule on port 80.</span></span>
* <span data-ttu-id="ef3b9-113">Nätverkssäkerhetsgrupp (NSG) regler tooprotect den virtuella datorn från oönskad trafik.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-113">Network security group (NSG) rules tooprotect your VM from unwanted traffic.</span></span>

<span data-ttu-id="ef3b9-114">toocreate den här anpassade miljön, behöver du hello senaste [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) i Resource Manager-läget (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-114">toocreate this custom environment, you need hello latest [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) in Resource Manager mode (`azure config mode arm`).</span></span> <span data-ttu-id="ef3b9-115">Du måste också en JSON-parsning verktyget.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-115">You also need a JSON parsing tool.</span></span> <span data-ttu-id="ef3b9-116">Det här exemplet används [jq](https://stedolan.github.io/jq/).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-116">This example uses [jq](https://stedolan.github.io/jq/).</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="ef3b9-117">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="ef3b9-117">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="ef3b9-118">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-118">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="ef3b9-119">[Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="ef3b9-119">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="ef3b9-120">[Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="ef3b9-120">[Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="ef3b9-121">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="ef3b9-121">Quick commands</span></span>
<span data-ttu-id="ef3b9-122">Om du behöver tooquickly utföra hello uppgiften, hello följande avsnitt information hello bas kommandon tooupload tooAzure en VM.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-122">If you need tooquickly accomplish hello task, hello following section details hello base commands tooupload a VM tooAzure.</span></span> <span data-ttu-id="ef3b9-123">Mer detaljerad information och kontext för varje steg i hello resten av hello dokumentet med början [här](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-123">More detailed information and context for each step can be found in hello rest of hello document, starting [here](#detailed-walkthrough).</span></span>

<span data-ttu-id="ef3b9-124">Se till att du har [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) loggas och med hjälp av Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-124">Make sure that you have [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="ef3b9-125">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-125">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="ef3b9-126">Exempel parameternamn inkluderar `myResourceGroup`, `mystorageaccount`, och `myVM`.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-126">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>

<span data-ttu-id="ef3b9-127">Skapa hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-127">Create hello resource group.</span></span> <span data-ttu-id="ef3b9-128">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westeurope` plats:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-128">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` location:</span></span>

```azurecli
azure group create -n myResourceGroup -l westeurope
```

<span data-ttu-id="ef3b9-129">Kontrollera hello resursgrupp med hjälp av hello JSON-parsern:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-129">Verify hello resource group by using hello JSON parser:</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="ef3b9-130">Skapa hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-130">Create hello storage account.</span></span> <span data-ttu-id="ef3b9-131">hello följande exempel skapas ett lagringskonto med namnet `mystorageaccount`.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-131">hello following example creates a storage account named `mystorageaccount`.</span></span> <span data-ttu-id="ef3b9-132">(hello lagringskontonamn måste vara unikt, så ange dina egna unika namn)</span><span class="sxs-lookup"><span data-stu-id="ef3b9-132">(hello storage account name must be unique, so provide your own unique name.)</span></span>

```azurecli
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

<span data-ttu-id="ef3b9-133">Kontrollera hello storage-konto med hjälp av hello JSON-parsern:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-133">Verify hello storage account by using hello JSON parser:</span></span>

```azurecli
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

<span data-ttu-id="ef3b9-134">Skapa hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-134">Create hello virtual network.</span></span> <span data-ttu-id="ef3b9-135">hello följande exempel skapas ett virtuellt nätverk med namnet `myVnet`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-135">hello following example creates a virtual network named `myVnet`:</span></span>

```azurecli
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

<span data-ttu-id="ef3b9-136">Skapa ett undernät.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-136">Create a subnet.</span></span> <span data-ttu-id="ef3b9-137">hello följande exempel skapas ett undernät med namnet `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-137">hello following example creates a subnet named `mySubnet`:</span></span>

```azurecli
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

<span data-ttu-id="ef3b9-138">Verifiera hello virtuellt nätverk och undernät med hello JSON-parsern:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-138">Verify hello virtual network and subnet by using hello JSON parser:</span></span>

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

<span data-ttu-id="ef3b9-139">Skapa en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-139">Create a public IP.</span></span> <span data-ttu-id="ef3b9-140">hello följande exempel skapas en offentlig IP-adress med namnet `myPublicIP` med hello DNS-namnet på `mypublicdns`.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-140">hello following example creates a public IP named `myPublicIP` with hello DNS name of `mypublicdns`.</span></span> <span data-ttu-id="ef3b9-141">(hello DNS-namn måste vara unikt, så ange dina egna unika namn)</span><span class="sxs-lookup"><span data-stu-id="ef3b9-141">(hello DNS name must be unique, so provide your own unique name.)</span></span>

```azurecli
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

<span data-ttu-id="ef3b9-142">Skapa hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-142">Create hello load balancer.</span></span> <span data-ttu-id="ef3b9-143">hello följande exempel skapas en belastningsutjämnare med namnet `myLoadBalancer`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-143">hello following example creates a load balancer named `myLoadBalancer`:</span></span>

```azurecli
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

<span data-ttu-id="ef3b9-144">Skapa en frontend IP-adresspool för hello belastningsutjämnare och associera hello offentlig IP.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-144">Create a front-end IP pool for hello load balancer, and associate hello public IP.</span></span> <span data-ttu-id="ef3b9-145">hello följande exempel skapas en frontend IP-pool med namnet `mySubnetPool`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-145">hello following example creates a front-end IP pool named `mySubnetPool`:</span></span>

```azurecli
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool
```

<span data-ttu-id="ef3b9-146">Skapa hello backend-IP-adresspool för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-146">Create hello back-end IP pool for hello load balancer.</span></span> <span data-ttu-id="ef3b9-147">hello följande exempel skapas en backend-IP-pool med namnet `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-147">hello following example creates a back-end IP pool named `myBackEndPool`:</span></span>

```azurecli
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

<span data-ttu-id="ef3b9-148">Skapa SSH inkommande network adress translation (NAT) regler för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-148">Create SSH inbound network address translation (NAT) rules for hello load balancer.</span></span> <span data-ttu-id="ef3b9-149">hello följande exempel skapar två regler för inläsning av belastningsutjämnare, `myLoadBalancerRuleSSH1` och `myLoadBalancerRuleSSH2`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-149">hello following example creates two load balancer rules, `myLoadBalancerRuleSSH1` and `myLoadBalancerRuleSSH2`:</span></span>

```azurecli
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

<span data-ttu-id="ef3b9-150">Skapa hello webb ingående NAT-regler för hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-150">Create hello web inbound NAT rules for hello load balancer.</span></span> <span data-ttu-id="ef3b9-151">hello följande exempel skapas en regel för belastningsutjämnare med namnet `myLoadBalancerRuleWeb`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-151">hello following example creates a load balancer rule named `myLoadBalancerRuleWeb`:</span></span>

```azurecli
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

<span data-ttu-id="ef3b9-152">Skapa hello belastningsutjämnaren, hälsoavsökningen.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-152">Create hello load balancer health probe.</span></span> <span data-ttu-id="ef3b9-153">hello följande exempel skapas en TCP-avsökning med namnet `myHealthProbe`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-153">hello following example creates a TCP probe named `myHealthProbe`:</span></span>

```azurecli
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

<span data-ttu-id="ef3b9-154">Kontrollera hello belastningsutjämnare, IP-adresspooler och NAT-regler med hjälp av hello JSON-parsern:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-154">Verify hello load balancer, IP pools, and NAT rules by using hello JSON parser:</span></span>

```azurecli
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

<span data-ttu-id="ef3b9-155">Skapa hello första nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-155">Create hello first network interface card (NIC).</span></span> <span data-ttu-id="ef3b9-156">Ersätt hello `#####-###-###` avsnitt med din egen Azure prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-156">Replace hello `#####-###-###` sections with your own Azure subscription ID.</span></span> <span data-ttu-id="ef3b9-157">Din prenumeration ID anges i hello utdata från **jq** när du undersöker hello-resurser som du skapar.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-157">Your subscription ID is noted in hello output of **jq** when you examine hello resources you are creating.</span></span> <span data-ttu-id="ef3b9-158">Du kan också visa ditt prenumerations-ID med `azure account list`.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-158">You can also view your subscription ID with `azure account list`.</span></span>

<span data-ttu-id="ef3b9-159">hello följande exempel skapas ett nätverkskort med namnet `myNic1`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-159">hello following example creates a NIC named `myNic1`:</span></span>

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

<span data-ttu-id="ef3b9-160">Skapa hello andra nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-160">Create hello second NIC.</span></span> <span data-ttu-id="ef3b9-161">hello följande exempel skapas ett nätverkskort med namnet `myNic2`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-161">hello following example creates a NIC named `myNic2`:</span></span>

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

<span data-ttu-id="ef3b9-162">Verifiera hello två nätverkskort med hello JSON-parsern:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-162">Verify hello two NICs by using hello JSON parser:</span></span>

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

<span data-ttu-id="ef3b9-163">Skapa hello nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-163">Create hello network security group.</span></span> <span data-ttu-id="ef3b9-164">hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-164">hello following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

<span data-ttu-id="ef3b9-165">Lägga till två regler för inkommande trafik för hello nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-165">Add two inbound rules for hello network security group.</span></span> <span data-ttu-id="ef3b9-166">hello följande exempel skapar två regler `myNetworkSecurityGroupRuleSSH` och `myNetworkSecurityGroupRuleHTTP`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-166">hello following example creates two rules, `myNetworkSecurityGroupRuleSSH` and `myNetworkSecurityGroupRuleHTTP`:</span></span>

```azurecli
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

<span data-ttu-id="ef3b9-167">Kontrollera hello nätverkssäkerhetsgruppen och regler för inkommande trafik med hjälp av hello JSON-parsern:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-167">Verify hello network security group and inbound rules by using hello JSON parser:</span></span>

```azurecli
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

<span data-ttu-id="ef3b9-168">Binda hello nätverkssäkerhet gruppera toohello två nätverkskort:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-168">Bind hello network security group toohello two NICs:</span></span>

```azurecli
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

<span data-ttu-id="ef3b9-169">Skapa hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-169">Create hello availability set.</span></span> <span data-ttu-id="ef3b9-170">hello följande exempel skapas en tillgänglighetsuppsättning namngivna `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-170">hello following example creates an availability set named `myAvailabilitySet`:</span></span>

```azurecli
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

<span data-ttu-id="ef3b9-171">Skapa hello första Linux VM.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-171">Create hello first Linux VM.</span></span> <span data-ttu-id="ef3b9-172">hello följande exempel skapas en virtuell dator med namnet `myVM1`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-172">hello following example creates a VM named `myVM1`:</span></span>

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

<span data-ttu-id="ef3b9-173">Skapa hello andra Linux VM.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-173">Create hello second Linux VM.</span></span> <span data-ttu-id="ef3b9-174">hello följande exempel skapas en virtuell dator med namnet `myVM2`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-174">hello following example creates a VM named `myVM2`:</span></span>

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

<span data-ttu-id="ef3b9-175">Använd hello JSON-parsern tooverify att allt som har skapats:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-175">Use hello JSON parser tooverify that everything that was built:</span></span>

```azurecli
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

<span data-ttu-id="ef3b9-176">Exportera din nya miljö tooa mallen tooquickly återskapa nya instanser:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-176">Export your new environment tooa template tooquickly re-create new instances:</span></span>

```azurecli
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="ef3b9-177">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="ef3b9-177">Detailed walkthrough</span></span>
<span data-ttu-id="ef3b9-178">hello beskrivs detaljerade anvisningar som följer vad varje kommando gör när du skapar i din miljö.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-178">hello detailed steps that follow explain what each command is doing as you build out your environment.</span></span> <span data-ttu-id="ef3b9-179">De här koncepten är användbara när du skapar dina egna anpassade miljöer för utveckling och produktion.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-179">These concepts are helpful when you build your own custom environments for development or production.</span></span>

<span data-ttu-id="ef3b9-180">Se till att du har [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) loggas och med hjälp av Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-180">Make sure that you have [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="ef3b9-181">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-181">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="ef3b9-182">Exempel parameternamn inkluderar `myResourceGroup`, `mystorageaccount`, och `myVM`.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-182">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>

## <a name="create-resource-groups-and-choose-deployment-locations"></a><span data-ttu-id="ef3b9-183">Skapa resursgrupper och välj distributionen platser</span><span class="sxs-lookup"><span data-stu-id="ef3b9-183">Create resource groups and choose deployment locations</span></span>
<span data-ttu-id="ef3b9-184">Azure-resursgrupper är distribution av logiska enheter som innehåller information och metadata tooenable hello logiska konfigurationshantering för resurs-distributioner.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-184">Azure resource groups are logical deployment entities that contain configuration information and metadata tooenable hello logical management of resource deployments.</span></span> <span data-ttu-id="ef3b9-185">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westeurope` plats:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-185">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` location:</span></span>

```azurecli
azure group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="ef3b9-186">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-186">Output:</span></span>

```azurecli                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a><span data-ttu-id="ef3b9-187">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="ef3b9-187">Create a storage account</span></span>
<span data-ttu-id="ef3b9-188">Du måste storage-konton för VM-diskar och eventuella ytterligare hårddiskar som du vill tooadd.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-188">You need storage accounts for your VM disks and for any additional data disks that you want tooadd.</span></span> <span data-ttu-id="ef3b9-189">Du kan skapa storage-konton nästan omedelbart när du skapar resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-189">You create storage accounts almost immediately after you create resource groups.</span></span>

<span data-ttu-id="ef3b9-190">Här använder vi hello `azure storage account create` kommandot Skicka hello platsen för hello konto, hello resursgruppen som styr och hello typ av Lagringsstöd som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-190">Here we use hello `azure storage account create` command, passing hello location of hello account, hello resource group that controls it, and hello type of storage support you want.</span></span> <span data-ttu-id="ef3b9-191">hello följande exempel skapas ett lagringskonto med namnet `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-191">hello following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

<span data-ttu-id="ef3b9-192">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-192">Output:</span></span>

```azurecli
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

<span data-ttu-id="ef3b9-193">tooexamine våra resursgrupp med hjälp av hello `azure group show` kommando använder vi hello [jq](https://stedolan.github.io/jq/) verktyget tillsammans med hello `--json` Azure CLI-alternativet.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-193">tooexamine our resource group by using hello `azure group show` command, let's use hello [jq](https://stedolan.github.io/jq/) tool along with hello `--json` Azure CLI option.</span></span> <span data-ttu-id="ef3b9-194">(Du kan använda **jsawk** eller något språk bibliotek som du föredrar tooparse hello JSON.)</span><span class="sxs-lookup"><span data-stu-id="ef3b9-194">(You can use **jsawk** or any language library you prefer tooparse hello JSON.)</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="ef3b9-195">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-195">Output:</span></span>

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

<span data-ttu-id="ef3b9-196">tooinvestigate hello storage-konto med hjälp av hello CLI, måste du först tooset hello kontonamn och nycklar.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-196">tooinvestigate hello storage account by using hello CLI, you first need tooset hello account names and keys.</span></span> <span data-ttu-id="ef3b9-197">Ersätt hello namnet på hello storage-konto i följande exempel med ett namn som du väljer hello:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-197">Replace hello name of hello storage account in hello following example with a name that you choose:</span></span>

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

<span data-ttu-id="ef3b9-198">Du kan sedan visa storage-informationen enkelt:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-198">Then you can view your storage information easily:</span></span>

```azurecli
azure storage container list
```

<span data-ttu-id="ef3b9-199">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-199">Output:</span></span>

```azurecli
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a><span data-ttu-id="ef3b9-200">Skapa ett virtuellt nätverk och undernät</span><span class="sxs-lookup"><span data-stu-id="ef3b9-200">Create a virtual network and subnet</span></span>
<span data-ttu-id="ef3b9-201">Härnäst ska tooneed toocreate ett virtuellt nätverk som körs i Azure och ett undernät som du kan skapa dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-201">Next you're going tooneed toocreate a virtual network running in Azure and a subnet in which you can create your VMs.</span></span> <span data-ttu-id="ef3b9-202">hello följande exempel skapas ett virtuellt nätverk med namnet `myVnet` med hello `192.168.0.0/16` adressprefix:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-202">hello following example creates a virtual network named `myVnet` with hello `192.168.0.0/16` address prefix:</span></span>

```azurecli
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16
```

<span data-ttu-id="ef3b9-203">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-203">Output:</span></span>

```azurecli
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

<span data-ttu-id="ef3b9-204">Igen, vi alternativet hello--json av `azure group show` och `jq` toosee hur vi bygger våra resurser.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-204">Again, let's use hello --json option of `azure group show` and `jq` toosee how we're building our resources.</span></span> <span data-ttu-id="ef3b9-205">Nu har vi en `storageAccounts` resurs och ett `virtualNetworks` resurs.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-205">We now have a `storageAccounts` resource and a `virtualNetworks` resource.</span></span>  

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="ef3b9-206">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-206">Output:</span></span>

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

<span data-ttu-id="ef3b9-207">Nu ska vi skapa ett undernät i hello `myVnet` virtuellt nätverk i vilka hello virtuella datorer distribueras.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-207">Now let's create a subnet in hello `myVnet` virtual network into which hello VMs are deployed.</span></span> <span data-ttu-id="ef3b9-208">Vi använder hello `azure network vnet subnet create` kommandot tillsammans med hello resurser som redan har vi skapat: hello `myResourceGroup` resursgrupp och hello `myVnet` virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-208">We use hello `azure network vnet subnet create` command, along with hello resources we've already created: hello `myResourceGroup` resource group and hello `myVnet` virtual network.</span></span> <span data-ttu-id="ef3b9-209">I följande exempel hello, vi lägga till hello undernät med namnet `mySubnet` med hello undernät adressprefixet `192.168.1.0/24`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-209">In hello following example, we add hello subnet named `mySubnet` with hello subnet address prefix of `192.168.1.0/24`:</span></span>

```azurecli
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

<span data-ttu-id="ef3b9-210">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-210">Output:</span></span>

```azurecli
info:    Executing command network vnet subnet create
+ Looking up hello subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up hello subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

<span data-ttu-id="ef3b9-211">Eftersom hello undernät är logiskt i hello virtuella nätverket, ser vi hello undernät information med ett något annorlunda kommando.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-211">Because hello subnet is logically inside hello virtual network, we look for hello subnet information with a slightly different command.</span></span> <span data-ttu-id="ef3b9-212">hello-kommandot som vi använder `azure network vnet show`, men vi fortsätta tooexamine hello JSON-utdata med hjälp av `jq`.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-212">hello command we use is `azure network vnet show`, but we continue tooexamine hello JSON output by using `jq`.</span></span>

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

<span data-ttu-id="ef3b9-213">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-213">Output:</span></span>

```json
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address"></a><span data-ttu-id="ef3b9-214">Skapa en offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="ef3b9-214">Create a public IP address</span></span>
<span data-ttu-id="ef3b9-215">Nu skapar vi hello offentlig IP-adress (PIP) som vi tilldelar tooyour belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-215">Now let's create hello public IP address (PIP) that we assign tooyour load balancer.</span></span> <span data-ttu-id="ef3b9-216">Det gör att du tooconnect tooyour virtuella datorer från hello Internet med hjälp av hello `azure network public-ip create` kommando.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-216">It enables you tooconnect tooyour VMs from hello Internet by using hello `azure network public-ip create` command.</span></span> <span data-ttu-id="ef3b9-217">Eftersom hello standardadressen är dynamiska, vi skapa en namngiven DNS-post i hello **cloudapp.azure.com** domänen med hjälp av hello `--domain-name-label` alternativet.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-217">Because hello default address is dynamic, we create a named DNS entry in hello **cloudapp.azure.com** domain by using hello `--domain-name-label` option.</span></span> <span data-ttu-id="ef3b9-218">hello följande exempel skapas en offentlig IP-adress med namnet `myPublicIP` med hello DNS-namnet på `mypublicdns`.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-218">hello following example creates a public IP named `myPublicIP` with hello DNS name of `mypublicdns`.</span></span> <span data-ttu-id="ef3b9-219">Eftersom hello DNS-namnet måste vara unikt, kan du ange egna unikt DNS-namn:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-219">Because hello DNS name must be unique, you provide your own unique DNS name:</span></span>

```azurecli
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

<span data-ttu-id="ef3b9-220">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-220">Output:</span></span>

```azurecli
info:    Executing command network public-ip create
+ Looking up hello public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up hello public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

<span data-ttu-id="ef3b9-221">hello offentliga IP-adressen är också en översta resurs så att du kan se den med `azure group show`.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-221">hello public IP address is also a top-level resource, so you can see it with `azure group show`.</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="ef3b9-222">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-222">Output:</span></span>

```json
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

<span data-ttu-id="ef3b9-223">Du kan undersöka mer resursinformation,, inklusive hello fullständigt kvalificerade domännamnet (FQDN) för hello underdomänen med hjälp av hello fullständig `azure network public-ip show` kommando.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-223">You can investigate more resource details, including hello fully qualified domain name (FQDN) of hello subdomain, by using hello complete `azure network public-ip show` command.</span></span> <span data-ttu-id="ef3b9-224">hello offentliga IP-adressresurs har allokerats logiskt, men en specifik adress har ännu inte tilldelats.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-224">hello public IP address resource has been allocated logically, but a specific address has not yet been assigned.</span></span> <span data-ttu-id="ef3b9-225">tooobtain en IP-adress ska tooneed en belastningsutjämnare som vi ännu inte har skapats.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-225">tooobtain an IP address, you're going tooneed a load balancer, which we have not yet created.</span></span>

```azurecli
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

<span data-ttu-id="ef3b9-226">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-226">Output:</span></span>

```json
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a><span data-ttu-id="ef3b9-227">Skapa en belastningsutjämnare och IP-pooler</span><span class="sxs-lookup"><span data-stu-id="ef3b9-227">Create a load balancer and IP pools</span></span>
<span data-ttu-id="ef3b9-228">När du skapar en belastningsutjämnare kan du toodistribute trafik över flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-228">When you create a load balancer, it enables you toodistribute traffic across multiple VMs.</span></span> <span data-ttu-id="ef3b9-229">Det ger också redundans tooyour program genom att köra flera virtuella datorer som svarar toouser begäranden i hello händelsen för underhåll eller tunga laster.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-229">It also provides redundancy tooyour application by running multiple VMs that respond toouser requests in hello event of maintenance or heavy loads.</span></span> <span data-ttu-id="ef3b9-230">hello följande exempel skapas en belastningsutjämnare med namnet `myLoadBalancer`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-230">hello following example creates a load balancer named `myLoadBalancer`:</span></span>

```azurecli
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

<span data-ttu-id="ef3b9-231">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-231">Output:</span></span>

```azurecli
info:    Executing command network lb create
+ Looking up hello load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

<span data-ttu-id="ef3b9-232">Vår belastningsutjämnare är ganska tomt, så vi skapa vissa IP-adresspooler.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-232">Our load balancer is fairly empty, so let's create some IP pools.</span></span> <span data-ttu-id="ef3b9-233">Vi vill toocreate två IP-adresspooler för våra belastningsutjämnare, en för hello klientdelen och en för hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-233">We want toocreate two IP pools for our load balancer, one for hello front end and one for hello back end.</span></span> <span data-ttu-id="ef3b9-234">hello frontend IP-adresspool är synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-234">hello front-end IP pool is publicly visible.</span></span> <span data-ttu-id="ef3b9-235">Det är också hello plats toowhich vi tilldela hello PIP som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-235">It's also hello location toowhich we assign hello PIP that we created earlier.</span></span> <span data-ttu-id="ef3b9-236">Sedan använder vi hello backend-adresspool som en plats för våra VMs tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-236">Then we use hello back-end pool as a location for our VMs tooconnect to.</span></span> <span data-ttu-id="ef3b9-237">På så sätt kan hello trafiken kan flöda via hello load balancer toohello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-237">That way, hello traffic can flow through hello load balancer toohello VMs.</span></span>

<span data-ttu-id="ef3b9-238">Först skapar vi vårt frontend IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-238">First, let's create our front-end IP pool.</span></span> <span data-ttu-id="ef3b9-239">hello följande exempel skapas en frontend-pool med namnet `myFrontEndPool`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-239">hello following example creates a front-end pool named `myFrontEndPool`:</span></span>

```azurecli
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool
```

<span data-ttu-id="ef3b9-240">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-240">Output:</span></span>

```azurecli
info:    Executing command network lb frontend-ip create
+ Looking up hello load balancer "myLoadBalancer"
+ Looking up hello public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

<span data-ttu-id="ef3b9-241">Observera hur vi använde hello `--public-ip-name` växla toopass i hello `myPublicIP` som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-241">Note how we used hello `--public-ip-name` switch toopass in hello `myPublicIP` that we created earlier.</span></span> <span data-ttu-id="ef3b9-242">Tilldela hello offentliga IP-adress toohello belastningsutjämnare kan du tooreach din virtuella dator över hello Internet.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-242">Assigning hello public IP address toohello load balancer allows you tooreach your VMs across hello Internet.</span></span>

<span data-ttu-id="ef3b9-243">Nu ska vi skapa våra andra IP-adresspool nu för våra backend-trafik.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-243">Next, let's create our second IP pool, this time for our back-end traffic.</span></span> <span data-ttu-id="ef3b9-244">hello följande exempel skapas en backend-pool med namnet `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-244">hello following example creates a back-end pool named `myBackEndPool`:</span></span>

```azurecli
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

<span data-ttu-id="ef3b9-245">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-245">Output:</span></span>

```azurecli
info:    Executing command network lb address-pool create
+ Looking up hello load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

<span data-ttu-id="ef3b9-246">Vi kan se hur våra belastningsutjämnaren fungerar genom att titta med `azure network lb show` och undersöka hello JSON-utdata:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-246">We can see how our load balancer is doing by looking with `azure network lb show` and examining hello JSON output:</span></span>

```azurecli
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

<span data-ttu-id="ef3b9-247">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-247">Output:</span></span>

```json
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a><span data-ttu-id="ef3b9-248">Skapa belastningsutjämnaren NAT-regler</span><span class="sxs-lookup"><span data-stu-id="ef3b9-248">Create load balancer NAT rules</span></span>
<span data-ttu-id="ef3b9-249">tooget trafik som passerar genom vår belastningsutjämnaren måste toocreate network address translation (NAT) regler som anger inkommande eller utgående åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-249">tooget traffic flowing through our load balancer, we need toocreate network address translation (NAT) rules that specify either inbound or outbound actions.</span></span> <span data-ttu-id="ef3b9-250">Du kan ange hello protokollet toouse sedan mappa externa portar toointernal portar som du vill.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-250">You can specify hello protocol toouse, then map external ports toointernal ports as desired.</span></span> <span data-ttu-id="ef3b9-251">Nu ska vi skapa vissa regler som tillåter SSH via vårt belastningen belastningsutjämnaren tooour virtuella datorer för våra miljö.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-251">For our environment, let's create some rules that allow SSH through our load balancer tooour VMs.</span></span> <span data-ttu-id="ef3b9-252">Vi konfigurerar TCP-portarna 4222 och 4223 toodirect tooTCP-port 22 på vår virtuella datorer (som vi skapa senare).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-252">We set up TCP ports 4222 and 4223 toodirect tooTCP port 22 on our VMs (which we create later).</span></span> <span data-ttu-id="ef3b9-253">hello följande exempel skapas en regel med namnet `myLoadBalancerRuleSSH1` toomap TCP-port 4222 tooport 22:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-253">hello following example creates a rule named `myLoadBalancerRuleSSH1` toomap TCP port 4222 tooport 22:</span></span>

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

<span data-ttu-id="ef3b9-254">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-254">Output:</span></span>

```azurecli
info:    Executing command network lb inbound-nat-rule create
+ Looking up hello load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

<span data-ttu-id="ef3b9-255">Upprepa hello proceduren för andra NAT-regel för SSH.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-255">Repeat hello procedure for your second NAT rule for SSH.</span></span> <span data-ttu-id="ef3b9-256">hello följande exempel skapas en regel med namnet `myLoadBalancerRuleSSH2` toomap TCP-port 4223 tooport 22:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-256">hello following example creates a rule named `myLoadBalancerRuleSSH2` toomap TCP port 4223 tooport 22:</span></span>

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

<span data-ttu-id="ef3b9-257">Nu ska vi också gå vidare och skapa en NAT-regel för TCP-port 80 för webbtrafik, koppla upp hello regeln upp tooour IP-adresspooler.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-257">Let's also go ahead and create a NAT rule for TCP port 80 for web traffic, hooking hello rule up tooour IP pools.</span></span> <span data-ttu-id="ef3b9-258">Om vi koppla samman hello regeln tooan IP-poolen, i stället för att koppla upp dig hello regeln tooour VMs individuellt kan vi lägga till eller ta bort virtuella datorer från hello IP-pool.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-258">If we hook up hello rule tooan IP pool, instead of hooking up hello rule tooour VMs individually, we can add or remove VMs from hello IP pool.</span></span> <span data-ttu-id="ef3b9-259">hello belastningsutjämnaren justeras automatiskt hello flödet av trafik.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-259">hello load balancer automatically adjusts hello flow of traffic.</span></span> <span data-ttu-id="ef3b9-260">hello följande exempel skapas en regel med namnet `myLoadBalancerRuleWeb` toomap TCP-port 80 tooport 80:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-260">hello following example creates a rule named `myLoadBalancerRuleWeb` toomap TCP port 80 tooport 80:</span></span>

```azurecli
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

<span data-ttu-id="ef3b9-261">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-261">Output:</span></span>

```azurecli
info:    Executing command network lb rule create
+ Looking up hello load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a><span data-ttu-id="ef3b9-262">Skapa en belastningsutjämnaren, hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="ef3b9-262">Create a load balancer health probe</span></span>
<span data-ttu-id="ef3b9-263">En avsökning med jämna mellanrum kontroller på hello virtuella datorer som finns bakom våra belastningen belastningsutjämnaren toomake att de fungerar och svarar toorequests som har definierats.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-263">A health probe periodically checks on hello VMs that are behind our load balancer toomake sure they're operating and responding toorequests as defined.</span></span> <span data-ttu-id="ef3b9-264">Om inte, de tas bort från dirigerad åtgärden tooensure som användare inte toothem.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-264">If not, they're removed from operation tooensure that users aren't being directed toothem.</span></span> <span data-ttu-id="ef3b9-265">Du kan definiera egna kontroller för hello hälsoavsökningen tillsammans med intervall och timeout-värden.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-265">You can define custom checks for hello health probe, along with intervals and timeout values.</span></span> <span data-ttu-id="ef3b9-266">Läs mer om hälsoavsökningar [avsökningar belastningsutjämnaren](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-266">For more information about health probes, see [Load Balancer probes](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="ef3b9-267">hello följande exempel skapas en TCP hälsa avsöktes namngivna `myHealthProbe`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-267">hello following example creates a TCP health probed named `myHealthProbe`:</span></span>

```azurecli
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

<span data-ttu-id="ef3b9-268">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-268">Output:</span></span>

```azurecli
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up hello load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

<span data-ttu-id="ef3b9-269">Vi anges här, ett intervall på 15 sekunder för våra hälsokontroller.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-269">Here, we specified an interval of 15 seconds for our health checks.</span></span> <span data-ttu-id="ef3b9-270">Vi kan missa högst fyra avsökningar (en minut) innan hello belastningsutjämnaren anser hello värden fungerar inte längre.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-270">We can miss a maximum of four probes (one minute) before hello load balancer considers that hello host is no longer functioning.</span></span>

## <a name="verify-hello-load-balancer"></a><span data-ttu-id="ef3b9-271">Kontrollera hello belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ef3b9-271">Verify hello load balancer</span></span>
<span data-ttu-id="ef3b9-272">Nu görs hello konfiguration av belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-272">Now hello load balancer configuration is done.</span></span> <span data-ttu-id="ef3b9-273">Här är hello steg som du tog:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-273">Here are hello steps you took:</span></span>

1. <span data-ttu-id="ef3b9-274">Du har skapat en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-274">You created a load balancer.</span></span>
2. <span data-ttu-id="ef3b9-275">Du har skapat en frontend IP-adresspool och tilldelats en offentlig IP-tooit.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-275">You created a front-end IP pool and assigned a public IP tooit.</span></span>
3. <span data-ttu-id="ef3b9-276">Du har skapat en backend-IP-adresspool som virtuella datorer kan ansluta till.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-276">You created a back-end IP pool that VMs can connect to.</span></span>
4. <span data-ttu-id="ef3b9-277">Du har skapat NAT-regler som tillåter SSH toohello virtuella datorer för hantering, tillsammans med en regel som tillåter TCP-port 80 för våra webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-277">You created NAT rules that allow SSH toohello VMs for management, along with a rule that allows TCP port 80 for our web app.</span></span>
5. <span data-ttu-id="ef3b9-278">Du har lagt till en hälsa avsökningen tooperiodically Kontrollera hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-278">You added a health probe tooperiodically check hello VMs.</span></span> <span data-ttu-id="ef3b9-279">Den här hälsoavsökningen säkerställer att användare inte försöker tooaccess en virtuell dator som inte längre fungerar eller betjänar innehåll.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-279">This health probe ensures that users don't try tooaccess a VM that is no longer functioning or serving content.</span></span>

<span data-ttu-id="ef3b9-280">Nu ska vi se hur din belastningsutjämnare ser ut nu:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-280">Let's review what your load balancer looks like now:</span></span>

```azurecli
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

<span data-ttu-id="ef3b9-281">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-281">Output:</span></span>

```json
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-toouse-with-hello-linux-vm"></a><span data-ttu-id="ef3b9-282">Skapa ett NIC-toouse med hello Linux VM</span><span class="sxs-lookup"><span data-stu-id="ef3b9-282">Create an NIC toouse with hello Linux VM</span></span>
<span data-ttu-id="ef3b9-283">Nätverkskort är tillgängliga via programmering eftersom du kan använda regler tootheir användning.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-283">NICs are programmatically available because you can apply rules tootheir use.</span></span> <span data-ttu-id="ef3b9-284">Du kan också ha fler än en.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-284">You can also have more than one.</span></span> <span data-ttu-id="ef3b9-285">I följande hello `azure network nic create` kommandot du koppla samman hello NIC toohello belastningen backend-IP-pool och associera det med hello NAT-regeln toopermit SSH-trafik.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-285">In hello following `azure network nic create` command, you hook up hello NIC toohello load back-end IP pool and associate it with hello NAT rule toopermit SSH traffic.</span></span>

<span data-ttu-id="ef3b9-286">Ersätt hello `#####-###-###` avsnitt med din egen Azure prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-286">Replace hello `#####-###-###` sections with your own Azure subscription ID.</span></span> <span data-ttu-id="ef3b9-287">Din prenumeration ID anges i hello utdata från `jq` när du undersöker hello-resurser som du skapar.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-287">Your subscription ID is noted in hello output of `jq` when you examine hello resources you are creating.</span></span> <span data-ttu-id="ef3b9-288">Du kan också visa ditt prenumerations-ID med `azure account list`.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-288">You can also view your subscription ID with `azure account list`.</span></span>

<span data-ttu-id="ef3b9-289">hello följande exempel skapas ett nätverkskort med namnet `myNic1`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-289">hello following example creates a NIC named `myNic1`:</span></span>

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

<span data-ttu-id="ef3b9-290">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-290">Output:</span></span>

```azurecli
info:    Executing command network nic create
+ Looking up hello subnet "mySubnet"
+ Looking up hello network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

<span data-ttu-id="ef3b9-291">Du kan se hello information genom att undersöka hello resursen direkt.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-291">You can see hello details by examining hello resource directly.</span></span> <span data-ttu-id="ef3b9-292">Du undersöka hello resursen med hjälp av hello `azure network nic show` kommando:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-292">You examine hello resource by using hello `azure network nic show` command:</span></span>

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

<span data-ttu-id="ef3b9-293">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-293">Output:</span></span>

```json
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

<span data-ttu-id="ef3b9-294">Nu skapar vi hello andra nätverkskortet, koppla upp i tooour backend-IP-poolen igen.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-294">Now we create hello second NIC, hooking in tooour back-end IP pool again.</span></span> <span data-ttu-id="ef3b9-295">Den här gången hello andra NAT-regeln tillåter SSH-trafik.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-295">This time hello second NAT rule permits SSH traffic.</span></span> <span data-ttu-id="ef3b9-296">hello följande exempel skapas ett nätverkskort med namnet `myNic2`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-296">hello following example creates a NIC named `myNic2`:</span></span>

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a><span data-ttu-id="ef3b9-297">Skapa en säkerhetsgrupp för nätverk och regler</span><span class="sxs-lookup"><span data-stu-id="ef3b9-297">Create a network security group and rules</span></span>
<span data-ttu-id="ef3b9-298">Nu när vi skapa en säkerhetsgrupp för nätverk och hello inkommande regler som styr åtkomst till toohello NIC.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-298">Now we create a network security group and hello inbound rules that govern access toohello NIC.</span></span> <span data-ttu-id="ef3b9-299">En nätverkssäkerhetsgrupp kan vara tillämpade tooa nätverkskort eller ett undernät.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-299">A network security group can be applied tooa NIC or subnet.</span></span> <span data-ttu-id="ef3b9-300">Du kan definiera regler toocontrol hello flödet av trafik till och från dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-300">You define rules toocontrol hello flow of traffic in and out of your VMs.</span></span> <span data-ttu-id="ef3b9-301">hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-301">hello following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

<span data-ttu-id="ef3b9-302">Lägg till hello regel för inkommande trafik hello NSG tooallow inkommande anslutningar på port 22 (toosupport SSH).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-302">Let's add hello inbound rule for hello NSG tooallow inbound connections on port 22 (toosupport SSH).</span></span> <span data-ttu-id="ef3b9-303">hello följande exempel skapas en regel med namnet `myNetworkSecurityGroupRuleSSH` tooallow TCP på port 22:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-303">hello following example creates a rule named `myNetworkSecurityGroupRuleSSH` tooallow TCP on port 22:</span></span>

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

<span data-ttu-id="ef3b9-304">Nu ska vi lägga till hello regel för inkommande trafik hello NSG tooallow inkommande anslutningar på port 80 (toosupport webbtrafik).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-304">Now let's add hello inbound rule for hello NSG tooallow inbound connections on port 80 (toosupport web traffic).</span></span> <span data-ttu-id="ef3b9-305">hello följande exempel skapas en regel med namnet `myNetworkSecurityGroupRuleHTTP` tooallow TCP på port 80:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-305">hello following example creates a rule named `myNetworkSecurityGroupRuleHTTP` tooallow TCP on port 80:</span></span>

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [!NOTE]
> <span data-ttu-id="ef3b9-306">regel för inkommande trafik hello är ett filter för inkommande nätverksanslutningar.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-306">hello inbound rule is a filter for inbound network connections.</span></span> <span data-ttu-id="ef3b9-307">I det här exemplet Binder vi hello NSG toohello virtuellt nätverkskort för virtuella datorer, vilket innebär att alla begäran tooport 22 överförs via toohello NIC på vår VM.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-307">In this example, we bind hello NSG toohello VMs virtual NIC, which means that any request tooport 22 is passed through toohello NIC on our VM.</span></span> <span data-ttu-id="ef3b9-308">Denna regel för inkommande trafik som handlar om en nätverksanslutning och inte om en slutpunkt som är det skulle ha varit om i klassiska distributioner.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-308">This inbound rule is about a network connection, and not about an endpoint, which is what it would be about in classic deployments.</span></span> <span data-ttu-id="ef3b9-309">tooopen en port du lämnar hello `--source-port-range` angetts för '\*' (hello standardvärdet) tooaccept inkommande begäranden från **alla** begär port.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-309">tooopen a port, you must leave hello `--source-port-range` set too'\*' (hello default value) tooaccept inbound requests from **any** requesting port.</span></span> <span data-ttu-id="ef3b9-310">Det finns vanligtvis dynamiska portar.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-310">Ports are typically dynamic.</span></span>
>
>

## <a name="bind-toohello-nic"></a><span data-ttu-id="ef3b9-311">Binda toohello NIC</span><span class="sxs-lookup"><span data-stu-id="ef3b9-311">Bind toohello NIC</span></span>
<span data-ttu-id="ef3b9-312">Binda hello NSG toohello nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-312">Bind hello NSG toohello NICs.</span></span> <span data-ttu-id="ef3b9-313">Vi behöver tooconnect våra nätverkskort med vår nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-313">We need tooconnect our NICs with our network security group.</span></span> <span data-ttu-id="ef3b9-314">Kör både kommandon toohook in båda våra nätverkskort:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-314">Run both commands, toohook up both of our NICs:</span></span>

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a><span data-ttu-id="ef3b9-315">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="ef3b9-315">Create an availability set</span></span>
<span data-ttu-id="ef3b9-316">Tillgänglighetsuppsättningar hjälp spridning din virtuella dator över feldomäner och uppgradera domäner.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-316">Availability sets help spread your VMs across fault domains and upgrade domains.</span></span> <span data-ttu-id="ef3b9-317">Nu ska vi skapa en tillgänglighetsuppsättning för dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-317">Let's create an availability set for your VMs.</span></span> <span data-ttu-id="ef3b9-318">hello följande exempel skapas en tillgänglighetsuppsättning namngivna `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-318">hello following example creates an availability set named `myAvailabilitySet`:</span></span>

```azurecli
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

<span data-ttu-id="ef3b9-319">Feldomäner definiera en gruppering av virtuella datorer som delar en gemensam käll- och strömbrytare.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-319">Fault domains define a grouping of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="ef3b9-320">Som standard separeras hello virtuella datorer som har konfigurerats i din tillgänglighetsuppsättning över in toothree feldomäner.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-320">By default, hello virtual machines that are configured within your availability set are separated across up toothree fault domains.</span></span> <span data-ttu-id="ef3b9-321">hello idé är att maskinvaruproblem i något av dessa feldomäner inte påverkar varje virtuell dator som kör din app.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-321">hello idea is that a hardware issue in one of these fault domains does not affect every VM that is running your app.</span></span> <span data-ttu-id="ef3b9-322">Azure distribuerar automatiskt virtuella datorer över hello feldomäner när de placeras i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-322">Azure automatically distributes VMs across hello fault domains when placing them in an availability set.</span></span>

<span data-ttu-id="ef3b9-323">Uppgraderingsdomäner ange grupper av virtuella datorer och underliggande fysiska maskinvara som kan startas på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-323">Upgrade domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at hello same time.</span></span> <span data-ttu-id="ef3b9-324">hello ordning i vilken uppgraderingsdomäner startas om kan inte vara sekventiella under planerat underhåll, men endast en uppgradering på att startas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-324">hello order in which upgrade domains are rebooted might not be sequential during planned maintenance, but only one upgrade is rebooted at a time.</span></span> <span data-ttu-id="ef3b9-325">Igen, Azure automatiskt distribuerar din virtuella dator över uppgraderingsdomäner när de placeras i en tillgänglig plats.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-325">Again, Azure automatically distributes your VMs across upgrade domains when placing them in an availability site.</span></span>

<span data-ttu-id="ef3b9-326">Läs mer om [hantera hello tillgängligheten för virtuella datorer](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-326">Read more about [managing hello availability of VMs](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="create-hello-linux-vms"></a><span data-ttu-id="ef3b9-327">Skapa hello virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="ef3b9-327">Create hello Linux VMs</span></span>
<span data-ttu-id="ef3b9-328">Du har skapat hello lagring och nätverksresurser toosupport Internet-tillgängliga virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-328">You've created hello storage and network resources toosupport Internet-accessible VMs.</span></span> <span data-ttu-id="ef3b9-329">Nu ska vi skapa de virtuella datorer och skydda dem med en SSH-nyckel som inte har ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-329">Now let's create those VMs and secure them with an SSH key that doesn't have a password.</span></span> <span data-ttu-id="ef3b9-330">I det här fallet ska vi toocreate en Ubuntu VM baserat på hello senaste LTS.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-330">In this case, we're going toocreate an Ubuntu VM based on hello most recent LTS.</span></span> <span data-ttu-id="ef3b9-331">Vi Leta upp avbildningsinformationen med hjälp av `azure vm image list`, enligt beskrivningen i [söker Azure VM-bilder](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-331">We locate that image information by using `azure vm image list`, as described in [finding Azure VM images](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="ef3b9-332">Vi valde en bild med hello kommandot `azure vm image list westeurope canonical | grep LTS`.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-332">We selected an image by using hello command `azure vm image list westeurope canonical | grep LTS`.</span></span> <span data-ttu-id="ef3b9-333">I detta fall kan vi använda `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-333">In this case, we use `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`.</span></span> <span data-ttu-id="ef3b9-334">Hello sista fältet, skickar vi `latest` så att i framtiden hello vi alltid får hello den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-334">For hello last field, we pass `latest` so that in hello future we always get hello most recent build.</span></span> <span data-ttu-id="ef3b9-335">(vi använder hello-strängen är `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-335">(hello string we use is `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).</span></span>

<span data-ttu-id="ef3b9-336">Nästa steg är bekant tooanyone som redan har skapat ett ssh rsa-offentliga och privata nyckel koppla på Linux- eller Mac med hjälp av **ssh-keygen - t rsa -b 2048**.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-336">This next step is familiar tooanyone who has already created an ssh rsa public and private key pair on Linux or Mac by using **ssh-keygen -t rsa -b 2048**.</span></span> <span data-ttu-id="ef3b9-337">Om du inte har några certifikat nyckelpar i din `~/.ssh` katalog så kan du skapa dem:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-337">If you do not have any certificate key pairs in your `~/.ssh` directory, you can create them:</span></span>

* <span data-ttu-id="ef3b9-338">Automatiskt, genom att använda hello `azure vm create --generate-ssh-keys` alternativet.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-338">Automatically, by using hello `azure vm create --generate-ssh-keys` option.</span></span>
* <span data-ttu-id="ef3b9-339">Manuellt med hjälp av [hello instruktioner toocreate dem själv](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-339">Manually, by using [hello instructions toocreate them yourself](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="ef3b9-340">Du kan också använda hello `--admin-password` metoden tooauthenticate SSH-anslutningar när hello VM skapas.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-340">Alternatively, you can use hello `--admin-password` method tooauthenticate your SSH connections after hello VM is created.</span></span> <span data-ttu-id="ef3b9-341">Den här metoden är vanligtvis mindre säker.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-341">This method is typically less secure.</span></span>

<span data-ttu-id="ef3b9-342">Vi skapar hello VM genom att våra resurser och information tillsammans med hello `azure vm create` kommando:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-342">We create hello VM by bringing all our resources and information together with hello `azure vm create` command:</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

<span data-ttu-id="ef3b9-343">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-343">Output:</span></span>

```azurecli
info:    Executing command vm create
+ Looking up hello VM "myVM1"
info:    Verifying hello public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using hello VM Size "Standard_DS1"
info:    hello [OS, Data] Disk or image configuration requires storage account
+ Looking up hello storage account mystorageaccount
+ Looking up hello availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up hello NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in hello NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    hello storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by hello parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

<span data-ttu-id="ef3b9-344">Du kan ansluta tooyour VM omedelbart genom att använda din standard SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-344">You can connect tooyour VM immediately by using your default SSH keys.</span></span> <span data-ttu-id="ef3b9-345">Se till att du anger rätt port för hello eftersom vi passera hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-345">Make sure that you specify hello appropriate port since we're passing through hello load balancer.</span></span> <span data-ttu-id="ef3b9-346">(För vårt första VM vi ställa in hello NAT-regel tooforward port 4222 tooour VM.)</span><span class="sxs-lookup"><span data-stu-id="ef3b9-346">(For our first VM, we set up hello NAT rule tooforward port 4222 tooour VM.)</span></span>

```bash
ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

<span data-ttu-id="ef3b9-347">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-347">Output:</span></span>

```bash
hello authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want toocontinue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) toohello list of known hosts.
Welcome tooUbuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

<span data-ttu-id="ef3b9-348">Gå vidare och skapa den virtuella datorn med andra i hello samma sätt:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-348">Go ahead and create your second VM in hello same manner:</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

<span data-ttu-id="ef3b9-349">Du kan nu använda hello `azure vm show myResourceGroup myVM1` kommandot tooexamine vad du har skapat.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-349">And you can now use hello `azure vm show myResourceGroup myVM1` command tooexamine what you've created.</span></span> <span data-ttu-id="ef3b9-350">Nu är kör du dina Ubuntu virtuella datorer bakom en belastningsutjämnare i Azure som du kan logga in till endast med SSH-nyckel (eftersom lösenord är inaktiverade).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-350">At this point, you're running your Ubuntu VMs behind a load balancer in Azure that you can sign into only with your SSH key pair (because passwords are disabled).</span></span> <span data-ttu-id="ef3b9-351">Du kan installera nginx eller httpd distribuera en webbapp och se hello trafik flödet genom hello belastningen belastningsutjämnaren tooboth hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-351">You can install nginx or httpd, deploy a web app, and see hello traffic flow through hello load balancer tooboth of hello VMs.</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myVM1
```

<span data-ttu-id="ef3b9-352">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-352">Output:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up hello VM "TestVM1"
+ Looking up hello NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-hello-environment-as-a-template"></a><span data-ttu-id="ef3b9-353">Exportera hello miljö som en mall</span><span class="sxs-lookup"><span data-stu-id="ef3b9-353">Export hello environment as a template</span></span>
<span data-ttu-id="ef3b9-354">Nu när du har skapat i den här miljön om du vill att toocreate en ytterligare utvecklingsmiljö med hello samma parametrar eller en produktionsmiljö som matchar det?</span><span class="sxs-lookup"><span data-stu-id="ef3b9-354">Now that you have built out this environment, what if you want toocreate an additional development environment with hello same parameters, or a production environment that matches it?</span></span> <span data-ttu-id="ef3b9-355">Hanteraren för filserverresurser använder JSON-mallar som definierar alla hello parametrar för din miljö.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-355">Resource Manager uses JSON templates that define all hello parameters for your environment.</span></span> <span data-ttu-id="ef3b9-356">Du bygga ut hela miljöer genom att referera till den här JSON-mallen.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-356">You build out entire environments by referencing this JSON template.</span></span> <span data-ttu-id="ef3b9-357">Du kan [skapa JSON-mallarna manuellt](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller exportera en befintlig miljö toocreate hello JSON mall:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-357">You can [build JSON templates manually](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or export an existing environment toocreate hello JSON template for you:</span></span>

```azurecli
azure group export --name myResourceGroup
```

<span data-ttu-id="ef3b9-358">Det här kommandot skapar hello `myResourceGroup.json` fil i den aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-358">This command creates hello `myResourceGroup.json` file in your current working directory.</span></span> <span data-ttu-id="ef3b9-359">När du skapar en miljö med den här mallen kan uppmanas du alla hello resursnamn, inklusive hello namn för hello belastningsutjämnare, nätverkskort eller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-359">When you create an environment from this template, you are prompted for all hello resource names, including hello names for hello load balancer, network interfaces, or VMs.</span></span> <span data-ttu-id="ef3b9-360">Du kan fylla i dessa namn i mallfilen genom att lägga till hello `-p` eller `--includeParameterDefaultValue` parametern toohello `azure group export` kommando som visades tidigare.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-360">You can populate these names in your template file by adding hello `-p` or `--includeParameterDefaultValue` parameter toohello `azure group export` command that was shown earlier.</span></span> <span data-ttu-id="ef3b9-361">Redigera din JSON mallen toospecify hello resursnamn, eller [skapa en fil med parameters.json](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) som anger hello resursnamn.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-361">Edit your JSON template toospecify hello resource names, or [create a parameters.json file](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) that specifies hello resource names.</span></span>

<span data-ttu-id="ef3b9-362">toocreate en miljö med din mall:</span><span class="sxs-lookup"><span data-stu-id="ef3b9-362">toocreate an environment from your template:</span></span>

```azurecli
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

<span data-ttu-id="ef3b9-363">Du kanske vill tooread [mer om hur toodeploy från mallar](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ef3b9-363">You might want tooread [more about how toodeploy from templates](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="ef3b9-364">Läs mer om hur tooincrementally uppdatering miljöer, använda hello parameterfilen och komma åt mallar från en enda lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-364">Learn about how tooincrementally update environments, use hello parameters file, and access templates from a single storage location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ef3b9-365">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ef3b9-365">Next steps</span></span>
<span data-ttu-id="ef3b9-366">Nu är du redo toobegin arbeta med flera nätverkskomponenter och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-366">Now you're ready toobegin working with multiple networking components and VMs.</span></span> <span data-ttu-id="ef3b9-367">Du kan använda det här exemplet miljö toobuild ut ditt program med hjälp av hello kärnkomponenter introduceras här.</span><span class="sxs-lookup"><span data-stu-id="ef3b9-367">You can use this sample environment toobuild out your application by using hello core components introduced here.</span></span>
