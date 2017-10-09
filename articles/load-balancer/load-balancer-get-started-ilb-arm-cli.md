---
title: "aaaCreate en intern belastningsutjämnare - Azure CLI | Microsoft Docs"
description: "Lär dig hur toocreate en intern belastningsutjämnare med hjälp av hello Azure CLI i Resource Manager"
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
ms.openlocfilehash: 3aea6fdb07600f0d661ec6b8ffc784b03380a127
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-by-using-hello-azure-cli"></a><span data-ttu-id="7470c-103">Skapa en intern belastningsutjämnare med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7470c-103">Create an internal load balancer by using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7470c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7470c-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="7470c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7470c-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="7470c-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7470c-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="7470c-107">Mall</span><span class="sxs-lookup"><span data-stu-id="7470c-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="7470c-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7470c-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="7470c-109">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](load-balancer-get-started-ilb-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="7470c-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](load-balancer-get-started-ilb-classic-cli.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-hello-solution-by-using-hello-azure-cli"></a><span data-ttu-id="7470c-110">Distribuera hello lösning med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7470c-110">Deploy hello solution by using hello Azure CLI</span></span>

<span data-ttu-id="7470c-111">hello följande steg visar hur toocreate en Internetriktade belastningsutjämnare med hjälp av Azure Resource Manager med CLI.</span><span class="sxs-lookup"><span data-stu-id="7470c-111">hello following steps show how toocreate an Internet-facing load balancer by using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="7470c-112">Med Azure Resource Manager varje resurs har skapats och konfigurerats individuellt och sedan sätta ihop toocreate en resurs.</span><span class="sxs-lookup"><span data-stu-id="7470c-112">With Azure Resource Manager, each resource is created and configured individually, and then put together toocreate a resource.</span></span>

<span data-ttu-id="7470c-113">Du behöver toocreate och konfigurera hello följande objekt toodeploy en belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="7470c-113">You need toocreate and configure hello following objects toodeploy a load balancer:</span></span>

* <span data-ttu-id="7470c-114">**IP-konfiguration på klientsidan**: innehåller offentliga IP-adresser för inkommande nätverkstrafik</span><span class="sxs-lookup"><span data-stu-id="7470c-114">**Front-end IP configuration**: contains public IP addresses for incoming network traffic</span></span>
* <span data-ttu-id="7470c-115">**Backend-adresspool**: innehåller nätverksgränssnitt (NIC) som gör att virtuella datorer hello tooreceive nätverkstrafik från hello belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="7470c-115">**Back-end address pool**: contains network interfaces (NICs) that enable hello virtual machines tooreceive network traffic from hello load balancer</span></span>
* <span data-ttu-id="7470c-116">**Regler för belastningsutjämning**: innehåller regler som mappar en offentlig port på hello belastningen belastningsutjämnaren tooport i hello backend-adresspool</span><span class="sxs-lookup"><span data-stu-id="7470c-116">**Load-balancing rules**: contains rules that map a public port on hello load balancer tooport in hello back-end address pool</span></span>
* <span data-ttu-id="7470c-117">**Inkommande NAT-regler**: innehåller regler som mappar en offentlig port på hello belastningen belastningsutjämnaren tooa port för en specifik virtuell dator i hello backend-adresspool</span><span class="sxs-lookup"><span data-stu-id="7470c-117">**Inbound NAT rules**: contains rules that map a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool</span></span>
* <span data-ttu-id="7470c-118">**Avsökningar**: innehåller hälsoavsökningar som används toocheck hello tillgängligheten för virtuella datorer instanser i hello backend-adresspool</span><span class="sxs-lookup"><span data-stu-id="7470c-118">**Probes**: contains health probes that are used toocheck hello availability of virtual machines instances in hello back-end address pool</span></span>

<span data-ttu-id="7470c-119">Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="7470c-119">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-cli-toouse-resource-manager"></a><span data-ttu-id="7470c-120">Ställ in CLI toouse Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7470c-120">Set up CLI toouse Resource Manager</span></span>

1. <span data-ttu-id="7470c-121">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="7470c-121">If you have never used Azure CLI, see [Install and configure hello Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="7470c-122">Följ instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7470c-122">Follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="7470c-123">Kör hello **azure config mode** kommandot tooswitch tooResource Manager-läge, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="7470c-123">Run hello **azure config mode** command tooswitch tooResource Manager mode, as follows:</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="7470c-124">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="7470c-124">Expected output:</span></span>

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a><span data-ttu-id="7470c-125">Stegvisa anvisningar för hur du skapar en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="7470c-125">Create an internal load balancer step by step</span></span>

1. <span data-ttu-id="7470c-126">Logga in tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7470c-126">Sign in tooAzure.</span></span>

    ```azurecli
    azure login
    ```

    <span data-ttu-id="7470c-127">Ange inte dina autentiseringsuppgifter för Azure när du uppmanas göra det.</span><span class="sxs-lookup"><span data-stu-id="7470c-127">When prompted, enter your Azure credentials.</span></span>

2. <span data-ttu-id="7470c-128">Ändra hello kommandot verktyg tooAzure Resource Manager-läget.</span><span class="sxs-lookup"><span data-stu-id="7470c-128">Change hello command tools tooAzure Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

## <a name="create-a-resource-group"></a><span data-ttu-id="7470c-129">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="7470c-129">Create a resource group</span></span>

<span data-ttu-id="7470c-130">Alla resurser i Azure Resource Manager är kopplade till en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7470c-130">All resources in Azure Resource Manager are associated with a resource group.</span></span> <span data-ttu-id="7470c-131">Om du inte redan gjort det skapar du en resursgrupp nu.</span><span class="sxs-lookup"><span data-stu-id="7470c-131">If you haven't done so yet, create a resource group.</span></span>

```azurecli
azure group create <resource group name> <location>
```

## <a name="create-an-internal-load-balancer-set"></a><span data-ttu-id="7470c-132">Skapa en intern belastningsutjämningsuppsättning</span><span class="sxs-lookup"><span data-stu-id="7470c-132">Create an internal load balancer set</span></span>

1. <span data-ttu-id="7470c-133">Skapa en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="7470c-133">Create an internal load balancer</span></span>

    <span data-ttu-id="7470c-134">I följande scenario hello, skapas en resursgrupp med namnet nrprg i östra USA.</span><span class="sxs-lookup"><span data-stu-id="7470c-134">In hello following scenario, a resource group named nrprg is created in East US region.</span></span>

    ```azurecli
    azure network lb create --name nrprg --location eastus
    ```

   > [!NOTE]
   > <span data-ttu-id="7470c-135">Alla resurser för en intern belastningsutjämnare, till exempel virtuella nätverk och undernät för virtuellt nätverk, måste vara i samma resursgrupp hello och hello i samma region.</span><span class="sxs-lookup"><span data-stu-id="7470c-135">All resources for an internal load balancers, such as virtual networks and virtual network subnets, must be in hello same resource group and in hello same region.</span></span>

2. <span data-ttu-id="7470c-136">Skapa en frontend IP-adress för hello intern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="7470c-136">Create a front-end IP address for hello internal load balancer.</span></span>

    <span data-ttu-id="7470c-137">hello IP-adress som du använder måste vara inom intervallet för hello undernät för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="7470c-137">hello IP address that you use must be within hello subnet range of your virtual network.</span></span>

    ```azurecli
    azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet
    ```

3. <span data-ttu-id="7470c-138">Skapa hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="7470c-138">Create hello back-end address pool.</span></span>

    ```azurecli
    azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb
    ```

    <span data-ttu-id="7470c-139">När du har definierat en IP-adress för klientdelen och en serverdelsadresspool kan du skapa belastningsutjämningsregler, ingående NAT-regler och anpassade hälsoavsökningar.</span><span class="sxs-lookup"><span data-stu-id="7470c-139">After you define a front-end IP address and a back-end address pool, you can create load balancer rules, inbound NAT rules, and customized health probes.</span></span>

4. <span data-ttu-id="7470c-140">Skapa en regel för belastningsutjämnare för hello intern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="7470c-140">Create a load balancer rule for hello internal load balancer.</span></span>

    <span data-ttu-id="7470c-141">När du följer stegen för hello hello kommando skapar en regel för belastningsutjämning för lyssnande tooport 1433 i hello frontend poolen och skicka belastningsutjämnad trafik toohello backend-adresspool för nätverk, också använder port 1433.</span><span class="sxs-lookup"><span data-stu-id="7470c-141">When you follow hello previous steps, hello command creates a load-balancer rule for listening tooport 1433 in hello front-end pool and sending load-balanced network traffic toohello back-end address pool, also using port 1433.</span></span>

    ```azurecli
    azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb
    ```

5. <span data-ttu-id="7470c-142">Skapa ingående NAT-regler.</span><span class="sxs-lookup"><span data-stu-id="7470c-142">Create inbound NAT rules.</span></span>

    <span data-ttu-id="7470c-143">Inkommande NAT-regler finns används toocreate slutpunkter i en belastningsutjämnare som går tooa specifik virtuell dator-instansen.</span><span class="sxs-lookup"><span data-stu-id="7470c-143">Inbound NAT rules are used toocreate endpoints in a load balancer that go tooa specific virtual machine instance.</span></span> <span data-ttu-id="7470c-144">hello föregående steg skapa två NAT-regler för fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="7470c-144">hello previous steps created two NAT rules  for remote desktop.</span></span>

    ```azurecli
    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389
    ```

6. <span data-ttu-id="7470c-145">Skapa hälsoavsökningar för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="7470c-145">Create health probes for hello load balancer.</span></span>

    <span data-ttu-id="7470c-146">En hälsoavsökningen kontrollerar alla virtuella instanser toomake att de kan skicka nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="7470c-146">A health probe checks all virtual machine instances toomake sure they can send network traffic.</span></span> <span data-ttu-id="7470c-147">hello virtuella instans med misslyckade avsökning kontrollerar tas bort från belastningsutjämnaren hello tills den går tillbaka online och en avsökning kontroll anger att den är felfri.</span><span class="sxs-lookup"><span data-stu-id="7470c-147">hello virtual machine instance with failed probe checks is removed from hello load balancer until it goes back online and a probe check determines that it's healthy.</span></span>

    ```azurecli
    azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4
    ```

    > [!NOTE]
    > <span data-ttu-id="7470c-148">hello Microsoft Azure-plattformen använder en statisk, offentligt dirigerbara IPv4-adress för en mängd olika administrativa scenarier.</span><span class="sxs-lookup"><span data-stu-id="7470c-148">hello Microsoft Azure platform uses a static, publicly routable IPv4 address for a variety of administrative scenarios.</span></span> <span data-ttu-id="7470c-149">hello IP-adressen är 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="7470c-149">hello IP address is 168.63.129.16.</span></span> <span data-ttu-id="7470c-150">Den här IP-adressen får inte blockeras av eventuella brandväggar eftersom det kan orsaka oväntade resultat.</span><span class="sxs-lookup"><span data-stu-id="7470c-150">This IP address should not be blocked by any firewalls, because this can cause unexpected behavior.</span></span>
    > <span data-ttu-id="7470c-151">Avseende tooAzure intern belastningsutjämning används den här IP-adress genom att övervaka avsökningar från hello belastningen belastningsutjämnaren toodetermine hello hälsotillståndet för virtuella datorer i en belastningsutjämnad uppsättning.</span><span class="sxs-lookup"><span data-stu-id="7470c-151">With respect tooAzure internal load balancing, this IP address is used by monitoring probes from hello load balancer toodetermine hello health state for virtual machines in a load-balanced set.</span></span> <span data-ttu-id="7470c-152">Om en nätverkssäkerhetsgrupp används toorestrict trafik tooAzure virtuella datorer i en internt belastningsutjämnad uppsättning eller är tillämpade tooa undernät för virtuellt nätverk, se till att en nätverkssäkerhetsregeln läggs tooallow trafik från 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="7470c-152">If a network security group is used toorestrict traffic tooAzure virtual machines in an internally load-balanced set or is applied tooa virtual network subnet, ensure that a network security rule is added tooallow traffic from 168.63.129.16.</span></span>

## <a name="create-nics"></a><span data-ttu-id="7470c-153">Skapa nätverkskort</span><span class="sxs-lookup"><span data-stu-id="7470c-153">Create NICs</span></span>

<span data-ttu-id="7470c-154">Du behöver toocreate nätverkskort (eller ändra befintliga) och kopplar dem tooNAT regler, regler för inläsning av belastningsutjämnare och avsökningar.</span><span class="sxs-lookup"><span data-stu-id="7470c-154">You need toocreate NICs (or modify existing ones) and associate them tooNAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="7470c-155">Skapa ett nätverkskort med namnet *lb nic1 vara*, och koppla den till hello *rdp1* NAT regeln och hello *beilb* backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="7470c-155">Create an NIC named *lb-nic1-be*, and then associate it with hello *rdp1* NAT rule and hello *beilb* back-end address pool.</span></span>

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus
    ```

    <span data-ttu-id="7470c-156">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="7470c-156">Expected output:</span></span>

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

2. <span data-ttu-id="7470c-157">Skapa ett nätverkskort med namnet *lb nic2 vara*, och koppla den till hello *rdp2* NAT regeln och hello *beilb* backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="7470c-157">Create an NIC named *lb-nic2-be*, and then associate it with hello *rdp2* NAT rule and hello *beilb* back-end address pool.</span></span>

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus
    ```

3. <span data-ttu-id="7470c-158">Skapa en virtuell dator med namnet *DB1*, och koppla den till hello NIC med namnet *lb nic1 vara*.</span><span class="sxs-lookup"><span data-stu-id="7470c-158">Create a virtual machine named *DB1*, and then associate it with hello NIC named *lb-nic1-be*.</span></span> <span data-ttu-id="7470c-159">Ett lagringskonto kallas *web1nrp* har skapats innan hello efter kommandot körs:</span><span class="sxs-lookup"><span data-stu-id="7470c-159">A storage account called *web1nrp* is created before hello following command runs:</span></span>

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```
    > [!IMPORTANT]
    > <span data-ttu-id="7470c-160">Virtuella datorer i load belastningsutjämnaren måste toobe i hello samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="7470c-160">VMs in a load balancer need toobe in hello same availability set.</span></span> <span data-ttu-id="7470c-161">Använd `azure availset create` toocreate en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="7470c-161">Use `azure availset create` toocreate an availability set.</span></span>

4. <span data-ttu-id="7470c-162">Skapa en virtuell dator (VM) med namnet *DB2*, och koppla den till hello NIC med namnet *lb nic2 vara*.</span><span class="sxs-lookup"><span data-stu-id="7470c-162">Create a virtual machine (VM) named *DB2*, and then associate it with hello NIC named *lb-nic2-be*.</span></span> <span data-ttu-id="7470c-163">Ett lagringskonto kallas *web1nrp* har skapats innan du kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="7470c-163">A storage account called *web1nrp* was created before running hello following command.</span></span>

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="delete-a-load-balancer"></a><span data-ttu-id="7470c-164">Ta bort en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="7470c-164">Delete a load balancer</span></span>

<span data-ttu-id="7470c-165">tooremove en belastningsutjämnare använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7470c-165">tooremove a load balancer, use hello following command:</span></span>

```azurecli
azure network lb delete --resource-group nrprg --name ilbset
```

## <a name="next-steps"></a><span data-ttu-id="7470c-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7470c-166">Next steps</span></span>

[<span data-ttu-id="7470c-167">Konfigurera ett distributionsläge för en belastningsutjämnare med hjälp av käll-IP-tilldelning</span><span class="sxs-lookup"><span data-stu-id="7470c-167">Configure a load balancer distribution mode by using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="7470c-168">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="7470c-168">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

