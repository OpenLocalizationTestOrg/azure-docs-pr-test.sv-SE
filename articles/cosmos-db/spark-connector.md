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
# <a name="accelerate-real-time-big-data-analytics-with-hello-spark-tooazure-cosmos-db-connector"></a>Påskynda realtid stordata med hello Spark tooAzure Cosmos DB connector

hello Spark tooAzure Cosmos DB connector kan Azure Cosmos DB tooact som Indatakällan eller utdatamottagaren för Apache Spark-jobb. Ansluta [Spark](http://spark.apache.org/) för[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) accelererar din möjlighet toosolve fast flytta datavetenskap problem där du kan använda Azure Cosmos DB tooquickly kvarstår och fråga efter data. hello Spark tooAzure Cosmos DB anslutningen använder effektivt hello interna Azure Cosmos DB hanteras index. hello index aktivera uppdateringsbara kolumner när du utföra analyser och push-ned predikat filtrering mot fast föränderliga globalt distribuerade data mellan scenarier i Sakernas Internet (IoT) toodata vetenskap och analyser.

För att arbeta med Spark GraphX och hello Gremlin graph API: er i Azure Cosmos DB, se [utföra diagrammet analytics med hjälp av Spark och Apache TinkerPop Gremlin](spark-connector-graph.md).

## <a name="download"></a>Ladda ned

tooget igång genom att hämta hello Spark tooAzure Cosmos DB connector (förhandsgranskning) från hello [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/) databasen på GitHub.

## <a name="connector-components"></a>Connector-komponenter

hello-anslutningen använder hello följande komponenter:

* [Azure Cosmos-DB](http://documentdb.com) gör att kunder tooprovision och skala Elastiskt både dataflöden och lagringsutrymmen till alla geografiska regioner. hello-tjänsten erbjuder:
   * Aktivera nyckeln [global distributionsplatsen](distribute-data-globally.md) och skala horisontellt
   * Garanteras siffra fördröjningar vid hello 99th percentilen
   * [Flera väldefinierade konsekvenskontroll modeller](consistency-levels.md)
   * Garanterat hög tillgänglighet med flera funktioner
   * Alla funktioner backas upp av branschledande, omfattande [serviceavtal](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLA).

* [Apache Spark](http://spark.apache.org/) är en kraftfull öppen källkod bearbetningsmotor som är uppbyggd kring hastighet, enkel användning och avancerade analyser.

* [Apache Spark på Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) så att du kan distribuera Apache Spark i hello molnet för verksamhetskritiska distributioner med hjälp av [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).

Officiellt versioner som stöds:

| Komponent | Version |
|---------|-------|
|Apache Spark|2.0+|
| Scala| 2.11|
| Azure DocumentDB Java SDK | 1.10.0 |

Den här artikeln hjälper dig att köra några enkla exempel med hjälp av Python (via pyDocumentDB) och hello Scala gränssnitt.

Det finns två tillvägagångssätt tooconnect Apache Spark och Azure Cosmos DB:
- Använd pyDocumentDB via hello [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).
- Skapa en Java-baserad Spark tooAzure Cosmos-DB-koppling genom att använda hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).

## <a name="pydocumentdb-implementation"></a>pyDocumentDB implementering
hello aktuella [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) tooconnect Spark tooAzure Cosmos DB gör enligt följande diagram hello:

![Spark tooAzure Cosmos DB dataflöde via pyDocumentDB DB](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow-of-hello-pydocumentdb-implementation"></a>Dataflöde för hello pyDocumentDB implementering

hello dataflöde är följande:

1. hello Spark huvudnoden ansluter toohello Azure Cosmos DB gateway-noden via pyDocumentDB. En användare anger endast hello Spark och Azure Cosmos DB-anslutningar. Anslutningar toohello respektive huvud- och gateway-noderna är transparent toohello användare.
2. hello gateway-noden gör hello frågan mot Azure Cosmos DB där hello frågan därefter körs mot hello samling partitioner i hello datanoder. hello svar för dessa frågor skickas tillbaka toohello gateway-noden och att resultatet returneras toohello Spark-nod.
3. Efterföljande frågor (till exempel mot ett Spark-DataFrame) skickas toohello Spark arbetarnoder för bearbetning.

Kommunikation mellan Spark och Azure Cosmos DB är begränsad toohello Spark huvudnod och Azure Cosmos DB gateway-noder.  hello frågor går snabbt hello Transportskiktet mellan dessa två noder kan.

### <a name="install-pydocumentdb"></a>Installera pyDocumentDB
Du kan installera pyDocumentDB på Drivrutinsnoden med hjälp av **pip**, till exempel:

```
pip install pyDocumentDB
```


### <a name="connect-spark-tooazure-cosmos-db-via-pydocumentdb"></a>Ansluta Spark tooAzure Cosmos DB via pyDocumentDB
hello enkelhet hello kommunikation transport gör körningen av en fråga från Spark tooAzure Cosmos DB genom att använda relativt enkla pyDocumentDB.

Hej följande fragment visas hur toouse pyDocumentDB i en kontext som Spark.

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

Enligt beskrivningen i hello kodstycke:

* hello Azure Cosmos DB Python SDK (`pyDocumentDB`) innehåller hello alla hello nödvändiga anslutningsparametrar. Till exempel önskade hello platser parametern väljer hello läsa replik och prioritet.
*  Importera hello nödvändiga bibliotek och konfigurera din **masterKey** och **värden** toocreate hello Azure Cosmos DB *klienten* (**pydocumentdb.document_ klienten**).


### <a name="execute-spark-queries-via-pydocumentdb"></a>Köra Spark-frågor via pyDocumentDB
följande exempel används hello Azure DB som Cosmos-instans som skapades i föregående hello-fragment med hjälp av hello hello angetts skrivskyddade nycklar. hello följande kodavsnitt ansluter toohello **airports.codes** samling i hello DoctorWho konto som angavs tidigare och kör en fråga tooextract hello flygplats orter i staten Washington.

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

När hello frågan har utförts **frågan**, hello resultatet är en **query_iterable. QueryIterable** som är konverterade tooa Python-listan. En lista med Python kan vara enkelt konverterade tooa Spark DataFrame med hjälp av hello följande kod:

```
# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(elements)
```

### <a name="why-use-hello-pydocumentdb-tooconnect-spark-tooazure-cosmos-db"></a>Varför använda hello pyDocumentDB tooconnect Spark tooAzure Cosmos DB?
Ansluta Spark tooAzure Cosmos-databas med hjälp av pyDocumentDB är vanligtvis för scenarier där:

* Vill du toouse Python.
* Du returnerar en relativt liten resultatuppsättning från Azure Cosmos DB tooSpark. Observera att hello underliggande dataset i Azure Cosmos-databasen kan vara ganska stor. Du kopplar filter, det vill säga kör predikat filter mot dina Azure Cosmos DB-datakälla.  

## <a name="spark-tooazure-cosmos-db-connector"></a>Spark tooAzure Cosmos-DB-koppling

hello Spark tooAzure Cosmos DB anslutningen använder hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) och flyttar data mellan hello Spark arbetarnoder och Azure Cosmos DB som visas i följande diagram hello:

![Dataflödet i hello Spark tooAzure Cosmos-DB-koppling](./media/spark-connector/spark-connector.png)

hello dataflöde är följande:

1. hello Spark huvudnoden ansluter toohello Azure Cosmos DB gateway-noden tooobtain hello partitionsöversikten. En användare anger endast hello Spark och Azure Cosmos DB-anslutningar. Anslutningar toohello respektive huvud- och gateway-noderna är transparent toohello användare.
2. Den här informationen kan tillbaka toohello Spark-nod.  Du ska nu kunna tooparse hello frågan toodetermine hello partitioner och deras platser i Azure Cosmos-databasen som du behöver tooaccess.
3. Den här informationen är överförda toohello Spark arbetsnoderna.
4. hello Spark arbetarnoder ansluta toohello Azure Cosmos DB partitioner direkt tooextract hello data och returnera hello data toohello Spark partitioner i hello Spark arbetsnoderna.

Kommunikation mellan Spark och Azure Cosmos DB är betydligt snabbare eftersom hello dataförflyttning mellan hello Spark arbetarnoder och hello Azure Cosmos DB datanoder (partitioner).

### <a name="build-hello-spark-tooazure-cosmos-db-connector"></a>Skapa hello Spark tooAzure Cosmos-DB-koppling
Hello connector projekt används för närvarande maven. toobuild hello-anslutningen utan beroenden, som du kan köra:
```
mvn clean package
```
Du kan också hämta hello senaste versionerna av hello JAR från hello *släpper* mapp.

### <a name="include-hello-azure-cosmos-db-spark-jar"></a>Inkludera hello Azure Cosmos DB Spark JAR
Innan du kör någon kod, behöver du tooinclude hello Azure Cosmos DB Spark JAR.  Om du använder hello **spark-shell**, och du kan inkludera hello JAR med hjälp av hello **--burkar** alternativet.  

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3-jar-with-dependencies.jar
```

Om du vill tooexecute hello JAR utan beroenden, Använd hello följande kod:

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3.jar,/$location/azure-documentdb-1.10.0.jar
```

Om du använder en bärbar dator tjänst, till exempel Azure HDInsight Jupyter-anteckningsbok service kan du använda hello **Väck magic** kommandon:

```
%%configure
{ "jars": ["wasb:///example/jars/azure-documentdb-1.10.0.jar","wasb:///example/jars/azure-cosmosdb-spark-0.0.3.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

Hej **burkar** kommando aktiverar du tooinclude hello två JAR: er som krävs för **azure cosmosdb spark** (sig själv och hello Azure DocumentDB Java SDK) och utelämna **scala-återspeglar** så att den inte stör hello Livius anropar (Jupyter-anteckningsbok > Livius > Spark).

### <a name="connect-spark-tooazure-cosmos-db-using-hello-connector"></a>Ansluta Spark tooAzure Cosmos DB med hello connector
Även om hello kommunikation transport är lite mer komplicerad, kör en fråga från Spark tooAzure Cosmos DB med hello-anslutningen är betydligt snabbare.

hello följande kodfragment som visar hur toouse hello kopplingen i en Spark-kontext.

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

Enligt beskrivningen i hello kodstycke:

- **Azure cosmosdb spark** innehåller hello alla hello nödvändiga anslutningsparametrar, bland annat hello önskade platser. Exempelvis kan du välja hello läsa replik och prioritet.
- Bara importera hello nödvändiga bibliotek och konfigurera din masterKey och värden toocreate hello Azure DB som Cosmos-klienten.

### <a name="execute-spark-queries-via-hello-connector"></a>Köra Spark-frågor via hello-koppling

följande exempel använder hello Azure DB som Cosmos-instans som skapades i föregående hello-fragment med hjälp av hello hello angetts skrivskyddade nycklar. hello följande kodavsnitt ansluter toohello DepartureDelays.flights_pcoll samling (i hello DoctorWho kontot som angavs tidigare) och kör en fråga tooextract hello fördröjning flyginformation av flygplan reser från Seattle.

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-hello-spark-tooazure-cosmos-db-connector-implementation"></a>Varför använda hello Spark tooAzure Cosmos DB connector implementering?

Ansluta Spark tooAzure Cosmos DB med hello-anslutningen är vanligtvis för scenarier där:

* Du vill använda toouse Scala och uppdatera den tooinclude en Python-omslutning som anges i [problemet 3: lägga till Python wrapper och exempel](https://github.com/Azure/azure-cosmosdb-spark/issues/3).
* Du har en stor mängd data tootransfer mellan Apache Spark och Azure Cosmos DB.

toogive dig en uppfattning om hello frågan prestandaskillnaden finns hello [frågan Testkörningar wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).

## <a name="distributed-aggregation-example"></a>Distribuerade aggregeringsexempel
Det här avsnittet innehåller några exempel på hur du kan göra distribuerade aggregeringar och analytics med hjälp av Apache Spark och Azure Cosmos DB tillsammans. Azure Cosmos-DB stöder redan aggregeringar, som beskrivs i hello [planeten skala mängder med Azure Cosmos DB blogg](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/). Här är hur du kan dra den toohello nästa nivå med Apache Spark.

Observera att dessa aggregeringar i referens toohello [Spark tooAzure Cosmos DB Connector anteckningsboken](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).

### <a name="connect-tooflights-sample-data"></a>Ansluta tooflights exempeldata
Exemplen aggregering åtkomst till vissa svarta prestandadata som är lagrad i vår **DoctorWho** Azure Cosmos-DB-databas. tooconnect tooit måste tooutilize hello följande kodutdrag:

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

Den här fragment har vi också kommer toorun en grundläggande fråga att överföringar hello filtrerad uppsättning data från Azure Cosmos DB tooSpark där hello senare kan utföra distribuerade mängder. I det här fallet ber vi om flygplan avviker från Seattle (SEA).

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

hello har följande resultat skapats genom att köra frågor hello hello Jupyter-anteckningsbok service.  Observera att alla hello kodavsnitt är generisk och inte är specifika tooany service.

### <a name="running-limit-and-count-queries"></a>Körs GRÄNSEN och antal frågor
Precis som du är används tooin SQL/Spark SQL, låt oss börja med en **GRÄNSEN** fråga:

![Spark GRÄNSEN fråga](./media/spark-connector/spark-sql-query.png)

hello nästa frågan är en enkel och snabb **antal** fråga:

![Spark COUNT-frågan](./media/spark-connector/spark-count-query.png)

### <a name="group-by-query"></a>GROUP BY-fråga
I den här nästa, vi kan köras **GROUP BY** frågor mot vår Azure Cosmos-DB-databas:

```
select destination, sum(delay) as TotalDelays
from originSEA
group by destination
order by sum(delay) desc limit 10
```

![Spark GROUP BY frågan diagram](./media/spark-connector/group-by-query-graph.png)

### <a name="distinct-order-by-query"></a>DISTINKT ORDER BY-fråga
Och här är en **DISTINCT, ORDER BY** fråga:

![Spark GROUP BY frågan diagram](./media/spark-connector/order-by-query.png)

### <a name="continue-hello-flight-data-analysis"></a>Fortsätta hello svarta dataanalys
Du kan använda följande exempel frågor toocontinue dataanalys hello svarta hello:

#### <a name="top-5-delayed-destinations-cities-departing-from-seattle"></a>Vanliga 5 fördröjd mål (orter) reser från Seattle
```
select destination, sum(delay)
from originSEA
where delay < 0
group by destination
order by sum(delay) limit 5
```
![Spark översta fördröjningar diagram](./media/spark-connector/top-delays-graph.png)


#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a>Beräkna median fördröjningar av mål orter reser från Seattle
```
select destination, percentile_approx(delay, 0.5) as median_delay
from originSEA
where delay < 0
group by destination
order by percentile_approx(delay, 0.5)
```

![Spark median fördröjningar diagram](./media/spark-connector/median-delays-graph.png)

## <a name="next-steps"></a>Nästa steg

Om du inte redan gjort hämta hello Spark tooAzure Cosmos DB koppling från hello [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub-lagringsplatsen och utforska hello fler resurser i hello lagringsplatsen:

* [Distribuerade aggregeringar exempel](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Exempel på skript och bärbara datorer](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

Du kan också tooreview hello [datauppsättningar Guide, DataFrames och Apache Spark SQL](http://spark.apache.org/docs/latest/sql-programming-guide.html) och hello [Apache Spark på Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) artikel.
