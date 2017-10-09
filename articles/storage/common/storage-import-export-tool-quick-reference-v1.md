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
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a><span data-ttu-id="914fd-104">Snabbreferens för ofta använda kommandon för importjobb</span><span class="sxs-lookup"><span data-stu-id="914fd-104">Quick reference for frequently used commands for import jobs</span></span>
<span data-ttu-id="914fd-105">Det här avsnittet innehåller en översikt över för några vanliga kommandon.</span><span class="sxs-lookup"><span data-stu-id="914fd-105">This section provides a quick reference for some frequently used commands.</span></span> <span data-ttu-id="914fd-106">För detaljerad användning, se [förbereder hårddiskar för ett importjobb](../storage-import-export-tool-preparing-hard-drives-import-v1.md).</span><span class="sxs-lookup"><span data-stu-id="914fd-106">For detailed usage, see [Preparing Hard Drives for an Import Job](../storage-import-export-tool-preparing-hard-drives-import-v1.md).</span></span>  

## <a name="prepare-a-hard-drive-when-data-has-already-been-copied-toohello-hard-drive"></a><span data-ttu-id="914fd-107">Förbereda en hårddisk när data har redan kopieras toohello hårddisk</span><span class="sxs-lookup"><span data-stu-id="914fd-107">Prepare a hard drive when data has already been copied toohello hard drive</span></span>
 <span data-ttu-id="914fd-108">hello följande kommando förbereds en hårddisk när data har redan kopierade tooit men ännu inte har krypterats med BitLocker:</span><span class="sxs-lookup"><span data-stu-id="914fd-108">hello following command prepares a hard drive when data has already been copied tooit, but has not yet been encrypted with BitLocker:</span></span>  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-tooa-hard-drive"></a><span data-ttu-id="914fd-109">Kopiera en enda directory tooa hårddisk</span><span class="sxs-lookup"><span data-stu-id="914fd-109">Copy a single directory tooa hard drive</span></span>  
 <span data-ttu-id="914fd-110">följande kommando hello kopierar en enda källa directory tooa hårddisk som ännu inte har krypterats med BitLocker:</span><span class="sxs-lookup"><span data-stu-id="914fd-110">hello following command copies a single source directory tooa hard drive that has not yet been encrypted with BitLocker:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-two-directories-tooa-hard-drive"></a><span data-ttu-id="914fd-111">Kopiera två kataloger tooa hårddisk</span><span class="sxs-lookup"><span data-stu-id="914fd-111">Copy two directories tooa hard drive</span></span>  
 <span data-ttu-id="914fd-112">toocopy två kataloger tooa källenheten, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="914fd-112">toocopy two source directories tooa drive, use hello following commands:</span></span>  
  
 <span data-ttu-id="914fd-113">hello första kommandot anger hello loggkatalogen, lagringskontonyckel, target enhetsbeteckningsordning, `format/encrypt` krav och gemensamma parametrar:</span><span class="sxs-lookup"><span data-stu-id="914fd-113">hello first command specifies hello log directory, storage account key, target drive letter, `format/encrypt` requirements, and common parameters:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 <span data-ttu-id="914fd-114">hello andra kommandot anger hello journal-fil, en ny session-ID och hello käll- och målplatserna:</span><span class="sxs-lookup"><span data-stu-id="914fd-114">hello second command specifies hello journal file, a new session ID, and hello source and destination locations:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-tooa-hard-drive-in-a-second-copy-session"></a><span data-ttu-id="914fd-115">Kopiera en stor fil tooa hårddisk i en andra kopia-session</span><span class="sxs-lookup"><span data-stu-id="914fd-115">Copy a large file tooa hard drive in a second copy session</span></span>  
 <span data-ttu-id="914fd-116">följande kommando hello kopierar en enda stor fil tooa hårddisk som förbereddes i en tidigare session kopia:</span><span class="sxs-lookup"><span data-stu-id="914fd-116">hello following command copies a single large file tooa hard drive that was prepared in a previous copy session:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a><span data-ttu-id="914fd-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="914fd-117">Next steps</span></span>

* [<span data-ttu-id="914fd-118">Exempel arbetsflödet tooprepare hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="914fd-118">Sample workflow tooprepare hard drives for an import job</span></span>](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
