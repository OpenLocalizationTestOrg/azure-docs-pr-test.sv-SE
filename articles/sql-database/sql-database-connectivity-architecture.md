---
title: aaaAzure SQL Database connectivity arkitektur | Microsoft Docs
description: "Det här dokumentet förklarar hello Azure SQLDB anslutning arkitektur från Azure eller från utanför Azure."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: monicar
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/05/2017
ms.author: carlrab
ms.openlocfilehash: 917df6d88a16f1b841b617fb2a53025b4d14d034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-connectivity-architecture"></a><span data-ttu-id="694f9-103">Azure SQL Database Connectivity-arkitektur</span><span class="sxs-lookup"><span data-stu-id="694f9-103">Azure SQL Database Connectivity Architecture</span></span> 

<span data-ttu-id="694f9-104">Den här artikeln beskriver hello Azure SQL Database connectivity arkitektur och förklarar hur hello olika komponenter fungerar toodirect trafik tooyour instans av Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="694f9-104">This article explains hello Azure SQL Database connectivity architecture and explains how hello different components function toodirect traffic tooyour instance of Azure SQL Database.</span></span> <span data-ttu-id="694f9-105">Komponenterna Azure SQL Database connectivity fungera toodirect nätverk trafik toohello Azure-databas med klienter som ansluter från i Azure och anslutande klienter från utanför Azure.</span><span class="sxs-lookup"><span data-stu-id="694f9-105">These Azure SQL Database connectivity components function toodirect network traffic toohello Azure database with clients connecting from within Azure and with clients connecting from outside of Azure.</span></span> <span data-ttu-id="694f9-106">Den här artikeln innehåller också skript prover toochange hur anslutningen sker och hello överväganden relaterade toochanging hello anslutningen standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="694f9-106">This article also provides script samples toochange how connectivity occurs, and hello considerations related toochanging hello default connectivity settings.</span></span> <span data-ttu-id="694f9-107">Om det finns några frågor när du har läst den här artikeln kan du kontakta Dhruv på dmalik@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="694f9-107">If there are any questions after reading this article, please contact Dhruv at dmalik@microsoft.com.</span></span> 

## <a name="connectivity-architecture"></a><span data-ttu-id="694f9-108">Anslutningsarkitektur</span><span class="sxs-lookup"><span data-stu-id="694f9-108">Connectivity architecture</span></span>

<span data-ttu-id="694f9-109">hello följande diagram ger en översikt över hello Azure SQL Database connectivity arkitektur.</span><span class="sxs-lookup"><span data-stu-id="694f9-109">hello following diagram provides a high-level overview of hello Azure SQL Database connectivity architecture.</span></span> 

![arkitektur, översikt](./media/sql-database-connectivity-architecture/architecture-overview.png)


<span data-ttu-id="694f9-111">hello följande steg beskriver hur en anslutning är upprättad tooan Azure SQL database via hello Azure SQL Database-programbelastningsutjämnaren (SLB) och hello Azure SQL Database-gateway.</span><span class="sxs-lookup"><span data-stu-id="694f9-111">hello following steps describe how a connection is established tooan Azure SQL database through hello Azure SQL Database software load-balancer (SLB) and hello Azure SQL Database gateway.</span></span>

- <span data-ttu-id="694f9-112">Klienter i Azure eller utanför Azure ansluta toohello SLB som har en offentlig IP-adress och lyssnar på port 1433.</span><span class="sxs-lookup"><span data-stu-id="694f9-112">Clients within Azure or outside of Azure connect toohello SLB, which has a public IP address and listens on port 1433.</span></span>
- <span data-ttu-id="694f9-113">Hej SLB dirigerar trafik toohello Azure SQL Database-gateway.</span><span class="sxs-lookup"><span data-stu-id="694f9-113">hello SLB directs traffic toohello Azure SQL Database gateway.</span></span>
- <span data-ttu-id="694f9-114">hello-gateway dirigerar hello trafik toohello rätt proxy mellanprogram.</span><span class="sxs-lookup"><span data-stu-id="694f9-114">hello gateway redirects hello traffic toohello correct proxy middleware.</span></span>
- <span data-ttu-id="694f9-115">hello proxy mellanprogram omdirigerar hello-trafik toohello lämplig Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="694f9-115">hello proxy middleware redirects hello traffic toohello appropriate Azure SQL database.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="694f9-116">Var och en av dessa komponenter har distribuerats för tjänsten (DDoS) skydd inbyggda på hello nätverk och hello app lager.</span><span class="sxs-lookup"><span data-stu-id="694f9-116">Each of these components has distributed denial of service (DDoS) protection built-in at hello network and hello app layer.</span></span>
>

## <a name="connectivity-from-within-azure"></a><span data-ttu-id="694f9-117">Om du använder i Azure</span><span class="sxs-lookup"><span data-stu-id="694f9-117">Connectivity from within Azure</span></span>

<span data-ttu-id="694f9-118">Om du ansluter från i Azure anslutningarna har en princip för **omdirigera** som standard.</span><span class="sxs-lookup"><span data-stu-id="694f9-118">If you are connecting from within Azure, your connections have a connection policy of **Redirect** by default.</span></span> <span data-ttu-id="694f9-119">En princip av **omdirigera** innebär att anslutningar när hello TCP-session är etablerad toohello Azure SQL database, hello klientsessionen sedan omdirigeras toohello proxy mellanprogram med en ändring toohello mål virtuella IP-adress från som hello Azure SQL Database gateway toothat av hello proxy mellanprogram.</span><span class="sxs-lookup"><span data-stu-id="694f9-119">A policy of **Redirect** means that connections after hello TCP session is established toohello Azure SQL database, hello client session is then redirected toohello proxy middleware with a change toohello destination virtual IP from that of hello Azure SQL Database gateway toothat of hello proxy middleware.</span></span> <span data-ttu-id="694f9-120">Därefter alla efterföljande paket som flödar direkt via hello proxy mellanprogram, vilket kringgår hello Azure SQL Database-gateway.</span><span class="sxs-lookup"><span data-stu-id="694f9-120">Thereafter, all subsequent packets flow directly via hello proxy middleware, bypassing hello Azure SQL Database gateway.</span></span> <span data-ttu-id="694f9-121">hello följande diagram illustrerar flödet i nätverkstrafiken.</span><span class="sxs-lookup"><span data-stu-id="694f9-121">hello following diagram illustrates this traffic flow.</span></span>

![arkitektur, översikt](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

## <a name="connectivity-from-outside-of-azure"></a><span data-ttu-id="694f9-123">Anslutningen från utanför Azure</span><span class="sxs-lookup"><span data-stu-id="694f9-123">Connectivity from outside of Azure</span></span>

<span data-ttu-id="694f9-124">Om du ansluter från utanför Azure anslutningarna har en princip för **Proxy** som standard.</span><span class="sxs-lookup"><span data-stu-id="694f9-124">If you are connecting from outside Azure, your connections have a connection policy of **Proxy** by default.</span></span> <span data-ttu-id="694f9-125">En princip av **Proxy** innebär att hello TCP-sessionen har upprättats via hello Azure SQL Database-gateway och alla efterföljande paket flöda hello gateway.</span><span class="sxs-lookup"><span data-stu-id="694f9-125">A policy of **Proxy** means that hello TCP session is established via hello Azure SQL Database gateway and all subsequent packets flow via hello gateway.</span></span> <span data-ttu-id="694f9-126">hello följande diagram illustrerar flödet i nätverkstrafiken.</span><span class="sxs-lookup"><span data-stu-id="694f9-126">hello following diagram illustrates this traffic flow.</span></span>

![arkitektur, översikt](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a><span data-ttu-id="694f9-128">Azure SQL Database gateway IP-adresser</span><span class="sxs-lookup"><span data-stu-id="694f9-128">Azure SQL Database gateway IP addresses</span></span>

<span data-ttu-id="694f9-129">tooconnect tooan Azure SQL-databas från lokala resurser, behöver du tooallow utgående trafik toohello Azure SQL Database nätverksgateway för din Azure-region.</span><span class="sxs-lookup"><span data-stu-id="694f9-129">tooconnect tooan Azure SQL database from on-premises resources, you need tooallow outbound network traffic toohello Azure SQL Database gateway for your Azure region.</span></span> <span data-ttu-id="694f9-130">Anslutningar kan bara gå via hello gateway vid anslutning i proxyläge är hello standard när du ansluter från lokala resurser.</span><span class="sxs-lookup"><span data-stu-id="694f9-130">Your connections only go via hello gateway when connecting in Proxy mode, which is hello default when connecting from on-premises resources.</span></span>

<span data-ttu-id="694f9-131">följande tabell visar hello hello primära och sekundära IP-adresser för hello Azure SQL Database-gateway för alla dataområden.</span><span class="sxs-lookup"><span data-stu-id="694f9-131">hello following table lists hello primary and secondary IPs of hello Azure SQL Database gateway for all data regions.</span></span> <span data-ttu-id="694f9-132">Det finns två IP-adresser för vissa regioner.</span><span class="sxs-lookup"><span data-stu-id="694f9-132">For some regions, there are two IP addresses.</span></span> <span data-ttu-id="694f9-133">I dessa regioner hello primära IP-adress är hello aktuella IP-adressen för hello gateway och hello andra IP-adressen är en IP-adress för redundans.</span><span class="sxs-lookup"><span data-stu-id="694f9-133">In these regions, hello primary IP address is hello current IP address of hello gateway and hello second IP address is a failover IP address.</span></span> <span data-ttu-id="694f9-134">hello-adress för redundans är hello adress toowhich vi kan flytta din server tookeep hello tjänsttillgänglighet hög.</span><span class="sxs-lookup"><span data-stu-id="694f9-134">hello failover address is hello address toowhich we might move your server tookeep hello service availability high.</span></span> <span data-ttu-id="694f9-135">För dessa regioner rekommenderar vi att du utgående tooboth hello IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="694f9-135">For these regions, we recommend that you allow outbound tooboth hello IP addresses.</span></span> <span data-ttu-id="694f9-136">hello andra IP-adressen ägs av Microsoft och lyssnar inte på några tjänster förrän den aktiveras av Azure SQL Database tooaccept anslutningar.</span><span class="sxs-lookup"><span data-stu-id="694f9-136">hello second IP address is owned by Microsoft and does not listen in on any services until it is activated by Azure SQL Database tooaccept connections.</span></span>

| <span data-ttu-id="694f9-137">Regionsnamn</span><span class="sxs-lookup"><span data-stu-id="694f9-137">Region Name</span></span> | <span data-ttu-id="694f9-138">Primära IP-adress</span><span class="sxs-lookup"><span data-stu-id="694f9-138">Primary IP address</span></span> | <span data-ttu-id="694f9-139">Sekundär IP-adress</span><span class="sxs-lookup"><span data-stu-id="694f9-139">Secondary IP address</span></span> |
| --- | --- |--- |
| <span data-ttu-id="694f9-140">Östra Australien</span><span class="sxs-lookup"><span data-stu-id="694f9-140">Australia East</span></span> | <span data-ttu-id="694f9-141">191.238.66.109</span><span class="sxs-lookup"><span data-stu-id="694f9-141">191.238.66.109</span></span> | <span data-ttu-id="694f9-142">13.75.149.87</span><span class="sxs-lookup"><span data-stu-id="694f9-142">13.75.149.87</span></span> |
| <span data-ttu-id="694f9-143">Sydöstra Australien</span><span class="sxs-lookup"><span data-stu-id="694f9-143">Australia South East</span></span> | <span data-ttu-id="694f9-144">191.239.192.109</span><span class="sxs-lookup"><span data-stu-id="694f9-144">191.239.192.109</span></span> | <span data-ttu-id="694f9-145">13.73.109.251</span><span class="sxs-lookup"><span data-stu-id="694f9-145">13.73.109.251</span></span> |
| <span data-ttu-id="694f9-146">Södra Brasilien</span><span class="sxs-lookup"><span data-stu-id="694f9-146">Brazil South</span></span> | <span data-ttu-id="694f9-147">104.41.11.5</span><span class="sxs-lookup"><span data-stu-id="694f9-147">104.41.11.5</span></span> | |    
| <span data-ttu-id="694f9-148">Centrala Kanada</span><span class="sxs-lookup"><span data-stu-id="694f9-148">Canada Central</span></span> | <span data-ttu-id="694f9-149">40.85.224.249</span><span class="sxs-lookup"><span data-stu-id="694f9-149">40.85.224.249</span></span> | |    
| <span data-ttu-id="694f9-150">Östra Kanada</span><span class="sxs-lookup"><span data-stu-id="694f9-150">Canada East</span></span> | <span data-ttu-id="694f9-151">40.86.226.166</span><span class="sxs-lookup"><span data-stu-id="694f9-151">40.86.226.166</span></span> | |
| <span data-ttu-id="694f9-152">Centrala USA</span><span class="sxs-lookup"><span data-stu-id="694f9-152">Central US</span></span> | <span data-ttu-id="694f9-153">23.99.160.139</span><span class="sxs-lookup"><span data-stu-id="694f9-153">23.99.160.139</span></span> | <span data-ttu-id="694f9-154">13.67.215.62</span><span class="sxs-lookup"><span data-stu-id="694f9-154">13.67.215.62</span></span> |
| <span data-ttu-id="694f9-155">Östasien</span><span class="sxs-lookup"><span data-stu-id="694f9-155">East Asia</span></span> | <span data-ttu-id="694f9-156">191.234.2.139</span><span class="sxs-lookup"><span data-stu-id="694f9-156">191.234.2.139</span></span> | <span data-ttu-id="694f9-157">52.175.33.150</span><span class="sxs-lookup"><span data-stu-id="694f9-157">52.175.33.150</span></span> |
| <span data-ttu-id="694f9-158">Östra USA 1</span><span class="sxs-lookup"><span data-stu-id="694f9-158">East US 1</span></span> | <span data-ttu-id="694f9-159">191.238.6.43</span><span class="sxs-lookup"><span data-stu-id="694f9-159">191.238.6.43</span></span> | <span data-ttu-id="694f9-160">40.121.158.30</span><span class="sxs-lookup"><span data-stu-id="694f9-160">40.121.158.30</span></span> |
| <span data-ttu-id="694f9-161">Östra USA 2</span><span class="sxs-lookup"><span data-stu-id="694f9-161">East US 2</span></span> | <span data-ttu-id="694f9-162">191.239.224.107</span><span class="sxs-lookup"><span data-stu-id="694f9-162">191.239.224.107</span></span> | <span data-ttu-id="694f9-163">40.79.84.180</span><span class="sxs-lookup"><span data-stu-id="694f9-163">40.79.84.180</span></span> |
| <span data-ttu-id="694f9-164">Centrala Indien</span><span class="sxs-lookup"><span data-stu-id="694f9-164">India Central</span></span> | <span data-ttu-id="694f9-165">104.211.96.159</span><span class="sxs-lookup"><span data-stu-id="694f9-165">104.211.96.159</span></span>  | |   
| <span data-ttu-id="694f9-166">Södra Indien</span><span class="sxs-lookup"><span data-stu-id="694f9-166">India South</span></span> | <span data-ttu-id="694f9-167">104.211.224.146</span><span class="sxs-lookup"><span data-stu-id="694f9-167">104.211.224.146</span></span>  | |
| <span data-ttu-id="694f9-168">Västra Indien</span><span class="sxs-lookup"><span data-stu-id="694f9-168">India West</span></span> | <span data-ttu-id="694f9-169">104.211.160.80</span><span class="sxs-lookup"><span data-stu-id="694f9-169">104.211.160.80</span></span> | |
| <span data-ttu-id="694f9-170">Östra Japan</span><span class="sxs-lookup"><span data-stu-id="694f9-170">Japan East</span></span> | <span data-ttu-id="694f9-171">191.237.240.43</span><span class="sxs-lookup"><span data-stu-id="694f9-171">191.237.240.43</span></span> | <span data-ttu-id="694f9-172">13.78.61.196</span><span class="sxs-lookup"><span data-stu-id="694f9-172">13.78.61.196</span></span> |
| <span data-ttu-id="694f9-173">Västra Japan</span><span class="sxs-lookup"><span data-stu-id="694f9-173">Japan West</span></span> | <span data-ttu-id="694f9-174">191.238.68.11</span><span class="sxs-lookup"><span data-stu-id="694f9-174">191.238.68.11</span></span> | <span data-ttu-id="694f9-175">104.214.148.156</span><span class="sxs-lookup"><span data-stu-id="694f9-175">104.214.148.156</span></span> |
| <span data-ttu-id="694f9-176">Centrala Korea</span><span class="sxs-lookup"><span data-stu-id="694f9-176">Korea Central</span></span> | <span data-ttu-id="694f9-177">52.231.32.42</span><span class="sxs-lookup"><span data-stu-id="694f9-177">52.231.32.42</span></span> | |
| <span data-ttu-id="694f9-178">Sydkorea</span><span class="sxs-lookup"><span data-stu-id="694f9-178">Korea South</span></span> | <span data-ttu-id="694f9-179">52.231.200.86</span><span class="sxs-lookup"><span data-stu-id="694f9-179">52.231.200.86</span></span> |  |
| <span data-ttu-id="694f9-180">Norra centrala USA</span><span class="sxs-lookup"><span data-stu-id="694f9-180">North Central US</span></span> | <span data-ttu-id="694f9-181">23.98.55.75</span><span class="sxs-lookup"><span data-stu-id="694f9-181">23.98.55.75</span></span> | <span data-ttu-id="694f9-182">23.96.178.199</span><span class="sxs-lookup"><span data-stu-id="694f9-182">23.96.178.199</span></span> |
| <span data-ttu-id="694f9-183">Norra Europa</span><span class="sxs-lookup"><span data-stu-id="694f9-183">North Europe</span></span> | <span data-ttu-id="694f9-184">191.235.193.75</span><span class="sxs-lookup"><span data-stu-id="694f9-184">191.235.193.75</span></span> | <span data-ttu-id="694f9-185">40.113.93.91</span><span class="sxs-lookup"><span data-stu-id="694f9-185">40.113.93.91</span></span> |
| <span data-ttu-id="694f9-186">Södra centrala USA</span><span class="sxs-lookup"><span data-stu-id="694f9-186">South Central US</span></span> | <span data-ttu-id="694f9-187">23.98.162.75</span><span class="sxs-lookup"><span data-stu-id="694f9-187">23.98.162.75</span></span> | <span data-ttu-id="694f9-188">13.66.62.124</span><span class="sxs-lookup"><span data-stu-id="694f9-188">13.66.62.124</span></span> |
| <span data-ttu-id="694f9-189">Sydostasien</span><span class="sxs-lookup"><span data-stu-id="694f9-189">South East Asia</span></span> | <span data-ttu-id="694f9-190">23.100.117.95</span><span class="sxs-lookup"><span data-stu-id="694f9-190">23.100.117.95</span></span> | <span data-ttu-id="694f9-191">104.43.15.0</span><span class="sxs-lookup"><span data-stu-id="694f9-191">104.43.15.0</span></span> |
| <span data-ttu-id="694f9-192">Storbritannien, norra</span><span class="sxs-lookup"><span data-stu-id="694f9-192">UK North</span></span> | <span data-ttu-id="694f9-193">13.87.97.210</span><span class="sxs-lookup"><span data-stu-id="694f9-193">13.87.97.210</span></span> | |
| <span data-ttu-id="694f9-194">Storbritannien, Syd 1</span><span class="sxs-lookup"><span data-stu-id="694f9-194">UK South 1</span></span> | <span data-ttu-id="694f9-195">51.140.184.11</span><span class="sxs-lookup"><span data-stu-id="694f9-195">51.140.184.11</span></span> | |    
| <span data-ttu-id="694f9-196">Storbritannien, södra 2</span><span class="sxs-lookup"><span data-stu-id="694f9-196">UK South 2</span></span> | <span data-ttu-id="694f9-197">13.87.34.7</span><span class="sxs-lookup"><span data-stu-id="694f9-197">13.87.34.7</span></span> | |
| <span data-ttu-id="694f9-198">Storbritannien, västra</span><span class="sxs-lookup"><span data-stu-id="694f9-198">UK West</span></span> | <span data-ttu-id="694f9-199">51.141.8.11</span><span class="sxs-lookup"><span data-stu-id="694f9-199">51.141.8.11</span></span>  | |
| <span data-ttu-id="694f9-200">Västra centrala USA</span><span class="sxs-lookup"><span data-stu-id="694f9-200">West Central US</span></span> | <span data-ttu-id="694f9-201">13.78.145.25</span><span class="sxs-lookup"><span data-stu-id="694f9-201">13.78.145.25</span></span> | |
| <span data-ttu-id="694f9-202">Västra Europa</span><span class="sxs-lookup"><span data-stu-id="694f9-202">West Europe</span></span> | <span data-ttu-id="694f9-203">191.237.232.75</span><span class="sxs-lookup"><span data-stu-id="694f9-203">191.237.232.75</span></span> | <span data-ttu-id="694f9-204">40.68.37.158</span><span class="sxs-lookup"><span data-stu-id="694f9-204">40.68.37.158</span></span> |
| <span data-ttu-id="694f9-205">USA, västra 1</span><span class="sxs-lookup"><span data-stu-id="694f9-205">West US 1</span></span> | <span data-ttu-id="694f9-206">23.99.34.75</span><span class="sxs-lookup"><span data-stu-id="694f9-206">23.99.34.75</span></span> | <span data-ttu-id="694f9-207">104.42.238.205</span><span class="sxs-lookup"><span data-stu-id="694f9-207">104.42.238.205</span></span> |
| <span data-ttu-id="694f9-208">Västra USA 2</span><span class="sxs-lookup"><span data-stu-id="694f9-208">West US 2</span></span> | <span data-ttu-id="694f9-209">13.66.226.202</span><span class="sxs-lookup"><span data-stu-id="694f9-209">13.66.226.202</span></span>  | |
||||

## <a name="change-azure-sql-database-connection-policy"></a><span data-ttu-id="694f9-210">Ändra principen för Azure SQL Database-anslutning</span><span class="sxs-lookup"><span data-stu-id="694f9-210">Change Azure SQL Database connection policy</span></span>

<span data-ttu-id="694f9-211">toochange hello Azure SQL Database-princip för en Azure SQL Database-server, Använd hello [REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span><span class="sxs-lookup"><span data-stu-id="694f9-211">toochange hello Azure SQL Database connection policy for an Azure SQL Database server, use hello [REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span></span> 

- <span data-ttu-id="694f9-212">Om din anslutningsprincip har angetts för**Proxy**, alla nätverksenheter paket flöde via hello Azure SQL Database-gateway.</span><span class="sxs-lookup"><span data-stu-id="694f9-212">If your connection policy is set too**Proxy**, all network packets flow via hello Azure SQL Database gateway.</span></span> <span data-ttu-id="694f9-213">Du måste tooallow utgående tooonly hello Azure SQL Database gateway IP för den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="694f9-213">For this setting, you need tooallow outbound tooonly hello Azure SQL Database gateway IP.</span></span> <span data-ttu-id="694f9-214">Med hjälp av inställningen **Proxy** har flera svarstider än inställningen **omdirigera**.</span><span class="sxs-lookup"><span data-stu-id="694f9-214">Using a setting of **Proxy** has more latency than a setting of **Redirect**.</span></span> 
- <span data-ttu-id="694f9-215">Om din anslutning principinställningen **omdirigera**, alla nätverkspaket flöda direkt toohello mellanprogram proxy.</span><span class="sxs-lookup"><span data-stu-id="694f9-215">If your connection policy is setting **Redirect**, all network packets flow directly toohello middleware proxy.</span></span> <span data-ttu-id="694f9-216">Du måste tooallow utgående toomultiple IP-adresser för den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="694f9-216">For this setting, you need tooallow outbound toomultiple IPs.</span></span> 

## <a name="script-toochange-connection-settings"></a><span data-ttu-id="694f9-217">Skriptet toochange anslutningsinställningar</span><span class="sxs-lookup"><span data-stu-id="694f9-217">Script toochange connection settings</span></span>

> [!IMPORTANT]
> <span data-ttu-id="694f9-218">Skriptet kräver hello [Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="694f9-218">This script requires hello [Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>
>

<span data-ttu-id="694f9-219">hello följande PowerShell-skript visar hur toochange hello anslutningsprincip.</span><span class="sxs-lookup"><span data-stu-id="694f9-219">hello following PowerShell script shows how toochange hello connection policy.</span></span>

```powershell
import-module azureRm
Login-AzureRmAccount

$tenantId =  #your AAD tenant ID
$subscriptionId = #Azure SubscriptionID
$uri = #AAD uri
$authUrl = "https://login.microsoftonline.com/$tenantId"
$serverName = #sqldb server name 
$resourceGroupName=#sqldb resource group
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl

$result = $AuthContext.AcquireToken("https://management.core.windows.net/",
$clientId,
[Uri]$uri, 
[Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Auto)

$authHeader = @{
'Content-Type'='application\json; '
'Authorization'=$result.CreateAuthorizationHeader()
}

#getting hello current connection property
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method GET -Headers $authHeader

#setting hello property too‘Proxy’
$connectionType=”Proxy” <#Redirect / Default are other options#>
$body = @{properties=@{connectionType=$connectionType}} | ConvertTo-Json

Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method PUT -Headers $authHeader -Body $body -ContentType "application/json"
```

## <a name="next-steps"></a><span data-ttu-id="694f9-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="694f9-220">Next steps</span></span>

- <span data-ttu-id="694f9-221">Mer information om hur toochange hello Azure SQL Database-princip för en Azure SQL Database-server finns [skapa eller uppdateringsprincip Server-anslutning med hjälp av hello REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span><span class="sxs-lookup"><span data-stu-id="694f9-221">For information on how toochange hello Azure SQL Database connection policy for an Azure SQL Database server, see [Create or Update Server Connection Policy using hello REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span></span>
- <span data-ttu-id="694f9-222">Mer information om Azure SQL Database-anslutningen för klienter som använder ADO.NET 4.5 eller senare finns [portar utöver 1433 ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).</span><span class="sxs-lookup"><span data-stu-id="694f9-222">For information about Azure SQL Database connection behavior for clients that use ADO.NET 4.5 or a later version, see [Ports beyond 1433 for ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).</span></span>
- <span data-ttu-id="694f9-223">Allmänt program development översikt översiktsinformation finns i [SQL Database Development Programöversikt](sql-database-develop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="694f9-223">For general application development overview information, see [SQL Database Application Development Overview](sql-database-develop-overview.md).</span></span>
