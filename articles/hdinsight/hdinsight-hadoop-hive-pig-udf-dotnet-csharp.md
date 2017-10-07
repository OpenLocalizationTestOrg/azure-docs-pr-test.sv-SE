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
# <a name="use-c-user-defined-functions-with-hive-and-pig-streaming-on-hadoop-in-hdinsight"></a>Använda C# användardefinierade funktioner med Hive och Pig strömning på Hadoop i HDInsight

Lär dig hur toouse C# användardefinierade funktioner (UDF) med Apache Hive och Pig på HDInsight.

> [!IMPORTANT]
> hello stegen i det här dokumentet fungerar med både Linux- och Windows-baserade HDInsight-kluster. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight component-versioning](hdinsight-component-versioning.md).

Både Hive och Pig kan skicka data tooexternal program för bearbetning. Den här processen kallas _strömning_. När du använder en .NET-kretsmönstret toohello programmet hello data skickas vidare STDIN och returnerar hello programmet hello resultat på STDOUT. tooread och Skriv från STDIN och STDOUT, kan du använda `Console.ReadLine()` och `Console.WriteLine()` från ett konsolprogram.

## <a name="prerequisites"></a>Krav

* Tidigare erfarenhet av skrivning och bygga C#-kod som riktar sig till .NET Framework 4.5.

    * Använd oavsett IDE som du vill använda. Vi rekommenderar [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, eller [Visual Studio Code](https://code.visualstudio.com/). hello stegen i det här dokumentet används Visual Studio 2017.

* Ett sätt tooupload .exe-filer toohello klustret och köra Pig och Hive-jobb. Vi rekommenderar hello Data Lake-verktyg för Visual Studio, Azure PowerShell och Azure CLI. hello stegen i det här dokumentet använder hello Data Lake-verktyg för Visual Studio tooupload hello filer och kör hello exempel Hive-fråga.

    Information om andra sätt toorun Hive-frågor och Pig-jobb finns hello följande dokument:

    * [Använda Apache Hive med HDInsight](hdinsight-use-hive.md)

    * [Använda Apache Pig med HDInsight](hdinsight-use-pig.md)

* En Hadoop på HDInsight-kluster. Mer information om hur du skapar ett kluster finns [skapar ett HDInsight-kluster](hdinsight-provision-clusters.md).

## <a name="net-on-hdinsight"></a>.NET på HDInsight

* __Linux-baserat HDInsight__ kluster med [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET-program. Monoljud version 4.2.1 ingår i HDInsight version 3.5.

    Mer information om monoljud kompatibilitet med .NET Framework-versioner finns [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/).

    toouse en viss version av Mono, se hello [installera eller uppdatera Mono](hdinsight-hadoop-install-mono.md) dokumentet.

* __Windows-baserade HDInsight__ kluster använder hello Microsoft .NET CLR toorun .NET-program.

Läs mer på hello hello .NET framework och Mono som ingår i HDInsight-versioner, [HDInsight komponenten versioner](hdinsight-component-versioning.md).

## <a name="create-hello-c-projects"></a>Skapa hello C\# projekt

### <a name="hive-udf"></a>Hive UDF

1. Öppna Visual Studio och skapa en lösning. Hello projekttypen Välj **Konsolapp (.NET Framework)**, och namnet hello nytt projekt **HiveCSharp**.

    > [!IMPORTANT]
    > Välj __.NET Framework 4.5__ om du använder en Linux-baserade HDInsight-kluster. Mer information om monoljud kompatibilitet med .NET Framework-versioner finns [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/).

2. Ersätt hello innehållet i **Program.cs** med hello följande:

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

3. Skapa hello-projekt.

### <a name="pig-udf"></a>Pig UDF

1. Öppna Visual Studio och skapa en lösning. Hello projekttypen Välj **konsolprogram**, och namnet hello nytt projekt **PigUDF**.

2. Ersätt hello innehållet i hello **Program.cs** fil med hello följande kod:

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

    Det här programmet Parsar hello rader skickas från Pig och formatera om rader som börjar med `java.lang.Exception`.

3. Spara **Program.cs**, och sedan skapa hello-projekt.

## <a name="upload-toostorage"></a>Överför toostorage

1. Öppna i Visual Studio **Server Explorer**.

2. Expandera **Azure** och expandera därefter **HDInsight**.

3. Om du uppmanas ange dina autentiseringsuppgifter för Azure-prenumeration och klicka sedan på **logga In**.

4. Expandera hello HDInsight-kluster som du vill toodeploy programmet. En post med hello text __(standard Storage-konto)__ visas.

    ![Server Explorer som visar hello storage-konto för hello-kluster](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * Om den här posten kan utökas måste du använder en __Azure Storage-konto__ som standard lagringen för hello klustret. tooview hello filer på hello standardlagring för hello-kluster expanderar hello-post och dubbelklicka sedan på hello __(standardbehållaren)__.

    * Om den här posten inte kan expanderas, använder du __Azure Data Lake Store__ som hello standardlagring för hello-kluster. tooview hello filer på hello standardlagring för hello kluster, dubbelklicka på hello __(standardkontot för lagring)__ post.

6. tooupload hello .exe-filer med någon av följande metoder hello:

    * Om du använder en __Azure Storage-konto__, klicka på ikonen för hello överföringen och bläddra sedan toohello **bin\debug** mapp för hello **HiveCSharp** projekt. Välj slutligen hello **HiveCSharp.exe** fil och klicka på **Ok**.

        ![Överför ikon](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * Om du använder __Azure Data Lake Store__, högerklicka på ett tomt område i hello med och välj sedan __överför__. Välj slutligen hello **HiveCSharp.exe** fil och klicka på **öppna**.

    En gång hello __HiveCSharp.exe__ överföringen har slutförts, upprepa hello överföringsprocessen för hello __PigUDF.exe__ fil.

## <a name="run-a-hive-query"></a>Köra en Hive-fråga

1. Öppna i Visual Studio **Server Explorer**.

2. Expandera **Azure** och expandera därefter **HDInsight**.

3. Högerklicka på hello klustret som du distribuerat hello **HiveCSharp** program till och markera sedan **skriva en Hive-fråga**.

4. Använd hello följande text för hello Hive-fråga:

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
    > Ta bort kommentarerna hello `add file` uttryck som matchar hello typ av standard lagringsutrymme som används för klustret.

    Den här frågan väljer hello `clientid`, `devicemake`, och `devicemodel` fält från `hivesampletable`, och skickar hello fält toohello HiveCSharp.exe program. hello fråga förväntas hello programmet tooreturn tre fält, som lagras som `clientid`, `phoneLabel`, och `phoneHash`. hello fråga förväntas också toofind HiveCSharp.exe i hello rot hello standardbehållare för lagring.

5. Klicka på **skicka** toosubmit hello jobbet toohello HDInsight-kluster. Hej **Hive-jobbsammanfattning** öppnas.

6. Klicka på **uppdatera** toorefresh hello sammanfattning tills **jobbstatus** ändras för**slutförd**. tooview hello-jobbets utdatafil, klickar du på **Jobbutdata**.

## <a name="run-a-pig-job"></a>Kör ett Pig-jobb

1. Använd någon av följande metoder tooconnect tooyour HDInsight-kluster hello:

    * Om du använder en __Linux-baserade__ HDInsight-kluster använder SSH. Till exempel `ssh sshuser@mycluster-ssh.azurehdinsight.net`. Mer information finns i [använda SSH withHDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * Om du använder en __Windows-baserade__ HDInsight-kluster [Anslut toohello kluster med hjälp av fjärrskrivbord](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)

2. Använd en hello efter kommandot toostart hello Pig kommandoraden:

        pig

    > [!IMPORTANT]
    > Om du använder ett Windows-baserade kluster använder du följande kommandon i stället hello:
    > ```
    > cd %PIG_HOME%
    > bin\pig
    > ```

    En `grunt>` visas frågan.

3. Ange hello följande toorun ett Pig-jobb som använder hello .NET Framework-program:

        DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
        LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    Hej `DEFINE` uttrycket skapar ett alias för `streamer` för hello pigudf.exe program och `CACHE` läser från standard lagringen för hello klustret. Senare, `streamer` används med hello `STREAM` operatorn tooprocess hello rader som ingår i LOGGEN och returnera hello som en serie kolumner.

    > [!NOTE]
    > hello programnamn som används för strömning måste omges av hello \` (backtick) tecken när ett alias, och ' (enkelt citattecken) när det används med `SHIP`.

4. När du har angett hello sista raden ska hello jobbet starta. Den returnerar utdata liknande toohello följande text:

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="next-steps"></a>Nästa steg

I det här dokumentet har du lärt dig hur toouse .NET Framework-program från Hive och Pig på HDInsight. Om du vill att toolearn hur toouse Python med Hive och Pig, se [används Python med Hive och Pig i HDInsight](hdinsight-python.md).

Andra sätt toouse Pig och Hive och toolearn om hur du använder MapReduce finns hello följande dokument:

* [Använda Hive med HDInsight](hdinsight-use-hive.md)
* [Använda Pig med HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med HDInsight](hdinsight-use-mapreduce.md)
