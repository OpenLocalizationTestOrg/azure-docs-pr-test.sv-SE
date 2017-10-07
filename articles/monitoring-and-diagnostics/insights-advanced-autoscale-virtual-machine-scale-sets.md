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
# <a name="advanced-autoscale-configuration-using-resource-manager-templates-for-vm-scale-sets"></a><span data-ttu-id="b64ee-103">Avancerade Autoskala konfigurationen med hjälp av Resource Manager-mallar för Skalningsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="b64ee-103">Advanced autoscale configuration using Resource Manager templates for VM Scale Sets</span></span>
<span data-ttu-id="b64ee-104">Du kan skala i och skalbar i Skalningsuppsättningar i virtuella datorer baserat på mått prestandatrösklarna genom ett återkommande schema eller genom att ett visst datum.</span><span class="sxs-lookup"><span data-stu-id="b64ee-104">You can scale-in and scale-out in Virtual Machine Scale Sets based on performance metric thresholds, by a recurring schedule, or by a particular date.</span></span> <span data-ttu-id="b64ee-105">Du kan också konfigurera e-post och webhook-meddelanden för skala åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b64ee-105">You can also configure email and webhook notifications for scale actions.</span></span> <span data-ttu-id="b64ee-106">Den här genomgången visar ett exempel på hur du konfigurerar dessa objekt med en Resource Manager-mall i en Skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="b64ee-106">This walkthrough shows an example of configuring all these objects using a Resource Manager template on a VM Scale Set.</span></span>

> [!NOTE]
> <span data-ttu-id="b64ee-107">När den här genomgången beskriver hello steg för Skalningsuppsättningar, hello samma information gäller tooautoscaling [molntjänster](https://azure.microsoft.com/services/cloud-services/), och [App Service - Webbappar](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="b64ee-107">While this walkthrough explains hello steps for VM Scale Sets, hello same information applies tooautoscaling [Cloud Services](https://azure.microsoft.com/services/cloud-services/), and [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/).</span></span>
> <span data-ttu-id="b64ee-108">För en enkel skala in/ut inställning på en VM Scale Set baserat på ett enkelt prestanda mått t.ex CPU, se toohello [Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) och [Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) dokument</span><span class="sxs-lookup"><span data-stu-id="b64ee-108">For a simple scale in/out setting on a VM Scale Set based on a simple performance metric such as CPU, refer toohello [Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) and [Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) documents</span></span>
>
>

## <a name="walkthrough"></a><span data-ttu-id="b64ee-109">Genomgång</span><span class="sxs-lookup"><span data-stu-id="b64ee-109">Walkthrough</span></span>
<span data-ttu-id="b64ee-110">I den här genomgången ska vi använda [resursutforskaren Azure](https://resources.azure.com/) tooconfigure och uppdatera hello autoskalningsinställning för en skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="b64ee-110">In this walkthrough, we use [Azure Resource Explorer](https://resources.azure.com/) tooconfigure and update hello autoscale setting for a scale set.</span></span> <span data-ttu-id="b64ee-111">Azure Resource Explorer är ett enkelt sätt toomanage Azure-resurser via Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="b64ee-111">Azure Resource Explorer is an easy way toomanage Azure resources via Resource Manager templates.</span></span> <span data-ttu-id="b64ee-112">Om du är ny tooAzure Resursläsaren verktyget läser [här introduktionen](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).</span><span class="sxs-lookup"><span data-stu-id="b64ee-112">If you are new tooAzure Resource Explorer tool, read [this introduction](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).</span></span>

1. <span data-ttu-id="b64ee-113">Distribuera en ny skala med en grundläggande autoskalningsinställning.</span><span class="sxs-lookup"><span data-stu-id="b64ee-113">Deploy a new scale set with a basic autoscale setting.</span></span> <span data-ttu-id="b64ee-114">Den här artikeln använder hello något från hello Azure Snabbstartsgalleriet som har en Windows skaluppsättning med en grundläggande Autoskala mall.</span><span class="sxs-lookup"><span data-stu-id="b64ee-114">This article uses hello one from hello Azure QuickStart Gallery, which has a Windows scale set with a basic autoscale template.</span></span> <span data-ttu-id="b64ee-115">Linux skalningsuppsättningarna arbete hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="b64ee-115">Linux scale sets work hello same way.</span></span>
2. <span data-ttu-id="b64ee-116">När du har skapat hello skaluppsättning navigera toohello scale set resurs från resursutforskaren i Azure.</span><span class="sxs-lookup"><span data-stu-id="b64ee-116">After hello scale set is created, navigate toohello scale set resource from Azure Resource Explorer.</span></span> <span data-ttu-id="b64ee-117">Du kan se hello följande under Microsoft.Insights nod.</span><span class="sxs-lookup"><span data-stu-id="b64ee-117">You see hello following under Microsoft.Insights node.</span></span>

    ![Azure Explorer](./media/insights-advanced-autoscale-vmss/azure_explorer_navigate.png)

    <span data-ttu-id="b64ee-119">hello mallen körningen har skapat en Autoskala standardinställning med hello namnet **'autoscalewad'**.</span><span class="sxs-lookup"><span data-stu-id="b64ee-119">hello template execution has created a default autoscale setting with hello name **'autoscalewad'**.</span></span> <span data-ttu-id="b64ee-120">Du kan visa hello fullständig definitionen av den här autoskalningsinställning hello höger.</span><span class="sxs-lookup"><span data-stu-id="b64ee-120">On hello right-hand side, you can view hello full definition of this autoscale setting.</span></span> <span data-ttu-id="b64ee-121">I det här fallet levereras hello standardinställningen Autoskala med en processor % baserat skalbar och skala i regeln.</span><span class="sxs-lookup"><span data-stu-id="b64ee-121">In this case, hello default autoscale setting comes with a CPU% based scale-out and scale-in rule.</span></span>  

3. <span data-ttu-id="b64ee-122">Du kan nu lägga till fler profiler och regler baserat på schemat för hello eller specifika krav.</span><span class="sxs-lookup"><span data-stu-id="b64ee-122">You can now add more profiles and rules based on hello schedule or specific requirements.</span></span> <span data-ttu-id="b64ee-123">Vi skapa en autoskalningsinställning med tre profiler.</span><span class="sxs-lookup"><span data-stu-id="b64ee-123">We create an autoscale setting with three profiles.</span></span> <span data-ttu-id="b64ee-124">toounderstand profiler och regler i Autoskala, granska [Autoskala metodtips](insights-autoscale-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="b64ee-124">toounderstand profiles and rules in autoscale, review [Autoscale Best Practices](insights-autoscale-best-practices.md).</span></span>  

    | <span data-ttu-id="b64ee-125">Profiler och regler</span><span class="sxs-lookup"><span data-stu-id="b64ee-125">Profiles & Rules</span></span> | <span data-ttu-id="b64ee-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b64ee-126">Description</span></span> |
    |--- | --- |
    | <span data-ttu-id="b64ee-127">**Profil**</span><span class="sxs-lookup"><span data-stu-id="b64ee-127">**Profile**</span></span> |<span data-ttu-id="b64ee-128">**Prestanda/mått baserade**</span><span class="sxs-lookup"><span data-stu-id="b64ee-128">**Performance/metric based**</span></span> |
    | <span data-ttu-id="b64ee-129">Regel</span><span class="sxs-lookup"><span data-stu-id="b64ee-129">Rule</span></span> |<span data-ttu-id="b64ee-130">Service Bus-kön Meddelandemängd > x</span><span class="sxs-lookup"><span data-stu-id="b64ee-130">Service Bus Queue Message Count > x</span></span> |
    | <span data-ttu-id="b64ee-131">Regel</span><span class="sxs-lookup"><span data-stu-id="b64ee-131">Rule</span></span> |<span data-ttu-id="b64ee-132">Service Bus-kön Meddelandemängd < y</span><span class="sxs-lookup"><span data-stu-id="b64ee-132">Service Bus Queue Message Count < y</span></span> |
    | <span data-ttu-id="b64ee-133">Regel</span><span class="sxs-lookup"><span data-stu-id="b64ee-133">Rule</span></span> |<span data-ttu-id="b64ee-134">CPU i % > n</span><span class="sxs-lookup"><span data-stu-id="b64ee-134">CPU% > n</span></span> |
    | <span data-ttu-id="b64ee-135">Regel</span><span class="sxs-lookup"><span data-stu-id="b64ee-135">Rule</span></span> |<span data-ttu-id="b64ee-136">CPU i % < p</span><span class="sxs-lookup"><span data-stu-id="b64ee-136">CPU% < p</span></span> |
    | <span data-ttu-id="b64ee-137">**Profil**</span><span class="sxs-lookup"><span data-stu-id="b64ee-137">**Profile**</span></span> |<span data-ttu-id="b64ee-138">**Veckodag morgon timmar (inga regler)**</span><span class="sxs-lookup"><span data-stu-id="b64ee-138">**Weekday morning hours (no rules)**</span></span> |
    | <span data-ttu-id="b64ee-139">**Profil**</span><span class="sxs-lookup"><span data-stu-id="b64ee-139">**Profile**</span></span> |<span data-ttu-id="b64ee-140">**Produkten starta dag (inga regler)**</span><span class="sxs-lookup"><span data-stu-id="b64ee-140">**Product Launch day (no rules)**</span></span> |

4. <span data-ttu-id="b64ee-141">Här är ett hypotetiskt skalning scenario som vi använder för den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="b64ee-141">Here is a hypothetical scaling scenario that we use for this walk-through.</span></span>

    * <span data-ttu-id="b64ee-142">**Belastningen baserat** -jag vill tooscale in eller ut utifrån hello belastningen på mitt program som finns på min skala set.*</span><span class="sxs-lookup"><span data-stu-id="b64ee-142">**Load based** - I'd like tooscale out or in based on hello load on my application hosted on my scale set.*</span></span>
    * <span data-ttu-id="b64ee-143">**Meddelande köstorlek** -jag använda en Service Bus-kö för hello inkommande meddelanden toomy program.</span><span class="sxs-lookup"><span data-stu-id="b64ee-143">**Message Queue size** - I use a Service Bus Queue for hello incoming messages toomy application.</span></span> <span data-ttu-id="b64ee-144">Jag använder hello kön meddelandemängd och processor och konfigurera en standard profil tootrigger en skalningsåtgärd om antalet meddelanden eller CPU träffar hello threshold.*</span><span class="sxs-lookup"><span data-stu-id="b64ee-144">I use hello queue's message count and CPU% and configure a default profile tootrigger a scale action if either of message count or CPU hits hello threshold.*</span></span>
    * <span data-ttu-id="b64ee-145">**Tid för vecka och dag** -jag vill ha en vecka återkommande 'tid på dagen hello' baserat profil som kallas veckodag morgon-timmar.</span><span class="sxs-lookup"><span data-stu-id="b64ee-145">**Time of week and day** - I want a weekly recurring 'time of hello day' based profile called 'Weekday Morning Hours'.</span></span> <span data-ttu-id="b64ee-146">Baserat på historisk data, vet jag att det är bättre toohave vissa antal Virtuella instanser toohandle min programinläsning under den här time.*</span><span class="sxs-lookup"><span data-stu-id="b64ee-146">Based on historical data, I know it is better toohave certain number of VM instances toohandle my application's load during this time.*</span></span>
    * <span data-ttu-id="b64ee-147">**Datum** -jag har lagt till en profil för produkten starta dag.</span><span class="sxs-lookup"><span data-stu-id="b64ee-147">**Special Dates** - I added a 'Product Launch Day' profile.</span></span> <span data-ttu-id="b64ee-148">Jag planera för specifika datum så min programmet är redo toohandle hello belastningen på grund av marknadsföring meddelanden och när vi placera en ny produkt i hello application.*</span><span class="sxs-lookup"><span data-stu-id="b64ee-148">I plan ahead for specific dates so my application is ready toohandle hello load due marketing announcements and when we put a new product in hello application.*</span></span>
    * <span data-ttu-id="b64ee-149">*hello senast två profiler kan också ha andra prestanda mått baserat på regler i dem. I det här fallet jag valt inte toohave en och i stället toorely på hello standardmått prestanda-baserade regler. Regler är valfria för hello återkommande och datum-baserade profiler.*</span><span class="sxs-lookup"><span data-stu-id="b64ee-149">*hello last two profiles can also have other performance metric based rules within them. In this case, I decided not toohave one and instead toorely on hello default performance metric based rules. Rules are optional for hello recurring and date-based profiles.*</span></span>

    <span data-ttu-id="b64ee-150">Autoskala motorn prioritering av hello profiler och regler fångas även i hello [autoskalning metodtips](insights-autoscale-best-practices.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="b64ee-150">Autoscale engine's prioritization of hello profiles and rules is also captured in hello [autoscaling best practices](insights-autoscale-best-practices.md) article.</span></span>
    <span data-ttu-id="b64ee-151">En lista över vanliga mätvärden för Autoskala [vanliga mätvärden för Autoskala](insights-autoscale-common-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="b64ee-151">For a list of common metrics for autoscale, refer [Common metrics for Autoscale](insights-autoscale-common-metrics.md)</span></span>

5. <span data-ttu-id="b64ee-152">Kontrollera att du är på hello **läsning och skrivning** läge i Resursläsaren</span><span class="sxs-lookup"><span data-stu-id="b64ee-152">Make sure you are on hello **Read/Write** mode in Resource Explorer</span></span>

    ![Autoscalewad autoskalningsinställning som standard](./media/insights-advanced-autoscale-vmss/autoscalewad.png)

6. <span data-ttu-id="b64ee-154">Klicka på Redigera.</span><span class="sxs-lookup"><span data-stu-id="b64ee-154">Click Edit.</span></span> <span data-ttu-id="b64ee-155">**Ersätt** hello profiler element i autoskalningsinställningen med hello följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="b64ee-155">**Replace** hello 'profiles' element in autoscale setting with hello following configuration:</span></span>

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
    <span data-ttu-id="b64ee-157">Fält som stöds och deras värden finns i [Autoskala REST API-dokumentation](https://msdn.microsoft.com/en-us/library/azure/dn931928.aspx).</span><span class="sxs-lookup"><span data-stu-id="b64ee-157">For supported fields and their values, see [Autoscale REST API documentation](https://msdn.microsoft.com/en-us/library/azure/dn931928.aspx).</span></span> <span data-ttu-id="b64ee-158">Autoskalningsinställningen innehåller nu hello tre profiler som tidigare förklarats.</span><span class="sxs-lookup"><span data-stu-id="b64ee-158">Now your autoscale setting contains hello three profiles explained previously.</span></span>

7. <span data-ttu-id="b64ee-159">Slutligen titta på hello Autoskala **meddelande** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b64ee-159">Finally, look at hello Autoscale **notification** section.</span></span> <span data-ttu-id="b64ee-160">Meddelanden om autoskalning kan du toodo tre saker när en skalbar eller i praktiken utlösts har.</span><span class="sxs-lookup"><span data-stu-id="b64ee-160">Autoscale notifications allow you toodo three things when a scale-out or in action is successfully triggered.</span></span>
   - <span data-ttu-id="b64ee-161">Meddela Hej administratör och medadministratörer för din prenumeration</span><span class="sxs-lookup"><span data-stu-id="b64ee-161">Notify hello admin and co-admins of your subscription</span></span>
   - <span data-ttu-id="b64ee-162">Skicka e-post till en uppsättning användare</span><span class="sxs-lookup"><span data-stu-id="b64ee-162">Email a set of users</span></span>
   - <span data-ttu-id="b64ee-163">Utlös ett webhook-anrop.</span><span class="sxs-lookup"><span data-stu-id="b64ee-163">Trigger a webhook call.</span></span> <span data-ttu-id="b64ee-164">När utlöses, denna webhook skickar metadata om hello autoskalning villkoret och hello skaluppsättning för resursen.</span><span class="sxs-lookup"><span data-stu-id="b64ee-164">When fired, this webhook sends metadata about hello autoscaling condition and hello scale set resource.</span></span> <span data-ttu-id="b64ee-165">toolearn mer om hello nyttolast Autoskala webhook finns [konfigurera Webhook & e-postaviseringar för Autoskala](insights-autoscale-to-webhook-email.md).</span><span class="sxs-lookup"><span data-stu-id="b64ee-165">toolearn more about hello payload of autoscale webhook, see [Configure Webhook & Email Notifications for Autoscale](insights-autoscale-to-webhook-email.md).</span></span>

   <span data-ttu-id="b64ee-166">Lägg till följande toohello Autoskala inställningen ersätter hello din **meddelande** element vars värde är null</span><span class="sxs-lookup"><span data-stu-id="b64ee-166">Add hello following toohello Autoscale setting replacing your **notification** element whose value is null</span></span>

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

   <span data-ttu-id="b64ee-167">Träffa **placera** knapp i Resursläsaren tooupdate hello autoskalningsinställning.</span><span class="sxs-lookup"><span data-stu-id="b64ee-167">Hit **Put** button in Resource Explorer tooupdate hello autoscale setting.</span></span>

<span data-ttu-id="b64ee-168">Du har uppdaterat en autoskalningsinställning på en VM Scale set tooinclude flera skala profiler och skala meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b64ee-168">You have updated an autoscale setting on a VM Scale set tooinclude multiple scale profiles and scale notifications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b64ee-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b64ee-169">Next Steps</span></span>
<span data-ttu-id="b64ee-170">Använd dessa länkar toolearn mer om autoskalning.</span><span class="sxs-lookup"><span data-stu-id="b64ee-170">Use these links toolearn more about autoscaling.</span></span>

[<span data-ttu-id="b64ee-171">Felsöka Autoskala med virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="b64ee-171">TroubleShoot Autoscale with Virtual Machine Scale Sets</span></span>](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

[<span data-ttu-id="b64ee-172">Vanliga mätvärden för Autoskala</span><span class="sxs-lookup"><span data-stu-id="b64ee-172">Common Metrics for Autoscale</span></span>](insights-autoscale-common-metrics.md)

[<span data-ttu-id="b64ee-173">Metodtips för Azure Autoskala</span><span class="sxs-lookup"><span data-stu-id="b64ee-173">Best Practices for Azure Autoscale</span></span>](insights-autoscale-best-practices.md)

[<span data-ttu-id="b64ee-174">Hantera Autoskala med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="b64ee-174">Manage Autoscale using PowerShell</span></span>](insights-powershell-samples.md#create-and-manage-autoscale-settings)

[<span data-ttu-id="b64ee-175">Hantera Autoskala med hjälp av CLI</span><span class="sxs-lookup"><span data-stu-id="b64ee-175">Manage Autoscale using CLI</span></span>](insights-cli-samples.md#autoscale)

[<span data-ttu-id="b64ee-176">Konfigurera Webhook & e-postaviseringar för Autoskala</span><span class="sxs-lookup"><span data-stu-id="b64ee-176">Configure Webhook & Email Notifications for Autoscale</span></span>](insights-autoscale-to-webhook-email.md)
