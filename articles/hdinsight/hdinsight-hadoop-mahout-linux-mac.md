---
title: "Skapa rekommendationer med hjälp av Mahout och HDInsight (SSH) - Azure | Microsoft Docs"
description: "Lär dig hur du använder Apache Mahout-machine learning-biblioteket för att generera filmrekommendationer med HDInsight (Hadoop)."
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
ms.openlocfilehash: 28450d72f19a5467d88bc787d11f6c37c5afbf9a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight-ssh"></a><span data-ttu-id="608dc-103">Generera filmrekommendationer med hjälp av Apache Mahout med Linux-baserade Hadoop i HDInsight (SSH)</span><span class="sxs-lookup"><span data-stu-id="608dc-103">Generate movie recommendations by using Apache Mahout with Linux-based Hadoop in HDInsight (SSH)</span></span>

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

<span data-ttu-id="608dc-104">Lär dig hur du använder den [Apache Mahout](http://mahout.apache.org) machine learning-biblioteket med Azure HDInsight för att generera filmrekommendationer.</span><span class="sxs-lookup"><span data-stu-id="608dc-104">Learn how to use the [Apache Mahout](http://mahout.apache.org) machine learning library with Azure HDInsight to generate movie recommendations.</span></span>

<span data-ttu-id="608dc-105">Mahout är en [maskininlärning] [ ml] -biblioteket för Apache Hadoop.</span><span class="sxs-lookup"><span data-stu-id="608dc-105">Mahout is a [machine learning][ml] library for Apache Hadoop.</span></span> <span data-ttu-id="608dc-106">Mahout innehåller algoritmer för bearbetning av data, till exempel filtrering, klassificering, och klustring.</span><span class="sxs-lookup"><span data-stu-id="608dc-106">Mahout contains algorithms for processing data, such as filtering, classification, and clustering.</span></span> <span data-ttu-id="608dc-107">I den här artikeln använder du en rekommendation motor för att generera filmrekommendationer som baseras på dina vänner har sett filmer.</span><span class="sxs-lookup"><span data-stu-id="608dc-107">In this article, you use a recommendation engine to generate movie recommendations that are based on movies your friends have seen.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="608dc-108">Krav</span><span class="sxs-lookup"><span data-stu-id="608dc-108">Prerequisites</span></span>

* <span data-ttu-id="608dc-109">Ett Linux-baserat HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="608dc-109">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="608dc-110">Information om hur du skapar en finns [komma igång med Linux-baserade Hadoop i HDInsight][getstarted].</span><span class="sxs-lookup"><span data-stu-id="608dc-110">For information about creating one, see [Get started using Linux-based Hadoop in HDInsight][getstarted].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="608dc-111">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="608dc-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="608dc-112">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="608dc-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="608dc-113">En SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="608dc-113">An SSH client.</span></span> <span data-ttu-id="608dc-114">Mer information finns i dokumentet [Använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="608dc-114">For more information, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

## <a name="mahout-versioning"></a><span data-ttu-id="608dc-115">Mahout versionshantering</span><span class="sxs-lookup"><span data-stu-id="608dc-115">Mahout versioning</span></span>

<span data-ttu-id="608dc-116">Mer information om versionen av Mahout i HDInsight finns [HDInsight versioner och Hadoop-komponenter](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="608dc-116">For more information about the version of Mahout in HDInsight, see [HDInsight versions and Hadoop components](hdinsight-component-versioning.md).</span></span>

## <span data-ttu-id="608dc-117"><a name="recommendations"></a>Förstå rekommendationer</span><span class="sxs-lookup"><span data-stu-id="608dc-117"><a name="recommendations"></a>Understanding recommendations</span></span>

<span data-ttu-id="608dc-118">En av de funktioner som tillhandahålls av Mahout är en rekommendation motor.</span><span class="sxs-lookup"><span data-stu-id="608dc-118">One of the functions that is provided by Mahout is a recommendation engine.</span></span> <span data-ttu-id="608dc-119">Den här motorn accepterar data i formatet `userID`, `itemId`, och `prefValue` (inställningar för artikeln).</span><span class="sxs-lookup"><span data-stu-id="608dc-119">This engine accepts data in the format of `userID`, `itemId`, and `prefValue` (the preference for the item).</span></span> <span data-ttu-id="608dc-120">Mahout kan sedan utföra samtidigt förekomst analysen för att avgöra: *användare som har en inställning för ett objekt har också en inställning för dessa andra objekt*.</span><span class="sxs-lookup"><span data-stu-id="608dc-120">Mahout can then perform co-occurance analysis to determine: *users who have a preference for an item also have a preference for these other items*.</span></span> <span data-ttu-id="608dc-121">Mahout bestämmer användare med liknande objekt inställningar som kan användas för att ge rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="608dc-121">Mahout then determines users with like-item preferences, which can be used to make recommendations.</span></span>

<span data-ttu-id="608dc-122">Följande arbetsflöde är en förenklad exempel som använder filmdata:</span><span class="sxs-lookup"><span data-stu-id="608dc-122">The following workflow is a simplified example that uses movie data:</span></span>

* <span data-ttu-id="608dc-123">**Samtidigt förekomst**: Joe Alice och Bob alla tyckte *Star krig*, *i Empire strejker tillbaka*, och *tillbaka Jedi*.</span><span class="sxs-lookup"><span data-stu-id="608dc-123">**Co-occurance**: Joe, Alice, and Bob all liked *Star Wars*, *The Empire Strikes Back*, and *Return of the Jedi*.</span></span> <span data-ttu-id="608dc-124">Mahout anger att användare också som en av dessa filmer som de andra två.</span><span class="sxs-lookup"><span data-stu-id="608dc-124">Mahout determines that users who like any one of these movies also like the other two.</span></span>

* <span data-ttu-id="608dc-125">**Samtidigt förekomst**: Bob och Alice också tyckte *i Phantom hot*, *Attack av klonerna*, och *Revenge av Sith*.</span><span class="sxs-lookup"><span data-stu-id="608dc-125">**Co-occurance**: Bob and Alice also liked *The Phantom Menace*, *Attack of the Clones*, and *Revenge of the Sith*.</span></span> <span data-ttu-id="608dc-126">Mahout anger att användare som även gillade föregående tre filmer som dessa tre filmer.</span><span class="sxs-lookup"><span data-stu-id="608dc-126">Mahout determines that users who liked the previous three movies also like these three movies.</span></span>

* <span data-ttu-id="608dc-127">**Likhet rekommendation**: Joe eftersom tyckte om de första tre filmerna, Mahout tittar på filmer som andra med liknande inställningar tyckte om, men Johan har inte bevakade (tyckte/klassificerad).</span><span class="sxs-lookup"><span data-stu-id="608dc-127">**Similarity recommendation**: Because Joe liked the first three movies, Mahout looks at movies that others with similar preferences liked, but Joe has not watched (liked/rated).</span></span> <span data-ttu-id="608dc-128">I det här fallet Mahout rekommenderar *i Phantom hot*, *Attack av klonerna*, och *Revenge av Sith*.</span><span class="sxs-lookup"><span data-stu-id="608dc-128">In this case, Mahout recommends *The Phantom Menace*, *Attack of the Clones*, and *Revenge of the Sith*.</span></span>

### <a name="understanding-the-data"></a><span data-ttu-id="608dc-129">Förstå data</span><span class="sxs-lookup"><span data-stu-id="608dc-129">Understanding the data</span></span>

<span data-ttu-id="608dc-130">Enkelt, [GroupLens Research] [ movielens] innehåller klassificering av data för filmer i ett format som är kompatibel med Mahout.</span><span class="sxs-lookup"><span data-stu-id="608dc-130">Conveniently, [GroupLens Research][movielens] provides rating data for movies in a format that is compatible with Mahout.</span></span> <span data-ttu-id="608dc-131">Dessa data är tillgängliga på ditt kluster standardlagring på `/HdiSamples/HdiSamples/MahoutMovieData`.</span><span class="sxs-lookup"><span data-stu-id="608dc-131">This data is available on your cluster's default storage at `/HdiSamples/HdiSamples/MahoutMovieData`.</span></span>

<span data-ttu-id="608dc-132">Det finns två filer, `moviedb.txt` och `user-ratings.txt`.</span><span class="sxs-lookup"><span data-stu-id="608dc-132">There are two files, `moviedb.txt` and `user-ratings.txt`.</span></span> <span data-ttu-id="608dc-133">Användaren ratings.txt filen används under analys, medan moviedb.txt används för att tillhandahålla användarvänliga textinformation när du visar resultatet av analysen.</span><span class="sxs-lookup"><span data-stu-id="608dc-133">The user-ratings.txt file is used during analysis, while moviedb.txt is used to provide user-friendly text information when displaying the results of the analysis.</span></span>

<span data-ttu-id="608dc-134">Data i ratings.txt användare har en struktur med `userID`, `movieID`, `userRating`, och `timestamp`, som talar om för oss hur mycket en film klassificerats som varje användare.</span><span class="sxs-lookup"><span data-stu-id="608dc-134">The data contained in user-ratings.txt has a structure of `userID`, `movieID`, `userRating`, and `timestamp`, which tells us how highly each user rated a movie.</span></span> <span data-ttu-id="608dc-135">Här är ett exempel på data:</span><span class="sxs-lookup"><span data-stu-id="608dc-135">Here is an example of the data:</span></span>

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

## <a name="run-the-analysis"></a><span data-ttu-id="608dc-136">Analys körs</span><span class="sxs-lookup"><span data-stu-id="608dc-136">Run the analysis</span></span>

<span data-ttu-id="608dc-137">Från en SSH-anslutning till klustret använder du följande kommando för att köra jobbet rekommendation:</span><span class="sxs-lookup"><span data-stu-id="608dc-137">From an SSH connection to the cluster, use the following command to run the recommendation job:</span></span>

```bash
mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
```

> [!NOTE]
> <span data-ttu-id="608dc-138">Jobbet kan ta flera minuter att slutföra, och kan köra flera MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="608dc-138">The job may take several minutes to complete, and may run multiple MapReduce jobs.</span></span>

## <a name="view-the-output"></a><span data-ttu-id="608dc-139">Visa utdata</span><span class="sxs-lookup"><span data-stu-id="608dc-139">View the output</span></span>

1. <span data-ttu-id="608dc-140">När jobbet har slutförts använder du följande kommando visa genererade utdata:</span><span class="sxs-lookup"><span data-stu-id="608dc-140">Once the job completes, use the following command to view the generated output:</span></span>

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    <span data-ttu-id="608dc-141">Utdata visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="608dc-141">The output appears as follows:</span></span>

        1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    <span data-ttu-id="608dc-142">Den första kolumnen är den `userID`.</span><span class="sxs-lookup"><span data-stu-id="608dc-142">The first column is the `userID`.</span></span> <span data-ttu-id="608dc-143">Värdena i ' [' och ']' är `movieId`:`recommendationScore`.</span><span class="sxs-lookup"><span data-stu-id="608dc-143">The values contained in '[' and ']' are `movieId`:`recommendationScore`.</span></span>

2. <span data-ttu-id="608dc-144">Du kan använda utdata, tillsammans med moviedb.txt, för att ge mer information om rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="608dc-144">You can use the output, along with the moviedb.txt, to provide more information on the recommendations.</span></span> <span data-ttu-id="608dc-145">Vi måste först kopiera filerna lokalt med hjälp av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="608dc-145">First, we need to copy the files locally using the following commands:</span></span>

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    <span data-ttu-id="608dc-146">Det här kommandot kopierar utdata till en fil med namnet **recommendations.txt** i den aktuella katalogen, tillsammans med film datafiler.</span><span class="sxs-lookup"><span data-stu-id="608dc-146">This command copies the output data to a file named **recommendations.txt** in the current directory, along with the movie data files.</span></span>

3. <span data-ttu-id="608dc-147">Använder du följande kommando för att skapa en Python-skriptet som slår upp film namn för data i rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="608dc-147">Use the following command to create a Python script that looks up movie names for the data in the recommendations output:</span></span>

    ```bash
    nano show_recommendations.py
    ```

    <span data-ttu-id="608dc-148">När det öppnas redigeraren, Använd följande text som innehållet i filen:</span><span class="sxs-lookup"><span data-stu-id="608dc-148">When the editor opens, use the following text as the contents of the file:</span></span>

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

    <span data-ttu-id="608dc-149">Tryck på **Ctrl + X**, **Y**, och slutligen **RETUR** att spara data.</span><span class="sxs-lookup"><span data-stu-id="608dc-149">Press **Ctrl-X**, **Y**, and finally **Enter** to save the data.</span></span>

4. <span data-ttu-id="608dc-150">Kör skriptet Python.</span><span class="sxs-lookup"><span data-stu-id="608dc-150">Run the Python script.</span></span> <span data-ttu-id="608dc-151">Följande kommando förutsätter att du är i katalogen där alla filer som hämtades:</span><span class="sxs-lookup"><span data-stu-id="608dc-151">The following command assumes you are in the directory where all the files were downloaded:</span></span>

    ```bash
    python show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    <span data-ttu-id="608dc-152">Detta kommando kontrollerar de rekommendationer som genererats för användaren ID 4.</span><span class="sxs-lookup"><span data-stu-id="608dc-152">This command looks at the recommendations generated for user ID 4.</span></span>

    * <span data-ttu-id="608dc-153">Den **användare ratings.txt** används för att hämta filmer som har bedömts.</span><span class="sxs-lookup"><span data-stu-id="608dc-153">The **user-ratings.txt** file is used to retrieve movies that have been rated.</span></span>

    * <span data-ttu-id="608dc-154">Den **moviedb.txt** används för att hämta namnen på filmer.</span><span class="sxs-lookup"><span data-stu-id="608dc-154">The **moviedb.txt** file is used to retrieve the names of the movies.</span></span>

    * <span data-ttu-id="608dc-155">Den **recommendations.txt** används för att hämta filmrekommendationer för den här användaren.</span><span class="sxs-lookup"><span data-stu-id="608dc-155">The **recommendations.txt** is used to retrieve the movie recommendations for this user.</span></span>

     <span data-ttu-id="608dc-156">Utdata från kommandot liknar följande:</span><span class="sxs-lookup"><span data-stu-id="608dc-156">The output from this command is similar to the following text:</span></span>

        <span data-ttu-id="608dc-157">Sju år i Tibet (1997), poängsätta = 5.0 Indiana brev och den senaste Crusade (1989) poängsätta = 5.0 Jaws (1975) poäng = 5.0 mening och känseln (1995) poäng = 5.0 oberoende dag (ID4) (1996), poäng = 5.0 min bästa vän Wedding (1997) poängsätta = 5.0 Jerry Maguire (1996) poäng = 5.0 Scream 2 (1997), poäng = 5.0 tid att Kill, (1996), poängsätta = 5.0</span><span class="sxs-lookup"><span data-stu-id="608dc-157">Seven Years in Tibet (1997), score=5.0   Indiana Jones and the Last Crusade (1989), score=5.0   Jaws (1975), score=5.0   Sense and Sensibility (1995), score=5.0   Independence Day (ID4) (1996), score=5.0   My Best Friend's Wedding (1997), score=5.0   Jerry Maguire (1996), score=5.0   Scream 2 (1997), score=5.0   Time to Kill, A (1996), score=5.0</span></span>

## <a name="delete-temporary-data"></a><span data-ttu-id="608dc-158">Ta bort temporära data</span><span class="sxs-lookup"><span data-stu-id="608dc-158">Delete temporary data</span></span>

<span data-ttu-id="608dc-159">Mahout jobb ta inte bort tillfälliga data som har skapats under bearbetning av jobbet.</span><span class="sxs-lookup"><span data-stu-id="608dc-159">Mahout jobs do not remove temporary data that is created while processing the job.</span></span> <span data-ttu-id="608dc-160">Den `--tempDir` parameter har angetts i exempel jobbet att isolera de temporära filerna i en specifik sökväg för enkelt borttagning.</span><span class="sxs-lookup"><span data-stu-id="608dc-160">The `--tempDir` parameter is specified in the example job to isolate the temporary files into a specific path for easy deletion.</span></span> <span data-ttu-id="608dc-161">Om du vill ta bort temporära filer, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="608dc-161">To remove the temp files, use the following command:</span></span>

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]
> <span data-ttu-id="608dc-162">Om du vill köra kommandot igen måste du också ta bort den angivna katalogen.</span><span class="sxs-lookup"><span data-stu-id="608dc-162">If you want to run the command again, you must also delete the output directory.</span></span> <span data-ttu-id="608dc-163">Använd följande för att ta bort den här katalogen:</span><span class="sxs-lookup"><span data-stu-id="608dc-163">Use the following to delete this directory:</span></span>
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`


## <a name="next-steps"></a><span data-ttu-id="608dc-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="608dc-164">Next steps</span></span>

<span data-ttu-id="608dc-165">Nu när du har lärt dig hur du använder Mahout, identifiera andra sätt att arbeta med data i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="608dc-165">Now that you have learned how to use Mahout, discover other ways of working with data on HDInsight:</span></span>

* [<span data-ttu-id="608dc-166">Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="608dc-166">Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="608dc-167">Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="608dc-167">Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="608dc-168">MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="608dc-168">MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

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
