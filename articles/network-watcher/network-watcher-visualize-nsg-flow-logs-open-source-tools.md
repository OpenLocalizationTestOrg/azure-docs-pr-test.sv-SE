---
title: "aaaVisualize Azure Network Watcher NSG flödet loggar med öppen källkod verktyg | Microsoft Docs"
description: "Den här sidan beskrivs hur toouse Öppna källa verktyg toovisualize NSG flödet loggar."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e9b2dcad-4da4-4d6b-aee2-6d0afade0cb8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 47cb529d4a1e00e8c4c0fa6885cbf72aed3e74c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a><span data-ttu-id="85d33-103">Visualisera Azure Network Watcher NSG flödet loggar med öppen källkod verktyg</span><span class="sxs-lookup"><span data-stu-id="85d33-103">Visualize Azure Network Watcher NSG flow logs using open source tools</span></span>

<span data-ttu-id="85d33-104">Nätverkssäkerhetsgruppen flöde loggarna ger information som kan användas för förstå ingående och utgående IP-trafik på Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="85d33-104">Network Security Group flow logs provide information that can be used understand ingress and egress IP traffic on Network Security Groups.</span></span> <span data-ttu-id="85d33-105">Loggarna flödet visar utgående och inkommande flöden på grundval av per regel hello NIC hello flödet gäller för 5 tuppel information om hello flödet (källan/målet IP, källan/målet Port Protocol), och om hello trafik tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="85d33-105">These flow logs show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5 tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

<span data-ttu-id="85d33-106">Loggarna flödet kan vara svårt toomanually parse och få insikter från.</span><span class="sxs-lookup"><span data-stu-id="85d33-106">These flow logs can be difficult toomanually parse and gain insights from.</span></span> <span data-ttu-id="85d33-107">Det finns dock flera öppen källkod verktyg som hjälper dig att visualisera informationen.</span><span class="sxs-lookup"><span data-stu-id="85d33-107">However, there are several open source tools that can help visualize this data.</span></span> <span data-ttu-id="85d33-108">Den här artikeln får du en lösning toovisualize loggarna använder hello elastisk stacken, vilket gör att du tooquickly index och visualisera flödet loggarna på en instrumentpanel med Kibana.</span><span class="sxs-lookup"><span data-stu-id="85d33-108">This article will provide a solution toovisualize these logs using hello Elastic Stack, which will allow you tooquickly index and visualize your flow logs on a Kibana dashboard.</span></span>

## <a name="scenario"></a><span data-ttu-id="85d33-109">Scenario</span><span class="sxs-lookup"><span data-stu-id="85d33-109">Scenario</span></span>

<span data-ttu-id="85d33-110">I den här artikeln ska vi ställa in en lösning som gör att du toovisualize Nätverkssäkerhetsgruppen flöde loggar med hello elastisk stacken.</span><span class="sxs-lookup"><span data-stu-id="85d33-110">In this article, we will set up a solution that will allow you toovisualize Network Security Group flow logs using hello Elastic Stack.</span></span>  <span data-ttu-id="85d33-111">Ett Logstash inkommande plugin-program ska få hello flödet loggarna direkt från hello storage blob som konfigurerats för som innehåller hello flödet loggar.</span><span class="sxs-lookup"><span data-stu-id="85d33-111">A Logstash input plugin will obtain hello flow logs directly from hello storage blob configured for containing hello flow logs.</span></span> <span data-ttu-id="85d33-112">Sedan använder hello elastisk Stack hello flödet loggar indexeras och används toocreate en Kibana instrumentpanelen toovisualize hello information.</span><span class="sxs-lookup"><span data-stu-id="85d33-112">Then, using hello Elastic Stack, hello flow logs will be indexed and used toocreate a Kibana dashboard toovisualize hello information.</span></span>

![scenario][scenario]

## <a name="steps"></a><span data-ttu-id="85d33-114">Steg</span><span class="sxs-lookup"><span data-stu-id="85d33-114">Steps</span></span>

### <a name="enable-network-security-group-flow-logging"></a><span data-ttu-id="85d33-115">Aktivera Nätverkssäkerhetsgruppen flöde loggning</span><span class="sxs-lookup"><span data-stu-id="85d33-115">Enable Network Security Group flow logging</span></span>
<span data-ttu-id="85d33-116">I det här scenariot måste du ha nätverket grupp flöda säkerhetsloggning aktiverat på minst en säkerhetsgrupp för nätverk i ditt konto.</span><span class="sxs-lookup"><span data-stu-id="85d33-116">For this scenario, you must have Network Security Group Flow Logging enabled on at least one Network Security Group in your account.</span></span> <span data-ttu-id="85d33-117">Anvisningar om hur du aktiverar Network flöda säkerhetsloggar finns toohello följande artikel [introduktion tooflow loggning för Nätverkssäkerhetsgrupper](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="85d33-117">For instructions on enabling Network Security Flow Logs, refer toohello following article [Introduction tooflow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md).</span></span>


### <a name="set-up-hello-elastic-stack"></a><span data-ttu-id="85d33-118">Ställ in hello elastisk Stack</span><span class="sxs-lookup"><span data-stu-id="85d33-118">Set up hello Elastic Stack</span></span>
<span data-ttu-id="85d33-119">Genom att ansluta NSG flöda loggar med hello elastisk Stack kan vi skapa en instrumentpanel för Kibana vad gör att vi toosearch, diagram, analysera och härleda insikter från våra loggar.</span><span class="sxs-lookup"><span data-stu-id="85d33-119">By connecting NSG flow logs with hello Elastic Stack, we can create a Kibana dashboard what allows us toosearch, graph, analyze, and derive insights from our logs.</span></span>

#### <a name="install-elasticsearch"></a><span data-ttu-id="85d33-120">Installera Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="85d33-120">Install Elasticsearch</span></span>

1. <span data-ttu-id="85d33-121">hello elastisk Stack version 5.0 och senare kräver Java 8.</span><span class="sxs-lookup"><span data-stu-id="85d33-121">hello Elastic Stack from version 5.0 and above requires Java 8.</span></span> <span data-ttu-id="85d33-122">Kör kommandot hello `java -version` toocheck din version.</span><span class="sxs-lookup"><span data-stu-id="85d33-122">Run hello command `java -version` toocheck your version.</span></span> <span data-ttu-id="85d33-123">Om du inte har java installera, se toodocumentation på [Oracles webbplats](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span><span class="sxs-lookup"><span data-stu-id="85d33-123">If you do not have java install, refer toodocumentation on [Oracle's website](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span></span>
1. <span data-ttu-id="85d33-124">Hämta hello rätt binära paket för systemet:</span><span class="sxs-lookup"><span data-stu-id="85d33-124">Download hello correct binary package for your system:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    <span data-ttu-id="85d33-125">Andra installationsmetoder finns på [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span><span class="sxs-lookup"><span data-stu-id="85d33-125">Other installation methods can be found at [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span></span>

1. <span data-ttu-id="85d33-126">Kontrollera att Elasticsearch körs med hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="85d33-126">Verify that Elasticsearch is running with hello command:</span></span>

    ```
    curl http://127.0.0.1:9200
    ```

    <span data-ttu-id="85d33-127">Du bör se en liknande toothis svar:</span><span class="sxs-lookup"><span data-stu-id="85d33-127">You should see a response similar toothis:</span></span>

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

<span data-ttu-id="85d33-128">Ytterligare information om installation elastisk Sök finns toohello sidan [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span><span class="sxs-lookup"><span data-stu-id="85d33-128">For further instructions on installing Elastic search, refer toohello page [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span></span>

### <a name="install-logstash"></a><span data-ttu-id="85d33-129">Installera Logstash</span><span class="sxs-lookup"><span data-stu-id="85d33-129">Install Logstash</span></span>

1. <span data-ttu-id="85d33-130">tooinstall Logstash kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="85d33-130">tooinstall Logstash run hello following commands:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. <span data-ttu-id="85d33-131">Därefter vi behöver tooconfigure Logstash tooaccess och parsa hello flödet loggar.</span><span class="sxs-lookup"><span data-stu-id="85d33-131">Next we need tooconfigure Logstash tooaccess and parse hello flow logs.</span></span> <span data-ttu-id="85d33-132">Skapa filen logstash.conf med:</span><span class="sxs-lookup"><span data-stu-id="85d33-132">Create a logstash.conf file using:</span></span>

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. <span data-ttu-id="85d33-133">Lägg till följande innehåll toohello hello:</span><span class="sxs-lookup"><span data-stu-id="85d33-133">Add hello following content toohello file:</span></span>

  ```
    input {
      azureblob
        {
            storage_account_name => "mystorageaccount"
            storage_access_key => "storageaccesskey"
            container => "nsgflowlogContainerName"
            codec => "json"
        }
      }

      filter {
        split { field => "[records]" }
        split { field => "[records][properties][flows]"}
        split { field => "[records][properties][flows][flows]"}
        split { field => "[records][properties][flows][flows][flowTuples]"}

     mutate{
      split => { "[records][resourceId]" => "/"}
      add_field => {"Subscription" => "%{[records][resourceId][2]}"
                    "ResourceGroup" => "%{[records][resourceId][4]}"
                    "NetworkSecurityGroup" => "%{[records][resourceId][8]}"}
      convert => {"Subscription" => "string"}
      convert => {"ResourceGroup" => "string"}
      convert => {"NetworkSecurityGroup" => "string"}
      split => { "[records][properties][flows][flows][flowTuples]" => ","}
      add_field => {
                  "unixtimestamp" => "%{[records][properties][flows][flows][flowTuples][0]}"
                  "srcIp" => "%{[records][properties][flows][flows][flowTuples][1]}"
                  "destIp" => "%{[records][properties][flows][flows][flowTuples][2]}"
                  "srcPort" => "%{[records][properties][flows][flows][flowTuples][3]}"
                  "destPort" => "%{[records][properties][flows][flows][flowTuples][4]}"
                  "protocol" => "%{[records][properties][flows][flows][flowTuples][5]}"
                  "trafficflow" => "%{[records][properties][flows][flows][flowTuples][6]}"
                  "traffic" => "%{[records][properties][flows][flows][flowTuples][7]}"
                   }
      convert => {"unixtimestamp" => "integer"}
      convert => {"srcPort" => "integer"}
      convert => {"destPort" => "integer"}        
     }

     date{
       match => ["unixtimestamp" , "UNIX"]
     }
    }

    output {
      stdout { codec => rubydebug }
      elasticsearch {
        hosts => "localhost"
        index => "nsg-flow-logs"
      }
    }  

  ```

<span data-ttu-id="85d33-134">Ytterligare instruktioner om hur du installerar Logstash finns toohello [officiella dokumentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span><span class="sxs-lookup"><span data-stu-id="85d33-134">For further instructions on installing Logstash, refer toohello [official documentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span></span>

### <a name="install-hello-logstash-input-plugin-for-azure-blob-storage"></a><span data-ttu-id="85d33-135">Installera hello Logstash inkommande plugin-program för Azure-blobblagring</span><span class="sxs-lookup"><span data-stu-id="85d33-135">Install hello Logstash input plugin for Azure blob storage</span></span>

<span data-ttu-id="85d33-136">Den här Logstash plugin-programmet kan du toodirectly åtkomst hello flödet loggar från deras avsedda storage-konto.</span><span class="sxs-lookup"><span data-stu-id="85d33-136">This Logstash plugin will allow you toodirectly access hello flow logs from their designated storage account.</span></span> <span data-ttu-id="85d33-137">tooinstall den här plugin-programmet hello Logstash Standardinstallationskatalogen (i det här fallet /usr/share/logstash/bin) kör hello kommando från:</span><span class="sxs-lookup"><span data-stu-id="85d33-137">tooinstall this plugin, from hello default Logstash installation directory (in this case /usr/share/logstash/bin) run hello command:</span></span>

```
logstash-plugin install logstash-input-azureblob
```

<span data-ttu-id="85d33-138">toostart Logstash kör hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="85d33-138">toostart Logstash run hello command:</span></span>

```
sudo /etc/init.d/logstash start
```

<span data-ttu-id="85d33-139">Mer information om den här plugin-program finns i toodocumentation [här](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)</span><span class="sxs-lookup"><span data-stu-id="85d33-139">For more information about this plugin, refer toodocumentation [here](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)</span></span>

### <a name="install-kibana"></a><span data-ttu-id="85d33-140">Installera Kibana</span><span class="sxs-lookup"><span data-stu-id="85d33-140">Install Kibana</span></span>

1. <span data-ttu-id="85d33-141">Kör följande kommandon tooinstall Kibana hello:</span><span class="sxs-lookup"><span data-stu-id="85d33-141">Run hello following commands tooinstall Kibana:</span></span>

  ```
  curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
  tar xzvf kibana-5.2.0-linux-x86_64.tar.gz
  ```

1. <span data-ttu-id="85d33-142">toorun Kibana använda hello-kommandon:</span><span class="sxs-lookup"><span data-stu-id="85d33-142">toorun Kibana use hello commands:</span></span>

  ```
  cd kibana-5.2.0-linux-x86_64/
  ./bin/kibana
  ```

1. <span data-ttu-id="85d33-143">tooview webbplatsen Kibana gränssnitt, navigera för`http://localhost:5601`</span><span class="sxs-lookup"><span data-stu-id="85d33-143">tooview your Kibana web interface, navigate too`http://localhost:5601`</span></span>
1. <span data-ttu-id="85d33-144">I det här scenariot är hello index mönstret för hello flödet loggar ”nsg-flöde-logs”.</span><span class="sxs-lookup"><span data-stu-id="85d33-144">For this scenario, hello index pattern used for hello flow logs is "nsg-flow-logs".</span></span> <span data-ttu-id="85d33-145">Du kan ändra hello index mönster i hello ”utdata” avsnittet av filen logstash.conf.</span><span class="sxs-lookup"><span data-stu-id="85d33-145">You may change hello index pattern in hello "output" section of your logstash.conf file.</span></span>

1. <span data-ttu-id="85d33-146">Om du vill tooview hello Kibana instrumentpanel via fjärranslutning, skapar du en inkommande NSG regel för att tillåta åtkomst för**port 5601**.</span><span class="sxs-lookup"><span data-stu-id="85d33-146">If you want tooview hello Kibana dashboard remotely, create an inbound NSG rule allowing access too**port 5601**.</span></span>

### <a name="create-a-kibana-dashboard"></a><span data-ttu-id="85d33-147">Skapa en Kibana instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="85d33-147">Create a Kibana dashboard</span></span>

<span data-ttu-id="85d33-148">Vi har angett en exempelinstrumentpanel du tooview trender och information i aviseringar för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="85d33-148">For this article, we have provided a sample dashboard for you tooview trends and details in your alerts.</span></span>

![bild 1][1]

1. <span data-ttu-id="85d33-150">Hämta hello instrumentpanelen filen [här](https://aka.ms/networkwatchernsgflowlogdashboard), hello visualiseringen filen [här](https://aka.ms/networkwatchernsgflowlogvisualizations), och hello sparade sökningen [här](https://aka.ms/networkwatchernsgflowlogsearch).</span><span class="sxs-lookup"><span data-stu-id="85d33-150">Download hello dashboard file [here](https://aka.ms/networkwatchernsgflowlogdashboard), hello visualization file [here](https://aka.ms/networkwatchernsgflowlogvisualizations), and hello saved search file [here](https://aka.ms/networkwatchernsgflowlogsearch).</span></span>

1. <span data-ttu-id="85d33-151">Under hello **Management** fliken av Kibana, navigera för**sparade objekt** och importera alla tre filerna.</span><span class="sxs-lookup"><span data-stu-id="85d33-151">Under hello **Management** tab of Kibana, navigate too**Saved Objects** and import all three files.</span></span> <span data-ttu-id="85d33-152">Sedan från hello **instrumentpanelen** fliken kan du öppna och läsa in hello exempel på en instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="85d33-152">Then from hello **Dashboard** tab you can open and load hello sample dashboard.</span></span>

<span data-ttu-id="85d33-153">Du kan också skapa egna visualiseringar och instrumentpaneler som skräddarsys mot mätvärden för ditt eget intresse.</span><span class="sxs-lookup"><span data-stu-id="85d33-153">You can also create your own visualizations and dashboards tailored towards metrics of your own interest.</span></span> <span data-ttu-id="85d33-154">Läs mer om hur du skapar Kibana visualiseringar från Kibana's [officiella dokumentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span><span class="sxs-lookup"><span data-stu-id="85d33-154">Read more about creating Kibana visualizations from Kibana's [official documentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span></span>

### <a name="visualize-nsg-flow-logs"></a><span data-ttu-id="85d33-155">Visualisera flödet NSG-loggar</span><span class="sxs-lookup"><span data-stu-id="85d33-155">Visualize NSG flow logs</span></span>

<span data-ttu-id="85d33-156">exemplet på instrumentpanel med hello innehåller flera visualiseringar hello flödet loggar:</span><span class="sxs-lookup"><span data-stu-id="85d33-156">hello sample dashboard provides several visualizations of hello flow logs:</span></span>

1. <span data-ttu-id="85d33-157">Flöden av beslut/riktning över tid - tid serien diagram som visar hello Antal flöden över hello tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="85d33-157">Flows by Decision/Direction Over Time - time series graphs showing hello number of flows over hello time period.</span></span> <span data-ttu-id="85d33-158">Du kan redigera hello tidsenhet och tidsrymden för båda dessa visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="85d33-158">You can edit hello unit of time and span of both these visualizations.</span></span> <span data-ttu-id="85d33-159">Flöden av beslut visar hello andelen Tillåt eller neka som måste fattas när flöden av riktning visar hello andelen inkommande och utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="85d33-159">Flows by Decision shows hello proportion of allow or deny decisions made, while Flows by Direction shows hello proportion of inbound and outbound traffic.</span></span> <span data-ttu-id="85d33-160">Med dessa visuell information du undersöka trender trafik över tid och leta efter alla toppar eller ovanliga mönster.</span><span class="sxs-lookup"><span data-stu-id="85d33-160">With these visuals you can examine traffic trends over time and look for any spikes or unusual patterns.</span></span>

  ![bild 2][2]

1. <span data-ttu-id="85d33-162">Flöden av målservern/källport – cirkeldiagram visar hello uppdelning av flöden tootheir respektive portar.</span><span class="sxs-lookup"><span data-stu-id="85d33-162">Flows by Destination/Source Port – pie charts showing hello breakdown of flows tootheir respective ports.</span></span> <span data-ttu-id="85d33-163">Med den här vyn kan du se dina mest använda portar.</span><span class="sxs-lookup"><span data-stu-id="85d33-163">With this view you can see your most commonly used ports.</span></span> <span data-ttu-id="85d33-164">Om du klickar på en specifik port inom hello cirkeldiagram filtrerar hello resten av hello instrumentpanelen ned tooflows för denna port.</span><span class="sxs-lookup"><span data-stu-id="85d33-164">If you click on a specific port within hello pie chart, hello rest of hello dashboard will filter down tooflows of that port.</span></span>

  ![figure3][3]

1. <span data-ttu-id="85d33-166">Antal flöden och tidigaste Loggtid – mått som visar hello Antal flöden registreras och hello datum hello tidigaste loggen avbildas.</span><span class="sxs-lookup"><span data-stu-id="85d33-166">Number of Flows and Earliest Log Time – metrics showing you hello number of flows recorded and hello date of hello earliest log captured.</span></span>

  ![bild 4][4]

1. <span data-ttu-id="85d33-168">Flöden av NSG och regel – ett stapeldiagram som visar hello distribution av flöden inom varje NSG, samt hello distribution av regler inom varje NSG.</span><span class="sxs-lookup"><span data-stu-id="85d33-168">Flows by NSG and Rule – a bar graph showing you hello distribution of flows within each NSG, as well as hello distribution of rules within each NSG.</span></span> <span data-ttu-id="85d33-169">Härifrån kan du se vilka NSG och regler som genererats hello merparten av trafiken.</span><span class="sxs-lookup"><span data-stu-id="85d33-169">From here you can see which NSG and rules generated hello most traffic.</span></span>

  ![figure5][5]

1. <span data-ttu-id="85d33-171">Topp 10 källan/målet IP-adresser – stapeldiagram som visar hello topp 10 käll- och IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="85d33-171">Top 10 Source/Destination IPs – bar charts showing hello top 10 source and destination IPs.</span></span> <span data-ttu-id="85d33-172">Du kan justera de här diagrammen tooshow mer eller mindre översta IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="85d33-172">You can adjust these charts tooshow more or less top IPs.</span></span> <span data-ttu-id="85d33-173">Härifrån du kan se hello inträffar oftast IP-adresser samt hello trafik beslut (Tillåt eller neka) som görs mot varje IP.</span><span class="sxs-lookup"><span data-stu-id="85d33-173">From here you can see hello most commonly occurring IPs as well as hello traffic decision (allow or deny) being made towards each IP.</span></span>

  ![figure6][6]

1. <span data-ttu-id="85d33-175">Flödet Tupplar – den här tabellen visar hello informationen i varje flöde tuppel samt dess motsvarande NGS och regeln.</span><span class="sxs-lookup"><span data-stu-id="85d33-175">Flow Tuples – this table shows you hello information contained within each flow tuple, as well as its corresponding NGS and rule.</span></span>

  ![figure7][7]

<span data-ttu-id="85d33-177">Med hjälp av hello fråga fältet hello överst i hello instrumentpanelen kan du filtrera ned hello instrumentpanel baserat på valfri parameter för hello flöden, till exempel prenumerations-ID, resursgrupper, regel eller alla andra variabler av intresse.</span><span class="sxs-lookup"><span data-stu-id="85d33-177">Using hello query bar at hello top of hello dashboard, you can filter down hello dashboard based on any parameter of hello flows, such as subscription ID, resource groups, rule, or any other variable of interest.</span></span> <span data-ttu-id="85d33-178">Mer information om Kibanas frågor och filter finns toohello [officiella dokumentation](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)</span><span class="sxs-lookup"><span data-stu-id="85d33-178">For more about Kibana's queries and filters, refer toohello [official documentation](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)</span></span>

## <a name="conclusion"></a><span data-ttu-id="85d33-179">Slutsats</span><span class="sxs-lookup"><span data-stu-id="85d33-179">Conclusion</span></span>

<span data-ttu-id="85d33-180">Genom att kombinera hello Nätverkssäkerhetsgruppen flöde loggar med hello elastisk Stack har vi kom fram till anpassningsbara och kraftfulla toovisualize våra nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="85d33-180">By combining hello Network Security Group flow logs with hello Elastic Stack, we have come up with powerful and customizable way toovisualize our network traffic.</span></span> <span data-ttu-id="85d33-181">Instrumentpanelerna kan du få tooquickly och dela information om nätverkstrafik samt filter ned och undersöka på alla eventuella avvikelser.</span><span class="sxs-lookup"><span data-stu-id="85d33-181">These dashboards allow you tooquickly gain and share insights about your network traffic, as well as filter down and investigate on any potential anomalies.</span></span> <span data-ttu-id="85d33-182">Med Kibana kan du anpassa instrumentpanelerna och skapa specifika visualiseringar toomeet alla säkerhets-, gransknings- och kompatibilitet behov.</span><span class="sxs-lookup"><span data-stu-id="85d33-182">Using Kibana, you can tailor these dashboards and create specific visualizations toomeet any security, audit, and compliance needs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="85d33-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="85d33-183">Next steps</span></span>

<span data-ttu-id="85d33-184">Lär dig hur toovisualize NSG-flöde loggar med Power BI genom att besöka [visualisera NSG flödar loggar med Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="85d33-184">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>


<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
