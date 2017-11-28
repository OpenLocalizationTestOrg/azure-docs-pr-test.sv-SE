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
# <a name="write-toohdfs-from-apache-storm-on-hdinsight"></a><span data-ttu-id="37598-105">Skriva tooHDFS från Apache Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="37598-105">Write tooHDFS from Apache Storm on HDInsight</span></span>

<span data-ttu-id="37598-106">Lär dig hur toouse Storm toowrite data toohello HDFS-kompatibla lagring användas av Apache Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="37598-106">Learn how toouse Storm toowrite data toohello HDFS-compatible storage used by Apache Storm on HDInsight.</span></span> <span data-ttu-id="37598-107">HDInsight kan använda båda Azure Storage- och Azure Data Lake lagra som HDFS comptabile lagring.</span><span class="sxs-lookup"><span data-stu-id="37598-107">HDInsight can use both Azure Storage and Azure Data Lake store as HDFS-comptabile storage.</span></span> <span data-ttu-id="37598-108">Storm tillhandahåller en [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) komponent som skriver data tooHDFS.</span><span class="sxs-lookup"><span data-stu-id="37598-108">Storm provides an [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) component that writes data tooHDFS.</span></span> <span data-ttu-id="37598-109">Det här dokumentet innehåller information om hur du skriver tooeither typer av lagring från hello HdfsBolt.</span><span class="sxs-lookup"><span data-stu-id="37598-109">This document provides information on writing tooeither type of storage from hello HdfsBolt.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="37598-110">hello exempel topologi används i det här dokumentet är beroende av komponenter som ingår i Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="37598-110">hello example topology used in this document relies on components that are included with Storm on HDInsight.</span></span> <span data-ttu-id="37598-111">Det kan kräva ändring toowork med Azure Data Lake Store tillsammans med andra Apache Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="37598-111">It may require modification toowork with Azure Data Lake Store when used with other Apache Storm clusters.</span></span>

## <a name="get-hello-code"></a><span data-ttu-id="37598-112">Hämta hello kod</span><span class="sxs-lookup"><span data-stu-id="37598-112">Get hello code</span></span>

<span data-ttu-id="37598-113">hello-projekt som innehåller den här topologin är tillgänglig för hämtning från [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="37598-113">hello project containing this topology is available as a download from [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).</span></span>

<span data-ttu-id="37598-114">toocompile det här projektet du behöver följande konfiguration för din utvecklingsmiljö hello:</span><span class="sxs-lookup"><span data-stu-id="37598-114">toocompile this project, you need hello following configuration for your development environment:</span></span>

* <span data-ttu-id="37598-115">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) eller högre.</span><span class="sxs-lookup"><span data-stu-id="37598-115">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span> <span data-ttu-id="37598-116">HDInsight 3.5 eller högre krävs Java 8.</span><span class="sxs-lookup"><span data-stu-id="37598-116">HDInsight 3.5 or higher require Java 8.</span></span>

* [<span data-ttu-id="37598-117">Maven 3.x</span><span class="sxs-lookup"><span data-stu-id="37598-117">Maven 3.x</span></span>](https://maven.apache.org/download.cgi)

<span data-ttu-id="37598-118">hello kan följande miljövariabler anges när du installerar Java och hello JDK på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="37598-118">hello following environment variables may be set when you install Java and hello JDK on your development workstation.</span></span> <span data-ttu-id="37598-119">Dock bör du kontrollera att de finns och att de innehåller hello rätt värden för ditt system.</span><span class="sxs-lookup"><span data-stu-id="37598-119">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="37598-120">`JAVA_HOME`-måste peka toohello katalog där hello JDK har installerats.</span><span class="sxs-lookup"><span data-stu-id="37598-120">`JAVA_HOME` - should point toohello directory where hello JDK is installed.</span></span>
* <span data-ttu-id="37598-121">`PATH`-bör innehålla hello följande sökvägar:</span><span class="sxs-lookup"><span data-stu-id="37598-121">`PATH` - should contain hello following paths:</span></span>
  
    * <span data-ttu-id="37598-122">`JAVA_HOME`(eller motsvarande hello-sökväg).</span><span class="sxs-lookup"><span data-stu-id="37598-122">`JAVA_HOME` (or hello equivalent path).</span></span>
    * <span data-ttu-id="37598-123">`JAVA_HOME\bin`(eller motsvarande hello-sökväg).</span><span class="sxs-lookup"><span data-stu-id="37598-123">`JAVA_HOME\bin` (or hello equivalent path).</span></span>
    * <span data-ttu-id="37598-124">hello katalog där Maven är installerat.</span><span class="sxs-lookup"><span data-stu-id="37598-124">hello directory where Maven is installed.</span></span>

## <a name="how-toouse-hello-hdfsbolt-with-hdinsight"></a><span data-ttu-id="37598-125">Hur toouse hello HdfsBolt med HDInsight</span><span class="sxs-lookup"><span data-stu-id="37598-125">How toouse hello HdfsBolt with HDInsight</span></span>

> [!IMPORTANT]
> <span data-ttu-id="37598-126">Innan du använder hello HdfsBolt med Storm på HDInsight, måste du först använda ett skript åtgärd toocopy krävs jar-filer i hello `extpath` för Storm.</span><span class="sxs-lookup"><span data-stu-id="37598-126">Before using hello HdfsBolt with Storm on HDInsight, you must first use a script action toocopy required jar files into hello `extpath` for Storm.</span></span> <span data-ttu-id="37598-127">Mer information finns i hello [konfigurera hello klustret](#configure) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="37598-127">For more information, see hello [Configure hello cluster](#configure) section.</span></span>

<span data-ttu-id="37598-128">hello HdfsBolt använder hello filen schema för att ange toounderstand hur toowrite tooHDFS.</span><span class="sxs-lookup"><span data-stu-id="37598-128">hello HdfsBolt uses hello file scheme that you provide toounderstand how toowrite tooHDFS.</span></span> <span data-ttu-id="37598-129">Använd någon av följande scheman hello med HDInsight:</span><span class="sxs-lookup"><span data-stu-id="37598-129">With HDInsight, use one of hello following schemes:</span></span>

* <span data-ttu-id="37598-130">`wasb://`: Används med ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="37598-130">`wasb://`: Used with an Azure Storage account.</span></span>
* <span data-ttu-id="37598-131">`adl://`: Används med Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="37598-131">`adl://`: Used with Azure Data Lake Store.</span></span>

<span data-ttu-id="37598-132">hello innehåller följande tabell exempel på användning av hello filen schemat för olika scenarier:</span><span class="sxs-lookup"><span data-stu-id="37598-132">hello following table provides examples of using hello file scheme for different scenarios:</span></span>

| <span data-ttu-id="37598-133">schemat</span><span class="sxs-lookup"><span data-stu-id="37598-133">Scheme</span></span> | <span data-ttu-id="37598-134">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="37598-134">Notes</span></span> |
| ----- | ----- |
| `wasb:///` | <span data-ttu-id="37598-135">hello standardkontot för lagring är en blobbbehållare i ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="37598-135">hello default storage account is a blob container in an Azure Storage account</span></span> |
| `adl:///` | <span data-ttu-id="37598-136">hello standardkontot för lagring är en katalog i Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="37598-136">hello default storage account is a directory in Azure Data Lake Store.</span></span> <span data-ttu-id="37598-137">När klustret skapas ange hello directory i Data Lake Store är hello roten hello klustret HDFS.</span><span class="sxs-lookup"><span data-stu-id="37598-137">During cluster creation, you specify hello directory in Data Lake Store that is hello root of hello cluster's HDFS.</span></span> <span data-ttu-id="37598-138">Till exempel hello `/clusters/myclustername/` directory.</span><span class="sxs-lookup"><span data-stu-id="37598-138">For example, hello `/clusters/myclustername/` directory.</span></span> |
| `wasb://CONTAINER@ACCOUNT.blob.core.windows.net/` | <span data-ttu-id="37598-139">En icke-standard (ytterligare) Azure storage-konto som är associerade med hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="37598-139">A non-default (additional) Azure storage account associated with hello cluster.</span></span> |
| `adl://STORENAME/` | <span data-ttu-id="37598-140">hello roten för hello Data Lake Store används av hello klustret.</span><span class="sxs-lookup"><span data-stu-id="37598-140">hello root of hello Data Lake Store used by hello cluster.</span></span> <span data-ttu-id="37598-141">Det här schemat kan du tooaccess data som finns utanför hello-katalog som innehåller hello kluster-filsystemet.</span><span class="sxs-lookup"><span data-stu-id="37598-141">This scheme allows you tooaccess data that is located outside hello directory that contains hello cluster file system.</span></span> |

<span data-ttu-id="37598-142">Mer information finns i hello [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) referens på Apache.org.</span><span class="sxs-lookup"><span data-stu-id="37598-142">For more information, see hello [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) reference at Apache.org.</span></span>

### <a name="example-configuration"></a><span data-ttu-id="37598-143">Exempel på konfiguration</span><span class="sxs-lookup"><span data-stu-id="37598-143">Example configuration</span></span>

<span data-ttu-id="37598-144">hello följande YAML är hämtad från hello `resources/writetohdfs.yaml` -fil som ingår i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="37598-144">hello following YAML is an excerpt from hello `resources/writetohdfs.yaml` file included in hello example.</span></span> <span data-ttu-id="37598-145">Den här filen definierar hello Storm-topologi med hello [som](https://storm.apache.org/releases/1.1.0/flux.html) ramverk för Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="37598-145">This file defines hello Storm topology using hello [Flux](https://storm.apache.org/releases/1.1.0/flux.html) framework for Apache Storm.</span></span>

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

<span data-ttu-id="37598-146">Den här YAML definierar hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="37598-146">This YAML defines hello following items:</span></span>

* <span data-ttu-id="37598-147">`syncPolicy`: Definierar när filer är synkroniserade/rensade toohello filsystem.</span><span class="sxs-lookup"><span data-stu-id="37598-147">`syncPolicy`: Defines when files are synched/flushed toohello file system.</span></span> <span data-ttu-id="37598-148">I det här exemplet var 1000 tupplar.</span><span class="sxs-lookup"><span data-stu-id="37598-148">In this example, every 1000 tuples.</span></span>
* <span data-ttu-id="37598-149">`fileNameFormat`: Definierar hello sökvägen och namnet mönster toouse när du skriver filer.</span><span class="sxs-lookup"><span data-stu-id="37598-149">`fileNameFormat`: Defines hello path and file name pattern toouse when writing files.</span></span> <span data-ttu-id="37598-150">I det här exemplet hello sökväg har angetts under körning med ett filter och hello filnamnstillägget är `.txt`.</span><span class="sxs-lookup"><span data-stu-id="37598-150">In this example, hello path is provided at runtime using a filter, and hello file extension is `.txt`.</span></span>
* <span data-ttu-id="37598-151">`recordFormat`: Definierar hello hello-filer som skrivits internt format.</span><span class="sxs-lookup"><span data-stu-id="37598-151">`recordFormat`: Defines hello internal format of hello files written.</span></span> <span data-ttu-id="37598-152">I det här exemplet fälten avgränsas med hello `|` tecken.</span><span class="sxs-lookup"><span data-stu-id="37598-152">In this example, fields are delimited by hello `|` character.</span></span>
* <span data-ttu-id="37598-153">`rotationPolicy`: Om du definierar när toorotate filer.</span><span class="sxs-lookup"><span data-stu-id="37598-153">`rotationPolicy`: Defines when toorotate files.</span></span> <span data-ttu-id="37598-154">I det här exemplet utförs ingen rotation.</span><span class="sxs-lookup"><span data-stu-id="37598-154">In this example, no rotation is performed.</span></span>
* <span data-ttu-id="37598-155">`hdfs-bolt`: Använder hello tidigare komponenter som konfigurationsparametrar för hello `HdfsBolt` klass.</span><span class="sxs-lookup"><span data-stu-id="37598-155">`hdfs-bolt`: Uses hello previous components as configuration parameters for hello `HdfsBolt` class.</span></span>

<span data-ttu-id="37598-156">Mer information om hello som framework finns [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="37598-156">For more information on hello Flux framework, see [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span></span>

## <a name="configure-hello-cluster"></a><span data-ttu-id="37598-157">Konfigurera hello-kluster</span><span class="sxs-lookup"><span data-stu-id="37598-157">Configure hello cluster</span></span>

<span data-ttu-id="37598-158">Som standard omfattar inte Storm på HDInsight hello komponenter att HdfsBolt använder toocommunicate med Azure Storage eller Azure Data Lake Store i Storms klassökvägen.</span><span class="sxs-lookup"><span data-stu-id="37598-158">By default, Storm on HDInsight does not include hello components that HdfsBolt uses toocommunicate with Azure Storage or Data Lake Store in Storm's classpath.</span></span> <span data-ttu-id="37598-159">Använd hello följande skript åtgärd tooadd dessa komponenter toohello `extlib` katalogen för Storm på klustret:</span><span class="sxs-lookup"><span data-stu-id="37598-159">Use hello following script action tooadd these components toohello `extlib` directory for Storm on your cluster:</span></span>

<span data-ttu-id="37598-160">| Script URI | Noder tooapply att | Parametrar. `https://000aarperiscus.blob.core.windows.net/certs/stormextlib.sh` | Nimbus, handledarens | Ingen |</span><span class="sxs-lookup"><span data-stu-id="37598-160">| Script URI | Nodes tooapply it to| Parameters | | `https://000aarperiscus.blob.core.windows.net/certs/stormextlib.sh` | Nimbus, Supervisor | None |</span></span>

<span data-ttu-id="37598-161">Information om hur du använder det här skriptet med ditt kluster finns i hello [anpassa HDInsight-kluster med skriptåtgärder](./hdinsight-hadoop-customize-cluster-linux.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="37598-161">For information on using this script with your cluster, see hello [Customize HDInsight clusters using script actions](./hdinsight-hadoop-customize-cluster-linux.md) document.</span></span>

## <a name="build-and-package-hello-topology"></a><span data-ttu-id="37598-162">Skapa och paketera hello-topologi</span><span class="sxs-lookup"><span data-stu-id="37598-162">Build and package hello topology</span></span>

1. <span data-ttu-id="37598-163">Hämta hello exempelprojekt från [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) tooyour utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="37598-163">Download hello example project from [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) tooyour development environment.</span></span>

2. <span data-ttu-id="37598-164">Terminal eller shell-sessionen, ändra kataloger toohello rot hello hämtas projektet från en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="37598-164">From a command prompt, terminal, or shell session, change directories toohello root of hello downloaded project.</span></span> <span data-ttu-id="37598-165">toobuild och paketet hello topologi använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="37598-165">toobuild and package hello topology, use hello following command:</span></span>
   
        mvn compile package
   
    <span data-ttu-id="37598-166">När hello bygg- och paketering är klar finns en ny katalog med namnet `target`, som innehåller en fil med namnet `StormToHdfs-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="37598-166">Once hello build and packaging completes, there is a new directory named `target`, that contains a file named `StormToHdfs-1.0-SNAPSHOT.jar`.</span></span> <span data-ttu-id="37598-167">Den här filen innehåller hello kompileras topologi.</span><span class="sxs-lookup"><span data-stu-id="37598-167">This file contains hello compiled topology.</span></span>

## <a name="deploy-and-run-hello-topology"></a><span data-ttu-id="37598-168">Distribuera och köra hello-topologi</span><span class="sxs-lookup"><span data-stu-id="37598-168">Deploy and run hello topology</span></span>

1. <span data-ttu-id="37598-169">Använd hello efter kommandot toocopy hello topologi toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="37598-169">Use hello following command toocopy hello topology toohello HDInsight cluster.</span></span> <span data-ttu-id="37598-170">Ersätt **användaren** med hello SSH-användarnamn används när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="37598-170">Replace **USER** with hello SSH user name you used when creating hello cluster.</span></span> <span data-ttu-id="37598-171">Ersätt **KLUSTERNAMN** med hello hello klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="37598-171">Replace **CLUSTERNAME** with hello name of hello cluster.</span></span>
   
        scp target\StormToHdfs-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs1.0-SNAPSHOT.jar
   
    <span data-ttu-id="37598-172">När du uppmanas ange hello lösenord som används när du skapar hello SSH-användare för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="37598-172">When prompted, enter hello password used when creating hello SSH user for hello cluster.</span></span> <span data-ttu-id="37598-173">Om du använde en offentlig nyckel i stället för ett lösenord kan du behöva toouse hello `-i` parametern toospecify hello sökvägen toohello matchar privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="37598-173">If you used a public key instead of a password, you may need toouse hello `-i` parameter toospecify hello path toohello matching private key.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="37598-174">Mer information om hur du använder `scp` med HDInsight, se [använda SSH med HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="37598-174">For more information on using `scp` with HDInsight, see [Use SSH with HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="37598-175">När hello överföringen har slutförts kan du använda hello följande tooconnect toohello HDInsight-kluster med SSH.</span><span class="sxs-lookup"><span data-stu-id="37598-175">Once hello upload completes, use hello following tooconnect toohello HDInsight cluster using SSH.</span></span> <span data-ttu-id="37598-176">Ersätt **användaren** med hello SSH-användarnamn används när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="37598-176">Replace **USER** with hello SSH user name you used when creating hello cluster.</span></span> <span data-ttu-id="37598-177">Ersätt **KLUSTERNAMN** med hello hello klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="37598-177">Replace **CLUSTERNAME** with hello name of hello cluster.</span></span>
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="37598-178">När du uppmanas ange hello lösenord som används när du skapar hello SSH-användare för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="37598-178">When prompted, enter hello password used when creating hello SSH user for hello cluster.</span></span> <span data-ttu-id="37598-179">Om du använde en offentlig nyckel i stället för ett lösenord kan du behöva toouse hello `-i` parametern toospecify hello sökvägen toohello matchar privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="37598-179">If you used a public key instead of a password, you may need toouse hello `-i` parameter toospecify hello path toohello matching private key.</span></span>
   
   <span data-ttu-id="37598-180">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="37598-180">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="37598-181">När du är ansluten, Använd hello följande kommando toocreate en fil med namnet `dev.properties`:</span><span class="sxs-lookup"><span data-stu-id="37598-181">Once connected, use hello following command toocreate a file named `dev.properties`:</span></span>

        nano dev.properties

4. <span data-ttu-id="37598-182">Använd hello följande text som hello hello `dev.properties` fil:</span><span class="sxs-lookup"><span data-stu-id="37598-182">Use hello following text as hello contents of hello `dev.properties` file:</span></span>

        hdfs.write.dir: /stormdata/
        hdfs.url: wasb:///

    > [!IMPORTANT]
    > <span data-ttu-id="37598-183">Det här exemplet förutsätter att klustret använder ett Azure Storage-konto som hello standardlagring.</span><span class="sxs-lookup"><span data-stu-id="37598-183">This example assumes that your cluster uses an Azure Storage account as hello default storage.</span></span> <span data-ttu-id="37598-184">Om klustret använder Azure Data Lake Store, Använd `hdfs.url: adl:///` i stället.</span><span class="sxs-lookup"><span data-stu-id="37598-184">If your cluster uses Azure Data Lake Store, use `hdfs.url: adl:///` instead.</span></span>
    
    <span data-ttu-id="37598-185">toosave hello-fil, Använd __Ctrl + X__, sedan __Y__, och slutligen __RETUR__.</span><span class="sxs-lookup"><span data-stu-id="37598-185">toosave hello file, use __Ctrl + X__, then __Y__, and finally __Enter__.</span></span> <span data-ttu-id="37598-186">hello-värden i den här filen ange Webbadressen för hello Data Lake store och hello katalognamn som data skrivs till.</span><span class="sxs-lookup"><span data-stu-id="37598-186">hello values in this file set hello Data Lake store URL and hello directory name that data is written to.</span></span>

3. <span data-ttu-id="37598-187">Använd följande kommando toostart hello topologi hello:</span><span class="sxs-lookup"><span data-stu-id="37598-187">Use hello following command toostart hello topology:</span></span>
   
        storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties

    <span data-ttu-id="37598-188">Detta kommando startar hello topologi med hello som framework genom att skicka den toohello Nimbus-noden hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="37598-188">This command starts hello topology using hello Flux framework by submitting it toohello Nimbus node of hello cluster.</span></span> <span data-ttu-id="37598-189">hello topologi definieras av hello `writetohdfs.yaml` -fil som ingår i hello jar.</span><span class="sxs-lookup"><span data-stu-id="37598-189">hello topology is defined by hello `writetohdfs.yaml` file included in hello jar.</span></span> <span data-ttu-id="37598-190">Hej `dev.properties` filen skickas som ett filter och hello värden i hello filen läses av hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="37598-190">hello `dev.properties` file is passed as a filter, and hello values contained in hello file are read by hello topology.</span></span>

## <a name="view-output-data"></a><span data-ttu-id="37598-191">Visa utdata</span><span class="sxs-lookup"><span data-stu-id="37598-191">View output data</span></span>

<span data-ttu-id="37598-192">tooview hello data, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="37598-192">tooview hello data, use hello following command:</span></span>

    hdfs dfs -ls /stormdata/

<span data-ttu-id="37598-193">En lista över hello-filer som skapas av den här topologin visas.</span><span class="sxs-lookup"><span data-stu-id="37598-193">A list of hello files created by this topology is displayed.</span></span>

<span data-ttu-id="37598-194">hello är följande lista ett exempel på hello data retuned av hello tidigare kommandon:</span><span class="sxs-lookup"><span data-stu-id="37598-194">hello following list is an example of hello data retuned by hello previous commands:</span></span>

    Found 30 items
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-0-1488568403092.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-1-1488568404567.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-10-1488568408678.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-11-1488568411636.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-12-1488568411884.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-13-1488568412603.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-14-1488568415055.txt

## <a name="stop-hello-topology"></a><span data-ttu-id="37598-195">Stoppa hello-topologi</span><span class="sxs-lookup"><span data-stu-id="37598-195">Stop hello topology</span></span>

<span data-ttu-id="37598-196">Storm-topologier kör förrän stoppats eller hello kluster har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="37598-196">Storm topologies run until stopped, or hello cluster is deleted.</span></span> <span data-ttu-id="37598-197">toostop hello topologi, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="37598-197">toostop hello topology, use hello following command:</span></span>

    storm kill hdfswriter

## <a name="delete-your-cluster"></a><span data-ttu-id="37598-198">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="37598-198">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="37598-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37598-199">Next steps</span></span>

<span data-ttu-id="37598-200">Nu när du har lärt dig hur toouse Storm toowrite tooAzure lagring och Azure Data Lake Store kan identifiera andra [Storm-exempel för HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="37598-200">Now that you have learned how toouse Storm toowrite tooAzure Storage and Azure Data Lake Store, discover other [Storm examples for HDInsight](hdinsight-storm-example-topology.md).</span></span>

