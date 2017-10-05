---
title: "Använda Apache Phoenix och SQuirreL med HBase - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du använder Apache Phoenix i HDInsight och hur du installerar och konfigurerar SQuirreL på din arbetsstation för att ansluta till ett HBase-kluster i HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: cda0f33b-a2e8-494c-972f-ae0bb482b818
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/26/2017
ms.author: jgao
ms.openlocfilehash: 13d17083bbe26fa9745ce4c5fef9f56859243c2e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Använda Apache Phoenix med Linux-baserade HBase-kluster i HDInsight
Lär dig hur du använder [Apache Phoenix](http://phoenix.apache.org/) i HDInsight och hur du använder SQLLine. Läs mer om Phoenix [Phoenix i 15 minuter eller mindre](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Phoenix-grammatik finns [Phoenix grammatik](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Phoenix versionsinformation i HDInsight, se [vad är nytt i Hadoop-klusterversioner som tillhandahålls av HDInsight?](hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>Använd SQLLine
[SQLLine](http://sqlline.sourceforge.net/) är ett kommandoradsverktyg för att köra SQL.

### <a name="prerequisites"></a>Krav
Innan du kan använda SQLLine, måste du ha följande:

* **Ett HBase-kluster i HDInsight**. Mer information om etablera HBase-kluster, se [Kom igång med Apache HBase i HDInsight][hdinsight-hbase-get-started].
* **Ansluta till HBase-kluster via remote desktop protocol**. Instruktioner finns i [hantera Hadoop-kluster i HDInsight med hjälp av Azure portal][hdinsight-manage-portal].

När du ansluter till ett HBase-kluster, måste du ansluta till en Zookeepers. Varje HDInsight-kluster har tre Zookeepers.

**Ta reda på Zookeeper-värdnamn**

1. Öppna Ambari genom att bläddra till **https://<ClusterName>. azurehdinsight.net**.
2. Ange HTTP (kluster)-användarnamn och lösenord för inloggning.
3. Klicka på **ZooKeeper** i den vänstra menyn. Du ser tre **ZooKeeper-Server** visas.
4. Klicka på någon av de **ZooKeeper-Server** visas. I rutan Sammanfattning hitta den **värdnamn**. Den liknar *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**Att använda SQLLine**

1. Ansluta till klustret med SSH. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Kör följande kommandon för att köra SQLLine från SSH:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure
3. Kör följande kommandon för att skapa en HBase-tabell och infoga data:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

Mer information finns i [SQLLine manuell](http://sqlline.sourceforge.net/#manual) och [Phoenix grammatik](http://phoenix.apache.org/language/index.html).

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur du använder Apache Phoenix i HDInsight.  Du kan läsa mer här:

* [HDInsight HBase-översikt][hdinsight-hbase-overview]: HBase är en NoSQL-databas av Apachetyp med öppen källkod som bygger på Hadoop och som ger direktåtkomst och stark konsekvens för stora mängder ostrukturerade och semistrukturerade data.
* [Etablera HBase-kluster i Azure Virtual Network][hdinsight-hbase-provision-vnet]: med virtuell nätverksintegration HBase-kluster kan bara distribueras till samma virtuella nätverk som dina program så att program kan kommunicera med HBase direkt.
* [Konfigurera HBase-replikering i HDInsight](hdinsight-hbase-replication.md): Lär dig att konfigurera HBase-replikering mellan två Azure-datacenter.
* [Analysera Twitter-åsikter med HBase i HDInsight][hbase-twitter-sentiment]: Lär dig hur du gör realtid [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) av stordata med HBase i ett Hadoop-kluster i HDInsight.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
