---
title: "aaaSetting egenskaper och metadata med hjälp av Azure Import/Export | Microsoft Docs"
description: "Lär dig hur toospecify egenskaper och metadata toobe anges för hello mål blobbar när du kör hello Azure Import/Export verktyget tooprepare dina enheter."
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
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: c763237160f0e4b72ce88fd31e2958994bfe8e50
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

## <a name="specify-blob-properties-in-a-text-file"></a>Ange blob-egenskaper i en textfil

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

## <a name="specify-blob-metadata-in-a-text-file"></a>Ange blobbmetadata i en textfil

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

## <a name="add-hello-path-tooproperties-and-metadata-files-in-datasetcsv"></a>Lägg till hello sökväg tooproperties och metadata för filer i dataset.csv

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,https://mystorageaccount.blob.core.windows.net/video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,https://mystorageaccount.blob.core.windows.net/photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,https://mystorageaccount.blob.core.windows.net/music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

## <a name="next-steps"></a>Nästa steg

* [Import/Export-tjänstens metadata och egenskapers filformat](../storage-import-export-file-format-metadata-and-properties.md)
