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
# <a name="connecting-your-security-products-toohello-operations-management-suite-oms-security-and-audit-solution"></a>Ansluta din säkerhet produkter toohello Operations Management Suite (OMS) säkerhet och granska lösning 
Det här dokumentet hjälper dig att ansluta din säkerhetsprodukter i hello OMS säkerhet och granska lösningen. följande källor hello stöds:

- CEF-händelser (Common Event Format)
- Cisco ASA-händelser


## <a name="what-is-cef"></a>Vad är CEF?
Vanliga händelse (CEF) är en branschstandard för format ovanpå Syslog-meddelanden som används av många säkerhet leverantörer tooallow händelse samverkan mellan olika plattformar. OMS säkerhets- och gransknings-lösningen stöd för datapåfyllning med CEF, vilket gör att du tooconnect din säkerhetsprodukter OMS-säkerhet. 

Genom att ansluta dina datakälla tooOMS är kan tootake nytta av följande funktioner som ingår i den här plattformen hello:

- Sökning och korrelation
- Granskning
- Varning
- Hotinformation
- Anmärkningsvärda problem

## <a name="collection-of-security-solution-logs"></a>Insamling av loggar för säkerhetslösningen

OMS-säkerhetslösningen har stöd för insamling av loggar via CEF över Syslogs- och [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/)-loggar. I det här exemplet hello källan (dator som genererar hello loggar) är en Linux-dator som kör daemon för syslog-ng och hello målet är OMS-säkerhet. tooprepare hello Linux-dator måste tooperform hello följande uppgifter:

- Hämta hello OMS-Agent för Linux version 1.2.0-25 eller senare.
- Följ hello avsnittet **installera snabbguide** från [i den här artikeln](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) tooinstall och publicera hello agent tooyour arbetsyta.

Normalt är hello-agenten installerad på en annan dator från hello på vilka hello loggar genereras. Vidarebefordran hello loggar toohello agentdator kräver vanligtvis hello följande steg:

- Konfigurera hello loggning produkten/datorn tooforward hello nödvändiga händelser toohello daemon för syslog (rsyslog eller syslog-ng) på hello agentdator.
- Aktivera hello syslog-daemon på agenten datorn tooreceive hälsningsmeddelande från ett fjärrsystem.

På hello agentdator måste hello händelser skickas från hello syslog-daemon toolocal UDP-port 25226 toobe. hello agenten lyssnar efter inkommande händelser på denna port. hello följande är ett exempel på en konfiguration för att skicka alla händelser från hello lokalt system toohello agent (du kan ändra hello configuration toofit dina lokala inställningar):

1. Öppna hello terminalfönster och gå toohello directory */etc/syslog-ng /* 
2. Skapa en ny fil *security-config-omsagent.conf* och Lägg till följande innehåll hello: OMS_facility = local4
    
    filter f_local4_oms { facility(local4); };

    destination security_oms { tcp("127.0.0.1" port(25226)); };

    log { source(src); filter(f_local4_oms); destination(security_oms); };
    
3. Hämta hello filen *security_events.conf* och placera i */etc/opt/microsoft/omsagent/conf/omsagent.d/* i hello OMS-Agent datorn.
4. Skriv hello-kommandot nedan toorestart hello syslog-daemon: *för syslog-ng kör:*
    
    ```
    sudo service rsyslog restart
    ```

    *För rsyslog kör du:*
    
    ```
    /etc/init.d/syslog-ng restart
    ```
5. Skriv hello-kommandot nedan toorestart hello OMS-Agent:

    *För syslog-ng kör du:*
    
    ```
    sudo service omsagent restart
    ```

    *För rsyslog kör du:*
    
    ```
    systemctl restart omsagent
    ```
6. Ange hello kommandot nedan och granska hello resultatet tooconfirm att det inte finns några fel i loggen för hello OMS-Agent:

    ``` 
    tail /var/opt/microsoft/omsagent/log/omsagent.log
    ```

## <a name="reviewing-collected-security-events"></a>Gå igenom de insamlade säkerhetshändelserna

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

När hello konfigurationen är klar, startar hello säkerhetshändelse toobe som inhämtas av OMS-säkerhet. toovisualize dessa händelser, öppna hello loggen Sök skriver hello kommando *typ = CommonSecurityLog* i hello sökfältet och tryck på RETUR. hello följande exempel visar hello resultatet av kommandot, Lägg märke till att i det här fallet OMS säkerhet redan inhämtas säkerhetsloggar från flera leverantörer:
   
![Utvärdering av säkerhetsbaslinjen i säkerhets- och granskningslösningen i OMS](./media/oms-security-connect-products/oms-security-connect-products-fig1.png)

Du kan förfina sökningen för en enda leverantör, till exempel toovisualize online Cisco loggar, typ: *typ = CommonSecurityLog DeviceVendor = Cisco*. Hej ”CommonSecurityLog” har fördefinierade fält för alla CEF-huvudet exempel hello grundläggande extensios, medan andra tillägg om den är ”tillägget för anpassat” eller inte, kommer att infogas i fältet ”AdditionalExtensions”. Du kan använda anpassade fält hello funktionen tooget särskilda fält från den. 

### <a name="accessing-computers-missing-baseline-assessment"></a>Kontrollera datorer utan utvärdering av säkerhetsbaslinje
OMS stöder hello domänprofilen medlem baslinje i Windows Server 2008 R2 upp tooWindows Server 2012 R2. Baslinjen för Windows Server 2016 är inte klar än och läggs till så fort den publiceras. Alla andra operativsystem som genomsöks via OMS säkerhet och granska baslinjen bedömning visas under hello **datorer som saknar baslinjen assessment** avsnitt.

## <a name="see-also"></a>Se även
I det här dokumentet du lärt dig hur tooconnect tooOMS din CEF-lösning. toolearn mer om OMS-säkerhet finns hello följande artiklar:

* [Översikt över Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Övervakning och svarar tooSecurity varningar i Operations Management Suite säkerhet och granska lösning](oms-security-responding-alerts.md)
* [Övervaka resurser i säkerhets- och granskningslösningen i Operations Management Suite](oms-security-monitoring-resources.md)

