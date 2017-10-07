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
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a>Använda C# med MapReduce strömning på Hadoop i HDInsight

Lär dig hur toouse C# toocreate MapReduce lösningar för HDInsight.

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight component-versioning](hdinsight-component-versioning.md).

Hadoop streaming är ett verktyg som gör att du använder ett skript eller körbara filer toorun MapReduce-jobb. I det här exemplet är .NET används tooimplement hello mapper och reducer för en word antal lösning.

## <a name="net-on-hdinsight"></a>.NET på HDInsight

__Linux-baserat HDInsight__ kluster Använd [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET-program. Monoljud version 4.2.1 ingår i HDInsight version 3.5. Mer information om hello version Mono som ingår i HDInsight finns [HDInsight komponenten versioner](hdinsight-component-versioning.md). toouse en viss version av Mono, se hello [installera eller uppdatera Mono](hdinsight-hadoop-install-mono.md) dokumentet.

Mer information om monoljud kompatibilitet med .NET Framework-versioner finns [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/).

## <a name="how-hadoop-streaming-works"></a>Så här fungerar Hadoop-strömning

hello grundläggande process används för strömning i det här dokumentet är följande:

1. Hadoop skickar datamappning toohello (mapper.exe i det här exemplet) på STDIN.
2. hello mapper bearbetar hello data och genererar tabbavgränsad nyckel/värde-par tooSTDOUT.
3. hello utdata läses av Hadoop och skickas sedan STDIN toohello reducer (reducer.exe i det här exemplet).
4. Hej reducer läser hello tabbavgränsad nyckel/värde-par, bearbetar hello data och skickar sedan hello resultatet som tabbavgränsad nyckel/värde-par i STDOUT.
5. hello utdata läses av Hadoop och skrivs toohello målkatalogen.

Mer information om strömning finns [Hadoop Streaming (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).

## <a name="prerequisites"></a>Krav

* Tidigare erfarenhet av skrivning och bygga C#-kod som riktar sig till .NET Framework 4.5. hello stegen i det här dokumentet används Visual Studio 2017.

* Ett sätt tooupload .exe-filer toohello kluster. hello använder stegen i det här dokumentet hello Data Lake-verktyg för Visual Studio tooupload hello filer tooprimary lagringen för hello klustret.

* Azure PowerShell eller en SSH-klient.

* En Hadoop på HDInsight-kluster. Mer information om hur du skapar ett kluster finns [skapar ett HDInsight-kluster](hdinsight-provision-clusters.md).

## <a name="create-hello-mapper"></a>Skapa hello mapper

I Visual Studio skapar en ny __Konsolapp__ med namnet __mapper__. Använd följande kod för hello programmet hello:

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

När du har skapat programmet hello skapar det tooproduce hello `/bin/Debug/mapper.exe` filen i hello projektkatalogen.

## <a name="create-hello-reducer"></a>Skapa hello reducer

I Visual Studio skapar en ny __Konsolapp__ med namnet __reducer__. Använd följande kod för hello programmet hello:

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

När du har skapat programmet hello skapar det tooproduce hello `/bin/Debug/reducer.exe` filen i hello projektkatalogen.

## <a name="upload-toostorage"></a>Överför toostorage

1. Öppna i Visual Studio **Server Explorer**.

2. Expandera **Azure** och expandera därefter **HDInsight**.

3. Om du uppmanas ange dina autentiseringsuppgifter för Azure-prenumeration och klicka sedan på **logga In**.

4. Expandera hello HDInsight-kluster som du vill toodeploy programmet. En post med hello text __(standard Storage-konto)__ visas.

    ![Server Explorer som visar hello storage-konto för hello-kluster](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * Om den här posten kan utökas måste du använder en __Azure Storage-konto__ som standard lagringen för hello klustret. tooview hello filer på hello standardlagring för hello-kluster expanderar hello-post och dubbelklicka sedan på hello __(standardbehållaren)__.

    * Om den här posten inte kan expanderas, använder du __Azure Data Lake Store__ som hello standardlagring för hello-kluster. tooview hello filer på hello standardlagring för hello kluster, dubbelklicka på hello __(standardkontot för lagring)__ post.

5. tooupload hello .exe-filer med någon av följande metoder hello:

    * Om du använder en __Azure Storage-konto__, klicka på ikonen för hello överföringen och bläddra sedan toohello **bin\debug** mapp för hello **mapper** projekt. Välj slutligen hello **mapper.exe** fil och klicka på **Ok**.

        ![Överför ikon](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * Om du använder __Azure Data Lake Store__, högerklicka på ett tomt område i hello med och välj sedan __överför__. Välj slutligen hello **mapper.exe** fil och klicka på **öppna**.

    En gång hello __mapper.exe__ överföringen har slutförts, upprepa hello överföringsprocessen för hello __reducer.exe__ fil.

## <a name="run-a-job-using-an-ssh-session"></a>Kör ett jobb: använder en SSH-session

1. Använda SSH tooconnect toohello HDInsight-kluster. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Använd någon av följande kommando toostart hello MapReduce-jobb hello:

    * Om du använder __Datasjölager__ som standardlagring:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * Om du använder __Azure Storage__ som standardlagring:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    hello följande lista beskrivs vad varje parameter är:

    * `hadoop-streaming.jar`: hello jar-filen som innehåller hello strömning MapReduce-funktioner.
    * `-files`: Lägger till hello `mapper.exe` och `reducer.exe` filer toothis jobb. Hej `adl:///` eller `wasb:///` innan varje fil är hello sökvägen toohello roten standardlagring för hello-kluster.
    * `-mapper`: Anger vilken fil som implementerar hello mapper.
    * `-reducer`: Anger vilken fil som implementerar hello reducer.
    * `-input`: hello indata.
    * `-output`: hello målkatalogen.

3. När hello MapReduce-jobbet har slutförts använder du följande tooview hello resultat hello:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    hello är följande ett exempel på hello data som returneras av kommandot:

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a>Kör ett jobb: med hjälp av PowerShell

Använd följande PowerShell-skriptet toorun ett MapReduce-jobb hello och hämta hello resultat.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

Det här skriptet efterfrågar hello klustret konto inloggningsnamn och lösenord, tillsammans med hello HDInsight-klustrets namn. När hello jobbet är slutfört, hello utdata är hämtade toohello `output.txt` filen i hello directory hello skriptet kördes från. hello följande är ett exempel på hello data i hello `output.txt` fil:

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a>Nästa steg

Mer information om hur du använder MapReduce med HDInsight finns [använda MapReduce med HDInsight](hdinsight-use-mapreduce.md).

Information om hur du använder C# med Hive och Pig finns [använder en C# användardefinierad funktion med Hive och Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).

Information om hur du använder C# med Storm på HDInsight finns i [utveckla C#-topologier för Storm på HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).