---
title: "Visualisera Azure Network Watcher NSG flödet loggar med öppen källkod verktyg | Microsoft Docs"
description: "Den här sidan beskriver hur du använder verktyg med öppen källkod visualisera NSG flödet loggar."
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
ms.openlocfilehash: 20f60ccd9108a7473705c2368f28d3152d0dd614
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a><span data-ttu-id="974b9-103">Visualisera Azure Network Watcher NSG flödet loggar med öppen källkod verktyg</span><span class="sxs-lookup"><span data-stu-id="974b9-103">Visualize Azure Network Watcher NSG flow logs using open source tools</span></span>

<span data-ttu-id="974b9-104">Nätverkssäkerhetsgruppen flöde loggarna ger information som kan användas för förstå ingående och utgående IP-trafik på Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="974b9-104">Network Security Group flow logs provide information that can be used understand ingress and egress IP traffic on Network Security Groups.</span></span> <span data-ttu-id="974b9-105">Loggarna flödet visar utgående och inkommande flöden på grundval av per regel, NIC flödet gäller, 5 tuppel information om flödet (källan/målet IP, källan/målet Port Protocol), och om trafiken tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="974b9-105">These flow logs show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5 tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

<span data-ttu-id="974b9-106">Loggarna flödet kan vara svårt att tolka och få insikter från manuellt.</span><span class="sxs-lookup"><span data-stu-id="974b9-106">These flow logs can be difficult to manually parse and gain insights from.</span></span> <span data-ttu-id="974b9-107">Det finns dock flera öppen källkod verktyg som hjälper dig att visualisera informationen.</span><span class="sxs-lookup"><span data-stu-id="974b9-107">However, there are several open source tools that can help visualize this data.</span></span> <span data-ttu-id="974b9-108">Den här artikeln får du en lösning för att visualisera dessa loggar med elastisk stacken, vilket innebär att du snabbt index och visualisera dina flödet loggar på en instrumentpanel med Kibana.</span><span class="sxs-lookup"><span data-stu-id="974b9-108">This article will provide a solution to visualize these logs using the Elastic Stack, which will allow you to quickly index and visualize your flow logs on a Kibana dashboard.</span></span>

## <a name="scenario"></a><span data-ttu-id="974b9-109">Scenario</span><span class="sxs-lookup"><span data-stu-id="974b9-109">Scenario</span></span>

<span data-ttu-id="974b9-110">I den här artikeln ska vi ställa in en lösning som gör att du kan visualisera Nätverkssäkerhetsgruppen flöde loggar med elastisk stacken.</span><span class="sxs-lookup"><span data-stu-id="974b9-110">In this article, we will set up a solution that will allow you to visualize Network Security Group flow logs using the Elastic Stack.</span></span>  <span data-ttu-id="974b9-111">Ett Logstash inkommande plugin-program hämtar flödet loggarna direkt från lagringsblob som konfigurerats för som innehåller loggarna flödet.</span><span class="sxs-lookup"><span data-stu-id="974b9-111">A Logstash input plugin will obtain the flow logs directly from the storage blob configured for containing the flow logs.</span></span> <span data-ttu-id="974b9-112">Sedan kommer använder elastisk stacken flödet loggarna indexeras och används för att skapa en Kibana instrumentpanel om du vill visualisera informationen.</span><span class="sxs-lookup"><span data-stu-id="974b9-112">Then, using the Elastic Stack, the flow logs will be indexed and used to create a Kibana dashboard to visualize the information.</span></span>

![scenario][scenario]

## <a name="steps"></a><span data-ttu-id="974b9-114">Steg</span><span class="sxs-lookup"><span data-stu-id="974b9-114">Steps</span></span>

### <a name="enable-network-security-group-flow-logging"></a><span data-ttu-id="974b9-115">Aktivera Nätverkssäkerhetsgruppen flöde loggning</span><span class="sxs-lookup"><span data-stu-id="974b9-115">Enable Network Security Group flow logging</span></span>
<span data-ttu-id="974b9-116">I det här scenariot måste du ha nätverket grupp flöda säkerhetsloggning aktiverat på minst en säkerhetsgrupp för nätverk i ditt konto.</span><span class="sxs-lookup"><span data-stu-id="974b9-116">For this scenario, you must have Network Security Group Flow Logging enabled on at least one Network Security Group in your account.</span></span> <span data-ttu-id="974b9-117">Instruktioner om hur du aktiverar Network säkerhetsloggar flödet finns i följande artikel [introduktion till flödet loggning för Nätverkssäkerhetsgrupper](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="974b9-117">For instructions on enabling Network Security Flow Logs, refer to the following article [Introduction to flow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md).</span></span>


### <a name="set-up-the-elastic-stack"></a><span data-ttu-id="974b9-118">Konfigurera elastisk Stack</span><span class="sxs-lookup"><span data-stu-id="974b9-118">Set up the Elastic Stack</span></span>
<span data-ttu-id="974b9-119">Genom att ansluta NSG flödet loggar med elastisk Stack kan skapa vi en instrumentpanel för Kibana vad gör att vi kan söka, diagram, analysera och härleda insikter från våra loggar.</span><span class="sxs-lookup"><span data-stu-id="974b9-119">By connecting NSG flow logs with the Elastic Stack, we can create a Kibana dashboard what allows us to search, graph, analyze, and derive insights from our logs.</span></span>

#### <a name="install-elasticsearch"></a><span data-ttu-id="974b9-120">Installera Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="974b9-120">Install Elasticsearch</span></span>

1. <span data-ttu-id="974b9-121">Elastisk stacken version 5.0 och senare kräver Java 8.</span><span class="sxs-lookup"><span data-stu-id="974b9-121">The Elastic Stack from version 5.0 and above requires Java 8.</span></span> <span data-ttu-id="974b9-122">Kör kommandot `java -version` att kontrollera din version.</span><span class="sxs-lookup"><span data-stu-id="974b9-122">Run the command `java -version` to check your version.</span></span> <span data-ttu-id="974b9-123">Om du inte har java installera, finns i dokumentationen till på [Oracles webbplats](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span><span class="sxs-lookup"><span data-stu-id="974b9-123">If you do not have java install, refer to documentation on [Oracle's website](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span></span>
1. <span data-ttu-id="974b9-124">Hämta rätt binära paket för ditt system:</span><span class="sxs-lookup"><span data-stu-id="974b9-124">Download the correct binary package for your system:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    <span data-ttu-id="974b9-125">Andra installationsmetoder finns på [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span><span class="sxs-lookup"><span data-stu-id="974b9-125">Other installation methods can be found at [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span></span>

1. <span data-ttu-id="974b9-126">Kontrollera att Elasticsearch körs med kommandot:</span><span class="sxs-lookup"><span data-stu-id="974b9-126">Verify that Elasticsearch is running with the command:</span></span>

    ```
    curl http://127.0.0.1:9200
    ```

    <span data-ttu-id="974b9-127">Du bör se ett svar som liknar detta:</span><span class="sxs-lookup"><span data-stu-id="974b9-127">You should see a response similar to this:</span></span>

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

<span data-ttu-id="974b9-128">Ytterligare information om installation elastisk Sök hänvisar till sidan [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span><span class="sxs-lookup"><span data-stu-id="974b9-128">For further instructions on installing Elastic search, refer to the page [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span></span>

### <a name="install-logstash"></a><span data-ttu-id="974b9-129">Installera Logstash</span><span class="sxs-lookup"><span data-stu-id="974b9-129">Install Logstash</span></span>

1. <span data-ttu-id="974b9-130">Installera Logstash kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="974b9-130">To install Logstash run the following commands:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. <span data-ttu-id="974b9-131">Nästa måste vi du konfigurera Logstash för åtkomst och parsa flödet loggarna.</span><span class="sxs-lookup"><span data-stu-id="974b9-131">Next we need to configure Logstash to access and parse the flow logs.</span></span> <span data-ttu-id="974b9-132">Skapa filen logstash.conf med:</span><span class="sxs-lookup"><span data-stu-id="974b9-132">Create a logstash.conf file using:</span></span>

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. <span data-ttu-id="974b9-133">Lägg till följande innehåll i filen:</span><span class="sxs-lookup"><span data-stu-id="974b9-133">Add the following content to the file:</span></span>

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

<span data-ttu-id="974b9-134">Ytterligare instruktioner om hur du installerar Logstash finns i den [officiella dokumentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span><span class="sxs-lookup"><span data-stu-id="974b9-134">For further instructions on installing Logstash, refer to the [official documentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span></span>

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a><span data-ttu-id="974b9-135">Installera Logstash inkommande plugin-programmet för Azure-blobblagring</span><span class="sxs-lookup"><span data-stu-id="974b9-135">Install the Logstash input plugin for Azure blob storage</span></span>

<span data-ttu-id="974b9-136">Den här Logstash plugin-program kan du få direkt åtkomst till flödet loggar från deras avsedda storage-konto.</span><span class="sxs-lookup"><span data-stu-id="974b9-136">This Logstash plugin will allow you to directly access the flow logs from their designated storage account.</span></span> <span data-ttu-id="974b9-137">Om du vill installera den här plugin-programmet kör från Standardinstallationskatalogen Logstash (i det här fallet /usr/share/logstash/bin) du kommandot:</span><span class="sxs-lookup"><span data-stu-id="974b9-137">To install this plugin, from the default Logstash installation directory (in this case /usr/share/logstash/bin) run the command:</span></span>

```
logstash-plugin install logstash-input-azureblob
```

<span data-ttu-id="974b9-138">Om du vill starta Logstash köra kommandot:</span><span class="sxs-lookup"><span data-stu-id="974b9-138">To start Logstash run the command:</span></span>

```
sudo /etc/init.d/logstash start
```

<span data-ttu-id="974b9-139">Mer information om den här plugin-program finns i dokumentationen [här](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)</span><span class="sxs-lookup"><span data-stu-id="974b9-139">For more information about this plugin, refer to documentation [here](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)</span></span>

### <a name="install-kibana"></a><span data-ttu-id="974b9-140">Installera Kibana</span><span class="sxs-lookup"><span data-stu-id="974b9-140">Install Kibana</span></span>

1. <span data-ttu-id="974b9-141">Kör följande kommandon för att installera Kibana:</span><span class="sxs-lookup"><span data-stu-id="974b9-141">Run the following commands to install Kibana:</span></span>

  ```
  curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
  tar xzvf kibana-5.2.0-linux-x86_64.tar.gz
  ```

1. <span data-ttu-id="974b9-142">Om du vill köra Använd Kibana kommandon:</span><span class="sxs-lookup"><span data-stu-id="974b9-142">To run Kibana use the commands:</span></span>

  ```
  cd kibana-5.2.0-linux-x86_64/
  ./bin/kibana
  ```

1. <span data-ttu-id="974b9-143">Om du vill visa dina Kibana webbgränssnitt, gå till`http://localhost:5601`</span><span class="sxs-lookup"><span data-stu-id="974b9-143">To view your Kibana web interface, navigate to `http://localhost:5601`</span></span>
1. <span data-ttu-id="974b9-144">I det här scenariot är index mönstret som används för flödet loggar ”nsg-flöde-logs”.</span><span class="sxs-lookup"><span data-stu-id="974b9-144">For this scenario, the index pattern used for the flow logs is "nsg-flow-logs".</span></span> <span data-ttu-id="974b9-145">Du kan ändra indexet mönster i avsnittet ”utdata” i filen logstash.conf.</span><span class="sxs-lookup"><span data-stu-id="974b9-145">You may change the index pattern in the "output" section of your logstash.conf file.</span></span>

1. <span data-ttu-id="974b9-146">Om du vill visa infopanelen Kibana via fjärranslutning, skapar du en inkommande NSG regel för att tillåta åtkomst till **port 5601**.</span><span class="sxs-lookup"><span data-stu-id="974b9-146">If you want to view the Kibana dashboard remotely, create an inbound NSG rule allowing access to **port 5601**.</span></span>

### <a name="create-a-kibana-dashboard"></a><span data-ttu-id="974b9-147">Skapa en Kibana instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="974b9-147">Create a Kibana dashboard</span></span>

<span data-ttu-id="974b9-148">Vi har angett ett exempel på en instrumentpanel där du kan visa trender och information i aviseringar för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="974b9-148">For this article, we have provided a sample dashboard for you to view trends and details in your alerts.</span></span>

![bild 1][1]

1. <span data-ttu-id="974b9-150">Hämta filen instrumentpanelen [här](https://aka.ms/networkwatchernsgflowlogdashboard), visualisering filen [här](https://aka.ms/networkwatchernsgflowlogvisualizations), och filen sparad sökning [här](https://aka.ms/networkwatchernsgflowlogsearch).</span><span class="sxs-lookup"><span data-stu-id="974b9-150">Download the dashboard file [here](https://aka.ms/networkwatchernsgflowlogdashboard), the visualization file [here](https://aka.ms/networkwatchernsgflowlogvisualizations), and the saved search file [here](https://aka.ms/networkwatchernsgflowlogsearch).</span></span>

1. <span data-ttu-id="974b9-151">Under den **Management** fliken av Kibana, gå till **sparade objekt** och importera alla tre filerna.</span><span class="sxs-lookup"><span data-stu-id="974b9-151">Under the **Management** tab of Kibana, navigate to **Saved Objects** and import all three files.</span></span> <span data-ttu-id="974b9-152">Sedan från den **instrumentpanelen** fliken kan du öppna och läsa in de exempel på en instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="974b9-152">Then from the **Dashboard** tab you can open and load the sample dashboard.</span></span>

<span data-ttu-id="974b9-153">Du kan också skapa egna visualiseringar och instrumentpaneler som skräddarsys mot mätvärden för ditt eget intresse.</span><span class="sxs-lookup"><span data-stu-id="974b9-153">You can also create your own visualizations and dashboards tailored towards metrics of your own interest.</span></span> <span data-ttu-id="974b9-154">Läs mer om hur du skapar Kibana visualiseringar från Kibana's [officiella dokumentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span><span class="sxs-lookup"><span data-stu-id="974b9-154">Read more about creating Kibana visualizations from Kibana's [official documentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span></span>

### <a name="visualize-nsg-flow-logs"></a><span data-ttu-id="974b9-155">Visualisera flödet NSG-loggar</span><span class="sxs-lookup"><span data-stu-id="974b9-155">Visualize NSG flow logs</span></span>

<span data-ttu-id="974b9-156">I exemplet på instrumentpanel innehåller flera visualiseringar flödet loggar:</span><span class="sxs-lookup"><span data-stu-id="974b9-156">The sample dashboard provides several visualizations of the flow logs:</span></span>

1. <span data-ttu-id="974b9-157">Flöden av beslut/riktning över tid - tid serien diagram som visar antal flöden för tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="974b9-157">Flows by Decision/Direction Over Time - time series graphs showing the number of flows over the time period.</span></span> <span data-ttu-id="974b9-158">Du kan redigera tidsenheten och tidsrymden för båda dessa visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="974b9-158">You can edit the unit of time and span of both these visualizations.</span></span> <span data-ttu-id="974b9-159">Flöden genom beslut visar andelen tillåta eller neka som måste fattas medan flöden av riktning visas andelen inkommande och utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="974b9-159">Flows by Decision shows the proportion of allow or deny decisions made, while Flows by Direction shows the proportion of inbound and outbound traffic.</span></span> <span data-ttu-id="974b9-160">Med dessa visuell information du undersöka trender trafik över tid och leta efter alla toppar eller ovanliga mönster.</span><span class="sxs-lookup"><span data-stu-id="974b9-160">With these visuals you can examine traffic trends over time and look for any spikes or unusual patterns.</span></span>

  ![bild 2][2]

1. <span data-ttu-id="974b9-162">Flöden av målservern/källport – cirkeldiagram visar fördelningen av flödar till respektive port.</span><span class="sxs-lookup"><span data-stu-id="974b9-162">Flows by Destination/Source Port – pie charts showing the breakdown of flows to their respective ports.</span></span> <span data-ttu-id="974b9-163">Med den här vyn kan du se dina mest använda portar.</span><span class="sxs-lookup"><span data-stu-id="974b9-163">With this view you can see your most commonly used ports.</span></span> <span data-ttu-id="974b9-164">Om du klickar på en specifik port i cirkeldiagrammet filtrerar resten av instrumentpanelen till flödet av denna port.</span><span class="sxs-lookup"><span data-stu-id="974b9-164">If you click on a specific port within the pie chart, the rest of the dashboard will filter down to flows of that port.</span></span>

  ![figure3][3]

1. <span data-ttu-id="974b9-166">Antal flöden och tidigaste Loggtid – mått som visar du antal flöden registreras och datumet för den tidigaste loggen avbildas.</span><span class="sxs-lookup"><span data-stu-id="974b9-166">Number of Flows and Earliest Log Time – metrics showing you the number of flows recorded and the date of the earliest log captured.</span></span>

  ![bild 4][4]

1. <span data-ttu-id="974b9-168">Flöden av NSG och regel – ett stapeldiagram som visar fördelningen av flöden inom varje NSG, samt distribution av regler inom varje NSG.</span><span class="sxs-lookup"><span data-stu-id="974b9-168">Flows by NSG and Rule – a bar graph showing you the distribution of flows within each NSG, as well as the distribution of rules within each NSG.</span></span> <span data-ttu-id="974b9-169">Du kan se vilka NSG och regler genereras mest trafik från den här.</span><span class="sxs-lookup"><span data-stu-id="974b9-169">From here you can see which NSG and rules generated the most traffic.</span></span>

  ![figure5][5]

1. <span data-ttu-id="974b9-171">Främsta 10 källan/målet IP-adresser – stapeldiagram som visar upp 10 käll- och IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="974b9-171">Top 10 Source/Destination IPs – bar charts showing the top 10 source and destination IPs.</span></span> <span data-ttu-id="974b9-172">Du kan justera dessa diagram om du vill visa mer eller mindre översta IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="974b9-172">You can adjust these charts to show more or less top IPs.</span></span> <span data-ttu-id="974b9-173">Härifrån kan du se de vanligaste uppstår IP-adresser samt trafik beslut (Tillåt eller neka) som görs mot varje IP.</span><span class="sxs-lookup"><span data-stu-id="974b9-173">From here you can see the most commonly occurring IPs as well as the traffic decision (allow or deny) being made towards each IP.</span></span>

  ![figure6][6]

1. <span data-ttu-id="974b9-175">Flödet Tupplar – den här tabellen visar du informationen i varje flöde tuppel samt dess motsvarande NGS och regeln.</span><span class="sxs-lookup"><span data-stu-id="974b9-175">Flow Tuples – this table shows you the information contained within each flow tuple, as well as its corresponding NGS and rule.</span></span>

  ![figure7][7]

<span data-ttu-id="974b9-177">Med hjälp av fältet fråga överst på instrumentpanelen kan du filtrera ned instrumentpanelen med någon parameter av flöden, till exempel prenumerations-ID, resursgrupper, regel eller alla andra variabler av intresse.</span><span class="sxs-lookup"><span data-stu-id="974b9-177">Using the query bar at the top of the dashboard, you can filter down the dashboard based on any parameter of the flows, such as subscription ID, resource groups, rule, or any other variable of interest.</span></span> <span data-ttu-id="974b9-178">Mer information om Kibanas frågor och filter som avser den [officiella dokumentation](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)</span><span class="sxs-lookup"><span data-stu-id="974b9-178">For more about Kibana's queries and filters, refer to the [official documentation](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)</span></span>

## <a name="conclusion"></a><span data-ttu-id="974b9-179">Slutsats</span><span class="sxs-lookup"><span data-stu-id="974b9-179">Conclusion</span></span>

<span data-ttu-id="974b9-180">Genom att kombinera Nätverkssäkerhetsgruppen flöde loggar med elastisk Stack, har vi kom fram till kraftfulla och anpassningsbara sätt att visualisera våra nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="974b9-180">By combining the Network Security Group flow logs with the Elastic Stack, we have come up with powerful and customizable way to visualize our network traffic.</span></span> <span data-ttu-id="974b9-181">Instrumentpanelerna kan du snabbt få och dela information om nätverkstrafik samt filter ned och undersöka på alla potentiella avvikelser.</span><span class="sxs-lookup"><span data-stu-id="974b9-181">These dashboards allow you to quickly gain and share insights about your network traffic, as well as filter down and investigate on any potential anomalies.</span></span> <span data-ttu-id="974b9-182">Med Kibana kan du anpassa instrumentpanelerna och skapa specifika grafik som uppfyller behoven för alla säkerhets-, gransknings- och kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="974b9-182">Using Kibana, you can tailor these dashboards and create specific visualizations to meet any security, audit, and compliance needs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="974b9-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="974b9-183">Next steps</span></span>

<span data-ttu-id="974b9-184">Lär dig hur du visualisera dina NSG flödet loggar med Power BI genom att besöka [visualisera NSG flödar loggar med Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="974b9-184">Learn how to visualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>


<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
