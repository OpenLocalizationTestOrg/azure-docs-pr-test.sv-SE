---
title: Analysera Twitter-data med Hadoop i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur du använder Hive för att analysera Twitter-data med Hadoop i HDInsight för att hitta frekvensen för användning av ett visst ord."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 78e4ea33-9714-424d-ac07-3d60ecaebf2e
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: a5f97dfa084291cefde9bf27b5639926de1bc80e
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/03/2017
---
# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Analysera Twitter-data med Hive i HDInsight
Sociala webbplatser är en av större Drivande faktorer för stordata införande. Offentliga API: er som tillhandahålls av webbplatser som Twitter är en användbar datakälla för att analysera och förstå populära trender.
I den här självstudiekursen kommer du få tweets med hjälp av en Twitter streaming API och sedan använda Apache Hive på Azure HDInsight för att hämta en lista över Twitter-användare som skickade de flesta tweets som innehöll ett visst ord.

> [!IMPORTANT]
> Stegen i det här dokumentet kräver ett Windows-baserade HDInsight-kluster. Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Åtgärder som är specifika för ett Linux-baserade kluster, se [analysera Twitter-data med hjälp av Hive i HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).

## <a name="prerequisites"></a>Krav
Innan du påbörjar de här självstudierna måste du ha:

* **En arbetsstation** med Azure PowerShell installeras och konfigureras.

    Om du vill köra Windows PowerShell-skript, måste du köra Azure PowerShell som administratör och ange körningsprincipen som *RemoteSigned*. Se [kör Windows PowerShell-skript][powershell-script].

    Kontrollera att du är ansluten till din Azure-prenumeration med hjälp av följande cmdlet innan du kör Windows PowerShell-skript:

    ```powershell
    Login-AzureRmAccount
    ```

    Om du har flera Azure-prenumerationer, använder du följande cmdlet för att ange den aktuella prenumerationen:

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]
    > Azure PowerShell-stöd för hantering av HDInsight-resurser med hjälp av Azure Service Manager **är föråldrat** och togs bort den 1 januari 2017. I stegen i det här dokumentet används de nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.
    >
    > Följ stegen i [Installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) för att installera den senaste versionen av Azure PowerShell. Om du har skript som behöver ändras för att använda de nya cmdletarna som fungerar med Azure Resource Manager, hittar du mer information i [Migrera till Azure Resource Manager-baserade utvecklingsverktyg för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md).

* **Ett Azure HDInsight-kluster**. Mer information om klusteretablering finns [komma igång med HDInsight] [ hdinsight-get-started] eller [etablera HDInsight-kluster][hdinsight-provision]. Klusternamnet måste senare under kursen.

I följande tabell visas de filer som används i den här självstudiekursen:

| Filer | Beskrivning |
| --- | --- |
| /tutorials/Twitter/data/tweets.txt |Källdata för Hive-jobb. |
| /tutorials/Twitter/Output |Den utgående mappen för Hive-jobb. Hive-jobbet utdata Standardfilnamnet är **000000_0**. |
| tutorials/Twitter/Twitter.hql |HiveQL skriptfilen. |
| /tutorials/Twitter/JobStatus |Hadoop-jobbstatus. |

## <a name="get-twitter-feed"></a>Get Twitter-flöde
I den här kursen använder du den [Twitter-API: er för strömning][twitter-streaming-api]. Specifika Twitter streaming API som du ska använda är [statusar/filter][twitter-statuses-filter].

> [!NOTE]
> En fil som innehåller 10 000 tweets och Hive-skriptfil (beskrivs i nästa avsnitt) har laddats upp i en offentlig blobbbehållare. Du kan hoppa över det här avsnittet om du vill använda de överförda filerna.

[Tweets data](https://dev.twitter.com/docs/platform-objects/tweets) lagras i formatet JavaScript Object Notation (JSON) som innehåller en komplex kapslade struktur. I stället för att skriva många rader med kod med hjälp av en konventionell programmeringsspråk, kan du omvandla den här kapslade strukturen i en Hive-tabell så att den kan efterfrågas av en SQL Structured Query Language ()-som språk som kallas HiveQL.

Twitter använder OAuth för auktoriserad åtkomst till dess API. OAuth är ett autentiseringsprotokoll som tillåter användare att godkänna program fungerar åt utan att dela sina lösenord. Mer information finns på [oauth.net](http://oauth.net/) eller i den utmärkt [Nybörjarguide till OAuth](http://hueniverse.com/oauth/) från Hueniverse.

Det första steget att använda OAuth är att skapa ett nytt program på webbplatsen Twitter-utvecklare.

**Skapa ett Twitter-program**

1. Logga in på [https://apps.twitter.com/](https://apps.twitter.com/). Klicka på den **registrera nu** länk om du inte har ett Twitter-konto.
2. Klicka på **Skapa ny App**.
3. Ange **namn**, **beskrivning**, **webbplats**. Du kan göra upp en URL för den **webbplats** fältet. I följande tabell visas några exempelvärden som ska användas:

   | Fält | Värde |
   | --- | --- |
   |  Namn |MyHDInsightApp |
   |  Beskrivning |MyHDInsightApp |
   |  Webbplats |http://www.myhdinsightapp.com |
4. Kontrollera **Ja, jag godkänner**, och klicka sedan på **skapa programmet Twitter**.
5. Klicka på den **behörigheter** fliken. Standardbehörigheten är **skrivskyddad**. Detta är tillräcklig för den här kursen.
6. Klicka på den **nycklar och åtkomst-token** fliken.
7. Klicka på **skapa åtkomst-token**.
8. Klicka på **Test OAuth** i det övre högra hörnet på sidan.
9. Skriv ned **konsumenten nyckeln**, **konsumenthemlighet**, **åtkomsttoken**, och **åtkomst-token hemlighet**. Du behöver värdena senare under kursen.

I den här kursen använder du Windows PowerShell för att anropa webbtjänsten. Andra populära verktyget göra webbtjänstanrop [ *Curl*][curl]. CURL kan hämtas från [här][curl-download].

> [!NOTE]
> När du använder curl-kommando i Windows, Använd dubbla citattecken i stället för enkla citattecken för värden för alternativ.

**Få tweets**

1. Öppna Windows PowerShell Integrated Scripting Environment (ISE). (Ange på startskärmen för Windows 8 **PowerShell_ISE** och klicka sedan på **Windows PowerShell ISE**. Se [starta Windows PowerShell på Windows 8 och Windows][powershell-start].)
2. Kopiera följande skript i skriptfönstret:

    ```powershell
    #region - variables and constants
    $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

    # Enter the OAuth information for your Twitter application
    $oauth_consumer_key = "<TwitterAppConsumerKey>";
    $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
    $oauth_token = "<TwitterAppAccessToken>";
    $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

    $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

    $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
    $track = [System.Uri]::EscapeDataString($trackString);
    $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    Login-AzureRmAccount
    #endregion

    #region - Create a block blob object for writing tweets into Blob storage
    Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $containerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

    Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
    Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

    Write-Host "Create block blob object ..." -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($containerName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    #end region

    # region - Format OAuth strings
    Write-Host "Format oauth strings ..." -ForegroundColor Green
    $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
    $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
    $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

    $signature = "POST&";
    $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
    $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
    $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
    $signature += [System.Uri]::EscapeDataString("track=" + $track);

    $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

    $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
    $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
    $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

    $oauth_authorization = 'OAuth ';
    $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
    $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
    $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
    $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
    $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
    $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
    $oauth_authorization += 'oauth_version="1.0"';

    $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
    #endregion

    #region - Read tweets
    Write-Host "Create HTTP web request ..." -ForegroundColor Green
    [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
    $request.Method = "POST";
    $request.Headers.Add("Authorization", $oauth_authorization);
    $request.ContentType = "application/x-www-form-urlencoded";
    $body = $request.GetRequestStream();

    $body.write($post_body, 0, $post_body.length);
    $body.flush();
    $body.close();
    $response = $request.GetResponse() ;

    Write-Host "Start stream reading ..." -ForegroundColor Green

    Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

    $inrec = $sReader.ReadLine()
    $count = 0
    while (($inrec -ne $null) -and ($count -le $lineMax))
    {
        if ($inrec -ne "")
        {
            Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

            $writeStream.WriteLine($inrec)
            $count ++
        }

        $inrec=$sReader.ReadLine()
    }
    #endregion

    #region - Write tweets to Blob storage
    Write-Host "Write to the destination blob ..." -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    $sReader.close()
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. Ange först fem till åtta variabler i skriptet:

    Variabel|Beskrivning
    ---|---
    $clusterName|Detta är namnet på HDInsight-klustret där du vill köra programmet.
    $oauth_consumer_key|Detta är det Twitter-programmet **konsumenten nyckeln** du skrivit tidigare när du skapade Twitter-programmet.
    $oauth_consumer_secret|Detta är det Twitter-programmet **konsumenthemlighet** du skrev ned tidigare.
    $oauth_token|Detta är det Twitter-programmet **åtkomsttoken** du skrev ned tidigare.
    $oauth_token_secret|Detta är det Twitter-programmet **åtkomst-token hemlighet** du skrev ned tidigare.
    $destBlobName|Detta är blob utdatanamn. Standardvärdet är **tutorials/twitter/data/tweets.txt**. Om du ändrar standardvärdet, behöver du uppdateras också Windows PowerShell-skript.
    $trackString|Webbtjänsten returneras tweets som rör nyckelorden. Standardvärdet är **Azure moln, HDInsight**. Om du ändrar standardvärdet, uppdaterar du Windows PowerShell-skript i enlighet med detta.
    $lineMax|Värdet anger hur många tweets skriptet läses. Det tar cirka tre minuter för att läsa 100 tweets. Du kan ange ett större värde, men det tar längre tid att hämta.

1. Tryck **F5** för att köra skriptet. Om du stöter på problem, som en tillfällig lösning kan du markera alla rader och tryck sedan på **F8**.
2. Du bör se ”Slutför”! i slutet av utdata. Eventuella felmeddelanden visas i rött.

Du kan kontrollera filen, som en valideringsproceduren **/tutorials/twitter/data/tweets.txt**, på ditt Azure Blob storage med hjälp av en Azure Lagringsutforskaren eller Azure PowerShell. En Windows PowerShell-exempelskript för att lista filer, se [använda Blob storage med HDInsight][hdinsight-storage-powershell].

## <a name="create-hiveql-script"></a>Skapa HiveQL-skript
Med Azure PowerShell kan du köra flera HiveQL-instruktioner en i taget eller paketet HiveQL-instruktionen i en skriptfil. I den här självstudiekursen skapar du en HiveQL-skript. Skriptfilen måste överföras till Azure Blob storage. I nästa avsnitt ska du köra skriptfilen med hjälp av Azure PowerShell.

> [!NOTE]
> Hive skriptfilen och en fil som innehåller 10 000 tweets har laddats upp i en offentlig blobbbehållare. Du kan hoppa över det här avsnittet om du vill använda de överförda filerna.

HiveQL-skript kommer att utföra följande:

1. **Ta bort tabellen tweets_raw** om tabellen redan finns.
2. **Skapa Hive-tabell tweets_raw**. Hive strukturerade registret innehåller data för ytterligare extrahering, transformering och laddning (ETL) bearbetning. Mer information om partitioner finns [Hive kursen][apache-hive-tutorial].
3. **Läs in data** från källmappen /tutorials/twitter/data. Stora tweets datauppsättningen i kapslade JSON-format har nu tagits omvandlas till en tillfällig Hive tabellstruktur.
4. **Ta bort tabellen tweets** om tabellen redan finns.
5. **Skapa tabellen tweets**. Innan du kan fråga mot tweets dataset med hjälp av Hive, måste du kör en annan ETL-processen. ETL-processen definierar en mer detaljerad tabellschemat för de data som du har lagrat i tabellen ”twitter_raw”.
6. **Skriv över för infoga**. Det här komplexa Hive-skriptet kommer startar en uppsättning lång MapReduce-jobb med Hadoop-kluster. Detta kan ta ungefär 10 minuter beroende på datamängden och storleken på ditt kluster.
7. **Infoga skriva över katalogen**. Kör en fråga och utdatauppsättningen till en fil. Den här frågan returnerar en lista över Twitter-användare som skickade de flesta tweets som innehåller ordet ”Azure”.

**Skapa en Hive-skript och överföra det till Azure**

1. Öppna Windows PowerShell ISE.
2. Kopiera följande skript i skriptfönstret:

    ```powershell
    #region - variables and constants
    $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
    $subscriptionID = "<Azure Subscription ID>"

    $sourceDataPath = "/tutorials/twitter/data"
    $outputPath = "/tutorials/twitter/output"
    $hqlScriptFile = "tutorials/twitter/twitter.hql"

    $hqlStatements = @"
    set hive.exec.dynamic.partition = true;
    set hive.exec.dynamic.partition.mode = nonstrict;

    DROP TABLE tweets_raw;
    CREATE EXTERNAL TABLE tweets_raw (
        json_response STRING
    )
    STORED AS TEXTFILE LOCATION '$sourceDataPath';

    DROP TABLE tweets;
    CREATE TABLE tweets
    (
        id BIGINT,
        created_at STRING,
        created_at_date STRING,
        created_at_year STRING,
        created_at_month STRING,
        created_at_day STRING,
        created_at_time STRING,
        in_reply_to_user_id_str STRING,
        text STRING,
        contributors STRING,
        retweeted STRING,
        truncated STRING,
        coordinates STRING,
        source STRING,
        retweet_count INT,
        url STRING,
        hashtags array<STRING>,
        user_mentions array<STRING>,
        first_hashtag STRING,
        first_user_mention STRING,
        screen_name STRING,
        name STRING,
        followers_count INT,
        listed_count INT,
        friends_count INT,
        lang STRING,
        user_location STRING,
        time_zone STRING,
        profile_image_url STRING,
        json_response STRING
    );

    FROM tweets_raw
    INSERT OVERWRITE TABLE tweets
    SELECT
        cast(get_json_object(json_response, '$.id_str') as BIGINT),
        get_json_object(json_response, '$.created_at'),
        concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
        substr (get_json_object(json_response, '$.created_at'),27,4)),
        substr (get_json_object(json_response, '$.created_at'),27,4),
        case substr (get_json_object(json_response, '$.created_at'),5,3)
            when "Jan" then "01"
            when "Feb" then "02"
            when "Mar" then "03"
            when "Apr" then "04"
            when "May" then "05"
            when "Jun" then "06"
            when "Jul" then "07"
            when "Aug" then "08"
            when "Sep" then "09"
            when "Oct" then "10"
            when "Nov" then "11"
            when "Dec" then "12" end,
        substr (get_json_object(json_response, '$.created_at'),9,2),
        substr (get_json_object(json_response, '$.created_at'),12,8),
        get_json_object(json_response, '$.in_reply_to_user_id_str'),
        get_json_object(json_response, '$.text'),
        get_json_object(json_response, '$.contributors'),
        get_json_object(json_response, '$.retweeted'),
        get_json_object(json_response, '$.truncated'),
        get_json_object(json_response, '$.coordinates'),
        get_json_object(json_response, '$.source'),
        cast (get_json_object(json_response, '$.retweet_count') as INT),
        get_json_object(json_response, '$.entities.display_url'),
        array(
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
        array(
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
        trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
        trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
        get_json_object(json_response, '$.user.screen_name'),
        get_json_object(json_response, '$.user.name'),
        cast (get_json_object(json_response, '$.user.followers_count') as INT),
        cast (get_json_object(json_response, '$.user.listed_count') as INT),
        cast (get_json_object(json_response, '$.user.friends_count') as INT),
        get_json_object(json_response, '$.user.lang'),
        get_json_object(json_response, '$.user.location'),
        get_json_object(json_response, '$.user.time_zone'),
        get_json_object(json_response, '$.user.profile_image_url'),
        json_response
    WHERE (length(json_response) > 500);

    INSERT OVERWRITE DIRECTORY '$outputPath'
    SELECT name, screen_name, count(1) as cc
        FROM tweets
        WHERE text like "%Azure%"
        GROUP BY name,screen_name
        ORDER BY cc DESC LIMIT 10;
    "@
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #endregion

    #region - Create a block blob object for writing the Hive script file
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

    Write-Host "Define the connection string ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)

    Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    $writeStream.Writeline($hqlStatements)
    #endregion

    #region - Write the Hive script file to Blob storage
    Write-Host "Write to the destination blob ... " -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $hqlScriptBlob.UploadFromStream($memStream)
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. Ange de första två variablerna i skriptet:

   | Variabel | Beskrivning |
   | --- | --- |
   |  $clusterName |Ange klusternamnet HDInsight där du vill köra programmet. |
   |  $subscriptionID |Ange ditt Azure-prenumeration-ID. |
   |  $sourceDataPath |Azure Blob storage plats där Hive-frågor kommer att läsa data från. Du behöver inte ändra den här variabeln. |
   |  $outputPath |Azure Blob storage plats där Hive-frågor kommer skriver resultatet. Du behöver inte ändra den här variabeln. |
   |  $hqlScriptFile |Sökvägen och filnamnet för filen HiveQL-skript. Du behöver inte ändra den här variabeln. |
4. Tryck **F5** för att köra skriptet. Om du stöter på problem, som en tillfällig lösning kan du markera alla rader och tryck sedan på **F8**.
5. Du bör se ”Slutför”! i slutet av utdata. Eventuella felmeddelanden visas i rött.

Du kan kontrollera filen, som en valideringsproceduren **/tutorials/twitter/twitter.hql**, på ditt Azure Blob storage med hjälp av en Azure Lagringsutforskaren eller Azure PowerShell. En Windows PowerShell-exempelskript för att lista filer, se [använda Blob storage med HDInsight][hdinsight-storage-powershell].

## <a name="process-twitter-data-by-using-hive"></a>Bearbeta Twitter-data med hjälp av Hive
Du är klar med alla förberedelse av arbetet. Nu kan du anropa Hive-skript och kontrollera resultaten.

### <a name="submit-a-hive-job"></a>Skicka ett Hive-jobb
Använd följande Windows PowerShell-skript för att köra Hive-skript. Du måste ange den första variabeln.

> [!NOTE]
> Om du vill använda tweets och HiveQL-skript som du har överfört i de sista två avsnitt, tilldelas $hqlScriptFile ”/ tutorials/twitter/twitter.hql”. Om du vill använda de som har överförts till en offentlig blob du värdet $hqlScriptFile ”wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql”.

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"
$httpUserName = "admin"
$httpUserPassword = "<HDInsight Cluster HTTP User Password>"

#use one of the following
$hqlScriptFile = "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
$hqlScriptFile = "/tutorials/twitter/twitter.hql"

$statusFolder = "/tutorials/twitter/jobstatus"
#endregion

$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value

$defaultBlobContainerName = $myCluster.DefaultStorageContainer

#region - Invoke Hive
Write-Host "Invoke Hive ... " -ForegroundColor Green

# Create the HDInsight cluster
$pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential
$response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable

Write-Host "Display the standard error log ... " -ForegroundColor Green
$jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
#endregion
```

### <a name="check-the-results"></a>Kontrollera resultaten
Använd följande Windows PowerShell-skript för att kontrollera utdata för Hive-jobb. Du måste ställa in de första två variablerna.

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"

$blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
#endregion

#region - Create an Azure storage context object
Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
$defaultBlobContainerName = $myCluster.DefaultStorageContainer

Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

Write-Host "Create a context object ... " -ForegroundColor Green
$storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
#endregion

#region - Download blob and display blob
Write-Host "Download the blob ..." -ForegroundColor Green
cd $HOME
Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force

Write-Host "Display the output ..." -ForegroundColor Green
Write-Host "==================================" -ForegroundColor Green
cat "./$blob"
Write-Host "==================================" -ForegroundColor Green
#end region
```

> [!NOTE]
> Hive-tabell använder \001 som fältavgränsaren. Avgränsaren visas inte i utdata.

När de har placerats i Azure Blob storage, kan du exportera data till en Azure SQL-databas/SQL server, exportera data till Excel med hjälp av Power Query eller ansluta programmet till data med hjälp av Hive ODBC-drivrutinen. Mer information finns i [använda Sqoop med HDInsight][hdinsight-use-sqoop], [analysera svarta fördröjning data med HDInsight][hdinsight-analyze-flight-delay-data], [ Anslut Excel till HDInsight med Power Query][hdinsight-power-query], och [ansluter Excel till HDInsight med Microsoft Hive ODBC-drivrutinen][hdinsight-hive-odbc].

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen har vi sett hur att omvandla en ostrukturerad datauppsättning JSON till en strukturerad Hive-tabell för att fråga, utforska och analysera data från Twitter med HDInsight på Azure. Du kan läsa mer här:

* [Komma igång med HDInsight][hdinsight-get-started]
* [Analysera svarta fördröjning data med HDInsight][hdinsight-analyze-flight-delay-data]
* [Anslut Excel till HDInsight med Power Query][hdinsight-power-query]
* [Anslut Excel till HDInsight med Microsoft Hive ODBC-drivrutin][hdinsight-hive-odbc]
* [Använda Sqoop med HDInsight][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]:hdinsight-hadoop-use-blob-storage.md#access-blobs-using-azure-powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-power-query]:hadoop/apache-hadoop-connect-excel-power-query.md
[hdinsight-hive-odbc]:hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md
