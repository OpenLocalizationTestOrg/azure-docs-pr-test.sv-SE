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
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a>Visualisera Azure Network Watcher NSG flödet loggar med öppen källkod verktyg

Nätverkssäkerhetsgruppen flöde loggarna ger information som kan användas för förstå ingående och utgående IP-trafik på Nätverkssäkerhetsgrupper. Loggarna flödet visar utgående och inkommande flöden på grundval av per regel hello NIC hello flödet gäller för 5 tuppel information om hello flödet (källan/målet IP, källan/målet Port Protocol), och om hello trafik tillåts eller nekas.

Loggarna flödet kan vara svårt toomanually parse och få insikter från. Det finns dock flera öppen källkod verktyg som hjälper dig att visualisera informationen. Den här artikeln får du en lösning toovisualize loggarna använder hello elastisk stacken, vilket gör att du tooquickly index och visualisera flödet loggarna på en instrumentpanel med Kibana.

## <a name="scenario"></a>Scenario

I den här artikeln ska vi ställa in en lösning som gör att du toovisualize Nätverkssäkerhetsgruppen flöde loggar med hello elastisk stacken.  Ett Logstash inkommande plugin-program ska få hello flödet loggarna direkt från hello storage blob som konfigurerats för som innehåller hello flödet loggar. Sedan använder hello elastisk Stack hello flödet loggar indexeras och används toocreate en Kibana instrumentpanelen toovisualize hello information.

![scenario][scenario]

## <a name="steps"></a>Steg

### <a name="enable-network-security-group-flow-logging"></a>Aktivera Nätverkssäkerhetsgruppen flöde loggning
I det här scenariot måste du ha nätverket grupp flöda säkerhetsloggning aktiverat på minst en säkerhetsgrupp för nätverk i ditt konto. Anvisningar om hur du aktiverar Network flöda säkerhetsloggar finns toohello följande artikel [introduktion tooflow loggning för Nätverkssäkerhetsgrupper](network-watcher-nsg-flow-logging-overview.md).


### <a name="set-up-hello-elastic-stack"></a>Ställ in hello elastisk Stack
Genom att ansluta NSG flöda loggar med hello elastisk Stack kan vi skapa en instrumentpanel för Kibana vad gör att vi toosearch, diagram, analysera och härleda insikter från våra loggar.

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
1. Därefter vi behöver tooconfigure Logstash tooaccess och parsa hello flödet loggar. Skapa filen logstash.conf med:

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. Lägg till följande innehåll toohello hello:

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

Ytterligare instruktioner om hur du installerar Logstash finns toohello [officiella dokumentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)

### <a name="install-hello-logstash-input-plugin-for-azure-blob-storage"></a>Installera hello Logstash inkommande plugin-program för Azure-blobblagring

Den här Logstash plugin-programmet kan du toodirectly åtkomst hello flödet loggar från deras avsedda storage-konto. tooinstall den här plugin-programmet hello Logstash Standardinstallationskatalogen (i det här fallet /usr/share/logstash/bin) kör hello kommando från:

```
logstash-plugin install logstash-input-azureblob
```

toostart Logstash kör hello-kommando:

```
sudo /etc/init.d/logstash start
```

Mer information om den här plugin-program finns i toodocumentation [här](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)

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
1. I det här scenariot är hello index mönstret för hello flödet loggar ”nsg-flöde-logs”. Du kan ändra hello index mönster i hello ”utdata” avsnittet av filen logstash.conf.

1. Om du vill tooview hello Kibana instrumentpanel via fjärranslutning, skapar du en inkommande NSG regel för att tillåta åtkomst för**port 5601**.

### <a name="create-a-kibana-dashboard"></a>Skapa en Kibana instrumentpanel

Vi har angett en exempelinstrumentpanel du tooview trender och information i aviseringar för den här artikeln.

![bild 1][1]

1. Hämta hello instrumentpanelen filen [här](https://aka.ms/networkwatchernsgflowlogdashboard), hello visualiseringen filen [här](https://aka.ms/networkwatchernsgflowlogvisualizations), och hello sparade sökningen [här](https://aka.ms/networkwatchernsgflowlogsearch).

1. Under hello **Management** fliken av Kibana, navigera för**sparade objekt** och importera alla tre filerna. Sedan från hello **instrumentpanelen** fliken kan du öppna och läsa in hello exempel på en instrumentpanel.

Du kan också skapa egna visualiseringar och instrumentpaneler som skräddarsys mot mätvärden för ditt eget intresse. Läs mer om hur du skapar Kibana visualiseringar från Kibana's [officiella dokumentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).

### <a name="visualize-nsg-flow-logs"></a>Visualisera flödet NSG-loggar

exemplet på instrumentpanel med hello innehåller flera visualiseringar hello flödet loggar:

1. Flöden av beslut/riktning över tid - tid serien diagram som visar hello Antal flöden över hello tidsperiod. Du kan redigera hello tidsenhet och tidsrymden för båda dessa visualiseringar. Flöden av beslut visar hello andelen Tillåt eller neka som måste fattas när flöden av riktning visar hello andelen inkommande och utgående trafik. Med dessa visuell information du undersöka trender trafik över tid och leta efter alla toppar eller ovanliga mönster.

  ![bild 2][2]

1. Flöden av målservern/källport – cirkeldiagram visar hello uppdelning av flöden tootheir respektive portar. Med den här vyn kan du se dina mest använda portar. Om du klickar på en specifik port inom hello cirkeldiagram filtrerar hello resten av hello instrumentpanelen ned tooflows för denna port.

  ![figure3][3]

1. Antal flöden och tidigaste Loggtid – mått som visar hello Antal flöden registreras och hello datum hello tidigaste loggen avbildas.

  ![bild 4][4]

1. Flöden av NSG och regel – ett stapeldiagram som visar hello distribution av flöden inom varje NSG, samt hello distribution av regler inom varje NSG. Härifrån kan du se vilka NSG och regler som genererats hello merparten av trafiken.

  ![figure5][5]

1. Topp 10 källan/målet IP-adresser – stapeldiagram som visar hello topp 10 käll- och IP-adresser. Du kan justera de här diagrammen tooshow mer eller mindre översta IP-adresser. Härifrån du kan se hello inträffar oftast IP-adresser samt hello trafik beslut (Tillåt eller neka) som görs mot varje IP.

  ![figure6][6]

1. Flödet Tupplar – den här tabellen visar hello informationen i varje flöde tuppel samt dess motsvarande NGS och regeln.

  ![figure7][7]

Med hjälp av hello fråga fältet hello överst i hello instrumentpanelen kan du filtrera ned hello instrumentpanel baserat på valfri parameter för hello flöden, till exempel prenumerations-ID, resursgrupper, regel eller alla andra variabler av intresse. Mer information om Kibanas frågor och filter finns toohello [officiella dokumentation](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)

## <a name="conclusion"></a>Slutsats

Genom att kombinera hello Nätverkssäkerhetsgruppen flöde loggar med hello elastisk Stack har vi kom fram till anpassningsbara och kraftfulla toovisualize våra nätverkstrafik. Instrumentpanelerna kan du få tooquickly och dela information om nätverkstrafik samt filter ned och undersöka på alla eventuella avvikelser. Med Kibana kan du anpassa instrumentpanelerna och skapa specifika visualiseringar toomeet alla säkerhets-, gransknings- och kompatibilitet behov.

## <a name="next-steps"></a>Nästa steg

Lär dig hur toovisualize NSG-flöde loggar med Power BI genom att besöka [visualisera NSG flödar loggar med Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)


<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
