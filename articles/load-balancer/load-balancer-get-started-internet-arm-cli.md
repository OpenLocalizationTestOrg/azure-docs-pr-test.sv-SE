---
title: "aaaCreate en Internetriktade belastningsutjämnare - Azure CLI | Microsoft Docs"
description: "Lär dig hur toocreate ett Internet belastningsutjämnare i Resource Manager med hjälp av hello Azure CLI"
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
ms.openlocfilehash: cadb5edb3b4a4e2f0813109d027eaafdc7ef7303
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-internet-load-balancer-using-hello-azure-cli"></a><span data-ttu-id="8f92d-103">Skapa en internet-belastningsutjämnare använder hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8f92d-103">Creating an internet load balancer using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="8f92d-104">Portal</span><span class="sxs-lookup"><span data-stu-id="8f92d-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="8f92d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8f92d-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="8f92d-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8f92d-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="8f92d-107">Mall</span><span class="sxs-lookup"><span data-stu-id="8f92d-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="8f92d-108">Den här artikeln beskriver hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="8f92d-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="8f92d-109">Du kan också [Lär dig hur toocreate Internet-riktade belastningsutjämnare använder klassisk distribution](load-balancer-get-started-internet-classic-portal.md)</span><span class="sxs-lookup"><span data-stu-id="8f92d-109">You can also [Learn how toocreate an Internet facing load balancer using classic deployment](load-balancer-get-started-internet-classic-portal.md)</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-hello-solution-using-hello-azure-cli"></a><span data-ttu-id="8f92d-110">Distribuera hello-lösning med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8f92d-110">Deploying hello solution using hello Azure CLI</span></span>

<span data-ttu-id="8f92d-111">hello följande steg visar hur toocreate Internet-riktade belastningsutjämnaren med Azure Resource Manager med CLI.</span><span class="sxs-lookup"><span data-stu-id="8f92d-111">hello following steps show how toocreate an Internet facing load balancer using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="8f92d-112">Med Azure Resource Manager varje resurs skapas och konfigureras separat, sedan samlat toocreate en resurs.</span><span class="sxs-lookup"><span data-stu-id="8f92d-112">With Azure Resource Manager each resource is created and configured individually, then put together toocreate a resource.</span></span>

<span data-ttu-id="8f92d-113">Du måste skapa och konfigurera hello följande objekt toodeploy en belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="8f92d-113">You must create and configure hello following objects toodeploy a load balancer:</span></span>

* <span data-ttu-id="8f92d-114">IP-konfiguration på klientsidan – innehåller offentliga IP-adresser för inkommande nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="8f92d-114">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="8f92d-115">Backend-adresspool - innehåller nätverksgränssnitt (NIC) för virtuella datorer hello tooreceive nätverkstrafik från hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="8f92d-115">Back-end address pool - contains network interfaces (NICs) for hello virtual machines tooreceive network traffic from hello load balancer.</span></span>
* <span data-ttu-id="8f92d-116">Belastningsutjämning regler - innehåller regler för mappning av en offentlig port på hello belastningen belastningsutjämnaren tooport i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="8f92d-116">Load balancing rules - contains rules mapping a public port on hello load balancer tooport in hello back-end address pool.</span></span>
* <span data-ttu-id="8f92d-117">Inkommande NAT-regler - innehåller regler för mappning av en offentlig port på hello belastningen belastningsutjämnaren tooa port för en specifik virtuell dator i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="8f92d-117">Inbound NAT rules - contains rules mapping a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool.</span></span>
* <span data-ttu-id="8f92d-118">Avsökningar - innehåller hälsa-avsökningar som används toocheck tillgängligheten för virtuella datorer instanser i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="8f92d-118">Probes - contains health probes used toocheck availability of virtual machines instances in hello back-end address pool.</span></span>

<span data-ttu-id="8f92d-119">Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="8f92d-119">For more information see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-cli-toouse-resource-manager"></a><span data-ttu-id="8f92d-120">Ställ in CLI toouse Resource Manager</span><span class="sxs-lookup"><span data-stu-id="8f92d-120">Set up CLI toouse Resource Manager</span></span>

1. <span data-ttu-id="8f92d-121">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8f92d-121">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="8f92d-122">Kör hello **azure config mode** kommandot tooswitch tooResource Manager-läge enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="8f92d-122">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
        azure config mode arm
    ```

    <span data-ttu-id="8f92d-123">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="8f92d-123">Expected output:</span></span>

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a><span data-ttu-id="8f92d-124">Skapa ett virtuellt nätverk och en offentlig IP-adress för hello frontend IP-adresspool</span><span class="sxs-lookup"><span data-stu-id="8f92d-124">Create a virtual network and a public IP address for hello front-end IP pool</span></span>

1. <span data-ttu-id="8f92d-125">Skapa ett virtuellt nätverk (VNet) med namnet *NRPVnet* i hello östra USA plats med hjälp av en resursgrupp med namnet *NRPRG*.</span><span class="sxs-lookup"><span data-stu-id="8f92d-125">Create a virtual network (VNet) named *NRPVnet* in hello East US location using a resource group named *NRPRG*.</span></span>

    ```azurecli
        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16
    ```

    <span data-ttu-id="8f92d-126">Skapa ett undernät med namnet *NRPVnetSubnet* med CIDR-blocket 10.0.0.0/24 i *NRPVnet*.</span><span class="sxs-lookup"><span data-stu-id="8f92d-126">Create a subnet named *NRPVnetSubnet* with a CIDR block of 10.0.0.0/24 in *NRPVnet*.</span></span>

    ```azurecli
        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24
    ```

2. <span data-ttu-id="8f92d-127">Skapa en offentlig IP-adress med namnet *NRPPublicIP* toobe som används av en frontend IP-adresspool med DNS-namnet *loadbalancernrp.eastus.cloudapp.azure.com*. hello kommandot nedan använder hello statisk tilldelningsmetod typ och timeout för inaktivitet 4 minuter.</span><span class="sxs-lookup"><span data-stu-id="8f92d-127">Create a public IP address named *NRPPublicIP* toobe used by a front-end IP pool with DNS name *loadbalancernrp.eastus.cloudapp.azure.com*. hello command below uses hello static allocation type and idle timeout of 4 minutes.</span></span>

    ```azurecli
        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="8f92d-128">hello belastningsutjämnare använder hello domänetikett hello offentlig IP-adress som dess FQDN.</span><span class="sxs-lookup"><span data-stu-id="8f92d-128">hello load balancer will use hello domain label of hello public IP as its FQDN.</span></span> <span data-ttu-id="8f92d-129">Denna ändring från klassisk distribution som använder hello molnbaserad tjänst som hello belastningsutjämnaren fullständigt kvalificerade domännamn (FQDN).</span><span class="sxs-lookup"><span data-stu-id="8f92d-129">This a change from classic deployment, which uses hello cloud service as hello load balancer Fully Qualified Domain Name (FQDN).</span></span>
   > <span data-ttu-id="8f92d-130">I det här exemplet hello FQDN är *loadbalancernrp.eastus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="8f92d-130">In this example, hello FQDN is *loadbalancernrp.eastus.cloudapp.azure.com*.</span></span>

## <a name="create-a-load-balancer"></a><span data-ttu-id="8f92d-131">Skapa en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8f92d-131">Create a load balancer</span></span>

<span data-ttu-id="8f92d-132">hello följande kommando skapar en belastningsutjämnare med namnet *NRPlb* i hello *NRPRG* resursgrupp i hello *östra USA* Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="8f92d-132">hello following command creates a load balancer named *NRPlb* in hello *NRPRG* resource group in hello *East US* Azure location.</span></span>

    ```azurecli
    azure network lb create NRPRG NRPlb eastus
    ```

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a><span data-ttu-id="8f92d-133">Skapa en IP-adresspool på klientsidan och en backend-adresspool</span><span class="sxs-lookup"><span data-stu-id="8f92d-133">Create a front-end IP pool and a backend address pool</span></span>
<span data-ttu-id="8f92d-134">Det här exemplet visar hur toocreate hello frontend IP-pool som tar emot hello inkommande nätverkstrafik på hello belastningsutjämnare och hello IP serverdelspool där hello frontend poolen skickar hello belastningsutjämnad trafik.</span><span class="sxs-lookup"><span data-stu-id="8f92d-134">This example demonstrates how toocreate hello front-end IP pool that receives hello incoming network traffic on hello load balancer and hello backend IP pool where hello front-end pool sends hello load balanced network traffic.</span></span>

1. <span data-ttu-id="8f92d-135">Skapa en frontend IP-adresspool som kopplar hello offentliga IP-Adressen skapas i hello föregående steg och hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="8f92d-135">Create a front-end IP pool associating hello public IP created in hello previous step and hello load balancer.</span></span>

    ```azurecli
        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip
    ```

2. <span data-ttu-id="8f92d-136">Ställ in en backend-adresspool används tooreceive inkommande trafik från hello frontend IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="8f92d-136">Set up a back-end address pool used tooreceive incoming traffic from hello front-end IP pool.</span></span>

    ```azurecli
        azure network lb address-pool create NRPRG NRPlb NRPbackendpool
    ```

## <a name="create-lb-rules-nat-rules-and-probe"></a><span data-ttu-id="8f92d-137">Skapa LB-regler, NAT-regler och avsökning</span><span class="sxs-lookup"><span data-stu-id="8f92d-137">Create LB rules, NAT rules, and probe</span></span>

<span data-ttu-id="8f92d-138">Det här exemplet skapar hello följande objekt.</span><span class="sxs-lookup"><span data-stu-id="8f92d-138">This example creates hello following items.</span></span>

* <span data-ttu-id="8f92d-139">en NAT-regel tootranslate all inkommande trafik på port 21 tooport 22<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="8f92d-139">a NAT rule tootranslate all incoming traffic on port 21 tooport 22<sup>1</sup></span></span>
* <span data-ttu-id="8f92d-140">en NAT-regel tootranslate all inkommande trafik på port 23 tooport 22</span><span class="sxs-lookup"><span data-stu-id="8f92d-140">a NAT rule tootranslate all incoming traffic on port 23 tooport 22</span></span>
* <span data-ttu-id="8f92d-141">en belastningen belastningsutjämnaren regeln toobalance all inkommande trafik på port 80 tooport 80 på hello adresser i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="8f92d-141">a load balancer rule toobalance all incoming traffic on port 80 tooport 80 on hello addresses in hello back-end pool.</span></span>
* <span data-ttu-id="8f92d-142">en avsökning regeln toocheck hello hälsostatus på en sida med namnet *HealthProbe.aspx*.</span><span class="sxs-lookup"><span data-stu-id="8f92d-142">a probe rule toocheck hello health status on a page named *HealthProbe.aspx*.</span></span>

<span data-ttu-id="8f92d-143"><sup>1</sup> NAT-regler är associerade tooa specifik virtuell datorinstans bakom hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="8f92d-143"><sup>1</sup> NAT rules are associated tooa specific virtual machine instance behind hello load balancer.</span></span> <span data-ttu-id="8f92d-144">hello nätverkstrafik som inkommer på port 21 skickas tooa specifik virtuell dator på port 22 som associeras med den här NAT-regel.</span><span class="sxs-lookup"><span data-stu-id="8f92d-144">hello network traffic arriving on port 21 is sent tooa specific virtual machine on port 22 associated with this NAT rule.</span></span> <span data-ttu-id="8f92d-145">Du måste ange ett protokoll (UDP eller TCP) för en NAT-regel.</span><span class="sxs-lookup"><span data-stu-id="8f92d-145">You must specify a protocol (UDP or TCP) for a NAT rule.</span></span> <span data-ttu-id="8f92d-146">Båda protokollen kan inte vara tilldelad toohello samma port.</span><span class="sxs-lookup"><span data-stu-id="8f92d-146">Both protocols can't be assigned toohello same port.</span></span>

1. <span data-ttu-id="8f92d-147">Skapa hello NAT-regler.</span><span class="sxs-lookup"><span data-stu-id="8f92d-147">Create hello NAT rules.</span></span>

    ```azurecli
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22
    ```

2. <span data-ttu-id="8f92d-148">Skapa en belastningsutjämningsregel.</span><span class="sxs-lookup"><span data-stu-id="8f92d-148">Create a load balancer rule.</span></span>

    ```azurecli
        azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name NRPfrontendpool --backend-address-pool-name NRPbackendpool
    ```

3. <span data-ttu-id="8f92d-149">Skapa en hälsoavsökning.</span><span class="sxs-lookup"><span data-stu-id="8f92d-149">Create a health probe.</span></span>

    ```azurecli
        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4
    ```

4. <span data-ttu-id="8f92d-150">Kontrollera inställningarna.</span><span class="sxs-lookup"><span data-stu-id="8f92d-150">Check your settings.</span></span>

    ```azurecli
        azure network lb show nrprg nrplb
    ```

    <span data-ttu-id="8f92d-151">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="8f92d-151">Expected output:</span></span>

        info:    Executing command network lb show
        + Looking up hello load balancer "nrplb"
        + Looking up hello public ip "NRPPublicIP"
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

## <a name="create-nics"></a><span data-ttu-id="8f92d-152">Skapa nätverkskort</span><span class="sxs-lookup"><span data-stu-id="8f92d-152">Create NICs</span></span>

<span data-ttu-id="8f92d-153">Du behöver toocreate nätverkskort (eller ändra befintliga) och kopplar dem tooNAT regler, regler för inläsning av belastningsutjämnare och avsökningar.</span><span class="sxs-lookup"><span data-stu-id="8f92d-153">You need toocreate NICs (or modify existing ones) and associate them tooNAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="8f92d-154">Skapa ett nätverkskort med namnet *lb nic1 vara*, och koppla den till hello *rdp1* NAT-regel och hello *NRPbackendpool* backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="8f92d-154">Create a NIC named *lb-nic1-be*, and associate it with hello *rdp1* NAT rule, and hello *NRPbackendpool* back-end address pool.</span></span>

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
    ```

    <span data-ttu-id="8f92d-155">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="8f92d-155">Expected output:</span></span>

        info:    Executing command network nic create
        + Looking up hello network interface "lb-nic1-be"
        + Looking up hello subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up hello network interface "lb-nic1-be"
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

2. <span data-ttu-id="8f92d-156">Skapa ett nätverkskort med namnet *lb nic2 vara*, och koppla den till hello *rdp2* NAT-regel och hello *NRPbackendpool* backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="8f92d-156">Create a NIC named *lb-nic2-be*, and associate it with hello *rdp2* NAT rule, and hello *NRPbackendpool* back-end address pool.</span></span>

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
    ```

3. <span data-ttu-id="8f92d-157">Skapa en virtuell dator (VM) med namnet *web1*, och koppla den till hello NIC med namnet *lb nic1 vara*.</span><span class="sxs-lookup"><span data-stu-id="8f92d-157">Create a virtual machine (VM) named *web1*, and associate it with hello NIC named *lb-nic1-be*.</span></span> <span data-ttu-id="8f92d-158">Ett lagringskonto kallas *web1nrp* har skapats innan du kör hello kommandot nedan.</span><span class="sxs-lookup"><span data-stu-id="8f92d-158">A storage account called *web1nrp* was created before running hello command below.</span></span>

    ```azurecli
        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="8f92d-159">Virtuella datorer i load belastningsutjämnaren måste toobe i hello samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="8f92d-159">VMs in a load balancer need toobe in hello same availability set.</span></span> <span data-ttu-id="8f92d-160">Använd `azure availset create` toocreate en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="8f92d-160">Use `azure availset create` toocreate an availability set.</span></span>

    <span data-ttu-id="8f92d-161">hello utdata ska vara liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="8f92d-161">hello output should be similar toohello following:</span></span>

        info:    Executing command vm create
        + Looking up hello VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using hello VM Size "Standard_A1"
        info:    hello [OS, Data] Disk or image configuration requires storage account
        + Looking up hello storage account web1nrp
        + Looking up hello availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up hello NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in hello NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    > [!NOTE]
    > <span data-ttu-id="8f92d-162">hello informationsmeddelande **detta är ett nätverkskort utan publicIP konfigurerad** förväntas eftersom hello skapa nätverkskort för hello belastningsutjämnaren ansluter tooInternet med hello offentliga IP-adressen för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="8f92d-162">hello informational message **This is a NIC without publicIP configured** is expected since hello NIC created for hello load balancer connecting tooInternet using hello load balancer public IP address.</span></span>

    <span data-ttu-id="8f92d-163">Eftersom hello *lb nic1 vara* NIC som är associerad med hello *rdp1* NAT-regel, du kan ansluta för*web1* med RDP via port 3441 på hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="8f92d-163">Since hello *lb-nic1-be* NIC is associated with hello *rdp1* NAT rule, you can connect too*web1* using RDP through port 3441 on hello load balancer.</span></span>

4. <span data-ttu-id="8f92d-164">Skapa en virtuell dator (VM) med namnet *web2*, och koppla den till hello NIC med namnet *lb nic2 vara*.</span><span class="sxs-lookup"><span data-stu-id="8f92d-164">Create a virtual machine (VM) named *web2*, and associate it with hello NIC named *lb-nic2-be*.</span></span> <span data-ttu-id="8f92d-165">Ett lagringskonto kallas *web1nrp* har skapats innan du kör hello kommandot nedan.</span><span class="sxs-lookup"><span data-stu-id="8f92d-165">A storage account called *web1nrp* was created before running hello command below.</span></span>

    ```azurecli
        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="update-an-existing-load-balancer"></a><span data-ttu-id="8f92d-166">Uppdatera en befintlig belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8f92d-166">Update an existing load balancer</span></span>
<span data-ttu-id="8f92d-167">Du kan lägga till regler som refererar till en befintlig belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="8f92d-167">You can add rules referencing an existing load balancer.</span></span> <span data-ttu-id="8f92d-168">I nästa exempel hello läggs en ny regel för belastningsutjämnare tooan befintliga belastningsutjämnaren **NRPlb**</span><span class="sxs-lookup"><span data-stu-id="8f92d-168">In hello next example, a new load balancer rule is added tooan existing load balancer **NRPlb**</span></span>

```azurecli
azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool
```

## <a name="delete-a-load-balancer"></a><span data-ttu-id="8f92d-169">Ta bort en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8f92d-169">Delete a load balancer</span></span>
<span data-ttu-id="8f92d-170">Använd följande kommando tooremove belastningsutjämning hello:</span><span class="sxs-lookup"><span data-stu-id="8f92d-170">Use hello following command tooremove a load balancer:</span></span>

```azurecli
azure network lb delete --resource-group nrprg --name nrplb
```

## <a name="next-steps"></a><span data-ttu-id="8f92d-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f92d-171">Next steps</span></span>
[<span data-ttu-id="8f92d-172">Komma igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8f92d-172">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-cli.md)

[<span data-ttu-id="8f92d-173">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8f92d-173">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="8f92d-174">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8f92d-174">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
