---
title: "aaaSample arbetsflöde tooprep hårddiskar för Azure Import/Export-importjobb - v1 | Microsoft Docs"
description: "Se en genomgång för hello Slutför process förbereder enheter för ett importjobb i hello Azure Import/Export service."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 6eb1b1b7-c69f-4365-b5ef-3cd5e05eb72a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: eb77831a88c16c14838179a6432ddb06503067dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sample-workflow-tooprepare-hard-drives-for-an-import-job"></a>Exempel arbetsflödet tooprepare hårddiskar för ett importjobb
Det här avsnittet vägleder dig genom hello Slutför process förbereder enheter för importen.  
  
Det här exemplet importerar hello följande data till en Windows Azure storage-konto med namnet `mystorageaccount`:  
  
|Plats|Beskrivning|  
|--------------|-----------------|  
|H:\Video|En samling videor, 5 TB totalt.|  
|H:\Photo|En samling foton, 30 GB totalt.|  
|K:\Temp\FavoriteMovie.ISO|A Blu-ray™ diskavbildning, 25 GB.|  
|\\\bigshare\john\music|En samling av musik på en nätverksresurs, 10 GB totalt.|  
  
hello importjobbet importerar data till hello följande mål i hello storage-konto:  
  
|Källa|Mål virtuell katalog eller blob|  
|------------|-------------------------------------------|  
|H:\Video|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Photo|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|  
|K:\Temp\FavoriteMovie.ISO|https://mystorageaccount.BLOB.Core.Windows.NET/favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|https://mystorageaccount.BLOB.Core.Windows.NET/Music|  
  
Med den här mappningen hello filen `H:\Video\Drama\GreatMovie.mov` är importerade toohello blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.  
  
Nästa toodetermine hur många hårddiskar krävs beräkning hello storleken på hello data:  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
I det här exemplet är två 3 TB-hårddiskar tillräckliga. Eftersom hello källkatalog `H:\Video` har 5 TB data och den enda hårddisken kapacitet är 3 TB, är det nödvändigt toobreak `H:\Video` i två mindre kataloger: `H:\Video1` och `H:\Video2`innan körs hello Microsoft Verktyget Azure Import/Export. Det här steget ger hello efter källan:  
  
|Plats|Storlek|Mål virtuell katalog eller blob|  
|--------------|----------|-------------------------------------------|  
|H:\Video1|2,5 TB|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Video2|2,5 TB|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Photo|30 GB|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|  
|K:\Temp\FavoriteMovies.ISO|25 GB|https://mystorageaccount.BLOB.Core.Windows.NET/favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|10 GB|https://mystorageaccount.BLOB.Core.Windows.NET/Music|  
  
 Även om hello `H:\Video`directory har delats tootwo kataloger, de peka toohello samma mål virtuell katalog på hello storage-konto. På så sätt kan alla videofiler bevaras under en enda `video` behållare i hello storage-konto.  
  
 Därefter är hello tidigare källa kataloger jämnt fördelade toohello två hårddiskar:  
  
||||  
|-|-|-|  
|Hårddisk|Källan|Total storlek|  
|Första enhet|H:\Video1|2,5 TB + 30 GB|  
||H:\Photo||  
|Andra enhet|H:\Video2|2,5 TB + 35 GB|  
||K:\Temp\BlueRay.ISO||  
||\\\bigshare\john\music||  
  
Du kan dessutom ange hello efter metadata för alla filer:  
  
-   **UploadMethod:** Windows Azure Import/Export service  
  
-   **DataSetName:** SampleData  
  
-   **CreationDate:** 10/1/2013  
  
tooset metadata för hello importeras filer, skapa en textfil `c:\WAImportExport\SampleMetadata.txt`, med hello följande innehåll:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
Du kan också ange vissa egenskaper för hello `FavoriteMovie.ISO` blob:  
  
-   **Content-Type:** program/oktett-ström  
  
-   **Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==  
  
-   **Cache-Control:** no-cache  
  
tooset dessa egenskaper, skapa en textfil `c:\WAImportExport\SampleProperties.txt`:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
Nu är du redo toorun hello Azure Import/Export verktyget tooprepare hello två hårddiskar. Tänk på följande:  
  
-   hello första enheten är monterad enhet X.  
  
-   andra hello-enhet är monterad enhet Y.  
  
-   hello nyckel för hello lagringskonto `mystorageaccount` är `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a>Förbereda disk för import när data har redan lästs in
 
 Om det redan finns på disken hello hello data toobe importeras, Använd hello flaggan /skipwrite. hello-värdet för /t och /srcdir ska båda punkt toohello disk förbereds för import. Om alla hello data toobe importeras inte ska toohello samma mål virtuell katalog eller roten för hello storage-konto, kör hello samma kommando för varje målkatalogen separat, hålla hello värdet för /id hello samma över alla körs.

>[!NOTE] 
>Ange inte/format som den kommer att radera hello data på hello disk. Du kan ange / kryptera eller /bk beroende på om hello disken redan är krypterad eller inte. 
>

```
    When data is already present on hello disk for each drive run hello following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a>Kopiera sessioner - enheten först

Första hello-enheten kör hello Azure Import/Export-verktyget datakällan två gånger toocopy hello två kataloger:  

**Först kopiera session**
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

**Den andra kopian sessionen**

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a>Kopiera sessioner - andra enhet
 
För hello andra enhet som kör hello Azure Import/Export verktyget tre gånger när varje hello datakälla kataloger och en gång för hello fristående Blu-Ray™ bildfilen):  
  
**Först kopiera session** 

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Video2 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:y /format /encrypt /srcdir:H:\Video2 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```
  
**Den andra kopian sessionen**

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Music /srcdir:\\bigshare\john\music /dstdir:music/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```  
  
**Den tredje kopia session**  

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```

## <a name="copy-session-completion"></a>Kopiera session slutförande

När hello kopiera sessioner har slutfört kan du koppla hello två enheter från hello kopiera dator och leverera dem toohello lämplig Windows Azure-datacenter. Överför hello två journalfiler `FirstDrive.jrn` och `SecondDrive.jrn`, när du skapar hello importjobb i hello [Windows Azure-portalen](https://manage.windowsazure.com/).  
  
## <a name="next-steps"></a>Nästa steg

* [Förbereda hårddiskar för ett importjobb](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Snabbreferens för vanliga kommandon](../storage-import-export-tool-quick-reference-v1.md) 
