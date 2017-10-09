---
title: aaaCollect Nagios och Zabbix aviseringar i OMS Log Analytics | Microsoft Docs
description: "Nagios och Zabbix är öppen källkod övervakningsverktyg. Du kan samla in varningar från dessa verktyg i logganalys i ordning tooanalyze dem tillsammans med aviseringar från andra källor.  Den här artikeln beskriver hur tooconfigure hello OMS-Agent för Linux toocollect varningar från dessa system."
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
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 23e2252e4fed8bc87baec063694a8472ca84220d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-oms-agent-for-linux"></a><span data-ttu-id="aae10-105">Samla in aviseringar från Nagios och Zabbix i logganalys från OMS-Agent för Linux</span><span class="sxs-lookup"><span data-stu-id="aae10-105">Collect alerts from Nagios and Zabbix in Log Analytics from OMS Agent for Linux</span></span> 
<span data-ttu-id="aae10-106">[Nagios](https://www.nagios.org/) och [Zabbix](http://www.zabbix.com/) är öppen källkod övervakningsverktyg.</span><span class="sxs-lookup"><span data-stu-id="aae10-106">[Nagios](https://www.nagios.org/) and [Zabbix](http://www.zabbix.com/) are open source monitoring tools.</span></span>  <span data-ttu-id="aae10-107">Du kan samla in varningar från dessa verktyg i logganalys i ordning tooanalyze dem tillsammans med [aviseringar från andra källor](log-analytics-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="aae10-107">You can collect alerts from these tools into Log Analytics in order tooanalyze them along with [alerts from other sources](log-analytics-alerts.md).</span></span>  <span data-ttu-id="aae10-108">Den här artikeln beskriver hur tooconfigure hello OMS-Agent för Linux toocollect varningar från dessa system.</span><span class="sxs-lookup"><span data-stu-id="aae10-108">This article describes how tooconfigure hello OMS Agent for Linux toocollect alerts from these systems.</span></span>
 
## <a name="configure-alert-collection"></a><span data-ttu-id="aae10-109">Konfigurera varningssamlingen</span><span class="sxs-lookup"><span data-stu-id="aae10-109">Configure alert collection</span></span>

### <a name="configuring-nagios-alert-collection"></a><span data-ttu-id="aae10-110">Konfigurera Nagios varningssamlingen</span><span class="sxs-lookup"><span data-stu-id="aae10-110">Configuring Nagios alert collection</span></span>
<span data-ttu-id="aae10-111">Utföra hello följande hello Nagios server toocollect aviseringar för.</span><span class="sxs-lookup"><span data-stu-id="aae10-111">Perform hello following steps on hello Nagios server toocollect alerts.</span></span>

1. <span data-ttu-id="aae10-112">Bevilja hello användaren **omsagent** läsbehörighet toohello Nagios-loggfilen (d.v.s. `/var/log/nagios/nagios.log`).</span><span class="sxs-lookup"><span data-stu-id="aae10-112">Grant hello user **omsagent** read access toohello Nagios log file (i.e. `/var/log/nagios/nagios.log`).</span></span> <span data-ttu-id="aae10-113">Under förutsättning att hello nagios.log filen ägs av hello grupp `nagios`, du kan lägga till användaren hello **omsagent** toohello **nagios** grupp.</span><span class="sxs-lookup"><span data-stu-id="aae10-113">Assuming hello nagios.log file is owned by hello group `nagios`, you can add hello user **omsagent** toohello **nagios** group.</span></span> 

    <span data-ttu-id="aae10-114">sudo usermod - a -G nagios omsagent</span><span class="sxs-lookup"><span data-stu-id="aae10-114">sudo usermod -a -G nagios omsagent</span></span>

2.  <span data-ttu-id="aae10-115">Ändra hello-konfigurationsfilen vid (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span><span class="sxs-lookup"><span data-stu-id="aae10-115">Modify hello configuration file at (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span></span> <span data-ttu-id="aae10-116">Kontrollera att följande poster hello finns och inte kommenterad ut:</span><span class="sxs-lookup"><span data-stu-id="aae10-116">Ensure hello following entries are present and not commented out:</span></span>  

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

3. <span data-ttu-id="aae10-117">Starta om hello omsagent daemon</span><span class="sxs-lookup"><span data-stu-id="aae10-117">Restart hello omsagent daemon</span></span>

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a><span data-ttu-id="aae10-118">Konfigurera Zabbix varningssamlingen</span><span class="sxs-lookup"><span data-stu-id="aae10-118">Configuring Zabbix alert collection</span></span>
<span data-ttu-id="aae10-119">toocollect aviseringar från en Zabbix-server, behöver du toospecify en användare och lösenord i *klartext*.</span><span class="sxs-lookup"><span data-stu-id="aae10-119">toocollect alerts from a Zabbix server, you need toospecify a user and password in *clear text*.</span></span> <span data-ttu-id="aae10-120">Detta är inte idealiskt, men vi rekommenderar att du skapar hello användare och bevilja behörigheter toomonitor onlu.</span><span class="sxs-lookup"><span data-stu-id="aae10-120">This is not ideal, but we recommend that you create hello user and grant permissions toomonitor onlu.</span></span>

<span data-ttu-id="aae10-121">Utföra hello följande hello Nagios server toocollect aviseringar för.</span><span class="sxs-lookup"><span data-stu-id="aae10-121">Perform hello following steps on hello Nagios server toocollect alerts.</span></span>

1. <span data-ttu-id="aae10-122">Ändra hello-konfigurationsfilen vid (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span><span class="sxs-lookup"><span data-stu-id="aae10-122">Modify hello configuration file at (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span></span> <span data-ttu-id="aae10-123">Kontrollera att följande poster hello finns och inte kommenterad ut.  Ändra hello användarens namn och lösenord toovalues för Zabbix-miljö.</span><span class="sxs-lookup"><span data-stu-id="aae10-123">Ensure hello following entries are present and not commented out.  Change hello user name and password toovalues for your Zabbix environment.</span></span>

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. <span data-ttu-id="aae10-124">Starta om hello omsagent daemon</span><span class="sxs-lookup"><span data-stu-id="aae10-124">Restart hello omsagent daemon</span></span>

    <span data-ttu-id="aae10-125">sudo del /opt/microsoft/omsagent/bin/service_control starta om</span><span class="sxs-lookup"><span data-stu-id="aae10-125">sudo sh /opt/microsoft/omsagent/bin/service_control restart</span></span>


## <a name="alert-records"></a><span data-ttu-id="aae10-126">Varning-poster</span><span class="sxs-lookup"><span data-stu-id="aae10-126">Alert records</span></span>
<span data-ttu-id="aae10-127">Du kan hämta avisering poster från Nagios och Zabbix med [logga sökningar](log-analytics-log-searches.md) i logganalys.</span><span class="sxs-lookup"><span data-stu-id="aae10-127">You can retrieve alert records from Nagios and Zabbix using [log searches](log-analytics-log-searches.md) in Log Analytics.</span></span>

### <a name="nagios-alert-records"></a><span data-ttu-id="aae10-128">Avisering för Nagios-poster</span><span class="sxs-lookup"><span data-stu-id="aae10-128">Nagios Alert records</span></span>

<span data-ttu-id="aae10-129">Varna poster som samlas in av Nagios har en **typen** av **avisering** och en **SourceSystem** av **Nagios**.</span><span class="sxs-lookup"><span data-stu-id="aae10-129">Alert records collected by Nagios have a **Type** of **Alert** and a **SourceSystem** of **Nagios**.</span></span>  <span data-ttu-id="aae10-130">De har hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="aae10-130">They have hello properties in hello following table.</span></span>

| <span data-ttu-id="aae10-131">Egenskap</span><span class="sxs-lookup"><span data-stu-id="aae10-131">Property</span></span> | <span data-ttu-id="aae10-132">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="aae10-132">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="aae10-133">Typ</span><span class="sxs-lookup"><span data-stu-id="aae10-133">Type</span></span> |<span data-ttu-id="aae10-134">*Varning*</span><span class="sxs-lookup"><span data-stu-id="aae10-134">*Alert*</span></span> |
| <span data-ttu-id="aae10-135">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="aae10-135">SourceSystem</span></span> |<span data-ttu-id="aae10-136">*Nagios*</span><span class="sxs-lookup"><span data-stu-id="aae10-136">*Nagios*</span></span> |
| <span data-ttu-id="aae10-137">AlertName</span><span class="sxs-lookup"><span data-stu-id="aae10-137">AlertName</span></span> |<span data-ttu-id="aae10-138">Namnet på hello avisering.</span><span class="sxs-lookup"><span data-stu-id="aae10-138">Name of hello alert.</span></span> |
| <span data-ttu-id="aae10-139">AlertDescription</span><span class="sxs-lookup"><span data-stu-id="aae10-139">AlertDescription</span></span> | <span data-ttu-id="aae10-140">Beskrivning av hello avisering.</span><span class="sxs-lookup"><span data-stu-id="aae10-140">Description of hello alert.</span></span> |
| <span data-ttu-id="aae10-141">In AlertState</span><span class="sxs-lookup"><span data-stu-id="aae10-141">AlertState</span></span> | <span data-ttu-id="aae10-142">Status för tjänsten hello eller värden.</span><span class="sxs-lookup"><span data-stu-id="aae10-142">Status of hello service or host.</span></span><br><br><span data-ttu-id="aae10-143">OKEJ</span><span class="sxs-lookup"><span data-stu-id="aae10-143">OK</span></span><br><span data-ttu-id="aae10-144">VARNING</span><span class="sxs-lookup"><span data-stu-id="aae10-144">WARNING</span></span><br><span data-ttu-id="aae10-145">KONFIGURERA</span><span class="sxs-lookup"><span data-stu-id="aae10-145">UP</span></span><br><span data-ttu-id="aae10-146">NED</span><span class="sxs-lookup"><span data-stu-id="aae10-146">DOWN</span></span> |
| <span data-ttu-id="aae10-147">Värdnamn</span><span class="sxs-lookup"><span data-stu-id="aae10-147">HostName</span></span> | <span data-ttu-id="aae10-148">Namnet på hello-värden som skapade hello avisering.</span><span class="sxs-lookup"><span data-stu-id="aae10-148">Name of hello host that created hello alert.</span></span> |
| <span data-ttu-id="aae10-149">PriorityNumber</span><span class="sxs-lookup"><span data-stu-id="aae10-149">PriorityNumber</span></span> | <span data-ttu-id="aae10-150">Prioritetsnivån för hello avisering.</span><span class="sxs-lookup"><span data-stu-id="aae10-150">Priority level of hello alert.</span></span> |
| <span data-ttu-id="aae10-151">StateType</span><span class="sxs-lookup"><span data-stu-id="aae10-151">StateType</span></span> | <span data-ttu-id="aae10-152">hello typ av tillståndet för hello avisering.</span><span class="sxs-lookup"><span data-stu-id="aae10-152">hello type of state of hello alert.</span></span><br><br><span data-ttu-id="aae10-153">SOFT - problem som inte kontrolleras igen.</span><span class="sxs-lookup"><span data-stu-id="aae10-153">SOFT - Issue that has not been rechecked.</span></span><br><span data-ttu-id="aae10-154">HÅRDDISK - problem som har ett angivet antal gånger begäran igen.</span><span class="sxs-lookup"><span data-stu-id="aae10-154">HARD - Issue that has been rechecked a specified number of times.</span></span>  |
| <span data-ttu-id="aae10-155">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="aae10-155">TimeGenerated</span></span> |<span data-ttu-id="aae10-156">Datum och tid hello aviseringen skapades.</span><span class="sxs-lookup"><span data-stu-id="aae10-156">Date and time hello alert was created.</span></span> |


### <a name="zabbix-alert-records"></a><span data-ttu-id="aae10-157">Varning Zabbix-poster</span><span class="sxs-lookup"><span data-stu-id="aae10-157">Zabbix alert records</span></span>
<span data-ttu-id="aae10-158">Varna poster som samlas in av Zabbix har en **typen** av **avisering** och en **SourceSystem** av **Zabbix**.</span><span class="sxs-lookup"><span data-stu-id="aae10-158">Alert records collected by Zabbix have a **Type** of **Alert** and a **SourceSystem** of **Zabbix**.</span></span>  <span data-ttu-id="aae10-159">De har hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="aae10-159">They have hello properties in hello following table.</span></span>

| <span data-ttu-id="aae10-160">Egenskap</span><span class="sxs-lookup"><span data-stu-id="aae10-160">Property</span></span> | <span data-ttu-id="aae10-161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="aae10-161">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="aae10-162">Typ</span><span class="sxs-lookup"><span data-stu-id="aae10-162">Type</span></span> |<span data-ttu-id="aae10-163">*Varning*</span><span class="sxs-lookup"><span data-stu-id="aae10-163">*Alert*</span></span> |
| <span data-ttu-id="aae10-164">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="aae10-164">SourceSystem</span></span> |<span data-ttu-id="aae10-165">*Zabbix*</span><span class="sxs-lookup"><span data-stu-id="aae10-165">*Zabbix*</span></span> |
| <span data-ttu-id="aae10-166">AlertName</span><span class="sxs-lookup"><span data-stu-id="aae10-166">AlertName</span></span> | <span data-ttu-id="aae10-167">Namnet på hello avisering.</span><span class="sxs-lookup"><span data-stu-id="aae10-167">Name of hello alert.</span></span> |
| <span data-ttu-id="aae10-168">AlertPriority</span><span class="sxs-lookup"><span data-stu-id="aae10-168">AlertPriority</span></span> | <span data-ttu-id="aae10-169">Allvarlighetsgraden hello avisering.</span><span class="sxs-lookup"><span data-stu-id="aae10-169">Severity of hello alert.</span></span><br><br><span data-ttu-id="aae10-170">inte har klassificerats</span><span class="sxs-lookup"><span data-stu-id="aae10-170">not classified</span></span><br><span data-ttu-id="aae10-171">Information</span><span class="sxs-lookup"><span data-stu-id="aae10-171">information</span></span><br><span data-ttu-id="aae10-172">Varning</span><span class="sxs-lookup"><span data-stu-id="aae10-172">warning</span></span><br><span data-ttu-id="aae10-173">Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="aae10-173">average</span></span><br><span data-ttu-id="aae10-174">Hög</span><span class="sxs-lookup"><span data-stu-id="aae10-174">high</span></span><br><span data-ttu-id="aae10-175">katastrofåterställning</span><span class="sxs-lookup"><span data-stu-id="aae10-175">disaster</span></span>  |
| <span data-ttu-id="aae10-176">In AlertState</span><span class="sxs-lookup"><span data-stu-id="aae10-176">AlertState</span></span> | <span data-ttu-id="aae10-177">Tillståndet för hello avisering.</span><span class="sxs-lookup"><span data-stu-id="aae10-177">State of hello alert.</span></span><br><br><span data-ttu-id="aae10-178">0 - tillståndet är upp toodate.</span><span class="sxs-lookup"><span data-stu-id="aae10-178">0 - State is up toodate.</span></span><br><span data-ttu-id="aae10-179">1 - tillståndet är okänt.</span><span class="sxs-lookup"><span data-stu-id="aae10-179">1 - State is unknown.</span></span>  |
| <span data-ttu-id="aae10-180">AlertTypeNumber</span><span class="sxs-lookup"><span data-stu-id="aae10-180">AlertTypeNumber</span></span> | <span data-ttu-id="aae10-181">Anger om avisering kan skapa flera problem händelser.</span><span class="sxs-lookup"><span data-stu-id="aae10-181">Specifies whether alert can generate multiple problem events.</span></span><br><br><span data-ttu-id="aae10-182">0 - tillståndet är upp toodate.</span><span class="sxs-lookup"><span data-stu-id="aae10-182">0 - State is up toodate.</span></span><br><span data-ttu-id="aae10-183">1 - tillståndet är okänt.</span><span class="sxs-lookup"><span data-stu-id="aae10-183">1 - State is unknown.</span></span>    |
| <span data-ttu-id="aae10-184">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="aae10-184">Comments</span></span> | <span data-ttu-id="aae10-185">Ytterligare kommentarer för aviseringen.</span><span class="sxs-lookup"><span data-stu-id="aae10-185">Additional comments for alert.</span></span> |
| <span data-ttu-id="aae10-186">Värdnamn</span><span class="sxs-lookup"><span data-stu-id="aae10-186">HostName</span></span> | <span data-ttu-id="aae10-187">Namnet på hello-värden som skapade hello avisering.</span><span class="sxs-lookup"><span data-stu-id="aae10-187">Name of hello host that created hello alert.</span></span> |
| <span data-ttu-id="aae10-188">PriorityNumber</span><span class="sxs-lookup"><span data-stu-id="aae10-188">PriorityNumber</span></span> | <span data-ttu-id="aae10-189">Värdet anger allvarlighetsgraden hello avisering.</span><span class="sxs-lookup"><span data-stu-id="aae10-189">Value indicating severity of hello alert.</span></span><br><br><span data-ttu-id="aae10-190">0 - inte har klassificerats</span><span class="sxs-lookup"><span data-stu-id="aae10-190">0 - not classified</span></span><br><span data-ttu-id="aae10-191">1 - information</span><span class="sxs-lookup"><span data-stu-id="aae10-191">1 - information</span></span><br><span data-ttu-id="aae10-192">2 - varning</span><span class="sxs-lookup"><span data-stu-id="aae10-192">2 - warning</span></span><br><span data-ttu-id="aae10-193">3 - medel</span><span class="sxs-lookup"><span data-stu-id="aae10-193">3 - average</span></span><br><span data-ttu-id="aae10-194">4 - hög</span><span class="sxs-lookup"><span data-stu-id="aae10-194">4 - high</span></span><br><span data-ttu-id="aae10-195">5 - katastrofåterställning</span><span class="sxs-lookup"><span data-stu-id="aae10-195">5 - disaster</span></span> |
| <span data-ttu-id="aae10-196">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="aae10-196">TimeGenerated</span></span> |<span data-ttu-id="aae10-197">Datum och tid hello aviseringen skapades.</span><span class="sxs-lookup"><span data-stu-id="aae10-197">Date and time hello alert was created.</span></span> |
| <span data-ttu-id="aae10-198">TimeLastModified</span><span class="sxs-lookup"><span data-stu-id="aae10-198">TimeLastModified</span></span> |<span data-ttu-id="aae10-199">Datum och tid hello tillstånd hello aviseringen senast ändrades.</span><span class="sxs-lookup"><span data-stu-id="aae10-199">Date and time hello state of hello alert was last changed.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="aae10-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aae10-200">Next steps</span></span>
* <span data-ttu-id="aae10-201">Lär dig mer om [aviseringar](log-analytics-alerts.md) i logganalys.</span><span class="sxs-lookup"><span data-stu-id="aae10-201">Learn about [alerts](log-analytics-alerts.md) in Log Analytics.</span></span>
* <span data-ttu-id="aae10-202">Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar.</span><span class="sxs-lookup"><span data-stu-id="aae10-202">Learn about [log searches](log-analytics-log-searches.md) tooanalyze hello data collected from data sources and solutions.</span></span> 
