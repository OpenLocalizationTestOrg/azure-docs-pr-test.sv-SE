---
title: aaaUse MapReduce och Curl med Hadoop i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur tooremotely kör MapReduce-jobb med Hadoop på HDInsight med Curl."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: bc6daf37-fcdc-467a-a8a8-6fb2f0f773d1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 16920205bacf9699f88090568099e0508a172b3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a><span data-ttu-id="8986c-103">Kör jobb för MapReduce med Hadoop i HDInsight med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="8986c-103">Run MapReduce jobs with Hadoop on HDInsight using REST</span></span>

<span data-ttu-id="8986c-104">Lär dig hur toouse hello WebHCat REST API toorun MapReduce-jobb på en Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8986c-104">Learn how toouse hello WebHCat REST API toorun MapReduce jobs on a Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="8986c-105">CURL är används toodemonstrate hur du kan interagera med HDInsight med hjälp av rådata HTTP-begäranden toorun MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="8986c-105">Curl is used toodemonstrate how you can interact with HDInsight by using raw HTTP requests toorun MapReduce jobs.</span></span>

> [!NOTE]
> <span data-ttu-id="8986c-106">Om du redan är bekant med Linux-baserade Hadoop-servrar, men du är ny tooHDInsight, se hello [vad du behöver tooknow om Linux-baserade Hadoop på HDInsight](hdinsight-hadoop-linux-information.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="8986c-106">If you are already familiar with using Linux-based Hadoop servers, but you are new tooHDInsight, see hello [What you need tooknow about Linux-based Hadoop on HDInsight](hdinsight-hadoop-linux-information.md) document.</span></span>


## <span data-ttu-id="8986c-107"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="8986c-107"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="8986c-108">En Hadoop på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="8986c-108">A Hadoop on HDInsight cluster</span></span>
* [<span data-ttu-id="8986c-109">CURL</span><span class="sxs-lookup"><span data-stu-id="8986c-109">Curl</span></span>](http://curl.haxx.se/)
* [<span data-ttu-id="8986c-110">jq</span><span class="sxs-lookup"><span data-stu-id="8986c-110">jq</span></span>](http://stedolan.github.io/jq/)

## <span data-ttu-id="8986c-111"><a id="curl"></a>Kör MapReduce-jobb med Curl</span><span class="sxs-lookup"><span data-stu-id="8986c-111"><a id="curl"></a>Run MapReduce jobs using Curl</span></span>

> [!NOTE]
> <span data-ttu-id="8986c-112">När du använder Curl eller annan REST-kommunikation med WebHCat, måste du autentisera hello begäranden genom att tillhandahålla hello HDInsight-kluster administratörsanvändarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="8986c-112">When you use Curl or any other REST communication with WebHCat, you must authenticate hello requests by providing hello HDInsight cluster administrator user name and password.</span></span> <span data-ttu-id="8986c-113">Som en del av hello URI som används toosend hello begäranden toohello server måste du använda hello klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="8986c-113">You must use hello cluster name as part of hello URI that is used toosend hello requests toohello server.</span></span>
>
> <span data-ttu-id="8986c-114">Hello kommandon i det här avsnittet, ersätter **användarnamn** med hello användaren tooauthenticate toohello kluster och **lösenord** med hello lösenordet för användarkontot för hello.</span><span class="sxs-lookup"><span data-stu-id="8986c-114">For hello commands in this section, replace **USERNAME** with hello user tooauthenticate toohello cluster, and **PASSWORD** with hello password for hello user account.</span></span> <span data-ttu-id="8986c-115">Ersätt **KLUSTERNAMN** med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="8986c-115">Replace **CLUSTERNAME** with hello name of your cluster.</span></span>
>
> <span data-ttu-id="8986c-116">hello REST API skyddas med hjälp av [grundläggande autentisering](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="8986c-116">hello REST API is secured by using [basic access authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="8986c-117">Du bör alltid göra begäranden genom att använda HTTPS tooensure att dina autentiseringsuppgifter skickas toohello server på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="8986c-117">You should always make requests by using HTTPS tooensure that your credentials are securely sent toohello server.</span></span>


1. <span data-ttu-id="8986c-118">Använd hello efter kommandot tooverify att du kan ansluta tooyour HDInsight-kluster från en kommandorad:</span><span class="sxs-lookup"><span data-stu-id="8986c-118">From a command line, use hello following command tooverify that you can connect tooyour HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="8986c-119">Du bör få ett svar liknande toohello följande JSON:</span><span class="sxs-lookup"><span data-stu-id="8986c-119">You should receive a response similar toohello following JSON:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="8986c-120">hello-parametrar som används i det här kommandot är följande:</span><span class="sxs-lookup"><span data-stu-id="8986c-120">hello parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="8986c-121">**-u**: Anger hello användarnamn och lösenord används tooauthenticate hello begäran</span><span class="sxs-lookup"><span data-stu-id="8986c-121">**-u**: Indicates hello user name and password used tooauthenticate hello request</span></span>
   * <span data-ttu-id="8986c-122">**-G**: Anger att den här åtgärden är en GET-begäran</span><span class="sxs-lookup"><span data-stu-id="8986c-122">**-G**: Indicates that this operation is a GET request</span></span>

     <span data-ttu-id="8986c-123">Hej i början av hello URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, hello samma för alla begäranden.</span><span class="sxs-lookup"><span data-stu-id="8986c-123">hello beginning of hello URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hello same for all requests.</span></span>

2. <span data-ttu-id="8986c-124">toosubmit ett MapReduce-jobb, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8986c-124">toosubmit a MapReduce job, use hello following command:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    <span data-ttu-id="8986c-125">hello slutet av hello URI (/ mapreduce/jar) anger WebHCat att denna begäran startar ett MapReduce-jobb från en klass i jar-filen.</span><span class="sxs-lookup"><span data-stu-id="8986c-125">hello end of hello URI (/mapreduce/jar) tells WebHCat that this request starts a MapReduce job from a class in a jar file.</span></span> <span data-ttu-id="8986c-126">hello-parametrar som används i det här kommandot är följande:</span><span class="sxs-lookup"><span data-stu-id="8986c-126">hello parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="8986c-127">**-d**: `-G` inte används så hello begäran standardvärden toohello POST-metoden.</span><span class="sxs-lookup"><span data-stu-id="8986c-127">**-d**: `-G` is not used, so hello request defaults toohello POST method.</span></span> <span data-ttu-id="8986c-128">`-d`Anger hello datavärden som skickas med hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="8986c-128">`-d` specifies hello data values that are sent with hello request.</span></span>
    * <span data-ttu-id="8986c-129">**User.name**: hello-användaren som kör hello kommando</span><span class="sxs-lookup"><span data-stu-id="8986c-129">**user.name**: hello user who is running hello command</span></span>
    * <span data-ttu-id="8986c-130">**JAR**: hello platsen för hello jar-filen som innehåller klassen toobe kördes</span><span class="sxs-lookup"><span data-stu-id="8986c-130">**jar**: hello location of hello jar file that contains class toobe ran</span></span>
    * <span data-ttu-id="8986c-131">**klassen**: hello klass som innehåller hello MapReduce-logik</span><span class="sxs-lookup"><span data-stu-id="8986c-131">**class**: hello class that contains hello MapReduce logic</span></span>
    * <span data-ttu-id="8986c-132">**%d{arg/**: hello argument toobe skickades toohello MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="8986c-132">**arg**: hello arguments toobe passed toohello MapReduce job.</span></span> <span data-ttu-id="8986c-133">I det här fallet indata hello text fil- och hello katalog som används för hello-utdata</span><span class="sxs-lookup"><span data-stu-id="8986c-133">In this case, hello input text file and hello directory that are used for hello output</span></span>

     <span data-ttu-id="8986c-134">Det här kommandot ska returnera ett jobb-ID som kan använda toocheck hello status för hello jobb:</span><span class="sxs-lookup"><span data-stu-id="8986c-134">This command should return a job ID that can be used toocheck hello status of hello job:</span></span>

       <span data-ttu-id="8986c-135">{”id”: ”job_1415651640909_0026”}</span><span class="sxs-lookup"><span data-stu-id="8986c-135">{"id":"job_1415651640909_0026"}</span></span>

3. <span data-ttu-id="8986c-136">toocheck hello hello jobbets status, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8986c-136">toocheck hello status of hello job, use hello following command:</span></span>

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    <span data-ttu-id="8986c-137">Ersätt hello **JOBID** med hello-värde som returneras i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="8986c-137">Replace hello **JOBID** with hello value returned in hello previous step.</span></span> <span data-ttu-id="8986c-138">Till exempel om hello returnera värdet var `{"id":"job_1415651640909_0026"}`, sedan hello JOBID skulle vara `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="8986c-138">For example, if hello return value was `{"id":"job_1415651640909_0026"}`, then hello JOBID would be `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="8986c-139">Om hello jobbet är klart hello tillstånd returnerade är `SUCCEEDED`.</span><span class="sxs-lookup"><span data-stu-id="8986c-139">If hello job is complete, hello state returned is `SUCCEEDED`.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8986c-140">Den här Curl-begäran returnerar ett JSON-dokument med information om hello jobb.</span><span class="sxs-lookup"><span data-stu-id="8986c-140">This Curl request returns a JSON document with information about hello job.</span></span> <span data-ttu-id="8986c-141">Jq används tooretrieve hello endast värdet state.</span><span class="sxs-lookup"><span data-stu-id="8986c-141">Jq is used tooretrieve only hello state value.</span></span>

4. <span data-ttu-id="8986c-142">När hello tillståndet för hello jobbet har ändrats för`SUCCEEDED`, du kan hämta hello resultatet av hello jobbet från Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="8986c-142">When hello state of hello job has changed too`SUCCEEDED`, you can retrieve hello results of hello job from Azure Blob storage.</span></span> <span data-ttu-id="8986c-143">Hej `statusdir` parameter som skickas med hello frågan innehåller hello plats hello utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="8986c-143">hello `statusdir` parameter that is passed with hello query contains hello location of hello output file.</span></span> <span data-ttu-id="8986c-144">I det här exemplet hello platsen är `/example/curl`.</span><span class="sxs-lookup"><span data-stu-id="8986c-144">In this example, hello location is `/example/curl`.</span></span> <span data-ttu-id="8986c-145">Den här adressen lagrar hello utdata för hello jobb i hello kluster standardlagring på `/example/curl`.</span><span class="sxs-lookup"><span data-stu-id="8986c-145">This address stores hello output of hello job in hello clusters default storage at `/example/curl`.</span></span>

<span data-ttu-id="8986c-146">Du kan visa och ladda ned dessa filer med hjälp av hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8986c-146">You can list and download these files by using hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="8986c-147">Mer information om hur du arbetar med blobbar från hello Azure CLI finns hello [Using hello Azure CLI 2.0 med Azure Storage](../storage/common/storage-azure-cli.md#create-and-manage-blobs) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="8986c-147">For more information on working with blobs from hello Azure CLI, see hello [Using hello Azure CLI 2.0 with Azure Storage](../storage/common/storage-azure-cli.md#create-and-manage-blobs) document.</span></span>

## <span data-ttu-id="8986c-148"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8986c-148"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="8986c-149">Allmän information om MapReduce-jobb i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="8986c-149">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="8986c-150">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="8986c-150">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="8986c-151">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="8986c-151">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="8986c-152">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="8986c-152">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="8986c-153">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="8986c-153">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="8986c-154">Mer information om hello REST-gränssnitt som används i den här artikeln finns hello [WebHCat referens](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span><span class="sxs-lookup"><span data-stu-id="8986c-154">For more information about hello REST interface that is used in this article, see hello [WebHCat Reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span></span>
