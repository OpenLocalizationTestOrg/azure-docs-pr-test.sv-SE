---
title: "Azure Import/Export metadata och egenskaper för nytt filformat | Microsoft Docs"
description: "Lär dig hur du anger metadata och egenskaperna för en eller flera blobbar som är en del av en importera eller exportera jobb."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 840364c6-d9a8-4b43-a9f3-f7441c625069
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 3f728ad94cdcbd32092b677f11a737ae91376720
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a><span data-ttu-id="51896-103">Azure Import/Export service metadata och egenskaper filformat</span><span class="sxs-lookup"><span data-stu-id="51896-103">Azure Import/Export service metadata and properties file format</span></span>
<span data-ttu-id="51896-104">Du kan ange egenskaper för en eller flera blobbar och metadata som en del av ett importjobb eller ett exportjobb.</span><span class="sxs-lookup"><span data-stu-id="51896-104">You can specify metadata and properties for one or more blobs as part of an import job or an export job.</span></span> <span data-ttu-id="51896-105">Om du vill ange egenskaper för blob som skapas som en del av ett importjobb eller metadata, kan du ange en egenskaper eller metadata fil på hårddisken som innehåller data som ska importeras.</span><span class="sxs-lookup"><span data-stu-id="51896-105">To set metadata or properties for blobs being created as part of an import job, you provide a metadata or properties file on the hard drive containing the data to be imported.</span></span> <span data-ttu-id="51896-106">För ett exportjobb skrivs metadata och egenskaper till en fil för metadata eller egenskaper som ingår på hårddisken till dig.</span><span class="sxs-lookup"><span data-stu-id="51896-106">For an export job, metadata and properties are written to a metadata or properties file that is included on the hard drive returned to you.</span></span>  
  
## <a name="metadata-file-format"></a><span data-ttu-id="51896-107">Filformat för metadata</span><span class="sxs-lookup"><span data-stu-id="51896-107">Metadata file format</span></span>  
<span data-ttu-id="51896-108">Formatet för en metadatafil är följande:</span><span class="sxs-lookup"><span data-stu-id="51896-108">The format of a metadata file is as follows:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|<span data-ttu-id="51896-109">XML-Element</span><span class="sxs-lookup"><span data-stu-id="51896-109">XML Element</span></span>|<span data-ttu-id="51896-110">Typ</span><span class="sxs-lookup"><span data-stu-id="51896-110">Type</span></span>|<span data-ttu-id="51896-111">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="51896-111">Description</span></span>|  
|-----------------|----------|-----------------|  
|`Metadata`|<span data-ttu-id="51896-112">Rotelementet</span><span class="sxs-lookup"><span data-stu-id="51896-112">Root element</span></span>|<span data-ttu-id="51896-113">Rotelementet i metadatafilen.</span><span class="sxs-lookup"><span data-stu-id="51896-113">The root element of the metadata file.</span></span>|  
|`metadata-name`|<span data-ttu-id="51896-114">Sträng</span><span class="sxs-lookup"><span data-stu-id="51896-114">String</span></span>|<span data-ttu-id="51896-115">Valfri.</span><span class="sxs-lookup"><span data-stu-id="51896-115">Optional.</span></span> <span data-ttu-id="51896-116">XML-elementet anger namnet på metadata för blob och dess värde anger värdet för inställningen metadata.</span><span class="sxs-lookup"><span data-stu-id="51896-116">The XML element specifies the name of the metadata for the blob, and its value specifies the value of the metadata setting.</span></span>|  
  
## <a name="properties-file-format"></a><span data-ttu-id="51896-117">Filformatet för egenskaper</span><span class="sxs-lookup"><span data-stu-id="51896-117">Properties file format</span></span>  
<span data-ttu-id="51896-118">Formatet för en egenskapsfil är följande:</span><span class="sxs-lookup"><span data-stu-id="51896-118">The format of a properties file is as follows:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
[<Last-Modified>date-time-value</Last-Modified>]  
[<Etag>etag</Etag>]  
[<Content-Length>size-in-bytes<Content-Length>]  
[<Content-Type>content-type</Content-Type>]  
[<Content-MD5>content-md5</Content-MD5>]  
[<Content-Encoding>content-encoding</Content-Encoding>]  
[<Content-Language>content-language</Content-Language>]  
[<Cache-Control>cache-control</Cache-Control>]  
</Properties>  
```
  
|<span data-ttu-id="51896-119">XML-Element</span><span class="sxs-lookup"><span data-stu-id="51896-119">XML Element</span></span>|<span data-ttu-id="51896-120">Typ</span><span class="sxs-lookup"><span data-stu-id="51896-120">Type</span></span>|<span data-ttu-id="51896-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="51896-121">Description</span></span>|  
|-----------------|----------|-----------------|  
|`Properties`|<span data-ttu-id="51896-122">Rotelementet</span><span class="sxs-lookup"><span data-stu-id="51896-122">Root element</span></span>|<span data-ttu-id="51896-123">Rotelementet i filen egenskaper.</span><span class="sxs-lookup"><span data-stu-id="51896-123">The root element of the properties file.</span></span>|  
|`Last-Modified`|<span data-ttu-id="51896-124">Sträng</span><span class="sxs-lookup"><span data-stu-id="51896-124">String</span></span>|<span data-ttu-id="51896-125">Valfri.</span><span class="sxs-lookup"><span data-stu-id="51896-125">Optional.</span></span> <span data-ttu-id="51896-126">Den senast ändrade tiden för blob.</span><span class="sxs-lookup"><span data-stu-id="51896-126">The last-modified time for the blob.</span></span> <span data-ttu-id="51896-127">För exportjobb.</span><span class="sxs-lookup"><span data-stu-id="51896-127">For export jobs only.</span></span>|  
|`Etag`|<span data-ttu-id="51896-128">Sträng</span><span class="sxs-lookup"><span data-stu-id="51896-128">String</span></span>|<span data-ttu-id="51896-129">Valfri.</span><span class="sxs-lookup"><span data-stu-id="51896-129">Optional.</span></span> <span data-ttu-id="51896-130">Blobens ETag-värde.</span><span class="sxs-lookup"><span data-stu-id="51896-130">The blob's ETag value.</span></span> <span data-ttu-id="51896-131">För exportjobb.</span><span class="sxs-lookup"><span data-stu-id="51896-131">For export jobs only.</span></span>|  
|`Content-Length`|<span data-ttu-id="51896-132">Sträng</span><span class="sxs-lookup"><span data-stu-id="51896-132">String</span></span>|<span data-ttu-id="51896-133">Valfri.</span><span class="sxs-lookup"><span data-stu-id="51896-133">Optional.</span></span> <span data-ttu-id="51896-134">Storleken på blobben i byte.</span><span class="sxs-lookup"><span data-stu-id="51896-134">The size of the blob in bytes.</span></span> <span data-ttu-id="51896-135">För exportjobb.</span><span class="sxs-lookup"><span data-stu-id="51896-135">For export jobs only.</span></span>|  
|`Content-Type`|<span data-ttu-id="51896-136">Sträng</span><span class="sxs-lookup"><span data-stu-id="51896-136">String</span></span>|<span data-ttu-id="51896-137">Valfri.</span><span class="sxs-lookup"><span data-stu-id="51896-137">Optional.</span></span> <span data-ttu-id="51896-138">Innehållstypen för blob.</span><span class="sxs-lookup"><span data-stu-id="51896-138">The content type of the blob.</span></span>|  
|`Content-MD5`|<span data-ttu-id="51896-139">Sträng</span><span class="sxs-lookup"><span data-stu-id="51896-139">String</span></span>|<span data-ttu-id="51896-140">Valfri.</span><span class="sxs-lookup"><span data-stu-id="51896-140">Optional.</span></span> <span data-ttu-id="51896-141">Blobens MD5-hash.</span><span class="sxs-lookup"><span data-stu-id="51896-141">The blob's MD5 hash.</span></span>|  
|`Content-Encoding`|<span data-ttu-id="51896-142">Sträng</span><span class="sxs-lookup"><span data-stu-id="51896-142">String</span></span>|<span data-ttu-id="51896-143">Valfri.</span><span class="sxs-lookup"><span data-stu-id="51896-143">Optional.</span></span> <span data-ttu-id="51896-144">Den blob-innehåll kodning.</span><span class="sxs-lookup"><span data-stu-id="51896-144">The blob's content encoding.</span></span>|  
|`Content-Language`|<span data-ttu-id="51896-145">Sträng</span><span class="sxs-lookup"><span data-stu-id="51896-145">String</span></span>|<span data-ttu-id="51896-146">Valfri.</span><span class="sxs-lookup"><span data-stu-id="51896-146">Optional.</span></span> <span data-ttu-id="51896-147">Blobens innehåll språk.</span><span class="sxs-lookup"><span data-stu-id="51896-147">The blob's content language.</span></span>|  
|`Cache-Control`|<span data-ttu-id="51896-148">Sträng</span><span class="sxs-lookup"><span data-stu-id="51896-148">String</span></span>|<span data-ttu-id="51896-149">Valfri.</span><span class="sxs-lookup"><span data-stu-id="51896-149">Optional.</span></span> <span data-ttu-id="51896-150">Kontroll av cache sträng för blob.</span><span class="sxs-lookup"><span data-stu-id="51896-150">The cache control string for the blob.</span></span>|  

## <a name="next-steps"></a><span data-ttu-id="51896-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="51896-151">Next steps</span></span>

<span data-ttu-id="51896-152">Se [ange egenskaper för blob](/rest/api/storageservices/set-blob-properties), [ange Blob-Metadata](/rest/api/storageservices/set-blob-metadata), och [inställningen och hämtning av egenskaper och metadata för blob-resurser](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) för detaljerade regler om inställningen blob-metadata och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="51896-152">See [Set blob properties](/rest/api/storageservices/set-blob-properties), [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata), and [Setting and retrieving properties and metadata for blob resources](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) for detailed rules about setting blob metadata and properties.</span></span>
