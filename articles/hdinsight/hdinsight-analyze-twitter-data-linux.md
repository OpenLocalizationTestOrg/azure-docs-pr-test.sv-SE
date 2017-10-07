---
title: aaaAnalyze Twitter-data med Apache Hive - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse använda Hive och Hadoop på HDInsight tootransform TWitter rådata till en sökbar Hive-tabell."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e1e249ed-5f57-40d6-b3bc-a1b4d9a871d3
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 02c4d027c7bbf390ac1c3724c14f8d549ea5195e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-twitter-data-using-hive-and-hadoop-on-hdinsight"></a>Analysera Twitter-data med Hive och Hadoop på HDInsight

Lär dig hur toouse Apache Hive tooprocess Twitter-data. hello resultatet är en lista över Twitter-användare som skickade hello de flesta tweets som innehåller ett visst ord.

> [!IMPORTANT]
> hello stegen i det här dokumentet har testats på HDInsight 3,6.
>
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="get-hello-data"></a>Hämta hello data

Twitter kan du tooretrieve hello [data för varje tweet](https://dev.twitter.com/docs/platform-objects/tweets) som JavaScript Object Notation (JSON) dokument via ett REST-API. [OAuth](http://oauth.net) krävs för autentisering toohello API.

### <a name="create-a-twitter-application"></a>Skapa ett Twitter-program

1. Från en webbläsare, logga in för[https://apps.twitter.com/](https://apps.twitter.com/). Klicka på hello **registrering nu** länk om du inte har ett Twitter-konto.

2. Klicka på **Skapa ny App**.

3. Ange **namn**, **beskrivning**, **webbplats**. Du kan göra upp en URL för hello **webbplats** fältet. hello följande tabell visar några exempel värden toouse:

   | Fält | Värde |
   |:--- |:--- |
   | Namn |MyHDInsightApp |
   | Beskrivning |MyHDInsightApp |
   | Webbplats |http://www.myhdinsightapp.com |

4. Kontrollera **Ja, jag godkänner**, och klicka sedan på **skapa programmet Twitter**.

5. Klicka på hello **behörigheter** är fliken hello standardbehörigheten **skrivskyddad**.

6. Klicka på hello **nycklar och åtkomst-token** fliken.

7. Klicka på **skapa åtkomst-token**.

8. Klicka på **Test OAuth** i hello övre högra hörnet av hello-sidan.

9. Skriv ned **konsumenten nyckeln**, **konsumenthemlighet**, **åtkomsttoken**, och **åtkomst-token hemlighet**.

### <a name="download-tweets"></a>Hämta tweets

hello följande Python-kod hämtar 10 000 tweets från Twitter och spara dem tooa fil med namnet **tweets.txt**.

> [!NOTE]
> hello följande steg utförs på hello HDInsight-kluster, eftersom Python redan är installerad.

1. Anslut toohello HDInsight-kluster med SSH:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

3. Använd hello följande kommandon tooinstall [Tweepy](http://www.tweepy.org/), [Progressbar](https://pypi.python.org/pypi/progressbar/2.2), och andra nödvändiga paket:

   ```bash
   sudo apt install python-dev libffi-dev libssl-dev
   sudo apt remove python-openssl
   pip install virtualenv
   mkdir gettweets
   cd gettweets
   virtualenv gettweets
   source gettweets/bin/activate
   pip install tweepy progressbar pyOpenSSL requests[security]
   ```

4. Använd hello följande kommando toocreate en fil med namnet **gettweets.py**:

   ```bash
   nano gettweets.py
   ```

5. Använd hello följande text som hello hello **gettweets.py** fil:

   ```python
   #!/usr/bin/python

   from tweepy import Stream, OAuthHandler
   from tweepy.streaming import StreamListener
   from progressbar import ProgressBar, Percentage, Bar
   import json
   import sys

   #Twitter app information
   consumer_secret='Your consumer secret'
   consumer_key='Your consumer key'
   access_token='Your access token'
   access_token_secret='Your access token secret'

   #hello number of tweets we want tooget
   max_tweets=10000

   #Create hello listener class that receives and saves tweets
   class listener(StreamListener):
       #On init, set hello counter toozero and create a progress bar
       def __init__(self, api=None):
           self.num_tweets = 0
           self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

       #When data is received, do this
       def on_data(self, data):
           #Append hello tweet toohello 'tweets.txt' file
           with open('tweets.txt', 'a') as tweet_file:
               tweet_file.write(data)
               #Increment hello number of tweets
               self.num_tweets += 1
               #Check toosee if we have hit max_tweets and exit if so
               if self.num_tweets >= max_tweets:
                   self.pbar.finish()
                   sys.exit(0)
               else:
                   #increment hello progress bar
                   self.pbar.update(self.num_tweets)
           return True

       #Handle any errors that may occur
       def on_error(self, status):
           print status

   #Get hello OAuth token
   auth = OAuthHandler(consumer_key, consumer_secret)
   auth.set_access_token(access_token, access_token_secret)
   #Use hello listener class for stream processing
   twitterStream = Stream(auth, listener())
   #Filter for these topics
   twitterStream.filter(track=["azure","cloud","hdinsight"])
   ```

    > [!IMPORTANT]
    > Ersätt hello platshållartext för hello följande objekt med hello information från ditt twitter-program:
    >
    > * `consumer_secret`
    > * `consumer_key`
    > * `access_token`
    > * `access_token_secret`

6. Använd **Ctrl + X**, sedan **Y** toosave hello-filen.

7. Använd följande kommando toorun hello hello och hämta tweets:

    ```bash
    python gettweets.py
    ```

    En förloppsindikator. Räknar upp too100% som hello tweets hämtas.

   > [!NOTE]
   > Om det tar lång tid för hello förlopp fältet tooadvance, bör du ändra hello filter tootrack trender avsnitt. När det finns många tweets om hello-avsnittet i filtret, kan du snabbt få hello 10000 tweets som behövs.

### <a name="upload-hello-data"></a>Ladda upp hello data

tooupload hello tooHDInsight datalagring, Använd hello följande kommandon:

   ```bash
   hdfs dfs -mkdir -p /tutorials/twitter/data
   hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt
```

Dessa kommandon lagra hello data på en plats som har åtkomst till alla noder i klustret hello.

## <a name="run-hello-hiveql-job"></a>Kör jobb för hello HiveQL

1. Använd hello efter kommandot toocreate en fil som innehåller HiveQL-instruktioner:

   ```bash
   nano twitter.hql
   ```

    Använd följande text som hello innehållet i filen hello hello:

   ```hiveql
   set hive.exec.dynamic.partition = true;
   set hive.exec.dynamic.partition.mode = nonstrict;
   -- Drop table, if it exists
   DROP TABLE tweets_raw;
   -- Create it, pointing toward hello tweets logged from Twitter
   CREATE EXTERNAL TABLE tweets_raw (
       json_response STRING
   )
   STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
   -- Drop and recreate hello destination table
   DROP TABLE tweets;
   CREATE TABLE tweets
   (
       id BIGINT,
       created_at STRING,
       created_at_date STRING,
       created_at_year STRING,
       created_at_month STRING,
       created_at_day STRING,
       created_at_time STRING,
       in_reply_to_user_id_str STRING,
       text STRING,
       contributors STRING,
       retweeted STRING,
       truncated STRING,
       coordinates STRING,
       source STRING,
       retweet_count INT,
       url STRING,
       hashtags array<STRING>,
       user_mentions array<STRING>,
       first_hashtag STRING,
       first_user_mention STRING,
       screen_name STRING,
       name STRING,
       followers_count INT,
       listed_count INT,
       friends_count INT,
       lang STRING,
       user_location STRING,
       time_zone STRING,
       profile_image_url STRING,
       json_response STRING
   );
   -- Select tweets from hello imported data, parse hello JSON,
   -- and insert into hello tweets table
   FROM tweets_raw
   INSERT OVERWRITE TABLE tweets
   SELECT
       cast(get_json_object(json_response, '$.id_str') as BIGINT),
       get_json_object(json_response, '$.created_at'),
       concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
       substr (get_json_object(json_response, '$.created_at'),27,4)),
       substr (get_json_object(json_response, '$.created_at'),27,4),
       case substr (get_json_object(json_response,    '$.created_at'),5,3)
           when "Jan" then "01"
           when "Feb" then "02"
           when "Mar" then "03"
           when "Apr" then "04"
           when "May" then "05"
           when "Jun" then "06"
           when "Jul" then "07"
           when "Aug" then "08"
           when "Sep" then "09"
           when "Oct" then "10"
           when "Nov" then "11"
           when "Dec" then "12" end,
       substr (get_json_object(json_response, '$.created_at'),9,2),
       substr (get_json_object(json_response, '$.created_at'),12,8),
       get_json_object(json_response, '$.in_reply_to_user_id_str'),
       get_json_object(json_response, '$.text'),
       get_json_object(json_response, '$.contributors'),
       get_json_object(json_response, '$.retweeted'),
       get_json_object(json_response, '$.truncated'),
       get_json_object(json_response, '$.coordinates'),
       get_json_object(json_response, '$.source'),
       cast (get_json_object(json_response, '$.retweet_count') as INT),
       get_json_object(json_response, '$.entities.display_url'),
       array(
           trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
       array(
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
       trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
       trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
       get_json_object(json_response, '$.user.screen_name'),
       get_json_object(json_response, '$.user.name'),
       cast (get_json_object(json_response, '$.user.followers_count') as INT),
       cast (get_json_object(json_response, '$.user.listed_count') as INT),
       cast (get_json_object(json_response, '$.user.friends_count') as INT),
       get_json_object(json_response, '$.user.lang'),
       get_json_object(json_response, '$.user.location'),
       get_json_object(json_response, '$.user.time_zone'),
       get_json_object(json_response, '$.user.profile_image_url'),
       json_response
   WHERE (length(json_response) > 500);
   ```

2. Tryck på **Ctrl + X**, tryck på **Y** toosave hello-filen.
3. Använd hello efter kommandot toorun hello HiveQL finns i hello-filen:

   ```bash
   beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i twitter.hql
   ```

    Det här kommandot Kör hello hello **twitter.hql** fil. När hello frågan har slutförts kan du se en `jdbc:hive2//localhost:10001/>` prompt.

4. Använd hello följande fråga tooverify att data importerades från hello beeline prompten:

   ```hiveql
   SELECT name, screen_name, count(1) as cc
       FROM tweets
       WHERE text like "%Azure%"
       GROUP BY name,screen_name
       ORDER BY cc DESC LIMIT 10;
   ```

    Den här frågan returnerar högst 10 tweets som innehåller hello word **Azure** i hello meddelandetext.

## <a name="next-steps"></a>Nästa steg

Du har lärt dig hur tootransform en ostrukturerad datauppsättning JSON till en strukturerad Hive-tabell. toolearn mer om Hive i HDInsight, se hello följande dokument:

* [Komma igång med HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Analysera svarta fördröjning data med HDInsight](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
