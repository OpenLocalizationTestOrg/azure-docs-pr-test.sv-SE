---
title: aaaAdd ytterligare Azure storage-konton tooHDInsight | Microsoft Docs
description: "Lär dig hur tooadd ytterligare Azure storage-konton tooan befintligt HDInsight-kluster."
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
ms.openlocfilehash: ce5acfa4b61bf7e83b1fb374d64a1eaa3182fbec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-additional-storage-accounts-toohdinsight"></a><span data-ttu-id="61ab6-103">Lägga till ytterligare lagringsutrymme konton tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="61ab6-103">Add additional storage accounts tooHDInsight</span></span>

<span data-ttu-id="61ab6-104">Lär dig hur toouse skript åtgärder tooadd ytterligare Azure storage-konton tooHDInsight.</span><span class="sxs-lookup"><span data-stu-id="61ab6-104">Learn how toouse script actions tooadd additional Azure storage accounts tooHDInsight.</span></span> <span data-ttu-id="61ab6-105">hello stegen i det här dokumentet lägga till ett storage-konto tooan befintliga Linux-baserat HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="61ab6-105">hello steps in this document add a storage account tooan existing Linux-based HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61ab6-106">hello informationen i det här dokumentet handlar om att lägga till ytterligare lagringsutrymme tooa klustret när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="61ab6-106">hello information in this document is about adding additional storage tooa cluster after it has been created.</span></span> <span data-ttu-id="61ab6-107">Mer information om att lägga till lagringskonton när klustret skapas finns [ställa in kluster i HDInsight Hadoop, Spark, Kafka och mycket mer](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="61ab6-107">For information on adding storage accounts during cluster creation, see [Set up clusters in HDInsight with Hadoop, Spark, Kafka, and more](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="how-it-works"></a><span data-ttu-id="61ab6-108">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="61ab6-108">How it works</span></span>

<span data-ttu-id="61ab6-109">Det här skriptet tar hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="61ab6-109">This script takes hello following parameters:</span></span>

* <span data-ttu-id="61ab6-110">__Azure lagringskontonamnet__: hello namnet på hello storage-konto tooadd toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="61ab6-110">__Azure storage account name__: hello name of hello storage account tooadd toohello HDInsight cluster.</span></span> <span data-ttu-id="61ab6-111">När du har kört skriptet hello kan HDInsight läsa och skriva data lagras i det här lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="61ab6-111">After running hello script, HDInsight can read and write data stored in this storage account.</span></span>

* <span data-ttu-id="61ab6-112">__Azure lagringskontonyckel__: en nyckel som beviljar åtkomst toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="61ab6-112">__Azure storage account key__: A key that grants access toohello storage account.</span></span>

* <span data-ttu-id="61ab6-113">__-p__ (valfritt): Om du hello nyckeln krypteras inte och lagras i hello core-site.xml filen som oformaterad text.</span><span class="sxs-lookup"><span data-stu-id="61ab6-113">__-p__ (optional): If specified, hello key is not encrypted and is stored in hello core-site.xml file as plain text.</span></span>

<span data-ttu-id="61ab6-114">Under bearbetning av utför hello skript hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="61ab6-114">During processing, hello script performs hello following actions:</span></span>

* <span data-ttu-id="61ab6-115">Om hello storage-konto finns redan i hello core-site.xml konfigurationen för hello kluster, hello skriptet avslutas och inga ytterligare åtgärder utförs.</span><span class="sxs-lookup"><span data-stu-id="61ab6-115">If hello storage account already exists in hello core-site.xml configuration for hello cluster, hello script exits and no further actions are performed.</span></span>

* <span data-ttu-id="61ab6-116">Verifierar att hello lagringskontot finns och kan nås med hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="61ab6-116">Verifies that hello storage account exists and can be accessed using hello key.</span></span>

* <span data-ttu-id="61ab6-117">Krypterar hello nyckeln med hjälp av autentiseringsuppgifter för hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="61ab6-117">Encrypts hello key using hello cluster credential.</span></span>

* <span data-ttu-id="61ab6-118">Lägger till hello konto toohello core-site.xml lagringsfilen.</span><span class="sxs-lookup"><span data-stu-id="61ab6-118">Adds hello storage account toohello core-site.xml file.</span></span>

* <span data-ttu-id="61ab6-119">Stoppar och startar om hello Oozie, YARN, MapReduce2 och HDFS-tjänster.</span><span class="sxs-lookup"><span data-stu-id="61ab6-119">Stops and restarts hello Oozie, YARN, MapReduce2, and HDFS services.</span></span> <span data-ttu-id="61ab6-120">Stoppa och starta dessa tjänster kan de toouse hello nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="61ab6-120">Stopping and starting these services allows them toouse hello new storage account.</span></span>

> [!WARNING]
> <span data-ttu-id="61ab6-121">Med hjälp av ett lagringskonto i en annan plats än hello HDInsight-kluster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="61ab6-121">Using a storage account in a different location than hello HDInsight cluster is not supported.</span></span>

## <a name="hello-script"></a><span data-ttu-id="61ab6-122">hello-skript</span><span class="sxs-lookup"><span data-stu-id="61ab6-122">hello script</span></span>

<span data-ttu-id="61ab6-123">__Script plats__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)</span><span class="sxs-lookup"><span data-stu-id="61ab6-123">__Script location__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)</span></span>

<span data-ttu-id="61ab6-124">__Krav för__:</span><span class="sxs-lookup"><span data-stu-id="61ab6-124">__Requirements__:</span></span>

* <span data-ttu-id="61ab6-125">hello skriptet måste tillämpas på hello __huvudnoder__.</span><span class="sxs-lookup"><span data-stu-id="61ab6-125">hello script must be applied on hello __Head nodes__.</span></span>

## <a name="toouse-hello-script"></a><span data-ttu-id="61ab6-126">toouse hello skript</span><span class="sxs-lookup"><span data-stu-id="61ab6-126">toouse hello script</span></span>

<span data-ttu-id="61ab6-127">Det här skriptet kan användas från hello Azure-portalen, Azure PowerShell eller hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="61ab6-127">This script can be used from hello Azure portal, Azure PowerShell, or hello Azure CLI 1.0.</span></span> <span data-ttu-id="61ab6-128">Mer information finns i hello [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="61ab6-128">For more information, see hello [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61ab6-129">Använd följande information tooapply hello skriptet när du använder hello stegen i hello anpassning dokument:</span><span class="sxs-lookup"><span data-stu-id="61ab6-129">When using hello steps provided in hello customization document, use hello following information tooapply this script:</span></span>
>
> * <span data-ttu-id="61ab6-130">Ersätt alla exempel skriptåtgärd URI med hello URI för det här skriptet (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).</span><span class="sxs-lookup"><span data-stu-id="61ab6-130">Replace any example script action URI with hello URI for this script (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).</span></span>
> * <span data-ttu-id="61ab6-131">Ersätt alla exempel parametrar med hello Azure storage-kontonamnet och nyckeln hello konto toobe tillagda toohello av klustret för lagring.</span><span class="sxs-lookup"><span data-stu-id="61ab6-131">Replace any example parameters with hello Azure storage account name and key of hello storage account toobe added toohello cluster.</span></span> <span data-ttu-id="61ab6-132">Om du använder hello Azure-portalen, måste parametrarna avgränsas med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="61ab6-132">If using hello Azure portal, these parameters must be separated by a space.</span></span>
> * <span data-ttu-id="61ab6-133">Du behöver inte toomark skriptet som __bevarade__, enligt den direkt uppdaterar hello Ambari konfigurationen för hello kluster.</span><span class="sxs-lookup"><span data-stu-id="61ab6-133">You do not need toomark this script as __Persisted__, as it directly updates hello Ambari configuration for hello cluster.</span></span>

## <a name="known-issues"></a><span data-ttu-id="61ab6-134">Kända problem</span><span class="sxs-lookup"><span data-stu-id="61ab6-134">Known issues</span></span>

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a><span data-ttu-id="61ab6-135">Storage-konton som inte visas i Azure-portalen eller verktyg</span><span class="sxs-lookup"><span data-stu-id="61ab6-135">Storage accounts not displayed in Azure portal or tools</span></span>

<span data-ttu-id="61ab6-136">När Visa hello HDInsight-klustret i hello Azure-portalen, välja hello __Lagringskonton__ posten under __egenskaper__ storage-konton som lagts till via den här skriptåtgärden visas inte.</span><span class="sxs-lookup"><span data-stu-id="61ab6-136">When viewing hello HDInsight cluster in hello Azure portal, selecting hello __Storage Accounts__ entry under __Properties__ does not display storage accounts added through this script action.</span></span> <span data-ttu-id="61ab6-137">Azure PowerShell och Azure CLI visas inte hello ytterligare storage-konto.</span><span class="sxs-lookup"><span data-stu-id="61ab6-137">Azure PowerShell and Azure CLI do not display hello additional storage account either.</span></span>

<span data-ttu-id="61ab6-138">hello storage-informationen visas inte eftersom hello skript ändras endast hello core-site.xml konfigurationen för hello kluster.</span><span class="sxs-lookup"><span data-stu-id="61ab6-138">hello storage information isn't displayed because hello script only modifies hello core-site.xml configuration for hello cluster.</span></span> <span data-ttu-id="61ab6-139">Den här informationen används inte vid hämtning av hello klustret information med hjälp av Azure management API: er.</span><span class="sxs-lookup"><span data-stu-id="61ab6-139">This information is not used when retrieving hello cluster information using Azure management APIs.</span></span>

<span data-ttu-id="61ab6-140">information om lagringskontot tooview lägga toohello kluster med hjälp av det här skriptet kan använda hello Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="61ab6-140">tooview storage account information added toohello cluster using this script, use hello Ambari REST API.</span></span> <span data-ttu-id="61ab6-141">Använd följande kommandon tooretrieve hello informationen för klustret:</span><span class="sxs-lookup"><span data-stu-id="61ab6-141">Use hello following commands tooretrieve this information for your cluster:</span></span>

```PowerShell
$creds = Get-Credential -UserName "admin" -Message "Enter hello cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$storageAccountName.blob.core.windows.net"
```

> [!NOTE]
> <span data-ttu-id="61ab6-142">Ange `$clusterName` toohello namnet på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="61ab6-142">Set `$clusterName` toohello name of hello HDInsight cluster.</span></span> <span data-ttu-id="61ab6-143">Ange `$storageAccountName` toohello namnet på hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="61ab6-143">Set `$storageAccountName` toohello name of hello storage account.</span></span> <span data-ttu-id="61ab6-144">När du uppmanas ange hello klusterinloggning (admin) och lösenord.</span><span class="sxs-lookup"><span data-stu-id="61ab6-144">When prompted, enter hello cluster login (admin) and password.</span></span>

```Bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.azure.account.key.$STORAGEACCOUNTNAME.blob.core.windows.net"] | select(. != null)'
```

> [!NOTE]
> <span data-ttu-id="61ab6-145">Ange `$PASSWORD` toohello klustret inloggning (admin) lösenord.</span><span class="sxs-lookup"><span data-stu-id="61ab6-145">Set `$PASSWORD` toohello cluster login (admin) account password.</span></span> <span data-ttu-id="61ab6-146">Ange `$CLUSTERNAME` toohello namnet på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="61ab6-146">Set `$CLUSTERNAME` toohello name of hello HDInsight cluster.</span></span> <span data-ttu-id="61ab6-147">Ange `$STORAGEACCOUNTNAME` toohello namnet på hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="61ab6-147">Set `$STORAGEACCOUNTNAME` toohello name of hello storage account.</span></span>
>
> <span data-ttu-id="61ab6-148">Det här exemplet används [curl (http://curl.haxx.se/)](http://curl.haxx.se/) och [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) tooretrieve och parsa JSON-data.</span><span class="sxs-lookup"><span data-stu-id="61ab6-148">This example uses [curl (http://curl.haxx.se/)](http://curl.haxx.se/) and [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) tooretrieve and parse JSON data.</span></span>

<span data-ttu-id="61ab6-149">När det här kommandot ersätter __KLUSTERNAMN__ med hello namnet hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="61ab6-149">When using this command, replace __CLUSTERNAME__ with hello name of hello HDInsight cluster.</span></span> <span data-ttu-id="61ab6-150">Ersätt __lösenord__ med hello HTTP inloggningslösenordet för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="61ab6-150">Replace __PASSWORD__ with hello HTTP login password for hello cluster.</span></span> <span data-ttu-id="61ab6-151">Ersätt __STORAGEACCOUNT__ med hello namn som lagts till med skriptåtgärder hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="61ab6-151">Replace __STORAGEACCOUNT__ with hello name of hello storage account added using script action.</span></span> <span data-ttu-id="61ab6-152">Informationen som returneras från det här kommandot visas liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="61ab6-152">Information returned from this command appears similar toohello following text:</span></span>

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

<span data-ttu-id="61ab6-153">Den här texten är ett exempel på en krypterad nyckel som används för tooaccess hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="61ab6-153">This text is an example of an encrypted key, which is used tooaccess hello storage account.</span></span>

### <a name="unable-tooaccess-storage-after-changing-key"></a><span data-ttu-id="61ab6-154">Det går inte tooaccess lagring när du har ändrat nyckel</span><span class="sxs-lookup"><span data-stu-id="61ab6-154">Unable tooaccess storage after changing key</span></span>

<span data-ttu-id="61ab6-155">Om du ändrar hello nyckeln för ett lagringskonto HDInsight inte längre komma åt hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="61ab6-155">If you change hello key for a storage account, HDInsight can no longer access hello storage account.</span></span> <span data-ttu-id="61ab6-156">HDInsight använder en cachelagrad kopia av nyckeln i hello core-site.xml för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="61ab6-156">HDInsight uses a cached copy of key in hello core-site.xml for hello cluster.</span></span> <span data-ttu-id="61ab6-157">Den här kopian måste vara uppdaterade toomatch hello ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="61ab6-157">This cached copy must be updated toomatch hello new key.</span></span>

<span data-ttu-id="61ab6-158">Hello skriptet igen körs __inte__ uppdatera hello nyckeln som hello skriptet kontrollerar toosee om det finns redan en post för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="61ab6-158">Running hello script action again does __not__ update hello key, as hello script checks toosee if an entry for hello storage account already exists.</span></span> <span data-ttu-id="61ab6-159">Om det finns redan en post, den inte göra några ändringar.</span><span class="sxs-lookup"><span data-stu-id="61ab6-159">If an entry already exists, it does not make any changes.</span></span>

<span data-ttu-id="61ab6-160">toowork problemet måste du ta bort hello befintlig post för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="61ab6-160">toowork around this problem, you must remove hello existing entry for hello storage account.</span></span> <span data-ttu-id="61ab6-161">Använd följande steg tooremove hello befintlig post hello:</span><span class="sxs-lookup"><span data-stu-id="61ab6-161">Use hello following steps tooremove hello existing entry:</span></span>

1. <span data-ttu-id="61ab6-162">Öppna hello Ambari-Webbgränssnittet för ditt HDInsight-kluster i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="61ab6-162">In a web browser, open hello Ambari Web UI for your HDInsight cluster.</span></span> <span data-ttu-id="61ab6-163">hello URI är https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="61ab6-163">hello URI is https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="61ab6-164">Ersätt __KLUSTERNAMN__ med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="61ab6-164">Replace __CLUSTERNAME__ with hello name of your cluster.</span></span>

    <span data-ttu-id="61ab6-165">När du uppmanas ange hello HTTP-inloggning och lösenord för klustret.</span><span class="sxs-lookup"><span data-stu-id="61ab6-165">When prompted, enter hello HTTP login user and password for your cluster.</span></span>

2. <span data-ttu-id="61ab6-166">Hello listan över tjänster hello till vänster i hello sidan Välj __HDFS__.</span><span class="sxs-lookup"><span data-stu-id="61ab6-166">From hello list of services on hello left of hello page, select __HDFS__.</span></span> <span data-ttu-id="61ab6-167">Välj hello __konfigurationerna__ fliken i hello center hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="61ab6-167">Then select hello __Configs__ tab in hello center of hello page.</span></span>

3. <span data-ttu-id="61ab6-168">I hello __Filter...__  , ange ett värde för __fs.azure.account__.</span><span class="sxs-lookup"><span data-stu-id="61ab6-168">In hello __Filter...__ field, enter a value of __fs.azure.account__.</span></span> <span data-ttu-id="61ab6-169">Detta returnerar poster för alla ytterligare lagringskonton som har lagts till toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="61ab6-169">This returns entries for any additional storage accounts that have been added toohello cluster.</span></span> <span data-ttu-id="61ab6-170">Det finns två typer av uppgifter. __keyprovider__ och __nyckeln__.</span><span class="sxs-lookup"><span data-stu-id="61ab6-170">There are two types of entries; __keyprovider__ and __key__.</span></span> <span data-ttu-id="61ab6-171">Innehåller båda hello namnet på hello lagringskonto som en del av hello nyckelnamn.</span><span class="sxs-lookup"><span data-stu-id="61ab6-171">Both contain hello name of hello storage account as part of hello key name.</span></span>

    <span data-ttu-id="61ab6-172">hello följande är Exempelposter för ett lagringskonto med namnet __mystorage__:</span><span class="sxs-lookup"><span data-stu-id="61ab6-172">hello following are example entries for a storage account named __mystorage__:</span></span>

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. <span data-ttu-id="61ab6-173">När du har identifierat hello nycklar för hello storage-konto behöver du tooremove använda hello red '-' toohello ikonen till höger om hello post toodelete den.</span><span class="sxs-lookup"><span data-stu-id="61ab6-173">After you have identified hello keys for hello storage account you need tooremove, use hello red '-' icon toohello right of hello entry toodelete it.</span></span> <span data-ttu-id="61ab6-174">Använd hello __spara__ knappen toosave ändringarna.</span><span class="sxs-lookup"><span data-stu-id="61ab6-174">Then use hello __Save__ button toosave your changes.</span></span>

5. <span data-ttu-id="61ab6-175">När ändringarna har sparats, använda hello skript åtgärd tooadd hello storage-konto och nya nyckelvärdet toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="61ab6-175">After changes have been saved, use hello script action tooadd hello storage account and new key value toohello cluster.</span></span>

### <a name="poor-performance"></a><span data-ttu-id="61ab6-176">Dåliga prestanda</span><span class="sxs-lookup"><span data-stu-id="61ab6-176">Poor performance</span></span>

<span data-ttu-id="61ab6-177">Om hello storage-konto finns i en annan region än hello HDInsight-kluster, kan det uppstå sämre prestanda.</span><span class="sxs-lookup"><span data-stu-id="61ab6-177">If hello storage account is in a different region than hello HDInsight cluster, you may experience poor performance.</span></span> <span data-ttu-id="61ab6-178">Åtkomst till data i en annan region skickar trafik utanför hello regionala Azure-datacenter och tvärs över hello offentliga internet, som kan introducera svarstid.</span><span class="sxs-lookup"><span data-stu-id="61ab6-178">Accessing data in a different region sends network traffic outside hello regional Azure data center and across hello public internet, which can introduce latency.</span></span>

> [!WARNING]
> <span data-ttu-id="61ab6-179">Med hjälp av ett lagringskonto i en annan region än hello HDInsight-kluster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="61ab6-179">Using a storage account in a different region than hello HDInsight cluster is not supported.</span></span>

### <a name="additional-charges"></a><span data-ttu-id="61ab6-180">Ytterligare kostnader</span><span class="sxs-lookup"><span data-stu-id="61ab6-180">Additional charges</span></span>

<span data-ttu-id="61ab6-181">Om hello storage-konto har en annan region än hello HDInsight-kluster, kan du eventuellt se ytterligare utgång avgifter på ditt Azure fakturering.</span><span class="sxs-lookup"><span data-stu-id="61ab6-181">If hello storage account is in a different region than hello HDInsight cluster, you may notice additional egress charges on your Azure billing.</span></span> <span data-ttu-id="61ab6-182">En utgående kostnad tillämpas när data lämnar ett regionala datacenter.</span><span class="sxs-lookup"><span data-stu-id="61ab6-182">An egress charge is applied when data leaves a regional data center.</span></span> <span data-ttu-id="61ab6-183">Det här tillägget används även om hello trafik är avsedda för en annan Azure-datacenter i en annan region.</span><span class="sxs-lookup"><span data-stu-id="61ab6-183">This charge is applied even if hello traffic is destined for another Azure data center in a different region.</span></span>

> [!WARNING]
> <span data-ttu-id="61ab6-184">Med hjälp av ett lagringskonto i en annan region än hello HDInsight-kluster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="61ab6-184">Using a storage account in a different region than hello HDInsight cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="61ab6-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="61ab6-185">Next steps</span></span>

<span data-ttu-id="61ab6-186">Du har lärt dig hur tooadd ytterligare lagringskonton tooan befintligt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="61ab6-186">You have learned how tooadd additional storage accounts tooan existing HDInsight cluster.</span></span> <span data-ttu-id="61ab6-187">Mer information om skriptåtgärder finns [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="61ab6-187">For more information on script actions, see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>
