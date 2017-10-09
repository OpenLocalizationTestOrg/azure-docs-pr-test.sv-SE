---
title: aaaUse Apache Phoenix och SQuirreL med HBase - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Apache Phoenix i HDInsight, och hur tooinstall och konfigurera SQuirreL på din arbetsstation tooconnect tooan HBase-kluster i HDInsight."
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
ms.openlocfilehash: a63e8c8212b7a992453ec94fa638ec3863a0ede3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Använda Apache Phoenix med Linux-baserade HBase-kluster i HDInsight
Lär dig hur toouse [Apache Phoenix](http://phoenix.apache.org/) i HDInsight, och hur toouse SQLLine. Läs mer om Phoenix [Phoenix i 15 minuter eller mindre](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Hello Phoenix grammatik, se [Phoenix grammatik](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Hello Phoenix versionsinformation i HDInsight, se [vad är nytt i hello Hadoop-klusterversioner som tillhandahålls av HDInsight?](hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>Använd SQLLine
[SQLLine](http://sqlline.sourceforge.net/) är ett kommandoradsverktyg tooexecute SQL.

### <a name="prerequisites"></a>Krav
Innan du kan använda SQLLine, måste du ha hello följande:

* **Ett HBase-kluster i HDInsight**. Mer information om etablera HBase-kluster, se [Kom igång med Apache HBase i HDInsight][hdinsight-hbase-get-started].
* **Ansluta toohello HBase-kluster via hello Fjärrskrivbordsprotokollet**. Instruktioner finns i [hantera Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen][hdinsight-manage-portal].

När du ansluter tooan HBase-kluster måste tooconnect tooone av hello Zookeepers. Varje HDInsight-kluster har tre Zookeepers.

**toofind ut hello Zookeeper-värdnamn**

1. Öppna Ambari genom att bläddra för**https://<ClusterName>. azurehdinsight.net**.
2. Ange hello HTTP (kluster) användarnamn och lösenord toologin.
3. Klicka på **ZooKeeper** hello vänstra menyn. Du ser tre **ZooKeeper-Server** visas.
4. Klicka på någon av hello **ZooKeeper-Server** visas. Hitta hello på hello sammanfattningsfönstret, **värdnamn**. Det är liknande för*zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**toouse SQLLine**

1. Ansluta toohello kluster med SSH. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Från SSH kör du följande kommandon toorun SQLLine hello:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure
3. Kör följande kommandon toocreate hello HBase-tabellen och infoga data:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

Mer information finns i [SQLLine manuell](http://sqlline.sourceforge.net/#manual) och [Phoenix grammatik](http://phoenix.apache.org/language/index.html).

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur toouse Apache Phoenix i HDInsight.  Det finns fler toolearn:

* [HDInsight HBase-översikt][hdinsight-hbase-overview]: HBase är en NoSQL-databas av Apachetyp med öppen källkod som bygger på Hadoop och som ger direktåtkomst och stark konsekvens för stora mängder ostrukturerade och semistrukturerade data.
* [Etablera HBase-kluster i Azure Virtual Network][hdinsight-hbase-provision-vnet]: med virtuell nätverksintegration HBase-kluster kan vara distribuerade toohello samma virtuella nätverk som dina program så att program kan kommunicera med HBase direkt.
* [Konfigurera HBase-replikering i HDInsight](hdinsight-hbase-replication.md): Lär dig hur tooconfigure HBase-replikering mellan två Azure-datacenter.
* [Analysera Twitter-åsikter med HBase i HDInsight][hbase-twitter-sentiment]: Lär dig hur toodo realtid [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) av stordata med HBase i ett Hadoop-kluster i HDInsight.

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
