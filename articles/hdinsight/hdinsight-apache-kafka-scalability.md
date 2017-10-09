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
# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a>Konfigurera lagring och skalbarhet för Apache Kafka på HDInsight

Lär dig hur tooconfigure hello antalet hanterade diskar som används av Apache Kafka på HDInsight.

Kafka på HDInsight använder hello lokal disk hello virtuella datorer i hello HDInsight-kluster. Eftersom Kafka är mycket i/o-stor, [Azure hanterade diskar](../virtual-machines/windows/managed-disks-overview.md) är används tooprovide högt genomflöde och ger mer lagringsutrymme per nod. Om traditionella virtuella hårddiskar (VHD) användes för Kafka, är varje nod begränsad too1 TB. För hanterade diskar, kan du använda flera diskar tooachieve 16 TB för varje nod i klustret hello.

hello innehåller följande diagram en jämförelse mellan Kafka på HDInsight innan hanterade diskar och Kafka i HDInsight med hanterade diskar:

![Diagram över Kafka på HDInsight med hjälp av en enda virtuell hårddisk per virtuell dator jämfört med flera hanterade diskar per virtuell dator](./media/hdinsight-apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## <a name="configure-managed-disks-azure-portal"></a>Konfigurera hanterade diskar: Azure-portalen

1. Åtgärderna i hello hello [skapar ett HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md) toounderstand hello vanliga steg toocreate ett kluster med hello-portalen. Utför inte hello portal skapandeprocessen.

2. Från hello __klusterstorleken__ blad, Använd hello __diskar per arbetsnoden__ fältet tooconfigure hello antal diskar.

    > [!NOTE]
    > hello hanterade disktyp kan vara antingen __Standard__ (HDD) eller __Premium__ (SSD). Premiumdiskar används med DS- och GS-serien virtuella datorer. Alla andra typer av virtuella dator använder standard.

    ![Bild av hello storlek klusterbladet med hello diskar per arbetsnoden markerat](./media/hdinsight-apache-kafka-scalability/set-managed-disks-portal.png)

## <a name="configure-managed-disks-resource-manager-template"></a>Konfigurera hanterade diskar: Resource Manager-mall

toocontrol hello antal diskar som används av hello arbetarnoder i ett kluster för Kafka, Använd hello följande avsnitt i hello mall:

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

Du hittar en fullständig mall som visar hur tooconfigure hanterade diskar på [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).

## <a name="next-steps"></a>Nästa steg

Mer information om hur du arbetar med Kafka på HDInsight finns i hello följande dokument:

* [Använda MirrorMaker toocreate en replik av Kafka på HDInsight](hdinsight-apache-kafka-mirroring.md)
* [Använda Apache Storm med Kafka på HDInsight](hdinsight-apache-storm-with-kafka.md)
* [Använda Apache Spark med Kafka på HDInsight](hdinsight-apache-spark-with-kafka.md)
* [Ansluta tooKafka via ett virtuellt Azure-nätverk](hdinsight-apache-kafka-connect-vpn-gateway.md)

* [HDInsight-blogg om hanterade diskar med Kafka](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)