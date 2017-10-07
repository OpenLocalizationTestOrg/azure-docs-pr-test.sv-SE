---
title: aaaMonitor och hantera Hadoop med Ambari REST API - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Ambari toomonitor och hantera Hadoop-kluster i Azure HDInsight. I detta dokument kan du lära dig hur toouse hello Ambari REST API som ingår i HDInsight-kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2400530f-92b3-47b7-aa48-875f028765ff
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: 1866a77c8e402231bccbcfba7174253aca41339b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hdinsight-clusters-by-using-hello-ambari-rest-api"></a><span data-ttu-id="acf7d-104">Hantera HDInsight-kluster med hjälp av hello Ambari REST API</span><span class="sxs-lookup"><span data-stu-id="acf7d-104">Manage HDInsight clusters by using hello Ambari REST API</span></span>

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

<span data-ttu-id="acf7d-105">Lär dig hur toouse hello Ambari REST API toomanage och övervaka Hadoop-kluster i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="acf7d-105">Learn how toouse hello Ambari REST API toomanage and monitor Hadoop clusters in Azure HDInsight.</span></span>

<span data-ttu-id="acf7d-106">Apache Ambari förenklar hello hantering och övervakning av ett Hadoop-kluster genom att tillhandahålla en enkel toouse web användargränssnitt och REST API.</span><span class="sxs-lookup"><span data-stu-id="acf7d-106">Apache Ambari simplifies hello management and monitoring of a Hadoop cluster by providing an easy toouse web UI and REST API.</span></span> <span data-ttu-id="acf7d-107">Ambari ingår i HDInsight-kluster som använder hello Linux-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="acf7d-107">Ambari is included on HDInsight clusters that use hello Linux operating system.</span></span> <span data-ttu-id="acf7d-108">Du kan använda Ambari toomonitor hello klustret och gör ändringar i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="acf7d-108">You can use Ambari toomonitor hello cluster and make configuration changes.</span></span>

## <span data-ttu-id="acf7d-109"><a id="whatis"></a>Vad är Ambari</span><span class="sxs-lookup"><span data-stu-id="acf7d-109"><a id="whatis"></a>What is Ambari</span></span>

<span data-ttu-id="acf7d-110">[Apache Ambari](http://ambari.apache.org) ger webbgränssnitt som kan använda tooprovision, hantera och övervaka Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="acf7d-110">[Apache Ambari](http://ambari.apache.org) provides web UI that can be used tooprovision, manage, and monitor Hadoop clusters.</span></span> <span data-ttu-id="acf7d-111">Utvecklare kan integrera dessa funktioner i sina program med hjälp av hello [Ambari REST API: er](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="acf7d-111">Developers can integrate these capabilities into their applications by using hello [Ambari REST APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

<span data-ttu-id="acf7d-112">Ambari är som standard med Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="acf7d-112">Ambari is provided by default with Linux-based HDInsight clusters.</span></span>

## <a name="how-toouse-hello-ambari-rest-api"></a><span data-ttu-id="acf7d-113">Hur toouse hello Ambari REST API</span><span class="sxs-lookup"><span data-stu-id="acf7d-113">How toouse hello Ambari REST API</span></span>

> [!IMPORTANT]
> <span data-ttu-id="acf7d-114">hello information och exempel i det här dokumentet kräver ett HDInsight-kluster som använder Linux-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="acf7d-114">hello information and examples in this document require an HDInsight cluster that uses Linux operating system.</span></span> <span data-ttu-id="acf7d-115">Mer information finns i [komma igång med HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="acf7d-115">For more information, see [Get started with HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

<span data-ttu-id="acf7d-116">hello exemplen i det här dokumentet tillhandahålls för både hello Bourne shell (bash) och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="acf7d-116">hello examples in this document are provided for both hello Bourne shell (bash) and PowerShell.</span></span> <span data-ttu-id="acf7d-117">hello bash exempel har testats med GNU bash 4.3.11, men bör arbeta tillsammans med andra Unix-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="acf7d-117">hello bash examples were tested with GNU bash 4.3.11, but should work with other Unix shells.</span></span> <span data-ttu-id="acf7d-118">hello PowerShell-exemplen har testats med PowerShell 5.0, men ska fungera med PowerShell 3.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="acf7d-118">hello PowerShell examples were tested with PowerShell 5.0, but should work with PowerShell 3.0 or higher.</span></span>

<span data-ttu-id="acf7d-119">Om du använder hello __Bourne shell__ (Bash), måste du ha hello följande installerat:</span><span class="sxs-lookup"><span data-stu-id="acf7d-119">If using hello __Bourne shell__ (Bash), you must have hello following installed:</span></span>

* <span data-ttu-id="acf7d-120">[cURL](http://curl.haxx.se/): cURL är ett verktyg som kan använda toowork med REST API: er från hello kommandorad.</span><span class="sxs-lookup"><span data-stu-id="acf7d-120">[cURL](http://curl.haxx.se/): cURL is a utility that can be used toowork with REST APIs from hello command line.</span></span> <span data-ttu-id="acf7d-121">I detta dokument är det använda toocommunicate med hello Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="acf7d-121">In this document, it is used toocommunicate with hello Ambari REST API.</span></span>

<span data-ttu-id="acf7d-122">Om du använder Bash eller PowerShell, du måste ha [jq](https://stedolan.github.io/jq/) installerad.</span><span class="sxs-lookup"><span data-stu-id="acf7d-122">Whether using Bash or PowerShell, you must also have [jq](https://stedolan.github.io/jq/) installed.</span></span> <span data-ttu-id="acf7d-123">Jq är ett verktyg för att arbeta med JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="acf7d-123">Jq is a utility for working with JSON documents.</span></span> <span data-ttu-id="acf7d-124">Den används i **alla** hello Bash exempel och **en** av hello PowerShell-exemplen.</span><span class="sxs-lookup"><span data-stu-id="acf7d-124">It is used in **all** hello Bash examples, and **one** of hello PowerShell examples.</span></span>

### <a name="base-uri-for-ambari-rest-api"></a><span data-ttu-id="acf7d-125">Bas-URI för Ambari Rest API</span><span class="sxs-lookup"><span data-stu-id="acf7d-125">Base URI for Ambari Rest API</span></span>

<span data-ttu-id="acf7d-126">hello bas-URI för hello Ambari REST API på HDInsight är https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, där **KLUSTERNAMN** är hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="acf7d-126">hello base URI for hello Ambari REST API on HDInsight is https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, where **CLUSTERNAME** is hello name of your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="acf7d-127">Medan hello klusternamnet i hello fullständigt kvalificerade namn (FQDN) domändelen av hello URI (CLUSTERNAME.azurehdinsight.net) är skiftlägeskänslig, andra förekomster i hello URI är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="acf7d-127">While hello cluster name in hello fully qualified domain name (FQDN) part of hello URI (CLUSTERNAME.azurehdinsight.net) is case-insensitive, other occurrences in hello URI are case-sensitive.</span></span> <span data-ttu-id="acf7d-128">Om klustret har namnet till exempel `MyCluster`, hello följande är giltiga URI: er:</span><span class="sxs-lookup"><span data-stu-id="acf7d-128">For example, if your cluster is named `MyCluster`, hello following are valid URIs:</span></span>
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> <span data-ttu-id="acf7d-129">hello följande URI returneras ett fel eftersom hello andra förekomsten av hello namn inte hello korrigera fallet.</span><span class="sxs-lookup"><span data-stu-id="acf7d-129">hello following URIs return an error because hello second occurrence of hello name is not hello correct case.</span></span>
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a><span data-ttu-id="acf7d-130">Autentisering</span><span class="sxs-lookup"><span data-stu-id="acf7d-130">Authentication</span></span>

<span data-ttu-id="acf7d-131">Ansluta tooAmbari på HDInsight kräver HTTPS.</span><span class="sxs-lookup"><span data-stu-id="acf7d-131">Connecting tooAmbari on HDInsight requires HTTPS.</span></span> <span data-ttu-id="acf7d-132">Använd hello admin kontonamn (hello standardvärdet är **admin**) och lösenord som du angav när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="acf7d-132">Use hello admin account name (hello default is **admin**) and password you provided during cluster creation.</span></span>

## <a name="examples-authentication-and-parsing-json"></a><span data-ttu-id="acf7d-133">Exempel: Autentisering och parsa JSON</span><span class="sxs-lookup"><span data-stu-id="acf7d-133">Examples: Authentication and parsing JSON</span></span>

<span data-ttu-id="acf7d-134">hello som följande exempel visar hur toomake en GET-begäran mot hello basera Ambari REST API:</span><span class="sxs-lookup"><span data-stu-id="acf7d-134">hello following examples demonstrate how toomake a GET request against hello base Ambari REST API:</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> <span data-ttu-id="acf7d-135">hello Bash exemplen i det här dokumentet att Hej följande förutsättningar:</span><span class="sxs-lookup"><span data-stu-id="acf7d-135">hello Bash examples in this document make hello following assumptions:</span></span>
>
> * <span data-ttu-id="acf7d-136">hello inloggningsnamn för hello kluster är hello standardvärdet för `admin`.</span><span class="sxs-lookup"><span data-stu-id="acf7d-136">hello login name for hello cluster is hello default value of `admin`.</span></span>
> * <span data-ttu-id="acf7d-137">`$PASSWORD`innehåller hello lösenord för hello HDInsight inloggningen kommando.</span><span class="sxs-lookup"><span data-stu-id="acf7d-137">`$PASSWORD` contains hello password for hello HDInsight login command.</span></span> <span data-ttu-id="acf7d-138">Du kan ange ett värde med hjälp av `PASSWORD='mypassword'`.</span><span class="sxs-lookup"><span data-stu-id="acf7d-138">You can set this value by using `PASSWORD='mypassword'`.</span></span>
> * <span data-ttu-id="acf7d-139">`$CLUSTERNAME`innehåller hello hello klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="acf7d-139">`$CLUSTERNAME` contains hello name of hello cluster.</span></span> <span data-ttu-id="acf7d-140">Du kan ange ett värde med hjälp av`set CLUSTERNAME='clustername'`</span><span class="sxs-lookup"><span data-stu-id="acf7d-140">You can set this value by using `set CLUSTERNAME='clustername'`</span></span>

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> <span data-ttu-id="acf7d-141">hello PowerShell-exemplen i det här dokumentet att Hej följande förutsättningar:</span><span class="sxs-lookup"><span data-stu-id="acf7d-141">hello PowerShell examples in this document make hello following assumptions:</span></span>
>
> * <span data-ttu-id="acf7d-142">`$creds`är ett autentiseringsuppgiftobjekt som innehåller hello admin inloggningsnamn och lösenord för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="acf7d-142">`$creds` is a credential object that contains hello admin login and password for hello cluster.</span></span> <span data-ttu-id="acf7d-143">Du kan ange ett värde med hjälp av `$creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"` och ge hello autentiseringsuppgifter när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="acf7d-143">You can set this value by using `$creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"` and providing hello credentials when prompted.</span></span>
> * <span data-ttu-id="acf7d-144">`$clusterName`är en sträng som innehåller hello hello klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="acf7d-144">`$clusterName` is a string that contains hello name of hello cluster.</span></span> <span data-ttu-id="acf7d-145">Du kan ange ett värde med hjälp av `$clusterName="clustername"`.</span><span class="sxs-lookup"><span data-stu-id="acf7d-145">You can set this value by using `$clusterName="clustername"`.</span></span>

<span data-ttu-id="acf7d-146">Båda exempel returnera ett JSON-dokument som börjar med liknande information toohello som följande exempel:</span><span class="sxs-lookup"><span data-stu-id="acf7d-146">Both examples return a JSON document that begins with information similar toohello following example:</span></span>

```json
{
"href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
"Clusters" : {
    "cluster_id" : 2,
    "cluster_name" : "CLUSTERNAME",
    "health_report" : {
    "Host/stale_config" : 0,
    "Host/maintenance_state" : 0,
    "Host/host_state/HEALTHY" : 7,
    "Host/host_state/UNHEALTHY" : 0,
    "Host/host_state/HEARTBEAT_LOST" : 0,
    "Host/host_state/INIT" : 0,
    "Host/host_status/HEALTHY" : 7,
    "Host/host_status/UNHEALTHY" : 0,
    "Host/host_status/UNKNOWN" : 0,
    "Host/host_status/ALERT" : 0
    ...
```

### <a name="parsing-json-data"></a><span data-ttu-id="acf7d-147">Parsning av JSON-data</span><span class="sxs-lookup"><span data-stu-id="acf7d-147">Parsing JSON data</span></span>

<span data-ttu-id="acf7d-148">hello följande exempel används `jq` tooparse hello svar JSON-dokumentet och visa endast hello `health_report` information från hello resultat.</span><span class="sxs-lookup"><span data-stu-id="acf7d-148">hello following example uses `jq` tooparse hello JSON response document and display only hello `health_report` information from hello results.</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

<span data-ttu-id="acf7d-149">PowerShell 3.0 och senare innehåller hello `ConvertFrom-Json` cmdlet, som konverterar hello JSON-dokumentet till ett objekt som är enklare toowork med från PowerShell.</span><span class="sxs-lookup"><span data-stu-id="acf7d-149">PowerShell 3.0 and higher provides hello `ConvertFrom-Json` cmdlet, which converts hello JSON document into an object that is easier toowork with from PowerShell.</span></span> <span data-ttu-id="acf7d-150">hello följande exempel används `ConvertFrom-Json` toodisplay endast hello `health_report` information från hello resultat.</span><span class="sxs-lookup"><span data-stu-id="acf7d-150">hello following example uses `ConvertFrom-Json` toodisplay only hello `health_report` information from hello results.</span></span>

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> <span data-ttu-id="acf7d-151">När de flesta exemplen i det här dokumentet används `ConvertFrom-Json` toodisplay element från hello svarsdokument hello [uppdatering Ambari configuration](#example-update-ambari-configuration) exempel används jq.</span><span class="sxs-lookup"><span data-stu-id="acf7d-151">While most examples in this document use `ConvertFrom-Json` toodisplay elements from hello response document, hello [Update Ambari configuration](#example-update-ambari-configuration) example uses jq.</span></span> <span data-ttu-id="acf7d-152">Jq används i det här exemplet tooconstruct en ny mall från svar hello JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="acf7d-152">Jq is used in this example tooconstruct a new template from hello JSON response document.</span></span>

<span data-ttu-id="acf7d-153">En fullständig hello REST API-referens, se [Ambari API-referens V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="acf7d-153">For a complete reference of hello REST API, see [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

## <a name="example-get-hello-fqdn-of-cluster-nodes"></a><span data-ttu-id="acf7d-154">Exempel: Hämta hello FQDN för klusternoder</span><span class="sxs-lookup"><span data-stu-id="acf7d-154">Example: Get hello FQDN of cluster nodes</span></span>

<span data-ttu-id="acf7d-155">När du arbetar med HDInsight kan behöva tooknow hello fullständigt kvalificerade domännamnet (FQDN) på en klusternod.</span><span class="sxs-lookup"><span data-stu-id="acf7d-155">When working with HDInsight, you may need tooknow hello fully qualified domain name (FQDN) of a cluster node.</span></span> <span data-ttu-id="acf7d-156">Du kan enkelt hämta hello FQDN för hello olika noder i hello-kluster med hjälp av hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="acf7d-156">You can easily retrieve hello FQDN for hello various nodes in hello cluster using hello following examples:</span></span>

* <span data-ttu-id="acf7d-157">**Alla noder**</span><span class="sxs-lookup"><span data-stu-id="acf7d-157">**All nodes**</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" \
    | jq '.items[].Hosts.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.Hosts.host_name
    ```

* <span data-ttu-id="acf7d-158">**HEAD-noder**</span><span class="sxs-lookup"><span data-stu-id="acf7d-158">**Head nodes**</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HDFS/components/NAMENODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* <span data-ttu-id="acf7d-159">**Arbetsnoder**</span><span class="sxs-lookup"><span data-stu-id="acf7d-159">**Worker nodes**</span></span>

    ```bash
    curl -u admin:PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* <span data-ttu-id="acf7d-160">**Zookeeper-noder**</span><span class="sxs-lookup"><span data-stu-id="acf7d-160">**Zookeeper nodes**</span></span>

    ```bash
    curl -u admin:PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

## <a name="example-get-hello-internal-ip-address-of-cluster-nodes"></a><span data-ttu-id="acf7d-161">Exempel: Hämta hello interna IP-adressen för klusternoder</span><span class="sxs-lookup"><span data-stu-id="acf7d-161">Example: Get hello internal IP address of cluster nodes</span></span>

> [!IMPORTANT]
> <span data-ttu-id="acf7d-162">hello IP-adresser som returneras av hello exemplen i det här avsnittet är inte direkt tillgänglig över hello internet.</span><span class="sxs-lookup"><span data-stu-id="acf7d-162">hello IP addresses returned by hello examples in this section are not directly accessible over hello internet.</span></span> <span data-ttu-id="acf7d-163">De är bara tillgängliga i hello Azure Virtual Network som innehåller hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="acf7d-163">They are only accessible within hello Azure Virtual Network that contains hello HDInsight cluster.</span></span>
>
> <span data-ttu-id="acf7d-164">Mer information om att arbeta med HDInsight och virtuella nätverk finns [utöka HDInsight funktioner med hjälp av en anpassad Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="acf7d-164">For more information on working with HDInsight and virtual networks, see [Extend HDInsight capabilities by using a custom Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).</span></span>

<span data-ttu-id="acf7d-165">toofind hello IP-adress, måste du veta hello interna fullständigt kvalificerade domännamnet (FQDN) för hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="acf7d-165">toofind hello IP address, you must know hello internal fully qualified domain name (FQDN) of hello cluster nodes.</span></span> <span data-ttu-id="acf7d-166">När du har hello FQDN, kan du hämta hello hello värdens IP-adress.</span><span class="sxs-lookup"><span data-stu-id="acf7d-166">Once you have hello FQDN, you can then get hello IP address of hello host.</span></span> <span data-ttu-id="acf7d-167">hello följande exempel först fråga Ambari för hello FQDN för alla noder i hello-värden och fråga Ambari för hello IP-adress för varje värd.</span><span class="sxs-lookup"><span data-stu-id="acf7d-167">hello following examples first query Ambari for hello FQDN of all hello host nodes, then query Ambari for hello IP address of each host.</span></span>

```bash
for HOSTNAME in $(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```

```powershell
$uri = "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts"
$resp = Invoke-WebRequest -Uri $uri -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
foreach($item in $respObj.items) {
    $hostName = [string]$item.Hosts.host_name
    $hostInfoResp = Invoke-WebRequest -Uri "$uri/$hostName" `
        -Credential $creds
    $hostInfoObj = ConvertFrom-Json $hostInfoResp 
    $hostIp = $hostInfoObj.Hosts.ip
    "$hostName <--> $hostIp"
}
```

## <a name="example-get-hello-default-storage"></a><span data-ttu-id="acf7d-168">Exempel: Hämta hello standardlagring</span><span class="sxs-lookup"><span data-stu-id="acf7d-168">Example: Get hello default storage</span></span>

<span data-ttu-id="acf7d-169">När du skapar ett HDInsight-kluster måste du använda ett Azure Storage-konto eller ett Data Lake Store som hello standardlagring för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="acf7d-169">When you create an HDInsight cluster, you must use an Azure Storage Account or Data Lake Store as hello default storage for hello cluster.</span></span> <span data-ttu-id="acf7d-170">Du kan använda Ambari tooretrieve informationen efter hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="acf7d-170">You can use Ambari tooretrieve this information after hello cluster has been created.</span></span> <span data-ttu-id="acf7d-171">Till exempel om du vill tooread/skriva data toohello behållare utanför HDInsight.</span><span class="sxs-lookup"><span data-stu-id="acf7d-171">For example, if you want tooread/write data toohello container outside HDInsight.</span></span>

<span data-ttu-id="acf7d-172">hello hämtar följande exempel hello standardkonfigurationen för lagring från hello klustret:</span><span class="sxs-lookup"><span data-stu-id="acf7d-172">hello following examples retrieve hello default storage configuration from hello cluster:</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]
> <span data-ttu-id="acf7d-173">De här exemplen returnera hello första tillämpas toohello konfigurationsservern (`service_config_version=1`) som innehåller den här informationen.</span><span class="sxs-lookup"><span data-stu-id="acf7d-173">These examples return hello first configuration applied toohello server (`service_config_version=1`) which contains this information.</span></span> <span data-ttu-id="acf7d-174">Om du hämtar ett värde som har ändrats efter att klustret har skapats kan du behöver toolist hello konfigurationsversionerna och hämta hello senaste.</span><span class="sxs-lookup"><span data-stu-id="acf7d-174">If you retrieve a value that has been modified after cluster creation, you may need toolist hello configuration versions and retrieve hello latest one.</span></span>

<span data-ttu-id="acf7d-175">hello returvärdet är liknande tooone av hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="acf7d-175">hello return value is similar tooone of hello following examples:</span></span>

* <span data-ttu-id="acf7d-176">`wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net`-Det här värdet anger hello klustret använder ett Azure Storage-konto för standardlagring.</span><span class="sxs-lookup"><span data-stu-id="acf7d-176">`wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net` - This value indicates that hello cluster is using an Azure Storage account for default storage.</span></span> <span data-ttu-id="acf7d-177">Hej `ACCOUNTNAME` värdet är hello namnet på hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="acf7d-177">hello `ACCOUNTNAME` value is hello name of hello storage account.</span></span> <span data-ttu-id="acf7d-178">Hej `CONTAINER` del är hello namnet på hello blob-behållaren i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="acf7d-178">hello `CONTAINER` portion is hello name of hello blob container in hello storage account.</span></span> <span data-ttu-id="acf7d-179">hello-behållaren är hello roten hello HDFS kompatibel lagringen för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="acf7d-179">hello container is hello root of hello HDFS compatible storage for hello cluster.</span></span>

* <span data-ttu-id="acf7d-180">`adl://home`-Det här värdet anger hello klustret använder en Azure Data Lake Store för standardlagring.</span><span class="sxs-lookup"><span data-stu-id="acf7d-180">`adl://home` - This value indicates that hello cluster is using an Azure Data Lake Store for default storage.</span></span>

    <span data-ttu-id="acf7d-181">toofind hello Data Lake Store kontonamn, Använd hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="acf7d-181">toofind hello Data Lake Store account name, use hello following examples:</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    <span data-ttu-id="acf7d-182">hello returvärdet är liknande för`ACCOUNTNAME.azuredatalakestore.net`, där `ACCOUNTNAME` är hello namnet på hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="acf7d-182">hello return value is similar too`ACCOUNTNAME.azuredatalakestore.net`, where `ACCOUNTNAME` is hello name of hello Data Lake Store account.</span></span>

    <span data-ttu-id="acf7d-183">toofind hello katalog i Data Lake Store som innehåller hello lagringen för hello klustret, Använd hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="acf7d-183">toofind hello directory within Data Lake Store that contains hello storage for hello cluster, use hello following examples:</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    <span data-ttu-id="acf7d-184">hello returvärdet är liknande för`/clusters/CLUSTERNAME/`.</span><span class="sxs-lookup"><span data-stu-id="acf7d-184">hello return value is similar too`/clusters/CLUSTERNAME/`.</span></span> <span data-ttu-id="acf7d-185">Det här värdet är en sökväg i hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="acf7d-185">This value is a path within hello Data Lake Store account.</span></span> <span data-ttu-id="acf7d-186">Den här sökvägen är hello roten hello HDFS-kompatibla filsystem för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="acf7d-186">This path is hello root of hello HDFS compatible file system for hello cluster.</span></span> 

> [!NOTE]
> <span data-ttu-id="acf7d-187">Hej `Get-AzureRmHDInsightCluster` cmdlet som tillhandahålls av [Azure PowerShell](/powershell/azure/overview) också returnerar hello storage-informationen för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="acf7d-187">hello `Get-AzureRmHDInsightCluster` cmdlet provided by [Azure PowerShell](/powershell/azure/overview) also returns hello storage information for hello cluster.</span></span>


## <a name="example-get-configuration"></a><span data-ttu-id="acf7d-188">Exempel: Get-konfiguration</span><span class="sxs-lookup"><span data-stu-id="acf7d-188">Example: Get configuration</span></span>

1. <span data-ttu-id="acf7d-189">Hämta hello-konfigurationer som är tillgängliga för klustret.</span><span class="sxs-lookup"><span data-stu-id="acf7d-189">Get hello configurations that are available for your cluster.</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    <span data-ttu-id="acf7d-190">Det här exemplet returnerar ett JSON-dokument som innehåller aktuella hello-konfiguration (identifieras av hello *taggen* värde) för hello-komponenter installeras på hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="acf7d-190">This example returns a JSON document containing hello current configuration (identified by hello *tag* value) for hello components installed on hello cluster.</span></span> <span data-ttu-id="acf7d-191">hello är följande exempel ett utdrag ur hello data som returneras från en typ av Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="acf7d-191">hello following example is an excerpt from hello data returned from a Spark cluster type.</span></span>
   
   ```json
   "spark-metrics-properties" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-fairscheduler" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-sparkconf" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   }
   ```

2. <span data-ttu-id="acf7d-192">Få hello-konfiguration för hello-komponent som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="acf7d-192">Get hello configuration for hello component that you are interested in.</span></span> <span data-ttu-id="acf7d-193">Följande exempel Ersätt i hello `INITIAL` med hello Taggvärde som returneras från hello tidigare begäran.</span><span class="sxs-lookup"><span data-stu-id="acf7d-193">In hello following example, replace `INITIAL` with hello tag value returned from hello previous request.</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    <span data-ttu-id="acf7d-194">Det här exemplet returnerar ett JSON-dokument som innehåller hello aktuella konfiguration för hello `core-site` komponent.</span><span class="sxs-lookup"><span data-stu-id="acf7d-194">This example returns a JSON document containing hello current configuration for hello `core-site` component.</span></span>

## <a name="example-update-configuration"></a><span data-ttu-id="acf7d-195">Exempel: Uppdateringskonfiguration</span><span class="sxs-lookup"><span data-stu-id="acf7d-195">Example: Update configuration</span></span>

1. <span data-ttu-id="acf7d-196">Hämta hello aktuella konfigurationen som Ambari lagrar som hello ”desired configuration”:</span><span class="sxs-lookup"><span data-stu-id="acf7d-196">Get hello current configuration, which Ambari stores as hello "desired configuration":</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    <span data-ttu-id="acf7d-197">Det här exemplet returnerar ett JSON-dokument som innehåller aktuella hello-konfiguration (identifieras av hello *taggen* värde) för hello-komponenter installeras på hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="acf7d-197">This example returns a JSON document containing hello current configuration (identified by hello *tag* value) for hello components installed on hello cluster.</span></span> <span data-ttu-id="acf7d-198">hello är följande exempel ett utdrag ur hello data som returneras från en typ av Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="acf7d-198">hello following example is an excerpt from hello data returned from a Spark cluster type.</span></span>
   
    ```json
    "spark-metrics-properties" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-fairscheduler" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-sparkconf" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    }
    ```
   
    <span data-ttu-id="acf7d-199">I den här listan måste toocopy hello namnet på hello-komponenten (till exempel **spark\_thrift\_sparkconf** och hello **taggen** värde.</span><span class="sxs-lookup"><span data-stu-id="acf7d-199">From this list, you need toocopy hello name of hello component (for example, **spark\_thrift\_sparkconf** and hello **tag** value.</span></span>

2. <span data-ttu-id="acf7d-200">Hämta hello konfiguration för hello komponent och tagg med hjälp av hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="acf7d-200">Retrieve hello configuration for hello component and tag by using hello following commands:</span></span>
   
    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" \
    | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    ```powershell
    $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
    $now = Get-Date
    $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=spark-thrift-sparkconf&tag=INITIAL" `
        -Credential $creds
    $resp.Content | jq --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    > [!NOTE]
    > <span data-ttu-id="acf7d-201">Ersätt **spark-thrift-sparkconf** och **inledande** med hello-komponenten och tagg som du vill tooretrieve hello konfiguration för.</span><span class="sxs-lookup"><span data-stu-id="acf7d-201">Replace **spark-thrift-sparkconf** and **INITIAL** with hello component and tag that you want tooretrieve hello configuration for.</span></span>
   
    <span data-ttu-id="acf7d-202">Jq är används tooturn hello data som hämtats från HDInsight till en ny konfigurationsmall för.</span><span class="sxs-lookup"><span data-stu-id="acf7d-202">Jq is used tooturn hello data retrieved from HDInsight into a new configuration template.</span></span> <span data-ttu-id="acf7d-203">Mer specifikt utför de här exemplen hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="acf7d-203">Specifically, these examples perform hello following actions:</span></span>
   
    * <span data-ttu-id="acf7d-204">Skapar ett unikt värde som innehåller hello strängen ”version” och hello datum, som lagras i `newtag`.</span><span class="sxs-lookup"><span data-stu-id="acf7d-204">Creates a unique value containing hello string "version" and hello date, which is stored in `newtag`.</span></span>

    * <span data-ttu-id="acf7d-205">Skapar ett rot-dokument för hello nya önskad konfiguration.</span><span class="sxs-lookup"><span data-stu-id="acf7d-205">Creates a root document for hello new desired configuration.</span></span>

    * <span data-ttu-id="acf7d-206">Hämtar hello innehållet i hello `.items[]` matrisen och lägger till den under hello **desired_config** element.</span><span class="sxs-lookup"><span data-stu-id="acf7d-206">Gets hello contents of hello `.items[]` array and adds it under hello **desired_config** element.</span></span>

    * <span data-ttu-id="acf7d-207">Tar bort hello `href`, `version`, och `Config` element, som de här elementen inte behövs toosubmit en ny konfiguration.</span><span class="sxs-lookup"><span data-stu-id="acf7d-207">Deletes hello `href`, `version`, and `Config` elements, as these elements aren't needed toosubmit a new configuration.</span></span>

    * <span data-ttu-id="acf7d-208">Lägger till en `tag` element med värdet `version#################`.</span><span class="sxs-lookup"><span data-stu-id="acf7d-208">Adds a `tag` element with a value of `version#################`.</span></span> <span data-ttu-id="acf7d-209">Hej sifferdelen baseras på hello aktuellt datum.</span><span class="sxs-lookup"><span data-stu-id="acf7d-209">hello numeric portion is based on hello current date.</span></span> <span data-ttu-id="acf7d-210">Varje konfiguration måste ha en unik tagg.</span><span class="sxs-lookup"><span data-stu-id="acf7d-210">Each configuration must have a unique tag.</span></span>
     
    <span data-ttu-id="acf7d-211">Slutligen hello data sparas toohello `newconfig.json` dokumentet.</span><span class="sxs-lookup"><span data-stu-id="acf7d-211">Finally, hello data is saved toohello `newconfig.json` document.</span></span> <span data-ttu-id="acf7d-212">hello dokumentstruktur ska se ut ungefär toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="acf7d-212">hello document structure should appear similar toohello following example:</span></span>
     
     ```json
    {
        "Clusters": {
            "desired_config": {
            "tag": "version1459260185774265400",
            "type": "spark-thrift-sparkconf",
            "properties": {
                ....
            },
            "properties_attributes": {
                ....
            }
        }
    }
    ```

3. <span data-ttu-id="acf7d-213">Öppna hello `newconfig.json` dokument och ändra/Lägg till värden i hello `properties` objekt.</span><span class="sxs-lookup"><span data-stu-id="acf7d-213">Open hello `newconfig.json` document and modify/add values in hello `properties` object.</span></span> <span data-ttu-id="acf7d-214">hello följande exempel ändringar hello värdet för `"spark.yarn.am.memory"` från `"1g"` för`"3g"`.</span><span class="sxs-lookup"><span data-stu-id="acf7d-214">hello following example changes hello value of `"spark.yarn.am.memory"` from `"1g"` too`"3g"`.</span></span> <span data-ttu-id="acf7d-215">Det ger också `"spark.kryoserializer.buffer.max"` med värdet `"256m"`.</span><span class="sxs-lookup"><span data-stu-id="acf7d-215">It also adds `"spark.kryoserializer.buffer.max"` with a value of `"256m"`.</span></span>
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    <span data-ttu-id="acf7d-216">Spara hello-filen när du är klar med ändringarna.</span><span class="sxs-lookup"><span data-stu-id="acf7d-216">Save hello file once you are done making modifications.</span></span>

4. <span data-ttu-id="acf7d-217">Använd följande kommandon toosubmit hello uppdateras configuration tooAmbari hello.</span><span class="sxs-lookup"><span data-stu-id="acf7d-217">Use hello following commands toosubmit hello updated configuration tooAmbari.</span></span>
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
    ```

    ```powershell
    $newConfig = Get-Content .\newconfig.json
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body $newConfig
    $resp.Content
    ```
   
    <span data-ttu-id="acf7d-218">Dessa kommandon skicka hello innehållet i hello **newconfig.json** filen toohello klustret som hello nya önskad konfiguration.</span><span class="sxs-lookup"><span data-stu-id="acf7d-218">These commands submit hello contents of hello **newconfig.json** file toohello cluster as hello new desired configuration.</span></span> <span data-ttu-id="acf7d-219">hello-begäran returnerar ett JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="acf7d-219">hello request returns a JSON document.</span></span> <span data-ttu-id="acf7d-220">Hej **versionTag** element i det här dokumentet ska matcha hello-version du har skickats och hello **konfigurationerna** objektet innehåller hello konfigurationsändringar som du begärde.</span><span class="sxs-lookup"><span data-stu-id="acf7d-220">hello **versionTag** element in this document should match hello version you submitted, and hello **configs** object contains hello configuration changes you requested.</span></span>

### <a name="example-restart-a-service-component"></a><span data-ttu-id="acf7d-221">Exempel: Starta om en tjänstkomponent</span><span class="sxs-lookup"><span data-stu-id="acf7d-221">Example: Restart a service component</span></span>

<span data-ttu-id="acf7d-222">Du tittar på hello Ambari-webbgränssnittet visar hello Spark-tjänst nu att den behöver toobe startas om innan hello nya konfigurationen börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="acf7d-222">At this point, if you look at hello Ambari web UI, hello Spark service indicates that it needs toobe restarted before hello new configuration can take effect.</span></span> <span data-ttu-id="acf7d-223">Använd hello följande steg toorestart hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="acf7d-223">Use hello following steps toorestart hello service.</span></span>

1. <span data-ttu-id="acf7d-224">Använd hello följande tooenable Underhållsläge för hello Spark-tjänst:</span><span class="sxs-lookup"><span data-stu-id="acf7d-224">Use hello following tooenable maintenance mode for hello Spark service:</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}'
    $resp.Content
    ```
   
    <span data-ttu-id="acf7d-225">Dessa kommandon skickar en JSON-dokument toohello server som aktiverar underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="acf7d-225">These commands send a JSON document toohello server that turns on maintenance mode.</span></span> <span data-ttu-id="acf7d-226">Du kan verifiera att hello-tjänsten är nu i underhållsläge med hello följande begäran:</span><span class="sxs-lookup"><span data-stu-id="acf7d-226">You can verify that hello service is now in maintenance mode using hello following request:</span></span>
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```
   
    <span data-ttu-id="acf7d-227">hello returvärdet är `ON`.</span><span class="sxs-lookup"><span data-stu-id="acf7d-227">hello return value is `ON`.</span></span>

2. <span data-ttu-id="acf7d-228">Använd sedan hello följande tooturn av hello-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="acf7d-228">Next, use hello following tooturn off hello service:</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}'
    $resp.Content
    ```
    
    <span data-ttu-id="acf7d-229">hello svaret är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="acf7d-229">hello response is similar toohello following example:</span></span>
   
    ```json
    {
        "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
        "Requests" : {
            "id" : 29,
            "status" : "Accepted"
        }
    }
    ```
    
    > [!IMPORTANT]
    > <span data-ttu-id="acf7d-230">Hej `href` värdet som returneras av den här URI: N använder hello interna IP-adress hello klusternod.</span><span class="sxs-lookup"><span data-stu-id="acf7d-230">hello `href` value returned by this URI is using hello internal IP address of hello cluster node.</span></span> <span data-ttu-id="acf7d-231">toouse från utanför hello klustret ersätta hello '10.0.0.18:8080' delen med hello domännamn för hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="acf7d-231">toouse it from outside hello cluster, replace hello \`10.0.0.18:8080' portion with hello FQDN of hello cluster.</span></span> 
    
    <span data-ttu-id="acf7d-232">Hej efter kommandona hämta hello status för hello begäran:</span><span class="sxs-lookup"><span data-stu-id="acf7d-232">hello following commands retrieve hello status of hello request:</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    <span data-ttu-id="acf7d-233">Ett svar av `COMPLETED` anger hello begäran har slutförts.</span><span class="sxs-lookup"><span data-stu-id="acf7d-233">A response of `COMPLETED` indicates that hello request has finished.</span></span>

3. <span data-ttu-id="acf7d-234">Använda hello följande toostart hello tjänsten när hello tidigare begäran har slutförts.</span><span class="sxs-lookup"><span data-stu-id="acf7d-234">Once hello previous request completes, use hello following toostart hello service.</span></span>
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```
   
    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}'
    ```
    <span data-ttu-id="acf7d-235">hello-tjänsten använder hello ny konfiguration.</span><span class="sxs-lookup"><span data-stu-id="acf7d-235">hello service is now using hello new configuration.</span></span>

4. <span data-ttu-id="acf7d-236">Använd slutligen hello följande tooturn av underhållsläget.</span><span class="sxs-lookup"><span data-stu-id="acf7d-236">Finally, use hello following tooturn off maintenance mode.</span></span>
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}'
    ```

## <a name="next-steps"></a><span data-ttu-id="acf7d-237">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="acf7d-237">Next steps</span></span>

<span data-ttu-id="acf7d-238">En fullständig hello REST API-referens, se [Ambari API-referens V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="acf7d-238">For a complete reference of hello REST API, see [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

