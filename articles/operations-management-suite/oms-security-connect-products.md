---
title: "aaaConnecting din säkerhet produkter toohello Operations Management Suite (OMS) säkerhet och granska lösningen | Microsoft Docs"
description: "Det här avsnittet får du tooconnect din säkerhet produkter tooOperations Management Suite säkerhets- och granska lösningen formatet händelse vanliga."
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
ms.openlocfilehash: 0f4b372d0379987c4e249628a3c8d52733be65c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-your-security-products-toohello-operations-management-suite-oms-security-and-audit-solution"></a><span data-ttu-id="b0792-103">Ansluta din säkerhet produkter toohello Operations Management Suite (OMS) säkerhet och granska lösning</span><span class="sxs-lookup"><span data-stu-id="b0792-103">Connecting your security products toohello Operations Management Suite (OMS) Security and Audit Solution</span></span> 
<span data-ttu-id="b0792-104">Det här dokumentet hjälper dig att ansluta din säkerhetsprodukter i hello OMS säkerhet och granska lösningen.</span><span class="sxs-lookup"><span data-stu-id="b0792-104">This document helps you connect your security products into hello OMS Security and Audit Solution.</span></span> <span data-ttu-id="b0792-105">följande källor hello stöds:</span><span class="sxs-lookup"><span data-stu-id="b0792-105">hello following sources are supported:</span></span>

- <span data-ttu-id="b0792-106">CEF-händelser (Common Event Format)</span><span class="sxs-lookup"><span data-stu-id="b0792-106">Common Event Format (CEF) events</span></span>
- <span data-ttu-id="b0792-107">Cisco ASA-händelser</span><span class="sxs-lookup"><span data-stu-id="b0792-107">Cisco ASA events</span></span>


## <a name="what-is-cef"></a><span data-ttu-id="b0792-108">Vad är CEF?</span><span class="sxs-lookup"><span data-stu-id="b0792-108">What is CEF?</span></span>
<span data-ttu-id="b0792-109">Vanliga händelse (CEF) är en branschstandard för format ovanpå Syslog-meddelanden som används av många säkerhet leverantörer tooallow händelse samverkan mellan olika plattformar.</span><span class="sxs-lookup"><span data-stu-id="b0792-109">Common Event Format (CEF) is an industry standard format on top of Syslog messages, used by many security vendors tooallow event interoperability among different platforms.</span></span> <span data-ttu-id="b0792-110">OMS säkerhets- och gransknings-lösningen stöd för datapåfyllning med CEF, vilket gör att du tooconnect din säkerhetsprodukter OMS-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="b0792-110">OMS Security and Audit Solution support data ingestion using CEF, which enables you tooconnect your security products with OMS Security.</span></span> 

<span data-ttu-id="b0792-111">Genom att ansluta dina datakälla tooOMS är kan tootake nytta av följande funktioner som ingår i den här plattformen hello:</span><span class="sxs-lookup"><span data-stu-id="b0792-111">By connecting your data source tooOMS, you are able tootake advantage of hello following capabilities that are part of this platform:</span></span>

- <span data-ttu-id="b0792-112">Sökning och korrelation</span><span class="sxs-lookup"><span data-stu-id="b0792-112">Search & Correlation</span></span>
- <span data-ttu-id="b0792-113">Granskning</span><span class="sxs-lookup"><span data-stu-id="b0792-113">Auditing</span></span>
- <span data-ttu-id="b0792-114">Varning</span><span class="sxs-lookup"><span data-stu-id="b0792-114">Alert</span></span>
- <span data-ttu-id="b0792-115">Hotinformation</span><span class="sxs-lookup"><span data-stu-id="b0792-115">Threat Intelligence</span></span>
- <span data-ttu-id="b0792-116">Anmärkningsvärda problem</span><span class="sxs-lookup"><span data-stu-id="b0792-116">Notable Issues</span></span>

## <a name="collection-of-security-solution-logs"></a><span data-ttu-id="b0792-117">Insamling av loggar för säkerhetslösningen</span><span class="sxs-lookup"><span data-stu-id="b0792-117">Collection of security solution logs</span></span>

<span data-ttu-id="b0792-118">OMS-säkerhetslösningen har stöd för insamling av loggar via CEF över Syslogs- och [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/)-loggar.</span><span class="sxs-lookup"><span data-stu-id="b0792-118">OMS Security supports collection of logs using CEF over Syslogs and [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/) logs.</span></span> <span data-ttu-id="b0792-119">I det här exemplet hello källan (dator som genererar hello loggar) är en Linux-dator som kör daemon för syslog-ng och hello målet är OMS-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="b0792-119">In this example, hello source (computer that generates hello logs) is a Linux computer running syslog-ng daemon and hello target is OMS Security.</span></span> <span data-ttu-id="b0792-120">tooprepare hello Linux-dator måste tooperform hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="b0792-120">tooprepare hello Linux computer you will need tooperform hello following tasks:</span></span>

- <span data-ttu-id="b0792-121">Hämta hello OMS-Agent för Linux version 1.2.0-25 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b0792-121">Download hello OMS Agent for Linux, version 1.2.0-25 or above.</span></span>
- <span data-ttu-id="b0792-122">Följ hello avsnittet **installera snabbguide** från [i den här artikeln](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) tooinstall och publicera hello agent tooyour arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="b0792-122">Follow hello section **Quick Install Guide** from [this article](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) tooinstall and onboard hello agent tooyour workspace.</span></span>

<span data-ttu-id="b0792-123">Normalt är hello-agenten installerad på en annan dator från hello på vilka hello loggar genereras.</span><span class="sxs-lookup"><span data-stu-id="b0792-123">Typically, hello agent is installed on a different computer from hello one on which hello logs are generated.</span></span> <span data-ttu-id="b0792-124">Vidarebefordran hello loggar toohello agentdator kräver vanligtvis hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b0792-124">Forwarding hello logs toohello agent machine will usually require hello following steps:</span></span>

- <span data-ttu-id="b0792-125">Konfigurera hello loggning produkten/datorn tooforward hello nödvändiga händelser toohello daemon för syslog (rsyslog eller syslog-ng) på hello agentdator.</span><span class="sxs-lookup"><span data-stu-id="b0792-125">Configure hello logging product/machine tooforward hello required events toohello syslog daemon (rsyslog or syslog-ng) on hello agent machine.</span></span>
- <span data-ttu-id="b0792-126">Aktivera hello syslog-daemon på agenten datorn tooreceive hälsningsmeddelande från ett fjärrsystem.</span><span class="sxs-lookup"><span data-stu-id="b0792-126">Enable hello syslog daemon on hello agent machine tooreceive messages from a remote system.</span></span>

<span data-ttu-id="b0792-127">På hello agentdator måste hello händelser skickas från hello syslog-daemon toolocal UDP-port 25226 toobe.</span><span class="sxs-lookup"><span data-stu-id="b0792-127">On hello agent machine, hello events need toobe sent from hello syslog daemon toolocal UDP port 25226.</span></span> <span data-ttu-id="b0792-128">hello agenten lyssnar efter inkommande händelser på denna port.</span><span class="sxs-lookup"><span data-stu-id="b0792-128">hello agent is listening for incoming events on this port.</span></span> <span data-ttu-id="b0792-129">hello följande är ett exempel på en konfiguration för att skicka alla händelser från hello lokalt system toohello agent (du kan ändra hello configuration toofit dina lokala inställningar):</span><span class="sxs-lookup"><span data-stu-id="b0792-129">hello following is an example configuration for sending all events from hello local system toohello agent (you can modify hello configuration toofit your local settings):</span></span>

1. <span data-ttu-id="b0792-130">Öppna hello terminalfönster och gå toohello directory */etc/syslog-ng /*</span><span class="sxs-lookup"><span data-stu-id="b0792-130">Open hello terminal window, and go toohello directory */etc/syslog-ng/*</span></span> 
2. <span data-ttu-id="b0792-131">Skapa en ny fil *security-config-omsagent.conf* och Lägg till följande innehåll hello: OMS_facility = local4</span><span class="sxs-lookup"><span data-stu-id="b0792-131">Create a new file *security-config-omsagent.conf* and add hello following content:  OMS_facility = local4</span></span>
    
    <span data-ttu-id="b0792-132">filter f_local4_oms { facility(local4); };</span><span class="sxs-lookup"><span data-stu-id="b0792-132">filter f_local4_oms { facility(local4); };</span></span>

    <span data-ttu-id="b0792-133">destination security_oms { tcp("127.0.0.1" port(25226)); };</span><span class="sxs-lookup"><span data-stu-id="b0792-133">destination security_oms { tcp("127.0.0.1" port(25226)); };</span></span>

    <span data-ttu-id="b0792-134">log { source(src); filter(f_local4_oms); destination(security_oms); };</span><span class="sxs-lookup"><span data-stu-id="b0792-134">log { source(src); filter(f_local4_oms); destination(security_oms); };</span></span>
    
3. <span data-ttu-id="b0792-135">Hämta hello filen *security_events.conf* och placera i */etc/opt/microsoft/omsagent/conf/omsagent.d/* i hello OMS-Agent datorn.</span><span class="sxs-lookup"><span data-stu-id="b0792-135">Download hello file *security_events.conf* and place at */etc/opt/microsoft/omsagent/conf/omsagent.d/* in hello OMS Agent computer.</span></span>
4. <span data-ttu-id="b0792-136">Skriv hello-kommandot nedan toorestart hello syslog-daemon: *för syslog-ng kör:*</span><span class="sxs-lookup"><span data-stu-id="b0792-136">Type hello command below toorestart hello syslog daemon:  *For syslog-ng run:*</span></span>
    
    ```
    sudo service rsyslog restart
    ```

    <span data-ttu-id="b0792-137">*För rsyslog kör du:*</span><span class="sxs-lookup"><span data-stu-id="b0792-137">*For rsyslog run:*</span></span>
    
    ```
    /etc/init.d/syslog-ng restart
    ```
5. <span data-ttu-id="b0792-138">Skriv hello-kommandot nedan toorestart hello OMS-Agent:</span><span class="sxs-lookup"><span data-stu-id="b0792-138">Type hello command below toorestart hello OMS Agent:</span></span>

    <span data-ttu-id="b0792-139">*För syslog-ng kör du:*</span><span class="sxs-lookup"><span data-stu-id="b0792-139">*For syslog-ng run:*</span></span>
    
    ```
    sudo service omsagent restart
    ```

    <span data-ttu-id="b0792-140">*För rsyslog kör du:*</span><span class="sxs-lookup"><span data-stu-id="b0792-140">*For rsyslog run:*</span></span>
    
    ```
    systemctl restart omsagent
    ```
6. <span data-ttu-id="b0792-141">Ange hello kommandot nedan och granska hello resultatet tooconfirm att det inte finns några fel i loggen för hello OMS-Agent:</span><span class="sxs-lookup"><span data-stu-id="b0792-141">Type hello command below and review hello result tooconfirm that there are no errors in hello OMS Agent log:</span></span>

    ``` 
    tail /var/opt/microsoft/omsagent/log/omsagent.log
    ```

## <a name="reviewing-collected-security-events"></a><span data-ttu-id="b0792-142">Gå igenom de insamlade säkerhetshändelserna</span><span class="sxs-lookup"><span data-stu-id="b0792-142">Reviewing collected security events</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

<span data-ttu-id="b0792-143">När hello konfigurationen är klar, startar hello säkerhetshändelse toobe som inhämtas av OMS-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="b0792-143">After hello configuration is over, hello security event will start toobe ingested by OMS Security.</span></span> <span data-ttu-id="b0792-144">toovisualize dessa händelser, öppna hello loggen Sök skriver hello kommando *typ = CommonSecurityLog* i hello sökfältet och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="b0792-144">toovisualize those events, open hello Log Search, type hello command *Type=CommonSecurityLog* in hello search field and press ENTER.</span></span> <span data-ttu-id="b0792-145">hello följande exempel visar hello resultatet av kommandot, Lägg märke till att i det här fallet OMS säkerhet redan inhämtas säkerhetsloggar från flera leverantörer:</span><span class="sxs-lookup"><span data-stu-id="b0792-145">hello following example shows hello result of this command, notice that in this case OMS Security already ingested security logs from multiple vendors:</span></span>
   
![Utvärdering av säkerhetsbaslinjen i säkerhets- och granskningslösningen i OMS](./media/oms-security-connect-products/oms-security-connect-products-fig1.png)

<span data-ttu-id="b0792-147">Du kan förfina sökningen för en enda leverantör, till exempel toovisualize online Cisco loggar, typ: *typ = CommonSecurityLog DeviceVendor = Cisco*.</span><span class="sxs-lookup"><span data-stu-id="b0792-147">You can refine this search for one single vendor, for example, toovisualize online Cisco logs, type: *Type=CommonSecurityLog DeviceVendor=Cisco*.</span></span> <span data-ttu-id="b0792-148">Hej ”CommonSecurityLog” har fördefinierade fält för alla CEF-huvudet exempel hello grundläggande extensios, medan andra tillägg om den är ”tillägget för anpassat” eller inte, kommer att infogas i fältet ”AdditionalExtensions”.</span><span class="sxs-lookup"><span data-stu-id="b0792-148">hello “CommonSecurityLog” has predefined fields for any CEF header including hello basic extensios, while any other extension whether it’s “Custom Extension” or not, will be inserted into "AdditionalExtensions" field.</span></span> <span data-ttu-id="b0792-149">Du kan använda anpassade fält hello funktionen tooget särskilda fält från den.</span><span class="sxs-lookup"><span data-stu-id="b0792-149">You can use hello Custom Fields feature tooget dedicated fields from it.</span></span> 

### <a name="accessing-computers-missing-baseline-assessment"></a><span data-ttu-id="b0792-150">Kontrollera datorer utan utvärdering av säkerhetsbaslinje</span><span class="sxs-lookup"><span data-stu-id="b0792-150">Accessing computers missing baseline assessment</span></span>
<span data-ttu-id="b0792-151">OMS stöder hello domänprofilen medlem baslinje i Windows Server 2008 R2 upp tooWindows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="b0792-151">OMS supports hello domain member baseline profile on Windows Server 2008 R2 up tooWindows Server 2012 R2.</span></span> <span data-ttu-id="b0792-152">Baslinjen för Windows Server 2016 är inte klar än och läggs till så fort den publiceras.</span><span class="sxs-lookup"><span data-stu-id="b0792-152">Windows Server 2016 baseline isn’t final yet and will be added as soon as it is published.</span></span> <span data-ttu-id="b0792-153">Alla andra operativsystem som genomsöks via OMS säkerhet och granska baslinjen bedömning visas under hello **datorer som saknar baslinjen assessment** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b0792-153">All other operating systems scanned via OMS Security and Audit baseline assessment appear under hello **Computers missing baseline assessment** section.</span></span>

## <a name="see-also"></a><span data-ttu-id="b0792-154">Se även</span><span class="sxs-lookup"><span data-stu-id="b0792-154">See also</span></span>
<span data-ttu-id="b0792-155">I det här dokumentet du lärt dig hur tooconnect tooOMS din CEF-lösning.</span><span class="sxs-lookup"><span data-stu-id="b0792-155">In this document, you learned how tooconnect your CEF solution tooOMS.</span></span> <span data-ttu-id="b0792-156">toolearn mer om OMS-säkerhet finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="b0792-156">toolearn more about OMS Security, see hello following articles:</span></span>

* [<span data-ttu-id="b0792-157">Översikt över Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="b0792-157">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="b0792-158">Övervakning och svarar tooSecurity varningar i Operations Management Suite säkerhet och granska lösning</span><span class="sxs-lookup"><span data-stu-id="b0792-158">Monitoring and Responding tooSecurity Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="b0792-159">Övervaka resurser i säkerhets- och granskningslösningen i Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="b0792-159">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

