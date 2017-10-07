---
title: aaaEncode AS2 - meddelanden i Azure Logic Apps | Microsoft Docs
description: "Hur toouse hello AS2-kodaren i hello Enterprise-Integrationspaket för Logikappar i Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 332fb9e3-576c-4683-bd10-d177a0ebe9a3
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 2b372c416512ffa9ea5dc50ce0f767bfd8aefbc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encode-as2-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Koda AS2-meddelanden för Logikappar i Azure med hello Enterprise-Integrationspaket

tooestablish säkerheten och pålitligheten vid överföring av meddelanden, Använd hello koda AS2-koppling för meddelandet. Den här anslutningen ger digital signering, kryptering och bekräftelser via meddelandet Disposition meddelanden (MDN), vilket leder också toosupport för icke-Repudiation.

## <a name="before-you-start"></a>Innan du börjar

Här är hello-objekt som du behöver:

* Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)
* En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration. Du måste ha en integration konto toouse hello koda AS2 meddelande-koppling.
* Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering
* En [AS2-avtal](logic-apps-enterprise-integration-as2.md) som redan har definierats i ditt konto för integrering

## <a name="encode-as2-messages"></a>Koda AS2-meddelanden

1. [Skapa en logikapp](logic-apps-create-a-logic-app.md).

2. hello koda AS2-koppling för meddelandet har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran. Lägg till en utlösare hello logik App Designer, och sedan lägga till en åtgärd tooyour logikapp.

3.  Ange ”AS2” i sökrutan hello för filtret. Välj **AS2 - koda AS2-meddelandet**.
   
    ![Sök efter ”AS2”](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. Om du tidigare inte har skapat alla anslutningar tooyour integrering konto uppmanas du toocreate anslutningen nu. Namnge din anslutning och välj hello integration konto som du vill tooconnect. 
   
    ![Skapa toointegration anslutningskonto](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    Egenskaper med en asterisk krävs.

    | Egenskap | Information |
    | --- | --- |
    | Anslutningsnamn * |Ange ett namn för anslutningen. |
    | Integration konto * |Ange ett namn för ditt konto för integrering. Se till att appen integration konto och logik finns i hello samma Azure-plats. |

5.  När du är klar din innehållet bör se ut liknande toothis exempel. toofinish skapa din anslutning och välj **skapa**.
   
    ![Integration anslutningsinformation](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. När anslutningen har skapats, som visas i det här exemplet, ange information för **AS2-från**, **AS2-tooidentifiers** som konfigurerats i ditt avtal och **brödtext**, vilket är nyttolast för hello-meddelande.
   
    ![Ange obligatoriska fält](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a>AS2-kodaren information

hello koda AS2-koppling utför dessa uppgifter: 

* Gäller AS2/HTTP-huvuden
* Loggar utgående meddelanden (om konfigurerad)
* Krypterar utgående meddelanden (om konfigurerad)
* Komprimerar hello-meddelande (om konfigurerad)

## <a name="try-this-sample"></a>Försök med det här exemplet

distribuera ett fullt fungerande logik appen och exempel AS2-scenario tootry finns hello [AS2 logik appmallen och scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-hello-swagger"></a>Visa hello swagger
Se hello [swagger information](/connectors/as2/). 

## <a name="next-steps"></a>Nästa steg
[Mer information om hello Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

