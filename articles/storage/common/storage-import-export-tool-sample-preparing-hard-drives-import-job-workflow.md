---
title: "aaaSample arbetsflöde tooprep hårddiskar för Azure Import/Export-importjobb | Microsoft Docs"
description: "Se en genomgång för hello Slutför process förbereder enheter för ett importjobb i hello Azure Import/Export service."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: muralikk
ms.openlocfilehash: fa7f36300b35b81757523de4960fd583bd4bf305
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sample-workflow-tooprepare-hard-drives-for-an-import-job"></a><span data-ttu-id="272cf-103">Exempel arbetsflödet tooprepare hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="272cf-103">Sample workflow tooprepare hard drives for an import job</span></span>

<span data-ttu-id="272cf-104">Den här artikeln vägleder dig genom hello Slutför process förbereder enheter för importen.</span><span class="sxs-lookup"><span data-stu-id="272cf-104">This article walks you through hello complete process of preparing drives for an import job.</span></span>

## <a name="sample-data"></a><span data-ttu-id="272cf-105">Exempeldata</span><span class="sxs-lookup"><span data-stu-id="272cf-105">Sample data</span></span>

<span data-ttu-id="272cf-106">Det här exemplet importerar hello efter data i ett Azure storage-konto med namnet `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="272cf-106">This example imports hello following data into an Azure storage account named `mystorageaccount`:</span></span>

|<span data-ttu-id="272cf-107">Plats</span><span class="sxs-lookup"><span data-stu-id="272cf-107">Location</span></span>|<span data-ttu-id="272cf-108">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="272cf-108">Description</span></span>|<span data-ttu-id="272cf-109">Datastorlek</span><span class="sxs-lookup"><span data-stu-id="272cf-109">Data size</span></span>|
|--------------|-----------------|-----|
|<span data-ttu-id="272cf-110">H:\Video\\</span><span class="sxs-lookup"><span data-stu-id="272cf-110">H:\Video\\</span></span> |<span data-ttu-id="272cf-111">En samling videor</span><span class="sxs-lookup"><span data-stu-id="272cf-111">A collection of videos</span></span>|<span data-ttu-id="272cf-112">12 TB</span><span class="sxs-lookup"><span data-stu-id="272cf-112">12 TB</span></span>|
|<span data-ttu-id="272cf-113">H:\Photo\\</span><span class="sxs-lookup"><span data-stu-id="272cf-113">H:\Photo\\</span></span> |<span data-ttu-id="272cf-114">En samling bilder</span><span class="sxs-lookup"><span data-stu-id="272cf-114">A collection of photos</span></span>|<span data-ttu-id="272cf-115">30 GB</span><span class="sxs-lookup"><span data-stu-id="272cf-115">30 GB</span></span>|
|<span data-ttu-id="272cf-116">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="272cf-116">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="272cf-117">A Blu-ray™ diskavbildning</span><span class="sxs-lookup"><span data-stu-id="272cf-117">A Blu-Ray™ disk image</span></span>|<span data-ttu-id="272cf-118">25 GB</span><span class="sxs-lookup"><span data-stu-id="272cf-118">25 GB</span></span>|
|<span data-ttu-id="272cf-119">\\\bigshare\john\music\\</span><span class="sxs-lookup"><span data-stu-id="272cf-119">\\\bigshare\john\music\\</span></span>|<span data-ttu-id="272cf-120">En samling musikfiler på en nätverksresurs</span><span class="sxs-lookup"><span data-stu-id="272cf-120">A collection of music files on a network share</span></span>|<span data-ttu-id="272cf-121">10 GB</span><span class="sxs-lookup"><span data-stu-id="272cf-121">10 GB</span></span>|

## <a name="storage-account-destinations"></a><span data-ttu-id="272cf-122">Mål för Storage-konto</span><span class="sxs-lookup"><span data-stu-id="272cf-122">Storage account destinations</span></span>

<span data-ttu-id="272cf-123">hello importjobbet importeras hello data till hello följande mål i hello storage-konto:</span><span class="sxs-lookup"><span data-stu-id="272cf-123">hello import job will import hello data into hello following destinations in hello storage account:</span></span>

|<span data-ttu-id="272cf-124">Källa</span><span class="sxs-lookup"><span data-stu-id="272cf-124">Source</span></span>|<span data-ttu-id="272cf-125">Mål virtuell katalog eller blob</span><span class="sxs-lookup"><span data-stu-id="272cf-125">Destination virtual directory or blob</span></span>|
|------------|-------------------------------------------|
|<span data-ttu-id="272cf-126">H:\Video\\</span><span class="sxs-lookup"><span data-stu-id="272cf-126">H:\Video\\</span></span> |<span data-ttu-id="272cf-127">video /</span><span class="sxs-lookup"><span data-stu-id="272cf-127">video/</span></span>|
|<span data-ttu-id="272cf-128">H:\Photo\\</span><span class="sxs-lookup"><span data-stu-id="272cf-128">H:\Photo\\</span></span> |<span data-ttu-id="272cf-129">foto /</span><span class="sxs-lookup"><span data-stu-id="272cf-129">photo/</span></span>|
|<span data-ttu-id="272cf-130">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="272cf-130">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="272cf-131">favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="272cf-131">favorite/FavoriteMovies.ISO</span></span>|
|<span data-ttu-id="272cf-132">\\\bigshare\john\music\\</span><span class="sxs-lookup"><span data-stu-id="272cf-132">\\\bigshare\john\music\\</span></span> |<span data-ttu-id="272cf-133">Musik</span><span class="sxs-lookup"><span data-stu-id="272cf-133">music</span></span>|

<span data-ttu-id="272cf-134">Med den här mappningen hello filen `H:\Video\Drama\GreatMovie.mov` blir importerade toohello blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span><span class="sxs-lookup"><span data-stu-id="272cf-134">With this mapping, hello file `H:\Video\Drama\GreatMovie.mov` will be imported toohello blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span></span>

## <a name="determine-hard-drive-requirements"></a><span data-ttu-id="272cf-135">Ange krav för hårddisk</span><span class="sxs-lookup"><span data-stu-id="272cf-135">Determine hard drive requirements</span></span>

<span data-ttu-id="272cf-136">Nästa toodetermine hur många hårddiskar krävs beräkning hello storleken på hello data:</span><span class="sxs-lookup"><span data-stu-id="272cf-136">Next, toodetermine how many hard drives are needed, compute hello size of hello data:</span></span>

`12TB + 30GB + 25GB + 10GB = 12TB + 65GB`

<span data-ttu-id="272cf-137">I det här exemplet är två 8TB-hårddiskar tillräckliga.</span><span class="sxs-lookup"><span data-stu-id="272cf-137">For this example, two 8TB hard drives should be sufficient.</span></span> <span data-ttu-id="272cf-138">Eftersom hello källkatalog `H:\Video` har 12TB data och den enda hårddisken kapacitet är 8TB, kommer du att kunna toospecify detta i hello följande sätt i hello **driveset.csv** fil:</span><span class="sxs-lookup"><span data-stu-id="272cf-138">However, since hello source directory `H:\Video` has 12TB of data and your single hard drive's capacity is only 8TB, you will be able toospecify this in hello following way in hello **driveset.csv** file:</span></span>

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
X,Format,SilentMode,Encrypt,
Y,Format,SilentMode,Encrypt,
```
<span data-ttu-id="272cf-139">hello verktyget distribuera data på två hårddiskar på ett optimerat sätt.</span><span class="sxs-lookup"><span data-stu-id="272cf-139">hello tool will distribute data across two hard drives in an optimized way.</span></span>

## <a name="attach-drives-and-configure-hello-job"></a><span data-ttu-id="272cf-140">Koppla enheter och konfigurera hello jobb</span><span class="sxs-lookup"><span data-stu-id="272cf-140">Attach drives and configure hello job</span></span>
<span data-ttu-id="272cf-141">Du ska ansluta båda diskarna toohello datorn och skapa volymer.</span><span class="sxs-lookup"><span data-stu-id="272cf-141">You will attach both disks toohello machine and create volumes.</span></span> <span data-ttu-id="272cf-142">Sedan redigerar **dataset.csv** fil:</span><span class="sxs-lookup"><span data-stu-id="272cf-142">Then author **dataset.csv** file:</span></span>
```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

<span data-ttu-id="272cf-143">Du kan dessutom ange hello efter metadata för alla filer:</span><span class="sxs-lookup"><span data-stu-id="272cf-143">In addition, you can set hello following metadata for all files:</span></span>

* <span data-ttu-id="272cf-144">**UploadMethod:** Windows Azure Import/Export service</span><span class="sxs-lookup"><span data-stu-id="272cf-144">**UploadMethod:** Windows Azure Import/Export service</span></span>
* <span data-ttu-id="272cf-145">**DataSetName:** SampleData</span><span class="sxs-lookup"><span data-stu-id="272cf-145">**DataSetName:** SampleData</span></span>
* <span data-ttu-id="272cf-146">**CreationDate:** 10/1/2013</span><span class="sxs-lookup"><span data-stu-id="272cf-146">**CreationDate:** 10/1/2013</span></span>

<span data-ttu-id="272cf-147">tooset metadata för hello importeras filer, skapa en textfil `c:\WAImportExport\SampleMetadata.txt`, med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="272cf-147">tooset metadata for hello imported files, create a text file, `c:\WAImportExport\SampleMetadata.txt`, with hello following content:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

<span data-ttu-id="272cf-148">Du kan också ange vissa egenskaper för hello `FavoriteMovie.ISO` blob:</span><span class="sxs-lookup"><span data-stu-id="272cf-148">You can also set some properties for hello `FavoriteMovie.ISO` blob:</span></span>

* <span data-ttu-id="272cf-149">**Content-Type:** program/oktett-ström</span><span class="sxs-lookup"><span data-stu-id="272cf-149">**Content-Type:** application/octet-stream</span></span>
* <span data-ttu-id="272cf-150">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==</span><span class="sxs-lookup"><span data-stu-id="272cf-150">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==</span></span>
* <span data-ttu-id="272cf-151">**Cache-Control:** no-cache</span><span class="sxs-lookup"><span data-stu-id="272cf-151">**Cache-Control:** no-cache</span></span>

<span data-ttu-id="272cf-152">tooset dessa egenskaper, skapa en textfil `c:\WAImportExport\SampleProperties.txt`:</span><span class="sxs-lookup"><span data-stu-id="272cf-152">tooset these properties, create a text file, `c:\WAImportExport\SampleProperties.txt`:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

## <a name="run-hello-azure-importexport-tool-waimportexportexe"></a><span data-ttu-id="272cf-153">Kör hello Azure Import/Export-verktyget (WAImportExport.exe)</span><span class="sxs-lookup"><span data-stu-id="272cf-153">Run hello Azure Import/Export Tool (WAImportExport.exe)</span></span>

<span data-ttu-id="272cf-154">Nu är du redo toorun hello Azure Import/Export verktyget tooprepare hello två hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="272cf-154">Now you are ready toorun hello Azure Import/Export Tool tooprepare hello two hard drives.</span></span>

<span data-ttu-id="272cf-155">**För hello första sessionen:**</span><span class="sxs-lookup"><span data-stu-id="272cf-155">**For hello first session:**</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

<span data-ttu-id="272cf-156">Om fler data måste toobe som lagts till, skapar du en annan dataset-fil (samma format som Initialdataset).</span><span class="sxs-lookup"><span data-stu-id="272cf-156">If any more data needs toobe added, create another dataset file (same format as Initialdataset).</span></span>

<span data-ttu-id="272cf-157">**För hello andra sessionen:**</span><span class="sxs-lookup"><span data-stu-id="272cf-157">**For hello second session:**</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

<span data-ttu-id="272cf-158">När hello kopiera sessioner har slutfört kan du koppla hello två enheter från hello kopiera dator och leverera dem toohello lämplig Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="272cf-158">Once hello copy sessions have completed, you can disconnect hello two drives from hello copy computer and ship them toohello appropriate Azure data center.</span></span> <span data-ttu-id="272cf-159">Du måste överföra hello två journalfiler `<FirstDriveSerialNumber>.xml` och `<SecondDriveSerialNumber>.xml`, när du skapar hello importjobb i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="272cf-159">You'll upload hello two journal files, `<FirstDriveSerialNumber>.xml` and `<SecondDriveSerialNumber>.xml`, when you create hello import job in hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="272cf-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="272cf-160">Next steps</span></span>

* [<span data-ttu-id="272cf-161">Förbereda hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="272cf-161">Preparing hard drives for an import job</span></span>](../storage-import-export-tool-preparing-hard-drives-import.md)
* [<span data-ttu-id="272cf-162">Snabbreferens för vanliga kommandon</span><span class="sxs-lookup"><span data-stu-id="272cf-162">Quick reference for frequently used commands</span></span>](../storage-import-export-tool-quick-reference.md)
