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
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a><span data-ttu-id="46755-104">Utveckla Apache Storm-topologier med Python i HDInsight</span><span class="sxs-lookup"><span data-stu-id="46755-104">Develop Apache Storm topologies using Python on HDInsight</span></span>

<span data-ttu-id="46755-105">Lär dig hur toocreate en Apache Storm-topologi som använder Python-komponenter.</span><span class="sxs-lookup"><span data-stu-id="46755-105">Learn how toocreate an Apache Storm topology that uses Python components.</span></span> <span data-ttu-id="46755-106">Apache Storm har stöd för flera språk och även så att du toocombine komponenter från flera språk i en topologi.</span><span class="sxs-lookup"><span data-stu-id="46755-106">Apache Storm supports multiple languages, even allowing you toocombine components from several languages in one topology.</span></span> <span data-ttu-id="46755-107">hello som framework (introducerades med Storm 0.10.0) kan du tooeasily skapa lösningar som använder Python-komponenter.</span><span class="sxs-lookup"><span data-stu-id="46755-107">hello Flux framework (introduced with Storm 0.10.0) allows you tooeasily create solutions that use Python components.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="46755-108">hello informationen i det här dokumentet har testats med Storm på HDInsight 3,6.</span><span class="sxs-lookup"><span data-stu-id="46755-108">hello information in this document was tested using Storm on HDInsight 3.6.</span></span> <span data-ttu-id="46755-109">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="46755-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="46755-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="46755-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="46755-111">hello-koden för det här projektet är tillgänglig på [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).</span><span class="sxs-lookup"><span data-stu-id="46755-111">hello code for this project is available at [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="46755-112">Krav</span><span class="sxs-lookup"><span data-stu-id="46755-112">Prerequisites</span></span>

* <span data-ttu-id="46755-113">Python 2.7 eller högre</span><span class="sxs-lookup"><span data-stu-id="46755-113">Python 2.7 or higher</span></span>

* <span data-ttu-id="46755-114">Java JDK 1.8 eller högre</span><span class="sxs-lookup"><span data-stu-id="46755-114">Java JDK 1.8 or higher</span></span>

* <span data-ttu-id="46755-115">Maven 3</span><span class="sxs-lookup"><span data-stu-id="46755-115">Maven 3</span></span>

* <span data-ttu-id="46755-116">(Valfritt) En lokal Storm-utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="46755-116">(Optional) A local Storm development environment.</span></span> <span data-ttu-id="46755-117">En lokal miljö Storm krävs endast om du vill toorun hello topologi lokalt.</span><span class="sxs-lookup"><span data-stu-id="46755-117">A local Storm environment is only needed if you want toorun hello topology locally.</span></span> <span data-ttu-id="46755-118">Mer information finns i [ställa in en utvecklingsmiljö](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html).</span><span class="sxs-lookup"><span data-stu-id="46755-118">For more information, see [Setting up a development environment](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html).</span></span>

## <a name="storm-multi-language-support"></a><span data-ttu-id="46755-119">Storm-stöd för flera språk</span><span class="sxs-lookup"><span data-stu-id="46755-119">Storm multi-language support</span></span>

<span data-ttu-id="46755-120">Apache Storm var utformad toowork med komponenter som skrivits med alla programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="46755-120">Apache Storm was designed toowork with components written using any programming language.</span></span> <span data-ttu-id="46755-121">hello-komponenter måste förstå hur toowork med hello [Thrift-definition för Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift).</span><span class="sxs-lookup"><span data-stu-id="46755-121">hello components must understand how toowork with hello [Thrift definition for Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift).</span></span> <span data-ttu-id="46755-122">För Python, har en modul angetts som en del av hello Apache Storm-projekt som du kan använda tooeasily gränssnittet med Storm.</span><span class="sxs-lookup"><span data-stu-id="46755-122">For Python, a module is provided as part of hello Apache Storm project that allows you tooeasily interface with Storm.</span></span> <span data-ttu-id="46755-123">Du hittar den här modulen på [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).</span><span class="sxs-lookup"><span data-stu-id="46755-123">You can find this module at [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).</span></span>

<span data-ttu-id="46755-124">Storm är en Java-process som körs på hello Java Virtual Machine (JVM).</span><span class="sxs-lookup"><span data-stu-id="46755-124">Storm is a Java process that runs on hello Java Virtual Machine (JVM).</span></span> <span data-ttu-id="46755-125">Komponenter som skrivits i andra språk körs som delprocesser.</span><span class="sxs-lookup"><span data-stu-id="46755-125">Components written in other languages are executed as subprocesses.</span></span> <span data-ttu-id="46755-126">hello Storm kommunicerar med dessa delprocesser som använder JSON-meddelanden som skickas över stdin/stdout.</span><span class="sxs-lookup"><span data-stu-id="46755-126">hello Storm communicates with these subprocesses using JSON messages sent over stdin/stdout.</span></span> <span data-ttu-id="46755-127">Mer information om kommunikation mellan komponenter finns i hello [lang Multi Protocol](https://storm.apache.org/documentation/Multilang-protocol.html) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="46755-127">More details on communication between components can be found in hello [Multi-lang Protocol](https://storm.apache.org/documentation/Multilang-protocol.html) documentation.</span></span>

## <a name="python-with-hello-flux-framework"></a><span data-ttu-id="46755-128">Python hello som Framework</span><span class="sxs-lookup"><span data-stu-id="46755-128">Python with hello Flux framework</span></span>

<span data-ttu-id="46755-129">hello som framework kan toodefine Storm-topologier separat från hello komponenter.</span><span class="sxs-lookup"><span data-stu-id="46755-129">hello Flux framework allows you toodefine Storm topologies separately from hello components.</span></span> <span data-ttu-id="46755-130">hello som framework använder YAML toodefine hello Storm-topologi.</span><span class="sxs-lookup"><span data-stu-id="46755-130">hello Flux framework uses YAML toodefine hello Storm topology.</span></span> <span data-ttu-id="46755-131">hello följande är ett exempel på hur tooreference en Python-komponent i hello YAML dokument:</span><span class="sxs-lookup"><span data-stu-id="46755-131">hello following text is an example of how tooreference a Python component in hello YAML document:</span></span>

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

<span data-ttu-id="46755-132">Hej klassen `FluxShellSpout` är används toostart hello `sentencespout.py` skript som implementerar hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="46755-132">hello class `FluxShellSpout` is used toostart hello `sentencespout.py` script that implements hello spout.</span></span>

<span data-ttu-id="46755-133">Som förväntar hello Python skript toobe i hello `/resources` katalogen i hello jar-filen som innehåller hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="46755-133">Flux expects hello Python scripts toobe in hello `/resources` directory inside hello jar file that contains hello topology.</span></span> <span data-ttu-id="46755-134">Så det här exemplet lagrar hello Python-skript i hello `/multilang/resources` directory.</span><span class="sxs-lookup"><span data-stu-id="46755-134">So this example stores hello Python scripts in hello `/multilang/resources` directory.</span></span> <span data-ttu-id="46755-135">Hej `pom.xml` innehåller den här filen med hjälp av följande XML hello:</span><span class="sxs-lookup"><span data-stu-id="46755-135">hello `pom.xml` includes this file using hello following XML:</span></span>

```xml
<!-- include hello Python components -->
<resource>
    <directory>${basedir}/multilang</directory>
    <filtering>false</filtering>
</resource>
```

<span data-ttu-id="46755-136">Som tidigare nämnts är en `storm.py` fil som implementerar hello Thrift-definition för Storm.</span><span class="sxs-lookup"><span data-stu-id="46755-136">As mentioned earlier, there is a `storm.py` file that implements hello Thrift definition for Storm.</span></span> <span data-ttu-id="46755-137">hello som framework inkluderar `storm.py` automatiskt när hello projektet har skapats, så du inte behöver tooworry om, inklusive den.</span><span class="sxs-lookup"><span data-stu-id="46755-137">hello Flux framework includes `storm.py` automatically when hello project is built, so you don't have tooworry about including it.</span></span>

## <a name="build-hello-project"></a><span data-ttu-id="46755-138">Skapa hello-projekt</span><span class="sxs-lookup"><span data-stu-id="46755-138">Build hello project</span></span>

<span data-ttu-id="46755-139">Använd hello följande kommando från hello-roten av hello projekt:</span><span class="sxs-lookup"><span data-stu-id="46755-139">From hello root of hello project, use hello following command:</span></span>

```bash
mvn clean compile package
```

<span data-ttu-id="46755-140">Det här kommandot skapar en `target/WordCount-1.0-SNAPSHOT.jar` -fil som innehåller hello kompileras topologi.</span><span class="sxs-lookup"><span data-stu-id="46755-140">This command creates a `target/WordCount-1.0-SNAPSHOT.jar` file that contains hello compiled topology.</span></span>

## <a name="run-hello-topology-locally"></a><span data-ttu-id="46755-141">Kör hello topologi lokalt</span><span class="sxs-lookup"><span data-stu-id="46755-141">Run hello topology locally</span></span>

<span data-ttu-id="46755-142">toorun hello topologi lokalt, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="46755-142">toorun hello topology locally, use hello following command:</span></span>

```bash
storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -l -R /topology.yaml
```

> [!NOTE]
> <span data-ttu-id="46755-143">Kommandot kräver en lokal Storm-utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="46755-143">This command requires a local Storm development environment.</span></span> <span data-ttu-id="46755-144">Mer information finns i [ställa in en utvecklingsmiljö](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html)</span><span class="sxs-lookup"><span data-stu-id="46755-144">For more information, see [Setting up a development environment](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html)</span></span>

<span data-ttu-id="46755-145">En gång hello topologi startar den skickar information toohello lokala konsolen liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="46755-145">Once hello topology starts, it emits information toohello local console similar toohello following text:</span></span>


    24302 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting hello cow jumped over hello moon
    24302 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting the
    24302 [Thread-28] INFO  o.a.s.t.ShellBolt - ShellLog pid:2437, name:counter-bolt Emitting years:160
    24302 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=the, count=599}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=seven, count=302}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=dwarfs, count=143}
    24303 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting hello cow jumped over hello moon
    24303 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting cow
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=four, count=160}


<span data-ttu-id="46755-146">toostop hello topologi, Använd __Ctrl + C__.</span><span class="sxs-lookup"><span data-stu-id="46755-146">toostop hello topology, use __Ctrl + C__.</span></span>

## <a name="run-hello-storm-topology-on-hdinsight"></a><span data-ttu-id="46755-147">Kör hello Storm-topologi på HDInsight</span><span class="sxs-lookup"><span data-stu-id="46755-147">Run hello Storm topology on HDInsight</span></span>

1. <span data-ttu-id="46755-148">Använd hello följande kommando toocopy hello `WordCount-1.0-SNAPSHOT.jar` filen tooyour Storm på HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="46755-148">Use hello following command toocopy hello `WordCount-1.0-SNAPSHOT.jar` file tooyour Storm on HDInsight cluster:</span></span>

    ```bash
    scp target\WordCount-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="46755-149">Ersätt `sshuser` med hello SSH-användare för klustret.</span><span class="sxs-lookup"><span data-stu-id="46755-149">Replace `sshuser` with hello SSH user for your cluster.</span></span> <span data-ttu-id="46755-150">Ersätt `mycluster` med hello klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="46755-150">Replace `mycluster` with hello cluster name.</span></span> <span data-ttu-id="46755-151">Du kanske ange tooenter hello lösenordet för hello SSH-användare.</span><span class="sxs-lookup"><span data-stu-id="46755-151">You may be prompted tooenter hello password for hello SSH user.</span></span>

    <span data-ttu-id="46755-152">Mer information om hur du använder SSH och SCP finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="46755-152">For more information on using SSH and SCP, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="46755-153">När hello-filen har överförts, ansluta toohello kluster med SSH:</span><span class="sxs-lookup"><span data-stu-id="46755-153">Once hello file has been uploaded, connect toohello cluster using SSH:</span></span>

    ```bash
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. <span data-ttu-id="46755-154">Använd hello följande kommando från hello SSH-session toostart hello-topologin på hello klustret:</span><span class="sxs-lookup"><span data-stu-id="46755-154">From hello SSH session, use hello following command toostart hello topology on hello cluster:</span></span>

    ```bash
    storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -r -R /topology.yaml
    ```

3. <span data-ttu-id="46755-155">Du kan använda hello Storm-Användargränssnittet tooview hello-topologi på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="46755-155">You can use hello Storm UI tooview hello topology on hello cluster.</span></span> <span data-ttu-id="46755-156">hello Storm-Användargränssnittet finns på https://mycluster.azurehdinsight.net/stormui.</span><span class="sxs-lookup"><span data-stu-id="46755-156">hello Storm UI is located at https://mycluster.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="46755-157">Ersätt `mycluster` med klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="46755-157">Replace `mycluster` with your cluster name.</span></span>

> [!NOTE]
> <span data-ttu-id="46755-158">När den har startat, kör en Storm-topologi förrän stoppats.</span><span class="sxs-lookup"><span data-stu-id="46755-158">Once started, a Storm topology runs until stopped.</span></span> <span data-ttu-id="46755-159">toostop Hej topologi, med någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="46755-159">toostop hello topology, use one of hello following methods:</span></span>
>
> * <span data-ttu-id="46755-160">Hej `storm kill TOPOLOGYNAME` från hello kommandoraden</span><span class="sxs-lookup"><span data-stu-id="46755-160">hello `storm kill TOPOLOGYNAME` command from hello command line</span></span>
> * <span data-ttu-id="46755-161">Hej **Kill** knapp i hello Storm-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="46755-161">hello **Kill** button in hello Storm UI.</span></span>


## <a name="next-steps"></a><span data-ttu-id="46755-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="46755-162">Next steps</span></span>

<span data-ttu-id="46755-163">Se hello efter dokument för andra sätt toouse Python med HDInsight:</span><span class="sxs-lookup"><span data-stu-id="46755-163">See hello following documents for other ways toouse Python with HDInsight:</span></span>

* [<span data-ttu-id="46755-164">Hur toouse Python för strömning MapReduce-jobb</span><span class="sxs-lookup"><span data-stu-id="46755-164">How toouse Python for streaming MapReduce jobs</span></span>](hdinsight-hadoop-streaming-python.md)
* [<span data-ttu-id="46755-165">Hur toouse Python användaren definierat funktioner (UDF) i Pig och Hive</span><span class="sxs-lookup"><span data-stu-id="46755-165">How toouse Python User Defined Functions (UDF) in Pig and Hive</span></span>](hdinsight-python.md)
