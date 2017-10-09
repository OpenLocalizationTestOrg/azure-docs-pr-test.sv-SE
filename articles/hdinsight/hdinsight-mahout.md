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
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight-powershell"></a><span data-ttu-id="ae235-103">Generera filmrekommendationer med hjälp av Apache Mahout med Hadoop i HDInsight (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="ae235-103">Generate movie recommendations by using Apache Mahout with Hadoop in HDInsight (PowerShell)</span></span>

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

<span data-ttu-id="ae235-104">Lär dig hur toouse hello [Apache Mahout](http://mahout.apache.org) machine learning-biblioteket med Azure HDInsight toogenerate filmrekommendationer.</span><span class="sxs-lookup"><span data-stu-id="ae235-104">Learn how toouse hello [Apache Mahout](http://mahout.apache.org) machine learning library with Azure HDInsight toogenerate movie recommendations.</span></span> <span data-ttu-id="ae235-105">hello exemplet i det här dokumentet används Azure PowerShell toorun Mahout jobb.</span><span class="sxs-lookup"><span data-stu-id="ae235-105">hello example in this document uses Azure PowerShell toorun Mahout jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ae235-106">Krav</span><span class="sxs-lookup"><span data-stu-id="ae235-106">Prerequisites</span></span>

* <span data-ttu-id="ae235-107">Ett Linux-baserat HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ae235-107">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="ae235-108">Information om hur du skapar en finns [komma igång med Linux-baserade Hadoop i HDInsight][getstarted].</span><span class="sxs-lookup"><span data-stu-id="ae235-108">For information about creating one, see [Get started using Linux-based Hadoop in HDInsight][getstarted].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ae235-109">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ae235-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ae235-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ae235-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* [<span data-ttu-id="ae235-111">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ae235-111">Azure PowerShell</span></span>](/powershell/azure/overview)

## <span data-ttu-id="ae235-112"><a name="recommendations"></a>Skapa rekommendationer med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ae235-112"><a name="recommendations"></a>Generate recommendations by using Azure PowerShell</span></span>

> [!WARNING]
> <span data-ttu-id="ae235-113">hello jobbet i det här avsnittet fungerar med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ae235-113">hello job in this section works by using Azure PowerShell.</span></span> <span data-ttu-id="ae235-114">Många av hello-klasser som medföljer Mahout fungerar inte för närvarande med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ae235-114">Many of hello classes provided with Mahout do not currently work with Azure PowerShell.</span></span> <span data-ttu-id="ae235-115">En lista med klasser som inte fungerar med Azure PowerShell finns i hello [felsökning](#troubleshooting) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ae235-115">For a list of classes that do not work with Azure PowerShell, see hello [Troubleshooting](#troubleshooting) section.</span></span>
>
> <span data-ttu-id="ae235-116">Ett exempel på hur du använder SSH tooconnect tooHDInsight och kör Mahout exempel direkt på klustret hello finns [generera filmrekommendationer med hjälp av Mahout och HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span><span class="sxs-lookup"><span data-stu-id="ae235-116">For an example of using SSH tooconnect tooHDInsight and run Mahout examples directly on hello cluster, see [Generate movie recommendations using Mahout and HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span></span>

<span data-ttu-id="ae235-117">En av hello-funktioner som tillhandahålls av Mahout är en rekommendation motor.</span><span class="sxs-lookup"><span data-stu-id="ae235-117">One of hello functions that is provided by Mahout is a recommendation engine.</span></span> <span data-ttu-id="ae235-118">Den här motorn accepterar data i hello-format för `userID`, `itemId`, och `prefValue` (hello användare inställningar för hello objekt).</span><span class="sxs-lookup"><span data-stu-id="ae235-118">This engine accepts data in hello format of `userID`, `itemId`, and `prefValue` (hello users preference for hello item).</span></span> <span data-ttu-id="ae235-119">Mahout använder hello data toodetermine användare med liknande objekt inställningar, vilket kan vara används toomake rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="ae235-119">Mahout uses hello data toodetermine users with like-item preferences, which can be used toomake recommendations.</span></span>

<span data-ttu-id="ae235-120">hello är följande exempel en förenklad genomgång av hur hello rekommendation processen fungerar:</span><span class="sxs-lookup"><span data-stu-id="ae235-120">hello following example is a simplified walk-through of how hello recommendation process works:</span></span>

* <span data-ttu-id="ae235-121">**samtidigt förekomsten**: Joe Alice och Bob alla tyckte *Star krig*, *hello Empire strejker tillbaka*, och *tillbaka hello Jedi*.</span><span class="sxs-lookup"><span data-stu-id="ae235-121">**co-occurrence**: Joe, Alice, and Bob all liked *Star Wars*, *hello Empire Strikes Back*, and *Return of hello Jedi*.</span></span> <span data-ttu-id="ae235-122">Mahout anger att användare som någon av dessa filmer också hello andra två.</span><span class="sxs-lookup"><span data-stu-id="ae235-122">Mahout determines that users who like any one of these movies also like hello other two.</span></span>

* <span data-ttu-id="ae235-123">**samtidigt förekomsten**: Bob och Alice också tyckte *hello Phantom hot*, *Attack av hello kloner*, och *Revenge av hello Sith*.</span><span class="sxs-lookup"><span data-stu-id="ae235-123">**co-occurrence**: Bob and Alice also liked *hello Phantom Menace*, *Attack of hello Clones*, and *Revenge of hello Sith*.</span></span> <span data-ttu-id="ae235-124">Mahout anger att användare också tyckte hello föregående tre filmer som dessa filmer.</span><span class="sxs-lookup"><span data-stu-id="ae235-124">Mahout determines that users who liked hello previous three movies also like these movies.</span></span>

* <span data-ttu-id="ae235-125">**Likhet rekommendation**: Joe eftersom tyckte hello tre första filmer, Mahout tittar på filmer som andra med liknande inställningar tyckte om, men Johan har inte bevakade (tyckte/klassificerad).</span><span class="sxs-lookup"><span data-stu-id="ae235-125">**Similarity recommendation**: Because Joe liked hello first three movies, Mahout looks at movies that others with similar preferences liked, but Joe has not watched (liked/rated).</span></span> <span data-ttu-id="ae235-126">I det här fallet Mahout rekommenderar *hello Phantom hot*, *Attack av hello kloner*, och *Revenge av hello Sith*.</span><span class="sxs-lookup"><span data-stu-id="ae235-126">In this case, Mahout recommends *hello Phantom Menace*, *Attack of hello Clones*, and *Revenge of hello Sith*.</span></span>

### <a name="understanding-hello-data"></a><span data-ttu-id="ae235-127">Förstå hello data</span><span class="sxs-lookup"><span data-stu-id="ae235-127">Understanding hello data</span></span>

<span data-ttu-id="ae235-128">[GroupLens Research] [ movielens] innehåller klassificering av data för filmer i ett format som är kompatibel med Mahout.</span><span class="sxs-lookup"><span data-stu-id="ae235-128">[GroupLens Research][movielens] provides rating data for movies in a format that is compatible with Mahout.</span></span> <span data-ttu-id="ae235-129">Dessa data är tillgängliga på hello standardlagring för klustret på `/HdiSamples//HdiSamples/MahoutMovieData`.</span><span class="sxs-lookup"><span data-stu-id="ae235-129">This data is available on hello default storage for your cluster at `/HdiSamples//HdiSamples/MahoutMovieData`.</span></span>

<span data-ttu-id="ae235-130">Det finns två filer, `moviedb.txt` (information om hello filmer) och `user-ratings.txt`.</span><span class="sxs-lookup"><span data-stu-id="ae235-130">There are two files, `moviedb.txt` (information about hello movies) and `user-ratings.txt`.</span></span> <span data-ttu-id="ae235-131">Hej `user-ratings.txt` fil som ska användas vid analys.</span><span class="sxs-lookup"><span data-stu-id="ae235-131">hello `user-ratings.txt` file is used during analysis.</span></span> <span data-ttu-id="ae235-132">Hej `moviedb.txt` filen är används tooprovide användarvänliga text hello resultat från hello analys.</span><span class="sxs-lookup"><span data-stu-id="ae235-132">hello `moviedb.txt` file is used tooprovide user-friendly text when displaying hello results of hello analysis.</span></span>

<span data-ttu-id="ae235-133">hello data i användaren ratings.txt har en struktur med `userID`, `movieID`, `userRating`, och `timestamp`, som talar om för oss hur mycket en film klassificerats som varje användare.</span><span class="sxs-lookup"><span data-stu-id="ae235-133">hello data contained in user-ratings.txt has a structure of `userID`, `movieID`, `userRating`, and `timestamp`, which tells us how highly each user rated a movie.</span></span> <span data-ttu-id="ae235-134">Här är ett exempel på hello data:</span><span class="sxs-lookup"><span data-stu-id="ae235-134">Here is an example of hello data:</span></span>

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

### <a name="run-hello-job"></a><span data-ttu-id="ae235-135">Kör jobb för hello</span><span class="sxs-lookup"><span data-stu-id="ae235-135">Run hello job</span></span>

<span data-ttu-id="ae235-136">Använd följande Windows PowerShell-skript toorun ett jobb som använder hello Mahout rekommendation motor med hello filmdata hello:</span><span class="sxs-lookup"><span data-stu-id="ae235-136">Use hello following Windows PowerShell script toorun a job that uses hello Mahout recommendation engine with hello movie data:</span></span>

> [!NOTE]
> <span data-ttu-id="ae235-137">Den här filen efterfrågas information som används tooconnect tooyour HDInsight-kluster och köra jobb.</span><span class="sxs-lookup"><span data-stu-id="ae235-137">This file prompts you for information that is used tooconnect tooyour HDInsight cluster and run jobs.</span></span> <span data-ttu-id="ae235-138">Det kan ta flera minuter innan hello jobb toocomplete och hämta hello output.txt fil.</span><span class="sxs-lookup"><span data-stu-id="ae235-138">It may take several minutes for hello jobs toocomplete and download hello output.txt file.</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=5-98)]

> [!NOTE]
> <span data-ttu-id="ae235-139">Mahout jobb ta inte bort tillfälliga data som har skapats under bearbetning av hello jobb.</span><span class="sxs-lookup"><span data-stu-id="ae235-139">Mahout jobs do not remove temporary data that is created while processing hello job.</span></span> <span data-ttu-id="ae235-140">Hej `--tempDir` parameter har angetts i hello exempel jobbet tooisolate hello temporära filer i en specifik katalog.</span><span class="sxs-lookup"><span data-stu-id="ae235-140">hello `--tempDir` parameter is specified in hello example job tooisolate hello temporary files into a specific directory.</span></span>

<span data-ttu-id="ae235-141">Hej Mahout jobbet returnerar inte hello utdata tooSTDOUT.</span><span class="sxs-lookup"><span data-stu-id="ae235-141">hello Mahout job does not return hello output tooSTDOUT.</span></span> <span data-ttu-id="ae235-142">I stället lagras den i hello angivna målkatalogen som **del-r-00000**.</span><span class="sxs-lookup"><span data-stu-id="ae235-142">Instead, it stores it in hello specified output directory as **part-r-00000**.</span></span> <span data-ttu-id="ae235-143">hello skript hämtar den här filen för**output.txt** i hello aktuell katalog på din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="ae235-143">hello script downloads this file too**output.txt** in hello current directory on your workstation.</span></span>

<span data-ttu-id="ae235-144">hello är följande ett exempel på hello innehållet i den här filen:</span><span class="sxs-lookup"><span data-stu-id="ae235-144">hello following text is an example of hello content of this file:</span></span>

    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

<span data-ttu-id="ae235-145">hello första kolumnen är hello `userID`.</span><span class="sxs-lookup"><span data-stu-id="ae235-145">hello first column is hello `userID`.</span></span> <span data-ttu-id="ae235-146">Hej värden i ' [' och ']' är `movieId`:`recommendationScore`.</span><span class="sxs-lookup"><span data-stu-id="ae235-146">hello values contained in '[' and ']' are `movieId`:`recommendationScore`.</span></span>

<span data-ttu-id="ae235-147">hello skript hämtar också hello `moviedb.txt` och `user-ratings.txt` filer, som är nödvändiga tooformat hello utdata toobe mer lättläst.</span><span class="sxs-lookup"><span data-stu-id="ae235-147">hello script also downloads hello `moviedb.txt` and `user-ratings.txt` files, which are needed tooformat hello output toobe more readable.</span></span>

### <a name="view-hello-output"></a><span data-ttu-id="ae235-148">Visa hello utdata</span><span class="sxs-lookup"><span data-stu-id="ae235-148">View hello output</span></span>

<span data-ttu-id="ae235-149">Även om hello genererade utdata kan vara OK för användning i ett program, är det inte användarvänliga.</span><span class="sxs-lookup"><span data-stu-id="ae235-149">Although hello generated output might be OK for use in an application, it's not user-friendly.</span></span> <span data-ttu-id="ae235-150">Hej `moviedb.txt` från hello-server kan vara används tooresolve hello `movieId` tooa film namn.</span><span class="sxs-lookup"><span data-stu-id="ae235-150">hello `moviedb.txt` from hello server can be used tooresolve hello `movieId` tooa movie name.</span></span> <span data-ttu-id="ae235-151">Använd följande PowerShell-skriptet toodisplay rekommendationer med film namn hello:</span><span class="sxs-lookup"><span data-stu-id="ae235-151">Use hello following PowerShell script toodisplay recommendations with movie names:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=106-180)]

<span data-ttu-id="ae235-152">Använd hello följande kommando toodisplay hello rekommendationerna i ett användarvänligt format:</span><span class="sxs-lookup"><span data-stu-id="ae235-152">Use hello following command toodisplay hello recommendations in a user-friendly format:</span></span> 

```powershell
.\show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt
```

<span data-ttu-id="ae235-153">hello utdata är liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="ae235-153">hello output is similar toohello following text:</span></span>

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

## <span data-ttu-id="ae235-154"><a name="troubleshooting"></a>Felsökning</span><span class="sxs-lookup"><span data-stu-id="ae235-154"><a name="troubleshooting"></a>Troubleshooting</span></span>

### <a name="cannot-overwrite-files"></a><span data-ttu-id="ae235-155">Det går inte att skriva över filer</span><span class="sxs-lookup"><span data-stu-id="ae235-155">Cannot overwrite files</span></span>

<span data-ttu-id="ae235-156">Mahout jobb vill inte ta bort temporära filer som skapades under bearbetning.</span><span class="sxs-lookup"><span data-stu-id="ae235-156">Mahout jobs do not clean up temporary files that were created during processing.</span></span> <span data-ttu-id="ae235-157">Dessutom skriver hello jobb inte över befintliga utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="ae235-157">In addition, hello jobs do not overwrite existing output file.</span></span>

<span data-ttu-id="ae235-158">tooavoid fel när du kör Mahout jobb, ta bort temporära och utgående filer mellan körs.</span><span class="sxs-lookup"><span data-stu-id="ae235-158">tooavoid errors when running Mahout jobs, delete temporary and output files between runs.</span></span> <span data-ttu-id="ae235-159">tooremove hello filer som skapas av hello tidigare skript i det här dokumentet använder hello följande PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="ae235-159">tooremove hello files created by hello earlier scripts in this document, use hello following PowerShell script:</span></span>

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

### <span data-ttu-id="ae235-160"><a name="nopowershell"></a>Klasser som inte fungerar med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ae235-160"><a name="nopowershell"></a>Classes that do not work with Azure PowerShell</span></span>

<span data-ttu-id="ae235-161">Mahout jobb som använder följande klasser hello returnera olika felmeddelanden när används från Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="ae235-161">Mahout jobs that use hello following classes return various error messages when used from Windows PowerShell:</span></span>

* <span data-ttu-id="ae235-162">org.apache.mahout.utils.clustering.ClusterDumper</span><span class="sxs-lookup"><span data-stu-id="ae235-162">org.apache.mahout.utils.clustering.ClusterDumper</span></span>
* <span data-ttu-id="ae235-163">org.apache.mahout.utils.SequenceFileDumper</span><span class="sxs-lookup"><span data-stu-id="ae235-163">org.apache.mahout.utils.SequenceFileDumper</span></span>
* <span data-ttu-id="ae235-164">org.apache.mahout.utils.vectors.lucene.Driver</span><span class="sxs-lookup"><span data-stu-id="ae235-164">org.apache.mahout.utils.vectors.lucene.Driver</span></span>
* <span data-ttu-id="ae235-165">org.apache.mahout.utils.vectors.arff.Driver</span><span class="sxs-lookup"><span data-stu-id="ae235-165">org.apache.mahout.utils.vectors.arff.Driver</span></span>
* <span data-ttu-id="ae235-166">org.apache.mahout.text.WikipediaToSequenceFile</span><span class="sxs-lookup"><span data-stu-id="ae235-166">org.apache.mahout.text.WikipediaToSequenceFile</span></span>
* <span data-ttu-id="ae235-167">org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles</span><span class="sxs-lookup"><span data-stu-id="ae235-167">org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles</span></span>
* <span data-ttu-id="ae235-168">org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer</span><span class="sxs-lookup"><span data-stu-id="ae235-168">org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer</span></span>
* <span data-ttu-id="ae235-169">org.apache.mahout.classifier.sgd.TrainLogistic</span><span class="sxs-lookup"><span data-stu-id="ae235-169">org.apache.mahout.classifier.sgd.TrainLogistic</span></span>
* <span data-ttu-id="ae235-170">org.apache.mahout.classifier.sgd.RunLogistic</span><span class="sxs-lookup"><span data-stu-id="ae235-170">org.apache.mahout.classifier.sgd.RunLogistic</span></span>
* <span data-ttu-id="ae235-171">org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic</span><span class="sxs-lookup"><span data-stu-id="ae235-171">org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic</span></span>
* <span data-ttu-id="ae235-172">org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic</span><span class="sxs-lookup"><span data-stu-id="ae235-172">org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic</span></span>
* <span data-ttu-id="ae235-173">org.apache.mahout.classifier.sgd.RunAdaptiveLogistic</span><span class="sxs-lookup"><span data-stu-id="ae235-173">org.apache.mahout.classifier.sgd.RunAdaptiveLogistic</span></span>
* <span data-ttu-id="ae235-174">org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer</span><span class="sxs-lookup"><span data-stu-id="ae235-174">org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer</span></span>
* <span data-ttu-id="ae235-175">org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator</span><span class="sxs-lookup"><span data-stu-id="ae235-175">org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator</span></span>
* <span data-ttu-id="ae235-176">org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator</span><span class="sxs-lookup"><span data-stu-id="ae235-176">org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator</span></span>
* <span data-ttu-id="ae235-177">org.apache.mahout.classifier.df.tools.Describe</span><span class="sxs-lookup"><span data-stu-id="ae235-177">org.apache.mahout.classifier.df.tools.Describe</span></span>

<span data-ttu-id="ae235-178">toorun jobb som använder dessa klasser Anslut toohello HDInsight-kluster med SSH och kör hello jobb från hello kommandorad.</span><span class="sxs-lookup"><span data-stu-id="ae235-178">toorun jobs that use these classes, connect toohello HDInsight cluster using SSH and run hello jobs from hello command line.</span></span> <span data-ttu-id="ae235-179">Ett exempel på hur du använder SSH toorun Mahout jobb finns [generera filmrekommendationer med hjälp av Mahout och HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span><span class="sxs-lookup"><span data-stu-id="ae235-179">For an example of using SSH toorun Mahout jobs, see [Generate movie recommendations using Mahout and HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae235-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ae235-180">Next steps</span></span>

<span data-ttu-id="ae235-181">Nu när du har lärt dig hur toouse Mahout, identifiera andra sätt att arbeta med data i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="ae235-181">Now that you have learned how toouse Mahout, discover other ways of working with data on HDInsight:</span></span>

* [<span data-ttu-id="ae235-182">Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="ae235-182">Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="ae235-183">Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="ae235-183">Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="ae235-184">MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="ae235-184">MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

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
