---
title: Kopiera eller flytta data till Azure Storage med AzCopy i Windows | Microsoft Docs
description: "Använd AzCopy på Windows-verktyg för att flytta eller kopiera data till och från blob-, tabell- och innehåll. Kopiera data till Azure Storage från lokala filer eller kopiera data inom eller mellan lagringskonton. Enkelt migrera dina data till Azure Storage."
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
ms.date: 05/14/2017
ms.author: seguler
ms.openlocfilehash: 9806697747b2409d4bd0ae19dc0b9fe01f500dc0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="transfer-data-with-the-azcopy-on-windows"></a><span data-ttu-id="fa97e-105">Överföra data med AzCopy i Windows</span><span class="sxs-lookup"><span data-stu-id="fa97e-105">Transfer data with the AzCopy on Windows</span></span>
<span data-ttu-id="fa97e-106">AzCopy är ett kommandoradsverktyg som utformats för att kopiera data till och från Microsoft Azure Blob-, fil- och Table storage med hjälp av enkla kommandon med optimal prestanda.</span><span class="sxs-lookup"><span data-stu-id="fa97e-106">AzCopy is a command-line utility designed for copying data to and from Microsoft Azure Blob, File, and Table storage using simple commands with optimal performance.</span></span> <span data-ttu-id="fa97e-107">Du kan kopiera data från ett objekt till en annan inom ditt lagringskonto eller mellan lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="fa97e-107">You can copy data from one object to another within your storage account, or between storage accounts.</span></span>

<span data-ttu-id="fa97e-108">Det finns två versioner av AzCopy som du kan hämta.</span><span class="sxs-lookup"><span data-stu-id="fa97e-108">There are two versions of AzCopy that you can download.</span></span> <span data-ttu-id="fa97e-109">AzCopy i Windows har skapats med .NET Framework och erbjuder kommandoradsalternativ för Windows-formatet.</span><span class="sxs-lookup"><span data-stu-id="fa97e-109">AzCopy on Windows is built with .NET Framework, and offers Windows style command-line options.</span></span> <span data-ttu-id="fa97e-110">[AzCopy på Linux](storage-use-azcopy-linux.md) har byggts med .NET Core Framework som riktar sig till Linux-plattformar som erbjuder POSIX format kommandoradsalternativ.</span><span class="sxs-lookup"><span data-stu-id="fa97e-110">[AzCopy on Linux](storage-use-azcopy-linux.md) is built with .NET Core Framework which targets Linux platforms offering POSIX style command-line options.</span></span> <span data-ttu-id="fa97e-111">Den här artikeln beskriver AzCopy i Windows.</span><span class="sxs-lookup"><span data-stu-id="fa97e-111">This article covers AzCopy on Windows.</span></span>

## <a name="download-and-install-azcopy"></a><span data-ttu-id="fa97e-112">Hämta och installera AzCopy</span><span class="sxs-lookup"><span data-stu-id="fa97e-112">Download and install AzCopy</span></span>
### <a name="azcopy-on-windows"></a><span data-ttu-id="fa97e-113">AzCopy i Windows</span><span class="sxs-lookup"><span data-stu-id="fa97e-113">AzCopy on Windows</span></span>
<span data-ttu-id="fa97e-114">Hämta den [senaste versionen av AzCopy på Windows](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="fa97e-114">Download the [latest version of AzCopy on Windows](http://aka.ms/downloadazcopy).</span></span>

#### <a name="installation-on-windows"></a><span data-ttu-id="fa97e-115">Installation på Windows</span><span class="sxs-lookup"><span data-stu-id="fa97e-115">Installation on Windows</span></span>
<span data-ttu-id="fa97e-116">När du har installerat AzCopy i Windows med installationsprogrammet, öppna ett kommandofönster och gå till installationskatalogen för AzCopy på din dator - där den `AzCopy.exe` körbara finns.</span><span class="sxs-lookup"><span data-stu-id="fa97e-116">After installing AzCopy on Windows using the installer, open a command window and navigate to the AzCopy installation directory on your computer - where the `AzCopy.exe` executable is located.</span></span> <span data-ttu-id="fa97e-117">Om du vill kan du lägga till installationsplatsen AzCopy systemsökvägen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-117">If desired, you can add the AzCopy installation location to your system path.</span></span> <span data-ttu-id="fa97e-118">Som standard installeras AzCopy på `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` eller `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="fa97e-118">By default, AzCopy is installed to `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` or `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.</span></span>

## <a name="writing-your-first-azcopy-command"></a><span data-ttu-id="fa97e-119">Skriva ditt första AzCopy-kommandot</span><span class="sxs-lookup"><span data-stu-id="fa97e-119">Writing your first AzCopy command</span></span>
<span data-ttu-id="fa97e-120">Den grundläggande syntaxen för AzCopy kommandon är:</span><span class="sxs-lookup"><span data-stu-id="fa97e-120">The basic syntax for AzCopy commands is:</span></span>

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

<span data-ttu-id="fa97e-121">Följande exempel visar en mängd olika scenarier för att kopiera data till och från Microsoft Azure-Blobbar, filer och tabeller.</span><span class="sxs-lookup"><span data-stu-id="fa97e-121">The following examples demonstrate a variety of scenarios for copying data to and from Microsoft Azure Blobs, Files, and Tables.</span></span> <span data-ttu-id="fa97e-122">Referera till den [AzCopy parametrar](#azcopy-parameters) avsnittet för en detaljerad förklaring av de parametrar som används i varje prov.</span><span class="sxs-lookup"><span data-stu-id="fa97e-122">Refer to the [AzCopy Parameters](#azcopy-parameters) section for a detailed explanation of the parameters used in each sample.</span></span>

## <a name="blob-download"></a><span data-ttu-id="fa97e-123">BLOB: ladda ned</span><span class="sxs-lookup"><span data-stu-id="fa97e-123">Blob: Download</span></span>
### <a name="download-single-blob"></a><span data-ttu-id="fa97e-124">Hämta en enda blob</span><span class="sxs-lookup"><span data-stu-id="fa97e-124">Download single blob</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

<span data-ttu-id="fa97e-125">Observera att om mappen `C:\myfolder` finns inte, AzCopy skapar den och ladda ned `abc.txt ` till den nya mappen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-125">Note that if the folder `C:\myfolder` does not exist, AzCopy creates it and download `abc.txt ` into the new folder.</span></span>

### <a name="download-single-blob-from-secondary-region"></a><span data-ttu-id="fa97e-126">Hämta en enda blob från sekundär region</span><span class="sxs-lookup"><span data-stu-id="fa97e-126">Download single blob from secondary region</span></span>

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

<span data-ttu-id="fa97e-127">Observera att du måste ha läsbehörighet geo-redundant lagring aktiverad.</span><span class="sxs-lookup"><span data-stu-id="fa97e-127">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="download-all-blobs"></a><span data-ttu-id="fa97e-128">Hämta alla BLOB</span><span class="sxs-lookup"><span data-stu-id="fa97e-128">Download all blobs</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

<span data-ttu-id="fa97e-129">Anta följande blobbar finns i den angivna behållaren:</span><span class="sxs-lookup"><span data-stu-id="fa97e-129">Assume the following blobs reside in the specified container:</span></span>  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

<span data-ttu-id="fa97e-130">När hämtningen katalogen `C:\myfolder` innehåller följande filer:</span><span class="sxs-lookup"><span data-stu-id="fa97e-130">After the download operation, the directory `C:\myfolder` includes the following files:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

<span data-ttu-id="fa97e-131">Om du inte anger alternativet `/S`, hämtas inga blobbar.</span><span class="sxs-lookup"><span data-stu-id="fa97e-131">If you do not specify option `/S`, no blobs are downloaded.</span></span>

### <a name="download-blobs-with-specified-prefix"></a><span data-ttu-id="fa97e-132">Ladda ned blobbar med angivna prefix</span><span class="sxs-lookup"><span data-stu-id="fa97e-132">Download blobs with specified prefix</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

<span data-ttu-id="fa97e-133">Anta att följande blobbar finns i den angivna behållaren.</span><span class="sxs-lookup"><span data-stu-id="fa97e-133">Assume the following blobs reside in the specified container.</span></span> <span data-ttu-id="fa97e-134">Alla blobbar som börjar med prefixet `a` hämtas:</span><span class="sxs-lookup"><span data-stu-id="fa97e-134">All blobs beginning with the prefix `a` are downloaded:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

<span data-ttu-id="fa97e-135">När hämtningen mappen `C:\myfolder` innehåller följande filer:</span><span class="sxs-lookup"><span data-stu-id="fa97e-135">After the download operation, the folder `C:\myfolder` includes the following files:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

<span data-ttu-id="fa97e-136">Prefixet som gäller för den virtuella katalogen, som utgör den första delen av blobbnamnet.</span><span class="sxs-lookup"><span data-stu-id="fa97e-136">The prefix applies to the virtual directory, which forms the first part of the blob name.</span></span> <span data-ttu-id="fa97e-137">I exemplet ovan, matchar den virtuella katalogen inte det angivna prefixet så inte laddas ned.</span><span class="sxs-lookup"><span data-stu-id="fa97e-137">In the example shown above, the virtual directory does not match the specified prefix, so it is not downloaded.</span></span> <span data-ttu-id="fa97e-138">Dessutom, om alternativet `\S` anges AzCopy inte hämta alla blobbar.</span><span class="sxs-lookup"><span data-stu-id="fa97e-138">In addition, if the option `\S` is not specified, AzCopy does not download any blobs.</span></span>

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a><span data-ttu-id="fa97e-139">Ange last-modified-tid med exporterade filer som ska vara densamma som källan BLOB</span><span class="sxs-lookup"><span data-stu-id="fa97e-139">Set the last-modified time of exported files to be same as the source blobs</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

<span data-ttu-id="fa97e-140">Du kan också utesluta blobbar från hämtningen baserat på deras tid för senaste ändring.</span><span class="sxs-lookup"><span data-stu-id="fa97e-140">You can also exclude blobs from the download operation based on their last-modified time.</span></span> <span data-ttu-id="fa97e-141">Till exempel om du vill undanta blobbar vars senast ändrad är samma som eller nyare än målfilen och lägga till den `/XN` alternativ:</span><span class="sxs-lookup"><span data-stu-id="fa97e-141">For example, if you want to exclude blobs whose last modified time is the same or newer than the destination file, add the `/XN` option:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

<span data-ttu-id="fa97e-142">Eller om du vill undanta blobbar vars senast ändrad är samma eller äldre än målfilen och lägga till den `/XO` alternativ:</span><span class="sxs-lookup"><span data-stu-id="fa97e-142">Or if you want to exclude blobs whose last modified time is the same or older than the destination file, add the `/XO` option:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="blob-upload"></a><span data-ttu-id="fa97e-143">BLOB: Överför</span><span class="sxs-lookup"><span data-stu-id="fa97e-143">Blob: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="fa97e-144">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="fa97e-144">Upload single file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

<span data-ttu-id="fa97e-145">Om den angivna Målbehållaren inte finns, skapar den AzCopy och överför filen till den.</span><span class="sxs-lookup"><span data-stu-id="fa97e-145">If the specified destination container does not exist, AzCopy creates it and uploads the file into it.</span></span>

### <a name="upload-single-file-to-virtual-directory"></a><span data-ttu-id="fa97e-146">Överför en fil till virtuell katalog</span><span class="sxs-lookup"><span data-stu-id="fa97e-146">Upload single file to virtual directory</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="fa97e-147">Om den angivna virtuella katalogen inte finns, AzCopy överför filen så att den virtuella katalogen i sitt namn (*t.ex.*, `vd/abc.txt` i exemplet ovan).</span><span class="sxs-lookup"><span data-stu-id="fa97e-147">If the specified virtual directory does not exist, AzCopy uploads the file to include the virtual directory in its name (*e.g.*, `vd/abc.txt` in the example above).</span></span>

### <a name="upload-all-files"></a><span data-ttu-id="fa97e-148">Ladda upp alla filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-148">Upload all files</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

<span data-ttu-id="fa97e-149">Om du anger alternativet `/S` Överför innehållet i den angivna katalogen till Blob storage rekursivt, vilket innebär att alla undermappar och filer överförs också.</span><span class="sxs-lookup"><span data-stu-id="fa97e-149">Specifying option `/S` uploads the contents of the specified directory to Blob storage recursively, meaning that all subfolders and their files are uploaded as well.</span></span> <span data-ttu-id="fa97e-150">Anta exempelvis att följande filer finns i mappen `C:\myfolder`:</span><span class="sxs-lookup"><span data-stu-id="fa97e-150">For instance, assume the following files reside in folder `C:\myfolder`:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

<span data-ttu-id="fa97e-151">När överföringen innehåller behållaren följande filer:</span><span class="sxs-lookup"><span data-stu-id="fa97e-151">After the upload operation, the container includes the following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

<span data-ttu-id="fa97e-152">Om du inte anger alternativet `/S`, AzCopy inte överföra rekursivt.</span><span class="sxs-lookup"><span data-stu-id="fa97e-152">If you do not specify option `/S`, AzCopy does not upload recursively.</span></span> <span data-ttu-id="fa97e-153">När överföringen innehåller behållaren följande filer:</span><span class="sxs-lookup"><span data-stu-id="fa97e-153">After the upload operation, the container includes the following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="fa97e-154">Överföra filer som matchar angivna mönstret</span><span class="sxs-lookup"><span data-stu-id="fa97e-154">Upload files matching specified pattern</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

<span data-ttu-id="fa97e-155">Anta att följande filer finns i mappen `C:\myfolder`:</span><span class="sxs-lookup"><span data-stu-id="fa97e-155">Assume the following files reside in folder `C:\myfolder`:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

<span data-ttu-id="fa97e-156">När överföringen innehåller behållaren följande filer:</span><span class="sxs-lookup"><span data-stu-id="fa97e-156">After the upload operation, the container includes the following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

<span data-ttu-id="fa97e-157">Om du inte anger alternativet `/S`, AzCopy Överför bara blob som inte finns i en virtuell katalog:</span><span class="sxs-lookup"><span data-stu-id="fa97e-157">If you do not specify option `/S`, AzCopy only uploads blobs that don't reside in a virtual directory:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a><span data-ttu-id="fa97e-158">Ange MIME content-type för en mål-blob</span><span class="sxs-lookup"><span data-stu-id="fa97e-158">Specify the MIME content type of a destination blob</span></span>
<span data-ttu-id="fa97e-159">Som standard anges AzCopy innehållstypen för en mål-blob till `application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="fa97e-159">By default, AzCopy sets the content type of a destination blob to `application/octet-stream`.</span></span> <span data-ttu-id="fa97e-160">Från och med version 3.1.0, du kan uttryckligen ange content-type via alternativet `/SetContentType:[content-type]`.</span><span class="sxs-lookup"><span data-stu-id="fa97e-160">Beginning with version 3.1.0, you can explicitly specify the content type via the option `/SetContentType:[content-type]`.</span></span> <span data-ttu-id="fa97e-161">Den här syntaxen anger content-type för alla blobbar i en överföringen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-161">This syntax sets the content type for all blobs in an upload operation.</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

<span data-ttu-id="fa97e-162">Om du anger `/SetContentType` utan värde, AzCopy anger varje blob eller filens innehållstyp enligt dess filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="fa97e-162">If you specify `/SetContentType` without a value, AzCopy sets each blob or file's content type according to its file extension.</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="blob-copy"></a><span data-ttu-id="fa97e-163">BLOB: kopiera</span><span class="sxs-lookup"><span data-stu-id="fa97e-163">Blob: Copy</span></span>
### <a name="copy-single-blob-within-storage-account"></a><span data-ttu-id="fa97e-164">Kopiera enda blob inom lagringskonto</span><span class="sxs-lookup"><span data-stu-id="fa97e-164">Copy single blob within Storage account</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="fa97e-165">När du kopierar en blobb inom ett lagringskonto, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="fa97e-165">When you copy a blob within a Storage account, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-across-storage-accounts"></a><span data-ttu-id="fa97e-166">Kopiera enda blob på Storage-konton</span><span class="sxs-lookup"><span data-stu-id="fa97e-166">Copy single blob across Storage accounts</span></span>

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

<span data-ttu-id="fa97e-167">När du kopierar en blobb mellan lagringskonton, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="fa97e-167">When you copy a blob across Storage accounts, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a><span data-ttu-id="fa97e-168">Kopiera enda blob från sekundär region till primär region</span><span class="sxs-lookup"><span data-stu-id="fa97e-168">Copy single blob from secondary region to primary region</span></span>

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

<span data-ttu-id="fa97e-169">Observera att du måste ha läsbehörighet geo-redundant lagring aktiverad.</span><span class="sxs-lookup"><span data-stu-id="fa97e-169">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a><span data-ttu-id="fa97e-170">Kopiera enda blob och dess ögonblicksbilder på Storage-konton</span><span class="sxs-lookup"><span data-stu-id="fa97e-170">Copy single blob and its snapshots across Storage accounts</span></span>

```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

<span data-ttu-id="fa97e-171">Efter kopieringen innehåller Målbehållaren blob och dess ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="fa97e-171">After the copy operation, the target container includes the blob and its snapshots.</span></span> <span data-ttu-id="fa97e-172">Under förutsättning att blob i exemplet ovan har två ögonblicksbilder, innehåller behållaren följande blob och ögonblicksbilder:</span><span class="sxs-lookup"><span data-stu-id="fa97e-172">Assuming the blob in the example above has two snapshots, the container includes the following blob and snapshots:</span></span>

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a><span data-ttu-id="fa97e-173">Kopiera synkront blobar på lagringskonton</span><span class="sxs-lookup"><span data-stu-id="fa97e-173">Synchronously copy blobs across Storage accounts</span></span>
<span data-ttu-id="fa97e-174">AzCopy som standard kopierar data mellan två slutpunkter för lagring asynkront.</span><span class="sxs-lookup"><span data-stu-id="fa97e-174">AzCopy by default copies data between two storage endpoints asynchronously.</span></span> <span data-ttu-id="fa97e-175">Därför körs kopieringen bakgrunden med hjälp av ledig bandbreddskapacitet som har inga SLA vad gäller hur snabbt en blob kopieras och AzCopy söker regelbundet kopian status tills kopiering är slutförd eller misslyckad.</span><span class="sxs-lookup"><span data-stu-id="fa97e-175">Therefore, the copy operation runs in the background using spare bandwidth capacity that has no SLA in terms of how fast a blob is copied, and AzCopy periodically checks the copy status until the copying is completed or failed.</span></span>

<span data-ttu-id="fa97e-176">Den `/SyncCopy` alternativet ser du till att kopieringen hämtar konsekvent hastighet.</span><span class="sxs-lookup"><span data-stu-id="fa97e-176">The `/SyncCopy` option ensures that the copy operation gets consistent speed.</span></span> <span data-ttu-id="fa97e-177">AzCopy utför synkron kopian genom att hämta blobbarna att kopiera från den angivna källan till lokalt minne och överför dem till Blob-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="fa97e-177">AzCopy performs the synchronous copy by downloading the blobs to copy from the specified source to local memory, and then uploading them to the Blob storage destination.</span></span>

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

<span data-ttu-id="fa97e-178">`/SyncCopy`kan skapa ytterligare utgång kostnaden jämfört med asynkron kopia, den rekommenderade metoden är att använda det här alternativet i en virtuell Azure-dator som är i samma region som ditt källa storage-konto för att undvika kostnader för utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="fa97e-178">`/SyncCopy` might generate additional egress cost compared to asynchronous copy, the recommended approach is to use this option in an Azure VM that is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="file-download"></a><span data-ttu-id="fa97e-179">Fil: ladda ned</span><span class="sxs-lookup"><span data-stu-id="fa97e-179">File: Download</span></span>
### <a name="download-single-file"></a><span data-ttu-id="fa97e-180">Hämta en fil</span><span class="sxs-lookup"><span data-stu-id="fa97e-180">Download single file</span></span>

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

<span data-ttu-id="fa97e-181">Om den angivna källan är en Azure-filresurs och sedan måste du antingen ange det exakta filnamnet (*t.ex.* `abc.txt`) att hämta en fil eller ange alternativet `/S` att ladda ned alla filer i resursen rekursivt.</span><span class="sxs-lookup"><span data-stu-id="fa97e-181">If the specified source is an Azure file share, then you must either specify the exact file name, (*e.g.* `abc.txt`) to download a single file, or specify option `/S` to download all files in the share recursively.</span></span> <span data-ttu-id="fa97e-182">Försök att ange både filmönstret och alternativet `/S` tillsammans resulterar i ett fel.</span><span class="sxs-lookup"><span data-stu-id="fa97e-182">Attempting to specify both a file pattern and option `/S` together results in an error.</span></span>

### <a name="download-all-files"></a><span data-ttu-id="fa97e-183">Hämta alla filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-183">Download all files</span></span>

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

<span data-ttu-id="fa97e-184">Observera att alla tomma mappar inte laddas ned.</span><span class="sxs-lookup"><span data-stu-id="fa97e-184">Note that any empty folders are not downloaded.</span></span>

## <a name="file-upload"></a><span data-ttu-id="fa97e-185">Fil: Överför</span><span class="sxs-lookup"><span data-stu-id="fa97e-185">File: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="fa97e-186">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="fa97e-186">Upload single file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files"></a><span data-ttu-id="fa97e-187">Ladda upp alla filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-187">Upload all files</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

<span data-ttu-id="fa97e-188">Observera att alla tomma mappar inte upp.</span><span class="sxs-lookup"><span data-stu-id="fa97e-188">Note that any empty folders are not uploaded.</span></span>

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="fa97e-189">Överföra filer som matchar angivna mönstret</span><span class="sxs-lookup"><span data-stu-id="fa97e-189">Upload files matching specified pattern</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="file-copy"></a><span data-ttu-id="fa97e-190">Fil: kopiera</span><span class="sxs-lookup"><span data-stu-id="fa97e-190">File: Copy</span></span>
### <a name="copy-across-file-shares"></a><span data-ttu-id="fa97e-191">Kopiera på filresurser</span><span class="sxs-lookup"><span data-stu-id="fa97e-191">Copy across file shares</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="fa97e-192">När du kopierar en fil på filresurser, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="fa97e-192">When you copy a file across file shares, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-file-share-to-blob"></a><span data-ttu-id="fa97e-193">Kopiera från filresursen till blob</span><span class="sxs-lookup"><span data-stu-id="fa97e-193">Copy from file share to blob</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="fa97e-194">När du kopierar en fil från filresursen till blob, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="fa97e-194">When you copy a file from file share to blob, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>


### <a name="copy-from-blob-to-file-share"></a><span data-ttu-id="fa97e-195">Kopiera från blob till filresurs</span><span class="sxs-lookup"><span data-stu-id="fa97e-195">Copy from blob to file share</span></span>

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="fa97e-196">När du kopierar en fil från blob till filresurs, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="fa97e-196">When you copy a file from blob to file share, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="synchronously-copy-files"></a><span data-ttu-id="fa97e-197">Synkront kopiera filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-197">Synchronously copy files</span></span>
<span data-ttu-id="fa97e-198">Du kan ange den `/SyncCopy` alternativet för att kopiera data från fillagring för lagring, lagring av filer till Blob Storage och Blob Storage till File Storage synkront, AzCopy gör detta genom att hämta källdata till lokalt minne och ladda upp den igen till mål.</span><span class="sxs-lookup"><span data-stu-id="fa97e-198">You can specify the `/SyncCopy` option to copy data from File Storage to File Storage, from File Storage to Blob Storage and from Blob Storage to File Storage synchronously, AzCopy does this by downloading the source data to local memory and upload it again to destination.</span></span> <span data-ttu-id="fa97e-199">Standard utgång kostnaden gäller.</span><span class="sxs-lookup"><span data-stu-id="fa97e-199">Standard egress cost applies.</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

<span data-ttu-id="fa97e-200">När du kopierar från fillagring till Blob Storage blob standardtypen är blockblob, kan användaren ange alternativet `/BlobType:page` att ändra typen av mål-blob.</span><span class="sxs-lookup"><span data-stu-id="fa97e-200">When copying from File Storage to Blob Storage, the default blob type is block blob, user can specify option `/BlobType:page` to change the destination blob type.</span></span>

<span data-ttu-id="fa97e-201">Observera att `/SyncCopy` kan skapa ytterligare utgång kostnad jämföra den asynkrona kopia, den rekommenderade metoden är att använda det här alternativet i den virtuella Azure-datorn som är i samma region som ditt källa storage-konto för att undvika kostnader för utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="fa97e-201">Note that `/SyncCopy` might generate additional egress cost comparing to asynchronous copy, the recommended approach is to use this option in the Azure VM which is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="table-export"></a><span data-ttu-id="fa97e-202">Tabell: exportera</span><span class="sxs-lookup"><span data-stu-id="fa97e-202">Table: Export</span></span>
### <a name="export-table"></a><span data-ttu-id="fa97e-203">Exportera tabell</span><span class="sxs-lookup"><span data-stu-id="fa97e-203">Export table</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

<span data-ttu-id="fa97e-204">AzCopy skriver en manifestfil till angiven målmapp.</span><span class="sxs-lookup"><span data-stu-id="fa97e-204">AzCopy writes a manifest file to the specified destination folder.</span></span> <span data-ttu-id="fa97e-205">Manifestfilen används i importen för att hitta de nödvändiga filerna och utföra dataverifiering.</span><span class="sxs-lookup"><span data-stu-id="fa97e-205">The manifest file is used in the import process to locate the necessary data files and perform data validation.</span></span> <span data-ttu-id="fa97e-206">Manifestfilen använder följande namngivningskonvention som standard:</span><span class="sxs-lookup"><span data-stu-id="fa97e-206">The manifest file uses the following naming convention by default:</span></span>

    <account name>_<table name>_<timestamp>.manifest

<span data-ttu-id="fa97e-207">Användaren kan även ange alternativet `/Manifest:<manifest file name>` att ange namnet på manifestfilen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-207">User can also specify the option `/Manifest:<manifest file name>` to set the manifest file name.</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-export-into-multiple-files"></a><span data-ttu-id="fa97e-208">Dela export i flera filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-208">Split export into multiple files</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

<span data-ttu-id="fa97e-209">AzCopy använder en *volymindex* i filnamnen dela data att skilja flera filer.</span><span class="sxs-lookup"><span data-stu-id="fa97e-209">AzCopy uses a *volume index* in the split data file names to distinguish multiple files.</span></span> <span data-ttu-id="fa97e-210">Volymindexet består av två delar, en *partition viktiga områdesindex* och en *dela filen index*.</span><span class="sxs-lookup"><span data-stu-id="fa97e-210">The volume index consists of two parts, a *partition key range index* and a *split file index*.</span></span> <span data-ttu-id="fa97e-211">Både index är nollbaserade.</span><span class="sxs-lookup"><span data-stu-id="fa97e-211">Both indexes are zero-based.</span></span>

<span data-ttu-id="fa97e-212">Index för partitionen viktiga intervallet är 0 om användaren inte anger alternativet `/PKRS`.</span><span class="sxs-lookup"><span data-stu-id="fa97e-212">The partition key range index is 0 if the user does not specify option `/PKRS`.</span></span>

<span data-ttu-id="fa97e-213">Anta exempelvis AzCopy genererar två datafiler när användaren anger alternativet `/SplitSize`.</span><span class="sxs-lookup"><span data-stu-id="fa97e-213">For instance, suppose AzCopy generates two data files after the user specifies option `/SplitSize`.</span></span> <span data-ttu-id="fa97e-214">De resulterande filnamn data kan vara:</span><span class="sxs-lookup"><span data-stu-id="fa97e-214">The resulting data file names might be:</span></span>

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

<span data-ttu-id="fa97e-215">Observera att minsta möjliga värde för alternativet `/SplitSize` 32 MB.</span><span class="sxs-lookup"><span data-stu-id="fa97e-215">Note that the minimum possible value for option `/SplitSize` is 32MB.</span></span> <span data-ttu-id="fa97e-216">Om det angivna målet är Blob storage, AzCopy delar upp datafilen när dess storlek når storleksbegränsning för blob (200GB), oavsett om alternativet `/SplitSize` har angetts av användaren.</span><span class="sxs-lookup"><span data-stu-id="fa97e-216">If the specified destination is Blob storage, AzCopy splits the data file once its sizes reaches the blob size limitation (200GB), regardless of whether option `/SplitSize` has been specified by the user.</span></span>

### <a name="export-table-to-json-or-csv-data-file-format"></a><span data-ttu-id="fa97e-217">Exportera tabellen till JSON eller CSV-filformat för data</span><span class="sxs-lookup"><span data-stu-id="fa97e-217">Export table to JSON or CSV data file format</span></span>
<span data-ttu-id="fa97e-218">AzCopy som standard exporterar tabeller till datafiler i JSON.</span><span class="sxs-lookup"><span data-stu-id="fa97e-218">AzCopy by default exports tables to JSON data files.</span></span> <span data-ttu-id="fa97e-219">Du kan ange alternativet `/PayloadFormat:JSON|CSV` att exportera tabellerna som JSON- eller CSV.</span><span class="sxs-lookup"><span data-stu-id="fa97e-219">You can specify the option `/PayloadFormat:JSON|CSV` to export the tables as JSON or CSV.</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

<span data-ttu-id="fa97e-220">När du anger nyttolastformatet CSV, AzCopy också genererar en schemafil med filnamnstillägget `.schema.csv` för varje datafil.</span><span class="sxs-lookup"><span data-stu-id="fa97e-220">When specifying the CSV payload format, AzCopy also generates a schema file with file extension `.schema.csv` for each data file.</span></span>

### <a name="export-table-entities-concurrently"></a><span data-ttu-id="fa97e-221">Exportera tabellentiteter samtidigt</span><span class="sxs-lookup"><span data-stu-id="fa97e-221">Export table entities concurrently</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

<span data-ttu-id="fa97e-222">AzCopy startar samtidiga åtgärder om du vill exportera entiteter när användaren anger alternativet `/PKRS`.</span><span class="sxs-lookup"><span data-stu-id="fa97e-222">AzCopy starts concurrent operations to export entities when the user specifies option `/PKRS`.</span></span> <span data-ttu-id="fa97e-223">Varje åtgärd exporterar en partitionsnyckelintervallet.</span><span class="sxs-lookup"><span data-stu-id="fa97e-223">Each operation exports one partition key range.</span></span>

<span data-ttu-id="fa97e-224">Observera att antalet samtidiga åtgärder också styrs av alternativet `/NC`.</span><span class="sxs-lookup"><span data-stu-id="fa97e-224">Note that the number of concurrent operations is also controlled by option `/NC`.</span></span> <span data-ttu-id="fa97e-225">AzCopy använder antalet kärnprocessorer som standardvärdet `/NC` när du kopierar tabellen enheter, även om `/NC` har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="fa97e-225">AzCopy uses the number of core processors as the default value of `/NC` when copying table entities, even if `/NC` was not specified.</span></span> <span data-ttu-id="fa97e-226">När användaren anger alternativet `/PKRS`, AzCopy använder mindre av de två värdena - partition nyckelintervall jämfört med implicit eller explicit anges samtidiga åtgärder – ta reda på antalet samtidiga åtgärder att starta.</span><span class="sxs-lookup"><span data-stu-id="fa97e-226">When the user specifies option `/PKRS`, AzCopy uses the smaller of the two values - partition key ranges versus implicitly or explicitly specified concurrent operations - to determine the number of concurrent operations to start.</span></span> <span data-ttu-id="fa97e-227">Mer information skriver du `AzCopy /?:NC` på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="fa97e-227">For more details, type `AzCopy /?:NC` at the command line.</span></span>

### <a name="export-table-to-blob"></a><span data-ttu-id="fa97e-228">Exportera tabell till blob</span><span class="sxs-lookup"><span data-stu-id="fa97e-228">Export table to blob</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

<span data-ttu-id="fa97e-229">AzCopy genererar en JSON-fil för data i blob-behållaren med följande namngivningskonvention:</span><span class="sxs-lookup"><span data-stu-id="fa97e-229">AzCopy generates a JSON data file into the blob container with following naming convention:</span></span>

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

<span data-ttu-id="fa97e-230">Den genererade JSON-datafilen följer nyttolastformat för minimal metadata.</span><span class="sxs-lookup"><span data-stu-id="fa97e-230">The generated JSON data file follows the payload format for minimal metadata.</span></span> <span data-ttu-id="fa97e-231">Mer information om den här nyttolastformatet finns [Nyttolastformat för tabellen tjänståtgärder](http://msdn.microsoft.com/library/azure/dn535600.aspx).</span><span class="sxs-lookup"><span data-stu-id="fa97e-231">For details on this payload format, see [Payload Format for Table Service Operations](http://msdn.microsoft.com/library/azure/dn535600.aspx).</span></span>

<span data-ttu-id="fa97e-232">Observera att när du exporterar tabeller till blobbar, AzCopy hämtar tabellentiteter till lokala temporära datafiler och sedan överför dessa enheter till blob.</span><span class="sxs-lookup"><span data-stu-id="fa97e-232">Note that when exporting tables to blobs, AzCopy downloads the Table entities to local temporary data files and then uploads those entities to the blob.</span></span> <span data-ttu-id="fa97e-233">Dessa temporära datafiler placeras i mappen journal filen med standardsökvägen ”<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>”, du kan ange alternativet/Z: [journal-filmapp] att ändra journalen filen mapp och därmed ändra platsen för tillfälliga data-filer.</span><span class="sxs-lookup"><span data-stu-id="fa97e-233">These temporary data files are put into the journal file folder with the default path "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", you can specify option /Z:[journal-file-folder] to change the journal file folder location and thus change the temporary data files location.</span></span> <span data-ttu-id="fa97e-234">Tillfällig data filernas storlek bestäms av din tabellentiteter och storlek som du har angett med alternativet-/SplitSize även om den tillfälliga datafilen i lokal disk tas bort omedelbart när den har överförts till blob, kontrollera att du har tillräckligt med lokala diskutrymme för att lagra filerna tillfälliga data innan de tas bort.</span><span class="sxs-lookup"><span data-stu-id="fa97e-234">The temporary data files' size is decided by your table entities' size and the size you specified with the option /SplitSize, although the temporary data file in local disk is deleted instantly once it has been uploaded to the blob, please make sure you have enough local disk space to store these temporary data files before they are deleted.</span></span>

## <a name="table-import"></a><span data-ttu-id="fa97e-235">Tabell: Import</span><span class="sxs-lookup"><span data-stu-id="fa97e-235">Table: Import</span></span>
### <a name="import-table"></a><span data-ttu-id="fa97e-236">Import av tabell</span><span class="sxs-lookup"><span data-stu-id="fa97e-236">Import table</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

<span data-ttu-id="fa97e-237">Alternativet `/EntityOperation` visar hur du infogar entiteter i tabellen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-237">The option `/EntityOperation` indicates how to insert entities into the table.</span></span> <span data-ttu-id="fa97e-238">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="fa97e-238">Possible values are:</span></span>

* <span data-ttu-id="fa97e-239">`InsertOrSkip`: Hoppar över en befintlig entitet eller infogar en ny entitet om den inte finns i tabellen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-239">`InsertOrSkip`: Skips an existing entity or inserts a new entity if it does not exist in the table.</span></span>
* <span data-ttu-id="fa97e-240">`InsertOrMerge`: Sammanfogar en befintlig entitet eller infogar en ny entitet om den inte finns i tabellen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-240">`InsertOrMerge`: Merges an existing entity or inserts a new entity if it does not exist in the table.</span></span>
* <span data-ttu-id="fa97e-241">`InsertOrReplace`: Ersätter en befintlig entitet eller infogar en ny entitet om den inte finns i tabellen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-241">`InsertOrReplace`: Replaces an existing entity or inserts a new entity if it does not exist in the table.</span></span>

<span data-ttu-id="fa97e-242">Observera att du inte kan ange alternativet `/PKRS` i import-scenariot.</span><span class="sxs-lookup"><span data-stu-id="fa97e-242">Note that you cannot specify option `/PKRS` in the import scenario.</span></span> <span data-ttu-id="fa97e-243">Till skillnad från export-scenariot, där du måste ange alternativet `/PKRS` om du vill starta samtidiga åtgärder AzCopy samtidiga åtgärder som standard startar när du importerar en tabell.</span><span class="sxs-lookup"><span data-stu-id="fa97e-243">Unlike the export scenario, in which you must specify option `/PKRS` to start concurrent operations, AzCopy starts concurrent operations by default when you import a table.</span></span> <span data-ttu-id="fa97e-244">Standardvärdet för antalet samtidiga åtgärder igång är lika med antalet kärnprocessorer; Du kan dock ange ett annat antal samtidiga med alternativet `/NC`.</span><span class="sxs-lookup"><span data-stu-id="fa97e-244">The default number of concurrent operations started is equal to the number of core processors; however, you can specify a different number of concurrent with option `/NC`.</span></span> <span data-ttu-id="fa97e-245">Mer information skriver du `AzCopy /?:NC` på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="fa97e-245">For more details, type `AzCopy /?:NC` at the command line.</span></span>

<span data-ttu-id="fa97e-246">Observera att AzCopy endast stöder import för JSON inte CSV.</span><span class="sxs-lookup"><span data-stu-id="fa97e-246">Note that AzCopy only supports importing for JSON, not CSV.</span></span> <span data-ttu-id="fa97e-247">AzCopy har inte stöd för import av tabell från användarskapade JSON och manifest filer.</span><span class="sxs-lookup"><span data-stu-id="fa97e-247">AzCopy does not support table imports from user-created JSON and manifest files.</span></span> <span data-ttu-id="fa97e-248">Båda dessa filer måste komma från en tabell AzCopy-export.</span><span class="sxs-lookup"><span data-stu-id="fa97e-248">Both of these files must come from an AzCopy table export.</span></span> <span data-ttu-id="fa97e-249">För att undvika fel kan du inte ändra exporterade JSON eller manifestfil.</span><span class="sxs-lookup"><span data-stu-id="fa97e-249">To avoid errors, please do not modify the exported JSON or manifest file.</span></span>

### <a name="import-entities-to-table-using-blobs"></a><span data-ttu-id="fa97e-250">Importera entiteter i tabellen med blobbar</span><span class="sxs-lookup"><span data-stu-id="fa97e-250">Import entities to table using blobs</span></span>
<span data-ttu-id="fa97e-251">Anta en Blob-behållare innehåller följande: en JSON-fil som representerar en Azure-tabellen och dess tillhörande manifestfilen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-251">Assume a Blob container contains the following: A JSON file representing an Azure Table and its accompanying manifest file.</span></span>

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

<span data-ttu-id="fa97e-252">Du kan köra följande kommando för att importera entiteter i en tabell med manifestfilen i den blob-behållaren:</span><span class="sxs-lookup"><span data-stu-id="fa97e-252">You can run the following command to import entities into a table using the manifest file in that blob container:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a><span data-ttu-id="fa97e-253">Andra AzCopy-funktioner</span><span class="sxs-lookup"><span data-stu-id="fa97e-253">Other AzCopy features</span></span>
### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a><span data-ttu-id="fa97e-254">Endast kopiera data som inte finns i målet</span><span class="sxs-lookup"><span data-stu-id="fa97e-254">Only copy data that doesn't exist in the destination</span></span>
<span data-ttu-id="fa97e-255">Den `/XO` och `/XN` parametrar kan du exkludera resurser som tidigare eller senare från att kopieras respektive.</span><span class="sxs-lookup"><span data-stu-id="fa97e-255">The `/XO` and `/XN` parameters allow you to exclude older or newer source resources from being copied, respectively.</span></span> <span data-ttu-id="fa97e-256">Om du bara vill kopiera käll-resurser som inte finns i målet kan du ange båda parametrarna i AzCopy-kommandot:</span><span class="sxs-lookup"><span data-stu-id="fa97e-256">If you only want to copy source resources that don't exist in the destination, you can specify both parameters in the AzCopy command:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

<span data-ttu-id="fa97e-257">Observera att detta inte stöds när källan eller målet är en tabell.</span><span class="sxs-lookup"><span data-stu-id="fa97e-257">Note that this is not supported when either the source or destination is a table.</span></span>

### <a name="use-a-response-file-to-specify-command-line-parameters"></a><span data-ttu-id="fa97e-258">Använda en svarsfil för att ange kommandoradsparametrar</span><span class="sxs-lookup"><span data-stu-id="fa97e-258">Use a response file to specify command-line parameters</span></span>

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

<span data-ttu-id="fa97e-259">Du kan inkludera eventuella kommandoradsparametrar för AzCopy i en svarsfil.</span><span class="sxs-lookup"><span data-stu-id="fa97e-259">You can include any AzCopy command-line parameters in a response file.</span></span> <span data-ttu-id="fa97e-260">AzCopy bearbetar parametrarna i filen som om de hade angetts på kommandoraden, utför en direkt ersättning med innehållet i filen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-260">AzCopy processes the parameters in the file as if they had been specified on the command line, performing a direct substitution with the contents of the file.</span></span>

<span data-ttu-id="fa97e-261">Anta en svarsfil med namnet `copyoperation.txt`, som innehåller följande rader.</span><span class="sxs-lookup"><span data-stu-id="fa97e-261">Assume a response file named `copyoperation.txt`, that contains the following lines.</span></span> <span data-ttu-id="fa97e-262">Varje AzCopy-parameter kan anges på en enda rad</span><span class="sxs-lookup"><span data-stu-id="fa97e-262">Each AzCopy parameter can be specified on a single line</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

<span data-ttu-id="fa97e-263">eller på separata rader:</span><span class="sxs-lookup"><span data-stu-id="fa97e-263">or on separate lines:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

<span data-ttu-id="fa97e-264">AzCopy misslyckas om du vill dela parametern på två rader som visas här för den `/sourcekey` parameter:</span><span class="sxs-lookup"><span data-stu-id="fa97e-264">AzCopy fails if you split the parameter across two lines, as shown here for the `/sourcekey` parameter:</span></span>

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a><span data-ttu-id="fa97e-265">Använda flera svarsfiler för att ange kommandoradsparametrar</span><span class="sxs-lookup"><span data-stu-id="fa97e-265">Use multiple response files to specify command-line parameters</span></span>
<span data-ttu-id="fa97e-266">Anta en svarsfil med namnet `source.txt` som anger en käll-behållare:</span><span class="sxs-lookup"><span data-stu-id="fa97e-266">Assume a response file named `source.txt` that specifies a source container:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer

<span data-ttu-id="fa97e-267">Och en svarsfil med namnet `dest.txt` som anger en målmapp i filsystemet:</span><span class="sxs-lookup"><span data-stu-id="fa97e-267">And a response file named `dest.txt` that specifies a destination folder in the file system:</span></span>

    /Dest:C:\myfolder

<span data-ttu-id="fa97e-268">Och en svarsfil med namnet `options.txt` som anger alternativ för AzCopy:</span><span class="sxs-lookup"><span data-stu-id="fa97e-268">And a response file named `options.txt` that specifies options for AzCopy:</span></span>

    /S /Y

<span data-ttu-id="fa97e-269">För att anropa AzCopy med dessa svarsfiler, som finns i en katalog `C:\responsefiles`, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="fa97e-269">To call AzCopy with these response files, all of which reside in a directory `C:\responsefiles`, use this command:</span></span>

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

<span data-ttu-id="fa97e-270">AzCopy bearbetar det här kommandot precis som om du har inkluderat alla enskilda parametrar på kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="fa97e-270">AzCopy processes this command just as it would if you included all of the individual parameters on the command line:</span></span>

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a><span data-ttu-id="fa97e-271">Ange en signatur för delad åtkomst (SAS)</span><span class="sxs-lookup"><span data-stu-id="fa97e-271">Specify a shared access signature (SAS)</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

<span data-ttu-id="fa97e-272">Du kan också ange en SAS för URI-behållaren:</span><span class="sxs-lookup"><span data-stu-id="fa97e-272">You can also specify a SAS on the container URI:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a><span data-ttu-id="fa97e-273">Ändringsjournalen mapp</span><span class="sxs-lookup"><span data-stu-id="fa97e-273">Journal file folder</span></span>
<span data-ttu-id="fa97e-274">Varje gång du utfärda ett kommando till AzCopy, kontrollerar den om en journal-fil finns i standardmappen eller om den finns i en mapp som du har angett via det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="fa97e-274">Each time you issue a command to AzCopy, it checks whether a journal file exists in the default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="fa97e-275">Om journalfilen inte finns på någon plats, behandlar åtgärden som nya AzCopy och genererar en ny journalfil.</span><span class="sxs-lookup"><span data-stu-id="fa97e-275">If the journal file does not exist in either place, AzCopy treats the operation as new and generates a new journal file.</span></span>

<span data-ttu-id="fa97e-276">Om journalfilen finns kontrollerar AzCopy om den kommandorad som du matar in matchar kommandoraden i journal-fil.</span><span class="sxs-lookup"><span data-stu-id="fa97e-276">If the journal file does exist, AzCopy checks whether the command line that you input matches the command line in the journal file.</span></span> <span data-ttu-id="fa97e-277">Om två kommandorader matchar återupptar AzCopy ofullständiga igen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-277">If the two command lines match, AzCopy resumes the incomplete operation.</span></span> <span data-ttu-id="fa97e-278">Om de inte matchar, uppmanas du att antingen skriva över journalfilen att starta en ny åtgärd eller Avbryt den aktuella åtgärden.</span><span class="sxs-lookup"><span data-stu-id="fa97e-278">If they do not match, you are prompted to either overwrite the journal file to start a new operation, or to cancel the current operation.</span></span>

<span data-ttu-id="fa97e-279">Om du vill använda standardplatsen för journalfilen:</span><span class="sxs-lookup"><span data-stu-id="fa97e-279">If you want to use the default location for the journal file:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

<span data-ttu-id="fa97e-280">Om du utelämnar alternativet `/Z`, eller ange alternativet `/Z` utan sökvägen till mappen som du ser ovan, AzCopy journalfilen skapas i standardplatsen, som är `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="fa97e-280">If you omit option `/Z`, or specify option `/Z` without the folder path, as shown above, AzCopy creates the journal file in the default location, which is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="fa97e-281">Om ändringsjournalen filen redan finns återupptas AzCopy igen baserat på journal-fil.</span><span class="sxs-lookup"><span data-stu-id="fa97e-281">If the journal file already exists, then AzCopy resumes the operation based on the journal file.</span></span>

<span data-ttu-id="fa97e-282">Om du vill ange en anpassad plats för journalfilen:</span><span class="sxs-lookup"><span data-stu-id="fa97e-282">If you want to specify a custom location for the journal file:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

<span data-ttu-id="fa97e-283">Det här exemplet skapar journal-fil om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="fa97e-283">This example creates the journal file if it does not already exist.</span></span> <span data-ttu-id="fa97e-284">Om den finns, återupptar AzCopy igen baserat på journal-fil.</span><span class="sxs-lookup"><span data-stu-id="fa97e-284">If it does exist, then AzCopy resumes the operation based on the journal file.</span></span>

<span data-ttu-id="fa97e-285">Om du vill fortsätta AzCopy:</span><span class="sxs-lookup"><span data-stu-id="fa97e-285">If you want to resume an AzCopy operation:</span></span>

```azcopy
AzCopy /Z:C:\journalfolder\
```

<span data-ttu-id="fa97e-286">Det här exemplet fortsätter med den senaste åtgärden som inte kunde slutföras.</span><span class="sxs-lookup"><span data-stu-id="fa97e-286">This example resumes the last operation, which may have failed to complete.</span></span>

### <a name="generate-a-log-file"></a><span data-ttu-id="fa97e-287">Generera en loggfil</span><span class="sxs-lookup"><span data-stu-id="fa97e-287">Generate a log file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

<span data-ttu-id="fa97e-288">Om du anger alternativet `/V` utan att ange en sökväg till den detaljerade loggen AzCopy skapar sedan loggfilen på standardplatsen, vilket är `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="fa97e-288">If you specify option `/V` without providing a file path to the verbose log, then AzCopy creates the log file in the default location, which is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span></span>

<span data-ttu-id="fa97e-289">Annars kan du skapa en loggfil på en annan plats:</span><span class="sxs-lookup"><span data-stu-id="fa97e-289">Otherwise, you can create an log file in a custom location:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

<span data-ttu-id="fa97e-290">Observera att om du anger en relativ sökväg efter alternativet `/V`, som `/V:test/azcopy1.log`, och sedan den detaljerade loggen skapas i den aktuella arbetskatalogen i en undermapp som heter `test`.</span><span class="sxs-lookup"><span data-stu-id="fa97e-290">Note that if you specify a relative path following option `/V`, such as `/V:test/azcopy1.log`, then the verbose log is created in the current working directory within a subfolder named `test`.</span></span>

### <a name="specify-the-number-of-concurrent-operations-to-start"></a><span data-ttu-id="fa97e-291">Ange antalet samtidiga åtgärder att starta</span><span class="sxs-lookup"><span data-stu-id="fa97e-291">Specify the number of concurrent operations to start</span></span>
<span data-ttu-id="fa97e-292">Alternativet `/NC` anger antalet samtidiga kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="fa97e-292">Option `/NC` specifies the number of concurrent copy operations.</span></span> <span data-ttu-id="fa97e-293">Som standard startar AzCopy antalet samtidiga åtgärder att öka genomflödet data transfer.</span><span class="sxs-lookup"><span data-stu-id="fa97e-293">By default, AzCopy starts a certain number of concurrent operations to increase the data transfer throughput.</span></span> <span data-ttu-id="fa97e-294">Antalet samtidiga åtgärder är lika med antalet processorer som du har för tabellåtgärder.</span><span class="sxs-lookup"><span data-stu-id="fa97e-294">For Table operations, the number of concurrent operations is equal to the number of processors you have.</span></span> <span data-ttu-id="fa97e-295">För Blob- och fil-åtgärder, antalet samtidiga åtgärder är lika med 8 gånger antalet processorer som du har.</span><span class="sxs-lookup"><span data-stu-id="fa97e-295">For Blob and File operations, the number of concurrent operations is equal 8 times the number of processors you have.</span></span> <span data-ttu-id="fa97e-296">Om du använder AzCopy över ett nätverk med låg bandbredd, kan du ange ett lägre värde /NC att undvika misslyckades på grund av konkurrens om resurser.</span><span class="sxs-lookup"><span data-stu-id="fa97e-296">If you are running AzCopy across a low-bandwidth network, you can specify a lower number for /NC to avoid failure caused by resource competition.</span></span>

### <a name="run-azcopy-against-azure-storage-emulator"></a><span data-ttu-id="fa97e-297">Köra AzCopy mot Azure Storage-emulatorn</span><span class="sxs-lookup"><span data-stu-id="fa97e-297">Run AzCopy against Azure Storage Emulator</span></span>
<span data-ttu-id="fa97e-298">Du kan köra AzCopy mot den [Azure Storage-emulatorn](storage-use-emulator.md) för BLOB:</span><span class="sxs-lookup"><span data-stu-id="fa97e-298">You can run AzCopy against the [Azure Storage Emulator](storage-use-emulator.md) for Blobs:</span></span>

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

<span data-ttu-id="fa97e-299">och tabeller:</span><span class="sxs-lookup"><span data-stu-id="fa97e-299">and Tables:</span></span>

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

## <a name="azcopy-parameters"></a><span data-ttu-id="fa97e-300">AzCopy-parametrar</span><span class="sxs-lookup"><span data-stu-id="fa97e-300">AzCopy Parameters</span></span>
<span data-ttu-id="fa97e-301">Parametrar för AzCopy beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="fa97e-301">Parameters for AzCopy are described below.</span></span> <span data-ttu-id="fa97e-302">Du kan också ange något av följande kommandon från kommandoraden för att få hjälp med AzCopy:</span><span class="sxs-lookup"><span data-stu-id="fa97e-302">You can also type one of the following commands from the command line for help in using AzCopy:</span></span>

* <span data-ttu-id="fa97e-303">För detaljerad hjälp för AzCopy:`AzCopy /?`</span><span class="sxs-lookup"><span data-stu-id="fa97e-303">For detailed command-line help for AzCopy: `AzCopy /?`</span></span>
* <span data-ttu-id="fa97e-304">För detaljerad hjälp om någon AzCopy-parameter:`AzCopy /?:SourceKey`</span><span class="sxs-lookup"><span data-stu-id="fa97e-304">For detailed help with any AzCopy parameter: `AzCopy /?:SourceKey`</span></span>
* <span data-ttu-id="fa97e-305">Kommandoradsverktyget exempel:`AzCopy /?:Samples`</span><span class="sxs-lookup"><span data-stu-id="fa97e-305">For command-line examples: `AzCopy /?:Samples`</span></span>

### <a name="sourcesource"></a><span data-ttu-id="fa97e-306">/ Source: ”källa”</span><span class="sxs-lookup"><span data-stu-id="fa97e-306">/Source:"source"</span></span>
<span data-ttu-id="fa97e-307">Anger källdata från som ska kopieras.</span><span class="sxs-lookup"><span data-stu-id="fa97e-307">Specifies the source data from which to copy.</span></span> <span data-ttu-id="fa97e-308">Källan kan vara en katalog i filsystemet, en blob-behållare, en virtuell katalog för blob, en lagringsfilresurs, en katalog i lagring eller en Azure-tabellen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-308">The source can be a file system directory, a blob container, a blob virtual directory, a storage file share, a storage file directory, or an Azure table.</span></span>

<span data-ttu-id="fa97e-309">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-309">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="destdestination"></a><span data-ttu-id="fa97e-310">/ Dest: ”mål”</span><span class="sxs-lookup"><span data-stu-id="fa97e-310">/Dest:"destination"</span></span>
<span data-ttu-id="fa97e-311">Anger att kopiera till målet.</span><span class="sxs-lookup"><span data-stu-id="fa97e-311">Specifies the destination to copy to.</span></span> <span data-ttu-id="fa97e-312">Målet kan vara en katalog i filsystemet, en blob-behållare, en virtuell katalog för blob, en lagringsfilresurs, en katalog i lagring eller en Azure-tabellen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-312">The destination can be a file system directory, a blob container, a blob virtual directory, a storage file share, a storage file directory, or an Azure table.</span></span>

<span data-ttu-id="fa97e-313">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-313">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="patternfile-pattern"></a><span data-ttu-id="fa97e-314">/ Mönstret: ”filmönstret”</span><span class="sxs-lookup"><span data-stu-id="fa97e-314">/Pattern:"file-pattern"</span></span>
<span data-ttu-id="fa97e-315">Anger ett fil-mönster som anger vilka filer som ska kopieras.</span><span class="sxs-lookup"><span data-stu-id="fa97e-315">Specifies a file pattern that indicates which files to copy.</span></span> <span data-ttu-id="fa97e-316">Beteendet för parametern /Pattern bestäms av platsen för datakällan och förekomsten av alternativet rekursiv läge.</span><span class="sxs-lookup"><span data-stu-id="fa97e-316">The behavior of the /Pattern parameter is determined by the location of the source data, and the presence of the recursive mode option.</span></span> <span data-ttu-id="fa97e-317">Rekursiva läge har angetts via alternativet/s.</span><span class="sxs-lookup"><span data-stu-id="fa97e-317">Recursive mode is specified via option /S.</span></span>

<span data-ttu-id="fa97e-318">Om den angivna källan är en katalog i filsystemet, sedan standard jokertecken är aktiverat och det angivna mönstret för filen matchas mot filer i katalogen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-318">If the specified source is a directory in the file system, then standard wildcards are in effect, and the file pattern provided is matched against files within the directory.</span></span> <span data-ttu-id="fa97e-319">Om alternativet /S anges sedan AzCopy också matchar det angivna mönstret mot alla filer i alla undermappar under katalogen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-319">If option /S is specified, then AzCopy also matches the specified pattern against all files in any subfolders beneath the directory.</span></span>

<span data-ttu-id="fa97e-320">Om den angivna källan är en blob-behållare eller virtuell katalog, tillämpas inte jokertecken.</span><span class="sxs-lookup"><span data-stu-id="fa97e-320">If the specified source is a blob container or virtual directory, then wildcards are not applied.</span></span> <span data-ttu-id="fa97e-321">Om alternativet /S anges sedan AzCopy tolkas det angivna mönstret som ett blob-prefix.</span><span class="sxs-lookup"><span data-stu-id="fa97e-321">If option /S is specified, then AzCopy interprets the specified file pattern as a blob prefix.</span></span> <span data-ttu-id="fa97e-322">Om alternativet inte anges /S sedan AzCopy matchar mönstret filen mot exakt blob-namn.</span><span class="sxs-lookup"><span data-stu-id="fa97e-322">If option /S is not specified, then AzCopy matches the file pattern against exact blob names.</span></span>

<span data-ttu-id="fa97e-323">Om den angivna källan är en Azure-filresurs, så du måste antingen ange exakt filnamnet, (t.ex. abc.txt) om du vill kopiera en fil eller ange alternativet /S att kopiera alla filer i resursen rekursivt.</span><span class="sxs-lookup"><span data-stu-id="fa97e-323">If the specified source is an Azure file share, then you must either specify the exact file name, (e.g. abc.txt) to copy a single file, or specify option /S to copy all files in the share recursively.</span></span> <span data-ttu-id="fa97e-324">Försök att ange både en fil mönstret och alternativet /S tillsammans resulterar i ett fel.</span><span class="sxs-lookup"><span data-stu-id="fa97e-324">Attempting to specify both a file pattern and option /S together results in an error.</span></span>

<span data-ttu-id="fa97e-325">AzCopy använder skiftlägeskänsliga matchar när den/Source är en blob-behållare eller blob virtuell katalog och använder icke skiftlägeskänslig matchning i andra fall.</span><span class="sxs-lookup"><span data-stu-id="fa97e-325">AzCopy uses case-sensitive matching when the /Source is a blob container or blob virtual directory, and uses case-insensitive matching in all the other cases.</span></span>

<span data-ttu-id="fa97e-326">Det standardmönster för filen som används när inga filmönstret har angetts är *.*</span><span class="sxs-lookup"><span data-stu-id="fa97e-326">The default file pattern used when no file pattern is specified is *.*</span></span> <span data-ttu-id="fa97e-327">för en plats för system eller ett tomt prefix för en Azure-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="fa97e-327">for a file system location or an empty prefix for an Azure Storage location.</span></span> <span data-ttu-id="fa97e-328">Ange flera filen mönster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="fa97e-328">Specifying multiple file patterns is not supported.</span></span>

<span data-ttu-id="fa97e-329">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-329">**Applicable to:** Blobs, Files</span></span>

### <a name="destkeystorage-key"></a><span data-ttu-id="fa97e-330">/ DestKey: ”lagringsnyckel”</span><span class="sxs-lookup"><span data-stu-id="fa97e-330">/DestKey:"storage-key"</span></span>
<span data-ttu-id="fa97e-331">Anger lagringskontonyckel för målresurs.</span><span class="sxs-lookup"><span data-stu-id="fa97e-331">Specifies the storage account key for the destination resource.</span></span>

<span data-ttu-id="fa97e-332">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-332">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="destsassas-token"></a><span data-ttu-id="fa97e-333">/ DestSAS: ”sas-token”</span><span class="sxs-lookup"><span data-stu-id="fa97e-333">/DestSAS:"sas-token"</span></span>
<span data-ttu-id="fa97e-334">Anger en delad signatur åtkomst (SAS) med behörigheter för Läs- och skrivbehörighet för mål (om tillämpligt).</span><span class="sxs-lookup"><span data-stu-id="fa97e-334">Specifies a Shared Access Signature (SAS) with READ and WRITE permissions for the destination (if applicable).</span></span> <span data-ttu-id="fa97e-335">Omge SAS med dubbla citattecken, eftersom det kanske innehåller kommandoradsverktyget specialtecken.</span><span class="sxs-lookup"><span data-stu-id="fa97e-335">Surround the SAS with double quotes, as it may contains special command-line characters.</span></span>

<span data-ttu-id="fa97e-336">Om mål-resursen är en blob-behållare, en filresurs eller en tabell, du kan antingen ange det här alternativet följt av SAS-token eller du kan ange SAS som en del av mål-blob-behållare, filresurser eller tabellens URI, utan det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="fa97e-336">If the destination resource is a blob container, file share or table, you can either specify this option followed by the SAS token, or you can specify the SAS as part of the destination blob container, file share or table's URI, without this option.</span></span>

<span data-ttu-id="fa97e-337">Om käll- och båda blobbar, måste mål-blob finnas inom samma lagringskonto som käll-blob.</span><span class="sxs-lookup"><span data-stu-id="fa97e-337">If the source and destination are both blobs, then the destination blob must reside within the same storage account as the source blob.</span></span>

<span data-ttu-id="fa97e-338">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-338">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcekeystorage-key"></a><span data-ttu-id="fa97e-339">/ SourceKey: ”lagringsnyckel”</span><span class="sxs-lookup"><span data-stu-id="fa97e-339">/SourceKey:"storage-key"</span></span>
<span data-ttu-id="fa97e-340">Anger lagringskontonyckel för käll-resurs.</span><span class="sxs-lookup"><span data-stu-id="fa97e-340">Specifies the storage account key for the source resource.</span></span>

<span data-ttu-id="fa97e-341">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-341">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcesassas-token"></a><span data-ttu-id="fa97e-342">/ SourceSAS: ”sas-token”</span><span class="sxs-lookup"><span data-stu-id="fa97e-342">/SourceSAS:"sas-token"</span></span>
<span data-ttu-id="fa97e-343">Anger en signatur för delad åtkomst med läs- och behörighet för källan (om tillämpligt).</span><span class="sxs-lookup"><span data-stu-id="fa97e-343">Specifies a Shared Access Signature with READ and LIST permissions for the source (if applicable).</span></span> <span data-ttu-id="fa97e-344">Omge SAS med dubbla citattecken, eftersom det kanske innehåller kommandoradsverktyget specialtecken.</span><span class="sxs-lookup"><span data-stu-id="fa97e-344">Surround the SAS with double quotes, as it may contains special command-line characters.</span></span>

<span data-ttu-id="fa97e-345">Om källa resursen är en blob-behållare och varken en nyckel eller en SAS anges, läses blob-behållaren via anonym åtkomst.</span><span class="sxs-lookup"><span data-stu-id="fa97e-345">If the source resource is a blob container, and neither a key nor a SAS is provided, then the blob container is read via anonymous access.</span></span>

<span data-ttu-id="fa97e-346">Om källan är en filresurs eller tabell, en nyckel eller en SAS måste anges.</span><span class="sxs-lookup"><span data-stu-id="fa97e-346">If the source is a file share or table, a key or a SAS must be provided.</span></span>

<span data-ttu-id="fa97e-347">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-347">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="s"></a><span data-ttu-id="fa97e-348">/ S</span><span class="sxs-lookup"><span data-stu-id="fa97e-348">/S</span></span>
<span data-ttu-id="fa97e-349">Anger rekursiv kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="fa97e-349">Specifies recursive mode for copy operations.</span></span> <span data-ttu-id="fa97e-350">AzCopy kopierar alla blobbar eller filer som matchar det angivna mönstret, inklusive de på undermappar i rekursiva läge.</span><span class="sxs-lookup"><span data-stu-id="fa97e-350">In recursive mode, AzCopy copies all blobs or files that match the specified file pattern, including those in subfolders.</span></span>

<span data-ttu-id="fa97e-351">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-351">**Applicable to:** Blobs, Files</span></span>

### <a name="blobtypeblock--page--append"></a><span data-ttu-id="fa97e-352">/ BlobType: ”block” | ”sidan” | ”Lägg till”</span><span class="sxs-lookup"><span data-stu-id="fa97e-352">/BlobType:"block" | "page" | "append"</span></span>
<span data-ttu-id="fa97e-353">Anger om mål-blob är en blockblobb, en sidblob eller en tilläggsblobb.</span><span class="sxs-lookup"><span data-stu-id="fa97e-353">Specifies whether the destination blob is a block blob, a page blob, or an append blob.</span></span> <span data-ttu-id="fa97e-354">Det här alternativet gäller bara när du laddar upp en blob.</span><span class="sxs-lookup"><span data-stu-id="fa97e-354">This option is applicable only when you are uploading a blob.</span></span> <span data-ttu-id="fa97e-355">Annars genereras ett fel.</span><span class="sxs-lookup"><span data-stu-id="fa97e-355">Otherwise, an error is generated.</span></span> <span data-ttu-id="fa97e-356">Om målet är en blob och det här alternativet inte anges som standard skapas en blockblobb AzCopy.</span><span class="sxs-lookup"><span data-stu-id="fa97e-356">If the destination is a blob and this option is not specified, by default, AzCopy creates a block blob.</span></span>

<span data-ttu-id="fa97e-357">**Gäller för:** Blobbar</span><span class="sxs-lookup"><span data-stu-id="fa97e-357">**Applicable to:** Blobs</span></span>

### <a name="checkmd5"></a><span data-ttu-id="fa97e-358">/ CheckMD5</span><span class="sxs-lookup"><span data-stu-id="fa97e-358">/CheckMD5</span></span>
<span data-ttu-id="fa97e-359">Beräknar en MD5-hash för hämtade data och verifierar att MD5-hash lagras i blob eller filens Content-MD5-egenskap stämmer med den beräknade hashen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-359">Calculates an MD5 hash for downloaded data and verifies that the MD5 hash stored in the blob or file's Content-MD5 property matches the calculated hash.</span></span> <span data-ttu-id="fa97e-360">MD5-kontrollen är inaktiverad som standard, så du måste ange det här alternativet för att kontrollera MD5 när du hämtar data.</span><span class="sxs-lookup"><span data-stu-id="fa97e-360">The MD5 check is turned off by default, so you must specify this option to perform the MD5 check when downloading data.</span></span>

<span data-ttu-id="fa97e-361">Observera att Azure Storage inte garantera att den MD5-hash som lagrats till filen eller blob är uppdaterad.</span><span class="sxs-lookup"><span data-stu-id="fa97e-361">Note that Azure Storage doesn't guarantee that the MD5 hash stored for the blob or file is up-to-date.</span></span> <span data-ttu-id="fa97e-362">Det är klientens ansvar att uppdatera MD5 när blob eller fil ändras.</span><span class="sxs-lookup"><span data-stu-id="fa97e-362">It is client's responsibility to update the MD5 whenever the blob or file is modified.</span></span>

<span data-ttu-id="fa97e-363">AzCopy egenskapen alltid Content-MD5 för ett Azure blob eller en fil efter överföring till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="fa97e-363">AzCopy always sets the Content-MD5 property for an Azure blob or file after uploading it to the service.</span></span>  

<span data-ttu-id="fa97e-364">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-364">**Applicable to:** Blobs, Files</span></span>

### <a name="snapshot"></a><span data-ttu-id="fa97e-365">/ Ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="fa97e-365">/Snapshot</span></span>
<span data-ttu-id="fa97e-366">Anger om du vill överföra ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="fa97e-366">Indicates whether to transfer snapshots.</span></span> <span data-ttu-id="fa97e-367">Det här alternativet gäller endast om källan är en blob.</span><span class="sxs-lookup"><span data-stu-id="fa97e-367">This option is only valid when the source is a blob.</span></span>

<span data-ttu-id="fa97e-368">Överförda blob-ögonblicksbilder ändras i det här formatet: .extension blobbnamnet (snapshot-time)</span><span class="sxs-lookup"><span data-stu-id="fa97e-368">The transferred blob snapshots are renamed in this format: blob-name (snapshot-time).extension</span></span>

<span data-ttu-id="fa97e-369">Som standard kopieras inte ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="fa97e-369">By default, snapshots are not copied.</span></span>

<span data-ttu-id="fa97e-370">**Gäller för:** Blobbar</span><span class="sxs-lookup"><span data-stu-id="fa97e-370">**Applicable to:** Blobs</span></span>

### <a name="vverbose-log-file"></a><span data-ttu-id="fa97e-371">/ V: [utförlig log-fil]</span><span class="sxs-lookup"><span data-stu-id="fa97e-371">/V:[verbose-log-file]</span></span>
<span data-ttu-id="fa97e-372">Utdata utförlig statusmeddelanden till en loggfil.</span><span class="sxs-lookup"><span data-stu-id="fa97e-372">Outputs verbose status messages into a log file.</span></span>

<span data-ttu-id="fa97e-373">Som standard heter den utförliga loggfilen AzCopyVerbose.log i `%LocalAppData%\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="fa97e-373">By default, the verbose log file is named AzCopyVerbose.log in `%LocalAppData%\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="fa97e-374">Om du anger en befintlig plats för det här alternativet läggs den detaljerade loggen till filen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-374">If you specify an existing file location for this option, the verbose log is appended to that file.</span></span>  

<span data-ttu-id="fa97e-375">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-375">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="zjournal-file-folder"></a><span data-ttu-id="fa97e-376">/ Z: [journal-filmapp]</span><span class="sxs-lookup"><span data-stu-id="fa97e-376">/Z:[journal-file-folder]</span></span>
<span data-ttu-id="fa97e-377">Anger en mapp för att återuppta en åtgärd journalen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-377">Specifies a journal file folder for resuming an operation.</span></span>

<span data-ttu-id="fa97e-378">AzCopy stöder alltid fortsätter om en åtgärd har avbrutits.</span><span class="sxs-lookup"><span data-stu-id="fa97e-378">AzCopy always supports resuming if an operation has been interrupted.</span></span>

<span data-ttu-id="fa97e-379">Om det här alternativet inte har angetts eller om den har angetts utan en mappsökväg, skapar AzCopy journal-fil på standardplatsen är % LocalAppData%\Microsoft\Azure\AzCopy.</span><span class="sxs-lookup"><span data-stu-id="fa97e-379">If this option is not specified, or it is specified without a folder path, then AzCopy creates the journal file in the default location, which is %LocalAppData%\Microsoft\Azure\AzCopy.</span></span>

<span data-ttu-id="fa97e-380">Varje gång du utfärda ett kommando till AzCopy, kontrollerar den om en journal-fil finns i standardmappen eller om den finns i en mapp som du har angett via det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="fa97e-380">Each time you issue a command to AzCopy, it checks whether a journal file exists in the default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="fa97e-381">Om journalfilen inte finns på någon plats, behandlar åtgärden som nya AzCopy och genererar en ny journalfil.</span><span class="sxs-lookup"><span data-stu-id="fa97e-381">If the journal file does not exist in either place, AzCopy treats the operation as new and generates a new journal file.</span></span>

<span data-ttu-id="fa97e-382">Om journalfilen finns kontrollerar AzCopy om den kommandorad som du matar in matchar kommandoraden i journal-fil.</span><span class="sxs-lookup"><span data-stu-id="fa97e-382">If the journal file does exist, AzCopy checks whether the command line that you input matches the command line in the journal file.</span></span> <span data-ttu-id="fa97e-383">Om två kommandorader matchar återupptar AzCopy ofullständiga igen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-383">If the two command lines match, AzCopy resumes the incomplete operation.</span></span> <span data-ttu-id="fa97e-384">Om de inte matchar, uppmanas du att antingen skriva över journalfilen att starta en ny åtgärd eller Avbryt den aktuella åtgärden.</span><span class="sxs-lookup"><span data-stu-id="fa97e-384">If they do not match, you are prompted to either overwrite the journal file to start a new operation, or to cancel the current operation.</span></span>

<span data-ttu-id="fa97e-385">Journalfilen tas bort vid slutförande av åtgärden.</span><span class="sxs-lookup"><span data-stu-id="fa97e-385">The journal file is deleted upon successful completion of the operation.</span></span>

<span data-ttu-id="fa97e-386">Observera att återuppta en åtgärd från en journal-fil som skapats av en tidigare version av AzCopy inte stöds.</span><span class="sxs-lookup"><span data-stu-id="fa97e-386">Note that resuming an operation from a journal file created by a previous version of AzCopy is not supported.</span></span>

<span data-ttu-id="fa97e-387">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-387">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="parameter-file"></a><span data-ttu-id="fa97e-388">/@:"parameter-File”</span><span class="sxs-lookup"><span data-stu-id="fa97e-388">/@:"parameter-file"</span></span>
<span data-ttu-id="fa97e-389">Anger en fil som innehåller parametrar.</span><span class="sxs-lookup"><span data-stu-id="fa97e-389">Specifies a file that contains parameters.</span></span> <span data-ttu-id="fa97e-390">AzCopy bearbetar parametrarna i filen, precis som om de hade angetts på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="fa97e-390">AzCopy processes the parameters in the file just as if they had been specified on the command line.</span></span>

<span data-ttu-id="fa97e-391">I en svarsfil, kan du antingen ange flera parametrar på en enda rad eller ange varje parameter på en egen rad.</span><span class="sxs-lookup"><span data-stu-id="fa97e-391">In a response file, you can either specify multiple parameters on a single line, or specify each parameter on its own line.</span></span> <span data-ttu-id="fa97e-392">Observera att en enskild parameter inte kan innehålla flera rader.</span><span class="sxs-lookup"><span data-stu-id="fa97e-392">Note that an individual parameter cannot span multiple lines.</span></span>

<span data-ttu-id="fa97e-393">Svarsfiler kan innehålla kommentarer rader som börjar med symbolen #.</span><span class="sxs-lookup"><span data-stu-id="fa97e-393">Response files can include comments lines that begin with the # symbol.</span></span>

<span data-ttu-id="fa97e-394">Du kan ange flera svarsfiler.</span><span class="sxs-lookup"><span data-stu-id="fa97e-394">You can specify multiple response files.</span></span> <span data-ttu-id="fa97e-395">Observera dock att AzCopy inte stöder kapslade svarsfiler.</span><span class="sxs-lookup"><span data-stu-id="fa97e-395">However, note that AzCopy does not support nested response files.</span></span>

<span data-ttu-id="fa97e-396">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-396">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="y"></a><span data-ttu-id="fa97e-397">/Y</span><span class="sxs-lookup"><span data-stu-id="fa97e-397">/Y</span></span>
<span data-ttu-id="fa97e-398">Förhindrar att alla meddelanden för bekräftelse av AzCopy.</span><span class="sxs-lookup"><span data-stu-id="fa97e-398">Suppresses all AzCopy confirmation prompts.</span></span>

<span data-ttu-id="fa97e-399">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-399">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="l"></a><span data-ttu-id="fa97e-400">/ L</span><span class="sxs-lookup"><span data-stu-id="fa97e-400">/L</span></span>
<span data-ttu-id="fa97e-401">Anger en åtgärd. Inga data kopieras.</span><span class="sxs-lookup"><span data-stu-id="fa97e-401">Specifies a listing operation only; no data is copied.</span></span>

<span data-ttu-id="fa97e-402">AzCopy tolkar den med hjälp av det här alternativet som en simulering för att köra kommandoraden utan det här alternativet /L och antal hur många objekt har kopierats kan du ange alternativet /V samtidigt för att kontrollera vilka objekt kopieras i den detaljerade loggen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-402">AzCopy interprets the using of this option as a simulation for running the command line without this option /L and counts how many objects are copied, you can specify option /V at the same time to check which objects are copied in the verbose log.</span></span>

<span data-ttu-id="fa97e-403">Beteendet för det här alternativet bestäms också av platsen för datakällan och förekomsten av rekursiva läge alternativet /S och filen alternativet mönster /Pattern.</span><span class="sxs-lookup"><span data-stu-id="fa97e-403">The behavior of this option is also determined by the location of the source data and the presence of the recursive mode option /S and file pattern option /Pattern.</span></span>

<span data-ttu-id="fa97e-404">AzCopy kräver behörighet att lista och läsa den här källplatsen när du använder det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="fa97e-404">AzCopy requires LIST and READ permission of this source location when using this option.</span></span>

<span data-ttu-id="fa97e-405">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-405">**Applicable to:** Blobs, Files</span></span>

### <a name="mt"></a><span data-ttu-id="fa97e-406">/MT</span><span class="sxs-lookup"><span data-stu-id="fa97e-406">/MT</span></span>
<span data-ttu-id="fa97e-407">Anger den hämtade filen modifierades senast vara densamma som källan blob eller filen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-407">Sets the downloaded file's last-modified time to be the same as the source blob or file's.</span></span>

<span data-ttu-id="fa97e-408">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-408">**Applicable to:** Blobs, Files</span></span>

### <a name="xn"></a><span data-ttu-id="fa97e-409">/XN</span><span class="sxs-lookup"><span data-stu-id="fa97e-409">/XN</span></span>
<span data-ttu-id="fa97e-410">Undantar en nyare käll-resurs.</span><span class="sxs-lookup"><span data-stu-id="fa97e-410">Excludes a newer source resource.</span></span> <span data-ttu-id="fa97e-411">Resursen kopieras inte om den senaste ändringstiden på källan är samma eller senare än målet.</span><span class="sxs-lookup"><span data-stu-id="fa97e-411">The resource is not copied if the last modified time of the source is the same or newer than destination.</span></span>

<span data-ttu-id="fa97e-412">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-412">**Applicable to:** Blobs, Files</span></span>

### <a name="xo"></a><span data-ttu-id="fa97e-413">/XO</span><span class="sxs-lookup"><span data-stu-id="fa97e-413">/XO</span></span>
<span data-ttu-id="fa97e-414">Undantar en äldre käll-resurs.</span><span class="sxs-lookup"><span data-stu-id="fa97e-414">Excludes an older source resource.</span></span> <span data-ttu-id="fa97e-415">Resursen kopieras inte om den senaste ändringstiden på källan är samma eller äldre än målet.</span><span class="sxs-lookup"><span data-stu-id="fa97e-415">The resource is not copied if the last modified time of the source is the same or older than destination.</span></span>

<span data-ttu-id="fa97e-416">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-416">**Applicable to:** Blobs, Files</span></span>

### <a name="a"></a><span data-ttu-id="fa97e-417">/A</span><span class="sxs-lookup"><span data-stu-id="fa97e-417">/A</span></span>
<span data-ttu-id="fa97e-418">Överför bara filer som har attributet Arkiv.</span><span class="sxs-lookup"><span data-stu-id="fa97e-418">Uploads only files that have the Archive attribute set.</span></span>

<span data-ttu-id="fa97e-419">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-419">**Applicable to:** Blobs, Files</span></span>

### <a name="iarashcnetoi"></a><span data-ttu-id="fa97e-420">/ IA: [RASHCNETOI]</span><span class="sxs-lookup"><span data-stu-id="fa97e-420">/IA:[RASHCNETOI]</span></span>
<span data-ttu-id="fa97e-421">Överför bara filer med någon av de angivna attributen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-421">Uploads only files that have any of the specified attributes set.</span></span>

<span data-ttu-id="fa97e-422">Tillgängliga attribut inkluderar:</span><span class="sxs-lookup"><span data-stu-id="fa97e-422">Available attributes include:</span></span>

* <span data-ttu-id="fa97e-423">R = skrivskyddade filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-423">R = Read-only files</span></span>
* <span data-ttu-id="fa97e-424">A = filer som är klara för arkivering</span><span class="sxs-lookup"><span data-stu-id="fa97e-424">A = Files ready for archiving</span></span>
* <span data-ttu-id="fa97e-425">S = systemfiler</span><span class="sxs-lookup"><span data-stu-id="fa97e-425">S = System files</span></span>
* <span data-ttu-id="fa97e-426">H = dolda filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-426">H = Hidden files</span></span>
* <span data-ttu-id="fa97e-427">C = komprimerade filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-427">C = Compressed files</span></span>
* <span data-ttu-id="fa97e-428">N = normala filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-428">N = Normal files</span></span>
* <span data-ttu-id="fa97e-429">E = krypterade filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-429">E = Encrypted files</span></span>
* <span data-ttu-id="fa97e-430">T = temporära filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-430">T = Temporary files</span></span>
* <span data-ttu-id="fa97e-431">O = Offline-filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-431">O = Offline files</span></span>
* <span data-ttu-id="fa97e-432">Jag = icke-indexerat filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-432">I = Non-indexed files</span></span>

<span data-ttu-id="fa97e-433">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-433">**Applicable to:** Blobs, Files</span></span>

### <a name="xarashcnetoi"></a><span data-ttu-id="fa97e-434">/ XA: [RASHCNETOI]</span><span class="sxs-lookup"><span data-stu-id="fa97e-434">/XA:[RASHCNETOI]</span></span>
<span data-ttu-id="fa97e-435">Undantar filer med någon av de angivna attributen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-435">Excludes files that have any of the specified attributes set.</span></span>

<span data-ttu-id="fa97e-436">Tillgängliga attribut inkluderar:</span><span class="sxs-lookup"><span data-stu-id="fa97e-436">Available attributes include:</span></span>

* <span data-ttu-id="fa97e-437">R = skrivskyddade filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-437">R = Read-only files</span></span>
* <span data-ttu-id="fa97e-438">A = filer som är klara för arkivering</span><span class="sxs-lookup"><span data-stu-id="fa97e-438">A = Files ready for archiving</span></span>
* <span data-ttu-id="fa97e-439">S = systemfiler</span><span class="sxs-lookup"><span data-stu-id="fa97e-439">S = System files</span></span>
* <span data-ttu-id="fa97e-440">H = dolda filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-440">H = Hidden files</span></span>
* <span data-ttu-id="fa97e-441">C = komprimerade filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-441">C = Compressed files</span></span>
* <span data-ttu-id="fa97e-442">N = normala filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-442">N = Normal files</span></span>
* <span data-ttu-id="fa97e-443">E = krypterade filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-443">E = Encrypted files</span></span>
* <span data-ttu-id="fa97e-444">T = temporära filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-444">T = Temporary files</span></span>
* <span data-ttu-id="fa97e-445">O = Offline-filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-445">O = Offline files</span></span>
* <span data-ttu-id="fa97e-446">Jag = icke-indexerat filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-446">I = Non-indexed files</span></span>

<span data-ttu-id="fa97e-447">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-447">**Applicable to:** Blobs, Files</span></span>

### <a name="delimiterdelimiter"></a><span data-ttu-id="fa97e-448">/ Avgränsare: ”avgränsare”</span><span class="sxs-lookup"><span data-stu-id="fa97e-448">/Delimiter:"delimiter"</span></span>
<span data-ttu-id="fa97e-449">Anger ett avgränsningstecken som används för att avgränsa virtuella kataloger i ett blob-namn.</span><span class="sxs-lookup"><span data-stu-id="fa97e-449">Indicates the delimiter character used to delimit virtual directories in a blob name.</span></span>

<span data-ttu-id="fa97e-450">Som standard använder AzCopy / som avgränsningstecken.</span><span class="sxs-lookup"><span data-stu-id="fa97e-450">By default, AzCopy uses / as the delimiter character.</span></span> <span data-ttu-id="fa97e-451">AzCopy stöder dock använda alla vanliga tecken (t ex @, # eller %) som avgränsare.</span><span class="sxs-lookup"><span data-stu-id="fa97e-451">However, AzCopy supports using any common character (such as @, #, or %) as a delimiter.</span></span> <span data-ttu-id="fa97e-452">Om du måste inkludera en av dessa specialtecken på kommandoraden, omger du filnamnet med citattecken.</span><span class="sxs-lookup"><span data-stu-id="fa97e-452">If you need to include one of these special characters on the command line, enclose the file name with double quotes.</span></span>

<span data-ttu-id="fa97e-453">Det här alternativet gäller endast för att ladda ned blobbar.</span><span class="sxs-lookup"><span data-stu-id="fa97e-453">This option is only applicable for downloading blobs.</span></span>

<span data-ttu-id="fa97e-454">**Gäller för:** Blobbar</span><span class="sxs-lookup"><span data-stu-id="fa97e-454">**Applicable to:** Blobs</span></span>

### <a name="ncnumber-of-concurrent-operations"></a><span data-ttu-id="fa97e-455">/ NC: ”många-för-samtidiga-operationer”</span><span class="sxs-lookup"><span data-stu-id="fa97e-455">/NC:"number-of-concurrent-operations"</span></span>
<span data-ttu-id="fa97e-456">Anger antalet samtidiga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="fa97e-456">Specifies the number of concurrent operations.</span></span>

<span data-ttu-id="fa97e-457">AzCopy som standard startar ett visst antal samtidiga åtgärder att öka genomflödet data transfer.</span><span class="sxs-lookup"><span data-stu-id="fa97e-457">AzCopy by default starts a certain number of concurrent operations to increase the data transfer throughput.</span></span> <span data-ttu-id="fa97e-458">Observera att många samtidiga åtgärder i en miljö med låg bandbredd kan överväldigande nätverksanslutningen och förhindra operations fullständigt slutförs.</span><span class="sxs-lookup"><span data-stu-id="fa97e-458">Note that large number of concurrent operations in a low-bandwidth environment may overwhelm the network connection and prevent the operations from fully completing.</span></span> <span data-ttu-id="fa97e-459">Begränsning samtidiga åtgärder baserat på faktiska tillgänglig nätverksbandbredd.</span><span class="sxs-lookup"><span data-stu-id="fa97e-459">Throttle concurrent operations based on actual available network bandwidth.</span></span>

<span data-ttu-id="fa97e-460">Den övre gränsen för samtidiga åtgärder är 512.</span><span class="sxs-lookup"><span data-stu-id="fa97e-460">The upper limit for concurrent operations is 512.</span></span>

<span data-ttu-id="fa97e-461">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-461">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcetypeblob--table"></a><span data-ttu-id="fa97e-462">/ SourceType: ”Blob” | ”Table”</span><span class="sxs-lookup"><span data-stu-id="fa97e-462">/SourceType:"Blob" | "Table"</span></span>
<span data-ttu-id="fa97e-463">Anger att den `source` resursen är en blob som är tillgängliga i den lokala utvecklingsmiljö som körs i storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="fa97e-463">Specifies that the `source` resource is a blob available in the local development environment, running in the storage emulator.</span></span>

<span data-ttu-id="fa97e-464">**Gäller för:** Blobbar, tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-464">**Applicable to:** Blobs, Tables</span></span>

### <a name="desttypeblob--table"></a><span data-ttu-id="fa97e-465">/ DestType: ”Blob” | ”Table”</span><span class="sxs-lookup"><span data-stu-id="fa97e-465">/DestType:"Blob" | "Table"</span></span>
<span data-ttu-id="fa97e-466">Anger att den `destination` resursen är en blob som är tillgängliga i den lokala utvecklingsmiljö som körs i storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="fa97e-466">Specifies that the `destination` resource is a blob available in the local development environment, running in the storage emulator.</span></span>

<span data-ttu-id="fa97e-467">**Gäller för:** Blobbar, tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-467">**Applicable to:** Blobs, Tables</span></span>

### <a name="pkrskey1key2key3"></a><span data-ttu-id="fa97e-468">/ PKRS ”: key&#1;key2 key&#3;...”</span><span class="sxs-lookup"><span data-stu-id="fa97e-468">/PKRS:"key1#key2#key3#..."</span></span>
<span data-ttu-id="fa97e-469">Delar partitionsnyckelintervallet om du vill aktivera Exportera tabelldata parallellt, vilket ökar hastigheten för exporten.</span><span class="sxs-lookup"><span data-stu-id="fa97e-469">Splits the partition key range to enable exporting table data in parallel, which increases the speed of the export operation.</span></span>

<span data-ttu-id="fa97e-470">Om det här alternativet inte anges använder AzCopy en tråd för att exportera tabellentiteter.</span><span class="sxs-lookup"><span data-stu-id="fa97e-470">If this option is not specified, then AzCopy uses a single thread to export table entities.</span></span> <span data-ttu-id="fa97e-471">Om användaren anger /PKRS till exempel: ”aa #bb” och sedan AzCopy startar tre samtidiga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="fa97e-471">For example, if the user specifies /PKRS:"aa#bb", then AzCopy starts three concurrent operations.</span></span>

<span data-ttu-id="fa97e-472">Varje åtgärd exporterar en av tre partition nyckelintervall, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="fa97e-472">Each operation exports one of three partition key ranges, as shown below:</span></span>

  <span data-ttu-id="fa97e-473">[första partitionsnyckel, aa)</span><span class="sxs-lookup"><span data-stu-id="fa97e-473">[first-partition-key, aa)</span></span>

  <span data-ttu-id="fa97e-474">[aa, bb)</span><span class="sxs-lookup"><span data-stu-id="fa97e-474">[aa, bb)</span></span>

  <span data-ttu-id="fa97e-475">[bb senaste partitionsnyckel]</span><span class="sxs-lookup"><span data-stu-id="fa97e-475">[bb, last-partition-key]</span></span>

<span data-ttu-id="fa97e-476">**Gäller för:** tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-476">**Applicable to:** Tables</span></span>

### <a name="splitsizefile-size"></a><span data-ttu-id="fa97e-477">/ SplitSize: ”filstorlek”</span><span class="sxs-lookup"><span data-stu-id="fa97e-477">/SplitSize:"file-size"</span></span>
<span data-ttu-id="fa97e-478">Anger den exporterade filen dela storlek i MB, det minsta tillåtna värdet är 32.</span><span class="sxs-lookup"><span data-stu-id="fa97e-478">Specifies the exported file split size in MB, the minimal value allowed is 32.</span></span>

<span data-ttu-id="fa97e-479">Om det här alternativet inte anges, exporterar AzCopy tabelldata till en enda fil.</span><span class="sxs-lookup"><span data-stu-id="fa97e-479">If this option is not specified, AzCopy exports table data to a single file.</span></span>

<span data-ttu-id="fa97e-480">Om tabelldata exporteras till en blobb, och den exporterade filen storleken uppnår 200 GB gränsen för blob-storlek, sedan AzCopy delar upp den exporterade filen även om det här alternativet inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="fa97e-480">If the table data is exported to a blob, and the exported file size reaches the 200 GB limit for blob size, then AzCopy splits the exported file, even if this option is not specified.</span></span>

<span data-ttu-id="fa97e-481">**Gäller för:** tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-481">**Applicable to:** Tables</span></span>

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a><span data-ttu-id="fa97e-482">/ EntityOperation: ”InsertOrSkip” | ”InsertOrMerge” | ”InsertOrReplace”</span><span class="sxs-lookup"><span data-stu-id="fa97e-482">/EntityOperation:"InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"</span></span>
<span data-ttu-id="fa97e-483">Anger Tabellfunktioner för import av data.</span><span class="sxs-lookup"><span data-stu-id="fa97e-483">Specifies the table data import behavior.</span></span>

* <span data-ttu-id="fa97e-484">InsertOrSkip - hoppar över en befintlig entitet eller infogar en ny entitet om den inte finns i tabellen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-484">InsertOrSkip - Skips an existing entity or inserts a new entity if it does not exist in the table.</span></span>
* <span data-ttu-id="fa97e-485">InsertOrMerge - sammanfogar en befintlig entitet eller infogar en ny entitet om den inte finns i tabellen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-485">InsertOrMerge - Merges an existing entity or inserts a new entity if it does not exist in the table.</span></span>
* <span data-ttu-id="fa97e-486">InsertOrReplace - ersätter en befintlig entitet eller infogar en ny entitet om den inte finns i tabellen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-486">InsertOrReplace - Replaces an existing entity or inserts a new entity if it does not exist in the table.</span></span>

<span data-ttu-id="fa97e-487">**Gäller för:** tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-487">**Applicable to:** Tables</span></span>

### <a name="manifestmanifest-file"></a><span data-ttu-id="fa97e-488">/ Manifest: ”manifest-fil”</span><span class="sxs-lookup"><span data-stu-id="fa97e-488">/Manifest:"manifest-file"</span></span>
<span data-ttu-id="fa97e-489">Anger manifestfilen för tabellen exportera och importera igen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-489">Specifies the manifest file for the table export and import operation.</span></span>

<span data-ttu-id="fa97e-490">Det här alternativet är valfritt under exportåtgärden, AzCopy genererar en manifestfil med fördefinierade namnet om det här alternativet inte anges.</span><span class="sxs-lookup"><span data-stu-id="fa97e-490">This option is optional during the export operation, AzCopy generates a manifest file with predefined name if this option is not specified.</span></span>

<span data-ttu-id="fa97e-491">Det här alternativet krävs under importen för att hitta datafilerna.</span><span class="sxs-lookup"><span data-stu-id="fa97e-491">This option is required during the import operation for locating the data files.</span></span>

<span data-ttu-id="fa97e-492">**Gäller för:** tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-492">**Applicable to:** Tables</span></span>

### <a name="synccopy"></a><span data-ttu-id="fa97e-493">/ SyncCopy</span><span class="sxs-lookup"><span data-stu-id="fa97e-493">/SyncCopy</span></span>
<span data-ttu-id="fa97e-494">Anger om du vill kopiera synkront blobbar eller filer mellan två Azure Storage-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="fa97e-494">Indicates whether to synchronously copy blobs or files between two Azure Storage endpoints.</span></span>

<span data-ttu-id="fa97e-495">AzCopy som standard använder serversidan asynkron kopia.</span><span class="sxs-lookup"><span data-stu-id="fa97e-495">AzCopy by default uses server-side asynchronous copy.</span></span> <span data-ttu-id="fa97e-496">Ange det här alternativet för att utföra en synkron kopia som laddar ned filer eller BLOB till lokalt minne och överför dem till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="fa97e-496">Specify this option to perform a synchronous copy, which downloads blobs or files to local memory and then uploads them to Azure Storage.</span></span>

<span data-ttu-id="fa97e-497">Du kan använda det här alternativet när du kopierar filer i Blob storage fillagring, eller från Blob storage till File storage och vice versa.</span><span class="sxs-lookup"><span data-stu-id="fa97e-497">You can use this option when copying files within Blob storage, within File storage, or from Blob storage to File storage or vice versa.</span></span>

<span data-ttu-id="fa97e-498">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-498">**Applicable to:** Blobs, Files</span></span>

### <a name="setcontenttypecontent-type"></a><span data-ttu-id="fa97e-499">/ SetContentType: ”content-type”</span><span class="sxs-lookup"><span data-stu-id="fa97e-499">/SetContentType:"content-type"</span></span>
<span data-ttu-id="fa97e-500">Anger MIME content-type för mål-BLOB eller filer.</span><span class="sxs-lookup"><span data-stu-id="fa97e-500">Specifies the MIME content type for destination blobs or files.</span></span>

<span data-ttu-id="fa97e-501">AzCopy anger content-type för en blob eller en fil till program/oktett-ström som standard.</span><span class="sxs-lookup"><span data-stu-id="fa97e-501">AzCopy sets the content type for a blob or file to application/octet-stream by default.</span></span> <span data-ttu-id="fa97e-502">Du kan ange content-type för alla blobbar eller filer genom att ange ett värde för det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="fa97e-502">You can set the content type for all blobs or files by explicitly specifying a value for this option.</span></span>

<span data-ttu-id="fa97e-503">Om du anger det här alternativet utan ett värde som anger AzCopy varje blob eller filens innehållstyp enligt dess filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="fa97e-503">If you specify this option without a value, then AzCopy sets each blob or file's content type according to its file extension.</span></span>

<span data-ttu-id="fa97e-504">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="fa97e-504">**Applicable to:** Blobs, Files</span></span>

### <a name="payloadformatjson--csv"></a><span data-ttu-id="fa97e-505">/ PayloadFormat: ”JSON” | ”CSV”</span><span class="sxs-lookup"><span data-stu-id="fa97e-505">/PayloadFormat:"JSON" | "CSV"</span></span>
<span data-ttu-id="fa97e-506">Anger formatet för den exporterade datafilen i tabellen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-506">Specifies the format of the table exported data file.</span></span>

<span data-ttu-id="fa97e-507">Om det här alternativet inte anges, exporterar AzCopy tabell datafilen i JSON-format som standard.</span><span class="sxs-lookup"><span data-stu-id="fa97e-507">If this option is not specified, by default AzCopy exports table data file in JSON format.</span></span>

<span data-ttu-id="fa97e-508">**Gäller för:** tabeller</span><span class="sxs-lookup"><span data-stu-id="fa97e-508">**Applicable to:** Tables</span></span>

## <a name="known-issues-and-best-practices"></a><span data-ttu-id="fa97e-509">Kända problem och bästa praxis</span><span class="sxs-lookup"><span data-stu-id="fa97e-509">Known Issues and Best Practices</span></span>
### <a name="limit-concurrent-writes-while-copying-data"></a><span data-ttu-id="fa97e-510">Begränsa samtidiga skrivningar vid kopiering av data</span><span class="sxs-lookup"><span data-stu-id="fa97e-510">Limit concurrent writes while copying data</span></span>
<span data-ttu-id="fa97e-511">När du kopierar blobar eller filer med AzCopy, Tänk på att ett annat program kan ändra data när du kopierar den.</span><span class="sxs-lookup"><span data-stu-id="fa97e-511">When you copy blobs or files with AzCopy, keep in mind that another application may be modifying the data while you are copying it.</span></span> <span data-ttu-id="fa97e-512">Kontrollera om det är möjligt att du kopierar data inte ändras under kopieringen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-512">If possible, ensure that the data you are copying is not being modified during the copy operation.</span></span> <span data-ttu-id="fa97e-513">Till exempel när du kopierar en virtuell Hårddisk som är kopplad till en virtuell Azure-dator, kontrollera att inga andra program för tillfället skrivning till den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="fa97e-513">For example, when copying a VHD associated with an Azure virtual machine, make sure that no other applications are currently writing to the VHD.</span></span> <span data-ttu-id="fa97e-514">Ett bra sätt att göra detta är genom leasing resurs som ska kopieras.</span><span class="sxs-lookup"><span data-stu-id="fa97e-514">A good way to do this is by leasing the resource to be copied.</span></span> <span data-ttu-id="fa97e-515">Alternativt kan du skapa en ögonblicksbild av den virtuella Hårddisken först och sedan kopiera ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="fa97e-515">Alternately, you can create a snapshot of the VHD first and then copy the snapshot.</span></span>

<span data-ttu-id="fa97e-516">Om du inte hindra andra program från att skriva till filer eller blobbar när de kopieras sedan Kom ihåg att när jobbet har slutförts, kopiera resurserna kan inte längre har fullständig paritet med resurserna som källa.</span><span class="sxs-lookup"><span data-stu-id="fa97e-516">If you cannot prevent other applications from writing to blobs or files while they are being copied, then keep in mind that by the time the job finishes, the copied resources may no longer have full parity with the source resources.</span></span>

### <a name="run-one-azcopy-instance-on-one-machine"></a><span data-ttu-id="fa97e-517">Köra en AzCopy-instans på en dator.</span><span class="sxs-lookup"><span data-stu-id="fa97e-517">Run one AzCopy instance on one machine.</span></span>
<span data-ttu-id="fa97e-518">AzCopy är utformat för att maximera användningen av din datorresurser som påskyndar dataöverföringen rekommenderar vi du kör endast en AzCopy-instans på en dator och ange alternativet `/NC` om du behöver fler samtidiga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="fa97e-518">AzCopy is designed to maximize the utilization of your machine resource to accelerate the data transfer, we recommend you run only one AzCopy instance on one machine, and specify the option `/NC` if you need more concurrent operations.</span></span> <span data-ttu-id="fa97e-519">Mer information skriver du `AzCopy /?:NC` på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="fa97e-519">For more details, type `AzCopy /?:NC` at the command line.</span></span>

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a><span data-ttu-id="fa97e-520">Aktivera FIPS-kompatibla MD5 algoritmer för AzCopy när du ”Använd FIPS-kompatibla algoritmer för kryptering, hashing och signering”.</span><span class="sxs-lookup"><span data-stu-id="fa97e-520">Enable FIPS compliant MD5 algorithms for AzCopy when you "Use FIPS compliant algorithms for encryption, hashing and signing".</span></span>
<span data-ttu-id="fa97e-521">AzCopy som standard använder .NET MD5-implementering för att beräkna MD5 när du kopierar objekt, men det finns vissa säkerhetskrav som behöver AzCopy för att aktivera FIPS-kompatibla MD5-inställningen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-521">AzCopy by default uses .NET MD5 implementation to calculate the MD5 when copying objects, but there are some security requirements that need AzCopy to enable FIPS compliant MD5 setting.</span></span>

<span data-ttu-id="fa97e-522">Du kan skapa en app.config-fil `AzCopy.exe.config` med egenskapen `AzureStorageUseV1MD5` och placera den reservera med AzCopy.exe.</span><span class="sxs-lookup"><span data-stu-id="fa97e-522">You can create an app.config file `AzCopy.exe.config` with property `AzureStorageUseV1MD5` and put it aside with AzCopy.exe.</span></span>

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

<span data-ttu-id="fa97e-523">För egenskapen ”AzureStorageUseV1MD5” • True - använder standardvärdet AzCopy .NET MD5-implementering.</span><span class="sxs-lookup"><span data-stu-id="fa97e-523">For property "AzureStorageUseV1MD5" • True - The default value, AzCopy uses .NET MD5 implementation.</span></span>
<span data-ttu-id="fa97e-524">• False – AzCopy använder FIPS-kompatibla MD5-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="fa97e-524">• False – AzCopy uses FIPS compliant MD5 algorithm.</span></span>

<span data-ttu-id="fa97e-525">Observera att FIPS-kompatibla algoritmer är inaktiverad som standard på din Windows-dator, du kan skriva secpol.msc i din körningsfönstret och kontrollera den här växeln på Säkerhet -> lokala principer -> säkerhet alternativ -> Systemkryptografi: Använd FIPS-kompatibel algoritmer för kryptering, hashing och signering.</span><span class="sxs-lookup"><span data-stu-id="fa97e-525">Note that FIPS compliant algorithms is disabled by default on your Windows machine, you can type secpol.msc in your Run window and check this switch at Security Setting->Local Policy->Security Options->System cryptography: Use FIPS compliant algorithms for encryption, hashing and signing.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa97e-526">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fa97e-526">Next steps</span></span>
<span data-ttu-id="fa97e-527">Mer information om Azure Storage och AzCopy finns i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="fa97e-527">For more information about Azure Storage and AzCopy, see the following resources:</span></span>

### <a name="azure-storage-documentation"></a><span data-ttu-id="fa97e-528">Azure Storage-dokumentation:</span><span class="sxs-lookup"><span data-stu-id="fa97e-528">Azure Storage documentation:</span></span>
* [<span data-ttu-id="fa97e-529">Introduktion till Azure Storage</span><span class="sxs-lookup"><span data-stu-id="fa97e-529">Introduction to Azure Storage</span></span>](../storage-introduction.md)
* [<span data-ttu-id="fa97e-530">Använda Blob storage från .NET</span><span class="sxs-lookup"><span data-stu-id="fa97e-530">How to use Blob storage from .NET</span></span>](../blobs/storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="fa97e-531">Hur du använder File storage från .NET</span><span class="sxs-lookup"><span data-stu-id="fa97e-531">How to use File storage from .NET</span></span>](../storage-dotnet-how-to-use-files.md)
* [<span data-ttu-id="fa97e-532">Använda Table storage från .NET</span><span class="sxs-lookup"><span data-stu-id="fa97e-532">How to use Table storage from .NET</span></span>](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [<span data-ttu-id="fa97e-533">Skapa, hantera eller ta bort ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="fa97e-533">How to create, manage, or delete a storage account</span></span>](../storage-create-storage-account.md)
* [<span data-ttu-id="fa97e-534">Överföra data med AzCopy på Linux</span><span class="sxs-lookup"><span data-stu-id="fa97e-534">Transfer data with AzCopy on Linux</span></span>](storage-use-azcopy-linux.md)

### <a name="azure-storage-blog-posts"></a><span data-ttu-id="fa97e-535">Azure Storage blogginlägg:</span><span class="sxs-lookup"><span data-stu-id="fa97e-535">Azure Storage blog posts:</span></span>
* [<span data-ttu-id="fa97e-536">Introduktion till Azure Storage Data Movement Library Preview</span><span class="sxs-lookup"><span data-stu-id="fa97e-536">Introducing Azure Storage Data Movement Library Preview</span></span>](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [<span data-ttu-id="fa97e-537">AzCopy: Introduktion till synkron kopia och anpassade innehållstyp</span><span class="sxs-lookup"><span data-stu-id="fa97e-537">AzCopy: Introducing synchronous copy and customized content type</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [<span data-ttu-id="fa97e-538">AzCopy: Om allmän tillgänglighet AzCopy 3.0 plus förhandsversionen av AzCopy 4.0 med tabell- och support</span><span class="sxs-lookup"><span data-stu-id="fa97e-538">AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [<span data-ttu-id="fa97e-539">AzCopy: Optimerade för storskaliga kopiera scenarier</span><span class="sxs-lookup"><span data-stu-id="fa97e-539">AzCopy: Optimized for Large-Scale Copy Scenarios</span></span>](http://go.microsoft.com/fwlink/?LinkId=507682)
* [<span data-ttu-id="fa97e-540">AzCopy: Stöd för geo-redundant lagring med läsbehörighet</span><span class="sxs-lookup"><span data-stu-id="fa97e-540">AzCopy: Support for read-access geo-redundant storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [<span data-ttu-id="fa97e-541">AzCopy: Överföra data med nytt starta läge och SAS-token</span><span class="sxs-lookup"><span data-stu-id="fa97e-541">AzCopy: Transfer data with re-startable mode and SAS token</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [<span data-ttu-id="fa97e-542">AzCopy: Använder mellan konto kopiera Blob</span><span class="sxs-lookup"><span data-stu-id="fa97e-542">AzCopy: Using cross-account Copy Blob</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [<span data-ttu-id="fa97e-543">AzCopy: Överför/hämta filer för Azure-BLOB</span><span class="sxs-lookup"><span data-stu-id="fa97e-543">AzCopy: Uploading/downloading files for Azure Blobs</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

