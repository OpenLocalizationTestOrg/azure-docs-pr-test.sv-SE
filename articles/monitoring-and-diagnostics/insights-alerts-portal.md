---
title: "aaaCreate aviseringar för Azure-tjänster - Azure-portalen | Microsoft Docs"
description: "Utlösa e-postmeddelanden, meddelanden, anrop webbplatser-URL: er (webhooks) eller automation när hello villkor är uppfyllda."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2016
ms.author: robb
ms.openlocfilehash: 78d862d25255cda9fdfe347329e908a471c39846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a>Skapa mått aviseringar i Azure-Monitor för Azure-tjänster - Azure-portalen
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Översikt
Den här artikeln visar hur tooset in Azure mått aviseringar med hello Azure-portalen.   

Du kan ta emot en avisering baserat på övervakning mätvärden för eller händelser på Azure-tjänster.

* **Måttvärden** - hello meddela utlösare när hello-värdet för ett visst mått överskrider ett tröskelvärde som du tilldelar i båda riktningarna. Det vill säga den utlöser både när hello villkor uppfylls först och sedan efteråt när villkor som inte längre är uppfyllt.    
* **Aktiviteten logghändelser** -utlösa en avisering på *varje* händelse eller bara när en vissa händelser inträffar. Mer om aktiviteten loggen aviseringar toolearn [Klicka här](monitoring-activity-log-alerts.md)

Du kan konfigurera en mått avisering toodo hello efter när den utlöser:

* Skicka e-postaviseringar toohello tjänstadministratören och medadministratörer
* Skicka e-post tooadditional e-postmeddelanden som du anger.
* anropa en webhook
* Starta körning av en Azure-runbook (endast från hello Azure-portalen)

Du kan konfigurera och få information om mått Varningsregler med

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [kommandoradsgränssnittet (CLI)](insights-alerts-command-line-interface.md)
* [Azure-Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-hello-azure-portal"></a>Skapa en aviseringsregel på ett mått med hello Azure-portalen
1. I hello [portal](https://portal.azure.com/)letar du upp hello-resurs som du är intresserad av övervakning och markera den.

2. Välj **aviseringar** eller **Varna regler** under hello övervakning avsnitt. hello text och ikon kan variera något mellan olika resurser.  

    ![Övervakning](./media/insights-alerts-portal/AlertRulesButton.png)

3. Välj hello **Lägg till avisering** kommandot och hello fält fylls i.

    ![Lägg till avisering](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Namnet** aviseringen regel och väljer en **beskrivning**, som visar även i e-postmeddelanden.

5. Välj hello **mått** du vill toomonitor och välj sedan en **villkoret** och **tröskelvärdet** värde för hello mått. Valde också hello **Period** som hello mått regeln måste uppfyllas innan hello avisering utlösare. Om du använder hello period ”PT5M” och aviseringen söker efter CPU över 80 procent, startar exempelvis hello avisering när hello CPU har konsekvent ovan 80% i 5 minuter. När hello första utlösaren infaller utlöses igen när hello CPU är mindre än 80% i 5 minuter. hello CPU mätning inträffar var 1 minut.   

6. Kontrollera **e-ägare...**  om du vill att administratörer och medadministratörer toobe e-post när hello avisering utlöses.

7. Om du vill veta e-postmeddelanden tooreceive ett meddelande när hello varning utlöses, lägga till dem i hello **ytterligare administratören email(s)** fältet. Avgränsa flera e-postmeddelanden med semikolon -  *email@contoso.com;email2@contoso.com*

8. Placera i en giltig URI i hello **Webhook** fält om du vill anropa när hello avisering utlöses.

9. Om du använder Azure Automation kan välja du en Runbook toobe körs när hello avisering utlöses.

10. Välj **OK** när klart toocreate hello avisering.   

Inom några minuter hello aviseringen är aktiv och utlöser som beskrivits tidigare.

## <a name="managing-your-alerts"></a>Hantera aviseringar
När du har skapat en avisering, kan du välja den och:

* Visa ett diagram som visar hello mått tröskelvärde och hello faktiska värden från hello föregående dag.
* Redigera eller ta bort den.
* **Inaktivera** eller **aktivera** den om du vill stoppa tootemporarily eller återuppta tar emot meddelanden om den här aviseringen.

## <a name="next-steps"></a>Nästa steg
* [Få en översikt över Azure övervakning](monitoring-overview.md) inklusive hello typer av information som du kan samla in och övervaka.
* Lär dig mer om [hur du konfigurerar webhooks i aviseringar](insights-webhooks-alerts.md).
* Lär dig mer om [konfigurera aviseringar på aktiviteten logghändelser](monitoring-activity-log-alerts.md).
* Lär dig mer om [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).
* Hämta en [översikt över diagnostikloggar](monitoring-overview-of-diagnostic-logs.md) och samla in detaljerade hög frekvens mått på din tjänst.
* Hämta en [översikt över mått samling](insights-how-to-customize-monitoring.md) toomake att tjänsten är tillgänglig och svarstid.
