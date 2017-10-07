---
title: "aaa ”Azure Batch-pool ändra storlek på händelsen klar | Microsoft Docs ”"
description: "Referens för Batch-pool ändra storlek på händelsen klar."
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
ms.openlocfilehash: dc64711a01aa4cf6192edba1a2c4cad56f953766
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pool-resize-complete-event"></a>Fullständig storleksändringar pool

 Denna händelse genereras när en storlek för poolen har slutförts eller misslyckades.

 hello visar följande exempel hello brödtexten i en pool fullständig storleksändringar för en pool som ökar i storlek och har slutförts.

```
{
    "id": "p_1_0_01503750-252d-4e57-bd96-d6aa05601ad8",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 4,
    "targetDedicated": 4,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "result": "Success",
    "resizeError": "hello operation succeeded"
}
```

|Element|Typ|Anteckningar|
|-------------|----------|-----------|
|id|Sträng|hello-id för hello poolen.|
|nodeDeallocationOption|Sträng|Anger om noder kan tas bort från poolen hello, om hello poolstorleken minskar.<br /><br /> Möjliga värden:<br /><br /> **meddelanden** – avsluta pågående aktiviteter och ställ dem i kö. hello aktiviteterna körs igen när jobbet hello är aktiverat. Ta bort noder när aktiviteterna har avslutats.<br /><br /> **Avsluta** – avsluta pågående aktiviteter. hello aktiviteter körs inte igen. Ta bort noder när aktiviteterna har avslutats.<br /><br /> **taskcompletion** – Tillåt pågående aktiviteter toocomplete. Schemalägg inga nya aktiviteter väntan. Ta bort noder när alla aktiviteter har slutförts.<br /><br /> **Retaineddata** - Låt pågående aktiviteter toocomplete och vänta tills alla aktivitets datakvarhållning punkter tooexpire. Schemalägg inga nya aktiviteter väntan. Ta bort noder när kvarhållningsperioder för alla aktivitet har upphört att gälla.<br /><br /> hello standardvärdet är meddelanden.<br /><br /> Om hello poolstorlek ökar sedan hello värdet för**ogiltigt**.|
|currentDedicated|Int32|hello antalet compute-noder tilldelade för närvarande toohello pool.|
|targetDedicated|Int32|hello antal compute-noder som har begärts för hello poolen.|
|enableAutoScale|bool|Anger om justerar hello poolstorlek automatiskt med tiden.|
|isAutoPool|bool|Anger om hello poolen har skapats via ett jobb AutoPool mekanism.|
|startTime|Datum och tid|hello tid hello poolen ändra storlek på Starta.|
|endTime|Datum och tid|hello slutförts tid hello poolen ändra storlek på.|
|ResultCode|Sträng|hello resultatet av hello storlek.|
|resultMessage|Sträng|hello storleksändring fel innehåller hello information om hello resultat.<br /><br /> Om hello ändrar storlek på slutförts det tillstånd som hello åtgärden lyckades.|
