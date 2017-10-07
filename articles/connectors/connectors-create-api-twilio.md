---
title: aaaAdd hello Twilio-anslutningen i dina logikappar i Azure | Microsoft Docs
description: "Översikt över hello Twilio-anslutningen med REST API-parametrar"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/19/2016
ms.author: mandia; ladocs
ms.openlocfilehash: b2b487f34bc76bee24b4237a71ee767d0d22ff7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-twilio-connector"></a>Kom igång med hello Twilio-koppling
Anslut tooTwilio toosend och ta emot globala IP, SMS och MMS-meddelanden. Med Twilio kan du:

* Skapa ditt företag flödet baserat på hello data du får från Twilio. 
* Använd åtgärder som får ett meddelande och listmeddelanden. De här åtgärderna få svar och hello utdata gör tillgängligt för andra åtgärder. När du får ett nytt Twilio-meddelande kan du exempelvis ta meddelandet och använda den ett Service Bus-arbetsflöde. 

Kom igång genom att skapa en logikapp; Se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-tootwilio"></a>Skapa en anslutning tooTwilio
När du lägger till den här anslutningen tooyour logikappar ange hello följande Twilio värden:

| Egenskap | Krävs | Beskrivning |
| --- | --- | --- |
| Konto-ID |Ja |Ange ditt Twilio-konto-ID |
| Åtkomst-Token |Ja |Ange ditt Twilio-åtkomsttoken |

> [!INCLUDE [Steps toocreate a connection tooTwilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

Om du inte har en Twilio-åtkomsttoken, se [användaridentitet & åtkomsttoken](https://www.twilio.com/docs/api/chat/guides/identity).

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/twilio/).

## <a name="more-connectors"></a>Flera kopplingar
Gå tillbaka toohello [API: er listan](apis-list.md).
