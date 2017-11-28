---
title: aaaCreate en anpassade instrumentpanel i Azure Log Analytics | Microsoft Docs
description: "Den här guiden hjälper dig att förstå hur logganalys instrumentpaneler kan visualisera alla dina sparad logg-sökningar, vilket ger dig en enda lins tooview din miljö."
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
ms.openlocfilehash: 73fcf131a91c743d473f37d5a40d52eaf78a7ba3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a><span data-ttu-id="13d2f-103">Skapa en anpassad instrumentpanel för användning i logganalys</span><span class="sxs-lookup"><span data-stu-id="13d2f-103">Create a custom dashboard for use in Log Analytics</span></span>

>[!NOTE]
> <span data-ttu-id="13d2f-104">Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), och du inte kan skapa nya instrumentpaneler eller redigera befintliga instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="13d2f-104">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you cannot create new dashboards or edit existing dashboards.</span></span> 

<span data-ttu-id="13d2f-105">Den här guiden hjälper dig att förstå hur logganalys instrumentpaneler kan visualisera alla dina sparad logg-sökningar, vilket ger dig en enda lins tooview din miljö.</span><span class="sxs-lookup"><span data-stu-id="13d2f-105">This guide helps you understand how Log Analytics dashboards can visualize all of your saved log searches, giving you a single lens tooview your environment.</span></span>

![Exempel instrumentpanelen](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

<span data-ttu-id="13d2f-107">Alla anpassade hello-instrumentpaneler som du skapar i hello OMS-portalen är också tillgängliga i hello OMS Mobilapp.</span><span class="sxs-lookup"><span data-stu-id="13d2f-107">All hello custom dashboards that you create in hello OMS portal are also available in hello OMS Mobile App.</span></span> <span data-ttu-id="13d2f-108">Se hello följande sidor för mer information om hello appar.</span><span class="sxs-lookup"><span data-stu-id="13d2f-108">See hello following pages for more information about hello apps.</span></span>

* [<span data-ttu-id="13d2f-109">OMS mobilappen från hello Microsoft Store</span><span class="sxs-lookup"><span data-stu-id="13d2f-109">OMS mobile app from hello Microsoft Store</span></span>](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [<span data-ttu-id="13d2f-110">OMS mobila appar från Apple iTunes</span><span class="sxs-lookup"><span data-stu-id="13d2f-110">OMS mobile app from Apple iTunes</span></span>](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobila instrumentpanelen](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a><span data-ttu-id="13d2f-112">Hur skapar jag min instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="13d2f-112">How do I create my dashboard?</span></span>
<span data-ttu-id="13d2f-113">toobegin gå toohello OMS översiktssidan.</span><span class="sxs-lookup"><span data-stu-id="13d2f-113">toobegin, go toohello OMS Overview page.</span></span> <span data-ttu-id="13d2f-114">Du ser hello **min instrumentpanel** panelen hello vänster.</span><span class="sxs-lookup"><span data-stu-id="13d2f-114">You'll see hello **My Dashboard** tile on hello left.</span></span> <span data-ttu-id="13d2f-115">Klicka på den toodrill ned i instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="13d2f-115">Click it toodrill down into your dashboard.</span></span>

![Översikt](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a><span data-ttu-id="13d2f-117">Lägga till en panel</span><span class="sxs-lookup"><span data-stu-id="13d2f-117">Adding a tile</span></span>
<span data-ttu-id="13d2f-118">I instrumentpaneler drivs paneler av sparad logg-sökningar.</span><span class="sxs-lookup"><span data-stu-id="13d2f-118">In dashboards, tiles are powered by your saved log searches.</span></span> <span data-ttu-id="13d2f-119">OMS levereras med många färdiga sparad logg sökningar, så att du kan börja direkt.</span><span class="sxs-lookup"><span data-stu-id="13d2f-119">OMS comes with many pre-made saved log searches, so you can begin right away.</span></span> <span data-ttu-id="13d2f-120">Använd hello följande steg dispositionen hur toobegin.</span><span class="sxs-lookup"><span data-stu-id="13d2f-120">Use hello following steps that outline how toobegin.</span></span>

<span data-ttu-id="13d2f-121">I hello Mina instrumentpanelsvy, klicka bara på **anpassa** tooenter anpassningsläge.</span><span class="sxs-lookup"><span data-stu-id="13d2f-121">In hello My Dashboard view, simply click **Customize** tooenter customize mode.</span></span>

![Illustrationer](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 <span data-ttu-id="13d2f-123">hello panel som öppnas hello höger hello sidan visas alla ditt arbetsområde sparad logg sökningar.</span><span class="sxs-lookup"><span data-stu-id="13d2f-123">hello panel that opens on hello right side of hello page shows all of your workspace's saved log searches.</span></span> <span data-ttu-id="13d2f-124">sparad logg sökning som en panel toovisualize hovra över en sparad sökning och klicka sedan på hello **plus** symbolen.</span><span class="sxs-lookup"><span data-stu-id="13d2f-124">toovisualize a saved log search as a tile,  hover over a saved search and then click hello **plus** symbol.</span></span>

![Lägg till paneler 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

<span data-ttu-id="13d2f-126">När du klickar på hello **plus** symboler, en ny panel visas i hello Mina instrumentpanelsvy.</span><span class="sxs-lookup"><span data-stu-id="13d2f-126">When you click hello **plus** symbol, a new tile appears in hello My Dashboard view.</span></span>

![Lägg till paneler 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a><span data-ttu-id="13d2f-128">Redigera en panel</span><span class="sxs-lookup"><span data-stu-id="13d2f-128">Edit a tile</span></span>
<span data-ttu-id="13d2f-129">I hello Mina instrumentpanelsvy, klicka bara på **anpassa** tooenter anpassningsläge.</span><span class="sxs-lookup"><span data-stu-id="13d2f-129">In hello My Dashboard view, simply click  **Customize** tooenter customize mode.</span></span> <span data-ttu-id="13d2f-130">Klicka på hello panelen du vill tooedit.</span><span class="sxs-lookup"><span data-stu-id="13d2f-130">Click hello tile you want tooedit.</span></span> <span data-ttu-id="13d2f-131">hello högra panelen ändrar tooedit och ger ett antal alternativ:</span><span class="sxs-lookup"><span data-stu-id="13d2f-131">hello right panel changes tooedit, and gives a selection of options:</span></span>

![Redigera sida vid sida](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Redigera sida vid sida](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a><span data-ttu-id="13d2f-134">Panelen visualiseringar</span><span class="sxs-lookup"><span data-stu-id="13d2f-134">Tile visualizations</span></span>
<span data-ttu-id="13d2f-135">Det finns tre typer av panelen visualiseringar toochoose från:</span><span class="sxs-lookup"><span data-stu-id="13d2f-135">There are three kinds of tile visualizations toochoose from:</span></span>

| <span data-ttu-id="13d2f-136">diagramtyp</span><span class="sxs-lookup"><span data-stu-id="13d2f-136">chart type</span></span> | <span data-ttu-id="13d2f-137">syfte</span><span class="sxs-lookup"><span data-stu-id="13d2f-137">what it does</span></span> |
| --- | --- |
| ![Liggande stapeldiagram](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |<span data-ttu-id="13d2f-139">Visar en tidslinje för din sparad logg sökresultat som ett stapeldiagram eller en lista med resultat av ett fält beroende på om sökningen loggen aggregerar resultaten efter ett fält eller inte.</span><span class="sxs-lookup"><span data-stu-id="13d2f-139">Displays a timeline of your saved log search's results as a bar chart, or a list of results by a field depending on if your log search aggregates results by a field or not.</span></span> |
| ![Mått](./media/log-analytics-dashboards/oms-dashboards-metric.png) |<span data-ttu-id="13d2f-141">Visar träffar din totala Sök resultatet som ett tal i en panel.</span><span class="sxs-lookup"><span data-stu-id="13d2f-141">Displays your total log search result hits as a number in a tile.</span></span> <span data-ttu-id="13d2f-142">Mått paneler kan du tooset ett tröskelvärde som ska markeras hello panelen när hello tröskelvärdet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="13d2f-142">Metric tiles allow you tooset a threshold that will highlight hello tile when hello threshold is reached.</span></span> |
| ![raden](./media/log-analytics-dashboards/oms-dashboards-line.png) |<span data-ttu-id="13d2f-144">Visar en tidslinje sparad logg Sök resultatet träffar med värden som ett linjediagram.</span><span class="sxs-lookup"><span data-stu-id="13d2f-144">Displays a timeline of your saved log search result hits with values as a line chart.</span></span> |

### <a name="threshold"></a><span data-ttu-id="13d2f-145">Tröskelvärde</span><span class="sxs-lookup"><span data-stu-id="13d2f-145">Threshold</span></span>
<span data-ttu-id="13d2f-146">Du kan skapa ett tröskelvärde på en panel med hello mått visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="13d2f-146">You can create a threshold on a tile using hello Metric visualization.</span></span> <span data-ttu-id="13d2f-147">Välj på toocreate ett tröskelvärde på hello sida vid sida.</span><span class="sxs-lookup"><span data-stu-id="13d2f-147">Select on toocreate a threshold value on hello tile.</span></span> <span data-ttu-id="13d2f-148">Välj om toohighlight hello panelen när hello värde ligger över eller under hello valt tröskelvärde sedan ange hello tröskelvärdet nedan.</span><span class="sxs-lookup"><span data-stu-id="13d2f-148">Choose whether toohighlight hello tile when hello value is over or under hello chosen threshold, then set hello threshold value below.</span></span>

## <a name="organizing-hello-dashboard"></a><span data-ttu-id="13d2f-149">Ordna hello instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="13d2f-149">Organizing hello dashboard</span></span>
<span data-ttu-id="13d2f-150">tooorganize instrumentpanelen, navigera toohello Mina instrumentpanelsvy och klicka på **anpassa** tooenter anpassningsläge.</span><span class="sxs-lookup"><span data-stu-id="13d2f-150">tooorganize your dashboard, navigate toohello My Dashboard view and click **Customize** tooenter customize mode.</span></span> <span data-ttu-id="13d2f-151">Klicka och dra hello panelen du vill toomove och flytta den toowhere som du vill att sida vid sida-toobe.</span><span class="sxs-lookup"><span data-stu-id="13d2f-151">Click and drag hello tile you want toomove, and move it toowhere you want your tile toobe.</span></span>

![Ordna din instrumentpanel](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a><span data-ttu-id="13d2f-153">Ta bort en panel</span><span class="sxs-lookup"><span data-stu-id="13d2f-153">Remove a tile</span></span>
<span data-ttu-id="13d2f-154">tooremove en panel navigera toohello Mina instrumentpanelsvy och klicka på **anpassa** tooenter anpassningsläge.</span><span class="sxs-lookup"><span data-stu-id="13d2f-154">tooremove a tile, navigate toohello My Dashboard view and click **Customize** tooenter customize mode.</span></span> <span data-ttu-id="13d2f-155">Välj hello panelen du vill tooremove på hello högra panelen och välj sedan **ta bort panelen**.</span><span class="sxs-lookup"><span data-stu-id="13d2f-155">Select hello tile you want tooremove, then on hello right panel select **Remove Tile**.</span></span>

![Ta bort en panel](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a><span data-ttu-id="13d2f-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="13d2f-157">Next steps</span></span>
* <span data-ttu-id="13d2f-158">Skapa [aviseringar](log-analytics-alerts.md) i logganalys toogenerate meddelanden och tooremediate problem.</span><span class="sxs-lookup"><span data-stu-id="13d2f-158">Create [alerts](log-analytics-alerts.md) in Log Analytics toogenerate notifications and tooremediate problems.</span></span>
