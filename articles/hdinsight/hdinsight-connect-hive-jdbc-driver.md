---
title: aaaQuery Hive via hello JDBC driver - Azure HDInsight | Microsoft Docs
description: "Använd hello JDBC-drivrutinen från en Java-program toosubmit Hive-frågor tooHadoop på HDInsight. Ansluta och programmässigt hello SQuirrel SQL-klient."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 928f8d2a-684d-48cb-894c-11c59a5599ae
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: 93178da3b8d497faa4c788e91dba89c4e45d3fff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="query-hive-through-hello-jdbc-driver-in-hdinsight"></a>Fråga Hive via hello JDBC-drivrutinen i HDInsight

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Lär dig hur toouse hello JDBC-drivrutinen från en Java-program toosubmit Hive frågar tooHadoop i Azure HDInsight. hello informationen i det här dokumentet visar hur tooconnect och programmässigt från hello SQuirrel SQL-klienten.

Mer information om hello Hive JDBC-gränssnittet finns [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).

## <a name="prerequisites"></a>Krav

* En Hadoop på HDInsight-kluster. Linux-baserade eller Windows-baserade kluster fungerar.

  > [!IMPORTANT]
  > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight 3.3 pensionering](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [SQuirreL SQL](http://squirrel-sql.sourceforge.net/). SQuirreL är en JDBC-klientprogram.

* Hej [Java Developer Kit (JDK) version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) eller högre.

* [Apache Maven](https://maven.apache.org). Maven är ett projekt bygg system för Java-projekt som används av hello projekt som är associerat med den här artikeln.

## <a name="jdbc-connection-string"></a>JDBC-anslutningssträng

JDBC-anslutningar tooan HDInsight-kluster i Azure görs över 443 och hello trafik skyddas med SSL. hello offentliga gateway hello kluster hamnar bakom omdirigerar hello trafik toohello port HiveServer2 faktiskt lyssnar på. hello följande är ett exempel anslutningssträng:

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

Ersätt `CLUSTERNAME` med hello namnet på ditt HDInsight-kluster.

## <a name="authentication"></a>Autentisering

När anslutningen har upprättats hello måste du använda hello HDInsight-kluster admin namn och lösenord tooauthenticate toohello klustret gateway. När du ansluter från JDBC-klienter, till exempel SQuirreL SQL måste du ange hello admin namn och lösenord i klientinställningarna.

Från ett Java-program, måste du använda hello namn och lösenord när en anslutning upprättas. Till exempel öppnas hello följande Java-kod en ny anslutning med hello anslutningssträngen, admin namn och lösenord:

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a>Ansluta med SQuirreL SQL-klienten

SQuirreL SQL är en JDBC-klient som kan använda tooremotely kör Hive-frågor med ditt HDInsight-kluster. hello förutsätter följande steg att du redan har installerat SQuirreL SQL.

1. Kopiera hello Hive JDBC-drivrutiner från ditt HDInsight-kluster.

    * För **Linux-baserat HDInsight**, Använd hello följande steg toodownload hello krävs jar-filer.

        1. Skapa en katalog som innehåller hello-filer. Till exempel `mkdir hivedriver`.

        2. Använd hello följande kommandon från en kommandorad toocopy hello filer från hello HDInsight-kluster:

            ```bash
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-jdbc*standalone.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
            ```

            Ersätt `USERNAME` med hello SSH användarkontonamnet för hello-kluster. Ersätt `CLUSTERNAME` med hello HDInsight-klustrets namn.

    * För **Windows-baserade HDInsight**, Använd hello följande steg toodownload hello jar-filer.

        1. Välj ditt HDInsight-kluster från hello Azure-portalen, och välj sedan hello **fjärrskrivbord** ikon.

            ![Ikon för Remote Desktop](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopicon.png)

        2. Använd hello på hello fjärrskrivbord bladet **Anslut** knappen tooconnect toohello klustret. Om hello fjärrskrivbord inte är aktiverad använder hello formuläret tooprovide ett användarnamn och lösenord och väljer sedan **aktivera** tooenable Remote Desktop för hello-kluster.

            ![Remote desktop bladet](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopblade.png)

            När du har valt **Anslut**, hämtas en RDP-fil. Använd den här filen toolaunch hello fjärrskrivbordsklienten. När du uppmanas, Använd hello användarnamn och lösenord som du angav för åtkomst via fjärrskrivbord.

        3. När du är ansluten, kopiera följande filer från hello Remote Desktop session tooyour lokal dator hello. Placera dem i en lokal katalog med namnet `hivedriver`.

            * C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-JDBC-0.14.0.2.2.9.1-7-standalone.JAR
            * C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-Common-2.6.0.2.2.9.1-7.JAR
            * C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.JAR

            > [!NOTE]
            > hello version siffror som ingår i hello sökvägar och filnamn kan vara olika för klustret.

        4. Koppla från hello fjärrskrivbordssession när du är klar med att kopiera hello-filer.

2. Starta hello SQuirreL SQL-programmet. Välj hello till vänster i fönstret hello **drivrutiner**.

    ![Fliken drivrutiner på hello till vänster i fönstret hello](./media/hdinsight-connect-hive-jdbc-driver/squirreldrivers.png)

3. Från hello ikoner hello överst i hello **drivrutiner** dialogrutan, Välj hello  **+**  ikonen toocreate en drivrutin.

    ![Drivrutiner ikoner](./media/hdinsight-connect-hive-jdbc-driver/driversicons.png)

4. Lägg till hello följande information i dialogrutan Lägg till drivrutin hello:

    * **Namnet**: Hive
    * **Exempel-URL**:`jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`
    * **Extra klassökvägen**: Använd hello Lägg till knappen tooadd hello jar-filer hämtade tidigare
    * **Klassnamn**: org.apache.hive.jdbc.HiveDriver

   ![Lägg till dialogrutan för drivrutin](./media/hdinsight-connect-hive-jdbc-driver/adddriver.png)

   Klicka på **OK** toosave inställningarna.

5. Hello till vänster i hello SQuirreL SQL-fönstret, Välj **alias**. Klicka på hello  **+**  ikonen toocreate ett alias för anslutningen.

    ![Lägg till nytt alias](./media/hdinsight-connect-hive-jdbc-driver/aliases.png)

6. Använd hello följande värden för hello **lägga till Alias** dialogrutan.

    * **Namnet**: Hive i HDInsight

    * **Drivrutinen**: Använd hello listrutan tooselect hello **Hive** drivrutin

    * **URL: en**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

        Ersätt **KLUSTERNAMN** med hello namnet på ditt HDInsight-kluster.

    * **Användarnamnet**: hello klustrets inloggningsnamn konto för ditt HDInsight-kluster. hello standardvärdet är `admin`.

    * **Lösenordet**: hello lösenordet för hello inloggningskonto för klustret.

 ![alias dialogrutan Lägg till](./media/hdinsight-connect-hive-jdbc-driver/addalias.png)

    Använd hello **Test** knappen tooverify som hello anslutning fungerar. När **ansluta till: Hive i HDInsight** dialogrutan visas, väljer du **Anslut** tooperform hello test. Om hello testet lyckas visas en **lyckad anslutning** dialogrutan.

    toosave hello anslutning alias, använda hello **Ok** knappen längst ned hello hello **lägga till Alias** dialogrutan.

7. Från hello **ansluta till** listrutan överst hello i SQuirreL SQL, Välj **Hive i HDInsight**. När du uppmanas, Välj **Anslut**.

    ![anslutningsdialogrutan](./media/hdinsight-connect-hive-jdbc-driver/connect.png)

8. När du är ansluten, ange hello följande fråga i dialogrutan för hello SQL-fråga och välj sedan hello **kör** ikon. hello resultatområdet ska visa hello resultaten av hello-frågan.

        select * from hivesampletable limit 10;

    ![dialogrutan för SQL-fråga, inklusive resultat](./media/hdinsight-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a>Ansluta från ett exempel Java-program

Ett exempel på hur en Java-klienten tooquery Hive i HDInsight finns på [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc). Följ anvisningarna för hello i hello databasen toobuild och kör hello exempel.

## <a name="troubleshooting"></a>Felsökning

### <a name="unexpected-error-occurred-attempting-tooopen-an-sql-connection"></a>Oväntat fel uppstod när tooopen en SQL-anslutning

**Symptom**: när du ansluter tooan HDInsight-kluster som är version 3.3 eller 3.4, kan ett felmeddelande som ett oväntat fel uppstod. hello stack-spårning för det här felet börjar med hello följande rader:

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

**Orsak**: det här felet beror på ett matchningsfel för hello-versionen av hello commons codec.jar filen används av SQuirreL och hello en krävs av hello Hive JDBC-komponenter.

**Lösning**: toofix felet, Använd hello följande steg:

1. Hämta hello commons codec jar-filen från ditt HDInsight-kluster.

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. Avsluta SQuirreL och sedan gå toohello directory där SQuirreL är installerad på datorn. I hello SquirreL katalog under hello `lib` directory, Ersätt hello befintliga commons-codec.jar med hello en hämtas från hello HDInsight-kluster.

3. Starta om SQuirreL. hello fel bör inte längre inträffa när du ansluter tooHive på HDInsight.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur länkar toouse JDBC toowork med Hive, Använd hello följande tooexplore andra sätt toowork med Azure HDInsight.

* [Ladda upp data tooHDInsight](hdinsight-upload-data.md)
* [Använda Hive med HDInsight](hdinsight-use-hive.md)
* [Använda Pig med HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce-jobb med HDInsight](hdinsight-use-mapreduce.md)
