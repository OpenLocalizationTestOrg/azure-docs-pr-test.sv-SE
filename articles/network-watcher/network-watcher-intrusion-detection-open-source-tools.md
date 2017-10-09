---
title: "aaaPerform nätverk intrångsidentifiering med öppen källkod verktyg och Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse Azure Nätverksbevakaren och öppen källkod verktyg tooperform nätverks intrångsidentifiering"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 0f043f08-19e1-4125-98b0-3e335ba69681
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b5a909b827ab32ad6b2fd8e2911a944fd940249e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="perform-network-intrusion-detection-with-network-watcher-and-open-source-tools"></a>Utföra nätverket intrångsidentifiering med öppen källkod verktyg och Nätverksbevakaren

Paketet insamlingar är en viktig del för att implementera nätverk intrång identifiering (ID) och utföra Network Security övervakning (NSM). Det finns flera ID: N verktyg för öppen källkod som bearbetar paket insamlingar och leta efter signaturer för intrång i nätverket och skadliga aktiviteter. Du kan analysera nätverket för skadliga intrång eller säkerhetsproblem med hello paket insamlingar som tillhandahålls av Nätverksbevakaren.

Ett sådant öppen källkod verktyg är Suricata, en ID-motor som använder regeluppsättningar toomonitor nätverkstrafik och utlöser aviseringar när misstänkta händelser inträffar. Suricata erbjuder ett flertrådat motorn, vilket innebär att den kan utföra nätverkstrafikanalysen med ökad hastighet och effektivitet. Mer information om Suricata och dess funktioner finns i deras webbplats på https://suricata-ids.org/.

## <a name="scenario"></a>Scenario

Den här artikeln förklarar hur tooset upp din miljö tooperform nätverk intrångsidentifiering med Nätverksbevakaren Suricata, och hello elastisk stacken. Nätverksbevakaren ger du hello paketet samlar in används tooperform nätverk intrångsidentifiering. Suricata processer hello paket avbildas och utlösare aviseringar baserat på paket som matchar dess given regeluppsättning hot. Dessa aviseringar som lagras i en loggfil på den lokala datorn. Med hello elastisk Stack hello-loggar som genereras av Suricata kan indexeras och används toocreate en Kibana instrumentpanel som ger dig en bild av hello loggar och en innebär tooquickly få insikter toopotential nätverk säkerhetsrisker.  

![enkel web application scenario][1]

Båda verktyg med öppen källkod kan ställas in på en virtuell dator i Azure, så att du tooperform analysen i din egen Azure nätverksmiljö.

## <a name="steps"></a>Steg

### <a name="install-suricata"></a>Installera Suricata

Alla andra metoder för installation finns i http://suricata.readthedocs.io/en/latest/install.html

1. Kör följande kommandon hello i hello kommandoradsverktyget terminal på den virtuella datorn:

    ```
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo sudo apt-get install suricata
    ```

1. tooverify installationen kör hello kommando `suricata -h` toosee hello fullständig lista över kommandon.

### <a name="download-hello-emerging-threats-ruleset"></a>Hämta hello nya hot RuleSet-metod

I det här skedet har vi inte några regler för Suricata toorun. Du kan skapa dina egna regler om det finns specifika hot tooyour som toodetect eller du kan också använda utvecklats uppsättningar från ett antal leverantörer, till exempel nya hot eller VRT regler från Snort. Vi använder hello fritt tillgänglig nya hot ruleset här:

Hämta hello regeluppsättning och kopiera dem till hello-katalogen:

```
wget http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
tar zxf emerging.rules.tar.gz
sudo cp -r rules /etc/suricata/
```

### <a name="process-packet-captures-with-suricata"></a>Processen paket fångar med Suricata

tooprocess paket fångar med Suricata, kör hello följande kommando:

```
sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
```
toocheck hello resulterande aviseringar läsa hello fast.log filen:
```
tail -f /var/log/suricata/fast.log
```

### <a name="set-up-hello-elastic-stack"></a>Ställ in hello elastisk Stack

Hello-loggar som producerar Suricata innehåller värdefull information om vad som händer på vårt nätverk, dessa loggfiler inte hello enklaste tooread och förstå. Genom att ansluta Suricata med hello elastisk Stack kan vi skapa en instrumentpanel för Kibana vad gör att vi toosearch, kurva, analysera och härleda insikter från våra loggar.

#### <a name="install-elasticsearch"></a>Installera Elasticsearch

1. hello elastisk Stack version 5.0 och senare kräver Java 8. Kör kommandot hello `java -version` toocheck din version. Om du inte har java installera, se toodocumentation på [Oracles webbplats](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)
1. Hämta hello rätt binära paket för systemet:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    Andra installationsmetoder finns på [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)

1. Kontrollera att Elasticsearch körs med hello-kommando:

    ```
    curl http://127.0.0.1:9200
    ```

    Du bör se en liknande toothis svar:

    ```
    {
    "name" : "Angela Del Toro",
    "cluster_name" : "elasticsearch",
    "version" : {
        "number" : "5.2.0",
        "build_hash" : "8ff36d139e16f8720f2947ef62c8167a888992fe",
        "build_timestamp" : "2016-01-27T13:32:39Z",
        "build_snapshot" : false,
        "lucene_version" : "6.1.0"
    },
    "tagline" : "You Know, for Search"
    }
    ```

Ytterligare information om installation elastisk Sök finns toohello sidan [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)

### <a name="install-logstash"></a>Installera Logstash

1. tooinstall Logstash kör hello följande kommandon:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. Vi behöver bredvid tooconfigure Logstash tooread från hello utdata från eve.json-filen. Skapa filen logstash.conf med:

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. Lägg till följande innehåll toohello hello (Kontrollera att hello sökvägen toohello eve.json filen är korrekt):

    ```ruby
    input {
    file {
        path => ["/var/log/suricata/eve.json"]
        codec =>  "json"
        type => "SuricataIDPS"
    }

    }

    filter {
    if [type] == "SuricataIDPS" {
        date {
        match => [ "timestamp", "ISO8601" ]
        }
        ruby {
        code => "
            if event.get('[event_type]') == 'fileinfo'
            event.set('[fileinfo][type]', event.get('[fileinfo][magic]').to_s.split(',')[0])
            end
        "
        }

        ruby{
        code => "
            if event.get('[event_type]') == 'alert'
            sp = event.get('[alert][signature]').to_s.split(' group ')
            if (sp.length == 2) and /\A\d+\z/.match(sp[1])
                event.set('[alert][signature]', sp[0])
            end
            end
            "
        }
    }

    if [src_ip]  {
        geoip {
        source => "src_ip"
        target => "geoip"
        #database => "/opt/logstash/vendor/geoip/GeoLiteCity.dat"
        add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
        add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
        }
        mutate {
        convert => [ "[geoip][coordinates]", "float" ]
        }
        if ![geoip.ip] {
        if [dest_ip]  {
            geoip {
            source => "dest_ip"
            target => "geoip"
            #database => "/opt/logstash/vendor/geoip/GeoLiteCity.dat"
            add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
            add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
            }
            mutate {
            convert => [ "[geoip][coordinates]", "float" ]
            }
        }
        }
    }
    }

    output {
    elasticsearch {
        hosts => "localhost"
    }
    }
    ```

1. Kontrollera att toogive hello behörigheter toohello eve.json filen så att Logstash kan mata in hello-filen.
    
    ```
    sudo chmod 775 /var/log/suricata/eve.json
    ```

1. toostart Logstash kör hello-kommando:

    ```
    sudo /etc/init.d/logstash start
    ```

Ytterligare instruktioner om hur du installerar Logstash finns toohello [officiella dokumentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)

### <a name="install-kibana"></a>Installera Kibana

1. Kör följande kommandon tooinstall Kibana hello:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
    tar xzvf kibana-5.2.0-linux-x86_64.tar.gz

    ```
1. toorun Kibana använda hello-kommandon:

    ```
    cd kibana-5.2.0-linux-x86_64/
    ./bin/kibana
    ```

1. tooview webbplatsen Kibana gränssnitt, navigera för`http://localhost:5601`
1. I det här scenariot hello index mönstret för hello Suricata loggar är ”logstash-*”

1. Om du vill tooview hello Kibana instrumentpanel via fjärranslutning, skapar du en inkommande NSG regel för att tillåta åtkomst för**port 5601**.

### <a name="create-a-kibana-dashboard"></a>Skapa en Kibana instrumentpanel

Vi har angett en exempelinstrumentpanel du tooview trender och information i aviseringar för den här artikeln.

1. Hämta hello instrumentpanelen filen [här](https://aka.ms/networkwatchersuricatadashboard), hello visualiseringen filen [här](https://aka.ms/networkwatchersuricatavisualization), och hello sparade sökningen [här](https://aka.ms/networkwatchersuricatasavedsearch).

1. Under hello **Management** fliken av Kibana, navigera för**sparade objekt** och importera alla tre filerna. Sedan från hello **instrumentpanelen** fliken kan du öppna och läsa in hello exempel på en instrumentpanel.

Du kan också skapa egna visualiseringar och instrumentpaneler som skräddarsys mot mätvärden för ditt eget intresse. Läs mer om hur du skapar Kibana visualiseringar från Kibana's [officiella dokumentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).

![kibana instrumentpanelen][2]

### <a name="visualize-ids-alert-logs"></a>Visualisera ID avisering loggar

exemplet på instrumentpanel med hello innehåller flera visualiseringar hello Suricata avisering loggar:

1. Aviseringar av GeoIP – en karta med hello distribution av aviseringar för sina ursprung baserat på geografisk plats (bestäms av IP)

    ![GEO-ip][3]

1. Främsta 10 aviseringar – en översikt över hello 10 vanligaste utlöst aviseringar och deras beskrivning. Klicka på en enskild varning filtrerar ned hello instrumentpanelen toohello information som rör toothat specifika aviseringen.

    ![bild 4][4]

1. Antal aviseringar – hello Totalt antal aviseringar som har utlösts av hello RuleSet-metod

    ![bild 5][5]

1. De 20 största källan/målet IP-adresser portar - cirkeldiagram visar hello översta 20 IP-adresser och portar som aviseringar har utlösts av. Du kan filtrera ned på specifika IP-adresser portar toosee hur många och vilka typer av aviseringar aktiveras.

    ![bild 6][6]

1. Aviseringen sammanfattning – en tabell sammanfattar närmare för varje enskild varning. Du kan anpassa den här tabellen tooshow andra parametrar av intresse för varje avisering.

    ![Bild 7][7]

Mer dokumentation om hur du skapar anpassade visualiseringar och instrumentpaneler finns [Kibanas officiella dokumentation](https://www.elastic.co/guide/en/kibana/current/introduction.html).

## <a name="conclusion"></a>Slutsats

Samlar in angivna genom Nätverksbevakaren och verktyg för öppen källkod-ID: N som Suricata, du kan utföra nätverket intrångsidentifiering för en mängd olika hot genom att kombinera paket. Instrumentpanelerna kan du tooquickly upptäcka trender och inkonsekvenser i nätverket, samt prova hello data toodiscover rotorsaken till aviseringar, till exempel skadliga användaragenter eller sårbara portarna. Med den här extraherade data kan fatta välgrundade beslut om hur tooreact tooand skydda nätverket från skadliga intrångsförsök och skapa regler tooprevent framtida intrång tooyour nätverk.

## <a name="next-steps"></a>Nästa steg

Lär dig hur tootrigger paket samlar in baserat på aviseringar genom att besöka [använda paketet avbilda toodo proaktiv nätverksövervakning med Azure Functions](network-watcher-alert-triggered-packet-capture.md)

Lär dig hur toovisualize NSG-flöde loggar med Power BI genom att besöka [visualisera NSG flödar loggar med Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)



<!-- images -->
[1]: ./media/network-watcher-intrusion-detection-open-source-tools/figure1.png
[2]: ./media/network-watcher-intrusion-detection-open-source-tools/figure2.png
[3]: ./media/network-watcher-intrusion-detection-open-source-tools/figure3.png
[4]: ./media/network-watcher-intrusion-detection-open-source-tools/figure4.png
[5]: ./media/network-watcher-intrusion-detection-open-source-tools/figure5.png
[6]: ./media/network-watcher-intrusion-detection-open-source-tools/figure6.png
[7]: ./media/network-watcher-intrusion-detection-open-source-tools/figure7.png
