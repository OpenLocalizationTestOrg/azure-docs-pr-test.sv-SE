---
title: aaaUse tidsbaserade Hadoop Oozie-koordinator i HDInsight | Microsoft Docs
description: "Använd tidsbaserade Hadoop Oozie-koordinator i HDInsight, en stordatatjänst. Lär dig hur toodefine Oozie arbetsflöden och koordinatorer, och skicka jobb."
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
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: aecbb5ee94a4234d1a7768bdb6de2a33508b1e4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-toodefine-workflows-and-coordinate-jobs"></a>Använda en tidsbaserad Oozie-koordinator med Hadoop i HDInsight toodefine arbetsflöden och samordna jobb
I den här artikeln lär du dig hur toodefine arbetsflöden och koordinatorer och hur tootrigger hello coordinator jobb, baserat på tid. Det är bra toogo via [Använd Oozie med HDInsight] [ hdinsight-use-oozie] innan den här artikeln. Dessutom tooOozie, du kan också schemalägga jobb med hjälp av Azure Data Factory. toolearn Azure Data Factory finns [Use Pig och Hive med Data Factory](../data-factory/data-factory-data-transformation-activities.md).

> [!NOTE]
> Den här artikeln kräver ett Windows-baserade HDInsight-kluster. Information om hur du använder Oozie, inklusive tidsbaserade jobb på en Linux-baserade kluster finns [Använd Oozie med Hadoop toodefine och kör ett arbetsflöde på Linux-baserat HDInsight](hdinsight-use-oozie-linux-mac.md)

## <a name="what-is-oozie"></a>Vad är Oozie
Apache Oozie är ett arbetsflöde/samordning system som hanterar Hadoop-jobb. Det är integrerat med hello Hadoop-stacken och stöder Hadoop-jobb för Apache MapReduce, Apache Pig, Apache Hive och Apache Sqoop. Det kan också vara används tooschedule jobb som är specifika tooa system, till exempel Java-program eller kommandoskript.

hello visar följande bild hello arbetsflöde som du kommer att implementera:

![Arbetsflödesdiagram][img-workflow-diagram]

hello arbetsflödet innehåller två åtgärder:

1. En Hive-åtgärden körs en HiveQL-skript toocount hello förekomster av varje loggningsnivån typ i en loggfil för log4j. Varje logg log4j består av en rad med fält som innehåller en [LOGGNINGSNIVÅ] fältet tooshow hello typ och hello allvarlighetsgrad, till exempel:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    hello utdata för Hive-skriptet är ungefär:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Mer information om Hive finns i [Använda Hive med HDInsight][hdinsight-use-hive].
2. En åtgärd för Sqoop exporterar hello HiveQL åtgärd tooa utdatatabellen i en Azure SQL database. Läs mer om Sqoop [använda Sqoop med HDInsight][hdinsight-use-sqoop].

> [!NOTE]
> Versioner som stöds Oozie i HDInsight-kluster, se [vad är nytt i hello-klusterversioner som tillhandahålls av HDInsight?] [hdinsight-versions].
>
>

## <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du ha hello följande:

* **En arbetsstation med Azure PowerShell**.

    > [!IMPORTANT]
    > Stödet för Azure PowerShell för hantering av HDInsight-resurser med hjälp av Azure Service Manager är **föråldrat** och dras tillbaka den 1 januari 2017. hello stegen i det här dokumentet används hello nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.
    >
    > Följ stegen hello i [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello senaste versionen av Azure PowerShell. Om du har skript som behöver toobe ändras toouse hello nya cmdletarna som fungerar med Azure Resource Manager kan se [migrera tooAzure Resource Manager-baserade development tools för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md) för mer information.

* **Ett HDInsight-kluster**. Information om hur du skapar ett HDInsight-kluster finns i [skapa HDInsight-kluster][hdinsight-provision], eller [komma igång med HDInsight][hdinsight-get-started]. Du behöver följande data toogo igenom kursen hello hello:

    <table border = "1">
    <tr><th>Kluster-egenskap</th><th>Variabelnamn för Windows PowerShell</th><th>Värde</th><th>Beskrivning</th></tr>
    <tr><td>HDInsight-klustrets namn</td><td>$clusterName</td><td></td><td>Hej HDInsight-kluster som du vill köra den här kursen.</td></tr>
    <tr><td>Användarnamn för HDInsight-kluster</td><td>$clusterUsername</td><td></td><td>användarnamn för hello HDInsight-kluster. </td></tr>
    <tr><td>Användarlösenordet för HDInsight-kluster </td><td>$clusterPassword</td><td></td><td>användarlösenord hello HDInsight-kluster.</td></tr>
    <tr><td>Azure storage-kontonamn</td><td>$storageAccountName</td><td></td><td>Ett Azure Storage-konto tillgängliga toohello HDInsight-kluster. Använd hello standardkontot för lagring som du angav under hello klustret etablera processen för den här självstudiekursen.</td></tr>
    <tr><td>Azure Blob-behållarnamn</td><td>$containerName</td><td></td><td>I det här exemplet använder du hello Azure Blob storage-behållare som används för hello HDInsight-kluster standardfilsystem. Som standard har hello samma namn som hello HDInsight-kluster.</td></tr>
    </table>
* **En Azure SQL database**. Du måste konfigurera en brandväggsregel för hello SQL Database server tooallow åtkomst från din arbetsstation. Instruktioner om hur du skapar en Azure SQL database och konfigurerar hello brandväggen finns [komma igång med Azure SQL database] [sqldatabase-get-started]. Den här artikeln innehåller en Windows PowerShell-skript för att skapa hello Azure SQL database-tabellen som ska användas för den här kursen.

    <table border = "1">
    <tr><th>Egenskapen för SQL-databas</th><th>Variabelnamn för Windows PowerShell</th><th>Värde</th><th>Beskrivning</th></tr>
    <tr><td>SQL server-databasnamn</td><td>$sqlDatabaseServer</td><td></td><td>hello SQL database server toowhich Sqoop kommer att exportera data. </td></tr>
    <tr><td>Inloggningsnamn för SQL-databas</td><td>$sqlDatabaseLogin</td><td></td><td>Inloggningsnamn för SQL-databas.</td></tr>
    <tr><td>Inloggningslösenordet för SQL-databas</td><td>$sqlDatabaseLoginPassword</td><td></td><td>Lösenordet för inloggningen för SQL-databas.</td></tr>
    <tr><td>SQL-databasnamn</td><td>$sqlDatabaseName</td><td></td><td>hello Azure SQL database toowhich Sqoop kommer att exportera data. </td></tr>
    </table>

  > [!NOTE]
  > Som standard tillåter en Azure SQL database anslutningar från Azure-tjänster, till exempel Azure HDInsight. Om brandväggsinställningen är inaktiverad måste du aktivera det från hello Azure-portalen. Instruktioner om hur du skapar en SQL-databas och konfigurera brandväggsregler, se [skapa och konfigurera SQL-databas][sqldatabase-get-started].

> [!NOTE]
> Fyll hello värden i hello tabeller. Kommer att vara till hjälp för att gå igenom den här kursen.

## <a name="define-oozie-workflow-and-hello-related-hiveql-script"></a>Definiera Oozie arbetsflödet och hello relaterade HiveQL-skript
Oozie arbetsflöden definitioner skrivs i hPDL (en XML-processen definition language). hello standardfilnamnet för arbetsflödet är *workflow.xml*.  Du spara hello arbetsflödet filen lokalt och sedan distribuera den toohello HDInsight-kluster med hjälp av Azure PowerShell senare i den här kursen.

hello Hive-åtgärden i hello arbetsflödet anropar en skriptfil för HiveQL. Den här skriptfilen innehåller tre HiveQL-instruktioner:

1. **hello DROP TABLE-instruktionen** borttagningar hello log4j Hive-tabell om den finns.
2. **hello CREATE TABLE-instruktion** skapar en log4j Hive extern tabell som pekar toohello plats för loggfilen för hello log4j;
3. **Hej plats för hello log4j loggfilen**. hello fältavgränsaren är ””,. hello standard rad avgränsaren är ”\n”. En extern tabell Hive är används tooavoid hello datafil som tas bort från hello ursprungliga platsen, om du vill toorun hello Oozie arbetsflöde flera gånger.
4. **hello Infoga över instruktionen** räknar hello förekomster av varje loggningsnivån typ från hello log4j Hive-tabell och sparar den hello tooan Azure Blob storage platsen.

> [!NOTE]
> Det finns ett känt problem i Hive-sökväg. När du skickar in ett Oozie-jobb körs i det här problemet. Hej instruktioner för att åtgärda hello problem kan hittas på hello TechNet Wiki: [HDInsight Hive-fel: Det gick inte toorename][technetwiki-hive-error].

**toodefine hello HiveQL-skript filen toobe anropas av hello-arbetsflöde**

1. Skapa en textfil med hello följande innehåll:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Det finns tre variabler i hello skript:

   * ${hiveTableName}
   * ${hiveDataFolder}
   * ${hiveOutputFolder}

     arbetsflöde för hello-definitionsfil (workflow.xml i den här självstudiekursen) skickar dessa värden toothis HiveQL-skript vid körning.
2. Spara hello som **C:\Tutorials\UseOozie\useooziewf.hql** med hjälp av kodningen ANSI (ASCII). (Använd anteckningar om din textredigerare inte ger det här alternativet.) Den här skriptfilen blir senare i självstudiekursen hello distribuerade toohello HDInsight-kluster.

**toodefine ett arbetsflöde**

1. Skapa en textfil med hello följande innehåll:

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

    Det finns två åtgärder som definierats i hello arbetsflöde. hello start-tooaction är *RunHiveScript*. Om hello-åtgärden körs *OK*, hello nästa åtgärd *RunSqoopExport*.

    Hej RunHiveScript har flera variabler. Hello-värden skickas när du skickar hello Oozie jobb från din arbetsstation med hjälp av Azure PowerShell.

    Arbetsflödesvariabler

    <table border = "1">
    <tr><th>Arbetsflödesvariabler</th><th>Beskrivning</th></tr>
    <tr><td>${jobTracker}</td><td>Ange hello URL för Spårare för hello Hadoop-jobb. Använd <strong>jobtrackerhost:9010</strong> kluster i HDInsight version 3.0 och 2.0.</td></tr>
    <tr><td>${nameNode}</td><td>Ange hello URL för hello Hadoop namn nod. Använd hello standard filen system wasb: / / adress, till exempel <i>wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${Könamn}</td><td>Anger hello könamnet som hello jobbet ska skickas till. Använd <strong>standard</strong>.</td></tr>
    </table>

    Hive åtgärdsvariabler

    <table border = "1">
    <tr><th>Hive variabel</th><th>Beskrivning</th></tr>
    <tr><td>${hiveDataFolder}</td><td>hello källkatalogen för hello Hive Create Table-kommando.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>hello utdatamapp för hello Infoga över instruktionen.</td></tr>
    <tr><td>${hiveTableName}</td><td>hello namnet på hello Hive-tabell som refererar till hello log4j datafiler.</td></tr>
    </table>

    Sqoop åtgärdsvariabler

    <table border = "1">
    <tr><th>Sqoop variabel</th><th>Beskrivning</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>Anslutningssträngen som SQL-databas.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>hello Azure SQL database tabell toowhere hello data kommer att exporteras.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>hello utdatamapp för hello Hive Infoga skriva över instruktionen. Detta är hello samma mapp för hello Sqoop export (export-dir).</td></tr>
    </table>

    Läs mer om Oozie arbetsflödet och använder hello arbetsflödesåtgärder [Apache Oozie 4.0 dokumentationen] [ apache-oozie-400] (för HDInsight-kluster av version 3.0) eller [Apache Oozie 3.3.2 dokumentationen] [ apache-oozie-332] (för HDInsight-kluster av version 2.1).

1. Spara hello som **C:\Tutorials\UseOozie\workflow.xml** med hjälp av kodningen ANSI (ASCII). (Använd anteckningar om din textredigerare inte ger det här alternativet.)

**toodefine coordinator**

1. Skapa en textfil med hello följande innehåll:

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
            <workflow>
                <app-path>${wfPath}</app-path>
            </workflow>
        </action>
    </coordinator-app>
    ```

    Det finns fem variabler som används i definitionsfilen för hello:

   | Variabel | Beskrivning |
   | --- | --- |
   | ${coordFrequency} |Jobbet paus tid. Frekvensen är alltid uttrycks i minuter. |
   | ${coordStart} |Jobbets starttid. |
   | ${coordEnd} |Jobbets sluttid. |
   | ${coordTimezone} |Oozie bearbetar coordinator jobb i en fast tidszon med ingen sommartid (vanligtvis representeras med hjälp av UTC). Den här tidszonen kallas hello ”Oozie bearbetning tidszonen”. |
   | ${wfPath} |hello sökvägen för hello workflow.xml.  Du måste ange om hello Arbetsflödesnamnet filen inte är hello standardfilnamnet (workflow.xml). |
2. Spara hello som **C:\Tutorials\UseOozie\coordinator.xml** med hjälp av hello ANSI (ASCII) kodning. (Använd anteckningar om din textredigerare inte ger det här alternativet.)

## <a name="deploy-hello-oozie-project-and-prepare-hello-tutorial"></a>Distribuera hello Oozie-projektet och förbereda hello självstudiekursen
Du ska köra en Azure PowerShell-skriptet tooperform hello följande:

* Kopiera hello HiveQL-skript (useoozie.hql) tooAzure Blob storage, wasb:///tutorials/useoozie/useoozie.hql.
* Kopiera workflow.xml toowasb:///tutorials/useoozie/workflow.xml.
* Kopiera coordinator.xml toowasb:///tutorials/useoozie/coordinator.xml.
* Kopiera hello-datafil (/ example/data/sample.log) toowasb:///tutorials/useoozie/data/sample.log.
* Skapa en Azure SQL database-tabellen för att lagra Sqoop export av data. hello tabellnamnet är *log4jLogCount*.

**Förstå HDInsight lagring**

HDInsight använder Azure Blob storage för lagring av data. wasb: / / är Microsofts implementering av hello Hadoop distributed file system (HDFS) i Azure Blob storage. Mer information finns i [använda Azure Blob storage med HDInsight][hdinsight-storage].

När du etablerar ett HDInsight-kluster, utses ett Azure Blob storage-konto och en specifik behållare från detta konto hello standardfilsystem, som i HDFS. Dessutom toothis storage-konto, du kan lägga till ytterligare lagringskonton från hello samma Azure-prenumeration eller från olika Azure-prenumerationer under hello etableringsprocessen. Anvisningar om att lägga till ytterligare lagringskonton finns [etablera HDInsight-kluster][hdinsight-provision]. toosimplify hello Azure PowerShell-skriptet som används i den här självstudiekursen, alla hello filer lagras i hello standard filsystemets behållare finns på */självstudier/useoozie*. Den här behållaren har som standard hello samma namn som hello HDInsight-klustrets namn.
hello-syntaxen är:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [!NOTE]
> Endast hello *wasb: / /* syntax stöds i HDInsight-kluster av version 3.0. hello äldre *asv: / /* syntax stöds i HDInsight 2.1 och 1.6 kluster, men stöds inte i HDInsight 3.0-kluster.
>
> Hej wasb: / / sökvägen är en virtuell sökväg. Mer information finns i [använda Azure Blob storage med HDInsight][hdinsight-storage].

En fil som lagras i hello standard filsystemets behållare kan nås från HDInsight med hjälp av hello följande URI: er (jag använder workflow.xml som exempel):

    wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasb:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Om du vill tooaccess hello filen direkt från hello storage-konto är hello blob-namnet för hello-filen:

    tutorials/useoozie/workflow.xml

**Förstå Hive interna och externa tabeller**

Det finns några saker du behöver tooknow om Hive interna och externa tabeller:

* hello CREATE TABLE-kommando skapar en intern tabell, även kallat en hanterad tabell. hello datafilen måste finnas i hello standardbehållaren.
* hello CREATE TABLE-kommando flyttar hello data filen toohello/hive-datalager<TableName> mapp i hello standardbehållaren.
* hello Skapa extern tabell kommando skapar en extern tabell. hello-datafilen kan finnas utanför hello standardbehållaren.
* hello Skapa extern tabell kommando flyttar inte hello-datafilen.
* hello Skapa extern tabell kommandot kan inte eventuella undermappar på hello-mappen som anges i hello plats-satsen. Detta är hello skäl till varför hello självstudiekursen gör en kopia av hello sample.log filen.

Mer information finns i [HDInsight: Hive interna och externa tabeller introduktion][cindygross-hive-tables].

**tooprepare hello självstudiekursen**

1. Öppna hello Windows PowerShell ISE (hello Start i Windows 8 skärm anger **PowerShell_ISE**, och klicka sedan på **Windows PowerShell ISE**. Mer information finns i [starta Windows PowerShell i Windows 8 och Windows][powershell-start]).
2. Kör följande kommando tooconnect tooyour Azure-prenumeration hello i hello längst ned i fönstret:

    ```powershell
    Add-AzureAccount
    ```

    Du kommer att tillfrågas tooenter dina Azure-autentiseringsuppgifter. Den här metoden för att lägga till en prenumerationsanslutning timeout och efter 12 timmar du har toorun hello cmdlet igen.

   > [!NOTE]
   > Om du har flera Azure-prenumerationer och hello standard prenumerationen är inte hello en du vill toouse använder hello <strong>Välj AzureSubscription</strong> cmdlet tooselect en prenumeration.

3. Kopiera följande skript i hello Skriptfönster hello och ange hello första sex variabler:

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

    # Oozie files for hello tutorial
    $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
    $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
    $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

    # WASB folder for storing hello Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use hello long path here
    ```

    Fler beskrivningar av hello variabler finns i hello [krav](#prerequisites) avsnitt i den här självstudiekursen.

4. Lägg till följande toohello skriptet i hello Skriptfönster hello:

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
        Write-Host "Make a copy of hello sample.log file ... " -ForegroundColor Green
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

        #Create hello log4jLogsCount table
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

    # make a copy of example/data/sample.log tooexample/data/log4j/sample.log
    prepareHiveDataFile;

    # create log4jlogsCount table on SQL database
    prepareSQLDatabase;
    ```

5. Klicka på **kör skriptet** eller tryck på **F5** toorun hello skript. hello utdata blir liknar:

    ![Förberedelse av kursen utdata][img-preparation-output]

## <a name="run-hello-oozie-project"></a>Kör hello Oozie-projekt
Azure PowerShell tillhandahåller inte för närvarande alla cmdlets för att definiera Oozie jobb. Du kan använda hello **Invoke-RestMethod** cmdlet tooinvoke Oozie-webbtjänster. hello Oozie web services API är en HTTP-REST-API för JSON. Mer information om webbtjänster för hello Oozie API finns [Apache Oozie 4.0 dokumentationen] [ apache-oozie-400] (för HDInsight-kluster av version 3.0) eller [Apache Oozie 3.3.2 dokumentationen] [ apache-oozie-332] (för HDInsight-kluster av version 2.1).

**toosubmit ett Oozie-jobb**

1. Öppna hello Windows PowerShell ISE (i Windows 8 startskärmen, skriver **PowerShell_ISE**, och klicka sedan på **Windows PowerShell ISE**. Mer information finns i [starta Windows PowerShell i Windows 8 och Windows][powershell-start]).
2. Kopiera hello följande skript i hello Skriptfönster och sedan ange hello först 14 variabler (dock hoppa över **$storageUri**).

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

    $oozieWFPath="$storageUri/tutorials/useoozie"  # hello default name is workflow.xml. And you don't need toospecify hello file name.
    $waitTimeBetweenOozieJobStatusCheck=10

    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
    $sqlDatabaseTableName = "log4jLogsCount"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)
    ```

    Fler beskrivningar av hello variabler finns i hello [krav](#prerequisites) avsnitt i den här självstudiekursen.

    $coordstart och $coordend är hello arbetsflödet start- och sluttid. toofind Sök ”utc-tid” på bing.com hello UTC/GMT tid. Hej $coordFrequency är hur ofta i minuter som du vill toorun hello arbetsflödet.
3. Lägg till hello följande toohello skript. Den här delen definierar hello Oozie nyttolast:

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
   > hello största skillnaden jämfört med toohello skicka nyttolast-arbetsflödesfil är hello variabel **oozie.coord.application.path**. När du skickar ett arbetsflödesjobb, använder du **oozie.wf.application.path** i stället.

4. Lägg till hello följande toohello skript. Den här delen kontrollerar hello Oozie web Servicestatus:

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
            Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check hello server status and re-run hello job."
            exit 1
        }
    }
    ```

5. Lägg till hello följande toohello skript. Den här delen skapas ett jobb för Oozie:

    ```powershell
    function createOozieJob()
    {
        # create Oozie job
        Write-Host "Sending hello following Payload toohello cluster:" -ForegroundColor Green
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
   > När du skickar ett arbetsflödesjobb måste du göra en annan web service anropet toostart hello jobbet när hello jobbet har skapats. I det här fallet utlöses hello coordinator jobb av tid. hello jobbet startar automatiskt.

6. Lägg till hello följande toohello skript. Den här delen kontrollerar hello Oozie jobbstatus:

    ```powershell
    function checkOozieJobStatus($oozieJobId)
    {
        # get job status
        Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until hello job metadata is populated in hello Oozie metastore..." -ForegroundColor Green
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

        Write-Host "Getting job status and waiting for hello job toocomplete..." -ForegroundColor Green
        $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")

        while($JobStatus -notmatch "SUCCEEDED|KILLED")
        {
            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for hello job toocomplete..."
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")
        }

        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
        if($JobStatus -notmatch "SUCCEEDED")
        {
            Write-Host "Check logs at http://headnode0:9014/cluster for detais."
            exit -1
        }
    }
    ```

7. (Valfritt) Lägg till hello följande toohello skript.

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
        Write-Host "Killing hello Oozie job $oozieJobId..." -ForegroundColor Green
        $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for hello 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
        $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
    }
    ```

8. Lägg till följande toohello skript hello:

    ```powershell
    checkOozieServerStatus
    # listOozieJobs
    $oozieJobId = createOozieJob($oozieJobId)
    checkOozieJobStatus($oozieJobId)
    # ShowOozieJobLog($oozieJobId)
    # killOozieJob($oozieJobId)
    ```

    Ta bort hello #-tecken om du vill toorun hello ytterligare funktioner.
9. Om ditt HDinsight-kluster är version 2.1, ersätter du ”https://$clusterName.azurehdinsight.net:443/oozie/v2/” med ”https://$clusterName.azurehdinsight.net:443/oozie/v1/”. HDInsight-kluster av version 2.1 stöder inte har stöd för version 2 av hello-webbtjänster.
10. Klicka på **kör skriptet** eller tryck på **F5** toorun hello skript. hello utdata blir liknar:

     ![Kursen kör arbetsflöde utdata][img-runworkflow-output]
11. Ansluta tooyour SQL-databas toosee hello exporterade data.

**toocheck hello jobbet felloggen**

tootroubleshoot ett arbetsflöde hello Oozie loggfilen finns på C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log från hello klustret headnode. Mer information om RDP finns [administrera HDInsight-kluster med hello Azure-portalen][hdinsight-admin-portal].

**toorerun hello självstudiekursen**

toorerun hello arbetsflödet, måste du utföra hello följande uppgifter:

* Ta bort utdatafilen för hello Hive-skript.
* Ta bort hello data i hello log4jLogsCount tabell.

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

Write-host "Delete hello Hive script output file ..." -ForegroundColor Green
$storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

Write-host "Delete all hello records from hello log4jLogsCount table ..." -ForegroundColor Green
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
I kursen får du lära sig hur toodefine ett arbetsflöde för Oozie och Oozie-koordinator och hur toorun Oozie-koordinator jobb med hjälp av Azure PowerShell. toolearn se fler hello följande artiklar:

* [Komma igång med HDInsight][hdinsight-get-started]
* [Använda Azure Blob storage med HDInsight][hdinsight-storage]
* [Administrera HDInsight med hjälp av Azure PowerShell][hdinsight-admin-powershell]
* [Ladda upp data tooHDInsight][hdinsight-upload-data]
* [Använda Sqoop med HDInsight][hdinsight-use-sqoop]
* [Använda Hive med HDInsight][hdinsight-use-hive]
* [Använda Pig med HDInsight][hdinsight-use-pig]
* [Utveckla Java-MapReduce-program för HDInsight][hdinsight-develop-java-mapreduce]

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
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
