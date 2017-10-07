---
title: "aaaRate begränsande för SMS, e-postmeddelanden och webhooks | Microsoft Docs"
description: "Förstå hur Azure begränsar hello antal möjliga SMS, e-post eller webhook meddelanden från en grupp."
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
ms.openlocfilehash: 1cd08a5b982c82bb02e0bf93451aa1fcd9bc34af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="rate-limiting-for-sms-messages-emails-and-webhook-posts"></a>Betygsätt begränsande för SMS-meddelanden, e-postmeddelanden och webhook inlägg
Hastighetsbegränsning är en upphävande av meddelanden som uppstår när för många meddelanden skickas tooa viss telefonnummer eller e-postadress. Hastighetsbegränsning säkerställer att aviseringar är hanterbara och rekommenderade åtgärder.

hello regler hello för SMS- och e-post är samma. hello hastighet gränsen tröskelvärdet är:

 - **SMS**: 10 meddelanden i en timme.
 - **E-post**: 100 meddelanden i en timme.

## <a name="rate-limit-rules"></a>Hastigheten med vilken gränsen regler
- Ett visst telefonnummer eller e-post är hastighet begränsad när den tar emot fler meddelanden än hello tröskeln tillåter.
- Ett telefonnummer eller e-post kan vara en del av åtgärdsgrupper mellan många prenumerationer. Hastighetsbegränsning gäller över alla prenumerationer. Det gäller så snart hello tröskelvärdet har uppnåtts, även om meddelanden skickas från flera prenumerationer.  
- När ett telefonnummer eller e-post är begränsad hastighet, skickas en ytterligare meddelandet toocommunicate hello hastighetsbegränsning. hello notification tillstånd när hello Betygsätt begränsa upphör att gälla.

## <a name="rate-limit-of-webhooks"></a>Hastighetsbegränsning för webhooks ##
Det finns ingen begränsning för webhooks hastighet.

## <a name="next-steps"></a>Nästa steg ##
* Lär dig mer om [SMS Varna beteende](monitoring-sms-alert-behavior.md).
* Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md), och lära dig hur tooreceive aviseringar.  
* Lär dig hur för[konfigurera aviseringar när ett meddelande om tjänstens hälsa är bokförd](monitoring-activity-log-alerts-on-service-notifications.md).
