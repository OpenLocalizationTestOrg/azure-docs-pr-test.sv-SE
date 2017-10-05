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
ms.openlocfilehash: 711d364c36c3aba699326f4a76d42891ba3219fb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a><span data-ttu-id="7799b-103">Analysera Twitter-data med Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7799b-103">Analyze Twitter data using Hive in HDInsight</span></span>
<span data-ttu-id="7799b-104">Sociala webbplatser är en av större Drivande faktorer för stordata införande.</span><span class="sxs-lookup"><span data-stu-id="7799b-104">Social websites are one of the major driving forces for big-data adoption.</span></span> <span data-ttu-id="7799b-105">Offentliga API: er som tillhandahålls av webbplatser som Twitter är en användbar datakälla för att analysera och förstå populära trender.</span><span class="sxs-lookup"><span data-stu-id="7799b-105">Public APIs provided by sites like Twitter are a useful source of data for analyzing and understanding popular trends.</span></span>
<span data-ttu-id="7799b-106">I den här självstudiekursen kommer du få tweets med hjälp av en Twitter streaming API och sedan använda Apache Hive på Azure HDInsight för att hämta en lista över Twitter-användare som skickade de flesta tweets som innehöll ett visst ord.</span><span class="sxs-lookup"><span data-stu-id="7799b-106">In this tutorial, you will get tweets by using a Twitter streaming API, and then use Apache Hive on Azure HDInsight to get a list of Twitter users who sent the most tweets that contained a certain word.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7799b-107">Stegen i det här dokumentet kräver ett Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7799b-107">The steps in this document require a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="7799b-108">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="7799b-108">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="7799b-109">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="7799b-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="7799b-110">Åtgärder som är specifika för ett Linux-baserade kluster, se [analysera Twitter-data med hjälp av Hive i HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).</span><span class="sxs-lookup"><span data-stu-id="7799b-110">For steps specific to a Linux-based cluster, see [Analyze Twitter data using Hive in HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7799b-111">Krav</span><span class="sxs-lookup"><span data-stu-id="7799b-111">Prerequisites</span></span>
<span data-ttu-id="7799b-112">Innan du påbörjar de här självstudierna måste du ha:</span><span class="sxs-lookup"><span data-stu-id="7799b-112">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="7799b-113">**En arbetsstation** med Azure PowerShell installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="7799b-113">**A workstation** with Azure PowerShell installed and configured.</span></span>

    <span data-ttu-id="7799b-114">Om du vill köra Windows PowerShell-skript, måste du köra Azure PowerShell som administratör och ange körningsprincipen som *RemoteSigned*.</span><span class="sxs-lookup"><span data-stu-id="7799b-114">To execute Windows PowerShell scripts, you must run Azure PowerShell as administrator and set the execution policy to *RemoteSigned*.</span></span> <span data-ttu-id="7799b-115">Se [kör Windows PowerShell-skript][powershell-script].</span><span class="sxs-lookup"><span data-stu-id="7799b-115">See [Run Windows PowerShell scripts][powershell-script].</span></span>

    <span data-ttu-id="7799b-116">Kontrollera att du är ansluten till din Azure-prenumeration med hjälp av följande cmdlet innan du kör Windows PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="7799b-116">Before running Windows PowerShell scripts, make sure you are connected to your Azure subscription by using the following cmdlet:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="7799b-117">Om du har flera Azure-prenumerationer, använder du följande cmdlet för att ange den aktuella prenumerationen:</span><span class="sxs-lookup"><span data-stu-id="7799b-117">If you have multiple Azure subscriptions, use the following cmdlet to set the current subscription:</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="7799b-118">Azure PowerShell-stöd för hantering av HDInsight-resurser med hjälp av Azure Service Manager **är föråldrat** och togs bort den 1 januari 2017.</span><span class="sxs-lookup"><span data-stu-id="7799b-118">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="7799b-119">I stegen i det här dokumentet används de nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7799b-119">The steps in this document use the new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="7799b-120">Följ stegen i [Installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) för att installera den senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7799b-120">Please follow the steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) to install the latest version of Azure PowerShell.</span></span> <span data-ttu-id="7799b-121">Om du har skript som behöver ändras för att använda de nya cmdletarna som fungerar med Azure Resource Manager, hittar du mer information i [Migrera till Azure Resource Manager-baserade utvecklingsverktyg för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="7799b-121">If you have scripts that need to be modified to use the new cmdlets that work with Azure Resource Manager, see [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

* <span data-ttu-id="7799b-122">**Ett Azure HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="7799b-122">**An Azure HDInsight cluster**.</span></span> <span data-ttu-id="7799b-123">Mer information om klusteretablering finns [komma igång med HDInsight] [ hdinsight-get-started] eller [etablera HDInsight-kluster][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="7799b-123">For instructions on cluster provisioning, see [Get started using HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="7799b-124">Klusternamnet måste senare under kursen.</span><span class="sxs-lookup"><span data-stu-id="7799b-124">You will need the cluster name later in the tutorial.</span></span>

<span data-ttu-id="7799b-125">I följande tabell visas de filer som används i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="7799b-125">The following table lists the files used in this tutorial:</span></span>

| <span data-ttu-id="7799b-126">Filer</span><span class="sxs-lookup"><span data-stu-id="7799b-126">Files</span></span> | <span data-ttu-id="7799b-127">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7799b-127">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7799b-128">/tutorials/Twitter/data/tweets.txt</span><span class="sxs-lookup"><span data-stu-id="7799b-128">/tutorials/twitter/data/tweets.txt</span></span> |<span data-ttu-id="7799b-129">Källdata för Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="7799b-129">The source data for the Hive job.</span></span> |
| <span data-ttu-id="7799b-130">/tutorials/Twitter/Output</span><span class="sxs-lookup"><span data-stu-id="7799b-130">/tutorials/twitter/output</span></span> |<span data-ttu-id="7799b-131">Den utgående mappen för Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="7799b-131">The output folder for the Hive job.</span></span> <span data-ttu-id="7799b-132">Hive-jobbet utdata Standardfilnamnet är **000000_0**.</span><span class="sxs-lookup"><span data-stu-id="7799b-132">The default Hive job output file name is **000000_0**.</span></span> |
| <span data-ttu-id="7799b-133">tutorials/Twitter/Twitter.hql</span><span class="sxs-lookup"><span data-stu-id="7799b-133">tutorials/twitter/twitter.hql</span></span> |<span data-ttu-id="7799b-134">HiveQL skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="7799b-134">The HiveQL script file.</span></span> |
| <span data-ttu-id="7799b-135">/tutorials/Twitter/JobStatus</span><span class="sxs-lookup"><span data-stu-id="7799b-135">/tutorials/twitter/jobstatus</span></span> |<span data-ttu-id="7799b-136">Hadoop-jobbstatus.</span><span class="sxs-lookup"><span data-stu-id="7799b-136">The Hadoop job status.</span></span> |

## <a name="get-twitter-feed"></a><span data-ttu-id="7799b-137">Get Twitter-flöde</span><span class="sxs-lookup"><span data-stu-id="7799b-137">Get Twitter feed</span></span>
<span data-ttu-id="7799b-138">I den här kursen använder du den [Twitter-API: er för strömning][twitter-streaming-api].</span><span class="sxs-lookup"><span data-stu-id="7799b-138">In this tutorial, you will use the [Twitter streaming APIs][twitter-streaming-api].</span></span> <span data-ttu-id="7799b-139">Specifika Twitter streaming API som du ska använda är [statusar/filter][twitter-statuses-filter].</span><span class="sxs-lookup"><span data-stu-id="7799b-139">The specific Twitter streaming API you will use is [statuses/filter][twitter-statuses-filter].</span></span>

> [!NOTE]
> <span data-ttu-id="7799b-140">En fil som innehåller 10 000 tweets och Hive-skriptfil (beskrivs i nästa avsnitt) har laddats upp i en offentlig blobbbehållare.</span><span class="sxs-lookup"><span data-stu-id="7799b-140">A file containing 10,000 tweets and the Hive script file (covered in the next section) have been uploaded in a public Blob container.</span></span> <span data-ttu-id="7799b-141">Du kan hoppa över det här avsnittet om du vill använda de överförda filerna.</span><span class="sxs-lookup"><span data-stu-id="7799b-141">You can skip this section if you want to use the uploaded files.</span></span>

<span data-ttu-id="7799b-142">[Tweets data](https://dev.twitter.com/docs/platform-objects/tweets) lagras i formatet JavaScript Object Notation (JSON) som innehåller en komplex kapslade struktur.</span><span class="sxs-lookup"><span data-stu-id="7799b-142">[Tweets data](https://dev.twitter.com/docs/platform-objects/tweets) is stored in the JavaScript Object Notation (JSON) format that contains a complex nested structure.</span></span> <span data-ttu-id="7799b-143">I stället för att skriva många rader med kod med hjälp av en konventionell programmeringsspråk, kan du omvandla den här kapslade strukturen i en Hive-tabell så att den kan efterfrågas av en SQL Structured Query Language ()-som språk som kallas HiveQL.</span><span class="sxs-lookup"><span data-stu-id="7799b-143">Instead of writing many lines of code by using a conventional programming language, you can transform this nested structure into a Hive table, so that it can be queried by a Structured Query Language (SQL)-like language called HiveQL.</span></span>

<span data-ttu-id="7799b-144">Twitter använder OAuth för auktoriserad åtkomst till dess API.</span><span class="sxs-lookup"><span data-stu-id="7799b-144">Twitter uses OAuth to provide authorized access to its API.</span></span> <span data-ttu-id="7799b-145">OAuth är ett autentiseringsprotokoll som tillåter användare att godkänna program fungerar åt utan att dela sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="7799b-145">OAuth is an authentication protocol that allows users to approve applications to act on their behalf without sharing their password.</span></span> <span data-ttu-id="7799b-146">Mer information finns på [oauth.net](http://oauth.net/) eller i den utmärkt [Nybörjarguide till OAuth](http://hueniverse.com/oauth/) från Hueniverse.</span><span class="sxs-lookup"><span data-stu-id="7799b-146">More information can be found at [oauth.net](http://oauth.net/) or in the excellent [Beginner's Guide to OAuth](http://hueniverse.com/oauth/) from Hueniverse.</span></span>

<span data-ttu-id="7799b-147">Det första steget att använda OAuth är att skapa ett nytt program på webbplatsen Twitter-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="7799b-147">The first step to use OAuth is to create a new application on the Twitter Developer site.</span></span>

<span data-ttu-id="7799b-148">**Skapa ett Twitter-program**</span><span class="sxs-lookup"><span data-stu-id="7799b-148">**To create a Twitter application**</span></span>

1. <span data-ttu-id="7799b-149">Logga in på [https://apps.twitter.com/](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="7799b-149">Sign in to [https://apps.twitter.com/](https://apps.twitter.com/).</span></span> <span data-ttu-id="7799b-150">Klicka på den **registrera nu** länk om du inte har ett Twitter-konto.</span><span class="sxs-lookup"><span data-stu-id="7799b-150">Click the **Sign up now** link if you don't have a Twitter account.</span></span>
2. <span data-ttu-id="7799b-151">Klicka på **Skapa ny App**.</span><span class="sxs-lookup"><span data-stu-id="7799b-151">Click **Create New App**.</span></span>
3. <span data-ttu-id="7799b-152">Ange **namn**, **beskrivning**, **webbplats**.</span><span class="sxs-lookup"><span data-stu-id="7799b-152">Enter **Name**, **Description**, **Website**.</span></span> <span data-ttu-id="7799b-153">Du kan göra upp en URL för den **webbplats** fältet.</span><span class="sxs-lookup"><span data-stu-id="7799b-153">You can make up a URL for the **Website** field.</span></span> <span data-ttu-id="7799b-154">I följande tabell visas några exempelvärden som ska användas:</span><span class="sxs-lookup"><span data-stu-id="7799b-154">The following table shows some sample values to use:</span></span>

   | <span data-ttu-id="7799b-155">Fält</span><span class="sxs-lookup"><span data-stu-id="7799b-155">Field</span></span> | <span data-ttu-id="7799b-156">Värde</span><span class="sxs-lookup"><span data-stu-id="7799b-156">Value</span></span> |
   | --- | --- |
   |  <span data-ttu-id="7799b-157">Namn</span><span class="sxs-lookup"><span data-stu-id="7799b-157">Name</span></span> |<span data-ttu-id="7799b-158">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="7799b-158">MyHDInsightApp</span></span> |
   |  <span data-ttu-id="7799b-159">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7799b-159">Description</span></span> |<span data-ttu-id="7799b-160">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="7799b-160">MyHDInsightApp</span></span> |
   |  <span data-ttu-id="7799b-161">Webbplats</span><span class="sxs-lookup"><span data-stu-id="7799b-161">Website</span></span> |<span data-ttu-id="7799b-162">http://www.myhdinsightapp.com</span><span class="sxs-lookup"><span data-stu-id="7799b-162">http://www.myhdinsightapp.com</span></span> |
4. <span data-ttu-id="7799b-163">Kontrollera **Ja, jag godkänner**, och klicka sedan på **skapa programmet Twitter**.</span><span class="sxs-lookup"><span data-stu-id="7799b-163">Check **Yes, I agree**, and then click **Create your Twitter application**.</span></span>
5. <span data-ttu-id="7799b-164">Klicka på den **behörigheter** fliken.</span><span class="sxs-lookup"><span data-stu-id="7799b-164">Click the **Permissions** tab.</span></span> <span data-ttu-id="7799b-165">Standardbehörigheten är **skrivskyddad**.</span><span class="sxs-lookup"><span data-stu-id="7799b-165">The default permission is **Read only**.</span></span> <span data-ttu-id="7799b-166">Detta är tillräcklig för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="7799b-166">This is sufficient for this tutorial.</span></span>
6. <span data-ttu-id="7799b-167">Klicka på den **nycklar och åtkomst-token** fliken.</span><span class="sxs-lookup"><span data-stu-id="7799b-167">Click the **Keys and Access Tokens** tab.</span></span>
7. <span data-ttu-id="7799b-168">Klicka på **skapa åtkomst-token**.</span><span class="sxs-lookup"><span data-stu-id="7799b-168">Click **Create my access token**.</span></span>
8. <span data-ttu-id="7799b-169">Klicka på **Test OAuth** i det övre högra hörnet på sidan.</span><span class="sxs-lookup"><span data-stu-id="7799b-169">Click **Test OAuth** in the upper-right corner of the page.</span></span>
9. <span data-ttu-id="7799b-170">Skriv ned **konsumenten nyckeln**, **konsumenthemlighet**, **åtkomsttoken**, och **åtkomst-token hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="7799b-170">Write down **consumer key**, **Consumer secret**, **Access token**, and **Access token secret**.</span></span> <span data-ttu-id="7799b-171">Du behöver värdena senare under kursen.</span><span class="sxs-lookup"><span data-stu-id="7799b-171">You will need the values later in the tutorial.</span></span>

<span data-ttu-id="7799b-172">I den här självstudiekursen kommer du använda Windows PowerShell för att anropa webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7799b-172">In this tutorial, you will use Windows PowerShell to make the web service call.</span></span> <span data-ttu-id="7799b-173">En .NET C# exempel hittar [analysera realtid Twitter-åsikter med HBase i HDInsight][hdinsight-hbase-twitter-sentiment].</span><span class="sxs-lookup"><span data-stu-id="7799b-173">For a .NET C# sample, see [Analyze real-time Twitter sentiment with HBase in HDInsight][hdinsight-hbase-twitter-sentiment].</span></span> <span data-ttu-id="7799b-174">Andra populära verktyget göra webbtjänstanrop [ *Curl*][curl].</span><span class="sxs-lookup"><span data-stu-id="7799b-174">The other popular tool to make web service calls is [*Curl*][curl].</span></span> <span data-ttu-id="7799b-175">CURL kan hämtas från [här][curl-download].</span><span class="sxs-lookup"><span data-stu-id="7799b-175">Curl can be downloaded from [here][curl-download].</span></span>

> [!NOTE]
> <span data-ttu-id="7799b-176">När du använder curl-kommando i Windows, Använd dubbla citattecken i stället för enkla citattecken för värden för alternativ.</span><span class="sxs-lookup"><span data-stu-id="7799b-176">When you use the curl command in Windows, use double quotes instead of single quotes for the option values.</span></span>

<span data-ttu-id="7799b-177">**Få tweets**</span><span class="sxs-lookup"><span data-stu-id="7799b-177">**To get tweets**</span></span>

1. <span data-ttu-id="7799b-178">Öppna Windows PowerShell Integrated Scripting Environment (ISE).</span><span class="sxs-lookup"><span data-stu-id="7799b-178">Open the Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="7799b-179">(Ange på startskärmen för Windows 8 **PowerShell_ISE** och klicka sedan på **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="7799b-179">(On the Windows 8 Start screen, type **PowerShell_ISE** and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="7799b-180">Se [starta Windows PowerShell på Windows 8 och Windows][powershell-start].)</span><span class="sxs-lookup"><span data-stu-id="7799b-180">See [Start Windows PowerShell on Windows 8 and Windows][powershell-start].)</span></span>
2. <span data-ttu-id="7799b-181">Kopiera följande skript i skriptfönstret:</span><span class="sxs-lookup"><span data-stu-id="7799b-181">Copy the following script into the script pane:</span></span>

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

3. <span data-ttu-id="7799b-182">Ange först fem till åtta variabler i skriptet:</span><span class="sxs-lookup"><span data-stu-id="7799b-182">Set the first five to eight variables in the script:</span></span>

    <span data-ttu-id="7799b-183">Variabel</span><span class="sxs-lookup"><span data-stu-id="7799b-183">Variable</span></span>|<span data-ttu-id="7799b-184">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7799b-184">Description</span></span>
    ---|---
    <span data-ttu-id="7799b-185">$clusterName</span><span class="sxs-lookup"><span data-stu-id="7799b-185">$clusterName</span></span>|<span data-ttu-id="7799b-186">Detta är namnet på HDInsight-klustret där du vill köra programmet.</span><span class="sxs-lookup"><span data-stu-id="7799b-186">This is the name of the HDInsight cluster where you want to run the application.</span></span>
    <span data-ttu-id="7799b-187">$oauth_consumer_key</span><span class="sxs-lookup"><span data-stu-id="7799b-187">$oauth_consumer_key</span></span>|<span data-ttu-id="7799b-188">Detta är det Twitter-programmet **konsumenten nyckeln** du skrivit tidigare när du skapade Twitter-programmet.</span><span class="sxs-lookup"><span data-stu-id="7799b-188">This is the Twitter application **consumer key** you wrote down earlier when you created the Twitter application.</span></span>
    <span data-ttu-id="7799b-189">$oauth_consumer_secret</span><span class="sxs-lookup"><span data-stu-id="7799b-189">$oauth_consumer_secret</span></span>|<span data-ttu-id="7799b-190">Detta är det Twitter-programmet **konsumenthemlighet** du skrev ned tidigare.</span><span class="sxs-lookup"><span data-stu-id="7799b-190">This is the Twitter application **consumer secret** you wrote down earlier.</span></span>
    <span data-ttu-id="7799b-191">$oauth_token</span><span class="sxs-lookup"><span data-stu-id="7799b-191">$oauth_token</span></span>|<span data-ttu-id="7799b-192">Detta är det Twitter-programmet **åtkomsttoken** du skrev ned tidigare.</span><span class="sxs-lookup"><span data-stu-id="7799b-192">This is the Twitter application **access token** you wrote down earlier.</span></span>
    <span data-ttu-id="7799b-193">$oauth_token_secret</span><span class="sxs-lookup"><span data-stu-id="7799b-193">$oauth_token_secret</span></span>|<span data-ttu-id="7799b-194">Detta är det Twitter-programmet **åtkomst-token hemlighet** du skrev ned tidigare.</span><span class="sxs-lookup"><span data-stu-id="7799b-194">This is the Twitter application **access token secret** you wrote down earlier.</span></span>
    <span data-ttu-id="7799b-195">$destBlobName</span><span class="sxs-lookup"><span data-stu-id="7799b-195">$destBlobName</span></span>|<span data-ttu-id="7799b-196">Detta är blob utdatanamn.</span><span class="sxs-lookup"><span data-stu-id="7799b-196">This is the output blob name.</span></span> <span data-ttu-id="7799b-197">Standardvärdet är **tutorials/twitter/data/tweets.txt**.</span><span class="sxs-lookup"><span data-stu-id="7799b-197">The default value is **tutorials/twitter/data/tweets.txt**.</span></span> <span data-ttu-id="7799b-198">Om du ändrar standardvärdet, behöver du uppdateras också Windows PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="7799b-198">If you change the default value, you will need to update the Windows PowerShell scripts accordingly.</span></span>
    <span data-ttu-id="7799b-199">$trackString</span><span class="sxs-lookup"><span data-stu-id="7799b-199">$trackString</span></span>|<span data-ttu-id="7799b-200">Webbtjänsten returneras tweets som rör nyckelorden.</span><span class="sxs-lookup"><span data-stu-id="7799b-200">The web service will return tweets related to these keywords.</span></span> <span data-ttu-id="7799b-201">Standardvärdet är **Azure moln, HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="7799b-201">The default value is **Azure, Cloud, HDInsight**.</span></span> <span data-ttu-id="7799b-202">Om du ändrar standardvärdet, uppdaterar du Windows PowerShell-skript i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="7799b-202">If you change the default value, you will update the Windows PowerShell scripts accordingly.</span></span>
    <span data-ttu-id="7799b-203">$lineMax</span><span class="sxs-lookup"><span data-stu-id="7799b-203">$lineMax</span></span>|<span data-ttu-id="7799b-204">Värdet anger hur många tweets skriptet läses.</span><span class="sxs-lookup"><span data-stu-id="7799b-204">The value determines how many tweets the script will read.</span></span> <span data-ttu-id="7799b-205">Det tar cirka tre minuter för att läsa 100 tweets.</span><span class="sxs-lookup"><span data-stu-id="7799b-205">It takes about three minutes to read 100 tweets.</span></span> <span data-ttu-id="7799b-206">Du kan ange ett större värde, men det tar längre tid att hämta.</span><span class="sxs-lookup"><span data-stu-id="7799b-206">You can set a larger number, but it will take more time to download.</span></span>

1. <span data-ttu-id="7799b-207">Tryck **F5** för att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="7799b-207">Press **F5** to run the script.</span></span> <span data-ttu-id="7799b-208">Om du stöter på problem, som en tillfällig lösning kan du markera alla rader och tryck sedan på **F8**.</span><span class="sxs-lookup"><span data-stu-id="7799b-208">If you run into problems, as a workaround, select all the lines, and then press **F8**.</span></span>
2. <span data-ttu-id="7799b-209">Du bör se ”Slutför”!</span><span class="sxs-lookup"><span data-stu-id="7799b-209">You shall see "Complete!"</span></span> <span data-ttu-id="7799b-210">i slutet av utdata.</span><span class="sxs-lookup"><span data-stu-id="7799b-210">at the end of the output.</span></span> <span data-ttu-id="7799b-211">Eventuella felmeddelanden visas i rött.</span><span class="sxs-lookup"><span data-stu-id="7799b-211">Any error messages will be displayed in red.</span></span>

<span data-ttu-id="7799b-212">Du kan kontrollera filen, som en valideringsproceduren **/tutorials/twitter/data/tweets.txt**, på ditt Azure Blob storage med hjälp av en Azure Lagringsutforskaren eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7799b-212">As a validation procedure, you can check the output file, **/tutorials/twitter/data/tweets.txt**, on your Azure Blob storage by using an Azure storage explorer or Azure PowerShell.</span></span> <span data-ttu-id="7799b-213">En Windows PowerShell-exempelskript för att lista filer, se [använda Blob storage med HDInsight][hdinsight-storage-powershell].</span><span class="sxs-lookup"><span data-stu-id="7799b-213">For a sample Windows PowerShell script for listing files, see [Use Blob storage with HDInsight][hdinsight-storage-powershell].</span></span>

## <a name="create-hiveql-script"></a><span data-ttu-id="7799b-214">Skapa HiveQL-skript</span><span class="sxs-lookup"><span data-stu-id="7799b-214">Create HiveQL script</span></span>
<span data-ttu-id="7799b-215">Med Azure PowerShell kan du köra flera HiveQL-instruktioner en i taget eller paketet HiveQL-instruktionen i en skriptfil.</span><span class="sxs-lookup"><span data-stu-id="7799b-215">Using Azure PowerShell, you can run multiple HiveQL statements one at a time, or package the HiveQL statement into a script file.</span></span> <span data-ttu-id="7799b-216">I den här självstudiekursen skapar du en HiveQL-skript.</span><span class="sxs-lookup"><span data-stu-id="7799b-216">In this tutorial, you will create a HiveQL script.</span></span> <span data-ttu-id="7799b-217">Skriptfilen måste överföras till Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="7799b-217">The script file must be uploaded to Azure Blob storage.</span></span> <span data-ttu-id="7799b-218">I nästa avsnitt ska du köra skriptfilen med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7799b-218">In the next section, you will run the script file by using Azure PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="7799b-219">Hive skriptfilen och en fil som innehåller 10 000 tweets har laddats upp i en offentlig blobbbehållare.</span><span class="sxs-lookup"><span data-stu-id="7799b-219">The Hive script file and a file containing 10,000 tweets have been uploaded in a public Blob container.</span></span> <span data-ttu-id="7799b-220">Du kan hoppa över det här avsnittet om du vill använda de överförda filerna.</span><span class="sxs-lookup"><span data-stu-id="7799b-220">You can skip this section if you want to use the uploaded files.</span></span>

<span data-ttu-id="7799b-221">HiveQL-skript kommer att utföra följande:</span><span class="sxs-lookup"><span data-stu-id="7799b-221">The HiveQL script will perform the following:</span></span>

1. <span data-ttu-id="7799b-222">**Ta bort tabellen tweets_raw** om tabellen redan finns.</span><span class="sxs-lookup"><span data-stu-id="7799b-222">**Drop the tweets_raw table** in case the table already exists.</span></span>
2. <span data-ttu-id="7799b-223">**Skapa Hive-tabell tweets_raw**.</span><span class="sxs-lookup"><span data-stu-id="7799b-223">**Create the tweets_raw Hive table**.</span></span> <span data-ttu-id="7799b-224">Hive strukturerade registret innehåller data för ytterligare extrahering, transformering och laddning (ETL) bearbetning.</span><span class="sxs-lookup"><span data-stu-id="7799b-224">This temporary Hive structured table holds the data for further extract, transform, and load (ETL) processing.</span></span> <span data-ttu-id="7799b-225">Mer information om partitioner finns [Hive kursen][apache-hive-tutorial].</span><span class="sxs-lookup"><span data-stu-id="7799b-225">For information on partitions, see [Hive tutorial][apache-hive-tutorial].</span></span>
3. <span data-ttu-id="7799b-226">**Läs in data** från källmappen /tutorials/twitter/data.</span><span class="sxs-lookup"><span data-stu-id="7799b-226">**Load data** from the source folder, /tutorials/twitter/data.</span></span> <span data-ttu-id="7799b-227">Stora tweets datauppsättningen i kapslade JSON-format har nu tagits omvandlas till en tillfällig Hive tabellstruktur.</span><span class="sxs-lookup"><span data-stu-id="7799b-227">The large tweets dataset in nested JSON format has now been transformed into a temporary Hive table structure.</span></span>
4. <span data-ttu-id="7799b-228">**Ta bort tabellen tweets** om tabellen redan finns.</span><span class="sxs-lookup"><span data-stu-id="7799b-228">**Drop the tweets table** in case the table already exists.</span></span>
5. <span data-ttu-id="7799b-229">**Skapa tabellen tweets**.</span><span class="sxs-lookup"><span data-stu-id="7799b-229">**Create the tweets table**.</span></span> <span data-ttu-id="7799b-230">Innan du kan fråga mot tweets dataset med hjälp av Hive, måste du kör en annan ETL-processen.</span><span class="sxs-lookup"><span data-stu-id="7799b-230">Before you can query against the tweets dataset by using Hive, you need to run another ETL process.</span></span> <span data-ttu-id="7799b-231">ETL-processen definierar en mer detaljerad tabellschemat för de data som du har lagrat i tabellen ”twitter_raw”.</span><span class="sxs-lookup"><span data-stu-id="7799b-231">This ETL process defines a more detailed table schema for the data that you have stored in the "twitter_raw" table.</span></span>
6. <span data-ttu-id="7799b-232">**Skriv över för infoga**.</span><span class="sxs-lookup"><span data-stu-id="7799b-232">**Insert overwrite table**.</span></span> <span data-ttu-id="7799b-233">Det här komplexa Hive-skriptet kommer startar en uppsättning lång MapReduce-jobb med Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="7799b-233">This complex Hive script will kick off a set of long MapReduce jobs by the Hadoop cluster.</span></span> <span data-ttu-id="7799b-234">Detta kan ta ungefär 10 minuter beroende på datamängden och storleken på ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="7799b-234">Depending on your dataset and the size of your cluster, this could take about 10 minutes.</span></span>
7. <span data-ttu-id="7799b-235">**Infoga skriva över katalogen**.</span><span class="sxs-lookup"><span data-stu-id="7799b-235">**Insert overwrite directory**.</span></span> <span data-ttu-id="7799b-236">Kör en fråga och utdatauppsättningen till en fil.</span><span class="sxs-lookup"><span data-stu-id="7799b-236">Run a query and output the dataset to a file.</span></span> <span data-ttu-id="7799b-237">Den här frågan returnerar en lista över Twitter-användare som skickade de flesta tweets som innehåller ordet ”Azure”.</span><span class="sxs-lookup"><span data-stu-id="7799b-237">This query will return a list of Twitter users who sent most tweets that contained the word "Azure".</span></span>

<span data-ttu-id="7799b-238">**Skapa en Hive-skript och överföra det till Azure**</span><span class="sxs-lookup"><span data-stu-id="7799b-238">**To create a Hive script and upload it to Azure**</span></span>

1. <span data-ttu-id="7799b-239">Öppna Windows PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="7799b-239">Open Windows PowerShell ISE.</span></span>
2. <span data-ttu-id="7799b-240">Kopiera följande skript i skriptfönstret:</span><span class="sxs-lookup"><span data-stu-id="7799b-240">Copy the following script into the script pane:</span></span>

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

3. <span data-ttu-id="7799b-241">Ange de första två variablerna i skriptet:</span><span class="sxs-lookup"><span data-stu-id="7799b-241">Set the first two variables in the script:</span></span>

   | <span data-ttu-id="7799b-242">Variabel</span><span class="sxs-lookup"><span data-stu-id="7799b-242">Variable</span></span> | <span data-ttu-id="7799b-243">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7799b-243">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="7799b-244">$clusterName</span><span class="sxs-lookup"><span data-stu-id="7799b-244">$clusterName</span></span> |<span data-ttu-id="7799b-245">Ange klusternamnet HDInsight där du vill köra programmet.</span><span class="sxs-lookup"><span data-stu-id="7799b-245">Enter the HDInsight cluster name where you want to run the application.</span></span> |
   |  <span data-ttu-id="7799b-246">$subscriptionID</span><span class="sxs-lookup"><span data-stu-id="7799b-246">$subscriptionID</span></span> |<span data-ttu-id="7799b-247">Ange ditt Azure-prenumeration-ID.</span><span class="sxs-lookup"><span data-stu-id="7799b-247">Enter your Azure subscription ID.</span></span> |
   |  <span data-ttu-id="7799b-248">$sourceDataPath</span><span class="sxs-lookup"><span data-stu-id="7799b-248">$sourceDataPath</span></span> |<span data-ttu-id="7799b-249">Azure Blob storage plats där Hive-frågor kommer att läsa data från.</span><span class="sxs-lookup"><span data-stu-id="7799b-249">The Azure Blob storage location where the Hive queries will read the data from.</span></span> <span data-ttu-id="7799b-250">Du behöver inte ändra den här variabeln.</span><span class="sxs-lookup"><span data-stu-id="7799b-250">You don't need to change this variable.</span></span> |
   |  <span data-ttu-id="7799b-251">$outputPath</span><span class="sxs-lookup"><span data-stu-id="7799b-251">$outputPath</span></span> |<span data-ttu-id="7799b-252">Azure Blob storage plats där Hive-frågor kommer skriver resultatet.</span><span class="sxs-lookup"><span data-stu-id="7799b-252">The Azure Blob storage location where the Hive queries will output the results.</span></span> <span data-ttu-id="7799b-253">Du behöver inte ändra den här variabeln.</span><span class="sxs-lookup"><span data-stu-id="7799b-253">You don't need to change this variable.</span></span> |
   |  <span data-ttu-id="7799b-254">$hqlScriptFile</span><span class="sxs-lookup"><span data-stu-id="7799b-254">$hqlScriptFile</span></span> |<span data-ttu-id="7799b-255">Sökvägen och filnamnet för filen HiveQL-skript.</span><span class="sxs-lookup"><span data-stu-id="7799b-255">The location and the file name of the HiveQL script file.</span></span> <span data-ttu-id="7799b-256">Du behöver inte ändra den här variabeln.</span><span class="sxs-lookup"><span data-stu-id="7799b-256">You don't need to change this variable.</span></span> |
4. <span data-ttu-id="7799b-257">Tryck **F5** för att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="7799b-257">Press **F5** to run the script.</span></span> <span data-ttu-id="7799b-258">Om du stöter på problem, som en tillfällig lösning kan du markera alla rader och tryck sedan på **F8**.</span><span class="sxs-lookup"><span data-stu-id="7799b-258">If you run into problems, as a workaround, select all the lines, and then press **F8**.</span></span>
5. <span data-ttu-id="7799b-259">Du bör se ”Slutför”!</span><span class="sxs-lookup"><span data-stu-id="7799b-259">You shall see "Complete!"</span></span> <span data-ttu-id="7799b-260">i slutet av utdata.</span><span class="sxs-lookup"><span data-stu-id="7799b-260">at the end of the output.</span></span> <span data-ttu-id="7799b-261">Eventuella felmeddelanden visas i rött.</span><span class="sxs-lookup"><span data-stu-id="7799b-261">Any error messages will be displayed in red.</span></span>

<span data-ttu-id="7799b-262">Du kan kontrollera filen, som en valideringsproceduren **/tutorials/twitter/twitter.hql**, på ditt Azure Blob storage med hjälp av en Azure Lagringsutforskaren eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7799b-262">As a validation procedure, you can check the output file, **/tutorials/twitter/twitter.hql**, on your Azure Blob storage by using an Azure storage explorer or Azure PowerShell.</span></span> <span data-ttu-id="7799b-263">En Windows PowerShell-exempelskript för att lista filer, se [använda Blob storage med HDInsight][hdinsight-storage-powershell].</span><span class="sxs-lookup"><span data-stu-id="7799b-263">For a sample Windows PowerShell script for listing files, see [Use Blob storage with HDInsight][hdinsight-storage-powershell].</span></span>

## <a name="process-twitter-data-by-using-hive"></a><span data-ttu-id="7799b-264">Bearbeta Twitter-data med hjälp av Hive</span><span class="sxs-lookup"><span data-stu-id="7799b-264">Process Twitter data by using Hive</span></span>
<span data-ttu-id="7799b-265">Du är klar med alla förberedelse av arbetet.</span><span class="sxs-lookup"><span data-stu-id="7799b-265">You have finished all the preparation work.</span></span> <span data-ttu-id="7799b-266">Nu kan du anropa Hive-skript och kontrollera resultaten.</span><span class="sxs-lookup"><span data-stu-id="7799b-266">Now, you can invoke the Hive script and check the results.</span></span>

### <a name="submit-a-hive-job"></a><span data-ttu-id="7799b-267">Skicka ett Hive-jobb</span><span class="sxs-lookup"><span data-stu-id="7799b-267">Submit a Hive job</span></span>
<span data-ttu-id="7799b-268">Använd följande Windows PowerShell-skript för att köra Hive-skript.</span><span class="sxs-lookup"><span data-stu-id="7799b-268">Use the following Windows PowerShell script to run the Hive script.</span></span> <span data-ttu-id="7799b-269">Du måste ange den första variabeln.</span><span class="sxs-lookup"><span data-stu-id="7799b-269">You will need to set the first variable.</span></span>

> [!NOTE]
> <span data-ttu-id="7799b-270">Om du vill använda tweets och HiveQL-skript som du har överfört i de sista två avsnitt, tilldelas $hqlScriptFile ”/ tutorials/twitter/twitter.hql”.</span><span class="sxs-lookup"><span data-stu-id="7799b-270">To use the tweets and the HiveQL script you uploaded in the last two sections, set $hqlScriptFile to "/tutorials/twitter/twitter.hql".</span></span> <span data-ttu-id="7799b-271">Om du vill använda de som har överförts till en offentlig blob du värdet $hqlScriptFile ”wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql”.</span><span class="sxs-lookup"><span data-stu-id="7799b-271">To use the ones that have been uploaded to a public blob for you, set $hqlScriptFile to "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".</span></span>

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

### <a name="check-the-results"></a><span data-ttu-id="7799b-272">Kontrollera resultaten</span><span class="sxs-lookup"><span data-stu-id="7799b-272">Check the results</span></span>
<span data-ttu-id="7799b-273">Använd följande Windows PowerShell-skript för att kontrollera utdata för Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="7799b-273">Use the following Windows PowerShell script to check the Hive job output.</span></span> <span data-ttu-id="7799b-274">Du måste ställa in de första två variablerna.</span><span class="sxs-lookup"><span data-stu-id="7799b-274">You will need to set the first two variables.</span></span>

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
> Hive-tabell använder \001 som fältavgränsaren. <span data-ttu-id="7799b-276">Avgränsaren visas inte i utdata.</span><span class="sxs-lookup"><span data-stu-id="7799b-276">The delimiter is not visible in the output.</span></span>

<span data-ttu-id="7799b-277">När de har placerats i Azure Blob storage, kan du exportera data till en Azure SQL-databas/SQL server, exportera data till Excel med hjälp av Power Query eller ansluta programmet till data med hjälp av Hive ODBC-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="7799b-277">After the analysis results have been placed in Azure Blob storage, you can export the data to an Azure SQL database/SQL server, export the data to Excel by using Power Query, or connect your application to the data by using the Hive ODBC Driver.</span></span> <span data-ttu-id="7799b-278">Mer information finns i [använda Sqoop med HDInsight][hdinsight-use-sqoop], [analysera svarta fördröjning data med HDInsight][hdinsight-analyze-flight-delay-data], [ Anslut Excel till HDInsight med Power Query][hdinsight-power-query], och [ansluter Excel till HDInsight med Microsoft Hive ODBC-drivrutinen][hdinsight-hive-odbc].</span><span class="sxs-lookup"><span data-stu-id="7799b-278">For more information, see [Use Sqoop with HDInsight][hdinsight-use-sqoop], [Analyze flight delay data using HDInsight][hdinsight-analyze-flight-delay-data], [Connect Excel to HDInsight with Power Query][hdinsight-power-query], and [Connect Excel to HDInsight with the Microsoft Hive ODBC Driver][hdinsight-hive-odbc].</span></span>

## <a name="next-steps"></a><span data-ttu-id="7799b-279">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7799b-279">Next steps</span></span>
<span data-ttu-id="7799b-280">I den här självstudiekursen har vi sett hur att omvandla en ostrukturerad datauppsättning JSON till en strukturerad Hive-tabell för att fråga, utforska och analysera data från Twitter med HDInsight på Azure.</span><span class="sxs-lookup"><span data-stu-id="7799b-280">In this tutorial we have seen how to transform an unstructured JSON dataset into a structured Hive table to query, explore, and analyze data from Twitter by using HDInsight on Azure.</span></span> <span data-ttu-id="7799b-281">Du kan läsa mer här:</span><span class="sxs-lookup"><span data-stu-id="7799b-281">To learn more, see:</span></span>

* <span data-ttu-id="7799b-282">[Komma igång med HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="7799b-282">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="7799b-283">[Analysera realtid Twitter-åsikter med HBase i HDInsight][hdinsight-hbase-twitter-sentiment]</span><span class="sxs-lookup"><span data-stu-id="7799b-283">[Analyze real-time Twitter sentiment with HBase in HDInsight][hdinsight-hbase-twitter-sentiment]</span></span>
* <span data-ttu-id="7799b-284">[Analysera svarta fördröjning data med HDInsight][hdinsight-analyze-flight-delay-data]</span><span class="sxs-lookup"><span data-stu-id="7799b-284">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-delay-data]</span></span>
* <span data-ttu-id="7799b-285">[Anslut Excel till HDInsight med Power Query][hdinsight-power-query]</span><span class="sxs-lookup"><span data-stu-id="7799b-285">[Connect Excel to HDInsight with Power Query][hdinsight-power-query]</span></span>
* <span data-ttu-id="7799b-286">[Anslut Excel till HDInsight med Microsoft Hive ODBC-drivrutin][hdinsight-hive-odbc]</span><span class="sxs-lookup"><span data-stu-id="7799b-286">[Connect Excel to HDInsight with the Microsoft Hive ODBC Driver][hdinsight-hive-odbc]</span></span>
* <span data-ttu-id="7799b-287">[Använda Sqoop med HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="7799b-287">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>

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
