---
title: "aaaRun ett Hadoop-jobb med hjälp av Azure Cosmos DB och HDInsight | Microsoft Docs"
description: "Lär dig hur toorun enkel Hive, Pig och MapReduce-jobb med Azure Cosmos DB och Azure HDInsight."
services: cosmos-db
author: dennyglee
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 06f0ea9d-07cb-4593-a9c5-ab912b62ac42
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 06/08/2017
ms.author: denlee
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2e27499f2c4ba951af9a1ade1bcc9c1b6d298fcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="Azure Cosmos DB-HDInsight"></a>Kör ett Apache Hive, Pig eller Hadoop-jobb med hjälp av Azure Cosmos DB och HDInsight
De här självstudierna visar hur toorun [Apache Hive][apache-hive], [Apache Pig][apache-pig], och [Apache Hadoop] [ apache-hadoop] MapReduce-jobb på Azure HDInsight med Cosmos DB Hadoop-anslutningen. Cosmos DB'S Hadoop-anslutningen kan Cosmos DB tooact som både källa och mottagare för Hive, Pig och MapReduce-jobb. Den här kursen använder Cosmos DB som både hello källa och mål för Hadoop-jobb.

Den här kursen att du kunna tooanswer hello följande frågor:

* Hur jag för att läsa in data från Cosmos-databasen med ett Hive, Pig eller MapReduce-jobb?
* Hur lagrar data i Cosmos-databasen med ett Hive, Pig eller MapReduce-jobb?

Vi rekommenderar att komma igång genom att titta på hello följande video, där vi kör via ett Hive-jobb med hjälp av Cosmos-DB och HDInsight.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Use-Azure-DocumentDB-Hadoop-Connector-with-Azure-HDInsight/player]
>
>

Gå sedan tillbaka toothis artikel, där du får hello fullständig information om hur du kan köra analytics-jobb på Cosmos-DB-data.

> [!TIP]
> Den här kursen förutsätter att du har tidigare erfarenhet av Apache Hadoop, Hive och Pig. Om du är ny tooApache Hadoop Hive och Pig, rekommenderar vi att besöka hello [Apache Hadoop-dokumentation][apache-hadoop-doc]. Den här kursen förutsätter också att du har tidigare erfarenhet av Cosmos-DB och har en Cosmos-DB-konto. Om du är ny tooCosmos DB eller du har inte ett Cosmos-DB-konto, Läs vår [komma igång] [ getting-started] sidan.
>
>

Har du inte tid toocomplete hello självstudier och bara vill tooget hello fullständigt exempelprogram PowerShell-skript för Hive, Pig och MapReduce? Inga problem, hämta dem [här][hdinsight-samples]. hello download innehåller också hello hql, pig och java-filer för exemplen.

## <a name="NewestVersion"></a>Senaste versionen
<table border='1'>
    <tr><th>Hadoop Connector-Version</th>
        <td>1.2.0</td></tr>
    <tr><th>Skript-Uri</th>
        <td>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</td></tr>
    <tr><th>Ändrad den</th>
        <td>04/26/2016</td></tr>
    <tr><th>Stöds HDInsight-versioner</th>
        <td>3.1, 3.2</td></tr>
    <tr><th>Ändringslogg</th>
        <td>Uppdatera Azure Cosmos DB Java SDK too1.6.0</br>
            Tillagt stöd för partitionerade samlingar som både källa och mottagare</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Förhandskrav
Se till att du har hello följande innan du följer hello instruktioner i den här självstudiekursen:

* Ett Cosmos-DB-konto, en databas och en samling med dokument i. Mer information finns i [komma igång med Cosmos DB][getting-started]. Importera exempeldata Cosmos-DB-konto med hello [Cosmos-DB-Importverktyget][import-data].
* Genomflöde. Läser och skriver från HDInsight räknas mot dina angivna frågeenheter för samlingar.
* Kapacitet för ytterligare en lagrad procedur inom varje utdata samling. hello lagrade procedurer används för att överföra resulterande dokument.
* Kapacitet för hello resulterande dokument från hello Hive, Pig eller MapReduce-jobb.
* [*Valfritt*] kapacitet för en ytterligare samling.

> [!WARNING]
> Ordning tooavoid hello att skapa en ny samling under någon av hello jobb, du antingen skriva ut hello resultat toostdout, spara hello utdata tooyour WASB behållare eller ange en redan befintlig samling. Hello skiftläget för att ange en befintlig samling, nya dokument kommer att skapas i hello samlingen och redan befintliga dokument påverkas endast om det finns en konflikt i *ID*. **hello-anslutningen automatiskt skriver över befintliga dokument med id-konflikter**. Du kan stänga av den här funktionen genom att ange hello upsert alternativet toofalse. Om upsert är false och en konflikt uppstår, hello Hadoop-jobb misslyckas; ett id konflikt fel rapporteras.
>
>

## <a name="ProvisionHDInsight"></a>Steg 1: Skapa ett nytt HDInsight-kluster
Den här kursen använder skriptåtgärd från hello Azure Portal toocustomize ditt HDInsight-kluster. I den här kursen ska vi använda hello Azure Portal toocreate ditt HDInsight-kluster. Anvisningar för hur toouse PowerShell-cmdletar eller hello HDInsight .NET SDK checka ut den [anpassa HDInsight-kluster med skriptåtgärder] [ hdinsight-custom-provision] artikel.

1. Logga in toohello [Azure Portal][azure-portal].
2. Klicka på **+ ny** på hello överkant hello vänster navigeringsfält, söka efter **HDInsight** i hello övre sökfältet på hello nytt blad.
3. **HDInsight** publiceras av **Microsoft** visas överst hello hello resultat. Klicka på den och klicka sedan på **skapa**.
4. Skapa bladet på hello nya HDInsight-kluster, ange din **klusternamnet** och välj hello **prenumeration** önskade tooprovision resursen under.

    <table border='1'>
        <tr><td>Klusternamn</td><td>Namnet hello-kluster.<br/>
DNS-namn måste börja och sluta med ett alfanumeriska tecken och får innehålla tankstreck.<br/>
hello-fältet måste vara en sträng mellan 3 och 63 tecken.</td></tr>
        <tr><td>Prenumerationsnamn</td>
            <td>Om du har mer än en Azure-prenumeration, Välj hello-prenumeration som är värd för ditt HDInsight-kluster. </td></tr>
    </table>
5.Klicka på **Välj typ av kluster** och ange hello följande egenskaper toohello angivna värden.

    <table border='1'>
        <tr><td>Klustertyp</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Kluster-nivå</td><td><strong>Standard</strong></td></tr>
        <tr><td>Operativsystem</td><td><strong>Windows</strong></td></tr>
        <tr><td>Version</td><td>senaste versionen</td></tr>
    </table>

    Klicka på **Välj**.

    ![Ange Hadoop HDInsight inledande klusterinformation][image-customprovision-page1]
6. Klicka på **autentiseringsuppgifter** tooset dina autentiseringsuppgifter för inloggning och fjärråtkomst. Välj din **kluster inloggning användarnamn** och **kluster inloggningslösenordet**.

    Om du vill tooremote i klustret, väljer *Ja* längst hello hello bladet och ange ett användarnamn och lösenord.
7. Klicka på **datakällan** tooset åtkomst till den primära platsen för data. Välj hello **urvalsmetod** och ange ett redan befintligt lagringskonto eller skapa en ny.
8. På Hej samma bladet, ange en **standard behållaren** och en **plats**. Klicka på **Välj**.

   > [!NOTE]
   > Välj en plats Stäng tooyour Cosmos DB konto region för bättre prestanda
   >
   >
9. Klicka på **priser** tooselect hello antal och typer av noder. Du kan behålla hello standardkonfigurationen och skala hello antalet arbetarnoder vid ett senare tillfälle.
10. Klicka på **valfri konfiguration**, sedan **skriptåtgärder** i hello valfri konfiguration bladet.

     I skriptåtgärder, anger du följande information toocustomize hello ditt HDInsight-kluster.

     <table border='1'>
         <tr><th>Egenskap</th><th>Värde</th></tr>
         <tr><td>Namn</td>
             <td>Ange ett namn för hello skriptåtgärder.</td></tr>
         <tr><td>Skript-URI</td>
             <td>Ange hello URI toohello skript som är anropade toocustomize hello-kluster.</br></br>
Ange: </br> <strong>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</strong>.</td></tr>
         <tr><td>Huvud</td>
             <td>Klicka på hello kryssrutan toorun hello PowerShell-skript till hello huvudnod.</br></br>
             <strong>Den här kryssrutan</strong>.</td></tr>
         <tr><td>Underordnad</td>
             <td>Klicka på hello kryssrutan toorun hello PowerShell-skript till hello arbetsnoden.</br></br>
             <strong>Den här kryssrutan</strong>.</td></tr>
         <tr><td>Zookeeper</td>
             <td>Klicka på hello kryssrutan toorun hello PowerShell-skript till hello Zookeeper.</br></br>
             <strong>Behövs inte</strong>.
             </td></tr>
         <tr><td>Parametrar</td>
             <td>Ange hello parametrar, om det krävs av hello skript.</br></br>
             <strong>Inga parametrar som behövs för</strong>.</td></tr>
     </table>
11.Skapa en ny **resursgruppen** eller Använd en befintlig resursgrupp under Azure-prenumeration.
12. Kontrollera nu **PIN-kod toodashboard** tootrack dess distribution och klickar på **skapa**!

## <a name="InstallCmdlets"></a>Steg 2: Installera och konfigurera Azure PowerShell
1. Installera Azure PowerShell. Anvisningar finns [här][powershell-install-configure].

   > [!NOTE]
   > Du kan också använda Hdinsight's online Hive-Redigeraren för Hive-frågor. toodo så logga in toohello [Azure Portal][azure-portal], klickar du på **HDInsight** på hello vänster tooview en lista över dina HDInsight-kluster. Klicka på hello kluster du vill använda toorun Hive-frågor på och klicka sedan på **frågan konsolen**.
   >
   >
2. Öppna hello Azure PowerShell Integrated Scripting Environment:

   * På en dator som kör Windows 8 eller Windows Server 2012 eller senare, kan du använda hello inbyggda sökning. Skriv från startskärmen hello **powershell ise** och på **RETUR**.
   * Använd hello Start-menyn på en dator som kör en version tidigare än Windows 8 eller Windows Server 2012. Hello Start-menyn, Skriv **kommandotolk** hello sökrutan och sedan i resultatlista hello, klickar du på **kommandotolk**. I hello kommandotolk, skriver **powershell_ise** och på **RETUR**.
3. Lägg till ditt Azure-konto.

   1. Skriv i hello konsolfönstret, **Add-AzureAccount** och på **RETUR**.
   2. Ange hello e-postadress som är associerad med din Azure-prenumeration och klicka på **Fortsätt**.
   3. Ange hello lösenord för din Azure-prenumeration.
   4. Klicka på **logga in**.
4. följande diagram hello identifierar hello viktiga delar i datormiljön Azure PowerShell-skript.

    ![Diagram för Azure PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>Steg 3: Kör en Hive-jobb med hjälp av Cosmos-databas och HDInsight
> [!IMPORTANT]
> Alla variabler som anges av < > måste fyllas i med hjälp av inställningarna.
>
>

1. Ange hello följande variabler i PowerShell-skript-fönstret.

        # Provide Azure subscription name, hello Azure Storage account and container that is used for hello default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide hello HDInsight cluster name where you want toorun hello Hive job.
        $clusterName = "<HDInsightClusterName>"
2. <p>Vi börjar konstruera frågesträngen. Vi ska skriva en Hive-fråga som tar alla dokument systemgenererade tidsstämplar (_ts) och unika ID: n (_rid) från en samling Azure Cosmos DB, räknar alla dokument med hello minut och sedan lagrar hello resultaten tillbaka till en ny Azure DB som Cosmos-samling.</p>

    <p>Först ska vi skapa en Hive-tabell från våra Azure DB som Cosmos-samling. Lägg till hello följande kodfragment toohello PowerShell-skript fönstret <strong>när</strong> hello kodstycke från #1. Kontrollera att du inkluderar hello valfria DocumentDB.query parametern t trim våra dokument toojust _ts och _rid.</p>

   > [!NOTE]
   > **Namnge DocumentDB.inputCollections var inte fel.** Ja, tillåter vi lägger till flera samlingar som indata: </br>
   >
   >

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> hello collection names are separated without spaces, using only a single comma.

        # Create a Hive table using data from DocumentDB. Pass DocumentDB hello query toofilter transferred data too_rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

1. Nu ska vi skapa en Hive-tabell för hello utdata samling. hello utdata dokumentegenskaper blir hello månad, dag, timme, minut och hello totala antalet förekomster.

   > [!NOTE]
   > **Namnge DocumentDB.outputCollections var ännu igen inte fel.** Ja, tillåter vi lägger till flera samlingar som utdata: </br>
   > '*DocumentDB.outputCollections*'='*\<utdata för DocumentDB-samlingsnamn 1\>*,*\<utdata för DocumentDB-samlingsnamn 2\>*' </br> hello samling namnen avgränsas utan blanksteg, med bara ett komma. </br></br>
   > Dokument kommer att distribuerade resursallokering över flera samlingar. En batch med dokument kommer att lagras i en samling och sedan en andra grupp dokument kommer att lagras i hello nästa insamling och så vidare.
   >
   >

       # Create a Hive table for hello output data tooDocumentDB.
       $queryStringPart2 = "drop table DocumentDB_analytics; " +
                             "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                             "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                             "tblproperties ( " +
                                 "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                 "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                 "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                 "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "
2. Slutligen utdata ska vi tally hello dokument efter månad, dag, timme och minut och infoga hello resultaten tillbaka till hello Hive-tabell.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "
3. Lägg till följande skript fragment toocreate jobbdefinitionen Hive från föregående frågan om hello hello.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    Du kan också använda hello - filen växla toospecify en skriptfil för HiveQL på HDFS.
4. Lägg till följande fragment toosave hello starttiden hello och skicka hello Hive-jobb.

        # Save hello start time and submit hello job toohello cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition
5. Lägg till följande toowait för hello Hive-jobbet toocomplete hello.

        # Wait for hello Hive job toocomplete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600
6. Lägg till hello följande tooprint hello standard utdata och hello start- och sluttider.

        # Print hello standard error, hello standard output of hello Hive job, and hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
7. **Kör** nya skriptet! **Klicka på** hello grönt köra knappen.
8. Kontrollera hello resultat. Logga in på hello [Azure Portal][azure-portal].

   1. Klicka på <strong>Bläddra</strong> på hello vänster panel. </br>
   2. Klicka på <strong>allt</strong> hello längst upp till höger i hello Bläddra panelen. </br>
   3. Hitta och klickar på <strong>Azure Cosmos DB konton</strong>. </br>
   4. Leta sedan reda din <strong>Azure Cosmos-DB konto</strong>, sedan <strong>Azure Cosmos DB Database</strong> och <strong>Azure Cosmos DB samling</strong> som är associerade med hello utdata samling som anges i Hive-frågan.</br>
   5. Klicka slutligen på <strong>Dokumentutforskaren</strong> under <strong>Developer Tools</strong>.</br></p>

   Du ser hello resultaten av Hive-fråga.

   ![Hive frågeresultat][image-hive-query-results]

## <a name="RunPig"></a>Steg 4: Kör ett Pig-jobb med hjälp av Cosmos-DB och HDInsight
> [!IMPORTANT]
> Alla variabler som anges av < > måste fyllas i med hjälp av inställningarna.
>
>

1. Ange hello följande variabler i PowerShell-skript-fönstret.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want toorun hello Pig job.
        $clusterName = "Azure HDInsight Cluster Name"
2. <p>Vi börjar konstruera frågesträngen. Vi ska skriva en Pig-fråga som tar alla dokument systemgenererade tidsstämplar (_ts) och unika ID: n (_rid) från en samling Azure Cosmos DB, räknar alla dokument med hello minut och sedan lagrar hello resultaten tillbaka till en ny Azure DB som Cosmos-samling.</p>
    <p>Ladda först dokument från Cosmos-DB i HDInsight. Lägg till hello följande kodfragment toohello PowerShell-skript fönstret <strong>när</strong> hello kodstycke från #1. Kontrollera att tooadd en DocumentDB fråga toohello valfria DocumentDB-fråga parametern tootrim våra dokument toojust _ts och _rid.</p>

   > [!NOTE]
   > Ja, tillåter vi lägger till flera samlingar som indata: </br>
   > '*\<Indata för DocumentDB-samlingsnamn 1\>*,*\<indata för DocumentDB-samlingsnamn 2\>*'</br> hello samling namnen avgränsas utan blanksteg, med bara ett komma. </b>
   >
   >

    Dokument kommer att distribuerade resursallokering över flera samlingar. En batch med dokument kommer att lagras i en samling och sedan en andra grupp dokument kommer att lagras i hello nästa insamling och så vidare.

        # Load data from Cosmos DB. Pass DocumentDB query toofilter transferred data too_rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
3. Nu ska vi tally hello dokument av hello månad, dag, timme, minut och hello totala antalet förekomster.

       # GROUP BY minute and COUNT entries for each.
       $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                           "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                           "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "
4. Slutligen ska vi lagra hello resultat i vår nya utdata-samlingen.

   > [!NOTE]
   > Ja, tillåter vi lägger till flera samlingar som utdata: </br>
   > '\<Utdata för DocumentDB-samlingsnamn 1\>,\<utdata för DocumentDB-samlingsnamn 2\>'</br> hello samling namnen avgränsas utan blanksteg, med bara ett komma.</br>
   > Dokument kan vara distribuerade resursallokering för hello flera samlingar. En batch med dokument kommer att lagras i en samling och sedan en andra grupp dokument kommer att lagras i hello nästa insamling och så vidare.
   >
   >

        # Store output data tooCosmos DB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "
5. Lägg till följande skript fragment toocreate jobbdefinitionen Pig från föregående frågan om hello hello.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    Du kan också använda hello - filen växla toospecify en skriptfil för Pig på HDFS.
6. Lägg till följande fragment toosave hello starttiden hello och skicka hello Pig-jobbet.

        # Save hello start time and submit hello job toohello cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition
7. Lägg till följande toowait för hello Pig-jobbet toocomplete hello.

        # Wait for hello Pig job toocomplete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600
8. Lägg till hello följande tooprint hello standard utdata och hello start- och sluttider.

        # Print hello standard error, hello standard output of hello Hive job, and hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
9. **Kör** nya skriptet! **Klicka på** hello grönt köra knappen.
10. Kontrollera hello resultat. Logga in på hello [Azure Portal][azure-portal].

    1. Klicka på <strong>Bläddra</strong> på hello vänster panel. </br>
    2. Klicka på <strong>allt</strong> hello längst upp till höger i hello Bläddra panelen. </br>
    3. Hitta och klickar på <strong>Azure Cosmos DB konton</strong>. </br>
    4. Leta sedan reda din <strong>Azure Cosmos-DB konto</strong>, sedan <strong>Azure Cosmos DB Database</strong> och <strong>Azure Cosmos DB samling</strong> som är associerade med hello utdata samling som anges i Pig frågan.</br>
    5. Klicka slutligen på <strong>Dokumentutforskaren</strong> under <strong>Developer Tools</strong>.</br></p>

    Hello resultatet av frågan Pig visas.

    ![Pig frågeresultat][image-pig-query-results]

## <a name="RunMapReduce"></a>Steg 5: Kör ett MapReduce-jobb med hjälp av Azure Cosmos DB och HDInsight
1. Ange hello följande variabler i PowerShell-skript-fönstret.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name
2. Vi ska köra ett MapReduce-jobb som räknar hello antalet förekomster för varje dokumentegenskap från Azure DB som Cosmos-samling. Lägg till det här skriptet fragment **när** hello fragment ovan.

        # Define hello MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

   > [!NOTE]
   > TallyProperties v01.jar medföljer hello anpassad installation av hello Cosmos DB Hadoop-anslutningen.
   >
   >
3. Lägg till hello efter kommandot toosubmit hello MapReduce-jobb.

        # Save hello start time and submit hello job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Dessutom toohello MapReduce jobbdefinitionen du också ange klusternamnet hello HDInsight där du vill toorun hello MapReduce-jobb och hello autentiseringsuppgifter. Hej AzureHDInsightJob Start är en asynkron anrop. toocheck hello slutförande av hello jobb, Använd hello *vänta AzureHDInsightJob* cmdlet.
4. Lägg till följande kommando toocheck hello eventuella fel med körs hello MapReduce-jobb.

        # Get hello job output and print hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
5. **Kör** nya skriptet! **Klicka på** hello grönt köra knappen.
6. Kontrollera hello resultat. Logga in på hello [Azure Portal][azure-portal].

   1. Klicka på <strong>Bläddra</strong> på hello vänster panel.
   2. Klicka på <strong>allt</strong> hello längst upp till höger i hello Bläddra panelen.
   3. Hitta och klickar på <strong>Azure Cosmos DB konton</strong>.
   4. Leta sedan reda din <strong>Azure Cosmos-DB konto</strong>, sedan <strong>Azure Cosmos DB Database</strong> och <strong>Azure Cosmos DB samling</strong> som är associerade med hello utdata samling som anges i MapReduce-jobb.
   5. Klicka slutligen på <strong>Dokumentutforskaren</strong> under <strong>Developer Tools</strong>.

      Du ser hello resultaten av MapReduce-jobb.

      ![MapReduce frågeresultat][image-mapreduce-query-results]

## <a name="NextSteps"></a>Nästa steg
Grattis! Du körde din första Hive, Pig och MapReduce-jobb med hjälp av Azure Cosmos DB och HDInsight.

Vi har öppen källkod våra Hadoop-anslutningen. Om du är intresserad av du kan bidra på [GitHub][github].

toolearn se fler hello följande artiklar:

* [Utveckla ett Java-program med Documentdb][documentdb-java-application]
* [Utveckla Java-MapReduce-program för Hadoop i HDInsight][hdinsight-develop-deploy-java-mapreduce]
* [Komma igång med Hadoop med Hive i HDInsight tooanalyze mobila luren användning][hdinsight-get-started]
* [Använda MapReduce med HDInsight][hdinsight-use-mapreduce]
* [Använda Hive med HDInsight][hdinsight-use-hive]
* [Använda Pig med HDInsight][hdinsight-use-pig]
* [Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[import-data]: import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0
