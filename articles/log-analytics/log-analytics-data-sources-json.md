---
title: aaaCollecting anpassad JSON-data i OMS Log Analytics | Microsoft Docs
description: "Anpassad JSON-datakällor kan samlas in Log Analytics med hjälp av hello OMS-Agent för Linux.  Dessa anpassade datakällor kan vara enkla skript som returnerar JSON som curl eller något av Fluentd's 300 + plugin-program. Den här artikeln beskriver hello-konfiguration som krävs för den här Datasamlingen."
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
ms.openlocfilehash: 97d401408a8c206d4a9ef2ec9b13ba1ca6b5e92b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collecting-custom-json-data-sources-with-hello-oms-agent-for-linux-in-log-analytics"></a><span data-ttu-id="e0db8-105">Samla in anpassad JSON-datakällor med hello OMS-Agent för Linux i logganalys</span><span class="sxs-lookup"><span data-stu-id="e0db8-105">Collecting custom JSON data sources with hello OMS Agent for Linux in Log Analytics</span></span>
<span data-ttu-id="e0db8-106">Anpassad JSON-datakällor kan samlas in Log Analytics med hjälp av hello OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="e0db8-106">Custom JSON data sources can be collected into Log Analytics using hello OMS Agent for Linux.</span></span>  <span data-ttu-id="e0db8-107">Dessa anpassade datakällor kan vara enkla skript som returnerar JSON [curl](https://curl.haxx.se/) eller någon av [Fluentd's 300 + plugin-program](http://www.fluentd.org/plugins/all).</span><span class="sxs-lookup"><span data-stu-id="e0db8-107">These custom data sources can be simple scripts returning JSON such as [curl](https://curl.haxx.se/) or one of [FluentD's 300+ plugins](http://www.fluentd.org/plugins/all).</span></span> <span data-ttu-id="e0db8-108">Den här artikeln beskriver hello-konfiguration som krävs för den här Datasamlingen.</span><span class="sxs-lookup"><span data-stu-id="e0db8-108">This article describes hello configuration required for this data collection.</span></span>

> [!NOTE]
> <span data-ttu-id="e0db8-109">OMS-Agent för Linux v1.1.0-217 + krävs för anpassad JSON-Data</span><span class="sxs-lookup"><span data-stu-id="e0db8-109">OMS Agent for Linux v1.1.0-217+ is required for Custom JSON Data</span></span>

## <a name="configuration"></a><span data-ttu-id="e0db8-110">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="e0db8-110">Configuration</span></span>

### <a name="configure-input-plugin"></a><span data-ttu-id="e0db8-111">Konfigurera plugin-programmet för indata</span><span class="sxs-lookup"><span data-stu-id="e0db8-111">Configure input plugin</span></span>

<span data-ttu-id="e0db8-112">Lägg till toocollect JSON-data i Log Analytics `oms.api.` toohello början av en FluentD-tagg i en inkommande plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="e0db8-112">toocollect JSON data in Log Analytics, add `oms.api.` toohello start of a FluentD tag in an input plugin.</span></span>

<span data-ttu-id="e0db8-113">Till exempel följande är en separat fil `exec-json.conf` i `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.</span><span class="sxs-lookup"><span data-stu-id="e0db8-113">For example, following is a separate configuration file `exec-json.conf` in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.</span></span>  <span data-ttu-id="e0db8-114">Här används FluentD-plugin-programmet hello `exec` toorun curl-kommando med 30 sekunders mellanrum.</span><span class="sxs-lookup"><span data-stu-id="e0db8-114">This uses hello FluentD plugin `exec` toorun a curl command every 30 seconds.</span></span>  <span data-ttu-id="e0db8-115">hello utdata från kommandot samlas in av hello JSON utdata plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="e0db8-115">hello output from this command is collected by hello JSON output plugin.</span></span>

```
<source>
  type exec
  command 'curl localhost/json.output'
  format json
  tag oms.api.httpresponse
  run_interval 30s
</source>

<match oms.api.httpresponse>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api_httpresponse*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```
<span data-ttu-id="e0db8-116">hello-konfigurationsfilen som lagts till under `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` kräver toohave ändra dess ägare med hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="e0db8-116">hello configuration file added under `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` will require toohave its ownership changed with hello following command.</span></span>

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a><span data-ttu-id="e0db8-117">Konfigurera plugin-programmet för utdata</span><span class="sxs-lookup"><span data-stu-id="e0db8-117">Configure output plugin</span></span> 
<span data-ttu-id="e0db8-118">Lägg till följande utdata plugin-programmet toohello huvudsakliga konfiguration i hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` eller som en separat fil placeras i`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`</span><span class="sxs-lookup"><span data-stu-id="e0db8-118">Add hello following output plugin configuration toohello main configuration in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` or as a separate configuration file placed in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`</span></span>

```
<match oms.api.**>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```

### <a name="restart-oms-agent-for-linux"></a><span data-ttu-id="e0db8-119">Starta om OMS-Agent för Linux</span><span class="sxs-lookup"><span data-stu-id="e0db8-119">Restart OMS Agent for Linux</span></span>
<span data-ttu-id="e0db8-120">Starta om hello OMS-Agent för Linux-tjänst med hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="e0db8-120">Restart hello OMS Agent for Linux service with hello following command.</span></span>

    sudo /opt/microsoft/omsagent/bin/service_control restart 

## <a name="output"></a><span data-ttu-id="e0db8-121">Resultat</span><span class="sxs-lookup"><span data-stu-id="e0db8-121">Output</span></span>
<span data-ttu-id="e0db8-122">hello data samlas i logganalys med en typ av post för `<FLUENTD_TAG>_CL`.</span><span class="sxs-lookup"><span data-stu-id="e0db8-122">hello data will be collected in Log Analytics with a record type of `<FLUENTD_TAG>_CL`.</span></span>

<span data-ttu-id="e0db8-123">Till exempel hello anpassad tagg `tag oms.api.tomcat` i logganalys med en typ av post för `tomcat_CL`.</span><span class="sxs-lookup"><span data-stu-id="e0db8-123">For example, hello custom tag `tag oms.api.tomcat` in Log Analytics with a record type of `tomcat_CL`.</span></span>  <span data-ttu-id="e0db8-124">Du kan hämta alla poster för den här typen med hello följande loggen Sök.</span><span class="sxs-lookup"><span data-stu-id="e0db8-124">You could retrieve all records of this type with hello following log search.</span></span>

    Type=tomcat_CL

<span data-ttu-id="e0db8-125">Kapslade JSON-data som stöds, men indexeras baserad på överordnade fältet.</span><span class="sxs-lookup"><span data-stu-id="e0db8-125">Nested JSON data sources are supported, but are indexed based off of parent field.</span></span> <span data-ttu-id="e0db8-126">Till exempel hello följande JSON-data som returneras från en logganalys sökning som `tag_s : "[{ "a":"1", "b":"2" }]`.</span><span class="sxs-lookup"><span data-stu-id="e0db8-126">For example, hello following JSON data is returned from a Log Analytics search as `tag_s : "[{ "a":"1", "b":"2" }]`.</span></span>

```
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a><span data-ttu-id="e0db8-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0db8-127">Next steps</span></span>
* <span data-ttu-id="e0db8-128">Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar.</span><span class="sxs-lookup"><span data-stu-id="e0db8-128">Learn about [log searches](log-analytics-log-searches.md) tooanalyze hello data collected from data sources and solutions.</span></span> 
 