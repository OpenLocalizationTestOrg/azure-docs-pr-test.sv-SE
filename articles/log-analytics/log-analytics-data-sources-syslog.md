---
title: aaaCollect och analysera Syslog-meddelanden i OMS Log Analytics | Microsoft Docs
description: "Syslog är en händelse loggning protokoll som är vanliga tooLinux. Den här artikeln beskriver hur tooconfigure samling Syslog-meddelanden i logganalys och information om hello poster skapas i hello OMS-databasen."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 8bfa0bca3f2f18287d1352c98bbaa2a70e41e276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="syslog-data-sources-in-log-analytics"></a><span data-ttu-id="b4acc-104">Syslog-datakällor i logganalys</span><span class="sxs-lookup"><span data-stu-id="b4acc-104">Syslog data sources in Log Analytics</span></span>
<span data-ttu-id="b4acc-105">Syslog är en händelse loggning protokoll som är vanliga tooLinux.</span><span class="sxs-lookup"><span data-stu-id="b4acc-105">Syslog is an event logging protocol that is common tooLinux.</span></span>  <span data-ttu-id="b4acc-106">Program skickar meddelanden som kan lagras på den lokala datorn för hello eller levereras tooa Syslog-insamlare.</span><span class="sxs-lookup"><span data-stu-id="b4acc-106">Applications will send messages that may be stored on hello local machine or delivered tooa Syslog collector.</span></span>  <span data-ttu-id="b4acc-107">När hello OMS-Agent för Linux är installerat konfigurerar hello lokal Syslog-daemon tooforward meddelanden toohello agent.</span><span class="sxs-lookup"><span data-stu-id="b4acc-107">When hello OMS Agent for Linux is installed, it configures hello local Syslog daemon tooforward messages toohello agent.</span></span>  <span data-ttu-id="b4acc-108">hello sedan skickar agenten hello meddelandet tooLog Analytics där en motsvarande post skapas i hello OMS-databas.</span><span class="sxs-lookup"><span data-stu-id="b4acc-108">hello agent then sends hello message tooLog Analytics where a corresponding record is created in hello OMS repository.</span></span>  

> [!NOTE]
> <span data-ttu-id="b4acc-109">Log Analytics har stöd för meddelanden som skickas av rsyslog eller syslog-ng där rsyslog är hello standard daemon samling.</span><span class="sxs-lookup"><span data-stu-id="b4acc-109">Log Analytics supports collection of messages sent by rsyslog or syslog-ng, where rsyslog is hello default daemon.</span></span> <span data-ttu-id="b4acc-110">hello standard syslog-daemon på version 5 Red Hat Enterprise Linux, CentOS och Oracle Linux-version (sysklog) stöds inte för insamling av syslog-händelser.</span><span class="sxs-lookup"><span data-stu-id="b4acc-110">hello default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="b4acc-111">toocollect syslog-data från den här versionen av dessa distributioner hello [rsyslog daemon](http://rsyslog.com) bör vara installerat och konfigurerat tooreplace sysklog.</span><span class="sxs-lookup"><span data-stu-id="b4acc-111">toocollect syslog data from this version of these distributions, hello [rsyslog daemon](http://rsyslog.com) should be installed and configured tooreplace sysklog.</span></span>
>
>

![Syslog-samling](media/log-analytics-data-sources-syslog/overview.png)

## <a name="configuring-syslog"></a><span data-ttu-id="b4acc-113">Konfigurera Syslog</span><span class="sxs-lookup"><span data-stu-id="b4acc-113">Configuring Syslog</span></span>
<span data-ttu-id="b4acc-114">hello OMS-Agent för Linux endast samlar in händelser med hello verksamhet och allvarlighetsgraderna som anges i dess konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b4acc-114">hello OMS Agent for Linux will only collect events with hello facilities and severities that are specified in its configuration.</span></span>  <span data-ttu-id="b4acc-115">Du kan konfigurera Syslog via hello OMS-portalen eller genom att hantera konfigurationsfiler på Linux-agenter.</span><span class="sxs-lookup"><span data-stu-id="b4acc-115">You can configure Syslog through hello OMS portal or by managing configuration files on your Linux agents.</span></span>

### <a name="configure-syslog-in-hello-oms-portal"></a><span data-ttu-id="b4acc-116">Konfigurera Syslog i hello OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="b4acc-116">Configure Syslog in hello OMS portal</span></span>
<span data-ttu-id="b4acc-117">Konfigurera Syslog från hello [Data-menyn i logganalys-inställningar](log-analytics-data-sources.md#configuring-data-sources).</span><span class="sxs-lookup"><span data-stu-id="b4acc-117">Configure Syslog from hello [Data menu in Log Analytics Settings](log-analytics-data-sources.md#configuring-data-sources).</span></span>  <span data-ttu-id="b4acc-118">Den här konfigurationen levereras toohello konfigurationsfilen på varje Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="b4acc-118">This configuration is delivered toohello configuration file on each Linux agent.</span></span>

<span data-ttu-id="b4acc-119">Du kan lägga till en ny funktion genom att skriva dess namn och klicka på  **+** .</span><span class="sxs-lookup"><span data-stu-id="b4acc-119">You can add a new facility by typing in its name and clicking **+**.</span></span>  <span data-ttu-id="b4acc-120">Endast meddelanden med hello valt allvarlighetsgraderna kommer att samlas in för varje anläggning.</span><span class="sxs-lookup"><span data-stu-id="b4acc-120">For each facility, only messages with hello selected severities will be collected.</span></span>  <span data-ttu-id="b4acc-121">Kontrollera hello allvarlighetsgraderna för hello viss som du vill toocollect.</span><span class="sxs-lookup"><span data-stu-id="b4acc-121">Check hello severities for hello particular facility that you want toocollect.</span></span>  <span data-ttu-id="b4acc-122">Du kan ange ytterligare kriterier toofilter meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b4acc-122">You cannot provide any additional criteria toofilter messages.</span></span>

![Konfigurera Syslog](media/log-analytics-data-sources-syslog/configure.png)

<span data-ttu-id="b4acc-124">Som standard är alla konfigurationsändringar automatiskt pushas tooall agenter.</span><span class="sxs-lookup"><span data-stu-id="b4acc-124">By default, all configuration changes are automatically pushed tooall agents.</span></span>  <span data-ttu-id="b4acc-125">Om du vill tooconfigure Syslog manuellt på varje Linux-agent, avmarkerar du kryssrutan hello *tillämpa konfigurationen toomy Linux-datorerna nedan*.</span><span class="sxs-lookup"><span data-stu-id="b4acc-125">If you want tooconfigure Syslog manually on each Linux agent, then uncheck hello box *Apply below configuration toomy Linux machines*.</span></span>

### <a name="configure-syslog-on-linux-agent"></a><span data-ttu-id="b4acc-126">Konfigurera Syslog på Linux-agent</span><span class="sxs-lookup"><span data-stu-id="b4acc-126">Configure Syslog on Linux agent</span></span>
<span data-ttu-id="b4acc-127">När hello [OMS-agenten är installerad på en Linux-klient](log-analytics-linux-agents.md), installeras en standard syslog-konfigurationsfil som definierar hello-anläggningen och allvarlighetsgraden hello meddelanden som har samlats in.</span><span class="sxs-lookup"><span data-stu-id="b4acc-127">When hello [OMS agent is installed on a Linux client](log-analytics-linux-agents.md), it installs a default syslog configuration file that defines hello facility and severity of hello messages that are collected.</span></span>  <span data-ttu-id="b4acc-128">Du kan ändra den här filen toochange hello-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b4acc-128">You can modify this file toochange hello configuration.</span></span>  <span data-ttu-id="b4acc-129">hello-konfigurationsfilen är olika beroende på hello Syslog-daemon som hello klienten har installerats.</span><span class="sxs-lookup"><span data-stu-id="b4acc-129">hello configuration file is different depending on hello Syslog daemon that hello client has installed.</span></span>

> [!NOTE]
> <span data-ttu-id="b4acc-130">Om du redigerar hello syslog konfiguration, måste du starta om hello syslog-daemon för hello ändringar tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="b4acc-130">If you edit hello syslog configuration, you must restart hello syslog daemon for hello changes tootake effect.</span></span>
>
>

#### <a name="rsyslog"></a><span data-ttu-id="b4acc-131">rsyslog</span><span class="sxs-lookup"><span data-stu-id="b4acc-131">rsyslog</span></span>
<span data-ttu-id="b4acc-132">hello konfigurationsfilen för rsyslog finns på **/etc/rsyslog.d/95-omsagent.conf**.</span><span class="sxs-lookup"><span data-stu-id="b4acc-132">hello configuration file for rsyslog is located at **/etc/rsyslog.d/95-omsagent.conf**.</span></span>  <span data-ttu-id="b4acc-133">Standardinnehållet visas nedan.</span><span class="sxs-lookup"><span data-stu-id="b4acc-133">Its default contents are shown below.</span></span>  <span data-ttu-id="b4acc-134">Detta samlar in syslog-meddelanden skickas från hello lokal agent för alla verksamhet med en nivå av varning eller högre.</span><span class="sxs-lookup"><span data-stu-id="b4acc-134">This collects syslog messages sent from hello local agent for all facilities with a level of warning or higher.</span></span>

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

<span data-ttu-id="b4acc-135">Du kan ta bort en funktion genom att ta bort delen av hello konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="b4acc-135">You can remove a facility by removing its section of hello configuration file.</span></span>  <span data-ttu-id="b4acc-136">Du kan begränsa hello allvarlighetsgraderna som samlas in för en viss funktion genom att ändra den lokal transaktion.</span><span class="sxs-lookup"><span data-stu-id="b4acc-136">You can limit hello severities that are collected for a particular facility by modifying that facility's entry.</span></span>  <span data-ttu-id="b4acc-137">Till exempel toolimit hello användaren anläggning toomessages med en allvarlighetsgrad för fel eller högre du ändrar raden hello configuration file toohello följande:</span><span class="sxs-lookup"><span data-stu-id="b4acc-137">For example, toolimit hello user facility toomessages with a severity of error or higher you would modify that line of hello configuration file toohello following:</span></span>

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a><span data-ttu-id="b4acc-138">Syslog-ng</span><span class="sxs-lookup"><span data-stu-id="b4acc-138">syslog-ng</span></span>
<span data-ttu-id="b4acc-139">hello konfigurationsfilen för syslog-ng är plats **/etc/syslog-ng/syslog-ng.conf**.</span><span class="sxs-lookup"><span data-stu-id="b4acc-139">hello configuration file for syslog-ng is location at **/etc/syslog-ng/syslog-ng.conf**.</span></span>  <span data-ttu-id="b4acc-140">Standardinnehållet visas nedan.</span><span class="sxs-lookup"><span data-stu-id="b4acc-140">Its default contents are shown below.</span></span>  <span data-ttu-id="b4acc-141">Detta samlar in syslog-meddelanden skickas från hello lokal agent för alla resurser och alla grader.</span><span class="sxs-lookup"><span data-stu-id="b4acc-141">This collects syslog messages sent from hello local agent for all facilities and all severities.</span></span>   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };

    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };

    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };

    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };

    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };

    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };

    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

<span data-ttu-id="b4acc-142">Du kan ta bort en funktion genom att ta bort delen av hello konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="b4acc-142">You can remove a facility by removing its section of hello configuration file.</span></span>  <span data-ttu-id="b4acc-143">Du kan begränsa hello allvarlighetsgraderna som samlas in för en viss funktion genom att ta bort dem från listan.</span><span class="sxs-lookup"><span data-stu-id="b4acc-143">You can limit hello severities that are collected for a particular facility by removing them from its list.</span></span>  <span data-ttu-id="b4acc-144">Till exempel toolimit hello användaren anläggning toojust aviseringen och viktiga meddelanden, ändra motsvarande avsnitt i hello configuration file toohello följande:</span><span class="sxs-lookup"><span data-stu-id="b4acc-144">For example, toolimit hello user facility toojust alert and critical messages, you would modify that section of hello configuration file toohello following:</span></span>

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a><span data-ttu-id="b4acc-145">Samla in data från ytterligare Syslog-portar</span><span class="sxs-lookup"><span data-stu-id="b4acc-145">Collecting data from additional Syslog ports</span></span>
<span data-ttu-id="b4acc-146">hello OMS-agenten lyssnar efter Syslog-meddelanden på hello lokal klient på port 25224.</span><span class="sxs-lookup"><span data-stu-id="b4acc-146">hello OMS agent listens for Syslog messages on hello local client on port 25224.</span></span>  <span data-ttu-id="b4acc-147">När hello-agenten är installerad, är en standardkonfiguration för syslog tillämpas och hittades i hello följande plats:</span><span class="sxs-lookup"><span data-stu-id="b4acc-147">When hello agent is installed, a default syslog configuration is applied and found in hello following location:</span></span>

* <span data-ttu-id="b4acc-148">Rsyslog:`/etc/rsyslog.d/95-omsagent.conf`</span><span class="sxs-lookup"><span data-stu-id="b4acc-148">Rsyslog: `/etc/rsyslog.d/95-omsagent.conf`</span></span>
* <span data-ttu-id="b4acc-149">Syslog-ng:`/etc/syslog-ng/syslog-ng.conf`</span><span class="sxs-lookup"><span data-stu-id="b4acc-149">Syslog-ng: `/etc/syslog-ng/syslog-ng.conf`</span></span>

<span data-ttu-id="b4acc-150">Du kan ändra hello portnumret genom att skapa två konfigurationsfiler: en FluentD .config-fil och en rsyslog-eller-syslog-ng fil beroende på hello Syslog-daemon som du har installerat.</span><span class="sxs-lookup"><span data-stu-id="b4acc-150">You can change hello port number by creating two configuration files: a FluentD config file and a rsyslog-or-syslog-ng file depending on hello Syslog daemon you have installed.</span></span>  

* <span data-ttu-id="b4acc-151">Hej FluentD config-fil ska vara en ny fil i: `/etc/opt/microsoft/omsagent/conf/omsagent.d` och Ersätt hello värdet i hello **port** post med din anpassade portnummer.</span><span class="sxs-lookup"><span data-stu-id="b4acc-151">hello FluentD config file should be a new file located in: `/etc/opt/microsoft/omsagent/conf/omsagent.d` and replace hello value in hello **port** entry with your custom port number.</span></span>

        <source>
          type syslog
          port %SYSLOG_PORT%
          bind 127.0.0.1
          protocol_type udp
          tag oms.syslog
        </source>
        <filter oms.syslog.**>
          type filter_syslog
        </filter>

* <span data-ttu-id="b4acc-152">För rsyslog, bör du skapa en ny konfigurationsfil finns på: `/etc/rsyslog.d/` och Ersätt hello värdet % SYSLOG_PORT % med dina egna portnummer.</span><span class="sxs-lookup"><span data-stu-id="b4acc-152">For rsyslog, you should create a new configuration file located in: `/etc/rsyslog.d/` and replace hello value %SYSLOG_PORT% with your custom port number.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="b4acc-153">Om du ändrar det här värdet i konfigurationsfilen för hello `95-omsagent.conf`, skrivs den över när hello agent gäller en standardkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="b4acc-153">If you modify this value in hello configuration file `95-omsagent.conf`, it will be overwritten when hello agent applies a default configuration.</span></span>
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* <span data-ttu-id="b4acc-154">hello syslog ng config ska ändras genom att kopiera hello exempelkonfiguration nedan och lägga till hello anpassade ändrade inställningar toohello slutet av hello syslog-ng.conf-konfigurationsfilen finns i `/etc/syslog-ng/`.</span><span class="sxs-lookup"><span data-stu-id="b4acc-154">hello syslog-ng config should be modified by copying hello example configuration shown below and adding hello custom modified settings toohello end of hello syslog-ng.conf configuration file located in `/etc/syslog-ng/`.</span></span>  <span data-ttu-id="b4acc-155">Gör **inte** använda hello standardetikett **% WORKSPACE_ID % _oms** eller **% WORKSPACE_ID_OMS**, definiera en anpassad etikett toohelp skilja dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="b4acc-155">Do **not** use hello default label **%WORKSPACE_ID%_oms** or **%WORKSPACE_ID_OMS**, define a custom label toohelp distinguish your changes.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="b4acc-156">Om du ändrar hello standardvärdena i konfigurationsfilen för hello skrivs de över när hello agent gäller en standardkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="b4acc-156">If you modify hello default values in hello configuration file, they will be overwritten when hello agent applies a default configuration.</span></span>
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

<span data-ttu-id="b4acc-157">När du har slutfört hello ändringar, hello Syslog och hello måste OMS agent-tjänsten startas om toobe tooensure hello configuration ändringar börja gälla.</span><span class="sxs-lookup"><span data-stu-id="b4acc-157">After completing hello changes, hello Syslog and hello OMS agent service needs toobe restarted tooensure hello configuration changes take effect.</span></span>   

## <a name="syslog-record-properties"></a><span data-ttu-id="b4acc-158">Egenskaper för Syslog-post</span><span class="sxs-lookup"><span data-stu-id="b4acc-158">Syslog record properties</span></span>
<span data-ttu-id="b4acc-159">Syslog-poster har en typ av **Syslog** och ha hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="b4acc-159">Syslog records have a type of **Syslog** and have hello properties in hello following table.</span></span>

| <span data-ttu-id="b4acc-160">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b4acc-160">Property</span></span> | <span data-ttu-id="b4acc-161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b4acc-161">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b4acc-162">Dator</span><span class="sxs-lookup"><span data-stu-id="b4acc-162">Computer</span></span> |<span data-ttu-id="b4acc-163">Dator som hello händelse som samlats in från.</span><span class="sxs-lookup"><span data-stu-id="b4acc-163">Computer that hello event was collected from.</span></span> |
| <span data-ttu-id="b4acc-164">Anläggningen</span><span class="sxs-lookup"><span data-stu-id="b4acc-164">Facility</span></span> |<span data-ttu-id="b4acc-165">Definierar hello tillhör hello system som genererade hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="b4acc-165">Defines hello part of hello system that generated hello message.</span></span> |
| <span data-ttu-id="b4acc-166">HostIP</span><span class="sxs-lookup"><span data-stu-id="b4acc-166">HostIP</span></span> |<span data-ttu-id="b4acc-167">IP-adress hello system hello-meddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="b4acc-167">IP address of hello system sending hello message.</span></span> |
| <span data-ttu-id="b4acc-168">Värdnamn</span><span class="sxs-lookup"><span data-stu-id="b4acc-168">HostName</span></span> |<span data-ttu-id="b4acc-169">Namnet på hello system hello-meddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="b4acc-169">Name of hello system sending hello message.</span></span> |
| <span data-ttu-id="b4acc-170">SeverityLevel</span><span class="sxs-lookup"><span data-stu-id="b4acc-170">SeverityLevel</span></span> |<span data-ttu-id="b4acc-171">Allvarlighetsgrad för hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="b4acc-171">Severity level of hello event.</span></span> |
| <span data-ttu-id="b4acc-172">SyslogMessage</span><span class="sxs-lookup"><span data-stu-id="b4acc-172">SyslogMessage</span></span> |<span data-ttu-id="b4acc-173">Texten för hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="b4acc-173">Text of hello message.</span></span> |
| <span data-ttu-id="b4acc-174">Process-ID</span><span class="sxs-lookup"><span data-stu-id="b4acc-174">ProcessID</span></span> |<span data-ttu-id="b4acc-175">ID för hello process som genererade hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="b4acc-175">ID of hello process that generated hello message.</span></span> |
| <span data-ttu-id="b4acc-176">EventTime</span><span class="sxs-lookup"><span data-stu-id="b4acc-176">EventTime</span></span> |<span data-ttu-id="b4acc-177">Datum och tid då hello händelsen skapades.</span><span class="sxs-lookup"><span data-stu-id="b4acc-177">Date and time that hello event was generated.</span></span> |

## <a name="log-queries-with-syslog-records"></a><span data-ttu-id="b4acc-178">Log-frågor med Syslog-poster</span><span class="sxs-lookup"><span data-stu-id="b4acc-178">Log queries with Syslog records</span></span>
<span data-ttu-id="b4acc-179">hello innehåller följande tabell olika exempel på loggen frågor som hämtar Syslog-poster.</span><span class="sxs-lookup"><span data-stu-id="b4acc-179">hello following table provides different examples of log queries that retrieve Syslog records.</span></span>

| <span data-ttu-id="b4acc-180">Fråga</span><span class="sxs-lookup"><span data-stu-id="b4acc-180">Query</span></span> | <span data-ttu-id="b4acc-181">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b4acc-181">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b4acc-182">Typ = Syslog</span><span class="sxs-lookup"><span data-stu-id="b4acc-182">Type=Syslog</span></span> |<span data-ttu-id="b4acc-183">Alla systemloggar.</span><span class="sxs-lookup"><span data-stu-id="b4acc-183">All Syslogs.</span></span> |
| <span data-ttu-id="b4acc-184">Typ = Syslog SeverityLevel = fel</span><span class="sxs-lookup"><span data-stu-id="b4acc-184">Type=Syslog SeverityLevel=error</span></span> |<span data-ttu-id="b4acc-185">Alla Syslog-poster med allvarlighetsgraden fel.</span><span class="sxs-lookup"><span data-stu-id="b4acc-185">All Syslog records with severity of error.</span></span> |
| <span data-ttu-id="b4acc-186">Typ = Syslog &#124; måttet count() per dator</span><span class="sxs-lookup"><span data-stu-id="b4acc-186">Type=Syslog &#124; measure count() by Computer</span></span> |<span data-ttu-id="b4acc-187">Antal Syslog poster per dator.</span><span class="sxs-lookup"><span data-stu-id="b4acc-187">Count of Syslog records by computer.</span></span> |
| <span data-ttu-id="b4acc-188">Typ = Syslog &#124; måttet count() av anläggningen</span><span class="sxs-lookup"><span data-stu-id="b4acc-188">Type=Syslog &#124; measure count() by Facility</span></span> |<span data-ttu-id="b4acc-189">Antal Syslog poster efter anläggning.</span><span class="sxs-lookup"><span data-stu-id="b4acc-189">Count of Syslog records by facility.</span></span> |

>[!NOTE]
> <span data-ttu-id="b4acc-190">Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello senare frågorna skulle ändra toohello följande.</span><span class="sxs-lookup"><span data-stu-id="b4acc-190">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then hello above queries would change toohello following.</span></span>

> | <span data-ttu-id="b4acc-191">Fråga</span><span class="sxs-lookup"><span data-stu-id="b4acc-191">Query</span></span> | <span data-ttu-id="b4acc-192">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b4acc-192">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b4acc-193">Syslog</span><span class="sxs-lookup"><span data-stu-id="b4acc-193">Syslog</span></span> |<span data-ttu-id="b4acc-194">Alla systemloggar.</span><span class="sxs-lookup"><span data-stu-id="b4acc-194">All Syslogs.</span></span> |
| <span data-ttu-id="b4acc-195">Syslog &#124; där SeverityLevel == ”error”</span><span class="sxs-lookup"><span data-stu-id="b4acc-195">Syslog &#124; where SeverityLevel == "error"</span></span> |<span data-ttu-id="b4acc-196">Alla Syslog-poster med allvarlighetsgraden fel.</span><span class="sxs-lookup"><span data-stu-id="b4acc-196">All Syslog records with severity of error.</span></span> |
| <span data-ttu-id="b4acc-197">Syslog &#124; Sammanfatta AggregatedValue = count() per dator</span><span class="sxs-lookup"><span data-stu-id="b4acc-197">Syslog &#124; summarize AggregatedValue = count() by Computer</span></span> |<span data-ttu-id="b4acc-198">Antal Syslog poster per dator.</span><span class="sxs-lookup"><span data-stu-id="b4acc-198">Count of Syslog records by computer.</span></span> |
| <span data-ttu-id="b4acc-199">Syslog &#124; Sammanfatta AggregatedValue = count() av anläggningen</span><span class="sxs-lookup"><span data-stu-id="b4acc-199">Syslog &#124; summarize AggregatedValue = count() by Facility</span></span> |<span data-ttu-id="b4acc-200">Antal Syslog poster efter anläggning.</span><span class="sxs-lookup"><span data-stu-id="b4acc-200">Count of Syslog records by facility.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b4acc-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b4acc-201">Next steps</span></span>
* <span data-ttu-id="b4acc-202">Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar.</span><span class="sxs-lookup"><span data-stu-id="b4acc-202">Learn about [log searches](log-analytics-log-searches.md) tooanalyze hello data collected from data sources and solutions.</span></span>
* <span data-ttu-id="b4acc-203">Använd [anpassade fält](log-analytics-custom-fields.md) tooparse data från syslog-poster till enskilda fält.</span><span class="sxs-lookup"><span data-stu-id="b4acc-203">Use [Custom Fields](log-analytics-custom-fields.md) tooparse data from syslog records into individual fields.</span></span>
* <span data-ttu-id="b4acc-204">[Konfigurera Linux-agenter](log-analytics-linux-agents.md) toocollect andra typer av data.</span><span class="sxs-lookup"><span data-stu-id="b4acc-204">[Configure Linux agents](log-analytics-linux-agents.md) toocollect other types of data.</span></span>
