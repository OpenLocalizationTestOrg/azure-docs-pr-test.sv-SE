---
title: "aaaAn introduktion tooApache Kafka på HDInsight - Azure | Microsoft Docs"
description: "Lär dig mer om Apache Kafka på HDInsight: vad det är syfte och där toofind exempel och komma igång information."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: f284b6e3-5f3b-4a50-b455-917e77588069
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/15/2017
ms.author: larryfr
ms.openlocfilehash: 1bc198d4cf93a4682030d4fa5f71030f49ad64be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a><span data-ttu-id="fa923-103">Introduktion till Apache Kafka på HDInsight (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="fa923-103">Introducing Apache Kafka on HDInsight (preview)</span></span>

<span data-ttu-id="fa923-104">[Apache Kafka](https://kafka.apache.org) är en öppen källkod distribuerade strömmande plattform som kan använda toobuild realtid strömmande data pipelines och program.</span><span class="sxs-lookup"><span data-stu-id="fa923-104">[Apache Kafka](https://kafka.apache.org) is an open-source distributed streaming platform that can be used toobuild real-time streaming data pipelines and applications.</span></span> <span data-ttu-id="fa923-105">Kafka ger också meddelandet broker funktioner liknande tooa meddelandekö, där du kan publicera och prenumerera på toonamed dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="fa923-105">Kafka also provides message broker functionality similar tooa message queue, where you can publish and subscribe toonamed data streams.</span></span> <span data-ttu-id="fa923-106">Kafka på HDInsight ger dig en hanterad, skalbara och hög tillgänglighet tjänst i hello Microsoft Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="fa923-106">Kafka on HDInsight provides you with a managed, highly scalable, and highly available service in hello Microsoft Azure cloud.</span></span>

## <a name="why-use-kafka-on-hdinsight"></a><span data-ttu-id="fa923-107">Varför använda Kafka på HDInsight?</span><span class="sxs-lookup"><span data-stu-id="fa923-107">Why use Kafka on HDInsight?</span></span>

<span data-ttu-id="fa923-108">Kafka innehåller hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="fa923-108">Kafka provides hello following features:</span></span>

* <span data-ttu-id="fa923-109">Publicera och prenumerera meddelanden mönster: Kafka innehåller ett producenten API för att publicera poster tooa Kafka avsnittet.</span><span class="sxs-lookup"><span data-stu-id="fa923-109">Publish-subscribe messaging pattern: Kafka provides a Producer API for publishing records tooa Kafka topic.</span></span> <span data-ttu-id="fa923-110">hello konsumenten API används när prenumerera tooa avsnittet.</span><span class="sxs-lookup"><span data-stu-id="fa923-110">hello Consumer API is used when subscribing tooa topic.</span></span>

* <span data-ttu-id="fa923-111">Dataströmsbearbetning: Kafka används ofta med Apache Storm eller Spark för bearbetning av dataströmmen i realtid.</span><span class="sxs-lookup"><span data-stu-id="fa923-111">Stream processing: Kafka is often used with Apache Storm or Spark for real-time stream processing.</span></span> <span data-ttu-id="fa923-112">Kafka 0.10.0.0 (HDInsight version 3.5) introducerades en strömmande API som gör att du toobuild strömning lösningar utan Storm eller Spark.</span><span class="sxs-lookup"><span data-stu-id="fa923-112">Kafka 0.10.0.0 (HDInsight version 3.5) introduced a streaming API that allows you toobuild streaming solutions without requiring Storm or Spark.</span></span>

* <span data-ttu-id="fa923-113">Skala horisontellt: Kafka partitionerar dataströmmar över hello noderna i hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="fa923-113">Horizontal scale: Kafka partitions streams across hello nodes in hello HDInsight cluster.</span></span> <span data-ttu-id="fa923-114">Konsumenten processer kan associeras med enskilda partitioner tooprovide nätverksbelastning när förbrukar poster.</span><span class="sxs-lookup"><span data-stu-id="fa923-114">Consumer processes can be associated with individual partitions tooprovide load balancing when consuming records.</span></span>

* <span data-ttu-id="fa923-115">I ordning: inom varje partition poster som lagras i hello dataströmmen i hello ordning som de har tagits emot.</span><span class="sxs-lookup"><span data-stu-id="fa923-115">In-order delivery: Within each partition, records are stored in hello stream in hello order that they were received.</span></span> <span data-ttu-id="fa923-116">Genom att associera en konsumentprocess per partition bearbetas posterna garanterat i ordning.</span><span class="sxs-lookup"><span data-stu-id="fa923-116">By associating one consumer process per partition, you can guarantee that records are processed in-order.</span></span>

* <span data-ttu-id="fa923-117">Feltoleranta:, Partitioner kan replikeras mellan noder tooprovide feltolerans.</span><span class="sxs-lookup"><span data-stu-id="fa923-117">Fault-tolerant: Partitions can be replicated between nodes tooprovide fault tolerance.</span></span>

* <span data-ttu-id="fa923-118">Integrering med Azure hanterade diskar: hanterade diskar ger högre skala och genomströmning för hello-diskar som används av hello virtuella datorer i hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="fa923-118">Integration with Azure Managed Disks: Managed disks provides higher scale and throughput for hello disks used by hello virtual machines in hello HDInsight cluster.</span></span>

    <span data-ttu-id="fa923-119">Hanterade diskar är aktiverat som standard för Kafka i HDInsight och hello antalet diskar som används per nod kan konfigureras när HDInsight skapas.</span><span class="sxs-lookup"><span data-stu-id="fa923-119">Managed disks are enabled by default for Kafka on HDInsight, and hello number of disks used per node can be configured during HDInsight creation.</span></span> <span data-ttu-id="fa923-120">För mer information om hanterade diskar, se [Hanterade Azure-diskar](../virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fa923-120">For more information on managed disks, see [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md).</span></span>

    <span data-ttu-id="fa923-121">Se [Ökad skalbalhet med Kafta på HDInsight](hdinsight-apache-kafka-scalability.md) för mer information om att konfigurera hanterade diskar med Kafka på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="fa923-121">For information on configuring managed disks with Kafka on HDInsight, see [Increase scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md).</span></span>

## <a name="use-cases"></a><span data-ttu-id="fa923-122">Användningsfall</span><span class="sxs-lookup"><span data-stu-id="fa923-122">Use cases</span></span>

* <span data-ttu-id="fa923-123">**Messaging**: eftersom den stöder hello Publicera prenumerera-mönster för meddelandet, Kafka används ofta som en koordinator för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="fa923-123">**Messaging**: Since it supports hello publish-subscribe message pattern, Kafka is often used as a message broker.</span></span>

* <span data-ttu-id="fa923-124">**Aktivitetsspårning**: eftersom Kafka tillhandahåller i ordning loggning med poster, den kan använda tootrack och skapa aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="fa923-124">**Activity tracking**: Since Kafka provides in-order logging of records, it can be used tootrack and re-create activities.</span></span> <span data-ttu-id="fa923-125">Till exempel användaråtgärder på en webbplats eller i ett program.</span><span class="sxs-lookup"><span data-stu-id="fa923-125">For example, user actions on a web site or within an application.</span></span>

* <span data-ttu-id="fa923-126">**Aggregeringen**: med bearbetning av dataströmmen kan du samla information från olika dataströmmar toocombine och centralisera hello information till användningsdata.</span><span class="sxs-lookup"><span data-stu-id="fa923-126">**Aggregation**: Using stream processing, you can aggregate information from different streams toocombine and centralize hello information into operational data.</span></span>

* <span data-ttu-id="fa923-127">**Omvandling**: Med dataströmsbearbetning kan du kombinera och utöka data från flera inkommande avsnitt i ett eller flera utdataämnen.</span><span class="sxs-lookup"><span data-stu-id="fa923-127">**Transformation**: Using stream processing, you can combine and enrich data from multiple input topics into one or more output topics.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa923-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fa923-128">Next steps</span></span>

<span data-ttu-id="fa923-129">Använd hello följande länkar toolearn hur toouse Apache Kafka på HDInsight:</span><span class="sxs-lookup"><span data-stu-id="fa923-129">Use hello following links toolearn how toouse Apache Kafka on HDInsight:</span></span>

* [<span data-ttu-id="fa923-130">Kom igång med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="fa923-130">Get started with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)

* [<span data-ttu-id="fa923-131">Använda MirrorMaker toocreate en replik av Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="fa923-131">Use MirrorMaker toocreate a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)

* [<span data-ttu-id="fa923-132">Använda Apache Storm med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="fa923-132">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)

* [<span data-ttu-id="fa923-133">Använda Apache Spark med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="fa923-133">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)

* [<span data-ttu-id="fa923-134">Ansluta tooKafka via ett virtuellt Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="fa923-134">Connect tooKafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)
