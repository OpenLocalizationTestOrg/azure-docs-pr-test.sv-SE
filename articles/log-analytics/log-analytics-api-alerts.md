---
title: aaaUsing OMS-Log Analytics avisering REST-API
description: "hello Log Analytics avisering REST-API kan du toocreate och hantera aviseringar i logganalys som är en del av Operations Management Suite (OMS).  Den här artikeln innehåller information om hello API och flera exempel för att utföra olika åtgärder."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 628ad256-7181-4a0d-9e68-4ed60c0f3f04
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 418dc7eb71d6151c6380b8925f1f147a0e13b178
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a>Skapa och hantera Varningsregler i logganalys med REST API
hello Log Analytics avisering REST-API kan du toocreate och hantera aviseringar i Operations Management Suite (OMS).  Den här artikeln innehåller information om hello API och flera exempel för att utföra olika åtgärder.

hello Log Analytics Sök REST API är RESTful och kan nås via hello Azure Resource Manager REST API. I det här dokumentet hittar du exempel där hello API kan nås från ett PowerShell-kommandorad med [ARMClient](https://github.com/projectkudu/ARMClient), en öppen källkod kommandoradsverktyg som förenklar anropar hello Azure Resource Manager API. hello användning av ARMClient och PowerShell är en av många alternativ tooaccess hello Log Analytics Sök-API. Du kan använda hello RESTful Azure Resource Manager API toomake anrop tooOMS arbetsytor och utföra sökkommandon i dem med dessa verktyg. hello API kommer utdata sökresultat tooyou i JSON-format, vilket gör att du toouse hello sökresultat på många olika sätt programmässigt.

## <a name="prerequisites"></a>Krav
Aviseringar kan för närvarande kan endast skapas med en sparad sökning i logganalys.  Du kan se toohello [loggen Sök REST API](log-analytics-log-search-api.md) för mer information.

## <a name="schedules"></a>Scheman
En sparad sökning kan ha ett eller flera scheman. hello schema definierar hur ofta hello sökningen ska köras och hello tidsintervall under vilka villkor hello identifieras.
Scheman har hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| intervall |Hur ofta hello sökning körs. Mätt i minuter. |
| QueryTimeSpan |hello tidsintervall under vilka hello villkor utvärderas. Måste vara lika tooor större än intervall. Mätt i minuter. |
| Version |hello API-version som används.  För närvarande är ska detta alltid anges too1. |

Anta till exempel att en fråga med ett intervall på 15 minuter och Timespan 30 minuter. I det här fallet hello-frågan skulle köras var 15: e minut och skulle att utlösa en avisering om hello kriterier fortfarande tooresolve tootrue under ett 30 minuters intervall.

### <a name="retrieving-schedules"></a>Hämtar scheman
Använd hello hämta metoden tooretrieve alla scheman för en sparad sökning.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Använd hello Get-metod med en schema-ID tooretrieve ett visst schema för en sparad sökning.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Följande är ett exempelsvar för ett schema.

```json
{
    "value": [{
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
            "Interval": 15,
            "QueryTimeSpan": 15
        }
    }]
}
```

### <a name="creating-a-schedule"></a>Skapa ett schema
Använda hello Put-metoden med ett unikt schema-ID toocreate ett nytt schema.  Observera att två scheman inte kan ha hello samma ID även om de är kopplade till olika sparade sökningar.  När du skapar ett schema i hello OMS-konsolen, skapas ett GUID för hello schema-ID.

> [!NOTE]
> hello namn för alla sparade sökningar, scheman och åtgärder som skapats med hello Log Analytics API måste vara i gemener.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Ändringar i schemat
Använda hello Put-metoden med ett befintligt schema-ID för hello samma sparade söka toomodify som schemalägger.  hello brödtexten i begäran hello måste innehålla hello etag hello schema.

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Ta bort scheman
Använda hello Delete-metoden med en schema-ID toodelete ett schema.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Åtgärder
Ett schema kan ha flera åtgärder. En åtgärd kan definiera en eller flera processer tooperform som skickar ett e-post eller starta en runbook eller den kan definiera ett tröskelvärde som bestämmer när hello resultaten av en sökning som matchar vissa villkor.  Vissa åtgärder definiera både så att hello processer utförs när hello tröskelvärdet har uppnåtts.

Alla åtgärder har hello egenskaper i hello i den följande tabellen.  Olika typer av aviseringar har olika ytterligare egenskaper som beskrivs nedan.

| Egenskap | Beskrivning |
|:--- |:--- |
| Typ |Typ av hello-åtgärd.  Hello möjliga värden är för närvarande aviseringen och Webhooken. |
| Namn |Visningsnamn för hello aviseringen. |
| Version |hello API-version som används.  För närvarande är ska detta alltid anges too1. |

### <a name="retrieving-actions"></a>Hämta åtgärder
Använd hello hämta metoden tooretrieve alla åtgärder för ett schema.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Använd hello Get-metod med hello åtgärd ID tooretrieve en viss åtgärd för ett schema.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Skapa eller redigera åtgärder
Använda hello Put-metoden med en åtgärds-ID som är unikt toohello schema toocreate en ny åtgärd.  När du skapar en åtgärd i hello OMS-konsolen är ett GUID för hello åtgärds-ID.

> [!NOTE]
> hello namn för alla sparade sökningar, scheman och åtgärder som skapats med hello Log Analytics API måste vara i gemener.

Med en befintlig åtgärd ID för hello samma sparade söka toomodify som schemalägger hello Put-metoden.  hello brödtexten i begäran hello måste innehålla hello etag hello schema.

hello begäran format för att skapa en ny åtgärd varierar beroende på typen så exemplen finns i hello avsnitten nedan.

### <a name="deleting-actions"></a>Ta bort åtgärder
Använda hello Delete-metoden med hello åtgärd ID toodelete en åtgärd.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Aviseringsåtgärder
Ett schema ska ha en Aviseringsåtgärd.  Aviseringsåtgärder har en eller flera av hello avsnitt i den följande tabellen hello.  Alla beskrivs i mer detalj nedan.

| Avsnittet | Beskrivning |
|:--- |:--- |
| Tröskelvärde |Kriterier för när hello-åtgärden körs. |
| EmailNotification |Skicka e-post toomultiple mottagare. |
| Reparation |Starta en runbook i Azure Automation tooattempt toocorrect identifierade problem. |

#### <a name="thresholds"></a>Tröskelvärden
En åtgärd måste ha ett och endast ett tröskelvärde.  När hello resultaten av en sparad sökning matchar hello tröskelvärdet i en åtgärd som är associerade med sökningen kör några andra processer i åtgärden.  En åtgärd kan också innehålla endast ett tröskelvärde så att den kan användas med åtgärder för andra typer som inte innehåller tröskelvärden.

Tröskelvärden har hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| Operatorn |Operator för hello tröskelvärde jämförelse. <br> gt = större än <br> lt = mindre än |
| Värde |Värdet för hello tröskelvärdet. |

Anta till exempel att en fråga med ett intervall på 15 minuter, Timespan 30 minuter och ett tröskelvärde som är större än 10. I det här fallet hello-frågan skulle köras var 15: e minut och skulle att utlösa en avisering om det returnerade 10 händelser som har skapats under ett 30 minuters intervall.

Följande är ett exempelsvar för en åtgärd med bara ett tröskelvärde.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Använda hello Put-metoden med en unik åtgärd ID toocreate en ny tröskelvärdet åtgärd för ett schema.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Använda hello Put-metoden med en befintlig åtgärd ID toomodify en åtgärd för tröskelvärde för ett schema.  hello brödtexten i begäran hello måste innehålla hello etag hello-åtgärd.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>E-postavisering
E-postaviseringar skicka e-post tooone eller flera mottagare.  De inkluderar hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| mottagare |Lista över e-postadresser. |
| Ämne |hello ämne hello e-post. |
| Bifogad fil |Bifogade filer stöds inte för närvarande, så att det alltid har värdet ”None”. |

Följande är ett exempelsvar för en e-postavisering åtgärd med ett tröskelvärde.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is hello subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Använda hello Put-metoden med en unik åtgärd ID toocreate en ny e-åtgärd för ett schema.  hello följande exempel skapas ett e-postmeddelande med en tröskel så hello e-post skickas när hello resultaten av hello sparad sökning hello överskrids.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

Använda hello Put-metoden med en befintlig åtgärd ID toomodify en e-åtgärd för ett schema.  hello brödtexten i begäran hello måste innehålla hello etag hello-åtgärd.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a>Reparationsåtgärder
Reparationer startar en runbook i Azure Automation som försöker toocorrect hello problem som identifieras av hello avisering.  Du måste skapa en webhook för hello-runbook som används i en Reparationsåtgärd och anger sedan hello URI i hello WebhookUri egenskapen.  När du skapar den här åtgärden med hello OMS-konsolen, skapas automatiskt en ny webhook för hello runbook.

Reparationer inkludera hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| RunbookName |Namnet på hello runbook. Detta måste matcha en publicerad runbook i hello automation-kontot som konfigurerats i hello Automation-lösningen i OMS-arbetsyta. |
| WebhookUri |Hej webhook-URI. |
| Förfallodatum |hello utgångsdatum och utgångstid av hello webhooken.  Om hello webhook inte har ett förfallodatum, kan det vara ett giltigt framtida datum. |

Följande är ett exempelsvar för en Reparationsåtgärd med ett tröskelvärde.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Använda hello Put-metoden med en unik åtgärd ID toocreate nya Reparationsåtgärd för ett schema.  hello följande exempel skapas en med ett tröskelvärde så hello runbook startas när hello resultaten av hello sparad sökning hello överskrids.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Använda hello Put-metoden med en befintlig åtgärd ID toomodify en Reparationsåtgärd för ett schema.  hello brödtexten i begäran hello måste innehålla hello etag hello-åtgärd.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Exempel
Följande är en komplett exempel toocreate en ny e-postavisering.  Detta skapar ett nytt schema tillsammans med en åtgärd som innehåller ett tröskelvärde och e-post.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Webhook-åtgärder
Webhook-åtgärder kan du starta en process genom att anropa en URL och du kan också tillhandahålla en nyttolast toobe skickas.  De är liknande tooRemediation åtgärder förutom de är avsedda för webhooks som kan anropa processer än Azure Automation-runbooks.  De ger också hello ytterligare alternativ för att tillhandahålla en nyttolast toobe levereras toohello fjärrprocess.

Webhook-åtgärder har inte ett tröskelvärde men i stället bör läggas tooa schema som har en åtgärd med ett tröskelvärde.  Du kan lägga till flera Webhook-åtgärder som alla körs när hello tröskelvärdet har uppnåtts.

Webhook-åtgärder inkludera hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| WebhookUri |hello ämne hello e-post. |
| CustomPayload |Anpassad nyttolast toobe skickas toohello webhooken.  hello format beror på vilken hello webhook förväntas. |

Följande är ett exempelsvar för webhook åtgärden och en associerad åtgärd med ett tröskelvärde.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Skapa eller redigera en webhook-åtgärd
Använda hello Put-metoden med en unik åtgärd ID toocreate en ny webhook-åtgärd för ett schema.  hello följande exempel skapas en Webhook-åtgärd och en åtgärd med ett tröskelvärde så att hello webhook ska utlösas när hello resultaten av hello sparad sökning hello överskrids.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Använda hello Put-metoden med en befintlig åtgärd ID toomodify en webhook-åtgärd för ett schema.  hello brödtexten i begäran hello måste innehålla hello etag hello-åtgärd.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Nästa steg
* Använd hello [REST API tooperform loggen sökningar](log-analytics-log-search-api.md) i logganalys.

