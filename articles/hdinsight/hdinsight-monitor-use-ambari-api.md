---
title: "Övervaka Hadoop-kluster i HDInsight med Ambari API - Azure | Microsoft Docs"
description: "Använd Apache Ambari APIs för att skapa, hantera och övervaka Hadoop-kluster. Döljer komplexiteten hos Hadoop intuitiva operatorn verktyg och API: er."
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
ms.openlocfilehash: b6fc2098027690eb76b69b1427f0e9541b8a7a69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a><span data-ttu-id="2b54f-104">Övervaka Hadoop-kluster i HDInsight Ambari API</span><span class="sxs-lookup"><span data-stu-id="2b54f-104">Monitor Hadoop clusters in HDInsight using the Ambari API</span></span>
<span data-ttu-id="2b54f-105">Lär dig hur du övervakar HDInsight-kluster med Ambari APIs.</span><span class="sxs-lookup"><span data-stu-id="2b54f-105">Learn how to monitor HDInsight clusters by using Ambari APIs.</span></span>

> [!NOTE]
> <span data-ttu-id="2b54f-106">Informationen i den här artikeln är främst för Windows-baserade HDInsight-kluster som innehåller en skrivskyddad version av Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="2b54f-106">The information in this article is primarily for Windows-based HDInsight clusters, which provide a read-only version of the Ambari REST API.</span></span> <span data-ttu-id="2b54f-107">Linux-baserade kluster, se [hantera Hadoop-kluster med Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="2b54f-107">For Linux-based clusters, see [Manage Hadoop clusters using Ambari](hdinsight-hadoop-manage-ambari.md).</span></span>
> 
> 

## <a name="what-is-ambari"></a><span data-ttu-id="2b54f-108">Vad är Ambari?</span><span class="sxs-lookup"><span data-stu-id="2b54f-108">What is Ambari?</span></span>
<span data-ttu-id="2b54f-109">[Apache Ambari] [ ambari-home] används för etablering, hantering och övervakning av Apache Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="2b54f-109">[Apache Ambari][ambari-home] is used for provisioning, managing, and monitoring Apache Hadoop clusters.</span></span> <span data-ttu-id="2b54f-110">Det innehåller en intuitiv samling operatörsverktyg och en stabil uppsättning API:er som döljer komplexiteten hos Hadoop och förenklar klusteranvändningen.</span><span class="sxs-lookup"><span data-stu-id="2b54f-110">It includes an intuitive collection of operator tools and a robust set of APIs that hide the complexity of Hadoop, simplifying the operation of clusters.</span></span> <span data-ttu-id="2b54f-111">Mer information om API: erna finns [Ambari API-referens][ambari-api-reference].</span><span class="sxs-lookup"><span data-stu-id="2b54f-111">For more information about the APIs, see [Ambari API Reference][ambari-api-reference].</span></span> 

<span data-ttu-id="2b54f-112">HDInsight stöder för närvarande endast övervakningsfunktion Ambari.</span><span class="sxs-lookup"><span data-stu-id="2b54f-112">HDInsight currently supports only the Ambari monitoring feature.</span></span> <span data-ttu-id="2b54f-113">Ambari API 1.0 stöds av version 3.0 och 2.1 HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2b54f-113">Ambari API 1.0 is supported by HDInsight version 3.0 and 2.1 clusters.</span></span> <span data-ttu-id="2b54f-114">Den här artikeln täcker inte att komma åt Ambari APIs på HDInsight version 3.1 och 2.1-kluster.</span><span class="sxs-lookup"><span data-stu-id="2b54f-114">This article covers accessing Ambari APIs on HDInsight version 3.1 and 2.1 clusters.</span></span> <span data-ttu-id="2b54f-115">Den viktigaste skillnaden mellan två är att vissa av komponenterna har ändrats av nya funktioner (t.ex historik jobbserver).</span><span class="sxs-lookup"><span data-stu-id="2b54f-115">The key difference between the two is that some of the components have changed with the introduction of new capabilities (such as the Job History Server).</span></span> 

<span data-ttu-id="2b54f-116">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="2b54f-116">**Prerequisites**</span></span>

<span data-ttu-id="2b54f-117">Innan du börjar den här självstudiekursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="2b54f-117">Before you begin this tutorial, you must have the following items:</span></span>

* <span data-ttu-id="2b54f-118">**En arbetsstation med Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="2b54f-118">**A workstation with Azure PowerShell**.</span></span>
* <span data-ttu-id="2b54f-119">(Valfritt) [cURL][curl].</span><span class="sxs-lookup"><span data-stu-id="2b54f-119">(Optional) [cURL][curl].</span></span> <span data-ttu-id="2b54f-120">Om du vill installera den [cURL versioner och hämtningsbara filer][curl-download].</span><span class="sxs-lookup"><span data-stu-id="2b54f-120">To install it, see [cURL Releases and Downloads][curl-download].</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="2b54f-121">När du använder cURL-kommando i Windows, Använd dubbla citattecken i stället för citattecken för alternativvärden.</span><span class="sxs-lookup"><span data-stu-id="2b54f-121">When use the cURL command in Windows, use double-quotation marks instead of single-quotation marks for the option values.</span></span>
  > 
  > 
* <span data-ttu-id="2b54f-122">**Ett Azure HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="2b54f-122">**An Azure HDInsight cluster**.</span></span> <span data-ttu-id="2b54f-123">Instruktioner om hur klusteretablering finns [komma igång med HDInsight] [ hdinsight-get-started] eller [etablera HDInsight-kluster][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="2b54f-123">For instructions about cluster provisioning, see [Get started using HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="2b54f-124">Du behöver följande data gå igenom kursen:</span><span class="sxs-lookup"><span data-stu-id="2b54f-124">You need the following data to go through the tutorial:</span></span>
  
  | <span data-ttu-id="2b54f-125">Kluster-egenskap</span><span class="sxs-lookup"><span data-stu-id="2b54f-125">Cluster property</span></span> | <span data-ttu-id="2b54f-126">Variabelnamn för Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="2b54f-126">Azure PowerShell variable name</span></span> | <span data-ttu-id="2b54f-127">Värde</span><span class="sxs-lookup"><span data-stu-id="2b54f-127">Value</span></span> | <span data-ttu-id="2b54f-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2b54f-128">Description</span></span> |
  | --- | --- | --- | --- |
  |   <span data-ttu-id="2b54f-129">HDInsight-klustrets namn</span><span class="sxs-lookup"><span data-stu-id="2b54f-129">HDInsight cluster name</span></span> |<span data-ttu-id="2b54f-130">$clusterName</span><span class="sxs-lookup"><span data-stu-id="2b54f-130">$clusterName</span></span> | |<span data-ttu-id="2b54f-131">Namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2b54f-131">The name of your HDInsight cluster.</span></span> |
  |   <span data-ttu-id="2b54f-132">Användarnamn för kluster</span><span class="sxs-lookup"><span data-stu-id="2b54f-132">Cluster username</span></span> |<span data-ttu-id="2b54f-133">$clusterUsername</span><span class="sxs-lookup"><span data-stu-id="2b54f-133">$clusterUsername</span></span> | |<span data-ttu-id="2b54f-134">Användaren klusternamnet anges när klustret skapades.</span><span class="sxs-lookup"><span data-stu-id="2b54f-134">Cluster user name specified when the cluster was created.</span></span> |
  |   <span data-ttu-id="2b54f-135">Kluster-lösenord</span><span class="sxs-lookup"><span data-stu-id="2b54f-135">Cluster password</span></span> |<span data-ttu-id="2b54f-136">$clusterPassword</span><span class="sxs-lookup"><span data-stu-id="2b54f-136">$clusterPassword</span></span> | |<span data-ttu-id="2b54f-137">Användarlösenordet för klustret.</span><span class="sxs-lookup"><span data-stu-id="2b54f-137">Cluster user password.</span></span> |

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


## <a name="jump-start"></a><span data-ttu-id="2b54f-138">Komma igång</span><span class="sxs-lookup"><span data-stu-id="2b54f-138">Jump-start</span></span>
<span data-ttu-id="2b54f-139">Det finns flera sätt att använda Ambari och övervaka HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2b54f-139">There are several ways to use Ambari to monitor HDInsight clusters.</span></span>

<span data-ttu-id="2b54f-140">**Använda Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="2b54f-140">**Use Azure PowerShell**</span></span>

<span data-ttu-id="2b54f-141">Följande Azure PowerShell-skript hämtar MapReduce spåraren jobbinformation *i ett kluster i HDInsight 3.5.*</span><span class="sxs-lookup"><span data-stu-id="2b54f-141">The following Azure PowerShell script gets the MapReduce job tracker information *in an HDInsight 3.5 cluster.*</span></span>  <span data-ttu-id="2b54f-142">Den viktigaste skillnaden är att vi hämtar informationen från den YARN-tjänsten (i stället MapReduce).</span><span class="sxs-lookup"><span data-stu-id="2b54f-142">The key difference is that we pull these details from the YARN service (rather than MapReduce).</span></span>

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName/services/YARN/components/RESOURCEMANAGER"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

<span data-ttu-id="2b54f-143">Följande PowerShell-skript hämtar MapReduce spåraren jobbinformation *i ett kluster i HDInsight 2.1*:</span><span class="sxs-lookup"><span data-stu-id="2b54f-143">The following PowerShell script gets the MapReduce job tracker information *in an HDInsight 2.1 cluster*:</span></span>

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

<span data-ttu-id="2b54f-144">Utdata är:</span><span class="sxs-lookup"><span data-stu-id="2b54f-144">The output is:</span></span>

![Jobtracker utdata][img-jobtracker-output]

<span data-ttu-id="2b54f-146">**Använda cURL**</span><span class="sxs-lookup"><span data-stu-id="2b54f-146">**Use cURL**</span></span>

<span data-ttu-id="2b54f-147">I följande exempel hämtar information om klustret med hjälp av cURL:</span><span class="sxs-lookup"><span data-stu-id="2b54f-147">The following example gets cluster information by using cURL:</span></span>

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

<span data-ttu-id="2b54f-148">Utdata är:</span><span class="sxs-lookup"><span data-stu-id="2b54f-148">The output is:</span></span>

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

<span data-ttu-id="2b54f-149">**2014-10-8-versionen**:</span><span class="sxs-lookup"><span data-stu-id="2b54f-149">**For the 10/8/2014 release**:</span></span>

<span data-ttu-id="2b54f-150">När du använder Ambari-slutpunkten ”https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}” den *värddatornamn* field returnerar det fullständigt kvalificerade domännamnet (FQDN) för noden i stället för värdnamnet.</span><span class="sxs-lookup"><span data-stu-id="2b54f-150">When using the Ambari endpoint, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", the *host_name* field returns the fully qualified domain name (FQDN) of the node instead of the host name.</span></span> <span data-ttu-id="2b54f-151">Före version 2014-10-8, det här exemplet returneras bara ”**headnode0**”.</span><span class="sxs-lookup"><span data-stu-id="2b54f-151">Before the 10/8/2014 release, this example returned simply "**headnode0**".</span></span> <span data-ttu-id="2b54f-152">Efter 2014-10-8-version du hämta det fullständiga Domännamnet ”**headnode0. { ClusterDNS}. azurehdinsight.NET .net**”, som visas i föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="2b54f-152">After the 10/8/2014 release, you get the FQDN "**headnode0.{ClusterDNS}.azurehdinsight.net**", as shown in the previous example.</span></span> <span data-ttu-id="2b54f-153">Den här ändringen krävdes för att underlätta scenarier där flera klustertyper (till exempel HBase och Hadoop) kan distribueras i ett virtuellt nätverk (VNET).</span><span class="sxs-lookup"><span data-stu-id="2b54f-153">This change was required to facilitate scenarios where multiple cluster types (such as HBase and Hadoop) can be deployed in one virtual network (VNET).</span></span> <span data-ttu-id="2b54f-154">Detta inträffar exempelvis när du använder HBase som en backend-plattform för Hadoop.</span><span class="sxs-lookup"><span data-stu-id="2b54f-154">This happens, for example, when using HBase as a back-end platform for Hadoop.</span></span>

## <a name="ambari-monitoring-apis"></a><span data-ttu-id="2b54f-155">Ambari övervakning API: er</span><span class="sxs-lookup"><span data-stu-id="2b54f-155">Ambari monitoring APIs</span></span>
<span data-ttu-id="2b54f-156">I följande tabell visas några av de vanligaste Ambari övervakning API-anrop.</span><span class="sxs-lookup"><span data-stu-id="2b54f-156">The following table lists some of the most common Ambari monitoring API calls.</span></span> <span data-ttu-id="2b54f-157">Mer information om API finns [Ambari API-referens][ambari-api-reference].</span><span class="sxs-lookup"><span data-stu-id="2b54f-157">For more information about the API, see [Ambari API Reference][ambari-api-reference].</span></span>

| <span data-ttu-id="2b54f-158">Övervakare för API-anrop</span><span class="sxs-lookup"><span data-stu-id="2b54f-158">Monitor API call</span></span> | <span data-ttu-id="2b54f-159">URI: N</span><span class="sxs-lookup"><span data-stu-id="2b54f-159">URI</span></span> | <span data-ttu-id="2b54f-160">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2b54f-160">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2b54f-161">Hämta kluster</span><span class="sxs-lookup"><span data-stu-id="2b54f-161">Get clusters</span></span> |`/api/v1/clusters` | |
| <span data-ttu-id="2b54f-162">Hämta klusterinformation.</span><span class="sxs-lookup"><span data-stu-id="2b54f-162">Get cluster info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net` |<span data-ttu-id="2b54f-163">kluster, tjänster, värdar</span><span class="sxs-lookup"><span data-stu-id="2b54f-163">clusters, services, hosts</span></span> |
| <span data-ttu-id="2b54f-164">Hämta tjänster</span><span class="sxs-lookup"><span data-stu-id="2b54f-164">Get services</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services` |<span data-ttu-id="2b54f-165">Tjänsterna omfattar: hdfs, mapreduce</span><span class="sxs-lookup"><span data-stu-id="2b54f-165">Services include: hdfs, mapreduce</span></span> |
| <span data-ttu-id="2b54f-166">Hämta information för tjänster.</span><span class="sxs-lookup"><span data-stu-id="2b54f-166">Get services info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>` | |
| <span data-ttu-id="2b54f-167">Hämta tjänstkomponenter</span><span class="sxs-lookup"><span data-stu-id="2b54f-167">Get service components</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components` |<span data-ttu-id="2b54f-168">HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker</span><span class="sxs-lookup"><span data-stu-id="2b54f-168">HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker</span></span> |
| <span data-ttu-id="2b54f-169">Hämta information för komponenten.</span><span class="sxs-lookup"><span data-stu-id="2b54f-169">Get component info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>` |<span data-ttu-id="2b54f-170">ServiceComponentInfo värd-komponenter, mått</span><span class="sxs-lookup"><span data-stu-id="2b54f-170">ServiceComponentInfo, host-components, metrics</span></span> |
| <span data-ttu-id="2b54f-171">Hämta värdar</span><span class="sxs-lookup"><span data-stu-id="2b54f-171">Get hosts</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts` |<span data-ttu-id="2b54f-172">headnode0 workernode0</span><span class="sxs-lookup"><span data-stu-id="2b54f-172">headnode0, workernode0</span></span> |
| <span data-ttu-id="2b54f-173">Hämta information om värden.</span><span class="sxs-lookup"><span data-stu-id="2b54f-173">Get host info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>` | |
| <span data-ttu-id="2b54f-174">Hämta värden komponenter</span><span class="sxs-lookup"><span data-stu-id="2b54f-174">Get host components</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components` |<span data-ttu-id="2b54f-175">namenode, resourcemanager</span><span class="sxs-lookup"><span data-stu-id="2b54f-175">namenode, resourcemanager</span></span> |
| <span data-ttu-id="2b54f-176">Hämta värden komponenten information.</span><span class="sxs-lookup"><span data-stu-id="2b54f-176">Get host component info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>` |<span data-ttu-id="2b54f-177">HostRoles komponent, värd, mått</span><span class="sxs-lookup"><span data-stu-id="2b54f-177">HostRoles, component, host, metrics</span></span> |
| <span data-ttu-id="2b54f-178">Hämta konfigurationer</span><span class="sxs-lookup"><span data-stu-id="2b54f-178">Get configurations</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations` |<span data-ttu-id="2b54f-179">Config-typer: core-plats, hdfs-plats, mapred-plats, hive-plats</span><span class="sxs-lookup"><span data-stu-id="2b54f-179">Config types: core-site, hdfs-site, mapred-site, hive-site</span></span> |
| <span data-ttu-id="2b54f-180">Hämta konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="2b54f-180">Get configuration info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>` |<span data-ttu-id="2b54f-181">Config-typer: core-plats, hdfs-plats, mapred-plats, hive-plats</span><span class="sxs-lookup"><span data-stu-id="2b54f-181">Config types: core-site, hdfs-site, mapred-site, hive-site</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2b54f-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b54f-182">Next Steps</span></span>
<span data-ttu-id="2b54f-183">Nu har du lärt dig hur du använder Ambari övervakning API-anrop.</span><span class="sxs-lookup"><span data-stu-id="2b54f-183">Now you have learned how to use Ambari monitoring API calls.</span></span> <span data-ttu-id="2b54f-184">Du kan läsa mer här:</span><span class="sxs-lookup"><span data-stu-id="2b54f-184">To learn more, see:</span></span>

* <span data-ttu-id="2b54f-185">[Hantera HDInsight-kluster med Azure-portalen][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="2b54f-185">[Manage HDInsight clusters using the Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="2b54f-186">[Hantera HDInsight-kluster med Azure PowerShell][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="2b54f-186">[Manage HDInsight clusters using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="2b54f-187">[Hantera HDInsight-kluster med hjälp av kommandoradsgränssnittet][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="2b54f-187">[Manage HDInsight clusters using command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="2b54f-188">[HDInsight-dokumentation][hdinsight-documentation]</span><span class="sxs-lookup"><span data-stu-id="2b54f-188">[HDInsight documentation][hdinsight-documentation]</span></span>
* <span data-ttu-id="2b54f-189">[Komma igång med HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="2b54f-189">[Get started with HDInsight][hdinsight-get-started]</span></span>

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
