---
title: "Lägg till ytterligare Azure storage-konton till HDInsight | Microsoft Docs"
description: "Lär dig hur du lägger till ytterligare Azure storage-konton till ett befintligt HDInsight-kluster."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/04/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 0853e8605e07c28867676e9c13b89263ade67c88
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="add-additional-storage-accounts-to-hdinsight"></a><span data-ttu-id="29980-103">Lägga till ytterligare lagringskonton i HDInsight</span><span class="sxs-lookup"><span data-stu-id="29980-103">Add additional storage accounts to HDInsight</span></span>

<span data-ttu-id="29980-104">Lär dig använda script actions för att lägga till ytterligare Azure storage-konton i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="29980-104">Learn how to use script actions to add additional Azure storage accounts to HDInsight.</span></span> <span data-ttu-id="29980-105">Stegen i det här dokumentet för att lägga till ett lagringskonto till ett befintligt Linux-baserat HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="29980-105">The steps in this document add a storage account to an existing Linux-based HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="29980-106">Informationen i det här dokumentet handlar om att lägga till ytterligare lagringsutrymme i ett kluster när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="29980-106">The information in this document is about adding additional storage to a cluster after it has been created.</span></span> <span data-ttu-id="29980-107">Mer information om att lägga till lagringskonton när klustret skapas finns [ställa in kluster i HDInsight Hadoop, Spark, Kafka och mycket mer](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="29980-107">For information on adding storage accounts during cluster creation, see [Set up clusters in HDInsight with Hadoop, Spark, Kafka, and more](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="how-it-works"></a><span data-ttu-id="29980-108">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="29980-108">How it works</span></span>

<span data-ttu-id="29980-109">Det här skriptet använder följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="29980-109">This script takes the following parameters:</span></span>

* <span data-ttu-id="29980-110">__Azure lagringskontonamnet__: namnet på lagringskontot för att lägga till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-110">__Azure storage account name__: The name of the storage account to add to the HDInsight cluster.</span></span> <span data-ttu-id="29980-111">När du har kört skriptet kan HDInsight läsa och skriva data lagras i det här lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="29980-111">After running the script, HDInsight can read and write data stored in this storage account.</span></span>

* <span data-ttu-id="29980-112">__Azure lagringskontonyckel__: en nyckel som ger åtkomst till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="29980-112">__Azure storage account key__: A key that grants access to the storage account.</span></span>

* <span data-ttu-id="29980-113">__-p__ (valfritt): Om nyckeln inte är krypterad och lagras i filen core-site.XML som oformaterad text.</span><span class="sxs-lookup"><span data-stu-id="29980-113">__-p__ (optional): If specified, the key is not encrypted and is stored in the core-site.xml file as plain text.</span></span>

<span data-ttu-id="29980-114">Under bearbetning av utförs skriptet följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="29980-114">During processing, the script performs the following actions:</span></span>

* <span data-ttu-id="29980-115">Om lagringskontot finns redan i core-site.xml konfigurationen för klustret, skriptet avslutas och inga ytterligare åtgärder utförs.</span><span class="sxs-lookup"><span data-stu-id="29980-115">If the storage account already exists in the core-site.xml configuration for the cluster, the script exits and no further actions are performed.</span></span>

* <span data-ttu-id="29980-116">Verifierar att lagringskontot finns och kan nås med hjälp av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="29980-116">Verifies that the storage account exists and can be accessed using the key.</span></span>

* <span data-ttu-id="29980-117">Krypterar nyckeln med hjälp av autentiseringsuppgifter för klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-117">Encrypts the key using the cluster credential.</span></span>

* <span data-ttu-id="29980-118">Lägger till lagringskontot i filen core-site.XML.</span><span class="sxs-lookup"><span data-stu-id="29980-118">Adds the storage account to the core-site.xml file.</span></span>

* <span data-ttu-id="29980-119">Stoppar och startar om Oozie, YARN, MapReduce2 och HDFS-tjänster.</span><span class="sxs-lookup"><span data-stu-id="29980-119">Stops and restarts the Oozie, YARN, MapReduce2, and HDFS services.</span></span> <span data-ttu-id="29980-120">Stoppa och starta dessa tjänster gör att de kan använda det nya lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="29980-120">Stopping and starting these services allows them to use the new storage account.</span></span>

> [!WARNING]
> <span data-ttu-id="29980-121">Med hjälp av ett lagringskonto i en annan plats än HDInsight-kluster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="29980-121">Using a storage account in a different location than the HDInsight cluster is not supported.</span></span>

## <a name="the-script"></a><span data-ttu-id="29980-122">Skriptet</span><span class="sxs-lookup"><span data-stu-id="29980-122">The script</span></span>

<span data-ttu-id="29980-123">__Script plats__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)</span><span class="sxs-lookup"><span data-stu-id="29980-123">__Script location__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)</span></span>

<span data-ttu-id="29980-124">__Krav för__:</span><span class="sxs-lookup"><span data-stu-id="29980-124">__Requirements__:</span></span>

* <span data-ttu-id="29980-125">Skriptet måste tillämpas på den __huvudnoder__.</span><span class="sxs-lookup"><span data-stu-id="29980-125">The script must be applied on the __Head nodes__.</span></span>

## <a name="to-use-the-script"></a><span data-ttu-id="29980-126">Du använder skriptet</span><span class="sxs-lookup"><span data-stu-id="29980-126">To use the script</span></span>

<span data-ttu-id="29980-127">Det här skriptet kan användas från Azure-portalen, Azure PowerShell eller Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="29980-127">This script can be used from the Azure portal, Azure PowerShell, or the Azure CLI 1.0.</span></span> <span data-ttu-id="29980-128">Mer information finns i [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="29980-128">For more information, see the [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="29980-129">Använd följande information när du använder finns i dokumentet anpassning för att tillämpa det här skriptet:</span><span class="sxs-lookup"><span data-stu-id="29980-129">When using the steps provided in the customization document, use the following information to apply this script:</span></span>
>
> * <span data-ttu-id="29980-130">Ersätt alla exempel skriptåtgärd URI med URI: N för det här skriptet (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).</span><span class="sxs-lookup"><span data-stu-id="29980-130">Replace any example script action URI with the URI for this script (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).</span></span>
> * <span data-ttu-id="29980-131">Ersätt alla exempel parametrar med Azure storage-kontonamnet och nyckeln för lagringskontot som ska läggas till klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-131">Replace any example parameters with the Azure storage account name and key of the storage account to be added to the cluster.</span></span> <span data-ttu-id="29980-132">Om du använder Azure portal måste parametrarna avgränsas med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="29980-132">If using the Azure portal, these parameters must be separated by a space.</span></span>
> * <span data-ttu-id="29980-133">Du behöver inte markera skriptet som __bevarade__, enligt den direkt uppdaterar Ambari-konfiguration för klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-133">You do not need to mark this script as __Persisted__, as it directly updates the Ambari configuration for the cluster.</span></span>

## <a name="known-issues"></a><span data-ttu-id="29980-134">Kända problem</span><span class="sxs-lookup"><span data-stu-id="29980-134">Known issues</span></span>

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a><span data-ttu-id="29980-135">Storage-konton som inte visas i Azure-portalen eller verktyg</span><span class="sxs-lookup"><span data-stu-id="29980-135">Storage accounts not displayed in Azure portal or tools</span></span>

<span data-ttu-id="29980-136">När du visar HDInsight-kluster i Azure-portalen kan du välja den __Lagringskonton__ posten under __egenskaper__ storage-konton som lagts till via den här skriptåtgärden visas inte.</span><span class="sxs-lookup"><span data-stu-id="29980-136">When viewing the HDInsight cluster in the Azure portal, selecting the __Storage Accounts__ entry under __Properties__ does not display storage accounts added through this script action.</span></span> <span data-ttu-id="29980-137">Azure PowerShell och Azure CLI visas inte ytterligare storage-konto.</span><span class="sxs-lookup"><span data-stu-id="29980-137">Azure PowerShell and Azure CLI do not display the additional storage account either.</span></span>

<span data-ttu-id="29980-138">Storage-informationen visas inte eftersom skriptet ändras endast core-site.xml konfigurationen för klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-138">The storage information isn't displayed because the script only modifies the core-site.xml configuration for the cluster.</span></span> <span data-ttu-id="29980-139">Den här informationen används inte vid hämtning av klusterinformationen med hjälp av Azure management API: er.</span><span class="sxs-lookup"><span data-stu-id="29980-139">This information is not used when retrieving the cluster information using Azure management APIs.</span></span>

<span data-ttu-id="29980-140">Använd Ambari REST API för att visa information om lagringskontot läggs till klustret med det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="29980-140">To view storage account information added to the cluster using this script, use the Ambari REST API.</span></span> <span data-ttu-id="29980-141">Använd följande kommandon för att hämta informationen för klustret:</span><span class="sxs-lookup"><span data-stu-id="29980-141">Use the following commands to retrieve this information for your cluster:</span></span>

```PowerShell
$creds = Get-Credential -UserName "admin" -Message "Enter the cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$storageAccountName.blob.core.windows.net"
```

> [!NOTE]
> <span data-ttu-id="29980-142">Ange `$clusterName` till namnet på HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-142">Set `$clusterName` to the name of the HDInsight cluster.</span></span> <span data-ttu-id="29980-143">Ange `$storageAccountName` till namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="29980-143">Set `$storageAccountName` to the name of the storage account.</span></span> <span data-ttu-id="29980-144">När du uppmanas ange klusterinloggning (admin) och lösenord.</span><span class="sxs-lookup"><span data-stu-id="29980-144">When prompted, enter the cluster login (admin) and password.</span></span>

```Bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.azure.account.key.$STORAGEACCOUNTNAME.blob.core.windows.net"] | select(. != null)'
```

> [!NOTE]
> <span data-ttu-id="29980-145">Ange `$PASSWORD` kontolösenord för kluster-inloggning (admin).</span><span class="sxs-lookup"><span data-stu-id="29980-145">Set `$PASSWORD` to the cluster login (admin) account password.</span></span> <span data-ttu-id="29980-146">Ange `$CLUSTERNAME` till namnet på HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-146">Set `$CLUSTERNAME` to the name of the HDInsight cluster.</span></span> <span data-ttu-id="29980-147">Ange `$STORAGEACCOUNTNAME` till namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="29980-147">Set `$STORAGEACCOUNTNAME` to the name of the storage account.</span></span>
>
> <span data-ttu-id="29980-148">Det här exemplet används [curl (http://curl.haxx.se/)](http://curl.haxx.se/) och [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) att hämta och tolka JSON-data.</span><span class="sxs-lookup"><span data-stu-id="29980-148">This example uses [curl (http://curl.haxx.se/)](http://curl.haxx.se/) and [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) to retrieve and parse JSON data.</span></span>

<span data-ttu-id="29980-149">När det här kommandot ersätter __KLUSTERNAMN__ med namnet på HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-149">When using this command, replace __CLUSTERNAME__ with the name of the HDInsight cluster.</span></span> <span data-ttu-id="29980-150">Ersätt __lösenord__ med HTTP-inloggningslösenordet för klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-150">Replace __PASSWORD__ with the HTTP login password for the cluster.</span></span> <span data-ttu-id="29980-151">Ersätt __STORAGEACCOUNT__ med namnet på lagringskontot läggs till med skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="29980-151">Replace __STORAGEACCOUNT__ with the name of the storage account added using script action.</span></span> <span data-ttu-id="29980-152">Informationen som returneras från det här kommandot visas liknar följande:</span><span class="sxs-lookup"><span data-stu-id="29980-152">Information returned from this command appears similar to the following text:</span></span>

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

<span data-ttu-id="29980-153">Den här texten är ett exempel på en krypterad nyckel som används för att komma åt lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="29980-153">This text is an example of an encrypted key, which is used to access the storage account.</span></span>

### <a name="unable-to-access-storage-after-changing-key"></a><span data-ttu-id="29980-154">Det går inte att komma åt lagringsutrymme när du har ändrat nyckel</span><span class="sxs-lookup"><span data-stu-id="29980-154">Unable to access storage after changing key</span></span>

<span data-ttu-id="29980-155">Om du ändrar nyckeln för ett lagringskonto HDInsight inte längre komma åt lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="29980-155">If you change the key for a storage account, HDInsight can no longer access the storage account.</span></span> <span data-ttu-id="29980-156">HDInsight använder en cachelagrad kopia av nyckeln i core-site.xml för klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-156">HDInsight uses a cached copy of key in the core-site.xml for the cluster.</span></span> <span data-ttu-id="29980-157">Den här kopian måste uppdateras för att matcha den nya nyckeln.</span><span class="sxs-lookup"><span data-stu-id="29980-157">This cached copy must be updated to match the new key.</span></span>

<span data-ttu-id="29980-158">Kör skriptet igen har __inte__ uppdatera nyckeln, som skriptet kontrollerar om det redan finns en post för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="29980-158">Running the script action again does __not__ update the key, as the script checks to see if an entry for the storage account already exists.</span></span> <span data-ttu-id="29980-159">Om det finns redan en post, den inte göra några ändringar.</span><span class="sxs-lookup"><span data-stu-id="29980-159">If an entry already exists, it does not make any changes.</span></span>

<span data-ttu-id="29980-160">Om du vill undvika det här problemet måste du ta bort den befintliga posten för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="29980-160">To work around this problem, you must remove the existing entry for the storage account.</span></span> <span data-ttu-id="29980-161">Använd följande steg för att ta bort den befintliga posten:</span><span class="sxs-lookup"><span data-stu-id="29980-161">Use the following steps to remove the existing entry:</span></span>

1. <span data-ttu-id="29980-162">Öppna Ambari-Webbgränssnittet för ditt HDInsight-kluster i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="29980-162">In a web browser, open the Ambari Web UI for your HDInsight cluster.</span></span> <span data-ttu-id="29980-163">URI: N är https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="29980-163">The URI is https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="29980-164">Ersätt __CLUSTERNAME__ med namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-164">Replace __CLUSTERNAME__ with the name of your cluster.</span></span>

    <span data-ttu-id="29980-165">När du uppmanas du ange HTTP-inloggning och lösenord för klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-165">When prompted, enter the HTTP login user and password for your cluster.</span></span>

2. <span data-ttu-id="29980-166">I listan över tjänster till vänster på sidan Välj __HDFS__.</span><span class="sxs-lookup"><span data-stu-id="29980-166">From the list of services on the left of the page, select __HDFS__.</span></span> <span data-ttu-id="29980-167">Välj sedan den __konfigurationerna__ fliken i mitten på sidan.</span><span class="sxs-lookup"><span data-stu-id="29980-167">Then select the __Configs__ tab in the center of the page.</span></span>

3. <span data-ttu-id="29980-168">I den __Filter...__  , ange ett värde för __fs.azure.account__.</span><span class="sxs-lookup"><span data-stu-id="29980-168">In the __Filter...__ field, enter a value of __fs.azure.account__.</span></span> <span data-ttu-id="29980-169">Detta returnerar poster för alla ytterligare lagringskonton som har lagts till i klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-169">This returns entries for any additional storage accounts that have been added to the cluster.</span></span> <span data-ttu-id="29980-170">Det finns två typer av uppgifter. __keyprovider__ och __nyckeln__.</span><span class="sxs-lookup"><span data-stu-id="29980-170">There are two types of entries; __keyprovider__ and __key__.</span></span> <span data-ttu-id="29980-171">Båda innehåller namnet på lagringskontot som en del av namnet.</span><span class="sxs-lookup"><span data-stu-id="29980-171">Both contain the name of the storage account as part of the key name.</span></span>

    <span data-ttu-id="29980-172">Följande är Exempelposter för ett lagringskonto med namnet __mystorage__:</span><span class="sxs-lookup"><span data-stu-id="29980-172">The following are example entries for a storage account named __mystorage__:</span></span>

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. <span data-ttu-id="29980-173">När du har identifierat nycklarna för lagringskontot som du måste ta bort, kan du använda röda '-' ikonen till höger om posten ska tas bort.</span><span class="sxs-lookup"><span data-stu-id="29980-173">After you have identified the keys for the storage account you need to remove, use the red '-' icon to the right of the entry to delete it.</span></span> <span data-ttu-id="29980-174">Använd sedan den __spara__ för att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="29980-174">Then use the __Save__ button to save your changes.</span></span>

5. <span data-ttu-id="29980-175">När ändringarna har sparats, Använd skriptåtgärden att lägga till lagringskontot och nya nyckelvärdet i klustret.</span><span class="sxs-lookup"><span data-stu-id="29980-175">After changes have been saved, use the script action to add the storage account and new key value to the cluster.</span></span>

### <a name="poor-performance"></a><span data-ttu-id="29980-176">Dåliga prestanda</span><span class="sxs-lookup"><span data-stu-id="29980-176">Poor performance</span></span>

<span data-ttu-id="29980-177">Om lagringskontot finns i en annan region än HDInsight-kluster, kan det uppstå sämre prestanda.</span><span class="sxs-lookup"><span data-stu-id="29980-177">If the storage account is in a different region than the HDInsight cluster, you may experience poor performance.</span></span> <span data-ttu-id="29980-178">Åtkomst till data i en annan region skickar nätverkstrafik utanför regionala Azure-datacentret och över det offentliga internet som kan introducera svarstid.</span><span class="sxs-lookup"><span data-stu-id="29980-178">Accessing data in a different region sends network traffic outside the regional Azure data center and across the public internet, which can introduce latency.</span></span>

> [!WARNING]
> <span data-ttu-id="29980-179">Med hjälp av ett lagringskonto i en annan region än HDInsight-kluster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="29980-179">Using a storage account in a different region than the HDInsight cluster is not supported.</span></span>

### <a name="additional-charges"></a><span data-ttu-id="29980-180">Ytterligare kostnader</span><span class="sxs-lookup"><span data-stu-id="29980-180">Additional charges</span></span>

<span data-ttu-id="29980-181">Om lagringskontot finns i en annan region än HDInsight-kluster, ser du ytterligare utgång avgifter på ditt Azure fakturering.</span><span class="sxs-lookup"><span data-stu-id="29980-181">If the storage account is in a different region than the HDInsight cluster, you may notice additional egress charges on your Azure billing.</span></span> <span data-ttu-id="29980-182">En utgående kostnad tillämpas när data lämnar ett regionala datacenter.</span><span class="sxs-lookup"><span data-stu-id="29980-182">An egress charge is applied when data leaves a regional data center.</span></span> <span data-ttu-id="29980-183">Det här tillägget används även om trafiken är avsedda för en annan Azure-datacenter i en annan region.</span><span class="sxs-lookup"><span data-stu-id="29980-183">This charge is applied even if the traffic is destined for another Azure data center in a different region.</span></span>

> [!WARNING]
> <span data-ttu-id="29980-184">Med hjälp av ett lagringskonto i en annan region än HDInsight-kluster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="29980-184">Using a storage account in a different region than the HDInsight cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="29980-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29980-185">Next steps</span></span>

<span data-ttu-id="29980-186">Du har lärt dig hur du lägger till ytterligare lagringskonton i ett befintligt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="29980-186">You have learned how to add additional storage accounts to an existing HDInsight cluster.</span></span> <span data-ttu-id="29980-187">Mer information om skriptåtgärder finns [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="29980-187">For more information on script actions, see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>
