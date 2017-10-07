---
title: "aaaSample arbetsflöde tooprep hårddiskar för Azure Import/Export-importjobb | Microsoft Docs"
description: "Se en genomgång för hello Slutför process förbereder enheter för ett importjobb i hello Azure Import/Export service."
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
ms.date: 04/07/2017
ms.author: muralikk
ms.openlocfilehash: 560220b7dc9f87416f1fec1ff30fa5cd65812ce5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sample-workflow-tooprepare-hard-drives-for-an-import-job"></a>Exempel arbetsflödet tooprepare hårddiskar för ett importjobb

Den här artikeln vägleder dig genom hello Slutför process förbereder enheter för importen.

## <a name="sample-data"></a>Exempeldata

Det här exemplet importerar hello efter data i ett Azure storage-konto med namnet `mystorageaccount`:

|Plats|Beskrivning|Datastorlek|
|--------------|-----------------|-----|
|H:\Video\ |En samling videor|12 TB|
|H:\Photo\ |En samling bilder|30 GB|
|K:\Temp\FavoriteMovie.ISO|A Blu-ray™ diskavbildning|25 GB|
|\\\bigshare\john\music\|En samling musikfiler på en nätverksresurs|10 GB|

## <a name="storage-account-destinations"></a>Mål för Storage-konto

hello importjobbet importeras hello data till hello följande mål i hello storage-konto:

|Källa|Mål virtuell katalog eller blob|
|------------|-------------------------------------------|
|H:\Video\ |video /|
|H:\Photo\ |foto /|
|K:\Temp\FavoriteMovie.ISO|favorite/FavoriteMovies.ISO|
|\\\bigshare\john\music\ |Musik|

Med den här mappningen hello filen `H:\Video\Drama\GreatMovie.mov` blir importerade toohello blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.

## <a name="determine-hard-drive-requirements"></a>Ange krav för hårddisk

Nästa toodetermine hur många hårddiskar krävs beräkning hello storleken på hello data:

`12TB + 30GB + 25GB + 10GB = 12TB + 65GB`

I det här exemplet är två 8TB-hårddiskar tillräckliga. Eftersom hello källkatalog `H:\Video` har 12TB data och den enda hårddisken kapacitet är 8TB, kommer du att kunna toospecify detta i hello följande sätt i hello **driveset.csv** fil:

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
X,Format,SilentMode,Encrypt,
Y,Format,SilentMode,Encrypt,
```
hello verktyget distribuera data på två hårddiskar på ett optimerat sätt.

## <a name="attach-drives-and-configure-hello-job"></a>Koppla enheter och konfigurera hello jobb
Du ska ansluta båda diskarna toohello datorn och skapa volymer. Sedan redigerar **dataset.csv** fil:
```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

Du kan dessutom ange hello efter metadata för alla filer:

* **UploadMethod:** Windows Azure Import/Export service
* **DataSetName:** SampleData
* **CreationDate:** 10/1/2013

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

* **Content-Type:** program/oktett-ström
* **Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==
* **Cache-Control:** no-cache

tooset dessa egenskaper, skapa en textfil `c:\WAImportExport\SampleProperties.txt`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

## <a name="run-hello-azure-importexport-tool-waimportexportexe"></a>Kör hello Azure Import/Export-verktyget (WAImportExport.exe)

Nu är du redo toorun hello Azure Import/Export verktyget tooprepare hello två hårddiskar.

**För hello första sessionen:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

Om fler data måste toobe som lagts till, skapar du en annan dataset-fil (samma format som Initialdataset).

**För hello andra sessionen:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

När hello kopiera sessioner har slutfört kan du koppla hello två enheter från hello kopiera dator och leverera dem toohello lämplig Azure-datacenter. Du måste överföra hello två journalfiler `<FirstDriveSerialNumber>.xml` och `<SecondDriveSerialNumber>.xml`, när du skapar hello importjobb i hello Azure-portalen.

## <a name="next-steps"></a>Nästa steg

* [Förbereda hårddiskar för ett importjobb](storage-import-export-tool-preparing-hard-drives-import.md)
* [Snabbreferens för vanliga kommandon](storage-import-export-tool-quick-reference.md)
