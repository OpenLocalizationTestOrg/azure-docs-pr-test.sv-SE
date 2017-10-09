---
title: "aaaQuick referens för Azure Import/Export verktyget import jobbet kommandon - v1 | Microsoft Docs"
description: "Azure Import/Export-verktyget kommandoreferens för vanliga importera jobbet kommandon. Detta refererar toov1 av hello verktyget Import/Export."
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
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: e36f065e5d23268758cf6b6db9428fe8a8e1056d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Snabbreferens för ofta använda kommandon för importjobb
Det här avsnittet ger en snabb referenser för några vanliga kommandon. För detaljerad användning, se [förbereder hårddiskar för ett importjobb](storage-import-export-tool-preparing-hard-drives-import-v1.md).  

## <a name="prepare-hello-disks-when-data-already-copied-toohello-disks"></a>Förbereda hello diskar när data kopieras redan toohello diskar
 Här är ett exempel kommandot tooprepare en diskar när data kopieras redan toohello hårddisk som inte har ännu tagits krypteras med BitLocker:  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-tooa-hard-drive"></a>Kopiera en enda directory tooa hårddisk  
 Här är ett exempel kommandot toocopy som en enskild datakälla directory tooa hårddisk som inte har ännu tagits krypteras med BitLocker:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-wwo-directories-tooa-hard-drive"></a>Kopiera kataloger wwo tooa hårddisk  
 toocopy två kataloger tooa källenheten, behöver du två kommandon.  
  
 hello första kommandot anger hello loggkatalogen, lagringskontonyckel, mål enhetsbeteckning och `format/encrypt` krav i tillägget toohello gemensamma parametrar:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 hello andra kommandot anger hello journal-fil, en ny session-ID och hello käll- och målplatserna:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-tooa-hard-drive-in-a-second-copy-session"></a>Kopiera en stor fil tooa hårddisk i en andra kopia-session  
 Här är ett Exempelkommando som kopierar en enda stor fil tooa enhet som har förberetts i en tidigare session kopia:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a>Nästa steg

* [Exempel arbetsflödet tooprepare hårddiskar för ett importjobb](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
