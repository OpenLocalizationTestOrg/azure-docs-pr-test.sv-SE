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
ms.openlocfilehash: 27c4d945e418b130c68cfde845571eb93658101e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-perform-graph-analytics-by-using-spark-and-apache-tinkerpop-gremlin"></a><span data-ttu-id="b2afe-103">Azure Cosmos DB: Utföra analyser av diagram med hjälp av Spark och Apache TinkerPop Gremlin</span><span class="sxs-lookup"><span data-stu-id="b2afe-103">Azure Cosmos DB: Perform graph analytics by using Spark and Apache TinkerPop Gremlin</span></span>

<span data-ttu-id="b2afe-104">[Azure Cosmos-DB](introduction.md) är global och flera olika modeller databastjänsten från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b2afe-104">[Azure Cosmos DB](introduction.md) is the globally distributed, multi-model database service from Microsoft.</span></span> <span data-ttu-id="b2afe-105">Du kan skapa och fråga dokument och nyckel/värde-diagrammet databaser, som dra nytta av funktioner för global distributionsplatsen och skala horisontellt kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b2afe-105">You can create and query document, key/value, and graph databases, all of which benefit from the global-distribution and horizontal-scale capabilities at the core of Azure Cosmos DB.</span></span> <span data-ttu-id="b2afe-106">Har stöd för online transaktionsbearbetning (OLTP) diagrammet arbetsbelastningar som använder Azure Cosmos-DB [Apache TinkerPop Gremlin](graph-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b2afe-106">Azure Cosmos DB supports online transaction processing (OLTP) graph workloads that use [Apache TinkerPop Gremlin](graph-introduction.md).</span></span>

<span data-ttu-id="b2afe-107">[Spark](http://spark.apache.org/) är ett Apache Software Foundation-projekt som fokuserar på allmänna online analytical processing (OLAP) databearbetning.</span><span class="sxs-lookup"><span data-stu-id="b2afe-107">[Spark](http://spark.apache.org/) is an Apache Software Foundation project that's focused on general-purpose online analytical processing (OLAP) data processing.</span></span> <span data-ttu-id="b2afe-108">Spark tillhandahåller en hybrid i-minne/diskbaserad distribuerad datamodell som liknar Hadoop MapReduce-modellen.</span><span class="sxs-lookup"><span data-stu-id="b2afe-108">Spark provides a hybrid in-memory/disk-based distributed computing model that is similar to the Hadoop MapReduce model.</span></span> <span data-ttu-id="b2afe-109">Du kan distribuera Apache Spark i molnet med hjälp av [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span><span class="sxs-lookup"><span data-stu-id="b2afe-109">You can deploy Apache Spark in the cloud by using [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span></span>

<span data-ttu-id="b2afe-110">Du kan utföra både OLTP och OLAP-arbetsbelastningar när du använder Gremlin genom att kombinera Azure Cosmos DB och Spark.</span><span class="sxs-lookup"><span data-stu-id="b2afe-110">By combining Azure Cosmos DB and Spark, you can perform both OLTP and OLAP workloads when you use Gremlin.</span></span> <span data-ttu-id="b2afe-111">Den här Snabbkurs artikeln visar hur du kör Gremlin frågor mot Azure Cosmos DB på ett Azure HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="b2afe-111">This quick-start article demonstrates how to run Gremlin queries against Azure Cosmos DB on an Azure HDInsight Spark cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b2afe-112">Krav</span><span class="sxs-lookup"><span data-stu-id="b2afe-112">Prerequisites</span></span>

<span data-ttu-id="b2afe-113">Innan du kan köra det här exemplet måste du uppfylla följande krav:</span><span class="sxs-lookup"><span data-stu-id="b2afe-113">Before you can run this sample, you must have the following prerequisites:</span></span>
* <span data-ttu-id="b2afe-114">Azure HDInsight Spark-kluster 2.0</span><span class="sxs-lookup"><span data-stu-id="b2afe-114">Azure HDInsight Spark cluster 2.0</span></span>
* <span data-ttu-id="b2afe-115">JDK 1.8 + (om du inte har JDK, köra `apt-get install default-jdk`.)</span><span class="sxs-lookup"><span data-stu-id="b2afe-115">JDK 1.8+ (If you don't have JDK, run `apt-get install default-jdk`.)</span></span>
* <span data-ttu-id="b2afe-116">Maven (om du inte har Maven kör `apt-get install maven`.)</span><span class="sxs-lookup"><span data-stu-id="b2afe-116">Maven (If you don't have Maven, run `apt-get install maven`.)</span></span>
* <span data-ttu-id="b2afe-117">En Azure-prenumeration ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])</span><span class="sxs-lookup"><span data-stu-id="b2afe-117">An Azure subscription ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])</span></span>

<span data-ttu-id="b2afe-118">Information om hur du ställer in ett Azure HDInsight Spark-kluster finns [etablera HDInsight-kluster](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="b2afe-118">For information about how to set up an Azure HDInsight Spark cluster, see [Provisioning HDInsight clusters](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="create-an-azure-cosmos-db-database-account"></a><span data-ttu-id="b2afe-119">Skapa ett databaskonto Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b2afe-119">Create an Azure Cosmos DB database account</span></span>

<span data-ttu-id="b2afe-120">Skapa först ett databaskonto med Graph API genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="b2afe-120">First, create a database account with the Graph API by doing the following:</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="b2afe-121">Lägga till en samling</span><span class="sxs-lookup"><span data-stu-id="b2afe-121">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="get-apache-tinkerpop"></a><span data-ttu-id="b2afe-122">Hämta Apache TinkerPop</span><span class="sxs-lookup"><span data-stu-id="b2afe-122">Get Apache TinkerPop</span></span>

<span data-ttu-id="b2afe-123">Hämta Apache TinkerPop genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="b2afe-123">Get Apache TinkerPop by doing the following:</span></span>

1. <span data-ttu-id="b2afe-124">Fjärråtkomst till huvudnoden för HDInsight-klustret `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-124">Remote to the master node of the HDInsight cluster `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.</span></span>

2. <span data-ttu-id="b2afe-125">Klona TinkerPop3 källkoden, skapar det lokalt och installerar Maven cacheminne.</span><span class="sxs-lookup"><span data-stu-id="b2afe-125">Clone the TinkerPop3 source code, build it locally, and install it to Maven cache.</span></span>

    ```bash
    git clone https://github.com/apache/tinkerpop.git
    cd tinkerpop
    mvn clean install
    ```

3. <span data-ttu-id="b2afe-126">Installera Spark-Gremlin plugin-program</span><span class="sxs-lookup"><span data-stu-id="b2afe-126">Install the Spark-Gremlin plug-in</span></span> 

    <span data-ttu-id="b2afe-127">a.</span><span class="sxs-lookup"><span data-stu-id="b2afe-127">a.</span></span> <span data-ttu-id="b2afe-128">Installationen av plugin-programmet hanteras av delvis.</span><span class="sxs-lookup"><span data-stu-id="b2afe-128">The installation of the plug-in is handled by Grape.</span></span> <span data-ttu-id="b2afe-129">Fyll i informationen om databaser för delvis så att den kan hämta plugin-programmet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="b2afe-129">Populate the repositories information for Grape so it can download the plug-in and its dependencies.</span></span> 

      <span data-ttu-id="b2afe-130">Skapa delvis konfigurationsfil om det inte finns på `~/.groovy/grapeConfig.xml`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-130">Create the grape configuration file if it's not present at `~/.groovy/grapeConfig.xml`.</span></span> <span data-ttu-id="b2afe-131">Använd följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="b2afe-131">Use the following settings:</span></span>

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

    <span data-ttu-id="b2afe-132">b.</span><span class="sxs-lookup"><span data-stu-id="b2afe-132">b.</span></span> <span data-ttu-id="b2afe-133">Starta konsolen Gremlin `bin/gremlin.sh`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-133">Start Gremlin console `bin/gremlin.sh`.</span></span>
        
    <span data-ttu-id="b2afe-134">c.</span><span class="sxs-lookup"><span data-stu-id="b2afe-134">c.</span></span> <span data-ttu-id="b2afe-135">Installera Spark-Gremlin plugin-program med version 3.3.0-SNAPSHOT som du skapat i föregående steg:</span><span class="sxs-lookup"><span data-stu-id="b2afe-135">Install the Spark-Gremlin plug-in with version 3.3.0-SNAPSHOT, which you built in the previous steps:</span></span>

    ```bash
    $ bin/gremlin.sh

            \,,,/
            (o o)
    -----oOOo-(3)-oOOo-----
    plugin activated: tinkerpop.server
    plugin activated: tinkerpop.utilities
    plugin activated: tinkerpop.tinkergraph
    gremlin> :install org.apache.tinkerpop spark-gremlin 3.3.0-SNAPSHOT
    ==>loaded: [org.apache.tinkerpop, spark-gremlin, 3.3.0-SNAPSHOT] - restart the console to use [tinkerpop.spark]
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

4. <span data-ttu-id="b2afe-136">Kontrollera om `Hadoop-Gremlin` aktiveras med `:plugin list`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-136">Check to see whether `Hadoop-Gremlin` is activated with `:plugin list`.</span></span> <span data-ttu-id="b2afe-137">Inaktivera det här plugin-programmet, eftersom det kan störa Spark-Gremlin plugin `:plugin unuse tinkerpop.hadoop`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-137">Disable this plug-in, because it could interfere with the Spark-Gremlin plug-in `:plugin unuse tinkerpop.hadoop`.</span></span>

## <a name="prepare-tinkerpop3-dependencies"></a><span data-ttu-id="b2afe-138">Förbereda TinkerPop3 beroenden</span><span class="sxs-lookup"><span data-stu-id="b2afe-138">Prepare TinkerPop3 dependencies</span></span>

<span data-ttu-id="b2afe-139">När du har skapat TinkerPop3 i föregående steg hämtas i processen alla jar-beroenden även för Spark och Hadoop i målkatalogen.</span><span class="sxs-lookup"><span data-stu-id="b2afe-139">When you built TinkerPop3 in the previous step, the process also pulled all jar dependencies for Spark and Hadoop in the target directory.</span></span> <span data-ttu-id="b2afe-140">Använd burkar som är förinstallerade med HDI och ta emot ytterligare beroenden bara vid behov.</span><span class="sxs-lookup"><span data-stu-id="b2afe-140">Use the jars that are pre-installed with HDI, and pull in additional dependencies only as necessary.</span></span>

1. <span data-ttu-id="b2afe-141">Gå till Gremlin konsolen målkatalogen på `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-141">Go to the Gremlin Console target directory at `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`.</span></span> 

2. <span data-ttu-id="b2afe-142">Flytta alla burkar under `ext/` till `lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-142">Move all jars under `ext/` to `lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.</span></span>

3. <span data-ttu-id="b2afe-143">Ta bort alla jar bibliotek under `lib/` som inte är i följande lista:</span><span class="sxs-lookup"><span data-stu-id="b2afe-143">Remove all jar libraries under `lib/` that are not in the following list:</span></span>

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

## <a name="get-the-azure-cosmos-db-spark-connector"></a><span data-ttu-id="b2afe-144">Hämta Azure Cosmos DB Spark-koppling</span><span class="sxs-lookup"><span data-stu-id="b2afe-144">Get the Azure Cosmos DB Spark connector</span></span>

1. <span data-ttu-id="b2afe-145">Hämta Azure Cosmos DB Spark kopplingen `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` och Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` från [Cosmos Azure DB Spark Connector på GitHub](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).</span><span class="sxs-lookup"><span data-stu-id="b2afe-145">Get the Azure Cosmos DB Spark connector `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` and Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` from [Azure Cosmos DB Spark Connector on GitHub](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).</span></span>

2. <span data-ttu-id="b2afe-146">Alternativt kan du skapa den lokalt.</span><span class="sxs-lookup"><span data-stu-id="b2afe-146">Alternatively, you can build it locally.</span></span> <span data-ttu-id="b2afe-147">Eftersom den senaste versionen av Spark-Gremlin har skapats med Spark 1.6.1 och är inte kompatibel med Spark punkt 2.0.2, som används för närvarande i Azure Cosmos DB Spark-koppling, kan du skapa senaste TinkerPop3 koden och installera burkar manuellt.</span><span class="sxs-lookup"><span data-stu-id="b2afe-147">Because the latest version of Spark-Gremlin was built with Spark 1.6.1 and is not compatible with Spark 2.0.2, which is currently used in the Azure Cosmos DB Spark connector, you can build the latest TinkerPop3 code and install the jars manually.</span></span> <span data-ttu-id="b2afe-148">Gör följande:</span><span class="sxs-lookup"><span data-stu-id="b2afe-148">Do the following:</span></span>

    <span data-ttu-id="b2afe-149">a.</span><span class="sxs-lookup"><span data-stu-id="b2afe-149">a.</span></span> <span data-ttu-id="b2afe-150">Klona Azure Cosmos DB Spark-kopplingen.</span><span class="sxs-lookup"><span data-stu-id="b2afe-150">Clone the Azure Cosmos DB Spark connector.</span></span>

    <span data-ttu-id="b2afe-151">b.</span><span class="sxs-lookup"><span data-stu-id="b2afe-151">b.</span></span> <span data-ttu-id="b2afe-152">Skapa TinkerPop3 (redan har gjort i föregående steg).</span><span class="sxs-lookup"><span data-stu-id="b2afe-152">Build TinkerPop3 (already done in previous steps).</span></span> <span data-ttu-id="b2afe-153">Installera alla TinkerPop 3.3.0-SNAPSHOT burkar lokalt.</span><span class="sxs-lookup"><span data-stu-id="b2afe-153">Install all TinkerPop 3.3.0-SNAPSHOT jars locally.</span></span>

    ```bash
    mvn install:install-file -Dfile="gremlin-core-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-core -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar
    mvn install:install-file -Dfile="gremlin-groovy-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-groovy -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="gremlin-shaded-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-shaded -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="hadoop-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=hadoop-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="spark-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=spark-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="tinkergraph-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=tinkergraph-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    ```

    <span data-ttu-id="b2afe-154">c.</span><span class="sxs-lookup"><span data-stu-id="b2afe-154">c.</span></span> <span data-ttu-id="b2afe-155">Uppdatera `tinkerpop.version` `azure-documentdb-spark/pom.xml` till `3.3.0-SNAPSHOT`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-155">Update `tinkerpop.version` `azure-documentdb-spark/pom.xml` to `3.3.0-SNAPSHOT`.</span></span>
    
    <span data-ttu-id="b2afe-156">d.</span><span class="sxs-lookup"><span data-stu-id="b2afe-156">d.</span></span> <span data-ttu-id="b2afe-157">Skapa med Maven.</span><span class="sxs-lookup"><span data-stu-id="b2afe-157">Build with Maven.</span></span> <span data-ttu-id="b2afe-158">Nödvändiga burkar placeras i `target` och `target/alternateLocation`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-158">The needed jars are placed in `target` and `target/alternateLocation`.</span></span>

    ```bash
    git clone https://github.com/Azure/azure-cosmosdb-spark.git
    cd azure-documentdb-spark
    mvn clean package
    ```

3. <span data-ttu-id="b2afe-159">Kopiera de tidigare nämnda burkar till en lokal katalog på ~ / azure-documentdb-spark:</span><span class="sxs-lookup"><span data-stu-id="b2afe-159">Copy the previously mentioned jars to a local directory at ~/azure-documentdb-spark:</span></span>

    ```bash
    $ azure-documentdb-spark:
    mkdir ~/azure-documentdb-spark
    cp target/azure-documentdb-spark-0.0.3-SNAPSHOT.jar ~/azure-documentdb-spark
    cp target/alternateLocation/azure-documentdb-1.10.0.jar ~/azure-documentdb-spark
    ```

## <a name="distribute-the-dependencies-to-the-spark-worker-nodes"></a><span data-ttu-id="b2afe-160">Distribuera beroenden till Spark arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="b2afe-160">Distribute the dependencies to the Spark worker nodes</span></span> 

1. <span data-ttu-id="b2afe-161">Eftersom transformering av diagramdata beror på TinkerPop3, måste du distribuera relaterade beroenden för alla arbetarnoder i Spark.</span><span class="sxs-lookup"><span data-stu-id="b2afe-161">Because the transformation of graph data depends on TinkerPop3, you must distribute the related dependencies to all Spark worker nodes.</span></span>

2. <span data-ttu-id="b2afe-162">Kopiera de tidigare nämnda Gremlin beroenden, CosmosDB Spark connector jar och CosmosDB Java SDK till arbetsnoderna genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="b2afe-162">Copy the previously mentioned Gremlin dependencies, the CosmosDB Spark connector jar, and CosmosDB Java SDK to the worker nodes by doing the following:</span></span>

    <span data-ttu-id="b2afe-163">a.</span><span class="sxs-lookup"><span data-stu-id="b2afe-163">a.</span></span> <span data-ttu-id="b2afe-164">Kopiera alla burkar i `~/azure-documentdb-spark`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-164">Copy all the jars into `~/azure-documentdb-spark`.</span></span>

    ```bash
    $ /home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone:
    cp lib/* ~/azure-documentdb-spark
    ```

    <span data-ttu-id="b2afe-165">b.</span><span class="sxs-lookup"><span data-stu-id="b2afe-165">b.</span></span> <span data-ttu-id="b2afe-166">Hämta en lista över alla Spark arbetarnoder, som du hittar på Ambari instrumentpanelen i den `Spark2 Clients` lista i den `Spark2` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b2afe-166">Get the list of all Spark worker nodes, which you can find on Ambari Dashboard, in the `Spark2 Clients` list in the `Spark2` section.</span></span>

    <span data-ttu-id="b2afe-167">c.</span><span class="sxs-lookup"><span data-stu-id="b2afe-167">c.</span></span> <span data-ttu-id="b2afe-168">Kopiera den katalogen till varje nod.</span><span class="sxs-lookup"><span data-stu-id="b2afe-168">Copy that directory to each of the nodes.</span></span>

    ```bash
    scp -r ~/azure-documentdb-spark sshuser@wn0-cosmos:/home/sshuser
    scp -r ~/azure-documentdb-spark sshuser@wn1-cosmos:/home/sshuser
    ...
    ```
    
## <a name="set-up-the-environment-variables"></a><span data-ttu-id="b2afe-169">Ställ in miljövariablerna</span><span class="sxs-lookup"><span data-stu-id="b2afe-169">Set up the environment variables</span></span>

1. <span data-ttu-id="b2afe-170">Hitta HDP version av Spark-klustret.</span><span class="sxs-lookup"><span data-stu-id="b2afe-170">Find the HDP version of the Spark cluster.</span></span> <span data-ttu-id="b2afe-171">Det är katalognamnet under `/usr/hdp/` (till exempel 2.5.4.2-7).</span><span class="sxs-lookup"><span data-stu-id="b2afe-171">It is the directory name under `/usr/hdp/` (for example, 2.5.4.2-7).</span></span>

2. <span data-ttu-id="b2afe-172">Ange hdp.version för alla noder.</span><span class="sxs-lookup"><span data-stu-id="b2afe-172">Set hdp.version for all nodes.</span></span> <span data-ttu-id="b2afe-173">Ambari instrumentpanel, gå till **YARN avsnittet** > **konfigurationerna** > **Avancerat**, och gör sedan följande:</span><span class="sxs-lookup"><span data-stu-id="b2afe-173">In Ambari Dashboard, go to **YARN section** > **Configs** > **Advanced**, and then do the following:</span></span> 
 
    <span data-ttu-id="b2afe-174">a.</span><span class="sxs-lookup"><span data-stu-id="b2afe-174">a.</span></span> <span data-ttu-id="b2afe-175">I `Custom yarn-site`, lägga till en ny egenskap `hdp.version` med värdet för HDP-versionen på huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="b2afe-175">In `Custom yarn-site`, add a new property `hdp.version` with the value of the HDP version on the master node.</span></span> 
     
    <span data-ttu-id="b2afe-176">b.</span><span class="sxs-lookup"><span data-stu-id="b2afe-176">b.</span></span> <span data-ttu-id="b2afe-177">Spara konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b2afe-177">Save the configurations.</span></span> <span data-ttu-id="b2afe-178">Det finns varningar som du kan ignorera.</span><span class="sxs-lookup"><span data-stu-id="b2afe-178">There are warnings, which you can ignore.</span></span> 
     
    <span data-ttu-id="b2afe-179">c.</span><span class="sxs-lookup"><span data-stu-id="b2afe-179">c.</span></span> <span data-ttu-id="b2afe-180">Starta om tjänsterna YARN och Oozie som notification ikonerna anger.</span><span class="sxs-lookup"><span data-stu-id="b2afe-180">Restart the YARN and Oozie services as the notification icons indicate.</span></span>

3. <span data-ttu-id="b2afe-181">Ange följande miljövariabler för huvudnoden (ersätter värdena med lämpliga):</span><span class="sxs-lookup"><span data-stu-id="b2afe-181">Set the following environment variables on the master node (replace the values as appropriate):</span></span>

    ```bash
    export HADOOP_GREMLIN_LIBS=/home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/ext/spark-gremlin/lib
    export CLASSPATH=$CLASSPATH:$HADOOP_CONF_DIR:/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    export HDP_VERSION=2.5.4.2-7
    export HADOOP_HOME=${HADOOP_HOME:-/usr/hdp/current/hadoop-client}
    ```

## <a name="prepare-the-graph-configuration"></a><span data-ttu-id="b2afe-182">Förbereda graph-konfiguration</span><span class="sxs-lookup"><span data-stu-id="b2afe-182">Prepare the graph configuration</span></span>

1. <span data-ttu-id="b2afe-183">Skapa en konfigurationsfil med Azure Cosmos DB anslutningsparametrar och Spark-inställningar och placera den på `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-183">Create a configuration file with the Azure Cosmos DB connection parameters and Spark settings, and put it at `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.</span></span>

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

    # Classpath for the driver and executors
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

2. <span data-ttu-id="b2afe-184">Uppdatering av `spark.driver.extraClassPath` och `spark.executor.extraClassPath` till inkluderingskatalogen burkar som du distribuerade i föregående steg i det här fallet `/home/sshuser/azure-documentdb-spark/*`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-184">Update the `spark.driver.extraClassPath` and `spark.executor.extraClassPath` to include the directory of the jars that you distributed in the previous step, in this case `/home/sshuser/azure-documentdb-spark/*`.</span></span>

3. <span data-ttu-id="b2afe-185">Ange följande information för Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="b2afe-185">Provide the following details for Azure Cosmos DB:</span></span>

    ```
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    # Optional
    #spark.documentdb.preferredRegions=West\ US;West\ US\ 2
    ```
   
## <a name="load-the-tinkerpop-graph-and-save-it-to-azure-cosmos-db"></a><span data-ttu-id="b2afe-186">Läsa in diagrammet TinkerPop och spara den till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b2afe-186">Load the TinkerPop graph, and save it to Azure Cosmos DB</span></span>
<span data-ttu-id="b2afe-187">För att demonstrera hur du bevara ett diagram i Azure Cosmos databas med det här exemplet fördefinierade använder TinkerPop TinkerPop moderna diagram.</span><span class="sxs-lookup"><span data-stu-id="b2afe-187">To demonstrate how to persist a graph into Azure Cosmos DB, this example uses the TinkerPop predefined TinkerPop modern graph.</span></span> <span data-ttu-id="b2afe-188">Diagrammet lagras i Kryo format och anges i TinkerPop-databasen.</span><span class="sxs-lookup"><span data-stu-id="b2afe-188">The graph is stored in Kryo format, and it's provided in the TinkerPop repository.</span></span>

1. <span data-ttu-id="b2afe-189">Eftersom du kör Gremlin i YARN-läge måste du diagrammets data i Hadoop-filsystem.</span><span class="sxs-lookup"><span data-stu-id="b2afe-189">Because you are running Gremlin in YARN mode, you must make the graph data available in the Hadoop file system.</span></span> <span data-ttu-id="b2afe-190">Använd följande kommandon för att göra en katalog och kopiera filen lokala diagram till den.</span><span class="sxs-lookup"><span data-stu-id="b2afe-190">Use the following commands to make a directory and copy the local graph file into it.</span></span> 

    ```bash
    $ tinkerpop:
    hadoop fs -mkdir /graphData
    hadoop fs -copyFromLocal ~/tinkerpop/data/tinkerpop-modern.kryo /graphData/tinkerpop-modern.kryo
    ```

2. <span data-ttu-id="b2afe-191">Uppdatera tillfälligt den `gremlin-spark.properties` fil som ska användas `GryoInputFormat` att läsa i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="b2afe-191">Temporarily update the `gremlin-spark.properties` file to use `GryoInputFormat` to read the graph.</span></span> <span data-ttu-id="b2afe-192">Ange också `inputLocation` som katalogen du skapar, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="b2afe-192">Also indicate `inputLocation` as the directory you create, as in the following:</span></span>

    ```
    gremlin.hadoop.graphReader=org.apache.tinkerpop.gremlin.hadoop.structure.io.gryo.GryoInputFormat
    gremlin.hadoop.inputLocation=/graphData/tinkerpop-modern.kryo
    ```

3. <span data-ttu-id="b2afe-193">Starta Gremlin konsolen och skapa följande för att spara data till samlingen konfigurerade Azure Cosmos DB beräkning:</span><span class="sxs-lookup"><span data-stu-id="b2afe-193">Start Gremlin Console, and then create the following computation steps to persist data to the configured Azure Cosmos DB collection:</span></span>  

    <span data-ttu-id="b2afe-194">a.</span><span class="sxs-lookup"><span data-stu-id="b2afe-194">a.</span></span> <span data-ttu-id="b2afe-195">Skapa diagrammet `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-195">Create the graph `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.</span></span>

    <span data-ttu-id="b2afe-196">b.</span><span class="sxs-lookup"><span data-stu-id="b2afe-196">b.</span></span> <span data-ttu-id="b2afe-197">Använd SparkGraphComputer för skrivning `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-197">Use SparkGraphComputer for writing `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.</span></span>

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

4. <span data-ttu-id="b2afe-198">Du kan verifiera att data har sparats i Azure Cosmos DB från Data Explorer.</span><span class="sxs-lookup"><span data-stu-id="b2afe-198">From Data Explorer, you can verify that the data has been persisted to Azure Cosmos DB.</span></span>

## <a name="load-the-graph-from-azure-cosmos-db-and-run-gremlin-queries"></a><span data-ttu-id="b2afe-199">Läsa in diagrammet från Azure Cosmos DB och köra Gremlin frågor</span><span class="sxs-lookup"><span data-stu-id="b2afe-199">Load the graph from Azure Cosmos DB, and run Gremlin queries</span></span>

1. <span data-ttu-id="b2afe-200">För att läsa in diagrammet redigera `gremlin-spark.properties` ange `graphReader` till `DocumentDBInputRDD`:</span><span class="sxs-lookup"><span data-stu-id="b2afe-200">To load the graph, edit `gremlin-spark.properties` to set `graphReader` to `DocumentDBInputRDD`:</span></span>

    ```
    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    ```

2. <span data-ttu-id="b2afe-201">Läsa in diagrammet passerar data och köra Gremlin frågor till den genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="b2afe-201">Load the graph, traverse the data, and run Gremlin queries with it by doing the following:</span></span>

    <span data-ttu-id="b2afe-202">a.</span><span class="sxs-lookup"><span data-stu-id="b2afe-202">a.</span></span> <span data-ttu-id="b2afe-203">Starta konsolen Gremlin `bin/gremlin.sh`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-203">Start the Gremlin Console `bin/gremlin.sh`.</span></span>

    <span data-ttu-id="b2afe-204">b.</span><span class="sxs-lookup"><span data-stu-id="b2afe-204">b.</span></span> <span data-ttu-id="b2afe-205">Skapa diagrammet med konfigurationen `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-205">Create the graph with the configuration `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.</span></span>

    <span data-ttu-id="b2afe-206">c.</span><span class="sxs-lookup"><span data-stu-id="b2afe-206">c.</span></span> <span data-ttu-id="b2afe-207">Skapa ett diagram traversal med SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.</span><span class="sxs-lookup"><span data-stu-id="b2afe-207">Create a graph traversal with SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.</span></span>

    <span data-ttu-id="b2afe-208">d.</span><span class="sxs-lookup"><span data-stu-id="b2afe-208">d.</span></span> <span data-ttu-id="b2afe-209">Kör följande Gremlin diagram frågor:</span><span class="sxs-lookup"><span data-stu-id="b2afe-209">Run the following Gremlin graph queries:</span></span>

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
> <span data-ttu-id="b2afe-210">Om du vill se mer detaljerad loggning anger loggen i `conf/log4j-console.properties` till en mer detaljerad nivå.</span><span class="sxs-lookup"><span data-stu-id="b2afe-210">To see more detailed logging, set the log level in `conf/log4j-console.properties` to a more verbose level.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="b2afe-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b2afe-211">Next steps</span></span>

<span data-ttu-id="b2afe-212">Du har lärt dig hur du arbetar med diagram genom att kombinera Azure Cosmos DB och Spark i den här Snabbkurs i artikeln.</span><span class="sxs-lookup"><span data-stu-id="b2afe-212">In this quick-start article, you've learned how to work with graphs by combining Azure Cosmos DB and Spark.</span></span>

> [!div class="nextstepaction"]
