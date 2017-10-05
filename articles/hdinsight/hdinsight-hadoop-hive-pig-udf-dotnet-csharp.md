---
title: "Använda C# med Hive och Pig med Hadoop i HDInsight - Azure | Microsoft Docs"
description: "Lär dig använda C# användardefinierade funktioner (UDF) med Hive och Pig strömning i Azure HDInsight."
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
ms.openlocfilehash: 58e7af47be71c3e0389e5fb4641e124eb648494e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-c-user-defined-functions-with-hive-and-pig-streaming-on-hadoop-in-hdinsight"></a><span data-ttu-id="ede45-103">Använda C# användardefinierade funktioner med Hive och Pig strömning på Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ede45-103">Use C# user-defined functions with Hive and Pig streaming on Hadoop in HDInsight</span></span>

<span data-ttu-id="ede45-104">Lär dig använda C# användardefinierade funktioner (UDF) med Apache Hive och Pig på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ede45-104">Learn how to use C# user defined functions (UDF) with Apache Hive and Pig on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ede45-105">Stegen i det här dokumentet fungerar med både Linux- och Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ede45-105">The steps in this document work with both Linux-based and Windows-based HDInsight clusters.</span></span> <span data-ttu-id="ede45-106">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="ede45-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ede45-107">Mer information finns i [HDInsight component-versioning](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="ede45-107">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="ede45-108">Både Hive och Pig kan överföra data till externa program för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="ede45-108">Both Hive and Pig can pass data to external applications for processing.</span></span> <span data-ttu-id="ede45-109">Den här processen kallas _strömning_.</span><span class="sxs-lookup"><span data-stu-id="ede45-109">This process is known as _streaming_.</span></span> <span data-ttu-id="ede45-110">När du använder en .NET-kretsmönstret data skickas till programmet på STDIN och programmet returnerar resultat i STDOUT.</span><span class="sxs-lookup"><span data-stu-id="ede45-110">When using a .NET applciation, the data is passed to the application on STDIN, and the application returns the results on STDOUT.</span></span> <span data-ttu-id="ede45-111">Du kan använda för att läsa och skriva från STDIN och STDOUT `Console.ReadLine()` och `Console.WriteLine()` från ett konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="ede45-111">To read and write from STDIN and STDOUT, you can use `Console.ReadLine()` and `Console.WriteLine()` from a console application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ede45-112">Krav</span><span class="sxs-lookup"><span data-stu-id="ede45-112">Prerequisites</span></span>

* <span data-ttu-id="ede45-113">Tidigare erfarenhet av skrivning och bygga C#-kod som riktar sig till .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="ede45-113">A familiarity with writing and building C# code that targets .NET Framework 4.5.</span></span>

    * <span data-ttu-id="ede45-114">Använd oavsett IDE som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="ede45-114">Use whatever IDE you want.</span></span> <span data-ttu-id="ede45-115">Vi rekommenderar [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, eller [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="ede45-115">We recommend [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, or [Visual Studio Code](https://code.visualstudio.com/).</span></span> <span data-ttu-id="ede45-116">Stegen i det här dokumentet använder Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="ede45-116">The steps in this document use Visual Studio 2017.</span></span>

* <span data-ttu-id="ede45-117">Ett sätt att överföra .exe-filer till klustret och köra Pig och Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="ede45-117">A way to upload .exe files to the cluster and run Pig and Hive jobs.</span></span> <span data-ttu-id="ede45-118">Vi rekommenderar Data Lake-verktyg för Visual Studio, Azure PowerShell och Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ede45-118">We recommend the Data Lake Tools for Visual Studio, Azure PowerShell and Azure CLI.</span></span> <span data-ttu-id="ede45-119">Stegen i det här dokumentet används Data Lake-verktyg för Visual Studio för att överföra filer och köra exemplet Hive frågan.</span><span class="sxs-lookup"><span data-stu-id="ede45-119">The steps in this document use the Data Lake Tools for Visual Studio to upload the files and run the example Hive query.</span></span>

    <span data-ttu-id="ede45-120">Information om andra sätt att köra Hive-frågor och Pig-jobb, finns i följande dokument:</span><span class="sxs-lookup"><span data-stu-id="ede45-120">For information on other ways to run Hive queries and Pig jobs, see the following documents:</span></span>

    * [<span data-ttu-id="ede45-121">Använda Apache Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="ede45-121">Use Apache Hive with HDInsight</span></span>](hdinsight-use-hive.md)

    * [<span data-ttu-id="ede45-122">Använda Apache Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="ede45-122">Use Apache Pig with HDInsight</span></span>](hdinsight-use-pig.md)

* <span data-ttu-id="ede45-123">En Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ede45-123">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="ede45-124">Mer information om hur du skapar ett kluster finns [skapar ett HDInsight-kluster](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="ede45-124">For more information on creating a cluster, see [Create an HDInsight cluster](hdinsight-provision-clusters.md).</span></span>

## <a name="net-on-hdinsight"></a><span data-ttu-id="ede45-125">.NET på HDInsight</span><span class="sxs-lookup"><span data-stu-id="ede45-125">.NET on HDInsight</span></span>

* <span data-ttu-id="ede45-126">__Linux-baserat HDInsight__ kluster med [Mono (https://mono-project.com)](https://mono-project.com) att köra .NET-program.</span><span class="sxs-lookup"><span data-stu-id="ede45-126">__Linux-based HDInsight__ clusters using [Mono (https://mono-project.com)](https://mono-project.com) to run .NET applications.</span></span> <span data-ttu-id="ede45-127">Monoljud version 4.2.1 ingår i HDInsight version 3.5.</span><span class="sxs-lookup"><span data-stu-id="ede45-127">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span>

    <span data-ttu-id="ede45-128">Mer information om monoljud kompatibilitet med .NET Framework-versioner finns [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="ede45-128">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

    <span data-ttu-id="ede45-129">Om du vill använda en viss version av Mono, finns det [installera eller uppdatera Mono](hdinsight-hadoop-install-mono.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="ede45-129">To use a specific version of Mono, see the [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

* <span data-ttu-id="ede45-130">__Windows-baserade HDInsight__ kluster använder Microsoft .NET CLR för att köra .NET-program.</span><span class="sxs-lookup"><span data-stu-id="ede45-130">__Windows-based HDInsight__ clusters use the Microsoft .NET CLR to run .NET applications.</span></span>

<span data-ttu-id="ede45-131">Mer information om versionen av .NET framework och Mono som ingår i HDInsight-versioner finns [HDInsight komponenten versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="ede45-131">For more information on the version of the .NET framework and Mono included with HDInsight versions, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span>

## <a name="create-the-c-projects"></a><span data-ttu-id="ede45-132">Skapa C\# projekt</span><span class="sxs-lookup"><span data-stu-id="ede45-132">Create the C\# projects</span></span>

### <a name="hive-udf"></a><span data-ttu-id="ede45-133">Hive UDF</span><span class="sxs-lookup"><span data-stu-id="ede45-133">Hive UDF</span></span>

1. <span data-ttu-id="ede45-134">Öppna Visual Studio och skapa en lösning.</span><span class="sxs-lookup"><span data-stu-id="ede45-134">Open Visual Studio and create a solution.</span></span> <span data-ttu-id="ede45-135">Projekttypen, Välj **Konsolapp (.NET Framework)**, och namnet på det nya projektet **HiveCSharp**.</span><span class="sxs-lookup"><span data-stu-id="ede45-135">For the project type, select **Console App (.NET Framework)**, and name the new project **HiveCSharp**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ede45-136">Välj __.NET Framework 4.5__ om du använder en Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ede45-136">Select __.NET Framework 4.5__ if you are using a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="ede45-137">Mer information om monoljud kompatibilitet med .NET Framework-versioner finns [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="ede45-137">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

2. <span data-ttu-id="ede45-138">Ersätt innehållet i **Program.cs** med följande:</span><span class="sxs-lookup"><span data-stu-id="ede45-138">Replace the contents of **Program.cs** with the following:</span></span>

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
                    // Parse the string, trimming line feeds
                    // and splitting fields at tabs
                    line = line.TrimEnd('\n');
                    string[] field = line.Split('\t');
                    string phoneLabel = field[1] + ' ' + field[2];
                    // Emit new data to stdout, delimited by tabs
                    Console.WriteLine("{0}\t{1}\t{2}", field[0], phoneLabel, GetMD5Hash(phoneLabel));
                }
            }
            /// <summary>
            /// Returns an MD5 hash for the given string
            /// </summary>
            /// <param name="input">string value</param>
            /// <returns>an MD5 hash</returns>
            static string GetMD5Hash(string input)
            {
                // Step 1, calculate MD5 hash from input
                MD5 md5 = System.Security.Cryptography.MD5.Create();
                byte[] inputBytes = System.Text.Encoding.ASCII.GetBytes(input);
                byte[] hash = md5.ComputeHash(inputBytes);

                // Step 2, convert byte array to hex string
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

3. <span data-ttu-id="ede45-139">Bygga projektet.</span><span class="sxs-lookup"><span data-stu-id="ede45-139">Build the project.</span></span>

### <a name="pig-udf"></a><span data-ttu-id="ede45-140">Pig UDF</span><span class="sxs-lookup"><span data-stu-id="ede45-140">Pig UDF</span></span>

1. <span data-ttu-id="ede45-141">Öppna Visual Studio och skapa en lösning.</span><span class="sxs-lookup"><span data-stu-id="ede45-141">Open Visual Studio and create a solution.</span></span> <span data-ttu-id="ede45-142">Projekttypen, Välj **konsolprogram**, och namnet på det nya projektet **PigUDF**.</span><span class="sxs-lookup"><span data-stu-id="ede45-142">For the project type, select **Console Application**, and name the new project **PigUDF**.</span></span>

2. <span data-ttu-id="ede45-143">Ersätt innehållet i den **Program.cs** med följande kod:</span><span class="sxs-lookup"><span data-stu-id="ede45-143">Replace the contents of the **Program.cs** file with the following code:</span></span>

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
                        // Trim the error info off the beginning and add a note to the end of the line
                        line = line.Remove(0, 21) + " - java.lang.Exception";
                    }
                    // Split the fields apart at tab characters
                    string[] field = line.Split('\t');
                    // Put fields back together for writing
                    Console.WriteLine(String.Join("\t",field));
                }
            }
        }
    }
    ```

    <span data-ttu-id="ede45-144">Det här programmet Parsar rader skickas från Pig och formatera om rader som börjar med `java.lang.Exception`.</span><span class="sxs-lookup"><span data-stu-id="ede45-144">This application parses the lines sent from Pig, and reformat lines that begin with `java.lang.Exception`.</span></span>

3. <span data-ttu-id="ede45-145">Spara **Program.cs**, och sedan skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="ede45-145">Save **Program.cs**, and then build the project.</span></span>

## <a name="upload-to-storage"></a><span data-ttu-id="ede45-146">Överföra till lagring</span><span class="sxs-lookup"><span data-stu-id="ede45-146">Upload to storage</span></span>

1. <span data-ttu-id="ede45-147">Öppna i Visual Studio **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ede45-147">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="ede45-148">Expandera **Azure** och expandera därefter **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="ede45-148">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="ede45-149">Om du uppmanas ange dina autentiseringsuppgifter för Azure-prenumeration och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="ede45-149">If prompted, enter your Azure subscription credentials, and then click **Sign In**.</span></span>

4. <span data-ttu-id="ede45-150">Expandera HDInsight-klustret som du vill distribuera programmet till.</span><span class="sxs-lookup"><span data-stu-id="ede45-150">Expand the HDInsight cluster that you wish to deploy this application to.</span></span> <span data-ttu-id="ede45-151">En post med texten __(standard Storage-konto)__ visas.</span><span class="sxs-lookup"><span data-stu-id="ede45-151">An entry with the text __(Default Storage Account)__ is listed.</span></span>

    ![Server Explorer visar lagringskontot för klustret](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * <span data-ttu-id="ede45-153">Om den här posten kan utökas måste du använder en __Azure Storage-konto__ som standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="ede45-153">If this entry can be expanded, you are using an __Azure Storage Account__ as default storage for the cluster.</span></span> <span data-ttu-id="ede45-154">Om du vill visa filerna på lagringsutrymmet som standard för klustret, expandera posten och dubbelklicka sedan på den __(standardbehållaren)__.</span><span class="sxs-lookup"><span data-stu-id="ede45-154">To view the files on the default storage for the cluster, expand the entry and then double-click the __(Default Container)__.</span></span>

    * <span data-ttu-id="ede45-155">Om den här posten inte kan expanderas, använder du __Azure Data Lake Store__ som standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="ede45-155">If this entry cannot be expanded, you are using __Azure Data Lake Store__ as the default storage for the cluster.</span></span> <span data-ttu-id="ede45-156">Om du vill visa filerna på lagringsutrymmet som standard för klustret, dubbelklickar du på den __(standardkontot för lagring)__ post.</span><span class="sxs-lookup"><span data-stu-id="ede45-156">To view the files on the default storage for the cluster, double-click the __(Default Storage Account)__ entry.</span></span>

6. <span data-ttu-id="ede45-157">Använd någon av följande metoder för att ladda upp .exe-filer:</span><span class="sxs-lookup"><span data-stu-id="ede45-157">To upload the .exe files, use one of the following methods:</span></span>

    * <span data-ttu-id="ede45-158">Om du använder en __Azure Storage-konto__, klicka på ikonen överföringen och bläddra sedan till den **bin\debug** mapp för den **HiveCSharp** projekt.</span><span class="sxs-lookup"><span data-stu-id="ede45-158">If using an __Azure Storage Account__, click the upload icon, and then browse to the **bin\debug** folder for the **HiveCSharp** project.</span></span> <span data-ttu-id="ede45-159">Välj slutligen den **HiveCSharp.exe** fil och klicka på **Ok**.</span><span class="sxs-lookup"><span data-stu-id="ede45-159">Finally, select the **HiveCSharp.exe** file and click **Ok**.</span></span>

        ![Överför ikon](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * <span data-ttu-id="ede45-161">Om du använder __Azure Data Lake Store__, högerklicka på ett tomt område i listan över filer och välj sedan __överför__.</span><span class="sxs-lookup"><span data-stu-id="ede45-161">If using __Azure Data Lake Store__, right-click an empty area in the file listing, and then select __Upload__.</span></span> <span data-ttu-id="ede45-162">Välj slutligen den **HiveCSharp.exe** fil och klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="ede45-162">Finally, select the **HiveCSharp.exe** file and click **Open**.</span></span>

    <span data-ttu-id="ede45-163">En gång i __HiveCSharp.exe__ överföringen har slutförts, Upprepa processen för att ladda upp den __PigUDF.exe__ fil.</span><span class="sxs-lookup"><span data-stu-id="ede45-163">Once the __HiveCSharp.exe__ upload has finished, repeat the upload process for the __PigUDF.exe__ file.</span></span>

## <a name="run-a-hive-query"></a><span data-ttu-id="ede45-164">Köra en Hive-fråga</span><span class="sxs-lookup"><span data-stu-id="ede45-164">Run a Hive query</span></span>

1. <span data-ttu-id="ede45-165">Öppna i Visual Studio **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ede45-165">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="ede45-166">Expandera **Azure** och expandera därefter **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="ede45-166">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="ede45-167">Högerklicka på klustret som du har distribuerat den **HiveCSharp** program till och markera sedan **skriva en Hive-fråga**.</span><span class="sxs-lookup"><span data-stu-id="ede45-167">Right-click the cluster that you deployed the **HiveCSharp** application to, and then select **Write a Hive Query**.</span></span>

4. <span data-ttu-id="ede45-168">Använd följande text för Hive-frågan:</span><span class="sxs-lookup"><span data-stu-id="ede45-168">Use the following text for the Hive query:</span></span>

    ```hiveql
    -- Uncomment the following if you are using Azure Storage
    -- add file wasb:///HiveCSharp.exe;
    -- Uncomment the following if you are using Azure Data Lake Store
    -- add file adl:///HiveCSharp.exe;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'HiveCSharp.exe' AS
    (clientid string, phoneLabel string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="ede45-169">Ta bort kommentarerna i `add file` -uttryck som matchar typ av standard lagringsutrymme som används för klustret.</span><span class="sxs-lookup"><span data-stu-id="ede45-169">Uncomment the `add file` statement that matches the type of default storage used for your cluster.</span></span>

    <span data-ttu-id="ede45-170">Den här frågan markeras den `clientid`, `devicemake`, och `devicemodel` fält från `hivesampletable`, och skickar fälten till HiveCSharp.exe-programmet.</span><span class="sxs-lookup"><span data-stu-id="ede45-170">This query selects the `clientid`, `devicemake`, and `devicemodel` fields from `hivesampletable`, and passes the fields to the HiveCSharp.exe application.</span></span> <span data-ttu-id="ede45-171">Frågan förväntar sig programmet att returnera tre fält, som lagras som `clientid`, `phoneLabel`, och `phoneHash`.</span><span class="sxs-lookup"><span data-stu-id="ede45-171">The query expects the application to return three fields, which are stored as `clientid`, `phoneLabel`, and `phoneHash`.</span></span> <span data-ttu-id="ede45-172">Frågan också förväntas HiveCSharp.exe i roten av standardbehållare för lagring.</span><span class="sxs-lookup"><span data-stu-id="ede45-172">The query also expects to find HiveCSharp.exe in the root of the default storage container.</span></span>

5. <span data-ttu-id="ede45-173">Klicka på **skicka** att skicka jobb till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="ede45-173">Click **Submit** to submit the job to the HDInsight cluster.</span></span> <span data-ttu-id="ede45-174">Den **Hive-jobbsammanfattning** öppnas.</span><span class="sxs-lookup"><span data-stu-id="ede45-174">The **Hive Job Summary** window opens.</span></span>

6. <span data-ttu-id="ede45-175">Klicka på **uppdatera** att uppdatera sammanfattning tills **jobbstatus** ändras till **slutförd**.</span><span class="sxs-lookup"><span data-stu-id="ede45-175">Click **Refresh** to refresh the summary until **Job Status** changes to **Completed**.</span></span> <span data-ttu-id="ede45-176">Om du vill visa jobbutdata klickar du på **Jobbutdata**.</span><span class="sxs-lookup"><span data-stu-id="ede45-176">To view the job output, click **Job Output**.</span></span>

## <a name="run-a-pig-job"></a><span data-ttu-id="ede45-177">Kör ett Pig-jobb</span><span class="sxs-lookup"><span data-stu-id="ede45-177">Run a Pig job</span></span>

1. <span data-ttu-id="ede45-178">Använd någon av följande metoder för att ansluta till ditt HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="ede45-178">Use one of the following methods to connect to your HDInsight cluster:</span></span>

    * <span data-ttu-id="ede45-179">Om du använder en __Linux-baserade__ HDInsight-kluster använder SSH.</span><span class="sxs-lookup"><span data-stu-id="ede45-179">If you are using a __Linux-based__ HDInsight cluster, use SSH.</span></span> <span data-ttu-id="ede45-180">Till exempel `ssh sshuser@mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="ede45-180">For example, `ssh sshuser@mycluster-ssh.azurehdinsight.net`.</span></span> <span data-ttu-id="ede45-181">Mer information finns i [använda SSH withHDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="ede45-181">For more information, see [Use SSH withHDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>
    
    * <span data-ttu-id="ede45-182">Om du använder en __Windows-baserade__ HDInsight-kluster [Anslut till klustret med hjälp av fjärrskrivbord](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span><span class="sxs-lookup"><span data-stu-id="ede45-182">If you are using a __Windows-based__ HDInsight cluster, [Connect to the cluster using Remote Desktop](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span></span>

2. <span data-ttu-id="ede45-183">Använd en följande kommando för att starta Pig-kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="ede45-183">Use one the following command to start the Pig command line:</span></span>

        pig

    > [!IMPORTANT]
    > <span data-ttu-id="ede45-184">Om du använder ett Windows-baserade kluster använder du följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ede45-184">If you are using a Windows-based cluster, use the following commands instead:</span></span>
    > ```
    > cd %PIG_HOME%
    > bin\pig
    > ```

    <span data-ttu-id="ede45-185">En `grunt>` visas frågan.</span><span class="sxs-lookup"><span data-stu-id="ede45-185">A `grunt>` prompt is displayed.</span></span>

3. <span data-ttu-id="ede45-186">Ange följande om du vill köra ett Pig-jobb som använder .NET Framework-program:</span><span class="sxs-lookup"><span data-stu-id="ede45-186">Enter the following to run a Pig job that uses the .NET Framework application:</span></span>

        DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
        LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    <span data-ttu-id="ede45-187">Den `DEFINE` uttrycket skapar ett alias för `streamer` för pigudf.exe-program och `CACHE` läser från standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="ede45-187">The `DEFINE` statement creates an alias of `streamer` for the pigudf.exe applications, and `CACHE` loads it from default storage for the cluster.</span></span> <span data-ttu-id="ede45-188">Senare, `streamer` används tillsammans med den `STREAM` operatorn för att bearbeta de rader som finns i LOGGEN och returnera data som en serie kolumner.</span><span class="sxs-lookup"><span data-stu-id="ede45-188">Later, `streamer` is used with the `STREAM` operator to process the single lines contained in LOG and return the data as a series of columns.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ede45-189">Namnet på programmet som används för strömning måste omges av den \` (backtick) tecken när ett alias, och ' (enkelt citattecken) när det används med `SHIP\`.</span><span class="sxs-lookup"><span data-stu-id="ede45-189">The application name that is used for streaming must be surrounded by the \` (backtick) character when aliased, and ' (single quote) when used with `SHIP\`.</span></span>

4. <span data-ttu-id="ede45-190">När du har angett den sista raden ska jobbet starta.</span><span class="sxs-lookup"><span data-stu-id="ede45-190">After entering the last line, the job should start.</span></span> <span data-ttu-id="ede45-191">Den returnerar utdata liknar följande:</span><span class="sxs-lookup"><span data-stu-id="ede45-191">It returns output similar to the following text:</span></span>

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="next-steps"></a><span data-ttu-id="ede45-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ede45-192">Next steps</span></span>

<span data-ttu-id="ede45-193">I det här dokumentet har du lärt dig hur du använder .NET Framework-program från Hive och Pig på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ede45-193">In this document, you have learned how to use a .NET Framework application from Hive and Pig on HDInsight.</span></span> <span data-ttu-id="ede45-194">Om du vill lära dig hur du använder Python med Hive och Pig [använder Python med Hive och Pig i HDInsight](hdinsight-python.md).</span><span class="sxs-lookup"><span data-stu-id="ede45-194">If you would like to learn how to use Python with Hive and Pig, see [Use Python with Hive and Pig in HDInsight](hdinsight-python.md).</span></span>

<span data-ttu-id="ede45-195">Andra sätt att använda Pig och Hive och lära dig mer om MapReduce finns i följande dokument:</span><span class="sxs-lookup"><span data-stu-id="ede45-195">For other ways to use Pig and Hive, and to learn about using MapReduce, see the following documents:</span></span>

* [<span data-ttu-id="ede45-196">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="ede45-196">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="ede45-197">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="ede45-197">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="ede45-198">Använda MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="ede45-198">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)
