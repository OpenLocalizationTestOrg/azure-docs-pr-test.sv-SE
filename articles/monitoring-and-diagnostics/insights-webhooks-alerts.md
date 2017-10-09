---
title: "aaaConfigure webhooks på Azure mått aviseringar | Microsoft Docs"
description: "Ändra Azure aviseringar tooother Azure-system."
author: johnkemnetz
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 8b3ae540-1d19-4f3d-a635-376042f8a5bb
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: johnkem
ms.openlocfilehash: bc4153ccdcff41c5b9d3c081e59a1bf260d8a283
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Konfigurera en webhook på en Azure mått avisering
Webhooks Tillåt tooroute en Azure Varna tooother meddelandesystem för efterbearbetning eller anpassade åtgärder. Du kan använda en webhook på en avisering tooroute den tooservices som skickar SMS, logga programfel, meddela ett team via chatt/meddelandetjänster eller göra en mängd olika andra åtgärder. Den här artikeln beskriver hur tooset en webhook på en Azure mått avisering och vilka hello nyttolasten för hello HTTP POST tooa webhook ser ut. Information om hello installationen och schemat för en Azure-aktivitetsloggen varning (varning på händelser) [se den här sidan i stället](insights-auditlog-to-webhook-email.md).

Azure aviseringar HTTP POST hello avisering innehållet i JSON-format, schema som definierats under tooa webhook URI som du anger när du skapar hello avisering. Den här URI måste vara en giltig HTTP eller HTTPS-slutpunkt. Azure skickar en post per begäran när en avisering aktiveras.

## <a name="configuring-webhooks-via-hello-portal"></a>Hur du konfigurerar webhooks via hello-portalen
Du kan lägga till eller uppdatera hello webhook URI i hello skapa/uppdatera aviseringar skärm i hello [portal](https://portal.azure.com/).

![Lägg till en varningsregel](./media/insights-webhooks-alerts/Alertwebhook.png)

Du kan också konfigurera en varning toopost tooa webhook URI med hello [Azure PowerShell-Cmdlets](insights-powershell-samples.md#create-metric-alerts), [plattformsoberoende CLI](insights-cli-samples.md#work-with-alerts), eller [REST-API för Azure-Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-hello-webhook"></a>Autentisera hello-webhook
Hej webhook kan autentisera med hjälp av tokenbaserad auktorisering. Hej webhook URI sparas med ett token ID, t.ex. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Nyttolasten i schemat
hello POST-åtgärden innehåller hello följande JSON-nyttolast och schemat för alla mått baserade aviseringar.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Fält | Obligatorisk | Fast uppsättning värden | Anteckningar |
|:--- |:--- |:--- |:--- |
| status |Y |”Aktiverad”, ”löst” |Status för hello aviseringen baserad på hello villkor du har angett. |
| Kontexten |Y | |Hej aviseringssammanhanget. |
| tidsstämpel |Y | |hello tid vid vilken hello aviseringen utlöstes. |
| id |Y | |Varje regel för varning har ett unikt id. |
| namn |Y | |namn på hello avisering. |
| description |Y | |Beskrivning av hello avisering. |
| conditionType |Y |”Mått”, ”Event” |Två typer av aviseringar stöds. En baserat på ett mått villkor och hello andra baserat på en händelse i hello aktivitetsloggen. Använd det här värdet toocheck om hello avisering baseras på mått eller händelse. |
| Villkor |Y | |hello specifika fält toocheck för baserat på hello conditionType. |
| metricName |för mått aviseringar | |hello namnet på hello mått som definierar vilka hello regeln övervakar. |
| metricUnit |för mått aviseringar |”Byte”, ”BytesPerSecond”, ”antal”, ”CountPerSecond”, ”procent”, ”sekunder” |hello-enhet är tillåtna i hello mått. [Tillåtna värden i den här listan](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |för mått aviseringar | |hello faktiskt värde för hello mått som orsakade aviseringen hello. |
| Tröskelvärde |för mått aviseringar | |hello tröskelvärdet på vilka hello aviseringen har aktiverats. |
| Fönsterstorlek |för mått aviseringar | |hello tidsperiod som används toomonitor avisering aktivitet utifrån hello tröskelvärdet. Måste vara mellan 5 minuter och 1 dag. Varaktighet i ISO 8601-format. |
| timeAggregation |för mått aviseringar |”Medel”, ”senaste”, ”högsta”, ”Minimum” och ”None”, ”totalt” |Hur hello data som samlas in ska kombineras med tiden. hello standardvärdet är medelvärdet. [Tillåtna värden i den här listan](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| Operatorn |för mått aviseringar | |hello operatorn användas toocompare hello aktuella mätvärden toohello angiven tröskel. |
| subscriptionId |Y | |Azure prenumerations-ID. |
| resourceGroupName |Y | |Namnet på resursgruppen hello för hello påverkas resurs. |
| resourceName |Y | |Resursnamn av hello påverkas resurs. |
| resourceType |Y | |Resurstypen för hello påverkas resurs. |
| resourceId |Y | |Resurs-ID för hello påverkas resurs. |
| resourceRegion |Y | |Region eller platsen för hello påverkas resurs. |
| portalLink |Y | |Direktlänk toohello portal resurs sammanfattningssidan. |
| properties |N |Valfri |En uppsättning `<Key, Value>` par (d.v.s. `Dictionary<String, String>`) som innehåller information om hello-händelse. hello egenskapsfältet är valfritt. Användare kan ett anpassat användargränssnitt eller logik app-baserat arbetsflöde ange nyckel/värden som kan skickas via hello nyttolast. hello alternativa sätt toopass anpassade egenskaper tillbaka toohello webhook är via hello webhook URI: n sig själv (som frågeparametrar) |

> [!NOTE]
> hello egenskapsfält kan endast anges med hello [REST-API för Azure-Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Nästa steg
* Läs mer om Azure-aviseringar och webhooks i hello videon [integrera Azure aviseringar med PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
* [Köra Azure Automation-skript (Runbooks) på Azure-aviseringar](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Använd Logikapp toosend ett SMS via Twilio från en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
* [Använd Logikapp toosend ett Slack meddelande från en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
* [Använd Logikapp toosend meddelandet tooan Azure Queue från en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
