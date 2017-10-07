---
title: aaaAdvanced Autoskala med Azure Virtual Machines | Microsoft Docs
description: "Använder Resource Manager och Skalningsuppsättningar med flera regler och profiler som kan skicka e-post och anropa webhook URL: er med skalningsåtgärder."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 7e3576e2-4a2b-4736-b5ae-98c4689cdd2b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2016
ms.author: ancav
ms.openlocfilehash: ecb01e3094f715837b75ef07a7dbecdf0f2e78f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-autoscale-configuration-using-resource-manager-templates-for-vm-scale-sets"></a>Avancerade Autoskala konfigurationen med hjälp av Resource Manager-mallar för Skalningsuppsättningar
Du kan skala i och skalbar i Skalningsuppsättningar i virtuella datorer baserat på mått prestandatrösklarna genom ett återkommande schema eller genom att ett visst datum. Du kan också konfigurera e-post och webhook-meddelanden för skala åtgärder. Den här genomgången visar ett exempel på hur du konfigurerar dessa objekt med en Resource Manager-mall i en Skaluppsättning.

> [!NOTE]
> När den här genomgången beskriver hello steg för Skalningsuppsättningar, hello samma information gäller tooautoscaling [molntjänster](https://azure.microsoft.com/services/cloud-services/), och [App Service - Webbappar](https://azure.microsoft.com/services/app-service/web/).
> För en enkel skala in/ut inställning på en VM Scale Set baserat på ett enkelt prestanda mått t.ex CPU, se toohello [Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) och [Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) dokument
>
>

## <a name="walkthrough"></a>Genomgång
I den här genomgången ska vi använda [resursutforskaren Azure](https://resources.azure.com/) tooconfigure och uppdatera hello autoskalningsinställning för en skaluppsättning. Azure Resource Explorer är ett enkelt sätt toomanage Azure-resurser via Resource Manager-mallar. Om du är ny tooAzure Resursläsaren verktyget läser [här introduktionen](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).

1. Distribuera en ny skala med en grundläggande autoskalningsinställning. Den här artikeln använder hello något från hello Azure Snabbstartsgalleriet som har en Windows skaluppsättning med en grundläggande Autoskala mall. Linux skalningsuppsättningarna arbete hello samma sätt.
2. När du har skapat hello skaluppsättning navigera toohello scale set resurs från resursutforskaren i Azure. Du kan se hello följande under Microsoft.Insights nod.

    ![Azure Explorer](./media/insights-advanced-autoscale-vmss/azure_explorer_navigate.png)

    hello mallen körningen har skapat en Autoskala standardinställning med hello namnet **'autoscalewad'**. Du kan visa hello fullständig definitionen av den här autoskalningsinställning hello höger. I det här fallet levereras hello standardinställningen Autoskala med en processor % baserat skalbar och skala i regeln.  

3. Du kan nu lägga till fler profiler och regler baserat på schemat för hello eller specifika krav. Vi skapa en autoskalningsinställning med tre profiler. toounderstand profiler och regler i Autoskala, granska [Autoskala metodtips](insights-autoscale-best-practices.md).  

    | Profiler och regler | Beskrivning |
    |--- | --- |
    | **Profil** |**Prestanda/mått baserade** |
    | Regel |Service Bus-kön Meddelandemängd > x |
    | Regel |Service Bus-kön Meddelandemängd < y |
    | Regel |CPU i % > n |
    | Regel |CPU i % < p |
    | **Profil** |**Veckodag morgon timmar (inga regler)** |
    | **Profil** |**Produkten starta dag (inga regler)** |

4. Här är ett hypotetiskt skalning scenario som vi använder för den här genomgången.

    * **Belastningen baserat** -jag vill tooscale in eller ut utifrån hello belastningen på mitt program som finns på min skala set.*
    * **Meddelande köstorlek** -jag använda en Service Bus-kö för hello inkommande meddelanden toomy program. Jag använder hello kön meddelandemängd och processor och konfigurera en standard profil tootrigger en skalningsåtgärd om antalet meddelanden eller CPU träffar hello threshold.*
    * **Tid för vecka och dag** -jag vill ha en vecka återkommande 'tid på dagen hello' baserat profil som kallas veckodag morgon-timmar. Baserat på historisk data, vet jag att det är bättre toohave vissa antal Virtuella instanser toohandle min programinläsning under den här time.*
    * **Datum** -jag har lagt till en profil för produkten starta dag. Jag planera för specifika datum så min programmet är redo toohandle hello belastningen på grund av marknadsföring meddelanden och när vi placera en ny produkt i hello application.*
    * *hello senast två profiler kan också ha andra prestanda mått baserat på regler i dem. I det här fallet jag valt inte toohave en och i stället toorely på hello standardmått prestanda-baserade regler. Regler är valfria för hello återkommande och datum-baserade profiler.*

    Autoskala motorn prioritering av hello profiler och regler fångas även i hello [autoskalning metodtips](insights-autoscale-best-practices.md) artikel.
    En lista över vanliga mätvärden för Autoskala [vanliga mätvärden för Autoskala](insights-autoscale-common-metrics.md)

5. Kontrollera att du är på hello **läsning och skrivning** läge i Resursläsaren

    ![Autoscalewad autoskalningsinställning som standard](./media/insights-advanced-autoscale-vmss/autoscalewad.png)

6. Klicka på Redigera. **Ersätt** hello profiler element i autoskalningsinställningen med hello följande konfiguration:

    ![Profiler](./media/insights-advanced-autoscale-vmss/profiles.png)

    ```
    {
            "name": "Perf_Based_Scale",
            "capacity": {
              "minimum": "2",
              "maximum": "12",
              "default": "2"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 10
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 3
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 85
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 60
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          },
          {
            "name": "Weekday_Morning_Hours_Scale",
            "capacity": {
              "minimum": "4",
              "maximum": "12",
              "default": "4"
            },
            "rules": [],
            "recurrence": {
              "frequency": "Week",
              "schedule": {
                "timeZone": "Pacific Standard Time",
                "days": [
                  "Monday",
                  "Tuesday",
                  "Wednesday",
                  "Thursday",
                  "Friday"
                ],
                "hours": [
                  6
                ],
                "minutes": [
                  0
                ]
              }
            }
          },
          {
            "name": "Product_Launch_Day",
            "capacity": {
              "minimum": "6",
              "maximum": "20",
              "default": "6"
            },
            "rules": [],
            "fixedDate": {
              "timeZone": "Pacific Standard Time",
              "start": "2016-06-20T00:06:00Z",
              "end": "2016-06-21T23:59:00Z"
            }
          }
    ```
    Fält som stöds och deras värden finns i [Autoskala REST API-dokumentation](https://msdn.microsoft.com/en-us/library/azure/dn931928.aspx). Autoskalningsinställningen innehåller nu hello tre profiler som tidigare förklarats.

7. Slutligen titta på hello Autoskala **meddelande** avsnitt. Meddelanden om autoskalning kan du toodo tre saker när en skalbar eller i praktiken utlösts har.
   - Meddela Hej administratör och medadministratörer för din prenumeration
   - Skicka e-post till en uppsättning användare
   - Utlös ett webhook-anrop. När utlöses, denna webhook skickar metadata om hello autoskalning villkoret och hello skaluppsättning för resursen. toolearn mer om hello nyttolast Autoskala webhook finns [konfigurera Webhook & e-postaviseringar för Autoskala](insights-autoscale-to-webhook-email.md).

   Lägg till följande toohello Autoskala inställningen ersätter hello din **meddelande** element vars värde är null

   ```
   "notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": true,
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

   Träffa **placera** knapp i Resursläsaren tooupdate hello autoskalningsinställning.

Du har uppdaterat en autoskalningsinställning på en VM Scale set tooinclude flera skala profiler och skala meddelanden.

## <a name="next-steps"></a>Nästa steg
Använd dessa länkar toolearn mer om autoskalning.

[Felsöka Autoskala med virtuella datorer](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

[Vanliga mätvärden för Autoskala](insights-autoscale-common-metrics.md)

[Metodtips för Azure Autoskala](insights-autoscale-best-practices.md)

[Hantera Autoskala med hjälp av PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)

[Hantera Autoskala med hjälp av CLI](insights-cli-samples.md#autoscale)

[Konfigurera Webhook & e-postaviseringar för Autoskala](insights-autoscale-to-webhook-email.md)
