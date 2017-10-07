---
title: "Azure Cosmos DB: Utföra analyser av diagram med hjälp av Spark och Apache TinkerPop Gremlin | Microsoft Docs"
description: "Den här artikeln innehåller anvisningar för att installera och köra diagrammet analytics och parallella beräkning i Azure Cosmos-databasen med Spark och TinkerPop SparkGraphComputer."
services: cosmosdb
documentationcenter: 
author: khdang
manager: shireest
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: gremlin
ms.topic: article
ms.date: 06/05/2017
ms.author: khdang
ms.openlocfilehash: 0be5c9b12cdba4a428c809d00e1e68785a9ec1ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-perform-graph-analytics-by-using-spark-and-apache-tinkerpop-gremlin"></a><span data-ttu-id="1d549-103">Azure Cosmos DB: Utföra analyser av diagram med hjälp av Spark och Apache TinkerPop Gremlin</span><span class="sxs-lookup"><span data-stu-id="1d549-103">Azure Cosmos DB: Perform graph analytics by using Spark and Apache TinkerPop Gremlin</span></span>

<span data-ttu-id="1d549-104">[Azure Cosmos-DB](introduction.md) är hello globalt distribuerad databas som flera modellen tjänst från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1d549-104">[Azure Cosmos DB](introduction.md) is hello globally distributed, multi-model database service from Microsoft.</span></span> <span data-ttu-id="1d549-105">Du kan skapa och fråga dokument och nyckel/värde-diagrammet databaser, som dra nytta av hello global distribution och skala horisontellt funktioner på hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1d549-105">You can create and query document, key/value, and graph databases, all of which benefit from hello global-distribution and horizontal-scale capabilities at hello core of Azure Cosmos DB.</span></span> <span data-ttu-id="1d549-106">Har stöd för online transaktionsbearbetning (OLTP) diagrammet arbetsbelastningar som använder Azure Cosmos-DB [Apache TinkerPop Gremlin](graph-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1d549-106">Azure Cosmos DB supports online transaction processing (OLTP) graph workloads that use [Apache TinkerPop Gremlin](graph-introduction.md).</span></span>

<span data-ttu-id="1d549-107">[Spark](http://spark.apache.org/) är ett Apache Software Foundation-projekt som fokuserar på allmänna online analytical processing (OLAP) databearbetning.</span><span class="sxs-lookup"><span data-stu-id="1d549-107">[Spark](http://spark.apache.org/) is an Apache Software Foundation project that's focused on general-purpose online analytical processing (OLAP) data processing.</span></span> <span data-ttu-id="1d549-108">Spark tillhandahåller en hybrid i-minne/diskbaserad distribuerad datamodell som är liknande toohello Hadoop MapReduce-modellen.</span><span class="sxs-lookup"><span data-stu-id="1d549-108">Spark provides a hybrid in-memory/disk-based distributed computing model that is similar toohello Hadoop MapReduce model.</span></span> <span data-ttu-id="1d549-109">Du kan distribuera Apache Spark i hello molnet med hjälp av [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span><span class="sxs-lookup"><span data-stu-id="1d549-109">You can deploy Apache Spark in hello cloud by using [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span></span>

<span data-ttu-id="1d549-110">Du kan utföra både OLTP och OLAP-arbetsbelastningar när du använder Gremlin genom att kombinera Azure Cosmos DB och Spark.</span><span class="sxs-lookup"><span data-stu-id="1d549-110">By combining Azure Cosmos DB and Spark, you can perform both OLTP and OLAP workloads when you use Gremlin.</span></span> <span data-ttu-id="1d549-111">Den här Snabbkurs artikeln visar hur toorun Gremlin frågor mot Azure Cosmos DB på ett Azure HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="1d549-111">This quick-start article demonstrates how toorun Gremlin queries against Azure Cosmos DB on an Azure HDInsight Spark cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d549-112">Krav</span><span class="sxs-lookup"><span data-stu-id="1d549-112">Prerequisites</span></span>

<span data-ttu-id="1d549-113">Innan du kan köra det här exemplet måste du ha hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="1d549-113">Before you can run this sample, you must have hello following prerequisites:</span></span>
* <span data-ttu-id="1d549-114">Azure HDInsight Spark-kluster 2.0</span><span class="sxs-lookup"><span data-stu-id="1d549-114">Azure HDInsight Spark cluster 2.0</span></span>
* <span data-ttu-id="1d549-115">JDK 1.8 + (om du inte har JDK, köra `apt-get install default-jdk`.)</span><span class="sxs-lookup"><span data-stu-id="1d549-115">JDK 1.8+ (If you don't have JDK, run `apt-get install default-jdk`.)</span></span>
* <span data-ttu-id="1d549-116">Maven (om du inte har Maven kör `apt-get install maven`.)</span><span class="sxs-lookup"><span data-stu-id="1d549-116">Maven (If you don't have Maven, run `apt-get install maven`.)</span></span>
* <span data-ttu-id="1d549-117">En Azure-prenumeration ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])</span><span class="sxs-lookup"><span data-stu-id="1d549-117">An Azure subscription ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])</span></span>

<span data-ttu-id="1d549-118">Information om hur tooset upp ett Azure HDInsight Spark-kluster, se [etablera HDInsight-kluster](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="1d549-118">For information about how tooset up an Azure HDInsight Spark cluster, see [Provisioning HDInsight clusters](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="create-an-azure-cosmos-db-database-account"></a><span data-ttu-id="1d549-119">Skapa ett databaskonto Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1d549-119">Create an Azure Cosmos DB database account</span></span>

<span data-ttu-id="1d549-120">Skapa först ett databaskonto med hello Graph API hello följande:</span><span class="sxs-lookup"><span data-stu-id="1d549-120">First, create a database account with hello Graph API by doing hello following:</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="1d549-121">Lägga till en samling</span><span class="sxs-lookup"><span data-stu-id="1d549-121">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="get-apache-tinkerpop"></a><span data-ttu-id="1d549-122">Hämta Apache TinkerPop</span><span class="sxs-lookup"><span data-stu-id="1d549-122">Get Apache TinkerPop</span></span>

<span data-ttu-id="1d549-123">Hämta Apache TinkerPop hello följande:</span><span class="sxs-lookup"><span data-stu-id="1d549-123">Get Apache TinkerPop by doing hello following:</span></span>

1. <span data-ttu-id="1d549-124">Remote toohello huvudnoden hello HDInsight-klustret `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="1d549-124">Remote toohello master node of hello HDInsight cluster `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.</span></span>

2. <span data-ttu-id="1d549-125">Klona hello TinkerPop3 källkoden, skapar det lokalt och installera den tooMaven cache.</span><span class="sxs-lookup"><span data-stu-id="1d549-125">Clone hello TinkerPop3 source code, build it locally, and install it tooMaven cache.</span></span>

    ```bash
    git clone https://github.com/apache/tinkerpop.git
    cd tinkerpop
    mvn clean install
    ```

3. <span data-ttu-id="1d549-126">Installera hello Spark-Gremlin plugin-program</span><span class="sxs-lookup"><span data-stu-id="1d549-126">Install hello Spark-Gremlin plug-in</span></span> 

    <span data-ttu-id="1d549-127">a.</span><span class="sxs-lookup"><span data-stu-id="1d549-127">a.</span></span> <span data-ttu-id="1d549-128">hello-installation av plugin-programmet hello hanteras av delvis.</span><span class="sxs-lookup"><span data-stu-id="1d549-128">hello installation of hello plug-in is handled by Grape.</span></span> <span data-ttu-id="1d549-129">Fyll i hello databaser information för delvis så att den kan hämta plugin-programmet hello och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="1d549-129">Populate hello repositories information for Grape so it can download hello plug-in and its dependencies.</span></span> 

      <span data-ttu-id="1d549-130">Skapa hello delvis konfigurationsfil om det inte finns på `~/.groovy/grapeConfig.xml`.</span><span class="sxs-lookup"><span data-stu-id="1d549-130">Create hello grape configuration file if it's not present at `~/.groovy/grapeConfig.xml`.</span></span> <span data-ttu-id="1d549-131">Använd hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="1d549-131">Use hello following settings:</span></span>

    ```xml
    <ivysettings>
    <settings defaultResolver="downloadGrapes"/>
    <resolvers>
        <chain name="downloadGrapes">
        <filesystem name="cachedGrapes">
            <ivy pattern="${user.home}/.groovy/grapes/[organisation]/[module]/ivy-[revision].xml"/>
            <artifact pattern="${user.home}/.groovy/grapes/[organisation]/[module]/[type]s/[artifact]-[revision].[ext]"/>
        </filesystem>
        <ibiblio name="codehaus" root="http://repository.codehaus.org/" m2compatible="true"/>
        <ibiblio name="central" root="http://central.maven.org/maven2/" m2compatible="true"/>
        <ibiblio name="jitpack" root="https://jitpack.io" m2compatible="true"/>
        <ibiblio name="java.net2" root="http://download.java.net/maven/2/" m2compatible="true"/>
        <ibiblio name="apache-snapshots" root="http://repository.apache.org/snapshots/" m2compatible="true"/>
        <ibiblio name="local" root="file:${user.home}/.m2/repository/" m2compatible="true"/>
        </chain>
    </resolvers>
    </ivysettings>
    ``` 

    <span data-ttu-id="1d549-132">b.</span><span class="sxs-lookup"><span data-stu-id="1d549-132">b.</span></span> <span data-ttu-id="1d549-133">Starta konsolen Gremlin `bin/gremlin.sh`.</span><span class="sxs-lookup"><span data-stu-id="1d549-133">Start Gremlin console `bin/gremlin.sh`.</span></span>
        
    <span data-ttu-id="1d549-134">c.</span><span class="sxs-lookup"><span data-stu-id="1d549-134">c.</span></span> <span data-ttu-id="1d549-135">Installera hello Spark-Gremlin plugin-program med version 3.3.0-SNAPSHOT som du skapat i föregående steg i hello:</span><span class="sxs-lookup"><span data-stu-id="1d549-135">Install hello Spark-Gremlin plug-in with version 3.3.0-SNAPSHOT, which you built in hello previous steps:</span></span>

    ```bash
    $ bin/gremlin.sh

            \,,,/
            (o o)
    -----oOOo-(3)-oOOo-----
    plugin activated: tinkerpop.server
    plugin activated: tinkerpop.utilities
    plugin activated: tinkerpop.tinkergraph
    gremlin> :install org.apache.tinkerpop spark-gremlin 3.3.0-SNAPSHOT
    ==>loaded: [org.apache.tinkerpop, spark-gremlin, 3.3.0-SNAPSHOT] - restart hello console toouse [tinkerpop.spark]
    gremlin> :q
    $ bin/gremlin.sh

            \,,,/
            (o o)
    -----oOOo-(3)-oOOo-----
    plugin activated: tinkerpop.server
    plugin activated: tinkerpop.utilities
    plugin activated: tinkerpop.tinkergraph
    gremlin> :plugin use tinkerpop.spark
    ==>tinkerpop.spark activated
    ```

4. <span data-ttu-id="1d549-136">Kontrollera om toosee `Hadoop-Gremlin` aktiveras med `:plugin list`.</span><span class="sxs-lookup"><span data-stu-id="1d549-136">Check toosee whether `Hadoop-Gremlin` is activated with `:plugin list`.</span></span> <span data-ttu-id="1d549-137">Inaktivera det här plugin-programmet, eftersom det kan störa hello Spark-Gremlin plugin `:plugin unuse tinkerpop.hadoop`.</span><span class="sxs-lookup"><span data-stu-id="1d549-137">Disable this plug-in, because it could interfere with hello Spark-Gremlin plug-in `:plugin unuse tinkerpop.hadoop`.</span></span>

## <a name="prepare-tinkerpop3-dependencies"></a><span data-ttu-id="1d549-138">Förbereda TinkerPop3 beroenden</span><span class="sxs-lookup"><span data-stu-id="1d549-138">Prepare TinkerPop3 dependencies</span></span>

<span data-ttu-id="1d549-139">När du har skapat TinkerPop3 i föregående steg i hello hämtas hello processen alla jar-beroenden även för Spark och Hadoop i hello målkatalogen.</span><span class="sxs-lookup"><span data-stu-id="1d549-139">When you built TinkerPop3 in hello previous step, hello process also pulled all jar dependencies for Spark and Hadoop in hello target directory.</span></span> <span data-ttu-id="1d549-140">Använd hello burkar som är förinstallerade med HDI och ta emot ytterligare beroenden bara vid behov.</span><span class="sxs-lookup"><span data-stu-id="1d549-140">Use hello jars that are pre-installed with HDI, and pull in additional dependencies only as necessary.</span></span>

1. <span data-ttu-id="1d549-141">Gå toohello Gremlin konsolen målkatalogen på `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`.</span><span class="sxs-lookup"><span data-stu-id="1d549-141">Go toohello Gremlin Console target directory at `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`.</span></span> 

2. <span data-ttu-id="1d549-142">Flytta alla burkar under `ext/` för`lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.</span><span class="sxs-lookup"><span data-stu-id="1d549-142">Move all jars under `ext/` too`lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.</span></span>

3. <span data-ttu-id="1d549-143">Ta bort alla jar bibliotek under `lib/` att är inte i hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="1d549-143">Remove all jar libraries under `lib/` that are not in hello following list:</span></span>

    ```bash
    # TinkerPop3
    gremlin-console-3.3.0-SNAPSHOT.jar
    gremlin-core-3.3.0-SNAPSHOT.jar       
    gremlin-groovy-3.3.0-SNAPSHOT.jar     
    gremlin-shaded-3.3.0-SNAPSHOT.jar     
    hadoop-gremlin-3.3.0-SNAPSHOT.jar     
    spark-gremlin-3.3.0-SNAPSHOT.jar      
    tinkergraph-gremlin-3.3.0-SNAPSHOT.jar

    # Gremlin depedencies
    asm-3.2.jar                                
    avro-1.7.4.jar                             
    caffeine-2.3.1.jar                         
    cglib-2.2.1-v20090111.jar                  
    gbench-0.4.3-groovy-2.4.jar                
    gprof-0.3.1-groovy-2.4.jar                 
    groovy-2.4.9-indy.jar                      
    groovy-2.4.9.jar                           
    groovy-console-2.4.9.jar                   
    groovy-groovysh-2.4.9-indy.jar             
    groovy-json-2.4.9-indy.jar                 
    groovy-jsr223-2.4.9-indy.jar               
    groovy-sql-2.4.9-indy.jar                  
    groovy-swing-2.4.9.jar                     
    groovy-templates-2.4.9.jar                 
    groovy-xml-2.4.9.jar                       
    hadoop-yarn-server-nodemanager-2.7.2.jar   
    hppc-0.7.1.jar                             
    javatuples-1.2.jar                         
    jaxb-impl-2.2.3-1.jar                      
    jbcrypt-0.4.jar                            
    jcabi-log-0.14.jar                         
    jcabi-manifests-1.1.jar                    
    jersey-core-1.9.jar                        
    jersey-guice-1.9.jar                       
    jersey-json-1.9.jar                        
    jettison-1.1.jar                           
    scalatest_2.11-2.2.6.jar                   
    servlet-api-2.5.jar                        
    snakeyaml-1.15.jar                         
    unused-1.0.0.jar                           
    xml-apis-1.3.04.jar                        
    ```

## <a name="get-hello-azure-cosmos-db-spark-connector"></a><span data-ttu-id="1d549-144">Hämta hello Azure Cosmos DB Spark-koppling</span><span class="sxs-lookup"><span data-stu-id="1d549-144">Get hello Azure Cosmos DB Spark connector</span></span>

1. <span data-ttu-id="1d549-145">Hämta hello Azure Cosmos DB Spark connector `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` och Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` från [Cosmos Azure DB Spark Connector på GitHub](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).</span><span class="sxs-lookup"><span data-stu-id="1d549-145">Get hello Azure Cosmos DB Spark connector `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` and Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` from [Azure Cosmos DB Spark Connector on GitHub](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).</span></span>

2. <span data-ttu-id="1d549-146">Alternativt kan du skapa den lokalt.</span><span class="sxs-lookup"><span data-stu-id="1d549-146">Alternatively, you can build it locally.</span></span> <span data-ttu-id="1d549-147">Eftersom hello senaste versionen av Spark-Gremlin har skapats med Spark 1.6.1 och är inte kompatibel med Spark punkt 2.0.2, som används för närvarande i hello Azure Cosmos DB Spark-koppling, kan du skapa hello senaste TinkerPop3 kod och installera hello burkar manuellt.</span><span class="sxs-lookup"><span data-stu-id="1d549-147">Because hello latest version of Spark-Gremlin was built with Spark 1.6.1 and is not compatible with Spark 2.0.2, which is currently used in hello Azure Cosmos DB Spark connector, you can build hello latest TinkerPop3 code and install hello jars manually.</span></span> <span data-ttu-id="1d549-148">Hej du följande:</span><span class="sxs-lookup"><span data-stu-id="1d549-148">Do hello following:</span></span>

    <span data-ttu-id="1d549-149">a.</span><span class="sxs-lookup"><span data-stu-id="1d549-149">a.</span></span> <span data-ttu-id="1d549-150">Klona hello Azure Cosmos DB Spark-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="1d549-150">Clone hello Azure Cosmos DB Spark connector.</span></span>

    <span data-ttu-id="1d549-151">b.</span><span class="sxs-lookup"><span data-stu-id="1d549-151">b.</span></span> <span data-ttu-id="1d549-152">Skapa TinkerPop3 (redan har gjort i föregående steg).</span><span class="sxs-lookup"><span data-stu-id="1d549-152">Build TinkerPop3 (already done in previous steps).</span></span> <span data-ttu-id="1d549-153">Installera alla TinkerPop 3.3.0-SNAPSHOT burkar lokalt.</span><span class="sxs-lookup"><span data-stu-id="1d549-153">Install all TinkerPop 3.3.0-SNAPSHOT jars locally.</span></span>

    ```bash
    mvn install:install-file -Dfile="gremlin-core-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-core -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar
    mvn install:install-file -Dfile="gremlin-groovy-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-groovy -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="gremlin-shaded-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-shaded -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="hadoop-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=hadoop-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="spark-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=spark-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="tinkergraph-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=tinkergraph-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    ```

    <span data-ttu-id="1d549-154">c.</span><span class="sxs-lookup"><span data-stu-id="1d549-154">c.</span></span> <span data-ttu-id="1d549-155">Uppdatera `tinkerpop.version` `azure-documentdb-spark/pom.xml` för`3.3.0-SNAPSHOT`.</span><span class="sxs-lookup"><span data-stu-id="1d549-155">Update `tinkerpop.version` `azure-documentdb-spark/pom.xml` too`3.3.0-SNAPSHOT`.</span></span>
    
    <span data-ttu-id="1d549-156">d.</span><span class="sxs-lookup"><span data-stu-id="1d549-156">d.</span></span> <span data-ttu-id="1d549-157">Skapa med Maven.</span><span class="sxs-lookup"><span data-stu-id="1d549-157">Build with Maven.</span></span> <span data-ttu-id="1d549-158">hello nödvändiga burkar placeras i `target` och `target/alternateLocation`.</span><span class="sxs-lookup"><span data-stu-id="1d549-158">hello needed jars are placed in `target` and `target/alternateLocation`.</span></span>

    ```bash
    git clone https://github.com/Azure/azure-cosmosdb-spark.git
    cd azure-documentdb-spark
    mvn clean package
    ```

3. <span data-ttu-id="1d549-159">Kopiera hello tidigare nämnts burkar tooa lokal katalog på ~ / azure-documentdb-spark:</span><span class="sxs-lookup"><span data-stu-id="1d549-159">Copy hello previously mentioned jars tooa local directory at ~/azure-documentdb-spark:</span></span>

    ```bash
    $ azure-documentdb-spark:
    mkdir ~/azure-documentdb-spark
    cp target/azure-documentdb-spark-0.0.3-SNAPSHOT.jar ~/azure-documentdb-spark
    cp target/alternateLocation/azure-documentdb-1.10.0.jar ~/azure-documentdb-spark
    ```

## <a name="distribute-hello-dependencies-toohello-spark-worker-nodes"></a><span data-ttu-id="1d549-160">Distribuera hello beroenden toohello Spark arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="1d549-160">Distribute hello dependencies toohello Spark worker nodes</span></span> 

1. <span data-ttu-id="1d549-161">Eftersom hello transformering av diagramdata beror på TinkerPop3, måste du distribuera hello relaterade beroenden tooall Spark arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="1d549-161">Because hello transformation of graph data depends on TinkerPop3, you must distribute hello related dependencies tooall Spark worker nodes.</span></span>

2. <span data-ttu-id="1d549-162">Kopiera hello tidigare nämnts Gremlin beroenden, hello CosmosDB Spark connector jar och CosmosDB Java SDK toohello arbetarnoder hello följande:</span><span class="sxs-lookup"><span data-stu-id="1d549-162">Copy hello previously mentioned Gremlin dependencies, hello CosmosDB Spark connector jar, and CosmosDB Java SDK toohello worker nodes by doing hello following:</span></span>

    <span data-ttu-id="1d549-163">a.</span><span class="sxs-lookup"><span data-stu-id="1d549-163">a.</span></span> <span data-ttu-id="1d549-164">Kopiera alla hello burkar i `~/azure-documentdb-spark`.</span><span class="sxs-lookup"><span data-stu-id="1d549-164">Copy all hello jars into `~/azure-documentdb-spark`.</span></span>

    ```bash
    $ /home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone:
    cp lib/* ~/azure-documentdb-spark
    ```

    <span data-ttu-id="1d549-165">b.</span><span class="sxs-lookup"><span data-stu-id="1d549-165">b.</span></span> <span data-ttu-id="1d549-166">Hämta hello lista över alla Spark arbetarnoder, som du hittar på Ambari instrumentpanelen i hello `Spark2 Clients` listan i hello `Spark2` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1d549-166">Get hello list of all Spark worker nodes, which you can find on Ambari Dashboard, in hello `Spark2 Clients` list in hello `Spark2` section.</span></span>

    <span data-ttu-id="1d549-167">c.</span><span class="sxs-lookup"><span data-stu-id="1d549-167">c.</span></span> <span data-ttu-id="1d549-168">Kopiera den directory tooeach hello noder.</span><span class="sxs-lookup"><span data-stu-id="1d549-168">Copy that directory tooeach of hello nodes.</span></span>

    ```bash
    scp -r ~/azure-documentdb-spark sshuser@wn0-cosmos:/home/sshuser
    scp -r ~/azure-documentdb-spark sshuser@wn1-cosmos:/home/sshuser
    ...
    ```
    
## <a name="set-up-hello-environment-variables"></a><span data-ttu-id="1d549-169">Ställ in hello miljövariabler</span><span class="sxs-lookup"><span data-stu-id="1d549-169">Set up hello environment variables</span></span>

1. <span data-ttu-id="1d549-170">Hitta hello HDP version av hello Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="1d549-170">Find hello HDP version of hello Spark cluster.</span></span> <span data-ttu-id="1d549-171">Det är hello katalognamnet under `/usr/hdp/` (till exempel 2.5.4.2-7).</span><span class="sxs-lookup"><span data-stu-id="1d549-171">It is hello directory name under `/usr/hdp/` (for example, 2.5.4.2-7).</span></span>

2. <span data-ttu-id="1d549-172">Ange hdp.version för alla noder.</span><span class="sxs-lookup"><span data-stu-id="1d549-172">Set hdp.version for all nodes.</span></span> <span data-ttu-id="1d549-173">I instrumentpanelen för Ambari gå för**YARN avsnittet** > **konfigurationerna** > **Avancerat**, och sedan hello följande:</span><span class="sxs-lookup"><span data-stu-id="1d549-173">In Ambari Dashboard, go too**YARN section** > **Configs** > **Advanced**, and then do hello following:</span></span> 
 
    <span data-ttu-id="1d549-174">a.</span><span class="sxs-lookup"><span data-stu-id="1d549-174">a.</span></span> <span data-ttu-id="1d549-175">I `Custom yarn-site`, lägga till en ny egenskap `hdp.version` med hello värdet hello HDP versionen på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="1d549-175">In `Custom yarn-site`, add a new property `hdp.version` with hello value of hello HDP version on hello master node.</span></span> 
     
    <span data-ttu-id="1d549-176">b.</span><span class="sxs-lookup"><span data-stu-id="1d549-176">b.</span></span> <span data-ttu-id="1d549-177">Spara hello konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="1d549-177">Save hello configurations.</span></span> <span data-ttu-id="1d549-178">Det finns varningar som du kan ignorera.</span><span class="sxs-lookup"><span data-stu-id="1d549-178">There are warnings, which you can ignore.</span></span> 
     
    <span data-ttu-id="1d549-179">c.</span><span class="sxs-lookup"><span data-stu-id="1d549-179">c.</span></span> <span data-ttu-id="1d549-180">Starta om tjänsterna för hello YARN och Oozie som hello notification ikoner anger.</span><span class="sxs-lookup"><span data-stu-id="1d549-180">Restart hello YARN and Oozie services as hello notification icons indicate.</span></span>

3. <span data-ttu-id="1d549-181">Ange hello följa miljövariabler på hello huvudnod (Ersätt hello-värden efter behov):</span><span class="sxs-lookup"><span data-stu-id="1d549-181">Set hello following environment variables on hello master node (replace hello values as appropriate):</span></span>

    ```bash
    export HADOOP_GREMLIN_LIBS=/home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/ext/spark-gremlin/lib
    export CLASSPATH=$CLASSPATH:$HADOOP_CONF_DIR:/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    export HDP_VERSION=2.5.4.2-7
    export HADOOP_HOME=${HADOOP_HOME:-/usr/hdp/current/hadoop-client}
    ```

## <a name="prepare-hello-graph-configuration"></a><span data-ttu-id="1d549-182">Förbereda hello graph-konfiguration</span><span class="sxs-lookup"><span data-stu-id="1d549-182">Prepare hello graph configuration</span></span>

1. <span data-ttu-id="1d549-183">Skapa en konfigurationsfil med hello Azure Cosmos DB anslutningsparametrar Väck inställningar och placera den på `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.</span><span class="sxs-lookup"><span data-stu-id="1d549-183">Create a configuration file with hello Azure Cosmos DB connection parameters and Spark settings, and put it at `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.</span></span>

    ```
    gremlin.graph=org.apache.tinkerpop.gremlin.hadoop.structure.HadoopGraph
    gremlin.hadoop.jarsInDistributedCache=true
    gremlin.hadoop.defaultGraphComputer=org.apache.tinkerpop.gremlin.spark.process.computer.SparkGraphComputer

    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    gremlin.hadoop.graphWriter=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBOutputRDD

    ####################################
    # SparkGraphComputer Configuration #
    ####################################
    spark.master=yarn
    spark.executor.memory=3g
    spark.executor.instances=6
    spark.serializer=org.apache.spark.serializer.KryoSerializer
    spark.kryo.registrator=org.apache.tinkerpop.gremlin.spark.structure.io.gryo.GryoRegistrator
    gremlin.spark.persistContext=true

    # Classpath for hello driver and executors
    spark.driver.extraClassPath=/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    spark.executor.extraClassPath=/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    
    ######################################
    # DocumentDB Spark connector         #
    ######################################
    spark.documentdb.connectionMode=Gateway
    spark.documentdb.schema_samplingratio=1.0
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    spark.documentdb.preferredRegions=FILLIN
    ```

2. <span data-ttu-id="1d549-184">Uppdatera hello `spark.driver.extraClassPath` och `spark.executor.extraClassPath` tooinclude hello-katalogen för hello burkar som du distribuerat i hello föregående steg i det här fallet `/home/sshuser/azure-documentdb-spark/*`.</span><span class="sxs-lookup"><span data-stu-id="1d549-184">Update hello `spark.driver.extraClassPath` and `spark.executor.extraClassPath` tooinclude hello directory of hello jars that you distributed in hello previous step, in this case `/home/sshuser/azure-documentdb-spark/*`.</span></span>

3. <span data-ttu-id="1d549-185">Ange följande information för Azure Cosmos DB hello:</span><span class="sxs-lookup"><span data-stu-id="1d549-185">Provide hello following details for Azure Cosmos DB:</span></span>

    ```
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    # Optional
    #spark.documentdb.preferredRegions=West\ US;West\ US\ 2
    ```
   
## <a name="load-hello-tinkerpop-graph-and-save-it-tooazure-cosmos-db"></a><span data-ttu-id="1d549-186">Läsa in hello TinkerPop diagram och spara den tooAzure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1d549-186">Load hello TinkerPop graph, and save it tooAzure Cosmos DB</span></span>
<span data-ttu-id="1d549-187">toodemonstrate hur toopersist ett diagram i Azure Cosmos databas med det här exemplet använder hello TinkerPop fördefinierade TinkerPop moderna diagram.</span><span class="sxs-lookup"><span data-stu-id="1d549-187">toodemonstrate how toopersist a graph into Azure Cosmos DB, this example uses hello TinkerPop predefined TinkerPop modern graph.</span></span> <span data-ttu-id="1d549-188">hello diagram lagras i Kryo format och anges i hello TinkerPop databas.</span><span class="sxs-lookup"><span data-stu-id="1d549-188">hello graph is stored in Kryo format, and it's provided in hello TinkerPop repository.</span></span>

1. <span data-ttu-id="1d549-189">Eftersom du kör Gremlin i YARN-läge måste du hello diagramdata i hello Hadoop-filsystem.</span><span class="sxs-lookup"><span data-stu-id="1d549-189">Because you are running Gremlin in YARN mode, you must make hello graph data available in hello Hadoop file system.</span></span> <span data-ttu-id="1d549-190">Använd hello följande kommandon toomake en katalog och kopiera hello lokala diagrammet fil till den.</span><span class="sxs-lookup"><span data-stu-id="1d549-190">Use hello following commands toomake a directory and copy hello local graph file into it.</span></span> 

    ```bash
    $ tinkerpop:
    hadoop fs -mkdir /graphData
    hadoop fs -copyFromLocal ~/tinkerpop/data/tinkerpop-modern.kryo /graphData/tinkerpop-modern.kryo
    ```

2. <span data-ttu-id="1d549-191">Uppdatera tillfälligt hello `gremlin-spark.properties` filen toouse `GryoInputFormat` tooread hello diagram.</span><span class="sxs-lookup"><span data-stu-id="1d549-191">Temporarily update hello `gremlin-spark.properties` file toouse `GryoInputFormat` tooread hello graph.</span></span> <span data-ttu-id="1d549-192">Ange också `inputLocation` som hello katalog skapas som hello följande:</span><span class="sxs-lookup"><span data-stu-id="1d549-192">Also indicate `inputLocation` as hello directory you create, as in hello following:</span></span>

    ```
    gremlin.hadoop.graphReader=org.apache.tinkerpop.gremlin.hadoop.structure.io.gryo.GryoInputFormat
    gremlin.hadoop.inputLocation=/graphData/tinkerpop-modern.kryo
    ```

3. <span data-ttu-id="1d549-193">Starta Gremlin konsolen och sedan skapa hello följande beräkning steg toopersist toohello konfigurerade Azure Cosmos DB datainsamling:</span><span class="sxs-lookup"><span data-stu-id="1d549-193">Start Gremlin Console, and then create hello following computation steps toopersist data toohello configured Azure Cosmos DB collection:</span></span>  

    <span data-ttu-id="1d549-194">a.</span><span class="sxs-lookup"><span data-stu-id="1d549-194">a.</span></span> <span data-ttu-id="1d549-195">Skapa hello diagram `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.</span><span class="sxs-lookup"><span data-stu-id="1d549-195">Create hello graph `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.</span></span>

    <span data-ttu-id="1d549-196">b.</span><span class="sxs-lookup"><span data-stu-id="1d549-196">b.</span></span> <span data-ttu-id="1d549-197">Använd SparkGraphComputer för skrivning `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.</span><span class="sxs-lookup"><span data-stu-id="1d549-197">Use SparkGraphComputer for writing `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.</span></span>

    ```bash
    gremlin> graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")
    ==>hadoopgraph[gryoinputformat->documentdboutputrdd]
    gremlin> hg = graph.
                compute(SparkGraphComputer.class).
                result(GraphComputer.ResultGraph.NEW).
                persist(GraphComputer.Persist.EDGES).
                program(TraversalVertexProgram.build().
                    traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)), "gremlin-groovy", "g.V()").
                    create(graph)).
                submit().
                get() 
    ==>result[hadoopgraph[documentdbinputrdd->documentdboutputrdd],memory[size:1]]
    ```

4. <span data-ttu-id="1d549-198">Du kan verifiera att data har hello beständiga tooAzure Cosmos DB från Data Explorer.</span><span class="sxs-lookup"><span data-stu-id="1d549-198">From Data Explorer, you can verify that hello data has been persisted tooAzure Cosmos DB.</span></span>

## <a name="load-hello-graph-from-azure-cosmos-db-and-run-gremlin-queries"></a><span data-ttu-id="1d549-199">Läsa in hello diagrammet från Azure Cosmos DB och köra Gremlin frågor</span><span class="sxs-lookup"><span data-stu-id="1d549-199">Load hello graph from Azure Cosmos DB, and run Gremlin queries</span></span>

1. <span data-ttu-id="1d549-200">tooload hello diagrammet redigera `gremlin-spark.properties` tooset `graphReader` för`DocumentDBInputRDD`:</span><span class="sxs-lookup"><span data-stu-id="1d549-200">tooload hello graph, edit `gremlin-spark.properties` tooset `graphReader` too`DocumentDBInputRDD`:</span></span>

    ```
    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    ```

2. <span data-ttu-id="1d549-201">Läsa in hello diagram, gå igenom hello data och kör Gremlin frågor med den hello följande:</span><span class="sxs-lookup"><span data-stu-id="1d549-201">Load hello graph, traverse hello data, and run Gremlin queries with it by doing hello following:</span></span>

    <span data-ttu-id="1d549-202">a.</span><span class="sxs-lookup"><span data-stu-id="1d549-202">a.</span></span> <span data-ttu-id="1d549-203">Starta hello Gremlin konsolen `bin/gremlin.sh`.</span><span class="sxs-lookup"><span data-stu-id="1d549-203">Start hello Gremlin Console `bin/gremlin.sh`.</span></span>

    <span data-ttu-id="1d549-204">b.</span><span class="sxs-lookup"><span data-stu-id="1d549-204">b.</span></span> <span data-ttu-id="1d549-205">Skapa hello diagram med hello configuration `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.</span><span class="sxs-lookup"><span data-stu-id="1d549-205">Create hello graph with hello configuration `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.</span></span>

    <span data-ttu-id="1d549-206">c.</span><span class="sxs-lookup"><span data-stu-id="1d549-206">c.</span></span> <span data-ttu-id="1d549-207">Skapa ett diagram traversal med SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.</span><span class="sxs-lookup"><span data-stu-id="1d549-207">Create a graph traversal with SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.</span></span>

    <span data-ttu-id="1d549-208">d.</span><span class="sxs-lookup"><span data-stu-id="1d549-208">d.</span></span> <span data-ttu-id="1d549-209">Kör hello följande Gremlin diagram frågor:</span><span class="sxs-lookup"><span data-stu-id="1d549-209">Run hello following Gremlin graph queries:</span></span>

    ```bash
    gremlin> graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")
    ==>hadoopgraph[documentdbinputrdd->documentdboutputrdd]
    gremlin> g = graph.traversal().withComputer(SparkGraphComputer)
    ==>graphtraversalsource[hadoopgraph[documentdbinputrdd->documentdboutputrdd], sparkgraphcomputer]
    gremlin> g.V().count()
    ==>6
    gremlin> g.E().count()
    ==>6
    gremlin> g.V(1).out().values('name')
    ==>josh
    ==>vadas
    ==>lop
    gremlin> g.V().hasLabel('person').coalesce(values('nickname'), values('name'))
    ==>josh
    ==>peter
    ==>vadas
    ==>marko
    gremlin> g.V().hasLabel('person').
            choose(values('name')).
                option('marko', values('age')).
                option('josh', values('name')).
                option('vadas', valueMap()).
                option('peter', label())
    ==>josh
    ==>person
    ==>[name:[vadas],age:[27]]
    ==>29
    ```

> [!NOTE]
> <span data-ttu-id="1d549-210">toosee mer detaljerad loggning som hello loggningsnivån i `conf/log4j-console.properties` tooa mer detaljerad nivå.</span><span class="sxs-lookup"><span data-stu-id="1d549-210">toosee more detailed logging, set hello log level in `conf/log4j-console.properties` tooa more verbose level.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="1d549-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d549-211">Next steps</span></span>

<span data-ttu-id="1d549-212">I den här Snabbkurs artikeln har du lärt dig hur toowork med diagram genom att kombinera Azure Cosmos DB och Spark.</span><span class="sxs-lookup"><span data-stu-id="1d549-212">In this quick-start article, you've learned how toowork with graphs by combining Azure Cosmos DB and Spark.</span></span>

> [!div class="nextstepaction"]
