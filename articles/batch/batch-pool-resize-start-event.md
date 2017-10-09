---
title: "aaa ”Azure Batch-pool storleksändring Starta händelsen | Microsoft Docs ”"
description: "Referens för Batch-pool storleksändringar start."
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
ms.openlocfilehash: 2ca2a4f1195c3f785ae5b051b63340f70eecbc22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pool-resize-start-event"></a>Poolen storleksändringar start

 Denna händelse genereras när en storlek för poolen har startats. Eftersom hello poolen storlek är en asynkron händelse, du kan förvänta dig poolen storleksändring slutförd händelse toobe orsakat när hello att ändra storlek på har slutförts.

 följande exempel visar hello brödtexten i en pool start storleksändringar för en pool storleksändring från 0 too2 noder med en manuell hello storlek.

```
{
    "poolId": "myPool1",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 0,
    "targetDedicated": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|Element|Typ|Anteckningar|
|-------------|----------|-----------|
|poolId|Sträng|hello-id för hello poolen.|
|nodeDeallocationOption|Sträng|Anger om noder kan tas bort från poolen hello, om hello poolstorleken minskar.<br /><br /> Möjliga värden:<br /><br /> **meddelanden** – avsluta pågående aktiviteter och ställ dem i kö. hello aktiviteterna körs igen när jobbet hello är aktiverat. Ta bort noder när aktiviteterna har avslutats.<br /><br /> **Avsluta** – avsluta pågående aktiviteter. hello aktiviteter körs inte igen. Ta bort noder när aktiviteterna har avslutats.<br /><br /> **taskcompletion** – Tillåt pågående aktiviteter toocomplete. Schemalägg inga nya aktiviteter väntan. Ta bort noder när alla aktiviteter har slutförts.<br /><br /> **Retaineddata** - Låt pågående aktiviteter toocomplete och vänta tills alla aktivitets datakvarhållning punkter tooexpire. Schemalägg inga nya aktiviteter väntan. Ta bort noder när kvarhållningsperioder för alla aktivitet har upphört att gälla.<br /><br /> hello standardvärdet är meddelanden.<br /><br /> Om hello poolstorlek ökar sedan hello värdet för**ogiltigt**.|
|currentDedicated|Int32|hello antalet compute-noder tilldelade för närvarande toohello pool.|
|targetDedicated|Int32|hello antal compute-noder som har begärts för hello poolen.|
|enableAutoScale|bool|Anger om justerar hello poolstorlek automatiskt med tiden.|
|isAutoPool|bool|Speficies om hello poolen har skapats via ett jobb AutoPool mekanism.|
