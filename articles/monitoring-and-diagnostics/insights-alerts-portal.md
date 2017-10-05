---
title: "Skapa aviseringar för Azure-tjänster - Azure-portalen | Microsoft Docs"
description: "Utlösaren e-postmeddelanden meddelanden, anropa webbplatser URL: er (webhooks) eller automation när angivna villkor uppfylls."
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
ms.openlocfilehash: 745a9c016bd037f1051025a2c5a468c3935e4550
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a><span data-ttu-id="4f4d6-103">Skapa mått aviseringar i Azure-Monitor för Azure-tjänster - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4f4d6-103">Create metric alerts in Azure Monitor for Azure services - Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4f4d6-104">Portal</span><span class="sxs-lookup"><span data-stu-id="4f4d6-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="4f4d6-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4f4d6-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="4f4d6-106">CLI</span><span class="sxs-lookup"><span data-stu-id="4f4d6-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="4f4d6-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="4f4d6-107">Overview</span></span>
<span data-ttu-id="4f4d6-108">Den här artikeln visar hur du ställer in Azure mått aviseringar med Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-108">This article shows you how to set up Azure metric alerts using the Azure portal.</span></span>   

<span data-ttu-id="4f4d6-109">Du kan ta emot en avisering baserat på övervakning mätvärden för eller händelser på Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-109">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="4f4d6-110">**Måttvärden** -aviseringen utlöses när värdet för ett visst mått överskrider ett tröskelvärde som du tilldelar i båda riktningarna.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-110">**Metric values** - The alert triggers when the value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="4f4d6-111">Det vill säga den utlöser både när villkoret uppfylls först och sedan efteråt när villkor som inte längre är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-111">That is, it triggers both when the condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="4f4d6-112">**Aktiviteten logghändelser** -utlösa en avisering på *varje* händelse eller bara när en vissa händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-112">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="4f4d6-113">Mer information om aktiviteten loggen aviseringar [Klicka här](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="4f4d6-113">To learn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="4f4d6-114">Du kan konfigurera en mått avisering när den utlöser gör du följande:</span><span class="sxs-lookup"><span data-stu-id="4f4d6-114">You can configure a metric alert to do the following when it triggers:</span></span>

* <span data-ttu-id="4f4d6-115">Skicka e-postmeddelanden till tjänstadministratören och medadministratörer</span><span class="sxs-lookup"><span data-stu-id="4f4d6-115">send email notifications to the service administrator and co-administrators</span></span>
* <span data-ttu-id="4f4d6-116">Skicka e-post till ytterligare e-postmeddelanden som du anger.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-116">send email to additional emails that you specify.</span></span>
* <span data-ttu-id="4f4d6-117">anropa en webhook</span><span class="sxs-lookup"><span data-stu-id="4f4d6-117">call a webhook</span></span>
* <span data-ttu-id="4f4d6-118">Starta körning av en Azure-runbook (endast från Azure portal)</span><span class="sxs-lookup"><span data-stu-id="4f4d6-118">start execution of an Azure runbook (only from the Azure portal)</span></span>

<span data-ttu-id="4f4d6-119">Du kan konfigurera och få information om mått Varningsregler med</span><span class="sxs-lookup"><span data-stu-id="4f4d6-119">You can configure and get information about metric alert rules using</span></span>

* [<span data-ttu-id="4f4d6-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4f4d6-120">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="4f4d6-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4f4d6-121">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="4f4d6-122">kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="4f4d6-122">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="4f4d6-123">Azure-Monitor REST API</span><span class="sxs-lookup"><span data-stu-id="4f4d6-123">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a><span data-ttu-id="4f4d6-124">Skapa en aviseringsregel på ett mått med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4f4d6-124">Create an alert rule on a metric with the Azure portal</span></span>
1. <span data-ttu-id="4f4d6-125">I den [portal](https://portal.azure.com/), leta upp den resurs som du är intresserad av övervakning och markera den.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-125">In the [portal](https://portal.azure.com/), locate the resource you are interested in monitoring and select it.</span></span>

2. <span data-ttu-id="4f4d6-126">Välj **aviseringar** eller **Varna regler** under avsnittet övervakning.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-126">Select **Alerts** or **Alert rules** under the MONITORING section.</span></span> <span data-ttu-id="4f4d6-127">Text och ikon kan variera något mellan olika resurser.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-127">The text and icon may vary slightly for different resources.</span></span>  

    ![Övervakning](./media/insights-alerts-portal/AlertRulesButton.png)

3. <span data-ttu-id="4f4d6-129">Välj den **Lägg till avisering** kommando och Fyll i fälten.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-129">Select the **Add alert** command and fill in the fields.</span></span>

    ![Lägg till avisering](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. <span data-ttu-id="4f4d6-131">**Namnet** aviseringen regel och väljer en **beskrivning**, som visar även i e-postmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-131">**Name** your alert rule, and choose a **Description**, which also shows in notification emails.</span></span>

5. <span data-ttu-id="4f4d6-132">Välj den **mått** du vill övervaka och väljer sedan en **villkoret** och **tröskelvärdet** värdet för måttet.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-132">Select the **Metric** you want to monitor, then choose a **Condition** and **Threshold** value for the metric.</span></span> <span data-ttu-id="4f4d6-133">Valde också den **Period** som mått regeln måste uppfyllas innan aviseringen utlösare.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-133">Also chose the **Period** of time that the metric rule must be satisfied before the alert triggers.</span></span> <span data-ttu-id="4f4d6-134">Om du använder period ”PT5M” och aviseringen söker efter CPU över 80 procent, startar exempelvis aviseringen när Processorn har konsekvent ovan 80% i 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-134">So for example, if you use the period "PT5M" and your alert looks for CPU above 80%, the alert triggers when the CPU has been consistently above 80% for 5 minutes.</span></span> <span data-ttu-id="4f4d6-135">När den första utlösaren infaller utlöses igen när Processorn är mindre än 80% i 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-135">Once the first trigger occurs, it again triggers when the CPU stays below 80% for 5 minutes.</span></span> <span data-ttu-id="4f4d6-136">CPU-mätning inträffar var 1 minut.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-136">The CPU measurement occurs every 1 minute.</span></span>   

6. <span data-ttu-id="4f4d6-137">Kontrollera **e-ägare...**  om du vill att administratörer och medadministratörer kan skickas när aviseringen utlöses.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-137">Check **Email owners...** if you want administrators and co-administrators to be emailed when the alert fires.</span></span>

7. <span data-ttu-id="4f4d6-138">Om du vill att ytterligare e-postmeddelanden ett meddelande när aviseringen utlöses, lägga till dem i den **ytterligare administratören email(s)** fältet.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-138">If you want additional emails to receive a notification when the alert fires, add them in the **Additional Administrator email(s)** field.</span></span> <span data-ttu-id="4f4d6-139">Avgränsa flera e-postmeddelanden med semikolon -  *email@contoso.com;email2@contoso.com*</span><span class="sxs-lookup"><span data-stu-id="4f4d6-139">Separate multiple emails with semi-colons - *email@contoso.com;email2@contoso.com*</span></span>

8. <span data-ttu-id="4f4d6-140">Placera i en giltig URI i den **Webhook** om du vill att den anropas när aviseringen utlöses.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-140">Put in a valid URI in the **Webhook** field if you want it called when the alert fires.</span></span>

9. <span data-ttu-id="4f4d6-141">Om du använder Azure Automation kan välja du en Runbook som ska köras när aviseringen utlöses.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-141">If you use Azure Automation, you can select a Runbook to be run when the alert fires.</span></span>

10. <span data-ttu-id="4f4d6-142">Välj **OK** när du är klar för att skapa aviseringen.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-142">Select **OK** when done to create the alert.</span></span>   

<span data-ttu-id="4f4d6-143">Inom några minuter aviseringen är aktiv och utlöser som beskrivits tidigare.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-143">Within a few minutes, the alert is active and triggers as previously described.</span></span>

## <a name="managing-your-alerts"></a><span data-ttu-id="4f4d6-144">Hantera aviseringar</span><span class="sxs-lookup"><span data-stu-id="4f4d6-144">Managing your alerts</span></span>
<span data-ttu-id="4f4d6-145">När du har skapat en avisering, kan du välja den och:</span><span class="sxs-lookup"><span data-stu-id="4f4d6-145">Once you have created an alert, you can select it and:</span></span>

* <span data-ttu-id="4f4d6-146">Visa ett diagram som visar mått tröskelvärdet och faktiska värden från föregående dag.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-146">View a graph showing the metric threshold and the actual values from the previous day.</span></span>
* <span data-ttu-id="4f4d6-147">Redigera eller ta bort den.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-147">Edit or delete it.</span></span>
* <span data-ttu-id="4f4d6-148">**Inaktivera** eller **aktivera** den om du vill att tillfälligt stoppa eller återuppta tar emot meddelanden om den här aviseringen.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-148">**Disable** or **Enable** it if you want to temporarily stop or resume receiving notifications for that alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f4d6-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4f4d6-149">Next steps</span></span>
* <span data-ttu-id="4f4d6-150">[Få en översikt över Azure övervakning](monitoring-overview.md) inklusive typerna av information som du kan samla in och övervaka.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-150">[Get an overview of Azure monitoring](monitoring-overview.md) including the types of information you can collect and monitor.</span></span>
* <span data-ttu-id="4f4d6-151">Lär dig mer om [hur du konfigurerar webhooks i aviseringar](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="4f4d6-151">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="4f4d6-152">Lär dig mer om [konfigurera aviseringar på aktiviteten logghändelser](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="4f4d6-152">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="4f4d6-153">Lär dig mer om [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="4f4d6-153">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="4f4d6-154">Hämta en [översikt över diagnostikloggar](monitoring-overview-of-diagnostic-logs.md) och samla in detaljerade hög frekvens mått på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-154">Get an [overview of diagnostic logs](monitoring-overview-of-diagnostic-logs.md) and collect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="4f4d6-155">Hämta en [översikt över mått samling](insights-how-to-customize-monitoring.md) att kontrollera att tjänsten är tillgänglig och svarstid.</span><span class="sxs-lookup"><span data-stu-id="4f4d6-155">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) to make sure your service is available and responsive.</span></span>
