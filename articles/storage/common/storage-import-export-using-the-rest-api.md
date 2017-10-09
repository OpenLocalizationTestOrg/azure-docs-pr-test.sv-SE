---
title: aaaUsing hello Azure Import/Export service REST API | Microsoft Docs
description: "Lär dig mer om toofind resurser för att använda hello Azure Import/Export service REST-API, inklusive både hur tooand referensmaterialet."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 233f80e9-2e7f-48e0-9639-5c7785e7d743
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: fc7e1007ad632cf6f771c2545644f8de43c8f181
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-importexport-service-rest-api"></a>Med hjälp av hello Azure Import/Export service REST API

hello Microsoft Azure Import/Export service Exponerar en REST API tooenable programmässiga kontroll av import/export av jobb. Du kan använda hello REST API tooperform alla hello import/export-åtgärder som du kan utföra med hello [Azure-portalen](https://portal.azure.com/). Du kan dessutom använda hello REST API tooperform vissa detaljerade åtgärder, till exempel fråga hello procentandel slutförande av ett jobb som inte är tillgängliga i hello klassiska Azure-portalen.

Se [med hello Microsoft Azure Import/Export service tooTransfer Data tooBlob lagring](../storage-import-export-service.md) för en översikt över hello Import/Export service och en genomgång som visar hur toouse hello klassiska portal toocreate och hantera import och exportera jobben.

## <a name="service-endpoints"></a>slutpunkter

hello Azure Import/Export service är en resursleverantör för Azure Resource Manager och innehåller en uppsättning REST API: er på hello efter HTTPS-slutpunkt för att hantera import/export av jobb:

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a>Versionshantering

Begäranden toohello Import/Export service måste ange hello `api-version` parametern och ange värdet för`2016-11-01`.

## <a name="importexport-service-operations"></a>Importera och exportera tjänståtgärder

[Skapa ett importjobb](../storage-import-export-creating-an-import-job.md)

[Skapa ett exportjobb](../storage-import-export-creating-an-export-job.md)

[Hämta statusinformation för ett jobb](storage-import-export-retrieving-state-info-for-a-job.md)

[Räkna upp jobb](../storage-import-export-enumerating-jobs.md)

[Avbryta och ta bort jobb](storage-import-export-cancelling-and-deleting-jobs.md)

[Säkerhetskopiering av enheten manifest](../storage-import-export-backing-up-drive-manifests.md)

[Diagnostik och felåterställning av Import/Export-jobb](../storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a>Nästa steg

* [Storage Import/Export REST](/rest/api/storageimportexport)
