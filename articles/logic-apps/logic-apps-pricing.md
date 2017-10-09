---
title: aaaPricing & fakturering - Azure Logic Apps | Microsoft Docs
description: "Lär dig hur priser och fakturering fungerar för Logikappar i Azure."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.openlocfilehash: fa10cbbf7657afba7fadb7c76817b7a5e4af7b42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-pricing-model"></a>Prissättningsmodell för Logic Apps
Med Azure Logikappar kan du tooscale och köra integration arbetsflöden i hello molnet.  Nedan visas information om hello fakturering och priser planer för Logic Apps.
## <a name="consumption-pricing"></a>Priser för förbrukning
Logic Apps använder nyskapad en plan för användning. Med hello Logic Apps förbrukning prismodellen betalar du bara för det du använder.  Logic Apps är inte begränsas när du använder en plan för användning.
Alla åtgärder som utförs i en körning av en logik app-instansen är avgiftsbelagda.
### <a name="what-are-action-executions"></a>Vad är åtgärden körningar?
Varje steg i en definition av logik app är en åtgärd som innehåller utlösare, kontroll flödet steg som villkor, scope för varje slingor gör tills slingor, anropar tooconnectors och anropar toonative åtgärder.
Utlösare är särskilda åtgärder som är utformad tooinstantiate en ny instans av logikappen när en viss händelse inträffar.  Det finns flera olika beteenden för utlösare som kan påverka hur hello logikapp mäts.
* **Avsökningen utlösaren** – den här utlösaren kontinuerligt avsöker en slutpunkt tills den får ett meddelande som uppfyller hello kriterier för att skapa en instans av logikappen.  hello avsökningsintervallet kan konfigureras i hello utlösaren i hello logik App Designer.  Varje avsökning begäran, räknas även om det inte att skapa en instans av en logikapp som en körning av åtgärden.
* **Webhook-utlösare** – den här utlösaren väntar på att en klient toosend den en begäran om på en viss slutpunkt.  Varje begäran skickas toohello webhook endpoint räknas som en körning av åtgärden. hello begäran och hello HTTP Webhook-utlösare är båda webhook-utlösare.
* **Återkommande utlösaren** – den här utlösaren skapas en instans av hello logikapp baserat på hello Upprepningsintervall som konfigurerats i hello utlösaren.  En upprepning utlösare kan till exempel vara konfigurerade toorun alla tre dagar eller även varje minut.

Utlösaren körningar kan ses i resursbladet för hello Logic Apps i hello utlösaren historik del.

Alla åtgärder som utfördes, om de hade lyckades eller misslyckades är avgiftsbelagda som en körning av åtgärden.  Åtgärder som hoppades över på grund av tooa villkor inte uppfylls eller åtgärder som inte köras eftersom hello logikapp avslutas innan räknas inte som åtgärden körningar.

Åtgärder som utförs i slingor räknas per iteration av hello loop.  Till exempel en enda åtgärd i ett för varje loop söka igenom en lista med 10 objekt räknas som hello antal objekt i listan hello (10) multiplicerat med hello antal åtgärder i hello loop (1) plus en för hello inleda hello loop , som i det här exemplet skulle vara (10 * 1) + 1 = 11 åtgärd körningar.
Inaktiverad Logic Apps kan inte ha nya instanser instansieras och därför vid inaktiveras debiteras inte.  Tänk på att det kan ta lite tid för hello instanser tooquiesce innan helt inaktiverad när du har inaktiverat en logikapp.
### <a name="integration-account-usage"></a>Användningen av integration konton
Ingår i förbrukningsbaserad användning är en [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) för undersökning, utveckling och testning så att du toouse hello [B2B/EDI](logic-apps-enterprise-integration-b2b.md) och [XML-bearbetning](logic-apps-enterprise-integration-xml.md)funktioner i Logic Apps utan extra kostnad. Du kan toocreate högst ett konto per region och lagra too10 avtal och 25 maps. Scheman, certifikat och partners har inga begränsningar och du kan ladda upp så många som du behöver.

Dessutom toohello inkludering av integrationskonton med förbrukning, du kan också skapa standard integrationskonton med våra standard Logic Apps SLA och utan begränsningar. Mer information finns i [priser för Azure](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="app-service-plans"></a>App Service-planer
Logikappar som tidigare har skapat refererar till en App Service-Plan fortsätter toobehave som innan. Beroende på hello planen valt har begränsats efter hello föreskrivna dagliga körningar överskrids men debiteras med hello åtgärd körning mätaren.
EA-kunder som har en Apptjänstplan i sin prenumeration som inte har uttryckligen associerat med hello Logikapp toobe kan få hello ingår kvantiteter förmånen.  Om du har en Standard App Service-Plan i prenumerationen EA och en Logikapp i hello samma prenumeration och du debiteras inte för 10 000 åtgärder körningar per dag (se följande tabell). 

App Service-planer och deras dagliga körningar tillåten åtgärd:
|  | Kostnadsfri/delad/enkel | Standard | Premium |
| --- | --- | --- | --- |
| Åtgärden körningar per dag |200 |10 000 |50,000 |
### <a name="convert-from-app-service-plan-pricing-tooconsumption"></a>Konvertera från Apptjänstplan priser tooConsumption
toochange en Logikapp som har en App Service-Plan som är associerade med den tooa förbrukning modellen, ta bort hello referens toohello App Service-Plan i hello logkappsdefinitionen.  Den här ändringen kan du göra med en anropet tooa PowerShell-cmdlet:`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`
## <a name="pricing"></a>Prissättning
Information om priser, se [Logic Apps priser](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Nästa steg
* [En översikt över Logic Apps][whatis]
* [Skapa din första logikapp][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-what-are-logic-apps.md
[create]: logic-apps-create-a-logic-app.md

