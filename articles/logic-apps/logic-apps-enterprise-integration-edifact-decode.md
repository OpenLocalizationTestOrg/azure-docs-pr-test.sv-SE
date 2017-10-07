---
title: aaaDecode EDIFACT-meddelanden - Azure Logic Apps | Microsoft Docs
description: "Validera EDI och generera bekräftelser med hello EDIFACT meddelandet avkodarens i hello Enterprise-Integrationspaket för Logikappar i Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 0e61501d-21a2-4419-8c6c-88724d346e81
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 94faebdec4e4ffc8ad76ad1609495ddf9f002250
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Avkoda EDIFACT-meddelanden för Logikappar i Azure med hello Enterprise-Integrationspaket

Med hello avkoda EDIFACT-meddelande anslutningen kan du verifiera EDI och partner-specifika egenskaper, dela externa utbyten i transaktioner uppsättningar eller bevara hela externa utbyten och generera bekräftelser för bearbetade transaktioner. toouse den här kopplingen måste du lägga till hello connector tooan befintliga utlösaren i din logikapp.

## <a name="before-you-start"></a>Innan du börjar

Här är hello-objekt som du behöver:

* Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)
* En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration. Du måste ha en integration konto toouse hello avkoda EDIFACT meddelande-koppling. 
* Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering
* En [EDIFACT-avtal](logic-apps-enterprise-integration-edifact.md) som redan har definierats i ditt konto för integrering

## <a name="decode-edifact-messages"></a>Avkoda EDIFACT-meddelanden

1. [Skapa en logikapp](logic-apps-create-a-logic-app.md).

2. hello avkoda EDIFACT meddelandet connector har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran. Lägg till en utlösare hello logik App Designer, och sedan lägga till en åtgärd tooyour logikapp.

3. Ange ”EDIFACT” i sökrutan hello som filter. Välj **avkoda meddelandet EDIFACT**.
   
    ![Sök EDIFACT](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. Om du tidigare inte har skapat alla anslutningar tooyour integrering konto uppmanas du toocreate anslutningen nu. Namnge din anslutning och välj hello integration konto som du vill tooconnect.
   
    ![Skapa integration konto](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    Egenskaper med en asterisk krävs.

    | Egenskap | Information |
    | --- | --- |
    | Anslutningsnamn * |Ange ett namn för anslutningen. |
    | Integration konto * |Ange ett namn för ditt konto för integrering. Se till att appen integration konto och logik finns i hello samma Azure-plats. |

4. När du är klar att skapa anslutningen toofinish välja **skapa**. Information om anslutningen ska se liknande toothis exempel:

    ![Integration kontoinformation](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. När anslutningen har skapats, som visas i det här exemplet, Välj hello EDIFACT flat fil meddelandet toodecode.

    ![Integration konto anslutning som skapats](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    Exempel:

    ![Välj EDIFACT flat fil meddelande för avkodning av](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a>EDIFACT avkodarens information

hello avkoda EDIFACT connector utför dessa uppgifter: 

* Verifierar hello kuvert mot handel partneravtalet.
* Löser hello avtalet genom att matcha hello avsändaren kvalificerare identifierare och mottagaren kvalificerare & identifierare.
* Delar en interchange i flera transaktioner när hello interchange har fler än en transaktion baserat på hello avtal ta emot konfigurationen av prenumerationsinställningar.
* Disassemblerar hello utbyte.
* Verifierar EDI och partner-specifika egenskaper, inklusive:
  * Validering av hello interchange kuvert struktur
  * Schemavalideringen hello Envelope mot hello kontrollen schema
  * Schemavalideringen av hello transaktion set dataelement mot hello Meddelandeschema
  * EDI-verifiering utförs på transaktion set dataelement
* Verifierar att hello interchange, grupp och transaktionen set kontrollen siffror inte är dubbletter (om konfigurerad) 
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
* Genererar en Technical (kontroll) och/eller funktionella bekräftelse (om konfigurerad).
  * En teknisk bekräftelse eller hello CONTRL ACK rapporterar hello resultaten av en syntaktiska kontroll av hello fullständig emot utbyte.
  * En funktionell bekräftelse om godkänna eller avvisa en mottagna utbyte eller en grupp

## <a name="view-swagger-file"></a>Visa Swagger-fil
tooview hello Swagger information för hello EDIFACT-anslutningen finns [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Nästa steg
[Mer information om hello Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

