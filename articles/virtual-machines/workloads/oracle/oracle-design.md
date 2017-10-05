---
title: "Utforma och implementera en Oracle-databas på Azure | Microsoft Docs"
description: "Utforma och implementera en Oracle-databas i Azure-miljön."
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
ms.date: 6/22/2017
ms.author: rclaus
ms.openlocfilehash: 1af7e1d40a0eb129875dd6a30ac899f2025bee13
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a><span data-ttu-id="cdeee-103">Utforma och implementera en Oracle-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="cdeee-103">Design and implement an Oracle database in Azure</span></span>

## <a name="assumptions"></a><span data-ttu-id="cdeee-104">Antaganden</span><span class="sxs-lookup"><span data-stu-id="cdeee-104">Assumptions</span></span>

- <span data-ttu-id="cdeee-105">Du planerar att migrera en Oracle-databas från lokal till Azure.</span><span class="sxs-lookup"><span data-stu-id="cdeee-105">You're planning to migrate an Oracle database from on-premises to Azure.</span></span>
- <span data-ttu-id="cdeee-106">Du har en förståelse av de olika mätvärdena i Oracle AWR rapporter.</span><span class="sxs-lookup"><span data-stu-id="cdeee-106">You have an understanding of the various metrics in Oracle AWR reports.</span></span>
- <span data-ttu-id="cdeee-107">Du har en grundläggande förståelse för programmets prestanda och användning av plattform.</span><span class="sxs-lookup"><span data-stu-id="cdeee-107">You have a baseline understanding of application performance and platform utilization.</span></span>

## <a name="goals"></a><span data-ttu-id="cdeee-108">Mål</span><span class="sxs-lookup"><span data-stu-id="cdeee-108">Goals</span></span>

- <span data-ttu-id="cdeee-109">Förstå hur du optimerar distributionen Oracle i Azure.</span><span class="sxs-lookup"><span data-stu-id="cdeee-109">Understand how to optimize your Oracle deployment in Azure.</span></span>
- <span data-ttu-id="cdeee-110">Utforska prestandajustering alternativ för en Oracle-databas i en Azure-miljö.</span><span class="sxs-lookup"><span data-stu-id="cdeee-110">Explore performance tuning options for an Oracle database in an Azure environment.</span></span>

## <a name="the-differences-between-an-on-premises-and-azure-implementation"></a><span data-ttu-id="cdeee-111">Skillnader mellan en lokal och Azure-implementering</span><span class="sxs-lookup"><span data-stu-id="cdeee-111">The differences between an on-premises and Azure implementation</span></span> 

<span data-ttu-id="cdeee-112">Följande är några viktiga saker att tänka på när du migrerar lokala program till Azure.</span><span class="sxs-lookup"><span data-stu-id="cdeee-112">Following are some important things to keep in mind when you're migrating on-premises applications to Azure.</span></span> 

<span data-ttu-id="cdeee-113">En viktig skillnad är att resurser som virtuella datorer, diskar och virtuella nätverk i en Azure-implementering måste delas med andra klienter.</span><span class="sxs-lookup"><span data-stu-id="cdeee-113">One important difference is that in an Azure implementation, resources such as VMs, disks, and virtual networks are shared among other clients.</span></span> <span data-ttu-id="cdeee-114">Dessutom kan resurser vara begränsas baserat på kraven.</span><span class="sxs-lookup"><span data-stu-id="cdeee-114">In addition, resources can be throttled based on the requirements.</span></span> <span data-ttu-id="cdeee-115">I stället för att fokusera på att undvika misslyckas (MTBF), fokuserar Azure mer på kvarvarande felet (MTTR).</span><span class="sxs-lookup"><span data-stu-id="cdeee-115">Instead of focusing on avoiding failing (MTBF), Azure is more focused on surviving the failure (MTTR).</span></span>

<span data-ttu-id="cdeee-116">I följande tabell visas några av skillnaderna mellan en lokal implementering och en Azure implementering av en Oracle-databas.</span><span class="sxs-lookup"><span data-stu-id="cdeee-116">The following table lists some of the differences between an on-premises implementation and an Azure implementation of an Oracle database.</span></span>

> 
> |  | <span data-ttu-id="cdeee-117">**Lokal implementering**</span><span class="sxs-lookup"><span data-stu-id="cdeee-117">**On-premises implementation**</span></span> | <span data-ttu-id="cdeee-118">**Azure-implementering**</span><span class="sxs-lookup"><span data-stu-id="cdeee-118">**Azure implementation**</span></span> |
> | --- | --- | --- |
> | <span data-ttu-id="cdeee-119">**Nätverk**</span><span class="sxs-lookup"><span data-stu-id="cdeee-119">**Networking**</span></span> |<span data-ttu-id="cdeee-120">LAN/WAN</span><span class="sxs-lookup"><span data-stu-id="cdeee-120">LAN/WAN</span></span>  |<span data-ttu-id="cdeee-121">SDN (programvarudefinierat nätverk)</span><span class="sxs-lookup"><span data-stu-id="cdeee-121">SDN (software-defined networking)</span></span>|
> | <span data-ttu-id="cdeee-122">**Säkerhetsgrupp**</span><span class="sxs-lookup"><span data-stu-id="cdeee-122">**Security group**</span></span> |<span data-ttu-id="cdeee-123">IP-port begränsning verktyg</span><span class="sxs-lookup"><span data-stu-id="cdeee-123">IP/port restriction tools</span></span> |[<span data-ttu-id="cdeee-124">Nätverkssäkerhetsgrupp (NSG)</span><span class="sxs-lookup"><span data-stu-id="cdeee-124">Network Security Group (NSG)</span></span>](https://azure.microsoft.com/blog/network-security-groups) |
> | <span data-ttu-id="cdeee-125">**Återhämtning**</span><span class="sxs-lookup"><span data-stu-id="cdeee-125">**Resilience**</span></span> |<span data-ttu-id="cdeee-126">MTBF (Genomsnittlig tid mellan fel)</span><span class="sxs-lookup"><span data-stu-id="cdeee-126">MTBF (mean time between failures)</span></span> |<span data-ttu-id="cdeee-127">MTTR (tiden till återställning)</span><span class="sxs-lookup"><span data-stu-id="cdeee-127">MTTR (mean time to recovery)</span></span>|
> | <span data-ttu-id="cdeee-128">**Planerat underhåll**</span><span class="sxs-lookup"><span data-stu-id="cdeee-128">**Planned maintenance**</span></span> |<span data-ttu-id="cdeee-129">Uppdatering/uppgradering</span><span class="sxs-lookup"><span data-stu-id="cdeee-129">Patching/upgrades</span></span>|<span data-ttu-id="cdeee-130">[Tillgänglighetsuppsättningar](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (korrigering/uppgraderingar hanteras av Azure)</span><span class="sxs-lookup"><span data-stu-id="cdeee-130">[Availability sets](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (patching/upgrades managed by Azure)</span></span> |
> | <span data-ttu-id="cdeee-131">**Resurs**</span><span class="sxs-lookup"><span data-stu-id="cdeee-131">**Resource**</span></span> |<span data-ttu-id="cdeee-132">Dedikerad</span><span class="sxs-lookup"><span data-stu-id="cdeee-132">Dedicated</span></span>  |<span data-ttu-id="cdeee-133">Delas med andra klienter</span><span class="sxs-lookup"><span data-stu-id="cdeee-133">Shared with other clients</span></span>|
> | <span data-ttu-id="cdeee-134">**Regioner**</span><span class="sxs-lookup"><span data-stu-id="cdeee-134">**Regions**</span></span> |<span data-ttu-id="cdeee-135">Datacenter</span><span class="sxs-lookup"><span data-stu-id="cdeee-135">Datacenters</span></span> |[<span data-ttu-id="cdeee-136">Region-par</span><span class="sxs-lookup"><span data-stu-id="cdeee-136">Region pairs</span></span>](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability)|
> | <span data-ttu-id="cdeee-137">**Storage**</span><span class="sxs-lookup"><span data-stu-id="cdeee-137">**Storage**</span></span> |<span data-ttu-id="cdeee-138">SAN/fysiska diskar</span><span class="sxs-lookup"><span data-stu-id="cdeee-138">SAN/physical disks</span></span> |[<span data-ttu-id="cdeee-139">Hanteras av Azure storage</span><span class="sxs-lookup"><span data-stu-id="cdeee-139">Azure-managed storage</span></span>](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | <span data-ttu-id="cdeee-140">**Skalning**</span><span class="sxs-lookup"><span data-stu-id="cdeee-140">**Scale**</span></span> |<span data-ttu-id="cdeee-141">Lodrät skala</span><span class="sxs-lookup"><span data-stu-id="cdeee-141">Vertical scale</span></span> |<span data-ttu-id="cdeee-142">Horisontell skalning</span><span class="sxs-lookup"><span data-stu-id="cdeee-142">Horizontal scale</span></span>|


### <a name="requirements"></a><span data-ttu-id="cdeee-143">Krav</span><span class="sxs-lookup"><span data-stu-id="cdeee-143">Requirements</span></span>

- <span data-ttu-id="cdeee-144">Bestämma storlek och tillväxt hastighet för databasen.</span><span class="sxs-lookup"><span data-stu-id="cdeee-144">Determine the database size and growth rate.</span></span>
- <span data-ttu-id="cdeee-145">Fastställ kraven som IOPS, vilket du kan beräkna baserat på Oracle AWR rapporter eller andra verktyg för nätverksövervakning.</span><span class="sxs-lookup"><span data-stu-id="cdeee-145">Determine the IOPS requirements, which you can estimate based on Oracle AWR reports or other network monitoring tools.</span></span>

## <a name="configuration-options"></a><span data-ttu-id="cdeee-146">Konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="cdeee-146">Configuration options</span></span>

<span data-ttu-id="cdeee-147">Det finns fyra möjliga problemområden som du kan finjustera för att förbättra prestanda i en Azure-miljö:</span><span class="sxs-lookup"><span data-stu-id="cdeee-147">There are four potential areas that you can tune to improve performance in an Azure environment:</span></span>

- <span data-ttu-id="cdeee-148">Storlek på virtuell dator</span><span class="sxs-lookup"><span data-stu-id="cdeee-148">Virtual machine size</span></span>
- <span data-ttu-id="cdeee-149">Dataflödet i nätverket</span><span class="sxs-lookup"><span data-stu-id="cdeee-149">Network throughput</span></span>
- <span data-ttu-id="cdeee-150">Disktyper och konfigurationer</span><span class="sxs-lookup"><span data-stu-id="cdeee-150">Disk types and configurations</span></span>
- <span data-ttu-id="cdeee-151">Inställningar för cachelagring av disk</span><span class="sxs-lookup"><span data-stu-id="cdeee-151">Disk cache settings</span></span>

### <a name="generate-an-awr-report"></a><span data-ttu-id="cdeee-152">Generera en rapport för AWR</span><span class="sxs-lookup"><span data-stu-id="cdeee-152">Generate an AWR report</span></span>

<span data-ttu-id="cdeee-153">Om du har en befintlig en Oracle-databas och planerar att migrera till Azure, har du flera alternativ.</span><span class="sxs-lookup"><span data-stu-id="cdeee-153">If you have an existing an Oracle database and are planning to migrate to Azure, you have several options.</span></span> <span data-ttu-id="cdeee-154">Du kan köra rapporten för Oracle AWR att hämta mätvärden (IOPS, Mbit/s, GiBs och så vidare).</span><span class="sxs-lookup"><span data-stu-id="cdeee-154">You can run the Oracle AWR report to get the metrics (IOPS, Mbps, GiBs, and so on).</span></span> <span data-ttu-id="cdeee-155">Välj den virtuella datorn utifrån de mätvärden som samlats in.</span><span class="sxs-lookup"><span data-stu-id="cdeee-155">Then choose the VM based on the metrics that you collected.</span></span> <span data-ttu-id="cdeee-156">Eller så kan du kontakta din infrastruktur-teamet för att få liknande information.</span><span class="sxs-lookup"><span data-stu-id="cdeee-156">Or you can contact your infrastructure team to get similar information.</span></span>

<span data-ttu-id="cdeee-157">Du kan även köra rapporten AWR under både vanliga och högsta antal arbetsbelastningar, så kan du jämföra.</span><span class="sxs-lookup"><span data-stu-id="cdeee-157">You might consider running your AWR report during both regular and peak workloads, so you can compare.</span></span> <span data-ttu-id="cdeee-158">Baserat på dessa rapporter, du kan ändra storlek på de virtuella datorerna baserat på den genomsnittliga arbetsbelastningen eller maximal arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="cdeee-158">Based on these reports, you can size the VMs based on either the average workload or the maximum workload.</span></span>

<span data-ttu-id="cdeee-159">Följande är ett exempel på hur du skapar en rapport över AWR:</span><span class="sxs-lookup"><span data-stu-id="cdeee-159">Following is an example of how to generate an AWR report:</span></span>

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a><span data-ttu-id="cdeee-160">Viktiga mått</span><span class="sxs-lookup"><span data-stu-id="cdeee-160">Key metrics</span></span>

<span data-ttu-id="cdeee-161">Följande är de mätvärden som kan hämtas från AWR rapporten:</span><span class="sxs-lookup"><span data-stu-id="cdeee-161">Following are the metrics that you can obtain from the AWR report:</span></span>

- <span data-ttu-id="cdeee-162">Totalt antal kärnor</span><span class="sxs-lookup"><span data-stu-id="cdeee-162">Total number of cores</span></span>
- <span data-ttu-id="cdeee-163">CPU-klockfrekvens</span><span class="sxs-lookup"><span data-stu-id="cdeee-163">CPU clock speed</span></span>
- <span data-ttu-id="cdeee-164">Totalt minne i GB</span><span class="sxs-lookup"><span data-stu-id="cdeee-164">Total memory in GB</span></span>
- <span data-ttu-id="cdeee-165">CPU-användning</span><span class="sxs-lookup"><span data-stu-id="cdeee-165">CPU utilization</span></span>
- <span data-ttu-id="cdeee-166">Högsta överföringshastighet</span><span class="sxs-lookup"><span data-stu-id="cdeee-166">Peak data transfer rate</span></span>
- <span data-ttu-id="cdeee-167">Antalet i/o-ändringar (läsa/skriva)</span><span class="sxs-lookup"><span data-stu-id="cdeee-167">Rate of I/O changes (read/write)</span></span>
- <span data-ttu-id="cdeee-168">Gör om loggen hastighet (MBPs)</span><span class="sxs-lookup"><span data-stu-id="cdeee-168">Redo log rate (MBPs)</span></span>
- <span data-ttu-id="cdeee-169">Dataflödet i nätverket</span><span class="sxs-lookup"><span data-stu-id="cdeee-169">Network throughput</span></span>
- <span data-ttu-id="cdeee-170">Nätverket latens hastighet (låg/hög)</span><span class="sxs-lookup"><span data-stu-id="cdeee-170">Network latency rate (low/high)</span></span>
- <span data-ttu-id="cdeee-171">Databasens storlek i GB</span><span class="sxs-lookup"><span data-stu-id="cdeee-171">Database size in GB</span></span>
- <span data-ttu-id="cdeee-172">Byte som tagits emot via SQL * Net från/till klienten</span><span class="sxs-lookup"><span data-stu-id="cdeee-172">Bytes received via SQL*Net from/to client</span></span>

### <a name="virtual-machine-size"></a><span data-ttu-id="cdeee-173">Storlek på virtuell dator</span><span class="sxs-lookup"><span data-stu-id="cdeee-173">Virtual machine size</span></span>

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-the-awr-report"></a><span data-ttu-id="cdeee-174">1. VM-storlek för uppskattning baserat på CPU, minne och i/o-användning i rapporten AWR</span><span class="sxs-lookup"><span data-stu-id="cdeee-174">1. Estimate VM size based on CPU, memory, and I/O usage from the AWR report</span></span>

<span data-ttu-id="cdeee-175">En sak som du kan titta på är översta fem tidsinställd förgrunden händelser som indikerar om systemet flaskhalsar är.</span><span class="sxs-lookup"><span data-stu-id="cdeee-175">One thing you might look at is the top five timed foreground events that indicate where the system bottlenecks are.</span></span>

<span data-ttu-id="cdeee-176">I följande diagram visas exempelvis filsynkronisering loggen överst.</span><span class="sxs-lookup"><span data-stu-id="cdeee-176">For example, in the following diagram, the log file sync is at the top.</span></span> <span data-ttu-id="cdeee-177">Anger antalet väntar som krävs innan LGWR skriver Loggningsbufferten till loggfilen gör om.</span><span class="sxs-lookup"><span data-stu-id="cdeee-177">It indicates the number of waits that are required before the LGWR writes the log buffer to the redo log file.</span></span> <span data-ttu-id="cdeee-178">Dessa resultat indikerar att bättre prestanda lagring eller diskar är nödvändiga.</span><span class="sxs-lookup"><span data-stu-id="cdeee-178">These results indicate that better performing storage or disks are required.</span></span> <span data-ttu-id="cdeee-179">Diagrammet visar dessutom också antalet CPU (kärnor) och mängden minne.</span><span class="sxs-lookup"><span data-stu-id="cdeee-179">In addition, the diagram also shows the number of CPU (cores) and the amount of memory.</span></span>

![Skärmbild av sidan AWR](./media/oracle-design/cpu_memory_info.png)

<span data-ttu-id="cdeee-181">Följande diagram visar den totala i/o för läsning och skrivning.</span><span class="sxs-lookup"><span data-stu-id="cdeee-181">The following diagram shows the total I/O of read and write.</span></span> <span data-ttu-id="cdeee-182">Det fanns 59 läsa och 247.3 GB skrivs vid tidpunkten för rapporten.</span><span class="sxs-lookup"><span data-stu-id="cdeee-182">There were 59 GB read and 247.3 GB written during the time of the report.</span></span>

![Skärmbild av sidan AWR](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a><span data-ttu-id="cdeee-184">2. Välj en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="cdeee-184">2. Choose a VM</span></span>

<span data-ttu-id="cdeee-185">Baserat på information som du samlade in i rapporten AWR är nästa steg att välja en virtuell dator med samma storlek som uppfyller dina krav.</span><span class="sxs-lookup"><span data-stu-id="cdeee-185">Based on the information that you collected from the AWR report, the next step is to choose a VM of a similar size that meets your requirements.</span></span> <span data-ttu-id="cdeee-186">Du hittar en lista med tillgängliga virtuella datorer i artikeln [Minnesoptimerade](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).</span><span class="sxs-lookup"><span data-stu-id="cdeee-186">You can find a list of available VMs in the article [Memory optimized](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).</span></span>

#### <a name="3-fine-tune-the-vm-sizing-with-a-similar-vm-series-based-on-the-acu"></a><span data-ttu-id="cdeee-187">3. Finjustera VM-storlek med liknande VM baserat på ACU</span><span class="sxs-lookup"><span data-stu-id="cdeee-187">3. Fine-tune the VM sizing with a similar VM series based on the ACU</span></span>

<span data-ttu-id="cdeee-188">När du har valt den virtuella datorn, kan du vara uppmärksam på ACU för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="cdeee-188">After you've chosen the VM, pay attention to the ACU for the VM.</span></span> <span data-ttu-id="cdeee-189">Du kan välja en annan virtuell dator baserat på värdet ACU som bättre passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="cdeee-189">You might choose a different VM based on the ACU value that better suits your requirements.</span></span> <span data-ttu-id="cdeee-190">Mer information finns i [Azure compute enhet](https://docs.microsoft.com/azure/virtual-machines/windows/acu).</span><span class="sxs-lookup"><span data-stu-id="cdeee-190">For more information, see [Azure compute unit](https://docs.microsoft.com/azure/virtual-machines/windows/acu).</span></span>

![Skärmbild av sidan ACU enheter](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a><span data-ttu-id="cdeee-192">Dataflödet i nätverket</span><span class="sxs-lookup"><span data-stu-id="cdeee-192">Network throughput</span></span>

<span data-ttu-id="cdeee-193">Följande diagram visar relationen mellan genomflöde och IOPS:</span><span class="sxs-lookup"><span data-stu-id="cdeee-193">The following diagram shows the relation between throughput and IOPS:</span></span>

![Skärmbild av dataflöde](./media/oracle-design/throughput.png)

<span data-ttu-id="cdeee-195">Det totala genomflödet beräknas baserat på följande information:</span><span class="sxs-lookup"><span data-stu-id="cdeee-195">The total network throughput is estimated based on the following information:</span></span>
- <span data-ttu-id="cdeee-196">SQL * Net trafik</span><span class="sxs-lookup"><span data-stu-id="cdeee-196">SQL*Net traffic</span></span>
- <span data-ttu-id="cdeee-197">Mbit/s x antal servrar (till exempel Oracle Data Guard utgående ström)</span><span class="sxs-lookup"><span data-stu-id="cdeee-197">MBps x number of servers (outbound stream such as Oracle Data Guard)</span></span>
- <span data-ttu-id="cdeee-198">Andra faktorer, till exempel program replikering</span><span class="sxs-lookup"><span data-stu-id="cdeee-198">Other factors, such as application replication</span></span>

![Skärmbild av SQL * Net genomflöde](./media/oracle-design/sqlnet_info.png)

<span data-ttu-id="cdeee-200">Baserat på din kraven på nätverksbandbredd finns olika typer av gateway som du kan välja från.</span><span class="sxs-lookup"><span data-stu-id="cdeee-200">Based on your network bandwidth requirements, there are various gateway types for you to choose from.</span></span> <span data-ttu-id="cdeee-201">Dessa inkluderar basic VpnGw och Azure ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="cdeee-201">These include basic, VpnGw, and Azure ExpressRoute.</span></span> <span data-ttu-id="cdeee-202">Mer information finns i [VPN-gateway sida med priser](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).</span><span class="sxs-lookup"><span data-stu-id="cdeee-202">For more information, see the [VPN gateway pricing page](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).</span></span>

<span data-ttu-id="cdeee-203">**Rekommendationer**</span><span class="sxs-lookup"><span data-stu-id="cdeee-203">**Recommendations**</span></span>

- <span data-ttu-id="cdeee-204">Nätverksfördröjningen är högre jämfört med en lokal distribution.</span><span class="sxs-lookup"><span data-stu-id="cdeee-204">Network latency is higher compared to an on-premises deployment.</span></span> <span data-ttu-id="cdeee-205">Minskar nätverket avrunda resor kan kraftigt förbättra prestanda.</span><span class="sxs-lookup"><span data-stu-id="cdeee-205">Reducing network round trips can greatly improve performance.</span></span>
- <span data-ttu-id="cdeee-206">Om du vill minska turer konsolidera program som har hög transaktioner eller ”chatty” appar i samma virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="cdeee-206">To reduce round-trips, consolidate applications that have high transactions or “chatty” apps on the same virtual machine.</span></span>

### <a name="disk-types-and-configurations"></a><span data-ttu-id="cdeee-207">Disktyper och konfigurationer</span><span class="sxs-lookup"><span data-stu-id="cdeee-207">Disk types and configurations</span></span>

- <span data-ttu-id="cdeee-208">*OS-diskar som standard*: dessa disktyper erbjuder beständiga data och cachelagring.</span><span class="sxs-lookup"><span data-stu-id="cdeee-208">*Default OS disks*: These disk types offer persistent data and caching.</span></span> <span data-ttu-id="cdeee-209">De är optimerade för OS åtkomst vid start och inte har utformats för antingen transaktionella eller datalager (Analytiska) arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="cdeee-209">They are optimized for OS access at startup, and aren't designed for either transactional or data warehouse (analytical) workloads.</span></span>

- <span data-ttu-id="cdeee-210">*Ohanterad diskar*: dessa disktyper du hantera de lagringskonton som lagra filer för virtuell hårddisk (VHD) som motsvarar dina VM-diskar.</span><span class="sxs-lookup"><span data-stu-id="cdeee-210">*Unmanaged disks*: With these disk types, you manage the storage accounts that store the virtual hard disk (VHD) files that correspond to your VM disks.</span></span> <span data-ttu-id="cdeee-211">VHD-filer lagras som sidblobbar i Azure storage-konton.</span><span class="sxs-lookup"><span data-stu-id="cdeee-211">VHD files are stored as page blobs in Azure storage accounts.</span></span>

- <span data-ttu-id="cdeee-212">*Hanterade diskar*: hanteras av Azure storage-konton som du använder för din Virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="cdeee-212">*Managed disks*: Azure manages the storage accounts that you use for your VM disks.</span></span> <span data-ttu-id="cdeee-213">Anger typ av disk (premium eller standard) och storleken på den disk som du behöver.</span><span class="sxs-lookup"><span data-stu-id="cdeee-213">You specify the disk type (premium or standard) and the size of the disk that you need.</span></span> <span data-ttu-id="cdeee-214">Azure skapar och hanterar disken åt dig.</span><span class="sxs-lookup"><span data-stu-id="cdeee-214">Azure creates and manages the disk for you.</span></span>

- <span data-ttu-id="cdeee-215">*Premium-lagringsdiskar*: dessa disktyper som passar bäst för produktionsarbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="cdeee-215">*Premium storage disks*: These disk types are best suited for production workloads.</span></span> <span data-ttu-id="cdeee-216">Premium-lagring stöder Virtuella diskar som kan kopplas till specifika storlek-serien virtuella datorer, till exempel DS, DSv2, GS och F serien virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="cdeee-216">Premium storage supports VM disks that can be attached to specific size-series VMs, such as DS, DSv2, GS, and F series VMs.</span></span> <span data-ttu-id="cdeee-217">Premium-disk som levereras med olika storlekar och du kan välja mellan diskar mellan 32 GB och 4 096 GB.</span><span class="sxs-lookup"><span data-stu-id="cdeee-217">The premium disk comes with different sizes, and you can choose between disks ranging from 32 GB to 4,096 GB.</span></span> <span data-ttu-id="cdeee-218">Varje diskstorleken har sin egen prestandakrav.</span><span class="sxs-lookup"><span data-stu-id="cdeee-218">Each disk size has its own performance specifications.</span></span> <span data-ttu-id="cdeee-219">Beroende på kraven för application kan du koppla en eller flera diskar till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="cdeee-219">Depending on your application requirements, you can attach one or more disks to your VM.</span></span>

<span data-ttu-id="cdeee-220">När du skapar en ny hanterade disk från portalen kan du välja den **kontotyp** för typ av disk du vill använda.</span><span class="sxs-lookup"><span data-stu-id="cdeee-220">When you create a new managed disk from the portal, you can choose the **Account type** for the type of disk you want to use.</span></span> <span data-ttu-id="cdeee-221">Tänk på att inte alla tillgängliga diskar visas i den nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="cdeee-221">Keep in mind that not all available disks are shown in the drop-down menu.</span></span> <span data-ttu-id="cdeee-222">När du har valt en viss VM-storlek visas på menyn endast tillgängliga premium-lagring SKU: er som är baserade på att VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="cdeee-222">After you choose a particular VM size, the menu shows only the available premium storage SKUs that are based on that VM size.</span></span>

![Skärmbild av sidan hanterade diskar](./media/oracle-design/premium_disk01.png)

<span data-ttu-id="cdeee-224">Mer information finns i [Premium-lagring med hög prestanda och hanterade diskar för virtuella datorer](https://docs.microsoft.com/azure/storage/storage-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="cdeee-224">For more information, see [High-performance Premium Storage and managed disks for VMs](https://docs.microsoft.com/azure/storage/storage-premium-storage).</span></span>

<span data-ttu-id="cdeee-225">När du har konfigurerat din lagring på en virtuell dator kanske du vill läsa in testa diskar innan du skapar en databas.</span><span class="sxs-lookup"><span data-stu-id="cdeee-225">After you configure your storage on a VM, you might want to load test the disks before creating a database.</span></span> <span data-ttu-id="cdeee-226">Veta i/o-hastighet på både svarstid och genomströmning kan hjälpa dig att avgöra om de virtuella datorerna har stöd för det förväntade genomflödet med latens mål.</span><span class="sxs-lookup"><span data-stu-id="cdeee-226">Knowing the I/O rate in terms of both latency and throughput can help you determine if the VMs support the expected throughput with latency targets.</span></span>

<span data-ttu-id="cdeee-227">Det finns ett antal verktyg programtest belastning, till exempel Oracle Orion, Sysbench och Fio.</span><span class="sxs-lookup"><span data-stu-id="cdeee-227">There are a number of tools for application load testing, such as Oracle Orion, Sysbench, and Fio.</span></span>

<span data-ttu-id="cdeee-228">Kör testet Läs in igen när du har distribuerat en Oracle-databas.</span><span class="sxs-lookup"><span data-stu-id="cdeee-228">Run the load test again after you've deployed an Oracle database.</span></span> <span data-ttu-id="cdeee-229">Starta din regelbundet och högsta antal arbetsbelastningar och resultaten visar baslinje för din miljö.</span><span class="sxs-lookup"><span data-stu-id="cdeee-229">Start your regular and peak workloads, and the results show you the baseline of your environment.</span></span>

<span data-ttu-id="cdeee-230">Det kan vara viktigare storleken baseras på IOPS-hastighet i stället för lagringsstorleken lagringen.</span><span class="sxs-lookup"><span data-stu-id="cdeee-230">It might be more important to size the storage based on the IOPS rate rather than the storage size.</span></span> <span data-ttu-id="cdeee-231">Till exempel om det obligatoriska IOPs-värdet är 5 000, men du behöver bara 200 GB, kan du fortfarande få P30 klassen premium disken även om den innehåller fler än 200 GB lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="cdeee-231">For example, if the required IOPS is 5,000, but you only need 200 GB, you might still get the P30 class premium disk even though it comes with more than 200 GB of storage.</span></span>

<span data-ttu-id="cdeee-232">Hastigheten med IOPS kan hämtas från AWR rapporten.</span><span class="sxs-lookup"><span data-stu-id="cdeee-232">The IOPS rate can be obtained from the AWR report.</span></span> <span data-ttu-id="cdeee-233">Det bestäms av gör om-loggen, fysiska läsningar och skrivningar hastighet.</span><span class="sxs-lookup"><span data-stu-id="cdeee-233">It's determined by the redo log, physical reads, and writes rate.</span></span>

![Skärmbild av sidan AWR](./media/oracle-design/awr_report.png)

<span data-ttu-id="cdeee-235">Gör om storleken är till exempel 12,200,000 byte per sekund som är lika med 11.63 Mbit/s.</span><span class="sxs-lookup"><span data-stu-id="cdeee-235">For example, the redo size is 12,200,000 bytes per second, which is equal to 11.63 MBPs.</span></span>
<span data-ttu-id="cdeee-236">IOPS är 12 200 000 / 2,358 = 5,174.</span><span class="sxs-lookup"><span data-stu-id="cdeee-236">The IOPS is 12,200,000 / 2,358 = 5,174.</span></span>

<span data-ttu-id="cdeee-237">När du har en tydlig bild av i/o-kraven kan välja du en kombination av enheter som är bäst att uppfylla kraven.</span><span class="sxs-lookup"><span data-stu-id="cdeee-237">After you have a clear picture of the I/O requirements, you can choose a combination of drives that are best suited to meet those requirements.</span></span>

<span data-ttu-id="cdeee-238">**Rekommendationer**</span><span class="sxs-lookup"><span data-stu-id="cdeee-238">**Recommendations**</span></span>

- <span data-ttu-id="cdeee-239">För data fördelade tabellutrymmet, i/o-arbetsbelastning på ett antal diskar med hjälp av hanterade lagringspoolen eller Oracle ASM.</span><span class="sxs-lookup"><span data-stu-id="cdeee-239">For data tablespace, spread the I/O workload across a number of disks by using managed storage or Oracle ASM.</span></span>
- <span data-ttu-id="cdeee-240">I/o-blockstorlek ökar för läsintensiva- och skrivåtgärder-intensiva, lägga till flera datadiskar.</span><span class="sxs-lookup"><span data-stu-id="cdeee-240">As the I/O block size increases for read-intensive and write-intensive operations, add more data disks.</span></span>
- <span data-ttu-id="cdeee-241">Öka blockstorleken för stora sekventiella processer.</span><span class="sxs-lookup"><span data-stu-id="cdeee-241">Increase the block size for large sequential processes.</span></span>
- <span data-ttu-id="cdeee-242">Du kan använda komprimering för att minska i/o (för både data och index).</span><span class="sxs-lookup"><span data-stu-id="cdeee-242">Use data compression to reduce I/O (for both data and indexes).</span></span>
- <span data-ttu-id="cdeee-243">Avgränsa gör om loggfiler, system och temps och ångra TS på separata hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="cdeee-243">Separate redo logs, system, and temps, and undo TS on separate data disks.</span></span>
- <span data-ttu-id="cdeee-244">Placera inte några programfiler på standard OS-diskar (/ dev/sda).</span><span class="sxs-lookup"><span data-stu-id="cdeee-244">Don't put any application files on default OS disks (/dev/sda).</span></span> <span data-ttu-id="cdeee-245">Diskarna inte är optimerad för snabb VM Start gånger, och de kan inte ange goda prestanda för ditt program.</span><span class="sxs-lookup"><span data-stu-id="cdeee-245">These disks aren't optimized for fast VM boot times, and they might not provide good performance for your application.</span></span>

### <a name="disk-cache-settings"></a><span data-ttu-id="cdeee-246">Inställningar för cachelagring av disk</span><span class="sxs-lookup"><span data-stu-id="cdeee-246">Disk cache settings</span></span>

<span data-ttu-id="cdeee-247">Det finns tre alternativ för värdcachelagring:</span><span class="sxs-lookup"><span data-stu-id="cdeee-247">There are three options for host caching:</span></span>

- <span data-ttu-id="cdeee-248">*Skrivskyddad*: alla begäranden cachelagras för framtida läsningar.</span><span class="sxs-lookup"><span data-stu-id="cdeee-248">*Read-only*: All requests are cached for future reads.</span></span> <span data-ttu-id="cdeee-249">Alla skrivningar beständiga direkt till Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="cdeee-249">All writes are persisted directly to Azure Blob storage.</span></span>

- <span data-ttu-id="cdeee-250">*Skrivskyddad*: Detta är en ”read-ahead” algoritm.</span><span class="sxs-lookup"><span data-stu-id="cdeee-250">*Read-write*: This is a “read-ahead” algorithm.</span></span> <span data-ttu-id="cdeee-251">Läsning och skrivning cachelagras för framtida läsningar.</span><span class="sxs-lookup"><span data-stu-id="cdeee-251">The reads and writes are cached for future reads.</span></span> <span data-ttu-id="cdeee-252">Icke skrivning via skrivningar beständiga först till den lokala cachen.</span><span class="sxs-lookup"><span data-stu-id="cdeee-252">Non-write-through writes are persisted to the local cache first.</span></span> <span data-ttu-id="cdeee-253">För SQL Server sparas skrivningar till Azure Storage eftersom den använder skrivning via.</span><span class="sxs-lookup"><span data-stu-id="cdeee-253">For SQL Server, writes are persisted to Azure Storage because it uses write-through.</span></span> <span data-ttu-id="cdeee-254">Det ger också den lägsta fördröjningen för disken för enstaka arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="cdeee-254">It also provides the lowest disk latency for light workloads.</span></span>

- <span data-ttu-id="cdeee-255">*Ingen* (inaktiverat): med det här alternativet kan du kringgå cachen.</span><span class="sxs-lookup"><span data-stu-id="cdeee-255">*None* (disabled): By using this option, you can bypass the cache.</span></span> <span data-ttu-id="cdeee-256">Alla data som överförs till disk och beständiga till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="cdeee-256">All the data is transferred to disk and persisted to Azure Storage.</span></span> <span data-ttu-id="cdeee-257">Den här metoden ger den högsta i/o-hastigheten för i/o-intensiv arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="cdeee-257">This method gives you the highest I/O rate for I/O intensive workloads.</span></span> <span data-ttu-id="cdeee-258">Du måste också beakta ”transaktionen kostnad”.</span><span class="sxs-lookup"><span data-stu-id="cdeee-258">You also need to take “transaction cost” into consideration.</span></span>

<span data-ttu-id="cdeee-259">**Rekommendationer**</span><span class="sxs-lookup"><span data-stu-id="cdeee-259">**Recommendations**</span></span>

<span data-ttu-id="cdeee-260">För att maximera genomströmningen, rekommenderar vi att du börjar med **ingen** för cachelagring av värden.</span><span class="sxs-lookup"><span data-stu-id="cdeee-260">To maximize the throughput, we recommend that you  start with **None** for host caching.</span></span> <span data-ttu-id="cdeee-261">För Premium-lagring, Tänk på att du inaktiverar ”barriärer” när du monterar filsystemet med den **ReadOnly** eller **ingen** alternativ.</span><span class="sxs-lookup"><span data-stu-id="cdeee-261">For Premium Storage, keep in mind that you must disable the "barriers" when you mount the file system with the **ReadOnly** or **None** options.</span></span> <span data-ttu-id="cdeee-262">Uppdatera filen /etc/fstab med UUID till diskarna.</span><span class="sxs-lookup"><span data-stu-id="cdeee-262">Update the /etc/fstab file with the UUID to the disks.</span></span>

<span data-ttu-id="cdeee-263">Mer information finns i [Premium-lagring för virtuella Linux-datorer](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).</span><span class="sxs-lookup"><span data-stu-id="cdeee-263">For more information, see [Premium Storage for Linux VMs](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).</span></span>

![Skärmbild av sidan hanterade diskar](./media/oracle-design/premium_disk02.png)

- <span data-ttu-id="cdeee-265">Använd standard för OS diskar **läsning och skrivning** cachelagring.</span><span class="sxs-lookup"><span data-stu-id="cdeee-265">For OS disks, use default **Read/Write** caching.</span></span>
- <span data-ttu-id="cdeee-266">Använd för SYSTEM, TEMP och ångra **ingen** för cachelagring.</span><span class="sxs-lookup"><span data-stu-id="cdeee-266">For SYSTEM, TEMP, and UNDO use **None** for caching.</span></span>
- <span data-ttu-id="cdeee-267">DATA, Använd **ingen** för cachelagring.</span><span class="sxs-lookup"><span data-stu-id="cdeee-267">For DATA, use **None** for caching.</span></span> <span data-ttu-id="cdeee-268">Men om databasen är skrivskyddad eller läsintensiva använder **skrivskyddad** cachelagring.</span><span class="sxs-lookup"><span data-stu-id="cdeee-268">But if your database is read-only or read-intensive, use **Read-only** caching.</span></span>

<span data-ttu-id="cdeee-269">När inställningen disk data sparas, kan du inte ändra värden cache-inställningen om du inte demontera enheten på OS-nivån och sedan montera när du har gjort ändringen.</span><span class="sxs-lookup"><span data-stu-id="cdeee-269">After your data disk setting is saved, you can't change the host cache setting unless you unmount the drive at the OS level and then remount it after you've made the change.</span></span>


## <a name="security"></a><span data-ttu-id="cdeee-270">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="cdeee-270">Security</span></span>

<span data-ttu-id="cdeee-271">När du har skapat och konfigurerat Azure-miljön, är nästa steg att skydda nätverket.</span><span class="sxs-lookup"><span data-stu-id="cdeee-271">After you have set up and configured your Azure environment, the next step is to secure your network.</span></span> <span data-ttu-id="cdeee-272">Här följer några rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cdeee-272">Here are some recommendations:</span></span>

- <span data-ttu-id="cdeee-273">*NSG princip*: NSG kan definieras av ett undernät eller nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="cdeee-273">*NSG policy*: NSG can be defined by a subnet or NIC.</span></span> <span data-ttu-id="cdeee-274">Det är enklare att styra åtkomsten på undernätverksnivån både för säkerhet och kraft routning för till exempel brandväggar för programmet.</span><span class="sxs-lookup"><span data-stu-id="cdeee-274">It's simpler to control access at the subnet level both for security and force routing for things like application firewalls.</span></span>

- <span data-ttu-id="cdeee-275">*Jumpbox*: för säkrare åtkomst administratörer bör inte ansluter direkt till programtjänsten eller databasen.</span><span class="sxs-lookup"><span data-stu-id="cdeee-275">*Jumpbox*: For more secure access, administrators should not directly connect to the application service or database.</span></span> <span data-ttu-id="cdeee-276">En jumpbox används som en media mellan administratören dator och Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="cdeee-276">A jumpbox is used as a media between the administrator machine and Azure resources.</span></span>
<span data-ttu-id="cdeee-277">![Skärmbild av sidan Jumpbox-topologi](./media/oracle-design/jumpbox.png)</span><span class="sxs-lookup"><span data-stu-id="cdeee-277">![Screenshot of the Jumpbox topology page](./media/oracle-design/jumpbox.png)</span></span>

    <span data-ttu-id="cdeee-278">Administratören datorn bör erbjuda IP tillgång till jumpbox endast.</span><span class="sxs-lookup"><span data-stu-id="cdeee-278">The administrator machine should offer IP-restricted access to the jumpbox only.</span></span> <span data-ttu-id="cdeee-279">Jumpbox ska ha åtkomst till programmet och databasen.</span><span class="sxs-lookup"><span data-stu-id="cdeee-279">The jumpbox should have access to the application and database.</span></span>

- <span data-ttu-id="cdeee-280">*Privat nätverk* (undernät): Vi rekommenderar att du har programtjänsten och databasen på olika undernät, så kan ställa in bättre kontroll av NSG principen.</span><span class="sxs-lookup"><span data-stu-id="cdeee-280">*Private network* (subnets): We recommend that you have the application service and database on separate subnets, so better control can be set by NSG policy.</span></span>


## <a name="additional-reading"></a><span data-ttu-id="cdeee-281">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cdeee-281">Additional reading</span></span>

- [<span data-ttu-id="cdeee-282">Konfigurera Oracle ASM</span><span class="sxs-lookup"><span data-stu-id="cdeee-282">Configure Oracle ASM</span></span>](configure-oracle-asm.md)
- [<span data-ttu-id="cdeee-283">Konfigurera Oracle Data Guard</span><span class="sxs-lookup"><span data-stu-id="cdeee-283">Configure Oracle Data Guard</span></span>](configure-oracle-dataguard.md)
- [<span data-ttu-id="cdeee-284">Konfigurera Oracle guld Gate</span><span class="sxs-lookup"><span data-stu-id="cdeee-284">Configure Oracle Golden Gate</span></span>](configure-oracle-golden-gate.md)
- [<span data-ttu-id="cdeee-285">Oracle-säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="cdeee-285">Oracle backup and recovery</span></span>](oracle-backup-recovery.md)

## <a name="next-steps"></a><span data-ttu-id="cdeee-286">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cdeee-286">Next steps</span></span>

- [<span data-ttu-id="cdeee-287">Självstudier: Skapa högtillgängliga virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="cdeee-287">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)
- [<span data-ttu-id="cdeee-288">Utforska VM distribution Azure CLI-exempel</span><span class="sxs-lookup"><span data-stu-id="cdeee-288">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
