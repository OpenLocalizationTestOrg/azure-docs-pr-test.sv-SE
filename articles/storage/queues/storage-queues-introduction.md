---
title: aaaIntroduction tooAzure Queue storage | Microsoft Docs
description: Introduktion tooAzure Queue storage
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: robinsh
ms.openlocfilehash: 669effedff7beedde8a119c350a2a70edafedcf0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooqueues"></a>Introduktion tooQueues

Azure Queue storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i Hej världen via autentiserade anrop med HTTP eller HTTPS. Ett enda kömeddelande kan vara upp too64 KB i storlek och en kö kan innehålla miljontals meddelanden, upp toohello totala kapacitetsgränsen för ett lagringskonto.

## <a name="common-uses"></a>Vanliga användningsområden

Vanliga användningsområden för Queue Storage är:

* Skapa en eftersläpning i arbetet tooprocess asynkront
* Skicka meddelanden från en Azure web rollen tooan Azure-arbetsroll

## <a name="queue-service-concepts"></a>Kötjänst-koncept

hello kötjänsten innehåller hello följande komponenter:

![Kön begrepp](./media/storage-queues-introduction/queue1.png)

* **URL-format:** köer är adresserbara via följande URL-format hello:   
    http://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    hello följande URL adresserar en kö i hello diagram:  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* **Storage-konto:** komma åt tooAzure Storage görs genom ett lagringskonto. Se [Skalbarhets- och prestandamål för Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) för information om kapacitet för lagringskonton.

* **Kö:** en kö innehåller en uppsättning meddelanden. Alla meddelanden måste vara i en kö. Observera att hello könamnet måste vara gemener. Mer information om namngivning av köer finns i [namngivning av köer och metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **Meddelande:** A meddelandet, i valfritt format för in too64 KB. hello maximala tid som ett meddelande kan finnas i kön hello är sju dagar.

## <a name="next-steps"></a>Nästa steg

* [Skapa ett lagringskonto](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Komma igång med köer med hjälp av .NET](storage-dotnet-how-to-use-queues.md)
