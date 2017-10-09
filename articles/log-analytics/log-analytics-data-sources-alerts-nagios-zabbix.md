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
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-oms-agent-for-linux"></a>Samla in aviseringar från Nagios och Zabbix i logganalys från OMS-Agent för Linux 
[Nagios](https://www.nagios.org/) och [Zabbix](http://www.zabbix.com/) är öppen källkod övervakningsverktyg.  Du kan samla in varningar från dessa verktyg i logganalys i ordning tooanalyze dem tillsammans med [aviseringar från andra källor](log-analytics-alerts.md).  Den här artikeln beskriver hur tooconfigure hello OMS-Agent för Linux toocollect varningar från dessa system.
 
## <a name="configure-alert-collection"></a>Konfigurera varningssamlingen

### <a name="configuring-nagios-alert-collection"></a>Konfigurera Nagios varningssamlingen
Utföra hello följande hello Nagios server toocollect aviseringar för.

1. Bevilja hello användaren **omsagent** läsbehörighet toohello Nagios-loggfilen (d.v.s. `/var/log/nagios/nagios.log`). Under förutsättning att hello nagios.log filen ägs av hello grupp `nagios`, du kan lägga till användaren hello **omsagent** toohello **nagios** grupp. 

    sudo usermod - a -G nagios omsagent

2.  Ändra hello-konfigurationsfilen vid (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Kontrollera att följande poster hello finns och inte kommenterad ut:  

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

3. Starta om hello omsagent daemon

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a>Konfigurera Zabbix varningssamlingen
toocollect aviseringar från en Zabbix-server, behöver du toospecify en användare och lösenord i *klartext*. Detta är inte idealiskt, men vi rekommenderar att du skapar hello användare och bevilja behörigheter toomonitor onlu.

Utföra hello följande hello Nagios server toocollect aviseringar för.

1. Ändra hello-konfigurationsfilen vid (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Kontrollera att följande poster hello finns och inte kommenterad ut.  Ändra hello användarens namn och lösenord toovalues för Zabbix-miljö.

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. Starta om hello omsagent daemon

    sudo del /opt/microsoft/omsagent/bin/service_control starta om


## <a name="alert-records"></a>Varning-poster
Du kan hämta avisering poster från Nagios och Zabbix med [logga sökningar](log-analytics-log-searches.md) i logganalys.

### <a name="nagios-alert-records"></a>Avisering för Nagios-poster

Varna poster som samlas in av Nagios har en **typen** av **avisering** och en **SourceSystem** av **Nagios**.  De har hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| Typ |*Varning* |
| SourceSystem |*Nagios* |
| AlertName |Namnet på hello avisering. |
| AlertDescription | Beskrivning av hello avisering. |
| In AlertState | Status för tjänsten hello eller värden.<br><br>OKEJ<br>VARNING<br>KONFIGURERA<br>NED |
| Värdnamn | Namnet på hello-värden som skapade hello avisering. |
| PriorityNumber | Prioritetsnivån för hello avisering. |
| StateType | hello typ av tillståndet för hello avisering.<br><br>SOFT - problem som inte kontrolleras igen.<br>HÅRDDISK - problem som har ett angivet antal gånger begäran igen.  |
| TimeGenerated |Datum och tid hello aviseringen skapades. |


### <a name="zabbix-alert-records"></a>Varning Zabbix-poster
Varna poster som samlas in av Zabbix har en **typen** av **avisering** och en **SourceSystem** av **Zabbix**.  De har hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| Typ |*Varning* |
| SourceSystem |*Zabbix* |
| AlertName | Namnet på hello avisering. |
| AlertPriority | Allvarlighetsgraden hello avisering.<br><br>inte har klassificerats<br>Information<br>Varning<br>Genomsnittlig<br>Hög<br>katastrofåterställning  |
| In AlertState | Tillståndet för hello avisering.<br><br>0 - tillståndet är upp toodate.<br>1 - tillståndet är okänt.  |
| AlertTypeNumber | Anger om avisering kan skapa flera problem händelser.<br><br>0 - tillståndet är upp toodate.<br>1 - tillståndet är okänt.    |
| Kommentarer | Ytterligare kommentarer för aviseringen. |
| Värdnamn | Namnet på hello-värden som skapade hello avisering. |
| PriorityNumber | Värdet anger allvarlighetsgraden hello avisering.<br><br>0 - inte har klassificerats<br>1 - information<br>2 - varning<br>3 - medel<br>4 - hög<br>5 - katastrofåterställning |
| TimeGenerated |Datum och tid hello aviseringen skapades. |
| TimeLastModified |Datum och tid hello tillstånd hello aviseringen senast ändrades. |


## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [aviseringar](log-analytics-alerts.md) i logganalys.
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar. 
