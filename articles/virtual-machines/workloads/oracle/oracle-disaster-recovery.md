---
title: "aaaOverview av en Oracle-katastrofåterställning i Azure-miljön | Microsoft Docs"
description: "En katastrofåterställning för en Oracle-databas 12c-databas i Azure-miljön"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/2/2017
ms.author: rclaus
ms.openlocfilehash: 1fa69e1ba044b46b27695fec92fd9ca82df796f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-for-an-oracle-database-12c-database-in-an-azure-environment"></a><span data-ttu-id="00e86-103">Haveriberedskap för en Oracle-databas 12c-databas i en Azure-miljö</span><span class="sxs-lookup"><span data-stu-id="00e86-103">Disaster recovery for an Oracle Database 12c database in an Azure environment</span></span>

## <a name="assumptions"></a><span data-ttu-id="00e86-104">Antaganden</span><span class="sxs-lookup"><span data-stu-id="00e86-104">Assumptions</span></span>

- <span data-ttu-id="00e86-105">Du har en förståelse av Oracle Data Guard design och miljöer i Azure.</span><span class="sxs-lookup"><span data-stu-id="00e86-105">You have an understanding of Oracle Data Guard design and Azure environments.</span></span>


## <a name="goals"></a><span data-ttu-id="00e86-106">Mål</span><span class="sxs-lookup"><span data-stu-id="00e86-106">Goals</span></span>
- <span data-ttu-id="00e86-107">Utforma hello topologi och konfiguration som uppfyller dina krav för disaster recovery (DR).</span><span class="sxs-lookup"><span data-stu-id="00e86-107">Design hello topology and configuration that meet your disaster recovery (DR) requirements.</span></span>

## <a name="scenario-1-primary-and-dr-sites-on-azure"></a><span data-ttu-id="00e86-108">Scenario 1: Den primära servern och DR platser på Azure</span><span class="sxs-lookup"><span data-stu-id="00e86-108">Scenario 1: Primary and DR sites on Azure</span></span>

<span data-ttu-id="00e86-109">En kund har en Oracle databasen set upp på hello primär plats.</span><span class="sxs-lookup"><span data-stu-id="00e86-109">A customer has an Oracle database set up on hello primary site.</span></span> <span data-ttu-id="00e86-110">En DR-plats finns i en annan region.</span><span class="sxs-lookup"><span data-stu-id="00e86-110">A DR site is in a different region.</span></span> <span data-ttu-id="00e86-111">hello kunden använder Oracle Data Guard för snabb återställning mellan dessa platser.</span><span class="sxs-lookup"><span data-stu-id="00e86-111">hello customer uses Oracle Data Guard for quick recovery between these sites.</span></span> <span data-ttu-id="00e86-112">hello primär plats har också en sekundär databas för rapportering och andra ändamål.</span><span class="sxs-lookup"><span data-stu-id="00e86-112">hello primary site also has a secondary database for reporting and other uses.</span></span> 

### <a name="topology"></a><span data-ttu-id="00e86-113">topologi</span><span class="sxs-lookup"><span data-stu-id="00e86-113">Topology</span></span>

<span data-ttu-id="00e86-114">Här följer en sammanfattning av hello installationsprogram för Azure:</span><span class="sxs-lookup"><span data-stu-id="00e86-114">Here is a summary of hello Azure setup:</span></span>

- <span data-ttu-id="00e86-115">Två platser (en primär plats och en DR-plats)</span><span class="sxs-lookup"><span data-stu-id="00e86-115">Two sites (a primary site and a DR site)</span></span>
- <span data-ttu-id="00e86-116">Två virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="00e86-116">Two virtual networks</span></span>
- <span data-ttu-id="00e86-117">Två Oracle-databaser med Data Guard (primär och vänteläge)</span><span class="sxs-lookup"><span data-stu-id="00e86-117">Two Oracle databases with Data Guard (primary and standby)</span></span>
- <span data-ttu-id="00e86-118">Två Oracle-databaser med guld Gate eller Data Guard (endast primär plats)</span><span class="sxs-lookup"><span data-stu-id="00e86-118">Two Oracle databases with Golden Gate or Data Guard (primary site only)</span></span>
- <span data-ttu-id="00e86-119">Två programtjänster, en primär och en på hello DR-plats</span><span class="sxs-lookup"><span data-stu-id="00e86-119">Two application services, one primary and one on hello DR site</span></span>
- <span data-ttu-id="00e86-120">En *tillgänglighetsuppsättning* som används för databasen och programmet hello primär plats</span><span class="sxs-lookup"><span data-stu-id="00e86-120">An *availability set,* which is used for database and application service on hello primary site</span></span>
- <span data-ttu-id="00e86-121">En jumpbox på varje plats, vilket begränsar åtkomst toohello privata nätverk och kan bara logga in som administratör</span><span class="sxs-lookup"><span data-stu-id="00e86-121">One jumpbox on each site, which restricts access toohello private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="00e86-122">En jumpbox programtjänsten, databas och VPN-gateway på separata undernät</span><span class="sxs-lookup"><span data-stu-id="00e86-122">A jumpbox, application service, database, and VPN gateway on separate subnets</span></span>
- <span data-ttu-id="00e86-123">NSG tillämpas på programmet och databasen undernät</span><span class="sxs-lookup"><span data-stu-id="00e86-123">NSG enforced on application and database subnets</span></span>

![Skärmbild av sidan för hello DR-topologi](./media/oracle-disaster-recovery/oracle_topology_01.png)

## <a name="scenario-2-primary-site-on-premises-and-dr-site-on-azure"></a><span data-ttu-id="00e86-125">Scenario 2: Primär plats på anläggningar och DR-plats på Azure</span><span class="sxs-lookup"><span data-stu-id="00e86-125">Scenario 2: Primary site on-premises and DR site on Azure</span></span>

<span data-ttu-id="00e86-126">En kund har en lokal Oracle database-installation (primär plats).</span><span class="sxs-lookup"><span data-stu-id="00e86-126">A customer has an on-premises Oracle database setup (primary site).</span></span> <span data-ttu-id="00e86-127">En DR-plats finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="00e86-127">A DR site is on Azure.</span></span> <span data-ttu-id="00e86-128">Oracle Data Guard används för snabb återställning mellan dessa platser.</span><span class="sxs-lookup"><span data-stu-id="00e86-128">Oracle Data Guard is used for quick recovery between these sites.</span></span> <span data-ttu-id="00e86-129">hello primär plats har också en sekundär databas för rapportering och andra ändamål.</span><span class="sxs-lookup"><span data-stu-id="00e86-129">hello primary site also has a secondary database for reporting and other uses.</span></span> 

<span data-ttu-id="00e86-130">Det finns två tillvägagångssätt för den här installationen.</span><span class="sxs-lookup"><span data-stu-id="00e86-130">There are two approaches for this setup.</span></span>

### <a name="approach-1-direct-connections-between-on-premises-and-azure-requiring-open-tcp-ports-on-hello-firewall"></a><span data-ttu-id="00e86-131">Metod 1: Direkta anslutningar mellan lokala och Azure, som kräver öppna TCP-portar i brandväggen hello</span><span class="sxs-lookup"><span data-stu-id="00e86-131">Approach 1: Direct connections between on-premises and Azure, requiring open TCP ports on hello firewall</span></span> 

<span data-ttu-id="00e86-132">Vi rekommenderar inte direkta anslutningar eftersom de exponera hello TCP-portar toohello utanför världen.</span><span class="sxs-lookup"><span data-stu-id="00e86-132">We don't recommend direct connections because they expose hello TCP ports toohello outside world.</span></span>

#### <a name="topology"></a><span data-ttu-id="00e86-133">topologi</span><span class="sxs-lookup"><span data-stu-id="00e86-133">Topology</span></span>

<span data-ttu-id="00e86-134">Nedan följer en sammanfattning av hello installationsprogram för Azure:</span><span class="sxs-lookup"><span data-stu-id="00e86-134">Following is a summary of hello Azure setup:</span></span>

- <span data-ttu-id="00e86-135">En DR-plats</span><span class="sxs-lookup"><span data-stu-id="00e86-135">One DR site</span></span> 
- <span data-ttu-id="00e86-136">Ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="00e86-136">One virtual network</span></span>
- <span data-ttu-id="00e86-137">En Oracle-databas med Data Guard (aktiv)</span><span class="sxs-lookup"><span data-stu-id="00e86-137">One Oracle database with Data Guard (active)</span></span>
- <span data-ttu-id="00e86-138">Ett program-tjänsten på hello DR-plats</span><span class="sxs-lookup"><span data-stu-id="00e86-138">One application service on hello DR site</span></span>
- <span data-ttu-id="00e86-139">En jumpbox som begränsar åtkomst toohello privata nätverk och kan bara logga in som administratör</span><span class="sxs-lookup"><span data-stu-id="00e86-139">One jumpbox, which restricts access toohello private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="00e86-140">En jumpbox programtjänsten, databas och VPN-gateway på separata undernät</span><span class="sxs-lookup"><span data-stu-id="00e86-140">A jumpbox, application service, database, and VPN gateway on separate subnets</span></span>
- <span data-ttu-id="00e86-141">NSG tillämpas på programmet och databasen undernät</span><span class="sxs-lookup"><span data-stu-id="00e86-141">NSG enforced on application and database subnets</span></span>
- <span data-ttu-id="00e86-142">En NSG principregeln/tooallow inkommande TCP-port 1521 (eller en användardefinierad port)</span><span class="sxs-lookup"><span data-stu-id="00e86-142">An NSG policy/rule tooallow inbound TCP port 1521 (or a user-defined port)</span></span>
- <span data-ttu-id="00e86-143">En NSG principregeln/toorestrict endast hello IP-adress/adresser lokalt (DB eller program) tooaccess hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="00e86-143">An NSG policy/rule toorestrict only hello IP address/addresses on-premises (DB or application) tooaccess hello virtual network</span></span>

![Skärmbild av sidan för hello DR-topologi](./media/oracle-disaster-recovery/oracle_topology_02.png)

### <a name="approach-2-site-to-site-vpn"></a><span data-ttu-id="00e86-145">Metod 2: Plats-till-plats VPN</span><span class="sxs-lookup"><span data-stu-id="00e86-145">Approach 2: Site-to-site VPN</span></span>
<span data-ttu-id="00e86-146">Plats-till-plats VPN är en bättre metod.</span><span class="sxs-lookup"><span data-stu-id="00e86-146">Site-to-site VPN is a better approach.</span></span> <span data-ttu-id="00e86-147">Mer information om hur du konfigurerar en VPN-anslutning finns [skapa ett virtuellt nätverk med en plats-till-plats VPN-anslutning med hjälp av CLI](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span><span class="sxs-lookup"><span data-stu-id="00e86-147">For more information about setting up a VPN, see [Create a virtual network with a Site-to-Site VPN connection using CLI](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span></span>

#### <a name="topology"></a><span data-ttu-id="00e86-148">topologi</span><span class="sxs-lookup"><span data-stu-id="00e86-148">Topology</span></span>

<span data-ttu-id="00e86-149">Nedan följer en sammanfattning av hello installationsprogram för Azure:</span><span class="sxs-lookup"><span data-stu-id="00e86-149">Following is a summary of hello Azure setup:</span></span>

- <span data-ttu-id="00e86-150">En DR-plats</span><span class="sxs-lookup"><span data-stu-id="00e86-150">One DR site</span></span> 
- <span data-ttu-id="00e86-151">Ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="00e86-151">One virtual network</span></span> 
- <span data-ttu-id="00e86-152">En Oracle-databas med Data Guard (aktiv)</span><span class="sxs-lookup"><span data-stu-id="00e86-152">One Oracle database with Data Guard (active)</span></span>
- <span data-ttu-id="00e86-153">Ett program-tjänsten på hello DR-plats</span><span class="sxs-lookup"><span data-stu-id="00e86-153">One application service on hello DR site</span></span>
- <span data-ttu-id="00e86-154">En jumpbox som begränsar åtkomst toohello privata nätverk och kan bara logga in som administratör</span><span class="sxs-lookup"><span data-stu-id="00e86-154">One jumpbox, which restricts access toohello private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="00e86-155">En jumpbox programtjänsten, databas och VPN-gateway finns på separata undernät</span><span class="sxs-lookup"><span data-stu-id="00e86-155">A jumpbox, application service, database, and VPN gateway are on separate subnets</span></span>
- <span data-ttu-id="00e86-156">NSG tillämpas på programmet och databasen undernät</span><span class="sxs-lookup"><span data-stu-id="00e86-156">NSG enforced on application and database subnets</span></span>
- <span data-ttu-id="00e86-157">Plats-till-plats VPN-anslutning mellan Azure och lokalt</span><span class="sxs-lookup"><span data-stu-id="00e86-157">Site-to-site VPN connection between on-premises and Azure</span></span>

![Skärmbild av sidan för hello DR-topologi](./media/oracle-disaster-recovery/oracle_topology_03.png)

## <a name="additional-reading"></a><span data-ttu-id="00e86-159">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="00e86-159">Additional reading</span></span>

- [<span data-ttu-id="00e86-160">Utforma och implementera en Oracle-databas på Azure</span><span class="sxs-lookup"><span data-stu-id="00e86-160">Design and implement an Oracle database on Azure</span></span>](oracle-design.md)
- [<span data-ttu-id="00e86-161">Konfigurera Oracle Data Guard</span><span class="sxs-lookup"><span data-stu-id="00e86-161">Configure Oracle Data Guard</span></span>](configure-oracle-dataguard.md)
- [<span data-ttu-id="00e86-162">Konfigurera Oracle guld Gate</span><span class="sxs-lookup"><span data-stu-id="00e86-162">Configure Oracle Golden Gate</span></span>](configure-oracle-golden-gate.md)
- [<span data-ttu-id="00e86-163">Oracle-säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="00e86-163">Oracle backup and recovery</span></span>](oracle-backup-recovery.md)


## <a name="next-steps"></a><span data-ttu-id="00e86-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="00e86-164">Next steps</span></span>

- [<span data-ttu-id="00e86-165">Självstudier: Skapa högtillgängliga virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="00e86-165">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)
- [<span data-ttu-id="00e86-166">Utforska VM distribution Azure CLI-exempel</span><span class="sxs-lookup"><span data-stu-id="00e86-166">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
