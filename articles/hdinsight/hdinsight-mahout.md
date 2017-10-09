---
title: "aaaGenerate rekommendationer med hjälp av Mahout HDInsight från PowerShell - Azure | Microsoft Docs"
description: "Lär dig hur toouse hello Apache Mahout machine learning biblioteket toogenerate filmrekommendationer med HDInsight (Hadoop) från ett PowerShell-skript som körs på klienten."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 07b57208-32aa-4e59-900a-6c934fa1b7a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: 675a2cd8ecaf7fc797d6cd094e4e58f9aca7ed92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight-powershell"></a>Generera filmrekommendationer med hjälp av Apache Mahout med Hadoop i HDInsight (PowerShell)

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Lär dig hur toouse hello [Apache Mahout](http://mahout.apache.org) machine learning-biblioteket med Azure HDInsight toogenerate filmrekommendationer. hello exemplet i det här dokumentet används Azure PowerShell toorun Mahout jobb.

## <a name="prerequisites"></a>Krav

* Ett Linux-baserat HDInsight-kluster. Information om hur du skapar en finns [komma igång med Linux-baserade Hadoop i HDInsight][getstarted].

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Azure PowerShell](/powershell/azure/overview)

## <a name="recommendations"></a>Skapa rekommendationer med hjälp av Azure PowerShell

> [!WARNING]
> hello jobbet i det här avsnittet fungerar med hjälp av Azure PowerShell. Många av hello-klasser som medföljer Mahout fungerar inte för närvarande med Azure PowerShell. En lista med klasser som inte fungerar med Azure PowerShell finns i hello [felsökning](#troubleshooting) avsnitt.
>
> Ett exempel på hur du använder SSH tooconnect tooHDInsight och kör Mahout exempel direkt på klustret hello finns [generera filmrekommendationer med hjälp av Mahout och HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).

En av hello-funktioner som tillhandahålls av Mahout är en rekommendation motor. Den här motorn accepterar data i hello-format för `userID`, `itemId`, och `prefValue` (hello användare inställningar för hello objekt). Mahout använder hello data toodetermine användare med liknande objekt inställningar, vilket kan vara används toomake rekommendationer.

hello är följande exempel en förenklad genomgång av hur hello rekommendation processen fungerar:

* **samtidigt förekomsten**: Joe Alice och Bob alla tyckte *Star krig*, *hello Empire strejker tillbaka*, och *tillbaka hello Jedi*. Mahout anger att användare som någon av dessa filmer också hello andra två.

* **samtidigt förekomsten**: Bob och Alice också tyckte *hello Phantom hot*, *Attack av hello kloner*, och *Revenge av hello Sith*. Mahout anger att användare också tyckte hello föregående tre filmer som dessa filmer.

* **Likhet rekommendation**: Joe eftersom tyckte hello tre första filmer, Mahout tittar på filmer som andra med liknande inställningar tyckte om, men Johan har inte bevakade (tyckte/klassificerad). I det här fallet Mahout rekommenderar *hello Phantom hot*, *Attack av hello kloner*, och *Revenge av hello Sith*.

### <a name="understanding-hello-data"></a>Förstå hello data

[GroupLens Research] [ movielens] innehåller klassificering av data för filmer i ett format som är kompatibel med Mahout. Dessa data är tillgängliga på hello standardlagring för klustret på `/HdiSamples//HdiSamples/MahoutMovieData`.

Det finns två filer, `moviedb.txt` (information om hello filmer) och `user-ratings.txt`. Hej `user-ratings.txt` fil som ska användas vid analys. Hej `moviedb.txt` filen är används tooprovide användarvänliga text hello resultat från hello analys.

hello data i användaren ratings.txt har en struktur med `userID`, `movieID`, `userRating`, och `timestamp`, som talar om för oss hur mycket en film klassificerats som varje användare. Här är ett exempel på hello data:

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

### <a name="run-hello-job"></a>Kör jobb för hello

Använd följande Windows PowerShell-skript toorun ett jobb som använder hello Mahout rekommendation motor med hello filmdata hello:

> [!NOTE]
> Den här filen efterfrågas information som används tooconnect tooyour HDInsight-kluster och köra jobb. Det kan ta flera minuter innan hello jobb toocomplete och hämta hello output.txt fil.

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=5-98)]

> [!NOTE]
> Mahout jobb ta inte bort tillfälliga data som har skapats under bearbetning av hello jobb. Hej `--tempDir` parameter har angetts i hello exempel jobbet tooisolate hello temporära filer i en specifik katalog.

Hej Mahout jobbet returnerar inte hello utdata tooSTDOUT. I stället lagras den i hello angivna målkatalogen som **del-r-00000**. hello skript hämtar den här filen för**output.txt** i hello aktuell katalog på din arbetsstation.

hello är följande ett exempel på hello innehållet i den här filen:

    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

hello första kolumnen är hello `userID`. Hej värden i ' [' och ']' är `movieId`:`recommendationScore`.

hello skript hämtar också hello `moviedb.txt` och `user-ratings.txt` filer, som är nödvändiga tooformat hello utdata toobe mer lättläst.

### <a name="view-hello-output"></a>Visa hello utdata

Även om hello genererade utdata kan vara OK för användning i ett program, är det inte användarvänliga. Hej `moviedb.txt` från hello-server kan vara används tooresolve hello `movieId` tooa film namn. Använd följande PowerShell-skriptet toodisplay rekommendationer med film namn hello:

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=106-180)]

Använd hello följande kommando toodisplay hello rekommendationerna i ett användarvänligt format: 

```powershell
.\show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt
```

hello utdata är liknande toohello följande text:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, hello (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of hello Dove, hello (1997)            4.6666665
    People vs. Larry Flynt, hello (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237

## <a name="troubleshooting"></a>Felsökning

### <a name="cannot-overwrite-files"></a>Det går inte att skriva över filer

Mahout jobb vill inte ta bort temporära filer som skapades under bearbetning. Dessutom skriver hello jobb inte över befintliga utdatafilen.

tooavoid fel när du kör Mahout jobb, ta bort temporära och utgående filer mellan körs. tooremove hello filer som skapas av hello tidigare skript i det här dokumentet använder hello följande PowerShell-skript:

```powershell
# Login tooyour Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter hello HDInsight cluster name"
$creds=Get-Credential -Message "Enter hello login for hello cluster"

#Get hello cluster info so we can get hello resource group, storage, etc.
$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
$container = $clusterInfo.DefaultStorageContainer
$storageAccountKey = (Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage context and upload hello file
$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

#Azure PowerShell can't delete blobs using wildcard,
#so have tooget a list and delete one at a time
# Start with hello output
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/out"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
# Next hello temp files
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/temp"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
```

### <a name="nopowershell"></a>Klasser som inte fungerar med Azure PowerShell

Mahout jobb som använder följande klasser hello returnera olika felmeddelanden när används från Windows PowerShell:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

toorun jobb som använder dessa klasser Anslut toohello HDInsight-kluster med SSH och kör hello jobb från hello kommandorad. Ett exempel på hur du använder SSH toorun Mahout jobb finns [generera filmrekommendationer med hjälp av Mahout och HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur toouse Mahout, identifiera andra sätt att arbeta med data i HDInsight:

* [Hive med HDInsight](hdinsight-use-hive.md)
* [Pig med HDInsight](hdinsight-use-pig.md)
* [MapReduce med HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: /powershell/azureps-cmdlets-docs
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
