---
title: aaaUse Livius Spark toosubmit jobb tooSpark-kluster i Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Apache Spark REST API toosubmit Spark jobb via fjärranslutning tooan Azure HDInsight-kluster."
keywords: Apache spark rest-api, Livius spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2817b779-1594-486b-8759-489379ca907d
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: nitinme
ms.openlocfilehash: 417213b5f1db1522373188002fe05117faea5243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-spark-rest-api-toosubmit-remote-jobs-tooan-hdinsight-spark-cluster"></a>Använda Apache Spark REST API toosubmit remote jobb tooan HDInsight Spark-kluster

Lär dig hur toouse Livius hello Apache Spark REST-API som används toosubmit remote jobb tooan Azure HDInsight Spark-kluster. Mer detaljerad dokumentation finns [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).

Du kan använda Livius toorun interaktiva Spark tankar eller skicka batch jobb toobe körs på Spark. Den här artikeln handlar om med hjälp av Livius toosubmit batchjobb. hello kodavsnitt i den här artikeln använder cURL toomake REST API-anrop toohello Livius Spark-slutpunkt.

**Krav:**

* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

* [cURL](http://curl.haxx.se/). Den här artikeln använder cURL toodemonstrate hur toomake REST API-anrop mot ett HDInsight Spark-kluster.

## <a name="submit-a-livy-spark-batch-job"></a>Skicka ett Livius Spark batchjobb
Du måste överföra hello programmet jar på hello klusterlagringen som associerats med hello kluster innan du skickar ett batchjobb. Du kan använda [ **AzCopy**](../storage/common/storage-use-azcopy.md), ett kommandoradsverktyg, toodo så. Det finns olika klienter du kan använda tooupload data. Du kan hitta mer om dem i [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path tooapplication jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Exempel**:

* Om hello jar-filen finns på hello klustret storage (WASB)
  
        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"
* Om du vill toopass hello jar filnamn och hello klassnamn som en del av en indatafil (i det här exemplet input.txt)
  
        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-livy-spark-batches-running-on-hello-cluster"></a>Hämta information om Livius Spark batchar körs på hello kluster
    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Exempel**:

* Om du vill tooretrieve alla hello Livius Spark batchar som körs på klustret hello:
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
* Om du vill tooretrieve en specifik grupp med en viss batch-ID
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="delete-a-livy-spark-batch-job"></a>Ta bort batchjobb Livius Spark
    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Exempel**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-spark-and-high-availability"></a>Livius Spark och hög tillgänglighet
Livius ger hög tillgänglighet för Spark-jobb som körs på hello klustret. Här är några exempel.

* Om hello Livius tjänsten stängs av när du har skickat ett jobb via fjärranslutning tooa Spark-kluster, hello jobbet fortsätter toorun i hello bakgrund. När Livius säkerhetskopiera återställer hello status för hello jobb och rapporter som det igen.
* Jupyter-anteckningsböcker för HDInsight är drivs av Livius i hello backend. Om en bärbar dator med ett Spark-jobb och hello Livius service hämtar startas om, fortsätter hello anteckningsboken toorun hello kod celler. 

## <a name="show-me-an-example"></a>Visa ett exempel
I det här avsnittet vi titta på exempel toouse Livius Spark toosubmit batchjobb, övervaka hello hello jobb och tar bort den. hello programmet vi använder i det här exemplet är hello en utvecklats i hello artikeln [skapa en fristående Scala program och toorun på HDInsight Spark-kluster](hdinsight-apache-spark-create-standalone-application.md). hello stegen här förutsätter som:

* Du har redan kopieras över hello programmet jar toohello storage-konto som är associerade med hello-kluster.
* Du har CuRL som installerats på hello datorn där du försöker de här stegen.

Utför följande steg hello:

1. Låt oss först kontrollera att Livius Spark körs på hello klustret. Vi kan göra det genom att hämta en lista över aktiva batchar. Om du kör ett jobb med Livy för hello första gången ska hello utdata returnera noll.
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    Du bör få en liknande utdata-toohello följande fragment:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    Observera hur hello sista raden i hello utdata står **totalt: 0**, som föreslås inga körs batchar.

2. Låt oss nu skicka batchjobb. följande fragment hello använder en indatafilen (input.txt) toopass hello jar namn och hello-klassnamn som parametrar. Om du använder de här stegen från en Windows-dator är med hjälp av en indatafil hello rekommendationer.
   
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    Hej parametrar i hello filen **input.txt** definieras enligt följande:
   
        { "file":"wasb:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
   
    Du bör se en utdata liknande toohello följande utdrag:
   
        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    Observera hur hello sista raden i hello resultatet säger **tillstånd: startar**. Det också står **-id: 0**. Här, **0** är hello batch-ID.

3. Du kan nu hämta hello status för den här specifika batch med hello batch-ID.
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    Du bör se en utdata liknande toohello följande utdrag:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    hello utdata nu **tillstånd: lyckade**, vilket tyder på hello jobbet har slutförts.

4. Du kan nu ta hello batch.
   
        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    Du bör se en utdata liknande toohello följande utdrag:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    hello sista raden i hello utdata visar att hello batch har tagits bort. Om du tar bort ett jobb när den körs stoppar också hello jobb. Om du tar bort ett jobb som har slutförts utan problem, eller i annat fall tas bort hello jobbinformation helt.

## <a name="using-livy-spark-on-hdinsight-35-clusters"></a>Använda Livius Spark i HDInsight 3.5-kluster

3.5 HDInsight-kluster inaktiverar som standard användningen av lokal fil sökvägar tooaccess exempeldatafiler eller burkar. Vi rekommenderar att du toouse hello `wasb://` sökväg i stället tooaccess burkar och exempeldata filer från hello kluster. Om du vill toouse lokal sökväg, måste du uppdatera hello Ambari configuration därefter. toodo så:

1. Gå toohello Ambari portal för hello-kluster. Hej Ambari-Webbgränssnittet är tillgängligt på ditt HDInsight-kluster på https://**KLUSTERNAMN**. azurehdidnsight.net, där KLUSTERNAMN är hello namnet på klustret.

2. Hello vänstra navigeringsrutan, klicka på **Livius**, och klicka sedan på **konfigurationerna**.

3. Under **Livius standard** lägga till hello egenskapsnamn `livy.file.local-dir-whitelist` och anger dess värde för**”/”** om du vill ha tooallow fullständig åtkomst toofile system. Om du vill tooallow åtkomst endast tooa specifik katalog, ange hello sökvägen toothat directory som hello-värde.

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a>Skickar Livius jobben för ett kluster i Azure-nätverk

Om du ansluter tooan HDInsight Spark-kluster från ett Azure Virtual Network, kan du direkt ansluta tooLivy på hello klustret. I sådana fall hello URL för Livius slutpunkten är `http://<IP address of hello headnode>:8998/batches`. Här, **8998** är hello-port som Livius körs på hello klustret headnode. Mer information om att komma åt tjänster på icke-offentliga portar finns [portar som används av Hadoop-tjänster på HDInsight](hdinsight-hadoop-port-settings-for-services.md).

## <a name="troubleshooting"></a>Felsökning

Här följer några problem som kan uppstå när du använder Livius för fjärranslutna jobbet skicka tooSpark kluster.

### <a name="using-an-external-jar-from-hello-additional-storage-is-not-supported"></a>Med hjälp av en extern jar från hello ytterligare lagring stöds inte

**Problem:** om Livius Spark-jobbet refererar till en extern jar från hello ytterligare lagringskonto som associerats med hello kluster, hello jobb misslyckas.

**Lösning:** se till att hello jar som du vill använda toouse är tillgängliga i hello standardlagring som är associerade med hello HDInsight-kluster.





## <a name="next-step"></a>Nästa steg

* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)

