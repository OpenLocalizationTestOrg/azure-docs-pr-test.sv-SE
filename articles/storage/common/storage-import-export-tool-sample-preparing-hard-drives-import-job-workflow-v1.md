---
title: "aaaSample arbetsflöde tooprep hårddiskar för Azure Import/Export-importjobb - v1 | Microsoft Docs"
description: "Se en genomgång för hello Slutför process förbereder enheter för ett importjobb i hello Azure Import/Export service."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 6eb1b1b7-c69f-4365-b5ef-3cd5e05eb72a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: eb77831a88c16c14838179a6432ddb06503067dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sample-workflow-tooprepare-hard-drives-for-an-import-job"></a><span data-ttu-id="46467-103">Exempel arbetsflödet tooprepare hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="46467-103">Sample workflow tooprepare hard drives for an import job</span></span>
<span data-ttu-id="46467-104">Det här avsnittet vägleder dig genom hello Slutför process förbereder enheter för importen.</span><span class="sxs-lookup"><span data-stu-id="46467-104">This topic walks you through hello complete process of preparing drives for an import job.</span></span>  
  
<span data-ttu-id="46467-105">Det här exemplet importerar hello följande data till en Windows Azure storage-konto med namnet `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="46467-105">This example imports hello following data into a Window Azure storage account named `mystorageaccount`:</span></span>  
  
|<span data-ttu-id="46467-106">Plats</span><span class="sxs-lookup"><span data-stu-id="46467-106">Location</span></span>|<span data-ttu-id="46467-107">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46467-107">Description</span></span>|  
|--------------|-----------------|  
|<span data-ttu-id="46467-108">H:\Video</span><span class="sxs-lookup"><span data-stu-id="46467-108">H:\Video</span></span>|<span data-ttu-id="46467-109">En samling videor, 5 TB totalt.</span><span class="sxs-lookup"><span data-stu-id="46467-109">A collection of videos, 5 TB in total.</span></span>|  
|<span data-ttu-id="46467-110">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="46467-110">H:\Photo</span></span>|<span data-ttu-id="46467-111">En samling foton, 30 GB totalt.</span><span class="sxs-lookup"><span data-stu-id="46467-111">A collection of photos, 30 GB in total.</span></span>|  
|<span data-ttu-id="46467-112">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="46467-112">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="46467-113">A Blu-ray™ diskavbildning, 25 GB.</span><span class="sxs-lookup"><span data-stu-id="46467-113">A Blu-Ray™ disk image, 25 GB.</span></span>|  
|<span data-ttu-id="46467-114">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="46467-114">\\\bigshare\john\music</span></span>|<span data-ttu-id="46467-115">En samling av musik på en nätverksresurs, 10 GB totalt.</span><span class="sxs-lookup"><span data-stu-id="46467-115">A collection of music files on a network share, 10 GB in total.</span></span>|  
  
<span data-ttu-id="46467-116">hello importjobbet importerar data till hello följande mål i hello storage-konto:</span><span class="sxs-lookup"><span data-stu-id="46467-116">hello import job imports this data into hello following destinations in hello storage account:</span></span>  
  
|<span data-ttu-id="46467-117">Källa</span><span class="sxs-lookup"><span data-stu-id="46467-117">Source</span></span>|<span data-ttu-id="46467-118">Mål virtuell katalog eller blob</span><span class="sxs-lookup"><span data-stu-id="46467-118">Destination virtual directory or blob</span></span>|  
|------------|-------------------------------------------|  
|<span data-ttu-id="46467-119">H:\Video</span><span class="sxs-lookup"><span data-stu-id="46467-119">H:\Video</span></span>|<span data-ttu-id="46467-120">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="46467-120">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="46467-121">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="46467-121">H:\Photo</span></span>|<span data-ttu-id="46467-122">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="46467-122">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="46467-123">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="46467-123">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="46467-124">https://mystorageaccount.BLOB.Core.Windows.NET/favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="46467-124">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="46467-125">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="46467-125">\\\bigshare\john\music</span></span>|<span data-ttu-id="46467-126">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="46467-126">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
<span data-ttu-id="46467-127">Med den här mappningen hello filen `H:\Video\Drama\GreatMovie.mov` är importerade toohello blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span><span class="sxs-lookup"><span data-stu-id="46467-127">With this mapping, hello file `H:\Video\Drama\GreatMovie.mov` is imported toohello blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span></span>  
  
<span data-ttu-id="46467-128">Nästa toodetermine hur många hårddiskar krävs beräkning hello storleken på hello data:</span><span class="sxs-lookup"><span data-stu-id="46467-128">Next, toodetermine how many hard drives are needed, compute hello size of hello data:</span></span>  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
<span data-ttu-id="46467-129">I det här exemplet är två 3 TB-hårddiskar tillräckliga.</span><span class="sxs-lookup"><span data-stu-id="46467-129">For this example, two 3-TB hard drives should be sufficient.</span></span> <span data-ttu-id="46467-130">Eftersom hello källkatalog `H:\Video` har 5 TB data och den enda hårddisken kapacitet är 3 TB, är det nödvändigt toobreak `H:\Video` i två mindre kataloger: `H:\Video1` och `H:\Video2`innan körs hello Microsoft Verktyget Azure Import/Export.</span><span class="sxs-lookup"><span data-stu-id="46467-130">However, since hello source directory `H:\Video` has 5 TB of data and your single hard drive's capacity is only 3 TB, it's necessary toobreak `H:\Video` into two smaller directories: `H:\Video1` and `H:\Video2`, before running hello Microsoft Azure Import/Export Tool.</span></span> <span data-ttu-id="46467-131">Det här steget ger hello efter källan:</span><span class="sxs-lookup"><span data-stu-id="46467-131">This step yields hello following source directories:</span></span>  
  
|<span data-ttu-id="46467-132">Plats</span><span class="sxs-lookup"><span data-stu-id="46467-132">Location</span></span>|<span data-ttu-id="46467-133">Storlek</span><span class="sxs-lookup"><span data-stu-id="46467-133">Size</span></span>|<span data-ttu-id="46467-134">Mål virtuell katalog eller blob</span><span class="sxs-lookup"><span data-stu-id="46467-134">Destination virtual directory or blob</span></span>|  
|--------------|----------|-------------------------------------------|  
|<span data-ttu-id="46467-135">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="46467-135">H:\Video1</span></span>|<span data-ttu-id="46467-136">2,5 TB</span><span class="sxs-lookup"><span data-stu-id="46467-136">2.5 TB</span></span>|<span data-ttu-id="46467-137">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="46467-137">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="46467-138">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="46467-138">H:\Video2</span></span>|<span data-ttu-id="46467-139">2,5 TB</span><span class="sxs-lookup"><span data-stu-id="46467-139">2.5 TB</span></span>|<span data-ttu-id="46467-140">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="46467-140">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="46467-141">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="46467-141">H:\Photo</span></span>|<span data-ttu-id="46467-142">30 GB</span><span class="sxs-lookup"><span data-stu-id="46467-142">30 GB</span></span>|<span data-ttu-id="46467-143">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="46467-143">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="46467-144">K:\Temp\FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="46467-144">K:\Temp\FavoriteMovies.ISO</span></span>|<span data-ttu-id="46467-145">25 GB</span><span class="sxs-lookup"><span data-stu-id="46467-145">25 GB</span></span>|<span data-ttu-id="46467-146">https://mystorageaccount.BLOB.Core.Windows.NET/favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="46467-146">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="46467-147">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="46467-147">\\\bigshare\john\music</span></span>|<span data-ttu-id="46467-148">10 GB</span><span class="sxs-lookup"><span data-stu-id="46467-148">10 GB</span></span>|<span data-ttu-id="46467-149">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="46467-149">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
 <span data-ttu-id="46467-150">Även om hello `H:\Video`directory har delats tootwo kataloger, de peka toohello samma mål virtuell katalog på hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="46467-150">Even though hello `H:\Video`directory has been split tootwo directories, they point toohello same destination virtual directory in hello storage account.</span></span> <span data-ttu-id="46467-151">På så sätt kan alla videofiler bevaras under en enda `video` behållare i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="46467-151">This way, all video files are maintained under a single `video` container in hello storage account.</span></span>  
  
 <span data-ttu-id="46467-152">Därefter är hello tidigare källa kataloger jämnt fördelade toohello två hårddiskar:</span><span class="sxs-lookup"><span data-stu-id="46467-152">Next, hello previous source directories are evenly distributed toohello two hard drives:</span></span>  
  
||||  
|-|-|-|  
|<span data-ttu-id="46467-153">Hårddisk</span><span class="sxs-lookup"><span data-stu-id="46467-153">Hard drive</span></span>|<span data-ttu-id="46467-154">Källan</span><span class="sxs-lookup"><span data-stu-id="46467-154">Source directories</span></span>|<span data-ttu-id="46467-155">Total storlek</span><span class="sxs-lookup"><span data-stu-id="46467-155">Total size</span></span>|  
|<span data-ttu-id="46467-156">Första enhet</span><span class="sxs-lookup"><span data-stu-id="46467-156">First Drive</span></span>|<span data-ttu-id="46467-157">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="46467-157">H:\Video1</span></span>|<span data-ttu-id="46467-158">2,5 TB + 30 GB</span><span class="sxs-lookup"><span data-stu-id="46467-158">2.5 TB + 30 GB</span></span>|  
||<span data-ttu-id="46467-159">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="46467-159">H:\Photo</span></span>||  
|<span data-ttu-id="46467-160">Andra enhet</span><span class="sxs-lookup"><span data-stu-id="46467-160">Second Drive</span></span>|<span data-ttu-id="46467-161">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="46467-161">H:\Video2</span></span>|<span data-ttu-id="46467-162">2,5 TB + 35 GB</span><span class="sxs-lookup"><span data-stu-id="46467-162">2.5 TB + 35 GB</span></span>|  
||<span data-ttu-id="46467-163">K:\Temp\BlueRay.ISO</span><span class="sxs-lookup"><span data-stu-id="46467-163">K:\Temp\BlueRay.ISO</span></span>||  
||<span data-ttu-id="46467-164">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="46467-164">\\\bigshare\john\music</span></span>||  
  
<span data-ttu-id="46467-165">Du kan dessutom ange hello efter metadata för alla filer:</span><span class="sxs-lookup"><span data-stu-id="46467-165">In addition, you can set hello following metadata for all files:</span></span>  
  
-   <span data-ttu-id="46467-166">**UploadMethod:** Windows Azure Import/Export service</span><span class="sxs-lookup"><span data-stu-id="46467-166">**UploadMethod:** Windows Azure Import/Export service</span></span>  
  
-   <span data-ttu-id="46467-167">**DataSetName:** SampleData</span><span class="sxs-lookup"><span data-stu-id="46467-167">**DataSetName:** SampleData</span></span>  
  
-   <span data-ttu-id="46467-168">**CreationDate:** 10/1/2013</span><span class="sxs-lookup"><span data-stu-id="46467-168">**CreationDate:** 10/1/2013</span></span>  
  
<span data-ttu-id="46467-169">tooset metadata för hello importeras filer, skapa en textfil `c:\WAImportExport\SampleMetadata.txt`, med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="46467-169">tooset metadata for hello imported files, create a text file, `c:\WAImportExport\SampleMetadata.txt`, with hello following content:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
<span data-ttu-id="46467-170">Du kan också ange vissa egenskaper för hello `FavoriteMovie.ISO` blob:</span><span class="sxs-lookup"><span data-stu-id="46467-170">You can also set some properties for hello `FavoriteMovie.ISO` blob:</span></span>  
  
-   <span data-ttu-id="46467-171">**Content-Type:** program/oktett-ström</span><span class="sxs-lookup"><span data-stu-id="46467-171">**Content-Type:** application/octet-stream</span></span>  
  
-   <span data-ttu-id="46467-172">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==</span><span class="sxs-lookup"><span data-stu-id="46467-172">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==</span></span>  
  
-   <span data-ttu-id="46467-173">**Cache-Control:** no-cache</span><span class="sxs-lookup"><span data-stu-id="46467-173">**Cache-Control:** no-cache</span></span>  
  
<span data-ttu-id="46467-174">tooset dessa egenskaper, skapa en textfil `c:\WAImportExport\SampleProperties.txt`:</span><span class="sxs-lookup"><span data-stu-id="46467-174">tooset these properties, create a text file, `c:\WAImportExport\SampleProperties.txt`:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
<span data-ttu-id="46467-175">Nu är du redo toorun hello Azure Import/Export verktyget tooprepare hello två hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="46467-175">Now you are ready toorun hello Azure Import/Export Tool tooprepare hello two hard drives.</span></span> <span data-ttu-id="46467-176">Tänk på följande:</span><span class="sxs-lookup"><span data-stu-id="46467-176">Note that:</span></span>  
  
-   <span data-ttu-id="46467-177">hello första enheten är monterad enhet X.</span><span class="sxs-lookup"><span data-stu-id="46467-177">hello first drive is mounted as drive X.</span></span>  
  
-   <span data-ttu-id="46467-178">andra hello-enhet är monterad enhet Y.</span><span class="sxs-lookup"><span data-stu-id="46467-178">hello second drive is mounted as drive Y.</span></span>  
  
-   <span data-ttu-id="46467-179">hello nyckel för hello lagringskonto `mystorageaccount` är `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span><span class="sxs-lookup"><span data-stu-id="46467-179">hello key for hello storage account `mystorageaccount` is `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span></span>  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a><span data-ttu-id="46467-180">Förbereda disk för import när data har redan lästs in</span><span class="sxs-lookup"><span data-stu-id="46467-180">Preparing disk for import when data is pre-loaded</span></span>
 
 <span data-ttu-id="46467-181">Om det redan finns på disken hello hello data toobe importeras, Använd hello flaggan /skipwrite.</span><span class="sxs-lookup"><span data-stu-id="46467-181">If hello data toobe imported is already present on hello disk, use hello flag /skipwrite.</span></span> <span data-ttu-id="46467-182">hello-värdet för /t och /srcdir ska båda punkt toohello disk förbereds för import.</span><span class="sxs-lookup"><span data-stu-id="46467-182">hello value of /t and /srcdir should both point toohello disk being prepared for import.</span></span> <span data-ttu-id="46467-183">Om alla hello data toobe importeras inte ska toohello samma mål virtuell katalog eller roten för hello storage-konto, kör hello samma kommando för varje målkatalogen separat, hålla hello värdet för /id hello samma över alla körs.</span><span class="sxs-lookup"><span data-stu-id="46467-183">If all of hello data toobe imported is not going toohello same destination virtual directory or root of hello storage account, run hello same command for each destination directory separately, keeping hello value of /id hello same across all runs.</span></span>

>[!NOTE] 
><span data-ttu-id="46467-184">Ange inte/format som den kommer att radera hello data på hello disk.</span><span class="sxs-lookup"><span data-stu-id="46467-184">Do not specify /format as it will wipe hello data on hello disk.</span></span> <span data-ttu-id="46467-185">Du kan ange / kryptera eller /bk beroende på om hello disken redan är krypterad eller inte.</span><span class="sxs-lookup"><span data-stu-id="46467-185">You can specify /encrypt or /bk depending on whether hello disk is already encrypted or not.</span></span> 
>

```
    When data is already present on hello disk for each drive run hello following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a><span data-ttu-id="46467-186">Kopiera sessioner - enheten först</span><span class="sxs-lookup"><span data-stu-id="46467-186">Copy sessions - first drive</span></span>

<span data-ttu-id="46467-187">Första hello-enheten kör hello Azure Import/Export-verktyget datakällan två gånger toocopy hello två kataloger:</span><span class="sxs-lookup"><span data-stu-id="46467-187">For hello first drive, run hello Azure Import/Export Tool twice toocopy hello two source directories:</span></span>  

<span data-ttu-id="46467-188">**Först kopiera session**</span><span class="sxs-lookup"><span data-stu-id="46467-188">**First copy session**</span></span>
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

<span data-ttu-id="46467-189">**Den andra kopian sessionen**</span><span class="sxs-lookup"><span data-stu-id="46467-189">**Second copy session**</span></span>

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a><span data-ttu-id="46467-190">Kopiera sessioner - andra enhet</span><span class="sxs-lookup"><span data-stu-id="46467-190">Copy sessions - second drive</span></span>
 
<span data-ttu-id="46467-191">För hello andra enhet som kör hello Azure Import/Export verktyget tre gånger när varje hello datakälla kataloger och en gång för hello fristående Blu-Ray™ bildfilen):</span><span class="sxs-lookup"><span data-stu-id="46467-191">For hello second drive, run hello Azure Import/Export Tool three times, once each for hello source directories, and once for hello standalone Blu-Ray™ image file):</span></span>  
  
<span data-ttu-id="46467-192">**Först kopiera session**</span><span class="sxs-lookup"><span data-stu-id="46467-192">**First copy session**</span></span> 

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Video2 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:y /format /encrypt /srcdir:H:\Video2 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```
  
<span data-ttu-id="46467-193">**Den andra kopian sessionen**</span><span class="sxs-lookup"><span data-stu-id="46467-193">**Second copy session**</span></span>

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Music /srcdir:\\bigshare\john\music /dstdir:music/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```  
  
<span data-ttu-id="46467-194">**Den tredje kopia session**</span><span class="sxs-lookup"><span data-stu-id="46467-194">**Third copy session**</span></span>  

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```

## <a name="copy-session-completion"></a><span data-ttu-id="46467-195">Kopiera session slutförande</span><span class="sxs-lookup"><span data-stu-id="46467-195">Copy session completion</span></span>

<span data-ttu-id="46467-196">När hello kopiera sessioner har slutfört kan du koppla hello två enheter från hello kopiera dator och leverera dem toohello lämplig Windows Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="46467-196">Once hello copy sessions have completed, you can disconnect hello two drives from hello copy computer and ship them toohello appropriate Windows Azure data center.</span></span> <span data-ttu-id="46467-197">Överför hello två journalfiler `FirstDrive.jrn` och `SecondDrive.jrn`, när du skapar hello importjobb i hello [Windows Azure-portalen](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="46467-197">Upload hello two journal files, `FirstDrive.jrn` and `SecondDrive.jrn`, when you create hello import job in hello [Windows Azure portal](https://manage.windowsazure.com/).</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="46467-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="46467-198">Next steps</span></span>

* [<span data-ttu-id="46467-199">Förbereda hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="46467-199">Preparing hard drives for an import job</span></span>](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="46467-200">Snabbreferens för vanliga kommandon</span><span class="sxs-lookup"><span data-stu-id="46467-200">Quick reference for frequently used commands</span></span>](../storage-import-export-tool-quick-reference-v1.md) 
