---
title: "aaaMigrate tooAzure Resource Manager-verktyg för HDInsight | Microsoft Docs"
description: "Hur toomigrate tooAzure Resource Manager development tools för HDInsight-kluster"
services: hdinsight
editor: cgronlun
manager: jhubbard
author: nitinme
documentationcenter: 
ms.assetid: 05efedb5-6456-4552-87ff-156d77fbe2e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: c087ae63d2544e5badae6be9c258f783aa92e2ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-tooazure-resource-manager-based-development-tools-for-hdinsight-clusters"></a><span data-ttu-id="4aef3-103">Migrera tooAzure Resource Manager-baserade utvecklingsverktyg för HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-103">Migrating tooAzure Resource Manager-based development tools for HDInsight clusters</span></span>

<span data-ttu-id="4aef3-104">HDInsight sluta Azure Service Manager ASM-baserat verktyg för HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4aef3-104">HDInsight is deprecating Azure Service Manager (ASM)-based tools for HDInsight.</span></span> <span data-ttu-id="4aef3-105">Om du har använt Azure PowerShell, Azure CLI eller hello HDInsight .NET SDK toowork med HDInsight-kluster, är du rekommenderar toouse hello Azure Resource Manager ARM-baserade versioner av PowerShell, CLI och .NET SDK framöver.</span><span class="sxs-lookup"><span data-stu-id="4aef3-105">If you have been using Azure PowerShell, Azure CLI, or hello HDInsight .NET SDK toowork with HDInsight clusters, you are encouraged toouse hello Azure Resource Manager (ARM)-based versions of PowerShell, CLI, and .NET SDK going forward.</span></span> <span data-ttu-id="4aef3-106">Den här artikeln innehåller tips om hur toomigrate toohello ny ARM-baserade metod.</span><span class="sxs-lookup"><span data-stu-id="4aef3-106">This article provides pointers on how toomigrate toohello new ARM-based approach.</span></span> <span data-ttu-id="4aef3-107">Tillämpliga fall lärt den här artikeln har även dig hello skillnaderna mellan hello ASM och ARM metoder för HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4aef3-107">Wherever applicable, this article also points out hello differences between hello ASM and ARM approaches for HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4aef3-108">hello-stöd för ASM baserat PowerShell, CLI och .NET SDK ska upphöra på **1 januari 2017**.</span><span class="sxs-lookup"><span data-stu-id="4aef3-108">hello support for ASM based PowerShell, CLI, and .NET SDK will discontinue on **January 1, 2017**.</span></span>
> 
> 

## <a name="migrating-azure-cli-tooazure-resource-manager"></a><span data-ttu-id="4aef3-109">Migrera Azure CLI tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4aef3-109">Migrating Azure CLI tooAzure Resource Manager</span></span>
<span data-ttu-id="4aef3-110">hello Azure CLI standardvärden nu tooAzure Resource Manager (ARM) läge, såvida du inte uppgraderar från en tidigare installation. i detta fall kan du behöva toouse hello `azure config mode arm` kommandot tooswitch tooARM läge.</span><span class="sxs-lookup"><span data-stu-id="4aef3-110">hello Azure CLI now defaults tooAzure Resource Manager (ARM) mode, unless you are upgrading from a previous installation; in this case, you may need toouse hello `azure config mode arm` command tooswitch tooARM mode.</span></span>

<span data-ttu-id="4aef3-111">grundläggande hello-kommandon som hello Azure CLI toowork medföljer HDInsight med hjälp av Azure Service Management (ASM) är samma hello när du använder ARM; men vissa parametrar och växlar kan ha nya namn och det finns många nya parametrar när du med hjälp av ARM.</span><span class="sxs-lookup"><span data-stu-id="4aef3-111">hello basic commands that hello Azure CLI provided toowork with HDInsight using Azure Service Management (ASM) are hello same when using ARM; however some parameters and switches may have new names, and there are many new parameters available when using ARM.</span></span> <span data-ttu-id="4aef3-112">Exempelvis kan du kan nu använda `azure hdinsight cluster create` toospecify hello Azure-nätverk som ska skapas i ett kluster eller Hive och Oozie metastore information.</span><span class="sxs-lookup"><span data-stu-id="4aef3-112">For example, you can now use `azure hdinsight cluster create` toospecify hello Azure Virtual Network that a cluster should be created in, or Hive and Oozie metastore information.</span></span>

<span data-ttu-id="4aef3-113">Grundläggande kommandon för att arbeta med HDInsight via Azure Resource Manager är:</span><span class="sxs-lookup"><span data-stu-id="4aef3-113">Basic commands for working with HDInsight through Azure Resource Manager are:</span></span>

* <span data-ttu-id="4aef3-114">`azure hdinsight cluster create`-skapar ett nytt HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-114">`azure hdinsight cluster create` - creates a new HDInsight cluster</span></span>
* <span data-ttu-id="4aef3-115">`azure hdinsight cluster delete`-tar bort ett befintligt HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-115">`azure hdinsight cluster delete` - deletes an existing HDInsight cluster</span></span>
* <span data-ttu-id="4aef3-116">`azure hdinsight cluster show`– Visa information om ett befintligt kluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-116">`azure hdinsight cluster show` - display information about an existing cluster</span></span>
* <span data-ttu-id="4aef3-117">`azure hdinsight cluster list`– Visar en lista över HDInsight-kluster för din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4aef3-117">`azure hdinsight cluster list` - lists HDInsight clusters for your Azure subscription</span></span>

<span data-ttu-id="4aef3-118">Använd hello `-h` växla tooinspect hello parametrar och växlar som är tillgängliga för varje kommando.</span><span class="sxs-lookup"><span data-stu-id="4aef3-118">Use hello `-h` switch tooinspect hello parameters and switches available for each command.</span></span>

### <a name="new-commands"></a><span data-ttu-id="4aef3-119">Nya kommandon</span><span class="sxs-lookup"><span data-stu-id="4aef3-119">New commands</span></span>
<span data-ttu-id="4aef3-120">Nya kommandon som är tillgängliga med Azure Resource Manager är:</span><span class="sxs-lookup"><span data-stu-id="4aef3-120">New commands available with Azure Resource Manager are:</span></span>

* <span data-ttu-id="4aef3-121">`azure hdinsight cluster resize`-dynamiskt ändringar hello antalet arbetarnoder i klustret hello</span><span class="sxs-lookup"><span data-stu-id="4aef3-121">`azure hdinsight cluster resize` - dynamically changes hello number of worker nodes in hello cluster</span></span>
* <span data-ttu-id="4aef3-122">`azure hdinsight cluster enable-http-access`-aktiverar HTTPs åtkomst toohello kluster (på som standard)</span><span class="sxs-lookup"><span data-stu-id="4aef3-122">`azure hdinsight cluster enable-http-access` - enables HTTPs access toohello cluster (on by default)</span></span>
* <span data-ttu-id="4aef3-123">`azure hdinsight cluster disable-http-access`-inaktiverar HTTPs åtkomst toohello kluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-123">`azure hdinsight cluster disable-http-access` - disables HTTPs access toohello cluster</span></span>
* <span data-ttu-id="4aef3-124">`azure hdinsight script-action`-innehåller kommandon för att skapa/hantera skriptåtgärder på ett kluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-124">`azure hdinsight script-action` - provides commands for creating/managing Script Actions on a cluster</span></span>
* <span data-ttu-id="4aef3-125">`azure hdinsight config`-innehåller kommandon för att skapa en konfigurationsfil som kan användas med hello `hdinsight cluster create` kommandot tooprovide konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="4aef3-125">`azure hdinsight config` - provides commands for creating a configuration file that can be used with hello `hdinsight cluster create` command tooprovide configuration information.</span></span>

### <a name="deprecated-commands"></a><span data-ttu-id="4aef3-126">Föråldrade kommandon</span><span class="sxs-lookup"><span data-stu-id="4aef3-126">Deprecated commands</span></span>
<span data-ttu-id="4aef3-127">Om du använder hello `azure hdinsight job` kommandon toosubmit jobb tooyour HDInsight-kluster dessa inte är tillgängliga via hello ARM-kommandon.</span><span class="sxs-lookup"><span data-stu-id="4aef3-127">If you use hello `azure hdinsight job` commands toosubmit jobs tooyour HDInsight cluster, these are not available through hello ARM commands.</span></span> <span data-ttu-id="4aef3-128">Om du behöver tooprogrammatically skicka jobb tooHDInsight från skript, bör du använda hello REST API: er som tillhandahålls av HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4aef3-128">If you need tooprogrammatically submit jobs tooHDInsight from scripts, you should instead use hello REST APIs provided by HDInsight.</span></span> <span data-ttu-id="4aef3-129">Mer information om att skicka jobb med hjälp av REST API: er finns i hello följande dokument.</span><span class="sxs-lookup"><span data-stu-id="4aef3-129">For more information on submitting jobs using REST APIs, see hello following documents.</span></span>

* [<span data-ttu-id="4aef3-130">Kör jobb för MapReduce med Hadoop i HDInsight med cURL</span><span class="sxs-lookup"><span data-stu-id="4aef3-130">Run MapReduce jobs with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-mapreduce-curl.md)
* [<span data-ttu-id="4aef3-131">Köra Hive-frågor med Hadoop i HDInsight med cURL</span><span class="sxs-lookup"><span data-stu-id="4aef3-131">Run Hive queries with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-hive-curl.md)
* [<span data-ttu-id="4aef3-132">Köra Pig med Hadoop på HDInsight med cURL</span><span class="sxs-lookup"><span data-stu-id="4aef3-132">Run Pig jobs with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-pig-curl.md)

<span data-ttu-id="4aef3-133">Information om andra sätt toorun MapReduce Hive, och svin interaktivt, se [använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md), [använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md), och [använda Pig med Hadoop på HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="4aef3-133">For information on other ways toorun MapReduce, Hive, and Pig interactively, see [Use MapReduce with Hadoop on HDInsight](hdinsight-use-mapreduce.md), [Use Hive with Hadoop on HDInsight](hdinsight-use-hive.md), and [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

### <a name="examples"></a><span data-ttu-id="4aef3-134">Exempel</span><span class="sxs-lookup"><span data-stu-id="4aef3-134">Examples</span></span>
<span data-ttu-id="4aef3-135">**Skapar ett kluster**</span><span class="sxs-lookup"><span data-stu-id="4aef3-135">**Creating a cluster**</span></span>

* <span data-ttu-id="4aef3-136">Gamla kommando (ASM)-`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`</span><span class="sxs-lookup"><span data-stu-id="4aef3-136">Old command (ASM) - `azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`</span></span>
* <span data-ttu-id="4aef3-137">Nytt kommando (ARM)-`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`</span><span class="sxs-lookup"><span data-stu-id="4aef3-137">New command (ARM) - `azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`</span></span>

<span data-ttu-id="4aef3-138">**Tar bort ett kluster**</span><span class="sxs-lookup"><span data-stu-id="4aef3-138">**Deleting a cluster**</span></span>

* <span data-ttu-id="4aef3-139">Gamla kommando (ASM)-`azure hdinsight cluster delete myhdicluster`</span><span class="sxs-lookup"><span data-stu-id="4aef3-139">Old command (ASM) - `azure hdinsight cluster delete myhdicluster`</span></span>
* <span data-ttu-id="4aef3-140">Nytt kommando (ARM)-`azure hdinsight cluster delete mycluster -g myresourcegroup`</span><span class="sxs-lookup"><span data-stu-id="4aef3-140">New command (ARM) - `azure hdinsight cluster delete mycluster -g myresourcegroup`</span></span>

<span data-ttu-id="4aef3-141">**Lista över kluster**</span><span class="sxs-lookup"><span data-stu-id="4aef3-141">**List clusters**</span></span>

* <span data-ttu-id="4aef3-142">Gamla kommando (ASM)-`azure hdinsight cluster list`</span><span class="sxs-lookup"><span data-stu-id="4aef3-142">Old command (ASM) - `azure hdinsight cluster list`</span></span>
* <span data-ttu-id="4aef3-143">Nytt kommando (ARM)-`azure hdinsight cluster list`</span><span class="sxs-lookup"><span data-stu-id="4aef3-143">New command (ARM) - `azure hdinsight cluster list`</span></span>

> [!NOTE]
> <span data-ttu-id="4aef3-144">För hello listan kommando anger hello resursen med `-g` returnerar hello-kluster i hello angivna resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="4aef3-144">For hello list command, specifying hello resource group using `-g` will return only hello clusters in hello specified resource group.</span></span>
> 
> 

<span data-ttu-id="4aef3-145">**Visa information om klustret**</span><span class="sxs-lookup"><span data-stu-id="4aef3-145">**Show cluster information**</span></span>

* <span data-ttu-id="4aef3-146">Gamla kommando (ASM)-`azure hdinsight cluster show myhdicluster`</span><span class="sxs-lookup"><span data-stu-id="4aef3-146">Old command (ASM) - `azure hdinsight cluster show myhdicluster`</span></span>
* <span data-ttu-id="4aef3-147">Nytt kommando (ARM)-`azure hdinsight cluster show myhdicluster -g myresourcegroup`</span><span class="sxs-lookup"><span data-stu-id="4aef3-147">New command (ARM) - `azure hdinsight cluster show myhdicluster -g myresourcegroup`</span></span>

## <a name="migrating-azure-powershell-tooazure-resource-manager"></a><span data-ttu-id="4aef3-148">Migrera Azure PowerShell tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4aef3-148">Migrating Azure PowerShell tooAzure Resource Manager</span></span>
<span data-ttu-id="4aef3-149">hello allmän information om Azure PowerShell i hello Azure Resource Manager (ARM) läge finns på [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="4aef3-149">hello general information about Azure PowerShell in hello Azure Resource Manager (ARM) mode can be found at [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="4aef3-150">hello Azure PowerShell ARM-cmdlets kan vara installerade sida-vid-sida med hello ASM-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="4aef3-150">hello Azure PowerShell ARM cmdlets can be installed side-by-side with hello ASM cmdlets.</span></span> <span data-ttu-id="4aef3-151">hello-cmdlets från hello två lägen kan särskiljas genom deras namn.</span><span class="sxs-lookup"><span data-stu-id="4aef3-151">hello cmdlets from hello two modes can be distinguished by their names.</span></span>  <span data-ttu-id="4aef3-152">hello ARM-läge har *AzureRmHDInsight* i hello cmdlet-namnen för att jämföra*AzureHDInsight* i hello ASM-läge.</span><span class="sxs-lookup"><span data-stu-id="4aef3-152">hello ARM mode has *AzureRmHDInsight* in hello cmdlet names comparing too*AzureHDInsight* in hello ASM mode.</span></span>  <span data-ttu-id="4aef3-153">Till exempel *ny AzureRmHDInsightCluster* vs. *Nya AzureHDInsightCluster*.</span><span class="sxs-lookup"><span data-stu-id="4aef3-153">For example, *New-AzureRmHDInsightCluster* vs. *New-AzureHDInsightCluster*.</span></span> <span data-ttu-id="4aef3-154">Parametrar och växlar som kan ha nyheter namn och det finns många nya parametrar när du med hjälp av ARM.</span><span class="sxs-lookup"><span data-stu-id="4aef3-154">Parameters and switches may have news names, and there are many new parameters available when using ARM.</span></span>  <span data-ttu-id="4aef3-155">Till exempel flera cmdlets kräver en ny växel kallas *- ResourceGroupName*.</span><span class="sxs-lookup"><span data-stu-id="4aef3-155">For example, several cmdlets require a new switch called *-ResourceGroupName*.</span></span> 

<span data-ttu-id="4aef3-156">Innan du kan använda hello HDInsight cmdlets, måste du ansluta tooyour Azure-konto och skapa en ny resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="4aef3-156">Before you can use hello HDInsight cmdlets, you must connect tooyour Azure account, and create a new resource group:</span></span>

* <span data-ttu-id="4aef3-157">Login-AzureRmAccount eller [Välj AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx).</span><span class="sxs-lookup"><span data-stu-id="4aef3-157">Login-AzureRmAccount or [Select-AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx).</span></span> <span data-ttu-id="4aef3-158">Se [autentiserar ett huvudnamn för tjänsten med Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md)</span><span class="sxs-lookup"><span data-stu-id="4aef3-158">See [Authenticating a service principal with Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md)</span></span>
* [<span data-ttu-id="4aef3-159">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4aef3-159">New-AzureRmResourceGroup</span></span>](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a><span data-ttu-id="4aef3-160">Har bytt namn till cmdlet: ar</span><span class="sxs-lookup"><span data-stu-id="4aef3-160">Renamed cmdlets</span></span>
<span data-ttu-id="4aef3-161">toolist hello HDInsight ASM-cmdlets i Windows PowerShell-konsol:</span><span class="sxs-lookup"><span data-stu-id="4aef3-161">toolist hello HDInsight ASM cmdlets in Windows PowerShell console:</span></span>

    help *azurermhdinsight*

<span data-ttu-id="4aef3-162">hello följande tabell visas hello ASM-cmdlets och deras namn i hello ARM-läge:</span><span class="sxs-lookup"><span data-stu-id="4aef3-162">hello following table lists hello ASM cmdlets and their names in hello ARM mode:</span></span>

| <span data-ttu-id="4aef3-163">ASM-cmdlets</span><span class="sxs-lookup"><span data-stu-id="4aef3-163">ASM cmdlets</span></span> | <span data-ttu-id="4aef3-164">ARM-cmdlets</span><span class="sxs-lookup"><span data-stu-id="4aef3-164">ARM cmdlets</span></span> |
| --- | --- |
| <span data-ttu-id="4aef3-165">Add-AzureHDInsightConfigValues</span><span class="sxs-lookup"><span data-stu-id="4aef3-165">Add-AzureHDInsightConfigValues</span></span> |[<span data-ttu-id="4aef3-166">Lägg till AzureRmHDInsightConfigValues</span><span class="sxs-lookup"><span data-stu-id="4aef3-166">Add-AzureRmHDInsightConfigValues</span></span>](https://msdn.microsoft.com/library/mt603530.aspx) |
| <span data-ttu-id="4aef3-167">Add-AzureHDInsightMetastore</span><span class="sxs-lookup"><span data-stu-id="4aef3-167">Add-AzureHDInsightMetastore</span></span> |[<span data-ttu-id="4aef3-168">Lägg till AzureRmHDInsightMetastore</span><span class="sxs-lookup"><span data-stu-id="4aef3-168">Add-AzureRmHDInsightMetastore</span></span>](https://msdn.microsoft.com/library/mt603670.aspx) |
| <span data-ttu-id="4aef3-169">Add-AzureHDInsightScriptAction</span><span class="sxs-lookup"><span data-stu-id="4aef3-169">Add-AzureHDInsightScriptAction</span></span> |[<span data-ttu-id="4aef3-170">Lägg till AzureRmHDInsightScriptAction</span><span class="sxs-lookup"><span data-stu-id="4aef3-170">Add-AzureRmHDInsightScriptAction</span></span>](https://msdn.microsoft.com/library/mt603527.aspx) |
| <span data-ttu-id="4aef3-171">Add-AzureHDInsightStorage</span><span class="sxs-lookup"><span data-stu-id="4aef3-171">Add-AzureHDInsightStorage</span></span> |[<span data-ttu-id="4aef3-172">Lägg till AzureRmHDInsightStorage</span><span class="sxs-lookup"><span data-stu-id="4aef3-172">Add-AzureRmHDInsightStorage</span></span>](https://msdn.microsoft.com/library/mt619445.aspx) |
| <span data-ttu-id="4aef3-173">Get-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-173">Get-AzureHDInsightCluster</span></span> |[<span data-ttu-id="4aef3-174">Get-AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-174">Get-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619371.aspx) |
| <span data-ttu-id="4aef3-175">Get-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="4aef3-175">Get-AzureHDInsightJob</span></span> |[<span data-ttu-id="4aef3-176">Get-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="4aef3-176">Get-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603590.aspx) |
| <span data-ttu-id="4aef3-177">Get-AzureHDInsightJobOutput</span><span class="sxs-lookup"><span data-stu-id="4aef3-177">Get-AzureHDInsightJobOutput</span></span> |[<span data-ttu-id="4aef3-178">Get-AzureRmHDInsightJobOutput</span><span class="sxs-lookup"><span data-stu-id="4aef3-178">Get-AzureRmHDInsightJobOutput</span></span>](https://msdn.microsoft.com/library/mt603793.aspx) |
| <span data-ttu-id="4aef3-179">Get-AzureHDInsightProperties</span><span class="sxs-lookup"><span data-stu-id="4aef3-179">Get-AzureHDInsightProperties</span></span> |[<span data-ttu-id="4aef3-180">Get-AzureRmHDInsightProperties</span><span class="sxs-lookup"><span data-stu-id="4aef3-180">Get-AzureRmHDInsightProperties</span></span>](https://msdn.microsoft.com/library/mt603546.aspx) |
| <span data-ttu-id="4aef3-181">Grant-AzureHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="4aef3-181">Grant-AzureHDInsightHttpServicesAccess</span></span> |[<span data-ttu-id="4aef3-182">Bevilja AzureRmHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="4aef3-182">Grant-AzureRmHDInsightHttpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt619407.aspx) |
| <span data-ttu-id="4aef3-183">Grant-AzureHdinsightRdpAccess</span><span class="sxs-lookup"><span data-stu-id="4aef3-183">Grant-AzureHdinsightRdpAccess</span></span> |[<span data-ttu-id="4aef3-184">Bevilja AzureRmHDInsightRdpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="4aef3-184">Grant-AzureRmHDInsightRdpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt603717.aspx) |
| <span data-ttu-id="4aef3-185">Invoke-AzureHDInsightHiveJob</span><span class="sxs-lookup"><span data-stu-id="4aef3-185">Invoke-AzureHDInsightHiveJob</span></span> |[<span data-ttu-id="4aef3-186">Anropa AzureRmHDInsightHiveJob</span><span class="sxs-lookup"><span data-stu-id="4aef3-186">Invoke-AzureRmHDInsightHiveJob</span></span>](https://msdn.microsoft.com/library/mt603593.aspx) |
| <span data-ttu-id="4aef3-187">New-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-187">New-AzureHDInsightCluster</span></span> |[<span data-ttu-id="4aef3-188">Ny AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-188">New-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619331.aspx) |
| <span data-ttu-id="4aef3-189">New-AzureHDInsightClusterConfig</span><span class="sxs-lookup"><span data-stu-id="4aef3-189">New-AzureHDInsightClusterConfig</span></span> |[<span data-ttu-id="4aef3-190">Ny AzureRmHDInsightClusterConfig</span><span class="sxs-lookup"><span data-stu-id="4aef3-190">New-AzureRmHDInsightClusterConfig</span></span>](https://msdn.microsoft.com/library/mt603700.aspx) |
| <span data-ttu-id="4aef3-191">New-AzureHDInsightHiveJobDefinition</span><span class="sxs-lookup"><span data-stu-id="4aef3-191">New-AzureHDInsightHiveJobDefinition</span></span> |[<span data-ttu-id="4aef3-192">Ny AzureRmHDInsightHiveJobDefinition</span><span class="sxs-lookup"><span data-stu-id="4aef3-192">New-AzureRmHDInsightHiveJobDefinition</span></span>](https://msdn.microsoft.com/library/mt619448.aspx) |
| <span data-ttu-id="4aef3-193">New-AzureHDInsightMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="4aef3-193">New-AzureHDInsightMapReduceJobDefinition</span></span> |[<span data-ttu-id="4aef3-194">Ny AzureRmHDInsightMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="4aef3-194">New-AzureRmHDInsightMapReduceJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603626.aspx) |
| <span data-ttu-id="4aef3-195">New-AzureHDInsightPigJobDefinition</span><span class="sxs-lookup"><span data-stu-id="4aef3-195">New-AzureHDInsightPigJobDefinition</span></span> |[<span data-ttu-id="4aef3-196">Ny AzureRmHDInsightPigJobDefinition</span><span class="sxs-lookup"><span data-stu-id="4aef3-196">New-AzureRmHDInsightPigJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603671.aspx) |
| <span data-ttu-id="4aef3-197">New-AzureHDInsightSqoopJobDefinition</span><span class="sxs-lookup"><span data-stu-id="4aef3-197">New-AzureHDInsightSqoopJobDefinition</span></span> |[<span data-ttu-id="4aef3-198">Ny AzureRmHDInsightSqoopJobDefinition</span><span class="sxs-lookup"><span data-stu-id="4aef3-198">New-AzureRmHDInsightSqoopJobDefinition</span></span>](https://msdn.microsoft.com/library/mt608551.aspx) |
| <span data-ttu-id="4aef3-199">New-AzureHDInsightStreamingMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="4aef3-199">New-AzureHDInsightStreamingMapReduceJobDefinition</span></span> |[<span data-ttu-id="4aef3-200">Ny AzureRmHDInsightStreamingMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="4aef3-200">New-AzureRmHDInsightStreamingMapReduceJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603626.aspx) |
| <span data-ttu-id="4aef3-201">Remove-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-201">Remove-AzureHDInsightCluster</span></span> |[<span data-ttu-id="4aef3-202">Ta bort AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-202">Remove-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619431.aspx) |
| <span data-ttu-id="4aef3-203">Revoke-AzureHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="4aef3-203">Revoke-AzureHDInsightHttpServicesAccess</span></span> |[<span data-ttu-id="4aef3-204">Återkalla AzureRmHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="4aef3-204">Revoke-AzureRmHDInsightHttpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt619375.aspx) |
| <span data-ttu-id="4aef3-205">Revoke-AzureHdinsightRdpAccess</span><span class="sxs-lookup"><span data-stu-id="4aef3-205">Revoke-AzureHdinsightRdpAccess</span></span> |[<span data-ttu-id="4aef3-206">Återkalla AzureRmHDInsightRdpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="4aef3-206">Revoke-AzureRmHDInsightRdpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt603523.aspx) |
| <span data-ttu-id="4aef3-207">Set-AzureHDInsightClusterSize</span><span class="sxs-lookup"><span data-stu-id="4aef3-207">Set-AzureHDInsightClusterSize</span></span> |[<span data-ttu-id="4aef3-208">Ange AzureRmHDInsightClusterSize</span><span class="sxs-lookup"><span data-stu-id="4aef3-208">Set-AzureRmHDInsightClusterSize</span></span>](https://msdn.microsoft.com/library/mt603513.aspx) |
| <span data-ttu-id="4aef3-209">Set-AzureHDInsightDefaultStorage</span><span class="sxs-lookup"><span data-stu-id="4aef3-209">Set-AzureHDInsightDefaultStorage</span></span> |[<span data-ttu-id="4aef3-210">Ange AzureRmHDInsightDefaultStorage</span><span class="sxs-lookup"><span data-stu-id="4aef3-210">Set-AzureRmHDInsightDefaultStorage</span></span>](https://msdn.microsoft.com/library/mt603486.aspx) |
| <span data-ttu-id="4aef3-211">Start-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="4aef3-211">Start-AzureHDInsightJob</span></span> |[<span data-ttu-id="4aef3-212">Start-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="4aef3-212">Start-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603798.aspx) |
| <span data-ttu-id="4aef3-213">Stop-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="4aef3-213">Stop-AzureHDInsightJob</span></span> |[<span data-ttu-id="4aef3-214">Stoppa AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="4aef3-214">Stop-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt619424.aspx) |
| <span data-ttu-id="4aef3-215">Use-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-215">Use-AzureHDInsightCluster</span></span> |[<span data-ttu-id="4aef3-216">Använd AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-216">Use-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619442.aspx) |
| <span data-ttu-id="4aef3-217">Wait-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="4aef3-217">Wait-AzureHDInsightJob</span></span> |[<span data-ttu-id="4aef3-218">Vänta AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="4aef3-218">Wait-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603834.aspx) |

### <a name="new-cmdlets"></a><span data-ttu-id="4aef3-219">Nya cmdletar</span><span class="sxs-lookup"><span data-stu-id="4aef3-219">New cmdlets</span></span>
<span data-ttu-id="4aef3-220">hello följande är nya hello-cmdlets som endast är tillgängliga i hello ARM-läge.</span><span class="sxs-lookup"><span data-stu-id="4aef3-220">hello following are hello new cmdlets that are only available in hello ARM mode.</span></span> 

<span data-ttu-id="4aef3-221">**Skriptåtgärder relaterade cmdlets:**</span><span class="sxs-lookup"><span data-stu-id="4aef3-221">**Script action related cmdlets:**</span></span>

* <span data-ttu-id="4aef3-222">**Get-AzureRmHDInsightPersistedScriptAction**: hämtar hello beständiga skriptåtgärder för ett kluster och visar dem i kronologisk ordning eller hämtar information för en angiven beständiga skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="4aef3-222">**Get-AzureRmHDInsightPersistedScriptAction**: Gets hello persisted script actions for a cluster and lists them in chronological order, or gets details for a specified persisted script action.</span></span> 
* <span data-ttu-id="4aef3-223">**Get-AzureRmHDInsightScriptActionHistory**: hämtar hello skriptåtgärdshistoriken för ett kluster och visas i omvänd kronologisk ordning eller hämtar information om en tidigare skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="4aef3-223">**Get-AzureRmHDInsightScriptActionHistory**: Gets hello script action history for a cluster and lists it in reverse chronological order, or gets details of a previously executed script action.</span></span> 
* <span data-ttu-id="4aef3-224">**Ta bort AzureRmHDInsightPersistedScriptAction**: tar bort en beständiga skriptåtgärder från ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4aef3-224">**Remove-AzureRmHDInsightPersistedScriptAction**: Removes a persisted script action from an HDInsight cluster.</span></span>
* <span data-ttu-id="4aef3-225">**Ange AzureRmHDInsightPersistedScriptAction**: Anger en tidigare körda skript åtgärd toobe beständiga skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="4aef3-225">**Set-AzureRmHDInsightPersistedScriptAction**: Sets a previously executed script action toobe a persisted script action.</span></span>
* <span data-ttu-id="4aef3-226">**Skicka AzureRmHDInsightScriptAction**: skickar ett nytt skript åtgärd tooan Azure HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4aef3-226">**Submit-AzureRmHDInsightScriptAction**: Submits a new script action tooan Azure HDInsight cluster.</span></span> 

<span data-ttu-id="4aef3-227">Av ytterligare användningsinformation finns i [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="4aef3-227">For additional usage information, see [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="4aef3-228">**Clsuter identitet relaterade cmdlets:**</span><span class="sxs-lookup"><span data-stu-id="4aef3-228">**Clsuter identity related cmdlets:**</span></span>

* <span data-ttu-id="4aef3-229">**Lägg till AzureRmHDInsightClusterIdentity**: lägger till ett konfigurationsobjekt för klustret identitet tooa klustret så att hello HDInsight-kluster kan komma åt Azure Data Lake butiker.</span><span class="sxs-lookup"><span data-stu-id="4aef3-229">**Add-AzureRmHDInsightClusterIdentity**: Adds a cluster identity tooa cluster configuration object so that hello HDInsight cluster can access Azure Data Lake Stores.</span></span> <span data-ttu-id="4aef3-230">Se [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="4aef3-230">See [Create an HDInsight cluster with Data Lake Store using Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).</span></span>

### <a name="examples"></a><span data-ttu-id="4aef3-231">Exempel</span><span class="sxs-lookup"><span data-stu-id="4aef3-231">Examples</span></span>
<span data-ttu-id="4aef3-232">**Skapa kluster**</span><span class="sxs-lookup"><span data-stu-id="4aef3-232">**Create cluster**</span></span>

<span data-ttu-id="4aef3-233">Gamla kommando (ASM):</span><span class="sxs-lookup"><span data-stu-id="4aef3-233">Old command (ASM):</span></span> 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

<span data-ttu-id="4aef3-234">Nytt kommando (ARM):</span><span class="sxs-lookup"><span data-stu-id="4aef3-234">New command (ARM):</span></span>

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials


<span data-ttu-id="4aef3-235">**Ta bort klustret**</span><span class="sxs-lookup"><span data-stu-id="4aef3-235">**Delete cluster**</span></span>

<span data-ttu-id="4aef3-236">Gamla kommando (ASM):</span><span class="sxs-lookup"><span data-stu-id="4aef3-236">Old command (ASM):</span></span>

    Remove-AzureHDInsightCluster -name $clusterName 

<span data-ttu-id="4aef3-237">Nytt kommando (ARM):</span><span class="sxs-lookup"><span data-stu-id="4aef3-237">New command (ARM):</span></span>

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

<span data-ttu-id="4aef3-238">**Lista över kluster**</span><span class="sxs-lookup"><span data-stu-id="4aef3-238">**List cluster**</span></span>

<span data-ttu-id="4aef3-239">Gamla kommando (ASM):</span><span class="sxs-lookup"><span data-stu-id="4aef3-239">Old command (ASM):</span></span>

    Get-AzureHDInsightCluster

<span data-ttu-id="4aef3-240">Nytt kommando (ARM):</span><span class="sxs-lookup"><span data-stu-id="4aef3-240">New command (ARM):</span></span>

    Get-AzureRmHDInsightCluster 

<span data-ttu-id="4aef3-241">**Visa kluster**</span><span class="sxs-lookup"><span data-stu-id="4aef3-241">**Show cluster**</span></span>

<span data-ttu-id="4aef3-242">Gamla kommando (ASM):</span><span class="sxs-lookup"><span data-stu-id="4aef3-242">Old command (ASM):</span></span>

    Get-AzureHDInsightCluster -Name $clusterName

<span data-ttu-id="4aef3-243">Nytt kommando (ARM):</span><span class="sxs-lookup"><span data-stu-id="4aef3-243">New command (ARM):</span></span>

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a><span data-ttu-id="4aef3-244">Andra exempel</span><span class="sxs-lookup"><span data-stu-id="4aef3-244">Other samples</span></span>
* [<span data-ttu-id="4aef3-245">Skapa HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="4aef3-245">Create HDInsight clusters</span></span>](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [<span data-ttu-id="4aef3-246">Skicka Hive-jobb</span><span class="sxs-lookup"><span data-stu-id="4aef3-246">Submit Hive jobs</span></span>](hdinsight-hadoop-use-hive-powershell.md)
* [<span data-ttu-id="4aef3-247">Skicka Pig-jobb</span><span class="sxs-lookup"><span data-stu-id="4aef3-247">Submit Pig jobs</span></span>](hdinsight-hadoop-use-pig-powershell.md)
* [<span data-ttu-id="4aef3-248">Skicka Sqoop jobb</span><span class="sxs-lookup"><span data-stu-id="4aef3-248">Submit Sqoop jobs</span></span>](hdinsight-hadoop-use-sqoop-powershell.md)

## <a name="migrating-toohello-arm-based-hdinsight-net-sdk"></a><span data-ttu-id="4aef3-249">Migrera toohello ARM-baserade HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-249">Migrating toohello ARM-based HDInsight .NET SDK</span></span>
<span data-ttu-id="4aef3-250">hello Azure Service Management-baserade [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) är nu föråldrad.</span><span class="sxs-lookup"><span data-stu-id="4aef3-250">hello Azure Service Management-based [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) is now deprecated.</span></span> <span data-ttu-id="4aef3-251">Du är rekommenderar toouse hello Azure Resource Manager-baserade [(ARM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="4aef3-251">You are encouraged toouse hello Azure Resource Management-based [(ARM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx).</span></span> <span data-ttu-id="4aef3-252">hello används följande ASM-baserat HDInsight-paket inte.</span><span class="sxs-lookup"><span data-stu-id="4aef3-252">hello following ASM-based HDInsight packages are being deprecated.</span></span>

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

<span data-ttu-id="4aef3-253">Det här avsnittet innehåller pekare toomore information om hur tooperform vissa aktiviteter med hjälp av hello ARM-baserade SDK.</span><span class="sxs-lookup"><span data-stu-id="4aef3-253">This section provides pointers toomore information on how tooperform certain tasks using hello ARM-based SDK.</span></span>

| <span data-ttu-id="4aef3-254">Så här... med hello ARM-baserade HDInsight SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-254">How to... using hello ARM-based HDInsight SDK</span></span> | <span data-ttu-id="4aef3-255">Länkar</span><span class="sxs-lookup"><span data-stu-id="4aef3-255">Links</span></span> |
| --- | --- |
| <span data-ttu-id="4aef3-256">Skapa HDInsight-kluster med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-256">Create HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="4aef3-257">Se [skapa HDInsight-kluster med hjälp av .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="4aef3-257">See [Create HDInsight clusters using .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="4aef3-258">Anpassa ett kluster med skriptåtgärder med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-258">Customize a cluster using Script Action with .NET SDK</span></span> |<span data-ttu-id="4aef3-259">Se [anpassa HDInsight Linux-kluster med skriptåtgärder](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action)</span><span class="sxs-lookup"><span data-stu-id="4aef3-259">See [Customize HDInsight Linux clusters using Script Action](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action)</span></span> |
| <span data-ttu-id="4aef3-260">Autentisera program interaktivt genom att använda Azure Active Directory med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-260">Authenticate applications interactively using Azure Active Directory with .NET SDK</span></span> |<span data-ttu-id="4aef3-261">Se [köra Hive-frågor med .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="4aef3-261">See [Run Hive queries using .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span> <span data-ttu-id="4aef3-262">hello kodstycket i den här artikeln använder hello interaktiv autentisering metod.</span><span class="sxs-lookup"><span data-stu-id="4aef3-262">hello code snippet in this article uses hello interactive authentication approach.</span></span> |
| <span data-ttu-id="4aef3-263">Autentisera program interaktivt genom att använda Azure Active Directory med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-263">Authenticate applications non-interactively using Azure Active Directory with .NET SDK</span></span> |<span data-ttu-id="4aef3-264">Se [skapa icke-interaktiva program för HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md)</span><span class="sxs-lookup"><span data-stu-id="4aef3-264">See [Create non-interactive applications for HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md)</span></span> |
| <span data-ttu-id="4aef3-265">Skicka ett Hive-jobb med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-265">Submit a Hive job using .NET SDK</span></span> |<span data-ttu-id="4aef3-266">Se [skicka Hive-jobb](hdinsight-hadoop-use-hive-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="4aef3-266">See [Submit Hive jobs](hdinsight-hadoop-use-hive-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="4aef3-267">Skicka Pig-jobbet med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-267">Submit a Pig job using .NET SDK</span></span> |<span data-ttu-id="4aef3-268">Se [skicka Pig-jobb](hdinsight-hadoop-use-pig-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="4aef3-268">See [Submit Pig jobs](hdinsight-hadoop-use-pig-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="4aef3-269">Skicka ett Sqoop-jobb med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-269">Submit a Sqoop job using .NET SDK</span></span> |<span data-ttu-id="4aef3-270">Se [skicka Sqoop jobb](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="4aef3-270">See [Submit Sqoop jobs](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="4aef3-271">Visa en lista med HDInsight-kluster med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-271">List HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="4aef3-272">Se [listan HDInsight-kluster](hdinsight-administer-use-dotnet-sdk.md#list-clusters)</span><span class="sxs-lookup"><span data-stu-id="4aef3-272">See [List HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#list-clusters)</span></span> |
| <span data-ttu-id="4aef3-273">Skala HDInsight-kluster med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-273">Scale HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="4aef3-274">Se [skala HDInsight-kluster](hdinsight-administer-use-dotnet-sdk.md#scale-clusters)</span><span class="sxs-lookup"><span data-stu-id="4aef3-274">See [Scale HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#scale-clusters)</span></span> |
| <span data-ttu-id="4aef3-275">Bevilja/återkalla åtkomst tooHDInsight kluster med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-275">Grant/revoke access tooHDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="4aef3-276">Se [bevilja/återkalla åtkomst tooHDInsight kluster](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access)</span><span class="sxs-lookup"><span data-stu-id="4aef3-276">See [Grant/revoke access tooHDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access)</span></span> |
| <span data-ttu-id="4aef3-277">Uppdatera http-autentiseringsuppgifterna för HDInsight-kluster med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-277">Update HTTP user credentials for HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="4aef3-278">Se [uppdatering HTTP användarautentiseringsuppgifter för HDInsight-kluster](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials)</span><span class="sxs-lookup"><span data-stu-id="4aef3-278">See [Update HTTP user credentials for HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials)</span></span> |
| <span data-ttu-id="4aef3-279">Hitta hello standardkontot för lagring för HDInsight-kluster med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-279">Find hello default storage account for HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="4aef3-280">Se [hitta hello standardkontot för lagring för HDInsight-kluster](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account)</span><span class="sxs-lookup"><span data-stu-id="4aef3-280">See [Find hello default storage account for HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account)</span></span> |
| <span data-ttu-id="4aef3-281">Ta bort HDInsight-kluster med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4aef3-281">Delete HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="4aef3-282">Se [ta bort HDInsight-kluster](hdinsight-administer-use-dotnet-sdk.md#delete-clusters)</span><span class="sxs-lookup"><span data-stu-id="4aef3-282">See [Delete HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#delete-clusters)</span></span> |

### <a name="examples"></a><span data-ttu-id="4aef3-283">Exempel</span><span class="sxs-lookup"><span data-stu-id="4aef3-283">Examples</span></span>
<span data-ttu-id="4aef3-284">Nedan följer några exempel på hur en åtgärd är utförs med hjälp av hello ASM-baserade SDK och hello motsvarande kodstycke för hello ARM-baserade SDK.</span><span class="sxs-lookup"><span data-stu-id="4aef3-284">Following are some examples on how an operation is performed using hello ASM-based SDK and hello equivalent code snippet for hello ARM-based SDK.</span></span>

<span data-ttu-id="4aef3-285">**Skapar ett kluster CRUD-klient**</span><span class="sxs-lookup"><span data-stu-id="4aef3-285">**Creating a cluster CRUD client**</span></span>

* <span data-ttu-id="4aef3-286">Gamla kommando (ASM)</span><span class="sxs-lookup"><span data-stu-id="4aef3-286">Old command (ASM)</span></span>
  
        //Certificate auth
        //This logs hello application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* <span data-ttu-id="4aef3-287">Nytt kommando (ARM) (Service principal auktorisering)</span><span class="sxs-lookup"><span data-stu-id="4aef3-287">New command (ARM) (Service principal authorization)</span></span>
  
        //Service principal auth
        //This will log hello application in as itself, rather than on behalf of a specific user.
        //For details, including how tooset up hello application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* <span data-ttu-id="4aef3-288">Nytt kommando (ARM) (auktoriseringen av användare)</span><span class="sxs-lookup"><span data-stu-id="4aef3-288">New command (ARM) (User authorization)</span></span>
  
        //User auth
        //This will log hello application in on behalf of hello user.
        //hello end-user will see a login popup.
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);

<span data-ttu-id="4aef3-289">**Skapar ett kluster**</span><span class="sxs-lookup"><span data-stu-id="4aef3-289">**Creating a cluster**</span></span>

* <span data-ttu-id="4aef3-290">Gamla kommando (ASM)</span><span class="sxs-lookup"><span data-stu-id="4aef3-290">Old command (ASM)</span></span>
  
        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);
* <span data-ttu-id="4aef3-291">Nytt kommando (ARM)</span><span class="sxs-lookup"><span data-stu-id="4aef3-291">New command (ARM)</span></span>
  
        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Linux,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);

<span data-ttu-id="4aef3-292">**Aktivera HTTP-åtkomst**</span><span class="sxs-lookup"><span data-stu-id="4aef3-292">**Enabling HTTP access**</span></span>

* <span data-ttu-id="4aef3-293">Gamla kommando (ASM)</span><span class="sxs-lookup"><span data-stu-id="4aef3-293">Old command (ASM)</span></span>
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* <span data-ttu-id="4aef3-294">Nytt kommando (ARM)</span><span class="sxs-lookup"><span data-stu-id="4aef3-294">New command (ARM)</span></span>
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

<span data-ttu-id="4aef3-295">**Tar bort ett kluster**</span><span class="sxs-lookup"><span data-stu-id="4aef3-295">**Deleting a cluster**</span></span>

* <span data-ttu-id="4aef3-296">Gamla kommando (ASM)</span><span class="sxs-lookup"><span data-stu-id="4aef3-296">Old command (ASM)</span></span>
  
        client.DeleteCluster(dnsName);
* <span data-ttu-id="4aef3-297">Nytt kommando (ARM)</span><span class="sxs-lookup"><span data-stu-id="4aef3-297">New command (ARM)</span></span>
  
        client.Clusters.Delete(resourceGroup, dnsname);

