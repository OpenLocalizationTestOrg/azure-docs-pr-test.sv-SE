---
title: aaaCancel och ta bort ett Azure Import/Export-jobb | Microsoft Docs
description: "Lär dig hur toocancel och ta bort jobb för hello Microsoft Azure Import/Export service."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: fd3d66f0-1dbb-4c75-9223-307d5abaeefc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 5d2aba510dafd0ca9a10f5643f721e7059a6a8f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Avbryter och ta bort Azure Import/Export-jobb

Du kan begära att ett jobb avbryts innan du använder hello `Packaging` tillstånd genom att anropa hello [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) åtgärden och inställningen hello `CancelRequested` element för`true`. hello-jobbet kommer att avbrytas för bästa prestanda. Om enheter är i hello processen att överföra data, kan data fortsätta toobe överförs även efter annullering har begärts.

 Avbrutna jobb flyttas toohello `Completed` tillstånd och bevaras under 90 dagar, då tas den bort.

 toodelete ett jobb anropet hello [ta bort jobbet](/rest/api/storageimportexport/jobs#Jobs_Delete) åtgärden innan hello jobbet har skickats (*d.v.s.*, medan hello jobb i hello `Creating` tillstånd). Du kan också ta bort ett jobb när den är i hello `Completed` tillstånd. När ett jobb har tagits bort, och dess status inte längre är tillgänglig via hello REST API eller hello Azure-portalen.

## <a name="next-steps"></a>Nästa steg

* [Med hjälp av hello Import/Export service REST API](storage-import-export-using-the-rest-api.md)
