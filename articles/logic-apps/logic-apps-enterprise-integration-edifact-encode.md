---
title: aaaEncode EDIFACT-meddelanden - Azure Logic Apps | Microsoft Docs
description: "Validera EDI och generera XML med EDIFACT meddelandekodare i hello Enterprise-Integrationspaket för Logikappar i Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 974ac339-d97a-4715-bc92-62d02281e900
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: b3799dbd2492adef597022d017cf28f5ceb1085c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Koda EDIFACT-meddelanden för Logikappar i Azure med hello Enterprise-Integrationspaket

Med hello koda EDIFACT-meddelande anslutningen kan du verifiera EDI och partner-specifika egenskaper, generera ett XML-dokument för varje transaktion och begär en teknisk bekräftelse eller den funktionella bekräftelse.
toouse den här kopplingen måste du lägga till hello connector tooan befintliga utlösaren i din logikapp.

## <a name="before-you-start"></a>Innan du börjar

Här är hello-objekt som du behöver:

* Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)
* En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration. Du måste ha en integration konto toouse hello koda EDIFACT meddelande-koppling. 
* Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering
* En [EDIFACT-avtal](logic-apps-enterprise-integration-edifact.md) som redan har definierats i ditt konto för integrering

## <a name="encode-edifact-messages"></a>Koda EDIFACT-meddelanden

1. [Skapa en logikapp](logic-apps-create-a-logic-app.md).

2. hello koda EDIFACT meddelandet connector har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran. Lägg till en utlösare hello logik App Designer, och sedan lägga till en åtgärd tooyour logikapp.

3.  Ange ”EDIFACT” i sökrutan hello som filter. Välj antingen **koda EDIFACT-meddelandet genom avtalsnamn** eller **koda tooEDIFACT meddelandet genom identiteter**.
   
    ![Sök EDIFACT](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. Om du tidigare inte har skapat alla anslutningar tooyour integrering konto uppmanas du toocreate anslutningen nu. Namnge din anslutning och välj hello integration konto som du vill tooconnect.

    ![Skapa integration konto anslutning](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    Egenskaper med en asterisk krävs.

    | Egenskap | Information |
    | --- | --- |
    | Anslutningsnamn * |Ange ett namn för anslutningen. |
    | Integration konto * |Ange ett namn för ditt konto för integrering. Se till att appen integration konto och logik finns i hello samma Azure-plats. |

5.  När du är klar din innehållet bör se ut liknande toothis exempel. toofinish skapa din anslutning och välj **skapa**.

    ![Integration kontoinformation för anslutning](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    Anslutningen har skapats.

    ![Integration konto anslutning som skapats](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a>Koda EDIFACT-meddelande med avtalsnamn

Om du väljer tooencode EDIFACT meddelanden avtalsnamn, öppna hello **namn EDIFACT-avtal** listan, ange eller välj namnet på din EDIFACT-avtal. Ange hello XML-meddelande tooencode.

![Ange EDIFACT avtalets namn och XML-meddelandet tooencode](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a>Koda EDIFACT-meddelande med identiteter

Om du väljer tooencode EDIFACT meddelanden identiteter, ange hello ingen avsändaridentifierare, avsändaren kvalificerare, mottagaren identifierare och mottagaren kvalificerare som konfigurerats i EDIFACT-avtal. Välj hello XML-meddelande tooencode.

![Ange identiteter för avsändare och mottagare, Välj XML-meddelandet tooencode](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a>EDIFACT koda information

hello koda EDIFACT connector utför dessa uppgifter: 

* Lös hello avtalet genom att matcha hello avsändaren kvalificerare & identifierare och mottagaren kvalificerare och identifierare
* Serialiserar hello EDI interchange, konvertera XML-kodade meddelanden till EDI transaktion uppsättningar i hello utbyte.
* Gäller transaktion set-huvudet och trailern segment
* Genererar ett utbyte kontrollen Antal, gruppnumret kontroll och en uppsättning kontrollen transaktionsnumret för varje utgående utbyte
* Ersätter avgränsare i hello nyttolasten
* Verifierar EDI och partner-specifika egenskaper
  * Schemavalideringen av hello transaktion set dataelement mot hello Meddelandeschema.
  * EDI-verifiering utförs på dataelement för transaktionen uppsättningen.
  * Utökad verifiering utförs på transaktion set dataelement
* Genererar XML-dokument för varje transaktion.
* Begär en tekniska och/eller funktionella bekräftelse (om konfigurerad).
  * Som en teknisk bekräftelse anger hälsningsmeddelande CONTRL mottagit ett utbyte.
  * Som en funktionell bekräftelse anger CONTRL hälsningsmeddelande godkännande eller underkännande hello emot interchange, grupp eller meddelandet med en lista över fel eller funktioner som inte stöds

## <a name="view-swagger-file"></a>Visa Swagger-fil
tooview hello Swagger information för hello EDIFACT-anslutningen finns [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Nästa steg
[Mer information om hello Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

