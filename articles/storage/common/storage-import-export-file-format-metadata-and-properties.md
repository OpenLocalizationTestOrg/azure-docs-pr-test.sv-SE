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
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a>Azure Import/Export service metadata och egenskaper filformat
Du kan ange egenskaper för en eller flera blobbar och metadata som en del av ett importjobb eller ett exportjobb. Egenskaper för blob som skapas som en del av ett importjobb eller tooset metadata kan du ange en egenskaper eller metadata fil på hello hårddisk som innehåller hello data toobe importeras. För ett exportjobb skrivs metadata och egenskaper tooa egenskaper eller metadata fil som ingår på hello hårddisken returnerade tooyou.  
  
## <a name="metadata-file-format"></a>Filformat för metadata  
hello-format för en metadatafil är följande:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|XML-Element|Typ|Beskrivning|  
|-----------------|----------|-----------------|  
|`Metadata`|Rotelementet|hello rotelementet för hello metadatafil.|  
|`metadata-name`|Sträng|Valfri. hello XML-elementet anger hello namnet på hello metadata för hello blob och dess värde anger hello hello metadata inställningens värde.|  
  
## <a name="properties-file-format"></a>Filformatet för egenskaper  
hello filformatet egenskaper är följande:  
  
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
  
|XML-Element|Typ|Beskrivning|  
|-----------------|----------|-----------------|  
|`Properties`|Rotelementet|hello hello egenskaper filens rotelement.|  
|`Last-Modified`|Sträng|Valfri. hello modifierades senast för hello-blob. För exportjobb.|  
|`Etag`|Sträng|Valfri. Hej blob ETag-värde. För exportjobb.|  
|`Content-Length`|Sträng|Valfri. hello storlek hello blob i byte. För exportjobb.|  
|`Content-Type`|Sträng|Valfri. hello innehållstypen för hello-blob.|  
|`Content-MD5`|Sträng|Valfri. Hej blob MD5-hash.|  
|`Content-Encoding`|Sträng|Valfri. Hej blob innehåll kodning.|  
|`Content-Language`|Sträng|Valfri. Hej blob innehåll språk.|  
|`Cache-Control`|Sträng|Valfri. hello cache kontrollen sträng för hello-blob.|  

## <a name="next-steps"></a>Nästa steg

Se [ange egenskaper för blob](/rest/api/storageservices/set-blob-properties), [ange Blob-Metadata](/rest/api/storageservices/set-blob-metadata), och [inställningen och hämtning av egenskaper och metadata för blob-resurser](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) för detaljerade regler om inställningen blob-metadata och egenskaper.
