---
title: "Ansluta säkerhetsprodukter till säkerhets- och granskningslösningen i Operations Management Suite (OMS) | Microsoft Docs"
description: "Det här dokumentet beskriver hur du ansluter dina säkerhetsprodukter till säkerhets- och granskningslösningen i Operations Management Suite med hjälp av Common Event Format."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 46eee484-e078-4bad-8c89-c88a3508f6aa
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: yurid
ms.openlocfilehash: 710a1fe0ce2b7a1841187cf75f4ffb090cc161e5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="connecting-your-security-products-to-the-operations-management-suite-oms-security-and-audit-solution"></a><span data-ttu-id="ac8f9-103">Ansluta säkerhetsprodukter till säkerhets- och granskningslösningen i Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="ac8f9-103">Connecting your security products to the Operations Management Suite (OMS) Security and Audit Solution</span></span> 
<span data-ttu-id="ac8f9-104">Det här dokumentet beskriver hur du ansluter dina säkerhetsprodukter till säkerhets- och granskningslösningen i OMS.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-104">This document helps you connect your security products into the OMS Security and Audit Solution.</span></span> <span data-ttu-id="ac8f9-105">Följande källor stöds:</span><span class="sxs-lookup"><span data-stu-id="ac8f9-105">The following sources are supported:</span></span>

- <span data-ttu-id="ac8f9-106">CEF-händelser (Common Event Format)</span><span class="sxs-lookup"><span data-stu-id="ac8f9-106">Common Event Format (CEF) events</span></span>
- <span data-ttu-id="ac8f9-107">Cisco ASA-händelser</span><span class="sxs-lookup"><span data-stu-id="ac8f9-107">Cisco ASA events</span></span>


## <a name="what-is-cef"></a><span data-ttu-id="ac8f9-108">Vad är CEF?</span><span class="sxs-lookup"><span data-stu-id="ac8f9-108">What is CEF?</span></span>
<span data-ttu-id="ac8f9-109">Common Event Format (CEF) är ett branschstandardformat ovanpå Syslog-meddelanden som används av många säkerhetsleverantörer för att händelser ska kunna samverka mellan olika plattformar.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-109">Common Event Format (CEF) is an industry standard format on top of Syslog messages, used by many security vendors to allow event interoperability among different platforms.</span></span> <span data-ttu-id="ac8f9-110">Säkerhets- och granskningslösningen i OMS stöder datainmatning med hjälp av CEF så att du kan ansluta dina säkerhetsprodukter till OMS-säkerhetslösningen.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-110">OMS Security and Audit Solution support data ingestion using CEF, which enables you to connect your security products with OMS Security.</span></span> 

<span data-ttu-id="ac8f9-111">Genom att ansluta din datakälla till OMS kan du dra nytta av följande funktioner som ingår i den här plattformen:</span><span class="sxs-lookup"><span data-stu-id="ac8f9-111">By connecting your data source to OMS, you are able to take advantage of the following capabilities that are part of this platform:</span></span>

- <span data-ttu-id="ac8f9-112">Sökning och korrelation</span><span class="sxs-lookup"><span data-stu-id="ac8f9-112">Search & Correlation</span></span>
- <span data-ttu-id="ac8f9-113">Granskning</span><span class="sxs-lookup"><span data-stu-id="ac8f9-113">Auditing</span></span>
- <span data-ttu-id="ac8f9-114">Varning</span><span class="sxs-lookup"><span data-stu-id="ac8f9-114">Alert</span></span>
- <span data-ttu-id="ac8f9-115">Hotinformation</span><span class="sxs-lookup"><span data-stu-id="ac8f9-115">Threat Intelligence</span></span>
- <span data-ttu-id="ac8f9-116">Anmärkningsvärda problem</span><span class="sxs-lookup"><span data-stu-id="ac8f9-116">Notable Issues</span></span>

## <a name="collection-of-security-solution-logs"></a><span data-ttu-id="ac8f9-117">Insamling av loggar för säkerhetslösningen</span><span class="sxs-lookup"><span data-stu-id="ac8f9-117">Collection of security solution logs</span></span>

<span data-ttu-id="ac8f9-118">OMS-säkerhetslösningen har stöd för insamling av loggar via CEF över Syslogs- och [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/)-loggar.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-118">OMS Security supports collection of logs using CEF over Syslogs and [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/) logs.</span></span> <span data-ttu-id="ac8f9-119">I det här exemplet är källan (datorn som genererar loggarna) en Linux-dator som kör syslog-ng-daemon och målet är OMS-säkerhetslösningen.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-119">In this example, the source (computer that generates the logs) is a Linux computer running syslog-ng daemon and the target is OMS Security.</span></span> <span data-ttu-id="ac8f9-120">Du måste förbereda Linux-datorn genom att utföra följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="ac8f9-120">To prepare the Linux computer you will need to perform the following tasks:</span></span>

- <span data-ttu-id="ac8f9-121">Ladda ned OMS-agenten för Linux, version 1.2.0-25 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-121">Download the OMS Agent for Linux, version 1.2.0-25 or above.</span></span>
- <span data-ttu-id="ac8f9-122">Följ stegen i **snabbinstallationsguiden** från [den här artikeln](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) för att installera och publicera agenten på din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-122">Follow the section **Quick Install Guide** from [this article](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) to install and onboard the agent to your workspace.</span></span>

<span data-ttu-id="ac8f9-123">Normalt installeras agenten på en annan dator än den där loggarna genereras.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-123">Typically, the agent is installed on a different computer from the one on which the logs are generated.</span></span> <span data-ttu-id="ac8f9-124">När loggarna ska vidarebefordras till agentdatorn krävs vanligtvis följande steg:</span><span class="sxs-lookup"><span data-stu-id="ac8f9-124">Forwarding the logs to the agent machine will usually require the following steps:</span></span>

- <span data-ttu-id="ac8f9-125">Konfigurera loggningsprodukten eller loggningsdatorn så att den vidarebefordrar nödvändiga händelser till syslog-daemon (rsyslog eller syslog-ng) på agentdatorn.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-125">Configure the logging product/machine to forward the required events to the syslog daemon (rsyslog or syslog-ng) on the agent machine.</span></span>
- <span data-ttu-id="ac8f9-126">Aktivera syslog-daemon på agentdatorn så att meddelanden kan tas emot från ett fjärrsystem.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-126">Enable the syslog daemon on the agent machine to receive messages from a remote system.</span></span>

<span data-ttu-id="ac8f9-127">På agentdatorn måste händelserna skickas från syslog-daemon till den lokala UDP-porten 25226.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-127">On the agent machine, the events need to be sent from the syslog daemon to local UDP port 25226.</span></span> <span data-ttu-id="ac8f9-128">Agenten lyssnar efter inkommande händelser på den här porten.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-128">The agent is listening for incoming events on this port.</span></span> <span data-ttu-id="ac8f9-129">Följande är ett exempel på en konfiguration som skickar alla händelser från det lokala systemet till agenten (du kan ändra konfigurationen så att den passar dina lokala inställningar):</span><span class="sxs-lookup"><span data-stu-id="ac8f9-129">The following is an example configuration for sending all events from the local system to the agent (you can modify the configuration to fit your local settings):</span></span>

1. <span data-ttu-id="ac8f9-130">Öppna terminalfönstret och gå till katalogen */etc/syslog-ng /*</span><span class="sxs-lookup"><span data-stu-id="ac8f9-130">Open the terminal window, and go to the directory */etc/syslog-ng/*</span></span> 
2. <span data-ttu-id="ac8f9-131">Skapa en ny fil *security-config-omsagent.conf* och lägg till följande innehåll: OMS_facility = local4</span><span class="sxs-lookup"><span data-stu-id="ac8f9-131">Create a new file *security-config-omsagent.conf* and add the following content:  OMS_facility = local4</span></span>
    
    <span data-ttu-id="ac8f9-132">filter f_local4_oms { facility(local4); };</span><span class="sxs-lookup"><span data-stu-id="ac8f9-132">filter f_local4_oms { facility(local4); };</span></span>

    <span data-ttu-id="ac8f9-133">destination security_oms { tcp("127.0.0.1" port(25226)); };</span><span class="sxs-lookup"><span data-stu-id="ac8f9-133">destination security_oms { tcp("127.0.0.1" port(25226)); };</span></span>

    <span data-ttu-id="ac8f9-134">log { source(src); filter(f_local4_oms); destination(security_oms); };</span><span class="sxs-lookup"><span data-stu-id="ac8f9-134">log { source(src); filter(f_local4_oms); destination(security_oms); };</span></span>
    
3. <span data-ttu-id="ac8f9-135">Hämta filen *security_events.conf* och placera den i */etc/opt/microsoft/omsagent/conf/omsagent.d/* på OMS-agentdatorn.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-135">Download the file *security_events.conf* and place at */etc/opt/microsoft/omsagent/conf/omsagent.d/* in the OMS Agent computer.</span></span>
4. <span data-ttu-id="ac8f9-136">Skriv kommandot nedan för att starta om daemon för syslog: *för syslog-ng kör:*</span><span class="sxs-lookup"><span data-stu-id="ac8f9-136">Type the command below to restart the syslog daemon:  *For syslog-ng run:*</span></span>
    
    ```
    sudo service rsyslog restart
    ```

    <span data-ttu-id="ac8f9-137">*För rsyslog kör du:*</span><span class="sxs-lookup"><span data-stu-id="ac8f9-137">*For rsyslog run:*</span></span>
    
    ```
    /etc/init.d/syslog-ng restart
    ```
5. <span data-ttu-id="ac8f9-138">Skriv kommandot nedan för att starta om OMS-agenten:</span><span class="sxs-lookup"><span data-stu-id="ac8f9-138">Type the command below to restart the OMS Agent:</span></span>

    <span data-ttu-id="ac8f9-139">*För syslog-ng kör du:*</span><span class="sxs-lookup"><span data-stu-id="ac8f9-139">*For syslog-ng run:*</span></span>
    
    ```
    sudo service omsagent restart
    ```

    <span data-ttu-id="ac8f9-140">*För rsyslog kör du:*</span><span class="sxs-lookup"><span data-stu-id="ac8f9-140">*For rsyslog run:*</span></span>
    
    ```
    systemctl restart omsagent
    ```
6. <span data-ttu-id="ac8f9-141">Skriv kommandot nedan och granska resultatet för att bekräfta att det inte finns några fel i OMS-agentloggen:</span><span class="sxs-lookup"><span data-stu-id="ac8f9-141">Type the command below and review the result to confirm that there are no errors in the OMS Agent log:</span></span>

    ``` 
    tail /var/opt/microsoft/omsagent/log/omsagent.log
    ```

## <a name="reviewing-collected-security-events"></a><span data-ttu-id="ac8f9-142">Gå igenom de insamlade säkerhetshändelserna</span><span class="sxs-lookup"><span data-stu-id="ac8f9-142">Reviewing collected security events</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

<span data-ttu-id="ac8f9-143">När konfigurationen är klar börjar säkerhetshändelsen att matas in av OMS-säkerhetslösningen.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-143">After the configuration is over, the security event will start to be ingested by OMS Security.</span></span> <span data-ttu-id="ac8f9-144">Du kan visualisera dessa händelser genom att öppna Loggsökning, skriva kommandot *Type=CommonSecurityLog* i sökfältet och trycka på Retur.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-144">To visualize those events, open the Log Search, type the command *Type=CommonSecurityLog* in the search field and press ENTER.</span></span> <span data-ttu-id="ac8f9-145">Följande exempel visar resultatet av det här kommandot. Observera att i det här fallet har säkerhetsloggar från flera leverantörer redan matats in i OMS-säkerhetslösningen:</span><span class="sxs-lookup"><span data-stu-id="ac8f9-145">The following example shows the result of this command, notice that in this case OMS Security already ingested security logs from multiple vendors:</span></span>
   
![Utvärdering av säkerhetsbaslinjen i säkerhets- och granskningslösningen i OMS](./media/oms-security-connect-products/oms-security-connect-products-fig1.png)

<span data-ttu-id="ac8f9-147">Du kan förfina sökningen för en enskild leverantör, till exempel för att visualisera Cisco-onlineloggar, genom att skriva: *Type=CommonSecurityLog DeviceVendor=Cisco*.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-147">You can refine this search for one single vendor, for example, to visualize online Cisco logs, type: *Type=CommonSecurityLog DeviceVendor=Cisco*.</span></span> <span data-ttu-id="ac8f9-148">”CommonSecurityLog” har fördefinierade fält för ett eventuellt CEF-huvud, inklusive de grundläggande tilläggen, medan andra tillägg, oavsett om det rör sig om ett ”anpassat tillägg” eller inte, infogas i fältet ”AdditionalExtensions”.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-148">The “CommonSecurityLog” has predefined fields for any CEF header including the basic extensios, while any other extension whether it’s “Custom Extension” or not, will be inserted into "AdditionalExtensions" field.</span></span> <span data-ttu-id="ac8f9-149">Du kan använda funktionen för anpassade fält för att hämta dedikerade fält.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-149">You can use the Custom Fields feature to get dedicated fields from it.</span></span> 

### <a name="accessing-computers-missing-baseline-assessment"></a><span data-ttu-id="ac8f9-150">Kontrollera datorer utan utvärdering av säkerhetsbaslinje</span><span class="sxs-lookup"><span data-stu-id="ac8f9-150">Accessing computers missing baseline assessment</span></span>
<span data-ttu-id="ac8f9-151">OMS stöder baslinjeprofilen för domänmedlemmar i Windows Server 2008 R2 upp till Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-151">OMS supports the domain member baseline profile on Windows Server 2008 R2 up to Windows Server 2012 R2.</span></span> <span data-ttu-id="ac8f9-152">Baslinjen för Windows Server 2016 är inte klar än och läggs till så fort den publiceras.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-152">Windows Server 2016 baseline isn’t final yet and will be added as soon as it is published.</span></span> <span data-ttu-id="ac8f9-153">Alla andra operativsystem som genomsöks med Utvärdering av säkerhetsbaslinje i säkerhets- och granskningslösningen i OMS visas i avsnittet **Datorer utan utvärdering av säkerhetsbaslinje**.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-153">All other operating systems scanned via OMS Security and Audit baseline assessment appear under the **Computers missing baseline assessment** section.</span></span>

## <a name="see-also"></a><span data-ttu-id="ac8f9-154">Se även</span><span class="sxs-lookup"><span data-stu-id="ac8f9-154">See also</span></span>
<span data-ttu-id="ac8f9-155">I det här dokumentet har du lärt dig hur du ansluter din CEF-lösning till OMS.</span><span class="sxs-lookup"><span data-stu-id="ac8f9-155">In this document, you learned how to connect your CEF solution to OMS.</span></span> <span data-ttu-id="ac8f9-156">Mer information om säkerheten i OMS finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="ac8f9-156">To learn more about OMS Security, see the following articles:</span></span>

* [<span data-ttu-id="ac8f9-157">Översikt över Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="ac8f9-157">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="ac8f9-158">Övervaka och svara på säkerhetsaviseringar i säkerhets- och granskningslösningen i Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="ac8f9-158">Monitoring and Responding to Security Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="ac8f9-159">Övervaka resurser i säkerhets- och granskningslösningen i Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="ac8f9-159">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

