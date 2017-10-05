---
title: Samla in och analysera Syslog-meddelanden i OMS Log Analytics | Microsoft Docs
description: "Syslog är en händelse loggning protokoll som är gemensamma för Linux. Den här artikeln beskriver hur du konfigurerar samling Syslog-meddelanden i logganalys och information om poster skapas i OMS-databasen."
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
ms.openlocfilehash: 7513f405d5c7c05a8e6e2b7b0e6313f23a319c84
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="syslog-data-sources-in-log-analytics"></a><span data-ttu-id="548ee-104">Syslog-datakällor i logganalys</span><span class="sxs-lookup"><span data-stu-id="548ee-104">Syslog data sources in Log Analytics</span></span>
<span data-ttu-id="548ee-105">Syslog är en händelse loggning protokoll som är gemensamma för Linux.</span><span class="sxs-lookup"><span data-stu-id="548ee-105">Syslog is an event logging protocol that is common to Linux.</span></span>  <span data-ttu-id="548ee-106">Program skickar meddelanden som kan lagras på den lokala datorn eller levereras till en Syslog-insamlare.</span><span class="sxs-lookup"><span data-stu-id="548ee-106">Applications will send messages that may be stored on the local machine or delivered to a Syslog collector.</span></span>  <span data-ttu-id="548ee-107">När OMS-Agent för Linux installeras konfigurerar den lokala Syslog-daemon för att vidarebefordra meddelanden till agenten.</span><span class="sxs-lookup"><span data-stu-id="548ee-107">When the OMS Agent for Linux is installed, it configures the local Syslog daemon to forward messages to the agent.</span></span>  <span data-ttu-id="548ee-108">Agenten skickar sedan meddelandet till logganalys där motsvarande post har skapats i OMS-databasen.</span><span class="sxs-lookup"><span data-stu-id="548ee-108">The agent then sends the message to Log Analytics where a corresponding record is created in the OMS repository.</span></span>  

> [!NOTE]
> <span data-ttu-id="548ee-109">Log Analytics har stöd för meddelanden som skickas av rsyslog eller syslog-ng där rsyslog är standard daemon samling.</span><span class="sxs-lookup"><span data-stu-id="548ee-109">Log Analytics supports collection of messages sent by rsyslog or syslog-ng, where rsyslog is the default daemon.</span></span> <span data-ttu-id="548ee-110">Standard-daemon för syslog på version 5 Red Hat Enterprise Linux, CentOS och Oracle Linux-version (sysklog) stöds inte för syslog händelseinsamling.</span><span class="sxs-lookup"><span data-stu-id="548ee-110">The default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="548ee-111">Samla in syslog-data från den här versionen av dessa distributioner av [rsyslog daemon](http://rsyslog.com) ska installeras och konfigureras för att ersätta sysklog.</span><span class="sxs-lookup"><span data-stu-id="548ee-111">To collect syslog data from this version of these distributions, the [rsyslog daemon](http://rsyslog.com) should be installed and configured to replace sysklog.</span></span>
>
>

![Syslog-samling](media/log-analytics-data-sources-syslog/overview.png)

## <a name="configuring-syslog"></a><span data-ttu-id="548ee-113">Konfigurera Syslog</span><span class="sxs-lookup"><span data-stu-id="548ee-113">Configuring Syslog</span></span>
<span data-ttu-id="548ee-114">OMS-Agent för Linux endast samlar in händelser med verksamhet och allvarlighetsgraderna som anges i dess konfiguration.</span><span class="sxs-lookup"><span data-stu-id="548ee-114">The OMS Agent for Linux will only collect events with the facilities and severities that are specified in its configuration.</span></span>  <span data-ttu-id="548ee-115">Du kan konfigurera Syslog via OMS-portalen eller genom att hantera konfigurationsfiler på Linux-agenter.</span><span class="sxs-lookup"><span data-stu-id="548ee-115">You can configure Syslog through the OMS portal or by managing configuration files on your Linux agents.</span></span>

### <a name="configure-syslog-in-the-oms-portal"></a><span data-ttu-id="548ee-116">Konfigurera Syslog i OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="548ee-116">Configure Syslog in the OMS portal</span></span>
<span data-ttu-id="548ee-117">Konfigurera Syslog från den [Data-menyn i logganalys-inställningar](log-analytics-data-sources.md#configuring-data-sources).</span><span class="sxs-lookup"><span data-stu-id="548ee-117">Configure Syslog from the [Data menu in Log Analytics Settings](log-analytics-data-sources.md#configuring-data-sources).</span></span>  <span data-ttu-id="548ee-118">Den här konfigurationen levereras till konfigurationsfilen på varje Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="548ee-118">This configuration is delivered to the configuration file on each Linux agent.</span></span>

<span data-ttu-id="548ee-119">Du kan lägga till en ny funktion genom att skriva dess namn och klicka på  **+** .</span><span class="sxs-lookup"><span data-stu-id="548ee-119">You can add a new facility by typing in its name and clicking **+**.</span></span>  <span data-ttu-id="548ee-120">Endast meddelanden med de valda allvarlighetsgraderna kommer att samlas in för varje anläggning.</span><span class="sxs-lookup"><span data-stu-id="548ee-120">For each facility, only messages with the selected severities will be collected.</span></span>  <span data-ttu-id="548ee-121">Kontrollera allvarlighetsgraderna för viss-funktion som du vill samla in.</span><span class="sxs-lookup"><span data-stu-id="548ee-121">Check the severities for the particular facility that you want to collect.</span></span>  <span data-ttu-id="548ee-122">Du kan inte ange några ytterligare kriterier för att filtrera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="548ee-122">You cannot provide any additional criteria to filter messages.</span></span>

![Konfigurera Syslog](media/log-analytics-data-sources-syslog/configure.png)

<span data-ttu-id="548ee-124">Som standard pushas alla konfigurationsändringar automatiskt till alla agenter.</span><span class="sxs-lookup"><span data-stu-id="548ee-124">By default, all configuration changes are automatically pushed to all agents.</span></span>  <span data-ttu-id="548ee-125">Om du vill konfigurera Syslog manuellt på varje Linux-agenten och avmarkera sedan kryssrutan *Använd konfigurationen nedan för Mina Linux-datorer*.</span><span class="sxs-lookup"><span data-stu-id="548ee-125">If you want to configure Syslog manually on each Linux agent, then uncheck the box *Apply below configuration to my Linux machines*.</span></span>

### <a name="configure-syslog-on-linux-agent"></a><span data-ttu-id="548ee-126">Konfigurera Syslog på Linux-agent</span><span class="sxs-lookup"><span data-stu-id="548ee-126">Configure Syslog on Linux agent</span></span>
<span data-ttu-id="548ee-127">När den [OMS-agenten är installerad på en Linux-klient](log-analytics-linux-agents.md), installeras en standard syslog-konfigurationsfil som definierar anläggning och allvarlighetsgrad för meddelanden som samlas in.</span><span class="sxs-lookup"><span data-stu-id="548ee-127">When the [OMS agent is installed on a Linux client](log-analytics-linux-agents.md), it installs a default syslog configuration file that defines the facility and severity of the messages that are collected.</span></span>  <span data-ttu-id="548ee-128">Du kan ändra den här filen om du vill ändra konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="548ee-128">You can modify this file to change the configuration.</span></span>  <span data-ttu-id="548ee-129">Konfigurationsfilen är olika beroende på daemon för Syslog som klienten har installerats.</span><span class="sxs-lookup"><span data-stu-id="548ee-129">The configuration file is different depending on the Syslog daemon that the client has installed.</span></span>

> [!NOTE]
> <span data-ttu-id="548ee-130">Om du redigerar syslog-konfigurationen, måste du starta om syslog-daemon för att ändringarna ska börja gälla.</span><span class="sxs-lookup"><span data-stu-id="548ee-130">If you edit the syslog configuration, you must restart the syslog daemon for the changes to take effect.</span></span>
>
>

#### <a name="rsyslog"></a><span data-ttu-id="548ee-131">rsyslog</span><span class="sxs-lookup"><span data-stu-id="548ee-131">rsyslog</span></span>
<span data-ttu-id="548ee-132">Konfigurationsfilen för rsyslog finns på **/etc/rsyslog.d/95-omsagent.conf**.</span><span class="sxs-lookup"><span data-stu-id="548ee-132">The configuration file for rsyslog is located at **/etc/rsyslog.d/95-omsagent.conf**.</span></span>  <span data-ttu-id="548ee-133">Standardinnehållet visas nedan.</span><span class="sxs-lookup"><span data-stu-id="548ee-133">Its default contents are shown below.</span></span>  <span data-ttu-id="548ee-134">Detta samlar in syslog-meddelanden skickas från den lokala agenten för alla verksamhet med en nivå av varning eller högre.</span><span class="sxs-lookup"><span data-stu-id="548ee-134">This collects syslog messages sent from the local agent for all facilities with a level of warning or higher.</span></span>

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

<span data-ttu-id="548ee-135">Du kan ta bort en funktion genom att ta bort delen av konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="548ee-135">You can remove a facility by removing its section of the configuration file.</span></span>  <span data-ttu-id="548ee-136">Du kan begränsa de allvarlighetsgraderna som samlas in för en viss funktion genom att ändra den lokal transaktion.</span><span class="sxs-lookup"><span data-stu-id="548ee-136">You can limit the severities that are collected for a particular facility by modifying that facility's entry.</span></span>  <span data-ttu-id="548ee-137">Om du vill begränsa användare möjlighet att meddelanden med en allvarlighetsgrad för fel eller högre ändrar du till exempel den raden i konfigurationsfilen på följande:</span><span class="sxs-lookup"><span data-stu-id="548ee-137">For example, to limit the user facility to messages with a severity of error or higher you would modify that line of the configuration file to the following:</span></span>

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a><span data-ttu-id="548ee-138">Syslog-ng</span><span class="sxs-lookup"><span data-stu-id="548ee-138">syslog-ng</span></span>
<span data-ttu-id="548ee-139">Konfigurationsfilen för syslog-ng är plats **/etc/syslog-ng/syslog-ng.conf**.</span><span class="sxs-lookup"><span data-stu-id="548ee-139">The configuration file for syslog-ng is location at **/etc/syslog-ng/syslog-ng.conf**.</span></span>  <span data-ttu-id="548ee-140">Standardinnehållet visas nedan.</span><span class="sxs-lookup"><span data-stu-id="548ee-140">Its default contents are shown below.</span></span>  <span data-ttu-id="548ee-141">Detta samlar in syslog-meddelanden skickas från den lokala agenten för alla resurser och alla grader.</span><span class="sxs-lookup"><span data-stu-id="548ee-141">This collects syslog messages sent from the local agent for all facilities and all severities.</span></span>   

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

<span data-ttu-id="548ee-142">Du kan ta bort en funktion genom att ta bort delen av konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="548ee-142">You can remove a facility by removing its section of the configuration file.</span></span>  <span data-ttu-id="548ee-143">Du kan begränsa de allvarlighetsgraderna som samlas in för en viss funktion genom att ta bort dem från listan.</span><span class="sxs-lookup"><span data-stu-id="548ee-143">You can limit the severities that are collected for a particular facility by removing them from its list.</span></span>  <span data-ttu-id="548ee-144">Om du vill begränsa användare möjlighet att bara aviseringen och viktiga meddelanden ändrar du till exempel det avsnittet av konfigurationsfilen till följande:</span><span class="sxs-lookup"><span data-stu-id="548ee-144">For example, to limit the user facility to just alert and critical messages, you would modify that section of the configuration file to the following:</span></span>

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a><span data-ttu-id="548ee-145">Samla in data från ytterligare Syslog-portar</span><span class="sxs-lookup"><span data-stu-id="548ee-145">Collecting data from additional Syslog ports</span></span>
<span data-ttu-id="548ee-146">OMS-agenten lyssnar efter Syslog-meddelanden på den lokala klienten på port 25224.</span><span class="sxs-lookup"><span data-stu-id="548ee-146">The OMS agent listens for Syslog messages on the local client on port 25224.</span></span>  <span data-ttu-id="548ee-147">När agenten har installerats kan en syslog-standardkonfiguration appliceras och finns på följande plats:</span><span class="sxs-lookup"><span data-stu-id="548ee-147">When the agent is installed, a default syslog configuration is applied and found in the following location:</span></span>

* <span data-ttu-id="548ee-148">Rsyslog:`/etc/rsyslog.d/95-omsagent.conf`</span><span class="sxs-lookup"><span data-stu-id="548ee-148">Rsyslog: `/etc/rsyslog.d/95-omsagent.conf`</span></span>
* <span data-ttu-id="548ee-149">Syslog-ng:`/etc/syslog-ng/syslog-ng.conf`</span><span class="sxs-lookup"><span data-stu-id="548ee-149">Syslog-ng: `/etc/syslog-ng/syslog-ng.conf`</span></span>

<span data-ttu-id="548ee-150">Du kan ändra portnumret genom att skapa två konfigurationsfiler: en FluentD .config-fil och en rsyslog-eller-syslog-ng fil beroende på daemon för Syslog som du har installerat.</span><span class="sxs-lookup"><span data-stu-id="548ee-150">You can change the port number by creating two configuration files: a FluentD config file and a rsyslog-or-syslog-ng file depending on the Syslog daemon you have installed.</span></span>  

* <span data-ttu-id="548ee-151">Konfigurationsfilen FluentD ska vara en ny fil i: `/etc/opt/microsoft/omsagent/conf/omsagent.d` och ersätta värdet i den **port** post med din anpassade portnummer.</span><span class="sxs-lookup"><span data-stu-id="548ee-151">The FluentD config file should be a new file located in: `/etc/opt/microsoft/omsagent/conf/omsagent.d` and replace the value in the **port** entry with your custom port number.</span></span>

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

* <span data-ttu-id="548ee-152">För rsyslog, bör du skapa en ny konfigurationsfil finns på: `/etc/rsyslog.d/` och Ersätt värdet % SYSLOG_PORT % med dina egna portnummer.</span><span class="sxs-lookup"><span data-stu-id="548ee-152">For rsyslog, you should create a new configuration file located in: `/etc/rsyslog.d/` and replace the value %SYSLOG_PORT% with your custom port number.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="548ee-153">Om du ändrar det här värdet i konfigurationsfilen `95-omsagent.conf`, skrivs den över när agenten använder en standardkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="548ee-153">If you modify this value in the configuration file `95-omsagent.conf`, it will be overwritten when the agent applies a default configuration.</span></span>
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* <span data-ttu-id="548ee-154">Syslog-ng config ska ändras genom att kopiera exempelkonfiguration nedan och lägga till anpassade ändrade inställningar i slutet av konfigurationsfilen syslog-ng.conf finns i `/etc/syslog-ng/`.</span><span class="sxs-lookup"><span data-stu-id="548ee-154">The syslog-ng config should be modified by copying the example configuration shown below and adding the custom modified settings to the end of the syslog-ng.conf configuration file located in `/etc/syslog-ng/`.</span></span>  <span data-ttu-id="548ee-155">Gör **inte** använda Standardetiketten **% WORKSPACE_ID % _oms** eller **% WORKSPACE_ID_OMS**, definiera en anpassad etikett för att skilja på dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="548ee-155">Do **not** use the default label **%WORKSPACE_ID%_oms** or **%WORKSPACE_ID_OMS**, define a custom label to help distinguish your changes.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="548ee-156">Om du ändrar standardvärdena i konfigurationsfilen skrivs de över när agenten använder en standardkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="548ee-156">If you modify the default values in the configuration file, they will be overwritten when the agent applies a default configuration.</span></span>
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

<span data-ttu-id="548ee-157">När du har slutfört ändringarna, Syslog och OMS-agent måste-tjänsten startas om för att säkerställa att konfigurationsändringarna börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="548ee-157">After completing the changes, the Syslog and the OMS agent service needs to be restarted to ensure the configuration changes take effect.</span></span>   

## <a name="syslog-record-properties"></a><span data-ttu-id="548ee-158">Egenskaper för Syslog-post</span><span class="sxs-lookup"><span data-stu-id="548ee-158">Syslog record properties</span></span>
<span data-ttu-id="548ee-159">Syslog-poster har en typ av **Syslog** och ha egenskaper i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="548ee-159">Syslog records have a type of **Syslog** and have the properties in the following table.</span></span>

| <span data-ttu-id="548ee-160">Egenskap</span><span class="sxs-lookup"><span data-stu-id="548ee-160">Property</span></span> | <span data-ttu-id="548ee-161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="548ee-161">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="548ee-162">Dator</span><span class="sxs-lookup"><span data-stu-id="548ee-162">Computer</span></span> |<span data-ttu-id="548ee-163">Dator som händelsen samlats in från.</span><span class="sxs-lookup"><span data-stu-id="548ee-163">Computer that the event was collected from.</span></span> |
| <span data-ttu-id="548ee-164">Anläggningen</span><span class="sxs-lookup"><span data-stu-id="548ee-164">Facility</span></span> |<span data-ttu-id="548ee-165">Definierar en del av systemet som genererade meddelandet.</span><span class="sxs-lookup"><span data-stu-id="548ee-165">Defines the part of the system that generated the message.</span></span> |
| <span data-ttu-id="548ee-166">HostIP</span><span class="sxs-lookup"><span data-stu-id="548ee-166">HostIP</span></span> |<span data-ttu-id="548ee-167">IP-adress i systemet som skickar meddelandet.</span><span class="sxs-lookup"><span data-stu-id="548ee-167">IP address of the system sending the message.</span></span> |
| <span data-ttu-id="548ee-168">Värdnamn</span><span class="sxs-lookup"><span data-stu-id="548ee-168">HostName</span></span> |<span data-ttu-id="548ee-169">Namnet på system som skickar meddelandet.</span><span class="sxs-lookup"><span data-stu-id="548ee-169">Name of the system sending the message.</span></span> |
| <span data-ttu-id="548ee-170">SeverityLevel</span><span class="sxs-lookup"><span data-stu-id="548ee-170">SeverityLevel</span></span> |<span data-ttu-id="548ee-171">Allvarlighetsgrad för händelsen.</span><span class="sxs-lookup"><span data-stu-id="548ee-171">Severity level of the event.</span></span> |
| <span data-ttu-id="548ee-172">SyslogMessage</span><span class="sxs-lookup"><span data-stu-id="548ee-172">SyslogMessage</span></span> |<span data-ttu-id="548ee-173">Texten i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="548ee-173">Text of the message.</span></span> |
| <span data-ttu-id="548ee-174">Process-ID</span><span class="sxs-lookup"><span data-stu-id="548ee-174">ProcessID</span></span> |<span data-ttu-id="548ee-175">ID för den process som genererade meddelandet.</span><span class="sxs-lookup"><span data-stu-id="548ee-175">ID of the process that generated the message.</span></span> |
| <span data-ttu-id="548ee-176">EventTime</span><span class="sxs-lookup"><span data-stu-id="548ee-176">EventTime</span></span> |<span data-ttu-id="548ee-177">Datum och tid då händelsen skapades.</span><span class="sxs-lookup"><span data-stu-id="548ee-177">Date and time that the event was generated.</span></span> |

## <a name="log-queries-with-syslog-records"></a><span data-ttu-id="548ee-178">Log-frågor med Syslog-poster</span><span class="sxs-lookup"><span data-stu-id="548ee-178">Log queries with Syslog records</span></span>
<span data-ttu-id="548ee-179">Följande tabell innehåller olika exempel på loggen frågor som hämtar Syslog-poster.</span><span class="sxs-lookup"><span data-stu-id="548ee-179">The following table provides different examples of log queries that retrieve Syslog records.</span></span>

| <span data-ttu-id="548ee-180">Fråga</span><span class="sxs-lookup"><span data-stu-id="548ee-180">Query</span></span> | <span data-ttu-id="548ee-181">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="548ee-181">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="548ee-182">Typ = Syslog</span><span class="sxs-lookup"><span data-stu-id="548ee-182">Type=Syslog</span></span> |<span data-ttu-id="548ee-183">Alla systemloggar.</span><span class="sxs-lookup"><span data-stu-id="548ee-183">All Syslogs.</span></span> |
| <span data-ttu-id="548ee-184">Typ = Syslog SeverityLevel = fel</span><span class="sxs-lookup"><span data-stu-id="548ee-184">Type=Syslog SeverityLevel=error</span></span> |<span data-ttu-id="548ee-185">Alla Syslog-poster med allvarlighetsgraden fel.</span><span class="sxs-lookup"><span data-stu-id="548ee-185">All Syslog records with severity of error.</span></span> |
| <span data-ttu-id="548ee-186">Typ = Syslog &#124; måttet count() per dator</span><span class="sxs-lookup"><span data-stu-id="548ee-186">Type=Syslog &#124; measure count() by Computer</span></span> |<span data-ttu-id="548ee-187">Antal Syslog poster per dator.</span><span class="sxs-lookup"><span data-stu-id="548ee-187">Count of Syslog records by computer.</span></span> |
| <span data-ttu-id="548ee-188">Typ = Syslog &#124; måttet count() av anläggningen</span><span class="sxs-lookup"><span data-stu-id="548ee-188">Type=Syslog &#124; measure count() by Facility</span></span> |<span data-ttu-id="548ee-189">Antal Syslog poster efter anläggning.</span><span class="sxs-lookup"><span data-stu-id="548ee-189">Count of Syslog records by facility.</span></span> |

>[!NOTE]
> <span data-ttu-id="548ee-190">Om din arbetsyta har uppgraderats till [det nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md) ändras frågorna ovan till följande.</span><span class="sxs-lookup"><span data-stu-id="548ee-190">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then the above queries would change to the following.</span></span>

> | <span data-ttu-id="548ee-191">Fråga</span><span class="sxs-lookup"><span data-stu-id="548ee-191">Query</span></span> | <span data-ttu-id="548ee-192">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="548ee-192">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="548ee-193">Syslog</span><span class="sxs-lookup"><span data-stu-id="548ee-193">Syslog</span></span> |<span data-ttu-id="548ee-194">Alla systemloggar.</span><span class="sxs-lookup"><span data-stu-id="548ee-194">All Syslogs.</span></span> |
| <span data-ttu-id="548ee-195">Syslog &#124; där SeverityLevel == ”error”</span><span class="sxs-lookup"><span data-stu-id="548ee-195">Syslog &#124; where SeverityLevel == "error"</span></span> |<span data-ttu-id="548ee-196">Alla Syslog-poster med allvarlighetsgraden fel.</span><span class="sxs-lookup"><span data-stu-id="548ee-196">All Syslog records with severity of error.</span></span> |
| <span data-ttu-id="548ee-197">Syslog &#124; Sammanfatta AggregatedValue = count() per dator</span><span class="sxs-lookup"><span data-stu-id="548ee-197">Syslog &#124; summarize AggregatedValue = count() by Computer</span></span> |<span data-ttu-id="548ee-198">Antal Syslog poster per dator.</span><span class="sxs-lookup"><span data-stu-id="548ee-198">Count of Syslog records by computer.</span></span> |
| <span data-ttu-id="548ee-199">Syslog &#124; Sammanfatta AggregatedValue = count() av anläggningen</span><span class="sxs-lookup"><span data-stu-id="548ee-199">Syslog &#124; summarize AggregatedValue = count() by Facility</span></span> |<span data-ttu-id="548ee-200">Antal Syslog poster efter anläggning.</span><span class="sxs-lookup"><span data-stu-id="548ee-200">Count of Syslog records by facility.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="548ee-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="548ee-201">Next steps</span></span>
* <span data-ttu-id="548ee-202">Lär dig mer om [logga sökningar](log-analytics-log-searches.md) att analysera data som samlas in från datakällor och lösningar.</span><span class="sxs-lookup"><span data-stu-id="548ee-202">Learn about [log searches](log-analytics-log-searches.md) to analyze the data collected from data sources and solutions.</span></span>
* <span data-ttu-id="548ee-203">Använd [anpassade fält](log-analytics-custom-fields.md) att tolka data från syslog-poster till enskilda fält.</span><span class="sxs-lookup"><span data-stu-id="548ee-203">Use [Custom Fields](log-analytics-custom-fields.md) to parse data from syslog records into individual fields.</span></span>
* <span data-ttu-id="548ee-204">[Konfigurera Linux-agenter](log-analytics-linux-agents.md) att samla in andra typer av data.</span><span class="sxs-lookup"><span data-stu-id="548ee-204">[Configure Linux agents](log-analytics-linux-agents.md) to collect other types of data.</span></span>
