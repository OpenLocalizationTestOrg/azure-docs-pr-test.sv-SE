---
title: "Använd Azure Toolkit för IntelliJ med Hortonworks Sandbox | Microsoft Docs"
description: "Lär dig hur du använder HDInsight-verktyg i Azure Toolkit för IntelliJ med Hortonworks Sandbox."
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
ms.openlocfilehash: c49f185db5a035f70a711bf309b973182d94a2b0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a>Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox

Lär dig hur du använder HDInsight-verktyg för IntelliJ att utveckla program för Apache Scala och testa programmen på [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) körs på din arbetsstation. 

[IntelliJ IDEA](https://www.jetbrains.com/idea/) är en Java-integrerad utvecklingsmiljö (IDE) för att utveckla programvara. När du har utvecklats och testa dina program på Hortonworks Sandbox, kan du flytta dem till [Azure HDInsight](hdinsight-hadoop-introduction.md).

## <a name="prerequisites"></a>Krav

Innan du börjar den här vägledningen måste du ha:

- Hortonworks Data Platform (HDP) 2.4 på Hortonworks Sandbox körs på din lokala miljö. Om du vill konfigurera HDP finns [komma igång med Hadoop-ekosystemet med en Hadoop sandbox på en virtuell dator](hdinsight-hadoop-emulator-get-started.md). 
    >[!NOTE]
    >HDInsight-verktyg för IntelliJ har testats med HDP 2.4. För att få HDP 2.4, expandera **Hortonworks Sandbox Arkiv** på den [Hortonworks Sandbox hämtar plats](http://hortonworks.com/downloads/#sandbox).

- [Java Developer Kit (JDK) version 1,8 eller senare](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). JDK krävs av Azure-verktyget för IntelliJ.

- [IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) med den [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) plugin-program och [Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij.md) plugin-programmet. HDInsight-verktyg för IntelliJ är tillgänglig som en del av Azure-verktygen för IntelliJ. 

  Om du vill installera plugin-program gör du följande:

  1. Öppna IntelliJ IDEA.
  2. På den **Välkommen** väljer **konfigurera**, och välj sedan **plugin-program**.
  3. Välj **installera JetBrains plugin** i det nedre vänstra hörnet.
  4. Använda sökfunktionen för att söka efter **Scala**, och välj sedan **installera**.
  5. Välj **starta om IntelliJ IDEA** att slutföra installationen.
  6. Upprepa steg 4 och 5 för att installera den **Azure Toolkit för IntelliJ**. Mer information finns i [installerar Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="create-a-spark-scala-application"></a>Skapa ett Spark Scala-program

I det här avsnittet Skapa en Scala exempelprojektet med IntelliJ IDEA. I nästa avsnitt länka du IntelliJ IDEA till Hortonworks Sandbox (emulator) innan du skickar projektet.

1. Öppna IntelliJ IDEA från din arbetsstation. I den **nytt projekt** dialogrutan Gör följande:

   a. Välj **HDInsight** > **Spark i HDInsight (Scala)**.

   b. I den **Build verktyget** väljer du något av följande enligt dina behov:

    * **Maven**, Scala skapa projekt guiden support
    * **Segregerade Barlasttankar**, för att hantera beroenden och skapa för Scala-projekt

   ![Dialogrutan Nytt projekt](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. Välj **nästa**.

3. I nästa **nytt projekt** dialogrutan Gör följande:

    ![Skapa IntelliJ Scala projektegenskaperna](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    a. I den **projektnamn** ange ett projektnamn.

    b. I den **projektet plats** ange en projektplats.

    c. Bredvid den **projekt SDK** listrutan, Välj **ny**väljer **JDK**, och ange sedan mappen Java JDK 1,7 eller senare. Välj **Java 1.8** för 2.x Spark-kluster eller välj **Java 1.7** för 1.x Spark-klustret. Standardsökvägen är C:\Program Files\Java\jdk1.8.x_xxx.

    d. I den **Spark version** listrutan Scala guide för att skapa projektet integreras rätt version för Spark SDK och Scala SDK. Om den Spark-kluster av versionen är äldre än 2.0 väljer **Väck 1.x**. Annars väljer **Spark2.x**. Det här exemplet använder Spark 1.6.2 (Scala 2.10.5). Kontrollera att du använder databasen markeras Scala 2.10.x. Använd inte databasen markeras Scala 2.11.x.

4. Välj **Slutför**.

5. Om den **projekt** vyn inte är redan öppen genom att trycka på **Alt + 1** att öppna den.

6. I **Projektutforskaren**, expandera projektet och välj sedan **src**.

7. Högerklicka på **src**, peka på **ny**, och välj sedan **Scala klassen**.

8. I den **namn** ange ett namn i den **typ** väljer **objekt**, och välj sedan **OK**.

    ![Fönstret Skapa nya Scala-klass](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. Klistra in följande kod i filen .scala:

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

10. På den **skapa** väljer du **bygga projektet**. Kontrollera att kompileringen har slutförts.


## <a name="link-to-the-hortonworks-sandbox"></a>Länk till sandlådan Hortonworks

Innan du kan länka till en Hortonworks Sandbox (emulator), måste du ha ett befintligt IntelliJ program.

Om du vill länka till en emulator, gör du följande:

1. Öppna projektet i IntelliJ.

2. På den **visa** väljer du **verktyg Windows**, och välj sedan **Azure Explorer**.

3. Expandera **Azure**, högerklicka på **HDInsight**, och välj sedan **länka en Emulator**.

4. I den **länken en ny emulatorn** och ange lösenord för att du har konfigurerat för rotkontot i sandlådan Hortonworks ange värden som är samma som i följande skärmbild och välj sedan **OK**. 

   ![Fönstret ”länka ett nytt emulatorn”](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. Om du vill konfigurera emulatorn, Välj **Ja**.

När emulatorn är ansluten med emulatorn (Hortonworks Sandbox) i HDInsight-nod.

## <a name="submit-the-spark-scala-application-to-the-hortonworks-sandbox"></a>Skicka Spark Scala-programmet till Hortonworks Sandbox

När du har länkat IntelliJ IDEA till emulatorn, kan du skicka projektet.

För att skicka ett projekt till en emulator, gör du följande:

1. I **Projektutforskaren**, högerklicka på projektet och välj sedan **skicka Spark-program till HDInsight**.

2. Gör följande:

    a. I den **Spark-kluster (endast Linux)** listrutan väljer du din lokala Hortonworks Sandbox.

    b. I den **Main klassnamn** rutan, Välj eller ange det huvudsakliga klassnamnet. Den här kursen är namn **GroupByTest**.

3. Välj **skicka**. Jobbet skicka loggarna visas i fönstret Spark skicka verktyget.

## <a name="next-steps"></a>Nästa steg

- Om du vill lära dig skapa Spark-program för HDInsight med hjälp av HDInsight-verktyg för IntelliJ, gå till [använda HDInsight Tools i Azure Toolkit för IntelliJ till att skapa Spark-program för HDInsight Spark Linux-kluster](hdinsight-apache-spark-intellij-tool-plugin.md).

- Om du vill se en video av HDInsight-verktyg för IntelliJ, gå till [införa HDInsight-verktyg för IntelliJ för Spark-utveckling](https://www.youtube.com/watch?v=YTZzYVgut6c).

- Om du vill lära dig att felsöka Spark-program med hjälp av verktyget via fjärranslutning på HDInsight via SSH, gå till [felsöka Spark-program på ett HDInsight-kluster med Azure Toolkit för IntelliJ via SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).

- Om du vill lära dig att felsöka Spark-program med hjälp av verktyget via fjärranslutning på HDInsight via VPN, gå till [använda HDInsight Tools i Azure Toolkit för IntelliJ till att felsöka Spark-program på HDInsight Spark Linux-kluster](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).

- Mer information om hur du använder HDInsight-verktyg för Eclipse för att skapa ett Spark-program, gå till [använder HDInsight-verktyg i Azure Toolkit för Eclipse att skapa Spark-program](hdinsight-apache-spark-eclipse-tool-plugin.md).

- Om du vill se en video av HDInsight-verktyg för Eclipse, gå till [HDInsight verktyg för Eclipse att skapa Spark-program](https://mix.office.com/watch/1rau2mopb6fha).
