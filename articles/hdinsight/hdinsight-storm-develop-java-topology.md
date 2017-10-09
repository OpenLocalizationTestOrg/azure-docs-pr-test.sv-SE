---
title: aaaApache Storm-exempel Java-topologi - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toocreate Apache Storm-topologier i Java genom att skapa ett ord exempel räknar topologi."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: Apache storm, apache storm exempel storm java, storm-topologi exempel
ms.assetid: a8838f29-9c08-4fd9-99ef-26655d1bf6d7
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 54fa9dc3c93ddad83ac861f3101f50f80117d804
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-storm-topology-in-java"></a>Skapa en Apache Storm-topologi i Java

Lär dig hur toocreate en Java-baserad topologi för Apache Storm. Du skapar en Storm-topologi som implementerar ett ordräkning program. Du kan använda Maven toobuild och paketet hello-projekt. Sedan kan du lära dig hur toodefine hello topologi med hello som framework.

> [!NOTE]
> hello som framework är tillgänglig i Storm 0.10.0 eller senare. Storm 0.10.0 är tillgänglig med HDInsight 3.3 och 3.4.

När du har slutfört hello stegen i det här dokumentet kan du distribuera hello topologi tooApache Storm på HDInsight.

> [!NOTE]
> En fullständig version av hello Storm-topologi exempel som skapats i det här dokumentet finns på [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

## <a name="prerequisites"></a>Krav

* [Java Developer Kit (JDK) version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

* [Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven är ett projekt build-system för Java-projekt.

* En textredigerare eller IDE.

## <a name="configure-environment-variables"></a>Konfigurera miljövariabler

hello kan följande miljövariabler anges när du installerar Java och hello JDK. Dock bör du kontrollera att de finns och att de innehåller hello rätt värden för ditt system.

* **JAVA_HOME** -ska peka toohello katalog där hello Java runtime environment (JRE) har installerats. Till exempel i en Unix- eller Linux-distribution, den måste ha ett värde liknande för`/usr/lib/jvm/java-7-oracle`. I Windows, skulle det ha ett värde som är liknande för`c:\Program Files (x86)\Java\jre1.7`

* **SÖKVÄGEN** -bör innehålla hello följande sökvägar:

  * **JAVA_HOME** (eller motsvarande hello-sökväg)

  * **JAVA_HOME\bin** (eller motsvarande hello-sökväg)

  * hello katalog där Maven har installerats

## <a name="create-a-maven-project"></a>Skapa ett Maven-projekt

Från kommandoraden hello Använd hello följande kommando toocreate ett Maven-projekt med namnet **WordCount**:

```bash
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false
```

> [!NOTE]
> Om du använder PowerShell måste omges av`-D` parametrar med dubbla citattecken.
>
> `mvn archetype:generate "-DarchetypeArtifactId=maven-archetype-quickstart" "-DgroupId=com.microsoft.example" "-DartifactId=WordCount" "-DinteractiveMode=false"`

Det här kommandot skapar en katalog med namnet `WordCount` på hello aktuella plats, som innehåller en grundläggande Maven-projekt. Hej `WordCount` katalogen innehåller hello följande objekt:

* `pom.xml`: Innehåller inställningar för hello Maven-projekt.
* `src\main\java\com\microsoft\example`: Innehåller programkoden.
* `src\test\java\com\microsoft\example`: Innehåller tester för ditt program. 

### <a name="remove-hello-generated-example-code"></a>Ta bort hello genereras exempelkod

Ta bort hello genereras test- och programfiler hello:

* **src\test\java\com\microsoft\example\AppTest.Java**
* **src\main\java\com\microsoft\example\App.Java**

## <a name="add-maven-repositories"></a>Lägg till Maven-databaser

HDInsight är baserad på hello Hortonworks Data Platform (HDP), så vi rekommenderar att du använder hello Hortonworks databasen toodownload beroenden för Apache Storm-projekt. I hello __pom.xml__ lägger du till följande XML efter hello hello `<url>http://maven.apache.org</url>` rad:

```xml
<repositories>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPReleases</id>
        <name>HDP Releases</name>
        <url>http://repo.hortonworks.com/content/repositories/releases/</url>
        <layout>default</layout>
    </repository>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPJetty</id>
        <name>Hadoop Jetty</name>
        <url>http://repo.hortonworks.com/content/repositories/jetty-hadoop/</url>
        <layout>default</layout>
    </repository>
</repositories>
```

## <a name="add-properties"></a>Lägga till egenskaper

Maven kan toodefine projektnivå värden kallas egenskaper. I hello __pom.xml__, Lägg till följande text efter hello hello `</repositories>` rad:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from hello Hortonworks repository that is compatible with HDInsight.
    -->
    <storm.version>1.0.1.2.5.3.0-37</storm.version>
</properties>
```

Nu kan du använda det här värdet i andra avsnitt i hello `pom.xml`. Till exempel när du anger hello version av Storm-komponenter, du kan använda `${storm.version}` i stället för hårda kodning ett värde.

## <a name="add-dependencies"></a>Lägga till beroenden

Lägg till ett beroende för Storm-komponenter. Öppna hello `pom.xml` och Lägg till följande kod i hello hello `<dependencies>` avsnitt:

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of hello jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

Vid kompileringen, Maven använder denna information toolook `storm-core` hello Maven på lagringsplatsen. Först genomsöks hello-databasen på den lokala datorn. Om hello filer finns inte Maven laddar ned dem från hello offentliga Maven-databasen och lagrar dem i hello lokal databas.

> [!NOTE]
> Meddelande hello `<scope>provided</scope>` raden i det här avsnittet. Den här inställningen instruerar Maven tooexclude **storm-core** från alla JAR-filer som har skapats, eftersom det ges av hello system.

## <a name="build-configuration"></a>Skapa konfiguration

Maven plugin-program kan du toocustomize hello build led i hello-projekt. Till exempel hur hello projekt kompileras eller hur toopackage till JAR-filen. Öppna hello `pom.xml` och Lägg till följande kod direkt ovanför hello hello `</project>` rad.

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

Det här avsnittet är används tooadd plugin-program, resurser och andra build-konfigurationsalternativ. Fullständiga referenser för hello **pom.xml** fil, se [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

### <a name="add-plug-ins"></a>Lägga till plugin-program

Apache Storm-topologier som har implementerats i Java hello [Exec Maven Plugin](http://www.mojohaus.org/exec-maven-plugin/) är användbar eftersom den tillåter tooeasily kör hello topologi lokalt i din utvecklingsmiljö. Lägg till följande toohello hello `<plugins>` avsnitt i hello `pom.xml` tooinclude hello Exec Maven plugin-filen:

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>1.4.0</version>
    <executions>
    <execution>
    <goals>
        <goal>exec</goal>
    </goals>
    </execution>
    </executions>
    <configuration>
    <executable>java</executable>
    <includeProjectDependencies>true</includeProjectDependencies>
    <includePluginDependencies>false</includePluginDependencies>
    <classpathScope>compile</classpathScope>
    <mainClass>${storm.topology}</mainClass>
    <cleanupDaemonThreads>false</cleanupDaemonThreads> 
    </configuration>
</plugin>
```

En annan användbar plugin-program är hello [Apache Maven-kompilatorn Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/), vilket är används toochange alternativ för kompilering. hello ändringar hello Java-version som Maven använder för hello källa och mål för ditt program.

* För HDInsight __3,4 eller tidigare__, ange hello källa och mål Java version too__1.7__.

* För HDInsight __3.5__, ange hello källa och mål Java version too__1.8__.

Lägg till följande text i hello hello `<plugins>` avsnitt i hello `pom.xml` tooinclude hello Apache Maven kompileraren plugin-filen. Det här exemplet anger 1.8, så hello HDInsight målversionen är 3.5.

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.3</version>
    <configuration>
    <source>1.8</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

### <a name="configure-resources"></a>Konfigurera resurser

hello resurser avsnittet kan du tooinclude-kod resurser, till exempel konfigurationsfiler som krävs av komponenter i hello-topologi. I det här exemplet lägger du till följande text i hello hello `<resources>` avsnitt i hello-filen pom.xml.

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
        <include>log4j2.xml</include>
    </includes>
</resource>
```

Det här exemplet lägger till hello resurser katalog i hello rot hello projekt (`${basedir}`) som en plats som innehåller resurser och hello-fil med namnet `log4j2.xml`. Den här filen är används tooconfigure vilken information som loggas av hello-topologi.

## <a name="create-hello-topology"></a>Skapa hello-topologi

En Java-baserad Apache Storm-topologi som består av tre komponenter som du måste skapa (eller referens) som ett beroende.

* **Spouts**: läser data från externa datakällor och skickar dataströmmar till hello-topologi.

* **Bolts**: utför bearbetning på dataströmmar som orsakat av kanaler eller andra bultar och genererar en eller flera strömmar.

* **Topologi**: definierar hur hello spouts och bultar ordnas och tillhandahåller en startpunkt för hello för hello-topologi.

### <a name="create-hello-spout"></a>Skapa hello kanal

tooreduce krav för att installera externa datakällor hello följande kanal bara genererar slumpmässiga meningar. Det är en modifierad version av en kanal som medföljer hello [Storm Starter-exempel](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).

> [!NOTE]
> Ett exempel på en kanal som läser från en extern datakälla finns i följande exempel hello:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): en exempel-kanal som läser från Twitter
> * [Storm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): en kanal som läser från Kafka

Skapa en fil med namnet för hello kanal `RandomSentenceSpout.java` i hello `src\main\java\com\microsoft\example` katalogen och Använd hello följande Java-kod som hello innehållet:

```java
package com.microsoft.example;

import org.apache.storm.spout.SpoutOutputCollector;
import org.apache.storm.task.TopologyContext;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseRichSpout;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Values;
import org.apache.storm.utils.Utils;

import java.util.Map;
import java.util.Random;

//This spout randomly emits sentences
public class RandomSentenceSpout extends BaseRichSpout {
  //Collector used tooemit output
  SpoutOutputCollector _collector;
  //Used toogenerate a random number
  Random _rand;

  //Open is called when an instance of hello class is created
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
  //Set hello instance collector toohello one passed in
    _collector = collector;
    //For randomness
    _rand = new Random();
  }

  //Emit data toohello stream
  @Override
  public void nextTuple() {
  //Sleep for a bit
    Utils.sleep(100);
    //hello sentences that are randomly emitted
    String[] sentences = new String[]{ "hello cow jumped over hello moon", "an apple a day keeps hello doctor away",
        "four score and seven years ago", "snow white and hello seven dwarfs", "i am at two with nature" };
    //Randomly pick a sentence
    String sentence = sentences[_rand.nextInt(sentences.length)];
    //Emit hello sentence
    _collector.emit(new Values(sentence));
  }

  //Ack is not implemented since this is a basic example
  @Override
  public void ack(Object id) {
  }

  //Fail is not implemented since this is a basic example
  @Override
  public void fail(Object id) {
  }

  //Declare hello output fields. In this case, an sentence
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("sentence"));
  }
}
```

> [!NOTE]
> Även om den här topologin använder endast en kanal kan har andra flera som går in data från olika källor i hello-topologi.

### <a name="create-hello-bolts"></a>Skapa hello bultar

Bultar hantera hello databearbetning. Den här topologin används två bultar:

* **SplitSentence**: delar hello meningar sänds av **RandomSentenceSpout** till enskilda ord.

* **WordCount**: räknar hur många gånger varje ord har uppstått.

> [!NOTE]
> Bultar kan göra något, till exempel beräkning, beständiga eller pratar tooexternal komponenter.

Skapa två nya filer `SplitSentence.java` och `WordCount.java` i hello `src\main\java\com\microsoft\example` directory. Använd följande text som hello innehållet för hello filer hello:

#### <a name="splitsentence"></a>SplitSentence

```java
package com.microsoft.example;

import java.text.BreakIterator;

import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class SplitSentence extends BaseBasicBolt {

  //Execute is called tooprocess tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //Get hello sentence content from hello tuple
    String sentence = tuple.getString(0);
    //An iterator tooget each word
    BreakIterator boundary=BreakIterator.getWordInstance();
    //Give hello iterator hello sentence
    boundary.setText(sentence);
    //Find hello beginning first word
    int start=boundary.first();
    //Iterate over each word and emit it toohello output stream
    for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
      //get hello word
      String word=sentence.substring(start,end);
      //If a word is whitespace characters, replace it with empty
      word=word.replaceAll("\\s+","");
      //if it's an actual word, emit it
      if (!word.equals("")) {
        collector.emit(new Values(word));
      }
    }
  }

  //Declare that emitted tuples contain a word field
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word"));
  }
}
```

#### <a name="wordcount"></a>WordCount

```java
package com.microsoft.example;

import java.util.HashMap;
import java.util.Map;
import java.util.Iterator;

import org.apache.storm.Constants;
import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;
import org.apache.storm.Config;

// For logging
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class WordCount extends BaseBasicBolt {
  //Create logger for this class
  private static final Logger logger = LogManager.getLogger(WordCount.class);
  //For holding words and counts
  Map<String, Integer> counts = new HashMap<String, Integer>();
  //How often tooemit a count of words
  private Integer emitFrequency;

  // Default constructor
  public WordCount() {
      emitFrequency=5; // Default too60 seconds
  }

  // Constructor that sets emit frequency
  public WordCount(Integer frequency) {
      emitFrequency=frequency;
  }

  //Configure frequency of tick tuples for this bolt
  //This delivers a 'tick' tuple on a specific interval,
  //which is used tootrigger certain actions
  @Override
  public Map<String, Object> getComponentConfiguration() {
      Config conf = new Config();
      conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
      return conf;
  }

  //execute is called tooprocess tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //If it's a tick tuple, emit all words and counts
    if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
            && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
      for(String word : counts.keySet()) {
        Integer count = counts.get(word);
        collector.emit(new Values(word, count));
        logger.info("Emitting a count of " + count + " for word " + word);
      }
    } else {
      //Get hello word contents from hello tuple
      String word = tuple.getString(0);
      //Have we counted any already?
      Integer count = counts.get(word);
      if (count == null)
        count = 0;
      //Increment hello count and store it
      count++;
      counts.put(word, count);
    }
  }

  //Declare that this emits a tuple containing two fields; word and count
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word", "count"));
  }
}
```

### <a name="define-hello-topology"></a>Definiera hello-topologi

hello topologi kopplar samman hello kanaler och bolts tillsammans i ett diagram som definierar hur data flödar mellan hello komponenter. Det ger också parallellitet tips som Storm använder när du skapar instanser av hello komponenter i hello kluster.

hello är följande bild ett grundläggande diagram hello diagrammet med komponenter för den här topologin.

![diagram som visar hello spouts och bolts placering](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

tooimplement Hej topologi, skapa en fil med namnet `WordCountTopology.java` i hello `src\main\java\com\microsoft\example` directory. Använd hello följande Java-kod som hello innehållet i hello-fil:

```java
package com.microsoft.example;

import org.apache.storm.Config;
import org.apache.storm.LocalCluster;
import org.apache.storm.StormSubmitter;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.tuple.Fields;

import com.microsoft.example.RandomSentenceSpout;

public class WordCountTopology {

  //Entry point for hello topology
  public static void main(String[] args) throws Exception {
  //Used toobuild hello topology
    TopologyBuilder builder = new TopologyBuilder();
    //Add hello spout, with a name of 'spout'
    //and parallelism hint of 5 executors
    builder.setSpout("spout", new RandomSentenceSpout(), 5);
    //Add hello SplitSentence bolt, with a name of 'split'
    //and parallelism hint of 8 executors
    //shufflegrouping subscribes toohello spout, and equally distributes
    //tuples (sentences) across instances of hello SplitSentence bolt
    builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
    //Add hello counter, with a name of 'count'
    //and parallelism hint of 12 executors
    //fieldsgrouping subscribes toohello split bolt, and
    //ensures that hello same word is sent toohello same instance (group by field 'word')
    builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

    //new configuration
    Config conf = new Config();
    //Set toofalse toodisable debug information when
    // running in production on a cluster
    conf.setDebug(false);

    //If there are arguments, we are running on a cluster
    if (args != null && args.length > 0) {
      //parallelism hint tooset hello number of workers
      conf.setNumWorkers(3);
      //submit hello topology
      StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
    }
    //Otherwise, we are running locally
    else {
      //Cap hello maximum number of executors that can be spawned
      //for a component too3
      conf.setMaxTaskParallelism(3);
      //LocalCluster is used toorun locally
      LocalCluster cluster = new LocalCluster();
      //submit hello topology
      cluster.submitTopology("word-count", conf, builder.createTopology());
      //sleep
      Thread.sleep(10000);
      //shut down hello cluster
      cluster.shutdown();
    }
  }
}
```

### <a name="configure-logging"></a>Konfigurera loggning

Storm använder Apache Log4j toolog information. Om du inte konfigurerar loggning avger hello topologi diagnostisk information. toocontrol vad är inloggad, skapa en fil med namnet `log4j2.xml` i hello `resources` directory. Använd hello följande XML som hello innehållet i hello-fil.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
<Appenders>
    <Console name="STDOUT" target="SYSTEM_OUT">
        <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
</Appenders>
<Loggers>
    <Logger name="com.microsoft.example" level="trace" additivity="false">
        <AppenderRef ref="STDOUT"/>
    </Logger>
    <Root level="error">
        <Appender-Ref ref="STDOUT"/>
    </Root>
</Loggers>
</Configuration>
```

Den här XML konfigurerar en ny loggare för hello `com.microsoft.example` -klassen, som innehåller hello komponenter i det här exemplet-topologi. hello anges tootrace för den här loggaren som samlar in alla loggningsinformation orsakat av komponenter i den här topologin.

Hej `<Root level="error">` avsnittet konfigureras hello rotnivå loggning (allt inte i `com.microsoft.example`) tooonly logginformation för fel.

Läs mer om hur du konfigurerar loggning för Log4j [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [!NOTE]
> Storm-version 0.10.0 och högre använder Log4j 2.x. Äldre versioner av storm används Log4j 1.x, som används av ett annat format för konfigurationen av loggen. Mer information om hello äldre konfiguration finns [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

## <a name="test-hello-topology-locally"></a>Testa hello lokalt topologi

När du har sparat hello filer, kommando Använd hello följande lokalt tootest hello-topologi.

```bash
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

När den kör visar hello topologi startinformation. hello är följande text ett exempel på hello word antal utdata:

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Det här exemplet loggen visar hello ordet ”och” har släppts 113 gånger. hello antal fortsätter toogo upp så länge hello topologi körs eftersom hello kanal avger alltid hello samma meningar.

Det finns ett 5 sekunder intervall mellan utsläpp av ord och antal. Hej **WordCount** -komponenten har konfigurerats tooonly generera information när en tidstuppel tas emot. Begär att skalstreck tupplar levereras endast var femte sekund.

## <a name="convert-hello-topology-tooflux"></a>Konvertera hello topologi tooFlux

Som är ett nytt regelverk tillgänglig med Storm 0.10.0 och högre, vilket gör att du tooseparate konfigurationen från implementering. Komponenterna definieras fortfarande i Java, men hello topologi definieras med hjälp av en YAML-fil. Du kan paketdefinition en standard-topologi med projektet eller använda en fristående fil när du skickar in hello-topologi. När hello topologi tooStorm, kan du använda miljövariabler eller konfigurationsvärden filer toopopulate i hello YAML topologi definition.

Hej YAML filen definierar hello komponenter toouse för hello topologi och hello dataflödet mellan dem. Du kan inkludera en YAML-fil som en del av hello jar-filen eller så kan du använda en extern YAML-fil.

Mer information om som finns [som framework (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

> [!WARNING]
> På grund av tooa [programfel (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) med Storm 1.0.1 behöva tooinstall en [Storm utvecklingsmiljö](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) toorun som lokalt topologier.

1. Flytta hello `WordCountTopology.java` filen utanför hello-projekt. Hello-topologi har definierats tidigare den här filen, men behövs inte med som.

2. I hello `resources` directory, skapa en fil med namnet `topology.yaml`. Använd hello följande text som hello innehållet i den här filen.

        name: "wordcount"       # friendly name for hello topology
        
        config:                 # Topology configuration
        topology.workers: 1     # Hint for hello number of workers toocreate
        
        spouts:                 # Spout definitions
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            parallelism: 1      # parallelism hint
        
        bolts:                  # Bolt definitions
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1
         
        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
                - 10
            parallelism: 1
        
        streams:                # Stream definitions
            - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            from: "sentence-spout"       # hello stream emitter
            to: "splitter-bolt"          # hello stream consumer
            grouping:                    # Grouping type
                type: SHUFFLE
          
            - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
                args: ["word"]           # field(s) toogroup on

3. Gör följande ändringar toohello hello `pom.xml` fil.
   
   * Lägg till följande nya beroendet hello hello `<dependencies>` avsnitt:
     
        ```xml
        <!-- Add a dependency on hello Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```
   * Lägg till följande plugin-programmet toohello hello `<plugins>` avsnitt. Det här plugin-programmet hanterar hello ett paket (jar-fil) skapas för hello projekt och gäller vissa specifika tooFlux transformationer när du skapar hello-paketet.
     
        ```xml
        <!-- build an uber jar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
                <transformers>
                    <!-- Keep us from getting a "can't overwrite file error" -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                    <!-- We're using Flux, so refer tooit as main -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>org.apache.storm.flux.Flux</mainClass>
                    </transformer>
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

   * I hello **exec-maven-plugin-programmet** `<configuration>` ändrar hello värde för `<mainClass>` för`org.apache.storm.flux.Flux`. Den här inställningen som toohandle körs lokalt hello topologi under utveckling.

   * I hello `<resources>` lägger du till följande toohello hello `<includes>`. Den här XML innehåller hello YAML-fil som definierar hello topologi som en del av hello-projekt.

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-hello-flux-topology-locally"></a>Testa hello som lokalt topologi

1. Använd följande toocompile hello och kör hello som topologi med Maven:

    ```bash
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    Om du använder PowerShell använder du följande kommando hello:

    ```bash
    mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"
    ```

    > [!WARNING]
    > Om din topologi använder Storm 1.0.1 bits, misslyckas kommandot. Felet orsakas av [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055). I stället [installera Storm i din utvecklingsmiljö](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) och Använd hello följande information.

    Om du har [installerat Storm i din utvecklingsmiljö](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), du kan använda följande kommandon i stället hello:

    ```bash
    mvn compile package
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    ```

    Hej `--local` parametern körs hello topologi i lokalt läge på din utvecklingsmiljö. Hej `-R /topology.yaml` parametern använder hello `topology.yaml` filen resurs från hello jar-filen toodefine hello topologi.

    När den kör visar hello topologi startinformation. hello efter texten är ett exempel på utdata hello:

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs

    Det finns en fördröjning på 10 sekunder mellan batchar av loggade informationen.

2. Skapa en kopia av hello `topology.yaml` filen från hello-projekt. Namnet hello nya filen `newtopology.yaml`. I hello `newtopology.yaml` fil, hitta hello följande avsnitt och ändra hello värdet på `10` för`5`. Ändring ändringar hello intervallet mellan avger batchar av word räknar från too5 10 sekunder.

    ```yaml
    - id: "counter-bolt"
    className: "com.microsoft.example.WordCount"
    constructorArgs:
    - 5
    parallelism: 1
    ```yaml

3. toorun hello topology, use hello following command:

    ```bash
    mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"
    ```

    Eller, om du använder Storm på din utvecklingsmiljö:

    ```bash
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml
    ```

    Ändra hello `/path/to/newtopology.yaml` toohello sökväg toohello newtopology.yaml fil du skapade i föregående steg i hello. Det här kommandot använder hello newtopology.yaml som hello topologi definition. Eftersom vi skickat hello `compile` parametern Maven använder hello version av hello-projekt som skapats i föregående steg.

    En gång hello topologi börjar, bör upptäcker du hello tiden mellan angivna batchar har tooreflect hello värdet i newtopology.yaml. Så att du kan se att du kan ändra konfigurationen via en YAML utan toorecompile hello-topologi.

Mer information om dessa och andra funktioner i hello som framework finns [som (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

## <a name="trident"></a>Trident

Trident är en abstraktion på hög nivå som tillhandahålls av Storm. Det stöder tillståndskänslig bearbetning. hello viktig fördel med Trident är att det garanterar att varje meddelande som anger hello topologi bearbetas bara en gång. Utan att använda Trident garantera topologin endast att meddelanden bearbetas minst en gång. Det finns även andra skillnader, till exempel inbyggda komponenter som kan användas i stället för att skapa bultar. I själva verket ersättas bultar med mindre generisk komponenter, till exempel filter, projektioner och funktioner.

Trident program kan skapas med hjälp av Maven-projekt. Du använder hello samma grundläggande steg som visas tidigare i den här artikeln – endast hello koden är olika. Heller Trident kan inte (för närvarande) användas med hello som framework.

Mer information om Trident finns hello [Trident API översikt](http://storm.apache.org/documentation/Trident-API-Overview.html).

Ett exempel på en Trident-program finns i [Twitter trender avsnitt med Apache Storm på HDInsight](hdinsight-storm-twitter-trending.md).

## <a name="next-steps"></a>Nästa steg

Du har lärt dig hur toocreate en Storm-topologi med Java. Nu Lär dig hur du:

* [Distribuera och hantera Apache Storm-topologier på HDInsight](hdinsight-storm-deploy-monitor-topology.md)

* [Utveckla C#-topologier för Apache Storm på HDInsight med Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Du hittar fler exempel på Storm-topologier genom att besöka [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md).

