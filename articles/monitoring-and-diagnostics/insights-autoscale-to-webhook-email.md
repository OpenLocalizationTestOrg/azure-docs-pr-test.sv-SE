---
title: "aaaUse Autoskala åtgärder toosend e-post och webhook varningsmeddelanden. | Microsoft Docs"
description: "Se hur toouse Autoskala åtgärder toocall Webb-URL: er eller skicka e-postmeddelanden i Azure-Monitor. "
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: eb9a4c98-0894-488c-8ee8-5df0065d094f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: ancav
ms.openlocfilehash: f611a18f5a808412fbdd0c89e3addb36437064c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-autoscale-actions-toosend-email-and-webhook-alert-notifications-in-azure-monitor"></a>Använda automatiska åtgärder toosend e-post och webhook aviseringar i Azure-Monitor
Den här artikeln lär du hur du konfigurerar utlösare så att du kan anropa specifika webbadresser eller skicka e-postmeddelanden baserat på Autoskala åtgärder i Azure.  

## <a name="webhooks"></a>Webhooks
Webhooks Tillåt tooroute hello Azure varningsmeddelanden tooother system för efterbearbetning eller anpassade meddelanden. Till exempel routning hello avisering tooservices som kan hantera en inkommande web begäran toosend SMS, log buggar, meddela ett team med hjälp av chatt eller tjänster, etc. hello webhook URI måste vara en giltig HTTP eller HTTPS-slutpunkt.

## <a name="email"></a>E-post
E-post kan skickas tooany giltig e-postadress. Administratörer och medadministratörer för hello prenumeration där hello regeln körs meddelas också.

## <a name="cloud-services-and-web-apps"></a>Molntjänster och Web Apps
Du kan anmäla sig från hello Azure-portalen för molntjänster och servergrupper (webbprogram).

* Välj hello **skala genom** mått.

![skala med](./media/insights-autoscale-to-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a>Skaluppsättningar för den virtuella datorn
För nya virtuella datorer skapas med Resource Manager (skalningsuppsättningar i virtuella datorer), kan du konfigurera detta med hjälp av REST-API, Resource Manager-mallar, PowerShell och CLI. Ett gränssnitt för portalen är inte tillgänglig ännu.
När du använder hello REST API eller Resource Manager-mall, inkluderar du hello meddelanden element med hello följande alternativ.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
| Fält | Obligatorisk? | Beskrivning |
| --- | --- | --- |
| åtgärden |Ja |Värdet måste vara ”skala” |
| sendToSubscriptionAdministrator |Ja |Värdet måste vara ”sant” eller ”false” |
| sendToSubscriptionCoAdministrators |Ja |Värdet måste vara ”sant” eller ”false” |
| customEmails |Ja |Värdet kan vara null [] eller Strängmatrisen för e-post |
| Webhooks |Ja |Värdet kan vara null eller giltiga Uri |
| serviceUri |Ja |en giltig https Uri |
| properties |Ja |Värdet måste vara tom {} eller kan innehålla nyckel / värde-par |

## <a name="authentication-in-webhooks"></a>Autentisering i webhooks
Hej webhook kan autentisera med tokenbaserad autentisering var du sparar hello webhook URI med ett token-ID som en frågeparameter. Till exempel https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue

## <a name="autoscale-notification-webhook-payload-schema"></a>Autoskala meddelande webhook nyttolast schemat
När hello Autoskala avisering genereras ingår hello följande metadata i hello webhook nyttolast:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' toocapacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


| Fält | Obligatorisk? | Beskrivning |
| --- | --- | --- |
| status |Ja |hello status som anger att en Autoskala åtgärd har skapats |
| åtgärden |Ja |För en ökning av instanser blir ”skala ut” och för en minskning av instanserna blir ”skala i” |
| Kontexten |Ja |hello Autoskala åtgärd kontext |
| tidsstämpel |Ja |Tidsstämpel när hello Autoskala åtgärd utlöstes |
| id |Ja |Resource Manager-ID för hello autoskalningsinställning |
| namn |Ja |hello namnet på hello autoskalningsinställning |
| Information |Ja |Förklaring av hello-åtgärd som hello Autoskala tjänsten tog och hello ändra i hello-instanser |
| subscriptionId |Ja |Prenumerations-ID för hello målresurs som som skalas |
| resourceGroupName |Ja |Resursgruppens namn av hello målresurs som som skalas |
| resourceName |Ja |Namnet på hello målresurs som som skalas |
| resourceType |Ja |hello stöds tre värden: ”microsoft.classiccompute/domainnames/slots/roles” - Molntjänsten roller, ”microsoft.compute/virtualmachinescalesets” - Skalningsuppsättningar i virtuella datorer, och ”Microsoft.Web/serverfarms” - webbprogram |
| resourceId |Ja |Resource Manager-ID för hello målresurs som som skalas |
| portalLink |Ja |Azure portal-länk toohello sammanfattningssidan hello målresurs |
| oldCapacity |Ja |hello aktuella (gamla) instansantalet när Autoskala tog en skalningsåtgärd |
| newCapacity |Ja |hello nya instanser Autoskala skalas hello resurs för|
| Egenskaper |Nej |Valfri. Ange < nyckel, värde > par (till exempel ordlista < String, String >). hello egenskapsfältet är valfritt. Du kan ange nycklar och värden som kan skickas med hello nyttolast i en anpassad användargränssnittet eller logik app baserat arbetsflöde. Ett annat sätt toopass anpassade egenskaper tillbaka toohello utgående webhook anrop är toouse hello webhook URI (som frågeparametrar) |
