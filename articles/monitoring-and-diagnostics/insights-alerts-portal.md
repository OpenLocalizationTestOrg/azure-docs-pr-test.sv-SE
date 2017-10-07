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
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a><span data-ttu-id="e572b-103">Skapa mått aviseringar i Azure-Monitor för Azure-tjänster - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e572b-103">Create metric alerts in Azure Monitor for Azure services - Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e572b-104">Portal</span><span class="sxs-lookup"><span data-stu-id="e572b-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="e572b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e572b-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="e572b-106">CLI</span><span class="sxs-lookup"><span data-stu-id="e572b-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="e572b-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="e572b-107">Overview</span></span>
<span data-ttu-id="e572b-108">Den här artikeln visar hur tooset in Azure mått aviseringar med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e572b-108">This article shows you how tooset up Azure metric alerts using hello Azure portal.</span></span>   

<span data-ttu-id="e572b-109">Du kan ta emot en avisering baserat på övervakning mätvärden för eller händelser på Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="e572b-109">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="e572b-110">**Måttvärden** - hello meddela utlösare när hello-värdet för ett visst mått överskrider ett tröskelvärde som du tilldelar i båda riktningarna.</span><span class="sxs-lookup"><span data-stu-id="e572b-110">**Metric values** - hello alert triggers when hello value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="e572b-111">Det vill säga den utlöser både när hello villkor uppfylls först och sedan efteråt när villkor som inte längre är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="e572b-111">That is, it triggers both when hello condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="e572b-112">**Aktiviteten logghändelser** -utlösa en avisering på *varje* händelse eller bara när en vissa händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="e572b-112">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="e572b-113">Mer om aktiviteten loggen aviseringar toolearn [Klicka här](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="e572b-113">toolearn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="e572b-114">Du kan konfigurera en mått avisering toodo hello efter när den utlöser:</span><span class="sxs-lookup"><span data-stu-id="e572b-114">You can configure a metric alert toodo hello following when it triggers:</span></span>

* <span data-ttu-id="e572b-115">Skicka e-postaviseringar toohello tjänstadministratören och medadministratörer</span><span class="sxs-lookup"><span data-stu-id="e572b-115">send email notifications toohello service administrator and co-administrators</span></span>
* <span data-ttu-id="e572b-116">Skicka e-post tooadditional e-postmeddelanden som du anger.</span><span class="sxs-lookup"><span data-stu-id="e572b-116">send email tooadditional emails that you specify.</span></span>
* <span data-ttu-id="e572b-117">anropa en webhook</span><span class="sxs-lookup"><span data-stu-id="e572b-117">call a webhook</span></span>
* <span data-ttu-id="e572b-118">Starta körning av en Azure-runbook (endast från hello Azure-portalen)</span><span class="sxs-lookup"><span data-stu-id="e572b-118">start execution of an Azure runbook (only from hello Azure portal)</span></span>

<span data-ttu-id="e572b-119">Du kan konfigurera och få information om mått Varningsregler med</span><span class="sxs-lookup"><span data-stu-id="e572b-119">You can configure and get information about metric alert rules using</span></span>

* [<span data-ttu-id="e572b-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e572b-120">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="e572b-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e572b-121">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="e572b-122">kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="e572b-122">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="e572b-123">Azure-Monitor REST API</span><span class="sxs-lookup"><span data-stu-id="e572b-123">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-hello-azure-portal"></a><span data-ttu-id="e572b-124">Skapa en aviseringsregel på ett mått med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e572b-124">Create an alert rule on a metric with hello Azure portal</span></span>
1. <span data-ttu-id="e572b-125">I hello [portal](https://portal.azure.com/)letar du upp hello-resurs som du är intresserad av övervakning och markera den.</span><span class="sxs-lookup"><span data-stu-id="e572b-125">In hello [portal](https://portal.azure.com/), locate hello resource you are interested in monitoring and select it.</span></span>

2. <span data-ttu-id="e572b-126">Välj **aviseringar** eller **Varna regler** under hello övervakning avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e572b-126">Select **Alerts** or **Alert rules** under hello MONITORING section.</span></span> <span data-ttu-id="e572b-127">hello text och ikon kan variera något mellan olika resurser.</span><span class="sxs-lookup"><span data-stu-id="e572b-127">hello text and icon may vary slightly for different resources.</span></span>  

    ![Övervakning](./media/insights-alerts-portal/AlertRulesButton.png)

3. <span data-ttu-id="e572b-129">Välj hello **Lägg till avisering** kommandot och hello fält fylls i.</span><span class="sxs-lookup"><span data-stu-id="e572b-129">Select hello **Add alert** command and fill in hello fields.</span></span>

    ![Lägg till avisering](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. <span data-ttu-id="e572b-131">**Namnet** aviseringen regel och väljer en **beskrivning**, som visar även i e-postmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="e572b-131">**Name** your alert rule, and choose a **Description**, which also shows in notification emails.</span></span>

5. <span data-ttu-id="e572b-132">Välj hello **mått** du vill toomonitor och välj sedan en **villkoret** och **tröskelvärdet** värde för hello mått.</span><span class="sxs-lookup"><span data-stu-id="e572b-132">Select hello **Metric** you want toomonitor, then choose a **Condition** and **Threshold** value for hello metric.</span></span> <span data-ttu-id="e572b-133">Valde också hello **Period** som hello mått regeln måste uppfyllas innan hello avisering utlösare.</span><span class="sxs-lookup"><span data-stu-id="e572b-133">Also chose hello **Period** of time that hello metric rule must be satisfied before hello alert triggers.</span></span> <span data-ttu-id="e572b-134">Om du använder hello period ”PT5M” och aviseringen söker efter CPU över 80 procent, startar exempelvis hello avisering när hello CPU har konsekvent ovan 80% i 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="e572b-134">So for example, if you use hello period "PT5M" and your alert looks for CPU above 80%, hello alert triggers when hello CPU has been consistently above 80% for 5 minutes.</span></span> <span data-ttu-id="e572b-135">När hello första utlösaren infaller utlöses igen när hello CPU är mindre än 80% i 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="e572b-135">Once hello first trigger occurs, it again triggers when hello CPU stays below 80% for 5 minutes.</span></span> <span data-ttu-id="e572b-136">hello CPU mätning inträffar var 1 minut.</span><span class="sxs-lookup"><span data-stu-id="e572b-136">hello CPU measurement occurs every 1 minute.</span></span>   

6. <span data-ttu-id="e572b-137">Kontrollera **e-ägare...**  om du vill att administratörer och medadministratörer toobe e-post när hello avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="e572b-137">Check **Email owners...** if you want administrators and co-administrators toobe emailed when hello alert fires.</span></span>

7. <span data-ttu-id="e572b-138">Om du vill veta e-postmeddelanden tooreceive ett meddelande när hello varning utlöses, lägga till dem i hello **ytterligare administratören email(s)** fältet.</span><span class="sxs-lookup"><span data-stu-id="e572b-138">If you want additional emails tooreceive a notification when hello alert fires, add them in hello **Additional Administrator email(s)** field.</span></span> <span data-ttu-id="e572b-139">Avgränsa flera e-postmeddelanden med semikolon -  *email@contoso.com;email2@contoso.com*</span><span class="sxs-lookup"><span data-stu-id="e572b-139">Separate multiple emails with semi-colons - *email@contoso.com;email2@contoso.com*</span></span>

8. <span data-ttu-id="e572b-140">Placera i en giltig URI i hello **Webhook** fält om du vill anropa när hello avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="e572b-140">Put in a valid URI in hello **Webhook** field if you want it called when hello alert fires.</span></span>

9. <span data-ttu-id="e572b-141">Om du använder Azure Automation kan välja du en Runbook toobe körs när hello avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="e572b-141">If you use Azure Automation, you can select a Runbook toobe run when hello alert fires.</span></span>

10. <span data-ttu-id="e572b-142">Välj **OK** när klart toocreate hello avisering.</span><span class="sxs-lookup"><span data-stu-id="e572b-142">Select **OK** when done toocreate hello alert.</span></span>   

<span data-ttu-id="e572b-143">Inom några minuter hello aviseringen är aktiv och utlöser som beskrivits tidigare.</span><span class="sxs-lookup"><span data-stu-id="e572b-143">Within a few minutes, hello alert is active and triggers as previously described.</span></span>

## <a name="managing-your-alerts"></a><span data-ttu-id="e572b-144">Hantera aviseringar</span><span class="sxs-lookup"><span data-stu-id="e572b-144">Managing your alerts</span></span>
<span data-ttu-id="e572b-145">När du har skapat en avisering, kan du välja den och:</span><span class="sxs-lookup"><span data-stu-id="e572b-145">Once you have created an alert, you can select it and:</span></span>

* <span data-ttu-id="e572b-146">Visa ett diagram som visar hello mått tröskelvärde och hello faktiska värden från hello föregående dag.</span><span class="sxs-lookup"><span data-stu-id="e572b-146">View a graph showing hello metric threshold and hello actual values from hello previous day.</span></span>
* <span data-ttu-id="e572b-147">Redigera eller ta bort den.</span><span class="sxs-lookup"><span data-stu-id="e572b-147">Edit or delete it.</span></span>
* <span data-ttu-id="e572b-148">**Inaktivera** eller **aktivera** den om du vill stoppa tootemporarily eller återuppta tar emot meddelanden om den här aviseringen.</span><span class="sxs-lookup"><span data-stu-id="e572b-148">**Disable** or **Enable** it if you want tootemporarily stop or resume receiving notifications for that alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e572b-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e572b-149">Next steps</span></span>
* <span data-ttu-id="e572b-150">[Få en översikt över Azure övervakning](monitoring-overview.md) inklusive hello typer av information som du kan samla in och övervaka.</span><span class="sxs-lookup"><span data-stu-id="e572b-150">[Get an overview of Azure monitoring](monitoring-overview.md) including hello types of information you can collect and monitor.</span></span>
* <span data-ttu-id="e572b-151">Lär dig mer om [hur du konfigurerar webhooks i aviseringar](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="e572b-151">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="e572b-152">Lär dig mer om [konfigurera aviseringar på aktiviteten logghändelser](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="e572b-152">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="e572b-153">Lär dig mer om [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="e572b-153">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="e572b-154">Hämta en [översikt över diagnostikloggar](monitoring-overview-of-diagnostic-logs.md) och samla in detaljerade hög frekvens mått på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="e572b-154">Get an [overview of diagnostic logs](monitoring-overview-of-diagnostic-logs.md) and collect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="e572b-155">Hämta en [översikt över mått samling](insights-how-to-customize-monitoring.md) toomake att tjänsten är tillgänglig och svarstid.</span><span class="sxs-lookup"><span data-stu-id="e572b-155">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) toomake sure your service is available and responsive.</span></span>
