---
title: "aaaUse Hadoop Pig med SSH på ett HDInsight-kluster - Azure | Microsoft Docs"
description: "Lär dig hur ansluter tooa Linux-baserade Hadoop-kluster med SSH och sedan använda hello Pig kommandot toorun Pig Latin instruktioner interaktivt eller som en batch jobbet."
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
ms.openlocfilehash: 1da303e239b537e6b331b1d33010058582718c90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-hello-pig-command-ssh"></a><span data-ttu-id="7a523-103">Köra Pig-jobb på en Linux-baserade kluster med hello Pig-kommandot (SSH)</span><span class="sxs-lookup"><span data-stu-id="7a523-103">Run Pig jobs on a Linux-based cluster with hello Pig command (SSH)</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="7a523-104">Lär dig hur toointeractively köra Pig-jobb från en SSH-anslutning tooyour HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7a523-104">Learn how toointeractively run Pig jobs from an SSH connection tooyour HDInsight cluster.</span></span> <span data-ttu-id="7a523-105">Hej Pig Latin programmeringsspråk kan toodescribe transformationer som är kopplade toohello inkommande data tooproduce hello önskade resultatet.</span><span class="sxs-lookup"><span data-stu-id="7a523-105">hello Pig Latin programming language allows you toodescribe transformations that are applied toohello input data tooproduce hello desired output.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7a523-106">hello kräver stegen i det här dokumentet ett Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7a523-106">hello steps in this document require a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="7a523-107">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7a523-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="7a523-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="7a523-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="7a523-109"><a id="ssh"></a>Ansluta med SSH</span><span class="sxs-lookup"><span data-stu-id="7a523-109"><a id="ssh"></a>Connect with SSH</span></span>

<span data-ttu-id="7a523-110">Använda SSH tooconnect tooyour HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7a523-110">Use SSH tooconnect tooyour HDInsight cluster.</span></span> <span data-ttu-id="7a523-111">hello följande exempel ansluter tooa kluster med namnet **myhdinsight** som hello-kontot med namnet **sshuser**:</span><span class="sxs-lookup"><span data-stu-id="7a523-111">hello following example connects tooa cluster named **myhdinsight** as hello account named **sshuser**:</span></span>

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net

<span data-ttu-id="7a523-112">**Om du har angett en certifikat-nyckel för SSH-autentisering** när du har skapat hello HDInsight-kluster kan du kanske måste toospecify hello platsen för hello privata nyckel på klientsystemet.</span><span class="sxs-lookup"><span data-stu-id="7a523-112">**If you provided a certificate key for SSH authentication** when you created hello HDInsight cluster, you may need toospecify hello location of hello private key on your client system.</span></span>

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

<span data-ttu-id="7a523-113">**Om du har angett ett lösenord för SSH-autentisering** när du skapade hello HDInsight-kluster, ange hello lösenord när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="7a523-113">**If you provided a password for SSH authentication** when you created hello HDInsight cluster, provide hello password when prompted.</span></span>

<span data-ttu-id="7a523-114">Mer information om hur du använder SSH med HDInsight finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="7a523-114">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="7a523-115"><a id="pig"></a>Använd hello Pig kommando</span><span class="sxs-lookup"><span data-stu-id="7a523-115"><a id="pig"></a>Use hello Pig command</span></span>

1. <span data-ttu-id="7a523-116">När du är ansluten, startar du hello Pig-kommandoradsgränssnittet (CLI) med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7a523-116">Once connected, start hello Pig command-line interface (CLI) by using hello following command:</span></span>

        pig

    <span data-ttu-id="7a523-117">Efter en stund bör du se en `grunt>` prompt.</span><span class="sxs-lookup"><span data-stu-id="7a523-117">After a moment, you should see a `grunt>` prompt.</span></span>

2. <span data-ttu-id="7a523-118">Ange hello följande instruktion:</span><span class="sxs-lookup"><span data-stu-id="7a523-118">Enter hello following statement:</span></span>

        LOGS = LOAD '/example/data/sample.log';

    <span data-ttu-id="7a523-119">Det här kommandot läser in hello innehållet i hello sample.log fil i LOGGARNA.</span><span class="sxs-lookup"><span data-stu-id="7a523-119">This command loads hello contents of hello sample.log file into LOGS.</span></span> <span data-ttu-id="7a523-120">Du kan visa hello innehållet i hello-fil med hjälp av följande instruktion hello:</span><span class="sxs-lookup"><span data-stu-id="7a523-120">You can view hello contents of hello file by using hello following statement:</span></span>

        DUMP LOGS;

3. <span data-ttu-id="7a523-121">Därefter transformera hello data genom att använda ett reguljärt uttryck tooextract endast hello loggningsnivån för varje post med hjälp av följande instruktion hello:</span><span class="sxs-lookup"><span data-stu-id="7a523-121">Next, transform hello data by applying a regular expression tooextract only hello logging level from each record by using hello following statement:</span></span>

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    <span data-ttu-id="7a523-122">Du kan använda **DUMP** tooview hello data efter hello omvandling.</span><span class="sxs-lookup"><span data-stu-id="7a523-122">You can use **DUMP** tooview hello data after hello transformation.</span></span> <span data-ttu-id="7a523-123">I det här fallet använder `DUMP LEVELS;`.</span><span class="sxs-lookup"><span data-stu-id="7a523-123">In this case, use `DUMP LEVELS;`.</span></span>

4. <span data-ttu-id="7a523-124">Fortsätta att tillämpa transformationer med hjälp av hello instruktioner i den följande tabellen hello:</span><span class="sxs-lookup"><span data-stu-id="7a523-124">Continue applying transformations by using hello statements in hello following table:</span></span>

    | <span data-ttu-id="7a523-125">Pig Latin-instruktion</span><span class="sxs-lookup"><span data-stu-id="7a523-125">Pig Latin statement</span></span> | <span data-ttu-id="7a523-126">Vilka hello-instruktionen har</span><span class="sxs-lookup"><span data-stu-id="7a523-126">What hello statement does</span></span> |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | <span data-ttu-id="7a523-127">Tar bort rader som innehåller ett null-värde för hello loggningsnivån och lagrar hello resultat i `FILTEREDLEVELS`.</span><span class="sxs-lookup"><span data-stu-id="7a523-127">Removes rows that contain a null value for hello log level and stores hello results into `FILTEREDLEVELS`.</span></span> |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | <span data-ttu-id="7a523-128">Grupper hello rader genom att loggningsnivån och lagrar hello resultat i `GROUPEDLEVELS`.</span><span class="sxs-lookup"><span data-stu-id="7a523-128">Groups hello rows by log level and stores hello results into `GROUPEDLEVELS`.</span></span> |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | <span data-ttu-id="7a523-129">Skapar en mängd som innehåller varje unik logg värde och hur många gånger den sker.</span><span class="sxs-lookup"><span data-stu-id="7a523-129">Creates a set of data that contains each unique log level value and how many times it occurs.</span></span> <span data-ttu-id="7a523-130">hello data lagras i `FREQUENCIES`.</span><span class="sxs-lookup"><span data-stu-id="7a523-130">hello data set is stored into `FREQUENCIES`.</span></span> |
    | `RESULT = order FREQUENCIES by COUNT desc;` | <span data-ttu-id="7a523-131">Sorterar hello loggningsnivåer efter antal (fallande) och lagras i `RESULT`.</span><span class="sxs-lookup"><span data-stu-id="7a523-131">Orders hello log levels by count (descending) and stores into `RESULT`.</span></span> |

    > [!TIP]
    > <span data-ttu-id="7a523-132">Använd `DUMP` tooview hello resultatet av hello omvandling efter varje steg.</span><span class="sxs-lookup"><span data-stu-id="7a523-132">Use `DUMP` tooview hello result of hello transformation after each step.</span></span>

5. <span data-ttu-id="7a523-133">Du kan också spara hello resultatet av en omvandling med hjälp av hello `STORE` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="7a523-133">You can also save hello results of a transformation by using hello `STORE` statement.</span></span> <span data-ttu-id="7a523-134">Till exempel följande instruktion hello sparar hello `RESULT` toohello `/example/data/pigout` på hello standardlagring för klustret:</span><span class="sxs-lookup"><span data-stu-id="7a523-134">For example, hello following statement saves hello `RESULT` toohello `/example/data/pigout` directory on hello default storage for your cluster:</span></span>

        STORE RESULT into '/example/data/pigout';

   > [!NOTE]
   > <span data-ttu-id="7a523-135">hello data lagras i hello angiven katalog i filer med namnet `part-nnnnn`.</span><span class="sxs-lookup"><span data-stu-id="7a523-135">hello data is stored in hello specified directory in files named `part-nnnnn`.</span></span> <span data-ttu-id="7a523-136">Om hello directory redan finns felmeddelande ett.</span><span class="sxs-lookup"><span data-stu-id="7a523-136">If hello directory already exists, you receive an error.</span></span>

6. <span data-ttu-id="7a523-137">tooexit hello grunt prompten anger du följande instruktion hello:</span><span class="sxs-lookup"><span data-stu-id="7a523-137">tooexit hello grunt prompt, enter hello following statement:</span></span>

        QUIT;

### <a name="pig-latin-batch-files"></a><span data-ttu-id="7a523-138">Pig Latin batch-filer</span><span class="sxs-lookup"><span data-stu-id="7a523-138">Pig Latin batch files</span></span>

<span data-ttu-id="7a523-139">Du kan också använda hello Pig kommandot toorun Pig Latin i en fil.</span><span class="sxs-lookup"><span data-stu-id="7a523-139">You can also use hello Pig command toorun Pig Latin contained in a file.</span></span>

1. <span data-ttu-id="7a523-140">När du avslutar hello grunt kommandotolk, Använd hello följande kommando toopipe STDIN till en fil med namnet `pigbatch.pig`.</span><span class="sxs-lookup"><span data-stu-id="7a523-140">After exiting hello grunt prompt, use hello following command toopipe STDIN into a file named `pigbatch.pig`.</span></span> <span data-ttu-id="7a523-141">Den här filen har skapats i hello arbetskatalog för hello SSH-användarkontot.</span><span class="sxs-lookup"><span data-stu-id="7a523-141">This file is created in hello home directory for hello SSH user account.</span></span>

        cat > ~/pigbatch.pig

2. <span data-ttu-id="7a523-142">Skriv eller klistra in hello följande rader och sedan använda Ctrl + D när du är klar.</span><span class="sxs-lookup"><span data-stu-id="7a523-142">Type or paste hello following lines, and then use Ctrl+D when finished.</span></span>

        LOGS = LOAD '/example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

3. <span data-ttu-id="7a523-143">Använd hello följande kommando toorun hello `pigbatch.pig` filen med hello Pig kommando.</span><span class="sxs-lookup"><span data-stu-id="7a523-143">Use hello following command toorun hello `pigbatch.pig` file by using hello Pig command.</span></span>

        pig ~/pigbatch.pig

    <span data-ttu-id="7a523-144">När hello batch-jobbet har slutförts kan du se hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="7a523-144">Once hello batch job finishes, you see hello following output:</span></span>

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <span data-ttu-id="7a523-145"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7a523-145"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="7a523-146">Allmän information om Pig i HDInsight finns i hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="7a523-146">For general information on Pig in HDInsight, see hello following document:</span></span>

* [<span data-ttu-id="7a523-147">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7a523-147">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="7a523-148">Mer information om andra sätt toowork med Hadoop i HDInsight finns i hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="7a523-148">For more information on other ways toowork with Hadoop on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="7a523-149">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7a523-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="7a523-150">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7a523-150">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
