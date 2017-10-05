---
title: "Bearbeta händelser från Event Hubs med Storm på HDInsight använder Java | Microsoft Docs"
description: "Lär dig mer om att bearbeta data i Händelsehubbar med en Java Storm-topologi som skapats med Maven."
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
ms.openlocfilehash: 2e8ebbdab2be7bed224a67facec798820615bb22
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-java"></a>Bearbeta händelser från Azure Event Hubs med Storm på HDInsight (Java)

Lär dig hur du använder Azure Event Hubs med Storm på HDInsight. Det här exemplet använder Java-baserade komponenter för att läsa och skriva data i Händelsehubbar i Azure.

Händelsehubbar i Azure kan du bearbetar stora mängder data från webbplatser, appar och enheter. Event Hub-kanal gör det enkelt att använda Apache Storm på HDInsight för att analysera data i realtid. Du kan också skriva data till Händelsehubbar från Storm med hjälp av bulten Händelsehubbar.

## <a name="prerequisites"></a>Krav

* En Apache Storm på HDInsight-kluster av version 3,6. Mer information finns i [Kom igång med Storm på HDInsight-kluster](hdinsight-apache-storm-tutorial-get-started-linux.md).

    > [!IMPORTANT]
    > Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* En [Azure Event Hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

* [Oracle Java Developer Kit (JDK) version 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) eller motsvarande, såsom [OpenJDK](http://openjdk.java.net/).

* [Maven](https://maven.apache.org/download.cgi): Maven är ett projekt build-system för Java-projekt.

* En textredigerare eller integrerad utvecklingsmiljö (IDE).

    > [!NOTE]
    > Editor- eller IDE kan ha specifika funktioner för att arbeta med Maven som inte riktar sig i det här dokumentet. Information om funktionerna i din miljö för redigering finns i dokumentationen för produkten som du använder.

    * En SSH-klient. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

* Den `ssh` och `scp` kommandon. Dessa används för att kopiera filer till HDInsight-klustret. Du kan hämta dessa via Bash på Windows 10 på Windows.

## <a name="understanding-the-example"></a>Förstå exemplet

Den [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) exempel innehåller två topologier:

Den `resources/writer.yaml` topologi skriver slumpmässiga data till en Azure-Händelsehubb. Data som genereras av den `DeviceSpout` komponent, och är en slumpmässig enhets-ID och ett värde för enheten. Det därför simulera vissa maskinvara som genererar ett sträng-ID och ett numeriskt värde.

Dig `resources/reader.yaml` topologi läser data från Event Hub (data som skrivits av EventHubWriter,) Parsar JSON-data och sedan loggar den `deviceId` och `deviceValue` data.

Data formateras som JSON-dokument innan den kan skrivas till Händelsehubben och när läses av läsaren tolkas utanför JSON och i tupplar. JSON-formatet är:

    { "deviceId": "unique identifier", "deviceValue": some value }

### <a name="project-configuration"></a>Konfigurationsinställningar

Den `POM.xml` filen innehåller konfigurationsinformation för det här Maven-projekt. Intressanta delarna är:

#### <a name="event-hub-components"></a>Event Hub komponenter

Den komponent som läser och skriver till Azure Event Hubs finns i den [HDInsight databasen](https://github.com/hdinsight/mvn-rep). I de följande avsnitten i den `POM.xml` fil att läsa in komponenterna från den här lagringsplatsen

```xml
<repositories>
    <repository>
        <id>hdinsight-examples</id>
        <url>http://raw.github.com/hdinsight/mvn-repo/master</url>
    </repository>
</repositories>
```

#### <a name="the-eventhubs-storm-spout-dependency"></a>Beroendet EventHubs Storm-kanalen

```xml
<dependency>
    <groupId>com.microsoft</groupId>
    <artifactId>eventhubs</artifactId>
    <version>${storm.eventhub.version}</version>
</dependency>
```

Den här xml definierar ett beroende för eventhubs paket, som innehåller både en kanal för att läsa från Event Hubs och en bult för att skriva till den.

```xml
</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

Den här xml konfigurerar projektet för att generera utdata för Java 8, som används av HDInsight 3.5 eller högre.

#### <a name="the-maven-shade-plugin"></a>Maven-skugga-plugin-programmet

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

Den här xml konfigurerar lösningen för att paketera utdata till en uber jar. Jar innehåller både Projektkod och nödvändiga beroenden. Det används också för att:

* Byt namn på licensfiler för beroenden.
* Exkludera security-signaturer.
* Se till att flera implementeringar av samma gränssnitt slås samman till en transaktion.

Dessa konfigurationsinställningar undvika fel vid körning.

#### <a name="topology-definitions"></a>Topologi definitioner

Det här exemplet används den [som](https://storm.apache.org/releases/1.1.0/flux.html) framework. Det här ramverket använder YAML för att definiera topologierna. Den största fördelen är att du inte hårddisken kodning topologi i Java-kod. Du kan ändra den innan du skickar topologi, utan att kompilera om allt eftersom definitionen YAML.

__Writer.yaml__:

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubwriter"

components:
  # Configure the Event Hub spout
  - id: "eventhubbolt-config"
    className: "org.apache.storm.eventhubs.bolt.EventHubBoltConfig"
    constructorArgs:
      # These are populated from the .properties file when the topology is started
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
    # parallelism hint. This should be the same as the number of partitions for your Event Hub, so we read it from the dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

# How data flows through the components
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
  # Configure the Event Hub spout
  - id: "eventhubspout-config"
    className: "org.apache.storm.eventhubs.spout.EventHubSpoutConfig"
    constructorArgs:
      # These are populated from the .properties file when the topology is started
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
    # parallelism hint. This should be the same as the number of partitions for your Event Hub, so we read it from the dev.properties file passed at run time.
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

# How data flows through the components
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

#### <a name="tell-the-topology-about-event-hub"></a>Berätta Event Hub topologin

Vid körning av `dev.properties` används för att skicka Event Hub-konfigurationen till i topologin. Följande exempel är standardinnehållet i filen:

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

Följande miljövariabler kan anges när du installerar Java och JDK på utvecklingsdatorn. Dock bör du kontrollera att de finns och att de innehåller rätt värden för ditt system.

* **JAVA_HOME** -måste peka på den katalog där med Java runtime environment (JRE) har installerats. Till exempel en Unix- eller Linux-distribution, den inte innehålla ett värde som liknar `/usr/lib/jvm/java-7-oracle`. I Windows, skulle det ha ett värde som liknar`c:\Program Files (x86)\Java\jre1.7`
* **SÖKVÄGEN** -bör innehålla följande sökvägar:

  * **JAVA_HOME** (eller motsvarande sökväg)
  * **JAVA_HOME\bin** (eller motsvarande sökväg)
  * Katalogen där Maven är installerat

## <a name="configure-event-hub"></a>Konfigurera Event Hub

Händelsehubbar är datakällan för det här exemplet. Använd följande steg för att skapa en Händelsehubb.

1. Från den [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **ny** > **Service Bus** > **Händelsehubb** > **skapa anpassade**.

2. På den **lägga till en ny Händelsehubb** anger en **Händelsehubbens namn**. Välj den **Region** att skapa hubben i, och skapa ett namnområde eller välj en befintlig. Klicka slutligen på den **pilen** att fortsätta.

    ![sida 1 i guiden](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

   > [!NOTE]
   > Välj samma **plats** som ditt Storm på HDInsight-servern för att minska latensen och kostnader.

3. På den **konfigurera Event Hub** anger den **partitions antal** och **meddelandet kvarhållning** värden. I det här exemplet använder du en partitionsantal 10 och ett meddelande kvarhållning av 1. Observera antalet partitioner eftersom du behöver det här värdet senare.

    ![sida 2 i guiden](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

4. När händelsehubben har skapats, Välj namnområdet, Välj **Händelsehubbar**, och välj sedan händelsehubben som du skapade tidigare.
5. Välj **konfigurera**, sedan skapa två nya åtkomstprinciper för med hjälp av följande information:

    <table>
    <tr><th>Namn</th><th>Behörigheter</th></tr>
    <tr><td>Skrivare</td><td>Skicka</td></tr>
    <tr><td>Läsare</td><td>Lyssna</td></tr>
    </table>

    När du har skapat behörigheterna som du väljer den **spara** längst ned på sidan. Dessa principer för delad åtkomst används för att läsa och skriva till Händelsehubben.

    ![principer](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

6. När du har sparat principerna använder den **nyckelgenerator för delad åtkomst** längst ned på sidan för att hämta nyckeln för den **writer** och **reader** principer. Spara de här nycklarna.

## <a name="download-and-build-the-project"></a>Hämta och skapa projektet

1. Hämta projektet från GitHub: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub). Du kan hämta paketet som ett zip-arkiv, eller använda [git](https://git-scm.com/) att klona projektet lokalt.

2. Ändra den `dev.properties` fil med konfigurationen för din Händelsehubb.

3. Använd följande för att bygga och paket i projektet:

        mvn package

    Det här kommandot laddar ned nödvändiga beroendena, versioner, och paket i projektet. Utdata lagras i den **/target** katalogen som **EventHubExample-1.0-SNAPSHOT.jar**.

## <a name="test-locally"></a>Testa lokalt

Eftersom dessa topologier kan bara läsa och skriva till Event Hubs, kan du testa dem lokalt om du har en [Storm utvecklingsmiljö](http://storm.apache.org/releases/current/Setting-up-development-environment.html). Använd följande steg för att köra lokalt i dev-miljö:

1. Kör skrivaren:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /writer.yaml --filter dev.properties

2. Kör läsaren:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /reader.yaml --filter dev.properties

> [!TIP]
> * `--local`: Kör topologin i lokalt läge (icke-distribuerade).
> * `-R /writer.yaml`: Läsa in definitionen topologi från den `resources` paketeras i jar. Ange sökvägen till den som den sista parametern om topologin är en fil på det lokala filsystemet.
> * `--filter dev.properties`: Använd innehållet i `dev.properties` att fylla i värdena i topologin definitioner. Till exempel `${eventhub.read.policy.name}`.

Utdata loggas i konsolen när du kör lokalt. Använd __Ctrl + C__ att stoppa topologin.

## <a name="deploy-the-topologies"></a>Distribuera topologierna

1. Använd SCP för att kopiera jar-paket till ditt HDInsight-kluster. Ersätt användarnamn med SSH-användare för klustret. Ersätt KLUSTERNAMN med namnet på ditt HDInsight-kluster:

        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Om du använder ett lösenord för SSH-konto uppmanas du att ange lösenordet. Om du använder en SSH-nyckel med kontot, kan du behöva använda de `-i` parametern för att ange sökvägen till nyckelfilen. Till exempel, `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

    Det här kommandot kopieras filen till arbetskatalogen för SSH-användare i klustret.

2. När filen har överförts, kan du använda SSH för att ansluta till HDInsight-klustret. Ersätt **användarnamn** namnet på SSH-inloggning. Ersätt **KLUSTERNAMN** med ditt HDInsight-klustrets namn:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!NOTE]
    > Om du använder ett lösenord för SSH-konto uppmanas du att ange lösenordet. Om du använder en SSH-nyckel med kontot, kan du behöva använda de `-i` parametern för att ange sökvägen till nyckelfilen. I följande exempel laddas den privata nyckeln från `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Använd följande kommando för att starta topologierna:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties

    > [!TIP]
    > * `--remote`: Skickar topologi till Nimbus-tjänsten som börjar på worker-noder i klustret.

4. Du kan visa data som loggats i https://CLUSTERNAME.azurehdinsight.net/stormui, där __KLUSTERNAMN__ är namnet på ditt HDInsight-kluster. Välj topologierna och öka detaljnivån till komponenterna. Välj den __port__ post för en instans av en komponent som ska visa loggade informationen.

5. Använd följande kommandon för att stoppa topologierna:

        storm kill reader
        storm kill writer

## <a name="delete-your-cluster"></a>Ta bort klustret

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nästa steg

* [Exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md)
