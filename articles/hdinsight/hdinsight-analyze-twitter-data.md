---
title: aaaAnalyze Twitter-data med Hadoop i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toouse Hive tooanalyze Twitter-data på Hadoop i HDInsight toofind hello Användningsfrekvens av ett visst ord."
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
ms.openlocfilehash: 40c0a1afbc1fff10c070d22a99cd9d32d42f230a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a><span data-ttu-id="cdfc2-103">Analysera Twitter-data med Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="cdfc2-103">Analyze Twitter data using Hive in HDInsight</span></span>
<span data-ttu-id="cdfc2-104">Sociala webbplatser är en av hello större Drivande faktorer för stordata införande.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-104">Social websites are one of hello major driving forces for big-data adoption.</span></span> <span data-ttu-id="cdfc2-105">Offentliga API: er som tillhandahålls av webbplatser som Twitter är en användbar datakälla för att analysera och förstå populära trender.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-105">Public APIs provided by sites like Twitter are a useful source of data for analyzing and understanding popular trends.</span></span>
<span data-ttu-id="cdfc2-106">I den här självstudiekursen kommer du få tweets med hjälp av en Twitter streaming API och sedan använda Apache Hive på Azure HDInsight tooget en lista över Twitter-användare som skickade hello de flesta tweets som innehöll ett visst ord.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-106">In this tutorial, you will get tweets by using a Twitter streaming API, and then use Apache Hive on Azure HDInsight tooget a list of Twitter users who sent hello most tweets that contained a certain word.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cdfc2-107">hello kräver stegen i det här dokumentet ett Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-107">hello steps in this document require a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="cdfc2-108">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="cdfc2-109">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="cdfc2-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="cdfc2-110">Steg specifika tooa Linux-baserade kluster, se [analysera Twitter-data med hjälp av Hive i HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).</span><span class="sxs-lookup"><span data-stu-id="cdfc2-110">For steps specific tooa Linux-based cluster, see [Analyze Twitter data using Hive in HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cdfc2-111">Krav</span><span class="sxs-lookup"><span data-stu-id="cdfc2-111">Prerequisites</span></span>
<span data-ttu-id="cdfc2-112">Innan du påbörjar den här självstudien måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="cdfc2-112">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="cdfc2-113">**En arbetsstation** med Azure PowerShell installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-113">**A workstation** with Azure PowerShell installed and configured.</span></span>

    <span data-ttu-id="cdfc2-114">tooexecute Windows PowerShell-skript som du måste köra Azure PowerShell som administratör och ange hello körningsprincipen för*RemoteSigned*.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-114">tooexecute Windows PowerShell scripts, you must run Azure PowerShell as administrator and set hello execution policy too*RemoteSigned*.</span></span> <span data-ttu-id="cdfc2-115">Se [kör Windows PowerShell-skript][powershell-script].</span><span class="sxs-lookup"><span data-stu-id="cdfc2-115">See [Run Windows PowerShell scripts][powershell-script].</span></span>

    <span data-ttu-id="cdfc2-116">Kontrollera att du är ansluten tooyour Azure-prenumeration med hjälp av följande cmdlet hello innan du kör Windows PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="cdfc2-116">Before running Windows PowerShell scripts, make sure you are connected tooyour Azure subscription by using hello following cmdlet:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="cdfc2-117">Om du har flera Azure-prenumerationer använder du följande cmdlet tooset hello aktuell prenumeration hello:</span><span class="sxs-lookup"><span data-stu-id="cdfc2-117">If you have multiple Azure subscriptions, use hello following cmdlet tooset hello current subscription:</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="cdfc2-118">Azure PowerShell-stöd för hantering av HDInsight-resurser med hjälp av Azure Service Manager **är föråldrat** och togs bort den 1 januari 2017.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-118">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="cdfc2-119">hello stegen i det här dokumentet används hello nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-119">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="cdfc2-120">Följ stegen hello i [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-120">Please follow hello steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="cdfc2-121">Om du har skript som behöver toobe ändras toouse hello nya cmdletarna som fungerar med Azure Resource Manager kan se [migrera tooAzure Resource Manager-baserade development tools för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-121">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

* <span data-ttu-id="cdfc2-122">**Ett Azure HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-122">**An Azure HDInsight cluster**.</span></span> <span data-ttu-id="cdfc2-123">Mer information om klusteretablering finns [komma igång med HDInsight] [ hdinsight-get-started] eller [etablera HDInsight-kluster][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="cdfc2-123">For instructions on cluster provisioning, see [Get started using HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="cdfc2-124">Du behöver hello klusternamnet senare i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-124">You will need hello cluster name later in hello tutorial.</span></span>

<span data-ttu-id="cdfc2-125">hello visas följande tabell hello-filer som används i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="cdfc2-125">hello following table lists hello files used in this tutorial:</span></span>

| <span data-ttu-id="cdfc2-126">Filer</span><span class="sxs-lookup"><span data-stu-id="cdfc2-126">Files</span></span> | <span data-ttu-id="cdfc2-127">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cdfc2-127">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cdfc2-128">/tutorials/Twitter/data/tweets.txt</span><span class="sxs-lookup"><span data-stu-id="cdfc2-128">/tutorials/twitter/data/tweets.txt</span></span> |<span data-ttu-id="cdfc2-129">hello källdata hello Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-129">hello source data for hello Hive job.</span></span> |
| <span data-ttu-id="cdfc2-130">/tutorials/Twitter/Output</span><span class="sxs-lookup"><span data-stu-id="cdfc2-130">/tutorials/twitter/output</span></span> |<span data-ttu-id="cdfc2-131">hello utdatamapp för hello Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-131">hello output folder for hello Hive job.</span></span> <span data-ttu-id="cdfc2-132">hello Hive-jobbet utdata Standardfilnamnet är **000000_0**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-132">hello default Hive job output file name is **000000_0**.</span></span> |
| <span data-ttu-id="cdfc2-133">tutorials/Twitter/Twitter.hql</span><span class="sxs-lookup"><span data-stu-id="cdfc2-133">tutorials/twitter/twitter.hql</span></span> |<span data-ttu-id="cdfc2-134">Hej HiveQL skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-134">hello HiveQL script file.</span></span> |
| <span data-ttu-id="cdfc2-135">/tutorials/Twitter/JobStatus</span><span class="sxs-lookup"><span data-stu-id="cdfc2-135">/tutorials/twitter/jobstatus</span></span> |<span data-ttu-id="cdfc2-136">Hej Hadoop jobbstatus.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-136">hello Hadoop job status.</span></span> |

## <a name="get-twitter-feed"></a><span data-ttu-id="cdfc2-137">Get Twitter-flöde</span><span class="sxs-lookup"><span data-stu-id="cdfc2-137">Get Twitter feed</span></span>
<span data-ttu-id="cdfc2-138">I den här självstudiekursen kommer du använda hello [Twitter-API: er för strömning][twitter-streaming-api].</span><span class="sxs-lookup"><span data-stu-id="cdfc2-138">In this tutorial, you will use hello [Twitter streaming APIs][twitter-streaming-api].</span></span> <span data-ttu-id="cdfc2-139">hello specifika Twitter streaming API som du ska använda är [statusar/filter][twitter-statuses-filter].</span><span class="sxs-lookup"><span data-stu-id="cdfc2-139">hello specific Twitter streaming API you will use is [statuses/filter][twitter-statuses-filter].</span></span>

> [!NOTE]
> <span data-ttu-id="cdfc2-140">En fil som innehåller 10 000 tweets och hello Hive skriptfilen (beskrivs i nästa avsnitt av hello) har laddats upp i en offentlig blobbbehållare.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-140">A file containing 10,000 tweets and hello Hive script file (covered in hello next section) have been uploaded in a public Blob container.</span></span> <span data-ttu-id="cdfc2-141">Du kan hoppa över det här avsnittet om du vill toouse hello överföra filer.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-141">You can skip this section if you want toouse hello uploaded files.</span></span>

<span data-ttu-id="cdfc2-142">[Tweets data](https://dev.twitter.com/docs/platform-objects/tweets) lagras i hello JavaScript Object Notation (JSON)-format som innehåller en komplex kapslade struktur.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-142">[Tweets data](https://dev.twitter.com/docs/platform-objects/tweets) is stored in hello JavaScript Object Notation (JSON) format that contains a complex nested structure.</span></span> <span data-ttu-id="cdfc2-143">I stället för att skriva många rader med kod med hjälp av en konventionell programmeringsspråk, kan du omvandla den här kapslade strukturen i en Hive-tabell så att den kan efterfrågas av en SQL Structured Query Language ()-som språk som kallas HiveQL.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-143">Instead of writing many lines of code by using a conventional programming language, you can transform this nested structure into a Hive table, so that it can be queried by a Structured Query Language (SQL)-like language called HiveQL.</span></span>

<span data-ttu-id="cdfc2-144">Twitter använder OAuth tooprovide auktoriserad åtkomst tooits API.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-144">Twitter uses OAuth tooprovide authorized access tooits API.</span></span> <span data-ttu-id="cdfc2-145">OAuth är ett autentiseringsprotokoll som gör att användare tooapprove program tooact åt utan att dela sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-145">OAuth is an authentication protocol that allows users tooapprove applications tooact on their behalf without sharing their password.</span></span> <span data-ttu-id="cdfc2-146">Mer information finns på [oauth.net](http://oauth.net/) eller i hello utmärkt [nybörjares Guide tooOAuth](http://hueniverse.com/oauth/) från Hueniverse.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-146">More information can be found at [oauth.net](http://oauth.net/) or in hello excellent [Beginner's Guide tooOAuth](http://hueniverse.com/oauth/) from Hueniverse.</span></span>

<span data-ttu-id="cdfc2-147">hello första steg toouse OAuth är toocreate ett nytt program på hello Twitter Developer.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-147">hello first step toouse OAuth is toocreate a new application on hello Twitter Developer site.</span></span>

<span data-ttu-id="cdfc2-148">**toocreate ett Twitter-program**</span><span class="sxs-lookup"><span data-stu-id="cdfc2-148">**toocreate a Twitter application**</span></span>

1. <span data-ttu-id="cdfc2-149">Logga in för[https://apps.twitter.com/](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="cdfc2-149">Sign in too[https://apps.twitter.com/](https://apps.twitter.com/).</span></span> <span data-ttu-id="cdfc2-150">Klicka på hello **registrera nu** länk om du inte har ett Twitter-konto.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-150">Click hello **Sign up now** link if you don't have a Twitter account.</span></span>
2. <span data-ttu-id="cdfc2-151">Klicka på **Skapa ny App**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-151">Click **Create New App**.</span></span>
3. <span data-ttu-id="cdfc2-152">Ange **namn**, **beskrivning**, **webbplats**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-152">Enter **Name**, **Description**, **Website**.</span></span> <span data-ttu-id="cdfc2-153">Du kan göra upp en URL för hello **webbplats** fältet.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-153">You can make up a URL for hello **Website** field.</span></span> <span data-ttu-id="cdfc2-154">hello följande tabell visar några exempel värden toouse:</span><span class="sxs-lookup"><span data-stu-id="cdfc2-154">hello following table shows some sample values toouse:</span></span>

   | <span data-ttu-id="cdfc2-155">Fält</span><span class="sxs-lookup"><span data-stu-id="cdfc2-155">Field</span></span> | <span data-ttu-id="cdfc2-156">Värde</span><span class="sxs-lookup"><span data-stu-id="cdfc2-156">Value</span></span> |
   | --- | --- |
   |  <span data-ttu-id="cdfc2-157">Namn</span><span class="sxs-lookup"><span data-stu-id="cdfc2-157">Name</span></span> |<span data-ttu-id="cdfc2-158">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="cdfc2-158">MyHDInsightApp</span></span> |
   |  <span data-ttu-id="cdfc2-159">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cdfc2-159">Description</span></span> |<span data-ttu-id="cdfc2-160">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="cdfc2-160">MyHDInsightApp</span></span> |
   |  <span data-ttu-id="cdfc2-161">Webbplats</span><span class="sxs-lookup"><span data-stu-id="cdfc2-161">Website</span></span> |<span data-ttu-id="cdfc2-162">http://www.myhdinsightapp.com</span><span class="sxs-lookup"><span data-stu-id="cdfc2-162">http://www.myhdinsightapp.com</span></span> |
4. <span data-ttu-id="cdfc2-163">Kontrollera **Ja, jag godkänner**, och klicka sedan på **skapa programmet Twitter**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-163">Check **Yes, I agree**, and then click **Create your Twitter application**.</span></span>
5. <span data-ttu-id="cdfc2-164">Klicka på hello **behörigheter** är fliken hello standardbehörigheten **skrivskyddad**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-164">Click hello **Permissions** tab. hello default permission is **Read only**.</span></span> <span data-ttu-id="cdfc2-165">Detta är tillräcklig för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-165">This is sufficient for this tutorial.</span></span>
6. <span data-ttu-id="cdfc2-166">Klicka på hello **nycklar och åtkomst-token** fliken.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-166">Click hello **Keys and Access Tokens** tab.</span></span>
7. <span data-ttu-id="cdfc2-167">Klicka på **skapa åtkomst-token**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-167">Click **Create my access token**.</span></span>
8. <span data-ttu-id="cdfc2-168">Klicka på **Test OAuth** i hello övre högra hörnet av hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-168">Click **Test OAuth** in hello upper-right corner of hello page.</span></span>
9. <span data-ttu-id="cdfc2-169">Skriv ned **konsumenten nyckeln**, **konsumenthemlighet**, **åtkomsttoken**, och **åtkomst-token hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-169">Write down **consumer key**, **Consumer secret**, **Access token**, and **Access token secret**.</span></span> <span data-ttu-id="cdfc2-170">Behöver du hello värden senare i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-170">You will need hello values later in hello tutorial.</span></span>

<span data-ttu-id="cdfc2-171">I den här kursen använder du Windows PowerShell toomake hello webbtjänstanrop.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-171">In this tutorial, you will use Windows PowerShell toomake hello web service call.</span></span> <span data-ttu-id="cdfc2-172">En .NET C# exempel hittar [analysera realtid Twitter-åsikter med HBase i HDInsight][hdinsight-hbase-twitter-sentiment].</span><span class="sxs-lookup"><span data-stu-id="cdfc2-172">For a .NET C# sample, see [Analyze real-time Twitter sentiment with HBase in HDInsight][hdinsight-hbase-twitter-sentiment].</span></span> <span data-ttu-id="cdfc2-173">hello webbtjänstanrop andra populära verktyget toomake är [ *Curl*][curl].</span><span class="sxs-lookup"><span data-stu-id="cdfc2-173">hello other popular tool toomake web service calls is [*Curl*][curl].</span></span> <span data-ttu-id="cdfc2-174">CURL kan hämtas från [här][curl-download].</span><span class="sxs-lookup"><span data-stu-id="cdfc2-174">Curl can be downloaded from [here][curl-download].</span></span>

> [!NOTE]
> <span data-ttu-id="cdfc2-175">När du använder hello curl-kommando i Windows, Använd dubbla citattecken i stället för enkla citattecken för hello alternativvärden.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-175">When you use hello curl command in Windows, use double quotes instead of single quotes for hello option values.</span></span>

<span data-ttu-id="cdfc2-176">**tooget tweets**</span><span class="sxs-lookup"><span data-stu-id="cdfc2-176">**tooget tweets**</span></span>

1. <span data-ttu-id="cdfc2-177">Öppna hello Windows PowerShell Integrated Scripting Environment (ISE).</span><span class="sxs-lookup"><span data-stu-id="cdfc2-177">Open hello Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="cdfc2-178">(På hello Start i Windows 8-skärmen, skriver **PowerShell_ISE** och klicka sedan på **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-178">(On hello Windows 8 Start screen, type **PowerShell_ISE** and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="cdfc2-179">Se [starta Windows PowerShell på Windows 8 och Windows][powershell-start].)</span><span class="sxs-lookup"><span data-stu-id="cdfc2-179">See [Start Windows PowerShell on Windows 8 and Windows][powershell-start].)</span></span>
2. <span data-ttu-id="cdfc2-180">Kopiera följande skript i hello Skriptfönster hello:</span><span class="sxs-lookup"><span data-stu-id="cdfc2-180">Copy hello following script into hello script pane:</span></span>

    ```powershell
    #region - variables and constants
    $clusterName = "<HDInsightClusterName>" # Enter hello HDInsight cluster name

    # Enter hello OAuth information for your Twitter application
    $oauth_consumer_key = "<TwitterAppConsumerKey>";
    $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
    $oauth_token = "<TwitterAppAccessToken>";
    $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

    $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves hello tweets into this blob.

    $trackString = "Azure, Cloud, HDInsight" # This script gets hello tweets containing these keywords.
    $track = [System.Uri]::EscapeDataString($trackString);
    $lineMax = 10000  # hello script will get this number of tweets. It is about 3 minutes every 100 lines.
    #endregion

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    Login-AzureRmAccount
    #endregion

    #region - Create a block blob object for writing tweets into Blob storage
    Write-Host "Get hello default storage account name and Blob container name using hello cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $containerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

    Write-Host "Define hello Azure storage connection string ..." -ForegroundColor Green
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

    #region - Write tweets tooBlob storage
    Write-Host "Write toohello destination blob ..." -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    $sReader.close()
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. <span data-ttu-id="cdfc2-181">Ange hello fem första tooeight variabler i hello skript:</span><span class="sxs-lookup"><span data-stu-id="cdfc2-181">Set hello first five tooeight variables in hello script:</span></span>

    <span data-ttu-id="cdfc2-182">Variabel</span><span class="sxs-lookup"><span data-stu-id="cdfc2-182">Variable</span></span>|<span data-ttu-id="cdfc2-183">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cdfc2-183">Description</span></span>
    ---|---
    <span data-ttu-id="cdfc2-184">$clusterName</span><span class="sxs-lookup"><span data-stu-id="cdfc2-184">$clusterName</span></span>|<span data-ttu-id="cdfc2-185">Detta är hello HDInsight-kluster där du vill att toorun hello programmet hello namn.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-185">This is hello name of hello HDInsight cluster where you want toorun hello application.</span></span>
    <span data-ttu-id="cdfc2-186">$oauth_consumer_key</span><span class="sxs-lookup"><span data-stu-id="cdfc2-186">$oauth_consumer_key</span></span>|<span data-ttu-id="cdfc2-187">Detta är hello Twitter program **konsumenten nyckeln** du skrivit tidigare när du skapade hello Twitter-programmet.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-187">This is hello Twitter application **consumer key** you wrote down earlier when you created hello Twitter application.</span></span>
    <span data-ttu-id="cdfc2-188">$oauth_consumer_secret</span><span class="sxs-lookup"><span data-stu-id="cdfc2-188">$oauth_consumer_secret</span></span>|<span data-ttu-id="cdfc2-189">Detta är hello Twitter program **konsumenthemlighet** du skrev ned tidigare.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-189">This is hello Twitter application **consumer secret** you wrote down earlier.</span></span>
    <span data-ttu-id="cdfc2-190">$oauth_token</span><span class="sxs-lookup"><span data-stu-id="cdfc2-190">$oauth_token</span></span>|<span data-ttu-id="cdfc2-191">Detta är hello Twitter program **åtkomsttoken** du skrev ned tidigare.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-191">This is hello Twitter application **access token** you wrote down earlier.</span></span>
    <span data-ttu-id="cdfc2-192">$oauth_token_secret</span><span class="sxs-lookup"><span data-stu-id="cdfc2-192">$oauth_token_secret</span></span>|<span data-ttu-id="cdfc2-193">Detta är hello Twitter program **åtkomst-token hemlighet** du skrev ned tidigare.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-193">This is hello Twitter application **access token secret** you wrote down earlier.</span></span>
    <span data-ttu-id="cdfc2-194">$destBlobName</span><span class="sxs-lookup"><span data-stu-id="cdfc2-194">$destBlobName</span></span>|<span data-ttu-id="cdfc2-195">Detta är hello blob utdatanamn.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-195">This is hello output blob name.</span></span> <span data-ttu-id="cdfc2-196">hello standardvärdet är **tutorials/twitter/data/tweets.txt**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-196">hello default value is **tutorials/twitter/data/tweets.txt**.</span></span> <span data-ttu-id="cdfc2-197">Om du ändrar standardvärdet för hello måste tooupdate hello Windows PowerShell-skript i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-197">If you change hello default value, you will need tooupdate hello Windows PowerShell scripts accordingly.</span></span>
    <span data-ttu-id="cdfc2-198">$trackString</span><span class="sxs-lookup"><span data-stu-id="cdfc2-198">$trackString</span></span>|<span data-ttu-id="cdfc2-199">hello webbtjänsten returneras tweets relaterade toothese nyckelord.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-199">hello web service will return tweets related toothese keywords.</span></span> <span data-ttu-id="cdfc2-200">hello standardvärdet är **Azure moln, HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-200">hello default value is **Azure, Cloud, HDInsight**.</span></span> <span data-ttu-id="cdfc2-201">Om du ändrar hello standardvärdet, uppdaterar du hello Windows PowerShell-skript i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-201">If you change hello default value, you will update hello Windows PowerShell scripts accordingly.</span></span>
    <span data-ttu-id="cdfc2-202">$lineMax</span><span class="sxs-lookup"><span data-stu-id="cdfc2-202">$lineMax</span></span>|<span data-ttu-id="cdfc2-203">hello värdet avgör hur många tweets hello skriptet läses.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-203">hello value determines how many tweets hello script will read.</span></span> <span data-ttu-id="cdfc2-204">Det tar cirka tre minuter tooread 100 tweets.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-204">It takes about three minutes tooread 100 tweets.</span></span> <span data-ttu-id="cdfc2-205">Du kan ange ett större värde, men det tar mer tid toodownload.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-205">You can set a larger number, but it will take more time toodownload.</span></span>

1. <span data-ttu-id="cdfc2-206">Tryck på **F5** toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-206">Press **F5** toorun hello script.</span></span> <span data-ttu-id="cdfc2-207">Om du stöter på problem, som en tillfällig lösning kan markera alla hello rader och tryck sedan på **F8**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-207">If you run into problems, as a workaround, select all hello lines, and then press **F8**.</span></span>
2. <span data-ttu-id="cdfc2-208">Du bör se ”Slutför”!</span><span class="sxs-lookup"><span data-stu-id="cdfc2-208">You shall see "Complete!"</span></span> <span data-ttu-id="cdfc2-209">hello slutet av hello utdata.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-209">at hello end of hello output.</span></span> <span data-ttu-id="cdfc2-210">Eventuella felmeddelanden visas i rött.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-210">Any error messages will be displayed in red.</span></span>

<span data-ttu-id="cdfc2-211">Som en valideringsproceduren kan du hello utdatafilen **/tutorials/twitter/data/tweets.txt**, på ditt Azure Blob storage med hjälp av en Azure Lagringsutforskaren eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-211">As a validation procedure, you can check hello output file, **/tutorials/twitter/data/tweets.txt**, on your Azure Blob storage by using an Azure storage explorer or Azure PowerShell.</span></span> <span data-ttu-id="cdfc2-212">En Windows PowerShell-exempelskript för att lista filer, se [använda Blob storage med HDInsight][hdinsight-storage-powershell].</span><span class="sxs-lookup"><span data-stu-id="cdfc2-212">For a sample Windows PowerShell script for listing files, see [Use Blob storage with HDInsight][hdinsight-storage-powershell].</span></span>

## <a name="create-hiveql-script"></a><span data-ttu-id="cdfc2-213">Skapa HiveQL-skript</span><span class="sxs-lookup"><span data-stu-id="cdfc2-213">Create HiveQL script</span></span>
<span data-ttu-id="cdfc2-214">Med Azure PowerShell kan köra du flera HiveQL-instruktioner som vid en tidpunkt eller paketet hello HiveQL-instruktionen i en skriptfil.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-214">Using Azure PowerShell, you can run multiple HiveQL statements one at a time, or package hello HiveQL statement into a script file.</span></span> <span data-ttu-id="cdfc2-215">I den här självstudiekursen skapar du en HiveQL-skript.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-215">In this tutorial, you will create a HiveQL script.</span></span> <span data-ttu-id="cdfc2-216">hello skriptfilen måste vara överförda tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-216">hello script file must be uploaded tooAzure Blob storage.</span></span> <span data-ttu-id="cdfc2-217">Hello nästa avsnitt, ska du köra hello skriptfilen med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-217">In hello next section, you will run hello script file by using Azure PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="cdfc2-218">hello Hive-skriptfilen och en fil som innehåller 10 000 tweets har laddats upp i en offentlig blobbbehållare.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-218">hello Hive script file and a file containing 10,000 tweets have been uploaded in a public Blob container.</span></span> <span data-ttu-id="cdfc2-219">Du kan hoppa över det här avsnittet om du vill toouse hello överföra filer.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-219">You can skip this section if you want toouse hello uploaded files.</span></span>

<span data-ttu-id="cdfc2-220">Hej HiveQL-skript utför hello följande:</span><span class="sxs-lookup"><span data-stu-id="cdfc2-220">hello HiveQL script will perform hello following:</span></span>

1. <span data-ttu-id="cdfc2-221">**Släppa hello tweets_raw tabellen** hello tabellen redan finns.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-221">**Drop hello tweets_raw table** in case hello table already exists.</span></span>
2. <span data-ttu-id="cdfc2-222">**Skapa hello tweets_raw Hive-tabell**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-222">**Create hello tweets_raw Hive table**.</span></span> <span data-ttu-id="cdfc2-223">Hive strukturerade registret innehåller hello data för ytterligare extrahering, transformering och laddning (ETL) bearbetning.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-223">This temporary Hive structured table holds hello data for further extract, transform, and load (ETL) processing.</span></span> <span data-ttu-id="cdfc2-224">Mer information om partitioner finns [Hive kursen][apache-hive-tutorial].</span><span class="sxs-lookup"><span data-stu-id="cdfc2-224">For information on partitions, see [Hive tutorial][apache-hive-tutorial].</span></span>
3. <span data-ttu-id="cdfc2-225">**Läs in data** från källmappen hello, /tutorials/twitter/data.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-225">**Load data** from hello source folder, /tutorials/twitter/data.</span></span> <span data-ttu-id="cdfc2-226">hello stora tweets dataset i kapslade JSON-format har nu tagits omvandlas till en tillfällig Hive tabellstruktur.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-226">hello large tweets dataset in nested JSON format has now been transformed into a temporary Hive table structure.</span></span>
4. <span data-ttu-id="cdfc2-227">**Släpp hello tweets tabell** hello tabellen redan finns.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-227">**Drop hello tweets table** in case hello table already exists.</span></span>
5. <span data-ttu-id="cdfc2-228">**Skapa hello tweets tabell**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-228">**Create hello tweets table**.</span></span> <span data-ttu-id="cdfc2-229">Innan du kan fråga mot hello tweets dataset med hjälp av Hive, behöver du toorun en annan ETL-processen.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-229">Before you can query against hello tweets dataset by using Hive, you need toorun another ETL process.</span></span> <span data-ttu-id="cdfc2-230">ETL-processen definierar en mer detaljerad tabellschemat för hello data som du har lagrat i hello ”twitter_raw” tabell.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-230">This ETL process defines a more detailed table schema for hello data that you have stored in hello "twitter_raw" table.</span></span>
6. <span data-ttu-id="cdfc2-231">**Skriv över för infoga**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-231">**Insert overwrite table**.</span></span> <span data-ttu-id="cdfc2-232">Det här komplexa Hive-skriptet kommer startar en uppsättning lång MapReduce-jobb av hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-232">This complex Hive script will kick off a set of long MapReduce jobs by hello Hadoop cluster.</span></span> <span data-ttu-id="cdfc2-233">Detta kan ta ungefär 10 minuter beroende på dina dataset och hello storleken på ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-233">Depending on your dataset and hello size of your cluster, this could take about 10 minutes.</span></span>
7. <span data-ttu-id="cdfc2-234">**Infoga skriva över katalogen**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-234">**Insert overwrite directory**.</span></span> <span data-ttu-id="cdfc2-235">Kör en fråga och utdata hello dataset tooa fil.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-235">Run a query and output hello dataset tooa file.</span></span> <span data-ttu-id="cdfc2-236">Den här frågan returnerar en lista över Twitter-användare som skickade de flesta tweets hello ordet ”Azure”.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-236">This query will return a list of Twitter users who sent most tweets that contained hello word "Azure".</span></span>

<span data-ttu-id="cdfc2-237">**toocreate en Hive-skript och ladda upp den tooAzure**</span><span class="sxs-lookup"><span data-stu-id="cdfc2-237">**toocreate a Hive script and upload it tooAzure**</span></span>

1. <span data-ttu-id="cdfc2-238">Öppna Windows PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-238">Open Windows PowerShell ISE.</span></span>
2. <span data-ttu-id="cdfc2-239">Kopiera följande skript i hello Skriptfönster hello:</span><span class="sxs-lookup"><span data-stu-id="cdfc2-239">Copy hello following script into hello script pane:</span></span>

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

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #endregion

    #region - Create a block blob object for writing hello Hive script file
    Write-Host "Get hello default storage account name and container name based on hello cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

    Write-Host "Define hello connection string ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    Write-Host "Create block blob objects referencing hello hql script file" -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)

    Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    $writeStream.Writeline($hqlStatements)
    #endregion

    #region - Write hello Hive script file tooBlob storage
    Write-Host "Write toohello destination blob ... " -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $hqlScriptBlob.UploadFromStream($memStream)
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. <span data-ttu-id="cdfc2-240">Ange hello först två variabler i hello skript:</span><span class="sxs-lookup"><span data-stu-id="cdfc2-240">Set hello first two variables in hello script:</span></span>

   | <span data-ttu-id="cdfc2-241">Variabel</span><span class="sxs-lookup"><span data-stu-id="cdfc2-241">Variable</span></span> | <span data-ttu-id="cdfc2-242">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cdfc2-242">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="cdfc2-243">$clusterName</span><span class="sxs-lookup"><span data-stu-id="cdfc2-243">$clusterName</span></span> |<span data-ttu-id="cdfc2-244">Ange hello HDInsight-klustrets namn där du vill toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-244">Enter hello HDInsight cluster name where you want toorun hello application.</span></span> |
   |  <span data-ttu-id="cdfc2-245">$subscriptionID</span><span class="sxs-lookup"><span data-stu-id="cdfc2-245">$subscriptionID</span></span> |<span data-ttu-id="cdfc2-246">Ange ditt Azure-prenumeration-ID.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-246">Enter your Azure subscription ID.</span></span> |
   |  <span data-ttu-id="cdfc2-247">$sourceDataPath</span><span class="sxs-lookup"><span data-stu-id="cdfc2-247">$sourceDataPath</span></span> |<span data-ttu-id="cdfc2-248">hello Azure Blob-lagringsplats där hello Hive-frågor ska läsa hello data från.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-248">hello Azure Blob storage location where hello Hive queries will read hello data from.</span></span> <span data-ttu-id="cdfc2-249">Du behöver inte toochange den här variabeln.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-249">You don't need toochange this variable.</span></span> |
   |  <span data-ttu-id="cdfc2-250">$outputPath</span><span class="sxs-lookup"><span data-stu-id="cdfc2-250">$outputPath</span></span> |<span data-ttu-id="cdfc2-251">hello Azure Blob-lagringsplats där hello Hive-frågor kommer utdata hello resultat.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-251">hello Azure Blob storage location where hello Hive queries will output hello results.</span></span> <span data-ttu-id="cdfc2-252">Du behöver inte toochange den här variabeln.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-252">You don't need toochange this variable.</span></span> |
   |  <span data-ttu-id="cdfc2-253">$hqlScriptFile</span><span class="sxs-lookup"><span data-stu-id="cdfc2-253">$hqlScriptFile</span></span> |<span data-ttu-id="cdfc2-254">hello plats och filnamn för hello hello HiveQL skriptfil.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-254">hello location and hello file name of hello HiveQL script file.</span></span> <span data-ttu-id="cdfc2-255">Du behöver inte toochange den här variabeln.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-255">You don't need toochange this variable.</span></span> |
4. <span data-ttu-id="cdfc2-256">Tryck på **F5** toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-256">Press **F5** toorun hello script.</span></span> <span data-ttu-id="cdfc2-257">Om du stöter på problem, som en tillfällig lösning kan markera alla hello rader och tryck sedan på **F8**.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-257">If you run into problems, as a workaround, select all hello lines, and then press **F8**.</span></span>
5. <span data-ttu-id="cdfc2-258">Du bör se ”Slutför”!</span><span class="sxs-lookup"><span data-stu-id="cdfc2-258">You shall see "Complete!"</span></span> <span data-ttu-id="cdfc2-259">hello slutet av hello utdata.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-259">at hello end of hello output.</span></span> <span data-ttu-id="cdfc2-260">Eventuella felmeddelanden visas i rött.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-260">Any error messages will be displayed in red.</span></span>

<span data-ttu-id="cdfc2-261">Som en valideringsproceduren kan du hello utdatafilen **/tutorials/twitter/twitter.hql**, på ditt Azure Blob storage med hjälp av en Azure Lagringsutforskaren eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-261">As a validation procedure, you can check hello output file, **/tutorials/twitter/twitter.hql**, on your Azure Blob storage by using an Azure storage explorer or Azure PowerShell.</span></span> <span data-ttu-id="cdfc2-262">En Windows PowerShell-exempelskript för att lista filer, se [använda Blob storage med HDInsight][hdinsight-storage-powershell].</span><span class="sxs-lookup"><span data-stu-id="cdfc2-262">For a sample Windows PowerShell script for listing files, see [Use Blob storage with HDInsight][hdinsight-storage-powershell].</span></span>

## <a name="process-twitter-data-by-using-hive"></a><span data-ttu-id="cdfc2-263">Bearbeta Twitter-data med hjälp av Hive</span><span class="sxs-lookup"><span data-stu-id="cdfc2-263">Process Twitter data by using Hive</span></span>
<span data-ttu-id="cdfc2-264">Du är klar med alla hello förberedelse arbete.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-264">You have finished all hello preparation work.</span></span> <span data-ttu-id="cdfc2-265">Nu kan du anropa hello Hive-skript och kontrollera hello resultat.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-265">Now, you can invoke hello Hive script and check hello results.</span></span>

### <a name="submit-a-hive-job"></a><span data-ttu-id="cdfc2-266">Skicka ett Hive-jobb</span><span class="sxs-lookup"><span data-stu-id="cdfc2-266">Submit a Hive job</span></span>
<span data-ttu-id="cdfc2-267">Använd hello följande Windows PowerShell-skript toorun hello Hive-skript.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-267">Use hello following Windows PowerShell script toorun hello Hive script.</span></span> <span data-ttu-id="cdfc2-268">Du behöver tooset hello första variabeln.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-268">You will need tooset hello first variable.</span></span>

> [!NOTE]
> <span data-ttu-id="cdfc2-269">toouse hello tweets och HiveQL-skript som du har överfört i hello sista två avsnitt set $hqlScriptFile too"/tutorials/twitter/twitter.hql hello”.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-269">toouse hello tweets and hello HiveQL script you uploaded in hello last two sections, set $hqlScriptFile too"/tutorials/twitter/twitter.hql".</span></span> <span data-ttu-id="cdfc2-270">toouse hello de som har överförts tooa offentlig blob du genom att ange $hqlScriptFile för ”wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql”.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-270">toouse hello ones that have been uploaded tooa public blob for you, set $hqlScriptFile too"wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".</span></span>

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"
$httpUserName = "admin"
$httpUserPassword = "<HDInsight Cluster HTTP User Password>"

#use one of hello following
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

# Create hello HDInsight cluster
$pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential
$response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable

Write-Host "Display hello standard error log ... " -ForegroundColor Green
$jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
#endregion
```

### <a name="check-hello-results"></a><span data-ttu-id="cdfc2-271">Hello resultat</span><span class="sxs-lookup"><span data-stu-id="cdfc2-271">Check hello results</span></span>
<span data-ttu-id="cdfc2-272">Använd följande Windows PowerShell-skript toocheck hello Hive jobbutdata hello.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-272">Use hello following Windows PowerShell script toocheck hello Hive job output.</span></span> <span data-ttu-id="cdfc2-273">Du behöver tooset hello först två variabler.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-273">You will need tooset hello first two variables.</span></span>

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"

$blob = "tutorials/twitter/output/000000_0" # hello name of hello blob toobe downloaded.
#endregion

#region - Create an Azure storage context object
Write-Host "Get hello default storage account name and container name based on hello cluster name ..." -ForegroundColor Green
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
Write-Host "Download hello blob ..." -ForegroundColor Green
cd $HOME
Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force

Write-Host "Display hello output ..." -ForegroundColor Green
Write-Host "==================================" -ForegroundColor Green
cat "./$blob"
Write-Host "==================================" -ForegroundColor Green
#end region
```

> [!NOTE]
> hello Hive-tabell använder \001 hello fältavgränsaren. <span data-ttu-id="cdfc2-275">hello avgränsare visas inte i hello utdata.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-275">hello delimiter is not visible in hello output.</span></span>

<span data-ttu-id="cdfc2-276">När hello analysresultat har placerats i Azure Blob storage, kan du exportera hello data tooan Azure SQL-databas/SQL server, exportera hello data tooExcel med Power Query eller ansluta dina programdata toohello med hjälp av hello Hive ODBC-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-276">After hello analysis results have been placed in Azure Blob storage, you can export hello data tooan Azure SQL database/SQL server, export hello data tooExcel by using Power Query, or connect your application toohello data by using hello Hive ODBC Driver.</span></span> <span data-ttu-id="cdfc2-277">Mer information finns i [använda Sqoop med HDInsight][hdinsight-use-sqoop], [analysera svarta fördröjning data med HDInsight][hdinsight-analyze-flight-delay-data], [ Ansluta Excel tooHDInsight med Power Query][hdinsight-power-query], och [ansluta Excel tooHDInsight med hello Microsoft Hive ODBC-drivrutinen][hdinsight-hive-odbc].</span><span class="sxs-lookup"><span data-stu-id="cdfc2-277">For more information, see [Use Sqoop with HDInsight][hdinsight-use-sqoop], [Analyze flight delay data using HDInsight][hdinsight-analyze-flight-delay-data], [Connect Excel tooHDInsight with Power Query][hdinsight-power-query], and [Connect Excel tooHDInsight with hello Microsoft Hive ODBC Driver][hdinsight-hive-odbc].</span></span>

## <a name="next-steps"></a><span data-ttu-id="cdfc2-278">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cdfc2-278">Next steps</span></span>
<span data-ttu-id="cdfc2-279">I den här självstudiekursen har vi sett hur tootransform en ostrukturerad datauppsättning JSON till en strukturerad Hive tabell tooquery utforska och analysera data från Twitter med HDInsight på Azure.</span><span class="sxs-lookup"><span data-stu-id="cdfc2-279">In this tutorial we have seen how tootransform an unstructured JSON dataset into a structured Hive table tooquery, explore, and analyze data from Twitter by using HDInsight on Azure.</span></span> <span data-ttu-id="cdfc2-280">Det finns fler toolearn:</span><span class="sxs-lookup"><span data-stu-id="cdfc2-280">toolearn more, see:</span></span>

* <span data-ttu-id="cdfc2-281">[Komma igång med HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="cdfc2-281">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="cdfc2-282">[Analysera realtid Twitter-åsikter med HBase i HDInsight][hdinsight-hbase-twitter-sentiment]</span><span class="sxs-lookup"><span data-stu-id="cdfc2-282">[Analyze real-time Twitter sentiment with HBase in HDInsight][hdinsight-hbase-twitter-sentiment]</span></span>
* <span data-ttu-id="cdfc2-283">[Analysera svarta fördröjning data med HDInsight][hdinsight-analyze-flight-delay-data]</span><span class="sxs-lookup"><span data-stu-id="cdfc2-283">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-delay-data]</span></span>
* <span data-ttu-id="cdfc2-284">[Ansluta Excel tooHDInsight med Power Query][hdinsight-power-query]</span><span class="sxs-lookup"><span data-stu-id="cdfc2-284">[Connect Excel tooHDInsight with Power Query][hdinsight-power-query]</span></span>
* <span data-ttu-id="cdfc2-285">[Ansluta Excel tooHDInsight med hello Microsoft Hive ODBC-drivrutinen][hdinsight-hive-odbc]</span><span class="sxs-lookup"><span data-stu-id="cdfc2-285">[Connect Excel tooHDInsight with hello Microsoft Hive ODBC Driver][hdinsight-hive-odbc]</span></span>
* <span data-ttu-id="cdfc2-286">[Använda Sqoop med HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="cdfc2-286">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]:hdinsight-hadoop-use-blob-storage.md#access-blobs-using-azure-powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
