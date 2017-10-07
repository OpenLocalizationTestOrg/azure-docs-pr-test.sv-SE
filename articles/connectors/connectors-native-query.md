---
title: "aaaAdd hello frågeåtgärden i logikappar | Microsoft Docs"
description: "Översikt över hello frågeåtgärden för att utföra åtgärder som filter matris."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 34e702c7-f9e5-4885-9266-fc7404adecfe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2016
ms.author: jehollan
ms.openlocfilehash: 3d4be901e7e6bf1b644057648930667ab34f2124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-query-action"></a>Kom igång med hello frågeåtgärden
Med åtgärden hello frågan kan du arbeta med batchar och matriser tooaccomplish arbetsflöden för att:

* Skapa en aktivitet för alla viktiga poster från en databas.
* Spara alla bifogade PDF-filer för e-postmeddelanden till en Azure blob.

tooget igång med hello frågeåtgärden i en logikapp finns [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-hello-query-action"></a>Använd hello frågan åtgärd
En åtgärd är en åtgärd som utförs av hello arbetsflöde som har definierats i en logikapp. [Mer information om åtgärder](connectors-overview.md).  

hello frågeåtgärden har för närvarande en åtgärd som kallas hello filter matris som exponeras i hello designer. Detta gör du tooquery en matris och returnerar en uppsättning filtrerade resultat.

Här är hur du lägger till den i en logikapp:

1. Välj hello **nytt steg** knappen.
2. Välj **lägga till en åtgärd**.
3. Skriv i sökrutan för hello åtgärd **filter** toolist hello **Filter matris** åtgärd.
   
    ![Välj hello frågan åtgärd](./media/connectors-native-query/using-action-1.png)
4. Välj en matris toofilter. (hello följande skärmbild visar hello matris av resultat från en Twitter-sökning.)
5. Skapa ett villkor tooevaluate på varje objekt. (följande skärmbild hello filtrerar tweets från användare som har fler än 100 blandare.)
   
    ![Fullständig hello frågeåtgärden](./media/connectors-native-query/using-action-2.png)
   
    hello-åtgärden kommer att skrivas ut en ny matris som innehåller endast resultat som uppfyller kraven för hello filter.
6. Klicka på hello övre vänstra hörnet av hello verktygsfältet toosave och din logik app kommer båda att spara och publicera (aktivera).

## <a name="query-action"></a>Frågeåtgärden
Här följer hello information för hello-åtgärd som har stöd för den här anslutningen. hello-koppling har en möjlig åtgärd.

| Åtgärd | Beskrivning |
| --- | --- |
| Filtrera matris |Utvärderar ett villkor för varje objekt i en matris och returnerar hello resultat |

## <a name="action-details"></a>Åtgärdsinformation
hello frågeåtgärden levereras med en möjlig åtgärd. hello följande tabeller beskrivs hello nödvändiga och valfria indatafält för hello åtgärd och hello motsvarande utdata information som är associerade med åtgärden hello.

### <a name="filter-array"></a>Filtrera matris
hello följande är inmatningsfält för hello åtgärden, vilket gör en utgående HTTP-begäran.
A * innebär att det är ett obligatoriskt fält.

| Visningsnamn | Egenskapsnamn | Beskrivning |
| --- | --- | --- |
| Från * |Från |hello matris toofilter |
| Villkoret * |där |hello villkoret tooevaluate för varje objekt |

<br>

### <a name="output-details"></a>Information för utdata
hello nedan visas utdata information för hello HTTP-svar.

| Egenskapsnamn | Datatyp | Beskrivning |
| --- | --- | --- |
| Filtrerade matris |matris |En matris som innehåller ett objekt för varje filtrerade resultat |

## <a name="next-steps"></a>Nästa steg
Prova nu hello plattform och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md). Du kan utforska hello andra tillgängliga kopplingar i logikappar genom att titta på vår [API: er listan](apis-list.md).

