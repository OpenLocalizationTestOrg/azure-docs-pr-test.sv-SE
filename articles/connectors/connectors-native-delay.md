---
title: "aaaAdd en fördröjning i logikappar | Microsoft Docs"
description: "Översikt över hello fördröjning och fördröjning-tills åtgärder, och hur toouse dem med en logikapp i Azure."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 915f48bf-3bd8-4656-be73-91a941d0afcd
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: e5bc9d639adbddc01ee0f6a4c68716f586d4344a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-delay-and-delay-until-actions"></a>Kom igång med hello fördröjning och fördröjning-tills åtgärder
Med hjälp av hello fördröjning och ”fördröjning-tills” åtgärder, kan du slutföra arbetsflödet scenarier.

Du kan till exempel:

* Vänta tills en veckodag toosend status uppdateras via e-post.
* Fördröjning hello arbetsflöde till ett HTTP-anrop har tid toofinish innan du fortsätter och hämta hello resultatet.

tooget igång med hello fördröjning åtgärden i en logikapp finns [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-hello-delay-actions"></a>Använda hello fördröjning åtgärder
En åtgärd är en åtgärd som utförs av hello arbetsflöde som har definierats i en logikapp. [Mer information om åtgärder](connectors-overview.md).

Här är en Exempelsekvens av hur toouse en fördröjning steg i en logikapp:

1. När du lägger till en utlösare, klickar du på **nytt steg** tooadd en åtgärd.
2. Sök efter **fördröjning** toobring in hello fördröjning åtgärder. I det här exemplet väljer vi **fördröjning**.
   
    ![Fördröjning åtgärder](./media/connectors-native-delay/using-action-1.png)
3. Slutför eventuella hello åtgärden egenskaper tooconfigure hello fördröjning.
   
    ![Fördröjning config](./media/connectors-native-delay/using-action-2.png)
4. Klicka på **spara** toopublish och aktivera hello logikapp.

## <a name="action-details"></a>Åtgärdsinformation
hello upprepning utlösaren har hello följande egenskaper som kan konfigureras.

### <a name="delay-action"></a>Fördröjning åtgärd
Den här åtgärden fördröjningar hello kör för ett visst tidsintervall.
A * innebär att det är ett obligatoriskt fält.

| Visningsnamn | Egenskapsnamn | Beskrivning |
| --- | --- | --- |
| Antal * |Antal |hello antalet tid enheter toodelay |
| A * |enhet |hello tidsenhet: `Second`, `Minute`, `Hour`, eller`Day` |

<br>

### <a name="delay-until-action"></a>Fördröjning-förrän åtgärden
Den här åtgärden fördröjningar hello körs tills angivet datum/tid.
A * innebär att det är ett obligatoriskt fält.

| Visningsnamn | Egenskapsnamn | Beskrivning |
| --- | --- | --- |
| År * |tidsstämpel |hello år toodelay tills (GMT) |
| Månad * |tidsstämpel |hello månad toodelay tills (GMT) |
| Dag * |tidsstämpel |hello dag toodelay tills (GMT) |

<br>

## <a name="next-steps"></a>Nästa steg
Prova nu hello plattform och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md). Du kan utforska hello andra tillgängliga kopplingar i logikappar genom att titta på vår [API: er listan](apis-list.md).

