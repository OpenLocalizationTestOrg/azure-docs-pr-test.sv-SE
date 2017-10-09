---
title: "aaaHow toocreate NSG: er i klassiskt läge med hjälp av hello Azure CLI | Microsoft Docs"
description: "Lär dig hur toocreate och distribuera NSG: er i klassiskt läge som använder hello Azure CLI"
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
ms.openlocfilehash: eb78861e10a0dd950bb2c3783ee957d1cce55016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-nsgs-classic-in-hello-azure-cli"></a><span data-ttu-id="e8e61-103">Hur toocreate NSG: er som (klassiskt) i hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e8e61-103">How toocreate NSGs (classic) in hello Azure CLI</span></span>
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="e8e61-104">Den här artikeln beskriver hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="e8e61-104">This article covers hello classic deployment model.</span></span> <span data-ttu-id="e8e61-105">Du kan också [skapa NSG: er i hello Resource Manager-distributionsmodellen](virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e8e61-105">You can also [create NSGs in hello Resource Manager deployment model](virtual-networks-create-nsg-arm-cli.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="e8e61-106">hello exempel Azure CLI-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="e8e61-106">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="e8e61-107">Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö av [skapa ett virtuellt nätverk](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e8e61-107">If you want toorun hello commands as they are displayed in this document, first build hello test environment by [creating a VNet](virtual-networks-create-vnet-classic-cli.md).</span></span>

## <a name="how-toocreate-hello-nsg-for-hello-front-end-subnet"></a><span data-ttu-id="e8e61-108">Hur toocreate hello NSG för hello klientdelens undernät</span><span class="sxs-lookup"><span data-stu-id="e8e61-108">How toocreate hello NSG for hello front end subnet</span></span>
<span data-ttu-id="e8e61-109">toocreate en NSG med namnet med namnet **NSG-klientdel** utifrån hello scenariot ovan gör hello nedan.</span><span class="sxs-lookup"><span data-stu-id="e8e61-109">toocreate an NSG named named **NSG-FrontEnd** based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="e8e61-110">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e8e61-110">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="e8e61-111">Kör hello  **`azure config mode`**  kommandot tooswitch tooclassic läge, som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="e8e61-111">Run hello **`azure config mode`** command tooswitch tooclassic mode, as shown below.</span></span>
   
        azure config mode asm
   
    <span data-ttu-id="e8e61-112">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="e8e61-112">Expected output:</span></span>
   
        info:    New mode is asm
3. <span data-ttu-id="e8e61-113">Kör hello  **`azure network nsg create`**  kommandot toocreate en NSG.</span><span class="sxs-lookup"><span data-stu-id="e8e61-113">Run hello **`azure network nsg create`** command toocreate an NSG.</span></span>
   
        azure network nsg create -l uswest -n NSG-FrontEnd
   
    <span data-ttu-id="e8e61-114">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="e8e61-114">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up hello network security group "NSG-FrontEnd"
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
   
    <span data-ttu-id="e8e61-115">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="e8e61-115">Parameters:</span></span>
   
   * <span data-ttu-id="e8e61-116">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-116">**-l (or --location)**.</span></span> <span data-ttu-id="e8e61-117">Azure-region där hello ny NSG kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="e8e61-117">Azure region where hello new NSG will be created.</span></span> <span data-ttu-id="e8e61-118">I vårt scenario, *westus*.</span><span class="sxs-lookup"><span data-stu-id="e8e61-118">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="e8e61-119">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-119">**-n (or --name)**.</span></span> <span data-ttu-id="e8e61-120">Namn för hello ny NSG.</span><span class="sxs-lookup"><span data-stu-id="e8e61-120">Name for hello new NSG.</span></span> <span data-ttu-id="e8e61-121">I vårt scenario, *NSG-klientdel*.</span><span class="sxs-lookup"><span data-stu-id="e8e61-121">For our scenario, *NSG-FrontEnd*.</span></span>
4. <span data-ttu-id="e8e61-122">Kör hello  **`azure network nsg rule create`**  kommandot toocreate en regel som tillåter åtkomst tooport 3389 (RDP) från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="e8e61-122">Run hello **`azure network nsg rule create`** command toocreate a rule that allows access tooport 3389 (RDP) from hello Internet.</span></span>
   
        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    <span data-ttu-id="e8e61-123">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="e8e61-123">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
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
   
    <span data-ttu-id="e8e61-124">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="e8e61-124">Parameters:</span></span>
   
   * <span data-ttu-id="e8e61-125">**-a (eller--nsg-namn)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-125">**-a (or --nsg-name)**.</span></span> <span data-ttu-id="e8e61-126">Namnet på hello NSG vilka hello regel kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="e8e61-126">Name of hello NSG in which hello rule will be created.</span></span> <span data-ttu-id="e8e61-127">I vårt scenario, *NSG-klientdel*.</span><span class="sxs-lookup"><span data-stu-id="e8e61-127">For our scenario, *NSG-FrontEnd*.</span></span>
   * <span data-ttu-id="e8e61-128">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-128">**-n (or --name)**.</span></span> <span data-ttu-id="e8e61-129">Namn på hello nya regeln.</span><span class="sxs-lookup"><span data-stu-id="e8e61-129">Name for hello new rule.</span></span> <span data-ttu-id="e8e61-130">I vårt scenario, *rdp-regel*.</span><span class="sxs-lookup"><span data-stu-id="e8e61-130">For our scenario, *rdp-rule*.</span></span>
   * <span data-ttu-id="e8e61-131">**-c (eller--åtgärd)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-131">**-c (or --action)**.</span></span> <span data-ttu-id="e8e61-132">Åtkomstnivå för hello regel (Tillåt eller neka).</span><span class="sxs-lookup"><span data-stu-id="e8e61-132">Access level for hello rule (Deny or Allow).</span></span>
   * <span data-ttu-id="e8e61-133">**-p (eller--protocol)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-133">**-p (or --protocol)**.</span></span> <span data-ttu-id="e8e61-134">Protocol (Tcp, Udp eller *) för hello regeln.</span><span class="sxs-lookup"><span data-stu-id="e8e61-134">Protocol (Tcp, Udp, or *) for hello rule.</span></span>
   * <span data-ttu-id="e8e61-135">**-r (eller--typ)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-135">**-r (or --type)**.</span></span> <span data-ttu-id="e8e61-136">Riktning för anslutning (inkommande eller utgående).</span><span class="sxs-lookup"><span data-stu-id="e8e61-136">Direction of connection (Inbound or Outbound).</span></span>
   * <span data-ttu-id="e8e61-137">**-y (eller--prioritet)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-137">**-y (or --priority)**.</span></span> <span data-ttu-id="e8e61-138">Prioritet för hello regeln.</span><span class="sxs-lookup"><span data-stu-id="e8e61-138">Priority for hello rule.</span></span>
   * <span data-ttu-id="e8e61-139">**-f (eller--källadress-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-139">**-f (or --source-address-prefix)**.</span></span> <span data-ttu-id="e8e61-140">Källadress-prefix i CIDR- eller använda standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="e8e61-140">Source address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="e8e61-141">**-o (eller--Källportintervall-)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-141">**-o (or --source-port-range)**.</span></span> <span data-ttu-id="e8e61-142">Källport eller portintervall.</span><span class="sxs-lookup"><span data-stu-id="e8e61-142">Source port, or port range.</span></span>
   * <span data-ttu-id="e8e61-143">**-e (eller--mål adressprefixet)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-143">**-e (or --destination-address-prefix)**.</span></span> <span data-ttu-id="e8e61-144">Måladress-prefix i CIDR- eller använda standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="e8e61-144">Destination address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="e8e61-145">**-u (eller--Målportintervall-)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-145">**-u (or --destination-port-range)**.</span></span> <span data-ttu-id="e8e61-146">Målport eller portintervall.</span><span class="sxs-lookup"><span data-stu-id="e8e61-146">Destination port, or port range.</span></span>
5. <span data-ttu-id="e8e61-147">Kör hello  **`azure network nsg rule create`**  kommandot toocreate en regel som tillåter åtkomst tooport 80 (HTTP) från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="e8e61-147">Run hello **`azure network nsg rule create`** command toocreate a rule that allows access tooport 80 (HTTP) from hello Internet.</span></span>
   
        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    <span data-ttu-id="e8e61-148">Förväntade putput:</span><span class="sxs-lookup"><span data-stu-id="e8e61-148">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
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
6. <span data-ttu-id="e8e61-149">Kör hello  **`azure network nsg subnet add`**  kommandot toolink hello NSG toohello klientdelens undernät.</span><span class="sxs-lookup"><span data-stu-id="e8e61-149">Run hello **`azure network nsg subnet add`** command toolink hello NSG toohello front end subnet.</span></span>
   
        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   
    <span data-ttu-id="e8e61-150">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="e8e61-150">Expected output:</span></span>
   
        info:    Executing command network nsg subnet add
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-toocreate-hello-nsg-for-hello-back-end-subnet"></a><span data-ttu-id="e8e61-151">Hur toocreate hello NSG för hello tillbaka avslutas undernät</span><span class="sxs-lookup"><span data-stu-id="e8e61-151">How toocreate hello NSG for hello back end subnet</span></span>
<span data-ttu-id="e8e61-152">toocreate en NSG med namnet med namnet *NSG BackEnd* utifrån hello scenariot ovan gör hello nedan.</span><span class="sxs-lookup"><span data-stu-id="e8e61-152">toocreate an NSG named named *NSG-BackEnd* based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="e8e61-153">Kör hello  **`azure network nsg create`**  kommandot toocreate en NSG.</span><span class="sxs-lookup"><span data-stu-id="e8e61-153">Run hello **`azure network nsg create`** command toocreate an NSG.</span></span>
   
        azure network nsg create -l uswest -n NSG-BackEnd
   
    <span data-ttu-id="e8e61-154">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="e8e61-154">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up hello network security group "NSG-BackEnd"
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
   
    <span data-ttu-id="e8e61-155">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="e8e61-155">Parameters:</span></span>
   
   * <span data-ttu-id="e8e61-156">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-156">**-l (or --location)**.</span></span> <span data-ttu-id="e8e61-157">Azure-region där hello ny NSG kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="e8e61-157">Azure region where hello new NSG will be created.</span></span> <span data-ttu-id="e8e61-158">I vårt scenario, *westus*.</span><span class="sxs-lookup"><span data-stu-id="e8e61-158">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="e8e61-159">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="e8e61-159">**-n (or --name)**.</span></span> <span data-ttu-id="e8e61-160">Namn för hello ny NSG.</span><span class="sxs-lookup"><span data-stu-id="e8e61-160">Name for hello new NSG.</span></span> <span data-ttu-id="e8e61-161">I vårt scenario, *NSG-klientdel*.</span><span class="sxs-lookup"><span data-stu-id="e8e61-161">For our scenario, *NSG-FrontEnd*.</span></span>
2. <span data-ttu-id="e8e61-162">Kör hello  **`azure network nsg rule create`**  kommandot toocreate en regel som tillåter åtkomst tooport 1433 (SQL) från hello klientdelens undernät.</span><span class="sxs-lookup"><span data-stu-id="e8e61-162">Run hello **`azure network nsg rule create`** command toocreate a rule that allows access tooport 1433 (SQL) from hello front end subnet.</span></span>
   
        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    <span data-ttu-id="e8e61-163">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="e8e61-163">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
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
3. <span data-ttu-id="e8e61-164">Kör hello  **`azure network nsg rule create`**  kommandot toocreate en regel som nekar åtkomst toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="e8e61-164">Run hello **`azure network nsg rule create`** command toocreate a rule that denies access toohello Internet.</span></span>
   
        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   
    <span data-ttu-id="e8e61-165">Förväntade putput:</span><span class="sxs-lookup"><span data-stu-id="e8e61-165">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
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
4. <span data-ttu-id="e8e61-166">Kör hello  **`azure network nsg subnet add`**  kommandot toolink hello NSG toohello tillbaka avslutas undernät.</span><span class="sxs-lookup"><span data-stu-id="e8e61-166">Run hello **`azure network nsg subnet add`** command toolink hello NSG toohello back end subnet.</span></span>
   
        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
   
    <span data-ttu-id="e8e61-167">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="e8e61-167">Expected output:</span></span>
   
        info:    Executing command network nsg subnet add
        info:    Looking up hello network security group "NSG-BackEndX"
        info:    Looking up hello subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK

