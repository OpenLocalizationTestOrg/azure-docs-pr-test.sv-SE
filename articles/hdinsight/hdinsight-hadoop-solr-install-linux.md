---
title: "aaaUse skriptåtgärd tooinstall Solr på Linux-baserade HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur tooinstall Solr på Linux-baserade HDInsight Hadoop-kluster med skriptåtgärder."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cc93ed5c-a358-456a-91a4-f179185c0e98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: larryfr
ms.openlocfilehash: 4c179032b95ae187f1830d8927f8796372fa8ebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Installera och använda Solr på HDInsight Hadoop-kluster

Lär dig hur tooinstall Solr på Azure HDInsight med hjälp av skriptåtgärder. Solr är en kraftfull sökplattform och företagsnivå sökfunktioner om data som hanteras av Hadoop.

> [!IMPORTANT]
    > hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!IMPORTANT]
> hello exempelskript som används i det här dokumentet installerar Solr 4.9 med en viss konfiguration. Om du vill tooconfigure hello Solr kluster med olika samlingar, delar, scheman, repliker, etc., måste du ändra hello skript och Solr binärfiler.

## <a name="whatis"></a>Vad är Solr

[Apache Solr](http://lucene.apache.org/solr/features.html) är en enterprise search-plattform som gör att kraftfulla fulltextsökning i data. Hadoop gör det möjligt för lagring och hantering av stora mängder data, ger Apache Solr hello sökfunktioner tooquickly hämta hello data.

> [!WARNING]
> Komponenter som ingår i hello HDInsight-kluster stöds fullt ut av Microsoft.
>
> Anpassade komponenter, till exempel Solr, ta emot kommersiellt rimliga stöd toohelp du toofurther hello felsökning. Microsoft-supporten kanske inte kan tooresolve problem med anpassade komponenter. Du kan behöva tooengage hello öppen källkod communities för hjälp. Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/).

## <a name="what-hello-script-does"></a>Vilka hello skriptet gör

Det här skriptet gör följande ändringar toohello HDInsight-kluster hello:

* Installerar Solr 4.9 i`/usr/hdp/current/solr`
* Skapar en användare **solrusr**, vilket är att använda toorun hello Solr service
* Anger **solruser** som hello ägare`/usr/hdp/current/solr`
* Lägger till en [Upstart](http://upstart.ubuntu.com/) konfiguration som startar Solr automatiskt.

## <a name="install"></a>Installera Solr med hjälp av skriptåtgärder

Ett exempel skriptet tooinstall Solr på ett HDInsight-kluster finns på följande plats hello:

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

toocreate ett kluster med installerat Solr, Använd hello stegen i hello [skapa HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md) dokumentet. Använd följande steg tooinstall Solr hello under skapandeprocessen hello:

1. Från hello __klustret sammanfattning__ bladet, select__Advanced settings__, sedan __skript åtgärder__. Använd följande information toopopulate hello formuläret hello:

   * **NAMNET**: Ange ett eget namn för hello skriptåtgärder.
   * **SKRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh
   * **HEAD**: Markera det här alternativet
   * **WORKER**: Markera det här alternativet
   * **ZOOKEEPER**: Kontrollera det här alternativet tooinstall på hello Zookeeper nod
   * **PARAMETRARNA**: lämna fältet tomt

2. Längst ned hello hello **skript åtgärder** blad, Använd hello **Välj** toosave hello konfiguration. Använd slutligen hello **nästa** knappen tooreturn toohello __klustret sammanfattning__

3. Från hello __klustret sammanfattning__ väljer __skapa__ toocreate hello klustret.

## <a name="usesolr"></a>Hur använder Solr i HDInsight

> [!IMPORTANT]
> hello stegen i det här avsnittet visar grundläggande Solr funktioner. Mer information om hur du använder Solr finns hello [Apache Solr plats](http://lucene.apache.org/solr/).

### <a name="index-data"></a>Indexinformationen

Använd följande steg tooadd exempel data tooSolr hello och fråga den:

1. Anslut toohello HDInsight-kluster med SSH:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

     > [!IMPORTANT]
     > Stegen senare i det här dokumentet använder en SSL-tunnel tooconnect toohello Solr webbgränssnittet. toouse dessa steg, måste du upprätta en SSL tunnel och sedan konfigurera din webbläsare toouse den.
     >
     > Mer information finns i hello [använda SSH-tunnlar med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentet.

2. Använd följande kommandon toohave Solr index exempeldata hello:

    ```bash
    cd /usr/hdp/current/solr/example/exampledocs
    java -jar post.jar solr.xml monitor.xml
    ```

    hello returneras följande utdata toohello konsolen:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes toohttp://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Hej `post.jar` verktyget lägger till hello **solr.xml** och **monitor.xml** dokument toohello index.
  
3. Använd hello följande kommando tooquery hello Solr REST API:

    ```bash
    curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"
    ```

    Det här kommandot söker **collection1** för alla dokument som matchar  **\*:\***  (kodad som \*% 3A\* i hello frågesträngen). hello följande JSON-dokumentet är ett exempel på hello svar:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, hello Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication tooother Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over hello e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }

### <a name="using-hello-solr-dashboard"></a>Med hjälp av hello Solr instrumentpanelen

Hej Solr instrumentpanelen är ett webbgränssnitt som låter dig toowork med Solr via webbläsaren. Hej Solr instrumentpanelen visas inte direkt på hello Internet från ditt HDInsight-kluster. Du kan använda en SSH-tunnel tooaccess den. Mer information om hur du använder en SSH-tunnel finns hello [använda SSH-tunnlar med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentet.

När du har skapat en SSH-tunnel använder du följande steg toouse hello Solr instrumentpanelen hello:

1. Bestäm hello värdnamnet för hello primära headnode:

   1. Använda SSH tooconnect toohello klustrets huvudnod. Till exempel `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

       Mer information om hur du använder SSH finns hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

   2. Använd följande kommando tooget hello fullständigt kvalificerade värdnamn hello:

        ```bash
        hostname -f
        ```

        Det här kommandot returnerar ett värde liknande toohello efter värdnamn:

            hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

        Spara hello-värdet som returneras, eftersom den används senare.

2. Anslut för i din webbläsare**#-http://HOSTNAME:8983/solr/**, där **VÄRDNAMN** är hello namn du bestämde i föregående steg i hello.

    hello begäran dirigeras via hello SSH-tunnel toohello Solr webbgränssnittet på ditt kluster. hello visas liknande toohello följande bild:

    ![Bild av Solr instrumentpanelen](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

3. Använd hello från vänster hello **Core Selector** listrutan tooselect **collection1**. Flera poster ska dem visas under **collection1**.

4. Från hello posterna nedan **collection1**väljer **frågan**. Använd följande värden toopopulate hello söksidan hello:

   * I hello **q** text Ange  **\*:**\*. Den här frågan returnerar alla hello-dokument som indexeras i Solr. Om du vill använda toosearch för en specifik sträng inom hello dokument kan du ange den här strängen.
   * I hello **wt** textruta, Välj hello utdataformat. Standardvärdet är **json**.

     Välj slutligen hello **köra frågan** knappen längst ned hello hello Sök pate.

     ![Använd skriptåtgärder toocustomize ett kluster](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     hello utdata returnerar hello två dokument som du lagt till toohello index tidigare. hello utdata är liknande toohello följande JSON-dokumentet:

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, hello Enterprise Search Server",
                   "manu": "Apache Software Foundation",
                   "cat": [
                     "software",
                     "search"
                   ],
                   "features": [
                     "Advanced Full-Text Search Capabilities using Lucene",
                     "Optimized for High Volume Web Traffic",
                     "Standards Based Open Interfaces - XML and HTTP",
                     "Comprehensive HTML Administration Interfaces",
                     "Scalability - Efficient Replication tooother Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over hello e)"
                   ],
                   "price": 0,
                   "price_c": "0,USD",
                   "popularity": 10,
                   "inStock": true,
                   "incubationdate_dt": "2006-01-17T00:00:00Z",
                   "_version_": 1486960636996878300
                 },
                 {
                   "id": "3007WFP",
                   "name": "Dell Widescreen UltraSharp 3007WFP",
                   "manu": "Dell, Inc.",
                   "manu_id_s": "dell",
                   "cat": [
                     "electronics and computer1"
                   ],
                   "features": [
                     "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                   ],
                   "includes": "USB cable",
                   "weight": 401.6,
                   "price": 2199,
                   "price_c": "2199,USD",
                   "popularity": 6,
                   "inStock": true,
                   "store": "43.17614,-90.57341",
                   "_version_": 1486960637584081000
                 }
               ]
             }

### <a name="starting-and-stopping-solr"></a>Starta och stoppa Solr

Använd följande kommandon toomanually stoppa och starta Solr hello:

```bash
sudo stop solr
sudo start solr
```

## <a name="backup-indexed-data"></a>Indexerade säkerhetskopieringsdata

Använd hello följande steg tooback Solr lagring av data toohello standard för klustret:

1. Ansluta toohello kluster med SSH och Använd sedan följande kommando tooget hello värdnamn för hello huvudnod hello:

    ```bash
    hostname -f
    ```

2. Använd följande kommando toocreate en ögonblicksbild av hello indexerade data hello. Ersätt **VÄRDNAMN** med hello namn som returnerades från föregående hello-kommando:

    ```bash
    curl http://HOSTNAME:8983/solr/replication?command=backup
    ```

    hello svaret är liknande toohello följande XML:

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>

3. Ändra kataloger för`/usr/hdp/current/solr/example/solr`. Det finns en underkatalog för varje samling. Varje samling katalogen innehåller en `data` katalog som innehåller hello ögonblicksbild för hello samling.

4. toocreate komprimerat arkiv av hello-mapp för ögonblicksbilder, Använd hello följande kommando:

    ```bash
    tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855
    ```

    Ersätt hello `snapshot.20150806185338855` värden med hello hello ögonblicksbild för din samling.

    Det här kommandot skapas ett arkiv med namnet **snapshot.20150806185338855.tgz**, som innehåller hello innehållet i hello **snapshot.20150806185338855** directory.

5. Du kan sedan lagra hello Arkiv toohello klustrets primära lagring med hjälp av hello följande kommando:

    ```bash
    hdfs dfs -put snapshot.20150806185338855.tgz /example/data
    ```

Mer information om hur du arbetar med Solr säkerhetskopiering och återställning finns [https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).

## <a name="next-steps"></a>Nästa steg

* [Installera Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install-linux.md). Använd kluster anpassning tooinstall Giraph på HDInsight Hadoop-kluster. Giraph kan du tooperform diagrammet bearbetning med hjälp av Hadoop och kan användas med Azure HDInsight.

* [Installera Hue i HDInsight-kluster](hdinsight-hadoop-hue-linux.md). Använd kluster anpassning tooinstall Hue på HDInsight Hadoop-kluster. Hue är en uppsättning webbprogram används toointeract med ett Hadoop-kluster.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
