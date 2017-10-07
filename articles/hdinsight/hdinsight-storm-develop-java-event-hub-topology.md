---
title: "aaaProcess händelser från Event Hubs med Storm på HDInsight använder Java | Microsoft Docs"
description: "Lär dig hur du skapar tooprocess Händelsehubbar data med en Java Storm-topologi med Maven."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 453fa7b0-c8a6-413e-8747-3ac3b71bed86
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/13/2017
ms.author: larryfr
ms.openlocfilehash: 6506f5bc8f6ab0e29350c071a3f84433382038e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-java"></a>Bearbeta händelser från Azure Event Hubs med Storm på HDInsight (Java)

Lär dig hur toouse Händelsehubbar i Azure med Storm på HDInsight. Det här exemplet använder Java-baserade komponenter tooread och skriva data i Händelsehubbar i Azure.

Händelsehubbar i Azure kan du tooprocess enorma mängder data från webbplatser, appar och enheter. hello Event Hub kanal gör det enkelt toouse Apache Storm på HDInsight tooanalyze dessa data i realtid. Du kan också skriva data tooEvent NAV från Storm med hjälp av hello Händelsehubbar bultar.

## <a name="prerequisites"></a>Krav

* En Apache Storm på HDInsight-kluster av version 3,6. Mer information finns i [Kom igång med Storm på HDInsight-kluster](hdinsight-apache-storm-tutorial-get-started-linux.md).

    > [!IMPORTANT]
    > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* En [Azure Event Hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

* [Oracle Java Developer Kit (JDK) version 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) eller motsvarande, såsom [OpenJDK](http://openjdk.java.net/).

* [Maven](https://maven.apache.org/download.cgi): Maven är ett projekt build-system för Java-projekt.

* En textredigerare eller integrerad utvecklingsmiljö (IDE).

    > [!NOTE]
    > Editor- eller IDE kan ha specifika funktioner för att arbeta med Maven som inte riktar sig i det här dokumentet. Information om hello funktioner för din miljö för redigering av dokumentationen hello för hello produkt som du använder.

    * En SSH-klient. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

* Hej `ssh` och `scp` kommandon. Dessa är används toocopy filer toohello HDInsight-kluster. Du kan hämta dessa via Bash på Windows 10 på Windows.

## <a name="understanding-hello-example"></a>Förstå hello-exempel

Hej [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) exempel innehåller två topologier:

Hej `resources/writer.yaml` topologi skriver slumpmässiga data tooan Azure Event Hub. hello data genereras hello `DeviceSpout` komponent, och är en slumpmässig enhets-ID och ett värde för enheten. Det därför simulera vissa maskinvara som genererar ett sträng-ID och ett numeriskt värde.

Dig `resources/reader.yaml` topologi läser data från Event Hub (hello data skrivits av EventHubWriter,) Parsar hello JSON-data och sedan loggar hello `deviceId` och `deviceValue` data.

hello data formateras som JSON-dokument innan den kan skrivas tooEvent hubb och när läses av hello reader tolkas utanför JSON och i tupplar. hello JSON-format är:

    { "deviceId": "unique identifier", "deviceValue": some value }

### <a name="project-configuration"></a>Konfigurationsinställningar

Hej `POM.xml` filen innehåller konfigurationsinformation för det här Maven-projekt. hello intressanta delar är:

#### <a name="event-hub-components"></a>Event Hub komponenter

hello-komponent som läser och skriver tooAzure Händelsehubbar finns i hello [HDInsight databasen](https://github.com/hdinsight/mvn-rep). följande avsnitt i hello hello `POM.xml` filen belastningen hello komponenter från den här lagringsplatsen

```xml
<repositories>
    <repository>
        <id>hdinsight-examples</id>
        <url>http://raw.github.com/hdinsight/mvn-repo/master</url>
    </repository>
</repositories>
```

#### <a name="hello-eventhubs-storm-spout-dependency"></a>Hej EventHubs Storm-kanalen beroende

```xml
<dependency>
    <groupId>com.microsoft</groupId>
    <artifactId>eventhubs</artifactId>
    <version>${storm.eventhub.version}</version>
</dependency>
```

Den här xml definierar ett beroende för hello eventhubs paket, som innehåller både en kanal för att läsa från Event Hubs och en bult för att skriva tooit.

```xml
</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

Den här xml konfigurerar hello projektet toogenerate utdata för Java 8, som används av HDInsight 3.5 eller högre.

#### <a name="hello-maven-shade-plugin"></a>Hej maven-skugga-plugin-program

```xml
<!-- build an uber jar -->
<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-shade-plugin</artifactId>
<version>2.3</version>
<configuration>
    <transformers>
    <!-- Keep us from getting a can't overwrite file error -->
    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer"/>
    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer"/>
    </transformers>
    <!-- Keep us from getting a bad signature error -->
    <filters>
    <filter>
        <artifact>*:*</artifact>
        <excludes>
            <exclude>META-INF/*.SF</exclude>
            <exclude>META-INF/*.DSA</exclude>
            <exclude>META-INF/*.RSA</exclude>
        </excludes>
    </filter>
    </filters>
</configuration>
<executions>
    <execution>
    <phase>package</phase>
    <goals>
        <goal>shade</goal>
    </goals>
    </execution>
</executions>
</plugin>
```

Den här xml konfigurerar hello lösning toopackage hello utdata till en uber jar. hello jar innehåller både hello Projektkod och nödvändiga beroenden. Det används också för att:

* Byt namn på licensfiler för hello beroenden.
* Exkludera security-signaturer.
* Se till att flera implementeringar av hello samma gränssnittet kombineras till en transaktion.

Dessa konfigurationsinställningar undvika fel vid körning.

#### <a name="topology-definitions"></a>Topologi definitioner

Det här exemplet används hello [som](https://storm.apache.org/releases/1.1.0/flux.html) framework. Det här ramverket använder YAML toodefine hello topologier. hello största fördelen är att du inte hårddisken kodning hello topologi i Java-kod. Du kan ändra den innan du skickar hello topologi, utan att behöva toorecompile allt eftersom hello definition YAML.

__Writer.yaml__:

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubwriter"

components:
  # Configure hello Event Hub spout
  - id: "eventhubbolt-config"
    className: "org.apache.storm.eventhubs.bolt.EventHubBoltConfig"
    constructorArgs:
      # These are populated from hello .properties file when hello topology is started
      - "${eventhub.write.policy.name}"
      - "${eventhub.write.policy.key}"
      - "${eventhub.namespace}"
      - "servicebus.windows.net"
      - "${eventhub.name}"

spouts:
  - id: "device-emulator-spout"
    className: "com.microsoft.example.DeviceSpout"
    parallelism: ${eventhub.partitions}

bolts:
  - id: "eventhub-bolt"
    className: "org.apache.storm.eventhubs.bolt.EventHubBolt"
    constructorArgs:
      - ref: "eventhubbolt-config" # config declared in components section
    # parallelism hint. This should be hello same as hello number of partitions for your Event Hub, so we read it from hello dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

# How data flows through hello components
streams:
  - name: "spout -> eventhub" # just a string used for logging
    from: "device-emulator-spout"
    to: "eventhub-bolt"
    grouping:
        type: SHUFFLE

  - name: "spout -> logger"
    from: "device-emulator-spout"
    to: "log-bolt"
    grouping:
        type: SHUFFLE
```

__Reader.yaml__:

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubreader"

components:
  # Configure hello Event Hub spout
  - id: "eventhubspout-config"
    className: "org.apache.storm.eventhubs.spout.EventHubSpoutConfig"
    constructorArgs:
      # These are populated from hello .properties file when hello topology is started
      - "${eventhub.read.policy.name}"
      - "${eventhub.read.policy.key}"
      - "${eventhub.namespace}"
      - "${eventhub.name}"
      - ${eventhub.partitions}

spouts:
  - id: "eventhub-spout"
    className: "org.apache.storm.eventhubs.spout.EventHubSpout"
    constructorArgs:
      - ref: "eventhubspout-config" # config declared in components section
    # parallelism hint. This should be hello same as hello number of partitions for your Event Hub, so we read it from hello dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

bolts:
  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

  # Parses from JSON into tuples
  - id: "parser-bolt"
    className: "com.microsoft.example.ParserBolt"
    parallelism: ${eventhub.partitions}

# How data flows through hello components
streams:
  - name: "spout -> parser" # just a string used for logging
    from: "eventhub-spout"
    to: "parser-bolt"
    grouping:
        type: SHUFFLE

  - name: "parser -> log-bolt"
    from: "parser-bolt"
    to: "log-bolt"
    grouping:
        type: SHUFFLE
```

#### <a name="tell-hello-topology-about-event-hub"></a>Berätta Event Hub hello-topologi

Vid körning hello `dev.properties` filen är används toopass hello Event Hub configuration toohello topologi. hello är följande exempel hello standardinnehållet i hello-fil:

```yaml
eventhub.write.policy.name: writer
eventhub.write.policy.key: your_key_here
eventhub.read.policy.name: reader
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: storm
eventhub.partitions: 2
```

## <a name="configure-environment-variables"></a>Konfigurera miljövariabler

hello kan följande miljövariabler anges när du installerar Java och hello JDK på utvecklingsdatorn. Dock bör du kontrollera att de finns och att de innehåller hello rätt värden för ditt system.

* **JAVA_HOME** -ska peka toohello katalog där hello Java runtime environment (JRE) har installerats. Till exempel i en Unix- eller Linux-distribution, den måste ha ett värde liknande för`/usr/lib/jvm/java-7-oracle`. I Windows, skulle det ha ett värde som är liknande för`c:\Program Files (x86)\Java\jre1.7`
* **SÖKVÄGEN** -bör innehålla hello följande sökvägar:

  * **JAVA_HOME** (eller motsvarande hello-sökväg)
  * **JAVA_HOME\bin** (eller motsvarande hello-sökväg)
  * hello katalog där Maven har installerats

## <a name="configure-event-hub"></a>Konfigurera Event Hub

Händelsehubbar är hello datakälla för det här exemplet. Använd följande steg toocreate en Händelsehubb hello.

1. Från hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **ny** > **Service Bus** > **Händelsehubb**  >  **Skapa anpassade**.

2. På hello **lägga till en ny Händelsehubb** anger en **Händelsehubbens namn**. Välj hello **Region** toocreate hello hubben i, och sedan skapa ett namnområde eller välj en befintlig. Klicka slutligen på hello **pilen** toocontinue.

    ![sida 1 i guiden](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

   > [!NOTE]
   > Välj hello samma **plats** som ditt Storm på HDInsight server tooreduce svarstid och kostnader.

3. På hello **konfigurera Event Hub** anger hello **partitions antal** och **meddelandet kvarhållning** värden. I det här exemplet använder du en partitionsantal 10 och ett meddelande kvarhållning av 1. Observera hello partitionsantal eftersom du behöver det här värdet senare.

    ![sida 2 i guiden](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

4. Efter hello händelsehubb har skapats, Välj hello namnområdet, Välj **Händelsehubbar**, och välj sedan hello händelsehubb som du skapade tidigare.
5. Välj **konfigurera**, sedan skapa två nya åtkomstprinciper för med hjälp av hello följande information:

    <table>
    <tr><th>Namn</th><th>Behörigheter</th></tr>
    <tr><td>Skrivare</td><td>Skicka</td></tr>
    <tr><td>Läsare</td><td>Lyssna</td></tr>
    </table>

    När du har skapat hello behörigheter väljer hello **spara** ikon på hello hello sidans nederkant. Dessa principer för delad åtkomst används tooread och skriva tooEvent hubb.

    ![principer](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

6. När du har sparat hello principer använda hello **nyckelgenerator för delad åtkomst** längst hello hello sidan tooretrieve hello nyckel för hello **writer** och **reader** principer. Spara de här nycklarna.

## <a name="download-and-build-hello-project"></a>Hämta och skapa hello-projekt

1. Hämta hello projektet från GitHub: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub). Du kan hämta hello paketet som ett zip-arkiv, eller använda [git](https://git-scm.com/) tooclone hello-projekt lokalt.

2. Ändra hello `dev.properties` fil med hello konfigurationen för din Händelsehubb.

3. Använd hello följande toobuild och paketet hello-projektet:

        mvn package

    Det här kommandot laddar ned nödvändiga beroendena, versioner, och sedan paket hello projektet. hello utdata lagras i hello **/target** katalogen som **EventHubExample-1.0-SNAPSHOT.jar**.

## <a name="test-locally"></a>Testa lokalt

Eftersom dessa topologier bara läsa och skriva tooEvent Hubs, kan du testa dem lokalt om du har en [Storm utvecklingsmiljö](http://storm.apache.org/releases/current/Setting-up-development-environment.html). Använd följande steg toorun lokalt i hello utvecklingsmiljö hello:

1. Kör hello writer:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /writer.yaml --filter dev.properties

2. Kör hello läsare:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /reader.yaml --filter dev.properties

> [!TIP]
> * `--local`: Kör hello topologi i lokalt läge (icke-distribuerade).
> * `-R /writer.yaml`: Läsa in definitionen av hello topologi från hello `resources` paketeras i hello jar. Ange hello sökvägen tooit som sista parameter i hello om hello topologin är en fil på hello lokala filsystem.
> * `--filter dev.properties`: Använd hello innehållet i `dev.properties` toofill i hello värden i hello topologi definitioner. Till exempel `${eventhub.read.policy.name}`.

Utdata är loggade toohello konsolen när du kör lokalt. Använd __Ctrl + C__ toostop hello-topologi.

## <a name="deploy-hello-topologies"></a>Distribuera hello topologier

1. Använd SCP toocopy hello jar paketet tooyour HDInsight-kluster. Ersätt användarnamn med hello SSH-användare för klustret. Ersätt KLUSTERNAMN med hello namnet på ditt HDInsight-kluster:

        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Om du använder ett lösenord för SSH-konto kan du ange tooenter hello lösenord. Om du har använt en SSH-nyckel med hello-konto kan du behöva toouse hello `-i` parametern toospecify hello sökvägen toohello nyckelfilen. Till exempel, `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

    Det här kommandot kopieras hello filen toohello-hemkataloger SSH-användare på hello klustret.

2. När hello-filen har överförts, kan du använda SSH tooconnect toohello HDInsight-kluster. Ersätt **användarnamn** hello namnet på SSH-inloggning. Ersätt **KLUSTERNAMN** med ditt HDInsight-klustrets namn:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!NOTE]
    > Om du använder ett lösenord för SSH-konto kan du ange tooenter hello lösenord. Om du har använt en SSH-nyckel med hello-konto kan du behöva toouse hello `-i` parametern toospecify hello sökvägen toohello nyckelfilen. hello följande exempel laddas hello privata nyckel från `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Använd följande kommando toostart hello topologier hello:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties

    > [!TIP]
    > * `--remote`: Skickar hello topologi toohello Nimbus-tjänsten, som börjar på hello arbetarnoder i klustret hello.

4. tooview hello loggade data gå toohttps://CLUSTERNAME.azurehdinsight.net/stormui, där __KLUSTERNAMN__ är hello namnet på ditt HDInsight-kluster. Välj hello topologier och detaljnivån toohello komponenter. Välj hello __port__ post för en instans av en komponent tooview loggade information.

5. Använd följande kommandon toostop hello topologier hello:

        storm kill reader
        storm kill writer

## <a name="delete-your-cluster"></a>Ta bort klustret

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nästa steg

* [Exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md)
