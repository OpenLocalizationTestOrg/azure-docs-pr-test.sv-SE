---
title: "Använd tidsbaserade Hadoop Oozie-koordinator i HDInsight | Microsoft Docs"
description: "Använd tidsbaserade Hadoop Oozie-koordinator i HDInsight, en stordatatjänst. Lär dig hur du definierar Oozie arbetsflöden och koordinatorer och skicka jobb."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00c3a395-d51a-44ff-af2d-1f116c4b1c83
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 0fa8e3630610913d909a75bf76236d120c8f1a2b
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/30/2017
---
# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>Använda tidsbaserad Oozie-koordinator med Hadoop i HDInsight för att definiera arbetsflöden och samordna jobb
I den här artikeln lär du dig hur du definierar arbetsflöden och koordinatorer och hur du utlöser coordinator jobb, baserat på tid. Är det bra att gå igenom [Använd Oozie med HDInsight] [ hdinsight-use-oozie] innan den här artikeln. Förutom Oozie, kan du också schemalägga jobb med hjälp av Azure Data Factory. Information om Azure Data Factory finns [Use Pig och Hive med Data Factory](../data-factory/transform-data.md).

> [!NOTE]
> Den här artikeln kräver ett Windows-baserade HDInsight-kluster. Information om hur du använder Oozie, inklusive tidsbaserade jobb på en Linux-baserade kluster finns [Använd Oozie med Hadoop för att definiera och köra ett arbetsflöde på Linux-baserat HDInsight](hdinsight-use-oozie-linux-mac.md)

## <a name="what-is-oozie"></a>Vad är Oozie
Apache Oozie är ett arbetsflöde/samordning system som hanterar Hadoop-jobb. Det är integrerat med Hadoop-stacken och stöder Hadoop-jobb för Apache MapReduce, Apache Pig, Apache Hive och Apache Sqoop. Det kan också användas för att schemalägga jobb som är specifika för ett system, till exempel Java-program eller kommandoskript.

Följande bild visar arbetsflödet som du kommer att implementera:

![Arbetsflödesdiagram][img-workflow-diagram]

Arbetsflödet innehåller två åtgärder:

1. En Hive-åtgärden körs ett HiveQL-skript för att räkna antalet förekomster av varje loggningsnivån typ i en log4j-loggfil. Varje logg log4j består av en rad med fält som innehåller ett [LOGGNINGSNIVÅ] fält om du vill visa typen och allvarlighetsgrad, till exempel:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Utdata för Hive-skriptet är ungefär:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Mer information om Hive finns i [Använda Hive med HDInsight][hdinsight-use-hive].
2. En åtgärd för Sqoop exporterar HiveQL åtgärd utdata till en tabell i Azure SQL-databas. Läs mer om Sqoop [använda Sqoop med HDInsight][hdinsight-use-sqoop].

> [!NOTE]
> Versioner som stöds Oozie i HDInsight-kluster, se [vad är nytt i klusterversioner som tillhandahålls av HDInsight?] [hdinsight-versions].
>
>

## <a name="prerequisites"></a>Krav
Innan du påbörjar de här självstudierna måste du ha:

* **En arbetsstation med Azure PowerShell**.

    > [!IMPORTANT]
    > Stödet för Azure PowerShell för hantering av HDInsight-resurser med hjälp av Azure Service Manager är **föråldrat** och dras tillbaka den 1 januari 2017. I stegen i det här dokumentet används de nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.
    >
    > Följ stegen i [Installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) för att installera den senaste versionen av Azure PowerShell. Om du har skript som behöver ändras för att använda de nya cmdletarna som fungerar med Azure Resource Manager, hittar du mer information i [Migrera till Azure Resource Manager-baserade utvecklingsverktyg för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md).

* **Ett HDInsight-kluster**. Information om hur du skapar ett HDInsight-kluster finns i [skapa HDInsight-kluster][hdinsight-provision], eller [komma igång med HDInsight][hdinsight-get-started]. Du behöver följande data gå igenom kursen:

    <table border = "1">
    <tr><th>Kluster-egenskap</th><th>Variabelnamn för Windows PowerShell</th><th>Värde</th><th>Beskrivning</th></tr>
    <tr><td>HDInsight-klustrets namn</td><td>$clusterName</td><td></td><td>HDInsight-klustret som du vill köra den här kursen.</td></tr>
    <tr><td>Användarnamn för HDInsight-kluster</td><td>$clusterUsername</td><td></td><td>Användarnamnet för HDInsight-kluster. </td></tr>
    <tr><td>Användarlösenordet för HDInsight-kluster </td><td>$clusterPassword</td><td></td><td>HDInsight-kluster användarens lösenord.</td></tr>
    <tr><td>Azure storage-kontonamn</td><td>$storageAccountName</td><td></td><td>Ett Azure Storage-konto är tillgängligt för HDInsight-klustret. Använd standardkontot för lagring som du angav när klustret etablera för den här självstudiekursen.</td></tr>
    <tr><td>Azure Blob-behållarnamn</td><td>$containerName</td><td></td><td>I det här exemplet använder du Azure Blob storage-behållare som används för filsystemet för HDInsight-kluster. Som standard har samma namn som HDInsight-klustret.</td></tr>
    </table>

* **En Azure SQL database**. Du måste konfigurera en brandväggsregel för SQL Database-server att tillåta åtkomst från din arbetsstation. Anvisningar om att skapa en Azure SQL-databas och hur du konfigurerar brandväggen finns [komma igång med Azure SQL database][sqldatabase-get-started]. Den här artikeln innehåller en Windows PowerShell-skript för att skapa Azure SQL database-tabellen som ska användas för den här kursen.

    <table border = "1">
    <tr><th>Egenskapen för SQL-databas</th><th>Variabelnamn för Windows PowerShell</th><th>Värde</th><th>Beskrivning</th></tr>
    <tr><td>SQL server-databasnamn</td><td>$sqlDatabaseServer</td><td></td><td>SQL-databasservern som Sqoop kommer data att exporteras. </td></tr>
    <tr><td>Inloggningsnamn för SQL-databas</td><td>$sqlDatabaseLogin</td><td></td><td>Inloggningsnamn för SQL-databas.</td></tr>
    <tr><td>Inloggningslösenordet för SQL-databas</td><td>$sqlDatabaseLoginPassword</td><td></td><td>Lösenordet för inloggningen för SQL-databas.</td></tr>
    <tr><td>SQL-databasnamn</td><td>$sqlDatabaseName</td><td></td><td>Azure SQL-databas som Sqoop kommer data att exporteras. </td></tr>
    </table>

  > [!NOTE]
  > Som standard tillåter en Azure SQL database anslutningar från Azure-tjänster, till exempel Azure HDInsight. Om brandväggsinställningen är inaktiverad måste du aktivera det från Azure Portal. Instruktioner om hur du skapar en SQL-databas och konfigurera brandväggsregler, se [skapa och konfigurera SQL-databas][sqldatabase-get-started].

> [!NOTE]
> Fyll i värden i tabeller. Kommer att vara till hjälp för att gå igenom den här kursen.

## <a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Definiera Oozie arbetsflödet och relaterade HiveQL-skript
Oozie arbetsflöden definitioner skrivs i hPDL (en XML-processen definition language). Standardfilnamnet för arbetsflödet är *workflow.xml*.  Du spara arbetsflödesfilen lokalt och distribuera den till HDInsight-kluster med hjälp av Azure PowerShell senare i den här kursen.

Åtgärden Hive i arbetsflödet anropar en skriptfil för HiveQL. Den här skriptfilen innehåller tre HiveQL-instruktioner:

1. **DROP TABLE-instruktionen** tar bort log4j Hive-tabell om den finns.
2. **Instruktionen CREATE TABLE** skapar en log4j Hive extern tabell som pekar på platsen för loggfilen log4j;
3. **Platsen för loggfilen log4j**. Fältavgränsaren är ””,. Standard rad avgränsaren är ”\n”. En extern tabell Hive används för att undvika att filen tas bort från den ursprungliga platsen, om du vill köra arbetsflödet Oozie flera gånger.
4. **Instruktionen INSERT över** antal förekomster av varje loggningsnivån typ från log4j Hive-tabell och sparar utdata till en Azure Blob-lagringsplats.

> [!NOTE]
> Det finns ett känt problem i Hive-sökväg. När du skickar in ett Oozie-jobb körs i det här problemet. Anvisningar för hur du löser problemet finns på TechNet Wiki: [HDInsight Hive-fel: Det gick inte att byta namn på][technetwiki-hive-error].

**Definiera HiveQL skriptfilen som ska anropas av arbetsflödet**

1. Skapa en textfil med följande innehåll:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Det finns tre variabler som används i skriptet:

   * ${hiveTableName}
   * ${hiveDataFolder}
   * ${hiveOutputFolder}

     Definitionsfilen för arbetsflöde (workflow.xml i den här självstudiekursen) skickar dessa värden till skriptet HiveQL vid körning.
2. Spara filen som **C:\Tutorials\UseOozie\useooziewf.hql** med hjälp av kodningen ANSI (ASCII). (Använd anteckningar om din textredigerare inte ger det här alternativet.) Den här skriptfilen kommer att distribueras till HDInsight-kluster senare under kursen.

**Definiera ett arbetsflöde**

1. Skapa en textfil med följande innehåll:

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>

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
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
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
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
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

    Det finns två åtgärder som definierats i arbetsflödet. Start-till-åtgärden är *RunHiveScript*. Om instruktionen körs *OK*, är nästa åtgärd *RunSqoopExport*.

    RunHiveScript har flera variabler. Du skickar värden när du har skickat jobbet Oozie från din arbetsstation med hjälp av Azure PowerShell.

    Arbetsflödesvariabler

    <table border = "1">
    <tr><th>Arbetsflödesvariabler</th><th>Beskrivning</th></tr>
    <tr><td>${jobTracker}</td><td>Ange URL för Spårare för Hadoop-jobb. Använd <strong>jobtrackerhost:9010</strong> kluster i HDInsight version 3.0 och 2.0.</td></tr>
    <tr><td>${nameNode}</td><td>Ange URL: en för noden Hadoop namn. Använd standard filen system wasb: / / adress, till exempel <i>wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${Könamn}</td><td>Anger namnet på kön som jobbet ska skickas till. Använd <strong>standard</strong>.</td></tr>
    </table>

    Hive åtgärdsvariabler

    <table border = "1">
    <tr><th>Hive variabel</th><th>Beskrivning</th></tr>
    <tr><td>${hiveDataFolder}</td><td>Källkatalogen för Hive Create Table-kommando.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Den utgående mappen för instruktionen INSERT skriva över.</td></tr>
    <tr><td>${hiveTableName}</td><td>Namnet på Hive-tabell som refererar till log4j datafiler.</td></tr>
    </table>

    Sqoop åtgärdsvariabler

    <table border = "1">
    <tr><th>Sqoop variabel</th><th>Beskrivning</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>Anslutningssträngen som SQL-databas.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>Azure SQL-databastabell där data ska exporteras.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Den utgående mappen för instruktionen Hive Infoga skriva över. Det här är samma mapp för Sqoop exporten (export-dir).</td></tr>
    </table>

    Läs mer om Oozie arbetsflödet och använder arbetsflödesåtgärderna [Apache Oozie 4.0 dokumentationen] [ apache-oozie-400] (för HDInsight-kluster av version 3.0) eller [Apache Oozie 3.3.2 dokumentationen] [ apache-oozie-332] (för HDInsight-kluster av version 2.1).

1. Spara filen som **C:\Tutorials\UseOozie\workflow.xml** med hjälp av kodningen ANSI (ASCII). (Använd anteckningar om din textredigerare inte ger det här alternativet.)

**Definiera coordinator**

1. Skapa en textfil med följande innehåll:

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
            <workflow>
                <app-path>${wfPath}</app-path>
            </workflow>
        </action>
    </coordinator-app>
    ```

    Det finns fem variabler som används i definitionsfilen för:

   | Variabel | Beskrivning |
   | --- | --- |
   | ${coordFrequency} |Jobbet paus tid. Frekvensen är alltid uttrycks i minuter. |
   | ${coordStart} |Jobbets starttid. |
   | ${coordEnd} |Jobbets sluttid. |
   | ${coordTimezone} |Oozie bearbetar coordinator jobb i en fast tidszon med ingen sommartid (vanligtvis representeras med hjälp av UTC). Den här tidszonen kallas ”Oozie bearbetning tidszonen”. |
   | ${wfPath} |Sökvägen för workflow.xml.  Du måste ange om arbetsflödet filnamnet inte standardfilnamnet (workflow.xml). |
2. Spara filen som **C:\Tutorials\UseOozie\coordinator.xml** med hjälp av kodningen ANSI (ASCII). (Använd anteckningar om din textredigerare inte ger det här alternativet.)

## <a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Distribuera projektet Oozie och förbereda självstudier
Du kan köra en Azure PowerShell-skript för att utföra följande:

* Kopiera HiveQL-skript (useoozie.hql) till Azure Blob storage wasb:///tutorials/useoozie/useoozie.hql.
* Kopiera workflow.xml till wasb:///tutorials/useoozie/workflow.xml.
* Kopiera coordinator.xml till wasb:///tutorials/useoozie/coordinator.xml.
* Kopiera datafilen (/ example/data/sample.log) till wasb:///tutorials/useoozie/data/sample.log.
* Skapa en Azure SQL database-tabellen för att lagra Sqoop export av data. Tabellnamnet är *log4jLogCount*.

**Förstå HDInsight lagring**

HDInsight använder Azure Blob storage för lagring av data. wasb: / / är Microsofts implementering av Hadoop distributed file system (HDFS) i Azure Blob storage. Mer information finns i [använda Azure Blob storage med HDInsight][hdinsight-storage].

När du etablerar ett HDInsight-kluster används ett Azure Blob storage-konto och en specifik behållare från detta konto som standardfilsystem, som i HDFS. Förutom det här lagringskontot du kan lägga till ytterligare lagringskonton från samma Azure-prenumeration eller andra Azure-prenumerationer under etableringen. Anvisningar om att lägga till ytterligare lagringskonton finns [etablera HDInsight-kluster][hdinsight-provision]. För att förenkla Azure PowerShell-skript som används i den här självstudiekursen, alla filer som lagras i filen system standardbehållaren på */självstudier/useoozie*. Som standard har den här behållaren har samma namn som HDInsight-klustrets namn.
Syntax:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [!NOTE]
> Endast den *wasb: / /* syntax stöds i HDInsight-kluster av version 3.0. Den äldre *asv: / /* syntax stöds i HDInsight 2.1 och 1.6 kluster, men stöds inte i HDInsight 3.0-kluster.
>
> Wasb: / / sökvägen är en virtuell sökväg. Mer information finns i [använda Azure Blob storage med HDInsight][hdinsight-storage].

En fil som är lagrad i standardbehållaren filen system kan nås från HDInsight med någon av följande URI: er (jag använder workflow.xml som exempel):

    wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasb:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Om du vill komma åt filen direkt från lagringskontot är blob-namnet för filen:

    tutorials/useoozie/workflow.xml

**Förstå Hive interna och externa tabeller**

Det finns några saker du behöver veta om Hive interna och externa tabeller:

* CREATE TABLE-kommando skapar en intern tabell, även kallat en hanterad tabell. Filen måste finnas i standardbehållaren.
* Kommandot CREATE TABLE flyttar filen till/hive/datalager/<TableName> mapp i standardbehållaren.
* Skapa extern tabell-kommando skapar en extern tabell. Filen kan finnas utanför standardbehållaren.
* Skapa extern tabell kommando flyttar inte datafilen.
* Skapa extern tabell-kommandot kan inte eventuella undermappar på den mapp som har angetts i plats-satsen. Detta är orsaken till varför kursen skapar en kopia av filen sample.log.

Mer information finns i [HDInsight: Hive interna och externa tabeller introduktion][cindygross-hive-tables].

**Förbereda självstudier**

1. Öppna Windows PowerShell ISE (Skriv i startskärmen för Windows 8 **PowerShell_ISE**, och klicka sedan på **Windows PowerShell ISE**. Mer information finns i [starta Windows PowerShell i Windows 8 och Windows][powershell-start]).
2. Kör följande kommando för att ansluta till din Azure-prenumeration i det nedre fönstret:

    ```powershell
    Add-AzureAccount
    ```

    Du uppmanas att ange dina autentiseringsuppgifter för Azure-konto. Den här metoden för att lägga till en prenumerationsanslutning på grund av timeout och du behöver köra cmdleten igen efter 12 timmar.

   > [!NOTE]
   > Om du har flera Azure-prenumerationer och standard-prenumerationen är inte den du vill använda måste använda den <strong>Välj AzureSubscription</strong> för att välja en prenumeration.

3. Kopiera följande skript i skriptfönstret och ange de första sex variablerna:

    ```powershell
    # WASB variables
    $storageAccountName = "<StorageAccountName>"
    $containerName = "<BlobStorageContainerName>"

    # SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    # Oozie files for the tutorial
    $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
    $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
    $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

    # WASB folder for storing the Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use the long path here
    ```

    Beskrivningar av variabler finns i [krav](#prerequisites) avsnitt i den här självstudiekursen.

4. Lägg till följande skript i skriptfönstret:

    ```powershell
    # Create a storage context object
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

    function uploadOozieFiles()
    {
        Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
        Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
        Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
        Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
    }

    function prepareHiveDataFile()
    {
        Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
        Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
    }

    function prepareSQLDatabase()
    {
        # SQL query string for creating log4jLogsCount table
        $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [Level] [nvarchar](10) NOT NULL,
                [Total] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
            [Level] ASC
            )
            )"

        #Create the log4jLogsCount table
        Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $cmdCreateLog4jCountTable
        $cmd.executenonquery()

        $conn.close()
    }

    # upload workflow.xml, coordinator.xml, and ooziewf.hql
    uploadOozieFiles;

    # make a copy of example/data/sample.log to example/data/log4j/sample.log
    prepareHiveDataFile;

    # create log4jlogsCount table on SQL database
    prepareSQLDatabase;
    ```

5. Klicka på **kör skriptet** eller tryck på **F5** att köra skriptet. Utdata blir liknar:

    ![Förberedelse av kursen utdata][img-preparation-output]

## <a name="run-the-oozie-project"></a>Kör Oozie-projektet
Azure PowerShell tillhandahåller inte för närvarande alla cmdlets för att definiera Oozie jobb. Du kan använda den **Invoke-RestMethod** för att anropa Oozie-webbtjänster. Oozie web services API är en HTTP-REST-API för JSON. Mer information om webbtjänster Oozie API finns [Apache Oozie 4.0 dokumentationen] [ apache-oozie-400] (för HDInsight-kluster av version 3.0) eller [Apache Oozie 3.3.2 dokumentationen] [ apache-oozie-332] (för HDInsight-kluster av version 2.1).

**Att skicka ett Oozie-jobb**

1. Öppna Windows PowerShell ISE (i Windows 8 startskärmen, skriver **PowerShell_ISE**, och klicka sedan på **Windows PowerShell ISE**. Mer information finns i [starta Windows PowerShell i Windows 8 och Windows][powershell-start]).
2. Kopiera följande skript i skriptfönstret och ange sedan de först 14 variablerna (dock hoppa över **$storageUri**).

    ```powershell
    #HDInsight cluster variables
    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterUserPassword>"

    #Azure Blob storage (WASB) variables
    $storageAccountName = "<StorageAccountName>"
    $storageContainerName = "<BlobContainerName>"
    $storageUri="wasb://$storageContainerName@$storageAccountName.blob.core.windows.net"

    #Azure SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"

    #Oozie WF/coordinator variables
    $coordStart = "2014-03-21T13:45Z"
    $coordEnd = "2014-03-21T13:45Z"
    $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
    $coordTimezone = "UTC"    #UTC/GMT

    $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
    $waitTimeBetweenOozieJobStatusCheck=10

    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

    #Sqoop action variables
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"  
    $sqlDatabaseTableName = "log4jLogsCount"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)
    ```

    Beskrivningar av variabler finns i [krav](#prerequisites) avsnitt i den här självstudiekursen.

    $coordstart och $coordend är arbetsflödet start- och sluttid. Om du vill ta reda på UTC/GTM Sök ”utc-tid” på bing.com. $CoordFrequency är hur ofta i minuter som du vill köra arbetsflödet.
3. Lägg till följande i skriptet. Den här delen definierar nyttolasten Oozie:

    ```powershell
    #OoziePayload used for Oozie web service submission
    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
            <name>nameNode</name>
            <value>$storageUrI</value>
        </property>

        <property>
            <name>jobTracker</name>
            <value>jobtrackerhost:9010</value>
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
            <name>oozie.coord.application.path</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>wfPath</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>coordStart</name>
            <value>$coordStart</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>$coordEnd</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>$coordFrequency</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>$coordTimezone</value>
        </property>

        <property>
            <name>hiveScript</name>
            <value>$hiveScript</value>
        </property>

        <property>
            <name>hiveTableName</name>
            <value>$hiveTableName</value>
        </property>

        <property>
            <name>hiveDataFolder</name>
            <value>$hiveDataFolder</value>
        </property>

        <property>
            <name>hiveOutputFolder</name>
            <value>$hiveOutputFolder</value>
        </property>

        <property>
            <name>sqlDatabaseConnectionString</name>
            <value>&quot;$sqlDatabaseConnectionString&quot;</value>
        </property>

        <property>
            <name>sqlDatabaseTableName</name>
            <value>$SQLDatabaseTableName</value>
        </property>

        <property>
            <name>user.name</name>
            <value>admin</value>
        </property>

    </configuration>
    "@
    ```

   > [!NOTE]
   > Den största skillnaden jämfört med filen arbetsflöde skickas nyttolasten är variabeln **oozie.coord.application.path**. När du skickar ett arbetsflödesjobb, använder du **oozie.wf.application.path** i stället.

4. Lägg till följande i skriptet. Den här delen kontrollerar Oozie web service status:

    ```powershell
    function checkOozieServerStatus()
    {
        Write-Host "Checking Oozie server status..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieServerSatus = $jsonResponse[0].("systemMode")
        Write-Host "Oozie server status is $oozieServerSatus..."

        if($oozieServerSatus -notmatch "NORMAL")
        {
            Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
        }
    }
    ```

5. Lägg till följande i skriptet. Den här delen skapas ett jobb för Oozie:

    ```powershell
    function createOozieJob()
    {
        # create Oozie job
        Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
        Write-Host "`n--------`n$OoziePayload`n--------"
        $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieJobId = $jsonResponse[0].("id")
        Write-Host "Oozie job id is $oozieJobId..."

        return $oozieJobId
    }
    ```

   > [!NOTE]
   > När du skickar ett arbetsflödesjobb, måste du göra en annan webbtjänst-anrop för att starta jobbet när jobbet har skapats. I det här fallet utlöses coordinator jobb av tid. Jobbet startar automatiskt.

6. Lägg till följande i skriptet. Den här delen kontrollerar jobbstatusen Oozie:

    ```powershell
    function checkOozieJobStatus($oozieJobId)
    {
        # get job status
        Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

        Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
        $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")

        while($JobStatus -notmatch "SUCCEEDED|KILLED")
        {
            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")
        }

        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
        if($JobStatus -notmatch "SUCCEEDED")
        {
            Write-Host "Check logs at http://headnode0:9014/cluster for detais."
        }
    }
    ```

7. (Valfritt) Lägg till följande i skriptet.

    ```powershell
    function listOozieJobs()
    {
        Write-Host "Listing Oozie jobs..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

        write-host "Job ID                                   App Name        Status      Started                         Ended"
        write-host "----------------------------------------------------------------------------------------------------------------------------------"
        foreach($job in $response.workflows)
        {
            Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
        }
    }

    function ShowOozieJobLog($oozieJobId)
    {
        Write-Host "Showing Oozie job info..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
        write-host $response
    }

    function killOozieJob($oozieJobId)
    {
        Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
        $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
        $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
    }
    ```

8. Lägg till följande i skriptet:

    ```powershell
    checkOozieServerStatus
    # listOozieJobs
    $oozieJobId = createOozieJob($oozieJobId)
    checkOozieJobStatus($oozieJobId)
    # ShowOozieJobLog($oozieJobId)
    # killOozieJob($oozieJobId)
    ```

Ta bort #-tecken om du vill köra ytterligare funktioner.

9. Om ditt HDinsight-kluster är version 2.1, ersätter du ”https://$clusterName.azurehdinsight.net:443/oozie/v2/” med ”https://$clusterName.azurehdinsight.net:443/oozie/v1/”. HDInsight-kluster av version 2.1 stöder inte har stöd för version 2 av webbtjänsterna.
10. Klicka på **kör skriptet** eller tryck på **F5** att köra skriptet. Utdata blir liknar:

     ![Kursen kör arbetsflöde utdata][img-runworkflow-output]
11. Ansluta till SQL-databasen för att se exporterade data.

**Kontrollera i felloggen för jobb**

Om du vill felsöka ett arbetsflöde, finns Oozie loggfilen på C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log från headnode för klustret. Mer information om RDP finns [administrera HDInsight-kluster med hjälp av Azure portal][hdinsight-admin-portal].

**Att köra guiden**

Om du vill köra arbetsflödet, måste du utföra följande uppgifter:

* Ta bort utdatafilen Hive-skript.
* Ta bort data i tabellen log4jLogsCount.

Här är en Windows PowerShell-skript som du kan använda:

```powershell
$storageAccountName = "<AzureStorageAccountName>"
$containerName = "<ContainerName>"

#SQL database variables
$sqlDatabaseServer = "<SQLDatabaseServerName>"
$sqlDatabaseLogin = "<SQLDatabaseLoginName>"
$sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
$sqlDatabaseName = "<SQLDatabaseName>"
$sqlDatabaseTableName = "log4jLogsCount"

Write-host "Delete the Hive script output file ..." -ForegroundColor Green
$storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
$conn = New-Object System.Data.SqlClient.SqlConnection
$conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
$conn.open()
$cmd = New-Object System.Data.SqlClient.SqlCommand
$cmd.connection = $conn
$cmd.commandtext = "delete from $sqlDatabaseTableName"
$cmd.executenonquery()

$conn.close()
```

## <a name="next-steps"></a>Nästa steg
I kursen får du har lärt dig hur du definierar ett arbetsflöde för Oozie och Oozie-koordinator och hur du kör ett jobb för Oozie-koordinator med hjälp av Azure PowerShell. Mer information finns i följande artiklar:

* [Komma igång med HDInsight][hdinsight-get-started]
* [Använda Azure Blob storage med HDInsight][hdinsight-storage]
* [Administrera HDInsight med hjälp av Azure PowerShell][hdinsight-admin-powershell]
* [Överföra data till HDInsight][hdinsight-upload-data]
* [Använda Sqoop med HDInsight][hdinsight-use-sqoop]
* [Använda Hive med HDInsight][hdinsight-use-hive]
* [Använda Pig med HDInsight][hdinsight-use-pig]
* [Utveckla Java-MapReduce-program för HDInsight][hdinsight-develop-java-mapreduce]

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md

[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
