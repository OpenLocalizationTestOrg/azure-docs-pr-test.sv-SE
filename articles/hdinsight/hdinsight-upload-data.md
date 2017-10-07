---
title: "aaaUpload data för Hadoop-jobb i HDInsight | Microsoft Docs"
description: "Lär dig hur tooupload och komma åt data för Hadoop-jobb i HDInsight med hello Azure CLI, Azure Lagringsutforskaren, Azure PowerShell, kommandorad för hello Hadoop eller Sqoop."
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
ms.openlocfilehash: 15da602085d41c19789e34800f3d9e238d7d1de8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a><span data-ttu-id="3e990-104">Ladda upp data för Hadoop-jobb i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e990-104">Upload data for Hadoop jobs in HDInsight</span></span>
<span data-ttu-id="3e990-105">Azure HDInsight ger en komplett Hadoop distributed file system (HDFS) över Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="3e990-105">Azure HDInsight provides a full-featured Hadoop distributed file system (HDFS) over Azure Blob storage.</span></span> <span data-ttu-id="3e990-106">Den är utformad som en HDFS tillägget tooprovide en sömlös upplevelse toocustomers.</span><span class="sxs-lookup"><span data-stu-id="3e990-106">It is designed as an HDFS extension tooprovide a seamless experience toocustomers.</span></span> <span data-ttu-id="3e990-107">Den gör hello fullständig uppsättning komponenter i hello Hadoop-ekosystemet toooperate direkt på hello data som den hanterar.</span><span class="sxs-lookup"><span data-stu-id="3e990-107">It enables hello full set of components in hello Hadoop ecosystem toooperate directly on hello data it manages.</span></span> <span data-ttu-id="3e990-108">Azure Blob storage och HDFS är distinkt filsystem som är optimerade för lagring av data och beräkningar på dessa data.</span><span class="sxs-lookup"><span data-stu-id="3e990-108">Azure Blob storage and HDFS are distinct file systems that are optimized for storage of data and computations on that data.</span></span> <span data-ttu-id="3e990-109">Information om hello fördelarna med att använda Azure Blob storage finns [använda Azure Blob storage med HDInsight][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="3e990-109">For information about hello benefits of using Azure Blob storage, see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="3e990-110">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="3e990-110">**Prerequisites**</span></span>

<span data-ttu-id="3e990-111">Observera följande krav innan du börjar hello:</span><span class="sxs-lookup"><span data-stu-id="3e990-111">Note hello following requirement before you begin:</span></span>

* <span data-ttu-id="3e990-112">Ett Azure HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3e990-112">An Azure HDInsight cluster.</span></span> <span data-ttu-id="3e990-113">Instruktioner finns i [komma igång med Azure HDInsight] [ hdinsight-get-started] eller [etablera HDInsight-kluster][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="3e990-113">For instructions, see [Get started with Azure HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span>

## <a name="why-blob-storage"></a><span data-ttu-id="3e990-114">Varför blob storage?</span><span class="sxs-lookup"><span data-stu-id="3e990-114">Why blob storage?</span></span>
<span data-ttu-id="3e990-115">Azure HDInsight-kluster är vanligtvis distribuerats toorun MapReduce-jobb och hello kluster tas bort när de här jobben har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3e990-115">Azure HDInsight clusters are typically deployed toorun MapReduce jobs, and hello clusters are dropped after these jobs complete.</span></span> <span data-ttu-id="3e990-116">Behålla hello data i hello HDFS kluster när beräkningar har slutförts skulle vara en kostsam sätt toostore dessa data.</span><span class="sxs-lookup"><span data-stu-id="3e990-116">Keeping hello data in hello HDFS clusters after computations are complete would be an expensive way toostore this data.</span></span> <span data-ttu-id="3e990-117">Azure Blob storage är en hög tillgänglighet, skalbara, hög kapacitet, låg kostnad och delbart lagringsalternativ för data som är toobe bearbetas med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e990-117">Azure Blob storage is a highly available, highly scalable, high capacity, low cost, and shareable storage option for data that is toobe processed using HDInsight.</span></span> <span data-ttu-id="3e990-118">Lagra data i en blob kan hello HDInsight-kluster som används för beräkning toobe publicerat på ett säkert sätt utan att förlora data.</span><span class="sxs-lookup"><span data-stu-id="3e990-118">Storing data in a blob enables hello HDInsight clusters that are used for computation toobe safely released without losing data.</span></span>

### <a name="directories"></a><span data-ttu-id="3e990-119">Kataloger</span><span class="sxs-lookup"><span data-stu-id="3e990-119">Directories</span></span>
<span data-ttu-id="3e990-120">Azure Blob storage-behållare som lagrar data som nyckel/värde-par och det finns ingen Kataloghierarki.</span><span class="sxs-lookup"><span data-stu-id="3e990-120">Azure Blob storage containers store data as key/value pairs, and there is no directory hierarchy.</span></span> <span data-ttu-id="3e990-121">Men hello ”/” tecken kan användas i hello nyckelnamn toomake visas den som om en fil lagras i en katalogstruktur.</span><span class="sxs-lookup"><span data-stu-id="3e990-121">However hello "/" character can be used within hello key name toomake it appear as if a file is stored within a directory structure.</span></span> <span data-ttu-id="3e990-122">HDInsight ser dessa som om de faktiska kataloger.</span><span class="sxs-lookup"><span data-stu-id="3e990-122">HDInsight sees these as if they are actual directories.</span></span>

<span data-ttu-id="3e990-123">Nyckeln för en blob kan till exempel vara *input/log1.txt*.</span><span class="sxs-lookup"><span data-stu-id="3e990-123">For example, a blob's key may be *input/log1.txt*.</span></span> <span data-ttu-id="3e990-124">Ingen verklig ”input” katalog finns, men på grund av toohello förekomst av hello tecknet ”/” hello nyckelnamn, den har hello utseendet på en sökväg till filen.</span><span class="sxs-lookup"><span data-stu-id="3e990-124">No actual "input" directory exists, but due toohello presence of hello "/" character in hello key name, it has hello appearance of a file path.</span></span>

<span data-ttu-id="3e990-125">Om du använder Azure Explorer-verktyg kan du märka vissa 0 byte-filer.</span><span class="sxs-lookup"><span data-stu-id="3e990-125">Because of this, if you use Azure Explorer tools you may notice some 0 byte files.</span></span> <span data-ttu-id="3e990-126">Filerna har två syften:</span><span class="sxs-lookup"><span data-stu-id="3e990-126">These files serve two purposes:</span></span>

* <span data-ttu-id="3e990-127">Om det finns tomma mappar, markera de av hello finns hello-mappen.</span><span class="sxs-lookup"><span data-stu-id="3e990-127">If there are empty folders, they mark of hello existence of hello folder.</span></span> <span data-ttu-id="3e990-128">Azure Blob storage är tillräckligt smarta tooknow att om en blob som kallas foo-fältet finns det finns en mapp med namnet **foo**.</span><span class="sxs-lookup"><span data-stu-id="3e990-128">Azure Blob storage is clever enough tooknow that if a blob called foo/bar exists, there is a folder called **foo**.</span></span> <span data-ttu-id="3e990-129">Men hello endast sätt toosignify kallas för en tom mapp **foo** av är att ha särskilda 0 byte filen på plats.</span><span class="sxs-lookup"><span data-stu-id="3e990-129">But hello only way toosignify an empty folder called **foo** is by having this special 0 byte file in place.</span></span>
* <span data-ttu-id="3e990-130">De innehåller särskilda metadata som behövs av hello Hadoop filsystem, särskilt hello behörigheter och ägare för hello mappar.</span><span class="sxs-lookup"><span data-stu-id="3e990-130">They hold special metadata that is needed by hello Hadoop file system, notably hello permissions and owners for hello folders.</span></span>

## <a name="command-line-utilities"></a><span data-ttu-id="3e990-131">Kommandoradsverktyg</span><span class="sxs-lookup"><span data-stu-id="3e990-131">Command-line utilities</span></span>
<span data-ttu-id="3e990-132">Microsoft tillhandahåller hello följande verktyg toowork med Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="3e990-132">Microsoft provides hello following utilities toowork with Azure Blob storage:</span></span>

| <span data-ttu-id="3e990-133">Verktyget</span><span class="sxs-lookup"><span data-stu-id="3e990-133">Tool</span></span> | <span data-ttu-id="3e990-134">Linux</span><span class="sxs-lookup"><span data-stu-id="3e990-134">Linux</span></span> | <span data-ttu-id="3e990-135">OS X</span><span class="sxs-lookup"><span data-stu-id="3e990-135">OS X</span></span> | <span data-ttu-id="3e990-136">Windows</span><span class="sxs-lookup"><span data-stu-id="3e990-136">Windows</span></span> |
| --- |:---:|:---:|:---:|
| <span data-ttu-id="3e990-137">[Azure-kommandoradsgränssnittet][azurecli]</span><span class="sxs-lookup"><span data-stu-id="3e990-137">[Azure Command-Line Interface][azurecli]</span></span> |<span data-ttu-id="3e990-138">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-138">✔</span></span> |<span data-ttu-id="3e990-139">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-139">✔</span></span> |<span data-ttu-id="3e990-140">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-140">✔</span></span> |
| <span data-ttu-id="3e990-141">[Azure PowerShell][azure-powershell]</span><span class="sxs-lookup"><span data-stu-id="3e990-141">[Azure PowerShell][azure-powershell]</span></span> | | |<span data-ttu-id="3e990-142">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-142">✔</span></span> |
| <span data-ttu-id="3e990-143">[AzCopy][azure-azcopy]</span><span class="sxs-lookup"><span data-stu-id="3e990-143">[AzCopy][azure-azcopy]</span></span> | | |<span data-ttu-id="3e990-144">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-144">✔</span></span> |
| [<span data-ttu-id="3e990-145">Hadoop-kommando</span><span class="sxs-lookup"><span data-stu-id="3e990-145">Hadoop command</span></span>](#commandline) |<span data-ttu-id="3e990-146">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-146">✔</span></span> |<span data-ttu-id="3e990-147">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-147">✔</span></span> |<span data-ttu-id="3e990-148">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-148">✔</span></span> |

> [!NOTE]
> <span data-ttu-id="3e990-149">Medan hello Azure CLI, Azure PowerShell och AzCopy kan alla användas utanför Azure, hello Hadoop kommandot är bara tillgängligt på hello HDInsight-kluster och kan bara läsa in data från hello lokala filsystem i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="3e990-149">While hello Azure CLI, Azure PowerShell, and AzCopy can all be used from outside Azure, hello Hadoop command is only available on hello HDInsight cluster and only allows loading data from hello local file system into Azure Blob storage.</span></span>
>
>

### <span data-ttu-id="3e990-150"><a id="xplatcli"></a>Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3e990-150"><a id="xplatcli"></a>Azure CLI</span></span>
<span data-ttu-id="3e990-151">hello Azure CLI är ett verktyg för flera plattformar som du kan använda toomanage Azure services.</span><span class="sxs-lookup"><span data-stu-id="3e990-151">hello Azure CLI is a cross-platform tool that allows you toomanage Azure services.</span></span> <span data-ttu-id="3e990-152">Använd följande steg tooupload data tooAzure Blob storage hello:</span><span class="sxs-lookup"><span data-stu-id="3e990-152">Use hello following steps tooupload data tooAzure Blob storage:</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. <span data-ttu-id="3e990-153">[Installera och konfigurera hello Azure CLI för Mac, Linux och Windows](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="3e990-153">[Install and configure hello Azure CLI for Mac, Linux and Windows](../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="3e990-154">Öppna en kommandotolk, bash eller andra gränssnitt och använda hello följande tooauthenticate tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3e990-154">Open a command prompt, bash, or other shell, and use hello following tooauthenticate tooyour Azure subscription.</span></span>

        azure login

    <span data-ttu-id="3e990-155">När du uppmanas ange hello användarnamn och lösenord för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3e990-155">When prompted, enter hello user name and password for your subscription.</span></span>
3. <span data-ttu-id="3e990-156">Ange hello efter kommandot toolist hello storage-konton för din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="3e990-156">Enter hello following command toolist hello storage accounts for your subscription:</span></span>

        azure storage account list
4. <span data-ttu-id="3e990-157">Markera hello storage-konto som innehåller hello blob som du vill toowork med och använda följande kommando tooretrieve hello nyckeln för det här kontot hello:</span><span class="sxs-lookup"><span data-stu-id="3e990-157">Select hello storage account that contains hello blob you want toowork with, then use hello following command tooretrieve hello key for this account:</span></span>

        azure storage account keys list <storage-account-name>

    <span data-ttu-id="3e990-158">Detta bör returnera **primära** och **sekundära** nycklar.</span><span class="sxs-lookup"><span data-stu-id="3e990-158">This should return **Primary** and **Secondary** keys.</span></span> <span data-ttu-id="3e990-159">Kopiera hello **primära** nyckelvärdet eftersom den används i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="3e990-159">Copy hello **Primary** key value because it will be used in hello next steps.</span></span>
5. <span data-ttu-id="3e990-160">Använd hello efter kommandot tooretrieve en lista över blobbbehållare i hello storage-konto:</span><span class="sxs-lookup"><span data-stu-id="3e990-160">Use hello following command tooretrieve a list of blob containers within hello storage account:</span></span>

        azure storage container list -a <storage-account-name> -k <primary-key>
6. <span data-ttu-id="3e990-161">Använd följande kommandon tooupload hello och hämta filer toohello blob:</span><span class="sxs-lookup"><span data-stu-id="3e990-161">Use hello following commands tooupload and download files toohello blob:</span></span>

   * <span data-ttu-id="3e990-162">tooupload en fil:</span><span class="sxs-lookup"><span data-stu-id="3e990-162">tooupload a file:</span></span>

           azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
   * <span data-ttu-id="3e990-163">toodownload en fil:</span><span class="sxs-lookup"><span data-stu-id="3e990-163">toodownload a file:</span></span>

           azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [!NOTE]
> <span data-ttu-id="3e990-164">Om du kommer alltid att arbeta med hello samma lagringskonto som du kan ange hello följande miljövariabler i stället för att ange hello konto och nyckel för varje kommando:</span><span class="sxs-lookup"><span data-stu-id="3e990-164">If you will always be working with hello same storage account, you can set hello following environment variables instead of specifying hello account and key for every command:</span></span>
>
> * <span data-ttu-id="3e990-165">**AZURE\_lagring\_konto**: hello lagringskontonamn</span><span class="sxs-lookup"><span data-stu-id="3e990-165">**AZURE\_STORAGE\_ACCOUNT**: hello storage account name</span></span>
> * <span data-ttu-id="3e990-166">**AZURE\_lagring\_åtkomst\_NYCKELN**: hello lagringskontonyckel</span><span class="sxs-lookup"><span data-stu-id="3e990-166">**AZURE\_STORAGE\_ACCESS\_KEY**: hello storage account key</span></span>
>
>

### <span data-ttu-id="3e990-167"><a id="powershell"></a>Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="3e990-167"><a id="powershell"></a>Azure PowerShell</span></span>
<span data-ttu-id="3e990-168">Azure PowerShell är en skriptmiljö som du kan använda toocontrol och automatisera hello distribution och hantering av dina arbetsbelastningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="3e990-168">Azure PowerShell is a scripting environment that you can use toocontrol and automate hello deployment and management of your workloads in Azure.</span></span> <span data-ttu-id="3e990-169">Information om hur du konfigurerar din arbetsstation toorun Azure PowerShell finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3e990-169">For information about configuring your workstation toorun Azure PowerShell, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="3e990-170">**tooupload en lokal fil tooAzure Blob storage**</span><span class="sxs-lookup"><span data-stu-id="3e990-170">**tooupload a local file tooAzure Blob storage**</span></span>

1. <span data-ttu-id="3e990-171">Öppna hello Azure PowerShell-konsolen som finns beskrivet i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3e990-171">Open hello Azure PowerShell console as instructed in [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
2. <span data-ttu-id="3e990-172">Ange hello värdena för hello fem första variabler i hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="3e990-172">Set hello values of hello first five variables in hello following script:</span></span>

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get hello storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create hello storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy hello file from local workstation toohello Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
3. <span data-ttu-id="3e990-173">Klistra in hello skript i hello Azure PowerShell-konsolen toorun som toocopy hello-filen.</span><span class="sxs-lookup"><span data-stu-id="3e990-173">Paste hello script into hello Azure PowerShell console toorun it toocopy hello file.</span></span>

<span data-ttu-id="3e990-174">Till exempel PowerShell-skript har skapats toowork med HDInsight, se [HDInsight tools](https://github.com/blackmist/hdinsight-tools).</span><span class="sxs-lookup"><span data-stu-id="3e990-174">For example PowerShell scripts created toowork with HDInsight, see [HDInsight tools](https://github.com/blackmist/hdinsight-tools).</span></span>

### <span data-ttu-id="3e990-175"><a id="azcopy"></a>AzCopy</span><span class="sxs-lookup"><span data-stu-id="3e990-175"><a id="azcopy"></a>AzCopy</span></span>
<span data-ttu-id="3e990-176">AzCopy är ett kommandoradsverktyg som är utformade toosimplify hello aktiviteten för att överföra data till och från ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3e990-176">AzCopy is a command-line tool that is designed toosimplify hello task of transferring data into and out of an Azure Storage account.</span></span> <span data-ttu-id="3e990-177">Du kan använda den som ett fristående verktyg eller innehålla verktyget i ett befintligt program.</span><span class="sxs-lookup"><span data-stu-id="3e990-177">You can use it as a standalone tool or incorporate this tool in an existing application.</span></span> <span data-ttu-id="3e990-178">[Hämta AzCopy][azure-azcopy-download].</span><span class="sxs-lookup"><span data-stu-id="3e990-178">[Download AzCopy][azure-azcopy-download].</span></span>

<span data-ttu-id="3e990-179">Hej AzCopy syntaxen är:</span><span class="sxs-lookup"><span data-stu-id="3e990-179">hello AzCopy syntax is:</span></span>

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

<span data-ttu-id="3e990-180">Mer information finns i [AzCopy - överföring/hämta filer för Azure-BLOB][azure-azcopy].</span><span class="sxs-lookup"><span data-stu-id="3e990-180">For more information, see [AzCopy - Uploading/Downloading files for Azure Blobs][azure-azcopy].</span></span>

### <span data-ttu-id="3e990-181"><a id="commandline"></a>Hadoop-kommandorad</span><span class="sxs-lookup"><span data-stu-id="3e990-181"><a id="commandline"></a>Hadoop command line</span></span>
<span data-ttu-id="3e990-182">Hej Hadoop kommandoraden är bara användbara för att lagra data i blob-lagring när data hello finns redan på hello klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="3e990-182">hello Hadoop command line is only useful for storing data into blob storage when hello data is already present on hello cluster head node.</span></span>

<span data-ttu-id="3e990-183">I ordning toouse hello Hadoop-kommandot, måste du först ansluta toohello headnode med någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="3e990-183">In order toouse hello Hadoop command, you must first connect toohello headnode using one of hello following methods:</span></span>

* <span data-ttu-id="3e990-184">**Windows-baserade HDInsight**: [ansluta med hjälp av fjärrskrivbord](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span><span class="sxs-lookup"><span data-stu-id="3e990-184">**Windows-based HDInsight**: [Connect using Remote Desktop](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span></span>
* <span data-ttu-id="3e990-185">**Linux-baserat HDInsight**: ansluta via SSH ([hello SSH-kommandot](hdinsight-hadoop-linux-use-ssh-unix.md) eller [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))</span><span class="sxs-lookup"><span data-stu-id="3e990-185">**Linux-based HDInsight**: Connect using SSH ([hello SSH command](hdinsight-hadoop-linux-use-ssh-unix.md) or [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))</span></span>

<span data-ttu-id="3e990-186">När du är ansluten, kan du använda följande syntax tooupload en fil toostorage hello.</span><span class="sxs-lookup"><span data-stu-id="3e990-186">Once connected, you can use hello following syntax tooupload a file toostorage.</span></span>

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

<span data-ttu-id="3e990-187">Till exempel, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`</span><span class="sxs-lookup"><span data-stu-id="3e990-187">For example, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`</span></span>

<span data-ttu-id="3e990-188">Eftersom hello standardfilsystem för HDInsight i Azure Blob storage är /example/data.txt i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="3e990-188">Because hello default file system for HDInsight is in Azure Blob storage, /example/data.txt is actually in Azure Blob storage.</span></span> <span data-ttu-id="3e990-189">Du kan också se toohello filen som:</span><span class="sxs-lookup"><span data-stu-id="3e990-189">You can also refer toohello file as:</span></span>

    wasb:///example/data/data.txt

<span data-ttu-id="3e990-190">eller</span><span class="sxs-lookup"><span data-stu-id="3e990-190">or</span></span>

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

<span data-ttu-id="3e990-191">En lista med andra Hadoop-kommandon som fungerar med filer, finns [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)</span><span class="sxs-lookup"><span data-stu-id="3e990-191">For a list of other Hadoop commands that work with files, see [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)</span></span>

> [!WARNING]
> <span data-ttu-id="3e990-192">På HBase-kluster hello standardblockstorleken används när data skrivs är 256KB.</span><span class="sxs-lookup"><span data-stu-id="3e990-192">On HBase clusters, hello default block size used when writing data is 256KB.</span></span> <span data-ttu-id="3e990-193">När det här fungerar bra när du använder HBase APIs eller REST API: er med hjälp av hello `hadoop` eller `hdfs dfs` kommandon toowrite data är större än ~ 12 GB resulterar i ett fel.</span><span class="sxs-lookup"><span data-stu-id="3e990-193">While this works fine when using HBase APIs or REST APIs, using hello `hadoop` or `hdfs dfs` commands toowrite data larger than ~12GB results in an error.</span></span> <span data-ttu-id="3e990-194">Se hello [skrivning till blob storage-undantag](#storageexception) avsnittet nedan för mer information.</span><span class="sxs-lookup"><span data-stu-id="3e990-194">See hello [storage exception for write on blob](#storageexception) section below for more information.</span></span>
>
>

## <a name="graphical-clients"></a><span data-ttu-id="3e990-195">Grafisk klienter</span><span class="sxs-lookup"><span data-stu-id="3e990-195">Graphical clients</span></span>
<span data-ttu-id="3e990-196">Det finns också flera program som ger ett grafiskt gränssnitt för att arbeta med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3e990-196">There are also several applications that provide a graphical interface for working with Azure Storage.</span></span> <span data-ttu-id="3e990-197">hello nedan följer en lista över några av dessa program:</span><span class="sxs-lookup"><span data-stu-id="3e990-197">hello following is a list of a few of these applications:</span></span>

| <span data-ttu-id="3e990-198">Client</span><span class="sxs-lookup"><span data-stu-id="3e990-198">Client</span></span> | <span data-ttu-id="3e990-199">Linux</span><span class="sxs-lookup"><span data-stu-id="3e990-199">Linux</span></span> | <span data-ttu-id="3e990-200">OS X</span><span class="sxs-lookup"><span data-stu-id="3e990-200">OS X</span></span> | <span data-ttu-id="3e990-201">Windows</span><span class="sxs-lookup"><span data-stu-id="3e990-201">Windows</span></span> |
| --- |:---:|:---:|:---:|
| [<span data-ttu-id="3e990-202">Microsoft Visual Studio-verktygen för HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e990-202">Microsoft Visual Studio Tools for HDInsight</span></span>](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) |<span data-ttu-id="3e990-203">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-203">✔</span></span> |<span data-ttu-id="3e990-204">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-204">✔</span></span> |<span data-ttu-id="3e990-205">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-205">✔</span></span> |
| [<span data-ttu-id="3e990-206">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="3e990-206">Azure Storage Explorer</span></span>](http://storageexplorer.com/) |<span data-ttu-id="3e990-207">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-207">✔</span></span> |<span data-ttu-id="3e990-208">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-208">✔</span></span> |<span data-ttu-id="3e990-209">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-209">✔</span></span> |
| [<span data-ttu-id="3e990-210">Molnet lagring Studio 2</span><span class="sxs-lookup"><span data-stu-id="3e990-210">Cloud Storage Studio 2</span></span>](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |<span data-ttu-id="3e990-211">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-211">✔</span></span> |
| [<span data-ttu-id="3e990-212">CloudXplorer</span><span class="sxs-lookup"><span data-stu-id="3e990-212">CloudXplorer</span></span>](http://clumsyleaf.com/products/cloudxplorer) | | |<span data-ttu-id="3e990-213">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-213">✔</span></span> |
| [<span data-ttu-id="3e990-214">Azure Explorer</span><span class="sxs-lookup"><span data-stu-id="3e990-214">Azure Explorer</span></span>](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |<span data-ttu-id="3e990-215">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-215">✔</span></span> |
| [<span data-ttu-id="3e990-216">Cyberduck</span><span class="sxs-lookup"><span data-stu-id="3e990-216">Cyberduck</span></span>](https://cyberduck.io/) | |<span data-ttu-id="3e990-217">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-217">✔</span></span> |<span data-ttu-id="3e990-218">✔</span><span class="sxs-lookup"><span data-stu-id="3e990-218">✔</span></span> |

### <a name="visual-studio-tools-for-hdinsight"></a><span data-ttu-id="3e990-219">Visual Studio-verktygen för HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e990-219">Visual Studio Tools for HDInsight</span></span>
<span data-ttu-id="3e990-220">Mer information finns i [analysera hello länkade resurser](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).</span><span class="sxs-lookup"><span data-stu-id="3e990-220">For more information, see [Navigate hello linked resources](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).</span></span>

### <span data-ttu-id="3e990-221"><a id="storageexplorer"></a>Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="3e990-221"><a id="storageexplorer"></a>Azure Storage Explorer</span></span>
<span data-ttu-id="3e990-222">*Azure Lagringsutforskaren* är användbart för att kontrollera och ändra hello data i BLOB.</span><span class="sxs-lookup"><span data-stu-id="3e990-222">*Azure Storage Explorer* is a useful tool for inspecting and altering hello data in blobs.</span></span> <span data-ttu-id="3e990-223">Det är ett kostnadsfritt, Öppna källa verktyg som kan hämtas från [http://storageexplorer.com/](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="3e990-223">It is a free, open source tool that can be downloaded from [http://storageexplorer.com/](http://storageexplorer.com/).</span></span> <span data-ttu-id="3e990-224">hello källkoden är tillgänglig från den här länken samt.</span><span class="sxs-lookup"><span data-stu-id="3e990-224">hello source code is available from this link as well.</span></span>

<span data-ttu-id="3e990-225">Innan du använder verktyget hello, måste du känna till din Azure storage-konto och nyckel.</span><span class="sxs-lookup"><span data-stu-id="3e990-225">Before using hello tool, you must know your Azure storage account name and account key.</span></span> <span data-ttu-id="3e990-226">Anvisningar om hur du får den här informationen finns hello ”så här: visa, kopiera och generera lagring åtkomstnycklar” avsnittet av [skapa, hantera eller ta bort ett lagringskonto][azure-create-storage-account].</span><span class="sxs-lookup"><span data-stu-id="3e990-226">For instructions about getting this information, see hello "How to: View, copy and regenerate storage access keys" section of [Create, manage, or delete a storage account][azure-create-storage-account].</span></span>

1. <span data-ttu-id="3e990-227">Kör Azure Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="3e990-227">Run Azure Storage Explorer.</span></span> <span data-ttu-id="3e990-228">Om det är hello första gången du har kört hello Lagringsutforskaren, du uppmanas att hello **_Storage kontonamn** och **lagringskontonyckel**.</span><span class="sxs-lookup"><span data-stu-id="3e990-228">If this is hello first time you have run hello Storage Explorer, you will be prompted for hello **_Storage account name** and **Storage account key**.</span></span> <span data-ttu-id="3e990-229">Om du har kört den innan du använder hello **Lägg till** knappen tooadd ett nytt lagringskontonamn och nyckel.</span><span class="sxs-lookup"><span data-stu-id="3e990-229">If you have run it before, use hello **Add** button tooadd a new storage account name and key.</span></span>

    <span data-ttu-id="3e990-230">Ange hello namn och nyckel för hello storage-konto som används av ditt HDInsight-kluster och välj sedan **Spara & Öppna**.</span><span class="sxs-lookup"><span data-stu-id="3e990-230">Enter hello name and key for hello storage account used by your HDInsight cluster and then select **SAVE & OPEN**.</span></span>

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]
2. <span data-ttu-id="3e990-232">Hello namnet på hello-behållare som är kopplad till ditt HDInsight-kluster på hello listan behållare toohello kvar för hello-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="3e990-232">In hello list of containers toohello left of hello interface, click hello name of hello container that is associated with your HDInsight cluster.</span></span> <span data-ttu-id="3e990-233">Detta är hello namnet på hello HDInsight-kluster som standard men kan skilja sig om du har angett ett specifikt namn när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="3e990-233">By default, this is hello name of hello HDInsight cluster, but may be different if you entered a specific name when creating hello cluster.</span></span>
3. <span data-ttu-id="3e990-234">Välj hello överför ikonen hello verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="3e990-234">From hello tool bar, select hello upload icon.</span></span>

    ![Verktygsfält med överför ikonen markerat](./media/hdinsight-upload-data/toolbar.png)
4. <span data-ttu-id="3e990-236">Ange en fil tooupload och klicka sedan på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="3e990-236">Specify a file tooupload, and then click **Open**.</span></span> <span data-ttu-id="3e990-237">När du uppmanas, Välj **överför** tooupload hello filen toohello rot hello lagringsbehållaren.</span><span class="sxs-lookup"><span data-stu-id="3e990-237">When prompted, select **Upload** tooupload hello file toohello root of hello storage container.</span></span> <span data-ttu-id="3e990-238">Om du vill tooupload hello tooa specifika sökväg, ange hello sökväg i hello **mål** fältet och välj sedan **överför**.</span><span class="sxs-lookup"><span data-stu-id="3e990-238">If you want tooupload hello file tooa specific path, enter hello path in hello **Destination** field and then select **Upload**.</span></span>

    ![Överför fildialogruta](./media/hdinsight-upload-data/fileupload.png)

    <span data-ttu-id="3e990-240">När hello-filen har överförts, kan du använda den från jobb på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3e990-240">Once hello file has finished uploading, you can use it from jobs on hello HDInsight cluster.</span></span>

## <a name="mount-azure-blob-storage-as-local-drive"></a><span data-ttu-id="3e990-241">Montera Azure Blob Storage som lokal enhet</span><span class="sxs-lookup"><span data-stu-id="3e990-241">Mount Azure Blob Storage as Local Drive</span></span>
<span data-ttu-id="3e990-242">Se [montera Azure Blob Storage som lokala enhet](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).</span><span class="sxs-lookup"><span data-stu-id="3e990-242">See [Mount Azure Blob Storage as Local Drive](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).</span></span>

## <a name="services"></a><span data-ttu-id="3e990-243">Tjänster</span><span class="sxs-lookup"><span data-stu-id="3e990-243">Services</span></span>
### <a name="azure-data-factory"></a><span data-ttu-id="3e990-244">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="3e990-244">Azure Data Factory</span></span>
<span data-ttu-id="3e990-245">hello Azure Data Factory-tjänsten är en helt hanterad tjänst för att skapa lagring, databehandling och data movement datatjänster till produktion pipeline-effektiviserad, skalbara och tillförlitliga data.</span><span class="sxs-lookup"><span data-stu-id="3e990-245">hello Azure Data Factory service is a fully managed service for composing data storage, data processing, and data movement services into streamlined, scalable, and reliable data production pipelines.</span></span>

<span data-ttu-id="3e990-246">Azure Data Factory kan vara används toomove data till Azure Blob storage eller toocreate data pipelines som använder direkt HDInsight funktioner som Hive och svin.</span><span class="sxs-lookup"><span data-stu-id="3e990-246">Azure Data Factory can be used toomove data into Azure Blob storage, or toocreate data pipelines that directly use HDInsight features such as Hive and Pig.</span></span>

<span data-ttu-id="3e990-247">Mer information finns i hello [dokumentation för Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="3e990-247">For more information, see hello [Azure Data Factory documentation](https://azure.microsoft.com/documentation/services/data-factory/).</span></span>

### <span data-ttu-id="3e990-248"><a id="sqoop"></a>Apache Sqoop</span><span class="sxs-lookup"><span data-stu-id="3e990-248"><a id="sqoop"></a>Apache Sqoop</span></span>
<span data-ttu-id="3e990-249">Sqoop är ett verktyg tootransfer data mellan Hadoop och relationsdatabaser.</span><span class="sxs-lookup"><span data-stu-id="3e990-249">Sqoop is a tool designed tootransfer data between Hadoop and relational databases.</span></span> <span data-ttu-id="3e990-250">Du kan använda den tooimport data från ett relationella databashanteringssystem (RDBMS) som SQL Server, MySQL eller Oracle till hello Hadoop distributed file system (HDFS), transformera hello data i Hadoop med MapReduce eller Hive och sedan exportera hello data tillbaka till en RDBMS.</span><span class="sxs-lookup"><span data-stu-id="3e990-250">You can use it tooimport data from a relational database management system (RDBMS), such as SQL Server, MySQL, or Oracle into hello Hadoop distributed file system (HDFS), transform hello data in Hadoop with MapReduce or Hive, and then export hello data back into an RDBMS.</span></span>

<span data-ttu-id="3e990-251">Mer information finns i [använda Sqoop med HDInsight][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="3e990-251">For more information, see [Use Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

## <a name="development-sdks"></a><span data-ttu-id="3e990-252">SDK: er för utveckling</span><span class="sxs-lookup"><span data-stu-id="3e990-252">Development SDKs</span></span>
<span data-ttu-id="3e990-253">Azure Blob storage kan även nås med ett Azure SDK från hello följande programmeringsspråk:</span><span class="sxs-lookup"><span data-stu-id="3e990-253">Azure Blob storage can also be accessed using an Azure SDK from hello following programming languages:</span></span>

* <span data-ttu-id="3e990-254">.NET</span><span class="sxs-lookup"><span data-stu-id="3e990-254">.NET</span></span>
* <span data-ttu-id="3e990-255">Java</span><span class="sxs-lookup"><span data-stu-id="3e990-255">Java</span></span>
* <span data-ttu-id="3e990-256">Node.js</span><span class="sxs-lookup"><span data-stu-id="3e990-256">Node.js</span></span>
* <span data-ttu-id="3e990-257">PHP</span><span class="sxs-lookup"><span data-stu-id="3e990-257">PHP</span></span>
* <span data-ttu-id="3e990-258">Python</span><span class="sxs-lookup"><span data-stu-id="3e990-258">Python</span></span>
* <span data-ttu-id="3e990-259">Ruby</span><span class="sxs-lookup"><span data-stu-id="3e990-259">Ruby</span></span>

<span data-ttu-id="3e990-260">Mer information om hur du installerar hello Azure SDK finns [hämtar Azure](https://azure.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="3e990-260">For more information on installing hello Azure SDKs, see [Azure downloads](https://azure.microsoft.com/downloads/)</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="3e990-261">Felsökning</span><span class="sxs-lookup"><span data-stu-id="3e990-261">Troubleshooting</span></span>
### <span data-ttu-id="3e990-262"><a id="storageexception"></a>Skrivning till blob Storage-undantag</span><span class="sxs-lookup"><span data-stu-id="3e990-262"><a id="storageexception"></a>Storage exception for write on blob</span></span>
<span data-ttu-id="3e990-263">**Symptom**: när du använder hello `hadoop` eller `hdfs dfs` kommandon toowrite filer som är ~ 12 GB eller större på ett HBase-kluster som kan uppstå hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="3e990-263">**Symptoms**: When using hello `hadoop` or `hdfs dfs` commands toowrite files that are ~12GB or larger on an HBase cluster, you may encounter hello following error:</span></span>

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
    Caused by: com.microsoft.azure.storage.StorageException: hello request body is too large and exceeds hello maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

<span data-ttu-id="3e990-264">**Orsak**: HBase på HDInsight-kluster tooa standardblockstorleken på 256 KB när du skriver tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="3e990-264">**Cause**: HBase on HDInsight clusters default tooa block size of 256KB when writing tooAzure storage.</span></span> <span data-ttu-id="3e990-265">När det här fungerar för HBase APIs eller REST API: er det kommer att resultera i ett fel när du använder hello `hadoop` eller `hdfs dfs` kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="3e990-265">While this works for HBase APIs or REST APIs, it will result in an error when using hello `hadoop` or `hdfs dfs` command-line utilities.</span></span>

<span data-ttu-id="3e990-266">**Lösning**: Använd `fs.azure.write.request.size` toospecify en större blockstorlek.</span><span class="sxs-lookup"><span data-stu-id="3e990-266">**Resolution**: Use `fs.azure.write.request.size` toospecify a larger block size.</span></span> <span data-ttu-id="3e990-267">Du kan göra detta på grundval av per tillfälle genom att använda hello `-D` parameter.</span><span class="sxs-lookup"><span data-stu-id="3e990-267">You can do this on a per-use basis by using hello `-D` parameter.</span></span> <span data-ttu-id="3e990-268">hello följande är ett exempel som använder den här parametern med hello `hadoop` kommando:</span><span class="sxs-lookup"><span data-stu-id="3e990-268">hello following is an example using this parameter with hello `hadoop` command:</span></span>

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

<span data-ttu-id="3e990-269">Du kan också öka hello värdet för `fs.azure.write.request.size` globalt med Ambari.</span><span class="sxs-lookup"><span data-stu-id="3e990-269">You can also increase hello value of `fs.azure.write.request.size` globally by using Ambari.</span></span> <span data-ttu-id="3e990-270">hello följande steg kan vara används toochange hello värdet i hello Ambari-Webbgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="3e990-270">hello following steps can be used toochange hello value in hello Ambari Web UI:</span></span>

1. <span data-ttu-id="3e990-271">Gå toohello Ambari-Webbgränssnittet för klustret i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="3e990-271">In your browser, go toohello Ambari Web UI for your cluster.</span></span> <span data-ttu-id="3e990-272">Detta är https://CLUSTERNAME.azurehdinsight.net, där **KLUSTERNAMN** är hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="3e990-272">This is https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is hello name of your cluster.</span></span>

    <span data-ttu-id="3e990-273">När du uppmanas ange hello admin namn och lösenord för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3e990-273">When prompted, enter hello admin name and password for hello cluster.</span></span>
2. <span data-ttu-id="3e990-274">Hello vänster sida av hello-skärmen, Välj **HDFS**, och välj sedan hello **konfigurationerna** fliken.</span><span class="sxs-lookup"><span data-stu-id="3e990-274">From hello left side of hello screen, select **HDFS**, and then select hello **Configs** tab.</span></span>
3. <span data-ttu-id="3e990-275">I hello **Filter...**  anger `fs.azure.write.request.size`.</span><span class="sxs-lookup"><span data-stu-id="3e990-275">In hello **Filter...** field, enter `fs.azure.write.request.size`.</span></span> <span data-ttu-id="3e990-276">Visas hello fältet och det aktuella värdet hello mitten av hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="3e990-276">This will display hello field and current value in hello middle of hello page.</span></span>
4. <span data-ttu-id="3e990-277">Ändra hello värdet från 262144 (256KB) toohello nytt värde.</span><span class="sxs-lookup"><span data-stu-id="3e990-277">Change hello value from 262144 (256KB) toohello new value.</span></span> <span data-ttu-id="3e990-278">Till exempel 4194304 (4MB).</span><span class="sxs-lookup"><span data-stu-id="3e990-278">For example, 4194304 (4MB).</span></span>

![Bild av ändra hello-värdet via Ambari-Webbgränssnittet](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

<span data-ttu-id="3e990-280">Läs mer om hur du använder Ambari [hantera HDInsight-kluster med Ambari-Webbgränssnittet för hello](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="3e990-280">For more information on using Ambari, see [Manage HDInsight clusters using hello Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e990-281">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e990-281">Next steps</span></span>
<span data-ttu-id="3e990-282">Nu när du förstår hur tooget data till HDInsight, läsa hello följande artiklar toolearn hur tooperform analys:</span><span class="sxs-lookup"><span data-stu-id="3e990-282">Now that you understand how tooget data into HDInsight, read hello following articles toolearn how tooperform analysis:</span></span>

* <span data-ttu-id="3e990-283">[Kom igång med Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="3e990-283">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="3e990-284">[Skicka Hadoop-jobb via programmering][hdinsight-submit-jobs]</span><span class="sxs-lookup"><span data-stu-id="3e990-284">[Submit Hadoop jobs programmatically][hdinsight-submit-jobs]</span></span>
* <span data-ttu-id="3e990-285">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="3e990-285">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="3e990-286">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="3e990-286">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>

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
