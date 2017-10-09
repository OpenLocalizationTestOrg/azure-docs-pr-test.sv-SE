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
# <a name="high-availability-of-your-data-with-apache-kafka-preview-on-hdinsight"></a>Hög tillgänglighet för dina data med Apache Kafka (förhandsversion) på HDInsight

Lär dig hur tooconfigure partition repliker för Kafka avsnitt tootake nytta av underliggande maskinvara rack konfiguration. Den här konfigurationen garanterar hello tillgängligheten för data som lagras i Apache Kafka på HDInsight.

## <a name="fault-and-update-domains-with-kafka"></a>Fel- och uppdateringsdomäner med Kafka

En feldomän är en logisk gruppering av underliggande maskinvara i ett Azure-datacenter. Varje feldomän delar en gemensam strömkälla och nätverksbrytare. hello virtuella datorer och hanterade diskar som implementerar hello noder i ett HDInsight-kluster är fördelade på dessa feldomäner. Den här arkitekturen begränsar hello potentiella effekten av fysiska maskinvarufel.

Varje Azure-region har ett visst antal feldomäner. En lista över domäner och hello antalet feldomäner som de innehåller finns hello [tillgänglighetsuppsättningar](../virtual-machines/linux/regions-and-availability.md#availability-sets) dokumentation.

> [!IMPORTANT]
> Kafka har ingen information om feldomäner. När du skapar ett ämne i Kafka, den kan lagra alla partition repliker i hello samma feldomän. toosolve det här problemet så vi tillhandahåller hello [Kafka partition balansera verktyget](https://github.com/hdinsight/hdinsight-kafka-tools).

## <a name="when-toorebalance-partition-replicas"></a>När toorebalance partitions-repliker

tooensure hello högsta tillgänglighet för dina data Kafka, du bör balansera hello partition repliker för ditt avsnitt på hello följande tider:

* När du skapar ett nytt ämne eller en ny partition

* När du skalar upp ett kluster

## <a name="replication-factor"></a>Replikeringsfaktor

> [!IMPORTANT]
> Vi rekommenderar att du använder en Azure-region som innehåller tre feldomäner, och använder replikeringsfaktorn 3.

Om du måste använda en region med bara två feldomäner, kan du använda replikering upp till 4 toospread hello repliker jämnt över hello två feldomäner.

Ett exempel för att skapa ämnen och inställningen hello replikering faktor finns hello [börjar med Kafka på HDInsight](hdinsight-apache-kafka-get-started.md) dokumentet.

## <a name="how-toorebalance-partition-replicas"></a>Hur toorebalance partitions-repliker

Använd hello [Kafka partition balansera verktyget](https://github.com/hdinsight/hdinsight-kafka-tools) toorebalance markerat hjälpavsnitt. Det här verktyget måste vara körde från en SSH-session toohello huvudnod Kafka klustret.

Mer information om hur du ansluter med hjälp av SSH tooHDInsight finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.

## <a name="next-steps"></a>Nästa steg

* [Skalbarhet för Kafka i HDInsight](hdinsight-apache-kafka-scalability.md)
* [Spegling med Kafka i HDInsight](hdinsight-apache-kafka-mirroring.md)