---
title: "aaaWire lösning i Log Analytics | Microsoft Docs"
description: "Wire-data är konsoliderade nätverks- och data från datorer med OMS-agenter, inklusive Operations Manager och Windows-anslutna agenter. Nätverksdata kombineras med din logg data toohelp du korrelera data."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: fc3d7127-0baa-4772-858a-5ba995d1519b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: banders
ms.openlocfilehash: adafdf98dfbda9d87759643a1a606a84eafd1348
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="wire-data-20-preview-solution-in-log-analytics"></a><span data-ttu-id="253ad-104">Överföring Data 2.0 (förhandsgranskning) lösning i logganalys</span><span class="sxs-lookup"><span data-stu-id="253ad-104">Wire Data 2.0 (Preview) solution in Log Analytics</span></span>

![Överföring Data symbol](./media/log-analytics-wire-data/wire-data2-symbol.png)

<span data-ttu-id="253ad-106">Wire-data är konsoliderade nätverks- och data från datorer med OMS-agenter, inklusive Operations Manager, Windows-ansluten och Linux-agenter.</span><span class="sxs-lookup"><span data-stu-id="253ad-106">Wire data is consolidated network and performance data from computers with OMS agents, including Operations Manager, Windows-connected, and Linux agents.</span></span> <span data-ttu-id="253ad-107">Nätverksdata kombineras med dina andra logga data toohelp du korrelera data.</span><span class="sxs-lookup"><span data-stu-id="253ad-107">Network data is combined with your other log data toohelp you correlate data.</span></span>

<span data-ttu-id="253ad-108">Dessutom tooOMS agenter hello Wire-Data lösningen använder Microsoft Dependency agenter som installeras på datorer i din IT-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="253ad-108">In addition tooOMS agents, hello Wire Data solution uses Microsoft Dependency agents that you install on computers in your IT infrastructure.</span></span> <span data-ttu-id="253ad-109">Beroende agenter Övervakare nätverk data som skickas tooand från datorer för nätverket nivåer 2-3 i hello [OSI-modell](https://en.wikipedia.org/wiki/OSI_model), inklusive hello olika protokoll och portar som används.</span><span class="sxs-lookup"><span data-stu-id="253ad-109">Dependency Agents monitor network data sent tooand from your computers for network levels 2-3 in hello [OSI model](https://en.wikipedia.org/wiki/OSI_model), including hello various protocols and ports used.</span></span> <span data-ttu-id="253ad-110">Informationen skickas sedan tooLog Analytics använder agenter.</span><span class="sxs-lookup"><span data-stu-id="253ad-110">Data is then sent tooLog Analytics using agents.</span></span>

> [!NOTE]
> <span data-ttu-id="253ad-111">Du kan inte lägga till hello tidigare version av hello Wire-Data lösning toonew arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="253ad-111">You cannot add hello previous version of hello Wire Data solution toonew workspaces.</span></span> <span data-ttu-id="253ad-112">Om du har hello ursprungliga Wire-Data-lösning aktiverat kan du fortsätta toouse den.</span><span class="sxs-lookup"><span data-stu-id="253ad-112">If you have hello original Wire Data solution enabled, you can continue toouse it.</span></span> <span data-ttu-id="253ad-113">Dock toouse överföring Data 2.0, måste du först ta bort hello originalversionen.</span><span class="sxs-lookup"><span data-stu-id="253ad-113">However, toouse Wire Data 2.0, you must first remove hello original version.</span></span>

<span data-ttu-id="253ad-114">Som standard logganalys samlar in loggdata för CPU, minne, disk och nätverk prestandadata från räknare som är inbyggd i Windows.</span><span class="sxs-lookup"><span data-stu-id="253ad-114">By default, Log Analytics collects logged data for CPU, memory, disk, and network performance data from counters built into Windows.</span></span> <span data-ttu-id="253ad-115">Nätverk och andra datainsamling görs realtid för varje agent, inklusive undernät och programnivå protokoll som används av hello-dator.</span><span class="sxs-lookup"><span data-stu-id="253ad-115">Network and other data collection is done in real-time for each agent, including subnets and application-level protocols being used by hello computer.</span></span> <span data-ttu-id="253ad-116">Du kan lägga till andra prestandaräknare på inställningssidan för hello på hello-loggar.</span><span class="sxs-lookup"><span data-stu-id="253ad-116">You can add other performance counters on hello Settings page on hello Logs tab.</span></span>

<span data-ttu-id="253ad-117">Om du har använt [sFlow](http://www.sflow.org/) eller annan programvara med [Ciscos NetFlow protokollet](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html)hello statistik och data som du ser från wire-data kommer att bekant tooyou.</span><span class="sxs-lookup"><span data-stu-id="253ad-117">If you've used [sFlow](http://www.sflow.org/) or other software with [Cisco's NetFlow protocol](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), then hello statistics and data you see from wire data will be familiar tooyou.</span></span>

<span data-ttu-id="253ad-118">Hello typer av inbyggda loggen sökfrågor bland annat:</span><span class="sxs-lookup"><span data-stu-id="253ad-118">Some of hello types of built-in Log search queries include:</span></span>

- <span data-ttu-id="253ad-119">Agenter som tillhandahåller wire-data</span><span class="sxs-lookup"><span data-stu-id="253ad-119">Agents that provide wire data</span></span>
- <span data-ttu-id="253ad-120">IP-adressen för agenter som tillhandahåller wire-data</span><span class="sxs-lookup"><span data-stu-id="253ad-120">IP address of agents providing wire data</span></span>
- <span data-ttu-id="253ad-121">Utgående kommunikation via IP-adresser</span><span class="sxs-lookup"><span data-stu-id="253ad-121">Outbound communications by IP addresses</span></span>
- <span data-ttu-id="253ad-122">Antal byte skickade efter programprotokoll</span><span class="sxs-lookup"><span data-stu-id="253ad-122">Number of bytes sent by application protocols</span></span>
- <span data-ttu-id="253ad-123">Antalet byte som skickats av en programtjänsten</span><span class="sxs-lookup"><span data-stu-id="253ad-123">Number of bytes sent by an application service</span></span>
- <span data-ttu-id="253ad-124">Byte mottagna efter olika protokoll</span><span class="sxs-lookup"><span data-stu-id="253ad-124">Bytes received by different protocols</span></span>
- <span data-ttu-id="253ad-125">Totalt antal byte som skickas och tas emot av IP-version</span><span class="sxs-lookup"><span data-stu-id="253ad-125">Total bytes sent and received by IP version</span></span>
- <span data-ttu-id="253ad-126">Genomsnittlig svarstid för anslutningar som har ett tillförlitligt sätt</span><span class="sxs-lookup"><span data-stu-id="253ad-126">Average latency for connections that were measured reliably</span></span>
- <span data-ttu-id="253ad-127">Datorn bearbetar som initierat eller mottagit nätverkstrafik</span><span class="sxs-lookup"><span data-stu-id="253ad-127">Computer processes that initiated or received network traffic</span></span>
- <span data-ttu-id="253ad-128">Mängden nätverkstrafik för en process</span><span class="sxs-lookup"><span data-stu-id="253ad-128">Amount of network traffic for a process</span></span>

<span data-ttu-id="253ad-129">När du söker med wire-data, kan du filtrera och data tooview gruppinformation hello översta agenter och övre protokoll.</span><span class="sxs-lookup"><span data-stu-id="253ad-129">When you search using wire data, you can filter and group data tooview information about hello top agents and top protocols.</span></span> <span data-ttu-id="253ad-130">Eller så kan du visa när vissa datorer (IP-adresser/MAC-adresser) kommunicerat med varandra, hur lång tid och hur mycket data skickades – i princip du visa metadata om nätverkstrafik som är Sökbaserade.</span><span class="sxs-lookup"><span data-stu-id="253ad-130">Or you can view when certain computers (IP addresses/MAC addresses) communicated with each other, for how long, and how much data was sent—basically, you view metadata about network traffic, which is search-based.</span></span>

<span data-ttu-id="253ad-131">Eftersom du visar metadata, är det dock inte nödvändigtvis användbart för avancerad felsökning.</span><span class="sxs-lookup"><span data-stu-id="253ad-131">However, since you're viewing metadata, it's not necessarily useful for in-depth troubleshooting.</span></span> <span data-ttu-id="253ad-132">Wire-data i logganalys är inte en fullständig insamlingen av nätverksdata.</span><span class="sxs-lookup"><span data-stu-id="253ad-132">Wire data in Log Analytics is not a full capture of network data.</span></span> <span data-ttu-id="253ad-133">Så är det inte avsedd för djupt paketnivå felsökning.</span><span class="sxs-lookup"><span data-stu-id="253ad-133">So, it's not intended for deep packet-level troubleshooting.</span></span> <span data-ttu-id="253ad-134">Hej fördelen med att använda hello agent, jämfört med tooother samling metoder, är att du inte har tooinstall installationer, konfigurera om dina nätverksväxlar eller utför komplicerade konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="253ad-134">hello advantage of using hello agent, compared tooother collection methods, is that you don't have tooinstall appliances, reconfigure your network switches, or preform complicated configurations.</span></span> <span data-ttu-id="253ad-135">Wire-data är helt enkelt agent-baserad – hello agent installeras på en dator och den ska övervaka sin egen nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="253ad-135">Wire data is simply agent-based—you install hello agent on a computer and it will monitor its own network traffic.</span></span> <span data-ttu-id="253ad-136">En annan fördel är om du vill toomonitor arbetsbelastningar som körs i molntjänstleverantörer eller värdleverantör eller Microsoft Azure, där hello användaren inte äger hello fabric lager.</span><span class="sxs-lookup"><span data-stu-id="253ad-136">Another advantage is when you want toomonitor workloads running in cloud providers or hosting service provider or Microsoft Azure, where hello user doesn't own hello fabric layer.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="253ad-137">Anslutna källor</span><span class="sxs-lookup"><span data-stu-id="253ad-137">Connected sources</span></span>

<span data-ttu-id="253ad-138">Wire-Data hämtar data från hello Microsoft Beroendeagent.</span><span class="sxs-lookup"><span data-stu-id="253ad-138">Wire Data gets its data from hello Microsoft Dependency Agent.</span></span> <span data-ttu-id="253ad-139">Hej Beroendeagent är beroende av hello OMS-Agent för dess anslutningar tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="253ad-139">hello Dependency Agent depends on hello OMS Agent for its connections tooLog Analytics.</span></span> <span data-ttu-id="253ad-140">Detta innebär att en server måste ha hello OMS-Agent installeras och konfigureras först och sedan installerar du hello Beroendeagent.</span><span class="sxs-lookup"><span data-stu-id="253ad-140">This means that a server must have hello OMS Agent installed and configured first, and then you install hello Dependency Agent.</span></span> <span data-ttu-id="253ad-141">hello beskrivs följande tabell hello anslutna källor som hello Wire-Data-lösningen stöder.</span><span class="sxs-lookup"><span data-stu-id="253ad-141">hello following table describes hello connected sources that hello Wire Data solution supports.</span></span>

| <span data-ttu-id="253ad-142">**Ansluten datakälla**</span><span class="sxs-lookup"><span data-stu-id="253ad-142">**Connected source**</span></span> | <span data-ttu-id="253ad-143">**Stöds**</span><span class="sxs-lookup"><span data-stu-id="253ad-143">**Supported**</span></span> | <span data-ttu-id="253ad-144">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="253ad-144">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="253ad-145">Windows-agenter</span><span class="sxs-lookup"><span data-stu-id="253ad-145">Windows agents</span></span> | <span data-ttu-id="253ad-146">Ja</span><span class="sxs-lookup"><span data-stu-id="253ad-146">Yes</span></span> | <span data-ttu-id="253ad-147">Wire-Data analyserar och samlar in data från datorer med Windows-agenten.</span><span class="sxs-lookup"><span data-stu-id="253ad-147">Wire Data analyzes and collects data from Windows agent computers.</span></span> <br><br> <span data-ttu-id="253ad-148">I tillägg toohello [OMS-Agent](log-analytics-windows-agents.md), Windows-agenter kräver hello Microsoft Beroendeagent.</span><span class="sxs-lookup"><span data-stu-id="253ad-148">In addition toohello [OMS Agent](log-analytics-windows-agents.md), Windows agents require hello Microsoft Dependency Agent.</span></span> <span data-ttu-id="253ad-149">Se hello [operativsystem](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) för en fullständig lista över versioner av operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="253ad-149">See hello [supported operating systems](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) for a complete list of operating system versions.</span></span> |
| <span data-ttu-id="253ad-150">Linux-agenter</span><span class="sxs-lookup"><span data-stu-id="253ad-150">Linux agents</span></span> | <span data-ttu-id="253ad-151">Ja</span><span class="sxs-lookup"><span data-stu-id="253ad-151">Yes</span></span> | <span data-ttu-id="253ad-152">Wire-Data analyserar och samlar in data från datorer för Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="253ad-152">Wire Data analyzes and collects data from Linux agent computers.</span></span><br><br> <span data-ttu-id="253ad-153">I tillägg toohello [OMS-Agent](log-analytics-linux-agents.md), Linux-agenter kräver hello Microsoft Beroendeagent.</span><span class="sxs-lookup"><span data-stu-id="253ad-153">In addition toohello [OMS Agent](log-analytics-linux-agents.md), Linux agents require hello Microsoft Dependency Agent.</span></span> <span data-ttu-id="253ad-154">Se hello [operativsystem](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) för en fullständig lista över versioner av operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="253ad-154">See hello [supported operating systems](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) for a complete list of operating system versions.</span></span> |
| <span data-ttu-id="253ad-155">System Center Operations Manager-hanteringsgrupp</span><span class="sxs-lookup"><span data-stu-id="253ad-155">System Center Operations Manager management group</span></span> | <span data-ttu-id="253ad-156">Ja</span><span class="sxs-lookup"><span data-stu-id="253ad-156">Yes</span></span> | <span data-ttu-id="253ad-157">Wire-Data analyserar och samlar in data från Windows- och Linux-agenter i en ansluten [System Center Operations Manager-hanteringsgruppen](log-analytics-om-agents.md).</span><span class="sxs-lookup"><span data-stu-id="253ad-157">Wire Data analyzes and collects data from Windows and Linux agents in a connected [System Center Operations Manager management group](log-analytics-om-agents.md).</span></span> <br><br> <span data-ttu-id="253ad-158">En direkt anslutning från hello System Center Operations Manager-agenten datorn tooLog Analytics krävs.</span><span class="sxs-lookup"><span data-stu-id="253ad-158">A direct connection from hello System Center Operations Manager agent computer tooLog Analytics is required.</span></span> <span data-ttu-id="253ad-159">Data vidarebefordras från hello management group tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="253ad-159">Data is forwarded from hello management group tooLog Analytics.</span></span> |
| <span data-ttu-id="253ad-160">Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="253ad-160">Azure storage account</span></span> | <span data-ttu-id="253ad-161">Nej</span><span class="sxs-lookup"><span data-stu-id="253ad-161">No</span></span> | <span data-ttu-id="253ad-162">Wire-Data samlar in data från agentdatorer, så det finns inga data från den toocollect från Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="253ad-162">Wire Data collects data from agent computers, so there is no data from it toocollect from Azure Storage.</span></span> |

<span data-ttu-id="253ad-163">På Windows hello Microsoft Monitoring Agent (MMA) används av både System Center Operations Manager och logganalys toogather och skicka data.</span><span class="sxs-lookup"><span data-stu-id="253ad-163">On Windows, hello Microsoft Monitoring Agent (MMA) is used by both System Center Operations Manager and Log Analytics toogather and send data.</span></span> <span data-ttu-id="253ad-164">Beroende på hello kontexten kallas hello agent hello System Center Operations Manager-Agent, OMS-Agent, Log Analytics Agent, MMA eller direkt Agent.</span><span class="sxs-lookup"><span data-stu-id="253ad-164">Depending on hello context, hello agent is called hello System Center Operations Manager Agent, OMS Agent, Log Analytics Agent, MMA, or Direct Agent.</span></span> <span data-ttu-id="253ad-165">System Center Operations Manager och Log Analytics tillhandahåller något olika versioner av hello MMA.</span><span class="sxs-lookup"><span data-stu-id="253ad-165">System Center Operations Manager and Log Analytics provide slightly different versions of hello MMA.</span></span> <span data-ttu-id="253ad-166">Dessa versioner kan varje rapport tooSystem Center Operations Manager, tooLog Analytics eller tooboth.</span><span class="sxs-lookup"><span data-stu-id="253ad-166">These versions can each report tooSystem Center Operations Manager, tooLog Analytics, or tooboth.</span></span>

<span data-ttu-id="253ad-167">På Linux, hello OMS-Agent för Linux samlar in och skickar data tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="253ad-167">On Linux, hello OMS Agent for Linux gathers and sends data tooLog Analytics.</span></span> <span data-ttu-id="253ad-168">Du kan använda Wire-Data på servrar med direkta OMS-Agent eller på servrar som är anslutna tooLog Analytics via System Center Operations Manager-hanteringsgrupper.</span><span class="sxs-lookup"><span data-stu-id="253ad-168">You can use Wire Data on servers with OMS Direct Agents or on servers that are attached tooLog Analytics via System Center Operations Manager management groups.</span></span>

<span data-ttu-id="253ad-169">I den här artikeln refererar tooall agenter om Linux- eller Windows, om anslutna tooa System Center Operations Manager-hanteringsgruppen eller direkt tooLog Analytics kallas hello _OMS-agent_.</span><span class="sxs-lookup"><span data-stu-id="253ad-169">In this article, references tooall agents, whether Linux or Windows, whether connected tooa System Center Operations Manager management group or directly tooLog Analytics are termed hello _OMS agent_.</span></span> <span data-ttu-id="253ad-170">Vi använder hello distributionsnamnet hello Agent endast om det behövs för kontext.</span><span class="sxs-lookup"><span data-stu-id="253ad-170">We'll use hello specific deployment name of hello agent only if it's needed for context.</span></span>

<span data-ttu-id="253ad-171">Hej Beroendeagent överföra inte alla data och kräver inte några ändringar toofirewalls eller portar.</span><span class="sxs-lookup"><span data-stu-id="253ad-171">hello Dependency Agent does not transmit any data itself, and it does not require any changes toofirewalls or ports.</span></span> <span data-ttu-id="253ad-172">hello data Wire-Data skickas alltid av hello OMS-agenten tooLog Analytics, antingen direkt eller använder hello OMS-Gateway.</span><span class="sxs-lookup"><span data-stu-id="253ad-172">hello data in Wire Data is always transmitted by hello OMS agent tooLog Analytics, either directly or using hello OMS Gateway.</span></span>

![agenten diagram](./media/log-analytics-wire-data/agents.png)

<span data-ttu-id="253ad-174">Om du är en System Center Operations Manager-användare med en management group anslutna tooLog Analytics:</span><span class="sxs-lookup"><span data-stu-id="253ad-174">If you are a System Center Operations Manager user with a management group connected tooLog Analytics:</span></span>

- <span data-ttu-id="253ad-175">Ingen ytterligare konfiguration krävs när System Center Operations Manager-agenter kan komma åt hello Internet tooconnect tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="253ad-175">No additional configuration is required when your System Center Operations Manager agents can access hello Internet tooconnect tooLog Analytics.</span></span>
- <span data-ttu-id="253ad-176">Du behöver tooconfigure hello OMS Gateway toowork med System Center Operations Manager när System Center Operations Manager-agenter inte kan komma åt logganalys över hello Internet.</span><span class="sxs-lookup"><span data-stu-id="253ad-176">You need tooconfigure hello OMS Gateway toowork with System Center Operations Manager when your System Center Operations Manager agents cannot access Log Analytics over hello Internet.</span></span>

<span data-ttu-id="253ad-177">Om du använder hello direkt Agent, behöver du tooconfigure hello OMS-agent själva tooconnect tooLog Analytics eller tooyour OMS-Gateway.</span><span class="sxs-lookup"><span data-stu-id="253ad-177">If you are using hello Direct Agent, you need tooconfigure hello OMS agent itself tooconnect tooLog Analytics or tooyour OMS Gateway.</span></span> <span data-ttu-id="253ad-178">Du kan hämta hello OMS Gateway från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).</span><span class="sxs-lookup"><span data-stu-id="253ad-178">You can download hello OMS Gateway from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="253ad-179">Krav</span><span class="sxs-lookup"><span data-stu-id="253ad-179">Prerequisites</span></span>

- <span data-ttu-id="253ad-180">Kräver hello [insikt och Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) lösningen-erbjudandet.</span><span class="sxs-lookup"><span data-stu-id="253ad-180">Requires hello [Insight and Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) solution offer.</span></span>
- <span data-ttu-id="253ad-181">Om du använder hello tidigare version av hello Wire-Data lösningen måste du först bort den.</span><span class="sxs-lookup"><span data-stu-id="253ad-181">If you're using hello previous version of hello Wire Data solution, you must first remove it.</span></span> <span data-ttu-id="253ad-182">Alla data som hämtats via hello ursprungliga Wire-Data-lösning är dock fortfarande tillgängliga i överföring Data 2.0 och Sök i loggfilen.</span><span class="sxs-lookup"><span data-stu-id="253ad-182">However, all data captured through hello original Wire Data solution is still available in Wire Data 2.0 and log search.</span></span>
- <span data-ttu-id="253ad-183">Administratörsbehörighet är nödvändiga tooinstall eller avinstallera hello Beroendeagent.</span><span class="sxs-lookup"><span data-stu-id="253ad-183">Administrator privileges are required tooinstall or uninstall hello Dependency Agent.</span></span>
- <span data-ttu-id="253ad-184">hello beroende agenten måste installeras på en dator med ett 64-bitars operativsystem.</span><span class="sxs-lookup"><span data-stu-id="253ad-184">hello Dependency Agent must be installed on a computer with a 64-bit operating system.</span></span>

### <a name="operating-systems"></a><span data-ttu-id="253ad-185">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="253ad-185">Operating systems</span></span>

<span data-ttu-id="253ad-186">hello listar följande avsnitt hello stöds operativsystem för hello Beroendeagent.</span><span class="sxs-lookup"><span data-stu-id="253ad-186">hello following sections list hello supported operating systems for hello Dependency Agent.</span></span> <span data-ttu-id="253ad-187">Wire-Data stöder inte 32-bitars arkitekturer för alla operativsystem.</span><span class="sxs-lookup"><span data-stu-id="253ad-187">Wire Data doesn't support 32-bit architectures for any operating system.</span></span>

#### <a name="windows-server"></a><span data-ttu-id="253ad-188">Windows Server</span><span class="sxs-lookup"><span data-stu-id="253ad-188">Windows Server</span></span>

- <span data-ttu-id="253ad-189">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="253ad-189">Windows Server 2016</span></span>
- <span data-ttu-id="253ad-190">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="253ad-190">Windows Server 2012 R2</span></span>
- <span data-ttu-id="253ad-191">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="253ad-191">Windows Server 2012</span></span>
- <span data-ttu-id="253ad-192">Windows Server 2008 R2 SP1</span><span class="sxs-lookup"><span data-stu-id="253ad-192">Windows Server 2008 R2 SP1</span></span>

#### <a name="windows-desktop"></a><span data-ttu-id="253ad-193">Windows-skrivbordet</span><span class="sxs-lookup"><span data-stu-id="253ad-193">Windows desktop</span></span>

- <span data-ttu-id="253ad-194">Windows 10</span><span class="sxs-lookup"><span data-stu-id="253ad-194">Windows 10</span></span>
- <span data-ttu-id="253ad-195">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="253ad-195">Windows 8.1</span></span>
- <span data-ttu-id="253ad-196">Windows 8</span><span class="sxs-lookup"><span data-stu-id="253ad-196">Windows 8</span></span>
- <span data-ttu-id="253ad-197">Windows 7</span><span class="sxs-lookup"><span data-stu-id="253ad-197">Windows 7</span></span>

#### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a><span data-ttu-id="253ad-198">Red Hat Enterprise Linux, CentOS Linux och Oracle Linux (med RHEL Kernel)</span><span class="sxs-lookup"><span data-stu-id="253ad-198">Red Hat Enterprise Linux, CentOS Linux, and Oracle Linux (with RHEL Kernel)</span></span>

- <span data-ttu-id="253ad-199">Endast standard och SMP Linux kernel-versioner stöds.</span><span class="sxs-lookup"><span data-stu-id="253ad-199">Only default and SMP Linux kernel releases are supported.</span></span>
- <span data-ttu-id="253ad-200">Avvikande kernel Frigör som PAE och Xen, inte stöds för Linux-distribution.</span><span class="sxs-lookup"><span data-stu-id="253ad-200">Nonstandard kernel releases, such as PAE and Xen, are not supported for any Linux distribution.</span></span> <span data-ttu-id="253ad-201">Till exempel ett system med hello versionen sträng med _2.6.16.21-0.8-xen_ stöds inte.</span><span class="sxs-lookup"><span data-stu-id="253ad-201">For example, a system with hello release string of _2.6.16.21-0.8-xen_ is not supported.</span></span>
- <span data-ttu-id="253ad-202">Anpassade kärnor, inklusive omkompileringar standard kärnor stöds inte.</span><span class="sxs-lookup"><span data-stu-id="253ad-202">Custom kernels, including recompiles of standard kernels, are not supported.</span></span>
- <span data-ttu-id="253ad-203">CentOSPlus kernel stöds inte.</span><span class="sxs-lookup"><span data-stu-id="253ad-203">CentOSPlus kernel is not supported.</span></span>
- <span data-ttu-id="253ad-204">Oracle Unbreakable Enterprise Kernel (UEK) ingår i ett senare avsnitt i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="253ad-204">Oracle Unbreakable Enterprise Kernel (UEK) is covered in a later section of this article.</span></span>

#### <a name="red-hat-linux-7"></a><span data-ttu-id="253ad-205">Red Hat Linux 7</span><span class="sxs-lookup"><span data-stu-id="253ad-205">Red Hat Linux 7</span></span>

| <span data-ttu-id="253ad-206">**OS-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-206">**OS version**</span></span> | <span data-ttu-id="253ad-207">**Kernel-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-207">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="253ad-208">7.0</span><span class="sxs-lookup"><span data-stu-id="253ad-208">7.0</span></span> | <span data-ttu-id="253ad-209">3.10.0-123</span><span class="sxs-lookup"><span data-stu-id="253ad-209">3.10.0-123</span></span> |
| <span data-ttu-id="253ad-210">7.1</span><span class="sxs-lookup"><span data-stu-id="253ad-210">7.1</span></span> | <span data-ttu-id="253ad-211">3.10.0-229</span><span class="sxs-lookup"><span data-stu-id="253ad-211">3.10.0-229</span></span> |
| <span data-ttu-id="253ad-212">7.2</span><span class="sxs-lookup"><span data-stu-id="253ad-212">7.2</span></span> | <span data-ttu-id="253ad-213">3.10.0-327</span><span class="sxs-lookup"><span data-stu-id="253ad-213">3.10.0-327</span></span> |
| <span data-ttu-id="253ad-214">7.3</span><span class="sxs-lookup"><span data-stu-id="253ad-214">7.3</span></span> | <span data-ttu-id="253ad-215">3.10.0-514</span><span class="sxs-lookup"><span data-stu-id="253ad-215">3.10.0-514</span></span> |

#### <a name="red-hat-linux-6"></a><span data-ttu-id="253ad-216">Red Hat Linux 6</span><span class="sxs-lookup"><span data-stu-id="253ad-216">Red Hat Linux 6</span></span>

| <span data-ttu-id="253ad-217">**OS-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-217">**OS version**</span></span> | <span data-ttu-id="253ad-218">**Kernel-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-218">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="253ad-219">6.0</span><span class="sxs-lookup"><span data-stu-id="253ad-219">6.0</span></span> | <span data-ttu-id="253ad-220">2.6.32-71</span><span class="sxs-lookup"><span data-stu-id="253ad-220">2.6.32-71</span></span> |
| <span data-ttu-id="253ad-221">6.1</span><span class="sxs-lookup"><span data-stu-id="253ad-221">6.1</span></span> | <span data-ttu-id="253ad-222">2.6.32-131</span><span class="sxs-lookup"><span data-stu-id="253ad-222">2.6.32-131</span></span> |
| <span data-ttu-id="253ad-223">6.2</span><span class="sxs-lookup"><span data-stu-id="253ad-223">6.2</span></span> | <span data-ttu-id="253ad-224">2.6.32-220</span><span class="sxs-lookup"><span data-stu-id="253ad-224">2.6.32-220</span></span> |
| <span data-ttu-id="253ad-225">6.3</span><span class="sxs-lookup"><span data-stu-id="253ad-225">6.3</span></span> | <span data-ttu-id="253ad-226">2.6.32-279</span><span class="sxs-lookup"><span data-stu-id="253ad-226">2.6.32-279</span></span> |
| <span data-ttu-id="253ad-227">6.4</span><span class="sxs-lookup"><span data-stu-id="253ad-227">6.4</span></span> | <span data-ttu-id="253ad-228">2.6.32-358</span><span class="sxs-lookup"><span data-stu-id="253ad-228">2.6.32-358</span></span> |
| <span data-ttu-id="253ad-229">6.5</span><span class="sxs-lookup"><span data-stu-id="253ad-229">6.5</span></span> | <span data-ttu-id="253ad-230">2.6.32-431</span><span class="sxs-lookup"><span data-stu-id="253ad-230">2.6.32-431</span></span> |
| <span data-ttu-id="253ad-231">6.6</span><span class="sxs-lookup"><span data-stu-id="253ad-231">6.6</span></span> | <span data-ttu-id="253ad-232">2.6.32-504</span><span class="sxs-lookup"><span data-stu-id="253ad-232">2.6.32-504</span></span> |
| <span data-ttu-id="253ad-233">6.7</span><span class="sxs-lookup"><span data-stu-id="253ad-233">6.7</span></span> | <span data-ttu-id="253ad-234">2.6.32-573</span><span class="sxs-lookup"><span data-stu-id="253ad-234">2.6.32-573</span></span> |
| <span data-ttu-id="253ad-235">6.8</span><span class="sxs-lookup"><span data-stu-id="253ad-235">6.8</span></span> | <span data-ttu-id="253ad-236">2.6.32-642</span><span class="sxs-lookup"><span data-stu-id="253ad-236">2.6.32-642</span></span> |

#### <a name="red-hat-linux-5"></a><span data-ttu-id="253ad-237">Red Hat Linux 5</span><span class="sxs-lookup"><span data-stu-id="253ad-237">Red Hat Linux 5</span></span>

| <span data-ttu-id="253ad-238">**OS-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-238">**OS version**</span></span> | <span data-ttu-id="253ad-239">**Kernel-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-239">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="253ad-240">5.8</span><span class="sxs-lookup"><span data-stu-id="253ad-240">5.8</span></span> | <span data-ttu-id="253ad-241">2.6.18-308</span><span class="sxs-lookup"><span data-stu-id="253ad-241">2.6.18-308</span></span> |
| <span data-ttu-id="253ad-242">5.9</span><span class="sxs-lookup"><span data-stu-id="253ad-242">5.9</span></span> | <span data-ttu-id="253ad-243">2.6.18-348</span><span class="sxs-lookup"><span data-stu-id="253ad-243">2.6.18-348</span></span> |
| <span data-ttu-id="253ad-244">5.10</span><span class="sxs-lookup"><span data-stu-id="253ad-244">5.10</span></span> | <span data-ttu-id="253ad-245">2.6.18-371</span><span class="sxs-lookup"><span data-stu-id="253ad-245">2.6.18-371</span></span> |
| <span data-ttu-id="253ad-246">5.11</span><span class="sxs-lookup"><span data-stu-id="253ad-246">5.11</span></span> | <span data-ttu-id="253ad-247">2.6.18-398</span><span class="sxs-lookup"><span data-stu-id="253ad-247">2.6.18-398</span></span> <br> <span data-ttu-id="253ad-248">2.6.18-400</span><span class="sxs-lookup"><span data-stu-id="253ad-248">2.6.18-400</span></span> <br><span data-ttu-id="253ad-249">2.6.18-402</span><span class="sxs-lookup"><span data-stu-id="253ad-249">2.6.18-402</span></span> <br><span data-ttu-id="253ad-250">2.6.18-404</span><span class="sxs-lookup"><span data-stu-id="253ad-250">2.6.18-404</span></span> <br><span data-ttu-id="253ad-251">2.6.18-406</span><span class="sxs-lookup"><span data-stu-id="253ad-251">2.6.18-406</span></span> <br> <span data-ttu-id="253ad-252">2.6.18-407</span><span class="sxs-lookup"><span data-stu-id="253ad-252">2.6.18-407</span></span> <br> <span data-ttu-id="253ad-253">2.6.18-408</span><span class="sxs-lookup"><span data-stu-id="253ad-253">2.6.18-408</span></span> <br> <span data-ttu-id="253ad-254">2.6.18-409</span><span class="sxs-lookup"><span data-stu-id="253ad-254">2.6.18-409</span></span> <br> <span data-ttu-id="253ad-255">2.6.18-410</span><span class="sxs-lookup"><span data-stu-id="253ad-255">2.6.18-410</span></span> <br> <span data-ttu-id="253ad-256">2.6.18-411</span><span class="sxs-lookup"><span data-stu-id="253ad-256">2.6.18-411</span></span> <br> <span data-ttu-id="253ad-257">2.6.18-412</span><span class="sxs-lookup"><span data-stu-id="253ad-257">2.6.18-412</span></span> <br> <span data-ttu-id="253ad-258">2.6.18-416</span><span class="sxs-lookup"><span data-stu-id="253ad-258">2.6.18-416</span></span> <br> <span data-ttu-id="253ad-259">2.6.18-417</span><span class="sxs-lookup"><span data-stu-id="253ad-259">2.6.18-417</span></span> <br> <span data-ttu-id="253ad-260">2.6.18-419</span><span class="sxs-lookup"><span data-stu-id="253ad-260">2.6.18-419</span></span> |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a><span data-ttu-id="253ad-261">Oracle Enterprise Linux med Unbreakable Enterprise Kernel</span><span class="sxs-lookup"><span data-stu-id="253ad-261">Oracle Enterprise Linux with Unbreakable Enterprise Kernel</span></span>

#### <a name="oracle-linux-6"></a><span data-ttu-id="253ad-262">Oracle Linux 6</span><span class="sxs-lookup"><span data-stu-id="253ad-262">Oracle Linux 6</span></span>

| <span data-ttu-id="253ad-263">**OS-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-263">**OS version**</span></span> | <span data-ttu-id="253ad-264">**Kernel-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-264">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="253ad-265">6.2</span><span class="sxs-lookup"><span data-stu-id="253ad-265">6.2</span></span> | <span data-ttu-id="253ad-266">Oracle 2.6.32-300 (UEK R1)</span><span class="sxs-lookup"><span data-stu-id="253ad-266">Oracle 2.6.32-300 (UEK R1)</span></span> |
| <span data-ttu-id="253ad-267">6.3</span><span class="sxs-lookup"><span data-stu-id="253ad-267">6.3</span></span> | <span data-ttu-id="253ad-268">Oracle 2.6.39-200 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="253ad-268">Oracle 2.6.39-200 (UEK R2)</span></span> |
| <span data-ttu-id="253ad-269">6.4</span><span class="sxs-lookup"><span data-stu-id="253ad-269">6.4</span></span> | <span data-ttu-id="253ad-270">Oracle 2.6.39-400 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="253ad-270">Oracle 2.6.39-400 (UEK R2)</span></span> |
| <span data-ttu-id="253ad-271">6.5</span><span class="sxs-lookup"><span data-stu-id="253ad-271">6.5</span></span> | <span data-ttu-id="253ad-272">Oracle 2.6.39-400 (UEK R2 i386)</span><span class="sxs-lookup"><span data-stu-id="253ad-272">Oracle 2.6.39-400 (UEK R2 i386)</span></span> |
| <span data-ttu-id="253ad-273">6.6</span><span class="sxs-lookup"><span data-stu-id="253ad-273">6.6</span></span> | <span data-ttu-id="253ad-274">Oracle 2.6.39-400 (UEK R2 i386)</span><span class="sxs-lookup"><span data-stu-id="253ad-274">Oracle 2.6.39-400 (UEK R2 i386)</span></span> |

#### <a name="oracle-linux-5"></a><span data-ttu-id="253ad-275">Oracle Linux 5</span><span class="sxs-lookup"><span data-stu-id="253ad-275">Oracle Linux 5</span></span>

| <span data-ttu-id="253ad-276">**OS-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-276">**OS version**</span></span> | <span data-ttu-id="253ad-277">**Kernel-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-277">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="253ad-278">5.8</span><span class="sxs-lookup"><span data-stu-id="253ad-278">5.8</span></span> | <span data-ttu-id="253ad-279">Oracle 2.6.32-300 (UEK R1)</span><span class="sxs-lookup"><span data-stu-id="253ad-279">Oracle 2.6.32-300 (UEK R1)</span></span> |
| <span data-ttu-id="253ad-280">5.9</span><span class="sxs-lookup"><span data-stu-id="253ad-280">5.9</span></span> | <span data-ttu-id="253ad-281">Oracle 2.6.39-300 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="253ad-281">Oracle 2.6.39-300 (UEK R2)</span></span> |
| <span data-ttu-id="253ad-282">5.10</span><span class="sxs-lookup"><span data-stu-id="253ad-282">5.10</span></span> | <span data-ttu-id="253ad-283">Oracle 2.6.39-400 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="253ad-283">Oracle 2.6.39-400 (UEK R2)</span></span> |
| <span data-ttu-id="253ad-284">5.11</span><span class="sxs-lookup"><span data-stu-id="253ad-284">5.11</span></span> | <span data-ttu-id="253ad-285">Oracle 2.6.39-400 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="253ad-285">Oracle 2.6.39-400 (UEK R2)</span></span> |

#### <a name="suse-linux-enterprise-server"></a><span data-ttu-id="253ad-286">SUSE Linux Enterprise Server</span><span class="sxs-lookup"><span data-stu-id="253ad-286">SUSE Linux Enterprise Server</span></span>

#### <a name="suse-linux-11"></a><span data-ttu-id="253ad-287">SUSE Linux 11</span><span class="sxs-lookup"><span data-stu-id="253ad-287">SUSE Linux 11</span></span>

| <span data-ttu-id="253ad-288">**OS-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-288">**OS version**</span></span> | <span data-ttu-id="253ad-289">**Kernel-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-289">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="253ad-290">11</span><span class="sxs-lookup"><span data-stu-id="253ad-290">11</span></span> | <span data-ttu-id="253ad-291">2.6.27</span><span class="sxs-lookup"><span data-stu-id="253ad-291">2.6.27</span></span> |
| <span data-ttu-id="253ad-292">11 SP1</span><span class="sxs-lookup"><span data-stu-id="253ad-292">11 SP1</span></span> | <span data-ttu-id="253ad-293">2.6.32</span><span class="sxs-lookup"><span data-stu-id="253ad-293">2.6.32</span></span> |
| <span data-ttu-id="253ad-294">11 SP2</span><span class="sxs-lookup"><span data-stu-id="253ad-294">11 SP2</span></span> | <span data-ttu-id="253ad-295">3.0.13</span><span class="sxs-lookup"><span data-stu-id="253ad-295">3.0.13</span></span> |
| <span data-ttu-id="253ad-296">11 SP3</span><span class="sxs-lookup"><span data-stu-id="253ad-296">11 SP3</span></span> | <span data-ttu-id="253ad-297">3.0.76</span><span class="sxs-lookup"><span data-stu-id="253ad-297">3.0.76</span></span> |
| <span data-ttu-id="253ad-298">11 SP4</span><span class="sxs-lookup"><span data-stu-id="253ad-298">11 SP4</span></span> | <span data-ttu-id="253ad-299">3.0.101</span><span class="sxs-lookup"><span data-stu-id="253ad-299">3.0.101</span></span> |

#### <a name="suse-linux-10"></a><span data-ttu-id="253ad-300">SUSE Linux 10</span><span class="sxs-lookup"><span data-stu-id="253ad-300">SUSE Linux 10</span></span>

| <span data-ttu-id="253ad-301">**OS-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-301">**OS version**</span></span> | <span data-ttu-id="253ad-302">**Kernel-version**</span><span class="sxs-lookup"><span data-stu-id="253ad-302">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="253ad-303">10 SP4</span><span class="sxs-lookup"><span data-stu-id="253ad-303">10 SP4</span></span> | <span data-ttu-id="253ad-304">2.6.16.60</span><span class="sxs-lookup"><span data-stu-id="253ad-304">2.6.16.60</span></span> |

#### <a name="dependency-agent-downloads"></a><span data-ttu-id="253ad-305">Hämtar Beroendeagent</span><span class="sxs-lookup"><span data-stu-id="253ad-305">Dependency Agent downloads</span></span>

| <span data-ttu-id="253ad-306">**Fil**</span><span class="sxs-lookup"><span data-stu-id="253ad-306">**File**</span></span> | <span data-ttu-id="253ad-307">**OS**</span><span class="sxs-lookup"><span data-stu-id="253ad-307">**OS**</span></span> | <span data-ttu-id="253ad-308">**Version**</span><span class="sxs-lookup"><span data-stu-id="253ad-308">**Version**</span></span> | <span data-ttu-id="253ad-309">**SHA-256**</span><span class="sxs-lookup"><span data-stu-id="253ad-309">**SHA-256**</span></span> |
| --- | --- | --- | --- |
| [<span data-ttu-id="253ad-310">InstallDependencyAgent Windows.exe</span><span class="sxs-lookup"><span data-stu-id="253ad-310">InstallDependencyAgent-Windows.exe</span></span>](https://aka.ms/dependencyagentwindows) | <span data-ttu-id="253ad-311">Windows</span><span class="sxs-lookup"><span data-stu-id="253ad-311">Windows</span></span> | <span data-ttu-id="253ad-312">9.0.5</span><span class="sxs-lookup"><span data-stu-id="253ad-312">9.0.5</span></span> | <span data-ttu-id="253ad-313">73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43</span><span class="sxs-lookup"><span data-stu-id="253ad-313">73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43</span></span> |
| [<span data-ttu-id="253ad-314">InstallDependencyAgent Linux64.bin</span><span class="sxs-lookup"><span data-stu-id="253ad-314">InstallDependencyAgent-Linux64.bin</span></span>](https://aka.ms/dependencyagentlinux) | <span data-ttu-id="253ad-315">Linux</span><span class="sxs-lookup"><span data-stu-id="253ad-315">Linux</span></span> | <span data-ttu-id="253ad-316">9.0.5</span><span class="sxs-lookup"><span data-stu-id="253ad-316">9.0.5</span></span> | <span data-ttu-id="253ad-317">A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371</span><span class="sxs-lookup"><span data-stu-id="253ad-317">A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371</span></span> |



## <a name="configuration"></a><span data-ttu-id="253ad-318">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="253ad-318">Configuration</span></span>

<span data-ttu-id="253ad-319">Utför följande steg tooconfigure hello Wire-Data lösningen för dina arbetsytor hello.</span><span class="sxs-lookup"><span data-stu-id="253ad-319">Perform hello following steps tooconfigure hello Wire Data solution for your workspaces.</span></span>

1. <span data-ttu-id="253ad-320">Aktivera hello aktivitet logganalys lösningar från hello [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="253ad-320">Enable hello Activity Log Analytics solution from hello [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="253ad-321">Installera hello beroende agenten på varje dator där du vill tooget data.</span><span class="sxs-lookup"><span data-stu-id="253ad-321">Install hello Dependency Agent on each computer where you want tooget data.</span></span> <span data-ttu-id="253ad-322">Hej Beroendeagent kan övervaka anslutningar tooimmediate grannar, så du inte kan ha en agent på varje dator.</span><span class="sxs-lookup"><span data-stu-id="253ad-322">hello Dependency Agent can monitor connections tooimmediate neighbors, so you might not need an agent on every computer.</span></span>

### <a name="install-hello-dependency-agent-on-windows"></a><span data-ttu-id="253ad-323">Installera hello Beroendeagent i Windows</span><span class="sxs-lookup"><span data-stu-id="253ad-323">Install hello Dependency Agent on Windows</span></span>

<span data-ttu-id="253ad-324">Administratörsbehörighet är nödvändiga tooinstall eller avinstallera hello-agenten.</span><span class="sxs-lookup"><span data-stu-id="253ad-324">Administrator privileges are required tooinstall or uninstall hello agent.</span></span>

<span data-ttu-id="253ad-325">Hej Beroendeagent är installerad på Windows-datorer via InstallDependencyAgent Windows.exe.</span><span class="sxs-lookup"><span data-stu-id="253ad-325">hello Dependency Agent is installed on computers running Windows through InstallDependencyAgent-Windows.exe.</span></span> <span data-ttu-id="253ad-326">Om du kör den här körbara filen utan alternativ, startas en guide som att du kan följa tooinstall interaktivt.</span><span class="sxs-lookup"><span data-stu-id="253ad-326">If you run this executable file without any options, it starts a wizard that you can follow tooinstall interactively.</span></span>

<span data-ttu-id="253ad-327">Använd följande steg tooinstall hello beroende agenten på varje dator som kör Windows hello:</span><span class="sxs-lookup"><span data-stu-id="253ad-327">Use hello following steps tooinstall hello Dependency Agent on each computer running Windows:</span></span>

1. <span data-ttu-id="253ad-328">Installera hello OMS-agenten med hjälp av hello instruktionerna på [toohello ansluta Windows-datorer i Azure Log Analytics-tjänsten](log-analytics-windows-agents.md).</span><span class="sxs-lookup"><span data-stu-id="253ad-328">Install hello OMS Agent by using hello instructions at [Connect Windows computers toohello Log Analytics service in Azure](log-analytics-windows-agents.md).</span></span>
2. <span data-ttu-id="253ad-329">Hämta hello Windows-agenten via hello länk i hello föregående avsnitt och kör sedan med hjälp av följande kommando hello: InstallDependencyAgent Windows.exe</span><span class="sxs-lookup"><span data-stu-id="253ad-329">Download hello Windows agent using hello link in hello previous section and then run it by using hello following command: InstallDependencyAgent-Windows.exe</span></span>
3. <span data-ttu-id="253ad-330">Följ hello guiden tooinstall hello agent.</span><span class="sxs-lookup"><span data-stu-id="253ad-330">Follow hello wizard tooinstall hello agent.</span></span>
4. <span data-ttu-id="253ad-331">Om hello Beroendeagent misslyckas toostart loggarna hello för Detaljerad felinformation.</span><span class="sxs-lookup"><span data-stu-id="253ad-331">If hello Dependency Agent fails toostart, check hello logs for detailed error information.</span></span> <span data-ttu-id="253ad-332">På Windows-agenter är hello loggkatalogen %Programfiles%\Microsoft beroende Agent\logs.</span><span class="sxs-lookup"><span data-stu-id="253ad-332">On Windows agents, hello log directory is %Programfiles%\Microsoft Dependency Agent\logs.</span></span>

#### <a name="windows-command-line"></a><span data-ttu-id="253ad-333">Windows-kommandoraden</span><span class="sxs-lookup"><span data-stu-id="253ad-333">Windows command line</span></span>

<span data-ttu-id="253ad-334">Använd alternativen från hello följande tabell tooinstall från en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="253ad-334">Use options from hello following table tooinstall from a command line.</span></span> <span data-ttu-id="253ad-335">toosee en lista över hello flaggor för installation, kör installationsprogrammet för hello med hjälp av hello /?</span><span class="sxs-lookup"><span data-stu-id="253ad-335">toosee a list of hello installation flags, run hello installer by using hello /?</span></span> <span data-ttu-id="253ad-336">Flagga på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="253ad-336">flag as follows.</span></span>

<span data-ttu-id="253ad-337">InstallDependencyAgent Windows.exe /?</span><span class="sxs-lookup"><span data-stu-id="253ad-337">InstallDependencyAgent-Windows.exe /?</span></span>

| <span data-ttu-id="253ad-338">**Flaggan**</span><span class="sxs-lookup"><span data-stu-id="253ad-338">**Flag**</span></span> | <span data-ttu-id="253ad-339">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="253ad-339">**Description**</span></span> |
| --- | --- |
| <code>/?</code> | <span data-ttu-id="253ad-340">Hämta en lista över hello kommandoradsalternativ.</span><span class="sxs-lookup"><span data-stu-id="253ad-340">Get a list of hello command-line options.</span></span> |
| <code>/S</code> | <span data-ttu-id="253ad-341">Utföra en tyst installation utan uppmaningar för användaren.</span><span class="sxs-lookup"><span data-stu-id="253ad-341">Perform a silent installation with no user prompts.</span></span> |

<span data-ttu-id="253ad-342">Filer för hello Windows Beroendeagent placeras i C:\Program Files\Microsoft beroende agenten som standard.</span><span class="sxs-lookup"><span data-stu-id="253ad-342">Files for hello Windows Dependency Agent are placed in C:\Program Files\Microsoft Dependency Agent by default.</span></span>

### <a name="install-hello-dependency-agent-on-linux"></a><span data-ttu-id="253ad-343">Installera hello Beroendeagent på Linux</span><span class="sxs-lookup"><span data-stu-id="253ad-343">Install hello Dependency Agent on Linux</span></span>

<span data-ttu-id="253ad-344">Rotåtkomst är nödvändiga tooinstall eller konfigurera hello agent.</span><span class="sxs-lookup"><span data-stu-id="253ad-344">Root access is required tooinstall or configure hello agent.</span></span>

<span data-ttu-id="253ad-345">Hej Beroendeagent är installerad på Linux-datorer via InstallDependencyAgent-Linux64.bin, ett kommandoskript med en självextraherande binär.</span><span class="sxs-lookup"><span data-stu-id="253ad-345">hello Dependency Agent is installed on Linux computers through InstallDependencyAgent-Linux64.bin, a shell script with a self-extracting binary.</span></span> <span data-ttu-id="253ad-346">Du kan köra hello-filen med hjälp av _del_ eller Lägg till kör behörigheter toohello själva filen.</span><span class="sxs-lookup"><span data-stu-id="253ad-346">You can run hello file by using _sh_ or add execute permissions toohello file itself.</span></span>

<span data-ttu-id="253ad-347">Använd hello följa steg tooinstall hello beroende agenten på varje Linux-dator:</span><span class="sxs-lookup"><span data-stu-id="253ad-347">Use hello following steps tooinstall hello Dependency Agent on each Linux computer:</span></span>

1. <span data-ttu-id="253ad-348">Installera hello OMS-agenten med hjälp av hello instruktionerna på [samla in och hantera data från Linux-datorer](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="253ad-348">Install hello OMS Agent by using hello instructions at [Collect and manage data from Linux computers](log-analytics-agent-linux.md).</span></span>
2. <span data-ttu-id="253ad-349">Hämta hello Linux beroendeagent via hello länk i hello föregående avsnitt och installera det som rot med hjälp av följande kommando hello: del InstallDependencyAgent Linux64.bin</span><span class="sxs-lookup"><span data-stu-id="253ad-349">Download hello Linux Dependency agent using hello link in hello previous section and then install it as root by using hello following command: sh InstallDependencyAgent-Linux64.bin</span></span>
3. <span data-ttu-id="253ad-350">Om hello Beroendeagent misslyckas toostart loggarna hello för Detaljerad felinformation.</span><span class="sxs-lookup"><span data-stu-id="253ad-350">If hello Dependency Agent fails toostart, check hello logs for detailed error information.</span></span> <span data-ttu-id="253ad-351">På Linux-agenter hello loggkatalogen är: /var/opt/microsoft/dependency-agent/log.</span><span class="sxs-lookup"><span data-stu-id="253ad-351">On Linux agents, hello log directory is: /var/opt/microsoft/dependency-agent/log.</span></span>

<span data-ttu-id="253ad-352">toosee en lista över hello flaggor för installation, kör installationsprogrammet för hello med hello `-help` flaggan på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="253ad-352">toosee a list of hello installation flags, run hello installation program with hello `-help` flag as follows.</span></span>

```
InstallDependencyAgent-Linux64.bin -help
```

| <span data-ttu-id="253ad-353">**Flaggan**</span><span class="sxs-lookup"><span data-stu-id="253ad-353">**Flag**</span></span> | <span data-ttu-id="253ad-354">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="253ad-354">**Description**</span></span> |
| --- | --- |
| <code>-help</code> | <span data-ttu-id="253ad-355">Hämta en lista över hello kommandoradsalternativ.</span><span class="sxs-lookup"><span data-stu-id="253ad-355">Get a list of hello command-line options.</span></span> |
| <code>-s</code> | <span data-ttu-id="253ad-356">Utföra en tyst installation utan uppmaningar för användaren.</span><span class="sxs-lookup"><span data-stu-id="253ad-356">Perform a silent installation with no user prompts.</span></span> |
| <code>--check</code> | <span data-ttu-id="253ad-357">Kontrollera behörigheter och hello operativsystemet men inte installera hello agent.</span><span class="sxs-lookup"><span data-stu-id="253ad-357">Check permissions and hello operating system but do not install hello agent.</span></span> |

<span data-ttu-id="253ad-358">Filer för hello Beroendeagent placeras i hello följande kataloger:</span><span class="sxs-lookup"><span data-stu-id="253ad-358">Files for hello Dependency Agent are placed in hello following directories:</span></span>

| <span data-ttu-id="253ad-359">**Filer**</span><span class="sxs-lookup"><span data-stu-id="253ad-359">**Files**</span></span> | <span data-ttu-id="253ad-360">**Plats**</span><span class="sxs-lookup"><span data-stu-id="253ad-360">**Location**</span></span> |
| --- | --- |
| <span data-ttu-id="253ad-361">-Filer</span><span class="sxs-lookup"><span data-stu-id="253ad-361">Core files</span></span> | <span data-ttu-id="253ad-362">/OPT/Microsoft/Dependency-Agent</span><span class="sxs-lookup"><span data-stu-id="253ad-362">/opt/microsoft/dependency-agent</span></span> |
| <span data-ttu-id="253ad-363">Loggfiler</span><span class="sxs-lookup"><span data-stu-id="253ad-363">Log files</span></span> | <span data-ttu-id="253ad-364">/var/OPT/Microsoft/Dependency-Agent/log</span><span class="sxs-lookup"><span data-stu-id="253ad-364">/var/opt/microsoft/dependency-agent/log</span></span> |
| <span data-ttu-id="253ad-365">Config-filer</span><span class="sxs-lookup"><span data-stu-id="253ad-365">Config files</span></span> | <span data-ttu-id="253ad-366">/etc/OPT/Microsoft/Dependency-Agent/config</span><span class="sxs-lookup"><span data-stu-id="253ad-366">/etc/opt/microsoft/dependency-agent/config</span></span> |
| <span data-ttu-id="253ad-367">Tjänsten körbara filer</span><span class="sxs-lookup"><span data-stu-id="253ad-367">Service executable files</span></span> | <span data-ttu-id="253ad-368">/OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent</span><span class="sxs-lookup"><span data-stu-id="253ad-368">/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent</span></span><br><br><span data-ttu-id="253ad-369">/OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent-Manager</span><span class="sxs-lookup"><span data-stu-id="253ad-369">/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager</span></span> |
| <span data-ttu-id="253ad-370">Binära filer</span><span class="sxs-lookup"><span data-stu-id="253ad-370">Binary storage files</span></span> | <span data-ttu-id="253ad-371">/var/OPT/Microsoft/Dependency-Agent/Storage</span><span class="sxs-lookup"><span data-stu-id="253ad-371">/var/opt/microsoft/dependency-agent/storage</span></span> |

### <a name="installation-script-examples"></a><span data-ttu-id="253ad-372">Exempel på skript för installation</span><span class="sxs-lookup"><span data-stu-id="253ad-372">Installation script examples</span></span>

<span data-ttu-id="253ad-373">tooeasily distribuera hello Beroendeagent på flera servrar samtidigt, hjälper det toouse ett skript.</span><span class="sxs-lookup"><span data-stu-id="253ad-373">tooeasily deploy hello Dependency Agent on many servers at once, it helps toouse a script.</span></span> <span data-ttu-id="253ad-374">Du kan använda följande skript exempel toodownload hello och installera hello Beroendeagent på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="253ad-374">You can use hello following script examples toodownload and install hello Dependency Agent on either Windows or Linux.</span></span>

#### <a name="powershell-script-for-windows"></a><span data-ttu-id="253ad-375">Windows PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="253ad-375">PowerShell script for Windows</span></span>

```PowerShell

Invoke-WebRequest &quot;https://aka.ms/dependencyagentwindows&quot; -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S

```

#### <a name="shell-script-for-linux"></a><span data-ttu-id="253ad-376">Shell-skript för Linux</span><span class="sxs-lookup"><span data-stu-id="253ad-376">Shell script for Linux</span></span>

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
```

```
sh InstallDependencyAgent-Linux64.bin -s
```

### <a name="desired-state-configuration"></a><span data-ttu-id="253ad-377">Önskad tillståndskonfiguration</span><span class="sxs-lookup"><span data-stu-id="253ad-377">Desired State Configuration</span></span>

<span data-ttu-id="253ad-378">toodeploy hello Beroendeagent via Desired State Configuration kan du använda hello xPSDesiredStateConfiguration modulen och lite kod som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="253ad-378">toodeploy hello Dependency Agent via Desired State Configuration, you can use hello xPSDesiredStateConfiguration module and a bit of code like hello following:</span></span>

```
Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = &quot;C:\InstallDependencyAgent-Windows.exe&quot;



Node $NodeName

{

    # Download and install hello Dependency Agent

    xRemoteFile DAPackage

    {

        Uri = &quot;https://aka.ms/dependencyagentwindows&quot;

        DestinationPath = $DAPackageLocalPath

        DependsOn = &quot;[Package]OI&quot;

    }

    xPackage DA

    {

        Ensure=&quot;Present&quot;

        Name = &quot;Dependency Agent&quot;

        Path = $DAPackageLocalPath

        Arguments = '/S'

        ProductId = &quot;&quot;

        InstalledCheckRegKey = &quot;HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent&quot;

        InstalledCheckRegValueName = &quot;DisplayName&quot;

        InstalledCheckRegValueData = &quot;Dependency Agent&quot;

    }

}

```
### <a name="uninstall-hello-dependency-agent"></a><span data-ttu-id="253ad-379">Avinstallera hello Beroendeagent</span><span class="sxs-lookup"><span data-stu-id="253ad-379">Uninstall hello Dependency Agent</span></span>

<span data-ttu-id="253ad-380">Använd följande avsnitt toohelp du ta bort hello Beroendeagent hello.</span><span class="sxs-lookup"><span data-stu-id="253ad-380">Use hello following sections toohelp you remove hello Dependency Agent.</span></span>

#### <a name="uninstall-hello-dependency-agent-on-windows"></a><span data-ttu-id="253ad-381">Avinstallera hello Beroendeagent på Windows</span><span class="sxs-lookup"><span data-stu-id="253ad-381">Uninstall hello Dependency Agent on Windows</span></span>

<span data-ttu-id="253ad-382">En administratör kan avinstallera hello beroende agenten för Windows via Kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="253ad-382">An administrator can uninstall hello Dependency Agent for Windows through Control Panel.</span></span>

<span data-ttu-id="253ad-383">En administratör kan också köra %Programfiles%\Microsoft beroende Agent\Uninstall.exe toouninstall hello Beroendeagent.</span><span class="sxs-lookup"><span data-stu-id="253ad-383">An administrator can also run %Programfiles%\Microsoft Dependency Agent\Uninstall.exe toouninstall hello Dependency Agent.</span></span>

#### <a name="uninstall-hello-dependency-agent-on-linux"></a><span data-ttu-id="253ad-384">Avinstallera hello Beroendeagent på Linux</span><span class="sxs-lookup"><span data-stu-id="253ad-384">Uninstall hello Dependency Agent on Linux</span></span>

<span data-ttu-id="253ad-385">toocompletely avinstallera Hej Beroendeagent från Linux, måste du ta bort själva hello-agenten och hello connector, som installeras automatiskt med hello agent.</span><span class="sxs-lookup"><span data-stu-id="253ad-385">toocompletely uninstall hello Dependency Agent from Linux, you must remove hello agent itself and hello connector, which is installed automatically with hello agent.</span></span> <span data-ttu-id="253ad-386">Du kan avinstallera både med hjälp av hello följande enskilt kommando:</span><span class="sxs-lookup"><span data-stu-id="253ad-386">You can uninstall both by using hello following single command:</span></span>

```
rpm -e dependency-agent dependency-agent-connector
```

## <a name="management-packs"></a><span data-ttu-id="253ad-387">Hanteringspaket</span><span class="sxs-lookup"><span data-stu-id="253ad-387">Management packs</span></span>

<span data-ttu-id="253ad-388">När Wire-Data är aktiverat i logganalys-arbetsytan, skickas ett 300 KB hanteringspaket tooall hello Windows-servrar på arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="253ad-388">When Wire Data is activated in a Log Analytics workspace, a 300-KB management pack is sent tooall hello Windows servers in that workspace.</span></span> <span data-ttu-id="253ad-389">Om du använder System Center Operations Manager-agenter i en [ansluten hanteringsgrupp](log-analytics-om-agents.md), hello Dependency Monitor-hanteringspaket distribueras från System Center Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="253ad-389">If you are using System Center Operations Manager agents in a [connected management group](log-analytics-om-agents.md), hello Dependency Monitor management pack is deployed from System Center Operations Manager.</span></span> <span data-ttu-id="253ad-390">Om hello agenter är anslutna direkt levererar logganalys hello management pack.</span><span class="sxs-lookup"><span data-stu-id="253ad-390">If hello agents are directly connected, Log Analytics delivers hello management pack.</span></span>

<span data-ttu-id="253ad-391">hello management pack heter Microsoft.IntelligencePacks.ApplicationDependencyMonitor.</span><span class="sxs-lookup"><span data-stu-id="253ad-391">hello management pack is named Microsoft.IntelligencePacks.ApplicationDependencyMonitor.</span></span> <span data-ttu-id="253ad-392">De skrivs till: %Programfiles%\Microsoft övervakning Agent\Agent\Health State\Management servicepack.</span><span class="sxs-lookup"><span data-stu-id="253ad-392">It's written to: %Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs.</span></span> <span data-ttu-id="253ad-393">hello-datakälla som använder hello hanteringspaket är: % Program files%\Microsoft övervakning Agent\Agent\Health Service State\Resources&lt;AutoGeneratedID&gt;\ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.</span><span class="sxs-lookup"><span data-stu-id="253ad-393">hello data source that hello management pack uses is: %Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources&lt;AutoGeneratedID&gt;\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.</span></span>

## <a name="using-hello-solution"></a><span data-ttu-id="253ad-394">Med hello-lösning</span><span class="sxs-lookup"><span data-stu-id="253ad-394">Using hello solution</span></span>

<span data-ttu-id="253ad-395">**Installera och konfigurera hello lösning**</span><span class="sxs-lookup"><span data-stu-id="253ad-395">**Installing and configuring hello solution**</span></span>

<span data-ttu-id="253ad-396">Använd följande information tooinstall hello och konfigurera hello lösning.</span><span class="sxs-lookup"><span data-stu-id="253ad-396">Use hello following information tooinstall and configure hello solution.</span></span>

- <span data-ttu-id="253ad-397">hello Wire-Data lösningen hämtar data från datorer som kör Windows Server 2012 R2, Windows 8.1 och senare operativsystem.</span><span class="sxs-lookup"><span data-stu-id="253ad-397">hello Wire Data solution acquires data from computers running Windows Server 2012 R2, Windows 8.1, and later operating systems.</span></span>
- <span data-ttu-id="253ad-398">Microsoft .NET Framework 4.0 eller senare krävs för datorer som du vill tooacquire wire-data från.</span><span class="sxs-lookup"><span data-stu-id="253ad-398">Microsoft .NET Framework 4.0 or later is required on computers where you want tooacquire wire data from.</span></span>
- <span data-ttu-id="253ad-399">Lägg till hello Wire-Data lösning tooyour logganalys-arbetsytan med hjälp av hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="253ad-399">Add hello Wire Data solution tooyour Log Analytics workspace using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="253ad-400">Det krävs ingen ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="253ad-400">There is no further configuration required.</span></span>
- <span data-ttu-id="253ad-401">Om du vill tooview wire-data för en viss lösning behöver du toohave hello lösning redan lagts till tooyour arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="253ad-401">If you want tooview wire data for a specific solution, you need toohave hello solution already added tooyour workspace.</span></span>

<span data-ttu-id="253ad-402">När du har agenter som installerats och du installerar hello lösning, visas hello överföring Data 2.0 panelen på arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="253ad-402">After you have agents installed and you install hello solution, hello Wire Data 2.0 tile appears in your workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="253ad-403">För närvarande måste du använda hello OMS-portalen tooview wire-data.</span><span class="sxs-lookup"><span data-stu-id="253ad-403">Currently, you must use hello OMS portal tooview wire data.</span></span> <span data-ttu-id="253ad-404">Du kan inte använda hello Azure portal tooview wire-data.</span><span class="sxs-lookup"><span data-stu-id="253ad-404">You cannot use hello Azure portal tooview wire data.</span></span>

![Panelen Wire-Data](./media/log-analytics-wire-data/wire-data-tile.png)

## <a name="using-hello-wire-data-20-solution"></a><span data-ttu-id="253ad-406">Med hello överföring Data 2.0-lösning</span><span class="sxs-lookup"><span data-stu-id="253ad-406">Using hello Wire Data 2.0 solution</span></span>

<span data-ttu-id="253ad-407">I hello OMS-portalen klickar du på hello **överföring Data 2.0** panelen tooopen hello Wire-Data instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="253ad-407">In hello OMS portal, click hello **Wire Data 2.0** tile tooopen hello Wire Data dashboard.</span></span> <span data-ttu-id="253ad-408">hello instrumentpanelen innehåller hello blad i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="253ad-408">hello dashboard includes hello blades in hello following table.</span></span> <span data-ttu-id="253ad-409">Varje bladet visar upp too10 objekt som matchar att bladets kriterier för hello angetts scope och ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="253ad-409">Each blade lists up too10 items matching that blade's criteria for hello specified scope and time range.</span></span> <span data-ttu-id="253ad-410">Du kan köra en sökning i loggen som returnerar alla poster genom att klicka på **se alla** längst ned hello hello bladet eller genom att klicka på hello bladet sidhuvud.</span><span class="sxs-lookup"><span data-stu-id="253ad-410">You can run a log search that returns all records by clicking **See all** at hello bottom of hello blade or by clicking hello blade header.</span></span>

| <span data-ttu-id="253ad-411">**Bladet**</span><span class="sxs-lookup"><span data-stu-id="253ad-411">**Blade**</span></span> | <span data-ttu-id="253ad-412">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="253ad-412">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="253ad-413">Agenter som samlar in nätverkstrafik</span><span class="sxs-lookup"><span data-stu-id="253ad-413">Agents capturing network traffic</span></span> | <span data-ttu-id="253ad-414">Visar hello antalet agenter som hämtar nätverkstrafik och visar hello översta 10 datorer som hämtar trafik.</span><span class="sxs-lookup"><span data-stu-id="253ad-414">Shows hello number of agents that are capturing network traffic and lists hello top 10 computers that are capturing traffic.</span></span> <span data-ttu-id="253ad-415">Klicka på hello nummer toorun en logg sökning efter <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code>.</span><span class="sxs-lookup"><span data-stu-id="253ad-415">Click hello number toorun a log search for <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code>.</span></span> <span data-ttu-id="253ad-416">Klicka på en dator i hello listan toorun en logg sökning returnerar hello sammanlagt antal byte som har hämtats.</span><span class="sxs-lookup"><span data-stu-id="253ad-416">Click a computer in hello list toorun a log search returning hello total number of bytes captured.</span></span> |
| <span data-ttu-id="253ad-417">Lokala undernät</span><span class="sxs-lookup"><span data-stu-id="253ad-417">Local Subnets</span></span> | <span data-ttu-id="253ad-418">Visar hello antalet lokala undernät som agenter har identifierats.</span><span class="sxs-lookup"><span data-stu-id="253ad-418">Shows hello number of local subnets that agents have discovered.</span></span>  <span data-ttu-id="253ad-419">Klicka på hello nummer toorun en logg sökning efter <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> som visar en lista över alla undernät med hello antal byte som skickats över var och en.</span><span class="sxs-lookup"><span data-stu-id="253ad-419">Click hello number toorun a log search for <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> that lists all subnets with hello number of bytes sent over each one.</span></span> <span data-ttu-id="253ad-420">Klicka på ett undernät i hello listan toorun en logg sökning returnerar hello Totalt antal byte som skickats över hello undernät.</span><span class="sxs-lookup"><span data-stu-id="253ad-420">Click a subnet in hello list toorun a log search returning hello total number of bytes sent over hello subnet.</span></span> |
| <span data-ttu-id="253ad-421">Protokoll på programnivå</span><span class="sxs-lookup"><span data-stu-id="253ad-421">Application-level Protocols</span></span> | <span data-ttu-id="253ad-422">Visar hello antalet protokoll på programnivå används, som identifierats av agenter.</span><span class="sxs-lookup"><span data-stu-id="253ad-422">Shows hello number of application-level protocols in use, as discovered by agents.</span></span> <span data-ttu-id="253ad-423">Klicka på hello nummer toorun en logg sökning efter <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code>.</span><span class="sxs-lookup"><span data-stu-id="253ad-423">Click hello number toorun a log search for <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code>.</span></span> <span data-ttu-id="253ad-424">Klicka på ett protokoll toorun en logg sökning returnerar hello sammanlagt antal byte som skickats med hello-protokollet.</span><span class="sxs-lookup"><span data-stu-id="253ad-424">Click a protocol toorun a log search returning hello total number of bytes sent using hello protocol.</span></span> |

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Överföring Data instrumentpanelen](./media/log-analytics-wire-data/wire-data-dash.png)

<span data-ttu-id="253ad-426">Du kan använda hello **agenter som samlar in nätverkstrafik** bladet toodetermine hur mycket nätverksbandbredd som används av datorer.</span><span class="sxs-lookup"><span data-stu-id="253ad-426">You can use hello **Agents capturing network traffic** blade toodetermine how much network bandwidth is being consumed by computers.</span></span> <span data-ttu-id="253ad-427">Det här bladet kan hjälpa dig att enkelt hitta hello _chattiest_ dator i din miljö.</span><span class="sxs-lookup"><span data-stu-id="253ad-427">This blade can help you easily find hello _chattiest_ computer in your environment.</span></span> <span data-ttu-id="253ad-428">Sådana datorer kan vara överbelastad fungerar onormalt eller använder mer nätverksresurser än normalt.</span><span class="sxs-lookup"><span data-stu-id="253ad-428">Such computers could be overloaded, acting abnormally, or using more network resources than normal.</span></span>

![loggen Sök-exempel](./media/log-analytics-wire-data/log-search-example01.png)

<span data-ttu-id="253ad-430">På liknande sätt kan du använda hello **lokala undernät** bladet toodetermine hur mycket nätverkstrafik gå igenom dina undernät.</span><span class="sxs-lookup"><span data-stu-id="253ad-430">Similarly, you can use hello **Local Subnets** blade toodetermine how much network traffic is moving through your subnets.</span></span> <span data-ttu-id="253ad-431">Användarna definiera ofta undernät runt viktiga områden för sina program.</span><span class="sxs-lookup"><span data-stu-id="253ad-431">Users often define subnets around critical areas for their applications.</span></span> <span data-ttu-id="253ad-432">Det här bladet ger en överblick över dessa områden.</span><span class="sxs-lookup"><span data-stu-id="253ad-432">This blade offers a view into those areas.</span></span>

![loggen Sök-exempel](./media/log-analytics-wire-data/log-search-example02.png)

<span data-ttu-id="253ad-434">Hej **protokoll på programnivå** bladet är användbar eftersom den är användbara veta vilka protokoll som används.</span><span class="sxs-lookup"><span data-stu-id="253ad-434">hello **Application-level Protocols** blade is useful because it's helpful know what protocols are in use.</span></span> <span data-ttu-id="253ad-435">Du kan till exempel förvänta SSH toonot vara används i din nätverksmiljö.</span><span class="sxs-lookup"><span data-stu-id="253ad-435">For example, you might expect SSH toonot be in use in your network environment.</span></span> <span data-ttu-id="253ad-436">Visa information som är tillgängliga i hello bladet kan snabbt bekräfta eller disprove din förväntan.</span><span class="sxs-lookup"><span data-stu-id="253ad-436">Viewing information available in hello blade can quickly confirm or disprove your expectation.</span></span>

![loggen Sök-exempel](./media/log-analytics-wire-data/log-search-example03.png)

<span data-ttu-id="253ad-438">I det här exemplet kan gå till SSH information toosee vilka datorer som använder SSH och många andra kommunikationsdetaljer.</span><span class="sxs-lookup"><span data-stu-id="253ad-438">In this example, you could drill-into SSH details toosee which computers are using SSH and many other communication details.</span></span>

![del sökresultat](./media/log-analytics-wire-data/ssh-details.png)

<span data-ttu-id="253ad-440">Det är också användbart tooknow om protokolltrafik ökar eller minskar med tiden.</span><span class="sxs-lookup"><span data-stu-id="253ad-440">It's also useful tooknow if protocol traffic is increasing or decreasing over time.</span></span> <span data-ttu-id="253ad-441">Till exempel om ökar hello mängden data som skickas av ett program som kan vara något du bör vara medveten om, eller att du kan hitta anmärkningsvärda.</span><span class="sxs-lookup"><span data-stu-id="253ad-441">For example, if hello amount of data being transmitted by an application is increasing, that might be something you should be aware of, or that you might find noteworthy.</span></span>

## <a name="input-data"></a><span data-ttu-id="253ad-442">Indata</span><span class="sxs-lookup"><span data-stu-id="253ad-442">Input data</span></span>

<span data-ttu-id="253ad-443">Wire-data samlas in metadata om nätverkstrafik som använder hello agenter som du har aktiverat.</span><span class="sxs-lookup"><span data-stu-id="253ad-443">Wire data collects metadata about network traffic using hello agents that you have enabled.</span></span> <span data-ttu-id="253ad-444">Varje agent skickar data om var 15: e sekund.</span><span class="sxs-lookup"><span data-stu-id="253ad-444">Each agent sends data about every 15 seconds.</span></span>

## <a name="output-data"></a><span data-ttu-id="253ad-445">utdata</span><span class="sxs-lookup"><span data-stu-id="253ad-445">Output data</span></span>

<span data-ttu-id="253ad-446">En post med en typ av _WireData_ skapas för varje typ av indata.</span><span class="sxs-lookup"><span data-stu-id="253ad-446">A record with a type of _WireData_ is created for each type of input data.</span></span> <span data-ttu-id="253ad-447">WireData poster har egenskaper som visas i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="253ad-447">WireData records have properties shown in hello following table:</span></span>

| <span data-ttu-id="253ad-448">Egenskap</span><span class="sxs-lookup"><span data-stu-id="253ad-448">Property</span></span> | <span data-ttu-id="253ad-449">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="253ad-449">Description</span></span> |
|---|---|
| <span data-ttu-id="253ad-450">Dator</span><span class="sxs-lookup"><span data-stu-id="253ad-450">Computer</span></span> | <span data-ttu-id="253ad-451">Datornamnet där data har samlats in</span><span class="sxs-lookup"><span data-stu-id="253ad-451">Computer name where data was collected</span></span> |
| <span data-ttu-id="253ad-452">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="253ad-452">TimeGenerated</span></span> | <span data-ttu-id="253ad-453">Tiden hello posten</span><span class="sxs-lookup"><span data-stu-id="253ad-453">Time of hello record</span></span> |
| <span data-ttu-id="253ad-454">LocalIP</span><span class="sxs-lookup"><span data-stu-id="253ad-454">LocalIP</span></span> | <span data-ttu-id="253ad-455">Hello lokala datorns IP-adress</span><span class="sxs-lookup"><span data-stu-id="253ad-455">IP address of hello local computer</span></span> |
| <span data-ttu-id="253ad-456">SessionState</span><span class="sxs-lookup"><span data-stu-id="253ad-456">SessionState</span></span> | <span data-ttu-id="253ad-457">Ansluten eller frånkopplad</span><span class="sxs-lookup"><span data-stu-id="253ad-457">Connected or disconnected</span></span> |
| <span data-ttu-id="253ad-458">receivedBytes</span><span class="sxs-lookup"><span data-stu-id="253ad-458">ReceivedBytes</span></span> | <span data-ttu-id="253ad-459">Mängden byte som tagits emot</span><span class="sxs-lookup"><span data-stu-id="253ad-459">Amount of bytes received</span></span> |
| <span data-ttu-id="253ad-460">ProtocolName</span><span class="sxs-lookup"><span data-stu-id="253ad-460">ProtocolName</span></span> | <span data-ttu-id="253ad-461">Namnet på hello nätverksprotokoll som används</span><span class="sxs-lookup"><span data-stu-id="253ad-461">Name of hello network protocol used</span></span> |
| <span data-ttu-id="253ad-462">IPVersion</span><span class="sxs-lookup"><span data-stu-id="253ad-462">IPVersion</span></span> | <span data-ttu-id="253ad-463">IP-version</span><span class="sxs-lookup"><span data-stu-id="253ad-463">IP version</span></span> |
| <span data-ttu-id="253ad-464">Riktning</span><span class="sxs-lookup"><span data-stu-id="253ad-464">Direction</span></span> | <span data-ttu-id="253ad-465">Inkommande eller utgående</span><span class="sxs-lookup"><span data-stu-id="253ad-465">Inbound or outbound</span></span> |
| <span data-ttu-id="253ad-466">MaliciousIP</span><span class="sxs-lookup"><span data-stu-id="253ad-466">MaliciousIP</span></span> | <span data-ttu-id="253ad-467">IP-adressen för en känd skadlig källa</span><span class="sxs-lookup"><span data-stu-id="253ad-467">IP address of a known malicious source</span></span> |
| <span data-ttu-id="253ad-468">Allvarsgrad</span><span class="sxs-lookup"><span data-stu-id="253ad-468">Severity</span></span> | <span data-ttu-id="253ad-469">Allvarlighetsgrad för misstänkt skadlig programvara</span><span class="sxs-lookup"><span data-stu-id="253ad-469">Suspected malware severity</span></span> |
| <span data-ttu-id="253ad-470">RemoteIPCountry</span><span class="sxs-lookup"><span data-stu-id="253ad-470">RemoteIPCountry</span></span> | <span data-ttu-id="253ad-471">Land hello fjärranslutna IP-adress</span><span class="sxs-lookup"><span data-stu-id="253ad-471">Country of hello remote IP address</span></span> |
| <span data-ttu-id="253ad-472">ManagementGroupName</span><span class="sxs-lookup"><span data-stu-id="253ad-472">ManagementGroupName</span></span> | <span data-ttu-id="253ad-473">Namnet på hanteringsgruppen för hello Operations Manager</span><span class="sxs-lookup"><span data-stu-id="253ad-473">Name of hello Operations Manager management group</span></span> |
| <span data-ttu-id="253ad-474">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="253ad-474">SourceSystem</span></span> | <span data-ttu-id="253ad-475">Källan där data har samlats in</span><span class="sxs-lookup"><span data-stu-id="253ad-475">Source where data was collected</span></span> |
| <span data-ttu-id="253ad-476">SessionStartTime</span><span class="sxs-lookup"><span data-stu-id="253ad-476">SessionStartTime</span></span> | <span data-ttu-id="253ad-477">Starttid för session</span><span class="sxs-lookup"><span data-stu-id="253ad-477">Start time of session</span></span> |
| <span data-ttu-id="253ad-478">SessionEndTime</span><span class="sxs-lookup"><span data-stu-id="253ad-478">SessionEndTime</span></span> | <span data-ttu-id="253ad-479">Sluttid för session</span><span class="sxs-lookup"><span data-stu-id="253ad-479">End time of session</span></span> |
| <span data-ttu-id="253ad-480">LocalSubnet</span><span class="sxs-lookup"><span data-stu-id="253ad-480">LocalSubnet</span></span> | <span data-ttu-id="253ad-481">Undernät där data har samlats in</span><span class="sxs-lookup"><span data-stu-id="253ad-481">Subnet where data was collected</span></span> |
| <span data-ttu-id="253ad-482">LocalPortNumber</span><span class="sxs-lookup"><span data-stu-id="253ad-482">LocalPortNumber</span></span> | <span data-ttu-id="253ad-483">Lokala portnumret</span><span class="sxs-lookup"><span data-stu-id="253ad-483">Local port number</span></span> |
| <span data-ttu-id="253ad-484">RemoteIP</span><span class="sxs-lookup"><span data-stu-id="253ad-484">RemoteIP</span></span> | <span data-ttu-id="253ad-485">Fjärranslutna IP-adress som används av hello fjärrdatorn</span><span class="sxs-lookup"><span data-stu-id="253ad-485">Remote IP address used by hello remote computer</span></span> |
| <span data-ttu-id="253ad-486">RemotePortNumber</span><span class="sxs-lookup"><span data-stu-id="253ad-486">RemotePortNumber</span></span> | <span data-ttu-id="253ad-487">Portnummer för hello fjärranslutna IP-adress</span><span class="sxs-lookup"><span data-stu-id="253ad-487">Port number used by hello remote IP address</span></span> |
| <span data-ttu-id="253ad-488">Sessions-ID</span><span class="sxs-lookup"><span data-stu-id="253ad-488">SessionID</span></span> | <span data-ttu-id="253ad-489">Ett unikt värde som identifierar kommunikationssessionen mellan två IP-adresser</span><span class="sxs-lookup"><span data-stu-id="253ad-489">A unique value that identifies communication session between two IP addresses</span></span> |
| <span data-ttu-id="253ad-490">sentBytes</span><span class="sxs-lookup"><span data-stu-id="253ad-490">SentBytes</span></span> | <span data-ttu-id="253ad-491">Antal skickade byte</span><span class="sxs-lookup"><span data-stu-id="253ad-491">Number of bytes sent</span></span> |
| <span data-ttu-id="253ad-492">TotalBytes</span><span class="sxs-lookup"><span data-stu-id="253ad-492">TotalBytes</span></span> | <span data-ttu-id="253ad-493">Totalt antal byte som skickats under sessionen</span><span class="sxs-lookup"><span data-stu-id="253ad-493">Total number of bytes sent during session</span></span> |
| <span data-ttu-id="253ad-494">ApplicationProtocol</span><span class="sxs-lookup"><span data-stu-id="253ad-494">ApplicationProtocol</span></span> | <span data-ttu-id="253ad-495">Nätverksprotokoll som används</span><span class="sxs-lookup"><span data-stu-id="253ad-495">Type of network protocol used</span></span>   |
| <span data-ttu-id="253ad-496">Process-ID</span><span class="sxs-lookup"><span data-stu-id="253ad-496">ProcessID</span></span> | <span data-ttu-id="253ad-497">Windows process-ID</span><span class="sxs-lookup"><span data-stu-id="253ad-497">Windows process ID</span></span> |
| <span data-ttu-id="253ad-498">Processnamn</span><span class="sxs-lookup"><span data-stu-id="253ad-498">ProcessName</span></span> | <span data-ttu-id="253ad-499">Sökvägen och filnamnet för hello-process</span><span class="sxs-lookup"><span data-stu-id="253ad-499">Path and file name of hello process</span></span> |
| <span data-ttu-id="253ad-500">RemoteIPLongitude</span><span class="sxs-lookup"><span data-stu-id="253ad-500">RemoteIPLongitude</span></span> | <span data-ttu-id="253ad-501">IP-Longitudvärde</span><span class="sxs-lookup"><span data-stu-id="253ad-501">IP longitude value</span></span> |
| <span data-ttu-id="253ad-502">RemoteIPLatitude</span><span class="sxs-lookup"><span data-stu-id="253ad-502">RemoteIPLatitude</span></span> | <span data-ttu-id="253ad-503">IP-latitud värde</span><span class="sxs-lookup"><span data-stu-id="253ad-503">IP latitude value</span></span> |


## <a name="next-steps"></a><span data-ttu-id="253ad-504">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="253ad-504">Next steps</span></span>

- <span data-ttu-id="253ad-505">[Söka i loggar](log-analytics-log-searches.md) tooview detaljerad överföring sökposter.</span><span class="sxs-lookup"><span data-stu-id="253ad-505">[Search logs](log-analytics-log-searches.md) tooview detailed wire data search records.</span></span>
