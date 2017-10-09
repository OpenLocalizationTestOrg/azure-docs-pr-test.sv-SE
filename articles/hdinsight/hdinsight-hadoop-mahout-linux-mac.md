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
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight-ssh"></a>Generera filmrekommendationer med hjälp av Apache Mahout med Linux-baserade Hadoop i HDInsight (SSH)

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Lär dig hur toouse hello [Apache Mahout](http://mahout.apache.org) machine learning-biblioteket med Azure HDInsight toogenerate filmrekommendationer.

Mahout är en [maskininlärning] [ ml] -biblioteket för Apache Hadoop. Mahout innehåller algoritmer för bearbetning av data, till exempel filtrering, klassificering, och klustring. I den här artikeln använder du en rekommendation motorn toogenerate filmrekommendationer som baseras på dina vänner har sett filmer.

## <a name="prerequisites"></a>Krav

* Ett Linux-baserat HDInsight-kluster. Information om hur du skapar en finns [komma igång med Linux-baserade Hadoop i HDInsight][getstarted].

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* En SSH-klient. Mer information finns i hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.

## <a name="mahout-versioning"></a>Mahout versionshantering

Mer information om hello-versionen av Mahout i HDInsight finns [HDInsight versioner och Hadoop-komponenter](hdinsight-component-versioning.md).

## <a name="recommendations"></a>Förstå rekommendationer

En av hello-funktioner som tillhandahålls av Mahout är en rekommendation motor. Den här motorn accepterar data i hello-format för `userID`, `itemId`, och `prefValue` (hello inställningar för hello objekt). Mahout kan sedan utföra samtidigt förekomst analysis toodetermine: *användare som har en inställning för ett objekt har också en inställning för dessa andra objekt*. Mahout bestämmer användare med liknande objekt inställningar, vilket kan vara används toomake rekommendationer.

hello är följande arbetsflöde en förenklad exempel som använder filmdata:

* **Samtidigt förekomst**: Joe Alice och Bob alla tyckte *Star krig*, *hello Empire strejker tillbaka*, och *tillbaka hello Jedi*. Mahout anger att användare som någon av dessa filmer också hello andra två.

* **Samtidigt förekomst**: Bob och Alice också tyckte *hello Phantom hot*, *Attack av hello kloner*, och *Revenge av hello Sith*. Mahout anger att användare också tyckte hello föregående tre filmer som dessa tre filmer.

* **Likhet rekommendation**: Joe eftersom tyckte hello tre första filmer, Mahout tittar på filmer som andra med liknande inställningar tyckte om, men Johan har inte bevakade (tyckte/klassificerad). I det här fallet Mahout rekommenderar *hello Phantom hot*, *Attack av hello kloner*, och *Revenge av hello Sith*.

### <a name="understanding-hello-data"></a>Förstå hello data

Enkelt, [GroupLens Research] [ movielens] innehåller klassificering av data för filmer i ett format som är kompatibel med Mahout. Dessa data är tillgängliga på ditt kluster standardlagring på `/HdiSamples/HdiSamples/MahoutMovieData`.

Det finns två filer, `moviedb.txt` och `user-ratings.txt`. hello användaren ratings.txt filen används under analys, medan moviedb.txt används tooprovide användarvänliga textinformation hello resultat från hello analys.

hello data i användaren ratings.txt har en struktur med `userID`, `movieID`, `userRating`, och `timestamp`, som talar om för oss hur mycket en film klassificerats som varje användare. Här är ett exempel på hello data:

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

## <a name="run-hello-analysis"></a>Kör hello analys

Använd hello följande kommando från en SSH-anslutning toohello kluster toorun hello rekommendation jobb:

```bash
mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
```

> [!NOTE]
> hello jobbet kan ta flera minuter toocomplete och kan köra flera MapReduce-jobb.

## <a name="view-hello-output"></a>Visa hello utdata

1. När hello jobbet är slutfört, använder du hello följande kommandoutdata tooview hello genereras:

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    hello utdata visas på följande sätt:

        1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    hello första kolumnen är hello `userID`. Hej värden i ' [' och ']' är `movieId`:`recommendationScore`.

2. Du kan använda hello utdata tillsammans med hello moviedb.txt, tooprovide mer information på hello rekommendationer. Först måste vi toocopy hello filer lokalt med hjälp av hello följande kommandon:

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    Det här kommandot kopior hello tooa utdatafilen med namnet **recommendations.txt** i hello aktuella katalogen, tillsammans med hello film datafiler.

3. Använd följande kommando toocreate Python-skriptet som slår upp film namn för hello data i hello rekommendationer utdata hello:

    ```bash
    nano show_recommendations.py
    ```

    Använd följande text som hello innehållet i filen hello hello när hello redigeraren öppnas och:

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

    Tryck på **Ctrl + X**, **Y**, och slutligen **RETUR** toosave hello data.

4. Kör hello Python-skriptet. hello förutsätter följande kommando att du är i hello directory där alla hello-filer har hämtats:

    ```bash
    python show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    Detta kommando kontrollerar hello rekommendationer som genererats för användaren ID 4.

    * Hej **användare ratings.txt** filen har använt tooretrieve filmer som har bedömts.

    * Hej **moviedb.txt** filen har använt tooretrieve hello namnen på hello filmer.

    * Hej **recommendations.txt** är används tooretrieve hello filmrekommendationer för den här användaren.

     hello utdata från det här kommandot är liknande toohello, som följande text:

        Sju år i Tibet (1997), poängsätta = 5.0 Indiana brev och hello senaste Crusade (1989) poängsätta = 5.0 Jaws (1975) poäng = 5.0 mening och känseln (1995) poäng = 5.0 oberoende dag (ID4) (1996), poäng = 5.0 min bästa vän Wedding (1997) poängsätta = 5.0 Jerry Maguire (1996 ), poäng = 5.0 Scream 2 (1997), poäng = 5.0 tid tooKill, (1996), poängsätta = 5.0

## <a name="delete-temporary-data"></a>Ta bort temporära data

Mahout jobb ta inte bort tillfälliga data som har skapats under bearbetning av hello jobb. Hej `--tempDir` parameter har angetts i hello exempel jobbet tooisolate hello temporära filer i en specifik sökväg för enkelt borttagning. tooremove hello temporära filer, Använd hello följande kommando:

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]
> Om du vill toorun hello kommandot igen, måste du också ta bort hello målkatalogen. Använd följande toodelete hello den här katalogen:
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`


## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur toouse Mahout, identifiera andra sätt att arbeta med data i HDInsight:

* [Hive med HDInsight](hdinsight-use-hive.md)
* [Pig med HDInsight](hdinsight-use-pig.md)
* [MapReduce med HDInsight](hdinsight-use-mapreduce.md)

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
