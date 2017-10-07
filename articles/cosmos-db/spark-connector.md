---
title: aaaConnecting Apache Spark tooAzure Cosmos DB | Microsoft Docs
description: "Använd den här självstudiekursen toolearn om hello Azure Cosmos DB Spark connector som du kan använda tooconnect Apache Spark tooAzure Cosmos DB tooperform distribuerade aggregeringar och data sciences hello flera innehavare globalt distribuerad databas systemet från Microsoft som är avsedd för hello molnet."
keywords: Apache spark
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: c4f46007-2606-4273-ab16-29d0e15c0736
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: denlee
ms.openlocfilehash: 70b496fc5ca8f65675f0224e749637f5d533c346
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="accelerate-real-time-big-data-analytics-with-hello-spark-tooazure-cosmos-db-connector"></a><span data-ttu-id="1ae7e-104">Påskynda realtid stordata med hello Spark tooAzure Cosmos DB connector</span><span class="sxs-lookup"><span data-stu-id="1ae7e-104">Accelerate real-time big-data analytics with hello Spark tooAzure Cosmos DB connector</span></span>

<span data-ttu-id="1ae7e-105">hello Spark tooAzure Cosmos DB connector kan Azure Cosmos DB tooact som Indatakällan eller utdatamottagaren för Apache Spark-jobb.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-105">hello Spark tooAzure Cosmos DB connector enables Azure Cosmos DB tooact as an input source or output sink for Apache Spark jobs.</span></span> <span data-ttu-id="1ae7e-106">Ansluta [Spark](http://spark.apache.org/) för[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) accelererar din möjlighet toosolve fast flytta datavetenskap problem där du kan använda Azure Cosmos DB tooquickly kvarstår och fråga efter data.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-106">Connecting [Spark](http://spark.apache.org/) too[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) accelerates your ability toosolve fast-moving data science problems where you can use Azure Cosmos DB tooquickly persist and query data.</span></span> <span data-ttu-id="1ae7e-107">hello Spark tooAzure Cosmos DB anslutningen använder effektivt hello interna Azure Cosmos DB hanteras index.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-107">hello Spark tooAzure Cosmos DB connector efficiently utilizes hello native Azure Cosmos DB managed indexes.</span></span> <span data-ttu-id="1ae7e-108">hello index aktivera uppdateringsbara kolumner när du utföra analyser och push-ned predikat filtrering mot fast föränderliga globalt distribuerade data mellan scenarier i Sakernas Internet (IoT) toodata vetenskap och analyser.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-108">hello indexes enable updateable columns when you perform analytics and push-down predicate filtering against fast-changing globally distributed data, which range from Internet of Things (IoT) toodata science and analytics scenarios.</span></span>

<span data-ttu-id="1ae7e-109">För att arbeta med Spark GraphX och hello Gremlin graph API: er i Azure Cosmos DB, se [utföra diagrammet analytics med hjälp av Spark och Apache TinkerPop Gremlin](spark-connector-graph.md).</span><span class="sxs-lookup"><span data-stu-id="1ae7e-109">For working with Spark GraphX and hello Gremlin graph APIs of Azure Cosmos DB, see [Perform graph analytics using Spark and Apache TinkerPop Gremlin](spark-connector-graph.md).</span></span>

## <a name="download"></a><span data-ttu-id="1ae7e-110">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="1ae7e-110">Download</span></span>

<span data-ttu-id="1ae7e-111">tooget igång genom att hämta hello Spark tooAzure Cosmos DB connector (förhandsgranskning) från hello [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/) databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-111">tooget started, download hello Spark tooAzure Cosmos DB connector (preview) from hello [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/) repository on GitHub.</span></span>

## <a name="connector-components"></a><span data-ttu-id="1ae7e-112">Connector-komponenter</span><span class="sxs-lookup"><span data-stu-id="1ae7e-112">Connector components</span></span>

<span data-ttu-id="1ae7e-113">hello-anslutningen använder hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-113">hello connector utilizes hello following components:</span></span>

* <span data-ttu-id="1ae7e-114">[Azure Cosmos-DB](http://documentdb.com) gör att kunder tooprovision och skala Elastiskt både dataflöden och lagringsutrymmen till alla geografiska regioner.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-114">[Azure Cosmos DB](http://documentdb.com) enables customers tooprovision and elastically scale both throughput and storage across any number of geographical regions.</span></span> <span data-ttu-id="1ae7e-115">hello-tjänsten erbjuder:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-115">hello service offers:</span></span>
   * <span data-ttu-id="1ae7e-116">Aktivera nyckeln [global distributionsplatsen](distribute-data-globally.md) och skala horisontellt</span><span class="sxs-lookup"><span data-stu-id="1ae7e-116">Turn key [global distribution](distribute-data-globally.md) and horizontal scale</span></span>
   * <span data-ttu-id="1ae7e-117">Garanteras siffra fördröjningar vid hello 99th percentilen</span><span class="sxs-lookup"><span data-stu-id="1ae7e-117">Guaranteed single digit latencies at hello 99th percentile</span></span>
   * [<span data-ttu-id="1ae7e-118">Flera väldefinierade konsekvenskontroll modeller</span><span class="sxs-lookup"><span data-stu-id="1ae7e-118">Multiple well-defined consistency models</span></span>](consistency-levels.md)
   * <span data-ttu-id="1ae7e-119">Garanterat hög tillgänglighet med flera funktioner</span><span class="sxs-lookup"><span data-stu-id="1ae7e-119">Guaranteed high availability with multi-homing capabilities</span></span>
   * <span data-ttu-id="1ae7e-120">Alla funktioner backas upp av branschledande, omfattande [serviceavtal](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLA).</span><span class="sxs-lookup"><span data-stu-id="1ae7e-120">All features are backed by industry-leading, comprehensive [service level agreements](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLAs).</span></span>

* <span data-ttu-id="1ae7e-121">[Apache Spark](http://spark.apache.org/) är en kraftfull öppen källkod bearbetningsmotor som är uppbyggd kring hastighet, enkel användning och avancerade analyser.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-121">[Apache Spark](http://spark.apache.org/) is a powerful open source processing engine that's built around speed, ease of use, and sophisticated analytics.</span></span>

* <span data-ttu-id="1ae7e-122">[Apache Spark på Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) så att du kan distribuera Apache Spark i hello molnet för verksamhetskritiska distributioner med hjälp av [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span><span class="sxs-lookup"><span data-stu-id="1ae7e-122">[Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) so that you can deploy Apache Spark in hello cloud for mission-critical deployments by using [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span></span>

<span data-ttu-id="1ae7e-123">Officiellt versioner som stöds:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-123">Officially supported versions:</span></span>

| <span data-ttu-id="1ae7e-124">Komponent</span><span class="sxs-lookup"><span data-stu-id="1ae7e-124">Component</span></span> | <span data-ttu-id="1ae7e-125">Version</span><span class="sxs-lookup"><span data-stu-id="1ae7e-125">Version</span></span> |
|---------|-------|
|<span data-ttu-id="1ae7e-126">Apache Spark</span><span class="sxs-lookup"><span data-stu-id="1ae7e-126">Apache Spark</span></span>|<span data-ttu-id="1ae7e-127">2.0+</span><span class="sxs-lookup"><span data-stu-id="1ae7e-127">2.0+</span></span>|
| <span data-ttu-id="1ae7e-128">Scala</span><span class="sxs-lookup"><span data-stu-id="1ae7e-128">Scala</span></span>| <span data-ttu-id="1ae7e-129">2.11</span><span class="sxs-lookup"><span data-stu-id="1ae7e-129">2.11</span></span>|
| <span data-ttu-id="1ae7e-130">Azure DocumentDB Java SDK</span><span class="sxs-lookup"><span data-stu-id="1ae7e-130">Azure DocumentDB Java SDK</span></span> | <span data-ttu-id="1ae7e-131">1.10.0</span><span class="sxs-lookup"><span data-stu-id="1ae7e-131">1.10.0</span></span> |

<span data-ttu-id="1ae7e-132">Den här artikeln hjälper dig att köra några enkla exempel med hjälp av Python (via pyDocumentDB) och hello Scala gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-132">This article helps you run some simple samples by using Python (via pyDocumentDB) and hello Scala interfaces.</span></span>

<span data-ttu-id="1ae7e-133">Det finns två tillvägagångssätt tooconnect Apache Spark och Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-133">There are two approaches tooconnect Apache Spark and Azure Cosmos DB:</span></span>
- <span data-ttu-id="1ae7e-134">Använd pyDocumentDB via hello [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).</span><span class="sxs-lookup"><span data-stu-id="1ae7e-134">Use pyDocumentDB via hello [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).</span></span>
- <span data-ttu-id="1ae7e-135">Skapa en Java-baserad Spark tooAzure Cosmos-DB-koppling genom att använda hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).</span><span class="sxs-lookup"><span data-stu-id="1ae7e-135">Create a Java-based Spark tooAzure Cosmos DB connector by utilizing hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).</span></span>

## <a name="pydocumentdb-implementation"></a><span data-ttu-id="1ae7e-136">pyDocumentDB implementering</span><span class="sxs-lookup"><span data-stu-id="1ae7e-136">pyDocumentDB implementation</span></span>
<span data-ttu-id="1ae7e-137">hello aktuella [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) tooconnect Spark tooAzure Cosmos DB gör enligt följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-137">hello current [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) enables you tooconnect Spark tooAzure Cosmos DB as shown in hello following diagram:</span></span>

![Spark tooAzure Cosmos DB dataflöde via pyDocumentDB DB](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow-of-hello-pydocumentdb-implementation"></a><span data-ttu-id="1ae7e-139">Dataflöde för hello pyDocumentDB implementering</span><span class="sxs-lookup"><span data-stu-id="1ae7e-139">Data flow of hello pyDocumentDB implementation</span></span>

<span data-ttu-id="1ae7e-140">hello dataflöde är följande:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-140">hello data flow is as follows:</span></span>

1. <span data-ttu-id="1ae7e-141">hello Spark huvudnoden ansluter toohello Azure Cosmos DB gateway-noden via pyDocumentDB.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-141">hello Spark master node connects toohello Azure Cosmos DB gateway node via pyDocumentDB.</span></span> <span data-ttu-id="1ae7e-142">En användare anger endast hello Spark och Azure Cosmos DB-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-142">A user specifies only hello Spark and Azure Cosmos DB connections.</span></span> <span data-ttu-id="1ae7e-143">Anslutningar toohello respektive huvud- och gateway-noderna är transparent toohello användare.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-143">Connections toohello respective master and gateway nodes are transparent toohello user.</span></span>
2. <span data-ttu-id="1ae7e-144">hello gateway-noden gör hello frågan mot Azure Cosmos DB där hello frågan därefter körs mot hello samling partitioner i hello datanoder.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-144">hello gateway node makes hello query against Azure Cosmos DB where hello query subsequently runs against hello collection's partitions in hello data nodes.</span></span> <span data-ttu-id="1ae7e-145">hello svar för dessa frågor skickas tillbaka toohello gateway-noden och att resultatet returneras toohello Spark-nod.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-145">hello response for those queries is sent back toohello gateway node, and that result set is returned toohello Spark master node.</span></span>
3. <span data-ttu-id="1ae7e-146">Efterföljande frågor (till exempel mot ett Spark-DataFrame) skickas toohello Spark arbetarnoder för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-146">Subsequent queries (for example, against a Spark DataFrame) are sent toohello Spark worker nodes for processing.</span></span>

<span data-ttu-id="1ae7e-147">Kommunikation mellan Spark och Azure Cosmos DB är begränsad toohello Spark huvudnod och Azure Cosmos DB gateway-noder.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-147">Communication between Spark and Azure Cosmos DB is limited toohello Spark master node and Azure Cosmos DB gateway nodes.</span></span>  <span data-ttu-id="1ae7e-148">hello frågor går snabbt hello Transportskiktet mellan dessa två noder kan.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-148">hello queries go as fast as hello transport layer between these two nodes allows.</span></span>

### <a name="install-pydocumentdb"></a><span data-ttu-id="1ae7e-149">Installera pyDocumentDB</span><span class="sxs-lookup"><span data-stu-id="1ae7e-149">Install pyDocumentDB</span></span>
<span data-ttu-id="1ae7e-150">Du kan installera pyDocumentDB på Drivrutinsnoden med hjälp av **pip**, till exempel:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-150">You can install pyDocumentDB on your driver node by using **pip**, for example:</span></span>

```
pip install pyDocumentDB
```


### <a name="connect-spark-tooazure-cosmos-db-via-pydocumentdb"></a><span data-ttu-id="1ae7e-151">Ansluta Spark tooAzure Cosmos DB via pyDocumentDB</span><span class="sxs-lookup"><span data-stu-id="1ae7e-151">Connect Spark tooAzure Cosmos DB via pyDocumentDB</span></span>
<span data-ttu-id="1ae7e-152">hello enkelhet hello kommunikation transport gör körningen av en fråga från Spark tooAzure Cosmos DB genom att använda relativt enkla pyDocumentDB.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-152">hello simplicity of hello communication transport makes execution of a query from Spark tooAzure Cosmos DB by using pyDocumentDB relatively simple.</span></span>

<span data-ttu-id="1ae7e-153">Hej följande fragment visas hur toouse pyDocumentDB i en kontext som Spark.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-153">hello following code snippet shows how toouse pyDocumentDB in a Spark context.</span></span>

```
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring hello connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery
connectionPolicy.PreferredLocations = ["Central US", "East US 2", "Southeast Asia", "Western Europe","Canada Central"]


# Set keys tooconnect tooAzure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)
```

<span data-ttu-id="1ae7e-154">Enligt beskrivningen i hello kodstycke:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-154">As noted in hello code snippet:</span></span>

* <span data-ttu-id="1ae7e-155">hello Azure Cosmos DB Python SDK (`pyDocumentDB`) innehåller hello alla hello nödvändiga anslutningsparametrar.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-155">hello Azure Cosmos DB Python SDK (`pyDocumentDB`) contains hello all hello necessary connection parameters.</span></span> <span data-ttu-id="1ae7e-156">Till exempel önskade hello platser parametern väljer hello läsa replik och prioritet.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-156">For example, hello preferred locations parameter chooses hello read replica and priority order.</span></span>
*  <span data-ttu-id="1ae7e-157">Importera hello nödvändiga bibliotek och konfigurera din **masterKey** och **värden** toocreate hello Azure Cosmos DB *klienten* (**pydocumentdb.document_ klienten**).</span><span class="sxs-lookup"><span data-stu-id="1ae7e-157">Import hello necessary libraries and configure your **masterKey** and **host** toocreate hello Azure Cosmos DB *client* (**pydocumentdb.document_client**).</span></span>


### <a name="execute-spark-queries-via-pydocumentdb"></a><span data-ttu-id="1ae7e-158">Köra Spark-frågor via pyDocumentDB</span><span class="sxs-lookup"><span data-stu-id="1ae7e-158">Execute Spark Queries via pyDocumentDB</span></span>
<span data-ttu-id="1ae7e-159">följande exempel används hello Azure DB som Cosmos-instans som skapades i föregående hello-fragment med hjälp av hello hello angetts skrivskyddade nycklar.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-159">hello following examples use hello Azure Cosmos DB instance that was created in hello previous snippet by using hello specified read-only keys.</span></span> <span data-ttu-id="1ae7e-160">hello följande kodavsnitt ansluter toohello **airports.codes** samling i hello DoctorWho konto som angavs tidigare och kör en fråga tooextract hello flygplats orter i staten Washington.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-160">hello following code snippet connects toohello **airports.codes** collection in hello DoctorWho account as specified earlier and runs a query tooextract hello airport cities in Washington state.</span></span>

```
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations hello Azure Cosmos DB client will use tooconnect toohello database and collection
dbLink = 'dbs/' + databaseId
collLink = dbLink + '/colls/' + collectionId

# Set query parameter
querystr = "SELECT c.City FROM c WHERE c.State='WA'"

# Query documents
query = client.QueryDocuments(collLink, querystr, options=None, partition_key=None)

# Query for partitioned collections
# query = client.QueryDocuments(collLink, query, options= { 'enableCrossPartitionQuery': True }, partition_key=None)

# Push into list `elements`
elements = list(query)
```

<span data-ttu-id="1ae7e-161">När hello frågan har utförts **frågan**, hello resultatet är en **query_iterable. QueryIterable** som är konverterade tooa Python-listan.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-161">After hello query has been executed via **query**, hello result is a **query_iterable.QueryIterable** that is converted tooa Python list.</span></span> <span data-ttu-id="1ae7e-162">En lista med Python kan vara enkelt konverterade tooa Spark DataFrame med hjälp av hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-162">A Python list can be easily converted tooa Spark DataFrame by using hello following code:</span></span>

```
# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(elements)
```

### <a name="why-use-hello-pydocumentdb-tooconnect-spark-tooazure-cosmos-db"></a><span data-ttu-id="1ae7e-163">Varför använda hello pyDocumentDB tooconnect Spark tooAzure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="1ae7e-163">Why use hello pyDocumentDB tooconnect Spark tooAzure Cosmos DB?</span></span>
<span data-ttu-id="1ae7e-164">Ansluta Spark tooAzure Cosmos-databas med hjälp av pyDocumentDB är vanligtvis för scenarier där:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-164">Connecting Spark tooAzure Cosmos DB by using pyDocumentDB is typically for scenarios where:</span></span>

* <span data-ttu-id="1ae7e-165">Vill du toouse Python.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-165">You want toouse Python.</span></span>
* <span data-ttu-id="1ae7e-166">Du returnerar en relativt liten resultatuppsättning från Azure Cosmos DB tooSpark.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-166">You are returning a relatively small result set from Azure Cosmos DB tooSpark.</span></span> <span data-ttu-id="1ae7e-167">Observera att hello underliggande dataset i Azure Cosmos-databasen kan vara ganska stor.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-167">Note that hello underlying dataset in Azure Cosmos DB can be quite large.</span></span> <span data-ttu-id="1ae7e-168">Du kopplar filter, det vill säga kör predikat filter mot dina Azure Cosmos DB-datakälla.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-168">You are applying filters, that is, running predicate filters, against your Azure Cosmos DB source.</span></span>  

## <a name="spark-tooazure-cosmos-db-connector"></a><span data-ttu-id="1ae7e-169">Spark tooAzure Cosmos-DB-koppling</span><span class="sxs-lookup"><span data-stu-id="1ae7e-169">Spark tooAzure Cosmos DB connector</span></span>

<span data-ttu-id="1ae7e-170">hello Spark tooAzure Cosmos DB anslutningen använder hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) och flyttar data mellan hello Spark arbetarnoder och Azure Cosmos DB som visas i följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-170">hello Spark tooAzure Cosmos DB connector utilizes hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) and moves data between hello Spark worker nodes and Azure Cosmos DB as shown in hello following diagram:</span></span>

![Dataflödet i hello Spark tooAzure Cosmos-DB-koppling](./media/spark-connector/spark-connector.png)

<span data-ttu-id="1ae7e-172">hello dataflöde är följande:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-172">hello data flow is as follows:</span></span>

1. <span data-ttu-id="1ae7e-173">hello Spark huvudnoden ansluter toohello Azure Cosmos DB gateway-noden tooobtain hello partitionsöversikten.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-173">hello Spark master node connects toohello Azure Cosmos DB gateway node tooobtain hello partition map.</span></span> <span data-ttu-id="1ae7e-174">En användare anger endast hello Spark och Azure Cosmos DB-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-174">A user specifies only hello Spark and Azure Cosmos DB connections.</span></span> <span data-ttu-id="1ae7e-175">Anslutningar toohello respektive huvud- och gateway-noderna är transparent toohello användare.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-175">Connections toohello respective master and gateway nodes are transparent toohello user.</span></span>
2. <span data-ttu-id="1ae7e-176">Den här informationen kan tillbaka toohello Spark-nod.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-176">This information is provided back toohello Spark master node.</span></span>  <span data-ttu-id="1ae7e-177">Du ska nu kunna tooparse hello frågan toodetermine hello partitioner och deras platser i Azure Cosmos-databasen som du behöver tooaccess.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-177">At this point, you should be able tooparse hello query toodetermine hello partitions and their locations in Azure Cosmos DB that you need tooaccess.</span></span>
3. <span data-ttu-id="1ae7e-178">Den här informationen är överförda toohello Spark arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-178">This information is transmitted toohello Spark worker nodes.</span></span>
4. <span data-ttu-id="1ae7e-179">hello Spark arbetarnoder ansluta toohello Azure Cosmos DB partitioner direkt tooextract hello data och returnera hello data toohello Spark partitioner i hello Spark arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-179">hello Spark worker nodes connect toohello Azure Cosmos DB partitions directly tooextract hello data and return hello data toohello Spark partitions in hello Spark worker nodes.</span></span>

<span data-ttu-id="1ae7e-180">Kommunikation mellan Spark och Azure Cosmos DB är betydligt snabbare eftersom hello dataförflyttning mellan hello Spark arbetarnoder och hello Azure Cosmos DB datanoder (partitioner).</span><span class="sxs-lookup"><span data-stu-id="1ae7e-180">Communication between Spark and Azure Cosmos DB is significantly faster because hello data movement is between hello Spark worker nodes and hello Azure Cosmos DB data nodes (partitions).</span></span>

### <a name="build-hello-spark-tooazure-cosmos-db-connector"></a><span data-ttu-id="1ae7e-181">Skapa hello Spark tooAzure Cosmos-DB-koppling</span><span class="sxs-lookup"><span data-stu-id="1ae7e-181">Build hello Spark tooAzure Cosmos DB connector</span></span>
<span data-ttu-id="1ae7e-182">Hello connector projekt används för närvarande maven.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-182">Currently, hello connector project uses maven.</span></span> <span data-ttu-id="1ae7e-183">toobuild hello-anslutningen utan beroenden, som du kan köra:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-183">toobuild hello connector without dependencies, you can run:</span></span>
```
mvn clean package
```
<span data-ttu-id="1ae7e-184">Du kan också hämta hello senaste versionerna av hello JAR från hello *släpper* mapp.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-184">You can also download hello latest versions of hello JAR from hello *releases* folder.</span></span>

### <a name="include-hello-azure-cosmos-db-spark-jar"></a><span data-ttu-id="1ae7e-185">Inkludera hello Azure Cosmos DB Spark JAR</span><span class="sxs-lookup"><span data-stu-id="1ae7e-185">Include hello Azure Cosmos DB Spark JAR</span></span>
<span data-ttu-id="1ae7e-186">Innan du kör någon kod, behöver du tooinclude hello Azure Cosmos DB Spark JAR.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-186">Before you execute any code, you need tooinclude hello Azure Cosmos DB Spark JAR.</span></span>  <span data-ttu-id="1ae7e-187">Om du använder hello **spark-shell**, och du kan inkludera hello JAR med hjälp av hello **--burkar** alternativet.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-187">If you are using hello **spark-shell**, then you can include hello JAR by using hello **--jars** option.</span></span>  

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3-jar-with-dependencies.jar
```

<span data-ttu-id="1ae7e-188">Om du vill tooexecute hello JAR utan beroenden, Använd hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-188">If you want tooexecute hello JAR without dependencies, use hello following code:</span></span>

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3.jar,/$location/azure-documentdb-1.10.0.jar
```

<span data-ttu-id="1ae7e-189">Om du använder en bärbar dator tjänst, till exempel Azure HDInsight Jupyter-anteckningsbok service kan du använda hello **Väck magic** kommandon:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-189">If you are using a notebook service such as Azure HDInsight Jupyter notebook service, you can use hello **spark magic** commands:</span></span>

```
%%configure
{ "jars": ["wasb:///example/jars/azure-documentdb-1.10.0.jar","wasb:///example/jars/azure-cosmosdb-spark-0.0.3.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

<span data-ttu-id="1ae7e-190">Hej **burkar** kommando aktiverar du tooinclude hello två JAR: er som krävs för **azure cosmosdb spark** (sig själv och hello Azure DocumentDB Java SDK) och utelämna **scala-återspeglar** så att den inte stör hello Livius anropar (Jupyter-anteckningsbok > Livius > Spark).</span><span class="sxs-lookup"><span data-stu-id="1ae7e-190">hello **jars** command enables you tooinclude hello two JARs that are needed for **azure-cosmosdb-spark** (itself and hello Azure DocumentDB Java SDK) and exclude **scala-reflect** so that it does not interfere with hello Livy calls (Jupyter notebook > Livy > Spark).</span></span>

### <a name="connect-spark-tooazure-cosmos-db-using-hello-connector"></a><span data-ttu-id="1ae7e-191">Ansluta Spark tooAzure Cosmos DB med hello connector</span><span class="sxs-lookup"><span data-stu-id="1ae7e-191">Connect Spark tooAzure Cosmos DB using hello connector</span></span>
<span data-ttu-id="1ae7e-192">Även om hello kommunikation transport är lite mer komplicerad, kör en fråga från Spark tooAzure Cosmos DB med hello-anslutningen är betydligt snabbare.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-192">Although hello communication transport is a little more complicated, executing a query from Spark tooAzure Cosmos DB by using hello connector is significantly faster.</span></span>

<span data-ttu-id="1ae7e-193">hello följande kodfragment som visar hur toouse hello kopplingen i en Spark-kontext.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-193">hello following code snippet shows how toouse hello connector in a Spark context.</span></span>

```
// Import Necessary Libraries
import org.joda.time._
import org.joda.time.format._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Configure connection tooyour collection
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US2;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(readConfig2)
coll.createOrReplaceTempView("c")
```

<span data-ttu-id="1ae7e-194">Enligt beskrivningen i hello kodstycke:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-194">As noted in hello code snippet:</span></span>

- <span data-ttu-id="1ae7e-195">**Azure cosmosdb spark** innehåller hello alla hello nödvändiga anslutningsparametrar, bland annat hello önskade platser.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-195">**azure-cosmosdb-spark** contains hello all hello necessary connection parameters, which include hello preferred locations.</span></span> <span data-ttu-id="1ae7e-196">Exempelvis kan du välja hello läsa replik och prioritet.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-196">For example, you can choose hello read replica and priority order.</span></span>
- <span data-ttu-id="1ae7e-197">Bara importera hello nödvändiga bibliotek och konfigurera din masterKey och värden toocreate hello Azure DB som Cosmos-klienten.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-197">Just import hello necessary libraries and configure your masterKey and host toocreate hello Azure Cosmos DB client.</span></span>

### <a name="execute-spark-queries-via-hello-connector"></a><span data-ttu-id="1ae7e-198">Köra Spark-frågor via hello-koppling</span><span class="sxs-lookup"><span data-stu-id="1ae7e-198">Execute Spark queries via hello connector</span></span>

<span data-ttu-id="1ae7e-199">följande exempel använder hello Azure DB som Cosmos-instans som skapades i föregående hello-fragment med hjälp av hello hello angetts skrivskyddade nycklar.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-199">hello following example uses hello Azure Cosmos DB instance that was created in hello previous snippet by using hello specified read-only keys.</span></span> <span data-ttu-id="1ae7e-200">hello följande kodavsnitt ansluter toohello DepartureDelays.flights_pcoll samling (i hello DoctorWho kontot som angavs tidigare) och kör en fråga tooextract hello fördröjning flyginformation av flygplan reser från Seattle.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-200">hello following code snippet connects toohello DepartureDelays.flights_pcoll collection (in hello DoctorWho account as specified earlier) and runs a query tooextract hello flight delay information of flights that are departing from Seattle.</span></span>

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-hello-spark-tooazure-cosmos-db-connector-implementation"></a><span data-ttu-id="1ae7e-201">Varför använda hello Spark tooAzure Cosmos DB connector implementering?</span><span class="sxs-lookup"><span data-stu-id="1ae7e-201">Why use hello Spark tooAzure Cosmos DB connector implementation?</span></span>

<span data-ttu-id="1ae7e-202">Ansluta Spark tooAzure Cosmos DB med hello-anslutningen är vanligtvis för scenarier där:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-202">Connecting Spark tooAzure Cosmos DB by using hello connector is typically for scenarios where:</span></span>

* <span data-ttu-id="1ae7e-203">Du vill använda toouse Scala och uppdatera den tooinclude en Python-omslutning som anges i [problemet 3: lägga till Python wrapper och exempel](https://github.com/Azure/azure-cosmosdb-spark/issues/3).</span><span class="sxs-lookup"><span data-stu-id="1ae7e-203">You want toouse Scala and update it tooinclude a Python wrapper as noted in [Issue 3: Add Python wrapper and examples](https://github.com/Azure/azure-cosmosdb-spark/issues/3).</span></span>
* <span data-ttu-id="1ae7e-204">Du har en stor mängd data tootransfer mellan Apache Spark och Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-204">You have a large amount of data tootransfer between Apache Spark and Azure Cosmos DB.</span></span>

<span data-ttu-id="1ae7e-205">toogive dig en uppfattning om hello frågan prestandaskillnaden finns hello [frågan Testkörningar wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).</span><span class="sxs-lookup"><span data-stu-id="1ae7e-205">toogive you an idea of hello query performance difference, see hello [Query Test Runs wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).</span></span>

## <a name="distributed-aggregation-example"></a><span data-ttu-id="1ae7e-206">Distribuerade aggregeringsexempel</span><span class="sxs-lookup"><span data-stu-id="1ae7e-206">Distributed aggregation example</span></span>
<span data-ttu-id="1ae7e-207">Det här avsnittet innehåller några exempel på hur du kan göra distribuerade aggregeringar och analytics med hjälp av Apache Spark och Azure Cosmos DB tillsammans.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-207">This section provides some examples of how you can do distributed aggregations and analytics by using Apache Spark and Azure Cosmos DB together.</span></span> <span data-ttu-id="1ae7e-208">Azure Cosmos-DB stöder redan aggregeringar, som beskrivs i hello [planeten skala mängder med Azure Cosmos DB blogg](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/).</span><span class="sxs-lookup"><span data-stu-id="1ae7e-208">Azure Cosmos DB already supports aggregations, which is discussed in hello [Planet scale aggregates with Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/).</span></span> <span data-ttu-id="1ae7e-209">Här är hur du kan dra den toohello nästa nivå med Apache Spark.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-209">Here is how you can take it toohello next level with Apache Spark.</span></span>

<span data-ttu-id="1ae7e-210">Observera att dessa aggregeringar i referens toohello [Spark tooAzure Cosmos DB Connector anteckningsboken](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).</span><span class="sxs-lookup"><span data-stu-id="1ae7e-210">Note that these aggregations are in reference toohello [Spark tooAzure Cosmos DB Connector notebook](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).</span></span>

### <a name="connect-tooflights-sample-data"></a><span data-ttu-id="1ae7e-211">Ansluta tooflights exempeldata</span><span class="sxs-lookup"><span data-stu-id="1ae7e-211">Connect tooflights sample data</span></span>
<span data-ttu-id="1ae7e-212">Exemplen aggregering åtkomst till vissa svarta prestandadata som är lagrad i vår **DoctorWho** Azure Cosmos-DB-databas.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-212">These aggregation examples access some flight performance data that's stored in our **DoctorWho** Azure Cosmos DB database.</span></span> <span data-ttu-id="1ae7e-213">tooconnect tooit måste tooutilize hello följande kodutdrag:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-213">tooconnect tooit, you need tooutilize hello following code snippet:</span></span>

```
// Import Spark tooAzure Cosmos DB connector
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Connect tooAzure Cosmos DB Database
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US 2;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(readConfig2)
coll.createOrReplaceTempView("c")
```

<span data-ttu-id="1ae7e-214">Den här fragment har vi också kommer toorun en grundläggande fråga att överföringar hello filtrerad uppsättning data från Azure Cosmos DB tooSpark där hello senare kan utföra distribuerade mängder.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-214">With this snippet, we are also going toorun a base query that transfers hello filtered set of data from Azure Cosmos DB tooSpark where hello latter can perform distributed aggregates.</span></span> <span data-ttu-id="1ae7e-215">I det här fallet ber vi om flygplan avviker från Seattle (SEA).</span><span class="sxs-lookup"><span data-stu-id="1ae7e-215">In this case, we are asking for flights that depart from Seattle (SEA).</span></span>

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

<span data-ttu-id="1ae7e-216">hello har följande resultat skapats genom att köra frågor hello hello Jupyter-anteckningsbok service.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-216">hello following results were generated by running hello queries from hello Jupyter notebook service.</span></span>  <span data-ttu-id="1ae7e-217">Observera att alla hello kodavsnitt är generisk och inte är specifika tooany service.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-217">Note that all hello code snippets are generic and not specific tooany service.</span></span>

### <a name="running-limit-and-count-queries"></a><span data-ttu-id="1ae7e-218">Körs GRÄNSEN och antal frågor</span><span class="sxs-lookup"><span data-stu-id="1ae7e-218">Running LIMIT and COUNT queries</span></span>
<span data-ttu-id="1ae7e-219">Precis som du är används tooin SQL/Spark SQL, låt oss börja med en **GRÄNSEN** fråga:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-219">Just like you're used tooin SQL/Spark SQL, let's start off with a **LIMIT** query:</span></span>

![Spark GRÄNSEN fråga](./media/spark-connector/spark-sql-query.png)

<span data-ttu-id="1ae7e-221">hello nästa frågan är en enkel och snabb **antal** fråga:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-221">hello next query is a simple and fast **COUNT** query:</span></span>

![Spark COUNT-frågan](./media/spark-connector/spark-count-query.png)

### <a name="group-by-query"></a><span data-ttu-id="1ae7e-223">GROUP BY-fråga</span><span class="sxs-lookup"><span data-stu-id="1ae7e-223">GROUP BY query</span></span>
<span data-ttu-id="1ae7e-224">I den här nästa, vi kan köras **GROUP BY** frågor mot vår Azure Cosmos-DB-databas:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-224">In this next set, we can easily run **GROUP BY** queries against our Azure Cosmos DB database:</span></span>

```
select destination, sum(delay) as TotalDelays
from originSEA
group by destination
order by sum(delay) desc limit 10
```

![Spark GROUP BY frågan diagram](./media/spark-connector/group-by-query-graph.png)

### <a name="distinct-order-by-query"></a><span data-ttu-id="1ae7e-226">DISTINKT ORDER BY-fråga</span><span class="sxs-lookup"><span data-stu-id="1ae7e-226">DISTINCT, ORDER BY query</span></span>
<span data-ttu-id="1ae7e-227">Och här är en **DISTINCT, ORDER BY** fråga:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-227">And here is a **DISTINCT, ORDER BY** query:</span></span>

![Spark GROUP BY frågan diagram](./media/spark-connector/order-by-query.png)

### <a name="continue-hello-flight-data-analysis"></a><span data-ttu-id="1ae7e-229">Fortsätta hello svarta dataanalys</span><span class="sxs-lookup"><span data-stu-id="1ae7e-229">Continue hello flight data analysis</span></span>
<span data-ttu-id="1ae7e-230">Du kan använda följande exempel frågor toocontinue dataanalys hello svarta hello:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-230">You can use hello following example queries toocontinue analysis of hello flight data:</span></span>

#### <a name="top-5-delayed-destinations-cities-departing-from-seattle"></a><span data-ttu-id="1ae7e-231">Vanliga 5 fördröjd mål (orter) reser från Seattle</span><span class="sxs-lookup"><span data-stu-id="1ae7e-231">Top 5 delayed destinations (cities) departing from Seattle</span></span>
```
select destination, sum(delay)
from originSEA
where delay < 0
group by destination
order by sum(delay) limit 5
```
![Spark översta fördröjningar diagram](./media/spark-connector/top-delays-graph.png)


#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a><span data-ttu-id="1ae7e-233">Beräkna median fördröjningar av mål orter reser från Seattle</span><span class="sxs-lookup"><span data-stu-id="1ae7e-233">Calculate median delays by destination cities departing from Seattle</span></span>
```
select destination, percentile_approx(delay, 0.5) as median_delay
from originSEA
where delay < 0
group by destination
order by percentile_approx(delay, 0.5)
```

![Spark median fördröjningar diagram](./media/spark-connector/median-delays-graph.png)

## <a name="next-steps"></a><span data-ttu-id="1ae7e-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1ae7e-235">Next steps</span></span>

<span data-ttu-id="1ae7e-236">Om du inte redan gjort hämta hello Spark tooAzure Cosmos DB koppling från hello [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub-lagringsplatsen och utforska hello fler resurser i hello lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="1ae7e-236">If you haven't already, download hello Spark tooAzure Cosmos DB connector from hello [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub repository and explore hello additional resources in hello repo:</span></span>

* [<span data-ttu-id="1ae7e-237">Distribuerade aggregeringar exempel</span><span class="sxs-lookup"><span data-stu-id="1ae7e-237">Distributed Aggregations Examples</span></span>](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [<span data-ttu-id="1ae7e-238">Exempel på skript och bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="1ae7e-238">Sample Scripts and Notebooks</span></span>](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

<span data-ttu-id="1ae7e-239">Du kan också tooreview hello [datauppsättningar Guide, DataFrames och Apache Spark SQL](http://spark.apache.org/docs/latest/sql-programming-guide.html) och hello [Apache Spark på Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="1ae7e-239">You might also want tooreview hello [Apache Spark SQL, DataFrames, and Datasets Guide](http://spark.apache.org/docs/latest/sql-programming-guide.html) and hello [Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) article.</span></span>
