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
ms.openlocfilehash: 945eb4f9eff28c92ec963585d27cba73a7eb59bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Snabbreferens för ofta använda kommandon för importjobb
Det här avsnittet innehåller en översikt över för några vanliga kommandon. För detaljerad användning, se [förbereder hårddiskar för ett importjobb](../storage-import-export-tool-preparing-hard-drives-import-v1.md).  

## <a name="prepare-a-hard-drive-when-data-has-already-been-copied-toohello-hard-drive"></a>Förbereda en hårddisk när data har redan kopieras toohello hårddisk
 hello följande kommando förbereds en hårddisk när data har redan kopierade tooit men ännu inte har krypterats med BitLocker:  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-tooa-hard-drive"></a>Kopiera en enda directory tooa hårddisk  
 följande kommando hello kopierar en enda källa directory tooa hårddisk som ännu inte har krypterats med BitLocker:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-two-directories-tooa-hard-drive"></a>Kopiera två kataloger tooa hårddisk  
 toocopy två kataloger tooa källenheten, Använd hello följande kommandon:  
  
 hello första kommandot anger hello loggkatalogen, lagringskontonyckel, target enhetsbeteckningsordning, `format/encrypt` krav och gemensamma parametrar:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 hello andra kommandot anger hello journal-fil, en ny session-ID och hello käll- och målplatserna:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-tooa-hard-drive-in-a-second-copy-session"></a>Kopiera en stor fil tooa hårddisk i en andra kopia-session  
 följande kommando hello kopierar en enda stor fil tooa hårddisk som förbereddes i en tidigare session kopia:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a>Nästa steg

* [Exempel arbetsflödet tooprepare hårddiskar för ett importjobb](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
