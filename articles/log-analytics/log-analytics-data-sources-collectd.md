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
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a>Samla in data från CollectD på Linux-agenter i logganalys
[CollectD](https://collectd.org/) är en öppen källkod Linux-demonen som regelbundet samlar in prestandastatistik från program och information om systemet. Exempelprogram inkluderar hello Java Virtual Machine (JVM), MySQL-Server och Nginx. Den här artikeln innehåller information om att samla in prestandadata från CollectD i logganalys.

En fullständig lista över tillgängliga plugin-program finns på [tabell av plugin-program](https://collectd.org/wiki/index.php/Table_of_Plugins).

![Översikt över CollectD](media/log-analytics-data-sources-collectd/overview.png)

hello ingår följande CollectD konfiguration i hello OMS-Agent för Linux tooroute CollectD data toohello OMS-Agent för Linux.

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

Dessutom, om du använder en versioner av collectD innan 5.5 använda hello följande konfiguration i stället.

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

Hej CollectD konfigurationen använder hello standard`write_http` plugin-programmet toosend mått prestandadata via port 26000 tooOMS Agent för Linux. 

> [!NOTE]
> Den här porten kan vara konfigurerade tooa anpassade porten om det behövs.

hello OMS-Agent för Linux också lyssnar på port 26000 CollectD mått och konverterar dem tooOMS schemat mått. hello följer hello OMS-Agent för Linux-konfiguration `collectd.conf`.

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a>Versioner som stöds
- Logganalys stöder för närvarande CollectD version 4.8 och högre.
- OMS-Agent för Linux v1.1.0-217 eller senare krävs för CollectD mått samling.


## <a name="configuration"></a>Konfiguration
hello nedan följer stegen tooconfigure samling CollectD data i logganalys.

1. Konfigurera CollectD toosend data toohello OMS-Agent för Linux med hello write_http plugin-programmet.  
2. Konfigurera hello OMS-Agent för Linux toolisten för hello CollectD data på hello rätt port.
3. Starta om CollectD och OMS-Agent för Linux.

### <a name="configure-collectd-tooforward-data"></a>Konfigurera CollectD tooforward data 

1. tooroute CollectD data toohello OMS-Agent för Linux `oms.conf` behov toobe läggs Toocollectd's konfigurationskatalogen. hello mål för den här filen är beroende av hello Linux distro av din dator.

    Om din CollectD config directory finns i /etc/collectd.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    Om din CollectD config directory finns i /etc/collectd/collectd.conf.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    >För CollectD versioner före 5.5 behöver toomodify hello taggar `oms.conf` som ovan.
    >

2. Kopiera collectd.conf toohello önskad arbetsytan omsagent konfigurationskatalogen.

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. Starta om CollectD och OMS-Agent för Linux med hello följande kommandon.

    sudo service collectd omstart sudo /opt/microsoft/omsagent/bin/service_control omstart

## <a name="collectd-metrics-toolog-analytics-schema-conversion"></a>CollectD mått tooLog Analytics schemat konvertering
toomaintain en bekant modell mellan infrastruktur-mätvärden som redan samlats in av OMS-Agent för Linux och hello nya mått som samlas in av CollectD hello följande schemamappning används:

| Fältet CollectD mått | Log Analytics-fält |
|:--|:--|
| värden | Dator |
| plugin-programmet | Ingen |
| plugin_instance | Instansnamn<br>Om **plugin_instance** är *null* sedan InstanceName = ”*_Total*” |
| typ | Objektnamn |
| type_instance | CounterName<br>Om **type_instance** är *null* sedan CounterName =**tomt** |
| dsnames] | CounterName |
| dstypes | Ingen |
| [] värden | CounterValue |

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar. 
* Använd [anpassade fält](log-analytics-custom-fields.md) tooparse data från syslog-poster till enskilda fält.

