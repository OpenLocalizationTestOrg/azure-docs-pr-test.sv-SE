---
title: "Ange egenskaper och metadata med hjälp av Azure Import/Export - v1 | Microsoft Docs"
description: "Lär dig mer om att ange egenskaper och metadata anges på mål-blobbar när du kör verktyget Azure Import/Export till förbereda dina enheter. Detta refererar till v1 i verktyget Import/Export."
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
ms.openlocfilehash: 6455ce57572f9ec36d0ebae88c1ddd9f40f237bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="setting-properties-and-metadata-during-the-import-process"></a><span data-ttu-id="1ec4f-104">Konfigurera egenskaper och metadata under importeringsprocessen</span><span class="sxs-lookup"><span data-stu-id="1ec4f-104">Setting properties and metadata during the import process</span></span>
<span data-ttu-id="1ec4f-105">När du kör verktyget Microsoft Azure Import/Export för att förbereda dina enheter, kan du ange egenskaper och metadata på mål-BLOB.</span><span class="sxs-lookup"><span data-stu-id="1ec4f-105">When you run the Microsoft Azure Import/Export Tool to prepare your drives, you can specify properties and metadata to be set on the destination blobs.</span></span> <span data-ttu-id="1ec4f-106">Följ de här stegen:</span><span class="sxs-lookup"><span data-stu-id="1ec4f-106">Follow these steps:</span></span>  
  
1.  <span data-ttu-id="1ec4f-107">Skapa en textfil på den lokala datorn som anger egenskapsnamn och värden för att ange egenskaper för blob.</span><span class="sxs-lookup"><span data-stu-id="1ec4f-107">To set blob properties, create a text file on your local computer that specifies property names and values.</span></span>  
  
2.  <span data-ttu-id="1ec4f-108">Skapa blobbmetadata, skapa en textfil på den lokala datorn som anger metadata namn och värden.</span><span class="sxs-lookup"><span data-stu-id="1ec4f-108">To set blob metadata, create a text file on your local computer that specifies metadata names and values.</span></span>  
  
3.  <span data-ttu-id="1ec4f-109">Skicka den fullständiga sökvägen till en eller båda av dessa filer till verktyget Azure Import/Export som en del av den `PrepImport` igen.</span><span class="sxs-lookup"><span data-stu-id="1ec4f-109">Pass the full path to one or both of these files to the Azure Import/Export Tool as part of the `PrepImport` operation.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="1ec4f-110">När du anger en egenskaper eller metadata-fil som en del av en kopia session ange dessa egenskaper eller metadata för varje blob som importeras som en del av den aktuella sessionen kopia.</span><span class="sxs-lookup"><span data-stu-id="1ec4f-110">When you specify a properties or metadata file as part of a copy session, those properties or metadata are set for every blob that is imported as part of that copy session.</span></span> <span data-ttu-id="1ec4f-111">Om du vill ange en annan uppsättning egenskaper eller metadata för några av de blobbar som importeras behöver du skapa en separat kopia-session med olika egenskaper eller metadatafiler.</span><span class="sxs-lookup"><span data-stu-id="1ec4f-111">If you want to specify a different set of properties or metadata for some of the blobs being imported, you'll need to create a separate copy session with different properties or metadata files.</span></span>  
  
## <a name="specify-blob-properties-in-a-text-file"></a><span data-ttu-id="1ec4f-112">Ange Blob-egenskaper i en textfil</span><span class="sxs-lookup"><span data-stu-id="1ec4f-112">Specify Blob Properties in a Text File</span></span>  
<span data-ttu-id="1ec4f-113">Skapa en lokal textfil för att ange blob-egenskaper, och innehåller XML som anger egenskapsnamn som element och egenskapsvärden som värden.</span><span class="sxs-lookup"><span data-stu-id="1ec4f-113">To specify blob properties, create a local text file, and include XML that specifies property names as elements, and property values as values.</span></span> <span data-ttu-id="1ec4f-114">Här är ett exempel som anger vissa egenskapsvärden:</span><span class="sxs-lookup"><span data-stu-id="1ec4f-114">Here's an example that specifies some property values:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
<span data-ttu-id="1ec4f-115">Spara filen på en lokal plats som `C:\WAImportExport\ImportProperties.txt`.</span><span class="sxs-lookup"><span data-stu-id="1ec4f-115">Save the file to a local location like `C:\WAImportExport\ImportProperties.txt`.</span></span>  
  
## <a name="specify-blob-metadata-in-a-text-file"></a><span data-ttu-id="1ec4f-116">Ange Blobbmetadata i en textfil</span><span class="sxs-lookup"><span data-stu-id="1ec4f-116">Specify Blob Metadata in a Text File</span></span>  
<span data-ttu-id="1ec4f-117">På samma sätt för att ange blobbmetadata, skapa en lokal textfil som anger metadatanamnen som element och metadatavärden som värden.</span><span class="sxs-lookup"><span data-stu-id="1ec4f-117">Similarly, to specify blob metadata, create a local text file that specifies metadata names as elements, and metadata values as values.</span></span> <span data-ttu-id="1ec4f-118">Här är ett exempel som anger vissa metadatavärden:</span><span class="sxs-lookup"><span data-stu-id="1ec4f-118">Here's an example that specifies some metadata values:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
<span data-ttu-id="1ec4f-119">Spara filen på en lokal plats som `C:\WAImportExport\ImportMetadata.txt`.</span><span class="sxs-lookup"><span data-stu-id="1ec4f-119">Save the file to a local location like `C:\WAImportExport\ImportMetadata.txt`.</span></span>  
  
## <a name="create-a-copy-session-including-the-properties-or-metadata-files"></a><span data-ttu-id="1ec4f-120">Skapa en kopia Session inklusive egenskaper eller metadatafiler</span><span class="sxs-lookup"><span data-stu-id="1ec4f-120">Create a Copy Session Including the Properties or Metadata Files</span></span>  
<span data-ttu-id="1ec4f-121">När du kör verktyget Azure Import/Export för att förbereda importjobbet ange egenskaper för filen på kommandoraden med den `PropertyFile` parameter.</span><span class="sxs-lookup"><span data-stu-id="1ec4f-121">When you run the Azure Import/Export Tool to prepare the import job, specify the properties file on the command line using the `PropertyFile` parameter.</span></span> <span data-ttu-id="1ec4f-122">Ange metadatafil på kommandoraden med den `/MetadataFile` parameter.</span><span class="sxs-lookup"><span data-stu-id="1ec4f-122">Specify the metadata file on the command line using the `/MetadataFile` parameter.</span></span> <span data-ttu-id="1ec4f-123">Här är ett exempel som anger båda filerna:</span><span class="sxs-lookup"><span data-stu-id="1ec4f-123">Here's an example that specifies both files:</span></span>  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="next-steps"></a><span data-ttu-id="1ec4f-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1ec4f-124">Next steps</span></span>

* [<span data-ttu-id="1ec4f-125">Import/Export-tjänstens metadata och egenskapers filformat</span><span class="sxs-lookup"><span data-stu-id="1ec4f-125">Import/Export service metadata and properties file format</span></span>](storage-import-export-file-format-metadata-and-properties.md)
