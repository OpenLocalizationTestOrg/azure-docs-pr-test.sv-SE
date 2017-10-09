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
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a>Introduktion till Apache Kafka på HDInsight (förhandsversion)

[Apache Kafka](https://kafka.apache.org) är en öppen källkod distribuerade strömmande plattform som kan använda toobuild realtid strömmande data pipelines och program. Kafka ger också meddelandet broker funktioner liknande tooa meddelandekö, där du kan publicera och prenumerera på toonamed dataströmmar. Kafka på HDInsight ger dig en hanterad, skalbara och hög tillgänglighet tjänst i hello Microsoft Azure-molnet.

## <a name="why-use-kafka-on-hdinsight"></a>Varför använda Kafka på HDInsight?

Kafka innehåller hello följande funktioner:

* Publicera och prenumerera meddelanden mönster: Kafka innehåller ett producenten API för att publicera poster tooa Kafka avsnittet. hello konsumenten API används när prenumerera tooa avsnittet.

* Dataströmsbearbetning: Kafka används ofta med Apache Storm eller Spark för bearbetning av dataströmmen i realtid. Kafka 0.10.0.0 (HDInsight version 3.5) introducerades en strömmande API som gör att du toobuild strömning lösningar utan Storm eller Spark.

* Skala horisontellt: Kafka partitionerar dataströmmar över hello noderna i hello HDInsight-kluster. Konsumenten processer kan associeras med enskilda partitioner tooprovide nätverksbelastning när förbrukar poster.

* I ordning: inom varje partition poster som lagras i hello dataströmmen i hello ordning som de har tagits emot. Genom att associera en konsumentprocess per partition bearbetas posterna garanterat i ordning.

* Feltoleranta:, Partitioner kan replikeras mellan noder tooprovide feltolerans.

* Integrering med Azure hanterade diskar: hanterade diskar ger högre skala och genomströmning för hello-diskar som används av hello virtuella datorer i hello HDInsight-kluster.

    Hanterade diskar är aktiverat som standard för Kafka i HDInsight och hello antalet diskar som används per nod kan konfigureras när HDInsight skapas. För mer information om hanterade diskar, se [Hanterade Azure-diskar](../virtual-machines/windows/managed-disks-overview.md).

    Se [Ökad skalbalhet med Kafta på HDInsight](hdinsight-apache-kafka-scalability.md) för mer information om att konfigurera hanterade diskar med Kafka på HDInsight.

## <a name="use-cases"></a>Användningsfall

* **Messaging**: eftersom den stöder hello Publicera prenumerera-mönster för meddelandet, Kafka används ofta som en koordinator för meddelandet.

* **Aktivitetsspårning**: eftersom Kafka tillhandahåller i ordning loggning med poster, den kan använda tootrack och skapa aktiviteter. Till exempel användaråtgärder på en webbplats eller i ett program.

* **Aggregeringen**: med bearbetning av dataströmmen kan du samla information från olika dataströmmar toocombine och centralisera hello information till användningsdata.

* **Omvandling**: Med dataströmsbearbetning kan du kombinera och utöka data från flera inkommande avsnitt i ett eller flera utdataämnen.

## <a name="next-steps"></a>Nästa steg

Använd hello följande länkar toolearn hur toouse Apache Kafka på HDInsight:

* [Kom igång med Kafka på HDInsight](hdinsight-apache-kafka-get-started.md)

* [Använda MirrorMaker toocreate en replik av Kafka på HDInsight](hdinsight-apache-kafka-mirroring.md)

* [Använda Apache Storm med Kafka på HDInsight](hdinsight-apache-storm-with-kafka.md)

* [Använda Apache Spark med Kafka på HDInsight](hdinsight-apache-spark-with-kafka.md)

* [Ansluta tooKafka via ett virtuellt Azure-nätverk](hdinsight-apache-kafka-connect-vpn-gateway.md)
