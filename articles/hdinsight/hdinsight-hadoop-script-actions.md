---
title: "Utveckling av skriptåtgärder med HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur du anpassar Hadoop-kluster med skriptåtgärder. Skriptåtgärder kan användas för att installera ytterligare programvara som körs på ett Hadoop-kluster eller ändra konfigurationen av program på ett kluster."
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
ms.openlocfilehash: 0e182e6b43fd2d17524c1da36cf4c204bb1b865a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a><span data-ttu-id="264fe-104">Utveckla skriptåtgärd skript för HDInsight Windows-baserade kluster</span><span class="sxs-lookup"><span data-stu-id="264fe-104">Develop Script Action scripts for HDInsight Windows-based clusters</span></span>
<span data-ttu-id="264fe-105">Lär dig hur du skriver skript för skriptåtgärder för HDInsight.</span><span class="sxs-lookup"><span data-stu-id="264fe-105">Learn how to write Script Action scripts for HDInsight.</span></span> <span data-ttu-id="264fe-106">Information om hur du använder skriptåtgärd skript finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="264fe-106">For information on using Script Action scripts, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md).</span></span> <span data-ttu-id="264fe-107">Samma artikel skrivna för Linux-baserade HDInsight-kluster, se [utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="264fe-107">For the same article written for Linux-based HDInsight clusters, see [Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions-linux.md).</span></span>



> [!IMPORTANT]
> <span data-ttu-id="264fe-108">Stegen i det här dokumentet fungerar endast för Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="264fe-108">The steps in this document only work for Windows-based HDInsight clusters.</span></span> <span data-ttu-id="264fe-109">HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="264fe-109">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="264fe-110">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="264fe-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="264fe-111">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="264fe-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="264fe-112">Information om användning av skriptåtgärder med Linux-baserade kluster finns i [utveckling av skriptåtgärder med HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="264fe-112">For information on using script actions with Linux-based clusters, see [Script action development with HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).</span></span>
>
>



<span data-ttu-id="264fe-113">Skriptåtgärder kan användas för att installera ytterligare programvara som körs på ett Hadoop-kluster eller ändra konfigurationen av program på ett kluster.</span><span class="sxs-lookup"><span data-stu-id="264fe-113">Script Action can be used to install additional software running on a Hadoop cluster or to change the configuration of applications installed on a cluster.</span></span> <span data-ttu-id="264fe-114">Skriptåtgärder avses skript som körs på klusternoderna när HDInsight-kluster distribueras och de utförs när noder i klustret Slutför HDInsight-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="264fe-114">Script actions are scripts that run on the cluster nodes when HDInsight clusters are deployed, and they are executed once nodes in the cluster complete HDInsight configuration.</span></span> <span data-ttu-id="264fe-115">En skriptåtgärd körs under kontot systemadministratörsprivilegier och ger fullständig behörighet till klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="264fe-115">A script action is executed under system admin account privileges and provides full access rights to the cluster nodes.</span></span> <span data-ttu-id="264fe-116">Varje kluster kan anges med en lista över skriptåtgärder ska utföras i den ordning som de har angetts.</span><span class="sxs-lookup"><span data-stu-id="264fe-116">Each cluster can be provided with a list of script actions to be executed in the order in which they are specified.</span></span>

> [!NOTE]
> <span data-ttu-id="264fe-117">Om du får följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="264fe-117">If you experience the following error message:</span></span>
>
> <span data-ttu-id="264fe-118">System.Management.Automation.CommandNotFoundException; ExceptionMessage: Termen ”spara HDIFile' identifieras inte som namnet på en cmdlet, funktion, skriptfilen eller körbart program.</span><span class="sxs-lookup"><span data-stu-id="264fe-118">System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program.</span></span> <span data-ttu-id="264fe-119">Kontrollera stavningen av namnet, eller om en sökväg ingick, kontrollera att sökvägen är korrekt och försök igen.</span><span class="sxs-lookup"><span data-stu-id="264fe-119">Check the spelling of the name, or if a path was included, verify that the path is correct and try again.</span></span>
> <span data-ttu-id="264fe-120">Det beror på att du inte inkludera helper-metoder.</span><span class="sxs-lookup"><span data-stu-id="264fe-120">It is because you didn't include the helper methods.</span></span>  <span data-ttu-id="264fe-121">Se [hjälpmetoder för anpassade skript](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).</span><span class="sxs-lookup"><span data-stu-id="264fe-121">See [Helper methods for custom scripts](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).</span></span>
>
>

## <a name="sample-scripts"></a><span data-ttu-id="264fe-122">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="264fe-122">Sample scripts</span></span>
<span data-ttu-id="264fe-123">För att skapa HDInsight-kluster på Windows operativsystem, är den skriptåtgärd Azure PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="264fe-123">For creating HDInsight clusters on Windows operating system, the Script Action is Azure PowerShell script.</span></span> <span data-ttu-id="264fe-124">Följande skript är ett exempel för att konfigurera platsen konfigurationsfiler:</span><span class="sxs-lookup"><span data-stu-id="264fe-124">The following script is a sample for configuring the site configuration files:</span></span>

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
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
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

<span data-ttu-id="264fe-125">Skriptet använder fyra parametrar, namnet på konfigurationsfilen, egenskapen du vill ändra värdet som du vill ange, och en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="264fe-125">The script takes four parameters, the configuration file name, the property you want to modify, the value you want to set, and a description.</span></span> <span data-ttu-id="264fe-126">Exempel:</span><span class="sxs-lookup"><span data-stu-id="264fe-126">For example:</span></span>

    hive-site.xml hive.metastore.client.socket.timeout 90

<span data-ttu-id="264fe-127">Dessa parametrar anger värdet hive.metastore.client.socket.timeout 90 i filen hive-site.XML.</span><span class="sxs-lookup"><span data-stu-id="264fe-127">These parameters sets the hive.metastore.client.socket.timeout value to 90 in the hive-site.xml file.</span></span>  <span data-ttu-id="264fe-128">Standardvärdet är 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="264fe-128">The default value is 60 seconds.</span></span>

<span data-ttu-id="264fe-129">Det här exempelskriptet finns även på [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).</span><span class="sxs-lookup"><span data-stu-id="264fe-129">This sample script can also be found at [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).</span></span>

<span data-ttu-id="264fe-130">HDInsight tillhandahåller flera skript för att installera ytterligare komponenter i HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="264fe-130">HDInsight provides several scripts to install additional components on HDInsight clusters:</span></span>

| <span data-ttu-id="264fe-131">Namn</span><span class="sxs-lookup"><span data-stu-id="264fe-131">Name</span></span> | <span data-ttu-id="264fe-132">Skript</span><span class="sxs-lookup"><span data-stu-id="264fe-132">Script</span></span> |
| --- | --- |
| <span data-ttu-id="264fe-133">**Installera Spark**</span><span class="sxs-lookup"><span data-stu-id="264fe-133">**Install Spark**</span></span> |<span data-ttu-id="264fe-134">https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1.</span><span class="sxs-lookup"><span data-stu-id="264fe-134">https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1.</span></span> <span data-ttu-id="264fe-135">Se [installera och använda Spark i HDInsight-kluster][hdinsight-install-spark].</span><span class="sxs-lookup"><span data-stu-id="264fe-135">See [Install and use Spark on HDInsight clusters][hdinsight-install-spark].</span></span> |
| <span data-ttu-id="264fe-136">**Installera R**</span><span class="sxs-lookup"><span data-stu-id="264fe-136">**Install R**</span></span> |<span data-ttu-id="264fe-137">https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1.</span><span class="sxs-lookup"><span data-stu-id="264fe-137">https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1.</span></span> <span data-ttu-id="264fe-138">Se [installera och använda R i HDInsight-kluster][hdinsight-r-scripts].</span><span class="sxs-lookup"><span data-stu-id="264fe-138">See [Install and use R on HDInsight clusters][hdinsight-r-scripts].</span></span> |
| <span data-ttu-id="264fe-139">**Installera Solr**</span><span class="sxs-lookup"><span data-stu-id="264fe-139">**Install Solr**</span></span> |<span data-ttu-id="264fe-140">https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="264fe-140">https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1.</span></span> <span data-ttu-id="264fe-141">Se [installerar och använder Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="264fe-141">See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span> |
| <span data-ttu-id="264fe-142">- **Installera Giraph**</span><span class="sxs-lookup"><span data-stu-id="264fe-142">- **Install Giraph**</span></span> |<span data-ttu-id="264fe-143">https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="264fe-143">https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1.</span></span> <span data-ttu-id="264fe-144">Se [installerar och använder Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="264fe-144">See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span> |

<span data-ttu-id="264fe-145">Skriptåtgärder kan distribueras från Azure-portalen, Azure PowerShell eller med hjälp av HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="264fe-145">Script Action can be deployed from the Azure portal, Azure PowerShell or by using the HDInsight .NET SDK.</span></span>  <span data-ttu-id="264fe-146">Mer information finns i [anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize].</span><span class="sxs-lookup"><span data-stu-id="264fe-146">For more information, see [Customize HDInsight clusters using Script Action][hdinsight-cluster-customize].</span></span>

> [!NOTE]
> <span data-ttu-id="264fe-147">Exempel på skript fungerar bara med HDInsight-kluster av version 3.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="264fe-147">The sample scripts work only with HDInsight cluster version 3.1 or above.</span></span> <span data-ttu-id="264fe-148">Mer information om HDInsight-kluster-versioner finns [HDInsight-kluster-versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="264fe-148">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>
>
>

## <a name="helper-methods-for-custom-scripts"></a><span data-ttu-id="264fe-149">Hjälpmetoder för anpassade skript</span><span class="sxs-lookup"><span data-stu-id="264fe-149">Helper methods for custom scripts</span></span>
<span data-ttu-id="264fe-150">Skriptet åtgärd helper metoder är verktyg som du kan använda vid skrivning till anpassade skript.</span><span class="sxs-lookup"><span data-stu-id="264fe-150">Script Action helper methods are utilities that you can use while writing custom scripts.</span></span> <span data-ttu-id="264fe-151">Dessa metoder är definierade i [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1), och kan ingå i ditt skript med hjälp av följande exempel:</span><span class="sxs-lookup"><span data-stu-id="264fe-151">These methods are defined in [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1), and can be included in your scripts using the following sample:</span></span>

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

<span data-ttu-id="264fe-152">Här följer hjälpmetoder som tillhandahålls av skriptet:</span><span class="sxs-lookup"><span data-stu-id="264fe-152">Here are the helper methods that are provided by this script:</span></span>

| <span data-ttu-id="264fe-153">Hjälpmetod</span><span class="sxs-lookup"><span data-stu-id="264fe-153">Helper method</span></span> | <span data-ttu-id="264fe-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="264fe-154">Description</span></span> |
| --- | --- |
| <span data-ttu-id="264fe-155">**Spara HDIFile**</span><span class="sxs-lookup"><span data-stu-id="264fe-155">**Save-HDIFile**</span></span> |<span data-ttu-id="264fe-156">Hämta en fil från den angivna identifieraren URI (Uniform Resource) till en plats på den lokala disken som associeras med den Virtuella Azure-noden som är tilldelade till klustret.</span><span class="sxs-lookup"><span data-stu-id="264fe-156">Download a file from the specified Uniform Resource Identifier (URI) to a location on the local disk that is associated with the Azure VM node assigned to the cluster.</span></span> |
| <span data-ttu-id="264fe-157">**Expandera HDIZippedFile**</span><span class="sxs-lookup"><span data-stu-id="264fe-157">**Expand-HDIZippedFile**</span></span> |<span data-ttu-id="264fe-158">Packa upp ZIP.</span><span class="sxs-lookup"><span data-stu-id="264fe-158">Unzip a zipped file.</span></span> |
| <span data-ttu-id="264fe-159">**Anropa HDICmdScript**</span><span class="sxs-lookup"><span data-stu-id="264fe-159">**Invoke-HDICmdScript**</span></span> |<span data-ttu-id="264fe-160">Köra ett skript från cmd.exe.</span><span class="sxs-lookup"><span data-stu-id="264fe-160">Run a script from cmd.exe.</span></span> |
| <span data-ttu-id="264fe-161">**Skriv HDILog**</span><span class="sxs-lookup"><span data-stu-id="264fe-161">**Write-HDILog**</span></span> |<span data-ttu-id="264fe-162">Skriva utdata från skriptet som används för en skriptåtgärd.</span><span class="sxs-lookup"><span data-stu-id="264fe-162">Write output from the custom script used for a script action.</span></span> |
| <span data-ttu-id="264fe-163">**Get-tjänster**</span><span class="sxs-lookup"><span data-stu-id="264fe-163">**Get-Services**</span></span> |<span data-ttu-id="264fe-164">Hämta en lista över tjänster som körs på datorn där skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="264fe-164">Get a list of services running on the machine where the script executes.</span></span> |
| <span data-ttu-id="264fe-165">**Get-Service**</span><span class="sxs-lookup"><span data-stu-id="264fe-165">**Get-Service**</span></span> |<span data-ttu-id="264fe-166">Med specifika tjänstnamnet som indata, får du detaljerad information för en specifik tjänst (namn på tjänst, process-ID, tillstånd, etc.) på datorn där skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="264fe-166">With the specific service name as input, get detailed information for a specific service (service name, process ID, state, etc.) on the machine where the script executes.</span></span> |
| <span data-ttu-id="264fe-167">**Get-HDIServices**</span><span class="sxs-lookup"><span data-stu-id="264fe-167">**Get-HDIServices**</span></span> |<span data-ttu-id="264fe-168">Hämta en lista över HDInsight-tjänster som körs på datorn där skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="264fe-168">Get a list of HDInsight services running on the computer where the script executes.</span></span> |
| <span data-ttu-id="264fe-169">**Get-HDIService**</span><span class="sxs-lookup"><span data-stu-id="264fe-169">**Get-HDIService**</span></span> |<span data-ttu-id="264fe-170">Med specifika HDInsight tjänstnamnet som indata, får du detaljerad information för en specifik tjänst (namn på tjänst, process-ID, tillstånd, etc.) på datorn där skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="264fe-170">With the specific HDInsight service name as input, get detailed information for a specific service (service name, process ID, state, etc.) on the machine where the script executes.</span></span> |
| <span data-ttu-id="264fe-171">**Get-ServicesRunning**</span><span class="sxs-lookup"><span data-stu-id="264fe-171">**Get-ServicesRunning**</span></span> |<span data-ttu-id="264fe-172">Hämta en lista över tjänster som körs på datorn där skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="264fe-172">Get a list of services that are running on the computer where the script executes.</span></span> |
| <span data-ttu-id="264fe-173">**Get-ServiceRunning**</span><span class="sxs-lookup"><span data-stu-id="264fe-173">**Get-ServiceRunning**</span></span> |<span data-ttu-id="264fe-174">Kontrollera om en specifik tjänst (efter namn) körs på datorn där skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="264fe-174">Check if a specific service (by name) is running on the computer where the script executes.</span></span> |
| <span data-ttu-id="264fe-175">**Get-HDIServicesRunning**</span><span class="sxs-lookup"><span data-stu-id="264fe-175">**Get-HDIServicesRunning**</span></span> |<span data-ttu-id="264fe-176">Hämta en lista över HDInsight-tjänster som körs på datorn där skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="264fe-176">Get a list of HDInsight services running on the computer where the script executes.</span></span> |
| <span data-ttu-id="264fe-177">**Get-HDIServiceRunning**</span><span class="sxs-lookup"><span data-stu-id="264fe-177">**Get-HDIServiceRunning**</span></span> |<span data-ttu-id="264fe-178">Kontrollera om en specifik HDInsight-tjänst (efter namn) körs på datorn där skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="264fe-178">Check if a specific HDInsight service (by name) is running on the computer where the script executes.</span></span> |
| <span data-ttu-id="264fe-179">**Get-HDIHadoopVersion**</span><span class="sxs-lookup"><span data-stu-id="264fe-179">**Get-HDIHadoopVersion**</span></span> |<span data-ttu-id="264fe-180">Hämta versionen av Hadoop som är installerad på datorn där skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="264fe-180">Get the version of Hadoop installed on the computer where the script executes.</span></span> |
| <span data-ttu-id="264fe-181">**Testa IsHDIHeadNode**</span><span class="sxs-lookup"><span data-stu-id="264fe-181">**Test-IsHDIHeadNode**</span></span> |<span data-ttu-id="264fe-182">Kontrollera om den dator där skriptet körs är en huvudnod.</span><span class="sxs-lookup"><span data-stu-id="264fe-182">Check if the computer where the script executes is a head node.</span></span> |
| <span data-ttu-id="264fe-183">**Testa IsActiveHDIHeadNode**</span><span class="sxs-lookup"><span data-stu-id="264fe-183">**Test-IsActiveHDIHeadNode**</span></span> |<span data-ttu-id="264fe-184">Kontrollera om den dator där skriptet körs är en aktiv huvudnod.</span><span class="sxs-lookup"><span data-stu-id="264fe-184">Check if the computer where the script executes is an active head node.</span></span> |
| <span data-ttu-id="264fe-185">**Testa IsHDIDataNode**</span><span class="sxs-lookup"><span data-stu-id="264fe-185">**Test-IsHDIDataNode**</span></span> |<span data-ttu-id="264fe-186">Kontrollera om den dator där skriptet körs är en datanod.</span><span class="sxs-lookup"><span data-stu-id="264fe-186">Check if the computer where the script executes is a data node.</span></span> |
| <span data-ttu-id="264fe-187">**Redigera HDIConfigFile**</span><span class="sxs-lookup"><span data-stu-id="264fe-187">**Edit-HDIConfigFile**</span></span> |<span data-ttu-id="264fe-188">Redigera config filer hive-site.xml, core-site.xml, hdfs-site.xml, mapred site.xml eller yarn-site.xml.</span><span class="sxs-lookup"><span data-stu-id="264fe-188">Edit the config files hive-site.xml, core-site.xml, hdfs-site.xml, mapred-site.xml, or yarn-site.xml.</span></span> |

## <a name="best-practices-for-script-development"></a><span data-ttu-id="264fe-189">Metodtips för skriptutveckling av</span><span class="sxs-lookup"><span data-stu-id="264fe-189">Best practices for script development</span></span>
<span data-ttu-id="264fe-190">Finns flera bästa praxis att tänka på när du utvecklar ett anpassat skript för ett HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="264fe-190">When you develop a custom script for an HDInsight cluster, there are several best practices to keep in mind:</span></span>

* <span data-ttu-id="264fe-191">Sök efter Hadoop-version</span><span class="sxs-lookup"><span data-stu-id="264fe-191">Check for the Hadoop version</span></span>

    <span data-ttu-id="264fe-192">Endast HDInsight version 3.1 (Hadoop 2.4) och högre support med skriptåtgärder installera anpassade komponenter på ett kluster.</span><span class="sxs-lookup"><span data-stu-id="264fe-192">Only HDInsight version 3.1 (Hadoop 2.4) and above support using Script Action to install custom components on a cluster.</span></span> <span data-ttu-id="264fe-193">I ett skript, måste du använda den **Get-HDIHadoopVersion** hjälpmetod Kontrollera Hadoop-versionen innan du fortsätter med att utföra andra uppgifter i skriptet.</span><span class="sxs-lookup"><span data-stu-id="264fe-193">In your custom script, you must use the **Get-HDIHadoopVersion** helper method to check the Hadoop version before proceeding with performing other tasks in the script.</span></span>
* <span data-ttu-id="264fe-194">Ange stabil länkar till skriptresurser</span><span class="sxs-lookup"><span data-stu-id="264fe-194">Provide stable links to script resources</span></span>

    <span data-ttu-id="264fe-195">Användare bör se till att alla skript och andra artefakter som används i anpassning av ett kluster förblir tillgängliga under hela livslängden för klustret och att versionerna av dessa filer inte ändrar under.</span><span class="sxs-lookup"><span data-stu-id="264fe-195">Users should make sure that all the scripts and other artifacts used in the customization of a cluster remain available throughout the lifetime of the cluster and that the versions of these files do not change for the duration.</span></span> <span data-ttu-id="264fe-196">Dessa resurser krävs om de återställning av avbildning av noder i klustret måste anges.</span><span class="sxs-lookup"><span data-stu-id="264fe-196">These resources are required if the reimaging of nodes in the cluster is required.</span></span> <span data-ttu-id="264fe-197">Det bästa sättet är att hämta och arkivera allt innehåll i ett lagringskonto som användaren anger.</span><span class="sxs-lookup"><span data-stu-id="264fe-197">The best practice is to download and archive everything in a Storage account that the user controls.</span></span> <span data-ttu-id="264fe-198">Detta kan vara standardkontot för lagring eller någon av de ytterligare lagringskonton som anges vid tidpunkten för distribution av anpassade kluster.</span><span class="sxs-lookup"><span data-stu-id="264fe-198">This can be the default Storage account or any of the additional Storage accounts specified at the time of deployment for a customized cluster.</span></span>
    <span data-ttu-id="264fe-199">Anpassade kluster prover i Spark och R anges i dokumentationen till exempel vi har gjort en lokal kopia av resurser i det här lagringskontot: https://hdiconfigactions.blob.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="264fe-199">In the Spark and R customized cluster samples provided in the documentation, for example, we have made a local copy of the resources in this Storage account: https://hdiconfigactions.blob.core.windows.net/.</span></span>
* <span data-ttu-id="264fe-200">Se till att klustret anpassning skriptet idempotent</span><span class="sxs-lookup"><span data-stu-id="264fe-200">Ensure that the cluster customization script is idempotent</span></span>

    <span data-ttu-id="264fe-201">Förväntat att noder i ett HDInsight-kluster är avbildade under klustrets livslängd.</span><span class="sxs-lookup"><span data-stu-id="264fe-201">You must expect that the nodes of an HDInsight cluster is reimaged during the cluster lifetime.</span></span> <span data-ttu-id="264fe-202">Klustret anpassning skript körs när ett kluster avbildade.</span><span class="sxs-lookup"><span data-stu-id="264fe-202">The cluster customization script is run whenever a cluster is reimaged.</span></span> <span data-ttu-id="264fe-203">Det här skriptet måste utformas ska vara idempotent i den mening att efter återställning av avbildning, skriptet bör se till att klustret returneras till samma anpassade tillståndet den var i när skriptet kördes för första gången när klustret skapades.</span><span class="sxs-lookup"><span data-stu-id="264fe-203">This script must be designed to be idempotent in the sense that upon reimaging, the script should ensure that the cluster is returned to the same customized state that it was in just after the script ran for the first time when the cluster was initially created.</span></span> <span data-ttu-id="264fe-204">Till exempel om ett anpassat skript har installerat ett program på D:\AppLocation första körs på varje efterföljande körning vid återställning av avbildning, skriptet ska kontrollera om programmet redan finns på plats D:\AppLocation innan du fortsätter med andra steg i den skript.</span><span class="sxs-lookup"><span data-stu-id="264fe-204">For example, if a custom script installed an application at D:\AppLocation on its first run, then on each subsequent run, upon reimaging, the script should check whether the application exists at the D:\AppLocation location before proceeding with other steps in the script.</span></span>
* <span data-ttu-id="264fe-205">Installera komponenter för anpassade optimala plats</span><span class="sxs-lookup"><span data-stu-id="264fe-205">Install custom components in the optimal location</span></span>

    <span data-ttu-id="264fe-206">Om klusternoderna är avbildade kan C:\ resurs enheten och D:\ systemenhet formateras om, vilket resulterar i förlust av data och program som har installerats på dessa enheter.</span><span class="sxs-lookup"><span data-stu-id="264fe-206">When cluster nodes are reimaged, the C:\ resource drive and D:\ system drive can be reformatted, resulting in the loss of data and applications that had been installed on those drives.</span></span> <span data-ttu-id="264fe-207">Detta kan också inträffa om en virtuell Azure-dator (VM)-nod som ingår i klustret slutar fungera och ersätts av en ny nod.</span><span class="sxs-lookup"><span data-stu-id="264fe-207">This could also happen if an Azure virtual machine (VM) node that is part of the cluster goes down and is replaced by a new node.</span></span> <span data-ttu-id="264fe-208">Du kan installera komponenterna på D:\-enhet eller på C:\apps plats på klustret.</span><span class="sxs-lookup"><span data-stu-id="264fe-208">You can install components on the D:\ drive or in the C:\apps location on the cluster.</span></span> <span data-ttu-id="264fe-209">Alla andra platser på enhet C:\ är reserverade.</span><span class="sxs-lookup"><span data-stu-id="264fe-209">All other locations on the C:\ drive are reserved.</span></span> <span data-ttu-id="264fe-210">Ange platsen för program eller bibliotek som ska installeras i klustret anpassning skriptet.</span><span class="sxs-lookup"><span data-stu-id="264fe-210">Specify the location where applications or libraries are to be installed in the cluster customization script.</span></span>
* <span data-ttu-id="264fe-211">Garantera hög tillgänglighet för kluster-arkitektur</span><span class="sxs-lookup"><span data-stu-id="264fe-211">Ensure high availability of the cluster architecture</span></span>

    <span data-ttu-id="264fe-212">HDInsight har ett aktivt-passivt arkitektur för hög tillgänglighet som en huvudnod är i aktivt läge (där HDInsight-tjänsterna körs) och andra Huvudnoden är i vänteläge (i vilken HDInsight tjänster inte körs).</span><span class="sxs-lookup"><span data-stu-id="264fe-212">HDInsight has an active-passive architecture for high availability, in which one head node is in active mode (where the HDInsight services are running) and the other head node is in standby mode (in which HDInsight services are not running).</span></span> <span data-ttu-id="264fe-213">Noderna växla mellan lägena aktiva och passiva om HDInsight tjänster avbryts.</span><span class="sxs-lookup"><span data-stu-id="264fe-213">The nodes switch active and passive modes if HDInsight services are interrupted.</span></span> <span data-ttu-id="264fe-214">Om en skriptåtgärd används för att installera tjänster på båda huvudnoderna för hög tillgänglighet, Observera att mekanismen för HDInsight-redundans inte kan automatiskt växla över dessa användare installerade tjänster.</span><span class="sxs-lookup"><span data-stu-id="264fe-214">If a script action is used to install services on both head nodes for high availability, note that the HDInsight failover mechanism is not able to automatically fail over these user-installed services.</span></span> <span data-ttu-id="264fe-215">Så användaren installerade tjänster på HDInsight huvudnoderna som förväntas ha hög tillgänglighet måste antingen ha sina egna mekanism för växling vid fel om i aktivt-passivt läge eller vara i läget för aktiv-aktiv.</span><span class="sxs-lookup"><span data-stu-id="264fe-215">So user-installed services on HDInsight head nodes that are expected to be highly available must either have their own failover mechanism if in active-passive mode or be in active-active mode.</span></span>

    <span data-ttu-id="264fe-216">Ett HDInsight-skriptåtgärder kommando som körs på båda huvudnoderna när rollen head-noden har angetts som ett värde i den *ClusterRoleCollection* parameter.</span><span class="sxs-lookup"><span data-stu-id="264fe-216">An HDInsight Script Action command runs on both head nodes when the head-node role is specified as a value in the *ClusterRoleCollection* parameter.</span></span> <span data-ttu-id="264fe-217">Så när du skapar ett anpassat skript, se till att skriptet är medveten om den här installationen.</span><span class="sxs-lookup"><span data-stu-id="264fe-217">So when you design a custom script, make sure that your script is aware of this setup.</span></span> <span data-ttu-id="264fe-218">Du bör inte köra problem där samma tjänster är installerade och igång på både huvudnoderna och hamnar konkurrerar med varandra.</span><span class="sxs-lookup"><span data-stu-id="264fe-218">You should not run into problems where the same services are installed and started on both of the head nodes and they end up competing with each other.</span></span> <span data-ttu-id="264fe-219">Tänk också på att data går förlorade vid återställning av avbildning, så att programvaran via skriptåtgärd måste vara motståndskraftiga mot sådana händelser.</span><span class="sxs-lookup"><span data-stu-id="264fe-219">Also, be aware that data is lost during reimaging, so software installed via Script Action has to be resilient to such events.</span></span> <span data-ttu-id="264fe-220">Program bör utformas för att arbeta med hög tillgänglighet data som distribueras över flera noder.</span><span class="sxs-lookup"><span data-stu-id="264fe-220">Applications should be designed to work with highly available data that is distributed across many nodes.</span></span> <span data-ttu-id="264fe-221">Observera att du kan avbildade upp till 1/5 noder i ett kluster på samma gång.</span><span class="sxs-lookup"><span data-stu-id="264fe-221">Note that as many as 1/5 of the nodes in a cluster can be reimaged at the same time.</span></span>
* <span data-ttu-id="264fe-222">Konfigurera anpassade komponenter om du vill använda Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="264fe-222">Configure the custom components to use Azure Blob storage</span></span>

    <span data-ttu-id="264fe-223">De anpassade komponenter som installeras på klusternoderna kan ha en standardkonfiguration lagringsmedier Hadoop Distributed File System (HDFS).</span><span class="sxs-lookup"><span data-stu-id="264fe-223">The custom components that you install on the cluster nodes might have a default configuration to use Hadoop Distributed File System (HDFS) storage.</span></span> <span data-ttu-id="264fe-224">Du bör ändra konfigurationen för att använda Azure Blob storage i stället.</span><span class="sxs-lookup"><span data-stu-id="264fe-224">You should change the configuration to use Azure Blob storage instead.</span></span> <span data-ttu-id="264fe-225">På ett kluster avbildningsåterställning HDFS hämtar filsystemet och du skulle förlora alla data som lagras där.</span><span class="sxs-lookup"><span data-stu-id="264fe-225">On a cluster reimage, the HDFS file system gets formatted and you would lose any data that is stored there.</span></span> <span data-ttu-id="264fe-226">Azure Blob storage i stället garanterar att dina data bevaras.</span><span class="sxs-lookup"><span data-stu-id="264fe-226">Using Azure Blob storage instead ensures that your data is retained.</span></span>

## <a name="common-usage-patterns"></a><span data-ttu-id="264fe-227">Vanliga användningsmönster</span><span class="sxs-lookup"><span data-stu-id="264fe-227">Common usage patterns</span></span>
<span data-ttu-id="264fe-228">Det här avsnittet ger vägledning om att implementera några av de vanliga användningsmönster som kan uppstå vid skrivning till ett eget skript.</span><span class="sxs-lookup"><span data-stu-id="264fe-228">This section provides guidance on implementing some of the common usage patterns that you might run into while writing your own custom script.</span></span>

### <a name="configure-environment-variables"></a><span data-ttu-id="264fe-229">Konfigurera miljövariabler</span><span class="sxs-lookup"><span data-stu-id="264fe-229">Configure environment variables</span></span>
<span data-ttu-id="264fe-230">I skriptutveckling, kan du ofta anser att behöva ange miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="264fe-230">Often in script action development, you feel the need to set environment variables.</span></span> <span data-ttu-id="264fe-231">Exempelvis är mest sannolika scenariot när du hämtar en binär från en extern plats, installera den på klustret och Lägg till platsen för där den är installerad 'PATH'-miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="264fe-231">For instance, a most likely scenario is when you download a binary from an external site, install it on the cluster, and add the location of where it is installed to your ‘PATH’ environment variable.</span></span> <span data-ttu-id="264fe-232">Följande utdrag visar hur du anger miljövariabler i skriptet.</span><span class="sxs-lookup"><span data-stu-id="264fe-232">The following snippet shows you how to set environment variables in the custom script.</span></span>

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

<span data-ttu-id="264fe-233">Den här instruktionen anger miljövariabeln **MDS_RUNNER_CUSTOM_CLUSTER** med värdet 'true' och även anger omfånget för den här variabeln ska vara datoromfattande.</span><span class="sxs-lookup"><span data-stu-id="264fe-233">This statement sets the environment variable **MDS_RUNNER_CUSTOM_CLUSTER** to the value 'true' and also sets the scope of this variable to be machine-wide.</span></span> <span data-ttu-id="264fe-234">Ibland är det viktigt att miljövariablerna anges på en lämplig omfattning – dator eller användare.</span><span class="sxs-lookup"><span data-stu-id="264fe-234">At times it is important that environment variables are set at the appropriate scope – machine or user.</span></span> <span data-ttu-id="264fe-235">Se [här] [ 1] för mer information om hur du anger miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="264fe-235">Refer [here][1] for more information on setting environment variables.</span></span>

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a><span data-ttu-id="264fe-236">Åtkomst till platser där anpassade skript lagras</span><span class="sxs-lookup"><span data-stu-id="264fe-236">Access to locations where the custom scripts are stored</span></span>
<span data-ttu-id="264fe-237">Skript som används för att anpassa ett kluster måste antingen vara i standardkontot för lagring för klustret eller i en offentlig skrivskyddad behållare för andra storage-konto.</span><span class="sxs-lookup"><span data-stu-id="264fe-237">Scripts used to customize a cluster needs to either be in the default storage account for the cluster or in a public read-only container on any other storage account.</span></span> <span data-ttu-id="264fe-238">Om skriptet har åtkomst till resurser som finns någon annanstans dessa måste vara en offentligt tillgänglig (minst offentliga skrivskyddad).</span><span class="sxs-lookup"><span data-stu-id="264fe-238">If your script accesses resources located elsewhere these need to be in a publicly accessible (at least public read-only).</span></span> <span data-ttu-id="264fe-239">Du kanske exempelvis vill komma åt en fil och spara den med hjälp av kommandot SaveFile HDI.</span><span class="sxs-lookup"><span data-stu-id="264fe-239">For instance you might want to access a file and save it using the SaveFile-HDI command.</span></span>

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

<span data-ttu-id="264fe-240">I det här exemplet måste du kontrollera att behållaren somecontainer om du i storage-konto 'somestorageaccount' är allmänt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="264fe-240">In this example, you must ensure that the container 'somecontainer' in storage account 'somestorageaccount' is publicly accessible.</span></span> <span data-ttu-id="264fe-241">Annars genereras ett undantag 'Gick inte att hitta' skriptet och misslyckas.</span><span class="sxs-lookup"><span data-stu-id="264fe-241">Otherwise, the script throws a ‘Not Found’ exception and fail.</span></span>

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a><span data-ttu-id="264fe-242">Skicka parametrar för cmdleten Add-AzureRmHDInsightScriptAction</span><span class="sxs-lookup"><span data-stu-id="264fe-242">Pass parameters to the Add-AzureRmHDInsightScriptAction cmdlet</span></span>
<span data-ttu-id="264fe-243">Om du vill lägga till flera parametrar för cmdleten Add-AzureRmHDInsightScriptAction, måste du formatera strängvärde så att den innehåller alla parametrar för skriptet.</span><span class="sxs-lookup"><span data-stu-id="264fe-243">To pass multiple parameters to the Add-AzureRmHDInsightScriptAction cmdlet, you need to format the string value to contain all parameters for the script.</span></span> <span data-ttu-id="264fe-244">Exempel:</span><span class="sxs-lookup"><span data-stu-id="264fe-244">For example:</span></span>

    "-CertifcateUri wasb:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

<span data-ttu-id="264fe-245">eller</span><span class="sxs-lookup"><span data-stu-id="264fe-245">or</span></span>

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a><span data-ttu-id="264fe-246">Utlös undantag för misslyckade Klusterdistribution</span><span class="sxs-lookup"><span data-stu-id="264fe-246">Throw exception for failed cluster deployment</span></span>
<span data-ttu-id="264fe-247">Om du vill hämta exakt ett meddelande om att kluster anpassning slutfördes inte som förväntat, är det viktigt att utlösa ett undantag och kunde inte skapa ett kluster.</span><span class="sxs-lookup"><span data-stu-id="264fe-247">If you want to get accurately notified of the fact that cluster customization did not succeed as expected, it is important to throw an exception and fail the cluster creation.</span></span> <span data-ttu-id="264fe-248">Du kanske exempelvis vill bearbeta en fil om den finns och hantera fel fallet där filen inte finns.</span><span class="sxs-lookup"><span data-stu-id="264fe-248">For instance, you might want to process a file if it exists and handle the error case where the file does not exist.</span></span> <span data-ttu-id="264fe-249">Detta skulle se till att skriptet avslutas utan problem och kallas korrekt tillstånd för klustret.</span><span class="sxs-lookup"><span data-stu-id="264fe-249">This would ensure that the script exits gracefully and the state of the cluster is correctly known.</span></span> <span data-ttu-id="264fe-250">Följande kodavsnitt ger ett exempel på hur du kan åstadkomma detta:</span><span class="sxs-lookup"><span data-stu-id="264fe-250">The following snippet gives an example of how to achieve this:</span></span>

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

<span data-ttu-id="264fe-251">I det här kodstycket om filen inte finns, skulle det leda till ett tillstånd där skriptet faktiskt avslutas korrekt efter utskrift felmeddelandet och klustret når körtillstånd under förutsättning att den ”” slutförts anpassningsprocess av klustret.</span><span class="sxs-lookup"><span data-stu-id="264fe-251">In this snippet, if the file did not exist, it would lead to a state where the script actually exits gracefully after printing the error message, and the cluster reaches running state assuming it "successfully" completed cluster customization process.</span></span> <span data-ttu-id="264fe-252">Om du vill meddelas korrekt att klustret anpassning i stort sett slutfördes inte som förväntat på grund av en fil som saknas, det är mer lämpligt att utlösa ett undantag och misslyckas steget klustret anpassning.</span><span class="sxs-lookup"><span data-stu-id="264fe-252">If you want to be accurately notified of the fact that cluster customization essentially did not succeed as expected because of a missing file, it is more appropriate to throw an exception and fail the cluster customization step.</span></span> <span data-ttu-id="264fe-253">För att åstadkomma detta måste du använda följande exempel kodfragment i stället.</span><span class="sxs-lookup"><span data-stu-id="264fe-253">To achieve this you must use the following sample code snippet instead.</span></span>

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a><span data-ttu-id="264fe-254">Checklista för distribution av en skriptåtgärd</span><span class="sxs-lookup"><span data-stu-id="264fe-254">Checklist for deploying a script action</span></span>
<span data-ttu-id="264fe-255">Här följer de steg som vi har tagit när du förbereder att distribuera dessa skript:</span><span class="sxs-lookup"><span data-stu-id="264fe-255">Here are the steps we took when preparing to deploy these scripts:</span></span>

1. <span data-ttu-id="264fe-256">Placera de filer som innehåller anpassade skript på en plats som kan nås av klusternoder under distributionen.</span><span class="sxs-lookup"><span data-stu-id="264fe-256">Put the files that contain the custom scripts in a place that is accessible by the cluster nodes during deployment.</span></span> <span data-ttu-id="264fe-257">Detta kan vara någon av standard eller ytterligare lagringskonton som anges vid tidpunkten för distribution eller andra offentligt tillgänglig lagringsbehållaren.</span><span class="sxs-lookup"><span data-stu-id="264fe-257">This can be any of the default or additional Storage accounts specified at the time of cluster deployment, or any other publicly accessible storage container.</span></span>
2. <span data-ttu-id="264fe-258">Lägga till kontroller i skript för att se till att de kör idempotently, så att skriptet kan köras flera gånger på samma nod.</span><span class="sxs-lookup"><span data-stu-id="264fe-258">Add checks into scripts to make sure that they execute idempotently, so that the script can be executed multiple times on the same node.</span></span>
3. <span data-ttu-id="264fe-259">Använd den **Write-Output** Azure PowerShell-cmdlet för att skriva ut till STDOUT och STDERR.</span><span class="sxs-lookup"><span data-stu-id="264fe-259">Use the **Write-Output** Azure PowerShell cmdlet to print to STDOUT as well as STDERR.</span></span> <span data-ttu-id="264fe-260">Använd inte **Write-Host**.</span><span class="sxs-lookup"><span data-stu-id="264fe-260">Do not use **Write-Host**.</span></span>
4. <span data-ttu-id="264fe-261">Använda en tillfällig mapp, till exempel $env: TEMP att hålla den hämta filen som används av skripten och sedan rensa dem efter skript har körts.</span><span class="sxs-lookup"><span data-stu-id="264fe-261">Use a temporary file folder, such as $env:TEMP, to keep the downloaded file used by the scripts and then clean them up after scripts have executed.</span></span>
5. <span data-ttu-id="264fe-262">Installera anpassade programvara på D:\ eller C:\apps.</span><span class="sxs-lookup"><span data-stu-id="264fe-262">Install custom software only at D:\ or C:\apps.</span></span> <span data-ttu-id="264fe-263">Andra platser på enhet C: ska inte användas som de är reserverade.</span><span class="sxs-lookup"><span data-stu-id="264fe-263">Other locations on the C: drive should not be used as they are reserved.</span></span> <span data-ttu-id="264fe-264">Observera att installera filer på enhet C: utanför mappen C:\apps kan resultera i installationsfel under reimages för noden.</span><span class="sxs-lookup"><span data-stu-id="264fe-264">Note that installing files on the C: drive outside of the C:\apps folder may result in setup failures during reimages of the node.</span></span>
6. <span data-ttu-id="264fe-265">I händelse av att inställningarna för OS-nivå eller Hadoop service configuration-filer har ändrats, kan du vill starta om HDInsight-tjänster så att de kan hämta OS-nivå inställningar, till exempel miljövariabler som anges i skripten.</span><span class="sxs-lookup"><span data-stu-id="264fe-265">In the event that OS-level settings or Hadoop service configuration files were changed, you may want to restart HDInsight services so that they can pick up any OS-level settings, such as the environment variables set in the scripts.</span></span>

## <a name="debug-custom-scripts"></a><span data-ttu-id="264fe-266">Felsöka anpassade skript</span><span class="sxs-lookup"><span data-stu-id="264fe-266">Debug custom scripts</span></span>
<span data-ttu-id="264fe-267">Felloggarna skriptet lagras tillsammans med andra utdata i standardkontot för lagring som angetts för klustret när skapandet.</span><span class="sxs-lookup"><span data-stu-id="264fe-267">The script error logs are stored, along with other output, in the default Storage account that you specified for the cluster at its creation.</span></span> <span data-ttu-id="264fe-268">Loggfilerna lagras i en tabell med namnet *u < \cluster-name-fragment >< \time-stamp > setuplog*.</span><span class="sxs-lookup"><span data-stu-id="264fe-268">The logs are stored in a table with the name *u<\cluster-name-fragment><\time-stamp>setuplog*.</span></span> <span data-ttu-id="264fe-269">Det här är sammanställda loggar som har poster från alla noder (huvudnod och arbetarnoder) där skriptet körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="264fe-269">These are aggregated logs that have records from all of the nodes (head node and worker nodes) on which the script runs in the cluster.</span></span>
<span data-ttu-id="264fe-270">Ett enkelt sätt att kontrollera loggarna är att använda HDInsight Tools för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="264fe-270">An easy way to check the logs is to use HDInsight Tools for Visual Studio.</span></span> <span data-ttu-id="264fe-271">För att installera verktygen finns [komma igång med Visual Studio Hadoop-verktyg för HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)</span><span class="sxs-lookup"><span data-stu-id="264fe-271">For installing the tools, see [Get started using Visual Studio Hadoop tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)</span></span>

<span data-ttu-id="264fe-272">**Kontrollera loggen med hjälp av Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="264fe-272">**To check the log using Visual Studio**</span></span>

1. <span data-ttu-id="264fe-273">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="264fe-273">Open Visual Studio.</span></span>
2. <span data-ttu-id="264fe-274">Klicka på **visa**, och klicka sedan på **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="264fe-274">Click **View**, and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="264fe-275">Högerklicka på ”Azure”, klicka på Anslut till **Microsoft Azure-prenumerationer**, och ange dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="264fe-275">Right-click "Azure", click Connect to **Microsoft Azure Subscriptions**, and then enter your credentials.</span></span>
4. <span data-ttu-id="264fe-276">Expandera **lagring**, expandera Azure storage-konto som används som standardfilsystem, expandera **tabeller**, och dubbelklicka sedan på namnet på tabellen.</span><span class="sxs-lookup"><span data-stu-id="264fe-276">Expand **Storage**, expand the Azure storage account used as the default file system, expand **Tables**, and then double-click the table name.</span></span>

<span data-ttu-id="264fe-277">Du kan också fjärråtkomst till klusternoderna finns både STDOUT och STDERR för anpassade skript.</span><span class="sxs-lookup"><span data-stu-id="264fe-277">You can also remote into the cluster nodes to see both STDOUT and STDERR for custom scripts.</span></span> <span data-ttu-id="264fe-278">Loggar på varje nod bara gäller den noden och loggas i **C:\HDInsightLogs\DeploymentAgent.log**.</span><span class="sxs-lookup"><span data-stu-id="264fe-278">The logs on each node are specific only to that node and are logged into **C:\HDInsightLogs\DeploymentAgent.log**.</span></span> <span data-ttu-id="264fe-279">Dessa loggfilerna registrerar alla utdata från skriptet.</span><span class="sxs-lookup"><span data-stu-id="264fe-279">These log files record all outputs from the custom script.</span></span> <span data-ttu-id="264fe-280">Ett exempel loggen kodstycke för en Spark skriptåtgärd ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="264fe-280">An example log snippet for a Spark script action looks like this:</span></span>

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


<span data-ttu-id="264fe-281">I den här loggen är det klart att skriptåtgärd Spark har körts på den virtuella datorn med namnet HEADNODE0 och att inga undantag utlöstes under körningen.</span><span class="sxs-lookup"><span data-stu-id="264fe-281">In this log, it is clear that the Spark script action has been executed on the VM named HEADNODE0 and that no exceptions were thrown during the execution.</span></span>

<span data-ttu-id="264fe-282">I händelse av att ett körningsfel inträffar ingå utdata som beskriver den också i den här loggfilen.</span><span class="sxs-lookup"><span data-stu-id="264fe-282">In the event that an execution failure occurs, the output describing it is also contained in this log file.</span></span> <span data-ttu-id="264fe-283">Informationen i dessa loggar är användbar vid felsökning av problem med skript som kan uppstå.</span><span class="sxs-lookup"><span data-stu-id="264fe-283">The information provided in these logs should be helpful in debugging script problems that may arise.</span></span>

## <a name="see-also"></a><span data-ttu-id="264fe-284">Se även</span><span class="sxs-lookup"><span data-stu-id="264fe-284">See also</span></span>
* <span data-ttu-id="264fe-285">[Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]</span><span class="sxs-lookup"><span data-stu-id="264fe-285">[Customize HDInsight clusters using Script Action][hdinsight-cluster-customize]</span></span>
* <span data-ttu-id="264fe-286">[Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="264fe-286">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="264fe-287">[Installera och använda R i HDInsight-kluster][hdinsight-r-scripts]</span><span class="sxs-lookup"><span data-stu-id="264fe-287">[Install and use R on HDInsight clusters][hdinsight-r-scripts]</span></span>
* <span data-ttu-id="264fe-288">[Installera och använda Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="264fe-288">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="264fe-289">[Installera och använda Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="264fe-289">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
