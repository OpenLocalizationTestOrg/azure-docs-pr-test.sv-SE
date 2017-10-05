---
title: "Övervaka och hantera Hadoop med Ambari REST API - Azure HDInsight | Microsoft Docs"
description: "Lär dig använda Ambari och övervaka och hantera Hadoop-kluster i Azure HDInsight. I det här dokumentet får du lära dig hur du använder Ambari REST API som ingår i HDInsight-kluster."
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
ms.openlocfilehash: 7960d83bce22d4f671d61e9aaf55561bc24308f8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a><span data-ttu-id="10e17-104">Hantera HDInsight-kluster med Ambari REST API</span><span class="sxs-lookup"><span data-stu-id="10e17-104">Manage HDInsight clusters by using the Ambari REST API</span></span>

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

<span data-ttu-id="10e17-105">Lär dig använda Ambari REST API för att hantera och övervaka Hadoop-kluster i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="10e17-105">Learn how to use the Ambari REST API to manage and monitor Hadoop clusters in Azure HDInsight.</span></span>

<span data-ttu-id="10e17-106">Apache Ambari förenklar hantering och övervakning av ett Hadoop-kluster genom att tillhandahålla ett enkelt att använda webbgränssnittet och REST-API.</span><span class="sxs-lookup"><span data-stu-id="10e17-106">Apache Ambari simplifies the management and monitoring of a Hadoop cluster by providing an easy to use web UI and REST API.</span></span> <span data-ttu-id="10e17-107">Ambari ingår i HDInsight-kluster som använder Linux-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="10e17-107">Ambari is included on HDInsight clusters that use the Linux operating system.</span></span> <span data-ttu-id="10e17-108">Du kan använda Ambari för att övervaka klustret och gör ändringar i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="10e17-108">You can use Ambari to monitor the cluster and make configuration changes.</span></span>

## <span data-ttu-id="10e17-109"><a id="whatis"></a>Vad är Ambari</span><span class="sxs-lookup"><span data-stu-id="10e17-109"><a id="whatis"></a>What is Ambari</span></span>

<span data-ttu-id="10e17-110">[Apache Ambari](http://ambari.apache.org) ger webbgränssnitt som kan användas för att etablera, hantera och övervaka Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="10e17-110">[Apache Ambari](http://ambari.apache.org) provides web UI that can be used to provision, manage, and monitor Hadoop clusters.</span></span> <span data-ttu-id="10e17-111">Utvecklare kan integrera dessa funktioner i sina program med hjälp av den [Ambari REST API: er](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="10e17-111">Developers can integrate these capabilities into their applications by using the [Ambari REST APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

<span data-ttu-id="10e17-112">Ambari är som standard med Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="10e17-112">Ambari is provided by default with Linux-based HDInsight clusters.</span></span>

## <a name="how-to-use-the-ambari-rest-api"></a><span data-ttu-id="10e17-113">Använda Ambari REST API</span><span class="sxs-lookup"><span data-stu-id="10e17-113">How to use the Ambari REST API</span></span>

> [!IMPORTANT]
> <span data-ttu-id="10e17-114">Information och exempel i det här dokumentet kräver ett HDInsight-kluster som använder Linux-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="10e17-114">The information and examples in this document require an HDInsight cluster that uses Linux operating system.</span></span> <span data-ttu-id="10e17-115">Mer information finns i [komma igång med HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="10e17-115">For more information, see [Get started with HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

<span data-ttu-id="10e17-116">Exemplen i det här dokumentet tillhandahålls för både Bourne shell (bash) och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="10e17-116">The examples in this document are provided for both the Bourne shell (bash) and PowerShell.</span></span> <span data-ttu-id="10e17-117">Bash exempel har testats med GNU bash 4.3.11, men bör arbeta tillsammans med andra Unix-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="10e17-117">The bash examples were tested with GNU bash 4.3.11, but should work with other Unix shells.</span></span> <span data-ttu-id="10e17-118">PowerShell-exemplen har testats med PowerShell 5.0, men ska fungera med PowerShell 3.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="10e17-118">The PowerShell examples were tested with PowerShell 5.0, but should work with PowerShell 3.0 or higher.</span></span>

<span data-ttu-id="10e17-119">Om du använder den __Bourne shell__ (Bash), måste du ha följande installerat:</span><span class="sxs-lookup"><span data-stu-id="10e17-119">If using the __Bourne shell__ (Bash), you must have the following installed:</span></span>

* <span data-ttu-id="10e17-120">[cURL](http://curl.haxx.se/): cURL är ett verktyg som kan användas för att arbeta med REST API: er från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="10e17-120">[cURL](http://curl.haxx.se/): cURL is a utility that can be used to work with REST APIs from the command line.</span></span> <span data-ttu-id="10e17-121">I det här dokumentet används den för att kommunicera med Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="10e17-121">In this document, it is used to communicate with the Ambari REST API.</span></span>

<span data-ttu-id="10e17-122">Om du använder Bash eller PowerShell, du måste ha [jq](https://stedolan.github.io/jq/) installerad.</span><span class="sxs-lookup"><span data-stu-id="10e17-122">Whether using Bash or PowerShell, you must also have [jq](https://stedolan.github.io/jq/) installed.</span></span> <span data-ttu-id="10e17-123">Jq är ett verktyg för att arbeta med JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="10e17-123">Jq is a utility for working with JSON documents.</span></span> <span data-ttu-id="10e17-124">Den används i **alla** Bash-exempel och **en** av PowerShell-exempel.</span><span class="sxs-lookup"><span data-stu-id="10e17-124">It is used in **all** the Bash examples, and **one** of the PowerShell examples.</span></span>

### <a name="base-uri-for-ambari-rest-api"></a><span data-ttu-id="10e17-125">Bas-URI för Ambari Rest API</span><span class="sxs-lookup"><span data-stu-id="10e17-125">Base URI for Ambari Rest API</span></span>

<span data-ttu-id="10e17-126">Bas-URI för Ambari REST API på HDInsight är https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, där **KLUSTERNAMN** är namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-126">The base URI for the Ambari REST API on HDInsight is https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, where **CLUSTERNAME** is the name of your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="10e17-127">Klusternamnet i fullständigt kvalificerade domännamnet (FQDN) Namndelen för URI: N (CLUSTERNAME.azurehdinsight.net) är skiftlägeskänslig, är andra förekomster i URI: N skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="10e17-127">While the cluster name in the fully qualified domain name (FQDN) part of the URI (CLUSTERNAME.azurehdinsight.net) is case-insensitive, other occurrences in the URI are case-sensitive.</span></span> <span data-ttu-id="10e17-128">Om klustret har namnet till exempel `MyCluster`, följande är giltiga URI: er:</span><span class="sxs-lookup"><span data-stu-id="10e17-128">For example, if your cluster is named `MyCluster`, the following are valid URIs:</span></span>
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> <span data-ttu-id="10e17-129">Ett fel returneras följande URI eftersom den andra förekomsten av namnet inte är rätt skiftläge.</span><span class="sxs-lookup"><span data-stu-id="10e17-129">The following URIs return an error because the second occurrence of the name is not the correct case.</span></span>
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a><span data-ttu-id="10e17-130">Autentisering</span><span class="sxs-lookup"><span data-stu-id="10e17-130">Authentication</span></span>

<span data-ttu-id="10e17-131">Ansluter till Ambari på HDInsight kräver HTTPS.</span><span class="sxs-lookup"><span data-stu-id="10e17-131">Connecting to Ambari on HDInsight requires HTTPS.</span></span> <span data-ttu-id="10e17-132">Använda admin-kontonamn (standardvärdet är **admin**) och lösenord som du angav när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="10e17-132">Use the admin account name (the default is **admin**) and password you provided during cluster creation.</span></span>

## <a name="examples-authentication-and-parsing-json"></a><span data-ttu-id="10e17-133">Exempel: Autentisering och parsa JSON</span><span class="sxs-lookup"><span data-stu-id="10e17-133">Examples: Authentication and parsing JSON</span></span>

<span data-ttu-id="10e17-134">Följande exempel visar hur du gör en GET-begäran mot grundläggande Ambari REST API:</span><span class="sxs-lookup"><span data-stu-id="10e17-134">The following examples demonstrate how to make a GET request against the base Ambari REST API:</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> <span data-ttu-id="10e17-135">Bash exemplen i det här dokumentet gör följande antaganden:</span><span class="sxs-lookup"><span data-stu-id="10e17-135">The Bash examples in this document make the following assumptions:</span></span>
>
> * <span data-ttu-id="10e17-136">Inloggningsnamn för klustret som är standardvärdet för `admin`.</span><span class="sxs-lookup"><span data-stu-id="10e17-136">The login name for the cluster is the default value of `admin`.</span></span>
> * <span data-ttu-id="10e17-137">`$PASSWORD`innehåller lösenordet för kommandot HDInsight inloggningen.</span><span class="sxs-lookup"><span data-stu-id="10e17-137">`$PASSWORD` contains the password for the HDInsight login command.</span></span> <span data-ttu-id="10e17-138">Du kan ange ett värde med hjälp av `PASSWORD='mypassword'`.</span><span class="sxs-lookup"><span data-stu-id="10e17-138">You can set this value by using `PASSWORD='mypassword'`.</span></span>
> * <span data-ttu-id="10e17-139">`$CLUSTERNAME`innehåller namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-139">`$CLUSTERNAME` contains the name of the cluster.</span></span> <span data-ttu-id="10e17-140">Du kan ange ett värde med hjälp av`set CLUSTERNAME='clustername'`</span><span class="sxs-lookup"><span data-stu-id="10e17-140">You can set this value by using `set CLUSTERNAME='clustername'`</span></span>

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> <span data-ttu-id="10e17-141">PowerShell-exemplen i det här dokumentet gör följande antaganden:</span><span class="sxs-lookup"><span data-stu-id="10e17-141">The PowerShell examples in this document make the following assumptions:</span></span>
>
> * <span data-ttu-id="10e17-142">`$creds`är ett autentiseringsuppgiftobjekt som innehåller admin inloggningsnamn och lösenord för klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-142">`$creds` is a credential object that contains the admin login and password for the cluster.</span></span> <span data-ttu-id="10e17-143">Du kan ange ett värde med hjälp av `$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"` och anger autentiseringsuppgifter när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="10e17-143">You can set this value by using `$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"` and providing the credentials when prompted.</span></span>
> * <span data-ttu-id="10e17-144">`$clusterName`är en sträng som innehåller namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-144">`$clusterName` is a string that contains the name of the cluster.</span></span> <span data-ttu-id="10e17-145">Du kan ange ett värde med hjälp av `$clusterName="clustername"`.</span><span class="sxs-lookup"><span data-stu-id="10e17-145">You can set this value by using `$clusterName="clustername"`.</span></span>

<span data-ttu-id="10e17-146">Båda exempel returnera ett JSON-dokument som börjar med information som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="10e17-146">Both examples return a JSON document that begins with information similar to the following example:</span></span>

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

### <a name="parsing-json-data"></a><span data-ttu-id="10e17-147">Parsning av JSON-data</span><span class="sxs-lookup"><span data-stu-id="10e17-147">Parsing JSON data</span></span>

<span data-ttu-id="10e17-148">I följande exempel används `jq` tolka svaret JSON-dokumentet och visa endast de `health_report` information från resultaten.</span><span class="sxs-lookup"><span data-stu-id="10e17-148">The following example uses `jq` to parse the JSON response document and display only the `health_report` information from the results.</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

<span data-ttu-id="10e17-149">PowerShell 3.0 och senare innehåller den `ConvertFrom-Json` cmdlet, som konverterar JSON-dokumentet till ett objekt som är enklare att arbeta med från PowerShell.</span><span class="sxs-lookup"><span data-stu-id="10e17-149">PowerShell 3.0 and higher provides the `ConvertFrom-Json` cmdlet, which converts the JSON document into an object that is easier to work with from PowerShell.</span></span> <span data-ttu-id="10e17-150">I följande exempel används `ConvertFrom-Json` att visa endast de `health_report` information från resultaten.</span><span class="sxs-lookup"><span data-stu-id="10e17-150">The following example uses `ConvertFrom-Json` to display only the `health_report` information from the results.</span></span>

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> <span data-ttu-id="10e17-151">När de flesta exemplen i det här dokumentet används `ConvertFrom-Json` att visa element från svarsdokument i [uppdatering Ambari configuration](#example-update-ambari-configuration) exempel används jq.</span><span class="sxs-lookup"><span data-stu-id="10e17-151">While most examples in this document use `ConvertFrom-Json` to display elements from the response document, the [Update Ambari configuration](#example-update-ambari-configuration) example uses jq.</span></span> <span data-ttu-id="10e17-152">Jq används i det här exemplet för att skapa en ny mall från svar JSON-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="10e17-152">Jq is used in this example to construct a new template from the JSON response document.</span></span>

<span data-ttu-id="10e17-153">En fullständig referens för REST-API finns [Ambari API-referens V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="10e17-153">For a complete reference of the REST API, see [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

## <a name="example-get-the-fqdn-of-cluster-nodes"></a><span data-ttu-id="10e17-154">Exempel: Hämta FQDN för klusternoder</span><span class="sxs-lookup"><span data-stu-id="10e17-154">Example: Get the FQDN of cluster nodes</span></span>

<span data-ttu-id="10e17-155">När du arbetar med HDInsight, kanske du behöver veta det fullständigt kvalificerade domännamnet (FQDN) på en klusternod.</span><span class="sxs-lookup"><span data-stu-id="10e17-155">When working with HDInsight, you may need to know the fully qualified domain name (FQDN) of a cluster node.</span></span> <span data-ttu-id="10e17-156">Du kan lätt kunna hämta det fullständiga Domännamnet för olika noder i klustret med följande exempel:</span><span class="sxs-lookup"><span data-stu-id="10e17-156">You can easily retrieve the FQDN for the various nodes in the cluster using the following examples:</span></span>

* <span data-ttu-id="10e17-157">**Alla noder**</span><span class="sxs-lookup"><span data-stu-id="10e17-157">**All nodes**</span></span>

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

* <span data-ttu-id="10e17-158">**HEAD-noder**</span><span class="sxs-lookup"><span data-stu-id="10e17-158">**Head nodes**</span></span>

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

* <span data-ttu-id="10e17-159">**Arbetsnoder**</span><span class="sxs-lookup"><span data-stu-id="10e17-159">**Worker nodes**</span></span>

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

* <span data-ttu-id="10e17-160">**Zookeeper-noder**</span><span class="sxs-lookup"><span data-stu-id="10e17-160">**Zookeeper nodes**</span></span>

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

## <a name="example-get-the-internal-ip-address-of-cluster-nodes"></a><span data-ttu-id="10e17-161">Exempel: Hämta klusternoder interna IP-adress</span><span class="sxs-lookup"><span data-stu-id="10e17-161">Example: Get the internal IP address of cluster nodes</span></span>

> [!IMPORTANT]
> <span data-ttu-id="10e17-162">IP-adresser som returneras av exemplen i det här avsnittet är inte direkt åtkomliga via internet.</span><span class="sxs-lookup"><span data-stu-id="10e17-162">The IP addresses returned by the examples in this section are not directly accessible over the internet.</span></span> <span data-ttu-id="10e17-163">De är bara tillgängliga i det virtuella Azure-nätverket som innehåller HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-163">They are only accessible within the Azure Virtual Network that contains the HDInsight cluster.</span></span>
>
> <span data-ttu-id="10e17-164">Mer information om att arbeta med HDInsight och virtuella nätverk finns [utöka HDInsight funktioner med hjälp av en anpassad Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="10e17-164">For more information on working with HDInsight and virtual networks, see [Extend HDInsight capabilities by using a custom Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).</span></span>

<span data-ttu-id="10e17-165">Om du vill hitta IP-adress, måste du känna fullständigt kvalificerade domännamnet interna namnet (FQDN) på klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="10e17-165">To find the IP address, you must know the internal fully qualified domain name (FQDN) of the cluster nodes.</span></span> <span data-ttu-id="10e17-166">När du har FQDN kan du hämta IP-adressen för värden.</span><span class="sxs-lookup"><span data-stu-id="10e17-166">Once you have the FQDN, you can then get the IP address of the host.</span></span> <span data-ttu-id="10e17-167">I följande exempel först fråga Ambari för FQDN för alla värdnoder och fråga Ambari för IP-adressen för varje värd.</span><span class="sxs-lookup"><span data-stu-id="10e17-167">The following examples first query Ambari for the FQDN of all the host nodes, then query Ambari for the IP address of each host.</span></span>

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

## <a name="example-get-the-default-storage"></a><span data-ttu-id="10e17-168">Exempel: Hämta standardlagring</span><span class="sxs-lookup"><span data-stu-id="10e17-168">Example: Get the default storage</span></span>

<span data-ttu-id="10e17-169">När du skapar ett HDInsight-kluster måste du använda ett Azure Storage-konto eller ett Data Lake Store som standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-169">When you create an HDInsight cluster, you must use an Azure Storage Account or Data Lake Store as the default storage for the cluster.</span></span> <span data-ttu-id="10e17-170">Du kan använda Ambari för att hämta den här informationen när klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="10e17-170">You can use Ambari to retrieve this information after the cluster has been created.</span></span> <span data-ttu-id="10e17-171">Till exempel om du vill läsa/skriva data till behållaren utanför HDInsight.</span><span class="sxs-lookup"><span data-stu-id="10e17-171">For example, if you want to read/write data to the container outside HDInsight.</span></span>

<span data-ttu-id="10e17-172">Följande exempel hämtar lagringskonfigurationen som standard från klustret:</span><span class="sxs-lookup"><span data-stu-id="10e17-172">The following examples retrieve the default storage configuration from the cluster:</span></span>

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
> <span data-ttu-id="10e17-173">De här exemplen returnera den första konfigurationen tillämpas på servern (`service_config_version=1`) som innehåller den här informationen.</span><span class="sxs-lookup"><span data-stu-id="10e17-173">These examples return the first configuration applied to the server (`service_config_version=1`) which contains this information.</span></span> <span data-ttu-id="10e17-174">Om du hämtar ett värde som har ändrats efter att klustret har skapats kan du behöva listan konfigurationsversionerna och hämtar den senaste.</span><span class="sxs-lookup"><span data-stu-id="10e17-174">If you retrieve a value that has been modified after cluster creation, you may need to list the configuration versions and retrieve the latest one.</span></span>

<span data-ttu-id="10e17-175">Returvärdet liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="10e17-175">The return value is similar to one of the following examples:</span></span>

* <span data-ttu-id="10e17-176">`wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net`-Det här värdet anger att klustret använder ett Azure Storage-konto för standardlagring.</span><span class="sxs-lookup"><span data-stu-id="10e17-176">`wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net` - This value indicates that the cluster is using an Azure Storage account for default storage.</span></span> <span data-ttu-id="10e17-177">Den `ACCOUNTNAME` värde är namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="10e17-177">The `ACCOUNTNAME` value is the name of the storage account.</span></span> <span data-ttu-id="10e17-178">Den `CONTAINER` delen är namnet på blob-behållaren i storage-konto.</span><span class="sxs-lookup"><span data-stu-id="10e17-178">The `CONTAINER` portion is the name of the blob container in the storage account.</span></span> <span data-ttu-id="10e17-179">Behållaren är roten till HDFS kompatibel lagringsutrymme för klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-179">The container is the root of the HDFS compatible storage for the cluster.</span></span>

* <span data-ttu-id="10e17-180">`adl://home`-Det här värdet anger att klustret använder en Azure Data Lake Store för standardlagring.</span><span class="sxs-lookup"><span data-stu-id="10e17-180">`adl://home` - This value indicates that the cluster is using an Azure Data Lake Store for default storage.</span></span>

    <span data-ttu-id="10e17-181">Använd följande exempel för att hitta namnet på Data Lake Store-kontot:</span><span class="sxs-lookup"><span data-stu-id="10e17-181">To find the Data Lake Store account name, use the following examples:</span></span>

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

    <span data-ttu-id="10e17-182">Returvärdet liknar `ACCOUNTNAME.azuredatalakestore.net`, där `ACCOUNTNAME` är namnet på Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="10e17-182">The return value is similar to `ACCOUNTNAME.azuredatalakestore.net`, where `ACCOUNTNAME` is the name of the Data Lake Store account.</span></span>

    <span data-ttu-id="10e17-183">Använd följande exempel för att hitta katalogen i Data Lake Store med lagringsutrymme för klustret:</span><span class="sxs-lookup"><span data-stu-id="10e17-183">To find the directory within Data Lake Store that contains the storage for the cluster, use the following examples:</span></span>

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

    <span data-ttu-id="10e17-184">Returvärdet liknar `/clusters/CLUSTERNAME/`.</span><span class="sxs-lookup"><span data-stu-id="10e17-184">The return value is similar to `/clusters/CLUSTERNAME/`.</span></span> <span data-ttu-id="10e17-185">Det här värdet är en sökväg i Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="10e17-185">This value is a path within the Data Lake Store account.</span></span> <span data-ttu-id="10e17-186">Den här sökvägen är roten för den HDFS-kompatibel fil för klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-186">This path is the root of the HDFS compatible file system for the cluster.</span></span> 

> [!NOTE]
> <span data-ttu-id="10e17-187">Den `Get-AzureRmHDInsightCluster` cmdlet som tillhandahålls av [Azure PowerShell](/powershell/azure/overview) returnerar även i informationen för klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-187">The `Get-AzureRmHDInsightCluster` cmdlet provided by [Azure PowerShell](/powershell/azure/overview) also returns the storage information for the cluster.</span></span>


## <a name="example-get-configuration"></a><span data-ttu-id="10e17-188">Exempel: Get-konfiguration</span><span class="sxs-lookup"><span data-stu-id="10e17-188">Example: Get configuration</span></span>

1. <span data-ttu-id="10e17-189">Hämta de konfigurationer som är tillgängliga för klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-189">Get the configurations that are available for your cluster.</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    <span data-ttu-id="10e17-190">Det här exemplet returnerar ett JSON-dokument som innehåller den aktuella konfigurationen (identifieras av den *taggen* värde) för de komponenter som installeras i klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-190">This example returns a JSON document containing the current configuration (identified by the *tag* value) for the components installed on the cluster.</span></span> <span data-ttu-id="10e17-191">I följande exempel är ett utdrag ur data som returneras från en typ av Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="10e17-191">The following example is an excerpt from the data returned from a Spark cluster type.</span></span>
   
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

2. <span data-ttu-id="10e17-192">Hämta konfigurationen för den komponent som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="10e17-192">Get the configuration for the component that you are interested in.</span></span> <span data-ttu-id="10e17-193">I följande exempel ersätter `INITIAL` med tagg-värde returnerades från föregående begäran.</span><span class="sxs-lookup"><span data-stu-id="10e17-193">In the following example, replace `INITIAL` with the tag value returned from the previous request.</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    <span data-ttu-id="10e17-194">Det här exemplet returnerar ett JSON-dokument som innehåller den aktuella konfigurationen för den `core-site` komponent.</span><span class="sxs-lookup"><span data-stu-id="10e17-194">This example returns a JSON document containing the current configuration for the `core-site` component.</span></span>

## <a name="example-update-configuration"></a><span data-ttu-id="10e17-195">Exempel: Uppdateringskonfiguration</span><span class="sxs-lookup"><span data-stu-id="10e17-195">Example: Update configuration</span></span>

1. <span data-ttu-id="10e17-196">Hämta den aktuella konfigurationen Ambari lagrar som ”desired configuration”:</span><span class="sxs-lookup"><span data-stu-id="10e17-196">Get the current configuration, which Ambari stores as the "desired configuration":</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    <span data-ttu-id="10e17-197">Det här exemplet returnerar ett JSON-dokument som innehåller den aktuella konfigurationen (identifieras av den *taggen* värde) för de komponenter som installeras i klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-197">This example returns a JSON document containing the current configuration (identified by the *tag* value) for the components installed on the cluster.</span></span> <span data-ttu-id="10e17-198">I följande exempel är ett utdrag ur data som returneras från en typ av Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="10e17-198">The following example is an excerpt from the data returned from a Spark cluster type.</span></span>
   
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
   
    <span data-ttu-id="10e17-199">Den här listan, måste du kopiera namnet på komponenten (till exempel **spark\_thrift\_sparkconf** och **taggen** värde.</span><span class="sxs-lookup"><span data-stu-id="10e17-199">From this list, you need to copy the name of the component (for example, **spark\_thrift\_sparkconf** and the **tag** value.</span></span>

2. <span data-ttu-id="10e17-200">Hämta konfigurationen för komponenten och tagg med hjälp av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="10e17-200">Retrieve the configuration for the component and tag by using the following commands:</span></span>
   
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
    > <span data-ttu-id="10e17-201">Ersätt **spark-thrift-sparkconf** och **inledande** med komponenten och tagg som du vill hämta konfigurationen för.</span><span class="sxs-lookup"><span data-stu-id="10e17-201">Replace **spark-thrift-sparkconf** and **INITIAL** with the component and tag that you want to retrieve the configuration for.</span></span>
   
    <span data-ttu-id="10e17-202">Jq används för att aktivera data som hämtats från HDInsight till en ny konfigurationsmall för.</span><span class="sxs-lookup"><span data-stu-id="10e17-202">Jq is used to turn the data retrieved from HDInsight into a new configuration template.</span></span> <span data-ttu-id="10e17-203">Mer specifikt utför exemplen följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="10e17-203">Specifically, these examples perform the following actions:</span></span>
   
    * <span data-ttu-id="10e17-204">Skapar ett unikt värde som innehåller strängen ”version” och det datum som lagras i `newtag`.</span><span class="sxs-lookup"><span data-stu-id="10e17-204">Creates a unique value containing the string "version" and the date, which is stored in `newtag`.</span></span>

    * <span data-ttu-id="10e17-205">Skapar ett rot-dokument för nya önskad konfiguration.</span><span class="sxs-lookup"><span data-stu-id="10e17-205">Creates a root document for the new desired configuration.</span></span>

    * <span data-ttu-id="10e17-206">Hämtar innehållet i den `.items[]` matrisen och lägger till den under den **desired_config** element.</span><span class="sxs-lookup"><span data-stu-id="10e17-206">Gets the contents of the `.items[]` array and adds it under the **desired_config** element.</span></span>

    * <span data-ttu-id="10e17-207">Tar bort den `href`, `version`, och `Config` element, som de här elementen inte behövs för att skicka en ny konfiguration.</span><span class="sxs-lookup"><span data-stu-id="10e17-207">Deletes the `href`, `version`, and `Config` elements, as these elements aren't needed to submit a new configuration.</span></span>

    * <span data-ttu-id="10e17-208">Lägger till en `tag` element med värdet `version#################`.</span><span class="sxs-lookup"><span data-stu-id="10e17-208">Adds a `tag` element with a value of `version#################`.</span></span> <span data-ttu-id="10e17-209">Den numeriska delen är baserad på det aktuella datumet.</span><span class="sxs-lookup"><span data-stu-id="10e17-209">The numeric portion is based on the current date.</span></span> <span data-ttu-id="10e17-210">Varje konfiguration måste ha en unik tagg.</span><span class="sxs-lookup"><span data-stu-id="10e17-210">Each configuration must have a unique tag.</span></span>
     
    <span data-ttu-id="10e17-211">Slutligen data sparas på den `newconfig.json` dokumentet.</span><span class="sxs-lookup"><span data-stu-id="10e17-211">Finally, the data is saved to the `newconfig.json` document.</span></span> <span data-ttu-id="10e17-212">Dokumentstrukturen ska se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="10e17-212">The document structure should appear similar to the following example:</span></span>
     
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

3. <span data-ttu-id="10e17-213">Öppna den `newconfig.json` dokument och ändra/Lägg till värden i den `properties` objekt.</span><span class="sxs-lookup"><span data-stu-id="10e17-213">Open the `newconfig.json` document and modify/add values in the `properties` object.</span></span> <span data-ttu-id="10e17-214">I följande exempel ändras värdet för `"spark.yarn.am.memory"` från `"1g"` till `"3g"`.</span><span class="sxs-lookup"><span data-stu-id="10e17-214">The following example changes the value of `"spark.yarn.am.memory"` from `"1g"` to `"3g"`.</span></span> <span data-ttu-id="10e17-215">Det ger också `"spark.kryoserializer.buffer.max"` med värdet `"256m"`.</span><span class="sxs-lookup"><span data-stu-id="10e17-215">It also adds `"spark.kryoserializer.buffer.max"` with a value of `"256m"`.</span></span>
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    <span data-ttu-id="10e17-216">Spara filen när du är klar med ändringarna.</span><span class="sxs-lookup"><span data-stu-id="10e17-216">Save the file once you are done making modifications.</span></span>

4. <span data-ttu-id="10e17-217">Använd följande kommandon för att skicka den uppdaterade konfigurationen till Ambari.</span><span class="sxs-lookup"><span data-stu-id="10e17-217">Use the following commands to submit the updated configuration to Ambari.</span></span>
   
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
   
    <span data-ttu-id="10e17-218">Dessa kommandon skicka innehållet i den **newconfig.json** filen till klustret som ny önskad konfiguration.</span><span class="sxs-lookup"><span data-stu-id="10e17-218">These commands submit the contents of the **newconfig.json** file to the cluster as the new desired configuration.</span></span> <span data-ttu-id="10e17-219">Begäran returnerar ett JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="10e17-219">The request returns a JSON document.</span></span> <span data-ttu-id="10e17-220">Den **versionTag** element i det här dokumentet ska matcha den version som du skickade och **konfigurationerna** objektet innehåller ändringar i konfigurationen som du begärde.</span><span class="sxs-lookup"><span data-stu-id="10e17-220">The **versionTag** element in this document should match the version you submitted, and the **configs** object contains the configuration changes you requested.</span></span>

### <a name="example-restart-a-service-component"></a><span data-ttu-id="10e17-221">Exempel: Starta om en tjänstkomponent</span><span class="sxs-lookup"><span data-stu-id="10e17-221">Example: Restart a service component</span></span>

<span data-ttu-id="10e17-222">Du tittar på Ambari-webbgränssnittet anger Spark-tjänsten nu måste startas om innan den nya konfigurationen ska börja gälla.</span><span class="sxs-lookup"><span data-stu-id="10e17-222">At this point, if you look at the Ambari web UI, the Spark service indicates that it needs to be restarted before the new configuration can take effect.</span></span> <span data-ttu-id="10e17-223">Använd följande steg för att starta om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="10e17-223">Use the following steps to restart the service.</span></span>

1. <span data-ttu-id="10e17-224">Använd följande för att aktivera underhållsläget för Spark-tjänst:</span><span class="sxs-lookup"><span data-stu-id="10e17-224">Use the following to enable maintenance mode for the Spark service:</span></span>

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
   
    <span data-ttu-id="10e17-225">Dessa kommandon skicka ett JSON-dokument till servern som aktiverar underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="10e17-225">These commands send a JSON document to the server that turns on maintenance mode.</span></span> <span data-ttu-id="10e17-226">Du kan kontrollera att tjänsten är nu i underhållsläge med följande begäran:</span><span class="sxs-lookup"><span data-stu-id="10e17-226">You can verify that the service is now in maintenance mode using the following request:</span></span>
   
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
   
    <span data-ttu-id="10e17-227">Det returnera värdet är `ON`.</span><span class="sxs-lookup"><span data-stu-id="10e17-227">The return value is `ON`.</span></span>

2. <span data-ttu-id="10e17-228">Använd följande för att stänga av tjänsten:</span><span class="sxs-lookup"><span data-stu-id="10e17-228">Next, use the following to turn off the service:</span></span>

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
    
    <span data-ttu-id="10e17-229">Svaret liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="10e17-229">The response is similar to the following example:</span></span>
   
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
    > <span data-ttu-id="10e17-230">Den `href` värdet som returneras av den här URI: N använder interna IP-adressen för noden i klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-230">The `href` value returned by this URI is using the internal IP address of the cluster node.</span></span> <span data-ttu-id="10e17-231">Om du vill använda den från utanför klustret måste du ersätta den '10.0.0.18:8080'-delen med FQDN för klustret.</span><span class="sxs-lookup"><span data-stu-id="10e17-231">To use it from outside the cluster, replace the \`10.0.0.18:8080' portion with the FQDN of the cluster.</span></span> 
    
    <span data-ttu-id="10e17-232">Följande kommandon för att hämta status för begäran:</span><span class="sxs-lookup"><span data-stu-id="10e17-232">The following commands retrieve the status of the request:</span></span>

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

    <span data-ttu-id="10e17-233">Ett svar av `COMPLETED` anger att begäran har slutförts.</span><span class="sxs-lookup"><span data-stu-id="10e17-233">A response of `COMPLETED` indicates that the request has finished.</span></span>

3. <span data-ttu-id="10e17-234">När den tidigare begäranden har slutförts kan du använda följande för att starta tjänsten.</span><span class="sxs-lookup"><span data-stu-id="10e17-234">Once the previous request completes, use the following to start the service.</span></span>
   
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
    <span data-ttu-id="10e17-235">Tjänsten använder den nya konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="10e17-235">The service is now using the new configuration.</span></span>

4. <span data-ttu-id="10e17-236">Slutligen, Använd följande för att inaktivera underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="10e17-236">Finally, use the following to turn off maintenance mode.</span></span>
   
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

## <a name="next-steps"></a><span data-ttu-id="10e17-237">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="10e17-237">Next steps</span></span>

<span data-ttu-id="10e17-238">En fullständig referens för REST-API finns [Ambari API-referens V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="10e17-238">For a complete reference of the REST API, see [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

