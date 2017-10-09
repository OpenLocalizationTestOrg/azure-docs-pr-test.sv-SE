---
title: "aaaInstall och använda Giraph på HDInsight (Hadoop) - Azure | Microsoft Docs"
description: "Lär dig hur tooinstall Giraph på Linux-baserade HDInsight-kluster med skriptåtgärder. Skriptåtgärder kan du toocustomize hello klustret när skapas genom att ändra klusterkonfigurationen eller installera tjänster och verktyg."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9fcac906-8f06-4002-9fe8-473e42f8fd0f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 0f195b65cebf5e24d1808ef33b95b4d362555521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-tooprocess-large-scale-graphs"></a>Installera Giraph på HDInsight Hadoop-kluster och Använd Giraph tooprocess storskaliga diagram

Lär dig hur tooinstall Apache Giraph på ett HDInsight-kluster. hello skript åtgärd funktion i HDInsight kan du toocustomize klustret genom att köra ett bash-skript. Skript kan vara används toocustomize kluster under och efter klustret skapas.

> [!IMPORTANT]
> hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="whatis"></a>Vad är Giraph

[Apache Giraph](http://giraph.apache.org/) kan du tooperform diagrammet bearbetning med hjälp av Hadoop och kan användas med Azure HDInsight. Diagram modell förhållanden mellan objekt. Hello anslutningar mellan routrar i ett större nätverk som till exempel hello Internet eller relationer mellan personer på sociala nätverk. Diagrammet bearbetning kan du tooreason om hello relationer mellan objekt i ett diagram som:

* Identifiera potentiella vänner baserat på din aktuella relationer.

* Identifiera hello kortaste vägen mellan två datorer i ett nätverk.

* Beräkning av hello sidan rangordningen för webbsidor.

> [!WARNING]
> Komponenter som ingår i hello HDInsight-kluster stöds fullt ut - Microsoft-supporten hjälper tooisolate och lösa problem relaterade toothese komponenter.
>
> Anpassade komponenter, till exempel Giraph, ta emot kommersiellt rimliga stöd toohelp du toofurther hello felsökning. Microsoft-supporten kanske kan tooresolving hello problemet. Om inte, måste du kontakta öppen källkod communities där djup expertis för att teknik finns. Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/).


## <a name="what-hello-script-does"></a>Vilka hello skriptet gör

Det här skriptet utför hello följande åtgärder:

* Installerar Giraph för`/usr/hdp/current/giraph`

* Kopior hello `giraph-examples.jar` filen toodefault storage (WASB) för klustret:`/example/jars/giraph-examples.jar`

## <a name="install"></a>Installera Giraph med hjälp av skriptåtgärder

Ett exempel skriptet tooinstall Giraph på ett HDInsight-kluster finns på följande plats hello:

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Det här avsnittet innehåller instruktioner om hur toouse hello exempelskript när du skapar hello kluster med hjälp av hello Azure-portalen.

> [!NOTE]
> En skriptåtgärd kan tillämpas med hjälp av hello följande metoder:
> * Azure PowerShell
> * hello Azure CLI
> * Hej HDInsight .NET SDK
> * Azure Resource Manager-mallar
> 
> Du kan också använda skriptet åtgärder tooalready kluster som körs. Mer information finns i [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).

1. Skapa ett kluster med hjälp av hello stegen i [skapa Linux-baserade HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md), men inte slutföra skapandet.

2. På hello **valfri konfiguration** bladet väljer **skriptåtgärder**, och ange hello följande information:

   * **NAMNET**: Ange ett eget namn för hello skriptåtgärder.

   * **SKRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

   * **HEAD**: Kontrollera posten

   * **WORKER**: lämna det här alternativet är avmarkerat

   * **ZOOKEEPER**: lämna det här alternativet är avmarkerat

   * **PARAMETRARNA**: lämna fältet tomt

3. Längst ned hello hello **skriptåtgärder**, använda hello **Välj** toosave hello konfiguration. Använd slutligen hello **Välj** knappen längst ned hello hello **valfri konfiguration** bladet toosave hello ytterligare konfigurationsinformation.

4. Fortsätta skapa hello klustret enligt beskrivningen i [skapa Linux-baserade HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="usegiraph"></a>Hur använder Giraph i HDInsight?

Använda hello följande steg toorun hello SimpleShortestPathsComputation exempel medföljer Giraph när hello klustret har skapats. Det här exemplet används hello basic [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) implementering för att hitta hello shortest path mellan objekt i ett diagram.

1. Anslut toohello HDInsight-kluster med SSH:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Använd hello följande kommando toocreate en fil med namnet **tiny_graph.txt**:

    ```bash
    nano tiny_graph.txt
    ```

    Använd hello följande text som hello innehållet i den här filen:

    ```text
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```

    Informationen beskriver en relation mellan objekt i en riktat diagram hello formatet `[source_id, source_value,[[dest_id], [edge_value],...]]`. Varje rad representerar en relation mellan en `source_id` objekt och en eller flera `dest_id` objekt. Hej `edge_value` kan betraktas som hello styrkan eller avståndet mellan hello anslutning mellan `source_id` och `dest\_id`.

    Dras ut, och använder hello värde (eller vikt) som hello avståndet mellan objekt, hello data kan se ut hello följande diagram:

    ![tiny_graph.txt ritas som cirklar med rader med olika avståndet mellan](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. toosave hello-fil, Använd **Ctrl + X**, sedan **Y**, och slutligen **RETUR** tooaccept hello-filnamn.

4. Använd hello följande toostore hello data till primära lagringsplatsen för HDInsight-kluster:

    ```bash
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. Kör hello SimpleShortestPathsComputation exempel på användning av hello följande kommando:

    ```bash
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    hello-parametrar som används med det här kommandot beskrivs i följande tabell hello:

   | Parameter | Vad verktyget gör |
   | --- | --- |
   | `jar` |hello jar-fil som innehåller hello exempel. |
   | `org.apache.giraph.GiraphRunner` |hello klassen används toostart hello exempel. |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |hello-exempel som används. I det här exemplet beräknar hello kortaste sökvägen mellan ID 1 och alla andra ID: N i hello graph. |
   | `-ca mapred.job.tracker` |Hej headnode för hello-kluster. |
   | `-vif` |hello Indataformatet toouse för hello indata. |
   | `-vip` |hello indatafilen. |
   | `-vof` |hello utdataformat. I det här exemplet ID och värdet som oformaterad text. |
   | `-op` |hello platsen. |
   | `-w 2` |hello antal arbetare toouse. I det här exemplet 2. |

    Mer information om dessa och andra parametrar som används med Giraph exempel finns hello [Giraph quickstart](http://giraph.apache.org/quick_start.html).

6. När hello jobbet har slutförts, hello resultat lagras i hello **/example/out/shotestpaths** directory. hello utdata-filnamn börjar med **del-m -** och avslutas med ett tal som anger hello först andra, etc.-filen. Använd följande kommandoutdata tooview hello hello:

    ```bash
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    hello utdata ska se ut ungefär toohello följande text:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    Hej SimpleShortestPathComputation exempel är hårdkodad toostart med objekt-ID 1 och hitta hello shortest path tooother objekt. hello-utdata är i hello-format för `destination_id` och `distance`. Hej `distance` är hello värde (eller vikt) av hello kanter obligatoriska mellan objekt-ID 1 och hello mål-ID.

    Visualisera data, kan du kontrollera hello resultat av reser hello kortaste sökvägar mellan ID 1 och alla andra objekt. hello shortest path mellan ID 1 och 4-ID är 5. Det här värdet är hello totala avståndet mellan <span style="color:orange">ID 1 och 3</span>, och sedan <span style="color:red">ID 3 och 4</span>.

    ![Ritning av objekt som cirklar med kortast sökvägar mellan](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a>Nästa steg

* [Installera och använda Hue på HDInsight-kluster](hdinsight-hadoop-hue-linux.md).

* [Installera Solr på HDInsight-kluster](hdinsight-hadoop-solr-install-linux.md).
