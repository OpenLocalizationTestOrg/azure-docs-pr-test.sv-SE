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
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a><span data-ttu-id="c1045-104">Snabbreferens för ofta använda kommandon för importjobb</span><span class="sxs-lookup"><span data-stu-id="c1045-104">Quick reference for frequently used commands for import jobs</span></span>
<span data-ttu-id="c1045-105">Det här avsnittet ger en snabb referenser för några vanliga kommandon.</span><span class="sxs-lookup"><span data-stu-id="c1045-105">This section provides a quick references for some frequently used commands.</span></span> <span data-ttu-id="c1045-106">För detaljerad användning, se [förbereder hårddiskar för ett importjobb](storage-import-export-tool-preparing-hard-drives-import-v1.md).</span><span class="sxs-lookup"><span data-stu-id="c1045-106">For detailed usage, see [Preparing Hard Drives for an Import Job](storage-import-export-tool-preparing-hard-drives-import-v1.md).</span></span>  

## <a name="prepare-hello-disks-when-data-already-copied-toohello-disks"></a><span data-ttu-id="c1045-107">Förbereda hello diskar när data kopieras redan toohello diskar</span><span class="sxs-lookup"><span data-stu-id="c1045-107">Prepare hello disks when data already copied toohello disks</span></span>
 <span data-ttu-id="c1045-108">Här är ett exempel kommandot tooprepare en diskar när data kopieras redan toohello hårddisk som inte har ännu tagits krypteras med BitLocker:</span><span class="sxs-lookup"><span data-stu-id="c1045-108">Here is a sample command tooprepare a disks when data already copied toohello hard drive that hasn't been yet been encrypted with BitLocker:</span></span>  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-tooa-hard-drive"></a><span data-ttu-id="c1045-109">Kopiera en enda directory tooa hårddisk</span><span class="sxs-lookup"><span data-stu-id="c1045-109">Copy a single directory tooa hard drive</span></span>  
 <span data-ttu-id="c1045-110">Här är ett exempel kommandot toocopy som en enskild datakälla directory tooa hårddisk som inte har ännu tagits krypteras med BitLocker:</span><span class="sxs-lookup"><span data-stu-id="c1045-110">Here is a sample command toocopy a single source directory tooa hard drive that hasn't been yet been encrypted with BitLocker:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-wwo-directories-tooa-hard-drive"></a><span data-ttu-id="c1045-111">Kopiera kataloger wwo tooa hårddisk</span><span class="sxs-lookup"><span data-stu-id="c1045-111">Copy wwo directories tooa hard drive</span></span>  
 <span data-ttu-id="c1045-112">toocopy två kataloger tooa källenheten, behöver du två kommandon.</span><span class="sxs-lookup"><span data-stu-id="c1045-112">toocopy two source directories tooa drive, you will need two commands.</span></span>  
  
 <span data-ttu-id="c1045-113">hello första kommandot anger hello loggkatalogen, lagringskontonyckel, mål enhetsbeteckning och `format/encrypt` krav i tillägget toohello gemensamma parametrar:</span><span class="sxs-lookup"><span data-stu-id="c1045-113">hello first command specifies hello log directory, storage account key, target drive letter and `format/encrypt` requirements, in addition toohello common parameters:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 <span data-ttu-id="c1045-114">hello andra kommandot anger hello journal-fil, en ny session-ID och hello käll- och målplatserna:</span><span class="sxs-lookup"><span data-stu-id="c1045-114">hello second command specifies hello journal file, a new session ID, and hello source and destination locations:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-tooa-hard-drive-in-a-second-copy-session"></a><span data-ttu-id="c1045-115">Kopiera en stor fil tooa hårddisk i en andra kopia-session</span><span class="sxs-lookup"><span data-stu-id="c1045-115">Copy a large file tooa hard drive in a second copy session</span></span>  
 <span data-ttu-id="c1045-116">Här är ett Exempelkommando som kopierar en enda stor fil tooa enhet som har förberetts i en tidigare session kopia:</span><span class="sxs-lookup"><span data-stu-id="c1045-116">Here is a sample command that copies a single large file tooa drive that has been prepared in a previous copy session:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a><span data-ttu-id="c1045-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c1045-117">Next steps</span></span>

* [<span data-ttu-id="c1045-118">Exempel arbetsflödet tooprepare hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="c1045-118">Sample workflow tooprepare hard drives for an import job</span></span>](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
