---
title: "aaaSetting egenskaper och metadata med hjälp av Azure Import/Export - v1 | Microsoft Docs"
description: "Lär dig hur toospecify egenskaper och metadata toobe anges för hello mål blobbar när du kör hello Azure Import/Export verktyget tooprepare dina enheter. Detta refererar toov1 av hello verktyget Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: e8541695-bcfb-4bf0-84d9-72c9de32a0a8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 5b7b1c346ecde8a26d985bd5de7efcf7d86eb9e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="setting-properties-and-metadata-during-hello-import-process"></a>Ange egenskaper och metadata under hello import-processen
När du kör hello verktyget Azure Import/Export tooprepare enheter måste ange du egenskaper och metadata toobe på hello mål-BLOB. Följ de här stegen:  
  
1.  tooset blob-egenskaper, skapa en textfil på den lokala datorn som anger egenskapsnamn och värden.  
  
2.  tooset blob-metadata, skapa en textfil på den lokala datorn som anger metadata namn och värden.  
  
3.  Skicka hello fullständig sökväg tooone eller båda av dessa filer toohello Azure Import/Export-verktyget som en del av hello `PrepImport` igen.  
  
> [!NOTE]
>  När du anger en egenskaper eller metadata-fil som en del av en kopia session ange dessa egenskaper eller metadata för varje blob som importeras som en del av den aktuella sessionen kopia. Om du vill toospecify en annan uppsättning egenskaper eller metadata för några av hello blob som importeras måste toocreate som en separat kopiera session med olika egenskaper eller metadatafiler.  
  
## <a name="specify-blob-properties-in-a-text-file"></a>Ange Blob-egenskaper i en textfil  
toospecify blob-egenskaper, skapa en lokal textfil och innehåller XML som anger egenskapsnamn som element och egenskapsvärden som värden. Här är ett exempel som anger vissa egenskapsvärden:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
Spara hello tooa lokal plats som `C:\WAImportExport\ImportProperties.txt`.  
  
## <a name="specify-blob-metadata-in-a-text-file"></a>Ange Blobbmetadata i en textfil  
På liknande sätt toospecify blob-metadata, skapa en lokal textfil som anger metadatanamnen som element och metadatavärden som värden. Här är ett exempel som anger vissa metadatavärden:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
Spara hello tooa lokal plats som `C:\WAImportExport\ImportMetadata.txt`.  
  
## <a name="create-a-copy-session-including-hello-properties-or-metadata-files"></a>Skapa en kopia sessionen inklusive hello egenskaper eller metadatafiler  
När du kör hello Azure Import/Export verktyget tooprepare hello importjobbet anger hello egenskapsfil på hello kommandoraden med hjälp av hello `PropertyFile` parameter. Ange hello metadatafil med hello kommandorad med hello `/MetadataFile` parameter. Här är ett exempel som anger båda filerna:  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="next-steps"></a>Nästa steg

* [Import/Export-tjänstens metadata och egenskapers filformat](../storage-import-export-file-format-metadata-and-properties.md)
