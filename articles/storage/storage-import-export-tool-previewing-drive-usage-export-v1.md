---
title: "Förhandsgranska diskanvändning för ett Azure Import/Export-exportjobb - v1 | Microsoft Docs"
description: "Lär dig mer om att förhandsgranska listan över blobbar som du har valt för ett exportjobb i tjänsten Azure Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 7707d744-7ec7-4de8-ac9b-93a18608dc9a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 7bf74247090f91e17f81a9bc98ebfa78334c8c10
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="previewing-drive-usage-for-an-export-job"></a><span data-ttu-id="58a4c-103">Förhandsgranska diskanvändning för ett exportjobb</span><span class="sxs-lookup"><span data-stu-id="58a4c-103">Previewing drive usage for an export job</span></span>
<span data-ttu-id="58a4c-104">Innan du skapar ett exportjobb måste du välja en uppsättning blobbar som ska exporteras.</span><span class="sxs-lookup"><span data-stu-id="58a4c-104">Before you create an export job, you need to choose a set of blobs to be exported.</span></span> <span data-ttu-id="58a4c-105">Tjänsten Microsoft Azure Import/Export kan du använda en lista över blob-sökvägar eller blob-prefix för att representera blobbar som du har valt.</span><span class="sxs-lookup"><span data-stu-id="58a4c-105">The Microsoft Azure Import/Export service allows you to use a list of blob paths or blob prefixes to represent the blobs you've selected.</span></span>  
  
<span data-ttu-id="58a4c-106">Därefter måste du bestämma hur många enheter som du måste skicka.</span><span class="sxs-lookup"><span data-stu-id="58a4c-106">Next, you need to determine how many drives you need to send.</span></span> <span data-ttu-id="58a4c-107">Verktyget Import/Export ger den `PreviewExport` kommando för att förhandsgranska diskanvändning för blobbar som du har valt, baserat på storleken på enheterna som du ska använda.</span><span class="sxs-lookup"><span data-stu-id="58a4c-107">The Import/Export Tool provides the `PreviewExport` command to preview drive usage for the blobs you selected, based on the size of the drives you are going to use.</span></span>

## <a name="command-line-parameters"></a><span data-ttu-id="58a4c-108">Kommandoradsparametrar</span><span class="sxs-lookup"><span data-stu-id="58a4c-108">Command-line parameters</span></span>

<span data-ttu-id="58a4c-109">Du kan använda följande parametrar när du använder den `PreviewExport` kommandot i verktyget Import/Export.</span><span class="sxs-lookup"><span data-stu-id="58a4c-109">You can use the following parameters when using the `PreviewExport` command of the Import/Export Tool.</span></span>

|<span data-ttu-id="58a4c-110">Kommandoradsparametern</span><span class="sxs-lookup"><span data-stu-id="58a4c-110">Command-line parameter</span></span>|<span data-ttu-id="58a4c-111">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="58a4c-111">Description</span></span>|  
|--------------------------|-----------------|  
|<span data-ttu-id="58a4c-112">**/logdir:**< LogDirectory\></span><span class="sxs-lookup"><span data-stu-id="58a4c-112">**/logdir:**<LogDirectory\></span></span>|<span data-ttu-id="58a4c-113">Valfri.</span><span class="sxs-lookup"><span data-stu-id="58a4c-113">Optional.</span></span> <span data-ttu-id="58a4c-114">Loggkatalogen.</span><span class="sxs-lookup"><span data-stu-id="58a4c-114">The log directory.</span></span> <span data-ttu-id="58a4c-115">Utförlig loggfilerna skrivs till den här katalogen.</span><span class="sxs-lookup"><span data-stu-id="58a4c-115">Verbose log files will be written to this directory.</span></span> <span data-ttu-id="58a4c-116">Om inga loggkatalogen anges, används den aktuella katalogen som loggkatalogen.</span><span class="sxs-lookup"><span data-stu-id="58a4c-116">If no log directory is specified, the current directory will be used as the log directory.</span></span>|  
|<span data-ttu-id="58a4c-117">**/SN:**< StorageAccountName\></span><span class="sxs-lookup"><span data-stu-id="58a4c-117">**/sn:**<StorageAccountName\></span></span>|<span data-ttu-id="58a4c-118">Krävs.</span><span class="sxs-lookup"><span data-stu-id="58a4c-118">Required.</span></span> <span data-ttu-id="58a4c-119">Namnet på lagringskontot för exportjobbet.</span><span class="sxs-lookup"><span data-stu-id="58a4c-119">The name of the storage account for the export job.</span></span>|  
|<span data-ttu-id="58a4c-120">**/Sk:**< StorageAccountKey\></span><span class="sxs-lookup"><span data-stu-id="58a4c-120">**/sk:**<StorageAccountKey\></span></span>|<span data-ttu-id="58a4c-121">Krävs endast om en behållare SAS inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="58a4c-121">Required if and only if a container SAS is not specified.</span></span> <span data-ttu-id="58a4c-122">Kontonyckel för lagringskontot för exportjobbet.</span><span class="sxs-lookup"><span data-stu-id="58a4c-122">The account key for the storage account for the export job.</span></span>|  
|<span data-ttu-id="58a4c-123">**/csas:**< ContainerSas\></span><span class="sxs-lookup"><span data-stu-id="58a4c-123">**/csas:**<ContainerSas\></span></span>|<span data-ttu-id="58a4c-124">Krävs endast om en lagringskontonyckel inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="58a4c-124">Required if and only if a storage account key is not specified.</span></span> <span data-ttu-id="58a4c-125">Behållare SAS för att visa en lista över blobbar som ska exporteras i exportjobbet.</span><span class="sxs-lookup"><span data-stu-id="58a4c-125">The container SAS for listing the blobs to be exported in the export job.</span></span>|  
|<span data-ttu-id="58a4c-126">**/ ExportBlobListFile:**< ExportBlobListFile\></span><span class="sxs-lookup"><span data-stu-id="58a4c-126">**/ExportBlobListFile:**<ExportBlobListFile\></span></span>|<span data-ttu-id="58a4c-127">Krävs.</span><span class="sxs-lookup"><span data-stu-id="58a4c-127">Required.</span></span> <span data-ttu-id="58a4c-128">Sökvägen till XML-fil som innehåller listan över blob-sökvägar eller blob sökväg prefix för blob som ska exporteras.</span><span class="sxs-lookup"><span data-stu-id="58a4c-128">Path to the XML file containing list of blob paths or blob path prefixes for the blobs to be exported.</span></span> <span data-ttu-id="58a4c-129">Filformat som används i den `BlobListBlobPath` element i den [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) driften av tjänsten Import/Export REST API.</span><span class="sxs-lookup"><span data-stu-id="58a4c-129">The file format used in the `BlobListBlobPath` element in the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation of the Import/Export service REST API.</span></span>|  
|<span data-ttu-id="58a4c-130">**/ DriveSize:**< DriveSize\></span><span class="sxs-lookup"><span data-stu-id="58a4c-130">**/DriveSize:**<DriveSize\></span></span>|<span data-ttu-id="58a4c-131">Krävs.</span><span class="sxs-lookup"><span data-stu-id="58a4c-131">Required.</span></span> <span data-ttu-id="58a4c-132">Storleken på enheter som ska användas för ett exportjobb *t.ex.*, 500 GB, 1,5 TB.</span><span class="sxs-lookup"><span data-stu-id="58a4c-132">The size of drives to use for an export job, *e.g.*, 500GB, 1.5TB.</span></span>|  

## <a name="command-line-example"></a><span data-ttu-id="58a4c-133">Kommandoradsverktyget exempel</span><span class="sxs-lookup"><span data-stu-id="58a4c-133">Command-line example</span></span>

<span data-ttu-id="58a4c-134">I följande exempel visas den `PreviewExport` kommando:</span><span class="sxs-lookup"><span data-stu-id="58a4c-134">The following example demonstrates the `PreviewExport` command:</span></span>  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
<span data-ttu-id="58a4c-135">Exportfilen blob listan kan innehålla blobbnamnen och blob-prefix, som visas här:</span><span class="sxs-lookup"><span data-stu-id="58a4c-135">The export blob list file may contain blob names and blob prefixes, as shown here:</span></span>  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

<span data-ttu-id="58a4c-136">Verktyget Azure Import/Export listar alla BLOB som ska exporteras och beräknar så pack dem till enheter i den angivna storleken med hänsyn till alla nödvändiga kostnader och sedan beräknar antalet enheter som behövs för blobbar och användningsinformation för enheten.</span><span class="sxs-lookup"><span data-stu-id="58a4c-136">The Azure Import/Export Tool lists all blobs to be exported and calculates how to pack them into drives of the specified size, taking into account any necessary overhead, then estimates the number of drives needed to hold the blobs and drive usage information.</span></span>  
  
<span data-ttu-id="58a4c-137">Här är ett exempel på utdata med informativt loggar utelämnas:</span><span class="sxs-lookup"><span data-stu-id="58a4c-137">Here is an example of the output, with informational logs omitted:</span></span>  
  
```  
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  
  
Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB    
```  
  
## <a name="next-steps"></a><span data-ttu-id="58a4c-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="58a4c-138">Next steps</span></span>

* [<span data-ttu-id="58a4c-139">Referens för Azure Import/Export-verktyg</span><span class="sxs-lookup"><span data-stu-id="58a4c-139">Azure Import/Export Tool reference</span></span>](storage-import-export-tool-how-to-v1.md)
