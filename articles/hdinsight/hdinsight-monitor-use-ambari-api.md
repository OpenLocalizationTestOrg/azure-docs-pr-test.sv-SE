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
# <a name="monitor-hadoop-clusters-in-hdinsight-using-hello-ambari-api"></a><span data-ttu-id="9d666-104">Övervaka Hadoop-kluster i HDInsight med hello Ambari API</span><span class="sxs-lookup"><span data-stu-id="9d666-104">Monitor Hadoop clusters in HDInsight using hello Ambari API</span></span>
<span data-ttu-id="9d666-105">Lär dig hur toomonitor HDInsight-kluster med Ambari APIs.</span><span class="sxs-lookup"><span data-stu-id="9d666-105">Learn how toomonitor HDInsight clusters by using Ambari APIs.</span></span>

> [!NOTE]
> <span data-ttu-id="9d666-106">hello informationen i den här artikeln är främst för Windows-baserade HDInsight-kluster som innehåller en skrivskyddad version av hello Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="9d666-106">hello information in this article is primarily for Windows-based HDInsight clusters, which provide a read-only version of hello Ambari REST API.</span></span> <span data-ttu-id="9d666-107">Linux-baserade kluster, se [hantera Hadoop-kluster med Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="9d666-107">For Linux-based clusters, see [Manage Hadoop clusters using Ambari](hdinsight-hadoop-manage-ambari.md).</span></span>
> 
> 

## <a name="what-is-ambari"></a><span data-ttu-id="9d666-108">Vad är Ambari?</span><span class="sxs-lookup"><span data-stu-id="9d666-108">What is Ambari?</span></span>
<span data-ttu-id="9d666-109">[Apache Ambari] [ ambari-home] används för etablering, hantering och övervakning av Apache Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="9d666-109">[Apache Ambari][ambari-home] is used for provisioning, managing, and monitoring Apache Hadoop clusters.</span></span> <span data-ttu-id="9d666-110">Det innehåller en intuitiv samling operatörsverktyg och en stabil uppsättning API: er som döljer hello komplexiteten hos Hadoop och förenklar hello åtgärden kluster.</span><span class="sxs-lookup"><span data-stu-id="9d666-110">It includes an intuitive collection of operator tools and a robust set of APIs that hide hello complexity of Hadoop, simplifying hello operation of clusters.</span></span> <span data-ttu-id="9d666-111">Mer information om hello API: er finns [Ambari API-referens][ambari-api-reference].</span><span class="sxs-lookup"><span data-stu-id="9d666-111">For more information about hello APIs, see [Ambari API Reference][ambari-api-reference].</span></span> 

<span data-ttu-id="9d666-112">HDInsight stöder för närvarande endast hello Ambari övervakningsfunktion.</span><span class="sxs-lookup"><span data-stu-id="9d666-112">HDInsight currently supports only hello Ambari monitoring feature.</span></span> <span data-ttu-id="9d666-113">Ambari API 1.0 stöds av version 3.0 och 2.1 HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="9d666-113">Ambari API 1.0 is supported by HDInsight version 3.0 and 2.1 clusters.</span></span> <span data-ttu-id="9d666-114">Den här artikeln täcker inte att komma åt Ambari APIs på HDInsight version 3.1 och 2.1-kluster.</span><span class="sxs-lookup"><span data-stu-id="9d666-114">This article covers accessing Ambari APIs on HDInsight version 3.1 and 2.1 clusters.</span></span> <span data-ttu-id="9d666-115">hello viktiga skillnaden mellan hello två är att vissa av hello komponenterna har ändrats med hello införandet av nya funktioner (till exempel hello jobbserver historik).</span><span class="sxs-lookup"><span data-stu-id="9d666-115">hello key difference between hello two is that some of hello components have changed with hello introduction of new capabilities (such as hello Job History Server).</span></span> 

<span data-ttu-id="9d666-116">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="9d666-116">**Prerequisites**</span></span>

<span data-ttu-id="9d666-117">Innan du påbörjar den här självstudien måste du ha hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9d666-117">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="9d666-118">**En arbetsstation med Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="9d666-118">**A workstation with Azure PowerShell**.</span></span>
* <span data-ttu-id="9d666-119">(Valfritt) [cURL][curl].</span><span class="sxs-lookup"><span data-stu-id="9d666-119">(Optional) [cURL][curl].</span></span> <span data-ttu-id="9d666-120">tooinstall, se [cURL versioner och hämtningsbara filer][curl-download].</span><span class="sxs-lookup"><span data-stu-id="9d666-120">tooinstall it, see [cURL Releases and Downloads][curl-download].</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="9d666-121">När du använder hello cURL-kommando i Windows, Använd dubbla citattecken i stället för citattecken för hello alternativvärden.</span><span class="sxs-lookup"><span data-stu-id="9d666-121">When use hello cURL command in Windows, use double-quotation marks instead of single-quotation marks for hello option values.</span></span>
  > 
  > 
* <span data-ttu-id="9d666-122">**Ett Azure HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="9d666-122">**An Azure HDInsight cluster**.</span></span> <span data-ttu-id="9d666-123">Instruktioner om hur klusteretablering finns [komma igång med HDInsight] [ hdinsight-get-started] eller [etablera HDInsight-kluster][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="9d666-123">For instructions about cluster provisioning, see [Get started using HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="9d666-124">Du behöver följande data toogo igenom kursen hello hello:</span><span class="sxs-lookup"><span data-stu-id="9d666-124">You need hello following data toogo through hello tutorial:</span></span>
  
  | <span data-ttu-id="9d666-125">Kluster-egenskap</span><span class="sxs-lookup"><span data-stu-id="9d666-125">Cluster property</span></span> | <span data-ttu-id="9d666-126">Variabelnamn för Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9d666-126">Azure PowerShell variable name</span></span> | <span data-ttu-id="9d666-127">Värde</span><span class="sxs-lookup"><span data-stu-id="9d666-127">Value</span></span> | <span data-ttu-id="9d666-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9d666-128">Description</span></span> |
  | --- | --- | --- | --- |
  |   <span data-ttu-id="9d666-129">HDInsight-klustrets namn</span><span class="sxs-lookup"><span data-stu-id="9d666-129">HDInsight cluster name</span></span> |<span data-ttu-id="9d666-130">$clusterName</span><span class="sxs-lookup"><span data-stu-id="9d666-130">$clusterName</span></span> | |<span data-ttu-id="9d666-131">hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="9d666-131">hello name of your HDInsight cluster.</span></span> |
  |   <span data-ttu-id="9d666-132">Användarnamn för kluster</span><span class="sxs-lookup"><span data-stu-id="9d666-132">Cluster username</span></span> |<span data-ttu-id="9d666-133">$clusterUsername</span><span class="sxs-lookup"><span data-stu-id="9d666-133">$clusterUsername</span></span> | |<span data-ttu-id="9d666-134">Användaren klusternamnet anges när hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="9d666-134">Cluster user name specified when hello cluster was created.</span></span> |
  |   <span data-ttu-id="9d666-135">Kluster-lösenord</span><span class="sxs-lookup"><span data-stu-id="9d666-135">Cluster password</span></span> |<span data-ttu-id="9d666-136">$clusterPassword</span><span class="sxs-lookup"><span data-stu-id="9d666-136">$clusterPassword</span></span> | |<span data-ttu-id="9d666-137">Användarlösenordet för klustret.</span><span class="sxs-lookup"><span data-stu-id="9d666-137">Cluster user password.</span></span> |

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


## <a name="jump-start"></a><span data-ttu-id="9d666-138">Komma igång</span><span class="sxs-lookup"><span data-stu-id="9d666-138">Jump-start</span></span>
<span data-ttu-id="9d666-139">Det finns flera sätt toouse Ambari toomonitor HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="9d666-139">There are several ways toouse Ambari toomonitor HDInsight clusters.</span></span>

<span data-ttu-id="9d666-140">**Använda Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="9d666-140">**Use Azure PowerShell**</span></span>

<span data-ttu-id="9d666-141">hello följande Azure PowerShell-skript hämtar hello MapReduce jobbinformation spåraren *i ett kluster i HDInsight 3.5.*</span><span class="sxs-lookup"><span data-stu-id="9d666-141">hello following Azure PowerShell script gets hello MapReduce job tracker information *in an HDInsight 3.5 cluster.*</span></span>  <span data-ttu-id="9d666-142">hello viktigaste skillnaden är att vi hämtar informationen från hello YARN-tjänsten (i stället för MapReduce).</span><span class="sxs-lookup"><span data-stu-id="9d666-142">hello key difference is that we pull these details from hello YARN service (rather than MapReduce).</span></span>

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName/services/YARN/components/RESOURCEMANAGER"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

<span data-ttu-id="9d666-143">hello följande PowerShell-skript hämtar hello MapReduce jobbinformation spåraren *i ett kluster i HDInsight 2.1*:</span><span class="sxs-lookup"><span data-stu-id="9d666-143">hello following PowerShell script gets hello MapReduce job tracker information *in an HDInsight 2.1 cluster*:</span></span>

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

<span data-ttu-id="9d666-144">hello-utdata är:</span><span class="sxs-lookup"><span data-stu-id="9d666-144">hello output is:</span></span>

![Jobtracker utdata][img-jobtracker-output]

<span data-ttu-id="9d666-146">**Använda cURL**</span><span class="sxs-lookup"><span data-stu-id="9d666-146">**Use cURL**</span></span>

<span data-ttu-id="9d666-147">hello hämtar följande exempel information om klustret med hjälp av cURL:</span><span class="sxs-lookup"><span data-stu-id="9d666-147">hello following example gets cluster information by using cURL:</span></span>

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

<span data-ttu-id="9d666-148">hello-utdata är:</span><span class="sxs-lookup"><span data-stu-id="9d666-148">hello output is:</span></span>

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

<span data-ttu-id="9d666-149">**Hello 2014-10-8 versionen**:</span><span class="sxs-lookup"><span data-stu-id="9d666-149">**For hello 10/8/2014 release**:</span></span>

<span data-ttu-id="9d666-150">När med hello Ambari slutpunkt, ”https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}” Hej *värddatornamn* fält Returnerar hello fullständigt kvalificerade domännamnet (FQDN) för hello nod i stället för hello värdnamn.</span><span class="sxs-lookup"><span data-stu-id="9d666-150">When using hello Ambari endpoint, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", hello *host_name* field returns hello fully qualified domain name (FQDN) of hello node instead of hello host name.</span></span> <span data-ttu-id="9d666-151">Innan hello 2014-10-8 versionen kan det här exemplet returneras bara ”**headnode0**”.</span><span class="sxs-lookup"><span data-stu-id="9d666-151">Before hello 10/8/2014 release, this example returned simply "**headnode0**".</span></span> <span data-ttu-id="9d666-152">Efter hello 2014-10-8-versionen kan du hämta hello FQDN ”**headnode0. { ClusterDNS}. azurehdinsight.NET .net**”, enligt hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="9d666-152">After hello 10/8/2014 release, you get hello FQDN "**headnode0.{ClusterDNS}.azurehdinsight.net**", as shown in hello previous example.</span></span> <span data-ttu-id="9d666-153">Den här ändringen har nödvändiga toofacilitate scenarier där flera klustertyper (till exempel HBase och Hadoop) kan distribueras i ett virtuellt nätverk (VNET).</span><span class="sxs-lookup"><span data-stu-id="9d666-153">This change was required toofacilitate scenarios where multiple cluster types (such as HBase and Hadoop) can be deployed in one virtual network (VNET).</span></span> <span data-ttu-id="9d666-154">Detta inträffar exempelvis när du använder HBase som en backend-plattform för Hadoop.</span><span class="sxs-lookup"><span data-stu-id="9d666-154">This happens, for example, when using HBase as a back-end platform for Hadoop.</span></span>

## <a name="ambari-monitoring-apis"></a><span data-ttu-id="9d666-155">Ambari övervakning API: er</span><span class="sxs-lookup"><span data-stu-id="9d666-155">Ambari monitoring APIs</span></span>
<span data-ttu-id="9d666-156">hello följande tabell visas några av hello vanligaste Ambari övervakning API-anrop.</span><span class="sxs-lookup"><span data-stu-id="9d666-156">hello following table lists some of hello most common Ambari monitoring API calls.</span></span> <span data-ttu-id="9d666-157">Mer information om hello API finns [Ambari API-referens][ambari-api-reference].</span><span class="sxs-lookup"><span data-stu-id="9d666-157">For more information about hello API, see [Ambari API Reference][ambari-api-reference].</span></span>

| <span data-ttu-id="9d666-158">Övervakare för API-anrop</span><span class="sxs-lookup"><span data-stu-id="9d666-158">Monitor API call</span></span> | <span data-ttu-id="9d666-159">URI: N</span><span class="sxs-lookup"><span data-stu-id="9d666-159">URI</span></span> | <span data-ttu-id="9d666-160">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9d666-160">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9d666-161">Hämta kluster</span><span class="sxs-lookup"><span data-stu-id="9d666-161">Get clusters</span></span> |`/api/v1/clusters` | |
| <span data-ttu-id="9d666-162">Hämta klusterinformation.</span><span class="sxs-lookup"><span data-stu-id="9d666-162">Get cluster info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net` |<span data-ttu-id="9d666-163">kluster, tjänster, värdar</span><span class="sxs-lookup"><span data-stu-id="9d666-163">clusters, services, hosts</span></span> |
| <span data-ttu-id="9d666-164">Hämta tjänster</span><span class="sxs-lookup"><span data-stu-id="9d666-164">Get services</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services` |<span data-ttu-id="9d666-165">Tjänsterna omfattar: hdfs, mapreduce</span><span class="sxs-lookup"><span data-stu-id="9d666-165">Services include: hdfs, mapreduce</span></span> |
| <span data-ttu-id="9d666-166">Hämta information för tjänster.</span><span class="sxs-lookup"><span data-stu-id="9d666-166">Get services info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>` | |
| <span data-ttu-id="9d666-167">Hämta tjänstkomponenter</span><span class="sxs-lookup"><span data-stu-id="9d666-167">Get service components</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components` |<span data-ttu-id="9d666-168">HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker</span><span class="sxs-lookup"><span data-stu-id="9d666-168">HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker</span></span> |
| <span data-ttu-id="9d666-169">Hämta information för komponenten.</span><span class="sxs-lookup"><span data-stu-id="9d666-169">Get component info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>` |<span data-ttu-id="9d666-170">ServiceComponentInfo värd-komponenter, mått</span><span class="sxs-lookup"><span data-stu-id="9d666-170">ServiceComponentInfo, host-components, metrics</span></span> |
| <span data-ttu-id="9d666-171">Hämta värdar</span><span class="sxs-lookup"><span data-stu-id="9d666-171">Get hosts</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts` |<span data-ttu-id="9d666-172">headnode0 workernode0</span><span class="sxs-lookup"><span data-stu-id="9d666-172">headnode0, workernode0</span></span> |
| <span data-ttu-id="9d666-173">Hämta information om värden.</span><span class="sxs-lookup"><span data-stu-id="9d666-173">Get host info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>` | |
| <span data-ttu-id="9d666-174">Hämta värden komponenter</span><span class="sxs-lookup"><span data-stu-id="9d666-174">Get host components</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components` |<span data-ttu-id="9d666-175">namenode, resourcemanager</span><span class="sxs-lookup"><span data-stu-id="9d666-175">namenode, resourcemanager</span></span> |
| <span data-ttu-id="9d666-176">Hämta värden komponenten information.</span><span class="sxs-lookup"><span data-stu-id="9d666-176">Get host component info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>` |<span data-ttu-id="9d666-177">HostRoles komponent, värd, mått</span><span class="sxs-lookup"><span data-stu-id="9d666-177">HostRoles, component, host, metrics</span></span> |
| <span data-ttu-id="9d666-178">Hämta konfigurationer</span><span class="sxs-lookup"><span data-stu-id="9d666-178">Get configurations</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations` |<span data-ttu-id="9d666-179">Config-typer: core-plats, hdfs-plats, mapred-plats, hive-plats</span><span class="sxs-lookup"><span data-stu-id="9d666-179">Config types: core-site, hdfs-site, mapred-site, hive-site</span></span> |
| <span data-ttu-id="9d666-180">Hämta konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="9d666-180">Get configuration info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>` |<span data-ttu-id="9d666-181">Config-typer: core-plats, hdfs-plats, mapred-plats, hive-plats</span><span class="sxs-lookup"><span data-stu-id="9d666-181">Config types: core-site, hdfs-site, mapred-site, hive-site</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9d666-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9d666-182">Next Steps</span></span>
<span data-ttu-id="9d666-183">Nu har du fått veta hur toouse Ambari övervakning API-anrop.</span><span class="sxs-lookup"><span data-stu-id="9d666-183">Now you have learned how toouse Ambari monitoring API calls.</span></span> <span data-ttu-id="9d666-184">Det finns fler toolearn:</span><span class="sxs-lookup"><span data-stu-id="9d666-184">toolearn more, see:</span></span>

* <span data-ttu-id="9d666-185">[Hantera HDInsight-kluster med hello Azure-portalen][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="9d666-185">[Manage HDInsight clusters using hello Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="9d666-186">[Hantera HDInsight-kluster med Azure PowerShell][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="9d666-186">[Manage HDInsight clusters using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="9d666-187">[Hantera HDInsight-kluster med hjälp av kommandoradsgränssnittet][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="9d666-187">[Manage HDInsight clusters using command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="9d666-188">[HDInsight-dokumentation][hdinsight-documentation]</span><span class="sxs-lookup"><span data-stu-id="9d666-188">[HDInsight documentation][hdinsight-documentation]</span></span>
* <span data-ttu-id="9d666-189">[Komma igång med HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="9d666-189">[Get started with HDInsight][hdinsight-get-started]</span></span>

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
