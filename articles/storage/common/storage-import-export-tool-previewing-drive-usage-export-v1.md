---
title: "aaaPreviewing diskanvändning för ett Azure Import/Export-exportjobb - v1 | Microsoft Docs"
description: "Lär dig hur toopreview hello lista över BLOB du har valt för ett exportjobb i hello Azure Import/Export service."
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
ms.openlocfilehash: 7378c159f6d11702cda9ae7654e84d85f9b671b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="previewing-drive-usage-for-an-export-job"></a><span data-ttu-id="785a8-103">Förhandsgranska diskanvändning för ett exportjobb</span><span class="sxs-lookup"><span data-stu-id="785a8-103">Previewing drive usage for an export job</span></span>
<span data-ttu-id="785a8-104">Innan du skapar ett exportjobb måste toochoose en uppsättning blobbar toobe exporteras.</span><span class="sxs-lookup"><span data-stu-id="785a8-104">Before you create an export job, you need toochoose a set of blobs toobe exported.</span></span> <span data-ttu-id="785a8-105">hello Microsoft Azure Import/Export service kan du toouse en lista över blob-sökvägar eller blob-prefix toorepresent hello blob som du har valt.</span><span class="sxs-lookup"><span data-stu-id="785a8-105">hello Microsoft Azure Import/Export service allows you toouse a list of blob paths or blob prefixes toorepresent hello blobs you've selected.</span></span>  
  
<span data-ttu-id="785a8-106">Sedan måste toodetermine hur många enheter du behöver toosend.</span><span class="sxs-lookup"><span data-stu-id="785a8-106">Next, you need toodetermine how many drives you need toosend.</span></span> <span data-ttu-id="785a8-107">hello verktyget Import/Export ger hello `PreviewExport` kommandot toopreview diskanvändning för hello blob som du har valt, baserat på hello storlek hello enheter du kommer toouse.</span><span class="sxs-lookup"><span data-stu-id="785a8-107">hello Import/Export Tool provides hello `PreviewExport` command toopreview drive usage for hello blobs you selected, based on hello size of hello drives you are going toouse.</span></span>

## <a name="command-line-parameters"></a><span data-ttu-id="785a8-108">Kommandoradsparametrar</span><span class="sxs-lookup"><span data-stu-id="785a8-108">Command-line parameters</span></span>

<span data-ttu-id="785a8-109">Du kan använda följande parametrar när du använder hello hello `PreviewExport` kommandot av hello verktyget Import/Export.</span><span class="sxs-lookup"><span data-stu-id="785a8-109">You can use hello following parameters when using hello `PreviewExport` command of hello Import/Export Tool.</span></span>

|<span data-ttu-id="785a8-110">Kommandoradsparametern</span><span class="sxs-lookup"><span data-stu-id="785a8-110">Command-line parameter</span></span>|<span data-ttu-id="785a8-111">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="785a8-111">Description</span></span>|  
|--------------------------|-----------------|  
|<span data-ttu-id="785a8-112">**/logdir:**< LogDirectory\></span><span class="sxs-lookup"><span data-stu-id="785a8-112">**/logdir:**<LogDirectory\></span></span>|<span data-ttu-id="785a8-113">Valfri.</span><span class="sxs-lookup"><span data-stu-id="785a8-113">Optional.</span></span> <span data-ttu-id="785a8-114">hello loggkatalogen.</span><span class="sxs-lookup"><span data-stu-id="785a8-114">hello log directory.</span></span> <span data-ttu-id="785a8-115">Utförlig loggfilerna skrivs toothis directory.</span><span class="sxs-lookup"><span data-stu-id="785a8-115">Verbose log files will be written toothis directory.</span></span> <span data-ttu-id="785a8-116">Om inga loggkatalogen anges används hello katalogen som hello loggkatalogen.</span><span class="sxs-lookup"><span data-stu-id="785a8-116">If no log directory is specified, hello current directory will be used as hello log directory.</span></span>|  
|<span data-ttu-id="785a8-117">**/SN:**< StorageAccountName\></span><span class="sxs-lookup"><span data-stu-id="785a8-117">**/sn:**<StorageAccountName\></span></span>|<span data-ttu-id="785a8-118">Krävs.</span><span class="sxs-lookup"><span data-stu-id="785a8-118">Required.</span></span> <span data-ttu-id="785a8-119">hello namnet på hello storage-konto för hello exportera jobb.</span><span class="sxs-lookup"><span data-stu-id="785a8-119">hello name of hello storage account for hello export job.</span></span>|  
|<span data-ttu-id="785a8-120">**/Sk:**< StorageAccountKey\></span><span class="sxs-lookup"><span data-stu-id="785a8-120">**/sk:**<StorageAccountKey\></span></span>|<span data-ttu-id="785a8-121">Krävs endast om en behållare SAS inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="785a8-121">Required if and only if a container SAS is not specified.</span></span> <span data-ttu-id="785a8-122">Hej kontonyckel hello storage-konto för hello exportera jobb.</span><span class="sxs-lookup"><span data-stu-id="785a8-122">hello account key for hello storage account for hello export job.</span></span>|  
|<span data-ttu-id="785a8-123">**/csas:**< ContainerSas\></span><span class="sxs-lookup"><span data-stu-id="785a8-123">**/csas:**<ContainerSas\></span></span>|<span data-ttu-id="785a8-124">Krävs endast om en lagringskontonyckel inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="785a8-124">Required if and only if a storage account key is not specified.</span></span> <span data-ttu-id="785a8-125">hello behållare SAS för lista hello blobbar toobe exporteras i hello exportjobb.</span><span class="sxs-lookup"><span data-stu-id="785a8-125">hello container SAS for listing hello blobs toobe exported in hello export job.</span></span>|  
|<span data-ttu-id="785a8-126">**/ ExportBlobListFile:**< ExportBlobListFile\></span><span class="sxs-lookup"><span data-stu-id="785a8-126">**/ExportBlobListFile:**<ExportBlobListFile\></span></span>|<span data-ttu-id="785a8-127">Krävs.</span><span class="sxs-lookup"><span data-stu-id="785a8-127">Required.</span></span> <span data-ttu-id="785a8-128">Sökvägen toohello XML-fil som innehåller listan över blob-sökvägar eller blob sökväg-prefix för hello blobbar toobe exporteras.</span><span class="sxs-lookup"><span data-stu-id="785a8-128">Path toohello XML file containing list of blob paths or blob path prefixes for hello blobs toobe exported.</span></span> <span data-ttu-id="785a8-129">hello-filformat som används i hello `BlobListBlobPath` element i hello [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) driften av hello Import/Export service REST API.</span><span class="sxs-lookup"><span data-stu-id="785a8-129">hello file format used in hello `BlobListBlobPath` element in hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation of hello Import/Export service REST API.</span></span>|  
|<span data-ttu-id="785a8-130">**/ DriveSize:**< DriveSize\></span><span class="sxs-lookup"><span data-stu-id="785a8-130">**/DriveSize:**<DriveSize\></span></span>|<span data-ttu-id="785a8-131">Krävs.</span><span class="sxs-lookup"><span data-stu-id="785a8-131">Required.</span></span> <span data-ttu-id="785a8-132">Hej storleken på enheter toouse för ett exportjobb *t.ex.*, 500 GB, 1,5 TB.</span><span class="sxs-lookup"><span data-stu-id="785a8-132">hello size of drives toouse for an export job, *e.g.*, 500GB, 1.5TB.</span></span>|  

## <a name="command-line-example"></a><span data-ttu-id="785a8-133">Kommandoradsverktyget exempel</span><span class="sxs-lookup"><span data-stu-id="785a8-133">Command-line example</span></span>

<span data-ttu-id="785a8-134">hello exemplet nedan visar hello `PreviewExport` kommando:</span><span class="sxs-lookup"><span data-stu-id="785a8-134">hello following example demonstrates hello `PreviewExport` command:</span></span>  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
<span data-ttu-id="785a8-135">hello kan exportfilen blob listan innehålla blobbnamnen och blob-prefix, som visas här:</span><span class="sxs-lookup"><span data-stu-id="785a8-135">hello export blob list file may contain blob names and blob prefixes, as shown here:</span></span>  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

<span data-ttu-id="785a8-136">hello Azure Import/Export-verktyget visar en lista över alla blobbar toobe exporteras och beräknar hur toopack dem till enheter i hello angiven storlek, med hänsyn till alla nödvändiga kostnader, sedan beräknar hello antalet enheter som krävs för toohold hello blobbar och diskanvändning information.</span><span class="sxs-lookup"><span data-stu-id="785a8-136">hello Azure Import/Export Tool lists all blobs toobe exported and calculates how toopack them into drives of hello specified size, taking into account any necessary overhead, then estimates hello number of drives needed toohold hello blobs and drive usage information.</span></span>  
  
<span data-ttu-id="785a8-137">Här är ett exempel på hello utdata med informativt loggar utelämnas:</span><span class="sxs-lookup"><span data-stu-id="785a8-137">Here is an example of hello output, with informational logs omitted:</span></span>  
  
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
  
## <a name="next-steps"></a><span data-ttu-id="785a8-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="785a8-138">Next steps</span></span>

* [<span data-ttu-id="785a8-139">Referens för Azure Import/Export-verktyg</span><span class="sxs-lookup"><span data-stu-id="785a8-139">Azure Import/Export Tool reference</span></span>](../storage-import-export-tool-how-to-v1.md)
