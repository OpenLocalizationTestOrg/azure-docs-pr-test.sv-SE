---
title: aaaMonitor Hadoop-kluster i HDInsight med hello Ambari API - Azure | Microsoft Docs
description: "Använd hello Apache Ambari APIs för att skapa, hantera och övervaka Hadoop-kluster. Dölj hello komplexiteten hos Hadoop intuitiva operatorn verktyg och API: er."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
editor: cgronlun
manager: jhubbard
ms.assetid: 052135b3-d497-4acc-92ff-71cee49356ff
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: d61a8aae5ddfcd7d44f2e4cc899e0a4da5e5fdcc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-hadoop-clusters-in-hdinsight-using-hello-ambari-api"></a>Övervaka Hadoop-kluster i HDInsight med hello Ambari API
Lär dig hur toomonitor HDInsight-kluster med Ambari APIs.

> [!NOTE]
> hello informationen i den här artikeln är främst för Windows-baserade HDInsight-kluster som innehåller en skrivskyddad version av hello Ambari REST API. Linux-baserade kluster, se [hantera Hadoop-kluster med Ambari](hdinsight-hadoop-manage-ambari.md).
> 
> 

## <a name="what-is-ambari"></a>Vad är Ambari?
[Apache Ambari] [ ambari-home] används för etablering, hantering och övervakning av Apache Hadoop-kluster. Det innehåller en intuitiv samling operatörsverktyg och en stabil uppsättning API: er som döljer hello komplexiteten hos Hadoop och förenklar hello åtgärden kluster. Mer information om hello API: er finns [Ambari API-referens][ambari-api-reference]. 

HDInsight stöder för närvarande endast hello Ambari övervakningsfunktion. Ambari API 1.0 stöds av version 3.0 och 2.1 HDInsight-kluster. Den här artikeln täcker inte att komma åt Ambari APIs på HDInsight version 3.1 och 2.1-kluster. hello viktiga skillnaden mellan hello två är att vissa av hello komponenterna har ändrats med hello införandet av nya funktioner (till exempel hello jobbserver historik). 

**Förutsättningar**

Innan du påbörjar den här självstudien måste du ha hello följande objekt:

* **En arbetsstation med Azure PowerShell**.
* (Valfritt) [cURL][curl]. tooinstall, se [cURL versioner och hämtningsbara filer][curl-download].
  
  > [!NOTE]
  > När du använder hello cURL-kommando i Windows, Använd dubbla citattecken i stället för citattecken för hello alternativvärden.
  > 
  > 
* **Ett Azure HDInsight-kluster**. Instruktioner om hur klusteretablering finns [komma igång med HDInsight] [ hdinsight-get-started] eller [etablera HDInsight-kluster][hdinsight-provision]. Du behöver följande data toogo igenom kursen hello hello:
  
  | Kluster-egenskap | Variabelnamn för Azure PowerShell | Värde | Beskrivning |
  | --- | --- | --- | --- |
  |   HDInsight-klustrets namn |$clusterName | |hello namnet på ditt HDInsight-kluster. |
  |   Användarnamn för kluster |$clusterUsername | |Användaren klusternamnet anges när hello klustret har skapats. |
  |   Kluster-lösenord |$clusterPassword | |Användarlösenordet för klustret. |

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


## <a name="jump-start"></a>Komma igång
Det finns flera sätt toouse Ambari toomonitor HDInsight-kluster.

**Använda Azure PowerShell**

hello följande Azure PowerShell-skript hämtar hello MapReduce jobbinformation spåraren *i ett kluster i HDInsight 3.5.*  hello viktigaste skillnaden är att vi hämtar informationen från hello YARN-tjänsten (i stället för MapReduce).

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName/services/YARN/components/RESOURCEMANAGER"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

hello följande PowerShell-skript hämtar hello MapReduce jobbinformation spåraren *i ett kluster i HDInsight 2.1*:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

hello-utdata är:

![Jobtracker utdata][img-jobtracker-output]

**Använda cURL**

hello hämtar följande exempel information om klustret med hjälp av cURL:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

hello-utdata är:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**Hello 2014-10-8 versionen**:

När med hello Ambari slutpunkt, ”https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}” Hej *värddatornamn* fält Returnerar hello fullständigt kvalificerade domännamnet (FQDN) för hello nod i stället för hello värdnamn. Innan hello 2014-10-8 versionen kan det här exemplet returneras bara ”**headnode0**”. Efter hello 2014-10-8-versionen kan du hämta hello FQDN ”**headnode0. { ClusterDNS}. azurehdinsight.NET .net**”, enligt hello föregående exempel. Den här ändringen har nödvändiga toofacilitate scenarier där flera klustertyper (till exempel HBase och Hadoop) kan distribueras i ett virtuellt nätverk (VNET). Detta inträffar exempelvis när du använder HBase som en backend-plattform för Hadoop.

## <a name="ambari-monitoring-apis"></a>Ambari övervakning API: er
hello följande tabell visas några av hello vanligaste Ambari övervakning API-anrop. Mer information om hello API finns [Ambari API-referens][ambari-api-reference].

| Övervakare för API-anrop | URI: N | Beskrivning |
| --- | --- | --- |
| Hämta kluster |`/api/v1/clusters` | |
| Hämta klusterinformation. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net` |kluster, tjänster, värdar |
| Hämta tjänster |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services` |Tjänsterna omfattar: hdfs, mapreduce |
| Hämta information för tjänster. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>` | |
| Hämta tjänstkomponenter |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components` |HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker |
| Hämta information för komponenten. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>` |ServiceComponentInfo värd-komponenter, mått |
| Hämta värdar |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts` |headnode0 workernode0 |
| Hämta information om värden. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>` | |
| Hämta värden komponenter |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components` |namenode, resourcemanager |
| Hämta värden komponenten information. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>` |HostRoles komponent, värd, mått |
| Hämta konfigurationer |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations` |Config-typer: core-plats, hdfs-plats, mapred-plats, hive-plats |
| Hämta konfigurationsinformation. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>` |Config-typer: core-plats, hdfs-plats, mapred-plats, hive-plats |

## <a name="next-steps"></a>Nästa steg
Nu har du fått veta hur toouse Ambari övervakning API-anrop. Det finns fler toolearn:

* [Hantera HDInsight-kluster med hello Azure-portalen][hdinsight-admin-portal]
* [Hantera HDInsight-kluster med Azure PowerShell][hdinsight-admin-powershell]
* [Hantera HDInsight-kluster med hjälp av kommandoradsgränssnittet][hdinsight-admin-cli]
* [HDInsight-dokumentation][hdinsight-documentation]
* [Komma igång med HDInsight][hdinsight-get-started]

[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: https://docs.microsoft.com/azure/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
