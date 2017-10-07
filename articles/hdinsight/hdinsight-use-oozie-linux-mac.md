---
title: "aaaUse Hadoop Oozie arbetsflöden i Linux-baserat HDInsight | Microsoft Docs"
description: "Använd Hadoop Oozie i Linux-baserade HDInsight. Lär dig hur toodefine ett arbetsflöde för Oozie och skicka ett Oozie-jobb."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d7603471-5076-43d1-8b9a-dbc4e366ce5d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: larryfr
ms.openlocfilehash: cb5682837543312621e3424b7a9341b5d2a00bf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-oozie-with-hadoop-toodefine-and-run-a-workflow-on-linux-based-hdinsight"></a>Använda Oozie med Hadoop toodefine och köra ett arbetsflöde på Linux-baserat HDInsight

[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Lär dig hur toouse Apache Oozie med Hadoop i HDInsight. Apache Oozie är ett arbetsflöde/samordning system som hanterar Hadoop-jobb. Oozie är integrerad med hello Hadoop-stacken och stöder hello efter jobb:

* Apache MapReduce
* Apache Pig
* Apache Hive
* Apache Sqoop

Oozie kan också använda tooschedule jobb som är specifika tooa system, som Java-program eller kommandoskript

> [!NOTE]
> Ett annat alternativ för att definiera arbetsflöden med HDInsight är Azure Data Factory. toolearn mer om Azure Data Factory finns [Use Pig och Hive med Data Factory][azure-data-factory-pig-hive].

> [!IMPORTANT]
> Oozie har inte aktiverats på domänanslutna HDInsight.

## <a name="prerequisites"></a>Krav

* **Ett HDInsight-kluster**: finns [komma igång med HDInsight på Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

  > [!IMPORTANT]
  > hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="example-workflow"></a>Arbetsflödesexemplet

hello-arbetsflöde som används i det här dokumentet innehåller två åtgärder. Åtgärder är definitioner för uppgifter som att köra Hive, Sqoop, MapReduce eller någon annan process:

![Arbetsflödesdiagram][img-workflow-diagram]

1. En Hive-åtgärd körs HiveQL-skript tooextract poster från hello **hivesampletable** ingår i HDInsight. Varje rad med data beskriver ett besök av specifika mobila enheter. hello postformatet visas liknande toohello följande text:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    hello Hive-skript som används i det här dokumentet räknar hello totala besök för varje plattform (till exempel Android eller iPhone) och lagrar hello-antal tooa nya Hive-tabellen.

    Mer information om Hive finns i [Använda Hive med HDInsight][hdinsight-use-hive].

2. En åtgärd för Sqoop exporterar hello innehållet i hello nya Hive tooa tabellen i en Azure SQL database. Läs mer om Sqoop [Använd Hadoop Sqoop med HDInsight][hdinsight-use-sqoop].

> [!NOTE]
> Versioner som stöds Oozie i HDInsight-kluster, se [vad är nytt i hello Hadoop-klusterversioner som tillhandahålls av HDInsight][hdinsight-versions].

## <a name="create-hello-working-directory"></a>Skapa hello arbetskatalogen

Oozie förväntar resurser som krävs för ett jobb toobe lagrad i hello samma katalog. Det här exemplet används **wasb: / / / självstudier/useoozie**. Använd hello efter kommandot toocreate den här katalogen och hello data-katalog som innehåller hello nya Hive-tabell skapas av det här arbetsflödet:

```
hdfs dfs -mkdir -p /tutorials/useoozie/data
```

> [!NOTE]
> Hej `-p` parametern medför att alla kataloger i hello sökvägen toobe skapas. Hej **data** katalogen är används toohold data som används av hello **useooziewf.hql** skript.

Också köra följande kommando, vilket garanterar att Oozie kan personifiera ditt användarkonto när du kör Hive och Sqoop jobb hello. Ersätt **användarnamn** med ditt inloggningsnamn:

```
sudo adduser USERNAME users
```

> [!NOTE]
> Du kan ignorera felen hello användaren är redan medlem av hello `users` grupp.

## <a name="add-a-database-driver"></a>Lägga till en databasdrivrutin

Eftersom det här arbetsflödet använder Sqoop tooexport data tooSQL databasen, måste du ange en kopia av hello JDBC-drivrutinen används tootalk tooSQL databas. Använd hello följande kommando toocopy den toohello arbetskatalog:

```
hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/
```

Om arbetsflödet används andra resurser, till exempel en jar som innehåller ett MapReduce-program, behöver du tooadd samt dessa resurser.

## <a name="define-hello-hive-query"></a>Definiera hello Hive-fråga

Använd hello följande steg toocreate HiveQL-skript som definierar en fråga som används i ett arbetsflöde för Oozie senare i det här dokumentet.

1. Ansluta toohello kluster med SSH. hello följande kommando är ett exempel på hur du använder hello `ssh` kommando. Ersätt __användarnamn__ med hello SSH-användare för hello-kluster. Ersätt __KLUSTERNAMN__ med hello namnet hello HDInsight-kluster.

    ```
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Använd hello efter kommandot toocreate en fil från hello SSH-anslutningen:

    ```
    nano useooziewf.hql
    ```

3. När hello nanoredigeraren öppnas Använd hello följande fråga som hello innehållet i hello-fil:

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    Det finns två variabler i hello skript:

    * **${hiveTableName}**: innehåller hello namnet på hello tabell toobe skapas

    * **${hiveDataFolder}**: innehåller hello plats toostore hello-datafiler för hello tabellen

    arbetsflöde för hello-definitionsfil (workflow.xml i den här självstudiekursen) skickar dessa värden toothis HiveQL-skript vid körning.

4. tooexit hello redigerare, tryck på Ctrl + X. När du uppmanas, Välj **Y** toosave hello fil och sedan använda **RETUR** toouse hello **useooziewf.hql** filnamn.

5. Använd hello följande kommandon toocopy **useooziewf.hql** för**wasb:///tutorials/useoozie/useooziewf.hql**:

    ```
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    Dessa kommandon lagra hello **useooziewf.hql** fil på hello HDFS-kompatibla lagring för hello-kluster.

## <a name="define-hello-workflow"></a>Definiera hello-arbetsflöde

Oozie arbetsflöden definitioner skrivs i hPDL (en XML-processen Definition Language). Använd följande steg toodefine hello arbetsflödet hello:

1. Använd följande instruktion toocreate hello och redigera en ny fil:

    ```
    nano workflow.xml
    ```

2. Ange hello följande XML som hello filinnehållet när hello nanoredigeraren öppnas:

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start too= "RunHiveScript"/>
        <action name="RunHiveScript">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.job.queue.name</name>
                <value>${queueName}</value>
            </property>
            </configuration>
            <script>${hiveScript}</script>
            <param>hiveTableName=${hiveTableName}</param>
            <param>hiveDataFolder=${hiveDataFolder}</param>
        </hive>
        <ok to="RunSqoopExport"/>
        <error to="fail"/>
        </action>
        <action name="RunSqoopExport">
        <sqoop xmlns="uri:oozie:sqoop-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.compress.map.output</name>
                <value>true</value>
            </property>
            </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveDataFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\t"</arg>
            <archive>sqljdbc41.jar</archive>
            </sqoop>
        <ok to="end"/>
        <error to="fail"/>
        </action>
        <kill name="fail">
        <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>
        <end name="end"/>
    </workflow-app>
    ```

    Det finns två åtgärder som definierats i hello arbetsflöde:

   * **RunHiveScript**: den här åtgärden är hello start åtgärd och kör hello **useooziewf.hql** registreringsdatafilen

   * **RunSqoopExport**: den här åtgärden exporterar hello data som har skapats från hello Hive-skript tooSQL databasen med Sqoop. Den här åtgärden körs bara om hello **RunHiveScript** åtgärden lyckas.

     hello arbetsflödet har flera transaktioner som `${jobTracker}`. De här posterna ersätts med värden som du använder i hello jobbdefinitionen. hello jobbdefinitionen skapas senare i det här dokumentet.

     Även Obs hello `<archive>sqljdbc4.jar</arcive>` post i hello Sqoop avsnitt. Den här posten instruerar Oozie toomake det här arkivet för Sqoop när den här åtgärden körs.

3. Använd Ctrl + X sedan **Y** och **RETUR** toosave hello-filen.

4. Använd hello följande kommando toocopy hello **workflow.xml** filen för**/tutorials/useoozie/workflow.xml**:

    ```
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-hello-database"></a>Skapa hello-databas

toocreate en Azure SQL Database så hello i hello [skapa en SQL-databas](../sql-database/sql-database-get-started.md) dokumentet. När du skapar hello databasen använder `oozietest` som hello databasnamn. Även anteckna hello namnet på hello databasserver.

### <a name="create-hello-table"></a>Skapa hello tabell

> [!NOTE]
> Det finns många sätt tooconnect tooSQL databasen toocreate en tabell. Hej följande steg används [FreeTDS](http://www.freetds.org/) från hello HDInsight-kluster.


1. Använd hello efter kommandot tooinstall FreeTDS på hello HDInsight-kluster:

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. När du har installerat FreeTDS kommando Använd hello följande tooconnect toohello SQL Database-server som du skapade tidigare:

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest
    ```

    Du får utdata liknande toohello följande text:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set toooozietest
        1>

3. Vid hello `1>` uppmanar, ange hello följande rader:

    ```
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    När hello `GO` uttryck har angetts, hello tidigare rapporter utvärderas. De här uttrycken skapa en tabell med namnet **mobiledata** som används av hello arbetsflöde.

    Använd hello följande tooverify som hello tabellen har skapats:

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    Du kan se utdata liknande toohello följande text:

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    oozietest       dbo     mobiledata      BASE TABLE
    ```

4. Ange `exit` på hello `1>` fråga tooexit hello tsql-verktyget.

## <a name="create-hello-job-definition"></a>Skapa hello jobbdefinitionen

hello jobbdefinitionen beskriver där toofind hello workflow.xml. Här beskrivs också var toofind andra filer som används av hello arbetsflöde (till exempel useooziewf.hql.) Den definierar även hello värden för egenskaper som används i hello arbetsflöde och tillhörande filer.

1. Använd följande kommando tooget hello fullständiga adress hello standardlagring hello. Den här adressen används i konfigurationsfilen för hello strax:

    ```
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    Det här kommandot returnerar informationen liknande toohello följande XML:

    ```xml
    <name>fs.defaultFS</name>
    <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]
    > Om hello HDInsight-kluster använder Azure Storage som hello standardlagring, hello `<value>` element innehållet börjar med `wasb://`. Om Azure Data Lake Store används i stället, den börjar med `adl://`.

    Spara hello innehållet i hello `<value>` elementet eftersom den används i hello nästa steg.

2. Använd följande kommando tooget FQDN för hello klustret headnode hello. Den här informationen används för hello JobTracker adress för hello klustret:

    ```
    hostname -f
    ```

    Detta returnerar information liknande toohello följande text:

    ```hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net```

    hello port som används för hello JobTracker är 8050, så hello fullständiga adress toouse för hello JobTracker `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.

3. Använd hello följande toocreate hello Oozie jobbet definition konfiguration:

    ```
    nano job.xml
    ```

4. När hello nanoredigeraren öppnas Använd hello följande XML som hello innehållet i hello-fil:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
        <name>nameNode</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
        </property>

        <property>
        <name>jobTracker</name>
        <value>JOBTRACKERADDRESS</value>
        </property>

        <property>
        <name>queueName</name>
        <value>default</value>
        </property>

        <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
        </property>

        <property>
        <name>hiveScript</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
        </property>

        <property>
        <name>hiveTableName</name>
        <value>mobilecount</value>
        </property>

        <property>
        <name>hiveDataFolder</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
        </property>

        <property>
        <name>sqlDatabaseConnectionString</name>
        <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
        </property>

        <property>
        <name>sqlDatabaseTableName</name>
        <value>mobiledata</value>
        </property>

        <property>
        <name>user.name</name>
        <value>YourName</value>
        </property>

        <property>
        <name>oozie.wf.application.path</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
    </configuration>
    ```

   * Ersätt alla förekomster av  **wasb://mycontainer@mystorageaccount.blob.core.windows.net**  med hello-värde som du fick tidigare för standardlagring.

     > [!WARNING]
     > Om hello sökvägen är en `wasb` sökväg, måste du använda hello fullständig sökväg. Inte förkorta toojust `wasb:///`.

   * Ersätt **JOBTRACKERADDRESS** med hello JobTracker/ResourceManager-adress som du fått tidigare.
   * Ersätt **dittnamn** med ditt inloggningsnamn för hello HDInsight-kluster.
   * Ersätt **serverName**, **adminLogin**, och **adminPassword** med hello information för din Azure SQL Database.

     I de flesta hello informationen i filen är används toopopulate hello värden som används i hello workflow.xml eller ooziewf.hql filer (till exempel ${nameNode}.)

     > [!NOTE]
     > Hej **oozie.wf.application.path** post definierar var toofind hello workflow.xml filen som innehåller hello arbetsflödet kördes av jobbet.

5. Använd Ctrl + X sedan **Y** och **RETUR** toosave hello-filen.

## <a name="submit-and-manage-hello-job"></a>Skicka in och hantera hello jobb

hello följande använda hello Oozie kommandot toosubmit och hantera Oozie arbetsflöden på hello klustret. Hej Oozie kommando är ett gränssnitt för egna över hello [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [!IMPORTANT]
> När kommandot hello Oozie måste du använda hello FQDN för hello HDInsight headnode. Detta FQDN är enbart tillgänglig från hello kluster, eller om hello klustret på ett virtuellt Azure-nätverk från andra datorer på hello samma nätverk.


1. Använd följande tooobtain hello URL toohello Oozie service hello:

    ```
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    Detta returnerar information liknande toohello följande XML:

    ```xml
    <name>oozie.base.url</name>
    <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    Hej `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` del är hello URL toouse med hello Oozie-kommando.

2. Använd hello följande toocreate en miljövariabel för hello-URL så att du inte behöver tootype för varje kommando:

    ```
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

    Ersätt hello URL med hello en tidigare.
3. Använd hello följande toosubmit hello jobb:

    ```
    oozie job -config job.xml -submit
    ```

    Det här kommandot laddar hello jobbinformation från **job.xml** och skickar den tooOozie, men har inte köra den.

    Det ska returnera hello-ID för hello jobbet när hello-kommandot har slutförts. Till exempel `0000005-150622124850154-oozie-oozi-W`. Detta ID är används toomanage hello jobb.

4. Visa hello status för hello jobb med hjälp av hello följande kommando:

    ```
    oozie job -info <JOBID>
    ```

    > [!NOTE]
    > Ersätt `<JOBID>` med hello-ID som returneras i hello föregående steg.

    Detta returnerar information liknande toohello följande text:

    ```
    Job ID : 0000005-150622124850154-oozie-oozi-W
    ------------------------------------------------------------------------------------------------------------------------------------
    Workflow Name : useooziewf
    App Path      : wasb:///tutorials/useoozie
    Status        : PREP
    Run           : 0
    User          : USERNAME
    Group         : -
    Created       : 2015-06-22 15:06 GMT
    Started       : -
    Last Modified : 2015-06-22 15:06 GMT
    Ended         : -
    CoordAction ID: -
    ------------------------------------------------------------------------------------------------------------------------------------
    ```

    Det här jobbet har statusen `PREP`. Denna status anger hello jobbet har skapats, men inte har startats.

5. Använd hello följande kommando toostart hello jobb:

    ```
    oozie job -start JOBID
    ```

    > [!NOTE]
    > Ersätt `<JOBID>` med hello-ID som returneras tidigare.

    Om du kontaktar hello status efter kommandot, den är i ett körningstillstånd och information returneras för hello åtgärder inom hello jobb.

6. När hello aktiviteten är klar kan du kontrollera att hello data genererades och exporterade toohello SQL Database-tabellen med hello följande kommandon:

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest
    ```

    Vid hello `1>` uppmanar, ange hello följande fråga:

    ```
    SELECT * FROM mobiledata
    GO
    ```

    hello information som returnerades är liknande toohello följande text:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Mer information om hello Oozie kommandot finns [Oozie kommandoradsverktyget](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).

## <a name="oozie-rest-api"></a>Oozie REST API

Hej Oozie REST-API kan du toobuild egna verktyg som fungerar med Oozie. hello följande är HDInsight specifik information om hur du använder hello Oozie REST API:

* **URI**: hello REST-API som kan nås från utanför hello klustret på`https://CLUSTERNAME.azurehdinsight.net/oozie`

* **Autentisering**: autentisera toohello API hello klustret HTTP-konto (admin) och lösenord. Exempel:

    ```
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

Mer information om hur du använder hello Oozie REST-API finns [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

## <a name="oozie-web-ui"></a>Oozie webbgränssnittet

Hej Oozie Webbgränssnittet kan du öppna webbaserade hello status för Oozie-jobb på hello klustret. hello webbgränssnittet kan tooview hello följande information:

* Jobbstatus
* Jobbdefinitionen
* Konfiguration
* Ett diagram över hello åtgärder i hello jobb
* Loggar för jobbet hello

Du kan också visa information om åtgärder i ett jobb.

tooaccess hello Oozie Webbgränssnittet använder hello följande steg:

1. Skapa en SSH-tunnel toohello HDInsight-kluster. Mer information finns i hello [använda SSH-tunnlar med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentet.

2. När en tunnel har skapats, öppnar du hello Ambari-webbgränssnittet i webbläsaren. hello URI för hello Ambari plats är **https://CLUSTERNAME.azurehdinsight.net**. Ersätt **KLUSTERNAMN** med hello namnet på ditt Linux-baserade HDInsight-kluster.

3. Hello vänster sida av hello-sidan, Välj **Oozie**, sedan **snabblänkar**, och slutligen **Oozie Webbgränssnittet**.

    ![Bild av hello menyer](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. Hej Oozie Webbgränssnittet standardvärden toodisplaying kör arbetsflödesjobb. toosee alla arbetsflödesuppgifter, Välj **alla jobb**.

    ![Alla jobb som visas](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Välj ett jobb tooview mer information om hello jobb.

    ![Jobbinformationen](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. Du kan se grundläggande jobbinformation och hello enskilda åtgärder i hello jobb från hello Jobbinformationen fliken. Med hjälp av hello flikarna överst hello kan du visa hello jobbdefinitionen, jobb Configuration hello åtkomst till Jobblogg eller visa en dirigeras acykliska diagram (DAG) för hello jobb.

   * **Jobblogg**: Välj hello **GetLogs** tooget alla loggar för jobbet hello, eller använda hello **ange sökfilter** fältet toofilter loggar

       ![Jobblogg](./media/hdinsight-use-oozie-linux-mac/joblog.png)

   * **JobDAG**: hello DAG är en grafisk översikt över hello datasökvägar vidtas via hello-arbetsflöde

       ![Jobbet DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. Att välja något av hello hello **Jobbinformationen** fliken visar information om hello-åtgärd. Välj exempelvis hello **RunHiveScript** åtgärd.

    ![Åtgärden info](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Du kan visa detaljer för hello-åtgärden, till exempel en länk toohello **-konsolens URL**. Den här länken kan vara används tooview JobTracker information för hello jobbet.

## <a name="scheduling-jobs"></a>Schemalägger jobb

hello coordinator kan toospecify en start och end förekomsten frekvens för jobb. toodefine ett schema för hello arbetsflöde, Använd hello följande steg:

1. Använd hello följande toocreate en fil med namnet **coordinator.xml**:

    ```
    nano coordinator.xml
    ```

    Använd hello följande XML som hello innehållet i hello-fil:

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
        <workflow>
            <app-path>${workflowPath}</app-path>
        </workflow>
        </action>
    </coordinator-app>
    ```

    > [!NOTE]
    > Hej `${...}` variabler ersätts med värden i hello jobbdefinitionen vid körning. hello-variabler är:
    >
    > * `${coordFrequency}`: Tiden mellan instanser av hello jobbet körs.
    > ** `${coordStart}`: hello jobbets starttid.
    > * `${coordEnd}`: hello jobbets sluttid.
    > * `${coordTimezone}`: Coordinator jobb är i en fast tidszon med ingen sommartid (vanligtvis representeras med hjälp av UTC). Den här tidszonen kallas hello ”Oozie bearbetning tidszonen”.
    > * `${wfPath}`: hello sökvägen toohello workflow.xml.

2. toosave hello fil ska du använda Ctrl + X **Y**, och **RETUR**.

3. Använd hello efter kommandot toocopy hello filen toohello arbetskatalog för det här jobbet:

    ```
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. Använd hello följande toomodify hello **job.xml** fil:

    ```
    nano job.xml
    ```

    Att Hej följande ändringar:

   * tooinstruct oozie toorun hello coordinator fil i stället för hello arbetsflöde, ändra `<name>oozie.wf.application.path</name>` för`<name>oozie.coord.application.path</name>`.

   * tooset hello `workflowPath` variabeln som används av hello coordinator lägga till hello följande XML:

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       Ersätt hello `wasb://mycontainer@mystorageaccount.blob.core.windows` text med hello-värde som används i andra transaktioner i hello job.xml-filen.

   * Lägg till hello följande XML toodefine hello start och end frekvens för hello coordinator:

        ```xml
        <property>
            <name>coordStart</name>
            <value>2017-05-10T12:00Z</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>2017-05-12T12:00Z</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>1440</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>UTC</value>
        </property>
        ```

       Dessa värden anges hello start tid too12: 00 PM på 10 maj 2017 hello slutet tid tooMay 12, 2017. hello-intervall för det här jobbet körs varje dag. hello frekvensen är i minuter, så 24 timmar x 60 minuter = 1440 minuter. Slutligen anges hello tidszonen tooUTC.

5. Använd Ctrl + X sedan **Y** och **RETUR** toosave hello-filen.

6. toorun hello jobbet, Använd hello följande kommando:

    ```
    oozie job -config job.xml -run
    ```

    Det här kommandot skickar och startar hello jobb.

7. Om du besöker hello Oozie Webbgränssnittet och välj hello **Coordinator jobb** kan du se information liknande toohello följande bild:

    ![fliken för Coordinator jobb](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    Hej **nästa materialisering** posten innehåller hello nästa gång hello jobbet körs.

8. Liknande toohello tidigare arbetsflödesjobbet Markera hello jobbet post i hello webbgränssnittet visar information om hello jobb:

    ![Coordinator Jobbinformationen](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    > [!NOTE]
    > Den här bilden visar endast lyckade körningar av hello jobb, inte enskilda åtgärder hello schemalagda arbetsflödet. toosee som väljer du något av hello **åtgärd** poster.

    ![Åtgärden info](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

## <a name="troubleshooting"></a>Felsökning

Hej Oozie UI kan tooview Oozie loggar. Den innehåller också länkar tooJobTracker loggar för MapReduce-aktiviteter som startas av hello arbetsflöde. hello mönster för att felsöka bör vara:

1. Visa hello jobb i Oozie-Webbgränssnittet.

2. Om det finns ett fel eller ett misslyckande för en specifik åtgärd, Välj hello åtgärd toosee om hello **felmeddelande** fältet innehåller mer information om hello-fel.

3. Om det finns använder du hello URL: en från hello åtgärd tooview mer information (till exempel JobTracker loggar) för hello-åtgärd.

hello nedan visas specifika fel som kan uppstå, och hur tooresolve dem.

### <a name="ja009-cannot-initialize-cluster"></a>JA009: Det går inte att initiera kluster

**Symptom**: hello ändringar av jobbstatus för**PAUSAD**. Information om hello jobbet visar hello RunHiveScript status som **START_MANUAL**. Välja hello åtgärd visar hello följande felmeddelande:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**Orsak**: hello WASB-adresser som används i hello **job.xml** filen innehåller inte hello lagringsbehållaren eller namnet för lagringskontot. Hej WASB-adressformat måste vara `wasb://containername@storageaccountname.blob.core.windows.net`.

**Lösning**: ändra hello WASB-adresser som används av hello jobb.

### <a name="ja002-oozie-is-not-allowed-tooimpersonate-ltuser"></a>JA002: Oozie är inte tillåtet tooimpersonate &lt;användare >

**Symptom**: hello ändringar av jobbstatus för**PAUSAD**. Information om hello jobbet visar hello RunHiveScript status som **START_MANUAL**. Välja hello åtgärd visar hello följande felmeddelande:

    JA002: User: oozie is not allowed tooimpersonate <USER>

**Orsak**: aktuella behörighetsinställningarna Tillåt inte Oozie tooimpersonate hello angivna användarkontot.

**Lösning**: Oozie tooimpersonate användare tillåts i hello **användare** grupp. Använd hello `groups USERNAME` toosee hello grupper som hello användarkonto är medlem i. Om hello användaren inte är medlem i hello **användare** och använda hello efter kommandot tooadd hello toohello grupp:

    sudo adduser USERNAME users

> [!NOTE]
> Det kan ta flera minuter innan HDInsight identifierar hello användaren har lagts till toohello grupp.

### <a name="launcher-error-sqoop"></a>Starta fel (Sqoop)

**Symptom**: hello ändringar av jobbstatus för**KILLED**. Information om hello jobbet visar hello RunSqoopExport status som **fel**. Välja hello åtgärd visar hello följande felmeddelande:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**Orsak**: Sqoop är tooload hello databasen krävs tooaccess hello databas.

**Lösning**: när du använder Sqoop från ett Oozie-jobb, måste du inkludera hello databasdrivrutinen med hello andra resurser (till exempel hello workflow.xml) som används av hello jobb. Också referera hello Arkiv som innehåller hello databasdrivrutinen från hello `<sqoop>...</sqoop>` avsnitt i hello workflow.xml.

Till exempel använder hello-jobbet i det här dokumentet hello följande steg:

1. Kopiera hello sqljdbc4.1.jar toohello /tutorials/useoozie katalog:

    ```
    hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar
    ```

2. Ändra följande XML på en ny rad ovanför hello workflow.xml tooadd hello `</sqoop>`:

    ```xml
    <archive>sqljdbc41.jar</archive>
    ```

## <a name="next-steps"></a>Nästa steg

I kursen får du lärt dig hur toodefine ett arbetsflöde för Oozie och hur toorun ett Oozie-jobb. toolearn mer information om hur du arbetar med HDInsight finns hello följande artiklar:

* [Använd tidsbaserad Oozie-koordinator med HDInsight][hdinsight-oozie-coordinator-time]
* [Överföra data för Hadoop-jobb i HDInsight][hdinsight-upload-data]
* [Använda Sqoop med Hadoop i HDInsight][hdinsight-use-sqoop]
* [Använda Hive med Hadoop i HDInsight][hdinsight-use-hive]
* [Använda Pig med Hadoop i HDInsight][hdinsight-use-pig]
* [Utveckla Java-MapReduce-program för HDInsight][hdinsight-develop-mapreduce]

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563
[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
