---
title: aaaUse Ambari Tez vy med HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toouse hello Ambari Tez visa toodebug Tez jobb i HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 9c39ea56-670b-4699-aba0-0f64c261e411
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 5d61bd0403c98284c86982073af91468ae62ac60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-ambari-views-toodebug-tez-jobs-on-hdinsight"></a>Använda Ambari Views toodebug Tez jobb i HDInsight

Hej Ambari-Webbgränssnittet för HDInsight innehåller en vy i Tez som kan använda toounderstand och felsöka jobb som använder Tez. Hej Tez vy kan du toovisualize hello jobb som ett diagram över anslutna objekt detaljer om varje objekt och hämta statistik och loggningsinformation.

> [!IMPORTANT]
> hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight component-versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Krav

* Ett Linux-baserat HDInsight-kluster. Anvisningar om hur du skapar ett kluster finns [komma igång med Linux-baserat HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
* En modern webbläsare som stöder HTML5.

## <a name="understanding-tez"></a>Förstå Tez

Tez är ett utökningsbart ramverk för databearbetning i Hadoop och som ger högre hastigheter än traditionella MapReduce-bearbetning. För Linux-baserade HDInsight-kluster är det hello standardmotorn för Hive.

Tez skapar en dirigeras acykliska diagram (DAG) som beskriver hello ordning för åtgärder som krävs av jobb. Enskilda åtgärder kallas formhörnen och köra en typ av hello övergripande jobb. hello faktiska utförande av hello beskrivs av en nod kallas för en aktivitet och kan distribueras över flera noder i klustret hello.

### <a name="understanding-hello-tez-view"></a>Förstå hello Tez vy

Hej Tez-vyn innehåller både historisk information och information om processer som körs. Den här informationen visar hur ett jobb är fördelad över kluster. Visar även räknare som används av uppgifter och formhörnen och information om fel relaterade toohello jobb. Den kan erbjuda användbar information i hello följande scenarier:

* Övervaka tidskrävande processer, visa hello förloppet för kartan och minska uppgifter.
* Analysera historiska data för lyckade eller misslyckade processer toolearn hur bearbetning kan förbättras eller orsaken till felet.

## <a name="generate-a-dag"></a>Generera en DAG

hello Tez-vyn innehåller bara data om ett jobb som använder hello Tez-motorn är för närvarande körs eller har varit kördes tidigare. Enkel Hive-frågor kan lösas utan att använda Tez. Mer komplexa frågor som vill filtrering, gruppering, sortering, kopplingar och så vidare. Använd hello Tez-motorn.

Använd följande steg toorun en Hive-fråga som använder Tez hello:

1. I en webbläsare, navigerar du toohttps://CLUSTERNAME.azurehdinsight.net, där **KLUSTERNAMN** är hello namnet på ditt HDInsight-kluster.

2. Välj hello hello menyn hello överst på sidan hello **vyer** ikon. Den här ikonen som ser ut som en serie rutor. I hello listrutan som visas väljer du **Hive visa**.

    ![Du har markerat Hive-vy](./media/hdinsight-debug-ambari-tez-view/selecthive.png)

3. När hello Hive-vyn läses in, klistra in hello följande fråga i hello frågeredigeraren och klicka sedan på **köra**.

        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;

    När hello jobbet har slutförts visas hello utdata som visas i hello **Frågeprocessresultat** avsnitt. hello resultaten ska vara liknande toohello följande text:

        market  state       country
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica

4. Välj hello **loggen** fliken. Du kan se information liknande toohello följande text:

        INFO : Session is already open
        INFO :

        INFO : Status: Running (Executing on YARN cluster with App id application_1454546500517_0063)

    Spara hello **App-id** värdet som det här värdet används i nästa avsnitt om hello.

## <a name="use-hello-tez-view"></a>Använd hello Tez vy

1. Välj hello hello menyn hello överst på sidan hello **vyer** ikon. I hello listrutan som visas väljer du **Tez visa**.

    ![Du har markerat Tez vy](./media/hdinsight-debug-ambari-tez-view/selecttez.png)

2. När hello Tez vyn läses in, visas en lista över hive-frågor som körs eller har körts på hello klustret.

    ![Alla dag](./media/hdinsight-debug-ambari-tez-view/tez-view-home.png)

3. Om du har en enda post, är det för hello-fråga som du körde hello föregående avsnitt. Om du har flera poster kan söka du genom att använda hello fält hello överst på hello sidan.

4. Välj hello **fråge-ID** för en Hive-fråga. Information om hello frågan visas.

    ![DAG-information](./media/hdinsight-debug-ambari-tez-view/query-details.png)

5. hello flikarna på den här sidan kan du tooview hello följande information:

    * **Fråga information**: information om hello Hive-fråga.
    * **Tidslinjen**: Information om hur lång tid tog av varje steg i processen.
    * **Konfigurationer**: hello-konfiguration som använts för den här frågan.

    Från __Frågedetaljer__ du kan använda hello länkar toofind information om hello __programmet__ eller hello __DAG__ för den här frågan.
    
    * Hej __programmet__ länken visar information om hello YARN-programmet för den här frågan. Du kan komma åt programmet hello YARN-loggar från den här.
    * Hej __DAG__ länken visar information om hello riktat acykliskt diagram för den här frågan. Härifrån kan du visa en grafisk representation av hello DAG. Du kan också hitta information om hello formhörnen i hello DAG.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur toouse hello Tez visa, lär du dig mer om [med hjälp av Hive i HDInsight](hdinsight-use-hive.md).

Mer teknisk information om Tez finns hello [Tez sidan vid Hortonworks](http://hortonworks.com/hadoop/tez/).

Mer information om hur du använder Ambari med HDInsight finns [hantera HDInsight-kluster med hello Ambari-Webbgränssnittet](hdinsight-hadoop-manage-ambari.md)
