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
# <a name="setting-properties-and-metadata-during-hello-import-process"></a><span data-ttu-id="4216a-104">Ange egenskaper och metadata under hello import-processen</span><span class="sxs-lookup"><span data-stu-id="4216a-104">Setting properties and metadata during hello import process</span></span>
<span data-ttu-id="4216a-105">När du kör hello verktyget Azure Import/Export tooprepare enheter måste ange du egenskaper och metadata toobe på hello mål-BLOB.</span><span class="sxs-lookup"><span data-stu-id="4216a-105">When you run hello Microsoft Azure Import/Export Tool tooprepare your drives, you can specify properties and metadata toobe set on hello destination blobs.</span></span> <span data-ttu-id="4216a-106">Följ de här stegen:</span><span class="sxs-lookup"><span data-stu-id="4216a-106">Follow these steps:</span></span>  
  
1.  <span data-ttu-id="4216a-107">tooset blob-egenskaper, skapa en textfil på den lokala datorn som anger egenskapsnamn och värden.</span><span class="sxs-lookup"><span data-stu-id="4216a-107">tooset blob properties, create a text file on your local computer that specifies property names and values.</span></span>  
  
2.  <span data-ttu-id="4216a-108">tooset blob-metadata, skapa en textfil på den lokala datorn som anger metadata namn och värden.</span><span class="sxs-lookup"><span data-stu-id="4216a-108">tooset blob metadata, create a text file on your local computer that specifies metadata names and values.</span></span>  
  
3.  <span data-ttu-id="4216a-109">Skicka hello fullständig sökväg tooone eller båda av dessa filer toohello Azure Import/Export-verktyget som en del av hello `PrepImport` igen.</span><span class="sxs-lookup"><span data-stu-id="4216a-109">Pass hello full path tooone or both of these files toohello Azure Import/Export Tool as part of hello `PrepImport` operation.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="4216a-110">När du anger en egenskaper eller metadata-fil som en del av en kopia session ange dessa egenskaper eller metadata för varje blob som importeras som en del av den aktuella sessionen kopia.</span><span class="sxs-lookup"><span data-stu-id="4216a-110">When you specify a properties or metadata file as part of a copy session, those properties or metadata are set for every blob that is imported as part of that copy session.</span></span> <span data-ttu-id="4216a-111">Om du vill toospecify en annan uppsättning egenskaper eller metadata för några av hello blob som importeras måste toocreate som en separat kopiera session med olika egenskaper eller metadatafiler.</span><span class="sxs-lookup"><span data-stu-id="4216a-111">If you want toospecify a different set of properties or metadata for some of hello blobs being imported, you'll need toocreate a separate copy session with different properties or metadata files.</span></span>  
  
## <a name="specify-blob-properties-in-a-text-file"></a><span data-ttu-id="4216a-112">Ange Blob-egenskaper i en textfil</span><span class="sxs-lookup"><span data-stu-id="4216a-112">Specify Blob Properties in a Text File</span></span>  
<span data-ttu-id="4216a-113">toospecify blob-egenskaper, skapa en lokal textfil och innehåller XML som anger egenskapsnamn som element och egenskapsvärden som värden.</span><span class="sxs-lookup"><span data-stu-id="4216a-113">toospecify blob properties, create a local text file, and include XML that specifies property names as elements, and property values as values.</span></span> <span data-ttu-id="4216a-114">Här är ett exempel som anger vissa egenskapsvärden:</span><span class="sxs-lookup"><span data-stu-id="4216a-114">Here's an example that specifies some property values:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
<span data-ttu-id="4216a-115">Spara hello tooa lokal plats som `C:\WAImportExport\ImportProperties.txt`.</span><span class="sxs-lookup"><span data-stu-id="4216a-115">Save hello file tooa local location like `C:\WAImportExport\ImportProperties.txt`.</span></span>  
  
## <a name="specify-blob-metadata-in-a-text-file"></a><span data-ttu-id="4216a-116">Ange Blobbmetadata i en textfil</span><span class="sxs-lookup"><span data-stu-id="4216a-116">Specify Blob Metadata in a Text File</span></span>  
<span data-ttu-id="4216a-117">På liknande sätt toospecify blob-metadata, skapa en lokal textfil som anger metadatanamnen som element och metadatavärden som värden.</span><span class="sxs-lookup"><span data-stu-id="4216a-117">Similarly, toospecify blob metadata, create a local text file that specifies metadata names as elements, and metadata values as values.</span></span> <span data-ttu-id="4216a-118">Här är ett exempel som anger vissa metadatavärden:</span><span class="sxs-lookup"><span data-stu-id="4216a-118">Here's an example that specifies some metadata values:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
<span data-ttu-id="4216a-119">Spara hello tooa lokal plats som `C:\WAImportExport\ImportMetadata.txt`.</span><span class="sxs-lookup"><span data-stu-id="4216a-119">Save hello file tooa local location like `C:\WAImportExport\ImportMetadata.txt`.</span></span>  
  
## <a name="create-a-copy-session-including-hello-properties-or-metadata-files"></a><span data-ttu-id="4216a-120">Skapa en kopia sessionen inklusive hello egenskaper eller metadatafiler</span><span class="sxs-lookup"><span data-stu-id="4216a-120">Create a Copy Session Including hello Properties or Metadata Files</span></span>  
<span data-ttu-id="4216a-121">När du kör hello Azure Import/Export verktyget tooprepare hello importjobbet anger hello egenskapsfil på hello kommandoraden med hjälp av hello `PropertyFile` parameter.</span><span class="sxs-lookup"><span data-stu-id="4216a-121">When you run hello Azure Import/Export Tool tooprepare hello import job, specify hello properties file on hello command line using hello `PropertyFile` parameter.</span></span> <span data-ttu-id="4216a-122">Ange hello metadatafil med hello kommandorad med hello `/MetadataFile` parameter.</span><span class="sxs-lookup"><span data-stu-id="4216a-122">Specify hello metadata file on hello command line using hello `/MetadataFile` parameter.</span></span> <span data-ttu-id="4216a-123">Här är ett exempel som anger båda filerna:</span><span class="sxs-lookup"><span data-stu-id="4216a-123">Here's an example that specifies both files:</span></span>  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="next-steps"></a><span data-ttu-id="4216a-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4216a-124">Next steps</span></span>

* [<span data-ttu-id="4216a-125">Import/Export-tjänstens metadata och egenskapers filformat</span><span class="sxs-lookup"><span data-stu-id="4216a-125">Import/Export service metadata and properties file format</span></span>](../storage-import-export-file-format-metadata-and-properties.md)
