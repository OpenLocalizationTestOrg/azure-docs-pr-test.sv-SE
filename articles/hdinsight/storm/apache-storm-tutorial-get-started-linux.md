---
title: "Storm-startexempel som finns på Apache Storm på HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur du analyserar stordata och bearbeta data i realtid med storm starter-exempel och Apache Storm på HDInsight."
keywords: storm-starter apache storm-exempel
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: d710dcac-35d1-4c27-a8d6-acaf8146b485
ms.service: hdinsight
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 19ab428913517e4f3df156c93782fe23f1cd67ec
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/07/2017
---
#<a name="get-started-with-apache-storm-on-hdinsight-using-the-storm-starter-examples"></a>Kom igång med Apache Storm på HDInsight med storm starter-exempel

Lär dig att använda Apache Storm på HDInsight med storm starter-exempel.

Apache Storm är ett skalbart, feltolerant och distribuerat system för beräkningar i realtid för bearbetning av dataströmmar. Du kan skapa ett molnbaserat Storm-kluster som utför analyser av stordata i realtid med Storm på Azure HDInsight.

> [!IMPORTANT]
> Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare. Mer information finns i [HDInsight-avveckling på Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Krav

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **kunskap om SSH och SCP**. Mer information finns i [Use SSH with HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

## <a name="create-a-storm-cluster"></a>Skapa ett Storm-kluster

Använd följande steg om du vill skapa en Storm i HDInsight-klustret:

1. Från [Azure Portal](https://portal.azure.com) väljer du **+ NY**, **Data och analys** och väljer sedan **HDInsight**.

    ![Skapa ett HDInsight-kluster](./media/apache-storm-tutorial-get-started-linux/create-hdinsight.png)

2. Ange följande information i avsnittet **Grundläggande inställningar**:

    * **Klusternamn**: Namnet på HDInsight-klustret.
    * **Prenumeration**: Välj den prenumeration som du vill använda.
    * **Användarnamn för klusterinloggning** och **Lösenord för klusterinloggning**: Inloggningen vid åtkomst till klustret via HTTPS. Du kan använda dessa autentiseringsuppgifter för att få åtkomst till tjänster som Ambari-webbgränssnittet eller REST API.
    * **Secure Shell-användarnamn (SSH)**: Den inloggning som används vid åtkomst till klustret via SSH. Som standard är lösenordet detsamma som lösenordet för klusterinloggning.
    * **Resursgrupp**: Resursgruppen som klustret ska skapas i.
    * **Plats**: Azure-region som klustret ska skapas i.

   ![Välj en prenumeration](./media/apache-storm-tutorial-get-started-linux/hdinsight-basic-configuration.png)

3. Välj **Klustertyp** och ange följande värden i avsnittet **Klusterkonfiguration**:

    * **Typ av kluster**: Storm

    * **Operativsystem**: Linux

    * **Version**: Storm 1.1.0 (HDI 3.6)

    * **Klusternivå**: Standard

   Slutligen kan spara inställningarna med kommandot **Välj**.

    ![Välj klustertyp](./media/apache-storm-tutorial-get-started-linux/set-hdinsight-cluster-type.png)

4. När du har valt klustertypen anger du klustertypen med hjälp av knappen __Välj__. Använd sedan knappen __Nästa__ och slutföra den grundläggande konfigurationen.

5. Gå till avsnittet **Lagring** och välj eller skapa ett lagringskonto. Lämna övriga fält i det här avsnittet på standardvärden för anvisningarna i det här dokumentet. Spara lagringskonfigurationen genom att klicka på __Nästa__.

    ![Ange inställningarna för lagringskontot för HDInsight](./media/apache-storm-tutorial-get-started-linux/set-hdinsight-storage-account.png)

6. Gå till avsnittet **Sammanfattning** och granska konfigurationen för klustret. Använd länkarna __Redigera__ om du behöver ändra eventuella inställningar som är felaktiga. Till sist skapar du klustret genom att klicka på __Skapa__.

    ![Sammanfattning av klusterkonfiguration](./media/apache-storm-tutorial-get-started-linux/hdinsight-configuration-summary.png)

    > [!NOTE]
    > Det kan ta upp till 20 minuter att skapa klustret.

## <a name="run-a-storm-starter-sample-on-hdinsight"></a>Köra ett Storm Starter-exempel i HDInsight

1. Anslut till HDInsight-klustret med hjälp av SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!TIP]
    > SSH-klienten kan ange att värdens äkthet inte kan fastställas. Ange i så fall `yes` för att fortsätta.

    > [!NOTE]
    > Om du skyddat SSH-användarkontot med lösenord uppmanas du att ange det. Om du använde en offentlig nyckel kan du behöva använda `-i`-parametern för att ange motsvarande privata nyckel. Till exempel `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

    Mer information finns i [Use SSH with HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Använd följande kommando för att starta en exempeltopologi:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount

    Det här kommandot startar exempeltopologin för WordCount (ordräkning) på klustret. Den här topologin genererar meningar slumpmässigt och räknar hur många gånger ord används. Topologins egna namn är `wordcount`.

    > [!NOTE]
    > När du skickar in dina egna topologier till klustret måste du först kopiera jar-filen som innehåller klustret innan du använder kommandot `storm`. Använd `scp`-kommandot för att kopiera filen. Till exempel, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > WordCount-exemplet, och andra storm-starterexempel ingår redan i ditt kluster på `/usr/hdp/current/storm-client/contrib/storm-starter/`.

Om du är intresserad av att se källan för storm-starterexempel kan du hämta koden på [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter). Länken är till Storm 1.1.x, som medföljer HDInsight 3.6. Om du vill välja någon annan version av Storm väljer du version med knappen __Gren__ överst på sidan.

## <a name="monitor-the-topology"></a>Övervaka topologin

Storm-användargränssnittet innehåller ett webbgränssnitt för att arbeta med topologier som körs och ingår i ditt HDInsight-kluster.

Genomför följande för att övervaka topologin med hjälp av Storm-användargränssnittet:

1. Visa Storm-användargränssnittet genom att öppna en webbläsare på `https://CLUSTERNAME.azurehdinsight.net/stormui`. Ersätt **CLUSTERNAME** med namnet på klustret.

    > [!NOTE]
    > Om du uppmanas att ange ett användarnamn och lösenord, anger du klusteradministratören (admin) och lösenordet du använde när du skapade klustret.

2. Under **Topology summary** (Topologiöversikt) väljer du posten **wordcount** (ordräkning) i kolumnen **Name** (namn). Detta visar information om topologin.

    ![Storm Dashboard med storm-starter, topologisk information om WordCount.](./media/apache-storm-tutorial-get-started-linux/topology-summary.png)

    På sidan finns följande information:

    * **Topology stats** (Topologistatistik) – grundläggande information om topologins prestanda ordnad i tidsfönster.

        > [!NOTE]
        > När du markerar ett specifikt tidsfönster ändras tidsfönstret för information som visas i andra avsnitt på sidan.

    * **Spouts** (Kanaler) – grundläggande information om kanaler, inklusive det senaste fel som returnerats för varje kanal.

    * **Bolts** (Bultar) – grundläggande information om bultar.

    * **Topologi configuration** (Topologikonfiguration) – detaljerad information om topologins konfiguration.

    På sidan anges också åtgärder som kan göras på topologin:

    * **Activate** (Aktivera) – återupptar bearbetningen av en inaktiverad topologi.

    * **Deactivate**  (Inaktivera) – pausar en topologi som körs.

    * **Rebalance** (Balansera) – justerar topologins parallellitet. Du bör balansera om topologier som körs när du har ändrat antalet noder i klustret. Ombalansering justerar parallelliteten och kompenserar för det ökade/minskade antalet noder i klustret. Mer information finns i [Förstå parallellitet i en Storm-topologi](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

    * **Kill** (Avsluta) – avslutar en Storm-topologi efter en angiven tidsgräns.

3. På den här sidan väljer du en post från avsnittet **Spouts** (Kanaler) eller **Bolts** (Bultar). Detta visar information om den valda komponenten.

    ![Storm-instrumentpanelen med information om valda komponenter.](./media/apache-storm-tutorial-get-started-linux/component-summary.png)

    På sidan visas följande information:

    * **Spout/Bolt stats** (Statistik för kanaler/bultar) – grundläggande information om komponentens prestanda, ordnad i tidsfönster.

        > [!NOTE]
        > När du markerar ett specifikt tidsfönster ändras tidsfönstret för information som visas i andra avsnitt på sidan.

    * **Input stats** (Statistik för indata) (endast bultar) – information om komponenter som producerar de data som används av bulten.

    * **Output stats** (Statistik för utdata) – information om data som sänds av bulten.

    * **Executors** (Utförare) – information om komponentens instanser.

    * **Errors** (Fel) – fel som komponenten ger upphov till.

4. När du visar information om en kanal eller en bult väljer du en post i kolumnen **Port** i avsnittet **Executors** (Utförare) för att visa information för en viss komponentinstans.

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    I det här exemplet förekommer ordet **seven** (sju) 1 493 957 gånger. Det är antalet gånger ordet har påträffats sedan topologin startades.

## <a name="stop-the-topology"></a>Stoppa topologin

Gå tillbaka till sidan **Topology summary** (Topologiöversikt) för ordräkningstopologin och välj knappen **Kill** (Avsluta) i avsnittet **Topology actions** (Topologiåtgärder). När du uppmanas ange antal sekunder innan topologin stoppas, anger du 10. När tidsgränsen uppnåtts visas topologin inte längre när du går in på avsnittet **Storm UI** (Storm-användargränssnitt) på instrumentpanelen.

## <a name="delete-the-cluster"></a>Ta bort klustret

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](../hdinsight-administer-use-portal-linux.md#create-clusters).

## <a id="next"></a>Nästa steg

I den här självstudien för Apache Storm har du lärt dig grunderna för att arbeta med Storm på HDInsight. Härnäst får du lära dig att [Utveckla Java-baserade topologier med Maven](apache-storm-develop-java-topology.md).

Om du redan är bekant med att utveckla Java-baserade topologier kan du läsa dokumentet [Distribuera och hantera Apache Storm-topologier i HDInsight](apache-storm-deploy-monitor-topology-linux.md).

Om du är en .NET-utvecklare kan du skapa C#- eller C#/Java-hybridtopologier med Visual Studio. Mer information finns i [Utveckla C#-topologier för Apache Storm på HDInsight med Hadoop-verktyg för Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md).

Se följande exempel för exempeltopologier som kan användas med Storm på HDInsight:

* [Exempeltopologier för Storm på HDInsight](apache-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[preview-portal]: https://portal.azure.com/
