---
title: "Vad är HBase i Azure HDInsight? | Microsoft Docs"
description: "En introduktion till Apache HBase i HDInsight en NoSQL-databas som bygger på Hadoop. Läs mer om användningsfall och jämför HBase med andra Hadoop-kluster."
keywords: bigtable,nosql,what is hbase,apache hbase,hbase,habase overview,
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: d2a76d53-133a-4849-a30c-88d9c794391c
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 6823633bfdb07ce649083804ba211709519cb6da
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a><span data-ttu-id="dd35a-106">Vad är HBase i HDInsight: En NoSQL-databas som tillhandahåller BigTable-liknande kapacitet för Hadoop</span><span class="sxs-lookup"><span data-stu-id="dd35a-106">What is HBase in HDInsight: A NoSQL database that provides BigTable-like capabilities for Hadoop</span></span>
<span data-ttu-id="dd35a-107">Apache HBase är en NoSQL-databas med öppen källkod som har skapats på Hadoop och modellerats efter Google BigTable.</span><span class="sxs-lookup"><span data-stu-id="dd35a-107">Apache HBase is an open-source, NoSQL database that is built on Hadoop and modeled after Google BigTable.</span></span> <span data-ttu-id="dd35a-108">HBase ger direktåtkomst och stark konsekvens för stora mängder ostrukturerade och halvstrukturerade data i en schemalös databas sorterad per kolumnfamiljer.</span><span class="sxs-lookup"><span data-stu-id="dd35a-108">HBase provides random access and strong consistency for large amounts of unstructured and semistructured data in a schemaless database organized by column families.</span></span>

<span data-ttu-id="dd35a-109">Data lagras i tabellens rader och data i raderna grupperas per kolumnfamilj.</span><span class="sxs-lookup"><span data-stu-id="dd35a-109">Data is stored in the rows of a table, and data within a row is grouped by column family.</span></span> <span data-ttu-id="dd35a-110">HBase är en schemalös databas i den mening att varken kolumner eller den typ av data som lagras i dem måste definieras innan du använder dem.</span><span class="sxs-lookup"><span data-stu-id="dd35a-110">HBase is a schemaless database in the sense that neither the columns nor the type of data stored in them need to be defined before using them.</span></span> <span data-ttu-id="dd35a-111">Den öppna källkoden skalas linjärt för att hantera petabyte med data på tusentals noder.</span><span class="sxs-lookup"><span data-stu-id="dd35a-111">The open-source code scales linearly to handle petabytes of data on thousands of nodes.</span></span> <span data-ttu-id="dd35a-112">Den kan utgå ifrån dataredundans, batchbearbetning och andra funktioner som tillhandahålls av distribuerade program i Hadoop-miljön.</span><span class="sxs-lookup"><span data-stu-id="dd35a-112">It can rely on data redundancy, batch processing, and other features that are provided by distributed applications in the Hadoop ecosystem.</span></span>

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a><span data-ttu-id="dd35a-113">Hur är HBase implementerat i Azure HDInsight?</span><span class="sxs-lookup"><span data-stu-id="dd35a-113">How is HBase implemented in Azure HDInsight?</span></span>
<span data-ttu-id="dd35a-114">HDInsight HBase erbjuds som ett hanterat kluster som är integrerat i Azure-miljön.</span><span class="sxs-lookup"><span data-stu-id="dd35a-114">HDInsight HBase is offered as a managed cluster that is integrated into the Azure environment.</span></span> <span data-ttu-id="dd35a-115">Klustren har konfigurerats för att lagra data direkt i [Azure Storage](./hdinsight-hadoop-use-blob-storage.md) eller [Azure Data Lake Store](./hdinsight-hadoop-use-data-lake-store.md). Det ger mindre fördröjning och större flexibilitet när det gäller alternativ för prestanda och kostnader.</span><span class="sxs-lookup"><span data-stu-id="dd35a-115">The clusters are configured to store data directly in [Azure Storage](./hdinsight-hadoop-use-blob-storage.md) or [Azure Data Lake Store](./hdinsight-hadoop-use-data-lake-store.md), which provides low latency and increased elasticity in performance and cost choices.</span></span> <span data-ttu-id="dd35a-116">Det gör att kunderna kan bygga interaktiva webbplatser som fungerar med stora datauppsättningar och skapa tjänster som lagrar sensor- och telemetridata från miljontals slutpunkter och analysera dessa data med Hadoop-jobb.</span><span class="sxs-lookup"><span data-stu-id="dd35a-116">This enables customers to build interactive websites that work with large datasets, to build services that store sensor and telemetry data from millions of end points, and to analyze this data with Hadoop jobs.</span></span> <span data-ttu-id="dd35a-117">HBase och Hadoop är bra startpunkter för stordataprojekt i Azure som gör att realtidsprogram kan arbeta med stora datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="dd35a-117">HBase and Hadoop are good starting points for big data project in Azure; in particular, they can enable real-time applications to work with large datasets.</span></span>

<span data-ttu-id="dd35a-118">HDInsight-implementeringen utnyttjar HBase skalbara arkitektur för att tillhandahålla automatisk delning av tabeller, stor konsekvens för läsning och skrivning och automatisk redundans.</span><span class="sxs-lookup"><span data-stu-id="dd35a-118">The HDInsight implementation leverages the scale-out architecture of HBase to provide automatic sharding of tables, strong consistency for reads and writes, and automatic failover.</span></span> <span data-ttu-id="dd35a-119">Prestanda utökas av cachelagring i minnet för läsning och snabb strömning för skrivning.</span><span class="sxs-lookup"><span data-stu-id="dd35a-119">Performance is enhanced by in-memory caching for reads and high-throughput streaming for writes.</span></span> <span data-ttu-id="dd35a-120">HBase-kluster kan skapas i virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="dd35a-120">HBase cluster can be created inside virtual network.</span></span> <span data-ttu-id="dd35a-121">Mer information finns i [Create HDInsight clusters on Azure Virtual Network][hbase-provision-vnet] (Skapa HDInsight-kluster i Azure Virtual Network).</span><span class="sxs-lookup"><span data-stu-id="dd35a-121">For details, see  [Create HDInsight clusters on Azure Virtual Network][hbase-provision-vnet].</span></span>

## <a name="how-is-data-managed-in-hdinsight-hbase"></a><span data-ttu-id="dd35a-122">Hur hanteras data i HDInsight HBase?</span><span class="sxs-lookup"><span data-stu-id="dd35a-122">How is data managed in HDInsight HBase?</span></span>
<span data-ttu-id="dd35a-123">Data kan hanteras i HBase med hjälp av kommandona `create`, `get`, `put` och `scan` i HBase-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="dd35a-123">Data can be managed in HBase by using the `create`, `get`, `put`, and `scan` commands from the HBase shell.</span></span> <span data-ttu-id="dd35a-124">Data skrivs till databasen med hjälp av `put` och läses med hjälp av `get`.</span><span class="sxs-lookup"><span data-stu-id="dd35a-124">Data is written to the database by using `put` and read by using `get`.</span></span> <span data-ttu-id="dd35a-125">Kommandot `scan` används till att hämta data från flera rader i en tabell.</span><span class="sxs-lookup"><span data-stu-id="dd35a-125">The `scan` command is used to obtain data from multiple rows in a table.</span></span> <span data-ttu-id="dd35a-126">Data kan också hanteras med HBase C#-API. Det ger ett klientbibliotek utöver HBase REST-API.</span><span class="sxs-lookup"><span data-stu-id="dd35a-126">Data can also be managed using the HBase C# API, which provides a client library on top of the HBase REST API.</span></span> <span data-ttu-id="dd35a-127">En HBase-databas kan också efterfrågas med hjälp av Hive.</span><span class="sxs-lookup"><span data-stu-id="dd35a-127">An HBase database can also be queried by using Hive.</span></span> <span data-ttu-id="dd35a-128">En introduktion till dessa programmeringsmodeller finns i [Komma igång med Hadoop i HDInsight HBase][hbase-get-started].</span><span class="sxs-lookup"><span data-stu-id="dd35a-128">For an introduction to these programming models, see [Get started using HBase with Hadoop in HDInsight][hbase-get-started].</span></span> <span data-ttu-id="dd35a-129">Det finns också coprocessorer för databehandling i de noder som är värdar för databasen.</span><span class="sxs-lookup"><span data-stu-id="dd35a-129">Co-processors are also available, which allow data processing in the nodes that host the database.</span></span>

> [!NOTE]
> <span data-ttu-id="dd35a-130">Thrift stöds inte av HBase i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dd35a-130">Thrift is not supported by HBase in HDInsight.</span></span>
>

## <a name="scenarios-use-cases-for-hbase"></a><span data-ttu-id="dd35a-131">Scenarier: Användningsfall för HBase</span><span class="sxs-lookup"><span data-stu-id="dd35a-131">Scenarios: Use cases for HBase</span></span>
<span data-ttu-id="dd35a-132">BigTable (och HBase via tillägg) skapades framförallt för webbsökning.</span><span class="sxs-lookup"><span data-stu-id="dd35a-132">The canonical use case for which BigTable (and by extension, HBase) was created was web search.</span></span> <span data-ttu-id="dd35a-133">Sökmotorer skapar index som mappar termer till de webbsidor som innehåller dem.</span><span class="sxs-lookup"><span data-stu-id="dd35a-133">Search engines build indexes that map terms to the web pages that contain them.</span></span> <span data-ttu-id="dd35a-134">Men HBase passar också för många andra användningsområden och du hittar många av dem i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="dd35a-134">But there are many other use cases that HBase is suitable for—several of which are itemized in this section.</span></span>

* <span data-ttu-id="dd35a-135">Nyckelvärdeslagring</span><span class="sxs-lookup"><span data-stu-id="dd35a-135">Key-value store</span></span>
  
    <span data-ttu-id="dd35a-136">HBase kan användas som ett nyckelvärdeslager och passar för hantering av meddelandesystem.</span><span class="sxs-lookup"><span data-stu-id="dd35a-136">HBase can be used as a key-value store, and it is suitable for managing message systems.</span></span> <span data-ttu-id="dd35a-137">HBase är perfekt för lagring och hantering av internetkommunikation och används till exempel av Facebook för meddelandesystemen.</span><span class="sxs-lookup"><span data-stu-id="dd35a-137">Facebook uses HBase for their messaging system, and it is ideal for storing and managing Internet communications.</span></span> <span data-ttu-id="dd35a-138">WebTable använder HBase till att söka efter och hantera tabeller som extraheras från webbsidor.</span><span class="sxs-lookup"><span data-stu-id="dd35a-138">WebTable uses HBase to search for and manage tables that are extracted from webpages.</span></span>
* <span data-ttu-id="dd35a-139">Sensordata</span><span class="sxs-lookup"><span data-stu-id="dd35a-139">Sensor data</span></span>
  
    <span data-ttu-id="dd35a-140">HBase är användbart för insamling av data som samlas in inkrementellt från olika källor.</span><span class="sxs-lookup"><span data-stu-id="dd35a-140">HBase is useful for capturing data that is collected incrementally from various sources.</span></span> <span data-ttu-id="dd35a-141">Det omfattar sociala analyser, tidsserier, hålla interaktiva instrumentpaneler uppdaterade med trender och räknare och hantera system för granskningsloggar.</span><span class="sxs-lookup"><span data-stu-id="dd35a-141">This includes social analytics, time series, keeping interactive dashboards up-to-date with trends and counters, and managing audit log systems.</span></span> <span data-ttu-id="dd35a-142">Några exempel är Bloomberg trader terminal och Open Time Series Database (OpenTSDB), som lagrar och ger åtkomst till mätvärden som samlats in om hälsostatus för serversystem.</span><span class="sxs-lookup"><span data-stu-id="dd35a-142">Examples include Bloomberg trader terminal and the Open Time Series Database (OpenTSDB), which stores and provides access to metrics collected about the health of server systems.</span></span>
* <span data-ttu-id="dd35a-143">Realtidsfråga</span><span class="sxs-lookup"><span data-stu-id="dd35a-143">Real-time query</span></span>
  
    <span data-ttu-id="dd35a-144">[Phoenix](http://phoenix.apache.org/) är en SQL-frågemotor för Apache HBase.</span><span class="sxs-lookup"><span data-stu-id="dd35a-144">[Phoenix](http://phoenix.apache.org/) is a SQL query engine for Apache HBase.</span></span> <span data-ttu-id="dd35a-145">Den används som en JDBC-drivrutin, och gör att du kan ställa frågor och hantera HBase-tabeller med SQL.</span><span class="sxs-lookup"><span data-stu-id="dd35a-145">It is accessed as a JDBC driver, and it enables querying and managing HBase tables by using SQL.</span></span>
* <span data-ttu-id="dd35a-146">HBase som en plattform</span><span class="sxs-lookup"><span data-stu-id="dd35a-146">HBase as a platform</span></span>
  
    <span data-ttu-id="dd35a-147">Program kan köras ovanpå HBase genom att använda det som ett datalager.</span><span class="sxs-lookup"><span data-stu-id="dd35a-147">Applications can run on top of HBase by using it as a datastore.</span></span> <span data-ttu-id="dd35a-148">Några exempel är Phoenix, OpenTSDB, Kiji och Titan.</span><span class="sxs-lookup"><span data-stu-id="dd35a-148">Examples include Phoenix, OpenTSDB, Kiji, and Titan.</span></span> <span data-ttu-id="dd35a-149">Program kan också integreras med HBase.</span><span class="sxs-lookup"><span data-stu-id="dd35a-149">Applications can also integrate with HBase.</span></span> <span data-ttu-id="dd35a-150">Exemplen innefattar Hive, Pig, Solr, Storm, Flume, Impala, Spark, Ganglia och Drill.</span><span class="sxs-lookup"><span data-stu-id="dd35a-150">Examples include Hive, Pig, Solr, Storm, Flume, Impala, Spark, Ganglia, and Drill.</span></span>

## <span data-ttu-id="dd35a-151"><a name="next-steps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd35a-151"><a name="next-steps"></a>Next steps</span></span>
* <span data-ttu-id="dd35a-152">[Komma igång med HBase med Hadoop i HDInsight][hbase-get-started]</span><span class="sxs-lookup"><span data-stu-id="dd35a-152">[Get started using HBase with Hadoop in HDInsight][hbase-get-started]</span></span>
* <span data-ttu-id="dd35a-153">[Create HDInsight clusters on Azure Virtual Network][hbase-provision-vnet] (Skapa HDInsight-kluster i Azure Virtual Network)</span><span class="sxs-lookup"><span data-stu-id="dd35a-153">[Create HDInsight clusters on Azure Virtual Network][hbase-provision-vnet]</span></span>
* [<span data-ttu-id="dd35a-154">Konfigurera HBase-replikering i HDInsight</span><span class="sxs-lookup"><span data-stu-id="dd35a-154">Configure HBase replication in HDInsight</span></span>](hdinsight-hbase-replication.md)
* <span data-ttu-id="dd35a-155">[Analyze Twitter sentiment with HBase in HDInsight][hbase-twitter-sentiment] (Analysera Twitter-sentiment med HBase i HDInsight)</span><span class="sxs-lookup"><span data-stu-id="dd35a-155">[Analyze Twitter sentiment with HBase in HDInsight][hbase-twitter-sentiment]</span></span>
* <span data-ttu-id="dd35a-156">[Use Maven to build Java applications that use HBase with HDInsight (Hadoop)][hbase-build-java-maven] (Använda Maven för att skapa Java-program som använder HBase med HDInsight (Hadoop))</span><span class="sxs-lookup"><span data-stu-id="dd35a-156">[Use Maven to build Java applications that use HBase with HDInsight (Hadoop)][hbase-build-java-maven]</span></span>

## <span data-ttu-id="dd35a-157"><a name="see-also"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="dd35a-157"><a name="see-also"></a>See also</span></span>
* [<span data-ttu-id="dd35a-158">Apache HBase</span><span class="sxs-lookup"><span data-stu-id="dd35a-158">Apache HBase</span></span>](https://hbase.apache.org/)
* [<span data-ttu-id="dd35a-159">Bigtable: Ett distribuerat Storage-system för strukturerade data</span><span class="sxs-lookup"><span data-stu-id="dd35a-159">Bigtable: A Distributed Storage System for Structured Data</span></span>](http://research.google.com/archive/bigtable.html)

[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
