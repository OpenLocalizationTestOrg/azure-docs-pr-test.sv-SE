---
title: "Utföra nätverket intrångsidentifiering med öppen källkod verktyg och Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här artikeln beskriver hur du använder Azure Nätverksbevakaren och Öppna källa verktyg för att utföra intrångsidentifiering för nätverk"
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
ms.openlocfilehash: 82d5e525859ebe03b152c63e4debbae469049c12
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="perform-network-intrusion-detection-with-network-watcher-and-open-source-tools"></a><span data-ttu-id="06bad-103">Utföra nätverket intrångsidentifiering med öppen källkod verktyg och Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="06bad-103">Perform network intrusion detection with Network Watcher and open source tools</span></span>

<span data-ttu-id="06bad-104">Paketet insamlingar är en viktig del för att implementera nätverk intrång identifiering (ID) och utföra Network Security övervakning (NSM).</span><span class="sxs-lookup"><span data-stu-id="06bad-104">Packet captures are a key component for implementing network intrusion detection systems (IDS) and performing Network Security Monitoring (NSM).</span></span> <span data-ttu-id="06bad-105">Det finns flera ID: N verktyg för öppen källkod som bearbetar paket insamlingar och leta efter signaturer för intrång i nätverket och skadliga aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="06bad-105">There are several open source IDS tools that process packet captures and look for signatures of possible network intrusions and malicious activity.</span></span> <span data-ttu-id="06bad-106">Med hjälp av paketet samlar in angivna genom Nätverksbevakaren, kan du analysera nätverket för skadliga intrång eller säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="06bad-106">Using the packet captures provided by Network Watcher, you can analyze your network for any harmful intrusions or vulnerabilities.</span></span>

<span data-ttu-id="06bad-107">Ett sådant öppen källkod verktyg är Suricata, en ID-motor som använder regeluppsättningar att övervaka nätverkstrafik och utlöser aviseringar när misstänkta händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="06bad-107">One such open source tool is Suricata, an IDS engine that uses rulesets to monitor network traffic and triggers alerts whenever suspicious events occur.</span></span> <span data-ttu-id="06bad-108">Suricata erbjuder ett flertrådat motorn, vilket innebär att den kan utföra nätverkstrafikanalysen med ökad hastighet och effektivitet.</span><span class="sxs-lookup"><span data-stu-id="06bad-108">Suricata offers a multi-threaded engine, meaning it can perform network traffic analysis with increased speed and efficiency.</span></span> <span data-ttu-id="06bad-109">Mer information om Suricata och dess funktioner finns i deras webbplats på https://suricata-ids.org/.</span><span class="sxs-lookup"><span data-stu-id="06bad-109">For more details about Suricata and its capabilities, visit their website at https://suricata-ids.org/.</span></span>

## <a name="scenario"></a><span data-ttu-id="06bad-110">Scenario</span><span class="sxs-lookup"><span data-stu-id="06bad-110">Scenario</span></span>

<span data-ttu-id="06bad-111">Den här artikeln beskriver hur du konfigurerar miljön för att utföra nätverket intrångsidentifiering med Nätverksbevakaren, Suricata och elastiska stacken.</span><span class="sxs-lookup"><span data-stu-id="06bad-111">This article explains how to set up your environment to perform network intrusion detection using Network Watcher, Suricata, and the Elastic Stack.</span></span> <span data-ttu-id="06bad-112">Nätverksbevakaren ger paket-insamlingar som används för att utföra nätverket intrångsidentifiering.</span><span class="sxs-lookup"><span data-stu-id="06bad-112">Network Watcher provides you with the packet captures used to perform network intrusion detection.</span></span> <span data-ttu-id="06bad-113">Suricata bearbetar paket insamlingar och utlösa varningar baserat på paket som matchar dess given regeluppsättning hot.</span><span class="sxs-lookup"><span data-stu-id="06bad-113">Suricata processes the packet captures and trigger alerts based on packets that match its given ruleset of threats.</span></span> <span data-ttu-id="06bad-114">Dessa aviseringar som lagras i en loggfil på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="06bad-114">These alerts are stored in a log file on your local machine.</span></span> <span data-ttu-id="06bad-115">Med elastisk stacken kan loggar som genereras av Suricata indexeras och används för att skapa en Kibana instrumentpanel som ger dig en bild av loggarna och ett sätt att snabbt få insikter om potentiella säkerhetsproblem i nätverket.</span><span class="sxs-lookup"><span data-stu-id="06bad-115">Using the Elastic Stack, the logs generated by Suricata can be indexed and used to create a Kibana dashboard, providing you with a visual representation of the logs and a means to quickly gain insights to potential network vulnerabilities.</span></span>  

![enkel web application scenario][1]

<span data-ttu-id="06bad-117">Båda verktyg med öppen källkod kan ställas in på en virtuell dator i Azure, så att du kan utföra den här analysen i Azure network-miljön.</span><span class="sxs-lookup"><span data-stu-id="06bad-117">Both open source tools can be set up on an Azure VM, allowing you to perform this analysis within your own Azure network environment.</span></span>

## <a name="steps"></a><span data-ttu-id="06bad-118">Steg</span><span class="sxs-lookup"><span data-stu-id="06bad-118">Steps</span></span>

### <a name="install-suricata"></a><span data-ttu-id="06bad-119">Installera Suricata</span><span class="sxs-lookup"><span data-stu-id="06bad-119">Install Suricata</span></span>

<span data-ttu-id="06bad-120">Alla andra metoder för installation finns i http://suricata.readthedocs.io/en/latest/install.html</span><span class="sxs-lookup"><span data-stu-id="06bad-120">For all other methods of installation, visit http://suricata.readthedocs.io/en/latest/install.html</span></span>

1. <span data-ttu-id="06bad-121">Kör följande kommandon i Kommandotolken terminal på den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="06bad-121">In the command-line terminal of your VM run the following commands:</span></span>

    ```
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo sudo apt-get install suricata
    ```

1. <span data-ttu-id="06bad-122">Verifiera installationen genom att köra kommandot `suricata -h` att se en fullständig lista över kommandon.</span><span class="sxs-lookup"><span data-stu-id="06bad-122">To verify your installation, run the command `suricata -h` to see the full list of commands.</span></span>

### <a name="download-the-emerging-threats-ruleset"></a><span data-ttu-id="06bad-123">Hämta nya hot RuleSet-metod</span><span class="sxs-lookup"><span data-stu-id="06bad-123">Download the Emerging Threats ruleset</span></span>

<span data-ttu-id="06bad-124">I det här skedet har vi inte några regler för Suricata ska köras.</span><span class="sxs-lookup"><span data-stu-id="06bad-124">At this stage, we do not have any rules for Suricata to run.</span></span> <span data-ttu-id="06bad-125">Du kan skapa dina egna regler om det finns specifika hot mot nätverket som du vill identifiera och du kan också använda utvecklats uppsättningar från ett antal leverantörer, till exempel nya hot eller VRT regler från Snort.</span><span class="sxs-lookup"><span data-stu-id="06bad-125">You can create your own rules if there are specific threats to your network you would like to detect, or you can also use developed rule sets from a number of providers, such as Emerging Threats, or VRT rules from Snort.</span></span> <span data-ttu-id="06bad-126">Vi använder fritt tillgänglig nya hot ruleset här:</span><span class="sxs-lookup"><span data-stu-id="06bad-126">We use the freely accessible Emerging Threats ruleset here:</span></span>

<span data-ttu-id="06bad-127">Hämta regeluppsättningen och kopiera dem till katalogen:</span><span class="sxs-lookup"><span data-stu-id="06bad-127">Download the rule set and copy them into the directory:</span></span>

```
wget http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
tar zxf emerging.rules.tar.gz
sudo cp -r rules /etc/suricata/
```

### <a name="process-packet-captures-with-suricata"></a><span data-ttu-id="06bad-128">Processen paket fångar med Suricata</span><span class="sxs-lookup"><span data-stu-id="06bad-128">Process packet captures with Suricata</span></span>

<span data-ttu-id="06bad-129">Samlar in med hjälp av Suricata, kör du följande kommando för att bearbeta paketet:</span><span class="sxs-lookup"><span data-stu-id="06bad-129">To process packet captures using Suricata, run the following command:</span></span>

```
sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
```
<span data-ttu-id="06bad-130">Läs filen fast.log för att kontrollera de resulterande aviseringarna:</span><span class="sxs-lookup"><span data-stu-id="06bad-130">To check the resulting alerts, read the fast.log file:</span></span>
```
tail -f /var/log/suricata/fast.log
```

### <a name="set-up-the-elastic-stack"></a><span data-ttu-id="06bad-131">Konfigurera elastisk Stack</span><span class="sxs-lookup"><span data-stu-id="06bad-131">Set up the Elastic Stack</span></span>

<span data-ttu-id="06bad-132">När de loggar som producerar Suricata innehåller värdefull information om vad som händer på vårt nätverk, är dessa loggfiler inte enklast att läsa och förstå.</span><span class="sxs-lookup"><span data-stu-id="06bad-132">While the logs that Suricata produces contain valuable information about what’s happening on our network, these log files aren’t the easiest to read and understand.</span></span> <span data-ttu-id="06bad-133">Genom att ansluta Suricata med elastisk Stack kan skapa vi en instrumentpanel för Kibana vad gör att vi kan söka, diagram, analysera och härleda insikter från våra loggar.</span><span class="sxs-lookup"><span data-stu-id="06bad-133">By connecting Suricata with the Elastic Stack, we can create a Kibana dashboard what allows us to search, graph, analyze, and derive insights from our logs.</span></span>

#### <a name="install-elasticsearch"></a><span data-ttu-id="06bad-134">Installera Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="06bad-134">Install Elasticsearch</span></span>

1. <span data-ttu-id="06bad-135">Elastisk stacken version 5.0 och senare kräver Java 8.</span><span class="sxs-lookup"><span data-stu-id="06bad-135">The Elastic Stack from version 5.0 and above requires Java 8.</span></span> <span data-ttu-id="06bad-136">Kör kommandot `java -version` att kontrollera din version.</span><span class="sxs-lookup"><span data-stu-id="06bad-136">Run the command `java -version` to check your version.</span></span> <span data-ttu-id="06bad-137">Om du inte har java installera, finns i dokumentationen till på [Oracles webbplats](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span><span class="sxs-lookup"><span data-stu-id="06bad-137">If you do not have java install, refer to documentation on [Oracle's website](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span></span>
1. <span data-ttu-id="06bad-138">Hämta rätt binära paket för ditt system:</span><span class="sxs-lookup"><span data-stu-id="06bad-138">Download the correct binary package for your system:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    <span data-ttu-id="06bad-139">Andra installationsmetoder finns på [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span><span class="sxs-lookup"><span data-stu-id="06bad-139">Other installation methods can be found at [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span></span>

1. <span data-ttu-id="06bad-140">Kontrollera att Elasticsearch körs med kommandot:</span><span class="sxs-lookup"><span data-stu-id="06bad-140">Verify that Elasticsearch is running with the command:</span></span>

    ```
    curl http://127.0.0.1:9200
    ```

    <span data-ttu-id="06bad-141">Du bör se ett svar som liknar detta:</span><span class="sxs-lookup"><span data-stu-id="06bad-141">You should see a response similar to this:</span></span>

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

<span data-ttu-id="06bad-142">Ytterligare information om installation elastisk Sök hänvisar till sidan [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span><span class="sxs-lookup"><span data-stu-id="06bad-142">For further instructions on installing Elastic search, refer to the page [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span></span>

### <a name="install-logstash"></a><span data-ttu-id="06bad-143">Installera Logstash</span><span class="sxs-lookup"><span data-stu-id="06bad-143">Install Logstash</span></span>

1. <span data-ttu-id="06bad-144">Installera Logstash kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="06bad-144">To install Logstash run the following commands:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. <span data-ttu-id="06bad-145">Nästa vi måste du konfigurera Logstash att läsa från utdata från eve.json-filen.</span><span class="sxs-lookup"><span data-stu-id="06bad-145">Next we need to configure Logstash to read from the output of eve.json file.</span></span> <span data-ttu-id="06bad-146">Skapa filen logstash.conf med:</span><span class="sxs-lookup"><span data-stu-id="06bad-146">Create a logstash.conf file using:</span></span>

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. <span data-ttu-id="06bad-147">Lägg till följande innehåll i filen (Kontrollera att sökvägen till filen eve.json är korrekt):</span><span class="sxs-lookup"><span data-stu-id="06bad-147">Add the following content to the file (make sure that the path to the eve.json file is correct):</span></span>

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

1. <span data-ttu-id="06bad-148">Se till att ge behörighet till filen eve.json så att Logstash kan infognings-filen.</span><span class="sxs-lookup"><span data-stu-id="06bad-148">Make sure to give the correct permissions to the eve.json file so that Logstash can ingest the file.</span></span>
    
    ```
    sudo chmod 775 /var/log/suricata/eve.json
    ```

1. <span data-ttu-id="06bad-149">Om du vill starta Logstash köra kommandot:</span><span class="sxs-lookup"><span data-stu-id="06bad-149">To start Logstash run the command:</span></span>

    ```
    sudo /etc/init.d/logstash start
    ```

<span data-ttu-id="06bad-150">Ytterligare instruktioner om hur du installerar Logstash finns i den [officiella dokumentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span><span class="sxs-lookup"><span data-stu-id="06bad-150">For further instructions on installing Logstash, refer to the [official documentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span></span>

### <a name="install-kibana"></a><span data-ttu-id="06bad-151">Installera Kibana</span><span class="sxs-lookup"><span data-stu-id="06bad-151">Install Kibana</span></span>

1. <span data-ttu-id="06bad-152">Kör följande kommandon för att installera Kibana:</span><span class="sxs-lookup"><span data-stu-id="06bad-152">Run the following commands to install Kibana:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
    tar xzvf kibana-5.2.0-linux-x86_64.tar.gz

    ```
1. <span data-ttu-id="06bad-153">Om du vill köra Använd Kibana kommandon:</span><span class="sxs-lookup"><span data-stu-id="06bad-153">To run Kibana use the commands:</span></span>

    ```
    cd kibana-5.2.0-linux-x86_64/
    ./bin/kibana
    ```

1. <span data-ttu-id="06bad-154">Om du vill visa dina Kibana webbgränssnitt, gå till`http://localhost:5601`</span><span class="sxs-lookup"><span data-stu-id="06bad-154">To view your Kibana web interface, navigate to `http://localhost:5601`</span></span>
1. <span data-ttu-id="06bad-155">I det här scenariot index mönstret för Suricata loggarna är ”logstash-*”</span><span class="sxs-lookup"><span data-stu-id="06bad-155">For this scenario, the index pattern used for the Suricata logs is "logstash-*"</span></span>

1. <span data-ttu-id="06bad-156">Om du vill visa infopanelen Kibana via fjärranslutning, skapar du en inkommande NSG regel för att tillåta åtkomst till **port 5601**.</span><span class="sxs-lookup"><span data-stu-id="06bad-156">If you want to view the Kibana dashboard remotely, create an inbound NSG rule allowing access to **port 5601**.</span></span>

### <a name="create-a-kibana-dashboard"></a><span data-ttu-id="06bad-157">Skapa en Kibana instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="06bad-157">Create a Kibana dashboard</span></span>

<span data-ttu-id="06bad-158">Vi har angett ett exempel på en instrumentpanel där du kan visa trender och information i aviseringar för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="06bad-158">For this article, we have provided a sample dashboard for you to view trends and details in your alerts.</span></span>

1. <span data-ttu-id="06bad-159">Hämta filen instrumentpanelen [här](https://aka.ms/networkwatchersuricatadashboard), visualisering filen [här](https://aka.ms/networkwatchersuricatavisualization), och filen sparad sökning [här](https://aka.ms/networkwatchersuricatasavedsearch).</span><span class="sxs-lookup"><span data-stu-id="06bad-159">Download the dashboard file [here](https://aka.ms/networkwatchersuricatadashboard), the visualization file [here](https://aka.ms/networkwatchersuricatavisualization), and the saved search file [here](https://aka.ms/networkwatchersuricatasavedsearch).</span></span>

1. <span data-ttu-id="06bad-160">Under den **Management** fliken av Kibana, gå till **sparade objekt** och importera alla tre filerna.</span><span class="sxs-lookup"><span data-stu-id="06bad-160">Under the **Management** tab of Kibana, navigate to **Saved Objects** and import all three files.</span></span> <span data-ttu-id="06bad-161">Sedan från den **instrumentpanelen** fliken kan du öppna och läsa in de exempel på en instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="06bad-161">Then from the **Dashboard** tab you can open and load the sample dashboard.</span></span>

<span data-ttu-id="06bad-162">Du kan också skapa egna visualiseringar och instrumentpaneler som skräddarsys mot mätvärden för ditt eget intresse.</span><span class="sxs-lookup"><span data-stu-id="06bad-162">You can also create your own visualizations and dashboards tailored towards metrics of your own interest.</span></span> <span data-ttu-id="06bad-163">Läs mer om hur du skapar Kibana visualiseringar från Kibana's [officiella dokumentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span><span class="sxs-lookup"><span data-stu-id="06bad-163">Read more about creating Kibana visualizations from Kibana's [official documentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span></span>

![kibana instrumentpanelen][2]

### <a name="visualize-ids-alert-logs"></a><span data-ttu-id="06bad-165">Visualisera ID avisering loggar</span><span class="sxs-lookup"><span data-stu-id="06bad-165">Visualize IDS alert logs</span></span>

<span data-ttu-id="06bad-166">I exemplet på instrumentpanel innehåller flera visualiseringar Suricata avisering loggar:</span><span class="sxs-lookup"><span data-stu-id="06bad-166">The sample dashboard provides several visualizations of the Suricata alert logs:</span></span>

1. <span data-ttu-id="06bad-167">Aviseringar av GeoIP – en karta med distribution av aviseringar för sina ursprung baserat på geografisk plats (bestäms av IP)</span><span class="sxs-lookup"><span data-stu-id="06bad-167">Alerts by GeoIP – a map showing the distribution of alerts by their country of origin based on geographic location (determined by IP)</span></span>

    ![GEO-ip][3]

1. <span data-ttu-id="06bad-169">Översta 10 aviseringar – en sammanfattning av de vanligaste 10 utlöst aviseringar och deras beskrivning.</span><span class="sxs-lookup"><span data-stu-id="06bad-169">Top 10 Alerts – a summary of the 10 most frequent triggered alerts and their description.</span></span> <span data-ttu-id="06bad-170">Klicka på en enskild varning filtrerar ned instrumentpanelen på den information som rör den specifika aviseringen.</span><span class="sxs-lookup"><span data-stu-id="06bad-170">Clicking an individual alert filters down the dashboard to the information pertaining to that specific alert.</span></span>

    ![bild 4][4]

1. <span data-ttu-id="06bad-172">Antal aviseringar – totalt antal aviseringar som har utlösts av den RuleSet-metod</span><span class="sxs-lookup"><span data-stu-id="06bad-172">Number of Alerts – the total count of alerts triggered by the ruleset</span></span>

    ![bild 5][5]

1. <span data-ttu-id="06bad-174">Övre 20 källan/målet IP-adresser portar - cirkeldiagram visar de översta 20 IP-adresser och portar som aviseringar har utlösts av.</span><span class="sxs-lookup"><span data-stu-id="06bad-174">Top 20 Source/Destination IPs/Ports - pie charts showing the top 20 IPs and ports that alerts were triggered on.</span></span> <span data-ttu-id="06bad-175">Du kan filtrera nedåt på specifika IP-adresser/portar att se hur många och vilka typer av aviseringar aktiveras.</span><span class="sxs-lookup"><span data-stu-id="06bad-175">You can filter down on specific IPs/ports to see how many and what kind of alerts are being triggered.</span></span>

    ![bild 6][6]

1. <span data-ttu-id="06bad-177">Aviseringen sammanfattning – en tabell sammanfattar närmare för varje enskild varning.</span><span class="sxs-lookup"><span data-stu-id="06bad-177">Alert Summary – a table summarizing specific details of each individual alert.</span></span> <span data-ttu-id="06bad-178">Du kan anpassa den här tabellen om du vill visa andra parametrar av intresse för varje avisering.</span><span class="sxs-lookup"><span data-stu-id="06bad-178">You can customize this table to show other parameters of interest for each alert.</span></span>

    ![Bild 7][7]

<span data-ttu-id="06bad-180">Mer dokumentation om hur du skapar anpassade visualiseringar och instrumentpaneler finns [Kibanas officiella dokumentation](https://www.elastic.co/guide/en/kibana/current/introduction.html).</span><span class="sxs-lookup"><span data-stu-id="06bad-180">For more documentation on creating custom visualizations and dashboards, see [Kibana’s official documentation](https://www.elastic.co/guide/en/kibana/current/introduction.html).</span></span>

## <a name="conclusion"></a><span data-ttu-id="06bad-181">Slutsats</span><span class="sxs-lookup"><span data-stu-id="06bad-181">Conclusion</span></span>

<span data-ttu-id="06bad-182">Samlar in angivna genom Nätverksbevakaren och verktyg för öppen källkod-ID: N som Suricata, du kan utföra nätverket intrångsidentifiering för en mängd olika hot genom att kombinera paket.</span><span class="sxs-lookup"><span data-stu-id="06bad-182">By combining packet captures provided by Network Watcher and open source IDS tools such as Suricata, you can perform network intrusion detection for a wide range of threats.</span></span> <span data-ttu-id="06bad-183">Instrumentpanelerna kan du snabbt se trender och avvikelser i nätverket, som korrekt gräva i data för att identifiera rotorsaken till aviseringar, till exempel skadliga användaragenter eller sårbara portarna.</span><span class="sxs-lookup"><span data-stu-id="06bad-183">These dashboards allow you to quickly spot trends and anomalies within your network, as well dig into the data to discover root causes of alerts such as malicious user agents or vulnerable ports.</span></span> <span data-ttu-id="06bad-184">Med den här extraherade data fatta du välgrundade beslut om hur du ta hänsyn till och skydda nätverket från skadliga intrångsförsök och skapa regler för att förhindra framtida intrång i nätverket.</span><span class="sxs-lookup"><span data-stu-id="06bad-184">With this extracted data, you can make informed decisions on how to react to and protect your network from any harmful intrusion attempts, and create rules to prevent future intrusions to your network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="06bad-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="06bad-185">Next steps</span></span>

<span data-ttu-id="06bad-186">Lär dig hur du utlöser paket insamlingar baserat på aviseringar genom att besöka [använder paketinsamling för proaktiv nätverksövervakning med Azure Functions](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="06bad-186">Learn how to trigger packet captures based on alerts by visiting [Use packet capture to do proactive network monitoring with Azure Functions](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="06bad-187">Lär dig hur du visualisera dina NSG flödet loggar med Power BI genom att besöka [visualisera NSG flödar loggar med Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="06bad-187">Learn how to visualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>



<!-- images -->
[1]: ./media/network-watcher-intrusion-detection-open-source-tools/figure1.png
[2]: ./media/network-watcher-intrusion-detection-open-source-tools/figure2.png
[3]: ./media/network-watcher-intrusion-detection-open-source-tools/figure3.png
[4]: ./media/network-watcher-intrusion-detection-open-source-tools/figure4.png
[5]: ./media/network-watcher-intrusion-detection-open-source-tools/figure5.png
[6]: ./media/network-watcher-intrusion-detection-open-source-tools/figure6.png
[7]: ./media/network-watcher-intrusion-detection-open-source-tools/figure7.png
