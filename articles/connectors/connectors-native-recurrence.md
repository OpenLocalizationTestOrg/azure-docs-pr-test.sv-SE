---
title: "aaaAdd hello upprepning utlösaren i logikappar | Microsoft Docs"
description: "Översikt över hello upprepning utlösare och hur toouse den med en Azure logikapp."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 51dd4f22-7dc5-41af-a0a9-e7148378cd50
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: e7c625c382a88a1e7cdfff4ddc0caf55727232bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-recurrence-trigger"></a>Kom igång med hello upprepning utlösare
Du kan skapa kraftfulla arbetsflöden i hello molnet med hjälp av hello upprepning utlösare.

Du kan till exempel:

* Schemalägga ett arbetsflöde toorun en SQL-lagrade procedur varje dag.
* E-en sammanfattning av alla tweets inom hello föregående vecka. Om en viss hashtaggar.

tooget igång med hello upprepning utlösaren i en logikapp finns [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Använda en utlösare för upprepning
En utlösare är en händelse som kan använda toostart hello arbetsflöde som har definierats i en logikapp. [Mer information om utlösare](connectors-overview.md).

Här är en Exempelsekvens av hur tooset in en upprepning utlösare i en logikapp:

1. Lägg till hello **återkommande** utlösare som hello första steg i en logikapp.
2. Fyll i hello parametrar för hello upprepningsintervallet.

Hej logikapp startar en körning efter varje tidsintervall.

![HTTP-utlösare](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Information om utlösaren
hello upprepning utlösaren har följande egenskaper som du kan konfigurera hello.

Den utlöses en logikapp efter ett angivet tidsintervall.
A * innebär att det är ett obligatoriskt fält.

| Visningsnamn | Egenskapsnamn | Beskrivning |
| --- | --- | --- |
| Frekvens * |frequency |hello tidsenhet: `Second`, `Minute`, `Hour`, `Day`, eller `Year`. |
| Intervall * |interval |hello tidsintervall hello angivna frekvensen för hello upprepning. |
| Tidszon |Tidszon |Om en starttid anges utan en UTC-förskjutningen, används den här tidszonen. |
| Starttid |startTime |hello starttid i [ISO 8601-format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). |

<br>

## <a name="next-steps"></a>Nästa steg
Prova nu hello plattform och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md). Du kan utforska hello andra tillgängliga kopplingar i logikappar genom att titta på vår [API: er listan](apis-list.md).

