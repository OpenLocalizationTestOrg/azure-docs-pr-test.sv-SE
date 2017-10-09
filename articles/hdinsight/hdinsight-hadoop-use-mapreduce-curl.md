---
title: aaaUse MapReduce och Curl med Hadoop i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur tooremotely kör MapReduce-jobb med Hadoop på HDInsight med Curl."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: bc6daf37-fcdc-467a-a8a8-6fb2f0f773d1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 16920205bacf9699f88090568099e0508a172b3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a>Kör jobb för MapReduce med Hadoop i HDInsight med hjälp av REST

Lär dig hur toouse hello WebHCat REST API toorun MapReduce-jobb på en Hadoop på HDInsight-kluster. CURL är används toodemonstrate hur du kan interagera med HDInsight med hjälp av rådata HTTP-begäranden toorun MapReduce-jobb.

> [!NOTE]
> Om du redan är bekant med Linux-baserade Hadoop-servrar, men du är ny tooHDInsight, se hello [vad du behöver tooknow om Linux-baserade Hadoop på HDInsight](hdinsight-hadoop-linux-information.md) dokumentet.


## <a id="prereq"></a>Förhandskrav

* En Hadoop på HDInsight-kluster
* [CURL](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Kör MapReduce-jobb med Curl

> [!NOTE]
> När du använder Curl eller annan REST-kommunikation med WebHCat, måste du autentisera hello begäranden genom att tillhandahålla hello HDInsight-kluster administratörsanvändarnamn och lösenord. Som en del av hello URI som används toosend hello begäranden toohello server måste du använda hello klusternamnet.
>
> Hello kommandon i det här avsnittet, ersätter **användarnamn** med hello användaren tooauthenticate toohello kluster och **lösenord** med hello lösenordet för användarkontot för hello. Ersätt **KLUSTERNAMN** med hello namnet på klustret.
>
> hello REST API skyddas med hjälp av [grundläggande autentisering](http://en.wikipedia.org/wiki/Basic_access_authentication). Du bör alltid göra begäranden genom att använda HTTPS tooensure att dina autentiseringsuppgifter skickas toohello server på ett säkert sätt.


1. Använd hello efter kommandot tooverify att du kan ansluta tooyour HDInsight-kluster från en kommandorad:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Du bör få ett svar liknande toohello följande JSON:

        {"status":"ok","version":"v1"}

    hello-parametrar som används i det här kommandot är följande:

   * **-u**: Anger hello användarnamn och lösenord används tooauthenticate hello begäran
   * **-G**: Anger att den här åtgärden är en GET-begäran

     Hej i början av hello URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, hello samma för alla begäranden.

2. toosubmit ett MapReduce-jobb, Använd hello följande kommando:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    hello slutet av hello URI (/ mapreduce/jar) anger WebHCat att denna begäran startar ett MapReduce-jobb från en klass i jar-filen. hello-parametrar som används i det här kommandot är följande:

   * **-d**: `-G` inte används så hello begäran standardvärden toohello POST-metoden. `-d`Anger hello datavärden som skickas med hello-begäran.
    * **User.name**: hello-användaren som kör hello kommando
    * **JAR**: hello platsen för hello jar-filen som innehåller klassen toobe kördes
    * **klassen**: hello klass som innehåller hello MapReduce-logik
    * **%d{arg/**: hello argument toobe skickades toohello MapReduce-jobb. I det här fallet indata hello text fil- och hello katalog som används för hello-utdata

     Det här kommandot ska returnera ett jobb-ID som kan använda toocheck hello status för hello jobb:

       {”id”: ”job_1415651640909_0026”}

3. toocheck hello hello jobbets status, Använd hello följande kommando:

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Ersätt hello **JOBID** med hello-värde som returneras i hello föregående steg. Till exempel om hello returnera värdet var `{"id":"job_1415651640909_0026"}`, sedan hello JOBID skulle vara `job_1415651640909_0026`.

    Om hello jobbet är klart hello tillstånd returnerade är `SUCCEEDED`.

   > [!NOTE]
   > Den här Curl-begäran returnerar ett JSON-dokument med information om hello jobb. Jq används tooretrieve hello endast värdet state.

4. När hello tillståndet för hello jobbet har ändrats för`SUCCEEDED`, du kan hämta hello resultatet av hello jobbet från Azure Blob storage. Hej `statusdir` parameter som skickas med hello frågan innehåller hello plats hello utdatafilen. I det här exemplet hello platsen är `/example/curl`. Den här adressen lagrar hello utdata för hello jobb i hello kluster standardlagring på `/example/curl`.

Du kan visa och ladda ned dessa filer med hjälp av hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Mer information om hur du arbetar med blobbar från hello Azure CLI finns hello [Using hello Azure CLI 2.0 med Azure Storage](../storage/common/storage-azure-cli.md#create-and-manage-blobs) dokumentet.

## <a id="nextsteps"></a>Nästa steg

Allmän information om MapReduce-jobb i HDInsight:

* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)
* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)

Mer information om hello REST-gränssnitt som används i den här artikeln finns hello [WebHCat referens](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).
