---
title: aaaBacking in Azure Import/Export enhet manifesten | Microsoft Docs
description: "Lär dig hur toohave enheten visar för hello Microsoft Azure Import/Export service säkerhetskopieras automatiskt."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 594abd80-b834-4077-a474-d8a0f4b7928a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: f48b97a2cce62714aace2b30a393305202c7ecd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a>Säkerhetskopiera enheten visar för Azure Import/Export-jobb

Enheten manifest kan automatiskt säkerhetskopieras tooblobs genom att ange hello `BackupDriveManifest` egenskapen för`true` i hello [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) eller [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) REST-API: et. Som standard säkerhetskopieras hello enhet manifesten inte. hello enhet manifestet säkerhetskopior lagras som blockblobar i en behållare i hello storage-konto som är associerade med hello jobb. Hello behållarens namn är som standard `waimportexport`, men du kan ange ett annat namn i hello `DiagnosticsPath` egenskapen när du anropar hello `Put Job` eller `Update Job Properties` åtgärder. hello säkerhetskopiering manifestet blob namnges i hello följande format: `waies/jobname_driveid_timestamp_manifest.xml`.

 Du kan hämta hello hello enhet-URI-manifestet för ett jobb genom att anropa hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) igen. hello blob-URI returneras hello `ManifestUri` egenskap för varje enhet.

## <a name="next-steps"></a>Nästa steg

* [Med hjälp av hello Import/Export service REST API](storage-import-export-using-the-rest-api.md)
