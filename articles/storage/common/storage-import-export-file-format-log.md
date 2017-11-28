---
title: Azure Import/Export-loggfilsformatet | Microsoft Docs
description: "Mer information om formatet för loggfilerna som skapades när åtgärder utförs för ett jobb med Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 38cc16bd-ad55-4625-9a85-e1726c35fd1b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 16234ccaf13ce1d85cfd207ed4734e683070faa6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-importexport-service-log-file-format"></a><span data-ttu-id="1c8a9-103">Azure Import/Export service loggfilsformat</span><span class="sxs-lookup"><span data-stu-id="1c8a9-103">Azure Import/Export service log file format</span></span>
<span data-ttu-id="1c8a9-104">När tjänsten Microsoft Azure Import/Export utför en åtgärd på en enhet som en del av ett importjobb eller ett exportjobb skrivs loggarna till blockblobbar i storage-konto som är associerade med jobbet.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-104">When the Microsoft Azure Import/Export service performs an action on a drive as part of an import job or an export job, logs are written to block blobs in the storage account associated with that job.</span></span>  
  
<span data-ttu-id="1c8a9-105">Det finns två loggar som kan skrivas av tjänsten Import/Export:</span><span class="sxs-lookup"><span data-stu-id="1c8a9-105">There are two logs that may be written by the Import/Export service:</span></span>  
  
-   <span data-ttu-id="1c8a9-106">Felloggen skapas alltid i händelse av ett fel.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-106">The error log is always generated in the event of an error.</span></span>  
  
-   <span data-ttu-id="1c8a9-107">Utförlig loggen är inte aktiverad som standard, men kan aktiveras genom att ange den `EnableVerboseLog` egenskapen på en [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) eller [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) igen.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-107">The verbose log is not enabled by default, but may be enabled by setting the `EnableVerboseLog` property on a [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation.</span></span>  
  
## <a name="log-file-location"></a><span data-ttu-id="1c8a9-108">Plats för loggfilen</span><span class="sxs-lookup"><span data-stu-id="1c8a9-108">Log file location</span></span>  
<span data-ttu-id="1c8a9-109">Loggarna skrivs till blockblobbar i behållare eller virtuella katalogen som anges av den `ImportExportStatesPath` inställning som du kan ange för en `Put Job` igen.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-109">The logs are written to block blobs in the container or virtual directory specified by the `ImportExportStatesPath` setting, which you can set on a `Put Job` operation.</span></span> <span data-ttu-id="1c8a9-110">Den plats som loggarna skrivs beror på hur autentisering har angetts för jobbet, tillsammans med det angivna värdet för `ImportExportStatesPath`.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-110">The location to which the logs are written depends on how authentication is specified for the job, together with the value specified for `ImportExportStatesPath`.</span></span> <span data-ttu-id="1c8a9-111">Autentisering för jobbet kan anges via en lagringskontonyckel eller en behållare SAS (signatur för delad åtkomst).</span><span class="sxs-lookup"><span data-stu-id="1c8a9-111">Authentication for the job may be specified via a storage account key, or a container SAS (shared access signature).</span></span>  
  
<span data-ttu-id="1c8a9-112">Namnet på behållaren eller virtuell katalog kan antingen vara standardnamnet `waimportexport`, eller en annan behållare eller en virtuell katalog som du anger.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-112">The name of the container or virtual directory may either be the default name of `waimportexport`, or another container or virtual directory name that you specify.</span></span>  
  
<span data-ttu-id="1c8a9-113">Tabellen nedan visar möjliga alternativ:</span><span class="sxs-lookup"><span data-stu-id="1c8a9-113">The table below shows the possible options:</span></span>  
  
|<span data-ttu-id="1c8a9-114">Autentiseringsmetod</span><span class="sxs-lookup"><span data-stu-id="1c8a9-114">Authentication Method</span></span>|<span data-ttu-id="1c8a9-115">Värdet för `ImportExportStatesPath`Element</span><span class="sxs-lookup"><span data-stu-id="1c8a9-115">Value of `ImportExportStatesPath`Element</span></span>|<span data-ttu-id="1c8a9-116">Plats för logg-BLOB</span><span class="sxs-lookup"><span data-stu-id="1c8a9-116">Location of Log Blobs</span></span>|  
|---------------------------|----------------------------------------------|---------------------------|  
|<span data-ttu-id="1c8a9-117">Lagringskontonyckel</span><span class="sxs-lookup"><span data-stu-id="1c8a9-117">Storage account key</span></span>|<span data-ttu-id="1c8a9-118">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="1c8a9-118">Default value</span></span>|<span data-ttu-id="1c8a9-119">En behållare med namnet `waimportexport`, vilket är standardbehållaren.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-119">A container named `waimportexport`, which is the default container.</span></span> <span data-ttu-id="1c8a9-120">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1c8a9-120">For example:</span></span><br /><br /> `https://myaccount.blob.core.windows.net/waimportexport`|  
|<span data-ttu-id="1c8a9-121">Lagringskontonyckel</span><span class="sxs-lookup"><span data-stu-id="1c8a9-121">Storage account key</span></span>|<span data-ttu-id="1c8a9-122">Värdet som angetts av användaren</span><span class="sxs-lookup"><span data-stu-id="1c8a9-122">User-specified value</span></span>|<span data-ttu-id="1c8a9-123">En behållare med namnet av användaren.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-123">A container named by the user.</span></span> <span data-ttu-id="1c8a9-124">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1c8a9-124">For example:</span></span><br /><br /> `https://myaccount.blob.core.windows.net/mylogcontainer`|  
|<span data-ttu-id="1c8a9-125">Behållaren SAS</span><span class="sxs-lookup"><span data-stu-id="1c8a9-125">Container SAS</span></span>|<span data-ttu-id="1c8a9-126">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="1c8a9-126">Default value</span></span>|<span data-ttu-id="1c8a9-127">En virtuell katalog med namnet `waimportexport`, vilket är standardnamnet under den behållare som angavs i SAS.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-127">A virtual directory named `waimportexport`, which is the default name, beneath the container specified in the SAS.</span></span><br /><br /> <span data-ttu-id="1c8a9-128">Till exempel om SAS som angetts för jobbet är `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, loggplatsen skulle vara`https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`</span><span class="sxs-lookup"><span data-stu-id="1c8a9-128">For example, if the SAS specified for the job is  `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, then the log location would be `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`</span></span>|  
|<span data-ttu-id="1c8a9-129">Behållaren SAS</span><span class="sxs-lookup"><span data-stu-id="1c8a9-129">Container SAS</span></span>|<span data-ttu-id="1c8a9-130">Värdet som angetts av användaren</span><span class="sxs-lookup"><span data-stu-id="1c8a9-130">User-specified value</span></span>|<span data-ttu-id="1c8a9-131">En virtuell katalog med namnet av användaren under den behållare som angavs i SAS.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-131">A virtual directory named by the user, beneath the container specified in the SAS.</span></span><br /><br /> <span data-ttu-id="1c8a9-132">Till exempel om SAS som angetts för jobbet är `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, och den angivna virtuella katalogen heter `mylogblobs`, loggplatsen skulle vara `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-132">For example, if the SAS specified for the job is  `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, and the specified virtual directory is named `mylogblobs`, then the log location would be `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.</span></span>|  
  
<span data-ttu-id="1c8a9-133">Du kan hämta Webbadressen för fel och utförlig loggar genom att anropa den [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) igen.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-133">You can retrieve the URL for the error and verbose logs by calling the [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="1c8a9-134">Loggarna är tillgängliga när bearbetningen av enheten har slutförts.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-134">The logs are available after processing of the drive is complete.</span></span>  
  
## <a name="log-file-format"></a><span data-ttu-id="1c8a9-135">Loggfilsformat</span><span class="sxs-lookup"><span data-stu-id="1c8a9-135">Log file format</span></span>  
<span data-ttu-id="1c8a9-136">Formatet för båda loggarna är samma: en blob som innehåller XML-beskrivningar av de händelser som inträffade vid kopiering av BLOB mellan hårddisken och på kundens konto.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-136">The format for both logs is the same: a blob containing XML descriptions of the events that occurred while copying blobs between the hard drive and the customer's account.</span></span>  
  
<span data-ttu-id="1c8a9-137">Den detaljerade loggen innehåller fullständig information om status för kopieringen för varje blob (för ett importjobb) eller filen (för ett exportjobb), felloggen innehåller endast information för blobbar eller filer som påträffade fel under importen eller Exportera jobb.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-137">The verbose log contains complete information about the status of the copy operation for every blob (for an import job) or file (for an export job), whereas the error log contains only the information for blobs or files that encountered errors during the import or export job.</span></span>  
  
<span data-ttu-id="1c8a9-138">Utförlig loggformatet visas nedan.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-138">The verbose log format is shown below.</span></span> <span data-ttu-id="1c8a9-139">Felloggen har samma struktur, men filtrerar ut lyckade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-139">The error log has the same structure, but filters out successful operations.</span></span>  

```xml
<DriveLog Version="2014-11-01">  
  <DriveId>drive-id</DriveId>  
  [<Blob Status="blob-status">  
   <BlobPath>blob-path</BlobPath>  
   <FilePath>file-path</FilePath>  
   [<Snapshot>snapshot</Snapshot>]  
   <Length>length</Length>  
   [<LastModified>last-modified</LastModified>]  
   [<ImportDisposition Status="import-disposition-status">import-disposition</ImportDisposition>]  
   [page-range-list-or-block-list]  
   [metadata-status]  
   [properties-status]  
  </Blob>]  
  [<Blob>  
    . . .  
  </Blob>]  
  <Status>drive-status</Status>  
</DriveLog>  
  
page-range-list-or-block-list ::= 
  page-range-list | block-list  
  
page-range-list ::=   
<PageRangeList>  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
</PageRangeList>  
  
block-list ::=  
<BlockList>  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]  
       [Hash="md5-hash"] Status="block-status"/>]  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]   
       [Hash="md5-hash"] Status="block-status"/>]  
</BlockList>  
  
metadata-status ::=  
<Metadata Status="metadata-status">  
   [<GlobalPath Hash="md5-hash">global-metadata-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">metadata-file-path</Path>]  
</Metadata>  
  
properties-status ::=  
<Properties Status="properties-status">  
   [<GlobalPath Hash="md5-hash">global-properties-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">properties-file-path</Path>]  
</Properties>  
```

<span data-ttu-id="1c8a9-140">I följande tabell beskrivs elementen i loggfilen.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-140">The following table describes the elements of the log file.</span></span>  
  
|<span data-ttu-id="1c8a9-141">XML-Element</span><span class="sxs-lookup"><span data-stu-id="1c8a9-141">XML Element</span></span>|<span data-ttu-id="1c8a9-142">Typ</span><span class="sxs-lookup"><span data-stu-id="1c8a9-142">Type</span></span>|<span data-ttu-id="1c8a9-143">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1c8a9-143">Description</span></span>|  
|-----------------|----------|-----------------|  
|`DriveLog`|<span data-ttu-id="1c8a9-144">XML-Element</span><span class="sxs-lookup"><span data-stu-id="1c8a9-144">XML Element</span></span>|<span data-ttu-id="1c8a9-145">Representerar en logg för enheten.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-145">Represents a drive log.</span></span>|  
|`Version`|<span data-ttu-id="1c8a9-146">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-146">Attribute, String</span></span>|<span data-ttu-id="1c8a9-147">Versionen av loggformatet.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-147">The version of the log format.</span></span>|  
|`DriveId`|<span data-ttu-id="1c8a9-148">Sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-148">String</span></span>|<span data-ttu-id="1c8a9-149">Enhetens serienummer för maskinvara.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-149">The drive's hardware serial number.</span></span>|  
|`Status`|<span data-ttu-id="1c8a9-150">Sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-150">String</span></span>|<span data-ttu-id="1c8a9-151">Status för enheten bearbetning.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-151">Status of the drive processing.</span></span> <span data-ttu-id="1c8a9-152">Finns det `Drive Status Codes` tabellen nedan för mer information.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-152">See the `Drive Status Codes` table below for more information.</span></span>|  
|`Blob`|<span data-ttu-id="1c8a9-153">Kapslade XML-element</span><span class="sxs-lookup"><span data-stu-id="1c8a9-153">Nested XML element</span></span>|<span data-ttu-id="1c8a9-154">Representerar en blob.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-154">Represents a blob.</span></span>|  
|`Blob/BlobPath`|<span data-ttu-id="1c8a9-155">Sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-155">String</span></span>|<span data-ttu-id="1c8a9-156">Blob-URI.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-156">The URI of the blob.</span></span>|  
|`Blob/FilePath`|<span data-ttu-id="1c8a9-157">Sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-157">String</span></span>|<span data-ttu-id="1c8a9-158">Den relativa sökvägen till filen på enheten.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-158">The relative path to the file on the drive.</span></span>|  
|`Blob/Snapshot`|<span data-ttu-id="1c8a9-159">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="1c8a9-159">DateTime</span></span>|<span data-ttu-id="1c8a9-160">Versionen av blob för ett exportjobb endast ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-160">The snapshot version of the blob, for an export job only.</span></span>|  
|`Blob/Length`|<span data-ttu-id="1c8a9-161">Integer</span><span class="sxs-lookup"><span data-stu-id="1c8a9-161">Integer</span></span>|<span data-ttu-id="1c8a9-162">Den totala längden på blob i byte.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-162">The total length of the blob in bytes.</span></span>|  
|`Blob/LastModified`|<span data-ttu-id="1c8a9-163">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="1c8a9-163">DateTime</span></span>|<span data-ttu-id="1c8a9-164">Tidsvärdet som blob ändrades senast, för ett exportjobb endast.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-164">The date/time that the blob was last modified, for an export job only.</span></span>|  
|`Blob/ImportDisposition`|<span data-ttu-id="1c8a9-165">Sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-165">String</span></span>|<span data-ttu-id="1c8a9-166">Importera disposition på blob för en importjobb.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-166">The import disposition of the blob, for an import job only.</span></span>|  
|`Blob/ImportDisposition/@Status`|<span data-ttu-id="1c8a9-167">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-167">Attribute, String</span></span>|<span data-ttu-id="1c8a9-168">Status för import-disposition.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-168">The status of the import disposition.</span></span>|  
|`PageRangeList`|<span data-ttu-id="1c8a9-169">Kapslade XML-element</span><span class="sxs-lookup"><span data-stu-id="1c8a9-169">Nested XML element</span></span>|<span data-ttu-id="1c8a9-170">Representerar en lista över sidintervall för en sidblobb.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-170">Represents a list of page ranges for a page blob.</span></span>|  
|`PageRange`|<span data-ttu-id="1c8a9-171">XML-element</span><span class="sxs-lookup"><span data-stu-id="1c8a9-171">XML element</span></span>|<span data-ttu-id="1c8a9-172">Representerar ett sidintervall.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-172">Represents a page range.</span></span>|  
|`PageRange/@Offset`|<span data-ttu-id="1c8a9-173">Attributet heltal</span><span class="sxs-lookup"><span data-stu-id="1c8a9-173">Attribute, Integer</span></span>|<span data-ttu-id="1c8a9-174">Första förskjutningen av sidintervallet i blob.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-174">Starting offset of the page range in the blob.</span></span>|  
|`PageRange/@Length`|<span data-ttu-id="1c8a9-175">Attributet heltal</span><span class="sxs-lookup"><span data-stu-id="1c8a9-175">Attribute, Integer</span></span>|<span data-ttu-id="1c8a9-176">Längden i byte på sidintervallet.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-176">Length in bytes of the page range.</span></span>|  
|`PageRange/@Hash`|<span data-ttu-id="1c8a9-177">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-177">Attribute, String</span></span>|<span data-ttu-id="1c8a9-178">Base16-kodade MD5-hash för sidintervallet.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-178">Base16-encoded MD5 hash of the page range.</span></span>|  
|`PageRange/@Status`|<span data-ttu-id="1c8a9-179">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-179">Attribute, String</span></span>|<span data-ttu-id="1c8a9-180">Status för bearbetning av sidintervallet.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-180">Status of processing the page range.</span></span>|  
|`BlockList`|<span data-ttu-id="1c8a9-181">Kapslade XML-element</span><span class="sxs-lookup"><span data-stu-id="1c8a9-181">Nested XML element</span></span>|<span data-ttu-id="1c8a9-182">Representerar en lista över block för en blockblob.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-182">Represents a list of blocks for a block blob.</span></span>|  
|`Block`|<span data-ttu-id="1c8a9-183">XML-element</span><span class="sxs-lookup"><span data-stu-id="1c8a9-183">XML element</span></span>|<span data-ttu-id="1c8a9-184">Representerar ett block.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-184">Represents a block.</span></span>|  
|`Block/@Offset`|<span data-ttu-id="1c8a9-185">Attributet heltal</span><span class="sxs-lookup"><span data-stu-id="1c8a9-185">Attribute, Integer</span></span>|<span data-ttu-id="1c8a9-186">Första förskjutningen av block i blob.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-186">Starting offset of the block in the blob.</span></span>|  
|`Block/@Length`|<span data-ttu-id="1c8a9-187">Attributet heltal</span><span class="sxs-lookup"><span data-stu-id="1c8a9-187">Attribute, Integer</span></span>|<span data-ttu-id="1c8a9-188">Längden i byte av blocket.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-188">Length in bytes of the block.</span></span>|  
|`Block/@Id`|<span data-ttu-id="1c8a9-189">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-189">Attribute, String</span></span>|<span data-ttu-id="1c8a9-190">Block-ID.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-190">The block ID.</span></span>|  
|`Block/@Hash`|<span data-ttu-id="1c8a9-191">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-191">Attribute, String</span></span>|<span data-ttu-id="1c8a9-192">Base16-kodade MD5-hash av blocket.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-192">Base16-encoded MD5 hash of the block.</span></span>|  
|`Block/@Status`|<span data-ttu-id="1c8a9-193">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-193">Attribute, String</span></span>|<span data-ttu-id="1c8a9-194">Status för bearbetning av blocket.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-194">Status of processing the block.</span></span>|  
|`Metadata`|<span data-ttu-id="1c8a9-195">Kapslade XML-element</span><span class="sxs-lookup"><span data-stu-id="1c8a9-195">Nested XML element</span></span>|<span data-ttu-id="1c8a9-196">Representerar det blob-metadata.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-196">Represents the blob's metadata.</span></span>|  
|`Metadata/@Status`|<span data-ttu-id="1c8a9-197">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-197">Attribute, String</span></span>|<span data-ttu-id="1c8a9-198">Status för bearbetning av blobmetadata.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-198">Status of processing of the blob metadata.</span></span>|  
|`Metadata/GlobalPath`|<span data-ttu-id="1c8a9-199">Sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-199">String</span></span>|<span data-ttu-id="1c8a9-200">Relativ sökväg till filen globala metadata.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-200">Relative path to the global metadata file.</span></span>|  
|`Metadata/GlobalPath/@Hash`|<span data-ttu-id="1c8a9-201">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-201">Attribute, String</span></span>|<span data-ttu-id="1c8a9-202">Base16-kodade MD5-hash för den globala metadatafilen.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-202">Base16-encoded MD5 hash of the global metadata file.</span></span>|  
|`Metadata/Path`|<span data-ttu-id="1c8a9-203">Sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-203">String</span></span>|<span data-ttu-id="1c8a9-204">Relativ sökväg till metadatafilen.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-204">Relative path to the metadata file.</span></span>|  
|`Metadata/Path/@Hash`|<span data-ttu-id="1c8a9-205">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-205">Attribute, String</span></span>|<span data-ttu-id="1c8a9-206">Base16-kodade MD5-hash för metadatafilen.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-206">Base16-encoded MD5 hash of the metadata file.</span></span>|  
|`Properties`|<span data-ttu-id="1c8a9-207">Kapslade XML-element</span><span class="sxs-lookup"><span data-stu-id="1c8a9-207">Nested XML element</span></span>|<span data-ttu-id="1c8a9-208">Representerar blob-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-208">Represents the blob properties.</span></span>|  
|`Properties/@Status`|<span data-ttu-id="1c8a9-209">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-209">Attribute, String</span></span>|<span data-ttu-id="1c8a9-210">Status för bearbetning av blob-egenskaper, t.ex. inte att hitta filen, slutförts.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-210">Status of processing the blob properties, e.g. file not found, completed.</span></span>|  
|`Properties/GlobalPath`|<span data-ttu-id="1c8a9-211">Sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-211">String</span></span>|<span data-ttu-id="1c8a9-212">Relativ sökväg till filen globala egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-212">Relative path to the global properties file.</span></span>|  
|`Properties/GlobalPath/@Hash`|<span data-ttu-id="1c8a9-213">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-213">Attribute, String</span></span>|<span data-ttu-id="1c8a9-214">Base16-kodade MD5 hash för den globala egenskaper för filen.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-214">Base16-encoded MD5 hash of the global properties file.</span></span>|  
|`Properties/Path`|<span data-ttu-id="1c8a9-215">Sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-215">String</span></span>|<span data-ttu-id="1c8a9-216">Relativ sökväg till filen egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-216">Relative path to the properties file.</span></span>|  
|`Properties/Path/@Hash`|<span data-ttu-id="1c8a9-217">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-217">Attribute, String</span></span>|<span data-ttu-id="1c8a9-218">Base16-kodade MD5-hash för filen egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-218">Base16-encoded MD5 hash of the properties file.</span></span>|  
|`Blob/Status`|<span data-ttu-id="1c8a9-219">Sträng</span><span class="sxs-lookup"><span data-stu-id="1c8a9-219">String</span></span>|<span data-ttu-id="1c8a9-220">Status för bearbetning av blob.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-220">Status of processing the blob.</span></span>|  
  
# <a name="drive-status-codes"></a><span data-ttu-id="1c8a9-221">Statuskoder för enhet</span><span class="sxs-lookup"><span data-stu-id="1c8a9-221">Drive status codes</span></span>  
<span data-ttu-id="1c8a9-222">I följande tabell visas statuskoder för bearbetning av en enhet.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-222">The following table lists the status codes for processing a drive.</span></span>  
  
|<span data-ttu-id="1c8a9-223">Statuskod</span><span class="sxs-lookup"><span data-stu-id="1c8a9-223">Status code</span></span>|<span data-ttu-id="1c8a9-224">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1c8a9-224">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="1c8a9-225">Enheten har bearbetat utan fel.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-225">The drive has finished processing without any errors.</span></span>|  
|`CompletedWithWarnings`|<span data-ttu-id="1c8a9-226">Enheten har slutförts med varningar i en eller flera blobbar per import-disposition som angetts för den BLOB-objekt.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-226">The drive has finished processing with warnings in one or more blobs per the import dispositions specified for the blobs.</span></span>|  
|`CompletedWithErrors`|<span data-ttu-id="1c8a9-227">Enheten har slutförts med fel i en eller flera blobbar segment.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-227">The drive has finished with errors in one or more blobs or chunks.</span></span>|  
|`DiskNotFound`|<span data-ttu-id="1c8a9-228">Ingen disk finns på enheten.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-228">No disk is found on the drive.</span></span>|  
|`VolumeNotNtfs`|<span data-ttu-id="1c8a9-229">Den första datavolymen på disken är inte i NTFS-format.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-229">The first data volume on the disk is not in NTFS format.</span></span>|  
|`DiskOperationFailed`|<span data-ttu-id="1c8a9-230">Ett okänt fel uppstod när du utför åtgärder på enheten.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-230">An unknown failure occurred when performing operations on the drive.</span></span>|  
|`BitLockerVolumeNotFound`|<span data-ttu-id="1c8a9-231">Ingen encryptable volym BitLocker finns.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-231">No BitLocker encryptable volume is found.</span></span>|  
|`BitLockerNotActivated`|<span data-ttu-id="1c8a9-232">BitLocker är inte aktiverat på volymen.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-232">BitLocker is not enabled on the volume.</span></span>|  
|`BitLockerProtectorNotFound`|<span data-ttu-id="1c8a9-233">Nyckelskydd för numeriskt lösenord finns inte på volymen.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-233">The numerical password key protector does not exist on the volume.</span></span>|  
|`BitLockerKeyInvalid`|<span data-ttu-id="1c8a9-234">Det angivna numeriska lösenordet går inte att låsa upp volymen.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-234">The numerical password provided cannot unlock the volume.</span></span>|  
|`BitLockerUnlockVolumeFailed`|<span data-ttu-id="1c8a9-235">Okänt fel uppstod vid försök att låsa upp volymen.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-235">Unknown failure has happened when trying to unlock the volume.</span></span>|  
|`BitLockerFailed`|<span data-ttu-id="1c8a9-236">Ett okänt fel uppstod vid BitLocker-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-236">An unknown failure occurred while performing BitLocker operations.</span></span>|  
|`ManifestNameInvalid`|<span data-ttu-id="1c8a9-237">Manifestfilens namn är ogiltigt.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-237">The manifest file name is invalid.</span></span>|  
|`ManifestNameTooLong`|<span data-ttu-id="1c8a9-238">Manifestfilens filnamn är för långt.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-238">The manifest file name is too long.</span></span>|  
|`ManifestNotFound`|<span data-ttu-id="1c8a9-239">Manifestfilen hittades inte.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-239">The manifest file is not found.</span></span>|  
|`ManifestAccessDenied`|<span data-ttu-id="1c8a9-240">Åtkomst till manifestfilen nekas.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-240">Access to the manifest file is denied.</span></span>|  
|`ManifestCorrupted`|<span data-ttu-id="1c8a9-241">Manifestfilen är skadad (innehållet matchar inte dess hash).</span><span class="sxs-lookup"><span data-stu-id="1c8a9-241">The manifest file is corrupted (the content does not match its hash).</span></span>|  
|`ManifestFormatInvalid`|<span data-ttu-id="1c8a9-242">Manifestet innehållet överensstämmer inte med formatet som krävs.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-242">The manifest content does not conform to the required format.</span></span>|  
|`ManifestDriveIdMismatch`|<span data-ttu-id="1c8a9-243">Enhetsidentitet i manifestfilen överensstämmer inte med en läsningen från enheten.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-243">The drive ID in the manifest file does not match the one read from the drive.</span></span>|  
|`ReadManifestFailed`|<span data-ttu-id="1c8a9-244">En disk-i/o-fel uppstod vid läsning från manifestet.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-244">A disk I/O failure occurred while reading from the manifest.</span></span>|  
|`BlobListFormatInvalid`|<span data-ttu-id="1c8a9-245">Exportera blob listan blob stämmer inte överens med formatet som krävs.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-245">The export blob list blob does not conform to the required format.</span></span>|  
|`BlobRequestForbidden`|<span data-ttu-id="1c8a9-246">Åtkomst till blobarna i lagringskontot är förbjuden.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-246">Access to the blobs in the storage account is forbidden.</span></span> <span data-ttu-id="1c8a9-247">Detta kan bero på ogiltiga lagringskontonyckel eller SAS-behållare.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-247">This might be due to invalid storage account key or container SAS.</span></span>|  
|`InternalError`|<span data-ttu-id="1c8a9-248">Och internt fel uppstod under bearbetning av enheten.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-248">And internal error occurred while processing the drive.</span></span>|  
  
## <a name="blob-status-codes"></a><span data-ttu-id="1c8a9-249">BLOB-statuskoder</span><span class="sxs-lookup"><span data-stu-id="1c8a9-249">Blob status codes</span></span>  
<span data-ttu-id="1c8a9-250">I följande tabell visas statuskoder för bearbetning av en blob.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-250">The following table lists the status codes for processing a blob.</span></span>  
  
|<span data-ttu-id="1c8a9-251">Statuskod</span><span class="sxs-lookup"><span data-stu-id="1c8a9-251">Status code</span></span>|<span data-ttu-id="1c8a9-252">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1c8a9-252">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="1c8a9-253">Blobben har bearbetat utan fel.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-253">The blob has finished processing without errors.</span></span>|  
|`CompletedWithErrors`|<span data-ttu-id="1c8a9-254">Blobben har bearbetat med fel i en eller flera sidintervall block, metadata eller egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-254">The blob has finished processing with errors in one or more page ranges or blocks, metadata, or properties.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="1c8a9-255">Filnamnet är ogiltigt.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-255">The file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="1c8a9-256">Filnamnet är för långt.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-256">The file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="1c8a9-257">Filen hittades inte.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-257">The file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="1c8a9-258">Åtkomst till filen nekades.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-258">Access to the file is denied.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="1c8a9-259">Det gick inte att tjänstbegäran Blob för att få åtkomst till blob.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-259">The Blob service request to access the blob has failed.</span></span>|  
|`BlobRequestForbidden`|<span data-ttu-id="1c8a9-260">Blob-tjänsten åtkomstbegäran till blob tillåts inte.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-260">The Blob service request to access the blob is forbidden.</span></span> <span data-ttu-id="1c8a9-261">Detta kan bero på ogiltiga lagringskontonyckel eller SAS-behållare.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-261">This might be due to invalid storage account key or container SAS.</span></span>|  
|`RenameFailed`|<span data-ttu-id="1c8a9-262">Det gick inte att byta namn på blob (för ett importjobb) eller filen (för ett exportjobb).</span><span class="sxs-lookup"><span data-stu-id="1c8a9-262">Failed to rename the blob (for an import job) or the file (for an export job).</span></span>|  
|`BlobUnexpectedChange`|<span data-ttu-id="1c8a9-263">En oväntad ändring uppstod med blobben (för ett exportjobb).</span><span class="sxs-lookup"><span data-stu-id="1c8a9-263">An unexpected change has occurred with the blob (for an export job).</span></span>|  
|`LeasePresent`|<span data-ttu-id="1c8a9-264">Det finns ett lån som finns på blob.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-264">There is a lease present on the blob.</span></span>|  
|`IOFailed`|<span data-ttu-id="1c8a9-265">Ett disk- eller i/o-fel uppstod under bearbetning av blobben.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-265">A disk or network I/O failure occurred while processing the blob.</span></span>|  
|`Failed`|<span data-ttu-id="1c8a9-266">Ett okänt fel uppstod under bearbetning av blobben.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-266">An unknown failure occurred while processing the blob.</span></span>|  
  
## <a name="import-disposition-status-codes"></a><span data-ttu-id="1c8a9-267">Importera disposition statuskoder</span><span class="sxs-lookup"><span data-stu-id="1c8a9-267">Import disposition status codes</span></span>  
<span data-ttu-id="1c8a9-268">I följande tabell visas statuskoder för att lösa ett import-disposition.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-268">The following table lists the status codes for resolving an import disposition.</span></span>  
  
|<span data-ttu-id="1c8a9-269">Statuskod</span><span class="sxs-lookup"><span data-stu-id="1c8a9-269">Status code</span></span>|<span data-ttu-id="1c8a9-270">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1c8a9-270">Description</span></span>|  
|-----------------|-----------------|  
|`Created`|<span data-ttu-id="1c8a9-271">Blobben har skapats.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-271">The blob has been created.</span></span>|  
|`Renamed`|<span data-ttu-id="1c8a9-272">Blobben har bytt per Byt namn på import disposition.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-272">The blob has been renamed per rename import disposition.</span></span> <span data-ttu-id="1c8a9-273">Den `Blob/BlobPath` elementet innehåller URI för bytt namn till blob.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-273">The `Blob/BlobPath` element contains the URI for the renamed blob.</span></span>|  
|`Skipped`|<span data-ttu-id="1c8a9-274">Blobben har hoppats över `no-overwrite` importera disposition.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-274">The blob has been skipped per `no-overwrite` import disposition.</span></span>|  
|`Overwritten`|<span data-ttu-id="1c8a9-275">Blobben har skrivit över en befintlig blob per `overwrite` importera disposition.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-275">The blob has overwritten an existing blob per `overwrite` import disposition.</span></span>|  
|`Cancelled`|<span data-ttu-id="1c8a9-276">Ett tidigare fel har stoppats vidare bearbetning av disposition för import.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-276">A prior failure has stopped further processing of the import disposition.</span></span>|  
  
## <a name="page-rangeblock-status-codes"></a><span data-ttu-id="1c8a9-277">Statuskoder för sidan intervall /-block</span><span class="sxs-lookup"><span data-stu-id="1c8a9-277">Page range/block status codes</span></span>  
<span data-ttu-id="1c8a9-278">I följande tabell visas statuskoder för bearbetning av ett sidintervall eller ett block.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-278">The following table lists the status codes for processing a page range or a block.</span></span>  
  
|<span data-ttu-id="1c8a9-279">Statuskod</span><span class="sxs-lookup"><span data-stu-id="1c8a9-279">Status code</span></span>|<span data-ttu-id="1c8a9-280">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1c8a9-280">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="1c8a9-281">Sidan intervall eller block har bearbetat utan fel.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-281">The page range or block has finished processing without any errors.</span></span>|  
|`Committed`|<span data-ttu-id="1c8a9-282">Blocket har allokerats, men i fullständig blocket listan eftersom andra block har misslyckats eller placera fullständig Blockeringslista själva misslyckades inte.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-282">The block has been committed,  but not in the full block list because other blocks have failed, or put full block list itself has failed.</span></span>|  
|`Uncommitted`|<span data-ttu-id="1c8a9-283">Blocket har överförts men inte allokerad.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-283">The block is uploaded but not committed.</span></span>|  
|`Corrupted`|<span data-ttu-id="1c8a9-284">Sidan intervall eller block är skadad (innehållet matchar inte dess hash).</span><span class="sxs-lookup"><span data-stu-id="1c8a9-284">The page range or block is corrupted (the content does not match its hash).</span></span>|  
|`FileUnexpectedEnd`|<span data-ttu-id="1c8a9-285">Ett oväntat filslut påträffades.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-285">An unexpected end of file has been encountered.</span></span>|  
|`BlobUnexpectedEnd`|<span data-ttu-id="1c8a9-286">Det inträffade ett oväntat slut på blob.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-286">An unexpected end of blob has been encountered.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="1c8a9-287">Det gick inte att tjänstbegäran Blob för att komma åt sidan intervall eller block.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-287">The Blob service request to access the page range or block has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="1c8a9-288">Ett disk- eller i/o-fel uppstod vid bearbetningen av sidan intervall eller block.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-288">A disk or network I/O failure occurred while processing the page range or block.</span></span>|  
|`Failed`|<span data-ttu-id="1c8a9-289">Ett okänt fel uppstod under bearbetning av sidan intervall eller block.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-289">An unknown failure occurred while processing the page range or block.</span></span>|  
|`Cancelled`|<span data-ttu-id="1c8a9-290">Ett tidigare fel har stoppats vidare bearbetning av sidan intervall eller block.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-290">A prior failure has stopped further processing of the page range or block.</span></span>|  
  
## <a name="metadata-status-codes"></a><span data-ttu-id="1c8a9-291">Statuskoder för metadata</span><span class="sxs-lookup"><span data-stu-id="1c8a9-291">Metadata status codes</span></span>  
<span data-ttu-id="1c8a9-292">I följande tabell visas statuskoder för bearbetning av blobbmetadata.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-292">The following table lists the status codes for processing blob metadata.</span></span>  
  
|<span data-ttu-id="1c8a9-293">Statuskod</span><span class="sxs-lookup"><span data-stu-id="1c8a9-293">Status code</span></span>|<span data-ttu-id="1c8a9-294">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1c8a9-294">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="1c8a9-295">Metadata har bearbetat utan fel.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-295">The metadata has finished processing without errors.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="1c8a9-296">Metadata-filnamnet är ogiltigt.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-296">The metadata file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="1c8a9-297">Metadata-filnamnet är för långt.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-297">The metadata file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="1c8a9-298">Metadatafilen hittades inte.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-298">The metadata file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="1c8a9-299">Åtkomst till metadatafilen nekas.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-299">Access to the metadata file is denied.</span></span>|  
|`Corrupted`|<span data-ttu-id="1c8a9-300">Metadatafilen är skadad (innehållet matchar inte dess hash).</span><span class="sxs-lookup"><span data-stu-id="1c8a9-300">The metadata file is corrupted (the content does not match its hash).</span></span>|  
|`XmlReadFailed`|<span data-ttu-id="1c8a9-301">Metadata-innehållet överensstämmer inte med formatet som krävs.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-301">The metadata content does not conform to the required format.</span></span>|  
|`XmlWriteFailed`|<span data-ttu-id="1c8a9-302">Skriva metadata misslyckades XML.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-302">Writing the metadata XML has failed.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="1c8a9-303">Det gick inte att tjänstbegäran Blob för att komma åt metadata.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-303">The Blob service request to access the metadata has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="1c8a9-304">Ett disk- eller i/o-fel uppstod under bearbetning av metadata.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-304">A disk or network I/O failure occurred while processing the metadata.</span></span>|  
|`Failed`|<span data-ttu-id="1c8a9-305">Ett okänt fel uppstod under bearbetning av metadata.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-305">An unknown failure occurred while processing the metadata.</span></span>|  
|`Cancelled`|<span data-ttu-id="1c8a9-306">Ett tidigare fel har stoppats vidare bearbetning av metadata.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-306">A prior failure has stopped further processing of the metadata.</span></span>|  
  
## <a name="properties-status-codes"></a><span data-ttu-id="1c8a9-307">Statuskoder för egenskaper</span><span class="sxs-lookup"><span data-stu-id="1c8a9-307">Properties status codes</span></span>  
<span data-ttu-id="1c8a9-308">I följande tabell visas statuskoder för bearbetning av blob-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-308">The following table lists the status codes for processing blob properties.</span></span>  
  
|<span data-ttu-id="1c8a9-309">Statuskod</span><span class="sxs-lookup"><span data-stu-id="1c8a9-309">Status code</span></span>|<span data-ttu-id="1c8a9-310">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1c8a9-310">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="1c8a9-311">Egenskaperna är klar bearbetning utan fel.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-311">The properties have finished processing without any errors.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="1c8a9-312">Egenskaper för filnamnet är ogiltigt.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-312">The properties file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="1c8a9-313">Egenskaper för filnamn är för lång.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-313">The properties file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="1c8a9-314">Egenskaper för filen hittades inte.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-314">The properties file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="1c8a9-315">Nekad åtkomst till filen egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-315">Access to the properties file is denied.</span></span>|  
|`Corrupted`|<span data-ttu-id="1c8a9-316">Egenskaper för filen är skadad (innehållet matchar inte dess hash).</span><span class="sxs-lookup"><span data-stu-id="1c8a9-316">The properties file is corrupted (the content does not match its hash).</span></span>|  
|`XmlReadFailed`|<span data-ttu-id="1c8a9-317">Egenskaper för innehållet överensstämmer inte med formatet som krävs.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-317">The properties content does not conform to the required format.</span></span>|  
|`XmlWriteFailed`|<span data-ttu-id="1c8a9-318">Skriva egenskaperna misslyckades XML.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-318">Writing the properties XML has failed.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="1c8a9-319">Det gick inte att tjänstbegäran Blob för att komma åt egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-319">The Blob service request to access the properties has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="1c8a9-320">Ett disk- eller i/o-fel uppstod under bearbetning av egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-320">A disk or network I/O failure occurred while processing the properties.</span></span>|  
|`Failed`|<span data-ttu-id="1c8a9-321">Ett okänt fel uppstod under bearbetning av egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-321">An unknown failure occurred while processing the properties.</span></span>|  
|`Cancelled`|<span data-ttu-id="1c8a9-322">Ett tidigare fel har stoppats ytterligare bearbetning av egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-322">A prior failure has stopped further processing of the properties.</span></span>|  
  
## <a name="sample-logs"></a><span data-ttu-id="1c8a9-323">Exempel loggar</span><span class="sxs-lookup"><span data-stu-id="1c8a9-323">Sample logs</span></span>  
<span data-ttu-id="1c8a9-324">Följande är ett exempel på detaljerade loggen.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-324">The following is an example of verbose log.</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV123456</DriveId>  
    <Blob Status="Completed">  
       <BlobPath>pictures/bob/wild/desert.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\wild\desert.jpg</FilePath>  
       <Length>98304</Length>  
       <ImportDisposition Status="Created">overwrite</ImportDisposition>  
       <BlockList>  
          <Block Offset="0" Length="65536" Id="AAAAAA==" Hash=" 9C8AE14A55241F98533C4D80D85CDC68" Status="Completed"/>  
          <Block Offset="65536" Length="32768" Id="AQAAAA==" Hash=" DF54C531C9B3CA2570FDDDB3BCD0E27D" Status="Completed"/>  
       </BlockList>  
       <Metadata Status="Completed">  
          <GlobalPath Hash=" E34F54B7086BCF4EC1601D056F4C7E37">\Users\bob\Pictures\wild\metadata.xml</GlobalPath>  
       </Metadata>  
    </Blob>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="0" Length="65536" Hash="19701B8877418393CB3CB567F53EE225" Status="Completed"/>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
          <PageRange Offset="131072" Length="4096" Hash="9BA552E1C3EEAFFC91B42B979900A996" Status="Completed"/>  
       </PageRangeList>  
       <Properties Status="Completed">  
          <Path Hash="38D7AE80653F47F63C0222FEE90EC4E7">\Users\bob\Pictures\animals\koala.jpg.properties</Path>  
       </Properties>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
<span data-ttu-id="1c8a9-325">Motsvarande felloggen visas nedan.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-325">The corresponding error log is shown below.</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV6965824</DriveId>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
       </PageRangeList>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

 <span data-ttu-id="1c8a9-326">Följ felloggen för importen innehåller ett fel om en fil inte hittas på enheten för import.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-326">The follow error log for an import job contains an error about a file not found on the import drive.</span></span> <span data-ttu-id="1c8a9-327">Observera att status för efterföljande komponenter är `Cancelled`.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-327">Note that the status of subsequent components is `Cancelled`.</span></span>  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="FileNotFound">  
    <BlobPath>pictures/animals/koala.jpg</BlobPath>  
    <FilePath>\animals\koala.jpg</FilePath>  
    <Length>30310</Length>  
    <ImportDisposition Status="Cancelled">rename</ImportDisposition>  
    <BlockList>  
      <Block Offset="0" Length="6062" Id="MD5/cAzn4h7VVSWXf696qp5Uaw==" Hash="700CE7E21ED55525977FAF7AAA9E546B" Status="Cancelled" />  
      <Block Offset="6062" Length="6062" Id="MD5/PEnGwYOI8LPLNYdfKr7kAg==" Hash="3C49C6C18388F0B3CB35875F2ABEE402" Status="Cancelled" />  
      <Block Offset="12124" Length="6062" Id="MD5/FG4WxqfZKuUWZ2nGTU2qVA==" Hash="146E16C6A7D92AE5166769C64D4DAA54" Status="Cancelled" />  
      <Block Offset="18186" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
      <Block Offset="24248" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

<span data-ttu-id="1c8a9-328">Följande felloggen för ett exportjobb anger att blob-innehållet har skrivits till enheten, utan att ett fel inträffade vid export av blob-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1c8a9-328">The following error log for an export job indicates that the blob content has been successfully written to the drive, but that an error occurred while exporting the blob's properties.</span></span>  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C3U</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
    <FilePath>\pictures\wild\canyon.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList />  
    <Properties Status="Failed" />  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
## <a name="next-steps"></a><span data-ttu-id="1c8a9-329">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1c8a9-329">Next steps</span></span>
 
* [<span data-ttu-id="1c8a9-330">REST-API för Storage Import/Export</span><span class="sxs-lookup"><span data-stu-id="1c8a9-330">Storage Import/Export REST API</span></span>](/rest/api/storageimportexport/)
