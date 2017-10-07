---
title: "aaaUse Azure Toolkit för IntelliJ med Hortonworks Sandbox | Microsoft Docs"
description: "Lär dig hur toouse HDInsight Tools i Azure Toolkit för IntelliJ med Hortonworks Sandbox."
keywords: "hadoop-verktygen hive-fråga, intellij, hortonworks sandbox, azure toolkit för intellij"
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: b587cc9b-a41a-49ac-998f-b54d6c0bdfe0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 2bf97068a9cec99fcc7b4ff9469b91d8cbe2a8d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a>Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox

Lär dig hur toouse HDInsight-verktyg för IntelliJ toodevelop Apache Scala program och testa hello program på [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) körs på din arbetsstation. 

[IntelliJ IDEA](https://www.jetbrains.com/idea/) är en Java-integrerad utvecklingsmiljö (IDE) för att utveckla programvara. När du har utvecklats och testa dina program på Hortonworks Sandbox kan du flytta dem för[Azure HDInsight](hdinsight-hadoop-introduction.md).

## <a name="prerequisites"></a>Krav

Innan du börjar den här vägledningen måste du ha:

- Hortonworks Data Platform (HDP) 2.4 på Hortonworks Sandbox körs på din lokala miljö. tooconfigure HDP, se [komma igång i hello Hadoop-ekosystemet med en Hadoop sandbox på en virtuell dator](hdinsight-hadoop-emulator-get-started.md). 
    >[!NOTE]
    >HDInsight-verktyg för IntelliJ har testats med HDP 2.4. Expandera tooget HDP 2.4 **Hortonworks Sandbox Arkiv** på hello [Hortonworks Sandbox hämtar plats](http://hortonworks.com/downloads/#sandbox).

- [Java Developer Kit (JDK) version 1,8 eller senare](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). JDK krävs av hello Azure Toolkit för IntelliJ.

- [IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) med hello [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) plugin-program och hello [Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij.md) plugin-programmet. HDInsight-verktyg för IntelliJ är tillgänglig som en del av hello Azure Toolkit för IntelliJ. 

  tooinstall hello plugin-program, hello följande:

  1. Öppna IntelliJ IDEA.
  2. På hello **Välkommen** väljer **konfigurera**, och välj sedan **plugin-program**.
  3. Välj **installera JetBrains plugin** i hello nedre vänstra hörnet.
  4. Använd hello Sök funktionen toosearch för **Scala**, och välj sedan **installera**.
  5. Välj **starta om IntelliJ IDEA** toocomplete hello installation.
  6. Upprepa steg 4 och 5 tooinstall hello **Azure Toolkit för IntelliJ**. Mer information finns i [installera hello Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="create-a-spark-scala-application"></a>Skapa ett Spark Scala-program

I det här avsnittet Skapa en Scala exempelprojektet med IntelliJ IDEA. I nästa avsnitt om hello länka IntelliJ IDEA toohello Hortonworks Sandbox (emulator) innan du skickar hello-projekt.

1. Öppna IntelliJ IDEA från din arbetsstation. I hello **nytt projekt** dialogrutan rutan, hello följande:

   a. Välj **HDInsight** > **Spark i HDInsight (Scala)**.

   b. I hello **Build verktyget** väljer du antingen hello följande, enligt tooyour behov:

    * **Maven**, Scala skapa projekt guiden support
    * **Segregerade Barlasttankar**, för att hantera hello beroenden och bygga för hello Scala-projekt

   ![dialogrutan Nytt projekt för hello](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. Välj **nästa**.

3. I hello nästa **nytt projekt** dialogrutan rutan, hello följande:

    ![Skapa IntelliJ Scala projektegenskaperna](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    a. I hello **projektnamn** ange ett projektnamn.

    b. I hello **projektet plats** ange en projektplats.

    c. Nästa toohello **projekt SDK** listrutan, Välj **ny**väljer **JDK**, och sedan ange hello mapp för Java JDK 1,7 eller senare. Välj **Java 1.8** för 2.x hello Spark-kluster eller välj **Java 1.7** för 1.x hello Spark-kluster. hello standardsökvägen är C:\Program Files\Java\jdk1.8.x_xxx.

    d. I hello **Spark version** listrutan Scala guide för att skapa projektet integreras hello rätt version för Spark SDK och Scala SDK. Om hello Spark-kluster av version är äldre än 2.0 väljer **Väck 1.x**. Annars väljer **Spark2.x**. Det här exemplet använder Spark 1.6.2 (Scala 2.10.5). Kontrollera att du använder hello databasen markeras Scala 2.10.x. Använd inte hello databasen markeras Scala 2.11.x.

4. Välj **Slutför**.

5. Om hello **projekt** vyn inte är redan öppen genom att trycka på **Alt + 1** tooopen den.

6. I **Projektutforskaren**, expandera hello projektet och välj sedan **src**.

7. Högerklicka på **src**, peka för**ny**, och välj sedan **Scala klassen**.

8. I hello **namn** ange ett namn i hello **typ** väljer **objekt**, och välj sedan **OK**.

    ![hello Skapa ny Scala klass fönster](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. Klistra in följande kod hello i hello .scala filen:

        import java.util.Random
        import org.apache.spark.{SparkConf, SparkContext}
        import org.apache.spark.SparkContext._

        /**
        * Usage: GroupByTest [numMappers] [numKVPairs] [valSize] [numReducers]
        */
        object GroupByTest {
            def main(args: Array[String]) {
                val sparkConf = new SparkConf().setAppName("GroupBy Test")
                var numMappers = 3
                var numKVPairs = 10
                var valSize = 10
                var numReducers = 2

                val sc = new SparkContext(sparkConf)

                val pairs1 = sc.parallelize(0 until numMappers, numMappers).flatMap { p =>
                val ranGen = new Random
                var arr1 = new Array[(Int, Array[Byte])](numKVPairs)
                for (i <- 0 until numKVPairs) {
                    val byteArr = new Array[Byte](valSize)
                    ranGen.nextBytes(byteArr)
                    arr1(i) = (ranGen.nextInt(Int.MaxValue), byteArr)
                }
                arr1
                }.cache
                // Enforce that everything has been calculated and in cache
                pairs1.count

                println(pairs1.groupByKey(numReducers).count)
            }
        }

10. På hello **skapa** väljer du **bygga projektet**. Kontrollera att hello kompilering har slutförts.


## <a name="link-toohello-hortonworks-sandbox"></a>Länka toohello Hortonworks Sandbox

Innan du kan länka tooa Hortonworks Sandbox (emulator), måste du ha ett befintligt IntelliJ program.

toolink tooan emulatorn hello följande:

1. Öppna hello projekt i IntelliJ.

2. På hello **visa** väljer du **verktyg Windows**, och välj sedan **Azure Explorer**.

3. Expandera **Azure**, högerklicka på **HDInsight**, och välj sedan **länka en Emulator**.

4. I hello **länken en ny emulatorn** fönstret Ange hello lösenord att du har konfigurerats för hello rotkontot av hello Hortonworks Sandbox ange värden liknande toothose i följande skärmbild hello och välj sedan **OK** . 

   ![Hej ”länka ett nytt emulatorn” fönster](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. tooconfigure hello-emulatorn väljer **Ja**.

När hello-emulatorn är ansluten med hello-emulatorn (Hortonworks Sandbox) i hello HDInsight nod.

## <a name="submit-hello-spark-scala-application-toohello-hortonworks-sandbox"></a>Skicka hello Spark Scala programmet toohello Hortonworks Sandbox

När du har länkat IntelliJ IDEA toohello emulator, kan du skicka projektet.

toosubmit en tooan emulator projekt hello följande:

1. I **Projektutforskaren**, högerklicka på hello-projektet och välj sedan **skicka Spark programmet tooHDInsight**.

2. Hej du följande:

    a. I hello **Spark-kluster (endast Linux)** listrutan väljer du din lokala Hortonworks Sandbox.

    b. I hello **Main klassnamn** rutan, Välj eller ange hello huvudsakliga klassnamn. Den här kursen är hello namn **GroupByTest**.

3. Välj **skicka**. hello jobbet skicka loggarna visas i hello Spark skicka verktygsfönster.

## <a name="next-steps"></a>Nästa steg

- hur toocreate Spark-program för HDInsight med hjälp av HDInsight-verktyg för IntelliJ, gå för toolearn[använda HDInsight Tools i Azure Toolkit för IntelliJ toocreate Spark-program för HDInsight Spark Linux-kluster](hdinsight-apache-spark-intellij-tool-plugin.md).

- toowatch en video med HDInsight-verktyg för IntelliJ, gå för[införa HDInsight-verktyg för IntelliJ för Spark-utveckling](https://www.youtube.com/watch?v=YTZzYVgut6c).

- toolearn hur toodebug Spark-program med hjälp av hello toolkit via fjärranslutning på HDInsight via SSH för Gå[felsöka Spark-program på ett HDInsight-kluster med Azure Toolkit för IntelliJ via SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).

- toolearn hur toodebug Spark-program med hjälp av hello toolkit via fjärranslutning på HDInsight via VPN, gå för[använda HDInsight Tools i Azure Toolkit för IntelliJ toodebug Spark-program på HDInsight Spark Linux-kluster](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).

- hur toouse HDInsight-verktyg för Eclipse toocreate ett Spark-program går för toolearn[använda HDInsight Tools i Azure Toolkit för Eclipse toocreate Spark-program](hdinsight-apache-spark-eclipse-tool-plugin.md).

- toowatch en video med HDInsight-verktyg för Eclipse gå för[HDInsight verktyg för Eclipse toocreate Spark-program](https://mix.office.com/watch/1rau2mopb6fha).
