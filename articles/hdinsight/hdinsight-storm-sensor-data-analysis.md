---
title: aaaAnalyze sensordata med Apache Storm och HBase | Microsoft Docs
description: "Lär dig hur tooconnect tooApache Storm med ett virtuellt nätverk. Använda Storm med HBase tooprocess sensordata från en händelsehubb och visualisera med D3.js."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a9a1ac8e-5708-4833-b965-e453815e671f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/09/2017
ms.author: larryfr
ms.openlocfilehash: e54fe9ffc720b0089f90e302b24a9438bd43999a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Analysera sensordata med Apache Storm, Event Hub och HBase i HDInsight (Hadoop)

Lär dig hur toouse Apache Storm på HDInsight tooprocess sensordata från Azure Event Hub. hello data lagras i Apache HBase i HDInsight och visualiseras med D3.js.

hello Azure Resource Manager-mall som används i det här dokumentet visar hur toocreate flera Azure-resurser i en resursgrupp. hello mallen skapar ett Azure Virtual Network, två HDInsight-kluster (Storm och HBase) och en Webbapp i Azure. En node.js-implementering av en webbinstrumentpanel i realtid är automatiskt distribuerade toohello webbprogram.

> [!NOTE]
> hello informationen i det här dokumentet och exemplet i det här dokumentet kräver HDInsight version 3,6.
>
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Krav

* En Azure-prenumeration.
* [Node.js](http://nodejs.org/): använda toopreview hello web instrumentpanelen lokalt på din utvecklingsmiljö.
* [Java och hello JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): används toodevelop hello Storm-topologi.
* [Maven](http://maven.apache.org/what-is-maven.html): använda toobuild och kompilera hello-projekt.
* [Git](http://git-scm.com/): använda toodownload hello projektet från GitHub.
* En **SSH** klienten: används tooconnect toohello Linux-baserade HDInsight-kluster. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).


> [!IMPORTANT]
> Du behöver inte ett befintligt HDInsight-kluster. hello stegen i det här dokumentet skapa hello följande resurser:
> 
> * Azure-nätverk
> * Ett Storm på HDInsight-kluster (Linux-baserade två arbetarnoder)
> * En HBase på HDInsight-kluster (Linux-baserade två arbetarnoder)
> * En Azure-Webbapp som är värd för hello web instrumentpanelen

## <a name="architecture"></a>Arkitektur

![Arkitekturdiagram](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

Det här exemplet består av hello följande komponenter:

* **Händelsehubbar i Azure**: innehåller data som samlas in från sensorer.
* **Storm på HDInsight**: ger realtidsbearbetning av data från Event Hub.
* **HBase på HDInsight**: ger en beständig NoSQL-databas för data när den har bearbetats av Storm.
* **Azure Virtual Network service**: möjliggör säker kommunikation mellan hello Storm på HDInsight och HBase på HDInsight-kluster.
  
  > [!NOTE]
  > Ett virtuellt nätverk krävs när du använder hello Java HBase klient-API. Exponeras inte via hello offentliga gateway för HBase-kluster. Installera HBase och Storm-kluster i samma virtuella nätverk gör hello hello Storm-kluster (eller andra system i hello virtuella nätverk) toodirectly åtkomst till HBase med klient-API.

* **Instrumentpanelen webbplats**: ett exempel instrumentpanel diagram data i realtid.
  
  * hello webbplats är implementerat i Node.js.
  * [Socket.IO](http://socket.io/) används för kommunikation mellan hello Storm-topologi och hello webbplats i realtid.
    
    > [!NOTE]
    > Med Socket.io för kommunikation är en implementering detaljer. Du kan använda alla kommunikationssystem som oformaterade WebSockets eller SignalR.

  * [D3.js](http://d3js.org/) används toograph hello data som skickas toohello webbplats.

> [!IMPORTANT]
> Två kluster krävs, eftersom det inte finns några stöds metoden toocreate ett HDInsight-kluster för både Storm och HBase.

hello topologi läser data från Event Hub med hjälp av hello [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) klass och skriver data till HBase med hello [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html) klass. Kommunikation med webbplatser för hello åstadkoms med hjälp av [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).

hello följande diagram beskriver hello layout hello topologi:

![diagram över topologi](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> Det här diagrammet är en förenklad vy av hello-topologi. En instans av varje komponent skapas för varje partition i din Event Hub. Dessa instanser är fördelade på hello noder i klustret hello och data dirigeras mellan dem på följande sätt:
> 
> * Data från hello kanal toohello tolken är Utjämning av nätverksbelastning.
> * Data från hello parsern toohello instrumentpanelen och HBase är grupperad efter enhets-ID, så att meddelanden från hello samma enhet alltid flöda toohello samma komponent.

### <a name="topology-components"></a>Topologi komponenter

* **Event Hub-kanalen**: hello kanal har angetts som en del av Apache Storm-version 0.10.0 och högre.
  
  > [!NOTE]
  > hello Event Hub-kanal som används i det här exemplet kräver ett Storm på HDInsight-kluster av version 3.5 eller 3,6.

* **ParserBolt.java**: hello data som genereras av hello kanal är raw JSON och ibland fler än en händelse genereras i taget. Bulten läser hello data som sänds av hello prata och Parsar hello JSON-meddelande. hello bult skickar sedan hello data som en tuppel som innehåller flera fält.
* **DashboardBolt.java**: den här komponenten visar hur toouse hello Socket.io klientbibliotek för Java toosend data i realtid toohello web instrumentpanelen.
* **Ingen hbase.yaml**: hello topologi definition som används vid körning i lokalt läge. Den använder inte HBase-komponenter.
* **med hbase.yaml**: hello topologi definition används när du kör hello-topologi på hello klustret. Den använder HBase-komponenter.
* **dev.Properties**: hello konfigurationsinformation för hello Event Hub kanal, HBase bult och instrumentpanelskomponenter.

## <a name="prepare-your-environment"></a>Förbered din miljö

Innan du använder det här exemplet måste du skapa en Azure-Händelsehubb, vilka hello Storm-topologi läser från.

### <a name="configure-event-hub"></a>Konfigurera Event Hub

Event Hub är hello datakälla för det här exemplet. Använd följande steg toocreate en Händelsehubb hello.

1. Från hello [Azure-portalen](https://portal.azure.com)väljer **+ ny** -> **Sakernas Internet** -> **Händelsehubbar**.
2. I hello **skapa Namespace** avsnittet, utföra hello följande uppgifter:
   
   1. Ange en **namn** för hello namnområde.
   2. Välj en prisnivå. **Grundläggande** är tillräcklig för det här exemplet.
   3. Välj hello Azure **prenumeration** toouse.
   4. Välj en befintlig resursgrupp eller skapa en ny.
   5. Välj hello **plats** för hello Event Hub.
   6. Välj **PIN-kod toodashboard**, och klicka sedan på **skapa**.

3. När hello-processen är klar visas hello Händelsehubbar information för namnområdet. Här kan du välja **+ Lägg till Händelsehubben**. I hello **skapa Event Hub** ange ett namn på **sensordata**, och välj sedan **skapa**. Lämna hello andra fält till hello standardvärden.
4. Hej Händelsehubbar Se för namnområdet, Välj **Händelsehubbar**. Välj hello **sensordata** transaktionen.
5. Hej sensordata Händelsehubb, Välj **principer för delad åtkomst**. Använd hello **+ Lägg till** länk tooadd hello följande principer:

    | Principnamn | Anspråk |
    | ----- | ----- |
    | enheter | Skicka |
    | Storm | Lyssna |

1. Markera båda principer och anteckna hello **PRIMÄRNYCKEL** värde. Du behöver hello värde för båda principerna i kommande steg.

## <a name="download-and-configure-hello-project"></a>Hämta och konfigurera hello-projekt

Använd hello följande toodownload hello projektet från GitHub.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

När hello-kommandot har slutförts har hello följande katalogstruktur:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains hello topology
            resources/
                log4j2.xml - set logging toominimal.
                no-hbase.yaml - topology definition without hbase components.
                with-hbase.yaml - topology definition with hbase components.
            src/main/java/com/microsoft/examples/bolts/
                ParserBolt.java - parses JSON data into tuples
                DashboardBolt.java - sends data over Socket.IO toohello web dashboard.
        dashboard/nodejs/ - this is hello node.js web dashboard.
        SendEvents/ - utilities toosend fake sensor data.

> [!NOTE]
> Det går inte att det här dokumentet i toofull information om hello-kod som ingår i det här exemplet. Hello koden är helt kommenterad.

tooconfigure hello projektet tooread från Event Hub, öppna hello `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` och Lägg till din Event Hub information toohello följande rader:

```bash
eventhub.read.policy.name: your_read_policy_name
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: your_event_hub_name
eventhub.partitions: 2
```

## <a name="compile-and-test-locally"></a>Kompilera och testa lokalt

> [!IMPORTANT]
> Med hello topologi lokalt kräver en fungerande Storm-utvecklingsmiljö. Mer information finns i [ställa in en Storm-utvecklingsmiljö](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) på Apache.org.

> [!WARNING]
> Om du använder en Windows-utvecklingsmiljö, kan du få en `java.io.IOException` körs när hello topologi lokalt. I så fall, flytta toorunning hello-topologi på HDInsight.

Innan du testar, måste du starta hello instrumentpanelen tooview hello utdata från hello topologi och generera data toostore i Event Hub.

> [!IMPORTANT]
> Hej HBase-komponenten i den här topologin är inte aktivt när du testar lokalt. hello Java API för hello HBase-kluster kan inte nås från utanför hello Azure Virtual Network som innehåller hello-kluster.

### <a name="start-hello-web-application"></a>Starta hello-webbprogram

1. Öppna en kommandotolk och ändra sökvägen för`hdinsight-eventhub-example/dashboard`. Använd hello följande kommando tooinstall hello beroenden som krävs av hello-webbprogram:
   
    ```bash
    npm install
    ```

2. Använd hello följande kommando toostart hello-webbprogram:
   
    ```bash
    node server.js
    ```
   
    Du ser ett meddelande liknande toohello följande text:
   
        Server listening at port 3000

3. Öppna en webbläsare och ange `http://localhost:3000/` som hello-adress. En sida liknande toohello följande bild visas:
   
    ![instrumentpanel för webbprogram](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)
   
    Lämna den här Kommandotolken öppen. Efter testning, använda Ctrl-C toostop hello-webbserver.

### <a name="generate-data"></a>Generera data

> [!NOTE]
> hello använder stegen i det här avsnittet Node.js så att de kan användas på alla plattformar. Andra språk-exempel finns hello `SendEvents` directory.

1. Öppna en ny fråga eller shell terminal och ändra kataloger för`hdinsight-eventhub-example/SendEvents/nodejs`. tooinstall hello beroenden som krävs av programmet hello, Använd hello följande kommando:

    ```bash
    npm install
    ```

2. Öppna hello `app.js` i en textredigerare och Lägg till hello Event Hub-information som du tidigare hämtade:
   
    ```javascript
    // ServiceBus Namespace
    var namespace = 'YourNamespace';
    // Event Hub Name
    var hubname ='sensordata';
    // Shared access Policy name and key (from Event Hub configuration)
    var my_key_name = 'devices';
    var my_key = 'YourKey';
    ```
   
   > [!NOTE]
   > Det här exemplet förutsätter att du har använt `sensordata` som hello namnet på din Event Hub. Och att `devices` som hello namnet på hello-princip som har en `Send` anspråk.

3. Använd följande kommando tooinsert nya poster i Event Hub hello:
   
    ```bash
    node app.js
    ```
   
    Du ser flera rader på utdata som innehåller hello data skickas tooEvent hubb:
   
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="build-and-start-hello-topology"></a>Skapa och starta hello-topologi

1. Öppna en ny kommandotolk och ändra sökvägen för`hdinsight-eventhub-example/TemperatureMonitor`. toobuild och paketet hello topologi använder hello följande kommando: 

    ```bash
    mvn clean package
    ```

2. toostart Hej topologi i lokalt läge, Använd hello följande kommando:

    ```bash
    storm jar target/TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local --filter dev.properties resources/no-hbase.yaml
    ```

    * `--local`startar hello topologi i lokalt läge.
    * `--filter`använder hello `dev.properties` filen toopopulate parametrar i hello topologi definition.
    * `resources/no-hbase.yaml`använder hello `no-hbase.yaml` topologi definition.
 
   När den har startat, hello topologi läser poster från Händelsehubb och skickar dem toohello instrumentpanelen som körs på den lokala datorn. Du bör se rader som visas i hello webbinstrumentpanel, liknande toohello följande bild:
   
    ![instrumentpanel med data](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

2. När instrumentpanelen hello körs använder hello `node app.js` kommando från hello föregående steg toosend nya data tooEvent Hubs. Eftersom hello temperatur värden genereras slumpmässigt, bör hello diagram uppdatera tooshow stora förändringar i temperatur.
   
   > [!NOTE]
   > Du måste vara i hello **hdinsight-eventhub-exempel/SendEvents/Nodejs** directory när du använder hello `node app.js` kommando.

3. När du har verifierat hello instrumentpanelen uppdaterar stoppa hello topologi med Ctrl + C. Du kan också använda Ctrl + C toostop hello lokal webbserver.

## <a name="create-a-storm-and-hbase-cluster"></a>Skapa ett Storm och HBase-kluster

hello stegen i det här avsnittet används en [Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md) toocreate ett Azure-nätverk och en Storm och HBase-kluster på hello virtuellt nätverk. hello mallen även skapar en Azure Web App och distribuerar en kopia av hello instrumentpanelen till den.

> [!NOTE]
> Ett virtuellt nätverk används så att hello-topologi som körs på hello Storm-kluster kan kommunicera direkt med hello HBase-kluster med hjälp av hello HBase Java API.

hello Resource Manager-mall som används i det här dokumentet finns i en offentlig blob-behållare på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.

1. Klicka på hello efter knappen toosign i tooAzure och öppna hello Resource Manager-mall i hello Azure-portalen.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.6.json" target="_blank"><img src="./media/hdinsight-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. Från hello **anpassad distribution** ange hello följande värden:
   
    ![HDInsight-parametrar](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
   
   * **Basera klusternamnet**: det här värdet används som hello huvudnamnet för hello Storm och HBase-kluster. Ange till exempel **abc** skapar en Storm-kluster med namnet **storm abc** och ett HBase-kluster med namnet **hbase abc**.
   * **Klustrets inloggningsnamn**: hello administratörsanvändarnamnet för hello Storm och HBase-kluster.
   * **Klustret inloggningslösenordet**: hello administratörslösenord för hello Storm och HBase-kluster.
   * **SSH-användarnamn**: hello SSH användaren toocreate för hello Storm och HBase-kluster.
   * **SSH-lösenordet**: hello användarlösenord hello SSH för hello Storm och HBase-kluster.
   * **Plats**: hello region som hello kluster skapas i.
     
     Klicka på **OK** toosave hello parametrar.

3. Använd hello **grunderna** avsnittet toocreate en resursgrupp eller välj en befintlig.
4. I hello **resursgruppsplats** väljer hello samma plats som du valt för hello **plats** parameter i hello **inställningar** avsnitt.
5. Läs hello villkor och välj sedan **acceptera toohello villkoren ovan**.
6. Kontrollera slutligen **PIN-kod toodashboard** och välj sedan **inköp**. Det tar cirka 20 minuter toocreate hello kluster.

När hello resurserna har skapats, visas information om hello resursgruppen.

![Resursgruppen för hello vnet och kluster](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> Observera att hello namnen på hello HDInsight-kluster är **storm-BASENAME** och **hbase BASENAME**, där BASENAME är hello namn du angett toohello mall. Du kan använda dessa namn i ett senare steg när du ansluter toohello kluster. Observera att hello namnet på webbplatsen för hello instrumentpanelen är också **basename instrumentpanelen**. Det här värdet används senare i det här dokumentet.

## <a name="configure-hello-dashboard-bolt"></a>Konfigurera hello instrumentpanelen bult

toosend data toohello instrumentpanelen distribueras som en webbapp, måste du ändra följande rad i hello hello `dev.properties`fil:

```yaml
dashboard.uri: http://localhost:3000
```

Ändra `http://localhost:3000` för`http://BASENAME-dashboard.azurewebsites.net` och spara hello-filen. Ersätt **BASENAME** med hello grundläggande namnet du angav i föregående steg i hello. Du kan också använda hello resursgruppen som skapats tidigare tooselect hello instrumentpanelen och visa hello-URL.

## <a name="create-hello-hbase-table"></a>Skapa hello HBase-tabell

toostore data i HBase, vi måste först skapa en tabell. Skapa resurser som Storm måste toowrite att i förväg, eftersom den försöker toocreate resurser från inuti en Storm-topologi kan resultera i flera instanser försök toocreate hello samma resurs. Skapa hello resurser utanför hello topologi och använda Storm för läsning/skrivning och analyser.

1. Använda SSH tooconnect toohello HBase-kluster med hello SSH-användare och lösenord som du har angett toohello mall när klustret skapas. Om exempelvis ansluter hello `ssh` kommandot om du vill använda hello följande syntax:
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```
   
    Ersätt `sshuser` med hello SSH-användarnamnet du angav när du skapar hello kluster. Ersätt `clustername` med hello HBase klustrets namn.

2. Starta hello HBase-gränssnittet från hello SSH-session.
   
    ```bash
    hbase shell
    ```
   
    När hello shell har lästs in, finns en `hbase(main):001:0>` prompt.

3. Ange hello efter kommandot toocreate en tabell toostore hello sensordata från hello HBase-gränssnittet:
   
    ```hbase
    create 'SensorData', 'cf'
    ```

4. Kontrollera hello tabellen har skapats med hjälp av hello följande kommando:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Detta returnerar information liknande toohello följande exempel, som anger att det finns 0 rader i tabellen hello.
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Ange `exit` tooexit hello HBase-gränssnittet:

## <a name="configure-hello-hbase-bolt"></a>Konfigurera hello HBase bult

toowrite tooHBase från hello Storm-kluster, måste du ange hello HBase bulten med hello konfigurationsinformation för HBase-kluster.

1. Använd någon av följande exempel tooretrieve hello Zookeeper kvorum för HBase-kluster hello:

    ```bash
    CLUSTERNAME='your_HDInsight_cluster_name'
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HBASE/components/HBASE_MASTER" | jq '.metrics.hbase.master.ZookeeperQuorum'
    ```

    > [!NOTE]
    > Ersätt `your_HDInsight_cluster_name` med hello namnet på ditt HDInsight-kluster. Mer information om hur du installerar hello `jq` -verktyget, se [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).
    >
    > När du uppmanas du ange hello lösenord för inloggning för serveradministratör hello HDInsight.

    ```powershell
    $clusterName = 'your_HDInsight_cluster_name`
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HBASE/components/HBASE_MASTER" -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.metrics.hbase.master.ZookeeperQuorum
    ```

    > [!NOTE]
    > Ersätt ”your_HDInsight_cluster_name med hello namnet på ditt HDInsight-kluster. När du uppmanas du ange hello lösenord för inloggning för serveradministratör hello HDInsight.
    >
    > Det här exemplet kräver Azure PowerShell. Mer information om hur du använder Azure PowerShell finns [Kom igång med Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)

    hello information som returnerades av de här exemplen är liknande toohello följande text:

    `zk2-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk0-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk3-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181`

    Den här informationen används av Storm toocommunicate med hello HBase-kluster.

2. Ändra hello `dev.properties` och Lägg till hello Zookeeper kvorum information toohello följande rad:

    ```yaml
    hbase.zookeeper.quorum: your_hbase_quorum
    ```

## <a name="build-package-and-deploy-hello-solution-toohdinsight"></a>Skapa paket och distribuera hello lösning tooHDInsight

Använd hello följande steg toodeploy hello Storm-topologi toohello storm-kluster i din utvecklingsmiljö.

1. Från hello `TemperatureMonitor` katalogen Använd hello följande kommando tooperform en ny version och skapa ett JAR-paket från projektet:
   
        mvn clean package
   
    Det här kommandot skapar en fil med namnet `TemperatureMonitor-1.0-SNAPSHOT.jar in hello `mål-katalogen i ditt projekt.

2. Använd scp tooupload hello `TemperatureMonitor-1.0-SNAPSHOT.jar` och `dev.properties` filer tooyour Storm-kluster. Följande exempel Ersätt i hello `sshuser` med hello SSH-användare som du angav när du skapar hello kluster och `clustername` med hello namnet Storm-kluster. När du uppmanas du ange hello lösenord för hello SSH-användare.
   
    ```bash
    scp target/TemperatureMonitor-1.0-SNAPSHOT.jar dev.properties sshuser@clustername-ssh.azurehdinsight.net:
    ```

   > [!NOTE]
   > Det kan ta flera minuter tooupload hello filer.

    Mer information om hur du använder hello `scp` och `ssh` kommandon med HDInsight finns [använda SSH med HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md)

3. När hello-filen har överförts, ansluta toohello Storm-kluster med SSH.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Ersätt `sshuser` med hello SSH-användarnamn. Ersätt `clustername` med hello Storm klustrets namn.

4. toostart hello topologi, använder du följande kommando från SSH-session för hello hello:
   
    ```bash
    storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote --filter dev.properties -R /with-hbase.yaml
    ```

    * `--remote`skickar hello topologi toohello Nimbus-tjänsten, som distribuerar det toohello handledarens noder i klustret hello.
    * `--filter`använder hello `dev.properties` filen toopopulate parametrar i hello topologi definition.
    * `-R /with-hbase.yaml`använder hello `with-hbase.yaml` topologi som ingår i hello-paketet.

5. Öppna en webbläsare toohello webbplats som du har publicerat i Azure, sedan används hello när hello-topologi har börjat `node app.js` kommandot toosend data tooEvent hubb. Du bör se hello web instrumentpanelen toodisplay hello uppdateringsinformation.
   
    ![instrumentpanel](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>Visa data i HBase

Använd följande steg tooconnect tooHBase hello och kontrollera att hello data har skrivits toohello tabell:

1. Använda SSH tooconnect toohello HBase-kluster.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Ersätt `sshuser` med hello SSH-användarnamn. Ersätt `clustername` med hello HBase klustrets namn.

2. Starta hello HBase-gränssnittet från hello SSH-session.
   
    ```bash
    hbase shell
    ```
   
    När hello shell har lästs in, finns en `hbase(main):001:0>` prompt.

3. Visa rader från hello tabellen:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Det här kommandot returnerar informationen liknande toohello följande text, som anger att det finns data i hello tabell.
   
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds
   
   > [!NOTE]
   > Åtgärden genomsökning returnerar högst 10 raderna från hello tabell.

## <a name="delete-your-clusters"></a>Ta bort dina kluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

toodelete hello kluster, lagring och webb-app samtidigt, ta bort hello resursgruppen som innehåller dem..

## <a name="next-steps"></a>Nästa steg

Fler exempel på Storm-topologier med HDInsight finns i [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md)

Mer information om Apache Storm finns hello [Apache Storm](https://storm.incubator.apache.org/) plats.

Mer information om HBase i HDInsight finns hello [HBase med HDInsight Overview](hdinsight-hbase-overview.md).

Mer information om Socket.io finns hello [socket.io](http://socket.io/) plats.

Läs mer om D3.js [D3.js - Data drivs dokument](http://d3js.org/).

Information om hur du skapar topologier i Java finns [utveckla Java-topologier för Apache Storm på HDInsight](hdinsight-storm-develop-java-topology.md).

Information om hur du skapar topologier i .NET finns [utveckla C#-topologier för Apache Storm på HDInsight med Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
