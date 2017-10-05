---
title: "Använda C# med MapReduce med Hadoop i HDInsight - Azure | Microsoft Docs"
description: "Lär dig använda C# för att skapa lösningar för MapReduce med Hadoop i Azure HDInsight."
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
ms.openlocfilehash: adb454e56378a800c671614735aec78b6851aeb2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a><span data-ttu-id="cbb25-103">Använda C# med MapReduce strömning på Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="cbb25-103">Use C# with MapReduce streaming on Hadoop in HDInsight</span></span>

<span data-ttu-id="cbb25-104">Lär dig använda C# för att skapa en lösning för MapReduce på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cbb25-104">Learn how to use C# to create a MapReduce solution on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cbb25-105">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="cbb25-105">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="cbb25-106">Mer information finns i [HDInsight component-versioning](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="cbb25-106">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="cbb25-107">Hadoop streaming är ett verktyg som låter dig köra MapReduce-jobb med hjälp av ett skript eller en körbar fil.</span><span class="sxs-lookup"><span data-stu-id="cbb25-107">Hadoop streaming is a utility that allows you to run MapReduce jobs using a script or executable.</span></span> <span data-ttu-id="cbb25-108">I det här exemplet används .NET för att implementera mapper och reducer för en word antal lösning.</span><span class="sxs-lookup"><span data-stu-id="cbb25-108">In this example, .NET is used to implement the mapper and reducer for a word count solution.</span></span>

## <a name="net-on-hdinsight"></a><span data-ttu-id="cbb25-109">.NET på HDInsight</span><span class="sxs-lookup"><span data-stu-id="cbb25-109">.NET on HDInsight</span></span>

<span data-ttu-id="cbb25-110">__Linux-baserat HDInsight__ kluster Använd [Mono (https://mono-project.com)](https://mono-project.com) att köra .NET-program.</span><span class="sxs-lookup"><span data-stu-id="cbb25-110">__Linux-based HDInsight__ clusters use [Mono (https://mono-project.com)](https://mono-project.com) to run .NET applications.</span></span> <span data-ttu-id="cbb25-111">Monoljud version 4.2.1 ingår i HDInsight version 3.5.</span><span class="sxs-lookup"><span data-stu-id="cbb25-111">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span> <span data-ttu-id="cbb25-112">Mer information om versionen av Mono som ingår i HDInsight finns [HDInsight komponenten versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="cbb25-112">For more information on the version of Mono included with HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="cbb25-113">Om du vill använda en viss version av Mono, finns det [installera eller uppdatera Mono](hdinsight-hadoop-install-mono.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="cbb25-113">To use a specific version of Mono, see the [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

<span data-ttu-id="cbb25-114">Mer information om monoljud kompatibilitet med .NET Framework-versioner finns [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="cbb25-114">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

## <a name="how-hadoop-streaming-works"></a><span data-ttu-id="cbb25-115">Så här fungerar Hadoop-strömning</span><span class="sxs-lookup"><span data-stu-id="cbb25-115">How Hadoop streaming works</span></span>

<span data-ttu-id="cbb25-116">Den grundläggande processen som används för strömning i det här dokumentet är följande:</span><span class="sxs-lookup"><span data-stu-id="cbb25-116">The basic process used for streaming in this document is as follows:</span></span>

1. <span data-ttu-id="cbb25-117">Hadoop skickar data till mapper (mapper.exe i det här exemplet) på STDIN.</span><span class="sxs-lookup"><span data-stu-id="cbb25-117">Hadoop passes data to the mapper (mapper.exe in this example) on STDIN.</span></span>
2. <span data-ttu-id="cbb25-118">Mapparen bearbetar data och skickar tabbavgränsad nyckel/värde-par STDOUT.</span><span class="sxs-lookup"><span data-stu-id="cbb25-118">The mapper processes the data, and emits tab-delimited key/value pairs to STDOUT.</span></span>
3. <span data-ttu-id="cbb25-119">Utdata läses av Hadoop och skickas sedan till reducer (reducer.exe i det här exemplet) STDIN.</span><span class="sxs-lookup"><span data-stu-id="cbb25-119">The output is read by Hadoop, and then passed to the reducer (reducer.exe in this example) on STDIN.</span></span>
4. <span data-ttu-id="cbb25-120">Reducer läser tabbavgränsad nyckel/värde-par, bearbetar data och skickar sedan resultatet som tabbavgränsad nyckel/värde-par i STDOUT.</span><span class="sxs-lookup"><span data-stu-id="cbb25-120">The reducer reads the tab-delimited key/value pairs, processes the data, and then emits the result as tab-delimited key/value pairs on STDOUT.</span></span>
5. <span data-ttu-id="cbb25-121">Utdata läses av Hadoop och skrivs till den angivna katalogen.</span><span class="sxs-lookup"><span data-stu-id="cbb25-121">The output is read by Hadoop and written to the output directory.</span></span>

<span data-ttu-id="cbb25-122">Mer information om strömning finns [Hadoop Streaming (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).</span><span class="sxs-lookup"><span data-stu-id="cbb25-122">For more information on streaming, see [Hadoop Streaming (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cbb25-123">Krav</span><span class="sxs-lookup"><span data-stu-id="cbb25-123">Prerequisites</span></span>

* <span data-ttu-id="cbb25-124">Tidigare erfarenhet av skrivning och bygga C#-kod som riktar sig till .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="cbb25-124">A familiarity with writing and building C# code that targets .NET Framework 4.5.</span></span> <span data-ttu-id="cbb25-125">Stegen i det här dokumentet använder Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="cbb25-125">The steps in this document use Visual Studio 2017.</span></span>

* <span data-ttu-id="cbb25-126">Ett sätt att överföra .exe-filer till klustret.</span><span class="sxs-lookup"><span data-stu-id="cbb25-126">A way to upload .exe files to the cluster.</span></span> <span data-ttu-id="cbb25-127">Stegen i det här dokumentet använder Data Lake-verktyg för Visual Studio för att överföra filer till primära lagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="cbb25-127">The steps in this document use the Data Lake Tools for Visual Studio to upload the files to primary storage for the cluster.</span></span>

* <span data-ttu-id="cbb25-128">Azure PowerShell eller en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="cbb25-128">Azure PowerShell or a SSH client.</span></span>

* <span data-ttu-id="cbb25-129">En Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="cbb25-129">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="cbb25-130">Mer information om hur du skapar ett kluster finns [skapar ett HDInsight-kluster](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="cbb25-130">For more information on creating a cluster, see [Create an HDInsight cluster](hdinsight-provision-clusters.md).</span></span>

## <a name="create-the-mapper"></a><span data-ttu-id="cbb25-131">Skapa mapparen</span><span class="sxs-lookup"><span data-stu-id="cbb25-131">Create the mapper</span></span>

<span data-ttu-id="cbb25-132">I Visual Studio skapar en ny __Konsolapp__ med namnet __mapper__.</span><span class="sxs-lookup"><span data-stu-id="cbb25-132">In Visual Studio, create a new __Console application__ named __mapper__.</span></span> <span data-ttu-id="cbb25-133">Använd följande kod för programmet:</span><span class="sxs-lookup"><span data-stu-id="cbb25-133">Use the following code for the application:</span></span>

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
            //Hadoop passes data to the mapper on STDIN
            while((line = Console.ReadLine()) != null)
            {
                // We only want words, so strip out punctuation, numbers, etc.
                var onlyText = Regex.Replace(line, @"\.|;|:|,|[0-9]|'", "");
                // Split at whitespace.
                var words = Regex.Matches(onlyText, @"[\w]+");
                // Loop over the words
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

<span data-ttu-id="cbb25-134">När du har skapat programmet, skapar du det att skapa den `/bin/Debug/mapper.exe` filen i projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="cbb25-134">After creating the application, build it to produce the `/bin/Debug/mapper.exe` file in the project directory.</span></span>

## <a name="create-the-reducer"></a><span data-ttu-id="cbb25-135">Skapa reducer</span><span class="sxs-lookup"><span data-stu-id="cbb25-135">Create the reducer</span></span>

<span data-ttu-id="cbb25-136">I Visual Studio skapar en ny __Konsolapp__ med namnet __reducer__.</span><span class="sxs-lookup"><span data-stu-id="cbb25-136">In Visual Studio, create a new __Console application__ named __reducer__.</span></span> <span data-ttu-id="cbb25-137">Använd följande kod för programmet:</span><span class="sxs-lookup"><span data-stu-id="cbb25-137">Use the following code for the application:</span></span>

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
                // Get the word
                string word = sArr[0];
                // Get the count
                int count = Convert.ToInt32(sArr[1]);

                //Do we already have a count for the word?
                if(words.ContainsKey(word))
                {
                    //If so, increment the count
                    words[word] += count;
                } else
                {
                    //Add the key to the collection
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

<span data-ttu-id="cbb25-138">När du har skapat programmet, skapar du det att skapa den `/bin/Debug/reducer.exe` filen i projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="cbb25-138">After creating the application, build it to produce the `/bin/Debug/reducer.exe` file in the project directory.</span></span>

## <a name="upload-to-storage"></a><span data-ttu-id="cbb25-139">Överföra till lagring</span><span class="sxs-lookup"><span data-stu-id="cbb25-139">Upload to storage</span></span>

1. <span data-ttu-id="cbb25-140">Öppna i Visual Studio **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="cbb25-140">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="cbb25-141">Expandera **Azure** och expandera därefter **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="cbb25-141">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="cbb25-142">Om du uppmanas ange dina autentiseringsuppgifter för Azure-prenumeration och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="cbb25-142">If prompted, enter your Azure subscription credentials, and then click **Sign In**.</span></span>

4. <span data-ttu-id="cbb25-143">Expandera HDInsight-klustret som du vill distribuera programmet till.</span><span class="sxs-lookup"><span data-stu-id="cbb25-143">Expand the HDInsight cluster that you wish to deploy this application to.</span></span> <span data-ttu-id="cbb25-144">En post med texten __(standard Storage-konto)__ visas.</span><span class="sxs-lookup"><span data-stu-id="cbb25-144">An entry with the text __(Default Storage Account)__ is listed.</span></span>

    ![Server Explorer visar lagringskontot för klustret](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * <span data-ttu-id="cbb25-146">Om den här posten kan utökas måste du använder en __Azure Storage-konto__ som standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="cbb25-146">If this entry can be expanded, you are using an __Azure Storage Account__ as default storage for the cluster.</span></span> <span data-ttu-id="cbb25-147">Om du vill visa filerna på lagringsutrymmet som standard för klustret, expandera posten och dubbelklicka sedan på den __(standardbehållaren)__.</span><span class="sxs-lookup"><span data-stu-id="cbb25-147">To view the files on the default storage for the cluster, expand the entry and then double-click the __(Default Container)__.</span></span>

    * <span data-ttu-id="cbb25-148">Om den här posten inte kan expanderas, använder du __Azure Data Lake Store__ som standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="cbb25-148">If this entry cannot be expanded, you are using __Azure Data Lake Store__ as the default storage for the cluster.</span></span> <span data-ttu-id="cbb25-149">Om du vill visa filerna på lagringsutrymmet som standard för klustret, dubbelklickar du på den __(standardkontot för lagring)__ post.</span><span class="sxs-lookup"><span data-stu-id="cbb25-149">To view the files on the default storage for the cluster, double-click the __(Default Storage Account)__ entry.</span></span>

5. <span data-ttu-id="cbb25-150">Använd någon av följande metoder för att ladda upp .exe-filer:</span><span class="sxs-lookup"><span data-stu-id="cbb25-150">To upload the .exe files, use one of the following methods:</span></span>

    * <span data-ttu-id="cbb25-151">Om du använder en __Azure Storage-konto__, klicka på ikonen överföringen och bläddra sedan till den **bin\debug** mapp för den **mapper** projekt.</span><span class="sxs-lookup"><span data-stu-id="cbb25-151">If using an __Azure Storage Account__, click the upload icon, and then browse to the **bin\debug** folder for the **mapper** project.</span></span> <span data-ttu-id="cbb25-152">Välj slutligen den **mapper.exe** fil och klicka på **Ok**.</span><span class="sxs-lookup"><span data-stu-id="cbb25-152">Finally, select the **mapper.exe** file and click **Ok**.</span></span>

        ![Överför ikon](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * <span data-ttu-id="cbb25-154">Om du använder __Azure Data Lake Store__, högerklicka på ett tomt område i listan över filer och välj sedan __överför__.</span><span class="sxs-lookup"><span data-stu-id="cbb25-154">If using __Azure Data Lake Store__, right-click an empty area in the file listing, and then select __Upload__.</span></span> <span data-ttu-id="cbb25-155">Välj slutligen den **mapper.exe** fil och klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="cbb25-155">Finally, select the **mapper.exe** file and click **Open**.</span></span>

    <span data-ttu-id="cbb25-156">En gång i __mapper.exe__ överföringen har slutförts, Upprepa processen för att ladda upp den __reducer.exe__ fil.</span><span class="sxs-lookup"><span data-stu-id="cbb25-156">Once the __mapper.exe__ upload has finished, repeat the upload process for the __reducer.exe__ file.</span></span>

## <a name="run-a-job-using-an-ssh-session"></a><span data-ttu-id="cbb25-157">Kör ett jobb: använder en SSH-session</span><span class="sxs-lookup"><span data-stu-id="cbb25-157">Run a job: Using an SSH session</span></span>

1. <span data-ttu-id="cbb25-158">Använda SSH för att ansluta till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="cbb25-158">Use SSH to connect to the HDInsight cluster.</span></span> <span data-ttu-id="cbb25-159">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="cbb25-159">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="cbb25-160">Använd en av följande kommando för att starta MapReduce-jobb:</span><span class="sxs-lookup"><span data-stu-id="cbb25-160">Use one of the following command to start the MapReduce job:</span></span>

    * <span data-ttu-id="cbb25-161">Om du använder __Datasjölager__ som standardlagring:</span><span class="sxs-lookup"><span data-stu-id="cbb25-161">If using __Data Lake Store__ as default storage:</span></span>

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * <span data-ttu-id="cbb25-162">Om du använder __Azure Storage__ som standardlagring:</span><span class="sxs-lookup"><span data-stu-id="cbb25-162">If using __Azure Storage__ as default storage:</span></span>

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    <span data-ttu-id="cbb25-163">I följande lista beskrivs vad varje parameter är:</span><span class="sxs-lookup"><span data-stu-id="cbb25-163">The following list describes what each parameter does:</span></span>

    * <span data-ttu-id="cbb25-164">`hadoop-streaming.jar`: Jar-fil som innehåller funktioner för direktuppspelning MapReduce.</span><span class="sxs-lookup"><span data-stu-id="cbb25-164">`hadoop-streaming.jar`: The jar file that contains the streaming MapReduce functionality.</span></span>
    * <span data-ttu-id="cbb25-165">`-files`: Lägger till den `mapper.exe` och `reducer.exe` filer till det här jobbet.</span><span class="sxs-lookup"><span data-stu-id="cbb25-165">`-files`: Adds the `mapper.exe` and `reducer.exe` files to this job.</span></span> <span data-ttu-id="cbb25-166">Den `adl:///` eller `wasb:///` innan varje fil är sökvägen till roten i standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="cbb25-166">The `adl:///` or `wasb:///` before each file is the path to the root of default storage for the cluster.</span></span>
    * <span data-ttu-id="cbb25-167">`-mapper`: Anger vilken fil som implementerar mapparen.</span><span class="sxs-lookup"><span data-stu-id="cbb25-167">`-mapper`: Specifies which file implements the mapper.</span></span>
    * <span data-ttu-id="cbb25-168">`-reducer`: Anger vilken fil som implementerar reducer.</span><span class="sxs-lookup"><span data-stu-id="cbb25-168">`-reducer`: Specifies which file implements the reducer.</span></span>
    * <span data-ttu-id="cbb25-169">`-input`: Indata.</span><span class="sxs-lookup"><span data-stu-id="cbb25-169">`-input`: The input data.</span></span>
    * <span data-ttu-id="cbb25-170">`-output`: Den angivna katalogen.</span><span class="sxs-lookup"><span data-stu-id="cbb25-170">`-output`: The output directory.</span></span>

3. <span data-ttu-id="cbb25-171">När MapReduce-jobbet har slutförts kan du använda följande för att visa resultatet:</span><span class="sxs-lookup"><span data-stu-id="cbb25-171">Once the MapReduce job completes, use the following to view the results:</span></span>

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    <span data-ttu-id="cbb25-172">Följande är ett exempel på de data som returneras av kommandot:</span><span class="sxs-lookup"><span data-stu-id="cbb25-172">The following text is an example of the data returned by this command:</span></span>

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a><span data-ttu-id="cbb25-173">Kör ett jobb: med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="cbb25-173">Run a job: Using PowerShell</span></span>

<span data-ttu-id="cbb25-174">Använd följande PowerShell-skript för att köra ett MapReduce-jobb och hämta resultaten.</span><span class="sxs-lookup"><span data-stu-id="cbb25-174">Use the following PowerShell script to run a MapReduce job and download the results.</span></span>

<span data-ttu-id="cbb25-175">[!code-powershell[huvudsakliga](../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]</span><span class="sxs-lookup"><span data-stu-id="cbb25-175">[!code-powershell[main](../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]</span></span>

<span data-ttu-id="cbb25-176">Det här skriptet uppmanas du att klustret konto inloggningsnamn och lösenord, tillsammans med HDInsight-klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="cbb25-176">This script prompts you for the cluster login account name and password, along with the HDInsight cluster name.</span></span> <span data-ttu-id="cbb25-177">När jobbet är slutfört utdata hämtas till den `output.txt` filen i katalogen skriptet kördes från.</span><span class="sxs-lookup"><span data-stu-id="cbb25-177">Once the job completes, the output is downloaded to the `output.txt` file in the directory the script is ran from.</span></span> <span data-ttu-id="cbb25-178">Följande är ett exempel på data i den `output.txt` filen:</span><span class="sxs-lookup"><span data-stu-id="cbb25-178">The following text is an example of the data in the `output.txt` file:</span></span>

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a><span data-ttu-id="cbb25-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cbb25-179">Next steps</span></span>

<span data-ttu-id="cbb25-180">Mer information om hur du använder MapReduce med HDInsight finns [använda MapReduce med HDInsight](hdinsight-use-mapreduce.md).</span><span class="sxs-lookup"><span data-stu-id="cbb25-180">For more information on using MapReduce with HDInsight, see [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md).</span></span>

<span data-ttu-id="cbb25-181">Information om hur du använder C# med Hive och Pig finns [använder en C# användardefinierad funktion med Hive och Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="cbb25-181">For information on using C# with Hive and Pig, see [Use a C# user defined function with Hive and Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).</span></span>

<span data-ttu-id="cbb25-182">Information om hur du använder C# med Storm på HDInsight finns i [utveckla C#-topologier för Storm på HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="cbb25-182">For information on using C# with Storm on HDInsight, see [Develop C# topologies for Storm on HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>