---
title: "aaaTroubleshooting och övervakning av SAP HANA i Azure (stora instanser) | Microsoft Docs"
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
ms.openlocfilehash: 1f1cd35820e227fd99af495431cd4b826aa53600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-and-monitor-sap-hana-large-instances-on-azure"></a><span data-ttu-id="24c7c-103">Hur tootroubleshoot och övervaka SAP HANA (stora instanser) på Azure</span><span class="sxs-lookup"><span data-stu-id="24c7c-103">How tootroubleshoot and monitor SAP HANA (large instances) on Azure</span></span>


## <a name="monitoring-in-sap-hana-on-azure-large-instances"></a><span data-ttu-id="24c7c-104">Övervakning av SAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="24c7c-104">Monitoring in SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="24c7c-105">SAP HANA i Azure (stora instanser) skiljer sig inte från andra IaaS-distribution, behöver du toomonitor vad hello OS och hello programmet är gör och hur de använder hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="24c7c-105">SAP HANA on Azure (Large Instances) is no different from any other IaaS deployment — you need toomonitor what hello OS and hello application is doing and how these consume hello following resources:</span></span>

- <span data-ttu-id="24c7c-106">Processor</span><span class="sxs-lookup"><span data-stu-id="24c7c-106">CPU</span></span>
- <span data-ttu-id="24c7c-107">Minne</span><span class="sxs-lookup"><span data-stu-id="24c7c-107">Memory</span></span>
- <span data-ttu-id="24c7c-108">Nätverkets bandbredd</span><span class="sxs-lookup"><span data-stu-id="24c7c-108">Network bandwidth</span></span>
- <span data-ttu-id="24c7c-109">Diskutrymme</span><span class="sxs-lookup"><span data-stu-id="24c7c-109">Disk space</span></span>

<span data-ttu-id="24c7c-110">Som måste med Azure Virtual Machines toofigure ut om hello resursklasser med namnet ovan är tillräckligt eller om de få slut.</span><span class="sxs-lookup"><span data-stu-id="24c7c-110">As with Azure Virtual Machines you need toofigure out whether hello resource classes named above are sufficient, or whether these get depleted.</span></span> <span data-ttu-id="24c7c-111">Det finns mer information på varje hello olika klasser:</span><span class="sxs-lookup"><span data-stu-id="24c7c-111">Here is more detail on each of hello different classes:</span></span>

<span data-ttu-id="24c7c-112">**CPU-resursanvändningen:** hello som SAP som definierats för vissa arbetsbelastningar mot HANA är tvingande toomake till att det ska vara tillräckligt med CPU resurser tillgängliga toowork via hello data som lagras i minnet.</span><span class="sxs-lookup"><span data-stu-id="24c7c-112">**CPU resource consumption:** hello ratio that SAP defined for certain workload against HANA is enforced toomake sure that there should be enough CPU resources available toowork through hello data that is stored in memory.</span></span> <span data-ttu-id="24c7c-113">Det kan dock finnas fall där HANA förbrukar en stor mängd CPU köra frågor på grund av toomissing index eller liknande problem.</span><span class="sxs-lookup"><span data-stu-id="24c7c-113">Nevertheless, there might be cases where HANA consumes a lot of CPU executing queries due toomissing indexes or similar issues.</span></span> <span data-ttu-id="24c7c-114">Det innebär att du ska övervaka CPU-resursanvändningen hello HANA stora instans enhet samt CPU-resurser som används av hello specifika HANA tjänster.</span><span class="sxs-lookup"><span data-stu-id="24c7c-114">This means you should monitor CPU resource consumption of hello HANA large instance unit as well as CPU resources consumed by hello specific HANA services.</span></span>

<span data-ttu-id="24c7c-115">**Minnesförbrukning:** är viktiga toomonitor från inom HANA samt utanför HANA på hello enheten.</span><span class="sxs-lookup"><span data-stu-id="24c7c-115">**Memory consumption:** Is important toomonitor from within HANA, as well as outside of HANA on hello unit.</span></span> <span data-ttu-id="24c7c-116">Övervaka hur hello data förbrukar allokerat minne i ordning toostay inom hello krävs storleksanpassa riktlinjer för SAP HANA inom HANA.</span><span class="sxs-lookup"><span data-stu-id="24c7c-116">Within HANA, monitor how hello data is consuming HANA allocated memory in order toostay within hello required sizing guidelines of SAP.</span></span> <span data-ttu-id="24c7c-117">Du bör också toomonitor minnesförbrukning på hello stora instans nivå toomake till att ytterligare installerade icke-HANA programvara inte förbrukar för mycket minne och därför konkurrerar med HANA för minne.</span><span class="sxs-lookup"><span data-stu-id="24c7c-117">You also want toomonitor memory consumption on hello Large Instance level toomake sure that additional installed non-HANA software does not consume too much memory, and therefore compete with HANA for memory.</span></span>

<span data-ttu-id="24c7c-118">**Nätverkets bandbredd:** hello Azure VNet gateway har begränsad bandbredd på data flyttas till hello Azure VNet, så är det bra toomonitor hello data som tas emot av alla hello virtuella Azure-datorer inom ett VNet toofigure reda på hur nära du toohello gränserna för hello Azure gateway-SKU som du har valt.</span><span class="sxs-lookup"><span data-stu-id="24c7c-118">**Network bandwidth:** hello Azure VNet gateway is limited in bandwidth of data moving into hello Azure VNet, so it is helpful toomonitor hello data received by all hello Azure VMs within a VNet toofigure out how close you are toohello limits of hello Azure gateway SKU you selected.</span></span> <span data-ttu-id="24c7c-119">Det gör på hello HANA stora instans enheten meningsfullt toomonitor inkommande och utgående nätverkstrafik samt och tookeep spåra hello volymer som hanteras med tiden.</span><span class="sxs-lookup"><span data-stu-id="24c7c-119">On hello HANA Large Instance unit, it does make sense toomonitor incoming and outgoing network traffic as well, and tookeep track of hello volumes that are handled over time.</span></span>

<span data-ttu-id="24c7c-120">**Diskutrymme:** förbrukningen av diskutrymme vanligtvis ökar med tiden.</span><span class="sxs-lookup"><span data-stu-id="24c7c-120">**Disk space:** Disk space consumption usually increases over time.</span></span> <span data-ttu-id="24c7c-121">Det finns många orsaker, men de flesta av alla är: datavolym ökar, körning av säkerhetskopiering av transaktionsloggar, lagra spårningsfiler och utföra storage snapshots.</span><span class="sxs-lookup"><span data-stu-id="24c7c-121">There are many reasons for this, but most of all are: data volume increases, execution of transaction log backups, storing trace files, and performing storage snapshots.</span></span> <span data-ttu-id="24c7c-122">Därför är det viktigt toomonitor diskutrymmesanvändning och hantera hello diskutrymme som är associerade med hello HANA stora instans enhet.</span><span class="sxs-lookup"><span data-stu-id="24c7c-122">Therefore, it is important toomonitor disk space usage and manage hello disk space associated with hello HANA Large Instance unit.</span></span>

## <a name="monitoring-and-troubleshooting-from-hana-side"></a><span data-ttu-id="24c7c-123">Övervakning och felsökning från HANA sida</span><span class="sxs-lookup"><span data-stu-id="24c7c-123">Monitoring and troubleshooting from HANA side</span></span>

<span data-ttu-id="24c7c-124">I ordning tooeffectively analysera problem relaterade tooSAP HANA i Azure (stora instanser), är det användbart toonarrow ned hello orsaken till problemet.</span><span class="sxs-lookup"><span data-stu-id="24c7c-124">In order tooeffectively analyze problems related tooSAP HANA on Azure (Large Instances), it is useful toonarrow down hello root cause of a problem.</span></span> <span data-ttu-id="24c7c-125">SAP har publicerat en stor mängd dokumentationen toohelp du.</span><span class="sxs-lookup"><span data-stu-id="24c7c-125">SAP has published a large amount of documentation toohelp you.</span></span>

<span data-ttu-id="24c7c-126">Tillämpliga vanliga frågor och svar relaterade tooSAP HANA prestanda finns i följande SAP anteckningar hello:</span><span class="sxs-lookup"><span data-stu-id="24c7c-126">Applicable FAQs related tooSAP HANA performance can be found in hello following SAP Notes:</span></span>

- [<span data-ttu-id="24c7c-127">SAP-kommentar #2222200 – vanliga frågor och svar: SAP HANA-nätverk</span><span class="sxs-lookup"><span data-stu-id="24c7c-127">SAP Note #2222200 – FAQ: SAP HANA Network</span></span>](https://launchpad.support.sap.com/#/notes/2222200)
- [<span data-ttu-id="24c7c-128">SAP-kommentar #2100040 – vanliga frågor och svar: SAP HANA-processor</span><span class="sxs-lookup"><span data-stu-id="24c7c-128">SAP Note #2100040 – FAQ: SAP HANA CPU</span></span>](https://launchpad.support.sap.com/#/notes/0002100040)
- [<span data-ttu-id="24c7c-129">SAP-kommentar #199997 – vanliga frågor och svar: SAP HANA-minne</span><span class="sxs-lookup"><span data-stu-id="24c7c-129">SAP Note #199997 – FAQ: SAP HANA Memory</span></span>](https://launchpad.support.sap.com/#/notes/2177064)
- [<span data-ttu-id="24c7c-130">SAP-kommentar #200000 – vanliga frågor och svar: SAP HANA prestandaoptimering</span><span class="sxs-lookup"><span data-stu-id="24c7c-130">SAP Note #200000 – FAQ: SAP HANA Performance Optimization</span></span>](https://launchpad.support.sap.com/#/notes/2000000)
- [<span data-ttu-id="24c7c-131">SAP-kommentar #199930 – vanliga frågor och svar: SAP HANA-i/o analys</span><span class="sxs-lookup"><span data-stu-id="24c7c-131">SAP Note #199930 – FAQ: SAP HANA I/O Analysis</span></span>](https://launchpad.support.sap.com/#/notes/1999930)
- [<span data-ttu-id="24c7c-132">SAP-kommentar #2177064 – vanliga frågor och svar: SAP HANA-tjänsten startas om och kraschar</span><span class="sxs-lookup"><span data-stu-id="24c7c-132">SAP Note #2177064 – FAQ: SAP HANA Service Restart and Crashes</span></span>](https://launchpad.support.sap.com/#/notes/2177064)

<span data-ttu-id="24c7c-133">**SAP HANA aviseringar**</span><span class="sxs-lookup"><span data-stu-id="24c7c-133">**SAP HANA Alerts**</span></span>

<span data-ttu-id="24c7c-134">Som ett första steg loggarna hello aktuella SAP HANA avisering.</span><span class="sxs-lookup"><span data-stu-id="24c7c-134">As a first step, check hello current SAP HANA alert logs.</span></span> <span data-ttu-id="24c7c-135">SAP HANA Studio, gå för**administrationskonsolen: aviseringar: visa: alla aviseringar**.</span><span class="sxs-lookup"><span data-stu-id="24c7c-135">In SAP HANA Studio, go too**Administration Console: Alerts: Show: all alerts**.</span></span> <span data-ttu-id="24c7c-136">Den här fliken visas alla aviseringar för SAP HANA för specifika värden (fysiskt minne, CPU-användning osv.) som faller utanför hello lägsta och högsta tröskelvärden.</span><span class="sxs-lookup"><span data-stu-id="24c7c-136">This tab will show all SAP HANA alerts for specific values (free physical memory, CPU utilization, etc.) that fall outside of hello set minimum and maximum thresholds.</span></span> <span data-ttu-id="24c7c-137">Som standard är kontrollerar uppdateras automatiskt var 15: e minut.</span><span class="sxs-lookup"><span data-stu-id="24c7c-137">By default, checks are auto-refreshed every 15 minutes.</span></span>

![Gå tooAdministration konsolen i SAP HANA Studio: aviseringar: visa: alla aviseringar](./media/troubleshooting-monitoring/image1-show-alerts.png)

<span data-ttu-id="24c7c-139">**CPU**</span><span class="sxs-lookup"><span data-stu-id="24c7c-139">**CPU**</span></span>

<span data-ttu-id="24c7c-140">För en avisering som aktiveras på grund av tooimproper tröskelvärdet inställningen är en lösning tooreset toohello standardvärde eller ett rimliga tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="24c7c-140">For an alert triggered due tooimproper threshold setting, a resolution is tooreset toohello default value or a more reasonable threshold value.</span></span>

![Återställa toohello standardvärde eller ett rimliga tröskelvärde](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

<span data-ttu-id="24c7c-142">hello följande aviseringar kan tyda på problem med CPU företagsresurser:</span><span class="sxs-lookup"><span data-stu-id="24c7c-142">hello following alerts may indicate CPU resource problems:</span></span>

- <span data-ttu-id="24c7c-143">Värd för CPU-användning (varning 5)</span><span class="sxs-lookup"><span data-stu-id="24c7c-143">Host CPU Usage (Alert 5)</span></span>
- <span data-ttu-id="24c7c-144">Senaste lagringspunktsåtgärd (varning 28)</span><span class="sxs-lookup"><span data-stu-id="24c7c-144">Most recent savepoint operation (Alert 28)</span></span>
- <span data-ttu-id="24c7c-145">Lagringspunkt varaktighet (varning 54)</span><span class="sxs-lookup"><span data-stu-id="24c7c-145">Savepoint duration (Alert 54)</span></span>

<span data-ttu-id="24c7c-146">Du kan upptäcka hög CPU-förbrukning på din SAP HANA-databas från en av följande hello:</span><span class="sxs-lookup"><span data-stu-id="24c7c-146">You may notice high CPU consumption on your SAP HANA database from one of hello following:</span></span>

- <span data-ttu-id="24c7c-147">Aviseringen 5 (värd processoranvändning) aktiveras aktuella eller förfallna CPU-användning</span><span class="sxs-lookup"><span data-stu-id="24c7c-147">Alert 5 (Host CPU usage) is raised for current or past CPU usage</span></span>
- <span data-ttu-id="24c7c-148">hello visas CPU-användning på hello översiktsskärm</span><span class="sxs-lookup"><span data-stu-id="24c7c-148">hello displayed CPU usage on hello overview screen</span></span>

![Visas CPU-användning på hello översiktsskärm](./media/troubleshooting-monitoring/image3-cpu-usage.png)

<span data-ttu-id="24c7c-150">hello belastningen diagram kan visa hög CPU-förbrukning eller hög förbrukning i hello senaste:</span><span class="sxs-lookup"><span data-stu-id="24c7c-150">hello Load graph might show high CPU consumption, or high consumption in hello past:</span></span>

![hello belastningen diagram kan visa hög CPU-förbrukning eller hög förbrukning i hello senaste](./media/troubleshooting-monitoring/image4-load-graph.png)

<span data-ttu-id="24c7c-152">En avisering aktiveras på grund av toohigh CPU-användning kan orsakas av olika orsaker, inklusive men inte begränsat till: körning av vissa transaktioner, datainläsning, hängande för jobb, långvariga SQL-satser och felaktiga frågeprestanda (till exempel med BW på HANA kuber).</span><span class="sxs-lookup"><span data-stu-id="24c7c-152">An alert triggered due toohigh CPU utilization could be caused by several reasons, including, but not limited to: execution of certain transactions, data loading, hanging of jobs, long running SQL statements, and bad query performance (for example, with BW on HANA cubes).</span></span>

<span data-ttu-id="24c7c-153">Se toohello [SAP HANA Felsökning: relaterade gör att CPU och lösningar](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) för detaljerad felsökning.</span><span class="sxs-lookup"><span data-stu-id="24c7c-153">Refer toohello [SAP HANA Troubleshooting: CPU Related Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="24c7c-154">**Operativsystem**</span><span class="sxs-lookup"><span data-stu-id="24c7c-154">**Operating System**</span></span>

<span data-ttu-id="24c7c-155">En av de viktigaste hello kontrollerar för SAP HANA på Linux är toomake till att Transparent stora sidor är inaktiverat, se [SAP Obs #2131662 – Transparent stora sidor (THP) på SAP HANA-servrar](https://launchpad.support.sap.com/#/notes/2131662).</span><span class="sxs-lookup"><span data-stu-id="24c7c-155">One of hello most important checks for SAP HANA on Linux is toomake sure that Transparent Huge Pages are disabled, see [SAP Note #2131662 – Transparent Huge Pages (THP) on SAP HANA Servers](https://launchpad.support.sap.com/#/notes/2131662).</span></span>

- <span data-ttu-id="24c7c-156">Du kan kontrollera om Transparent stora sidor aktiveras via hello följande Linux-kommando: **cat /sys/kernel/mm/transparent\_hugepage aktiveras**</span><span class="sxs-lookup"><span data-stu-id="24c7c-156">You can check if Transparent Huge Pages are enabled through hello following Linux command: **cat /sys/kernel/mm/transparent\_hugepage/enabled**</span></span>
- <span data-ttu-id="24c7c-157">Om _alltid_ omges inom hakparenteser enligt nedan, betyder det att hello Transparent stora sidor är aktiverad: [alltid] madvise aldrig; om _aldrig_ omges inom hakparenteser enligt nedan, innebär det att hello Transparent Stora sidor är inaktiverade: madvise alltid [aldrig]</span><span class="sxs-lookup"><span data-stu-id="24c7c-157">If _always_ is enclosed in brackets as below, it means that hello Transparent Huge Pages are enabled: [always] madvise never; if _never_ is enclosed in brackets as below, it means that hello Transparent Huge Pages are disabled: always madvise [never]</span></span>

<span data-ttu-id="24c7c-158">följande Linux-kommando hello ska returnera ingenting: **rpm - qa | grep kommandot ulimit.**</span><span class="sxs-lookup"><span data-stu-id="24c7c-158">hello following Linux command should return nothing: **rpm -qa | grep ulimit.**</span></span> <span data-ttu-id="24c7c-159">Om det verkar som _kommandot ulimit_ är installerad avinstallerar du den omedelbart.</span><span class="sxs-lookup"><span data-stu-id="24c7c-159">If it appears _ulimit_ is installed, uninstall it immediately.</span></span>

<span data-ttu-id="24c7c-160">**Minne**</span><span class="sxs-lookup"><span data-stu-id="24c7c-160">**Memory**</span></span>

<span data-ttu-id="24c7c-161">Du kan se detta hello belopp minne som allokerats av hello SAP HANA-databasen är högre än förväntat.</span><span class="sxs-lookup"><span data-stu-id="24c7c-161">You may observe that hello amount of memory allocated by hello SAP HANA database is higher than expected.</span></span> <span data-ttu-id="24c7c-162">hello följande aviseringar tyda på problem med hög minnesanvändning:</span><span class="sxs-lookup"><span data-stu-id="24c7c-162">hello following alerts indicate issues with high memory usage:</span></span>

- <span data-ttu-id="24c7c-163">Värden fysisk minnesanvändning (varning 1)</span><span class="sxs-lookup"><span data-stu-id="24c7c-163">Host physical memory usage (Alert 1)</span></span>
- <span data-ttu-id="24c7c-164">Minnesanvändning för namnserver (varning 12)</span><span class="sxs-lookup"><span data-stu-id="24c7c-164">Memory usage of name server (Alert 12)</span></span>
- <span data-ttu-id="24c7c-165">Totalt antal minnesanvändningen för kolumnen Store tabeller (varning 40)</span><span class="sxs-lookup"><span data-stu-id="24c7c-165">Total memory usage of Column Store tables (Alert 40)</span></span>
- <span data-ttu-id="24c7c-166">Minnesanvändning av tjänster (avisering 43)</span><span class="sxs-lookup"><span data-stu-id="24c7c-166">Memory usage of services (Alert 43)</span></span>
- <span data-ttu-id="24c7c-167">Mängden minne på huvudsakliga lagring av kolumn Store tabeller (varning 45)</span><span class="sxs-lookup"><span data-stu-id="24c7c-167">Memory usage of main storage of Column Store tables (Alert 45)</span></span>
- <span data-ttu-id="24c7c-168">Runtime dumpfiler (varning 46)</span><span class="sxs-lookup"><span data-stu-id="24c7c-168">Runtime dump files (Alert 46)</span></span>

<span data-ttu-id="24c7c-169">Se toohello [SAP HANA Felsökning: minnesproblem](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) för detaljerad felsökning.</span><span class="sxs-lookup"><span data-stu-id="24c7c-169">Refer toohello [SAP HANA Troubleshooting: Memory Problems](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="24c7c-170">**Nätverk**</span><span class="sxs-lookup"><span data-stu-id="24c7c-170">**Network**</span></span>

<span data-ttu-id="24c7c-171">Se för[SAP Obs #2081065 – felsökning av nätverk för SAP HANA](https://launchpad.support.sap.com/#/notes/2081065) och utföra felsökning i den här SAP-kommentar hello-nätverk.</span><span class="sxs-lookup"><span data-stu-id="24c7c-171">Refer too[SAP Note #2081065 – Troubleshooting SAP HANA Network](https://launchpad.support.sap.com/#/notes/2081065) and perform hello network troubleshooting steps in this SAP Note.</span></span>

1. <span data-ttu-id="24c7c-172">Analysera tidsfördröjningen mellan servern och klienten.</span><span class="sxs-lookup"><span data-stu-id="24c7c-172">Analyzing round-trip time between server and client.</span></span>
  <span data-ttu-id="24c7c-173">A.</span><span class="sxs-lookup"><span data-stu-id="24c7c-173">A.</span></span> <span data-ttu-id="24c7c-174">Kör SQL-skript för hello [ _HANA\_nätverk\_klienter_](https://launchpad.support.sap.com/#/notes/1969700)_._</span><span class="sxs-lookup"><span data-stu-id="24c7c-174">Run hello SQL script [_HANA\_Network\_Clients_](https://launchpad.support.sap.com/#/notes/1969700)_._</span></span>
  
2. <span data-ttu-id="24c7c-175">Analysera dessa kommunikation.</span><span class="sxs-lookup"><span data-stu-id="24c7c-175">Analyze internode communication.</span></span>
  <span data-ttu-id="24c7c-176">A.</span><span class="sxs-lookup"><span data-stu-id="24c7c-176">A.</span></span> <span data-ttu-id="24c7c-177">Kör SQL-skript [ _HANA\_nätverk\_Services_](https://launchpad.support.sap.com/#/notes/1969700)_._</span><span class="sxs-lookup"><span data-stu-id="24c7c-177">Run SQL script [_HANA\_Network\_Services_](https://launchpad.support.sap.com/#/notes/1969700)_._</span></span>

3. <span data-ttu-id="24c7c-178">Kör Linux-kommando **ifconfig** (hello utdata visar om paketförluster sker).</span><span class="sxs-lookup"><span data-stu-id="24c7c-178">Run Linux command **ifconfig** (hello output shows if any packet losses are occurring).</span></span>
4. <span data-ttu-id="24c7c-179">Kör Linux-kommando **tcpdump**.</span><span class="sxs-lookup"><span data-stu-id="24c7c-179">Run Linux command **tcpdump**.</span></span>

<span data-ttu-id="24c7c-180">Du kan också använda hello öppen källkod [IPERF](https://iperf.fr/) verktyget (eller liknande) toomeasure verkliga programmet nätverkets prestanda.</span><span class="sxs-lookup"><span data-stu-id="24c7c-180">Also, use hello open source [IPERF](https://iperf.fr/) tool (or similar) toomeasure real application network performance.</span></span>

<span data-ttu-id="24c7c-181">Se toohello [SAP HANA Felsökning: nätverk prestanda och anslutningsproblem](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) för detaljerad felsökning.</span><span class="sxs-lookup"><span data-stu-id="24c7c-181">Refer toohello [SAP HANA Troubleshooting: Networking Performance and Connectivity Problems](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="24c7c-182">**Storage**</span><span class="sxs-lookup"><span data-stu-id="24c7c-182">**Storage**</span></span>

<span data-ttu-id="24c7c-183">En ur slutanvändaren ett program (eller hello system som helhet) körs långsamt, svarar inte eller kan även verkar vara toohang om det finns problem med i/o-prestanda.</span><span class="sxs-lookup"><span data-stu-id="24c7c-183">From an end-user perspective, an application (or hello system as a whole) runs sluggishly, is unresponsive, or can even seem toohang if there are issues with I/O performance.</span></span> <span data-ttu-id="24c7c-184">I hello **volymer** fliken i SAP HANA Studio kan du se hello anslutna volymer och vilka volymer som används av varje tjänst.</span><span class="sxs-lookup"><span data-stu-id="24c7c-184">In hello **Volumes** tab in SAP HANA Studio, you can see hello attached volumes, and what volumes are used by each service.</span></span>

![Hello volymer på fliken i SAP HANA Studio finns hello anslutna volymer och vilka volymer som används av varje tjänst](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

<span data-ttu-id="24c7c-186">Anslutna volymer i hello nedre delen av du hittar information om hello-skärmen hello volymer, till exempel filer och i/o-statistik.</span><span class="sxs-lookup"><span data-stu-id="24c7c-186">Attached volumes in hello lower part of hello screen you can see details of hello volumes, such as files and I/O statistics.</span></span>

![Anslutna volymer i hello nedre delen av du hittar information om hello-skärmen hello volymer, till exempel filer och i/o-statistik](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

<span data-ttu-id="24c7c-188">Se toohello [SAP HANA Felsökning: i/o relaterade rotorsaker och lösningar](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) och [SAP HANA Felsökning: Disk relaterade rotorsaker och lösningar](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) för detaljerad felsökning.</span><span class="sxs-lookup"><span data-stu-id="24c7c-188">Refer toohello [SAP HANA Troubleshooting: I/O Related Root Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) and [SAP HANA Troubleshooting: Disk Related Root Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="24c7c-189">**Verktyg för Nätverksdiagnostik**</span><span class="sxs-lookup"><span data-stu-id="24c7c-189">**Diagnostic Tools**</span></span>

<span data-ttu-id="24c7c-190">Utföra en SAP HANA hälsokontroll via HANA\_Configuration\_Minichecks.</span><span class="sxs-lookup"><span data-stu-id="24c7c-190">Perform an SAP HANA Health Check through HANA\_Configuration\_Minichecks.</span></span> <span data-ttu-id="24c7c-191">Det här verktyget returnerar allvarliga tekniska problem som bör har redan aktiverats som aviseringar i SAP HANA Studio.</span><span class="sxs-lookup"><span data-stu-id="24c7c-191">This tool returns potentially critical technical issues that should have already been raised as alerts in SAP HANA Studio.</span></span>

<span data-ttu-id="24c7c-192">Se för[SAP Obs #1969700 – SQL-instruktionen samlingen för SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) och hämta hello SQL Statements.zip filen bifogade toothat anteckning.</span><span class="sxs-lookup"><span data-stu-id="24c7c-192">Refer too[SAP Note #1969700 – SQL statement collection for SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) and download hello SQL Statements.zip file attached toothat note.</span></span> <span data-ttu-id="24c7c-193">Lagra den här ZIP-filen på hello lokala hårddisken.</span><span class="sxs-lookup"><span data-stu-id="24c7c-193">Store this .zip file on hello local hard drive.</span></span>

<span data-ttu-id="24c7c-194">I SAP HANA Studio på hello **Systeminformation** , högerklickar på i hello **namn** kolumn och markera **SQL importuttryck**.</span><span class="sxs-lookup"><span data-stu-id="24c7c-194">In SAP HANA Studio, on hello **System Information** tab, right-click in hello **Name** column and select **Import SQL Statements**.</span></span>

![Högerklicka i hello namnkolumnen i SAP HANA Studio hello Systeminformation på fliken och markera Importera SQL-instruktioner](./media/troubleshooting-monitoring/image7-import-statements-a.png)

<span data-ttu-id="24c7c-196">Välj hello SQL Statements.zip filen lagras lokalt och en mapp med hello motsvarande SQL-uttryck som ska importeras.</span><span class="sxs-lookup"><span data-stu-id="24c7c-196">Select hello SQL Statements.zip file stored locally, and a folder with hello corresponding SQL statements will be imported.</span></span> <span data-ttu-id="24c7c-197">Nu hello många olika diagnostiska kontroller kan köras med de här SQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="24c7c-197">At this point, hello many different diagnostic checks can be run with these SQL statements.</span></span>

<span data-ttu-id="24c7c-198">Till exempel tootest bandbreddskraven för SAP HANA System replikering, högerklicka på hello **bandbredd** instruktionen under **replikering: bandbredd** och välj **öppna** i SQL-konsolen.</span><span class="sxs-lookup"><span data-stu-id="24c7c-198">For example, tootest SAP HANA System Replication bandwidth requirements, right-click hello **Bandwidth** statement under **Replication: Bandwidth** and select **Open** in SQL Console.</span></span>

<span data-ttu-id="24c7c-199">hello fullständig SQL-sats öppnas indataparametrar (ändring avsnittet) vilket gör att toobe ändrats och därmed köras.</span><span class="sxs-lookup"><span data-stu-id="24c7c-199">hello complete SQL statement opens allowing input parameters (modification section) toobe changed and then executed.</span></span>

![hello fullständig SQL-sats öppnar indataparametrar (ändring avsnittet) vilket gör att toobe ändras och sedan köras](./media/troubleshooting-monitoring/image8-import-statements-b.png)

<span data-ttu-id="24c7c-201">Ett annat exempel är Högerklicka på hello instruktioner under **replikering: översikt över**.</span><span class="sxs-lookup"><span data-stu-id="24c7c-201">Another example is right-clicking on hello statements under **Replication: Overview**.</span></span> <span data-ttu-id="24c7c-202">Välj **Execute** hello snabbmenyn:</span><span class="sxs-lookup"><span data-stu-id="24c7c-202">Select **Execute** from hello context menu:</span></span>

![Ett annat exempel är Högerklicka på hello instruktioner under replikeringen: översikt.](./media/troubleshooting-monitoring/image9-import-statements-c.png)

<span data-ttu-id="24c7c-205">Detta resulterar i information som hjälper med felsökning:</span><span class="sxs-lookup"><span data-stu-id="24c7c-205">This results in information that helps with troubleshooting:</span></span>

![Detta leder till information som hjälper med felsökning](./media/troubleshooting-monitoring/image10-import-statements-d.png)

<span data-ttu-id="24c7c-207">Hello samma för HANA\_Configuration\_Minichecks och Sök efter alla _X_ märken i hello _C_ (kritisk)-kolumn.</span><span class="sxs-lookup"><span data-stu-id="24c7c-207">Do hello same for HANA\_Configuration\_Minichecks and check for any _X_ marks in hello _C_ (Critical) column.</span></span>

<span data-ttu-id="24c7c-208">Exempel utdata:</span><span class="sxs-lookup"><span data-stu-id="24c7c-208">Sample outputs:</span></span>

<span data-ttu-id="24c7c-209">**HANA\_Configuration\_MiniChecks\_Rev102.01 + 1** för allmän SAP HANA kontrollerar.</span><span class="sxs-lookup"><span data-stu-id="24c7c-209">**HANA\_Configuration\_MiniChecks\_Rev102.01+1** for general SAP HANA checks.</span></span>

![HANA\_Configuration\_MiniChecks\_Rev102.01 + 1 för allmänna SAP HANA-kontroller](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

<span data-ttu-id="24c7c-211">**HANA\_Services\_översikt** en översikt över vad SAP HANA tjänster som körs.</span><span class="sxs-lookup"><span data-stu-id="24c7c-211">**HANA\_Services\_Overview** for an overview of what SAP HANA services are currently running.</span></span>

![HANA\_Services\_översikt för en översikt över vad SAP HANA tjänster som körs](./media/troubleshooting-monitoring/image12-services-overview.png)

<span data-ttu-id="24c7c-213">**HANA\_Services\_statistik** för SAP HANA tjänstinformation (CPU, minne, osv.).</span><span class="sxs-lookup"><span data-stu-id="24c7c-213">**HANA\_Services\_Statistics** for SAP HANA service information (CPU, memory, etc.).</span></span>

![<span data-ttu-id="24c7c-214">HANA\_Services\_statistik för SAP HANA tjänstinformation</span><span class="sxs-lookup"><span data-stu-id="24c7c-214">HANA\_Services\_Statistics for SAP HANA service information</span></span> ](./media/troubleshooting-monitoring/image13-services-statistics.png)

<span data-ttu-id="24c7c-215">**HANA\_Configuration\_översikt\_Rev110 +** allmän information om hello SAP HANA-instans.</span><span class="sxs-lookup"><span data-stu-id="24c7c-215">**HANA\_Configuration\_Overview\_Rev110+** for general information on hello SAP HANA instance.</span></span>

![HANA\_Configuration\_översikt\_Rev110 + allmän information om hello SAP HANA-instans](./media/troubleshooting-monitoring/image14-configuration-overview.png)

<span data-ttu-id="24c7c-217">**HANA\_Configuration\_parametrar\_Rev70 +** toocheck SAP HANA-parametrar.</span><span class="sxs-lookup"><span data-stu-id="24c7c-217">**HANA\_Configuration\_Parameters\_Rev70+** toocheck SAP HANA parameters.</span></span>

![HANA\_Configuration\_parametrar\_Rev70 + toocheck SAP HANA-parametrar](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

