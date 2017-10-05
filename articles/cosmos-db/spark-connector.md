---
title: Ansluta Apache Spark till Azure Cosmos DB | Microsoft Docs
description: "Använd den här självstudiekursen vill veta mer om Azure Cosmos DB Spark-koppling kan du ansluta Apache Spark i Azure Cosmos DB att utföra distribuerade aggregeringar och data sciences på flera innehavare globalt distribuerade databassystemet från Microsoft som utformad för molnet."
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
ms.openlocfilehash: 8ecbb478c81cde25bbd0d1c9ee07ae02b07f8cc7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="accelerate-real-time-big-data-analytics-with-the-spark-to-azure-cosmos-db-connector"></a><span data-ttu-id="9fc02-104">Påskynda realtid stordata med Spark på Azure DB som Cosmos-kopplingen</span><span class="sxs-lookup"><span data-stu-id="9fc02-104">Accelerate real-time big-data analytics with the Spark to Azure Cosmos DB connector</span></span>

<span data-ttu-id="9fc02-105">Spark på Azure DB som Cosmos-kopplingen kan Azure Cosmos-Databsen ska fungera som en Indatakällan eller utdatamottagaren för Apache Spark-jobb.</span><span class="sxs-lookup"><span data-stu-id="9fc02-105">The Spark to Azure Cosmos DB connector enables Azure Cosmos DB to act as an input source or output sink for Apache Spark jobs.</span></span> <span data-ttu-id="9fc02-106">Ansluta [Spark](http://spark.apache.org/) till [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) accelererar möjligheten att lösa fast flyttar datavetenskap problem där du kan använda Azure Cosmos DB snabbt bevara och fråga efter data.</span><span class="sxs-lookup"><span data-stu-id="9fc02-106">Connecting [Spark](http://spark.apache.org/) to [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) accelerates your ability to solve fast-moving data science problems where you can use Azure Cosmos DB to quickly persist and query data.</span></span> <span data-ttu-id="9fc02-107">Spark på Azure DB som Cosmos-kopplingen använder effektivt interna Azure Cosmos DB hanteras index.</span><span class="sxs-lookup"><span data-stu-id="9fc02-107">The Spark to Azure Cosmos DB connector efficiently utilizes the native Azure Cosmos DB managed indexes.</span></span> <span data-ttu-id="9fc02-108">Index aktivera uppdateringsbara kolumner när du utföra analyser och push-ned predikat filtrering mot fast föränderliga globalt distribueras data mellan Sakernas Internet (IoT) till scenarion för vetenskap och analys av data.</span><span class="sxs-lookup"><span data-stu-id="9fc02-108">The indexes enable updateable columns when you perform analytics and push-down predicate filtering against fast-changing globally distributed data, which range from Internet of Things (IoT) to data science and analytics scenarios.</span></span>

<span data-ttu-id="9fc02-109">För att arbeta med Spark GraphX och Gremlin graph API: er i Azure Cosmos DB, se [utföra diagrammet analytics med hjälp av Spark och Apache TinkerPop Gremlin](spark-connector-graph.md).</span><span class="sxs-lookup"><span data-stu-id="9fc02-109">For working with Spark GraphX and the Gremlin graph APIs of Azure Cosmos DB, see [Perform graph analytics using Spark and Apache TinkerPop Gremlin](spark-connector-graph.md).</span></span>

## <a name="download"></a><span data-ttu-id="9fc02-110">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="9fc02-110">Download</span></span>

<span data-ttu-id="9fc02-111">Kom igång genom att hämta i Spark till Azure DB som Cosmos-anslutningstjänsten (förhandsgranskning) från den [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/) databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="9fc02-111">To get started, download the Spark to Azure Cosmos DB connector (preview) from the [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/) repository on GitHub.</span></span>

## <a name="connector-components"></a><span data-ttu-id="9fc02-112">Connector-komponenter</span><span class="sxs-lookup"><span data-stu-id="9fc02-112">Connector components</span></span>

<span data-ttu-id="9fc02-113">Anslutningen använder följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="9fc02-113">The connector utilizes the following components:</span></span>

* <span data-ttu-id="9fc02-114">[Azure Cosmos-DB](http://documentdb.com) ger kunder möjlighet att etablera och skala Elastiskt både dataflöden och lagringsutrymmen till alla geografiska regioner.</span><span class="sxs-lookup"><span data-stu-id="9fc02-114">[Azure Cosmos DB](http://documentdb.com) enables customers to provision and elastically scale both throughput and storage across any number of geographical regions.</span></span> <span data-ttu-id="9fc02-115">Tjänsten innehåller:</span><span class="sxs-lookup"><span data-stu-id="9fc02-115">The service offers:</span></span>
   * <span data-ttu-id="9fc02-116">Aktivera nyckeln [global distributionsplatsen](distribute-data-globally.md) och skala horisontellt</span><span class="sxs-lookup"><span data-stu-id="9fc02-116">Turn key [global distribution](distribute-data-globally.md) and horizontal scale</span></span>
   * <span data-ttu-id="9fc02-117">Garanteras siffra fördröjningar vid 99th percentilen</span><span class="sxs-lookup"><span data-stu-id="9fc02-117">Guaranteed single digit latencies at the 99th percentile</span></span>
   * [<span data-ttu-id="9fc02-118">Flera väldefinierade konsekvenskontroll modeller</span><span class="sxs-lookup"><span data-stu-id="9fc02-118">Multiple well-defined consistency models</span></span>](consistency-levels.md)
   * <span data-ttu-id="9fc02-119">Garanterat hög tillgänglighet med flera funktioner</span><span class="sxs-lookup"><span data-stu-id="9fc02-119">Guaranteed high availability with multi-homing capabilities</span></span>
   * <span data-ttu-id="9fc02-120">Alla funktioner backas upp av branschledande, omfattande [serviceavtal](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLA).</span><span class="sxs-lookup"><span data-stu-id="9fc02-120">All features are backed by industry-leading, comprehensive [service level agreements](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLAs).</span></span>

* <span data-ttu-id="9fc02-121">[Apache Spark](http://spark.apache.org/) är en kraftfull öppen källkod bearbetningsmotor som är uppbyggd kring hastighet, enkel användning och avancerade analyser.</span><span class="sxs-lookup"><span data-stu-id="9fc02-121">[Apache Spark](http://spark.apache.org/) is a powerful open source processing engine that's built around speed, ease of use, and sophisticated analytics.</span></span>

* <span data-ttu-id="9fc02-122">[Apache Spark på Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) så att du kan distribuera Apache Spark i molnet för verksamhetskritiska distributioner med hjälp av [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span><span class="sxs-lookup"><span data-stu-id="9fc02-122">[Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) so that you can deploy Apache Spark in the cloud for mission-critical deployments by using [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span></span>

<span data-ttu-id="9fc02-123">Officiellt versioner som stöds:</span><span class="sxs-lookup"><span data-stu-id="9fc02-123">Officially supported versions:</span></span>

| <span data-ttu-id="9fc02-124">Komponent</span><span class="sxs-lookup"><span data-stu-id="9fc02-124">Component</span></span> | <span data-ttu-id="9fc02-125">Version</span><span class="sxs-lookup"><span data-stu-id="9fc02-125">Version</span></span> |
|---------|-------|
|<span data-ttu-id="9fc02-126">Apache Spark</span><span class="sxs-lookup"><span data-stu-id="9fc02-126">Apache Spark</span></span>|<span data-ttu-id="9fc02-127">2.0+</span><span class="sxs-lookup"><span data-stu-id="9fc02-127">2.0+</span></span>|
| <span data-ttu-id="9fc02-128">Scala</span><span class="sxs-lookup"><span data-stu-id="9fc02-128">Scala</span></span>| <span data-ttu-id="9fc02-129">2.11</span><span class="sxs-lookup"><span data-stu-id="9fc02-129">2.11</span></span>|
| <span data-ttu-id="9fc02-130">Azure DocumentDB Java SDK</span><span class="sxs-lookup"><span data-stu-id="9fc02-130">Azure DocumentDB Java SDK</span></span> | <span data-ttu-id="9fc02-131">1.10.0</span><span class="sxs-lookup"><span data-stu-id="9fc02-131">1.10.0</span></span> |

<span data-ttu-id="9fc02-132">Den här artikeln hjälper dig att köra några enkla exempel med hjälp av Python (via pyDocumentDB) och Scala-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="9fc02-132">This article helps you run some simple samples by using Python (via pyDocumentDB) and the Scala interfaces.</span></span>

<span data-ttu-id="9fc02-133">Det finns två sätt att ansluta Apache Spark och Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="9fc02-133">There are two approaches to connect Apache Spark and Azure Cosmos DB:</span></span>
- <span data-ttu-id="9fc02-134">Använd pyDocumentDB via den [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).</span><span class="sxs-lookup"><span data-stu-id="9fc02-134">Use pyDocumentDB via the [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).</span></span>
- <span data-ttu-id="9fc02-135">Skapa en Java-baserad Spark på Azure Cosmos DB kopplingen genom att använda den [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).</span><span class="sxs-lookup"><span data-stu-id="9fc02-135">Create a Java-based Spark to Azure Cosmos DB connector by utilizing the [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).</span></span>

## <a name="pydocumentdb-implementation"></a><span data-ttu-id="9fc02-136">pyDocumentDB implementering</span><span class="sxs-lookup"><span data-stu-id="9fc02-136">pyDocumentDB implementation</span></span>
<span data-ttu-id="9fc02-137">Aktuellt [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) kan du ansluta Spark i Azure Cosmos DB som visas i följande diagram:</span><span class="sxs-lookup"><span data-stu-id="9fc02-137">The current [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) enables you to connect Spark to Azure Cosmos DB as shown in the following diagram:</span></span>

![Väck på Azure Cosmos DB dataflöde via pyDocumentDB DB](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow-of-the-pydocumentdb-implementation"></a><span data-ttu-id="9fc02-139">Dataflöde för pyDocumentDB-implementering</span><span class="sxs-lookup"><span data-stu-id="9fc02-139">Data flow of the pyDocumentDB implementation</span></span>

<span data-ttu-id="9fc02-140">Dataflödet är följande:</span><span class="sxs-lookup"><span data-stu-id="9fc02-140">The data flow is as follows:</span></span>

1. <span data-ttu-id="9fc02-141">Noden som Spark ansluter till Azure Cosmos DB gateway-noden via pyDocumentDB.</span><span class="sxs-lookup"><span data-stu-id="9fc02-141">The Spark master node connects to the Azure Cosmos DB gateway node via pyDocumentDB.</span></span> <span data-ttu-id="9fc02-142">En användare anger endast Spark och Azure Cosmos DB-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="9fc02-142">A user specifies only the Spark and Azure Cosmos DB connections.</span></span> <span data-ttu-id="9fc02-143">Anslutningar till respektive huvud- och gateway-noderna är transparent för användaren.</span><span class="sxs-lookup"><span data-stu-id="9fc02-143">Connections to the respective master and gateway nodes are transparent to the user.</span></span>
2. <span data-ttu-id="9fc02-144">Gateway-noden gör frågan mot Azure Cosmos DB där frågan därefter körs mot mängdens partitioner i datanoder.</span><span class="sxs-lookup"><span data-stu-id="9fc02-144">The gateway node makes the query against Azure Cosmos DB where the query subsequently runs against the collection's partitions in the data nodes.</span></span> <span data-ttu-id="9fc02-145">Svaret på dessa frågor skickas tillbaka till gateway-noden och att resultatet returneras till huvudnoden Spark.</span><span class="sxs-lookup"><span data-stu-id="9fc02-145">The response for those queries is sent back to the gateway node, and that result set is returned to the Spark master node.</span></span>
3. <span data-ttu-id="9fc02-146">Efterföljande frågor (till exempel mot ett Spark-DataFrame) skickas till arbetsnoder Spark för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="9fc02-146">Subsequent queries (for example, against a Spark DataFrame) are sent to the Spark worker nodes for processing.</span></span>

<span data-ttu-id="9fc02-147">Kommunikation mellan Spark och Azure Cosmos DB är begränsad till Spark-huvudnod och Azure Cosmos DB gateway-noder.</span><span class="sxs-lookup"><span data-stu-id="9fc02-147">Communication between Spark and Azure Cosmos DB is limited to the Spark master node and Azure Cosmos DB gateway nodes.</span></span>  <span data-ttu-id="9fc02-148">Frågorna går snabbt Transportskiktet mellan dessa två noder kan.</span><span class="sxs-lookup"><span data-stu-id="9fc02-148">The queries go as fast as the transport layer between these two nodes allows.</span></span>

### <a name="install-pydocumentdb"></a><span data-ttu-id="9fc02-149">Installera pyDocumentDB</span><span class="sxs-lookup"><span data-stu-id="9fc02-149">Install pyDocumentDB</span></span>
<span data-ttu-id="9fc02-150">Du kan installera pyDocumentDB på Drivrutinsnoden med hjälp av **pip**, till exempel:</span><span class="sxs-lookup"><span data-stu-id="9fc02-150">You can install pyDocumentDB on your driver node by using **pip**, for example:</span></span>

```
pip install pyDocumentDB
```


### <a name="connect-spark-to-azure-cosmos-db-via-pydocumentdb"></a><span data-ttu-id="9fc02-151">Ansluta Spark i Azure Cosmos DB via pyDocumentDB</span><span class="sxs-lookup"><span data-stu-id="9fc02-151">Connect Spark to Azure Cosmos DB via pyDocumentDB</span></span>
<span data-ttu-id="9fc02-152">Enkelhet kommunikation transport gör körningen av en fråga från Spark i Azure Cosmos DB genom att använda relativt enkla pyDocumentDB.</span><span class="sxs-lookup"><span data-stu-id="9fc02-152">The simplicity of the communication transport makes execution of a query from Spark to Azure Cosmos DB by using pyDocumentDB relatively simple.</span></span>

<span data-ttu-id="9fc02-153">Följande kodavsnitt visar hur du använder pyDocumentDB i en kontext som Spark.</span><span class="sxs-lookup"><span data-stu-id="9fc02-153">The following code snippet shows how to use pyDocumentDB in a Spark context.</span></span>

```
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring the connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery
connectionPolicy.PreferredLocations = ["Central US", "East US 2", "Southeast Asia", "Western Europe","Canada Central"]


# Set keys to connect to Azure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)
```

<span data-ttu-id="9fc02-154">Enligt beskrivningen i kodfragmentet:</span><span class="sxs-lookup"><span data-stu-id="9fc02-154">As noted in the code snippet:</span></span>

* <span data-ttu-id="9fc02-155">Azure Cosmos DB Python SDK (`pyDocumentDB`) innehåller alla de nödvändiga anslutningsparametrarna.</span><span class="sxs-lookup"><span data-stu-id="9fc02-155">The Azure Cosmos DB Python SDK (`pyDocumentDB`) contains the all the necessary connection parameters.</span></span> <span data-ttu-id="9fc02-156">Parametern önskade platser väljer exempelvis skrivskyddade replik och prioritet ordningsföljden.</span><span class="sxs-lookup"><span data-stu-id="9fc02-156">For example, the preferred locations parameter chooses the read replica and priority order.</span></span>
*  <span data-ttu-id="9fc02-157">Importera de nödvändiga bibliotek och konfigurera din **masterKey** och **värden** att skapa Azure Cosmos DB *klienten* (**pydocumentdb.document_client** ).</span><span class="sxs-lookup"><span data-stu-id="9fc02-157">Import the necessary libraries and configure your **masterKey** and **host** to create the Azure Cosmos DB *client* (**pydocumentdb.document_client**).</span></span>


### <a name="execute-spark-queries-via-pydocumentdb"></a><span data-ttu-id="9fc02-158">Köra Spark-frågor via pyDocumentDB</span><span class="sxs-lookup"><span data-stu-id="9fc02-158">Execute Spark Queries via pyDocumentDB</span></span>
<span data-ttu-id="9fc02-159">I följande exempel används Azure DB som Cosmos-instans som skapades i föregående fragment med angivna nycklar i skrivskyddat läge.</span><span class="sxs-lookup"><span data-stu-id="9fc02-159">The following examples use the Azure Cosmos DB instance that was created in the previous snippet by using the specified read-only keys.</span></span> <span data-ttu-id="9fc02-160">Följande kodavsnitt som ansluter till den **airports.codes** samling i DoctorWho kontot som angavs tidigare och kör en fråga om du vill extrahera flygplats orter i staten Washington.</span><span class="sxs-lookup"><span data-stu-id="9fc02-160">The following code snippet connects to the **airports.codes** collection in the DoctorWho account as specified earlier and runs a query to extract the airport cities in Washington state.</span></span>

```
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations the Azure Cosmos DB client will use to connect to the database and collection
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

<span data-ttu-id="9fc02-161">Efter att frågan har utförts **frågan**, resultatet är en **query_iterable. QueryIterable** som konverteras till en Python-lista.</span><span class="sxs-lookup"><span data-stu-id="9fc02-161">After the query has been executed via **query**, the result is a **query_iterable.QueryIterable** that is converted to a Python list.</span></span> <span data-ttu-id="9fc02-162">En lista med Python kan enkelt omvandlas till ett Spark-DataFrame med hjälp av följande kod:</span><span class="sxs-lookup"><span data-stu-id="9fc02-162">A Python list can be easily converted to a Spark DataFrame by using the following code:</span></span>

```
# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(elements)
```

### <a name="why-use-the-pydocumentdb-to-connect-spark-to-azure-cosmos-db"></a><span data-ttu-id="9fc02-163">Varför ska jag använda pyDocumentDB ansluta Spark i Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="9fc02-163">Why use the pyDocumentDB to connect Spark to Azure Cosmos DB?</span></span>
<span data-ttu-id="9fc02-164">Ansluter Spark i Azure Cosmos DB med hjälp av pyDocumentDB är vanligtvis för scenarier där:</span><span class="sxs-lookup"><span data-stu-id="9fc02-164">Connecting Spark to Azure Cosmos DB by using pyDocumentDB is typically for scenarios where:</span></span>

* <span data-ttu-id="9fc02-165">Du vill använda Python.</span><span class="sxs-lookup"><span data-stu-id="9fc02-165">You want to use Python.</span></span>
* <span data-ttu-id="9fc02-166">Du returnerar relativt små resultat från Azure Cosmos DB till Spark.</span><span class="sxs-lookup"><span data-stu-id="9fc02-166">You are returning a relatively small result set from Azure Cosmos DB to Spark.</span></span> <span data-ttu-id="9fc02-167">Observera att den underliggande datamängden i Azure Cosmos DB kan vara ganska stor.</span><span class="sxs-lookup"><span data-stu-id="9fc02-167">Note that the underlying dataset in Azure Cosmos DB can be quite large.</span></span> <span data-ttu-id="9fc02-168">Du kopplar filter, det vill säga kör predikat filter mot dina Azure Cosmos DB-datakälla.</span><span class="sxs-lookup"><span data-stu-id="9fc02-168">You are applying filters, that is, running predicate filters, against your Azure Cosmos DB source.</span></span>  

## <a name="spark-to-azure-cosmos-db-connector"></a><span data-ttu-id="9fc02-169">Väck till Azure DB som Cosmos-koppling</span><span class="sxs-lookup"><span data-stu-id="9fc02-169">Spark to Azure Cosmos DB connector</span></span>

<span data-ttu-id="9fc02-170">Spark på Azure DB som Cosmos-kopplingen använder den [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) och flyttar data mellan Spark arbetarnoder och Azure Cosmos DB som visas i följande diagram:</span><span class="sxs-lookup"><span data-stu-id="9fc02-170">The Spark to Azure Cosmos DB connector utilizes the [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) and moves data between the Spark worker nodes and Azure Cosmos DB as shown in the following diagram:</span></span>

![Dataflödet i Spark på Azure DB som Cosmos-kopplingen](./media/spark-connector/spark-connector.png)

<span data-ttu-id="9fc02-172">Dataflödet är följande:</span><span class="sxs-lookup"><span data-stu-id="9fc02-172">The data flow is as follows:</span></span>

1. <span data-ttu-id="9fc02-173">Noden som Spark ansluter till Azure Cosmos DB gateway-noden för att hämta partitionen kartan.</span><span class="sxs-lookup"><span data-stu-id="9fc02-173">The Spark master node connects to the Azure Cosmos DB gateway node to obtain the partition map.</span></span> <span data-ttu-id="9fc02-174">En användare anger endast Spark och Azure Cosmos DB-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="9fc02-174">A user specifies only the Spark and Azure Cosmos DB connections.</span></span> <span data-ttu-id="9fc02-175">Anslutningar till respektive huvud- och gateway-noderna är transparent för användaren.</span><span class="sxs-lookup"><span data-stu-id="9fc02-175">Connections to the respective master and gateway nodes are transparent to the user.</span></span>
2. <span data-ttu-id="9fc02-176">Den här informationen tillbaka till noden som Spark.</span><span class="sxs-lookup"><span data-stu-id="9fc02-176">This information is provided back to the Spark master node.</span></span>  <span data-ttu-id="9fc02-177">Du bör nu kunna parsa frågan för att fastställa partitionerna och deras platser i Azure Cosmos-databasen som du behöver åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="9fc02-177">At this point, you should be able to parse the query to determine the partitions and their locations in Azure Cosmos DB that you need to access.</span></span>
3. <span data-ttu-id="9fc02-178">Den här informationen skickas till arbetsnoder Spark.</span><span class="sxs-lookup"><span data-stu-id="9fc02-178">This information is transmitted to the Spark worker nodes.</span></span>
4. <span data-ttu-id="9fc02-179">Arbetsnoderna Spark ansluta till Azure Cosmos DB partitioner direkt för att extrahera data och returnera data till Spark-partitioner i Spark arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="9fc02-179">The Spark worker nodes connect to the Azure Cosmos DB partitions directly to extract the data and return the data to the Spark partitions in the Spark worker nodes.</span></span>

<span data-ttu-id="9fc02-180">Kommunikation mellan Spark och Azure Cosmos DB är betydligt snabbare eftersom data flyttas mellan arbetarnoder Spark och Azure Cosmos DB datanoder (partitioner).</span><span class="sxs-lookup"><span data-stu-id="9fc02-180">Communication between Spark and Azure Cosmos DB is significantly faster because the data movement is between the Spark worker nodes and the Azure Cosmos DB data nodes (partitions).</span></span>

### <a name="build-the-spark-to-azure-cosmos-db-connector"></a><span data-ttu-id="9fc02-181">Skapa Spark på Azure DB som Cosmos-kopplingen</span><span class="sxs-lookup"><span data-stu-id="9fc02-181">Build the Spark to Azure Cosmos DB connector</span></span>
<span data-ttu-id="9fc02-182">För närvarande använder connector projektet maven.</span><span class="sxs-lookup"><span data-stu-id="9fc02-182">Currently, the connector project uses maven.</span></span> <span data-ttu-id="9fc02-183">Du kan köra för att skapa anslutningen utan beroenden:</span><span class="sxs-lookup"><span data-stu-id="9fc02-183">To build the connector without dependencies, you can run:</span></span>
```
mvn clean package
```
<span data-ttu-id="9fc02-184">Du kan också hämta de senaste versionerna av JAR från den *släpper* mapp.</span><span class="sxs-lookup"><span data-stu-id="9fc02-184">You can also download the latest versions of the JAR from the *releases* folder.</span></span>

### <a name="include-the-azure-cosmos-db-spark-jar"></a><span data-ttu-id="9fc02-185">Inkludera Azure Cosmos DB Spark JAR</span><span class="sxs-lookup"><span data-stu-id="9fc02-185">Include the Azure Cosmos DB Spark JAR</span></span>
<span data-ttu-id="9fc02-186">Innan du kan köra all kod som du behöver inkludera JAR till Spark för Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9fc02-186">Before you execute any code, you need to include the Azure Cosmos DB Spark JAR.</span></span>  <span data-ttu-id="9fc02-187">Om du använder den **spark-shell**, och du kan inkludera JAR med hjälp av den **--burkar** alternativet.</span><span class="sxs-lookup"><span data-stu-id="9fc02-187">If you are using the **spark-shell**, then you can include the JAR by using the **--jars** option.</span></span>  

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3-jar-with-dependencies.jar
```

<span data-ttu-id="9fc02-188">Om du vill köra JAR utan beroenden, använder du följande kod:</span><span class="sxs-lookup"><span data-stu-id="9fc02-188">If you want to execute the JAR without dependencies, use the following code:</span></span>

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3.jar,/$location/azure-documentdb-1.10.0.jar
```

<span data-ttu-id="9fc02-189">Om du använder en bärbar dator tjänst, till exempel Azure HDInsight Jupyter-anteckningsbok service kan du använda den **Väck magic** kommandon:</span><span class="sxs-lookup"><span data-stu-id="9fc02-189">If you are using a notebook service such as Azure HDInsight Jupyter notebook service, you can use the **spark magic** commands:</span></span>

```
%%configure
{ "jars": ["wasb:///example/jars/azure-documentdb-1.10.0.jar","wasb:///example/jars/azure-cosmosdb-spark-0.0.3.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

<span data-ttu-id="9fc02-190">Den **burkar** kommandot kan du inkludera två burkar som behövs för **azure cosmosdb spark** (sig själv och Azure DocumentDB Java SDK) och utelämna **scala-återspeglar** så att den inte stör Livius anropar (Jupyter-anteckningsbok > Livius > Spark).</span><span class="sxs-lookup"><span data-stu-id="9fc02-190">The **jars** command enables you to include the two JARs that are needed for **azure-cosmosdb-spark** (itself and the Azure DocumentDB Java SDK) and exclude **scala-reflect** so that it does not interfere with the Livy calls (Jupyter notebook > Livy > Spark).</span></span>

### <a name="connect-spark-to-azure-cosmos-db-using-the-connector"></a><span data-ttu-id="9fc02-191">Ansluta Spark till Azure Cosmos-databasen med anslutningen</span><span class="sxs-lookup"><span data-stu-id="9fc02-191">Connect Spark to Azure Cosmos DB using the connector</span></span>
<span data-ttu-id="9fc02-192">Även om kommunikationen transporten är lite mer komplicerad, är kör en fråga från Spark i Azure Cosmos DB med hjälp av anslutningen betydligt snabbare.</span><span class="sxs-lookup"><span data-stu-id="9fc02-192">Although the communication transport is a little more complicated, executing a query from Spark to Azure Cosmos DB by using the connector is significantly faster.</span></span>

<span data-ttu-id="9fc02-193">Följande kodavsnitt visar hur du använder kopplingen i en kontext som Spark.</span><span class="sxs-lookup"><span data-stu-id="9fc02-193">The following code snippet shows how to use the connector in a Spark context.</span></span>

```
// Import Necessary Libraries
import org.joda.time._
import org.joda.time.format._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Configure connection to your collection
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

<span data-ttu-id="9fc02-194">Enligt beskrivningen i kodfragmentet:</span><span class="sxs-lookup"><span data-stu-id="9fc02-194">As noted in the code snippet:</span></span>

- <span data-ttu-id="9fc02-195">**Azure cosmosdb spark** innehåller alla de nödvändiga anslutningsparametrarna, bland annat de primära platserna.</span><span class="sxs-lookup"><span data-stu-id="9fc02-195">**azure-cosmosdb-spark** contains the all the necessary connection parameters, which include the preferred locations.</span></span> <span data-ttu-id="9fc02-196">Exempelvis kan du välja den skrivskyddade replik och prioritet ordningen.</span><span class="sxs-lookup"><span data-stu-id="9fc02-196">For example, you can choose the read replica and priority order.</span></span>
- <span data-ttu-id="9fc02-197">Bara importera de nödvändiga bibliotek och konfigurera masterKey och värden för att skapa Azure DB som Cosmos-klienten.</span><span class="sxs-lookup"><span data-stu-id="9fc02-197">Just import the necessary libraries and configure your masterKey and host to create the Azure Cosmos DB client.</span></span>

### <a name="execute-spark-queries-via-the-connector"></a><span data-ttu-id="9fc02-198">Köra Spark-frågor via connector</span><span class="sxs-lookup"><span data-stu-id="9fc02-198">Execute Spark queries via the connector</span></span>

<span data-ttu-id="9fc02-199">I följande exempel används Azure DB som Cosmos-instans som skapades i föregående fragment med angivna nycklar i skrivskyddat läge.</span><span class="sxs-lookup"><span data-stu-id="9fc02-199">The following example uses the Azure Cosmos DB instance that was created in the previous snippet by using the specified read-only keys.</span></span> <span data-ttu-id="9fc02-200">Följande kodavsnitt ansluter till samlingen DepartureDelays.flights_pcoll (i det DoctorWho kontot som angavs tidigare) och kör en fråga om du vill extrahera flyginformation för fördröjning av flygplan reser från Seattle.</span><span class="sxs-lookup"><span data-stu-id="9fc02-200">The following code snippet connects to the DepartureDelays.flights_pcoll collection (in the DoctorWho account as specified earlier) and runs a query to extract the flight delay information of flights that are departing from Seattle.</span></span>

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-the-spark-to-azure-cosmos-db-connector-implementation"></a><span data-ttu-id="9fc02-201">Varför använda Spark Azure Cosmos DB-implementeringen för anslutningen?</span><span class="sxs-lookup"><span data-stu-id="9fc02-201">Why use the Spark to Azure Cosmos DB connector implementation?</span></span>

<span data-ttu-id="9fc02-202">Ansluta Spark i Azure Cosmos DB med hjälp av anslutningen är vanligtvis för scenarier där:</span><span class="sxs-lookup"><span data-stu-id="9fc02-202">Connecting Spark to Azure Cosmos DB by using the connector is typically for scenarios where:</span></span>

* <span data-ttu-id="9fc02-203">Du vill använda Scala och uppdatera den om du vill inkludera en Python-omslutning som anges i [problemet 3: lägga till Python wrapper och exempel](https://github.com/Azure/azure-cosmosdb-spark/issues/3).</span><span class="sxs-lookup"><span data-stu-id="9fc02-203">You want to use Scala and update it to include a Python wrapper as noted in [Issue 3: Add Python wrapper and examples](https://github.com/Azure/azure-cosmosdb-spark/issues/3).</span></span>
* <span data-ttu-id="9fc02-204">Du har en stor mängd data för överföring mellan Apache Spark och Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9fc02-204">You have a large amount of data to transfer between Apache Spark and Azure Cosmos DB.</span></span>

<span data-ttu-id="9fc02-205">För att ge dig en uppfattning om frågan prestandaskillnaden, finns det [frågan Testkörningar wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).</span><span class="sxs-lookup"><span data-stu-id="9fc02-205">To give you an idea of the query performance difference, see the [Query Test Runs wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).</span></span>

## <a name="distributed-aggregation-example"></a><span data-ttu-id="9fc02-206">Distribuerade aggregeringsexempel</span><span class="sxs-lookup"><span data-stu-id="9fc02-206">Distributed aggregation example</span></span>
<span data-ttu-id="9fc02-207">Det här avsnittet innehåller några exempel på hur du kan göra distribuerade aggregeringar och analytics med hjälp av Apache Spark och Azure Cosmos DB tillsammans.</span><span class="sxs-lookup"><span data-stu-id="9fc02-207">This section provides some examples of how you can do distributed aggregations and analytics by using Apache Spark and Azure Cosmos DB together.</span></span> <span data-ttu-id="9fc02-208">Azure Cosmos-DB stöder redan aggregeringar, som beskrivs i den [planeten skala mängder med Azure Cosmos DB blogg](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/).</span><span class="sxs-lookup"><span data-stu-id="9fc02-208">Azure Cosmos DB already supports aggregations, which is discussed in the [Planet scale aggregates with Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/).</span></span> <span data-ttu-id="9fc02-209">Här är hur du kan dra den till nästa nivå med Apache Spark.</span><span class="sxs-lookup"><span data-stu-id="9fc02-209">Here is how you can take it to the next level with Apache Spark.</span></span>

<span data-ttu-id="9fc02-210">Observera att dessa aggregeringar är reference till den [Spark till Azure Cosmos DB Connector anteckningsboken](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).</span><span class="sxs-lookup"><span data-stu-id="9fc02-210">Note that these aggregations are in reference to the [Spark to Azure Cosmos DB Connector notebook](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).</span></span>

### <a name="connect-to-flights-sample-data"></a><span data-ttu-id="9fc02-211">Ansluta till flygplan exempeldata</span><span class="sxs-lookup"><span data-stu-id="9fc02-211">Connect to flights sample data</span></span>
<span data-ttu-id="9fc02-212">Exemplen aggregering åtkomst till vissa svarta prestandadata som är lagrad i vår **DoctorWho** Azure Cosmos-DB-databas.</span><span class="sxs-lookup"><span data-stu-id="9fc02-212">These aggregation examples access some flight performance data that's stored in our **DoctorWho** Azure Cosmos DB database.</span></span> <span data-ttu-id="9fc02-213">För att ansluta till den, måste du använda följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="9fc02-213">To connect to it, you need to utilize the following code snippet:</span></span>

```
// Import Spark to Azure Cosmos DB connector
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Connect to Azure Cosmos DB Database
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

<span data-ttu-id="9fc02-214">Med den här fragment kommer vi också att köra en bas-fråga som överför en filtrerad uppsättning data från Azure Cosmos DB till Spark där denna kan utföra distribuerade mängder.</span><span class="sxs-lookup"><span data-stu-id="9fc02-214">With this snippet, we are also going to run a base query that transfers the filtered set of data from Azure Cosmos DB to Spark where the latter can perform distributed aggregates.</span></span> <span data-ttu-id="9fc02-215">I det här fallet ber vi om flygplan avviker från Seattle (SEA).</span><span class="sxs-lookup"><span data-stu-id="9fc02-215">In this case, we are asking for flights that depart from Seattle (SEA).</span></span>

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

<span data-ttu-id="9fc02-216">Följande resultat har skapats genom att köra frågor från Jupyter-anteckningsbok tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9fc02-216">The following results were generated by running the queries from the Jupyter notebook service.</span></span>  <span data-ttu-id="9fc02-217">Observera att alla kodavsnitt är generisk och inte är specifika för varje tjänst.</span><span class="sxs-lookup"><span data-stu-id="9fc02-217">Note that all the code snippets are generic and not specific to any service.</span></span>

### <a name="running-limit-and-count-queries"></a><span data-ttu-id="9fc02-218">Körs GRÄNSEN och antal frågor</span><span class="sxs-lookup"><span data-stu-id="9fc02-218">Running LIMIT and COUNT queries</span></span>
<span data-ttu-id="9fc02-219">Precis som du ofta i SQL/Spark SQL, låt oss börja med en **GRÄNSEN** fråga:</span><span class="sxs-lookup"><span data-stu-id="9fc02-219">Just like you're used to in SQL/Spark SQL, let's start off with a **LIMIT** query:</span></span>

![Spark GRÄNSEN fråga](./media/spark-connector/spark-sql-query.png)

<span data-ttu-id="9fc02-221">Nästa fråga är en enkel och snabb **antal** fråga:</span><span class="sxs-lookup"><span data-stu-id="9fc02-221">The next query is a simple and fast **COUNT** query:</span></span>

![Spark COUNT-frågan](./media/spark-connector/spark-count-query.png)

### <a name="group-by-query"></a><span data-ttu-id="9fc02-223">GROUP BY-fråga</span><span class="sxs-lookup"><span data-stu-id="9fc02-223">GROUP BY query</span></span>
<span data-ttu-id="9fc02-224">I den här nästa, vi kan köras **GROUP BY** frågor mot vår Azure Cosmos-DB-databas:</span><span class="sxs-lookup"><span data-stu-id="9fc02-224">In this next set, we can easily run **GROUP BY** queries against our Azure Cosmos DB database:</span></span>

```
select destination, sum(delay) as TotalDelays
from originSEA
group by destination
order by sum(delay) desc limit 10
```

![Spark GROUP BY frågan diagram](./media/spark-connector/group-by-query-graph.png)

### <a name="distinct-order-by-query"></a><span data-ttu-id="9fc02-226">DISTINKT ORDER BY-fråga</span><span class="sxs-lookup"><span data-stu-id="9fc02-226">DISTINCT, ORDER BY query</span></span>
<span data-ttu-id="9fc02-227">Och här är en **DISTINCT, ORDER BY** fråga:</span><span class="sxs-lookup"><span data-stu-id="9fc02-227">And here is a **DISTINCT, ORDER BY** query:</span></span>

![Spark GROUP BY frågan diagram](./media/spark-connector/order-by-query.png)

### <a name="continue-the-flight-data-analysis"></a><span data-ttu-id="9fc02-229">Fortsätta svarta dataanalys</span><span class="sxs-lookup"><span data-stu-id="9fc02-229">Continue the flight data analysis</span></span>
<span data-ttu-id="9fc02-230">Du kan använda följande Exempelfrågor för att fortsätta analys av data rör sig:</span><span class="sxs-lookup"><span data-stu-id="9fc02-230">You can use the following example queries to continue analysis of the flight data:</span></span>

#### <a name="top-5-delayed-destinations-cities-departing-from-seattle"></a><span data-ttu-id="9fc02-231">Vanliga 5 fördröjd mål (orter) reser från Seattle</span><span class="sxs-lookup"><span data-stu-id="9fc02-231">Top 5 delayed destinations (cities) departing from Seattle</span></span>
```
select destination, sum(delay)
from originSEA
where delay < 0
group by destination
order by sum(delay) limit 5
```
![Spark översta fördröjningar diagram](./media/spark-connector/top-delays-graph.png)


#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a><span data-ttu-id="9fc02-233">Beräkna median fördröjningar av mål orter reser från Seattle</span><span class="sxs-lookup"><span data-stu-id="9fc02-233">Calculate median delays by destination cities departing from Seattle</span></span>
```
select destination, percentile_approx(delay, 0.5) as median_delay
from originSEA
where delay < 0
group by destination
order by percentile_approx(delay, 0.5)
```

![Spark median fördröjningar diagram](./media/spark-connector/median-delays-graph.png)

## <a name="next-steps"></a><span data-ttu-id="9fc02-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9fc02-235">Next steps</span></span>

<span data-ttu-id="9fc02-236">Om du inte redan gjort ladda ned Spark till Azure DB som Cosmos-anslutningen från den [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub-lagringsplatsen och utforska ytterligare resurser i lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="9fc02-236">If you haven't already, download the Spark to Azure Cosmos DB connector from the [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub repository and explore the additional resources in the repo:</span></span>

* [<span data-ttu-id="9fc02-237">Distribuerade aggregeringar exempel</span><span class="sxs-lookup"><span data-stu-id="9fc02-237">Distributed Aggregations Examples</span></span>](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [<span data-ttu-id="9fc02-238">Exempel på skript och bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="9fc02-238">Sample Scripts and Notebooks</span></span>](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

<span data-ttu-id="9fc02-239">Du kanske också vill granska den [datauppsättningar Guide, DataFrames och Apache Spark SQL](http://spark.apache.org/docs/latest/sql-programming-guide.html) och [Apache Spark på Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="9fc02-239">You might also want to review the [Apache Spark SQL, DataFrames, and Datasets Guide](http://spark.apache.org/docs/latest/sql-programming-guide.html) and the [Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) article.</span></span>
