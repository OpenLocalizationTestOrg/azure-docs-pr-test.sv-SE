---
title: "aaaUse skriptåtgärd tooinstall Solr på Hadoop - kluster i Azure | Microsoft Docs"
description: "Lär dig hur toocustomize HDInsight-kluster med Solr med skriptåtgärder."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b1e6f338-8ac1-4b38-bbb5-2f7388b9de3b
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 022ba56b7499390a91bfe869e5069893e56b6503
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-solr-on-windows-based-hdinsight-clusters"></a>Installera och använda Solr på Windows-baserade HDInsight-kluster

Lär dig hur toocustomize Windows-baserade HDInsight-kluster med Solr med skriptåtgärder och hur toouse Solr toosearch data.

> [!IMPORTANT]
> hello stegen i det här dokumentet som endast fungerar med Windows-baserade HDInsight-kluster. HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Information om hur du använder Solr med ett Linux-baserade kluster finns i [installerar och använder Solr på HDinsight Hadoop-kluster (Linux)](hdinsight-hadoop-solr-install-linux.md).


Du kan installera Solr på någon typ av kluster (Hadoop, Storm, HBase, Spark) på Azure HDInsight med hjälp av *skriptåtgärd*. Ett exempel skriptet tooinstall Solr på ett HDInsight-kluster är tillgänglig från en skrivskyddad Azure storage blob [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

hello exempelskript fungerar bara med HDInsight-kluster av version 3.1. Mer information om HDInsight-kluster-versioner finns [HDInsight-kluster-versioner](hdinsight-component-versioning.md).

hello exempelskript som används i det här avsnittet skapar ett Windows-baserade Solr kluster med en viss konfiguration. Om du vill tooconfigure hello Solr kluster med olika samlingar, delar, scheman, repliker, etc., måste du ändra hello skript och Solr binärfiler därefter.

**Relaterade artiklar**

* [Installera och använda Solr på HDinsight Hadoop-kluster (Linux)](hdinsight-hadoop-solr-install-linux.md)
* [Skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md): allmän information om hur du skapar HDInsight-kluster.
* [Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder.
* [Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md).

## <a name="what-is-solr"></a>Vad är Solr?
<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> är en enterprise search-plattform som gör att kraftfulla fulltextsökning i data. Hadoop gör det möjligt för lagring och hantering av stora mängder data, ger Apache Solr hello sökfunktioner tooquickly hämta hello data.

## <a name="install-solr-using-portal"></a>Installera Solr med hjälp av portalen
1. Skapa ett kluster med hjälp av hello **skapa anpassade** alternativ, enligt beskrivningen i [skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md).
2. På hello **skriptåtgärder** sidan hello guiden klickar du på **lägga till skriptåtgärd** tooprovide information om hello skriptåtgärd enligt nedan:

    ![Använd skriptåtgärder toocustomize ett kluster](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Använd skriptåtgärder toocustomize ett kluster")

    <table border='1'>
        <tr><th>Egenskap</th><th>Värde</th></tr>
        <tr><td>Namn</td>
            <td>Ange ett namn för hello skriptåtgärder. Till exempel <b>installera Solr</b>.</td></tr>
        <tr><td>Skript-URI</td>
            <td>Ange hello identifierare URI (Uniform Resource) toohello skript som är anropade toocustomize hello-kluster. Till exempel <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Nodtyp</td>
            <td>Ange hello noder som hello anpassning skript körs. Du kan välja <b>alla noder</b>, <b>huvudnoder endast</b>, eller <b>arbetarnoder endast</b>.
        <tr><td>Parametrar</td>
            <td>Ange hello parametrar, om det krävs av hello skript. hello skriptet tooinstall Solr kräver inte parametrar, så du kan lämna den tom.</td></tr>
    </table>

    Du kan lägga till fler än en skript åtgärd tooinstall flera komponenter på hello klustret. När du har lagt till hello skript klickar du på hello markering toostart skapar hello kluster.

## <a name="use-solr"></a>Använd Solr
Du måste börja med att indexera Solr med vissa datafiler. Du kan sedan använda Solr toorun sökningar på hello indexerade data. Utför följande steg toouse Solr i ett HDInsight-kluster hello:

1. **Använda Remote Desktop Protocol (RDP) tooremote till hello HDInsight-kluster med installerat Solr**. Aktivera Fjärrskrivbord för hello-kluster som du skapade med Solr installerad och sedan fjärråtkomst till hello kluster från hello Azure-portalen. Instruktioner finns i [ansluta tooHDInsight kluster med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).
2. **Index Solr genom att överföra filer**. När du indexera Solr placera dokument i den som du kan behöva toosearch på. tooindex Solr, Använd RDP tooremote i hello kluster, navigera toohello skrivbordet, öppna hello Hadoop-kommandoraden och navigera för**C:\apps\dist\solr-4.7.2\example\exampledocs**. Kör följande kommando hello:

        java -jar post.jar solr.xml monitor.xml

    Hello efter hello konsolen utdata visas:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes toohttp://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Hej post.jar verktyget indexerar Solr två exempel dokument **solr.xml** och **monitor.xml**. Hej post.jar verktyget och hello exempeldokument är tillgängliga med Solr installation.
3. **Använd hello Solr instrumentpanelen toosearch inom hello indexerade dokument**. I hello RDP-session toohello HDInsight-kluster, öppna Internet Explorer och starta hello Solr instrumentpanelen på **#-http://headnodehost:8983/solr/**. Från hello vänster från hello **Core Selector** listrutan, Välj **collection1**, och i, klickar du på **frågan**. Som exempel, tooselect och returnera ange alla hello dokument i Solr, hello följande värden:

   * I hello **q** text Ange  **\*:**\*. Detta returnerar alla hello-dokument som indexeras i Solr. Om du vill använda toosearch för en specifik sträng inom hello dokument kan du ange den här strängen.
   * I hello **wt** textruta, Välj hello utdataformat. Standardvärdet är **json**. Klicka på **köra frågan**.

     ![Använd skriptåtgärder toocustomize ett kluster](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "köra en fråga på Solr instrumentpanelen")

     hello utdata returnerar hello två dokument som vi använde för fulltextindexering Solr. hello utdata liknar hello följande:

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
4. **Rekommenderat: Säkerhetskopiera hello indexerade data från Solr tooAzure Blob storage som är associerade med hello HDInsight-kluster**. Ett bra tips är bör du säkerhetskopiera hello indexerade data från hello Solr klusternoder till Azure Blob storage. Utför följande steg toodo så hello:

   1. Öppna Internet Explorer och punkt toohello följande URL: en från hello RDP-session:

           http://localhost:8983/solr/replication?command=backup

       Du bör se ett svar som detta:

           <?xml version="1.0" encoding="UTF-8"?>
           <response>
             <lst name="responseHeader">
               <int name="status">0</int>
               <int name="QTime">9</int>
             </lst>
             <str name="status">OK</str>
           </response>
   2. Hello fjärrsession navigera för {SOLR_HOME}\{samling} \data. Detta bör vara för hello-kluster som har skapats via hello exempelskript **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. På den här platsen bör du se en mapp för ögonblicksbilder som skapats med ett namn liknande för**ögonblicksbild.* tidsstämpel***.
   3. ZIP-mapp för ögonblicksbilder för hello och överför den tooAzure Blob storage. Från kommandoraden för hello Hadoop, navigera toohello platsen för hello ögonblicksbildmappen med hello följande kommando:

             hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

       Det här kommandot kopior hello ögonblicksbild för/exempel/data/under hello behållare i hello standard Storage-kontot som associerats med hello kluster.

## <a name="install-solr-using-aure-powershell"></a>Installera Solr med Aure PowerShell
Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  hello exemplet visar hur tooinstall Spark med hjälp av Azure PowerShell. Du behöver toocustomize hello skriptet toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="install-solr-using-net-sdk"></a>Installera Solr med .NET SDK
Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). hello exemplet visar hur tooinstall Spark med hello .NET SDK. Du behöver toocustomize hello skriptet toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="see-also"></a>Se även
* [Installera och använda Solr på HDinsight Hadoop-kluster (Linux)](hdinsight-hadoop-solr-install-linux.md)
* [Skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md): allmän information om hur du skapar HDInsight-kluster.
* [Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder.
* [Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md).
* [Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]: skriptåtgärder exempel om hur du installerar Spark.
* [Installera R på HDInsight-kluster][hdinsight-install-r]: skriptåtgärder exempel om hur du installerar R.
* [Installera Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md): skriptåtgärder exempel om hur du installerar Giraph.

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
