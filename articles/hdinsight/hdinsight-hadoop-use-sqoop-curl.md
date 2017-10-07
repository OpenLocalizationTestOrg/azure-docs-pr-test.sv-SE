---
title: aaaUse Hadoop Sqoop med Curl i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur tooremotely skicka Sqoop jobb tooHDInsight med Curl."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 39798321-78ca-428c-bcfe-322e49af4059
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: d9c09a6704ab6c5f48be50ed6d6314ec406df8ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a><span data-ttu-id="39aa8-103">Kör jobb för Sqoop med Hadoop i HDInsight med Curl</span><span class="sxs-lookup"><span data-stu-id="39aa8-103">Run Sqoop jobs with Hadoop in HDInsight with Curl</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="39aa8-104">Lär dig hur toouse Curl toorun Sqoop jobb på en Hadoop-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="39aa8-104">Learn how toouse Curl toorun Sqoop jobs on a Hadoop cluster in HDInsight.</span></span>

<span data-ttu-id="39aa8-105">CURL är används toodemonstrate hur du kan interagera med HDInsight med hjälp av rådata toorun för HTTP-begäranden, Övervakare och hämta hello resultaten av Sqoop jobb.</span><span class="sxs-lookup"><span data-stu-id="39aa8-105">Curl is used toodemonstrate how you can interact with HDInsight by using raw HTTP requests toorun, monitor, and retrieve hello results of Sqoop jobs.</span></span> <span data-ttu-id="39aa8-106">Det här fungerar med hjälp av hello WebHCat REST-API (kallades tidigare Templeton) tillhandahålls av HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="39aa8-106">This works by using hello WebHCat REST API (formerly known as Templeton) provided by your HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="39aa8-107">Om du redan är bekant med Linux-baserade Hadoop-servrar, men nya tooHDInsight, se [Information om hur du använder HDInsight på Linux](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="39aa8-107">If you are already familiar with using Linux-based Hadoop servers, but are new tooHDInsight, see [Information about using HDInsight on Linux](hdinsight-hadoop-linux-information.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="39aa8-108">Krav</span><span class="sxs-lookup"><span data-stu-id="39aa8-108">Prerequisites</span></span>
<span data-ttu-id="39aa8-109">toocomplete hello stegen i den här artikeln, behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="39aa8-109">toocomplete hello steps in this article, you will need hello following:</span></span>

* <span data-ttu-id="39aa8-110">En Hadoop på HDInsight-kluster (Linux och Windows-baserade)</span><span class="sxs-lookup"><span data-stu-id="39aa8-110">A Hadoop on HDInsight cluster (Linux or Windows-based)</span></span>
* [<span data-ttu-id="39aa8-111">CURL</span><span class="sxs-lookup"><span data-stu-id="39aa8-111">Curl</span></span>](http://curl.haxx.se/)
* [<span data-ttu-id="39aa8-112">jq</span><span class="sxs-lookup"><span data-stu-id="39aa8-112">jq</span></span>](http://stedolan.github.io/jq/)

## <a name="submit-sqoop-jobs-by-using-curl"></a><span data-ttu-id="39aa8-113">Skicka Sqoop jobb med Curl</span><span class="sxs-lookup"><span data-stu-id="39aa8-113">Submit Sqoop jobs by using Curl</span></span>
> [!NOTE]
> <span data-ttu-id="39aa8-114">När du använder Curl eller annan REST-kommunikation med WebHCat, måste du autentisera hello begäranden genom att tillhandahålla hello användarnamn och lösenord för hello HDInsight-klustrets administratör.</span><span class="sxs-lookup"><span data-stu-id="39aa8-114">When using Curl or any other REST communication with WebHCat, you must authenticate hello requests by providing hello user name and password for hello HDInsight cluster administrator.</span></span> <span data-ttu-id="39aa8-115">Du måste också använda hello klustrets namn som en del av hello identifierare URI (Uniform Resource) används toosend hello begäranden toohello server.</span><span class="sxs-lookup"><span data-stu-id="39aa8-115">You must also use hello cluster name as part of hello Uniform Resource Identifier (URI) used toosend hello requests toohello server.</span></span>
> 
> <span data-ttu-id="39aa8-116">Hello kommandon i det här avsnittet, ersätter **användarnamn** med hello användaren tooauthenticate toohello kluster och Ersätt **lösenord** med hello lösenordet för användarkontot för hello.</span><span class="sxs-lookup"><span data-stu-id="39aa8-116">For hello commands in this section, replace **USERNAME** with hello user tooauthenticate toohello cluster, and replace **PASSWORD** with hello password for hello user account.</span></span> <span data-ttu-id="39aa8-117">Ersätt **KLUSTERNAMN** med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="39aa8-117">Replace **CLUSTERNAME** with hello name of your cluster.</span></span>
> 
> <span data-ttu-id="39aa8-118">hello REST API skyddas via [grundläggande autentisering](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="39aa8-118">hello REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="39aa8-119">Du bör alltid göra begäranden genom att använda säker HTTP (HTTPS) toohelp se till att dina autentiseringsuppgifter skickas toohello server på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="39aa8-119">You should always make requests by using Secure HTTP (HTTPS) toohelp ensure that your credentials are securely sent toohello server.</span></span>
> 
> 

1. <span data-ttu-id="39aa8-120">Använd hello efter kommandot tooverify att du kan ansluta tooyour HDInsight-kluster från en kommandorad:</span><span class="sxs-lookup"><span data-stu-id="39aa8-120">From a command line, use hello following command tooverify that you can connect tooyour HDInsight cluster:</span></span>
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    <span data-ttu-id="39aa8-121">Du bör få ett svar liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="39aa8-121">You should receive a response similar toohello following:</span></span>
   
        {"status":"ok","version":"v1"}
   
    <span data-ttu-id="39aa8-122">hello-parametrar som används i det här kommandot är följande:</span><span class="sxs-lookup"><span data-stu-id="39aa8-122">hello parameters used in this command are as follows:</span></span>
   
   * <span data-ttu-id="39aa8-123">**-u** -hello användarnamn och lösenord används tooauthenticate hello begäran.</span><span class="sxs-lookup"><span data-stu-id="39aa8-123">**-u** - hello user name and password used tooauthenticate hello request.</span></span>
   * <span data-ttu-id="39aa8-124">**-G** – Anger att detta är en GET-begäran.</span><span class="sxs-lookup"><span data-stu-id="39aa8-124">**-G** - Indicates that this is a GET request.</span></span>
     
     <span data-ttu-id="39aa8-125">Hej i början av hello URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, kommer att hello samma för alla begäranden.</span><span class="sxs-lookup"><span data-stu-id="39aa8-125">hello beginning of hello URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, will be hello same for all requests.</span></span> <span data-ttu-id="39aa8-126">hello sökvägen **/status**, visar hello begäran är tooreturn WebHCat (även kallat Templeton) statusen för hello-servern.</span><span class="sxs-lookup"><span data-stu-id="39aa8-126">hello path, **/status**, indicates that hello request is tooreturn a status of WebHCat (also known as Templeton) for hello server.</span></span> 
2. <span data-ttu-id="39aa8-127">Använd hello följande toosubmit ett sqoop jobb:</span><span class="sxs-lookup"><span data-stu-id="39aa8-127">Use hello following toosubmit a sqoop job:</span></span>

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    <span data-ttu-id="39aa8-128">hello-parametrar som används i det här kommandot är följande:</span><span class="sxs-lookup"><span data-stu-id="39aa8-128">hello parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="39aa8-129">**-d** - sedan `-G` används inte hello begäran standardvärden toohello POST-metoden.</span><span class="sxs-lookup"><span data-stu-id="39aa8-129">**-d** - Since `-G` is not used, hello request defaults toohello POST method.</span></span> <span data-ttu-id="39aa8-130">`-d`Anger hello datavärden som skickas med hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="39aa8-130">`-d` specifies hello data values that are sent with hello request.</span></span>

        * <span data-ttu-id="39aa8-131">**User.name** -hello-användaren som kör hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="39aa8-131">**user.name** - hello user that is running hello command.</span></span>

        * <span data-ttu-id="39aa8-132">**kommandot** -hello Sqoop kommandot tooexecute.</span><span class="sxs-lookup"><span data-stu-id="39aa8-132">**command** - hello Sqoop command tooexecute.</span></span>

        * <span data-ttu-id="39aa8-133">**statusdir** -hello-katalog som hello status för jobbet kommer att skrivas till.</span><span class="sxs-lookup"><span data-stu-id="39aa8-133">**statusdir** - hello directory that hello status for this job will be written to.</span></span>

    <span data-ttu-id="39aa8-134">Det här kommandot ska returnera ett jobb-ID som kan använda toocheck hello status för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="39aa8-134">This command should return a job ID that can be used toocheck hello status of hello job.</span></span>

        {"id":"job_1415651640909_0026"}

1. <span data-ttu-id="39aa8-135">toocheck hello hello jobbets status, Använd hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="39aa8-135">toocheck hello status of hello job, use hello following command.</span></span> <span data-ttu-id="39aa8-136">Ersätt **JOBID** med hello-värde som returneras i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="39aa8-136">Replace **JOBID** with hello value returned in hello previous step.</span></span> <span data-ttu-id="39aa8-137">Till exempel om hello returnera värdet var `{"id":"job_1415651640909_0026"}`, sedan **JOBID** skulle vara `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="39aa8-137">For example, if hello return value was `{"id":"job_1415651640909_0026"}`, then **JOBID** would be `job_1415651640909_0026`.</span></span>
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    <span data-ttu-id="39aa8-138">Om hello jobbet har slutförts, hello tillstånd blir **lyckades**.</span><span class="sxs-lookup"><span data-stu-id="39aa8-138">If hello job has finished, hello state will be **SUCCEEDED**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="39aa8-139">Den här Curl-begäran returnerar ett JavaScript Object Notation (JSON) dokument med information om hello jobb. jq används tooretrieve hello endast värdet state.</span><span class="sxs-lookup"><span data-stu-id="39aa8-139">This Curl request returns a JavaScript Object Notation (JSON) document with information about hello job; jq is used tooretrieve only hello state value.</span></span>
   > 
   > 
2. <span data-ttu-id="39aa8-140">När hello tillståndet för hello jobbet har ändrats för**lyckades**, du kan hämta hello resultatet av hello jobbet från Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="39aa8-140">Once hello state of hello job has changed too**SUCCEEDED**, you can retrieve hello results of hello job from Azure Blob storage.</span></span> <span data-ttu-id="39aa8-141">Hej `statusdir` -parameter som överförs med hello frågan innehåller hello platsen hello utdatafilen; i det här fallet **wasb: / / / exempel/curl**.</span><span class="sxs-lookup"><span data-stu-id="39aa8-141">hello `statusdir` parameter passed with hello query contains hello location of hello output file; in this case, **wasb:///example/curl**.</span></span> <span data-ttu-id="39aa8-142">Den här adressen lagrar hello utdata för hello jobb i hello **exempel/curl** på hello standardbehållare för lagring används av ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="39aa8-142">This address stores hello output of hello job in hello **example/curl** directory on hello default storage container used by your HDInsight cluster.</span></span>
   
    <span data-ttu-id="39aa8-143">Du kan visa och ladda ned dessa filer med hjälp av hello [Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="39aa8-143">You can list and download these files by using hello [Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="39aa8-144">Till exempel toolist filer i **exempel/curl**, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="39aa8-144">For example, toolist files in **example/curl**, use hello following command:</span></span>
   
        azure storage blob list <container-name> example/curl
   
    <span data-ttu-id="39aa8-145">toodownload en fil, använder hello följande:</span><span class="sxs-lookup"><span data-stu-id="39aa8-145">toodownload a file, use hello following:</span></span>
   
        azure storage blob download <container-name> <blob-name> <destination-file>
   
   > [!NOTE]
   > <span data-ttu-id="39aa8-146">Du måste antingen ange hello lagringskontonamnet som innehåller hello blob med hjälp av hello `-a` och `-k` parametrar eller ange hello **AZURE\_lagring\_konto** och **AZURE\_lagring\_åtkomst\_NYCKELN** miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="39aa8-146">You must either specify hello storage account name that contains hello blob by using hello `-a` and `-k` parameters, or set hello **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS\_KEY** environment variables.</span></span> <span data-ttu-id="39aa8-147">Se < en href = ”hdinsight-överför-data.md” target = ”_blank” mer information.</span><span class="sxs-lookup"><span data-stu-id="39aa8-147">See <a href="hdinsight-upload-data.md" target="_blank" for more information.</span></span>
   > 
   > 

## <a name="limitations"></a><span data-ttu-id="39aa8-148">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="39aa8-148">Limitations</span></span>
* <span data-ttu-id="39aa8-149">Masskapat export - med Linux-baserat HDInsight, hello Sqoop koppling som används tooexport data tooMicrosoft SQL Server eller Azure SQL Database stöder för närvarande inte bulkinfogningar.</span><span class="sxs-lookup"><span data-stu-id="39aa8-149">Bulk export - With Linux-based HDInsight, hello Sqoop connector used tooexport data tooMicrosoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="39aa8-150">Batchbearbetning - med Linux-baserat HDInsight när du använder hello `-batch` växeln när du utför infogningar, Sqoop utför flera infogningar i stället för batchbearbetning hello infogningsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="39aa8-150">Batching - With Linux-based HDInsight, When using hello `-batch` switch when performing inserts, Sqoop will perform multiple inserts instead of batching hello insert operations.</span></span>

## <a name="summary"></a><span data-ttu-id="39aa8-151">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="39aa8-151">Summary</span></span>
<span data-ttu-id="39aa8-152">Som visas i det här dokumentet, kan du använda en rå HTTP-begäran toorun, övervaka och visa hello resultat av Sqoop jobb på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="39aa8-152">As demonstrated in this document, you can use a raw HTTP request toorun, monitor, and view hello results of Sqoop jobs on your HDInsight cluster.</span></span>

<span data-ttu-id="39aa8-153">Mer information om hello REST-gränssnitt som används i den här artikeln finns hello <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API-guiden</a>.</span><span class="sxs-lookup"><span data-stu-id="39aa8-153">For more information on hello REST interface used in this article, see hello <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API guide</a>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="39aa8-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="39aa8-154">Next steps</span></span>
<span data-ttu-id="39aa8-155">Allmän information om Hive med HDInsight:</span><span class="sxs-lookup"><span data-stu-id="39aa8-155">For general information on Hive with HDInsight:</span></span>

* [<span data-ttu-id="39aa8-156">Använda Sqoop med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="39aa8-156">Use Sqoop with Hadoop on HDInsight</span></span>](hdinsight-use-sqoop.md)

<span data-ttu-id="39aa8-157">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="39aa8-157">For information on other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="39aa8-158">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="39aa8-158">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="39aa8-159">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="39aa8-159">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="39aa8-160">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="39aa8-160">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


