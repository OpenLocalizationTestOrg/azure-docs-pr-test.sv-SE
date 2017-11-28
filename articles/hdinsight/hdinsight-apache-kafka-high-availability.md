---
title: "Hög tillgänglighet med Apache Kafka – Azure HDInsight | Microsoft Docs"
description: "Lär dig mer om att säkerställa hög tillgänglighet med Apache Kafka på Azure HDInsight. Lär dig mer om att balansera om partitionsrepliker i Kafka så att de är inom olika feldomäner i Azure-regionen som innehåller HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: f8164d1c3483b28e5f2abcc8035da78880daec1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="high-availability-of-your-data-with-apache-kafka-preview-on-hdinsight"></a><span data-ttu-id="6c586-104">Hög tillgänglighet för dina data med Apache Kafka (förhandsversion) på HDInsight</span><span class="sxs-lookup"><span data-stu-id="6c586-104">High availability of your data with Apache Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="6c586-105">Lär dig hur du konfigurerar partitionsrepliker för Kafka-ämnen med hänsyn till underliggande rackkonfigurationer för maskinvara.</span><span class="sxs-lookup"><span data-stu-id="6c586-105">Learn how to configure partition replicas for Kafka topics to take advantage of underlying hardware rack configuration.</span></span> <span data-ttu-id="6c586-106">Den här konfigurationen garanterar tillgängligheten för data som lagras i Apache Kafka på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6c586-106">This configuration ensures the availability of data stored in Apache Kafka on HDInsight.</span></span>

## <a name="fault-and-update-domains-with-kafka"></a><span data-ttu-id="6c586-107">Fel- och uppdateringsdomäner med Kafka</span><span class="sxs-lookup"><span data-stu-id="6c586-107">Fault and update domains with Kafka</span></span>

<span data-ttu-id="6c586-108">En feldomän är en logisk gruppering av underliggande maskinvara i ett Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="6c586-108">A fault domain is a logical grouping of underlying hardware in an Azure data center.</span></span> <span data-ttu-id="6c586-109">Varje feldomän delar en gemensam strömkälla och nätverksbrytare.</span><span class="sxs-lookup"><span data-stu-id="6c586-109">Each fault domain shares a common power source and network switch.</span></span> <span data-ttu-id="6c586-110">De virtuella datorer och hanterade diskar som implementerar noderna i ett HDInsight-kluster är fördelade mellan dessa feldomäner.</span><span class="sxs-lookup"><span data-stu-id="6c586-110">The virtual machines and managed disks that implement the nodes within an HDInsight cluster are distributed across these fault domains.</span></span> <span data-ttu-id="6c586-111">Den här arkitekturen begränsar de potentiella problemen vid fysiska maskinvarufel.</span><span class="sxs-lookup"><span data-stu-id="6c586-111">This architecture limits the potential impact of physical hardware failures.</span></span>

<span data-ttu-id="6c586-112">Varje Azure-region har ett visst antal feldomäner.</span><span class="sxs-lookup"><span data-stu-id="6c586-112">Each Azure region has a specific number of fault domains.</span></span> <span data-ttu-id="6c586-113">En lista med domäner och antalet feldomäner de innehåller finns i dokumentationen av [tillgänglighetsuppsättningar](../virtual-machines/linux/regions-and-availability.md#availability-sets).</span><span class="sxs-lookup"><span data-stu-id="6c586-113">For a list of domains and the number of fault domains they contain, see the [Availability sets](../virtual-machines/linux/regions-and-availability.md#availability-sets) documentation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6c586-114">Kafka har ingen information om feldomäner.</span><span class="sxs-lookup"><span data-stu-id="6c586-114">Kafka is not aware of fault domains.</span></span> <span data-ttu-id="6c586-115">När du skapar ett ämne i Kafka kan det lagra alla partitionsrepliker i samma feldomän.</span><span class="sxs-lookup"><span data-stu-id="6c586-115">When you create a topic in Kafka, it may store all partition replicas in the same fault domain.</span></span> <span data-ttu-id="6c586-116">Vi tillhandahåller [verktyget för ombalansering av Kafka-partitioner](https://github.com/hdinsight/hdinsight-kafka-tools) som lösning på det här problemet.</span><span class="sxs-lookup"><span data-stu-id="6c586-116">To solve this problem, we provide the [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools).</span></span>

## <a name="when-to-rebalance-partition-replicas"></a><span data-ttu-id="6c586-117">När ska du balansera om partitionsrepliker</span><span class="sxs-lookup"><span data-stu-id="6c586-117">When to rebalance partition replicas</span></span>

<span data-ttu-id="6c586-118">Du får bästa möjliga tillgänglighet för dina Kafka-data om du balanserar om partitionsreplikerna för ditt ämne vid följande tidpunkter:</span><span class="sxs-lookup"><span data-stu-id="6c586-118">To ensure the highest availability of your Kafka data, you should rebalance the partition replicas for your topic at the following times:</span></span>

* <span data-ttu-id="6c586-119">När du skapar ett nytt ämne eller en ny partition</span><span class="sxs-lookup"><span data-stu-id="6c586-119">When a new topic or partition is created</span></span>

* <span data-ttu-id="6c586-120">När du skalar upp ett kluster</span><span class="sxs-lookup"><span data-stu-id="6c586-120">When you scale up a cluster</span></span>

## <a name="replication-factor"></a><span data-ttu-id="6c586-121">Replikeringsfaktor</span><span class="sxs-lookup"><span data-stu-id="6c586-121">Replication factor</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6c586-122">Vi rekommenderar att du använder en Azure-region som innehåller tre feldomäner, och använder replikeringsfaktorn 3.</span><span class="sxs-lookup"><span data-stu-id="6c586-122">We recommend using an Azure region that contains three fault domains, and using a replication factor of 3.</span></span>

<span data-ttu-id="6c586-123">Om du måste använda en region som bara har två feldomäner ska du använda replikeringsfaktorn 4, så att replikerna fördelas jämnt mellan de två feldomänerna.</span><span class="sxs-lookup"><span data-stu-id="6c586-123">If you must use a region that contains only two fault domains, use a replication factor of 4 to spread the replicas evenly across the two fault domains.</span></span>

<span data-ttu-id="6c586-124">I dokumentet [Start with Kafka on HDInsight](hdinsight-apache-kafka-get-started.md) (Kom igång med Kafka i HDInsight) ges ett exempel på hur du skapar ämnen och ställer in replikeringsfaktorn.</span><span class="sxs-lookup"><span data-stu-id="6c586-124">For an example of creating topics and setting the replication factor, see the [Start with Kafka on HDInsight](hdinsight-apache-kafka-get-started.md) document.</span></span>

## <a name="how-to-rebalance-partition-replicas"></a><span data-ttu-id="6c586-125">Så balanserar du om partitionsrepliker</span><span class="sxs-lookup"><span data-stu-id="6c586-125">How to rebalance partition replicas</span></span>

<span data-ttu-id="6c586-126">Använd [verktyget för ombalansering av Kafka-partitioner](https://github.com/hdinsight/hdinsight-kafka-tools) till att balansera om valda ämnen.</span><span class="sxs-lookup"><span data-stu-id="6c586-126">Use the [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools) to rebalance selected topics.</span></span> <span data-ttu-id="6c586-127">Du måste köra det här verktyget från en SSH-session till huvudnoden för ditt Kafka-kluster.</span><span class="sxs-lookup"><span data-stu-id="6c586-127">This tool must be ran from an SSH session to the head node of your Kafka cluster.</span></span>

<span data-ttu-id="6c586-128">Mer information om hur du ansluter till HDInsight via SSH finns i dokumentet [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="6c586-128">For more information on connecting to HDInsight using SSH, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c586-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c586-129">Next steps</span></span>

* [<span data-ttu-id="6c586-130">Skalbarhet för Kafka i HDInsight</span><span class="sxs-lookup"><span data-stu-id="6c586-130">Scalability of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-scalability.md)
* [<span data-ttu-id="6c586-131">Spegling med Kafka i HDInsight</span><span class="sxs-lookup"><span data-stu-id="6c586-131">Mirroring with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)