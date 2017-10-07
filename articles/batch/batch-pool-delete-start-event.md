---
title: "aaa ”Azure Batch-pool borttagning start-händelsen | Microsoft Docs ”"
description: "Referens för Batch-pool delete Starta händelsen."
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
ms.openlocfilehash: 79bb28bffc760a49cc0a95062f5086dc96c6a795
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pool-delete-start-event"></a>Starta händelsen för poolen borttagning

 Denna händelse genereras när en programpool borttagningsåtgärd har startats. Eftersom hello poolen delete är en asynkron händelse, du kan förvänta dig poolen ta bort händelsen klar toobe orsakat när hello borttagningsåtgärden har slutförts.

 hello visar följande exempel hello brödtexten i en pool borttagning start-händelse.

```
{
    "id": "myPool1"
}
```

|Element|Typ|Anteckningar|
|-------------|----------|-----------|
|id|Sträng|hello-id för hello poolen.|
