---
title: aaaUse C# med MapReduce med Hadoop i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toouse C# toocreate MapReduce lösningar med Hadoop i Azure HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d83def76-12ad-4538-bb8e-3ba3542b7211
ms.custom: hdinsightactive
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: dd8b684e74155bc1a37d4ab8d6f9033276ef5aa3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a><span data-ttu-id="21f72-103">Använda C# med MapReduce strömning på Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="21f72-103">Use C# with MapReduce streaming on Hadoop in HDInsight</span></span>

<span data-ttu-id="21f72-104">Lär dig hur toouse C# toocreate MapReduce lösningar för HDInsight.</span><span class="sxs-lookup"><span data-stu-id="21f72-104">Learn how toouse C# toocreate a MapReduce solution on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21f72-105">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="21f72-105">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="21f72-106">Mer information finns i [HDInsight component-versioning](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="21f72-106">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="21f72-107">Hadoop streaming är ett verktyg som gör att du använder ett skript eller körbara filer toorun MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="21f72-107">Hadoop streaming is a utility that allows you toorun MapReduce jobs using a script or executable.</span></span> <span data-ttu-id="21f72-108">I det här exemplet är .NET används tooimplement hello mapper och reducer för en word antal lösning.</span><span class="sxs-lookup"><span data-stu-id="21f72-108">In this example, .NET is used tooimplement hello mapper and reducer for a word count solution.</span></span>

## <a name="net-on-hdinsight"></a><span data-ttu-id="21f72-109">.NET på HDInsight</span><span class="sxs-lookup"><span data-stu-id="21f72-109">.NET on HDInsight</span></span>

<span data-ttu-id="21f72-110">__Linux-baserat HDInsight__ kluster Använd [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET-program.</span><span class="sxs-lookup"><span data-stu-id="21f72-110">__Linux-based HDInsight__ clusters use [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET applications.</span></span> <span data-ttu-id="21f72-111">Monoljud version 4.2.1 ingår i HDInsight version 3.5.</span><span class="sxs-lookup"><span data-stu-id="21f72-111">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span> <span data-ttu-id="21f72-112">Mer information om hello version Mono som ingår i HDInsight finns [HDInsight komponenten versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="21f72-112">For more information on hello version of Mono included with HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="21f72-113">toouse en viss version av Mono, se hello [installera eller uppdatera Mono](hdinsight-hadoop-install-mono.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="21f72-113">toouse a specific version of Mono, see hello [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

<span data-ttu-id="21f72-114">Mer information om monoljud kompatibilitet med .NET Framework-versioner finns [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="21f72-114">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

## <a name="how-hadoop-streaming-works"></a><span data-ttu-id="21f72-115">Så här fungerar Hadoop-strömning</span><span class="sxs-lookup"><span data-stu-id="21f72-115">How Hadoop streaming works</span></span>

<span data-ttu-id="21f72-116">hello grundläggande process används för strömning i det här dokumentet är följande:</span><span class="sxs-lookup"><span data-stu-id="21f72-116">hello basic process used for streaming in this document is as follows:</span></span>

1. <span data-ttu-id="21f72-117">Hadoop skickar datamappning toohello (mapper.exe i det här exemplet) på STDIN.</span><span class="sxs-lookup"><span data-stu-id="21f72-117">Hadoop passes data toohello mapper (mapper.exe in this example) on STDIN.</span></span>
2. <span data-ttu-id="21f72-118">hello mapper bearbetar hello data och genererar tabbavgränsad nyckel/värde-par tooSTDOUT.</span><span class="sxs-lookup"><span data-stu-id="21f72-118">hello mapper processes hello data, and emits tab-delimited key/value pairs tooSTDOUT.</span></span>
3. <span data-ttu-id="21f72-119">hello utdata läses av Hadoop och skickas sedan STDIN toohello reducer (reducer.exe i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="21f72-119">hello output is read by Hadoop, and then passed toohello reducer (reducer.exe in this example) on STDIN.</span></span>
4. <span data-ttu-id="21f72-120">Hej reducer läser hello tabbavgränsad nyckel/värde-par, bearbetar hello data och skickar sedan hello resultatet som tabbavgränsad nyckel/värde-par i STDOUT.</span><span class="sxs-lookup"><span data-stu-id="21f72-120">hello reducer reads hello tab-delimited key/value pairs, processes hello data, and then emits hello result as tab-delimited key/value pairs on STDOUT.</span></span>
5. <span data-ttu-id="21f72-121">hello utdata läses av Hadoop och skrivs toohello målkatalogen.</span><span class="sxs-lookup"><span data-stu-id="21f72-121">hello output is read by Hadoop and written toohello output directory.</span></span>

<span data-ttu-id="21f72-122">Mer information om strömning finns [Hadoop Streaming (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).</span><span class="sxs-lookup"><span data-stu-id="21f72-122">For more information on streaming, see [Hadoop Streaming (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21f72-123">Krav</span><span class="sxs-lookup"><span data-stu-id="21f72-123">Prerequisites</span></span>

* <span data-ttu-id="21f72-124">Tidigare erfarenhet av skrivning och bygga C#-kod som riktar sig till .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="21f72-124">A familiarity with writing and building C# code that targets .NET Framework 4.5.</span></span> <span data-ttu-id="21f72-125">hello stegen i det här dokumentet används Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="21f72-125">hello steps in this document use Visual Studio 2017.</span></span>

* <span data-ttu-id="21f72-126">Ett sätt tooupload .exe-filer toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="21f72-126">A way tooupload .exe files toohello cluster.</span></span> <span data-ttu-id="21f72-127">hello använder stegen i det här dokumentet hello Data Lake-verktyg för Visual Studio tooupload hello filer tooprimary lagringen för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="21f72-127">hello steps in this document use hello Data Lake Tools for Visual Studio tooupload hello files tooprimary storage for hello cluster.</span></span>

* <span data-ttu-id="21f72-128">Azure PowerShell eller en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="21f72-128">Azure PowerShell or a SSH client.</span></span>

* <span data-ttu-id="21f72-129">En Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="21f72-129">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="21f72-130">Mer information om hur du skapar ett kluster finns [skapar ett HDInsight-kluster](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="21f72-130">For more information on creating a cluster, see [Create an HDInsight cluster](hdinsight-provision-clusters.md).</span></span>

## <a name="create-hello-mapper"></a><span data-ttu-id="21f72-131">Skapa hello mapper</span><span class="sxs-lookup"><span data-stu-id="21f72-131">Create hello mapper</span></span>

<span data-ttu-id="21f72-132">I Visual Studio skapar en ny __Konsolapp__ med namnet __mapper__.</span><span class="sxs-lookup"><span data-stu-id="21f72-132">In Visual Studio, create a new __Console application__ named __mapper__.</span></span> <span data-ttu-id="21f72-133">Använd följande kod för hello programmet hello:</span><span class="sxs-lookup"><span data-stu-id="21f72-133">Use hello following code for hello application:</span></span>

```csharp
using System;
using System.Text.RegularExpressions;

namespace mapper
{
    class Program
    {
        static void Main(string[] args)
        {
            string line;
            //Hadoop passes data toohello mapper on STDIN
            while((line = Console.ReadLine()) != null)
            {
                // We only want words, so strip out punctuation, numbers, etc.
                var onlyText = Regex.Replace(line, @"\.|;|:|,|[0-9]|'", "");
                // Split at whitespace.
                var words = Regex.Matches(onlyText, @"[\w]+");
                // Loop over hello words
                foreach(var word in words)
                {
                    //Emit tab-delimited key/value pairs.
                    //In this case, a word and a count of 1.
                    Console.WriteLine("{0}\t1",word);
                }
            }
        }
    }
}
```

<span data-ttu-id="21f72-134">När du har skapat programmet hello skapar det tooproduce hello `/bin/Debug/mapper.exe` filen i hello projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="21f72-134">After creating hello application, build it tooproduce hello `/bin/Debug/mapper.exe` file in hello project directory.</span></span>

## <a name="create-hello-reducer"></a><span data-ttu-id="21f72-135">Skapa hello reducer</span><span class="sxs-lookup"><span data-stu-id="21f72-135">Create hello reducer</span></span>

<span data-ttu-id="21f72-136">I Visual Studio skapar en ny __Konsolapp__ med namnet __reducer__.</span><span class="sxs-lookup"><span data-stu-id="21f72-136">In Visual Studio, create a new __Console application__ named __reducer__.</span></span> <span data-ttu-id="21f72-137">Använd följande kod för hello programmet hello:</span><span class="sxs-lookup"><span data-stu-id="21f72-137">Use hello following code for hello application:</span></span>

```csharp
using System;
using System.Collections.Generic;

namespace reducer
{
    class Program
    {
        static void Main(string[] args)
        {
            //Dictionary for holding a count of words
            Dictionary<string, int> words = new Dictionary<string, int>();

            string line;
            //Read from STDIN
            while ((line = Console.ReadLine()) != null)
            {
                // Data from Hadoop is tab-delimited key/value pairs
                var sArr = line.Split('\t');
                // Get hello word
                string word = sArr[0];
                // Get hello count
                int count = Convert.ToInt32(sArr[1]);

                //Do we already have a count for hello word?
                if(words.ContainsKey(word))
                {
                    //If so, increment hello count
                    words[word] += count;
                } else
                {
                    //Add hello key toohello collection
                    words.Add(word, count);
                }
            }
            //Finally, emit each word and count
            foreach (var word in words)
            {
                //Emit tab-delimited key/value pairs.
                //In this case, a word and a count of 1.
                Console.WriteLine("{0}\t{1}", word.Key, word.Value);
            }
        }
    }
}
```

<span data-ttu-id="21f72-138">När du har skapat programmet hello skapar det tooproduce hello `/bin/Debug/reducer.exe` filen i hello projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="21f72-138">After creating hello application, build it tooproduce hello `/bin/Debug/reducer.exe` file in hello project directory.</span></span>

## <a name="upload-toostorage"></a><span data-ttu-id="21f72-139">Överför toostorage</span><span class="sxs-lookup"><span data-stu-id="21f72-139">Upload toostorage</span></span>

1. <span data-ttu-id="21f72-140">Öppna i Visual Studio **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="21f72-140">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="21f72-141">Expandera **Azure** och expandera därefter **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="21f72-141">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="21f72-142">Om du uppmanas ange dina autentiseringsuppgifter för Azure-prenumeration och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="21f72-142">If prompted, enter your Azure subscription credentials, and then click **Sign In**.</span></span>

4. <span data-ttu-id="21f72-143">Expandera hello HDInsight-kluster som du vill toodeploy programmet.</span><span class="sxs-lookup"><span data-stu-id="21f72-143">Expand hello HDInsight cluster that you wish toodeploy this application to.</span></span> <span data-ttu-id="21f72-144">En post med hello text __(standard Storage-konto)__ visas.</span><span class="sxs-lookup"><span data-stu-id="21f72-144">An entry with hello text __(Default Storage Account)__ is listed.</span></span>

    ![Server Explorer som visar hello storage-konto för hello-kluster](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * <span data-ttu-id="21f72-146">Om den här posten kan utökas måste du använder en __Azure Storage-konto__ som standard lagringen för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="21f72-146">If this entry can be expanded, you are using an __Azure Storage Account__ as default storage for hello cluster.</span></span> <span data-ttu-id="21f72-147">tooview hello filer på hello standardlagring för hello-kluster expanderar hello-post och dubbelklicka sedan på hello __(standardbehållaren)__.</span><span class="sxs-lookup"><span data-stu-id="21f72-147">tooview hello files on hello default storage for hello cluster, expand hello entry and then double-click hello __(Default Container)__.</span></span>

    * <span data-ttu-id="21f72-148">Om den här posten inte kan expanderas, använder du __Azure Data Lake Store__ som hello standardlagring för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="21f72-148">If this entry cannot be expanded, you are using __Azure Data Lake Store__ as hello default storage for hello cluster.</span></span> <span data-ttu-id="21f72-149">tooview hello filer på hello standardlagring för hello kluster, dubbelklicka på hello __(standardkontot för lagring)__ post.</span><span class="sxs-lookup"><span data-stu-id="21f72-149">tooview hello files on hello default storage for hello cluster, double-click hello __(Default Storage Account)__ entry.</span></span>

5. <span data-ttu-id="21f72-150">tooupload hello .exe-filer med någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="21f72-150">tooupload hello .exe files, use one of hello following methods:</span></span>

    * <span data-ttu-id="21f72-151">Om du använder en __Azure Storage-konto__, klicka på ikonen för hello överföringen och bläddra sedan toohello **bin\debug** mapp för hello **mapper** projekt.</span><span class="sxs-lookup"><span data-stu-id="21f72-151">If using an __Azure Storage Account__, click hello upload icon, and then browse toohello **bin\debug** folder for hello **mapper** project.</span></span> <span data-ttu-id="21f72-152">Välj slutligen hello **mapper.exe** fil och klicka på **Ok**.</span><span class="sxs-lookup"><span data-stu-id="21f72-152">Finally, select hello **mapper.exe** file and click **Ok**.</span></span>

        ![Överför ikon](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * <span data-ttu-id="21f72-154">Om du använder __Azure Data Lake Store__, högerklicka på ett tomt område i hello med och välj sedan __överför__.</span><span class="sxs-lookup"><span data-stu-id="21f72-154">If using __Azure Data Lake Store__, right-click an empty area in hello file listing, and then select __Upload__.</span></span> <span data-ttu-id="21f72-155">Välj slutligen hello **mapper.exe** fil och klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="21f72-155">Finally, select hello **mapper.exe** file and click **Open**.</span></span>

    <span data-ttu-id="21f72-156">En gång hello __mapper.exe__ överföringen har slutförts, upprepa hello överföringsprocessen för hello __reducer.exe__ fil.</span><span class="sxs-lookup"><span data-stu-id="21f72-156">Once hello __mapper.exe__ upload has finished, repeat hello upload process for hello __reducer.exe__ file.</span></span>

## <a name="run-a-job-using-an-ssh-session"></a><span data-ttu-id="21f72-157">Kör ett jobb: använder en SSH-session</span><span class="sxs-lookup"><span data-stu-id="21f72-157">Run a job: Using an SSH session</span></span>

1. <span data-ttu-id="21f72-158">Använda SSH tooconnect toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="21f72-158">Use SSH tooconnect toohello HDInsight cluster.</span></span> <span data-ttu-id="21f72-159">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="21f72-159">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="21f72-160">Använd någon av följande kommando toostart hello MapReduce-jobb hello:</span><span class="sxs-lookup"><span data-stu-id="21f72-160">Use one of hello following command toostart hello MapReduce job:</span></span>

    * <span data-ttu-id="21f72-161">Om du använder __Datasjölager__ som standardlagring:</span><span class="sxs-lookup"><span data-stu-id="21f72-161">If using __Data Lake Store__ as default storage:</span></span>

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * <span data-ttu-id="21f72-162">Om du använder __Azure Storage__ som standardlagring:</span><span class="sxs-lookup"><span data-stu-id="21f72-162">If using __Azure Storage__ as default storage:</span></span>

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    <span data-ttu-id="21f72-163">hello följande lista beskrivs vad varje parameter är:</span><span class="sxs-lookup"><span data-stu-id="21f72-163">hello following list describes what each parameter does:</span></span>

    * <span data-ttu-id="21f72-164">`hadoop-streaming.jar`: hello jar-filen som innehåller hello strömning MapReduce-funktioner.</span><span class="sxs-lookup"><span data-stu-id="21f72-164">`hadoop-streaming.jar`: hello jar file that contains hello streaming MapReduce functionality.</span></span>
    * <span data-ttu-id="21f72-165">`-files`: Lägger till hello `mapper.exe` och `reducer.exe` filer toothis jobb.</span><span class="sxs-lookup"><span data-stu-id="21f72-165">`-files`: Adds hello `mapper.exe` and `reducer.exe` files toothis job.</span></span> <span data-ttu-id="21f72-166">Hej `adl:///` eller `wasb:///` innan varje fil är hello sökvägen toohello roten standardlagring för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="21f72-166">hello `adl:///` or `wasb:///` before each file is hello path toohello root of default storage for hello cluster.</span></span>
    * <span data-ttu-id="21f72-167">`-mapper`: Anger vilken fil som implementerar hello mapper.</span><span class="sxs-lookup"><span data-stu-id="21f72-167">`-mapper`: Specifies which file implements hello mapper.</span></span>
    * <span data-ttu-id="21f72-168">`-reducer`: Anger vilken fil som implementerar hello reducer.</span><span class="sxs-lookup"><span data-stu-id="21f72-168">`-reducer`: Specifies which file implements hello reducer.</span></span>
    * <span data-ttu-id="21f72-169">`-input`: hello indata.</span><span class="sxs-lookup"><span data-stu-id="21f72-169">`-input`: hello input data.</span></span>
    * <span data-ttu-id="21f72-170">`-output`: hello målkatalogen.</span><span class="sxs-lookup"><span data-stu-id="21f72-170">`-output`: hello output directory.</span></span>

3. <span data-ttu-id="21f72-171">När hello MapReduce-jobbet har slutförts använder du följande tooview hello resultat hello:</span><span class="sxs-lookup"><span data-stu-id="21f72-171">Once hello MapReduce job completes, use hello following tooview hello results:</span></span>

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    <span data-ttu-id="21f72-172">hello är följande ett exempel på hello data som returneras av kommandot:</span><span class="sxs-lookup"><span data-stu-id="21f72-172">hello following text is an example of hello data returned by this command:</span></span>

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a><span data-ttu-id="21f72-173">Kör ett jobb: med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="21f72-173">Run a job: Using PowerShell</span></span>

<span data-ttu-id="21f72-174">Använd följande PowerShell-skriptet toorun ett MapReduce-jobb hello och hämta hello resultat.</span><span class="sxs-lookup"><span data-stu-id="21f72-174">Use hello following PowerShell script toorun a MapReduce job and download hello results.</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

<span data-ttu-id="21f72-175">Det här skriptet efterfrågar hello klustret konto inloggningsnamn och lösenord, tillsammans med hello HDInsight-klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="21f72-175">This script prompts you for hello cluster login account name and password, along with hello HDInsight cluster name.</span></span> <span data-ttu-id="21f72-176">När hello jobbet är slutfört, hello utdata är hämtade toohello `output.txt` filen i hello directory hello skriptet kördes från.</span><span class="sxs-lookup"><span data-stu-id="21f72-176">Once hello job completes, hello output is downloaded toohello `output.txt` file in hello directory hello script is ran from.</span></span> <span data-ttu-id="21f72-177">hello följande är ett exempel på hello data i hello `output.txt` fil:</span><span class="sxs-lookup"><span data-stu-id="21f72-177">hello following text is an example of hello data in hello `output.txt` file:</span></span>

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a><span data-ttu-id="21f72-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="21f72-178">Next steps</span></span>

<span data-ttu-id="21f72-179">Mer information om hur du använder MapReduce med HDInsight finns [använda MapReduce med HDInsight](hdinsight-use-mapreduce.md).</span><span class="sxs-lookup"><span data-stu-id="21f72-179">For more information on using MapReduce with HDInsight, see [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md).</span></span>

<span data-ttu-id="21f72-180">Information om hur du använder C# med Hive och Pig finns [använder en C# användardefinierad funktion med Hive och Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="21f72-180">For information on using C# with Hive and Pig, see [Use a C# user defined function with Hive and Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).</span></span>

<span data-ttu-id="21f72-181">Information om hur du använder C# med Storm på HDInsight finns i [utveckla C#-topologier för Storm på HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="21f72-181">For information on using C# with Storm on HDInsight, see [Develop C# topologies for Storm on HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>