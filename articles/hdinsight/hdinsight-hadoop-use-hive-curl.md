---
title: aaaUse Hadoop Hive med Curl i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur tooremotely skicka Pig-jobb med Curl tooHDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: e725829ad2adcf3540f44375e3e87b7cdaebd15e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a>Köra Hive-frågor med Hadoop i HDInsight med hjälp av REST

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Lär dig hur toouse hello WebHCat REST API toorun Hive-frågor med Hadoop på Azure HDInsight-kluster.

[CURL](http://curl.haxx.se/) är används toodemonstrate hur du kan interagera med HDInsight med hjälp av rådata HTTP-begäranden. Hej [jq](http://stedolan.github.io/jq/) -verktyget är används tooprocess hello JSON-data som returneras från REST-begäranden.

> [!NOTE]
> Om du redan är bekant med Linux-baserade Hadoop-servrar, men nya tooHDInsight, se hello [vad du behöver tooknow om Hadoop i Linux-baserat HDInsight](hdinsight-hadoop-linux-information.md) dokumentet.

## <a id="curl"></a>Köra Hive-frågor

> [!NOTE]
> När du använder cURL eller annan REST-kommunikation med WebHCat, måste du autentisera hello begäranden genom att tillhandahålla hello användarnamn och lösenord för hello HDInsight-klustrets administratör.
>
> Hello kommandon i det här avsnittet, ersätter **användarnamn** med hello användaren tooauthenticate toohello kluster och Ersätt **lösenord** med hello lösenordet för användarkontot för hello. Ersätt **KLUSTERNAMN** med hello namnet på klustret.
>
> hello REST API skyddas via [grundläggande autentisering](http://en.wikipedia.org/wiki/Basic_access_authentication). toohelp se till att dina autentiseringsuppgifter på ett säkert sätt skickas toohello server alltid göra begäranden genom att använda säker HTTP (HTTPS).

1. Använd hello efter kommandot tooverify att du kan ansluta tooyour HDInsight-kluster från en kommandorad:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Du får ett svar liknande toohello följande text:

        {"status":"ok","version":"v1"}

    hello-parametrar som används i det här kommandot är följande:

   * **-u** -hello användarnamn och lösenord används tooauthenticate hello begäran.
   * **-G** -anger att begäran är en GET-åtgärd.

     Hej i början av hello URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, hello samma för alla begäranden. hello sökvägen **/status**, visar hello begäran är tooreturn WebHCat (även kallat Templeton) statusen för hello-servern. Du kan också begära hello version av Hive med hjälp av hello följande kommando:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

     Denna begäran returnerar ett svar liknande toohello följande text:

       {”modulen”: ”hive”, ”version”: ”0.13.0.2.1.6.0-2103”}

2. Använd hello följande toocreate en tabell med namnet **log4jLogs**:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    hello följande parametrar som används med denna begäran:

   * **-d** - sedan `-G` används inte hello begäran standardvärden toohello POST-metoden. `-d`Anger hello datavärden som skickas med hello-begäran.

     * **User.name** -hello-användaren som kör hello-kommando.
     * **köra** -hello HiveQL-instruktioner tooexecute.
     * **statusdir** -hello-katalog som hello status för jobbet ska skrivas till.

     Dessa instruktioner utför hello följande åtgärder:
   * **DROP TABLE** -hello tabellen redan raderas.
   * **Skapa extern tabell** -skapar en ny ”externa” tabell i Hive. Externa tabeller lagra endast hello tabelldefinition i Hive. hello data finns kvar i hello ursprungliga plats.

     > [!NOTE]
     > Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa. Till exempel en automatisk överföring av data eller en annan MapReduce-åtgärd.
     >
     > Släppa en extern tabell har **inte** ta bort data hello, bara hello tabelldefinitionen.

   * **RADEN FORMAT** – hur hello data formateras. hello fälten i varje logg avgränsas med ett blanksteg.
   * **LAGRAS AS TEXTFILE plats** - när hello data lagras (hello exempel/datakatalog) och att den lagras som text.
   * **Välj** -väljer en uppräkning av alla rader där kolumnen **t4** innehåller hello värdet **[fel]**. Den här instruktionen returnerar ett värde för **3** som det finns tre rader som innehåller det här värdet.

     > [!NOTE]
     > Observera att hello blanksteg mellan HiveQL-instruktioner har ersatts av hello `+` tecken när det används med Curl. Citattecken värden som innehåller blanksteg, till exempel hello avgränsaren, bör inte ersättas av `+`.

   * **INPUT__FILE__NAME som '% 25.log'** – den här instruktionen gränser hello Sök tooonly filer slutar på. log.

     > [!NOTE]
     > Hej `%25` är hello URL-kodade formatet %, så hello faktiska villkoret är `like '%.log'`. Hej % har toobe URL-kodade, eftersom den behandlas som ett specialtecken i URL: er.

     Det här kommandot ska returnera ett jobb-ID som kan använda toocheck hello status för hello jobb.

       {”id”: ”job_1415651640909_0026”}

3. toocheck hello hello jobbets status, Använd hello följande kommando:

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Ersätt **JOBID** med hello-värde som returneras i hello föregående steg. Till exempel om hello returnera värdet var `{"id":"job_1415651640909_0026"}`, sedan **JOBID** skulle vara `job_1415651640909_0026`.

    Om hello jobbet har slutförts, hello tillstånd är **lyckades**.

   > [!NOTE]
   > Den här Curl-begäran returnerar ett JavaScript Object Notation (JSON) dokument med information om hello jobb. Jq används tooretrieve hello endast värdet state.

4. När hello tillståndet för hello jobbet har ändrats för**lyckades**, du kan hämta hello resultatet av hello jobbet från Azure Blob storage. Hej `statusdir` -parameter som överförs med hello frågan innehåller hello platsen hello utdatafilen; i det här fallet **-exempel curl**. Den här adressen lagrar hello utdata i hello **exempel/curl** katalog i hello kluster standardlagring.

    Du kan visa och ladda ned dessa filer med hjälp av hello [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli). Mer information om hur du använder hello Azure CLI med Azure Storage finns hello [Använd Azure CLI 2.0 med Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli#create-and-manage-blobs) dokumentet.

5. Använd hello följande instruktioner toocreate en ny ”interna” tabell med namnet **errorLogs**:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    Dessa instruktioner utför hello följande åtgärder:

   * **Skapa tabell om inte finns** -skapar en tabell om den inte redan finns. Den här instruktionen skapar en intern tabell som lagras i datalagret för hello Hive och hanteras helt av Hive.

     > [!NOTE]
     > Till skillnad från externa tabeller kan släppa en intern tabell tar bort hello samt underliggande data.

   * **LAGRAS AS ORC** -lagrar hello data i optimerad raden kolumner (ORC)-format. ORC är ett mycket optimerad och effektiv format för att lagra data med Hive.
   * **INFOGA ÖVER... Välj** -väljer rader från hello **log4jLogs** tabellen som innehåller **[fel]**, och sedan infogningar hello data i hello **errorLogs** tabell.
   * **Välj** -markeras alla rader från hello nya **errorLogs** tabell.

6. Använd hello jobb-ID som returnerades toocheck hello status för hello jobb. Använd hello Azure CLI som beskrivits ovan toodownload och visa hello resultat när den har slutförts. hello utdata ska innehålla tre rader, som alla innehåller **[fel]**.

## <a id="nextsteps"></a>Nästa steg

Allmän information om Hive med HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)

Om du använder Tez med Hive finns i följande dokument för felsökningsinformation hello:

* [Använd hello Ambari Tez vy på Linux-baserat HDInsight](hdinsight-debug-ambari-tez-view.md)

Mer information om hello REST-API som används i det här dokumentet finns hello [WebHCat referens](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) dokumentet.

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


