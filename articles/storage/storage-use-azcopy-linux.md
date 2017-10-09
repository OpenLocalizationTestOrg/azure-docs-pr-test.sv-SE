---
title: "aaaCopy eller flytta data tooAzure lagring med AzCopy på Linux | Microsoft Docs"
description: "Använd hello AzCopy på Linux-verktyget toomove eller kopiera data tooor från blob- och innehåll. Kopiera data tooAzure lagring från lokala filer eller kopiera data inom eller mellan lagringskonton. Enkelt migrera dina data tooAzure lagring."
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
ms.openlocfilehash: dccb03c9e8cc3ea661494e7834f307b0e3e30cb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-azcopy-on-linux"></a><span data-ttu-id="dec29-105">Överföra data med AzCopy på Linux</span><span class="sxs-lookup"><span data-stu-id="dec29-105">Transfer data with AzCopy on Linux</span></span>
<span data-ttu-id="dec29-106">AzCopy på Linux är ett kommandoradsverktyg som utformats för att kopiera data tooand från Microsoft Azure-Blob och fillagring med enkla kommandon med optimal prestanda.</span><span class="sxs-lookup"><span data-stu-id="dec29-106">AzCopy on Linux is a command-line utility designed for copying data tooand from Microsoft Azure Blob and File storage using simple commands with optimal performance.</span></span> <span data-ttu-id="dec29-107">Du kan kopiera data från en objektet tooanother inom ditt lagringskonto eller mellan lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="dec29-107">You can copy data from one object tooanother within your storage account, or between storage accounts.</span></span>

<span data-ttu-id="dec29-108">Det finns två versioner av AzCopy som du kan hämta.</span><span class="sxs-lookup"><span data-stu-id="dec29-108">There are two versions of AzCopy that you can download.</span></span> <span data-ttu-id="dec29-109">AzCopy på Linux är byggd med .NET Core Framework, som riktar sig till Linux-plattformar som erbjuder POSIX format kommandoradsalternativ.</span><span class="sxs-lookup"><span data-stu-id="dec29-109">AzCopy on Linux is built with .NET Core Framework, which targets Linux platforms offering POSIX style command-line options.</span></span> <span data-ttu-id="dec29-110">[AzCopy på Windows](storage-use-azcopy.md) har skapats med .NET Framework och erbjuder kommandoradsalternativ för Windows-formatet.</span><span class="sxs-lookup"><span data-stu-id="dec29-110">[AzCopy on Windows](storage-use-azcopy.md) is built with .NET Framework, and offers Windows style command-line options.</span></span> <span data-ttu-id="dec29-111">Den här artikeln beskriver AzCopy på Linux.</span><span class="sxs-lookup"><span data-stu-id="dec29-111">This article covers AzCopy on Linux.</span></span>

## <a name="download-and-install-azcopy"></a><span data-ttu-id="dec29-112">Hämta och installera AzCopy</span><span class="sxs-lookup"><span data-stu-id="dec29-112">Download and install AzCopy</span></span>
### <a name="installation-on-linux"></a><span data-ttu-id="dec29-113">Installation på Linux</span><span class="sxs-lookup"><span data-stu-id="dec29-113">Installation on Linux</span></span>

<span data-ttu-id="dec29-114">AzCopy på Linux kräver .NET Core framework på hello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="dec29-114">AzCopy on Linux requires .NET Core framework on hello platform.</span></span> <span data-ttu-id="dec29-115">Se hello installationsinstruktioner på hello [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) sidan.</span><span class="sxs-lookup"><span data-stu-id="dec29-115">See hello installation instructions on hello [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) page.</span></span>

<span data-ttu-id="dec29-116">Exempelvis kan vi installera .NET Core på Ubuntu 16,10.</span><span class="sxs-lookup"><span data-stu-id="dec29-116">As an example, let's install .NET Core on Ubuntu 16.10.</span></span> <span data-ttu-id="dec29-117">Hello senaste installationsguiden finns [.NET Core på Linux](https://www.microsoft.com/net/core#linuxubuntu) installationssidan.</span><span class="sxs-lookup"><span data-stu-id="dec29-117">For hello latest installation guide, visit [.NET Core on Linux](https://www.microsoft.com/net/core#linuxubuntu) installation page.</span></span>


```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.3
```

<span data-ttu-id="dec29-118">När du har installerat .NET Core, hämta och installera AzCopy.</span><span class="sxs-lookup"><span data-stu-id="dec29-118">Once you have installed .NET Core, download and install AzCopy.</span></span>

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

<span data-ttu-id="dec29-119">Du kan ta bort hello extraherade filer när AzCopy på Linux har installerats.</span><span class="sxs-lookup"><span data-stu-id="dec29-119">You can remove hello extracted files once AzCopy on Linux is installed.</span></span> <span data-ttu-id="dec29-120">Du kan också om du inte har superanvändare, kan du också köra AzCopy med hello kommandoskript 'azcopy' i hello extraherade mappen.</span><span class="sxs-lookup"><span data-stu-id="dec29-120">Alternatively if you do not have superuser privileges, you can also run AzCopy using hello shell script 'azcopy' in hello extracted folder.</span></span> 

### <a name="alternative-installation-on-ubuntu"></a><span data-ttu-id="dec29-121">Alternativ Installation på Ubuntu</span><span class="sxs-lookup"><span data-stu-id="dec29-121">Alternative Installation on Ubuntu</span></span>

<span data-ttu-id="dec29-122">**Ubuntu 14.04**</span><span class="sxs-lookup"><span data-stu-id="dec29-122">**Ubuntu 14.04**</span></span>

<span data-ttu-id="dec29-123">Lägg till lgh källa för .net Core:</span><span class="sxs-lookup"><span data-stu-id="dec29-123">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="dec29-124">Lägg till lgh källa för Microsoft Linux produkten lagringsplatsen och installera AzCopy:</span><span class="sxs-lookup"><span data-stu-id="dec29-124">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

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

<span data-ttu-id="dec29-125">**Ubuntu 16.04**</span><span class="sxs-lookup"><span data-stu-id="dec29-125">**Ubuntu 16.04**</span></span>

<span data-ttu-id="dec29-126">Lägg till lgh källa för .net Core:</span><span class="sxs-lookup"><span data-stu-id="dec29-126">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="dec29-127">Lägg till lgh källa för Microsoft Linux produkten lagringsplatsen och installera AzCopy:</span><span class="sxs-lookup"><span data-stu-id="dec29-127">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

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

<span data-ttu-id="dec29-128">**Ubuntu 16,10**</span><span class="sxs-lookup"><span data-stu-id="dec29-128">**Ubuntu 16.10**</span></span>

<span data-ttu-id="dec29-129">Lägg till lgh källa för .net Core:</span><span class="sxs-lookup"><span data-stu-id="dec29-129">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="dec29-130">Lägg till lgh källa för Microsoft Linux produkten lagringsplatsen och installera AzCopy:</span><span class="sxs-lookup"><span data-stu-id="dec29-130">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

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

## <a name="writing-your-first-azcopy-command"></a><span data-ttu-id="dec29-131">Skriva ditt första AzCopy-kommandot</span><span class="sxs-lookup"><span data-stu-id="dec29-131">Writing your first AzCopy command</span></span>
<span data-ttu-id="dec29-132">grundläggande hello-syntaxen för AzCopy kommandon är:</span><span class="sxs-lookup"><span data-stu-id="dec29-132">hello basic syntax for AzCopy commands is:</span></span>

```azcopy
azcopy --source <source> --destination <destination> [Options]
```

<span data-ttu-id="dec29-133">hello som följande exempel visar olika scenarier för att kopiera data tooand från Microsoft Azure-BLOB och filer.</span><span class="sxs-lookup"><span data-stu-id="dec29-133">hello following examples demonstrate various scenarios for copying data tooand from Microsoft Azure Blobs and Files.</span></span> <span data-ttu-id="dec29-134">Se toohello `azcopy --help` menyn för en detaljerad förklaring av hello parametrar som används i varje prov.</span><span class="sxs-lookup"><span data-stu-id="dec29-134">Refer toohello `azcopy --help` menu for a detailed explanation of hello parameters used in each sample.</span></span>

## <a name="blob-download"></a><span data-ttu-id="dec29-135">BLOB: ladda ned</span><span class="sxs-lookup"><span data-stu-id="dec29-135">Blob: Download</span></span>
### <a name="download-single-blob"></a><span data-ttu-id="dec29-136">Hämta en enda blob</span><span class="sxs-lookup"><span data-stu-id="dec29-136">Download single blob</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="dec29-137">Om hello mappen `/mnt/myfiles` finns inte, AzCopy skapar den och hämtar `abc.txt ` till hello ny mapp.</span><span class="sxs-lookup"><span data-stu-id="dec29-137">If hello folder `/mnt/myfiles` does not exist, AzCopy creates it and downloads `abc.txt ` into hello new folder.</span></span>

### <a name="download-single-blob-from-secondary-region"></a><span data-ttu-id="dec29-138">Hämta en enda blob från sekundär region</span><span class="sxs-lookup"><span data-stu-id="dec29-138">Download single blob from secondary region</span></span>

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="dec29-139">Observera att du måste ha läsbehörighet geo-redundant lagring aktiverad.</span><span class="sxs-lookup"><span data-stu-id="dec29-139">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="download-all-blobs"></a><span data-ttu-id="dec29-140">Hämta alla BLOB</span><span class="sxs-lookup"><span data-stu-id="dec29-140">Download all blobs</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

<span data-ttu-id="dec29-141">Anta hello följande blobbar finns i angivna hello-behållaren:</span><span class="sxs-lookup"><span data-stu-id="dec29-141">Assume hello following blobs reside in hello specified container:</span></span>  

```
abc.txt
abc1.txt
abc2.txt
vd1/a.txt
vd1/abcd.txt
```

<span data-ttu-id="dec29-142">Efter hello hämtningen hello directory `/mnt/myfiles` innehåller hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="dec29-142">After hello download operation, hello directory `/mnt/myfiles` includes hello following files:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/vd1/a.txt
/mnt/myfiles/vd1/abcd.txt
```

<span data-ttu-id="dec29-143">Om du inte anger alternativet `--recursive`, ingen blob som ska hämtas.</span><span class="sxs-lookup"><span data-stu-id="dec29-143">If you do not specify option `--recursive`, no blob will be downloaded.</span></span>

### <a name="download-blobs-with-specified-prefix"></a><span data-ttu-id="dec29-144">Ladda ned blobbar med angivna prefix</span><span class="sxs-lookup"><span data-stu-id="dec29-144">Download blobs with specified prefix</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "a" \
    --recursive
```

<span data-ttu-id="dec29-145">Anta hello följande blobbar finns i hello angivna behållaren.</span><span class="sxs-lookup"><span data-stu-id="dec29-145">Assume hello following blobs reside in hello specified container.</span></span> <span data-ttu-id="dec29-146">Alla blobbar som börjar med prefixet hello `a` hämtas.</span><span class="sxs-lookup"><span data-stu-id="dec29-146">All blobs beginning with hello prefix `a` are downloaded.</span></span>

```
abc.txt
abc1.txt
abc2.txt
xyz.txt
vd1\a.txt
vd1\abcd.txt
```

<span data-ttu-id="dec29-147">Efter hello hämtningen hello mappen `/mnt/myfiles` innehåller hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="dec29-147">After hello download operation, hello folder `/mnt/myfiles` includes hello following files:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
```

<span data-ttu-id="dec29-148">hello prefix gäller toohello virtuella katalogen, som utgör hello första delen av hello blob-namnet.</span><span class="sxs-lookup"><span data-stu-id="dec29-148">hello prefix applies toohello virtual directory, which forms hello first part of hello blob name.</span></span> <span data-ttu-id="dec29-149">I hello exemplet ovan matchar hello virtuella katalogen inte hello angivna prefix, så inga blob hämtas.</span><span class="sxs-lookup"><span data-stu-id="dec29-149">In hello example shown above, hello virtual directory does not match hello specified prefix, so no blob is downloaded.</span></span> <span data-ttu-id="dec29-150">Dessutom, om hello alternativet `--recursive` anges AzCopy inte hämta alla blobbar.</span><span class="sxs-lookup"><span data-stu-id="dec29-150">In addition, if hello option `--recursive` is not specified, AzCopy does not download any blobs.</span></span>

### <a name="set-hello-last-modified-time-of-exported-files-toobe-same-as-hello-source-blobs"></a><span data-ttu-id="dec29-151">Ange hello modifierades senast av exporterade filerna toobe samma som hello källa blobbar</span><span class="sxs-lookup"><span data-stu-id="dec29-151">Set hello last-modified time of exported files toobe same as hello source blobs</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination "/mnt/myfiles" \
    --source-key <key> \
    --preserve-last-modified-time
```

<span data-ttu-id="dec29-152">Du kan också utesluta blobbar från hello hämtningen baserat på deras tid för senaste ändring.</span><span class="sxs-lookup"><span data-stu-id="dec29-152">You can also exclude blobs from hello download operation based on their last-modified time.</span></span> <span data-ttu-id="dec29-153">Till exempel om du vill tooexclude blobbar vars senaste ändringstiden är hello samma eller en senare än hello målfilen, lägga till hello `--exclude-newer` alternativ:</span><span class="sxs-lookup"><span data-stu-id="dec29-153">For example, if you want tooexclude blobs whose last modified time is hello same or newer than hello destination file, add hello `--exclude-newer` option:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-newer
```

<span data-ttu-id="dec29-154">Eller om du vill tooexclude blobbar vars senaste ändringstiden är hello samma eller äldre än hello målfilen, lägger du till hello `--exclude-older` alternativ:</span><span class="sxs-lookup"><span data-stu-id="dec29-154">Or if you want tooexclude blobs whose last modified time is hello same or older than hello destination file, add hello `--exclude-older` option:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-older
```

## <a name="blob-upload"></a><span data-ttu-id="dec29-155">BLOB: Överför</span><span class="sxs-lookup"><span data-stu-id="dec29-155">Blob: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="dec29-156">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="dec29-156">Upload single file</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="dec29-157">Om hello angivna målbehållare inte finns, AzCopy skapar den och överföringar hello filen till den.</span><span class="sxs-lookup"><span data-stu-id="dec29-157">If hello specified destination container does not exist, AzCopy creates it and uploads hello file into it.</span></span>

### <a name="upload-single-file-toovirtual-directory"></a><span data-ttu-id="dec29-158">Överför en fil toovirtual directory</span><span class="sxs-lookup"><span data-stu-id="dec29-158">Upload single file toovirtual directory</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="dec29-159">Om hello angivna virtuella katalogen inte finns, överför AzCopy hello filen tooinclude hello virtuell katalog på hello blobbnamnet (*t.ex.*, `vd/abc.txt` i hello-exemplet ovan).</span><span class="sxs-lookup"><span data-stu-id="dec29-159">If hello specified virtual directory does not exist, AzCopy uploads hello file tooinclude hello virtual directory in hello blob name (*e.g.*, `vd/abc.txt` in hello example above).</span></span>

### <a name="upload-all-files"></a><span data-ttu-id="dec29-160">Ladda upp alla filer</span><span class="sxs-lookup"><span data-stu-id="dec29-160">Upload all files</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive
```

<span data-ttu-id="dec29-161">Om du anger alternativet `--recursive` överföringar hello innehållet i hello angetts directory tooBlob lagring rekursivt, vilket innebär att alla undermappar och filer överförs också.</span><span class="sxs-lookup"><span data-stu-id="dec29-161">Specifying option `--recursive` uploads hello contents of hello specified directory tooBlob storage recursively, meaning that all subfolders and their files are uploaded as well.</span></span> <span data-ttu-id="dec29-162">Anta exempelvis att hello följande filer finns i mappen `/mnt/myfiles`:</span><span class="sxs-lookup"><span data-stu-id="dec29-162">For instance, assume hello following files reside in folder `/mnt/myfiles`:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

<span data-ttu-id="dec29-163">Efter hello överföringen innehåller hello behållaren hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="dec29-163">After hello upload operation, hello container includes hello following files:</span></span>

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

<span data-ttu-id="dec29-164">När hello alternativet `--recursive` anges endast hello följande tre filer överförs:</span><span class="sxs-lookup"><span data-stu-id="dec29-164">When hello option `--recursive` is not specified, only hello following three files are uploaded:</span></span>

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="dec29-165">Överföra filer som matchar angivna mönstret</span><span class="sxs-lookup"><span data-stu-id="dec29-165">Upload files matching specified pattern</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "a*" \
    --recursive
```

<span data-ttu-id="dec29-166">Anta hello följande filer finns i mappen `/mnt/myfiles`:</span><span class="sxs-lookup"><span data-stu-id="dec29-166">Assume hello following files reside in folder `/mnt/myfiles`:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/xyz.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

<span data-ttu-id="dec29-167">Efter hello överföringen innehåller hello behållaren hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="dec29-167">After hello upload operation, hello container includes hello following files:</span></span>

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

<span data-ttu-id="dec29-168">När hello alternativet `--recursive` anges AzCopy hoppar över filer som finns i underkataloger:</span><span class="sxs-lookup"><span data-stu-id="dec29-168">When hello option `--recursive` is not specified, AzCopy skips files that are in sub-directories:</span></span>

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="specify-hello-mime-content-type-of-a-destination-blob"></a><span data-ttu-id="dec29-169">Ange hello MIME content-type för en mål-blob</span><span class="sxs-lookup"><span data-stu-id="dec29-169">Specify hello MIME content type of a destination blob</span></span>
<span data-ttu-id="dec29-170">Som standard AzCopy anger hello innehållstypen för en mål-blob för`application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="dec29-170">By default, AzCopy sets hello content type of a destination blob too`application/octet-stream`.</span></span> <span data-ttu-id="dec29-171">Du kan dock uttryckligen ange hello innehållstyp via hello alternativet `--set-content-type [content-type]`.</span><span class="sxs-lookup"><span data-stu-id="dec29-171">However, you can explicitly specify hello content type via hello option `--set-content-type [content-type]`.</span></span> <span data-ttu-id="dec29-172">Den här syntaxen anger hello content-type för alla blobbar i en överföringen.</span><span class="sxs-lookup"><span data-stu-id="dec29-172">This syntax sets hello content type for all blobs in an upload operation.</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type "video/mp4"
```

<span data-ttu-id="dec29-173">Om hello alternativet `--set-content-type` anges utan värde, och sedan AzCopy anger varje blob eller en fil vars innehållstyp enligt tooits filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="dec29-173">If hello option `--set-content-type` is specified without a value, then AzCopy sets each blob or file's content type according tooits file extension.</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type
```

## <a name="blob-copy"></a><span data-ttu-id="dec29-174">BLOB: kopiera</span><span class="sxs-lookup"><span data-stu-id="dec29-174">Blob: Copy</span></span>
### <a name="copy-single-blob-within-storage-account"></a><span data-ttu-id="dec29-175">Kopiera enda blob inom lagringskonto</span><span class="sxs-lookup"><span data-stu-id="dec29-175">Copy single blob within Storage account</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key> \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="dec29-176">När du kopierar en blobb utan--synkroniserad kopia alternativet en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="dec29-176">When you copy a blob without --sync-copy option, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-across-storage-accounts"></a><span data-ttu-id="dec29-177">Kopiera enda blob på Storage-konton</span><span class="sxs-lookup"><span data-stu-id="dec29-177">Copy single blob across Storage accounts</span></span>

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

<span data-ttu-id="dec29-178">När du kopierar en blobb utan--synkroniserad kopia alternativet en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="dec29-178">When you copy a blob without --sync-copy option, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-from-secondary-region-tooprimary-region"></a><span data-ttu-id="dec29-179">Kopiera enda blob från sekundär region tooprimary region</span><span class="sxs-lookup"><span data-stu-id="dec29-179">Copy single blob from secondary region tooprimary region</span></span>

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

<span data-ttu-id="dec29-180">Observera att du måste ha läsbehörighet geo-redundant lagring aktiverad.</span><span class="sxs-lookup"><span data-stu-id="dec29-180">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a><span data-ttu-id="dec29-181">Kopiera enda blob och dess ögonblicksbilder på Storage-konton</span><span class="sxs-lookup"><span data-stu-id="dec29-181">Copy single blob and its snapshots across Storage accounts</span></span>

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --include-snapshot
```

<span data-ttu-id="dec29-182">Efter hello kopieringen innehåller hello Målbehållaren hello blob och dess ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="dec29-182">After hello copy operation, hello target container includes hello blob and its snapshots.</span></span> <span data-ttu-id="dec29-183">hello behållaren omfattar hello följande blob och dess ögonblicksbilder:</span><span class="sxs-lookup"><span data-stu-id="dec29-183">hello container includes hello following blob and its snapshots:</span></span>

```
abc.txt
abc (2013-02-25 080757).txt
abc (2014-02-21 150331).txt
```

### <a name="synchronously-copy-blobs-across-storage-accounts"></a><span data-ttu-id="dec29-184">Kopiera synkront blobar på lagringskonton</span><span class="sxs-lookup"><span data-stu-id="dec29-184">Synchronously copy blobs across Storage accounts</span></span>
<span data-ttu-id="dec29-185">AzCopy som standard kopierar data mellan två slutpunkter för lagring asynkront.</span><span class="sxs-lookup"><span data-stu-id="dec29-185">AzCopy by default copies data between two storage endpoints asynchronously.</span></span> <span data-ttu-id="dec29-186">Därför kopieras hello Kopiera åtgärden körs i hello bakgrunden med hjälp av ledig bandbreddskapacitet som har inga SLA vad gäller hur snabbt en blob.</span><span class="sxs-lookup"><span data-stu-id="dec29-186">Therefore, hello copy operation runs in hello background using spare bandwidth capacity that has no SLA in terms of how fast a blob is copied.</span></span> 

<span data-ttu-id="dec29-187">Hej `--sync-copy` alternativet ser du till att hello kopieringsåtgärden hämtar konsekvent hastighet.</span><span class="sxs-lookup"><span data-stu-id="dec29-187">hello `--sync-copy` option ensures that hello copy operation gets consistent speed.</span></span> <span data-ttu-id="dec29-188">AzCopy utför hello synkron kopia genom att hämta hello blobbar toocopy från hello anges källa toolocal minne, och överföra dem toohello Blob-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="dec29-188">AzCopy performs hello synchronous copy by downloading hello blobs toocopy from hello specified source toolocal memory, and then uploading them toohello Blob storage destination.</span></span>

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "ab" \
    --sync-copy
```

<span data-ttu-id="dec29-189">`--sync-copy`kan skapa ytterligare utgång kostnaden jämfört med tooasynchronous kopia.</span><span class="sxs-lookup"><span data-stu-id="dec29-189">`--sync-copy` might generate additional egress cost compared tooasynchronous copy.</span></span> <span data-ttu-id="dec29-190">hello rekommenderade metoden är toouse det här alternativet i en Azure VM i hello samma region som din datakälla konto tooavoid utgång lagringskostnaden.</span><span class="sxs-lookup"><span data-stu-id="dec29-190">hello recommended approach is toouse this option in an Azure VM, that is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="file-download"></a><span data-ttu-id="dec29-191">Fil: ladda ned</span><span class="sxs-lookup"><span data-stu-id="dec29-191">File: Download</span></span>
### <a name="download-single-file"></a><span data-ttu-id="dec29-192">Hämta en fil</span><span class="sxs-lookup"><span data-stu-id="dec29-192">Download single file</span></span>

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="dec29-193">Om hello anges källan är en Azure-filresurs, måste du antingen ange hello exakta filnamnet (*t.ex.* `abc.txt`) toodownload en enskild fil, eller ange alternativet `--recursive` toodownload alla filer i hello resurs rekursivt.</span><span class="sxs-lookup"><span data-stu-id="dec29-193">If hello specified source is an Azure file share, then you must either specify hello exact file name, (*e.g.* `abc.txt`) toodownload a single file, or specify option `--recursive` toodownload all files in hello share recursively.</span></span> <span data-ttu-id="dec29-194">Försök toospecify både filmönstret och alternativet `--recursive` tillsammans resulterar i ett fel.</span><span class="sxs-lookup"><span data-stu-id="dec29-194">Attempting toospecify both a file pattern and option `--recursive` together results in an error.</span></span>

### <a name="download-all-files"></a><span data-ttu-id="dec29-195">Hämta alla filer</span><span class="sxs-lookup"><span data-stu-id="dec29-195">Download all files</span></span>

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

<span data-ttu-id="dec29-196">Observera att alla tomma mappar inte laddas ned.</span><span class="sxs-lookup"><span data-stu-id="dec29-196">Note that any empty folders are not downloaded.</span></span>

## <a name="file-upload"></a><span data-ttu-id="dec29-197">Fil: Överför</span><span class="sxs-lookup"><span data-stu-id="dec29-197">File: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="dec29-198">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="dec29-198">Upload single file</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include abc.txt
```

### <a name="upload-all-files"></a><span data-ttu-id="dec29-199">Ladda upp alla filer</span><span class="sxs-lookup"><span data-stu-id="dec29-199">Upload all files</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --recursive
```

<span data-ttu-id="dec29-200">Observera att alla tomma mappar inte upp.</span><span class="sxs-lookup"><span data-stu-id="dec29-200">Note that any empty folders are not uploaded.</span></span>

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="dec29-201">Överföra filer som matchar angivna mönstret</span><span class="sxs-lookup"><span data-stu-id="dec29-201">Upload files matching specified pattern</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include "ab*" \
    --recursive
```

## <a name="file-copy"></a><span data-ttu-id="dec29-202">Fil: kopiera</span><span class="sxs-lookup"><span data-stu-id="dec29-202">File: Copy</span></span>
### <a name="copy-across-file-shares"></a><span data-ttu-id="dec29-203">Kopiera på filresurser</span><span class="sxs-lookup"><span data-stu-id="dec29-203">Copy across file shares</span></span>

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="dec29-204">När du kopierar en fil på filresurser, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="dec29-204">When you copy a file across file shares, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-file-share-tooblob"></a><span data-ttu-id="dec29-205">Kopiera från filen resursen tooblob</span><span class="sxs-lookup"><span data-stu-id="dec29-205">Copy from file share tooblob</span></span>

```azcopy
azcopy \ 
    --source https://myaccount1.file.core.windows.net/myfileshare/ \
    --destination https://myaccount2.blob.core.windows.net/mycontainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="dec29-206">När du kopierar en fil från filen resursen tooblob en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="dec29-206">When you copy a file from file share tooblob, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-blob-toofile-share"></a><span data-ttu-id="dec29-207">Kopiera från blob toofile resurs</span><span class="sxs-lookup"><span data-stu-id="dec29-207">Copy from blob toofile share</span></span>

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/mycontainer/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="dec29-208">När du kopierar en fil från blob toofile resurs, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="dec29-208">When you copy a file from blob toofile share, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="synchronously-copy-files"></a><span data-ttu-id="dec29-209">Synkront kopiera filer</span><span class="sxs-lookup"><span data-stu-id="dec29-209">Synchronously copy files</span></span>
<span data-ttu-id="dec29-210">Du kan ange hello `--sync-copy` alternativet toocopy data från fillagring tooFile lagring, File Storage tooBlob lagring och Blob Storage tooFile lagring synkront.</span><span class="sxs-lookup"><span data-stu-id="dec29-210">You can specify hello `--sync-copy` option toocopy data from File Storage tooFile Storage, from File Storage tooBlob Storage and from Blob Storage tooFile Storage synchronously.</span></span> <span data-ttu-id="dec29-211">AzCopy körs åtgärden genom att hämta hello källa data toolocal minne och överföra den toodestination.</span><span class="sxs-lookup"><span data-stu-id="dec29-211">AzCopy runs this operation by downloading hello source data toolocal memory, and then uploading it toodestination.</span></span> <span data-ttu-id="dec29-212">I det här fallet gäller standard utgång kostnaden.</span><span class="sxs-lookup"><span data-stu-id="dec29-212">In this case, standard egress cost applies.</span></span>

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive \
    --sync-copy
```

<span data-ttu-id="dec29-213">När du kopierar från fillagring tooBlob lagring hello standard blob-datatyp är blockblob, kan användaren ange alternativet `/BlobType:page` toochange hello blob måltypen.</span><span class="sxs-lookup"><span data-stu-id="dec29-213">When copying from File Storage tooBlob Storage, hello default blob type is block blob, user can specify option `/BlobType:page` toochange hello destination blob type.</span></span>

<span data-ttu-id="dec29-214">Observera att `--sync-copy` kan skapa ytterligare utgång kostnad jämför tooasynchronous kopia.</span><span class="sxs-lookup"><span data-stu-id="dec29-214">Note that `--sync-copy` might generate additional egress cost comparing tooasynchronous copy.</span></span> <span data-ttu-id="dec29-215">hello rekommenderade metoden är toouse det här alternativet i en Azure VM i hello samma region som din datakälla konto tooavoid utgång lagringskostnaden.</span><span class="sxs-lookup"><span data-stu-id="dec29-215">hello recommended approach is toouse this option in an Azure VM, that is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="other-azcopy-features"></a><span data-ttu-id="dec29-216">Andra AzCopy-funktioner</span><span class="sxs-lookup"><span data-stu-id="dec29-216">Other AzCopy features</span></span>
### <a name="only-copy-data-that-doesnt-exist-in-hello-destination"></a><span data-ttu-id="dec29-217">Endast kopiera data som inte finns i hello mål</span><span class="sxs-lookup"><span data-stu-id="dec29-217">Only copy data that doesn't exist in hello destination</span></span>
<span data-ttu-id="dec29-218">Hej `--exclude-older` och `--exclude-newer` parametrar kan du tooexclude äldre eller nyare resurser från att kopieras respektive.</span><span class="sxs-lookup"><span data-stu-id="dec29-218">hello `--exclude-older` and `--exclude-newer` parameters allow you tooexclude older or newer source resources from being copied, respectively.</span></span> <span data-ttu-id="dec29-219">Om du bara vill toocopy källa resurser som inte finns i hello målet kan du ange båda parametrarna i hello AzCopy-kommandot:</span><span class="sxs-lookup"><span data-stu-id="dec29-219">If you only want toocopy source resources that don't exist in hello destination, you can specify both parameters in hello AzCopy command:</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --exclude-older --exclude-newer

    --source /mnt/myfiles --destination http://myaccount.file.core.windows.net/myfileshare --dest-key <destkey> --recursive --exclude-older --exclude-newer

    --source http://myaccount.blob.core.windows.net/mycontainer --destination http://myaccount.blob.core.windows.net/mycontainer1 --source-key <sourcekey> --dest-key <destkey> --recursive --exclude-older --exclude-newer

### <a name="use-a-configuration-file-toospecify-command-line-parameters"></a><span data-ttu-id="dec29-220">Använd en fil toospecify kommandoradsverktyget konfigurationsparametrar</span><span class="sxs-lookup"><span data-stu-id="dec29-220">Use a configuration file toospecify command-line parameters</span></span>

```azcopy
azcopy --config-file "azcopy-config.ini"
```

<span data-ttu-id="dec29-221">Du kan inkludera eventuella kommandoradsparametrar för AzCopy i en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="dec29-221">You can include any AzCopy command-line parameters in a configuration file.</span></span> <span data-ttu-id="dec29-222">AzCopy processer hello parametrar i hello-filen som om de hade angetts på kommandoraden för hello utför en direkt ersättning med hello innehållet i hello-fil.</span><span class="sxs-lookup"><span data-stu-id="dec29-222">AzCopy processes hello parameters in hello file as if they had been specified on hello command line, performing a direct substitution with hello contents of hello file.</span></span>

<span data-ttu-id="dec29-223">Anta en fil med namnet `copyoperation`, som innehåller hello följande rader.</span><span class="sxs-lookup"><span data-stu-id="dec29-223">Assume a configuration file named `copyoperation`, that contains hello following lines.</span></span> <span data-ttu-id="dec29-224">Varje AzCopy-parameter kan anges på en enda rad.</span><span class="sxs-lookup"><span data-stu-id="dec29-224">Each AzCopy parameter can be specified on a single line.</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --quiet

<span data-ttu-id="dec29-225">eller på separata rader:</span><span class="sxs-lookup"><span data-stu-id="dec29-225">or on separate lines:</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer
    --destination /mnt/myfiles
    --source-key<sourcekey>
    --recursive
    --quiet

<span data-ttu-id="dec29-226">AzCopy misslyckas om du vill dela hello parametern på två rader som visas här för hello `--source-key` parameter:</span><span class="sxs-lookup"><span data-stu-id="dec29-226">AzCopy fails if you split hello parameter across two lines, as shown here for hello `--source-key` parameter:</span></span>

    http://myaccount.blob.core.windows.net/mycontainer
    /mnt/myfiles
    --sourcekey
    <sourcekey>
    --recursive
    --quiet

### <a name="specify-a-shared-access-signature-sas"></a><span data-ttu-id="dec29-227">Ange en signatur för delad åtkomst (SAS)</span><span class="sxs-lookup"><span data-stu-id="dec29-227">Specify a shared access signature (SAS)</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-sas <SAS1> \
    --dest-sas <SAS2> \
    --include abc.txt
```

<span data-ttu-id="dec29-228">Du kan också ange en SAS för hello behållaren URI:</span><span class="sxs-lookup"><span data-stu-id="dec29-228">You can also specify a SAS on hello container URI:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken \
    --destination /mnt/myfiles \
    --recursive
```

<span data-ttu-id="dec29-229">Observera att AzCopy för närvarande endast stöder hello [kontots SAS](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="dec29-229">Note that AzCopy currently only supports hello [Account SAS](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).</span></span>

### <a name="journal-file-folder"></a><span data-ttu-id="dec29-230">Ändringsjournalen mapp</span><span class="sxs-lookup"><span data-stu-id="dec29-230">Journal file folder</span></span>
<span data-ttu-id="dec29-231">Varje gång du utfärda ett kommando tooAzCopy kontrollerar den om en journal-fil finns i hello standardmappen eller om den finns i en mapp som du har angett via det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="dec29-231">Each time you issue a command tooAzCopy, it checks whether a journal file exists in hello default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="dec29-232">Om hello journalfilen inte finns på någon plats, behandlar hello åtgärden som nya AzCopy och genererar en ny journalfil.</span><span class="sxs-lookup"><span data-stu-id="dec29-232">If hello journal file does not exist in either place, AzCopy treats hello operation as new and generates a new journal file.</span></span>

<span data-ttu-id="dec29-233">Om hello journalfilen finns kontrollerar AzCopy om hello-kommandoraden som du matar in matchar hello kommandoraden i hello journal-fil.</span><span class="sxs-lookup"><span data-stu-id="dec29-233">If hello journal file does exist, AzCopy checks whether hello command line that you input matches hello command line in hello journal file.</span></span> <span data-ttu-id="dec29-234">Om hello två kommandorader matchar återupptar AzCopy hello ofullständiga igen.</span><span class="sxs-lookup"><span data-stu-id="dec29-234">If hello two command lines match, AzCopy resumes hello incomplete operation.</span></span> <span data-ttu-id="dec29-235">Om de inte matchar uppmanas AzCopy användaren tooeither överskrivning hello journal filen toostart en ny åtgärd eller toocancel hello den aktuella åtgärden.</span><span class="sxs-lookup"><span data-stu-id="dec29-235">If they do not match, AzCopy prompts user tooeither overwrite hello journal file toostart a new operation, or toocancel hello current operation.</span></span>

<span data-ttu-id="dec29-236">Om du vill toouse hello standardplatsen för hello journal-fil:</span><span class="sxs-lookup"><span data-stu-id="dec29-236">If you want toouse hello default location for hello journal file:</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --resume
```

<span data-ttu-id="dec29-237">Om du utelämnar alternativet `--resume`, eller ange alternativet `--resume` utan hello mappsökvägen, enligt ovan, AzCopy skapar hello journal-fil i hello standardplatsen, som är `~\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="dec29-237">If you omit option `--resume`, or specify option `--resume` without hello folder path, as shown above, AzCopy creates hello journal file in hello default location, which is `~\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="dec29-238">Om hello journalfilen redan finns återupptas AzCopy hello-åtgärden utifrån hello journal-fil.</span><span class="sxs-lookup"><span data-stu-id="dec29-238">If hello journal file already exists, then AzCopy resumes hello operation based on hello journal file.</span></span>

<span data-ttu-id="dec29-239">Om du vill toospecify en egen placering för hello journal-fil:</span><span class="sxs-lookup"><span data-stu-id="dec29-239">If you want toospecify a custom location for hello journal file:</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key key \
    --resume "/mnt/myjournal"
```

<span data-ttu-id="dec29-240">Det här exemplet skapar hello journal-filen om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="dec29-240">This example creates hello journal file if it does not already exist.</span></span> <span data-ttu-id="dec29-241">Om den finns, återupptar AzCopy hello-åtgärden utifrån hello journal-fil.</span><span class="sxs-lookup"><span data-stu-id="dec29-241">If it does exist, then AzCopy resumes hello operation based on hello journal file.</span></span>

<span data-ttu-id="dec29-242">Om du vill tooresume en AzCopy-åtgärd, upprepa hello samma kommando.</span><span class="sxs-lookup"><span data-stu-id="dec29-242">If you want tooresume an AzCopy operation, repeat hello same command.</span></span> <span data-ttu-id="dec29-243">AzCopy på Linux sedan uppmanas att bekräfta:</span><span class="sxs-lookup"><span data-stu-id="dec29-243">AzCopy on Linux then will prompt for confirmation:</span></span>

```azcopy
Incomplete operation with same command line detected at hello journal directory "/home/myaccount/Microsoft/Azure/AzCopy", do you want tooresume hello operation? Choose Yes tooresume, choose No toooverwrite hello journal toostart a new operation. (Yes/No)
```

### <a name="output-verbose-logs"></a><span data-ttu-id="dec29-244">Detaljerade loggarna för utdata</span><span class="sxs-lookup"><span data-stu-id="dec29-244">Output verbose logs</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --verbose
```

### <a name="specify-hello-number-of-concurrent-operations-toostart"></a><span data-ttu-id="dec29-245">Ange hello antalet samtidiga åtgärder toostart</span><span class="sxs-lookup"><span data-stu-id="dec29-245">Specify hello number of concurrent operations toostart</span></span>
<span data-ttu-id="dec29-246">Alternativet `--parallel-level` anger hello antalet samtidiga kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="dec29-246">Option `--parallel-level` specifies hello number of concurrent copy operations.</span></span> <span data-ttu-id="dec29-247">Som standard startar AzCopy antalet samtidiga åtgärder tooincrease hello data transfer genomflöde.</span><span class="sxs-lookup"><span data-stu-id="dec29-247">By default, AzCopy starts a certain number of concurrent operations tooincrease hello data transfer throughput.</span></span> <span data-ttu-id="dec29-248">hello antalet samtidiga åtgärder är lika åtta gånger hello antal processorer som du har.</span><span class="sxs-lookup"><span data-stu-id="dec29-248">hello number of concurrent operations is equal eight times hello number of processors you have.</span></span> <span data-ttu-id="dec29-249">Om du använder AzCopy över ett nätverk med låg bandbredd, kan du ange ett lägre värde--parallell nivå tooavoid misslyckades på grund av konkurrens om resurser.</span><span class="sxs-lookup"><span data-stu-id="dec29-249">If you are running AzCopy across a low-bandwidth network, you can specify a lower number for --parallel-level tooavoid failure caused by resource competition.</span></span>

[!TIP]
><span data-ttu-id="dec29-250">fullständig lista över tooview hello av AzCopy-parametrar, kolla 'azcopy--Hjälp-menyn.</span><span class="sxs-lookup"><span data-stu-id="dec29-250">tooview hello complete list of AzCopy parameters, check out 'azcopy --help' menu.</span></span>

## <a name="known-issues-and-best-practices"></a><span data-ttu-id="dec29-251">Kända problem och bästa praxis</span><span class="sxs-lookup"><span data-stu-id="dec29-251">Known issues and best practices</span></span>
### <a name="error-net-core-is-not-found-in-hello-system"></a><span data-ttu-id="dec29-252">Fel: .NET Core finns inte i hello system.</span><span class="sxs-lookup"><span data-stu-id="dec29-252">Error: .NET Core is not found in hello system.</span></span>
<span data-ttu-id="dec29-253">Om du får ett felmeddelande om att .NET Core inte är installerat i systemet hello hello SÖKVÄGEN toohello .NET Core binär `dotnet` kanske saknas.</span><span class="sxs-lookup"><span data-stu-id="dec29-253">If you encounter an error stating that .NET Core is not installed in hello system, hello PATH toohello .NET Core binary `dotnet` may be missing.</span></span>

<span data-ttu-id="dec29-254">I ordning tooaddress problemet, hitta hello .NET Core binary i hello system:</span><span class="sxs-lookup"><span data-stu-id="dec29-254">In order tooaddress this issue, find hello .NET Core binary in hello system:</span></span>
```bash
sudo find / -name dotnet
```

<span data-ttu-id="dec29-255">Detta returnerar hello sökvägen toohello dotnet binär.</span><span class="sxs-lookup"><span data-stu-id="dec29-255">This returns hello path toohello dotnet binary.</span></span> 

    /opt/rh/rh-dotnetcore11/root/usr/bin/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/shared/Microsoft.NETCore.App/1.1.2/dotnet

<span data-ttu-id="dec29-256">Nu ska du lägga till den här sökvägen toohello PATH-variabeln.</span><span class="sxs-lookup"><span data-stu-id="dec29-256">Now add this path toohello PATH variable.</span></span> <span data-ttu-id="dec29-257">För sudo, redigera secure_path toocontain hello sökvägen toohello dotnet binära:</span><span class="sxs-lookup"><span data-stu-id="dec29-257">For sudo, edit secure_path toocontain hello path toohello dotnet binary:</span></span>
```bash 
sudo visudo
### Append hello path found in hello preceding example too'secure_path' variable
```

<span data-ttu-id="dec29-258">I det här exemplet läser secure_path variabeln som:</span><span class="sxs-lookup"><span data-stu-id="dec29-258">In this example, secure_path variable reads as:</span></span>

```
secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/opt/rh/rh-dotnetcore11/root/usr/bin/
```

<span data-ttu-id="dec29-259">Redigera.bash_profile/.profile tooinclude hello sökvägen toohello dotnet binära i PATH-variabeln för hello aktuell användare</span><span class="sxs-lookup"><span data-stu-id="dec29-259">For hello current user, edit .bash_profile/.profile tooinclude hello path toohello dotnet binary in PATH variable</span></span> 
```bash
vi ~/.bash_profile
### Append hello path found in hello preceding example too'PATH' variable
```

<span data-ttu-id="dec29-260">Kontrollera att .NET Core finns i SÖKVÄGEN:</span><span class="sxs-lookup"><span data-stu-id="dec29-260">Verify that .NET Core is now in PATH:</span></span>
```bash
which dotnet
sudo which dotnet
```

### <a name="error-installing-azcopy"></a><span data-ttu-id="dec29-261">Fel vid installation av AzCopy</span><span class="sxs-lookup"><span data-stu-id="dec29-261">Error Installing AzCopy</span></span>
<span data-ttu-id="dec29-262">Om du får problem med installation av AzCopy försök toorun AzCopy använda hello bash-skript på hello extraherade `azcopy` mapp.</span><span class="sxs-lookup"><span data-stu-id="dec29-262">If you encounter issues with AzCopy installation, you may try toorun AzCopy using hello bash script in hello extracted `azcopy` folder.</span></span>

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a><span data-ttu-id="dec29-263">Begränsa samtidiga skrivningar vid kopiering av data</span><span class="sxs-lookup"><span data-stu-id="dec29-263">Limit concurrent writes while copying data</span></span>
<span data-ttu-id="dec29-264">När du kopierar blobar eller filer med AzCopy, Tänk på att ett annat program kanske ändra hello data när du kopierar den.</span><span class="sxs-lookup"><span data-stu-id="dec29-264">When you copy blobs or files with AzCopy, keep in mind that another application may be modifying hello data while you are copying it.</span></span> <span data-ttu-id="dec29-265">Kontrollera om det är möjligt att hello data som du kopierar inte ändras under hello kopieringen.</span><span class="sxs-lookup"><span data-stu-id="dec29-265">If possible, ensure that hello data you are copying is not being modified during hello copy operation.</span></span> <span data-ttu-id="dec29-266">Till exempel när du kopierar en virtuell Hårddisk som är kopplad till en virtuell Azure-dator, kontrollera att inga andra program för närvarande skriver toohello VHD.</span><span class="sxs-lookup"><span data-stu-id="dec29-266">For example, when copying a VHD associated with an Azure virtual machine, make sure that no other applications are currently writing toohello VHD.</span></span> <span data-ttu-id="dec29-267">Ett bra sätt toodo denna genom leasing hello resurs toobe kopieras.</span><span class="sxs-lookup"><span data-stu-id="dec29-267">A good way toodo this is by leasing hello resource toobe copied.</span></span> <span data-ttu-id="dec29-268">Alternativt kan du skapa en ögonblicksbild av hello VHD först och sedan kopiera hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="dec29-268">Alternately, you can create a snapshot of hello VHD first and then copy hello snapshot.</span></span>

<span data-ttu-id="dec29-269">Om du inte hindra andra program från att skriva tooblobs eller filer när de kopieras och Kom ihåg att av hello hello jobbet har slutförts kan hello kopiera resurserna inte längre ha fullständig paritet med hello källa resurser.</span><span class="sxs-lookup"><span data-stu-id="dec29-269">If you cannot prevent other applications from writing tooblobs or files while they are being copied, then keep in mind that by hello time hello job finishes, hello copied resources may no longer have full parity with hello source resources.</span></span>

### <a name="run-one-azcopy-instance-on-one-machine"></a><span data-ttu-id="dec29-270">Köra en AzCopy-instans på en dator.</span><span class="sxs-lookup"><span data-stu-id="dec29-270">Run one AzCopy instance on one machine.</span></span>
<span data-ttu-id="dec29-271">AzCopy är utformad toomaximize hello användning av din dator resurs tooaccelerate hello dataöverföring, rekommenderar vi du kör endast en AzCopy-instans på en dator och ange hello alternativet `--parallel-level` om du behöver fler samtidiga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="dec29-271">AzCopy is designed toomaximize hello utilization of your machine resource tooaccelerate hello data transfer, we recommend you run only one AzCopy instance on one machine, and specify hello option `--parallel-level` if you need more concurrent operations.</span></span> <span data-ttu-id="dec29-272">Mer information, Skriv `AzCopy --help parallel-level` hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="dec29-272">For more details, type `AzCopy --help parallel-level` at hello command line.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dec29-273">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dec29-273">Next steps</span></span>
<span data-ttu-id="dec29-274">Mer information om Azure Storage och AzCopy finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="dec29-274">For more information about Azure Storage and AzCopy, see hello following resources:</span></span>

### <a name="azure-storage-documentation"></a><span data-ttu-id="dec29-275">Azure Storage-dokumentation:</span><span class="sxs-lookup"><span data-stu-id="dec29-275">Azure Storage documentation:</span></span>
* [<span data-ttu-id="dec29-276">Introduktion tooAzure lagring</span><span class="sxs-lookup"><span data-stu-id="dec29-276">Introduction tooAzure Storage</span></span>](storage-introduction.md)
* [<span data-ttu-id="dec29-277">Skapa ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="dec29-277">Create a storage account</span></span>](storage-create-storage-account.md)
* [<span data-ttu-id="dec29-278">Hantera blobbar med Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="dec29-278">Manage blobs with Storage Explorer</span></span>](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs)
* [<span data-ttu-id="dec29-279">Med Azure Storage hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="dec29-279">Using hello Azure CLI 2.0 with Azure Storage</span></span>](storage-azure-cli.md)
* [<span data-ttu-id="dec29-280">Hur toouse Blob storage från C++</span><span class="sxs-lookup"><span data-stu-id="dec29-280">How toouse Blob storage from C++</span></span>](storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="dec29-281">Hur toouse Blob storage från Java</span><span class="sxs-lookup"><span data-stu-id="dec29-281">How toouse Blob storage from Java</span></span>](storage-java-how-to-use-blob-storage.md)
* [<span data-ttu-id="dec29-282">Hur toouse Blob storage från Node.js</span><span class="sxs-lookup"><span data-stu-id="dec29-282">How toouse Blob storage from Node.js</span></span>](storage-nodejs-how-to-use-blob-storage.md)
* [<span data-ttu-id="dec29-283">Hur toouse Blob storage från Python</span><span class="sxs-lookup"><span data-stu-id="dec29-283">How toouse Blob storage from Python</span></span>](storage-python-how-to-use-blob-storage.md)

### <a name="azure-storage-blog-posts"></a><span data-ttu-id="dec29-284">Azure Storage blogginlägg:</span><span class="sxs-lookup"><span data-stu-id="dec29-284">Azure Storage blog posts:</span></span>
* [<span data-ttu-id="dec29-285">Om AzCopy på Linux-Preview</span><span class="sxs-lookup"><span data-stu-id="dec29-285">Announcing AzCopy on Linux Preview</span></span>](https://azure.microsoft.com/en-in/blog/announcing-azcopy-on-linux-preview/)
* [<span data-ttu-id="dec29-286">Introduktion till Azure Storage Data Movement Library Preview</span><span class="sxs-lookup"><span data-stu-id="dec29-286">Introducing Azure Storage Data Movement Library Preview</span></span>](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [<span data-ttu-id="dec29-287">AzCopy: Introduktion till synkron kopia och anpassade innehållstyp</span><span class="sxs-lookup"><span data-stu-id="dec29-287">AzCopy: Introducing synchronous copy and customized content type</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [<span data-ttu-id="dec29-288">AzCopy: Om allmän tillgänglighet AzCopy 3.0 plus förhandsversionen av AzCopy 4.0 med tabell- och support</span><span class="sxs-lookup"><span data-stu-id="dec29-288">AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [<span data-ttu-id="dec29-289">AzCopy: Optimerade för storskaliga kopiera scenarier</span><span class="sxs-lookup"><span data-stu-id="dec29-289">AzCopy: Optimized for Large-Scale Copy Scenarios</span></span>](http://go.microsoft.com/fwlink/?LinkId=507682)
* [<span data-ttu-id="dec29-290">AzCopy: Stöd för geo-redundant lagring med läsbehörighet</span><span class="sxs-lookup"><span data-stu-id="dec29-290">AzCopy: Support for read-access geo-redundant storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [<span data-ttu-id="dec29-291">AzCopy: Överföra data med omstartsläge och SAS-token</span><span class="sxs-lookup"><span data-stu-id="dec29-291">AzCopy: Transfer data with restartable mode and SAS token</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [<span data-ttu-id="dec29-292">AzCopy: Använder mellan konto kopiera Blob</span><span class="sxs-lookup"><span data-stu-id="dec29-292">AzCopy: Using cross-account Copy Blob</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [<span data-ttu-id="dec29-293">AzCopy: Överför/hämta filer för Azure-BLOB</span><span class="sxs-lookup"><span data-stu-id="dec29-293">AzCopy: Uploading/downloading files for Azure Blobs</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

