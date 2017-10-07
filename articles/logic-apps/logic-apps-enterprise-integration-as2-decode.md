---
title: aaaDecode AS2 - meddelanden i Azure Logic Apps | Microsoft Docs
description: "Hur toouse hello AS2 avkodarens i hello Enterprise-Integrationspaket för Logikappar i Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 2406e5ec68e0906700fad97d60cb83ef0d106cd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="decode-as2-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Avkoda AS2-meddelanden för Logikappar i Azure med hello Enterprise-Integrationspaket 

tooestablish säkerheten och pålitligheten vid överföring av meddelanden, Använd hello avkoda AS2-koppling för meddelandet. Den här anslutningen ger digital signering dekryptering och bekräftelser via meddelandet Disposition meddelanden (MDN).

## <a name="before-you-start"></a>Innan du börjar

Här är hello-objekt som du behöver:

* Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)
* En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration. Du måste ha en integration konto toouse hello avkoda AS2 meddelande-koppling.
* Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering
* En [AS2-avtal](logic-apps-enterprise-integration-as2.md) som redan har definierats i ditt konto för integrering

## <a name="decode-as2-messages"></a>Avkoda AS2-meddelanden

1. [Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

2. hello avkoda AS2-koppling för meddelandet har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran. Lägg till en utlösare hello logik App Designer, och sedan lägga till en åtgärd tooyour logikapp.

3.  Ange ”AS2” i sökrutan hello för filtret. Välj **AS2 - avkoda AS2-meddelandet**.
   
    ![Sök efter ”AS2”](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)

4. Om du tidigare inte har skapat alla anslutningar tooyour integrering konto uppmanas du toocreate anslutningen nu. Namnge din anslutning och välj hello integration konto som du vill tooconnect.
   
    ![Skapa integration anslutning](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)

    Egenskaper med en asterisk krävs.

    | Egenskap | Information |
    | --- | --- |
    | Anslutningsnamn * |Ange ett namn för anslutningen. |
    | Integration konto * |Ange ett namn för ditt konto för integrering. Se till att appen integration konto och logik finns i hello samma Azure-plats. |

5.  När du är klar din innehållet bör se ut liknande toothis exempel. toofinish skapa din anslutning och välj **skapa**.

    ![Integration anslutningsinformation](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)

6. När anslutningen har skapats, som visas i det här exemplet, Välj **brödtext** och **huvuden** från hello begära utdata.
   
    ![Integration anslutning som har skapats](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 

    Exempel:

    ![Välj brödtext och rubriker från utdata för begäran](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 

## <a name="as2-decoder-details"></a>AS2 avkodarens information

hello avkoda AS2-koppling utför dessa uppgifter: 

* Bearbetar AS2/HTTP-huvuden
* Verifierar hello signatur (om konfigurerad)
* Dekrypterar hälsningsmeddelande (om konfigurerad)
* Expanderar hello-meddelande (om konfigurerad)
* Synkroniserar en mottagna MDN med ursprungliga utgående hello-meddelande
* Uppdateringar och korrelerar poster i hello oavvislighet databas
* Skriver poster för AS2 statusrapportering
* hello utdata nyttolast innehållet är base64-kodade
* Anger om en MDN krävs, och om hello MDN ska synkron eller asynkron baserat på konfigurationen i AS2-avtal
* Genererar en synkron eller asynkron MDN (baserat på avtal konfigurationer)
* Anger hello korrelation token och egenskaper för hello MDN

## <a name="try-this-sample"></a>Försök med det här exemplet

distribuera ett fullt fungerande logik appen och exempel AS2-scenario tootry finns hello [AS2 logik appmallen och scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-hello-swagger"></a>Visa hello swagger
Se hello [swagger information](/connectors/as2/). 

## <a name="next-steps"></a>Nästa steg
[Mer information om hello Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md) 

