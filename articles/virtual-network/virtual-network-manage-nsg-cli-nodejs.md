---
title: "aaaManage nätverkssäkerhetsgrupper - Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur toomanage nätverkssäkerhetsgrupper med hello Azure-kommandoradsgränssnittet (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9a429f947abbcb5fa6adb40c84504f68efd5e20e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-groups-using-hello-azure-cli-10"></a><span data-ttu-id="10545-103">Hantera nätverkssäkerhetsgrupper med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="10545-103">Manage network security groups using hello Azure CLI 1.0</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="10545-104">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="10545-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="10545-105">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="10545-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="10545-106">[Azure CLI 1.0](#View-existing-NSGs) – våra CLI för hello klassisk och resurs management distributionsmodeller</span><span class="sxs-lookup"><span data-stu-id="10545-106">[Azure CLI 1.0](#View-existing-NSGs) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="10545-107">[Azure CLI 2.0](virtual-network-manage-nsg-arm-cli.md) -vår nästa generations CLI för hello management resursdistributionsmodell (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="10545-107">[Azure CLI 2.0](virtual-network-manage-nsg-arm-cli.md) - our next generation CLI for hello resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="10545-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="10545-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="10545-109">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="10545-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="10545-110">Hämta Information</span><span class="sxs-lookup"><span data-stu-id="10545-110">Retrieve Information</span></span>
<span data-ttu-id="10545-111">Du kan visa dina befintliga NSG: er, hämta regler för en befintlig NSG och ta reda på vilka resurser en NSG är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="10545-111">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="10545-112">Visa befintliga NSG: er</span><span class="sxs-lookup"><span data-stu-id="10545-112">View existing NSGs</span></span>
<span data-ttu-id="10545-113">tooview hello listan över NSG: er i en viss resursgrupp, kör hello `azure network nsg list` som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="10545-113">tooview hello list of NSGs in a specific resource group, run hello `azure network nsg list` command as shown below.</span></span>

```azurecli
azure network nsg list --resource-group RG-NSG
```

<span data-ttu-id="10545-114">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="10545-114">Expected output:</span></span>

    info:    Executing command network nsg list
    + Getting hello network security groups
    data:    Name          Location
    data:    ------------  --------
    data:    NSG-BackEnd   westus
    data:    NSG-FrontEnd  westus
    info:    network nsg list command OK

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="10545-115">Visa en lista med alla regler för en NSG</span><span class="sxs-lookup"><span data-stu-id="10545-115">List all rules for an NSG</span></span>
<span data-ttu-id="10545-116">tooview hello regler för en NSG som heter **NSG-klientdel**kör hello `azure network nsg show` som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="10545-116">tooview hello rules of an NSG named **NSG-FrontEnd**, run hello `azure network nsg show` command as shown below.</span></span> 

```azurecli
azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd
```

<span data-ttu-id="10545-117">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="10545-117">Expected output:</span></span>

    info:    Executing command network nsg show
    + Looking up hello network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Name                            : NSG-FrontEnd
    data:    Type                            : Microsoft.Network/networkSecurityGroups
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    Tags                            : displayName=NSG - Front End
    data:    Security group rules:
    data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
    data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
    data:    rdp-rule                       Internet           *            *               3389              Tcp       Inbound    Allow   100
    data:    web-rule                       Internet           *            *               80                Tcp       Inbound    Allow   101
    data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000
    data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001
    data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500
    data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000
    data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001
    data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500
    info:    network nsg show command OK

> [!NOTE]
> <span data-ttu-id="10545-118">Du kan också använda `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` toolist hello regler från hello **NSG-klientdel** NSG.</span><span class="sxs-lookup"><span data-stu-id="10545-118">You can also use `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` toolist hello rules from hello **NSG-FrontEnd** NSG.</span></span>
>

### <a name="view-nsg-associations"></a><span data-ttu-id="10545-119">Visa NSG associationer</span><span class="sxs-lookup"><span data-stu-id="10545-119">View NSG associations</span></span>

<span data-ttu-id="10545-120">tooview vilka resurser hello **NSG-klientdel** NSG är associerat med, kör hello `azure network nsg show` som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="10545-120">tooview what resources hello **NSG-FrontEnd** NSG is associate with, run hello `azure network nsg show` command as shown below.</span></span> <span data-ttu-id="10545-121">Observera att hello endast skillnaden är hello användning av hello **--json** parameter.</span><span class="sxs-lookup"><span data-stu-id="10545-121">Notice that hello only difference is hello use of hello **--json** parameter.</span></span>

```azurecli
azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json
```

<span data-ttu-id="10545-122">Leta efter hello **networkInterfaces** och **undernät** egenskaper som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="10545-122">Look for hello **networkInterfaces** and **subnets** properties as shown below:</span></span>

    "networkInterfaces": [],
    ...
    "subnets": [
        {
            "id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
        }
    ],
    ...

<span data-ttu-id="10545-123">I hello-exemplet ovan, hello NSG inte är associerad tooany nätverksgränssnitt (NIC), och det är associerade tooa undernät med namnet **klientdel**.</span><span class="sxs-lookup"><span data-stu-id="10545-123">In hello example above, hello NSG is not associated tooany network interfaces (NICs), and it is associated tooa subnet named **FrontEnd**.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="10545-124">Hantera regler</span><span class="sxs-lookup"><span data-stu-id="10545-124">Manage rules</span></span>
<span data-ttu-id="10545-125">Du kan lägga till regler tooan befintliga NSG, redigera befintliga regler och ta bort regler.</span><span class="sxs-lookup"><span data-stu-id="10545-125">You can add rules tooan existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="10545-126">Lägg till en regel</span><span class="sxs-lookup"><span data-stu-id="10545-126">Add a rule</span></span>
<span data-ttu-id="10545-127">tooadd en regel som tillåter **inkommande** trafik tooport **443** från alla datorn toohello **NSG-klientdel** NSG, ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="10545-127">tooadd a rule allowing **inbound** traffic tooport **443** from any machine toohello **NSG-FrontEnd** NSG, enter hello following command:</span></span>

```azurecli
azure network nsg rule create --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --description "Allow access tooport 443 for HTTPS" \
    --protocol Tcp \
    --source-address-prefix * \
    --source-port-range * \
    --destination-address-prefix * \
    --destination-port-range 443 \
    --access Allow \
    --priority 102 \
    --direction Inbound
```

<span data-ttu-id="10545-128">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="10545-128">Expected output:</span></span>

    info:    Executing command network nsg rule create
    + Looking up hello network security rule "allow-https"
    + Creating a network security rule "allow-https"
    + Looking up hello network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access tooport 443 for HTTPS
    data:    Source IP                       : *
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule create command OK

### <a name="change-a-rule"></a><span data-ttu-id="10545-129">Ändra en regel</span><span class="sxs-lookup"><span data-stu-id="10545-129">Change a rule</span></span>
<span data-ttu-id="10545-130">toochange hello regeln som skapade ovan tooallow inkommande trafik från hello **Internet** endast, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="10545-130">toochange hello rule created above tooallow inbound traffic from hello **Internet** only, run hello following command:</span></span>

```azurecli
azure network nsg rule set --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --source-address-prefix Internet
```

<span data-ttu-id="10545-131">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="10545-131">Expected output:</span></span>

    info:    Executing command network nsg rule set
    + Looking up hello network security group "NSG-FrontEnd"
    + Setting a network security rule "allow-https"
    + Looking up hello network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access tooport 443 for HTTPS
    data:    Source IP                       : Internet
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule set command OK

### <a name="delete-a-rule"></a><span data-ttu-id="10545-132">Ta bort en regel</span><span class="sxs-lookup"><span data-stu-id="10545-132">Delete a rule</span></span>
<span data-ttu-id="10545-133">toodelete hello regeln som skapade ovan, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="10545-133">toodelete hello rule created above, run hello following command:</span></span>

```azurecli
azure network nsg rule delete --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --quiet
```

> [!NOTE]
> <span data-ttu-id="10545-134">Hej `--quiet` parametern ser du inte behöver tooconfirm hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="10545-134">hello `--quiet` parameter ensures you don't need tooconfirm hello deletion.</span></span>
>

<span data-ttu-id="10545-135">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="10545-135">Expected output:</span></span>

    info:    Executing command network nsg rule delete
    + Looking up hello network security group "NSG-FrontEnd"
    + Deleting network security rule "allow-https"
    info:    network nsg rule delete command OK

## <a name="manage-associations"></a><span data-ttu-id="10545-136">Hantera kopplingar</span><span class="sxs-lookup"><span data-stu-id="10545-136">Manage associations</span></span>
<span data-ttu-id="10545-137">Du kan koppla en NSG toosubnets och nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="10545-137">You can associate an NSG toosubnets and NICs.</span></span> <span data-ttu-id="10545-138">Du kan också koppla bort en NSG från alla resurser som den är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="10545-138">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-tooa-nic"></a><span data-ttu-id="10545-139">Koppla en NSG tooa NIC</span><span class="sxs-lookup"><span data-stu-id="10545-139">Associate an NSG tooa NIC</span></span>
<span data-ttu-id="10545-140">tooassociate hello **NSG-klientdel** NSG toohello **TestNICWeb1** NIC, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="10545-140">tooassociate hello **NSG-FrontEnd** NSG toohello **TestNICWeb1** NIC, run hello following command:</span></span>

```azurecli
azure network nic set --resource-group RG-NSG \
    --name TestNICWeb1 \
    --network-security-group-name NSG-FrontEnd
```

<span data-ttu-id="10545-141">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="10545-141">Expected output:</span></span>

    info:    Executing command network nic set
    + Looking up hello network interface "TestNICWeb1"
    + Looking up hello network security group "NSG-FrontEnd"
    + Updating network interface "TestNICWeb1"
    + Looking up hello network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Virtual machine                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="10545-142">Koppla bort en NSG från ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="10545-142">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="10545-143">toodissociate hello **NSG-klientdel** NSG från hello **TestNICWeb1** NIC, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="10545-143">toodissociate hello **NSG-FrontEnd** NSG from hello **TestNICWeb1** NIC, run hello following command:</span></span>

```azurecli
azure network nic set --resource-group RG-NSG --name TestNICWeb1 --network-security-group-id ""
```

> [!NOTE]
> <span data-ttu-id="10545-144">Meddelande hello ”” (tom) värdet för hello `network-security-group-id` parameter.</span><span class="sxs-lookup"><span data-stu-id="10545-144">Notice hello "" (empty) value for hello `network-security-group-id` parameter.</span></span> <span data-ttu-id="10545-145">Det är hur du tar bort en association tooan NSG.</span><span class="sxs-lookup"><span data-stu-id="10545-145">That is how you remove an association tooan NSG.</span></span> <span data-ttu-id="10545-146">Du kan inte göra hello samma med hello `network-security-group-name` parameter.</span><span class="sxs-lookup"><span data-stu-id="10545-146">You can't do hello same with hello `network-security-group-name` parameter.</span></span>
> 

<span data-ttu-id="10545-147">Förväntat resultat:</span><span class="sxs-lookup"><span data-stu-id="10545-147">Expected result:</span></span>

    info:    Executing command network nic set
    + Looking up hello network interface "TestNICWeb1"
    + Updating network interface "TestNICWeb1"
    + Looking up hello network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="10545-148">Koppla bort en NSG från ett undernät</span><span class="sxs-lookup"><span data-stu-id="10545-148">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="10545-149">toodissociate hello **NSG-klientdel** NSG från hello **klientdel** undernät, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="10545-149">toodissociate hello **NSG-FrontEnd** NSG from hello **FrontEnd** subnet, run hello following command:</span></span>

```azurecli
azure network vnet subnet set --resource-group RG-NSG \
    --vnet-name TestVNet \
    --name FrontEnd \
    --network-security-group-id ""
```

<span data-ttu-id="10545-150">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="10545-150">Expected output:</span></span>

    info:    Executing command network vnet subnet set
    + Looking up hello subnet "FrontEnd"
    + Setting subnet "FrontEnd"
    + Looking up hello subnet "FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:    Type                            : Microsoft.Network/virtualNetworks/subnets
    data:    ProvisioningState               : Succeeded
    data:    Name                            : FrontEnd
    data:    Address prefix                  : 192.168.1.0/24
    data:    IP configurations:
    data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
    data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
    data:
    info:    network vnet subnet set command OK

### <a name="associate-an-nsg-tooa-subnet"></a><span data-ttu-id="10545-151">Koppla en NSG tooa undernät</span><span class="sxs-lookup"><span data-stu-id="10545-151">Associate an NSG tooa subnet</span></span>
<span data-ttu-id="10545-152">tooassociate hello **NSG-klientdel** NSG toohello **FronEnd** undernät igen och kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="10545-152">tooassociate hello **NSG-FrontEnd** NSG toohello **FronEnd** subnet again, run hello following command:</span></span>

```azurecli
azure network vnet subnet set --resource-group RG-NSG \
    --vnet-name TestVNet \
    --name FrontEnd \
    --network-security-group-name NSG-FronEnd
```

> [!NOTE]
> <span data-ttu-id="10545-153">hello kommandot ovan fungerar bara eftersom hello **NSG-klientdel** NSG tillhör hello samma resursgrupp som hello virtuellt nätverk **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="10545-153">hello command above only works because hello **NSG-FrontEnd** NSG is in hello same resource group as hello virtual network **TestVNet**.</span></span> <span data-ttu-id="10545-154">Om hello NSG i en annan resursgrupp måste toouse hello `--network-security-group-id` parameter i stället och ger fullständig hello-id för hello NSG.</span><span class="sxs-lookup"><span data-stu-id="10545-154">If hello NSG is in a different resource group, you need toouse hello `--network-security-group-id` parameter instead, and provide hello full id for hello NSG.</span></span> <span data-ttu-id="10545-155">Du kan hämta hello-id genom att köra `azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json` och letar efter hello **id** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="10545-155">You can retrieve hello id by running `azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json` and looking for hello **id** property.</span></span> 
> 

<span data-ttu-id="10545-156">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="10545-156">Expected output:</span></span>

        info:    Executing command network vnet subnet set
        + Looking up hello subnet "FrontEnd"
        + Looking up hello network security group "NSG-FrontEnd"
        + Setting subnet "FrontEnd"
        + Looking up hello subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
        data:
        info:    network vnet subnet set command OK

## <a name="delete-an-nsg"></a><span data-ttu-id="10545-157">Ta bort en NSG</span><span class="sxs-lookup"><span data-stu-id="10545-157">Delete an NSG</span></span>
<span data-ttu-id="10545-158">Du kan bara ta bort en NSG om den inte är kopplad till tooany resurs.</span><span class="sxs-lookup"><span data-stu-id="10545-158">You can only delete an NSG if it's not associated tooany resource.</span></span> <span data-ttu-id="10545-159">toodelete en NSG åtgärderna hello nedan.</span><span class="sxs-lookup"><span data-stu-id="10545-159">toodelete an NSG, follow hello steps below.</span></span>

1. <span data-ttu-id="10545-160">associerad toocheck hello resurser tooan NSG, kör hello `azure network nsg show` enligt [visa NSG: er kopplingarna](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="10545-160">toocheck hello resources associated tooan NSG, run hello `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="10545-161">Om hello NSG är associerad tooany nätverkskort, kör hello `azure network nic set` enligt [koppla bort en NSG från ett nätverkskort](#Dissociate-an-NSG-from-a-NIC) för varje nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="10545-161">If hello NSG is associated tooany NICs, run hello `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="10545-162">Om hello NSG är associerad tooany undernät, kör hello `azure network vnet subnet set` enligt [koppla bort en NSG från ett undernät](#Dissociate-an-NSG-from-a-subnet) för varje undernät.</span><span class="sxs-lookup"><span data-stu-id="10545-162">If hello NSG is associated tooany subnet, run hello `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="10545-163">toodelete hello NSG, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="10545-163">toodelete hello NSG, run hello following command:</span></span>

    ```azurecli
    azure network nsg delete --resource-group RG-NSG --name NSG-FrontEnd --quiet
    ```

    <span data-ttu-id="10545-164">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="10545-164">Expected output:</span></span>

        info:    Executing command network nsg delete
        + Looking up hello network security group "NSG-FrontEnd"
        + Deleting network security group "NSG-FrontEnd"
        info:    network nsg delete command OK

## <a name="next-steps"></a><span data-ttu-id="10545-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="10545-165">Next steps</span></span>
* <span data-ttu-id="10545-166">[Aktivera loggning](virtual-network-nsg-manage-log.md) för NSG: er.</span><span class="sxs-lookup"><span data-stu-id="10545-166">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

