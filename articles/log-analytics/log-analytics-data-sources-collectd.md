---
title: "aaaCollect data från CollectD i OMS Log Analytics | Microsoft Docs"
description: "CollectD är en öppen källkod Linux-demonen som regelbundet samlar in data från program och information om systemet.  Den här artikeln innehåller information om att samla in data från CollectD i logganalys."
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
ms.date: 05/02/2017
ms.author: magoedte
ms.openlocfilehash: 7ad82c9c67a664aabd44f08bef2253d84cd2dfba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a><span data-ttu-id="085f5-104">Samla in data från CollectD på Linux-agenter i logganalys</span><span class="sxs-lookup"><span data-stu-id="085f5-104">Collect data from CollectD on Linux agents in Log Analytics</span></span>
<span data-ttu-id="085f5-105">[CollectD](https://collectd.org/) är en öppen källkod Linux-demonen som regelbundet samlar in prestandastatistik från program och information om systemet.</span><span class="sxs-lookup"><span data-stu-id="085f5-105">[CollectD](https://collectd.org/) is an open source Linux daemon that periodically collects performance metrics from applications and system level information.</span></span> <span data-ttu-id="085f5-106">Exempelprogram inkluderar hello Java Virtual Machine (JVM), MySQL-Server och Nginx.</span><span class="sxs-lookup"><span data-stu-id="085f5-106">Example applications include hello Java Virtual Machine (JVM), MySQL Server, and Nginx.</span></span> <span data-ttu-id="085f5-107">Den här artikeln innehåller information om att samla in prestandadata från CollectD i logganalys.</span><span class="sxs-lookup"><span data-stu-id="085f5-107">This article provides information on collecting performance data from CollectD in Log Analytics.</span></span>

<span data-ttu-id="085f5-108">En fullständig lista över tillgängliga plugin-program finns på [tabell av plugin-program](https://collectd.org/wiki/index.php/Table_of_Plugins).</span><span class="sxs-lookup"><span data-stu-id="085f5-108">A full list of available plugins can be found at [Table of Plugins](https://collectd.org/wiki/index.php/Table_of_Plugins).</span></span>

![Översikt över CollectD](media/log-analytics-data-sources-collectd/overview.png)

<span data-ttu-id="085f5-110">hello ingår följande CollectD konfiguration i hello OMS-Agent för Linux tooroute CollectD data toohello OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="085f5-110">hello following CollectD configuration is included in hello OMS Agent for Linux tooroute  CollectD data toohello OMS Agent for Linux.</span></span>

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

<span data-ttu-id="085f5-111">Dessutom, om du använder en versioner av collectD innan 5.5 använda hello följande konfiguration i stället.</span><span class="sxs-lookup"><span data-stu-id="085f5-111">Additionally, if using an versions of collectD before 5.5 use hello following configuration instead.</span></span>

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

<span data-ttu-id="085f5-112">Hej CollectD konfigurationen använder hello standard`write_http` plugin-programmet toosend mått prestandadata via port 26000 tooOMS Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="085f5-112">hello CollectD configuration uses hello default`write_http` plugin toosend performance metric data over port 26000 tooOMS Agent for Linux.</span></span> 

> [!NOTE]
> <span data-ttu-id="085f5-113">Den här porten kan vara konfigurerade tooa anpassade porten om det behövs.</span><span class="sxs-lookup"><span data-stu-id="085f5-113">This port can be configured tooa custom-defined port if needed.</span></span>

<span data-ttu-id="085f5-114">hello OMS-Agent för Linux också lyssnar på port 26000 CollectD mått och konverterar dem tooOMS schemat mått.</span><span class="sxs-lookup"><span data-stu-id="085f5-114">hello OMS Agent for Linux also listens on port 26000 for CollectD metrics and then converts them tooOMS schema metrics.</span></span> <span data-ttu-id="085f5-115">hello följer hello OMS-Agent för Linux-konfiguration `collectd.conf`.</span><span class="sxs-lookup"><span data-stu-id="085f5-115">hello following is hello OMS Agent for Linux configuration  `collectd.conf`.</span></span>

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a><span data-ttu-id="085f5-116">Versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="085f5-116">Versions supported</span></span>
- <span data-ttu-id="085f5-117">Logganalys stöder för närvarande CollectD version 4.8 och högre.</span><span class="sxs-lookup"><span data-stu-id="085f5-117">Log Analytics currently supports CollectD version 4.8 and above.</span></span>
- <span data-ttu-id="085f5-118">OMS-Agent för Linux v1.1.0-217 eller senare krävs för CollectD mått samling.</span><span class="sxs-lookup"><span data-stu-id="085f5-118">OMS Agent for Linux v1.1.0-217 or above is required for CollectD metric collection.</span></span>


## <a name="configuration"></a><span data-ttu-id="085f5-119">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="085f5-119">Configuration</span></span>
<span data-ttu-id="085f5-120">hello nedan följer stegen tooconfigure samling CollectD data i logganalys.</span><span class="sxs-lookup"><span data-stu-id="085f5-120">hello following are basic steps tooconfigure collection of CollectD data in Log Analytics.</span></span>

1. <span data-ttu-id="085f5-121">Konfigurera CollectD toosend data toohello OMS-Agent för Linux med hello write_http plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="085f5-121">Configure CollectD toosend data toohello OMS Agent for Linux using hello write_http plugin.</span></span>  
2. <span data-ttu-id="085f5-122">Konfigurera hello OMS-Agent för Linux toolisten för hello CollectD data på hello rätt port.</span><span class="sxs-lookup"><span data-stu-id="085f5-122">Configure hello OMS Agent for Linux toolisten for hello CollectD data on hello appropriate port.</span></span>
3. <span data-ttu-id="085f5-123">Starta om CollectD och OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="085f5-123">Restart CollectD and OMS Agent for Linux.</span></span>

### <a name="configure-collectd-tooforward-data"></a><span data-ttu-id="085f5-124">Konfigurera CollectD tooforward data</span><span class="sxs-lookup"><span data-stu-id="085f5-124">Configure CollectD tooforward data</span></span> 

1. <span data-ttu-id="085f5-125">tooroute CollectD data toohello OMS-Agent för Linux `oms.conf` behov toobe läggs Toocollectd's konfigurationskatalogen.</span><span class="sxs-lookup"><span data-stu-id="085f5-125">tooroute CollectD data toohello OMS Agent for Linux, `oms.conf` needs toobe added tooCollectD's configuration directory.</span></span> <span data-ttu-id="085f5-126">hello mål för den här filen är beroende av hello Linux distro av din dator.</span><span class="sxs-lookup"><span data-stu-id="085f5-126">hello destination of this file depends on hello Linux  distro of your machine.</span></span>

    <span data-ttu-id="085f5-127">Om din CollectD config directory finns i /etc/collectd.d/:</span><span class="sxs-lookup"><span data-stu-id="085f5-127">If your CollectD config directory is located in /etc/collectd.d/:</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    <span data-ttu-id="085f5-128">Om din CollectD config directory finns i /etc/collectd/collectd.conf.d/:</span><span class="sxs-lookup"><span data-stu-id="085f5-128">If your CollectD config directory is located in /etc/collectd/collectd.conf.d/:</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    ><span data-ttu-id="085f5-129">För CollectD versioner före 5.5 behöver toomodify hello taggar `oms.conf` som ovan.</span><span class="sxs-lookup"><span data-stu-id="085f5-129">For CollectD versions before 5.5 you will have toomodify hello tags in `oms.conf` as shown above.</span></span>
    >

2. <span data-ttu-id="085f5-130">Kopiera collectd.conf toohello önskad arbetsytan omsagent konfigurationskatalogen.</span><span class="sxs-lookup"><span data-stu-id="085f5-130">Copy collectd.conf toohello desired workspace's omsagent configuration directory.</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. <span data-ttu-id="085f5-131">Starta om CollectD och OMS-Agent för Linux med hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="085f5-131">Restart CollectD and OMS Agent for Linux with hello following commands.</span></span>

    <span data-ttu-id="085f5-132">sudo service collectd omstart sudo /opt/microsoft/omsagent/bin/service_control omstart</span><span class="sxs-lookup"><span data-stu-id="085f5-132">sudo service collectd restart  sudo /opt/microsoft/omsagent/bin/service_control restart</span></span>

## <a name="collectd-metrics-toolog-analytics-schema-conversion"></a><span data-ttu-id="085f5-133">CollectD mått tooLog Analytics schemat konvertering</span><span class="sxs-lookup"><span data-stu-id="085f5-133">CollectD metrics tooLog Analytics schema conversion</span></span>
<span data-ttu-id="085f5-134">toomaintain en bekant modell mellan infrastruktur-mätvärden som redan samlats in av OMS-Agent för Linux och hello nya mått som samlas in av CollectD hello följande schemamappning används:</span><span class="sxs-lookup"><span data-stu-id="085f5-134">toomaintain a familiar model between infrastructure metrics already collected by OMS Agent for Linux and hello new metrics collected by CollectD hello following schema mapping is used:</span></span>

| <span data-ttu-id="085f5-135">Fältet CollectD mått</span><span class="sxs-lookup"><span data-stu-id="085f5-135">CollectD Metric field</span></span> | <span data-ttu-id="085f5-136">Log Analytics-fält</span><span class="sxs-lookup"><span data-stu-id="085f5-136">Log Analytics field</span></span> |
|:--|:--|
| <span data-ttu-id="085f5-137">värden</span><span class="sxs-lookup"><span data-stu-id="085f5-137">host</span></span> | <span data-ttu-id="085f5-138">Dator</span><span class="sxs-lookup"><span data-stu-id="085f5-138">Computer</span></span> |
| <span data-ttu-id="085f5-139">plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="085f5-139">plugin</span></span> | <span data-ttu-id="085f5-140">Ingen</span><span class="sxs-lookup"><span data-stu-id="085f5-140">None</span></span> |
| <span data-ttu-id="085f5-141">plugin_instance</span><span class="sxs-lookup"><span data-stu-id="085f5-141">plugin_instance</span></span> | <span data-ttu-id="085f5-142">Instansnamn</span><span class="sxs-lookup"><span data-stu-id="085f5-142">Instance Name</span></span><br><span data-ttu-id="085f5-143">Om **plugin_instance** är *null* sedan InstanceName = ”*_Total*”</span><span class="sxs-lookup"><span data-stu-id="085f5-143">If **plugin_instance** is *null* then InstanceName="*_Total*"</span></span> |
| <span data-ttu-id="085f5-144">typ</span><span class="sxs-lookup"><span data-stu-id="085f5-144">type</span></span> | <span data-ttu-id="085f5-145">Objektnamn</span><span class="sxs-lookup"><span data-stu-id="085f5-145">ObjectName</span></span> |
| <span data-ttu-id="085f5-146">type_instance</span><span class="sxs-lookup"><span data-stu-id="085f5-146">type_instance</span></span> | <span data-ttu-id="085f5-147">CounterName</span><span class="sxs-lookup"><span data-stu-id="085f5-147">CounterName</span></span><br><span data-ttu-id="085f5-148">Om **type_instance** är *null* sedan CounterName =**tomt**</span><span class="sxs-lookup"><span data-stu-id="085f5-148">If **type_instance** is *null* then CounterName=**blank**</span></span> |
| <span data-ttu-id="085f5-149">dsnames]</span><span class="sxs-lookup"><span data-stu-id="085f5-149">dsnames[]</span></span> | <span data-ttu-id="085f5-150">CounterName</span><span class="sxs-lookup"><span data-stu-id="085f5-150">CounterName</span></span> |
| <span data-ttu-id="085f5-151">dstypes</span><span class="sxs-lookup"><span data-stu-id="085f5-151">dstypes</span></span> | <span data-ttu-id="085f5-152">Ingen</span><span class="sxs-lookup"><span data-stu-id="085f5-152">None</span></span> |
| <span data-ttu-id="085f5-153">[] värden</span><span class="sxs-lookup"><span data-stu-id="085f5-153">values[]</span></span> | <span data-ttu-id="085f5-154">CounterValue</span><span class="sxs-lookup"><span data-stu-id="085f5-154">CounterValue</span></span> |

## <a name="next-steps"></a><span data-ttu-id="085f5-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="085f5-155">Next steps</span></span>
* <span data-ttu-id="085f5-156">Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar.</span><span class="sxs-lookup"><span data-stu-id="085f5-156">Learn about [log searches](log-analytics-log-searches.md) tooanalyze hello data collected from data sources and solutions.</span></span> 
* <span data-ttu-id="085f5-157">Använd [anpassade fält](log-analytics-custom-fields.md) tooparse data från syslog-poster till enskilda fält.</span><span class="sxs-lookup"><span data-stu-id="085f5-157">Use [Custom Fields](log-analytics-custom-fields.md) tooparse data from syslog records into individual fields.</span></span>

