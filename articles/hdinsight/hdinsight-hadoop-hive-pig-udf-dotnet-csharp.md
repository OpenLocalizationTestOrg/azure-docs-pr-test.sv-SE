---
title: aaaUse C# med Hive och Pig med Hadoop i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toouse C#-användardefinierade funktioner (UDF) med Hive och Pig strömning i Azure HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d83def76-12ad-4538-bb8e-3ba3542b7211
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: dd35409766f2dafe4d8050c3f9bc351949473ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-c-user-defined-functions-with-hive-and-pig-streaming-on-hadoop-in-hdinsight"></a><span data-ttu-id="ee93b-103">Använda C# användardefinierade funktioner med Hive och Pig strömning på Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee93b-103">Use C# user-defined functions with Hive and Pig streaming on Hadoop in HDInsight</span></span>

<span data-ttu-id="ee93b-104">Lär dig hur toouse C# användardefinierade funktioner (UDF) med Apache Hive och Pig på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ee93b-104">Learn how toouse C# user defined functions (UDF) with Apache Hive and Pig on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ee93b-105">hello stegen i det här dokumentet fungerar med både Linux- och Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee93b-105">hello steps in this document work with both Linux-based and Windows-based HDInsight clusters.</span></span> <span data-ttu-id="ee93b-106">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ee93b-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ee93b-107">Mer information finns i [HDInsight component-versioning](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="ee93b-107">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="ee93b-108">Både Hive och Pig kan skicka data tooexternal program för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="ee93b-108">Both Hive and Pig can pass data tooexternal applications for processing.</span></span> <span data-ttu-id="ee93b-109">Den här processen kallas _strömning_.</span><span class="sxs-lookup"><span data-stu-id="ee93b-109">This process is known as _streaming_.</span></span> <span data-ttu-id="ee93b-110">När du använder en .NET-kretsmönstret toohello programmet hello data skickas vidare STDIN och returnerar hello programmet hello resultat på STDOUT.</span><span class="sxs-lookup"><span data-stu-id="ee93b-110">When using a .NET applciation, hello data is passed toohello application on STDIN, and hello application returns hello results on STDOUT.</span></span> <span data-ttu-id="ee93b-111">tooread och Skriv från STDIN och STDOUT, kan du använda `Console.ReadLine()` och `Console.WriteLine()` från ett konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="ee93b-111">tooread and write from STDIN and STDOUT, you can use `Console.ReadLine()` and `Console.WriteLine()` from a console application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee93b-112">Krav</span><span class="sxs-lookup"><span data-stu-id="ee93b-112">Prerequisites</span></span>

* <span data-ttu-id="ee93b-113">Tidigare erfarenhet av skrivning och bygga C#-kod som riktar sig till .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="ee93b-113">A familiarity with writing and building C# code that targets .NET Framework 4.5.</span></span>

    * <span data-ttu-id="ee93b-114">Använd oavsett IDE som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="ee93b-114">Use whatever IDE you want.</span></span> <span data-ttu-id="ee93b-115">Vi rekommenderar [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, eller [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="ee93b-115">We recommend [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, or [Visual Studio Code](https://code.visualstudio.com/).</span></span> <span data-ttu-id="ee93b-116">hello stegen i det här dokumentet används Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="ee93b-116">hello steps in this document use Visual Studio 2017.</span></span>

* <span data-ttu-id="ee93b-117">Ett sätt tooupload .exe-filer toohello klustret och köra Pig och Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="ee93b-117">A way tooupload .exe files toohello cluster and run Pig and Hive jobs.</span></span> <span data-ttu-id="ee93b-118">Vi rekommenderar hello Data Lake-verktyg för Visual Studio, Azure PowerShell och Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ee93b-118">We recommend hello Data Lake Tools for Visual Studio, Azure PowerShell and Azure CLI.</span></span> <span data-ttu-id="ee93b-119">hello stegen i det här dokumentet använder hello Data Lake-verktyg för Visual Studio tooupload hello filer och kör hello exempel Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="ee93b-119">hello steps in this document use hello Data Lake Tools for Visual Studio tooupload hello files and run hello example Hive query.</span></span>

    <span data-ttu-id="ee93b-120">Information om andra sätt toorun Hive-frågor och Pig-jobb finns hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="ee93b-120">For information on other ways toorun Hive queries and Pig jobs, see hello following documents:</span></span>

    * [<span data-ttu-id="ee93b-121">Använda Apache Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee93b-121">Use Apache Hive with HDInsight</span></span>](hdinsight-use-hive.md)

    * [<span data-ttu-id="ee93b-122">Använda Apache Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee93b-122">Use Apache Pig with HDInsight</span></span>](hdinsight-use-pig.md)

* <span data-ttu-id="ee93b-123">En Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee93b-123">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="ee93b-124">Mer information om hur du skapar ett kluster finns [skapar ett HDInsight-kluster](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="ee93b-124">For more information on creating a cluster, see [Create an HDInsight cluster](hdinsight-provision-clusters.md).</span></span>

## <a name="net-on-hdinsight"></a><span data-ttu-id="ee93b-125">.NET på HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee93b-125">.NET on HDInsight</span></span>

* <span data-ttu-id="ee93b-126">__Linux-baserat HDInsight__ kluster med [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET-program.</span><span class="sxs-lookup"><span data-stu-id="ee93b-126">__Linux-based HDInsight__ clusters using [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET applications.</span></span> <span data-ttu-id="ee93b-127">Monoljud version 4.2.1 ingår i HDInsight version 3.5.</span><span class="sxs-lookup"><span data-stu-id="ee93b-127">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span>

    <span data-ttu-id="ee93b-128">Mer information om monoljud kompatibilitet med .NET Framework-versioner finns [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="ee93b-128">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

    <span data-ttu-id="ee93b-129">toouse en viss version av Mono, se hello [installera eller uppdatera Mono](hdinsight-hadoop-install-mono.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="ee93b-129">toouse a specific version of Mono, see hello [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

* <span data-ttu-id="ee93b-130">__Windows-baserade HDInsight__ kluster använder hello Microsoft .NET CLR toorun .NET-program.</span><span class="sxs-lookup"><span data-stu-id="ee93b-130">__Windows-based HDInsight__ clusters use hello Microsoft .NET CLR toorun .NET applications.</span></span>

<span data-ttu-id="ee93b-131">Läs mer på hello hello .NET framework och Mono som ingår i HDInsight-versioner, [HDInsight komponenten versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="ee93b-131">For more information on hello version of hello .NET framework and Mono included with HDInsight versions, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span>

## <a name="create-hello-c-projects"></a><span data-ttu-id="ee93b-132">Skapa hello C\# projekt</span><span class="sxs-lookup"><span data-stu-id="ee93b-132">Create hello C\# projects</span></span>

### <a name="hive-udf"></a><span data-ttu-id="ee93b-133">Hive UDF</span><span class="sxs-lookup"><span data-stu-id="ee93b-133">Hive UDF</span></span>

1. <span data-ttu-id="ee93b-134">Öppna Visual Studio och skapa en lösning.</span><span class="sxs-lookup"><span data-stu-id="ee93b-134">Open Visual Studio and create a solution.</span></span> <span data-ttu-id="ee93b-135">Hello projekttypen Välj **Konsolapp (.NET Framework)**, och namnet hello nytt projekt **HiveCSharp**.</span><span class="sxs-lookup"><span data-stu-id="ee93b-135">For hello project type, select **Console App (.NET Framework)**, and name hello new project **HiveCSharp**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ee93b-136">Välj __.NET Framework 4.5__ om du använder en Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee93b-136">Select __.NET Framework 4.5__ if you are using a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="ee93b-137">Mer information om monoljud kompatibilitet med .NET Framework-versioner finns [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="ee93b-137">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

2. <span data-ttu-id="ee93b-138">Ersätt hello innehållet i **Program.cs** med hello följande:</span><span class="sxs-lookup"><span data-stu-id="ee93b-138">Replace hello contents of **Program.cs** with hello following:</span></span>

    ```csharp
    using System;
    using System.Security.Cryptography;
    using System.Text;
    using System.Threading.Tasks;

    namespace HiveCSharp
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Parse hello string, trimming line feeds
                    // and splitting fields at tabs
                    line = line.TrimEnd('\n');
                    string[] field = line.Split('\t');
                    string phoneLabel = field[1] + ' ' + field[2];
                    // Emit new data toostdout, delimited by tabs
                    Console.WriteLine("{0}\t{1}\t{2}", field[0], phoneLabel, GetMD5Hash(phoneLabel));
                }
            }
            /// <summary>
            /// Returns an MD5 hash for hello given string
            /// </summary>
            /// <param name="input">string value</param>
            /// <returns>an MD5 hash</returns>
            static string GetMD5Hash(string input)
            {
                // Step 1, calculate MD5 hash from input
                MD5 md5 = System.Security.Cryptography.MD5.Create();
                byte[] inputBytes = System.Text.Encoding.ASCII.GetBytes(input);
                byte[] hash = md5.ComputeHash(inputBytes);

                // Step 2, convert byte array toohex string
                StringBuilder sb = new StringBuilder();
                for (int i = 0; i < hash.Length; i++)
                {
                    sb.Append(hash[i].ToString("x2"));
                }
                return sb.ToString();
            }
        }
    }
    ```

3. <span data-ttu-id="ee93b-139">Skapa hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="ee93b-139">Build hello project.</span></span>

### <a name="pig-udf"></a><span data-ttu-id="ee93b-140">Pig UDF</span><span class="sxs-lookup"><span data-stu-id="ee93b-140">Pig UDF</span></span>

1. <span data-ttu-id="ee93b-141">Öppna Visual Studio och skapa en lösning.</span><span class="sxs-lookup"><span data-stu-id="ee93b-141">Open Visual Studio and create a solution.</span></span> <span data-ttu-id="ee93b-142">Hello projekttypen Välj **konsolprogram**, och namnet hello nytt projekt **PigUDF**.</span><span class="sxs-lookup"><span data-stu-id="ee93b-142">For hello project type, select **Console Application**, and name hello new project **PigUDF**.</span></span>

2. <span data-ttu-id="ee93b-143">Ersätt hello innehållet i hello **Program.cs** fil med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="ee93b-143">Replace hello contents of hello **Program.cs** file with hello following code:</span></span>

    ```csharp
    using System;

    namespace PigUDF
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Fix formatting on lines that begin with an exception
                    if(line.StartsWith("java.lang.Exception"))
                    {
                        // Trim hello error info off hello beginning and add a note toohello end of hello line
                        line = line.Remove(0, 21) + " - java.lang.Exception";
                    }
                    // Split hello fields apart at tab characters
                    string[] field = line.Split('\t');
                    // Put fields back together for writing
                    Console.WriteLine(String.Join("\t",field));
                }
            }
        }
    }
    ```

    <span data-ttu-id="ee93b-144">Det här programmet Parsar hello rader skickas från Pig och formatera om rader som börjar med `java.lang.Exception`.</span><span class="sxs-lookup"><span data-stu-id="ee93b-144">This application parses hello lines sent from Pig, and reformat lines that begin with `java.lang.Exception`.</span></span>

3. <span data-ttu-id="ee93b-145">Spara **Program.cs**, och sedan skapa hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="ee93b-145">Save **Program.cs**, and then build hello project.</span></span>

## <a name="upload-toostorage"></a><span data-ttu-id="ee93b-146">Överför toostorage</span><span class="sxs-lookup"><span data-stu-id="ee93b-146">Upload toostorage</span></span>

1. <span data-ttu-id="ee93b-147">Öppna i Visual Studio **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ee93b-147">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="ee93b-148">Expandera **Azure** och expandera därefter **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="ee93b-148">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="ee93b-149">Om du uppmanas ange dina autentiseringsuppgifter för Azure-prenumeration och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="ee93b-149">If prompted, enter your Azure subscription credentials, and then click **Sign In**.</span></span>

4. <span data-ttu-id="ee93b-150">Expandera hello HDInsight-kluster som du vill toodeploy programmet.</span><span class="sxs-lookup"><span data-stu-id="ee93b-150">Expand hello HDInsight cluster that you wish toodeploy this application to.</span></span> <span data-ttu-id="ee93b-151">En post med hello text __(standard Storage-konto)__ visas.</span><span class="sxs-lookup"><span data-stu-id="ee93b-151">An entry with hello text __(Default Storage Account)__ is listed.</span></span>

    ![Server Explorer som visar hello storage-konto för hello-kluster](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * <span data-ttu-id="ee93b-153">Om den här posten kan utökas måste du använder en __Azure Storage-konto__ som standard lagringen för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="ee93b-153">If this entry can be expanded, you are using an __Azure Storage Account__ as default storage for hello cluster.</span></span> <span data-ttu-id="ee93b-154">tooview hello filer på hello standardlagring för hello-kluster expanderar hello-post och dubbelklicka sedan på hello __(standardbehållaren)__.</span><span class="sxs-lookup"><span data-stu-id="ee93b-154">tooview hello files on hello default storage for hello cluster, expand hello entry and then double-click hello __(Default Container)__.</span></span>

    * <span data-ttu-id="ee93b-155">Om den här posten inte kan expanderas, använder du __Azure Data Lake Store__ som hello standardlagring för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee93b-155">If this entry cannot be expanded, you are using __Azure Data Lake Store__ as hello default storage for hello cluster.</span></span> <span data-ttu-id="ee93b-156">tooview hello filer på hello standardlagring för hello kluster, dubbelklicka på hello __(standardkontot för lagring)__ post.</span><span class="sxs-lookup"><span data-stu-id="ee93b-156">tooview hello files on hello default storage for hello cluster, double-click hello __(Default Storage Account)__ entry.</span></span>

6. <span data-ttu-id="ee93b-157">tooupload hello .exe-filer med någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="ee93b-157">tooupload hello .exe files, use one of hello following methods:</span></span>

    * <span data-ttu-id="ee93b-158">Om du använder en __Azure Storage-konto__, klicka på ikonen för hello överföringen och bläddra sedan toohello **bin\debug** mapp för hello **HiveCSharp** projekt.</span><span class="sxs-lookup"><span data-stu-id="ee93b-158">If using an __Azure Storage Account__, click hello upload icon, and then browse toohello **bin\debug** folder for hello **HiveCSharp** project.</span></span> <span data-ttu-id="ee93b-159">Välj slutligen hello **HiveCSharp.exe** fil och klicka på **Ok**.</span><span class="sxs-lookup"><span data-stu-id="ee93b-159">Finally, select hello **HiveCSharp.exe** file and click **Ok**.</span></span>

        ![Överför ikon](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * <span data-ttu-id="ee93b-161">Om du använder __Azure Data Lake Store__, högerklicka på ett tomt område i hello med och välj sedan __överför__.</span><span class="sxs-lookup"><span data-stu-id="ee93b-161">If using __Azure Data Lake Store__, right-click an empty area in hello file listing, and then select __Upload__.</span></span> <span data-ttu-id="ee93b-162">Välj slutligen hello **HiveCSharp.exe** fil och klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="ee93b-162">Finally, select hello **HiveCSharp.exe** file and click **Open**.</span></span>

    <span data-ttu-id="ee93b-163">En gång hello __HiveCSharp.exe__ överföringen har slutförts, upprepa hello överföringsprocessen för hello __PigUDF.exe__ fil.</span><span class="sxs-lookup"><span data-stu-id="ee93b-163">Once hello __HiveCSharp.exe__ upload has finished, repeat hello upload process for hello __PigUDF.exe__ file.</span></span>

## <a name="run-a-hive-query"></a><span data-ttu-id="ee93b-164">Köra en Hive-fråga</span><span class="sxs-lookup"><span data-stu-id="ee93b-164">Run a Hive query</span></span>

1. <span data-ttu-id="ee93b-165">Öppna i Visual Studio **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ee93b-165">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="ee93b-166">Expandera **Azure** och expandera därefter **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="ee93b-166">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="ee93b-167">Högerklicka på hello klustret som du distribuerat hello **HiveCSharp** program till och markera sedan **skriva en Hive-fråga**.</span><span class="sxs-lookup"><span data-stu-id="ee93b-167">Right-click hello cluster that you deployed hello **HiveCSharp** application to, and then select **Write a Hive Query**.</span></span>

4. <span data-ttu-id="ee93b-168">Använd hello följande text för hello Hive-fråga:</span><span class="sxs-lookup"><span data-stu-id="ee93b-168">Use hello following text for hello Hive query:</span></span>

    ```hiveql
    -- Uncomment hello following if you are using Azure Storage
    -- add file wasb:///HiveCSharp.exe;
    -- Uncomment hello following if you are using Azure Data Lake Store
    -- add file adl:///HiveCSharp.exe;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'HiveCSharp.exe' AS
    (clientid string, phoneLabel string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="ee93b-169">Ta bort kommentarerna hello `add file` uttryck som matchar hello typ av standard lagringsutrymme som används för klustret.</span><span class="sxs-lookup"><span data-stu-id="ee93b-169">Uncomment hello `add file` statement that matches hello type of default storage used for your cluster.</span></span>

    <span data-ttu-id="ee93b-170">Den här frågan väljer hello `clientid`, `devicemake`, och `devicemodel` fält från `hivesampletable`, och skickar hello fält toohello HiveCSharp.exe program.</span><span class="sxs-lookup"><span data-stu-id="ee93b-170">This query selects hello `clientid`, `devicemake`, and `devicemodel` fields from `hivesampletable`, and passes hello fields toohello HiveCSharp.exe application.</span></span> <span data-ttu-id="ee93b-171">hello fråga förväntas hello programmet tooreturn tre fält, som lagras som `clientid`, `phoneLabel`, och `phoneHash`.</span><span class="sxs-lookup"><span data-stu-id="ee93b-171">hello query expects hello application tooreturn three fields, which are stored as `clientid`, `phoneLabel`, and `phoneHash`.</span></span> <span data-ttu-id="ee93b-172">hello fråga förväntas också toofind HiveCSharp.exe i hello rot hello standardbehållare för lagring.</span><span class="sxs-lookup"><span data-stu-id="ee93b-172">hello query also expects toofind HiveCSharp.exe in hello root of hello default storage container.</span></span>

5. <span data-ttu-id="ee93b-173">Klicka på **skicka** toosubmit hello jobbet toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee93b-173">Click **Submit** toosubmit hello job toohello HDInsight cluster.</span></span> <span data-ttu-id="ee93b-174">Hej **Hive-jobbsammanfattning** öppnas.</span><span class="sxs-lookup"><span data-stu-id="ee93b-174">hello **Hive Job Summary** window opens.</span></span>

6. <span data-ttu-id="ee93b-175">Klicka på **uppdatera** toorefresh hello sammanfattning tills **jobbstatus** ändras för**slutförd**.</span><span class="sxs-lookup"><span data-stu-id="ee93b-175">Click **Refresh** toorefresh hello summary until **Job Status** changes too**Completed**.</span></span> <span data-ttu-id="ee93b-176">tooview hello-jobbets utdatafil, klickar du på **Jobbutdata**.</span><span class="sxs-lookup"><span data-stu-id="ee93b-176">tooview hello job output, click **Job Output**.</span></span>

## <a name="run-a-pig-job"></a><span data-ttu-id="ee93b-177">Kör ett Pig-jobb</span><span class="sxs-lookup"><span data-stu-id="ee93b-177">Run a Pig job</span></span>

1. <span data-ttu-id="ee93b-178">Använd någon av följande metoder tooconnect tooyour HDInsight-kluster hello:</span><span class="sxs-lookup"><span data-stu-id="ee93b-178">Use one of hello following methods tooconnect tooyour HDInsight cluster:</span></span>

    * <span data-ttu-id="ee93b-179">Om du använder en __Linux-baserade__ HDInsight-kluster använder SSH.</span><span class="sxs-lookup"><span data-stu-id="ee93b-179">If you are using a __Linux-based__ HDInsight cluster, use SSH.</span></span> <span data-ttu-id="ee93b-180">Till exempel `ssh sshuser@mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="ee93b-180">For example, `ssh sshuser@mycluster-ssh.azurehdinsight.net`.</span></span> <span data-ttu-id="ee93b-181">Mer information finns i [använda SSH withHDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="ee93b-181">For more information, see [Use SSH withHDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>
    
    * <span data-ttu-id="ee93b-182">Om du använder en __Windows-baserade__ HDInsight-kluster [Anslut toohello kluster med hjälp av fjärrskrivbord](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span><span class="sxs-lookup"><span data-stu-id="ee93b-182">If you are using a __Windows-based__ HDInsight cluster, [Connect toohello cluster using Remote Desktop](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span></span>

2. <span data-ttu-id="ee93b-183">Använd en hello efter kommandot toostart hello Pig kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="ee93b-183">Use one hello following command toostart hello Pig command line:</span></span>

        pig

    > [!IMPORTANT]
    > <span data-ttu-id="ee93b-184">Om du använder ett Windows-baserade kluster använder du följande kommandon i stället hello:</span><span class="sxs-lookup"><span data-stu-id="ee93b-184">If you are using a Windows-based cluster, use hello following commands instead:</span></span>
    > ```
    > cd %PIG_HOME%
    > bin\pig
    > ```

    <span data-ttu-id="ee93b-185">En `grunt>` visas frågan.</span><span class="sxs-lookup"><span data-stu-id="ee93b-185">A `grunt>` prompt is displayed.</span></span>

3. <span data-ttu-id="ee93b-186">Ange hello följande toorun ett Pig-jobb som använder hello .NET Framework-program:</span><span class="sxs-lookup"><span data-stu-id="ee93b-186">Enter hello following toorun a Pig job that uses hello .NET Framework application:</span></span>

        DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
        LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    <span data-ttu-id="ee93b-187">Hej `DEFINE` uttrycket skapar ett alias för `streamer` för hello pigudf.exe program och `CACHE` läser från standard lagringen för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="ee93b-187">hello `DEFINE` statement creates an alias of `streamer` for hello pigudf.exe applications, and `CACHE` loads it from default storage for hello cluster.</span></span> <span data-ttu-id="ee93b-188">Senare, `streamer` används med hello `STREAM` operatorn tooprocess hello rader som ingår i LOGGEN och returnera hello som en serie kolumner.</span><span class="sxs-lookup"><span data-stu-id="ee93b-188">Later, `streamer` is used with hello `STREAM` operator tooprocess hello single lines contained in LOG and return hello data as a series of columns.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ee93b-189">hello programnamn som används för strömning måste omges av hello \` (backtick) tecken när ett alias, och ' (enkelt citattecken) när det används med `SHIP\`.</span><span class="sxs-lookup"><span data-stu-id="ee93b-189">hello application name that is used for streaming must be surrounded by hello \` (backtick) character when aliased, and ' (single quote) when used with `SHIP\`.</span></span>

4. <span data-ttu-id="ee93b-190">När du har angett hello sista raden ska hello jobbet starta.</span><span class="sxs-lookup"><span data-stu-id="ee93b-190">After entering hello last line, hello job should start.</span></span> <span data-ttu-id="ee93b-191">Den returnerar utdata liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="ee93b-191">It returns output similar toohello following text:</span></span>

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="next-steps"></a><span data-ttu-id="ee93b-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ee93b-192">Next steps</span></span>

<span data-ttu-id="ee93b-193">I det här dokumentet har du lärt dig hur toouse .NET Framework-program från Hive och Pig på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ee93b-193">In this document, you have learned how toouse a .NET Framework application from Hive and Pig on HDInsight.</span></span> <span data-ttu-id="ee93b-194">Om du vill att toolearn hur toouse Python med Hive och Pig, se [används Python med Hive och Pig i HDInsight](hdinsight-python.md).</span><span class="sxs-lookup"><span data-stu-id="ee93b-194">If you would like toolearn how toouse Python with Hive and Pig, see [Use Python with Hive and Pig in HDInsight](hdinsight-python.md).</span></span>

<span data-ttu-id="ee93b-195">Andra sätt toouse Pig och Hive och toolearn om hur du använder MapReduce finns hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="ee93b-195">For other ways toouse Pig and Hive, and toolearn about using MapReduce, see hello following documents:</span></span>

* [<span data-ttu-id="ee93b-196">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee93b-196">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="ee93b-197">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee93b-197">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="ee93b-198">Använda MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee93b-198">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)
