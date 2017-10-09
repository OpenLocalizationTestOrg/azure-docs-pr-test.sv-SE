---
title: "aaaApache Kafka öka skala - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur tooconfigure hanterade diskar för Apache Kafka kluster på Azure HDInsight tooincrease skalbarhet."
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
ms.date: 06/14/2017
ms.author: larryfr
ms.openlocfilehash: b51114b33359f2c385f057a7a7a5b134cad27e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a><span data-ttu-id="bf65b-103">Konfigurera lagring och skalbarhet för Apache Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="bf65b-103">Configure storage and scalability for Apache Kafka on HDInsight</span></span>

<span data-ttu-id="bf65b-104">Lär dig hur tooconfigure hello antalet hanterade diskar som används av Apache Kafka på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bf65b-104">Learn how tooconfigure hello number of managed disks used by Apache Kafka on HDInsight.</span></span>

<span data-ttu-id="bf65b-105">Kafka på HDInsight använder hello lokal disk hello virtuella datorer i hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bf65b-105">Kafka on HDInsight uses hello local disk of hello virtual machines in hello HDInsight cluster.</span></span> <span data-ttu-id="bf65b-106">Eftersom Kafka är mycket i/o-stor, [Azure hanterade diskar](../virtual-machines/windows/managed-disks-overview.md) är används tooprovide högt genomflöde och ger mer lagringsutrymme per nod.</span><span class="sxs-lookup"><span data-stu-id="bf65b-106">Since Kafka is very I/O heavy, [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md) is used tooprovide high throughput and provide more storage per node.</span></span> <span data-ttu-id="bf65b-107">Om traditionella virtuella hårddiskar (VHD) användes för Kafka, är varje nod begränsad too1 TB.</span><span class="sxs-lookup"><span data-stu-id="bf65b-107">If traditional virtual hard drives (VHD) were used for Kafka, each node is limited too1 TB.</span></span> <span data-ttu-id="bf65b-108">För hanterade diskar, kan du använda flera diskar tooachieve 16 TB för varje nod i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="bf65b-108">With managed disks, you can use multiple disks tooachieve 16 TB for each node in hello cluster.</span></span>

<span data-ttu-id="bf65b-109">hello innehåller följande diagram en jämförelse mellan Kafka på HDInsight innan hanterade diskar och Kafka i HDInsight med hanterade diskar:</span><span class="sxs-lookup"><span data-stu-id="bf65b-109">hello following diagram provides a comparison between Kafka on HDInsight before managed disks, and Kafka on HDInsight with managed disks:</span></span>

![Diagram över Kafka på HDInsight med hjälp av en enda virtuell hårddisk per virtuell dator jämfört med flera hanterade diskar per virtuell dator](./media/hdinsight-apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## <a name="configure-managed-disks-azure-portal"></a><span data-ttu-id="bf65b-111">Konfigurera hanterade diskar: Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bf65b-111">Configure managed disks: Azure portal</span></span>

1. <span data-ttu-id="bf65b-112">Åtgärderna i hello hello [skapar ett HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md) toounderstand hello vanliga steg toocreate ett kluster med hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="bf65b-112">Follow hello steps in hello [Create an HDInsight cluster](hdinsight-hadoop-create-linux-clusters-portal.md) toounderstand hello common steps toocreate a cluster using hello portal.</span></span> <span data-ttu-id="bf65b-113">Utför inte hello portal skapandeprocessen.</span><span class="sxs-lookup"><span data-stu-id="bf65b-113">Do not complete hello portal creation process.</span></span>

2. <span data-ttu-id="bf65b-114">Från hello __klusterstorleken__ blad, Använd hello __diskar per arbetsnoden__ fältet tooconfigure hello antal diskar.</span><span class="sxs-lookup"><span data-stu-id="bf65b-114">From hello __Cluster size__ blade, use hello __Disks per worker node__ field tooconfigure hello number of disks.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bf65b-115">hello hanterade disktyp kan vara antingen __Standard__ (HDD) eller __Premium__ (SSD).</span><span class="sxs-lookup"><span data-stu-id="bf65b-115">hello type of managed disk can be either __Standard__ (HDD) or __Premium__ (SSD).</span></span> <span data-ttu-id="bf65b-116">Premiumdiskar används med DS- och GS-serien virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="bf65b-116">Premium disks are used with DS and GS series VMs.</span></span> <span data-ttu-id="bf65b-117">Alla andra typer av virtuella dator använder standard.</span><span class="sxs-lookup"><span data-stu-id="bf65b-117">All other VM types use standard.</span></span>

    ![Bild av hello storlek klusterbladet med hello diskar per arbetsnoden markerat](./media/hdinsight-apache-kafka-scalability/set-managed-disks-portal.png)

## <a name="configure-managed-disks-resource-manager-template"></a><span data-ttu-id="bf65b-119">Konfigurera hanterade diskar: Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="bf65b-119">Configure managed disks: Resource Manager template</span></span>

<span data-ttu-id="bf65b-120">toocontrol hello antal diskar som används av hello arbetarnoder i ett kluster för Kafka, Använd hello följande avsnitt i hello mall:</span><span class="sxs-lookup"><span data-stu-id="bf65b-120">toocontrol hello number of disks used by hello worker nodes in a Kafka cluster, use hello following section of hello template:</span></span>

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

<span data-ttu-id="bf65b-121">Du hittar en fullständig mall som visar hur tooconfigure hanterade diskar på [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).</span><span class="sxs-lookup"><span data-stu-id="bf65b-121">You can find a complete template that demonstrates how tooconfigure managed disks at [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf65b-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bf65b-122">Next steps</span></span>

<span data-ttu-id="bf65b-123">Mer information om hur du arbetar med Kafka på HDInsight finns i hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="bf65b-123">For more information on working with Kafka on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="bf65b-124">Använda MirrorMaker toocreate en replik av Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="bf65b-124">Use MirrorMaker toocreate a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
* [<span data-ttu-id="bf65b-125">Använda Apache Storm med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="bf65b-125">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)
* [<span data-ttu-id="bf65b-126">Använda Apache Spark med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="bf65b-126">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)
* [<span data-ttu-id="bf65b-127">Ansluta tooKafka via ett virtuellt Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="bf65b-127">Connect tooKafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)

* [<span data-ttu-id="bf65b-128">HDInsight-blogg om hanterade diskar med Kafka</span><span class="sxs-lookup"><span data-stu-id="bf65b-128">HDInsight blog on managed disks with Kafka</span></span>](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)