---
title: "Skapa en intern belastningsutjämnare – CLI Azure | Microsoft Docs"
description: "Ta reda på hur du skapar en intern belastningsutjämnare med hjälp av Azure CLI i Resource Manager"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c7a24e92-b4da-43c0-90f2-841c1b7ce489
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 128b91c685b5f7e494a69ca5b04165a0ee7cbb78
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a><span data-ttu-id="908ec-103">Skapa en intern belastningsutjämnare med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="908ec-103">Create an internal load balancer by using the Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="908ec-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="908ec-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="908ec-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="908ec-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="908ec-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="908ec-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="908ec-107">Mall</span><span class="sxs-lookup"><span data-stu-id="908ec-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="908ec-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="908ec-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="908ec-109">Den här artikeln beskriver Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för [den klassiska distributionsmodellen](load-balancer-get-started-ilb-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="908ec-109">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](load-balancer-get-started-ilb-classic-cli.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a><span data-ttu-id="908ec-110">Distribuera lösningen med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="908ec-110">Deploy the solution by using the Azure CLI</span></span>

<span data-ttu-id="908ec-111">Följande steg beskriver hur du skapar en Internetuppkopplad belastningsutjämnare med hjälp av Azure Resource Manager med CLI.</span><span class="sxs-lookup"><span data-stu-id="908ec-111">The following steps show how to create an Internet-facing load balancer by using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="908ec-112">Med Azure Resource Manager skapas och konfigureras varje resurs separat, och läggs sedan ihop för att skapa en resurs.</span><span class="sxs-lookup"><span data-stu-id="908ec-112">With Azure Resource Manager, each resource is created and configured individually, and then put together to create a resource.</span></span>

<span data-ttu-id="908ec-113">Du måste skapa och konfigurera följande objekt för att distribuera en belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="908ec-113">You need to create and configure the following objects to deploy a load balancer:</span></span>

* <span data-ttu-id="908ec-114">**IP-konfiguration på klientsidan**: innehåller offentliga IP-adresser för inkommande nätverkstrafik</span><span class="sxs-lookup"><span data-stu-id="908ec-114">**Front-end IP configuration**: contains public IP addresses for incoming network traffic</span></span>
* <span data-ttu-id="908ec-115">**Serverdelsadresspool**: innehåller nätverksgränssnitten (nätverkskort) som gör det möjligt för de virtuella datorerna att ta emot nätverkstrafik från belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="908ec-115">**Back-end address pool**: contains network interfaces (NICs) that enable the virtual machines to receive network traffic from the load balancer</span></span>
* <span data-ttu-id="908ec-116">**Belastningsutjämningsregler**: innehåller regler som mappar en offentlig port på belastningsutjämnaren till en port i serverdelsadresspoolen</span><span class="sxs-lookup"><span data-stu-id="908ec-116">**Load-balancing rules**: contains rules that map a public port on the load balancer to port in the back-end address pool</span></span>
* <span data-ttu-id="908ec-117">**Ingående NAT-regler**: innehåller regler som mappar en offentlig port i belastningsutjämnaren till en port för en specifik virtuell dator i serverdelsadresspoolen</span><span class="sxs-lookup"><span data-stu-id="908ec-117">**Inbound NAT rules**: contains rules that map a public port on the load balancer to a port for a specific virtual machine in the back-end address pool</span></span>
* <span data-ttu-id="908ec-118">**Avsökningar**: innehåller hälsoavsökningar som används för att kontrollera tillgängligheten för virtuella datorer i serverdelsadresspoolen</span><span class="sxs-lookup"><span data-stu-id="908ec-118">**Probes**: contains health probes that are used to check the availability of virtual machines instances in the back-end address pool</span></span>

<span data-ttu-id="908ec-119">Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="908ec-119">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-cli-to-use-resource-manager"></a><span data-ttu-id="908ec-120">Konfigurera CLI för att använda Resource Manager</span><span class="sxs-lookup"><span data-stu-id="908ec-120">Set up CLI to use Resource Manager</span></span>

1. <span data-ttu-id="908ec-121">Om du aldrig har använt Azure CLI hittar du mer information i [Installera och konfigurera Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="908ec-121">If you have never used Azure CLI, see [Install and configure the Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="908ec-122">Följ instruktionerna fram till den punkt där du kan välja Azure-konto och -prenumeration.</span><span class="sxs-lookup"><span data-stu-id="908ec-122">Follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="908ec-123">Kör kommandot **azure config mode** för att växla till Resource Manager-läge, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="908ec-123">Run the **azure config mode** command to switch to Resource Manager mode, as follows:</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="908ec-124">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="908ec-124">Expected output:</span></span>

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a><span data-ttu-id="908ec-125">Stegvisa anvisningar för hur du skapar en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="908ec-125">Create an internal load balancer step by step</span></span>

1. <span data-ttu-id="908ec-126">Logga in i Azure.</span><span class="sxs-lookup"><span data-stu-id="908ec-126">Sign in to Azure.</span></span>

    ```azurecli
    azure login
    ```

    <span data-ttu-id="908ec-127">Ange inte dina autentiseringsuppgifter för Azure när du uppmanas göra det.</span><span class="sxs-lookup"><span data-stu-id="908ec-127">When prompted, enter your Azure credentials.</span></span>

2. <span data-ttu-id="908ec-128">Ändra kommandoverktygen till Azure Resource Manager-läge.</span><span class="sxs-lookup"><span data-stu-id="908ec-128">Change the command tools to Azure Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

## <a name="create-a-resource-group"></a><span data-ttu-id="908ec-129">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="908ec-129">Create a resource group</span></span>

<span data-ttu-id="908ec-130">Alla resurser i Azure Resource Manager är kopplade till en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="908ec-130">All resources in Azure Resource Manager are associated with a resource group.</span></span> <span data-ttu-id="908ec-131">Om du inte redan gjort det skapar du en resursgrupp nu.</span><span class="sxs-lookup"><span data-stu-id="908ec-131">If you haven't done so yet, create a resource group.</span></span>

```azurecli
azure group create <resource group name> <location>
```

## <a name="create-an-internal-load-balancer-set"></a><span data-ttu-id="908ec-132">Skapa en intern belastningsutjämningsuppsättning</span><span class="sxs-lookup"><span data-stu-id="908ec-132">Create an internal load balancer set</span></span>

1. <span data-ttu-id="908ec-133">Skapa en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="908ec-133">Create an internal load balancer</span></span>

    <span data-ttu-id="908ec-134">I följande scenario skapas en resursgrupp med namnet nrprg i regionen för östra USA.</span><span class="sxs-lookup"><span data-stu-id="908ec-134">In the following scenario, a resource group named nrprg is created in East US region.</span></span>

    ```azurecli
    azure network lb create --name nrprg --location eastus
    ```

   > [!NOTE]
   > <span data-ttu-id="908ec-135">Alla resurser för en intern belastningsutjämnare, till exempel virtuella nätverk och undernät i virtuella nätverk, måste vara i samma resursgrupp och i samma region.</span><span class="sxs-lookup"><span data-stu-id="908ec-135">All resources for an internal load balancers, such as virtual networks and virtual network subnets, must be in the same resource group and in the same region.</span></span>

2. <span data-ttu-id="908ec-136">Skapa en IP-adress för klientdelen för den interna belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="908ec-136">Create a front-end IP address for the internal load balancer.</span></span>

    <span data-ttu-id="908ec-137">IP-adressen som du använder måste vara inom undernätsintervallet för ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="908ec-137">The IP address that you use must be within the subnet range of your virtual network.</span></span>

    ```azurecli
    azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet
    ```

3. <span data-ttu-id="908ec-138">Skapa serverdelsadresspoolen.</span><span class="sxs-lookup"><span data-stu-id="908ec-138">Create the back-end address pool.</span></span>

    ```azurecli
    azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb
    ```

    <span data-ttu-id="908ec-139">När du har definierat en IP-adress för klientdelen och en serverdelsadresspool kan du skapa belastningsutjämningsregler, ingående NAT-regler och anpassade hälsoavsökningar.</span><span class="sxs-lookup"><span data-stu-id="908ec-139">After you define a front-end IP address and a back-end address pool, you can create load balancer rules, inbound NAT rules, and customized health probes.</span></span>

4. <span data-ttu-id="908ec-140">Skapa en belastningsutjämningsregel för den interna belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="908ec-140">Create a load balancer rule for the internal load balancer.</span></span>

    <span data-ttu-id="908ec-141">När du följer de här stegen skapar kommandot en belastningsutjämningsregel för avlyssning av port 1433 i klientdelspoolen och belastningsutjämnad nätverkstrafik skickas till serverdelsadresspoolen, som också använder port 1433.</span><span class="sxs-lookup"><span data-stu-id="908ec-141">When you follow the previous steps, the command creates a load-balancer rule for listening to port 1433 in the front-end pool and sending load-balanced network traffic to the back-end address pool, also using port 1433.</span></span>

    ```azurecli
    azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb
    ```

5. <span data-ttu-id="908ec-142">Skapa ingående NAT-regler.</span><span class="sxs-lookup"><span data-stu-id="908ec-142">Create inbound NAT rules.</span></span>

    <span data-ttu-id="908ec-143">Ingående NAT-regler används för att skapa slutpunkter i en belastningsutjämnare som leder till en specifik virtuell datorinstans.</span><span class="sxs-lookup"><span data-stu-id="908ec-143">Inbound NAT rules are used to create endpoints in a load balancer that go to a specific virtual machine instance.</span></span> <span data-ttu-id="908ec-144">I föregående steg skapade du två NAT-regler för fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="908ec-144">The previous steps created two NAT rules  for remote desktop.</span></span>

    ```azurecli
    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389
    ```

6. <span data-ttu-id="908ec-145">Skapa hälsoavsökningar för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="908ec-145">Create health probes for the load balancer.</span></span>

    <span data-ttu-id="908ec-146">En hälsoavsökning kontrollerar alla virtuella datorinstanser för att säkerställa att de kan skicka nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="908ec-146">A health probe checks all virtual machine instances to make sure they can send network traffic.</span></span> <span data-ttu-id="908ec-147">Den virtuella datorinstansen med misslyckad hälsoavsökning tas bort från belastningsutjämnaren tills den är tillbaka online och en avsökningskontroll visar att den är felfri.</span><span class="sxs-lookup"><span data-stu-id="908ec-147">The virtual machine instance with failed probe checks is removed from the load balancer until it goes back online and a probe check determines that it's healthy.</span></span>

    ```azurecli
    azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4
    ```

    > [!NOTE]
    > <span data-ttu-id="908ec-148">Microsoft Azure-plattformen använder en statisk, offentligt dirigerbar IPv4-adress för en rad olika administrativa scenarier.</span><span class="sxs-lookup"><span data-stu-id="908ec-148">The Microsoft Azure platform uses a static, publicly routable IPv4 address for a variety of administrative scenarios.</span></span> <span data-ttu-id="908ec-149">IP-adressen är 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="908ec-149">The IP address is 168.63.129.16.</span></span> <span data-ttu-id="908ec-150">Den här IP-adressen får inte blockeras av eventuella brandväggar eftersom det kan orsaka oväntade resultat.</span><span class="sxs-lookup"><span data-stu-id="908ec-150">This IP address should not be blocked by any firewalls, because this can cause unexpected behavior.</span></span>
    > <span data-ttu-id="908ec-151">Vid belastningsutjämning i Azure används den här IP-adressen av övervakningsavsökningar från belastningsutjämnaren för att fastställa hälsotillståndet hos virtuella datorer i en belastningsutjämnad uppsättning.</span><span class="sxs-lookup"><span data-stu-id="908ec-151">With respect to Azure internal load balancing, this IP address is used by monitoring probes from the load balancer to determine the health state for virtual machines in a load-balanced set.</span></span> <span data-ttu-id="908ec-152">Om en nätverkssäkerhetsgrupp används för att begränsa trafik till virtuella datorer i Azure i en internt belastningsutjämnad uppsättning eller om den tillämpas på ett virtuellt nätverksundernät krävs en nätverkssäkerhetsregel som tillåter trafik från 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="908ec-152">If a network security group is used to restrict traffic to Azure virtual machines in an internally load-balanced set or is applied to a virtual network subnet, ensure that a network security rule is added to allow traffic from 168.63.129.16.</span></span>

## <a name="create-nics"></a><span data-ttu-id="908ec-153">Skapa nätverkskort</span><span class="sxs-lookup"><span data-stu-id="908ec-153">Create NICs</span></span>

<span data-ttu-id="908ec-154">Du måste skapa nätverkskort (eller ändra befintliga) och associera dem med NAT-regler, belastningsutjämningsregler och avsökningar.</span><span class="sxs-lookup"><span data-stu-id="908ec-154">You need to create NICs (or modify existing ones) and associate them to NAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="908ec-155">Skapa ett nätverkskort med namnet *lb-nic1-be* och associera till med NAT-regeln *rdp1* och serverdelsadresspoolen *beilb*.</span><span class="sxs-lookup"><span data-stu-id="908ec-155">Create an NIC named *lb-nic1-be*, and then associate it with the *rdp1* NAT rule and the *beilb* back-end address pool.</span></span>

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus
    ```

    <span data-ttu-id="908ec-156">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="908ec-156">Expected output:</span></span>

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

2. <span data-ttu-id="908ec-157">Skapa ett nätverkskort med namnet *lb-nic2-be* och associera det till NAT-regeln *rdp2* och serverdelsadresspoolen *beilb*.</span><span class="sxs-lookup"><span data-stu-id="908ec-157">Create an NIC named *lb-nic2-be*, and then associate it with the *rdp2* NAT rule and the *beilb* back-end address pool.</span></span>

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus
    ```

3. <span data-ttu-id="908ec-158">Skapa en virtuell dator med namnet *DB1* och associera den sedan till nätverkskortet *lb-nic1-be*.</span><span class="sxs-lookup"><span data-stu-id="908ec-158">Create a virtual machine named *DB1*, and then associate it with the NIC named *lb-nic1-be*.</span></span> <span data-ttu-id="908ec-159">Ett lagringskonto med namnet *web1nrp* skapas innan du kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="908ec-159">A storage account called *web1nrp* is created before the following command runs:</span></span>

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```
    > [!IMPORTANT]
    > <span data-ttu-id="908ec-160">Virtuella datorer i en belastningsutjämnare måste finnas i samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="908ec-160">VMs in a load balancer need to be in the same availability set.</span></span> <span data-ttu-id="908ec-161">Använd `azure availset create` för att skapa en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="908ec-161">Use `azure availset create` to create an availability set.</span></span>

4. <span data-ttu-id="908ec-162">Skapa en virtuell dator med namnet *DB2* och associera den till nätverkskortet med namnet *lb-nic2-be*.</span><span class="sxs-lookup"><span data-stu-id="908ec-162">Create a virtual machine (VM) named *DB2*, and then associate it with the NIC named *lb-nic2-be*.</span></span> <span data-ttu-id="908ec-163">Ett lagringskonto med namnet *web1nrp* skapas innan följande kommando kördes.</span><span class="sxs-lookup"><span data-stu-id="908ec-163">A storage account called *web1nrp* was created before running the following command.</span></span>

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="delete-a-load-balancer"></a><span data-ttu-id="908ec-164">Ta bort en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="908ec-164">Delete a load balancer</span></span>

<span data-ttu-id="908ec-165">Ta bort en belastningsutjämnare genom att använda följande kommando:</span><span class="sxs-lookup"><span data-stu-id="908ec-165">To remove a load balancer, use the following command:</span></span>

```azurecli
azure network lb delete --resource-group nrprg --name ilbset
```

## <a name="next-steps"></a><span data-ttu-id="908ec-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="908ec-166">Next steps</span></span>

[<span data-ttu-id="908ec-167">Konfigurera ett distributionsläge för en belastningsutjämnare med hjälp av käll-IP-tilldelning</span><span class="sxs-lookup"><span data-stu-id="908ec-167">Configure a load balancer distribution mode by using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="908ec-168">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="908ec-168">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

