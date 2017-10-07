---
title: "aaaPorts som används av Hadoop på HDInsight - Azure-tjänster | Microsoft Docs"
description: "En lista över portar som används av Hadoop-tjänster som körs på HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: dd14aed9-ec25-4bb3-a20c-e29562735a7d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: larryfr
ms.openlocfilehash: 0abc5c1c678aa79816e3e82a74538d2fb6db40ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ports-used-by-hadoop-services-on-hdinsight"></a>Portar som används av Hadoop-tjänster på HDInsight

Det här dokumentet innehåller en lista över hello-portar som används av Hadoop-tjänster som körs på Linux-baserade HDInsight-kluster. Det ger också information om portar används tooconnect toohello kluster med SSH.

## <a name="public-ports-vs-non-public-ports"></a>Offentliga portar jämfört med icke-offentliga portar

Linux-baserat HDInsight kluster endast exponera tre portar offentligt på hello internet. 22, 23 och 443. Dessa portar är används toosecurely åtkomst hello klustret med SSH och tjänster som exponeras via hello säker HTTPS-protokollet.

Internt HDInsight implementeras av flera virtuella datorer i Azure (hello noder i klustret hello) körs på ett Azure Virtual Network. Hello virtuellt nätverk, från tillgång portarna exponeras inte via hello internet. Till exempel om du ansluter tooone av hello head-noder som använder SSH från hello huvudnod du kan sedan direkt åtkomst till tjänster som körs på hello klusternoder.

> [!IMPORTANT]
> Om du inte anger ett virtuellt Azure-nätverk som ett alternativ för HDInsight, skapas en automatiskt. Men du kan inte ansluta till andra datorer (till exempel andra Azure-datorer eller utvecklingsdatorn klienten) toothis virtuellt nätverk.


toojoin ytterligare datorer toohello virtuellt nätverk, du måste skapa hello virtuellt nätverk först och sedan ange den när du skapar ditt HDInsight-kluster. Mer information finns i [utöka HDInsight funktioner med hjälp av ett virtuellt Azure-nätverk](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Offentliga portar

Alla hello noder i ett HDInsight-kluster finns i ett virtuellt Azure-nätverk och kan inte användas direkt från hello internet. En offentlig gateway ger internet access toohello följande portar som är gemensamma för alla typer av HDInsight-kluster.

| Tjänst | Port | Protokoll | Beskrivning |
| --- | --- | --- | --- | --- |
| sshd |22 |SSH |Ansluter klienter toosshd på hello primära headnode. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight). |
| sshd |22 |SSH |Ansluter klienter toosshd på hello kantnod. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight). |
| sshd |23 |SSH |Ansluter klienter toosshd på hello sekundära headnode. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight). |
| Ambari |443 |HTTPS |Ambari-webbgränssnittet. Se [hantera HDInsight med hello Ambari-Webbgränssnittet](hdinsight-hadoop-manage-ambari.md) |
| Ambari |443 |HTTPS |Ambari REST API. Se [hantera HDInsight med hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat |443 |HTTPS |HCatalog REST API. Se [använda Hive med Curl](hdinsight-hadoop-use-pig-curl.md), [använda Pig med Curl](hdinsight-hadoop-use-pig-curl.md), [använder MapReduce med Curl](hdinsight-hadoop-use-mapreduce-curl.md) |
| HiveServer2 |443 |ODBC |Ansluter tooHive med ODBC. Se [ansluta Excel tooHDInsight med hello Microsoft ODBC driver](hdinsight-connect-excel-hive-odbc-driver.md). |
| HiveServer2 |443 |JDBC |Ansluter tooHive med JDBC. Se [ansluta tooHive på HDInsight med hello Hive JDBC-drivrutinen](hdinsight-connect-hive-jdbc-driver.md) |

hello följande är tillgängliga för specifika klustertyper:

| Tjänst | Port | Protokoll | Klustertyp | Beskrivning |
| --- | --- | --- | --- | --- |
| Stargate |443 |HTTPS |HBase |HBase REST-API. Se [komma igång med HBase](hdinsight-hbase-tutorial-get-started-linux.md) |
| Livy |443 |HTTPS |Spark |Spark REST API. Se [skicka Spark jobb via fjärranslutning med Livy](hdinsight-apache-spark-livy-rest-interface.md) |
| Storm |443 |HTTPS |Storm |Storm-webbgränssnittet. Se [distribuera och hantera Storm-topologier på HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md) |

### <a name="authentication"></a>Autentisering

Alla tjänster som är offentligt tillgängliga på internet måste autentiseras hello:

| Port | Autentiseringsuppgifter |
| --- | --- |
| 22 eller 23 |hello SSH-autentiseringsuppgifter anges när klustret skapas |
| 443 |hello inloggningsnamnet (standard: admin) och lösenordet som angavs när klustret skapas |

## <a name="non-public-ports"></a>Icke-offentligt portar

> [!NOTE]
> Vissa tjänster är bara tillgängliga på specifika klustertyper. HBase är till exempel bara tillgänglig på HBase-klustertyper.

> [!IMPORTANT]
> Vissa tjänster bara köras på en headnode i taget. Om du försöker tooconnect toohello-tjänsten på hello primära headnode och får ett 404-fel kan du försöka använda hello sekundära headnode.

### <a name="ambari"></a>Ambari

| Tjänst | Noder | Port | URL-sökväg | Protokoll | 
| --- | --- | --- | --- | --- |
| Ambari-webbgränssnittet | HEAD-noder | 8080 | / | HTTP |
| Ambari REST API | HEAD-noder | 8080 | / api/v1 | HTTP |

Exempel:

* Ambari REST API:`curl -u admin "http://10.0.0.11:8080/api/v1/clusters"`

### <a name="hdfs-ports"></a>HDFS-portar

| Tjänst | Noder | Port | Protokoll | Beskrivning |
| --- | --- | --- | --- | --- |
| NameNode webbgränssnittet |HEAD-noder |30070 |HTTPS |Web UI tooview status |
| NameNode metadata service |HEAD-noder |8020 |IPC |Filmetadata system |
| DataNode |Alla arbetarnoder |30075 |HTTPS |Web UI tooview status, loggar och så vidare. |
| DataNode |Alla arbetarnoder |30010 |&nbsp; |Dataöverföring |
| DataNode |Alla arbetarnoder |30020 |IPC |Åtgärder för metadata |
| Sekundär NameNode |HEAD-noder |50090 |HTTP |Kontrollpunkt för NameNode metadata |

### <a name="yarn-ports"></a>YARN-portar

| Tjänst | Noder | Port | Protokoll | Beskrivning |
| --- | --- | --- | --- | --- |
| Hanteraren för filserverresurser webbgränssnittet |HEAD-noder |8088 |HTTP |Webbgränssnittet för Resource Manager |
| Hanteraren för filserverresurser webbgränssnittet |HEAD-noder |8090 |HTTPS |Webbgränssnittet för Resource Manager |
| Hanteraren för filserverresurser administratörsgränssnittet |HEAD-noder |8141 |IPC |För program bidrag (Hive, Pig, Hive server osv.) |
| Hanteraren för filserverresurser Schemaläggaren |HEAD-noder |8030 |HTTP |Administrativt gränssnitt |
| Programgränssnittet för Resource Manager |HEAD-noder |8050 |HTTP |Adressen för hello program manager-gränssnittet |
| NodeManager |Alla arbetarnoder |30050 |&nbsp; |hello-adressen för hello behållaren manager |
| NodeManager webbgränssnittet |Alla arbetarnoder |30060 |HTTP |Resource manager-gränssnittet |
| Tidslinjen adress |HEAD-noder |10200 |RPC |hello tidslinjen RPC-tjänsten. |
| Tidslinjen webbgränssnittet |HEAD-noder |8181 |HTTP |hello tidslinjen service webbgränssnittet |

### <a name="hive-ports"></a>Hive-portar

| Tjänst | Noder | Port | Protokoll | Beskrivning |
| --- | --- | --- | --- | --- |
| HiveServer2 |HEAD-noder |10001 |Thrift |Tjänsten för att ansluta tooHive (Thrift/JDBC) |
| Hive Metastore |HEAD-noder |9083 |Thrift |Tjänsten för anslutande tooHive metadata (Thrift/JDBC) |

### <a name="webhcat-ports"></a>WebHCat-portar

| Tjänst | Noder | Port | Protokoll | Beskrivning |
| --- | --- | --- | --- | --- |
| WebHCat-server |HEAD-noder |30111 |HTTP |Webb-API ovanpå HCatalog och andra Hadoop-tjänster |

### <a name="mapreduce-ports"></a>MapReduce-portar

| Tjänst | Noder | Port | Protokoll | Beskrivning |
| --- | --- | --- | --- | --- |
| Jobbhistorik |HEAD-noder |19888 |HTTP |MapReduce JobHistory webbgränssnittet |
| Jobbhistorik |HEAD-noder |10020 |&nbsp; |MapReduce JobHistory server |
| ShuffleHandler |&nbsp; |13562 |&nbsp; |Överföringar mellanliggande kartan matar ut toorequesting förminskningsapparater |

### <a name="oozie"></a>Oozie

| Tjänst | Noder | Port | Protokoll | Beskrivning |
| --- | --- | --- | --- | --- |
| Oozie-server |HEAD-noder |11000 |HTTP |URL för Oozie-tjänsten |
| Oozie-server |HEAD-noder |11001 |HTTP |Port för Oozie admin |

### <a name="ambari-metrics"></a>Ambari mått

| Tjänst | Noder | Port | Protokoll | Beskrivning |
| --- | --- | --- | --- | --- |
| Tidslinjen (programmet historik) |HEAD-noder |6188 |HTTP |hello tidslinjen service webbgränssnittet |
| Tidslinjen (programmet historik) |HEAD-noder |30200 |RPC |hello tidslinjen service webbgränssnittet |

### <a name="hbase-ports"></a>HBase-portar

| Tjänst | Noder | Port | Protokoll | Beskrivning |
| --- | --- | --- | --- | --- |
| HMaster |HEAD-noder |16000 |&nbsp; |&nbsp; |
| HMaster info Webbgränssnittet |HEAD-noder |16010 |HTTP |hello-port för hello HBase Master webbgränssnittet |
| Region server |Alla arbetarnoder |16020 |&nbsp; |&nbsp; |
| &nbsp; |&nbsp; |2181 |&nbsp; |hello-port som klienter använder tooconnect tooZooKeeper |

### <a name="kafka-ports"></a>Kafka portar

| Tjänst | Noder | Port | Protokoll | Beskrivning |
| --- | --- | --- | --- | --- |
| Service Broker |Arbetsnoder |9092 |[Kafka-protokollet](http://kafka.apache.org/protocol.html) |Används för klientkommunikation |
| &nbsp; |Zookeeper-noder |2181 |&nbsp; |hello-port som klienter använder tooconnect tooZookeeper |

### <a name="spark-ports"></a>Spark-portar

| Tjänst | Noder | Port | Protokoll | URL-sökväg | Beskrivning |
| --- | --- | --- | --- | --- | --- |
| Spark Thrift-servrar |HEAD-noder |10002 |Thrift | &nbsp; | Tjänsten för att ansluta tooSpark SQL (Thrift/JDBC) |
| Livius server | HEAD-noder | 8998 | HTTP | /batches | Tjänsten för att köra instruktioner, jobb och program |

Exempel:

* Livius: `curl "http://10.0.0.11:8998/batches"`. I det här exemplet `10.0.0.11` är hello hello headnode som är värd för hello Livius service IP-adress.
