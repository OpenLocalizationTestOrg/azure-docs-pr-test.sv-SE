---
title: "Använda Hadoop Pig med SSH på ett HDInsight-kluster - Azure | Microsoft Docs"
description: "Lär dig hur ansluter till ett Linux-baserade Hadoop-kluster med SSH, och sedan använda kommandot Pig köras interaktivt Pig Latin-instruktioner eller som ett batchjobb."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b646a93b-4c51-4ba4-84da-3275d9124ebe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: e4c893ef4bfa573dd9fbc9c9b0ae296720769842
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a><span data-ttu-id="9789b-103">Köra Pig-jobb på en Linux-baserade kluster med kommandot Pig (SSH)</span><span class="sxs-lookup"><span data-stu-id="9789b-103">Run Pig jobs on a Linux-based cluster with the Pig command (SSH)</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="9789b-104">Lär dig hur ska köras interaktivt Pig-jobb från en SSH-anslutning till ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="9789b-104">Learn how to interactively run Pig jobs from an SSH connection to your HDInsight cluster.</span></span> <span data-ttu-id="9789b-105">Pig Latin programmeringsspråk kan du beskriva transformationer som tillämpas på indata till önskat resultat.</span><span class="sxs-lookup"><span data-stu-id="9789b-105">The Pig Latin programming language allows you to describe transformations that are applied to the input data to produce the desired output.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9789b-106">Stegen i det här dokumentet kräver ett Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="9789b-106">The steps in this document require a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="9789b-107">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="9789b-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="9789b-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="9789b-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="9789b-109"><a id="ssh"></a>Ansluta med SSH</span><span class="sxs-lookup"><span data-stu-id="9789b-109"><a id="ssh"></a>Connect with SSH</span></span>

<span data-ttu-id="9789b-110">Använda SSH för att ansluta till ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="9789b-110">Use SSH to connect to your HDInsight cluster.</span></span> <span data-ttu-id="9789b-111">I följande exempel ansluter till ett kluster med namnet **myhdinsight** som-konto med namnet **sshuser**:</span><span class="sxs-lookup"><span data-stu-id="9789b-111">The following example connects to a cluster named **myhdinsight** as the account named **sshuser**:</span></span>

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net

<span data-ttu-id="9789b-112">**Om du har angett en certifikat-nyckel för SSH-autentisering** när du har skapat HDInsight-klustret kan du behöva ange platsen för den privata nyckeln på datorn för klienten.</span><span class="sxs-lookup"><span data-stu-id="9789b-112">**If you provided a certificate key for SSH authentication** when you created the HDInsight cluster, you may need to specify the location of the private key on your client system.</span></span>

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

<span data-ttu-id="9789b-113">**Om du har angett ett lösenord för SSH-autentisering** när du skapade HDInsight-kluster, ange lösenordet när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="9789b-113">**If you provided a password for SSH authentication** when you created the HDInsight cluster, provide the password when prompted.</span></span>

<span data-ttu-id="9789b-114">Mer information om hur du använder SSH med HDInsight finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="9789b-114">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="9789b-115"><a id="pig"></a>Använd kommandot Pig</span><span class="sxs-lookup"><span data-stu-id="9789b-115"><a id="pig"></a>Use the Pig command</span></span>

1. <span data-ttu-id="9789b-116">När du är ansluten, starta Pig-kommandoradsgränssnittet (CLI) med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9789b-116">Once connected, start the Pig command-line interface (CLI) by using the following command:</span></span>

        pig

    <span data-ttu-id="9789b-117">Efter en stund bör du se en `grunt>` prompt.</span><span class="sxs-lookup"><span data-stu-id="9789b-117">After a moment, you should see a `grunt>` prompt.</span></span>

2. <span data-ttu-id="9789b-118">Ange följande sats:</span><span class="sxs-lookup"><span data-stu-id="9789b-118">Enter the following statement:</span></span>

        LOGS = LOAD '/example/data/sample.log';

    <span data-ttu-id="9789b-119">Det här kommandot läser in innehållet i filen sample.log i LOGGARNA.</span><span class="sxs-lookup"><span data-stu-id="9789b-119">This command loads the contents of the sample.log file into LOGS.</span></span> <span data-ttu-id="9789b-120">Du kan visa innehållet i filen med hjälp av följande sats:</span><span class="sxs-lookup"><span data-stu-id="9789b-120">You can view the contents of the file by using the following statement:</span></span>

        DUMP LOGS;

3. <span data-ttu-id="9789b-121">Därefter Transformera data genom att använda ett reguljärt uttryck för att extrahera endast loggningsnivån för varje post med hjälp av följande sats:</span><span class="sxs-lookup"><span data-stu-id="9789b-121">Next, transform the data by applying a regular expression to extract only the logging level from each record by using the following statement:</span></span>

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    <span data-ttu-id="9789b-122">Du kan använda **DUMP** att visa data efter omvandlingen.</span><span class="sxs-lookup"><span data-stu-id="9789b-122">You can use **DUMP** to view the data after the transformation.</span></span> <span data-ttu-id="9789b-123">I det här fallet använder `DUMP LEVELS;`.</span><span class="sxs-lookup"><span data-stu-id="9789b-123">In this case, use `DUMP LEVELS;`.</span></span>

4. <span data-ttu-id="9789b-124">Fortsätta att tillämpa transformationer med hjälp av instruktionerna i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="9789b-124">Continue applying transformations by using the statements in the following table:</span></span>

    | <span data-ttu-id="9789b-125">Pig Latin-instruktion</span><span class="sxs-lookup"><span data-stu-id="9789b-125">Pig Latin statement</span></span> | <span data-ttu-id="9789b-126">Instruktionen har</span><span class="sxs-lookup"><span data-stu-id="9789b-126">What the statement does</span></span> |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | <span data-ttu-id="9789b-127">Tar bort rader som innehåller ett null-värde för loggningsnivån och lagrar resultaten i `FILTEREDLEVELS`.</span><span class="sxs-lookup"><span data-stu-id="9789b-127">Removes rows that contain a null value for the log level and stores the results into `FILTEREDLEVELS`.</span></span> |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | <span data-ttu-id="9789b-128">Grupperar raderna genom att loggningsnivån och lagrar resultaten i `GROUPEDLEVELS`.</span><span class="sxs-lookup"><span data-stu-id="9789b-128">Groups the rows by log level and stores the results into `GROUPEDLEVELS`.</span></span> |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | <span data-ttu-id="9789b-129">Skapar en mängd som innehåller varje unik logg värde och hur många gånger den sker.</span><span class="sxs-lookup"><span data-stu-id="9789b-129">Creates a set of data that contains each unique log level value and how many times it occurs.</span></span> <span data-ttu-id="9789b-130">De uppgifter som lagras i `FREQUENCIES`.</span><span class="sxs-lookup"><span data-stu-id="9789b-130">The data set is stored into `FREQUENCIES`.</span></span> |
    | `RESULT = order FREQUENCIES by COUNT desc;` | <span data-ttu-id="9789b-131">Sorterar loggningsnivåerna efter antal (fallande) och lagras i `RESULT`.</span><span class="sxs-lookup"><span data-stu-id="9789b-131">Orders the log levels by count (descending) and stores into `RESULT`.</span></span> |

    > [!TIP]
    > <span data-ttu-id="9789b-132">Använd `DUMP` att visa resultatet av transformeringen efter varje steg.</span><span class="sxs-lookup"><span data-stu-id="9789b-132">Use `DUMP` to view the result of the transformation after each step.</span></span>

5. <span data-ttu-id="9789b-133">Du kan också spara resultaten av en omvandling med hjälp av den `STORE` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="9789b-133">You can also save the results of a transformation by using the `STORE` statement.</span></span> <span data-ttu-id="9789b-134">Till exempel följande sats sparar den `RESULT` till den `/example/data/pigout` på standardlagring för klustret:</span><span class="sxs-lookup"><span data-stu-id="9789b-134">For example, the following statement saves the `RESULT` to the `/example/data/pigout` directory on the default storage for your cluster:</span></span>

        STORE RESULT into '/example/data/pigout';

   > [!NOTE]
   > <span data-ttu-id="9789b-135">Data lagras i katalogen som angetts i filer med namnet `part-nnnnn`.</span><span class="sxs-lookup"><span data-stu-id="9789b-135">The data is stored in the specified directory in files named `part-nnnnn`.</span></span> <span data-ttu-id="9789b-136">Om mappen redan finns felmeddelande ett.</span><span class="sxs-lookup"><span data-stu-id="9789b-136">If the directory already exists, you receive an error.</span></span>

6. <span data-ttu-id="9789b-137">Om du vill avsluta grunt prompten anger du följande sats:</span><span class="sxs-lookup"><span data-stu-id="9789b-137">To exit the grunt prompt, enter the following statement:</span></span>

        QUIT;

### <a name="pig-latin-batch-files"></a><span data-ttu-id="9789b-138">Pig Latin batch-filer</span><span class="sxs-lookup"><span data-stu-id="9789b-138">Pig Latin batch files</span></span>

<span data-ttu-id="9789b-139">Du kan också använda Pig-kommandot för att köra Pig Latin i en fil.</span><span class="sxs-lookup"><span data-stu-id="9789b-139">You can also use the Pig command to run Pig Latin contained in a file.</span></span>

1. <span data-ttu-id="9789b-140">När du avslutar grunt uppmaningen, använder du följande kommando till pipe STDIN till en fil med namnet `pigbatch.pig`.</span><span class="sxs-lookup"><span data-stu-id="9789b-140">After exiting the grunt prompt, use the following command to pipe STDIN into a file named `pigbatch.pig`.</span></span> <span data-ttu-id="9789b-141">Den här filen skapas i arbetskatalogen för SSH-användarkontot.</span><span class="sxs-lookup"><span data-stu-id="9789b-141">This file is created in the home directory for the SSH user account.</span></span>

        cat > ~/pigbatch.pig

2. <span data-ttu-id="9789b-142">Skriv eller klistra in följande rader och sedan använda Ctrl + D när du är klar.</span><span class="sxs-lookup"><span data-stu-id="9789b-142">Type or paste the following lines, and then use Ctrl+D when finished.</span></span>

        LOGS = LOAD '/example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

3. <span data-ttu-id="9789b-143">Använd följande kommando för att köra den `pigbatch.pig` filen med hjälp av kommandot Pig.</span><span class="sxs-lookup"><span data-stu-id="9789b-143">Use the following command to run the `pigbatch.pig` file by using the Pig command.</span></span>

        pig ~/pigbatch.pig

    <span data-ttu-id="9789b-144">När batch-jobbet har slutförts visas följande utdata:</span><span class="sxs-lookup"><span data-stu-id="9789b-144">Once the batch job finishes, you see the following output:</span></span>

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <span data-ttu-id="9789b-145"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9789b-145"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="9789b-146">Allmän information om Pig i HDInsight finns i följande dokument:</span><span class="sxs-lookup"><span data-stu-id="9789b-146">For general information on Pig in HDInsight, see the following document:</span></span>

* [<span data-ttu-id="9789b-147">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="9789b-147">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="9789b-148">Mer information om andra sätt att arbeta med Hadoop i HDInsight finns i följande dokument:</span><span class="sxs-lookup"><span data-stu-id="9789b-148">For more information on other ways to work with Hadoop on HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="9789b-149">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="9789b-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="9789b-150">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="9789b-150">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
