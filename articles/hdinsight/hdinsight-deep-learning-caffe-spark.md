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
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a>Använda Caffe för distribuerade djup learning på Azure HDInsight Spark


## <a name="introduction"></a>Introduktion

Djupgående learning är påverkar allt från sjukvården tootransportation toomanufacturing och mycket mer. Företag aktiverar toodeep learning toosolve hårda problem, t.ex. [bild klassificering](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [taligenkänning](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html)objekt recognition och datorn översättning. 

Det finns [många populära ramverk](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), inklusive [Microsoft kognitiva Toolkit](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, osv. Caffe är en av hello mest berömda icke symboliska (viktigt) neurala nätverket ramverk, och som används ofta i många områden, inklusive datorn vision. Dessutom [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) kombinerar Caffe med Apache Spark, i vilket fall djupt learning enkelt kan användas på en befintlig Hadoop-kluster tillsammans med Spark ETL pipelines, vilket minskar komplexiteten system och svarstid för slutpunkt till slutpunkt Learning.

[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) är hello, bara fullständigt hanterade molnet Hadoop erbjudande som tillhandahåller optimerade öppen källkod analytiska kluster för Spark, Hive, MapReduce, HBase, Storm, Kafka och R Server backas upp av en SLA för 99,9%. Var och en av dessa tekniker för stordata och ISV-program kan enkelt distribueras som hanterade kluster med säkerhet och övervakning på företagsnivå.

Vissa användare ber vi om hur toouse djup learning på HDInsight, vilket är Microsofts PaaS Hadoop-produkten. Vi kommer att ha flera tooshare i hello framtida, men idag vi vill toosummarize en teknisk blogg om hur toouse Caffe på HDInsight Spark.

Om du har installerat Caffe innan du ser du att installera det här ramverket är riktigt svårt. I den här bloggen vi först illustrerar hur tooinstall [Caffe på Spark](https://github.com/yahoo/CaffeOnSpark) för ett HDInsight-kluster, sedan använda hello inbyggda MNIST demo toodemostrate hur toouse distribuerade djup Learning med HDInsight Spark på processorer.

Det finns fyra viktiga steg tooget den fungerar på HDInsight.

1. Installera beroenden hello krävs på alla hello noder
2. Skapa Caffe i Spark för HDInsight på hello huvudnod
3. Distribuera hello krävs bibliotek tooall hello arbetsnoder
4. Skapa en Caffe modell och kör den distributely

Eftersom HDInsight är en PaaS-lösning, erbjuder den bra funktioner - så att det är ganska enkelt tooperform vissa uppgifter. En av hello-funktioner som vi använder kraftigt i det här blogginlägget kallas [skriptåtgärd](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), med vilket du kan köra kommandon shell toocustomize klusternoder (huvudnod, arbetsnoden eller kantnod).

## <a name="step-1--install-hello-required-dependencies-on-all-hello-nodes"></a>Steg 1: Installera beroenden hello krävs på alla noder i hello

tooget igång, behöver vi tooinstall hello beroenden måste. Hej Caffe plats och [CaffeOnSpark plats](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) erbjuder vissa användbar wiki för att installera hello beroenden för Spark på YARN-läget (som är hello-läge för HDInsight Spark), men vi behöver tooadd några fler beroenden för HDInsight-plattformen. Vi använder hello skriptåtgärd enligt nedan och kör det på alla hello huvudnoderna och arbetsnoder. Den här skriptåtgärden tar ungefär 20 minuter som dessa beroenden beror också på andra paket. Du bör placera den på en plats som är tillgänglig tooyour HDInsight-klustret, till exempel en GitHub-plats eller hello standard BLOB storage-konto.

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


Det finns två steg i hello skriptåtgärd ovan. hello första steget är tooinstall alla hello bibliotek som krävs. Dessa bibliotek innehåller nödvändiga hello-bibliotek för både kompilera Caffe (till exempel gflags glog) och köra Caffe (till exempel numpy). Vi använder libatlas för CPU-optimering, men du kan alltid följa hello CaffeOnSpark wiki om hur du installerar andra optimering bibliotek, till exempel MKL eller CUDA (för GPU).

hello andra steget är toodownload, kompilera och installera protobuf 2.5.0 för Caffe under körningen. Protobuf 2.5.0 [krävs](https://github.com/yahoo/CaffeOnSpark/issues/87), men den här versionen inte är tillgängligt som ett paket på Ubuntu 16, så vi behöver toocompile från hello källkoden. Det finns också några resurser på hello Internet på hur toocompile, som [detta](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)

toosimply komma igång, du kan bara köra den här skriptåtgärden mot ditt kluster tooall hello worker noder och head noder (för HDInsight 3.5). Du kan antingen köra hello skriptåtgärder för ett kluster som körs eller du kan också köra hello skriptåtgärder under hello klustret etablera tiden. Mer information om hello skriptåtgärder finns hello dokumentationen [här](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)

![Skriptet åtgärder tooInstall beroenden](./media/hdinsight-deep-learning-caffe-spark/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-hello-head-node"></a>Steg 2: Skapa Caffe på Spark för HDInsight på hello huvudnod

hello andra steget är toobuild Caffe på hello headnode och sedan distribuera hello kompileras bibliotek tooall hello arbetsnoderna. I det här steget behöver du för[ssh till din headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), Följ bara hello [CaffeOnSpark Byggprocessen](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), och nedan är hello-skript som du kan använda toobuild CaffeOnSpark med några ytterligare steg. 

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

Du kan behöva toodo mer än vad hello-dokumentationen för CaffeOnSpark står. hello ändringar är:
- Ändra bara tooCPU och använda libatlas för den här särskilt ändamål.
- Placera hello datauppsättningar toohello BLOB storage som är en delad plats som är tillgänglig tooall arbetarnoder för senare användning.
- Placera hello kompileras Caffe bibliotek tooBLOB lagring och du kommer senare att kopiera dessa bibliotek tooall hello-noder som använder skriptet åtgärder tooavoid ytterligare kompileringstid.


### <a name="troubleshooting-an-ant-buildexception-has-occured-exec-returned-2"></a>Felsökning: Ett Ant BuildException har inträffat: exec som returnerades: 2

När du först försöker toobuild CaffeOnSpark, anges ibland

    failed tooexecute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

Bara ren hello databasen genom att ”kontrollera ren” och sedan kör ”Kontrollera skapa” löser problemet, så länge som du har rätt hello-beroenden.

### <a name="troubleshooting-maven-repository-connection-time-out"></a>Felsöka: Maven databasen anslutnings-timeout

Ibland ger maven mig hello timeout anslutningsfel, liknande toobelow:

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request too{s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

Den OK Vänta några minuter och sedan försöka toorebuild hello kod, så kan det vara Maven på något sätt gränser hello trafik från en angiven IP-adress.


### <a name="troubleshooting-test-failure-for-caffe"></a>Felsöka: Testa fel för Caffe

Förmodligen visas ett testfel när du gör hello sista kontrollen för CaffeOnSpark, liknande med nedan. Detta är prabably relaterat med UTF-8-kodning, men ska inte påverkar hello användning av Caffe

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-hello-required-libraries-tooall-hello-worker-nodes"></a>Steg 3: Distribuera hello krävs bibliotek tooall hello arbetsnoder

hello nästa steg är toodistribute hello bibliotek (i praktiken hello-biblioteken i CaffeOnSpark/caffe-offentlig/distribuera/lib/och CaffeOnSpark/caffe-distri/distribuera/lib /) tooall hello noder. Steg 2, vi lägga till dessa bibliotek i BLOB storage och i det här steget ska vi använda skriptet åtgärder toocopy den tooall hello huvudnoderna och arbetsnoder.

toodo Detta möjliggör enkel kör en skriptåtgärd som nedan (du måste toopoint toohello rätt plats specifika tooyour kluster):

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

Eftersom i steg 2, vi placera den hello BLOB-lagring som är tillgänglig tooall hello noder, i det här steget kopiera vi bara bara det tooall hello noder.

## <a name="step-4-compose-a-caffe-model-and-run-it-distributely"></a>Steg 4: Skapa en Caffe modell och kör den distributely

När du har kört hello senare steg, Caffe är installerad på hello headnode alreay och vi är bra toogo. hello nästa steg är toowrite en Caffe modell. 

Caffe använder en ”lättfattliga arkitektur”, där för att skapa en modell du bara behöver toodefine en konfigurationsfil och utan kodning alls (i de flesta fall). Vi titta det. 

hello modellen ska vi träna idag är en exempel-modell för MNIST utbildning. Hej är MNIST handskriven siffror en uppsättning 60 000 exempel utbildning och en test av 10 000 exempel. Det är en del av en större grupp som är tillgängliga från NIST. hello siffror har storleken-normaliserat och centrerade i en bild med fast storlek. CaffeOnSpark har vissa skript toodownload hello dataset och omvandla dem till hello rätt format.

CaffeOnSpark innehåller några exempel på nätverket topologier för MNIST utbildning. Den har en bra design på Dela hello nätverksarkitekturen (hello topologi för hello nätverk) och optimering. I det här fallet finns två filer krävs: 

Hej ”Solver” filen (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) används för att övervaka hello optimering och generera parametern uppdateringar. Exempelvis definierar om CPU eller GPU kommer att användas, vad är hello momentum, hur många iterationer kommer att vara, osv. Den definierar vilka neuron nätverkstopologi bör hello programmet används (vilket är hello andra fil som vi behöver). Mer information om Solver finns för[Caffe dokumentationen](http://caffe.berkeleyvision.org/tutorial/solver.html).

Eftersom vi använder CPU i stället för GPU, i det här exemplet ska vi ändra hello sista raden till:

    # solver mode: CPU or GPU
    solver_mode: CPU

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-1.png)

Du kan ändra andra rader efter behov.

andra hello-fil (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) definierar hur hello neuron nätverk ser ut, och hello relevanta indata och utdata-fil. Vi behöver också tooupdate hello tooreflect hello utbildning data filplats. Ändra hello efter en del i lenet_memory_train_test.prototxt (du måste toopoint toohello rätt plats specifika tooyour kluster):

- Ändra hello ”file:/Users/mridul/bigml/demodl/mnist_train_lmdb” för ”wasb: / / / projekt/machine_learning/image_dataset/mnist_train_lmdb”
- Ändra ”file:/Users/mridul/bigml/demodl/mnist_test_lmdb/” för ”wasb: / / / projekt/machine_learning/image_dataset/mnist_test_lmdb”

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-2.png)

Mer information om hur toodefine hello nätverket Kontrollera hello [Caffe dokumentation om MNIST dataset](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)

För hello syftet med den här bloggen använda vi bara det här enkla exemplet MNIST. Du bör köra hello-kommandot nedan från hello huvudnod:

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

I princip distribueras hello krävs filer (lenet_memory_solver.prototxt och lenet_memory_train_test.prototxt) tooeach garn behållare, samt även hello relevanta SÖKVÄGEN för varje Spark drivrutinen/utföraren tooLD_LIBRARY_PATH som definieras i hello föregående kod fragment och punkter toohello plats som har CaffeOnSpark bibliotek. 

## <a name="monitoring-and-troubleshooting"></a>Övervakning och felsökning

Eftersom vi använder YARN klustret läget i vilket fall hello Spark drivrutinen kommer att vara schemalagda tooan godtycklig behållaren (och en valfri arbetsnoden) bör du kan bara se i hello konsolen mata ut liknande:

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

Om du vill tooknow vad hände, måste vanligtvis tooget hello Spark-drivrutinens logg som innehåller mer information. I det här fallet måste toogo toohello YARN-Användargränssnittet toofind hello relevanta YARN-loggar. Du kan få hello YARN-Användargränssnittet av denna URL: 

    https://yourclustername.azurehdinsight.net/yarnui
   
![YARN-ANVÄNDARGRÄNSSNITTET](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-1.png)

Du kan ta en titt på hur många resurser för det aktuella programmet. Du kan klicka på hello ”Scheduler” länk och sedan du kommer att för det här programmet, det finns 9 behållare som kör. Vi ber YARN tooprovide 8 executors och en annan behållare för drivrutinen processen. 

![YARN Schemaläggaren](./media/hdinsight-deep-learning-caffe-spark/YARN-Scheduler.png)

Du kanske vill toocheck hello drivrutinsloggar eller behållaren loggar om det finns fel. För drivrutinsloggar du klickar du på hello-ID i YARN-Användargränssnittet och sedan klicka hello ”loggar”. hello drivrutinen loggarna skrivs till stderr.

![YARN-ANVÄNDARGRÄNSSNITTET 2](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-2.png)

T.ex, kan du se några av hello fel under från hello drivrutinsloggar som anger du allokera för många executors.

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

Ibland kan hello problemet inträffa i executors i stället för drivrutiner. I det här fallet måste toocheck hello behållaren loggar. Du kan alltid hämta hello behållaren loggar och få hello misslyckade behållare. Du kan till exempel uppfylla det här felet när du kör Caffe.

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

I det här fallet måste tooget hello misslyckades behållar-ID (i hello senare fallet är det container_1485916338528_0008_05_000005). Måste toorun 

    yarn logs -containerId container_1485916338528_0008_03_000005

från hello headnode. När du har kontrollerat behållaren felet beror det på använder GPU-läge (där du ska använda CPU-läge i stället) i lenet_memory_solver.prototxt.

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written tooSTDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a>Hämtar resultat

Eftersom vi allokera 8 executors och det är enkelt att hello nätverkets topologi, tar bara ungefär 30 minuter toorun hello resultat. Från kommandoraden hello, du kan se att vi placera hello modellen toowasb:///mnist.model och placera hello resultat tooa mapp med namnet wasb: / / / mnist_features_result.

Du kan hämta hello resultat genom att köra

    hadoop fs -cat hdfs:///mnist_features_result/*

och hello resultatet ser ut som:

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

hello SampleID representerar hello-ID i hello MNIST dataset och hello etiketten är hello som hello modellen sidnumret.


## <a name="conclusion"></a>Slutsats

Du har försökt tooinstall CaffeOnSpark med ett enkelt exempel i den här dokumentationen. HDInsight är en fullständigt hanterad distribuerade beräkning molnplattform och hello bästa platsen för att köra machine learning och avancerade analyser arbetsbelastningar på stora datamängder och för distribuerade djup, du kan använda Caffe HDInsight Spark tooperform djup inlärning aktiviteter.


## <a name="seealso"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenarier
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

