---
title: "aaaCreate aviseringar för Azure-tjänster - plattformar CLI | Microsoft Docs"
description: "Utlösa e-postmeddelanden, meddelanden, anrop webbplatser-URL: er (webhooks) eller automation när hello villkor är uppfyllda."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 5c6a2d27-7dcc-4f89-8752-9bb31b05ff35
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: robb
ms.openlocfilehash: e53701e5377a415038a69fbd32f1e5fc5fe99be9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---cross-platform-cli"></a>Skapa mått aviseringar i Azure-Monitor för Azure-tjänster - plattformar CLI
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Översikt
Den här artikeln visar hur tooset in Azure mått aviseringar med hello plattformsoberoende kommandoradsgränssnittet (CLI).

> [!NOTE]
> Azure övervakaren är hello nytt namn för vad anropades ”Azure Insights” förrän den 25 september 2016. Dock innehåller hello namnområden och därmed hello-kommandona nedan fortfarande insikter ”hello”.
>
>

Du kan ta emot en avisering baserat på övervakning mätvärden för eller händelser på Azure-tjänster.

* **Måttvärden** - hello meddela utlösare när hello-värdet för ett visst mått överskrider ett tröskelvärde som du tilldelar i båda riktningarna. Det vill säga den utlöser både när hello villkor uppfylls först och sedan efteråt när villkor som inte längre är uppfyllt.    
* **Aktiviteten logghändelser** -utlösa en avisering på *varje* händelse eller bara när en vissa händelser inträffar. Mer om aktiviteten loggen aviseringar toolearn [Klicka här](monitoring-activity-log-alerts.md)

Du kan konfigurera en mått avisering toodo hello efter när den utlöser:

* Skicka e-postaviseringar toohello tjänstadministratören och medadministratörer
* Skicka e-post tooadditional e-postmeddelanden som du anger.
* anropa en webhook
* Starta körning av en Azure-runbook (endast från hello Azure-portalen just nu)

Du kan konfigurera och få information om mått Varningsregler med

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [kommandoradsgränssnittet (CLI)](insights-alerts-command-line-interface.md)
* [Azure-Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Du kan alltid få hjälp för kommandon genom att skriva ett kommando och tas - hjälp hello slutet. Exempel:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-hello-cli"></a>Skapa Varningsregler med hello CLI
1. Utför hello krav och inloggningen tooAzure. Se [Azure övervakaren CLI exempel](insights-cli-samples.md). Kort sagt: Installera hello CLI och köra dessa kommandon. De får du loggade in, visa vilken prenumeration som du använder och förbereda toorun Azure-Monitor-kommandon.

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. toolist befintliga regler på en resursgrupp, använda hello efter formuläret **azure insikter aviseringar regel listan** *[alternativ] &lt;resourceGroup&gt;*

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. toocreate en regel måste toohave flera viktiga uppgifter för först.
  * Hej **resurs-ID** hello resursen du vill tooset en avisering om
  * Hej **måttdefinitioner** tillgänglig för den här resursen

     Enkelriktade tooget hello resurs-ID är toouse hello Azure-portalen. Under förutsättning att hello resursen har redan skapats, väljer du den i hello-portalen. Markera i hello nästa bladet *egenskaper* under hello *inställningar* avsnitt. Hej *resurs-ID* är ett fält i nästa hello-bladet. Ett annat sätt är toouse hello [resursutforskaren Azure](https://resources.azure.com/).

     Är ett exempel resurs-id för en webbapp

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     tooget en lista över tillgängliga mått för hello och enheter för de mätvärdena som exempelvis hello tidigare resurs Använd hello följande CLI-kommando:  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     *PT1M* är hello Granulariteten för hello tillgängliga mått (1 minut). Olika granulariteter får du använder olika alternativ för mått.
4. toocreate ett mått baserat varningsregeln, kommandot hello följande format:

    **Azure insikter aviseringar regeluppsättning mått** *[alternativ] &lt;ruleName&gt; &lt;plats&gt; &lt;resourceGroup&gt; &lt;fönsterstorlek&gt; &lt;operatorn&gt; &lt;tröskelvärdet&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*

    hello följande exempel ställer in en avisering för en resurs för webbplatsen. hello avisering utlösare när den får konsekvent all trafik för 5 minuter och igen när den tar emot någon trafik i 5 minuter.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. toocreate webhook eller skicka e-post när en avisering om mått utlöses först skapa hello e-post och/eller webhooks. Skapa sedan hello regel omedelbart efteråt. Du kan inte associera webhook eller e-post med redan skapat regler med hjälp av hello CLI.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. Du kan kontrollera att aviseringar har skapats korrekt genom att titta på en enskild regel.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. toodelete regler kommandot hello formuläret:

    **insikter aviseringar regel delete** [alternativ] &lt;resourceGroup&gt; &lt;ruleName&gt;

    Dessa kommandon för att ta bort hello regler som har skapats tidigare i den här artikeln.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a>Nästa steg
* [Få en översikt över Azure övervakning](monitoring-overview.md) inklusive hello typer av information som du kan samla in och övervaka.
* Lär dig mer om [hur du konfigurerar webhooks i aviseringar](insights-webhooks-alerts.md).
* Lär dig mer om [konfigurera aviseringar på aktiviteten logghändelser](monitoring-activity-log-alerts.md).
* Lär dig mer om [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).
* Hämta en [översikt över att samla in diagnostikloggar](monitoring-overview-of-diagnostic-logs.md) toocollect detaljerad hög frekvens mått på din tjänst.
* Hämta en [översikt över mått samling](insights-how-to-customize-monitoring.md) toomake att tjänsten är tillgänglig och svarstid.
