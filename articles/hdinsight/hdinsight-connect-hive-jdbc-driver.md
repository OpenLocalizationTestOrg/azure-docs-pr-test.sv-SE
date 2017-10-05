---
title: "Fråga Hive genom JDBC driver - Azure HDInsight | Microsoft Docs"
description: "Använda JDBC-drivrutinen från ett Java-program för att skicka Hive-frågor till Hadoop på HDInsight. Ansluta programmässigt och SQuirrel SQL-klient."
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
ms.openlocfilehash: f2de58c2763b2ddd0926336164738929c477c4ae
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="query-hive-through-the-jdbc-driver-in-hdinsight"></a>Frågan Hive genom JDBC driver i HDInsight

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Lär dig hur du använder JDBC driver från ett Java-program för att skicka Hive-frågor till Hadoop i Azure HDInsight. Informationen i det här dokumentet beskrivs hur du ansluta programmässigt och SQuirrel SQL-klient.

Läs mer på gränssnittet Hive JDBC [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).

## <a name="prerequisites"></a>Krav

* En Hadoop på HDInsight-kluster. Linux-baserade eller Windows-baserade kluster fungerar.

  > [!IMPORTANT]
  > Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare. Mer information finns i [HDInsight 3.3 pensionering](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [SQuirreL SQL](http://squirrel-sql.sourceforge.net/). SQuirreL är en JDBC-klientprogram.

* Den [Java Developer Kit (JDK) version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) eller högre.

* [Apache Maven](https://maven.apache.org). Maven är ett projekt bygg system för Java-projekt som används av projektet som associeras med den här artikeln.

## <a name="jdbc-connection-string"></a>JDBC-anslutningssträng

JDBC-anslutningar till ett HDInsight-kluster i Azure görs över 443 och trafiken skyddas med SSL. Den offentliga gateway som kluster hamnar bakom omdirigerar trafik till den port som faktiskt HiveServer2 lyssnar på. Följande är ett exempel anslutningssträngen:

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

Ersätt `CLUSTERNAME` med namnet på HDInsight-klustret.

## <a name="authentication"></a>Autentisering

När anslutningen upprättas, måste du använda HDInsight-kluster admin namn och lösenord för att autentisera till kluster-gateway. När du ansluter från JDBC-klienter, till exempel SQuirreL SQL måste du ange admin namn och lösenord i klientinställningarna.

Från ett Java-program, måste du använda namn och lösenord när en anslutning upprättas. Till exempel öppnas följande Java-kod en ny anslutning med anslutningssträngen, admin namn och lösenord:

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a>Ansluta med SQuirreL SQL-klienten

SQuirreL SQL är en JDBC-klient som kan användas för att köra Hive-frågor via fjärranslutning med ditt HDInsight-kluster. Följande steg förutsätter att du redan har installerat SQuirreL SQL.

1. Kopiera Hive JDBC-drivrutiner från ditt HDInsight-kluster.

    * För **Linux-baserat HDInsight**, Använd följande steg för att hämta nödvändiga jar-filer.

        1. Skapa en katalog som innehåller filerna. Till exempel `mkdir hivedriver`.

        2. Använd följande kommandon för att kopiera filer från HDInsight-kluster från en kommandorad:

            ```bash
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-jdbc*standalone.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
            ```

            Ersätt `USERNAME` med SSH användarkontonamnet för klustret. Ersätt `CLUSTERNAME` med HDInsight-klustrets namn.

    * För **Windows-baserade HDInsight**, Använd följande steg för att hämta jar-filer.

        1. Välj ditt HDInsight-kluster från Azure-portalen och välj sedan den **fjärrskrivbord** ikon.

            ![Ikon för Remote Desktop](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopicon.png)

        2. I bladet fjärrskrivbord kan använda den **Anslut** för att ansluta till klustret. Om fjärrdatorn inte är aktiverad, Använd formuläret för att ange ett användarnamn och lösenord och väljer sedan **aktivera** att aktivera Remote Desktop för klustret.

            ![Remote desktop bladet](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopblade.png)

            När du har valt **Anslut**, hämtas en RDP-fil. Använd den här filen för att starta fjärrskrivbordsklienten. När du uppmanas, använder du användarnamnet och lösenordet du angav för åtkomst via fjärrskrivbord.

        3. När du är ansluten, kopiera följande filer från sessionen för fjärrskrivbord på din lokala dator. Placera dem i en lokal katalog med namnet `hivedriver`.

            * C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-JDBC-0.14.0.2.2.9.1-7-standalone.JAR
            * C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-Common-2.6.0.2.2.9.1-7.JAR
            * C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.JAR

            > [!NOTE]
            > Versionsnummer som ingår i sökvägar och filnamn kan vara olika för klustret.

        4. Koppla från fjärrskrivbordssession när du har slutfört kopieringen av filer.

2. Starta SQuirreL SQL-programmet. Till vänster i fönstret Välj **drivrutiner**.

    ![Fliken drivrutiner till vänster i fönstret](./media/hdinsight-connect-hive-jdbc-driver/squirreldrivers.png)

3. Från ikonerna längst upp i den **drivrutiner** markerar den  **+**  ikon för att skapa en drivrutin.

    ![Drivrutiner ikoner](./media/hdinsight-connect-hive-jdbc-driver/driversicons.png)

4. Lägg till följande information i dialogrutan Lägg till drivrutin:

    * **Namnet**: Hive
    * **Exempel-URL**:`jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`
    * **Extra klassökvägen**: Använd knappen Lägg till för att lägga till jar-filer tidigare
    * **Klassnamn**: org.apache.hive.jdbc.HiveDriver

   ![Lägg till dialogrutan för drivrutin](./media/hdinsight-connect-hive-jdbc-driver/adddriver.png)

   Klicka på **OK** dessa inställningar ska sparas.

5. Till vänster i fönstret SQuirreL SQL markerar **alias**. Klicka på den  **+**  ikon för att skapa ett alias för anslutningen.

    ![Lägg till nytt alias](./media/hdinsight-connect-hive-jdbc-driver/aliases.png)

6. Använd följande värden för den **lägga till Alias** dialogrutan.

    * **Namnet**: Hive i HDInsight

    * **Drivrutinen**: Använd listrutan för att välja den **Hive** drivrutin

    * **URL: en**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

        Ersätt **KLUSTERNAMN** med namnet på ditt HDInsight-kluster.

    * **Användarnamnet**: klustret konto inloggningsnamnet för ditt HDInsight-kluster. Standardvärdet är `admin`.

    * **Lösenordet**: lösenordet för inloggningskontot för klustret.

 ![alias dialogrutan Lägg till](./media/hdinsight-connect-hive-jdbc-driver/addalias.png)

    Använd den **Test** för att kontrollera att anslutningen fungerar. När **ansluta till: Hive i HDInsight** dialogrutan visas, väljer du **Anslut** att utföra testet. Om testet lyckas, ser du en **lyckad anslutning** dialogrutan.

    För att spara anslutningen alias, Använd den **Ok** knappen längst ned i den **lägga till Alias** dialogrutan.

7. Från den **ansluta till** listrutan överst i SQuirreL SQL, Välj **Hive i HDInsight**. När du uppmanas, Välj **Anslut**.

    ![anslutningsdialogrutan](./media/hdinsight-connect-hive-jdbc-driver/connect.png)

8. När du är ansluten, ange följande fråga i dialogrutan för SQL-fråga och välj sedan den **kör** ikon. Resultatområdet ska visa resultatet av frågan.

        select * from hivesampletable limit 10;

    ![dialogrutan för SQL-fråga, inklusive resultat](./media/hdinsight-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a>Ansluta från ett exempel Java-program

Ett exempel på hur en Java-klient för att fråga Hive i HDInsight finns på [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc). Följ instruktionerna i databasen för att skapa och köra exemplet.

## <a name="troubleshooting"></a>Felsökning

### <a name="unexpected-error-occurred-attempting-to-open-an-sql-connection"></a>Ett oväntat fel uppstod när att öppna en SQL-anslutning

**Symptom**: vid anslutning till ett HDInsight-kluster som är version 3.3 eller 3.4, kan du få ett felmeddelande som ett oväntat fel uppstod. Stack-spårning för det här felet börjar med följande rader:

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

**Orsak**: det här felet beror på ett matchningsfel för versionen av filen commons codec.jar används av SQuirreL och den som krävs för komponenterna som Hive JDBC.

**Lösning**: åtgärda felet genom att använda följande steg:

1. Hämta commons codec jar-filen från ditt HDInsight-kluster.

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. Avsluta SQuirreL och gå sedan till katalogen där SQuirreL är installerad på datorn. I katalogen SquirreL under den `lib` directory, Ersätt den befintliga commons codec.jar med det hämtade från HDInsight-klustret.

3. Starta om SQuirreL. Fel bör inte längre uppstå när du ansluter till Hive i HDInsight.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur du använder JDBC för att arbeta med Hive, utforska andra sätt att arbeta med Azure HDInsight med hjälp av följande länkar.

* [Överföra data till HDInsight](hdinsight-upload-data.md)
* [Använda Hive med HDInsight](hdinsight-use-hive.md)
* [Använda Pig med HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce-jobb med HDInsight](hdinsight-use-mapreduce.md)
