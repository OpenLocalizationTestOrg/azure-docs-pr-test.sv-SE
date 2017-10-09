---
title: aaaEncode X12 meddelanden - Azure Logic Apps | Microsoft Docs
description: "Validera EDI och konvertera XML-kodade meddelanden med X12 meddelande kodare i hello Enterprise-Integrationspaket för Logikappar i Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: a01e9ca9-816b-479e-ab11-4a984f10f62d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 785dbd2c7c82463154732921e07e3586d2307840
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encode-x12-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Koda X12 meddelanden för Logikappar i Azure med hello Enterprise-Integrationspaket

Med hello koda X12 meddelande-anslutningen kan du verifiera EDI och partner-specifika egenskaper, konvertera XML-kodade meddelanden till EDI transaktion uppsättningar i hello utbyte och begär en teknisk bekräftelse eller den funktionella bekräftelse.
toouse den här kopplingen måste du lägga till hello connector tooan befintliga utlösaren i din logikapp.

## <a name="before-you-start"></a>Innan du börjar

Här är hello-objekt som du behöver:

* Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)
* En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration. Du måste ha en integration konto toouse hello koda X12 meddelande-koppling.
* Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering
* En [X12 avtal](logic-apps-enterprise-integration-x12.md) som redan har definierats i ditt konto för integrering

## <a name="encode-x12-messages"></a>Koda X12 meddelanden

1. [Skapa en logikapp](logic-apps-create-a-logic-app.md).

2. hello koda X12 meddelande connector har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran. Lägg till en utlösare hello logik App Designer, och sedan lägga till en åtgärd tooyour logikapp.

3.  Ange ”x12” i sökrutan hello för filtret. Välj antingen **X12-koda tooX12 meddelande av avtalsnamn** eller **X12-koda tooX12 meddelande av identiteter**.
   
    ![Sök efter ”x12”](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. Om du tidigare inte har skapat alla anslutningar tooyour integrering konto uppmanas du toocreate anslutningen nu. Namnge din anslutning och välj hello integration konto som du vill tooconnect. 
   
    ![Integration konto anslutning](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    Egenskaper med en asterisk krävs.

    | Egenskap | Information |
    | --- | --- |
    | Anslutningsnamn * |Ange ett namn för anslutningen. |
    | Integration konto * |Ange ett namn för ditt konto för integrering. Se till att appen integration konto och logik finns i hello samma Azure-plats. |

5.  När du är klar din innehållet bör se ut liknande toothis exempel. toofinish skapa din anslutning och välj **skapa**.

    ![Integration konto anslutning som skapats](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    Anslutningen har skapats.

    ![Integration kontoinformation för anslutning](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a>Koda X12 meddelanden efter avtalsnamn

Om du har valt tooencode X12 meddelanden av avtalsnamn öppna hello **namnet på X12 avtal** listan, ange eller välj en befintlig X12 avtal. Ange hello XML-meddelande tooencode.

![Ange X12 avtal namn och en XML-meddelandet tooencode](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a>Koda X12 meddelanden med identiteter

Om du väljer tooencode X12 meddelanden av identiteter ange hello ingen avsändaridentifierare, kvalificerare avsändaren, mottagaren identifierare och mottagaren kvalificerare som konfigurerats i din X12 avtal. Välj hello XML-meddelande tooencode.
   
![Ange identiteter för avsändare och mottagare, Välj XML-meddelandet tooencode](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a>X12 koda information

Hej X12 koda connector utför dessa uppgifter:

* Avtalet lösning genom att matcha avsändare och mottagare kontextegenskaper.
* Serialiserar hello EDI interchange, konvertera XML-kodade meddelanden till EDI transaktion uppsättningar i hello utbyte.
* Gäller transaktion set-huvudet och trailern segment
* Genererar ett utbyte kontrollen Antal, gruppnumret kontroll och en uppsättning kontrollen transaktionsnumret för varje utgående utbyte
* Ersätter avgränsare i hello nyttolasten
* Verifierar EDI och partner-specifika egenskaper
  * Schemavalideringen hello transaktion set dataelement mot hello-meddelande schemat för
  * EDI-verifiering utförs på dataelement för transaktionen uppsättningen.
  * Utökad verifiering utförs på transaktion set dataelement
* Begär en tekniska och/eller funktionella bekräftelse (om konfigurerad).
  * En teknisk bekräftelse genererar på grund av huvud-verifiering. hello tekniska bekräftelse rapporter hello status med hello bearbetning av ett utbyte huvudet och trailern av hello adress mottagare
  * En funktionell bekräftelse genererar på grund av brödtext validering. hello rapporterar funktionella bekräftelse varje fel uppstod under bearbetning av hello emot dokumentet

## <a name="view-hello-swagger"></a>Visa hello swagger
Se hello [swagger information](/connectors/x12/). 

## <a name="next-steps"></a>Nästa steg
[Mer information om hello Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

