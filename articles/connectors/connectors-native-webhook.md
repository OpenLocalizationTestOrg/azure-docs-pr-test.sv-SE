---
title: "aaaWebhook connector för Logikappar i Azure | Microsoft Docs"
description: "Hur toouse webhook-åtgärder och utlösare tooperform åtgärder som Filter matris från logikappar"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: b2dee12750f3f20f10e7b257da05a79f28f90f43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-webhook-connector"></a>Kom igång med hello webhook-koppling

Med hello webhook åtgärd och utlösare kan du starta, pausa och återuppta flöden tooperform dessa uppgifter:

* Starta från en [Azure Event Hub](https://github.com/logicappsio/EventHubAPI) när ett objekt tas emot
* Vänta på ett godkännande innan du fortsätter ett arbetsflöde

Lär dig mer om [hur toocreate anpassade API: er som stöder en webhook](../logic-apps/logic-apps-create-api-app.md).

## <a name="use-hello-webhook-trigger"></a>Använda hello webhook utlösare

En [ *utlösaren* ](connectors-overview.md) är en händelse som startar ett arbetsflöde för logik-app. En webhook-utlösare är händelsebaserat och förlitar sig inte på avsökning för nya objekt. Som hello [begäran utlösaren](connectors-native-reqres.md), hello logikapp utlöses hello snabbmeddelanden att en händelse inträffar. Hej webhook utlösaren registrerar en *motringning URL* tooa tjänsten och använder den URL toofire hello logikappen som behövs.

Här är ett exempel som visar hur tooset in HTTP utlösare i hello logik App Designer. hello anvisningarna förutsätter att du redan har distribuerat eller använder ett API som följer hello [webhook prenumerera och avbryta prenumerationen mönster i logikappar](../logic-apps/logic-apps-create-api-app.md#webhook-triggers). hello prenumerera anrop görs när en logikapp sparas med en ny webhook eller växlade från inaktiverade tooenabled. hello avbryta prenumerationen anrop görs när en webhook apputlösare logik tas bort och spara eller växlade från aktiverade toodisabled.

**tooadd hello webhook utlösare**

1. Lägg till hello **HTTP Webhook** utlösare som hello första steg i en logikapp.
2. Fyll i hello parametrar för hello webhook prenumerera och avbryta prenumerationen anrop.

   Det här steget följer samma mönster som hello hello [HTTP-åtgärd](connectors-native-http.md) format.

     ![HTTP-utlösare](./media/connectors-native-webhook/using-trigger.png)

3. Lägg till minst en åtgärd.
4. Klicka på **spara** toopublish hello logikapp. Det här steget anrop hello prenumerera slutpunkten med hello återanrop URL krävs tootrigger denna logikapp.
5. När hello tjänsten gör en `HTTP POST` toohello motringning URL hello logik app utlöses och innehåller alla data som skickas till hello-begäran.

## <a name="use-hello-webhook-action"></a>Använd hello webhook åtgärd

En [ *åtgärd* ](connectors-overview.md) är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp. En webhook-åtgärden registrerar en *motringning URL* med en tjänst och väntar tills hello URL anropas innan du fortsätter. Hej [”Skicka godkännande e-post”](connectors-create-api-office365-outlook.md) är ett exempel på en koppling som följer detta mönster. Du kan utöka det här mönstret till alla tjänster genom hello webhook åtgärd. 

Här är ett exempel som visar hur tooset en webhook-åtgärder i hello logik App Designer. Dessa instruktioner förutsätter att du redan har distribuerat eller använder ett API som följer hello [webhook prenumerera och avbryta prenumerationen mönster som används i logikappar](../logic-apps/logic-apps-create-api-app.md#webhook-actions). hello prenumerera anrop görs när en logikapp utför hello webhook åtgärd. hello avbryta prenumerationen anrop görs när en körning avbryts under väntan på ett svar, innan hello logik app.

**tooadd en webhook-åtgärd**

1. Välj **nytt steg** > **lägga till en åtgärd**.

2. Skriv i sökrutan hello ”webhook” toofind hello **HTTP Webhook** åtgärd.

    ![Åtgärden Select-frågan](./media/connectors-native-webhook/using-action-1.png)

3. Fyll i hello parametrar för hello webhook prenumerera och avbryta prenumerationen anrop

   Det här steget följer samma mönster som hello hello [HTTP-åtgärd](connectors-native-http.md) format.

     ![Fullständig frågeåtgärden](./media/connectors-native-webhook/using-action-2.png)
   
   Vid körning prenumerera hello logik app anrop hello slutpunkten efter att ha uppnått steget.

4. Klicka på **spara** toopublish hello logikapp.

## <a name="technical-details"></a>Teknisk information

Här finns mer information om hello utlösare och åtgärder som webhook har stöd för.

## <a name="webhook-triggers"></a>Webhook-utlösare

| Åtgärd | Beskrivning |
| --- | --- |
| HTTP-Webhook |Prenumerera på en motringning URL tooa tjänst som kan anropa hello URL toofire logikapp efter behov. |

### <a name="trigger-details"></a>Information om utlösaren

#### <a name="http-webhook"></a>HTTP-Webhook

Prenumerera på en motringning URL tooa tjänst som kan anropa hello URL toofire logikapp efter behov.
Ett * innebär obligatoriskt fält.

| Visningsnamn | Egenskapsnamn | Beskrivning |
| --- | --- | --- |
| Prenumerera metoden * |Metoden |HTTP-metoden toouse för prenumerera begäran |
| Prenumerera URI * |URI: N |HTTP URI toouse för prenumerera begäran |
| Avbryta prenumerationen metoden * |Metoden |HTTP-metoden toouse för unsubscribe begäran |
| Avbryta prenumerationen URI * |URI: N |HTTP URI toouse för unsubscribe begäran |
| Prenumerera brödtext |Brödtext |Text för HTTP-begäran för prenumerera |
| Prenumerera rubriker |Rubriker |HTTP-huvuden för begäran för prenumerera |
| Prenumerera autentisering |Autentisering |HTTP-autentisering toouse för prenumerera. [HTTP-anslutningen finns](connectors-native-http.md#authentication) information |
| Avbryta prenumerationen brödtext |Brödtext |Text för HTTP-begäran för unsubscribe |
| Avbryta prenumerationen rubriker |Rubriker |HTTP-huvuden för begäran för unsubscribe |
| Avbryta prenumerationen autentisering |Autentisering |HTTP-autentisering toouse för unsubscribe. [HTTP-anslutningen finns](connectors-native-http.md#authentication) information |

**Information för utdata**

Webhook-begäran

| Egenskapsnamn | Datatyp | Beskrivning |
| --- | --- | --- |
| Rubriker |Objektet |Webhook-huvuden för begäran |
| Innehåll |Objektet |Webhook request-objektet |
| Statuskod |int |Webhook statuskod för begäran |

## <a name="webhook-actions"></a>Webhook-åtgärder

| Åtgärd | Beskrivning |
| --- | --- |
| HTTP-Webhook |Prenumerera på en motringning URL tooa tjänst som kan anropa hello URL tooresume ett arbetsflödessteg efter behov. |

### <a name="action-details"></a>Åtgärdsinformation

#### <a name="http-webhook"></a>HTTP-Webhook

Prenumerera på en motringning URL tooa tjänst som kan anropa hello URL tooresume ett arbetsflödessteg efter behov.
Ett * innebär obligatoriskt fält.

| Visningsnamn | Egenskapsnamn | Beskrivning |
| --- | --- | --- |
| Prenumerera metoden * |Metoden |HTTP-metoden toouse för prenumerera begäran |
| Prenumerera URI * |URI: N |HTTP URI toouse för prenumerera begäran |
| Avbryta prenumerationen metoden * |Metoden |HTTP-metoden toouse för unsubscribe begäran |
| Avbryta prenumerationen URI * |URI: N |HTTP URI toouse för unsubscribe begäran |
| Prenumerera brödtext |Brödtext |Text för HTTP-begäran för prenumerera |
| Prenumerera rubriker |Rubriker |HTTP-huvuden för begäran för prenumerera |
| Prenumerera autentisering |Autentisering |HTTP-autentisering toouse för prenumerera. [HTTP-anslutningen finns](connectors-native-http.md#authentication) information |
| Avbryta prenumerationen brödtext |Brödtext |Text för HTTP-begäran för unsubscribe |
| Avbryta prenumerationen rubriker |Rubriker |HTTP-huvuden för begäran för unsubscribe |
| Avbryta prenumerationen autentisering |Autentisering |HTTP-autentisering toouse för unsubscribe. [HTTP-anslutningen finns](connectors-native-http.md#authentication) information |

**Information för utdata**

Webhook-begäran

| Egenskapsnamn | Datatyp | Beskrivning |
| --- | --- | --- |
| Rubriker |Objektet |Webhook-huvuden för begäran |
| Innehåll |Objektet |Webhook request-objektet |
| Statuskod |int |Webhook statuskod för begäran |

## <a name="next-steps"></a>Nästa steg

* [Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md)
* [Sök efter andra kopplingar](apis-list.md)