---
title: "Använd MapReduce och Curl med Hadoop i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur du Fjärrkör MapReduce-jobb med Hadoop i HDInsight med Curl."
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
ms.openlocfilehash: 8238bb829df95dcb8c99c0b7fff53c627a56f47c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a><span data-ttu-id="9fa69-103">Kör jobb för MapReduce med Hadoop i HDInsight med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="9fa69-103">Run MapReduce jobs with Hadoop on HDInsight using REST</span></span>

<span data-ttu-id="9fa69-104">Lär dig hur du använder WebHCat REST API för att köra MapReduce-jobb på en Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="9fa69-104">Learn how to use the WebHCat REST API to run MapReduce jobs on a Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="9fa69-105">CURL används för att demonstrera hur du kan interagera med HDInsight med hjälp av rådata HTTP-begäranden för att köra MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="9fa69-105">Curl is used to demonstrate how you can interact with HDInsight by using raw HTTP requests to run MapReduce jobs.</span></span>

> [!NOTE]
> <span data-ttu-id="9fa69-106">Om du redan är bekant med Linux-baserade Hadoop-servrar, men du är nybörjare på HDInsight, finns det [vad du behöver veta om Linux-baserade Hadoop på HDInsight](hdinsight-hadoop-linux-information.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="9fa69-106">If you are already familiar with using Linux-based Hadoop servers, but you are new to HDInsight, see the [What you need to know about Linux-based Hadoop on HDInsight](hdinsight-hadoop-linux-information.md) document.</span></span>


## <span data-ttu-id="9fa69-107"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="9fa69-107"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="9fa69-108">En Hadoop på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="9fa69-108">A Hadoop on HDInsight cluster</span></span>
* [<span data-ttu-id="9fa69-109">CURL</span><span class="sxs-lookup"><span data-stu-id="9fa69-109">Curl</span></span>](http://curl.haxx.se/)
* [<span data-ttu-id="9fa69-110">jq</span><span class="sxs-lookup"><span data-stu-id="9fa69-110">jq</span></span>](http://stedolan.github.io/jq/)

## <span data-ttu-id="9fa69-111"><a id="curl"></a>Kör MapReduce-jobb med Curl</span><span class="sxs-lookup"><span data-stu-id="9fa69-111"><a id="curl"></a>Run MapReduce jobs using Curl</span></span>

> [!NOTE]
> <span data-ttu-id="9fa69-112">När du använder Curl eller annan REST-kommunikation med WebHCat, måste du autentisera begärandena genom att ange administratörsanvändarnamn för HDInsight-kluster och lösenord.</span><span class="sxs-lookup"><span data-stu-id="9fa69-112">When you use Curl or any other REST communication with WebHCat, you must authenticate the requests by providing the HDInsight cluster administrator user name and password.</span></span> <span data-ttu-id="9fa69-113">Du måste använda klustrets namn som en del av den URI som används för att skicka begäranden till servern.</span><span class="sxs-lookup"><span data-stu-id="9fa69-113">You must use the cluster name as part of the URI that is used to send the requests to the server.</span></span>
>
> <span data-ttu-id="9fa69-114">Kommandona i det här avsnittet, ersätter **användarnamn** med användaren för att autentisera för klustret och **lösenord** med lösenordet för användarkontot.</span><span class="sxs-lookup"><span data-stu-id="9fa69-114">For the commands in this section, replace **USERNAME** with the user to authenticate to the cluster, and **PASSWORD** with the password for the user account.</span></span> <span data-ttu-id="9fa69-115">Ersätt **CLUSTERNAME** med namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="9fa69-115">Replace **CLUSTERNAME** with the name of your cluster.</span></span>
>
> <span data-ttu-id="9fa69-116">REST API skyddas med hjälp av [grundläggande autentisering](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="9fa69-116">The REST API is secured by using [basic access authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="9fa69-117">Du bör alltid göra begäranden genom att använda HTTPS för att se till att dina autentiseringsuppgifter skickas på ett säkert sätt till servern.</span><span class="sxs-lookup"><span data-stu-id="9fa69-117">You should always make requests by using HTTPS to ensure that your credentials are securely sent to the server.</span></span>


1. <span data-ttu-id="9fa69-118">Använd följande kommando från en kommandorad för att verifiera att du kan ansluta till ditt HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="9fa69-118">From a command line, use the following command to verify that you can connect to your HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="9fa69-119">Du bör få ett svar som liknar följande JSON:</span><span class="sxs-lookup"><span data-stu-id="9fa69-119">You should receive a response similar to the following JSON:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="9fa69-120">De parametrar som används i det här kommandot är följande:</span><span class="sxs-lookup"><span data-stu-id="9fa69-120">The parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="9fa69-121">**-u**: anger användarnamn och lösenord som används för att autentisera begäran</span><span class="sxs-lookup"><span data-stu-id="9fa69-121">**-u**: Indicates the user name and password used to authenticate the request</span></span>
   * <span data-ttu-id="9fa69-122">**-G**: Anger att den här åtgärden är en GET-begäran</span><span class="sxs-lookup"><span data-stu-id="9fa69-122">**-G**: Indicates that this operation is a GET request</span></span>

     <span data-ttu-id="9fa69-123">I början av URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, är samma för alla begäranden.</span><span class="sxs-lookup"><span data-stu-id="9fa69-123">The beginning of the URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is the same for all requests.</span></span>

2. <span data-ttu-id="9fa69-124">För att skicka ett MapReduce-jobb, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9fa69-124">To submit a MapReduce job, use the following command:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    <span data-ttu-id="9fa69-125">I slutet av URI: N (/ mapreduce/jar) anger WebHCat att denna begäran startar ett MapReduce-jobb från en klass i jar-filen.</span><span class="sxs-lookup"><span data-stu-id="9fa69-125">The end of the URI (/mapreduce/jar) tells WebHCat that this request starts a MapReduce job from a class in a jar file.</span></span> <span data-ttu-id="9fa69-126">De parametrar som används i det här kommandot är följande:</span><span class="sxs-lookup"><span data-stu-id="9fa69-126">The parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="9fa69-127">**-d**: `-G` används inte, så begäran som standard POST-metoden.</span><span class="sxs-lookup"><span data-stu-id="9fa69-127">**-d**: `-G` is not used, so the request defaults to the POST method.</span></span> <span data-ttu-id="9fa69-128">`-d`Anger datavärdena som skickas med begäran.</span><span class="sxs-lookup"><span data-stu-id="9fa69-128">`-d` specifies the data values that are sent with the request.</span></span>
    * <span data-ttu-id="9fa69-129">**User.name**: användare som kör kommandot</span><span class="sxs-lookup"><span data-stu-id="9fa69-129">**user.name**: The user who is running the command</span></span>
    * <span data-ttu-id="9fa69-130">**JAR**: platsen för jar-filen som innehåller klassen kördes</span><span class="sxs-lookup"><span data-stu-id="9fa69-130">**jar**: The location of the jar file that contains class to be ran</span></span>
    * <span data-ttu-id="9fa69-131">**klassen**: klass som innehåller logiken som MapReduce</span><span class="sxs-lookup"><span data-stu-id="9fa69-131">**class**: The class that contains the MapReduce logic</span></span>
    * <span data-ttu-id="9fa69-132">**%d{arg/**: argument som ska skickas till MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="9fa69-132">**arg**: The arguments to be passed to the MapReduce job.</span></span> <span data-ttu-id="9fa69-133">I det här fallet, inkommande textfilen och katalogen som används för utdata</span><span class="sxs-lookup"><span data-stu-id="9fa69-133">In this case, the input text file and the directory that are used for the output</span></span>

     <span data-ttu-id="9fa69-134">Det här kommandot ska returnera ett jobb-ID som kan användas för att kontrollera status för jobbet:</span><span class="sxs-lookup"><span data-stu-id="9fa69-134">This command should return a job ID that can be used to check the status of the job:</span></span>

       <span data-ttu-id="9fa69-135">{”id”: ”job_1415651640909_0026”}</span><span class="sxs-lookup"><span data-stu-id="9fa69-135">{"id":"job_1415651640909_0026"}</span></span>

3. <span data-ttu-id="9fa69-136">Om du vill kontrollera status för jobbet, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9fa69-136">To check the status of the job, use the following command:</span></span>

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    <span data-ttu-id="9fa69-137">Ersätt den **JOBID** med värdet som returneras i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="9fa69-137">Replace the **JOBID** with the value returned in the previous step.</span></span> <span data-ttu-id="9fa69-138">Om exempelvis returvärde `{"id":"job_1415651640909_0026"}`, jobb-ID skulle vara `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="9fa69-138">For example, if the return value was `{"id":"job_1415651640909_0026"}`, then the JOBID would be `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="9fa69-139">Om jobbet är klart, tillståndet returneras är `SUCCEEDED`.</span><span class="sxs-lookup"><span data-stu-id="9fa69-139">If the job is complete, the state returned is `SUCCEEDED`.</span></span>

   > [!NOTE]
   > <span data-ttu-id="9fa69-140">Den här Curl-begäran returnerar ett JSON-dokument med information om jobbet.</span><span class="sxs-lookup"><span data-stu-id="9fa69-140">This Curl request returns a JSON document with information about the job.</span></span> <span data-ttu-id="9fa69-141">Jq används för att hämta tillståndet värdet.</span><span class="sxs-lookup"><span data-stu-id="9fa69-141">Jq is used to retrieve only the state value.</span></span>

4. <span data-ttu-id="9fa69-142">När status för jobbet har ändrats till `SUCCEEDED`, kan du hämta resultatet av jobbet från Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="9fa69-142">When the state of the job has changed to `SUCCEEDED`, you can retrieve the results of the job from Azure Blob storage.</span></span> <span data-ttu-id="9fa69-143">Den `statusdir` parameter som skickas med frågan innehåller platsen för utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="9fa69-143">The `statusdir` parameter that is passed with the query contains the location of the output file.</span></span> <span data-ttu-id="9fa69-144">I det här exemplet platsen är `/example/curl`.</span><span class="sxs-lookup"><span data-stu-id="9fa69-144">In this example, the location is `/example/curl`.</span></span> <span data-ttu-id="9fa69-145">Den här adressen lagrar resultatet av jobbet i kluster standardlagring på `/example/curl`.</span><span class="sxs-lookup"><span data-stu-id="9fa69-145">This address stores the output of the job in the clusters default storage at `/example/curl`.</span></span>

<span data-ttu-id="9fa69-146">Du kan visa och ladda ned dessa filer med hjälp av den [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9fa69-146">You can list and download these files by using the [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="9fa69-147">Mer information om hur du arbetar med blobbar från Azure CLI finns i [använder Azure CLI 2.0 med Azure Storage](../storage/common/storage-azure-cli.md#create-and-manage-blobs) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="9fa69-147">For more information on working with blobs from the Azure CLI, see the [Using the Azure CLI 2.0 with Azure Storage](../storage/common/storage-azure-cli.md#create-and-manage-blobs) document.</span></span>

## <span data-ttu-id="9fa69-148"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9fa69-148"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="9fa69-149">Allmän information om MapReduce-jobb i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="9fa69-149">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="9fa69-150">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="9fa69-150">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="9fa69-151">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="9fa69-151">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="9fa69-152">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="9fa69-152">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="9fa69-153">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="9fa69-153">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="9fa69-154">Mer information om REST-gränssnitt som används i den här artikeln finns det [WebHCat referens](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span><span class="sxs-lookup"><span data-stu-id="9fa69-154">For more information about the REST interface that is used in this article, see the [WebHCat Reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span></span>