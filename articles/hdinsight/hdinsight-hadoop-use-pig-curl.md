---
title: "aaaUse Hadoop Pig med övriga i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse REST toorun Pig Latin jobb på en Hadoop-kluster i Azure HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ed5e10d1-4f47-459c-a0d6-7ff967b468c4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 760139e3caad9103d8c9d34e7f548d476014b5ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-rest"></a>Köra Pig med Hadoop på HDInsight med hjälp av REST

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Lär dig hur toorun Pig Latin jobb genom att göra REST begäranden tooan Azure HDInsight-kluster. CURL är används toodemonstrate hur du kan interagera med HDInsight med hello WebHCat REST API.

> [!NOTE]
> Om du redan är bekant med Linux-baserade Hadoop-servrar, men nya tooHDInsight, se [Linux-baserade HDInsight-Tips](hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Förhandskrav

* Ett Azure HDInsight (Hadoop på HDInsight)-kluster (Linux- eller Windows-baserade)

  > [!IMPORTANT]
  > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [CURL](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Köra Pig-jobb med Curl

> [!NOTE]
> hello REST API skyddas via [grundläggande autentisering](http://en.wikipedia.org/wiki/Basic_access_authentication). Alltid göra begäranden genom att använda säker HTTP (HTTPS) tooensure att dina autentiseringsuppgifter skickas toohello server på ett säkert sätt.
>
> När du använder hello kommandon i det här avsnittet, ersätter `USERNAME` med hello användaren tooauthenticate toohello kluster och Ersätt `PASSWORD` med hello lösenordet för användarkontot för hello. Ersätt `CLUSTERNAME` med hello namnet på klustret.
>


1. Använd hello efter kommandot tooverify att du kan ansluta tooyour HDInsight-kluster från en kommandorad:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Du bör få hello följande JSON-svar:

        {"status":"ok","version":"v1"}

    hello-parametrar som används i det här kommandot är följande:

    * **-u**: hello användarnamn och lösenord används tooauthenticate hello begäran
    * **-G**: Anger att begäran är en GET-begäran

     Hej i början av hello URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, hello samma för alla begäranden. hello sökvägen **/status**, visar hello begäran är tooreturn hello status för WebHCat (även kallat Templeton) för hello-servern.

2. Använd följande kod toosubmit ett Pig Latin jobbet toohello kluster hello:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
    ```

    hello-parametrar som används i det här kommandot är följande:

    * **-d**: eftersom `-G` används inte hello begäran standardvärden toohello POST-metoden. `-d`Anger hello datavärden som skickas med hello-begäran.

    * **User.name**: hello-användaren som kör hello kommando
    * **köra**: hello Pig Latin instruktioner tooexecute
    * **statusdir**: hello-katalog som hello status för jobbet skrivs till

    > [!NOTE]
    > Observera att hello blanksteg i Pig Latin rapporterna har ersatts av hello `+` tecken när det används med Curl.

    Det här kommandot ska returnera ett jobb-ID som kan använda toocheck hello status för hello jobb, till exempel:

        {"id":"job_1415651640909_0026"}

3. toocheck hello status för hello jobb, Använd hello följande kommando

     ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

     Ersätt `JOBID` med hello-värde som returneras i hello föregående steg. Till exempel om hello returnera värdet var `{"id":"job_1415651640909_0026"}`, sedan `JOBID` är `job_1415651640909_0026`.

    Om hello jobbet har slutförts, hello tillstånd är **lyckades**.

    > [!NOTE]
    > Den här Curl-begäran returnerar en JavaScript Object Notation (JSON) dokument med information om hello jobb och jq är används tooretrieve hello endast värdet state.

## <a id="results"></a>Visa resultat

När hello tillståndet för hello jobbet har ändrats för**lyckades**, du kan hämta hello resultatet av hello-jobbet. Hej `statusdir` -parameter som överförs med hello frågan innehåller hello platsen hello utdatafilen; i det här fallet `/example/pigcurl`.

HDInsight kan använda Azure Storage eller Azure Data Lake Store som hello standard datalager. Det finns olika sätt tooget vid hello data beroende på vilket som du använder. Mer information finns i avsnittet för hello lagring av hello [Linux-baserat HDInsight information](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) dokumentet.

## <a id="summary"></a>Sammanfattning

Som visas i det här dokumentet, kan du använda en rå HTTP-begäran toorun, övervaka och visa hello resultat av Pig-jobb på ditt HDInsight-kluster.

Mer information om hello REST-gränssnitt som används i den här artikeln finns hello [WebHCat referens](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

## <a id="nextsteps"></a>Nästa steg

Allmän information om Pig på HDInsight:

* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)
