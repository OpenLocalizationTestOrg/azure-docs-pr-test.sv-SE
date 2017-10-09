---
title: "aaaCreate nätverkssäkerhetsgrupper - Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur toocreate och distribuera nätverkssäkerhetsgrupper med hello Azure CLI 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eeb7feedab959d92659e03c5c46d93fdfc08faea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-hello-azure-cli-10"></a><span data-ttu-id="416d3-103">Skapa nätverk med hjälp av hello Azure CLI 1.0-säkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="416d3-103">Create network security groups using hello Azure CLI 1.0</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="416d3-104">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="416d3-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="416d3-105">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="416d3-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="416d3-106">[Azure CLI 1.0](#how-to-create-the-nsg-for-the-front-end-subnet) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="416d3-106">[Azure CLI 1.0](#how-to-create-the-nsg-for-the-front-end-subnet) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="416d3-107">[Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="416d3-107">[Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md) - our next-generation CLI for hello resource management deployment model</span></span> 

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="416d3-108">Den här artikeln beskriver hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="416d3-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="416d3-109">Du kan också [skapa NSG: er i hello klassiska distributionsmodellen](virtual-networks-create-nsg-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="416d3-109">You can also [create NSGs in hello classic deployment model](virtual-networks-create-nsg-classic-cli.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="416d3-110">hello exempel Azure CLI-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="416d3-110">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> 

## <a name="how-toocreate-hello-nsg-for-hello-front-end-subnet"></a><span data-ttu-id="416d3-111">Hur toocreate hello NSG för hello klientdelens undernät</span><span class="sxs-lookup"><span data-stu-id="416d3-111">How toocreate hello NSG for hello front end subnet</span></span>
<span data-ttu-id="416d3-112">toocreate en NSG med namnet med namnet *NSG-klientdel* utifrån hello scenariot ovan gör hello nedan.</span><span class="sxs-lookup"><span data-stu-id="416d3-112">toocreate an NSG named named *NSG-FrontEnd* based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="416d3-113">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="416d3-113">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="416d3-114">Kör hello **azure config mode** kommandot tooswitch tooResource Manager-läge enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="416d3-114">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>
   
        azure config mode arm
   
    <span data-ttu-id="416d3-115">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="416d3-115">Expected output:</span></span>
   
        info:    New mode is arm
3. <span data-ttu-id="416d3-116">Kör hello **azure-nätverk nsg skapa** kommandot toocreate en NSG.</span><span class="sxs-lookup"><span data-stu-id="416d3-116">Run hello **azure network nsg create** command toocreate an NSG.</span></span>
   
        azure network nsg create -g TestRG -l westus -n NSG-FrontEnd
   
    <span data-ttu-id="416d3-117">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="416d3-117">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up hello network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
        data:    Name                            : NSG-FrontEnd
        data:    Type                            : Microsoft.Network/networkSecurityGroups
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Security group rules:
        data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
        data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
        data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000   
        data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001   
        data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500   
        data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000   
        data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001   
        data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500   
        info:    network nsg create command OK
   
    <span data-ttu-id="416d3-118">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="416d3-118">Parameters:</span></span>
   
   * <span data-ttu-id="416d3-119">**-g (eller --resource-group)**.</span><span class="sxs-lookup"><span data-stu-id="416d3-119">**-g (or --resource-group)**.</span></span> <span data-ttu-id="416d3-120">Namnet på hello resursgruppen där hello NSG kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="416d3-120">Name of hello resource group where hello NSG will be created.</span></span> <span data-ttu-id="416d3-121">I vårt exempel, *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="416d3-121">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="416d3-122">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="416d3-122">**-l (or --location)**.</span></span> <span data-ttu-id="416d3-123">Azure-region där hello ny NSG kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="416d3-123">Azure region where hello new NSG will be created.</span></span> <span data-ttu-id="416d3-124">I vårt scenario, *westus*.</span><span class="sxs-lookup"><span data-stu-id="416d3-124">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="416d3-125">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="416d3-125">**-n (or --name)**.</span></span> <span data-ttu-id="416d3-126">Namn för hello ny NSG.</span><span class="sxs-lookup"><span data-stu-id="416d3-126">Name for hello new NSG.</span></span> <span data-ttu-id="416d3-127">I vårt scenario, *NSG-klientdel*.</span><span class="sxs-lookup"><span data-stu-id="416d3-127">For our scenario, *NSG-FrontEnd*.</span></span>
4. <span data-ttu-id="416d3-128">Kör hello **azure-nätverk nsg regeln skapa** kommandot toocreate en regel som tillåter åtkomst tooport 3389 (RDP) från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="416d3-128">Run hello **azure network nsg rule create** command toocreate a rule that allows access tooport 3389 (RDP) from hello Internet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    <span data-ttu-id="416d3-129">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="416d3-129">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        warn:    Using default direction: Inbound
        info:    Looking up hello network security rule "rdp-rule"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp
        -rule
        data:    Name                            : rdp-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : Internet
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 3389
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
   
    <span data-ttu-id="416d3-130">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="416d3-130">Parameters:</span></span>
   
   * <span data-ttu-id="416d3-131">**-a (eller--nsg-namn)**.</span><span class="sxs-lookup"><span data-stu-id="416d3-131">**-a (or --nsg-name)**.</span></span> <span data-ttu-id="416d3-132">Namnet på hello NSG vilka hello regel kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="416d3-132">Name of hello NSG in which hello rule will be created.</span></span> <span data-ttu-id="416d3-133">I vårt scenario, *NSG-klientdel*.</span><span class="sxs-lookup"><span data-stu-id="416d3-133">For our scenario, *NSG-FrontEnd*.</span></span>
   * <span data-ttu-id="416d3-134">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="416d3-134">**-n (or --name)**.</span></span> <span data-ttu-id="416d3-135">Namn på hello nya regeln.</span><span class="sxs-lookup"><span data-stu-id="416d3-135">Name for hello new rule.</span></span> <span data-ttu-id="416d3-136">I vårt scenario, *rdp-regel*.</span><span class="sxs-lookup"><span data-stu-id="416d3-136">For our scenario, *rdp-rule*.</span></span>
   * <span data-ttu-id="416d3-137">**-c (eller--åtkomst)**.</span><span class="sxs-lookup"><span data-stu-id="416d3-137">**-c (or --access)**.</span></span> <span data-ttu-id="416d3-138">Åtkomstnivå för hello regel (Tillåt eller neka).</span><span class="sxs-lookup"><span data-stu-id="416d3-138">Access level for hello rule (Deny or Allow).</span></span>
   * <span data-ttu-id="416d3-139">**-p (eller--protocol)**.</span><span class="sxs-lookup"><span data-stu-id="416d3-139">**-p (or --protocol)**.</span></span> <span data-ttu-id="416d3-140">Protocol (Tcp, Udp eller *) för hello regeln.</span><span class="sxs-lookup"><span data-stu-id="416d3-140">Protocol (Tcp, Udp, or *) for hello rule.</span></span>
   * <span data-ttu-id="416d3-141">**-r (eller--riktning)**.</span><span class="sxs-lookup"><span data-stu-id="416d3-141">**-r (or --direction)**.</span></span> <span data-ttu-id="416d3-142">Riktning för anslutning (inkommande eller utgående).</span><span class="sxs-lookup"><span data-stu-id="416d3-142">Direction of connection (Inbound or Outbound).</span></span>
   * <span data-ttu-id="416d3-143">**-y (eller--prioritet)**.</span><span class="sxs-lookup"><span data-stu-id="416d3-143">**-y (or --priority)**.</span></span> <span data-ttu-id="416d3-144">Prioritet för hello regeln.</span><span class="sxs-lookup"><span data-stu-id="416d3-144">Priority for hello rule.</span></span>
   * <span data-ttu-id="416d3-145">**-f (eller--källadress-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="416d3-145">**-f (or --source-address-prefix)**.</span></span> <span data-ttu-id="416d3-146">Källadress-prefix i CIDR- eller använda standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="416d3-146">Source address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="416d3-147">**-o (eller--Källportintervall-)**.</span><span class="sxs-lookup"><span data-stu-id="416d3-147">**-o (or --source-port-range)**.</span></span> <span data-ttu-id="416d3-148">Källport eller portintervall.</span><span class="sxs-lookup"><span data-stu-id="416d3-148">Source port, or port range.</span></span>
   * <span data-ttu-id="416d3-149">**-e (eller--mål adressprefixet)**.</span><span class="sxs-lookup"><span data-stu-id="416d3-149">**-e (or --destination-address-prefix)**.</span></span> <span data-ttu-id="416d3-150">Måladress-prefix i CIDR- eller använda standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="416d3-150">Destination address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="416d3-151">**-u (eller--Målportintervall-)**.</span><span class="sxs-lookup"><span data-stu-id="416d3-151">**-u (or --destination-port-range)**.</span></span> <span data-ttu-id="416d3-152">Målport eller portintervall.</span><span class="sxs-lookup"><span data-stu-id="416d3-152">Destination port, or port range.</span></span>    
5. <span data-ttu-id="416d3-153">Kör hello **azure-nätverk nsg regeln skapa** kommandot toocreate en regel som tillåter åtkomst tooport 80 (HTTP) från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="416d3-153">Run hello **azure network nsg rule create** command toocreate a rule that allows access tooport 80 (HTTP) from hello Internet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    <span data-ttu-id="416d3-154">Förväntade putput:</span><span class="sxs-lookup"><span data-stu-id="416d3-154">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
        data:    Name                            : web-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : Internet
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 80
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK
6. <span data-ttu-id="416d3-155">Kör hello **azure network vnet undernät set** kommandot toolink hello NSG toohello klientdelens undernät.</span><span class="sxs-lookup"><span data-stu-id="416d3-155">Run hello **azure network vnet subnet set** command toolink hello NSG toohello front end subnet.</span></span>
   
        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -o NSG-FrontEnd
   
    <span data-ttu-id="416d3-156">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="416d3-156">Expected output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up hello subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ip
        Configurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ip
        Configurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

## <a name="how-toocreate-hello-nsg-for-hello-back-end-subnet"></a><span data-ttu-id="416d3-157">Hur toocreate hello NSG för hello tillbaka avslutas undernät</span><span class="sxs-lookup"><span data-stu-id="416d3-157">How toocreate hello NSG for hello back end subnet</span></span>
<span data-ttu-id="416d3-158">toocreate en NSG med namnet med namnet *NSG BackEnd* utifrån hello scenariot ovan gör hello nedan.</span><span class="sxs-lookup"><span data-stu-id="416d3-158">toocreate an NSG named named *NSG-BackEnd* based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="416d3-159">Kör hello **azure-nätverk nsg skapa** kommandot toocreate en NSG.</span><span class="sxs-lookup"><span data-stu-id="416d3-159">Run hello **azure network nsg create** command toocreate an NSG.</span></span>
   
        azure network nsg create -g TestRG -l westus -n NSG-BackEnd
   
    <span data-ttu-id="416d3-160">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="416d3-160">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up hello network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd
        data:    Name                            : NSG-BackEnd
        data:    Type                            : Microsoft.Network/networkSecurityGroups
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Security group rules:
        data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
        data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
        data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000   
        data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001   
        data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500   
        data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000   
        data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001   
        data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500   
        info:    network nsg create command OK
2. <span data-ttu-id="416d3-161">Kör hello **azure-nätverk nsg regeln skapa** kommandot toocreate en regel som tillåter åtkomst tooport 1433 (SQL) från hello klientdelens undernät.</span><span class="sxs-lookup"><span data-stu-id="416d3-161">Run hello **azure network nsg rule create** command toocreate a rule that allows access tooport 1433 (SQL) from hello front end subnet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    <span data-ttu-id="416d3-162">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="416d3-162">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security rule "sql-rule"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd/securityRules/sql-rule
        data:    Name                            : sql-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 1433
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
3. <span data-ttu-id="416d3-163">Kör hello **azure-nätverk nsg regeln skapa** kommandot toocreate en regel som nekar åtkomst toohello Internet från.</span><span class="sxs-lookup"><span data-stu-id="416d3-163">Run hello **azure network nsg rule create** command toocreate a rule that denies access toohello Internet from.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-BackEnd -n web-rule -c Deny -p * -r Outbound -y 200 -f * -o * -e Internet -u *
   
    <span data-ttu-id="416d3-164">Förväntade putput:</span><span class="sxs-lookup"><span data-stu-id="416d3-164">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd/securityRules/web-rule
        data:    Name                            : web-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : *
        data:    Source Port                     : *
        data:    Destination IP                  : Internet
        data:    Destination Port                : *
        data:    Protocol                        : *
        data:    Direction                       : Outbound
        data:    Access                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK
4. <span data-ttu-id="416d3-165">Kör hello **azure network vnet undernät set** kommandot toolink hello NSG toohello tillbaka avslutas undernät.</span><span class="sxs-lookup"><span data-stu-id="416d3-165">Run hello **azure network vnet subnet set** command toolink hello NSG toohello back end subnet.</span></span>
   
        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -o NSG-BackEnd
   
    <span data-ttu-id="416d3-166">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="416d3-166">Expected output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up hello subnet "BackEnd"
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Setting subnet "BackEnd"
        info:    Looking up hello subnet "BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/BackEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : BackEnd
        data:    Address prefix                  : 192.168.2.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICSQL1/ip
        Configurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICSQL2/ip
        Configurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

