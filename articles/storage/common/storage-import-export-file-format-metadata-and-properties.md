---
title: aaaAzure Import/Export metadata och egenskaper filformatet | Microsoft Docs
description: "Lär dig hur toospecify metadata och egenskaper för en eller flera BLOB-objekt som är en del av en importera eller exportera jobb."
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
ms.openlocfilehash: bb13c1f1a27baea77298cb224970cd521d02d8c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a><span data-ttu-id="00820-103">Azure Import/Export service metadata och egenskaper filformat</span><span class="sxs-lookup"><span data-stu-id="00820-103">Azure Import/Export service metadata and properties file format</span></span>
<span data-ttu-id="00820-104">Du kan ange egenskaper för en eller flera blobbar och metadata som en del av ett importjobb eller ett exportjobb.</span><span class="sxs-lookup"><span data-stu-id="00820-104">You can specify metadata and properties for one or more blobs as part of an import job or an export job.</span></span> <span data-ttu-id="00820-105">Egenskaper för blob som skapas som en del av ett importjobb eller tooset metadata kan du ange en egenskaper eller metadata fil på hello hårddisk som innehåller hello data toobe importeras.</span><span class="sxs-lookup"><span data-stu-id="00820-105">tooset metadata or properties for blobs being created as part of an import job, you provide a metadata or properties file on hello hard drive containing hello data toobe imported.</span></span> <span data-ttu-id="00820-106">För ett exportjobb skrivs metadata och egenskaper tooa egenskaper eller metadata fil som ingår på hello hårddisken returnerade tooyou.</span><span class="sxs-lookup"><span data-stu-id="00820-106">For an export job, metadata and properties are written tooa metadata or properties file that is included on hello hard drive returned tooyou.</span></span>  
  
## <a name="metadata-file-format"></a><span data-ttu-id="00820-107">Filformat för metadata</span><span class="sxs-lookup"><span data-stu-id="00820-107">Metadata file format</span></span>  
<span data-ttu-id="00820-108">hello-format för en metadatafil är följande:</span><span class="sxs-lookup"><span data-stu-id="00820-108">hello format of a metadata file is as follows:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|<span data-ttu-id="00820-109">XML-Element</span><span class="sxs-lookup"><span data-stu-id="00820-109">XML Element</span></span>|<span data-ttu-id="00820-110">Typ</span><span class="sxs-lookup"><span data-stu-id="00820-110">Type</span></span>|<span data-ttu-id="00820-111">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="00820-111">Description</span></span>|  
|-----------------|----------|-----------------|  
|`Metadata`|<span data-ttu-id="00820-112">Rotelementet</span><span class="sxs-lookup"><span data-stu-id="00820-112">Root element</span></span>|<span data-ttu-id="00820-113">hello rotelementet för hello metadatafil.</span><span class="sxs-lookup"><span data-stu-id="00820-113">hello root element of hello metadata file.</span></span>|  
|`metadata-name`|<span data-ttu-id="00820-114">Sträng</span><span class="sxs-lookup"><span data-stu-id="00820-114">String</span></span>|<span data-ttu-id="00820-115">Valfri.</span><span class="sxs-lookup"><span data-stu-id="00820-115">Optional.</span></span> <span data-ttu-id="00820-116">hello XML-elementet anger hello namnet på hello metadata för hello blob och dess värde anger hello hello metadata inställningens värde.</span><span class="sxs-lookup"><span data-stu-id="00820-116">hello XML element specifies hello name of hello metadata for hello blob, and its value specifies hello value of hello metadata setting.</span></span>|  
  
## <a name="properties-file-format"></a><span data-ttu-id="00820-117">Filformatet för egenskaper</span><span class="sxs-lookup"><span data-stu-id="00820-117">Properties file format</span></span>  
<span data-ttu-id="00820-118">hello filformatet egenskaper är följande:</span><span class="sxs-lookup"><span data-stu-id="00820-118">hello format of a properties file is as follows:</span></span>  
  
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
  
|<span data-ttu-id="00820-119">XML-Element</span><span class="sxs-lookup"><span data-stu-id="00820-119">XML Element</span></span>|<span data-ttu-id="00820-120">Typ</span><span class="sxs-lookup"><span data-stu-id="00820-120">Type</span></span>|<span data-ttu-id="00820-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="00820-121">Description</span></span>|  
|-----------------|----------|-----------------|  
|`Properties`|<span data-ttu-id="00820-122">Rotelementet</span><span class="sxs-lookup"><span data-stu-id="00820-122">Root element</span></span>|<span data-ttu-id="00820-123">hello hello egenskaper filens rotelement.</span><span class="sxs-lookup"><span data-stu-id="00820-123">hello root element of hello properties file.</span></span>|  
|`Last-Modified`|<span data-ttu-id="00820-124">Sträng</span><span class="sxs-lookup"><span data-stu-id="00820-124">String</span></span>|<span data-ttu-id="00820-125">Valfri.</span><span class="sxs-lookup"><span data-stu-id="00820-125">Optional.</span></span> <span data-ttu-id="00820-126">hello modifierades senast för hello-blob.</span><span class="sxs-lookup"><span data-stu-id="00820-126">hello last-modified time for hello blob.</span></span> <span data-ttu-id="00820-127">För exportjobb.</span><span class="sxs-lookup"><span data-stu-id="00820-127">For export jobs only.</span></span>|  
|`Etag`|<span data-ttu-id="00820-128">Sträng</span><span class="sxs-lookup"><span data-stu-id="00820-128">String</span></span>|<span data-ttu-id="00820-129">Valfri.</span><span class="sxs-lookup"><span data-stu-id="00820-129">Optional.</span></span> <span data-ttu-id="00820-130">Hej blob ETag-värde.</span><span class="sxs-lookup"><span data-stu-id="00820-130">hello blob's ETag value.</span></span> <span data-ttu-id="00820-131">För exportjobb.</span><span class="sxs-lookup"><span data-stu-id="00820-131">For export jobs only.</span></span>|  
|`Content-Length`|<span data-ttu-id="00820-132">Sträng</span><span class="sxs-lookup"><span data-stu-id="00820-132">String</span></span>|<span data-ttu-id="00820-133">Valfri.</span><span class="sxs-lookup"><span data-stu-id="00820-133">Optional.</span></span> <span data-ttu-id="00820-134">hello storlek hello blob i byte.</span><span class="sxs-lookup"><span data-stu-id="00820-134">hello size of hello blob in bytes.</span></span> <span data-ttu-id="00820-135">För exportjobb.</span><span class="sxs-lookup"><span data-stu-id="00820-135">For export jobs only.</span></span>|  
|`Content-Type`|<span data-ttu-id="00820-136">Sträng</span><span class="sxs-lookup"><span data-stu-id="00820-136">String</span></span>|<span data-ttu-id="00820-137">Valfri.</span><span class="sxs-lookup"><span data-stu-id="00820-137">Optional.</span></span> <span data-ttu-id="00820-138">hello innehållstypen för hello-blob.</span><span class="sxs-lookup"><span data-stu-id="00820-138">hello content type of hello blob.</span></span>|  
|`Content-MD5`|<span data-ttu-id="00820-139">Sträng</span><span class="sxs-lookup"><span data-stu-id="00820-139">String</span></span>|<span data-ttu-id="00820-140">Valfri.</span><span class="sxs-lookup"><span data-stu-id="00820-140">Optional.</span></span> <span data-ttu-id="00820-141">Hej blob MD5-hash.</span><span class="sxs-lookup"><span data-stu-id="00820-141">hello blob's MD5 hash.</span></span>|  
|`Content-Encoding`|<span data-ttu-id="00820-142">Sträng</span><span class="sxs-lookup"><span data-stu-id="00820-142">String</span></span>|<span data-ttu-id="00820-143">Valfri.</span><span class="sxs-lookup"><span data-stu-id="00820-143">Optional.</span></span> <span data-ttu-id="00820-144">Hej blob innehåll kodning.</span><span class="sxs-lookup"><span data-stu-id="00820-144">hello blob's content encoding.</span></span>|  
|`Content-Language`|<span data-ttu-id="00820-145">Sträng</span><span class="sxs-lookup"><span data-stu-id="00820-145">String</span></span>|<span data-ttu-id="00820-146">Valfri.</span><span class="sxs-lookup"><span data-stu-id="00820-146">Optional.</span></span> <span data-ttu-id="00820-147">Hej blob innehåll språk.</span><span class="sxs-lookup"><span data-stu-id="00820-147">hello blob's content language.</span></span>|  
|`Cache-Control`|<span data-ttu-id="00820-148">Sträng</span><span class="sxs-lookup"><span data-stu-id="00820-148">String</span></span>|<span data-ttu-id="00820-149">Valfri.</span><span class="sxs-lookup"><span data-stu-id="00820-149">Optional.</span></span> <span data-ttu-id="00820-150">hello cache kontrollen sträng för hello-blob.</span><span class="sxs-lookup"><span data-stu-id="00820-150">hello cache control string for hello blob.</span></span>|  

## <a name="next-steps"></a><span data-ttu-id="00820-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="00820-151">Next steps</span></span>

<span data-ttu-id="00820-152">Se [ange egenskaper för blob](/rest/api/storageservices/set-blob-properties), [ange Blob-Metadata](/rest/api/storageservices/set-blob-metadata), och [inställningen och hämtning av egenskaper och metadata för blob-resurser](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) för detaljerade regler om inställningen blob-metadata och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="00820-152">See [Set blob properties](/rest/api/storageservices/set-blob-properties), [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata), and [Setting and retrieving properties and metadata for blob resources](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) for detailed rules about setting blob metadata and properties.</span></span>
