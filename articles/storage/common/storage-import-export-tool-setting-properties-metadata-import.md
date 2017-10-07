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
# <a name="setting-properties-and-metadata-during-hello-import-process"></a><span data-ttu-id="4b715-103">Ange egenskaper och metadata under hello import-processen</span><span class="sxs-lookup"><span data-stu-id="4b715-103">Setting properties and metadata during hello import process</span></span>

<span data-ttu-id="4b715-104">När du kör hello verktyget Azure Import/Export tooprepare enheter måste ange du egenskaper och metadata toobe på hello mål-BLOB.</span><span class="sxs-lookup"><span data-stu-id="4b715-104">When you run hello Microsoft Azure Import/Export Tool tooprepare your drives, you can specify properties and metadata toobe set on hello destination blobs.</span></span> <span data-ttu-id="4b715-105">Följ de här stegen:</span><span class="sxs-lookup"><span data-stu-id="4b715-105">Follow these steps:</span></span>

1.  <span data-ttu-id="4b715-106">tooset blob-egenskaper, skapa en textfil på den lokala datorn som anger egenskapsnamn och värden.</span><span class="sxs-lookup"><span data-stu-id="4b715-106">tooset blob properties, create a text file on your local computer that specifies property names and values.</span></span>
2.  <span data-ttu-id="4b715-107">tooset blob-metadata, skapa en textfil på den lokala datorn som anger metadata namn och värden.</span><span class="sxs-lookup"><span data-stu-id="4b715-107">tooset blob metadata, create a text file on your local computer that specifies metadata names and values.</span></span>
3.  <span data-ttu-id="4b715-108">Skicka hello fullständig sökväg tooone eller båda av dessa filer toohello Azure Import/Export-verktyget som en del av hello `PrepImport` igen.</span><span class="sxs-lookup"><span data-stu-id="4b715-108">Pass hello full path tooone or both of these files toohello Azure Import/Export Tool as part of hello `PrepImport` operation.</span></span>

> [!NOTE]
>  <span data-ttu-id="4b715-109">När du anger en egenskaper eller metadata-fil som en del av en kopia session ange dessa egenskaper eller metadata för varje blob som importeras som en del av den aktuella sessionen kopia.</span><span class="sxs-lookup"><span data-stu-id="4b715-109">When you specify a properties or metadata file as part of a copy session, those properties or metadata are set for every blob that is imported as part of that copy session.</span></span> <span data-ttu-id="4b715-110">Om du vill toospecify en annan uppsättning egenskaper eller metadata för några av hello blob som importeras måste toocreate som en separat kopiera session med olika egenskaper eller metadatafiler.</span><span class="sxs-lookup"><span data-stu-id="4b715-110">If you want toospecify a different set of properties or metadata for some of hello blobs being imported, you'll need toocreate a separate copy session with different properties or metadata files.</span></span>

## <a name="specify-blob-properties-in-a-text-file"></a><span data-ttu-id="4b715-111">Ange blob-egenskaper i en textfil</span><span class="sxs-lookup"><span data-stu-id="4b715-111">Specify blob properties in a text file</span></span>

<span data-ttu-id="4b715-112">toospecify blob-egenskaper, skapa en lokal textfil och innehåller XML som anger egenskapsnamn som element och egenskapsvärden som värden.</span><span class="sxs-lookup"><span data-stu-id="4b715-112">toospecify blob properties, create a local text file, and include XML that specifies property names as elements, and property values as values.</span></span> <span data-ttu-id="4b715-113">Här är ett exempel som anger vissa egenskapsvärden:</span><span class="sxs-lookup"><span data-stu-id="4b715-113">Here's an example that specifies some property values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

<span data-ttu-id="4b715-114">Spara hello tooa lokal plats som `C:\WAImportExport\ImportProperties.txt`.</span><span class="sxs-lookup"><span data-stu-id="4b715-114">Save hello file tooa local location like `C:\WAImportExport\ImportProperties.txt`.</span></span>

## <a name="specify-blob-metadata-in-a-text-file"></a><span data-ttu-id="4b715-115">Ange blobbmetadata i en textfil</span><span class="sxs-lookup"><span data-stu-id="4b715-115">Specify blob metadata in a text file</span></span>

<span data-ttu-id="4b715-116">På liknande sätt toospecify blob-metadata, skapa en lokal textfil som anger metadatanamnen som element och metadatavärden som värden.</span><span class="sxs-lookup"><span data-stu-id="4b715-116">Similarly, toospecify blob metadata, create a local text file that specifies metadata names as elements, and metadata values as values.</span></span> <span data-ttu-id="4b715-117">Här är ett exempel som anger vissa metadatavärden:</span><span class="sxs-lookup"><span data-stu-id="4b715-117">Here's an example that specifies some metadata values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

<span data-ttu-id="4b715-118">Spara hello tooa lokal plats som `C:\WAImportExport\ImportMetadata.txt`.</span><span class="sxs-lookup"><span data-stu-id="4b715-118">Save hello file tooa local location like `C:\WAImportExport\ImportMetadata.txt`.</span></span>

## <a name="add-hello-path-tooproperties-and-metadata-files-in-datasetcsv"></a><span data-ttu-id="4b715-119">Lägg till hello sökväg tooproperties och metadata för filer i dataset.csv</span><span class="sxs-lookup"><span data-stu-id="4b715-119">Add hello path tooproperties and metadata files in dataset.csv</span></span>

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,https://mystorageaccount.blob.core.windows.net/video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,https://mystorageaccount.blob.core.windows.net/photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,https://mystorageaccount.blob.core.windows.net/music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

## <a name="next-steps"></a><span data-ttu-id="4b715-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4b715-120">Next steps</span></span>

* [<span data-ttu-id="4b715-121">Import/Export-tjänstens metadata och egenskapers filformat</span><span class="sxs-lookup"><span data-stu-id="4b715-121">Import/Export service metadata and properties file format</span></span>](../storage-import-export-file-format-metadata-and-properties.md)
