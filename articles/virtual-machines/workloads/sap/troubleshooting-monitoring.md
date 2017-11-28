---
title: "Felsökning och övervakning av SAP HANA i Azure (stora instanser) | Microsoft Docs"
description: "Felsöka och övervaka SAP HANA på ett Azure (stora instanser)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee5be707b443cbe42bf4a492d79390e534d4b91f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-troubleshoot-and-monitor-sap-hana-large-instances-on-azure"></a><span data-ttu-id="c8819-103">Hur du felsöker och övervaka SAP HANA (stora instanser) på Azure</span><span class="sxs-lookup"><span data-stu-id="c8819-103">How to troubleshoot and monitor SAP HANA (large instances) on Azure</span></span>


## <a name="monitoring-in-sap-hana-on-azure-large-instances"></a><span data-ttu-id="c8819-104">Övervakning av SAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="c8819-104">Monitoring in SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="c8819-105">SAP HANA i Azure (stora instanser) skiljer sig inte från andra IaaS-distribution, behöver du övervaka vad Operativsystemet och programmet gör och hur de använder följande resurser:</span><span class="sxs-lookup"><span data-stu-id="c8819-105">SAP HANA on Azure (Large Instances) is no different from any other IaaS deployment — you need to monitor what the OS and the application is doing and how these consume the following resources:</span></span>

- <span data-ttu-id="c8819-106">Processor</span><span class="sxs-lookup"><span data-stu-id="c8819-106">CPU</span></span>
- <span data-ttu-id="c8819-107">Minne</span><span class="sxs-lookup"><span data-stu-id="c8819-107">Memory</span></span>
- <span data-ttu-id="c8819-108">Nätverkets bandbredd</span><span class="sxs-lookup"><span data-stu-id="c8819-108">Network bandwidth</span></span>
- <span data-ttu-id="c8819-109">Diskutrymme</span><span class="sxs-lookup"><span data-stu-id="c8819-109">Disk space</span></span>

<span data-ttu-id="c8819-110">Med Azure virtuella datorer som du behöver ta reda på om resursen klasserna som anges ovan är tillräckligt eller om de få slut.</span><span class="sxs-lookup"><span data-stu-id="c8819-110">As with Azure Virtual Machines you need to figure out whether the resource classes named above are sufficient, or whether these get depleted.</span></span> <span data-ttu-id="c8819-111">Här är mer information om var och en av de olika klasserna:</span><span class="sxs-lookup"><span data-stu-id="c8819-111">Here is more detail on each of the different classes:</span></span>

<span data-ttu-id="c8819-112">**CPU-resursanvändningen:** förhållandet SAP som definierats för vissa arbetsbelastningar mot HANA tillämpas för att se till att det ska vara tillräckligt med processorresurser som är tillgänglig för arbete med data som lagras i minnet.</span><span class="sxs-lookup"><span data-stu-id="c8819-112">**CPU resource consumption:** The ratio that SAP defined for certain workload against HANA is enforced to make sure that there should be enough CPU resources available to work through the data that is stored in memory.</span></span> <span data-ttu-id="c8819-113">Det kan dock finnas fall där HANA förbrukar en stor mängd CPU köra frågor på grund av index som saknas eller liknande problem.</span><span class="sxs-lookup"><span data-stu-id="c8819-113">Nevertheless, there might be cases where HANA consumes a lot of CPU executing queries due to missing indexes or similar issues.</span></span> <span data-ttu-id="c8819-114">Det innebär att du ska övervaka CPU resursförbrukning HANA stora instans enhet samt CPU-resurser som används av specifika HANA-tjänster.</span><span class="sxs-lookup"><span data-stu-id="c8819-114">This means you should monitor CPU resource consumption of the HANA large instance unit as well as CPU resources consumed by the specific HANA services.</span></span>

<span data-ttu-id="c8819-115">**Minnesförbrukning:** är viktigt att övervaka från inom HANA samt utanför HANA på enheten.</span><span class="sxs-lookup"><span data-stu-id="c8819-115">**Memory consumption:** Is important to monitor from within HANA, as well as outside of HANA on the unit.</span></span> <span data-ttu-id="c8819-116">Övervaka inom HANA, hur data förbrukar allokerat minne för att hålla sig inom de nödvändiga storlek riktlinjerna för SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="c8819-116">Within HANA, monitor how the data is consuming HANA allocated memory in order to stay within the required sizing guidelines of SAP.</span></span> <span data-ttu-id="c8819-117">Du bör också övervaka minnesförbrukning på instansnivå stora för att säkerställa att ytterligare installerade icke-HANA programvara inte förbrukar för mycket minne och därför konkurrerar med HANA för minne.</span><span class="sxs-lookup"><span data-stu-id="c8819-117">You also want to monitor memory consumption on the Large Instance level to make sure that additional installed non-HANA software does not consume too much memory, and therefore compete with HANA for memory.</span></span>

<span data-ttu-id="c8819-118">**Nätverkets bandbredd:** i Azure-VNet-gateway har begränsad bandbredd på data flyttas till Azure-VNet, så är det bra att övervaka data tas emot av alla virtuella Azure-datorer inom ett VNet att ta reda på hur nära du kommer att gränserna för Azure gateway SKU du markering å.</span><span class="sxs-lookup"><span data-stu-id="c8819-118">**Network bandwidth:** The Azure VNet gateway is limited in bandwidth of data moving into the Azure VNet, so it is helpful to monitor the data received by all the Azure VMs within a VNet to figure out how close you are to the limits of the Azure gateway SKU you selected.</span></span> <span data-ttu-id="c8819-119">På HANA stora instans-enhet det vara klokt att övervaka inkommande och utgående nätverkstrafik samt och för att hålla reda på de volymer som ska hanteras med tiden.</span><span class="sxs-lookup"><span data-stu-id="c8819-119">On the HANA Large Instance unit, it does make sense to monitor incoming and outgoing network traffic as well, and to keep track of the volumes that are handled over time.</span></span>

<span data-ttu-id="c8819-120">**Diskutrymme:** förbrukningen av diskutrymme vanligtvis ökar med tiden.</span><span class="sxs-lookup"><span data-stu-id="c8819-120">**Disk space:** Disk space consumption usually increases over time.</span></span> <span data-ttu-id="c8819-121">Det finns många orsaker, men de flesta av alla är: datavolym ökar, körning av säkerhetskopiering av transaktionsloggar, lagra spårningsfiler och utföra storage snapshots.</span><span class="sxs-lookup"><span data-stu-id="c8819-121">There are many reasons for this, but most of all are: data volume increases, execution of transaction log backups, storing trace files, and performing storage snapshots.</span></span> <span data-ttu-id="c8819-122">Det är därför viktigt att övervaka användning av diskutrymme och hantera det diskutrymme som är associerade med HANA stora instans-enhet.</span><span class="sxs-lookup"><span data-stu-id="c8819-122">Therefore, it is important to monitor disk space usage and manage the disk space associated with the HANA Large Instance unit.</span></span>

## <a name="monitoring-and-troubleshooting-from-hana-side"></a><span data-ttu-id="c8819-123">Övervakning och felsökning från HANA sida</span><span class="sxs-lookup"><span data-stu-id="c8819-123">Monitoring and troubleshooting from HANA side</span></span>

<span data-ttu-id="c8819-124">För att effektivt analysera problem relaterade till SAP HANA i Azure (stora instanser), är det bra att begränsa den grundläggande orsaken till problemet.</span><span class="sxs-lookup"><span data-stu-id="c8819-124">In order to effectively analyze problems related to SAP HANA on Azure (Large Instances), it is useful to narrow down the root cause of a problem.</span></span> <span data-ttu-id="c8819-125">SAP har publicerat en stor mängd dokumentation som hjälper dig.</span><span class="sxs-lookup"><span data-stu-id="c8819-125">SAP has published a large amount of documentation to help you.</span></span>

<span data-ttu-id="c8819-126">Tillämpliga vanliga frågor och svar för SAP HANA prestanda finns i följande SAP-information:</span><span class="sxs-lookup"><span data-stu-id="c8819-126">Applicable FAQs related to SAP HANA performance can be found in the following SAP Notes:</span></span>

- [<span data-ttu-id="c8819-127">SAP-kommentar #2222200 – vanliga frågor och svar: SAP HANA-nätverk</span><span class="sxs-lookup"><span data-stu-id="c8819-127">SAP Note #2222200 – FAQ: SAP HANA Network</span></span>](https://launchpad.support.sap.com/#/notes/2222200)
- [<span data-ttu-id="c8819-128">SAP-kommentar #2100040 – vanliga frågor och svar: SAP HANA-processor</span><span class="sxs-lookup"><span data-stu-id="c8819-128">SAP Note #2100040 – FAQ: SAP HANA CPU</span></span>](https://launchpad.support.sap.com/#/notes/0002100040)
- [<span data-ttu-id="c8819-129">SAP-kommentar #199997 – vanliga frågor och svar: SAP HANA-minne</span><span class="sxs-lookup"><span data-stu-id="c8819-129">SAP Note #199997 – FAQ: SAP HANA Memory</span></span>](https://launchpad.support.sap.com/#/notes/2177064)
- [<span data-ttu-id="c8819-130">SAP-kommentar #200000 – vanliga frågor och svar: SAP HANA prestandaoptimering</span><span class="sxs-lookup"><span data-stu-id="c8819-130">SAP Note #200000 – FAQ: SAP HANA Performance Optimization</span></span>](https://launchpad.support.sap.com/#/notes/2000000)
- [<span data-ttu-id="c8819-131">SAP-kommentar #199930 – vanliga frågor och svar: SAP HANA-i/o analys</span><span class="sxs-lookup"><span data-stu-id="c8819-131">SAP Note #199930 – FAQ: SAP HANA I/O Analysis</span></span>](https://launchpad.support.sap.com/#/notes/1999930)
- [<span data-ttu-id="c8819-132">SAP-kommentar #2177064 – vanliga frågor och svar: SAP HANA-tjänsten startas om och kraschar</span><span class="sxs-lookup"><span data-stu-id="c8819-132">SAP Note #2177064 – FAQ: SAP HANA Service Restart and Crashes</span></span>](https://launchpad.support.sap.com/#/notes/2177064)

<span data-ttu-id="c8819-133">**SAP HANA aviseringar**</span><span class="sxs-lookup"><span data-stu-id="c8819-133">**SAP HANA Alerts**</span></span>

<span data-ttu-id="c8819-134">Som ett första steg i loggarna aktuella SAP HANA avisering.</span><span class="sxs-lookup"><span data-stu-id="c8819-134">As a first step, check the current SAP HANA alert logs.</span></span> <span data-ttu-id="c8819-135">SAP HANA Studio, gå till **administrationskonsolen: aviseringar: visa: alla aviseringar**.</span><span class="sxs-lookup"><span data-stu-id="c8819-135">In SAP HANA Studio, go to **Administration Console: Alerts: Show: all alerts**.</span></span> <span data-ttu-id="c8819-136">Den här fliken visas alla aviseringar för SAP HANA för specifika värden (fysiskt minne, CPU-användning osv.) som faller utanför ange lägsta och högsta tröskelvärden.</span><span class="sxs-lookup"><span data-stu-id="c8819-136">This tab will show all SAP HANA alerts for specific values (free physical memory, CPU utilization, etc.) that fall outside of the set minimum and maximum thresholds.</span></span> <span data-ttu-id="c8819-137">Som standard är kontrollerar uppdateras automatiskt var 15: e minut.</span><span class="sxs-lookup"><span data-stu-id="c8819-137">By default, checks are auto-refreshed every 15 minutes.</span></span>

![Gå till Administration Console i SAP HANA Studio: aviseringar: visa: alla aviseringar](./media/troubleshooting-monitoring/image1-show-alerts.png)

<span data-ttu-id="c8819-139">**CPU**</span><span class="sxs-lookup"><span data-stu-id="c8819-139">**CPU**</span></span>

<span data-ttu-id="c8819-140">För en avisering som aktiveras på grund av felaktig tröskelvärdet inställning är en lösning att återställa till standardvärdet eller ett rimliga tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="c8819-140">For an alert triggered due to improper threshold setting, a resolution is to reset to the default value or a more reasonable threshold value.</span></span>

![Återställer till standardvärdet eller ett rimliga tröskelvärde](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

<span data-ttu-id="c8819-142">Följande aviseringar kan tyda på problem med CPU företagsresurser:</span><span class="sxs-lookup"><span data-stu-id="c8819-142">The following alerts may indicate CPU resource problems:</span></span>

- <span data-ttu-id="c8819-143">Värd för CPU-användning (varning 5)</span><span class="sxs-lookup"><span data-stu-id="c8819-143">Host CPU Usage (Alert 5)</span></span>
- <span data-ttu-id="c8819-144">Senaste lagringspunktsåtgärd (varning 28)</span><span class="sxs-lookup"><span data-stu-id="c8819-144">Most recent savepoint operation (Alert 28)</span></span>
- <span data-ttu-id="c8819-145">Lagringspunkt varaktighet (varning 54)</span><span class="sxs-lookup"><span data-stu-id="c8819-145">Savepoint duration (Alert 54)</span></span>

<span data-ttu-id="c8819-146">Du kan upptäcka hög CPU-förbrukning på din SAP HANA-databas från något av följande:</span><span class="sxs-lookup"><span data-stu-id="c8819-146">You may notice high CPU consumption on your SAP HANA database from one of the following:</span></span>

- <span data-ttu-id="c8819-147">Aviseringen 5 (värd processoranvändning) aktiveras aktuella eller förfallna CPU-användning</span><span class="sxs-lookup"><span data-stu-id="c8819-147">Alert 5 (Host CPU usage) is raised for current or past CPU usage</span></span>
- <span data-ttu-id="c8819-148">Processoranvändningen visas på översiktsskärmen</span><span class="sxs-lookup"><span data-stu-id="c8819-148">The displayed CPU usage on the overview screen</span></span>

![CPU-användning visas på översiktsskärmen](./media/troubleshooting-monitoring/image3-cpu-usage.png)

<span data-ttu-id="c8819-150">Läs in diagrammet kan visa hög CPU-förbrukning eller hög förbrukning tidigare:</span><span class="sxs-lookup"><span data-stu-id="c8819-150">The Load graph might show high CPU consumption, or high consumption in the past:</span></span>

![Läs in diagrammet kan visa hög CPU-förbrukning eller hög förbrukning tidigare](./media/troubleshooting-monitoring/image4-load-graph.png)

<span data-ttu-id="c8819-152">En avisering aktiveras på grund av hög processoranvändning kan orsakas av olika orsaker, inklusive men inte begränsat till: körning av vissa transaktioner, datainläsning, hängande för jobb, långvariga SQL-satser och felaktiga frågeprestanda (till exempel med BW på HANA kuber).</span><span class="sxs-lookup"><span data-stu-id="c8819-152">An alert triggered due to high CPU utilization could be caused by several reasons, including, but not limited to: execution of certain transactions, data loading, hanging of jobs, long running SQL statements, and bad query performance (for example, with BW on HANA cubes).</span></span>

<span data-ttu-id="c8819-153">Referera till den [SAP HANA Felsökning: relaterade gör att CPU och lösningar](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) för detaljerad felsökning.</span><span class="sxs-lookup"><span data-stu-id="c8819-153">Refer to the [SAP HANA Troubleshooting: CPU Related Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="c8819-154">**Operativsystem**</span><span class="sxs-lookup"><span data-stu-id="c8819-154">**Operating System**</span></span>

<span data-ttu-id="c8819-155">En av de viktigaste kontrollerna för SAP HANA på Linux är att se till att Transparent stora sidor är inaktiverat, se [SAP Obs #2131662 – Transparent stora sidor (THP) på SAP HANA-servrar](https://launchpad.support.sap.com/#/notes/2131662).</span><span class="sxs-lookup"><span data-stu-id="c8819-155">One of the most important checks for SAP HANA on Linux is to make sure that Transparent Huge Pages are disabled, see [SAP Note #2131662 – Transparent Huge Pages (THP) on SAP HANA Servers](https://launchpad.support.sap.com/#/notes/2131662).</span></span>

- <span data-ttu-id="c8819-156">Du kan kontrollera om Transparent stora sidor aktiveras via följande Linux-kommando: **cat /sys/kernel/mm/transparent\_hugepage aktiveras**</span><span class="sxs-lookup"><span data-stu-id="c8819-156">You can check if Transparent Huge Pages are enabled through the following Linux command: **cat /sys/kernel/mm/transparent\_hugepage/enabled**</span></span>
- <span data-ttu-id="c8819-157">Om _alltid_ omges inom hakparenteser enligt nedan, betyder det att Transparent stora sidor är aktiverad: [alltid] madvise aldrig; om _aldrig_ omges inom hakparenteser enligt nedan, innebär det att Transparent enorma Sidor är inaktiverade: madvise alltid [aldrig]</span><span class="sxs-lookup"><span data-stu-id="c8819-157">If _always_ is enclosed in brackets as below, it means that the Transparent Huge Pages are enabled: [always] madvise never; if _never_ is enclosed in brackets as below, it means that the Transparent Huge Pages are disabled: always madvise [never]</span></span>

<span data-ttu-id="c8819-158">Följande Linux-kommandot ska returnera ingenting: **rpm - qa | grep kommandot ulimit.**</span><span class="sxs-lookup"><span data-stu-id="c8819-158">The following Linux command should return nothing: **rpm -qa | grep ulimit.**</span></span> <span data-ttu-id="c8819-159">Om det verkar som _kommandot ulimit_ är installerad avinstallerar du den omedelbart.</span><span class="sxs-lookup"><span data-stu-id="c8819-159">If it appears _ulimit_ is installed, uninstall it immediately.</span></span>

<span data-ttu-id="c8819-160">**Minne**</span><span class="sxs-lookup"><span data-stu-id="c8819-160">**Memory**</span></span>

<span data-ttu-id="c8819-161">Du kan se att mängden minne som allokerats av SAP HANA-databas är högre än förväntat.</span><span class="sxs-lookup"><span data-stu-id="c8819-161">You may observe that the amount of memory allocated by the SAP HANA database is higher than expected.</span></span> <span data-ttu-id="c8819-162">Följande aviseringarna indikerar problem med hög minnesanvändning:</span><span class="sxs-lookup"><span data-stu-id="c8819-162">The following alerts indicate issues with high memory usage:</span></span>

- <span data-ttu-id="c8819-163">Värden fysisk minnesanvändning (varning 1)</span><span class="sxs-lookup"><span data-stu-id="c8819-163">Host physical memory usage (Alert 1)</span></span>
- <span data-ttu-id="c8819-164">Minnesanvändning för namnserver (varning 12)</span><span class="sxs-lookup"><span data-stu-id="c8819-164">Memory usage of name server (Alert 12)</span></span>
- <span data-ttu-id="c8819-165">Totalt antal minnesanvändningen för kolumnen Store tabeller (varning 40)</span><span class="sxs-lookup"><span data-stu-id="c8819-165">Total memory usage of Column Store tables (Alert 40)</span></span>
- <span data-ttu-id="c8819-166">Minnesanvändning av tjänster (avisering 43)</span><span class="sxs-lookup"><span data-stu-id="c8819-166">Memory usage of services (Alert 43)</span></span>
- <span data-ttu-id="c8819-167">Mängden minne på huvudsakliga lagring av kolumn Store tabeller (varning 45)</span><span class="sxs-lookup"><span data-stu-id="c8819-167">Memory usage of main storage of Column Store tables (Alert 45)</span></span>
- <span data-ttu-id="c8819-168">Runtime dumpfiler (varning 46)</span><span class="sxs-lookup"><span data-stu-id="c8819-168">Runtime dump files (Alert 46)</span></span>

<span data-ttu-id="c8819-169">Referera till den [SAP HANA Felsökning: minnesproblem](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) för detaljerad felsökning.</span><span class="sxs-lookup"><span data-stu-id="c8819-169">Refer to the [SAP HANA Troubleshooting: Memory Problems](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="c8819-170">**Nätverk**</span><span class="sxs-lookup"><span data-stu-id="c8819-170">**Network**</span></span>

<span data-ttu-id="c8819-171">Referera till [SAP Obs #2081065 – felsökning av nätverk för SAP HANA](https://launchpad.support.sap.com/#/notes/2081065) och utföra felsökning i den här SAP-kommentar nätverk.</span><span class="sxs-lookup"><span data-stu-id="c8819-171">Refer to [SAP Note #2081065 – Troubleshooting SAP HANA Network](https://launchpad.support.sap.com/#/notes/2081065) and perform the network troubleshooting steps in this SAP Note.</span></span>

1. <span data-ttu-id="c8819-172">Analysera tidsfördröjningen mellan servern och klienten.</span><span class="sxs-lookup"><span data-stu-id="c8819-172">Analyzing round-trip time between server and client.</span></span>
  <span data-ttu-id="c8819-173">A.</span><span class="sxs-lookup"><span data-stu-id="c8819-173">A.</span></span> <span data-ttu-id="c8819-174">Kör SQL-skript [ _HANA\_nätverk\_klienter_](https://launchpad.support.sap.com/#/notes/1969700)_._</span><span class="sxs-lookup"><span data-stu-id="c8819-174">Run the SQL script [_HANA\_Network\_Clients_](https://launchpad.support.sap.com/#/notes/1969700)_._</span></span>
  
2. <span data-ttu-id="c8819-175">Analysera dessa kommunikation.</span><span class="sxs-lookup"><span data-stu-id="c8819-175">Analyze internode communication.</span></span>
  <span data-ttu-id="c8819-176">A.</span><span class="sxs-lookup"><span data-stu-id="c8819-176">A.</span></span> <span data-ttu-id="c8819-177">Kör SQL-skript [ _HANA\_nätverk\_Services_](https://launchpad.support.sap.com/#/notes/1969700)_._</span><span class="sxs-lookup"><span data-stu-id="c8819-177">Run SQL script [_HANA\_Network\_Services_](https://launchpad.support.sap.com/#/notes/1969700)_._</span></span>

3. <span data-ttu-id="c8819-178">Kör Linux-kommando **ifconfig** (utdata visar om paketförluster sker).</span><span class="sxs-lookup"><span data-stu-id="c8819-178">Run Linux command **ifconfig** (the output shows if any packet losses are occurring).</span></span>
4. <span data-ttu-id="c8819-179">Kör Linux-kommando **tcpdump**.</span><span class="sxs-lookup"><span data-stu-id="c8819-179">Run Linux command **tcpdump**.</span></span>

<span data-ttu-id="c8819-180">Använd öppen källkod [IPERF](https://iperf.fr/) verktyget (eller liknande) att mäta verkliga programmet nätverkets prestanda.</span><span class="sxs-lookup"><span data-stu-id="c8819-180">Also, use the open source [IPERF](https://iperf.fr/) tool (or similar) to measure real application network performance.</span></span>

<span data-ttu-id="c8819-181">Referera till den [SAP HANA Felsökning: nätverk prestanda och anslutningsproblem](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) för detaljerad felsökning.</span><span class="sxs-lookup"><span data-stu-id="c8819-181">Refer to the [SAP HANA Troubleshooting: Networking Performance and Connectivity Problems](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="c8819-182">**Storage**</span><span class="sxs-lookup"><span data-stu-id="c8819-182">**Storage**</span></span>

<span data-ttu-id="c8819-183">En ur slutanvändaren ett program (eller system som helhet) körs långsamt, svarar inte eller även kan verka låsa om det finns problem med i/o-prestanda.</span><span class="sxs-lookup"><span data-stu-id="c8819-183">From an end-user perspective, an application (or the system as a whole) runs sluggishly, is unresponsive, or can even seem to hang if there are issues with I/O performance.</span></span> <span data-ttu-id="c8819-184">I den **volymer** fliken i SAP HANA Studio visas anslutna volymer och vilka volymer som används av varje tjänst.</span><span class="sxs-lookup"><span data-stu-id="c8819-184">In the **Volumes** tab in SAP HANA Studio, you can see the attached volumes, and what volumes are used by each service.</span></span>

![I fliken volymer i SAP HANA Studio visas anslutna volymer och vilka volymer som används av varje tjänst](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

<span data-ttu-id="c8819-186">Anslutna volymer i den nedre delen av skärmen ser du information om volymer, till exempel filer och i/o-statistik.</span><span class="sxs-lookup"><span data-stu-id="c8819-186">Attached volumes in the lower part of the screen you can see details of the volumes, such as files and I/O statistics.</span></span>

![Anslutna volymer i den nedre delen av skärmen ser du information om volymer, till exempel filer och i/o-statistik](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

<span data-ttu-id="c8819-188">Referera till den [SAP HANA Felsökning: i/o relaterade rotorsaker och lösningar](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) och [SAP HANA Felsökning: Disk relaterade rotorsaker och lösningar](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) för detaljerad felsökning.</span><span class="sxs-lookup"><span data-stu-id="c8819-188">Refer to the [SAP HANA Troubleshooting: I/O Related Root Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) and [SAP HANA Troubleshooting: Disk Related Root Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="c8819-189">**Verktyg för Nätverksdiagnostik**</span><span class="sxs-lookup"><span data-stu-id="c8819-189">**Diagnostic Tools**</span></span>

<span data-ttu-id="c8819-190">Utföra en SAP HANA hälsokontroll via HANA\_Configuration\_Minichecks.</span><span class="sxs-lookup"><span data-stu-id="c8819-190">Perform an SAP HANA Health Check through HANA\_Configuration\_Minichecks.</span></span> <span data-ttu-id="c8819-191">Det här verktyget returnerar allvarliga tekniska problem som bör har redan aktiverats som aviseringar i SAP HANA Studio.</span><span class="sxs-lookup"><span data-stu-id="c8819-191">This tool returns potentially critical technical issues that should have already been raised as alerts in SAP HANA Studio.</span></span>

<span data-ttu-id="c8819-192">Referera till [SAP Obs #1969700 – SQL-instruktionen samlingen för SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) och hämta SQL Statements.zip-filen som bifogas anteckningen.</span><span class="sxs-lookup"><span data-stu-id="c8819-192">Refer to [SAP Note #1969700 – SQL statement collection for SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) and download the SQL Statements.zip file attached to that note.</span></span> <span data-ttu-id="c8819-193">Lagra den här ZIP-filen på den lokala hårddisken.</span><span class="sxs-lookup"><span data-stu-id="c8819-193">Store this .zip file on the local hard drive.</span></span>

<span data-ttu-id="c8819-194">I SAP HANA Studio på den **Systeminformation** , högerklickar på i den **namn** kolumn och markera **SQL importuttryck**.</span><span class="sxs-lookup"><span data-stu-id="c8819-194">In SAP HANA Studio, on the **System Information** tab, right-click in the **Name** column and select **Import SQL Statements**.</span></span>

![Högerklicka i kolumnen namn i SAP HANA Studio på fliken Systeminformation och välj Importera SQL-instruktioner](./media/troubleshooting-monitoring/image7-import-statements-a.png)

<span data-ttu-id="c8819-196">Välj SQL Statements.zip filen lagras lokalt och en mapp med motsvarande SQL-uttryck som ska importeras.</span><span class="sxs-lookup"><span data-stu-id="c8819-196">Select the SQL Statements.zip file stored locally, and a folder with the corresponding SQL statements will be imported.</span></span> <span data-ttu-id="c8819-197">Nu kan du köra många olika diagnostiska kontroller med de här SQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="c8819-197">At this point, the many different diagnostic checks can be run with these SQL statements.</span></span>

<span data-ttu-id="c8819-198">Till exempel om du vill testa SAP HANA System Replication krav på bandbredd, högerklickar du på den **bandbredd** instruktionen under **replikering: bandbredd** och välj **öppna** i SQL-konsolen.</span><span class="sxs-lookup"><span data-stu-id="c8819-198">For example, to test SAP HANA System Replication bandwidth requirements, right-click the **Bandwidth** statement under **Replication: Bandwidth** and select **Open** in SQL Console.</span></span>

<span data-ttu-id="c8819-199">Den fullständiga SQL-instruktionen öppnas så att indataparametrar (ändring avsnittet) har ändrats och därmed köras.</span><span class="sxs-lookup"><span data-stu-id="c8819-199">The complete SQL statement opens allowing input parameters (modification section) to be changed and then executed.</span></span>

![Den fullständiga SQL-instruktionen öppnar så att indataparametrar (ändring avsnittet) har ändrats och därmed köras](./media/troubleshooting-monitoring/image8-import-statements-b.png)

<span data-ttu-id="c8819-201">Ett annat exempel är Högerklicka på instruktionerna under **replikering: översikt över**.</span><span class="sxs-lookup"><span data-stu-id="c8819-201">Another example is right-clicking on the statements under **Replication: Overview**.</span></span> <span data-ttu-id="c8819-202">Välj **kör** på snabbmenyn:</span><span class="sxs-lookup"><span data-stu-id="c8819-202">Select **Execute** from the context menu:</span></span>

![Ett annat exempel är Högerklicka på instruktionerna under replikeringen: översikt.](./media/troubleshooting-monitoring/image9-import-statements-c.png)

<span data-ttu-id="c8819-205">Detta resulterar i information som hjälper med felsökning:</span><span class="sxs-lookup"><span data-stu-id="c8819-205">This results in information that helps with troubleshooting:</span></span>

![Detta leder till information som hjälper med felsökning](./media/troubleshooting-monitoring/image10-import-statements-d.png)

<span data-ttu-id="c8819-207">Gör likadant för HANA\_Configuration\_Minichecks och Sök efter alla _X_ markerar i den _C_ (kritisk)-kolumn.</span><span class="sxs-lookup"><span data-stu-id="c8819-207">Do the same for HANA\_Configuration\_Minichecks and check for any _X_ marks in the _C_ (Critical) column.</span></span>

<span data-ttu-id="c8819-208">Exempel utdata:</span><span class="sxs-lookup"><span data-stu-id="c8819-208">Sample outputs:</span></span>

<span data-ttu-id="c8819-209">**HANA\_Configuration\_MiniChecks\_Rev102.01 + 1** för allmän SAP HANA kontrollerar.</span><span class="sxs-lookup"><span data-stu-id="c8819-209">**HANA\_Configuration\_MiniChecks\_Rev102.01+1** for general SAP HANA checks.</span></span>

![HANA\_Configuration\_MiniChecks\_Rev102.01 + 1 för allmänna SAP HANA-kontroller](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

<span data-ttu-id="c8819-211">**HANA\_Services\_översikt** en översikt över vad SAP HANA tjänster som körs.</span><span class="sxs-lookup"><span data-stu-id="c8819-211">**HANA\_Services\_Overview** for an overview of what SAP HANA services are currently running.</span></span>

![HANA\_Services\_översikt för en översikt över vad SAP HANA tjänster som körs](./media/troubleshooting-monitoring/image12-services-overview.png)

<span data-ttu-id="c8819-213">**HANA\_Services\_statistik** för SAP HANA tjänstinformation (CPU, minne, osv.).</span><span class="sxs-lookup"><span data-stu-id="c8819-213">**HANA\_Services\_Statistics** for SAP HANA service information (CPU, memory, etc.).</span></span>

![<span data-ttu-id="c8819-214">HANA\_Services\_statistik för SAP HANA tjänstinformation</span><span class="sxs-lookup"><span data-stu-id="c8819-214">HANA\_Services\_Statistics for SAP HANA service information</span></span> ](./media/troubleshooting-monitoring/image13-services-statistics.png)

<span data-ttu-id="c8819-215">**HANA\_Configuration\_översikt\_Rev110 +** allmän information om SAP HANA-instans.</span><span class="sxs-lookup"><span data-stu-id="c8819-215">**HANA\_Configuration\_Overview\_Rev110+** for general information on the SAP HANA instance.</span></span>

![HANA\_Configuration\_översikt\_Rev110 + allmän information om SAP HANA-instans](./media/troubleshooting-monitoring/image14-configuration-overview.png)

<span data-ttu-id="c8819-217">**HANA\_Configuration\_parametrar\_Rev70 +** att kontrollera SAP HANA-parametrar.</span><span class="sxs-lookup"><span data-stu-id="c8819-217">**HANA\_Configuration\_Parameters\_Rev70+** to check SAP HANA parameters.</span></span>

![HANA\_Configuration\_parametrar\_Rev70 + för att kontrollera SAP HANA-parametrar](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

