---
title: aaaHow tooload balansera Linux virtuella datorer i Azure | Microsoft Docs
description: "Lär dig hur toouse hello Azure läsa in belastningsutjämning toocreate ett program med hög tillgänglighet och säkert över tre virtuella Linux-datorer"
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
ms.openlocfilehash: f01752c3caec3489ee13e63000775769f3236e11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooload-balance-linux-virtual-machines-in-azure-toocreate-a-highly-available-application"></a><span data-ttu-id="ffcbe-103">Hur balansera tooload Linux-datorer i Azure toocreate ett program med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="ffcbe-103">How tooload balance Linux virtual machines in Azure toocreate a highly available application</span></span>
<span data-ttu-id="ffcbe-104">Belastningsutjämning ger högre tillgänglighet genom att sprida inkommande begäranden över flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-104">Load balancing provides a higher level of availability by spreading incoming requests across multiple virtual machines.</span></span> <span data-ttu-id="ffcbe-105">I kursen får information du om hello olika komponenterna i hello Azure belastningsutjämnare som distribuerar trafik och ger hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-105">In this tutorial, you learn about hello different components of hello Azure load balancer that distribute traffic and provide high availability.</span></span> <span data-ttu-id="ffcbe-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ffcbe-107">Skapa en Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-107">Create an Azure load balancer</span></span>
> * <span data-ttu-id="ffcbe-108">Skapa en belastningsutjämnaren, hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="ffcbe-108">Create a load balancer health probe</span></span>
> * <span data-ttu-id="ffcbe-109">Skapa regler för nätverkstrafik för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-109">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="ffcbe-110">Använda molntjänster init toocreate en grundläggande Node.js-app</span><span class="sxs-lookup"><span data-stu-id="ffcbe-110">Use cloud-init toocreate a basic Node.js app</span></span>
> * <span data-ttu-id="ffcbe-111">Skapa virtuella datorer och bifoga tooa belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-111">Create virtual machines and attach tooa load balancer</span></span>
> * <span data-ttu-id="ffcbe-112">Visa en belastningsutjämnare i praktiken</span><span class="sxs-lookup"><span data-stu-id="ffcbe-112">View a load balancer in action</span></span>
> * <span data-ttu-id="ffcbe-113">Lägga till och ta bort virtuella datorer från en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-113">Add and remove VMs from a load balancer</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ffcbe-114">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-114">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="ffcbe-115">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="ffcbe-116">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="azure-load-balancer-overview"></a><span data-ttu-id="ffcbe-117">Översikt över Azure belastningen belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-117">Azure load balancer overview</span></span>
<span data-ttu-id="ffcbe-118">En Azure belastningsutjämnare är en belastningsutjämnare för Layer-4 (TCP, UDP) som ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfri virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-118">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="ffcbe-119">En belastningsutjämnaren, hälsoavsökningen övervakar en viss port på varje virtuell dator och distribuerar endast trafik tooan operativa VM.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-119">A load balancer health probe monitors a given port on each VM and only distributes traffic tooan operational VM.</span></span>

<span data-ttu-id="ffcbe-120">Du kan definiera en frontend IP-konfiguration som innehåller en eller flera offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-120">You define a front-end IP configuration that contains one or more public IP addresses.</span></span> <span data-ttu-id="ffcbe-121">Den här frontend IP-konfiguration kan din load balancer och program toobe tillgänglig via hello Internet.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-121">This front-end IP configuration allows your load balancer and applications toobe accessible over hello Internet.</span></span> 

<span data-ttu-id="ffcbe-122">Virtuella datorer ansluta tooa belastningsutjämning med hjälp av deras virtuella nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-122">Virtual machines connect tooa load balancer using their virtual network interface card (NIC).</span></span> <span data-ttu-id="ffcbe-123">toodistribute trafik toohello virtuella datorer, en backend-adresspool innehåller hello IP-adresser för hello virtuella (NIC) anslutna toohello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-123">toodistribute traffic toohello VMs, a back-end address pool contains hello IP addresses of hello virtual (NICs) connected toohello load balancer.</span></span>

<span data-ttu-id="ffcbe-124">toocontrol hello trafikflödet, definiera regler för inläsning av belastningsutjämning för specifika portar och protokoll som mappar tooyour virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-124">toocontrol hello flow of traffic, you define load balancer rules for specific ports and protocols that map tooyour VMs.</span></span>

<span data-ttu-id="ffcbe-125">Om du har följt hello tidigare självstudier för[skapa en virtuella datorns skaluppsättning](tutorial-create-vmss.md), belastningsutjämning skapades för dig.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-125">If you followed hello previous tutorial too[create a virtual machine scale set](tutorial-create-vmss.md), a load balancer was created for you.</span></span> <span data-ttu-id="ffcbe-126">Alla dessa komponenter konfigurerades för dig som en del av hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-126">All these components were configured for you as part of hello scale set.</span></span>


## <a name="create-azure-load-balancer"></a><span data-ttu-id="ffcbe-127">Skapa Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-127">Create Azure load balancer</span></span>
<span data-ttu-id="ffcbe-128">Det här avsnittet beskrivs hur du kan skapa och konfigurera varje komponent av hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-128">This section details how you can create and configure each component of hello load balancer.</span></span> <span data-ttu-id="ffcbe-129">Innan du kan skapa din belastningsutjämnare, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-129">Before you can create your load balancer, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="ffcbe-130">hello följande exempel skapar en resursgrupp med namnet *myResourceGroupLoadBalancer* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-130">hello following example creates a resource group named *myResourceGroupLoadBalancer* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupLoadBalancer --location eastus
```

### <a name="create-a-public-ip-address"></a><span data-ttu-id="ffcbe-131">Skapa en offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="ffcbe-131">Create a public IP address</span></span>
<span data-ttu-id="ffcbe-132">tooaccess din app på hello Internet, måste en offentlig IP-adress för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-132">tooaccess your app on hello Internet, you need a public IP address for hello load balancer.</span></span> <span data-ttu-id="ffcbe-133">Skapa en offentlig IP-adress med [az nätverket offentliga IP-skapa](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-133">Create a public IP address with [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="ffcbe-134">hello följande exempel skapas en offentlig IP-adress med namnet *myPublicIP* i hello *myResourceGroupLoadBalancer* resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-134">hello following example creates a public IP address named *myPublicIP* in hello *myResourceGroupLoadBalancer* resource group:</span></span>

```azurecli-interactive 
az network public-ip create \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a><span data-ttu-id="ffcbe-135">Skapa en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-135">Create a load balancer</span></span>
<span data-ttu-id="ffcbe-136">Skapa en belastningsutjämnare med [az nätverket lb skapa](/cli/azure/network/lb#create).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-136">Create a load balancer with [az network lb create](/cli/azure/network/lb#create).</span></span> <span data-ttu-id="ffcbe-137">hello följande exempel skapas en belastningsutjämnare med namnet *myLoadBalancer* och tilldelar hello *myPublicIP* toohello frontend IP-adresskonfiguration:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-137">hello following example creates a load balancer named *myLoadBalancer* and assigns hello *myPublicIP* address toohello front-end IP configuration:</span></span>

```azurecli-interactive 
az network lb create \
    --resource-group myResourceGroupLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a><span data-ttu-id="ffcbe-138">Skapa en hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="ffcbe-138">Create a health probe</span></span>
<span data-ttu-id="ffcbe-139">tooallow hello belastningen belastningsutjämnaren toomonitor hello statusen för din app, använda en hälsoavsökningen.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-139">tooallow hello load balancer toomonitor hello status of your app, you use a health probe.</span></span> <span data-ttu-id="ffcbe-140">Hej hälsoavsökningen dynamiskt lägger till eller tar bort virtuella datorer från hello belastningen belastningsutjämnaren rotation baserat på deras svar toohealth kontroller.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-140">hello health probe dynamically adds or removes VMs from hello load balancer rotation based on their response toohealth checks.</span></span> <span data-ttu-id="ffcbe-141">En virtuell dator tas bort från hello belastningsutjämnaren belastningsdistribution efter två på varandra följande fel intervaller 15 sekunder som standard.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-141">By default, a VM is removed from hello load balancer distribution after two consecutive failures at 15-second intervals.</span></span> <span data-ttu-id="ffcbe-142">Du skapar en hälsoavsökningen baserat på ett protokoll eller ett visst hälsotillstånd sidan för kontroll för din app.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-142">You create a health probe based on a protocol or a specific health check page for your app.</span></span> 

<span data-ttu-id="ffcbe-143">hello följande exempel skapar en TCP-avsökning.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-143">hello following example creates a TCP probe.</span></span> <span data-ttu-id="ffcbe-144">Du kan också skapa anpassade HTTP-avsökningar mer detaljerade hälsokontroller.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-144">You can also create custom HTTP probes for more fine grained health checks.</span></span> <span data-ttu-id="ffcbe-145">När du använder en anpassad HTTP-avsökningen, måste du skapa hello hälsa sidan kontroll som *healthcheck.js*.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-145">When using a custom HTTP probe, you must create hello health check page, such as *healthcheck.js*.</span></span> <span data-ttu-id="ffcbe-146">hello avsökning måste returnera ett **HTTP 200 OK** svar för hello belastningen belastningsutjämnaren tookeep hello värd i rotation.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-146">hello probe must return an **HTTP 200 OK** response for hello load balancer tookeep hello host in rotation.</span></span>

<span data-ttu-id="ffcbe-147">toocreate en TCP-hälsoavsökningen du använder [az nätverket lb avsökningen skapa](/cli/azure/network/lb/probe#create).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-147">toocreate a TCP health probe, you use [az network lb probe create](/cli/azure/network/lb/probe#create).</span></span> <span data-ttu-id="ffcbe-148">hello följande exempel skapas en hälsoavsökningen med namnet *myHealthProbe*:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-148">hello following example creates a health probe named *myHealthProbe*:</span></span>

```azurecli-interactive 
az network lb probe create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a><span data-ttu-id="ffcbe-149">Skapa en regel för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-149">Create a load balancer rule</span></span>
<span data-ttu-id="ffcbe-150">En regel för belastningsutjämnare är används toodefine hur trafiken är distribuerade toohello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-150">A load balancer rule is used toodefine how traffic is distributed toohello VMs.</span></span> <span data-ttu-id="ffcbe-151">Du kan definiera hello frontend IP-konfiguration för hello inkommande trafik och hello backend-IP-adresspool tooreceive hello trafik, tillsammans med hello krävs käll- och målport.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-151">You define hello front-end IP configuration for hello incoming traffic and hello back-end IP pool tooreceive hello traffic, along with hello required source and destination port.</span></span> <span data-ttu-id="ffcbe-152">toomake att endast felfri virtuella datorer ta emot trafik, du också definiera hello hälsa avsökningen toouse.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-152">toomake sure only healthy VMs receive traffic, you also define hello health probe toouse.</span></span>

<span data-ttu-id="ffcbe-153">Skapa en regel för belastningsutjämnare med [az nätverket lb regeln skapa](/cli/azure/network/lb/rule#create).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-153">Create a load balancer rule with [az network lb rule create](/cli/azure/network/lb/rule#create).</span></span> <span data-ttu-id="ffcbe-154">hello följande exempel skapas en regel med namnet *myLoadBalancerRule*, använder hello *myHealthProbe* hälsoavsökningen och saldon trafik på port *80*:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-154">hello following example creates a rule named *myLoadBalancerRule*, uses hello *myHealthProbe* health probe, and balances traffic on port *80*:</span></span>

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


## <a name="configure-virtual-network"></a><span data-ttu-id="ffcbe-155">Konfigurera ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="ffcbe-155">Configure virtual network</span></span>
<span data-ttu-id="ffcbe-156">Innan du distribuerar vissa virtuella datorer och testa din belastningsutjämnare, skapa hello stöder nätverksresurser på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-156">Before you deploy some VMs and can test your balancer, create hello supporting virtual network resources.</span></span> <span data-ttu-id="ffcbe-157">Mer information om virtuella nätverk finns hello [hantera virtuella Azure-nätverk](tutorial-virtual-network.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-157">For more information about virtual networks, see hello [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="ffcbe-158">Skapa nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="ffcbe-158">Create network resources</span></span>
<span data-ttu-id="ffcbe-159">Skapa ett virtuellt nätverk med [az network vnet skapa](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-159">Create a virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="ffcbe-160">hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* med ett undernät med namnet *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-160">hello following example creates a virtual network named *myVnet* with a subnet named *mySubnet*:</span></span>

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

<span data-ttu-id="ffcbe-161">tooadd en nätverkssäkerhetsgrupp som du använder [az nätverket nsg skapa](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-161">tooadd a network security group, you use [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="ffcbe-162">hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-162">hello following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli-interactive 
az network nsg create \
    --resource-group myResourceGroupLoadBalancer \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="ffcbe-163">Skapa en grupp nätverkssäkerhetsregeln med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-163">Create a network security group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="ffcbe-164">hello följande exempel skapar en grupp nätverkssäkerhetsregeln med namnet *myNetworkSecurityGroupRule*:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-164">hello following example creates a network security group rule named *myNetworkSecurityGroupRule*:</span></span>

```azurecli-interactive 
az network nsg rule create \
    --resource-group myResourceGroupLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

<span data-ttu-id="ffcbe-165">Virtuella nätverkskort skapas med [az nätverket nic skapa](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-165">Virtual NICs are created with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="ffcbe-166">hello skapas följande exempel tre virtuella nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-166">hello following example creates three virtual NICs.</span></span> <span data-ttu-id="ffcbe-167">(Ett virtuellt nätverkskort för varje virtuell dator som du skapar för din app i hello följande steg).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-167">(One virtual NIC for each VM you create for your app in hello following steps).</span></span> <span data-ttu-id="ffcbe-168">Du kan skapa ytterligare virtuella nätverkskort och virtuella datorer när som helst och lägga till dem toohello belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-168">You can create additional virtual NICs and VMs at any time and add them toohello load balancer:</span></span>

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

## <a name="create-virtual-machines"></a><span data-ttu-id="ffcbe-169">Skapa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="ffcbe-169">Create virtual machines</span></span>

### <a name="create-cloud-init-config"></a><span data-ttu-id="ffcbe-170">Skapa moln init config</span><span class="sxs-lookup"><span data-stu-id="ffcbe-170">Create cloud-init config</span></span>
<span data-ttu-id="ffcbe-171">I en tidigare självstudiekurs om [hur toocustomize en Linux-dator vid den första starten](tutorial-automate-vm-deployment.md), du lärt dig hur tooautomate VM anpassning med molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-171">In a previous tutorial on [How toocustomize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md), you learned how tooautomate VM customization with cloud-init.</span></span> <span data-ttu-id="ffcbe-172">Du kan använda hello samma molnet init configuration file tooinstall NGINX och kör en enkel Hello World Node.js-app.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-172">You can use hello same cloud-init configuration file tooinstall NGINX and run a simple 'Hello World' Node.js app.</span></span>

<span data-ttu-id="ffcbe-173">Skapa en fil med namnet i din aktuella shell *moln init.txt* och klistra in hello följande konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-173">In your current shell, create a file named *cloud-init.txt* and paste hello following configuration.</span></span> <span data-ttu-id="ffcbe-174">Till exempel skapa hello-filen i hello molnet Shell inte på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-174">For example, create hello file in hello Cloud Shell not on your local machine.</span></span> <span data-ttu-id="ffcbe-175">Ange `sensible-editor cloud-init.txt` toocreate hello filen och visas i listan över tillgängliga redigerare.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-175">Enter `sensible-editor cloud-init.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="ffcbe-176">Kontrollera att filen hello hela molnet initiering kopierats korrekt, särskilt hello första raden:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-176">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

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

### <a name="create-virtual-machines"></a><span data-ttu-id="ffcbe-177">Skapa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="ffcbe-177">Create virtual machines</span></span>
<span data-ttu-id="ffcbe-178">tooimprove hello hög tillgänglighet för din app, placera dina virtuella datorer i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-178">tooimprove hello high availability of your app, place your VMs in an availability set.</span></span> <span data-ttu-id="ffcbe-179">Mer information om tillgänglighetsuppsättningar finns hello tidigare [hur toocreate högtillgängliga virtuella datorer](tutorial-availability-sets.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-179">For more information about availability sets, see hello previous [How toocreate highly available virtual machines](tutorial-availability-sets.md) tutorial.</span></span>

<span data-ttu-id="ffcbe-180">Skapa en tillgänglighetsuppsättning med [az vm tillgänglighetsuppsättning skapa](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-180">Create an availability set with [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="ffcbe-181">hello följande exempel skapas en tillgänglighetsuppsättning namngivna *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-181">hello following example creates an availability set named *myAvailabilitySet*:</span></span>

```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupLoadBalancer \
    --name myAvailabilitySet
```

<span data-ttu-id="ffcbe-182">Nu kan du skapa hello virtuella datorer med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-182">Now you can create hello VMs with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="ffcbe-183">hello följande exempel skapas tre virtuella datorer och genererar SSH-nycklar, om de inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-183">hello following example creates three VMs and generates SSH keys if they do not already exist:</span></span>

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

<span data-ttu-id="ffcbe-184">Det finns bakgrundsaktiviteter för att fortsätta toorun när hello Azure CLI returnerar toohello prompt.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-184">There are background tasks that continue toorun after hello Azure CLI returns you toohello prompt.</span></span> <span data-ttu-id="ffcbe-185">Hej `--no-wait` parametern inte väntar för alla hello uppgifter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-185">hello `--no-wait` parameter does not wait for all hello tasks toocomplete.</span></span> <span data-ttu-id="ffcbe-186">Det kan vara en annan några minuter innan du kan komma åt hello app.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-186">It may be another couple of minutes before you can access hello app.</span></span> <span data-ttu-id="ffcbe-187">hello identifierar belastningsutjämnaren, hälsoavsökningen automatiskt när hello appen körs på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-187">hello load balancer health probe automatically detects when hello app is running on each VM.</span></span> <span data-ttu-id="ffcbe-188">När hello appen körs startar hello-regel för belastningsutjämnare toodistribute trafik.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-188">Once hello app is running, hello load balancer rule starts toodistribute traffic.</span></span>


## <a name="test-load-balancer"></a><span data-ttu-id="ffcbe-189">Testa belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-189">Test load balancer</span></span>
<span data-ttu-id="ffcbe-190">Hämta hello offentliga IP-adressen för din belastningsutjämnare med [az nätverket offentliga ip-visa](/cli/azure/network/public-ip#show).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-190">Obtain hello public IP address of your load balancer with [az network public-ip show](/cli/azure/network/public-ip#show).</span></span> <span data-ttu-id="ffcbe-191">hello följande exempel hämtar hello IP-adress för *myPublicIP* skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-191">hello following example obtains hello IP address for *myPublicIP* created earlier:</span></span>

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="ffcbe-192">Du kan sedan ange hello offentliga IP-adressen i tooa webbläsare.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-192">You can then enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="ffcbe-193">Kom ihåg - det tar några minuter hello hello VMs toobe redo innan hello belastningsutjämnaren startar toodistribute trafik toothem.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-193">Remember - it takes a few minutes hello hello VMs toobe ready before hello load balancer starts toodistribute traffic toothem.</span></span> <span data-ttu-id="ffcbe-194">hello appen visas, inklusive hello värdnamnet för hello VM som hello belastningsutjämnaren distribuerade trafik tooas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-194">hello app is displayed, including hello hostname of hello VM that hello load balancer distributed traffic tooas in hello following example:</span></span>

![Node.js-app som körs](./media/tutorial-load-balancer/running-nodejs-app.png)

<span data-ttu-id="ffcbe-196">toosee hello belastningsutjämnare distribuerar trafik över alla tre virtuella datorer som kör appen, du kan framtvinga uppdatera webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-196">toosee hello load balancer distribute traffic across all three VMs running your app, you can force-refresh your web browser.</span></span>


## <a name="add-and-remove-vms"></a><span data-ttu-id="ffcbe-197">Lägga till och ta bort virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="ffcbe-197">Add and remove VMs</span></span>
<span data-ttu-id="ffcbe-198">Du kan behöva tooperform Underhåll på hello virtuella datorer som kör appen, till exempel installera uppdateringar av OS.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-198">You may need tooperform maintenance on hello VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="ffcbe-199">toodeal med ökat tooyour app som du kan behöva tooadd ytterligare virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-199">toodeal with increased traffic tooyour app, you may need tooadd additional VMs.</span></span> <span data-ttu-id="ffcbe-200">Det här avsnittet beskrivs hur du tooremove eller lägga till en virtuell dator från hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-200">This section shows you how tooremove or add a VM from hello load balancer.</span></span>

### <a name="remove-a-vm-from-hello-load-balancer"></a><span data-ttu-id="ffcbe-201">Ta bort en virtuell dator från hello belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-201">Remove a VM from hello load balancer</span></span>
<span data-ttu-id="ffcbe-202">Du kan ta bort en virtuell dator från hello backend-adresspool med [az nic ip-config-nätverksadresspool ta bort](/cli/azure/network/nic/ip-config/address-pool#remove).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-202">You can remove a VM from hello backend address pool with [az network nic ip-config address-pool remove](/cli/azure/network/nic/ip-config/address-pool#remove).</span></span> <span data-ttu-id="ffcbe-203">följande exempel tar bort hello hello virtuella nätverkskortet för **myVM2** från *myLoadBalancer*:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-203">hello following example removes hello virtual NIC for **myVM2** from *myLoadBalancer*:</span></span>

```azurecli-interactive 
az network nic ip-config address-pool remove \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

<span data-ttu-id="ffcbe-204">toosee hello belastningsutjämnare distribuerar trafik över hello återstående två virtuella datorer som kör appen du kan framtvinga uppdatera webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-204">toosee hello load balancer distribute traffic across hello remaining two VMs running your app you can force-refresh your web browser.</span></span> <span data-ttu-id="ffcbe-205">Du kan nu utföra underhåll på hello VM, till exempel installera uppdateringar av operativsystem eller utföra en VM-omstart.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-205">You can now perform maintenance on hello VM, such as installing OS updates or performing a VM reboot.</span></span>

### <a name="add-a-vm-toohello-load-balancer"></a><span data-ttu-id="ffcbe-206">Lägga till en belastningsutjämnare för VM-toohello</span><span class="sxs-lookup"><span data-stu-id="ffcbe-206">Add a VM toohello load balancer</span></span>
<span data-ttu-id="ffcbe-207">När du utför underhåll på VM, eller om du behöver tooexpand kapacitet kan du lägga till en VM toohello backend-adresspool med [az nic ip-config-nätverksadresspool lägga till](/cli/azure/network/nic/ip-config/address-pool#add).</span><span class="sxs-lookup"><span data-stu-id="ffcbe-207">After performing VM maintenance, or if you need tooexpand capacity, you can add a VM toohello backend address pool with [az network nic ip-config address-pool add](/cli/azure/network/nic/ip-config/address-pool#add).</span></span> <span data-ttu-id="ffcbe-208">hello följande exempel läggs hello virtuella nätverkskortet för **myVM2** för*myLoadBalancer*:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-208">hello following example adds hello virtual NIC for **myVM2** too*myLoadBalancer*:</span></span>

```azurecli-interactive 
az network nic ip-config address-pool add \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```


## <a name="next-steps"></a><span data-ttu-id="ffcbe-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ffcbe-209">Next steps</span></span>
<span data-ttu-id="ffcbe-210">I den här självstudiekursen, skapas en belastningsutjämnare och kopplade tooit för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-210">In this tutorial, you created a load balancer and attached VMs tooit.</span></span> <span data-ttu-id="ffcbe-211">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="ffcbe-211">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ffcbe-212">Skapa en Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-212">Create an Azure load balancer</span></span>
> * <span data-ttu-id="ffcbe-213">Skapa en belastningsutjämnaren, hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="ffcbe-213">Create a load balancer health probe</span></span>
> * <span data-ttu-id="ffcbe-214">Skapa regler för nätverkstrafik för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-214">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="ffcbe-215">Använda molntjänster init toocreate en grundläggande Node.js-app</span><span class="sxs-lookup"><span data-stu-id="ffcbe-215">Use cloud-init toocreate a basic Node.js app</span></span>
> * <span data-ttu-id="ffcbe-216">Skapa virtuella datorer och bifoga tooa belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-216">Create virtual machines and attach tooa load balancer</span></span>
> * <span data-ttu-id="ffcbe-217">Visa en belastningsutjämnare i praktiken</span><span class="sxs-lookup"><span data-stu-id="ffcbe-217">View a load balancer in action</span></span>
> * <span data-ttu-id="ffcbe-218">Lägga till och ta bort virtuella datorer från en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ffcbe-218">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="ffcbe-219">Avancera toohello nästa självstudiekurs toolearn mer om virtuella Azure-nätverkskomponenter.</span><span class="sxs-lookup"><span data-stu-id="ffcbe-219">Advance toohello next tutorial toolearn more about Azure virtual network components.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ffcbe-220">Hantera virtuella datorer och virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="ffcbe-220">Manage VMs and virtual networks</span></span>](tutorial-virtual-network.md)
