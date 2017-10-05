---
title: "Skapa en fullständig Linux-miljö med Azure CLI 1.0 | Microsoft Docs"
description: "Skapa lagring, en Linux VM, ett virtuellt nätverk och undernät, belastningsutjämning, ett nätverkskort, en offentlig IP-adress och en nätverkssäkerhetsgrupp alla från grunden med hjälp av Azure CLI 1.0."
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
ms.openlocfilehash: 201ccd523e49d638ace50fbc0ffdceb705b35473
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-complete-linux-environment-with-the-azure-cli-10"></a><span data-ttu-id="bce33-103">Skapa en fullständig Linux-miljö med Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="bce33-103">Create a complete Linux environment with the Azure CLI 1.0</span></span>
<span data-ttu-id="bce33-104">I den här artikeln skapar vi ett enkelt nätverk med en belastningsutjämnare och ett par med virtuella datorer som är användbara för utveckling och enkel datoranvändning.</span><span class="sxs-lookup"><span data-stu-id="bce33-104">In this article, we build a simple network with a load balancer and a pair of VMs that are useful for development and simple computing.</span></span> <span data-ttu-id="bce33-105">Vi går igenom processen kommandot av kommandot förrän du har två fungerar, säker virtuella Linux-datorer som du kan ansluta från var som helst på Internet.</span><span class="sxs-lookup"><span data-stu-id="bce33-105">We walk through the process command by command, until you have two working, secure Linux VMs to which you can connect from anywhere on the Internet.</span></span> <span data-ttu-id="bce33-106">Sedan kan du gå vidare till mer komplexa nätverk och miljöer.</span><span class="sxs-lookup"><span data-stu-id="bce33-106">Then you can move on to more complex networks and environments.</span></span>

<span data-ttu-id="bce33-107">På vägen du lär dig mer om beroende hierarkin att Resource Manager-distributionsmodellen ger dig och starta om hur mycket den innehåller.</span><span class="sxs-lookup"><span data-stu-id="bce33-107">Along the way, you learn about the dependency hierarchy that the Resource Manager deployment model gives you, and about how much power it provides.</span></span> <span data-ttu-id="bce33-108">När du ser hur systemet byggs, kan du återskapa den mycket snabbare med hjälp av [Azure Resource Manager-mallar](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bce33-108">After you see how the system is built, you can rebuild it much more quickly by using [Azure Resource Manager templates](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="bce33-109">När du lär dig hur delarna av din miljö fungerar ihop, blir hur du skapar mallar för att automatisera dem också enklare.</span><span class="sxs-lookup"><span data-stu-id="bce33-109">Also, after you learn how the parts of your environment fit together, creating templates to automate them becomes easier.</span></span>

<span data-ttu-id="bce33-110">Miljön innehåller:</span><span class="sxs-lookup"><span data-stu-id="bce33-110">The environment contains:</span></span>

* <span data-ttu-id="bce33-111">Två virtuella datorer i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="bce33-111">Two VMs inside an availability set.</span></span>
* <span data-ttu-id="bce33-112">En belastningsutjämnare med en regel för belastningsutjämning på port 80.</span><span class="sxs-lookup"><span data-stu-id="bce33-112">A load balancer with a load-balancing rule on port 80.</span></span>
* <span data-ttu-id="bce33-113">Nätverks-regler för nätverkssäkerhetsgrupper (NSG) till att skydda den virtuella datorn från oönskad trafik.</span><span class="sxs-lookup"><span data-stu-id="bce33-113">Network security group (NSG) rules to protect your VM from unwanted traffic.</span></span>

<span data-ttu-id="bce33-114">Om du vill skapa den här anpassade miljön, måste senast [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) i Resource Manager-läget (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="bce33-114">To create this custom environment, you need the latest [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) in Resource Manager mode (`azure config mode arm`).</span></span> <span data-ttu-id="bce33-115">Du måste också en JSON-parsning verktyget.</span><span class="sxs-lookup"><span data-stu-id="bce33-115">You also need a JSON parsing tool.</span></span> <span data-ttu-id="bce33-116">Det här exemplet används [jq](https://stedolan.github.io/jq/).</span><span class="sxs-lookup"><span data-stu-id="bce33-116">This example uses [jq](https://stedolan.github.io/jq/).</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="bce33-117">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="bce33-117">CLI versions to complete the task</span></span>
<span data-ttu-id="bce33-118">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="bce33-118">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="bce33-119">[Azure CLI 1.0](#quick-commands) – våra CLI för klassisk och resurs management på distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="bce33-119">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="bce33-120">[Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="bce33-120">[Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="bce33-121">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="bce33-121">Quick commands</span></span>
<span data-ttu-id="bce33-122">Om du behöver utföra aktiviteten följande avsnittet beskriver grundläggande kommandon för att överföra en virtuell dator till Azure.</span><span class="sxs-lookup"><span data-stu-id="bce33-122">If you need to quickly accomplish the task, the following section details the base commands to upload a VM to Azure.</span></span> <span data-ttu-id="bce33-123">Mer detaljerad information och kontext för varje steg i resten av dokumentet, med början [här](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="bce33-123">More detailed information and context for each step can be found in the rest of the document, starting [here](#detailed-walkthrough).</span></span>

<span data-ttu-id="bce33-124">Se till att du har [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) loggas och med hjälp av Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="bce33-124">Make sure that you have [the Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="bce33-125">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="bce33-125">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="bce33-126">Exempel parameternamn inkluderar `myResourceGroup`, `mystorageaccount`, och `myVM`.</span><span class="sxs-lookup"><span data-stu-id="bce33-126">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>

<span data-ttu-id="bce33-127">Skapa resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="bce33-127">Create the resource group.</span></span> <span data-ttu-id="bce33-128">I följande exempel skapas en resursgrupp med namnet `myResourceGroup` i den `westeurope` plats:</span><span class="sxs-lookup"><span data-stu-id="bce33-128">The following example creates a resource group named `myResourceGroup` in the `westeurope` location:</span></span>

```azurecli
azure group create -n myResourceGroup -l westeurope
```

<span data-ttu-id="bce33-129">Kontrollera resursgruppen med hjälp av JSON-parsern:</span><span class="sxs-lookup"><span data-stu-id="bce33-129">Verify the resource group by using the JSON parser:</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="bce33-130">Skapa lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="bce33-130">Create the storage account.</span></span> <span data-ttu-id="bce33-131">I följande exempel skapas ett lagringskonto med namnet `mystorageaccount`.</span><span class="sxs-lookup"><span data-stu-id="bce33-131">The following example creates a storage account named `mystorageaccount`.</span></span> <span data-ttu-id="bce33-132">(Lagringskontonamn måste vara unikt, så ange dina egna unika namn)</span><span class="sxs-lookup"><span data-stu-id="bce33-132">(The storage account name must be unique, so provide your own unique name.)</span></span>

```azurecli
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

<span data-ttu-id="bce33-133">Kontrollera storage-konto med hjälp av JSON-parsern:</span><span class="sxs-lookup"><span data-stu-id="bce33-133">Verify the storage account by using the JSON parser:</span></span>

```azurecli
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

<span data-ttu-id="bce33-134">Skapa det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="bce33-134">Create the virtual network.</span></span> <span data-ttu-id="bce33-135">I följande exempel skapas ett virtuellt nätverk med namnet `myVnet`:</span><span class="sxs-lookup"><span data-stu-id="bce33-135">The following example creates a virtual network named `myVnet`:</span></span>

```azurecli
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

<span data-ttu-id="bce33-136">Skapa ett undernät.</span><span class="sxs-lookup"><span data-stu-id="bce33-136">Create a subnet.</span></span> <span data-ttu-id="bce33-137">I följande exempel skapas ett undernät med namnet `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="bce33-137">The following example creates a subnet named `mySubnet`:</span></span>

```azurecli
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

<span data-ttu-id="bce33-138">Kontrollera det virtuella nätverk och undernät med hjälp av JSON-parsern:</span><span class="sxs-lookup"><span data-stu-id="bce33-138">Verify the virtual network and subnet by using the JSON parser:</span></span>

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

<span data-ttu-id="bce33-139">Skapa en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="bce33-139">Create a public IP.</span></span> <span data-ttu-id="bce33-140">I följande exempel skapas en offentlig IP-adress med namnet `myPublicIP` med DNS-namnet på `mypublicdns`.</span><span class="sxs-lookup"><span data-stu-id="bce33-140">The following example creates a public IP named `myPublicIP` with the DNS name of `mypublicdns`.</span></span> <span data-ttu-id="bce33-141">(DNS-namnet måste vara unikt, så ange dina egna unika namn)</span><span class="sxs-lookup"><span data-stu-id="bce33-141">(The DNS name must be unique, so provide your own unique name.)</span></span>

```azurecli
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

<span data-ttu-id="bce33-142">Skapa belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="bce33-142">Create the load balancer.</span></span> <span data-ttu-id="bce33-143">I följande exempel skapas en belastningsutjämnare med namnet `myLoadBalancer`:</span><span class="sxs-lookup"><span data-stu-id="bce33-143">The following example creates a load balancer named `myLoadBalancer`:</span></span>

```azurecli
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

<span data-ttu-id="bce33-144">Skapa en frontend IP-adresspool för belastningsutjämnaren och associera den offentliga IP-Adressen.</span><span class="sxs-lookup"><span data-stu-id="bce33-144">Create a front-end IP pool for the load balancer, and associate the public IP.</span></span> <span data-ttu-id="bce33-145">I följande exempel skapas en frontend IP-pool med namnet `mySubnetPool`:</span><span class="sxs-lookup"><span data-stu-id="bce33-145">The following example creates a front-end IP pool named `mySubnetPool`:</span></span>

```azurecli
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool
```

<span data-ttu-id="bce33-146">Skapa backend-IP-adresspool för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="bce33-146">Create the back-end IP pool for the load balancer.</span></span> <span data-ttu-id="bce33-147">I följande exempel skapas en backend-IP-pool med namnet `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="bce33-147">The following example creates a back-end IP pool named `myBackEndPool`:</span></span>

```azurecli
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

<span data-ttu-id="bce33-148">Skapa SSH inkommande network adress translation (NAT) regler för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="bce33-148">Create SSH inbound network address translation (NAT) rules for the load balancer.</span></span> <span data-ttu-id="bce33-149">I följande exempel skapas två regler för inläsning av belastningsutjämnare, `myLoadBalancerRuleSSH1` och `myLoadBalancerRuleSSH2`:</span><span class="sxs-lookup"><span data-stu-id="bce33-149">The following example creates two load balancer rules, `myLoadBalancerRuleSSH1` and `myLoadBalancerRuleSSH2`:</span></span>

```azurecli
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

<span data-ttu-id="bce33-150">Skapa webben inkommande NAT-regler för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="bce33-150">Create the web inbound NAT rules for the load balancer.</span></span> <span data-ttu-id="bce33-151">I följande exempel skapas en regel för belastningsutjämnare med namnet `myLoadBalancerRuleWeb`:</span><span class="sxs-lookup"><span data-stu-id="bce33-151">The following example creates a load balancer rule named `myLoadBalancerRuleWeb`:</span></span>

```azurecli
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

<span data-ttu-id="bce33-152">Skapa den belastningsutjämnaren, hälsoavsökningen.</span><span class="sxs-lookup"><span data-stu-id="bce33-152">Create the load balancer health probe.</span></span> <span data-ttu-id="bce33-153">I följande exempel skapas en TCP-avsökning med namnet `myHealthProbe`:</span><span class="sxs-lookup"><span data-stu-id="bce33-153">The following example creates a TCP probe named `myHealthProbe`:</span></span>

```azurecli
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

<span data-ttu-id="bce33-154">Kontrollera belastningsutjämnare, IP-adresspooler och NAT-regler med hjälp av JSON-parsern:</span><span class="sxs-lookup"><span data-stu-id="bce33-154">Verify the load balancer, IP pools, and NAT rules by using the JSON parser:</span></span>

```azurecli
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

<span data-ttu-id="bce33-155">Skapa första network interface card (NIC).</span><span class="sxs-lookup"><span data-stu-id="bce33-155">Create the first network interface card (NIC).</span></span> <span data-ttu-id="bce33-156">Ersätt den `#####-###-###` avsnitt med din egen Azure prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="bce33-156">Replace the `#####-###-###` sections with your own Azure subscription ID.</span></span> <span data-ttu-id="bce33-157">Din prenumeration ID anges i resultatet av **jq** när du undersöker de resurser som du skapar.</span><span class="sxs-lookup"><span data-stu-id="bce33-157">Your subscription ID is noted in the output of **jq** when you examine the resources you are creating.</span></span> <span data-ttu-id="bce33-158">Du kan också visa ditt prenumerations-ID med `azure account list`.</span><span class="sxs-lookup"><span data-stu-id="bce33-158">You can also view your subscription ID with `azure account list`.</span></span>

<span data-ttu-id="bce33-159">I följande exempel skapas ett nätverkskort med namnet `myNic1`:</span><span class="sxs-lookup"><span data-stu-id="bce33-159">The following example creates a NIC named `myNic1`:</span></span>

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

<span data-ttu-id="bce33-160">Skapa det andra nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="bce33-160">Create the second NIC.</span></span> <span data-ttu-id="bce33-161">I följande exempel skapas ett nätverkskort med namnet `myNic2`:</span><span class="sxs-lookup"><span data-stu-id="bce33-161">The following example creates a NIC named `myNic2`:</span></span>

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

<span data-ttu-id="bce33-162">Kontrollera de två nätverkskorten med hjälp av JSON-parsern:</span><span class="sxs-lookup"><span data-stu-id="bce33-162">Verify the two NICs by using the JSON parser:</span></span>

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

<span data-ttu-id="bce33-163">Skapa nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="bce33-163">Create the network security group.</span></span> <span data-ttu-id="bce33-164">I följande exempel skapas en nätverkssäkerhetsgrupp som heter `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="bce33-164">The following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

<span data-ttu-id="bce33-165">Lägga till två regler för inkommande trafik för nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="bce33-165">Add two inbound rules for the network security group.</span></span> <span data-ttu-id="bce33-166">I följande exempel skapas två regler `myNetworkSecurityGroupRuleSSH` och `myNetworkSecurityGroupRuleHTTP`:</span><span class="sxs-lookup"><span data-stu-id="bce33-166">The following example creates two rules, `myNetworkSecurityGroupRuleSSH` and `myNetworkSecurityGroupRuleHTTP`:</span></span>

```azurecli
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

<span data-ttu-id="bce33-167">Kontrollera nätverkssäkerhetsgruppen och regler för inkommande trafik med hjälp av JSON-parsern:</span><span class="sxs-lookup"><span data-stu-id="bce33-167">Verify the network security group and inbound rules by using the JSON parser:</span></span>

```azurecli
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

<span data-ttu-id="bce33-168">Binda nätverkssäkerhetsgruppen till två nätverkskort:</span><span class="sxs-lookup"><span data-stu-id="bce33-168">Bind the network security group to the two NICs:</span></span>

```azurecli
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

<span data-ttu-id="bce33-169">Skapa tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="bce33-169">Create the availability set.</span></span> <span data-ttu-id="bce33-170">I följande exempel skapas en tillgänglighetsuppsättning namngivna `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="bce33-170">The following example creates an availability set named `myAvailabilitySet`:</span></span>

```azurecli
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

<span data-ttu-id="bce33-171">Skapa första Linux VM.</span><span class="sxs-lookup"><span data-stu-id="bce33-171">Create the first Linux VM.</span></span> <span data-ttu-id="bce33-172">I följande exempel skapas en virtuell dator med namnet `myVM1`:</span><span class="sxs-lookup"><span data-stu-id="bce33-172">The following example creates a VM named `myVM1`:</span></span>

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

<span data-ttu-id="bce33-173">Skapa andra Linux VM.</span><span class="sxs-lookup"><span data-stu-id="bce33-173">Create the second Linux VM.</span></span> <span data-ttu-id="bce33-174">I följande exempel skapas en virtuell dator med namnet `myVM2`:</span><span class="sxs-lookup"><span data-stu-id="bce33-174">The following example creates a VM named `myVM2`:</span></span>

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

<span data-ttu-id="bce33-175">Använda JSON-parsern för att kontrollera att allt som har skapats:</span><span class="sxs-lookup"><span data-stu-id="bce33-175">Use the JSON parser to verify that everything that was built:</span></span>

```azurecli
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

<span data-ttu-id="bce33-176">Exportera den nya miljön till en mall för att snabbt skapa nya instanser:</span><span class="sxs-lookup"><span data-stu-id="bce33-176">Export your new environment to a template to quickly re-create new instances:</span></span>

```azurecli
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="bce33-177">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="bce33-177">Detailed walkthrough</span></span>
<span data-ttu-id="bce33-178">Detaljerade anvisningar som följer beskrivs vad varje kommando gör när du skapar i din miljö.</span><span class="sxs-lookup"><span data-stu-id="bce33-178">The detailed steps that follow explain what each command is doing as you build out your environment.</span></span> <span data-ttu-id="bce33-179">De här koncepten är användbara när du skapar dina egna anpassade miljöer för utveckling och produktion.</span><span class="sxs-lookup"><span data-stu-id="bce33-179">These concepts are helpful when you build your own custom environments for development or production.</span></span>

<span data-ttu-id="bce33-180">Se till att du har [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) loggas och med hjälp av Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="bce33-180">Make sure that you have [the Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="bce33-181">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="bce33-181">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="bce33-182">Exempel parameternamn inkluderar `myResourceGroup`, `mystorageaccount`, och `myVM`.</span><span class="sxs-lookup"><span data-stu-id="bce33-182">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>

## <a name="create-resource-groups-and-choose-deployment-locations"></a><span data-ttu-id="bce33-183">Skapa resursgrupper och välj distributionen platser</span><span class="sxs-lookup"><span data-stu-id="bce33-183">Create resource groups and choose deployment locations</span></span>
<span data-ttu-id="bce33-184">Azure-resursgrupper är distribution av logiska enheter som innehåller information om konfiguration och metadata för att aktivera logiska hantering av resurs-distributioner.</span><span class="sxs-lookup"><span data-stu-id="bce33-184">Azure resource groups are logical deployment entities that contain configuration information and metadata to enable the logical management of resource deployments.</span></span> <span data-ttu-id="bce33-185">I följande exempel skapas en resursgrupp med namnet `myResourceGroup` i den `westeurope` plats:</span><span class="sxs-lookup"><span data-stu-id="bce33-185">The following example creates a resource group named `myResourceGroup` in the `westeurope` location:</span></span>

```azurecli
azure group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="bce33-186">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-186">Output:</span></span>

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

## <a name="create-a-storage-account"></a><span data-ttu-id="bce33-187">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="bce33-187">Create a storage account</span></span>
<span data-ttu-id="bce33-188">Du måste storage-konton för VM-diskar och eventuella ytterligare hårddiskar som du vill lägga till.</span><span class="sxs-lookup"><span data-stu-id="bce33-188">You need storage accounts for your VM disks and for any additional data disks that you want to add.</span></span> <span data-ttu-id="bce33-189">Du kan skapa storage-konton nästan omedelbart när du skapar resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="bce33-189">You create storage accounts almost immediately after you create resource groups.</span></span>

<span data-ttu-id="bce33-190">Här exemplet använder vi den `azure storage account create` kommandot skickar platsen för kontot, resursgruppen som styr och typ av Lagringsstöd som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="bce33-190">Here we use the `azure storage account create` command, passing the location of the account, the resource group that controls it, and the type of storage support you want.</span></span> <span data-ttu-id="bce33-191">I följande exempel skapas ett lagringskonto med namnet `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="bce33-191">The following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

<span data-ttu-id="bce33-192">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-192">Output:</span></span>

```azurecli
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

<span data-ttu-id="bce33-193">För att granska våra resursgrupp med hjälp av den `azure group show` kommando använder vi den [jq](https://stedolan.github.io/jq/) verktyget tillsammans med den `--json` Azure CLI-alternativet.</span><span class="sxs-lookup"><span data-stu-id="bce33-193">To examine our resource group by using the `azure group show` command, let's use the [jq](https://stedolan.github.io/jq/) tool along with the `--json` Azure CLI option.</span></span> <span data-ttu-id="bce33-194">(Du kan använda **jsawk** eller något språk bibliotek som du föredrar att parsa JSON.)</span><span class="sxs-lookup"><span data-stu-id="bce33-194">(You can use **jsawk** or any language library you prefer to parse the JSON.)</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="bce33-195">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-195">Output:</span></span>

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

<span data-ttu-id="bce33-196">För att undersöka storage-konto med hjälp av CLI, måste du först ange kontonamn och nycklar.</span><span class="sxs-lookup"><span data-stu-id="bce33-196">To investigate the storage account by using the CLI, you first need to set the account names and keys.</span></span> <span data-ttu-id="bce33-197">Ersätt namnet på lagringskontot i följande exempel med ett namn som du väljer:</span><span class="sxs-lookup"><span data-stu-id="bce33-197">Replace the name of the storage account in the following example with a name that you choose:</span></span>

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

<span data-ttu-id="bce33-198">Du kan sedan visa storage-informationen enkelt:</span><span class="sxs-lookup"><span data-stu-id="bce33-198">Then you can view your storage information easily:</span></span>

```azurecli
azure storage container list
```

<span data-ttu-id="bce33-199">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-199">Output:</span></span>

```azurecli
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a><span data-ttu-id="bce33-200">Skapa ett virtuellt nätverk och undernät</span><span class="sxs-lookup"><span data-stu-id="bce33-200">Create a virtual network and subnet</span></span>
<span data-ttu-id="bce33-201">Härnäst ska du behöver skapa ett virtuellt nätverk som körs i Azure och ett undernät som du kan skapa dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="bce33-201">Next you're going to need to create a virtual network running in Azure and a subnet in which you can create your VMs.</span></span> <span data-ttu-id="bce33-202">I följande exempel skapas ett virtuellt nätverk med namnet `myVnet` med den `192.168.0.0/16` adressprefix:</span><span class="sxs-lookup"><span data-stu-id="bce33-202">The following example creates a virtual network named `myVnet` with the `192.168.0.0/16` address prefix:</span></span>

```azurecli
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16
```

<span data-ttu-id="bce33-203">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-203">Output:</span></span>

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

<span data-ttu-id="bce33-204">Igen och Använd alternativet--json i vi `azure group show` och `jq` att se hur vi bygger våra resurser.</span><span class="sxs-lookup"><span data-stu-id="bce33-204">Again, let's use the --json option of `azure group show` and `jq` to see how we're building our resources.</span></span> <span data-ttu-id="bce33-205">Nu har vi en `storageAccounts` resurs och ett `virtualNetworks` resurs.</span><span class="sxs-lookup"><span data-stu-id="bce33-205">We now have a `storageAccounts` resource and a `virtualNetworks` resource.</span></span>  

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="bce33-206">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-206">Output:</span></span>

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

<span data-ttu-id="bce33-207">Nu skapar vi ett undernät i det `myVnet` virtuella nätverk som de virtuella datorerna har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="bce33-207">Now let's create a subnet in the `myVnet` virtual network into which the VMs are deployed.</span></span> <span data-ttu-id="bce33-208">Vi använder den `azure network vnet subnet create` kommandot tillsammans med de resurser som redan har vi skapat: den `myResourceGroup` resursgrupp och `myVnet` virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="bce33-208">We use the `azure network vnet subnet create` command, along with the resources we've already created: the `myResourceGroup` resource group and the `myVnet` virtual network.</span></span> <span data-ttu-id="bce33-209">I följande exempel tar vi lägga till undernätet med namnet `mySubnet` med det undernät adressprefixet `192.168.1.0/24`:</span><span class="sxs-lookup"><span data-stu-id="bce33-209">In the following example, we add the subnet named `mySubnet` with the subnet address prefix of `192.168.1.0/24`:</span></span>

```azurecli
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

<span data-ttu-id="bce33-210">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-210">Output:</span></span>

```azurecli
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

<span data-ttu-id="bce33-211">Eftersom undernätet är logiskt i det virtuella nätverket, leta vi efter information om undernät med ett något annorlunda kommando.</span><span class="sxs-lookup"><span data-stu-id="bce33-211">Because the subnet is logically inside the virtual network, we look for the subnet information with a slightly different command.</span></span> <span data-ttu-id="bce33-212">Vi använder kommandot är `azure network vnet show`, men vi fortsätta att använda för att undersöka JSON-utdata `jq`.</span><span class="sxs-lookup"><span data-stu-id="bce33-212">The command we use is `azure network vnet show`, but we continue to examine the JSON output by using `jq`.</span></span>

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

<span data-ttu-id="bce33-213">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-213">Output:</span></span>

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

## <a name="create-a-public-ip-address"></a><span data-ttu-id="bce33-214">Skapa en offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="bce33-214">Create a public IP address</span></span>
<span data-ttu-id="bce33-215">Nu ska vi skapa offentlig IP-adress (PIP) som vi tilldela din belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="bce33-215">Now let's create the public IP address (PIP) that we assign to your load balancer.</span></span> <span data-ttu-id="bce33-216">Du kan ansluta till dina virtuella datorer från Internet med hjälp av `azure network public-ip create` kommando.</span><span class="sxs-lookup"><span data-stu-id="bce33-216">It enables you to connect to your VMs from the Internet by using the `azure network public-ip create` command.</span></span> <span data-ttu-id="bce33-217">Eftersom standardadressen är dynamisk kan vi skapa en namngiven DNS-post i den **cloudapp.azure.com** domänen med hjälp av den `--domain-name-label` alternativet.</span><span class="sxs-lookup"><span data-stu-id="bce33-217">Because the default address is dynamic, we create a named DNS entry in the **cloudapp.azure.com** domain by using the `--domain-name-label` option.</span></span> <span data-ttu-id="bce33-218">I följande exempel skapas en offentlig IP-adress med namnet `myPublicIP` med DNS-namnet på `mypublicdns`.</span><span class="sxs-lookup"><span data-stu-id="bce33-218">The following example creates a public IP named `myPublicIP` with the DNS name of `mypublicdns`.</span></span> <span data-ttu-id="bce33-219">Eftersom DNS-namnet måste vara unikt, kan du ange egna unikt DNS-namn:</span><span class="sxs-lookup"><span data-stu-id="bce33-219">Because the DNS name must be unique, you provide your own unique DNS name:</span></span>

```azurecli
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

<span data-ttu-id="bce33-220">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-220">Output:</span></span>

```azurecli
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
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

<span data-ttu-id="bce33-221">Den offentliga IP-adressen är också en översta resurs så att du kan se den med `azure group show`.</span><span class="sxs-lookup"><span data-stu-id="bce33-221">The public IP address is also a top-level resource, so you can see it with `azure group show`.</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="bce33-222">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-222">Output:</span></span>

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

<span data-ttu-id="bce33-223">Du kan undersöka mer resursinformation,, inklusive det fullständigt kvalificerade domännamnet (FQDN) för underdomänen, med hjälp av det fullständiga `azure network public-ip show` kommando.</span><span class="sxs-lookup"><span data-stu-id="bce33-223">You can investigate more resource details, including the fully qualified domain name (FQDN) of the subdomain, by using the complete `azure network public-ip show` command.</span></span> <span data-ttu-id="bce33-224">Den offentliga IP-adressresursen har allokerats logiskt, men en specifik adress har ännu inte tilldelats.</span><span class="sxs-lookup"><span data-stu-id="bce33-224">The public IP address resource has been allocated logically, but a specific address has not yet been assigned.</span></span> <span data-ttu-id="bce33-225">För att erhålla en IP-adress kommer du att behöva en belastningsutjämnare som vi ännu inte har skapats.</span><span class="sxs-lookup"><span data-stu-id="bce33-225">To obtain an IP address, you're going to need a load balancer, which we have not yet created.</span></span>

```azurecli
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

<span data-ttu-id="bce33-226">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-226">Output:</span></span>

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

## <a name="create-a-load-balancer-and-ip-pools"></a><span data-ttu-id="bce33-227">Skapa en belastningsutjämnare och IP-pooler</span><span class="sxs-lookup"><span data-stu-id="bce33-227">Create a load balancer and IP pools</span></span>
<span data-ttu-id="bce33-228">När du skapar en belastningsutjämnare, kan du distribuera trafik mellan flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="bce33-228">When you create a load balancer, it enables you to distribute traffic across multiple VMs.</span></span> <span data-ttu-id="bce33-229">Det ger också redundans i tillämpningsprogrammet genom att köra flera virtuella datorer som svarar på begäranden från användare vid underhåll eller tunga laster.</span><span class="sxs-lookup"><span data-stu-id="bce33-229">It also provides redundancy to your application by running multiple VMs that respond to user requests in the event of maintenance or heavy loads.</span></span> <span data-ttu-id="bce33-230">I följande exempel skapas en belastningsutjämnare med namnet `myLoadBalancer`:</span><span class="sxs-lookup"><span data-stu-id="bce33-230">The following example creates a load balancer named `myLoadBalancer`:</span></span>

```azurecli
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

<span data-ttu-id="bce33-231">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-231">Output:</span></span>

```azurecli
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

<span data-ttu-id="bce33-232">Vår belastningsutjämnare är ganska tomt, så vi skapa vissa IP-adresspooler.</span><span class="sxs-lookup"><span data-stu-id="bce33-232">Our load balancer is fairly empty, so let's create some IP pools.</span></span> <span data-ttu-id="bce33-233">Vi vill skapa två IP-adresspooler för våra belastningsutjämnare, en för klientdelen och en för serverdelen.</span><span class="sxs-lookup"><span data-stu-id="bce33-233">We want to create two IP pools for our load balancer, one for the front end and one for the back end.</span></span> <span data-ttu-id="bce33-234">Frontend IP-adresspool är synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="bce33-234">The front-end IP pool is publicly visible.</span></span> <span data-ttu-id="bce33-235">Det är också den plats som vi tilldela PIP som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="bce33-235">It's also the location to which we assign the PIP that we created earlier.</span></span> <span data-ttu-id="bce33-236">Sedan använder vi backend-poolen som en plats för våra virtuella datorer för att ansluta till.</span><span class="sxs-lookup"><span data-stu-id="bce33-236">Then we use the back-end pool as a location for our VMs to connect to.</span></span> <span data-ttu-id="bce33-237">På så sätt kan trafiken kan flöda till belastningsutjämnaren till de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="bce33-237">That way, the traffic can flow through the load balancer to the VMs.</span></span>

<span data-ttu-id="bce33-238">Först skapar vi vårt frontend IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="bce33-238">First, let's create our front-end IP pool.</span></span> <span data-ttu-id="bce33-239">I följande exempel skapas en frontend-pool med namnet `myFrontEndPool`:</span><span class="sxs-lookup"><span data-stu-id="bce33-239">The following example creates a front-end pool named `myFrontEndPool`:</span></span>

```azurecli
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool
```

<span data-ttu-id="bce33-240">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-240">Output:</span></span>

```azurecli
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

<span data-ttu-id="bce33-241">Observera hur vi använde den `--public-ip-name` växel för att skicka in den `myPublicIP` som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="bce33-241">Note how we used the `--public-ip-name` switch to pass in the `myPublicIP` that we created earlier.</span></span> <span data-ttu-id="bce33-242">Tilldela den offentliga IP-adressen till belastningsutjämnaren kan du nå din virtuella dator via Internet.</span><span class="sxs-lookup"><span data-stu-id="bce33-242">Assigning the public IP address to the load balancer allows you to reach your VMs across the Internet.</span></span>

<span data-ttu-id="bce33-243">Nu ska vi skapa våra andra IP-adresspool nu för våra backend-trafik.</span><span class="sxs-lookup"><span data-stu-id="bce33-243">Next, let's create our second IP pool, this time for our back-end traffic.</span></span> <span data-ttu-id="bce33-244">I följande exempel skapas en backend-pool med namnet `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="bce33-244">The following example creates a back-end pool named `myBackEndPool`:</span></span>

```azurecli
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

<span data-ttu-id="bce33-245">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-245">Output:</span></span>

```azurecli
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

<span data-ttu-id="bce33-246">Vi kan se hur våra belastningsutjämnaren fungerar genom att titta med `azure network lb show` och undersöka JSON-utdata:</span><span class="sxs-lookup"><span data-stu-id="bce33-246">We can see how our load balancer is doing by looking with `azure network lb show` and examining the JSON output:</span></span>

```azurecli
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

<span data-ttu-id="bce33-247">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-247">Output:</span></span>

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

## <a name="create-load-balancer-nat-rules"></a><span data-ttu-id="bce33-248">Skapa belastningsutjämnaren NAT-regler</span><span class="sxs-lookup"><span data-stu-id="bce33-248">Create load balancer NAT rules</span></span>
<span data-ttu-id="bce33-249">Vi behöver skapa network address translation (NAT) regler som anger inkommande eller utgående åtgärder för att få trafik som passerar genom vår belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="bce33-249">To get traffic flowing through our load balancer, we need to create network address translation (NAT) rules that specify either inbound or outbound actions.</span></span> <span data-ttu-id="bce33-250">Du kan ange protokoll och sedan mappa externa portar till interna portar som du vill.</span><span class="sxs-lookup"><span data-stu-id="bce33-250">You can specify the protocol to use, then map external ports to internal ports as desired.</span></span> <span data-ttu-id="bce33-251">Nu ska vi skapa vissa regler som tillåter SSH via vårt belastningsutjämnaren till vår virtuella datorer för våra miljö.</span><span class="sxs-lookup"><span data-stu-id="bce33-251">For our environment, let's create some rules that allow SSH through our load balancer to our VMs.</span></span> <span data-ttu-id="bce33-252">Vi konfigurerar TCP-portarna 4222 och 4223 att dirigera till TCP-port 22 på vår virtuella datorer (som vi skapa senare).</span><span class="sxs-lookup"><span data-stu-id="bce33-252">We set up TCP ports 4222 and 4223 to direct to TCP port 22 on our VMs (which we create later).</span></span> <span data-ttu-id="bce33-253">I följande exempel skapas en regel med namnet `myLoadBalancerRuleSSH1` att mappa TCP-port 4222 port 22:</span><span class="sxs-lookup"><span data-stu-id="bce33-253">The following example creates a rule named `myLoadBalancerRuleSSH1` to map TCP port 4222 to port 22:</span></span>

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

<span data-ttu-id="bce33-254">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-254">Output:</span></span>

```azurecli
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
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

<span data-ttu-id="bce33-255">Upprepa proceduren för andra NAT-regeln för SSH.</span><span class="sxs-lookup"><span data-stu-id="bce33-255">Repeat the procedure for your second NAT rule for SSH.</span></span> <span data-ttu-id="bce33-256">I följande exempel skapas en regel med namnet `myLoadBalancerRuleSSH2` att mappa TCP-port 4223 port 22:</span><span class="sxs-lookup"><span data-stu-id="bce33-256">The following example creates a rule named `myLoadBalancerRuleSSH2` to map TCP port 4223 to port 22:</span></span>

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

<span data-ttu-id="bce33-257">Nu ska vi också gå vidare och skapa en NAT-regel för TCP-port 80 för webbtrafik, koppla upp regeln upp till vår IP-adresspooler.</span><span class="sxs-lookup"><span data-stu-id="bce33-257">Let's also go ahead and create a NAT rule for TCP port 80 for web traffic, hooking the rule up to our IP pools.</span></span> <span data-ttu-id="bce33-258">Om vi koppla en regel till en IP-adresspool, i stället för att koppla upp en regel till vår virtuella datorer separat, vi lägga till eller ta bort virtuella datorer från IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="bce33-258">If we hook up the rule to an IP pool, instead of hooking up the rule to our VMs individually, we can add or remove VMs from the IP pool.</span></span> <span data-ttu-id="bce33-259">Belastningsutjämnaren justeras automatiskt trafikflödet.</span><span class="sxs-lookup"><span data-stu-id="bce33-259">The load balancer automatically adjusts the flow of traffic.</span></span> <span data-ttu-id="bce33-260">I följande exempel skapas en regel med namnet `myLoadBalancerRuleWeb` att mappa TCP-port 80 till port 80:</span><span class="sxs-lookup"><span data-stu-id="bce33-260">The following example creates a rule named `myLoadBalancerRuleWeb` to map TCP port 80 to port 80:</span></span>

```azurecli
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

<span data-ttu-id="bce33-261">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-261">Output:</span></span>

```azurecli
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
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

## <a name="create-a-load-balancer-health-probe"></a><span data-ttu-id="bce33-262">Skapa en belastningsutjämnaren, hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="bce33-262">Create a load balancer health probe</span></span>
<span data-ttu-id="bce33-263">En avsökning med jämna mellanrum kontroller på virtuella datorer som är bakom våra belastningsutjämnare för att kontrollera att de fungerar och svarar på förfrågningar som har definierats.</span><span class="sxs-lookup"><span data-stu-id="bce33-263">A health probe periodically checks on the VMs that are behind our load balancer to make sure they're operating and responding to requests as defined.</span></span> <span data-ttu-id="bce33-264">Om inte, de tas bort från igen så att användare inte dirigeras till dem.</span><span class="sxs-lookup"><span data-stu-id="bce33-264">If not, they're removed from operation to ensure that users aren't being directed to them.</span></span> <span data-ttu-id="bce33-265">Du kan definiera egna kontroller för hälsoavsökningen tillsammans med intervall och timeout-värden.</span><span class="sxs-lookup"><span data-stu-id="bce33-265">You can define custom checks for the health probe, along with intervals and timeout values.</span></span> <span data-ttu-id="bce33-266">Läs mer om hälsoavsökningar [avsökningar belastningsutjämnaren](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bce33-266">For more information about health probes, see [Load Balancer probes](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="bce33-267">I följande exempel skapas en TCP hälsa avsöktes namngivna `myHealthProbe`:</span><span class="sxs-lookup"><span data-stu-id="bce33-267">The following example creates a TCP health probed named `myHealthProbe`:</span></span>

```azurecli
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

<span data-ttu-id="bce33-268">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-268">Output:</span></span>

```azurecli
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

<span data-ttu-id="bce33-269">Vi anges här, ett intervall på 15 sekunder för våra hälsokontroller.</span><span class="sxs-lookup"><span data-stu-id="bce33-269">Here, we specified an interval of 15 seconds for our health checks.</span></span> <span data-ttu-id="bce33-270">Vi kan missa högst fyra avsökningar (en minut) innan belastningsutjämnaren anser att värden inte längre fungerar.</span><span class="sxs-lookup"><span data-stu-id="bce33-270">We can miss a maximum of four probes (one minute) before the load balancer considers that the host is no longer functioning.</span></span>

## <a name="verify-the-load-balancer"></a><span data-ttu-id="bce33-271">Kontrollera belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="bce33-271">Verify the load balancer</span></span>
<span data-ttu-id="bce33-272">Belastningsutjämningskonfigurationen är nu klar.</span><span class="sxs-lookup"><span data-stu-id="bce33-272">Now the load balancer configuration is done.</span></span> <span data-ttu-id="bce33-273">Här följer de steg som du tog:</span><span class="sxs-lookup"><span data-stu-id="bce33-273">Here are the steps you took:</span></span>

1. <span data-ttu-id="bce33-274">Du har skapat en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="bce33-274">You created a load balancer.</span></span>
2. <span data-ttu-id="bce33-275">Du har skapat en frontend IP-adresspool och tilldelats en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="bce33-275">You created a front-end IP pool and assigned a public IP to it.</span></span>
3. <span data-ttu-id="bce33-276">Du har skapat en backend-IP-adresspool som virtuella datorer kan ansluta till.</span><span class="sxs-lookup"><span data-stu-id="bce33-276">You created a back-end IP pool that VMs can connect to.</span></span>
4. <span data-ttu-id="bce33-277">Du har skapat NAT-regler som tillåter SSH till VM: ar för hantering, tillsammans med en regel som tillåter TCP-port 80 för våra webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="bce33-277">You created NAT rules that allow SSH to the VMs for management, along with a rule that allows TCP port 80 for our web app.</span></span>
5. <span data-ttu-id="bce33-278">Du har lagt till en hälsoavsökningen för att regelbundet kontrollera de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="bce33-278">You added a health probe to periodically check the VMs.</span></span> <span data-ttu-id="bce33-279">Den här hälsoavsökningen säkerställer att användare inte försöker komma åt en virtuell dator som inte längre fungerar eller betjänar innehåll.</span><span class="sxs-lookup"><span data-stu-id="bce33-279">This health probe ensures that users don't try to access a VM that is no longer functioning or serving content.</span></span>

<span data-ttu-id="bce33-280">Nu ska vi se hur din belastningsutjämnare ser ut nu:</span><span class="sxs-lookup"><span data-stu-id="bce33-280">Let's review what your load balancer looks like now:</span></span>

```azurecli
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

<span data-ttu-id="bce33-281">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-281">Output:</span></span>

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

## <a name="create-an-nic-to-use-with-the-linux-vm"></a><span data-ttu-id="bce33-282">Skapa ett nätverkskort som ska användas med Linux VM</span><span class="sxs-lookup"><span data-stu-id="bce33-282">Create an NIC to use with the Linux VM</span></span>
<span data-ttu-id="bce33-283">Nätverkskort är tillgängliga via programmering eftersom du kan använda regler för deras användning.</span><span class="sxs-lookup"><span data-stu-id="bce33-283">NICs are programmatically available because you can apply rules to their use.</span></span> <span data-ttu-id="bce33-284">Du kan också ha fler än en.</span><span class="sxs-lookup"><span data-stu-id="bce33-284">You can also have more than one.</span></span> <span data-ttu-id="bce33-285">I följande `azure network nic create` kommandot du ansluta nätverkskortet till backend-IP-adresspool för belastning och associera det med NAT-regel för att tillåta SSH-trafik.</span><span class="sxs-lookup"><span data-stu-id="bce33-285">In the following `azure network nic create` command, you hook up the NIC to the load back-end IP pool and associate it with the NAT rule to permit SSH traffic.</span></span>

<span data-ttu-id="bce33-286">Ersätt den `#####-###-###` avsnitt med din egen Azure prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="bce33-286">Replace the `#####-###-###` sections with your own Azure subscription ID.</span></span> <span data-ttu-id="bce33-287">Din prenumeration ID anges i resultatet av `jq` när du undersöker de resurser som du skapar.</span><span class="sxs-lookup"><span data-stu-id="bce33-287">Your subscription ID is noted in the output of `jq` when you examine the resources you are creating.</span></span> <span data-ttu-id="bce33-288">Du kan också visa ditt prenumerations-ID med `azure account list`.</span><span class="sxs-lookup"><span data-stu-id="bce33-288">You can also view your subscription ID with `azure account list`.</span></span>

<span data-ttu-id="bce33-289">I följande exempel skapas ett nätverkskort med namnet `myNic1`:</span><span class="sxs-lookup"><span data-stu-id="bce33-289">The following example creates a NIC named `myNic1`:</span></span>

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

<span data-ttu-id="bce33-290">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-290">Output:</span></span>

```azurecli
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
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

<span data-ttu-id="bce33-291">Du kan se information genom att undersöka resursen direkt.</span><span class="sxs-lookup"><span data-stu-id="bce33-291">You can see the details by examining the resource directly.</span></span> <span data-ttu-id="bce33-292">Du undersöka resursen med hjälp av den `azure network nic show` kommando:</span><span class="sxs-lookup"><span data-stu-id="bce33-292">You examine the resource by using the `azure network nic show` command:</span></span>

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

<span data-ttu-id="bce33-293">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-293">Output:</span></span>

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

<span data-ttu-id="bce33-294">Nu skapar vi det andra nätverkskortet kopplar i vår backend-IP-adresspool igen.</span><span class="sxs-lookup"><span data-stu-id="bce33-294">Now we create the second NIC, hooking in to our back-end IP pool again.</span></span> <span data-ttu-id="bce33-295">Den här gången andra NAT-regeln tillåter SSH-trafik.</span><span class="sxs-lookup"><span data-stu-id="bce33-295">This time the second NAT rule permits SSH traffic.</span></span> <span data-ttu-id="bce33-296">I följande exempel skapas ett nätverkskort med namnet `myNic2`:</span><span class="sxs-lookup"><span data-stu-id="bce33-296">The following example creates a NIC named `myNic2`:</span></span>

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a><span data-ttu-id="bce33-297">Skapa en säkerhetsgrupp för nätverk och regler</span><span class="sxs-lookup"><span data-stu-id="bce33-297">Create a network security group and rules</span></span>
<span data-ttu-id="bce33-298">Nu skapa vi en säkerhetsgrupp för nätverk och de inkommande reglerna som styr åtkomsten till nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="bce33-298">Now we create a network security group and the inbound rules that govern access to the NIC.</span></span> <span data-ttu-id="bce33-299">En nätverkssäkerhetsgrupp kan tillämpas på ett nätverkskort eller ett undernät.</span><span class="sxs-lookup"><span data-stu-id="bce33-299">A network security group can be applied to a NIC or subnet.</span></span> <span data-ttu-id="bce33-300">Du kan definiera regler för att styra flödet av trafik till och från dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="bce33-300">You define rules to control the flow of traffic in and out of your VMs.</span></span> <span data-ttu-id="bce33-301">I följande exempel skapas en nätverkssäkerhetsgrupp som heter `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="bce33-301">The following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

<span data-ttu-id="bce33-302">Lägg till regel för inkommande trafik för NSG: N att tillåta inkommande anslutningar på port 22 (till stöd för SSH).</span><span class="sxs-lookup"><span data-stu-id="bce33-302">Let's add the inbound rule for the NSG to allow inbound connections on port 22 (to support SSH).</span></span> <span data-ttu-id="bce33-303">I följande exempel skapas en regel med namnet `myNetworkSecurityGroupRuleSSH` att tillåta TCP på port 22:</span><span class="sxs-lookup"><span data-stu-id="bce33-303">The following example creates a rule named `myNetworkSecurityGroupRuleSSH` to allow TCP on port 22:</span></span>

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

<span data-ttu-id="bce33-304">Nu ska vi lägga till regel för inkommande trafik för NSG: N att tillåta inkommande anslutningar på port 80 (för att stödja webbtrafik).</span><span class="sxs-lookup"><span data-stu-id="bce33-304">Now let's add the inbound rule for the NSG to allow inbound connections on port 80 (to support web traffic).</span></span> <span data-ttu-id="bce33-305">I följande exempel skapas en regel med namnet `myNetworkSecurityGroupRuleHTTP` att tillåta TCP på port 80:</span><span class="sxs-lookup"><span data-stu-id="bce33-305">The following example creates a rule named `myNetworkSecurityGroupRuleHTTP` to allow TCP on port 80:</span></span>

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [!NOTE]
> <span data-ttu-id="bce33-306">Regel för inkommande trafik är ett filter för inkommande nätverksanslutningar.</span><span class="sxs-lookup"><span data-stu-id="bce33-306">The inbound rule is a filter for inbound network connections.</span></span> <span data-ttu-id="bce33-307">I det här exemplet Binder vi NSG: N till det virtuella nätverkskortet virtuella datorer, vilket innebär att alla förfrågningar till port 22 överförs via till nätverkskortet på vår VM.</span><span class="sxs-lookup"><span data-stu-id="bce33-307">In this example, we bind the NSG to the VMs virtual NIC, which means that any request to port 22 is passed through to the NIC on our VM.</span></span> <span data-ttu-id="bce33-308">Denna regel för inkommande trafik som handlar om en nätverksanslutning och inte om en slutpunkt som är det skulle ha varit om i klassiska distributioner.</span><span class="sxs-lookup"><span data-stu-id="bce33-308">This inbound rule is about a network connection, and not about an endpoint, which is what it would be about in classic deployments.</span></span> <span data-ttu-id="bce33-309">Om du vill öppna en port måste du lämna den `--source-port-range` inställd på '\*”(standardvärdet) att acceptera inkommande begäranden från **alla** begär port.</span><span class="sxs-lookup"><span data-stu-id="bce33-309">To open a port, you must leave the `--source-port-range` set to '\*' (the default value) to accept inbound requests from **any** requesting port.</span></span> <span data-ttu-id="bce33-310">Det finns vanligtvis dynamiska portar.</span><span class="sxs-lookup"><span data-stu-id="bce33-310">Ports are typically dynamic.</span></span>
>
>

## <a name="bind-to-the-nic"></a><span data-ttu-id="bce33-311">Binda till nätverkskortet</span><span class="sxs-lookup"><span data-stu-id="bce33-311">Bind to the NIC</span></span>
<span data-ttu-id="bce33-312">Binda NSG: N till nätverkskorten.</span><span class="sxs-lookup"><span data-stu-id="bce33-312">Bind the NSG to the NICs.</span></span> <span data-ttu-id="bce33-313">Vi behöver ansluta våra nätverkskort med vår nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="bce33-313">We need to connect our NICs with our network security group.</span></span> <span data-ttu-id="bce33-314">Kör både kommandon för att koppla samman både våra nätverkskort:</span><span class="sxs-lookup"><span data-stu-id="bce33-314">Run both commands, to hook up both of our NICs:</span></span>

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a><span data-ttu-id="bce33-315">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="bce33-315">Create an availability set</span></span>
<span data-ttu-id="bce33-316">Tillgänglighetsuppsättningar hjälp spridning din virtuella dator över feldomäner och uppgradera domäner.</span><span class="sxs-lookup"><span data-stu-id="bce33-316">Availability sets help spread your VMs across fault domains and upgrade domains.</span></span> <span data-ttu-id="bce33-317">Nu ska vi skapa en tillgänglighetsuppsättning för dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="bce33-317">Let's create an availability set for your VMs.</span></span> <span data-ttu-id="bce33-318">I följande exempel skapas en tillgänglighetsuppsättning namngivna `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="bce33-318">The following example creates an availability set named `myAvailabilitySet`:</span></span>

```azurecli
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

<span data-ttu-id="bce33-319">Feldomäner definiera en gruppering av virtuella datorer som delar en gemensam käll- och strömbrytare.</span><span class="sxs-lookup"><span data-stu-id="bce33-319">Fault domains define a grouping of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="bce33-320">Som standard är de virtuella datorer som har konfigurerats i din tillgänglighetsuppsättning åtskilda över upp till tre feldomäner.</span><span class="sxs-lookup"><span data-stu-id="bce33-320">By default, the virtual machines that are configured within your availability set are separated across up to three fault domains.</span></span> <span data-ttu-id="bce33-321">Tanken är att maskinvaruproblem i något av dessa feldomäner inte påverkar varje virtuell dator som kör din app.</span><span class="sxs-lookup"><span data-stu-id="bce33-321">The idea is that a hardware issue in one of these fault domains does not affect every VM that is running your app.</span></span> <span data-ttu-id="bce33-322">Azure distribuerar automatiskt virtuella datorer i feldomäner när de placeras i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="bce33-322">Azure automatically distributes VMs across the fault domains when placing them in an availability set.</span></span>

<span data-ttu-id="bce33-323">Uppgraderingsdomäner ange grupper av virtuella datorer och underliggande fysiska maskinvara som kan startas på samma gång.</span><span class="sxs-lookup"><span data-stu-id="bce33-323">Upgrade domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at the same time.</span></span> <span data-ttu-id="bce33-324">Den ordning i vilken uppgraderingsdomäner startas om kanske inte sekventiella under planerat underhåll, men bara en uppgradering på att startas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="bce33-324">The order in which upgrade domains are rebooted might not be sequential during planned maintenance, but only one upgrade is rebooted at a time.</span></span> <span data-ttu-id="bce33-325">Igen, Azure automatiskt distribuerar din virtuella dator över uppgraderingsdomäner när de placeras i en tillgänglig plats.</span><span class="sxs-lookup"><span data-stu-id="bce33-325">Again, Azure automatically distributes your VMs across upgrade domains when placing them in an availability site.</span></span>

<span data-ttu-id="bce33-326">Läs mer om [hantera tillgängligheten för virtuella datorer](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bce33-326">Read more about [managing the availability of VMs](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="create-the-linux-vms"></a><span data-ttu-id="bce33-327">Skapa de virtuella Linux-datorerna</span><span class="sxs-lookup"><span data-stu-id="bce33-327">Create the Linux VMs</span></span>
<span data-ttu-id="bce33-328">Du har skapat resurser för lagring och nätverk för att stödja Internet-tillgängliga virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="bce33-328">You've created the storage and network resources to support Internet-accessible VMs.</span></span> <span data-ttu-id="bce33-329">Nu ska vi skapa de virtuella datorer och skydda dem med en SSH-nyckel som inte har ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="bce33-329">Now let's create those VMs and secure them with an SSH key that doesn't have a password.</span></span> <span data-ttu-id="bce33-330">I det här fallet är det dags att skapa en Ubuntu VM baserat på senaste LTS.</span><span class="sxs-lookup"><span data-stu-id="bce33-330">In this case, we're going to create an Ubuntu VM based on the most recent LTS.</span></span> <span data-ttu-id="bce33-331">Vi Leta upp avbildningsinformationen med hjälp av `azure vm image list`, enligt beskrivningen i [söker Azure VM-bilder](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bce33-331">We locate that image information by using `azure vm image list`, as described in [finding Azure VM images](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="bce33-332">Vi valde en avbildning med hjälp av kommandot `azure vm image list westeurope canonical | grep LTS`.</span><span class="sxs-lookup"><span data-stu-id="bce33-332">We selected an image by using the command `azure vm image list westeurope canonical | grep LTS`.</span></span> <span data-ttu-id="bce33-333">I detta fall kan vi använda `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`.</span><span class="sxs-lookup"><span data-stu-id="bce33-333">In this case, we use `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`.</span></span> <span data-ttu-id="bce33-334">Vi klara för det senaste fältet `latest` så att i framtiden vi alltid får den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="bce33-334">For the last field, we pass `latest` so that in the future we always get the most recent build.</span></span> <span data-ttu-id="bce33-335">(Vi använder strängen är `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).</span><span class="sxs-lookup"><span data-stu-id="bce33-335">(The string we use is `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).</span></span>

<span data-ttu-id="bce33-336">Nästa steg är bekant för alla som redan har skapat ett ssh rsa-offentliga och privata nyckel koppla på Linux- eller Mac med hjälp av **ssh-keygen - t rsa -b 2048**.</span><span class="sxs-lookup"><span data-stu-id="bce33-336">This next step is familiar to anyone who has already created an ssh rsa public and private key pair on Linux or Mac by using **ssh-keygen -t rsa -b 2048**.</span></span> <span data-ttu-id="bce33-337">Om du inte har några certifikat nyckelpar i din `~/.ssh` katalog så kan du skapa dem:</span><span class="sxs-lookup"><span data-stu-id="bce33-337">If you do not have any certificate key pairs in your `~/.ssh` directory, you can create them:</span></span>

* <span data-ttu-id="bce33-338">Automatiskt, med hjälp av den `azure vm create --generate-ssh-keys` alternativet.</span><span class="sxs-lookup"><span data-stu-id="bce33-338">Automatically, by using the `azure vm create --generate-ssh-keys` option.</span></span>
* <span data-ttu-id="bce33-339">Manuellt med hjälp av [instruktionerna för att skapa dem själv](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bce33-339">Manually, by using [the instructions to create them yourself](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="bce33-340">Du kan också använda den `--admin-password` metod för att autentisera SSH-anslutningar när den virtuella datorn har skapats.</span><span class="sxs-lookup"><span data-stu-id="bce33-340">Alternatively, you can use the `--admin-password` method to authenticate your SSH connections after the VM is created.</span></span> <span data-ttu-id="bce33-341">Den här metoden är vanligtvis mindre säker.</span><span class="sxs-lookup"><span data-stu-id="bce33-341">This method is typically less secure.</span></span>

<span data-ttu-id="bce33-342">Vi skapa den virtuella datorn genom att våra resurser och information tillsammans med den `azure vm create` kommando:</span><span class="sxs-lookup"><span data-stu-id="bce33-342">We create the VM by bringing all our resources and information together with the `azure vm create` command:</span></span>

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

<span data-ttu-id="bce33-343">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-343">Output:</span></span>

```azurecli
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

<span data-ttu-id="bce33-344">Du kan ansluta till den virtuella datorn direkt med hjälp av din standard SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="bce33-344">You can connect to your VM immediately by using your default SSH keys.</span></span> <span data-ttu-id="bce33-345">Se till att du anger rätt port eftersom vi passera belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="bce33-345">Make sure that you specify the appropriate port since we're passing through the load balancer.</span></span> <span data-ttu-id="bce33-346">(För vårt första virtuella dator vi konfigurera NAT-regeln att vidarebefordra port 4222 till vår VM.)</span><span class="sxs-lookup"><span data-stu-id="bce33-346">(For our first VM, we set up the NAT rule to forward port 4222 to our VM.)</span></span>

```bash
ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

<span data-ttu-id="bce33-347">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-347">Output:</span></span>

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

<span data-ttu-id="bce33-348">Gå vidare och skapa den virtuella datorn med andra på samma sätt:</span><span class="sxs-lookup"><span data-stu-id="bce33-348">Go ahead and create your second VM in the same manner:</span></span>

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

<span data-ttu-id="bce33-349">Du kan nu använda den `azure vm show myResourceGroup myVM1` kommando för att undersöka vad du har skapat.</span><span class="sxs-lookup"><span data-stu-id="bce33-349">And you can now use the `azure vm show myResourceGroup myVM1` command to examine what you've created.</span></span> <span data-ttu-id="bce33-350">Nu är kör du dina Ubuntu virtuella datorer bakom en belastningsutjämnare i Azure som du kan logga in till endast med SSH-nyckel (eftersom lösenord är inaktiverade).</span><span class="sxs-lookup"><span data-stu-id="bce33-350">At this point, you're running your Ubuntu VMs behind a load balancer in Azure that you can sign into only with your SSH key pair (because passwords are disabled).</span></span> <span data-ttu-id="bce33-351">Du kan installera nginx eller httpd distribuera en webbapp och se att trafiken flödet genom belastningsutjämnaren till båda de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="bce33-351">You can install nginx or httpd, deploy a web app, and see the traffic flow through the load balancer to both of the VMs.</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myVM1
```

<span data-ttu-id="bce33-352">Resultat:</span><span class="sxs-lookup"><span data-stu-id="bce33-352">Output:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
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


## <a name="export-the-environment-as-a-template"></a><span data-ttu-id="bce33-353">Exportera miljön som en mall</span><span class="sxs-lookup"><span data-stu-id="bce33-353">Export the environment as a template</span></span>
<span data-ttu-id="bce33-354">Nu när du har skapat i den här miljön, vad händer om du vill skapa en ytterligare utvecklingsmiljö med samma parametrar eller en produktionsmiljö som matchar det?</span><span class="sxs-lookup"><span data-stu-id="bce33-354">Now that you have built out this environment, what if you want to create an additional development environment with the same parameters, or a production environment that matches it?</span></span> <span data-ttu-id="bce33-355">Hanteraren för filserverresurser använder JSON-mallar som definierar alla parametrar för din miljö.</span><span class="sxs-lookup"><span data-stu-id="bce33-355">Resource Manager uses JSON templates that define all the parameters for your environment.</span></span> <span data-ttu-id="bce33-356">Du bygga ut hela miljöer genom att referera till den här JSON-mallen.</span><span class="sxs-lookup"><span data-stu-id="bce33-356">You build out entire environments by referencing this JSON template.</span></span> <span data-ttu-id="bce33-357">Du kan [skapa JSON-mallarna manuellt](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller exportera en befintlig miljö för att skapa JSON-mallen du:</span><span class="sxs-lookup"><span data-stu-id="bce33-357">You can [build JSON templates manually](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or export an existing environment to create the JSON template for you:</span></span>

```azurecli
azure group export --name myResourceGroup
```

<span data-ttu-id="bce33-358">Det här kommandot skapar den `myResourceGroup.json` filen i din aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="bce33-358">This command creates the `myResourceGroup.json` file in your current working directory.</span></span> <span data-ttu-id="bce33-359">När du skapar en miljö med den här mallen kan uppmanas du alla resursnamn, inklusive namn för belastningsutjämnaren, nätverkskort eller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="bce33-359">When you create an environment from this template, you are prompted for all the resource names, including the names for the load balancer, network interfaces, or VMs.</span></span> <span data-ttu-id="bce33-360">Du kan fylla i dessa namn i mallfilen genom att lägga till den `-p` eller `--includeParameterDefaultValue` parametern till den `azure group export` kommando som visades tidigare.</span><span class="sxs-lookup"><span data-stu-id="bce33-360">You can populate these names in your template file by adding the `-p` or `--includeParameterDefaultValue` parameter to the `azure group export` command that was shown earlier.</span></span> <span data-ttu-id="bce33-361">Redigera JSON-mall om du vill ange resursnamn, eller [skapa en fil med parameters.json](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) som anger resursnamnen.</span><span class="sxs-lookup"><span data-stu-id="bce33-361">Edit your JSON template to specify the resource names, or [create a parameters.json file](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) that specifies the resource names.</span></span>

<span data-ttu-id="bce33-362">Skapa en miljö från mallen:</span><span class="sxs-lookup"><span data-stu-id="bce33-362">To create an environment from your template:</span></span>

```azurecli
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

<span data-ttu-id="bce33-363">Du kanske vill läsa [mer om hur du distribuerar från mallar](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bce33-363">You might want to read [more about how to deploy from templates](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="bce33-364">Läs mer om hur du uppdaterar miljöer, använda parameterfilen och komma åt mallar från en enda lagringsplats inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="bce33-364">Learn about how to incrementally update environments, use the parameters file, and access templates from a single storage location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bce33-365">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bce33-365">Next steps</span></span>
<span data-ttu-id="bce33-366">Nu är du redo att börja arbeta med flera nätverkskomponenter och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="bce33-366">Now you're ready to begin working with multiple networking components and VMs.</span></span> <span data-ttu-id="bce33-367">Du kan använda det här exemplet miljö för att bygga ut ditt program med hjälp av de kärnkomponenter som introducerades här.</span><span class="sxs-lookup"><span data-stu-id="bce33-367">You can use this sample environment to build out your application by using the core components introduced here.</span></span>
