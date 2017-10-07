---
title: aaaUse hello Slack-anslutningen i dina Azure logic apps | Microsoft Docs
description: Ansluta tooSlack i dina logic apps
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 6599d7b69d2147425c9fab978c5d0f93e5605f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-slack-connector"></a>Kom igång med hello Slack-koppling
Slack är ett team kommunikationsverktyg som sammanför alla meddelanden i ett team placera, direkt sökbara och tillgänglig överallt. 

Kom igång genom att skapa en logikapp nu. Se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-tooslack"></a>Skapa en anslutning tooSlack
toouse hello Slack anslutningen kan du först skapa en **anslutning** ange hello information för dessa egenskaper: 

| Egenskap | Krävs | Beskrivning |
| --- | --- | --- |
| Token |Ja |Ange Slack autentiseringsuppgifter |

Följ dessa steg toosign till Slack och fullständig hello konfiguration av hello Slack **anslutning** i din logikapp:

1. Välj **upprepning**
2. Välj en **frekvens** och ange en **intervall**
3. Välj **lägga till en åtgärd**  
   ![Konfigurera Slack][1]  
4. Ange Slack i sökrutan hello och vänta tills hello Sök tooreturn alla poster med Slack i hello namn
5. Välj **Slack - skicka meddelandet**
6. Välj **logga in tooSlack**:  
   ![Konfigurera Slack][2]
7. Ange dina autentiseringsuppgifter för Slack toosign i tooauthorize hello program    
   ![Konfigurera Slack][3]  
8. Du kommer att omdirigerade tooyour organisation inloggningssidan. **Auktorisera** Slack toointeract med din logikapp:      
   ![Konfigurera Slack][5] 
9. När hello auktorisering har slutförts kommer du att omdirigerade tooyour logik app toocomplete den genom att konfigurera hello **Slack - hämta alla meddelanden** avsnitt. Lägg till andra utlösare och åtgärder som du behöver.  
   ![Konfigurera Slack][6]
10. Spara ditt arbete genom att välja **spara** hello menyraden ovan.

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/slack/).

## <a name="more-connectors"></a>Flera kopplingar
Gå tillbaka toohello [API: er listan](apis-list.md).

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
