---
title: "Hur du skapar NSG: er i klassiskt läge med hjälp av Azure CLI | Microsoft Docs"
description: "Lär dig hur du skapar och distribuerar NSG: er i klassiskt läge med hjälp av Azure CLI"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 17d98950-5fbb-4653-bef6-d822ab37541e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 115a1937a4c88ba2b986a40c84b1b759ed5e03b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a><span data-ttu-id="a65cc-103">Hur du skapar NSG: er (klassisk) i Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a65cc-103">How to create NSGs (classic) in the Azure CLI</span></span>
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="a65cc-104">Den här artikeln beskriver hur du gör om du använder den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="a65cc-104">This article covers the classic deployment model.</span></span> <span data-ttu-id="a65cc-105">Du kan också [skapa NSG: er i Resource Manager-distributionsmodellen](virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="a65cc-105">You can also [create NSGs in the Resource Manager deployment model](virtual-networks-create-nsg-arm-cli.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="a65cc-106">Exemplet Azure CLI-kommandona nedan förväntar sig en enkel miljö som redan har skapats baserat på scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="a65cc-106">The sample Azure CLI commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="a65cc-107">Om du vill köra kommandon som de visas i det här dokumentet först skapa testmiljö av [skapa ett virtuellt nätverk](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="a65cc-107">If you want to run the commands as they are displayed in this document, first build the test environment by [creating a VNet](virtual-networks-create-vnet-classic-cli.md).</span></span>

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a><span data-ttu-id="a65cc-108">Hur du skapar NSG för undernätet frontend</span><span class="sxs-lookup"><span data-stu-id="a65cc-108">How to create the NSG for the front end subnet</span></span>
<span data-ttu-id="a65cc-109">Om du vill skapa en NSG som heter heter **NSG-klientdel** baserat på scenariot ovan, Följ stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="a65cc-109">To create an NSG named named **NSG-FrontEnd** based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="a65cc-110">Om du aldrig har använt Azure CLI, se [installera och konfigurera Azure CLI](../cli-install-nodejs.md) och följ instruktionerna upp till den punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a65cc-110">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="a65cc-111">Kör den  **`azure config mode`**  kommando för att växla till klassiskt läge enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="a65cc-111">Run the **`azure config mode`** command to switch to classic mode, as shown below.</span></span>
   
        azure config mode asm
   
    <span data-ttu-id="a65cc-112">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="a65cc-112">Expected output:</span></span>
   
        info:    New mode is asm
3. <span data-ttu-id="a65cc-113">Kör den  **`azure network nsg create`**  kommando för att skapa en NSG.</span><span class="sxs-lookup"><span data-stu-id="a65cc-113">Run the **`azure network nsg create`** command to create an NSG.</span></span>
   
        azure network nsg create -l uswest -n NSG-FrontEnd
   
    <span data-ttu-id="a65cc-114">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="a65cc-114">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    <span data-ttu-id="a65cc-115">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="a65cc-115">Parameters:</span></span>
   
   * <span data-ttu-id="a65cc-116">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-116">**-l (or --location)**.</span></span> <span data-ttu-id="a65cc-117">Azure-region där den nya NSG kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="a65cc-117">Azure region where the new NSG will be created.</span></span> <span data-ttu-id="a65cc-118">I vårt scenario, *westus*.</span><span class="sxs-lookup"><span data-stu-id="a65cc-118">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="a65cc-119">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-119">**-n (or --name)**.</span></span> <span data-ttu-id="a65cc-120">Namn på ny NSG: N.</span><span class="sxs-lookup"><span data-stu-id="a65cc-120">Name for the new NSG.</span></span> <span data-ttu-id="a65cc-121">I vårt scenario, *NSG-klientdel*.</span><span class="sxs-lookup"><span data-stu-id="a65cc-121">For our scenario, *NSG-FrontEnd*.</span></span>
4. <span data-ttu-id="a65cc-122">Kör den  **`azure network nsg rule create`**  kommando för att skapa en regel som tillåter åtkomst till port 3389 (RDP) från Internet.</span><span class="sxs-lookup"><span data-stu-id="a65cc-122">Run the **`azure network nsg rule create`** command to create a rule that allows access to port 3389 (RDP) from the Internet.</span></span>
   
        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    <span data-ttu-id="a65cc-123">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="a65cc-123">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
   
    <span data-ttu-id="a65cc-124">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="a65cc-124">Parameters:</span></span>
   
   * <span data-ttu-id="a65cc-125">**-a (eller--nsg-namn)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-125">**-a (or --nsg-name)**.</span></span> <span data-ttu-id="a65cc-126">Namnet på NSG: N som regeln ska skapas.</span><span class="sxs-lookup"><span data-stu-id="a65cc-126">Name of the NSG in which the rule will be created.</span></span> <span data-ttu-id="a65cc-127">I vårt scenario, *NSG-klientdel*.</span><span class="sxs-lookup"><span data-stu-id="a65cc-127">For our scenario, *NSG-FrontEnd*.</span></span>
   * <span data-ttu-id="a65cc-128">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-128">**-n (or --name)**.</span></span> <span data-ttu-id="a65cc-129">Namnet på den nya regeln.</span><span class="sxs-lookup"><span data-stu-id="a65cc-129">Name for the new rule.</span></span> <span data-ttu-id="a65cc-130">I vårt scenario, *rdp-regel*.</span><span class="sxs-lookup"><span data-stu-id="a65cc-130">For our scenario, *rdp-rule*.</span></span>
   * <span data-ttu-id="a65cc-131">**-c (eller--åtgärd)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-131">**-c (or --action)**.</span></span> <span data-ttu-id="a65cc-132">Åtkomstnivå för regeln (Tillåt eller neka).</span><span class="sxs-lookup"><span data-stu-id="a65cc-132">Access level for the rule (Deny or Allow).</span></span>
   * <span data-ttu-id="a65cc-133">**-p (eller--protocol)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-133">**-p (or --protocol)**.</span></span> <span data-ttu-id="a65cc-134">Protocol (Tcp, Udp eller *) för regeln.</span><span class="sxs-lookup"><span data-stu-id="a65cc-134">Protocol (Tcp, Udp, or *) for the rule.</span></span>
   * <span data-ttu-id="a65cc-135">**-r (eller--typ)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-135">**-r (or --type)**.</span></span> <span data-ttu-id="a65cc-136">Riktning för anslutning (inkommande eller utgående).</span><span class="sxs-lookup"><span data-stu-id="a65cc-136">Direction of connection (Inbound or Outbound).</span></span>
   * <span data-ttu-id="a65cc-137">**-y (eller--prioritet)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-137">**-y (or --priority)**.</span></span> <span data-ttu-id="a65cc-138">Prioritet för regeln.</span><span class="sxs-lookup"><span data-stu-id="a65cc-138">Priority for the rule.</span></span>
   * <span data-ttu-id="a65cc-139">**-f (eller--källadress-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-139">**-f (or --source-address-prefix)**.</span></span> <span data-ttu-id="a65cc-140">Källadress-prefix i CIDR- eller använda standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="a65cc-140">Source address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="a65cc-141">**-o (eller--Källportintervall-)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-141">**-o (or --source-port-range)**.</span></span> <span data-ttu-id="a65cc-142">Källport eller portintervall.</span><span class="sxs-lookup"><span data-stu-id="a65cc-142">Source port, or port range.</span></span>
   * <span data-ttu-id="a65cc-143">**-e (eller--mål adressprefixet)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-143">**-e (or --destination-address-prefix)**.</span></span> <span data-ttu-id="a65cc-144">Måladress-prefix i CIDR- eller använda standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="a65cc-144">Destination address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="a65cc-145">**-u (eller--Målportintervall-)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-145">**-u (or --destination-port-range)**.</span></span> <span data-ttu-id="a65cc-146">Målport eller portintervall.</span><span class="sxs-lookup"><span data-stu-id="a65cc-146">Destination port, or port range.</span></span>
5. <span data-ttu-id="a65cc-147">Kör den  **`azure network nsg rule create`**  kommando för att skapa en regel som tillåter åtkomst till port 80 (HTTP) från Internet.</span><span class="sxs-lookup"><span data-stu-id="a65cc-147">Run the **`azure network nsg rule create`** command to create a rule that allows access to port 80 (HTTP) from the Internet.</span></span>
   
        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    <span data-ttu-id="a65cc-148">Förväntade putput:</span><span class="sxs-lookup"><span data-stu-id="a65cc-148">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK
6. <span data-ttu-id="a65cc-149">Kör den  **`azure network nsg subnet add`**  kommando för att länka NSG: N till klientdelens undernät.</span><span class="sxs-lookup"><span data-stu-id="a65cc-149">Run the **`azure network nsg subnet add`** command to link the NSG to the front end subnet.</span></span>
   
        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   
    <span data-ttu-id="a65cc-150">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="a65cc-150">Expected output:</span></span>
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a><span data-ttu-id="a65cc-151">Hur du skapar NSG för backend-undernät</span><span class="sxs-lookup"><span data-stu-id="a65cc-151">How to create the NSG for the back end subnet</span></span>
<span data-ttu-id="a65cc-152">Om du vill skapa en NSG som heter heter *NSG BackEnd* baserat på scenariot ovan, Följ stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="a65cc-152">To create an NSG named named *NSG-BackEnd* based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="a65cc-153">Kör den  **`azure network nsg create`**  kommando för att skapa en NSG.</span><span class="sxs-lookup"><span data-stu-id="a65cc-153">Run the **`azure network nsg create`** command to create an NSG.</span></span>
   
        azure network nsg create -l uswest -n NSG-BackEnd
   
    <span data-ttu-id="a65cc-154">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="a65cc-154">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    <span data-ttu-id="a65cc-155">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="a65cc-155">Parameters:</span></span>
   
   * <span data-ttu-id="a65cc-156">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-156">**-l (or --location)**.</span></span> <span data-ttu-id="a65cc-157">Azure-region där den nya NSG kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="a65cc-157">Azure region where the new NSG will be created.</span></span> <span data-ttu-id="a65cc-158">I vårt scenario, *westus*.</span><span class="sxs-lookup"><span data-stu-id="a65cc-158">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="a65cc-159">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="a65cc-159">**-n (or --name)**.</span></span> <span data-ttu-id="a65cc-160">Namn på ny NSG: N.</span><span class="sxs-lookup"><span data-stu-id="a65cc-160">Name for the new NSG.</span></span> <span data-ttu-id="a65cc-161">I vårt scenario, *NSG-klientdel*.</span><span class="sxs-lookup"><span data-stu-id="a65cc-161">For our scenario, *NSG-FrontEnd*.</span></span>
2. <span data-ttu-id="a65cc-162">Kör den  **`azure network nsg rule create`**  kommando för att skapa en regel som tillåter åtkomst till port 1433 (SQL) från klientdelens undernät.</span><span class="sxs-lookup"><span data-stu-id="a65cc-162">Run the **`azure network nsg rule create`** command to create a rule that allows access to port 1433 (SQL) from the front end subnet.</span></span>
   
        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    <span data-ttu-id="a65cc-163">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="a65cc-163">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
3. <span data-ttu-id="a65cc-164">Kör den  **`azure network nsg rule create`**  kommando för att skapa en regel som nekar åtkomst till Internet.</span><span class="sxs-lookup"><span data-stu-id="a65cc-164">Run the **`azure network nsg rule create`** command to create a rule that denies access to the Internet.</span></span>
   
        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   
    <span data-ttu-id="a65cc-165">Förväntade putput:</span><span class="sxs-lookup"><span data-stu-id="a65cc-165">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK
4. <span data-ttu-id="a65cc-166">Kör den  **`azure network nsg subnet add`**  kommando för att länka NSG: N till backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="a65cc-166">Run the **`azure network nsg subnet add`** command to link the NSG to the back end subnet.</span></span>
   
        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
   
    <span data-ttu-id="a65cc-167">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="a65cc-167">Expected output:</span></span>
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK

