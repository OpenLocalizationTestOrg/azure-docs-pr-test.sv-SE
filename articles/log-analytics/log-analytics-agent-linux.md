---
title: aaaConnect ditt Linux-datorer tooOperations Management Suite (OMS) | Microsoft Docs
description: "Den här artikeln beskriver hur tooconnect Linux-datorer finns i Azure andra molntjänster eller lokala tooOMS med hello OMS-Agent för Linux."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: magoedte
ms.openlocfilehash: cb4fc671d0678f9fadc689c6ba7d719213aa61b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-linux-computers-toooperations-management-suite-oms"></a><span data-ttu-id="259f4-103">Ansluta din Linux-datorer tooOperations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="259f4-103">Connect your Linux Computers tooOperations Management Suite (OMS)</span></span> 

<span data-ttu-id="259f4-104">Med Microsoft Operations Management Suite (OMS), som du kan samla in och arbeta med data som genereras från Linux-datorer och lösningar för behållare som Docker, som finns i ditt lokala datacenter som fysiska servrar eller virtuella datorer, virtuella datorer i en molnbaserad tjänst som Amazon Web Services (AWS) eller Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="259f4-104">With Microsoft Operations Management Suite (OMS), you can collect and act on data generated from Linux computers and container solutions like Docker, residing in your on-premises data center as physical servers or virtual machines, virtual machines in a cloud-hosted service like Amazon Web Services (AWS) or Microsoft Azure.</span></span> <span data-ttu-id="259f4-105">Du kan också använda hanteringslösningar som är tillgängliga i OMS, till exempel ändringsspårning, tooidentify konfigurationsändringar och uppdateringshantering toomanage programvara uppdateringar tooproactively hantera hello livscykeln för din virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="259f4-105">You can also use management solutions available in OMS such as Change Tracking, tooidentify configuration changes, and Update Management toomanage software updates tooproactively manage hello lifecycle of your Linux VMs.</span></span> 

<span data-ttu-id="259f4-106">hello OMS-Agent för Linux kommunicerar utgående med hello OMS-tjänsten via TCP-port 443, och om hello datorn ansluter tooa brandvägg eller proxyserver server toocommunicate via hello Internet, granska [konfigurera hello agent för användning med en HTTP-proxy servern eller Gateway OMS](#configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway) toounderstand vilka konfigurationsändringar behöver toobe tillämpas.</span><span class="sxs-lookup"><span data-stu-id="259f4-106">hello OMS Agent for Linux communicates outbound with hello OMS service over TCP port 443, and if hello computer connects tooa firewall or proxy server toocommunicate over hello Internet, review [Configuring hello agent for use with an HTTP proxy server or OMS Gateway](#configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway) toounderstand what configuration changes will need toobe applied.</span></span>  <span data-ttu-id="259f4-107">Om du övervakar hello-dator med System Center 2016 - Operations Manager eller Operations Manager 2012 R2 kan vara multi-homed med hello OMS-tjänsten toocollect data och vidarebefordra toohello service och fortfarande övervakas av Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="259f4-107">If you are monitoring hello computer with System Center 2016 - Operations Manager or Operations Manager 2012 R2, it can be multi-homed with hello OMS service toocollect data and forward toohello service and still be monitored by Operations Manager.</span></span>  <span data-ttu-id="259f4-108">Linux-datorer som övervakas av en Operations Manager-hanteringsgrupp som är integrerad med OMS får inte konfigurationen för datakällor och vidarebefordra insamlade data via hello hanteringsgruppen.</span><span class="sxs-lookup"><span data-stu-id="259f4-108">Linux computers monitored by an Operations Manager management group that is integrated with OMS do not receive configuration for data sources and forward collected data through hello management group.</span></span>  <span data-ttu-id="259f4-109">hello OMS-agenten kan inte vara konfigurerade tooreport toomore än en arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="259f4-109">hello OMS agent cannot be configured tooreport toomore than one workspace.</span></span>  

<span data-ttu-id="259f4-110">Om din IT-säkerhetsprinciper inte tillåter datorer på ditt nätverk tooconnect toohello Internet, kan hello agent vara konfigurerade tooconnect toohello OMS Gateway tooreceive konfigurationsinformation och skicka insamlade data beroende på hello-lösning som du har aktiverad.</span><span class="sxs-lookup"><span data-stu-id="259f4-110">If your IT security policies do not allow computers on your network tooconnect toohello Internet, hello agent can be configured tooconnect toohello OMS Gateway tooreceive configuration information and send collected data depending on hello solution you have enabled.</span></span> <span data-ttu-id="259f4-111">Mer information och anvisningar om hur tooconfigure din OMS Linux-agenten toocommunicate via en OMS-Gateway toohello OMS-tjänst, se [ansluta datorer tooOMS med hello OMS Gateway](log-analytics-oms-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="259f4-111">For more information and steps on how tooconfigure your OMS Linux Agent toocommunicate through an OMS Gateway toohello OMS service, see [Connect computers tooOMS using hello OMS Gateway](log-analytics-oms-gateway.md).</span></span>  

<span data-ttu-id="259f4-112">hello visar följande diagram hello anslutningen mellan hello hanteras med agent Linux-datorer och OMS, inklusive hello riktning och portar.</span><span class="sxs-lookup"><span data-stu-id="259f4-112">hello following diagram depicts hello connection between hello agent-managed Linux computers and OMS, including hello direction and ports.</span></span>

![direkt agentkommunikation med OMS-diagram](./media/log-analytics-agent-linux/log-analytics-agent-linux-communication.png)

## <a name="system-requirements"></a><span data-ttu-id="259f4-114">Systemkrav</span><span class="sxs-lookup"><span data-stu-id="259f4-114">System requirements</span></span>
<span data-ttu-id="259f4-115">Granska hello följande information tooverify hello förutsättningar vara uppfyllda innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="259f4-115">Before starting, review hello following details tooverify you meet hello prerequisites.</span></span>

### <a name="supported-linux-operating-systems"></a><span data-ttu-id="259f4-116">Linux-operativsystem som stöds</span><span class="sxs-lookup"><span data-stu-id="259f4-116">Supported Linux operating systems</span></span>
<span data-ttu-id="259f4-117">hello följande Linux-distributioner stöds officiellt.</span><span class="sxs-lookup"><span data-stu-id="259f4-117">hello following Linux distributions are officially supported.</span></span>  <span data-ttu-id="259f4-118">Hello OMS-Agent för Linux kan också köra på andra distributioner som inte finns.</span><span class="sxs-lookup"><span data-stu-id="259f4-118">However, hello OMS Agent for Linux might also run on other distributions not listed.</span></span>

* <span data-ttu-id="259f4-119">Amazon Linux 2012.09 too2015.09 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="259f4-119">Amazon Linux 2012.09 too2015.09 (x86/x64)</span></span>
* <span data-ttu-id="259f4-120">CentOS Linux 5, 6 och 7 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="259f4-120">CentOS Linux 5, 6, and 7 (x86/x64)</span></span>
* <span data-ttu-id="259f4-121">Oracle Linux 5, 6 och 7 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="259f4-121">Oracle Linux 5, 6, and 7 (x86/x64)</span></span>
* <span data-ttu-id="259f4-122">Red Hat Enterprise Linux Server 5, 6 och 7 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="259f4-122">Red Hat Enterprise Linux Server 5, 6 and 7 (x86/x64)</span></span>
* <span data-ttu-id="259f4-123">Debian GNU/Linux 6, 7 och 8 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="259f4-123">Debian GNU/Linux 6, 7, and 8 (x86/x64)</span></span>
* <span data-ttu-id="259f4-124">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="259f4-124">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)</span></span>
* <span data-ttu-id="259f4-125">SUSE Linux Enterprise Server 11 och 12 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="259f4-125">SUSE Linux Enterprise Server 11 and 12 (x86/x64)</span></span>

### <a name="network"></a><span data-ttu-id="259f4-126">Nätverk</span><span class="sxs-lookup"><span data-stu-id="259f4-126">Network</span></span>
<span data-ttu-id="259f4-127">hello informationen under listan hello proxy och brandväggen konfigurationsinformation som krävs för hello Linux-agenten toocommunicate med OMS.</span><span class="sxs-lookup"><span data-stu-id="259f4-127">hello information below list hello proxy and firewall configuration information required for hello Linux agent toocommunicate with OMS.</span></span> <span data-ttu-id="259f4-128">Trafiken är utgående från din nätverkstjänst toohello OMS.</span><span class="sxs-lookup"><span data-stu-id="259f4-128">Traffic is outbound from your network toohello OMS service.</span></span> 

|<span data-ttu-id="259f4-129">Agentresurs</span><span class="sxs-lookup"><span data-stu-id="259f4-129">Agent Resource</span></span>| <span data-ttu-id="259f4-130">Portar</span><span class="sxs-lookup"><span data-stu-id="259f4-130">Ports</span></span> |  
|------|---------|  
|<span data-ttu-id="259f4-131">*.ods.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="259f4-131">*.ods.opinsights.azure.com</span></span> | <span data-ttu-id="259f4-132">Port 443</span><span class="sxs-lookup"><span data-stu-id="259f4-132">Port 443</span></span>|   
|<span data-ttu-id="259f4-133">*.oms.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="259f4-133">*.oms.opinsights.azure.com</span></span> | <span data-ttu-id="259f4-134">Port 443</span><span class="sxs-lookup"><span data-stu-id="259f4-134">Port 443</span></span>|   
|<span data-ttu-id="259f4-135">*.BLOB.Core.Windows.NET/</span><span class="sxs-lookup"><span data-stu-id="259f4-135">*.blob.core.windows.net/</span></span> | <span data-ttu-id="259f4-136">Port 443</span><span class="sxs-lookup"><span data-stu-id="259f4-136">Port 443</span></span>|   
|<span data-ttu-id="259f4-137">*.azure-automation.net</span><span class="sxs-lookup"><span data-stu-id="259f4-137">*.azure-automation.net</span></span> | <span data-ttu-id="259f4-138">Port 443</span><span class="sxs-lookup"><span data-stu-id="259f4-138">Port 443</span></span>|  

### <a name="package-requirements"></a><span data-ttu-id="259f4-139">Krav för paketet</span><span class="sxs-lookup"><span data-stu-id="259f4-139">Package requirements</span></span>

 <span data-ttu-id="259f4-140">**Nödvändigt paket**</span><span class="sxs-lookup"><span data-stu-id="259f4-140">**Required package**</span></span>   | <span data-ttu-id="259f4-141">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="259f4-141">**Description**</span></span>   | <span data-ttu-id="259f4-142">**Lägsta version**</span><span class="sxs-lookup"><span data-stu-id="259f4-142">**Minimum version**</span></span>
--------------------- | --------------------- | -------------------
<span data-ttu-id="259f4-143">Glibc</span><span class="sxs-lookup"><span data-stu-id="259f4-143">Glibc</span></span> | <span data-ttu-id="259f4-144">GNU C-bibliotek</span><span class="sxs-lookup"><span data-stu-id="259f4-144">GNU C Library</span></span>   | <span data-ttu-id="259f4-145">2.5-12</span><span class="sxs-lookup"><span data-stu-id="259f4-145">2.5-12</span></span> 
<span data-ttu-id="259f4-146">Openssl</span><span class="sxs-lookup"><span data-stu-id="259f4-146">Openssl</span></span> | <span data-ttu-id="259f4-147">OpenSSL-bibliotek</span><span class="sxs-lookup"><span data-stu-id="259f4-147">OpenSSL Libraries</span></span> | <span data-ttu-id="259f4-148">0.9.8e eller 1.0</span><span class="sxs-lookup"><span data-stu-id="259f4-148">0.9.8e or 1.0</span></span>
<span data-ttu-id="259f4-149">CURL</span><span class="sxs-lookup"><span data-stu-id="259f4-149">Curl</span></span> | <span data-ttu-id="259f4-150">cURL Webbklient</span><span class="sxs-lookup"><span data-stu-id="259f4-150">cURL web client</span></span> | <span data-ttu-id="259f4-151">7.15.5</span><span class="sxs-lookup"><span data-stu-id="259f4-151">7.15.5</span></span>
<span data-ttu-id="259f4-152">Python-ctypes</span><span class="sxs-lookup"><span data-stu-id="259f4-152">Python-ctypes</span></span> | | 
<span data-ttu-id="259f4-153">PAM</span><span class="sxs-lookup"><span data-stu-id="259f4-153">PAM</span></span> | <span data-ttu-id="259f4-154">Lösenordssynkronisering moduler</span><span class="sxs-lookup"><span data-stu-id="259f4-154">Pluggable authentication Modules</span></span> | 

> [!NOTE]
>  <span data-ttu-id="259f4-155">Rsyslog eller syslog-ng är obligatoriska toocollect syslog-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="259f4-155">Either rsyslog or syslog-ng are required toocollect syslog messages.</span></span> <span data-ttu-id="259f4-156">hello standard syslog-daemon på version 5 Red Hat Enterprise Linux, CentOS och Oracle Linux-version (sysklog) stöds inte för insamling av syslog-händelser.</span><span class="sxs-lookup"><span data-stu-id="259f4-156">hello default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="259f4-157">toocollect syslog-data från den här versionen av dessa distributioner hello rsyslog daemon ska vara installerat och konfigurerat tooreplace sysklog</span><span class="sxs-lookup"><span data-stu-id="259f4-157">toocollect syslog data from this version of these distributions, hello rsyslog daemon should be installed and configured tooreplace sysklog,</span></span> 

<span data-ttu-id="259f4-158">hello agent innehåller flera paket.</span><span class="sxs-lookup"><span data-stu-id="259f4-158">hello agent includes multiple packages.</span></span> <span data-ttu-id="259f4-159">hello versionen filen innehåller hello följande paket, som körs hello shell-paket med `--extract`:</span><span class="sxs-lookup"><span data-stu-id="259f4-159">hello release file contains hello following packages, available by running hello shell bundle with `--extract`:</span></span>

<span data-ttu-id="259f4-160">**Paketet**</span><span class="sxs-lookup"><span data-stu-id="259f4-160">**Package**</span></span> | <span data-ttu-id="259f4-161">**Version**</span><span class="sxs-lookup"><span data-stu-id="259f4-161">**Version**</span></span> | <span data-ttu-id="259f4-162">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="259f4-162">**Description**</span></span>
----------- | ----------- | --------------
<span data-ttu-id="259f4-163">omsagent</span><span class="sxs-lookup"><span data-stu-id="259f4-163">omsagent</span></span> | <span data-ttu-id="259f4-164">1.4.0</span><span class="sxs-lookup"><span data-stu-id="259f4-164">1.4.0</span></span> | <span data-ttu-id="259f4-165">hello Operations Management Suite-Agent för Linux</span><span class="sxs-lookup"><span data-stu-id="259f4-165">hello Operations Management Suite Agent for Linux</span></span>
<span data-ttu-id="259f4-166">omsconfig</span><span class="sxs-lookup"><span data-stu-id="259f4-166">omsconfig</span></span> | <span data-ttu-id="259f4-167">1.1.1</span><span class="sxs-lookup"><span data-stu-id="259f4-167">1.1.1</span></span> | <span data-ttu-id="259f4-168">Konfiguration av agenten för hello OMS-Agent</span><span class="sxs-lookup"><span data-stu-id="259f4-168">Configuration agent for hello OMS Agent</span></span>
<span data-ttu-id="259f4-169">omi</span><span class="sxs-lookup"><span data-stu-id="259f4-169">omi</span></span> | <span data-ttu-id="259f4-170">1.2.0</span><span class="sxs-lookup"><span data-stu-id="259f4-170">1.2.0</span></span> | <span data-ttu-id="259f4-171">Öppna Management Infrastructure (OMI) - lightweight CIM-servern</span><span class="sxs-lookup"><span data-stu-id="259f4-171">Open Management Infrastructure (OMI) - a lightweight CIM Server</span></span>
<span data-ttu-id="259f4-172">scx</span><span class="sxs-lookup"><span data-stu-id="259f4-172">scx</span></span> | <span data-ttu-id="259f4-173">1.6.3</span><span class="sxs-lookup"><span data-stu-id="259f4-173">1.6.3</span></span> | <span data-ttu-id="259f4-174">OMI CIM-Providers för operativsystemet prestandamått</span><span class="sxs-lookup"><span data-stu-id="259f4-174">OMI CIM Providers for operating system performance metrics</span></span>
<span data-ttu-id="259f4-175">Apache cimprov</span><span class="sxs-lookup"><span data-stu-id="259f4-175">apache-cimprov</span></span> | <span data-ttu-id="259f4-176">1.0.1</span><span class="sxs-lookup"><span data-stu-id="259f4-176">1.0.1</span></span> | <span data-ttu-id="259f4-177">Apache HTTP-Server prestandaövervakning provider för OMI.</span><span class="sxs-lookup"><span data-stu-id="259f4-177">Apache HTTP Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="259f4-178">Installeras om Apache HTTP-Server har identifierats.</span><span class="sxs-lookup"><span data-stu-id="259f4-178">Installed if Apache HTTP Server is detected.</span></span>
<span data-ttu-id="259f4-179">MySQL-cimprov</span><span class="sxs-lookup"><span data-stu-id="259f4-179">mysql-cimprov</span></span> | <span data-ttu-id="259f4-180">1.0.1</span><span class="sxs-lookup"><span data-stu-id="259f4-180">1.0.1</span></span> | <span data-ttu-id="259f4-181">MySQL-Server prestandaövervakning provider för OMI.</span><span class="sxs-lookup"><span data-stu-id="259f4-181">MySQL Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="259f4-182">Installeras om MySQL/MariaDB server har identifierats.</span><span class="sxs-lookup"><span data-stu-id="259f4-182">Installed if MySQL/MariaDB server is detected.</span></span>
<span data-ttu-id="259f4-183">docker-cimprov</span><span class="sxs-lookup"><span data-stu-id="259f4-183">docker-cimprov</span></span> | <span data-ttu-id="259f4-184">1.0.0</span><span class="sxs-lookup"><span data-stu-id="259f4-184">1.0.0</span></span> | <span data-ttu-id="259f4-185">Docker-provider för OMI.</span><span class="sxs-lookup"><span data-stu-id="259f4-185">Docker provider for OMI.</span></span> <span data-ttu-id="259f4-186">Installeras om Docker har identifierats.</span><span class="sxs-lookup"><span data-stu-id="259f4-186">Installed if Docker is detected.</span></span>

### <a name="compatibility-with-system-center-operations-manager"></a><span data-ttu-id="259f4-187">Kompatibilitet med System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="259f4-187">Compatibility with System Center Operations Manager</span></span>
<span data-ttu-id="259f4-188">hello OMS-Agent för Linux delar agent binärfiler med hello System Center Operations Manager-agenten.</span><span class="sxs-lookup"><span data-stu-id="259f4-188">hello OMS Agent for Linux shares agent binaries with hello System Center Operations Manager agent.</span></span> <span data-ttu-id="259f4-189">Om du installerar hello OMS-Agent för Linux på ett system som för närvarande hanteras av Operations Manager hello OMI och SCX paket på hello datorn tooa nyare version.</span><span class="sxs-lookup"><span data-stu-id="259f4-189">If you install hello OMS Agent for Linux on a system currently managed by Operations Manager, it hello OMI and SCX packages on hello computer tooa newer version.</span></span> <span data-ttu-id="259f4-190">I den här versionen, hello OMS och System Center 2016 - är Operations Manager/Operations Manager 2012 R2-agenter för Linux kompatibla.</span><span class="sxs-lookup"><span data-stu-id="259f4-190">In this release, hello OMS and System Center 2016 - Operations Manager/Operations Manager 2012 R2 agents for Linux are compatible.</span></span> 

> [!NOTE]
> <span data-ttu-id="259f4-191">System Center 2012 SP1 och tidigare versioner är för närvarande inte kompatibelt och stöds med hello OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="259f4-191">System Center 2012 SP1 and earlier versions are currently not compatible or supported with hello OMS Agent for Linux.</span></span><br>
> <span data-ttu-id="259f4-192">Om hello OMS-Agent för Linux är installerade tooa dator som för närvarande inte övervakas av Operations Manager, och du sedan toomonitor hello datorn med Operations Manager, måste du ändra hello [OMI configuration](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) tidigare toodiscovering hello-dator.</span><span class="sxs-lookup"><span data-stu-id="259f4-192">If hello OMS Agent for Linux is installed tooa computer that is not currently monitored by Operations Manager, and you then wish toomonitor hello computer with Operations Manager, you must modify hello [OMI configuration](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) prior toodiscovering hello computer.</span></span> <span data-ttu-id="259f4-193">**Det här steget är *inte* behövs om hello Operations Manager-agenten är installerad före hello OMS-Agent för Linux.**</span><span class="sxs-lookup"><span data-stu-id="259f4-193">**This step is *not* needed if hello Operations Manager agent is installed before hello OMS Agent for Linux.**</span></span>

### <a name="system-configuration-changes"></a><span data-ttu-id="259f4-194">Ändringar i systemkonfigurationen</span><span class="sxs-lookup"><span data-stu-id="259f4-194">System configuration changes</span></span>
<span data-ttu-id="259f4-195">När du har installerat hello OMS-Agent för Linux-paket tillämpas hello följande ytterligare systemomfattande konfigurationsändringar.</span><span class="sxs-lookup"><span data-stu-id="259f4-195">After installing hello OMS Agent for Linux packages, hello following additional system-wide configuration changes are applied.</span></span> <span data-ttu-id="259f4-196">Dessa artefakter tas bort när hello omsagent paketet är avinstallerat.</span><span class="sxs-lookup"><span data-stu-id="259f4-196">These artifacts are removed when hello omsagent package is uninstalled.</span></span>

* <span data-ttu-id="259f4-197">En icke-privilegierade användare med namnet: `omsagent` skapas.</span><span class="sxs-lookup"><span data-stu-id="259f4-197">A non-privileged user named: `omsagent` is created.</span></span> <span data-ttu-id="259f4-198">Detta är hello konto hello omsagent daemon körs som.</span><span class="sxs-lookup"><span data-stu-id="259f4-198">This is hello account hello omsagent daemon runs as.</span></span>
* <span data-ttu-id="259f4-199">En sudoers ”innehåller” filen skapas vid /etc/sudoers.d/omsagent.</span><span class="sxs-lookup"><span data-stu-id="259f4-199">A sudoers “include” file is created at /etc/sudoers.d/omsagent.</span></span> <span data-ttu-id="259f4-200">Detta ger rätt omsagent toorestart hello syslog och omsagent daemon.</span><span class="sxs-lookup"><span data-stu-id="259f4-200">This authorizes omsagent toorestart hello syslog and omsagent daemons.</span></span> <span data-ttu-id="259f4-201">Om sudo ”” Includes inte stöds i hello installerade versionen av sudo, skrivs dessa poster för/etc/sudoers.</span><span class="sxs-lookup"><span data-stu-id="259f4-201">If sudo “include” directives are not supported in hello installed version of sudo, these entries are written too/etc/sudoers.</span></span>
* <span data-ttu-id="259f4-202">hello syslog-konfigurationen är ändrade tooforward en delmängd av händelser toohello agent.</span><span class="sxs-lookup"><span data-stu-id="259f4-202">hello syslog configuration is modified tooforward a subset of events toohello agent.</span></span> <span data-ttu-id="259f4-203">Mer information finns i hello **konfigurera datainsamling** nedan</span><span class="sxs-lookup"><span data-stu-id="259f4-203">For more information, see hello **Configuring Data Collection** section below</span></span>

### <a name="upgrade-from-a-previous-release"></a><span data-ttu-id="259f4-204">Uppgradera från en tidigare version</span><span class="sxs-lookup"><span data-stu-id="259f4-204">Upgrade from a previous release</span></span>
<span data-ttu-id="259f4-205">Uppgradera från versioner tidigare än 1.0.0-47 stöds i den här versionen.</span><span class="sxs-lookup"><span data-stu-id="259f4-205">Upgrade from versions earlier than 1.0.0-47 is supported in this release.</span></span> <span data-ttu-id="259f4-206">Utföra hello installation med hello `--upgrade` kommandot uppgraderar alla komponenter i hello agent toohello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="259f4-206">Performing hello installation with hello `--upgrade` command upgrades all components of hello agent toohello latest version.</span></span>

## <a name="installing-hello-agent"></a><span data-ttu-id="259f4-207">Installera hello-agent</span><span class="sxs-lookup"><span data-stu-id="259f4-207">Installing hello agent</span></span>

<span data-ttu-id="259f4-208">Det här avsnittet beskrivs hur tooinstall hello OMS-Agent för Linux med en bunndle som innehåller Debian och RPM-paket för varje hello agentkomponenter.</span><span class="sxs-lookup"><span data-stu-id="259f4-208">This section describes how tooinstall hello OMS Agent for Linux using a bunndle, which contains Debian and RPM packages for each of hello agent components.</span></span>  <span data-ttu-id="259f4-209">Den kan installeras direkt eller extraherade tooretrieve hello enskilda paket.</span><span class="sxs-lookup"><span data-stu-id="259f4-209">It can be installed directly or extracted tooretrieve hello individual packages.</span></span>  

<span data-ttu-id="259f4-210">Du måste först ditt OMS arbetsyte-ID och nyckel som du hittar genom att växla toohello [klassiska OMS-portalen](https://mms.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="259f4-210">First you need your OMS workspace ID and key, which you can find by switching toohello [OMS classic portal](https://mms.microsoft.com).</span></span>  <span data-ttu-id="259f4-211">På hello **översikt** sidan hello huvudmenyn väljer **inställningar**, och sedan gå för**anslutna Sources\Linux servrar**.</span><span class="sxs-lookup"><span data-stu-id="259f4-211">On hello **Overview** page, from hello top menu select **Settings**, and then navigate too**Connected Sources\Linux Servers**.</span></span>  <span data-ttu-id="259f4-212">Du ser hello värdet toohello höger om **arbetsyte-ID** och **primärnyckel**.</span><span class="sxs-lookup"><span data-stu-id="259f4-212">You see hello value toohello right of **Workspace ID** and **Primary Key**.</span></span>  <span data-ttu-id="259f4-213">Kopiera och klistra in både i din favorit-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="259f4-213">Copy and paste both into your favorite editor.</span></span>    

1. <span data-ttu-id="259f4-214">Hämta senaste hello [OMS-Agent för Linux (x64)](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x64.sh) eller [OMS-Agent för Linux x86](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x86.sh) från GitHub.</span><span class="sxs-lookup"><span data-stu-id="259f4-214">Download hello latest [OMS Agent for Linux (x64)](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x64.sh) or [OMS Agent for Linux x86](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x86.sh) from GitHub.</span></span>  
2. <span data-ttu-id="259f4-215">Överför hello lämpliga paket (x86 eller x64) tooyour Linux-dator med tjänstanslutningspunkten/sftp.</span><span class="sxs-lookup"><span data-stu-id="259f4-215">Transfer hello appropriate bundle (x86 or x64) tooyour Linux computer using scp/sftp.</span></span>
3. <span data-ttu-id="259f4-216">Installera hello paket med hjälp av hello `--install` eller `--upgrade` argumentet.</span><span class="sxs-lookup"><span data-stu-id="259f4-216">Install hello bundle by using hello `--install` or `--upgrade` argument.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="259f4-217">Om alla befintliga paket installeras, till exempel när hello System Center Operations Manager-agent för Linux redan är installerad, använda hello `--upgrade` argumentet.</span><span class="sxs-lookup"><span data-stu-id="259f4-217">If any existing packages are installed such as when hello System Center Operations Manager agent for Linux is already installed, use hello `--upgrade` argument.</span></span> <span data-ttu-id="259f4-218">tooconnect tooOperations Management Suite under installationen, ange hello `-w <WorkspaceID>` och `-s <Shared Key>` parametrar.</span><span class="sxs-lookup"><span data-stu-id="259f4-218">tooconnect tooOperations Management Suite during installation, provide hello `-w <WorkspaceID>` and `-s <Shared Key>` parameters.</span></span>


#### <a name="tooinstall-and-onboard-directly"></a><span data-ttu-id="259f4-219">tooinstall och publicera direkt</span><span class="sxs-lookup"><span data-stu-id="259f4-219">tooinstall and onboard directly</span></span>
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key>
```

#### <a name="tooupgrade-hello-agent-package"></a><span data-ttu-id="259f4-220">tooupgrade hello-agenten</span><span class="sxs-lookup"><span data-stu-id="259f4-220">tooupgrade hello agent package</span></span>
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade
```

#### <a name="tooinstall-and-onboard-tooa-workspace-in-us-government-cloud"></a><span data-ttu-id="259f4-221">tooinstall och publicera tooa arbetsyta i US Government-moln</span><span class="sxs-lookup"><span data-stu-id="259f4-221">tooinstall and onboard tooa workspace in US Government Cloud</span></span>
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
```

## <a name="configuring-hello-agent-for-use-with-an-http-proxy-server-or-oms-gateway"></a><span data-ttu-id="259f4-222">Konfigurera hello agenten för användning med en HTTP-proxyserver eller OMS-Gateway</span><span class="sxs-lookup"><span data-stu-id="259f4-222">Configuring hello agent for use with an HTTP proxy server or OMS Gateway</span></span>
<span data-ttu-id="259f4-223">hello OMS-Agent för Linux stöder kommunikation via en proxyserver för HTTP eller HTTPS eller OMS Gateway toohello OMS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="259f4-223">hello OMS Agent for Linux supports communicating either through an HTTP or HTTPS proxy server or OMS Gateway toohello OMS service.</span></span>  <span data-ttu-id="259f4-224">Både anonyma och grundläggande autentisering (användarnamn/lösenord) stöds.</span><span class="sxs-lookup"><span data-stu-id="259f4-224">Both anonymous and basic authentication (username/password) is supported.</span></span>  

### <a name="proxy-configuration"></a><span data-ttu-id="259f4-225">Proxykonfiguration</span><span class="sxs-lookup"><span data-stu-id="259f4-225">Proxy configuration</span></span>
<span data-ttu-id="259f4-226">Konfigurationsvärdet för hello-proxy har hello följande syntax:</span><span class="sxs-lookup"><span data-stu-id="259f4-226">hello proxy configuration value has hello following syntax:</span></span>

`[protocol://][user:password@]proxyhost[:port]`

<span data-ttu-id="259f4-227">Egenskap</span><span class="sxs-lookup"><span data-stu-id="259f4-227">Property</span></span>|<span data-ttu-id="259f4-228">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="259f4-228">Description</span></span>
-|-
<span data-ttu-id="259f4-229">Protokoll</span><span class="sxs-lookup"><span data-stu-id="259f4-229">Protocol</span></span>|<span data-ttu-id="259f4-230">HTTP eller https</span><span class="sxs-lookup"><span data-stu-id="259f4-230">http or https</span></span>
<span data-ttu-id="259f4-231">Användaren</span><span class="sxs-lookup"><span data-stu-id="259f4-231">user</span></span>|<span data-ttu-id="259f4-232">Valfritt användarnamn för proxyautentisering</span><span class="sxs-lookup"><span data-stu-id="259f4-232">Optional username for proxy authentication</span></span>
<span data-ttu-id="259f4-233">lösenord</span><span class="sxs-lookup"><span data-stu-id="259f4-233">password</span></span>|<span data-ttu-id="259f4-234">Lösenord för proxyautentisering</span><span class="sxs-lookup"><span data-stu-id="259f4-234">Optional password for proxy authentication</span></span>
<span data-ttu-id="259f4-235">proxyhost</span><span class="sxs-lookup"><span data-stu-id="259f4-235">proxyhost</span></span>|<span data-ttu-id="259f4-236">Adress eller FQDN för hello proxy server/OMS Gateway</span><span class="sxs-lookup"><span data-stu-id="259f4-236">Address or FQDN of hello proxy server/OMS Gateway</span></span>
<span data-ttu-id="259f4-237">port</span><span class="sxs-lookup"><span data-stu-id="259f4-237">port</span></span>|<span data-ttu-id="259f4-238">Valfria portnummer för hello proxy server/OMS Gateway</span><span class="sxs-lookup"><span data-stu-id="259f4-238">Optional port number for hello proxy server/OMS Gateway</span></span>

<span data-ttu-id="259f4-239">Exempel: `http://user01:password@proxy01.contoso.com:8080`</span><span class="sxs-lookup"><span data-stu-id="259f4-239">For example: `http://user01:password@proxy01.contoso.com:8080`</span></span>

<span data-ttu-id="259f4-240">hello-proxyserver kan anges under installationen eller genom att ändra hello proxy.conf-konfigurationsfilen efter installationen.</span><span class="sxs-lookup"><span data-stu-id="259f4-240">hello proxy server can be specified during installation or by modifying hello proxy.conf configuration file after installation.</span></span>   

### <a name="specify-proxy-configuration-during-installation"></a><span data-ttu-id="259f4-241">Ange proxykonfiguration under installationen</span><span class="sxs-lookup"><span data-stu-id="259f4-241">Specify proxy configuration during installation</span></span>
<span data-ttu-id="259f4-242">Hej `-p` eller `--proxy` argument för hello omsagent installation paket anger hello proxy configuration toouse.</span><span class="sxs-lookup"><span data-stu-id="259f4-242">hello `-p` or `--proxy` argument for hello omsagent installation bundle specifies hello proxy configuration toouse.</span></span> 

```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -p http://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
```

### <a name="define-hello-proxy-configuration-in-a-file"></a><span data-ttu-id="259f4-243">Definiera hello proxykonfiguration i en fil</span><span class="sxs-lookup"><span data-stu-id="259f4-243">Define hello proxy configuration in a file</span></span>
<span data-ttu-id="259f4-244">hello proxykonfiguration kan anges i hello filer `/etc/opt/microsoft/omsagent/proxy.conf` och `/etc/opt/microsoft/omsagent/conf/proxy.conf `.</span><span class="sxs-lookup"><span data-stu-id="259f4-244">hello proxy configuration can be set in hello files `/etc/opt/microsoft/omsagent/proxy.conf`  and `/etc/opt/microsoft/omsagent/conf/proxy.conf `.</span></span> <span data-ttu-id="259f4-245">kan skapas eller redigeras hello filer direkt, men deras behörigheter måste vara uppdaterade toogrant hello omiuser användare läsbehörighet på hello-filer.</span><span class="sxs-lookup"><span data-stu-id="259f4-245">hello files can be directly created or edited, but their permissions must be updated toogrant hello omiuser user read permission on hello files.</span></span> <span data-ttu-id="259f4-246">Exempel:</span><span class="sxs-lookup"><span data-stu-id="259f4-246">For example:</span></span>
```
proxyconf="https://proxyuser:proxypassword@proxyserver01:8080"
sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
sudo chmod 600 /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf  
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```

### <a name="removing-hello-proxy-configuration"></a><span data-ttu-id="259f4-247">Ta bort hello proxykonfiguration</span><span class="sxs-lookup"><span data-stu-id="259f4-247">Removing hello proxy configuration</span></span>
<span data-ttu-id="259f4-248">tooremove en tidigare definierad proxykonfigurationen och återställa toodirect anslutning, ta bort hello proxy.conf fil:</span><span class="sxs-lookup"><span data-stu-id="259f4-248">tooremove a previously defined proxy configuration and revert toodirect connectivity, remove hello proxy.conf file:</span></span>
```
sudo rm /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart 
```

## <a name="onboarding-with-operations-management-suite"></a><span data-ttu-id="259f4-249">Onboarding med Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="259f4-249">Onboarding with Operations Management Suite</span></span>
<span data-ttu-id="259f4-250">Om ett arbetsyte-ID och nyckel inte angavs under installationen av hello paket, registreras hello agent senare med Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="259f4-250">If a workspace ID and key were not provided during hello bundle installation, hello agent must be subsequently registered with Operations Management Suite.</span></span>

### <a name="onboarding-using-hello-command-line"></a><span data-ttu-id="259f4-251">Onboarding hello kommandoraden</span><span class="sxs-lookup"><span data-stu-id="259f4-251">Onboarding using hello command line</span></span>
<span data-ttu-id="259f4-252">Kör hello omsadmin.sh kommando tillhandahåller hello arbetsyte-id och nyckel för din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="259f4-252">Run hello omsadmin.sh command supplying hello workspace id and key for your workspace.</span></span> <span data-ttu-id="259f4-253">Det här kommandot måste köras som rot (med sudo-höjning):</span><span class="sxs-lookup"><span data-stu-id="259f4-253">This command must be run as root (with sudo elevation):</span></span>
```
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <Shared Key>
```

### <a name="onboarding-using-a-file"></a><span data-ttu-id="259f4-254">Onboarding med hjälp av en fil</span><span class="sxs-lookup"><span data-stu-id="259f4-254">Onboarding using a file</span></span>
1.  <span data-ttu-id="259f4-255">Skapa hello fil `/etc/omsagent-onboard.conf`.</span><span class="sxs-lookup"><span data-stu-id="259f4-255">Create hello file `/etc/omsagent-onboard.conf`.</span></span> <span data-ttu-id="259f4-256">hello-filen måste vara Läs- och skrivbara för roten.</span><span class="sxs-lookup"><span data-stu-id="259f4-256">hello file must be readable and writable for root.</span></span>
`sudo vi /etc/omsagent-onboard.conf`
2.  <span data-ttu-id="259f4-257">Infoga följande rader i hello-filen med ditt arbetsyte-ID och delad nyckel hello:</span><span class="sxs-lookup"><span data-stu-id="259f4-257">Insert hello following lines in hello file with your Workspace ID and Shared Key:</span></span>

        WORKSPACE_ID=<WorkspaceID>  
        SHARED_KEY=<Shared Key>  
   
3.  <span data-ttu-id="259f4-258">Kör följande kommando tooOnboard tooOMS hello:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh`</span><span class="sxs-lookup"><span data-stu-id="259f4-258">Run hello following command tooOnboard tooOMS: `sudo /opt/microsoft/omsagent/bin/omsadmin.sh`</span></span>
4.  <span data-ttu-id="259f4-259">hello filen tas bort på lyckad onboarding.</span><span class="sxs-lookup"><span data-stu-id="259f4-259">hello file is deleted on successful onboarding.</span></span>

## <a name="enable-hello-oms-agent-for-linux-tooreport-toosystem-center-operations-manager"></a><span data-ttu-id="259f4-260">Aktivera hello OMS-Agent för Linux tooreport tooSystem Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="259f4-260">Enable hello OMS Agent for Linux tooreport tooSystem Center Operations Manager</span></span>
<span data-ttu-id="259f4-261">Utföra hello följande steg tooconfigure hello OMS-Agent för Linux tooreport tooa System Center Operations Manager-hanteringsgruppen.</span><span class="sxs-lookup"><span data-stu-id="259f4-261">Perform hello following steps tooconfigure hello OMS Agent for Linux tooreport tooa System Center Operations Manager management group.</span></span>  

1. <span data-ttu-id="259f4-262">Redigera hello-filen`/etc/opt/omi/conf/omiserver.conf`</span><span class="sxs-lookup"><span data-stu-id="259f4-262">Edit hello file `/etc/opt/omi/conf/omiserver.conf`</span></span>
2. <span data-ttu-id="259f4-263">Se till att hello rad som börjar med **httpsport =** definierar hello port 1270.</span><span class="sxs-lookup"><span data-stu-id="259f4-263">Ensure that hello line beginning with **httpsport=** defines hello port 1270.</span></span> <span data-ttu-id="259f4-264">Exempelvis:`httpsport=1270`</span><span class="sxs-lookup"><span data-stu-id="259f4-264">Such as: `httpsport=1270`</span></span>
3. <span data-ttu-id="259f4-265">Starta om hello OMI-servern:`sudo /opt/omi/bin/service_control restart`</span><span class="sxs-lookup"><span data-stu-id="259f4-265">Restart hello OMI server: `sudo /opt/omi/bin/service_control restart`</span></span>

## <a name="agent-logs"></a><span data-ttu-id="259f4-266">Agenten loggar</span><span class="sxs-lookup"><span data-stu-id="259f4-266">Agent logs</span></span>
<span data-ttu-id="259f4-267">Hej loggar för hello OMS-Agent för Linux finns på: `/var/opt/microsoft/omsagent/<workspace id>/log/` hello loggar för hello omsconfig (agentkonfiguration) programmet finns på: `/var/opt/microsoft/omsconfig/log/` loggar för hello OMI och SCX-komponenter (som tillhandahåller mått prestandadata) finns på:`/var/opt/omi/log/ and /var/opt/microsoft/scx/log`</span><span class="sxs-lookup"><span data-stu-id="259f4-267">hello logs for hello OMS Agent for Linux can be found at: `/var/opt/microsoft/omsagent/<workspace id>/log/` hello logs for hello omsconfig (agent configuration) program can be found at: `/var/opt/microsoft/omsconfig/log/` Logs for hello OMI and SCX components (which provide performance metrics data) can be found at: `/var/opt/omi/log/ and /var/opt/microsoft/scx/log`</span></span>

### <a name="log-rotation-configuration"></a><span data-ttu-id="259f4-268">Loggen rotation konfiguration ##</span><span class="sxs-lookup"><span data-stu-id="259f4-268">Log rotation configuration##</span></span>
<span data-ttu-id="259f4-269">hello rotera konfigurationen av loggen för omsagent finns på:`/etc/logrotate.d/omsagent-<workspace id>`</span><span class="sxs-lookup"><span data-stu-id="259f4-269">hello log rotate configuration for omsagent can be found at: `/etc/logrotate.d/omsagent-<workspace id>`</span></span>

<span data-ttu-id="259f4-270">hello standardinställningarna är:</span><span class="sxs-lookup"><span data-stu-id="259f4-270">hello default settings are:</span></span> 
```
/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log {
    rotate 5
    missingok
    notifempty
    compress
    size 50k
    copytruncate
}
```

## <a name="uninstalling-hello-oms-agent-for-linux"></a><span data-ttu-id="259f4-271">Avinstallera hello OMS-Agent för Linux</span><span class="sxs-lookup"><span data-stu-id="259f4-271">Uninstalling hello OMS Agent for Linux</span></span>
<span data-ttu-id="259f4-272">hello-agentpaketen avinstalleras med körs hello .sh samlingsfilen med hello `--purge` argument som helt tar bort hello agent och dess konfiguration från hello-dator.</span><span class="sxs-lookup"><span data-stu-id="259f4-272">hello agent packages can be uninstalled by running hello bundle .sh file with hello `--purge` argument, which completely removes hello agent and its configuration from hello computer.</span></span>   

```
> sudo rpm -e omsconfig
> sudo rpm -e omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```

## <a name="troubleshooting"></a><span data-ttu-id="259f4-273">Felsökning</span><span class="sxs-lookup"><span data-stu-id="259f4-273">Troubleshooting</span></span>

### <a name="issue-unable-tooconnect-through-proxy-toooms"></a><span data-ttu-id="259f4-274">Problem: Det gick inte tooconnect via proxy tooOMS</span><span class="sxs-lookup"><span data-stu-id="259f4-274">Issue: Unable tooconnect through proxy tooOMS</span></span>

#### <a name="probable-causes"></a><span data-ttu-id="259f4-275">Troliga orsaker</span><span class="sxs-lookup"><span data-stu-id="259f4-275">Probable causes</span></span>
* <span data-ttu-id="259f4-276">hello-proxyn som anges under onboarding var felaktigt</span><span class="sxs-lookup"><span data-stu-id="259f4-276">hello proxy specified during onboarding was incorrect</span></span>
* <span data-ttu-id="259f4-277">hello OMS slutpunkter är inte whitelistested i ditt datacenter</span><span class="sxs-lookup"><span data-stu-id="259f4-277">hello OMS Service Endpoints are not whitelistested in your datacenter</span></span> 

#### <a name="resolutions"></a><span data-ttu-id="259f4-278">Lösningar</span><span class="sxs-lookup"><span data-stu-id="259f4-278">Resolutions</span></span>
1. <span data-ttu-id="259f4-279">Reonboard toohello OMS-tjänsten med hello OMS-Agent för Linux med hjälp av följande kommando med hello alternativet hello `-v` aktiverat.</span><span class="sxs-lookup"><span data-stu-id="259f4-279">Reonboard toohello OMS Service with hello OMS Agent for Linux by using hello following command with hello option `-v` enabled.</span></span> <span data-ttu-id="259f4-280">Detta gör att utförliga utdata av hello-agenten ansluter via hello proxy toohello OMS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="259f4-280">This allows verbose output of hello agent connecting through hello proxy toohello OMS Service.</span></span> 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. <span data-ttu-id="259f4-281">Granska hello avsnittet [konfigurera hello agent för användning med en HTTP-proxy server(#configuring the-agent-for-use-with-a-http-proxy-server) tooverify som du har konfigurerat korrekt hello agent toocommunicate via en proxyserver.</span><span class="sxs-lookup"><span data-stu-id="259f4-281">Review hello section [Configuring hello agent for use with an HTTP proxy server(#configuring the-agent-for-use-with-a-http-proxy-server) tooverify you have properly configured hello agent toocommunicate through a proxy server.</span></span>    
* <span data-ttu-id="259f4-282">Kontrollera hello följande OMS-tjänstens slutpunkter är godkända:</span><span class="sxs-lookup"><span data-stu-id="259f4-282">Double check that hello following OMS Service endpoints are whitelisted:</span></span>

    |<span data-ttu-id="259f4-283">Agentresurs</span><span class="sxs-lookup"><span data-stu-id="259f4-283">Agent Resource</span></span>| <span data-ttu-id="259f4-284">Portar</span><span class="sxs-lookup"><span data-stu-id="259f4-284">Ports</span></span> |  
    |------|---------|  
    |<span data-ttu-id="259f4-285">*.ods.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="259f4-285">*.ods.opinsights.azure.com</span></span> | <span data-ttu-id="259f4-286">Port 443</span><span class="sxs-lookup"><span data-stu-id="259f4-286">Port 443</span></span>|   
    |<span data-ttu-id="259f4-287">*.oms.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="259f4-287">*.oms.opinsights.azure.com</span></span> | <span data-ttu-id="259f4-288">Port 443</span><span class="sxs-lookup"><span data-stu-id="259f4-288">Port 443</span></span>|   
    |<span data-ttu-id="259f4-289">ods.systemcenteradvisor.com</span><span class="sxs-lookup"><span data-stu-id="259f4-289">ods.systemcenteradvisor.com</span></span> | <span data-ttu-id="259f4-290">Port 443</span><span class="sxs-lookup"><span data-stu-id="259f4-290">Port 443</span></span>|   
    |<span data-ttu-id="259f4-291">*.BLOB.Core.Windows.NET/</span><span class="sxs-lookup"><span data-stu-id="259f4-291">*.blob.core.windows.net/</span></span> | <span data-ttu-id="259f4-292">Port 443</span><span class="sxs-lookup"><span data-stu-id="259f4-292">Port 443</span></span>|   

### <a name="issue-you-receive-a-403-error-when-trying-tooonboard"></a><span data-ttu-id="259f4-293">Problem: Du får ett 403-fel vid försök tooonboard</span><span class="sxs-lookup"><span data-stu-id="259f4-293">Issue: You receive a 403 error when trying tooonboard</span></span>

#### <a name="probable-causes"></a><span data-ttu-id="259f4-294">Troliga orsaker</span><span class="sxs-lookup"><span data-stu-id="259f4-294">Probable causes</span></span>
* <span data-ttu-id="259f4-295">Datum och tid är felaktiga på Linux-Server</span><span class="sxs-lookup"><span data-stu-id="259f4-295">Date and Time is incorrect on Linux Server</span></span> 
* <span data-ttu-id="259f4-296">Arbetsyte-ID och Arbetsytenyckel som används är inte korrekt</span><span class="sxs-lookup"><span data-stu-id="259f4-296">Workspace ID and Workspace Key used are not correct</span></span>

#### <a name="resolution"></a><span data-ttu-id="259f4-297">Lösning</span><span class="sxs-lookup"><span data-stu-id="259f4-297">Resolution</span></span>

1. <span data-ttu-id="259f4-298">Kontrollera hello tid på Linux-servern med hello kommandot datum.</span><span class="sxs-lookup"><span data-stu-id="259f4-298">Check hello time on your Linux server with hello command date.</span></span> <span data-ttu-id="259f4-299">Om hello tid +/-15 minuter från aktuell tid, misslyckas onboarding.</span><span class="sxs-lookup"><span data-stu-id="259f4-299">If hello time is +/- 15 minutes from current time, then onboarding fails.</span></span> <span data-ttu-id="259f4-300">toocorrect den här uppdateringen hello datum och/eller tidszonen för Linux-servern.</span><span class="sxs-lookup"><span data-stu-id="259f4-300">toocorrect this update hello date and/or timezone of your Linux server.</span></span> 
2. <span data-ttu-id="259f4-301">Kontrollera att du har installerat hello senaste versionen av hello OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="259f4-301">Verify you have installed hello latest version of hello OMS Agent for Linux.</span></span>  <span data-ttu-id="259f4-302">hello senaste versionen nu meddelar dig om tid skeva orsakar hello onboarding fel.</span><span class="sxs-lookup"><span data-stu-id="259f4-302">hello newest version now notifies you if time skew is causing hello onboarding failure.</span></span>
3. <span data-ttu-id="259f4-303">Reonboard med rätt arbetsyte-ID och Arbetsytenyckel följande hello Installationsinstruktioner tidigare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="259f4-303">Reonboard using correct Workspace ID and Workspace Key following hello installation instructions earlier in this topic.</span></span>

### <a name="issue-you-see-a-500-and-404-error-in-hello-log-file-right-after-onboarding"></a><span data-ttu-id="259f4-304">Problem: Du ser ett 500 och 404-fel i loggfilen för hello direkt efter onboarding</span><span class="sxs-lookup"><span data-stu-id="259f4-304">Issue: You see a 500 and 404 error in hello log file right after onboarding</span></span>
<span data-ttu-id="259f4-305">Detta är ett känt problem som uppstår på första uppladdning av Linux-data i en OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="259f4-305">This is a known issue that occurs on first upload of Linux data into an OMS workspace.</span></span> <span data-ttu-id="259f4-306">Detta påverkar inte data som skickas eller tjänsten erfarenhet.</span><span class="sxs-lookup"><span data-stu-id="259f4-306">This does not affect data being sent or service experience.</span></span>

### <a name="issue--you-are-not-seeing-any-data-in-hello-oms-portal"></a><span data-ttu-id="259f4-307">Problem: Du inte ser några data i hello OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="259f4-307">Issue:  You are not seeing any data in hello OMS portal</span></span>

#### <a name="probable-causes"></a><span data-ttu-id="259f4-308">Troliga orsaker</span><span class="sxs-lookup"><span data-stu-id="259f4-308">Probable causes</span></span>

- <span data-ttu-id="259f4-309">Onboarding toohello OMS-tjänsten misslyckades</span><span class="sxs-lookup"><span data-stu-id="259f4-309">Onboarding toohello OMS Service failed</span></span>
- <span data-ttu-id="259f4-310">Anslutningen toohello OMS-tjänsten är blockerad</span><span class="sxs-lookup"><span data-stu-id="259f4-310">Connection toohello OMS Service is blocked</span></span>
- <span data-ttu-id="259f4-311">OMS-Agent för Linux data säkerhetskopieras</span><span class="sxs-lookup"><span data-stu-id="259f4-311">OMS Agent for Linux data is backed up</span></span>

#### <a name="resolutions"></a><span data-ttu-id="259f4-312">Lösningar</span><span class="sxs-lookup"><span data-stu-id="259f4-312">Resolutions</span></span>
1. <span data-ttu-id="259f4-313">Kontrollera om onboarding hello OMS-tjänsten lyckades genom att kontrollera om hello följande fil:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`</span><span class="sxs-lookup"><span data-stu-id="259f4-313">Check if onboarding hello OMS Service was successful by checking if hello following file exists: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`</span></span>
2. <span data-ttu-id="259f4-314">Reonboard med hello `omsadmin.sh` kommandoradsinstruktioner</span><span class="sxs-lookup"><span data-stu-id="259f4-314">Reonboard using hello `omsadmin.sh` command-line instructions</span></span>
3. <span data-ttu-id="259f4-315">Om du använder en proxy finns toohello proxy Lösningssteg som tidigare.</span><span class="sxs-lookup"><span data-stu-id="259f4-315">If using a proxy, refer toohello proxy resolution steps provided earlier.</span></span>
4. <span data-ttu-id="259f4-316">I vissa fall när hello OMS-Agent för Linux inte kan kommunicera med hello OMS-tjänsten är data på hello agent köade toohello fullständig buffertstorleken, vilket är 50 MB.</span><span class="sxs-lookup"><span data-stu-id="259f4-316">In some cases, when hello OMS Agent for Linux cannot communicate with hello OMS Service, data on hello agent is queued toohello full buffer size, which is 50 MB.</span></span> <span data-ttu-id="259f4-317">hello OMS-Agent för Linux ska startas om genom att köra följande kommando hello: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`.</span><span class="sxs-lookup"><span data-stu-id="259f4-317">hello OMS Agent for Linux should be restarted by running hello following command: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`.</span></span> 

    >[!NOTE]
    ><span data-ttu-id="259f4-318">Det här problemet har lösts i agenten version 1.1.0-28 och senare.</span><span class="sxs-lookup"><span data-stu-id="259f4-318">This issue is fixed in agent version 1.1.0-28 and later.</span></span>
> 