---
title: "åtgärder för aaaUse förfrågan och svar | Microsoft Docs"
description: "Översikt över hello förfrågan och svar trigger och action i en Azure logikapp"
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 566924a4-0988-4d86-9ecd-ad22507858c0
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 24c378cc12d5f3f65116d5e59278236186a99662
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-request-and-response-components"></a>Kom igång med hello komponenter för förfrågan och svar
Med hello förfrågan och svar komponenter i en logikapp, kan du svara i realtid tooevents.

Du kan till exempel:

* Svara tooan HTTP-begäran med data från en lokal databas via en logikapp.
* Utlös en logikapp från en extern webhook-händelse.
* Anropa en logikapp med en åtgärd för förfrågan och svar från inom en annan logikapp.

tooget igång med hello förfrågan och svar åtgärder i en logikapp finns [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-hello-http-request-trigger"></a>Använd hello utlösare för HTTP-begäran
En utlösare är en händelse som kan använda toostart hello arbetsflöde som har definierats i en logikapp. [Mer information om utlösare](connectors-overview.md).

Här är en Exempelsekvens av hur tooset in HTTP begäran i hello logik App Designer.

1. Lägga till hello utlösare **begäran - när en HTTP-begäran tas emot** i din logikapp. Du kan också medföra en JSON-schema (med hjälp av ett verktyg som [JSONSchema.net](http://jsonschema.net)) för hello frågans brödtext. På så sätt kan hello designer toogenerate token för egenskaper i hello HTTP-begäran.
2. Lägg till en annan åtgärd så att du kan spara hello logikapp.
3. Du kan hämta hello HTTP URL-begäran från hello begäran kort när du har sparat hello logikapp.
4. En HTTP POST (du kan använda ett verktyg som [Postman](https://www.getpostman.com/)) toohello URL utlösare hello logikapp.

> [!NOTE]
> Om du inte definierar en åtgärd för svar, en `202 ACCEPTED` svar returneras omedelbart toohello anroparen. Du kan använda hello svar åtgärden toocustomize ett svar.
> 
> 

![Svaret utlösare](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-hello-http-response-action"></a>Använd hello åtgärd för HTTP-svar
hello HTTP-svar som åtgärden är endast giltig när du använder den i ett arbetsflöde som utlöses av en HTTP-begäran. Om du inte definierar en åtgärd för svar, en `202 ACCEPTED` svar returneras omedelbart toohello anroparen.  Du kan lägga till en åtgärd som svar på något steg i hello arbetsflöde. Hej logikapp endast håller hello inkommande begäran öppen för en minut för svar.  Efter en minut, om inget svar skickades från hello arbetsflöde (och det finns ett svar-åtgärd i hello definition), en `504 GATEWAY TIMEOUT` returneras toohello anroparen.

Här är hur tooadd åtgärden HTTP-svar:

1. Välj hello **nytt steg** knappen.
2. Välj **lägga till en åtgärd**.
3. Skriv i sökrutan för hello åtgärd **svar** toolist hello svar åtgärd.
   
    ![Välj hello svar åtgärd](./media/connectors-native-reqres/using-action-1.png)
4. Lägga till i alla parametrar som krävs för hello HTTP-svarsmeddelandet.
   
    ![Fullständig hello svar åtgärd](./media/connectors-native-reqres/using-action-2.png)
5. Klicka på hello övre vänstra hörnet av hello verktygsfältet toosave och din logik app kommer båda att spara och publicera (aktivera).

## <a name="request-trigger"></a>Begäran utlösare
Här följer hello information för hello utlösare som har stöd för den här anslutningen. Det finns en enskild begäran-utlösare.

| Utlösare | Beskrivning |
| --- | --- |
| Förfrågan |Inträffar när en HTTP-begäran tas emot |

## <a name="response-action"></a>Svaret åtgärd
Här följer hello information för hello-åtgärd som har stöd för den här anslutningen. Det finns ett enda svar-åtgärd som kan endast användas när den åtföljs av en begäran-utlösare.

| Åtgärd | Beskrivning |
| --- | --- |
| Svar |Returnerar ett svar toohello korrelerade HTTP-begäran |

### <a name="trigger-and-action-details"></a>Trigger och action information
hello följande tabeller beskriver hello inmatningsfält för hello trigger och action och hello detaljer om motsvarande utdata.

#### <a name="request-trigger"></a>Begäran utlösare
hello följande är ett inmatningsfält för hello utlösare från en inkommande HTTP-begäran.

| Visningsnamn | Egenskapsnamn | Beskrivning |
| --- | --- | --- |
| JSON-Schema |Schemat |hello JSON-schema av text för hello HTTP-begäran |

<br>

**Information för utdata**

hello nedan visas utdata information för hello-begäran.

| Egenskapsnamn | Datatyp | Beskrivning |
| --- | --- | --- |
| Rubriker |Objektet |Huvuden för begäran |
| Innehåll |Objektet |Request-objektet |

#### <a name="response-action"></a>Svaret åtgärd
hello följande är inmatningsfält för hello åtgärd för HTTP-svar. A * innebär att det är ett obligatoriskt fält.

| Visningsnamn | Egenskapsnamn | Beskrivning |
| --- | --- | --- |
| Status kod * |statusCode |hello HTTP-statuskod |
| Rubriker |Rubriker |Alla svar huvuden tooinclude ett JSON-objekt |
| Innehåll |Brödtext |hello svarstexten |

## <a name="next-steps"></a>Nästa steg
Prova nu hello plattform och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md). Du kan utforska hello andra tillgängliga kopplingar i logikappar genom att titta på vår [API: er listan](apis-list.md).

