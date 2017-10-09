---
title: aaaScript utveckling med HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toocustomize Hadoop-kluster med skriptåtgärder. Skriptåtgärder kan vara används tooinstall ytterligare programvara som körs på en Hadoop-kluster eller toochange hello konfiguration av program på ett kluster."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 836d68a8-8b21-4d69-8b61-281a7fe67f21
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 4fc3a389df8a003f7129ab00b4cd9bc7ad81a419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a><span data-ttu-id="4d17f-104">Utveckla skriptåtgärd skript för HDInsight Windows-baserade kluster</span><span class="sxs-lookup"><span data-stu-id="4d17f-104">Develop Script Action scripts for HDInsight Windows-based clusters</span></span>
<span data-ttu-id="4d17f-105">Lär dig hur toowrite skriptåtgärd skript för HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4d17f-105">Learn how toowrite Script Action scripts for HDInsight.</span></span> <span data-ttu-id="4d17f-106">Information om hur du använder skriptåtgärd skript finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="4d17f-106">For information on using Script Action scripts, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md).</span></span> <span data-ttu-id="4d17f-107">Hello samma artikel skrivna för Linux-baserade HDInsight-kluster finns i [utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="4d17f-107">For hello same article written for Linux-based HDInsight clusters, see [Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions-linux.md).</span></span>



> [!IMPORTANT]
> <span data-ttu-id="4d17f-108">hello stegen i det här dokumentet endast fungerar för Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4d17f-108">hello steps in this document only work for Windows-based HDInsight clusters.</span></span> <span data-ttu-id="4d17f-109">HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="4d17f-109">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="4d17f-110">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4d17f-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="4d17f-111">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="4d17f-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="4d17f-112">Information om användning av skriptåtgärder med Linux-baserade kluster finns i [utveckling av skriptåtgärder med HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="4d17f-112">For information on using script actions with Linux-based clusters, see [Script action development with HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).</span></span>
>
>



<span data-ttu-id="4d17f-113">Skriptåtgärder kan vara används tooinstall ytterligare programvara som körs på en Hadoop-kluster eller toochange hello konfiguration av program på ett kluster.</span><span class="sxs-lookup"><span data-stu-id="4d17f-113">Script Action can be used tooinstall additional software running on a Hadoop cluster or toochange hello configuration of applications installed on a cluster.</span></span> <span data-ttu-id="4d17f-114">Med skriptåtgärder avses skript som körs på hello klusternoder när HDInsight-kluster har distribuerats och de utförs när noderna i klustret hello Slutför HDInsight-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="4d17f-114">Script actions are scripts that run on hello cluster nodes when HDInsight clusters are deployed, and they are executed once nodes in hello cluster complete HDInsight configuration.</span></span> <span data-ttu-id="4d17f-115">En skriptåtgärd körs under kontot systemadministratörsprivilegier och ger fullständig behörighet toohello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="4d17f-115">A script action is executed under system admin account privileges and provides full access rights toohello cluster nodes.</span></span> <span data-ttu-id="4d17f-116">Varje kluster kan anges med en lista över skript åtgärder toobe körs i hello ordning som de har angetts.</span><span class="sxs-lookup"><span data-stu-id="4d17f-116">Each cluster can be provided with a list of script actions toobe executed in hello order in which they are specified.</span></span>

> [!NOTE]
> <span data-ttu-id="4d17f-117">Om du får följande felmeddelande hello:</span><span class="sxs-lookup"><span data-stu-id="4d17f-117">If you experience hello following error message:</span></span>
>
> <span data-ttu-id="4d17f-118">System.Management.Automation.CommandNotFoundException; ExceptionMessage: hello termen ”spara HDIFile' identifieras inte som hello namnet för en cmdlet, funktion, skriptfilen eller körbart program.</span><span class="sxs-lookup"><span data-stu-id="4d17f-118">System.Management.Automation.CommandNotFoundException; ExceptionMessage : hello term 'Save-HDIFile' is not recognized as hello name of a cmdlet, function, script file, or operable program.</span></span> <span data-ttu-id="4d17f-119">Hello stavning hello namn eller om en sökväg ingick Kontrollera hello sökvägen är korrekt och försök igen.</span><span class="sxs-lookup"><span data-stu-id="4d17f-119">Check hello spelling of hello name, or if a path was included, verify that hello path is correct and try again.</span></span>
> <span data-ttu-id="4d17f-120">Det beror på att du inte inkludera hello hjälpmetoder.</span><span class="sxs-lookup"><span data-stu-id="4d17f-120">It is because you didn't include hello helper methods.</span></span>  <span data-ttu-id="4d17f-121">Se [hjälpmetoder för anpassade skript](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).</span><span class="sxs-lookup"><span data-stu-id="4d17f-121">See [Helper methods for custom scripts](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).</span></span>
>
>

## <a name="sample-scripts"></a><span data-ttu-id="4d17f-122">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="4d17f-122">Sample scripts</span></span>
<span data-ttu-id="4d17f-123">För att skapa HDInsight-kluster på Windows operativsystem, är hello skriptåtgärd Azure PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="4d17f-123">For creating HDInsight clusters on Windows operating system, hello Script Action is Azure PowerShell script.</span></span> <span data-ttu-id="4d17f-124">hello är följande skript ett exempel för att konfigurera hello plats konfigurationsfiler:</span><span class="sxs-lookup"><span data-stu-id="4d17f-124">hello following script is a sample for configuring hello site configuration files:</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable tooconfigure $ConfigFileName because it is not part of hello HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

<span data-ttu-id="4d17f-125">hello skriptet använder fyra parametrar, hello konfigurationsfilnamnet, hello-egenskap som du vill använda toomodify hello-värde som du vill tooset och en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="4d17f-125">hello script takes four parameters, hello configuration file name, hello property you want toomodify, hello value you want tooset, and a description.</span></span> <span data-ttu-id="4d17f-126">Exempel:</span><span class="sxs-lookup"><span data-stu-id="4d17f-126">For example:</span></span>

    hive-site.xml hive.metastore.client.socket.timeout 90

<span data-ttu-id="4d17f-127">Dessa parametrar anger hello hive.metastore.client.socket.timeout värdet too90 i hello hive-site.xml-filen.</span><span class="sxs-lookup"><span data-stu-id="4d17f-127">These parameters sets hello hive.metastore.client.socket.timeout value too90 in hello hive-site.xml file.</span></span>  <span data-ttu-id="4d17f-128">hello standardvärdet är 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="4d17f-128">hello default value is 60 seconds.</span></span>

<span data-ttu-id="4d17f-129">Det här exempelskriptet finns även på [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).</span><span class="sxs-lookup"><span data-stu-id="4d17f-129">This sample script can also be found at [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).</span></span>

<span data-ttu-id="4d17f-130">HDInsight tillhandahåller flera skript tooinstall ytterligare komponenter i HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="4d17f-130">HDInsight provides several scripts tooinstall additional components on HDInsight clusters:</span></span>

| <span data-ttu-id="4d17f-131">Namn</span><span class="sxs-lookup"><span data-stu-id="4d17f-131">Name</span></span> | <span data-ttu-id="4d17f-132">Skript</span><span class="sxs-lookup"><span data-stu-id="4d17f-132">Script</span></span> |
| --- | --- |
| <span data-ttu-id="4d17f-133">**Installera Spark**</span><span class="sxs-lookup"><span data-stu-id="4d17f-133">**Install Spark**</span></span> |<span data-ttu-id="4d17f-134">https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1.</span><span class="sxs-lookup"><span data-stu-id="4d17f-134">https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1.</span></span> <span data-ttu-id="4d17f-135">Se [installera och använda Spark i HDInsight-kluster][hdinsight-install-spark].</span><span class="sxs-lookup"><span data-stu-id="4d17f-135">See [Install and use Spark on HDInsight clusters][hdinsight-install-spark].</span></span> |
| <span data-ttu-id="4d17f-136">**Installera R**</span><span class="sxs-lookup"><span data-stu-id="4d17f-136">**Install R**</span></span> |<span data-ttu-id="4d17f-137">https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1.</span><span class="sxs-lookup"><span data-stu-id="4d17f-137">https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1.</span></span> <span data-ttu-id="4d17f-138">Se [installera och använda R i HDInsight-kluster][hdinsight-r-scripts].</span><span class="sxs-lookup"><span data-stu-id="4d17f-138">See [Install and use R on HDInsight clusters][hdinsight-r-scripts].</span></span> |
| <span data-ttu-id="4d17f-139">**Installera Solr**</span><span class="sxs-lookup"><span data-stu-id="4d17f-139">**Install Solr**</span></span> |<span data-ttu-id="4d17f-140">https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="4d17f-140">https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1.</span></span> <span data-ttu-id="4d17f-141">Se [installerar och använder Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="4d17f-141">See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span> |
| <span data-ttu-id="4d17f-142">- **Installera Giraph**</span><span class="sxs-lookup"><span data-stu-id="4d17f-142">- **Install Giraph**</span></span> |<span data-ttu-id="4d17f-143">https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="4d17f-143">https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1.</span></span> <span data-ttu-id="4d17f-144">Se [installerar och använder Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="4d17f-144">See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span> |

<span data-ttu-id="4d17f-145">Skriptåtgärder kan distribueras från hello Azure-portalen, Azure PowerShell eller med hjälp av hello HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="4d17f-145">Script Action can be deployed from hello Azure portal, Azure PowerShell or by using hello HDInsight .NET SDK.</span></span>  <span data-ttu-id="4d17f-146">Mer information finns i [anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize].</span><span class="sxs-lookup"><span data-stu-id="4d17f-146">For more information, see [Customize HDInsight clusters using Script Action][hdinsight-cluster-customize].</span></span>

> [!NOTE]
> <span data-ttu-id="4d17f-147">hello exempelskript fungerar bara med HDInsight-kluster av version 3.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4d17f-147">hello sample scripts work only with HDInsight cluster version 3.1 or above.</span></span> <span data-ttu-id="4d17f-148">Mer information om HDInsight-kluster-versioner finns [HDInsight-kluster-versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="4d17f-148">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>
>
>

## <a name="helper-methods-for-custom-scripts"></a><span data-ttu-id="4d17f-149">Hjälpmetoder för anpassade skript</span><span class="sxs-lookup"><span data-stu-id="4d17f-149">Helper methods for custom scripts</span></span>
<span data-ttu-id="4d17f-150">Skriptet åtgärd helper metoder är verktyg som du kan använda vid skrivning till anpassade skript.</span><span class="sxs-lookup"><span data-stu-id="4d17f-150">Script Action helper methods are utilities that you can use while writing custom scripts.</span></span> <span data-ttu-id="4d17f-151">Dessa metoder är definierade i [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1), och kan ingå i ditt skript med hjälp av hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="4d17f-151">These methods are defined in [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1), and can be included in your scripts using hello following sample:</span></span>

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module toomake writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed tooload HDInsightUtilities module, exiting ...";
        exit;
    }

<span data-ttu-id="4d17f-152">Här följer hello hjälpmetoder som tillhandahålls av skriptet:</span><span class="sxs-lookup"><span data-stu-id="4d17f-152">Here are hello helper methods that are provided by this script:</span></span>

| <span data-ttu-id="4d17f-153">Hjälpmetod</span><span class="sxs-lookup"><span data-stu-id="4d17f-153">Helper method</span></span> | <span data-ttu-id="4d17f-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4d17f-154">Description</span></span> |
| --- | --- |
| <span data-ttu-id="4d17f-155">**Spara HDIFile**</span><span class="sxs-lookup"><span data-stu-id="4d17f-155">**Save-HDIFile**</span></span> |<span data-ttu-id="4d17f-156">Hämta en fil från hello angetts identifierare URI (Uniform Resource) tooa plats på hello lokal disk som är associerad med kluster för hello Azure VM noder tilldelade toohello.</span><span class="sxs-lookup"><span data-stu-id="4d17f-156">Download a file from hello specified Uniform Resource Identifier (URI) tooa location on hello local disk that is associated with hello Azure VM node assigned toohello cluster.</span></span> |
| <span data-ttu-id="4d17f-157">**Expandera HDIZippedFile**</span><span class="sxs-lookup"><span data-stu-id="4d17f-157">**Expand-HDIZippedFile**</span></span> |<span data-ttu-id="4d17f-158">Packa upp ZIP.</span><span class="sxs-lookup"><span data-stu-id="4d17f-158">Unzip a zipped file.</span></span> |
| <span data-ttu-id="4d17f-159">**Anropa HDICmdScript**</span><span class="sxs-lookup"><span data-stu-id="4d17f-159">**Invoke-HDICmdScript**</span></span> |<span data-ttu-id="4d17f-160">Köra ett skript från cmd.exe.</span><span class="sxs-lookup"><span data-stu-id="4d17f-160">Run a script from cmd.exe.</span></span> |
| <span data-ttu-id="4d17f-161">**Skriv HDILog**</span><span class="sxs-lookup"><span data-stu-id="4d17f-161">**Write-HDILog**</span></span> |<span data-ttu-id="4d17f-162">Skriva utdata från hello anpassade skript som används för en skriptåtgärd.</span><span class="sxs-lookup"><span data-stu-id="4d17f-162">Write output from hello custom script used for a script action.</span></span> |
| <span data-ttu-id="4d17f-163">**Get-tjänster**</span><span class="sxs-lookup"><span data-stu-id="4d17f-163">**Get-Services**</span></span> |<span data-ttu-id="4d17f-164">Hämta en lista över tjänster som körs på datorn hello där hello skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="4d17f-164">Get a list of services running on hello machine where hello script executes.</span></span> |
| <span data-ttu-id="4d17f-165">**Get-Service**</span><span class="sxs-lookup"><span data-stu-id="4d17f-165">**Get-Service**</span></span> |<span data-ttu-id="4d17f-166">Med hello specifika namn som indata och få detaljerad information för en specifik tjänst (namn på tjänst, process-ID, tillstånd, etc.) på hello datorn där hello skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="4d17f-166">With hello specific service name as input, get detailed information for a specific service (service name, process ID, state, etc.) on hello machine where hello script executes.</span></span> |
| <span data-ttu-id="4d17f-167">**Get-HDIServices**</span><span class="sxs-lookup"><span data-stu-id="4d17f-167">**Get-HDIServices**</span></span> |<span data-ttu-id="4d17f-168">Hämta en lista över HDInsight-tjänster som körs på datorn hello där hello skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="4d17f-168">Get a list of HDInsight services running on hello computer where hello script executes.</span></span> |
| <span data-ttu-id="4d17f-169">**Get-HDIService**</span><span class="sxs-lookup"><span data-stu-id="4d17f-169">**Get-HDIService**</span></span> |<span data-ttu-id="4d17f-170">Med hello specifika HDInsight tjänstnamn som indata, får du detaljerad information för en specifik tjänst (namn på tjänst, process-ID, tillstånd, etc.) på hello datorn där hello skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="4d17f-170">With hello specific HDInsight service name as input, get detailed information for a specific service (service name, process ID, state, etc.) on hello machine where hello script executes.</span></span> |
| <span data-ttu-id="4d17f-171">**Get-ServicesRunning**</span><span class="sxs-lookup"><span data-stu-id="4d17f-171">**Get-ServicesRunning**</span></span> |<span data-ttu-id="4d17f-172">Hämta en lista över tjänster som körs på hello dator där hello skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="4d17f-172">Get a list of services that are running on hello computer where hello script executes.</span></span> |
| <span data-ttu-id="4d17f-173">**Get-ServiceRunning**</span><span class="sxs-lookup"><span data-stu-id="4d17f-173">**Get-ServiceRunning**</span></span> |<span data-ttu-id="4d17f-174">Kontrollera om en specifik tjänst (efter namn) körs på hello dator där hello skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="4d17f-174">Check if a specific service (by name) is running on hello computer where hello script executes.</span></span> |
| <span data-ttu-id="4d17f-175">**Get-HDIServicesRunning**</span><span class="sxs-lookup"><span data-stu-id="4d17f-175">**Get-HDIServicesRunning**</span></span> |<span data-ttu-id="4d17f-176">Hämta en lista över HDInsight-tjänster som körs på datorn hello där hello skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="4d17f-176">Get a list of HDInsight services running on hello computer where hello script executes.</span></span> |
| <span data-ttu-id="4d17f-177">**Get-HDIServiceRunning**</span><span class="sxs-lookup"><span data-stu-id="4d17f-177">**Get-HDIServiceRunning**</span></span> |<span data-ttu-id="4d17f-178">Kontrollera om en specifik HDInsight-tjänst (efter namn) körs på hello dator där hello skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="4d17f-178">Check if a specific HDInsight service (by name) is running on hello computer where hello script executes.</span></span> |
| <span data-ttu-id="4d17f-179">**Get-HDIHadoopVersion**</span><span class="sxs-lookup"><span data-stu-id="4d17f-179">**Get-HDIHadoopVersion**</span></span> |<span data-ttu-id="4d17f-180">Hämta hello Hadoop installeras på hello dator där hello skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="4d17f-180">Get hello version of Hadoop installed on hello computer where hello script executes.</span></span> |
| <span data-ttu-id="4d17f-181">**Testa IsHDIHeadNode**</span><span class="sxs-lookup"><span data-stu-id="4d17f-181">**Test-IsHDIHeadNode**</span></span> |<span data-ttu-id="4d17f-182">Kontrollera om en huvudnod hello datorn där hello skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="4d17f-182">Check if hello computer where hello script executes is a head node.</span></span> |
| <span data-ttu-id="4d17f-183">**Testa IsActiveHDIHeadNode**</span><span class="sxs-lookup"><span data-stu-id="4d17f-183">**Test-IsActiveHDIHeadNode**</span></span> |<span data-ttu-id="4d17f-184">Kontrollera om en aktiv huvudnod hello datorn där hello skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="4d17f-184">Check if hello computer where hello script executes is an active head node.</span></span> |
| <span data-ttu-id="4d17f-185">**Testa IsHDIDataNode**</span><span class="sxs-lookup"><span data-stu-id="4d17f-185">**Test-IsHDIDataNode**</span></span> |<span data-ttu-id="4d17f-186">Kontrollera att hello datorn där hello skriptet körs är en datanod.</span><span class="sxs-lookup"><span data-stu-id="4d17f-186">Check if hello computer where hello script executes is a data node.</span></span> |
| <span data-ttu-id="4d17f-187">**Redigera HDIConfigFile**</span><span class="sxs-lookup"><span data-stu-id="4d17f-187">**Edit-HDIConfigFile**</span></span> |<span data-ttu-id="4d17f-188">Redigera hello config filer hive-site.xml core-site.xml, hdfs-site.xml, mapred site.xml eller yarn-site.xml.</span><span class="sxs-lookup"><span data-stu-id="4d17f-188">Edit hello config files hive-site.xml, core-site.xml, hdfs-site.xml, mapred-site.xml, or yarn-site.xml.</span></span> |

## <a name="best-practices-for-script-development"></a><span data-ttu-id="4d17f-189">Metodtips för skriptutveckling av</span><span class="sxs-lookup"><span data-stu-id="4d17f-189">Best practices for script development</span></span>
<span data-ttu-id="4d17f-190">När du utvecklar ett anpassat skript för ett HDInsight-kluster finns flera bästa praxis tookeep i åtanke:</span><span class="sxs-lookup"><span data-stu-id="4d17f-190">When you develop a custom script for an HDInsight cluster, there are several best practices tookeep in mind:</span></span>

* <span data-ttu-id="4d17f-191">Sök efter hello Hadoop-version</span><span class="sxs-lookup"><span data-stu-id="4d17f-191">Check for hello Hadoop version</span></span>

    <span data-ttu-id="4d17f-192">Endast HDInsight version 3.1 (Hadoop 2.4) och senare stöd med skriptåtgärder tooinstall anpassade komponenter i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="4d17f-192">Only HDInsight version 3.1 (Hadoop 2.4) and above support using Script Action tooinstall custom components on a cluster.</span></span> <span data-ttu-id="4d17f-193">I ett skript, måste du använda hello **Get-HDIHadoopVersion** helper metod toocheck hello Hadoop version innan du fortsätter med att utföra andra uppgifter i hello skript.</span><span class="sxs-lookup"><span data-stu-id="4d17f-193">In your custom script, you must use hello **Get-HDIHadoopVersion** helper method toocheck hello Hadoop version before proceeding with performing other tasks in hello script.</span></span>
* <span data-ttu-id="4d17f-194">Ange stabil länkar tooscript resurser</span><span class="sxs-lookup"><span data-stu-id="4d17f-194">Provide stable links tooscript resources</span></span>

    <span data-ttu-id="4d17f-195">Användare bör se till att alla hello skript och andra artefakter som används i hello anpassning av ett kluster förblir tillgängliga i hela hello livstid hello klustret och att hello versioner av dessa filer inte ändrar hello tidsåtgång för.</span><span class="sxs-lookup"><span data-stu-id="4d17f-195">Users should make sure that all hello scripts and other artifacts used in hello customization of a cluster remain available throughout hello lifetime of hello cluster and that hello versions of these files do not change for hello duration.</span></span> <span data-ttu-id="4d17f-196">Dessa resurser krävs om hello återställning av avbildning av noderna i klustret hello krävs.</span><span class="sxs-lookup"><span data-stu-id="4d17f-196">These resources are required if hello reimaging of nodes in hello cluster is required.</span></span> <span data-ttu-id="4d17f-197">hello bästa praxis är toodownload och arkivera allt innehåll i ett lagringskonto som hello användarkontroller.</span><span class="sxs-lookup"><span data-stu-id="4d17f-197">hello best practice is toodownload and archive everything in a Storage account that hello user controls.</span></span> <span data-ttu-id="4d17f-198">Detta kan vara hello standardkontot för lagring eller någon av hello ytterligare lagringskonton på hello tidpunkten för distribution av anpassade kluster.</span><span class="sxs-lookup"><span data-stu-id="4d17f-198">This can be hello default Storage account or any of hello additional Storage accounts specified at hello time of deployment for a customized cluster.</span></span>
    <span data-ttu-id="4d17f-199">I hello Spark och R anpassade kluster exempel anges i dokumentationen för hello t.ex, vi har gjort en lokal kopia av hello resurser i det här lagringskontot: https://hdiconfigactions.blob.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="4d17f-199">In hello Spark and R customized cluster samples provided in hello documentation, for example, we have made a local copy of hello resources in this Storage account: https://hdiconfigactions.blob.core.windows.net/.</span></span>
* <span data-ttu-id="4d17f-200">Se till att hello kluster anpassning skriptet idempotent</span><span class="sxs-lookup"><span data-stu-id="4d17f-200">Ensure that hello cluster customization script is idempotent</span></span>

    <span data-ttu-id="4d17f-201">Förväntat att hello noder i ett HDInsight-kluster är avbildade under hello klustrets livslängd.</span><span class="sxs-lookup"><span data-stu-id="4d17f-201">You must expect that hello nodes of an HDInsight cluster is reimaged during hello cluster lifetime.</span></span> <span data-ttu-id="4d17f-202">hello klustret anpassning skript körs när ett kluster avbildade.</span><span class="sxs-lookup"><span data-stu-id="4d17f-202">hello cluster customization script is run whenever a cluster is reimaged.</span></span> <span data-ttu-id="4d17f-203">Det här skriptet måste vara utformad toobe idempotent i hello mening att hello skript vid återställning av avbildning, bör kontrollera hello klustret returneras toohello samma anpassade tillståndet den var i efter hello skriptet kördes för hello första gången när hello klustret har från början Skapa.</span><span class="sxs-lookup"><span data-stu-id="4d17f-203">This script must be designed toobe idempotent in hello sense that upon reimaging, hello script should ensure that hello cluster is returned toohello same customized state that it was in just after hello script ran for hello first time when hello cluster was initially created.</span></span> <span data-ttu-id="4d17f-204">Till exempel om ett anpassat skript för ett program på D:\AppLocation har installerats på först kör och sedan på varje efterföljande körning vid återställning av avbildning, hello skriptet ska kontrollera om hello program finns i hello D:\AppLocation plats innan du fortsätter med andra stegen i hello skript.</span><span class="sxs-lookup"><span data-stu-id="4d17f-204">For example, if a custom script installed an application at D:\AppLocation on its first run, then on each subsequent run, upon reimaging, hello script should check whether hello application exists at hello D:\AppLocation location before proceeding with other steps in hello script.</span></span>
* <span data-ttu-id="4d17f-205">Installera komponenter för anpassade hello optimala plats</span><span class="sxs-lookup"><span data-stu-id="4d17f-205">Install custom components in hello optimal location</span></span>

    <span data-ttu-id="4d17f-206">Om klusternoderna är avbildade kan hello C:\ resurs och D:\ systemenhet formateras om, vilket resulterar i hello förlust av data och program som har installerats på dessa enheter.</span><span class="sxs-lookup"><span data-stu-id="4d17f-206">When cluster nodes are reimaged, hello C:\ resource drive and D:\ system drive can be reformatted, resulting in hello loss of data and applications that had been installed on those drives.</span></span> <span data-ttu-id="4d17f-207">Detta kan också inträffa om en virtuell Azure-dator (VM)-nod som är en del av hello kluster kraschar och ersätts av en ny nod.</span><span class="sxs-lookup"><span data-stu-id="4d17f-207">This could also happen if an Azure virtual machine (VM) node that is part of hello cluster goes down and is replaced by a new node.</span></span> <span data-ttu-id="4d17f-208">Du kan installera komponenterna på hello D:\ enhet eller hello C:\apps plats på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="4d17f-208">You can install components on hello D:\ drive or in hello C:\apps location on hello cluster.</span></span> <span data-ttu-id="4d17f-209">Alla andra platser på hello enhet C:\ är reserverade.</span><span class="sxs-lookup"><span data-stu-id="4d17f-209">All other locations on hello C:\ drive are reserved.</span></span> <span data-ttu-id="4d17f-210">Ange hello platsen för program eller bibliotek toobe som installerats i hello kluster anpassning skriptet.</span><span class="sxs-lookup"><span data-stu-id="4d17f-210">Specify hello location where applications or libraries are toobe installed in hello cluster customization script.</span></span>
* <span data-ttu-id="4d17f-211">Garantera hög tillgänglighet för hello kluster-arkitektur</span><span class="sxs-lookup"><span data-stu-id="4d17f-211">Ensure high availability of hello cluster architecture</span></span>

    <span data-ttu-id="4d17f-212">HDInsight har ett aktivt-passivt arkitektur för hög tillgänglighet, i vilken en huvudnod är i aktivt läge (där hello HDInsight tjänster som körs) och hello andra huvudnod är i vänteläge (i vilken HDInsight tjänster inte körs).</span><span class="sxs-lookup"><span data-stu-id="4d17f-212">HDInsight has an active-passive architecture for high availability, in which one head node is in active mode (where hello HDInsight services are running) and hello other head node is in standby mode (in which HDInsight services are not running).</span></span> <span data-ttu-id="4d17f-213">hello noder växla mellan lägena aktiva och passiva om HDInsight tjänster avbryts.</span><span class="sxs-lookup"><span data-stu-id="4d17f-213">hello nodes switch active and passive modes if HDInsight services are interrupted.</span></span> <span data-ttu-id="4d17f-214">Om en skriptåtgärd är används tooinstall tjänster på båda huvudnoderna för hög tillgänglighet, Observera att hello HDInsight mekanism för växling vid fel inte kan tooautomatically misslyckas över dessa användare installerade tjänster.</span><span class="sxs-lookup"><span data-stu-id="4d17f-214">If a script action is used tooinstall services on both head nodes for high availability, note that hello HDInsight failover mechanism is not able tooautomatically fail over these user-installed services.</span></span> <span data-ttu-id="4d17f-215">Så användaren installerade tjänster på HDInsight huvudnoderna som förväntade toobe hög tillgänglighet måste antingen ha sina egna mekanism för växling vid fel om i aktivt-passivt läge eller vara i läget för aktiv-aktiv.</span><span class="sxs-lookup"><span data-stu-id="4d17f-215">So user-installed services on HDInsight head nodes that are expected toobe highly available must either have their own failover mechanism if in active-passive mode or be in active-active mode.</span></span>

    <span data-ttu-id="4d17f-216">Ett HDInsight-skriptåtgärder kommando som körs på båda huvudnoderna när hello huvudnod rollen har angetts som ett värde i hello *ClusterRoleCollection* parameter.</span><span class="sxs-lookup"><span data-stu-id="4d17f-216">An HDInsight Script Action command runs on both head nodes when hello head-node role is specified as a value in hello *ClusterRoleCollection* parameter.</span></span> <span data-ttu-id="4d17f-217">Så när du skapar ett anpassat skript, se till att skriptet är medveten om den här installationen.</span><span class="sxs-lookup"><span data-stu-id="4d17f-217">So when you design a custom script, make sure that your script is aware of this setup.</span></span> <span data-ttu-id="4d17f-218">Du bör inte köra problem där hello samma tjänster är installerade och igång på både hello huvudnoderna och hamnar konkurrerar med varandra.</span><span class="sxs-lookup"><span data-stu-id="4d17f-218">You should not run into problems where hello same services are installed and started on both of hello head nodes and they end up competing with each other.</span></span> <span data-ttu-id="4d17f-219">Tänk också på att data går förlorade vid återställning av avbildning, så att programvaran via skriptåtgärd har toobe flexibel toosuch händelser.</span><span class="sxs-lookup"><span data-stu-id="4d17f-219">Also, be aware that data is lost during reimaging, so software installed via Script Action has toobe resilient toosuch events.</span></span> <span data-ttu-id="4d17f-220">Program ska vara utformad toowork med hög tillgänglighet data som distribueras över flera noder.</span><span class="sxs-lookup"><span data-stu-id="4d17f-220">Applications should be designed toowork with highly available data that is distributed across many nodes.</span></span> <span data-ttu-id="4d17f-221">Observera att du kan avbildade upp till 1/5 hello noder i ett kluster på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="4d17f-221">Note that as many as 1/5 of hello nodes in a cluster can be reimaged at hello same time.</span></span>
* <span data-ttu-id="4d17f-222">Konfigurera hello anpassade komponenter toouse Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="4d17f-222">Configure hello custom components toouse Azure Blob storage</span></span>

    <span data-ttu-id="4d17f-223">hello anpassade komponenter som installeras på hello klusternoder kan ha en standard configuration toouse Hadoop Distributed File System (HDFS) lagring.</span><span class="sxs-lookup"><span data-stu-id="4d17f-223">hello custom components that you install on hello cluster nodes might have a default configuration toouse Hadoop Distributed File System (HDFS) storage.</span></span> <span data-ttu-id="4d17f-224">I stället bör du ändra hello configuration toouse Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="4d17f-224">You should change hello configuration toouse Azure Blob storage instead.</span></span> <span data-ttu-id="4d17f-225">På ett kluster avbildningsåterställning hello HDFS hämtar filsystemet och du skulle förlora alla data som lagras där.</span><span class="sxs-lookup"><span data-stu-id="4d17f-225">On a cluster reimage, hello HDFS file system gets formatted and you would lose any data that is stored there.</span></span> <span data-ttu-id="4d17f-226">Azure Blob storage i stället garanterar att dina data bevaras.</span><span class="sxs-lookup"><span data-stu-id="4d17f-226">Using Azure Blob storage instead ensures that your data is retained.</span></span>

## <a name="common-usage-patterns"></a><span data-ttu-id="4d17f-227">Vanliga användningsmönster</span><span class="sxs-lookup"><span data-stu-id="4d17f-227">Common usage patterns</span></span>
<span data-ttu-id="4d17f-228">Det här avsnittet ger vägledning om att implementera några hello vanliga användningsmönster som som kan uppstå vid skrivning till ett eget skript.</span><span class="sxs-lookup"><span data-stu-id="4d17f-228">This section provides guidance on implementing some of hello common usage patterns that you might run into while writing your own custom script.</span></span>

### <a name="configure-environment-variables"></a><span data-ttu-id="4d17f-229">Konfigurera miljövariabler</span><span class="sxs-lookup"><span data-stu-id="4d17f-229">Configure environment variables</span></span>
<span data-ttu-id="4d17f-230">I skriptutveckling du ofta hello måste tooset miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="4d17f-230">Often in script action development, you feel hello need tooset environment variables.</span></span> <span data-ttu-id="4d17f-231">Exempelvis är mest sannolika scenariot när du hämtar en binär från en extern plats, installera den på hello klustret och Lägg till hello plats där det är installerade tooyour 'PATH'-miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="4d17f-231">For instance, a most likely scenario is when you download a binary from an external site, install it on hello cluster, and add hello location of where it is installed tooyour ‘PATH’ environment variable.</span></span> <span data-ttu-id="4d17f-232">hello följande utdrag visar hur tooset miljövariabler i hello anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="4d17f-232">hello following snippet shows you how tooset environment variables in hello custom script.</span></span>

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

<span data-ttu-id="4d17f-233">Den här instruktionen anger hello miljövariabeln **MDS_RUNNER_CUSTOM_CLUSTER** toohello värdet 'true' och även hello omfånget för den här variabeln toobe datoromfattande.</span><span class="sxs-lookup"><span data-stu-id="4d17f-233">This statement sets hello environment variable **MDS_RUNNER_CUSTOM_CLUSTER** toohello value 'true' and also sets hello scope of this variable toobe machine-wide.</span></span> <span data-ttu-id="4d17f-234">Ibland är det viktigt att miljövariablerna anges vid hello lämplig omfattning – dator eller användare.</span><span class="sxs-lookup"><span data-stu-id="4d17f-234">At times it is important that environment variables are set at hello appropriate scope – machine or user.</span></span> <span data-ttu-id="4d17f-235">Se [här] [ 1] för mer information om hur du anger miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="4d17f-235">Refer [here][1] for more information on setting environment variables.</span></span>

### <a name="access-toolocations-where-hello-custom-scripts-are-stored"></a><span data-ttu-id="4d17f-236">Åtkomst till toolocations där hello anpassade skript lagras</span><span class="sxs-lookup"><span data-stu-id="4d17f-236">Access toolocations where hello custom scripts are stored</span></span>
<span data-ttu-id="4d17f-237">Skript som används toocustomize en tooeither för klustret måste finnas i hello standardkontot för lagring för hello kluster eller i en offentlig skrivskyddad behållare för andra storage-konto.</span><span class="sxs-lookup"><span data-stu-id="4d17f-237">Scripts used toocustomize a cluster needs tooeither be in hello default storage account for hello cluster or in a public read-only container on any other storage account.</span></span> <span data-ttu-id="4d17f-238">Om skriptet har åtkomst till resurser som finns någon annanstans dessa måste toobe i en offentligt tillgänglig (minst offentliga skrivskyddad).</span><span class="sxs-lookup"><span data-stu-id="4d17f-238">If your script accesses resources located elsewhere these need toobe in a publicly accessible (at least public read-only).</span></span> <span data-ttu-id="4d17f-239">Du kanske exempelvis vill tooaccess en fil och sparar den hello SaveFile HDI-kommandot.</span><span class="sxs-lookup"><span data-stu-id="4d17f-239">For instance you might want tooaccess a file and save it using hello SaveFile-HDI command.</span></span>

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

<span data-ttu-id="4d17f-240">I det här exemplet måste du kontrollera hello behållaren somecontainer om du i storage-konto 'somestorageaccount' är allmänt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="4d17f-240">In this example, you must ensure that hello container 'somecontainer' in storage account 'somestorageaccount' is publicly accessible.</span></span> <span data-ttu-id="4d17f-241">I annat fall utlöser ett undantag 'Gick inte att hitta' hello skript och misslyckas.</span><span class="sxs-lookup"><span data-stu-id="4d17f-241">Otherwise, hello script throws a ‘Not Found’ exception and fail.</span></span>

### <a name="pass-parameters-toohello-add-azurermhdinsightscriptaction-cmdlet"></a><span data-ttu-id="4d17f-242">Skicka parametrar toohello Lägg till AzureRmHDInsightScriptAction cmdlet</span><span class="sxs-lookup"><span data-stu-id="4d17f-242">Pass parameters toohello Add-AzureRmHDInsightScriptAction cmdlet</span></span>
<span data-ttu-id="4d17f-243">toopass flera parametrar toohello Lägg till AzureRmHDInsightScriptAction cmdlet, behöver du tooformat hello sträng värdet toocontain alla parametrar för hello skript.</span><span class="sxs-lookup"><span data-stu-id="4d17f-243">toopass multiple parameters toohello Add-AzureRmHDInsightScriptAction cmdlet, you need tooformat hello string value toocontain all parameters for hello script.</span></span> <span data-ttu-id="4d17f-244">Exempel:</span><span class="sxs-lookup"><span data-stu-id="4d17f-244">For example:</span></span>

    "-CertifcateUri wasb:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

<span data-ttu-id="4d17f-245">eller</span><span class="sxs-lookup"><span data-stu-id="4d17f-245">or</span></span>

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a><span data-ttu-id="4d17f-246">Utlös undantag för misslyckade Klusterdistribution</span><span class="sxs-lookup"><span data-stu-id="4d17f-246">Throw exception for failed cluster deployment</span></span>
<span data-ttu-id="4d17f-247">Om du vill tooget korrekt meddelande om hello faktum att klustret anpassning slutfördes inte som förväntat, det är viktigt toothrow ett undantag och misslyckas hello klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="4d17f-247">If you want tooget accurately notified of hello fact that cluster customization did not succeed as expected, it is important toothrow an exception and fail hello cluster creation.</span></span> <span data-ttu-id="4d17f-248">Du kanske exempelvis vill tooprocess en fil om den finns och hantera hello fel fall där hello-filen inte finns.</span><span class="sxs-lookup"><span data-stu-id="4d17f-248">For instance, you might want tooprocess a file if it exists and handle hello error case where hello file does not exist.</span></span> <span data-ttu-id="4d17f-249">Detta skulle se till att hello skriptet avslutas utan problem och hello tillståndet för hello kluster kallas korrekt.</span><span class="sxs-lookup"><span data-stu-id="4d17f-249">This would ensure that hello script exits gracefully and hello state of hello cluster is correctly known.</span></span> <span data-ttu-id="4d17f-250">hello följande kodavsnitt ger ett exempel på hur tooachieve detta:</span><span class="sxs-lookup"><span data-stu-id="4d17f-250">hello following snippet gives an example of how tooachieve this:</span></span>

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

<span data-ttu-id="4d17f-251">I det här kodstycket om hello-filen inte finns, skulle det leda tooa tillstånd där hello skriptet faktiskt avslutas korrekt efter utskrift hello felmeddelande och hello klustret når körtillstånd under förutsättning att den ”” slutförts anpassningsprocess av klustret.</span><span class="sxs-lookup"><span data-stu-id="4d17f-251">In this snippet, if hello file did not exist, it would lead tooa state where hello script actually exits gracefully after printing hello error message, and hello cluster reaches running state assuming it "successfully" completed cluster customization process.</span></span> <span data-ttu-id="4d17f-252">Om du vill toobe korrekt meddelande om hello faktum att klustret anpassning i stort sett slutfördes inte som förväntat på grund av en fil som saknas, det är mer lämpliga toothrow ett undantag och misslyckas hello klustret anpassning steg.</span><span class="sxs-lookup"><span data-stu-id="4d17f-252">If you want toobe accurately notified of hello fact that cluster customization essentially did not succeed as expected because of a missing file, it is more appropriate toothrow an exception and fail hello cluster customization step.</span></span> <span data-ttu-id="4d17f-253">tooachieve detta måste du använda hello följande exempel kodstycke i stället.</span><span class="sxs-lookup"><span data-stu-id="4d17f-253">tooachieve this you must use hello following sample code snippet instead.</span></span>

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a><span data-ttu-id="4d17f-254">Checklista för distribution av en skriptåtgärd</span><span class="sxs-lookup"><span data-stu-id="4d17f-254">Checklist for deploying a script action</span></span>
<span data-ttu-id="4d17f-255">Här är hello steg som vi har tagit när du förbereder toodeploy dessa skript:</span><span class="sxs-lookup"><span data-stu-id="4d17f-255">Here are hello steps we took when preparing toodeploy these scripts:</span></span>

1. <span data-ttu-id="4d17f-256">Placera hello-filer som innehåller hello anpassade skript på en plats som kan nås av hello klusternoder under distributionen.</span><span class="sxs-lookup"><span data-stu-id="4d17f-256">Put hello files that contain hello custom scripts in a place that is accessible by hello cluster nodes during deployment.</span></span> <span data-ttu-id="4d17f-257">Detta kan vara något av hello standard eller ytterligare lagringskonton som anges när hello Klusterdistribution och andra offentligt tillgänglig lagringsbehållaren.</span><span class="sxs-lookup"><span data-stu-id="4d17f-257">This can be any of hello default or additional Storage accounts specified at hello time of cluster deployment, or any other publicly accessible storage container.</span></span>
2. <span data-ttu-id="4d17f-258">Lägga till kontroller i skript toomake till att de kör idempotently, så att hello skript kan köras flera gånger på hello samma nod.</span><span class="sxs-lookup"><span data-stu-id="4d17f-258">Add checks into scripts toomake sure that they execute idempotently, so that hello script can be executed multiple times on hello same node.</span></span>
3. <span data-ttu-id="4d17f-259">Använd hello **Write-Output** Azure PowerShell-cmdlet tooprint tooSTDOUT samt STDERR.</span><span class="sxs-lookup"><span data-stu-id="4d17f-259">Use hello **Write-Output** Azure PowerShell cmdlet tooprint tooSTDOUT as well as STDERR.</span></span> <span data-ttu-id="4d17f-260">Använd inte **Write-Host**.</span><span class="sxs-lookup"><span data-stu-id="4d17f-260">Do not use **Write-Host**.</span></span>
4. <span data-ttu-id="4d17f-261">Använda en tillfällig mapp, till exempel $env: TEMP tookeep hello hämtade filen som används av hello skript och sedan rensa dem efter skript har körts.</span><span class="sxs-lookup"><span data-stu-id="4d17f-261">Use a temporary file folder, such as $env:TEMP, tookeep hello downloaded file used by hello scripts and then clean them up after scripts have executed.</span></span>
5. <span data-ttu-id="4d17f-262">Installera anpassade programvara på D:\ eller C:\apps.</span><span class="sxs-lookup"><span data-stu-id="4d17f-262">Install custom software only at D:\ or C:\apps.</span></span> <span data-ttu-id="4d17f-263">Andra platser på hello enhet C: ska inte användas som de är reserverade.</span><span class="sxs-lookup"><span data-stu-id="4d17f-263">Other locations on hello C: drive should not be used as they are reserved.</span></span> <span data-ttu-id="4d17f-264">Observera att installera filer på hello C: enhet utanför hello C:\apps mappen kan resultera i installationsfel under reimages för hello-nod.</span><span class="sxs-lookup"><span data-stu-id="4d17f-264">Note that installing files on hello C: drive outside of hello C:\apps folder may result in setup failures during reimages of hello node.</span></span>
6. <span data-ttu-id="4d17f-265">Du kanske vill toorestart HDInsight services så att de kan hämta OS-nivå inställningar, till exempel hello miljövariabler som anges i hello skript i hello händelse OS inställningar eller Hadoop service configuration-filer har ändrats.</span><span class="sxs-lookup"><span data-stu-id="4d17f-265">In hello event that OS-level settings or Hadoop service configuration files were changed, you may want toorestart HDInsight services so that they can pick up any OS-level settings, such as hello environment variables set in hello scripts.</span></span>

## <a name="debug-custom-scripts"></a><span data-ttu-id="4d17f-266">Felsöka anpassade skript</span><span class="sxs-lookup"><span data-stu-id="4d17f-266">Debug custom scripts</span></span>
<span data-ttu-id="4d17f-267">hello skript felloggar lagras tillsammans med andra utdata i hello standardkontot för lagring som du angett för hello klustret när skapandet.</span><span class="sxs-lookup"><span data-stu-id="4d17f-267">hello script error logs are stored, along with other output, in hello default Storage account that you specified for hello cluster at its creation.</span></span> <span data-ttu-id="4d17f-268">hello loggfilerna lagras i en tabell med namnet hello *u < \cluster-name-fragment >< \time-stamp > setuplog*.</span><span class="sxs-lookup"><span data-stu-id="4d17f-268">hello logs are stored in a table with hello name *u<\cluster-name-fragment><\time-stamp>setuplog*.</span></span> <span data-ttu-id="4d17f-269">Det här är sammanställda loggar som har poster från alla hello noder (huvudnod och arbetarnoder) på vilken hello skriptet körs i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="4d17f-269">These are aggregated logs that have records from all of hello nodes (head node and worker nodes) on which hello script runs in hello cluster.</span></span>
<span data-ttu-id="4d17f-270">Ett enkelt sätt toocheck hello loggar är toouse HDInsight Tools för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4d17f-270">An easy way toocheck hello logs is toouse HDInsight Tools for Visual Studio.</span></span> <span data-ttu-id="4d17f-271">Installera hello-verktyg finns [komma igång med Visual Studio Hadoop-verktyg för HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)</span><span class="sxs-lookup"><span data-stu-id="4d17f-271">For installing hello tools, see [Get started using Visual Studio Hadoop tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)</span></span>

<span data-ttu-id="4d17f-272">**toocheck hello logg med Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="4d17f-272">**toocheck hello log using Visual Studio**</span></span>

1. <span data-ttu-id="4d17f-273">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4d17f-273">Open Visual Studio.</span></span>
2. <span data-ttu-id="4d17f-274">Klicka på **visa**, och klicka sedan på **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="4d17f-274">Click **View**, and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="4d17f-275">Högerklicka på ”Azure”, klicka på Anslut för**Microsoft Azure-prenumerationer**, och ange dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="4d17f-275">Right-click "Azure", click Connect too**Microsoft Azure Subscriptions**, and then enter your credentials.</span></span>
4. <span data-ttu-id="4d17f-276">Expandera **lagring**, expandera hello Azure storage-konto som används som hello standardfilsystem, expandera **tabeller**, och dubbelklicka sedan på hello tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="4d17f-276">Expand **Storage**, expand hello Azure storage account used as hello default file system, expand **Tables**, and then double-click hello table name.</span></span>

<span data-ttu-id="4d17f-277">Du kan också fjärråtkomst till hello klustret noder toosee STDOUT- och STDERR för anpassade skript.</span><span class="sxs-lookup"><span data-stu-id="4d17f-277">You can also remote into hello cluster nodes toosee both STDOUT and STDERR for custom scripts.</span></span> <span data-ttu-id="4d17f-278">hello loggar på varje nod är särskilda endast toothat noder och loggas i **C:\HDInsightLogs\DeploymentAgent.log**.</span><span class="sxs-lookup"><span data-stu-id="4d17f-278">hello logs on each node are specific only toothat node and are logged into **C:\HDInsightLogs\DeploymentAgent.log**.</span></span> <span data-ttu-id="4d17f-279">Dessa loggfilerna registrerar alla utdata från hello anpassade skript.</span><span class="sxs-lookup"><span data-stu-id="4d17f-279">These log files record all outputs from hello custom script.</span></span> <span data-ttu-id="4d17f-280">Ett exempel loggen kodstycke för en Spark skriptåtgärd ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="4d17f-280">An example log snippet for a Spark script action looks like this:</span></span>

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


<span data-ttu-id="4d17f-281">Det är tydligt att hello Spark skriptåtgärd har körts på hello virtuella datorn med namnet HEADNODE0 och att inga undantag utlöstes under körning av hello i den här loggen.</span><span class="sxs-lookup"><span data-stu-id="4d17f-281">In this log, it is clear that hello Spark script action has been executed on hello VM named HEADNODE0 and that no exceptions were thrown during hello execution.</span></span>

<span data-ttu-id="4d17f-282">I hello händelse som ett körningsfel inträffar, finns hello utdata som beskriver det också i loggfilen.</span><span class="sxs-lookup"><span data-stu-id="4d17f-282">In hello event that an execution failure occurs, hello output describing it is also contained in this log file.</span></span> <span data-ttu-id="4d17f-283">hello informationen i dessa loggar är användbar vid felsökning av problem med skript som kan uppstå.</span><span class="sxs-lookup"><span data-stu-id="4d17f-283">hello information provided in these logs should be helpful in debugging script problems that may arise.</span></span>

## <a name="see-also"></a><span data-ttu-id="4d17f-284">Se även</span><span class="sxs-lookup"><span data-stu-id="4d17f-284">See also</span></span>
* <span data-ttu-id="4d17f-285">[Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]</span><span class="sxs-lookup"><span data-stu-id="4d17f-285">[Customize HDInsight clusters using Script Action][hdinsight-cluster-customize]</span></span>
* <span data-ttu-id="4d17f-286">[Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="4d17f-286">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="4d17f-287">[Installera och använda R i HDInsight-kluster][hdinsight-r-scripts]</span><span class="sxs-lookup"><span data-stu-id="4d17f-287">[Install and use R on HDInsight clusters][hdinsight-r-scripts]</span></span>
* <span data-ttu-id="4d17f-288">[Installera och använda Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="4d17f-288">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="4d17f-289">[Installera och använda Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="4d17f-289">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
