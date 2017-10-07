---
title: aaaApache Storm skriva tooStorage/Data Lake Store - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse hello Apache Storm toowrite toohello HDFS-kompatibla lagring för HDInsight. Azure Storage eller Azure Data Lake Store ger hello HDFS comptabile lagring för HDInsight. Det här dokumentet och hello associerade exemplet visar hur hello HdfsBolt komponenten kan vara används toowrite toohello standardlagring av ett Storm på HDInsight-kluster."
services: hdinsight
documentationcenter: na
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 1df98653-a6c8-4662-a8c6-5d288fc4f3a6
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/19/2017
ms.author: larryfr
ms.openlocfilehash: d76159a9ecd1be18e519511cfdb3bcfd18ae4d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="write-toohdfs-from-apache-storm-on-hdinsight"></a>Skriva tooHDFS från Apache Storm på HDInsight

Lär dig hur toouse Storm toowrite data toohello HDFS-kompatibla lagring användas av Apache Storm på HDInsight. HDInsight kan använda båda Azure Storage- och Azure Data Lake lagra som HDFS comptabile lagring. Storm tillhandahåller en [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) komponent som skriver data tooHDFS. Det här dokumentet innehåller information om hur du skriver tooeither typer av lagring från hello HdfsBolt. 

> [!IMPORTANT]
> hello exempel topologi används i det här dokumentet är beroende av komponenter som ingår i Storm på HDInsight. Det kan kräva ändring toowork med Azure Data Lake Store tillsammans med andra Apache Storm-kluster.

## <a name="get-hello-code"></a>Hämta hello kod

hello-projekt som innehåller den här topologin är tillgänglig för hämtning från [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).

toocompile det här projektet du behöver följande konfiguration för din utvecklingsmiljö hello:

* [Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) eller högre. HDInsight 3.5 eller högre krävs Java 8.

* [Maven 3.x](https://maven.apache.org/download.cgi)

hello kan följande miljövariabler anges när du installerar Java och hello JDK på utvecklingsdatorn. Dock bör du kontrollera att de finns och att de innehåller hello rätt värden för ditt system.

* `JAVA_HOME`-måste peka toohello katalog där hello JDK har installerats.
* `PATH`-bör innehålla hello följande sökvägar:
  
    * `JAVA_HOME`(eller motsvarande hello-sökväg).
    * `JAVA_HOME\bin`(eller motsvarande hello-sökväg).
    * hello katalog där Maven är installerat.

## <a name="how-toouse-hello-hdfsbolt-with-hdinsight"></a>Hur toouse hello HdfsBolt med HDInsight

> [!IMPORTANT]
> Innan du använder hello HdfsBolt med Storm på HDInsight, måste du först använda ett skript åtgärd toocopy krävs jar-filer i hello `extpath` för Storm. Mer information finns i hello [konfigurera hello klustret](#configure) avsnitt.

hello HdfsBolt använder hello filen schema för att ange toounderstand hur toowrite tooHDFS. Använd någon av följande scheman hello med HDInsight:

* `wasb://`: Används med ett Azure Storage-konto.
* `adl://`: Används med Azure Data Lake Store.

hello innehåller följande tabell exempel på användning av hello filen schemat för olika scenarier:

| schemat | Anteckningar |
| ----- | ----- |
| `wasb:///` | hello standardkontot för lagring är en blobbbehållare i ett Azure Storage-konto |
| `adl:///` | hello standardkontot för lagring är en katalog i Azure Data Lake Store. När klustret skapas ange hello directory i Data Lake Store är hello roten hello klustret HDFS. Till exempel hello `/clusters/myclustername/` directory. |
| `wasb://CONTAINER@ACCOUNT.blob.core.windows.net/` | En icke-standard (ytterligare) Azure storage-konto som är associerade med hello-kluster. |
| `adl://STORENAME/` | hello roten för hello Data Lake Store används av hello klustret. Det här schemat kan du tooaccess data som finns utanför hello-katalog som innehåller hello kluster-filsystemet. |

Mer information finns i hello [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) referens på Apache.org.

### <a name="example-configuration"></a>Exempel på konfiguration

hello följande YAML är hämtad från hello `resources/writetohdfs.yaml` -fil som ingår i hello exempel. Den här filen definierar hello Storm-topologi med hello [som](https://storm.apache.org/releases/1.1.0/flux.html) ramverk för Apache Storm.

```yaml
components:
  - id: "syncPolicy"
    className: "org.apache.storm.hdfs.bolt.sync.CountSyncPolicy"
    constructorArgs:
      - 1000

  - id: "rotationPolicy"
    className: "org.apache.storm.hdfs.bolt.rotation.NoRotationPolicy"

  - id: "fileNameFormat"
    className: "org.apache.storm.hdfs.bolt.format.DefaultFileNameFormat"
    configMethods:
      - name: "withPath"
        args: ["${hdfs.write.dir}"]
      - name: "withExtension"
        args: [".txt"]

  - id: "recordFormat"
    className: "org.apache.storm.hdfs.bolt.format.DelimitedRecordFormat"
    configMethods:
      - name: "withFieldDelimiter"
        args: ["|"]

# spout definitions
spouts:
  - id: "tick-spout"
    className: "com.microsoft.example.TickSpout"
    parallelism: 1


# bolt definitions
bolts:
  - id: "hdfs-bolt"
    className: "org.apache.storm.hdfs.bolt.HdfsBolt"
    configMethods:
      - name: "withConfigKey"
        args: ["hdfs.config"]
      - name: "withFsUrl"
        args: ["${hdfs.url}"]
      - name: "withFileNameFormat"
        args: [ref: "fileNameFormat"]
      - name: "withRecordFormat"
        args: [ref: "recordFormat"]
      - name: "withRotationPolicy"
        args: [ref: "rotationPolicy"]
      - name: "withSyncPolicy"
        args: [ref: "syncPolicy"]
```

Den här YAML definierar hello följande objekt:

* `syncPolicy`: Definierar när filer är synkroniserade/rensade toohello filsystem. I det här exemplet var 1000 tupplar.
* `fileNameFormat`: Definierar hello sökvägen och namnet mönster toouse när du skriver filer. I det här exemplet hello sökväg har angetts under körning med ett filter och hello filnamnstillägget är `.txt`.
* `recordFormat`: Definierar hello hello-filer som skrivits internt format. I det här exemplet fälten avgränsas med hello `|` tecken.
* `rotationPolicy`: Om du definierar när toorotate filer. I det här exemplet utförs ingen rotation.
* `hdfs-bolt`: Använder hello tidigare komponenter som konfigurationsparametrar för hello `HdfsBolt` klass.

Mer information om hello som framework finns [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).

## <a name="configure-hello-cluster"></a>Konfigurera hello-kluster

Som standard omfattar inte Storm på HDInsight hello komponenter att HdfsBolt använder toocommunicate med Azure Storage eller Azure Data Lake Store i Storms klassökvägen. Använd hello följande skript åtgärd tooadd dessa komponenter toohello `extlib` katalogen för Storm på klustret:

| Script URI | Noder tooapply att | Parametrar. `https://000aarperiscus.blob.core.windows.net/certs/stormextlib.sh` | Nimbus, handledarens | Ingen |

Information om hur du använder det här skriptet med ditt kluster finns i hello [anpassa HDInsight-kluster med skriptåtgärder](./hdinsight-hadoop-customize-cluster-linux.md) dokumentet.

## <a name="build-and-package-hello-topology"></a>Skapa och paketera hello-topologi

1. Hämta hello exempelprojekt från [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) tooyour utvecklingsmiljö.

2. Terminal eller shell-sessionen, ändra kataloger toohello rot hello hämtas projektet från en kommandotolk. toobuild och paketet hello topologi använder hello följande kommando:
   
        mvn compile package
   
    När hello bygg- och paketering är klar finns en ny katalog med namnet `target`, som innehåller en fil med namnet `StormToHdfs-1.0-SNAPSHOT.jar`. Den här filen innehåller hello kompileras topologi.

## <a name="deploy-and-run-hello-topology"></a>Distribuera och köra hello-topologi

1. Använd hello efter kommandot toocopy hello topologi toohello HDInsight-kluster. Ersätt **användaren** med hello SSH-användarnamn används när du skapar hello kluster. Ersätt **KLUSTERNAMN** med hello hello klustrets namn.
   
        scp target\StormToHdfs-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs1.0-SNAPSHOT.jar
   
    När du uppmanas ange hello lösenord som används när du skapar hello SSH-användare för hello-kluster. Om du använde en offentlig nyckel i stället för ett lösenord kan du behöva toouse hello `-i` parametern toospecify hello sökvägen toohello matchar privat nyckel.
   
   > [!NOTE]
   > Mer information om hur du använder `scp` med HDInsight, se [använda SSH med HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md).

2. När hello överföringen har slutförts kan du använda hello följande tooconnect toohello HDInsight-kluster med SSH. Ersätt **användaren** med hello SSH-användarnamn används när du skapar hello kluster. Ersätt **KLUSTERNAMN** med hello hello klustrets namn.
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    När du uppmanas ange hello lösenord som används när du skapar hello SSH-användare för hello-kluster. Om du använde en offentlig nyckel i stället för ett lösenord kan du behöva toouse hello `-i` parametern toospecify hello sökvägen toohello matchar privat nyckel.
   
   Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

3. När du är ansluten, Använd hello följande kommando toocreate en fil med namnet `dev.properties`:

        nano dev.properties

4. Använd hello följande text som hello hello `dev.properties` fil:

        hdfs.write.dir: /stormdata/
        hdfs.url: wasb:///

    > [!IMPORTANT]
    > Det här exemplet förutsätter att klustret använder ett Azure Storage-konto som hello standardlagring. Om klustret använder Azure Data Lake Store, Använd `hdfs.url: adl:///` i stället.
    
    toosave hello-fil, Använd __Ctrl + X__, sedan __Y__, och slutligen __RETUR__. hello-värden i den här filen ange Webbadressen för hello Data Lake store och hello katalognamn som data skrivs till.

3. Använd följande kommando toostart hello topologi hello:
   
        storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties

    Detta kommando startar hello topologi med hello som framework genom att skicka den toohello Nimbus-noden hello-klustret. hello topologi definieras av hello `writetohdfs.yaml` -fil som ingår i hello jar. Hej `dev.properties` filen skickas som ett filter och hello värden i hello filen läses av hello-topologi.

## <a name="view-output-data"></a>Visa utdata

tooview hello data, Använd hello följande kommando:

    hdfs dfs -ls /stormdata/

En lista över hello-filer som skapas av den här topologin visas.

hello är följande lista ett exempel på hello data retuned av hello tidigare kommandon:

    Found 30 items
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-0-1488568403092.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-1-1488568404567.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-10-1488568408678.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-11-1488568411636.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-12-1488568411884.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-13-1488568412603.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-14-1488568415055.txt

## <a name="stop-hello-topology"></a>Stoppa hello-topologi

Storm-topologier kör förrän stoppats eller hello kluster har tagits bort. toostop hello topologi, Använd hello följande kommando:

    storm kill hdfswriter

## <a name="delete-your-cluster"></a>Ta bort klustret

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur toouse Storm toowrite tooAzure lagring och Azure Data Lake Store kan identifiera andra [Storm-exempel för HDInsight](hdinsight-storm-example-topology.md).

