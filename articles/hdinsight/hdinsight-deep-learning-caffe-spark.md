---
title: "Använda Caffe för distribuerade djup learning på Azure HDInsight Spark | Microsoft Docs"
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
ms.openlocfilehash: 14b7808c9534bce3049422d6bce1e8914b2c2fbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a><span data-ttu-id="a7402-103">Använda Caffe för distribuerade djup learning på Azure HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="a7402-103">Use Caffe on Azure HDInsight Spark for distributed deep learning</span></span>


## <a name="introduction"></a><span data-ttu-id="a7402-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="a7402-104">Introduction</span></span>

<span data-ttu-id="a7402-105">Djupgående learning är påverkar allt från hälsovård till transport till tillverkning och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="a7402-105">Deep learning is impacting everything from healthcare to transportation to manufacturing, and more.</span></span> <span data-ttu-id="a7402-106">Företag byter till djup Lär dig att lösa hårda problem som [bild klassificering](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [taligenkänning](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html)objekt recognition och datorn översättning.</span><span class="sxs-lookup"><span data-stu-id="a7402-106">Companies are turning to deep learning to solve hard problems, like [image classification](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [speech recognition](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html), object recognition, and machine translation.</span></span> 

<span data-ttu-id="a7402-107">Det finns [många populära ramverk](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), inklusive [Microsoft kognitiva Toolkit](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, osv. Caffe är en av de mest berömda ramverk för icke-symboliska (viktigt) neurala nätverket, och som används ofta i många områden, inklusive datorn vision.</span><span class="sxs-lookup"><span data-stu-id="a7402-107">There are [many popular frameworks](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), including [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, etc. Caffe is one of the most famous non-symbolic (imperative) neural network frameworks, and widely used in many areas including computer vision.</span></span> <span data-ttu-id="a7402-108">Dessutom [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) kombinerar Caffe med Apache Spark, i vilket fall djupt learning enkelt kan användas på en befintlig Hadoop-kluster tillsammans med Spark ETL pipelines, vilket minskar komplexiteten system och svarstid för slutpunkt till slutpunkt Learning.</span><span class="sxs-lookup"><span data-stu-id="a7402-108">Furthermore, [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) combines Caffe with Apache Spark, in which case deep learning can be easily used on an existing Hadoop cluster together with Spark ETL pipelines, reducing system complexity and latency for end-to-end learning.</span></span>

<span data-ttu-id="a7402-109">[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) endast fullständigt hanterad molnet Hadoop erbjudandet som innehåller optimerade öppen källkod analytiska kluster för Spark, Hive, MapReduce, HBase, Storm, Kafka och R Server backas upp av en SLA för 99,9%.</span><span class="sxs-lookup"><span data-stu-id="a7402-109">[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) is the only fully-managed cloud Hadoop offering that provides optimized open source analytic clusters for Spark, Hive, MapReduce, HBase, Storm, Kafka, and R Server backed by a 99.9% SLA.</span></span> <span data-ttu-id="a7402-110">Var och en av dessa tekniker för stordata och ISV-program kan enkelt distribueras som hanterade kluster med säkerhet och övervakning på företagsnivå.</span><span class="sxs-lookup"><span data-stu-id="a7402-110">Each of these big data technologies and ISV applications are easily deployable as managed clusters with enterprise-level security and monitoring.</span></span>

<span data-ttu-id="a7402-111">Vissa användare ber vi om hur du använder djup learning på HDInsight, vilket är Microsofts PaaS Hadoop-produkten.</span><span class="sxs-lookup"><span data-stu-id="a7402-111">Some users are asking us about how to use deep learning on HDInsight, which is Microsoft's PaaS Hadoop product.</span></span> <span data-ttu-id="a7402-112">Vi behöver mer resurs i framtiden, men vi vill sammanfatta en teknisk blogg om hur du använder Caffe på HDInsight Spark idag.</span><span class="sxs-lookup"><span data-stu-id="a7402-112">We will have more to share in the future, but today we want to summarize a technical blog on how to use Caffe on HDInsight Spark.</span></span>

<span data-ttu-id="a7402-113">Om du har installerat Caffe innan du ser du att installera det här ramverket är riktigt svårt.</span><span class="sxs-lookup"><span data-stu-id="a7402-113">If you have installed Caffe before, you will notice that installing this framework is a little bit challenging.</span></span> <span data-ttu-id="a7402-114">I den här bloggen kommer först beskriver vi hur du installerar [Caffe på Spark](https://github.com/yahoo/CaffeOnSpark) för ett HDInsight-kluster, demonstration sedan använda den inbyggda MNIST till demostrate hur du använder distribuerade djup Learning med HDInsight Spark på processorer.</span><span class="sxs-lookup"><span data-stu-id="a7402-114">In this blog, we will first illustrate how to install [Caffe on Spark](https://github.com/yahoo/CaffeOnSpark) for an HDInsight cluster, then use the built-in MNIST demo to demostrate how to use Distributed Deep Learning using HDInsight Spark on CPUs.</span></span>

<span data-ttu-id="a7402-115">Det finns fyra viktiga steg för att den fungerar på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a7402-115">There are four major steps to get it work on HDInsight.</span></span>

1. <span data-ttu-id="a7402-116">Installera de nödvändiga beroendena på alla noder</span><span class="sxs-lookup"><span data-stu-id="a7402-116">Install the required dependencies on all the nodes</span></span>
2. <span data-ttu-id="a7402-117">Skapa Caffe i Spark för HDInsight på huvudnoden</span><span class="sxs-lookup"><span data-stu-id="a7402-117">Build Caffe on Spark for HDInsight on the head node</span></span>
3. <span data-ttu-id="a7402-118">Distribuera bibliotek som krävs för alla arbetarnoder</span><span class="sxs-lookup"><span data-stu-id="a7402-118">Distribute the required libraries to all the worker nodes</span></span>
4. <span data-ttu-id="a7402-119">Skapa en Caffe modell och kör den distributely</span><span class="sxs-lookup"><span data-stu-id="a7402-119">Compose a Caffe model and run it distributely</span></span>

<span data-ttu-id="a7402-120">Eftersom HDInsight är en PaaS-lösning, erbjuder bra funktioner - så att det är ganska enkelt att utföra vissa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="a7402-120">Since HDInsight is a PaaS solution, it offers great platform features - so it is quite easy to perform some tasks.</span></span> <span data-ttu-id="a7402-121">En av de funktioner som vi använder kraftigt i det här blogginlägget kallas [skriptåtgärd](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), som du kan köra shell-kommandon för att anpassa klusternoder (huvudnod, arbetsnoden eller kantnod).</span><span class="sxs-lookup"><span data-stu-id="a7402-121">One of the features that we heavily use in this blog post is called [Script Action](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), with which you can execute shell commands to customize cluster nodes (head node, worker node, or edge node).</span></span>

## <a name="step-1--install-the-required-dependencies-on-all-the-nodes"></a><span data-ttu-id="a7402-122">Steg 1: Installera de nödvändiga beroendena på alla noder</span><span class="sxs-lookup"><span data-stu-id="a7402-122">Step 1:  Install the required dependencies on all the nodes</span></span>

<span data-ttu-id="a7402-123">Vi behöver installera beroenden som vi behöver för att komma igång.</span><span class="sxs-lookup"><span data-stu-id="a7402-123">To get started, we need to install the dependencies we need.</span></span> <span data-ttu-id="a7402-124">Webbplatsen Caffe och [CaffeOnSpark plats](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) erbjuder vissa användbar wiki för att installera beroenden för Spark på YARN-läget (som är i läget för HDInsight Spark), men vi måste lägga till några fler beroenden för HDInsight-plattformen.</span><span class="sxs-lookup"><span data-stu-id="a7402-124">The Caffe site and [CaffeOnSpark site](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) offers some very useful wiki for installing the dependencies for Spark on YARN mode (which is the mode for HDInsight Spark), but we need to add a few more dependencies for HDInsight platform.</span></span> <span data-ttu-id="a7402-125">Vi använder skriptåtgärden enligt nedan och kör det på alla huvudnoderna och arbetsnoder.</span><span class="sxs-lookup"><span data-stu-id="a7402-125">We will use the script action as below and run it on all the head nodes and worker nodes.</span></span> <span data-ttu-id="a7402-126">Den här skriptåtgärden tar ungefär 20 minuter som dessa beroenden beror också på andra paket.</span><span class="sxs-lookup"><span data-stu-id="a7402-126">This script action will take about 20 minutes, as those dependencies also depend on other packages.</span></span> <span data-ttu-id="a7402-127">Du bör placera den på en plats som har åtkomst till ditt HDInsight-kluster, till exempel en GitHub-plats eller standard BLOB storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a7402-127">You should put it in some location that is accessible to your HDInsight cluster, such as a GitHub location or the default BLOB storage account.</span></span>

    #!/bin/bash
    #Please be aware that installing the below will add additional 20 mins to cluster creation because of the dependencies
    #installing all dependencies, including the ones mentioned in http://caffe.berkeleyvision.org/install_apt.html, as well a few packages that are not included in HDInsight, such as gflags, glog, lmdb, numpy
    #It seems numpy will only needed during compilation time, but for safety purpose we install them on all the nodes

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


<span data-ttu-id="a7402-128">Det finns två steg i skriptåtgärden.</span><span class="sxs-lookup"><span data-stu-id="a7402-128">There are two steps in the script action above.</span></span> <span data-ttu-id="a7402-129">Det första steget är att installera bibliotek som krävs.</span><span class="sxs-lookup"><span data-stu-id="a7402-129">The first step is to install all the required libraries.</span></span> <span data-ttu-id="a7402-130">Dessa bibliotek innehåller de nödvändiga biblioteken för både kompilering Caffe (till exempel gflags glog) och kör Caffe (till exempel numpy).</span><span class="sxs-lookup"><span data-stu-id="a7402-130">Those libraries include the necessary libraries for both compiling Caffe(such as gflags, glog) and running Caffe (such as numpy).</span></span> <span data-ttu-id="a7402-131">Vi använder libatlas för CPU-optimering, men du kan alltid följa CaffeOnSpark-wiki om hur du installerar andra optimering bibliotek, till exempel MKL eller CUDA (för GPU).</span><span class="sxs-lookup"><span data-stu-id="a7402-131">We are using libatlas for CPU optimization, but you can always follow the CaffeOnSpark wiki on installing other optimization libraries, such as MKL or CUDA (for GPU).</span></span>

<span data-ttu-id="a7402-132">Det andra steget är att hämta, kompilera och installera protobuf 2.5.0 för Caffe under körningen.</span><span class="sxs-lookup"><span data-stu-id="a7402-132">The second step is to download, compile, and install protobuf 2.5.0 for Caffe during runtime.</span></span> <span data-ttu-id="a7402-133">Protobuf 2.5.0 [krävs](https://github.com/yahoo/CaffeOnSpark/issues/87), men den här versionen inte är tillgängligt som ett paket på Ubuntu 16, så vi behöver kompileras från källkoden.</span><span class="sxs-lookup"><span data-stu-id="a7402-133">Protobuf 2.5.0 [is required](https://github.com/yahoo/CaffeOnSpark/issues/87), however this version is not available as a package on Ubuntu 16, so we need to compile it from the source code.</span></span> <span data-ttu-id="a7402-134">Det finns också några resurser på Internet på hur du kompilera den, exempelvis [detta](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)</span><span class="sxs-lookup"><span data-stu-id="a7402-134">There are also a few resources on the Internet on how to compile it, such as [this](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)</span></span>

<span data-ttu-id="a7402-135">För att helt enkelt komma igång kan du bara köra den här skriptåtgärden mot ditt kluster för alla arbetarnoder och huvudnoderna (för HDInsight 3.5).</span><span class="sxs-lookup"><span data-stu-id="a7402-135">To simply get started, you can just run this script action against your cluster to all the worker nodes and head nodes (for HDInsight 3.5).</span></span> <span data-ttu-id="a7402-136">Du kan antingen köra skriptåtgärder för ett kluster som körs eller du kan också köra skriptåtgärder under tid som klustret etablera.</span><span class="sxs-lookup"><span data-stu-id="a7402-136">You can either run the script actions for a running cluster, or you can also run the script actions during the cluster provision time.</span></span> <span data-ttu-id="a7402-137">Mer information om åtgärderna som skript finns i dokumentationen [här](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)</span><span class="sxs-lookup"><span data-stu-id="a7402-137">For more details on the script actions, please see the documentation [here](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)</span></span>

![Skriptåtgärder att installera beroenden](./media/hdinsight-deep-learning-caffe-spark/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-the-head-node"></a><span data-ttu-id="a7402-139">Steg 2: Skapa Caffe på Spark för HDInsight på huvudnoden</span><span class="sxs-lookup"><span data-stu-id="a7402-139">Step 2: Build Caffe on Spark for HDInsight on the head node</span></span>

<span data-ttu-id="a7402-140">Det andra steget är att skapa Caffe på headnode och sedan distribuera de kompilerade bibliotek för alla arbetarnoder.</span><span class="sxs-lookup"><span data-stu-id="a7402-140">The second step is to build Caffe on the headnode, and then distribute the compiled libraries to all the worker nodes.</span></span> <span data-ttu-id="a7402-141">I det här steget behöver du [ssh till din headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), Följ bara den [CaffeOnSpark Byggprocessen](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), och nedan är de skript som du kan använda för att skapa CaffeOnSpark med några ytterligare steg.</span><span class="sxs-lookup"><span data-stu-id="a7402-141">In this step, you will need to [ssh into your headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), then simply follow the [CaffeOnSpark build process](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), and below is the script you can use to build CaffeOnSpark with a few additional steps.</span></span> 

    #!/bin/bash
    git clone https://github.com/yahoo/CaffeOnSpark.git --recursive
    export CAFFE_ON_SPARK=$(pwd)/CaffeOnSpark

    pushd ${CAFFE_ON_SPARK}/caffe-public/
    cp Makefile.config.example Makefile.config
    echo "INCLUDE_DIRS += ${JAVA_HOME}/include" >> Makefile.config
    #Below configurations might need to be updated based on actual cases. For example, if you are using GPU, or using a different BLAS library, you may want to update those settings accordingly.
    echo "CPU_ONLY := 1" >> Makefile.config
    echo "BLAS := atlas" >> Makefile.config
    echo "INCLUDE_DIRS += /usr/include/hdf5/serial/" >> Makefile.config
    echo "LIBRARY_DIRS += /usr/lib/x86_64-linux-gnu/hdf5/serial/" >> Makefile.config
    popd

    #compile CaffeOnSpark
    pushd ${CAFFE_ON_SPARK}
    #always clean up the environment before building (especially when rebuiding), or there will be errors such as "failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2"
    make clean 
    #the build step usually takes 20~30 mins, since it has a lot maven dependencies
    make build 
    popd
    export LD_LIBRARY_PATH=${CAFFE_ON_SPARK}/caffe-public/distribute/lib:${CAFFE_ON_SPARK}/caffe-distri/distribute/lib

    hadoop fs -mkdir -p wasb:///projects/machine_learning/image_dataset

    ${CAFFE_ON_SPARK}/scripts/setup-mnist.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/mnist_*_lmdb wasb:///projects/machine_learning/image_dataset/

    ${CAFFE_ON_SPARK}/scripts/setup-cifar10.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/cifar10_*_lmdb wasb:///projects/machine_learning/image_dataset/

    #put the already compiled CaffeOnSpark libraries to wasb storage, then read back to each node using script actions. This is because CaffeOnSpark requires all the nodes have the libarries
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-public/distribute/lib/
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-distri/distribute/lib/* /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-public/distribute/lib/* /CaffeOnSpark/caffe-public/distribute/lib/

<span data-ttu-id="a7402-142">Du kan behöva göra mer än vad som står i dokumentationen för CaffeOnSpark.</span><span class="sxs-lookup"><span data-stu-id="a7402-142">You may need to do more than what the documentation of CaffeOnSpark says.</span></span> <span data-ttu-id="a7402-143">Ändringarna har:</span><span class="sxs-lookup"><span data-stu-id="a7402-143">The changes are:</span></span>
- <span data-ttu-id="a7402-144">Ändra till CPU endast och använda libatlas för den här särskilt ändamål.</span><span class="sxs-lookup"><span data-stu-id="a7402-144">Change to CPU only and use libatlas for this particular purpose.</span></span>
- <span data-ttu-id="a7402-145">Placera datauppsättningar till BLOB storage, vilket är en delad plats som är tillgänglig för alla arbetarnoder för senare användning.</span><span class="sxs-lookup"><span data-stu-id="a7402-145">Put the datasets to the BLOB storage, which is a shared location that is accessible to all worker nodes for later use.</span></span>
- <span data-ttu-id="a7402-146">Placera de kompilerade Caffe bibliotek till BLOB storage och du kommer senare att kopiera dessa bibliotek till alla noder med skriptåtgärder för att undvika ytterligare kompileringstid.</span><span class="sxs-lookup"><span data-stu-id="a7402-146">Put the compiled Caffe libraries to BLOB storage, and later you will copy those libraries to all the nodes using script actions to avoid additional compilation time.</span></span>


### <a name="troubleshooting-an-ant-buildexception-has-occured-exec-returned-2"></a><span data-ttu-id="a7402-147">Felsökning: Ett Ant BuildException har inträffat: exec som returnerades: 2</span><span class="sxs-lookup"><span data-stu-id="a7402-147">Troubleshooting: An Ant BuildException has occured: exec returned: 2</span></span>

<span data-ttu-id="a7402-148">När du först försöker skapa CaffeOnSpark, anges ibland</span><span class="sxs-lookup"><span data-stu-id="a7402-148">When first trying to build CaffeOnSpark, sometimes it will say</span></span>

    failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

<span data-ttu-id="a7402-149">Rensa kod databasen genom att helt enkelt ”gör ren” och sedan kör ”Kontrollera skapa” löser problemet, så länge som du har rätt beroenden.</span><span class="sxs-lookup"><span data-stu-id="a7402-149">Simply clean the code repository by "make clean" and then run "make build" will solve this issue, as long as you have the correct dependencies.</span></span>

### <a name="troubleshooting-maven-repository-connection-time-out"></a><span data-ttu-id="a7402-150">Felsöka: Maven databasen anslutnings-timeout</span><span class="sxs-lookup"><span data-stu-id="a7402-150">Troubleshooting: Maven repository connection time out</span></span>

<span data-ttu-id="a7402-151">Ibland ger maven mig på timeout-anslutningsfel, liknande den nedan:</span><span class="sxs-lookup"><span data-stu-id="a7402-151">Sometimes maven gives me the connection time out error, similar to below:</span></span>

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request to {s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

<span data-ttu-id="a7402-152">Den kommer att OK Vänta några minuter och försök sedan bara att återskapa kod, så kan det vara Maven på något sätt begränsar trafiken från en angiven IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a7402-152">It will be OK after waiting for a few minutes and then just try to rebuild the code, so it might be Maven somehow limits the traffic from a given IP address.</span></span>


### <a name="troubleshooting-test-failure-for-caffe"></a><span data-ttu-id="a7402-153">Felsöka: Testa fel för Caffe</span><span class="sxs-lookup"><span data-stu-id="a7402-153">Troubleshooting: Test failure for Caffe</span></span>

<span data-ttu-id="a7402-154">Förmodligen visas ett testfel när du gör den sista kontrollen för CaffeOnSpark, liknande med nedan.</span><span class="sxs-lookup"><span data-stu-id="a7402-154">You probably will see a test failure when doing the final check for CaffeOnSpark, similar with below.</span></span> <span data-ttu-id="a7402-155">Detta är prabably relaterat med UTF-8-kodning, men ska inte påverkar användningen av Caffe</span><span class="sxs-lookup"><span data-stu-id="a7402-155">This is prabably related with UTF-8 encoding, but should not impact the usage of Caffe</span></span>

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-the-required-libraries-to-all-the-worker-nodes"></a><span data-ttu-id="a7402-156">Steg 3: Distribuera bibliotek som krävs för alla arbetarnoder</span><span class="sxs-lookup"><span data-stu-id="a7402-156">Step 3: Distribute the required libraries to all the worker nodes</span></span>

<span data-ttu-id="a7402-157">Nästa steg är att distribuera biblioteken (i praktiken bibliotek i CaffeOnSpark/caffe-offentlig/distribuera/lib/och CaffeOnSpark/caffe-distri/distribuera/lib /) till alla noder.</span><span class="sxs-lookup"><span data-stu-id="a7402-157">The next step is to distribute the libraries (basically the libraries in CaffeOnSpark/caffe-public/distribute/lib/ and CaffeOnSpark/caffe-distri/distribute/lib/) to all the nodes.</span></span> <span data-ttu-id="a7402-158">Vi har gjort dessa bibliotek på BLOB storage i steg 2, och i det här steget ska vi använda skriptåtgärder för att kopiera den till alla huvudnoderna och arbetsnoder.</span><span class="sxs-lookup"><span data-stu-id="a7402-158">In Step 2, we put those libraries on BLOB storage, and in this step, we will use script actions to copy it to all the head nodes and worker nodes.</span></span>

<span data-ttu-id="a7402-159">Om du vill göra det enkelt kör en skriptåtgärd som nedan (du måste peka på rätt plats som är specifika för ditt kluster):</span><span class="sxs-lookup"><span data-stu-id="a7402-159">To do this, simple run a script action as below (you need to point to the right location specific to your cluster):</span></span>

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

<span data-ttu-id="a7402-160">Eftersom steg 2, vi lägga till den i BLOB-lagring som är tillgänglig för alla noder, i det här steget kopiera vi bara helt enkelt den till alla noder.</span><span class="sxs-lookup"><span data-stu-id="a7402-160">Because in step 2, we put it on the BLOB storage which is accessible to all the nodes, in this step we just simply copy it to all the nodes.</span></span>

## <a name="step-4-compose-a-caffe-model-and-run-it-distributely"></a><span data-ttu-id="a7402-161">Steg 4: Skapa en Caffe modell och kör den distributely</span><span class="sxs-lookup"><span data-stu-id="a7402-161">Step 4: Compose a Caffe model and run it distributely</span></span>

<span data-ttu-id="a7402-162">När du har kört stegen ovan Caffe är installerad på headnode alreay och vi är redo att börja.</span><span class="sxs-lookup"><span data-stu-id="a7402-162">After running the above steps, Caffe is alreay installed on the headnode and we are good to go.</span></span> <span data-ttu-id="a7402-163">Nästa steg är att skriva en Caffe modell.</span><span class="sxs-lookup"><span data-stu-id="a7402-163">The next step is to write a Caffe model.</span></span> 

<span data-ttu-id="a7402-164">Caffe via en ”lättfattliga arkitektur”, där för att skapa en modell, behöver du bara ange en konfigurationsfil, och utan kodning alls (i de flesta fall).</span><span class="sxs-lookup"><span data-stu-id="a7402-164">Caffe is using an "expressive architecture", where for composing a model, you just need to define a configuration file, and without coding at all (in most cases).</span></span> <span data-ttu-id="a7402-165">Vi titta det.</span><span class="sxs-lookup"><span data-stu-id="a7402-165">So let's take a look there.</span></span> 

<span data-ttu-id="a7402-166">Vi kommer idag träna modellen är en exempel-modell för MNIST utbildning.</span><span class="sxs-lookup"><span data-stu-id="a7402-166">The model we will train today is a sample model for MNIST training.</span></span> <span data-ttu-id="a7402-167">MNIST databasen handskriven siffror har en uppsättning 60 000 exempel utbildning och en test av 10 000 exempel.</span><span class="sxs-lookup"><span data-stu-id="a7402-167">The MNIST database of handwritten digits has a training set of 60,000 examples, and a test set of 10,000 examples.</span></span> <span data-ttu-id="a7402-168">Det är en del av en större grupp som är tillgängliga från NIST.</span><span class="sxs-lookup"><span data-stu-id="a7402-168">It is a subset of a larger set available from NIST.</span></span> <span data-ttu-id="a7402-169">Siffrorna har storleken-normaliserat och centrerade i en bild med fast storlek.</span><span class="sxs-lookup"><span data-stu-id="a7402-169">The digits have been size-normalized and centered in a fixed-size image.</span></span> <span data-ttu-id="a7402-170">CaffeOnSpark har vissa skript för att hämta datauppsättningen och omvandla dem till rätt format.</span><span class="sxs-lookup"><span data-stu-id="a7402-170">CaffeOnSpark has some scripts to download the dataset and convert it into the right format.</span></span>

<span data-ttu-id="a7402-171">CaffeOnSpark innehåller några exempel på nätverket topologier för MNIST utbildning.</span><span class="sxs-lookup"><span data-stu-id="a7402-171">CaffeOnSpark provides some network topologies example for MNIST training.</span></span> <span data-ttu-id="a7402-172">Den har en bra design på Dela nätverksarkitekturen (nätverkets topologi) och optimering.</span><span class="sxs-lookup"><span data-stu-id="a7402-172">It has a nice design of splitting the network architecture (the topology of the network) and optimization.</span></span> <span data-ttu-id="a7402-173">I det här fallet finns två filer krävs:</span><span class="sxs-lookup"><span data-stu-id="a7402-173">In this case, There are two files required:</span></span> 

<span data-ttu-id="a7402-174">filen ”Solver” (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) används för att övervaka optimeringen och generera parametern uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="a7402-174">the "Solver" file (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) is used for overseeing the optimization and generating parameter updates.</span></span> <span data-ttu-id="a7402-175">Exempelvis definierar om CPU eller GPU kommer att användas, vad är momentum, hur många iterationer kommer att vara, osv. Den definierar även vilka neuron nätverkstopologi ska användas i programmet (som är den andra filen måste).</span><span class="sxs-lookup"><span data-stu-id="a7402-175">For example, it defines whether CPU or GPU will be used, what's the momentum, how many iterations will be, etc. It also defines which neuron network topology should the program use (which is the second file we need).</span></span> <span data-ttu-id="a7402-176">Mer information om Solver Se [Caffe dokumentationen](http://caffe.berkeleyvision.org/tutorial/solver.html).</span><span class="sxs-lookup"><span data-stu-id="a7402-176">For more information about Solver, please refer to [Caffe documentation](http://caffe.berkeleyvision.org/tutorial/solver.html).</span></span>

<span data-ttu-id="a7402-177">Eftersom vi använder CPU i stället för GPU, i det här exemplet ska vi ändra den sista raden till:</span><span class="sxs-lookup"><span data-stu-id="a7402-177">For this example, since we are using CPU rather than GPU, we should change the last line to:</span></span>

    # solver mode: CPU or GPU
    solver_mode: CPU

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-1.png)

<span data-ttu-id="a7402-179">Du kan ändra andra rader efter behov.</span><span class="sxs-lookup"><span data-stu-id="a7402-179">You can change other lines as needed.</span></span>

<span data-ttu-id="a7402-180">Den andra filen (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) definierar hur nätverkets neuron ser ut och relevanta indata och utdata-fil.</span><span class="sxs-lookup"><span data-stu-id="a7402-180">The second file (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) defines how the neuron network looks like, and the relevant input and output file.</span></span> <span data-ttu-id="a7402-181">Vi måste också uppdatera-filen för att återspegla Dataplats utbildning.</span><span class="sxs-lookup"><span data-stu-id="a7402-181">We also need to update the file to reflect the training data location.</span></span> <span data-ttu-id="a7402-182">Ändra följande del i lenet_memory_train_test.prototxt (du måste peka på rätt plats som är specifika för ditt kluster):</span><span class="sxs-lookup"><span data-stu-id="a7402-182">Change the following part in lenet_memory_train_test.prototxt (you need to point to the right location specific to your cluster):</span></span>

- <span data-ttu-id="a7402-183">Ändra ”file:/Users/mridul/bigml/demodl/mnist_train_lmdb” till ”wasb: / / / projekt/machine_learning/image_dataset/mnist_train_lmdb”</span><span class="sxs-lookup"><span data-stu-id="a7402-183">change the "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" to "wasb:///projects/machine_learning/image_dataset/mnist_train_lmdb"</span></span>
- <span data-ttu-id="a7402-184">Ändra ”file:/Users/mridul/bigml/demodl/mnist_test_lmdb/” till ”wasb: / / / projekt/machine_learning/image_dataset/mnist_test_lmdb”</span><span class="sxs-lookup"><span data-stu-id="a7402-184">change "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" to "wasb:///projects/machine_learning/image_dataset/mnist_test_lmdb"</span></span>

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-2.png)

<span data-ttu-id="a7402-186">Mer information om hur du definierar nätverket Kontrollera den [Caffe dokumentation om MNIST dataset](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)</span><span class="sxs-lookup"><span data-stu-id="a7402-186">For more information on how to define the network, please check the [Caffe documentation on MNIST dataset](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)</span></span>

<span data-ttu-id="a7402-187">I den här bloggen använda vi bara det här enkla exemplet MNIST.</span><span class="sxs-lookup"><span data-stu-id="a7402-187">For the purpose of this blog, we just use this simple MNIST example.</span></span> <span data-ttu-id="a7402-188">Du bör köra kommandot nedan från huvudnod:</span><span class="sxs-lookup"><span data-stu-id="a7402-188">You should run the command below from the head node:</span></span>

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

<span data-ttu-id="a7402-189">I praktiken den distribuerar filerna som krävs (lenet_memory_solver.prototxt och lenet_memory_train_test.prototxt) till varje YARN-behållaren och också ställa in relevanta SÖKVÄGEN för varje drivrutin/utföraren Spark LD_LIBRARY_PATH som definieras i föregående kod fragment och pekar på den plats som har CaffeOnSpark bibliotek.</span><span class="sxs-lookup"><span data-stu-id="a7402-189">Basically it distributes the required files (lenet_memory_solver.prototxt and lenet_memory_train_test.prototxt) to each YARN container, and also set the relevant PATH of each Spark driver/executor to LD_LIBRARY_PATH, which is defined in the previous code snippet and points to the location that has CaffeOnSpark libraries.</span></span> 

## <a name="monitoring-and-troubleshooting"></a><span data-ttu-id="a7402-190">Övervakning och felsökning</span><span class="sxs-lookup"><span data-stu-id="a7402-190">Monitoring and troubleshooting</span></span>

<span data-ttu-id="a7402-191">Eftersom vi använder YARN klustret läget i vilket fall Spark-drivrutin kommer att schemaläggas för en godtycklig behållare (och en valfri arbetsnoden) bör endast visas i konsolen mata ut ungefär så:</span><span class="sxs-lookup"><span data-stu-id="a7402-191">Since we are using YARN cluster mode, in which case the Spark driver will be scheduled to an arbitrary container (and an arbitrary worker node) you should only see in the console outputting something like:</span></span>

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

<span data-ttu-id="a7402-192">Om du vill veta vad som hänt behöver du vanligtvis Spark-drivrutinens logg som innehåller mer information.</span><span class="sxs-lookup"><span data-stu-id="a7402-192">If you want to know what happened, you usually need to get the Spark driver's log, which has more information.</span></span> <span data-ttu-id="a7402-193">I det här fallet måste gå till YARN-Användargränssnittet för att hitta relevanta YARN-loggar.</span><span class="sxs-lookup"><span data-stu-id="a7402-193">In this case, you need to go to the YARN UI to find the relevant YARN logs.</span></span> <span data-ttu-id="a7402-194">Du kan få YARN-Användargränssnittet av denna URL:</span><span class="sxs-lookup"><span data-stu-id="a7402-194">You can get the YARN UI by this URL:</span></span> 

    https://yourclustername.azurehdinsight.net/yarnui
   
![YARN-ANVÄNDARGRÄNSSNITTET](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-1.png)

<span data-ttu-id="a7402-196">Du kan ta en titt på hur många resurser för det aktuella programmet.</span><span class="sxs-lookup"><span data-stu-id="a7402-196">You can take a look at how many resources are allocated for this particular application.</span></span> <span data-ttu-id="a7402-197">Du kan klicka på länken ”Scheduler” och sedan du kommer att för det här programmet, det finns 9 behållare som körs.</span><span class="sxs-lookup"><span data-stu-id="a7402-197">You can click the "Scheduler" link, and then you will see that for this application, there are 9 containers running.</span></span> <span data-ttu-id="a7402-198">Vi ber YARN att tillhandahålla 8 executors och en annan behållare för drivrutinen processen.</span><span class="sxs-lookup"><span data-stu-id="a7402-198">We ask YARN to provide 8 executors, and another container is for driver process.</span></span> 

![YARN Schemaläggaren](./media/hdinsight-deep-learning-caffe-spark/YARN-Scheduler.png)

<span data-ttu-id="a7402-200">Du kanske vill kontrollera drivrutinsloggar eller behållaren loggar om det finns fel.</span><span class="sxs-lookup"><span data-stu-id="a7402-200">You may want to check the  driver logs or container logs if there are failures.</span></span> <span data-ttu-id="a7402-201">För drivrutinsloggar kan du klicka på det program-ID i YARN-Användargränssnittet och sedan klicka på knappen ”loggar”.</span><span class="sxs-lookup"><span data-stu-id="a7402-201">For driver logs, you can click the application ID in YARN UI, then click the "Logs" button.</span></span> <span data-ttu-id="a7402-202">Drivrutinen loggarna skrivs till stderr.</span><span class="sxs-lookup"><span data-stu-id="a7402-202">The driver logs are written into stderr.</span></span>

![YARN-ANVÄNDARGRÄNSSNITTET 2](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-2.png)

<span data-ttu-id="a7402-204">Du kan se exempelvis några fel under från drivrutinsloggar som anger du allokera för många executors.</span><span class="sxs-lookup"><span data-stu-id="a7402-204">For example, you might see some of the error below from the driver logs, indicating you allocate too many executors.</span></span>

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

<span data-ttu-id="a7402-205">Ibland kan kan problemet inträffa i executors i stället för drivrutiner.</span><span class="sxs-lookup"><span data-stu-id="a7402-205">Sometimes, the issue can happen in executors rather than drivers.</span></span> <span data-ttu-id="a7402-206">Du måste i så fall behållaren finns i loggarna.</span><span class="sxs-lookup"><span data-stu-id="a7402-206">In this case, you need to check the container logs.</span></span> <span data-ttu-id="a7402-207">Du kan alltid få loggar för behållaren och få behållaren misslyckades.</span><span class="sxs-lookup"><span data-stu-id="a7402-207">You can always get the container logs, and then get the failed container.</span></span> <span data-ttu-id="a7402-208">Du kan till exempel uppfylla det här felet när du kör Caffe.</span><span class="sxs-lookup"><span data-stu-id="a7402-208">For example, you might meet this failure when running Caffe.</span></span>

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

<span data-ttu-id="a7402-209">I detta fall du behöver misslyckade behållar-ID (ovan om det är container_1485916338528_0008_05_000005).</span><span class="sxs-lookup"><span data-stu-id="a7402-209">In this case, you need to get the failed container ID (in the above case, it is container_1485916338528_0008_05_000005).</span></span> <span data-ttu-id="a7402-210">Sedan måste du köra</span><span class="sxs-lookup"><span data-stu-id="a7402-210">Then you need to run</span></span> 

    yarn logs -containerId container_1485916338528_0008_03_000005

<span data-ttu-id="a7402-211">från headnode.</span><span class="sxs-lookup"><span data-stu-id="a7402-211">from the headnode.</span></span> <span data-ttu-id="a7402-212">När du har kontrollerat behållaren felet beror det på använder GPU-läge (där du ska använda CPU-läge i stället) i lenet_memory_solver.prototxt.</span><span class="sxs-lookup"><span data-stu-id="a7402-212">After checking container failure, it is caused by using GPU mode (where you should use CPU mode instead) in lenet_memory_solver.prototxt.</span></span>

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written to STDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a><span data-ttu-id="a7402-213">Hämtar resultat</span><span class="sxs-lookup"><span data-stu-id="a7402-213">Getting results</span></span>

<span data-ttu-id="a7402-214">Eftersom vi allokera 8 executors och det är enkelt att nätverkets topologi, bör det endast ta runt 30 minuter för att köra resultatet.</span><span class="sxs-lookup"><span data-stu-id="a7402-214">Since we are allocating 8 executors, and the network topology is simple, it should only take around 30 minutes to run the result.</span></span> <span data-ttu-id="a7402-215">Från kommandoraden, kan du se att vi placera modellen till wasb:///mnist.model och lägga resultaten till en mapp med namnet wasb: / / / mnist_features_result.</span><span class="sxs-lookup"><span data-stu-id="a7402-215">From the command line, you can see that we put the model to wasb:///mnist.model, and put the results to a folder named wasb:///mnist_features_result.</span></span>

<span data-ttu-id="a7402-216">Du kan hämta resultaten genom att köra</span><span class="sxs-lookup"><span data-stu-id="a7402-216">You can get the results by running</span></span>

    hadoop fs -cat hdfs:///mnist_features_result/*

<span data-ttu-id="a7402-217">och resultatet ser ut som:</span><span class="sxs-lookup"><span data-stu-id="a7402-217">and the result looks like:</span></span>

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

<span data-ttu-id="a7402-218">SampleID representerar ID i datamängden MNIST och etiketten är det nummer som identifierar modellen.</span><span class="sxs-lookup"><span data-stu-id="a7402-218">The SampleID represents the ID in the MNIST dataset, and the label is the number that the model identifies.</span></span>


## <a name="conclusion"></a><span data-ttu-id="a7402-219">Slutsats</span><span class="sxs-lookup"><span data-stu-id="a7402-219">Conclusion</span></span>

<span data-ttu-id="a7402-220">Du har försökt att installera CaffeOnSpark med ett enkelt exempel i den här dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="a7402-220">In this documentation, you have tried to install CaffeOnSpark with running a simple example.</span></span> <span data-ttu-id="a7402-221">HDInsight är en fullständigt hanterad distribuerade beräkning molnplattform och är den bästa platsen för att köra machine learning och avancerade analyser arbetsbelastningar på stora datamängder och för distribuerade djup, du kan använda Caffe på HDInsight Spark för att utföra djup learning aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a7402-221">HDInsight is a full managed cloud distributed compute platform, and is the best place for running machine learning and advanced analytics workloads on large data set, and for distributed deep learning, you can use Caffe on HDInsight Spark to perform deep learning tasks.</span></span>


## <span data-ttu-id="a7402-222"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="a7402-222"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="a7402-223">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="a7402-223">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="a7402-224">Scenarier</span><span class="sxs-lookup"><span data-stu-id="a7402-224">Scenarios</span></span>
* [<span data-ttu-id="a7402-225">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="a7402-225">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="a7402-226">Spark med Machine Learning: Använda Spark i HDInsight för att förutsäga resultatet av en livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="a7402-226">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a><span data-ttu-id="a7402-227">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="a7402-227">Manage resources</span></span>
* [<span data-ttu-id="a7402-228">Hantera resurser för Apache Spark-klustret i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="a7402-228">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)

