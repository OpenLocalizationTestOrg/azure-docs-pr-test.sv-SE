---
redirect_url: /azure/log-analytics/log-analytics-agent-linux
redirect_document_id: True
ROBOTS: NOINDEX
ms.openlocfilehash: 8b526144cd565f6750368e12970f008e66cc2023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-linux-computers-toolog-analytics"></a><span data-ttu-id="74969-101">Ansluta din Linux-datorer tooLog Analytics</span><span class="sxs-lookup"><span data-stu-id="74969-101">Connect your Linux computers tooLog Analytics</span></span>
<span data-ttu-id="74969-102">Använda Log Analytics kan du samla in och fungerar på data som genereras från Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="74969-102">Using Log Analytics, you can collect and act on data generated from Linux computers.</span></span> <span data-ttu-id="74969-103">Om du lägger till data som samlas in från Linux tooOMS kan du toomanage Linux-system och lösningar för behållare som Docker, oavsett var dina datorer finns – var som helst.</span><span class="sxs-lookup"><span data-stu-id="74969-103">Adding data collected from Linux tooOMS allows you toomanage Linux systems and container solutions like Docker, regardless of where your computers are located — virtually anywhere.</span></span> <span data-ttu-id="74969-104">Datakällor kan finnas i ditt lokala datacenter som fysiska servrar, virtuella datorer i en molnbaserad tjänst som Amazon Web Services (AWS) eller Microsoft Azure eller även hello bärbar dator på ditt skrivbord.</span><span class="sxs-lookup"><span data-stu-id="74969-104">Data sources might reside in your on-premises datacenter as physical servers, virtual computers in a cloud-hosted service like Amazon Web Services (AWS) or Microsoft Azure, or even hello laptop on your desk.</span></span> <span data-ttu-id="74969-105">Dessutom OMS samlar även in data från Windows-datorer på samma sätt, så att den stöder en verkligen hybrid IT-miljö.</span><span class="sxs-lookup"><span data-stu-id="74969-105">In addition, OMS also collects data from Windows computers similarly, so it supports a truly hybrid IT environment.</span></span>

<span data-ttu-id="74969-106">Du kan visa och hantera data från alla dessa källor med logganalys i OMS med en enda hanteringsportal.</span><span class="sxs-lookup"><span data-stu-id="74969-106">You can view and manage data from all of those sources with Log Analytics in OMS with a single management portal.</span></span> <span data-ttu-id="74969-107">Detta minskar behovet av hello toomonitor med många olika system gör det enkelt tooconsume och du kan exportera alla data som du vill toowhatever analytics affärslösning eller system som du redan har.</span><span class="sxs-lookup"><span data-stu-id="74969-107">This reduces hello need toomonitor it using many different systems, makes it easy tooconsume, and you can export any data you like toowhatever business analytics solution or system that you already have.</span></span>

<span data-ttu-id="74969-108">Den här artikeln är en snabbstartsguiden som hjälper dig att samla in och hantera data för ditt Linux-datorer med hjälp av hello OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="74969-108">This article is a quick start guide that will help you collect and manage data for your Linux computers using hello OMS Agent for Linux.</span></span> <span data-ttu-id="74969-109">Ytterligare teknisk information, till exempel proxyserverkonfiguration, information om CollectD mått och anpassad JSON-datakällor hittar du informationen på [OMS-Agent för Linux-översikt](https://github.com/Microsoft/OMS-Agent-for-Linux) och [OMS-Agent för Linux hela dokumentationen](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) på GitHub.</span><span class="sxs-lookup"><span data-stu-id="74969-109">For more technical details such as proxy server configuration, information about CollectD metrics, and custom JSON data sources, you’ll find that information at [OMS Agent for Linux overview](https://github.com/Microsoft/OMS-Agent-for-Linux) and [OMS Agent for Linux full documentation](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) on GitHub.</span></span>

<span data-ttu-id="74969-110">För närvarande kan du samla in hello följande typer av data från Linux-datorer:</span><span class="sxs-lookup"><span data-stu-id="74969-110">Currently, you can collect hello following types of data from Linux computers:</span></span>

* <span data-ttu-id="74969-111">Prestandamått</span><span class="sxs-lookup"><span data-stu-id="74969-111">Performance metrics</span></span>
* <span data-ttu-id="74969-112">Syslog-händelser</span><span class="sxs-lookup"><span data-stu-id="74969-112">Syslog events</span></span>
* <span data-ttu-id="74969-113">Aviseringar från Nagios och Zabbix</span><span class="sxs-lookup"><span data-stu-id="74969-113">Alerts from Nagios and Zabbix</span></span>
* <span data-ttu-id="74969-114">Prestandamått för docker-behållare, inventering och loggar</span><span class="sxs-lookup"><span data-stu-id="74969-114">Docker container performance metrics, inventory and logs</span></span>

## <a name="supported-linux-versions"></a><span data-ttu-id="74969-115">Linux-versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="74969-115">Supported Linux versions</span></span>
<span data-ttu-id="74969-116">Både x86 och x64 versioner stöds officiellt på olika Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="74969-116">Both x86 and x64 versions are officially supported on a variety of Linux distributions.</span></span> <span data-ttu-id="74969-117">Hello OMS-Agent för Linux kan också köra på andra distributioner som inte finns.</span><span class="sxs-lookup"><span data-stu-id="74969-117">However, hello OMS Agent for Linux might also run on other distributions not listed.</span></span>

* <span data-ttu-id="74969-118">Amazon Linux 2012.09 via 2015.09</span><span class="sxs-lookup"><span data-stu-id="74969-118">Amazon Linux 2012.09 through 2015.09</span></span>
* <span data-ttu-id="74969-119">CentOS Linux 5, 6 och 7</span><span class="sxs-lookup"><span data-stu-id="74969-119">CentOS Linux 5, 6, and 7</span></span>
* <span data-ttu-id="74969-120">Oracle Linux 5, 6 och 7</span><span class="sxs-lookup"><span data-stu-id="74969-120">Oracle Linux 5, 6, and 7</span></span>
* <span data-ttu-id="74969-121">Red Hat Enterprise Linux Server 5, 6 och 7</span><span class="sxs-lookup"><span data-stu-id="74969-121">Red Hat Enterprise Linux Server 5, 6 and 7</span></span>
* <span data-ttu-id="74969-122">Debian GNU/Linux 6, 7 och 8</span><span class="sxs-lookup"><span data-stu-id="74969-122">Debian GNU/Linux 6, 7, and 8</span></span>
* <span data-ttu-id="74969-123">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10</span><span class="sxs-lookup"><span data-stu-id="74969-123">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10</span></span>
* <span data-ttu-id="74969-124">SUSE Linux Enterprise Server 11 och 12</span><span class="sxs-lookup"><span data-stu-id="74969-124">SUSE Linux Enterprise Server 11 and 12</span></span>

## <a name="oms-agent-for-linux"></a><span data-ttu-id="74969-125">OMS-Agent för Linux</span><span class="sxs-lookup"><span data-stu-id="74969-125">OMS Agent for Linux</span></span>
<span data-ttu-id="74969-126">hello Operations Management Suite-Agent för Linux består av flera paket.</span><span class="sxs-lookup"><span data-stu-id="74969-126">hello Operations Management Suite Agent for Linux comprises multiple packages.</span></span> <span data-ttu-id="74969-127">hello versionen filen innehåller hello följande paket, som körs hello shell-paket med `--extract`.</span><span class="sxs-lookup"><span data-stu-id="74969-127">hello release file contains hello following packages, available by running hello shell bundle with `--extract`.</span></span>

| <span data-ttu-id="74969-128">**Paketet**</span><span class="sxs-lookup"><span data-stu-id="74969-128">**Package**</span></span> | <span data-ttu-id="74969-129">**Version**</span><span class="sxs-lookup"><span data-stu-id="74969-129">**Version**</span></span> | <span data-ttu-id="74969-130">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="74969-130">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="74969-131">omsagent</span><span class="sxs-lookup"><span data-stu-id="74969-131">omsagent</span></span> |<span data-ttu-id="74969-132">1.1.0</span><span class="sxs-lookup"><span data-stu-id="74969-132">1.1.0</span></span> |<span data-ttu-id="74969-133">hello Operations Management Suite-Agent för Linux</span><span class="sxs-lookup"><span data-stu-id="74969-133">hello Operations Management Suite Agent for Linux</span></span> |
| <span data-ttu-id="74969-134">omsconfig</span><span class="sxs-lookup"><span data-stu-id="74969-134">omsconfig</span></span> |<span data-ttu-id="74969-135">1.1.1</span><span class="sxs-lookup"><span data-stu-id="74969-135">1.1.1</span></span> |<span data-ttu-id="74969-136">Konfiguration av agenten för hello OMS-Agent</span><span class="sxs-lookup"><span data-stu-id="74969-136">Configuration agent for hello OMS Agent</span></span> |
| <span data-ttu-id="74969-137">omi</span><span class="sxs-lookup"><span data-stu-id="74969-137">omi</span></span> |<span data-ttu-id="74969-138">1.0.8.3</span><span class="sxs-lookup"><span data-stu-id="74969-138">1.0.8.3</span></span> |<span data-ttu-id="74969-139">Open Management Infrastructure (OMI)--en förenklad CIM-servern</span><span class="sxs-lookup"><span data-stu-id="74969-139">Open Management Infrastructure (OMI) -- a lightweight CIM Server</span></span> |
| <span data-ttu-id="74969-140">scx</span><span class="sxs-lookup"><span data-stu-id="74969-140">scx</span></span> |<span data-ttu-id="74969-141">1.6.2</span><span class="sxs-lookup"><span data-stu-id="74969-141">1.6.2</span></span> |<span data-ttu-id="74969-142">OMI CIM-Providers för operativsystemet prestandamått</span><span class="sxs-lookup"><span data-stu-id="74969-142">OMI CIM Providers for operating system performance metrics</span></span> |
| <span data-ttu-id="74969-143">Apache cimprov</span><span class="sxs-lookup"><span data-stu-id="74969-143">apache-cimprov</span></span> |<span data-ttu-id="74969-144">1.0.0</span><span class="sxs-lookup"><span data-stu-id="74969-144">1.0.0</span></span> |<span data-ttu-id="74969-145">Apache HTTP-Server prestandaövervakning provider för OMI.</span><span class="sxs-lookup"><span data-stu-id="74969-145">Apache HTTP Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="74969-146">Endast installera om Apache HTTP-Server har upptäckts.</span><span class="sxs-lookup"><span data-stu-id="74969-146">Only installed if Apache HTTP Server is detected.</span></span> |
| <span data-ttu-id="74969-147">MySQL-cimprov</span><span class="sxs-lookup"><span data-stu-id="74969-147">mysql-cimprov</span></span> |<span data-ttu-id="74969-148">1.0.0</span><span class="sxs-lookup"><span data-stu-id="74969-148">1.0.0</span></span> |<span data-ttu-id="74969-149">MySQL-Server prestandaövervakning provider för OMI.</span><span class="sxs-lookup"><span data-stu-id="74969-149">MySQL Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="74969-150">Endast installera om MySQL/MariaDB server har upptäckts.</span><span class="sxs-lookup"><span data-stu-id="74969-150">Only installed if MySQL/MariaDB server is detected.</span></span> |
| <span data-ttu-id="74969-151">docker-cimprov</span><span class="sxs-lookup"><span data-stu-id="74969-151">docker-cimprov</span></span> |<span data-ttu-id="74969-152">0.1.0</span><span class="sxs-lookup"><span data-stu-id="74969-152">0.1.0</span></span> |<span data-ttu-id="74969-153">Docker-provider för OMI.</span><span class="sxs-lookup"><span data-stu-id="74969-153">Docker provider for OMI.</span></span> <span data-ttu-id="74969-154">Endast installera om Docker har upptäckts.</span><span class="sxs-lookup"><span data-stu-id="74969-154">Only installed if Docker is detected.</span></span> |

### <a name="additional-installation-artifacts"></a><span data-ttu-id="74969-155">Installation av ytterligare artefakter</span><span class="sxs-lookup"><span data-stu-id="74969-155">Additional installation artifacts</span></span>
<span data-ttu-id="74969-156">När du har installerat hello OMS-agent för Linux-paket tillämpas hello följande ytterligare systemomfattande konfigurationsändringar.</span><span class="sxs-lookup"><span data-stu-id="74969-156">After installing hello OMS agent for Linux packages, hello following additional system-wide configuration changes are applied.</span></span> <span data-ttu-id="74969-157">Dessa artefakter tas bort när hello omsagent paketet är avinstallerat.</span><span class="sxs-lookup"><span data-stu-id="74969-157">These artifacts are removed when hello omsagent package is uninstalled.</span></span>

* <span data-ttu-id="74969-158">En icke-privilegierade användare med namnet: `omsagent` skapas.</span><span class="sxs-lookup"><span data-stu-id="74969-158">A non-privileged user named: `omsagent` is created.</span></span> <span data-ttu-id="74969-159">Detta är hello konto hello omsagent daemon körs som</span><span class="sxs-lookup"><span data-stu-id="74969-159">This is hello account hello omsagent daemon runs as</span></span>
* <span data-ttu-id="74969-160">En sudoers ”innehåller” fil skapas på /etc/sudoers.d/omsagent detta ger rätt omsagent toorestart hello syslog och omsagent daemon.</span><span class="sxs-lookup"><span data-stu-id="74969-160">A sudoers “include” file is created at /etc/sudoers.d/omsagent This authorizes omsagent toorestart hello syslog and omsagent daemons.</span></span> <span data-ttu-id="74969-161">Om sudo ”” Includes inte stöds i hello installerade versionen av sudo, skrivs dessa poster för/etc/sudoers.</span><span class="sxs-lookup"><span data-stu-id="74969-161">If sudo “include” directives are not supported in hello installed version of sudo, these entries will be written too/etc/sudoers.</span></span>
* <span data-ttu-id="74969-162">hello syslog-konfigurationen är ändrade tooforward en delmängd av händelser toohello agent.</span><span class="sxs-lookup"><span data-stu-id="74969-162">hello syslog configuration is modified tooforward a subset of events toohello agent.</span></span> <span data-ttu-id="74969-163">Mer information finns i hello **konfigurera datainsamling** nedan</span><span class="sxs-lookup"><span data-stu-id="74969-163">For more information, see hello **Configuring Data Collection** section below</span></span>

### <a name="linux-data-collection-details"></a><span data-ttu-id="74969-164">Linux information för samlingen</span><span class="sxs-lookup"><span data-stu-id="74969-164">Linux data collection details</span></span>
<span data-ttu-id="74969-165">hello följande tabell visar metoder för insamling av data och annan information om hur data samlas in.</span><span class="sxs-lookup"><span data-stu-id="74969-165">hello following table shows data collection methods and other details about how data is collected.</span></span>

| <span data-ttu-id="74969-166">Källa</span><span class="sxs-lookup"><span data-stu-id="74969-166">source</span></span> | <span data-ttu-id="74969-167">Styr Agent</span><span class="sxs-lookup"><span data-stu-id="74969-167">Direct Agent</span></span> | <span data-ttu-id="74969-168">SCOM-agent</span><span class="sxs-lookup"><span data-stu-id="74969-168">SCOM agent</span></span> | <span data-ttu-id="74969-169">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="74969-169">Azure Storage</span></span> | <span data-ttu-id="74969-170">SCOM krävs?</span><span class="sxs-lookup"><span data-stu-id="74969-170">SCOM required?</span></span> | <span data-ttu-id="74969-171">SCOM-agent data som skickas via management-grupp</span><span class="sxs-lookup"><span data-stu-id="74969-171">SCOM agent data sent via management group</span></span> | <span data-ttu-id="74969-172">Frekvens för samlingen</span><span class="sxs-lookup"><span data-stu-id="74969-172">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="74969-173">Zabbix</span><span class="sxs-lookup"><span data-stu-id="74969-173">Zabbix</span></span> |![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="74969-179">1 minut</span><span class="sxs-lookup"><span data-stu-id="74969-179">1 minute</span></span> |
| <span data-ttu-id="74969-180">Nagios</span><span class="sxs-lookup"><span data-stu-id="74969-180">Nagios</span></span> |![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="74969-186">anländer</span><span class="sxs-lookup"><span data-stu-id="74969-186">on arrival</span></span> |
| <span data-ttu-id="74969-187">syslog</span><span class="sxs-lookup"><span data-stu-id="74969-187">syslog</span></span> |![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="74969-193">från Azure storage: 10 minuter. från agent: anländer</span><span class="sxs-lookup"><span data-stu-id="74969-193">from Azure storage: 10 minutes; from agent: on arrival</span></span> |
| <span data-ttu-id="74969-194">Linux-prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="74969-194">Linux performance counters</span></span> |![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="74969-200">Som planerat, minst 10 sekunder</span><span class="sxs-lookup"><span data-stu-id="74969-200">As scheduled, minimum of 10 seconds</span></span> |
| <span data-ttu-id="74969-201">Spåra ändringar</span><span class="sxs-lookup"><span data-stu-id="74969-201">change tracking</span></span> |![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="74969-207">varje timme</span><span class="sxs-lookup"><span data-stu-id="74969-207">hourly</span></span> |

### <a name="package-requirements"></a><span data-ttu-id="74969-208">Krav för paketet</span><span class="sxs-lookup"><span data-stu-id="74969-208">Package Requirements</span></span>
| <span data-ttu-id="74969-209">**Nödvändigt paket**</span><span class="sxs-lookup"><span data-stu-id="74969-209">**Required package**</span></span> | <span data-ttu-id="74969-210">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="74969-210">**Description**</span></span> | <span data-ttu-id="74969-211">**Lägsta version**</span><span class="sxs-lookup"><span data-stu-id="74969-211">**Minimum version**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="74969-212">Glibc</span><span class="sxs-lookup"><span data-stu-id="74969-212">Glibc</span></span> |<span data-ttu-id="74969-213">GNU C-bibliotek</span><span class="sxs-lookup"><span data-stu-id="74969-213">GNU C library</span></span> |<span data-ttu-id="74969-214">2.5-12</span><span class="sxs-lookup"><span data-stu-id="74969-214">2.5-12</span></span> |
| <span data-ttu-id="74969-215">Openssl</span><span class="sxs-lookup"><span data-stu-id="74969-215">Openssl</span></span> |<span data-ttu-id="74969-216">OpenSSL-bibliotek</span><span class="sxs-lookup"><span data-stu-id="74969-216">OpenSSL libraries</span></span> |<span data-ttu-id="74969-217">0.9.8e eller 1.0</span><span class="sxs-lookup"><span data-stu-id="74969-217">0.9.8e or 1.0</span></span> |
| <span data-ttu-id="74969-218">CURL</span><span class="sxs-lookup"><span data-stu-id="74969-218">Curl</span></span> |<span data-ttu-id="74969-219">cURL Webbklient</span><span class="sxs-lookup"><span data-stu-id="74969-219">cURL web client</span></span> |<span data-ttu-id="74969-220">7.15.5</span><span class="sxs-lookup"><span data-stu-id="74969-220">7.15.5</span></span> |
| <span data-ttu-id="74969-221">Python-ctypes</span><span class="sxs-lookup"><span data-stu-id="74969-221">Python-ctypes</span></span> |<span data-ttu-id="74969-222">Funktionsbibliotek</span><span class="sxs-lookup"><span data-stu-id="74969-222">function libraries</span></span> |<span data-ttu-id="74969-223">Saknas</span><span class="sxs-lookup"><span data-stu-id="74969-223">n/a</span></span> |
| <span data-ttu-id="74969-224">PAM</span><span class="sxs-lookup"><span data-stu-id="74969-224">PAM</span></span> |<span data-ttu-id="74969-225">Lösenordssynkronisering moduler</span><span class="sxs-lookup"><span data-stu-id="74969-225">Pluggable authentication Modules</span></span> |<span data-ttu-id="74969-226">Saknas</span><span class="sxs-lookup"><span data-stu-id="74969-226">n/a</span></span> |

> [!NOTE]
> <span data-ttu-id="74969-227">Rsyslog eller syslog-ng är obligatoriska toocollect syslog-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="74969-227">Either rsyslog or syslog-ng are required toocollect syslog messages.</span></span> <span data-ttu-id="74969-228">hello standard syslog-daemon på version 5 Red Hat Enterprise Linux, CentOS och Oracle Linux-version (sysklog) stöds inte för insamling av syslog-händelser.</span><span class="sxs-lookup"><span data-stu-id="74969-228">hello default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="74969-229">toocollect syslog-data från den här versionen av dessa distributioner hello rsyslog daemon ska vara installerat och konfigurerat tooreplace sysklog.</span><span class="sxs-lookup"><span data-stu-id="74969-229">toocollect syslog data from this version of these distributions, hello rsyslog daemon should be installed and configured tooreplace sysklog.</span></span>
>
>

## <a name="quick-install"></a><span data-ttu-id="74969-230">Snabb installation</span><span class="sxs-lookup"><span data-stu-id="74969-230">Quick install</span></span>
<span data-ttu-id="74969-231">Kör följande kommandon toodownload hello omsagent hello, verifiera hello kontrollsumma och sedan installera och publicera hello agent.</span><span class="sxs-lookup"><span data-stu-id="74969-231">Run hello following commands toodownload hello omsagent, validate hello checksum, then  install and onboard hello agent.</span></span> <span data-ttu-id="74969-232">Kommandon är för 64-bitarsversionen.</span><span class="sxs-lookup"><span data-stu-id="74969-232">Commands are for 64-bit.</span></span> <span data-ttu-id="74969-233">hello arbetsyte-ID och primärnyckel finns i hello OMS-portalen under **inställningar** på hello **anslutna källor** fliken.</span><span class="sxs-lookup"><span data-stu-id="74969-233">hello Workspace ID and Primary Key are found in hello OMS portal under **Settings** on hello **Connected Sources** tab.</span></span>

![information om arbetsytan](./media/log-analytics-linux-agents/oms-direct-agent-primary-key.png)

```
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR OMS WORKSPACE ID> -s <YOUR OMS WORKSPACE PRIMARY KEY>
```

<span data-ttu-id="74969-235">Det finns en mängd andra metoder tooinstall hello agent och uppgradera den.</span><span class="sxs-lookup"><span data-stu-id="74969-235">There are a variety of other methods tooinstall hello agent and upgrade it.</span></span> <span data-ttu-id="74969-236">Du kan läsa mer om dem i [steg tooinstall hello OMS-Agent för Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).</span><span class="sxs-lookup"><span data-stu-id="74969-236">You can read more about them at [Steps tooinstall hello OMS Agent for Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).</span></span>

<span data-ttu-id="74969-237">Du kan också visa hello [Azure videogenomgång](https://www.youtube.com/watch?v=mF1wtHPEzT0).</span><span class="sxs-lookup"><span data-stu-id="74969-237">You can also view hello [Azure video walkthrough](https://www.youtube.com/watch?v=mF1wtHPEzT0).</span></span>

## <a name="choose-your-linux-data-collection-method"></a><span data-ttu-id="74969-238">Välj din Linux metod för insamling av data</span><span class="sxs-lookup"><span data-stu-id="74969-238">Choose your Linux data collection method</span></span>
<span data-ttu-id="74969-239">Hur du väljer hello datatyper som du skulle t.ex toocollect beror på om du vill toouse hello OMS-portalen eller om du vill redigera olika konfigurationsfilerna direkt på Linux-klienter.</span><span class="sxs-lookup"><span data-stu-id="74969-239">How you choose hello data types you'd like toocollect depends on whether you want toouse hello OMS portal or if you want edit various configuration files directly on your Linux clients.</span></span> <span data-ttu-id="74969-240">Om du väljer toouse hello portal skickas hello configuration tooall av Linux-klienter automatiskt.</span><span class="sxs-lookup"><span data-stu-id="74969-240">If you choose toouse hello portal, hello configuration is sent tooall of your Linux clients automatically.</span></span> <span data-ttu-id="74969-241">Om du behöver olika konfigurationer för olika Linux-klienter kommer du behöver tooedit klientfiler individuellt – eller Använd ett alternativ som PowerShell DSC, Chef eller Puppet.</span><span class="sxs-lookup"><span data-stu-id="74969-241">If you need different configurations for different Linux clients, you will need tooedit client files individually – or use an alternative like PowerShell DSC, Chef, or Puppet.</span></span>

<span data-ttu-id="74969-242">Du kan ange hello syslog-händelser och prestandaräknare som du vill toocollect använder konfigurationsfiler på hello Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="74969-242">You can specify hello syslog events and performance counters that you want toocollect using configuration files on hello Linux computers.</span></span> <span data-ttu-id="74969-243">*Om du väljer tooconfigure datainsamling genom att redigera konfigurationsfilerna för agenten, bör du inaktivera hello centraliserad konfiguration.*</span><span class="sxs-lookup"><span data-stu-id="74969-243">*If you chose tooconfigure data collection by editing agent configuration files, you should disable hello centralized configuration.*</span></span>  <span data-ttu-id="74969-244">Instruktioner finns under tooconfigure datainsamling i hello agent configuration-filer samt toodisable centrala konfiguration för alla OMS-agenter för Linux eller enskilda datorer.</span><span class="sxs-lookup"><span data-stu-id="74969-244">Instructions are provided below tooconfigure data collection in hello agent's configuration files as well as toodisable central configuration for all OMS Agents for Linux, or individual computers.</span></span>

### <a name="disable-oms-management-for-an-individual-linux-computer"></a><span data-ttu-id="74969-245">Inaktivera OMS-hantering för en enskild Linux-dator</span><span class="sxs-lookup"><span data-stu-id="74969-245">Disable OMS management for an individual Linux computer</span></span>
<span data-ttu-id="74969-246">Centraliserad datainsamling för konfigurationsdata är inaktiverad för en enskild Linux-dator med hello OMS_MetaConfigHelper.py skript.</span><span class="sxs-lookup"><span data-stu-id="74969-246">Centralized data collection for configuration data is disabled for an individual Linux computer with hello OMS_MetaConfigHelper.py script.</span></span> <span data-ttu-id="74969-247">Detta kan vara användbart om en delmängd av datorerna ska ha en särskild konfiguration.</span><span class="sxs-lookup"><span data-stu-id="74969-247">This can be useful if a subset of computers should have a specialized configuration.</span></span>

<span data-ttu-id="74969-248">toodisable centraliserad konfiguration:</span><span class="sxs-lookup"><span data-stu-id="74969-248">toodisable centralized configuration:</span></span>

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```

<span data-ttu-id="74969-249">Aktivera toore centraliserad konfiguration:</span><span class="sxs-lookup"><span data-stu-id="74969-249">toore-enable centralized configuration:</span></span>

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py –enable
```

## <a name="linux-performance-counters"></a><span data-ttu-id="74969-250">Linux-prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="74969-250">Linux performance counters</span></span>
<span data-ttu-id="74969-251">Linux-prestandaräknare är liknande tooWindows prestandaräknare – både fungerar på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="74969-251">Linux performance counters are similar tooWindows performance counters—both operate similarly.</span></span> <span data-ttu-id="74969-252">Du kan använda följande procedurer tooadd hello och konfigurera dem.</span><span class="sxs-lookup"><span data-stu-id="74969-252">You can use hello following procedures tooadd and configure them.</span></span> <span data-ttu-id="74969-253">När de läggs tooOMS samlas data in för dem var 30: e sekund.</span><span class="sxs-lookup"><span data-stu-id="74969-253">After they are added tooOMS, data is collected for them every 30 seconds.</span></span>

### <a name="tooadd-a-linux-performance-counter-in-oms"></a><span data-ttu-id="74969-254">tooadd en Linux-prestandaräknare i OMS</span><span class="sxs-lookup"><span data-stu-id="74969-254">tooadd a Linux performance counter in OMS</span></span>
1. <span data-ttu-id="74969-255">tooconfigure OMS-agenter för Linux med hello OMS-portalen, du kan lägga till Linux prestandaräknare på hello inställningar klickar du på **Data**.</span><span class="sxs-lookup"><span data-stu-id="74969-255">tooconfigure OMS Agents for Linux using hello OMS portal, you can add Linux performance counters on hello Settings page, click **Data**.</span></span>  
2. <span data-ttu-id="74969-256">På hello **inställningar** sidan **Data** , klickar du på **Linux prestandaräknare** och sedan väljer eller typen hello hello räknare som du vill tooadd.</span><span class="sxs-lookup"><span data-stu-id="74969-256">On hello **Settings** page under **Data** , click **Linux performance counters** and then select or type hello name of hello counter you want tooadd.</span></span>  
    <span data-ttu-id="74969-257">![data](./media/log-analytics-linux-agents/oms-settings-data01.png)</span><span class="sxs-lookup"><span data-stu-id="74969-257">![data](./media/log-analytics-linux-agents/oms-settings-data01.png)</span></span>
3. <span data-ttu-id="74969-258">Om du inte vet hello fullständigt namn på räknaren av hello, du kan börja skriva ett partiellt namn och en lista över tillgängliga räknare visas.</span><span class="sxs-lookup"><span data-stu-id="74969-258">If you don't know hello full name of hello counter, you can start typing a partial name and a list of available counters will appear.</span></span> <span data-ttu-id="74969-259">När du har hittat hello räknare du vill tooadd på hello namn i hello listan och klicka sedan på hello plus ikonen tooadd hello räknaren.</span><span class="sxs-lookup"><span data-stu-id="74969-259">When you find hello counter you want tooadd, click hello name in hello list and then click hello plus icon tooadd hello counter.</span></span>
4. <span data-ttu-id="74969-260">När du har lagt till hello räknaren visas den i hello listan med räknare med ett färgade fält.</span><span class="sxs-lookup"><span data-stu-id="74969-260">After you add hello counter, it appears in hello list of counters highlighted with a colored bar.</span></span>
5. <span data-ttu-id="74969-261">Som standard hello **tillämpa konfigurationen toomy datorerna nedan** alternativet är markerat.</span><span class="sxs-lookup"><span data-stu-id="74969-261">By default, hello **Apply below configuration toomy machines** option is selected.</span></span> <span data-ttu-id="74969-262">Om du vill skicka konfigurationsdata toodisable avmarkerar du hello val.</span><span class="sxs-lookup"><span data-stu-id="74969-262">If you want toodisable sending configuration data, clear hello selection.</span></span>
6. <span data-ttu-id="74969-263">När du är klar ändra prestandaräknare på hello hello sidans nederkant klickar du på **spara** toofinalize ändringarna.</span><span class="sxs-lookup"><span data-stu-id="74969-263">When you are done modifying performance counters, at hello bottom of hello page click **Save** toofinalize your changes.</span></span> <span data-ttu-id="74969-264">hello konfigurationsändringar som du har gjort skickas sedan tooall hello OMS-Agent för Linux som har registrerats med OMS, vanligtvis inom 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="74969-264">hello configuration changes that you've made are then sent tooall hello OMS Agents for Linux that are registered with OMS, typically within 5 minutes.</span></span>

### <a name="configure-linux-performance-counters-in-oms"></a><span data-ttu-id="74969-265">Konfigurera Linux prestandaräknare i OMS</span><span class="sxs-lookup"><span data-stu-id="74969-265">Configure Linux performance counters in OMS</span></span>
<span data-ttu-id="74969-266">Du kan välja en specifik instans för varje prestandaräknare för Windows-prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="74969-266">For Windows performance counters, you can choose a specific instance for each performance counter.</span></span> <span data-ttu-id="74969-267">För Linux-prestandaräknare gäller dock instansen av en räknare som du väljer tooall underordnade räknare för hello överordnade räknaren.</span><span class="sxs-lookup"><span data-stu-id="74969-267">However, for Linux performance counters, whatever instance of a counter that you choose applies tooall child counters of hello parent counter.</span></span> <span data-ttu-id="74969-268">hello visar följande tabell hello vanliga instanser tillgängliga tooboth Linux och Windows prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="74969-268">hello following table shows hello common instances available tooboth Linux and Windows performance counters.</span></span>

| <span data-ttu-id="74969-269">**Instansnamn**</span><span class="sxs-lookup"><span data-stu-id="74969-269">**Instance name**</span></span> | <span data-ttu-id="74969-270">**Betydelse**</span><span class="sxs-lookup"><span data-stu-id="74969-270">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="74969-271">\_Totalt</span><span class="sxs-lookup"><span data-stu-id="74969-271">\_Total</span></span> |<span data-ttu-id="74969-272">Summan av alla hello-instanser</span><span class="sxs-lookup"><span data-stu-id="74969-272">Total of all hello instances</span></span> |
| \* |<span data-ttu-id="74969-273">Alla instanser</span><span class="sxs-lookup"><span data-stu-id="74969-273">All instances</span></span> |
| <span data-ttu-id="74969-274">(/ &#124; / var)</span><span class="sxs-lookup"><span data-stu-id="74969-274">(/&#124;/var)</span></span> |<span data-ttu-id="74969-275">Matchar instanser med namnet: / eller /var</span><span class="sxs-lookup"><span data-stu-id="74969-275">Matches instances named: / or /var</span></span> |

<span data-ttu-id="74969-276">På liknande sätt gäller hello intervall som du väljer för en överordnad räknare tooall dess underordnade räknare.</span><span class="sxs-lookup"><span data-stu-id="74969-276">Similarly, hello sample interval that you choose for a parent counter applies tooall its child counters.</span></span> <span data-ttu-id="74969-277">Med andra ord är alla hello underordnade räknaren samplingsintervall och instanser som knutna tillsammans.</span><span class="sxs-lookup"><span data-stu-id="74969-277">In other words, all hello child counter sample intervals and instances are tied together.</span></span>

### <a name="add-and-configure-performance-metrics-with-linux"></a><span data-ttu-id="74969-278">Lägga till och konfigurera prestandamått med Linux</span><span class="sxs-lookup"><span data-stu-id="74969-278">Add and configure performance metrics with Linux</span></span>
<span data-ttu-id="74969-279">Prestanda mått toocollect styrs av hello konfiguration i/etc/opt/microsoft/omsagent/&lt;arbetsyte-id&gt;/conf/omsagent.conf.</span><span class="sxs-lookup"><span data-stu-id="74969-279">Performance metrics toocollect are controlled by hello configuration in /etc/opt/microsoft/omsagent/&lt;workspace id&gt;/conf/omsagent.conf.</span></span> <span data-ttu-id="74969-280">Se [tillgängliga prestandamått](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) för tillgängliga klasser och mått för hello OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="74969-280">See [Available performance metrics](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) for available classes and metrics for hello OMS Agent for Linux.</span></span>

<span data-ttu-id="74969-281">Varje objekt eller kategori av prestanda mått toocollect ska vara definierat i hello konfigurationsfilen som en enda `<source>` element.</span><span class="sxs-lookup"><span data-stu-id="74969-281">Each object, or category, of performance metrics toocollect should be defined in hello configuration file as a single `<source>` element.</span></span> <span data-ttu-id="74969-282">hello syntax följer hello mönster nedan.</span><span class="sxs-lookup"><span data-stu-id="74969-282">hello syntax follows hello pattern below.</span></span>

```
<source>
  type oms_omi  
  object_name "Processor"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

<span data-ttu-id="74969-283">hello konfigurerbara parametrarna i det här elementet är:</span><span class="sxs-lookup"><span data-stu-id="74969-283">hello configurable parameters of this element are:</span></span>

* <span data-ttu-id="74969-284">**Objektet\_namn**: hello objektnamn för hello samling.</span><span class="sxs-lookup"><span data-stu-id="74969-284">**Object\_name**: hello object name for hello collection.</span></span>
* <span data-ttu-id="74969-285">**Instansen\_regex**: en *reguljärt uttryck* definierar vilka instanser toocollect.</span><span class="sxs-lookup"><span data-stu-id="74969-285">**Instance\_regex**: a *regular expression* defining which instances toocollect.</span></span> <span data-ttu-id="74969-286">Hej värde: `.*` anger alla instanser.</span><span class="sxs-lookup"><span data-stu-id="74969-286">hello value: `.*` specifies all instances.</span></span> <span data-ttu-id="74969-287">toocollect processor mätvärden för endast hello \_totala instans kan du ange `_Total`.</span><span class="sxs-lookup"><span data-stu-id="74969-287">toocollect processor metrics for only hello \_Total instance, you could specify `_Total`.</span></span> <span data-ttu-id="74969-288">toocollect Processmätvärden för hello endast crond eller sshd instanser, kan du ange: `(crond|sshd)`.</span><span class="sxs-lookup"><span data-stu-id="74969-288">toocollect process metrics for only hello crond or sshd instances, you could specify: `(crond|sshd)`.</span></span>
* <span data-ttu-id="74969-289">**Räknaren\_namn\_regex**: en *reguljärt uttryck* definierar vilka räknare (för hello) toocollect.</span><span class="sxs-lookup"><span data-stu-id="74969-289">**Counter\_name\_regex**: a *regular expression* defining which counters (for hello object) toocollect.</span></span> <span data-ttu-id="74969-290">toocollect alla räknare för hello, ange: `.*`.</span><span class="sxs-lookup"><span data-stu-id="74969-290">toocollect all counters for hello object, specify: `.*`.</span></span> <span data-ttu-id="74969-291">toocollect växla endast utrymme räknare för hello minnesobjekt, kan du ange:`.+Swap.+`</span><span class="sxs-lookup"><span data-stu-id="74969-291">toocollect only swap space counters for hello memory object, you could specify: `.+Swap.+`</span></span>
* <span data-ttu-id="74969-292">**Intervall:**: hello frekvens vid vilken hello objektets räknare samlas in.</span><span class="sxs-lookup"><span data-stu-id="74969-292">**Interval:**: hello frequency at which hello object's counters are collected.</span></span>

<span data-ttu-id="74969-293">hello standardkonfigurationen för prestandavärden är:</span><span class="sxs-lookup"><span data-stu-id="74969-293">hello default configuration for performance metrics is:</span></span>

```
<source>
  type oms_omi
  object_name "Physical Disk"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Logical Disk"
  instance_regex ".*
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Processor"
  instance_regex ".*
  counter_name_regex ".*"
  interval 30s
</source>

<source>
  type oms_omi
  object_name "Memory"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

### <a name="enable-mysql-performance-counters-using-linux-commands"></a><span data-ttu-id="74969-294">Aktivera MySQL prestandaräknare med Linux-kommandon</span><span class="sxs-lookup"><span data-stu-id="74969-294">Enable MySQL performance counters using Linux commands</span></span>
<span data-ttu-id="74969-295">Om MySQL-Server eller MariaDB Server har upptäckts på hello dator när hello omsagent paket installeras, installeras automatiskt en provider för MySQL-Server för prestandaövervakning.</span><span class="sxs-lookup"><span data-stu-id="74969-295">If MySQL Server or MariaDB Server is detected on hello computer when hello omsagent bundle is installed, a performance monitoring provider for MySQL Server is automatically installed.</span></span> <span data-ttu-id="74969-296">Den här providern ansluter toohello lokala MySQL/MariaDB server tooexpose prestandastatistik.</span><span class="sxs-lookup"><span data-stu-id="74969-296">This provider connects toohello local MySQL/MariaDB server tooexpose performance statistics.</span></span> <span data-ttu-id="74969-297">Du behöver tooconfigure MySQL användarens autentiseringsuppgifter så att hello providern kan komma åt hello MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="74969-297">You need tooconfigure MySQL user credentials so that hello provider can access hello MySQL Server.</span></span>

<span data-ttu-id="74969-298">toodefine standard användarkonton för hello MySQL-server på localhost, Använd hello efter kommandot.</span><span class="sxs-lookup"><span data-stu-id="74969-298">toodefine a default user account for hello MySQL server on localhost, use hello following command example.</span></span>

> [!NOTE]
> <span data-ttu-id="74969-299">hello autentiseringsuppgifter fil måste läsas av hello omsagent konto.</span><span class="sxs-lookup"><span data-stu-id="74969-299">hello credentials file must be readable by hello omsagent account.</span></span> <span data-ttu-id="74969-300">Du bör köra hello mycimprovauth kommando som omsgent.</span><span class="sxs-lookup"><span data-stu-id="74969-300">Running hello mycimprovauth command as omsgent is recommended.</span></span>
>
>

```
sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>

sudo /opt/omi/bin/service_control restart
```


<span data-ttu-id="74969-301">Du kan också ange hello krävs MySQL autentiseringsuppgifter i en fil genom att skapa hello: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. Mer information om hur du hanterar MySQL autentiseringsuppgifter för övervakning via hello mysql-auth-filen finns [hantera MySQL övervakning autentiseringsuppgifter i hello autentiseringsfilen](#manage-mysql-monitoring-credentials-in-the-authentication-file).</span><span class="sxs-lookup"><span data-stu-id="74969-301">Alternatively, you can specify hello required MySQL credentials in a file, by creating hello file: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. For more information about managing MySQL credentials for monitoring through hello mysql-auth file, see [Manage MySQL monitoring credentials in hello authentication file](#manage-mysql-monitoring-credentials-in-the-authentication-file).</span></span>

<span data-ttu-id="74969-302">Se [behörigheter som krävs för prestandaräknare för MySQL-databas](#database-permissions-required-for-mysql-performance-counters) för ytterligare information om objektbehörigheter som krävs av hello MySQL användaren toocollect MySQL prestandainformation.</span><span class="sxs-lookup"><span data-stu-id="74969-302">See [Database permissions required for MySQL performance counters](#database-permissions-required-for-mysql-performance-counters) for details about object permissions required by hello MySQL user toocollect MySQL Server performance data.</span></span>

### <a name="enable-apache-http-server-performance-counters-using-linux-commands"></a><span data-ttu-id="74969-303">Aktivera prestandaräknare för Apache HTTP-servern med hjälp av Linux-kommandon</span><span class="sxs-lookup"><span data-stu-id="74969-303">Enable Apache HTTP Server performance counters using Linux commands</span></span>
<span data-ttu-id="74969-304">Om Apache HTTP-Server har upptäckts på hello dator när hello omsagent paket installeras, installeras automatiskt en provider för Apache HTTP-Server för prestandaövervakning.</span><span class="sxs-lookup"><span data-stu-id="74969-304">If Apache HTTP Server is detected on hello computer when hello omsagent bundle is installed, a performance monitoring provider for Apache HTTP Server is automatically installed.</span></span> <span data-ttu-id="74969-305">Den här providern förlitar sig på en Apache ”modul” som måste läsas in i hello Apache HTTP-Server i ordning tooaccess prestandadata.</span><span class="sxs-lookup"><span data-stu-id="74969-305">This provider relies on an Apache "module" that must be loaded into hello Apache HTTP Server in order tooaccess performance data.</span></span>

<span data-ttu-id="74969-306">Du kan läsa in modulen hello med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="74969-306">You can load hello module with hello following command:</span></span>

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

<span data-ttu-id="74969-307">toounload hello Apache övervakningsmodulen, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="74969-307">toounload hello Apache monitoring module, run hello following command:</span></span>

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```
### <a name="tooview-performance-data-with-log-analytics"></a><span data-ttu-id="74969-308">tooview prestandadata med logganalys</span><span class="sxs-lookup"><span data-stu-id="74969-308">tooview performance data with Log Analytics</span></span>
1. <span data-ttu-id="74969-309">Klicka på hello loggen Sök panelen i hello Operations Management Suite-portalen.</span><span class="sxs-lookup"><span data-stu-id="74969-309">In hello Operations Management Suite portal, click hello Log Search tile.</span></span>
2. <span data-ttu-id="74969-310">Skriv i hello sökfältet `* (Type=Perf)` tooview alla prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="74969-310">In hello search bar, type `* (Type=Perf)` tooview all performance counters.</span></span>

<span data-ttu-id="74969-311">Eftersom OMS samlar även in data från Windows prestandaräknare, du bör scope på hello Sök tooLinux-specifika data.</span><span class="sxs-lookup"><span data-stu-id="74969-311">Because OMS also collects Windows performance counter data, you should scope-down hello search tooLinux-specific data.</span></span> <span data-ttu-id="74969-312">Därför visas hello följande exempel prestanda data specifika tooan exempel Linux-servern med namnet Chorizo21.</span><span class="sxs-lookup"><span data-stu-id="74969-312">So, hello following example would show performance data specific tooan example Linux server named Chorizo21.</span></span>

```
Type=Perf Computer=chorizo*
```

![exempel server visas i sökresultaten](./media/log-analytics-linux-agents/oms-perfsearch01.png)

<span data-ttu-id="74969-314">I hello resultat, kan du klicka på **mått** tooview hello räknare som data har samlats in för.</span><span class="sxs-lookup"><span data-stu-id="74969-314">In hello results, you can click **Metrics** tooview hello counters that data was collected for.</span></span> <span data-ttu-id="74969-315">Realtidsdata visas som diagram för varje räknare.</span><span class="sxs-lookup"><span data-stu-id="74969-315">Real-time data is shown as graphs for each counter.</span></span>

![metrics](./media/log-analytics-linux-agents/oms-perfmetrics01.png)

## <a name="syslog"></a><span data-ttu-id="74969-317">Syslog</span><span class="sxs-lookup"><span data-stu-id="74969-317">Syslog</span></span>
<span data-ttu-id="74969-318">Syslog är en händelseloggning protokollet liknande tooWindows händelseloggar – både fungerar på samma sätt när de visas i OMS.</span><span class="sxs-lookup"><span data-stu-id="74969-318">Syslog is an event logging protocol similar tooWindows Event logs—both operate similarly when displayed in OMS.</span></span>

### <a name="tooadd-a-new-linux-syslog-facility-in-oms"></a><span data-ttu-id="74969-319">tooadd en ny Linux syslog-funktion i OMS</span><span class="sxs-lookup"><span data-stu-id="74969-319">tooadd a new Linux syslog facility in OMS</span></span>
1. <span data-ttu-id="74969-320">På hello **inställningar** sidan **Data** , klickar du på **Syslog** och toohello vänster hello plus ikon, Skriv hello namnet på hello syslog-funktion som du vill tooadd.</span><span class="sxs-lookup"><span data-stu-id="74969-320">On hello **Settings** page under **Data** , click **Syslog** and then toohello left of hello plus icon, type hello name of hello syslog facility that you want tooadd.</span></span>
    <span data-ttu-id="74969-321">![Linux syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)</span><span class="sxs-lookup"><span data-stu-id="74969-321">![Linux syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)</span></span>
2. <span data-ttu-id="74969-322">Om du inte vet hello fullständiga namn hello och du kan börja skriva ett partiellt namn och en lista över tillgängliga syslog verksamhet visas.</span><span class="sxs-lookup"><span data-stu-id="74969-322">If you don’t know hello full name of hello facility, you can start typing a partial name and a list of available syslog facilities will appear.</span></span> <span data-ttu-id="74969-323">När du hittar hello syslog-funktion som du vill tooadd på hello namn i hello listan och klicka sedan på hello plus ikonen tooadd hello syslog-funktion.</span><span class="sxs-lookup"><span data-stu-id="74969-323">When you find hello syslog facility that you want tooadd, click hello name in hello list and then click hello plus icon tooadd hello syslog facility.</span></span>
3. <span data-ttu-id="74969-324">När du lägger till hello och det visas i hello listan över markerade med en färgade stapel.</span><span class="sxs-lookup"><span data-stu-id="74969-324">After you add hello facility, it appears in hello list of highlighted with a colored bar.</span></span> <span data-ttu-id="74969-325">Därefter väljer du hello allvarlighetsgraderna (informationskategorier syslog anläggning) som du vill toocollect.</span><span class="sxs-lookup"><span data-stu-id="74969-325">Next, choose hello severities (categories of syslog facility information) that you want toocollect.</span></span>
4. <span data-ttu-id="74969-326">Hello längst ned på sidan för hello klickar du på **spara** toofinalize ändringarna.</span><span class="sxs-lookup"><span data-stu-id="74969-326">At hello bottom of hello page click **Save** toofinalize your changes.</span></span> <span data-ttu-id="74969-327">hello konfigurationsändringar som du har gjort skickas sedan tooall hello OMS-Agent för Linux som har registrerats med OMS, vanligtvis inom 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="74969-327">hello configuration changes that you’ve made are then sent tooall hello OMS Agents for Linux that are registered with OMS, typically within 5 minutes.</span></span>

### <a name="configure-linux-syslog-facilities-in-linux"></a><span data-ttu-id="74969-328">Konfigurera Linux syslog anläggningarna i Linux</span><span class="sxs-lookup"><span data-stu-id="74969-328">Configure Linux syslog facilities in Linux</span></span>
<span data-ttu-id="74969-329">Syslog-händelser skickas från hello daemon för syslog, till exempel rsyslog eller syslog-ng, tooa lokal port hello agenten lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="74969-329">Syslog events are sent from hello syslog daemon, for example rsyslog or syslog-ng, tooa local port that hello agent is listening on.</span></span> <span data-ttu-id="74969-330">Som standard port 25224.</span><span class="sxs-lookup"><span data-stu-id="74969-330">By default, port 25224.</span></span> <span data-ttu-id="74969-331">När hello-agenten är installerad, används en standardkonfiguration för syslog.</span><span class="sxs-lookup"><span data-stu-id="74969-331">When hello agent is installed, a default syslog configuration is applied.</span></span> <span data-ttu-id="74969-332">Detta är finns på:</span><span class="sxs-lookup"><span data-stu-id="74969-332">This is found at:</span></span>

<span data-ttu-id="74969-333">Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf</span><span class="sxs-lookup"><span data-stu-id="74969-333">Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf</span></span>

<span data-ttu-id="74969-334">Syslog-ng: /etc/syslog-ng/syslog-ng.conf</span><span class="sxs-lookup"><span data-stu-id="74969-334">Syslog-ng: /etc/syslog-ng/syslog-ng.conf</span></span>

<span data-ttu-id="74969-335">hello OMS-agenten syslog standardkonfigurationen filöverföringar syslog-händelser från alla lokaler med en allvarlighetsgrad för varning eller högre.</span><span class="sxs-lookup"><span data-stu-id="74969-335">hello default OMS agent syslog configuration uploads syslog events from all facilities with a severity of warning or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="74969-336">Om du redigerar hello syslog konfiguration, måste du starta om hello syslog-daemon för hello ändringar tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="74969-336">If you edit hello syslog configuration, you must restart hello syslog daemon for hello changes tootake effect.</span></span>
>
>

<span data-ttu-id="74969-337">hello syslog standardkonfigurationen för hello OMS-Agent för Linux för OMS är:</span><span class="sxs-lookup"><span data-stu-id="74969-337">hello default syslog configuration for hello OMS Agent for Linux for OMS is:</span></span>

#### <a name="rsyslog"></a><span data-ttu-id="74969-338">rsyslog</span><span class="sxs-lookup"><span data-stu-id="74969-338">Rsyslog</span></span>
```
kern.warning       @127.0.0.1:25224
user.warning       @127.0.0.1:25224
daemon.warning     @127.0.0.1:25224
auth.warning       @127.0.0.1:25224
syslog.warning     @127.0.0.1:25224
uucp.warning       @127.0.0.1:25224
authpriv.warning   @127.0.0.1:25224
ftp.warning        @127.0.0.1:25224
cron.warning       @127.0.0.1:25224
local0.warning     @127.0.0.1:25224
local1.warning     @127.0.0.1:25224
local2.warning     @127.0.0.1:25224
local3.warning     @127.0.0.1:25224
local4.warning     @127.0.0.1:25224
local5.warning     @127.0.0.1:25224
local6.warning     @127.0.0.1:25224
local7.warning     @127.0.0.1:25224
```

#### <a name="syslog-ng"></a><span data-ttu-id="74969-339">Syslog-ng</span><span class="sxs-lookup"><span data-stu-id="74969-339">Syslog-ng</span></span>
```
#OMS_facility = all
filter f_warning_oms { level(warning); };
destination warning_oms { tcp("127.0.0.1" port(25224)); };
log { source(src); filter(f_warning_oms); destination(warning_oms); };
```

### <a name="tooview-all-syslog-events-with-log-analytics"></a><span data-ttu-id="74969-340">tooview Syslog-händelser med logganalys</span><span class="sxs-lookup"><span data-stu-id="74969-340">tooview all Syslog events with Log Analytics</span></span>
1. <span data-ttu-id="74969-341">I hello Operations Management Suite-portalen klickar du på hello **loggen Sök** panelen.</span><span class="sxs-lookup"><span data-stu-id="74969-341">In hello Operations Management Suite portal, click hello **Log Search** tile.</span></span>
2. <span data-ttu-id="74969-342">I hello **hantering** gruppering, Välj en fördefinierad syslog-sökning och välj sedan en toorun den.</span><span class="sxs-lookup"><span data-stu-id="74969-342">In hello **Log Management** grouping, choose a predefined syslog search and then select one toorun it.</span></span>

<span data-ttu-id="74969-343">Det här exemplet visar alla Syslog-händelser.</span><span class="sxs-lookup"><span data-stu-id="74969-343">This example shows all Syslog events.</span></span>

![Syslog-händelser som visas i loggen Sök](./media/log-analytics-linux-agents/oms-linux-syslog.png)

<span data-ttu-id="74969-345">Du kan nu detaljerat sökresultat.</span><span class="sxs-lookup"><span data-stu-id="74969-345">Now you can drill into search results.</span></span>

## <a name="linux-alerts"></a><span data-ttu-id="74969-346">Linux-aviseringar</span><span class="sxs-lookup"><span data-stu-id="74969-346">Linux alerts</span></span>
<span data-ttu-id="74969-347">Om du använder Nagios eller Zabbix toomanage Linux-datorer och sedan OMS kan ta emot hello varningar som genereras av dessa verktyg.</span><span class="sxs-lookup"><span data-stu-id="74969-347">If you use Nagios or Zabbix toomanage your Linux machines, then OMS can receive hello alerts generated from those tools.</span></span> <span data-ttu-id="74969-348">Det finns för närvarande inga metoden tooconfigure inkommande aviseringsdata med hello OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="74969-348">However, there is currently no method tooconfigure incoming alert data using hello OMS portal.</span></span> <span data-ttu-id="74969-349">I stället måste tooedit en config-fil toostart Skicka aviseringar-tooOMS.</span><span class="sxs-lookup"><span data-stu-id="74969-349">Instead, you will need tooedit a config file toostart sending alerts tooOMS.</span></span>

### <a name="collect-alerts-from-nagios"></a><span data-ttu-id="74969-350">Samla in aviseringar från Nagios</span><span class="sxs-lookup"><span data-stu-id="74969-350">Collect alerts from Nagios</span></span>
<span data-ttu-id="74969-351">toocollect aviseringar från en Nagios-server, måste toomake hello efter konfigurationsändringar.</span><span class="sxs-lookup"><span data-stu-id="74969-351">toocollect alerts from a Nagios server, you need toomake hello following configuration changes.</span></span>

1. <span data-ttu-id="74969-352">Bevilja hello användaren **omsagent** läsbehörighet toohello Nagios-loggfilen (d.v.s. /var/log/nagios/nagios.log).</span><span class="sxs-lookup"><span data-stu-id="74969-352">Grant hello user **omsagent** read access toohello Nagios log file (i.e. /var/log/nagios/nagios.log).</span></span> <span data-ttu-id="74969-353">Under förutsättning att hello nagios.log filen ägs av hello grupp **nagios** , du kan lägga till användaren hello **omsagent** toohello **nagios** grupp.</span><span class="sxs-lookup"><span data-stu-id="74969-353">Assuming hello nagios.log file is owned by hello group **nagios** , you can add hello user **omsagent** toohello **nagios** group.</span></span>

    ```
    sudo usermod –a -G nagios omsagent
    ```
2. <span data-ttu-id="74969-354">Ändra hello omsagent.confconfiguration filen (/ etc/opt/microsoft/omsagent/&lt;arbetsyte-id&gt;/conf/omsagent.conf).</span><span class="sxs-lookup"><span data-stu-id="74969-354">Modify hello omsagent.confconfiguration file (/etc/opt/microsoft/omsagent/&lt;workspace id&gt;/conf/omsagent.conf).</span></span> <span data-ttu-id="74969-355">Kontrollera att följande poster hello finns och inte kommenterad ut:</span><span class="sxs-lookup"><span data-stu-id="74969-355">Ensure hello following entries are present and not commented out:</span></span>

    ```
    <source>
    type tail
    #Update path toopoint tooyour nagios.log
    path /var/log/nagios/nagios.log
    format none
    tag oms.nagios
    </source>

    <filter oms.nagios>
    type filter_nagios_log
    </filter>
    ```
3. <span data-ttu-id="74969-356">Starta om hello omsagent daemon:</span><span class="sxs-lookup"><span data-stu-id="74969-356">Restart hello omsagent daemon:</span></span>

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="collect-alerts-from-zabbix"></a><span data-ttu-id="74969-357">Samla in aviseringar från Zabbix</span><span class="sxs-lookup"><span data-stu-id="74969-357">Collect alerts from Zabbix</span></span>
<span data-ttu-id="74969-358">toocollect varningar från en Zabbix-server du utför liknande steg toothose för Nagios ovan, men du behöver toospecify en användare och lösenord i *klartext*.</span><span class="sxs-lookup"><span data-stu-id="74969-358">toocollect alerts from a Zabbix server, you'll perform similar steps toothose for Nagios above, except you'll need toospecify a user and password in *clear text*.</span></span> <span data-ttu-id="74969-359">Detta är inte idealiskt, men troligen ändrar snart.</span><span class="sxs-lookup"><span data-stu-id="74969-359">This is not ideal, but will likely change soon.</span></span> <span data-ttu-id="74969-360">tooaddress problemet, rekommenderar vi att du skapar hello användare och bevilja behörighet toomonitor endast.</span><span class="sxs-lookup"><span data-stu-id="74969-360">tooaddress this issue, we recommend that you create hello user and grant it permission toomonitor only.</span></span>

<span data-ttu-id="74969-361">En exempel-avsnittet i konfigurationsfilen för hello omsagent.conf (/ etc/opt/microsoft/omsagent/&lt;arbetsyte-id&gt;/conf/omsagent.conf) för Zabbix bör likna följande hello:</span><span class="sxs-lookup"><span data-stu-id="74969-361">An example section of hello omsagent.conf configuration file  (/etc/opt/microsoft/omsagent/&lt;workspace id&gt;/conf/omsagent.conf) for Zabbix should resemble hello following:</span></span>

```
<source>
  type zabbix_alerts
  run_interval 1m
  tag oms.zabbix
  zabbix_url http://localhost/zabbix/api_jsonrpc.php
  zabbix_username Admin
  zabbix_password zabbix
</source>

```

### <a name="view-alerts-in-log-analytics-search"></a><span data-ttu-id="74969-362">Visa aviseringar i logganalys-sökning</span><span class="sxs-lookup"><span data-stu-id="74969-362">View alerts in Log Analytics search</span></span>
<span data-ttu-id="74969-363">När du har konfigurerat ditt Linux-datorer toosend aviseringar tooOMS kan använda du några enkla loggen Sök frågor tooview hello aviseringar.</span><span class="sxs-lookup"><span data-stu-id="74969-363">After you've configured your Linux computers toosend alerts tooOMS, you can use a few simple log search queries tooview hello alerts.</span></span> <span data-ttu-id="74969-364">hello returneras följande Sök frågan exempel alla hello registreras aviseringar som genererades.</span><span class="sxs-lookup"><span data-stu-id="74969-364">hello following search query example returns all hello recorded alerts that were generated.</span></span> <span data-ttu-id="74969-365">Om någon form av ett problem inträffar i din IT-infrastruktur, resulterar t.ex, sedan för hello följande exempel fråga hända om hello problem kan kommer.</span><span class="sxs-lookup"><span data-stu-id="74969-365">For example, if some sort of problem occurs in your IT infrastructure, then results for hello following example query might indicate where hello problem might originate.</span></span> <span data-ttu-id="74969-366">Och du kan enkelt detaljgranska i toohello aviseringar av källa system toohelp smala undersökningen.</span><span class="sxs-lookup"><span data-stu-id="74969-366">And, you can easily drill in toohello alerts by source system toohelp narrow your investigation.</span></span> <span data-ttu-id="74969-367">hello fördelen är att du inte nödvändigtvis behöver toogo toovarious hanteringssystem från hello start, förutsatt att aviseringar ska skickas tooOMS, du kan starta det.</span><span class="sxs-lookup"><span data-stu-id="74969-367">hello benefit is that you don't necessarily have toogo toovarious management systems from hello start—provided that your alerts are sent tooOMS, you can start there.</span></span>

```
Type=Alert
```

#### <a name="tooview-all-nagios-alerts-with-log-analytics"></a><span data-ttu-id="74969-368">tooview alla Nagios aviseringar med logganalys</span><span class="sxs-lookup"><span data-stu-id="74969-368">tooview all Nagios alerts with Log Analytics</span></span>
1. <span data-ttu-id="74969-369">I hello Operations Management Suite-portalen klickar du på hello **loggen Sök** panelen.</span><span class="sxs-lookup"><span data-stu-id="74969-369">In hello Operations Management Suite portal, click hello **Log Search** tile.</span></span>
2. <span data-ttu-id="74969-370">Adressfältet i hello frågan hello följande sökfråga</span><span class="sxs-lookup"><span data-stu-id="74969-370">In hello query bar, type hello following search query</span></span>

    ```
    Type=Alert SourceSystem=Nagios
    ```
   ![Nagios aviseringar visas i loggen Sök](./media/log-analytics-linux-agents/oms-linux-nagios-alerts.png)

<span data-ttu-id="74969-372">När du ser hello sökresultat, detaljer om ytterligare information som *in AlertState*.</span><span class="sxs-lookup"><span data-stu-id="74969-372">After you see hello search results, you can drill into additional details such as *AlertState*.</span></span>

### <a name="tooview-all-zabbix-alerts-with-log-analytics"></a><span data-ttu-id="74969-373">tooview alla Zabbix aviseringar med logganalys</span><span class="sxs-lookup"><span data-stu-id="74969-373">tooview all Zabbix alerts with Log Analytics</span></span>
1. <span data-ttu-id="74969-374">I hello Operations Management Suite-portalen klickar du på hello **loggen Sök** panelen.</span><span class="sxs-lookup"><span data-stu-id="74969-374">In hello Operations Management Suite portal, click hello **Log Search** tile.</span></span>
2. <span data-ttu-id="74969-375">Adressfältet i hello frågan hello följande sökfråga</span><span class="sxs-lookup"><span data-stu-id="74969-375">In hello query bar, type hello following search query</span></span>

    ```
    Type=Alert SourceSystem=Zabbix
    ```
   ![Zabbix aviseringar visas i loggen Sök](./media/log-analytics-linux-agents/oms-linux-zabbix-alerts.png)

<span data-ttu-id="74969-377">När du ser hello sökresultat, detaljer om ytterligare information som *AlertName*.</span><span class="sxs-lookup"><span data-stu-id="74969-377">After you see hello search results, you can drill into additional details such as *AlertName*.</span></span>

## <a name="compatibility-with-system-center-operations-manager"></a><span data-ttu-id="74969-378">Kompatibilitet med System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="74969-378">Compatibility with System Center Operations Manager</span></span>
<span data-ttu-id="74969-379">hello OMS-Agent för Linux delar agent binärfiler med hello System Center Operations Manager-agenten.</span><span class="sxs-lookup"><span data-stu-id="74969-379">hello OMS Agent for Linux shares agent binaries with hello System Center Operations Manager agent.</span></span> <span data-ttu-id="74969-380">Installera hello OMS-Agent för Linux på ett system som för närvarande hanteras av Operations Manager hello uppgraderingar OMI och SCX paket på hello datorn tooa nyare version.</span><span class="sxs-lookup"><span data-stu-id="74969-380">Installing hello OMS Agent for Linux on a system currently managed by Operations Manager upgrades hello OMI and SCX packages on hello computer tooa newer version.</span></span> <span data-ttu-id="74969-381">hello OMS-Agent för Linux och System Center 2012 R2 är kompatibla.</span><span class="sxs-lookup"><span data-stu-id="74969-381">hello OMS Agent for Linux and System Center 2012 R2 are compatible.</span></span> <span data-ttu-id="74969-382">Dock **System Center 2012 SP1 och tidigare versioner är inte kompatibla eller stöds med hello OMS-Agent för Linux.**</span><span class="sxs-lookup"><span data-stu-id="74969-382">However, **System Center 2012 SP1 and earlier versions are currently not compatible or supported with hello OMS Agent for Linux.**</span></span>

> [!NOTE]
> <span data-ttu-id="74969-383">Om du senare vill toomanage hello datorn med Operations Manager hello OMS-Agent för Linux är installerade tooa dator som för närvarande inte hanteras av Operations Manager, måste du ändra hello OMI konfiguration innan du identifiera hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="74969-383">If hello OMS Agent for Linux is installed tooa computer that is not currently managed by Operations Manager, and you later want toomanage hello computer with Operations Manager, you must modify hello OMI configuration before you discover hello computer.</span></span> <span data-ttu-id="74969-384">**Det här steget behövs inte om hello Operations Manager-agenten är installerad före hello OMS-Agent för Linux.**</span><span class="sxs-lookup"><span data-stu-id="74969-384">**This step is not needed if hello Operations Manager agent is installed before hello OMS Agent for Linux.**</span></span>
>
>

### <a name="tooenable-hello-oms-agent-for-linux-toocommunicate-with-operations-manager"></a><span data-ttu-id="74969-385">tooenable hello OMS-Agent för Linux toocommunicate med Operations Manager</span><span class="sxs-lookup"><span data-stu-id="74969-385">tooenable hello OMS Agent for Linux toocommunicate with Operations Manager</span></span>
1. <span data-ttu-id="74969-386">Redigera hello filen /etc/opt/omi/conf/omiserver.conf</span><span class="sxs-lookup"><span data-stu-id="74969-386">Edit hello file /etc/opt/omi/conf/omiserver.conf</span></span>
2. <span data-ttu-id="74969-387">Se till att hello rad som börjar med **httpsport =** definierar hello port 1270.</span><span class="sxs-lookup"><span data-stu-id="74969-387">Ensure that hello line beginning with **httpsport=** defines hello port 1270.</span></span> <span data-ttu-id="74969-388">Exempel`httpsport=1270`</span><span class="sxs-lookup"><span data-stu-id="74969-388">Such as `httpsport=1270`</span></span>
3. <span data-ttu-id="74969-389">Starta om hello OMI-servern:</span><span class="sxs-lookup"><span data-stu-id="74969-389">Restart hello OMI server:</span></span>

    ```
    sudo /opt/omi/bin/service_control restart
    ```

## <a name="database-permissions-required-for-mysql-performance-counters"></a><span data-ttu-id="74969-390">Databasbehörigheter som krävs för MySQL-prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="74969-390">Database permissions required for MySQL performance counters</span></span>
<span data-ttu-id="74969-391">toogrant behörigheter tooa MySQL övervakning hello beviljande användaren måste användaren, ha hello alternativet för BEVILJA behörighet samt hello behörighet beviljas.</span><span class="sxs-lookup"><span data-stu-id="74969-391">toogrant permissions tooa MySQL monitoring user, hello granting user must have hello 'GRANT option' privilege as well as hello privilege being granted.</span></span>

<span data-ttu-id="74969-392">För att hello MySQL användaren tooreturn prestanda data hello användare kommer behöver åtkomst till toohello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="74969-392">In order for hello MySQL User tooreturn performance data hello user will need access toohello following queries:</span></span>

```
SHOW GLOBAL STATUS;
SHOW GLOBAL VARIABLES:
```

<span data-ttu-id="74969-393">Dessutom kräver toothese frågor hello MySQL användaren väljer åtkomst toohello följande standardtabeller:</span><span class="sxs-lookup"><span data-stu-id="74969-393">In addition toothese queries hello MySQL user requires SELECT access toohello following default tables:</span></span>

* <span data-ttu-id="74969-394">INFORMATION_SCHEMA</span><span class="sxs-lookup"><span data-stu-id="74969-394">information_schema</span></span>
* <span data-ttu-id="74969-395">MySQL</span><span class="sxs-lookup"><span data-stu-id="74969-395">mysql</span></span>

<span data-ttu-id="74969-396">Dessa behörigheter kan tilldelas genom att köra hello följande grant-kommandon.</span><span class="sxs-lookup"><span data-stu-id="74969-396">These privileges can be granted by running hello following grant commands.</span></span>

```
GRANT SELECT ON information_schema.* too‘monuser’@’localhost’;
GRANT SELECT ON mysql.* too‘monuser’@’localhost’;
```

## <a name="manage-mysql-monitoring-credentials-in-hello-authentication-file"></a><span data-ttu-id="74969-397">Hantera MySQL övervakning autentiseringsuppgifter i hello autentiseringsfilen</span><span class="sxs-lookup"><span data-stu-id="74969-397">Manage MySQL monitoring credentials in hello authentication file</span></span>
<span data-ttu-id="74969-398">hello följande avsnitt hjälper dig att hantera MySQL-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="74969-398">hello following sections help you manage MySQL credentials.</span></span>

### <a name="configure-hello-mysql-omi-provider"></a><span data-ttu-id="74969-399">Konfigurera hello MySQL OMI-providern</span><span class="sxs-lookup"><span data-stu-id="74969-399">Configure hello MySQL OMI provider</span></span>
<span data-ttu-id="74969-400">hello MySQL OMI-providern kräver att en förkonfigurerad MySQL-användare och installerat MySQL klientbibliotek i ordning tooquery hello prestanda/hälsoinformation från hello MySQL-instans.</span><span class="sxs-lookup"><span data-stu-id="74969-400">hello MySQL OMI provider requires a preconfigured MySQL user and installed MySQL client libraries in order tooquery hello performance/health information from hello MySQL instance.</span></span>

### <a name="mysql-omi-authentication-file"></a><span data-ttu-id="74969-401">MySQL OMI autentiseringsfilen</span><span class="sxs-lookup"><span data-stu-id="74969-401">MySQL OMI authentication file</span></span>
<span data-ttu-id="74969-402">MySQL OMI-providern använder en autentisering filen toodetermine vilka bind-adress och port hello MySQL instans lyssnar på och vilka autentiseringsuppgifter toouse toogather mått.</span><span class="sxs-lookup"><span data-stu-id="74969-402">MySQL OMI provider uses an authentication file toodetermine what bind-address and port hello MySQL instance is listening on and what credentials toouse toogather metrics.</span></span> <span data-ttu-id="74969-403">Under installationen hello MySQL OMI genomsöks providern MySQL my.cnf configuration-filer (standardplatserna) för bind-adress och port och delvis set hello MySQL OMI autentiseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="74969-403">During installation hello MySQL OMI provider will scan MySQL my.cnf configuration files (default locations) for bind-address and port and partially set hello MySQL OMI authentication file.</span></span>

<span data-ttu-id="74969-404">toocomplete övervakning av en MySQL-serverinstans Lägg till en förgenererade MySQL OMI autentisering fil i hello rätt katalog.</span><span class="sxs-lookup"><span data-stu-id="74969-404">toocomplete monitoring of a MySQL server instance, add a pre-generated MySQL OMI authentication file into hello correct directory.</span></span>

### <a name="authentication-file-format"></a><span data-ttu-id="74969-405">Filformatet för autentisering</span><span class="sxs-lookup"><span data-stu-id="74969-405">Authentication file format</span></span>
<span data-ttu-id="74969-406">hello MySQL OMI autentiseringsfilen är en textfil som innehåller information om:</span><span class="sxs-lookup"><span data-stu-id="74969-406">hello MySQL OMI authentication file is a text file that contains information about:</span></span>

* <span data-ttu-id="74969-407">Port</span><span class="sxs-lookup"><span data-stu-id="74969-407">Port</span></span>
* <span data-ttu-id="74969-408">Bind-adress</span><span class="sxs-lookup"><span data-stu-id="74969-408">Bind-Address</span></span>
* <span data-ttu-id="74969-409">MySQL-användarnamn</span><span class="sxs-lookup"><span data-stu-id="74969-409">MySQL username</span></span>
* <span data-ttu-id="74969-410">Base64-kodat lösenord</span><span class="sxs-lookup"><span data-stu-id="74969-410">Base64 encoded password</span></span>

<span data-ttu-id="74969-411">hello MySQL OMI autentiseringsfilen ger endast behörighet för läsning och skrivning toohello Linux användaren som skapade den.</span><span class="sxs-lookup"><span data-stu-id="74969-411">hello MySQL OMI authentication file only grants privileges for read/write toohello Linux user that generated it.</span></span>

```
[Port]=[Bind-Address], [username], [Base64 encoded Password]
(Port)=(Bind-Address), (username), (Base64 encoded Password)
(Port)=(Bind-Address), (username), (Base64 encoded Password)
AutoUpdate=[true|false]
```

<span data-ttu-id="74969-412">Standard MySQL OMI autentiseringsfilen innehåller en standardinstans och ett portnummer beroende på vilken information som är tillgänglig och parsade från hello MySQL konfigurationsfil hittades.</span><span class="sxs-lookup"><span data-stu-id="74969-412">A default MySQL OMI authentication file contains a default instance and a port number depending on what information is available and parsed from hello found MySQL configuration file.</span></span>

<span data-ttu-id="74969-413">hello standardinstansen är ett sätt toomake hantera flera MySQL-instanser på en Linux-värd enklare och markeras med hello-instans med port 0.</span><span class="sxs-lookup"><span data-stu-id="74969-413">hello default instance is a means toomake managing multiple MySQL instances on one Linux host easier, and is denoted by hello instance with port 0.</span></span> <span data-ttu-id="74969-414">Alla tillagda instanser ärver egenskaper från hello standardinstansen.</span><span class="sxs-lookup"><span data-stu-id="74969-414">All added instances will inherit properties set from hello default instance.</span></span> <span data-ttu-id="74969-415">Till exempel om MySQL-instans som lyssnar på port '3308' läggs kommer hello standardinstans bind-adress, användarnamn och lösenord för Base64-kodade att använda tootry och övervaka hello-instans som lyssnar på 3308.</span><span class="sxs-lookup"><span data-stu-id="74969-415">For example, if MySQL instance listening on port '3308' is added, hello default instance's bind-address, username, and Base64 encoded password will be used tootry and monitor hello instance listening on 3308.</span></span> <span data-ttu-id="74969-416">Om hello-instans på 3308 är bundits tooanother adress och använder hello samma MySQL-användarnamn och lösenord par hello endast respecification av hello bind-adress behövs och hello andra egenskaper ärvs.</span><span class="sxs-lookup"><span data-stu-id="74969-416">If hello instance on 3308 is binded tooanother address and uses hello same MySQL username and password pair only hello respecification of hello bind-address is needed and hello other properties will be inherited.</span></span>

<span data-ttu-id="74969-417">Exempel på hello autentiseringsfilen likna följande hello.</span><span class="sxs-lookup"><span data-stu-id="74969-417">Examples of hello authentication file resemble hello following.</span></span>

<span data-ttu-id="74969-418">Standardinstansen och instans med port 3308:</span><span class="sxs-lookup"><span data-stu-id="74969-418">Default instance and instance with port 3308:</span></span>

```
0=127.0.0.1, myuser, cnBwdA==3308=, ,AutoUpdate=true
```

<span data-ttu-id="74969-419">Standardinstansen och -instans med port 3308 + olika Base64-kodade lösenord:</span><span class="sxs-lookup"><span data-stu-id="74969-419">Default instance and instance with port 3308 + different Base 64 encoded password:</span></span>

```
0=127.0.0.1, myuser, cnBwdA==3308=127.0.1.1, , AutoUpdate=true
```


| <span data-ttu-id="74969-420">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="74969-420">**Property**</span></span> | <span data-ttu-id="74969-421">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="74969-421">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="74969-422">Port</span><span class="sxs-lookup"><span data-stu-id="74969-422">Port</span></span> |<span data-ttu-id="74969-423">Port representerar hello aktuella port hello MySQL instans lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="74969-423">Port represents hello current port hello MySQL instance is listening on.</span></span>  <span data-ttu-id="74969-424">hello port 0 innebär att hello egenskaper följande används för standardinstansen.</span><span class="sxs-lookup"><span data-stu-id="74969-424">hello port 0 implies that hello properties following are used for default instance.</span></span> |
| <span data-ttu-id="74969-425">Bind-adress</span><span class="sxs-lookup"><span data-stu-id="74969-425">Bind-Address</span></span> |<span data-ttu-id="74969-426">hello binda adress är hello aktuella MySQL bind-adress</span><span class="sxs-lookup"><span data-stu-id="74969-426">hello Bind Address is hello current MySQL bind-address</span></span> |
| <span data-ttu-id="74969-427">användarnamn</span><span class="sxs-lookup"><span data-stu-id="74969-427">username</span></span> |<span data-ttu-id="74969-428">Hello användarnamnet för hello MySQL användare vill toouse toomonitor hello MySQL server-instansen.</span><span class="sxs-lookup"><span data-stu-id="74969-428">This hello username of hello MySQL user you wish toouse toomonitor hello MySQL server instance.</span></span> |
| <span data-ttu-id="74969-429">Base64-kodat lösenord</span><span class="sxs-lookup"><span data-stu-id="74969-429">Base64 encoded Password</span></span> |<span data-ttu-id="74969-430">Detta är hello lösenord för hello MySQL övervakning användare kodad i Base64.</span><span class="sxs-lookup"><span data-stu-id="74969-430">This is hello password of hello MySQL monitoring user encoded in Base64.</span></span> |
| <span data-ttu-id="74969-431">Automatisk uppdatering</span><span class="sxs-lookup"><span data-stu-id="74969-431">AutoUpdate</span></span> |<span data-ttu-id="74969-432">När hello MySQL OMI providern uppgraderas hello-providern genomsökning efter ändringar i hello my.cnf filen och skriva över hello MySQL OMI autentiseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="74969-432">When hello MySQL OMI Provider is upgraded hello provider will rescan for changes in hello my.cnf file and overwrite hello MySQL OMI Authentication file.</span></span> <span data-ttu-id="74969-433">Ange den här flaggan tootrue eller false beroende på nödvändiga uppdateringar toohello MySQL OMI autentiseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="74969-433">Set this flag tootrue or false depending on required updates toohello MySQL OMI authentication file.</span></span> |

#### <a name="authentication-file-location"></a><span data-ttu-id="74969-434">Filplats för autentisering</span><span class="sxs-lookup"><span data-stu-id="74969-434">Authentication file location</span></span>
<span data-ttu-id="74969-435">hello MySQL OMI autentiseringsfilen bör finns i hello följande plats och med namnet ”mysql-auth”:</span><span class="sxs-lookup"><span data-stu-id="74969-435">hello MySQL OMI Authentication File should be located in hello following location and named "mysql-auth":</span></span>

<span data-ttu-id="74969-436">/var/OPT/Microsoft/MySQL-cimprov/auth/omsagent/MySQL-auth</span><span class="sxs-lookup"><span data-stu-id="74969-436">/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth</span></span>

<span data-ttu-id="74969-437">hello-filen (och auth/omsagent directory) bör ägas av hello omsagent användare.</span><span class="sxs-lookup"><span data-stu-id="74969-437">hello file (and auth/omsagent directory) should be owned by hello omsagent user.</span></span>

## <a name="agent-logs"></a><span data-ttu-id="74969-438">Agenten loggar</span><span class="sxs-lookup"><span data-stu-id="74969-438">Agent logs</span></span>
<span data-ttu-id="74969-439">hello hello OMS-Agent för Linux-loggarna finns på:</span><span class="sxs-lookup"><span data-stu-id="74969-439">hello logs for hello OMS Agent for Linux is at:</span></span>

<span data-ttu-id="74969-440">/ var/opt/microsoft/omsagent/&lt;arbetsyte-id&gt;/log/</span><span class="sxs-lookup"><span data-stu-id="74969-440">/var/opt/microsoft/omsagent/&lt;workspace id&gt;/log/</span></span>

<span data-ttu-id="74969-441">hello-loggarna för hello OMS-Agent för Linux för omsconfig (agentkonfiguration) programmet finns på:</span><span class="sxs-lookup"><span data-stu-id="74969-441">hello logs for hello OMS Agent for Linux for omsconfig (agent configuration) program is at:</span></span>

<span data-ttu-id="74969-442">/ var/opt/microsoft/omsconfig/log /</span><span class="sxs-lookup"><span data-stu-id="74969-442">/var/opt/microsoft/omsconfig/log/</span></span>

<span data-ttu-id="74969-443">Loggar för hello OMI och SCX-komponenter (som tillhandahåller mått prestandadata) finns på:</span><span class="sxs-lookup"><span data-stu-id="74969-443">Logs for hello OMI and SCX components (which provide performance metrics data) is at:</span></span>

<span data-ttu-id="74969-444">/ var/opt/omi/log/och /var/opt/microsoft/scx/log</span><span class="sxs-lookup"><span data-stu-id="74969-444">/var/opt/omi/log/ and /var/opt/microsoft/scx/log</span></span>

## <a name="troubleshooting-hello-oms-agent-for-linux"></a><span data-ttu-id="74969-445">Felsöka hello OMS-Agent för Linux</span><span class="sxs-lookup"><span data-stu-id="74969-445">Troubleshooting hello OMS Agent for Linux</span></span>
<span data-ttu-id="74969-446">Använd följande information toodiagnose hello och felsöka vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="74969-446">Use hello following information toodiagnose and troubleshoot common issues.</span></span>

<span data-ttu-id="74969-447">Om ingen av hello felsökningsinformation i det här avsnittet hjälper dig att kan du också använda hello följande resurser toohelp Lös problemet.</span><span class="sxs-lookup"><span data-stu-id="74969-447">If none of hello troubleshooting information in this section helps you, you can also use hello following resources toohelp resolve your problem.</span></span>

* <span data-ttu-id="74969-448">Kunder med Premier support Logga ett supportärende via [Premier](https://premier.microsoft.com/)</span><span class="sxs-lookup"><span data-stu-id="74969-448">Customers with Premier support can log a support case via [Premier](https://premier.microsoft.com/)</span></span>
* <span data-ttu-id="74969-449">Kunder med Azure supportavtal kan logga Supportfall i hello [Azure-portalen](https://manage.windowsazure.com/?getsupport=true)</span><span class="sxs-lookup"><span data-stu-id="74969-449">Customers with Azure support agreements can log support cases in hello [Azure portal](https://manage.windowsazure.com/?getsupport=true)</span></span>
* <span data-ttu-id="74969-450">Filen en [GitHub problemet](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)</span><span class="sxs-lookup"><span data-stu-id="74969-450">File a [GitHub Issue](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)</span></span>
* <span data-ttu-id="74969-451">Feedbackforum för uppslag och toocreate en felrapport [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)</span><span class="sxs-lookup"><span data-stu-id="74969-451">Feedback forum for ideas and toocreate a bug report [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)</span></span>

### <a name="important-log-locations"></a><span data-ttu-id="74969-452">Viktiga loggfilernas placering</span><span class="sxs-lookup"><span data-stu-id="74969-452">Important log locations</span></span>
| <span data-ttu-id="74969-453">Fil</span><span class="sxs-lookup"><span data-stu-id="74969-453">File</span></span> | <span data-ttu-id="74969-454">Sökväg</span><span class="sxs-lookup"><span data-stu-id="74969-454">Path</span></span> |
| --- | --- |
| <span data-ttu-id="74969-455">OMS-Agent för Linux-loggfil</span><span class="sxs-lookup"><span data-stu-id="74969-455">OMS Agent for Linux Log File</span></span> |`/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log ` |
| <span data-ttu-id="74969-456">Loggfil för OMS-Agent konfiguration</span><span class="sxs-lookup"><span data-stu-id="74969-456">OMS Agent Configuration Log File</span></span> |`/var/opt/microsoft/omsconfig/omsconfig.log` |

### <a name="important-configuration-files"></a><span data-ttu-id="74969-457">Viktiga konfigurationsfiler</span><span class="sxs-lookup"><span data-stu-id="74969-457">Important configuration files</span></span>
| <span data-ttu-id="74969-458">Catergory</span><span class="sxs-lookup"><span data-stu-id="74969-458">Catergory</span></span> | <span data-ttu-id="74969-459">Filplats</span><span class="sxs-lookup"><span data-stu-id="74969-459">File Location</span></span> |
| --- | --- |
| <span data-ttu-id="74969-460">Syslog</span><span class="sxs-lookup"><span data-stu-id="74969-460">Syslog</span></span> |<span data-ttu-id="74969-461">`/etc/syslog-ng/syslog-ng.conf`eller `/etc/rsyslog.conf` eller`/etc/rsyslog.d/95-omsagent.conf`</span><span class="sxs-lookup"><span data-stu-id="74969-461">`/etc/syslog-ng/syslog-ng.conf` or `/etc/rsyslog.conf` or `/etc/rsyslog.d/95-omsagent.conf`</span></span> |
| <span data-ttu-id="74969-462">Prestanda, Nagios, Zabbix, OMS-utdata och allmänna agent</span><span class="sxs-lookup"><span data-stu-id="74969-462">Performance, Nagios, Zabbix, OMS output and general agent</span></span> |`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` |
| <span data-ttu-id="74969-463">Ytterligare konfigurationer</span><span class="sxs-lookup"><span data-stu-id="74969-463">Additional configurations</span></span> |`/etc/opt/microsoft/omsagent/<workspace id>/omsagent.d/*.conf` |

> [!NOTE]
> <span data-ttu-id="74969-464">Redigera konfigurationsfilerna för prestandaräknare och syslog skrivs över om OMS-portalen konfiguration har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="74969-464">Editing configuration files for performance counters and syslog are overwritten if OMS Portal Configuration is enabled.</span></span> <span data-ttu-id="74969-465">Du kan inaktivera konfigurationen i hello OMS-portalen (för alla noder) eller för enskild noder genom att köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="74969-465">You can disable configuration in hello OMS Portal (for all nodes) or for single nodes by running hello following:</span></span>
>
>

```
sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```


### <a name="enable-debug-logging"></a><span data-ttu-id="74969-466">Aktivera felsökningsloggning</span><span class="sxs-lookup"><span data-stu-id="74969-466">Enable debug logging</span></span>
<span data-ttu-id="74969-467">tooenable debug loggning, kan du använda hello OMS utdata plugin-programmet och utförlig utdata.</span><span class="sxs-lookup"><span data-stu-id="74969-467">tooenable debug logging, you can use hello OMS output plugin and verbose output.</span></span>

#### <a name="oms-output-plugin"></a><span data-ttu-id="74969-468">OMS utdata plugin-program</span><span class="sxs-lookup"><span data-stu-id="74969-468">OMS output plugin</span></span>
<span data-ttu-id="74969-469">FluentD kan hello plugin-programmet toospecify loggningsnivåerna för olika loggningsnivåer för in- och utdataenheter.</span><span class="sxs-lookup"><span data-stu-id="74969-469">FluentD allows hello plugin toospecify logging levels for different log levels for inputs and outputs.</span></span> <span data-ttu-id="74969-470">toospecify en annan loggningsnivån för OMS-utdata redigera hello allmänna agentkonfiguration i hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` fil.</span><span class="sxs-lookup"><span data-stu-id="74969-470">toospecify a different log level for OMS output, edit hello general agent configuration in hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` file.</span></span>

<span data-ttu-id="74969-471">Ändra hello hello nedre delen av konfigurationsfilen hello `log_level` egenskap från `info` för`debug`.</span><span class="sxs-lookup"><span data-stu-id="74969-471">Near hello bottom of hello configuration file, change hello `log_level` property from `info` too`debug`.</span></span>

 ```
 <match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
 ```

<span data-ttu-id="74969-472">Felsökningsloggning kan toosee batchar överföringar toohello OMS-tjänsten avgränsade med typen, antalet dataobjekt och tidsåtgång toosend.</span><span class="sxs-lookup"><span data-stu-id="74969-472">Debug logging allows you toosee batched uploads toohello OMS Service separated by type, number of data items, and time taken toosend.</span></span>

<span data-ttu-id="74969-473">*Exempel debug aktiverad logg:*</span><span class="sxs-lookup"><span data-stu-id="74969-473">*Example debug enabled log:*</span></span>

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

#### <a name="verbose-output"></a><span data-ttu-id="74969-474">Utförlig utdata</span><span class="sxs-lookup"><span data-stu-id="74969-474">Verbose output</span></span>
<span data-ttu-id="74969-475">Istället för att använda hello OMS utdata plugin-programmet, du kan också spara dataobjekt direkt för`stdout`, som är synligt i hello OMS-Agent för Linux-loggfil.</span><span class="sxs-lookup"><span data-stu-id="74969-475">Instead of using hello OMS output plugin, you can also output data items directly too`stdout`, which is visible in hello OMS Agent for Linux log file.</span></span>

<span data-ttu-id="74969-476">I hello OMS allmänna agent-konfigurationsfilen vid `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, kommenterar ut hello OMS utdata plugin-programmet genom att lägga till en `#` framför varje rad.</span><span class="sxs-lookup"><span data-stu-id="74969-476">In hello OMS general agent configuration file at `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, comment-out hello OMS output plugin by adding a `#` in front of each line.</span></span>

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

<span data-ttu-id="74969-477">Nedan Hej utdata plugin-programmet, ta bort hello kommentar i hello efter avsnittet genom att ta bort hello `#` symbol hello början av varje rad.</span><span class="sxs-lookup"><span data-stu-id="74969-477">Below hello output plugin, remove hello comment in hello following section by removing hello `#` symbol at hello beginning of each line.</span></span>

```
<match **>
  type stdout
</match>
```

### <a name="forwarded-syslog-messages-do-not-appear-in-hello-log"></a><span data-ttu-id="74969-478">Vidarebefordrade Syslog-meddelanden visas inte i hello logg</span><span class="sxs-lookup"><span data-stu-id="74969-478">Forwarded Syslog messages do not appear in hello log</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="74969-479">Troliga orsaker</span><span class="sxs-lookup"><span data-stu-id="74969-479">Probable causes</span></span>
* <span data-ttu-id="74969-480">hello tillämpas toohello Linux konfigurationsservern inte tillåta samling hello skickas verksamhet och/eller logga nivåer</span><span class="sxs-lookup"><span data-stu-id="74969-480">hello configuration applied toohello Linux server does not allow collection of hello sent facilities and/or log levels</span></span>
* <span data-ttu-id="74969-481">Syslog vidarebefordras inte korrekt toohello Linux-server</span><span class="sxs-lookup"><span data-stu-id="74969-481">Syslog is not being forwarded correctly toohello Linux server</span></span>
* <span data-ttu-id="74969-482">hello antal meddelanden vidarebefordras per sekund är för stora för hello baskonfigurationen för hello OMS-Agent för Linux toohandle</span><span class="sxs-lookup"><span data-stu-id="74969-482">hello number of messages being forwarded per second are too large for hello base configuration of hello OMS Agent for Linux toohandle</span></span>

#### <a name="resolutions"></a><span data-ttu-id="74969-483">Lösningar</span><span class="sxs-lookup"><span data-stu-id="74969-483">Resolutions</span></span>
* <span data-ttu-id="74969-484">Kontrollera att hello-konfigurationen i hello OMS-portalen för Syslog har alla hello hjälpmedel och hello rätt loggningsnivåer</span><span class="sxs-lookup"><span data-stu-id="74969-484">Verify that hello configuration in hello OMS Portal for Syslog has all hello facilities and hello correct log levels</span></span>
  * <span data-ttu-id="74969-485">**OMS-portalen > Inställningar > Data > Syslog**</span><span class="sxs-lookup"><span data-stu-id="74969-485">**OMS Portal > Settings > Data > Syslog**</span></span>
* <span data-ttu-id="74969-486">Kontrollera den interna syslog messaging Daemon (`rsyslog`, `syslog-ng`) är kan tooreceive hello vidarebefordrade meddelanden</span><span class="sxs-lookup"><span data-stu-id="74969-486">Verify that native syslog messaging daemons (`rsyslog`, `syslog-ng`) are able tooreceive hello forwarded messages</span></span>
* <span data-ttu-id="74969-487">Kontrollera brandväggsinställningarna på hello Syslog-servern tooensure att meddelanden inte blockeras</span><span class="sxs-lookup"><span data-stu-id="74969-487">Check firewall settings on hello Syslog server tooensure that messages are not being blocked</span></span>
* <span data-ttu-id="74969-488">Simulera en tooOMS för Syslog-meddelande som använder hello `logger` kommandot - exempel:</span><span class="sxs-lookup"><span data-stu-id="74969-488">Simulate a Syslog message tooOMS using hello `logger` command - for example:</span></span>
  * `logger -p local0.err "This is my test message"`

### <a name="problems-connecting-toooms-when-using-a-proxy"></a><span data-ttu-id="74969-489">Problem med att ansluta tooOMS när du använder en proxyserver</span><span class="sxs-lookup"><span data-stu-id="74969-489">Problems connecting tooOMS when using a proxy</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="74969-490">Troliga orsaker</span><span class="sxs-lookup"><span data-stu-id="74969-490">Probable causes</span></span>
* <span data-ttu-id="74969-491">hello proxy anges när installera och konfigurera hello-agenten är felaktig</span><span class="sxs-lookup"><span data-stu-id="74969-491">hello proxy specified when installing and configuring hello agent is incorrect</span></span>
* <span data-ttu-id="74969-492">hello OMS-tjänstens slutpunkter är inte whitelistested i ditt datacenter</span><span class="sxs-lookup"><span data-stu-id="74969-492">hello OMS Service endpoints are not whitelistested in your datacenter</span></span>

#### <a name="resolutions"></a><span data-ttu-id="74969-493">Lösningar</span><span class="sxs-lookup"><span data-stu-id="74969-493">Resolutions</span></span>
* <span data-ttu-id="74969-494">Installera om hello OMS-Agent för Linux med följande kommando med hello alternativet hello `-v` aktiverat.</span><span class="sxs-lookup"><span data-stu-id="74969-494">Reinstall hello OMS Agent for Linux using hello following command with hello option `-v` enabled.</span></span> <span data-ttu-id="74969-495">Detta gör att utförliga utdata av hello-agenten ansluter via hello proxy toohello OMS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="74969-495">This allows verbose output of hello agent connecting through hello proxy toohello OMS Service.</span></span>
  * `/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`
  * <span data-ttu-id="74969-496">Läs dokumentationen om hello OMS-proxyn på [konfigurera hello agent för användning med en HTTP-proxyserver](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)</span><span class="sxs-lookup"><span data-stu-id="74969-496">Review hello documentation for OMS proxy at [Configuring hello agent for use with an HTTP proxy server](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)</span></span>
* <span data-ttu-id="74969-497">Kontrollera att följande slutpunkter OMS hello är godkända</span><span class="sxs-lookup"><span data-stu-id="74969-497">Verify that hello following OMS Service endpoints are whitelisted</span></span>

| <span data-ttu-id="74969-498">Agentresurs</span><span class="sxs-lookup"><span data-stu-id="74969-498">Agent Resource</span></span> | <span data-ttu-id="74969-499">Portar</span><span class="sxs-lookup"><span data-stu-id="74969-499">Ports</span></span> |
| --- | --- |
| <span data-ttu-id="74969-500">&#42;. ods.opinsights.Azure.com</span><span class="sxs-lookup"><span data-stu-id="74969-500">&#42;.ods.opinsights.azure.com</span></span> |<span data-ttu-id="74969-501">Port 443</span><span class="sxs-lookup"><span data-stu-id="74969-501">Port 443</span></span> |
| <span data-ttu-id="74969-502">&#42;. OMS.opinsights.Azure.com</span><span class="sxs-lookup"><span data-stu-id="74969-502">&#42;.oms.opinsights.azure.com</span></span> |<span data-ttu-id="74969-503">Port 443</span><span class="sxs-lookup"><span data-stu-id="74969-503">Port 443</span></span> |
| <span data-ttu-id="74969-504">ods.systemcenteradvisor.com</span><span class="sxs-lookup"><span data-stu-id="74969-504">ods.systemcenteradvisor.com</span></span> |<span data-ttu-id="74969-505">Port 443</span><span class="sxs-lookup"><span data-stu-id="74969-505">Port 443</span></span> |
| <span data-ttu-id="74969-506">&#42;.blob.core.windows.net/</span><span class="sxs-lookup"><span data-stu-id="74969-506">&#42;.blob.core.windows.net/</span></span> |<span data-ttu-id="74969-507">Port 443</span><span class="sxs-lookup"><span data-stu-id="74969-507">Port 443</span></span> |

### <a name="a-403-error-is-displayed-when-onboarding"></a><span data-ttu-id="74969-508">Ett 403-fel visas när onboarding</span><span class="sxs-lookup"><span data-stu-id="74969-508">A 403 error is displayed when onboarding</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="74969-509">Troliga orsaker</span><span class="sxs-lookup"><span data-stu-id="74969-509">Probable causes</span></span>
* <span data-ttu-id="74969-510">hello datum och tid är felaktiga på Linux-servern</span><span class="sxs-lookup"><span data-stu-id="74969-510">hello date and time are incorrect on Linux Server</span></span>
* <span data-ttu-id="74969-511">hello arbetsyte-ID och Arbetsytenyckel som används är felaktig</span><span class="sxs-lookup"><span data-stu-id="74969-511">hello Workspace ID and Workspace Key used are incorrect</span></span>

#### <a name="resolution"></a><span data-ttu-id="74969-512">Lösning</span><span class="sxs-lookup"><span data-stu-id="74969-512">Resolution</span></span>
* <span data-ttu-id="74969-513">Kontrollera hello tid på Linux-servern med hello `date` kommando.</span><span class="sxs-lookup"><span data-stu-id="74969-513">Verify hello time on your Linux server with hello `date` command.</span></span> <span data-ttu-id="74969-514">Om hello data är större än eller mindre än 15 minuter från hello aktuell tid, misslyckas onboarding.</span><span class="sxs-lookup"><span data-stu-id="74969-514">If hello data is greater than or less than 15 minutes from hello current time, then onboarding fails.</span></span> <span data-ttu-id="74969-515">toocorrect, uppdatera hello datum och/eller tidszonen för Linux-servern.</span><span class="sxs-lookup"><span data-stu-id="74969-515">toocorrect this, update hello date and/or timezone of your Linux server.</span></span>
* <span data-ttu-id="74969-516">hello senaste versionen av hello OMS-Agent för Linux meddelar dig om en tidsskillnad som orsakar fel onboarding</span><span class="sxs-lookup"><span data-stu-id="74969-516">hello latest version of hello OMS Agent for Linux notifies you if a time difference is causing onboarding failure</span></span>
* <span data-ttu-id="74969-517">RE publicera med hjälp av hello rätt arbetsyte-ID och Arbetsytenyckel.</span><span class="sxs-lookup"><span data-stu-id="74969-517">Re-onboard using hello correct Workspace ID and Workspace Key.</span></span> <span data-ttu-id="74969-518">Se [Onboarding hello kommandoraden](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) för mer information.</span><span class="sxs-lookup"><span data-stu-id="74969-518">See  [Onboarding using hello command line](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) for more information.</span></span>

### <a name="a-500-error-or-404-error-appears-in-hello-log-file-after-onboarding"></a><span data-ttu-id="74969-519">Ett fel 500 eller 404-fel visas i loggfilen för hello efter onboarding</span><span class="sxs-lookup"><span data-stu-id="74969-519">A 500 error or 404 error appears in hello log file after onboarding</span></span>
<span data-ttu-id="74969-520">Detta är ett känt problem som uppstår under hello första uppladdning av Linux-data i en OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="74969-520">This is a known issue that occurs during hello first upload of Linux data into an OMS workspace.</span></span> <span data-ttu-id="74969-521">Detta påverkar inte data som skickas eller andra problem.</span><span class="sxs-lookup"><span data-stu-id="74969-521">This does not affect data being sent or other problems.</span></span> <span data-ttu-id="74969-522">Du kan ignorera hello-fel när ursprungligen onboarding.</span><span class="sxs-lookup"><span data-stu-id="74969-522">You can ignore hello errors when initially onboarding.</span></span>

### <a name="nagios-data-does-not-appear-in-hello-oms-portal"></a><span data-ttu-id="74969-523">Nagios data visas inte i hello OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="74969-523">Nagios data does not appear in hello OMS Portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="74969-524">Troliga orsaker</span><span class="sxs-lookup"><span data-stu-id="74969-524">Probable causes</span></span>
* <span data-ttu-id="74969-525">Hej omsagent användaren har inte behörighet tooread från hello Nagios loggfil</span><span class="sxs-lookup"><span data-stu-id="74969-525">hello omsagent user does not have permissions tooread from hello Nagios log file</span></span>
* <span data-ttu-id="74969-526">Hej Nagios källa och filter avsnitt fortfarande kommenterade i hello omsagent.conf fil</span><span class="sxs-lookup"><span data-stu-id="74969-526">hello Nagios source and filter sections are still commented in hello omsagent.conf file</span></span>

#### <a name="resolutions"></a><span data-ttu-id="74969-527">Lösningar</span><span class="sxs-lookup"><span data-stu-id="74969-527">Resolutions</span></span>
* <span data-ttu-id="74969-528">Lägga till hello omsagent användare i ordning tooread från hello Nagios-fil.</span><span class="sxs-lookup"><span data-stu-id="74969-528">Add hello omsagent user in order tooread from hello Nagios file.</span></span> <span data-ttu-id="74969-529">Se [Nagios aviseringar](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) för mer information.</span><span class="sxs-lookup"><span data-stu-id="74969-529">See [Nagios alerts](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) for more information.</span></span>
* <span data-ttu-id="74969-530">I hello OMS-Agent för Linux-Allmänt konfigurationsfilen vid `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, se till att **både** hello Nagios käll- och filter avsnitt har kommentarer bort, liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="74969-530">In hello OMS Agent for Linux general configuration file at `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, ensure that **both** hello Nagios source and filter sections have comments removed, similar toohello following example.</span></span>

```
<source>
  type tail
  path /var/log/nagios/nagios.log
  format none
  tag oms.nagios
</source>

<filter oms.nagios>
  type filter_nagios_log
</filter>
```


### <a name="linux-data-doesnt-appear-in-hello-oms-portal"></a><span data-ttu-id="74969-531">Linux-data visas inte i hello OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="74969-531">Linux data doesn't appear in hello OMS Portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="74969-532">Troliga orsaker</span><span class="sxs-lookup"><span data-stu-id="74969-532">Probable causes</span></span>
* <span data-ttu-id="74969-533">Onboarding toohello OMS-tjänsten misslyckades</span><span class="sxs-lookup"><span data-stu-id="74969-533">Onboarding toohello OMS Service failed</span></span>
* <span data-ttu-id="74969-534">Anslutningen toohello OMS-tjänsten är blockerad</span><span class="sxs-lookup"><span data-stu-id="74969-534">Connection toohello OMS Service is blocked</span></span>
* <span data-ttu-id="74969-535">hello OMS-Agent för Linux-data är säkerhetskopierade</span><span class="sxs-lookup"><span data-stu-id="74969-535">hello OMS Agent for Linux data is backed-up</span></span>

#### <a name="resolutions"></a><span data-ttu-id="74969-536">Lösningar</span><span class="sxs-lookup"><span data-stu-id="74969-536">Resolutions</span></span>
* <span data-ttu-id="74969-537">Kontrollera att onboarding toohello OMS-tjänsten lyckades genom att verifiera att hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` finns.</span><span class="sxs-lookup"><span data-stu-id="74969-537">Verify that onboarding toohello OMS Service was successful by verifying that hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` exists.</span></span>
* <span data-ttu-id="74969-538">RE publicera med hjälp av hello omsadmin.sh kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="74969-538">Re-onboard using hello omsadmin.sh command line.</span></span> <span data-ttu-id="74969-539">Se [Onboarding hello kommandoraden](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) för mer information.</span><span class="sxs-lookup"><span data-stu-id="74969-539">See [Onboarding using hello command line](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) for more information.</span></span>
* <span data-ttu-id="74969-540">Om du använder en proxyserver Använd hello proxy felsökning stegen ovan</span><span class="sxs-lookup"><span data-stu-id="74969-540">If using a proxy, use hello proxy troubleshooting steps above</span></span>
* <span data-ttu-id="74969-541">I vissa fall när hello OMS-Agent för Linux inte kan kommunicera med hello OMS-tjänsten är data på hello Agent säkerhetskopierade toohello fullständig buffertstorlek på 50 MB.</span><span class="sxs-lookup"><span data-stu-id="74969-541">In some cases, when hello OMS Agent for Linux cannot communicate with hello OMS Service, data on hello Agent is backed-up toohello full buffer size of 50 MB.</span></span> <span data-ttu-id="74969-542">Starta om hello OMS-Agent för Linux genom att köra hello `/opt/microsoft/omsagent/bin/service_control restart` kommando.</span><span class="sxs-lookup"><span data-stu-id="74969-542">Restart hello OMS Agent for Linux by running hello `/opt/microsoft/omsagent/bin/service_control restart` command.</span></span>
  >[AZURE.NOTE] <span data-ttu-id="74969-543">Det här problemet har lösts i agenten version 1.1.0-28 och senare.</span><span class="sxs-lookup"><span data-stu-id="74969-543">This issue is fixed in Agent version 1.1.0-28 and later.</span></span>

### <a name="syslog-linux-performance-counter-configuration-is-not-applied-in-hello-oms-portal"></a><span data-ttu-id="74969-544">Syslog Linux prestandaräknaren konfigurationen tillämpas inte i hello OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="74969-544">Syslog Linux performance counter configuration is not applied in hello OMS portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="74969-545">Troliga orsaker</span><span class="sxs-lookup"><span data-stu-id="74969-545">Probable causes</span></span>
* <span data-ttu-id="74969-546">hello OMS-Agent för Linux-hello configuration agenten har inte hämta hello senaste konfigurationen från hello OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="74969-546">hello configuration agent in hello OMS Agent for Linux has not retrieved hello latest configuration from hello OMS portal.</span></span>
* <span data-ttu-id="74969-547">hello har ändrade inställningar i hello portal inte tillämpats</span><span class="sxs-lookup"><span data-stu-id="74969-547">hello revised settings in hello portal were not applied</span></span>

#### <a name="resolutions"></a><span data-ttu-id="74969-548">Lösningar</span><span class="sxs-lookup"><span data-stu-id="74969-548">Resolutions</span></span>
<span data-ttu-id="74969-549">`omsconfig`är hello configuration agenten i hello OMS-Agent för Linux som hämtar OMS-portalen konfigurationsändringar var femte minut.</span><span class="sxs-lookup"><span data-stu-id="74969-549">`omsconfig` is hello configuration agent in hello OMS Agent for Linux that retrieves OMS portal configuration changes every 5 minutes.</span></span> <span data-ttu-id="74969-550">Den här konfigurationen är tillämpade toohello OMS-Agent för Linux-konfigurationsfiler finns på `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.</span><span class="sxs-lookup"><span data-stu-id="74969-550">This configuration is then applied toohello OMS Agent for Linux configuration files located at `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.</span></span>

* <span data-ttu-id="74969-551">I vissa fall kanske inte hello OMS-Agent för Linux configuration-agenten kan toocommunicate med hello konfiguration av tjänsten, vilket resulterar i senaste konfigurationen används inte.</span><span class="sxs-lookup"><span data-stu-id="74969-551">In some cases, hello OMS Agent for Linux configuration agent might not be able toocommunicate with hello portal configuration service resulting in latest configuration not being applied.</span></span>
* <span data-ttu-id="74969-552">Kontrollera att hello `omsconfig` agent installeras med hello följande:</span><span class="sxs-lookup"><span data-stu-id="74969-552">Verify that hello `omsconfig` agent is installed with hello following:</span></span>

  * <span data-ttu-id="74969-553">`dpkg --list omsconfig` eller `rpm -qi omsconfig`</span><span class="sxs-lookup"><span data-stu-id="74969-553">`dpkg --list omsconfig` or `rpm -qi omsconfig`</span></span>
  * <span data-ttu-id="74969-554">Om du inte har installerats installerar du om hello senaste versionen av hello OMS-Agent för Linux</span><span class="sxs-lookup"><span data-stu-id="74969-554">If not installed, reinstall hello latest version of hello OMS Agent for Linux</span></span>
* <span data-ttu-id="74969-555">Kontrollera att hello `omsconfig` agenten kan kommunicera med hello OMS-tjänsten</span><span class="sxs-lookup"><span data-stu-id="74969-555">Verify that hello `omsconfig` agent can communicate with hello OMS service</span></span>

  * <span data-ttu-id="74969-556">Kör hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` kommando</span><span class="sxs-lookup"><span data-stu-id="74969-556">Run hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` command</span></span>
    * <span data-ttu-id="74969-557">hello ovanstående kommando returnerar hello configuration agentens hämtar från hello portal, inklusive inställningar för Syslog, prestandaräknare för Linux och anpassade loggar</span><span class="sxs-lookup"><span data-stu-id="74969-557">hello command above returns hello configuration that agent retrieves from hello portal, including Syslog settings, Linux performance counters, and custom logs</span></span>
    * <span data-ttu-id="74969-558">Om hello kommandot ovan misslyckas, kör hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` kommando.</span><span class="sxs-lookup"><span data-stu-id="74969-558">If hello command above fails, run hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` command.</span></span> <span data-ttu-id="74969-559">Det här kommandot tvingar hello omsconfig agent toocommunicate med hello OMS tooretrieve hello senaste tjänstkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="74969-559">This command forces hello omsconfig agent toocommunicate with hello OMS service tooretrieve hello latest configuration.</span></span>

### <a name="custom-linux-log-data-does-not-appear-in-hello-oms-portal"></a><span data-ttu-id="74969-560">Anpassade loggdata för Linux visas inte i hello OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="74969-560">Custom Linux log data does not appear in hello OMS Portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="74969-561">Troliga orsaker</span><span class="sxs-lookup"><span data-stu-id="74969-561">Probable causes</span></span>
* <span data-ttu-id="74969-562">Onboarding tooOMS tjänsten misslyckades</span><span class="sxs-lookup"><span data-stu-id="74969-562">Onboarding tooOMS Service failed</span></span>
* <span data-ttu-id="74969-563">Hej **tillämpa hello följande konfiguration toomy Linux-servrar** inställningen har valts</span><span class="sxs-lookup"><span data-stu-id="74969-563">hello **Apply hello following configuration toomy Linux Servers** setting has not been selected</span></span>
* <span data-ttu-id="74969-564">omsconfig har inte plockats upp hello senaste anpassad logg från hello-portalen</span><span class="sxs-lookup"><span data-stu-id="74969-564">omsconfig has not picked up hello latest custom log from hello portal</span></span>
* <span data-ttu-id="74969-565">Hej `omsagent` används tooaccess hello anpassad logg på grund av tooa behörighetsproblem eller `omsagent` hittades inte.</span><span class="sxs-lookup"><span data-stu-id="74969-565">hello `omsagent` use is unable tooaccess hello custom log due tooa permissions problem or `omsagent` was not found.</span></span> <span data-ttu-id="74969-566">I det här fallet visas hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="74969-566">In this case, you'll see hello following output:</span></span>
  * `[DATETIME] [warn]: file not found. Continuing without tailing it.`
  * `[DATETIME] [error]: file not accessible by omsagent.`
* <span data-ttu-id="74969-567">Detta är ett känt problem med hello konkurrenstillstånd som fastställdes i hello OMS-Agent för Linux-version 1.1.0-217</span><span class="sxs-lookup"><span data-stu-id="74969-567">This is a known issue with hello Race Condition that was fixed in hello OMS Agent for Linux version 1.1.0-217</span></span>

#### <a name="resolutions"></a><span data-ttu-id="74969-568">Lösningar</span><span class="sxs-lookup"><span data-stu-id="74969-568">Resolutions</span></span>
* <span data-ttu-id="74969-569">Kontrollera att du nu har publicerats, eller så genom att fastställa om hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` filen finns.</span><span class="sxs-lookup"><span data-stu-id="74969-569">Verify that you've successfully onboarded, by determining whether hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` file exists.</span></span>
  * <span data-ttu-id="74969-570">Om nödvändiga, publicera igen kommandoraden hello omsadmin.sh.</span><span class="sxs-lookup"><span data-stu-id="74969-570">If needed, onboard again using hello omsadmin.sh command line.</span></span> <span data-ttu-id="74969-571">Se [Onboarding hello kommandoraden](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) för mer information.</span><span class="sxs-lookup"><span data-stu-id="74969-571">See [Onboarding using hello command line](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) for more information.</span></span>
* <span data-ttu-id="74969-572">I hello OMS-portalen under **inställningar** på hello **Data** kontrollerar att hello **tillämpa hello följande konfiguration toomy Linux-servrar** är vald</span><span class="sxs-lookup"><span data-stu-id="74969-572">In hello OMS Portal, under **Settings** on hello **Data** tab, ensure that hello **Apply hello following configuration toomy Linux Servers** setting is selected</span></span>  
  <span data-ttu-id="74969-573">![Använd konfiguration](./media/log-analytics-linux-agents/customloglinuxenabled.png)</span><span class="sxs-lookup"><span data-stu-id="74969-573">![apply configuration](./media/log-analytics-linux-agents/customloglinuxenabled.png)</span></span>
* <span data-ttu-id="74969-574">Kontrollera att hello `omsconfig` agenten kan kommunicera med hello OMS-tjänsten</span><span class="sxs-lookup"><span data-stu-id="74969-574">Verify that hello `omsconfig` agent can communicate with hello OMS service</span></span>

  * <span data-ttu-id="74969-575">Kör hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` kommando</span><span class="sxs-lookup"><span data-stu-id="74969-575">Run hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` command</span></span>
  * <span data-ttu-id="74969-576">hello ovanstående kommando returnerar hello configuration agentens hämtar från hello Portal, inklusive inställningar för Syslog, prestandaräknare för Linux och anpassade loggar</span><span class="sxs-lookup"><span data-stu-id="74969-576">hello command above returns hello configuration that agent retrieves from hello Portal, including Syslog settings, Linux performance counters, and custom Logs</span></span>
  * <span data-ttu-id="74969-577">Om hello kommandot ovan misslyckas, kör hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` kommando.</span><span class="sxs-lookup"><span data-stu-id="74969-577">If hello command above fails, run hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` command.</span></span> <span data-ttu-id="74969-578">Det här kommandot tvingar hello omsconfig agent toocommunicate med OMS-tjänsten och hämta senaste hello-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="74969-578">This command forces hello omsconfig agent toocommunicate with OMS service and retrieve hello latest configuration.</span></span>

<span data-ttu-id="74969-579">I stället för hello OMS-Agent för Linux-användare som körs som en privilegierad användare `root`, hello OMS-Agent för Linux körs som hello `omsagent` användare.</span><span class="sxs-lookup"><span data-stu-id="74969-579">Instead of hello OMS Agent for Linux user running as a privileged user `root`, hello OMS Agent for Linux runs as hello `omsagent` user.</span></span> <span data-ttu-id="74969-580">I de flesta fall måste vara explicit behörighet beviljas toohello användare i ordning tooread vissa filer.</span><span class="sxs-lookup"><span data-stu-id="74969-580">In most cases, explicit permission must be granted toohello user in order tooread certain files.</span></span>

<span data-ttu-id="74969-581">toogrant behörighet för`omsagent` användaren som kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="74969-581">toogrant permission too`omsagent` user, run hello following commands:</span></span>

1. <span data-ttu-id="74969-582">Lägg till hello `omsagent` tooa specifik användargrupp med`sudo usermod -a -G <GROUPNAME> <USERNAME>`</span><span class="sxs-lookup"><span data-stu-id="74969-582">Add hello `omsagent` user tooa specific group with `sudo usermod -a -G <GROUPNAME> <USERNAME>`</span></span>
2. <span data-ttu-id="74969-583">Bevilja läsbehörighet universal toohello krävs fil med`sudo chmod -R ugo+rw <FILE DIRECTORY>`</span><span class="sxs-lookup"><span data-stu-id="74969-583">Grant universal read access toohello required file with `sudo chmod -R ugo+rw <FILE DIRECTORY>`</span></span>

<span data-ttu-id="74969-584">Det finns ett känt problem med hello konkurrenstillstånd som fastställdes i hello OMS-Agent för Linux-version 1.1.0-217.</span><span class="sxs-lookup"><span data-stu-id="74969-584">There is a known issue with hello Race Condition that was fixed in hello OMS Agent for Linux version 1.1.0-217.</span></span> <span data-ttu-id="74969-585">Kör följande kommando tooget hello senaste versionen av utdata-plugin-programmet hello hello när du har uppdaterat toohello senaste agenten:</span><span class="sxs-lookup"><span data-stu-id="74969-585">After updating toohello latest agent, run hello following command tooget hello latest version of hello output plugin:</span></span>

```
sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf
```

## <a name="known-limitations"></a><span data-ttu-id="74969-586">Kända begränsningar</span><span class="sxs-lookup"><span data-stu-id="74969-586">Known limitations</span></span>
<span data-ttu-id="74969-587">Granska följande avsnitt toolearn om aktuella begränsningar av hello OMS-Agent för Linux hello.</span><span class="sxs-lookup"><span data-stu-id="74969-587">Review hello following sections toolearn about current limitations of hello OMS Agent for Linux.</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="74969-588">Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="74969-588">Azure Diagnostics</span></span>
<span data-ttu-id="74969-589">För Linux virtuella datorer som körs i Azure, kan ytterligare steg vara nödvändiga tooallow insamling av Azure-diagnostik och Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="74969-589">For Linux virtual machines running in Azure, additional steps may be required tooallow data collection by Azure Diagnostics and Operations Management Suite.</span></span> <span data-ttu-id="74969-590">**Version 2.2** av hello diagnostik tillägget för Linux krävs för kompatibilitet med hello OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="74969-590">**Version 2.2** of hello Diagnostics Extension for Linux is required for compatibility with hello OMS Agent for Linux.</span></span>

<span data-ttu-id="74969-591">Mer information om installation och konfiguration hello diagnostiska tillägget för Linux finns [använda hello Azure CLI kommandot tooenable Linux diagnostiska tillägget](../virtual-machines/linux/classic/diagnostic-extension-v2.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).</span><span class="sxs-lookup"><span data-stu-id="74969-591">For more information on installing and configuring hello Diagnostic Extension for Linux, see [Use hello Azure CLI command tooenable Linux Diagnostic Extension](../virtual-machines/linux/classic/diagnostic-extension-v2.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).</span></span>

<span data-ttu-id="74969-592">**Uppgradera hello Linux diagnostik tillägg från 2.0 too2.2 Azure CLI ASM:**</span><span class="sxs-lookup"><span data-stu-id="74969-592">**Upgrading hello Linux Diagnostics Extension from 2.0 too2.2 Azure CLI ASM:**</span></span>

```
azure vm extension set -u <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.0
azure vm extension set <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

<span data-ttu-id="74969-593">**ARM**</span><span class="sxs-lookup"><span data-stu-id="74969-593">**ARM**</span></span>

```
azure vm extension set -u <resource-group> <vm-name> Microsoft.Insights.VMDiagnosticsSettings Microsoft.OSTCExtensions 2.0
azure vm extension set <resource-group> <vm-name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

<span data-ttu-id="74969-594">Dessa kommandoexempel referera till en fil med namnet PrivateConfig.json.</span><span class="sxs-lookup"><span data-stu-id="74969-594">These command examples reference a file named PrivateConfig.json.</span></span> <span data-ttu-id="74969-595">hello-formatet för filen bör likna följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="74969-595">hello format of that file should resemble hello following sample.</span></span>

```
    {
    "storageAccountName":"hello storage account tooreceive data",
    "storageAccountKey":"hello key of hello account"
    }
```

### <a name="sysklog-is-not-supported"></a><span data-ttu-id="74969-596">Sysklog stöds inte</span><span class="sxs-lookup"><span data-stu-id="74969-596">Sysklog is not supported</span></span>
<span data-ttu-id="74969-597">Rsyslog eller syslog-ng är obligatoriska toocollect syslog-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="74969-597">Either rsyslog or syslog-ng are required toocollect syslog messages.</span></span> <span data-ttu-id="74969-598">hello standard syslog-daemon på version 5 Red Hat Enterprise Linux, CentOS och Oracle Linux-version (sysklog) stöds inte för insamling av syslog-händelser.</span><span class="sxs-lookup"><span data-stu-id="74969-598">hello default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="74969-599">toocollect syslog-data från den här versionen av dessa distributioner hello rsyslog daemon ska vara installerat och konfigurerat tooreplace sysklog.</span><span class="sxs-lookup"><span data-stu-id="74969-599">toocollect syslog data from this version of these distributions, hello rsyslog daemon should be installed and configured tooreplace sysklog.</span></span> <span data-ttu-id="74969-600">Läs mer om att ersätta sysklog med rsyslog [installera hello som nyligen skapats rsyslog RPM](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).</span><span class="sxs-lookup"><span data-stu-id="74969-600">For more information on replacing sysklog with rsyslog, see [Install hello newly built rsyslog RPM](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).</span></span>

## <a name="next-steps"></a><span data-ttu-id="74969-601">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="74969-601">Next Steps</span></span>
* <span data-ttu-id="74969-602">[Lägg till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md) tooadd funktioner och samla data.</span><span class="sxs-lookup"><span data-stu-id="74969-602">[Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md) tooadd functionality and gather data.</span></span>
* <span data-ttu-id="74969-603">Bekanta dig med [logga sökningar](log-analytics-log-searches.md) tooview detaljerad information som samlas in av lösningar.</span><span class="sxs-lookup"><span data-stu-id="74969-603">Get familiar with [log searches](log-analytics-log-searches.md) tooview detailed information gathered by solutions.</span></span>
* <span data-ttu-id="74969-604">Använd [instrumentpaneler](log-analytics-dashboards.md) toosave och visa dina egna anpassade söker.</span><span class="sxs-lookup"><span data-stu-id="74969-604">Use [dashboards](log-analytics-dashboards.md) toosave and display your own custom searches.</span></span>
