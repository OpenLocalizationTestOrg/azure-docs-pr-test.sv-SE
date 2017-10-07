---
title: "aaaAS2 spårning scheman för övervakning av B2B - Azure Logic Apps | Microsoft Docs"
description: "Använd AS2 spåra scheman toomonitor B2B-meddelanden i transaktioner i ditt Azure-konto för integrering."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f169c411-1bd7-4554-80c1-84351247bf94
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fe3c5845e2e80160d6857d8c308d836e88af7331
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="start-or-enable-tracking-of-as2-messages-and-mdns-toomonitor-success-errors-and-message-properties"></a>Starta eller aktivera spårning av AS2-meddelanden och MDNs toomonitor lyckades, fel och meddelandeegenskaper
Du kan använda dessa scheman för AS2-spårning i ditt Azure-integrering konto toohelp du övervaka business-to-business (B2B) transaktioner:

* AS2 meddelandet spårning av schemat
* AS2 MDN spårning av schemat

## <a name="as2-message-tracking-schema"></a>AS2 meddelandet spårning av schemat
````java

    {
       "agreementProperties": {  
            "senderPartnerName": "",  
            "receiverPartnerName": "",  
            "as2To": "",  
            "as2From": "",  
            "agreementName": ""  
        },  
        "messageProperties": {
            "direction": "",
            "messageId": "",
            "dispositionType": "",
            "fileName": "",
            "isMessageFailed": "",
            "isMessageSigned": "",
            "isMessageEncrypted": "",
            "isMessageCompressed": "",
            "correlationMessageId": "",
            "incomingHeaders": {
            },
            "outgoingHeaders": {
            },
        "isNrrEnabled": "",
        "isMdnExpected": "",
        "mdnType": ""
        }
    }
````

| Egenskap | Typ | Beskrivning |
| --- | --- | --- |
| senderPartnerName | Sträng | AS2 meddelandet partner avsändare. (Valfritt) |
| receiverPartnerName | Sträng | Mottagaren AS2-meddelande Partnernamn. (Valfritt) |
| as2To | Sträng | AS2 meddelandet mottagarens namn från hello hälsningsmeddelande AS2-huvuden. (Obligatoriskt) |
| as2From | Sträng | AS2 meddelandet avsändare, från hello hälsningsmeddelande AS2-huvuden. (Obligatoriskt) |
| agreementName | Sträng | Namnet på AS2-avtal toowhich hello hälsningsmeddelande matchas. (Valfritt) |
| Riktning | Sträng | Flödesriktning hello meddelande, ta emot och skicka. (Obligatoriskt) |
| messageId | Sträng | AS2 meddelande-ID från hello-huvuden hello AS2-meddelande (valfritt) |
| dispositionType |Sträng | Meddelandet Disposition meddelande (MDN) disposition TYPVÄRDE. (Valfritt) |
| fileName | Sträng | Namnet på filen från hello rubriken för hello AS2-meddelande. (Valfritt) |
| isMessageFailed |Booleskt värde | Om hello AS2-meddelandet misslyckades. (Obligatoriskt) |
| isMessageSigned | Booleskt värde | Om hello AS2 meddelandet signerades. (Obligatoriskt) |
| isMessageEncrypted | Booleskt värde | Om hälsningsmeddelande AS2 krypterades. (Obligatoriskt) |
| isMessageCompressed |Booleskt värde | Om hälsningsmeddelande AS2 har komprimerats. (Obligatoriskt) |
| correlationMessageId | Sträng | AS2 meddelande-ID, toocorrelate meddelanden med MDNs. (Valfritt) |
| incomingHeaders |Ordlista för JToken | AS2-huvud detaljer för inkommande meddelanden. (Valfritt) |
| outgoingHeaders |Ordlista för JToken | Utgående detaljer om AS2-huvud. (Valfritt) |
| isNrrEnabled | Booleskt värde | Använd standardvärdet om hello-värdet inte är känd. (Obligatoriskt) |
| isMdnExpected | Booleskt värde | Använd standardvärdet om hello-värdet inte är känd. (Obligatoriskt) |
| mdnType | Enum | Tillåtna värden är **NotConfigured**, **Sync**, och **asynkrona**. (Obligatoriskt) |

## <a name="as2-mdn-tracking-schema"></a>AS2 MDN spårning av schemat
````java

    {
        "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "as2To": "",
                "as2From": "",
                "agreementName": "g"
            },
            "messageProperties": {
                "direction": "",
                "messageId": "",
                "originalMessageId": "",
                "dispositionType": "",
                "isMessageFailed": "",
                "isMessageSigned": "",
                "isNrrEnabled": "",
                "statusCode": "",
                "micVerificationStatus": "",
                "correlationMessageId": "",
                "incomingHeaders": {
                },
                "outgoingHeaders": {
                }
            }
    }
````

| Egenskap | Typ | Beskrivning |
| --- | --- | --- |
| senderPartnerName | Sträng | AS2 meddelandet partner avsändare. (Valfritt) |
| receiverPartnerName | Sträng | Mottagaren AS2-meddelande Partnernamn. (Valfritt) |
| as2To | Sträng | Partnernamn som tar emot hello AS2-meddelande. (Obligatoriskt) |
| as2From | Sträng | Partnernamn som skickar hello AS2-meddelande. (Obligatoriskt) |
| agreementName | Sträng | Namnet på AS2-avtal toowhich hello hälsningsmeddelande matchas. (Valfritt) |
| Riktning |Sträng | Flödesriktning hello meddelande, ta emot och skicka. (Obligatoriskt) |
| messageId | Sträng | AS2-meddelande-ID. (Valfritt) |
| originalMessageId |Sträng | AS2 ursprungliga meddelande-ID. (Valfritt) |
| dispositionType | Sträng | MDN disposition TYPVÄRDE. (Valfritt) |
| isMessageFailed |Booleskt värde | Om hello AS2-meddelandet misslyckades. (Obligatoriskt) |
| isMessageSigned |Booleskt värde | Om hello AS2 meddelandet signerades. (Obligatoriskt) |
| isNrrEnabled | Booleskt värde | Använd standardvärdet om hello-värdet inte är känd. (Obligatoriskt) |
| statusCode | Enum | Tillåtna värden är **godkända**, **Avvisad**, och **AcceptedWithErrors**. (Obligatoriskt) |
| micVerificationStatus | Enum | Tillåtna värden är **NotApplicable**, **lyckades**, och **misslyckades**. (Obligatoriskt) |
| correlationMessageId | Sträng | Korrelations-ID. hello ursprungliga postmeddelandet ID (hello meddelande-ID för hello-meddelande som MDN är konfigurerat). (Valfritt) |
| incomingHeaders | Ordlista för JToken | Anger sidhuvudet detaljer för inkommande meddelanden. (Valfritt) |
| outgoingHeaders |Ordlista för JToken | Anger utgående meddelande sidhuvud information. (Valfritt) |

## <a name="next-steps"></a>Nästa steg
* Mer information om hello [Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md).    
* Lär dig mer om [övervakning B2B-meddelanden](logic-apps-monitor-b2b-message.md).   
* Lär dig mer om [B2B anpassade spårning scheman](logic-apps-track-integration-account-custom-tracking-schema.md).   
* Lär dig mer om [X12 spårning scheman](logic-apps-track-integration-account-x12-tracking-schema.md).   
* Lär dig mer om [spåra B2B-meddelanden i hello Operations Management Suite-portalen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
