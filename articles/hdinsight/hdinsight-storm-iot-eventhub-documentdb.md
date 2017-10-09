---
title: "aaaProcess vehicle sensordata med Apache Storm på HDInsight | Microsoft Docs"
description: "Lär dig hur tooprocess vehicle sensordata från Händelsehubbar med Apache Storm på HDInsight. Lägg till modelldata från Azure Cosmos-databasen och lagra utdata toostorage."
services: hdinsight,documentdb,notification-hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 78980635-8bef-4c33-96c3-fae50e932e31
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/03/2017
ms.author: larryfr
ms.openlocfilehash: 8f7b1dbb9072e095ea32160bb731bedd071288af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Bearbeta vehicle sensordata från Azure Event Hubs med Apache Storm på HDInsight

Lär dig hur tooprocess vehicle sensordata från Azure Event Hubs med Apache Storm på HDInsight. Det här exemplet läser sensordata från Azure Event Hubs, förbättra hello data genom att referera till data som lagras i Azure Cosmos DB. hello data lagras i Azure Storage med hjälp av hello Hadoop File System (HDFS).

![HDInsight och hello Arkitekturdiagram Sakernas Internet (IoT)](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

## <a name="overview"></a>Översikt

Lägger till sensorer toovehicles kan du toopredict utrustning problem baserat på historisk data, trender. Det gör även toomake förbättringar toofuture versioner baserat på mönstret användningsanalys. Du måste kunna tooquickly och effektivt läsa hello data från alla fordon till Hadoop innan MapReduce bearbetning kan ske. Dessutom kan du toodo analys för kritiskt fel sökvägar (motorn temperatur, bromsar osv.) i realtid.

Händelsehubbar i Azure bygger toohandle hello enorma mängder data som genereras av sensorer. Apache Storm kan vara används tooload och bearbeta hello data innan de lagras i HDFS.

## <a name="solution"></a>Lösning

Telemetridata för motorn temperatur, temperatur och hastigheten registreras av sensorer. Informationen skickas sedan tooEvent hubbar tillsammans med hello bil Vehicle identifiering numret (VIN) och en tidsstämpel. Därifrån kan en Storm-topologi som körs på en Apache Storm på HDInsight-kluster läser hello data, bearbetar och lagrar den i HDFS.

Under bearbetning av är hello VIN används tooretrieve informationen från Cosmos-databasen. Den här informationen läggs toohello dataströmmen innan den är lagrad.

hello-komponenter som används i hello Storm-topologi är:

* **EventHubSpout** -läser data från Azure Event Hubs
* **TypeConversionBolt** -konverterar hello JSON-strängen från Händelsehubbar i en tuppel som innehåller följande sensordata hello:
    * Motorn temperatur
    * Temperatur
    * Hastighet
    * VIN
    * tidsstämpel
* **DataReferencBolt** -slår upp hello vehicle modellen från Cosmos-databasen med hello VIN
* **WasbStoreBolt** -butiker hello data tooHDFS (Azure Storage)

hello följande bild är ett diagram över den här lösningen:

![Storm-topologi](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

## <a name="implementation"></a>Implementering

En fullständig automatisk lösning för det här scenariot är tillgänglig som en del av hello [HDInsight-Storm-exempel](https://github.com/hdinsight/hdinsight-storm-examples) databasen på GitHub. toouse det här exemplet, följ hello stegen i hello [IoTExample README. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Nästa steg

Flera exempel på Storm-topologier, se [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md).

