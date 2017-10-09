---
title: "aaaGenerate rekommendationer med hjälp av Mahout och HDInsight (SSH) - Azure | Microsoft Docs"
description: "Lär dig hur toouse hello Apache Mahout maskininlärning biblioteket toogenerate filmrekommendationer med HDInsight (Hadoop)."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c78ec37c-9a8c-4bb6-9e38-0bdb9e89fbd7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: fedac9ceb4268f8421bce4623a5ad271041b8b3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight-ssh"></a><span data-ttu-id="3c7e3-103">Generera filmrekommendationer med hjälp av Apache Mahout med Linux-baserade Hadoop i HDInsight (SSH)</span><span class="sxs-lookup"><span data-stu-id="3c7e3-103">Generate movie recommendations by using Apache Mahout with Linux-based Hadoop in HDInsight (SSH)</span></span>

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

<span data-ttu-id="3c7e3-104">Lär dig hur toouse hello [Apache Mahout](http://mahout.apache.org) machine learning-biblioteket med Azure HDInsight toogenerate filmrekommendationer.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-104">Learn how toouse hello [Apache Mahout](http://mahout.apache.org) machine learning library with Azure HDInsight toogenerate movie recommendations.</span></span>

<span data-ttu-id="3c7e3-105">Mahout är en [maskininlärning] [ ml] -biblioteket för Apache Hadoop.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-105">Mahout is a [machine learning][ml] library for Apache Hadoop.</span></span> <span data-ttu-id="3c7e3-106">Mahout innehåller algoritmer för bearbetning av data, till exempel filtrering, klassificering, och klustring.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-106">Mahout contains algorithms for processing data, such as filtering, classification, and clustering.</span></span> <span data-ttu-id="3c7e3-107">I den här artikeln använder du en rekommendation motorn toogenerate filmrekommendationer som baseras på dina vänner har sett filmer.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-107">In this article, you use a recommendation engine toogenerate movie recommendations that are based on movies your friends have seen.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c7e3-108">Krav</span><span class="sxs-lookup"><span data-stu-id="3c7e3-108">Prerequisites</span></span>

* <span data-ttu-id="3c7e3-109">Ett Linux-baserat HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-109">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="3c7e3-110">Information om hur du skapar en finns [komma igång med Linux-baserade Hadoop i HDInsight][getstarted].</span><span class="sxs-lookup"><span data-stu-id="3c7e3-110">For information about creating one, see [Get started using Linux-based Hadoop in HDInsight][getstarted].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3c7e3-111">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3c7e3-112">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="3c7e3-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="3c7e3-113">En SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-113">An SSH client.</span></span> <span data-ttu-id="3c7e3-114">Mer information finns i hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-114">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

## <a name="mahout-versioning"></a><span data-ttu-id="3c7e3-115">Mahout versionshantering</span><span class="sxs-lookup"><span data-stu-id="3c7e3-115">Mahout versioning</span></span>

<span data-ttu-id="3c7e3-116">Mer information om hello-versionen av Mahout i HDInsight finns [HDInsight versioner och Hadoop-komponenter](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="3c7e3-116">For more information about hello version of Mahout in HDInsight, see [HDInsight versions and Hadoop components](hdinsight-component-versioning.md).</span></span>

## <span data-ttu-id="3c7e3-117"><a name="recommendations"></a>Förstå rekommendationer</span><span class="sxs-lookup"><span data-stu-id="3c7e3-117"><a name="recommendations"></a>Understanding recommendations</span></span>

<span data-ttu-id="3c7e3-118">En av hello-funktioner som tillhandahålls av Mahout är en rekommendation motor.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-118">One of hello functions that is provided by Mahout is a recommendation engine.</span></span> <span data-ttu-id="3c7e3-119">Den här motorn accepterar data i hello-format för `userID`, `itemId`, och `prefValue` (hello inställningar för hello objekt).</span><span class="sxs-lookup"><span data-stu-id="3c7e3-119">This engine accepts data in hello format of `userID`, `itemId`, and `prefValue` (hello preference for hello item).</span></span> <span data-ttu-id="3c7e3-120">Mahout kan sedan utföra samtidigt förekomst analysis toodetermine: *användare som har en inställning för ett objekt har också en inställning för dessa andra objekt*.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-120">Mahout can then perform co-occurance analysis toodetermine: *users who have a preference for an item also have a preference for these other items*.</span></span> <span data-ttu-id="3c7e3-121">Mahout bestämmer användare med liknande objekt inställningar, vilket kan vara används toomake rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-121">Mahout then determines users with like-item preferences, which can be used toomake recommendations.</span></span>

<span data-ttu-id="3c7e3-122">hello är följande arbetsflöde en förenklad exempel som använder filmdata:</span><span class="sxs-lookup"><span data-stu-id="3c7e3-122">hello following workflow is a simplified example that uses movie data:</span></span>

* <span data-ttu-id="3c7e3-123">**Samtidigt förekomst**: Joe Alice och Bob alla tyckte *Star krig*, *hello Empire strejker tillbaka*, och *tillbaka hello Jedi*.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-123">**Co-occurance**: Joe, Alice, and Bob all liked *Star Wars*, *hello Empire Strikes Back*, and *Return of hello Jedi*.</span></span> <span data-ttu-id="3c7e3-124">Mahout anger att användare som någon av dessa filmer också hello andra två.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-124">Mahout determines that users who like any one of these movies also like hello other two.</span></span>

* <span data-ttu-id="3c7e3-125">**Samtidigt förekomst**: Bob och Alice också tyckte *hello Phantom hot*, *Attack av hello kloner*, och *Revenge av hello Sith*.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-125">**Co-occurance**: Bob and Alice also liked *hello Phantom Menace*, *Attack of hello Clones*, and *Revenge of hello Sith*.</span></span> <span data-ttu-id="3c7e3-126">Mahout anger att användare också tyckte hello föregående tre filmer som dessa tre filmer.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-126">Mahout determines that users who liked hello previous three movies also like these three movies.</span></span>

* <span data-ttu-id="3c7e3-127">**Likhet rekommendation**: Joe eftersom tyckte hello tre första filmer, Mahout tittar på filmer som andra med liknande inställningar tyckte om, men Johan har inte bevakade (tyckte/klassificerad).</span><span class="sxs-lookup"><span data-stu-id="3c7e3-127">**Similarity recommendation**: Because Joe liked hello first three movies, Mahout looks at movies that others with similar preferences liked, but Joe has not watched (liked/rated).</span></span> <span data-ttu-id="3c7e3-128">I det här fallet Mahout rekommenderar *hello Phantom hot*, *Attack av hello kloner*, och *Revenge av hello Sith*.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-128">In this case, Mahout recommends *hello Phantom Menace*, *Attack of hello Clones*, and *Revenge of hello Sith*.</span></span>

### <a name="understanding-hello-data"></a><span data-ttu-id="3c7e3-129">Förstå hello data</span><span class="sxs-lookup"><span data-stu-id="3c7e3-129">Understanding hello data</span></span>

<span data-ttu-id="3c7e3-130">Enkelt, [GroupLens Research] [ movielens] innehåller klassificering av data för filmer i ett format som är kompatibel med Mahout.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-130">Conveniently, [GroupLens Research][movielens] provides rating data for movies in a format that is compatible with Mahout.</span></span> <span data-ttu-id="3c7e3-131">Dessa data är tillgängliga på ditt kluster standardlagring på `/HdiSamples/HdiSamples/MahoutMovieData`.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-131">This data is available on your cluster's default storage at `/HdiSamples/HdiSamples/MahoutMovieData`.</span></span>

<span data-ttu-id="3c7e3-132">Det finns två filer, `moviedb.txt` och `user-ratings.txt`.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-132">There are two files, `moviedb.txt` and `user-ratings.txt`.</span></span> <span data-ttu-id="3c7e3-133">hello användaren ratings.txt filen används under analys, medan moviedb.txt används tooprovide användarvänliga textinformation hello resultat från hello analys.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-133">hello user-ratings.txt file is used during analysis, while moviedb.txt is used tooprovide user-friendly text information when displaying hello results of hello analysis.</span></span>

<span data-ttu-id="3c7e3-134">hello data i användaren ratings.txt har en struktur med `userID`, `movieID`, `userRating`, och `timestamp`, som talar om för oss hur mycket en film klassificerats som varje användare.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-134">hello data contained in user-ratings.txt has a structure of `userID`, `movieID`, `userRating`, and `timestamp`, which tells us how highly each user rated a movie.</span></span> <span data-ttu-id="3c7e3-135">Här är ett exempel på hello data:</span><span class="sxs-lookup"><span data-stu-id="3c7e3-135">Here is an example of hello data:</span></span>

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

## <a name="run-hello-analysis"></a><span data-ttu-id="3c7e3-136">Kör hello analys</span><span class="sxs-lookup"><span data-stu-id="3c7e3-136">Run hello analysis</span></span>

<span data-ttu-id="3c7e3-137">Använd hello följande kommando från en SSH-anslutning toohello kluster toorun hello rekommendation jobb:</span><span class="sxs-lookup"><span data-stu-id="3c7e3-137">From an SSH connection toohello cluster, use hello following command toorun hello recommendation job:</span></span>

```bash
mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
```

> [!NOTE]
> <span data-ttu-id="3c7e3-138">hello jobbet kan ta flera minuter toocomplete och kan köra flera MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-138">hello job may take several minutes toocomplete, and may run multiple MapReduce jobs.</span></span>

## <a name="view-hello-output"></a><span data-ttu-id="3c7e3-139">Visa hello utdata</span><span class="sxs-lookup"><span data-stu-id="3c7e3-139">View hello output</span></span>

1. <span data-ttu-id="3c7e3-140">När hello jobbet är slutfört, använder du hello följande kommandoutdata tooview hello genereras:</span><span class="sxs-lookup"><span data-stu-id="3c7e3-140">Once hello job completes, use hello following command tooview hello generated output:</span></span>

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    <span data-ttu-id="3c7e3-141">hello utdata visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="3c7e3-141">hello output appears as follows:</span></span>

        1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    <span data-ttu-id="3c7e3-142">hello första kolumnen är hello `userID`.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-142">hello first column is hello `userID`.</span></span> <span data-ttu-id="3c7e3-143">Hej värden i ' [' och ']' är `movieId`:`recommendationScore`.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-143">hello values contained in '[' and ']' are `movieId`:`recommendationScore`.</span></span>

2. <span data-ttu-id="3c7e3-144">Du kan använda hello utdata tillsammans med hello moviedb.txt, tooprovide mer information på hello rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-144">You can use hello output, along with hello moviedb.txt, tooprovide more information on hello recommendations.</span></span> <span data-ttu-id="3c7e3-145">Först måste vi toocopy hello filer lokalt med hjälp av hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3c7e3-145">First, we need toocopy hello files locally using hello following commands:</span></span>

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    <span data-ttu-id="3c7e3-146">Det här kommandot kopior hello tooa utdatafilen med namnet **recommendations.txt** i hello aktuella katalogen, tillsammans med hello film datafiler.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-146">This command copies hello output data tooa file named **recommendations.txt** in hello current directory, along with hello movie data files.</span></span>

3. <span data-ttu-id="3c7e3-147">Använd följande kommando toocreate Python-skriptet som slår upp film namn för hello data i hello rekommendationer utdata hello:</span><span class="sxs-lookup"><span data-stu-id="3c7e3-147">Use hello following command toocreate a Python script that looks up movie names for hello data in hello recommendations output:</span></span>

    ```bash
    nano show_recommendations.py
    ```

    <span data-ttu-id="3c7e3-148">Använd följande text som hello innehållet i filen hello hello när hello redigeraren öppnas och:</span><span class="sxs-lookup"><span data-stu-id="3c7e3-148">When hello editor opens, use hello following text as hello contents of hello file:</span></span>

   ```python
   #!/usr/bin/env python

   import sys

   if len(sys.argv) != 5:
        print "Arguments: userId userDataFilename movieFilename recommendationFilename"
        sys.exit(1)

   userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

   print "Reading Movies Descriptions"
   movieFile = open(movieFilename)
   movieById = {}
   for line in movieFile:
       tokens = line.split("|")
       movieById[tokens[0]] = tokens[1:]
   movieFile.close()

   print "Reading Rated Movies"
   userDataFile = open(userDataFilename)
   ratedMovieIds = []
   for line in userDataFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           ratedMovieIds.append((tokens[1],tokens[2]))
   userDataFile.close()

   print "Reading Recommendations"
   recommendationFile = open(recommendationFilename)
   recommendations = []
   for line in recommendationFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           movieIdAndScores = tokens[1].strip("[]\n").split(",")
           recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
           break
   recommendationFile.close()

   print "Rated Movies"
   print "------------------------"
   for movieId, rating in ratedMovieIds:
       print "%s, rating=%s" % (movieById[movieId][0], rating)
   print "------------------------"

   print "Recommended Movies"
   print "------------------------"
   for movieId, score in recommendations:
       print "%s, score=%s" % (movieById[movieId][0], score)
   print "------------------------"
   ```

    <span data-ttu-id="3c7e3-149">Tryck på **Ctrl + X**, **Y**, och slutligen **RETUR** toosave hello data.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-149">Press **Ctrl-X**, **Y**, and finally **Enter** toosave hello data.</span></span>

4. <span data-ttu-id="3c7e3-150">Kör hello Python-skriptet.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-150">Run hello Python script.</span></span> <span data-ttu-id="3c7e3-151">hello förutsätter följande kommando att du är i hello directory där alla hello-filer har hämtats:</span><span class="sxs-lookup"><span data-stu-id="3c7e3-151">hello following command assumes you are in hello directory where all hello files were downloaded:</span></span>

    ```bash
    python show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    <span data-ttu-id="3c7e3-152">Detta kommando kontrollerar hello rekommendationer som genererats för användaren ID 4.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-152">This command looks at hello recommendations generated for user ID 4.</span></span>

    * <span data-ttu-id="3c7e3-153">Hej **användare ratings.txt** filen har använt tooretrieve filmer som har bedömts.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-153">hello **user-ratings.txt** file is used tooretrieve movies that have been rated.</span></span>

    * <span data-ttu-id="3c7e3-154">Hej **moviedb.txt** filen har använt tooretrieve hello namnen på hello filmer.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-154">hello **moviedb.txt** file is used tooretrieve hello names of hello movies.</span></span>

    * <span data-ttu-id="3c7e3-155">Hej **recommendations.txt** är används tooretrieve hello filmrekommendationer för den här användaren.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-155">hello **recommendations.txt** is used tooretrieve hello movie recommendations for this user.</span></span>

     <span data-ttu-id="3c7e3-156">hello utdata från det här kommandot är liknande toohello, som följande text:</span><span class="sxs-lookup"><span data-stu-id="3c7e3-156">hello output from this command is similar toohello following text:</span></span>

        <span data-ttu-id="3c7e3-157">Sju år i Tibet (1997), poängsätta = 5.0 Indiana brev och hello senaste Crusade (1989) poängsätta = 5.0 Jaws (1975) poäng = 5.0 mening och känseln (1995) poäng = 5.0 oberoende dag (ID4) (1996), poäng = 5.0 min bästa vän Wedding (1997) poängsätta = 5.0 Jerry Maguire (1996 ), poäng = 5.0 Scream 2 (1997), poäng = 5.0 tid tooKill, (1996), poängsätta = 5.0</span><span class="sxs-lookup"><span data-stu-id="3c7e3-157">Seven Years in Tibet (1997), score=5.0   Indiana Jones and hello Last Crusade (1989), score=5.0   Jaws (1975), score=5.0   Sense and Sensibility (1995), score=5.0   Independence Day (ID4) (1996), score=5.0   My Best Friend's Wedding (1997), score=5.0   Jerry Maguire (1996), score=5.0   Scream 2 (1997), score=5.0   Time tooKill, A (1996), score=5.0</span></span>

## <a name="delete-temporary-data"></a><span data-ttu-id="3c7e3-158">Ta bort temporära data</span><span class="sxs-lookup"><span data-stu-id="3c7e3-158">Delete temporary data</span></span>

<span data-ttu-id="3c7e3-159">Mahout jobb ta inte bort tillfälliga data som har skapats under bearbetning av hello jobb.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-159">Mahout jobs do not remove temporary data that is created while processing hello job.</span></span> <span data-ttu-id="3c7e3-160">Hej `--tempDir` parameter har angetts i hello exempel jobbet tooisolate hello temporära filer i en specifik sökväg för enkelt borttagning.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-160">hello `--tempDir` parameter is specified in hello example job tooisolate hello temporary files into a specific path for easy deletion.</span></span> <span data-ttu-id="3c7e3-161">tooremove hello temporära filer, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3c7e3-161">tooremove hello temp files, use hello following command:</span></span>

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]
> <span data-ttu-id="3c7e3-162">Om du vill toorun hello kommandot igen, måste du också ta bort hello målkatalogen.</span><span class="sxs-lookup"><span data-stu-id="3c7e3-162">If you want toorun hello command again, you must also delete hello output directory.</span></span> <span data-ttu-id="3c7e3-163">Använd följande toodelete hello den här katalogen:</span><span class="sxs-lookup"><span data-stu-id="3c7e3-163">Use hello following toodelete this directory:</span></span>
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`


## <a name="next-steps"></a><span data-ttu-id="3c7e3-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c7e3-164">Next steps</span></span>

<span data-ttu-id="3c7e3-165">Nu när du har lärt dig hur toouse Mahout, identifiera andra sätt att arbeta med data i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="3c7e3-165">Now that you have learned how toouse Mahout, discover other ways of working with data on HDInsight:</span></span>

* [<span data-ttu-id="3c7e3-166">Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3c7e3-166">Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="3c7e3-167">Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3c7e3-167">Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="3c7e3-168">MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3c7e3-168">MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
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
