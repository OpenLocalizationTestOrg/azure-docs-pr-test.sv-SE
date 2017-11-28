---
title: "Skapa nätverkssäkerhetsgrupper - Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur du skapar och distribuerar nätverkssäkerhetsgrupper som använder Azure CLI 1.0."
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
ms.openlocfilehash: ca8c182651e3c9f2f1f3a85b94361755d8e638d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-network-security-groups-using-the-azure-cli-10"></a><span data-ttu-id="3e53a-103">Skapa nätverk med hjälp av Azure CLI 1.0-säkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="3e53a-103">Create network security groups using the Azure CLI 1.0</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="3e53a-104">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="3e53a-104">CLI versions to complete the task</span></span> 

<span data-ttu-id="3e53a-105">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="3e53a-105">You can complete the task using one of the following CLI versions:</span></span> 

- <span data-ttu-id="3e53a-106">[Azure CLI 1.0](#how-to-create-the-nsg-for-the-front-end-subnet) – våra CLI för klassisk och resurs management på distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="3e53a-106">[Azure CLI 1.0](#how-to-create-the-nsg-for-the-front-end-subnet) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="3e53a-107">[Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md) -vår nästa generations CLI för resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="3e53a-107">[Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md) - our next-generation CLI for the resource management deployment model</span></span> 

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="3e53a-108">Den här artikeln beskriver Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="3e53a-108">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="3e53a-109">Du kan också [skapa NSG: er i den klassiska distributionsmodellen](virtual-networks-create-nsg-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="3e53a-109">You can also [create NSGs in the classic deployment model](virtual-networks-create-nsg-classic-cli.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="3e53a-110">Exemplet Azure CLI-kommandona nedan förväntar sig en enkel miljö som redan har skapats baserat på scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="3e53a-110">The sample Azure CLI commands below expect a simple environment already created based on the scenario above.</span></span> 

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a><span data-ttu-id="3e53a-111">Hur du skapar NSG för undernätet frontend</span><span class="sxs-lookup"><span data-stu-id="3e53a-111">How to create the NSG for the front end subnet</span></span>
<span data-ttu-id="3e53a-112">Om du vill skapa en NSG som heter heter *NSG-klientdel* baserat på scenariot ovan, Följ stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="3e53a-112">To create an NSG named named *NSG-FrontEnd* based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="3e53a-113">Om du aldrig har använt Azure CLI, se [installera och konfigurera Azure CLI](../cli-install-nodejs.md) och följ instruktionerna upp till den punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3e53a-113">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="3e53a-114">Kör kommandot **azure config mode** för att växla till Resource Manager-läge, som det visas nedan.</span><span class="sxs-lookup"><span data-stu-id="3e53a-114">Run the **azure config mode** command to switch to Resource Manager mode, as shown below.</span></span>
   
        azure config mode arm
   
    <span data-ttu-id="3e53a-115">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="3e53a-115">Expected output:</span></span>
   
        info:    New mode is arm
3. <span data-ttu-id="3e53a-116">Kör den **azure-nätverk nsg skapa** kommando för att skapa en NSG.</span><span class="sxs-lookup"><span data-stu-id="3e53a-116">Run the **azure network nsg create** command to create an NSG.</span></span>
   
        azure network nsg create -g TestRG -l westus -n NSG-FrontEnd
   
    <span data-ttu-id="3e53a-117">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="3e53a-117">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
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
   
    <span data-ttu-id="3e53a-118">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="3e53a-118">Parameters:</span></span>
   
   * <span data-ttu-id="3e53a-119">**-g (eller --resource-group)**.</span><span class="sxs-lookup"><span data-stu-id="3e53a-119">**-g (or --resource-group)**.</span></span> <span data-ttu-id="3e53a-120">Namnet på resursgruppen där NSG: N kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="3e53a-120">Name of the resource group where the NSG will be created.</span></span> <span data-ttu-id="3e53a-121">I vårt exempel, *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="3e53a-121">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="3e53a-122">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="3e53a-122">**-l (or --location)**.</span></span> <span data-ttu-id="3e53a-123">Azure-region där den nya NSG kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="3e53a-123">Azure region where the new NSG will be created.</span></span> <span data-ttu-id="3e53a-124">I vårt scenario, *westus*.</span><span class="sxs-lookup"><span data-stu-id="3e53a-124">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="3e53a-125">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="3e53a-125">**-n (or --name)**.</span></span> <span data-ttu-id="3e53a-126">Namn på ny NSG: N.</span><span class="sxs-lookup"><span data-stu-id="3e53a-126">Name for the new NSG.</span></span> <span data-ttu-id="3e53a-127">I vårt scenario, *NSG-klientdel*.</span><span class="sxs-lookup"><span data-stu-id="3e53a-127">For our scenario, *NSG-FrontEnd*.</span></span>
4. <span data-ttu-id="3e53a-128">Kör den **azure-nätverk nsg regeln skapa** kommando för att skapa en regel som tillåter åtkomst till port 3389 (RDP) från Internet.</span><span class="sxs-lookup"><span data-stu-id="3e53a-128">Run the **azure network nsg rule create** command to create a rule that allows access to port 3389 (RDP) from the Internet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    <span data-ttu-id="3e53a-129">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="3e53a-129">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        warn:    Using default direction: Inbound
        info:    Looking up the network security rule "rdp-rule"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
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
   
    <span data-ttu-id="3e53a-130">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="3e53a-130">Parameters:</span></span>
   
   * <span data-ttu-id="3e53a-131">**-a (eller--nsg-namn)**.</span><span class="sxs-lookup"><span data-stu-id="3e53a-131">**-a (or --nsg-name)**.</span></span> <span data-ttu-id="3e53a-132">Namnet på NSG: N som regeln ska skapas.</span><span class="sxs-lookup"><span data-stu-id="3e53a-132">Name of the NSG in which the rule will be created.</span></span> <span data-ttu-id="3e53a-133">I vårt scenario, *NSG-klientdel*.</span><span class="sxs-lookup"><span data-stu-id="3e53a-133">For our scenario, *NSG-FrontEnd*.</span></span>
   * <span data-ttu-id="3e53a-134">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="3e53a-134">**-n (or --name)**.</span></span> <span data-ttu-id="3e53a-135">Namnet på den nya regeln.</span><span class="sxs-lookup"><span data-stu-id="3e53a-135">Name for the new rule.</span></span> <span data-ttu-id="3e53a-136">I vårt scenario, *rdp-regel*.</span><span class="sxs-lookup"><span data-stu-id="3e53a-136">For our scenario, *rdp-rule*.</span></span>
   * <span data-ttu-id="3e53a-137">**-c (eller--åtkomst)**.</span><span class="sxs-lookup"><span data-stu-id="3e53a-137">**-c (or --access)**.</span></span> <span data-ttu-id="3e53a-138">Åtkomstnivå för regeln (Tillåt eller neka).</span><span class="sxs-lookup"><span data-stu-id="3e53a-138">Access level for the rule (Deny or Allow).</span></span>
   * <span data-ttu-id="3e53a-139">**-p (eller--protocol)**.</span><span class="sxs-lookup"><span data-stu-id="3e53a-139">**-p (or --protocol)**.</span></span> <span data-ttu-id="3e53a-140">Protocol (Tcp, Udp eller *) för regeln.</span><span class="sxs-lookup"><span data-stu-id="3e53a-140">Protocol (Tcp, Udp, or *) for the rule.</span></span>
   * <span data-ttu-id="3e53a-141">**-r (eller--riktning)**.</span><span class="sxs-lookup"><span data-stu-id="3e53a-141">**-r (or --direction)**.</span></span> <span data-ttu-id="3e53a-142">Riktning för anslutning (inkommande eller utgående).</span><span class="sxs-lookup"><span data-stu-id="3e53a-142">Direction of connection (Inbound or Outbound).</span></span>
   * <span data-ttu-id="3e53a-143">**-y (eller--prioritet)**.</span><span class="sxs-lookup"><span data-stu-id="3e53a-143">**-y (or --priority)**.</span></span> <span data-ttu-id="3e53a-144">Prioritet för regeln.</span><span class="sxs-lookup"><span data-stu-id="3e53a-144">Priority for the rule.</span></span>
   * <span data-ttu-id="3e53a-145">**-f (eller--källadress-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="3e53a-145">**-f (or --source-address-prefix)**.</span></span> <span data-ttu-id="3e53a-146">Källadress-prefix i CIDR- eller använda standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="3e53a-146">Source address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="3e53a-147">**-o (eller--Källportintervall-)**.</span><span class="sxs-lookup"><span data-stu-id="3e53a-147">**-o (or --source-port-range)**.</span></span> <span data-ttu-id="3e53a-148">Källport eller portintervall.</span><span class="sxs-lookup"><span data-stu-id="3e53a-148">Source port, or port range.</span></span>
   * <span data-ttu-id="3e53a-149">**-e (eller--mål adressprefixet)**.</span><span class="sxs-lookup"><span data-stu-id="3e53a-149">**-e (or --destination-address-prefix)**.</span></span> <span data-ttu-id="3e53a-150">Måladress-prefix i CIDR- eller använda standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="3e53a-150">Destination address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="3e53a-151">**-u (eller--Målportintervall-)**.</span><span class="sxs-lookup"><span data-stu-id="3e53a-151">**-u (or --destination-port-range)**.</span></span> <span data-ttu-id="3e53a-152">Målport eller portintervall.</span><span class="sxs-lookup"><span data-stu-id="3e53a-152">Destination port, or port range.</span></span>    
5. <span data-ttu-id="3e53a-153">Kör den **azure-nätverk nsg regeln skapa** kommando för att skapa en regel som tillåter åtkomst till port 80 (HTTP) från Internet.</span><span class="sxs-lookup"><span data-stu-id="3e53a-153">Run the **azure network nsg rule create** command to create a rule that allows access to port 80 (HTTP) from the Internet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    <span data-ttu-id="3e53a-154">Förväntade putput:</span><span class="sxs-lookup"><span data-stu-id="3e53a-154">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
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
6. <span data-ttu-id="3e53a-155">Kör den **azure network vnet undernät set** kommando för att länka NSG: N till klientdelens undernät.</span><span class="sxs-lookup"><span data-stu-id="3e53a-155">Run the **azure network vnet subnet set** command to link the NSG to the front end subnet.</span></span>
   
        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -o NSG-FrontEnd
   
    <span data-ttu-id="3e53a-156">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="3e53a-156">Expected output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
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

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a><span data-ttu-id="3e53a-157">Hur du skapar NSG för backend-undernät</span><span class="sxs-lookup"><span data-stu-id="3e53a-157">How to create the NSG for the back end subnet</span></span>
<span data-ttu-id="3e53a-158">Om du vill skapa en NSG som heter heter *NSG BackEnd* baserat på scenariot ovan, Följ stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="3e53a-158">To create an NSG named named *NSG-BackEnd* based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="3e53a-159">Kör den **azure-nätverk nsg skapa** kommando för att skapa en NSG.</span><span class="sxs-lookup"><span data-stu-id="3e53a-159">Run the **azure network nsg create** command to create an NSG.</span></span>
   
        azure network nsg create -g TestRG -l westus -n NSG-BackEnd
   
    <span data-ttu-id="3e53a-160">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="3e53a-160">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
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
2. <span data-ttu-id="3e53a-161">Kör den **azure-nätverk nsg regeln skapa** kommando för att skapa en regel som tillåter åtkomst till port 1433 (SQL) från klientdelens undernät.</span><span class="sxs-lookup"><span data-stu-id="3e53a-161">Run the **azure network nsg rule create** command to create a rule that allows access to port 1433 (SQL) from the front end subnet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    <span data-ttu-id="3e53a-162">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="3e53a-162">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "sql-rule"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
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
3. <span data-ttu-id="3e53a-163">Kör den **azure-nätverk nsg regeln skapa** kommando för att skapa en regel som nekar åtkomst till Internet från.</span><span class="sxs-lookup"><span data-stu-id="3e53a-163">Run the **azure network nsg rule create** command to create a rule that denies access to the Internet from.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-BackEnd -n web-rule -c Deny -p * -r Outbound -y 200 -f * -o * -e Internet -u *
   
    <span data-ttu-id="3e53a-164">Förväntade putput:</span><span class="sxs-lookup"><span data-stu-id="3e53a-164">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
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
4. <span data-ttu-id="3e53a-165">Kör den **azure network vnet undernät set** kommando för att länka NSG: N till backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="3e53a-165">Run the **azure network vnet subnet set** command to link the NSG to the back end subnet.</span></span>
   
        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -o NSG-BackEnd
   
    <span data-ttu-id="3e53a-166">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="3e53a-166">Expected output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Setting subnet "BackEnd"
        info:    Looking up the subnet "BackEnd"
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

