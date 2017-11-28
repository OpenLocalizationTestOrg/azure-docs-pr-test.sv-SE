---
title: "aaaDesign och implementera en Oracle-databas på Azure | Microsoft Docs"
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
ms.openlocfilehash: 8fa1207458695df1c7330ec626888b1b6b8d8939
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a><span data-ttu-id="c39b5-103">Utforma och implementera en Oracle-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="c39b5-103">Design and implement an Oracle database in Azure</span></span>

## <a name="assumptions"></a><span data-ttu-id="c39b5-104">Antaganden</span><span class="sxs-lookup"><span data-stu-id="c39b5-104">Assumptions</span></span>

- <span data-ttu-id="c39b5-105">Du planerar toomigrate en Oracle-databas från lokala tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c39b5-105">You're planning toomigrate an Oracle database from on-premises tooAzure.</span></span>
- <span data-ttu-id="c39b5-106">Du har en förståelse av hello olika mått i Oracle AWR rapporter.</span><span class="sxs-lookup"><span data-stu-id="c39b5-106">You have an understanding of hello various metrics in Oracle AWR reports.</span></span>
- <span data-ttu-id="c39b5-107">Du har en grundläggande förståelse för programmets prestanda och användning av plattform.</span><span class="sxs-lookup"><span data-stu-id="c39b5-107">You have a baseline understanding of application performance and platform utilization.</span></span>

## <a name="goals"></a><span data-ttu-id="c39b5-108">Mål</span><span class="sxs-lookup"><span data-stu-id="c39b5-108">Goals</span></span>

- <span data-ttu-id="c39b5-109">Förstå hur toooptimize Oracle distributionen i Azure.</span><span class="sxs-lookup"><span data-stu-id="c39b5-109">Understand how toooptimize your Oracle deployment in Azure.</span></span>
- <span data-ttu-id="c39b5-110">Utforska prestandajustering alternativ för en Oracle-databas i en Azure-miljö.</span><span class="sxs-lookup"><span data-stu-id="c39b5-110">Explore performance tuning options for an Oracle database in an Azure environment.</span></span>

## <a name="hello-differences-between-an-on-premises-and-azure-implementation"></a><span data-ttu-id="c39b5-111">Hej skillnaderna mellan en lokal och Azure-implementering</span><span class="sxs-lookup"><span data-stu-id="c39b5-111">hello differences between an on-premises and Azure implementation</span></span> 

<span data-ttu-id="c39b5-112">Följande är exempel på viktiga saker tookeep i åtanke när du migrerar lokala program tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c39b5-112">Following are some important things tookeep in mind when you're migrating on-premises applications tooAzure.</span></span> 

<span data-ttu-id="c39b5-113">En viktig skillnad är att resurser som virtuella datorer, diskar och virtuella nätverk i en Azure-implementering måste delas med andra klienter.</span><span class="sxs-lookup"><span data-stu-id="c39b5-113">One important difference is that in an Azure implementation, resources such as VMs, disks, and virtual networks are shared among other clients.</span></span> <span data-ttu-id="c39b5-114">Dessutom kan resurser vara begränsas baserat på hello krav.</span><span class="sxs-lookup"><span data-stu-id="c39b5-114">In addition, resources can be throttled based on hello requirements.</span></span> <span data-ttu-id="c39b5-115">I stället för att fokusera på att undvika misslyckas (MTBF), fokuserar Azure mer på kvarvarande hello-fel (MTTR).</span><span class="sxs-lookup"><span data-stu-id="c39b5-115">Instead of focusing on avoiding failing (MTBF), Azure is more focused on surviving hello failure (MTTR).</span></span>

<span data-ttu-id="c39b5-116">hello följande tabell visas några av hello skillnader mellan en lokal implementering och en Azure implementering av en Oracle-databas.</span><span class="sxs-lookup"><span data-stu-id="c39b5-116">hello following table lists some of hello differences between an on-premises implementation and an Azure implementation of an Oracle database.</span></span>

> 
> |  | <span data-ttu-id="c39b5-117">**Lokal implementering**</span><span class="sxs-lookup"><span data-stu-id="c39b5-117">**On-premises implementation**</span></span> | <span data-ttu-id="c39b5-118">**Azure-implementering**</span><span class="sxs-lookup"><span data-stu-id="c39b5-118">**Azure implementation**</span></span> |
> | --- | --- | --- |
> | <span data-ttu-id="c39b5-119">**Nätverk**</span><span class="sxs-lookup"><span data-stu-id="c39b5-119">**Networking**</span></span> |<span data-ttu-id="c39b5-120">LAN/WAN</span><span class="sxs-lookup"><span data-stu-id="c39b5-120">LAN/WAN</span></span>  |<span data-ttu-id="c39b5-121">SDN (programvarudefinierat nätverk)</span><span class="sxs-lookup"><span data-stu-id="c39b5-121">SDN (software-defined networking)</span></span>|
> | <span data-ttu-id="c39b5-122">**Säkerhetsgrupp**</span><span class="sxs-lookup"><span data-stu-id="c39b5-122">**Security group**</span></span> |<span data-ttu-id="c39b5-123">IP-port begränsning verktyg</span><span class="sxs-lookup"><span data-stu-id="c39b5-123">IP/port restriction tools</span></span> |[<span data-ttu-id="c39b5-124">Nätverkssäkerhetsgrupp (NSG)</span><span class="sxs-lookup"><span data-stu-id="c39b5-124">Network Security Group (NSG)</span></span>](https://azure.microsoft.com/blog/network-security-groups) |
> | <span data-ttu-id="c39b5-125">**Återhämtning**</span><span class="sxs-lookup"><span data-stu-id="c39b5-125">**Resilience**</span></span> |<span data-ttu-id="c39b5-126">MTBF (Genomsnittlig tid mellan fel)</span><span class="sxs-lookup"><span data-stu-id="c39b5-126">MTBF (mean time between failures)</span></span> |<span data-ttu-id="c39b5-127">MTTR (tiden toorecovery)</span><span class="sxs-lookup"><span data-stu-id="c39b5-127">MTTR (mean time toorecovery)</span></span>|
> | <span data-ttu-id="c39b5-128">**Planerat underhåll**</span><span class="sxs-lookup"><span data-stu-id="c39b5-128">**Planned maintenance**</span></span> |<span data-ttu-id="c39b5-129">Uppdatering/uppgradering</span><span class="sxs-lookup"><span data-stu-id="c39b5-129">Patching/upgrades</span></span>|<span data-ttu-id="c39b5-130">[Tillgänglighetsuppsättningar](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (korrigering/uppgraderingar hanteras av Azure)</span><span class="sxs-lookup"><span data-stu-id="c39b5-130">[Availability sets](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (patching/upgrades managed by Azure)</span></span> |
> | <span data-ttu-id="c39b5-131">**Resurs**</span><span class="sxs-lookup"><span data-stu-id="c39b5-131">**Resource**</span></span> |<span data-ttu-id="c39b5-132">Dedikerad</span><span class="sxs-lookup"><span data-stu-id="c39b5-132">Dedicated</span></span>  |<span data-ttu-id="c39b5-133">Delas med andra klienter</span><span class="sxs-lookup"><span data-stu-id="c39b5-133">Shared with other clients</span></span>|
> | <span data-ttu-id="c39b5-134">**Regioner**</span><span class="sxs-lookup"><span data-stu-id="c39b5-134">**Regions**</span></span> |<span data-ttu-id="c39b5-135">Datacenter</span><span class="sxs-lookup"><span data-stu-id="c39b5-135">Datacenters</span></span> |[<span data-ttu-id="c39b5-136">Region-par</span><span class="sxs-lookup"><span data-stu-id="c39b5-136">Region pairs</span></span>](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability)|
> | <span data-ttu-id="c39b5-137">**Storage**</span><span class="sxs-lookup"><span data-stu-id="c39b5-137">**Storage**</span></span> |<span data-ttu-id="c39b5-138">SAN/fysiska diskar</span><span class="sxs-lookup"><span data-stu-id="c39b5-138">SAN/physical disks</span></span> |[<span data-ttu-id="c39b5-139">Hanteras av Azure storage</span><span class="sxs-lookup"><span data-stu-id="c39b5-139">Azure-managed storage</span></span>](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | <span data-ttu-id="c39b5-140">**Skalning**</span><span class="sxs-lookup"><span data-stu-id="c39b5-140">**Scale**</span></span> |<span data-ttu-id="c39b5-141">Lodrät skala</span><span class="sxs-lookup"><span data-stu-id="c39b5-141">Vertical scale</span></span> |<span data-ttu-id="c39b5-142">Horisontell skalning</span><span class="sxs-lookup"><span data-stu-id="c39b5-142">Horizontal scale</span></span>|


### <a name="requirements"></a><span data-ttu-id="c39b5-143">Krav</span><span class="sxs-lookup"><span data-stu-id="c39b5-143">Requirements</span></span>

- <span data-ttu-id="c39b5-144">Bestämma hello databasen storlek och tillväxt.</span><span class="sxs-lookup"><span data-stu-id="c39b5-144">Determine hello database size and growth rate.</span></span>
- <span data-ttu-id="c39b5-145">Fastställa hello IOPS krav som du kan beräkna baserat på Oracle AWR rapporter eller andra verktyg för nätverksövervakning.</span><span class="sxs-lookup"><span data-stu-id="c39b5-145">Determine hello IOPS requirements, which you can estimate based on Oracle AWR reports or other network monitoring tools.</span></span>

## <a name="configuration-options"></a><span data-ttu-id="c39b5-146">Konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="c39b5-146">Configuration options</span></span>

<span data-ttu-id="c39b5-147">Det finns fyra möjliga problemområden att du kan finjustera tooimprove prestanda i en Azure-miljö:</span><span class="sxs-lookup"><span data-stu-id="c39b5-147">There are four potential areas that you can tune tooimprove performance in an Azure environment:</span></span>

- <span data-ttu-id="c39b5-148">Storlek på virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c39b5-148">Virtual machine size</span></span>
- <span data-ttu-id="c39b5-149">Dataflödet i nätverket</span><span class="sxs-lookup"><span data-stu-id="c39b5-149">Network throughput</span></span>
- <span data-ttu-id="c39b5-150">Disktyper och konfigurationer</span><span class="sxs-lookup"><span data-stu-id="c39b5-150">Disk types and configurations</span></span>
- <span data-ttu-id="c39b5-151">Inställningar för cachelagring av disk</span><span class="sxs-lookup"><span data-stu-id="c39b5-151">Disk cache settings</span></span>

### <a name="generate-an-awr-report"></a><span data-ttu-id="c39b5-152">Generera en rapport för AWR</span><span class="sxs-lookup"><span data-stu-id="c39b5-152">Generate an AWR report</span></span>

<span data-ttu-id="c39b5-153">Om du har en befintlig en Oracle-databas och planerar toomigrate tooAzure, har du flera alternativ.</span><span class="sxs-lookup"><span data-stu-id="c39b5-153">If you have an existing an Oracle database and are planning toomigrate tooAzure, you have several options.</span></span> <span data-ttu-id="c39b5-154">Du kan köra hello Oracle AWR rapporten tooget hello mått (IOPS, Mbit/s, GiBs och så vidare).</span><span class="sxs-lookup"><span data-stu-id="c39b5-154">You can run hello Oracle AWR report tooget hello metrics (IOPS, Mbps, GiBs, and so on).</span></span> <span data-ttu-id="c39b5-155">Välj hello VM baserat på hello mått som du samlade in.</span><span class="sxs-lookup"><span data-stu-id="c39b5-155">Then choose hello VM based on hello metrics that you collected.</span></span> <span data-ttu-id="c39b5-156">Eller så kan du kontakta din infrastruktur team tooget liknande information.</span><span class="sxs-lookup"><span data-stu-id="c39b5-156">Or you can contact your infrastructure team tooget similar information.</span></span>

<span data-ttu-id="c39b5-157">Du kan även köra rapporten AWR under både vanliga och högsta antal arbetsbelastningar, så kan du jämföra.</span><span class="sxs-lookup"><span data-stu-id="c39b5-157">You might consider running your AWR report during both regular and peak workloads, so you can compare.</span></span> <span data-ttu-id="c39b5-158">Baserat på de här rapporterna kan du ändra storlek hello virtuella datorer baserat på hello genomsnittlig arbetsbelastning eller hello maximal arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="c39b5-158">Based on these reports, you can size hello VMs based on either hello average workload or hello maximum workload.</span></span>

<span data-ttu-id="c39b5-159">Följande är ett exempel på hur toogenerate en AWR rapport:</span><span class="sxs-lookup"><span data-stu-id="c39b5-159">Following is an example of how toogenerate an AWR report:</span></span>

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a><span data-ttu-id="c39b5-160">Viktiga mått</span><span class="sxs-lookup"><span data-stu-id="c39b5-160">Key metrics</span></span>

<span data-ttu-id="c39b5-161">Följande är hello mått som du kan hämta från hello AWR rapporten:</span><span class="sxs-lookup"><span data-stu-id="c39b5-161">Following are hello metrics that you can obtain from hello AWR report:</span></span>

- <span data-ttu-id="c39b5-162">Totalt antal kärnor</span><span class="sxs-lookup"><span data-stu-id="c39b5-162">Total number of cores</span></span>
- <span data-ttu-id="c39b5-163">CPU-klockfrekvens</span><span class="sxs-lookup"><span data-stu-id="c39b5-163">CPU clock speed</span></span>
- <span data-ttu-id="c39b5-164">Totalt minne i GB</span><span class="sxs-lookup"><span data-stu-id="c39b5-164">Total memory in GB</span></span>
- <span data-ttu-id="c39b5-165">CPU-användning</span><span class="sxs-lookup"><span data-stu-id="c39b5-165">CPU utilization</span></span>
- <span data-ttu-id="c39b5-166">Högsta överföringshastighet</span><span class="sxs-lookup"><span data-stu-id="c39b5-166">Peak data transfer rate</span></span>
- <span data-ttu-id="c39b5-167">Antalet i/o-ändringar (läsa/skriva)</span><span class="sxs-lookup"><span data-stu-id="c39b5-167">Rate of I/O changes (read/write)</span></span>
- <span data-ttu-id="c39b5-168">Gör om loggen hastighet (MBPs)</span><span class="sxs-lookup"><span data-stu-id="c39b5-168">Redo log rate (MBPs)</span></span>
- <span data-ttu-id="c39b5-169">Dataflödet i nätverket</span><span class="sxs-lookup"><span data-stu-id="c39b5-169">Network throughput</span></span>
- <span data-ttu-id="c39b5-170">Nätverket latens hastighet (låg/hög)</span><span class="sxs-lookup"><span data-stu-id="c39b5-170">Network latency rate (low/high)</span></span>
- <span data-ttu-id="c39b5-171">Databasens storlek i GB</span><span class="sxs-lookup"><span data-stu-id="c39b5-171">Database size in GB</span></span>
- <span data-ttu-id="c39b5-172">Byte som tagits emot via SQL * Net från / tooclient</span><span class="sxs-lookup"><span data-stu-id="c39b5-172">Bytes received via SQL*Net from/tooclient</span></span>

### <a name="virtual-machine-size"></a><span data-ttu-id="c39b5-173">Storlek på virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c39b5-173">Virtual machine size</span></span>

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-hello-awr-report"></a><span data-ttu-id="c39b5-174">1. Beräkna VM-storlek baserat på CPU, minne och i/o-användning från hello AWR rapport</span><span class="sxs-lookup"><span data-stu-id="c39b5-174">1. Estimate VM size based on CPU, memory, and I/O usage from hello AWR report</span></span>

<span data-ttu-id="c39b5-175">En sak som du kan titta på är hello översta fem tidsinställd förgrunden händelser som indikerar om hello system flaskhalsar är.</span><span class="sxs-lookup"><span data-stu-id="c39b5-175">One thing you might look at is hello top five timed foreground events that indicate where hello system bottlenecks are.</span></span>

<span data-ttu-id="c39b5-176">I följande diagram hello, är exempelvis hello loggen filsynkronisering hello överst.</span><span class="sxs-lookup"><span data-stu-id="c39b5-176">For example, in hello following diagram, hello log file sync is at hello top.</span></span> <span data-ttu-id="c39b5-177">Den anger hello väntar som krävs innan hello LGWR skriver hello buffert toohello gör om loggen loggfilen.</span><span class="sxs-lookup"><span data-stu-id="c39b5-177">It indicates hello number of waits that are required before hello LGWR writes hello log buffer toohello redo log file.</span></span> <span data-ttu-id="c39b5-178">Dessa resultat indikerar att bättre prestanda lagring eller diskar är nödvändiga.</span><span class="sxs-lookup"><span data-stu-id="c39b5-178">These results indicate that better performing storage or disks are required.</span></span> <span data-ttu-id="c39b5-179">Hello diagram visar dessutom också hello antalet CPU (kärnor) och hello mängden minne.</span><span class="sxs-lookup"><span data-stu-id="c39b5-179">In addition, hello diagram also shows hello number of CPU (cores) and hello amount of memory.</span></span>

![Skärmbild av sidan hello AWR](./media/oracle-design/cpu_memory_info.png)

<span data-ttu-id="c39b5-181">hello visar följande diagram hello total i/o för läsning och skrivning.</span><span class="sxs-lookup"><span data-stu-id="c39b5-181">hello following diagram shows hello total I/O of read and write.</span></span> <span data-ttu-id="c39b5-182">Det fanns 59 läsa och 247.3 GB skrivs under hello tiden för hello-rapport.</span><span class="sxs-lookup"><span data-stu-id="c39b5-182">There were 59 GB read and 247.3 GB written during hello time of hello report.</span></span>

![Skärmbild av sidan hello AWR](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a><span data-ttu-id="c39b5-184">2. Välj en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c39b5-184">2. Choose a VM</span></span>

<span data-ttu-id="c39b5-185">Baserat på hello information du samlade in från hello AWR rapporten är hello nästa steg toochoose en virtuell dator med samma storlek som uppfyller dina krav.</span><span class="sxs-lookup"><span data-stu-id="c39b5-185">Based on hello information that you collected from hello AWR report, hello next step is toochoose a VM of a similar size that meets your requirements.</span></span> <span data-ttu-id="c39b5-186">Du hittar en lista med tillgängliga virtuella datorer i hello artikel [Minnesoptimerade](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).</span><span class="sxs-lookup"><span data-stu-id="c39b5-186">You can find a list of available VMs in hello article [Memory optimized](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).</span></span>

#### <a name="3-fine-tune-hello-vm-sizing-with-a-similar-vm-series-based-on-hello-acu"></a><span data-ttu-id="c39b5-187">3. Finjustera hello VM-storlek med liknande VM baserat på hello ACU</span><span class="sxs-lookup"><span data-stu-id="c39b5-187">3. Fine-tune hello VM sizing with a similar VM series based on hello ACU</span></span>

<span data-ttu-id="c39b5-188">När du har valt hello VM betala uppmärksamhet toohello ACU för hello VM.</span><span class="sxs-lookup"><span data-stu-id="c39b5-188">After you've chosen hello VM, pay attention toohello ACU for hello VM.</span></span> <span data-ttu-id="c39b5-189">Du kan välja en annan virtuell dator baserat på hello ACU-värde som passar dina behov bättre.</span><span class="sxs-lookup"><span data-stu-id="c39b5-189">You might choose a different VM based on hello ACU value that better suits your requirements.</span></span> <span data-ttu-id="c39b5-190">Mer information finns i [Azure compute enhet](https://docs.microsoft.com/azure/virtual-machines/windows/acu).</span><span class="sxs-lookup"><span data-stu-id="c39b5-190">For more information, see [Azure compute unit](https://docs.microsoft.com/azure/virtual-machines/windows/acu).</span></span>

![Skärmbild av sidan för hello ACU enheter](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a><span data-ttu-id="c39b5-192">Dataflödet i nätverket</span><span class="sxs-lookup"><span data-stu-id="c39b5-192">Network throughput</span></span>

<span data-ttu-id="c39b5-193">följande diagram visar hello relationen mellan genomflöde och IOPS hello:</span><span class="sxs-lookup"><span data-stu-id="c39b5-193">hello following diagram shows hello relation between throughput and IOPS:</span></span>

![Skärmbild av dataflöde](./media/oracle-design/throughput.png)

<span data-ttu-id="c39b5-195">hello totala genomflödet beräknas utifrån hello följande information:</span><span class="sxs-lookup"><span data-stu-id="c39b5-195">hello total network throughput is estimated based on hello following information:</span></span>
- <span data-ttu-id="c39b5-196">SQL * Net trafik</span><span class="sxs-lookup"><span data-stu-id="c39b5-196">SQL*Net traffic</span></span>
- <span data-ttu-id="c39b5-197">Mbit/s x antal servrar (till exempel Oracle Data Guard utgående ström)</span><span class="sxs-lookup"><span data-stu-id="c39b5-197">MBps x number of servers (outbound stream such as Oracle Data Guard)</span></span>
- <span data-ttu-id="c39b5-198">Andra faktorer, till exempel program replikering</span><span class="sxs-lookup"><span data-stu-id="c39b5-198">Other factors, such as application replication</span></span>

![Skärmbild av hello SQL * Net genomflöde](./media/oracle-design/sqlnet_info.png)

<span data-ttu-id="c39b5-200">Baserat på din kraven på nätverksbandbredd finns olika typer av gateway för toochoose från.</span><span class="sxs-lookup"><span data-stu-id="c39b5-200">Based on your network bandwidth requirements, there are various gateway types for you toochoose from.</span></span> <span data-ttu-id="c39b5-201">Dessa inkluderar basic VpnGw och Azure ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="c39b5-201">These include basic, VpnGw, and Azure ExpressRoute.</span></span> <span data-ttu-id="c39b5-202">Mer information finns i hello [VPN-gateway sida med priser](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).</span><span class="sxs-lookup"><span data-stu-id="c39b5-202">For more information, see hello [VPN gateway pricing page](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).</span></span>

<span data-ttu-id="c39b5-203">**Rekommendationer**</span><span class="sxs-lookup"><span data-stu-id="c39b5-203">**Recommendations**</span></span>

- <span data-ttu-id="c39b5-204">Nätverksfördröjningen är högre jämfört med tooan lokal distribution.</span><span class="sxs-lookup"><span data-stu-id="c39b5-204">Network latency is higher compared tooan on-premises deployment.</span></span> <span data-ttu-id="c39b5-205">Minskar nätverket avrunda resor kan kraftigt förbättra prestanda.</span><span class="sxs-lookup"><span data-stu-id="c39b5-205">Reducing network round trips can greatly improve performance.</span></span>
- <span data-ttu-id="c39b5-206">tooreduce turer konsolidera program som har hög transaktioner eller ”chatty” appar i hello samma virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="c39b5-206">tooreduce round-trips, consolidate applications that have high transactions or “chatty” apps on hello same virtual machine.</span></span>

### <a name="disk-types-and-configurations"></a><span data-ttu-id="c39b5-207">Disktyper och konfigurationer</span><span class="sxs-lookup"><span data-stu-id="c39b5-207">Disk types and configurations</span></span>

- <span data-ttu-id="c39b5-208">*OS-diskar som standard*: dessa disktyper erbjuder beständiga data och cachelagring.</span><span class="sxs-lookup"><span data-stu-id="c39b5-208">*Default OS disks*: These disk types offer persistent data and caching.</span></span> <span data-ttu-id="c39b5-209">De är optimerade för OS åtkomst vid start och inte har utformats för antingen transaktionella eller datalager (Analytiska) arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="c39b5-209">They are optimized for OS access at startup, and aren't designed for either transactional or data warehouse (analytical) workloads.</span></span>

- <span data-ttu-id="c39b5-210">*Ohanterad diskar*: dessa disktyper du hantera hello storage-konton som lagrar hello virtuell hårddisk (VHD)-filer som motsvarar tooyour Virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="c39b5-210">*Unmanaged disks*: With these disk types, you manage hello storage accounts that store hello virtual hard disk (VHD) files that correspond tooyour VM disks.</span></span> <span data-ttu-id="c39b5-211">VHD-filer lagras som sidblobbar i Azure storage-konton.</span><span class="sxs-lookup"><span data-stu-id="c39b5-211">VHD files are stored as page blobs in Azure storage accounts.</span></span>

- <span data-ttu-id="c39b5-212">*Hanterade diskar*: Azure hanterar hello storage-konton som du använder för din Virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="c39b5-212">*Managed disks*: Azure manages hello storage accounts that you use for your VM disks.</span></span> <span data-ttu-id="c39b5-213">Anger hello disktyp (premium eller standard) och hello storleken på hello-disk som du behöver.</span><span class="sxs-lookup"><span data-stu-id="c39b5-213">You specify hello disk type (premium or standard) and hello size of hello disk that you need.</span></span> <span data-ttu-id="c39b5-214">Azure skapar och hanterar hello disk du.</span><span class="sxs-lookup"><span data-stu-id="c39b5-214">Azure creates and manages hello disk for you.</span></span>

- <span data-ttu-id="c39b5-215">*Premium-lagringsdiskar*: dessa disktyper som passar bäst för produktionsarbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="c39b5-215">*Premium storage disks*: These disk types are best suited for production workloads.</span></span> <span data-ttu-id="c39b5-216">Premium storage stöder Virtuella diskar som kan vara ansluten toospecific storlek-serien virtuella datorer, till exempel DS, DSv2, GS och F serien virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c39b5-216">Premium storage supports VM disks that can be attached toospecific size-series VMs, such as DS, DSv2, GS, and F series VMs.</span></span> <span data-ttu-id="c39b5-217">hello premium disk levereras med olika storlekar och du kan välja mellan diskar från 32 GB too4, 096 GB.</span><span class="sxs-lookup"><span data-stu-id="c39b5-217">hello premium disk comes with different sizes, and you can choose between disks ranging from 32 GB too4,096 GB.</span></span> <span data-ttu-id="c39b5-218">Varje diskstorleken har sin egen prestandakrav.</span><span class="sxs-lookup"><span data-stu-id="c39b5-218">Each disk size has its own performance specifications.</span></span> <span data-ttu-id="c39b5-219">Du kan koppla en eller flera diskar tooyour VM beroende på kraven för application.</span><span class="sxs-lookup"><span data-stu-id="c39b5-219">Depending on your application requirements, you can attach one or more disks tooyour VM.</span></span>

<span data-ttu-id="c39b5-220">När du skapar en ny hanterade disk från hello portal, kan du välja hello **kontotyp** hello typ av disk du vill ha toouse.</span><span class="sxs-lookup"><span data-stu-id="c39b5-220">When you create a new managed disk from hello portal, you can choose hello **Account type** for hello type of disk you want toouse.</span></span> <span data-ttu-id="c39b5-221">Tänk på att inte alla tillgängliga diskar visas i hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="c39b5-221">Keep in mind that not all available disks are shown in hello drop-down menu.</span></span> <span data-ttu-id="c39b5-222">När du väljer en viss VM-storlek, visar hello-menyn endast hello tillgängliga premium-lagring SKU: er som är baserade på att VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="c39b5-222">After you choose a particular VM size, hello menu shows only hello available premium storage SKUs that are based on that VM size.</span></span>

![Skärmbild av sidan för hello hanterade diskar](./media/oracle-design/premium_disk01.png)

<span data-ttu-id="c39b5-224">Mer information finns i [Premium-lagring med hög prestanda och hanterade diskar för virtuella datorer](https://docs.microsoft.com/azure/storage/storage-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="c39b5-224">For more information, see [High-performance Premium Storage and managed disks for VMs](https://docs.microsoft.com/azure/storage/storage-premium-storage).</span></span>

<span data-ttu-id="c39b5-225">När du har konfigurerat din lagring på en virtuell dator kanske du vill tooload test hello diskar innan du skapar en databas.</span><span class="sxs-lookup"><span data-stu-id="c39b5-225">After you configure your storage on a VM, you might want tooload test hello disks before creating a database.</span></span> <span data-ttu-id="c39b5-226">Att veta hello i/o-hastighet på både svarstid och genomströmning kan hjälpa dig att avgöra om hello virtuella datorer stöder hello förväntades dataflöde med latens mål.</span><span class="sxs-lookup"><span data-stu-id="c39b5-226">Knowing hello I/O rate in terms of both latency and throughput can help you determine if hello VMs support hello expected throughput with latency targets.</span></span>

<span data-ttu-id="c39b5-227">Det finns ett antal verktyg programtest belastning, till exempel Oracle Orion, Sysbench och Fio.</span><span class="sxs-lookup"><span data-stu-id="c39b5-227">There are a number of tools for application load testing, such as Oracle Orion, Sysbench, and Fio.</span></span>

<span data-ttu-id="c39b5-228">Kör hello belastningstest igen när du har distribuerat en Oracle-databas.</span><span class="sxs-lookup"><span data-stu-id="c39b5-228">Run hello load test again after you've deployed an Oracle database.</span></span> <span data-ttu-id="c39b5-229">Starta din regelbundet och högsta antal arbetsbelastningar och hello resultatet visar du hello baslinje för din miljö.</span><span class="sxs-lookup"><span data-stu-id="c39b5-229">Start your regular and peak workloads, and hello results show you hello baseline of your environment.</span></span>

<span data-ttu-id="c39b5-230">Det kan vara viktigare toosize hello-lagring baseras på hello IOPS hastighet i stället för hello lagringsstorlek.</span><span class="sxs-lookup"><span data-stu-id="c39b5-230">It might be more important toosize hello storage based on hello IOPS rate rather than hello storage size.</span></span> <span data-ttu-id="c39b5-231">Till exempel om hello krävs IOPS är 5 000, men du behöver bara 200 GB, kan du fortfarande få hello P30 klassen premium disk trots att den innehåller fler än 200 GB lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="c39b5-231">For example, if hello required IOPS is 5,000, but you only need 200 GB, you might still get hello P30 class premium disk even though it comes with more than 200 GB of storage.</span></span>

<span data-ttu-id="c39b5-232">hello IOPS hastighet kan hämtas från hello AWR rapporten.</span><span class="sxs-lookup"><span data-stu-id="c39b5-232">hello IOPS rate can be obtained from hello AWR report.</span></span> <span data-ttu-id="c39b5-233">Det bestäms av hello gör om loggen, fysiska läsningar och skrivningar snabbt.</span><span class="sxs-lookup"><span data-stu-id="c39b5-233">It's determined by hello redo log, physical reads, and writes rate.</span></span>

![Skärmbild av sidan hello AWR](./media/oracle-design/awr_report.png)

<span data-ttu-id="c39b5-235">Till exempel är hello gör om 12,200,000 byte per sekund, vilket är lika too11.63 Mbit/s.</span><span class="sxs-lookup"><span data-stu-id="c39b5-235">For example, hello redo size is 12,200,000 bytes per second, which is equal too11.63 MBPs.</span></span>
<span data-ttu-id="c39b5-236">hello IOPS är 12 200 000 / 2,358 = 5,174.</span><span class="sxs-lookup"><span data-stu-id="c39b5-236">hello IOPS is 12,200,000 / 2,358 = 5,174.</span></span>

<span data-ttu-id="c39b5-237">När du har en tydlig bild av hello i/o-kraven kan välja du en kombination av enheter som är bäst lämpade toomeet dessa krav.</span><span class="sxs-lookup"><span data-stu-id="c39b5-237">After you have a clear picture of hello I/O requirements, you can choose a combination of drives that are best suited toomeet those requirements.</span></span>

<span data-ttu-id="c39b5-238">**Rekommendationer**</span><span class="sxs-lookup"><span data-stu-id="c39b5-238">**Recommendations**</span></span>

- <span data-ttu-id="c39b5-239">Sprider sig hello i/o-belastning över ett antal diskar med hjälp av hanterade lagringspoolen eller Oracle ASM för data tabellutrymmet.</span><span class="sxs-lookup"><span data-stu-id="c39b5-239">For data tablespace, spread hello I/O workload across a number of disks by using managed storage or Oracle ASM.</span></span>
- <span data-ttu-id="c39b5-240">Hello i/o-blockstorlek ökar för läsintensiva- och skrivåtgärder-intensiva, lägga till flera datadiskar.</span><span class="sxs-lookup"><span data-stu-id="c39b5-240">As hello I/O block size increases for read-intensive and write-intensive operations, add more data disks.</span></span>
- <span data-ttu-id="c39b5-241">Öka hello blockstorlek för stora sekventiella processer.</span><span class="sxs-lookup"><span data-stu-id="c39b5-241">Increase hello block size for large sequential processes.</span></span>
- <span data-ttu-id="c39b5-242">Använd data komprimering tooreduce i/o (för både data och index).</span><span class="sxs-lookup"><span data-stu-id="c39b5-242">Use data compression tooreduce I/O (for both data and indexes).</span></span>
- <span data-ttu-id="c39b5-243">Avgränsa gör om loggfiler, system och temps och ångra TS på separata hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="c39b5-243">Separate redo logs, system, and temps, and undo TS on separate data disks.</span></span>
- <span data-ttu-id="c39b5-244">Placera inte några programfiler på standard OS-diskar (/ dev/sda).</span><span class="sxs-lookup"><span data-stu-id="c39b5-244">Don't put any application files on default OS disks (/dev/sda).</span></span> <span data-ttu-id="c39b5-245">Diskarna inte är optimerad för snabb VM Start gånger, och de kan inte ange goda prestanda för ditt program.</span><span class="sxs-lookup"><span data-stu-id="c39b5-245">These disks aren't optimized for fast VM boot times, and they might not provide good performance for your application.</span></span>

### <a name="disk-cache-settings"></a><span data-ttu-id="c39b5-246">Inställningar för cachelagring av disk</span><span class="sxs-lookup"><span data-stu-id="c39b5-246">Disk cache settings</span></span>

<span data-ttu-id="c39b5-247">Det finns tre alternativ för värdcachelagring:</span><span class="sxs-lookup"><span data-stu-id="c39b5-247">There are three options for host caching:</span></span>

- <span data-ttu-id="c39b5-248">*Skrivskyddad*: alla begäranden cachelagras för framtida läsningar.</span><span class="sxs-lookup"><span data-stu-id="c39b5-248">*Read-only*: All requests are cached for future reads.</span></span> <span data-ttu-id="c39b5-249">Alla skrivningar sparas direkt tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="c39b5-249">All writes are persisted directly tooAzure Blob storage.</span></span>

- <span data-ttu-id="c39b5-250">*Skrivskyddad*: Detta är en ”read-ahead” algoritm.</span><span class="sxs-lookup"><span data-stu-id="c39b5-250">*Read-write*: This is a “read-ahead” algorithm.</span></span> <span data-ttu-id="c39b5-251">hello läser och skriver cachelagras för framtida läsningar.</span><span class="sxs-lookup"><span data-stu-id="c39b5-251">hello reads and writes are cached for future reads.</span></span> <span data-ttu-id="c39b5-252">Icke skrivning via skrivningar är beständiga toohello lokalt cacheminne först.</span><span class="sxs-lookup"><span data-stu-id="c39b5-252">Non-write-through writes are persisted toohello local cache first.</span></span> <span data-ttu-id="c39b5-253">För SQL Server är skrivningar beständiga tooAzure lagring eftersom den använder skrivning via.</span><span class="sxs-lookup"><span data-stu-id="c39b5-253">For SQL Server, writes are persisted tooAzure Storage because it uses write-through.</span></span> <span data-ttu-id="c39b5-254">Det ger också hello lägsta disk fördröjningen för enstaka arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="c39b5-254">It also provides hello lowest disk latency for light workloads.</span></span>

- <span data-ttu-id="c39b5-255">*Ingen* (inaktiverat): med det här alternativet kan du kringgå hello cache.</span><span class="sxs-lookup"><span data-stu-id="c39b5-255">*None* (disabled): By using this option, you can bypass hello cache.</span></span> <span data-ttu-id="c39b5-256">Alla hello data överförda toodisk och beständiga tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="c39b5-256">All hello data is transferred toodisk and persisted tooAzure Storage.</span></span> <span data-ttu-id="c39b5-257">Den här metoden ger du hello högsta i/o-hastighet för i/o-intensiv arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="c39b5-257">This method gives you hello highest I/O rate for I/O intensive workloads.</span></span> <span data-ttu-id="c39b5-258">Du måste också tootake ”transaktionen kostnad” i beräkningen.</span><span class="sxs-lookup"><span data-stu-id="c39b5-258">You also need tootake “transaction cost” into consideration.</span></span>

<span data-ttu-id="c39b5-259">**Rekommendationer**</span><span class="sxs-lookup"><span data-stu-id="c39b5-259">**Recommendations**</span></span>

<span data-ttu-id="c39b5-260">toomaximize hello dataflöde, rekommenderar vi att du börjar med **ingen** för cachelagring av värden.</span><span class="sxs-lookup"><span data-stu-id="c39b5-260">toomaximize hello throughput, we recommend that you  start with **None** for host caching.</span></span> <span data-ttu-id="c39b5-261">För Premium-lagring, Tänk på att du måste inaktivera hello ”barriärer” när du monterar hello filsystem med hello **ReadOnly** eller **ingen** alternativ.</span><span class="sxs-lookup"><span data-stu-id="c39b5-261">For Premium Storage, keep in mind that you must disable hello "barriers" when you mount hello file system with hello **ReadOnly** or **None** options.</span></span> <span data-ttu-id="c39b5-262">Uppdatera hello /etc/fstab filen med hello UUID toohello diskar.</span><span class="sxs-lookup"><span data-stu-id="c39b5-262">Update hello /etc/fstab file with hello UUID toohello disks.</span></span>

<span data-ttu-id="c39b5-263">Mer information finns i [Premium-lagring för virtuella Linux-datorer](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).</span><span class="sxs-lookup"><span data-stu-id="c39b5-263">For more information, see [Premium Storage for Linux VMs](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).</span></span>

![Skärmbild av sidan för hello hanterade diskar](./media/oracle-design/premium_disk02.png)

- <span data-ttu-id="c39b5-265">Använd standard för OS diskar **läsning och skrivning** cachelagring.</span><span class="sxs-lookup"><span data-stu-id="c39b5-265">For OS disks, use default **Read/Write** caching.</span></span>
- <span data-ttu-id="c39b5-266">Använd för SYSTEM, TEMP och ångra **ingen** för cachelagring.</span><span class="sxs-lookup"><span data-stu-id="c39b5-266">For SYSTEM, TEMP, and UNDO use **None** for caching.</span></span>
- <span data-ttu-id="c39b5-267">DATA, Använd **ingen** för cachelagring.</span><span class="sxs-lookup"><span data-stu-id="c39b5-267">For DATA, use **None** for caching.</span></span> <span data-ttu-id="c39b5-268">Men om databasen är skrivskyddad eller läsintensiva använder **skrivskyddad** cachelagring.</span><span class="sxs-lookup"><span data-stu-id="c39b5-268">But if your database is read-only or read-intensive, use **Read-only** caching.</span></span>

<span data-ttu-id="c39b5-269">När inställningen disk data sparas, kan du inte ändra hello värden cache-inställningen om du inte demontera hello enheten vid hello OS-nivån och sedan montera när du har gjort hello ändra.</span><span class="sxs-lookup"><span data-stu-id="c39b5-269">After your data disk setting is saved, you can't change hello host cache setting unless you unmount hello drive at hello OS level and then remount it after you've made hello change.</span></span>


## <a name="security"></a><span data-ttu-id="c39b5-270">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="c39b5-270">Security</span></span>

<span data-ttu-id="c39b5-271">När du har skapat och konfigurerat Azure-miljön, hello nästa steg är toosecure nätverket.</span><span class="sxs-lookup"><span data-stu-id="c39b5-271">After you have set up and configured your Azure environment, hello next step is toosecure your network.</span></span> <span data-ttu-id="c39b5-272">Här följer några rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c39b5-272">Here are some recommendations:</span></span>

- <span data-ttu-id="c39b5-273">*NSG princip*: NSG kan definieras av ett undernät eller nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="c39b5-273">*NSG policy*: NSG can be defined by a subnet or NIC.</span></span> <span data-ttu-id="c39b5-274">Det är enklare toocontrol åtkomst på hello undernätverksnivå både för säkerhet och kraft routning för till exempel brandväggar för programmet.</span><span class="sxs-lookup"><span data-stu-id="c39b5-274">It's simpler toocontrol access at hello subnet level both for security and force routing for things like application firewalls.</span></span>

- <span data-ttu-id="c39b5-275">*Jumpbox*: för säkrare åtkomst administratörer bör inte ansluter direkt toohello programtjänsten eller databas.</span><span class="sxs-lookup"><span data-stu-id="c39b5-275">*Jumpbox*: For more secure access, administrators should not directly connect toohello application service or database.</span></span> <span data-ttu-id="c39b5-276">En jumpbox används som en media mellan Hej administratör dator och Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="c39b5-276">A jumpbox is used as a media between hello administrator machine and Azure resources.</span></span>
<span data-ttu-id="c39b5-277">![Skärmbild av hello Jumpbox topologi sida](./media/oracle-design/jumpbox.png)</span><span class="sxs-lookup"><span data-stu-id="c39b5-277">![Screenshot of hello Jumpbox topology page](./media/oracle-design/jumpbox.png)</span></span>

    <span data-ttu-id="c39b5-278">Hej administratör datorn bör erbjuda IP tillgång toohello jumpbox.</span><span class="sxs-lookup"><span data-stu-id="c39b5-278">hello administrator machine should offer IP-restricted access toohello jumpbox only.</span></span> <span data-ttu-id="c39b5-279">Hej jumpbox ska ha åtkomst toohello programmet och databasen.</span><span class="sxs-lookup"><span data-stu-id="c39b5-279">hello jumpbox should have access toohello application and database.</span></span>

- <span data-ttu-id="c39b5-280">*Privat nätverk* (undernät): Vi rekommenderar att du har hello programtjänsten och databasen på olika undernät så bättre kontroll kan anges av NSG principen.</span><span class="sxs-lookup"><span data-stu-id="c39b5-280">*Private network* (subnets): We recommend that you have hello application service and database on separate subnets, so better control can be set by NSG policy.</span></span>


## <a name="additional-reading"></a><span data-ttu-id="c39b5-281">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c39b5-281">Additional reading</span></span>

- [<span data-ttu-id="c39b5-282">Konfigurera Oracle ASM</span><span class="sxs-lookup"><span data-stu-id="c39b5-282">Configure Oracle ASM</span></span>](configure-oracle-asm.md)
- [<span data-ttu-id="c39b5-283">Konfigurera Oracle Data Guard</span><span class="sxs-lookup"><span data-stu-id="c39b5-283">Configure Oracle Data Guard</span></span>](configure-oracle-dataguard.md)
- [<span data-ttu-id="c39b5-284">Konfigurera Oracle guld Gate</span><span class="sxs-lookup"><span data-stu-id="c39b5-284">Configure Oracle Golden Gate</span></span>](configure-oracle-golden-gate.md)
- [<span data-ttu-id="c39b5-285">Oracle-säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="c39b5-285">Oracle backup and recovery</span></span>](oracle-backup-recovery.md)

## <a name="next-steps"></a><span data-ttu-id="c39b5-286">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c39b5-286">Next steps</span></span>

- [<span data-ttu-id="c39b5-287">Självstudier: Skapa högtillgängliga virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c39b5-287">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)
- [<span data-ttu-id="c39b5-288">Utforska VM distribution Azure CLI-exempel</span><span class="sxs-lookup"><span data-stu-id="c39b5-288">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
