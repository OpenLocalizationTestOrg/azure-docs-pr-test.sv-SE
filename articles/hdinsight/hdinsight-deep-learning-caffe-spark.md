---
title: "aaaUse Caffe på Azure HDInsight Spark för distribuerade djup learning | Microsoft Docs"
description: "Använda Caffe för distribuerade djup learning på Azure HDInsight Spark"
services: hdinsight
documentationcenter: 
author: xiaoyongzhu
manager: asadk
editor: cgronlun
tags: azure-portal
ms.assetid: 71dcd1ad-4cad-47ad-8a9d-dcb7fa3c2ff9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: xiaoyzhu
ms.openlocfilehash: d6476a7ed3a0df38538e845d7d5404067b01113c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a><span data-ttu-id="a61d0-103">Använda Caffe för distribuerade djup learning på Azure HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="a61d0-103">Use Caffe on Azure HDInsight Spark for distributed deep learning</span></span>


## <a name="introduction"></a><span data-ttu-id="a61d0-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="a61d0-104">Introduction</span></span>

<span data-ttu-id="a61d0-105">Djupgående learning är påverkar allt från sjukvården tootransportation toomanufacturing och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="a61d0-105">Deep learning is impacting everything from healthcare tootransportation toomanufacturing, and more.</span></span> <span data-ttu-id="a61d0-106">Företag aktiverar toodeep learning toosolve hårda problem, t.ex. [bild klassificering](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [taligenkänning](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html)objekt recognition och datorn översättning.</span><span class="sxs-lookup"><span data-stu-id="a61d0-106">Companies are turning toodeep learning toosolve hard problems, like [image classification](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [speech recognition](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html), object recognition, and machine translation.</span></span> 

<span data-ttu-id="a61d0-107">Det finns [många populära ramverk](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), inklusive [Microsoft kognitiva Toolkit](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, osv. Caffe är en av hello mest berömda icke symboliska (viktigt) neurala nätverket ramverk, och som används ofta i många områden, inklusive datorn vision.</span><span class="sxs-lookup"><span data-stu-id="a61d0-107">There are [many popular frameworks](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), including [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, etc. Caffe is one of hello most famous non-symbolic (imperative) neural network frameworks, and widely used in many areas including computer vision.</span></span> <span data-ttu-id="a61d0-108">Dessutom [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) kombinerar Caffe med Apache Spark, i vilket fall djupt learning enkelt kan användas på en befintlig Hadoop-kluster tillsammans med Spark ETL pipelines, vilket minskar komplexiteten system och svarstid för slutpunkt till slutpunkt Learning.</span><span class="sxs-lookup"><span data-stu-id="a61d0-108">Furthermore, [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) combines Caffe with Apache Spark, in which case deep learning can be easily used on an existing Hadoop cluster together with Spark ETL pipelines, reducing system complexity and latency for end-to-end learning.</span></span>

<span data-ttu-id="a61d0-109">[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) är hello, bara fullständigt hanterade molnet Hadoop erbjudande som tillhandahåller optimerade öppen källkod analytiska kluster för Spark, Hive, MapReduce, HBase, Storm, Kafka och R Server backas upp av en SLA för 99,9%.</span><span class="sxs-lookup"><span data-stu-id="a61d0-109">[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) is hello only fully-managed cloud Hadoop offering that provides optimized open source analytic clusters for Spark, Hive, MapReduce, HBase, Storm, Kafka, and R Server backed by a 99.9% SLA.</span></span> <span data-ttu-id="a61d0-110">Var och en av dessa tekniker för stordata och ISV-program kan enkelt distribueras som hanterade kluster med säkerhet och övervakning på företagsnivå.</span><span class="sxs-lookup"><span data-stu-id="a61d0-110">Each of these big data technologies and ISV applications are easily deployable as managed clusters with enterprise-level security and monitoring.</span></span>

<span data-ttu-id="a61d0-111">Vissa användare ber vi om hur toouse djup learning på HDInsight, vilket är Microsofts PaaS Hadoop-produkten.</span><span class="sxs-lookup"><span data-stu-id="a61d0-111">Some users are asking us about how toouse deep learning on HDInsight, which is Microsoft's PaaS Hadoop product.</span></span> <span data-ttu-id="a61d0-112">Vi kommer att ha flera tooshare i hello framtida, men idag vi vill toosummarize en teknisk blogg om hur toouse Caffe på HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="a61d0-112">We will have more tooshare in hello future, but today we want toosummarize a technical blog on how toouse Caffe on HDInsight Spark.</span></span>

<span data-ttu-id="a61d0-113">Om du har installerat Caffe innan du ser du att installera det här ramverket är riktigt svårt.</span><span class="sxs-lookup"><span data-stu-id="a61d0-113">If you have installed Caffe before, you will notice that installing this framework is a little bit challenging.</span></span> <span data-ttu-id="a61d0-114">I den här bloggen vi först illustrerar hur tooinstall [Caffe på Spark](https://github.com/yahoo/CaffeOnSpark) för ett HDInsight-kluster, sedan använda hello inbyggda MNIST demo toodemostrate hur toouse distribuerade djup Learning med HDInsight Spark på processorer.</span><span class="sxs-lookup"><span data-stu-id="a61d0-114">In this blog, we will first illustrate how tooinstall [Caffe on Spark](https://github.com/yahoo/CaffeOnSpark) for an HDInsight cluster, then use hello built-in MNIST demo toodemostrate how toouse Distributed Deep Learning using HDInsight Spark on CPUs.</span></span>

<span data-ttu-id="a61d0-115">Det finns fyra viktiga steg tooget den fungerar på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a61d0-115">There are four major steps tooget it work on HDInsight.</span></span>

1. <span data-ttu-id="a61d0-116">Installera beroenden hello krävs på alla hello noder</span><span class="sxs-lookup"><span data-stu-id="a61d0-116">Install hello required dependencies on all hello nodes</span></span>
2. <span data-ttu-id="a61d0-117">Skapa Caffe i Spark för HDInsight på hello huvudnod</span><span class="sxs-lookup"><span data-stu-id="a61d0-117">Build Caffe on Spark for HDInsight on hello head node</span></span>
3. <span data-ttu-id="a61d0-118">Distribuera hello krävs bibliotek tooall hello arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="a61d0-118">Distribute hello required libraries tooall hello worker nodes</span></span>
4. <span data-ttu-id="a61d0-119">Skapa en Caffe modell och kör den distributely</span><span class="sxs-lookup"><span data-stu-id="a61d0-119">Compose a Caffe model and run it distributely</span></span>

<span data-ttu-id="a61d0-120">Eftersom HDInsight är en PaaS-lösning, erbjuder den bra funktioner - så att det är ganska enkelt tooperform vissa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="a61d0-120">Since HDInsight is a PaaS solution, it offers great platform features - so it is quite easy tooperform some tasks.</span></span> <span data-ttu-id="a61d0-121">En av hello-funktioner som vi använder kraftigt i det här blogginlägget kallas [skriptåtgärd](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), med vilket du kan köra kommandon shell toocustomize klusternoder (huvudnod, arbetsnoden eller kantnod).</span><span class="sxs-lookup"><span data-stu-id="a61d0-121">One of hello features that we heavily use in this blog post is called [Script Action](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), with which you can execute shell commands toocustomize cluster nodes (head node, worker node, or edge node).</span></span>

## <a name="step-1--install-hello-required-dependencies-on-all-hello-nodes"></a><span data-ttu-id="a61d0-122">Steg 1: Installera beroenden hello krävs på alla noder i hello</span><span class="sxs-lookup"><span data-stu-id="a61d0-122">Step 1:  Install hello required dependencies on all hello nodes</span></span>

<span data-ttu-id="a61d0-123">tooget igång, behöver vi tooinstall hello beroenden måste.</span><span class="sxs-lookup"><span data-stu-id="a61d0-123">tooget started, we need tooinstall hello dependencies we need.</span></span> <span data-ttu-id="a61d0-124">Hej Caffe plats och [CaffeOnSpark plats](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) erbjuder vissa användbar wiki för att installera hello beroenden för Spark på YARN-läget (som är hello-läge för HDInsight Spark), men vi behöver tooadd några fler beroenden för HDInsight-plattformen.</span><span class="sxs-lookup"><span data-stu-id="a61d0-124">hello Caffe site and [CaffeOnSpark site](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) offers some very useful wiki for installing hello dependencies for Spark on YARN mode (which is hello mode for HDInsight Spark), but we need tooadd a few more dependencies for HDInsight platform.</span></span> <span data-ttu-id="a61d0-125">Vi använder hello skriptåtgärd enligt nedan och kör det på alla hello huvudnoderna och arbetsnoder.</span><span class="sxs-lookup"><span data-stu-id="a61d0-125">We will use hello script action as below and run it on all hello head nodes and worker nodes.</span></span> <span data-ttu-id="a61d0-126">Den här skriptåtgärden tar ungefär 20 minuter som dessa beroenden beror också på andra paket.</span><span class="sxs-lookup"><span data-stu-id="a61d0-126">This script action will take about 20 minutes, as those dependencies also depend on other packages.</span></span> <span data-ttu-id="a61d0-127">Du bör placera den på en plats som är tillgänglig tooyour HDInsight-klustret, till exempel en GitHub-plats eller hello standard BLOB storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a61d0-127">You should put it in some location that is accessible tooyour HDInsight cluster, such as a GitHub location or hello default BLOB storage account.</span></span>

    #!/bin/bash
    #Please be aware that installing hello below will add additional 20 mins toocluster creation because of hello dependencies
    #installing all dependencies, including hello ones mentioned in http://caffe.berkeleyvision.org/install_apt.html, as well a few packages that are not included in HDInsight, such as gflags, glog, lmdb, numpy
    #It seems numpy will only needed during compilation time, but for safety purpose we install them on all hello nodes

    sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler maven libatlas-base-dev libgflags-dev libgoogle-glog-dev liblmdb-dev build-essential  libboost-all-dev python-numpy python-scipy python-matplotlib ipython ipython-notebook python-pandas python-sympy python-nose

    #install protobuf
    wget https://github.com/google/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz
    sudo tar xzvf protobuf-2.5.0.tar.gz -C /tmp/
    cd /tmp/protobuf-2.5.0/
    sudo ./configure
    sudo make
    sudo make check
    sudo make install
    sudo ldconfig
    echo "protobuf installation done"


<span data-ttu-id="a61d0-128">Det finns två steg i hello skriptåtgärd ovan.</span><span class="sxs-lookup"><span data-stu-id="a61d0-128">There are two steps in hello script action above.</span></span> <span data-ttu-id="a61d0-129">hello första steget är tooinstall alla hello bibliotek som krävs.</span><span class="sxs-lookup"><span data-stu-id="a61d0-129">hello first step is tooinstall all hello required libraries.</span></span> <span data-ttu-id="a61d0-130">Dessa bibliotek innehåller nödvändiga hello-bibliotek för både kompilera Caffe (till exempel gflags glog) och köra Caffe (till exempel numpy).</span><span class="sxs-lookup"><span data-stu-id="a61d0-130">Those libraries include hello necessary libraries for both compiling Caffe(such as gflags, glog) and running Caffe (such as numpy).</span></span> <span data-ttu-id="a61d0-131">Vi använder libatlas för CPU-optimering, men du kan alltid följa hello CaffeOnSpark wiki om hur du installerar andra optimering bibliotek, till exempel MKL eller CUDA (för GPU).</span><span class="sxs-lookup"><span data-stu-id="a61d0-131">We are using libatlas for CPU optimization, but you can always follow hello CaffeOnSpark wiki on installing other optimization libraries, such as MKL or CUDA (for GPU).</span></span>

<span data-ttu-id="a61d0-132">hello andra steget är toodownload, kompilera och installera protobuf 2.5.0 för Caffe under körningen.</span><span class="sxs-lookup"><span data-stu-id="a61d0-132">hello second step is toodownload, compile, and install protobuf 2.5.0 for Caffe during runtime.</span></span> <span data-ttu-id="a61d0-133">Protobuf 2.5.0 [krävs](https://github.com/yahoo/CaffeOnSpark/issues/87), men den här versionen inte är tillgängligt som ett paket på Ubuntu 16, så vi behöver toocompile från hello källkoden.</span><span class="sxs-lookup"><span data-stu-id="a61d0-133">Protobuf 2.5.0 [is required](https://github.com/yahoo/CaffeOnSpark/issues/87), however this version is not available as a package on Ubuntu 16, so we need toocompile it from hello source code.</span></span> <span data-ttu-id="a61d0-134">Det finns också några resurser på hello Internet på hur toocompile, som [detta](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)</span><span class="sxs-lookup"><span data-stu-id="a61d0-134">There are also a few resources on hello Internet on how toocompile it, such as [this](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)</span></span>

<span data-ttu-id="a61d0-135">toosimply komma igång, du kan bara köra den här skriptåtgärden mot ditt kluster tooall hello worker noder och head noder (för HDInsight 3.5).</span><span class="sxs-lookup"><span data-stu-id="a61d0-135">toosimply get started, you can just run this script action against your cluster tooall hello worker nodes and head nodes (for HDInsight 3.5).</span></span> <span data-ttu-id="a61d0-136">Du kan antingen köra hello skriptåtgärder för ett kluster som körs eller du kan också köra hello skriptåtgärder under hello klustret etablera tiden.</span><span class="sxs-lookup"><span data-stu-id="a61d0-136">You can either run hello script actions for a running cluster, or you can also run hello script actions during hello cluster provision time.</span></span> <span data-ttu-id="a61d0-137">Mer information om hello skriptåtgärder finns hello dokumentationen [här](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)</span><span class="sxs-lookup"><span data-stu-id="a61d0-137">For more details on hello script actions, please see hello documentation [here](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)</span></span>

![Skriptet åtgärder tooInstall beroenden](./media/hdinsight-deep-learning-caffe-spark/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-hello-head-node"></a><span data-ttu-id="a61d0-139">Steg 2: Skapa Caffe på Spark för HDInsight på hello huvudnod</span><span class="sxs-lookup"><span data-stu-id="a61d0-139">Step 2: Build Caffe on Spark for HDInsight on hello head node</span></span>

<span data-ttu-id="a61d0-140">hello andra steget är toobuild Caffe på hello headnode och sedan distribuera hello kompileras bibliotek tooall hello arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="a61d0-140">hello second step is toobuild Caffe on hello headnode, and then distribute hello compiled libraries tooall hello worker nodes.</span></span> <span data-ttu-id="a61d0-141">I det här steget behöver du för[ssh till din headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), Följ bara hello [CaffeOnSpark Byggprocessen](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), och nedan är hello-skript som du kan använda toobuild CaffeOnSpark med några ytterligare steg.</span><span class="sxs-lookup"><span data-stu-id="a61d0-141">In this step, you will need too[ssh into your headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), then simply follow hello [CaffeOnSpark build process](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), and below is hello script you can use toobuild CaffeOnSpark with a few additional steps.</span></span> 

    #!/bin/bash
    git clone https://github.com/yahoo/CaffeOnSpark.git --recursive
    export CAFFE_ON_SPARK=$(pwd)/CaffeOnSpark

    pushd ${CAFFE_ON_SPARK}/caffe-public/
    cp Makefile.config.example Makefile.config
    echo "INCLUDE_DIRS += ${JAVA_HOME}/include" >> Makefile.config
    #Below configurations might need toobe updated based on actual cases. For example, if you are using GPU, or using a different BLAS library, you may want tooupdate those settings accordingly.
    echo "CPU_ONLY := 1" >> Makefile.config
    echo "BLAS := atlas" >> Makefile.config
    echo "INCLUDE_DIRS += /usr/include/hdf5/serial/" >> Makefile.config
    echo "LIBRARY_DIRS += /usr/lib/x86_64-linux-gnu/hdf5/serial/" >> Makefile.config
    popd

    #compile CaffeOnSpark
    pushd ${CAFFE_ON_SPARK}
    #always clean up hello environment before building (especially when rebuiding), or there will be errors such as "failed tooexecute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2"
    make clean 
    #hello build step usually takes 20~30 mins, since it has a lot maven dependencies
    make build 
    popd
    export LD_LIBRARY_PATH=${CAFFE_ON_SPARK}/caffe-public/distribute/lib:${CAFFE_ON_SPARK}/caffe-distri/distribute/lib

    hadoop fs -mkdir -p wasb:///projects/machine_learning/image_dataset

    ${CAFFE_ON_SPARK}/scripts/setup-mnist.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/mnist_*_lmdb wasb:///projects/machine_learning/image_dataset/

    ${CAFFE_ON_SPARK}/scripts/setup-cifar10.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/cifar10_*_lmdb wasb:///projects/machine_learning/image_dataset/

    #put hello already compiled CaffeOnSpark libraries toowasb storage, then read back tooeach node using script actions. This is because CaffeOnSpark requires all hello nodes have hello libarries
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-public/distribute/lib/
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-distri/distribute/lib/* /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-public/distribute/lib/* /CaffeOnSpark/caffe-public/distribute/lib/

<span data-ttu-id="a61d0-142">Du kan behöva toodo mer än vad hello-dokumentationen för CaffeOnSpark står.</span><span class="sxs-lookup"><span data-stu-id="a61d0-142">You may need toodo more than what hello documentation of CaffeOnSpark says.</span></span> <span data-ttu-id="a61d0-143">hello ändringar är:</span><span class="sxs-lookup"><span data-stu-id="a61d0-143">hello changes are:</span></span>
- <span data-ttu-id="a61d0-144">Ändra bara tooCPU och använda libatlas för den här särskilt ändamål.</span><span class="sxs-lookup"><span data-stu-id="a61d0-144">Change tooCPU only and use libatlas for this particular purpose.</span></span>
- <span data-ttu-id="a61d0-145">Placera hello datauppsättningar toohello BLOB storage som är en delad plats som är tillgänglig tooall arbetarnoder för senare användning.</span><span class="sxs-lookup"><span data-stu-id="a61d0-145">Put hello datasets toohello BLOB storage, which is a shared location that is accessible tooall worker nodes for later use.</span></span>
- <span data-ttu-id="a61d0-146">Placera hello kompileras Caffe bibliotek tooBLOB lagring och du kommer senare att kopiera dessa bibliotek tooall hello-noder som använder skriptet åtgärder tooavoid ytterligare kompileringstid.</span><span class="sxs-lookup"><span data-stu-id="a61d0-146">Put hello compiled Caffe libraries tooBLOB storage, and later you will copy those libraries tooall hello nodes using script actions tooavoid additional compilation time.</span></span>


### <a name="troubleshooting-an-ant-buildexception-has-occured-exec-returned-2"></a><span data-ttu-id="a61d0-147">Felsökning: Ett Ant BuildException har inträffat: exec som returnerades: 2</span><span class="sxs-lookup"><span data-stu-id="a61d0-147">Troubleshooting: An Ant BuildException has occured: exec returned: 2</span></span>

<span data-ttu-id="a61d0-148">När du först försöker toobuild CaffeOnSpark, anges ibland</span><span class="sxs-lookup"><span data-stu-id="a61d0-148">When first trying toobuild CaffeOnSpark, sometimes it will say</span></span>

    failed tooexecute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

<span data-ttu-id="a61d0-149">Bara ren hello databasen genom att ”kontrollera ren” och sedan kör ”Kontrollera skapa” löser problemet, så länge som du har rätt hello-beroenden.</span><span class="sxs-lookup"><span data-stu-id="a61d0-149">Simply clean hello code repository by "make clean" and then run "make build" will solve this issue, as long as you have hello correct dependencies.</span></span>

### <a name="troubleshooting-maven-repository-connection-time-out"></a><span data-ttu-id="a61d0-150">Felsöka: Maven databasen anslutnings-timeout</span><span class="sxs-lookup"><span data-stu-id="a61d0-150">Troubleshooting: Maven repository connection time out</span></span>

<span data-ttu-id="a61d0-151">Ibland ger maven mig hello timeout anslutningsfel, liknande toobelow:</span><span class="sxs-lookup"><span data-stu-id="a61d0-151">Sometimes maven gives me hello connection time out error, similar toobelow:</span></span>

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request too{s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

<span data-ttu-id="a61d0-152">Den OK Vänta några minuter och sedan försöka toorebuild hello kod, så kan det vara Maven på något sätt gränser hello trafik från en angiven IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a61d0-152">It will be OK after waiting for a few minutes and then just try toorebuild hello code, so it might be Maven somehow limits hello traffic from a given IP address.</span></span>


### <a name="troubleshooting-test-failure-for-caffe"></a><span data-ttu-id="a61d0-153">Felsöka: Testa fel för Caffe</span><span class="sxs-lookup"><span data-stu-id="a61d0-153">Troubleshooting: Test failure for Caffe</span></span>

<span data-ttu-id="a61d0-154">Förmodligen visas ett testfel när du gör hello sista kontrollen för CaffeOnSpark, liknande med nedan.</span><span class="sxs-lookup"><span data-stu-id="a61d0-154">You probably will see a test failure when doing hello final check for CaffeOnSpark, similar with below.</span></span> <span data-ttu-id="a61d0-155">Detta är prabably relaterat med UTF-8-kodning, men ska inte påverkar hello användning av Caffe</span><span class="sxs-lookup"><span data-stu-id="a61d0-155">This is prabably related with UTF-8 encoding, but should not impact hello usage of Caffe</span></span>

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-hello-required-libraries-tooall-hello-worker-nodes"></a><span data-ttu-id="a61d0-156">Steg 3: Distribuera hello krävs bibliotek tooall hello arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="a61d0-156">Step 3: Distribute hello required libraries tooall hello worker nodes</span></span>

<span data-ttu-id="a61d0-157">hello nästa steg är toodistribute hello bibliotek (i praktiken hello-biblioteken i CaffeOnSpark/caffe-offentlig/distribuera/lib/och CaffeOnSpark/caffe-distri/distribuera/lib /) tooall hello noder.</span><span class="sxs-lookup"><span data-stu-id="a61d0-157">hello next step is toodistribute hello libraries (basically hello libraries in CaffeOnSpark/caffe-public/distribute/lib/ and CaffeOnSpark/caffe-distri/distribute/lib/) tooall hello nodes.</span></span> <span data-ttu-id="a61d0-158">Steg 2, vi lägga till dessa bibliotek i BLOB storage och i det här steget ska vi använda skriptet åtgärder toocopy den tooall hello huvudnoderna och arbetsnoder.</span><span class="sxs-lookup"><span data-stu-id="a61d0-158">In Step 2, we put those libraries on BLOB storage, and in this step, we will use script actions toocopy it tooall hello head nodes and worker nodes.</span></span>

<span data-ttu-id="a61d0-159">toodo Detta möjliggör enkel kör en skriptåtgärd som nedan (du måste toopoint toohello rätt plats specifika tooyour kluster):</span><span class="sxs-lookup"><span data-stu-id="a61d0-159">toodo this, simple run a script action as below (you need toopoint toohello right location specific tooyour cluster):</span></span>

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

<span data-ttu-id="a61d0-160">Eftersom i steg 2, vi placera den hello BLOB-lagring som är tillgänglig tooall hello noder, i det här steget kopiera vi bara bara det tooall hello noder.</span><span class="sxs-lookup"><span data-stu-id="a61d0-160">Because in step 2, we put it on hello BLOB storage which is accessible tooall hello nodes, in this step we just simply copy it tooall hello nodes.</span></span>

## <a name="step-4-compose-a-caffe-model-and-run-it-distributely"></a><span data-ttu-id="a61d0-161">Steg 4: Skapa en Caffe modell och kör den distributely</span><span class="sxs-lookup"><span data-stu-id="a61d0-161">Step 4: Compose a Caffe model and run it distributely</span></span>

<span data-ttu-id="a61d0-162">När du har kört hello senare steg, Caffe är installerad på hello headnode alreay och vi är bra toogo.</span><span class="sxs-lookup"><span data-stu-id="a61d0-162">After running hello above steps, Caffe is alreay installed on hello headnode and we are good toogo.</span></span> <span data-ttu-id="a61d0-163">hello nästa steg är toowrite en Caffe modell.</span><span class="sxs-lookup"><span data-stu-id="a61d0-163">hello next step is toowrite a Caffe model.</span></span> 

<span data-ttu-id="a61d0-164">Caffe använder en ”lättfattliga arkitektur”, där för att skapa en modell du bara behöver toodefine en konfigurationsfil och utan kodning alls (i de flesta fall).</span><span class="sxs-lookup"><span data-stu-id="a61d0-164">Caffe is using an "expressive architecture", where for composing a model, you just need toodefine a configuration file, and without coding at all (in most cases).</span></span> <span data-ttu-id="a61d0-165">Vi titta det.</span><span class="sxs-lookup"><span data-stu-id="a61d0-165">So let's take a look there.</span></span> 

<span data-ttu-id="a61d0-166">hello modellen ska vi träna idag är en exempel-modell för MNIST utbildning.</span><span class="sxs-lookup"><span data-stu-id="a61d0-166">hello model we will train today is a sample model for MNIST training.</span></span> <span data-ttu-id="a61d0-167">Hej är MNIST handskriven siffror en uppsättning 60 000 exempel utbildning och en test av 10 000 exempel.</span><span class="sxs-lookup"><span data-stu-id="a61d0-167">hello MNIST database of handwritten digits has a training set of 60,000 examples, and a test set of 10,000 examples.</span></span> <span data-ttu-id="a61d0-168">Det är en del av en större grupp som är tillgängliga från NIST.</span><span class="sxs-lookup"><span data-stu-id="a61d0-168">It is a subset of a larger set available from NIST.</span></span> <span data-ttu-id="a61d0-169">hello siffror har storleken-normaliserat och centrerade i en bild med fast storlek.</span><span class="sxs-lookup"><span data-stu-id="a61d0-169">hello digits have been size-normalized and centered in a fixed-size image.</span></span> <span data-ttu-id="a61d0-170">CaffeOnSpark har vissa skript toodownload hello dataset och omvandla dem till hello rätt format.</span><span class="sxs-lookup"><span data-stu-id="a61d0-170">CaffeOnSpark has some scripts toodownload hello dataset and convert it into hello right format.</span></span>

<span data-ttu-id="a61d0-171">CaffeOnSpark innehåller några exempel på nätverket topologier för MNIST utbildning.</span><span class="sxs-lookup"><span data-stu-id="a61d0-171">CaffeOnSpark provides some network topologies example for MNIST training.</span></span> <span data-ttu-id="a61d0-172">Den har en bra design på Dela hello nätverksarkitekturen (hello topologi för hello nätverk) och optimering.</span><span class="sxs-lookup"><span data-stu-id="a61d0-172">It has a nice design of splitting hello network architecture (hello topology of hello network) and optimization.</span></span> <span data-ttu-id="a61d0-173">I det här fallet finns två filer krävs:</span><span class="sxs-lookup"><span data-stu-id="a61d0-173">In this case, There are two files required:</span></span> 

<span data-ttu-id="a61d0-174">Hej ”Solver” filen (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) används för att övervaka hello optimering och generera parametern uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="a61d0-174">hello "Solver" file (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) is used for overseeing hello optimization and generating parameter updates.</span></span> <span data-ttu-id="a61d0-175">Exempelvis definierar om CPU eller GPU kommer att användas, vad är hello momentum, hur många iterationer kommer att vara, osv. Den definierar vilka neuron nätverkstopologi bör hello programmet används (vilket är hello andra fil som vi behöver).</span><span class="sxs-lookup"><span data-stu-id="a61d0-175">For example, it defines whether CPU or GPU will be used, what's hello momentum, how many iterations will be, etc. It also defines which neuron network topology should hello program use (which is hello second file we need).</span></span> <span data-ttu-id="a61d0-176">Mer information om Solver finns för[Caffe dokumentationen](http://caffe.berkeleyvision.org/tutorial/solver.html).</span><span class="sxs-lookup"><span data-stu-id="a61d0-176">For more information about Solver, please refer too[Caffe documentation](http://caffe.berkeleyvision.org/tutorial/solver.html).</span></span>

<span data-ttu-id="a61d0-177">Eftersom vi använder CPU i stället för GPU, i det här exemplet ska vi ändra hello sista raden till:</span><span class="sxs-lookup"><span data-stu-id="a61d0-177">For this example, since we are using CPU rather than GPU, we should change hello last line to:</span></span>

    # solver mode: CPU or GPU
    solver_mode: CPU

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-1.png)

<span data-ttu-id="a61d0-179">Du kan ändra andra rader efter behov.</span><span class="sxs-lookup"><span data-stu-id="a61d0-179">You can change other lines as needed.</span></span>

<span data-ttu-id="a61d0-180">andra hello-fil (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) definierar hur hello neuron nätverk ser ut, och hello relevanta indata och utdata-fil.</span><span class="sxs-lookup"><span data-stu-id="a61d0-180">hello second file (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) defines how hello neuron network looks like, and hello relevant input and output file.</span></span> <span data-ttu-id="a61d0-181">Vi behöver också tooupdate hello tooreflect hello utbildning data filplats.</span><span class="sxs-lookup"><span data-stu-id="a61d0-181">We also need tooupdate hello file tooreflect hello training data location.</span></span> <span data-ttu-id="a61d0-182">Ändra hello efter en del i lenet_memory_train_test.prototxt (du måste toopoint toohello rätt plats specifika tooyour kluster):</span><span class="sxs-lookup"><span data-stu-id="a61d0-182">Change hello following part in lenet_memory_train_test.prototxt (you need toopoint toohello right location specific tooyour cluster):</span></span>

- <span data-ttu-id="a61d0-183">Ändra hello ”file:/Users/mridul/bigml/demodl/mnist_train_lmdb” för ”wasb: / / / projekt/machine_learning/image_dataset/mnist_train_lmdb”</span><span class="sxs-lookup"><span data-stu-id="a61d0-183">change hello "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" too"wasb:///projects/machine_learning/image_dataset/mnist_train_lmdb"</span></span>
- <span data-ttu-id="a61d0-184">Ändra ”file:/Users/mridul/bigml/demodl/mnist_test_lmdb/” för ”wasb: / / / projekt/machine_learning/image_dataset/mnist_test_lmdb”</span><span class="sxs-lookup"><span data-stu-id="a61d0-184">change "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" too"wasb:///projects/machine_learning/image_dataset/mnist_test_lmdb"</span></span>

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-2.png)

<span data-ttu-id="a61d0-186">Mer information om hur toodefine hello nätverket Kontrollera hello [Caffe dokumentation om MNIST dataset](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)</span><span class="sxs-lookup"><span data-stu-id="a61d0-186">For more information on how toodefine hello network, please check hello [Caffe documentation on MNIST dataset](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)</span></span>

<span data-ttu-id="a61d0-187">För hello syftet med den här bloggen använda vi bara det här enkla exemplet MNIST.</span><span class="sxs-lookup"><span data-stu-id="a61d0-187">For hello purpose of this blog, we just use this simple MNIST example.</span></span> <span data-ttu-id="a61d0-188">Du bör köra hello-kommandot nedan från hello huvudnod:</span><span class="sxs-lookup"><span data-stu-id="a61d0-188">You should run hello command below from hello head node:</span></span>

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

<span data-ttu-id="a61d0-189">I princip distribueras hello krävs filer (lenet_memory_solver.prototxt och lenet_memory_train_test.prototxt) tooeach garn behållare, samt även hello relevanta SÖKVÄGEN för varje Spark drivrutinen/utföraren tooLD_LIBRARY_PATH som definieras i hello föregående kod fragment och punkter toohello plats som har CaffeOnSpark bibliotek.</span><span class="sxs-lookup"><span data-stu-id="a61d0-189">Basically it distributes hello required files (lenet_memory_solver.prototxt and lenet_memory_train_test.prototxt) tooeach YARN container, and also set hello relevant PATH of each Spark driver/executor tooLD_LIBRARY_PATH, which is defined in hello previous code snippet and points toohello location that has CaffeOnSpark libraries.</span></span> 

## <a name="monitoring-and-troubleshooting"></a><span data-ttu-id="a61d0-190">Övervakning och felsökning</span><span class="sxs-lookup"><span data-stu-id="a61d0-190">Monitoring and troubleshooting</span></span>

<span data-ttu-id="a61d0-191">Eftersom vi använder YARN klustret läget i vilket fall hello Spark drivrutinen kommer att vara schemalagda tooan godtycklig behållaren (och en valfri arbetsnoden) bör du kan bara se i hello konsolen mata ut liknande:</span><span class="sxs-lookup"><span data-stu-id="a61d0-191">Since we are using YARN cluster mode, in which case hello Spark driver will be scheduled tooan arbitrary container (and an arbitrary worker node) you should only see in hello console outputting something like:</span></span>

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

<span data-ttu-id="a61d0-192">Om du vill tooknow vad hände, måste vanligtvis tooget hello Spark-drivrutinens logg som innehåller mer information.</span><span class="sxs-lookup"><span data-stu-id="a61d0-192">If you want tooknow what happened, you usually need tooget hello Spark driver's log, which has more information.</span></span> <span data-ttu-id="a61d0-193">I det här fallet måste toogo toohello YARN-Användargränssnittet toofind hello relevanta YARN-loggar.</span><span class="sxs-lookup"><span data-stu-id="a61d0-193">In this case, you need toogo toohello YARN UI toofind hello relevant YARN logs.</span></span> <span data-ttu-id="a61d0-194">Du kan få hello YARN-Användargränssnittet av denna URL:</span><span class="sxs-lookup"><span data-stu-id="a61d0-194">You can get hello YARN UI by this URL:</span></span> 

    https://yourclustername.azurehdinsight.net/yarnui
   
![YARN-ANVÄNDARGRÄNSSNITTET](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-1.png)

<span data-ttu-id="a61d0-196">Du kan ta en titt på hur många resurser för det aktuella programmet.</span><span class="sxs-lookup"><span data-stu-id="a61d0-196">You can take a look at how many resources are allocated for this particular application.</span></span> <span data-ttu-id="a61d0-197">Du kan klicka på hello ”Scheduler” länk och sedan du kommer att för det här programmet, det finns 9 behållare som kör.</span><span class="sxs-lookup"><span data-stu-id="a61d0-197">You can click hello "Scheduler" link, and then you will see that for this application, there are 9 containers running.</span></span> <span data-ttu-id="a61d0-198">Vi ber YARN tooprovide 8 executors och en annan behållare för drivrutinen processen.</span><span class="sxs-lookup"><span data-stu-id="a61d0-198">We ask YARN tooprovide 8 executors, and another container is for driver process.</span></span> 

![YARN Schemaläggaren](./media/hdinsight-deep-learning-caffe-spark/YARN-Scheduler.png)

<span data-ttu-id="a61d0-200">Du kanske vill toocheck hello drivrutinsloggar eller behållaren loggar om det finns fel.</span><span class="sxs-lookup"><span data-stu-id="a61d0-200">You may want toocheck hello  driver logs or container logs if there are failures.</span></span> <span data-ttu-id="a61d0-201">För drivrutinsloggar du klickar du på hello-ID i YARN-Användargränssnittet och sedan klicka hello ”loggar”.</span><span class="sxs-lookup"><span data-stu-id="a61d0-201">For driver logs, you can click hello application ID in YARN UI, then click hello "Logs" button.</span></span> <span data-ttu-id="a61d0-202">hello drivrutinen loggarna skrivs till stderr.</span><span class="sxs-lookup"><span data-stu-id="a61d0-202">hello driver logs are written into stderr.</span></span>

![YARN-ANVÄNDARGRÄNSSNITTET 2](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-2.png)

<span data-ttu-id="a61d0-204">T.ex, kan du se några av hello fel under från hello drivrutinsloggar som anger du allokera för många executors.</span><span class="sxs-lookup"><span data-stu-id="a61d0-204">For example, you might see some of hello error below from hello driver logs, indicating you allocate too many executors.</span></span>

    17/02/01 07:26:06 ERROR ApplicationMaster: User class threw exception: java.lang.IllegalStateException: Insufficient training data. Please adjust hyperparameters or increase dataset.
    java.lang.IllegalStateException: Insufficient training data. Please adjust hyperparameters or increase dataset.
        at com.yahoo.ml.caffe.CaffeOnSpark.trainWithValidation(CaffeOnSpark.scala:261)
        at com.yahoo.ml.caffe.CaffeOnSpark$.main(CaffeOnSpark.scala:42)
        at com.yahoo.ml.caffe.CaffeOnSpark.main(CaffeOnSpark.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.spark.deploy.yarn.ApplicationMaster$$anon$2.run(ApplicationMaster.scala:627)

<span data-ttu-id="a61d0-205">Ibland kan hello problemet inträffa i executors i stället för drivrutiner.</span><span class="sxs-lookup"><span data-stu-id="a61d0-205">Sometimes, hello issue can happen in executors rather than drivers.</span></span> <span data-ttu-id="a61d0-206">I det här fallet måste toocheck hello behållaren loggar.</span><span class="sxs-lookup"><span data-stu-id="a61d0-206">In this case, you need toocheck hello container logs.</span></span> <span data-ttu-id="a61d0-207">Du kan alltid hämta hello behållaren loggar och få hello misslyckade behållare.</span><span class="sxs-lookup"><span data-stu-id="a61d0-207">You can always get hello container logs, and then get hello failed container.</span></span> <span data-ttu-id="a61d0-208">Du kan till exempel uppfylla det här felet när du kör Caffe.</span><span class="sxs-lookup"><span data-stu-id="a61d0-208">For example, you might meet this failure when running Caffe.</span></span>

    17/02/01 07:12:05 WARN YarnAllocator: Container marked as failed: container_1485916338528_0008_05_000005 on host: 10.0.0.14. Exit status: 134. Diagnostics: Exception from container-launch.
    Container id: container_1485916338528_0008_05_000005
    Exit code: 134
    Exception message: /bin/bash: line 1: 12230 Aborted                 (core dumped) LD_LIBRARY_PATH=/usr/hdp/current/hadoop-client/lib/native:/usr/hdp/current/hadoop-client/lib/native/Linux-amd64-64:/home/xiaoyzhu/CaffeOnSpark/caffe-public/distribute/lib:/home/xiaoyzhu/CaffeOnSpark/caffe-distri/distribute/lib /usr/lib/jvm/java-8-openjdk-amd64/bin/java -server -Xmx4608m '-Dhdp.version=' '-Detwlogger.component=sparkexecutor' '-DlogFilter.filename=SparkLogFilters.xml' '-DpatternGroup.filename=SparkPatternGroups.xml' '-Dlog4jspark.root.logger=INFO,console,RFA,ETW,Anonymizer' '-Dlog4jspark.log.dir=/var/log/sparkapp/${user.name}' '-Dlog4jspark.log.file=sparkexecutor.log' '-Dlog4j.configuration=file:/usr/hdp/current/spark2-client/conf/log4j.properties' '-Djavax.xml.parsers.SAXParserFactory=com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl' -Djava.io.tmpdir=/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/tmp '-Dspark.driver.port=43942' '-Dspark.history.ui.port=18080' '-Dspark.ui.port=0' -Dspark.yarn.app.container.log.dir=/mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005 -XX:OnOutOfMemoryError='kill %p' org.apache.spark.executor.CoarseGrainedExecutorBackend --driver-url spark://CoarseGrainedScheduler@10.0.0.13:43942 --executor-id 4 --hostname 10.0.0.14 --cores 3 --app-id application_1485916338528_0008 --user-class-path file:/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/__app__.jar > /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stdout 2> /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stderr

    Stack trace: ExitCodeException exitCode=134: /bin/bash: line 1: 12230 Aborted                 (core dumped) LD_LIBRARY_PATH=/usr/hdp/current/hadoop-client/lib/native:/usr/hdp/current/hadoop-client/lib/native/Linux-amd64-64:/home/xiaoyzhu/CaffeOnSpark/caffe-public/distribute/lib:/home/xiaoyzhu/CaffeOnSpark/caffe-distri/distribute/lib /usr/lib/jvm/java-8-openjdk-amd64/bin/java -server -Xmx4608m '-Dhdp.version=' '-Detwlogger.component=sparkexecutor' '-DlogFilter.filename=SparkLogFilters.xml' '-DpatternGroup.filename=SparkPatternGroups.xml' '-Dlog4jspark.root.logger=INFO,console,RFA,ETW,Anonymizer' '-Dlog4jspark.log.dir=/var/log/sparkapp/${user.name}' '-Dlog4jspark.log.file=sparkexecutor.log' '-Dlog4j.configuration=file:/usr/hdp/current/spark2-client/conf/log4j.properties' '-Djavax.xml.parsers.SAXParserFactory=com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl' -Djava.io.tmpdir=/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/tmp '-Dspark.driver.port=43942' '-Dspark.history.ui.port=18080' '-Dspark.ui.port=0' -Dspark.yarn.app.container.log.dir=/mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005 -XX:OnOutOfMemoryError='kill %p' org.apache.spark.executor.CoarseGrainedExecutorBackend --driver-url spark://CoarseGrainedScheduler@10.0.0.13:43942 --executor-id 4 --hostname 10.0.0.14 --cores 3 --app-id application_1485916338528_0008 --user-class-path file:/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/__app__.jar > /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stdout 2> /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stderr

        at org.apache.hadoop.util.Shell.runCommand(Shell.java:933)
        at org.apache.hadoop.util.Shell.run(Shell.java:844)
        at org.apache.hadoop.util.Shell$ShellCommandExecutor.execute(Shell.java:1123)
        at org.apache.hadoop.yarn.server.nodemanager.DefaultContainerExecutor.launchContainer(DefaultContainerExecutor.java:225)
        at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:317)
        at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:83)
        at java.util.concurrent.FutureTask.run(FutureTask.java:266)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
        at java.lang.Thread.run(Thread.java:745)


    Container exited with a non-zero exit code 134

<span data-ttu-id="a61d0-209">I det här fallet måste tooget hello misslyckades behållar-ID (i hello senare fallet är det container_1485916338528_0008_05_000005).</span><span class="sxs-lookup"><span data-stu-id="a61d0-209">In this case, you need tooget hello failed container ID (in hello above case, it is container_1485916338528_0008_05_000005).</span></span> <span data-ttu-id="a61d0-210">Måste toorun</span><span class="sxs-lookup"><span data-stu-id="a61d0-210">Then you need toorun</span></span> 

    yarn logs -containerId container_1485916338528_0008_03_000005

<span data-ttu-id="a61d0-211">från hello headnode.</span><span class="sxs-lookup"><span data-stu-id="a61d0-211">from hello headnode.</span></span> <span data-ttu-id="a61d0-212">När du har kontrollerat behållaren felet beror det på använder GPU-läge (där du ska använda CPU-läge i stället) i lenet_memory_solver.prototxt.</span><span class="sxs-lookup"><span data-stu-id="a61d0-212">After checking container failure, it is caused by using GPU mode (where you should use CPU mode instead) in lenet_memory_solver.prototxt.</span></span>

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written tooSTDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a><span data-ttu-id="a61d0-213">Hämtar resultat</span><span class="sxs-lookup"><span data-stu-id="a61d0-213">Getting results</span></span>

<span data-ttu-id="a61d0-214">Eftersom vi allokera 8 executors och det är enkelt att hello nätverkets topologi, tar bara ungefär 30 minuter toorun hello resultat.</span><span class="sxs-lookup"><span data-stu-id="a61d0-214">Since we are allocating 8 executors, and hello network topology is simple, it should only take around 30 minutes toorun hello result.</span></span> <span data-ttu-id="a61d0-215">Från kommandoraden hello, du kan se att vi placera hello modellen toowasb:///mnist.model och placera hello resultat tooa mapp med namnet wasb: / / / mnist_features_result.</span><span class="sxs-lookup"><span data-stu-id="a61d0-215">From hello command line, you can see that we put hello model toowasb:///mnist.model, and put hello results tooa folder named wasb:///mnist_features_result.</span></span>

<span data-ttu-id="a61d0-216">Du kan hämta hello resultat genom att köra</span><span class="sxs-lookup"><span data-stu-id="a61d0-216">You can get hello results by running</span></span>

    hadoop fs -cat hdfs:///mnist_features_result/*

<span data-ttu-id="a61d0-217">och hello resultatet ser ut som:</span><span class="sxs-lookup"><span data-stu-id="a61d0-217">and hello result looks like:</span></span>

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

<span data-ttu-id="a61d0-218">hello SampleID representerar hello-ID i hello MNIST dataset och hello etiketten är hello som hello modellen sidnumret.</span><span class="sxs-lookup"><span data-stu-id="a61d0-218">hello SampleID represents hello ID in hello MNIST dataset, and hello label is hello number that hello model identifies.</span></span>


## <a name="conclusion"></a><span data-ttu-id="a61d0-219">Slutsats</span><span class="sxs-lookup"><span data-stu-id="a61d0-219">Conclusion</span></span>

<span data-ttu-id="a61d0-220">Du har försökt tooinstall CaffeOnSpark med ett enkelt exempel i den här dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="a61d0-220">In this documentation, you have tried tooinstall CaffeOnSpark with running a simple example.</span></span> <span data-ttu-id="a61d0-221">HDInsight är en fullständigt hanterad distribuerade beräkning molnplattform och hello bästa platsen för att köra machine learning och avancerade analyser arbetsbelastningar på stora datamängder och för distribuerade djup, du kan använda Caffe HDInsight Spark tooperform djup inlärning aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a61d0-221">HDInsight is a full managed cloud distributed compute platform, and is hello best place for running machine learning and advanced analytics workloads on large data set, and for distributed deep learning, you can use Caffe on HDInsight Spark tooperform deep learning tasks.</span></span>


## <span data-ttu-id="a61d0-222"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="a61d0-222"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="a61d0-223">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="a61d0-223">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="a61d0-224">Scenarier</span><span class="sxs-lookup"><span data-stu-id="a61d0-224">Scenarios</span></span>
* [<span data-ttu-id="a61d0-225">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="a61d0-225">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="a61d0-226">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="a61d0-226">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a><span data-ttu-id="a61d0-227">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="a61d0-227">Manage resources</span></span>
* [<span data-ttu-id="a61d0-228">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="a61d0-228">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)

