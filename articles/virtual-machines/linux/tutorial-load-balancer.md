---
title: "Hur du läser in balansera Linux virtuella datorer i Azure | Microsoft Docs"
description: "Lär dig hur du skapar ett program med hög tillgänglighet och säkert över tre virtuella Linux-datorer med hjälp av Azure belastningsutjämnare"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 7b3a089d2f6386afcc46cbc4377594be0d758fc6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-load-balance-linux-virtual-machines-in-azure-to-create-a-highly-available-application"></a><span data-ttu-id="3060a-103">Hur du läser in balansera Linux-datorer i Azure för att skapa ett program med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="3060a-103">How to load balance Linux virtual machines in Azure to create a highly available application</span></span>
<span data-ttu-id="3060a-104">Belastningsutjämning ger högre tillgänglighet genom att sprida inkommande begäranden över flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3060a-104">Load balancing provides a higher level of availability by spreading incoming requests across multiple virtual machines.</span></span> <span data-ttu-id="3060a-105">I kursen får du lära dig om de olika komponenterna i Azure belastningsutjämnare som distribuerar trafik och ger hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="3060a-105">In this tutorial, you learn about the different components of the Azure load balancer that distribute traffic and provide high availability.</span></span> <span data-ttu-id="3060a-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="3060a-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3060a-107">Skapa en Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3060a-107">Create an Azure load balancer</span></span>
> * <span data-ttu-id="3060a-108">Skapa en belastningsutjämnaren, hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="3060a-108">Create a load balancer health probe</span></span>
> * <span data-ttu-id="3060a-109">Skapa regler för nätverkstrafik för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3060a-109">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="3060a-110">Använda molntjänster init för att skapa en grundläggande Node.js-app</span><span class="sxs-lookup"><span data-stu-id="3060a-110">Use cloud-init to create a basic Node.js app</span></span>
> * <span data-ttu-id="3060a-111">Skapa virtuella datorer och ansluta till en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3060a-111">Create virtual machines and attach to a load balancer</span></span>
> * <span data-ttu-id="3060a-112">Visa en belastningsutjämnare i praktiken</span><span class="sxs-lookup"><span data-stu-id="3060a-112">View a load balancer in action</span></span>
> * <span data-ttu-id="3060a-113">Lägga till och ta bort virtuella datorer från en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3060a-113">Add and remove VMs from a load balancer</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="3060a-114">Om du väljer att installera och använda CLI lokalt kursen krävs att du använder Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3060a-114">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="3060a-115">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="3060a-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="3060a-116">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3060a-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="azure-load-balancer-overview"></a><span data-ttu-id="3060a-117">Översikt över Azure belastningen belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3060a-117">Azure load balancer overview</span></span>
<span data-ttu-id="3060a-118">En Azure belastningsutjämnare är en belastningsutjämnare för Layer-4 (TCP, UDP) som ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfri virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3060a-118">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="3060a-119">En belastningsutjämnaren, hälsoavsökningen övervakar en viss port på varje virtuell dator och distribuerar endast trafik till en virtuell dator i drift.</span><span class="sxs-lookup"><span data-stu-id="3060a-119">A load balancer health probe monitors a given port on each VM and only distributes traffic to an operational VM.</span></span>

<span data-ttu-id="3060a-120">Du kan definiera en frontend IP-konfiguration som innehåller en eller flera offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="3060a-120">You define a front-end IP configuration that contains one or more public IP addresses.</span></span> <span data-ttu-id="3060a-121">Den här frontend IP-konfigurationen kan din belastningsutjämnare och program som ska vara tillgänglig via Internet.</span><span class="sxs-lookup"><span data-stu-id="3060a-121">This front-end IP configuration allows your load balancer and applications to be accessible over the Internet.</span></span> 

<span data-ttu-id="3060a-122">Virtuella datorer ansluta till en belastningsutjämnare med hjälp av deras virtuella nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="3060a-122">Virtual machines connect to a load balancer using their virtual network interface card (NIC).</span></span> <span data-ttu-id="3060a-123">Om du vill distribuera trafik till de virtuella datorerna en backend-adresspool som innehåller IP-adresser för virtuella (NIC) som är anslutna till belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="3060a-123">To distribute traffic to the VMs, a back-end address pool contains the IP addresses of the virtual (NICs) connected to the load balancer.</span></span>

<span data-ttu-id="3060a-124">Om du vill styra flödet av trafik, kan du definiera belastningen belastningsutjämnaren regler för specifika portar och protokoll som mappar till dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3060a-124">To control the flow of traffic, you define load balancer rules for specific ports and protocols that map to your VMs.</span></span>

<span data-ttu-id="3060a-125">Om du har följt tidigare guiden för att [skapa en virtuella datorns skaluppsättning](tutorial-create-vmss.md), belastningsutjämning skapades för dig.</span><span class="sxs-lookup"><span data-stu-id="3060a-125">If you followed the previous tutorial to [create a virtual machine scale set](tutorial-create-vmss.md), a load balancer was created for you.</span></span> <span data-ttu-id="3060a-126">Alla dessa komponenter konfigurerades för dig som en del av skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="3060a-126">All these components were configured for you as part of the scale set.</span></span>


## <a name="create-azure-load-balancer"></a><span data-ttu-id="3060a-127">Skapa Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3060a-127">Create Azure load balancer</span></span>
<span data-ttu-id="3060a-128">Det här avsnittet beskrivs hur du kan skapa och konfigurera varje komponent i belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="3060a-128">This section details how you can create and configure each component of the load balancer.</span></span> <span data-ttu-id="3060a-129">Innan du kan skapa din belastningsutjämnare, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="3060a-129">Before you can create your load balancer, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="3060a-130">I följande exempel skapas en resursgrupp med namnet *myResourceGroupLoadBalancer* i den *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="3060a-130">The following example creates a resource group named *myResourceGroupLoadBalancer* in the *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupLoadBalancer --location eastus
```

### <a name="create-a-public-ip-address"></a><span data-ttu-id="3060a-131">Skapa en offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="3060a-131">Create a public IP address</span></span>
<span data-ttu-id="3060a-132">För att komma åt din app på Internet, måste en offentlig IP-adress för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="3060a-132">To access your app on the Internet, you need a public IP address for the load balancer.</span></span> <span data-ttu-id="3060a-133">Skapa en offentlig IP-adress med [az nätverket offentliga IP-skapa](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="3060a-133">Create a public IP address with [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="3060a-134">I följande exempel skapas en offentlig IP-adress med namnet *myPublicIP* i den *myResourceGroupLoadBalancer* resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="3060a-134">The following example creates a public IP address named *myPublicIP* in the *myResourceGroupLoadBalancer* resource group:</span></span>

```azurecli-interactive 
az network public-ip create \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a><span data-ttu-id="3060a-135">Skapa en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3060a-135">Create a load balancer</span></span>
<span data-ttu-id="3060a-136">Skapa en belastningsutjämnare med [az nätverket lb skapa](/cli/azure/network/lb#create).</span><span class="sxs-lookup"><span data-stu-id="3060a-136">Create a load balancer with [az network lb create](/cli/azure/network/lb#create).</span></span> <span data-ttu-id="3060a-137">I följande exempel skapas en belastningsutjämnare med namnet *myLoadBalancer* och tilldelar den *myPublicIP* adressen till frontend IP-konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="3060a-137">The following example creates a load balancer named *myLoadBalancer* and assigns the *myPublicIP* address to the front-end IP configuration:</span></span>

```azurecli-interactive 
az network lb create \
    --resource-group myResourceGroupLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a><span data-ttu-id="3060a-138">Skapa en hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="3060a-138">Create a health probe</span></span>
<span data-ttu-id="3060a-139">Om du vill att belastningsutjämnaren ska övervaka status för din app kan du använda en hälsoavsökningen.</span><span class="sxs-lookup"><span data-stu-id="3060a-139">To allow the load balancer to monitor the status of your app, you use a health probe.</span></span> <span data-ttu-id="3060a-140">Avsökningen hälsa dynamiskt lägger till eller tar bort virtuella datorer från belastningen belastningsutjämnaren rotationen baserat på deras svar på hälsokontroller.</span><span class="sxs-lookup"><span data-stu-id="3060a-140">The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks.</span></span> <span data-ttu-id="3060a-141">En virtuell dator tas bort från belastningsutjämnaren belastningsdistribution efter två på varandra följande fel intervaller 15 sekunder som standard.</span><span class="sxs-lookup"><span data-stu-id="3060a-141">By default, a VM is removed from the load balancer distribution after two consecutive failures at 15-second intervals.</span></span> <span data-ttu-id="3060a-142">Du skapar en hälsoavsökningen baserat på ett protokoll eller ett visst hälsotillstånd sidan för kontroll för din app.</span><span class="sxs-lookup"><span data-stu-id="3060a-142">You create a health probe based on a protocol or a specific health check page for your app.</span></span> 

<span data-ttu-id="3060a-143">I följande exempel skapas en TCP-avsökning.</span><span class="sxs-lookup"><span data-stu-id="3060a-143">The following example creates a TCP probe.</span></span> <span data-ttu-id="3060a-144">Du kan också skapa anpassade HTTP-avsökningar mer detaljerade hälsokontroller.</span><span class="sxs-lookup"><span data-stu-id="3060a-144">You can also create custom HTTP probes for more fine grained health checks.</span></span> <span data-ttu-id="3060a-145">När du använder en anpassad HTTP-avsökningen, måste du skapa sidan för kontroll av hälsotillstånd som *healthcheck.js*.</span><span class="sxs-lookup"><span data-stu-id="3060a-145">When using a custom HTTP probe, you must create the health check page, such as *healthcheck.js*.</span></span> <span data-ttu-id="3060a-146">Avsökningen måste returnera ett **HTTP 200 OK** svar att behålla värden i rotation belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="3060a-146">The probe must return an **HTTP 200 OK** response for the load balancer to keep the host in rotation.</span></span>

<span data-ttu-id="3060a-147">Om du vill skapa en TCP-hälsoavsökningen du använder [az nätverket lb avsökningen skapa](/cli/azure/network/lb/probe#create).</span><span class="sxs-lookup"><span data-stu-id="3060a-147">To create a TCP health probe, you use [az network lb probe create](/cli/azure/network/lb/probe#create).</span></span> <span data-ttu-id="3060a-148">I följande exempel skapas en hälsoavsökningen med namnet *myHealthProbe*:</span><span class="sxs-lookup"><span data-stu-id="3060a-148">The following example creates a health probe named *myHealthProbe*:</span></span>

```azurecli-interactive 
az network lb probe create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a><span data-ttu-id="3060a-149">Skapa en regel för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3060a-149">Create a load balancer rule</span></span>
<span data-ttu-id="3060a-150">En regel för belastningsutjämnare används för att definiera hur trafiken distribueras till de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="3060a-150">A load balancer rule is used to define how traffic is distributed to the VMs.</span></span> <span data-ttu-id="3060a-151">Du kan definiera frontend IP-konfiguration för inkommande trafik och backend-IP-adresspool för att ta emot trafik, tillsammans med nödvändig käll- och port.</span><span class="sxs-lookup"><span data-stu-id="3060a-151">You define the front-end IP configuration for the incoming traffic and the back-end IP pool to receive the traffic, along with the required source and destination port.</span></span> <span data-ttu-id="3060a-152">Om du vill kontrollera endast felfri virtuella datorer ta emot trafik kan definiera du också hälsoavsökningen att använda.</span><span class="sxs-lookup"><span data-stu-id="3060a-152">To make sure only healthy VMs receive traffic, you also define the health probe to use.</span></span>

<span data-ttu-id="3060a-153">Skapa en regel för belastningsutjämnare med [az nätverket lb regeln skapa](/cli/azure/network/lb/rule#create).</span><span class="sxs-lookup"><span data-stu-id="3060a-153">Create a load balancer rule with [az network lb rule create](/cli/azure/network/lb/rule#create).</span></span> <span data-ttu-id="3060a-154">I följande exempel skapas en regel med namnet *myLoadBalancerRule*, använder den *myHealthProbe* hälsoavsökningen och saldon trafik på port *80*:</span><span class="sxs-lookup"><span data-stu-id="3060a-154">The following example creates a rule named *myLoadBalancerRule*, uses the *myHealthProbe* health probe, and balances traffic on port *80*:</span></span>

```azurecli-interactive 
az network lb rule create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myLoadBalancerRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```


## <a name="configure-virtual-network"></a><span data-ttu-id="3060a-155">Konfigurera virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="3060a-155">Configure virtual network</span></span>
<span data-ttu-id="3060a-156">Innan du distribuerar vissa virtuella datorer och testa din belastningsutjämnare, skapa stödresurser för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="3060a-156">Before you deploy some VMs and can test your balancer, create the supporting virtual network resources.</span></span> <span data-ttu-id="3060a-157">Mer information om virtuella nätverk finns i [hantera virtuella Azure-nätverk](tutorial-virtual-network.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="3060a-157">For more information about virtual networks, see the [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="3060a-158">Skapa nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="3060a-158">Create network resources</span></span>
<span data-ttu-id="3060a-159">Skapa ett virtuellt nätverk med [az network vnet skapa](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="3060a-159">Create a virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="3060a-160">I följande exempel skapas ett virtuellt nätverk med namnet *myVnet* med ett undernät med namnet *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="3060a-160">The following example creates a virtual network named *myVnet* with a subnet named *mySubnet*:</span></span>

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

<span data-ttu-id="3060a-161">Om du vill lägga till en nätverkssäkerhetsgrupp måste du använda [az nätverket nsg skapa](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="3060a-161">To add a network security group, you use [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="3060a-162">I följande exempel skapas en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="3060a-162">The following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli-interactive 
az network nsg create \
    --resource-group myResourceGroupLoadBalancer \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="3060a-163">Skapa en grupp nätverkssäkerhetsregeln med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="3060a-163">Create a network security group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="3060a-164">I följande exempel skapas en grupp nätverkssäkerhetsregeln med namnet *myNetworkSecurityGroupRule*:</span><span class="sxs-lookup"><span data-stu-id="3060a-164">The following example creates a network security group rule named *myNetworkSecurityGroupRule*:</span></span>

```azurecli-interactive 
az network nsg rule create \
    --resource-group myResourceGroupLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

<span data-ttu-id="3060a-165">Virtuella nätverkskort skapas med [az nätverket nic skapa](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="3060a-165">Virtual NICs are created with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="3060a-166">I följande exempel skapas tre virtuella nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="3060a-166">The following example creates three virtual NICs.</span></span> <span data-ttu-id="3060a-167">(Ett virtuellt nätverkskort för varje virtuell dator skapar du en app i följande steg).</span><span class="sxs-lookup"><span data-stu-id="3060a-167">(One virtual NIC for each VM you create for your app in the following steps).</span></span> <span data-ttu-id="3060a-168">Du kan skapa ytterligare virtuella nätverkskort och virtuella datorer när som helst och lägga till dem i belastningsutjämnaren:</span><span class="sxs-lookup"><span data-stu-id="3060a-168">You can create additional virtual NICs and VMs at any time and add them to the load balancer:</span></span>

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupLoadBalancer \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-virtual-machines"></a><span data-ttu-id="3060a-169">Skapa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3060a-169">Create virtual machines</span></span>

### <a name="create-cloud-init-config"></a><span data-ttu-id="3060a-170">Skapa moln init config</span><span class="sxs-lookup"><span data-stu-id="3060a-170">Create cloud-init config</span></span>
<span data-ttu-id="3060a-171">I en tidigare självstudiekurs om [hur du anpassar en Linux-dator vid den första starten](tutorial-automate-vm-deployment.md), du lärt dig hur du automatiserar VM anpassning med molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="3060a-171">In a previous tutorial on [How to customize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md), you learned how to automate VM customization with cloud-init.</span></span> <span data-ttu-id="3060a-172">Du kan använda samma molnet init-konfigurationsfilen för att installera NGINX och köra en enkel ”Hello World” Node.js-app.</span><span class="sxs-lookup"><span data-stu-id="3060a-172">You can use the same cloud-init configuration file to install NGINX and run a simple 'Hello World' Node.js app.</span></span>

<span data-ttu-id="3060a-173">Skapa en fil med namnet i din aktuella shell *moln init.txt* och klistra in följande konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3060a-173">In your current shell, create a file named *cloud-init.txt* and paste the following configuration.</span></span> <span data-ttu-id="3060a-174">Till exempel skapa filen i molnet Shell inte på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="3060a-174">For example, create the file in the Cloud Shell not on your local machine.</span></span> <span data-ttu-id="3060a-175">Ange `sensible-editor cloud-init.txt` att skapa filen och se en lista över tillgängliga redigerare.</span><span class="sxs-lookup"><span data-stu-id="3060a-175">Enter `sensible-editor cloud-init.txt` to create the file and see a list of available editors.</span></span> <span data-ttu-id="3060a-176">Se till att hela molnet init-filen har kopierats korrekt, särskilt den första raden:</span><span class="sxs-lookup"><span data-stu-id="3060a-176">Make sure that the whole cloud-init file is copied correctly, especially the first line:</span></span>

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-virtual-machines"></a><span data-ttu-id="3060a-177">Skapa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3060a-177">Create virtual machines</span></span>
<span data-ttu-id="3060a-178">Placera dina virtuella datorer i en tillgänglighetsuppsättning för att förbättra tillgängligheten för din app.</span><span class="sxs-lookup"><span data-stu-id="3060a-178">To improve the high availability of your app, place your VMs in an availability set.</span></span> <span data-ttu-id="3060a-179">Mer information om tillgänglighetsuppsättningar finns i den tidigare [hur du skapar virtuella datorer med hög tillgänglighet](tutorial-availability-sets.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="3060a-179">For more information about availability sets, see the previous [How to create highly available virtual machines](tutorial-availability-sets.md) tutorial.</span></span>

<span data-ttu-id="3060a-180">Skapa en tillgänglighetsuppsättning med [az vm tillgänglighetsuppsättning skapa](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="3060a-180">Create an availability set with [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="3060a-181">I följande exempel skapas en tillgänglighetsuppsättning namngivna *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="3060a-181">The following example creates an availability set named *myAvailabilitySet*:</span></span>

```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupLoadBalancer \
    --name myAvailabilitySet
```

<span data-ttu-id="3060a-182">Nu kan du skapa de virtuella datorerna med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="3060a-182">Now you can create the VMs with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="3060a-183">I följande exempel skapas tre virtuella datorer och genererar SSH-nycklar, om de inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="3060a-183">The following example creates three VMs and generates SSH keys if they do not already exist:</span></span>

```bash
for i in `seq 1 3`; do
    az vm create \
        --resource-group myResourceGroupLoadBalancer \
        --name myVM$i \
        --availability-set myAvailabilitySet \
        --nics myNic$i \
        --image UbuntuLTS \
        --admin-username azureuser \
        --generate-ssh-keys \
        --custom-data cloud-init.txt \
        --no-wait
done
```

<span data-ttu-id="3060a-184">Det finns bakgrundsaktiviteter för att fortsätta att köras när Azure CLI återgår till Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="3060a-184">There are background tasks that continue to run after the Azure CLI returns you to the prompt.</span></span> <span data-ttu-id="3060a-185">Den `--no-wait` parametern inte väntar på att alla aktiviteter slutförts.</span><span class="sxs-lookup"><span data-stu-id="3060a-185">The `--no-wait` parameter does not wait for all the tasks to complete.</span></span> <span data-ttu-id="3060a-186">Det kan vara en annan några minuter innan du kan komma åt appen.</span><span class="sxs-lookup"><span data-stu-id="3060a-186">It may be another couple of minutes before you can access the app.</span></span> <span data-ttu-id="3060a-187">I belastningsutjämnaren, hälsoavsökningen identifierar automatiskt när appen körs på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3060a-187">The load balancer health probe automatically detects when the app is running on each VM.</span></span> <span data-ttu-id="3060a-188">När appen körs börjar belastningsutjämningsregeln distribuera trafiken.</span><span class="sxs-lookup"><span data-stu-id="3060a-188">Once the app is running, the load balancer rule starts to distribute traffic.</span></span>


## <a name="test-load-balancer"></a><span data-ttu-id="3060a-189">Testa belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3060a-189">Test load balancer</span></span>
<span data-ttu-id="3060a-190">Hämta offentlig IP-adressen för din belastningsutjämnare med [az nätverket offentliga ip-visa](/cli/azure/network/public-ip#show).</span><span class="sxs-lookup"><span data-stu-id="3060a-190">Obtain the public IP address of your load balancer with [az network public-ip show](/cli/azure/network/public-ip#show).</span></span> <span data-ttu-id="3060a-191">I följande exempel hämtar IP-adressen för *myPublicIP* skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="3060a-191">The following example obtains the IP address for *myPublicIP* created earlier:</span></span>

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="3060a-192">Du kan sedan ange den offentliga IP-adressen i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="3060a-192">You can then enter the public IP address in to a web browser.</span></span> <span data-ttu-id="3060a-193">Kom ihåg - det tar några minuter på de virtuella datorerna ska bli klar innan belastningsutjämnaren börjar distribuera trafiken till dem..</span><span class="sxs-lookup"><span data-stu-id="3060a-193">Remember - it takes a few minutes the the VMs to be ready before the load balancer starts to distribute traffic to them.</span></span> <span data-ttu-id="3060a-194">Appen visas, inklusive värdnamnet för den virtuella datorn som belastningsutjämnaren distribuerade trafik till som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="3060a-194">The app is displayed, including the hostname of the VM that the load balancer distributed traffic to as in the following example:</span></span>

![Node.js-app som körs](./media/tutorial-load-balancer/running-nodejs-app.png)

<span data-ttu-id="3060a-196">Om du vill se belastningsutjämnaren distribuerar trafik över alla tre virtuella datorer som kör appen du kan force-uppdatera webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="3060a-196">To see the load balancer distribute traffic across all three VMs running your app, you can force-refresh your web browser.</span></span>


## <a name="add-and-remove-vms"></a><span data-ttu-id="3060a-197">Lägga till och ta bort virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3060a-197">Add and remove VMs</span></span>
<span data-ttu-id="3060a-198">Du kan behöva utföra underhåll på virtuella datorer som kör appen, till exempel installera uppdateringar av OS.</span><span class="sxs-lookup"><span data-stu-id="3060a-198">You may need to perform maintenance on the VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="3060a-199">Du kan behöva lägga till ytterligare virtuella datorer för att hantera den ökade trafiken till din app.</span><span class="sxs-lookup"><span data-stu-id="3060a-199">To deal with increased traffic to your app, you may need to add additional VMs.</span></span> <span data-ttu-id="3060a-200">Det här avsnittet visar hur du tar bort eller lägga till en virtuell dator från belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="3060a-200">This section shows you how to remove or add a VM from the load balancer.</span></span>

### <a name="remove-a-vm-from-the-load-balancer"></a><span data-ttu-id="3060a-201">Ta bort en virtuell dator från belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="3060a-201">Remove a VM from the load balancer</span></span>
<span data-ttu-id="3060a-202">Du kan ta bort en virtuell dator från backend-adresspool med [az nic ip-config-nätverksadresspool ta bort](/cli/azure/network/nic/ip-config/address-pool#remove).</span><span class="sxs-lookup"><span data-stu-id="3060a-202">You can remove a VM from the backend address pool with [az network nic ip-config address-pool remove](/cli/azure/network/nic/ip-config/address-pool#remove).</span></span> <span data-ttu-id="3060a-203">I följande exempel tar bort det virtuella nätverkskortet för **myVM2** från *myLoadBalancer*:</span><span class="sxs-lookup"><span data-stu-id="3060a-203">The following example removes the virtual NIC for **myVM2** from *myLoadBalancer*:</span></span>

```azurecli-interactive 
az network nic ip-config address-pool remove \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

<span data-ttu-id="3060a-204">Om du vill se belastningsutjämnaren distribuerar trafik över de återstående två virtuella datorerna kör appen du kan framtvinga-uppdatera webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="3060a-204">To see the load balancer distribute traffic across the remaining two VMs running your app you can force-refresh your web browser.</span></span> <span data-ttu-id="3060a-205">Du kan nu utföra underhåll på den virtuella datorn, till exempel installera uppdateringar av operativsystem eller utföra en VM-omstart.</span><span class="sxs-lookup"><span data-stu-id="3060a-205">You can now perform maintenance on the VM, such as installing OS updates or performing a VM reboot.</span></span>

### <a name="add-a-vm-to-the-load-balancer"></a><span data-ttu-id="3060a-206">Lägga till en virtuell dator till belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="3060a-206">Add a VM to the load balancer</span></span>
<span data-ttu-id="3060a-207">När du utför underhåll på VM, eller om du behöver Utöka kapaciteten kan du lägga till en virtuell dator till backend-adresspool med [az nic ip-config-nätverksadresspool lägga till](/cli/azure/network/nic/ip-config/address-pool#add).</span><span class="sxs-lookup"><span data-stu-id="3060a-207">After performing VM maintenance, or if you need to expand capacity, you can add a VM to the backend address pool with [az network nic ip-config address-pool add](/cli/azure/network/nic/ip-config/address-pool#add).</span></span> <span data-ttu-id="3060a-208">I följande exempel läggs det virtuella nätverkskortet för **myVM2** till *myLoadBalancer*:</span><span class="sxs-lookup"><span data-stu-id="3060a-208">The following example adds the virtual NIC for **myVM2** to *myLoadBalancer*:</span></span>

```azurecli-interactive 
az network nic ip-config address-pool add \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```


## <a name="next-steps"></a><span data-ttu-id="3060a-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3060a-209">Next steps</span></span>
<span data-ttu-id="3060a-210">I kursen får du skapat en belastningsutjämnare och anslutna virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3060a-210">In this tutorial, you created a load balancer and attached VMs to it.</span></span> <span data-ttu-id="3060a-211">Du har lärt dig hur till:</span><span class="sxs-lookup"><span data-stu-id="3060a-211">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3060a-212">Skapa en Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3060a-212">Create an Azure load balancer</span></span>
> * <span data-ttu-id="3060a-213">Skapa en belastningsutjämnaren, hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="3060a-213">Create a load balancer health probe</span></span>
> * <span data-ttu-id="3060a-214">Skapa regler för nätverkstrafik för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3060a-214">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="3060a-215">Använda molntjänster init för att skapa en grundläggande Node.js-app</span><span class="sxs-lookup"><span data-stu-id="3060a-215">Use cloud-init to create a basic Node.js app</span></span>
> * <span data-ttu-id="3060a-216">Skapa virtuella datorer och ansluta till en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3060a-216">Create virtual machines and attach to a load balancer</span></span>
> * <span data-ttu-id="3060a-217">Visa en belastningsutjämnare i praktiken</span><span class="sxs-lookup"><span data-stu-id="3060a-217">View a load balancer in action</span></span>
> * <span data-ttu-id="3060a-218">Lägga till och ta bort virtuella datorer från en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3060a-218">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="3060a-219">Gå vidare till nästa kurs lära dig mer om virtuella Azure-nätverkskomponenter.</span><span class="sxs-lookup"><span data-stu-id="3060a-219">Advance to the next tutorial to learn more about Azure virtual network components.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3060a-220">Hantera virtuella datorer och virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="3060a-220">Manage VMs and virtual networks</span></span>](tutorial-virtual-network.md)
