---
title: aaaCollect och analysera Syslog-meddelanden i OMS Log Analytics | Microsoft Docs
description: "Syslog är en händelse loggning protokoll som är vanliga tooLinux. Den här artikeln beskriver hur tooconfigure samling Syslog-meddelanden i logganalys och information om hello poster skapas i hello OMS-databasen."
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
ms.openlocfilehash: 8bfa0bca3f2f18287d1352c98bbaa2a70e41e276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="syslog-data-sources-in-log-analytics"></a>Syslog-datakällor i logganalys
Syslog är en händelse loggning protokoll som är vanliga tooLinux.  Program skickar meddelanden som kan lagras på den lokala datorn för hello eller levereras tooa Syslog-insamlare.  När hello OMS-Agent för Linux är installerat konfigurerar hello lokal Syslog-daemon tooforward meddelanden toohello agent.  hello sedan skickar agenten hello meddelandet tooLog Analytics där en motsvarande post skapas i hello OMS-databas.  

> [!NOTE]
> Log Analytics har stöd för meddelanden som skickas av rsyslog eller syslog-ng där rsyslog är hello standard daemon samling. hello standard syslog-daemon på version 5 Red Hat Enterprise Linux, CentOS och Oracle Linux-version (sysklog) stöds inte för insamling av syslog-händelser. toocollect syslog-data från den här versionen av dessa distributioner hello [rsyslog daemon](http://rsyslog.com) bör vara installerat och konfigurerat tooreplace sysklog.
>
>

![Syslog-samling](media/log-analytics-data-sources-syslog/overview.png)

## <a name="configuring-syslog"></a>Konfigurera Syslog
hello OMS-Agent för Linux endast samlar in händelser med hello verksamhet och allvarlighetsgraderna som anges i dess konfiguration.  Du kan konfigurera Syslog via hello OMS-portalen eller genom att hantera konfigurationsfiler på Linux-agenter.

### <a name="configure-syslog-in-hello-oms-portal"></a>Konfigurera Syslog i hello OMS-portalen
Konfigurera Syslog från hello [Data-menyn i logganalys-inställningar](log-analytics-data-sources.md#configuring-data-sources).  Den här konfigurationen levereras toohello konfigurationsfilen på varje Linux-agenten.

Du kan lägga till en ny funktion genom att skriva dess namn och klicka på  **+** .  Endast meddelanden med hello valt allvarlighetsgraderna kommer att samlas in för varje anläggning.  Kontrollera hello allvarlighetsgraderna för hello viss som du vill toocollect.  Du kan ange ytterligare kriterier toofilter meddelanden.

![Konfigurera Syslog](media/log-analytics-data-sources-syslog/configure.png)

Som standard är alla konfigurationsändringar automatiskt pushas tooall agenter.  Om du vill tooconfigure Syslog manuellt på varje Linux-agent, avmarkerar du kryssrutan hello *tillämpa konfigurationen toomy Linux-datorerna nedan*.

### <a name="configure-syslog-on-linux-agent"></a>Konfigurera Syslog på Linux-agent
När hello [OMS-agenten är installerad på en Linux-klient](log-analytics-linux-agents.md), installeras en standard syslog-konfigurationsfil som definierar hello-anläggningen och allvarlighetsgraden hello meddelanden som har samlats in.  Du kan ändra den här filen toochange hello-konfigurationen.  hello-konfigurationsfilen är olika beroende på hello Syslog-daemon som hello klienten har installerats.

> [!NOTE]
> Om du redigerar hello syslog konfiguration, måste du starta om hello syslog-daemon för hello ändringar tootake effekt.
>
>

#### <a name="rsyslog"></a>rsyslog
hello konfigurationsfilen för rsyslog finns på **/etc/rsyslog.d/95-omsagent.conf**.  Standardinnehållet visas nedan.  Detta samlar in syslog-meddelanden skickas från hello lokal agent för alla verksamhet med en nivå av varning eller högre.

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

Du kan ta bort en funktion genom att ta bort delen av hello konfigurationsfilen.  Du kan begränsa hello allvarlighetsgraderna som samlas in för en viss funktion genom att ändra den lokal transaktion.  Till exempel toolimit hello användaren anläggning toomessages med en allvarlighetsgrad för fel eller högre du ändrar raden hello configuration file toohello följande:

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a>Syslog-ng
hello konfigurationsfilen för syslog-ng är plats **/etc/syslog-ng/syslog-ng.conf**.  Standardinnehållet visas nedan.  Detta samlar in syslog-meddelanden skickas från hello lokal agent för alla resurser och alla grader.   

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

Du kan ta bort en funktion genom att ta bort delen av hello konfigurationsfilen.  Du kan begränsa hello allvarlighetsgraderna som samlas in för en viss funktion genom att ta bort dem från listan.  Till exempel toolimit hello användaren anläggning toojust aviseringen och viktiga meddelanden, ändra motsvarande avsnitt i hello configuration file toohello följande:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a>Samla in data från ytterligare Syslog-portar
hello OMS-agenten lyssnar efter Syslog-meddelanden på hello lokal klient på port 25224.  När hello-agenten är installerad, är en standardkonfiguration för syslog tillämpas och hittades i hello följande plats:

* Rsyslog:`/etc/rsyslog.d/95-omsagent.conf`
* Syslog-ng:`/etc/syslog-ng/syslog-ng.conf`

Du kan ändra hello portnumret genom att skapa två konfigurationsfiler: en FluentD .config-fil och en rsyslog-eller-syslog-ng fil beroende på hello Syslog-daemon som du har installerat.  

* Hej FluentD config-fil ska vara en ny fil i: `/etc/opt/microsoft/omsagent/conf/omsagent.d` och Ersätt hello värdet i hello **port** post med din anpassade portnummer.

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

* För rsyslog, bör du skapa en ny konfigurationsfil finns på: `/etc/rsyslog.d/` och Ersätt hello värdet % SYSLOG_PORT % med dina egna portnummer.  

    > [!NOTE]
    > Om du ändrar det här värdet i konfigurationsfilen för hello `95-omsagent.conf`, skrivs den över när hello agent gäller en standardkonfiguration.
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* hello syslog ng config ska ändras genom att kopiera hello exempelkonfiguration nedan och lägga till hello anpassade ändrade inställningar toohello slutet av hello syslog-ng.conf-konfigurationsfilen finns i `/etc/syslog-ng/`.  Gör **inte** använda hello standardetikett **% WORKSPACE_ID % _oms** eller **% WORKSPACE_ID_OMS**, definiera en anpassad etikett toohelp skilja dina ändringar.  

    > [!NOTE]
    > Om du ändrar hello standardvärdena i konfigurationsfilen för hello skrivs de över när hello agent gäller en standardkonfiguration.
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

När du har slutfört hello ändringar, hello Syslog och hello måste OMS agent-tjänsten startas om toobe tooensure hello configuration ändringar börja gälla.   

## <a name="syslog-record-properties"></a>Egenskaper för Syslog-post
Syslog-poster har en typ av **Syslog** och ha hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| Dator |Dator som hello händelse som samlats in från. |
| Anläggningen |Definierar hello tillhör hello system som genererade hello-meddelande. |
| HostIP |IP-adress hello system hello-meddelande skickas. |
| Värdnamn |Namnet på hello system hello-meddelande skickas. |
| SeverityLevel |Allvarlighetsgrad för hello-händelse. |
| SyslogMessage |Texten för hello-meddelande. |
| Process-ID |ID för hello process som genererade hello-meddelande. |
| EventTime |Datum och tid då hello händelsen skapades. |

## <a name="log-queries-with-syslog-records"></a>Log-frågor med Syslog-poster
hello innehåller följande tabell olika exempel på loggen frågor som hämtar Syslog-poster.

| Fråga | Beskrivning |
|:--- |:--- |
| Typ = Syslog |Alla systemloggar. |
| Typ = Syslog SeverityLevel = fel |Alla Syslog-poster med allvarlighetsgraden fel. |
| Typ = Syslog &#124; måttet count() per dator |Antal Syslog poster per dator. |
| Typ = Syslog &#124; måttet count() av anläggningen |Antal Syslog poster efter anläggning. |

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello senare frågorna skulle ändra toohello följande.

> | Fråga | Beskrivning |
|:--- |:--- |
| Syslog |Alla systemloggar. |
| Syslog &#124; där SeverityLevel == ”error” |Alla Syslog-poster med allvarlighetsgraden fel. |
| Syslog &#124; Sammanfatta AggregatedValue = count() per dator |Antal Syslog poster per dator. |
| Syslog &#124; Sammanfatta AggregatedValue = count() av anläggningen |Antal Syslog poster efter anläggning. |

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar.
* Använd [anpassade fält](log-analytics-custom-fields.md) tooparse data från syslog-poster till enskilda fält.
* [Konfigurera Linux-agenter](log-analytics-linux-agents.md) toocollect andra typer av data.
