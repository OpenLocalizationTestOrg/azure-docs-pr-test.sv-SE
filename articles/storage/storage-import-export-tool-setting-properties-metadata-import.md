---
title: "Ange egenskaper och metadata med hjälp av Azure Import/Export | Microsoft Docs"
description: "Lär dig mer om att ange egenskaper och metadata anges på mål-blobbar när du kör verktyget Azure Import/Export till förbereda dina enheter."
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
ms.openlocfilehash: bdc7a53f82d1fbbb726e2b1bd5d96678a8563566
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="setting-properties-and-metadata-during-the-import-process"></a><span data-ttu-id="02711-103">Konfigurera egenskaper och metadata under importeringsprocessen</span><span class="sxs-lookup"><span data-stu-id="02711-103">Setting properties and metadata during the import process</span></span>

<span data-ttu-id="02711-104">När du kör verktyget Microsoft Azure Import/Export för att förbereda dina enheter, kan du ange egenskaper och metadata på mål-BLOB.</span><span class="sxs-lookup"><span data-stu-id="02711-104">When you run the Microsoft Azure Import/Export Tool to prepare your drives, you can specify properties and metadata to be set on the destination blobs.</span></span> <span data-ttu-id="02711-105">Följ de här stegen:</span><span class="sxs-lookup"><span data-stu-id="02711-105">Follow these steps:</span></span>

1.  <span data-ttu-id="02711-106">Skapa en textfil på den lokala datorn som anger egenskapsnamn och värden för att ange egenskaper för blob.</span><span class="sxs-lookup"><span data-stu-id="02711-106">To set blob properties, create a text file on your local computer that specifies property names and values.</span></span>
2.  <span data-ttu-id="02711-107">Skapa blobbmetadata, skapa en textfil på den lokala datorn som anger metadata namn och värden.</span><span class="sxs-lookup"><span data-stu-id="02711-107">To set blob metadata, create a text file on your local computer that specifies metadata names and values.</span></span>
3.  <span data-ttu-id="02711-108">Skicka den fullständiga sökvägen till en eller båda av dessa filer till verktyget Azure Import/Export som en del av den `PrepImport` igen.</span><span class="sxs-lookup"><span data-stu-id="02711-108">Pass the full path to one or both of these files to the Azure Import/Export Tool as part of the `PrepImport` operation.</span></span>

> [!NOTE]
>  <span data-ttu-id="02711-109">När du anger en egenskaper eller metadata-fil som en del av en kopia session ange dessa egenskaper eller metadata för varje blob som importeras som en del av den aktuella sessionen kopia.</span><span class="sxs-lookup"><span data-stu-id="02711-109">When you specify a properties or metadata file as part of a copy session, those properties or metadata are set for every blob that is imported as part of that copy session.</span></span> <span data-ttu-id="02711-110">Om du vill ange en annan uppsättning egenskaper eller metadata för några av de blobbar som importeras behöver du skapa en separat kopia-session med olika egenskaper eller metadatafiler.</span><span class="sxs-lookup"><span data-stu-id="02711-110">If you want to specify a different set of properties or metadata for some of the blobs being imported, you'll need to create a separate copy session with different properties or metadata files.</span></span>

## <a name="specify-blob-properties-in-a-text-file"></a><span data-ttu-id="02711-111">Ange blob-egenskaper i en textfil</span><span class="sxs-lookup"><span data-stu-id="02711-111">Specify blob properties in a text file</span></span>

<span data-ttu-id="02711-112">Skapa en lokal textfil för att ange blob-egenskaper, och innehåller XML som anger egenskapsnamn som element och egenskapsvärden som värden.</span><span class="sxs-lookup"><span data-stu-id="02711-112">To specify blob properties, create a local text file, and include XML that specifies property names as elements, and property values as values.</span></span> <span data-ttu-id="02711-113">Här är ett exempel som anger vissa egenskapsvärden:</span><span class="sxs-lookup"><span data-stu-id="02711-113">Here's an example that specifies some property values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

<span data-ttu-id="02711-114">Spara filen på en lokal plats som `C:\WAImportExport\ImportProperties.txt`.</span><span class="sxs-lookup"><span data-stu-id="02711-114">Save the file to a local location like `C:\WAImportExport\ImportProperties.txt`.</span></span>

## <a name="specify-blob-metadata-in-a-text-file"></a><span data-ttu-id="02711-115">Ange blobbmetadata i en textfil</span><span class="sxs-lookup"><span data-stu-id="02711-115">Specify blob metadata in a text file</span></span>

<span data-ttu-id="02711-116">På samma sätt för att ange blobbmetadata, skapa en lokal textfil som anger metadatanamnen som element och metadatavärden som värden.</span><span class="sxs-lookup"><span data-stu-id="02711-116">Similarly, to specify blob metadata, create a local text file that specifies metadata names as elements, and metadata values as values.</span></span> <span data-ttu-id="02711-117">Här är ett exempel som anger vissa metadatavärden:</span><span class="sxs-lookup"><span data-stu-id="02711-117">Here's an example that specifies some metadata values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

<span data-ttu-id="02711-118">Spara filen på en lokal plats som `C:\WAImportExport\ImportMetadata.txt`.</span><span class="sxs-lookup"><span data-stu-id="02711-118">Save the file to a local location like `C:\WAImportExport\ImportMetadata.txt`.</span></span>

## <a name="add-the-path-to-properties-and-metadata-files-in-datasetcsv"></a><span data-ttu-id="02711-119">Lägg till sökvägen egenskaper och metadatafiler i dataset.csv</span><span class="sxs-lookup"><span data-stu-id="02711-119">Add the path to properties and metadata files in dataset.csv</span></span>

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,https://mystorageaccount.blob.core.windows.net/video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,https://mystorageaccount.blob.core.windows.net/photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,https://mystorageaccount.blob.core.windows.net/music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

## <a name="next-steps"></a><span data-ttu-id="02711-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="02711-120">Next steps</span></span>

* [<span data-ttu-id="02711-121">Import/Export-tjänstens metadata och egenskapers filformat</span><span class="sxs-lookup"><span data-stu-id="02711-121">Import/Export service metadata and properties file format</span></span>](storage-import-export-file-format-metadata-and-properties.md)
