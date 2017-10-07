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
# <a name="azure-cosmos-db-perform-graph-analytics-by-using-spark-and-apache-tinkerpop-gremlin"></a>Azure Cosmos DB: Utföra analyser av diagram med hjälp av Spark och Apache TinkerPop Gremlin

[Azure Cosmos-DB](introduction.md) är hello globalt distribuerad databas som flera modellen tjänst från Microsoft. Du kan skapa och fråga dokument och nyckel/värde-diagrammet databaser, som dra nytta av hello global distribution och skala horisontellt funktioner på hello kärnan i Azure Cosmos DB. Har stöd för online transaktionsbearbetning (OLTP) diagrammet arbetsbelastningar som använder Azure Cosmos-DB [Apache TinkerPop Gremlin](graph-introduction.md).

[Spark](http://spark.apache.org/) är ett Apache Software Foundation-projekt som fokuserar på allmänna online analytical processing (OLAP) databearbetning. Spark tillhandahåller en hybrid i-minne/diskbaserad distribuerad datamodell som är liknande toohello Hadoop MapReduce-modellen. Du kan distribuera Apache Spark i hello molnet med hjälp av [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).

Du kan utföra både OLTP och OLAP-arbetsbelastningar när du använder Gremlin genom att kombinera Azure Cosmos DB och Spark. Den här Snabbkurs artikeln visar hur toorun Gremlin frågor mot Azure Cosmos DB på ett Azure HDInsight Spark-kluster.

## <a name="prerequisites"></a>Krav

Innan du kan köra det här exemplet måste du ha hello följande krav:
* Azure HDInsight Spark-kluster 2.0
* JDK 1.8 + (om du inte har JDK, köra `apt-get install default-jdk`.)
* Maven (om du inte har Maven kör `apt-get install maven`.)
* En Azure-prenumeration ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])

Information om hur tooset upp ett Azure HDInsight Spark-kluster, se [etablera HDInsight-kluster](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-an-azure-cosmos-db-database-account"></a>Skapa ett databaskonto Azure Cosmos DB

Skapa först ett databaskonto med hello Graph API hello följande:

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Lägga till en samling

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="get-apache-tinkerpop"></a>Hämta Apache TinkerPop

Hämta Apache TinkerPop hello följande:

1. Remote toohello huvudnoden hello HDInsight-klustret `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.

2. Klona hello TinkerPop3 källkoden, skapar det lokalt och installera den tooMaven cache.

    ```bash
    git clone https://github.com/apache/tinkerpop.git
    cd tinkerpop
    mvn clean install
    ```

3. Installera hello Spark-Gremlin plugin-program 

    a. hello-installation av plugin-programmet hello hanteras av delvis. Fyll i hello databaser information för delvis så att den kan hämta plugin-programmet hello och dess beroenden. 

      Skapa hello delvis konfigurationsfil om det inte finns på `~/.groovy/grapeConfig.xml`. Använd hello följande inställningar:

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

    b. Starta konsolen Gremlin `bin/gremlin.sh`.
        
    c. Installera hello Spark-Gremlin plugin-program med version 3.3.0-SNAPSHOT som du skapat i föregående steg i hello:

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

4. Kontrollera om toosee `Hadoop-Gremlin` aktiveras med `:plugin list`. Inaktivera det här plugin-programmet, eftersom det kan störa hello Spark-Gremlin plugin `:plugin unuse tinkerpop.hadoop`.

## <a name="prepare-tinkerpop3-dependencies"></a>Förbereda TinkerPop3 beroenden

När du har skapat TinkerPop3 i föregående steg i hello hämtas hello processen alla jar-beroenden även för Spark och Hadoop i hello målkatalogen. Använd hello burkar som är förinstallerade med HDI och ta emot ytterligare beroenden bara vid behov.

1. Gå toohello Gremlin konsolen målkatalogen på `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`. 

2. Flytta alla burkar under `ext/` för`lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.

3. Ta bort alla jar bibliotek under `lib/` att är inte i hello följande lista:

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

## <a name="get-hello-azure-cosmos-db-spark-connector"></a>Hämta hello Azure Cosmos DB Spark-koppling

1. Hämta hello Azure Cosmos DB Spark connector `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` och Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` från [Cosmos Azure DB Spark Connector på GitHub](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).

2. Alternativt kan du skapa den lokalt. Eftersom hello senaste versionen av Spark-Gremlin har skapats med Spark 1.6.1 och är inte kompatibel med Spark punkt 2.0.2, som används för närvarande i hello Azure Cosmos DB Spark-koppling, kan du skapa hello senaste TinkerPop3 kod och installera hello burkar manuellt. Hej du följande:

    a. Klona hello Azure Cosmos DB Spark-anslutningen.

    b. Skapa TinkerPop3 (redan har gjort i föregående steg). Installera alla TinkerPop 3.3.0-SNAPSHOT burkar lokalt.

    ```bash
    mvn install:install-file -Dfile="gremlin-core-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-core -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar
    mvn install:install-file -Dfile="gremlin-groovy-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-groovy -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="gremlin-shaded-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-shaded -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="hadoop-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=hadoop-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="spark-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=spark-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="tinkergraph-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=tinkergraph-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    ```

    c. Uppdatera `tinkerpop.version` `azure-documentdb-spark/pom.xml` för`3.3.0-SNAPSHOT`.
    
    d. Skapa med Maven. hello nödvändiga burkar placeras i `target` och `target/alternateLocation`.

    ```bash
    git clone https://github.com/Azure/azure-cosmosdb-spark.git
    cd azure-documentdb-spark
    mvn clean package
    ```

3. Kopiera hello tidigare nämnts burkar tooa lokal katalog på ~ / azure-documentdb-spark:

    ```bash
    $ azure-documentdb-spark:
    mkdir ~/azure-documentdb-spark
    cp target/azure-documentdb-spark-0.0.3-SNAPSHOT.jar ~/azure-documentdb-spark
    cp target/alternateLocation/azure-documentdb-1.10.0.jar ~/azure-documentdb-spark
    ```

## <a name="distribute-hello-dependencies-toohello-spark-worker-nodes"></a>Distribuera hello beroenden toohello Spark arbetsnoder 

1. Eftersom hello transformering av diagramdata beror på TinkerPop3, måste du distribuera hello relaterade beroenden tooall Spark arbetsnoderna.

2. Kopiera hello tidigare nämnts Gremlin beroenden, hello CosmosDB Spark connector jar och CosmosDB Java SDK toohello arbetarnoder hello följande:

    a. Kopiera alla hello burkar i `~/azure-documentdb-spark`.

    ```bash
    $ /home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone:
    cp lib/* ~/azure-documentdb-spark
    ```

    b. Hämta hello lista över alla Spark arbetarnoder, som du hittar på Ambari instrumentpanelen i hello `Spark2 Clients` listan i hello `Spark2` avsnitt.

    c. Kopiera den directory tooeach hello noder.

    ```bash
    scp -r ~/azure-documentdb-spark sshuser@wn0-cosmos:/home/sshuser
    scp -r ~/azure-documentdb-spark sshuser@wn1-cosmos:/home/sshuser
    ...
    ```
    
## <a name="set-up-hello-environment-variables"></a>Ställ in hello miljövariabler

1. Hitta hello HDP version av hello Spark-kluster. Det är hello katalognamnet under `/usr/hdp/` (till exempel 2.5.4.2-7).

2. Ange hdp.version för alla noder. I instrumentpanelen för Ambari gå för**YARN avsnittet** > **konfigurationerna** > **Avancerat**, och sedan hello följande: 
 
    a. I `Custom yarn-site`, lägga till en ny egenskap `hdp.version` med hello värdet hello HDP versionen på hello huvudnod. 
     
    b. Spara hello konfigurationer. Det finns varningar som du kan ignorera. 
     
    c. Starta om tjänsterna för hello YARN och Oozie som hello notification ikoner anger.

3. Ange hello följa miljövariabler på hello huvudnod (Ersätt hello-värden efter behov):

    ```bash
    export HADOOP_GREMLIN_LIBS=/home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/ext/spark-gremlin/lib
    export CLASSPATH=$CLASSPATH:$HADOOP_CONF_DIR:/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    export HDP_VERSION=2.5.4.2-7
    export HADOOP_HOME=${HADOOP_HOME:-/usr/hdp/current/hadoop-client}
    ```

## <a name="prepare-hello-graph-configuration"></a>Förbereda hello graph-konfiguration

1. Skapa en konfigurationsfil med hello Azure Cosmos DB anslutningsparametrar Väck inställningar och placera den på `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.

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

2. Uppdatera hello `spark.driver.extraClassPath` och `spark.executor.extraClassPath` tooinclude hello-katalogen för hello burkar som du distribuerat i hello föregående steg i det här fallet `/home/sshuser/azure-documentdb-spark/*`.

3. Ange följande information för Azure Cosmos DB hello:

    ```
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    # Optional
    #spark.documentdb.preferredRegions=West\ US;West\ US\ 2
    ```
   
## <a name="load-hello-tinkerpop-graph-and-save-it-tooazure-cosmos-db"></a>Läsa in hello TinkerPop diagram och spara den tooAzure Cosmos DB
toodemonstrate hur toopersist ett diagram i Azure Cosmos databas med det här exemplet använder hello TinkerPop fördefinierade TinkerPop moderna diagram. hello diagram lagras i Kryo format och anges i hello TinkerPop databas.

1. Eftersom du kör Gremlin i YARN-läge måste du hello diagramdata i hello Hadoop-filsystem. Använd hello följande kommandon toomake en katalog och kopiera hello lokala diagrammet fil till den. 

    ```bash
    $ tinkerpop:
    hadoop fs -mkdir /graphData
    hadoop fs -copyFromLocal ~/tinkerpop/data/tinkerpop-modern.kryo /graphData/tinkerpop-modern.kryo
    ```

2. Uppdatera tillfälligt hello `gremlin-spark.properties` filen toouse `GryoInputFormat` tooread hello diagram. Ange också `inputLocation` som hello katalog skapas som hello följande:

    ```
    gremlin.hadoop.graphReader=org.apache.tinkerpop.gremlin.hadoop.structure.io.gryo.GryoInputFormat
    gremlin.hadoop.inputLocation=/graphData/tinkerpop-modern.kryo
    ```

3. Starta Gremlin konsolen och sedan skapa hello följande beräkning steg toopersist toohello konfigurerade Azure Cosmos DB datainsamling:  

    a. Skapa hello diagram `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.

    b. Använd SparkGraphComputer för skrivning `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.

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

4. Du kan verifiera att data har hello beständiga tooAzure Cosmos DB från Data Explorer.

## <a name="load-hello-graph-from-azure-cosmos-db-and-run-gremlin-queries"></a>Läsa in hello diagrammet från Azure Cosmos DB och köra Gremlin frågor

1. tooload hello diagrammet redigera `gremlin-spark.properties` tooset `graphReader` för`DocumentDBInputRDD`:

    ```
    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    ```

2. Läsa in hello diagram, gå igenom hello data och kör Gremlin frågor med den hello följande:

    a. Starta hello Gremlin konsolen `bin/gremlin.sh`.

    b. Skapa hello diagram med hello configuration `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.

    c. Skapa ett diagram traversal med SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.

    d. Kör hello följande Gremlin diagram frågor:

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
> toosee mer detaljerad loggning som hello loggningsnivån i `conf/log4j-console.properties` tooa mer detaljerad nivå.
>

## <a name="next-steps"></a>Nästa steg

I den här Snabbkurs artikeln har du lärt dig hur toowork med diagram genom att kombinera Azure Cosmos DB och Spark.

> [!div class="nextstepaction"]
