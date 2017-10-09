---
title: "aaaHigh tillgänglighet med Apache Kafka - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur tooensure hög tillgänglighet med Apache Kafka på Azure HDInsight. Lär dig hur toorebalance partitionera repliker i Kafka, så att de är på olika feldomäner i hello Azure-region som innehåller HDInsight."
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
ms.openlocfilehash: 337468f36b531f83c2999e87907de89cf3d19dd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-of-your-data-with-apache-kafka-preview-on-hdinsight"></a><span data-ttu-id="67b8c-104">Hög tillgänglighet för dina data med Apache Kafka (förhandsversion) på HDInsight</span><span class="sxs-lookup"><span data-stu-id="67b8c-104">High availability of your data with Apache Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="67b8c-105">Lär dig hur tooconfigure partition repliker för Kafka avsnitt tootake nytta av underliggande maskinvara rack konfiguration.</span><span class="sxs-lookup"><span data-stu-id="67b8c-105">Learn how tooconfigure partition replicas for Kafka topics tootake advantage of underlying hardware rack configuration.</span></span> <span data-ttu-id="67b8c-106">Den här konfigurationen garanterar hello tillgängligheten för data som lagras i Apache Kafka på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="67b8c-106">This configuration ensures hello availability of data stored in Apache Kafka on HDInsight.</span></span>

## <a name="fault-and-update-domains-with-kafka"></a><span data-ttu-id="67b8c-107">Fel- och uppdateringsdomäner med Kafka</span><span class="sxs-lookup"><span data-stu-id="67b8c-107">Fault and update domains with Kafka</span></span>

<span data-ttu-id="67b8c-108">En feldomän är en logisk gruppering av underliggande maskinvara i ett Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="67b8c-108">A fault domain is a logical grouping of underlying hardware in an Azure data center.</span></span> <span data-ttu-id="67b8c-109">Varje feldomän delar en gemensam strömkälla och nätverksbrytare.</span><span class="sxs-lookup"><span data-stu-id="67b8c-109">Each fault domain shares a common power source and network switch.</span></span> <span data-ttu-id="67b8c-110">hello virtuella datorer och hanterade diskar som implementerar hello noder i ett HDInsight-kluster är fördelade på dessa feldomäner.</span><span class="sxs-lookup"><span data-stu-id="67b8c-110">hello virtual machines and managed disks that implement hello nodes within an HDInsight cluster are distributed across these fault domains.</span></span> <span data-ttu-id="67b8c-111">Den här arkitekturen begränsar hello potentiella effekten av fysiska maskinvarufel.</span><span class="sxs-lookup"><span data-stu-id="67b8c-111">This architecture limits hello potential impact of physical hardware failures.</span></span>

<span data-ttu-id="67b8c-112">Varje Azure-region har ett visst antal feldomäner.</span><span class="sxs-lookup"><span data-stu-id="67b8c-112">Each Azure region has a specific number of fault domains.</span></span> <span data-ttu-id="67b8c-113">En lista över domäner och hello antalet feldomäner som de innehåller finns hello [tillgänglighetsuppsättningar](../virtual-machines/linux/regions-and-availability.md#availability-sets) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="67b8c-113">For a list of domains and hello number of fault domains they contain, see hello [Availability sets](../virtual-machines/linux/regions-and-availability.md#availability-sets) documentation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="67b8c-114">Kafka har ingen information om feldomäner.</span><span class="sxs-lookup"><span data-stu-id="67b8c-114">Kafka is not aware of fault domains.</span></span> <span data-ttu-id="67b8c-115">När du skapar ett ämne i Kafka, den kan lagra alla partition repliker i hello samma feldomän.</span><span class="sxs-lookup"><span data-stu-id="67b8c-115">When you create a topic in Kafka, it may store all partition replicas in hello same fault domain.</span></span> <span data-ttu-id="67b8c-116">toosolve det här problemet så vi tillhandahåller hello [Kafka partition balansera verktyget](https://github.com/hdinsight/hdinsight-kafka-tools).</span><span class="sxs-lookup"><span data-stu-id="67b8c-116">toosolve this problem, we provide hello [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools).</span></span>

## <a name="when-toorebalance-partition-replicas"></a><span data-ttu-id="67b8c-117">När toorebalance partitions-repliker</span><span class="sxs-lookup"><span data-stu-id="67b8c-117">When toorebalance partition replicas</span></span>

<span data-ttu-id="67b8c-118">tooensure hello högsta tillgänglighet för dina data Kafka, du bör balansera hello partition repliker för ditt avsnitt på hello följande tider:</span><span class="sxs-lookup"><span data-stu-id="67b8c-118">tooensure hello highest availability of your Kafka data, you should rebalance hello partition replicas for your topic at hello following times:</span></span>

* <span data-ttu-id="67b8c-119">När du skapar ett nytt ämne eller en ny partition</span><span class="sxs-lookup"><span data-stu-id="67b8c-119">When a new topic or partition is created</span></span>

* <span data-ttu-id="67b8c-120">När du skalar upp ett kluster</span><span class="sxs-lookup"><span data-stu-id="67b8c-120">When you scale up a cluster</span></span>

## <a name="replication-factor"></a><span data-ttu-id="67b8c-121">Replikeringsfaktor</span><span class="sxs-lookup"><span data-stu-id="67b8c-121">Replication factor</span></span>

> [!IMPORTANT]
> <span data-ttu-id="67b8c-122">Vi rekommenderar att du använder en Azure-region som innehåller tre feldomäner, och använder replikeringsfaktorn 3.</span><span class="sxs-lookup"><span data-stu-id="67b8c-122">We recommend using an Azure region that contains three fault domains, and using a replication factor of 3.</span></span>

<span data-ttu-id="67b8c-123">Om du måste använda en region med bara två feldomäner, kan du använda replikering upp till 4 toospread hello repliker jämnt över hello två feldomäner.</span><span class="sxs-lookup"><span data-stu-id="67b8c-123">If you must use a region that contains only two fault domains, use a replication factor of 4 toospread hello replicas evenly across hello two fault domains.</span></span>

<span data-ttu-id="67b8c-124">Ett exempel för att skapa ämnen och inställningen hello replikering faktor finns hello [börjar med Kafka på HDInsight](hdinsight-apache-kafka-get-started.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="67b8c-124">For an example of creating topics and setting hello replication factor, see hello [Start with Kafka on HDInsight](hdinsight-apache-kafka-get-started.md) document.</span></span>

## <a name="how-toorebalance-partition-replicas"></a><span data-ttu-id="67b8c-125">Hur toorebalance partitions-repliker</span><span class="sxs-lookup"><span data-stu-id="67b8c-125">How toorebalance partition replicas</span></span>

<span data-ttu-id="67b8c-126">Använd hello [Kafka partition balansera verktyget](https://github.com/hdinsight/hdinsight-kafka-tools) toorebalance markerat hjälpavsnitt.</span><span class="sxs-lookup"><span data-stu-id="67b8c-126">Use hello [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools) toorebalance selected topics.</span></span> <span data-ttu-id="67b8c-127">Det här verktyget måste vara körde från en SSH-session toohello huvudnod Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="67b8c-127">This tool must be ran from an SSH session toohello head node of your Kafka cluster.</span></span>

<span data-ttu-id="67b8c-128">Mer information om hur du ansluter med hjälp av SSH tooHDInsight finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="67b8c-128">For more information on connecting tooHDInsight using SSH, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="67b8c-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="67b8c-129">Next steps</span></span>

* [<span data-ttu-id="67b8c-130">Skalbarhet för Kafka i HDInsight</span><span class="sxs-lookup"><span data-stu-id="67b8c-130">Scalability of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-scalability.md)
* [<span data-ttu-id="67b8c-131">Spegling med Kafka i HDInsight</span><span class="sxs-lookup"><span data-stu-id="67b8c-131">Mirroring with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)