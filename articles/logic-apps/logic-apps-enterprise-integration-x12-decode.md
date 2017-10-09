---
title: aaaDecode X12 meddelanden - Azure Logic Apps | Microsoft Docs
description: "Validera EDI och generera bekräftelser med hello X12 meddelande avkodarens i hello Enterprise-Integrationspaket för Logikappar i Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4fd48d2d-2008-4080-b6a1-8ae183b48131
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 1ffececca1ff835b319b64c85f86c421395833c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="decode-x12-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Avkoda X12 meddelanden för Logikappar i Azure med hello Enterprise-Integrationspaket

Med hello avkoda X12 meddelande-anslutningen kan kan du verifiera hello kuvert mot ett handelspartneravtal, verifiera EDI och partner-specifika egenskaper, dela externa utbyten i transaktioner uppsättningar eller bevara hela externa utbyten och generera bekräftelser för bearbetade transaktioner. toouse den här kopplingen måste du lägga till hello connector tooan befintliga utlösaren i din logikapp.

## <a name="before-you-start"></a>Innan du börjar

Här är hello-objekt som du behöver:

* Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)
* En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration. Du måste ha en integration konto toouse hello avkoda X12 meddelande-koppling.
* Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering
* En [X12 avtal](logic-apps-enterprise-integration-x12.md) som redan har definierats i ditt konto för integrering

## <a name="decode-x12-messages"></a>Avkoda X12 meddelanden

1. [Skapa en logikapp](logic-apps-create-a-logic-app.md).

2. hello avkoda X12 meddelande connector har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran. Lägg till en utlösare hello logik App Designer, och sedan lägga till en åtgärd tooyour logikapp.

3.  Ange ”x12” i sökrutan hello för filtret. Välj **X12-avkoda X12 meddelandet**.
   
    ![Sök efter ”x12”](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. Om du tidigare inte har skapat alla anslutningar tooyour integrering konto uppmanas du toocreate anslutningen nu. Namnge din anslutning och välj hello integration konto som du vill tooconnect. 

    ![Ange integration kontoinformation för anslutning](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    Egenskaper med en asterisk krävs.

    | Egenskap | Information |
    | --- | --- |
    | Anslutningsnamn * |Ange ett namn för anslutningen. |
    | Integration konto * |Ange ett namn för ditt konto för integrering. Se till att appen integration konto och logik finns i hello samma Azure-plats. |

5.  När du är klar din innehållet bör se ut liknande toothis exempel. toofinish skapa din anslutning och välj **skapa**.
   
    ![Integration kontoinformation för anslutning](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. När anslutningen har skapats, som visas i det här exemplet, Välj hello X12 flat fil meddelandet toodecode.

    ![Integration konto anslutning som skapats](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    Exempel:

    ![Välj X12 flat fil meddelande för avkodning av](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

## <a name="x12-decode-details"></a>X12 avkoda information

Hej X12 avkoda connector utför dessa uppgifter:

* Verifierar hello kuvert mot handel partneravtalet
* Verifierar EDI och partner-specifika egenskaper
  * Strukturella EDI-validering och utökade schemavalidering
  * Validering av hello interchange kuvert hello struktur.
  * Schemavalideringen hello Envelope mot hello kontrollen schema.
  * Schemavalideringen av hello transaktion set dataelement mot hello Meddelandeschema.
  * EDI-verifiering utförs på transaktion set dataelement 
* Verifierar att hello interchange, grupp och transaktionen set kontrollen siffror inte är dubbletter
  * Kontrollerar hello interchange kontrollnummer mot tidigare mottagna externa utbyten.
  * Kontrollerar hello kontrollen gruppnumret mot andra grupp kontrollen talen i hello utbyte.
  * Kontrollerar hello transaktion ange kontroll många mot andra transaktion set kontrollen talen i den gruppen.
* Delar upp hello interchange i transaktionen uppsättningar eller bevarar hello hela interchange:
  * Dela Interchange som transaktionen uppsättningar - inaktivera transaktion anger vid fel: delningar interchange i transaktion anger och Parsar varje transaktion uppsättning. 
  Hej X12 avkoda åtgärd matar ut bara de transaktion filer som inte kan valideras för`badMessages`, och anger hello återstående transaktioner anger för`goodMessages`.
  * Dela Interchange som transaktionen uppsättningar - inaktivera utbyte vid fel: delningar interchange i transaktion anger och Parsar varje transaktion uppsättning. 
  Om en eller flera transaktion anger i hello interchange misslyckas verifieringen, hello X12 avkoda åtgärd matar ut alla hello transaktion anger i den interchange för`badMessages`.
  * Bevara Interchange – inaktivera transaktion anger vid fel: Preserve hello utbyte och processen hello hela gruppbaserad utbyte. 
  Hej X12 avkoda åtgärd matar ut bara de transaktion filer som inte kan valideras för`badMessages`, och anger hello återstående transaktioner anger för`goodMessages`.
  * Bevara Interchange – inaktivera utbyte vid fel: Preserve hello utbyte och processen hello hela gruppbaserad utbyte. 
  Om en eller flera transaktion anger i hello interchange misslyckas verifieringen, hello X12 avkoda åtgärd matar ut alla hello transaktion anger i den interchange för`badMessages`. 
* Genererar en tekniska och/eller funktionella bekräftelse (om konfigurerad).
  * En teknisk bekräftelse genererar på grund av huvud-verifiering. Hej tekniska bekräftelse rapporter hello status med hello bearbetning av ett utbyte huvud och inkluderande av hello adress mottagare.
  * En funktionell bekräftelse genererar på grund av brödtext validering. hello rapporterar funktionella bekräftelse varje fel uppstod under bearbetning av hello emot dokumentet

## <a name="view-hello-swagger"></a>Visa hello swagger
Se hello [swagger information](/connectors/x12/). 

## <a name="next-steps"></a>Nästa steg
[Mer information om hello Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

