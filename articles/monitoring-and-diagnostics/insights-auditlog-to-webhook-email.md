---
title: "aaaCall en webhook på Azure-aktivitetsloggen aviseringar | Microsoft Docs"
description: "Vidarebefordra aktivitet logga händelser tooother Server för anpassade åtgärder. Till exempel skicka SMS, logga programfel eller meddela ett team via chatt/meddelandetjänst."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 64d333d1-7f37-4a00-9d16-dda6e69a113b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: johnkem
ms.openlocfilehash: 9017ff3e5165857ec7084a8f07f4123552e55f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a>Anropa en webhook i Azure-aktivitetsloggen aviseringar
Webhooks Tillåt tooroute en Azure Varna tooother meddelandesystem för efterbearbetning eller anpassade åtgärder. Du kan använda en webhook på en avisering tooroute den tooservices som skickar SMS, logga programfel, meddela ett team via chatt/meddelandetjänster eller göra en mängd olika andra åtgärder. Den här artikeln beskriver hur tooset en webhook toobe anropas när en varning utlöses Azure-aktivitetsloggen. Här visas också vilken hello nyttolasten för hello HTTP POST tooa webhook verkar vara. Information om hello installationen och schemat för en Azure mått avisering [se den här sidan i stället](insights-webhooks-alerts.md). Du kan också konfigurera ett e-postmeddelande med aktivitetsloggen avisering toosend när aktiverad.

> [!NOTE]
> Den här funktionen är för närvarande under förhandsgranskning och kommer att tas bort på någon punkt i hello framtida.
>
>

Du kan ställa in en aktivitetsloggen avisering med hello [Azure PowerShell-Cmdlets](insights-powershell-samples.md#create-metric-alerts), [plattformsoberoende CLI](insights-cli-samples.md#work-with-alerts), eller [REST-API för Azure-Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx). För närvarande kan du ange en med hello Azure-portalen.

## <a name="authenticating-hello-webhook"></a>Autentisera hello-webhook
Hej webhook kan autentisera med någon av följande metoder:

1. **Tokenbaserad auktorisering** -hello webhook URI sparas med ett token ID, t.ex.`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2. **Grundläggande auktorisering** -hello webhook URI sparas med ett användarnamn och lösenord, t.ex.`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Nyttolasten i schemat
hello POST-åtgärden innehåller hello följande JSON-nyttolast och schemat för alla aktivitetsloggen-baserade aviseringar. Det här schemat är liknande toohello en används av måttet-baserade aviseringar.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

| Elementnamn | Beskrivning |
| --- | --- |
| status |Används för mått aviseringar. Alltid inställt för ”aktiverad” för aktivitetsloggen aviseringar. |
| Kontexten |Kontexten för hello-händelse. |
| resourceProviderName |Hej resursprovidern av hello påverkas resurs. |
| conditionType |Alltid ”Event”. |
| namn |Namnet på hello varningsregel. |
| id |Resurs-ID för hello avisering. |
| description |Aviseringsbeskrivningen som angetts under skapandet av hello avisering. |
| subscriptionId |Azure prenumerations-ID. |
| tidsstämpel |Tiden på vilka hello händelsen skapades av hello Azure-tjänst som bearbetade hello-begäran. |
| resourceId |Resurs-ID för hello påverkas resurs. |
| resourceGroupName |Namnet på resursgruppen hello för hello påverkas resurs |
| properties |En uppsättning `<Key, Value>` par (d.v.s. `Dictionary<String, String>`) som innehåller information om hello-händelse. |
| Händelse |Element som innehåller metadata om hello-händelse. |
| Auktorisering |Hej RBAC egenskaper för hello-händelse. Dessa omfattar vanligtvis hello ”åtgärd” och ”roll” hello ”omfattningen”. |
| category |Kategori hello-händelse. Värden som stöds omfattar: administrativa, varning, säkerhet, ServiceHealth och rekommendation. |
| Anroparen |E-postadressen hello användaren som utförde hello operation, UPN-anspråk eller SPN-anspråk baserat på tillgänglighet. Kan vara null för vissa system-anrop. |
| correlationId |Vanligtvis ett GUID i strängformat. Händelser med correlationId tillhör toohello samma större åtgärd och dela vanligtvis en correlationId. |
| eventDescription |Statisk textbeskrivning av hello-händelse. |
| eventDataId |Unik identifierare för hello-händelse. |
| eventSource |Namnet på hello Azure-tjänst eller infrastruktur som genererade hello-händelse. |
| httpRequest |Inkluderar vanligtvis hello ”clientRequestId”, ”clientIpAddress” och ”metod” (HTTP-metoden anger t.ex.). |
| nivå |En av hello följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”. |
| Åtgärds-ID |Vanligtvis ett GUID som delas med hello händelser motsvarande toosingle igen. |
| operationName |Namnet på hello igen. |
| properties |Egenskaper för hello-händelse. |
| status |Sträng. Status för hello-åtgärd. Vanliga värden är: ”starta”, ”pågående”, ”Succeeded”, ”misslyckades”, ”aktiv”, ”löst”. |
| subStatus |Inkluderar vanligtvis hello HTTP-statuskoden hello motsvarande REST-anrop. Det kan även innehålla andra strängar som beskriver en sådan. Vanliga understatus värden är: OK (HTTP-statuskod: 200), skapade (HTTP-statuskod: 201), godkända (HTTP-statuskod: 202), inte innehåll (HTTP-statuskod: 204), felaktig begäran (HTTP-statuskod: 400), inte att hitta (HTTP-statuskod: 404), konflikt (HTTP-statuskod: 409), internt serverfel (HTTP-statuskod: 500), tjänsten är inte tillgänglig (HTTP-statuskod: 503), Gateway-Timeout (HTTP-statuskod: 504) |

## <a name="next-steps"></a>Nästa steg
* [Mer information om hello aktivitetsloggen](monitoring-overview-activity-logs.md)
* [Köra Azure Automation-skript (Runbooks) på Azure-aviseringar](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Använd Logikapp toosend ett SMS via Twilio från en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Det här exemplet är för mått aviseringar, men kan vara ändrade toowork med en aktivitetsloggen avisering.
* [Använd Logikapp toosend ett Slack meddelande från en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Det här exemplet är för mått aviseringar, men kan vara ändrade toowork med en aktivitetsloggen avisering.
* [Använd Logikapp toosend meddelandet tooan Azure Queue från en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Det här exemplet är för mått aviseringar, men kan vara ändrade toowork med en aktivitetsloggen avisering.
