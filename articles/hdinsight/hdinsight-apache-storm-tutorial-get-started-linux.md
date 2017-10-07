---
title: "aaaStorm-startexempel som finns på Apache Storm på HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toodo stordata och bearbeta data i realtid med Apache Storm och hello storm starter-exempel i HDInsight."
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
ms.date: 08/15/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: bb6d6549e67ca5b557f0692f98c89692a87267b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#<a name="get-started-with-apache-storm-on-hdinsight-using-hello-storm-starter-examples"></a>Kom igång med Apache Storm på HDInsight med hello storm starter-exempel

Lär dig hur toouse Apache Storm i HDInsight med hello storm starter-exempel.

Apache Storm är ett skalbart, feltolerant och distribuerat system för beräkningar i realtid för bearbetning av dataströmmar. Du kan skapa ett molnbaserat Storm-kluster som utför analyser av stordata i realtid med Storm på Azure HDInsight.

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Krav

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **kunskap om SSH och SCP**. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

## <a name="create-a-storm-cluster"></a>Skapa ett Storm-kluster

Använd hello följa steg toocreate ett Storm på HDInsight-kluster:

1. Från hello [Azure-portalen](https://portal.azure.com)väljer **+ ny**, **Intelligence + analys**, och välj sedan **HDInsight**.

    ![Skapa ett HDInsight-kluster](./media/hdinsight-apache-storm-tutorial-get-started-linux/create-hdinsight.png)

2. Från hello **grunderna** bladet ange hello följande information:

    * **Klusternamn**: hello namnet på hello HDInsight-kluster.
    * **Prenumerationen**: Välj hello prenumeration toouse.
    * **Klustret inloggning användarnamn** och **klustret inloggningslösenordet**: hello inloggning vid åtkomst till hello klustret via HTTPS. Du använder dessa autentiseringsuppgifter tooaccess tjänster, till exempel hello Ambari-Webbgränssnittet eller REST API.
    * **Secure Shell (SSH) användarnamn**: hello-inloggning som används vid åtkomst till hello klustret via SSH. Som standard är hello lösenord hello samma som hello inloggningslösenordet för klustret.
    * **Resursgruppen**: hello resurs grupp toocreate hello klustret i.
    * **Plats**: hello Azure-region toocreate hello klustret i.

    ![Välj en prenumeration](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-basic-configuration.png)

3. Välj **kluster typen**, och sedan ange hello följa värden på hello **klusterkonfigurationen** bladet:

    * **Typ av kluster**: Storm

    * **Operativsystem**: Linux

    * **Version**: Storm 1.1.0 (HDI 3.6)

    * **Klusternivå**: Standard

    Använd slutligen hello **Välj** knappen toosave inställningar.

    ![Välj klustertyp](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-cluster-type.png)

4. När du har valt hello typ av kluster, använda hello __Välj__ knappen tooset hello typ av kluster. Använd sedan hello __nästa__ toofinish grundläggande konfiguration.

5. Från hello **lagring** bladet Välj eller skapa ett lagringskonto. Lämna hello andra fält för hello stegen i det här dokumentet, på det här bladet på hello standardvärden. Använd hello __nästa__ toosave konfiguration för lagring.

    ![Ange hello konto lagringsinställningarna för HDInsight](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-storage-account.png)

6. Från hello **sammanfattning** bladet granska hello konfigurationen för hello-kluster. Använd hello __redigera__ länkar toochange inställningar som är felaktiga. Slutligen Använd the__Create__ knappen toocreate hello-kluster.

    ![Sammanfattning av klusterkonfiguration](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-configuration-summary.png)

    > [!NOTE]
    > Det kan ta upp too20 minuter toocreate hello klustret.

## <a name="run-a-storm-starter-sample-on-hdinsight"></a>Köra ett Storm Starter-exempel i HDInsight

1. Anslut toohello HDInsight-kluster med SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Om du har använt ett lösenord toosecure SSH-användarkontot, är du tillfrågas tooenter den. Om du använder en offentlig nyckel, du måste använda hello `-i` parametern toospecify hello motsvarande privata nyckel. Till exempel `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Använd följande kommando toostart en exempeltopologi hello:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount

    > [!NOTE]
    > I tidigare versioner av HDInsight hello klassnamnet för hello topologin är `storm.starter.WordCountTopology` i stället för `org.apache.storm.starter.WordCountTopology`.

    Detta kommando startar hello exempel WordCount-topologi på hello-kluster med ett eget namn för 'wordcount'. Den genererar slumpmässigt meningar och antal hello förekomsten av varje ord i hello meningar.

    > [!NOTE]
    > När ditt eget topologier toohello kluster, måste du först kopiera hello jar-filen som innehåller hello klustret innan du använder hello `storm` kommando. Använd hello `scp` kommandofilen toocopy hello. Till exempel, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > Hej WordCount-exemplet och andra storm starter-exempel finns redan i klustret på `/usr/hdp/current/storm-client/contrib/storm-starter/`.

Om du vill visa hello källa för hello storm-startexempel som finns hittar du hello koden i [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter). Länken är till Storm 1.1.x, som medföljer HDInsight 3.6. För andra versioner av Storm använda hello __gren__ knappen hello överst i hello sidan tooselect en annan Storm-version.

## <a name="monitor-hello-topology"></a>Övervakaren hello-topologi

hello Storm-Användargränssnittet innehåller ett webbgränssnitt för att arbeta med topologier som körs och ingår i ditt HDInsight-kluster.

Använd följande steg toomonitor hello topologi med hello Storm-Användargränssnittet hello:

1. toodisplay hello Storm-Användargränssnittet, öppna en webbläsare web-toohttps://CLUSTERNAME.azurehdinsight.net/stormui. Ersätt **KLUSTERNAMN** med hello namnet på klustret.

    > [!NOTE]
    > Om du blir tillfrågad tooprovide ett användarnamn och lösenord, ange hello Klusteradministratören (admin) och lösenord som du använde när du skapar hello-klustret.

2. Under **Topology summary**väljer hello **wordcount** post i hello **namn** kolumn. Information om hello topologin visas.

    ![Storm Dashboard med storm-starter, topologisk information om WordCount.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)

    Den här sidan innehåller hello följande information:

    * **Topology stats** – grundläggande information om hello topologins prestanda ordnad i tidsfönster.

        > [!NOTE]
        > Att välja en viss tid fönstret ändringar hello tidsfönstret för information som visas i andra avsnitt i hello-sidan.

    * **Spouts** – grundläggande information om kanaler, inklusive hello senaste fel som returnerats för varje kanal.

    * **Bolts** (Bultar) – grundläggande information om bultar.

    * **Topologi configuration** -detaljerad information om hello topologins konfiguration.

    Den här sidan innehåller också åtgärder som kan göras på topologin hello:

    * **Activate** (Aktivera) – återupptar bearbetningen av en inaktiverad topologi.

    * **Deactivate**  (Inaktivera) – pausar en topologi som körs.

    * **Balansera** -justerar hello parallellitet hello-topologi. Du bör balansera om topologier som körs när du har ändrat hello antalet noder i klustret hello. Ombalansering justerar parallellitet toocompensate för hello ökade/minskade antalet noder i klustret hello. Mer information finns i [förstå hello parallellitet i en Storm-topologi](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

    * **Avsluta** -avslutar en Storm-topologi efter hello angivna timeout.

3. Den här sidan väljer du en post från hello **Spouts** eller **Bolts** avsnitt. Information om hello valda komponenten visas.

    ![Storm-instrumentpanelen med information om valda komponenter.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)

    Den här sidan visar hello följande information:

    * **Kanal/bult stats** – grundläggande information om hello komponentens prestanda, ordnad i tidsfönster.

        > [!NOTE]
        > Att välja en viss tid fönstret ändringar hello tidsfönstret för information som visas i andra avsnitt i hello-sidan.

    * **Input stats** (endast bultar) – Information om komponenter som producerar de data som används av hello bulten.

    * **Output stats** (Statistik för utdata) – information om data som sänds av bulten.

    * **Executors** (Utförare) – information om komponentens instanser.

    * **Errors** (Fel) – fel som komponenten ger upphov till.

4. När du visar hello information om en kanal eller en bult väljer du en post från hello **Port** kolumn i hello **Executors** avsnittet tooview information för en specifik instans av hello-komponent.

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    I det här exemplet hello word **sju** uppstod 1493957 gånger. Det här antalet är hur många gånger hello ordet har påträffats sedan topologin startades.

## <a name="stop-hello-topology"></a>Stoppa hello-topologi

Returnera toohello **Topology summary** för hello ordräkningstopologin och välj sedan hello **Kill** knappen från hello **Topology actions** avsnitt. När du uppmanas, anger du 10 för hello sekunder toowait innan du stoppar hello-topologi. Efter hello tidsgränsen hello topologin inte längre när du besöker hello **Storm-Användargränssnittet** avsnittet hello instrumentpanelen.

## <a name="delete-hello-cluster"></a>Ta bort hello kluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a id="next"></a>Nästa steg

I den här Apache Storm-kurs berättat hello grunderna i att arbeta med Storm på HDInsight. Lär dig sedan hur för[utveckla Java-baserade topologier med Maven](hdinsight-storm-develop-java-topology.md).

Om du redan är bekant med att utveckla Java-baserad topologier och vill toodeploy en befintlig topologi tooHDInsight finns [distribuera och hantera Apache Storm-topologier på HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).

Om du är en .NET-utvecklare kan du skapa C#- eller C#/Java-hybridtopologier med Visual Studio. Mer information finns i [Utveckla C#-topologier för Apache Storm på HDInsight med Hadoop-verktyg för Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Till exempel topologier som kan användas med Storm på HDInsight, se följande exempel hello:

* [Exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[preview-portal]: https://portal.azure.com/
