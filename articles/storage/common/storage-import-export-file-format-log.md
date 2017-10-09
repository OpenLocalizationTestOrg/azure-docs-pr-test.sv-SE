---
title: "loggfilsformat för aaaAzure Import/Export | Microsoft Docs"
description: "Läs mer om hello format hello loggfilerna som skapades när åtgärder utförs för ett jobb med Import/Export."
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
ms.openlocfilehash: 15a652455aa947922af0aa39ccefe68811a3db19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-importexport-service-log-file-format"></a><span data-ttu-id="8e589-103">Azure Import/Export service loggfilsformat</span><span class="sxs-lookup"><span data-stu-id="8e589-103">Azure Import/Export service log file format</span></span>
<span data-ttu-id="8e589-104">När hello Microsoft Azure Import/Export service utför en åtgärd på en enhet som en del av ett importjobb eller ett exportjobb skrivs loggarna tooblock blobbar i hello storage-konto som är associerade med jobbet.</span><span class="sxs-lookup"><span data-stu-id="8e589-104">When hello Microsoft Azure Import/Export service performs an action on a drive as part of an import job or an export job, logs are written tooblock blobs in hello storage account associated with that job.</span></span>  
  
<span data-ttu-id="8e589-105">Det finns två loggar som kan skrivas av hello Import/Export service:</span><span class="sxs-lookup"><span data-stu-id="8e589-105">There are two logs that may be written by hello Import/Export service:</span></span>  
  
-   <span data-ttu-id="8e589-106">hello felloggen skapas alltid i hello händelse av ett fel.</span><span class="sxs-lookup"><span data-stu-id="8e589-106">hello error log is always generated in hello event of an error.</span></span>  
  
-   <span data-ttu-id="8e589-107">hello utförlig loggen är inte aktiverad som standard, men kan aktiveras genom att ange hello `EnableVerboseLog` -egenskapen i en [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) eller [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) igen.</span><span class="sxs-lookup"><span data-stu-id="8e589-107">hello verbose log is not enabled by default, but may be enabled by setting hello `EnableVerboseLog` property on a [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation.</span></span>  
  
## <a name="log-file-location"></a><span data-ttu-id="8e589-108">Plats för loggfilen</span><span class="sxs-lookup"><span data-stu-id="8e589-108">Log file location</span></span>  
<span data-ttu-id="8e589-109">hello loggarna skrivs tooblock blobbar i behållaren hello eller virtuella katalogen som anges av hello `ImportExportStatesPath` inställning som du kan ange för en `Put Job` igen.</span><span class="sxs-lookup"><span data-stu-id="8e589-109">hello logs are written tooblock blobs in hello container or virtual directory specified by hello `ImportExportStatesPath` setting, which you can set on a `Put Job` operation.</span></span> <span data-ttu-id="8e589-110">hello plats toowhich hello loggarna skrivs beror på hur autentisering har angetts för hello jobb, tillsammans med hello-värde som angetts för `ImportExportStatesPath`.</span><span class="sxs-lookup"><span data-stu-id="8e589-110">hello location toowhich hello logs are written depends on how authentication is specified for hello job, together with hello value specified for `ImportExportStatesPath`.</span></span> <span data-ttu-id="8e589-111">Autentisering för hello jobb kan anges via en lagringskontonyckel eller en behållare SAS (signatur för delad åtkomst).</span><span class="sxs-lookup"><span data-stu-id="8e589-111">Authentication for hello job may be specified via a storage account key, or a container SAS (shared access signature).</span></span>  
  
<span data-ttu-id="8e589-112">hello namnet på behållaren hello eller virtuell katalog kan antingen vara hello standardnamnet `waimportexport`, eller en annan behållare eller en virtuell katalog som du anger.</span><span class="sxs-lookup"><span data-stu-id="8e589-112">hello name of hello container or virtual directory may either be hello default name of `waimportexport`, or another container or virtual directory name that you specify.</span></span>  
  
<span data-ttu-id="8e589-113">hello tabellen nedan visar hello möjliga alternativ:</span><span class="sxs-lookup"><span data-stu-id="8e589-113">hello table below shows hello possible options:</span></span>  
  
|<span data-ttu-id="8e589-114">Autentiseringsmetod</span><span class="sxs-lookup"><span data-stu-id="8e589-114">Authentication Method</span></span>|<span data-ttu-id="8e589-115">Värdet för `ImportExportStatesPath`Element</span><span class="sxs-lookup"><span data-stu-id="8e589-115">Value of `ImportExportStatesPath`Element</span></span>|<span data-ttu-id="8e589-116">Plats för logg-BLOB</span><span class="sxs-lookup"><span data-stu-id="8e589-116">Location of Log Blobs</span></span>|  
|---------------------------|----------------------------------------------|---------------------------|  
|<span data-ttu-id="8e589-117">Lagringskontonyckel</span><span class="sxs-lookup"><span data-stu-id="8e589-117">Storage account key</span></span>|<span data-ttu-id="8e589-118">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="8e589-118">Default value</span></span>|<span data-ttu-id="8e589-119">En behållare med namnet `waimportexport`, vilket är hello standardbehållaren.</span><span class="sxs-lookup"><span data-stu-id="8e589-119">A container named `waimportexport`, which is hello default container.</span></span> <span data-ttu-id="8e589-120">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8e589-120">For example:</span></span><br /><br /> `https://myaccount.blob.core.windows.net/waimportexport`|  
|<span data-ttu-id="8e589-121">Lagringskontonyckel</span><span class="sxs-lookup"><span data-stu-id="8e589-121">Storage account key</span></span>|<span data-ttu-id="8e589-122">Värdet som angetts av användaren</span><span class="sxs-lookup"><span data-stu-id="8e589-122">User-specified value</span></span>|<span data-ttu-id="8e589-123">En behållare med namnet av hello användare.</span><span class="sxs-lookup"><span data-stu-id="8e589-123">A container named by hello user.</span></span> <span data-ttu-id="8e589-124">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8e589-124">For example:</span></span><br /><br /> `https://myaccount.blob.core.windows.net/mylogcontainer`|  
|<span data-ttu-id="8e589-125">Behållaren SAS</span><span class="sxs-lookup"><span data-stu-id="8e589-125">Container SAS</span></span>|<span data-ttu-id="8e589-126">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="8e589-126">Default value</span></span>|<span data-ttu-id="8e589-127">En virtuell katalog med namnet `waimportexport`, vilket är hello för standardnamnet under hello-behållaren som angetts i hello SAS.</span><span class="sxs-lookup"><span data-stu-id="8e589-127">A virtual directory named `waimportexport`, which is hello default name, beneath hello container specified in hello SAS.</span></span><br /><br /> <span data-ttu-id="8e589-128">Till exempel om hello SAS för hello jobbet är `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, hello loggplatsen skulle vara`https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`</span><span class="sxs-lookup"><span data-stu-id="8e589-128">For example, if hello SAS specified for hello job is  `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, then hello log location would be `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`</span></span>|  
|<span data-ttu-id="8e589-129">Behållaren SAS</span><span class="sxs-lookup"><span data-stu-id="8e589-129">Container SAS</span></span>|<span data-ttu-id="8e589-130">Värdet som angetts av användaren</span><span class="sxs-lookup"><span data-stu-id="8e589-130">User-specified value</span></span>|<span data-ttu-id="8e589-131">En virtuell katalog med namnet av hello användare under hello-behållaren som angetts i hello SAS.</span><span class="sxs-lookup"><span data-stu-id="8e589-131">A virtual directory named by hello user, beneath hello container specified in hello SAS.</span></span><br /><br /> <span data-ttu-id="8e589-132">Till exempel om hello SAS för hello jobbet är `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, och hello angivna virtuella katalogen heter `mylogblobs`, hello loggplatsen skulle vara `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.</span><span class="sxs-lookup"><span data-stu-id="8e589-132">For example, if hello SAS specified for hello job is  `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, and hello specified virtual directory is named `mylogblobs`, then hello log location would be `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.</span></span>|  
  
<span data-ttu-id="8e589-133">Du kan hämta hello URL: en för hello fel och utförlig loggar genom att anropa hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) igen.</span><span class="sxs-lookup"><span data-stu-id="8e589-133">You can retrieve hello URL for hello error and verbose logs by calling hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="8e589-134">hello loggar är tillgängliga när bearbetningen av hello enheten är klar.</span><span class="sxs-lookup"><span data-stu-id="8e589-134">hello logs are available after processing of hello drive is complete.</span></span>  
  
## <a name="log-file-format"></a><span data-ttu-id="8e589-135">Loggfilsformat</span><span class="sxs-lookup"><span data-stu-id="8e589-135">Log file format</span></span>  
<span data-ttu-id="8e589-136">hello format för båda loggarna är hello samma: en blob som innehåller XML-beskrivningar av hello händelser som inträffade vid kopiering av BLOB mellan hello hårddisken och hello kundens konto.</span><span class="sxs-lookup"><span data-stu-id="8e589-136">hello format for both logs is hello same: a blob containing XML descriptions of hello events that occurred while copying blobs between hello hard drive and hello customer's account.</span></span>  
  
<span data-ttu-id="8e589-137">hello utförlig loggen innehåller fullständig information om status för hello hello kopieringsåtgärden för varje blob (för ett importjobb) eller filen (för ett exportjobb), hello felloggen innehåller endast hello information för blobbar eller filer som påträffade fel under hello Importera eller exportera jobb.</span><span class="sxs-lookup"><span data-stu-id="8e589-137">hello verbose log contains complete information about hello status of hello copy operation for every blob (for an import job) or file (for an export job), whereas hello error log contains only hello information for blobs or files that encountered errors during hello import or export job.</span></span>  
  
<span data-ttu-id="8e589-138">hello utförlig loggformatet visas nedan.</span><span class="sxs-lookup"><span data-stu-id="8e589-138">hello verbose log format is shown below.</span></span> <span data-ttu-id="8e589-139">hello felloggen har hello samma struktur, men filtrerar ut lyckade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="8e589-139">hello error log has hello same structure, but filters out successful operations.</span></span>  

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

<span data-ttu-id="8e589-140">hello beskrivs följande tabell hello element i hello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="8e589-140">hello following table describes hello elements of hello log file.</span></span>  
  
|<span data-ttu-id="8e589-141">XML-Element</span><span class="sxs-lookup"><span data-stu-id="8e589-141">XML Element</span></span>|<span data-ttu-id="8e589-142">Typ</span><span class="sxs-lookup"><span data-stu-id="8e589-142">Type</span></span>|<span data-ttu-id="8e589-143">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e589-143">Description</span></span>|  
|-----------------|----------|-----------------|  
|`DriveLog`|<span data-ttu-id="8e589-144">XML-Element</span><span class="sxs-lookup"><span data-stu-id="8e589-144">XML Element</span></span>|<span data-ttu-id="8e589-145">Representerar en logg för enheten.</span><span class="sxs-lookup"><span data-stu-id="8e589-145">Represents a drive log.</span></span>|  
|`Version`|<span data-ttu-id="8e589-146">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-146">Attribute, String</span></span>|<span data-ttu-id="8e589-147">hello version av hello loggformat.</span><span class="sxs-lookup"><span data-stu-id="8e589-147">hello version of hello log format.</span></span>|  
|`DriveId`|<span data-ttu-id="8e589-148">Sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-148">String</span></span>|<span data-ttu-id="8e589-149">Hej enhetens serienummer för maskinvara.</span><span class="sxs-lookup"><span data-stu-id="8e589-149">hello drive's hardware serial number.</span></span>|  
|`Status`|<span data-ttu-id="8e589-150">Sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-150">String</span></span>|<span data-ttu-id="8e589-151">Status för hello enhet bearbetning.</span><span class="sxs-lookup"><span data-stu-id="8e589-151">Status of hello drive processing.</span></span> <span data-ttu-id="8e589-152">Se hello `Drive Status Codes` tabellen nedan för mer information.</span><span class="sxs-lookup"><span data-stu-id="8e589-152">See hello `Drive Status Codes` table below for more information.</span></span>|  
|`Blob`|<span data-ttu-id="8e589-153">Kapslade XML-element</span><span class="sxs-lookup"><span data-stu-id="8e589-153">Nested XML element</span></span>|<span data-ttu-id="8e589-154">Representerar en blob.</span><span class="sxs-lookup"><span data-stu-id="8e589-154">Represents a blob.</span></span>|  
|`Blob/BlobPath`|<span data-ttu-id="8e589-155">Sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-155">String</span></span>|<span data-ttu-id="8e589-156">hello hello blob-URI.</span><span class="sxs-lookup"><span data-stu-id="8e589-156">hello URI of hello blob.</span></span>|  
|`Blob/FilePath`|<span data-ttu-id="8e589-157">Sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-157">String</span></span>|<span data-ttu-id="8e589-158">hello relativ sökväg toohello-hello enheten.</span><span class="sxs-lookup"><span data-stu-id="8e589-158">hello relative path toohello file on hello drive.</span></span>|  
|`Blob/Snapshot`|<span data-ttu-id="8e589-159">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="8e589-159">DateTime</span></span>|<span data-ttu-id="8e589-160">hello ögonblicksbild version av hello blob för ett exportjobb endast.</span><span class="sxs-lookup"><span data-stu-id="8e589-160">hello snapshot version of hello blob, for an export job only.</span></span>|  
|`Blob/Length`|<span data-ttu-id="8e589-161">Integer</span><span class="sxs-lookup"><span data-stu-id="8e589-161">Integer</span></span>|<span data-ttu-id="8e589-162">hello totala längden för hello blob i byte.</span><span class="sxs-lookup"><span data-stu-id="8e589-162">hello total length of hello blob in bytes.</span></span>|  
|`Blob/LastModified`|<span data-ttu-id="8e589-163">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="8e589-163">DateTime</span></span>|<span data-ttu-id="8e589-164">hello tidsvärdet hello blobben senast ändrades för ett exportjobb endast.</span><span class="sxs-lookup"><span data-stu-id="8e589-164">hello date/time that hello blob was last modified, for an export job only.</span></span>|  
|`Blob/ImportDisposition`|<span data-ttu-id="8e589-165">Sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-165">String</span></span>|<span data-ttu-id="8e589-166">hello importera disposition av hello blob för en importjobb.</span><span class="sxs-lookup"><span data-stu-id="8e589-166">hello import disposition of hello blob, for an import job only.</span></span>|  
|`Blob/ImportDisposition/@Status`|<span data-ttu-id="8e589-167">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-167">Attribute, String</span></span>|<span data-ttu-id="8e589-168">hello status för hello importera disposition.</span><span class="sxs-lookup"><span data-stu-id="8e589-168">hello status of hello import disposition.</span></span>|  
|`PageRangeList`|<span data-ttu-id="8e589-169">Kapslade XML-element</span><span class="sxs-lookup"><span data-stu-id="8e589-169">Nested XML element</span></span>|<span data-ttu-id="8e589-170">Representerar en lista över sidintervall för en sidblobb.</span><span class="sxs-lookup"><span data-stu-id="8e589-170">Represents a list of page ranges for a page blob.</span></span>|  
|`PageRange`|<span data-ttu-id="8e589-171">XML-element</span><span class="sxs-lookup"><span data-stu-id="8e589-171">XML element</span></span>|<span data-ttu-id="8e589-172">Representerar ett sidintervall.</span><span class="sxs-lookup"><span data-stu-id="8e589-172">Represents a page range.</span></span>|  
|`PageRange/@Offset`|<span data-ttu-id="8e589-173">Attributet heltal</span><span class="sxs-lookup"><span data-stu-id="8e589-173">Attribute, Integer</span></span>|<span data-ttu-id="8e589-174">Startar förskjutningen för hello sidintervall i hello-blob.</span><span class="sxs-lookup"><span data-stu-id="8e589-174">Starting offset of hello page range in hello blob.</span></span>|  
|`PageRange/@Length`|<span data-ttu-id="8e589-175">Attributet heltal</span><span class="sxs-lookup"><span data-stu-id="8e589-175">Attribute, Integer</span></span>|<span data-ttu-id="8e589-176">Längden i byte på hello sidintervallet.</span><span class="sxs-lookup"><span data-stu-id="8e589-176">Length in bytes of hello page range.</span></span>|  
|`PageRange/@Hash`|<span data-ttu-id="8e589-177">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-177">Attribute, String</span></span>|<span data-ttu-id="8e589-178">Base16-kodade MD5-hash för hello sidintervallet.</span><span class="sxs-lookup"><span data-stu-id="8e589-178">Base16-encoded MD5 hash of hello page range.</span></span>|  
|`PageRange/@Status`|<span data-ttu-id="8e589-179">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-179">Attribute, String</span></span>|<span data-ttu-id="8e589-180">Status för bearbetning av hello sidintervallet.</span><span class="sxs-lookup"><span data-stu-id="8e589-180">Status of processing hello page range.</span></span>|  
|`BlockList`|<span data-ttu-id="8e589-181">Kapslade XML-element</span><span class="sxs-lookup"><span data-stu-id="8e589-181">Nested XML element</span></span>|<span data-ttu-id="8e589-182">Representerar en lista över block för en blockblob.</span><span class="sxs-lookup"><span data-stu-id="8e589-182">Represents a list of blocks for a block blob.</span></span>|  
|`Block`|<span data-ttu-id="8e589-183">XML-element</span><span class="sxs-lookup"><span data-stu-id="8e589-183">XML element</span></span>|<span data-ttu-id="8e589-184">Representerar ett block.</span><span class="sxs-lookup"><span data-stu-id="8e589-184">Represents a block.</span></span>|  
|`Block/@Offset`|<span data-ttu-id="8e589-185">Attributet heltal</span><span class="sxs-lookup"><span data-stu-id="8e589-185">Attribute, Integer</span></span>|<span data-ttu-id="8e589-186">Första förskjutningen av hello block i hello-blob.</span><span class="sxs-lookup"><span data-stu-id="8e589-186">Starting offset of hello block in hello blob.</span></span>|  
|`Block/@Length`|<span data-ttu-id="8e589-187">Attributet heltal</span><span class="sxs-lookup"><span data-stu-id="8e589-187">Attribute, Integer</span></span>|<span data-ttu-id="8e589-188">Längden i byte på hello-block.</span><span class="sxs-lookup"><span data-stu-id="8e589-188">Length in bytes of hello block.</span></span>|  
|`Block/@Id`|<span data-ttu-id="8e589-189">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-189">Attribute, String</span></span>|<span data-ttu-id="8e589-190">hello block-ID.</span><span class="sxs-lookup"><span data-stu-id="8e589-190">hello block ID.</span></span>|  
|`Block/@Hash`|<span data-ttu-id="8e589-191">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-191">Attribute, String</span></span>|<span data-ttu-id="8e589-192">Base16-kodade MD5-hash för hello-block.</span><span class="sxs-lookup"><span data-stu-id="8e589-192">Base16-encoded MD5 hash of hello block.</span></span>|  
|`Block/@Status`|<span data-ttu-id="8e589-193">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-193">Attribute, String</span></span>|<span data-ttu-id="8e589-194">Status för bearbetning av hello-block.</span><span class="sxs-lookup"><span data-stu-id="8e589-194">Status of processing hello block.</span></span>|  
|`Metadata`|<span data-ttu-id="8e589-195">Kapslade XML-element</span><span class="sxs-lookup"><span data-stu-id="8e589-195">Nested XML element</span></span>|<span data-ttu-id="8e589-196">Representerar metadata för hello blob.</span><span class="sxs-lookup"><span data-stu-id="8e589-196">Represents hello blob's metadata.</span></span>|  
|`Metadata/@Status`|<span data-ttu-id="8e589-197">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-197">Attribute, String</span></span>|<span data-ttu-id="8e589-198">Status för bearbetning av hello blobbmetadata.</span><span class="sxs-lookup"><span data-stu-id="8e589-198">Status of processing of hello blob metadata.</span></span>|  
|`Metadata/GlobalPath`|<span data-ttu-id="8e589-199">Sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-199">String</span></span>|<span data-ttu-id="8e589-200">Relativ sökväg toohello globala metadatafil.</span><span class="sxs-lookup"><span data-stu-id="8e589-200">Relative path toohello global metadata file.</span></span>|  
|`Metadata/GlobalPath/@Hash`|<span data-ttu-id="8e589-201">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-201">Attribute, String</span></span>|<span data-ttu-id="8e589-202">Base16-kodade MD5-hash för hello globala metadatafil.</span><span class="sxs-lookup"><span data-stu-id="8e589-202">Base16-encoded MD5 hash of hello global metadata file.</span></span>|  
|`Metadata/Path`|<span data-ttu-id="8e589-203">Sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-203">String</span></span>|<span data-ttu-id="8e589-204">Relativ sökväg toohello metadatafil.</span><span class="sxs-lookup"><span data-stu-id="8e589-204">Relative path toohello metadata file.</span></span>|  
|`Metadata/Path/@Hash`|<span data-ttu-id="8e589-205">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-205">Attribute, String</span></span>|<span data-ttu-id="8e589-206">Base16-kodade MD5-hash för hello metadatafil.</span><span class="sxs-lookup"><span data-stu-id="8e589-206">Base16-encoded MD5 hash of hello metadata file.</span></span>|  
|`Properties`|<span data-ttu-id="8e589-207">Kapslade XML-element</span><span class="sxs-lookup"><span data-stu-id="8e589-207">Nested XML element</span></span>|<span data-ttu-id="8e589-208">Representerar hello blob-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8e589-208">Represents hello blob properties.</span></span>|  
|`Properties/@Status`|<span data-ttu-id="8e589-209">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-209">Attribute, String</span></span>|<span data-ttu-id="8e589-210">Status för bearbetning av hello blob-egenskaper, t.ex. inte att hitta filen, slutförts.</span><span class="sxs-lookup"><span data-stu-id="8e589-210">Status of processing hello blob properties, e.g. file not found, completed.</span></span>|  
|`Properties/GlobalPath`|<span data-ttu-id="8e589-211">Sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-211">String</span></span>|<span data-ttu-id="8e589-212">Relativ sökväg toohello globala egenskaper för filen.</span><span class="sxs-lookup"><span data-stu-id="8e589-212">Relative path toohello global properties file.</span></span>|  
|`Properties/GlobalPath/@Hash`|<span data-ttu-id="8e589-213">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-213">Attribute, String</span></span>|<span data-ttu-id="8e589-214">Base16-kodade MD5-hash för hello globala egenskaper för filen.</span><span class="sxs-lookup"><span data-stu-id="8e589-214">Base16-encoded MD5 hash of hello global properties file.</span></span>|  
|`Properties/Path`|<span data-ttu-id="8e589-215">Sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-215">String</span></span>|<span data-ttu-id="8e589-216">Relativ sökväg toohello egenskaper för filen.</span><span class="sxs-lookup"><span data-stu-id="8e589-216">Relative path toohello properties file.</span></span>|  
|`Properties/Path/@Hash`|<span data-ttu-id="8e589-217">Attributet sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-217">Attribute, String</span></span>|<span data-ttu-id="8e589-218">Base16-kodade MD5-hash för hello egenskapsfil.</span><span class="sxs-lookup"><span data-stu-id="8e589-218">Base16-encoded MD5 hash of hello properties file.</span></span>|  
|`Blob/Status`|<span data-ttu-id="8e589-219">Sträng</span><span class="sxs-lookup"><span data-stu-id="8e589-219">String</span></span>|<span data-ttu-id="8e589-220">Status för bearbetning av hello-blob.</span><span class="sxs-lookup"><span data-stu-id="8e589-220">Status of processing hello blob.</span></span>|  
  
# <a name="drive-status-codes"></a><span data-ttu-id="8e589-221">Statuskoder för enhet</span><span class="sxs-lookup"><span data-stu-id="8e589-221">Drive status codes</span></span>  
<span data-ttu-id="8e589-222">hello visar följande tabell hello statuskoder för bearbetning av en enhet.</span><span class="sxs-lookup"><span data-stu-id="8e589-222">hello following table lists hello status codes for processing a drive.</span></span>  
  
|<span data-ttu-id="8e589-223">Statuskod</span><span class="sxs-lookup"><span data-stu-id="8e589-223">Status code</span></span>|<span data-ttu-id="8e589-224">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e589-224">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="8e589-225">hello enhet har bearbetat utan fel.</span><span class="sxs-lookup"><span data-stu-id="8e589-225">hello drive has finished processing without any errors.</span></span>|  
|`CompletedWithWarnings`|<span data-ttu-id="8e589-226">hello enheten har slutförts med varningar i en eller flera blobbar per hello importera disposition som angetts för hello-BLOB.</span><span class="sxs-lookup"><span data-stu-id="8e589-226">hello drive has finished processing with warnings in one or more blobs per hello import dispositions specified for hello blobs.</span></span>|  
|`CompletedWithErrors`|<span data-ttu-id="8e589-227">hello enheten har slutförts med fel i en eller flera blobbar segment.</span><span class="sxs-lookup"><span data-stu-id="8e589-227">hello drive has finished with errors in one or more blobs or chunks.</span></span>|  
|`DiskNotFound`|<span data-ttu-id="8e589-228">Ingen disk finns på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="8e589-228">No disk is found on hello drive.</span></span>|  
|`VolumeNotNtfs`|<span data-ttu-id="8e589-229">hello första datavolym på hello disk är inte i NTFS-format.</span><span class="sxs-lookup"><span data-stu-id="8e589-229">hello first data volume on hello disk is not in NTFS format.</span></span>|  
|`DiskOperationFailed`|<span data-ttu-id="8e589-230">Ett okänt fel uppstod när du utför åtgärder på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="8e589-230">An unknown failure occurred when performing operations on hello drive.</span></span>|  
|`BitLockerVolumeNotFound`|<span data-ttu-id="8e589-231">Ingen encryptable volym BitLocker finns.</span><span class="sxs-lookup"><span data-stu-id="8e589-231">No BitLocker encryptable volume is found.</span></span>|  
|`BitLockerNotActivated`|<span data-ttu-id="8e589-232">BitLocker har inte aktiverats på hello volym.</span><span class="sxs-lookup"><span data-stu-id="8e589-232">BitLocker is not enabled on hello volume.</span></span>|  
|`BitLockerProtectorNotFound`|<span data-ttu-id="8e589-233">nyckelskydd för hello numeriskt lösenord finns inte på hello volym.</span><span class="sxs-lookup"><span data-stu-id="8e589-233">hello numerical password key protector does not exist on hello volume.</span></span>|  
|`BitLockerKeyInvalid`|<span data-ttu-id="8e589-234">hello numeriskt lösenord angavs inte låsa upp hello volym.</span><span class="sxs-lookup"><span data-stu-id="8e589-234">hello numerical password provided cannot unlock hello volume.</span></span>|  
|`BitLockerUnlockVolumeFailed`|<span data-ttu-id="8e589-235">Okänt fel uppstod vid försök toounlock hello volym.</span><span class="sxs-lookup"><span data-stu-id="8e589-235">Unknown failure has happened when trying toounlock hello volume.</span></span>|  
|`BitLockerFailed`|<span data-ttu-id="8e589-236">Ett okänt fel uppstod vid BitLocker-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="8e589-236">An unknown failure occurred while performing BitLocker operations.</span></span>|  
|`ManifestNameInvalid`|<span data-ttu-id="8e589-237">hello Manifestfilens filnamn är ogiltigt.</span><span class="sxs-lookup"><span data-stu-id="8e589-237">hello manifest file name is invalid.</span></span>|  
|`ManifestNameTooLong`|<span data-ttu-id="8e589-238">hello Manifestfilens filnamn är för långt.</span><span class="sxs-lookup"><span data-stu-id="8e589-238">hello manifest file name is too long.</span></span>|  
|`ManifestNotFound`|<span data-ttu-id="8e589-239">hello manifestfilen hittades inte.</span><span class="sxs-lookup"><span data-stu-id="8e589-239">hello manifest file is not found.</span></span>|  
|`ManifestAccessDenied`|<span data-ttu-id="8e589-240">Åtkomst toohello manifestfilen nekas.</span><span class="sxs-lookup"><span data-stu-id="8e589-240">Access toohello manifest file is denied.</span></span>|  
|`ManifestCorrupted`|<span data-ttu-id="8e589-241">hello manifest-filen är skadad (hello innehållet matchar inte dess hash).</span><span class="sxs-lookup"><span data-stu-id="8e589-241">hello manifest file is corrupted (hello content does not match its hash).</span></span>|  
|`ManifestFormatInvalid`|<span data-ttu-id="8e589-242">Manifestet hello-innehåll följer inte toohello format som krävs.</span><span class="sxs-lookup"><span data-stu-id="8e589-242">hello manifest content does not conform toohello required format.</span></span>|  
|`ManifestDriveIdMismatch`|<span data-ttu-id="8e589-243">hello enheten inte matchar ID i hello manifestfilen hello en lästes från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="8e589-243">hello drive ID in hello manifest file does not match hello one read from hello drive.</span></span>|  
|`ReadManifestFailed`|<span data-ttu-id="8e589-244">En disk-i/o-fel uppstod vid läsning från hello manifest.</span><span class="sxs-lookup"><span data-stu-id="8e589-244">A disk I/O failure occurred while reading from hello manifest.</span></span>|  
|`BlobListFormatInvalid`|<span data-ttu-id="8e589-245">hello export blob listan blob följer inte toohello format som krävs.</span><span class="sxs-lookup"><span data-stu-id="8e589-245">hello export blob list blob does not conform toohello required format.</span></span>|  
|`BlobRequestForbidden`|<span data-ttu-id="8e589-246">Få åtkomst till toohello blobbar i hello storage-konto är förbjuden.</span><span class="sxs-lookup"><span data-stu-id="8e589-246">Access toohello blobs in hello storage account is forbidden.</span></span> <span data-ttu-id="8e589-247">Detta kan bero på att tooinvalid lagringskontonyckel eller SAS-behållare.</span><span class="sxs-lookup"><span data-stu-id="8e589-247">This might be due tooinvalid storage account key or container SAS.</span></span>|  
|`InternalError`|<span data-ttu-id="8e589-248">Och internt fel uppstod under bearbetning av hello enhet.</span><span class="sxs-lookup"><span data-stu-id="8e589-248">And internal error occurred while processing hello drive.</span></span>|  
  
## <a name="blob-status-codes"></a><span data-ttu-id="8e589-249">BLOB-statuskoder</span><span class="sxs-lookup"><span data-stu-id="8e589-249">Blob status codes</span></span>  
<span data-ttu-id="8e589-250">hello visar följande tabell hello statuskoder för bearbetning av en blob.</span><span class="sxs-lookup"><span data-stu-id="8e589-250">hello following table lists hello status codes for processing a blob.</span></span>  
  
|<span data-ttu-id="8e589-251">Statuskod</span><span class="sxs-lookup"><span data-stu-id="8e589-251">Status code</span></span>|<span data-ttu-id="8e589-252">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e589-252">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="8e589-253">hello-blob har bearbetat utan fel.</span><span class="sxs-lookup"><span data-stu-id="8e589-253">hello blob has finished processing without errors.</span></span>|  
|`CompletedWithErrors`|<span data-ttu-id="8e589-254">hello-blob har bearbetat med fel i en eller flera sidintervall block, metadata eller egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8e589-254">hello blob has finished processing with errors in one or more page ranges or blocks, metadata, or properties.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="8e589-255">hello-filnamnet är ogiltigt.</span><span class="sxs-lookup"><span data-stu-id="8e589-255">hello file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="8e589-256">hello-filnamnet är för långt.</span><span class="sxs-lookup"><span data-stu-id="8e589-256">hello file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="8e589-257">hello-filen hittades inte.</span><span class="sxs-lookup"><span data-stu-id="8e589-257">hello file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="8e589-258">Åtkomst toohello filen nekades.</span><span class="sxs-lookup"><span data-stu-id="8e589-258">Access toohello file is denied.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="8e589-259">Det gick inte att hello Blob tjänstbegäran tooaccess hello blob.</span><span class="sxs-lookup"><span data-stu-id="8e589-259">hello Blob service request tooaccess hello blob has failed.</span></span>|  
|`BlobRequestForbidden`|<span data-ttu-id="8e589-260">hello Blob tjänstbegäran tooaccess hello blob tillåts inte.</span><span class="sxs-lookup"><span data-stu-id="8e589-260">hello Blob service request tooaccess hello blob is forbidden.</span></span> <span data-ttu-id="8e589-261">Detta kan bero på att tooinvalid lagringskontonyckel eller SAS-behållare.</span><span class="sxs-lookup"><span data-stu-id="8e589-261">This might be due tooinvalid storage account key or container SAS.</span></span>|  
|`RenameFailed`|<span data-ttu-id="8e589-262">Det gick inte toorename hello blob (för ett importjobb) eller hello-filen (för ett exportjobb).</span><span class="sxs-lookup"><span data-stu-id="8e589-262">Failed toorename hello blob (for an import job) or hello file (for an export job).</span></span>|  
|`BlobUnexpectedChange`|<span data-ttu-id="8e589-263">En oväntad ändring har uppstått med hello blob (för ett exportjobb).</span><span class="sxs-lookup"><span data-stu-id="8e589-263">An unexpected change has occurred with hello blob (for an export job).</span></span>|  
|`LeasePresent`|<span data-ttu-id="8e589-264">Det finns ett lån som finns på hello-blob.</span><span class="sxs-lookup"><span data-stu-id="8e589-264">There is a lease present on hello blob.</span></span>|  
|`IOFailed`|<span data-ttu-id="8e589-265">En disk eller nätverk i/o-fel uppstod under bearbetning av hello blob.</span><span class="sxs-lookup"><span data-stu-id="8e589-265">A disk or network I/O failure occurred while processing hello blob.</span></span>|  
|`Failed`|<span data-ttu-id="8e589-266">Ett okänt fel uppstod under bearbetning av hello-blob.</span><span class="sxs-lookup"><span data-stu-id="8e589-266">An unknown failure occurred while processing hello blob.</span></span>|  
  
## <a name="import-disposition-status-codes"></a><span data-ttu-id="8e589-267">Importera disposition statuskoder</span><span class="sxs-lookup"><span data-stu-id="8e589-267">Import disposition status codes</span></span>  
<span data-ttu-id="8e589-268">hello visar följande tabell hello statuskoder för att lösa ett import-disposition.</span><span class="sxs-lookup"><span data-stu-id="8e589-268">hello following table lists hello status codes for resolving an import disposition.</span></span>  
  
|<span data-ttu-id="8e589-269">Statuskod</span><span class="sxs-lookup"><span data-stu-id="8e589-269">Status code</span></span>|<span data-ttu-id="8e589-270">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e589-270">Description</span></span>|  
|-----------------|-----------------|  
|`Created`|<span data-ttu-id="8e589-271">hello-blob har skapats.</span><span class="sxs-lookup"><span data-stu-id="8e589-271">hello blob has been created.</span></span>|  
|`Renamed`|<span data-ttu-id="8e589-272">hello-blob har bytt per Byt namn på import disposition.</span><span class="sxs-lookup"><span data-stu-id="8e589-272">hello blob has been renamed per rename import disposition.</span></span> <span data-ttu-id="8e589-273">Hej `Blob/BlobPath` elementet innehåller hello URI för hello byta namn på blob.</span><span class="sxs-lookup"><span data-stu-id="8e589-273">hello `Blob/BlobPath` element contains hello URI for hello renamed blob.</span></span>|  
|`Skipped`|<span data-ttu-id="8e589-274">hello-blob har hoppats över `no-overwrite` importera disposition.</span><span class="sxs-lookup"><span data-stu-id="8e589-274">hello blob has been skipped per `no-overwrite` import disposition.</span></span>|  
|`Overwritten`|<span data-ttu-id="8e589-275">hello-blob har skrivit över en befintlig blob per `overwrite` importera disposition.</span><span class="sxs-lookup"><span data-stu-id="8e589-275">hello blob has overwritten an existing blob per `overwrite` import disposition.</span></span>|  
|`Cancelled`|<span data-ttu-id="8e589-276">Ett tidigare fel har stoppats vidare bearbetning av hello importera disposition.</span><span class="sxs-lookup"><span data-stu-id="8e589-276">A prior failure has stopped further processing of hello import disposition.</span></span>|  
  
## <a name="page-rangeblock-status-codes"></a><span data-ttu-id="8e589-277">Statuskoder för sidan intervall /-block</span><span class="sxs-lookup"><span data-stu-id="8e589-277">Page range/block status codes</span></span>  
<span data-ttu-id="8e589-278">hello visar följande tabell hello statuskoder för bearbetning av ett sidintervall eller ett block.</span><span class="sxs-lookup"><span data-stu-id="8e589-278">hello following table lists hello status codes for processing a page range or a block.</span></span>  
  
|<span data-ttu-id="8e589-279">Statuskod</span><span class="sxs-lookup"><span data-stu-id="8e589-279">Status code</span></span>|<span data-ttu-id="8e589-280">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e589-280">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="8e589-281">hello sidan intervall eller block har bearbetat utan fel.</span><span class="sxs-lookup"><span data-stu-id="8e589-281">hello page range or block has finished processing without any errors.</span></span>|  
|`Committed`|<span data-ttu-id="8e589-282">hello block som har allokerats, men inte i hello fullständig Blockeringslista eftersom andra block har misslyckats eller placera fullständig Blockeringslista själva har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="8e589-282">hello block has been committed,  but not in hello full block list because other blocks have failed, or put full block list itself has failed.</span></span>|  
|`Uncommitted`|<span data-ttu-id="8e589-283">hello block överförs men inte allokerad.</span><span class="sxs-lookup"><span data-stu-id="8e589-283">hello block is uploaded but not committed.</span></span>|  
|`Corrupted`|<span data-ttu-id="8e589-284">hello sidan intervall eller block är skadad (hello innehållet matchar inte dess hash).</span><span class="sxs-lookup"><span data-stu-id="8e589-284">hello page range or block is corrupted (hello content does not match its hash).</span></span>|  
|`FileUnexpectedEnd`|<span data-ttu-id="8e589-285">Ett oväntat filslut påträffades.</span><span class="sxs-lookup"><span data-stu-id="8e589-285">An unexpected end of file has been encountered.</span></span>|  
|`BlobUnexpectedEnd`|<span data-ttu-id="8e589-286">Det inträffade ett oväntat slut på blob.</span><span class="sxs-lookup"><span data-stu-id="8e589-286">An unexpected end of blob has been encountered.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="8e589-287">Hej Blob tjänstbegäran tooaccess hello sidintervall eller blockera misslyckades.</span><span class="sxs-lookup"><span data-stu-id="8e589-287">hello Blob service request tooaccess hello page range or block has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="8e589-288">En disk eller nätverk i/o-fel inträffade vid bearbetning hello sidan intervall eller block.</span><span class="sxs-lookup"><span data-stu-id="8e589-288">A disk or network I/O failure occurred while processing hello page range or block.</span></span>|  
|`Failed`|<span data-ttu-id="8e589-289">Ett okänt fel uppstod under bearbetning av hello sidan intervall eller block.</span><span class="sxs-lookup"><span data-stu-id="8e589-289">An unknown failure occurred while processing hello page range or block.</span></span>|  
|`Cancelled`|<span data-ttu-id="8e589-290">Ett tidigare fel har stoppats vidare bearbetning av hello sidan intervall eller block.</span><span class="sxs-lookup"><span data-stu-id="8e589-290">A prior failure has stopped further processing of hello page range or block.</span></span>|  
  
## <a name="metadata-status-codes"></a><span data-ttu-id="8e589-291">Statuskoder för metadata</span><span class="sxs-lookup"><span data-stu-id="8e589-291">Metadata status codes</span></span>  
<span data-ttu-id="8e589-292">hello visar följande tabell hello statuskoder för bearbetning blob-metadata.</span><span class="sxs-lookup"><span data-stu-id="8e589-292">hello following table lists hello status codes for processing blob metadata.</span></span>  
  
|<span data-ttu-id="8e589-293">Statuskod</span><span class="sxs-lookup"><span data-stu-id="8e589-293">Status code</span></span>|<span data-ttu-id="8e589-294">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e589-294">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="8e589-295">hello metadata har bearbetat utan fel.</span><span class="sxs-lookup"><span data-stu-id="8e589-295">hello metadata has finished processing without errors.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="8e589-296">hello metadatafilnamnet är ogiltigt.</span><span class="sxs-lookup"><span data-stu-id="8e589-296">hello metadata file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="8e589-297">hello metadata-filnamnet är för långt.</span><span class="sxs-lookup"><span data-stu-id="8e589-297">hello metadata file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="8e589-298">hello metadatafil hittades inte.</span><span class="sxs-lookup"><span data-stu-id="8e589-298">hello metadata file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="8e589-299">Metadatafil toohello för åtkomst nekas.</span><span class="sxs-lookup"><span data-stu-id="8e589-299">Access toohello metadata file is denied.</span></span>|  
|`Corrupted`|<span data-ttu-id="8e589-300">hello metadatafil är skadad (hello innehållet matchar inte dess hash).</span><span class="sxs-lookup"><span data-stu-id="8e589-300">hello metadata file is corrupted (hello content does not match its hash).</span></span>|  
|`XmlReadFailed`|<span data-ttu-id="8e589-301">hello metadata innehåll följer inte toohello format som krävs.</span><span class="sxs-lookup"><span data-stu-id="8e589-301">hello metadata content does not conform toohello required format.</span></span>|  
|`XmlWriteFailed`|<span data-ttu-id="8e589-302">Skriva hello metadata XML misslyckades.</span><span class="sxs-lookup"><span data-stu-id="8e589-302">Writing hello metadata XML has failed.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="8e589-303">Det gick inte att hello Blob tjänstbegäran tooaccess hello metadata.</span><span class="sxs-lookup"><span data-stu-id="8e589-303">hello Blob service request tooaccess hello metadata has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="8e589-304">Ett disk- eller i/o-fel uppstod under bearbetning av hello metadata.</span><span class="sxs-lookup"><span data-stu-id="8e589-304">A disk or network I/O failure occurred while processing hello metadata.</span></span>|  
|`Failed`|<span data-ttu-id="8e589-305">Ett okänt fel uppstod under bearbetning av hello metadata.</span><span class="sxs-lookup"><span data-stu-id="8e589-305">An unknown failure occurred while processing hello metadata.</span></span>|  
|`Cancelled`|<span data-ttu-id="8e589-306">Ett tidigare fel har stoppats vidare bearbetning av hello metadata.</span><span class="sxs-lookup"><span data-stu-id="8e589-306">A prior failure has stopped further processing of hello metadata.</span></span>|  
  
## <a name="properties-status-codes"></a><span data-ttu-id="8e589-307">Statuskoder för egenskaper</span><span class="sxs-lookup"><span data-stu-id="8e589-307">Properties status codes</span></span>  
<span data-ttu-id="8e589-308">hello visar följande tabell hello statuskoder för bearbetning av blob-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8e589-308">hello following table lists hello status codes for processing blob properties.</span></span>  
  
|<span data-ttu-id="8e589-309">Statuskod</span><span class="sxs-lookup"><span data-stu-id="8e589-309">Status code</span></span>|<span data-ttu-id="8e589-310">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e589-310">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="8e589-311">hello egenskaper har slutfört bearbetningen utan fel.</span><span class="sxs-lookup"><span data-stu-id="8e589-311">hello properties have finished processing without any errors.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="8e589-312">hello egenskaper filnamnet är ogiltigt.</span><span class="sxs-lookup"><span data-stu-id="8e589-312">hello properties file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="8e589-313">hello egenskaper filnamnet är för långt.</span><span class="sxs-lookup"><span data-stu-id="8e589-313">hello properties file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="8e589-314">hello egenskaper filen hittades inte.</span><span class="sxs-lookup"><span data-stu-id="8e589-314">hello properties file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="8e589-315">Åtkomst till toohello egenskapsfil nekas.</span><span class="sxs-lookup"><span data-stu-id="8e589-315">Access toohello properties file is denied.</span></span>|  
|`Corrupted`|<span data-ttu-id="8e589-316">hello egenskaper för filen är skadad (hello innehållet matchar inte dess hash).</span><span class="sxs-lookup"><span data-stu-id="8e589-316">hello properties file is corrupted (hello content does not match its hash).</span></span>|  
|`XmlReadFailed`|<span data-ttu-id="8e589-317">hello egenskaper innehåll följer inte toohello format som krävs.</span><span class="sxs-lookup"><span data-stu-id="8e589-317">hello properties content does not conform toohello required format.</span></span>|  
|`XmlWriteFailed`|<span data-ttu-id="8e589-318">Skriver hello egenskaper XML misslyckades.</span><span class="sxs-lookup"><span data-stu-id="8e589-318">Writing hello properties XML has failed.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="8e589-319">Det gick inte att hello Blob tjänstbegäran tooaccess hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8e589-319">hello Blob service request tooaccess hello properties has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="8e589-320">Ett disk- eller i/o-fel uppstod under bearbetning av hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8e589-320">A disk or network I/O failure occurred while processing hello properties.</span></span>|  
|`Failed`|<span data-ttu-id="8e589-321">Ett okänt fel uppstod under bearbetning av hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8e589-321">An unknown failure occurred while processing hello properties.</span></span>|  
|`Cancelled`|<span data-ttu-id="8e589-322">Ett tidigare fel har stoppats vidare bearbetning av hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8e589-322">A prior failure has stopped further processing of hello properties.</span></span>|  
  
## <a name="sample-logs"></a><span data-ttu-id="8e589-323">Exempel loggar</span><span class="sxs-lookup"><span data-stu-id="8e589-323">Sample logs</span></span>  
<span data-ttu-id="8e589-324">hello följande är ett exempel på detaljerade loggen.</span><span class="sxs-lookup"><span data-stu-id="8e589-324">hello following is an example of verbose log.</span></span>  
  
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
  
<span data-ttu-id="8e589-325">hello motsvarande felloggen visas nedan.</span><span class="sxs-lookup"><span data-stu-id="8e589-325">hello corresponding error log is shown below.</span></span>  
  
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

 <span data-ttu-id="8e589-326">hello Följ felloggen för importen innehåller ett fel om en fil inte hittas på hello importera enhet.</span><span class="sxs-lookup"><span data-stu-id="8e589-326">hello follow error log for an import job contains an error about a file not found on hello import drive.</span></span> <span data-ttu-id="8e589-327">Observera att hello status för efterföljande komponenter är `Cancelled`.</span><span class="sxs-lookup"><span data-stu-id="8e589-327">Note that hello status of subsequent components is `Cancelled`.</span></span>  
  
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

<span data-ttu-id="8e589-328">hello visar följande felloggen för ett exportjobb att hello blob-innehåll har skrivits toohello enheten utan att ett fel inträffade vid export av hello blob-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8e589-328">hello following error log for an export job indicates that hello blob content has been successfully written toohello drive, but that an error occurred while exporting hello blob's properties.</span></span>  
  
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
  
## <a name="next-steps"></a><span data-ttu-id="8e589-329">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8e589-329">Next steps</span></span>
 
* [<span data-ttu-id="8e589-330">REST-API för Storage Import/Export</span><span class="sxs-lookup"><span data-stu-id="8e589-330">Storage Import/Export REST API</span></span>](/rest/api/storageimportexport/)
