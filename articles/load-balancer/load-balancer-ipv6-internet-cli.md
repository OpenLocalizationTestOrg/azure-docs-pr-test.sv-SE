---
title: "Skapa en Internetriktade belastningsutjämnare med IPv6 - Azure CLI | Microsoft Docs"
description: "Lär dig hur du skapar en Internetuppkopplad belastningsutjämnare med IPv6 i Azure Resource Manager med hjälp av Azure CLI"
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
ms.openlocfilehash: efb4771800c42df544c3cc37d1d164045fdcaf3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a><span data-ttu-id="28d90-104">Skapa en Internetuppkopplad belastningsutjämnare med IPv6 i Azure Resource Manager med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="28d90-104">Create an Internet facing load balancer with IPv6 in Azure Resource Manager using the Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="28d90-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="28d90-105">PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
> * [<span data-ttu-id="28d90-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="28d90-106">Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
> * [<span data-ttu-id="28d90-107">Mall</span><span class="sxs-lookup"><span data-stu-id="28d90-107">Template</span></span>](load-balancer-ipv6-internet-template.md)

<span data-ttu-id="28d90-108">En Azure belastningsutjämnare är en Layer 4-belastningsutjämnare (TCP, UDP).</span><span class="sxs-lookup"><span data-stu-id="28d90-108">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer.</span></span> <span data-ttu-id="28d90-109">Belastningsutjämnaren ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfria tjänstinstanser i molntjänster eller virtuella datorer i en belastningsutjämningsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="28d90-109">The load balancer provides high availability by distributing incoming traffic among healthy service instances in cloud services or virtual machines in a load balancer set.</span></span> <span data-ttu-id="28d90-110">Azure belastningsutjämnare kan även presentera dessa tjänster på flera portar, flera IP-adresser eller både och.</span><span class="sxs-lookup"><span data-stu-id="28d90-110">Azure Load Balancer can also present those services on multiple ports, multiple IP addresses, or both.</span></span>

## <a name="example-deployment-scenario"></a><span data-ttu-id="28d90-111">Exempelscenario för distribution</span><span class="sxs-lookup"><span data-stu-id="28d90-111">Example deployment scenario</span></span>

<span data-ttu-id="28d90-112">Följande diagram illustrerar nätverkslösning för belastningsutjämning som distribueras med hjälp av mallen exempel som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="28d90-112">The following diagram illustrates the load balancing solution being deployed using the example template described in this article.</span></span>

![Belastningsutjämningsscenario](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

<span data-ttu-id="28d90-114">I det här scenariot skapar du följande Azure-resurser:</span><span class="sxs-lookup"><span data-stu-id="28d90-114">In this scenario you will create the following Azure resources:</span></span>

* <span data-ttu-id="28d90-115">två virtuella datorer (VM)</span><span class="sxs-lookup"><span data-stu-id="28d90-115">two virtual machines (VMs)</span></span>
* <span data-ttu-id="28d90-116">ett virtuellt nätverksgränssnitt för varje virtuell dator med både IPv4 och IPv6-adresser som tilldelats</span><span class="sxs-lookup"><span data-stu-id="28d90-116">a virtual network interface for each VM with both IPv4 and IPv6 addresses assigned</span></span>
* <span data-ttu-id="28d90-117">en Internetriktade belastningsutjämnare med en IPv4 och IPv6-offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="28d90-117">an Internet-facing Load Balancer with an IPv4 and an IPv6 Public IP address</span></span>
* <span data-ttu-id="28d90-118">en Tillgänglighetsuppsättning som innehåller två virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="28d90-118">an Availability Set to that contains the two VMs</span></span>
* <span data-ttu-id="28d90-119">två läsa in belastningsutjämning regler för att mappa offentliga VIP till privata slutpunkter</span><span class="sxs-lookup"><span data-stu-id="28d90-119">two load balancing rules to map the public VIPs to the private endpoints</span></span>

## <a name="deploying-the-solution-using-the-azure-cli"></a><span data-ttu-id="28d90-120">Distribuera lösningen med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="28d90-120">Deploying the solution using the Azure CLI</span></span>

<span data-ttu-id="28d90-121">Följande steg beskriver hur du skapar en Internetuppkopplad belastningsutjämnare med hjälp av Azure Resource Manager med CLI.</span><span class="sxs-lookup"><span data-stu-id="28d90-121">The following steps show how to create an Internet facing load balancer using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="28d90-122">Med Azure Resource Manager skapas och konfigureras varje resurs separat, och läggs sedan ihop för att skapa en resurs.</span><span class="sxs-lookup"><span data-stu-id="28d90-122">With Azure Resource Manager, each resource is created and configured individually, and then put together to create a resource.</span></span>

<span data-ttu-id="28d90-123">För att distribuera en belastningsutjämnare måste du skapa och konfigurera följande objekt:</span><span class="sxs-lookup"><span data-stu-id="28d90-123">To deploy a load balancer, you create and configure the following objects:</span></span>

* <span data-ttu-id="28d90-124">IP-konfiguration på klientsidan – innehåller offentliga IP-adresser för inkommande nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="28d90-124">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="28d90-125">Backend-adresspool (serverdelspool) – innehåller nätverksgränssnitten (NIC) som de virtuella datorerna använder för att ta emot nätverkstrafik från belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="28d90-125">Back-end address pool - contains network interfaces (NICs) for the virtual machines to receive network traffic from the load balancer.</span></span>
* <span data-ttu-id="28d90-126">Belastningsutjämningsregler – innehåller regler för mappning av en offentlig port på belastningsutjämnaren till en port i backend-adresspoolen.</span><span class="sxs-lookup"><span data-stu-id="28d90-126">Load balancing rules - contains rules mapping a public port on the load balancer to port in the back-end address pool.</span></span>
* <span data-ttu-id="28d90-127">NAT-regler för inkommande trafik – innehåller regler för mappning av en offentlig port i belastningsutjämnaren till en port för en specifik virtuell dator i backend-adresspoolen.</span><span class="sxs-lookup"><span data-stu-id="28d90-127">Inbound NAT rules - contains rules mapping a public port on the load balancer to a port for a specific virtual machine in the back-end address pool.</span></span>
* <span data-ttu-id="28d90-128">Avsökningar – innehåller hälsoavsökningar som används för att kontrollera tillgängligheten av instanser av virtuella datorer i backend-adresspoolen.</span><span class="sxs-lookup"><span data-stu-id="28d90-128">Probes - contains health probes used to check availability of virtual machines instances in the back-end address pool.</span></span>

<span data-ttu-id="28d90-129">Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="28d90-129">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a><span data-ttu-id="28d90-130">Ställ in CLI miljön att använda Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="28d90-130">Set up your CLI environment to use Azure Resource Manager</span></span>

<span data-ttu-id="28d90-131">I det här exemplet kör vi CLI-verktygen i PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="28d90-131">For this example, we are running the CLI tools in a PowerShell command window.</span></span> <span data-ttu-id="28d90-132">Vi använder inte Azure PowerShell-cmdlets, men vi använder PowerShells skriptfunktioner för att förbättra läsbarhet och återanvändning.</span><span class="sxs-lookup"><span data-stu-id="28d90-132">We are not using the Azure PowerShell cmdlets but we use PowerShell's scripting capabilities to improve readability and reuse.</span></span>

1. <span data-ttu-id="28d90-133">Om du aldrig har använt Azure CLI, se [installera och konfigurera Azure CLI](../cli-install-nodejs.md) och följ instruktionerna upp till den punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="28d90-133">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="28d90-134">Kör den **azure config mode** kommando för att växla till Resource Manager-läget.</span><span class="sxs-lookup"><span data-stu-id="28d90-134">Run the **azure config mode** command to switch to Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="28d90-135">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="28d90-135">Expected output:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="28d90-136">Logga in på Azure och få en lista över prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="28d90-136">Sign in to Azure and get a list of subscriptions.</span></span>

    ```azurecli
    azure login
    ```

    <span data-ttu-id="28d90-137">Ange dina autentiseringsuppgifter för Azure när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="28d90-137">Enter your Azure credentials when prompted.</span></span>

    ```azurecli
    azure account list
    ```

    <span data-ttu-id="28d90-138">Välj den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="28d90-138">Pick the subscription you want to use.</span></span> <span data-ttu-id="28d90-139">Anteckna prenumerations-Id för nästa steg.</span><span class="sxs-lookup"><span data-stu-id="28d90-139">Make note of the subscription Id for the next step.</span></span>

4. <span data-ttu-id="28d90-140">Ställ in PowerShell variabler för användning med CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="28d90-140">Set up PowerShell variables for use with the CLI commands.</span></span>

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

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a><span data-ttu-id="28d90-141">Skapa en resursgrupp, en belastningsutjämning, ett virtuellt nätverk och undernät</span><span class="sxs-lookup"><span data-stu-id="28d90-141">Create a resource group, a load balancer, a virtual network, and subnets</span></span>

1. <span data-ttu-id="28d90-142">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="28d90-142">Create a resource group</span></span>

    ```azurecli
    azure group create $rgName $location
    ```

2. <span data-ttu-id="28d90-143">Skapa en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="28d90-143">Create a load balancer</span></span>

    ```azurecli
    $lb = azure network lb create --resource-group $rgname --location $location --name $lbName
    ```

3. <span data-ttu-id="28d90-144">Skapa ett virtuellt nätverk (VNet).</span><span class="sxs-lookup"><span data-stu-id="28d90-144">Create a virtual network (VNet).</span></span>

    ```azurecli
    $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix
    ```

    <span data-ttu-id="28d90-145">Skapa två undernät i detta virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="28d90-145">Create two subnets in this VNet.</span></span>

    ```azurecli
    $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
    $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName
    ```

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a><span data-ttu-id="28d90-146">Skapa den offentliga IP-adresser för frontend-pool</span><span class="sxs-lookup"><span data-stu-id="28d90-146">Create public IP addresses for the front-end pool</span></span>

1. <span data-ttu-id="28d90-147">Ställ in variablerna PowerShell</span><span class="sxs-lookup"><span data-stu-id="28d90-147">Set up the PowerShell variables</span></span>

    ```powershell
    $publicIpv4Name = "myIPv4Vip"
    $publicIpv6Name = "myIPv6Vip"
    ```

2. <span data-ttu-id="28d90-148">Skapa en offentlig IP-adress frontend IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="28d90-148">Create a public IP address the front-end IP pool.</span></span>

    ```azurecli
    $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
    $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="28d90-149">Belastningsutjämnaren använder domänetiketten för den offentliga IP-adressen som sitt fullständiga domännamn (FQDN).</span><span class="sxs-lookup"><span data-stu-id="28d90-149">The load balancer uses the domain label of the public IP as its FQDN.</span></span> <span data-ttu-id="28d90-150">Detta en ändring från klassisk distribution som använder Molntjänsten namn som belastningsutjämnare FQDN.</span><span class="sxs-lookup"><span data-stu-id="28d90-150">This a change from classic deployment, which uses the cloud service name as the load balancer FQDN.</span></span>
    > <span data-ttu-id="28d90-151">I det här exemplet FQDN är *contoso09152016.southcentralus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="28d90-151">In this example, the FQDN is *contoso09152016.southcentralus.cloudapp.azure.com*.</span></span>

## <a name="create-front-end-and-back-end-pools"></a><span data-ttu-id="28d90-152">Skapa frontend och backend-pooler</span><span class="sxs-lookup"><span data-stu-id="28d90-152">Create front-end and back-end pools</span></span>

<span data-ttu-id="28d90-153">Det här exemplet skapar frontend IP-adresspool som tar emot inkommande nätverkstrafik på belastningsutjämnaren och backend-IP-pool där frontend poolen skickar belastningsutjämnade nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="28d90-153">This example creates the front-end IP pool that receives the incoming network traffic on the load balancer and the back-end IP pool where the front-end pool sends the load balanced network traffic.</span></span>

1. <span data-ttu-id="28d90-154">Ställ in variablerna PowerShell</span><span class="sxs-lookup"><span data-stu-id="28d90-154">Set up the PowerShell variables</span></span>

    ```powershell
    $frontendV4Name = "FrontendVipIPv4"
    $frontendV6Name = "FrontendVipIPv6"
    $backendAddressPoolV4Name = "BackendPoolIPv4"
    $backendAddressPoolV6Name = "BackendPoolIPv6"
    ```

2. <span data-ttu-id="28d90-155">Skapa en IP-adresspool för klientsidan och koppla den offentliga IP-adressen som du skapade i föregående steg och belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="28d90-155">Create a front-end IP pool associating the public IP created in the previous step and the load balancer.</span></span>

    ```azurecli
    $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
    $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
    $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
    $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName
    ```

## <a name="create-the-probe-nat-rules-and-lb-rules"></a><span data-ttu-id="28d90-156">Skapa avsökningen, NAT-regler och LB regler</span><span class="sxs-lookup"><span data-stu-id="28d90-156">Create the probe, NAT rules, and LB rules</span></span>

<span data-ttu-id="28d90-157">I det här exemplet skapas följande objekt:</span><span class="sxs-lookup"><span data-stu-id="28d90-157">This example creates the following items:</span></span>

* <span data-ttu-id="28d90-158">en avsökning-regel för att söka efter anslutning till TCP-port 80</span><span class="sxs-lookup"><span data-stu-id="28d90-158">a probe rule to check for connectivity to TCP port 80</span></span>
* <span data-ttu-id="28d90-159">en NAT-regel för att översätta all inkommande trafik på port 3389 till port 3389 för RDP<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="28d90-159">a NAT rule to translate all incoming traffic on port 3389 to port 3389 for RDP<sup>1</sup></span></span>
* <span data-ttu-id="28d90-160">en NAT-regel för att översätta all inkommande trafik på port 3391 till port 3389 för RDP<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="28d90-160">a NAT rule to translate all incoming traffic on port 3391 to port 3389 for RDP<sup>1</sup></span></span>
* <span data-ttu-id="28d90-161">En belastningsutjämningsregel som balanserar all inkommande trafik på port 80 till port 80 för adresserna i backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="28d90-161">a load balancer rule to balance all incoming traffic on port 80 to port 80 on the addresses in the back-end pool.</span></span>

<span data-ttu-id="28d90-162"><sup>1</sup> NAT-regler associeras med en specifik instans av virtuella datorer bakom belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="28d90-162"><sup>1</sup> NAT rules are associated to a specific virtual machine instance behind the load balancer.</span></span> <span data-ttu-id="28d90-163">Den nätverkstrafik som ankommer till port 3389 skickas till specifik virtuell dator och port som associeras med NAT-regeln.</span><span class="sxs-lookup"><span data-stu-id="28d90-163">The network traffic arriving on port 3389 is sent to the specific virtual machine and port associated with the NAT rule.</span></span> <span data-ttu-id="28d90-164">Du måste ange ett protokoll (UDP eller TCP) för en NAT-regel.</span><span class="sxs-lookup"><span data-stu-id="28d90-164">You must specify a protocol (UDP or TCP) for a NAT rule.</span></span> <span data-ttu-id="28d90-165">Båda protokollen kan inte tilldelas till samma port.</span><span class="sxs-lookup"><span data-stu-id="28d90-165">Both protocols can't be assigned to the same port.</span></span>

1. <span data-ttu-id="28d90-166">Ställ in variablerna PowerShell</span><span class="sxs-lookup"><span data-stu-id="28d90-166">Set up the PowerShell variables</span></span>

    ```powershell
    $probeV4V6Name = "ProbeForIPv4AndIPv6"
    $natRule1V4Name = "NatRule-For-Rdp-VM1"
    $natRule2V4Name = "NatRule-For-Rdp-VM2"
    $lbRule1V4Name = "LBRuleForIPv4-Port80"
    $lbRule1V6Name = "LBRuleForIPv6-Port80"
    ```

2. <span data-ttu-id="28d90-167">Skapa avsökningen</span><span class="sxs-lookup"><span data-stu-id="28d90-167">Create the probe</span></span>

    <span data-ttu-id="28d90-168">I följande exempel skapas en TCP-avsökning som kontrollerar anslutningen till backend-TCP-port 80 var 15: e sekund.</span><span class="sxs-lookup"><span data-stu-id="28d90-168">The following example creates a TCP probe that checks for connectivity to back-end TCP port 80 every 15 seconds.</span></span> <span data-ttu-id="28d90-169">Den markeras backend-resursen tillgänglig efter två på varandra följande fel.</span><span class="sxs-lookup"><span data-stu-id="28d90-169">It will mark the back-end resource unavailable after two consecutive failures.</span></span>

    ```azurecli
    $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName
    ```

3. <span data-ttu-id="28d90-170">Skapa inkommande NAT-regler som tillåter RDP-anslutningar till backend-resurser</span><span class="sxs-lookup"><span data-stu-id="28d90-170">Create inbound NAT rules that allow RDP connections to the back-end resources</span></span>

    ```azurecli
    $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
    $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName
    ```

4. <span data-ttu-id="28d90-171">Skapa regler som skickar trafik till olika portar beroende på vilken frontend tog emot begäran för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="28d90-171">Create a load balancer rules that send traffic to different back-end ports depending on which front end received the request</span></span>

    ```azurecli
    $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
    $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName
    ```

5. <span data-ttu-id="28d90-172">Kontrollera inställningarna</span><span class="sxs-lookup"><span data-stu-id="28d90-172">Check your settings</span></span>

    ```azurecli
    azure network lb show --resource-group $rgName --name $lbName
    ```

    <span data-ttu-id="28d90-173">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="28d90-173">Expected output:</span></span>

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
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

## <a name="create-nics"></a><span data-ttu-id="28d90-174">Skapa nätverkskort</span><span class="sxs-lookup"><span data-stu-id="28d90-174">Create NICs</span></span>

<span data-ttu-id="28d90-175">Skapa nätverkskort och kopplar dem till NAT-regler, regler för inläsning av belastningsutjämnare och avsökningar.</span><span class="sxs-lookup"><span data-stu-id="28d90-175">Create NICs and associate them to NAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="28d90-176">Ställ in variablerna PowerShell</span><span class="sxs-lookup"><span data-stu-id="28d90-176">Set up the PowerShell variables</span></span>

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

2. <span data-ttu-id="28d90-177">Skapa ett nätverkskort för varje serverdel och Lägg till en IPv6-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="28d90-177">Create a NIC for each back-end and add an IPv6 configuration.</span></span>

    ```azurecli
    $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
    $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

    $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule2V4Id
    $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name
    ```

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a><span data-ttu-id="28d90-178">Skapa backend-VM resurser och koppla varje nätverkskort</span><span class="sxs-lookup"><span data-stu-id="28d90-178">Create the back-end VM resources and attach each NIC</span></span>

<span data-ttu-id="28d90-179">Du måste ha ett lagringskonto för att skapa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="28d90-179">To create VMs, you must have a storage account.</span></span> <span data-ttu-id="28d90-180">Belastningsutjämning måste de virtuella datorerna ska vara medlemmar i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="28d90-180">For load balancing, the VMs need to be members of an availability set.</span></span> <span data-ttu-id="28d90-181">Mer information om hur du skapar virtuella datorer finns [skapa en virtuell Azure-dator med hjälp av PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="28d90-181">For more information about creating VMs, see [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span></span>

1. <span data-ttu-id="28d90-182">Ställ in variablerna PowerShell</span><span class="sxs-lookup"><span data-stu-id="28d90-182">Set up the PowerShell variables</span></span>

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
    > <span data-ttu-id="28d90-183">Det här exemplet använder användarnamn och lösenord för de virtuella datorerna i klartext.</span><span class="sxs-lookup"><span data-stu-id="28d90-183">This example uses the username and password for the VMs in clear text.</span></span> <span data-ttu-id="28d90-184">Lämplig måste vara försiktig när du använder autentiseringsuppgifter i klartext.</span><span class="sxs-lookup"><span data-stu-id="28d90-184">Appropriate care must be taken when using credentials in the clear.</span></span> <span data-ttu-id="28d90-185">Ett säkrare sätt hantera autentiseringsuppgifter i PowerShell finns i [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="28d90-185">For a more secure method of handling credentials in PowerShell, see the [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

2. <span data-ttu-id="28d90-186">Skapa storage-konto och tillgänglighet?</span><span class="sxs-lookup"><span data-stu-id="28d90-186">Create the storage account and availability set</span></span>

    <span data-ttu-id="28d90-187">Du kan använda ett befintligt lagringskonto när du skapar de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="28d90-187">You may use an existing storage account when you create the VMs.</span></span> <span data-ttu-id="28d90-188">Följande kommando skapar ett nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="28d90-188">The following command creates a new storage account.</span></span>

    ```azurecli
    $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"
    ```

    <span data-ttu-id="28d90-189">Skapa sedan tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="28d90-189">Next, create the availability set.</span></span>

    ```azurecli
    $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location
    ```

3. <span data-ttu-id="28d90-190">Skapa virtuella datorer med associerade nätverkskort</span><span class="sxs-lookup"><span data-stu-id="28d90-190">Create the virtual machines with the associated NICs</span></span>

    ```azurecli
    $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

    $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension
    ```

## <a name="next-steps"></a><span data-ttu-id="28d90-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28d90-191">Next steps</span></span>

[<span data-ttu-id="28d90-192">Komma igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="28d90-192">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-cli.md)

[<span data-ttu-id="28d90-193">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="28d90-193">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="28d90-194">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="28d90-194">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
