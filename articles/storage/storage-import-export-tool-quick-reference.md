---
title: "aaaQuick referens för Azure Import/Export verktyget import jobbet kommandon | Microsoft Docs"
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
ms.openlocfilehash: 0a615aed938e5e1b52d55a340aa6b48fa0744367
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Snabbreferens för ofta använda kommandon för importjobb

Den här artikeln innehåller en översikt över för några vanliga kommandon. För detaljerad användning, se [förbereder hårddiskar för ett importjobb](storage-import-export-tool-preparing-hard-drives-import.md).

## <a name="first-session"></a>Första sessionen

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1 /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

## <a name="second-session"></a>Andra sessionen

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /DataSet:dataset-2.csv
```

## <a name="abort-latest-session"></a>Avbryt senaste sessionen

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /AbortSession
```

## <a name="resume-latest-interrupted-session"></a>Återuppta senaste avbruten session

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /ResumeSession
```

## <a name="add-drives-toolatest-session"></a>Lägg till enheter toolatest session

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /AdditionalDriveSet:driveset-2.csv
```

## <a name="next-steps"></a>Nästa steg

* [Exempel arbetsflödet tooprepare hårddiskar för ett importjobb](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
