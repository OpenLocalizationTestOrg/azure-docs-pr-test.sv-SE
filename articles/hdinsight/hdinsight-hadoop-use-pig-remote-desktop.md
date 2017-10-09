---
title: "aaaUse Hadoop Pig med fjärrskrivbord i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse hello Pig kommandot toorun Pig Latin rapporter från ett fjärrskrivbord anslutning tooa Windows-baserade Hadoop-kluster i HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e034a286-de0f-465f-8bf1-3d085ca6abed
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 2a4565fa827cd45fdbe6194b0486df93a6561084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-from-a-remote-desktop-connection"></a><span data-ttu-id="ab461-103">Köra Pig-jobb från en fjärrskrivbordsanslutning</span><span class="sxs-lookup"><span data-stu-id="ab461-103">Run Pig jobs from a Remote Desktop connection</span></span>
[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="ab461-104">Det här dokumentet innehåller en genomgång för hello Pig kommandot toorun Pig Latin rapporter från ett fjärrskrivbord anslutning tooa Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ab461-104">This document provides a walkthrough for using hello Pig command toorun Pig Latin statements from a Remote Desktop connection tooa Windows-based HDInsight cluster.</span></span> <span data-ttu-id="ab461-105">Pig Latin kan du toocreate MapReduce program genom att beskriva Datatransformationer, snarare än mappa och minska funktioner.</span><span class="sxs-lookup"><span data-stu-id="ab461-105">Pig Latin allows you toocreate MapReduce applications by describing data transformations, rather than map and reduce functions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ab461-106">Fjärrskrivbord är bara tillgängligt på HDInsight-kluster som använder Windows som hello-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="ab461-106">Remote Desktop is only available on HDInsight clusters that use Windows as hello operating system.</span></span> <span data-ttu-id="ab461-107">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ab461-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ab461-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ab461-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="ab461-109">För HDInsight 3.4 eller större finns [använda Pig med HDInsight och SSH](hdinsight-hadoop-use-pig-ssh.md) information om hur du kör interaktivt Pig-jobb direkt på hello kluster från en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="ab461-109">For HDInsight 3.4 or greater, see [Use Pig with HDInsight and SSH](hdinsight-hadoop-use-pig-ssh.md) for information on interactively running Pig jobs directly on hello cluster from a command-line.</span></span>

## <span data-ttu-id="ab461-110"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="ab461-110"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="ab461-111">toocomplete hello stegen i den här artikeln, behöver du hello följande.</span><span class="sxs-lookup"><span data-stu-id="ab461-111">toocomplete hello steps in this article, you will need hello following.</span></span>

* <span data-ttu-id="ab461-112">Ett kluster med Windows-baserade HDInsight (Hadoop på HDInsight)</span><span class="sxs-lookup"><span data-stu-id="ab461-112">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="ab461-113">En klientdator som kör Windows 10, Windows 8 eller Windows 7</span><span class="sxs-lookup"><span data-stu-id="ab461-113">A client computer running Windows 10, Windows 8, or Windows 7</span></span>

## <span data-ttu-id="ab461-114"><a id="connect"></a>Ansluta med fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="ab461-114"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="ab461-115">Aktivera Fjärrskrivbord för hello HDInsight-kluster och sedan ansluta tooit genom att följa anvisningarna hello på [ansluta tooHDInsight kluster med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="ab461-115">Enable Remote Desktop for hello HDInsight cluster, then connect tooit by following hello instructions at [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="ab461-116"><a id="pig"></a>Använd hello Pig kommando</span><span class="sxs-lookup"><span data-stu-id="ab461-116"><a id="pig"></a>Use hello Pig command</span></span>
1. <span data-ttu-id="ab461-117">När du har en fjärrskrivbordsanslutning starta hello **Hadoop kommandoraden** med hjälp av hello-ikon på hello skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="ab461-117">After you have a Remote Desktop connection, start hello **Hadoop Command Line** by using hello icon on hello desktop.</span></span>
2. <span data-ttu-id="ab461-118">Använd följande kommando för toostart hello Pig hello:</span><span class="sxs-lookup"><span data-stu-id="ab461-118">Use hello following toostart hello Pig command:</span></span>

        %pig_home%\bin\pig

    <span data-ttu-id="ab461-119">Du kan välja en `grunt>` prompt.</span><span class="sxs-lookup"><span data-stu-id="ab461-119">You will be presented with a `grunt>` prompt.</span></span>
3. <span data-ttu-id="ab461-120">Ange hello följande instruktion:</span><span class="sxs-lookup"><span data-stu-id="ab461-120">Enter hello following statement:</span></span>

        LOGS = LOAD 'wasb:///example/data/sample.log';

    <span data-ttu-id="ab461-121">Det här kommandot laddar hello innehållet i hello sample.log fil till hello loggar filen.</span><span class="sxs-lookup"><span data-stu-id="ab461-121">This command loads hello contents of hello sample.log file into hello LOGS file.</span></span> <span data-ttu-id="ab461-122">Du kan visa hello innehållet i hello-fil med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ab461-122">You can view hello contents of hello file by using hello following command:</span></span>

        DUMP LOGS;
4. <span data-ttu-id="ab461-123">Transformera hello data genom att använda ett reguljärt uttryck tooextract endast hello loggningsnivån för varje post:</span><span class="sxs-lookup"><span data-stu-id="ab461-123">Transform hello data by applying a regular expression tooextract only hello logging level from each record:</span></span>

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    <span data-ttu-id="ab461-124">Du kan använda **DUMP** tooview hello data efter hello omvandling.</span><span class="sxs-lookup"><span data-stu-id="ab461-124">You can use **DUMP** tooview hello data after hello transformation.</span></span> <span data-ttu-id="ab461-125">I det här fallet `DUMP LEVELS;`.</span><span class="sxs-lookup"><span data-stu-id="ab461-125">In this case, `DUMP LEVELS;`.</span></span>
5. <span data-ttu-id="ab461-126">Fortsätta att tillämpa transformationer med hjälp av hello följande instruktioner.</span><span class="sxs-lookup"><span data-stu-id="ab461-126">Continue applying transformations by using hello following statements.</span></span> <span data-ttu-id="ab461-127">Använd `DUMP` tooview hello resultatet av hello omvandling efter varje steg.</span><span class="sxs-lookup"><span data-stu-id="ab461-127">Use `DUMP` tooview hello result of hello transformation after each step.</span></span>

    <table>
    <tr>
    <th><span data-ttu-id="ab461-128">Instruktionen</span><span class="sxs-lookup"><span data-stu-id="ab461-128">Statement</span></span></th><th><span data-ttu-id="ab461-129">Vad verktyget gör</span><span class="sxs-lookup"><span data-stu-id="ab461-129">What it does</span></span></th>
    </tr>
    <tr>
    <td><span data-ttu-id="ab461-130">FILTEREDLEVELS = FILTER nivåer av LOGLEVEL inte är null.</span><span class="sxs-lookup"><span data-stu-id="ab461-130">FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;</span></span></td><td><span data-ttu-id="ab461-131">Tar bort rader som innehåller ett null-värde för hello loggningsnivån och lagrar hello resultat i FILTEREDLEVELS.</span><span class="sxs-lookup"><span data-stu-id="ab461-131">Removes rows that contain a null value for hello log level and stores hello results into FILTEREDLEVELS.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="ab461-132">GROUPEDLEVELS = grupp FILTEREDLEVELS av LOGLEVEL;</span><span class="sxs-lookup"><span data-stu-id="ab461-132">GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;</span></span></td><td><span data-ttu-id="ab461-133">Grupper hello rader genom att loggningsnivån och lagrar hello resultat i GROUPEDLEVELS.</span><span class="sxs-lookup"><span data-stu-id="ab461-133">Groups hello rows by log level and stores hello results into GROUPEDLEVELS.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="ab461-134">FREKVENSERNA = foreach GROUPEDLEVELS generera grupp som LOGLEVEL COUNT (FILTEREDLEVELS. LOGLEVEL) som antal;</span><span class="sxs-lookup"><span data-stu-id="ab461-134">FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;</span></span></td><td><span data-ttu-id="ab461-135">Skapar en ny uppsättning som innehåller varje unik logg värde och hur många gånger den sker.</span><span class="sxs-lookup"><span data-stu-id="ab461-135">Creates a new set of data that contains each unique log level value and how many times it occurs.</span></span> <span data-ttu-id="ab461-136">Detta är lagrad i frekvenser</span><span class="sxs-lookup"><span data-stu-id="ab461-136">This is stored into FREQUENCIES</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="ab461-137">RESULTATET = order frekvenser av antal desc;</span><span class="sxs-lookup"><span data-stu-id="ab461-137">RESULT = order FREQUENCIES by COUNT desc;</span></span></td><td><span data-ttu-id="ab461-138">Sorterar hello loggningsnivåer efter antal (fallande) och lagras i resultatet</span><span class="sxs-lookup"><span data-stu-id="ab461-138">Orders hello log levels by count (descending,) and stores into RESULT</span></span></td>
    </tr>
    </table><span data-ttu-id="ab461-139">
6.Du kan också spara hello resultatet av en omvandling med hjälp av hello `STORE` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="ab461-139">
6. You can also save hello results of a transformation by using hello `STORE` statement.</span></span> <span data-ttu-id="ab461-140">Till exempel hello följande kommando sparar hello `RESULT` toohello **/example/data/pigout** katalog i hello standardbehållare för lagring för klustret:</span><span class="sxs-lookup"><span data-stu-id="ab461-140">For example, hello following command saves hello `RESULT` toohello **/example/data/pigout** directory in hello default storage container for your cluster:</span></span>

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > <span data-ttu-id="ab461-141">hello data lagras i hello angiven katalog i filer med namnet **del nnnnn**.</span><span class="sxs-lookup"><span data-stu-id="ab461-141">hello data is stored in hello specified directory in files named **part-nnnnn**.</span></span> <span data-ttu-id="ab461-142">Om hello katalogen redan finns visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="ab461-142">If hello directory already exists, you will receive an error message.</span></span>
   >
   >
7. <span data-ttu-id="ab461-143">tooexit hello grunt kommandoprompt, ange hello följande instruktion.</span><span class="sxs-lookup"><span data-stu-id="ab461-143">tooexit hello grunt prompt, enter hello following statement.</span></span>

        QUIT;

### <a name="pig-latin-batch-files"></a><span data-ttu-id="ab461-144">Pig Latin batch-filer</span><span class="sxs-lookup"><span data-stu-id="ab461-144">Pig Latin batch files</span></span>
<span data-ttu-id="ab461-145">Du kan också använda hello Pig kommandot toorun Pig Latin som ingår i en fil.</span><span class="sxs-lookup"><span data-stu-id="ab461-145">You can also use hello Pig command toorun Pig Latin that is contained in a file.</span></span>

1. <span data-ttu-id="ab461-146">När du avslutar hello grunt prompt öppna **anteckningar** och skapa en ny fil med namnet **pigbatch.pig** i hello **PIG_HOME %** directory.</span><span class="sxs-lookup"><span data-stu-id="ab461-146">After exiting hello grunt prompt, open **Notepad** and create a new file named **pigbatch.pig** in hello **%PIG_HOME%** directory.</span></span>
2. <span data-ttu-id="ab461-147">Skriv eller klistra in hello följande rader i hello **pigbatch.pig** filen och spara den:</span><span class="sxs-lookup"><span data-stu-id="ab461-147">Type or paste hello following lines into hello **pigbatch.pig** file, and then save it:</span></span>

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. <span data-ttu-id="ab461-148">Använd hello följande toorun hello **pigbatch.pig** filen hello pig kommandot.</span><span class="sxs-lookup"><span data-stu-id="ab461-148">Use hello following toorun hello **pigbatch.pig** file using hello pig command.</span></span>

        pig %PIG_HOME%\pigbatch.pig

    <span data-ttu-id="ab461-149">När hello batch-jobbet är klart du bör se hello följande utdata som ska vara hello densamma som när du använde `DUMP RESULT;` i hello föregående steg:</span><span class="sxs-lookup"><span data-stu-id="ab461-149">When hello batch job completes, you should see hello following output, which should be hello same as when you used `DUMP RESULT;` in hello previous steps:</span></span>

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <span data-ttu-id="ab461-150"><a id="summary"></a>Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ab461-150"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="ab461-151">Som du ser kan hello Pig-kommandot toointeractively kör MapReduce åtgärder eller köra Pig Latin-jobb som lagras i en batchfil.</span><span class="sxs-lookup"><span data-stu-id="ab461-151">As you can see, hello Pig command allows you toointeractively run MapReduce operations, or run Pig Latin jobs that are stored in a batch file.</span></span>

## <span data-ttu-id="ab461-152"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ab461-152"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="ab461-153">Allmän information om Pig i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="ab461-153">For general information about Pig in HDInsight:</span></span>

* [<span data-ttu-id="ab461-154">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab461-154">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="ab461-155">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="ab461-155">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="ab461-156">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab461-156">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="ab461-157">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab461-157">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
