---
title: aaaUse Hadoop Sqoop med Curl i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur tooremotely skicka Sqoop jobb tooHDInsight med Curl."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 39798321-78ca-428c-bcfe-322e49af4059
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: d9c09a6704ab6c5f48be50ed6d6314ec406df8ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Kör jobb för Sqoop med Hadoop i HDInsight med Curl
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Lär dig hur toouse Curl toorun Sqoop jobb på en Hadoop-kluster i HDInsight.

CURL är används toodemonstrate hur du kan interagera med HDInsight med hjälp av rådata toorun för HTTP-begäranden, Övervakare och hämta hello resultaten av Sqoop jobb. Det här fungerar med hjälp av hello WebHCat REST-API (kallades tidigare Templeton) tillhandahålls av HDInsight-kluster.

> [!NOTE]
> Om du redan är bekant med Linux-baserade Hadoop-servrar, men nya tooHDInsight, se [Information om hur du använder HDInsight på Linux](hdinsight-hadoop-linux-information.md).
> 
> 

## <a name="prerequisites"></a>Krav
toocomplete hello stegen i den här artikeln, behöver du hello följande:

* En Hadoop på HDInsight-kluster (Linux och Windows-baserade)
* [CURL](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a name="submit-sqoop-jobs-by-using-curl"></a>Skicka Sqoop jobb med Curl
> [!NOTE]
> När du använder Curl eller annan REST-kommunikation med WebHCat, måste du autentisera hello begäranden genom att tillhandahålla hello användarnamn och lösenord för hello HDInsight-klustrets administratör. Du måste också använda hello klustrets namn som en del av hello identifierare URI (Uniform Resource) används toosend hello begäranden toohello server.
> 
> Hello kommandon i det här avsnittet, ersätter **användarnamn** med hello användaren tooauthenticate toohello kluster och Ersätt **lösenord** med hello lösenordet för användarkontot för hello. Ersätt **KLUSTERNAMN** med hello namnet på klustret.
> 
> hello REST API skyddas via [grundläggande autentisering](http://en.wikipedia.org/wiki/Basic_access_authentication). Du bör alltid göra begäranden genom att använda säker HTTP (HTTPS) toohelp se till att dina autentiseringsuppgifter skickas toohello server på ett säkert sätt.
> 
> 

1. Använd hello efter kommandot tooverify att du kan ansluta tooyour HDInsight-kluster från en kommandorad:
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    Du bör få ett svar liknande toohello följande:
   
        {"status":"ok","version":"v1"}
   
    hello-parametrar som används i det här kommandot är följande:
   
   * **-u** -hello användarnamn och lösenord används tooauthenticate hello begäran.
   * **-G** – Anger att detta är en GET-begäran.
     
     Hej i början av hello URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, kommer att hello samma för alla begäranden. hello sökvägen **/status**, visar hello begäran är tooreturn WebHCat (även kallat Templeton) statusen för hello-servern. 
2. Använd hello följande toosubmit ett sqoop jobb:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    hello-parametrar som används i det här kommandot är följande:

    * **-d** - sedan `-G` används inte hello begäran standardvärden toohello POST-metoden. `-d`Anger hello datavärden som skickas med hello-begäran.

        * **User.name** -hello-användaren som kör hello-kommando.

        * **kommandot** -hello Sqoop kommandot tooexecute.

        * **statusdir** -hello-katalog som hello status för jobbet kommer att skrivas till.

    Det här kommandot ska returnera ett jobb-ID som kan använda toocheck hello status för hello jobb.

        {"id":"job_1415651640909_0026"}

1. toocheck hello hello jobbets status, Använd hello följande kommando. Ersätt **JOBID** med hello-värde som returneras i hello föregående steg. Till exempel om hello returnera värdet var `{"id":"job_1415651640909_0026"}`, sedan **JOBID** skulle vara `job_1415651640909_0026`.
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    Om hello jobbet har slutförts, hello tillstånd blir **lyckades**.
   
   > [!NOTE]
   > Den här Curl-begäran returnerar ett JavaScript Object Notation (JSON) dokument med information om hello jobb. jq används tooretrieve hello endast värdet state.
   > 
   > 
2. När hello tillståndet för hello jobbet har ändrats för**lyckades**, du kan hämta hello resultatet av hello jobbet från Azure Blob storage. Hej `statusdir` -parameter som överförs med hello frågan innehåller hello platsen hello utdatafilen; i det här fallet **wasb: / / / exempel/curl**. Den här adressen lagrar hello utdata för hello jobb i hello **exempel/curl** på hello standardbehållare för lagring används av ditt HDInsight-kluster.
   
    Du kan visa och ladda ned dessa filer med hjälp av hello [Azure CLI](../cli-install-nodejs.md). Till exempel toolist filer i **exempel/curl**, använda hello följande kommando:
   
        azure storage blob list <container-name> example/curl
   
    toodownload en fil, använder hello följande:
   
        azure storage blob download <container-name> <blob-name> <destination-file>
   
   > [!NOTE]
   > Du måste antingen ange hello lagringskontonamnet som innehåller hello blob med hjälp av hello `-a` och `-k` parametrar eller ange hello **AZURE\_lagring\_konto** och **AZURE\_lagring\_åtkomst\_NYCKELN** miljövariabler. Se < en href = ”hdinsight-överför-data.md” target = ”_blank” mer information.
   > 
   > 

## <a name="limitations"></a>Begränsningar
* Masskapat export - med Linux-baserat HDInsight, hello Sqoop koppling som används tooexport data tooMicrosoft SQL Server eller Azure SQL Database stöder för närvarande inte bulkinfogningar.
* Batchbearbetning - med Linux-baserat HDInsight när du använder hello `-batch` växeln när du utför infogningar, Sqoop utför flera infogningar i stället för batchbearbetning hello infogningsåtgärder.

## <a name="summary"></a>Sammanfattning
Som visas i det här dokumentet, kan du använda en rå HTTP-begäran toorun, övervaka och visa hello resultat av Sqoop jobb på ditt HDInsight-kluster.

Mer information om hello REST-gränssnitt som används i den här artikeln finns hello <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API-guiden</a>.

## <a name="next-steps"></a>Nästa steg
Allmän information om Hive med HDInsight:

* [Använda Sqoop med Hadoop i HDInsight](hdinsight-use-sqoop.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)
* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)

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


