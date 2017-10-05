---
title: "Snabbreferens för Azure Import/Export verktyget import jobbet kommandon | Microsoft Docs"
description: "Azure Import/Export-verktyget kommandoreferens för vanliga importera jobbet kommandon."
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
ms.openlocfilehash: e9377e0c5001cf5be220e19e06ff96c1e058e853
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a><span data-ttu-id="13e0f-103">Snabbreferens för ofta använda kommandon för importjobb</span><span class="sxs-lookup"><span data-stu-id="13e0f-103">Quick reference for frequently used commands for import jobs</span></span>

<span data-ttu-id="13e0f-104">Den här artikeln innehåller en översikt över för några vanliga kommandon.</span><span class="sxs-lookup"><span data-stu-id="13e0f-104">This article provides a quick reference for some frequently used commands.</span></span> <span data-ttu-id="13e0f-105">För detaljerad användning, se [förbereder hårddiskar för ett importjobb](storage-import-export-tool-preparing-hard-drives-import.md).</span><span class="sxs-lookup"><span data-stu-id="13e0f-105">For detailed usage, see [Preparing Hard Drives for an Import Job](storage-import-export-tool-preparing-hard-drives-import.md).</span></span>

## <a name="first-session"></a><span data-ttu-id="13e0f-106">Första sessionen</span><span class="sxs-lookup"><span data-stu-id="13e0f-106">First session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1 /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

## <a name="second-session"></a><span data-ttu-id="13e0f-107">Andra sessionen</span><span class="sxs-lookup"><span data-stu-id="13e0f-107">Second session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /DataSet:dataset-2.csv
```

## <a name="abort-latest-session"></a><span data-ttu-id="13e0f-108">Avbryt senaste sessionen</span><span class="sxs-lookup"><span data-stu-id="13e0f-108">Abort latest session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /AbortSession
```

## <a name="resume-latest-interrupted-session"></a><span data-ttu-id="13e0f-109">Återuppta senaste avbruten session</span><span class="sxs-lookup"><span data-stu-id="13e0f-109">Resume latest interrupted session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /ResumeSession
```

## <a name="add-drives-to-latest-session"></a><span data-ttu-id="13e0f-110">Lägga till enheter i senaste sessionen</span><span class="sxs-lookup"><span data-stu-id="13e0f-110">Add drives to latest session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /AdditionalDriveSet:driveset-2.csv
```

## <a name="next-steps"></a><span data-ttu-id="13e0f-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="13e0f-111">Next steps</span></span>

* [<span data-ttu-id="13e0f-112">Exempelarbetsflöde för att förbereda hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="13e0f-112">Sample workflow to prepare hard drives for an import job</span></span>](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
