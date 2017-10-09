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
# <a name="perform-network-intrusion-detection-with-network-watcher-and-open-source-tools"></a><span data-ttu-id="3cc43-103">Utföra nätverket intrångsidentifiering med öppen källkod verktyg och Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="3cc43-103">Perform network intrusion detection with Network Watcher and open source tools</span></span>

<span data-ttu-id="3cc43-104">Paketet insamlingar är en viktig del för att implementera nätverk intrång identifiering (ID) och utföra Network Security övervakning (NSM).</span><span class="sxs-lookup"><span data-stu-id="3cc43-104">Packet captures are a key component for implementing network intrusion detection systems (IDS) and performing Network Security Monitoring (NSM).</span></span> <span data-ttu-id="3cc43-105">Det finns flera ID: N verktyg för öppen källkod som bearbetar paket insamlingar och leta efter signaturer för intrång i nätverket och skadliga aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3cc43-105">There are several open source IDS tools that process packet captures and look for signatures of possible network intrusions and malicious activity.</span></span> <span data-ttu-id="3cc43-106">Du kan analysera nätverket för skadliga intrång eller säkerhetsproblem med hello paket insamlingar som tillhandahålls av Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="3cc43-106">Using hello packet captures provided by Network Watcher, you can analyze your network for any harmful intrusions or vulnerabilities.</span></span>

<span data-ttu-id="3cc43-107">Ett sådant öppen källkod verktyg är Suricata, en ID-motor som använder regeluppsättningar toomonitor nätverkstrafik och utlöser aviseringar när misstänkta händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="3cc43-107">One such open source tool is Suricata, an IDS engine that uses rulesets toomonitor network traffic and triggers alerts whenever suspicious events occur.</span></span> <span data-ttu-id="3cc43-108">Suricata erbjuder ett flertrådat motorn, vilket innebär att den kan utföra nätverkstrafikanalysen med ökad hastighet och effektivitet.</span><span class="sxs-lookup"><span data-stu-id="3cc43-108">Suricata offers a multi-threaded engine, meaning it can perform network traffic analysis with increased speed and efficiency.</span></span> <span data-ttu-id="3cc43-109">Mer information om Suricata och dess funktioner finns i deras webbplats på https://suricata-ids.org/.</span><span class="sxs-lookup"><span data-stu-id="3cc43-109">For more details about Suricata and its capabilities, visit their website at https://suricata-ids.org/.</span></span>

## <a name="scenario"></a><span data-ttu-id="3cc43-110">Scenario</span><span class="sxs-lookup"><span data-stu-id="3cc43-110">Scenario</span></span>

<span data-ttu-id="3cc43-111">Den här artikeln förklarar hur tooset upp din miljö tooperform nätverk intrångsidentifiering med Nätverksbevakaren Suricata, och hello elastisk stacken.</span><span class="sxs-lookup"><span data-stu-id="3cc43-111">This article explains how tooset up your environment tooperform network intrusion detection using Network Watcher, Suricata, and hello Elastic Stack.</span></span> <span data-ttu-id="3cc43-112">Nätverksbevakaren ger du hello paketet samlar in används tooperform nätverk intrångsidentifiering.</span><span class="sxs-lookup"><span data-stu-id="3cc43-112">Network Watcher provides you with hello packet captures used tooperform network intrusion detection.</span></span> <span data-ttu-id="3cc43-113">Suricata processer hello paket avbildas och utlösare aviseringar baserat på paket som matchar dess given regeluppsättning hot.</span><span class="sxs-lookup"><span data-stu-id="3cc43-113">Suricata processes hello packet captures and trigger alerts based on packets that match its given ruleset of threats.</span></span> <span data-ttu-id="3cc43-114">Dessa aviseringar som lagras i en loggfil på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="3cc43-114">These alerts are stored in a log file on your local machine.</span></span> <span data-ttu-id="3cc43-115">Med hello elastisk Stack hello-loggar som genereras av Suricata kan indexeras och används toocreate en Kibana instrumentpanel som ger dig en bild av hello loggar och en innebär tooquickly få insikter toopotential nätverk säkerhetsrisker.</span><span class="sxs-lookup"><span data-stu-id="3cc43-115">Using hello Elastic Stack, hello logs generated by Suricata can be indexed and used toocreate a Kibana dashboard, providing you with a visual representation of hello logs and a means tooquickly gain insights toopotential network vulnerabilities.</span></span>  

![enkel web application scenario][1]

<span data-ttu-id="3cc43-117">Båda verktyg med öppen källkod kan ställas in på en virtuell dator i Azure, så att du tooperform analysen i din egen Azure nätverksmiljö.</span><span class="sxs-lookup"><span data-stu-id="3cc43-117">Both open source tools can be set up on an Azure VM, allowing you tooperform this analysis within your own Azure network environment.</span></span>

## <a name="steps"></a><span data-ttu-id="3cc43-118">Steg</span><span class="sxs-lookup"><span data-stu-id="3cc43-118">Steps</span></span>

### <a name="install-suricata"></a><span data-ttu-id="3cc43-119">Installera Suricata</span><span class="sxs-lookup"><span data-stu-id="3cc43-119">Install Suricata</span></span>

<span data-ttu-id="3cc43-120">Alla andra metoder för installation finns i http://suricata.readthedocs.io/en/latest/install.html</span><span class="sxs-lookup"><span data-stu-id="3cc43-120">For all other methods of installation, visit http://suricata.readthedocs.io/en/latest/install.html</span></span>

1. <span data-ttu-id="3cc43-121">Kör följande kommandon hello i hello kommandoradsverktyget terminal på den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="3cc43-121">In hello command-line terminal of your VM run hello following commands:</span></span>

    ```
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo sudo apt-get install suricata
    ```

1. <span data-ttu-id="3cc43-122">tooverify installationen kör hello kommando `suricata -h` toosee hello fullständig lista över kommandon.</span><span class="sxs-lookup"><span data-stu-id="3cc43-122">tooverify your installation, run hello command `suricata -h` toosee hello full list of commands.</span></span>

### <a name="download-hello-emerging-threats-ruleset"></a><span data-ttu-id="3cc43-123">Hämta hello nya hot RuleSet-metod</span><span class="sxs-lookup"><span data-stu-id="3cc43-123">Download hello Emerging Threats ruleset</span></span>

<span data-ttu-id="3cc43-124">I det här skedet har vi inte några regler för Suricata toorun.</span><span class="sxs-lookup"><span data-stu-id="3cc43-124">At this stage, we do not have any rules for Suricata toorun.</span></span> <span data-ttu-id="3cc43-125">Du kan skapa dina egna regler om det finns specifika hot tooyour som toodetect eller du kan också använda utvecklats uppsättningar från ett antal leverantörer, till exempel nya hot eller VRT regler från Snort.</span><span class="sxs-lookup"><span data-stu-id="3cc43-125">You can create your own rules if there are specific threats tooyour network you would like toodetect, or you can also use developed rule sets from a number of providers, such as Emerging Threats, or VRT rules from Snort.</span></span> <span data-ttu-id="3cc43-126">Vi använder hello fritt tillgänglig nya hot ruleset här:</span><span class="sxs-lookup"><span data-stu-id="3cc43-126">We use hello freely accessible Emerging Threats ruleset here:</span></span>

<span data-ttu-id="3cc43-127">Hämta hello regeluppsättning och kopiera dem till hello-katalogen:</span><span class="sxs-lookup"><span data-stu-id="3cc43-127">Download hello rule set and copy them into hello directory:</span></span>

```
wget http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
tar zxf emerging.rules.tar.gz
sudo cp -r rules /etc/suricata/
```

### <a name="process-packet-captures-with-suricata"></a><span data-ttu-id="3cc43-128">Processen paket fångar med Suricata</span><span class="sxs-lookup"><span data-stu-id="3cc43-128">Process packet captures with Suricata</span></span>

<span data-ttu-id="3cc43-129">tooprocess paket fångar med Suricata, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3cc43-129">tooprocess packet captures using Suricata, run hello following command:</span></span>

```
sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
```
<span data-ttu-id="3cc43-130">toocheck hello resulterande aviseringar läsa hello fast.log filen:</span><span class="sxs-lookup"><span data-stu-id="3cc43-130">toocheck hello resulting alerts, read hello fast.log file:</span></span>
```
tail -f /var/log/suricata/fast.log
```

### <a name="set-up-hello-elastic-stack"></a><span data-ttu-id="3cc43-131">Ställ in hello elastisk Stack</span><span class="sxs-lookup"><span data-stu-id="3cc43-131">Set up hello Elastic Stack</span></span>

<span data-ttu-id="3cc43-132">Hello-loggar som producerar Suricata innehåller värdefull information om vad som händer på vårt nätverk, dessa loggfiler inte hello enklaste tooread och förstå.</span><span class="sxs-lookup"><span data-stu-id="3cc43-132">While hello logs that Suricata produces contain valuable information about what’s happening on our network, these log files aren’t hello easiest tooread and understand.</span></span> <span data-ttu-id="3cc43-133">Genom att ansluta Suricata med hello elastisk Stack kan vi skapa en instrumentpanel för Kibana vad gör att vi toosearch, kurva, analysera och härleda insikter från våra loggar.</span><span class="sxs-lookup"><span data-stu-id="3cc43-133">By connecting Suricata with hello Elastic Stack, we can create a Kibana dashboard what allows us toosearch, graph, analyze, and derive insights from our logs.</span></span>

#### <a name="install-elasticsearch"></a><span data-ttu-id="3cc43-134">Installera Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="3cc43-134">Install Elasticsearch</span></span>

1. <span data-ttu-id="3cc43-135">hello elastisk Stack version 5.0 och senare kräver Java 8.</span><span class="sxs-lookup"><span data-stu-id="3cc43-135">hello Elastic Stack from version 5.0 and above requires Java 8.</span></span> <span data-ttu-id="3cc43-136">Kör kommandot hello `java -version` toocheck din version.</span><span class="sxs-lookup"><span data-stu-id="3cc43-136">Run hello command `java -version` toocheck your version.</span></span> <span data-ttu-id="3cc43-137">Om du inte har java installera, se toodocumentation på [Oracles webbplats](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span><span class="sxs-lookup"><span data-stu-id="3cc43-137">If you do not have java install, refer toodocumentation on [Oracle's website](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span></span>
1. <span data-ttu-id="3cc43-138">Hämta hello rätt binära paket för systemet:</span><span class="sxs-lookup"><span data-stu-id="3cc43-138">Download hello correct binary package for your system:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    <span data-ttu-id="3cc43-139">Andra installationsmetoder finns på [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span><span class="sxs-lookup"><span data-stu-id="3cc43-139">Other installation methods can be found at [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span></span>

1. <span data-ttu-id="3cc43-140">Kontrollera att Elasticsearch körs med hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="3cc43-140">Verify that Elasticsearch is running with hello command:</span></span>

    ```
    curl http://127.0.0.1:9200
    ```

    <span data-ttu-id="3cc43-141">Du bör se en liknande toothis svar:</span><span class="sxs-lookup"><span data-stu-id="3cc43-141">You should see a response similar toothis:</span></span>

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

<span data-ttu-id="3cc43-142">Ytterligare information om installation elastisk Sök finns toohello sidan [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span><span class="sxs-lookup"><span data-stu-id="3cc43-142">For further instructions on installing Elastic search, refer toohello page [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span></span>

### <a name="install-logstash"></a><span data-ttu-id="3cc43-143">Installera Logstash</span><span class="sxs-lookup"><span data-stu-id="3cc43-143">Install Logstash</span></span>

1. <span data-ttu-id="3cc43-144">tooinstall Logstash kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3cc43-144">tooinstall Logstash run hello following commands:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. <span data-ttu-id="3cc43-145">Vi behöver bredvid tooconfigure Logstash tooread från hello utdata från eve.json-filen.</span><span class="sxs-lookup"><span data-stu-id="3cc43-145">Next we need tooconfigure Logstash tooread from hello output of eve.json file.</span></span> <span data-ttu-id="3cc43-146">Skapa filen logstash.conf med:</span><span class="sxs-lookup"><span data-stu-id="3cc43-146">Create a logstash.conf file using:</span></span>

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. <span data-ttu-id="3cc43-147">Lägg till följande innehåll toohello hello (Kontrollera att hello sökvägen toohello eve.json filen är korrekt):</span><span class="sxs-lookup"><span data-stu-id="3cc43-147">Add hello following content toohello file (make sure that hello path toohello eve.json file is correct):</span></span>

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

1. <span data-ttu-id="3cc43-148">Kontrollera att toogive hello behörigheter toohello eve.json filen så att Logstash kan mata in hello-filen.</span><span class="sxs-lookup"><span data-stu-id="3cc43-148">Make sure toogive hello correct permissions toohello eve.json file so that Logstash can ingest hello file.</span></span>
    
    ```
    sudo chmod 775 /var/log/suricata/eve.json
    ```

1. <span data-ttu-id="3cc43-149">toostart Logstash kör hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="3cc43-149">toostart Logstash run hello command:</span></span>

    ```
    sudo /etc/init.d/logstash start
    ```

<span data-ttu-id="3cc43-150">Ytterligare instruktioner om hur du installerar Logstash finns toohello [officiella dokumentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span><span class="sxs-lookup"><span data-stu-id="3cc43-150">For further instructions on installing Logstash, refer toohello [official documentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span></span>

### <a name="install-kibana"></a><span data-ttu-id="3cc43-151">Installera Kibana</span><span class="sxs-lookup"><span data-stu-id="3cc43-151">Install Kibana</span></span>

1. <span data-ttu-id="3cc43-152">Kör följande kommandon tooinstall Kibana hello:</span><span class="sxs-lookup"><span data-stu-id="3cc43-152">Run hello following commands tooinstall Kibana:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
    tar xzvf kibana-5.2.0-linux-x86_64.tar.gz

    ```
1. <span data-ttu-id="3cc43-153">toorun Kibana använda hello-kommandon:</span><span class="sxs-lookup"><span data-stu-id="3cc43-153">toorun Kibana use hello commands:</span></span>

    ```
    cd kibana-5.2.0-linux-x86_64/
    ./bin/kibana
    ```

1. <span data-ttu-id="3cc43-154">tooview webbplatsen Kibana gränssnitt, navigera för`http://localhost:5601`</span><span class="sxs-lookup"><span data-stu-id="3cc43-154">tooview your Kibana web interface, navigate too`http://localhost:5601`</span></span>
1. <span data-ttu-id="3cc43-155">I det här scenariot hello index mönstret för hello Suricata loggar är ”logstash-*”</span><span class="sxs-lookup"><span data-stu-id="3cc43-155">For this scenario, hello index pattern used for hello Suricata logs is "logstash-*"</span></span>

1. <span data-ttu-id="3cc43-156">Om du vill tooview hello Kibana instrumentpanel via fjärranslutning, skapar du en inkommande NSG regel för att tillåta åtkomst för**port 5601**.</span><span class="sxs-lookup"><span data-stu-id="3cc43-156">If you want tooview hello Kibana dashboard remotely, create an inbound NSG rule allowing access too**port 5601**.</span></span>

### <a name="create-a-kibana-dashboard"></a><span data-ttu-id="3cc43-157">Skapa en Kibana instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="3cc43-157">Create a Kibana dashboard</span></span>

<span data-ttu-id="3cc43-158">Vi har angett en exempelinstrumentpanel du tooview trender och information i aviseringar för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="3cc43-158">For this article, we have provided a sample dashboard for you tooview trends and details in your alerts.</span></span>

1. <span data-ttu-id="3cc43-159">Hämta hello instrumentpanelen filen [här](https://aka.ms/networkwatchersuricatadashboard), hello visualiseringen filen [här](https://aka.ms/networkwatchersuricatavisualization), och hello sparade sökningen [här](https://aka.ms/networkwatchersuricatasavedsearch).</span><span class="sxs-lookup"><span data-stu-id="3cc43-159">Download hello dashboard file [here](https://aka.ms/networkwatchersuricatadashboard), hello visualization file [here](https://aka.ms/networkwatchersuricatavisualization), and hello saved search file [here](https://aka.ms/networkwatchersuricatasavedsearch).</span></span>

1. <span data-ttu-id="3cc43-160">Under hello **Management** fliken av Kibana, navigera för**sparade objekt** och importera alla tre filerna.</span><span class="sxs-lookup"><span data-stu-id="3cc43-160">Under hello **Management** tab of Kibana, navigate too**Saved Objects** and import all three files.</span></span> <span data-ttu-id="3cc43-161">Sedan från hello **instrumentpanelen** fliken kan du öppna och läsa in hello exempel på en instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="3cc43-161">Then from hello **Dashboard** tab you can open and load hello sample dashboard.</span></span>

<span data-ttu-id="3cc43-162">Du kan också skapa egna visualiseringar och instrumentpaneler som skräddarsys mot mätvärden för ditt eget intresse.</span><span class="sxs-lookup"><span data-stu-id="3cc43-162">You can also create your own visualizations and dashboards tailored towards metrics of your own interest.</span></span> <span data-ttu-id="3cc43-163">Läs mer om hur du skapar Kibana visualiseringar från Kibana's [officiella dokumentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span><span class="sxs-lookup"><span data-stu-id="3cc43-163">Read more about creating Kibana visualizations from Kibana's [official documentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span></span>

![kibana instrumentpanelen][2]

### <a name="visualize-ids-alert-logs"></a><span data-ttu-id="3cc43-165">Visualisera ID avisering loggar</span><span class="sxs-lookup"><span data-stu-id="3cc43-165">Visualize IDS alert logs</span></span>

<span data-ttu-id="3cc43-166">exemplet på instrumentpanel med hello innehåller flera visualiseringar hello Suricata avisering loggar:</span><span class="sxs-lookup"><span data-stu-id="3cc43-166">hello sample dashboard provides several visualizations of hello Suricata alert logs:</span></span>

1. <span data-ttu-id="3cc43-167">Aviseringar av GeoIP – en karta med hello distribution av aviseringar för sina ursprung baserat på geografisk plats (bestäms av IP)</span><span class="sxs-lookup"><span data-stu-id="3cc43-167">Alerts by GeoIP – a map showing hello distribution of alerts by their country of origin based on geographic location (determined by IP)</span></span>

    ![GEO-ip][3]

1. <span data-ttu-id="3cc43-169">Främsta 10 aviseringar – en översikt över hello 10 vanligaste utlöst aviseringar och deras beskrivning.</span><span class="sxs-lookup"><span data-stu-id="3cc43-169">Top 10 Alerts – a summary of hello 10 most frequent triggered alerts and their description.</span></span> <span data-ttu-id="3cc43-170">Klicka på en enskild varning filtrerar ned hello instrumentpanelen toohello information som rör toothat specifika aviseringen.</span><span class="sxs-lookup"><span data-stu-id="3cc43-170">Clicking an individual alert filters down hello dashboard toohello information pertaining toothat specific alert.</span></span>

    ![bild 4][4]

1. <span data-ttu-id="3cc43-172">Antal aviseringar – hello Totalt antal aviseringar som har utlösts av hello RuleSet-metod</span><span class="sxs-lookup"><span data-stu-id="3cc43-172">Number of Alerts – hello total count of alerts triggered by hello ruleset</span></span>

    ![bild 5][5]

1. <span data-ttu-id="3cc43-174">De 20 största källan/målet IP-adresser portar - cirkeldiagram visar hello översta 20 IP-adresser och portar som aviseringar har utlösts av.</span><span class="sxs-lookup"><span data-stu-id="3cc43-174">Top 20 Source/Destination IPs/Ports - pie charts showing hello top 20 IPs and ports that alerts were triggered on.</span></span> <span data-ttu-id="3cc43-175">Du kan filtrera ned på specifika IP-adresser portar toosee hur många och vilka typer av aviseringar aktiveras.</span><span class="sxs-lookup"><span data-stu-id="3cc43-175">You can filter down on specific IPs/ports toosee how many and what kind of alerts are being triggered.</span></span>

    ![bild 6][6]

1. <span data-ttu-id="3cc43-177">Aviseringen sammanfattning – en tabell sammanfattar närmare för varje enskild varning.</span><span class="sxs-lookup"><span data-stu-id="3cc43-177">Alert Summary – a table summarizing specific details of each individual alert.</span></span> <span data-ttu-id="3cc43-178">Du kan anpassa den här tabellen tooshow andra parametrar av intresse för varje avisering.</span><span class="sxs-lookup"><span data-stu-id="3cc43-178">You can customize this table tooshow other parameters of interest for each alert.</span></span>

    ![Bild 7][7]

<span data-ttu-id="3cc43-180">Mer dokumentation om hur du skapar anpassade visualiseringar och instrumentpaneler finns [Kibanas officiella dokumentation](https://www.elastic.co/guide/en/kibana/current/introduction.html).</span><span class="sxs-lookup"><span data-stu-id="3cc43-180">For more documentation on creating custom visualizations and dashboards, see [Kibana’s official documentation](https://www.elastic.co/guide/en/kibana/current/introduction.html).</span></span>

## <a name="conclusion"></a><span data-ttu-id="3cc43-181">Slutsats</span><span class="sxs-lookup"><span data-stu-id="3cc43-181">Conclusion</span></span>

<span data-ttu-id="3cc43-182">Samlar in angivna genom Nätverksbevakaren och verktyg för öppen källkod-ID: N som Suricata, du kan utföra nätverket intrångsidentifiering för en mängd olika hot genom att kombinera paket.</span><span class="sxs-lookup"><span data-stu-id="3cc43-182">By combining packet captures provided by Network Watcher and open source IDS tools such as Suricata, you can perform network intrusion detection for a wide range of threats.</span></span> <span data-ttu-id="3cc43-183">Instrumentpanelerna kan du tooquickly upptäcka trender och inkonsekvenser i nätverket, samt prova hello data toodiscover rotorsaken till aviseringar, till exempel skadliga användaragenter eller sårbara portarna.</span><span class="sxs-lookup"><span data-stu-id="3cc43-183">These dashboards allow you tooquickly spot trends and anomalies within your network, as well dig into hello data toodiscover root causes of alerts such as malicious user agents or vulnerable ports.</span></span> <span data-ttu-id="3cc43-184">Med den här extraherade data kan fatta välgrundade beslut om hur tooreact tooand skydda nätverket från skadliga intrångsförsök och skapa regler tooprevent framtida intrång tooyour nätverk.</span><span class="sxs-lookup"><span data-stu-id="3cc43-184">With this extracted data, you can make informed decisions on how tooreact tooand protect your network from any harmful intrusion attempts, and create rules tooprevent future intrusions tooyour network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3cc43-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3cc43-185">Next steps</span></span>

<span data-ttu-id="3cc43-186">Lär dig hur tootrigger paket samlar in baserat på aviseringar genom att besöka [använda paketet avbilda toodo proaktiv nätverksövervakning med Azure Functions](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="3cc43-186">Learn how tootrigger packet captures based on alerts by visiting [Use packet capture toodo proactive network monitoring with Azure Functions](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="3cc43-187">Lär dig hur toovisualize NSG-flöde loggar med Power BI genom att besöka [visualisera NSG flödar loggar med Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="3cc43-187">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>



<!-- images -->
[1]: ./media/network-watcher-intrusion-detection-open-source-tools/figure1.png
[2]: ./media/network-watcher-intrusion-detection-open-source-tools/figure2.png
[3]: ./media/network-watcher-intrusion-detection-open-source-tools/figure3.png
[4]: ./media/network-watcher-intrusion-detection-open-source-tools/figure4.png
[5]: ./media/network-watcher-intrusion-detection-open-source-tools/figure5.png
[6]: ./media/network-watcher-intrusion-detection-open-source-tools/figure6.png
[7]: ./media/network-watcher-intrusion-detection-open-source-tools/figure7.png
