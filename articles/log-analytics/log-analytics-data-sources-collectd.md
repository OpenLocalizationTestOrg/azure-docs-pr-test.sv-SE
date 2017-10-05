---
title: "Samla in data från CollectD i OMS Log Analytics | Microsoft Docs"
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
ms.openlocfilehash: a63b15ca5126b45451f0694c9ee75d7b67b1ceaf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a><span data-ttu-id="ed271-104">Samla in data från CollectD på Linux-agenter i logganalys</span><span class="sxs-lookup"><span data-stu-id="ed271-104">Collect data from CollectD on Linux agents in Log Analytics</span></span>
<span data-ttu-id="ed271-105">[CollectD](https://collectd.org/) är en öppen källkod Linux-demonen som regelbundet samlar in prestandastatistik från program och information om systemet.</span><span class="sxs-lookup"><span data-stu-id="ed271-105">[CollectD](https://collectd.org/) is an open source Linux daemon that periodically collects performance metrics from applications and system level information.</span></span> <span data-ttu-id="ed271-106">Exempelprogram inkluderar Java Virtual Machine (JVM), MySQL-servern och Nginx.</span><span class="sxs-lookup"><span data-stu-id="ed271-106">Example applications include the Java Virtual Machine (JVM), MySQL Server, and Nginx.</span></span> <span data-ttu-id="ed271-107">Den här artikeln innehåller information om att samla in prestandadata från CollectD i logganalys.</span><span class="sxs-lookup"><span data-stu-id="ed271-107">This article provides information on collecting performance data from CollectD in Log Analytics.</span></span>

<span data-ttu-id="ed271-108">En fullständig lista över tillgängliga plugin-program finns på [tabell av plugin-program](https://collectd.org/wiki/index.php/Table_of_Plugins).</span><span class="sxs-lookup"><span data-stu-id="ed271-108">A full list of available plugins can be found at [Table of Plugins](https://collectd.org/wiki/index.php/Table_of_Plugins).</span></span>

![Översikt över CollectD](media/log-analytics-data-sources-collectd/overview.png)

<span data-ttu-id="ed271-110">Konfigurera följande CollectD ingår i OMS-Agent för Linux att vidarebefordra CollectD data till OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="ed271-110">The following CollectD configuration is included in the OMS Agent for Linux to route  CollectD data to the OMS Agent for Linux.</span></span>

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

<span data-ttu-id="ed271-111">Dessutom, om du använder en versioner av collectD innan 5.5 använder följande konfiguration i stället.</span><span class="sxs-lookup"><span data-stu-id="ed271-111">Additionally, if using an versions of collectD before 5.5 use the following configuration instead.</span></span>

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

<span data-ttu-id="ed271-112">CollectD konfigurationen använder standard`write_http` plugin-programmet för att skicka mått prestandadata via port 26000 till OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="ed271-112">The CollectD configuration uses the default`write_http` plugin to send performance metric data over port 26000 to OMS Agent for Linux.</span></span> 

> [!NOTE]
> <span data-ttu-id="ed271-113">Den här porten kan konfigureras att ett egendefinierat porten om det behövs.</span><span class="sxs-lookup"><span data-stu-id="ed271-113">This port can be configured to a custom-defined port if needed.</span></span>

<span data-ttu-id="ed271-114">OMS-Agent för Linux också lyssnar på port 26000 CollectD mått och konverterar dem till OMS-schemat statistik.</span><span class="sxs-lookup"><span data-stu-id="ed271-114">The OMS Agent for Linux also listens on port 26000 for CollectD metrics and then converts them to OMS schema metrics.</span></span> <span data-ttu-id="ed271-115">Följande är OMS-Agent för Linux-konfiguration `collectd.conf`.</span><span class="sxs-lookup"><span data-stu-id="ed271-115">The following is the OMS Agent for Linux configuration  `collectd.conf`.</span></span>

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a><span data-ttu-id="ed271-116">Versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="ed271-116">Versions supported</span></span>
- <span data-ttu-id="ed271-117">Logganalys stöder för närvarande CollectD version 4.8 och högre.</span><span class="sxs-lookup"><span data-stu-id="ed271-117">Log Analytics currently supports CollectD version 4.8 and above.</span></span>
- <span data-ttu-id="ed271-118">OMS-Agent för Linux v1.1.0-217 eller senare krävs för CollectD mått samling.</span><span class="sxs-lookup"><span data-stu-id="ed271-118">OMS Agent for Linux v1.1.0-217 or above is required for CollectD metric collection.</span></span>


## <a name="configuration"></a><span data-ttu-id="ed271-119">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="ed271-119">Configuration</span></span>
<span data-ttu-id="ed271-120">Här följer grundläggande steg för att konfigurera insamling av data för CollectD i logganalys.</span><span class="sxs-lookup"><span data-stu-id="ed271-120">The following are basic steps to configure collection of CollectD data in Log Analytics.</span></span>

1. <span data-ttu-id="ed271-121">Konfigurera CollectD för att skicka data till OMS-Agent för Linux med write_http plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="ed271-121">Configure CollectD to send data to the OMS Agent for Linux using the write_http plugin.</span></span>  
2. <span data-ttu-id="ed271-122">Konfigurera OMS-Agent för Linux att lyssna efter CollectD data på rätt port.</span><span class="sxs-lookup"><span data-stu-id="ed271-122">Configure the OMS Agent for Linux to listen for the CollectD data on the appropriate port.</span></span>
3. <span data-ttu-id="ed271-123">Starta om CollectD och OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="ed271-123">Restart CollectD and OMS Agent for Linux.</span></span>

### <a name="configure-collectd-to-forward-data"></a><span data-ttu-id="ed271-124">Konfigurera CollectD för att vidarebefordra data</span><span class="sxs-lookup"><span data-stu-id="ed271-124">Configure CollectD to forward data</span></span> 

1. <span data-ttu-id="ed271-125">Att vidarebefordra CollectD data till OMS-Agent för Linux `oms.conf` måste läggas till Collectds konfigurationskatalogen.</span><span class="sxs-lookup"><span data-stu-id="ed271-125">To route CollectD data to the OMS Agent for Linux, `oms.conf` needs to be added to CollectD's configuration directory.</span></span> <span data-ttu-id="ed271-126">Mål för den här filen är beroende av Linux-distro av din dator.</span><span class="sxs-lookup"><span data-stu-id="ed271-126">The destination of this file depends on the Linux  distro of your machine.</span></span>

    <span data-ttu-id="ed271-127">Om din CollectD config directory finns i /etc/collectd.d/:</span><span class="sxs-lookup"><span data-stu-id="ed271-127">If your CollectD config directory is located in /etc/collectd.d/:</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    <span data-ttu-id="ed271-128">Om din CollectD config directory finns i /etc/collectd/collectd.conf.d/:</span><span class="sxs-lookup"><span data-stu-id="ed271-128">If your CollectD config directory is located in /etc/collectd/collectd.conf.d/:</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    ><span data-ttu-id="ed271-129">Du behöver ändra taggar i för CollectD versioner före 5.5 `oms.conf` som ovan.</span><span class="sxs-lookup"><span data-stu-id="ed271-129">For CollectD versions before 5.5 you will have to modify the tags in `oms.conf` as shown above.</span></span>
    >

2. <span data-ttu-id="ed271-130">Kopiera collectd.conf till arbetsytan önskade omsagent konfigurationskatalogen.</span><span class="sxs-lookup"><span data-stu-id="ed271-130">Copy collectd.conf to the desired workspace's omsagent configuration directory.</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. <span data-ttu-id="ed271-131">Starta om CollectD och OMS-Agent för Linux med följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="ed271-131">Restart CollectD and OMS Agent for Linux with the following commands.</span></span>

    <span data-ttu-id="ed271-132">sudo service collectd omstart sudo /opt/microsoft/omsagent/bin/service_control omstart</span><span class="sxs-lookup"><span data-stu-id="ed271-132">sudo service collectd restart  sudo /opt/microsoft/omsagent/bin/service_control restart</span></span>

## <a name="collectd-metrics-to-log-analytics-schema-conversion"></a><span data-ttu-id="ed271-133">CollectD mått till logganalys schemat konvertering</span><span class="sxs-lookup"><span data-stu-id="ed271-133">CollectD metrics to Log Analytics schema conversion</span></span>
<span data-ttu-id="ed271-134">Om du vill behålla en bekant modell mellan infrastruktur-mätvärden som redan samlats in av OMS-Agent för Linux och nya mått som samlas in av CollectD följande schemamappning används:</span><span class="sxs-lookup"><span data-stu-id="ed271-134">To maintain a familiar model between infrastructure metrics already collected by OMS Agent for Linux and the new metrics collected by CollectD the following schema mapping is used:</span></span>

| <span data-ttu-id="ed271-135">Fältet CollectD mått</span><span class="sxs-lookup"><span data-stu-id="ed271-135">CollectD Metric field</span></span> | <span data-ttu-id="ed271-136">Log Analytics-fält</span><span class="sxs-lookup"><span data-stu-id="ed271-136">Log Analytics field</span></span> |
|:--|:--|
| <span data-ttu-id="ed271-137">värden</span><span class="sxs-lookup"><span data-stu-id="ed271-137">host</span></span> | <span data-ttu-id="ed271-138">Dator</span><span class="sxs-lookup"><span data-stu-id="ed271-138">Computer</span></span> |
| <span data-ttu-id="ed271-139">plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="ed271-139">plugin</span></span> | <span data-ttu-id="ed271-140">Ingen</span><span class="sxs-lookup"><span data-stu-id="ed271-140">None</span></span> |
| <span data-ttu-id="ed271-141">plugin_instance</span><span class="sxs-lookup"><span data-stu-id="ed271-141">plugin_instance</span></span> | <span data-ttu-id="ed271-142">Instansnamn</span><span class="sxs-lookup"><span data-stu-id="ed271-142">Instance Name</span></span><br><span data-ttu-id="ed271-143">Om **plugin_instance** är *null* sedan InstanceName = ”*_Total*”</span><span class="sxs-lookup"><span data-stu-id="ed271-143">If **plugin_instance** is *null* then InstanceName="*_Total*"</span></span> |
| <span data-ttu-id="ed271-144">typ</span><span class="sxs-lookup"><span data-stu-id="ed271-144">type</span></span> | <span data-ttu-id="ed271-145">Objektnamn</span><span class="sxs-lookup"><span data-stu-id="ed271-145">ObjectName</span></span> |
| <span data-ttu-id="ed271-146">type_instance</span><span class="sxs-lookup"><span data-stu-id="ed271-146">type_instance</span></span> | <span data-ttu-id="ed271-147">CounterName</span><span class="sxs-lookup"><span data-stu-id="ed271-147">CounterName</span></span><br><span data-ttu-id="ed271-148">Om **type_instance** är *null* sedan CounterName =**tomt**</span><span class="sxs-lookup"><span data-stu-id="ed271-148">If **type_instance** is *null* then CounterName=**blank**</span></span> |
| <span data-ttu-id="ed271-149">dsnames]</span><span class="sxs-lookup"><span data-stu-id="ed271-149">dsnames[]</span></span> | <span data-ttu-id="ed271-150">CounterName</span><span class="sxs-lookup"><span data-stu-id="ed271-150">CounterName</span></span> |
| <span data-ttu-id="ed271-151">dstypes</span><span class="sxs-lookup"><span data-stu-id="ed271-151">dstypes</span></span> | <span data-ttu-id="ed271-152">Ingen</span><span class="sxs-lookup"><span data-stu-id="ed271-152">None</span></span> |
| <span data-ttu-id="ed271-153">[] värden</span><span class="sxs-lookup"><span data-stu-id="ed271-153">values[]</span></span> | <span data-ttu-id="ed271-154">CounterValue</span><span class="sxs-lookup"><span data-stu-id="ed271-154">CounterValue</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ed271-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ed271-155">Next steps</span></span>
* <span data-ttu-id="ed271-156">Lär dig mer om [logga sökningar](log-analytics-log-searches.md) att analysera data som samlas in från datakällor och lösningar.</span><span class="sxs-lookup"><span data-stu-id="ed271-156">Learn about [log searches](log-analytics-log-searches.md) to analyze the data collected from data sources and solutions.</span></span> 
* <span data-ttu-id="ed271-157">Använd [anpassade fält](log-analytics-custom-fields.md) att tolka data från syslog-poster till enskilda fält.</span><span class="sxs-lookup"><span data-stu-id="ed271-157">Use [Custom Fields](log-analytics-custom-fields.md) to parse data from syslog records into individual fields.</span></span>

