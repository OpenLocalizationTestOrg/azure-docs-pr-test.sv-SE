---
title: "aaaCreate Scala app toorun på Spark - kluster i Azure HDInsight | Microsoft Docs"
description: "Skapa ett Spark-program som skrivits i Scala med Apache Maven medan hello skapar system och en befintlig Maven archetype för Scala som tillhandahålls av IntelliJ IDEA."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b2467a40-a340-4b80-bb00-f2c3339db57b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: nitinme
ms.openlocfilehash: b25291b60921021486f55d78b4832a070a54d163
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-scala-maven-application-toorun-on-apache-spark-cluster-on-hdinsight"></a>Skapa en Scala Maven programmet toorun på Apache Spark-kluster i HDInsight

Lär dig hur toocreate ett Spark-program som skrivits i Scala med IntelliJ IDEA Maven. hello artikeln använder Apache Maven hello bygga system och börjar med en befintlig Maven archetype för Scala som tillhandahålls av IntelliJ IDEA.  Skapa en koppling i Scala i IntelliJ IDEA omfattar hello följande steg:

* Använd Maven som hello build-system.
* Uppdatera projektet Object Model (POM) tooresolve Spark modulen filberoenden.
* Skriva ditt program i Scala.
* Generera en jar-fil som kan vara skickade tooHDInsight Spark-kluster.
* Kör hello på Spark-kluster med Livy.

> [!NOTE]
> HDInsight tillhandahåller även en IntelliJ IDEA plugin verktyget tooease hello process för att skapa och skicka program tooan HDInsight Spark-kluster på Linux. Mer information finns i [använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark-program](hdinsight-apache-spark-intellij-tool-plugin.md).
> 
> 

## <a name="prerequisites"></a>Krav

* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java Development kit. Du kan installera den från [här](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* En Java IDE. Den här artikeln använder IntelliJ IDEA 15.0.1. Du kan installera den från [här](https://www.jetbrains.com/idea/download/).

## <a name="install-scala-plugin-for-intellij-idea"></a>Installera Scala-plugin för IntelliJ IDEA
Om installationen för IntelliJ IDEA tillfrågades inte inte för att aktivera Scala-plugin-programmet, starta IntelliJ IDEA och gå igenom följande steg tooinstall hello plugin-programmet hello:

1. Starta IntelliJ IDEA och klicka på välkomstskärmen **konfigurera** och klicka sedan på **plugin-program**.
   
    ![Aktivera scala plugin-program](./media/hdinsight-apache-spark-create-standalone-application/enable-scala-plugin.png)
2. I nästa hello-skärmen klickar du på **installera JetBrains plugin** från hello nedre vänstra hörnet. I hello **Bläddra JetBrains plugin-program** dialogrutan som öppnas, söka efter Scala och klicka sedan på **installera**.
   
    ![Installera scala plugin-programmet](./media/hdinsight-apache-spark-create-standalone-application/install-scala-plugin.png)
3. När hello plugin-programmet installeras, klickar du på hello **starta om IntelliJ IDEA knappen** toorestart hello IDE.

## <a name="create-a-standalone-scala-project"></a>Skapa ett fristående Scala-projekt
1. Öppna IntelliJ IDEA och skapa ett nytt projekt. Kontrollera hello följande alternativ och klickar sedan på i hello dialogrutan Nytt projekt, **nästa**.
   
    ![Skapa Maven-projekt](./media/hdinsight-apache-spark-create-standalone-application/create-maven-project.png)
   
   * Välj **Maven** som hello projekttypen.
   * Ange en **projektet SDK**. Klicka på nytt och navigera toohello Java-installationskatalogen, vanligtvis `C:\Program Files\Java\jdk1.8.0_66`.
   * Välj hello **skapa från archetype** alternativet.
   * Välj hello listan över archetypes **org.scala tools.archetypes:scala-archetype-enkel**. Detta skapar hello rätt katalogstrukturen och hämta hello krävs beroenden toowrite Scala standardprogrammet.
2. Ange relevanta värden för **GroupId**, **artefakt-ID**, och **Version**. Klicka på **Nästa**.
3. Hello nästa i dialogrutan anger du Maven arbetskatalog och andra inställningar, accepterar hello standardinställningarna och klickar på **nästa**.
4. Hello senaste i dialogrutan Ange namn och plats och klicka sedan på **Slutför**.
5. Ta bort hello **MySpec.Scala** filen på **src\test\scala\com\microsoft\spark\example**. Du behöver inte detta för hello program.
6. Om det behövs kan du byta namn på hello käll- och filer som standard. Från hello till vänster i hello IntelliJ IDEA och navigera för**src\main\scala\com.microsoft.spark.example**. Högerklicka på **App.scala**, klickar du på **Refactor**, klicka på Byt namn på filen hello i dialogrutan Ange hello nytt namn för hello program och klicka sedan på **Refactor**.
   
    ![Byta namn på filer](./media/hdinsight-apache-spark-create-standalone-application/rename-scala-files.png)  
7. I hello efterföljande steg uppdaterar du hello pom.xml toodefine hello beroenden för hello Spark Scala-programmet. För dessa beroenden toobe hämtas och lösas automatiskt, måste du konfigurera Maven därefter.
   
    ![Konfigurera Maven vid automatisk hämtning](./media/hdinsight-apache-spark-create-standalone-application/configure-maven.png)
   
   1. Från hello **filen** -menyn klickar du på **inställningar**.
   2. I hello **inställningar** dialogrutan går för**bygga, körning och distribution** > **Build Tools** > **Maven**  >  **Importera**.
   3. Välj alternativet för hello för**importera Maven-projekt automatiskt**.
   4. Klicka på **tillämpa**, och klicka sedan på **OK**.
8. Uppdatera hello Scala källa filen tooinclude programkoden. Öppna och ersätta befintliga exempelkod med följande kod hello hello och spara hello ändringar. Den här koden läser hello data från hello HVAC.csv (finns i alla HDInsight Spark-kluster), hämtar hello rader som bara innehåller en siffra i hello sjätte kolumnen och skriver hello utdata för**/HVACOut** under hello standardlagring behållare för hello klustret.
   
        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO toowasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find hello rows which have only one digit in hello 7th column in hello CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
9. Uppdatera hello pom.xml.
   
   1. Inom `<project>\<properties>` lägga till hello följande:
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   2. Inom `<project>\<dependencies>` lägga till hello följande:
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      Spara ändringarna toopom.xml.
10. Skapa hello .jar-fil. IntelliJ IDEA gör det möjligt för skapa JAR som ett resultat av ett projekt. Utför följande steg hello.
    
    1. Från hello **filen** -menyn klickar du på **projektstruktur**.
    2. I hello **projektstruktur** dialogrutan klickar du på **artefakter** och klicka sedan på hello plus symbolen. Hello popup-menyn klickar du på **JAR**, och klicka sedan på **från moduler med beroenden**.
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-1.png)
    3. I hello **skapa JAR från moduler** dialogrutan klickar du på ellipsknappen hello (![tre punkter](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) mot hello **Main klassen**.
    4. I hello **Välj Main klassen** dialogrutan, Välj hello-klass som visas som standard och klicka sedan på **OK**.
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-2.png)
    5. I hello **skapa JAR från moduler** dialogrutan Kontrollera hello alternativet för**extrahera toohello mål JAR** är markerad och klicka sedan på **OK**. Detta skapar en enda JAR med alla beroenden.
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-3.png)
    6. hello utdata layout fliken listar alla hello burkar som ingår som en del av hello Maven-projekt. Du kan välja och ta bort hello sådana där hello Scala program har ingen direkt beroende. För hello program skapar vi här, kan du ta bort alla utom hello senast (**SparkSimpleApp kompilera utdata**). Välj hello burkar toodelete och klicka sedan på hello **ta bort** ikon.
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/delete-output-jars.png)
       
        Kontrollera att **bygger på Kontrollera** är markerad, vilket säkerställer att hello jar skapas varje gång hello projektet skapats eller uppdaterats. Klicka på **gäller** och sedan **OK**.
    7. Hello menyraden klickar du på **skapa**, och klicka sedan på **Se projektet**. Du kan också klicka på **skapa artefakter** toocreate hello jar. hello utdata jar skapas under **\out\artifacts**.
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/output.png)

## <a name="run-hello-application-on-hello-spark-cluster"></a>Kör hello på hello Spark-kluster
toorun hello programmet på hello klustret, måste du göra hello följande:

* **Kopiera hello programmet jar toohello Azure storage blob** som associerats med hello kluster. Du kan använda [ **AzCopy**](../storage/common/storage-use-azcopy.md), ett kommando rad toodo-verktyget så. Det finns en mängd andra klienter samt att du kan använda tooupload data. Du kan hitta mer om dem i [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md).
* **Använd Livius toosubmit ett program-jobb via fjärranslutning** toohello Spark-kluster. Spark-kluster i HDInsight innehåller Livius som exponerar REST-slutpunkter tooremotely skicka Spark jobb. Mer information finns i [skicka Spark jobb via fjärranslutning med Livy med Spark-kluster i HDInsight](hdinsight-apache-spark-livy-rest-interface.md).

## <a name="next-step"></a>Nästa steg

I denna artikel du lärt dig hur toocreate ett Spark scala-program. Avancerade toohello nästa artikel toolearn hur toorun det här programmet på ett HDInsight Spark-kluster med Livy.

> [!div class="nextstepaction"]
>[Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)

