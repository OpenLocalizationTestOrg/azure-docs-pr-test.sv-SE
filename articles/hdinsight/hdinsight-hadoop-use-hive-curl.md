---
title: aaaUse Hadoop Hive med Curl i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur tooremotely skicka Pig-jobb med Curl tooHDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: e725829ad2adcf3540f44375e3e87b7cdaebd15e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a><span data-ttu-id="0dd29-103">Köra Hive-frågor med Hadoop i HDInsight med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="0dd29-103">Run Hive queries with Hadoop in HDInsight using REST</span></span>

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="0dd29-104">Lär dig hur toouse hello WebHCat REST API toorun Hive-frågor med Hadoop på Azure HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0dd29-104">Learn how toouse hello WebHCat REST API toorun Hive queries with Hadoop on Azure HDInsight cluster.</span></span>

<span data-ttu-id="0dd29-105">[CURL](http://curl.haxx.se/) är används toodemonstrate hur du kan interagera med HDInsight med hjälp av rådata HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="0dd29-105">[Curl](http://curl.haxx.se/) is used toodemonstrate how you can interact with HDInsight by using raw HTTP requests.</span></span> <span data-ttu-id="0dd29-106">Hej [jq](http://stedolan.github.io/jq/) -verktyget är används tooprocess hello JSON-data som returneras från REST-begäranden.</span><span class="sxs-lookup"><span data-stu-id="0dd29-106">hello [jq](http://stedolan.github.io/jq/) utility is used tooprocess hello JSON data returned from REST requests.</span></span>

> [!NOTE]
> <span data-ttu-id="0dd29-107">Om du redan är bekant med Linux-baserade Hadoop-servrar, men nya tooHDInsight, se hello [vad du behöver tooknow om Hadoop i Linux-baserat HDInsight](hdinsight-hadoop-linux-information.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="0dd29-107">If you are already familiar with using Linux-based Hadoop servers, but are new tooHDInsight, see hello [What you need tooknow about Hadoop on Linux-based HDInsight](hdinsight-hadoop-linux-information.md) document.</span></span>

## <span data-ttu-id="0dd29-108"><a id="curl"></a>Köra Hive-frågor</span><span class="sxs-lookup"><span data-stu-id="0dd29-108"><a id="curl"></a>Run Hive queries</span></span>

> [!NOTE]
> <span data-ttu-id="0dd29-109">När du använder cURL eller annan REST-kommunikation med WebHCat, måste du autentisera hello begäranden genom att tillhandahålla hello användarnamn och lösenord för hello HDInsight-klustrets administratör.</span><span class="sxs-lookup"><span data-stu-id="0dd29-109">When using cURL or any other REST communication with WebHCat, you must authenticate hello requests by providing hello user name and password for hello HDInsight cluster administrator.</span></span>
>
> <span data-ttu-id="0dd29-110">Hello kommandon i det här avsnittet, ersätter **användarnamn** med hello användaren tooauthenticate toohello kluster och Ersätt **lösenord** med hello lösenordet för användarkontot för hello.</span><span class="sxs-lookup"><span data-stu-id="0dd29-110">For hello commands in this section, replace **USERNAME** with hello user tooauthenticate toohello cluster, and replace **PASSWORD** with hello password for hello user account.</span></span> <span data-ttu-id="0dd29-111">Ersätt **KLUSTERNAMN** med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="0dd29-111">Replace **CLUSTERNAME** with hello name of your cluster.</span></span>
>
> <span data-ttu-id="0dd29-112">hello REST API skyddas via [grundläggande autentisering](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="0dd29-112">hello REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="0dd29-113">toohelp se till att dina autentiseringsuppgifter på ett säkert sätt skickas toohello server alltid göra begäranden genom att använda säker HTTP (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="0dd29-113">toohelp ensure that your credentials are securely sent toohello server, always make requests by using Secure HTTP (HTTPS).</span></span>

1. <span data-ttu-id="0dd29-114">Använd hello efter kommandot tooverify att du kan ansluta tooyour HDInsight-kluster från en kommandorad:</span><span class="sxs-lookup"><span data-stu-id="0dd29-114">From a command line, use hello following command tooverify that you can connect tooyour HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="0dd29-115">Du får ett svar liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="0dd29-115">You receive a response similar toohello following text:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="0dd29-116">hello-parametrar som används i det här kommandot är följande:</span><span class="sxs-lookup"><span data-stu-id="0dd29-116">hello parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="0dd29-117">**-u** -hello användarnamn och lösenord används tooauthenticate hello begäran.</span><span class="sxs-lookup"><span data-stu-id="0dd29-117">**-u** - hello user name and password used tooauthenticate hello request.</span></span>
   * <span data-ttu-id="0dd29-118">**-G** -anger att begäran är en GET-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0dd29-118">**-G** - Indicates that this request is a GET operation.</span></span>

     <span data-ttu-id="0dd29-119">Hej i början av hello URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, hello samma för alla begäranden.</span><span class="sxs-lookup"><span data-stu-id="0dd29-119">hello beginning of hello URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hello same for all requests.</span></span> <span data-ttu-id="0dd29-120">hello sökvägen **/status**, visar hello begäran är tooreturn WebHCat (även kallat Templeton) statusen för hello-servern.</span><span class="sxs-lookup"><span data-stu-id="0dd29-120">hello path, **/status**, indicates that hello request is tooreturn a status of WebHCat (also known as Templeton) for hello server.</span></span> <span data-ttu-id="0dd29-121">Du kan också begära hello version av Hive med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0dd29-121">You can also request hello version of Hive by using hello following command:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

     <span data-ttu-id="0dd29-122">Denna begäran returnerar ett svar liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="0dd29-122">This request returns a response similar toohello following text:</span></span>

       <span data-ttu-id="0dd29-123">{”modulen”: ”hive”, ”version”: ”0.13.0.2.1.6.0-2103”}</span><span class="sxs-lookup"><span data-stu-id="0dd29-123">{"module":"hive","version":"0.13.0.2.1.6.0-2103"}</span></span>

2. <span data-ttu-id="0dd29-124">Använd hello följande toocreate en tabell med namnet **log4jLogs**:</span><span class="sxs-lookup"><span data-stu-id="0dd29-124">Use hello following toocreate a table named **log4jLogs**:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    <span data-ttu-id="0dd29-125">hello följande parametrar som används med denna begäran:</span><span class="sxs-lookup"><span data-stu-id="0dd29-125">hello following parameters used with this request:</span></span>

   * <span data-ttu-id="0dd29-126">**-d** - sedan `-G` används inte hello begäran standardvärden toohello POST-metoden.</span><span class="sxs-lookup"><span data-stu-id="0dd29-126">**-d** - Since `-G` is not used, hello request defaults toohello POST method.</span></span> <span data-ttu-id="0dd29-127">`-d`Anger hello datavärden som skickas med hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="0dd29-127">`-d` specifies hello data values that are sent with hello request.</span></span>

     * <span data-ttu-id="0dd29-128">**User.name** -hello-användaren som kör hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="0dd29-128">**user.name** - hello user that is running hello command.</span></span>
     * <span data-ttu-id="0dd29-129">**köra** -hello HiveQL-instruktioner tooexecute.</span><span class="sxs-lookup"><span data-stu-id="0dd29-129">**execute** - hello HiveQL statements tooexecute.</span></span>
     * <span data-ttu-id="0dd29-130">**statusdir** -hello-katalog som hello status för jobbet ska skrivas till.</span><span class="sxs-lookup"><span data-stu-id="0dd29-130">**statusdir** - hello directory that hello status for this job is written to.</span></span>

     <span data-ttu-id="0dd29-131">Dessa instruktioner utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="0dd29-131">These statements perform hello following actions:</span></span>
   * <span data-ttu-id="0dd29-132">**DROP TABLE** -hello tabellen redan raderas.</span><span class="sxs-lookup"><span data-stu-id="0dd29-132">**DROP TABLE** - If hello table already exists, it is deleted.</span></span>
   * <span data-ttu-id="0dd29-133">**Skapa extern tabell** -skapar en ny ”externa” tabell i Hive.</span><span class="sxs-lookup"><span data-stu-id="0dd29-133">**CREATE EXTERNAL TABLE** - Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="0dd29-134">Externa tabeller lagra endast hello tabelldefinition i Hive.</span><span class="sxs-lookup"><span data-stu-id="0dd29-134">External tables store only hello table definition in Hive.</span></span> <span data-ttu-id="0dd29-135">hello data finns kvar i hello ursprungliga plats.</span><span class="sxs-lookup"><span data-stu-id="0dd29-135">hello data is left in hello original location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="0dd29-136">Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa.</span><span class="sxs-lookup"><span data-stu-id="0dd29-136">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="0dd29-137">Till exempel en automatisk överföring av data eller en annan MapReduce-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0dd29-137">For example, an automated data upload process or another MapReduce operation.</span></span>
     >
     > <span data-ttu-id="0dd29-138">Släppa en extern tabell har **inte** ta bort data hello, bara hello tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="0dd29-138">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>

   * <span data-ttu-id="0dd29-139">**RADEN FORMAT** – hur hello data formateras.</span><span class="sxs-lookup"><span data-stu-id="0dd29-139">**ROW FORMAT** - How hello data is formatted.</span></span> <span data-ttu-id="0dd29-140">hello fälten i varje logg avgränsas med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="0dd29-140">hello fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="0dd29-141">**LAGRAS AS TEXTFILE plats** - när hello data lagras (hello exempel/datakatalog) och att den lagras som text.</span><span class="sxs-lookup"><span data-stu-id="0dd29-141">**STORED AS TEXTFILE LOCATION** - Where hello data is stored (hello example/data directory) and that it is stored as text.</span></span>
   * <span data-ttu-id="0dd29-142">**Välj** -väljer en uppräkning av alla rader där kolumnen **t4** innehåller hello värdet **[fel]**.</span><span class="sxs-lookup"><span data-stu-id="0dd29-142">**SELECT** - Selects a count of all rows where column **t4** contains hello value **[ERROR]**.</span></span> <span data-ttu-id="0dd29-143">Den här instruktionen returnerar ett värde för **3** som det finns tre rader som innehåller det här värdet.</span><span class="sxs-lookup"><span data-stu-id="0dd29-143">This statement returns a value of **3** as there are three rows that contain this value.</span></span>

     > [!NOTE]
     > <span data-ttu-id="0dd29-144">Observera att hello blanksteg mellan HiveQL-instruktioner har ersatts av hello `+` tecken när det används med Curl.</span><span class="sxs-lookup"><span data-stu-id="0dd29-144">Notice that hello spaces between HiveQL statements are replaced by hello `+` character when used with Curl.</span></span> <span data-ttu-id="0dd29-145">Citattecken värden som innehåller blanksteg, till exempel hello avgränsaren, bör inte ersättas av `+`.</span><span class="sxs-lookup"><span data-stu-id="0dd29-145">Quoted values that contain a space, such as hello delimiter, should not be replaced by `+`.</span></span>

   * <span data-ttu-id="0dd29-146">**INPUT__FILE__NAME som '% 25.log'** – den här instruktionen gränser hello Sök tooonly filer slutar på. log.</span><span class="sxs-lookup"><span data-stu-id="0dd29-146">**INPUT__FILE__NAME LIKE '%25.log'** - This statement limits hello search tooonly use files ending in .log.</span></span>

     > [!NOTE]
     > <span data-ttu-id="0dd29-147">Hej `%25` är hello URL-kodade formatet %, så hello faktiska villkoret är `like '%.log'`.</span><span class="sxs-lookup"><span data-stu-id="0dd29-147">hello `%25` is hello URL encoded form of %, so hello actual condition is `like '%.log'`.</span></span> <span data-ttu-id="0dd29-148">Hej % har toobe URL-kodade, eftersom den behandlas som ett specialtecken i URL: er.</span><span class="sxs-lookup"><span data-stu-id="0dd29-148">hello % has toobe URL encoded, as it is treated as a special character in URLs.</span></span>

     <span data-ttu-id="0dd29-149">Det här kommandot ska returnera ett jobb-ID som kan använda toocheck hello status för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="0dd29-149">This command should return a job ID that can be used toocheck hello status of hello job.</span></span>

       <span data-ttu-id="0dd29-150">{”id”: ”job_1415651640909_0026”}</span><span class="sxs-lookup"><span data-stu-id="0dd29-150">{"id":"job_1415651640909_0026"}</span></span>

3. <span data-ttu-id="0dd29-151">toocheck hello hello jobbets status, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0dd29-151">toocheck hello status of hello job, use hello following command:</span></span>

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    <span data-ttu-id="0dd29-152">Ersätt **JOBID** med hello-värde som returneras i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="0dd29-152">Replace **JOBID** with hello value returned in hello previous step.</span></span> <span data-ttu-id="0dd29-153">Till exempel om hello returnera värdet var `{"id":"job_1415651640909_0026"}`, sedan **JOBID** skulle vara `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="0dd29-153">For example, if hello return value was `{"id":"job_1415651640909_0026"}`, then **JOBID** would be `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="0dd29-154">Om hello jobbet har slutförts, hello tillstånd är **lyckades**.</span><span class="sxs-lookup"><span data-stu-id="0dd29-154">If hello job has finished, hello state is **SUCCEEDED**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0dd29-155">Den här Curl-begäran returnerar ett JavaScript Object Notation (JSON) dokument med information om hello jobb.</span><span class="sxs-lookup"><span data-stu-id="0dd29-155">This Curl request returns a JavaScript Object Notation (JSON) document with information about hello job.</span></span> <span data-ttu-id="0dd29-156">Jq används tooretrieve hello endast värdet state.</span><span class="sxs-lookup"><span data-stu-id="0dd29-156">Jq is used tooretrieve only hello state value.</span></span>

4. <span data-ttu-id="0dd29-157">När hello tillståndet för hello jobbet har ändrats för**lyckades**, du kan hämta hello resultatet av hello jobbet från Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="0dd29-157">Once hello state of hello job has changed too**SUCCEEDED**, you can retrieve hello results of hello job from Azure Blob storage.</span></span> <span data-ttu-id="0dd29-158">Hej `statusdir` -parameter som överförs med hello frågan innehåller hello platsen hello utdatafilen; i det här fallet **-exempel curl**.</span><span class="sxs-lookup"><span data-stu-id="0dd29-158">hello `statusdir` parameter passed with hello query contains hello location of hello output file; in this case, **/example/curl**.</span></span> <span data-ttu-id="0dd29-159">Den här adressen lagrar hello utdata i hello **exempel/curl** katalog i hello kluster standardlagring.</span><span class="sxs-lookup"><span data-stu-id="0dd29-159">This address stores hello output in hello **example/curl** directory in hello clusters default storage.</span></span>

    <span data-ttu-id="0dd29-160">Du kan visa och ladda ned dessa filer med hjälp av hello [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0dd29-160">You can list and download these files by using hello [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="0dd29-161">Mer information om hur du använder hello Azure CLI med Azure Storage finns hello [Använd Azure CLI 2.0 med Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli#create-and-manage-blobs) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="0dd29-161">For more information on using hello Azure CLI with Azure Storage, see hello [Use Azure CLI 2.0 with Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli#create-and-manage-blobs) document.</span></span>

5. <span data-ttu-id="0dd29-162">Använd hello följande instruktioner toocreate en ny ”interna” tabell med namnet **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="0dd29-162">Use hello following statements toocreate a new 'internal' table named **errorLogs**:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    <span data-ttu-id="0dd29-163">Dessa instruktioner utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="0dd29-163">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="0dd29-164">**Skapa tabell om inte finns** -skapar en tabell om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="0dd29-164">**CREATE TABLE IF NOT EXISTS** - Creates a table, if it does not already exist.</span></span> <span data-ttu-id="0dd29-165">Den här instruktionen skapar en intern tabell som lagras i datalagret för hello Hive och hanteras helt av Hive.</span><span class="sxs-lookup"><span data-stu-id="0dd29-165">This statement creates an internal table, which is stored in hello Hive data warehouse and is managed completely by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="0dd29-166">Till skillnad från externa tabeller kan släppa en intern tabell tar bort hello samt underliggande data.</span><span class="sxs-lookup"><span data-stu-id="0dd29-166">Unlike external tables, dropping an internal table deletes hello underlying data as well.</span></span>

   * <span data-ttu-id="0dd29-167">**LAGRAS AS ORC** -lagrar hello data i optimerad raden kolumner (ORC)-format.</span><span class="sxs-lookup"><span data-stu-id="0dd29-167">**STORED AS ORC** - Stores hello data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="0dd29-168">ORC är ett mycket optimerad och effektiv format för att lagra data med Hive.</span><span class="sxs-lookup"><span data-stu-id="0dd29-168">ORC is a highly optimized and efficient format for storing Hive data.</span></span>
   * <span data-ttu-id="0dd29-169">**INFOGA ÖVER... Välj** -väljer rader från hello **log4jLogs** tabellen som innehåller **[fel]**, och sedan infogningar hello data i hello **errorLogs** tabell.</span><span class="sxs-lookup"><span data-stu-id="0dd29-169">**INSERT OVERWRITE ... SELECT** - Selects rows from hello **log4jLogs** table that contain **[ERROR]**, then inserts hello data into hello **errorLogs** table.</span></span>
   * <span data-ttu-id="0dd29-170">**Välj** -markeras alla rader från hello nya **errorLogs** tabell.</span><span class="sxs-lookup"><span data-stu-id="0dd29-170">**SELECT** - Selects all rows from hello new **errorLogs** table.</span></span>

6. <span data-ttu-id="0dd29-171">Använd hello jobb-ID som returnerades toocheck hello status för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="0dd29-171">Use hello job ID returned toocheck hello status of hello job.</span></span> <span data-ttu-id="0dd29-172">Använd hello Azure CLI som beskrivits ovan toodownload och visa hello resultat när den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="0dd29-172">Once it has succeeded, use hello Azure CLI as described previously toodownload and view hello results.</span></span> <span data-ttu-id="0dd29-173">hello utdata ska innehålla tre rader, som alla innehåller **[fel]**.</span><span class="sxs-lookup"><span data-stu-id="0dd29-173">hello output should contain three lines, all of which contain **[ERROR]**.</span></span>

## <span data-ttu-id="0dd29-174"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0dd29-174"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="0dd29-175">Allmän information om Hive med HDInsight:</span><span class="sxs-lookup"><span data-stu-id="0dd29-175">For general information on Hive with HDInsight:</span></span>

* [<span data-ttu-id="0dd29-176">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0dd29-176">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="0dd29-177">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="0dd29-177">For information on other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="0dd29-178">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0dd29-178">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="0dd29-179">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0dd29-179">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="0dd29-180">Om du använder Tez med Hive finns i följande dokument för felsökningsinformation hello:</span><span class="sxs-lookup"><span data-stu-id="0dd29-180">If you are using Tez with Hive, see hello following documents for debugging information:</span></span>

* [<span data-ttu-id="0dd29-181">Använd hello Ambari Tez vy på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="0dd29-181">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

<span data-ttu-id="0dd29-182">Mer information om hello REST-API som används i det här dokumentet finns hello [WebHCat referens](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="0dd29-182">For more information on hello REST API used in this document, see hello [WebHCat reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) document.</span></span>

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


