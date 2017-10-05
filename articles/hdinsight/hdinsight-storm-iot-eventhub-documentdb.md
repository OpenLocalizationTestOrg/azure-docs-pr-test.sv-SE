---
title: "Bearbeta vehicle sensordata med Apache Storm på HDInsight | Microsoft Docs"
description: "Lär dig mer om att bearbeta vehicle sensordata från Händelsehubbar med Apache Storm på HDInsight. Lägg till modelldata från Azure Cosmos-databasen och lagra utdata till lagring."
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
ms.openlocfilehash: 8e8ebc724e1c70e8fcd56312adef5da2342373ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Bearbeta vehicle sensordata från Azure Event Hubs med Apache Storm på HDInsight

Lär dig mer om att bearbeta vehicle sensordata från Azure Event Hubs med Apache Storm på HDInsight. Det här exemplet läser sensordata från Azure Event Hubs, förbättra data genom att referera till data som lagras i Azure Cosmos DB. Data lagras i Azure Storage med Hadoop File System (HDFS).

![HDInsight och arkitekturdiagram Sakernas Internet (IoT)](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

## <a name="overview"></a>Översikt

Att lägga till sensorer fordon kan du förutsäga utrustning problem baserat på historisk data, trender. Du kan också för att förbättra kommande versioner baserat på mönstret användningsanalys. Du måste kunna snabbare och effektivare Läs in data från alla fordon till Hadoop innan MapReduce bearbetning kan ske. Dessutom kan du göra analyser för kritiskt fel sökvägar (motorn temperatur, bromsar osv.) i realtid.

Händelsehubbar i Azure är utformat för att hantera data som genereras av sensorer massiv volymen. Apache Storm kan användas för att läsa in och bearbeta data innan de lagras i HDFS.

## <a name="solution"></a>Lösning

Telemetridata för motorn temperatur, temperatur och hastigheten registreras av sensorer. Informationen skickas sedan till Händelsehubbar tillsammans med den bilen Vehicle identifiering numret (VIN) och en tidsstämpel. Därifrån kan en Storm-topologi som körs på en Apache Storm på HDInsight-kluster läser data, bearbetar och lagrar den i HDFS.

Under bearbetning av används VIN för att hämta informationen från Cosmos-databasen. Dessa data läggs till dataströmmen innan den är lagrad.

Storm-topologi-komponenter är:

* **EventHubSpout** -läser data från Azure Event Hubs
* **TypeConversionBolt** -konverterar JSON-strängen från Händelsehubbar till en tuppel som innehåller följande sensordata:
    * Motorn temperatur
    * Temperatur
    * Hastighet
    * VIN
    * tidsstämpel
* **DataReferencBolt** -slår upp vehicle modellen från Cosmos-databasen med VIN
* **WasbStoreBolt** -lagrar data till HDFS (Azure Storage)

Nedan visas ett diagram över den här lösningen:

![Storm-topologi](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

## <a name="implementation"></a>Implementering

En fullständig automatisk lösning för det här scenariot är tillgängliga som en del av den [HDInsight-Storm-exempel](https://github.com/hdinsight/hdinsight-storm-examples) databasen på GitHub. Om du vill använda det här exemplet, följer du stegen i den [IoTExample README. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Nästa steg

Flera exempel på Storm-topologier, se [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md).

