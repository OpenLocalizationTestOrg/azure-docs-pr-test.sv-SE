---
title: "aaa ”Azure Batch-pool ta bort händelsen klar | Microsoft Docs ”"
description: "Referens för Batch-pool att ta bort händelsen klar."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 494c371e48ebfb1bf3d2973a7401829a939ba141
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pool-delete-complete-event"></a>Händelsen i poolen ta bort klar

 Denna händelse genereras när en programpool borttagningen har slutförts.

 hello visar följande exempel hello brödtexten i en pool delete complete-händelse.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Element|Typ|Anteckningar|
|-------------|----------|-----------|
|id|Sträng|hello-id för hello poolen.|
|startTime|Datum och tid|hello när hello poolen tar bort startas.|
|endTime|Datum och tid|hello slutförts när hello poolen tar bort.|

## <a name="remarks"></a>Kommentarer
Mer information om tillstånd och felkoder för åtgärden Ändra storlek för poolen finns [ta bort poolen från ett konto](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).