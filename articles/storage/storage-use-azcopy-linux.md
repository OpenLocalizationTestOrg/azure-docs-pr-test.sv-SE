---
title: "Kopiera eller flytta data till Azure Storage med AzCopy på Linux | Microsoft Docs"
description: "Använd AzCopy på Linux-verktyget för att flytta eller kopiera data till och från blob- och innehåll. Kopiera data till Azure Storage från lokala filer eller kopiera data inom eller mellan lagringskonton. Enkelt migrera dina data till Azure Storage."
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: aa155738-7c69-4a83-94f8-b97af4461274
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: seguler
ms.openlocfilehash: d17f63dcee590529756d48d699f78b3fb30f973c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="transfer-data-with-azcopy-on-linux"></a><span data-ttu-id="15071-105">Överföra data med AzCopy på Linux</span><span class="sxs-lookup"><span data-stu-id="15071-105">Transfer data with AzCopy on Linux</span></span>
<span data-ttu-id="15071-106">AzCopy på Linux är ett kommandoradsverktyg som utformats för att kopiera data till och från Microsoft Azure-Blob och fillagring med enkla kommandon med optimal prestanda.</span><span class="sxs-lookup"><span data-stu-id="15071-106">AzCopy on Linux is a command-line utility designed for copying data to and from Microsoft Azure Blob and File storage using simple commands with optimal performance.</span></span> <span data-ttu-id="15071-107">Du kan kopiera data från ett objekt till en annan inom ditt lagringskonto eller mellan lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="15071-107">You can copy data from one object to another within your storage account, or between storage accounts.</span></span>

<span data-ttu-id="15071-108">Det finns två versioner av AzCopy som du kan hämta.</span><span class="sxs-lookup"><span data-stu-id="15071-108">There are two versions of AzCopy that you can download.</span></span> <span data-ttu-id="15071-109">AzCopy på Linux är byggd med .NET Core Framework, som riktar sig till Linux-plattformar som erbjuder POSIX format kommandoradsalternativ.</span><span class="sxs-lookup"><span data-stu-id="15071-109">AzCopy on Linux is built with .NET Core Framework, which targets Linux platforms offering POSIX style command-line options.</span></span> <span data-ttu-id="15071-110">[AzCopy på Windows](storage-use-azcopy.md) har skapats med .NET Framework och erbjuder kommandoradsalternativ för Windows-formatet.</span><span class="sxs-lookup"><span data-stu-id="15071-110">[AzCopy on Windows](storage-use-azcopy.md) is built with .NET Framework, and offers Windows style command-line options.</span></span> <span data-ttu-id="15071-111">Den här artikeln beskriver AzCopy på Linux.</span><span class="sxs-lookup"><span data-stu-id="15071-111">This article covers AzCopy on Linux.</span></span>

## <a name="download-and-install-azcopy"></a><span data-ttu-id="15071-112">Hämta och installera AzCopy</span><span class="sxs-lookup"><span data-stu-id="15071-112">Download and install AzCopy</span></span>
### <a name="installation-on-linux"></a><span data-ttu-id="15071-113">Installation på Linux</span><span class="sxs-lookup"><span data-stu-id="15071-113">Installation on Linux</span></span>

<span data-ttu-id="15071-114">AzCopy på Linux kräver .NET Core framework på plattformen.</span><span class="sxs-lookup"><span data-stu-id="15071-114">AzCopy on Linux requires .NET Core framework on the platform.</span></span> <span data-ttu-id="15071-115">Finns i installationsanvisningarna på den [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) sidan.</span><span class="sxs-lookup"><span data-stu-id="15071-115">See the installation instructions on the [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) page.</span></span>

<span data-ttu-id="15071-116">Exempelvis kan vi installera .NET Core på Ubuntu 16,10.</span><span class="sxs-lookup"><span data-stu-id="15071-116">As an example, let's install .NET Core on Ubuntu 16.10.</span></span> <span data-ttu-id="15071-117">Senaste installationsguiden finns [.NET Core på Linux](https://www.microsoft.com/net/core#linuxubuntu) installationssidan.</span><span class="sxs-lookup"><span data-stu-id="15071-117">For the latest installation guide, visit [.NET Core on Linux](https://www.microsoft.com/net/core#linuxubuntu) installation page.</span></span>


```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.3
```

<span data-ttu-id="15071-118">När du har installerat .NET Core, hämta och installera AzCopy.</span><span class="sxs-lookup"><span data-stu-id="15071-118">Once you have installed .NET Core, download and install AzCopy.</span></span>

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

<span data-ttu-id="15071-119">Du kan ta bort de extraherade filerna när AzCopy på Linux har installerats.</span><span class="sxs-lookup"><span data-stu-id="15071-119">You can remove the extracted files once AzCopy on Linux is installed.</span></span> <span data-ttu-id="15071-120">Du kan också om du inte har superanvändare, kan du också köra AzCopy med kommandoskript 'azcopy' i den extraherade mappen.</span><span class="sxs-lookup"><span data-stu-id="15071-120">Alternatively if you do not have superuser privileges, you can also run AzCopy using the shell script 'azcopy' in the extracted folder.</span></span> 

### <a name="alternative-installation-on-ubuntu"></a><span data-ttu-id="15071-121">Alternativ Installation på Ubuntu</span><span class="sxs-lookup"><span data-stu-id="15071-121">Alternative Installation on Ubuntu</span></span>

<span data-ttu-id="15071-122">**Ubuntu 14.04**</span><span class="sxs-lookup"><span data-stu-id="15071-122">**Ubuntu 14.04**</span></span>

<span data-ttu-id="15071-123">Lägg till lgh källa för .net Core:</span><span class="sxs-lookup"><span data-stu-id="15071-123">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="15071-124">Lägg till lgh källa för Microsoft Linux produkten lagringsplatsen och installera AzCopy:</span><span class="sxs-lookup"><span data-stu-id="15071-124">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

```bash
curl https://packages.microsoft.com/config/ubuntu/14.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

<span data-ttu-id="15071-125">**Ubuntu 16.04**</span><span class="sxs-lookup"><span data-stu-id="15071-125">**Ubuntu 16.04**</span></span>

<span data-ttu-id="15071-126">Lägg till lgh källa för .net Core:</span><span class="sxs-lookup"><span data-stu-id="15071-126">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="15071-127">Lägg till lgh källa för Microsoft Linux produkten lagringsplatsen och installera AzCopy:</span><span class="sxs-lookup"><span data-stu-id="15071-127">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

```bash
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

<span data-ttu-id="15071-128">**Ubuntu 16,10**</span><span class="sxs-lookup"><span data-stu-id="15071-128">**Ubuntu 16.10**</span></span>

<span data-ttu-id="15071-129">Lägg till lgh källa för .net Core:</span><span class="sxs-lookup"><span data-stu-id="15071-129">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="15071-130">Lägg till lgh källa för Microsoft Linux produkten lagringsplatsen och installera AzCopy:</span><span class="sxs-lookup"><span data-stu-id="15071-130">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

```bash
curl https://packages.microsoft.com/config/ubuntu/16.10/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

## <a name="writing-your-first-azcopy-command"></a><span data-ttu-id="15071-131">Skriva ditt första AzCopy-kommandot</span><span class="sxs-lookup"><span data-stu-id="15071-131">Writing your first AzCopy command</span></span>
<span data-ttu-id="15071-132">Den grundläggande syntaxen för AzCopy kommandon är:</span><span class="sxs-lookup"><span data-stu-id="15071-132">The basic syntax for AzCopy commands is:</span></span>

```azcopy
azcopy --source <source> --destination <destination> [Options]
```

<span data-ttu-id="15071-133">Följande exempel visar olika scenarier för att kopiera data till och från Microsoft Azure-BLOB och filer.</span><span class="sxs-lookup"><span data-stu-id="15071-133">The following examples demonstrate various scenarios for copying data to and from Microsoft Azure Blobs and Files.</span></span> <span data-ttu-id="15071-134">Referera till den `azcopy --help` menyn för en detaljerad förklaring av de parametrar som används i varje prov.</span><span class="sxs-lookup"><span data-stu-id="15071-134">Refer to the `azcopy --help` menu for a detailed explanation of the parameters used in each sample.</span></span>

## <a name="blob-download"></a><span data-ttu-id="15071-135">BLOB: ladda ned</span><span class="sxs-lookup"><span data-stu-id="15071-135">Blob: Download</span></span>
### <a name="download-single-blob"></a><span data-ttu-id="15071-136">Hämta en enda blob</span><span class="sxs-lookup"><span data-stu-id="15071-136">Download single blob</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="15071-137">Om mappen `/mnt/myfiles` finns inte, AzCopy skapar den och hämtar `abc.txt ` till den nya mappen.</span><span class="sxs-lookup"><span data-stu-id="15071-137">If the folder `/mnt/myfiles` does not exist, AzCopy creates it and downloads `abc.txt ` into the new folder.</span></span>

### <a name="download-single-blob-from-secondary-region"></a><span data-ttu-id="15071-138">Hämta en enda blob från sekundär region</span><span class="sxs-lookup"><span data-stu-id="15071-138">Download single blob from secondary region</span></span>

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="15071-139">Observera att du måste ha läsbehörighet geo-redundant lagring aktiverad.</span><span class="sxs-lookup"><span data-stu-id="15071-139">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="download-all-blobs"></a><span data-ttu-id="15071-140">Hämta alla BLOB</span><span class="sxs-lookup"><span data-stu-id="15071-140">Download all blobs</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

<span data-ttu-id="15071-141">Anta följande blobbar finns i den angivna behållaren:</span><span class="sxs-lookup"><span data-stu-id="15071-141">Assume the following blobs reside in the specified container:</span></span>  

```
abc.txt
abc1.txt
abc2.txt
vd1/a.txt
vd1/abcd.txt
```

<span data-ttu-id="15071-142">När hämtningen katalogen `/mnt/myfiles` innehåller följande filer:</span><span class="sxs-lookup"><span data-stu-id="15071-142">After the download operation, the directory `/mnt/myfiles` includes the following files:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/vd1/a.txt
/mnt/myfiles/vd1/abcd.txt
```

<span data-ttu-id="15071-143">Om du inte anger alternativet `--recursive`, ingen blob som ska hämtas.</span><span class="sxs-lookup"><span data-stu-id="15071-143">If you do not specify option `--recursive`, no blob will be downloaded.</span></span>

### <a name="download-blobs-with-specified-prefix"></a><span data-ttu-id="15071-144">Ladda ned blobbar med angivna prefix</span><span class="sxs-lookup"><span data-stu-id="15071-144">Download blobs with specified prefix</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "a" \
    --recursive
```

<span data-ttu-id="15071-145">Anta att följande blobbar finns i den angivna behållaren.</span><span class="sxs-lookup"><span data-stu-id="15071-145">Assume the following blobs reside in the specified container.</span></span> <span data-ttu-id="15071-146">Alla blobbar som börjar med prefixet `a` hämtas.</span><span class="sxs-lookup"><span data-stu-id="15071-146">All blobs beginning with the prefix `a` are downloaded.</span></span>

```
abc.txt
abc1.txt
abc2.txt
xyz.txt
vd1\a.txt
vd1\abcd.txt
```

<span data-ttu-id="15071-147">När hämtningen mappen `/mnt/myfiles` innehåller följande filer:</span><span class="sxs-lookup"><span data-stu-id="15071-147">After the download operation, the folder `/mnt/myfiles` includes the following files:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
```

<span data-ttu-id="15071-148">Prefixet som gäller för den virtuella katalogen, som utgör den första delen av blobbnamnet.</span><span class="sxs-lookup"><span data-stu-id="15071-148">The prefix applies to the virtual directory, which forms the first part of the blob name.</span></span> <span data-ttu-id="15071-149">I exemplet ovan, matchar den virtuella katalogen inte det angivna prefixet, så inga blob hämtas.</span><span class="sxs-lookup"><span data-stu-id="15071-149">In the example shown above, the virtual directory does not match the specified prefix, so no blob is downloaded.</span></span> <span data-ttu-id="15071-150">Dessutom, om alternativet `--recursive` anges AzCopy inte hämta alla blobbar.</span><span class="sxs-lookup"><span data-stu-id="15071-150">In addition, if the option `--recursive` is not specified, AzCopy does not download any blobs.</span></span>

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a><span data-ttu-id="15071-151">Ange last-modified-tid med exporterade filer som ska vara densamma som källan BLOB</span><span class="sxs-lookup"><span data-stu-id="15071-151">Set the last-modified time of exported files to be same as the source blobs</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination "/mnt/myfiles" \
    --source-key <key> \
    --preserve-last-modified-time
```

<span data-ttu-id="15071-152">Du kan också utesluta blobbar från hämtningen baserat på deras tid för senaste ändring.</span><span class="sxs-lookup"><span data-stu-id="15071-152">You can also exclude blobs from the download operation based on their last-modified time.</span></span> <span data-ttu-id="15071-153">Till exempel om du vill undanta blobbar vars senast ändrad är samma som eller nyare än målfilen och lägga till den `--exclude-newer` alternativ:</span><span class="sxs-lookup"><span data-stu-id="15071-153">For example, if you want to exclude blobs whose last modified time is the same or newer than the destination file, add the `--exclude-newer` option:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-newer
```

<span data-ttu-id="15071-154">Eller om du vill undanta blobbar vars senast ändrad är samma eller äldre än målfilen och lägga till den `--exclude-older` alternativ:</span><span class="sxs-lookup"><span data-stu-id="15071-154">Or if you want to exclude blobs whose last modified time is the same or older than the destination file, add the `--exclude-older` option:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-older
```

## <a name="blob-upload"></a><span data-ttu-id="15071-155">BLOB: Överför</span><span class="sxs-lookup"><span data-stu-id="15071-155">Blob: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="15071-156">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="15071-156">Upload single file</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="15071-157">Om den angivna Målbehållaren inte finns, skapar den AzCopy och överför filen till den.</span><span class="sxs-lookup"><span data-stu-id="15071-157">If the specified destination container does not exist, AzCopy creates it and uploads the file into it.</span></span>

### <a name="upload-single-file-to-virtual-directory"></a><span data-ttu-id="15071-158">Överför en fil till virtuell katalog</span><span class="sxs-lookup"><span data-stu-id="15071-158">Upload single file to virtual directory</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="15071-159">Om den angivna virtuella katalogen inte finns, AzCopy överför filen så att den virtuella katalogen i blob-namnet (*t.ex.*, `vd/abc.txt` i exemplet ovan).</span><span class="sxs-lookup"><span data-stu-id="15071-159">If the specified virtual directory does not exist, AzCopy uploads the file to include the virtual directory in the blob name (*e.g.*, `vd/abc.txt` in the example above).</span></span>

### <a name="upload-all-files"></a><span data-ttu-id="15071-160">Ladda upp alla filer</span><span class="sxs-lookup"><span data-stu-id="15071-160">Upload all files</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive
```

<span data-ttu-id="15071-161">Om du anger alternativet `--recursive` Överför innehållet i den angivna katalogen till Blob storage rekursivt, vilket innebär att alla undermappar och filer överförs också.</span><span class="sxs-lookup"><span data-stu-id="15071-161">Specifying option `--recursive` uploads the contents of the specified directory to Blob storage recursively, meaning that all subfolders and their files are uploaded as well.</span></span> <span data-ttu-id="15071-162">Anta exempelvis att följande filer finns i mappen `/mnt/myfiles`:</span><span class="sxs-lookup"><span data-stu-id="15071-162">For instance, assume the following files reside in folder `/mnt/myfiles`:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

<span data-ttu-id="15071-163">När överföringen innehåller behållaren följande filer:</span><span class="sxs-lookup"><span data-stu-id="15071-163">After the upload operation, the container includes the following files:</span></span>

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

<span data-ttu-id="15071-164">När alternativet `--recursive` anges endast följande tre filer överförs:</span><span class="sxs-lookup"><span data-stu-id="15071-164">When the option `--recursive` is not specified, only the following three files are uploaded:</span></span>

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="15071-165">Överföra filer som matchar angivna mönstret</span><span class="sxs-lookup"><span data-stu-id="15071-165">Upload files matching specified pattern</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "a*" \
    --recursive
```

<span data-ttu-id="15071-166">Anta att följande filer finns i mappen `/mnt/myfiles`:</span><span class="sxs-lookup"><span data-stu-id="15071-166">Assume the following files reside in folder `/mnt/myfiles`:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/xyz.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

<span data-ttu-id="15071-167">När överföringen innehåller behållaren följande filer:</span><span class="sxs-lookup"><span data-stu-id="15071-167">After the upload operation, the container includes the following files:</span></span>

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

<span data-ttu-id="15071-168">När alternativet `--recursive` anges AzCopy hoppar över filer som finns i underkataloger:</span><span class="sxs-lookup"><span data-stu-id="15071-168">When the option `--recursive` is not specified, AzCopy skips files that are in sub-directories:</span></span>

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a><span data-ttu-id="15071-169">Ange MIME content-type för en mål-blob</span><span class="sxs-lookup"><span data-stu-id="15071-169">Specify the MIME content type of a destination blob</span></span>
<span data-ttu-id="15071-170">Som standard anges AzCopy innehållstypen för en mål-blob till `application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="15071-170">By default, AzCopy sets the content type of a destination blob to `application/octet-stream`.</span></span> <span data-ttu-id="15071-171">Du kan dock uttryckligen ange content-type via alternativet `--set-content-type [content-type]`.</span><span class="sxs-lookup"><span data-stu-id="15071-171">However, you can explicitly specify the content type via the option `--set-content-type [content-type]`.</span></span> <span data-ttu-id="15071-172">Den här syntaxen anger content-type för alla blobbar i en överföringen.</span><span class="sxs-lookup"><span data-stu-id="15071-172">This syntax sets the content type for all blobs in an upload operation.</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type "video/mp4"
```

<span data-ttu-id="15071-173">Om alternativet `--set-content-type` anges utan värde, och sedan AzCopy anger varje blob eller filens innehållstyp enligt dess filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="15071-173">If the option `--set-content-type` is specified without a value, then AzCopy sets each blob or file's content type according to its file extension.</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type
```

## <a name="blob-copy"></a><span data-ttu-id="15071-174">BLOB: kopiera</span><span class="sxs-lookup"><span data-stu-id="15071-174">Blob: Copy</span></span>
### <a name="copy-single-blob-within-storage-account"></a><span data-ttu-id="15071-175">Kopiera enda blob inom lagringskonto</span><span class="sxs-lookup"><span data-stu-id="15071-175">Copy single blob within Storage account</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key> \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="15071-176">När du kopierar en blobb utan--synkroniserad kopia alternativet en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="15071-176">When you copy a blob without --sync-copy option, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-across-storage-accounts"></a><span data-ttu-id="15071-177">Kopiera enda blob på Storage-konton</span><span class="sxs-lookup"><span data-stu-id="15071-177">Copy single blob across Storage accounts</span></span>

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

<span data-ttu-id="15071-178">När du kopierar en blobb utan--synkroniserad kopia alternativet en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="15071-178">When you copy a blob without --sync-copy option, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a><span data-ttu-id="15071-179">Kopiera enda blob från sekundär region till primär region</span><span class="sxs-lookup"><span data-stu-id="15071-179">Copy single blob from secondary region to primary region</span></span>

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

<span data-ttu-id="15071-180">Observera att du måste ha läsbehörighet geo-redundant lagring aktiverad.</span><span class="sxs-lookup"><span data-stu-id="15071-180">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a><span data-ttu-id="15071-181">Kopiera enda blob och dess ögonblicksbilder på Storage-konton</span><span class="sxs-lookup"><span data-stu-id="15071-181">Copy single blob and its snapshots across Storage accounts</span></span>

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --include-snapshot
```

<span data-ttu-id="15071-182">Efter kopieringen innehåller Målbehållaren blob och dess ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="15071-182">After the copy operation, the target container includes the blob and its snapshots.</span></span> <span data-ttu-id="15071-183">Behållaren innehåller följande blob och dess ögonblicksbilder:</span><span class="sxs-lookup"><span data-stu-id="15071-183">The container includes the following blob and its snapshots:</span></span>

```
abc.txt
abc (2013-02-25 080757).txt
abc (2014-02-21 150331).txt
```

### <a name="synchronously-copy-blobs-across-storage-accounts"></a><span data-ttu-id="15071-184">Kopiera synkront blobar på lagringskonton</span><span class="sxs-lookup"><span data-stu-id="15071-184">Synchronously copy blobs across Storage accounts</span></span>
<span data-ttu-id="15071-185">AzCopy som standard kopierar data mellan två slutpunkter för lagring asynkront.</span><span class="sxs-lookup"><span data-stu-id="15071-185">AzCopy by default copies data between two storage endpoints asynchronously.</span></span> <span data-ttu-id="15071-186">Kopiera åtgärden körs i bakgrunden med hjälp av ledig bandbreddskapacitet som har inga SLA vad gäller hur snabbt en blob kopieras.</span><span class="sxs-lookup"><span data-stu-id="15071-186">Therefore, the copy operation runs in the background using spare bandwidth capacity that has no SLA in terms of how fast a blob is copied.</span></span> 

<span data-ttu-id="15071-187">Den `--sync-copy` alternativet ser du till att kopieringen hämtar konsekvent hastighet.</span><span class="sxs-lookup"><span data-stu-id="15071-187">The `--sync-copy` option ensures that the copy operation gets consistent speed.</span></span> <span data-ttu-id="15071-188">AzCopy utför synkron kopian genom att hämta blobbarna att kopiera från den angivna källan till lokalt minne och överför dem till Blob-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="15071-188">AzCopy performs the synchronous copy by downloading the blobs to copy from the specified source to local memory, and then uploading them to the Blob storage destination.</span></span>

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "ab" \
    --sync-copy
```

<span data-ttu-id="15071-189">`--sync-copy`kan skapa ytterligare utgång kostnaden jämfört med asynkron kopia.</span><span class="sxs-lookup"><span data-stu-id="15071-189">`--sync-copy` might generate additional egress cost compared to asynchronous copy.</span></span> <span data-ttu-id="15071-190">Den rekommenderade metoden är att använda det här alternativet i en Azure VM, som finns i samma region som ditt källa storage-konto för att undvika kostnader för utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="15071-190">The recommended approach is to use this option in an Azure VM, that is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="file-download"></a><span data-ttu-id="15071-191">Fil: ladda ned</span><span class="sxs-lookup"><span data-stu-id="15071-191">File: Download</span></span>
### <a name="download-single-file"></a><span data-ttu-id="15071-192">Hämta en fil</span><span class="sxs-lookup"><span data-stu-id="15071-192">Download single file</span></span>

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="15071-193">Om den angivna källan är en Azure-filresurs och sedan måste du antingen ange det exakta filnamnet (*t.ex.* `abc.txt`) att hämta en fil eller ange alternativet `--recursive` att ladda ned alla filer i resursen rekursivt.</span><span class="sxs-lookup"><span data-stu-id="15071-193">If the specified source is an Azure file share, then you must either specify the exact file name, (*e.g.* `abc.txt`) to download a single file, or specify option `--recursive` to download all files in the share recursively.</span></span> <span data-ttu-id="15071-194">Försök att ange både filmönstret och alternativet `--recursive` tillsammans resulterar i ett fel.</span><span class="sxs-lookup"><span data-stu-id="15071-194">Attempting to specify both a file pattern and option `--recursive` together results in an error.</span></span>

### <a name="download-all-files"></a><span data-ttu-id="15071-195">Hämta alla filer</span><span class="sxs-lookup"><span data-stu-id="15071-195">Download all files</span></span>

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

<span data-ttu-id="15071-196">Observera att alla tomma mappar inte laddas ned.</span><span class="sxs-lookup"><span data-stu-id="15071-196">Note that any empty folders are not downloaded.</span></span>

## <a name="file-upload"></a><span data-ttu-id="15071-197">Fil: Överför</span><span class="sxs-lookup"><span data-stu-id="15071-197">File: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="15071-198">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="15071-198">Upload single file</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include abc.txt
```

### <a name="upload-all-files"></a><span data-ttu-id="15071-199">Ladda upp alla filer</span><span class="sxs-lookup"><span data-stu-id="15071-199">Upload all files</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --recursive
```

<span data-ttu-id="15071-200">Observera att alla tomma mappar inte upp.</span><span class="sxs-lookup"><span data-stu-id="15071-200">Note that any empty folders are not uploaded.</span></span>

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="15071-201">Överföra filer som matchar angivna mönstret</span><span class="sxs-lookup"><span data-stu-id="15071-201">Upload files matching specified pattern</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include "ab*" \
    --recursive
```

## <a name="file-copy"></a><span data-ttu-id="15071-202">Fil: kopiera</span><span class="sxs-lookup"><span data-stu-id="15071-202">File: Copy</span></span>
### <a name="copy-across-file-shares"></a><span data-ttu-id="15071-203">Kopiera på filresurser</span><span class="sxs-lookup"><span data-stu-id="15071-203">Copy across file shares</span></span>

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="15071-204">När du kopierar en fil på filresurser, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="15071-204">When you copy a file across file shares, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-file-share-to-blob"></a><span data-ttu-id="15071-205">Kopiera från filresursen till blob</span><span class="sxs-lookup"><span data-stu-id="15071-205">Copy from file share to blob</span></span>

```azcopy
azcopy \ 
    --source https://myaccount1.file.core.windows.net/myfileshare/ \
    --destination https://myaccount2.blob.core.windows.net/mycontainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="15071-206">När du kopierar en fil från filresursen till blob, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="15071-206">When you copy a file from file share to blob, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-blob-to-file-share"></a><span data-ttu-id="15071-207">Kopiera från blob till filresurs</span><span class="sxs-lookup"><span data-stu-id="15071-207">Copy from blob to file share</span></span>

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/mycontainer/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="15071-208">När du kopierar en fil från blob till filresurs, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="15071-208">When you copy a file from blob to file share, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="synchronously-copy-files"></a><span data-ttu-id="15071-209">Synkront kopiera filer</span><span class="sxs-lookup"><span data-stu-id="15071-209">Synchronously copy files</span></span>
<span data-ttu-id="15071-210">Du kan ange den `--sync-copy` alternativet för att kopiera data från lagring av filer till File Storage, från fillagring till Blob Storage och från Blob Storage till File Storage synkront.</span><span class="sxs-lookup"><span data-stu-id="15071-210">You can specify the `--sync-copy` option to copy data from File Storage to File Storage, from File Storage to Blob Storage and from Blob Storage to File Storage synchronously.</span></span> <span data-ttu-id="15071-211">AzCopy körs åtgärden genom att hämta källdata till lokalt minne och överföra den till målet.</span><span class="sxs-lookup"><span data-stu-id="15071-211">AzCopy runs this operation by downloading the source data to local memory, and then uploading it to destination.</span></span> <span data-ttu-id="15071-212">I det här fallet gäller standard utgång kostnaden.</span><span class="sxs-lookup"><span data-stu-id="15071-212">In this case, standard egress cost applies.</span></span>

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive \
    --sync-copy
```

<span data-ttu-id="15071-213">När du kopierar från fillagring till Blob Storage blob standardtypen är blockblob, kan användaren ange alternativet `/BlobType:page` att ändra typen av mål-blob.</span><span class="sxs-lookup"><span data-stu-id="15071-213">When copying from File Storage to Blob Storage, the default blob type is block blob, user can specify option `/BlobType:page` to change the destination blob type.</span></span>

<span data-ttu-id="15071-214">Observera att `--sync-copy` kan skapa ytterligare utgång kostnad jämföra den asynkrona kopia.</span><span class="sxs-lookup"><span data-stu-id="15071-214">Note that `--sync-copy` might generate additional egress cost comparing to asynchronous copy.</span></span> <span data-ttu-id="15071-215">Den rekommenderade metoden är att använda det här alternativet i en Azure VM, som finns i samma region som ditt källa storage-konto för att undvika kostnader för utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="15071-215">The recommended approach is to use this option in an Azure VM, that is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="other-azcopy-features"></a><span data-ttu-id="15071-216">Andra AzCopy-funktioner</span><span class="sxs-lookup"><span data-stu-id="15071-216">Other AzCopy features</span></span>
### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a><span data-ttu-id="15071-217">Endast kopiera data som inte finns i målet</span><span class="sxs-lookup"><span data-stu-id="15071-217">Only copy data that doesn't exist in the destination</span></span>
<span data-ttu-id="15071-218">Den `--exclude-older` och `--exclude-newer` parametrar kan du exkludera resurser som tidigare eller senare från att kopieras respektive.</span><span class="sxs-lookup"><span data-stu-id="15071-218">The `--exclude-older` and `--exclude-newer` parameters allow you to exclude older or newer source resources from being copied, respectively.</span></span> <span data-ttu-id="15071-219">Om du bara vill kopiera käll-resurser som inte finns i målet kan du ange båda parametrarna i AzCopy-kommandot:</span><span class="sxs-lookup"><span data-stu-id="15071-219">If you only want to copy source resources that don't exist in the destination, you can specify both parameters in the AzCopy command:</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --exclude-older --exclude-newer

    --source /mnt/myfiles --destination http://myaccount.file.core.windows.net/myfileshare --dest-key <destkey> --recursive --exclude-older --exclude-newer

    --source http://myaccount.blob.core.windows.net/mycontainer --destination http://myaccount.blob.core.windows.net/mycontainer1 --source-key <sourcekey> --dest-key <destkey> --recursive --exclude-older --exclude-newer

### <a name="use-a-configuration-file-to-specify-command-line-parameters"></a><span data-ttu-id="15071-220">Använd en konfigurationsfil för att ange kommandoradsparametrar</span><span class="sxs-lookup"><span data-stu-id="15071-220">Use a configuration file to specify command-line parameters</span></span>

```azcopy
azcopy --config-file "azcopy-config.ini"
```

<span data-ttu-id="15071-221">Du kan inkludera eventuella kommandoradsparametrar för AzCopy i en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="15071-221">You can include any AzCopy command-line parameters in a configuration file.</span></span> <span data-ttu-id="15071-222">AzCopy bearbetar parametrarna i filen som om de hade angetts på kommandoraden, utför en direkt ersättning med innehållet i filen.</span><span class="sxs-lookup"><span data-stu-id="15071-222">AzCopy processes the parameters in the file as if they had been specified on the command line, performing a direct substitution with the contents of the file.</span></span>

<span data-ttu-id="15071-223">Anta en fil med namnet `copyoperation`, som innehåller följande rader.</span><span class="sxs-lookup"><span data-stu-id="15071-223">Assume a configuration file named `copyoperation`, that contains the following lines.</span></span> <span data-ttu-id="15071-224">Varje AzCopy-parameter kan anges på en enda rad.</span><span class="sxs-lookup"><span data-stu-id="15071-224">Each AzCopy parameter can be specified on a single line.</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --quiet

<span data-ttu-id="15071-225">eller på separata rader:</span><span class="sxs-lookup"><span data-stu-id="15071-225">or on separate lines:</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer
    --destination /mnt/myfiles
    --source-key<sourcekey>
    --recursive
    --quiet

<span data-ttu-id="15071-226">AzCopy misslyckas om du vill dela parametern på två rader som visas här för den `--source-key` parameter:</span><span class="sxs-lookup"><span data-stu-id="15071-226">AzCopy fails if you split the parameter across two lines, as shown here for the `--source-key` parameter:</span></span>

    http://myaccount.blob.core.windows.net/mycontainer
    /mnt/myfiles
    --sourcekey
    <sourcekey>
    --recursive
    --quiet

### <a name="specify-a-shared-access-signature-sas"></a><span data-ttu-id="15071-227">Ange en signatur för delad åtkomst (SAS)</span><span class="sxs-lookup"><span data-stu-id="15071-227">Specify a shared access signature (SAS)</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-sas <SAS1> \
    --dest-sas <SAS2> \
    --include abc.txt
```

<span data-ttu-id="15071-228">Du kan också ange en SAS för URI-behållaren:</span><span class="sxs-lookup"><span data-stu-id="15071-228">You can also specify a SAS on the container URI:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken \
    --destination /mnt/myfiles \
    --recursive
```

<span data-ttu-id="15071-229">Observera att AzCopy stöder för närvarande endast den [kontots SAS](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="15071-229">Note that AzCopy currently only supports the [Account SAS](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).</span></span>

### <a name="journal-file-folder"></a><span data-ttu-id="15071-230">Ändringsjournalen mapp</span><span class="sxs-lookup"><span data-stu-id="15071-230">Journal file folder</span></span>
<span data-ttu-id="15071-231">Varje gång du utfärda ett kommando till AzCopy, kontrollerar den om en journal-fil finns i standardmappen eller om den finns i en mapp som du har angett via det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="15071-231">Each time you issue a command to AzCopy, it checks whether a journal file exists in the default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="15071-232">Om journalfilen inte finns på någon plats, behandlar åtgärden som nya AzCopy och genererar en ny journalfil.</span><span class="sxs-lookup"><span data-stu-id="15071-232">If the journal file does not exist in either place, AzCopy treats the operation as new and generates a new journal file.</span></span>

<span data-ttu-id="15071-233">Om journalfilen finns kontrollerar AzCopy om den kommandorad som du matar in matchar kommandoraden i journal-fil.</span><span class="sxs-lookup"><span data-stu-id="15071-233">If the journal file does exist, AzCopy checks whether the command line that you input matches the command line in the journal file.</span></span> <span data-ttu-id="15071-234">Om två kommandorader matchar återupptar AzCopy ofullständiga igen.</span><span class="sxs-lookup"><span data-stu-id="15071-234">If the two command lines match, AzCopy resumes the incomplete operation.</span></span> <span data-ttu-id="15071-235">Om de inte matchar uppmanas AzCopy användaren att antingen skriva över journalfilen att starta en ny åtgärd eller Avbryt den aktuella åtgärden.</span><span class="sxs-lookup"><span data-stu-id="15071-235">If they do not match, AzCopy prompts user to either overwrite the journal file to start a new operation, or to cancel the current operation.</span></span>

<span data-ttu-id="15071-236">Om du vill använda standardplatsen för journalfilen:</span><span class="sxs-lookup"><span data-stu-id="15071-236">If you want to use the default location for the journal file:</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --resume
```

<span data-ttu-id="15071-237">Om du utelämnar alternativet `--resume`, eller ange alternativet `--resume` utan sökvägen till mappen som du ser ovan, AzCopy journalfilen skapas i standardplatsen, som är `~\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="15071-237">If you omit option `--resume`, or specify option `--resume` without the folder path, as shown above, AzCopy creates the journal file in the default location, which is `~\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="15071-238">Om ändringsjournalen filen redan finns återupptas AzCopy igen baserat på journal-fil.</span><span class="sxs-lookup"><span data-stu-id="15071-238">If the journal file already exists, then AzCopy resumes the operation based on the journal file.</span></span>

<span data-ttu-id="15071-239">Om du vill ange en anpassad plats för journalfilen:</span><span class="sxs-lookup"><span data-stu-id="15071-239">If you want to specify a custom location for the journal file:</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key key \
    --resume "/mnt/myjournal"
```

<span data-ttu-id="15071-240">Det här exemplet skapar journal-fil om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="15071-240">This example creates the journal file if it does not already exist.</span></span> <span data-ttu-id="15071-241">Om den finns, återupptar AzCopy igen baserat på journal-fil.</span><span class="sxs-lookup"><span data-stu-id="15071-241">If it does exist, then AzCopy resumes the operation based on the journal file.</span></span>

<span data-ttu-id="15071-242">Upprepa samma kommando om du vill fortsätta AzCopy.</span><span class="sxs-lookup"><span data-stu-id="15071-242">If you want to resume an AzCopy operation, repeat the same command.</span></span> <span data-ttu-id="15071-243">AzCopy på Linux sedan uppmanas att bekräfta:</span><span class="sxs-lookup"><span data-stu-id="15071-243">AzCopy on Linux then will prompt for confirmation:</span></span>

```azcopy
Incomplete operation with same command line detected at the journal directory "/home/myaccount/Microsoft/Azure/AzCopy", do you want to resume the operation? Choose Yes to resume, choose No to overwrite the journal to start a new operation. (Yes/No)
```

### <a name="output-verbose-logs"></a><span data-ttu-id="15071-244">Detaljerade loggarna för utdata</span><span class="sxs-lookup"><span data-stu-id="15071-244">Output verbose logs</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --verbose
```

### <a name="specify-the-number-of-concurrent-operations-to-start"></a><span data-ttu-id="15071-245">Ange antalet samtidiga åtgärder att starta</span><span class="sxs-lookup"><span data-stu-id="15071-245">Specify the number of concurrent operations to start</span></span>
<span data-ttu-id="15071-246">Alternativet `--parallel-level` anger antalet samtidiga kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="15071-246">Option `--parallel-level` specifies the number of concurrent copy operations.</span></span> <span data-ttu-id="15071-247">Som standard startar AzCopy antalet samtidiga åtgärder att öka genomflödet data transfer.</span><span class="sxs-lookup"><span data-stu-id="15071-247">By default, AzCopy starts a certain number of concurrent operations to increase the data transfer throughput.</span></span> <span data-ttu-id="15071-248">Antalet samtidiga åtgärder är lika åtta gånger antalet processorer som du har.</span><span class="sxs-lookup"><span data-stu-id="15071-248">The number of concurrent operations is equal eight times the number of processors you have.</span></span> <span data-ttu-id="15071-249">Om du använder AzCopy över ett nätverk med låg bandbredd, kan du ange ett lägre värde--parallell-nivå för att undvika misslyckades på grund av konkurrens om resurser.</span><span class="sxs-lookup"><span data-stu-id="15071-249">If you are running AzCopy across a low-bandwidth network, you can specify a lower number for --parallel-level to avoid failure caused by resource competition.</span></span>

[!TIP]
><span data-ttu-id="15071-250">Om du vill visa en fullständig lista över AzCopy parametrar kolla 'azcopy--Hjälp-menyn.</span><span class="sxs-lookup"><span data-stu-id="15071-250">To view the complete list of AzCopy parameters, check out 'azcopy --help' menu.</span></span>

## <a name="known-issues-and-best-practices"></a><span data-ttu-id="15071-251">Kända problem och bästa praxis</span><span class="sxs-lookup"><span data-stu-id="15071-251">Known issues and best practices</span></span>
### <a name="error-net-core-is-not-found-in-the-system"></a><span data-ttu-id="15071-252">Fel: .NET Core finns inte i systemet.</span><span class="sxs-lookup"><span data-stu-id="15071-252">Error: .NET Core is not found in the system.</span></span>
<span data-ttu-id="15071-253">Om du får ett felmeddelande om att .NET Core inte är installerat i systemet, SÖKVÄGEN till .NET Core binär `dotnet` kanske saknas.</span><span class="sxs-lookup"><span data-stu-id="15071-253">If you encounter an error stating that .NET Core is not installed in the system, the PATH to the .NET Core binary `dotnet` may be missing.</span></span>

<span data-ttu-id="15071-254">Hitta .NET Core binära i systemet för att lösa problemet:</span><span class="sxs-lookup"><span data-stu-id="15071-254">In order to address this issue, find the .NET Core binary in the system:</span></span>
```bash
sudo find / -name dotnet
```

<span data-ttu-id="15071-255">Detta returnerar sökvägen till dotnet binär.</span><span class="sxs-lookup"><span data-stu-id="15071-255">This returns the path to the dotnet binary.</span></span> 

    /opt/rh/rh-dotnetcore11/root/usr/bin/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/shared/Microsoft.NETCore.App/1.1.2/dotnet

<span data-ttu-id="15071-256">Lägg nu till denna sökväg i PATH-variabeln.</span><span class="sxs-lookup"><span data-stu-id="15071-256">Now add this path to the PATH variable.</span></span> <span data-ttu-id="15071-257">För sudo, redigera secure_path innehåller sökvägen till den binära dotnet:</span><span class="sxs-lookup"><span data-stu-id="15071-257">For sudo, edit secure_path to contain the path to the dotnet binary:</span></span>
```bash 
sudo visudo
### Append the path found in the preceding example to 'secure_path' variable
```

<span data-ttu-id="15071-258">I det här exemplet läser secure_path variabeln som:</span><span class="sxs-lookup"><span data-stu-id="15071-258">In this example, secure_path variable reads as:</span></span>

```
secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/opt/rh/rh-dotnetcore11/root/usr/bin/
```

<span data-ttu-id="15071-259">Redigera.bash_profile/.profile om du vill ta med sökvägen till dotnet binära i PATH-variabeln för den aktuella användaren</span><span class="sxs-lookup"><span data-stu-id="15071-259">For the current user, edit .bash_profile/.profile to include the path to the dotnet binary in PATH variable</span></span> 
```bash
vi ~/.bash_profile
### Append the path found in the preceding example to 'PATH' variable
```

<span data-ttu-id="15071-260">Kontrollera att .NET Core finns i SÖKVÄGEN:</span><span class="sxs-lookup"><span data-stu-id="15071-260">Verify that .NET Core is now in PATH:</span></span>
```bash
which dotnet
sudo which dotnet
```

### <a name="error-installing-azcopy"></a><span data-ttu-id="15071-261">Fel vid installation av AzCopy</span><span class="sxs-lookup"><span data-stu-id="15071-261">Error Installing AzCopy</span></span>
<span data-ttu-id="15071-262">Om du får problem med installation av AzCopy kan du försöker köra AzCopy använda bash-skript på den extraherade `azcopy` mapp.</span><span class="sxs-lookup"><span data-stu-id="15071-262">If you encounter issues with AzCopy installation, you may try to run AzCopy using the bash script in the extracted `azcopy` folder.</span></span>

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a><span data-ttu-id="15071-263">Begränsa samtidiga skrivningar vid kopiering av data</span><span class="sxs-lookup"><span data-stu-id="15071-263">Limit concurrent writes while copying data</span></span>
<span data-ttu-id="15071-264">När du kopierar blobar eller filer med AzCopy, Tänk på att ett annat program kan ändra data när du kopierar den.</span><span class="sxs-lookup"><span data-stu-id="15071-264">When you copy blobs or files with AzCopy, keep in mind that another application may be modifying the data while you are copying it.</span></span> <span data-ttu-id="15071-265">Kontrollera om det är möjligt att du kopierar data inte ändras under kopieringen.</span><span class="sxs-lookup"><span data-stu-id="15071-265">If possible, ensure that the data you are copying is not being modified during the copy operation.</span></span> <span data-ttu-id="15071-266">Till exempel när du kopierar en virtuell Hårddisk som är kopplad till en virtuell Azure-dator, kontrollera att inga andra program för tillfället skrivning till den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="15071-266">For example, when copying a VHD associated with an Azure virtual machine, make sure that no other applications are currently writing to the VHD.</span></span> <span data-ttu-id="15071-267">Ett bra sätt att göra detta är genom leasing resurs som ska kopieras.</span><span class="sxs-lookup"><span data-stu-id="15071-267">A good way to do this is by leasing the resource to be copied.</span></span> <span data-ttu-id="15071-268">Alternativt kan du skapa en ögonblicksbild av den virtuella Hårddisken först och sedan kopiera ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="15071-268">Alternately, you can create a snapshot of the VHD first and then copy the snapshot.</span></span>

<span data-ttu-id="15071-269">Om du inte hindra andra program från att skriva till filer eller blobbar när de kopieras sedan Kom ihåg att när jobbet har slutförts, kopiera resurserna kan inte längre har fullständig paritet med resurserna som källa.</span><span class="sxs-lookup"><span data-stu-id="15071-269">If you cannot prevent other applications from writing to blobs or files while they are being copied, then keep in mind that by the time the job finishes, the copied resources may no longer have full parity with the source resources.</span></span>

### <a name="run-one-azcopy-instance-on-one-machine"></a><span data-ttu-id="15071-270">Köra en AzCopy-instans på en dator.</span><span class="sxs-lookup"><span data-stu-id="15071-270">Run one AzCopy instance on one machine.</span></span>
<span data-ttu-id="15071-271">AzCopy är utformat för att maximera användningen av din datorresurser som påskyndar dataöverföringen rekommenderar vi du kör endast en AzCopy-instans på en dator och ange alternativet `--parallel-level` om du behöver fler samtidiga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="15071-271">AzCopy is designed to maximize the utilization of your machine resource to accelerate the data transfer, we recommend you run only one AzCopy instance on one machine, and specify the option `--parallel-level` if you need more concurrent operations.</span></span> <span data-ttu-id="15071-272">Mer information skriver du `AzCopy --help parallel-level` på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="15071-272">For more details, type `AzCopy --help parallel-level` at the command line.</span></span>

## <a name="next-steps"></a><span data-ttu-id="15071-273">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="15071-273">Next steps</span></span>
<span data-ttu-id="15071-274">Mer information om Azure Storage och AzCopy finns i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="15071-274">For more information about Azure Storage and AzCopy, see the following resources:</span></span>

### <a name="azure-storage-documentation"></a><span data-ttu-id="15071-275">Azure Storage-dokumentation:</span><span class="sxs-lookup"><span data-stu-id="15071-275">Azure Storage documentation:</span></span>
* [<span data-ttu-id="15071-276">Introduktion till Azure Storage</span><span class="sxs-lookup"><span data-stu-id="15071-276">Introduction to Azure Storage</span></span>](storage-introduction.md)
* [<span data-ttu-id="15071-277">Skapa ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="15071-277">Create a storage account</span></span>](storage-create-storage-account.md)
* [<span data-ttu-id="15071-278">Hantera blobbar med Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="15071-278">Manage blobs with Storage Explorer</span></span>](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs)
* [<span data-ttu-id="15071-279">Med hjälp av Azure CLI 2.0 med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="15071-279">Using the Azure CLI 2.0 with Azure Storage</span></span>](storage-azure-cli.md)
* [<span data-ttu-id="15071-280">Hur du använder Blob storage från C++</span><span class="sxs-lookup"><span data-stu-id="15071-280">How to use Blob storage from C++</span></span>](storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="15071-281">Använda Blob Storage från Java</span><span class="sxs-lookup"><span data-stu-id="15071-281">How to use Blob storage from Java</span></span>](storage-java-how-to-use-blob-storage.md)
* [<span data-ttu-id="15071-282">Använda Blob Storage från Node.js</span><span class="sxs-lookup"><span data-stu-id="15071-282">How to use Blob storage from Node.js</span></span>](storage-nodejs-how-to-use-blob-storage.md)
* [<span data-ttu-id="15071-283">Använda Blob Storage från Python</span><span class="sxs-lookup"><span data-stu-id="15071-283">How to use Blob storage from Python</span></span>](storage-python-how-to-use-blob-storage.md)

### <a name="azure-storage-blog-posts"></a><span data-ttu-id="15071-284">Azure Storage blogginlägg:</span><span class="sxs-lookup"><span data-stu-id="15071-284">Azure Storage blog posts:</span></span>
* [<span data-ttu-id="15071-285">Om AzCopy på Linux-Preview</span><span class="sxs-lookup"><span data-stu-id="15071-285">Announcing AzCopy on Linux Preview</span></span>](https://azure.microsoft.com/en-in/blog/announcing-azcopy-on-linux-preview/)
* [<span data-ttu-id="15071-286">Introduktion till Azure Storage Data Movement Library Preview</span><span class="sxs-lookup"><span data-stu-id="15071-286">Introducing Azure Storage Data Movement Library Preview</span></span>](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [<span data-ttu-id="15071-287">AzCopy: Introduktion till synkron kopia och anpassade innehållstyp</span><span class="sxs-lookup"><span data-stu-id="15071-287">AzCopy: Introducing synchronous copy and customized content type</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [<span data-ttu-id="15071-288">AzCopy: Om allmän tillgänglighet AzCopy 3.0 plus förhandsversionen av AzCopy 4.0 med tabell- och support</span><span class="sxs-lookup"><span data-stu-id="15071-288">AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [<span data-ttu-id="15071-289">AzCopy: Optimerade för storskaliga kopiera scenarier</span><span class="sxs-lookup"><span data-stu-id="15071-289">AzCopy: Optimized for Large-Scale Copy Scenarios</span></span>](http://go.microsoft.com/fwlink/?LinkId=507682)
* [<span data-ttu-id="15071-290">AzCopy: Stöd för geo-redundant lagring med läsbehörighet</span><span class="sxs-lookup"><span data-stu-id="15071-290">AzCopy: Support for read-access geo-redundant storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [<span data-ttu-id="15071-291">AzCopy: Överföra data med omstartsläge och SAS-token</span><span class="sxs-lookup"><span data-stu-id="15071-291">AzCopy: Transfer data with restartable mode and SAS token</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [<span data-ttu-id="15071-292">AzCopy: Använder mellan konto kopiera Blob</span><span class="sxs-lookup"><span data-stu-id="15071-292">AzCopy: Using cross-account Copy Blob</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [<span data-ttu-id="15071-293">AzCopy: Överför/hämta filer för Azure-BLOB</span><span class="sxs-lookup"><span data-stu-id="15071-293">AzCopy: Uploading/downloading files for Azure Blobs</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

