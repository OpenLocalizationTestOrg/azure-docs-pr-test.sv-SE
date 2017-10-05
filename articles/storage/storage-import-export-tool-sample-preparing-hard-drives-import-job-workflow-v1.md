---
title: "Exempelarbetsflöde till Förbered dig hårddiskar för ett Azure Import/Export-importjobb - v1 | Microsoft Docs"
description: "Se en genomgång för fullständig processen med att förbereda enheter för ett importjobb i tjänsten Azure Import/Export."
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
ms.openlocfilehash: 313f8c1f3962a943b4c98c530c324ff28aa84c10
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a><span data-ttu-id="91de2-103">Exempelarbetsflöde för att förbereda hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="91de2-103">Sample workflow to prepare hard drives for an import job</span></span>
<span data-ttu-id="91de2-104">Det här avsnittet vägleder dig genom processen förbereder enheter för importen.</span><span class="sxs-lookup"><span data-stu-id="91de2-104">This topic walks you through the complete process of preparing drives for an import job.</span></span>  
  
<span data-ttu-id="91de2-105">Det här exemplet importerar följande data till en Windows Azure-lagringskonto med namnet `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="91de2-105">This example imports the following data into a Window Azure storage account named `mystorageaccount`:</span></span>  
  
|<span data-ttu-id="91de2-106">Plats</span><span class="sxs-lookup"><span data-stu-id="91de2-106">Location</span></span>|<span data-ttu-id="91de2-107">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="91de2-107">Description</span></span>|  
|--------------|-----------------|  
|<span data-ttu-id="91de2-108">H:\Video</span><span class="sxs-lookup"><span data-stu-id="91de2-108">H:\Video</span></span>|<span data-ttu-id="91de2-109">En samling videor, 5 TB totalt.</span><span class="sxs-lookup"><span data-stu-id="91de2-109">A collection of videos, 5 TB in total.</span></span>|  
|<span data-ttu-id="91de2-110">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="91de2-110">H:\Photo</span></span>|<span data-ttu-id="91de2-111">En samling foton, 30 GB totalt.</span><span class="sxs-lookup"><span data-stu-id="91de2-111">A collection of photos, 30 GB in total.</span></span>|  
|<span data-ttu-id="91de2-112">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="91de2-112">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="91de2-113">A Blu-ray™ diskavbildning, 25 GB.</span><span class="sxs-lookup"><span data-stu-id="91de2-113">A Blu-Ray™ disk image, 25 GB.</span></span>|  
|<span data-ttu-id="91de2-114">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="91de2-114">\\\bigshare\john\music</span></span>|<span data-ttu-id="91de2-115">En samling av musik på en nätverksresurs, 10 GB totalt.</span><span class="sxs-lookup"><span data-stu-id="91de2-115">A collection of music files on a network share, 10 GB in total.</span></span>|  
  
<span data-ttu-id="91de2-116">Importjobbet importerar data till med följande mål i storage-konto:</span><span class="sxs-lookup"><span data-stu-id="91de2-116">The import job will import this data into the following destinations in the storage account:</span></span>  
  
|<span data-ttu-id="91de2-117">Källa</span><span class="sxs-lookup"><span data-stu-id="91de2-117">Source</span></span>|<span data-ttu-id="91de2-118">Mål virtuell katalog eller blob</span><span class="sxs-lookup"><span data-stu-id="91de2-118">Destination virtual directory or blob</span></span>|  
|------------|-------------------------------------------|  
|<span data-ttu-id="91de2-119">H:\Video</span><span class="sxs-lookup"><span data-stu-id="91de2-119">H:\Video</span></span>|<span data-ttu-id="91de2-120">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="91de2-120">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="91de2-121">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="91de2-121">H:\Photo</span></span>|<span data-ttu-id="91de2-122">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="91de2-122">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="91de2-123">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="91de2-123">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="91de2-124">https://mystorageaccount.BLOB.Core.Windows.NET/favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="91de2-124">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="91de2-125">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="91de2-125">\\\bigshare\john\music</span></span>|<span data-ttu-id="91de2-126">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="91de2-126">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
<span data-ttu-id="91de2-127">Med den här mappningen filen `H:\Video\Drama\GreatMovie.mov` ska importeras till blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span><span class="sxs-lookup"><span data-stu-id="91de2-127">With this mapping, the file `H:\Video\Drama\GreatMovie.mov` will be imported to the blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span></span>  
  
<span data-ttu-id="91de2-128">Sedan beräkna storleken på data för att avgöra hur många hårddiskar behövs:</span><span class="sxs-lookup"><span data-stu-id="91de2-128">Next, to determine how many hard drives are needed, compute the size of the data:</span></span>  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
<span data-ttu-id="91de2-129">I det här exemplet är två 3TB-hårddiskar tillräckliga.</span><span class="sxs-lookup"><span data-stu-id="91de2-129">For this example, two 3TB hard drives should be sufficient.</span></span> <span data-ttu-id="91de2-130">Eftersom källkatalogen `H:\Video` har 5TB data och den enda hårddisken kapacitet är 3TB, är det nödvändigt att bryta `H:\Video` i två mindre kataloger innan du kör verktyget Microsoft Azure Import/Export: `H:\Video1` och `H:\Video2`.</span><span class="sxs-lookup"><span data-stu-id="91de2-130">However, since the source directory `H:\Video` has 5TB of data and your single hard drive's capacity is only 3TB, it's necessary to break `H:\Video` into two smaller directories before running the Microsoft Azure Import/Export Tool: `H:\Video1` and `H:\Video2`.</span></span> <span data-ttu-id="91de2-131">Det här steget ger följande källa kataloger:</span><span class="sxs-lookup"><span data-stu-id="91de2-131">This step yields the following source directories:</span></span>  
  
|<span data-ttu-id="91de2-132">Plats</span><span class="sxs-lookup"><span data-stu-id="91de2-132">Location</span></span>|<span data-ttu-id="91de2-133">Storlek</span><span class="sxs-lookup"><span data-stu-id="91de2-133">Size</span></span>|<span data-ttu-id="91de2-134">Mål virtuell katalog eller blob</span><span class="sxs-lookup"><span data-stu-id="91de2-134">Destination virtual directory or blob</span></span>|  
|--------------|----------|-------------------------------------------|  
|<span data-ttu-id="91de2-135">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="91de2-135">H:\Video1</span></span>|<span data-ttu-id="91de2-136">2.5 TB</span><span class="sxs-lookup"><span data-stu-id="91de2-136">2.5TB</span></span>|<span data-ttu-id="91de2-137">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="91de2-137">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="91de2-138">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="91de2-138">H:\Video2</span></span>|<span data-ttu-id="91de2-139">2.5 TB</span><span class="sxs-lookup"><span data-stu-id="91de2-139">2.5TB</span></span>|<span data-ttu-id="91de2-140">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="91de2-140">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="91de2-141">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="91de2-141">H:\Photo</span></span>|<span data-ttu-id="91de2-142">30 GB</span><span class="sxs-lookup"><span data-stu-id="91de2-142">30GB</span></span>|<span data-ttu-id="91de2-143">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="91de2-143">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="91de2-144">K:\Temp\FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="91de2-144">K:\Temp\FavoriteMovies.ISO</span></span>|<span data-ttu-id="91de2-145">25 GB</span><span class="sxs-lookup"><span data-stu-id="91de2-145">25GB</span></span>|<span data-ttu-id="91de2-146">https://mystorageaccount.BLOB.Core.Windows.NET/favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="91de2-146">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="91de2-147">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="91de2-147">\\\bigshare\john\music</span></span>|<span data-ttu-id="91de2-148">10GB</span><span class="sxs-lookup"><span data-stu-id="91de2-148">10GB</span></span>|<span data-ttu-id="91de2-149">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="91de2-149">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
 <span data-ttu-id="91de2-150">Observera att även om den `H:\Video`directory har delats till två kataloger som de pekar på samma virtuella målkatalogen i storage-konto.</span><span class="sxs-lookup"><span data-stu-id="91de2-150">Note that even though the `H:\Video`directory has been split to two directories, they point to the same destination virtual directory in the storage account.</span></span> <span data-ttu-id="91de2-151">På så sätt kan alla videofiler bevaras under en enda `video` behållare i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="91de2-151">This way, all video files are maintained under a single `video` container in the storage account.</span></span>  
  
 <span data-ttu-id="91de2-152">Därefter fördelas ovan källa kataloger jämnt till två hårddiskar:</span><span class="sxs-lookup"><span data-stu-id="91de2-152">Next, the above source directories are evenly distributed to the two hard drives:</span></span>  
  
||||  
|-|-|-|  
|<span data-ttu-id="91de2-153">Hårddisk</span><span class="sxs-lookup"><span data-stu-id="91de2-153">Hard drive</span></span>|<span data-ttu-id="91de2-154">Källan</span><span class="sxs-lookup"><span data-stu-id="91de2-154">Source directories</span></span>|<span data-ttu-id="91de2-155">Total storlek</span><span class="sxs-lookup"><span data-stu-id="91de2-155">Total size</span></span>|  
|<span data-ttu-id="91de2-156">Första enhet</span><span class="sxs-lookup"><span data-stu-id="91de2-156">First Drive</span></span>|<span data-ttu-id="91de2-157">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="91de2-157">H:\Video1</span></span>|<span data-ttu-id="91de2-158">2,5 TB + 30 GB</span><span class="sxs-lookup"><span data-stu-id="91de2-158">2.5TB + 30GB</span></span>|  
||<span data-ttu-id="91de2-159">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="91de2-159">H:\Photo</span></span>||  
|<span data-ttu-id="91de2-160">Andra enhet</span><span class="sxs-lookup"><span data-stu-id="91de2-160">Second Drive</span></span>|<span data-ttu-id="91de2-161">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="91de2-161">H:\Video2</span></span>|<span data-ttu-id="91de2-162">2,5 TB + 35 GB</span><span class="sxs-lookup"><span data-stu-id="91de2-162">2.5TB + 35GB</span></span>|  
||<span data-ttu-id="91de2-163">K:\Temp\BlueRay.ISO</span><span class="sxs-lookup"><span data-stu-id="91de2-163">K:\Temp\BlueRay.ISO</span></span>||  
||<span data-ttu-id="91de2-164">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="91de2-164">\\\bigshare\john\music</span></span>||  
  
<span data-ttu-id="91de2-165">Dessutom kan du ange följande metadata för alla filer:</span><span class="sxs-lookup"><span data-stu-id="91de2-165">In addition, you can set the following metadata for all files:</span></span>  
  
-   <span data-ttu-id="91de2-166">**UploadMethod:** Windows Azure Import/Export service</span><span class="sxs-lookup"><span data-stu-id="91de2-166">**UploadMethod:** Windows Azure Import/Export service</span></span>  
  
-   <span data-ttu-id="91de2-167">**DataSetName:** SampleData</span><span class="sxs-lookup"><span data-stu-id="91de2-167">**DataSetName:** SampleData</span></span>  
  
-   <span data-ttu-id="91de2-168">**CreationDate:** 10/1/2013</span><span class="sxs-lookup"><span data-stu-id="91de2-168">**CreationDate:** 10/1/2013</span></span>  
  
<span data-ttu-id="91de2-169">Skapa en textfil, om du vill ange metadata för de importerade filerna `c:\WAImportExport\SampleMetadata.txt`, med följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="91de2-169">To set metadata for the imported files, create a text file, `c:\WAImportExport\SampleMetadata.txt`, with the following content:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
<span data-ttu-id="91de2-170">Du kan också ange vissa egenskaper för den `FavoriteMovie.ISO` blob:</span><span class="sxs-lookup"><span data-stu-id="91de2-170">You can also set some properties for the `FavoriteMovie.ISO` blob:</span></span>  
  
-   <span data-ttu-id="91de2-171">**Content-Type:** program/oktett-ström</span><span class="sxs-lookup"><span data-stu-id="91de2-171">**Content-Type:** application/octet-stream</span></span>  
  
-   <span data-ttu-id="91de2-172">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==</span><span class="sxs-lookup"><span data-stu-id="91de2-172">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==</span></span>  
  
-   <span data-ttu-id="91de2-173">**Cache-Control:** no-cache</span><span class="sxs-lookup"><span data-stu-id="91de2-173">**Cache-Control:** no-cache</span></span>  
  
<span data-ttu-id="91de2-174">Att ange dessa egenskaper, skapa en textfil `c:\WAImportExport\SampleProperties.txt`:</span><span class="sxs-lookup"><span data-stu-id="91de2-174">To set these properties, create a text file, `c:\WAImportExport\SampleProperties.txt`:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
<span data-ttu-id="91de2-175">Nu är du redo att köra verktyget Azure Import/Export för att förbereda två hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="91de2-175">Now you are ready to run the Azure Import/Export Tool to prepare the two hard drives.</span></span> <span data-ttu-id="91de2-176">Tänk på följande:</span><span class="sxs-lookup"><span data-stu-id="91de2-176">Note that:</span></span>  
  
-   <span data-ttu-id="91de2-177">Den första enheten är monterad enhet X.</span><span class="sxs-lookup"><span data-stu-id="91de2-177">The first drive is mounted as drive X.</span></span>  
  
-   <span data-ttu-id="91de2-178">Den andra enheten är monterad enhet Y.</span><span class="sxs-lookup"><span data-stu-id="91de2-178">The second drive is mounted as drive Y.</span></span>  
  
-   <span data-ttu-id="91de2-179">Nyckeln för lagringskontot `mystorageaccount` är `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span><span class="sxs-lookup"><span data-stu-id="91de2-179">The key for the storage account `mystorageaccount` is `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span></span>  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a><span data-ttu-id="91de2-180">Förbereda disk för import när data har redan lästs in</span><span class="sxs-lookup"><span data-stu-id="91de2-180">Preparing disk for import when data is pre-loaded</span></span>
 
 <span data-ttu-id="91de2-181">Om data som ska importeras finns redan på disken, använder du flaggan /skipwrite.</span><span class="sxs-lookup"><span data-stu-id="91de2-181">If the data to be imported is already present on the disk, use the flag /skipwrite.</span></span> <span data-ttu-id="91de2-182">Värdet för /t och /srcdir både måste peka på den disk som förbereds för import.</span><span class="sxs-lookup"><span data-stu-id="91de2-182">Value of /t and /srcdir both should point to the disk being prepared for import.</span></span> <span data-ttu-id="91de2-183">Om inte alla data på disken gå till samma virtuella målkatalogen eller roten för storage-konto måste köra samma kommando för varje katalog separat som värdet för /id samma över alla körs.</span><span class="sxs-lookup"><span data-stu-id="91de2-183">If not all the data on the disk needs to go to the same destination virtual directory or root of the storage account, run the same command for each directory separately keeping the value of /id same across all runs.</span></span>

>[!NOTE] 
><span data-ttu-id="91de2-184">Ange inte/format som den kommer att radera data på disken.</span><span class="sxs-lookup"><span data-stu-id="91de2-184">Do not specify /format as it will wipe the data on the disk.</span></span> <span data-ttu-id="91de2-185">Du kan ange / kryptera eller /bk beroende på om disken redan är krypterad eller inte.</span><span class="sxs-lookup"><span data-stu-id="91de2-185">You can specify /encrypt or /bk depending on whether the disk is already encrypted or not.</span></span> 
>

```
    When data is already present on the disk for each drive run the following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a><span data-ttu-id="91de2-186">Kopiera sessioner - enheten först</span><span class="sxs-lookup"><span data-stu-id="91de2-186">Copy sessions - first drive</span></span>

<span data-ttu-id="91de2-187">Kör verktyget Azure Import/Export två gånger för att kopiera kataloger för två källa för den första enheten:</span><span class="sxs-lookup"><span data-stu-id="91de2-187">For the first drive, run the Azure Import/Export Tool twice to copy the two source directories:</span></span>  

<span data-ttu-id="91de2-188">**Först kopiera session**</span><span class="sxs-lookup"><span data-stu-id="91de2-188">**First copy session**</span></span>
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

<span data-ttu-id="91de2-189">**Den andra kopian sessionen**</span><span class="sxs-lookup"><span data-stu-id="91de2-189">**Second copy session**</span></span>

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a><span data-ttu-id="91de2-190">Kopiera sessioner - andra enhet</span><span class="sxs-lookup"><span data-stu-id="91de2-190">Copy sessions - second drive</span></span>
 
<span data-ttu-id="91de2-191">Den andra enheten kör du verktyget Azure Import/Export tre gånger, en gång var och en för käll-kataloger och en gång för avbildningsfilen fristående Blu-Ray™):</span><span class="sxs-lookup"><span data-stu-id="91de2-191">For the second drive, run the Azure Import/Export Tool three times, once each for the source directories, and once for the standalone Blu-Ray™ image file):</span></span>  
  
<span data-ttu-id="91de2-192">**Först kopiera session**</span><span class="sxs-lookup"><span data-stu-id="91de2-192">**First copy session**</span></span> 

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Video2 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:y /format /encrypt /srcdir:H:\Video2 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```
  
<span data-ttu-id="91de2-193">**Den andra kopian sessionen**</span><span class="sxs-lookup"><span data-stu-id="91de2-193">**Second copy session**</span></span>

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Music /srcdir:\\bigshare\john\music /dstdir:music/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```  
  
<span data-ttu-id="91de2-194">**Den tredje kopia session**</span><span class="sxs-lookup"><span data-stu-id="91de2-194">**Third copy session**</span></span>  

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```

## <a name="copy-session-completion"></a><span data-ttu-id="91de2-195">Kopiera session slutförande</span><span class="sxs-lookup"><span data-stu-id="91de2-195">Copy session completion</span></span>

<span data-ttu-id="91de2-196">När kopiera sessioner har slutfört du koppla de två enheterna från datorn som kopia och skicka dem till lämpliga Windows Azure-datacentret.</span><span class="sxs-lookup"><span data-stu-id="91de2-196">Once the copy sessions have completed, you can disconnect the two drives from the copy computer and ship them to the appropriate Windows Azure data center.</span></span> <span data-ttu-id="91de2-197">Du måste ladda upp två journalfiler `FirstDrive.jrn` och `SecondDrive.jrn`när du skapar importjobbet i den [Windows Azure Management Portal](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="91de2-197">You'll upload the two journal files, `FirstDrive.jrn` and `SecondDrive.jrn`, when you create the import job in the [Windows Azure Management Portal](https://manage.windowsazure.com/).</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="91de2-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91de2-198">Next steps</span></span>

* [<span data-ttu-id="91de2-199">Förbereda hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="91de2-199">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="91de2-200">Snabbreferens för vanliga kommandon</span><span class="sxs-lookup"><span data-stu-id="91de2-200">Quick reference for frequently used commands</span></span>](storage-import-export-tool-quick-reference-v1.md) 
