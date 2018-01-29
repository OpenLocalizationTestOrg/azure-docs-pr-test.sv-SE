---
title: "SMS-avisering beteende i åtgärdsgrupper | Microsoft Docs"
description: "SMS-meddelandeformat och svara på SMS-meddelanden för att avbryta prenumerationen, prenumerera igen eller be om hjälp."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 012f001356463a8a7d9b95f186111959627f2c28
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/08/2017
---
# <a name="sms-alert-behavior-in-action-groups"></a>SMS Varna beteende i åtgärdsgrupper
## <a name="overview"></a>Översikt ##
Åtgärdsgrupper kan du konfigurera en lista över mottagare. Dessa grupper kan sedan utnyttjas när du definierar aktiviteten Logga varningar; Se till att en viss grupp meddelas när aktiviteten loggen avisering utlöses. En av de aviseringsdata mekanismer som stöds är SMS; aviseringarna stöd för dubbelriktad kommunikation. En användare kan svara på en avisering till:

- **Prenumerationen aviseringar:** en användare kan säga upp prenumerationen från alla SMS-aviseringar för alla åtgärdsgrupper som eller en enda grupp.  
- **Prenumerera igen på varningar:** en användare kan prenumerera igen till alla SMS-aviseringar för alla åtgärdsgrupper som eller en enda grupp.  
- **Be om hjälp:** en användare kan be om mer information om SMS. De dirigeras till den här artikeln

Den här artikeln beskriver beteendet för SMS-aviseringar och svar åtgärder användaren kan utföra baserat på de nationella inställningarna för användaren:

## <a name="usacanada-sms-behavior"></a>USA/Kanada SMS-beteende
### <a name="receiving-an-sms-alert"></a>Ta emot en SMS-avisering
En SMS-mottagare som är konfigurerad som en del av en grupp, får ett SMS när en avisering utlöses. SMS ska utföra följande uppgifter:
* Kort filnamn i åtgärdsgruppen aviseringen har skickats till
- Rubrik på aviseringen

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a>Avsluta abonnemang från SMS-aviseringar för en grupp
En användare kan säga upp prenumerationen från SMS för aviseringar för en grupp genom att besvara kort kod 29873 med nyckelorden ”: inaktivera &lt;kort filnamn för grupp&gt;”.

T.ex. En användare som vill avbryta prenumerationen på aviseringar för en grupp med kort filnamn ”Azure” skulle skicka ett SMS till kort koden 29873 som säger ”inaktivera Azure”

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a>Avsluta abonnemang från SMS-varningar för alla åtgärdsgrupper
En användare kan säga upp prenumerationen från alla SMS-aviseringar för alla åtgärdsgrupper som genom att besvara kort kod 29873 med någon av följande nyckelord:
* STANNA

T.ex. En användare som vill avbryta prenumerationen på alla SMS-aviseringar för alla åtgärdsgrupper skulle skicka ett SMS till kort koden 29873 som säger ”stoppa”

>[!NOTE]
>Om en användare har avbrutit från SMS aviseringar, men läggs sedan till en ny åtgärdsgrupp; de ska ta emot SMS-aviseringar för den nya gruppen åtgärd, men förblir avbryta prenumerationen på alla åtgärdsgrupper för föregående.
>
>

### <a name="resubscribing-to-sms-alerts-for-one-action-group"></a>Resubscribing till SMS-aviseringar för en grupp
En användare kan prenumerera igen SMS för aviseringar för en grupp genom att besvara kort kod 29873 med nyckelorden ”: aktivera &lt;kort filnamn för grupp&gt;”.

T.ex. En användare som vill prenumerera igen till aviseringar för en grupp med kort filnamn ”Azure” skulle skicka ett SMS till kort koden 29873 som säger ”aktivera Azure”

### <a name="resubscribing-to-sms-alerts-for-all-action-groups"></a>Resubscribing till SMS-aviseringar för alla åtgärdsgrupper
En användare kan prenumerera igen till alla SMS för aviseringar för alla åtgärdsgrupper genom att besvara kort kod 29873 med någon av följande nyckelord:

* STARTA

T.ex. En användare som vill avbryta prenumerationen på alla SMS-aviseringar för alla åtgärdsgrupper skulle skicka ett SMS till kort koden 29873 som säger ”START”

### <a name="requesting-help-via-sms"></a>Begär hjälp via SMS
En användare kan be om mer information om SMS som de har fått genom att besvara kort kod 29873 med någon av följande nyckelord:
* HJÄLP

Ett svar skickas till användaren med en länk till den här artikeln.

## <a name="next-steps"></a>Nästa steg
Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md) och lär dig att få aviseringar  
Lär dig mer om [SMS hastighetsbegränsning](monitoring-alerts-rate-limiting.md)  
Lär dig mer om [åtgärdsgrupper](monitoring-action-groups.md)
