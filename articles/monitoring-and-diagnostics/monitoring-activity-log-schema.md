---
title: "aaaAzure händelse för aktivitetslogg | Microsoft Docs"
description: "Förstå hello Händelseschema för data som sänds till hello aktivitetsloggen"
author: johnkemnetz
manager: robb
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: johnkem
ms.openlocfilehash: dfece949a20a4d9b4e8a4d488c1c34842d87d586
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-activity-log-event-schema"></a>Azure aktivitetsloggen Händelseschema
Hej **Azure-aktivitetsloggen** är en logg som ger inblick i prenumerationsnivån händelser som har inträffat i Azure. Den här artikeln beskriver hello Händelseschema per kategori av data.

## <a name="administrative"></a>Administrativa
Den här kategorin innehåller hello post för alla skapa, uppdatera, ta bort och åtgärden åtgärder utföras via Resource Manager. Exempel på typer av händelser som visas i den här kategorin omfattar hello ”Skapa virtuell dator” och ”ta bort nätverkssäkerhetsgruppen” varje åtgärd som en användare eller program med hjälp av hanteraren för filserverresurser modelleras som en åtgärd på en viss resurstyp. Om hello åtgärd av typen skriva, ta bort eller åtgärden hello-poster för både hello start- och lyckas eller misslyckas av åtgärden registreras i hello administrativa kategorin. hello administrativa kategorin omfattar även eventuella ändringar toorole-baserad åtkomstkontroll i en prenumeration.

### <a name="sample-event"></a>Exempelhändelse
```json
{
  "authorization": {
    "action": "microsoft.support/supporttickets/write",
    "role": "Subscription Admin",
    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
  },
  "caller": "admin@contoso.com",
  "channels": "Operation",
  "claims": {
    "aud": "https://management.core.windows.net/",
    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
    "iat": "1421876371",
    "nbf": "1421876371",
    "exp": "1421880271",
    "ver": "1.0",
    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
    "puid": "20030000801A118C",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
    "name": "John Smith",
    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
    "appidacr": "2",
    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
    "http://schemas.microsoft.com/claims/authnclassreference": "1"
  },
  "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "description": "",
  "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
  "eventName": {
    "value": "EndRequest",
    "localizedValue": "End request"
  },
  "httpRequest": {
    "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
    "clientIpAddress": "192.168.35.115",
    "method": "PUT"
  },
  "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
  "level": "Informational",
  "resourceGroupName": "MSSupportGroup",
  "resourceProviderName": {
    "value": "microsoft.support",
    "localizedValue": "microsoft.support"
  },
  "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
  "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "operationName": {
    "value": "microsoft.support/supporttickets/write",
    "localizedValue": "microsoft.support/supporttickets/write"
  },
  "properties": {
    "statusCode": "Created"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": "Created",
    "localizedValue": "Created (HTTP Status Code: 201)"
  },
  "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
  "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
  "subscriptionId": "s1"
}
```

### <a name="property-descriptions"></a>Egenskapsbeskrivningar
| Elementnamn | Beskrivning |
| --- | --- |
| Auktorisering |BLOB RBAC egenskaper för hello-händelse. Normalt innehåller hello ””, ”roll” och ”omfattning” egenskaper. |
| Anroparen |E-postadress för hello-användare som har utfört hello operation, UPN-anspråk eller SPN-anspråk baserat på tillgänglighet. |
| kanaler |En av hello följande värden: ”Admin”, ”åtgärden” |
| Anspråk |Hej JWT-token som används av Active Directory tooauthenticate hello användaren eller programmet tooperform åtgärden i hanteraren för filserverresurser. |
| correlationId |Vanligtvis ett GUID i hello-strängformat. Händelser som delar en correlationId tillhör toohello samma uber åtgärd. |
| description |Statisk textbeskrivning av en händelse. |
| eventDataId |Unik identifierare för en händelse. |
| httpRequest |BLOB som beskriver hello Http-begäran. Inkluderar vanligtvis hello ”clientRequestId”, ”clientIpAddress” och ”metod” (HTTP-metod. Till exempel PLACERA). |
| nivå |Nivå av hello-händelse. En av hello följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig” |
| resourceGroupName |Namnet på resursgruppen hello för hello påverkas resurs. |
| resourceProviderName |Namnet på hello resource provider för hello påverkas resurs |
| resourceId |Resurs-id för hello påverkas resurs. |
| Åtgärds-ID |Ett GUID som delas med hello händelser som motsvarar tooa enda åtgärd. |
| operationName |Namnet på hello igen. |
| properties |En uppsättning `<Key, Value>` par (det vill säga en ordlista) som beskriver hello information om hello-händelse. |
| status |Sträng som beskriver hello status för hello-åtgärd. Vissa vanliga värden: startas i pågår, slutfört, misslyckades, aktiv, löst. |
| subStatus |Vanligtvis hello HTTP-statuskoden hello motsvarande REST-anrop, men kan även innehålla andra strängar som beskriver en sådan, till exempel värdena vanliga: OK (HTTP-statuskod: 200), skapade (HTTP-statuskod: 201), godkända (HTTP-statuskod: 202), Nej innehåll (HTTP Statuskod: 204), felaktig begäran (HTTP-statuskod: 400) inte hittades (HTTP-statuskod: 404), konflikt (HTTP-statuskod: 409), internt serverfel (HTTP-statuskod: 500), tjänsten inte tillgänglig (HTTP-statuskod: 503), Gateway-Timeout (HTTP-statuskod: 504). |
| eventTimestamp |Tidsstämpel när hello händelse har genererats av hello Azure service bearbetning hello begära motsvarande hello-händelse. |
| submissionTimestamp |Tidsstämpel när hello händelse blev tillgängliga för frågor. |
| subscriptionId |Azure prenumerations-Id. |

## <a name="service-health"></a>Tjänstens hälsa
Den här kategorin innehåller hello post för varje service hälsa incidenter som har inträffat i Azure. Ett exempel på hello typen av händelse visas i den här kategorin är ”SQL Azure i östra USA upplever driftstopp”. Hälsa av tjänsten-händelser finns i fem sorter: åtgärd krävs stödd återställning, Incident, underhåll, Information eller säkerhet, och visas endast om du har en resurs i hello-prenumeration som skulle påverkas av hello-händelse.

### <a name="sample-event"></a>Exempelhändelse
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/mySubscriptionID/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/mySubscriptionID",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "mySubscriptionID",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited tooApp Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows toomitigate hello impact. hello next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited tooApp Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows toomitigate hello impact. hello next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```

### <a name="property-descriptions"></a>Egenskapsbeskrivningar
Elementnamn | Beskrivning
-------- | -----------
kanaler | Är en av hello följande värden: ”Admin”, ”åtgärden”
correlationId | Är vanligtvis ett GUID i hello-strängformat. Händelser med som tillhör toohello vanligtvis delar samma uber åtgärd hello samma correlationId.
description | Beskrivning av hello-händelse.
eventDataId | hello Unik identifierare för en händelse.
EventName | hello titel hello-händelse.
nivå | Nivå av hello-händelse. En av hello följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”
resourceProviderName | Namnet på hello resource provider för hello påverkas resurs. Detta kan vara null om det är inte känt.
resourceType| hello typ av resurs av hello påverkas resurs. Detta kan vara null om det är inte känt.
subStatus | Vanligtvis är null för händelser för Hälsotjänst.
eventTimestamp | Tidsstämpel när hello logga en händelse har genererats och skickats toohello aktivitetsloggen.
submissionTimestamp |   Tidsstämpel när hello händelse blev tillgängliga i hello aktivitetsloggen.
subscriptionId | hello Azure-prenumeration som den här händelsen loggades.
status | Sträng som beskriver hello status för hello-åtgärd. Vissa vanliga värden: aktiva, löst.
operationName | Namnet på hello igen. Vanligtvis Microsoft.ServiceHealth/incident/action.
category | ”ServiceHealth”
resourceId | Resurs-id för hello påverkas resurs, om den är känd. Prenumerations-ID har angetts på annat sätt.
Properties.title | hello lokaliserade rubrik för den här kommunikationen. Engelska är standardspråk för hello.
Properties.Communication | hello lokaliserad information hello-kommunikation med HTML-kod. Engelska är hello som standard.
Properties.incidentType | Möjliga värden: AssistedRecovery, ActionRequired, Information, incidenter, underhåll, säkerhet
Properties.trackingId | Identifierar hello incident som den här händelsen är associerad med. Använd den här toocorrelate hello händelser relaterade tooan incidenten.
Properties.impactedServices | En ESC JSON-blob som beskriver hello tjänster och områden som påverkas av hello incident. En lista över tjänster, som har en ServiceName och en lista över ImpactedRegions, som har en RegionName.
Properties.defaultLanguageTitle | hello-kommunikation på engelska
Properties.defaultLanguageContent | hello-kommunikation på engelska som HTML-kod eller oformaterad text
Properties.Stage | Möjliga värden för AssistedRecovery, ActionRequired, Information, incidenter, säkerhet: är aktiv, löst. De är för underhåll: aktiv, planerad, InProgress, Avbruten, Rescheduled, löst, Slutför
Properties.communicationId | hello kommunikation den här händelsen är kopplat.

## <a name="alert"></a>Varning
Den här kategorin innehåller hello post för alla aktiveringar av Azure-aviseringar. Ett exempel på hello typen av händelse visas i den här kategorin är ”processor på myVM har över 80 för hello senaste 5 minuter”. En mängd Azure system har en aviseringar begrepp – du kan definiera en regel av något slag och ett meddelande när villkor matchar regeln. Varje gång en stöds Azure aviseringstyp 'aktiveras,' eller hello villkor är uppfyllda toogenerate ett meddelande, en post för hello aktiveringen skickas också hello aktivitetsloggen toothis kategori.

### <a name="sample-event"></a>Exempelhändelse

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in hello last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "mySubscriptionID"
}
```

### <a name="property-descriptions"></a>Egenskapsbeskrivningar
| Elementnamn | Beskrivning |
| --- | --- |
| Anroparen | Alltid Microsoft.Insights/alertRules |
| kanaler | Alltid ”Admin, åtgärden” |
| Anspråk | JSON-blob med hello SPN (service principal name) eller resurs typ, varning hello-motorn. |
| correlationId | Ett GUID i hello-strängformat. |
| description |Statisk textbeskrivning av hello varning avisering. |
| eventDataId |Unik identifierare för hello varning avisering. |
| nivå |Nivå av hello-händelse. En av hello följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig” |
| resourceGroupName |Namnet på resursgruppen hello för hello påverkas resursen om den här varningen är mått. För andra aviseringstyper är hello namnet hello resursgruppen som innehåller hello avisering sig själv. |
| resourceProviderName |Namnet på hello resource provider för hello påverkas resursen om den här varningen är mått. För andra aviseringstyper är hello namnet på hello resource provider för hello aviseringen sig själv. |
| resourceId | Namnet på hello resurs-ID för hello påverkas resursen om den här varningen är mått. För andra aviseringstyper är hello resurs-ID för hello avisering resurs sig själv. |
| Åtgärds-ID |Ett GUID som delas med hello händelser som motsvarar tooa enda åtgärd. |
| operationName |Namnet på hello igen. |
| properties |En uppsättning `<Key, Value>` par (det vill säga en ordlista) som beskriver hello information om hello-händelse. |
| status |Sträng som beskriver hello status för hello-åtgärd. Vissa vanliga värden: startas i pågår, slutfört, misslyckades, aktiv, löst. |
| subStatus | Vanligtvis är null för aviseringar. |
| eventTimestamp |Tidsstämpel när hello händelse har genererats av hello Azure service bearbetning hello begära motsvarande hello-händelse. |
| submissionTimestamp |Tidsstämpel när hello händelse blev tillgängliga för frågor. |
| subscriptionId |Azure prenumerations-Id. |

### <a name="properties-field-per-alert-type"></a>Egenskapsfältet per typ av avisering
hello innehåller egenskaper fältet olika värden beroende på hello källan för hello varning avisering. Två vanliga varning avisering providers är aktivitetsloggen aviseringar och mått aviseringar.

#### <a name="properties-for-activity-log-alerts"></a>Egenskaper för aktivitetsloggen aviseringar
| Elementnamn | Beskrivning |
| --- | --- |
| properties.subscriptionId | hello prenumerations-ID från hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras. |
| properties.eventDataId | hello händelse data-ID från hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras. |
| properties.resourceGroup | hello resursgrupp från hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras. |
| properties.resourceId | hello resurs-ID från hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras. |
| properties.eventTimestamp | hello händelse tidsstämpel hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras. |
| properties.operationName | hello åtgärdsnamn från hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras. |
| Properties.status | hello status från hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras.|

#### <a name="properties-for-metric-alerts"></a>Egenskaper för mått aviseringar
| Elementnamn | Beskrivning |
| --- | --- |
| Egenskaper. RuleUri | Resurs-ID för hello mått varningsregeln sig själv. |
| Egenskaper. Regelnamn | hello namnet hello mått varningsregel. |
| Egenskaper. RuleDescription | hello beskrivning av hello mått varningsregeln (enligt definitionen i hello varningsregeln). |
| Egenskaper. Tröskelvärde | hello tröskelvärdet används i hello utvärdering av hello mått varningsregel. |
| Egenskaper. WindowSizeInMinutes | hello fönsterstorlek används i hello utvärdering av hello mått varningsregel. |
| Egenskaper. Aggregeringen | hello Aggregeringstyp har definierats i hello mått varningsregel. |
| Egenskaper. Operatorn | hello villkorsstyrd operatör används i hello utvärdering av hello mått varningsregel. |
| Egenskaper. MetricName | hello måttnamnet av hello mått används i hello utvärdering av hello mått varningsregel. |
| Egenskaper. MetricUnit | hello mått enhet för hello måttet används i hello utvärdering av hello mått varningsregel. |

## <a name="autoscale"></a>Automatisk skalning
Den här kategorin innehåller hello post för varje händelser relaterade toohello användning av hello Autoskala motorn baserat på automatiska inställningar du har definierat i din prenumeration. Ett exempel på hello typen av händelse visas i den här kategorin är ”Autoskala skalas upp misslyckades”. Använda autoskalning kan du automatiskt skala ut eller skala i hello antalet instanser i en stöds resurstyp baserat på tid på dagen och/eller belastningen (mått) data med hjälp av en autoskalningsinställning. När hello villkor är uppfyllda tooscale uppåt eller nedåt hello start och lyckades eller registreras misslyckade händelser i den här kategorin.

### <a name="sample-event"></a>Exempelhändelse
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "hello autoscale engine attempting tooscale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count too2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "hello autoscale engine attempting tooscale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count too2 instances count.",
    "ResourceName": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "mySubscriptionID"
}

```

### <a name="property-descriptions"></a>Egenskapsbeskrivningar
| Elementnamn | Beskrivning |
| --- | --- |
| Anroparen | Alltid Microsoft.Insights/autoscaleSettings |
| kanaler | Alltid ”Admin, åtgärden” |
| Anspråk | JSON-blob med hello SPN (service principal name) eller resurs typ av hello Autoskala motorn. |
| correlationId | Ett GUID i hello-strängformat. |
| description |Statisk textbeskrivning av hello Autoskala händelse. |
| eventDataId |Unik identifierare för hello Autoskala händelse. |
| nivå |Nivå av hello-händelse. En av hello följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig” |
| resourceGroupName |Namnet på resursgruppen hello för hello autoskalningsinställning. |
| resourceProviderName |Namnet på hello resource provider för hello autoskalningsinställning. |
| resourceId |Resurs-id för hello autoskalningsinställning. |
| Åtgärds-ID |Ett GUID som delas med hello händelser som motsvarar tooa enda åtgärd. |
| operationName |Namnet på hello igen. |
| properties |En uppsättning `<Key, Value>` par (det vill säga en ordlista) som beskriver hello information om hello-händelse. |
| Egenskaper. Beskrivning | Detaljerad beskrivning av vad hello Autoskala motorn utfördes. |
| Egenskaper. ResourceName | Resurs-ID för hello påverkas resurs (hello resurs på vilka hello skalningsåtgärd utfördes) |
| Egenskaper. OldInstancesCount | hello antal instanser före hello Autoskala åtgärden tog effekt. |
| Egenskaper. NewInstancesCount | hello antalet instanser när hello Autoskala åtgärden tog effekt. |
| Egenskaper. LastScaleActionTime | hello tidsstämpel när hello Autoskala åtgärd utfördes. |
| status |Sträng som beskriver hello status för hello-åtgärd. Vissa vanliga värden: startas i pågår, slutfört, misslyckades, aktiv, löst. |
| subStatus | Vanligtvis är null för Autoskala. |
| eventTimestamp |Tidsstämpel när hello händelse har genererats av hello Azure service bearbetning hello begära motsvarande hello-händelse. |
| submissionTimestamp |Tidsstämpel när hello händelse blev tillgängliga för frågor. |
| subscriptionId |Azure prenumerations-Id. |

## <a name="next-steps"></a>Nästa steg
* [Mer information om hello aktivitetsloggen (tidigare granskningsloggar)](monitoring-overview-activity-logs.md)
* [Strömma hello Azure-aktivitetsloggen tooEvent hubbar](monitoring-stream-activity-logs-event-hubs.md)
