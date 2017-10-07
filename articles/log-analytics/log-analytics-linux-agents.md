---
redirect_url: /azure/log-analytics/log-analytics-agent-linux
redirect_document_id: True
ROBOTS: NOINDEX
ms.openlocfilehash: 8b526144cd565f6750368e12970f008e66cc2023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-linux-computers-toolog-analytics"></a>Ansluta din Linux-datorer tooLog Analytics
Använda Log Analytics kan du samla in och fungerar på data som genereras från Linux-datorer. Om du lägger till data som samlas in från Linux tooOMS kan du toomanage Linux-system och lösningar för behållare som Docker, oavsett var dina datorer finns – var som helst. Datakällor kan finnas i ditt lokala datacenter som fysiska servrar, virtuella datorer i en molnbaserad tjänst som Amazon Web Services (AWS) eller Microsoft Azure eller även hello bärbar dator på ditt skrivbord. Dessutom OMS samlar även in data från Windows-datorer på samma sätt, så att den stöder en verkligen hybrid IT-miljö.

Du kan visa och hantera data från alla dessa källor med logganalys i OMS med en enda hanteringsportal. Detta minskar behovet av hello toomonitor med många olika system gör det enkelt tooconsume och du kan exportera alla data som du vill toowhatever analytics affärslösning eller system som du redan har.

Den här artikeln är en snabbstartsguiden som hjälper dig att samla in och hantera data för ditt Linux-datorer med hjälp av hello OMS-Agent för Linux. Ytterligare teknisk information, till exempel proxyserverkonfiguration, information om CollectD mått och anpassad JSON-datakällor hittar du informationen på [OMS-Agent för Linux-översikt](https://github.com/Microsoft/OMS-Agent-for-Linux) och [OMS-Agent för Linux hela dokumentationen](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) på GitHub.

För närvarande kan du samla in hello följande typer av data från Linux-datorer:

* Prestandamått
* Syslog-händelser
* Aviseringar från Nagios och Zabbix
* Prestandamått för docker-behållare, inventering och loggar

## <a name="supported-linux-versions"></a>Linux-versioner som stöds
Både x86 och x64 versioner stöds officiellt på olika Linux-distributioner. Hello OMS-Agent för Linux kan också köra på andra distributioner som inte finns.

* Amazon Linux 2012.09 via 2015.09
* CentOS Linux 5, 6 och 7
* Oracle Linux 5, 6 och 7
* Red Hat Enterprise Linux Server 5, 6 och 7
* Debian GNU/Linux 6, 7 och 8
* Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10
* SUSE Linux Enterprise Server 11 och 12

## <a name="oms-agent-for-linux"></a>OMS-Agent för Linux
hello Operations Management Suite-Agent för Linux består av flera paket. hello versionen filen innehåller hello följande paket, som körs hello shell-paket med `--extract`.

| **Paketet** | **Version** | **Beskrivning** |
| --- | --- | --- |
| omsagent |1.1.0 |hello Operations Management Suite-Agent för Linux |
| omsconfig |1.1.1 |Konfiguration av agenten för hello OMS-Agent |
| omi |1.0.8.3 |Open Management Infrastructure (OMI)--en förenklad CIM-servern |
| scx |1.6.2 |OMI CIM-Providers för operativsystemet prestandamått |
| Apache cimprov |1.0.0 |Apache HTTP-Server prestandaövervakning provider för OMI. Endast installera om Apache HTTP-Server har upptäckts. |
| MySQL-cimprov |1.0.0 |MySQL-Server prestandaövervakning provider för OMI. Endast installera om MySQL/MariaDB server har upptäckts. |
| docker-cimprov |0.1.0 |Docker-provider för OMI. Endast installera om Docker har upptäckts. |

### <a name="additional-installation-artifacts"></a>Installation av ytterligare artefakter
När du har installerat hello OMS-agent för Linux-paket tillämpas hello följande ytterligare systemomfattande konfigurationsändringar. Dessa artefakter tas bort när hello omsagent paketet är avinstallerat.

* En icke-privilegierade användare med namnet: `omsagent` skapas. Detta är hello konto hello omsagent daemon körs som
* En sudoers ”innehåller” fil skapas på /etc/sudoers.d/omsagent detta ger rätt omsagent toorestart hello syslog och omsagent daemon. Om sudo ”” Includes inte stöds i hello installerade versionen av sudo, skrivs dessa poster för/etc/sudoers.
* hello syslog-konfigurationen är ändrade tooforward en delmängd av händelser toohello agent. Mer information finns i hello **konfigurera datainsamling** nedan

### <a name="linux-data-collection-details"></a>Linux information för samlingen
hello följande tabell visar metoder för insamling av data och annan information om hur data samlas in.

| Källa | Styr Agent | SCOM-agent | Azure Storage | SCOM krävs? | SCOM-agent data som skickas via management-grupp | Frekvens för samlingen |
| --- | --- | --- | --- | --- | --- | --- |
| Zabbix |![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |1 minut |
| Nagios |![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |anländer |
| syslog |![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |från Azure storage: 10 minuter. från agent: anländer |
| Linux-prestandaräknare |![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |Som planerat, minst 10 sekunder |
| Spåra ändringar |![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nej](./media/log-analytics-linux-agents/oms-bullet-red.png) |varje timme |

### <a name="package-requirements"></a>Krav för paketet
| **Nödvändigt paket** | **Beskrivning** | **Lägsta version** |
| --- | --- | --- |
| Glibc |GNU C-bibliotek |2.5-12 |
| Openssl |OpenSSL-bibliotek |0.9.8e eller 1.0 |
| CURL |cURL Webbklient |7.15.5 |
| Python-ctypes |Funktionsbibliotek |Saknas |
| PAM |Lösenordssynkronisering moduler |Saknas |

> [!NOTE]
> Rsyslog eller syslog-ng är obligatoriska toocollect syslog-meddelanden. hello standard syslog-daemon på version 5 Red Hat Enterprise Linux, CentOS och Oracle Linux-version (sysklog) stöds inte för insamling av syslog-händelser. toocollect syslog-data från den här versionen av dessa distributioner hello rsyslog daemon ska vara installerat och konfigurerat tooreplace sysklog.
>
>

## <a name="quick-install"></a>Snabb installation
Kör följande kommandon toodownload hello omsagent hello, verifiera hello kontrollsumma och sedan installera och publicera hello agent. Kommandon är för 64-bitarsversionen. hello arbetsyte-ID och primärnyckel finns i hello OMS-portalen under **inställningar** på hello **anslutna källor** fliken.

![information om arbetsytan](./media/log-analytics-linux-agents/oms-direct-agent-primary-key.png)

```
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR OMS WORKSPACE ID> -s <YOUR OMS WORKSPACE PRIMARY KEY>
```

Det finns en mängd andra metoder tooinstall hello agent och uppgradera den. Du kan läsa mer om dem i [steg tooinstall hello OMS-Agent för Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).

Du kan också visa hello [Azure videogenomgång](https://www.youtube.com/watch?v=mF1wtHPEzT0).

## <a name="choose-your-linux-data-collection-method"></a>Välj din Linux metod för insamling av data
Hur du väljer hello datatyper som du skulle t.ex toocollect beror på om du vill toouse hello OMS-portalen eller om du vill redigera olika konfigurationsfilerna direkt på Linux-klienter. Om du väljer toouse hello portal skickas hello configuration tooall av Linux-klienter automatiskt. Om du behöver olika konfigurationer för olika Linux-klienter kommer du behöver tooedit klientfiler individuellt – eller Använd ett alternativ som PowerShell DSC, Chef eller Puppet.

Du kan ange hello syslog-händelser och prestandaräknare som du vill toocollect använder konfigurationsfiler på hello Linux-datorer. *Om du väljer tooconfigure datainsamling genom att redigera konfigurationsfilerna för agenten, bör du inaktivera hello centraliserad konfiguration.*  Instruktioner finns under tooconfigure datainsamling i hello agent configuration-filer samt toodisable centrala konfiguration för alla OMS-agenter för Linux eller enskilda datorer.

### <a name="disable-oms-management-for-an-individual-linux-computer"></a>Inaktivera OMS-hantering för en enskild Linux-dator
Centraliserad datainsamling för konfigurationsdata är inaktiverad för en enskild Linux-dator med hello OMS_MetaConfigHelper.py skript. Detta kan vara användbart om en delmängd av datorerna ska ha en särskild konfiguration.

toodisable centraliserad konfiguration:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```

Aktivera toore centraliserad konfiguration:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py –enable
```

## <a name="linux-performance-counters"></a>Linux-prestandaräknare
Linux-prestandaräknare är liknande tooWindows prestandaräknare – både fungerar på samma sätt. Du kan använda följande procedurer tooadd hello och konfigurera dem. När de läggs tooOMS samlas data in för dem var 30: e sekund.

### <a name="tooadd-a-linux-performance-counter-in-oms"></a>tooadd en Linux-prestandaräknare i OMS
1. tooconfigure OMS-agenter för Linux med hello OMS-portalen, du kan lägga till Linux prestandaräknare på hello inställningar klickar du på **Data**.  
2. På hello **inställningar** sidan **Data** , klickar du på **Linux prestandaräknare** och sedan väljer eller typen hello hello räknare som du vill tooadd.  
    ![data](./media/log-analytics-linux-agents/oms-settings-data01.png)
3. Om du inte vet hello fullständigt namn på räknaren av hello, du kan börja skriva ett partiellt namn och en lista över tillgängliga räknare visas. När du har hittat hello räknare du vill tooadd på hello namn i hello listan och klicka sedan på hello plus ikonen tooadd hello räknaren.
4. När du har lagt till hello räknaren visas den i hello listan med räknare med ett färgade fält.
5. Som standard hello **tillämpa konfigurationen toomy datorerna nedan** alternativet är markerat. Om du vill skicka konfigurationsdata toodisable avmarkerar du hello val.
6. När du är klar ändra prestandaräknare på hello hello sidans nederkant klickar du på **spara** toofinalize ändringarna. hello konfigurationsändringar som du har gjort skickas sedan tooall hello OMS-Agent för Linux som har registrerats med OMS, vanligtvis inom 5 minuter.

### <a name="configure-linux-performance-counters-in-oms"></a>Konfigurera Linux prestandaräknare i OMS
Du kan välja en specifik instans för varje prestandaräknare för Windows-prestandaräknare. För Linux-prestandaräknare gäller dock instansen av en räknare som du väljer tooall underordnade räknare för hello överordnade räknaren. hello visar följande tabell hello vanliga instanser tillgängliga tooboth Linux och Windows prestandaräknare.

| **Instansnamn** | **Betydelse** |
| --- | --- |
| \_Totalt |Summan av alla hello-instanser |
| \* |Alla instanser |
| (/ &#124; / var) |Matchar instanser med namnet: / eller /var |

På liknande sätt gäller hello intervall som du väljer för en överordnad räknare tooall dess underordnade räknare. Med andra ord är alla hello underordnade räknaren samplingsintervall och instanser som knutna tillsammans.

### <a name="add-and-configure-performance-metrics-with-linux"></a>Lägga till och konfigurera prestandamått med Linux
Prestanda mått toocollect styrs av hello konfiguration i/etc/opt/microsoft/omsagent/&lt;arbetsyte-id&gt;/conf/omsagent.conf. Se [tillgängliga prestandamått](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) för tillgängliga klasser och mått för hello OMS-Agent för Linux.

Varje objekt eller kategori av prestanda mått toocollect ska vara definierat i hello konfigurationsfilen som en enda `<source>` element. hello syntax följer hello mönster nedan.

```
<source>
  type oms_omi  
  object_name "Processor"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

hello konfigurerbara parametrarna i det här elementet är:

* **Objektet\_namn**: hello objektnamn för hello samling.
* **Instansen\_regex**: en *reguljärt uttryck* definierar vilka instanser toocollect. Hej värde: `.*` anger alla instanser. toocollect processor mätvärden för endast hello \_totala instans kan du ange `_Total`. toocollect Processmätvärden för hello endast crond eller sshd instanser, kan du ange: `(crond|sshd)`.
* **Räknaren\_namn\_regex**: en *reguljärt uttryck* definierar vilka räknare (för hello) toocollect. toocollect alla räknare för hello, ange: `.*`. toocollect växla endast utrymme räknare för hello minnesobjekt, kan du ange:`.+Swap.+`
* **Intervall:**: hello frekvens vid vilken hello objektets räknare samlas in.

hello standardkonfigurationen för prestandavärden är:

```
<source>
  type oms_omi
  object_name "Physical Disk"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Logical Disk"
  instance_regex ".*
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Processor"
  instance_regex ".*
  counter_name_regex ".*"
  interval 30s
</source>

<source>
  type oms_omi
  object_name "Memory"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

### <a name="enable-mysql-performance-counters-using-linux-commands"></a>Aktivera MySQL prestandaräknare med Linux-kommandon
Om MySQL-Server eller MariaDB Server har upptäckts på hello dator när hello omsagent paket installeras, installeras automatiskt en provider för MySQL-Server för prestandaövervakning. Den här providern ansluter toohello lokala MySQL/MariaDB server tooexpose prestandastatistik. Du behöver tooconfigure MySQL användarens autentiseringsuppgifter så att hello providern kan komma åt hello MySQL-servern.

toodefine standard användarkonton för hello MySQL-server på localhost, Använd hello efter kommandot.

> [!NOTE]
> hello autentiseringsuppgifter fil måste läsas av hello omsagent konto. Du bör köra hello mycimprovauth kommando som omsgent.
>
>

```
sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>

sudo /opt/omi/bin/service_control restart
```


Du kan också ange hello krävs MySQL autentiseringsuppgifter i en fil genom att skapa hello: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. Mer information om hur du hanterar MySQL autentiseringsuppgifter för övervakning via hello mysql-auth-filen finns [hantera MySQL övervakning autentiseringsuppgifter i hello autentiseringsfilen](#manage-mysql-monitoring-credentials-in-the-authentication-file).

Se [behörigheter som krävs för prestandaräknare för MySQL-databas](#database-permissions-required-for-mysql-performance-counters) för ytterligare information om objektbehörigheter som krävs av hello MySQL användaren toocollect MySQL prestandainformation.

### <a name="enable-apache-http-server-performance-counters-using-linux-commands"></a>Aktivera prestandaräknare för Apache HTTP-servern med hjälp av Linux-kommandon
Om Apache HTTP-Server har upptäckts på hello dator när hello omsagent paket installeras, installeras automatiskt en provider för Apache HTTP-Server för prestandaövervakning. Den här providern förlitar sig på en Apache ”modul” som måste läsas in i hello Apache HTTP-Server i ordning tooaccess prestandadata.

Du kan läsa in modulen hello med hello följande kommando:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

toounload hello Apache övervakningsmodulen, kör hello följande kommando:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```
### <a name="tooview-performance-data-with-log-analytics"></a>tooview prestandadata med logganalys
1. Klicka på hello loggen Sök panelen i hello Operations Management Suite-portalen.
2. Skriv i hello sökfältet `* (Type=Perf)` tooview alla prestandaräknare.

Eftersom OMS samlar även in data från Windows prestandaräknare, du bör scope på hello Sök tooLinux-specifika data. Därför visas hello följande exempel prestanda data specifika tooan exempel Linux-servern med namnet Chorizo21.

```
Type=Perf Computer=chorizo*
```

![exempel server visas i sökresultaten](./media/log-analytics-linux-agents/oms-perfsearch01.png)

I hello resultat, kan du klicka på **mått** tooview hello räknare som data har samlats in för. Realtidsdata visas som diagram för varje räknare.

![metrics](./media/log-analytics-linux-agents/oms-perfmetrics01.png)

## <a name="syslog"></a>Syslog
Syslog är en händelseloggning protokollet liknande tooWindows händelseloggar – både fungerar på samma sätt när de visas i OMS.

### <a name="tooadd-a-new-linux-syslog-facility-in-oms"></a>tooadd en ny Linux syslog-funktion i OMS
1. På hello **inställningar** sidan **Data** , klickar du på **Syslog** och toohello vänster hello plus ikon, Skriv hello namnet på hello syslog-funktion som du vill tooadd.
    ![Linux syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)
2. Om du inte vet hello fullständiga namn hello och du kan börja skriva ett partiellt namn och en lista över tillgängliga syslog verksamhet visas. När du hittar hello syslog-funktion som du vill tooadd på hello namn i hello listan och klicka sedan på hello plus ikonen tooadd hello syslog-funktion.
3. När du lägger till hello och det visas i hello listan över markerade med en färgade stapel. Därefter väljer du hello allvarlighetsgraderna (informationskategorier syslog anläggning) som du vill toocollect.
4. Hello längst ned på sidan för hello klickar du på **spara** toofinalize ändringarna. hello konfigurationsändringar som du har gjort skickas sedan tooall hello OMS-Agent för Linux som har registrerats med OMS, vanligtvis inom 5 minuter.

### <a name="configure-linux-syslog-facilities-in-linux"></a>Konfigurera Linux syslog anläggningarna i Linux
Syslog-händelser skickas från hello daemon för syslog, till exempel rsyslog eller syslog-ng, tooa lokal port hello agenten lyssnar på. Som standard port 25224. När hello-agenten är installerad, används en standardkonfiguration för syslog. Detta är finns på:

Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf

Syslog-ng: /etc/syslog-ng/syslog-ng.conf

hello OMS-agenten syslog standardkonfigurationen filöverföringar syslog-händelser från alla lokaler med en allvarlighetsgrad för varning eller högre.

> [!NOTE]
> Om du redigerar hello syslog konfiguration, måste du starta om hello syslog-daemon för hello ändringar tootake effekt.
>
>

hello syslog standardkonfigurationen för hello OMS-Agent för Linux för OMS är:

#### <a name="rsyslog"></a>rsyslog
```
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
```

#### <a name="syslog-ng"></a>Syslog-ng
```
#OMS_facility = all
filter f_warning_oms { level(warning); };
destination warning_oms { tcp("127.0.0.1" port(25224)); };
log { source(src); filter(f_warning_oms); destination(warning_oms); };
```

### <a name="tooview-all-syslog-events-with-log-analytics"></a>tooview Syslog-händelser med logganalys
1. I hello Operations Management Suite-portalen klickar du på hello **loggen Sök** panelen.
2. I hello **hantering** gruppering, Välj en fördefinierad syslog-sökning och välj sedan en toorun den.

Det här exemplet visar alla Syslog-händelser.

![Syslog-händelser som visas i loggen Sök](./media/log-analytics-linux-agents/oms-linux-syslog.png)

Du kan nu detaljerat sökresultat.

## <a name="linux-alerts"></a>Linux-aviseringar
Om du använder Nagios eller Zabbix toomanage Linux-datorer och sedan OMS kan ta emot hello varningar som genereras av dessa verktyg. Det finns för närvarande inga metoden tooconfigure inkommande aviseringsdata med hello OMS-portalen. I stället måste tooedit en config-fil toostart Skicka aviseringar-tooOMS.

### <a name="collect-alerts-from-nagios"></a>Samla in aviseringar från Nagios
toocollect aviseringar från en Nagios-server, måste toomake hello efter konfigurationsändringar.

1. Bevilja hello användaren **omsagent** läsbehörighet toohello Nagios-loggfilen (d.v.s. /var/log/nagios/nagios.log). Under förutsättning att hello nagios.log filen ägs av hello grupp **nagios** , du kan lägga till användaren hello **omsagent** toohello **nagios** grupp.

    ```
    sudo usermod –a -G nagios omsagent
    ```
2. Ändra hello omsagent.confconfiguration filen (/ etc/opt/microsoft/omsagent/&lt;arbetsyte-id&gt;/conf/omsagent.conf). Kontrollera att följande poster hello finns och inte kommenterad ut:

    ```
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
    ```
3. Starta om hello omsagent daemon:

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="collect-alerts-from-zabbix"></a>Samla in aviseringar från Zabbix
toocollect varningar från en Zabbix-server du utför liknande steg toothose för Nagios ovan, men du behöver toospecify en användare och lösenord i *klartext*. Detta är inte idealiskt, men troligen ändrar snart. tooaddress problemet, rekommenderar vi att du skapar hello användare och bevilja behörighet toomonitor endast.

En exempel-avsnittet i konfigurationsfilen för hello omsagent.conf (/ etc/opt/microsoft/omsagent/&lt;arbetsyte-id&gt;/conf/omsagent.conf) för Zabbix bör likna följande hello:

```
<source>
  type zabbix_alerts
  run_interval 1m
  tag oms.zabbix
  zabbix_url http://localhost/zabbix/api_jsonrpc.php
  zabbix_username Admin
  zabbix_password zabbix
</source>

```

### <a name="view-alerts-in-log-analytics-search"></a>Visa aviseringar i logganalys-sökning
När du har konfigurerat ditt Linux-datorer toosend aviseringar tooOMS kan använda du några enkla loggen Sök frågor tooview hello aviseringar. hello returneras följande Sök frågan exempel alla hello registreras aviseringar som genererades. Om någon form av ett problem inträffar i din IT-infrastruktur, resulterar t.ex, sedan för hello följande exempel fråga hända om hello problem kan kommer. Och du kan enkelt detaljgranska i toohello aviseringar av källa system toohelp smala undersökningen. hello fördelen är att du inte nödvändigtvis behöver toogo toovarious hanteringssystem från hello start, förutsatt att aviseringar ska skickas tooOMS, du kan starta det.

```
Type=Alert
```

#### <a name="tooview-all-nagios-alerts-with-log-analytics"></a>tooview alla Nagios aviseringar med logganalys
1. I hello Operations Management Suite-portalen klickar du på hello **loggen Sök** panelen.
2. Adressfältet i hello frågan hello följande sökfråga

    ```
    Type=Alert SourceSystem=Nagios
    ```
   ![Nagios aviseringar visas i loggen Sök](./media/log-analytics-linux-agents/oms-linux-nagios-alerts.png)

När du ser hello sökresultat, detaljer om ytterligare information som *in AlertState*.

### <a name="tooview-all-zabbix-alerts-with-log-analytics"></a>tooview alla Zabbix aviseringar med logganalys
1. I hello Operations Management Suite-portalen klickar du på hello **loggen Sök** panelen.
2. Adressfältet i hello frågan hello följande sökfråga

    ```
    Type=Alert SourceSystem=Zabbix
    ```
   ![Zabbix aviseringar visas i loggen Sök](./media/log-analytics-linux-agents/oms-linux-zabbix-alerts.png)

När du ser hello sökresultat, detaljer om ytterligare information som *AlertName*.

## <a name="compatibility-with-system-center-operations-manager"></a>Kompatibilitet med System Center Operations Manager
hello OMS-Agent för Linux delar agent binärfiler med hello System Center Operations Manager-agenten. Installera hello OMS-Agent för Linux på ett system som för närvarande hanteras av Operations Manager hello uppgraderingar OMI och SCX paket på hello datorn tooa nyare version. hello OMS-Agent för Linux och System Center 2012 R2 är kompatibla. Dock **System Center 2012 SP1 och tidigare versioner är inte kompatibla eller stöds med hello OMS-Agent för Linux.**

> [!NOTE]
> Om du senare vill toomanage hello datorn med Operations Manager hello OMS-Agent för Linux är installerade tooa dator som för närvarande inte hanteras av Operations Manager, måste du ändra hello OMI konfiguration innan du identifiera hello-datorn. **Det här steget behövs inte om hello Operations Manager-agenten är installerad före hello OMS-Agent för Linux.**
>
>

### <a name="tooenable-hello-oms-agent-for-linux-toocommunicate-with-operations-manager"></a>tooenable hello OMS-Agent för Linux toocommunicate med Operations Manager
1. Redigera hello filen /etc/opt/omi/conf/omiserver.conf
2. Se till att hello rad som börjar med **httpsport =** definierar hello port 1270. Exempel`httpsport=1270`
3. Starta om hello OMI-servern:

    ```
    sudo /opt/omi/bin/service_control restart
    ```

## <a name="database-permissions-required-for-mysql-performance-counters"></a>Databasbehörigheter som krävs för MySQL-prestandaräknare
toogrant behörigheter tooa MySQL övervakning hello beviljande användaren måste användaren, ha hello alternativet för BEVILJA behörighet samt hello behörighet beviljas.

För att hello MySQL användaren tooreturn prestanda data hello användare kommer behöver åtkomst till toohello följande frågor:

```
SHOW GLOBAL STATUS;
SHOW GLOBAL VARIABLES:
```

Dessutom kräver toothese frågor hello MySQL användaren väljer åtkomst toohello följande standardtabeller:

* INFORMATION_SCHEMA
* MySQL

Dessa behörigheter kan tilldelas genom att köra hello följande grant-kommandon.

```
GRANT SELECT ON information_schema.* too‘monuser’@’localhost’;
GRANT SELECT ON mysql.* too‘monuser’@’localhost’;
```

## <a name="manage-mysql-monitoring-credentials-in-hello-authentication-file"></a>Hantera MySQL övervakning autentiseringsuppgifter i hello autentiseringsfilen
hello följande avsnitt hjälper dig att hantera MySQL-autentiseringsuppgifter.

### <a name="configure-hello-mysql-omi-provider"></a>Konfigurera hello MySQL OMI-providern
hello MySQL OMI-providern kräver att en förkonfigurerad MySQL-användare och installerat MySQL klientbibliotek i ordning tooquery hello prestanda/hälsoinformation från hello MySQL-instans.

### <a name="mysql-omi-authentication-file"></a>MySQL OMI autentiseringsfilen
MySQL OMI-providern använder en autentisering filen toodetermine vilka bind-adress och port hello MySQL instans lyssnar på och vilka autentiseringsuppgifter toouse toogather mått. Under installationen hello MySQL OMI genomsöks providern MySQL my.cnf configuration-filer (standardplatserna) för bind-adress och port och delvis set hello MySQL OMI autentiseringsfilen.

toocomplete övervakning av en MySQL-serverinstans Lägg till en förgenererade MySQL OMI autentisering fil i hello rätt katalog.

### <a name="authentication-file-format"></a>Filformatet för autentisering
hello MySQL OMI autentiseringsfilen är en textfil som innehåller information om:

* Port
* Bind-adress
* MySQL-användarnamn
* Base64-kodat lösenord

hello MySQL OMI autentiseringsfilen ger endast behörighet för läsning och skrivning toohello Linux användaren som skapade den.

```
[Port]=[Bind-Address], [username], [Base64 encoded Password]
(Port)=(Bind-Address), (username), (Base64 encoded Password)
(Port)=(Bind-Address), (username), (Base64 encoded Password)
AutoUpdate=[true|false]
```

Standard MySQL OMI autentiseringsfilen innehåller en standardinstans och ett portnummer beroende på vilken information som är tillgänglig och parsade från hello MySQL konfigurationsfil hittades.

hello standardinstansen är ett sätt toomake hantera flera MySQL-instanser på en Linux-värd enklare och markeras med hello-instans med port 0. Alla tillagda instanser ärver egenskaper från hello standardinstansen. Till exempel om MySQL-instans som lyssnar på port '3308' läggs kommer hello standardinstans bind-adress, användarnamn och lösenord för Base64-kodade att använda tootry och övervaka hello-instans som lyssnar på 3308. Om hello-instans på 3308 är bundits tooanother adress och använder hello samma MySQL-användarnamn och lösenord par hello endast respecification av hello bind-adress behövs och hello andra egenskaper ärvs.

Exempel på hello autentiseringsfilen likna följande hello.

Standardinstansen och instans med port 3308:

```
0=127.0.0.1, myuser, cnBwdA==3308=, ,AutoUpdate=true
```

Standardinstansen och -instans med port 3308 + olika Base64-kodade lösenord:

```
0=127.0.0.1, myuser, cnBwdA==3308=127.0.1.1, , AutoUpdate=true
```


| **Egenskap** | **Beskrivning** |
| --- | --- |
| Port |Port representerar hello aktuella port hello MySQL instans lyssnar på.  hello port 0 innebär att hello egenskaper följande används för standardinstansen. |
| Bind-adress |hello binda adress är hello aktuella MySQL bind-adress |
| användarnamn |Hello användarnamnet för hello MySQL användare vill toouse toomonitor hello MySQL server-instansen. |
| Base64-kodat lösenord |Detta är hello lösenord för hello MySQL övervakning användare kodad i Base64. |
| Automatisk uppdatering |När hello MySQL OMI providern uppgraderas hello-providern genomsökning efter ändringar i hello my.cnf filen och skriva över hello MySQL OMI autentiseringsfilen. Ange den här flaggan tootrue eller false beroende på nödvändiga uppdateringar toohello MySQL OMI autentiseringsfilen. |

#### <a name="authentication-file-location"></a>Filplats för autentisering
hello MySQL OMI autentiseringsfilen bör finns i hello följande plats och med namnet ”mysql-auth”:

/var/OPT/Microsoft/MySQL-cimprov/auth/omsagent/MySQL-auth

hello-filen (och auth/omsagent directory) bör ägas av hello omsagent användare.

## <a name="agent-logs"></a>Agenten loggar
hello hello OMS-Agent för Linux-loggarna finns på:

/ var/opt/microsoft/omsagent/&lt;arbetsyte-id&gt;/log/

hello-loggarna för hello OMS-Agent för Linux för omsconfig (agentkonfiguration) programmet finns på:

/ var/opt/microsoft/omsconfig/log /

Loggar för hello OMI och SCX-komponenter (som tillhandahåller mått prestandadata) finns på:

/ var/opt/omi/log/och /var/opt/microsoft/scx/log

## <a name="troubleshooting-hello-oms-agent-for-linux"></a>Felsöka hello OMS-Agent för Linux
Använd följande information toodiagnose hello och felsöka vanliga problem.

Om ingen av hello felsökningsinformation i det här avsnittet hjälper dig att kan du också använda hello följande resurser toohelp Lös problemet.

* Kunder med Premier support Logga ett supportärende via [Premier](https://premier.microsoft.com/)
* Kunder med Azure supportavtal kan logga Supportfall i hello [Azure-portalen](https://manage.windowsazure.com/?getsupport=true)
* Filen en [GitHub problemet](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)
* Feedbackforum för uppslag och toocreate en felrapport [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)

### <a name="important-log-locations"></a>Viktiga loggfilernas placering
| Fil | Sökväg |
| --- | --- |
| OMS-Agent för Linux-loggfil |`/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log ` |
| Loggfil för OMS-Agent konfiguration |`/var/opt/microsoft/omsconfig/omsconfig.log` |

### <a name="important-configuration-files"></a>Viktiga konfigurationsfiler
| Catergory | Filplats |
| --- | --- |
| Syslog |`/etc/syslog-ng/syslog-ng.conf`eller `/etc/rsyslog.conf` eller`/etc/rsyslog.d/95-omsagent.conf` |
| Prestanda, Nagios, Zabbix, OMS-utdata och allmänna agent |`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` |
| Ytterligare konfigurationer |`/etc/opt/microsoft/omsagent/<workspace id>/omsagent.d/*.conf` |

> [!NOTE]
> Redigera konfigurationsfilerna för prestandaräknare och syslog skrivs över om OMS-portalen konfiguration har aktiverats. Du kan inaktivera konfigurationen i hello OMS-portalen (för alla noder) eller för enskild noder genom att köra hello följande:
>
>

```
sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```


### <a name="enable-debug-logging"></a>Aktivera felsökningsloggning
tooenable debug loggning, kan du använda hello OMS utdata plugin-programmet och utförlig utdata.

#### <a name="oms-output-plugin"></a>OMS utdata plugin-program
FluentD kan hello plugin-programmet toospecify loggningsnivåerna för olika loggningsnivåer för in- och utdataenheter. toospecify en annan loggningsnivån för OMS-utdata redigera hello allmänna agentkonfiguration i hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` fil.

Ändra hello hello nedre delen av konfigurationsfilen hello `log_level` egenskap från `info` för`debug`.

 ```
 <match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
 ```

Felsökningsloggning kan toosee batchar överföringar toohello OMS-tjänsten avgränsade med typen, antalet dataobjekt och tidsåtgång toosend.

*Exempel debug aktiverad logg:*

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

#### <a name="verbose-output"></a>Utförlig utdata
Istället för att använda hello OMS utdata plugin-programmet, du kan också spara dataobjekt direkt för`stdout`, som är synligt i hello OMS-Agent för Linux-loggfil.

I hello OMS allmänna agent-konfigurationsfilen vid `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, kommenterar ut hello OMS utdata plugin-programmet genom att lägga till en `#` framför varje rad.

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

Nedan Hej utdata plugin-programmet, ta bort hello kommentar i hello efter avsnittet genom att ta bort hello `#` symbol hello början av varje rad.

```
<match **>
  type stdout
</match>
```

### <a name="forwarded-syslog-messages-do-not-appear-in-hello-log"></a>Vidarebefordrade Syslog-meddelanden visas inte i hello logg
#### <a name="probable-causes"></a>Troliga orsaker
* hello tillämpas toohello Linux konfigurationsservern inte tillåta samling hello skickas verksamhet och/eller logga nivåer
* Syslog vidarebefordras inte korrekt toohello Linux-server
* hello antal meddelanden vidarebefordras per sekund är för stora för hello baskonfigurationen för hello OMS-Agent för Linux toohandle

#### <a name="resolutions"></a>Lösningar
* Kontrollera att hello-konfigurationen i hello OMS-portalen för Syslog har alla hello hjälpmedel och hello rätt loggningsnivåer
  * **OMS-portalen > Inställningar > Data > Syslog**
* Kontrollera den interna syslog messaging Daemon (`rsyslog`, `syslog-ng`) är kan tooreceive hello vidarebefordrade meddelanden
* Kontrollera brandväggsinställningarna på hello Syslog-servern tooensure att meddelanden inte blockeras
* Simulera en tooOMS för Syslog-meddelande som använder hello `logger` kommandot - exempel:
  * `logger -p local0.err "This is my test message"`

### <a name="problems-connecting-toooms-when-using-a-proxy"></a>Problem med att ansluta tooOMS när du använder en proxyserver
#### <a name="probable-causes"></a>Troliga orsaker
* hello proxy anges när installera och konfigurera hello-agenten är felaktig
* hello OMS-tjänstens slutpunkter är inte whitelistested i ditt datacenter

#### <a name="resolutions"></a>Lösningar
* Installera om hello OMS-Agent för Linux med följande kommando med hello alternativet hello `-v` aktiverat. Detta gör att utförliga utdata av hello-agenten ansluter via hello proxy toohello OMS-tjänsten.
  * `/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`
  * Läs dokumentationen om hello OMS-proxyn på [konfigurera hello agent för användning med en HTTP-proxyserver](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)
* Kontrollera att följande slutpunkter OMS hello är godkända

| Agentresurs | Portar |
| --- | --- |
| &#42;. ods.opinsights.Azure.com |Port 443 |
| &#42;. OMS.opinsights.Azure.com |Port 443 |
| ods.systemcenteradvisor.com |Port 443 |
| &#42;.blob.core.windows.net/ |Port 443 |

### <a name="a-403-error-is-displayed-when-onboarding"></a>Ett 403-fel visas när onboarding
#### <a name="probable-causes"></a>Troliga orsaker
* hello datum och tid är felaktiga på Linux-servern
* hello arbetsyte-ID och Arbetsytenyckel som används är felaktig

#### <a name="resolution"></a>Lösning
* Kontrollera hello tid på Linux-servern med hello `date` kommando. Om hello data är större än eller mindre än 15 minuter från hello aktuell tid, misslyckas onboarding. toocorrect, uppdatera hello datum och/eller tidszonen för Linux-servern.
* hello senaste versionen av hello OMS-Agent för Linux meddelar dig om en tidsskillnad som orsakar fel onboarding
* RE publicera med hjälp av hello rätt arbetsyte-ID och Arbetsytenyckel. Se [Onboarding hello kommandoraden](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) för mer information.

### <a name="a-500-error-or-404-error-appears-in-hello-log-file-after-onboarding"></a>Ett fel 500 eller 404-fel visas i loggfilen för hello efter onboarding
Detta är ett känt problem som uppstår under hello första uppladdning av Linux-data i en OMS-arbetsyta. Detta påverkar inte data som skickas eller andra problem. Du kan ignorera hello-fel när ursprungligen onboarding.

### <a name="nagios-data-does-not-appear-in-hello-oms-portal"></a>Nagios data visas inte i hello OMS-portalen
#### <a name="probable-causes"></a>Troliga orsaker
* Hej omsagent användaren har inte behörighet tooread från hello Nagios loggfil
* Hej Nagios källa och filter avsnitt fortfarande kommenterade i hello omsagent.conf fil

#### <a name="resolutions"></a>Lösningar
* Lägga till hello omsagent användare i ordning tooread från hello Nagios-fil. Se [Nagios aviseringar](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) för mer information.
* I hello OMS-Agent för Linux-Allmänt konfigurationsfilen vid `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, se till att **både** hello Nagios käll- och filter avsnitt har kommentarer bort, liknande toohello följande exempel.

```
<source>
  type tail
  path /var/log/nagios/nagios.log
  format none
  tag oms.nagios
</source>

<filter oms.nagios>
  type filter_nagios_log
</filter>
```


### <a name="linux-data-doesnt-appear-in-hello-oms-portal"></a>Linux-data visas inte i hello OMS-portalen
#### <a name="probable-causes"></a>Troliga orsaker
* Onboarding toohello OMS-tjänsten misslyckades
* Anslutningen toohello OMS-tjänsten är blockerad
* hello OMS-Agent för Linux-data är säkerhetskopierade

#### <a name="resolutions"></a>Lösningar
* Kontrollera att onboarding toohello OMS-tjänsten lyckades genom att verifiera att hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` finns.
* RE publicera med hjälp av hello omsadmin.sh kommandoraden. Se [Onboarding hello kommandoraden](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) för mer information.
* Om du använder en proxyserver Använd hello proxy felsökning stegen ovan
* I vissa fall när hello OMS-Agent för Linux inte kan kommunicera med hello OMS-tjänsten är data på hello Agent säkerhetskopierade toohello fullständig buffertstorlek på 50 MB. Starta om hello OMS-Agent för Linux genom att köra hello `/opt/microsoft/omsagent/bin/service_control restart` kommando.
  >[AZURE.NOTE] Det här problemet har lösts i agenten version 1.1.0-28 och senare.

### <a name="syslog-linux-performance-counter-configuration-is-not-applied-in-hello-oms-portal"></a>Syslog Linux prestandaräknaren konfigurationen tillämpas inte i hello OMS-portalen
#### <a name="probable-causes"></a>Troliga orsaker
* hello OMS-Agent för Linux-hello configuration agenten har inte hämta hello senaste konfigurationen från hello OMS-portalen.
* hello har ändrade inställningar i hello portal inte tillämpats

#### <a name="resolutions"></a>Lösningar
`omsconfig`är hello configuration agenten i hello OMS-Agent för Linux som hämtar OMS-portalen konfigurationsändringar var femte minut. Den här konfigurationen är tillämpade toohello OMS-Agent för Linux-konfigurationsfiler finns på `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.

* I vissa fall kanske inte hello OMS-Agent för Linux configuration-agenten kan toocommunicate med hello konfiguration av tjänsten, vilket resulterar i senaste konfigurationen används inte.
* Kontrollera att hello `omsconfig` agent installeras med hello följande:

  * `dpkg --list omsconfig` eller `rpm -qi omsconfig`
  * Om du inte har installerats installerar du om hello senaste versionen av hello OMS-Agent för Linux
* Kontrollera att hello `omsconfig` agenten kan kommunicera med hello OMS-tjänsten

  * Kör hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` kommando
    * hello ovanstående kommando returnerar hello configuration agentens hämtar från hello portal, inklusive inställningar för Syslog, prestandaräknare för Linux och anpassade loggar
    * Om hello kommandot ovan misslyckas, kör hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` kommando. Det här kommandot tvingar hello omsconfig agent toocommunicate med hello OMS tooretrieve hello senaste tjänstkonfigurationen.

### <a name="custom-linux-log-data-does-not-appear-in-hello-oms-portal"></a>Anpassade loggdata för Linux visas inte i hello OMS-portalen
#### <a name="probable-causes"></a>Troliga orsaker
* Onboarding tooOMS tjänsten misslyckades
* Hej **tillämpa hello följande konfiguration toomy Linux-servrar** inställningen har valts
* omsconfig har inte plockats upp hello senaste anpassad logg från hello-portalen
* Hej `omsagent` används tooaccess hello anpassad logg på grund av tooa behörighetsproblem eller `omsagent` hittades inte. I det här fallet visas hello följande utdata:
  * `[DATETIME] [warn]: file not found. Continuing without tailing it.`
  * `[DATETIME] [error]: file not accessible by omsagent.`
* Detta är ett känt problem med hello konkurrenstillstånd som fastställdes i hello OMS-Agent för Linux-version 1.1.0-217

#### <a name="resolutions"></a>Lösningar
* Kontrollera att du nu har publicerats, eller så genom att fastställa om hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` filen finns.
  * Om nödvändiga, publicera igen kommandoraden hello omsadmin.sh. Se [Onboarding hello kommandoraden](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) för mer information.
* I hello OMS-portalen under **inställningar** på hello **Data** kontrollerar att hello **tillämpa hello följande konfiguration toomy Linux-servrar** är vald  
  ![Använd konfiguration](./media/log-analytics-linux-agents/customloglinuxenabled.png)
* Kontrollera att hello `omsconfig` agenten kan kommunicera med hello OMS-tjänsten

  * Kör hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` kommando
  * hello ovanstående kommando returnerar hello configuration agentens hämtar från hello Portal, inklusive inställningar för Syslog, prestandaräknare för Linux och anpassade loggar
  * Om hello kommandot ovan misslyckas, kör hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` kommando. Det här kommandot tvingar hello omsconfig agent toocommunicate med OMS-tjänsten och hämta senaste hello-konfigurationen.

I stället för hello OMS-Agent för Linux-användare som körs som en privilegierad användare `root`, hello OMS-Agent för Linux körs som hello `omsagent` användare. I de flesta fall måste vara explicit behörighet beviljas toohello användare i ordning tooread vissa filer.

toogrant behörighet för`omsagent` användaren som kör hello följande kommandon:

1. Lägg till hello `omsagent` tooa specifik användargrupp med`sudo usermod -a -G <GROUPNAME> <USERNAME>`
2. Bevilja läsbehörighet universal toohello krävs fil med`sudo chmod -R ugo+rw <FILE DIRECTORY>`

Det finns ett känt problem med hello konkurrenstillstånd som fastställdes i hello OMS-Agent för Linux-version 1.1.0-217. Kör följande kommando tooget hello senaste versionen av utdata-plugin-programmet hello hello när du har uppdaterat toohello senaste agenten:

```
sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf
```

## <a name="known-limitations"></a>Kända begränsningar
Granska följande avsnitt toolearn om aktuella begränsningar av hello OMS-Agent för Linux hello.

### <a name="azure-diagnostics"></a>Azure Diagnostics
För Linux virtuella datorer som körs i Azure, kan ytterligare steg vara nödvändiga tooallow insamling av Azure-diagnostik och Operations Management Suite. **Version 2.2** av hello diagnostik tillägget för Linux krävs för kompatibilitet med hello OMS-Agent för Linux.

Mer information om installation och konfiguration hello diagnostiska tillägget för Linux finns [använda hello Azure CLI kommandot tooenable Linux diagnostiska tillägget](../virtual-machines/linux/classic/diagnostic-extension-v2.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).

**Uppgradera hello Linux diagnostik tillägg från 2.0 too2.2 Azure CLI ASM:**

```
azure vm extension set -u <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.0
azure vm extension set <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

**ARM**

```
azure vm extension set -u <resource-group> <vm-name> Microsoft.Insights.VMDiagnosticsSettings Microsoft.OSTCExtensions 2.0
azure vm extension set <resource-group> <vm-name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

Dessa kommandoexempel referera till en fil med namnet PrivateConfig.json. hello-formatet för filen bör likna följande exempel hello.

```
    {
    "storageAccountName":"hello storage account tooreceive data",
    "storageAccountKey":"hello key of hello account"
    }
```

### <a name="sysklog-is-not-supported"></a>Sysklog stöds inte
Rsyslog eller syslog-ng är obligatoriska toocollect syslog-meddelanden. hello standard syslog-daemon på version 5 Red Hat Enterprise Linux, CentOS och Oracle Linux-version (sysklog) stöds inte för insamling av syslog-händelser. toocollect syslog-data från den här versionen av dessa distributioner hello rsyslog daemon ska vara installerat och konfigurerat tooreplace sysklog. Läs mer om att ersätta sysklog med rsyslog [installera hello som nyligen skapats rsyslog RPM](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).

## <a name="next-steps"></a>Nästa steg
* [Lägg till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md) tooadd funktioner och samla data.
* Bekanta dig med [logga sökningar](log-analytics-log-searches.md) tooview detaljerad information som samlas in av lösningar.
* Använd [instrumentpaneler](log-analytics-dashboards.md) toosave och visa dina egna anpassade söker.
