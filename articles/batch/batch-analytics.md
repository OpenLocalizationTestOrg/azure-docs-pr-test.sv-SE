---
title: aaaAzure Batch Analytics | Microsoft Docs
description: "Referens för Azure Batch Analytics."
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
ms.openlocfilehash: 462fbad1ac522b485ae18c1e8891b9d2cabd45e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="batch-analytics"></a>Batch Analytics
hello innehåller Batch Analytics referensinformation för hello händelser och varningar som är tillgängliga för Batch-tjänsten resurser.

Se [Azure Batch diagnostikloggning](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) mer information om att aktivera och använda Batch diagnostikloggar.

## <a name="diagnostic-logs"></a>Diagnostikloggar

hello Azure Batch-tjänsten skickar hello följande diagnostiska logghändelser under hello livstid vissa Batch-resurser.

**Tjänsten logghändelser**
* [Skapa poolen](batch-pool-create-event.md)
* [Poolen delete start](batch-pool-delete-start-event.md)
* [Poolen ta bort klar](batch-pool-delete-complete-event.md)
* [Poolen storleksändring start](batch-pool-resize-start-event.md)
* [Poolen storleksändring slutförd](batch-pool-resize-complete-event.md)
* [Uppgiften start](batch-task-start-event.md)
* [Uppgift slutförd](batch-task-complete-event.md)
* [Aktiviteten misslyckas](batch-task-fail-event.md)