---
title: "aaaCreate aviseringar för Azure-tjänster - PowerShell | Microsoft Docs"
description: "Utlösa e-postmeddelanden, meddelanden, anrop webbplatser-URL: er (webhooks) eller automation när hello villkor är uppfyllda."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d26ab15b-7b7e-42a9-81c8-3ce9ead5d252
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: robb
ms.openlocfilehash: 80d3a3f194fc6a5a09a81d04206ea7a1640bddb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---powershell"></a>Skapa mått aviseringar i Azure-Monitor för Azure-tjänster - PowerShell
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Översikt
Den här artikeln visar hur tooset upp Azure mått aviseringar med hjälp av PowerShell.  

Du kan ta emot en avisering baserat på övervakning mätvärden för eller händelser på Azure-tjänster.

* **Måttvärden** - hello meddela utlösare när hello-värdet för ett visst mått överskrider ett tröskelvärde som du tilldelar i båda riktningarna. Det vill säga den utlöser både när hello villkor uppfylls först och sedan efteråt när villkor som inte längre är uppfyllt.    
* **Aktiviteten logghändelser** -utlösa en avisering på *varje* händelse eller bara när en vissa händelser inträffar. Mer om aktiviteten loggen aviseringar toolearn [Klicka här](monitoring-activity-log-alerts.md)

Du kan konfigurera en mått avisering toodo hello efter när den utlöser:

* Skicka e-postaviseringar toohello tjänstadministratören och medadministratörer
* Skicka e-post tooadditional e-postmeddelanden som du anger.
* anropa en webhook
* Starta körning av en Azure-runbook (endast från hello Azure-portalen)

Du kan konfigurera och få information om aviseringen regler med hjälp av

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [kommandoradsgränssnittet (CLI)](insights-alerts-command-line-interface.md)
* [Azure-Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

För ytterligare information, kan du alltid skriva ```Get-Help``` och hello PowerShell-kommando som du vill ha hjälp med.

## <a name="create-alert-rules-in-powershell"></a>Skapa Varningsregler i PowerShell
1. Logga in tooAzure.   

    ```PowerShell
    Login-AzureRmAccount

    ```
2. Hämta en lista över hello prenumerationer som finns tillgängliga. Kontrollera att du arbetar med rätt hello-prenumeration. Om inte, ange den toohello höger en med hjälp av hello utdata från `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. toolist befintliga regler på en resursgrupp, Använd hello följande kommando:

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. toocreate en regel måste toohave flera viktiga uppgifter för först.

  * Hej **resurs-ID** hello resursen du vill tooset en avisering om
  * Hej **måttdefinitioner** tillgänglig för den här resursen

     Enkelriktade tooget hello resurs-ID är toouse hello Azure-portalen. Under förutsättning att hello resursen har redan skapats, väljer du den i hello-portalen. Markera i hello nästa bladet *egenskaper* under hello *inställningar* avsnitt. **RESURS-ID** är ett fält i nästa hello-bladet. Ett annat sätt är toouse hello [resursutforskaren Azure](https://resources.azure.com/).

     Är ett exempel resurs-ID för en webbapp

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Du kan använda `Get-AzureRmMetricDefinition` tooview hello lista över alla måttdefinitioner för en viss resurs.

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     hello skapar följande exempel en tabell med hello mått namn och hello enhet för att mått.

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     En fullständig lista över tillgängliga alternativ för Get-AzureRmMetricDefinition är tillgänglig genom att köra Get-MetricDefinitions.
5. hello följande exempel ställer in en avisering för en resurs för webbplatsen. hello avisering utlösare när den får konsekvent all trafik för 5 minuter och igen när den tar emot någon trafik i 5 minuter.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. toocreate webhook eller skicka e-post när en avisering utlöser först skapa hello e-post och/eller webhooks. Direkt skapa hello regeln efteråt med hello - åtgärder taggen och som visas i följande exempel hello. Du kan inte associera webhook eller e-post med redan skapat regler via PowerShell.

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. tooverify att aviseringar har skapats korrekt genom att titta på hello enskilda regler.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. Ta bort aviseringarna. Dessa kommandon för att ta bort hello regler som har skapats tidigare i den här artikeln.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Nästa steg
* [Få en översikt över Azure övervakning](monitoring-overview.md) inklusive hello typer av information som du kan samla in och övervaka.
* Lär dig mer om [hur du konfigurerar webhooks i aviseringar](insights-webhooks-alerts.md).
* Lär dig mer om [konfigurera aviseringar på aktiviteten logghändelser](monitoring-activity-log-alerts.md).
* Lär dig mer om [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).
* Hämta en [översikt över att samla in diagnostikloggar](monitoring-overview-of-diagnostic-logs.md) toocollect detaljerad hög frekvens mått på din tjänst.
* Hämta en [översikt över mått samling](insights-how-to-customize-monitoring.md) toomake att tjänsten är tillgänglig och svarstid.
