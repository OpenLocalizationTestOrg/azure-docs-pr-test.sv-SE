---
title: "aaaCustom upp scheman för B2B övervakning - Azure Logic Apps | Microsoft Docs"
description: "Skapa anpassade spårning scheman toomonitor B2B-meddelanden från transaktioner i ditt Azure-konto för integrering."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8cf26a43d89f0414a2a8c5ef59d804235afeb5d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-tracking-toomonitor-your-complete-workflow-end-to-end"></a>Aktivera spårning toomonitor hela arbetsflödet, slutpunkt-till-slutpunkt
Det finns inbyggda spåra att du kan aktivera för olika delar av ditt företag att arbetsflödet, till exempel uppföljning AS2 eller X12 meddelanden. När du skapar arbetsflöden som omfattar en logikapp, BizTalk Server, SQL Server eller något annat lager kan du aktivera anpassade spårning loggar händelser från hello början toohello slutet av arbetsflödet. 

Det här avsnittet innehåller anpassad kod som du kan använda i hello lager utanför din logikapp. 

## <a name="custom-tracking-schema"></a>Spårning av anpassade schemat
````java

        {
            "sourceType": "",
            "source": {

            "workflow": {
                "systemId": ""
            },
            "runInstance": {
                "runId": ""
            },
            "operation": {
                "operationName": "",
                "repeatItemScopeName": "",
                "repeatItemIndex": "",
                "trackingId": "",
                "correlationId": "",
                "clientRequestId": ""
                }
            },
            "events": [
            {
                "eventLevel": "",
                "eventTime": "",
                "recordType": "",
                "record": {                
                }
            }
         ]
      }

````

| Egenskap | Typ | Beskrivning |
| --- | --- | --- |
| SourceType |   | Typ av hello kör källa. Tillåtna värden är **Microsoft.Logic/workflows** och **anpassade**. (Obligatoriskt) |
| Källa |   | Om hello källtypen är **Microsoft.Logic/workflows**, hello källinformation måste toofollow det här schemat. Om hello källtypen är **anpassade**, hello-schemat är en JToken. (Obligatoriskt) |
| system-ID | Sträng | Logik app system-ID. (Obligatoriskt) |
| runId | Sträng | Logikapp kör ID. (Obligatoriskt) |
| operationName | Sträng | Namn på hello-åtgärd (till exempel åtgärd eller utlösare). (Obligatoriskt) |
| repeatItemScopeName | Sträng | Upprepa objektnamnet om hello åtgärd inuti en `foreach` / `until` loop. (Obligatoriskt) |
| repeatItemIndex | Integer | Om hello åtgärden är inuti en `foreach` / `until` loop. Anger hello upprepade objektindex. (Obligatoriskt) |
| trackingId | Sträng | Spårnings-ID, toocorrelate hälsningsmeddelande. (Valfritt) |
| correlationId | Sträng | Korrelations-ID, toocorrelate hälsningsmeddelande. (Valfritt) |
| ClientRequestId | Sträng | Klienten kan fylla i den toocorrelate meddelanden. (Valfritt) |
| EventLevel |   | Nivå av hello-händelse. (Obligatoriskt) |
| EventTime |   | Tid för hello händelsen i UTC-formatet ÅÅÅÅ-MM-DDTHH:MM:SS.00000Z. (Obligatoriskt) |
| RecordType |   | Typ av hello spåra post. Tillåtna värde är **anpassade**. (Obligatoriskt) |
| Post |   | Anpassad posttyp. Tillåtna format är JToken. (Obligatoriskt) |

## <a name="b2b-protocol-tracking-schemas"></a>B2B-protokollet spårning scheman
Information om spårning av scheman B2B-protokollet finns i:
* [AS2-spårningsscheman](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [X12-spårningsscheman](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [övervakning B2B-meddelanden](logic-apps-monitor-b2b-message.md).   
* Lär dig mer om [spåra B2B-meddelanden i hello Operations Management Suite-portalen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
* Mer information om hello [Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md).
