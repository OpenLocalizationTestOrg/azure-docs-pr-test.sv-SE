---
title: "aaaAzure virtuella nätverk och virtuella Linux-datorer | Microsoft Docs"
description: "Självstudiekurs – hantera virtuella Azure-nätverk och virtuella Linux-datorer med hello Azure CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 57e6bd4de16f0e31d53dc67bf50dc5730d43712b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-hello-azure-cli"></a><span data-ttu-id="07e6f-103">Hantera virtuella Azure-nätverk och virtuella Linux-datorer med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="07e6f-103">Manage Azure Virtual Networks and Linux Virtual Machines with hello Azure CLI</span></span>

<span data-ttu-id="07e6f-104">Azure virtual machines använder Azure-nätverk för interna och externa nätverkskommunikation.</span><span class="sxs-lookup"><span data-stu-id="07e6f-104">Azure virtual machines use Azure networking for internal and external network communication.</span></span> <span data-ttu-id="07e6f-105">Den här kursen går igenom hur du distribuerar två virtuella datorer och konfigurera Azure nätverk för dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="07e6f-105">This tutorial walks through deploying two virtual machines and configuring Azure networking for these VMs.</span></span> <span data-ttu-id="07e6f-106">hello exemplen i den här kursen förutsätter att hello virtuella datorer är värdar för ett webbprogram med en-databas, men ett program inte har distribuerats i hello självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="07e6f-106">hello examples in this tutorial assume that hello VMs are hosting a web application with a database back-end, however an application is not deployed in hello tutorial.</span></span> <span data-ttu-id="07e6f-107">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="07e6f-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="07e6f-108">Distribuera ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="07e6f-108">Deploy a virtual network</span></span>
> * <span data-ttu-id="07e6f-109">Skapa ett undernät i ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="07e6f-109">Create a subnet within a virtual network</span></span>
> * <span data-ttu-id="07e6f-110">Anslut virtuella datorer tooa undernät</span><span class="sxs-lookup"><span data-stu-id="07e6f-110">Attach virtual machines tooa subnet</span></span>
> * <span data-ttu-id="07e6f-111">Hantera virtuella offentliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="07e6f-111">Manage virtual machine public IP addresses</span></span>
> * <span data-ttu-id="07e6f-112">Skydda inkommande trafik för internet</span><span class="sxs-lookup"><span data-stu-id="07e6f-112">Secure incoming internet traffic</span></span>
> * <span data-ttu-id="07e6f-113">Skydda Virtuella tooVM trafik</span><span class="sxs-lookup"><span data-stu-id="07e6f-113">Secure VM tooVM traffic</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="07e6f-114">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="07e6f-114">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="07e6f-115">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="07e6f-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="07e6f-116">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="07e6f-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="vm-networking-overview"></a><span data-ttu-id="07e6f-117">VM-nätverk – översikt</span><span class="sxs-lookup"><span data-stu-id="07e6f-117">VM networking overview</span></span>

<span data-ttu-id="07e6f-118">Virtuella Azure-nätverk kan säkra nätverksanslutningar mellan virtuella datorer, hello internet och andra Azure-tjänster, till exempel Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="07e6f-118">Azure virtual networks enable secure network connections between virtual machines, hello internet, and other Azure services such as Azure SQL database.</span></span> <span data-ttu-id="07e6f-119">Virtuella nätverk är uppdelad i logiska segment som kallas subnät.</span><span class="sxs-lookup"><span data-stu-id="07e6f-119">Virtual networks are broken down into logical segments called subnets.</span></span> <span data-ttu-id="07e6f-120">Undernät används toocontrol nätverk flöde, och som en säkerhetsgräns.</span><span class="sxs-lookup"><span data-stu-id="07e6f-120">Subnets are used toocontrol network flow, and as a security boundary.</span></span> <span data-ttu-id="07e6f-121">När du distribuerar en virtuell dator innehåller vanligtvis ett virtuellt nätverksgränssnitt som är bifogade tooa undernät.</span><span class="sxs-lookup"><span data-stu-id="07e6f-121">When deploying a VM, it generally includes a virtual network interface, which is attached tooa subnet.</span></span>

## <a name="deploy-virtual-network"></a><span data-ttu-id="07e6f-122">Distribuera virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="07e6f-122">Deploy virtual network</span></span>

<span data-ttu-id="07e6f-123">Ett enda virtuellt nätverk skapas med två undernät för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="07e6f-123">For this tutorial, a single virtual network is created with two subnets.</span></span> <span data-ttu-id="07e6f-124">Ett frontend undernät som värd för ett webbprogram och ett backend-undernät som värd för en databasserver.</span><span class="sxs-lookup"><span data-stu-id="07e6f-124">A front-end subnet for hosting a web application, and a back-end subnet for hosting a database server.</span></span>

<span data-ttu-id="07e6f-125">Innan du kan skapa ett virtuellt nätverk, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="07e6f-125">Before you can create a virtual network, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="07e6f-126">hello följande exempel skapar en resursgrupp med namnet *myRGNetwork* hello eastus plats.</span><span class="sxs-lookup"><span data-stu-id="07e6f-126">hello following example creates a resource group named *myRGNetwork* in hello eastus location.</span></span>

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a><span data-ttu-id="07e6f-127">Skapa det virtuella nätverket</span><span class="sxs-lookup"><span data-stu-id="07e6f-127">Create virtual network</span></span>

<span data-ttu-id="07e6f-128">Oss hello [az network vnet skapa](/cli/azure/network/vnet#create) kommandot toocreate ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="07e6f-128">Us hello [az network vnet create](/cli/azure/network/vnet#create) command toocreate a virtual network.</span></span> <span data-ttu-id="07e6f-129">I det här exemplet hello nätverk med namnet *mvVnet* och ges en adressprefixet *10.0.0.0/16*.</span><span class="sxs-lookup"><span data-stu-id="07e6f-129">In this example, hello network is named *mvVnet* and is given an address prefix of *10.0.0.0/16*.</span></span> <span data-ttu-id="07e6f-130">Dessutom skapas ett undernät med namnet *mySubnetFrontEnd* och prefixet *10.0.1.0/24*.</span><span class="sxs-lookup"><span data-stu-id="07e6f-130">A subnet is also created with a name of *mySubnetFrontEnd* and a prefix of *10.0.1.0/24*.</span></span> <span data-ttu-id="07e6f-131">Senare i den här kursen är en frontend VM anslutna toothis undernät.</span><span class="sxs-lookup"><span data-stu-id="07e6f-131">Later in this tutorial a front-end VM is connected toothis subnet.</span></span> 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnetFrontEnd \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a><span data-ttu-id="07e6f-132">Skapa ett undernät</span><span class="sxs-lookup"><span data-stu-id="07e6f-132">Create subnet</span></span>

<span data-ttu-id="07e6f-133">Ett nytt undernät läggs toohello virtuellt nätverk med hello [az undernät för virtuellt nätverk skapa](/cli/azure/network/vnet/subnet#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="07e6f-133">A new subnet is added toohello virtual network using hello [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) command.</span></span> <span data-ttu-id="07e6f-134">I det här exemplet hello undernät med namnet *mySubnetBackEnd* och ges en adressprefixet *10.0.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="07e6f-134">In this example, hello subnet is named *mySubnetBackEnd* and is given an address prefix of *10.0.2.0/24*.</span></span> <span data-ttu-id="07e6f-135">Det här undernätet används med alla backend-tjänster.</span><span class="sxs-lookup"><span data-stu-id="07e6f-135">This subnet is used with all back-end services.</span></span>

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --address-prefix 10.0.2.0/24
```

<span data-ttu-id="07e6f-136">Ett nätverk har nu skapats och upp i två undernät, en för frontend-tjänster och en för backend-tjänster.</span><span class="sxs-lookup"><span data-stu-id="07e6f-136">At this point, a network has been created and segmented into two subnets, one for front-end services, and another for back-end services.</span></span> <span data-ttu-id="07e6f-137">I nästa avsnitt om hello, virtuella datorer skapas och anslutna toothese undernät.</span><span class="sxs-lookup"><span data-stu-id="07e6f-137">In hello next section, virtual machines are created and connected toothese subnets.</span></span>

## <a name="understand-public-ip-address"></a><span data-ttu-id="07e6f-138">Förstå offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="07e6f-138">Understand public IP address</span></span>

<span data-ttu-id="07e6f-139">En offentlig IP-adress kan toobe Azure-resurser tillgängliga på hello internet.</span><span class="sxs-lookup"><span data-stu-id="07e6f-139">A public IP address allows Azure resources toobe accessible on hello internet.</span></span> <span data-ttu-id="07e6f-140">I det här avsnittet av kursen hello skapas en virtuell dator toodemonstrate hur toowork med en offentlig IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="07e6f-140">In this section of hello tutorial, a VM is created toodemonstrate how toowork with public IP addresses.</span></span>

### <a name="allocation-method"></a><span data-ttu-id="07e6f-141">Allokeringsmetod</span><span class="sxs-lookup"><span data-stu-id="07e6f-141">Allocation method</span></span>

<span data-ttu-id="07e6f-142">En offentlig IP-adress kan fördelas som dynamiska eller statiska.</span><span class="sxs-lookup"><span data-stu-id="07e6f-142">A public IP address can be allocated as either dynamic or static.</span></span> <span data-ttu-id="07e6f-143">Offentliga IP-adressen tilldelas dynamiskt som standard.</span><span class="sxs-lookup"><span data-stu-id="07e6f-143">By default, public IP address dynamically allocated.</span></span> <span data-ttu-id="07e6f-144">Dynamiska IP-adresser släpps när en virtuell dator har frigjorts.</span><span class="sxs-lookup"><span data-stu-id="07e6f-144">Dynamic IP addresses are released when a VM is deallocated.</span></span> <span data-ttu-id="07e6f-145">Detta medför hello IP-adress toochange under åtgärder som innehåller en VM-flyttningen.</span><span class="sxs-lookup"><span data-stu-id="07e6f-145">This behavior causes hello IP address toochange during any operation that includes a VM deallocation.</span></span>

<span data-ttu-id="07e6f-146">hello allokeringsmetod kan ställas in toostatic, vilket säkerställer att hello IP-adress vara tilldelad tooa VM, även under en frigjord tillstånd.</span><span class="sxs-lookup"><span data-stu-id="07e6f-146">hello allocation method can be set toostatic, which ensures that hello IP address remain assigned tooa VM, even during a deallocated state.</span></span> <span data-ttu-id="07e6f-147">När du använder en statiskt tilldelade IP-adress, kan hello själva IP-adressen inte anges.</span><span class="sxs-lookup"><span data-stu-id="07e6f-147">When using a statically allocated IP address, hello IP address itself cannot be specified.</span></span> <span data-ttu-id="07e6f-148">Istället tilldelas den från en pool med tillgängliga adresser.</span><span class="sxs-lookup"><span data-stu-id="07e6f-148">Instead, it is allocated from a pool of available addresses.</span></span>

### <a name="dynamic-allocation"></a><span data-ttu-id="07e6f-149">Dynamisk allokering</span><span class="sxs-lookup"><span data-stu-id="07e6f-149">Dynamic allocation</span></span>

<span data-ttu-id="07e6f-150">När du skapar en virtuell dator med hello [az vm skapa](/cli/azure/vm#create) kommandot hello standard offentliga IP-adress allokeringsmetoden är dynamisk.</span><span class="sxs-lookup"><span data-stu-id="07e6f-150">When creating a VM with hello [az vm create](/cli/azure/vm#create) command, hello default public IP address allocation method is dynamic.</span></span> <span data-ttu-id="07e6f-151">I följande exempel hello, skapas en virtuell dator med en dynamisk IP-adress.</span><span class="sxs-lookup"><span data-stu-id="07e6f-151">In hello following example, a VM is created with a dynamic IP address.</span></span> 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myFrontEndVM \
  --vnet-name myVnet \
  --subnet mySubnetFrontEnd \
  --nsg myNSGFrontEnd \
  --public-ip-address myFrontEndIP \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="static-allocation"></a><span data-ttu-id="07e6f-152">Statisk tilldelning</span><span class="sxs-lookup"><span data-stu-id="07e6f-152">Static allocation</span></span>

<span data-ttu-id="07e6f-153">När du skapar en virtuell dator med hello [az vm skapa](/cli/azure/vm#create) kommandot, inkludera hello `--public-ip-address-allocation static` argumentet tooassign en statisk offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="07e6f-153">When creating a virtual machine using hello [az vm create](/cli/azure/vm#create) command, include hello `--public-ip-address-allocation static` argument tooassign a static public IP address.</span></span> <span data-ttu-id="07e6f-154">Den här åtgärden visas inte i den här kursen, men i nästa avsnitt om hello en dynamiskt tilldelad IP-adress är ändrade tooa statiskt tilldelade adressen.</span><span class="sxs-lookup"><span data-stu-id="07e6f-154">This operation is not demonstrated in this tutorial, however in hello next section a dynamically allocated IP address is changed tooa statically allocated address.</span></span> 

### <a name="change-allocation-method"></a><span data-ttu-id="07e6f-155">Ändra allokeringsmetod</span><span class="sxs-lookup"><span data-stu-id="07e6f-155">Change allocation method</span></span>

<span data-ttu-id="07e6f-156">hello allokeringsmetod för IP-adress kan ändras med hello [az nätverket offentliga ip-uppdatering](/cli/azure/network/public-ip#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="07e6f-156">hello IP address allocation method can be changed using hello [az network public-ip update](/cli/azure/network/public-ip#update) command.</span></span> <span data-ttu-id="07e6f-157">I det här exemplet hello allokeringsmetod för IP-adress för hello frontend VM ändras toostatic.</span><span class="sxs-lookup"><span data-stu-id="07e6f-157">In this example, hello IP address allocation method of hello front-end VM is changed toostatic.</span></span>

<span data-ttu-id="07e6f-158">Först frigöra hello VM.</span><span class="sxs-lookup"><span data-stu-id="07e6f-158">First, deallocate hello VM.</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontEndVM
```

<span data-ttu-id="07e6f-159">Använd hello [az nätverket offentliga ip-uppdatering](/cli/azure/network/public-ip#update) kommandot tooupdate hello allokeringsmetod.</span><span class="sxs-lookup"><span data-stu-id="07e6f-159">Use hello [az network public-ip update](/cli/azure/network/public-ip#update) command tooupdate hello allocation method.</span></span> <span data-ttu-id="07e6f-160">I det här fallet hello `--allocation-method` anges för*statiska*.</span><span class="sxs-lookup"><span data-stu-id="07e6f-160">In this case, hello `--allocation-method` is being set too*static*.</span></span>

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myFrontEndIP --allocation-method static
```

<span data-ttu-id="07e6f-161">Starta hello VM.</span><span class="sxs-lookup"><span data-stu-id="07e6f-161">Start hello VM.</span></span>

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontEndVM --no-wait
```

### <a name="no-public-ip-address"></a><span data-ttu-id="07e6f-162">Ingen offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="07e6f-162">No public IP address</span></span>

<span data-ttu-id="07e6f-163">Ofta är en virtuell dator måste inte toobe nås över hello internet.</span><span class="sxs-lookup"><span data-stu-id="07e6f-163">Often, a VM does not need toobe accessible over hello internet.</span></span> <span data-ttu-id="07e6f-164">toocreate en virtuell dator utan en offentlig IP-adress, Använd hello `--public-ip-address ""` argument med en tom uppsättning med dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="07e6f-164">toocreate a VM without a public IP address, use hello `--public-ip-address ""` argument with an empty set of double quotes.</span></span> <span data-ttu-id="07e6f-165">Den här konfigurationen visas senare i den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="07e6f-165">This configuration is demonstrated later in this tutorial</span></span>

## <a name="secure-network-traffic"></a><span data-ttu-id="07e6f-166">Skydda nätverkstrafik</span><span class="sxs-lookup"><span data-stu-id="07e6f-166">Secure network traffic</span></span>

<span data-ttu-id="07e6f-167">En nätverkssäkerhetsgrupp (NSG) innehåller en lista över säkerhetsregler som tillåter eller nekar nätverket trafik tooresources anslutna tooAzure virtuella nätverk (VNet).</span><span class="sxs-lookup"><span data-stu-id="07e6f-167">A network security group (NSG) contains a list of security rules that allow or deny network traffic tooresources connected tooAzure Virtual Networks (VNet).</span></span> <span data-ttu-id="07e6f-168">NSG: er kan vara associerade toosubnets eller enskilda nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="07e6f-168">NSGs can be associated toosubnets or individual network interfaces.</span></span> <span data-ttu-id="07e6f-169">När en NSG är associerad med ett nätverksgränssnitt gäller endast hello associerade VM.</span><span class="sxs-lookup"><span data-stu-id="07e6f-169">When an NSG is associated with a network interface, it applies only hello associated VM.</span></span> <span data-ttu-id="07e6f-170">När en NSG är associerad tooa undernät, tillämpas hello reglerna tooall resurser anslutna toohello undernät.</span><span class="sxs-lookup"><span data-stu-id="07e6f-170">When an NSG is associated tooa subnet, hello rules apply tooall resources connected toohello subnet.</span></span> 

### <a name="network-security-group-rules"></a><span data-ttu-id="07e6f-171">Regler för nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="07e6f-171">Network security group rules</span></span>

<span data-ttu-id="07e6f-172">NSG-regler definiera nätverk portar under vilken trafik tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="07e6f-172">NSG rules define networking ports over which traffic is allowed or denied.</span></span> <span data-ttu-id="07e6f-173">hello-regler kan innehålla käll- och IP-adressintervall så att trafik styrs mellan specifika system eller undernät.</span><span class="sxs-lookup"><span data-stu-id="07e6f-173">hello rules can include source and destination IP address ranges so that traffic is controlled between specific systems or subnets.</span></span> <span data-ttu-id="07e6f-174">NSG-regler kan även innehålla en prioritet (mellan 1 – och 4096).</span><span class="sxs-lookup"><span data-stu-id="07e6f-174">NSG rules also include a priority (between 1—and 4096).</span></span> <span data-ttu-id="07e6f-175">Reglerna utvärderas hello efter prioritet.</span><span class="sxs-lookup"><span data-stu-id="07e6f-175">Rules are evaluated in hello order of priority.</span></span> <span data-ttu-id="07e6f-176">En regel med en prioritet på 100 utvärderas innan en regel med prioritet 200.</span><span class="sxs-lookup"><span data-stu-id="07e6f-176">A rule with a priority of 100 is evaluated before a rule with priority 200.</span></span>

<span data-ttu-id="07e6f-177">Alla NSG:er har en uppsättning standardregler.</span><span class="sxs-lookup"><span data-stu-id="07e6f-177">All NSGs contain a set of default rules.</span></span> <span data-ttu-id="07e6f-178">hello standardreglerna kan inte tas bort, men eftersom de har tilldelats lägst prioritet för hello, de kan åsidosättas med hello regler som du skapar.</span><span class="sxs-lookup"><span data-stu-id="07e6f-178">hello default rules cannot be deleted, but because they are assigned hello lowest priority, they can be overridden by hello rules that you create.</span></span>

- <span data-ttu-id="07e6f-179">**Virtuellt nätverk** - trafik med ursprung och slutar med ett virtuellt nätverk tillåts både i inkommande och utgående riktningar.</span><span class="sxs-lookup"><span data-stu-id="07e6f-179">**Virtual network** - Traffic originating and ending in a virtual network is allowed both in inbound and outbound directions.</span></span>
- <span data-ttu-id="07e6f-180">**Internet** - utgående trafik tillåts, men inkommande trafik blockeras.</span><span class="sxs-lookup"><span data-stu-id="07e6f-180">**Internet** - Outbound traffic is allowed, but inbound traffic is blocked.</span></span>
- <span data-ttu-id="07e6f-181">**Belastningsutjämnaren** -Tillåt Azure belastningen belastningsutjämnaren tooprobe hello hälsotillståndet för dina virtuella datorer och rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="07e6f-181">**Load balancer** - Allow Azure’s load balancer tooprobe hello health of your VMs and role instances.</span></span> <span data-ttu-id="07e6f-182">Om du inte använder en belastningsutjämnad uppsättning, kan du åsidosätta den här regeln.</span><span class="sxs-lookup"><span data-stu-id="07e6f-182">If you are not using a load balanced set, you can override this rule.</span></span>

### <a name="create-network-security-groups"></a><span data-ttu-id="07e6f-183">Skapa säkerhetsgrupper för nätverk</span><span class="sxs-lookup"><span data-stu-id="07e6f-183">Create network security groups</span></span>

<span data-ttu-id="07e6f-184">En nätverkssäkerhetsgrupp kan skapas på hello samma tid som en virtuell dator med hjälp av hello [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="07e6f-184">A network security group can be created at hello same time as a VM using hello [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="07e6f-185">När du gör det hello NSG är associerad med hello nätverksgränssnitt för virtuella datorer och en NSG-regel är skapas automatiskt tooallow trafik på port *22* från andra källor.</span><span class="sxs-lookup"><span data-stu-id="07e6f-185">When doing so, hello NSG is associated with hello VMs network interface and an NSG rule is auto created tooallow traffic on port *22* from any source.</span></span> <span data-ttu-id="07e6f-186">Tidigare i den här självstudien hello hello frontend NSG har skapats automatiskt med frontend VM.</span><span class="sxs-lookup"><span data-stu-id="07e6f-186">Earlier in this tutorial, hello front-end NSG was auto-created with hello front-end VM.</span></span> <span data-ttu-id="07e6f-187">En regel för NSG har också automatiskt skapat för port 22.</span><span class="sxs-lookup"><span data-stu-id="07e6f-187">An NSG rule was also auto created for port 22.</span></span> 

<span data-ttu-id="07e6f-188">I vissa fall kan det vara bra toopre-skapa en NSG till exempel när standardreglerna för SSH inte skapas eller när hello NSG ska vara anslutna tooa undernät.</span><span class="sxs-lookup"><span data-stu-id="07e6f-188">In some cases, it may be helpful toopre-create an NSG, such as when default SSH rules should not be created, or when hello NSG should be attached tooa subnet.</span></span> 

<span data-ttu-id="07e6f-189">Använd hello [az nätverket nsg skapa](/cli/azure/network/nsg#create) kommandot toocreate en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="07e6f-189">Use hello [az network nsg create](/cli/azure/network/nsg#create) command toocreate a network security group.</span></span>

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myNSGBackEnd
```

<span data-ttu-id="07e6f-190">I stället för att associera hello NSG tooa nätverksgränssnitt, är den associerad med ett undernät.</span><span class="sxs-lookup"><span data-stu-id="07e6f-190">Instead of associating hello NSG tooa network interface, it is associated with a subnet.</span></span> <span data-ttu-id="07e6f-191">I den här konfigurationen ärver någon virtuell dator som är bifogade toohello undernät hello NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="07e6f-191">In this configuration, any VM that is attached toohello subnet inherits hello NSG rules.</span></span>

<span data-ttu-id="07e6f-192">Uppdatera hello befintligt undernät med namnet *mySubnetBackEnd* med hello ny NSG.</span><span class="sxs-lookup"><span data-stu-id="07e6f-192">Update hello existing subnet named *mySubnetBackEnd* with hello new NSG.</span></span>

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --network-security-group myNSGBackEnd
```

<span data-ttu-id="07e6f-193">Nu skapa en virtuell dator som är bifogade toohello *mySubnetBackEnd*.</span><span class="sxs-lookup"><span data-stu-id="07e6f-193">Now create a virtual machine, which is attached toohello *mySubnetBackEnd*.</span></span> <span data-ttu-id="07e6f-194">Observera att hello `--nsg` argumentet har värdet tomt dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="07e6f-194">Notice that hello `--nsg` argument has a value of empty double quotes.</span></span> <span data-ttu-id="07e6f-195">En NSG behöver inte toobe som skapats med hello VM.</span><span class="sxs-lookup"><span data-stu-id="07e6f-195">An NSG does not need toobe created with hello VM.</span></span> <span data-ttu-id="07e6f-196">hello VM är anslutna toohello backend-undernät, som är skyddat med hello förskapade backend-NSG.</span><span class="sxs-lookup"><span data-stu-id="07e6f-196">hello VM is attached toohello back-end subnet, which is protected with hello pre-created back-end NSG.</span></span> <span data-ttu-id="07e6f-197">Den här NSG gäller toohello VM.</span><span class="sxs-lookup"><span data-stu-id="07e6f-197">This NSG applies toohello VM.</span></span> <span data-ttu-id="07e6f-198">Observera också här den hello `--public-ip-address` argumentet har värdet tomt dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="07e6f-198">Also, notice here that hello `--public-ip-address` argument has a value of empty double quotes.</span></span> <span data-ttu-id="07e6f-199">Den här konfigurationen skapar en virtuell dator utan en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="07e6f-199">This configuration creates a VM without a public IP address.</span></span> 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myBackEndVM \
  --vnet-name myVnet \
  --subnet mySubnetBackEnd \
  --public-ip-address "" \
  --nsg "" \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="secure-incoming-traffic"></a><span data-ttu-id="07e6f-200">Skydda inkommande trafik</span><span class="sxs-lookup"><span data-stu-id="07e6f-200">Secure incoming traffic</span></span>

<span data-ttu-id="07e6f-201">När hello frontend VM skapades skapades en NSG regeln tooallow inkommande trafik på port 22.</span><span class="sxs-lookup"><span data-stu-id="07e6f-201">When hello front-end VM was created, an NSG rule was created tooallow incoming traffic on port 22.</span></span> <span data-ttu-id="07e6f-202">Den här regeln kan SSH-anslutningar toohello VM.</span><span class="sxs-lookup"><span data-stu-id="07e6f-202">This rule allows SSH connections toohello VM.</span></span> <span data-ttu-id="07e6f-203">I det här exemplet trafik även ska tillåtas på port *80*.</span><span class="sxs-lookup"><span data-stu-id="07e6f-203">For this example, traffic should also be allowed on port *80*.</span></span> <span data-ttu-id="07e6f-204">Den här konfigurationen kan en web application toobe åt på hello VM.</span><span class="sxs-lookup"><span data-stu-id="07e6f-204">This configuration allows a web application toobe accessed on hello VM.</span></span>

<span data-ttu-id="07e6f-205">Använd hello [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) kommandot toocreate en regel för port *80*.</span><span class="sxs-lookup"><span data-stu-id="07e6f-205">Use hello [az network nsg rule create](/cli/azure/network/nsg/rule#create) command toocreate a rule for port *80*.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGFrontEnd \
  --name http \
  --access allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80
```

<span data-ttu-id="07e6f-206">hello frontend virtuella datorn är nu endast tillgänglig på port *22* och port *80*.</span><span class="sxs-lookup"><span data-stu-id="07e6f-206">hello front-end VM is now only accessible on port *22* and port *80*.</span></span> <span data-ttu-id="07e6f-207">All annan inkommande trafik blockeras på hello nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="07e6f-207">All other incoming traffic is blocked at hello network security group.</span></span> <span data-ttu-id="07e6f-208">Det kan vara användbara toovisualize hello NSG konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="07e6f-208">It may be helpful toovisualize hello NSG rule configurations.</span></span> <span data-ttu-id="07e6f-209">Returnerar hello NSG regelkonfigurationen med hello [az nätverket regellistan](/cli/azure/network/nsg/rule#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="07e6f-209">Return hello NSG rule configuration with hello [az network rule list](/cli/azure/network/nsg/rule#list) command.</span></span> 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGFrontEnd --output table
```

<span data-ttu-id="07e6f-210">Resultat:</span><span class="sxs-lookup"><span data-stu-id="07e6f-210">Output:</span></span>

```azurecli-interactive 
Access    DestinationAddressPrefix      DestinationPortRange  Direction    Name                 Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -----------------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                                               22  Inbound      default-allow-ssh        1000  Tcp         Succeeded            myRGNetwork      *                      *
Allow     *                                               80  Inbound      http                      200  Tcp         Succeeded            myRGNetwork      *                      *
```

### <a name="secure-vm-toovm-traffic"></a><span data-ttu-id="07e6f-211">Skydda Virtuella tooVM trafik</span><span class="sxs-lookup"><span data-stu-id="07e6f-211">Secure VM tooVM traffic</span></span>

<span data-ttu-id="07e6f-212">Regler för nätverkssäkerhetsgrupper kan också använda mellan virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="07e6f-212">Network security group rules can also apply between VMs.</span></span> <span data-ttu-id="07e6f-213">I det här exemplet hello hello frontend VM måste toocommunicate med backend-VM på port *22* och *3306*.</span><span class="sxs-lookup"><span data-stu-id="07e6f-213">For this example, hello front-end VM needs toocommunicate with hello back-end VM on port *22* and *3306*.</span></span> <span data-ttu-id="07e6f-214">Den här konfigurationen tillåter SSH-anslutningar från hello frontend VM och även att ett program på hello frontend VM toocommunicate med en backend-MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="07e6f-214">This configuration allows SSH connections from hello front-end VM, and also allow an application on hello front-end VM toocommunicate with a back-end MySQL database.</span></span> <span data-ttu-id="07e6f-215">All annan trafik ska blockeras mellan hello frontend- och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="07e6f-215">All other traffic should be blocked between hello front-end and back-end virtual machines.</span></span>

<span data-ttu-id="07e6f-216">Använd hello [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) kommandot toocreate en regel för port 22.</span><span class="sxs-lookup"><span data-stu-id="07e6f-216">Use hello [az network nsg rule create](/cli/azure/network/nsg/rule#create) command toocreate a rule for port 22.</span></span> <span data-ttu-id="07e6f-217">Observera att hello `--source-address-prefix` argumentet anger ett värde för *10.0.1.0/24*.</span><span class="sxs-lookup"><span data-stu-id="07e6f-217">Notice that hello `--source-address-prefix` argument specifies a value of *10.0.1.0/24*.</span></span> <span data-ttu-id="07e6f-218">Den här konfigurationen säkerställer att endast trafik från hello frontend undernät tillåts via hello NSG.</span><span class="sxs-lookup"><span data-stu-id="07e6f-218">This configuration ensures that only traffic from hello front-end subnet is allowed through hello NSG.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name SSH \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "22"
```

<span data-ttu-id="07e6f-219">Lägg till regel för MySQL-trafik på port 3306.</span><span class="sxs-lookup"><span data-stu-id="07e6f-219">Now add a rule for MySQL traffic on port 3306.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name MySQL \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "3306"
```

<span data-ttu-id="07e6f-220">Slutligen eftersom NSG: er har en standard regel all trafik mellan virtuella datorer i hello samma virtuella nätverk, en regel kan skapas för hello backend-NSG: er tooblock all trafik.</span><span class="sxs-lookup"><span data-stu-id="07e6f-220">Finally, because NSGs have a default rule allowing all traffic between VMs in hello same VNet, a rule can be created for hello back-end NSGs tooblock all traffic.</span></span> <span data-ttu-id="07e6f-221">Observera att hello `--priority` ges värdet *300*, vilket är lägre både hello NSG och MySQL regler.</span><span class="sxs-lookup"><span data-stu-id="07e6f-221">Notice here that hello `--priority` is given a value of *300*, which is lower that both hello NSG and MySQL rules.</span></span> <span data-ttu-id="07e6f-222">Den här konfigurationen garanterar att SSH och MySQL trafiken fortfarande tillåts hello NSG.</span><span class="sxs-lookup"><span data-stu-id="07e6f-222">This configuration ensures that SSH and MySQL traffic is still allowed through hello NSG.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name denyAll \
  --access Deny \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "*"
```

<span data-ttu-id="07e6f-223">hello backend-VM är nu endast tillgänglig på port *22* och port *3306* från hello frontend undernät.</span><span class="sxs-lookup"><span data-stu-id="07e6f-223">hello back-end VM is now only accessible on port *22* and port *3306* from hello front-end subnet.</span></span> <span data-ttu-id="07e6f-224">All annan inkommande trafik blockeras på hello nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="07e6f-224">All other incoming traffic is blocked at hello network security group.</span></span> <span data-ttu-id="07e6f-225">Det kan vara användbara toovisualize hello NSG konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="07e6f-225">It may be helpful toovisualize hello NSG rule configurations.</span></span> <span data-ttu-id="07e6f-226">Returnerar hello NSG regelkonfigurationen med hello [az nätverket regellistan](/cli/azure/network/nsg/rule#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="07e6f-226">Return hello NSG rule configuration with hello [az network rule list](/cli/azure/network/nsg/rule#list) command.</span></span> 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGBackEnd --output table
```

<span data-ttu-id="07e6f-227">Resultat:</span><span class="sxs-lookup"><span data-stu-id="07e6f-227">Output:</span></span>

```azurecli-interactive 
Access    DestinationAddressPrefix    DestinationPortRange    Direction    Name       Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                           22                      Inbound      SSH             100  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Allow     *                           3306                    Inbound      MySQL           200  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Deny      *                           *                       Inbound      denyAll         300  Tcp         Succeeded            myRGNetwork      *                      *
```

## <a name="next-steps"></a><span data-ttu-id="07e6f-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="07e6f-228">Next steps</span></span>

<span data-ttu-id="07e6f-229">I den här självstudiekursen skapas och skyddas av Azure-nätverk som relaterade toovirtual datorer.</span><span class="sxs-lookup"><span data-stu-id="07e6f-229">In this tutorial, you created and secured Azure networks as related toovirtual machines.</span></span> <span data-ttu-id="07e6f-230">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="07e6f-230">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="07e6f-231">Distribuera ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="07e6f-231">Deploy a virtual network</span></span>
> * <span data-ttu-id="07e6f-232">Skapa ett undernät i ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="07e6f-232">Create a subnet within a virtual network</span></span>
> * <span data-ttu-id="07e6f-233">Anslut virtuella datorer tooa undernät</span><span class="sxs-lookup"><span data-stu-id="07e6f-233">Attach virtual machines tooa subnet</span></span>
> * <span data-ttu-id="07e6f-234">Hantera virtuella offentliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="07e6f-234">Manage virtual machine public IP addresses</span></span>
> * <span data-ttu-id="07e6f-235">Skydda inkommande trafik för internet</span><span class="sxs-lookup"><span data-stu-id="07e6f-235">Secure incoming internet traffic</span></span>
> * <span data-ttu-id="07e6f-236">Skydda Virtuella tooVM trafik</span><span class="sxs-lookup"><span data-stu-id="07e6f-236">Secure VM tooVM traffic</span></span>

<span data-ttu-id="07e6f-237">Avancera toohello nästa självstudiekurs toolearn om hur du skyddar data på virtuella datorer med Azure backup.</span><span class="sxs-lookup"><span data-stu-id="07e6f-237">Advance toohello next tutorial toolearn about securing data on virtual machines using Azure backup.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="07e6f-238">Säkerhetskopiera virtuella Linux-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="07e6f-238">Back up Linux virtual machines in Azure</span></span>](./tutorial-backup-vms.md)
