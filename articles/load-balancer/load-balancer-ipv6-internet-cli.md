---
title: "aaaCreate en Internetriktade belastningsutjämnare med IPv6 - Azure CLI | Microsoft Docs"
description: "Lär dig hur toocreate ett Internet belastningsutjämnare med IPv6 i Azure Resource Manager med hjälp av hello Azure CLI"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
keywords: "IPv6, azure belastningsutjämnare, dual stack, offentlig IP-adress, inbyggd ipv6, mobil, iot"
ms.assetid: a1957c9c-9c1d-423e-9d5c-d71449bc1f37
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 7ff75ac90d74a74e3d0c27672b36fbd955a086a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-hello-azure-cli"></a><span data-ttu-id="f71c1-104">Skapa en Internetuppkopplad belastningsutjämnare med IPv6 i Azure Resource Manager med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f71c1-104">Create an Internet facing load balancer with IPv6 in Azure Resource Manager using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f71c1-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f71c1-105">PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
> * [<span data-ttu-id="f71c1-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f71c1-106">Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
> * [<span data-ttu-id="f71c1-107">Mall</span><span class="sxs-lookup"><span data-stu-id="f71c1-107">Template</span></span>](load-balancer-ipv6-internet-template.md)

<span data-ttu-id="f71c1-108">En Azure belastningsutjämnare är en Layer 4-belastningsutjämnare (TCP, UDP).</span><span class="sxs-lookup"><span data-stu-id="f71c1-108">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer.</span></span> <span data-ttu-id="f71c1-109">hello belastningsutjämnare ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfri tjänstinstanser i molntjänster eller virtuella datorer i en belastningen belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="f71c1-109">hello load balancer provides high availability by distributing incoming traffic among healthy service instances in cloud services or virtual machines in a load balancer set.</span></span> <span data-ttu-id="f71c1-110">Azure belastningsutjämnare kan även presentera dessa tjänster på flera portar, flera IP-adresser eller både och.</span><span class="sxs-lookup"><span data-stu-id="f71c1-110">Azure Load Balancer can also present those services on multiple ports, multiple IP addresses, or both.</span></span>

## <a name="example-deployment-scenario"></a><span data-ttu-id="f71c1-111">Exempelscenario för distribution</span><span class="sxs-lookup"><span data-stu-id="f71c1-111">Example deployment scenario</span></span>

<span data-ttu-id="f71c1-112">hello följande diagram illustrerar hello nätverkslösning för belastningsutjämning som distribueras med hjälp av hello exempelmall som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f71c1-112">hello following diagram illustrates hello load balancing solution being deployed using hello example template described in this article.</span></span>

![Belastningsutjämningsscenario](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

<span data-ttu-id="f71c1-114">I det här scenariot skapar du hello följande Azure-resurser:</span><span class="sxs-lookup"><span data-stu-id="f71c1-114">In this scenario you will create hello following Azure resources:</span></span>

* <span data-ttu-id="f71c1-115">två virtuella datorer (VM)</span><span class="sxs-lookup"><span data-stu-id="f71c1-115">two virtual machines (VMs)</span></span>
* <span data-ttu-id="f71c1-116">ett virtuellt nätverksgränssnitt för varje virtuell dator med både IPv4 och IPv6-adresser som tilldelats</span><span class="sxs-lookup"><span data-stu-id="f71c1-116">a virtual network interface for each VM with both IPv4 and IPv6 addresses assigned</span></span>
* <span data-ttu-id="f71c1-117">en Internetriktade belastningsutjämnare med en IPv4 och IPv6-offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="f71c1-117">an Internet-facing Load Balancer with an IPv4 and an IPv6 Public IP address</span></span>
* <span data-ttu-id="f71c1-118">en Tillgänglighetsuppsättning toothat innehåller hello två virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="f71c1-118">an Availability Set toothat contains hello two VMs</span></span>
* <span data-ttu-id="f71c1-119">två läsa in belastningsutjämning regler toomap hello offentliga VIP toohello privata slutpunkter</span><span class="sxs-lookup"><span data-stu-id="f71c1-119">two load balancing rules toomap hello public VIPs toohello private endpoints</span></span>

## <a name="deploying-hello-solution-using-hello-azure-cli"></a><span data-ttu-id="f71c1-120">Distribuera hello-lösning med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f71c1-120">Deploying hello solution using hello Azure CLI</span></span>

<span data-ttu-id="f71c1-121">hello följande steg visar hur toocreate Internet-riktade belastningsutjämnaren med Azure Resource Manager med CLI.</span><span class="sxs-lookup"><span data-stu-id="f71c1-121">hello following steps show how toocreate an Internet facing load balancer using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="f71c1-122">Med Azure Resource Manager varje resurs har skapats och konfigurerats individuellt och sedan sätta ihop toocreate en resurs.</span><span class="sxs-lookup"><span data-stu-id="f71c1-122">With Azure Resource Manager, each resource is created and configured individually, and then put together toocreate a resource.</span></span>

<span data-ttu-id="f71c1-123">toodeploy en belastningsutjämnare som du skapar och konfigurerar hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="f71c1-123">toodeploy a load balancer, you create and configure hello following objects:</span></span>

* <span data-ttu-id="f71c1-124">IP-konfiguration på klientsidan – innehåller offentliga IP-adresser för inkommande nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="f71c1-124">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="f71c1-125">Backend-adresspool - innehåller nätverksgränssnitt (NIC) för virtuella datorer hello tooreceive nätverkstrafik från hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="f71c1-125">Back-end address pool - contains network interfaces (NICs) for hello virtual machines tooreceive network traffic from hello load balancer.</span></span>
* <span data-ttu-id="f71c1-126">Belastningsutjämning regler - innehåller regler för mappning av en offentlig port på hello belastningen belastningsutjämnaren tooport i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="f71c1-126">Load balancing rules - contains rules mapping a public port on hello load balancer tooport in hello back-end address pool.</span></span>
* <span data-ttu-id="f71c1-127">Inkommande NAT-regler - innehåller regler för mappning av en offentlig port på hello belastningen belastningsutjämnaren tooa port för en specifik virtuell dator i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="f71c1-127">Inbound NAT rules - contains rules mapping a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool.</span></span>
* <span data-ttu-id="f71c1-128">Avsökningar - innehåller hälsa-avsökningar som används toocheck tillgängligheten för virtuella datorer instanser i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="f71c1-128">Probes - contains health probes used toocheck availability of virtual machines instances in hello back-end address pool.</span></span>

<span data-ttu-id="f71c1-129">Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="f71c1-129">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-your-cli-environment-toouse-azure-resource-manager"></a><span data-ttu-id="f71c1-130">Ställ in din CLI miljö toouse Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f71c1-130">Set up your CLI environment toouse Azure Resource Manager</span></span>

<span data-ttu-id="f71c1-131">I det här exemplet kör vi hello CLI-verktygen i PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="f71c1-131">For this example, we are running hello CLI tools in a PowerShell command window.</span></span> <span data-ttu-id="f71c1-132">Vi använder inte hello Azure PowerShell-cmdlets, men vi använder PowerShells scripting funktioner tooimprove läsbarhet och återanvändning.</span><span class="sxs-lookup"><span data-stu-id="f71c1-132">We are not using hello Azure PowerShell cmdlets but we use PowerShell's scripting capabilities tooimprove readability and reuse.</span></span>

1. <span data-ttu-id="f71c1-133">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f71c1-133">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="f71c1-134">Kör hello **azure config mode** kommandot tooswitch tooResource Manager-läget.</span><span class="sxs-lookup"><span data-stu-id="f71c1-134">Run hello **azure config mode** command tooswitch tooResource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="f71c1-135">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="f71c1-135">Expected output:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="f71c1-136">Logga in tooAzure och hämta en lista över prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="f71c1-136">Sign in tooAzure and get a list of subscriptions.</span></span>

    ```azurecli
    azure login
    ```

    <span data-ttu-id="f71c1-137">Ange dina autentiseringsuppgifter för Azure när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="f71c1-137">Enter your Azure credentials when prompted.</span></span>

    ```azurecli
    azure account list
    ```

    <span data-ttu-id="f71c1-138">Välj hello-prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="f71c1-138">Pick hello subscription you want toouse.</span></span> <span data-ttu-id="f71c1-139">Anteckna hello prenumerations-Id för hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="f71c1-139">Make note of hello subscription Id for hello next step.</span></span>

4. <span data-ttu-id="f71c1-140">Ställ in PowerShell variabler för användning med hello CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="f71c1-140">Set up PowerShell variables for use with hello CLI commands.</span></span>

    ```powershell
    $subscriptionid = "########-####-####-####-############"  # enter subscription id
    $location = "southcentralus"
    $rgName = "pscontosorg1southctrlus09152016"
    $vnetName = "contosoIPv4Vnet"
    $vnetPrefix = "10.0.0.0/16"
    $subnet1Name = "clicontosoIPv4Subnet1"
    $subnet1Prefix = "10.0.0.0/24"
    $subnet2Name = "clicontosoIPv4Subnet2"
    $subnet2Prefix = "10.0.1.0/24"
    $dnsLabel = "contoso09152016"
    $lbName = "myIPv4IPv6Lb"
    ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a><span data-ttu-id="f71c1-141">Skapa en resursgrupp, en belastningsutjämning, ett virtuellt nätverk och undernät</span><span class="sxs-lookup"><span data-stu-id="f71c1-141">Create a resource group, a load balancer, a virtual network, and subnets</span></span>

1. <span data-ttu-id="f71c1-142">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="f71c1-142">Create a resource group</span></span>

    ```azurecli
    azure group create $rgName $location
    ```

2. <span data-ttu-id="f71c1-143">Skapa en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="f71c1-143">Create a load balancer</span></span>

    ```azurecli
    $lb = azure network lb create --resource-group $rgname --location $location --name $lbName
    ```

3. <span data-ttu-id="f71c1-144">Skapa ett virtuellt nätverk (VNet).</span><span class="sxs-lookup"><span data-stu-id="f71c1-144">Create a virtual network (VNet).</span></span>

    ```azurecli
    $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix
    ```

    <span data-ttu-id="f71c1-145">Skapa två undernät i detta virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f71c1-145">Create two subnets in this VNet.</span></span>

    ```azurecli
    $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
    $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName
    ```

## <a name="create-public-ip-addresses-for-hello-front-end-pool"></a><span data-ttu-id="f71c1-146">Skapa den offentliga IP-adresser för hello frontend-pool</span><span class="sxs-lookup"><span data-stu-id="f71c1-146">Create public IP addresses for hello front-end pool</span></span>

1. <span data-ttu-id="f71c1-147">Ställ in hello PowerShell variabler</span><span class="sxs-lookup"><span data-stu-id="f71c1-147">Set up hello PowerShell variables</span></span>

    ```powershell
    $publicIpv4Name = "myIPv4Vip"
    $publicIpv6Name = "myIPv6Vip"
    ```

2. <span data-ttu-id="f71c1-148">Skapa en offentlig IP-hello frontend IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="f71c1-148">Create a public IP address hello front-end IP pool.</span></span>

    ```azurecli
    $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
    $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="f71c1-149">hello belastningsutjämnare använder hello domänetikett hello offentlig IP-adress som dess FQDN.</span><span class="sxs-lookup"><span data-stu-id="f71c1-149">hello load balancer uses hello domain label of hello public IP as its FQDN.</span></span> <span data-ttu-id="f71c1-150">Denna ändring från klassisk distribution som använder hello molntjänstnamnet som hello belastningsutjämnaren FQDN.</span><span class="sxs-lookup"><span data-stu-id="f71c1-150">This a change from classic deployment, which uses hello cloud service name as hello load balancer FQDN.</span></span>
    > <span data-ttu-id="f71c1-151">I det här exemplet hello FQDN är *contoso09152016.southcentralus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="f71c1-151">In this example, hello FQDN is *contoso09152016.southcentralus.cloudapp.azure.com*.</span></span>

## <a name="create-front-end-and-back-end-pools"></a><span data-ttu-id="f71c1-152">Skapa frontend och backend-pooler</span><span class="sxs-lookup"><span data-stu-id="f71c1-152">Create front-end and back-end pools</span></span>

<span data-ttu-id="f71c1-153">Det här exemplet skapar hello frontend IP-pool som tar emot hello inkommande nätverkstrafik på hello belastningsutjämnare och hello backend-IP-pool där hello frontend poolen skickar hello belastningsutjämnad trafik.</span><span class="sxs-lookup"><span data-stu-id="f71c1-153">This example creates hello front-end IP pool that receives hello incoming network traffic on hello load balancer and hello back-end IP pool where hello front-end pool sends hello load balanced network traffic.</span></span>

1. <span data-ttu-id="f71c1-154">Ställ in hello PowerShell variabler</span><span class="sxs-lookup"><span data-stu-id="f71c1-154">Set up hello PowerShell variables</span></span>

    ```powershell
    $frontendV4Name = "FrontendVipIPv4"
    $frontendV6Name = "FrontendVipIPv6"
    $backendAddressPoolV4Name = "BackendPoolIPv4"
    $backendAddressPoolV6Name = "BackendPoolIPv6"
    ```

2. <span data-ttu-id="f71c1-155">Skapa en frontend IP-adresspool som kopplar hello offentliga IP-Adressen skapas i hello föregående steg och hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f71c1-155">Create a front-end IP pool associating hello public IP created in hello previous step and hello load balancer.</span></span>

    ```azurecli
    $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
    $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
    $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
    $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName
    ```

## <a name="create-hello-probe-nat-rules-and-lb-rules"></a><span data-ttu-id="f71c1-156">Skapa hello avsökningen, NAT-regler och LB regler</span><span class="sxs-lookup"><span data-stu-id="f71c1-156">Create hello probe, NAT rules, and LB rules</span></span>

<span data-ttu-id="f71c1-157">Det här exemplet skapar hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="f71c1-157">This example creates hello following items:</span></span>

* <span data-ttu-id="f71c1-158">en avsökning regeln toocheck för anslutningen tooTCP port 80</span><span class="sxs-lookup"><span data-stu-id="f71c1-158">a probe rule toocheck for connectivity tooTCP port 80</span></span>
* <span data-ttu-id="f71c1-159">en NAT-regel tootranslate all inkommande trafik på port 3389 tooport 3389 för RDP<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="f71c1-159">a NAT rule tootranslate all incoming traffic on port 3389 tooport 3389 for RDP<sup>1</sup></span></span>
* <span data-ttu-id="f71c1-160">en NAT-regel tootranslate all inkommande trafik på port 3391 tooport 3389 för RDP<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="f71c1-160">a NAT rule tootranslate all incoming traffic on port 3391 tooport 3389 for RDP<sup>1</sup></span></span>
* <span data-ttu-id="f71c1-161">en belastningen belastningsutjämnaren regeln toobalance all inkommande trafik på port 80 tooport 80 på hello adresser i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="f71c1-161">a load balancer rule toobalance all incoming traffic on port 80 tooport 80 on hello addresses in hello back-end pool.</span></span>

<span data-ttu-id="f71c1-162"><sup>1</sup> NAT-regler är associerade tooa specifik virtuell datorinstans bakom hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f71c1-162"><sup>1</sup> NAT rules are associated tooa specific virtual machine instance behind hello load balancer.</span></span> <span data-ttu-id="f71c1-163">hello nätverkstrafik som ankommer till port 3389 skickas toohello specifik virtuell dator och port som associeras med hello NAT-regeln.</span><span class="sxs-lookup"><span data-stu-id="f71c1-163">hello network traffic arriving on port 3389 is sent toohello specific virtual machine and port associated with hello NAT rule.</span></span> <span data-ttu-id="f71c1-164">Du måste ange ett protokoll (UDP eller TCP) för en NAT-regel.</span><span class="sxs-lookup"><span data-stu-id="f71c1-164">You must specify a protocol (UDP or TCP) for a NAT rule.</span></span> <span data-ttu-id="f71c1-165">Båda protokollen kan inte vara tilldelad toohello samma port.</span><span class="sxs-lookup"><span data-stu-id="f71c1-165">Both protocols can't be assigned toohello same port.</span></span>

1. <span data-ttu-id="f71c1-166">Ställ in hello PowerShell variabler</span><span class="sxs-lookup"><span data-stu-id="f71c1-166">Set up hello PowerShell variables</span></span>

    ```powershell
    $probeV4V6Name = "ProbeForIPv4AndIPv6"
    $natRule1V4Name = "NatRule-For-Rdp-VM1"
    $natRule2V4Name = "NatRule-For-Rdp-VM2"
    $lbRule1V4Name = "LBRuleForIPv4-Port80"
    $lbRule1V6Name = "LBRuleForIPv6-Port80"
    ```

2. <span data-ttu-id="f71c1-167">Skapa hello avsökningen</span><span class="sxs-lookup"><span data-stu-id="f71c1-167">Create hello probe</span></span>

    <span data-ttu-id="f71c1-168">hello följande exempel skapas en TCP-avsökning som kontrollerar för anslutningen tooback slutpunkt TCP-port 80 var 15: e sekund.</span><span class="sxs-lookup"><span data-stu-id="f71c1-168">hello following example creates a TCP probe that checks for connectivity tooback-end TCP port 80 every 15 seconds.</span></span> <span data-ttu-id="f71c1-169">Den markeras hello backend-resurs inte tillgänglig efter två på varandra följande fel.</span><span class="sxs-lookup"><span data-stu-id="f71c1-169">It will mark hello back-end resource unavailable after two consecutive failures.</span></span>

    ```azurecli
    $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName
    ```

3. <span data-ttu-id="f71c1-170">Skapa inkommande NAT-regler som tillåter RDP-anslutningar toohello backend-resurser</span><span class="sxs-lookup"><span data-stu-id="f71c1-170">Create inbound NAT rules that allow RDP connections toohello back-end resources</span></span>

    ```azurecli
    $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
    $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName
    ```

4. <span data-ttu-id="f71c1-171">Skapa regler som skickar trafik toodifferent portar beroende på vilken frontend emot hello begäran för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="f71c1-171">Create a load balancer rules that send traffic toodifferent back-end ports depending on which front end received hello request</span></span>

    ```azurecli
    $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
    $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName
    ```

5. <span data-ttu-id="f71c1-172">Kontrollera inställningarna</span><span class="sxs-lookup"><span data-stu-id="f71c1-172">Check your settings</span></span>

    ```azurecli
    azure network lb show --resource-group $rgName --name $lbName
    ```

    <span data-ttu-id="f71c1-173">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="f71c1-173">Expected output:</span></span>

        info:    Executing command network lb show
        info:    Looking up hello load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show

## <a name="create-nics"></a><span data-ttu-id="f71c1-174">Skapa nätverkskort</span><span class="sxs-lookup"><span data-stu-id="f71c1-174">Create NICs</span></span>

<span data-ttu-id="f71c1-175">Skapa nätverkskort och kopplar dem tooNAT regler, regler för inläsning av belastningsutjämnare och avsökningar.</span><span class="sxs-lookup"><span data-stu-id="f71c1-175">Create NICs and associate them tooNAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="f71c1-176">Ställ in hello PowerShell variabler</span><span class="sxs-lookup"><span data-stu-id="f71c1-176">Set up hello PowerShell variables</span></span>

    ```powershell
    $nic1Name = "myIPv4IPv6Nic1"
    $nic2Name = "myIPv4IPv6Nic2"
    $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
    $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
    $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
    $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
    $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
    $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"
    ```

2. <span data-ttu-id="f71c1-177">Skapa ett nätverkskort för varje serverdel och Lägg till en IPv6-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f71c1-177">Create a NIC for each back-end and add an IPv6 configuration.</span></span>

    ```azurecli
    $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
    $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

    $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule2V4Id
    $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name
    ```

## <a name="create-hello-back-end-vm-resources-and-attach-each-nic"></a><span data-ttu-id="f71c1-178">Skapa hello backend-VM resurser och koppla varje nätverkskort</span><span class="sxs-lookup"><span data-stu-id="f71c1-178">Create hello back-end VM resources and attach each NIC</span></span>

<span data-ttu-id="f71c1-179">Du måste ha ett lagringskonto toocreate virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f71c1-179">toocreate VMs, you must have a storage account.</span></span> <span data-ttu-id="f71c1-180">För belastningsutjämning, behöver hello VMs toobe medlemmar i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="f71c1-180">For load balancing, hello VMs need toobe members of an availability set.</span></span> <span data-ttu-id="f71c1-181">Mer information om hur du skapar virtuella datorer finns [skapa en virtuell Azure-dator med hjälp av PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="f71c1-181">For more information about creating VMs, see [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span></span>

1. <span data-ttu-id="f71c1-182">Ställ in hello PowerShell variabler</span><span class="sxs-lookup"><span data-stu-id="f71c1-182">Set up hello PowerShell variables</span></span>

    ```powershell
    $storageAccountName = "ps08092016v6sa0"
    $availabilitySetName = "myIPv4IPv6AvailabilitySet"
    $vm1Name = "myIPv4IPv6VM1"
    $vm2Name = "myIPv4IPv6VM2"
    $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
    $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
    $disk1Name = "WindowsVMosDisk1"
    $disk2Name = "WindowsVMosDisk2"
    $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
    $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
    $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
    $vmUserName = "vmUser"
    $mySecurePassword = "PlainTextPassword*1"
    ```

    > [!WARNING]
    > <span data-ttu-id="f71c1-183">Det här exemplet använder hello användarnamn och lösenord för hello virtuella datorer i klartext.</span><span class="sxs-lookup"><span data-stu-id="f71c1-183">This example uses hello username and password for hello VMs in clear text.</span></span> <span data-ttu-id="f71c1-184">Lämplig måste vara försiktig när du använder autentiseringsuppgifter i hello Rensa.</span><span class="sxs-lookup"><span data-stu-id="f71c1-184">Appropriate care must be taken when using credentials in hello clear.</span></span> <span data-ttu-id="f71c1-185">Ett säkrare sätt hantera autentiseringsuppgifter i PowerShell finns i hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f71c1-185">For a more secure method of handling credentials in PowerShell, see hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

2. <span data-ttu-id="f71c1-186">Skapa hello storage-konto och tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="f71c1-186">Create hello storage account and availability set</span></span>

    <span data-ttu-id="f71c1-187">Du kan använda ett befintligt lagringskonto när du skapar hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f71c1-187">You may use an existing storage account when you create hello VMs.</span></span> <span data-ttu-id="f71c1-188">hello följande kommando skapar ett nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f71c1-188">hello following command creates a new storage account.</span></span>

    ```azurecli
    $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"
    ```

    <span data-ttu-id="f71c1-189">Skapa sedan hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="f71c1-189">Next, create hello availability set.</span></span>

    ```azurecli
    $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location
    ```

3. <span data-ttu-id="f71c1-190">Skapa hello virtuella datorer med hello associerade nätverkskort</span><span class="sxs-lookup"><span data-stu-id="f71c1-190">Create hello virtual machines with hello associated NICs</span></span>

    ```azurecli
    $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

    $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension
    ```

## <a name="next-steps"></a><span data-ttu-id="f71c1-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f71c1-191">Next steps</span></span>

[<span data-ttu-id="f71c1-192">Komma igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="f71c1-192">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-cli.md)

[<span data-ttu-id="f71c1-193">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="f71c1-193">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="f71c1-194">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="f71c1-194">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
