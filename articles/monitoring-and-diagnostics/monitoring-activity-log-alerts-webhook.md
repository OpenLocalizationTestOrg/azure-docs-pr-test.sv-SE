---
title: "aaaUnderstand hello webhook schemat som används i aktiviteten loggen aviseringar | Microsoft Docs"
description: "Läs mer om hello schemat för hello JSON som är bokförd tooa Webhooksadressen när en aktivitet loggen avisering aktiveras."
author: johnkemnetz
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
ms.author: johnkem
ms.openlocfilehash: 75562e0589222d3e392ea73eacfd7414a422d115
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="webhooks-for-azure-activity-log-alerts"></a>Webhooks för Azure aktiviteten Logga varningar
Som en del av hello definition av en grupp kan konfigurera du webhook slutpunkter tooreceive aktivitet loggen varningsmeddelanden. Du kan vidarebefordra dessa meddelanden tooother system för efterbearbetning eller anpassade åtgärder med webhooks. Den här artikeln visar vilka hello nyttolasten för hello HTTP POST tooa webhook verkar vara.

Mer information om aktiviteten loggen aviseringar finns i hur för[skapa Azure aktivitet Logga varningar](monitoring-activity-log-alerts.md).

Mer information om åtgärdsgrupper finns hur för[skapa åtgärdsgrupper](monitoring-action-groups.md).

## <a name="authenticate-hello-webhook"></a>Autentisera hello-webhook
Hej webhook kan eventuellt använda tokenbaserad auktorisering för autentisering. Hej webhook URI sparas med ett token ID, till exempel `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.

## <a name="payload-schema"></a>Nyttolasten i schemat
hello JSON-nyttolast som ingår i hello efter åtgärden skiljer sig baserat på hello nyttolast data.context.activityLog.eventSource fält.

###<a name="common"></a>Vanliga
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "channels": "Operation",
                "correlationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "eventSource": "Administrative",
                "eventTimestamp": "2017-03-29T15:43:08.0019532+00:00",
                "eventDataId": "8195a56a-85de-4663-943e-1a2bf401ad94",
                "level": "Informational",
                "operationName": "Microsoft.Insights/actionGroups/write",
                "operationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "status": "Started",
                "subStatus": "",
                "subscriptionId": "52c65f65-0518-4d37-9719-7dbbfc68c57a",
                "submissionTimestamp": "2017-03-29T15:43:20.3863637+00:00",
                ...
            }
        },
        "properties": {}
    }
}
```
###<a name="administrative"></a>Administrativa
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "authorization": {
                    "action": "Microsoft.Insights/actionGroups/write",
                    "scope": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions"
                },
                "claims": "{...}",
                "caller": "me@contoso.com",
                "description": "",
                "httpRequest": "{...}",
                "resourceId": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions",
                "resourceGroupName": "CONTOSO-TEST",
                "resourceProviderName": "Microsoft.Insights",
                "resourceType": "Microsoft.Insights/actionGroups"
            }
        },
        "properties": {}
    }
}

```
###<a name="servicehealth"></a>ServiceHealth
```json
{
    "schemaId": "unknown",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "properties": {
                    "title": "...",
                    "service": "...",
                    "region": "...",
                    "communication": "...",
                    "incidentType": "Incident",
                    "trackingId": "...",
                    "groupId": "...",
                    "impactStartTime": "3/29/2017 3:43:21 PM",
                    "impactMitigationTime": "3/29/2017 3:43:21 PM",
                    "eventCreationTime": "3/29/2017 3:43:21 PM",
                    "impactedServices": "[{...}]",
                    "defaultLanguageTitle": "...",
                    "defaultLanguageContent": "...",
                    "stage": "Active",
                    "communicationId": "...",
                    "version": "0.1"
                }
            }
        },
        "properties": {}
    }
}
```

Specifika schemainformation om tjänsten notification aktivitet loggen hälsovarningar, finns [tjänsten meddelanden om hälsostatus](monitoring-service-notifications.md).

Information om specifika schemat på alla andra aktiviteten loggen aviseringar finns [översikt över hello Azure-aktivitetsloggen](monitoring-overview-activity-logs.md).

| Elementnamn | Beskrivning |
| --- | --- |
| status |Används för mått aviseringar. Alltid inställt för ”aktiverad” för aktiviteten loggen aviseringar. |
| Kontexten |Kontexten för hello-händelse. |
| resourceProviderName |Hej resursprovidern av hello påverkas resurs. |
| conditionType |Alltid ”Event”. |
| namn |Namnet på hello varningsregel. |
| id |Resurs-ID för hello avisering. |
| description |Aviseringsbeskrivningen när hello avisering skapas. |
| subscriptionId |Azure prenumerations-ID. |
| tidsstämpel |Tiden på vilka hello händelsen skapades av hello Azure-tjänst som bearbetade hello-begäran. |
| resourceId |Resurs-ID för hello påverkas resurs. |
| resourceGroupName |Namnet på resursgruppen hello för hello påverkas resurs. |
| properties |En uppsättning `<Key, Value>` par (det vill säga `Dictionary<String, String>`) som innehåller information om hello-händelse. |
| Händelse |Element som innehåller metadata om hello-händelse. |
| Auktorisering |hello rollbaserad åtkomstkontroll egenskaper för hello-händelse. Dessa egenskaper innehåller vanligtvis hello åtgärd, hello roll och hello omfång. |
| category |Kategori hello-händelse. Värden som stöds omfattar administrativa, varning, säkerhet, ServiceHealth och rekommendation. |
| Anroparen |E-postadressen hello användaren som utförde hello operation, UPN-anspråk eller SPN-anspråk baserat på tillgänglighet. Kan vara null för vissa system-anrop. |
| correlationId |Vanligtvis ett GUID i strängformat. Händelser med correlationId tillhör toohello samma större åtgärd och dela vanligtvis en correlationId. |
| eventDescription |Statisk textbeskrivning av hello-händelse. |
| eventDataId |Unik identifierare för hello-händelse. |
| eventSource |Namnet på hello Azure-tjänst eller infrastruktur som genererade hello-händelse. |
| httpRequest |hello begäran innehåller vanligen hello clientRequestId clientIpAddress och HTTP-metoden (till exempel PLACERA). |
| nivå |Något av följande värden hello: kritisk, fel, varning, information och utförlig. |
| Åtgärds-ID |Vanligtvis ett GUID som delas med hello händelser motsvarande toosingle igen. |
| operationName |Namnet på hello igen. |
| properties |Egenskaper för hello-händelse. |
| status |Sträng. Status för hello-åtgärd. Vanliga värden är igång, pågår, slutfört, misslyckades, aktiv och löst. |
| subStatus |Inkluderar vanligtvis hello HTTP-statuskoden hello motsvarande REST-anrop. Det kan även innehålla andra strängar som beskriver en sådan. Vanliga understatus värden är OK (HTTP-statuskod: 200), skapade (HTTP-statuskod: 201), godkända (HTTP-statuskod: 202), inte innehåll (HTTP-statuskod: 204), felaktig begäran (HTTP-statuskod: 400), det gick inte att hitta (HTTP-statuskod: 404), konflikt (HTTP-statuskod: 409 ), Internt serverfel (HTTP-statuskod: 500), tjänsten inte tillgänglig (HTTP-statuskod: 503), och Gateway-Timeout (HTTP-statuskod: 504). |

## <a name="next-steps"></a>Nästa steg
* [Mer information om hello aktivitetsloggen](monitoring-overview-activity-logs.md).
* [Köra Azure automation-skript (Runbooks) på Azure-aviseringar](http://go.microsoft.com/fwlink/?LinkId=627081).
* [Använd en logik app toosend ett SMS via Twilio från en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Det här exemplet är för mått aviseringar, men det kan vara ändrade toowork med en varning för loggen.
* [Använd en logik app toosend ett Slack meddelande från en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Det här exemplet är för mått aviseringar, men det kan vara ändrade toowork med en varning för loggen.
* [Använd en logik app toosend meddelande-tooan Azure kö i en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Det här exemplet är för mått aviseringar, men det kan vara ändrade toowork med en varning för loggen.
