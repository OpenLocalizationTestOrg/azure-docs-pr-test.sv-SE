---
title: Skapa en anpassad instrumentpanel i Azure Log Analytics | Microsoft Docs
description: "Den här guiden hjälper dig att förstå hur logganalys instrumentpaneler kan visualisera alla dina sparad logg-sökningar, vilket ger dig en enda lins att visa din miljö."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a90d9c620221bffbb225fb060b997af2f5e90390
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a><span data-ttu-id="07174-103">Skapa en anpassad instrumentpanel för användning i logganalys</span><span class="sxs-lookup"><span data-stu-id="07174-103">Create a custom dashboard for use in Log Analytics</span></span>

>[!NOTE]
> <span data-ttu-id="07174-104">Om ditt arbetsområde har uppgraderats till den [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), och du inte kan skapa nya instrumentpaneler eller redigera befintliga instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="07174-104">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you cannot create new dashboards or edit existing dashboards.</span></span> 

<span data-ttu-id="07174-105">Den här guiden hjälper dig att förstå hur logganalys instrumentpaneler kan visualisera alla dina sparad logg-sökningar, vilket ger dig en enda lins att visa din miljö.</span><span class="sxs-lookup"><span data-stu-id="07174-105">This guide helps you understand how Log Analytics dashboards can visualize all of your saved log searches, giving you a single lens to view your environment.</span></span>

![Exempel instrumentpanelen](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

<span data-ttu-id="07174-107">Alla anpassade instrumentpaneler som du skapar i OMS-portalen är också tillgängliga i OMS-Mobilapp.</span><span class="sxs-lookup"><span data-stu-id="07174-107">All the custom dashboards that you create in the OMS portal are also available in the OMS Mobile App.</span></span> <span data-ttu-id="07174-108">Finns på följande sidor för mer information om apparna.</span><span class="sxs-lookup"><span data-stu-id="07174-108">See the following pages for more information about the apps.</span></span>

* [<span data-ttu-id="07174-109">OMS mobilappar från Microsoft-Store</span><span class="sxs-lookup"><span data-stu-id="07174-109">OMS mobile app from the Microsoft Store</span></span>](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [<span data-ttu-id="07174-110">OMS mobila appar från Apple iTunes</span><span class="sxs-lookup"><span data-stu-id="07174-110">OMS mobile app from Apple iTunes</span></span>](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobila instrumentpanelen](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a><span data-ttu-id="07174-112">Hur skapar jag min instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="07174-112">How do I create my dashboard?</span></span>
<span data-ttu-id="07174-113">Börja, gå till sidan Översikt över OMS.</span><span class="sxs-lookup"><span data-stu-id="07174-113">To begin, go to the OMS Overview page.</span></span> <span data-ttu-id="07174-114">Du ser den **min instrumentpanel** panelen till vänster.</span><span class="sxs-lookup"><span data-stu-id="07174-114">You'll see the **My Dashboard** tile on the left.</span></span> <span data-ttu-id="07174-115">Klicka på den detaljnivån i instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="07174-115">Click it to drill down into your dashboard.</span></span>

![Översikt](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a><span data-ttu-id="07174-117">Lägga till en panel</span><span class="sxs-lookup"><span data-stu-id="07174-117">Adding a tile</span></span>
<span data-ttu-id="07174-118">I instrumentpaneler drivs paneler av sparad logg-sökningar.</span><span class="sxs-lookup"><span data-stu-id="07174-118">In dashboards, tiles are powered by your saved log searches.</span></span> <span data-ttu-id="07174-119">OMS levereras med många färdiga sparad logg sökningar, så att du kan börja direkt.</span><span class="sxs-lookup"><span data-stu-id="07174-119">OMS comes with many pre-made saved log searches, so you can begin right away.</span></span> <span data-ttu-id="07174-120">Använd följande steg som beskriver hur du börjar.</span><span class="sxs-lookup"><span data-stu-id="07174-120">Use the following steps that outline how to begin.</span></span>

<span data-ttu-id="07174-121">I vyn instrumentpanel för Mina Klicka bara på **anpassa** ange anpassningsläge.</span><span class="sxs-lookup"><span data-stu-id="07174-121">In the My Dashboard view, simply click **Customize** to enter customize mode.</span></span>

![Illustrationer](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 <span data-ttu-id="07174-123">Panelen som öppnas till höger på sidan visas alla ditt arbetsområde sparad logg sökningar.</span><span class="sxs-lookup"><span data-stu-id="07174-123">The panel that opens on the right side of the page shows all of your workspace's saved log searches.</span></span> <span data-ttu-id="07174-124">Om du vill visualisera en sparad logg sökning som en panel hovra över en sparad sökning och klicka sedan på den **plus** symbolen.</span><span class="sxs-lookup"><span data-stu-id="07174-124">To visualize a saved log search as a tile,  hover over a saved search and then click the **plus** symbol.</span></span>

![Lägg till paneler 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

<span data-ttu-id="07174-126">När du klickar på den **plus** symboler, en ny panel visas i den här instrumentpanelsvyn.</span><span class="sxs-lookup"><span data-stu-id="07174-126">When you click the **plus** symbol, a new tile appears in the My Dashboard view.</span></span>

![Lägg till paneler 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a><span data-ttu-id="07174-128">Redigera en panel</span><span class="sxs-lookup"><span data-stu-id="07174-128">Edit a tile</span></span>
<span data-ttu-id="07174-129">I vyn instrumentpanel för Mina Klicka bara på **anpassa** ange anpassningsläge.</span><span class="sxs-lookup"><span data-stu-id="07174-129">In the My Dashboard view, simply click  **Customize** to enter customize mode.</span></span> <span data-ttu-id="07174-130">Klicka på panelen som du vill redigera.</span><span class="sxs-lookup"><span data-stu-id="07174-130">Click the tile you want to edit.</span></span> <span data-ttu-id="07174-131">De högra panelen ändringarna att redigera, och ger ett antal alternativ:</span><span class="sxs-lookup"><span data-stu-id="07174-131">The right panel changes to edit, and gives a selection of options:</span></span>

![Redigera sida vid sida](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Redigera sida vid sida](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a><span data-ttu-id="07174-134">Panelen visualiseringar</span><span class="sxs-lookup"><span data-stu-id="07174-134">Tile visualizations</span></span>
<span data-ttu-id="07174-135">Det finns tre typer av panelen visualiseringar kan välja mellan:</span><span class="sxs-lookup"><span data-stu-id="07174-135">There are three kinds of tile visualizations to choose from:</span></span>

| <span data-ttu-id="07174-136">diagramtyp</span><span class="sxs-lookup"><span data-stu-id="07174-136">chart type</span></span> | <span data-ttu-id="07174-137">syfte</span><span class="sxs-lookup"><span data-stu-id="07174-137">what it does</span></span> |
| --- | --- |
| ![Liggande stapeldiagram](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |<span data-ttu-id="07174-139">Visar en tidslinje för din sparad logg sökresultat som ett stapeldiagram eller en lista med resultat av ett fält beroende på om sökningen loggen aggregerar resultaten efter ett fält eller inte.</span><span class="sxs-lookup"><span data-stu-id="07174-139">Displays a timeline of your saved log search's results as a bar chart, or a list of results by a field depending on if your log search aggregates results by a field or not.</span></span> |
| ![Mått](./media/log-analytics-dashboards/oms-dashboards-metric.png) |<span data-ttu-id="07174-141">Visar träffar din totala Sök resultatet som ett tal i en panel.</span><span class="sxs-lookup"><span data-stu-id="07174-141">Displays your total log search result hits as a number in a tile.</span></span> <span data-ttu-id="07174-142">Mått paneler kan du ange ett tröskelvärde som ska markeras panelen när tröskelvärdet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="07174-142">Metric tiles allow you to set a threshold that will highlight the tile when the threshold is reached.</span></span> |
| ![raden](./media/log-analytics-dashboards/oms-dashboards-line.png) |<span data-ttu-id="07174-144">Visar en tidslinje sparad logg Sök resultatet träffar med värden som ett linjediagram.</span><span class="sxs-lookup"><span data-stu-id="07174-144">Displays a timeline of your saved log search result hits with values as a line chart.</span></span> |

### <a name="threshold"></a><span data-ttu-id="07174-145">Tröskelvärde</span><span class="sxs-lookup"><span data-stu-id="07174-145">Threshold</span></span>
<span data-ttu-id="07174-146">Du kan skapa ett tröskelvärde på en panel med mått visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="07174-146">You can create a threshold on a tile using the Metric visualization.</span></span> <span data-ttu-id="07174-147">Välj för att skapa ett tröskelvärde på panelen.</span><span class="sxs-lookup"><span data-stu-id="07174-147">Select on to create a threshold value on the tile.</span></span> <span data-ttu-id="07174-148">Välj om du vill markera panelen när värdet ligger över eller under det valda tröskelvärdet och ange sedan tröskelvärdet nedan.</span><span class="sxs-lookup"><span data-stu-id="07174-148">Choose whether to highlight the tile when the value is over or under the chosen threshold, then set the threshold value below.</span></span>

## <a name="organizing-the-dashboard"></a><span data-ttu-id="07174-149">Ordna instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="07174-149">Organizing the dashboard</span></span>
<span data-ttu-id="07174-150">För att organisera instrumentpanelen, navigera till den här instrumentpanelsvyn och klickar på **anpassa** ange anpassningsläge.</span><span class="sxs-lookup"><span data-stu-id="07174-150">To organize your dashboard, navigate to the My Dashboard view and click **Customize** to enter customize mode.</span></span> <span data-ttu-id="07174-151">Klicka och dra panelen du vill flytta och placera den där du vill att din panel ska vara.</span><span class="sxs-lookup"><span data-stu-id="07174-151">Click and drag the tile you want to move, and move it to where you want your tile to be.</span></span>

![Ordna din instrumentpanel](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a><span data-ttu-id="07174-153">Ta bort en panel</span><span class="sxs-lookup"><span data-stu-id="07174-153">Remove a tile</span></span>
<span data-ttu-id="07174-154">Om du vill ta bort en panel, navigera till den här instrumentpanelsvyn och klickar på **anpassa** ange anpassningsläge.</span><span class="sxs-lookup"><span data-stu-id="07174-154">To remove a tile, navigate to the My Dashboard view and click **Customize** to enter customize mode.</span></span> <span data-ttu-id="07174-155">Välj panelen du vill ta bort och klicka sedan på den högra panelen väljer **ta bort panelen**.</span><span class="sxs-lookup"><span data-stu-id="07174-155">Select the tile you want to remove, then on the right panel select **Remove Tile**.</span></span>

![Ta bort en panel](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a><span data-ttu-id="07174-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="07174-157">Next steps</span></span>
* <span data-ttu-id="07174-158">Skapa [aviseringar](log-analytics-alerts.md) i logganalys att generera meddelanden och åtgärda problem.</span><span class="sxs-lookup"><span data-stu-id="07174-158">Create [alerts](log-analytics-alerts.md) in Log Analytics to generate notifications and to remediate problems.</span></span>
