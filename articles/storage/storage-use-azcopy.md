---
title: aaaCopy eller flytta data tooAzure lagring med AzCopy i Windows | Microsoft Docs
description: "Använd hello AzCopy på Windows-verktyget toomove eller kopiera data tooor från blob-, tabell- och innehåll. Kopiera data tooAzure lagring från lokala filer eller kopiera data inom eller mellan lagringskonton. Enkelt migrera dina data tooAzure lagring."
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
ms.openlocfilehash: a77db84c3a3e06f0ad4e87d02b14a5c62ed8d9ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-hello-azcopy-on-windows"></a><span data-ttu-id="b6446-105">Överföra data med hello AzCopy i Windows</span><span class="sxs-lookup"><span data-stu-id="b6446-105">Transfer data with hello AzCopy on Windows</span></span>
<span data-ttu-id="b6446-106">AzCopy är ett kommandoradsverktyg som utformats för att kopiera data tooand från Microsoft Azure Blob-, fil- och Table storage med hjälp av enkla kommandon med optimala prestanda.</span><span class="sxs-lookup"><span data-stu-id="b6446-106">AzCopy is a command-line utility designed for copying data tooand from Microsoft Azure Blob, File, and Table storage using simple commands with optimal performance.</span></span> <span data-ttu-id="b6446-107">Du kan kopiera data från en objektet tooanother inom ditt lagringskonto eller mellan lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="b6446-107">You can copy data from one object tooanother within your storage account, or between storage accounts.</span></span>

<span data-ttu-id="b6446-108">Det finns två versioner av AzCopy som du kan hämta.</span><span class="sxs-lookup"><span data-stu-id="b6446-108">There are two versions of AzCopy that you can download.</span></span> <span data-ttu-id="b6446-109">AzCopy i Windows har skapats med .NET Framework och erbjuder kommandoradsalternativ för Windows-formatet.</span><span class="sxs-lookup"><span data-stu-id="b6446-109">AzCopy on Windows is built with .NET Framework, and offers Windows style command-line options.</span></span> <span data-ttu-id="b6446-110">[AzCopy på Linux](storage-use-azcopy-linux.md) har byggts med .NET Core Framework som riktar sig till Linux-plattformar som erbjuder POSIX format kommandoradsalternativ.</span><span class="sxs-lookup"><span data-stu-id="b6446-110">[AzCopy on Linux](storage-use-azcopy-linux.md) is built with .NET Core Framework which targets Linux platforms offering POSIX style command-line options.</span></span> <span data-ttu-id="b6446-111">Den här artikeln beskriver AzCopy i Windows.</span><span class="sxs-lookup"><span data-stu-id="b6446-111">This article covers AzCopy on Windows.</span></span>

## <a name="download-and-install-azcopy"></a><span data-ttu-id="b6446-112">Hämta och installera AzCopy</span><span class="sxs-lookup"><span data-stu-id="b6446-112">Download and install AzCopy</span></span>
### <a name="azcopy-on-windows"></a><span data-ttu-id="b6446-113">AzCopy i Windows</span><span class="sxs-lookup"><span data-stu-id="b6446-113">AzCopy on Windows</span></span>
<span data-ttu-id="b6446-114">Hämta hello [senaste versionen av AzCopy på Windows](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="b6446-114">Download hello [latest version of AzCopy on Windows](http://aka.ms/downloadazcopy).</span></span>

#### <a name="installation-on-windows"></a><span data-ttu-id="b6446-115">Installation på Windows</span><span class="sxs-lookup"><span data-stu-id="b6446-115">Installation on Windows</span></span>
<span data-ttu-id="b6446-116">När du har installerat AzCopy i Windows med hello installer, öppna ett kommandofönster och gå toohello installationskatalogen för AzCopy på din dator - där hello `AzCopy.exe` körbara finns.</span><span class="sxs-lookup"><span data-stu-id="b6446-116">After installing AzCopy on Windows using hello installer, open a command window and navigate toohello AzCopy installation directory on your computer - where hello `AzCopy.exe` executable is located.</span></span> <span data-ttu-id="b6446-117">Om du vill kan du lägga till hello AzCopy tooyour systemsökvägen installationsplatsen.</span><span class="sxs-lookup"><span data-stu-id="b6446-117">If desired, you can add hello AzCopy installation location tooyour system path.</span></span> <span data-ttu-id="b6446-118">Som standard installeras AzCopy för`%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` eller `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="b6446-118">By default, AzCopy is installed too`%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` or `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.</span></span>

## <a name="writing-your-first-azcopy-command"></a><span data-ttu-id="b6446-119">Skriva ditt första AzCopy-kommandot</span><span class="sxs-lookup"><span data-stu-id="b6446-119">Writing your first AzCopy command</span></span>
<span data-ttu-id="b6446-120">grundläggande hello-syntaxen för AzCopy kommandon är:</span><span class="sxs-lookup"><span data-stu-id="b6446-120">hello basic syntax for AzCopy commands is:</span></span>

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

<span data-ttu-id="b6446-121">hello som följande exempel visar en mängd olika scenarier för att kopiera data tooand från Microsoft Azure-Blobbar, filer och tabeller.</span><span class="sxs-lookup"><span data-stu-id="b6446-121">hello following examples demonstrate a variety of scenarios for copying data tooand from Microsoft Azure Blobs, Files, and Tables.</span></span> <span data-ttu-id="b6446-122">Se toohello [AzCopy parametrar](#azcopy-parameters) avsnittet för en detaljerad förklaring av hello parametrar som används i varje prov.</span><span class="sxs-lookup"><span data-stu-id="b6446-122">Refer toohello [AzCopy Parameters](#azcopy-parameters) section for a detailed explanation of hello parameters used in each sample.</span></span>

## <a name="blob-download"></a><span data-ttu-id="b6446-123">BLOB: ladda ned</span><span class="sxs-lookup"><span data-stu-id="b6446-123">Blob: Download</span></span>
### <a name="download-single-blob"></a><span data-ttu-id="b6446-124">Hämta en enda blob</span><span class="sxs-lookup"><span data-stu-id="b6446-124">Download single blob</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

<span data-ttu-id="b6446-125">Observera att om hello mappen `C:\myfolder` finns inte, AzCopy skapar den och ladda ned `abc.txt ` till hello ny mapp.</span><span class="sxs-lookup"><span data-stu-id="b6446-125">Note that if hello folder `C:\myfolder` does not exist, AzCopy will create it and download `abc.txt ` into hello new folder.</span></span>

### <a name="download-single-blob-from-secondary-region"></a><span data-ttu-id="b6446-126">Hämta en enda blob från sekundär region</span><span class="sxs-lookup"><span data-stu-id="b6446-126">Download single blob from secondary region</span></span>

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

<span data-ttu-id="b6446-127">Observera att du måste ha läsbehörighet geo-redundant lagring aktiverad.</span><span class="sxs-lookup"><span data-stu-id="b6446-127">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="download-all-blobs"></a><span data-ttu-id="b6446-128">Hämta alla BLOB</span><span class="sxs-lookup"><span data-stu-id="b6446-128">Download all blobs</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

<span data-ttu-id="b6446-129">Anta hello följande blobbar finns i angivna hello-behållaren:</span><span class="sxs-lookup"><span data-stu-id="b6446-129">Assume hello following blobs reside in hello specified container:</span></span>  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

<span data-ttu-id="b6446-130">Efter hello hämtningen hello directory `C:\myfolder` innehåller hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="b6446-130">After hello download operation, hello directory `C:\myfolder` will include hello following files:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

<span data-ttu-id="b6446-131">Om du inte anger alternativet `/S`, kommer att hämtas inga blobbar.</span><span class="sxs-lookup"><span data-stu-id="b6446-131">If you do not specify option `/S`, no blobs will be downloaded.</span></span>

### <a name="download-blobs-with-specified-prefix"></a><span data-ttu-id="b6446-132">Ladda ned blobbar med angivna prefix</span><span class="sxs-lookup"><span data-stu-id="b6446-132">Download blobs with specified prefix</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

<span data-ttu-id="b6446-133">Anta hello följande blobbar finns i hello angivna behållaren.</span><span class="sxs-lookup"><span data-stu-id="b6446-133">Assume hello following blobs reside in hello specified container.</span></span> <span data-ttu-id="b6446-134">Alla blobbar som börjar med prefixet hello `a` hämtas:</span><span class="sxs-lookup"><span data-stu-id="b6446-134">All blobs beginning with hello prefix `a` will be downloaded:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

<span data-ttu-id="b6446-135">Efter hello hämtningen hello mappen `C:\myfolder` innehåller hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="b6446-135">After hello download operation, hello folder `C:\myfolder` will include hello following files:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

<span data-ttu-id="b6446-136">hello prefix gäller toohello virtuella katalogen, som utgör hello första delen av hello blob-namnet.</span><span class="sxs-lookup"><span data-stu-id="b6446-136">hello prefix applies toohello virtual directory, which forms hello first part of hello blob name.</span></span> <span data-ttu-id="b6446-137">I hello exemplet ovan matchar hello virtuella katalogen inte hello angivna prefix, så inte hämtas.</span><span class="sxs-lookup"><span data-stu-id="b6446-137">In hello example shown above, hello virtual directory does not match hello specified prefix, so it is not downloaded.</span></span> <span data-ttu-id="b6446-138">Dessutom, om hello alternativet `\S` anges AzCopy inte kommer att hämta alla blobbar.</span><span class="sxs-lookup"><span data-stu-id="b6446-138">In addition, if hello option `\S` is not specified, AzCopy will not download any blobs.</span></span>

### <a name="set-hello-last-modified-time-of-exported-files-toobe-same-as-hello-source-blobs"></a><span data-ttu-id="b6446-139">Ange hello modifierades senast av exporterade filerna toobe samma som hello källa blobbar</span><span class="sxs-lookup"><span data-stu-id="b6446-139">Set hello last-modified time of exported files toobe same as hello source blobs</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

<span data-ttu-id="b6446-140">Du kan också utesluta blobbar från hello hämtningen baserat på deras tid för senaste ändring.</span><span class="sxs-lookup"><span data-stu-id="b6446-140">You can also exclude blobs from hello download operation based on their last-modified time.</span></span> <span data-ttu-id="b6446-141">Till exempel om du vill tooexclude blobbar vars senaste ändringstiden är hello samma eller en senare än hello målfilen, lägga till hello `/XN` alternativ:</span><span class="sxs-lookup"><span data-stu-id="b6446-141">For example, if you want tooexclude blobs whose last modified time is hello same or newer than hello destination file, add hello `/XN` option:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

<span data-ttu-id="b6446-142">Eller om du vill tooexclude blobbar vars senaste ändringstiden är hello samma eller äldre än hello målfilen, lägger du till hello `/XO` alternativ:</span><span class="sxs-lookup"><span data-stu-id="b6446-142">Or if you want tooexclude blobs whose last modified time is hello same or older than hello destination file, add hello `/XO` option:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="blob-upload"></a><span data-ttu-id="b6446-143">BLOB: Överför</span><span class="sxs-lookup"><span data-stu-id="b6446-143">Blob: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="b6446-144">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="b6446-144">Upload single file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

<span data-ttu-id="b6446-145">Om hello angivna målbehållare inte finns, skapar den AzCopy och överför hello-fil i den.</span><span class="sxs-lookup"><span data-stu-id="b6446-145">If hello specified destination container does not exist, AzCopy will create it and upload hello file into it.</span></span>

### <a name="upload-single-file-toovirtual-directory"></a><span data-ttu-id="b6446-146">Överför en fil toovirtual directory</span><span class="sxs-lookup"><span data-stu-id="b6446-146">Upload single file toovirtual directory</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="b6446-147">Om hello angivna virtuella katalogen inte finns, AzCopy kommer att överföra hello filen tooinclude hello virtuell katalog på sitt namn (*t.ex.*, `vd/abc.txt` i hello-exemplet ovan).</span><span class="sxs-lookup"><span data-stu-id="b6446-147">If hello specified virtual directory does not exist, AzCopy will upload hello file tooinclude hello virtual directory in its name (*e.g.*, `vd/abc.txt` in hello example above).</span></span>

### <a name="upload-all-files"></a><span data-ttu-id="b6446-148">Ladda upp alla filer</span><span class="sxs-lookup"><span data-stu-id="b6446-148">Upload all files</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

<span data-ttu-id="b6446-149">Om du anger alternativet `/S` överföringar hello innehållet i hello angetts directory tooBlob lagring rekursivt, vilket innebär att alla undermappar och filer överförs också.</span><span class="sxs-lookup"><span data-stu-id="b6446-149">Specifying option `/S` uploads hello contents of hello specified directory tooBlob storage recursively, meaning that all subfolders and their files will be uploaded as well.</span></span> <span data-ttu-id="b6446-150">Anta exempelvis att hello följande filer finns i mappen `C:\myfolder`:</span><span class="sxs-lookup"><span data-stu-id="b6446-150">For instance, assume hello following files reside in folder `C:\myfolder`:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

<span data-ttu-id="b6446-151">Efter hello överföringen innehåller hello behållaren hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="b6446-151">After hello upload operation, hello container will include hello following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

<span data-ttu-id="b6446-152">Om du inte anger alternativet `/S`, AzCopy kommer inte att överföra rekursivt.</span><span class="sxs-lookup"><span data-stu-id="b6446-152">If you do not specify option `/S`, AzCopy will not upload recursively.</span></span> <span data-ttu-id="b6446-153">Efter hello överföringen innehåller hello behållaren hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="b6446-153">After hello upload operation, hello container will include hello following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="b6446-154">Överföra filer som matchar angivna mönstret</span><span class="sxs-lookup"><span data-stu-id="b6446-154">Upload files matching specified pattern</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

<span data-ttu-id="b6446-155">Anta hello följande filer finns i mappen `C:\myfolder`:</span><span class="sxs-lookup"><span data-stu-id="b6446-155">Assume hello following files reside in folder `C:\myfolder`:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

<span data-ttu-id="b6446-156">Efter hello överföringen innehåller hello behållaren hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="b6446-156">After hello upload operation, hello container will include hello following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

<span data-ttu-id="b6446-157">Om du inte anger alternativet `/S`, AzCopy kommer bara att överföra blob som inte finns i en virtuell katalog:</span><span class="sxs-lookup"><span data-stu-id="b6446-157">If you do not specify option `/S`, AzCopy will only upload blobs that don't reside in a virtual directory:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-hello-mime-content-type-of-a-destination-blob"></a><span data-ttu-id="b6446-158">Ange hello MIME content-type för en mål-blob</span><span class="sxs-lookup"><span data-stu-id="b6446-158">Specify hello MIME content type of a destination blob</span></span>
<span data-ttu-id="b6446-159">Som standard AzCopy anger hello innehållstypen för en mål-blob för`application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="b6446-159">By default, AzCopy sets hello content type of a destination blob too`application/octet-stream`.</span></span> <span data-ttu-id="b6446-160">Från och med version 3.1.0, anger du hello innehållstyp via hello alternativet `/SetContentType:[content-type]`.</span><span class="sxs-lookup"><span data-stu-id="b6446-160">Beginning with version 3.1.0, you can explicitly specify hello content type via hello option `/SetContentType:[content-type]`.</span></span> <span data-ttu-id="b6446-161">Den här syntaxen anger hello content-type för alla blobbar i en överföringen.</span><span class="sxs-lookup"><span data-stu-id="b6446-161">This syntax sets hello content type for all blobs in an upload operation.</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

<span data-ttu-id="b6446-162">Om du anger `/SetContentType` utan värde, AzCopy ska ange varje blob eller filens innehållstyp enligt tooits filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="b6446-162">If you specify `/SetContentType` without a value, then AzCopy will set each blob or file's content type according tooits file extension.</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="blob-copy"></a><span data-ttu-id="b6446-163">BLOB: kopiera</span><span class="sxs-lookup"><span data-stu-id="b6446-163">Blob: Copy</span></span>
### <a name="copy-single-blob-within-storage-account"></a><span data-ttu-id="b6446-164">Kopiera enda blob inom lagringskonto</span><span class="sxs-lookup"><span data-stu-id="b6446-164">Copy single blob within Storage account</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="b6446-165">När du kopierar en blobb inom ett lagringskonto, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="b6446-165">When you copy a blob within a Storage account, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-across-storage-accounts"></a><span data-ttu-id="b6446-166">Kopiera enda blob på Storage-konton</span><span class="sxs-lookup"><span data-stu-id="b6446-166">Copy single blob across Storage accounts</span></span>

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

<span data-ttu-id="b6446-167">När du kopierar en blobb mellan lagringskonton, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="b6446-167">When you copy a blob across Storage accounts, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-from-secondary-region-tooprimary-region"></a><span data-ttu-id="b6446-168">Kopiera enda blob från sekundär region tooprimary region</span><span class="sxs-lookup"><span data-stu-id="b6446-168">Copy single blob from secondary region tooprimary region</span></span>

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

<span data-ttu-id="b6446-169">Observera att du måste ha läsbehörighet geo-redundant lagring aktiverad.</span><span class="sxs-lookup"><span data-stu-id="b6446-169">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a><span data-ttu-id="b6446-170">Kopiera enda blob och dess ögonblicksbilder på Storage-konton</span><span class="sxs-lookup"><span data-stu-id="b6446-170">Copy single blob and its snapshots across Storage accounts</span></span>

```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

<span data-ttu-id="b6446-171">Efter hello kopieringsåtgärden innehåller hello Målbehållaren hello blob och dess ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="b6446-171">After hello copy operation, hello target container will include hello blob and its snapshots.</span></span> <span data-ttu-id="b6446-172">Under förutsättning att hello blob i hello-exemplet ovan har två ögonblicksbilder, hello behållaren innehåller följande hello blob och ögonblicksbilder:</span><span class="sxs-lookup"><span data-stu-id="b6446-172">Assuming hello blob in hello example above has two snapshots, hello container will include hello following blob and snapshots:</span></span>

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a><span data-ttu-id="b6446-173">Kopiera synkront blobar på lagringskonton</span><span class="sxs-lookup"><span data-stu-id="b6446-173">Synchronously copy blobs across Storage accounts</span></span>
<span data-ttu-id="b6446-174">AzCopy som standard kopierar data mellan två slutpunkter för lagring asynkront.</span><span class="sxs-lookup"><span data-stu-id="b6446-174">AzCopy by default copies data between two storage endpoints asynchronously.</span></span> <span data-ttu-id="b6446-175">Därför hello kopieringen körs i hello bakgrunden med hjälp av ledig bandbreddskapacitet som har inga SLA vad gäller hur snabbt en blob som ska kopieras och AzCopy kontrollerar regelbundet hello-kopian status tills hello kopiera är slutförd eller misslyckad.</span><span class="sxs-lookup"><span data-stu-id="b6446-175">Therefore, hello copy operation will run in hello background using spare bandwidth capacity that has no SLA in terms of how fast a blob will be copied, and AzCopy will periodically check hello copy status until hello copying is completed or failed.</span></span>

<span data-ttu-id="b6446-176">Hej `/SyncCopy` alternativet ser du till att hello kopieringsåtgärden kommer konsekvent hastighet.</span><span class="sxs-lookup"><span data-stu-id="b6446-176">hello `/SyncCopy` option ensures that hello copy operation will get consistent speed.</span></span> <span data-ttu-id="b6446-177">AzCopy utför hello synkron kopia genom att hämta hello blobbar toocopy från hello anges källa toolocal minne, och överföra dem toohello Blob-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="b6446-177">AzCopy performs hello synchronous copy by downloading hello blobs toocopy from hello specified source toolocal memory, and then uploading them toohello Blob storage destination.</span></span>

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

<span data-ttu-id="b6446-178">`/SyncCopy`kan skapa ytterligare utgång kostnaden jämfört med tooasynchronous kopiera hello rekommenderade metoden är toouse det här alternativet i en Azure VM i hello samma region som din datakälla konto tooavoid utgång lagringskostnaden.</span><span class="sxs-lookup"><span data-stu-id="b6446-178">`/SyncCopy` might generate additional egress cost compared tooasynchronous copy, hello recommended approach is toouse this option in an Azure VM that is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="file-download"></a><span data-ttu-id="b6446-179">Fil: ladda ned</span><span class="sxs-lookup"><span data-stu-id="b6446-179">File: Download</span></span>
### <a name="download-single-file"></a><span data-ttu-id="b6446-180">Hämta en fil</span><span class="sxs-lookup"><span data-stu-id="b6446-180">Download single file</span></span>

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

<span data-ttu-id="b6446-181">Om hello anges källan är en Azure-filresurs, måste du antingen ange hello exakta filnamnet (*t.ex.* `abc.txt`) toodownload en enskild fil, eller ange alternativet `/S` toodownload alla filer i hello resurs rekursivt.</span><span class="sxs-lookup"><span data-stu-id="b6446-181">If hello specified source is an Azure file share, then you must either specify hello exact file name, (*e.g.* `abc.txt`) toodownload a single file, or specify option `/S` toodownload all files in hello share recursively.</span></span> <span data-ttu-id="b6446-182">Försök toospecify både filmönstret och alternativet `/S` tillsammans resulterar i ett fel.</span><span class="sxs-lookup"><span data-stu-id="b6446-182">Attempting toospecify both a file pattern and option `/S` together will result in an error.</span></span>

### <a name="download-all-files"></a><span data-ttu-id="b6446-183">Hämta alla filer</span><span class="sxs-lookup"><span data-stu-id="b6446-183">Download all files</span></span>

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

<span data-ttu-id="b6446-184">Observera att inte hämtas alla tomma mappar.</span><span class="sxs-lookup"><span data-stu-id="b6446-184">Note that any empty folders will not be downloaded.</span></span>

## <a name="file-upload"></a><span data-ttu-id="b6446-185">Fil: Överför</span><span class="sxs-lookup"><span data-stu-id="b6446-185">File: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="b6446-186">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="b6446-186">Upload single file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files"></a><span data-ttu-id="b6446-187">Ladda upp alla filer</span><span class="sxs-lookup"><span data-stu-id="b6446-187">Upload all files</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

<span data-ttu-id="b6446-188">Observera att alla tomma mappar inte laddas upp.</span><span class="sxs-lookup"><span data-stu-id="b6446-188">Note that any empty folders will not be uploaded.</span></span>

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="b6446-189">Överföra filer som matchar angivna mönstret</span><span class="sxs-lookup"><span data-stu-id="b6446-189">Upload files matching specified pattern</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="file-copy"></a><span data-ttu-id="b6446-190">Fil: kopiera</span><span class="sxs-lookup"><span data-stu-id="b6446-190">File: Copy</span></span>
### <a name="copy-across-file-shares"></a><span data-ttu-id="b6446-191">Kopiera på filresurser</span><span class="sxs-lookup"><span data-stu-id="b6446-191">Copy across file shares</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="b6446-192">När du kopierar en fil på filresurser, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="b6446-192">When you copy a file across file shares, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-file-share-tooblob"></a><span data-ttu-id="b6446-193">Kopiera från filen resursen tooblob</span><span class="sxs-lookup"><span data-stu-id="b6446-193">Copy from file share tooblob</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="b6446-194">När du kopierar en fil från filen resursen tooblob en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="b6446-194">When you copy a file from file share tooblob, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>


### <a name="copy-from-blob-toofile-share"></a><span data-ttu-id="b6446-195">Kopiera från blob toofile resurs</span><span class="sxs-lookup"><span data-stu-id="b6446-195">Copy from blob toofile share</span></span>

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="b6446-196">När du kopierar en fil från blob toofile resurs, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="b6446-196">When you copy a file from blob toofile share, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="synchronously-copy-files"></a><span data-ttu-id="b6446-197">Synkront kopiera filer</span><span class="sxs-lookup"><span data-stu-id="b6446-197">Synchronously copy files</span></span>
<span data-ttu-id="b6446-198">Du kan ange hello `/SyncCopy` alternativet toocopy data från fillagring tooFile lagring, från fillagring tooBlob lagring och från Blobblagring tooFile lagring synkront, AzCopy gör detta genom att hämta hello källa data toolocal minne och ladda upp den toodestination igen.</span><span class="sxs-lookup"><span data-stu-id="b6446-198">You can specify hello `/SyncCopy` option toocopy data from File Storage tooFile Storage, from File Storage tooBlob Storage and from Blob Storage tooFile Storage synchronously, AzCopy does this by downloading hello source data toolocal memory and upload it again toodestination.</span></span> <span data-ttu-id="b6446-199">Standard utgång kostnaden gäller.</span><span class="sxs-lookup"><span data-stu-id="b6446-199">Standard egress cost will apply.</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

<span data-ttu-id="b6446-200">När du kopierar från fillagring tooBlob lagring hello standard blob-datatyp är blockblob, kan användaren ange alternativet `/BlobType:page` toochange hello blob måltypen.</span><span class="sxs-lookup"><span data-stu-id="b6446-200">When copying from File Storage tooBlob Storage, hello default blob type is block blob, user can specify option `/BlobType:page` toochange hello destination blob type.</span></span>

<span data-ttu-id="b6446-201">Observera att `/SyncCopy` kan skapa ytterligare utgång kostnad jämför tooasynchronous kopia, hello rekommenderade metoden är det här alternativet i hello Azure VM som är i hello toouse samma region som din datakälla konto tooavoid utgång lagringskostnaden.</span><span class="sxs-lookup"><span data-stu-id="b6446-201">Note that `/SyncCopy` might generate additional egress cost comparing tooasynchronous copy, hello recommended approach is toouse this option in hello Azure VM which is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="table-export"></a><span data-ttu-id="b6446-202">Tabell: exportera</span><span class="sxs-lookup"><span data-stu-id="b6446-202">Table: Export</span></span>
### <a name="export-table"></a><span data-ttu-id="b6446-203">Exportera tabell</span><span class="sxs-lookup"><span data-stu-id="b6446-203">Export table</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

<span data-ttu-id="b6446-204">AzCopy skriver en manifestfil toohello angivna målmapp.</span><span class="sxs-lookup"><span data-stu-id="b6446-204">AzCopy writes a manifest file toohello specified destination folder.</span></span> <span data-ttu-id="b6446-205">hello manifestfilen används i hello importera processen toolocate hello nödvändiga filer och utföra dataverifiering.</span><span class="sxs-lookup"><span data-stu-id="b6446-205">hello manifest file is used in hello import process toolocate hello necessary data files and perform data validation.</span></span> <span data-ttu-id="b6446-206">hello manifestfilen använder hello följa en namngivningskonvention som standard:</span><span class="sxs-lookup"><span data-stu-id="b6446-206">hello manifest file uses hello following naming convention by default:</span></span>

    <account name>_<table name>_<timestamp>.manifest

<span data-ttu-id="b6446-207">Användaren kan även ange hello alternativet `/Manifest:<manifest file name>` tooset hello Manifestfilens filnamn.</span><span class="sxs-lookup"><span data-stu-id="b6446-207">User can also specify hello option `/Manifest:<manifest file name>` tooset hello manifest file name.</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-export-into-multiple-files"></a><span data-ttu-id="b6446-208">Dela export i flera filer</span><span class="sxs-lookup"><span data-stu-id="b6446-208">Split export into multiple files</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

<span data-ttu-id="b6446-209">AzCopy använder en *volymindex* i hello dela data filnamnen toodistinguish flera filer.</span><span class="sxs-lookup"><span data-stu-id="b6446-209">AzCopy uses a *volume index* in hello split data file names toodistinguish multiple files.</span></span> <span data-ttu-id="b6446-210">hello volymindex består av två delar, en *partition viktiga områdesindex* och en *dela filen index*.</span><span class="sxs-lookup"><span data-stu-id="b6446-210">hello volume index consists of two parts, a *partition key range index* and a *split file index*.</span></span> <span data-ttu-id="b6446-211">Både index är nollbaserade.</span><span class="sxs-lookup"><span data-stu-id="b6446-211">Both indexes are zero-based.</span></span>

<span data-ttu-id="b6446-212">hello Partitionsindex viktiga intervallet är 0 om användaren inte anger alternativet `/PKRS`.</span><span class="sxs-lookup"><span data-stu-id="b6446-212">hello partition key range index will be 0 if user does not specify option `/PKRS`.</span></span>

<span data-ttu-id="b6446-213">Anta exempelvis AzCopy genererar två datafiler när hello användaren anger alternativet `/SplitSize`.</span><span class="sxs-lookup"><span data-stu-id="b6446-213">For instance, suppose AzCopy generates two data files after hello user specifies option `/SplitSize`.</span></span> <span data-ttu-id="b6446-214">hello resulterande filnamn data kan vara:</span><span class="sxs-lookup"><span data-stu-id="b6446-214">hello resulting data file names might be:</span></span>

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

<span data-ttu-id="b6446-215">Observera att hello minsta möjliga värde för alternativet `/SplitSize` 32 MB.</span><span class="sxs-lookup"><span data-stu-id="b6446-215">Note that hello minimum possible value for option `/SplitSize` is 32MB.</span></span> <span data-ttu-id="b6446-216">Om hello anges målet är Blob storage AzCopy ska dela hello datafil när dess storlek når hello blob storleksbegränsning (200GB), oavsett om alternativet `/SplitSize` har angetts av hello användare.</span><span class="sxs-lookup"><span data-stu-id="b6446-216">If hello specified destination is Blob storage, AzCopy will split hello data file once its sizes reaches hello blob size limitation (200GB), regardless of whether option `/SplitSize` has been specified by hello user.</span></span>

### <a name="export-table-toojson-or-csv-data-file-format"></a><span data-ttu-id="b6446-217">Exportera tabell tooJSON eller data för filformatet CSV</span><span class="sxs-lookup"><span data-stu-id="b6446-217">Export table tooJSON or CSV data file format</span></span>
<span data-ttu-id="b6446-218">AzCopy som standard exporterar tabeller tooJSON datafiler.</span><span class="sxs-lookup"><span data-stu-id="b6446-218">AzCopy by default exports tables tooJSON data files.</span></span> <span data-ttu-id="b6446-219">Du kan ange hello alternativet `/PayloadFormat:JSON|CSV` tooexport hello tabeller som JSON- eller CSV.</span><span class="sxs-lookup"><span data-stu-id="b6446-219">You can specify hello option `/PayloadFormat:JSON|CSV` tooexport hello tables as JSON or CSV.</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

<span data-ttu-id="b6446-220">När du anger nyttolast hello CSV-format, AzCopy också genererar en schemafil med filnamnstillägget `.schema.csv` för varje datafil.</span><span class="sxs-lookup"><span data-stu-id="b6446-220">When specifying hello CSV payload format, AzCopy will also generate a schema file with file extension `.schema.csv` for each data file.</span></span>

### <a name="export-table-entities-concurrently"></a><span data-ttu-id="b6446-221">Exportera tabellentiteter samtidigt</span><span class="sxs-lookup"><span data-stu-id="b6446-221">Export table entities concurrently</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

<span data-ttu-id="b6446-222">AzCopy startar samtidiga åtgärder tooexport entiteter hello användaren anger alternativet `/PKRS`.</span><span class="sxs-lookup"><span data-stu-id="b6446-222">AzCopy will start concurrent operations tooexport entities when hello user specifies option `/PKRS`.</span></span> <span data-ttu-id="b6446-223">Varje åtgärd exporterar en partitionsnyckelintervallet.</span><span class="sxs-lookup"><span data-stu-id="b6446-223">Each operation exports one partition key range.</span></span>

<span data-ttu-id="b6446-224">Observera att hello antalet samtidiga åtgärder också styrs av alternativet `/NC`.</span><span class="sxs-lookup"><span data-stu-id="b6446-224">Note that hello number of concurrent operations is also controlled by option `/NC`.</span></span> <span data-ttu-id="b6446-225">AzCopy använder hello antalet kärnprocessorer som hello standardvärdet `/NC` när du kopierar tabellen enheter, även om `/NC` har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="b6446-225">AzCopy uses hello number of core processors as hello default value of `/NC` when copying table entities, even if `/NC` was not specified.</span></span> <span data-ttu-id="b6446-226">När hello användaren anger alternativet `/PKRS`, AzCopy använder hello mindre hello två värden - partition nyckelintervall jämfört med implicit eller explicit angivna samtidiga åtgärder - toodetermine hello antalet samtidiga åtgärder toostart.</span><span class="sxs-lookup"><span data-stu-id="b6446-226">When hello user specifies option `/PKRS`, AzCopy uses hello smaller of hello two values - partition key ranges versus implicitly or explicitly specified concurrent operations - toodetermine hello number of concurrent operations toostart.</span></span> <span data-ttu-id="b6446-227">Mer information, Skriv `AzCopy /?:NC` hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="b6446-227">For more details, type `AzCopy /?:NC` at hello command line.</span></span>

### <a name="export-table-tooblob"></a><span data-ttu-id="b6446-228">Exportera tabellen tooblob</span><span class="sxs-lookup"><span data-stu-id="b6446-228">Export table tooblob</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

<span data-ttu-id="b6446-229">AzCopy skapar en JSON-datafil till hello blob-behållare med följande namngivningskonvention:</span><span class="sxs-lookup"><span data-stu-id="b6446-229">AzCopy will generate a JSON data file into hello blob container with following naming convention:</span></span>

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

<span data-ttu-id="b6446-230">hello genereras JSON-datafilen följer hello nyttolastformat för minimal metadata.</span><span class="sxs-lookup"><span data-stu-id="b6446-230">hello generated JSON data file follows hello payload format for minimal metadata.</span></span> <span data-ttu-id="b6446-231">Mer information om den här nyttolastformatet finns [Nyttolastformat för tabellen tjänståtgärder](http://msdn.microsoft.com/library/azure/dn535600.aspx).</span><span class="sxs-lookup"><span data-stu-id="b6446-231">For details on this payload format, see [Payload Format for Table Service Operations](http://msdn.microsoft.com/library/azure/dn535600.aspx).</span></span>

<span data-ttu-id="b6446-232">Observera att när du exporterar tabeller tooblobs kommer AzCopy hämta hello tabell entiteter toolocal temporära filer och sedan ladda upp de entiteter toohello blobben.</span><span class="sxs-lookup"><span data-stu-id="b6446-232">Note that when exporting tables tooblobs, AzCopy will download hello Table entities toolocal temporary data files and then upload those entities toohello blob.</span></span> <span data-ttu-id="b6446-233">Dessa temporära datafiler placeras i hello journalmapp med hello standardsökvägen ”<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>”, du kan ange alternativet/Z: [journal-filmapp] toochange hello journal sökvägen för mappen och därmed ändra hello plats för temporära filer.</span><span class="sxs-lookup"><span data-stu-id="b6446-233">These temporary data files are put into hello journal file folder with hello default path "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", you can specify option /Z:[journal-file-folder] toochange hello journal file folder location and thus change hello temporary data files location.</span></span> <span data-ttu-id="b6446-234">hello tillfälliga data filernas storlek bestäms av din tabellentiteter och hello storleken som du har angett med hello alternativet /SplitSize även om hello temporär fil i lokal disk tas bort omedelbart när det har laddats upp toohello blob, kontrollera att du ha tillräckligt med lokal disk space toostore filerna tillfälliga data innan de tas bort.</span><span class="sxs-lookup"><span data-stu-id="b6446-234">hello temporary data files' size is decided by your table entities' size and hello size you specified with hello option /SplitSize, although hello temporary data file in local disk will be deleted instantly once it has been uploaded toohello blob, please make sure you have enough local disk space toostore these temporary data files before they are deleted.</span></span>

## <a name="table-import"></a><span data-ttu-id="b6446-235">Tabell: Import</span><span class="sxs-lookup"><span data-stu-id="b6446-235">Table: Import</span></span>
### <a name="import-table"></a><span data-ttu-id="b6446-236">Import av tabell</span><span class="sxs-lookup"><span data-stu-id="b6446-236">Import table</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

<span data-ttu-id="b6446-237">Hej alternativet `/EntityOperation` anger hur hello tooinsert entiteter i tabellen.</span><span class="sxs-lookup"><span data-stu-id="b6446-237">hello option `/EntityOperation` indicates how tooinsert entities into hello table.</span></span> <span data-ttu-id="b6446-238">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="b6446-238">Possible values are:</span></span>

* <span data-ttu-id="b6446-239">`InsertOrSkip`: Hoppar över en befintlig entitet eller infogar en ny entitet om den inte finns i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="b6446-239">`InsertOrSkip`: Skips an existing entity or inserts a new entity if it does not exist in hello table.</span></span>
* <span data-ttu-id="b6446-240">`InsertOrMerge`: Sammanfogar en befintlig entitet eller infogar en ny entitet om den inte finns i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="b6446-240">`InsertOrMerge`: Merges an existing entity or inserts a new entity if it does not exist in hello table.</span></span>
* <span data-ttu-id="b6446-241">`InsertOrReplace`: Ersätter en befintlig entitet eller infogar en ny entitet om den inte finns i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="b6446-241">`InsertOrReplace`: Replaces an existing entity or inserts a new entity if it does not exist in hello table.</span></span>

<span data-ttu-id="b6446-242">Observera att du inte kan ange alternativet `/PKRS` i hello importera scenario.</span><span class="sxs-lookup"><span data-stu-id="b6446-242">Note that you cannot specify option `/PKRS` in hello import scenario.</span></span> <span data-ttu-id="b6446-243">Till skillnad från hello export scenario där du måste ange alternativet `/PKRS` toostart samtidiga åtgärder, AzCopy som standard startar samtidiga åtgärder när du importerar en tabell.</span><span class="sxs-lookup"><span data-stu-id="b6446-243">Unlike hello export scenario, in which you must specify option `/PKRS` toostart concurrent operations, AzCopy will by default start concurrent operations when you import a table.</span></span> <span data-ttu-id="b6446-244">hello standardantalet samtidiga åtgärder igång är lika toohello antalet kärnprocessorer; Du kan dock ange ett annat antal samtidiga med alternativet `/NC`.</span><span class="sxs-lookup"><span data-stu-id="b6446-244">hello default number of concurrent operations started is equal toohello number of core processors; however, you can specify a different number of concurrent with option `/NC`.</span></span> <span data-ttu-id="b6446-245">Mer information, Skriv `AzCopy /?:NC` hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="b6446-245">For more details, type `AzCopy /?:NC` at hello command line.</span></span>

<span data-ttu-id="b6446-246">Observera att AzCopy endast stöder import för JSON inte CSV.</span><span class="sxs-lookup"><span data-stu-id="b6446-246">Note that AzCopy only supports importing for JSON, not CSV.</span></span> <span data-ttu-id="b6446-247">AzCopy har inte stöd för import av tabell från användarskapade JSON och manifest filer.</span><span class="sxs-lookup"><span data-stu-id="b6446-247">AzCopy does not support table imports from user-created JSON and manifest files.</span></span> <span data-ttu-id="b6446-248">Båda dessa filer måste komma från en tabell AzCopy-export.</span><span class="sxs-lookup"><span data-stu-id="b6446-248">Both of these files must come from an AzCopy table export.</span></span> <span data-ttu-id="b6446-249">tooavoid fel, ändra inte hello exporteras JSON eller manifestfilen.</span><span class="sxs-lookup"><span data-stu-id="b6446-249">tooavoid errors, please do not modify hello exported JSON or manifest file.</span></span>

### <a name="import-entities-tootable-using-blobs"></a><span data-ttu-id="b6446-250">Importera entiteter tootable blobbar</span><span class="sxs-lookup"><span data-stu-id="b6446-250">Import entities tootable using blobs</span></span>
<span data-ttu-id="b6446-251">Anta en Blob-behållare innehåller hello följande: en JSON-fil som representerar en Azure-tabellen och dess tillhörande manifestfilen.</span><span class="sxs-lookup"><span data-stu-id="b6446-251">Assume a Blob container contains hello following: A JSON file representing an Azure Table and its accompanying manifest file.</span></span>

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

<span data-ttu-id="b6446-252">Du kan köra hello efter kommandot tooimport enheter till en tabell med hello manifestfilen i den blob-behållaren:</span><span class="sxs-lookup"><span data-stu-id="b6446-252">You can run hello following command tooimport entities into a table using hello manifest file in that blob container:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a><span data-ttu-id="b6446-253">Andra AzCopy-funktioner</span><span class="sxs-lookup"><span data-stu-id="b6446-253">Other AzCopy features</span></span>
### <a name="only-copy-data-that-doesnt-exist-in-hello-destination"></a><span data-ttu-id="b6446-254">Endast kopiera data som inte finns i hello mål</span><span class="sxs-lookup"><span data-stu-id="b6446-254">Only copy data that doesn't exist in hello destination</span></span>
<span data-ttu-id="b6446-255">Hej `/XO` och `/XN` parametrar kan du tooexclude äldre eller nyare resurser från att kopieras respektive.</span><span class="sxs-lookup"><span data-stu-id="b6446-255">hello `/XO` and `/XN` parameters allow you tooexclude older or newer source resources from being copied, respectively.</span></span> <span data-ttu-id="b6446-256">Om du bara vill toocopy källa resurser som inte finns i hello målet kan du ange båda parametrarna i hello AzCopy-kommandot:</span><span class="sxs-lookup"><span data-stu-id="b6446-256">If you only want toocopy source resources that don't exist in hello destination, you can specify both parameters in hello AzCopy command:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

<span data-ttu-id="b6446-257">Observera att detta inte stöds när hello källan eller målet är en tabell.</span><span class="sxs-lookup"><span data-stu-id="b6446-257">Note that this is not supported when either hello source or destination is a table.</span></span>

### <a name="use-a-response-file-toospecify-command-line-parameters"></a><span data-ttu-id="b6446-258">Använd en toospecify kommandoradsverktyget svarsfilparametrar</span><span class="sxs-lookup"><span data-stu-id="b6446-258">Use a response file toospecify command-line parameters</span></span>

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

<span data-ttu-id="b6446-259">Du kan inkludera eventuella kommandoradsparametrar för AzCopy i en svarsfil.</span><span class="sxs-lookup"><span data-stu-id="b6446-259">You can include any AzCopy command-line parameters in a response file.</span></span> <span data-ttu-id="b6446-260">AzCopy processer hello parametrar i hello-filen som om de hade angetts på kommandoraden för hello utför en direkt ersättning med hello innehållet i hello-fil.</span><span class="sxs-lookup"><span data-stu-id="b6446-260">AzCopy processes hello parameters in hello file as if they had been specified on hello command line, performing a direct substitution with hello contents of hello file.</span></span>

<span data-ttu-id="b6446-261">Anta en svarsfil med namnet `copyoperation.txt`, som innehåller hello följande rader.</span><span class="sxs-lookup"><span data-stu-id="b6446-261">Assume a response file named `copyoperation.txt`, that contains hello following lines.</span></span> <span data-ttu-id="b6446-262">Varje AzCopy-parameter kan anges på en enda rad</span><span class="sxs-lookup"><span data-stu-id="b6446-262">Each AzCopy parameter can be specified on a single line</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

<span data-ttu-id="b6446-263">eller på separata rader:</span><span class="sxs-lookup"><span data-stu-id="b6446-263">or on separate lines:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

<span data-ttu-id="b6446-264">AzCopy misslyckas om du vill dela hello parametern på två rader som visas här för hello `/sourcekey` parameter:</span><span class="sxs-lookup"><span data-stu-id="b6446-264">AzCopy will fail if you split hello parameter across two lines, as shown here for hello `/sourcekey` parameter:</span></span>

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-toospecify-command-line-parameters"></a><span data-ttu-id="b6446-265">Använda flera svar filer toospecify-kommandoradsparametrar</span><span class="sxs-lookup"><span data-stu-id="b6446-265">Use multiple response files toospecify command-line parameters</span></span>
<span data-ttu-id="b6446-266">Anta en svarsfil med namnet `source.txt` som anger en käll-behållare:</span><span class="sxs-lookup"><span data-stu-id="b6446-266">Assume a response file named `source.txt` that specifies a source container:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer

<span data-ttu-id="b6446-267">Och en svarsfil med namnet `dest.txt` som anger en målmapp i hello file system:</span><span class="sxs-lookup"><span data-stu-id="b6446-267">And a response file named `dest.txt` that specifies a destination folder in hello file system:</span></span>

    /Dest:C:\myfolder

<span data-ttu-id="b6446-268">Och en svarsfil med namnet `options.txt` som anger alternativ för AzCopy:</span><span class="sxs-lookup"><span data-stu-id="b6446-268">And a response file named `options.txt` that specifies options for AzCopy:</span></span>

    /S /Y

<span data-ttu-id="b6446-269">toocall AzCopy med dessa svarsfiler som finns i en katalog `C:\responsefiles`, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b6446-269">toocall AzCopy with these response files, all of which reside in a directory `C:\responsefiles`, use this command:</span></span>

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

<span data-ttu-id="b6446-270">AzCopy bearbetar det här kommandot precis som om du har lagt till alla hello enskilda parametrar på kommandoraden för hello:</span><span class="sxs-lookup"><span data-stu-id="b6446-270">AzCopy processes this command just as it would if you included all of hello individual parameters on hello command line:</span></span>

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a><span data-ttu-id="b6446-271">Ange en signatur för delad åtkomst (SAS)</span><span class="sxs-lookup"><span data-stu-id="b6446-271">Specify a shared access signature (SAS)</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

<span data-ttu-id="b6446-272">Du kan också ange en SAS för hello behållaren URI:</span><span class="sxs-lookup"><span data-stu-id="b6446-272">You can also specify a SAS on hello container URI:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a><span data-ttu-id="b6446-273">Ändringsjournalen mapp</span><span class="sxs-lookup"><span data-stu-id="b6446-273">Journal file folder</span></span>
<span data-ttu-id="b6446-274">Varje gång du utfärda ett kommando tooAzCopy kontrollerar den om en journal-fil finns i hello standardmappen eller om den finns i en mapp som du har angett via det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="b6446-274">Each time you issue a command tooAzCopy, it checks whether a journal file exists in hello default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="b6446-275">Om hello journalfilen inte finns på någon plats, behandlar hello åtgärden som nya AzCopy och genererar en ny journalfil.</span><span class="sxs-lookup"><span data-stu-id="b6446-275">If hello journal file does not exist in either place, AzCopy treats hello operation as new and generates a new journal file.</span></span>

<span data-ttu-id="b6446-276">Om hello journalfilen finns kontrollerar AzCopy om hello-kommandoraden som du matar in matchar hello kommandoraden i hello journal-fil.</span><span class="sxs-lookup"><span data-stu-id="b6446-276">If hello journal file does exist, AzCopy will check whether hello command line that you input matches hello command line in hello journal file.</span></span> <span data-ttu-id="b6446-277">Om hello två kommandorader matchar återupptar AzCopy hello ofullständiga igen.</span><span class="sxs-lookup"><span data-stu-id="b6446-277">If hello two command lines match, AzCopy resumes hello incomplete operation.</span></span> <span data-ttu-id="b6446-278">Du kommer att tillfrågas tooeither överskrivning hello journal toostart en ny åtgärd eller toocancel hello aktuella filåtgärd om de inte matchar.</span><span class="sxs-lookup"><span data-stu-id="b6446-278">If they do not match, you will be prompted tooeither overwrite hello journal file toostart a new operation, or toocancel hello current operation.</span></span>

<span data-ttu-id="b6446-279">Om du vill toouse hello standardplatsen för hello journal-fil:</span><span class="sxs-lookup"><span data-stu-id="b6446-279">If you want toouse hello default location for hello journal file:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

<span data-ttu-id="b6446-280">Om du utelämnar alternativet `/Z`, eller ange alternativet `/Z` utan hello mappsökvägen, enligt ovan, AzCopy skapar hello journal-fil i hello standardplatsen, som är `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="b6446-280">If you omit option `/Z`, or specify option `/Z` without hello folder path, as shown above, AzCopy creates hello journal file in hello default location, which is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="b6446-281">Om hello journalfilen redan finns återupptas AzCopy hello-åtgärden utifrån hello journal-fil.</span><span class="sxs-lookup"><span data-stu-id="b6446-281">If hello journal file already exists, then AzCopy resumes hello operation based on hello journal file.</span></span>

<span data-ttu-id="b6446-282">Om du vill toospecify en egen placering för hello journal-fil:</span><span class="sxs-lookup"><span data-stu-id="b6446-282">If you want toospecify a custom location for hello journal file:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

<span data-ttu-id="b6446-283">Det här exemplet skapar hello journal-filen om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="b6446-283">This example creates hello journal file if it does not already exist.</span></span> <span data-ttu-id="b6446-284">Om den finns, återupptar AzCopy hello-åtgärden utifrån hello journal-fil.</span><span class="sxs-lookup"><span data-stu-id="b6446-284">If it does exist, then AzCopy resumes hello operation based on hello journal file.</span></span>

<span data-ttu-id="b6446-285">Om du vill tooresume en AzCopy-åtgärd:</span><span class="sxs-lookup"><span data-stu-id="b6446-285">If you want tooresume an AzCopy operation:</span></span>

```azcopy
AzCopy /Z:C:\journalfolder\
```

<span data-ttu-id="b6446-286">Det här exemplet återupptar hello senaste åtgärd, som kan ha misslyckats toocomplete.</span><span class="sxs-lookup"><span data-stu-id="b6446-286">This example resumes hello last operation, which may have failed toocomplete.</span></span>

### <a name="generate-a-log-file"></a><span data-ttu-id="b6446-287">Generera en loggfil</span><span class="sxs-lookup"><span data-stu-id="b6446-287">Generate a log file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

<span data-ttu-id="b6446-288">Om du anger alternativet `/V` utan att ange en sökväg toohello utförlig filloggen sedan AzCopy hello loggfil skapas i hello standardplatsen, som är `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="b6446-288">If you specify option `/V` without providing a file path toohello verbose log, then AzCopy creates hello log file in hello default location, which is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span></span>

<span data-ttu-id="b6446-289">Annars kan du skapa en loggfil på en annan plats:</span><span class="sxs-lookup"><span data-stu-id="b6446-289">Otherwise, you can create an log file in a custom location:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

<span data-ttu-id="b6446-290">Observera att om du anger en relativ sökväg efter alternativet `/V`, som `/V:test/azcopy1.log`, och sedan hello utförlig loggen skapas i hello aktuella arbetskatalogen i en undermapp som heter `test`.</span><span class="sxs-lookup"><span data-stu-id="b6446-290">Note that if you specify a relative path following option `/V`, such as `/V:test/azcopy1.log`, then hello verbose log is created in hello current working directory within a subfolder named `test`.</span></span>

### <a name="specify-hello-number-of-concurrent-operations-toostart"></a><span data-ttu-id="b6446-291">Ange hello antalet samtidiga åtgärder toostart</span><span class="sxs-lookup"><span data-stu-id="b6446-291">Specify hello number of concurrent operations toostart</span></span>
<span data-ttu-id="b6446-292">Alternativet `/NC` anger hello antalet samtidiga kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="b6446-292">Option `/NC` specifies hello number of concurrent copy operations.</span></span> <span data-ttu-id="b6446-293">Som standard startar AzCopy antalet samtidiga åtgärder tooincrease hello data transfer genomflöde.</span><span class="sxs-lookup"><span data-stu-id="b6446-293">By default, AzCopy starts a certain number of concurrent operations tooincrease hello data transfer throughput.</span></span> <span data-ttu-id="b6446-294">För tabellen är hello antalet samtidiga åtgärder lika toohello antal processorer som du har.</span><span class="sxs-lookup"><span data-stu-id="b6446-294">For Table operations, hello number of concurrent operations is equal toohello number of processors you have.</span></span> <span data-ttu-id="b6446-295">För Blob- och fil-åtgärder, hello antalet samtidiga åtgärder är lika med 8 gånger hello antal processorer som du har.</span><span class="sxs-lookup"><span data-stu-id="b6446-295">For Blob and File operations, hello number of concurrent operations is equal 8 times hello number of processors you have.</span></span> <span data-ttu-id="b6446-296">Om du använder AzCopy över ett nätverk med låg bandbredd, kan du ange ett lägre värde /NC tooavoid misslyckades på grund av konkurrens om resurser.</span><span class="sxs-lookup"><span data-stu-id="b6446-296">If you are running AzCopy across a low-bandwidth network, you can specify a lower number for /NC tooavoid failure caused by resource competition.</span></span>

### <a name="run-azcopy-against-azure-storage-emulator"></a><span data-ttu-id="b6446-297">Köra AzCopy mot Azure Storage-emulatorn</span><span class="sxs-lookup"><span data-stu-id="b6446-297">Run AzCopy against Azure Storage Emulator</span></span>
<span data-ttu-id="b6446-298">Du kan köra AzCopy mot hello [Azure Storage-emulatorn](storage-use-emulator.md) för BLOB:</span><span class="sxs-lookup"><span data-stu-id="b6446-298">You can run AzCopy against hello [Azure Storage Emulator](storage-use-emulator.md) for Blobs:</span></span>

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

<span data-ttu-id="b6446-299">och tabeller:</span><span class="sxs-lookup"><span data-stu-id="b6446-299">and Tables:</span></span>

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

## <a name="azcopy-parameters"></a><span data-ttu-id="b6446-300">AzCopy-parametrar</span><span class="sxs-lookup"><span data-stu-id="b6446-300">AzCopy Parameters</span></span>
<span data-ttu-id="b6446-301">Parametrar för AzCopy beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="b6446-301">Parameters for AzCopy are described below.</span></span> <span data-ttu-id="b6446-302">Du kan också ange ett av följande kommandon från hello kommandoraden för att få hjälp med AzCopy hello:</span><span class="sxs-lookup"><span data-stu-id="b6446-302">You can also type one of hello following commands from hello command line for help in using AzCopy:</span></span>

* <span data-ttu-id="b6446-303">För detaljerad hjälp för AzCopy:`AzCopy /?`</span><span class="sxs-lookup"><span data-stu-id="b6446-303">For detailed command-line help for AzCopy: `AzCopy /?`</span></span>
* <span data-ttu-id="b6446-304">För detaljerad hjälp om någon AzCopy-parameter:`AzCopy /?:SourceKey`</span><span class="sxs-lookup"><span data-stu-id="b6446-304">For detailed help with any AzCopy parameter: `AzCopy /?:SourceKey`</span></span>
* <span data-ttu-id="b6446-305">Kommandoradsverktyget exempel:`AzCopy /?:Samples`</span><span class="sxs-lookup"><span data-stu-id="b6446-305">For command-line examples: `AzCopy /?:Samples`</span></span>

### <a name="sourcesource"></a><span data-ttu-id="b6446-306">/ Source: ”källa”</span><span class="sxs-lookup"><span data-stu-id="b6446-306">/Source:"source"</span></span>
<span data-ttu-id="b6446-307">Anger hello källdata från vilken toocopy.</span><span class="sxs-lookup"><span data-stu-id="b6446-307">Specifies hello source data from which toocopy.</span></span> <span data-ttu-id="b6446-308">hello källan kan vara en katalog i filsystemet, en blob-behållare, en virtuell katalog för blob, en lagringsfilresurs, en katalog i lagring eller en Azure-tabellen.</span><span class="sxs-lookup"><span data-stu-id="b6446-308">hello source can be a file system directory, a blob container, a blob virtual directory, a storage file share, a storage file directory, or an Azure table.</span></span>

<span data-ttu-id="b6446-309">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-309">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="destdestination"></a><span data-ttu-id="b6446-310">/ Dest: ”mål”</span><span class="sxs-lookup"><span data-stu-id="b6446-310">/Dest:"destination"</span></span>
<span data-ttu-id="b6446-311">Anger hello mål toocopy till.</span><span class="sxs-lookup"><span data-stu-id="b6446-311">Specifies hello destination toocopy to.</span></span> <span data-ttu-id="b6446-312">hello målet kan vara en katalog i filsystemet, en blob-behållare, en virtuell katalog för blob, en lagringsfilresurs, en katalog i lagring eller en Azure-tabellen.</span><span class="sxs-lookup"><span data-stu-id="b6446-312">hello destination can be a file system directory, a blob container, a blob virtual directory, a storage file share, a storage file directory, or an Azure table.</span></span>

<span data-ttu-id="b6446-313">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-313">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="patternfile-pattern"></a><span data-ttu-id="b6446-314">/ Mönstret: ”filmönstret”</span><span class="sxs-lookup"><span data-stu-id="b6446-314">/Pattern:"file-pattern"</span></span>
<span data-ttu-id="b6446-315">Anger ett fil-mönster som anger vilka filer toocopy.</span><span class="sxs-lookup"><span data-stu-id="b6446-315">Specifies a file pattern that indicates which files toocopy.</span></span> <span data-ttu-id="b6446-316">hello funktionssätt hello /Pattern parametern bestäms av hello plats hello källdata och hello förekomst av hello rekursiv läge.</span><span class="sxs-lookup"><span data-stu-id="b6446-316">hello behavior of hello /Pattern parameter is determined by hello location of hello source data, and hello presence of hello recursive mode option.</span></span> <span data-ttu-id="b6446-317">Rekursiva läge har angetts via alternativet/s.</span><span class="sxs-lookup"><span data-stu-id="b6446-317">Recursive mode is specified via option /S.</span></span>

<span data-ttu-id="b6446-318">Om hello angiven källa är en katalog i hello filsystemet standard jokertecken är aktiva och hello filen mönster som matchas mot filer i hello katalog.</span><span class="sxs-lookup"><span data-stu-id="b6446-318">If hello specified source is a directory in hello file system, then standard wildcards are in effect, and hello file pattern provided is matched against files within hello directory.</span></span> <span data-ttu-id="b6446-319">Om alternativet /S anges sedan AzCopy också matchar hello angivna mönstret mot alla filer i alla undermappar under hello directory.</span><span class="sxs-lookup"><span data-stu-id="b6446-319">If option /S is specified, then AzCopy also matches hello specified pattern against all files in any subfolders beneath hello directory.</span></span>

<span data-ttu-id="b6446-320">Om hello angiven källa är en blob-behållare eller virtuell katalog, tillämpas inte jokertecken.</span><span class="sxs-lookup"><span data-stu-id="b6446-320">If hello specified source is a blob container or virtual directory, then wildcards are not applied.</span></span> <span data-ttu-id="b6446-321">Om alternativet /S anges sedan AzCopy tolkar hello angivna filen mönster som ett blob-prefix.</span><span class="sxs-lookup"><span data-stu-id="b6446-321">If option /S is specified, then AzCopy interprets hello specified file pattern as a blob prefix.</span></span> <span data-ttu-id="b6446-322">Om alternativet inte anges /S sedan AzCopy matchar hello filmönstret mot exakt blob-namn.</span><span class="sxs-lookup"><span data-stu-id="b6446-322">If option /S is not specified, then AzCopy matches hello file pattern against exact blob names.</span></span>

<span data-ttu-id="b6446-323">Om hello anges källan är en Azure-filresurs, måste du antingen ange hello exakt namnet på filen (t.ex. abc.txt) toocopy en enskild fil, eller ange alternativet /S toocopy alla filer i hello resursen rekursivt.</span><span class="sxs-lookup"><span data-stu-id="b6446-323">If hello specified source is an Azure file share, then you must either specify hello exact file name, (e.g. abc.txt) toocopy a single file, or specify option /S toocopy all files in hello share recursively.</span></span> <span data-ttu-id="b6446-324">Försök toospecify både filmönstret och alternativet resulterar /S tillsammans i ett fel.</span><span class="sxs-lookup"><span data-stu-id="b6446-324">Attempting toospecify both a file pattern and option /S together will result in an error.</span></span>

<span data-ttu-id="b6446-325">AzCopy använder skiftlägeskänsliga matchar när hello/Source är en blob-behållare eller blob virtuell katalog och använder skiftlägeskänsliga matchar alla hello andra fall.</span><span class="sxs-lookup"><span data-stu-id="b6446-325">AzCopy uses case-sensitive matching when hello /Source is a blob container or blob virtual directory, and uses case-insensitive matching in all hello other cases.</span></span>

<span data-ttu-id="b6446-326">hello filen Standardmönster som används när inga filmönstret anges är *.*</span><span class="sxs-lookup"><span data-stu-id="b6446-326">hello default file pattern used when no file pattern is specified is *.*</span></span> <span data-ttu-id="b6446-327">för en plats för system eller ett tomt prefix för en Azure-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="b6446-327">for a file system location or an empty prefix for an Azure Storage location.</span></span> <span data-ttu-id="b6446-328">Ange flera filen mönster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="b6446-328">Specifying multiple file patterns is not supported.</span></span>

<span data-ttu-id="b6446-329">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="b6446-329">**Applicable to:** Blobs, Files</span></span>

### <a name="destkeystorage-key"></a><span data-ttu-id="b6446-330">/ DestKey: ”lagringsnyckel”</span><span class="sxs-lookup"><span data-stu-id="b6446-330">/DestKey:"storage-key"</span></span>
<span data-ttu-id="b6446-331">Anger hello lagringskontonyckel för hello målresurs.</span><span class="sxs-lookup"><span data-stu-id="b6446-331">Specifies hello storage account key for hello destination resource.</span></span>

<span data-ttu-id="b6446-332">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-332">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="destsassas-token"></a><span data-ttu-id="b6446-333">/ DestSAS: ”sas-token”</span><span class="sxs-lookup"><span data-stu-id="b6446-333">/DestSAS:"sas-token"</span></span>
<span data-ttu-id="b6446-334">Anger en delad signatur åtkomst (SAS) med behörigheter för Läs- och skrivbehörighet för hello mål (om tillämpligt).</span><span class="sxs-lookup"><span data-stu-id="b6446-334">Specifies a Shared Access Signature (SAS) with READ and WRITE permissions for hello destination (if applicable).</span></span> <span data-ttu-id="b6446-335">Omge hello SAS med dubbla citattecken, eftersom det kanske innehåller kommandoradsverktyget specialtecken.</span><span class="sxs-lookup"><span data-stu-id="b6446-335">Surround hello SAS with double quotes, as it may contains special command-line characters.</span></span>

<span data-ttu-id="b6446-336">Du kan antingen ange det här alternativet följt av hello SAS-token om hello målresurs är ett blob-behållare, en filresurs eller en tabell, eller kan du ange hello SAS som en del av hello mål blob-behållare, filresurs eller tabellens URI, utan det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="b6446-336">If hello destination resource is a blob container, file share or table, you can either specify this option followed by hello SAS token, or you can specify hello SAS as part of hello destination blob container, file share or table's URI, without this option.</span></span>

<span data-ttu-id="b6446-337">Om hello källa och mål är båda blobbar och sedan hello mål blob måste finnas inom hello samma lagringskonto som hello källa blob.</span><span class="sxs-lookup"><span data-stu-id="b6446-337">If hello source and destination are both blobs, then hello destination blob must reside within hello same storage account as hello source blob.</span></span>

<span data-ttu-id="b6446-338">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-338">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcekeystorage-key"></a><span data-ttu-id="b6446-339">/ SourceKey: ”lagringsnyckel”</span><span class="sxs-lookup"><span data-stu-id="b6446-339">/SourceKey:"storage-key"</span></span>
<span data-ttu-id="b6446-340">Anger hello lagringskontonyckel för hello källa resurs.</span><span class="sxs-lookup"><span data-stu-id="b6446-340">Specifies hello storage account key for hello source resource.</span></span>

<span data-ttu-id="b6446-341">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-341">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcesassas-token"></a><span data-ttu-id="b6446-342">/ SourceSAS: ”sas-token”</span><span class="sxs-lookup"><span data-stu-id="b6446-342">/SourceSAS:"sas-token"</span></span>
<span data-ttu-id="b6446-343">Anger en signatur för delad åtkomst med läs- och behörigheter för hello källan (om tillämpligt).</span><span class="sxs-lookup"><span data-stu-id="b6446-343">Specifies a Shared Access Signature with READ and LIST permissions for hello source (if applicable).</span></span> <span data-ttu-id="b6446-344">Omge hello SAS med dubbla citattecken, eftersom det kanske innehåller kommandoradsverktyget specialtecken.</span><span class="sxs-lookup"><span data-stu-id="b6446-344">Surround hello SAS with double quotes, as it may contains special command-line characters.</span></span>

<span data-ttu-id="b6446-345">Om varken en nyckel eller en SAS tillhandahålls hello källa resursen är en blob-behållare, kommer hello blob-behållare att läsas via anonym åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b6446-345">If hello source resource is a blob container, and neither a key nor a SAS is provided, then hello blob container will be read via anonymous access.</span></span>

<span data-ttu-id="b6446-346">Om hello källan är en filresurs eller tabell, en nyckel eller en SAS måste anges.</span><span class="sxs-lookup"><span data-stu-id="b6446-346">If hello source is a file share or table, a key or a SAS must be provided.</span></span>

<span data-ttu-id="b6446-347">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-347">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="s"></a><span data-ttu-id="b6446-348">/ S</span><span class="sxs-lookup"><span data-stu-id="b6446-348">/S</span></span>
<span data-ttu-id="b6446-349">Anger rekursiv kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="b6446-349">Specifies recursive mode for copy operations.</span></span> <span data-ttu-id="b6446-350">AzCopy i rekursiva läge kommer att kopiera alla blobbar eller filer som matchar mönstret för hello angivna filen, inklusive de på undermappar.</span><span class="sxs-lookup"><span data-stu-id="b6446-350">In recursive mode, AzCopy will copy all blobs or files that match hello specified file pattern, including those in subfolders.</span></span>

<span data-ttu-id="b6446-351">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="b6446-351">**Applicable to:** Blobs, Files</span></span>

### <a name="blobtypeblock--page--append"></a><span data-ttu-id="b6446-352">/ BlobType: ”block” | ”sidan” | ”Lägg till”</span><span class="sxs-lookup"><span data-stu-id="b6446-352">/BlobType:"block" | "page" | "append"</span></span>
<span data-ttu-id="b6446-353">Anger om hello mål blob är en blockblobb, en sidblob eller en tilläggsblobb.</span><span class="sxs-lookup"><span data-stu-id="b6446-353">Specifies whether hello destination blob is a block blob, a page blob, or an append blob.</span></span> <span data-ttu-id="b6446-354">Det här alternativet gäller bara när du laddar upp en blob.</span><span class="sxs-lookup"><span data-stu-id="b6446-354">This option is applicable only when you are uploading a blob.</span></span> <span data-ttu-id="b6446-355">Annars genereras ett fel.</span><span class="sxs-lookup"><span data-stu-id="b6446-355">Otherwise, an error is generated.</span></span> <span data-ttu-id="b6446-356">Om hello målet är en blob och det här alternativet inte anges som standard skapas en blockblobb AzCopy.</span><span class="sxs-lookup"><span data-stu-id="b6446-356">If hello destination is a blob and this option is not specified, by default, AzCopy creates a block blob.</span></span>

<span data-ttu-id="b6446-357">**Gäller för:** Blobbar</span><span class="sxs-lookup"><span data-stu-id="b6446-357">**Applicable to:** Blobs</span></span>

### <a name="checkmd5"></a><span data-ttu-id="b6446-358">/ CheckMD5</span><span class="sxs-lookup"><span data-stu-id="b6446-358">/CheckMD5</span></span>
<span data-ttu-id="b6446-359">Beräknar en MD5-hash för hämtade data och verifierar att hello MD5-hash lagras i hello blob eller filens Content-MD5-egenskap stämmer med hello beräknade hash.</span><span class="sxs-lookup"><span data-stu-id="b6446-359">Calculates an MD5 hash for downloaded data and verifies that hello MD5 hash stored in hello blob or file's Content-MD5 property matches hello calculated hash.</span></span> <span data-ttu-id="b6446-360">hello MD5-kontroll är inaktiverad som standard, så du måste ange det här alternativet tooperform hello MD5 kontroll när du hämtar data.</span><span class="sxs-lookup"><span data-stu-id="b6446-360">hello MD5 check is turned off by default, so you must specify this option tooperform hello MD5 check when downloading data.</span></span>

<span data-ttu-id="b6446-361">Observera att Azure Storage inte garanterar att hello MD5-hash lagras för hello blob eller filen är uppdaterad.</span><span class="sxs-lookup"><span data-stu-id="b6446-361">Note that Azure Storage doesn't guarantee that hello MD5 hash stored for hello blob or file is up-to-date.</span></span> <span data-ttu-id="b6446-362">Det är klientens ansvar tooupdate hello MD5 när hello blob eller fil ändras.</span><span class="sxs-lookup"><span data-stu-id="b6446-362">It is client's responsibility tooupdate hello MD5 whenever hello blob or file is modified.</span></span>

<span data-ttu-id="b6446-363">AzCopy anger alltid hello Content-MD5-egenskapen för ett Azure blob eller en fil efter att överföra den toohello service.</span><span class="sxs-lookup"><span data-stu-id="b6446-363">AzCopy always sets hello Content-MD5 property for an Azure blob or file after uploading it toohello service.</span></span>  

<span data-ttu-id="b6446-364">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="b6446-364">**Applicable to:** Blobs, Files</span></span>

### <a name="snapshot"></a><span data-ttu-id="b6446-365">/ Ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="b6446-365">/Snapshot</span></span>
<span data-ttu-id="b6446-366">Anger om tootransfer ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="b6446-366">Indicates whether tootransfer snapshots.</span></span> <span data-ttu-id="b6446-367">Det här alternativet gäller endast om hello källan är en blob.</span><span class="sxs-lookup"><span data-stu-id="b6446-367">This option is only valid when hello source is a blob.</span></span>

<span data-ttu-id="b6446-368">hello överförda blob ögonblicksbilder ändras i det här formatet: .extension blobbnamnet (snapshot-time)</span><span class="sxs-lookup"><span data-stu-id="b6446-368">hello transferred blob snapshots are renamed in this format: blob-name (snapshot-time).extension</span></span>

<span data-ttu-id="b6446-369">Som standard kopieras inte ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="b6446-369">By default, snapshots are not copied.</span></span>

<span data-ttu-id="b6446-370">**Gäller för:** Blobbar</span><span class="sxs-lookup"><span data-stu-id="b6446-370">**Applicable to:** Blobs</span></span>

### <a name="vverbose-log-file"></a><span data-ttu-id="b6446-371">/ V: [utförlig log-fil]</span><span class="sxs-lookup"><span data-stu-id="b6446-371">/V:[verbose-log-file]</span></span>
<span data-ttu-id="b6446-372">Utdata utförlig statusmeddelanden till en loggfil.</span><span class="sxs-lookup"><span data-stu-id="b6446-372">Outputs verbose status messages into a log file.</span></span>

<span data-ttu-id="b6446-373">Som standard heter hello detaljerad loggfil AzCopyVerbose.log i `%LocalAppData%\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="b6446-373">By default, hello verbose log file is named AzCopyVerbose.log in `%LocalAppData%\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="b6446-374">Om du anger en befintlig plats för det här alternativet kommer hello utförlig loggen vara tillagda toothat-fil.</span><span class="sxs-lookup"><span data-stu-id="b6446-374">If you specify an existing file location for this option, hello verbose log will be appended toothat file.</span></span>  

<span data-ttu-id="b6446-375">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-375">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="zjournal-file-folder"></a><span data-ttu-id="b6446-376">/ Z: [journal-filmapp]</span><span class="sxs-lookup"><span data-stu-id="b6446-376">/Z:[journal-file-folder]</span></span>
<span data-ttu-id="b6446-377">Anger en mapp för att återuppta en åtgärd journalen.</span><span class="sxs-lookup"><span data-stu-id="b6446-377">Specifies a journal file folder for resuming an operation.</span></span>

<span data-ttu-id="b6446-378">AzCopy stöder alltid fortsätter om en åtgärd har avbrutits.</span><span class="sxs-lookup"><span data-stu-id="b6446-378">AzCopy always supports resuming if an operation has been interrupted.</span></span>

<span data-ttu-id="b6446-379">Om det här alternativet inte har angetts eller om den har angetts utan en mappsökväg, sedan skapar AzCopy hello journal-fil i hello standardplatsen, som är % LocalAppData%\Microsoft\Azure\AzCopy.</span><span class="sxs-lookup"><span data-stu-id="b6446-379">If this option is not specified, or it is specified without a folder path, then AzCopy will create hello journal file in hello default location, which is %LocalAppData%\Microsoft\Azure\AzCopy.</span></span>

<span data-ttu-id="b6446-380">Varje gång du utfärda ett kommando tooAzCopy kontrollerar den om en journal-fil finns i hello standardmappen eller om den finns i en mapp som du har angett via det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="b6446-380">Each time you issue a command tooAzCopy, it checks whether a journal file exists in hello default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="b6446-381">Om hello journalfilen inte finns på någon plats, behandlar hello åtgärden som nya AzCopy och genererar en ny journalfil.</span><span class="sxs-lookup"><span data-stu-id="b6446-381">If hello journal file does not exist in either place, AzCopy treats hello operation as new and generates a new journal file.</span></span>

<span data-ttu-id="b6446-382">Om hello journalfilen finns kontrollerar AzCopy om hello-kommandoraden som du matar in matchar hello kommandoraden i hello journal-fil.</span><span class="sxs-lookup"><span data-stu-id="b6446-382">If hello journal file does exist, AzCopy will check whether hello command line that you input matches hello command line in hello journal file.</span></span> <span data-ttu-id="b6446-383">Om hello två kommandorader matchar återupptar AzCopy hello ofullständiga igen.</span><span class="sxs-lookup"><span data-stu-id="b6446-383">If hello two command lines match, AzCopy resumes hello incomplete operation.</span></span> <span data-ttu-id="b6446-384">Du kommer att tillfrågas tooeither överskrivning hello journal toostart en ny åtgärd eller toocancel hello aktuella filåtgärd om de inte matchar.</span><span class="sxs-lookup"><span data-stu-id="b6446-384">If they do not match, you will be prompted tooeither overwrite hello journal file toostart a new operation, or toocancel hello current operation.</span></span>

<span data-ttu-id="b6446-385">hello journalfilen tas bort vid slutförande av hello igen.</span><span class="sxs-lookup"><span data-stu-id="b6446-385">hello journal file is deleted upon successful completion of hello operation.</span></span>

<span data-ttu-id="b6446-386">Observera att återuppta en åtgärd från en journal-fil som skapats av en tidigare version av AzCopy inte stöds.</span><span class="sxs-lookup"><span data-stu-id="b6446-386">Note that resuming an operation from a journal file created by a previous version of AzCopy is not supported.</span></span>

<span data-ttu-id="b6446-387">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-387">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="parameter-file"></a><span data-ttu-id="b6446-388">/@:"parameter-File”</span><span class="sxs-lookup"><span data-stu-id="b6446-388">/@:"parameter-file"</span></span>
<span data-ttu-id="b6446-389">Anger en fil som innehåller parametrar.</span><span class="sxs-lookup"><span data-stu-id="b6446-389">Specifies a file that contains parameters.</span></span> <span data-ttu-id="b6446-390">AzCopy processer hello parametrar i hello-filen som om de hade angetts på kommandoraden för hello.</span><span class="sxs-lookup"><span data-stu-id="b6446-390">AzCopy processes hello parameters in hello file just as if they had been specified on hello command line.</span></span>

<span data-ttu-id="b6446-391">I en svarsfil, kan du antingen ange flera parametrar på en enda rad eller ange varje parameter på en egen rad.</span><span class="sxs-lookup"><span data-stu-id="b6446-391">In a response file, you can either specify multiple parameters on a single line, or specify each parameter on its own line.</span></span> <span data-ttu-id="b6446-392">Observera att en enskild parameter inte kan innehålla flera rader.</span><span class="sxs-lookup"><span data-stu-id="b6446-392">Note that an individual parameter cannot span multiple lines.</span></span>

<span data-ttu-id="b6446-393">Svarsfiler kan innehålla kommentarer rader som börjar med hello # symbolen.</span><span class="sxs-lookup"><span data-stu-id="b6446-393">Response files can include comments lines that begin with hello # symbol.</span></span>

<span data-ttu-id="b6446-394">Du kan ange flera svarsfiler.</span><span class="sxs-lookup"><span data-stu-id="b6446-394">You can specify multiple response files.</span></span> <span data-ttu-id="b6446-395">Observera dock att AzCopy inte stöder kapslade svarsfiler.</span><span class="sxs-lookup"><span data-stu-id="b6446-395">However, note that AzCopy does not support nested response files.</span></span>

<span data-ttu-id="b6446-396">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-396">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="y"></a><span data-ttu-id="b6446-397">/Y</span><span class="sxs-lookup"><span data-stu-id="b6446-397">/Y</span></span>
<span data-ttu-id="b6446-398">Förhindrar att alla meddelanden för bekräftelse av AzCopy.</span><span class="sxs-lookup"><span data-stu-id="b6446-398">Suppresses all AzCopy confirmation prompts.</span></span>

<span data-ttu-id="b6446-399">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-399">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="l"></a><span data-ttu-id="b6446-400">/ L</span><span class="sxs-lookup"><span data-stu-id="b6446-400">/L</span></span>
<span data-ttu-id="b6446-401">Anger en åtgärd. Inga data kopieras.</span><span class="sxs-lookup"><span data-stu-id="b6446-401">Specifies a listing operation only; no data is copied.</span></span>

<span data-ttu-id="b6446-402">AzCopy tolkar hello med det här alternativet som en simulering för körs hello kommandoraden utan det här alternativet /L och räkna hur många objekt som ska kopieras kan du ange alternativet /V på hello samma tid toocheck vilka objekt som ska kopieras i hello utförlig loggen.</span><span class="sxs-lookup"><span data-stu-id="b6446-402">AzCopy will interpret hello using of this option as a simulation for running hello command line without this option /L and count how many objects will be copied, you can specify option /V at hello same time toocheck which objects will be copied in hello verbose log.</span></span>

<span data-ttu-id="b6446-403">hello beteendet för det här alternativet beror också hello plats hello källdata och hello förekomst av hello rekursiv alternativet /S och filen mönster läge /Pattern.</span><span class="sxs-lookup"><span data-stu-id="b6446-403">hello behavior of this option is also determined by hello location of hello source data and hello presence of hello recursive mode option /S and file pattern option /Pattern.</span></span>

<span data-ttu-id="b6446-404">AzCopy kräver behörighet att lista och läsa den här källplatsen när du använder det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="b6446-404">AzCopy requires LIST and READ permission of this source location when using this option.</span></span>

<span data-ttu-id="b6446-405">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="b6446-405">**Applicable to:** Blobs, Files</span></span>

### <a name="mt"></a><span data-ttu-id="b6446-406">/MT</span><span class="sxs-lookup"><span data-stu-id="b6446-406">/MT</span></span>
<span data-ttu-id="b6446-407">Anger hello nedladdade filen modifierades senast toobe hello samma som hello källa blob eller filen.</span><span class="sxs-lookup"><span data-stu-id="b6446-407">Sets hello downloaded file's last-modified time toobe hello same as hello source blob or file's.</span></span>

<span data-ttu-id="b6446-408">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="b6446-408">**Applicable to:** Blobs, Files</span></span>

### <a name="xn"></a><span data-ttu-id="b6446-409">/XN</span><span class="sxs-lookup"><span data-stu-id="b6446-409">/XN</span></span>
<span data-ttu-id="b6446-410">Undantar en nyare käll-resurs.</span><span class="sxs-lookup"><span data-stu-id="b6446-410">Excludes a newer source resource.</span></span> <span data-ttu-id="b6446-411">hello resurs kopieras inte om hello senast ändrad av hello källa är hello samma eller en senare än målet.</span><span class="sxs-lookup"><span data-stu-id="b6446-411">hello resource will not be copied if hello last modified time of hello source is hello same or newer than destination.</span></span>

<span data-ttu-id="b6446-412">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="b6446-412">**Applicable to:** Blobs, Files</span></span>

### <a name="xo"></a><span data-ttu-id="b6446-413">/XO</span><span class="sxs-lookup"><span data-stu-id="b6446-413">/XO</span></span>
<span data-ttu-id="b6446-414">Undantar en äldre käll-resurs.</span><span class="sxs-lookup"><span data-stu-id="b6446-414">Excludes an older source resource.</span></span> <span data-ttu-id="b6446-415">hello resurs kopieras inte om hello senast ändrad av hello källa är hello samma eller äldre än målet.</span><span class="sxs-lookup"><span data-stu-id="b6446-415">hello resource will not be copied if hello last modified time of hello source is hello same or older than destination.</span></span>

<span data-ttu-id="b6446-416">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="b6446-416">**Applicable to:** Blobs, Files</span></span>

### <a name="a"></a><span data-ttu-id="b6446-417">/A</span><span class="sxs-lookup"><span data-stu-id="b6446-417">/A</span></span>
<span data-ttu-id="b6446-418">Överför bara filer som har hello Arkiv-attributet inställt.</span><span class="sxs-lookup"><span data-stu-id="b6446-418">Uploads only files that have hello Archive attribute set.</span></span>

<span data-ttu-id="b6446-419">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="b6446-419">**Applicable to:** Blobs, Files</span></span>

### <a name="iarashcnetoi"></a><span data-ttu-id="b6446-420">/ IA: [RASHCNETOI]</span><span class="sxs-lookup"><span data-stu-id="b6446-420">/IA:[RASHCNETOI]</span></span>
<span data-ttu-id="b6446-421">Överföringar endast filer med någon av hello angiven attribut.</span><span class="sxs-lookup"><span data-stu-id="b6446-421">Uploads only files that have any of hello specified attributes set.</span></span>

<span data-ttu-id="b6446-422">Tillgängliga attribut inkluderar:</span><span class="sxs-lookup"><span data-stu-id="b6446-422">Available attributes include:</span></span>

* <span data-ttu-id="b6446-423">R = skrivskyddade filer</span><span class="sxs-lookup"><span data-stu-id="b6446-423">R = Read-only files</span></span>
* <span data-ttu-id="b6446-424">A = filer som är klara för arkivering</span><span class="sxs-lookup"><span data-stu-id="b6446-424">A = Files ready for archiving</span></span>
* <span data-ttu-id="b6446-425">S = systemfiler</span><span class="sxs-lookup"><span data-stu-id="b6446-425">S = System files</span></span>
* <span data-ttu-id="b6446-426">H = dolda filer</span><span class="sxs-lookup"><span data-stu-id="b6446-426">H = Hidden files</span></span>
* <span data-ttu-id="b6446-427">C = komprimerade filer</span><span class="sxs-lookup"><span data-stu-id="b6446-427">C = Compressed files</span></span>
* <span data-ttu-id="b6446-428">N = normala filer</span><span class="sxs-lookup"><span data-stu-id="b6446-428">N = Normal files</span></span>
* <span data-ttu-id="b6446-429">E = krypterade filer</span><span class="sxs-lookup"><span data-stu-id="b6446-429">E = Encrypted files</span></span>
* <span data-ttu-id="b6446-430">T = temporära filer</span><span class="sxs-lookup"><span data-stu-id="b6446-430">T = Temporary files</span></span>
* <span data-ttu-id="b6446-431">O = Offline-filer</span><span class="sxs-lookup"><span data-stu-id="b6446-431">O = Offline files</span></span>
* <span data-ttu-id="b6446-432">Jag = icke-indexerat filer</span><span class="sxs-lookup"><span data-stu-id="b6446-432">I = Non-indexed files</span></span>

<span data-ttu-id="b6446-433">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="b6446-433">**Applicable to:** Blobs, Files</span></span>

### <a name="xarashcnetoi"></a><span data-ttu-id="b6446-434">/ XA: [RASHCNETOI]</span><span class="sxs-lookup"><span data-stu-id="b6446-434">/XA:[RASHCNETOI]</span></span>
<span data-ttu-id="b6446-435">Undantar filer med någon av hello angivna attribut uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="b6446-435">Excludes files that have any of hello specified attributes set.</span></span>

<span data-ttu-id="b6446-436">Tillgängliga attribut inkluderar:</span><span class="sxs-lookup"><span data-stu-id="b6446-436">Available attributes include:</span></span>

* <span data-ttu-id="b6446-437">R = skrivskyddade filer</span><span class="sxs-lookup"><span data-stu-id="b6446-437">R = Read-only files</span></span>
* <span data-ttu-id="b6446-438">A = filer som är klara för arkivering</span><span class="sxs-lookup"><span data-stu-id="b6446-438">A = Files ready for archiving</span></span>
* <span data-ttu-id="b6446-439">S = systemfiler</span><span class="sxs-lookup"><span data-stu-id="b6446-439">S = System files</span></span>
* <span data-ttu-id="b6446-440">H = dolda filer</span><span class="sxs-lookup"><span data-stu-id="b6446-440">H = Hidden files</span></span>
* <span data-ttu-id="b6446-441">C = komprimerade filer</span><span class="sxs-lookup"><span data-stu-id="b6446-441">C = Compressed files</span></span>
* <span data-ttu-id="b6446-442">N = normala filer</span><span class="sxs-lookup"><span data-stu-id="b6446-442">N = Normal files</span></span>
* <span data-ttu-id="b6446-443">E = krypterade filer</span><span class="sxs-lookup"><span data-stu-id="b6446-443">E = Encrypted files</span></span>
* <span data-ttu-id="b6446-444">T = temporära filer</span><span class="sxs-lookup"><span data-stu-id="b6446-444">T = Temporary files</span></span>
* <span data-ttu-id="b6446-445">O = Offline-filer</span><span class="sxs-lookup"><span data-stu-id="b6446-445">O = Offline files</span></span>
* <span data-ttu-id="b6446-446">Jag = icke-indexerat filer</span><span class="sxs-lookup"><span data-stu-id="b6446-446">I = Non-indexed files</span></span>

<span data-ttu-id="b6446-447">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="b6446-447">**Applicable to:** Blobs, Files</span></span>

### <a name="delimiterdelimiter"></a><span data-ttu-id="b6446-448">/ Avgränsare: ”avgränsare”</span><span class="sxs-lookup"><span data-stu-id="b6446-448">/Delimiter:"delimiter"</span></span>
<span data-ttu-id="b6446-449">Anger hello avgränsningstecken används toodelimit virtuella kataloger i ett blob-namn.</span><span class="sxs-lookup"><span data-stu-id="b6446-449">Indicates hello delimiter character used toodelimit virtual directories in a blob name.</span></span>

<span data-ttu-id="b6446-450">Som standard använder AzCopy / som hello avgränsningstecken.</span><span class="sxs-lookup"><span data-stu-id="b6446-450">By default, AzCopy uses / as hello delimiter character.</span></span> <span data-ttu-id="b6446-451">AzCopy stöder dock använda alla vanliga tecken (t ex @, # eller %) som avgränsare.</span><span class="sxs-lookup"><span data-stu-id="b6446-451">However, AzCopy supports using any common character (such as @, #, or %) as a delimiter.</span></span> <span data-ttu-id="b6446-452">Om du behöver tooinclude något av dessa specialtecken på kommandoraden för hello omge hello filnamn med dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="b6446-452">If you need tooinclude one of these special characters on hello command line, enclose hello file name with double quotes.</span></span>

<span data-ttu-id="b6446-453">Det här alternativet gäller endast för att ladda ned blobbar.</span><span class="sxs-lookup"><span data-stu-id="b6446-453">This option is only applicable for downloading blobs.</span></span>

<span data-ttu-id="b6446-454">**Gäller för:** Blobbar</span><span class="sxs-lookup"><span data-stu-id="b6446-454">**Applicable to:** Blobs</span></span>

### <a name="ncnumber-of-concurrent-operations"></a><span data-ttu-id="b6446-455">/ NC: ”många-för-samtidiga-operationer”</span><span class="sxs-lookup"><span data-stu-id="b6446-455">/NC:"number-of-concurrent-operations"</span></span>
<span data-ttu-id="b6446-456">Anger hello antalet samtidiga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b6446-456">Specifies hello number of concurrent operations.</span></span>

<span data-ttu-id="b6446-457">AzCopy som standard startar ett visst antal samtidiga åtgärder tooincrease hello data transfer genomflöde.</span><span class="sxs-lookup"><span data-stu-id="b6446-457">AzCopy by default starts a certain number of concurrent operations tooincrease hello data transfer throughput.</span></span> <span data-ttu-id="b6446-458">Observera att många samtidiga åtgärder i en miljö med låg bandbredd kan överväldigande hello nätverksanslutning och förhindra hello operations fullständigt slutförs.</span><span class="sxs-lookup"><span data-stu-id="b6446-458">Note that large number of concurrent operations in a low-bandwidth environment may overwhelm hello network connection and prevent hello operations from fully completing.</span></span> <span data-ttu-id="b6446-459">Begränsning samtidiga åtgärder baserat på faktiska tillgänglig nätverksbandbredd.</span><span class="sxs-lookup"><span data-stu-id="b6446-459">Throttle concurrent operations based on actual available network bandwidth.</span></span>

<span data-ttu-id="b6446-460">hello övre gräns för samtidiga åtgärder är 512.</span><span class="sxs-lookup"><span data-stu-id="b6446-460">hello upper limit for concurrent operations is 512.</span></span>

<span data-ttu-id="b6446-461">**Gäller för:** Blobbar, filer, tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-461">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcetypeblob--table"></a><span data-ttu-id="b6446-462">/ SourceType: ”Blob” | ”Table”</span><span class="sxs-lookup"><span data-stu-id="b6446-462">/SourceType:"Blob" | "Table"</span></span>
<span data-ttu-id="b6446-463">Anger att hello `source` resursen är en blob som är tillgängliga i hello lokala utvecklingsmiljö körs i hello storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b6446-463">Specifies that hello `source` resource is a blob available in hello local development environment, running in hello storage emulator.</span></span>

<span data-ttu-id="b6446-464">**Gäller för:** Blobbar, tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-464">**Applicable to:** Blobs, Tables</span></span>

### <a name="desttypeblob--table"></a><span data-ttu-id="b6446-465">/ DestType: ”Blob” | ”Table”</span><span class="sxs-lookup"><span data-stu-id="b6446-465">/DestType:"Blob" | "Table"</span></span>
<span data-ttu-id="b6446-466">Anger att hello `destination` resursen är en blob som är tillgängliga i hello lokala utvecklingsmiljö körs i hello storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b6446-466">Specifies that hello `destination` resource is a blob available in hello local development environment, running in hello storage emulator.</span></span>

<span data-ttu-id="b6446-467">**Gäller för:** Blobbar, tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-467">**Applicable to:** Blobs, Tables</span></span>

### <a name="pkrskey1key2key3"></a><span data-ttu-id="b6446-468">/ PKRS ”: key&#1;key2 key&#3;...”</span><span class="sxs-lookup"><span data-stu-id="b6446-468">/PKRS:"key1#key2#key3#..."</span></span>
<span data-ttu-id="b6446-469">Delningar hello partition viktiga intervallet tooenable Exportera tabelldata parallellt, vilket ökar hello exportåtgärden hello hastighet.</span><span class="sxs-lookup"><span data-stu-id="b6446-469">Splits hello partition key range tooenable exporting table data in parallel, which increases hello speed of hello export operation.</span></span>

<span data-ttu-id="b6446-470">Om det här alternativet inte anges används AzCopy tabellentiteter för tooexport en enkel tråd.</span><span class="sxs-lookup"><span data-stu-id="b6446-470">If this option is not specified, then AzCopy uses a single thread tooexport table entities.</span></span> <span data-ttu-id="b6446-471">Till exempel om hello användaren anger /PKRS: ”aa #bb” och sedan AzCopy startar tre samtidiga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b6446-471">For example, if hello user specifies /PKRS:"aa#bb", then AzCopy starts three concurrent operations.</span></span>

<span data-ttu-id="b6446-472">Varje åtgärd exporterar en av tre partition nyckelintervall, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="b6446-472">Each operation exports one of three partition key ranges, as shown below:</span></span>

  <span data-ttu-id="b6446-473">[första partitionsnyckel, aa)</span><span class="sxs-lookup"><span data-stu-id="b6446-473">[first-partition-key, aa)</span></span>

  <span data-ttu-id="b6446-474">[aa, bb)</span><span class="sxs-lookup"><span data-stu-id="b6446-474">[aa, bb)</span></span>

  <span data-ttu-id="b6446-475">[bb senaste partitionsnyckel]</span><span class="sxs-lookup"><span data-stu-id="b6446-475">[bb, last-partition-key]</span></span>

<span data-ttu-id="b6446-476">**Gäller för:** tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-476">**Applicable to:** Tables</span></span>

### <a name="splitsizefile-size"></a><span data-ttu-id="b6446-477">/ SplitSize: ”filstorlek”</span><span class="sxs-lookup"><span data-stu-id="b6446-477">/SplitSize:"file-size"</span></span>
<span data-ttu-id="b6446-478">Anger hello exporterade filen dela storlek i MB hello tillåtna minimalt värde är 32.</span><span class="sxs-lookup"><span data-stu-id="b6446-478">Specifies hello exported file split size in MB, hello minimal value allowed is 32.</span></span>

<span data-ttu-id="b6446-479">Om det här alternativet inte anges kommer att exportera tabellen datafilen toosingle AzCopy.</span><span class="sxs-lookup"><span data-stu-id="b6446-479">If this option is not specified, AzCopy will export table data toosingle file.</span></span>

<span data-ttu-id="b6446-480">Om hello tabelldata är exporterade tooa blob och hello exporterade filstorleken är hello 200 GB gränsen för blob-storlek, kommer AzCopy dela hello exporterade filen, även om det här alternativet inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="b6446-480">If hello table data is exported tooa blob, and hello exported file size reaches hello 200 GB limit for blob size, then AzCopy will split hello exported file, even if this option is not specified.</span></span>

<span data-ttu-id="b6446-481">**Gäller för:** tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-481">**Applicable to:** Tables</span></span>

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a><span data-ttu-id="b6446-482">/ EntityOperation: ”InsertOrSkip” | ”InsertOrMerge” | ”InsertOrReplace”</span><span class="sxs-lookup"><span data-stu-id="b6446-482">/EntityOperation:"InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"</span></span>
<span data-ttu-id="b6446-483">Anger hello Tabellfunktioner data import.</span><span class="sxs-lookup"><span data-stu-id="b6446-483">Specifies hello table data import behavior.</span></span>

* <span data-ttu-id="b6446-484">InsertOrSkip - hoppar över en befintlig entitet eller infogar en ny entitet om den inte finns i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="b6446-484">InsertOrSkip - Skips an existing entity or inserts a new entity if it does not exist in hello table.</span></span>
* <span data-ttu-id="b6446-485">InsertOrMerge - sammanfogar en befintlig entitet eller infogar en ny entitet om den inte finns i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="b6446-485">InsertOrMerge - Merges an existing entity or inserts a new entity if it does not exist in hello table.</span></span>
* <span data-ttu-id="b6446-486">InsertOrReplace - ersätter en befintlig entitet eller infogar en ny entitet om den inte finns i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="b6446-486">InsertOrReplace - Replaces an existing entity or inserts a new entity if it does not exist in hello table.</span></span>

<span data-ttu-id="b6446-487">**Gäller för:** tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-487">**Applicable to:** Tables</span></span>

### <a name="manifestmanifest-file"></a><span data-ttu-id="b6446-488">/ Manifest: ”manifest-fil”</span><span class="sxs-lookup"><span data-stu-id="b6446-488">/Manifest:"manifest-file"</span></span>
<span data-ttu-id="b6446-489">Anger hello manifestfilen för hello tabell exportera och importera igen.</span><span class="sxs-lookup"><span data-stu-id="b6446-489">Specifies hello manifest file for hello table export and import operation.</span></span>

<span data-ttu-id="b6446-490">Det här alternativet är valfritt under exportåtgärden hello, AzCopy genererar en manifestfil med fördefinierade namnet om det här alternativet inte anges.</span><span class="sxs-lookup"><span data-stu-id="b6446-490">This option is optional during hello export operation, AzCopy will generate a manifest file with predefined name if this option is not specified.</span></span>

<span data-ttu-id="b6446-491">Det här alternativet krävs under hello importen för att hitta hello datafiler.</span><span class="sxs-lookup"><span data-stu-id="b6446-491">This option is required during hello import operation for locating hello data files.</span></span>

<span data-ttu-id="b6446-492">**Gäller för:** tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-492">**Applicable to:** Tables</span></span>

### <a name="synccopy"></a><span data-ttu-id="b6446-493">/ SyncCopy</span><span class="sxs-lookup"><span data-stu-id="b6446-493">/SyncCopy</span></span>
<span data-ttu-id="b6446-494">Anger om toosynchronously kopiera BLOB eller filer mellan två Azure Storage-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="b6446-494">Indicates whether toosynchronously copy blobs or files between two Azure Storage endpoints.</span></span>

<span data-ttu-id="b6446-495">AzCopy som standard använder serversidan asynkron kopia.</span><span class="sxs-lookup"><span data-stu-id="b6446-495">AzCopy by default uses server-side asynchronous copy.</span></span> <span data-ttu-id="b6446-496">Ange det här alternativet tooperform en synkron kopiera som laddar ned blobbar eller filer toolocal minne och överför dem tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="b6446-496">Specify this option tooperform a synchronous copy, which downloads blobs or files toolocal memory and then uploads them tooAzure Storage.</span></span>

<span data-ttu-id="b6446-497">Du kan använda det här alternativet när du kopierar filer i Blob storage fillagring, eller från Blob storage tooFile lagringsutrymme eller tvärtom.</span><span class="sxs-lookup"><span data-stu-id="b6446-497">You can use this option when copying files within Blob storage, within File storage, or from Blob storage tooFile storage or vice versa.</span></span>

<span data-ttu-id="b6446-498">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="b6446-498">**Applicable to:** Blobs, Files</span></span>

### <a name="setcontenttypecontent-type"></a><span data-ttu-id="b6446-499">/ SetContentType: ”content-type”</span><span class="sxs-lookup"><span data-stu-id="b6446-499">/SetContentType:"content-type"</span></span>
<span data-ttu-id="b6446-500">Anger hello MIME content-type för mål-BLOB eller filer.</span><span class="sxs-lookup"><span data-stu-id="b6446-500">Specifies hello MIME content type for destination blobs or files.</span></span>

<span data-ttu-id="b6446-501">Uppsättningar av AzCopy hello content-type för en blob eller filen tooapplication/oktett-ström som standard.</span><span class="sxs-lookup"><span data-stu-id="b6446-501">AzCopy sets hello content type for a blob or file tooapplication/octet-stream by default.</span></span> <span data-ttu-id="b6446-502">Du kan ange hello content-type för alla blobbar eller filer genom att ange ett värde för det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="b6446-502">You can set hello content type for all blobs or files by explicitly specifying a value for this option.</span></span>

<span data-ttu-id="b6446-503">Om du anger det här alternativet utan ett värde ställs AzCopy varje blob eller filens innehållstyp enligt tooits filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="b6446-503">If you specify this option without a value, then AzCopy will set each blob or file's content type according tooits file extension.</span></span>

<span data-ttu-id="b6446-504">**Gäller för:** Blobbar, filer</span><span class="sxs-lookup"><span data-stu-id="b6446-504">**Applicable to:** Blobs, Files</span></span>

### <a name="payloadformatjson--csv"></a><span data-ttu-id="b6446-505">/ PayloadFormat: ”JSON” | ”CSV”</span><span class="sxs-lookup"><span data-stu-id="b6446-505">/PayloadFormat:"JSON" | "CSV"</span></span>
<span data-ttu-id="b6446-506">Anger hello formatet på filen med exporterade data hello tabell.</span><span class="sxs-lookup"><span data-stu-id="b6446-506">Specifies hello format of hello table exported data file.</span></span>

<span data-ttu-id="b6446-507">Om det här alternativet inte anges, exporterar AzCopy tabell datafilen i JSON-format som standard.</span><span class="sxs-lookup"><span data-stu-id="b6446-507">If this option is not specified, by default AzCopy exports table data file in JSON format.</span></span>

<span data-ttu-id="b6446-508">**Gäller för:** tabeller</span><span class="sxs-lookup"><span data-stu-id="b6446-508">**Applicable to:** Tables</span></span>

## <a name="known-issues-and-best-practices"></a><span data-ttu-id="b6446-509">Kända problem och bästa praxis</span><span class="sxs-lookup"><span data-stu-id="b6446-509">Known Issues and Best Practices</span></span>
### <a name="limit-concurrent-writes-while-copying-data"></a><span data-ttu-id="b6446-510">Begränsa samtidiga skrivningar vid kopiering av data</span><span class="sxs-lookup"><span data-stu-id="b6446-510">Limit concurrent writes while copying data</span></span>
<span data-ttu-id="b6446-511">När du kopierar blobar eller filer med AzCopy, Tänk på att ett annat program kanske ändra hello data när du kopierar den.</span><span class="sxs-lookup"><span data-stu-id="b6446-511">When you copy blobs or files with AzCopy, keep in mind that another application may be modifying hello data while you are copying it.</span></span> <span data-ttu-id="b6446-512">Kontrollera om det är möjligt att hello data som du kopierar inte ändras under hello kopieringen.</span><span class="sxs-lookup"><span data-stu-id="b6446-512">If possible, ensure that hello data you are copying is not being modified during hello copy operation.</span></span> <span data-ttu-id="b6446-513">Till exempel när du kopierar en virtuell Hårddisk som är kopplad till en virtuell Azure-dator, kontrollera att inga andra program för närvarande skriver toohello VHD.</span><span class="sxs-lookup"><span data-stu-id="b6446-513">For example, when copying a VHD associated with an Azure virtual machine, make sure that no other applications are currently writing toohello VHD.</span></span> <span data-ttu-id="b6446-514">Ett bra sätt toodo denna genom leasing hello resurs toobe kopieras.</span><span class="sxs-lookup"><span data-stu-id="b6446-514">A good way toodo this is by leasing hello resource toobe copied.</span></span> <span data-ttu-id="b6446-515">Alternativt kan du skapa en ögonblicksbild av hello VHD först och sedan kopiera hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="b6446-515">Alternately, you can create a snapshot of hello VHD first and then copy hello snapshot.</span></span>

<span data-ttu-id="b6446-516">Om du inte hindra andra program från att skriva tooblobs eller filer när de kopieras och Kom ihåg att av hello hello jobbet har slutförts kan hello kopiera resurserna inte längre ha fullständig paritet med hello källa resurser.</span><span class="sxs-lookup"><span data-stu-id="b6446-516">If you cannot prevent other applications from writing tooblobs or files while they are being copied, then keep in mind that by hello time hello job finishes, hello copied resources may no longer have full parity with hello source resources.</span></span>

### <a name="run-one-azcopy-instance-on-one-machine"></a><span data-ttu-id="b6446-517">Köra en AzCopy-instans på en dator.</span><span class="sxs-lookup"><span data-stu-id="b6446-517">Run one AzCopy instance on one machine.</span></span>
<span data-ttu-id="b6446-518">AzCopy är utformad toomaximize hello användning av din dator resurs tooaccelerate hello dataöverföring, rekommenderar vi du kör endast en AzCopy-instans på en dator och ange hello alternativet `/NC` om du behöver fler samtidiga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b6446-518">AzCopy is designed toomaximize hello utilization of your machine resource tooaccelerate hello data transfer, we recommend you run only one AzCopy instance on one machine, and specify hello option `/NC` if you need more concurrent operations.</span></span> <span data-ttu-id="b6446-519">Mer information, Skriv `AzCopy /?:NC` hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="b6446-519">For more details, type `AzCopy /?:NC` at hello command line.</span></span>

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a><span data-ttu-id="b6446-520">Aktivera FIPS-kompatibla MD5 algoritmer för AzCopy när du ”Använd FIPS-kompatibla algoritmer för kryptering, hashing och signering”.</span><span class="sxs-lookup"><span data-stu-id="b6446-520">Enable FIPS compliant MD5 algorithms for AzCopy when you "Use FIPS compliant algorithms for encryption, hashing and signing".</span></span>
<span data-ttu-id="b6446-521">AzCopy som standard använder .NET MD5 implementering toocalculate hello MD5 när du kopierar objekt, men det finns vissa säkerhetskrav som behöver AzCopy-tooenable FIPS kompatibla MD5-inställningen.</span><span class="sxs-lookup"><span data-stu-id="b6446-521">AzCopy by default uses .NET MD5 implementation toocalculate hello MD5 when copying objects, but there are some security requirements that need AzCopy tooenable FIPS compliant MD5 setting.</span></span>

<span data-ttu-id="b6446-522">Du kan skapa en app.config-fil `AzCopy.exe.config` med egenskapen `AzureStorageUseV1MD5` och placera den reservera med AzCopy.exe.</span><span class="sxs-lookup"><span data-stu-id="b6446-522">You can create an app.config file `AzCopy.exe.config` with property `AzureStorageUseV1MD5` and put it aside with AzCopy.exe.</span></span>

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

<span data-ttu-id="b6446-523">För egenskapen ”AzureStorageUseV1MD5” • True - använder hello standardvärdet, AzCopy .NET MD5-implementering.</span><span class="sxs-lookup"><span data-stu-id="b6446-523">For property "AzureStorageUseV1MD5" • True - hello default value, AzCopy will use .NET MD5 implementation.</span></span>
<span data-ttu-id="b6446-524">• False – AzCopy använder FIPS-kompatibla MD5-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="b6446-524">• False – AzCopy will use FIPS compliant MD5 algorithm.</span></span>

<span data-ttu-id="b6446-525">Observera att FIPS-kompatibla algoritmer är inaktiverad som standard på din Windows-dator, du kan skriva secpol.msc i din körningsfönstret och kontrollera den här växeln på Säkerhet -> lokala principer -> säkerhet alternativ -> Systemkryptografi: Använd FIPS-kompatibel algoritmer för kryptering, hashing och signering.</span><span class="sxs-lookup"><span data-stu-id="b6446-525">Note that FIPS compliant algorithms is disabled by default on your Windows machine, you can type secpol.msc in your Run window and check this switch at Security Setting->Local Policy->Security Options->System cryptography: Use FIPS compliant algorithms for encryption, hashing and signing.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6446-526">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b6446-526">Next steps</span></span>
<span data-ttu-id="b6446-527">Mer information om Azure Storage och AzCopy finns toohello följande resurser.</span><span class="sxs-lookup"><span data-stu-id="b6446-527">For more information about Azure Storage and AzCopy, refer toohello following resources.</span></span>

### <a name="azure-storage-documentation"></a><span data-ttu-id="b6446-528">Azure Storage-dokumentation:</span><span class="sxs-lookup"><span data-stu-id="b6446-528">Azure Storage documentation:</span></span>
* [<span data-ttu-id="b6446-529">Introduktion tooAzure lagring</span><span class="sxs-lookup"><span data-stu-id="b6446-529">Introduction tooAzure Storage</span></span>](storage-introduction.md)
* [<span data-ttu-id="b6446-530">Hur toouse Blob storage från .NET</span><span class="sxs-lookup"><span data-stu-id="b6446-530">How toouse Blob storage from .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="b6446-531">Hur toouse File storage från .NET</span><span class="sxs-lookup"><span data-stu-id="b6446-531">How toouse File storage from .NET</span></span>](storage-dotnet-how-to-use-files.md)
* [<span data-ttu-id="b6446-532">Hur toouse Table storage från .NET</span><span class="sxs-lookup"><span data-stu-id="b6446-532">How toouse Table storage from .NET</span></span>](storage-dotnet-how-to-use-tables.md)
* [<span data-ttu-id="b6446-533">Hur toocreate, hantera eller ta bort ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="b6446-533">How toocreate, manage, or delete a storage account</span></span>](storage-create-storage-account.md)
* [<span data-ttu-id="b6446-534">Överföra data med AzCopy på Linux</span><span class="sxs-lookup"><span data-stu-id="b6446-534">Transfer data with AzCopy on Linux</span></span>](storage-use-azcopy-linux.md)

### <a name="azure-storage-blog-posts"></a><span data-ttu-id="b6446-535">Azure Storage blogginlägg:</span><span class="sxs-lookup"><span data-stu-id="b6446-535">Azure Storage blog posts:</span></span>
* [<span data-ttu-id="b6446-536">Introduktion till Azure Storage Data Movement Library Preview</span><span class="sxs-lookup"><span data-stu-id="b6446-536">Introducing Azure Storage Data Movement Library Preview</span></span>](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [<span data-ttu-id="b6446-537">AzCopy: Introduktion till synkron kopia och anpassade innehållstyp</span><span class="sxs-lookup"><span data-stu-id="b6446-537">AzCopy: Introducing synchronous copy and customized content type</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [<span data-ttu-id="b6446-538">AzCopy: Om allmän tillgänglighet AzCopy 3.0 plus förhandsversionen av AzCopy 4.0 med tabell- och support</span><span class="sxs-lookup"><span data-stu-id="b6446-538">AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [<span data-ttu-id="b6446-539">AzCopy: Optimerade för storskaliga kopiera scenarier</span><span class="sxs-lookup"><span data-stu-id="b6446-539">AzCopy: Optimized for Large-Scale Copy Scenarios</span></span>](http://go.microsoft.com/fwlink/?LinkId=507682)
* [<span data-ttu-id="b6446-540">AzCopy: Stöd för geo-redundant lagring med läsbehörighet</span><span class="sxs-lookup"><span data-stu-id="b6446-540">AzCopy: Support for read-access geo-redundant storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [<span data-ttu-id="b6446-541">AzCopy: Överföra data med nytt starta läge och SAS-token</span><span class="sxs-lookup"><span data-stu-id="b6446-541">AzCopy: Transfer data with re-startable mode and SAS token</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [<span data-ttu-id="b6446-542">AzCopy: Använder mellan konto kopiera Blob</span><span class="sxs-lookup"><span data-stu-id="b6446-542">AzCopy: Using cross-account Copy Blob</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [<span data-ttu-id="b6446-543">AzCopy: Överför/hämta filer för Azure-BLOB</span><span class="sxs-lookup"><span data-stu-id="b6446-543">AzCopy: Uploading/downloading files for Azure Blobs</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

