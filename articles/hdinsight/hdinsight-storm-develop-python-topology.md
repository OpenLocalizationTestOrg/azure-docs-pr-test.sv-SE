---
title: aaaApache Storm med Python comopnents - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toocreate en Apache Storm-topologi som använder Python-komponenter."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
keywords: Apache storm python
ms.assetid: edd0ec4f-664d-4266-910c-6ecc94172ad8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/04/2017
ms.author: larryfr
ms.openlocfilehash: 143c639623f1992f913900a7c52d6e3f03c701e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Utveckla Apache Storm-topologier med Python i HDInsight

Lär dig hur toocreate en Apache Storm-topologi som använder Python-komponenter. Apache Storm har stöd för flera språk och även så att du toocombine komponenter från flera språk i en topologi. hello som framework (introducerades med Storm 0.10.0) kan du tooeasily skapa lösningar som använder Python-komponenter.

> [!IMPORTANT]
> hello informationen i det här dokumentet har testats med Storm på HDInsight 3,6. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

hello-koden för det här projektet är tillgänglig på [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).

## <a name="prerequisites"></a>Krav

* Python 2.7 eller högre

* Java JDK 1.8 eller högre

* Maven 3

* (Valfritt) En lokal Storm-utvecklingsmiljö. En lokal miljö Storm krävs endast om du vill toorun hello topologi lokalt. Mer information finns i [ställa in en utvecklingsmiljö](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html).

## <a name="storm-multi-language-support"></a>Storm-stöd för flera språk

Apache Storm var utformad toowork med komponenter som skrivits med alla programmeringsspråk. hello-komponenter måste förstå hur toowork med hello [Thrift-definition för Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift). För Python, har en modul angetts som en del av hello Apache Storm-projekt som du kan använda tooeasily gränssnittet med Storm. Du hittar den här modulen på [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Storm är en Java-process som körs på hello Java Virtual Machine (JVM). Komponenter som skrivits i andra språk körs som delprocesser. hello Storm kommunicerar med dessa delprocesser som använder JSON-meddelanden som skickas över stdin/stdout. Mer information om kommunikation mellan komponenter finns i hello [lang Multi Protocol](https://storm.apache.org/documentation/Multilang-protocol.html) dokumentation.

## <a name="python-with-hello-flux-framework"></a>Python hello som Framework

hello som framework kan toodefine Storm-topologier separat från hello komponenter. hello som framework använder YAML toodefine hello Storm-topologi. hello följande är ett exempel på hur tooreference en Python-komponent i hello YAML dokument:

```yaml
# Spout definitions
spouts:
  - id: "sentence-spout"
    className: "org.apache.storm.flux.wrappers.spouts.FluxShellSpout"
    constructorArgs:
      # Command line
      - ["python", "sentencespout.py"]
      # Output field(s)
      - ["sentence"]
    # parallelism hint
    parallelism: 1
```

Hej klassen `FluxShellSpout` är används toostart hello `sentencespout.py` skript som implementerar hello-kanal.

Som förväntar hello Python skript toobe i hello `/resources` katalogen i hello jar-filen som innehåller hello-topologi. Så det här exemplet lagrar hello Python-skript i hello `/multilang/resources` directory. Hej `pom.xml` innehåller den här filen med hjälp av följande XML hello:

```xml
<!-- include hello Python components -->
<resource>
    <directory>${basedir}/multilang</directory>
    <filtering>false</filtering>
</resource>
```

Som tidigare nämnts är en `storm.py` fil som implementerar hello Thrift-definition för Storm. hello som framework inkluderar `storm.py` automatiskt när hello projektet har skapats, så du inte behöver tooworry om, inklusive den.

## <a name="build-hello-project"></a>Skapa hello-projekt

Använd hello följande kommando från hello-roten av hello projekt:

```bash
mvn clean compile package
```

Det här kommandot skapar en `target/WordCount-1.0-SNAPSHOT.jar` -fil som innehåller hello kompileras topologi.

## <a name="run-hello-topology-locally"></a>Kör hello topologi lokalt

toorun hello topologi lokalt, Använd hello följande kommando:

```bash
storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -l -R /topology.yaml
```

> [!NOTE]
> Kommandot kräver en lokal Storm-utvecklingsmiljö. Mer information finns i [ställa in en utvecklingsmiljö](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html)

En gång hello topologi startar den skickar information toohello lokala konsolen liknande toohello följande text:


    24302 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting hello cow jumped over hello moon
    24302 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting the
    24302 [Thread-28] INFO  o.a.s.t.ShellBolt - ShellLog pid:2437, name:counter-bolt Emitting years:160
    24302 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=the, count=599}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=seven, count=302}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=dwarfs, count=143}
    24303 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting hello cow jumped over hello moon
    24303 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting cow
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=four, count=160}


toostop hello topologi, Använd __Ctrl + C__.

## <a name="run-hello-storm-topology-on-hdinsight"></a>Kör hello Storm-topologi på HDInsight

1. Använd hello följande kommando toocopy hello `WordCount-1.0-SNAPSHOT.jar` filen tooyour Storm på HDInsight-kluster:

    ```bash
    scp target\WordCount-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net
    ```

    Ersätt `sshuser` med hello SSH-användare för klustret. Ersätt `mycluster` med hello klusternamnet. Du kanske ange tooenter hello lösenordet för hello SSH-användare.

    Mer information om hur du använder SSH och SCP finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. När hello-filen har överförts, ansluta toohello kluster med SSH:

    ```bash
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. Använd hello följande kommando från hello SSH-session toostart hello-topologin på hello klustret:

    ```bash
    storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -r -R /topology.yaml
    ```

3. Du kan använda hello Storm-Användargränssnittet tooview hello-topologi på hello klustret. hello Storm-Användargränssnittet finns på https://mycluster.azurehdinsight.net/stormui. Ersätt `mycluster` med klusternamnet.

> [!NOTE]
> När den har startat, kör en Storm-topologi förrän stoppats. toostop Hej topologi, med någon av följande metoder hello:
>
> * Hej `storm kill TOPOLOGYNAME` från hello kommandoraden
> * Hej **Kill** knapp i hello Storm-Användargränssnittet.


## <a name="next-steps"></a>Nästa steg

Se hello efter dokument för andra sätt toouse Python med HDInsight:

* [Hur toouse Python för strömning MapReduce-jobb](hdinsight-hadoop-streaming-python.md)
* [Hur toouse Python användaren definierat funktioner (UDF) i Pig och Hive](hdinsight-python.md)
