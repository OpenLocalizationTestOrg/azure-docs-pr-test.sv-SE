---
title: "aaaUse Hadoop Pig med övriga i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse REST toorun Pig Latin jobb på en Hadoop-kluster i Azure HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ed5e10d1-4f47-459c-a0d6-7ff967b468c4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 760139e3caad9103d8c9d34e7f548d476014b5ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-rest"></a><span data-ttu-id="03221-103">Köra Pig med Hadoop på HDInsight med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="03221-103">Run Pig jobs with Hadoop on HDInsight by using REST</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="03221-104">Lär dig hur toorun Pig Latin jobb genom att göra REST begäranden tooan Azure HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="03221-104">Learn how toorun Pig Latin jobs by making REST requests tooan Azure HDInsight cluster.</span></span> <span data-ttu-id="03221-105">CURL är används toodemonstrate hur du kan interagera med HDInsight med hello WebHCat REST API.</span><span class="sxs-lookup"><span data-stu-id="03221-105">Curl is used toodemonstrate how you can interact with HDInsight using hello WebHCat REST API.</span></span>

> [!NOTE]
> <span data-ttu-id="03221-106">Om du redan är bekant med Linux-baserade Hadoop-servrar, men nya tooHDInsight, se [Linux-baserade HDInsight-Tips](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="03221-106">If you are already familiar with using Linux-based Hadoop servers, but are new tooHDInsight, see [Linux-based HDInsight Tips](hdinsight-hadoop-linux-information.md).</span></span>

## <span data-ttu-id="03221-107"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="03221-107"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="03221-108">Ett Azure HDInsight (Hadoop på HDInsight)-kluster (Linux- eller Windows-baserade)</span><span class="sxs-lookup"><span data-stu-id="03221-108">An Azure HDInsight (Hadoop on HDInsight) cluster (Linux-based or Windows-based)</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="03221-109">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="03221-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="03221-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="03221-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* [<span data-ttu-id="03221-111">CURL</span><span class="sxs-lookup"><span data-stu-id="03221-111">Curl</span></span>](http://curl.haxx.se/)

* [<span data-ttu-id="03221-112">jq</span><span class="sxs-lookup"><span data-stu-id="03221-112">jq</span></span>](http://stedolan.github.io/jq/)

## <span data-ttu-id="03221-113"><a id="curl"></a>Köra Pig-jobb med Curl</span><span class="sxs-lookup"><span data-stu-id="03221-113"><a id="curl"></a>Run Pig jobs by using Curl</span></span>

> [!NOTE]
> <span data-ttu-id="03221-114">hello REST API skyddas via [grundläggande autentisering](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="03221-114">hello REST API is secured via [basic access authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="03221-115">Alltid göra begäranden genom att använda säker HTTP (HTTPS) tooensure att dina autentiseringsuppgifter skickas toohello server på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="03221-115">Always make requests by using Secure HTTP (HTTPS) tooensure that your credentials are securely sent toohello server.</span></span>
>
> <span data-ttu-id="03221-116">När du använder hello kommandon i det här avsnittet, ersätter `USERNAME` med hello användaren tooauthenticate toohello kluster och Ersätt `PASSWORD` med hello lösenordet för användarkontot för hello.</span><span class="sxs-lookup"><span data-stu-id="03221-116">When using hello commands in this section, replace `USERNAME` with hello user tooauthenticate toohello cluster, and replace `PASSWORD` with hello password for hello user account.</span></span> <span data-ttu-id="03221-117">Ersätt `CLUSTERNAME` med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="03221-117">Replace `CLUSTERNAME` with hello name of your cluster.</span></span>
>


1. <span data-ttu-id="03221-118">Använd hello efter kommandot tooverify att du kan ansluta tooyour HDInsight-kluster från en kommandorad:</span><span class="sxs-lookup"><span data-stu-id="03221-118">From a command line, use hello following command tooverify that you can connect tooyour HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="03221-119">Du bör få hello följande JSON-svar:</span><span class="sxs-lookup"><span data-stu-id="03221-119">You should receive hello following JSON response:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="03221-120">hello-parametrar som används i det här kommandot är följande:</span><span class="sxs-lookup"><span data-stu-id="03221-120">hello parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="03221-121">**-u**: hello användarnamn och lösenord används tooauthenticate hello begäran</span><span class="sxs-lookup"><span data-stu-id="03221-121">**-u**: hello user name and password used tooauthenticate hello request</span></span>
    * <span data-ttu-id="03221-122">**-G**: Anger att begäran är en GET-begäran</span><span class="sxs-lookup"><span data-stu-id="03221-122">**-G**: Indicates that this request is a GET request</span></span>

     <span data-ttu-id="03221-123">Hej i början av hello URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, hello samma för alla begäranden.</span><span class="sxs-lookup"><span data-stu-id="03221-123">hello beginning of hello URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hello same for all requests.</span></span> <span data-ttu-id="03221-124">hello sökvägen **/status**, visar hello begäran är tooreturn hello status för WebHCat (även kallat Templeton) för hello-servern.</span><span class="sxs-lookup"><span data-stu-id="03221-124">hello path, **/status**, indicates that hello request is tooreturn hello status of WebHCat (also known as Templeton) for hello server.</span></span>

2. <span data-ttu-id="03221-125">Använd följande kod toosubmit ett Pig Latin jobbet toohello kluster hello:</span><span class="sxs-lookup"><span data-stu-id="03221-125">Use hello following code toosubmit a Pig Latin job toohello cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
    ```

    <span data-ttu-id="03221-126">hello-parametrar som används i det här kommandot är följande:</span><span class="sxs-lookup"><span data-stu-id="03221-126">hello parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="03221-127">**-d**: eftersom `-G` används inte hello begäran standardvärden toohello POST-metoden.</span><span class="sxs-lookup"><span data-stu-id="03221-127">**-d**: Because `-G` is not used, hello request defaults toohello POST method.</span></span> <span data-ttu-id="03221-128">`-d`Anger hello datavärden som skickas med hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="03221-128">`-d` specifies hello data values that are sent with hello request.</span></span>

    * <span data-ttu-id="03221-129">**User.name**: hello-användaren som kör hello kommando</span><span class="sxs-lookup"><span data-stu-id="03221-129">**user.name**: hello user who is running hello command</span></span>
    * <span data-ttu-id="03221-130">**köra**: hello Pig Latin instruktioner tooexecute</span><span class="sxs-lookup"><span data-stu-id="03221-130">**execute**: hello Pig Latin statements tooexecute</span></span>
    * <span data-ttu-id="03221-131">**statusdir**: hello-katalog som hello status för jobbet skrivs till</span><span class="sxs-lookup"><span data-stu-id="03221-131">**statusdir**: hello directory that hello status for this job is written to</span></span>

    > [!NOTE]
    > <span data-ttu-id="03221-132">Observera att hello blanksteg i Pig Latin rapporterna har ersatts av hello `+` tecken när det används med Curl.</span><span class="sxs-lookup"><span data-stu-id="03221-132">Notice that hello spaces in Pig Latin statements are replaced by hello `+` character when used with Curl.</span></span>

    <span data-ttu-id="03221-133">Det här kommandot ska returnera ett jobb-ID som kan använda toocheck hello status för hello jobb, till exempel:</span><span class="sxs-lookup"><span data-stu-id="03221-133">This command should return a job ID that can be used toocheck hello status of hello job, for example:</span></span>

        {"id":"job_1415651640909_0026"}

3. <span data-ttu-id="03221-134">toocheck hello status för hello jobb, Använd hello följande kommando</span><span class="sxs-lookup"><span data-stu-id="03221-134">toocheck hello status of hello job, use hello following command</span></span>

     ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

     <span data-ttu-id="03221-135">Ersätt `JOBID` med hello-värde som returneras i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="03221-135">Replace `JOBID` with hello value returned in hello previous step.</span></span> <span data-ttu-id="03221-136">Till exempel om hello returnera värdet var `{"id":"job_1415651640909_0026"}`, sedan `JOBID` är `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="03221-136">For example, if hello return value was `{"id":"job_1415651640909_0026"}`, then `JOBID` is `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="03221-137">Om hello jobbet har slutförts, hello tillstånd är **lyckades**.</span><span class="sxs-lookup"><span data-stu-id="03221-137">If hello job has finished, hello state is **SUCCEEDED**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="03221-138">Den här Curl-begäran returnerar en JavaScript Object Notation (JSON) dokument med information om hello jobb och jq är används tooretrieve hello endast värdet state.</span><span class="sxs-lookup"><span data-stu-id="03221-138">This Curl request returns a JavaScript Object Notation (JSON) document with information about hello job, and jq is used tooretrieve only hello state value.</span></span>

## <span data-ttu-id="03221-139"><a id="results"></a>Visa resultat</span><span class="sxs-lookup"><span data-stu-id="03221-139"><a id="results"></a>View results</span></span>

<span data-ttu-id="03221-140">När hello tillståndet för hello jobbet har ändrats för**lyckades**, du kan hämta hello resultatet av hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="03221-140">When hello state of hello job has changed too**SUCCEEDED**, you can retrieve hello results of hello job.</span></span> <span data-ttu-id="03221-141">Hej `statusdir` -parameter som överförs med hello frågan innehåller hello platsen hello utdatafilen; i det här fallet `/example/pigcurl`.</span><span class="sxs-lookup"><span data-stu-id="03221-141">hello `statusdir` parameter passed with hello query contains hello location of hello output file; in this case, `/example/pigcurl`.</span></span>

<span data-ttu-id="03221-142">HDInsight kan använda Azure Storage eller Azure Data Lake Store som hello standard datalager.</span><span class="sxs-lookup"><span data-stu-id="03221-142">HDInsight can use either Azure Storage or Azure Data Lake Store as hello default data store.</span></span> <span data-ttu-id="03221-143">Det finns olika sätt tooget vid hello data beroende på vilket som du använder.</span><span class="sxs-lookup"><span data-stu-id="03221-143">There are various ways tooget at hello data depending on which one you use.</span></span> <span data-ttu-id="03221-144">Mer information finns i avsnittet för hello lagring av hello [Linux-baserat HDInsight information](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="03221-144">For more information, see hello storage section of hello [Linux-based HDInsight information](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) document.</span></span>

## <span data-ttu-id="03221-145"><a id="summary"></a>Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="03221-145"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="03221-146">Som visas i det här dokumentet, kan du använda en rå HTTP-begäran toorun, övervaka och visa hello resultat av Pig-jobb på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="03221-146">As demonstrated in this document, you can use a raw HTTP request toorun, monitor, and view hello results of Pig jobs on your HDInsight cluster.</span></span>

<span data-ttu-id="03221-147">Mer information om hello REST-gränssnitt som används i den här artikeln finns hello [WebHCat referens](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span><span class="sxs-lookup"><span data-stu-id="03221-147">For more information about hello REST interface used in this article, see hello [WebHCat Reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span></span>

## <span data-ttu-id="03221-148"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="03221-148"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="03221-149">Allmän information om Pig på HDInsight:</span><span class="sxs-lookup"><span data-stu-id="03221-149">For general information about Pig on HDInsight:</span></span>

* [<span data-ttu-id="03221-150">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="03221-150">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="03221-151">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="03221-151">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="03221-152">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="03221-152">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="03221-153">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="03221-153">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
