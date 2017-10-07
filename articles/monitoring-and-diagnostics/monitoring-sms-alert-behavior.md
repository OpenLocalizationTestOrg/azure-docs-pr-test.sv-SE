---
title: "aaaSMS avisering beteende i åtgärdsgrupper | Microsoft Docs"
description: "SMS-meddelandeformat och svarar tooSMS meddelanden toounsubscribe prenumerera igen eller be om hjälp."
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
ms.openlocfilehash: 3cd09b1903e3472f6402f62b74409d97e7e7ea97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sms-alert-behavior-in-action-groups"></a>SMS Varna beteende i åtgärdsgrupper
## <a name="overview"></a>Översikt ##
Åtgärdsgrupper kan tooconfigure en lista över mottagare. Dessa grupper kan sedan utnyttjas när du definierar aktiviteten Logga varningar; Se till att en viss grupp meddelas när hello aktivitet loggen avisering utlöses. En av hello aviseringar mekanismer som stöds är SMS; hello aviseringar stöd för dubbelriktad kommunikation. En användare kan svara tooan aviseringen:

- **Prenumerationen aviseringar:** en användare kan säga upp prenumerationen från alla SMS-aviseringar för alla åtgärdsgrupper som eller en enda grupp.  
- **Prenumerera igen tooalerts:** en användare kan prenumerera igen tooall SMS-aviseringar för alla åtgärdsgrupper som eller en enda grupp.  
- **Be om hjälp:** en användare kan be om mer information om hello SMS. De kommer att omdirigerade toothis artikel

Den här artikeln beskriver hello funktionssätt hello SMS aviseringar och hello svar åtgärder hello användaren kan utföra baserat på hello språk för hello användare:

## <a name="usacanada-sms-behavior"></a>USA/Kanada SMS-beteende
### <a name="receiving-an-sms-alert"></a>Ta emot en SMS-avisering
En SMS-mottagare som är konfigurerad som en del av en grupp, får ett SMS när en avisering utlöses. hello SMS hanterar hello följande information:
* Kort filnamn för hello grupp aviseringen har skickats till
- Rubrik på hello avisering

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a>Avsluta abonnemang från SMS-aviseringar för en grupp
En användare kan säga upp prenumerationen från SMS för aviseringar för en grupp av svarande toohello shortcode 20873 med hello nyckelord ”: inaktivera &lt;kort filnamn för grupp&gt;”.

T.ex. En användare som du önskar toounsubscribe från aviseringar för en grupp med hello kort filnamn ”Azure” skulle skicka SMS toohello shortcode 20873 som säger ”inaktivera Azure”

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a>Avsluta abonnemang från SMS-varningar för alla åtgärdsgrupper
En användare kan säga upp prenumerationen från alla SMS-aviseringar för alla åtgärdsgrupper av svarande toohello shortcode 20873 med någon av följande nyckelord hello:
* STANNA

T.ex. En användare som du önskar toounsubscribe från alla SMS-aviseringar för alla åtgärdsgrupper skulle skicka SMS toohello shortcode 20873 som säger ”stoppa”

>[!NOTE]
>Om en användare har avbrutit från SMS aviseringar, men läggs sedan tooa ny grupp; de ska ta emot SMS-aviseringar för den nya gruppen åtgärd, men förblir avbryta prenumerationen på alla åtgärdsgrupper för föregående.
>
>

### <a name="resubscribing-toosms-alerts-for-one-action-group"></a>Resubscribing tooSMS aviseringar för en grupp
En användare kan prenumerera igen tooSMS för aviseringar för en grupp av svarande toohello shortcode 20873 med hello nyckelord ”: aktivera &lt;kort filnamn för grupp&gt;”.

T.ex. En användare som du önskar tooresubscribe tooalerts för en grupp med hello kort filnamn ”Azure” skulle skicka SMS toohello shortcode 20873 som säger ”aktivera Azure”

### <a name="resubscribing-toosms-alerts-for-all-action-groups"></a>Resubscribing tooSMS aviseringar för alla åtgärdsgrupper
En användare kan prenumerera igen tooall SMS för aviseringar för alla åtgärdsgrupper av svarande toohello shortcode 20873 med någon av följande nyckelord hello:

* STARTA

T.ex. En användare som du önskar toounsubscribe från alla SMS-aviseringar för alla åtgärdsgrupper skulle skicka SMS toohello shortcode 20873 som säger ”START”

### <a name="requesting-help-via-sms"></a>Begär hjälp via SMS
En användare kan be om mer information om hello SMS de har fått av svarande toohello shortcode 20873 med någon av följande nyckelord hello:
* HJÄLP

Ett svar skickas toohello användare med en länk toothis artikel.

## <a name="next-steps"></a>Nästa steg
Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md) och lära dig hur tooget aviseringar  
Lär dig mer om [SMS hastighetsbegränsning](monitoring-alerts-rate-limiting.md)  
Lär dig mer om [åtgärdsgrupper](monitoring-action-groups.md)
