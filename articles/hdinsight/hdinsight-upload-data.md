---
title: "Överföra data för Hadoop-jobb i HDInsight | Microsoft Docs"
description: "Lär dig mer om att överföra och komma åt data för Hadoop-jobb i HDInsight med hjälp av Azure CLI, Azure Lagringsutforskaren, Azure PowerShell, kommandorad för Hadoop eller Sqoop."
keywords: "etl-hadoop, hämta data till hadoop, Läs in data för hadoop"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 56b913ee-0f9a-4e9f-9eaf-c571f8603dd6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: jgao
ms.openlocfilehash: 6867f96c8ea0e31ed0e682cef48e7aa5e3f65f86
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a><span data-ttu-id="63749-104">Ladda upp data för Hadoop-jobb i HDInsight</span><span class="sxs-lookup"><span data-stu-id="63749-104">Upload data for Hadoop jobs in HDInsight</span></span>
<span data-ttu-id="63749-105">Azure HDInsight ger en komplett Hadoop distributed file system (HDFS) över Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="63749-105">Azure HDInsight provides a full-featured Hadoop distributed file system (HDFS) over Azure Blob storage.</span></span> <span data-ttu-id="63749-106">Den är utformad som ett HDFS-tillägg för att förse kunder en sömlös upplevelse.</span><span class="sxs-lookup"><span data-stu-id="63749-106">It is designed as an HDFS extension to provide a seamless experience to customers.</span></span> <span data-ttu-id="63749-107">Den gör en fullständig uppsättning komponenter i Hadoop-ekosystemet att arbeta direkt med de data som den hanterar.</span><span class="sxs-lookup"><span data-stu-id="63749-107">It enables the full set of components in the Hadoop ecosystem to operate directly on the data it manages.</span></span> <span data-ttu-id="63749-108">Azure Blob storage och HDFS är distinkt filsystem som är optimerade för lagring av data och beräkningar på dessa data.</span><span class="sxs-lookup"><span data-stu-id="63749-108">Azure Blob storage and HDFS are distinct file systems that are optimized for storage of data and computations on that data.</span></span> <span data-ttu-id="63749-109">Information om fördelarna med att använda Azure Blob storage finns [använda Azure Blob storage med HDInsight][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="63749-109">For information about the benefits of using Azure Blob storage, see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="63749-110">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="63749-110">**Prerequisites**</span></span>

<span data-ttu-id="63749-111">Observera följande krav innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="63749-111">Note the following requirement before you begin:</span></span>

* <span data-ttu-id="63749-112">Ett Azure HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="63749-112">An Azure HDInsight cluster.</span></span> <span data-ttu-id="63749-113">Instruktioner finns i [komma igång med Azure HDInsight] [ hdinsight-get-started] eller [etablera HDInsight-kluster][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="63749-113">For instructions, see [Get started with Azure HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span>

## <a name="why-blob-storage"></a><span data-ttu-id="63749-114">Varför blob storage?</span><span class="sxs-lookup"><span data-stu-id="63749-114">Why blob storage?</span></span>
<span data-ttu-id="63749-115">Azure HDInsight-kluster distribueras vanligtvis för att köra MapReduce-jobb och kluster tas bort när de här jobben har slutförts.</span><span class="sxs-lookup"><span data-stu-id="63749-115">Azure HDInsight clusters are typically deployed to run MapReduce jobs, and the clusters are dropped after these jobs complete.</span></span> <span data-ttu-id="63749-116">Behålla data i HDFS är kluster när beräkningar har slutförts en kostsam sätt att lagra dessa data.</span><span class="sxs-lookup"><span data-stu-id="63749-116">Keeping the data in the HDFS clusters after computations are complete would be an expensive way to store this data.</span></span> <span data-ttu-id="63749-117">Azure Blob storage är en hög tillgänglighet, skalbara, hög kapacitet, låg kostnad och delbart lagringsalternativ för data som ska bearbetas med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="63749-117">Azure Blob storage is a highly available, highly scalable, high capacity, low cost, and shareable storage option for data that is to be processed using HDInsight.</span></span> <span data-ttu-id="63749-118">Lagra data i en blob kan HDInsight-kluster som används för beräkning frigörs utan att förlora data på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="63749-118">Storing data in a blob enables the HDInsight clusters that are used for computation to be safely released without losing data.</span></span>

### <a name="directories"></a><span data-ttu-id="63749-119">Kataloger</span><span class="sxs-lookup"><span data-stu-id="63749-119">Directories</span></span>
<span data-ttu-id="63749-120">Azure Blob storage-behållare som lagrar data som nyckel/värde-par och det finns ingen Kataloghierarki.</span><span class="sxs-lookup"><span data-stu-id="63749-120">Azure Blob storage containers store data as key/value pairs, and there is no directory hierarchy.</span></span> <span data-ttu-id="63749-121">Men tecknet ”/” kan användas i nyckelnamnet så att det visas som om en fil lagras i en katalogstruktur.</span><span class="sxs-lookup"><span data-stu-id="63749-121">However the "/" character can be used within the key name to make it appear as if a file is stored within a directory structure.</span></span> <span data-ttu-id="63749-122">HDInsight ser dessa som om de faktiska kataloger.</span><span class="sxs-lookup"><span data-stu-id="63749-122">HDInsight sees these as if they are actual directories.</span></span>

<span data-ttu-id="63749-123">Nyckeln för en blob kan till exempel vara *input/log1.txt*.</span><span class="sxs-lookup"><span data-stu-id="63749-123">For example, a blob's key may be *input/log1.txt*.</span></span> <span data-ttu-id="63749-124">Det finns inga faktiska ”input” katalog, men på grund av förekomsten av ”/”-tecken i namnet har utseendet på en sökväg till filen.</span><span class="sxs-lookup"><span data-stu-id="63749-124">No actual "input" directory exists, but due to the presence of the "/" character in the key name, it has the appearance of a file path.</span></span>

<span data-ttu-id="63749-125">Om du använder Azure Explorer-verktyg kan du märka vissa 0 byte-filer.</span><span class="sxs-lookup"><span data-stu-id="63749-125">Because of this, if you use Azure Explorer tools you may notice some 0 byte files.</span></span> <span data-ttu-id="63749-126">Filerna har två syften:</span><span class="sxs-lookup"><span data-stu-id="63749-126">These files serve two purposes:</span></span>

* <span data-ttu-id="63749-127">Om det finns tomma mappar, markera de av finns i mappen.</span><span class="sxs-lookup"><span data-stu-id="63749-127">If there are empty folders, they mark of the existence of the folder.</span></span> <span data-ttu-id="63749-128">Azure Blob storage är smarta du behöver veta om det finns en blob som kallas foo-fältet, det finns en mapp med namnet **foo**.</span><span class="sxs-lookup"><span data-stu-id="63749-128">Azure Blob storage is clever enough to know that if a blob called foo/bar exists, there is a folder called **foo**.</span></span> <span data-ttu-id="63749-129">Men det enda sättet för att ange en tom mapp att kallas **foo** av är att ha särskilda 0 byte filen på plats.</span><span class="sxs-lookup"><span data-stu-id="63749-129">But the only way to signify an empty folder called **foo** is by having this special 0 byte file in place.</span></span>
* <span data-ttu-id="63749-130">De innehåller särskilda metadata som krävs av Hadoop-filsystem, särskilt behörigheter och ägare för mappar.</span><span class="sxs-lookup"><span data-stu-id="63749-130">They hold special metadata that is needed by the Hadoop file system, notably the permissions and owners for the folders.</span></span>

## <a name="command-line-utilities"></a><span data-ttu-id="63749-131">Kommandoradsverktyg</span><span class="sxs-lookup"><span data-stu-id="63749-131">Command-line utilities</span></span>
<span data-ttu-id="63749-132">Microsoft tillhandahåller följande verktyg för att arbeta med Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="63749-132">Microsoft provides the following utilities to work with Azure Blob storage:</span></span>

| <span data-ttu-id="63749-133">Verktyget</span><span class="sxs-lookup"><span data-stu-id="63749-133">Tool</span></span> | <span data-ttu-id="63749-134">Linux</span><span class="sxs-lookup"><span data-stu-id="63749-134">Linux</span></span> | <span data-ttu-id="63749-135">OS X</span><span class="sxs-lookup"><span data-stu-id="63749-135">OS X</span></span> | <span data-ttu-id="63749-136">Windows</span><span class="sxs-lookup"><span data-stu-id="63749-136">Windows</span></span> |
| --- |:---:|:---:|:---:|
| <span data-ttu-id="63749-137">[Azure-kommandoradsgränssnittet][azurecli]</span><span class="sxs-lookup"><span data-stu-id="63749-137">[Azure Command-Line Interface][azurecli]</span></span> |<span data-ttu-id="63749-138">✔</span><span class="sxs-lookup"><span data-stu-id="63749-138">✔</span></span> |<span data-ttu-id="63749-139">✔</span><span class="sxs-lookup"><span data-stu-id="63749-139">✔</span></span> |<span data-ttu-id="63749-140">✔</span><span class="sxs-lookup"><span data-stu-id="63749-140">✔</span></span> |
| <span data-ttu-id="63749-141">[Azure PowerShell][azure-powershell]</span><span class="sxs-lookup"><span data-stu-id="63749-141">[Azure PowerShell][azure-powershell]</span></span> | | |<span data-ttu-id="63749-142">✔</span><span class="sxs-lookup"><span data-stu-id="63749-142">✔</span></span> |
| <span data-ttu-id="63749-143">[AzCopy][azure-azcopy]</span><span class="sxs-lookup"><span data-stu-id="63749-143">[AzCopy][azure-azcopy]</span></span> | | |<span data-ttu-id="63749-144">✔</span><span class="sxs-lookup"><span data-stu-id="63749-144">✔</span></span> |
| [<span data-ttu-id="63749-145">Hadoop-kommando</span><span class="sxs-lookup"><span data-stu-id="63749-145">Hadoop command</span></span>](#commandline) |<span data-ttu-id="63749-146">✔</span><span class="sxs-lookup"><span data-stu-id="63749-146">✔</span></span> |<span data-ttu-id="63749-147">✔</span><span class="sxs-lookup"><span data-stu-id="63749-147">✔</span></span> |<span data-ttu-id="63749-148">✔</span><span class="sxs-lookup"><span data-stu-id="63749-148">✔</span></span> |

> [!NOTE]
> <span data-ttu-id="63749-149">Medan Azure CLI, Azure PowerShell och AzCopy kan alla användas utanför Azure, Hadoop kommandot är endast tillgängligt för HDInsight-klustret och kan bara läsa in data från det lokala filsystemet i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="63749-149">While the Azure CLI, Azure PowerShell, and AzCopy can all be used from outside Azure, the Hadoop command is only available on the HDInsight cluster and only allows loading data from the local file system into Azure Blob storage.</span></span>
>
>

### <span data-ttu-id="63749-150"><a id="xplatcli"></a>Azure CLI</span><span class="sxs-lookup"><span data-stu-id="63749-150"><a id="xplatcli"></a>Azure CLI</span></span>
<span data-ttu-id="63749-151">Azure CLI är ett plattformsoberoende verktyg som hjälper dig att hantera Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="63749-151">The Azure CLI is a cross-platform tool that allows you to manage Azure services.</span></span> <span data-ttu-id="63749-152">Använd följande steg för att överföra data till Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="63749-152">Use the following steps to upload data to Azure Blob storage:</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. <span data-ttu-id="63749-153">[Installera och konfigurera Azure CLI för Mac, Linux och Windows](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="63749-153">[Install and configure the Azure CLI for Mac, Linux and Windows](../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="63749-154">Öppna en kommandotolk, bash eller andra gränssnitt och Använd följande för att autentisera till Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="63749-154">Open a command prompt, bash, or other shell, and use the following to authenticate to your Azure subscription.</span></span>

        azure login

    <span data-ttu-id="63749-155">När du uppmanas, anger du användarnamn och lösenord för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="63749-155">When prompted, enter the user name and password for your subscription.</span></span>
3. <span data-ttu-id="63749-156">Ange följande kommando för att visa en lista med lagringskonton för din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="63749-156">Enter the following command to list the storage accounts for your subscription:</span></span>

        azure storage account list
4. <span data-ttu-id="63749-157">Välj lagringskonto som innehåller blob som du vill arbeta med och sedan använder du följande kommando för att hämta nyckeln för det här kontot:</span><span class="sxs-lookup"><span data-stu-id="63749-157">Select the storage account that contains the blob you want to work with, then use the following command to retrieve the key for this account:</span></span>

        azure storage account keys list <storage-account-name>

    <span data-ttu-id="63749-158">Detta bör returnera **primära** och **sekundära** nycklar.</span><span class="sxs-lookup"><span data-stu-id="63749-158">This should return **Primary** and **Secondary** keys.</span></span> <span data-ttu-id="63749-159">Kopiera den **primära** nyckelvärdet eftersom den används i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="63749-159">Copy the **Primary** key value because it will be used in the next steps.</span></span>
5. <span data-ttu-id="63749-160">Använd följande kommando för att hämta en lista över blobbbehållare i storage-konto:</span><span class="sxs-lookup"><span data-stu-id="63749-160">Use the following command to retrieve a list of blob containers within the storage account:</span></span>

        azure storage container list -a <storage-account-name> -k <primary-key>
6. <span data-ttu-id="63749-161">Ladda upp och laddar ned filer till blob med hjälp av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="63749-161">Use the following commands to upload and download files to the blob:</span></span>

   * <span data-ttu-id="63749-162">Att överföra en fil:</span><span class="sxs-lookup"><span data-stu-id="63749-162">To upload a file:</span></span>

           azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
   * <span data-ttu-id="63749-163">Att hämta en fil:</span><span class="sxs-lookup"><span data-stu-id="63749-163">To download a file:</span></span>

           azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [!NOTE]
> <span data-ttu-id="63749-164">Om du kommer alltid att arbeta med samma lagringskonto, kan du ange följande miljövariabler i stället för att ange konto och nyckel för varje kommando:</span><span class="sxs-lookup"><span data-stu-id="63749-164">If you will always be working with the same storage account, you can set the following environment variables instead of specifying the account and key for every command:</span></span>
>
> * <span data-ttu-id="63749-165">**AZURE\_lagring\_konto**: lagringskontonamnet</span><span class="sxs-lookup"><span data-stu-id="63749-165">**AZURE\_STORAGE\_ACCOUNT**: The storage account name</span></span>
> * <span data-ttu-id="63749-166">**AZURE\_lagring\_åtkomst\_NYCKELN**: lagringskontots åtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="63749-166">**AZURE\_STORAGE\_ACCESS\_KEY**: The storage account key</span></span>
>
>

### <span data-ttu-id="63749-167"><a id="powershell"></a>Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="63749-167"><a id="powershell"></a>Azure PowerShell</span></span>
<span data-ttu-id="63749-168">Azure PowerShell är en skriptmiljö som du kan använda för att styra och automatisera distributionen och hanteringen av dina arbetsbelastningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="63749-168">Azure PowerShell is a scripting environment that you can use to control and automate the deployment and management of your workloads in Azure.</span></span> <span data-ttu-id="63749-169">Information om hur du konfigurerar din arbetsstation för att köra Azure PowerShell finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="63749-169">For information about configuring your workstation to run Azure PowerShell, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="63749-170">**Att överföra en lokal fil till Azure Blob storage**</span><span class="sxs-lookup"><span data-stu-id="63749-170">**To upload a local file to Azure Blob storage**</span></span>

1. <span data-ttu-id="63749-171">Öppna Azure PowerShell-konsol som finns beskrivet i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="63749-171">Open the Azure PowerShell console as instructed in [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
2. <span data-ttu-id="63749-172">Ange värden för de första fem variablerna i skriptet med följande:</span><span class="sxs-lookup"><span data-stu-id="63749-172">Set the values of the first five variables in the following script:</span></span>

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
3. <span data-ttu-id="63749-173">Klistra in skriptet i Azure PowerShell-konsolen för att köra den att kopiera filen.</span><span class="sxs-lookup"><span data-stu-id="63749-173">Paste the script into the Azure PowerShell console to run it to copy the file.</span></span>

<span data-ttu-id="63749-174">Till exempel PowerShell-skript som skapats för att arbeta med HDInsight, se [HDInsight tools](https://github.com/blackmist/hdinsight-tools).</span><span class="sxs-lookup"><span data-stu-id="63749-174">For example PowerShell scripts created to work with HDInsight, see [HDInsight tools](https://github.com/blackmist/hdinsight-tools).</span></span>

### <span data-ttu-id="63749-175"><a id="azcopy"></a>AzCopy</span><span class="sxs-lookup"><span data-stu-id="63749-175"><a id="azcopy"></a>AzCopy</span></span>
<span data-ttu-id="63749-176">AzCopy är ett kommandoradsverktyg som förenklar uppgiften att överföra data till och från ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="63749-176">AzCopy is a command-line tool that is designed to simplify the task of transferring data into and out of an Azure Storage account.</span></span> <span data-ttu-id="63749-177">Du kan använda den som ett fristående verktyg eller innehålla verktyget i ett befintligt program.</span><span class="sxs-lookup"><span data-stu-id="63749-177">You can use it as a standalone tool or incorporate this tool in an existing application.</span></span> <span data-ttu-id="63749-178">[Hämta AzCopy][azure-azcopy-download].</span><span class="sxs-lookup"><span data-stu-id="63749-178">[Download AzCopy][azure-azcopy-download].</span></span>

<span data-ttu-id="63749-179">AzCopy-syntaxen är:</span><span class="sxs-lookup"><span data-stu-id="63749-179">The AzCopy syntax is:</span></span>

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

<span data-ttu-id="63749-180">Mer information finns i [AzCopy - överföring/hämta filer för Azure-BLOB][azure-azcopy].</span><span class="sxs-lookup"><span data-stu-id="63749-180">For more information, see [AzCopy - Uploading/Downloading files for Azure Blobs][azure-azcopy].</span></span>

### <span data-ttu-id="63749-181"><a id="commandline"></a>Hadoop-kommandorad</span><span class="sxs-lookup"><span data-stu-id="63749-181"><a id="commandline"></a>Hadoop command line</span></span>
<span data-ttu-id="63749-182">Hadoop-kommandoraden är bara användbara för att lagra data i blob-lagring när data finns redan på klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="63749-182">The Hadoop command line is only useful for storing data into blob storage when the data is already present on the cluster head node.</span></span>

<span data-ttu-id="63749-183">För att kunna använda kommandot Hadoop, måste du först ansluta till headnode med någon av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="63749-183">In order to use the Hadoop command, you must first connect to the headnode using one of the following methods:</span></span>

* <span data-ttu-id="63749-184">**Windows-baserade HDInsight**: [ansluta med hjälp av fjärrskrivbord](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span><span class="sxs-lookup"><span data-stu-id="63749-184">**Windows-based HDInsight**: [Connect using Remote Desktop](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span></span>
* <span data-ttu-id="63749-185">**Linux-baserat HDInsight**: ansluta via SSH ([SSH-kommandot](hdinsight-hadoop-linux-use-ssh-unix.md) eller [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))</span><span class="sxs-lookup"><span data-stu-id="63749-185">**Linux-based HDInsight**: Connect using SSH ([the SSH command](hdinsight-hadoop-linux-use-ssh-unix.md) or [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))</span></span>

<span data-ttu-id="63749-186">När du är ansluten, kan du använda följande syntax för att överföra en fil till lagring.</span><span class="sxs-lookup"><span data-stu-id="63749-186">Once connected, you can use the following syntax to upload a file to storage.</span></span>

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

<span data-ttu-id="63749-187">Till exempel, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`</span><span class="sxs-lookup"><span data-stu-id="63749-187">For example, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`</span></span>

<span data-ttu-id="63749-188">Eftersom filsystemet för HDInsight i Azure Blob storage är /example/data.txt i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="63749-188">Because the default file system for HDInsight is in Azure Blob storage, /example/data.txt is actually in Azure Blob storage.</span></span> <span data-ttu-id="63749-189">Du kan också gå till filen som:</span><span class="sxs-lookup"><span data-stu-id="63749-189">You can also refer to the file as:</span></span>

    wasb:///example/data/data.txt

<span data-ttu-id="63749-190">eller</span><span class="sxs-lookup"><span data-stu-id="63749-190">or</span></span>

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

<span data-ttu-id="63749-191">En lista med andra Hadoop-kommandon som fungerar med filer, finns [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)</span><span class="sxs-lookup"><span data-stu-id="63749-191">For a list of other Hadoop commands that work with files, see [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)</span></span>

> [!WARNING]
> <span data-ttu-id="63749-192">I HBase-kluster standard blockstorlek som används när data skrivs är 256KB.</span><span class="sxs-lookup"><span data-stu-id="63749-192">On HBase clusters, the default block size used when writing data is 256KB.</span></span> <span data-ttu-id="63749-193">När det här fungerar bra när du använder HBase APIs eller REST API: er med hjälp av den `hadoop` eller `hdfs dfs` kommandon för att skriva data som är större än ~ 12 GB resulterar i ett fel.</span><span class="sxs-lookup"><span data-stu-id="63749-193">While this works fine when using HBase APIs or REST APIs, using the `hadoop` or `hdfs dfs` commands to write data larger than ~12GB results in an error.</span></span> <span data-ttu-id="63749-194">Finns det [skrivning till blob storage-undantag](#storageexception) avsnittet nedan för mer information.</span><span class="sxs-lookup"><span data-stu-id="63749-194">See the [storage exception for write on blob](#storageexception) section below for more information.</span></span>
>
>

## <a name="graphical-clients"></a><span data-ttu-id="63749-195">Grafisk klienter</span><span class="sxs-lookup"><span data-stu-id="63749-195">Graphical clients</span></span>
<span data-ttu-id="63749-196">Det finns också flera program som ger ett grafiskt gränssnitt för att arbeta med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="63749-196">There are also several applications that provide a graphical interface for working with Azure Storage.</span></span> <span data-ttu-id="63749-197">Följande är en lista över några av dessa program:</span><span class="sxs-lookup"><span data-stu-id="63749-197">The following is a list of a few of these applications:</span></span>

| <span data-ttu-id="63749-198">Client</span><span class="sxs-lookup"><span data-stu-id="63749-198">Client</span></span> | <span data-ttu-id="63749-199">Linux</span><span class="sxs-lookup"><span data-stu-id="63749-199">Linux</span></span> | <span data-ttu-id="63749-200">OS X</span><span class="sxs-lookup"><span data-stu-id="63749-200">OS X</span></span> | <span data-ttu-id="63749-201">Windows</span><span class="sxs-lookup"><span data-stu-id="63749-201">Windows</span></span> |
| --- |:---:|:---:|:---:|
| [<span data-ttu-id="63749-202">Microsoft Visual Studio-verktygen för HDInsight</span><span class="sxs-lookup"><span data-stu-id="63749-202">Microsoft Visual Studio Tools for HDInsight</span></span>](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) |<span data-ttu-id="63749-203">✔</span><span class="sxs-lookup"><span data-stu-id="63749-203">✔</span></span> |<span data-ttu-id="63749-204">✔</span><span class="sxs-lookup"><span data-stu-id="63749-204">✔</span></span> |<span data-ttu-id="63749-205">✔</span><span class="sxs-lookup"><span data-stu-id="63749-205">✔</span></span> |
| [<span data-ttu-id="63749-206">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="63749-206">Azure Storage Explorer</span></span>](http://storageexplorer.com/) |<span data-ttu-id="63749-207">✔</span><span class="sxs-lookup"><span data-stu-id="63749-207">✔</span></span> |<span data-ttu-id="63749-208">✔</span><span class="sxs-lookup"><span data-stu-id="63749-208">✔</span></span> |<span data-ttu-id="63749-209">✔</span><span class="sxs-lookup"><span data-stu-id="63749-209">✔</span></span> |
| [<span data-ttu-id="63749-210">Molnet lagring Studio 2</span><span class="sxs-lookup"><span data-stu-id="63749-210">Cloud Storage Studio 2</span></span>](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |<span data-ttu-id="63749-211">✔</span><span class="sxs-lookup"><span data-stu-id="63749-211">✔</span></span> |
| [<span data-ttu-id="63749-212">CloudXplorer</span><span class="sxs-lookup"><span data-stu-id="63749-212">CloudXplorer</span></span>](http://clumsyleaf.com/products/cloudxplorer) | | |<span data-ttu-id="63749-213">✔</span><span class="sxs-lookup"><span data-stu-id="63749-213">✔</span></span> |
| [<span data-ttu-id="63749-214">Azure Explorer</span><span class="sxs-lookup"><span data-stu-id="63749-214">Azure Explorer</span></span>](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |<span data-ttu-id="63749-215">✔</span><span class="sxs-lookup"><span data-stu-id="63749-215">✔</span></span> |
| [<span data-ttu-id="63749-216">Cyberduck</span><span class="sxs-lookup"><span data-stu-id="63749-216">Cyberduck</span></span>](https://cyberduck.io/) | |<span data-ttu-id="63749-217">✔</span><span class="sxs-lookup"><span data-stu-id="63749-217">✔</span></span> |<span data-ttu-id="63749-218">✔</span><span class="sxs-lookup"><span data-stu-id="63749-218">✔</span></span> |

### <a name="visual-studio-tools-for-hdinsight"></a><span data-ttu-id="63749-219">Visual Studio-verktygen för HDInsight</span><span class="sxs-lookup"><span data-stu-id="63749-219">Visual Studio Tools for HDInsight</span></span>
<span data-ttu-id="63749-220">Mer information finns i [navigera de länkade resurserna](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).</span><span class="sxs-lookup"><span data-stu-id="63749-220">For more information, see [Navigate the linked resources](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).</span></span>

### <span data-ttu-id="63749-221"><a id="storageexplorer"></a>Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="63749-221"><a id="storageexplorer"></a>Azure Storage Explorer</span></span>
<span data-ttu-id="63749-222">*Azure Lagringsutforskaren* är användbart för att kontrollera och ändra data i BLOB.</span><span class="sxs-lookup"><span data-stu-id="63749-222">*Azure Storage Explorer* is a useful tool for inspecting and altering the data in blobs.</span></span> <span data-ttu-id="63749-223">Det är ett kostnadsfritt, Öppna källa verktyg som kan hämtas från [http://storageexplorer.com/](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="63749-223">It is a free, open source tool that can be downloaded from [http://storageexplorer.com/](http://storageexplorer.com/).</span></span> <span data-ttu-id="63749-224">Källkoden är tillgänglig från den här länken samt.</span><span class="sxs-lookup"><span data-stu-id="63749-224">The source code is available from this link as well.</span></span>

<span data-ttu-id="63749-225">Innan du använder verktyget, måste du känna till din Azure storage-konto och nyckel.</span><span class="sxs-lookup"><span data-stu-id="63749-225">Before using the tool, you must know your Azure storage account name and account key.</span></span> <span data-ttu-id="63749-226">Anvisningar om hur du får den här informationen finns i ”så här: visa, kopiera och generera lagring åtkomstnycklar” avsnitt i [skapa, hantera eller ta bort ett lagringskonto][azure-create-storage-account].</span><span class="sxs-lookup"><span data-stu-id="63749-226">For instructions about getting this information, see the "How to: View, copy and regenerate storage access keys" section of [Create, manage, or delete a storage account][azure-create-storage-account].</span></span>

1. <span data-ttu-id="63749-227">Kör Azure Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="63749-227">Run Azure Storage Explorer.</span></span> <span data-ttu-id="63749-228">Om det här är första gången du har kört Lagringsutforskaren, du uppmanas att den **_Storage kontonamn** och **lagringskontonyckel**.</span><span class="sxs-lookup"><span data-stu-id="63749-228">If this is the first time you have run the Storage Explorer, you will be prompted for the **_Storage account name** and **Storage account key**.</span></span> <span data-ttu-id="63749-229">Om du har kört den innan du använder den **Lägg till** för att lägga till ett nytt lagringskontonamn och nyckel.</span><span class="sxs-lookup"><span data-stu-id="63749-229">If you have run it before, use the **Add** button to add a new storage account name and key.</span></span>

    <span data-ttu-id="63749-230">Ange namnet och nyckeln för storage-konto som används av ditt HDInsight-kluster och välj sedan **Spara & Öppna**.</span><span class="sxs-lookup"><span data-stu-id="63749-230">Enter the name and key for the storage account used by your HDInsight cluster and then select **SAVE & OPEN**.</span></span>

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]
2. <span data-ttu-id="63749-232">I listan över behållare till vänster om gränssnittet, klickar du på namnet på behållaren som är kopplad till ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="63749-232">In the list of containers to the left of the interface, click the name of the container that is associated with your HDInsight cluster.</span></span> <span data-ttu-id="63749-233">Detta är namnet på HDInsight-klustret som standard men kan skilja sig om du har angett ett specifikt namn när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="63749-233">By default, this is the name of the HDInsight cluster, but may be different if you entered a specific name when creating the cluster.</span></span>
3. <span data-ttu-id="63749-234">I verktygsfältet, väljer du ikonen överföringen.</span><span class="sxs-lookup"><span data-stu-id="63749-234">From the tool bar, select the upload icon.</span></span>

    ![Verktygsfält med överför ikonen markerat](./media/hdinsight-upload-data/toolbar.png)
4. <span data-ttu-id="63749-236">Ange en fil för att ladda upp och klicka sedan på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="63749-236">Specify a file to upload, and then click **Open**.</span></span> <span data-ttu-id="63749-237">När du uppmanas, Välj **överför** att överföra filen till roten på lagringsbehållaren.</span><span class="sxs-lookup"><span data-stu-id="63749-237">When prompted, select **Upload** to upload the file to the root of the storage container.</span></span> <span data-ttu-id="63749-238">Om du vill överföra filen till en specifik sökväg anger du sökvägen i den **mål** fältet och välj sedan **överför**.</span><span class="sxs-lookup"><span data-stu-id="63749-238">If you want to upload the file to a specific path, enter the path in the **Destination** field and then select **Upload**.</span></span>

    ![Överför fildialogruta](./media/hdinsight-upload-data/fileupload.png)

    <span data-ttu-id="63749-240">När filen har överförts, kan du använda den från jobb i HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="63749-240">Once the file has finished uploading, you can use it from jobs on the HDInsight cluster.</span></span>

## <a name="mount-azure-blob-storage-as-local-drive"></a><span data-ttu-id="63749-241">Montera Azure Blob Storage som lokal enhet</span><span class="sxs-lookup"><span data-stu-id="63749-241">Mount Azure Blob Storage as Local Drive</span></span>
<span data-ttu-id="63749-242">Se [montera Azure Blob Storage som lokala enhet](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).</span><span class="sxs-lookup"><span data-stu-id="63749-242">See [Mount Azure Blob Storage as Local Drive](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).</span></span>

## <a name="services"></a><span data-ttu-id="63749-243">Tjänster</span><span class="sxs-lookup"><span data-stu-id="63749-243">Services</span></span>
### <a name="azure-data-factory"></a><span data-ttu-id="63749-244">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="63749-244">Azure Data Factory</span></span>
<span data-ttu-id="63749-245">Azure Data Factory-tjänsten är en helt hanterad tjänst för att skapa lagring, databehandling och data movement datatjänster till produktion pipeline-effektiviserad, skalbara och tillförlitliga data.</span><span class="sxs-lookup"><span data-stu-id="63749-245">The Azure Data Factory service is a fully managed service for composing data storage, data processing, and data movement services into streamlined, scalable, and reliable data production pipelines.</span></span>

<span data-ttu-id="63749-246">Azure Data Factory kan användas för att flytta data till Azure Blob storage, eller för att skapa data pipelines som använder HDInsight-funktioner, till exempel Hive och Pig direkt.</span><span class="sxs-lookup"><span data-stu-id="63749-246">Azure Data Factory can be used to move data into Azure Blob storage, or to create data pipelines that directly use HDInsight features such as Hive and Pig.</span></span>

<span data-ttu-id="63749-247">Mer information finns i [dokumentation för Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="63749-247">For more information, see the [Azure Data Factory documentation](https://azure.microsoft.com/documentation/services/data-factory/).</span></span>

### <span data-ttu-id="63749-248"><a id="sqoop"></a>Apache Sqoop</span><span class="sxs-lookup"><span data-stu-id="63749-248"><a id="sqoop"></a>Apache Sqoop</span></span>
<span data-ttu-id="63749-249">Sqoop är ett verktyg som utformats för att överföra data mellan Hadoop och relationsdatabaser.</span><span class="sxs-lookup"><span data-stu-id="63749-249">Sqoop is a tool designed to transfer data between Hadoop and relational databases.</span></span> <span data-ttu-id="63749-250">Du kan använda den för att importera data från ett relationella databashanteringssystem (RDBMS), exempelvis SQL Server, MySQL eller Oracle i Hadoop distributed file system (HDFS) Transformera data i Hadoop med MapReduce eller Hive och exportera data till en RDBMS.</span><span class="sxs-lookup"><span data-stu-id="63749-250">You can use it to import data from a relational database management system (RDBMS), such as SQL Server, MySQL, or Oracle into the Hadoop distributed file system (HDFS), transform the data in Hadoop with MapReduce or Hive, and then export the data back into an RDBMS.</span></span>

<span data-ttu-id="63749-251">Mer information finns i [använda Sqoop med HDInsight][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="63749-251">For more information, see [Use Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

## <a name="development-sdks"></a><span data-ttu-id="63749-252">SDK: er för utveckling</span><span class="sxs-lookup"><span data-stu-id="63749-252">Development SDKs</span></span>
<span data-ttu-id="63749-253">Azure Blob storage kan även nås med ett Azure SDK från följande programmeringsspråk:</span><span class="sxs-lookup"><span data-stu-id="63749-253">Azure Blob storage can also be accessed using an Azure SDK from the following programming languages:</span></span>

* <span data-ttu-id="63749-254">.NET</span><span class="sxs-lookup"><span data-stu-id="63749-254">.NET</span></span>
* <span data-ttu-id="63749-255">Java</span><span class="sxs-lookup"><span data-stu-id="63749-255">Java</span></span>
* <span data-ttu-id="63749-256">Node.js</span><span class="sxs-lookup"><span data-stu-id="63749-256">Node.js</span></span>
* <span data-ttu-id="63749-257">PHP</span><span class="sxs-lookup"><span data-stu-id="63749-257">PHP</span></span>
* <span data-ttu-id="63749-258">Python</span><span class="sxs-lookup"><span data-stu-id="63749-258">Python</span></span>
* <span data-ttu-id="63749-259">Ruby</span><span class="sxs-lookup"><span data-stu-id="63749-259">Ruby</span></span>

<span data-ttu-id="63749-260">Mer information om hur du installerar Azure SDK: erna finns [hämtar Azure](https://azure.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="63749-260">For more information on installing the Azure SDKs, see [Azure downloads](https://azure.microsoft.com/downloads/)</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="63749-261">Felsökning</span><span class="sxs-lookup"><span data-stu-id="63749-261">Troubleshooting</span></span>
### <span data-ttu-id="63749-262"><a id="storageexception"></a>Skrivning till blob Storage-undantag</span><span class="sxs-lookup"><span data-stu-id="63749-262"><a id="storageexception"></a>Storage exception for write on blob</span></span>
<span data-ttu-id="63749-263">**Symptom**: när du använder den `hadoop` eller `hdfs dfs` kommandon för att skriva filer som är ~ 12 GB eller större på ett HBase-kluster kan du stöta på följande fel:</span><span class="sxs-lookup"><span data-stu-id="63749-263">**Symptoms**: When using the `hadoop` or `hdfs dfs` commands to write files that are ~12GB or larger on an HBase cluster, you may encounter the following error:</span></span>

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

<span data-ttu-id="63749-264">**Orsak**: HBase på HDInsight-kluster standard till en blockstorlek på 256 KB när du skriver till Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="63749-264">**Cause**: HBase on HDInsight clusters default to a block size of 256KB when writing to Azure storage.</span></span> <span data-ttu-id="63749-265">När det här fungerar för HBase APIs eller REST API: er det kommer att resultera i ett fel när du använder den `hadoop` eller `hdfs dfs` kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="63749-265">While this works for HBase APIs or REST APIs, it will result in an error when using the `hadoop` or `hdfs dfs` command-line utilities.</span></span>

<span data-ttu-id="63749-266">**Lösning**: Använd `fs.azure.write.request.size` att ange en större blockstorlek.</span><span class="sxs-lookup"><span data-stu-id="63749-266">**Resolution**: Use `fs.azure.write.request.size` to specify a larger block size.</span></span> <span data-ttu-id="63749-267">Du kan göra detta på grundval av per användning med hjälp av den `-D` parameter.</span><span class="sxs-lookup"><span data-stu-id="63749-267">You can do this on a per-use basis by using the `-D` parameter.</span></span> <span data-ttu-id="63749-268">Följande är ett exempel som använder den här parametern med det `hadoop` kommando:</span><span class="sxs-lookup"><span data-stu-id="63749-268">The following is an example using this parameter with the `hadoop` command:</span></span>

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

<span data-ttu-id="63749-269">Du kan också öka värdet för `fs.azure.write.request.size` globalt med Ambari.</span><span class="sxs-lookup"><span data-stu-id="63749-269">You can also increase the value of `fs.azure.write.request.size` globally by using Ambari.</span></span> <span data-ttu-id="63749-270">Följande steg kan användas för att ändra värdet i Ambari-Webbgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="63749-270">The following steps can be used to change the value in the Ambari Web UI:</span></span>

1. <span data-ttu-id="63749-271">Gå till Ambari-Webbgränssnittet för klustret i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="63749-271">In your browser, go to the Ambari Web UI for your cluster.</span></span> <span data-ttu-id="63749-272">Detta är https://CLUSTERNAME.azurehdinsight.net, där **KLUSTERNAMN** är namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="63749-272">This is https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is the name of your cluster.</span></span>

    <span data-ttu-id="63749-273">När du uppmanas, anger du admin namn och lösenord för klustret.</span><span class="sxs-lookup"><span data-stu-id="63749-273">When prompted, enter the admin name and password for the cluster.</span></span>
2. <span data-ttu-id="63749-274">Vänster sida av skärmen, Välj **HDFS**, och välj sedan den **konfigurationerna** fliken.</span><span class="sxs-lookup"><span data-stu-id="63749-274">From the left side of the screen, select **HDFS**, and then select the **Configs** tab.</span></span>
3. <span data-ttu-id="63749-275">I den **Filter...**  anger `fs.azure.write.request.size`.</span><span class="sxs-lookup"><span data-stu-id="63749-275">In the **Filter...** field, enter `fs.azure.write.request.size`.</span></span> <span data-ttu-id="63749-276">Detta visas i fältet och det aktuella värdet i mitten på sidan.</span><span class="sxs-lookup"><span data-stu-id="63749-276">This will display the field and current value in the middle of the page.</span></span>
4. <span data-ttu-id="63749-277">Ändra värdet från 262144 (256KB) till det nya värdet.</span><span class="sxs-lookup"><span data-stu-id="63749-277">Change the value from 262144 (256KB) to the new value.</span></span> <span data-ttu-id="63749-278">Till exempel 4194304 (4MB).</span><span class="sxs-lookup"><span data-stu-id="63749-278">For example, 4194304 (4MB).</span></span>

![Bild av ändra värdet via Ambari-Webbgränssnittet](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

<span data-ttu-id="63749-280">Läs mer om hur du använder Ambari [hantera HDInsight-kluster med Ambari-Webbgränssnittet](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="63749-280">For more information on using Ambari, see [Manage HDInsight clusters using the Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="63749-281">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="63749-281">Next steps</span></span>
<span data-ttu-id="63749-282">Nu när du vet hur du hämta data till HDInsight kan du läsa följande artiklar om du vill lära dig att utföra analyser av:</span><span class="sxs-lookup"><span data-stu-id="63749-282">Now that you understand how to get data into HDInsight, read the following articles to learn how to perform analysis:</span></span>

* <span data-ttu-id="63749-283">[Kom igång med Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="63749-283">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="63749-284">[Skicka Hadoop-jobb via programmering][hdinsight-submit-jobs]</span><span class="sxs-lookup"><span data-stu-id="63749-284">[Submit Hadoop jobs programmatically][hdinsight-submit-jobs]</span></span>
* <span data-ttu-id="63749-285">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="63749-285">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="63749-286">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="63749-286">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>

[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]:../storage/common/storage-create-storage-account.md
[azure-azcopy-download]:../storage/common/storage-use-azcopy.md
[azure-azcopy]:../storage/common/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[azurecli]: ../cli-install-nodejs.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
