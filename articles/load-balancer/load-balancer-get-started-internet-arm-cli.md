---
title: "Skapa en Internetaktiverad belastningsutjämnare – Azure CLI | Microsoft Docs"
description: "Lär dig hur du skapar en Internetuppkopplad belastningsutjämnare i Resource Manager med hjälp av Azure CLI"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a8bcdd88-f94c-4537-8143-c710eaa86818
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 3b1780033cbc8aa3e108a213a4d2bfd0332fd7d7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="creating-an-internet-load-balancer-using-the-azure-cli"></a><span data-ttu-id="748f2-103">Skapa en intern belastningsutjämnare med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="748f2-103">Creating an internet load balancer using the Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="748f2-104">Portal</span><span class="sxs-lookup"><span data-stu-id="748f2-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="748f2-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="748f2-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="748f2-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="748f2-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="748f2-107">Mall</span><span class="sxs-lookup"><span data-stu-id="748f2-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="748f2-108">Den här artikeln beskriver Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="748f2-108">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="748f2-109">Du kan också läsa om [hur du skapar en Internetuppkopplad belastningsutjämnare med hjälp av en klassisk distribution](load-balancer-get-started-internet-classic-portal.md)</span><span class="sxs-lookup"><span data-stu-id="748f2-109">You can also [Learn how to create an Internet facing load balancer using classic deployment](load-balancer-get-started-internet-classic-portal.md)</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a><span data-ttu-id="748f2-110">Distribuera lösningen med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="748f2-110">Deploying the solution using the Azure CLI</span></span>

<span data-ttu-id="748f2-111">Följande steg beskriver hur du skapar en Internetuppkopplad belastningsutjämnare med hjälp av Azure Resource Manager med CLI.</span><span class="sxs-lookup"><span data-stu-id="748f2-111">The following steps show how to create an Internet facing load balancer using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="748f2-112">Med Azure Resource Manager skapas och konfigureras varje resurs separat, och läggs sedan ihop för att skapa en resurs.</span><span class="sxs-lookup"><span data-stu-id="748f2-112">With Azure Resource Manager each resource is created and configured individually, then put together to create a resource.</span></span>

<span data-ttu-id="748f2-113">Du måste skapa och konfigurera följande objekt för att distribuera en belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="748f2-113">You must create and configure the following objects to deploy a load balancer:</span></span>

* <span data-ttu-id="748f2-114">IP-konfiguration på klientsidan – innehåller offentliga IP-adresser för inkommande nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="748f2-114">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="748f2-115">Backend-adresspool (serverdelspool) – innehåller nätverksgränssnitten (NIC) som de virtuella datorerna använder för att ta emot nätverkstrafik från belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="748f2-115">Back-end address pool - contains network interfaces (NICs) for the virtual machines to receive network traffic from the load balancer.</span></span>
* <span data-ttu-id="748f2-116">Belastningsutjämningsregler – innehåller regler för mappning av en offentlig port på belastningsutjämnaren till en port i backend-adresspoolen.</span><span class="sxs-lookup"><span data-stu-id="748f2-116">Load balancing rules - contains rules mapping a public port on the load balancer to port in the back-end address pool.</span></span>
* <span data-ttu-id="748f2-117">NAT-regler för inkommande trafik – innehåller regler för mappning av en offentlig port i belastningsutjämnaren till en port för en specifik virtuell dator i backend-adresspoolen.</span><span class="sxs-lookup"><span data-stu-id="748f2-117">Inbound NAT rules - contains rules mapping a public port on the load balancer to a port for a specific virtual machine in the back-end address pool.</span></span>
* <span data-ttu-id="748f2-118">Avsökningar – innehåller hälsoavsökningar som används för att kontrollera tillgängligheten av instanser av virtuella datorer i backend-adresspoolen.</span><span class="sxs-lookup"><span data-stu-id="748f2-118">Probes - contains health probes used to check availability of virtual machines instances in the back-end address pool.</span></span>

<span data-ttu-id="748f2-119">Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="748f2-119">For more information see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-cli-to-use-resource-manager"></a><span data-ttu-id="748f2-120">Konfigurera CLI för att använda Resource Manager</span><span class="sxs-lookup"><span data-stu-id="748f2-120">Set up CLI to use Resource Manager</span></span>

1. <span data-ttu-id="748f2-121">Om du aldrig har använt Azure CLI, se [installera och konfigurera Azure CLI](../cli-install-nodejs.md) och följ instruktionerna upp till den punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="748f2-121">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="748f2-122">Kör kommandot **azure config mode** för att växla till Resource Manager-läge, som det visas nedan.</span><span class="sxs-lookup"><span data-stu-id="748f2-122">Run the **azure config mode** command to switch to Resource Manager mode, as shown below.</span></span>

    ```azurecli
        azure config mode arm
    ```

    <span data-ttu-id="748f2-123">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="748f2-123">Expected output:</span></span>

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a><span data-ttu-id="748f2-124">Skapa ett virtuellt nätverk och en offentlig IP-adress för IP-adresspoolen på klientsidan</span><span class="sxs-lookup"><span data-stu-id="748f2-124">Create a virtual network and a public IP address for the front-end IP pool</span></span>

1. <span data-ttu-id="748f2-125">Skapa ett virtuellt nätverk (VNet) med namnet *NRPVnet* på platsen East USA med hjälp av en resursgrupp med namnet *NRPRG*.</span><span class="sxs-lookup"><span data-stu-id="748f2-125">Create a virtual network (VNet) named *NRPVnet* in the East US location using a resource group named *NRPRG*.</span></span>

    ```azurecli
        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16
    ```

    <span data-ttu-id="748f2-126">Skapa ett undernät med namnet *NRPVnetSubnet* med CIDR-blocket 10.0.0.0/24 i *NRPVnet*.</span><span class="sxs-lookup"><span data-stu-id="748f2-126">Create a subnet named *NRPVnetSubnet* with a CIDR block of 10.0.0.0/24 in *NRPVnet*.</span></span>

    ```azurecli
        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24
    ```

2. <span data-ttu-id="748f2-127">Skapa en offentlig IP-adress med namnet *NRPPublicIP* som ska användas av en IP-adresspool på klientsidan med DNS-namnet *loadbalancernrp.eastus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="748f2-127">Create a public IP address named *NRPPublicIP* to be used by a front-end IP pool with DNS name *loadbalancernrp.eastus.cloudapp.azure.com*.</span></span> <span data-ttu-id="748f2-128">I kommandot nedan används den statiska allokeringstypen och en timeout för inaktivitet på 4 minuter.</span><span class="sxs-lookup"><span data-stu-id="748f2-128">The command below uses the static allocation type and idle timeout of 4 minutes.</span></span>

    ```azurecli
        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="748f2-129">Belastningsutjämnaren använder domänetiketten för den offentliga IP-adressen som dess fullständiga domännamn (FQDN).</span><span class="sxs-lookup"><span data-stu-id="748f2-129">The load balancer will use the domain label of the public IP as its FQDN.</span></span> <span data-ttu-id="748f2-130">Det här är en ändring från den klassiska distributionen, som använder molntjänsten som belastningsutjämnarens fullständiga domännamn.</span><span class="sxs-lookup"><span data-stu-id="748f2-130">This a change from classic deployment, which uses the cloud service as the load balancer Fully Qualified Domain Name (FQDN).</span></span>
   > <span data-ttu-id="748f2-131">I det här exemplet är det fullständiga domännamnet *loadbalancernrp.eastus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="748f2-131">In this example, the FQDN is *loadbalancernrp.eastus.cloudapp.azure.com*.</span></span>

## <a name="create-a-load-balancer"></a><span data-ttu-id="748f2-132">Skapa en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="748f2-132">Create a load balancer</span></span>

<span data-ttu-id="748f2-133">Följande kommando skapar en belastningsutjämnare med namnet *NRPlb* i *NRPRG*-resursgruppen på Azure-platsen *East USA*.</span><span class="sxs-lookup"><span data-stu-id="748f2-133">The following command creates a load balancer named *NRPlb* in the *NRPRG* resource group in the *East US* Azure location.</span></span>

    ```azurecli
    azure network lb create NRPRG NRPlb eastus
    ```

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a><span data-ttu-id="748f2-134">Skapa en IP-adresspool på klientsidan och en backend-adresspool</span><span class="sxs-lookup"><span data-stu-id="748f2-134">Create a front-end IP pool and a backend address pool</span></span>
<span data-ttu-id="748f2-135">Det här exemplet visar hur du skapar IP-adresspoolen på klientsidan som tar emot den inkommande nätverkstrafiken på belastningsutjämnaren och backend-IP-adresspoolen där serverdelspoolen skickar den belastningsutjämnade nätverkstrafiken.</span><span class="sxs-lookup"><span data-stu-id="748f2-135">This example demonstrates how to create the front-end IP pool that receives the incoming network traffic on the load balancer and the backend IP pool where the front-end pool sends the load balanced network traffic.</span></span>

1. <span data-ttu-id="748f2-136">Skapa en IP-adresspool för klientsidan och koppla den offentliga IP-adressen som du skapade i föregående steg och belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="748f2-136">Create a front-end IP pool associating the public IP created in the previous step and the load balancer.</span></span>

    ```azurecli
        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip
    ```

2. <span data-ttu-id="748f2-137">Konfigurera en adresspool på serversidan som används för att ta emot inkommande trafik från IP-poolen på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="748f2-137">Set up a back-end address pool used to receive incoming traffic from the front-end IP pool.</span></span>

    ```azurecli
        azure network lb address-pool create NRPRG NRPlb NRPbackendpool
    ```

## <a name="create-lb-rules-nat-rules-and-probe"></a><span data-ttu-id="748f2-138">Skapa LB-regler, NAT-regler och avsökning</span><span class="sxs-lookup"><span data-stu-id="748f2-138">Create LB rules, NAT rules, and probe</span></span>

<span data-ttu-id="748f2-139">I det här exemplet skapas följande objekt:</span><span class="sxs-lookup"><span data-stu-id="748f2-139">This example creates the following items.</span></span>

* <span data-ttu-id="748f2-140">En NAT-regel som översätter all inkommande trafik på port 21 till port 22<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="748f2-140">a NAT rule to translate all incoming traffic on port 21 to port 22<sup>1</sup></span></span>
* <span data-ttu-id="748f2-141">En NAT-regel som översätter all inkommande trafik på port 23 till port 22</span><span class="sxs-lookup"><span data-stu-id="748f2-141">a NAT rule to translate all incoming traffic on port 23 to port 22</span></span>
* <span data-ttu-id="748f2-142">En belastningsutjämningsregel som balanserar all inkommande trafik på port 80 till port 80 för adresserna i backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="748f2-142">a load balancer rule to balance all incoming traffic on port 80 to port 80 on the addresses in the back-end pool.</span></span>
* <span data-ttu-id="748f2-143">En avsökningsregel som kontrollerar hälsostatusen på en sida med namnet *HealthProbe.aspx*.</span><span class="sxs-lookup"><span data-stu-id="748f2-143">a probe rule to check the health status on a page named *HealthProbe.aspx*.</span></span>

<span data-ttu-id="748f2-144"><sup>1</sup> NAT-regler associeras med en specifik instans av virtuella datorer bakom belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="748f2-144"><sup>1</sup> NAT rules are associated to a specific virtual machine instance behind the load balancer.</span></span> <span data-ttu-id="748f2-145">Nätverkstrafik som kommer till port 21 skickas till en specifik virtuell dator på port 22 som är associerad med den här NAT-regeln.</span><span class="sxs-lookup"><span data-stu-id="748f2-145">The network traffic arriving on port 21 is sent to a specific virtual machine on port 22 associated with this NAT rule.</span></span> <span data-ttu-id="748f2-146">Du måste ange ett protokoll (UDP eller TCP) för en NAT-regel.</span><span class="sxs-lookup"><span data-stu-id="748f2-146">You must specify a protocol (UDP or TCP) for a NAT rule.</span></span> <span data-ttu-id="748f2-147">Båda protokollen kan inte tilldelas till samma port.</span><span class="sxs-lookup"><span data-stu-id="748f2-147">Both protocols can't be assigned to the same port.</span></span>

1. <span data-ttu-id="748f2-148">Skapa NAT-reglerna.</span><span class="sxs-lookup"><span data-stu-id="748f2-148">Create the NAT rules.</span></span>

    ```azurecli
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22
    ```

2. <span data-ttu-id="748f2-149">Skapa en belastningsutjämningsregel.</span><span class="sxs-lookup"><span data-stu-id="748f2-149">Create a load balancer rule.</span></span>

    ```azurecli
        azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name NRPfrontendpool --backend-address-pool-name NRPbackendpool
    ```

3. <span data-ttu-id="748f2-150">Skapa en hälsoavsökning.</span><span class="sxs-lookup"><span data-stu-id="748f2-150">Create a health probe.</span></span>

    ```azurecli
        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4
    ```

4. <span data-ttu-id="748f2-151">Kontrollera inställningarna.</span><span class="sxs-lookup"><span data-stu-id="748f2-151">Check your settings.</span></span>

    ```azurecli
        azure network lb show nrprg nrplb
    ```

    <span data-ttu-id="748f2-152">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="748f2-152">Expected output:</span></span>

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a><span data-ttu-id="748f2-153">Skapa nätverkskort</span><span class="sxs-lookup"><span data-stu-id="748f2-153">Create NICs</span></span>

<span data-ttu-id="748f2-154">Du måste skapa nätverkskort (eller ändra befintliga) och associera dem med NAT-regler, belastningsutjämningsregler och avsökningar.</span><span class="sxs-lookup"><span data-stu-id="748f2-154">You need to create NICs (or modify existing ones) and associate them to NAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="748f2-155">Skapa ett nätverkskort med namnet *lb-nic1-be* och associera det med NAT-regeln *rdp1* och backend-adresspoolen *NRPbackendpool*.</span><span class="sxs-lookup"><span data-stu-id="748f2-155">Create a NIC named *lb-nic1-be*, and associate it with the *rdp1* NAT rule, and the *NRPbackendpool* back-end address pool.</span></span>

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
    ```

    <span data-ttu-id="748f2-156">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="748f2-156">Expected output:</span></span>

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. <span data-ttu-id="748f2-157">Skapa ett nätverkskort med namnet *lb-nic2-be* och associera det med NAT-regeln *rdp2* och backend-adresspoolen *NRPbackendpool*.</span><span class="sxs-lookup"><span data-stu-id="748f2-157">Create a NIC named *lb-nic2-be*, and associate it with the *rdp2* NAT rule, and the *NRPbackendpool* back-end address pool.</span></span>

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
    ```

3. <span data-ttu-id="748f2-158">Skapa en virtuell dator (VM) med namnet *web1* och associera den med nätverkskortet med namnet *lb-nic1-be*.</span><span class="sxs-lookup"><span data-stu-id="748f2-158">Create a virtual machine (VM) named *web1*, and associate it with the NIC named *lb-nic1-be*.</span></span> <span data-ttu-id="748f2-159">Ett lagringskonto med namnet *web1nrp* skapas innan du kör kommandot nedan.</span><span class="sxs-lookup"><span data-stu-id="748f2-159">A storage account called *web1nrp* was created before running the command below.</span></span>

    ```azurecli
        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="748f2-160">Virtuella datorer i en belastningsutjämnare måste finnas i samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="748f2-160">VMs in a load balancer need to be in the same availability set.</span></span> <span data-ttu-id="748f2-161">Använd `azure availset create` för att skapa en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="748f2-161">Use `azure availset create` to create an availability set.</span></span>

    <span data-ttu-id="748f2-162">Resultatet bör likna följande:</span><span class="sxs-lookup"><span data-stu-id="748f2-162">The output should be similar to the following:</span></span>

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    > [!NOTE]
    > <span data-ttu-id="748f2-163">Ett meddelande som anger att **ingen offentlig IP-adress har konfigurerats för nätverkskortet** visas eftersom nätverkskortet som skapas för belastningsutjämnaren som ansluter till Internet använder belastningsutjämnarens offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="748f2-163">The informational message **This is a NIC without publicIP configured** is expected since the NIC created for the load balancer connecting to Internet using the load balancer public IP address.</span></span>

    <span data-ttu-id="748f2-164">Eftersom nätverkskortet *lb-nic1-be* är associerat med NAT-regeln *rdp1* kan du ansluta till *web1* med hjälp av RDP via port 3441 på belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="748f2-164">Since the *lb-nic1-be* NIC is associated with the *rdp1* NAT rule, you can connect to *web1* using RDP through port 3441 on the load balancer.</span></span>

4. <span data-ttu-id="748f2-165">Skapa en virtuell dator (VM) med namnet *web2* och associera den med nätverkskortet med namnet *lb-nic2-be*.</span><span class="sxs-lookup"><span data-stu-id="748f2-165">Create a virtual machine (VM) named *web2*, and associate it with the NIC named *lb-nic2-be*.</span></span> <span data-ttu-id="748f2-166">Ett lagringskonto med namnet *web1nrp* skapas innan du kör kommandot nedan.</span><span class="sxs-lookup"><span data-stu-id="748f2-166">A storage account called *web1nrp* was created before running the command below.</span></span>

    ```azurecli
        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="update-an-existing-load-balancer"></a><span data-ttu-id="748f2-167">Uppdatera en befintlig belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="748f2-167">Update an existing load balancer</span></span>
<span data-ttu-id="748f2-168">Du kan lägga till regler som refererar till en befintlig belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="748f2-168">You can add rules referencing an existing load balancer.</span></span> <span data-ttu-id="748f2-169">I nästa exempel läggs en ny belastningsutjämningsregel till i en befintlig belastningsutjämnare, **NRPlb**</span><span class="sxs-lookup"><span data-stu-id="748f2-169">In the next example, a new load balancer rule is added to an existing load balancer **NRPlb**</span></span>

```azurecli
azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool
```

## <a name="delete-a-load-balancer"></a><span data-ttu-id="748f2-170">Ta bort en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="748f2-170">Delete a load balancer</span></span>
<span data-ttu-id="748f2-171">Använd följande kommando om du vill ta bort en belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="748f2-171">Use the following command to remove a load balancer:</span></span>

```azurecli
azure network lb delete --resource-group nrprg --name nrplb
```

## <a name="next-steps"></a><span data-ttu-id="748f2-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="748f2-172">Next steps</span></span>
[<span data-ttu-id="748f2-173">Komma igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="748f2-173">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-cli.md)

[<span data-ttu-id="748f2-174">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="748f2-174">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="748f2-175">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="748f2-175">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
