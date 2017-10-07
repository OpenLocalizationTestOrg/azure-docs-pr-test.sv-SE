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
ms.openlocfilehash: 88495f921371458c0451da6878fd7cc9a45d20cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="previewing-drive-usage-for-an-export-job"></a>Förhandsgranska diskanvändning för ett exportjobb
Innan du skapar ett exportjobb måste toochoose en uppsättning blobbar toobe exporteras. hello Microsoft Azure Import/Export service kan du toouse en lista över blob-sökvägar eller blob-prefix toorepresent hello blob som du har valt.  
  
Sedan måste toodetermine hur många enheter du behöver toosend. hello verktyget Import/Export ger hello `PreviewExport` kommandot toopreview diskanvändning för hello blob som du har valt, baserat på hello storlek hello enheter du kommer toouse.

## <a name="command-line-parameters"></a>Kommandoradsparametrar

Du kan använda följande parametrar när du använder hello hello `PreviewExport` kommandot av hello verktyget Import/Export.

|Kommandoradsparametern|Beskrivning|  
|--------------------------|-----------------|  
|**/logdir:**< LogDirectory\>|Valfri. hello loggkatalogen. Utförlig loggfilerna skrivs toothis directory. Om inga loggkatalogen anges används hello katalogen som hello loggkatalogen.|  
|**/SN:**< StorageAccountName\>|Krävs. hello namnet på hello storage-konto för hello exportera jobb.|  
|**/Sk:**< StorageAccountKey\>|Krävs endast om en behållare SAS inte har angetts. Hej kontonyckel hello storage-konto för hello exportera jobb.|  
|**/csas:**< ContainerSas\>|Krävs endast om en lagringskontonyckel inte har angetts. hello behållare SAS för lista hello blobbar toobe exporteras i hello exportjobb.|  
|**/ ExportBlobListFile:**< ExportBlobListFile\>|Krävs. Sökvägen toohello XML-fil som innehåller listan över blob-sökvägar eller blob sökväg-prefix för hello blobbar toobe exporteras. hello-filformat som används i hello `BlobListBlobPath` element i hello [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) driften av hello Import/Export service REST API.|  
|**/ DriveSize:**< DriveSize\>|Krävs. Hej storleken på enheter toouse för ett exportjobb *t.ex.*, 500 GB, 1,5 TB.|  

## <a name="command-line-example"></a>Kommandoradsverktyget exempel

hello exemplet nedan visar hello `PreviewExport` kommando:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
hello kan exportfilen blob listan innehålla blobbnamnen och blob-prefix, som visas här:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

hello Azure Import/Export-verktyget visar en lista över alla blobbar toobe exporteras och beräknar hur toopack dem till enheter i hello angiven storlek, med hänsyn till alla nödvändiga kostnader, sedan beräknar hello antalet enheter som krävs för toohold hello blobbar och diskanvändning information.  
  
Här är ett exempel på hello utdata med informativt loggar utelämnas:  
  
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
  
## <a name="next-steps"></a>Nästa steg

* [Referens för Azure Import/Export-verktyg](storage-import-export-tool-how-to-v1.md)
